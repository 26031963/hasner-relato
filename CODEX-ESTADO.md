# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 09:42** · publicado em **12/11/2026 09:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 09:42 · sessao 1644ac242a95 · PUSH-SEM-REGRAVAR · verificador · origem=Code

A pré-verificação encontra um defeito concreto em (a): **dois locais com o mesmo patch-id perdem uma das associações**, podendo deixar uma duplicata sujeita a reprovação. Em (b), há ampliação para prosa e nomes inteiros que entram no vocabulário, mas continuam impossíveis de usar em `padrao=` por conterem vírgulas. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

Não executei o MORDE completo, pois ele cria arquivos e repositórios temporários; os resultados históricos RED/GREEN e “4/4 mutantes” permanecem **[não verificado]**. (`bin/tests/test_lei_akita.sh:429`, `bin/tests/test_lei_akita.sh:432`, `bin/tests/test_lei_akita.sh:642`)

1. **Aval versus implementação, por item.**

   **(a)** O aval transcrito pede dispensar o commit cujo patch-id coincide com um publicado e emitir `DUPLICATA de <sha publicado>`; a transcrição não está disponível como arquivo local **[não verificado em arquivo]**. O código implementa essa dispensa antes de `julgar`, mas somente para commits com linha `LEI-AKITA:`, com até um pai e presentes no mapa local; merges continuam pelo hunk próprio. Portanto, não cobre literalmente todo commit duplicado, e a perda de associações no mapa impede reconhecer todas as duplicatas elegíveis. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`, `bin/tests/test_lei_akita.sh:385`)

   A saída humana fica `ja julgado: <local8> DUPLICATA de <publicado12>`, e a duplicata deixa de incrementar `JULGADOS`. (`bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:822`)

   **(b)** O aval transcrito pede o nome inteiro da primeira coluna, além dos pedaços **[não verificado em arquivo]**. O código acrescenta tanto a célula inteira quanto sua versão sem parênteses, preservando a extração anterior das duas primeiras colunas; não acrescenta a segunda coluna inteira. Entretanto, o parser do campo ainda impede os nomes com vírgulas. (`bin/tests/test_lei_akita.sh:98`, `bin/tests/test_lei_akita.sh:117`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

   Os anexos também divergem no testemunho de `7313a031c`: o aval cita `072556ec8`, enquanto a mensagem do commit cita `4aa0a00fb`. Se ambos são publicados equivalentes, **[não verificado]**; o código escolhe o primeiro publicado encontrado para cada patch-id. (`bin/tests/test_lei_akita.sh:385`)

2. **Quando conteúdo diferente pode receber o mesmo patch-id.**

   A tabela descreve a semântica do comando escolhido; sua reprodução na versão/configuração Git de produção é **[não verificado]**. A chamada efetiva usa `log -p --no-merges --no-renames` seguido de `patch-id --stable`. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:379`)

   | Caso | Consequência |
   |---|---|
   | Whitespace | É ignorado no cálculo: diferenças de indentação ou espaços dentro de strings podem preservar o patch-id e mudar comportamento. O bypass não faz comparação adicional de árvore, blob ou AST. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:401`) |
   | Base/contexto | Patch-id identifica o patch normalizado, não a árvore resultante nem sua equivalência semântica em outra base; o código aceita somente essa identidade. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:401`) |
   | Modo de arquivo | Cabeçalhos de mudança de modo participam do patch; “só modo” pode produzir patch-id e ser duplicata. Não é automaticamente vazio. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:401`) |
   | Binário | Não se pode afirmar que todo binário recebe o mesmo ID: a representação binária incorpora identificação dos blobs. Não há prova dessa borda no MORDE novo. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:778`; comportamento em produção **[não verificado]**) |
   | Rename | `--no-renames` apresenta remoção e adição, incluindo caminhos e conteúdo; o bypass não se baseia apenas no rótulo “rename”. (`bin/tests/test_lei_akita.sh:378`) |
   | Commit vazio | Não produz registro de patch-id; portanto, não recebe um “ID vazio comum” que o torne duplicata. Sem associação local, segue o julgamento normal se tiver linha. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:401`) |

   **Há possibilidade de dispensar código de negócio novo sem julgá-lo:** por exemplo, patches que diferem apenas entre `return 'a b'` e `return 'ab'`. Isso decorre do critério de equivalência escolhido, além do defeito do mapa; sua ocorrência nos commits fornecidos é **[não verificado]**. (`bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:403`)

   Um vazio com linha e termo inexistente continua vermelho; vazio com `n/a (instrumento)` pode passar por não tocar negócio. Um commit só de modo, sem correspondência publicada, continua sujeito ao julgamento dos caminhos alterados e pode reprovar `n/a` em `app/`. (`bin/tests/test_lei_akita.sh:323`, `bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:415`, `bin/tests/test_lei_akita.sh:418`)

3. **Publicados, erros e `split()`.** [nao verificado]

   Os publicados são os commits não merge alcançáveis de `origin/main`, excluída a ancestralidade do pai do corte; é um limite de grafo, não uma data. Não há restrição a primeiro pai nem checagem de que o publicado teve efetivamente julgamento anterior. (`bin/tests/test_lei_akita.sh:371`, `bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:394`)

   `origin/main` ausente provoca erro estrito na enumeração; repositório raso é recusado explicitamente; retorno não zero de `patch-id` produz `PADRAO_ERRO`. Esses caminhos não viram conjunto vazio silencioso. (`bin/tests/test_lei_akita.sh:359`, `bin/tests/test_lei_akita.sh:363`, `bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:388`)

   **Há conjuntos vazios legítimos:** nenhum local enumerado; nenhum publicado restante após o piso; ou nenhum patch emitido, como numa seleção só de vazios. O código também aceita stdout vazio com retorno zero sem validar sua completude. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:395`)

   Na saída normal, cada registro tem dois campos; commit vazio simplesmente não gera registro. Mas `pid, sha = ln.split()` lança `ValueError` diante de linha com quantidade diferente de campos, sem convertê-lo em `PADRAO_ERRO`; dois campos inválidos também não são validados. A ocorrência dessa saída anormal é **[não verificado]**. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:384`)

   Exceções de execução ou decodificação dos subprocessos também não têm tratamento geral. Ainda assim, uma interrupção antes do fim deixa faltar `PADRAO_FIM`, e o shell reprova: diagnóstico fora do padrão, mas não verde por esse caminho. (`bin/tests/test_lei_akita.sh:356`, `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:823`)

4. **Locais, merges, raízes e mapa.**

   `--no-walk=unsorted` seleciona os SHAs fornecidos sem percorrer seus ancestrais; `--no-merges` exclui merges do cálculo. Os merges permanecem em `hs` e seguem o julgamento de hunks próprios. (`bin/tests/test_lei_akita.sh:378`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:405`)

   Raízes são elegíveis porque a condição é `len(pais) <= 1`. Na configuração usual, `log -p` mostra sua criação contra a árvore vazia; a configuração efetiva, inclusive `log.showRoot`, é **[não verificado]**. O julgamento posterior possui tratamento explícito de raiz. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:416`)

   **O mapa fica errado para dois locais com o mesmo patch-id:** `setdefault(pid, sha)` conserva apenas o primeiro SHA; inverter esse dicionário não recupera o outro. Um local omitido será julgado mesmo sendo duplicata publicada. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:401`)

5. **Termos novos do GLOSSÁRIO real.**

   São **dez entradas novas no conjunto**, provenientes das nove células abaixo; somente sete são utilizáveis pelo parser atual de `padrao=`. A normalização remove marcação, converte para minúsculas e compacta espaços. (`bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`)

   | Célula que ganha aceite integral | Uso no campo |
   |---|---|
   | `Single Source of Truth / Aggregate Root` | Aceito. (`app/docs/GLOSSARIO.md:11`) |
   | `sinal derivado do estado, reconciliacao level-triggered (o oposto, edge-triggered, e o fio morto)` | Entra inteiro **e sem o parêntese**, mas ambas as entradas contêm vírgula e são recusadas pelo parser. (`app/docs/GLOSSARIO.md:13`; `bin/tests/test_lei_akita.sh:123`, `bin/tests/test_lei_akita.sh:317`) |
   | `Architecture Conformance Test (armadilha: selar a FORMA e nao o SIGNIFICADO)` | Aceito, incluindo a prosa. (`app/docs/GLOSSARIO.md:15`; `bin/tests/test_lei_akita.sh:331`) |
   | `Parallel Change / Expand-Contract` | Aceito. (`app/docs/GLOSSARIO.md:18`) |
   | `Shadow mode / Parallel run` | Aceito. (`app/docs/GLOSSARIO.md:21`) |
   | `Backfill / Reprocessing` | Aceito. (`app/docs/GLOSSARIO.md:23`) |
   | `Duplicated business logic / logic drift` | Aceito. (`app/docs/GLOSSARIO.md:25`) |
   | `ADR (Architecture Decision Record)` | Aceito; o inteiro é novo, embora seus componentes já fossem extraídos. (`app/docs/GLOSSARIO.md:28`; `bin/tests/test_lei_akita.sh:114`, `bin/tests/test_lei_akita.sh:123`) |
   | `Offline Fallback (network-first: sem rede, a copia do cache; sem copia, o erro de rede de sempre -- nunca undefined no respondWith)` | Entra no conjunto após normalização, mas suas vírgulas impedem uso no campo. (`app/docs/GLOSSARIO.md:31`; `bin/tests/test_lei_akita.sh:83`, `bin/tests/test_lei_akita.sh:317`) |

   Portanto, **sim, prosa passa a ser aceita integralmente** no caso de `Architecture Conformance Test`. O filtro antigo para parênteses que parecem nomes não é aplicado pelo loop novo à célula inteira. (`app/docs/GLOSSARIO.md:15`; `bin/tests/test_lei_akita.sh:88`, `bin/tests/test_lei_akita.sh:123`)

6. **Quem escreve e quem lê.**

   | Informação | Escrita e leitura |
   |---|---|
   | `DUPLICATAS` | Zerada por chamada, preenchida com local8/publicado12; lida pelo MORDE novo e pela emissão normal. (`bin/tests/test_lei_akita.sh:374`, `bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:794`, `bin/tests/test_lei_akita.sh:814`) |
   | `JULGADOS` | Inicializada na enumeração e incrementada depois da exclusão de duplicatas; emitida em `PADRAO_JULGADOS`. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:816`) |
   | `ENUMERADOS` | Total de `hs`, incluindo sem linha e duplicatas; lida na saída e nas asserções de sincronização, outra ref e piso. (`bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:676`, `bin/tests/test_lei_akita.sh:701`, `bin/tests/test_lei_akita.sh:802`, `bin/tests/test_lei_akita.sh:816`) |
   | `PADRAO_FALHA` | Python emite; shell conta e transforma em `FALHA: PADRAO-NA-CURA`. (`bin/tests/test_lei_akita.sh:813`, `bin/tests/test_lei_akita.sh:820`, `bin/tests/test_lei_akita.sh:821`) |
   | `PADRAO_DUPLICATA` | Python emite; shell transforma em `ja julgado:`. (`bin/tests/test_lei_akita.sh:815`, `bin/tests/test_lei_akita.sh:822`) |
   | `PADRAO_JULGADOS` | Consumida pelo próprio selo para compor a frase final de sucesso. (`bin/tests/test_lei_akita.sh:816`, `bin/tests/test_lei_akita.sh:831`) |
   | `PADRAO_FIM` / `PADRAO_ERRO` | O sentinela é exigido; não há parser dedicado de `PADRAO_ERRO`, cuja interrupção provoca ausência do sentinela. (`bin/tests/test_lei_akita.sh:819`, `bin/tests/test_lei_akita.sh:823`) |

   `pre-push.sh` passa o SHA e decide pelo código de saída; `regua.sh` também decide pelo código de saída e apenas exibe a saída quando há falha. A adição da linha de duplicata não quebra esses leitores. (`bin/pre-push.sh:38`; `bin/regua.sh:140`, `bin/regua.sh:142`)

   Outro selo que grepe essa saída não foi encontrado na varredura da cópia; leitor externo, ignorado ou ausente da cópia é **[não verificado]**.

7. **MORDE e mutantes.**

   O caso sintético de vocabulário morde retirar o nome inteiro e acrescentar também o inteiro da segunda coluna; os dois casos de `julgar` mordem negar `Parallel Change / Expand-Contract` e aceitar uma composição inventada só porque contém um pedaço válido. (`bin/tests/test_lei_akita.sh:444`, `bin/tests/test_lei_akita.sh:449`, `bin/tests/test_lei_akita.sh:450`, `bin/tests/test_lei_akita.sh:471`)

   O caso de duplicata morde retirar a dispensa, dispensar indiscriminadamente o outro patch ou emitir associação publicada errada naquele cenário. Contudo, o patch diferente está em **outro arquivo**, e existe apenas um local com o patch-id publicado. (`bin/tests/test_lei_akita.sh:783`, `bin/tests/test_lei_akita.sh:789`, `bin/tests/test_lei_akita.sh:791`, `bin/tests/test_lei_akita.sh:795`)

   Permanecem lacunas concretas: dois locais com mesmo patch-id — inclusive o defeito atual —; mudança diferente no mesmo caminho; whitespace semântico; vazio/modo/binário/raiz; erro ou saída malformada de `patch-id`; prosa e vírgulas nas células. Esses casos não são plantados pela prova nova. (`bin/tests/test_lei_akita.sh:433`, `bin/tests/test_lei_akita.sh:778`)

   Também sobreviveria à asserção nova um mutante que continuasse contando duplicatas em `JULGADOS`, porque ela verifica somente falhas e associação da duplicata. Um mutante que devolvesse `{}` apenas quando `patch-id` falha não seria exercitado pelo cenário saudável; sua execução efetiva é **[não verificado]**. (`bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:795`)

8. **As três classes de reprovação citadas.**

   **Selo/RED sem mordida:** as asserções novas têm ligação com os comportamentos pretendidos, mas não provam todas as bordas; o defeito de múltiplos locais passa pela prova nova. O RED histórico alegado continua **[não verificado]**. (`bin/tests/test_lei_akita.sh:444`, `bin/tests/test_lei_akita.sh:795`, `bin/tests/test_lei_akita.sh:385`)

   **Erro operacional virando verde:** não encontrei esse caminho nas falhas explícitas examinadas; ausência de `PADRAO_FIM` reprova, e `MORDE_OK` sozinho não basta quando existe `MORDE_FALHOU`. Há, porém, exceções fora de `PADRAO_ERRO` e stdout vazio com sucesso sem validação adicional. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:823`, `bin/tests/test_lei_akita.sh:827`)

   **Campo com dois significados:** o formato `N de M` permanece, mas `JULGADOS` agora exclui duplicatas com linha. Nenhum dos dois leitores operacionais identificados depende da contagem; o próprio texto final continua dizendo “commit(s) com linha LEI-AKITA”, sem explicar a exclusão, e o MORDE novo não protege essa semântica. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:795`, `bin/tests/test_lei_akita.sh:831`; `bin/pre-push.sh:38`; `bin/regua.sh:140`)

## 12/11 09:42 · sessao 42642e1bff0d · - · - · origem=Ronald (tmux)

6–9 são **R**, com origem no c10 `3fc096198`: ele retirou `grade_espelho_janela` do PDF e substituiu suas órfãs por `orfas_grade` da tela ([pdf_espelho.py:471](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/relatorios/pdf_espelho.py:471)). A tela prioriza `grade_da_celula`, que pode responder sem célula e sem órfãs ([espelho.py:522](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/services/espelho.py:522)); isso explica a marca perdida e o aberto contado.

Cura mínima sugerida: restaurar a leitura real do contrato no PDF e passar `recusas=None` opcional pelo caminho:

- `grade_espelho_janela` → dispatcher → builder por turno → `_marcar_ausencias` ([utils.py:2338](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:2338), [utils.py:2099](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:2099), [utils.py:1292](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1292)).
- Preservar também os ramos legacy: chamada direta sem vínculo e `montar_grade_prevista` ([utils.py:1008](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1008), [utils.py:1016](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1016)).
- `_marcar_ausencias` entrega o destinatário à A2 e, antes de escolher faixa, transporta `erro.rotulo` no dia recusado; conserva células/órfãs e não conclui precedência ([utils.py:1209](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/escala/utils.py:1209)). Sem destinatário, a A2 continua levantando ([precedencia.py:299](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/precedencia.py:299)).

Há dois testes novos que selam a regressão e precisam caracterização curada: C10 proíbe explicitamente importar/chamar o contrato ([test_lastro_c10_sem_banco.py:324](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/tests/test_lastro_c10_sem_banco.py:324)); C11 exige órfã falsa/aberto 1 quando a tela recebe `sem_celula` ([test_lastro_c11_sem_banco.py:360](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/ponto/tests/test_lastro_c11_sem_banco.py:360)). Nenhum arquivo editado ou teste executado por este explorer. [nao verificado]

## 12/11 09:42 · sessao 6c6e0198f7a5 · FIO-E0-V4-E3-LEITORES · leitura · origem=Code

ACHADO 1 | CODIGO: confere em parte -- antes de descarimbar, `registrar(linhas_da_projecao_sem_linha(...))` persiste RESPOSTA e VALIDACAO históricas faltantes, identificadas por pergunta/ato/instante, conservando timestamps, texto e autor da validação (`app/chamados/models.py:1564`, `app/chamados/models.py:3932`, `app/chamados/models.py:3938`, `app/chamados/models.py:3954`). Depois limpa os campos sem acrescentar REABERTURA à sequência (`app/chamados/models.py:1567`); reabre a disputa e registra auditoria separada (`app/chamados/models.py:1576`, `app/chamados/models.py:1590`). Se VALIDACAO era o último ato e não existe posterior, **VALIDACAO continua por último**, pela ordenação ato_em/pk; isso não vale universalmente para toda ressuscitação (`app/chamados/models.py:3890`). | CENSO: NOVO; “0 escritores persistentes” trata do estado da tarefa, não dos seus insumos (`e3_censo_anexo.r:39`). | EFEITO: sim, condicional no código: a tarefa continua no ramo VALIDACAO, enquanto C6 pode passar a viva após a limpeza; as demais guardas continuam valendo (`app/chamados/services/tarefa_do_dia.py:141`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1466`). Ocorrência em produção [nao verificado].

ACHADO 2 | CODIGO: confere em parte -- o campo aceita NULL e o filtro por intervalo exclui essas perguntas da carga (`app/chamados/models.py:2424`, `app/chamados/services/tarefa_do_dia.py:374`). C6 pode considerá-las vivas, mas NULL não basta: validação, tranca, vias e célula têm precedência (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1438`, `app/chamados/juizes.py:1455`, `app/chamados/juizes.py:1466`). A abertura atual recusa data nula (`app/chamados/models.py:2788`). | CENSO: NOVO; a decisão 8 cobre conversas, não fatos sem data (`e3_censo_anexo.r:125`). | EFEITO: sim, condicional: C17 pode considerar a pergunta pela célula enquanto a tarefa não a carrega (`app/chamados/juizes.py:1571`, `app/chamados/juizes.py:1651`, `app/chamados/services/tarefa_do_dia.py:376`). Existência desse estado e desaparecimento após migração [nao verificado].

ACHADO 3 | CODIGO: confere em parte -- C17 agrupa pela célula; a tarefa, por colaborador/data_evento (`app/chamados/juizes.py:1574`, `app/chamados/services/tarefa_do_dia.py:380`). Porém `rechavear_data_evento` atualiza conjuntamente data e célula; o caminho citado não demonstra que o noturno produza desencontro (`app/chamados/services/fio.py:1035`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1044`). | CENSO: NOVO; não é um dos quatro conflitos enumerados (`e3_censo_anexo.r:113`). | EFEITO: sim, se houver desencontro e forem atendidas as demais condições: a pergunta participa de dias diferentes nos dois universos (`app/chamados/services/tarefa_do_dia.py:236`, `app/chamados/juizes.py:1649`). Incidência e invariante na frota [nao verificado].

ACHADO 4 | CODIGO: confere em parte -- `alimentar_portas` passa agora à fila, mas chama C6 sem instante; `fio_vivo` também, e a janela usa `date.today()` (`app/chamados/juizes.py:1566`, `app/chamados/juizes.py:1573`, `app/chamados/juizes.py:1503`, `app/chamados/juizes.py:592`). O DIFF avisa expressamente esse limite (`app/chamados/management/commands/diff_e3_sombra.py:105`). A alegação absoluta de irreprodutibilidade desconsidera que a sonda fornecida envolve a comparação em `freeze_time(agora)`; execução dessa sonda [nao verificado] (`e3_censo_anexo.r:392`). | CENSO: NOVO quanto ao defeito de propagação do relógio; o congelamento já consta da sonda (`e3_censo_anexo.r:392`). | EFEITO: sim, potencialmente na vitalidade e nas portas; a janela participa da decisão, enquanto a carga da tarefa fornece hoje/agora (`app/chamados/juizes.py:1455`, `app/chamados/juizes.py:1473`, `app/chamados/services/tarefa_do_dia.py:489`). Divergência efetiva na virada [nao verificado].

ACHADO 5 | CODIGO: confere em parte -- importa os privados e **exclui** dias com fio diferente de SEM_PORTA; os restantes viram candidatos à Pauta (`app/ponto/services/pautas_dos_discordantes.py:61`, `app/ponto/services/pautas_dos_discordantes.py:77`, `app/ponto/services/pautas_dos_discordantes.py:79`). A quebra ou permanência no juiz antigo não é necessária: `_fio_do_dia` chama `_porta`, cuja implementação pode mudar preservando o contrato (`app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:67`). | CENSO: NOVO consumidor; o wrapper já estava listado (`e3_censo_anexo.r:64`). | EFEITO: nao sobre tarefa/C6/C17; esse caminho consome a resposta e escreve Pautas quando aplicado (`app/ponto/services/pautas_dos_discordantes.py:75`, `app/ponto/services/pautas_dos_discordantes.py:177`, `app/ponto/services/pautas_dos_discordantes.py:183`).

ACHADO 6 | CODIGO: confere -- `do_dia` seleciona órfãos com causa `fabrica_nao_rodou`; a porta chama a fábrica com notificação e relê o resultado (`app/chamados/services/cobrar_dia.py:45`, `app/chamados/services/cobrar_dia.py:46`, `app/chamados/services/cobrar_dia.py:49`, `app/chamados/services/cobrar_dia.py:53`). Mensagem/ok usam perguntas e chamados retornados; `do_dia` mantém sua seleção e retenção (`app/chamados/services/cobrar_dia.py:55`, `app/chamados/services/cobrar_dia.py:58`, `app/chamados/services/pergunta_no_app.py:217`, `app/chamados/services/pergunta_no_app.py:223`). | CENSO: NOVO chamador com escrita; a referência anterior cobre o copiloto (`e3_censo_anexo.r:68`). | EFEITO: sim, potencialmente: a fábrica abre perguntas persistidas que entram nas cargas da tarefa/C17 (`app/chamados/services/disputa_emissao.py:837`, `app/chamados/models.py:3018`, `app/chamados/services/tarefa_do_dia.py:374`, `app/chamados/juizes.py:1571`). Ocorrência em produção [nao verificado].

ACHADO 7 | CODIGO: confere -- `_material` prioriza texto não vazio, depois consulta `fio_vivo`, senão retorna “nada” (`app/folha/previa.py:142`, `app/folha/previa.py:144`, `app/folha/previa.py:146`); C15 delega a C6 sem instante explícito (`app/chamados/juizes.py:1503`). | CENSO: NOVO consumidor; a descrição geral de C15 não enumera esse leitor (`e3_censo_anexo.r:35`). | EFEITO: nao sobre as respostas dos três juízes; o caminho recebe perguntas e produz o rótulo material da prévia (`app/folha/previa.py:95`, `app/folha/previa.py:139`).

ACHADO 8 | CODIGO: confere -- o vigia considera canal pela existência de pergunta sem respondida_em ou chamado em VIVOS, sem filtro de silêncio (`app/chamados/services/furo_sem_canal.py:80`, `app/chamados/services/furo_sem_canal.py:82`, `app/chamados/services/furo_sem_canal.py:85`). VIVOS inclui registrado; COBRANCA não (`app/chamados/catalogo/motor.py:24`, `app/chamados/catalogo/motor.py:29`). | CENSO: NOVO contador; o vigia enumerado é outro serviço (`e3_censo_anexo.r:66`). | EFEITO: nao sobre tarefa/C6/C17; altera a medição de “sem canal”, retornando contagens e amostra (`app/chamados/services/furo_sem_canal.py:89`). Divergência concreta em algum dia de produção [nao verificado].

ACHADO 9 | CODIGO: confere -- a fila exclui silêncio futuro; a lista de trabalho exige NULL, portanto também exclui silêncio vencido (`app/chamados/services/painel.py:190`, `app/chamados/services/janela_colab.py:73`). `q_vivas` amplia a janela para chamados com perguntas; não é o único critério de visibilidade (`app/chamados/services/janela_colab.py:63`, `app/chamados/services/janela_colab.py:69`, `app/chamados/services/janela_colab.py:74`). | CENSO: NOVO quanto à assimetria; a fonte compartilhada do app listada não cobre esse filtro (`e3_censo_anexo.r:62`). | EFEITO: sim, condicional: vencido o silêncio, a âncora pode voltar à fila e fornecer porta à tarefa/C17, embora continue fora dessa lista (`app/chamados/services/tarefa_do_dia.py:415`, `app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/juizes.py:1651`). Ocorrência concreta [nao verificado].

ACHADO 10 | CODIGO: confere -- somente aberto/em_analise sem silêncio vigente entram na fila; `_pela_decisao` usa porta None para âncora fora dela (`app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:189`, `app/chamados/services/painel.py:190`, `app/chamados/services/tarefa_do_dia.py:127`). A âncora retorna destino ou chamado da disputa, sem seguir uma cadeia de superação (`app/chamados/services/fio.py:117`). | CENSO: REPETIDO -- a decisão 4 já exige preservar ações humanas sem porta (`e3_censo_anexo.r:117`). | EFEITO: sim na porta publicada, sem necessariamente eliminar a ação exigível; C17 também exige âncora na fila (`app/chamados/services/tarefa_do_dia.py:131`, `app/chamados/services/tarefa_do_dia.py:546`, `app/chamados/juizes.py:1651`). Incidência desses estados [nao verificado].

ACHADO 11 | CODIGO: confere em parte -- os receivers acionam beco após commit e outbox; o beco consulta C7 (`app/chamados/signals.py:240`, `app/chamados/signals.py:257`, `app/chamados/services/becos.py:33`). `lastro.py:239` pertence à leitura `julgar`; o fechador real também consulta fila sem agora e tenta reconciliar (`app/ponto/services/lastro.py:196`, `app/ponto/services/lastro.py:239`, `app/ponto/services/lastro.py:388`, `app/ponto/services/lastro.py:401`). Ausência de post_save no lote não implica ausência de evento: a ressuscitação acorda células explicitamente (`app/chamados/models.py:1569`). | CENSO: NOVO mecanismo transitivo; a cobertura completa desses efeitos já estava declarada não verificada (`e3_censo_anexo.r:25`). | EFEITO: sim, se os gates aprovarem a mudança de status: ela altera a fila e as portas da tarefa/C17 (`app/chamados/models.py:652`, `app/chamados/services/painel.py:189`, `app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/juizes.py:1651`). Ocorrência em produção [nao verificado].

## 12/11 09:39 · sessao 1ff43799f416 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

Entrega parcial no [relatório, com tabela E/N, LEI-AKITA, leitores/escritores e comandos](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-c1.txt:1).

- **73 testes sem banco passaram; 24 mutantes ficaram vermelhos.** [Resultados](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-c1.txt:116) [nao verificado]
- **O cron mantém um RED:** ainda reescreve futuras. [Teste](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/tests/test_cadastro_futuro_sem_banco.py:742) [nao verificado]
- Suíte Django: **[nao verificado]**, dependência ausente. Commit não realizado pela proibição inicial. [Registro](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/docs/BUG-CADASTRO-NAO-ALCANCA-O-FUTURO-c1.txt:195) [nao verificado]

Para concluir o cron, é necessária exceção explícita à proibição de editar [celula.py:870](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:870), onde está o ramo exigido pelo próprio brief.

## 12/11 09:39 · sessao 872bd20c1a5c · - · - · origem=Ronald (tmux)

ACHADO 1: **confere em parte**. `ressuscitar_perguntas` realmente limpa `validada_em`, `validada_por`, `respondida_em`, além dos três campos do carimbo, sem registrar um novo ato de ressuscitação/reabertura (`app/chamados/models.py:1564`, `app/chamados/models.py:1567`, `app/chamados/models.py:3265`). Porém não apaga a projeção sem antes preservar os atos históricos: `linhas_da_projecao_sem_linha` produz **RESPOSTA** e **VALIDACAO** ausentes por correspondência `(pergunta, ato, instante)`, usando os instantes originais, texto e autor da validação (`app/chamados/models.py:3925`, `app/chamados/models.py:3932`, `app/chamados/models.py:3938`); `registrar` persiste essas linhas e acorda células (`app/chamados/models.py:3953`). Depois não acrescenta REABERTURA; reabre apenas a disputa e registra auditoria própria (`app/chamados/models.py:1575`, `app/chamados/models.py:1590`, `app/chamados/models.py:2174`). A sequência é ordenada por `ato_em, pk`; **no caso descrito, em que VALIDACAO era o último ato e não há posterior, VALIDACAO permanece por último** (`app/chamados/models.py:3890`, `app/chamados/services/tarefa_do_dia.py:390`). Não é correto universalizar que toda ressuscitação termina com VALIDACAO: pode não haver projeção assinada ou existir ato posterior (`app/chamados/models.py:3925`, `app/chamados/services/tarefa_do_dia.py:390`). EFEITO: **sim, condicional demonstrado pelo código**: a tarefa continua interpretando o último ato como validação (`app/chamados/services/tarefa_do_dia.py:141`), enquanto C6 lê os campos atuais e pode passar a viva após limpeza, observadas suas demais guardas (`app/chamados/juizes.py:1375`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1466`). No caso de validação, tarefa pode responder desconhecido, materializar, rejulgar ou encerrada, conforme lavra/efeitos; não há uma única resposta garantida (`app/chamados/services/tarefa_do_dia.py:142`). O contraste com `reabrir_para_correcao` procede: ela registra novo ato atual com foto antes do update (`app/chamados/services/disputa_emissao.py:1615`, `app/chamados/services/disputa_emissao.py:1630`). Os três chamadores citados foram abertos e conferem (`app/chamados/models.py:1452`, `app/ponto/services/cartorio.py:1673`, `app/chamados/views_arquivar.py:86`). Incidência concreta em produção: [nao verificado].

ACHADO 11: **confere em parte**. Os dois receivers existem: o de beco agenda `fechar_beco_da_disputa` em `on_commit`; o de outbox ignora criação/raw e acorda células quando os campos relevantes são salvos (`app/chamados/signals.py:229`, `app/chamados/signals.py:240`, `app/chamados/signals.py:252`, `app/chamados/signals.py:257`). O beco tem recorte próprio, mas delega fechamento a C7, resolve perguntas e fecha a disputa pela porta; o signal da disputa pode então reconciliar o pai (`app/chamados/services/becos.py:25`, `app/chamados/services/becos.py:33`, `app/chamados/services/becos.py:54`, `app/chamados/services/becos.py:57`, `app/chamados/signals.py:43`). O consumidor realmente chama `lastro.fechar_pelo_marco_colab`, que filtra `qs_na_fila` sem `agora`, verifica marco aceso e tenta `reconciliar`; esta grava o novo estado só se seus gates aprovam (`app/chamados/reconciliador.py:1383`, `app/ponto/services/lastro.py:388`, `app/ponto/services/lastro.py:395`, `app/ponto/services/lastro.py:401`, `app/chamados/models.py:652`). A linha `lastro.py:239` citada pertence a `julgar`, leitura usada na medição, não ao fechador; o mesmo default temporal aparece no fechador real (`app/ponto/services/lastro.py:196`, `app/ponto/services/lastro.py:239`, `app/ponto/services/lastro.py:388`). **Lote não disparar `post_save` não significa perder outbox neste caminho:** a ressuscitação emite explicitamente após o lote; `registrar` também emite e `reabrir_para_correcao` emite explicitamente (`app/chamados/models.py:1566`, `app/chamados/models.py:1569`, `app/chamados/models.py:3955`, `app/chamados/services/disputa_emissao.py:1633`). EFEITO: **sim, se o gate fecha**: mudança de status altera a fila e, portanto, a porta que tarefa/C17 publicam (`app/chamados/services/painel.py:189`, `app/chamados/services/tarefa_do_dia.py:126`, `app/chamados/juizes.py:1647`, `app/chamados/juizes.py:1653`). A alegação de que sejam necessariamente regras concorrentes está superdimensionada: C7 e `reconciliar` são autoridades compartilhadas nesses corpos (`app/chamados/services/becos.py:33`, `app/ponto/services/lastro.py:401`). Incidência concreta em produção: [nao verificado].

CENSO: os dois mecanismos específicos não constam das tabelas/decisões do censo transcrito; classificar como **NOVO**, sem confundir “0 escritores persistentes de tarefa” com “nenhum escritor de seus insumos”. O próprio censo limita sua cobertura transitiva — essa não é prova de ausência. Linha física do anexo transcrito: [nao verificado].

## 12/11 09:39 · sessao 1bad645ac0f9 · - · - · origem=Ronald (tmux)

ACHADO 2 | CODIGO: confere em parte — `data_evento` aceita NULL (`app/chamados/models.py:2424`), e a carga filtra intervalo, excluindo essas linhas (`app/chamados/services/tarefa_do_dia.py:374`); sem data, C6 pula a janela e pode devolver viva se não materializada (`app/chamados/juizes.py:338`, `app/chamados/juizes.py:1455`, `app/chamados/juizes.py:1466`), mas antes aplica validação, tranca, vias e veredito da célula (`app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1395`, `app/chamados/juizes.py:1428`, `app/chamados/juizes.py:1438`); a porta atual de abertura recusa NULL (`app/chamados/models.py:2788`). | CENSO: NOVO — DECISÃO 8 do anexo cobre conversas, não fato com data nula; numeração física do anexo [nao verificado]. | EFEITO: sim, como possibilidade condicional: C17 pode considerar pergunta ligada à célula enquanto C19 não a carrega (`app/chamados/juizes.py:1571`, `app/chamados/juizes.py:1651`, `app/chamados/services/tarefa_do_dia.py:376`); existência dessas linhas na frota e desaparecimento nos consumidores após migração [nao verificado].

ACHADO 3 | CODIGO: confere em parte — C17 agrupa pela FK de célula (`app/chamados/juizes.py:1571`, `app/chamados/juizes.py:1574`), e C19 pelo par colaborador/data_evento e igualdade com o dia (`app/chamados/services/tarefa_do_dia.py:380`, `app/chamados/services/tarefa_do_dia.py:236`); porém a porta citada de rechaveamento atualiza **ambos** data e célula para o novo dia, no mesmo save (`app/chamados/services/fio.py:1035`, `app/chamados/services/fio.py:1038`, `app/chamados/services/fio.py:1044`), e o nascimento seleciona a célula do dia do fato (`app/chamados/models.py:2897`); turno noturno, sozinho, não demonstra desencontro. | CENSO: NOVO — DECISÃO 2 do anexo enumera outros quatro conflitos; numeração física [nao verificado]. | EFEITO: sim **se** existir o desencontro e as demais condições das portas/tarefas forem atendidas (`app/chamados/juizes.py:1649`, `app/chamados/juizes.py:1651`, `app/chamados/services/tarefa_do_dia.py:236`); incidência/invariante na frota [nao verificado].

ACHADO 4 | CODIGO: confere em parte — `agora` chega somente à alimentação da fila (`app/chamados/juizes.py:1566`), enquanto perguntas são julgadas sem hoje/agora (`app/chamados/juizes.py:1573`); `fio_vivo` faz o mesmo (`app/chamados/juizes.py:1503`), e a janela usa `date.today()` quando hoje falta (`app/chamados/juizes.py:592`); `_alimentar` não oferece instante (`app/folha/fora_da_folha.py:55`, `app/folha/fora_da_folha.py:62`); os defaults vizinhos conferem (`app/chamados/services/verbos.py:35`, `app/chamados/services/painel.py:190`, `app/chamados/services/vigia_fio.py:566`). O próprio DIFF declara esse limite (`app/chamados/management/commands/diff_e3_sombra.py:105`); a sonda transcrita no censo usa `freeze_time(agora)`, portanto a irreprodutibilidade absoluta alegada não está demonstrada para ela [nao verificado]. | CENSO: NOVO — não há decisão específica sobre relógios no anexo; numeração física [nao verificado]. | EFEITO: sim, como possibilidade de mudar C6/C17: janela pode alterar a conclusão sobre carimbo sem testemunha (`app/chamados/juizes.py:693`, `app/chamados/juizes.py:700`, `app/chamados/juizes.py:1473`), enquanto C19 passa hoje e agora explicitamente (`app/chamados/services/tarefa_do_dia.py:489`, `app/chamados/services/tarefa_do_dia.py:491`); divergência efetiva de paridade perto da virada [nao verificado].

Observação para consolidação: se “EFEITO” significar ocorrência efetiva em algum dia **do dado de produção fornecido**, marque `[nao verificado]` nos três; não há instâncias/linhas de produção no material que demonstrem esses estados. Os “sim” acima são possibilidades condicionais demonstradas pelo código, não medições de frota.

## 12/11 09:39 · sessao e7a5c82407d6 · - · - · origem=Ronald (tmux)

ACHADO 5 | CODIGO: confere em parte — importa os privados em `app/ponto/services/pautas_dos_discordantes.py:61`; alimenta em `:75`, exclui dias cujo fio é diferente de `FIO_SEM_PORTA` em `:77` e inclui os restantes em `:79`; pode escrever Pautas quando `apply=True` (`:173`, `:177`, `:183`). A quebra ou permanência obrigatória no juiz antigo não decorre disso: `_fio_do_dia` chama `_porta` (`app/folha/fora_da_folha.py:77`), que chama C17 (`:67`); alterar internamente `_porta` preservando o contrato alcança esse consumidor. | CENSO: NOVO consumidor concreto; o censo fornecido lista o wrapper, mas não este chamador; linha numérica do anexo [nao verificado]. | EFEITO: nao — este caminho consulta a porta e escreve Pautas, sem alterar entradas dos três juízes (`app/ponto/services/pautas_dos_discordantes.py:75`, `:177`, `:183`).

ACHADO 6 | CODIGO: confere — o resultado de `do_dia` escolhe órfãos com causa `fabrica_nao_rodou`, dispara a fábrica com `notificar=True` e relê o resultado (`app/chamados/services/cobrar_dia.py:45`, `:46`, `:49`, `:53`); mensagem/`ok` usam pergunta/chamado do resultado (`:55`, `:58`, `:70`). `do_dia` usa `VIVOS`, módulos, dia do chamado, perguntas C6 e retenção (`app/chamados/services/pergunta_no_app.py:217`, `:218`, `:220`, `:221`, `:223`). | CENSO: NOVO chamador com efeito de escrita; o serviço já listado pelo copiloto não cobre este uso; linha numérica do anexo [nao verificado]. | EFEITO: sim, potencial estático — a fábrica abre perguntas (`app/chamados/services/disputa_emissao.py:837`), cujo nascimento persiste (`app/chamados/models.py:3018`); a tarefa e C17 carregam essas entradas (`app/chamados/services/tarefa_do_dia.py:374`, `app/chamados/juizes.py:1571`); ocorrência em produção [nao verificado].

ACHADO 7 | CODIGO: confere — qualquer texto não vazio ganha precedência sobre a vitalidade; depois consulta `fio_vivo` sem informar relógio e senão devolve `nada` (`app/folha/previa.py:142`, `:144`, `:146`); essa saída preenche o campo `material` (`:95`). | CENSO: NOVO leitor concreto; ausente da relação de leitores fornecida; linha numérica do anexo [nao verificado]. | EFEITO: nao — muda a apresentação `material` da prévia, não a resposta de tarefa/C6/C17; o caminho mostrado apenas recebe perguntas e devolve rótulo (`app/folha/previa.py:139`, `:142`, `:144`, `:146`).

ACHADO 8 | CODIGO: confere — o contador considera canal a pergunta com `respondida_em` nulo ou chamado em `VIVOS`, sem filtro de silêncio (`app/chamados/services/furo_sem_canal.py:80`, `:82`, `:85`); `VIVOS` inclui `registrado`, enquanto `COBRANCA` não (`app/chamados/catalogo/motor.py:24`, `:29`). Pode excluir do balde sem canal um dia cujo único chamado é registrado/silenciado; ocorrência real [nao verificado]. | CENSO: NOVO leitor concreto; ausente da relação fornecida; linha numérica do anexo [nao verificado]. | EFEITO: nao — o caminho calcula conjuntos e retorna contagens/amostra; não altera entradas ou respostas dos juízes (`app/chamados/services/furo_sem_canal.py:80`, `:85`, `:89`).

## 12/11 09:38 · sessao 4d2389e8adb8 · TRIAGEM-OFFLINE-SEM-CAMERA · leitura · origem=Code

**Sim: a entrega de páginas offline já era defeituosa no pré-v6.** O SW não grava documentos nas navegações normais; abrir as páginas com internet não executa o ramo que guardaria o HTML. Depois, sem rede e sem entrada correspondente no CacheStorage, ele devolve `Response.error()`. Essa cadeia permanece na v6, pois o patch fornecido não altera o SW. (`app/static/sw.js:3–12`, `29–30`, `45–50`, `67–79`.)

**A reprodução literal de “todos os toques inertes” e “tela branca” no iPhone é indeterminada pela leitura.** O código permite demonstrar a falha de entrega do documento; não demonstra a aparência apresentada pelo Safari, os eventos de toque nem o conteúdo efetivo do cache daquele aparelho. [nao verificado]

Nas referências seguintes, **`Δ arquivo:+N` identifica a linha do arquivo novo numerada pelos hunks do patch**, não a posição física na mensagem. **`=` significa pré-v6 e v6 iguais**: SW, views, URLs, home e bases não aparecem como arquivos alterados no diff completo fornecido. Para esses arquivos, não existe linha adicionada no patch para citar.

Para o SW — itens 1, 4 e C1 — a comparação é:

| Questão | Pré-v6 | V6 |
|---|---|---|
| Nome do cache | `juliani-__BUILD_ID__`. (`app/static/sw.js:1`.) | = |
| Precache | `/static/pwa.js`, `/static/manifest.json`, `/static/icons/icon-192.png`, `/static/icons/icon-512.png`. (`app/static/sw.js:3–8`.) | = |
| Instalação | `cache.addAll(STATIC_CACHE)`. (`app/static/sw.js:10–13`.) | = |
| Navegação online | `fetch(event.request)`, **sem gravar a resposta**. (`app/static/sw.js:45–50`.) | = |
| Navegação offline | `caches.match(request)`; ausente, `Response.error()`. (`app/static/sw.js:29–30`, `48`.) | = |
| Fallback de documento | Nenhuma página alternativa: só a resposta correspondente à própria requisição, ou erro de rede. (`app/static/sw.js:29–30`.) | = |
| Gravação dinâmica de HTML | Existe para GET de `/` e `/ponto/bater/…`, **depois do retorno antecipado das navegações**. (`app/static/sw.js:46–50`, `67–79`.) | = |

Portanto, o preparo relatado foi feito, mas **esse preparo por navegação não povoa o cache de documentos neste código**. Isso decorre do retorno em `app/static/sw.js:50`, anterior ao `cache.put` de `app/static/sw.js:74`; não depende de supor que o usuário esqueceu de abrir alguma tela.

Há também uma distinção de rota: a home efetiva do colaborador renderiza `dashboard_colaborador_v2.html`, cujo “Bater ponto” aponta para **`/ponto/bater-v2/`**, não `/ponto/bater/`. (`app/core/views.py:270–271`, `app/templates/core/dashboard_colaborador_v2.html:31`, `app/ponto/urls.py:12`; v6 =.)

| Página/documento | O que uma navegação ou reload offline recebe, nos dois lados |
|---|---|
| Home `/` | Resposta encontrada para a requisição; ausente, `Response.error()`. (`app/static/sw.js:29–30`, `45–50`.) |
| `/ponto/bater/` | O mesmo tratamento. (`app/static/sw.js:29–30`, `45–50`.) |
| Apto da triagem | Não tem URL própria nesses caminhos: é o template escolhido pela view de bater. O cache é consultado pela URL navegada, não pelo nome `apto.html`. (`app/ponto/views.py:225–235`, `358`; `app/static/sw.js:29–30`.) |
| `/ponto/bater-v2/`, destino da home | O mesmo tratamento de navegação; essa rota tampouco está na lista posterior `comCache`. (`app/static/sw.js:45–50`, `68`; `app/ponto/urls.py:12`.) |

**Não há mudança de fallback entre pré-v6 e v6.** O retorno sem documento é erro de rede; não é uma resposta HTML 404/500 fabricada pelo SW. A tela branca como apresentação desse erro no iOS é [nao verificado]. (`app/static/sw.js:29–30`.)

O build id é derivado assim, em ambos os lados:

- `GOVERNADOS_PELO_SW` contém `sw.js`, `pwa.js`, `manifest.json` e os dois ícones. Templates não entram. (`app/core/views.py:524–525`.)
- A raiz é `/app/staticfiles` quando ali existe `sw.js`; caso contrário, `/app/static`. (`app/core/views.py:528–530`.)
- O cálculo concatena os conteúdos no SHA-1, na ordem da tupla, usando `ausente:<nome>` em falha de leitura, e conserva os oito primeiros hexadecimais. Há memoização por processo durante 30 segundos. (`app/core/views.py:520–556`.)
- A view substitui `__BUILD_ID__` pelo resultado e usa `ETag: "sw-<build>"`. (`app/core/views.py:560–576`.)

**A v6 muda uma entrada desse hash: `pwa.js`.** Se o nome resultante mudar, o `activate` apaga os outros caches; o `controllerchange` já podia recarregar páginas sem formulário marcado como sujo. Esses mecanismos são anteriores à v6. Sua ocorrência durante o smoke e os nomes concretos dos caches implantados são [nao verificado]. (`app/core/views.py:548–555`; `Δ app/static/pwa.js:+58–305`; `app/static/sw.js:17–23`; `app/static/pwa.js:34–45`, trecho preservado.)

Sobre os cliques — item 2, C4 e C5 — **os cartões da home não precisam de handlers registrados por `pwa.js`**. Bater, Espelho, Escala, Confirmar Batidas, Chamados, Justificar, Ausência, Holerites e Férias são links `<a href>`, sem `await`, pré-checagem de rede ou teste de `navigator.onLine` antes de navegar. (`app/templates/core/dashboard_colaborador_v2.html:31`, `56`, `60`, `66`, `78`, `82`, `88`, `100`, `110`; v6 =.)

O relógio consulta `/hora/` separadamente; offline, usa seu fallback. Ele não envolve o clique dos cartões. (`app/templates/core/dashboard_colaborador_v2.html:152–159`; v6 =.)

| Controle | Quem fornece o comportamento, nos dois lados |
|---|---|
| Cartões de navegação | O próprio `<a href>`. (`app/templates/core/dashboard_colaborador_v2.html:31–110`.) |
| Menus da casca | Script inline independente do PWA. (`app/templates/base.html:340–382`; `app/templates/base_app.html:98–139`.) |
| Fechar/adiar modal de onboarding | `onclick` inline com `remove()`. Presença desse modal no smoke: [nao verificado]. (`app/templates/core/dashboard_colaborador_v2.html:121–141`.) |
| “Ligar avisos” e fechar o convite | Criados e ligados por `pwa.js`. (`app/static/pwa.js:351–374`; v6: linhas herdadas `+536–559`.) |

As duas cascas carregam `pwa.js` com `defer`; a ausência desse script prejudica a fila e os recursos que ele exporta, mas não remove os `href` dos cartões. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/static/pwa.js:305–318`; v6: `Δ app/static/pwa.js:+489–502`.)

A auditoria de sintaxe/APIs encontrou:

| Construção ou API | Pré-v6 | V6 e alcance da falha |
|---|---|---|
| Optional chaining | `event.data?.type`, `req.result?.valor` e acessos ao cookie. (`app/static/pwa.js:16`, `78`, `140`, `245`, `261`, `403`.) | Já existia; configuração passa a `linha?.valor`. (`Δ app/static/pwa.js:+124`; demais usos preservados.) Parser incompatível pode rejeitar o script inteiro; isso no aparelho relatado é [nao verificado]. |
| Nullish coalescing `??` | Já em `lerConfig`. (`app/static/pwa.js:78`.) | Preservado em `Δ app/static/pwa.js:+124`. |
| `async/await`, arrows, `let/const`, spread, destructuring e `catch` sem binding | Já presentes. (`app/static/pwa.js:11`, `50`, `95`, `209`, `240`, `286`.) | Continuam presentes; não constituem novidade exclusiva da v6. (`Δ app/static/pwa.js:+61–128`, `188–231`.) |
| Chave computada e métodos de objeto | — | Novos no helper de selfie/aviso. São sintaxe analisada junto com o script. (`Δ app/static/pwa.js:+199`, `244`, `253`.) |
| `AbortController` | — no `pwa.js` pré; já usado pelos relógios das telas. (`app/templates/core/dashboard_colaborador_v2.html:154`; `app/templates/ponto/triagem/base_triagem.html:41`.) | Novos usos no PWA verificam `typeof AbortController === 'function'`. (`Δ app/static/pwa.js:+146`, `297`.) |
| `navigator.mediaDevices.getUserMedia` | Chamado nas telas. (`app/templates/ponto/bater_ponto_v2.html:238`; `app/templates/ponto/triagem/apto.html:149`, `248`.) | Chamado dentro do trabalho assíncrono de `_noTeto`, que captura exceções/rejeições; não durante a avaliação inicial. (`Δ app/static/pwa.js:+188–195`, `211–217`.) |
| `indexedDB` | Acesso ao chamar `abrirDB`. (`app/static/pwa.js:48–60`.) | Continua dentro da função, agora com captura explícita. (`Δ app/static/pwa.js:+66–86`.) |
| `serviceWorker`, storage, geolocation e push | Guardas/`try` já existentes. (`app/static/pwa.js:12–30`, `35–46`, `291`, `333–344`, `436–443`.) | Preservados; não há nova chamada desses recursos identificada como fatal na avaliação inicial. (`Δ app/static/pwa.js`, trechos preservados.) |

**Primeiro iOS suportado para cada construção/API acima: [nao verificado].** O código e o anexo não fornecem uma matriz de compatibilidade.

Não há `structuredClone`, `AbortSignal.timeout`, `crypto.randomUUID`, `Promise.any` nem `await` de nível superior no PWA auditado; os `await` estão dentro de funções assíncronas. (`app/static/pwa.js:1–443`; `Δ app/static/pwa.js:+45–305`, `489–511`.) Não identifiquei no acréscimo da v6 uma nova API sem guarda chamada sincronamente antes dos exports; erro real de avaliação no Safari permanece [nao verificado].

A espera de `/hora/` na inicialização também ocorre **depois** da definição de `window.JulianiPWA`; sua pendência não impede, por essa cadeia, a criação dos exports. (`app/static/pwa.js:305–326`; v6: `Δ app/static/pwa.js:+489–511`.)

Para C2, o SW **não contém tratamento de `response.redirected`**: não rejeita a resposta, não a reconstrói e não normaliza seu URL antes de servir. Os ramos que gravam cache clonam a resposta e a guardam pela requisição original. (`app/static/sw.js:29–30`, `54–60`, `67–77`; v6 =.)

Entretanto, **o código chama `fetch(event.request)`**, sem estabelecer `redirect: 'follow'`. Não basta aplicar o padrão de `fetch(url)` e afirmar qual era o modo de redirect da requisição de navegação concreta. Esse valor e uma resposta `redirected=true` efetivamente guardada no aparelho são [nao verificado]. (`app/static/sw.js:42`, `48`, `57`, `71`.)

Há estes caminhos condicionais de redirect, iguais nos dois lados:

- Home: login, troca de senha e ciência de comunicado. (`app/core/views.py:117`, `122–123`, `136–140`; `app/config/settings/base.py:106`.)
- Bater e bater-v2: login, onboarding e ciência de comunicado. (`app/ponto/views.py:241–255`.)
- Ciência pode produzir URL com `?next=…` e `app=1`. (`app/comunicados/services.py:36–50`.)

Não existe redirect obrigatório nos caminhos liberados: eles renderizam os templates. Qual condição ocorreu no smoke é [nao verificado]. (`app/core/views.py:270–271`; `app/ponto/views.py:356–358`.) O `CommonMiddleware` está configurado, mas o redirect concreto de URL sem barra não foi demonstrado. (`app/config/settings/base.py:57`; comportamento efetivo [nao verificado].)

**Um GET não navegacional de `/` ou `/ponto/bater/…` pode guardar a resposta recebida sem verificar se ela veio de redirect**, inclusive porque a exclusão de login é decidida pelo pathname original. Depois, o fallback não verifica `redirected` ao servi-la. Isso é possibilidade do código; não prova que aconteceu no iPhone. (`app/static/sw.js:39–43`, `67–77`, `29–30`; v6 =.) A recusa específica pelo Safari/iOS e sua manifestação como branco são [nao verificado].

Para C3, o SW usa a requisição completa como chave em `cache.put` e em `caches.match`, sem `ignoreSearch`, `ignoreVary` ou normalização de barra. Portanto não programa equivalência entre URLs com queries diferentes ou com/sem barra; também não pede para ignorar `Vary`. (`app/static/sw.js:30`, `56`, `59`, `74`; v6 =.) Os headers `Vary`, URLs finais e pares de chaves realmente presentes no dispositivo são [nao verificado].

Assim, **C1, C2, C3 e C4 já tinham essas características antes da v6**: mesmo ramo de navegação, mesmo tratamento de respostas, mesmas chaves e mesmos links da home. (`app/static/sw.js:29–79`; `app/templates/core/dashboard_colaborador_v2.html:31–110`; arquivos ausentes do diff completo fornecido.)

Sobre `bater_ponto.html`, item 3:

| Referência pré-v6 | O que ela significa | V6 |
|---|---|---|
| Views e URLs ativas | `/bater/` seleciona triagem; `/bater-v2/` renderiza v2. Nenhum desses renders aponta para o legado. (`app/ponto/urls.py:11–12`; `app/ponto/views.py:225–235`, `356–358`.) | = |
| Verificador da lei | Lê o arquivo para inspeção textual. (`app/scripts/check_a_lei_batida.py:20–23`.) | Referências removidas no hunk `Δ app/scripts/check_a_lei_batida.py:@@ -17,10 +17,7 @@`. |
| Verificador de URLs | Inclui o arquivo na lista inspecionada. (`app/tools/check_frontend_urls.py:29`.) | Entrada removida no hunk `Δ app/tools/check_frontend_urls.py:@@ -26,7 +26,6 @@`. |
| Links dentro do legado | Usam o nome da rota `ponto:bater_ponto`, não o arquivo como destino. (`app/templates/ponto/bater_ponto.html:27`, `201`.) | Arquivo inteiro eliminado: `Δ app/templates/ponto/bater_ponto.html:@@ -1,614 +0,0 @@`. |
| SW/PWA e retorno offline | SW não precacheia esse documento; PWA envia à API; apto retorna à home. (`app/static/sw.js:3–8`; `app/static/pwa.js:153`; `app/templates/ponto/triagem/apto.html:115–118`, `298`.) | Nenhum desses caminhos passa a depender do arquivo apagado. (`Δ app/static/pwa.js`, sync preservado; `Δ app/templates/ponto/triagem/apto.html:+307`.) |

**A exclusão não retirou página alguma do precache nem rompeu os renders ativos identificados.** Atribuir a ela 404, 500 ou branco não é sustentado por esses caminhos; ocorrência real por outro caminho é [nao verificado]. (`app/static/sw.js:3–8`; `app/ponto/views.py:356–358`; hunk de exclusão acima.)

As mudanças da v6 que efetivamente alteram o caminho **depois de chegar à tela de batida** são:

| Pré-v6 | Alteração da v6 |
|---|---|
| V2 recusa continuar sem foto. (`app/templates/ponto/bater_ponto_v2.html:285–290`, `312`.) | Passa a consultar o helper de selfie e seguir com aviso. (`Δ app/templates/ponto/bater_ponto_v2.html:+269–296`.) |
| Apto offline esconde confirmar após rejeição da câmera; a gravação fica dentro do callback de `toBlob`. (`app/templates/ponto/triagem/apto.html:248–282`.) | Mantém confirmar e consulta captura com teto. (`Δ app/templates/ponto/triagem/apto.html:+245–293`.) |
| Escritor espera `obterGap`, que pode esperar `/hora/` e seu corpo sem teto. (`app/static/pwa.js:86–108`.) | Limita o gap antes da fila. (`Δ app/static/pwa.js:+135–162`, `269–276`.) |
| Transação da fila trata `complete` e `error`. (`app/static/pwa.js:111–121`.) | Helper trata também `abort`; demora mostra aviso e continua esperando o evento definitivo. (`Δ app/static/pwa.js:+90–103`, `239–286`.) |

O RED headless adequado deve exercitar **navegação HTTP real com SW**, porque o teste novo da v6 abre templates em `file://` e substitui rede/armazenamento; ele não cobre o trajeto home → documento offline, e declara não provar Safari. (`Δ app/ponto/tests/test_selfie_nao_barra_chromium.py:+9–18`, `30–34`.)

Proposta de RED, **não executada** [nao verificado]:

1. Usar contexto novo, usuário de teste liberado dos redirects e SW controlando a página; abrir com rede home, bater-v2 e bater/confirmar, depois voltar à home. As rotas e condições estão em `app/ponto/urls.py:11–12`, `app/ponto/views.py:241–255`, `356–358`.
2. Cortar a rede e clicar no `<a>` “Bater ponto”; exigir que apareça o documento de bater-v2. (`app/templates/core/dashboard_colaborador_v2.html:31`.)
3. Registrar a batida e exigir uma linha persistida em `batidas_pendentes`; testar câmera funcional e, separadamente, câmera ausente. (`app/static/pwa.js:112–120`; v6: `Δ app/static/pwa.js:+275–282`.)
4. Recarregar offline home e cada tela; exigir documento utilizável. O ramo sob teste é `app/static/sw.js:45–50`.

**No contexto novo preparado apenas por navegações, o primeiro passo que viola o esperado já é o clique da home:** o SW não guardou o documento durante o preparo e entrega `Response.error()` quando a consulta não encontra entrada. O reload tem a mesma falha. Isso vale para pré-v6 e v6. (`app/static/sw.js:3–12`, `29–30`, `45–50`, `67–79`.) Se a tela for alcançada por outro estado de cache, o pré-v6 ainda possui as barreiras de foto e espera da hora descritas acima; qual delas ocorreu no campo é [nao verificado].

Por fim, estes são os escritores e leitores dos caminhos de execução do front:

| Dado | Quem escreve/define | Quem lê — pré-v6 e v6 |
|---|---|---|
| Build id | Calcula SHA-1 e grava `_CACHE_DO_BUILD['v']`. (`app/core/views.py:547–556`; v6 =.) | Memoização e view `/sw.js`. (`app/core/views.py:543–545`, `562`, `572`; v6 =.) |
| `GOVERNADOS_PELO_SW` | Tupla declarada em `app/core/views.py:524–525`; v6 =. | Laço do hash em `app/core/views.py:548`; v6 =. |
| `CACHE_NAME` | View injeta o build; SW define a constante. (`app/core/views.py:572`; `app/static/sw.js:1`; v6 =.) | Instalação, limpeza de caches e gravações dinâmicas. (`app/static/sw.js:12`, `20`, `59`, `74`; v6 =.) |
| Lista de precache | Array `STATIC_CACHE`. (`app/static/sw.js:3–8`; v6 =.) | `cache.addAll`. (`app/static/sw.js:12`; v6 =.) |
| Conteúdo do CacheStorage | `addAll` e `cache.put`. (`app/static/sw.js:12`, `59`, `74`; v6 =.) | `caches.match`, consultando caches sem restringir por `CACHE_NAME`. (`app/static/sw.js:30`, `56`; v6 =.) |
| Banco e chave da fila | IndexedDB **`juliani-offline`, versão 2**, store **`batidas_pendentes`**, chave **`id` autoIncrement**. Pré: `app/static/pwa.js:50–54`; v6: `Δ app/static/pwa.js:+73–77`. | Operações abaixo; não é uma chave de `localStorage`. (Mesmas referências.) |
| Inclusão de batida | Pré: `registrarBatidaOffline` → `add`, `app/static/pwa.js:107–120`. V6: `Δ app/static/pwa.js:+269–282`. | Sync usa `getAll`: pré `app/static/pwa.js:132`; v6 linha herdada `+313`. |
| Contagem da fila | As inclusões/exclusões alteram a quantidade. (Referências desta tabela.) | Indicador usa `count`: pré `app/static/pwa.js:190`; v6 herdada `+371`. |
| Exclusão da fila | Sync usa `delete(b.id)` após sucesso ou HTTP 400: pré `app/static/pwa.js:158–174`; v6 herdadas `+339–355`, deletes em `+343`, `+353`. | Próximos `getAll`/`count` acima. |
| Configuração do relógio, separada da fila | Store `config`, chave `chave`; `put` de `ntp_gap`/`ntp_calculado_em`. Pré: `app/static/pwa.js:56`, `67`, `92–93`; v6: `Δ app/static/pwa.js:+79`, `114`, `158–159`. | Pré `get` em `app/static/pwa.js:77`; v6 `Δ app/static/pwa.js:+123`. |

As linhas v6 marcadas **herdadas** resultam do deslocamento `+181` nos trechos de sync/indicador preservados pelo patch; não estão impressas individualmente nele. (`Δ app/static/pwa.js:@@ -104,23 +170,138 @@`, `@@ -308,6 +489,9 @@`.)

O SW não abre essa fila: envia `SYNC_BATIDAS` às páginas, e o receptor de `pwa.js` chama a sincronização. A limpeza de caches usa CacheStorage, enquanto a fila permanece no IndexedDB. (`app/static/sw.js:19–20`, `112–125`; `app/static/pwa.js:15–16`, `50–54`; v6 = nesses trechos, banco em `Δ app/static/pwa.js:+73–77`.)

## 12/11 09:38 · sessao fb866086792a · - · - · origem=Ronald (tmux)

- O caso sintético exige nome inteiro da primeira coluna, preserva seus dois pedaços e recusa o inteiro da segunda coluna e uma combinação inventada; portanto morde retirar o aceite do inteiro ou adicionar também o inteiro da descrição. O próprio teste não contém parênteses, vírgulas ou prosa na primeira coluna. `bin/tests/test_lei_akita.sh:433`, `bin/tests/test_lei_akita.sh:444`.
- Os dois casos novos exercitam `julgar`: `Parallel Change / Expand-Contract` precisa passar; `Parallel Change / Padrao Inventado` precisa falhar. A comparação é executada contra a expectativa booleana, logo morde negar o nome completo e aceitar qualquer expressão com um pedaço válido. `bin/tests/test_lei_akita.sh:449`, `bin/tests/test_lei_akita.sh:450`, `bin/tests/test_lei_akita.sh:471`.
- O novo cenário de duplicata publica `dup.py`, recria o mesmo conteúdo em outro commit com padrão inválido e acrescenta um patch diferente, também inválido, em `outro.py`; exige somente a falha de `outro` e a associação exata local→publicado. Assim morde retirar a exceção, aplicá-la indiscriminadamente ou informar SHA publicado incorreto nesse cenário. `bin/tests/test_lei_akita.sh:778`, `bin/tests/test_lei_akita.sh:783`, `bin/tests/test_lei_akita.sh:789`, `bin/tests/test_lei_akita.sh:791`, `bin/tests/test_lei_akita.sh:795`.
- A nova prova tem somente um local duplicado: não cobre dois locais com o mesmo patch-id. A implementação descarta todos os SHA locais além do primeiro por `setdefault(pid, sha)` e depois inverte o mapa; portanto só um deles pode ser reconhecido como duplicata. `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:778`.
- Não há injeção de erro nem saída malformada de `patch-id` nesse teste; um mutante que trocasse a exceção por `return {}` somente quando `patch-id` falha não é exercitado pelo cenário novo. A execução efetiva desse mutante permanece [não verificado]. `bin/tests/test_lei_akita.sh:379`, `bin/tests/test_lei_akita.sh:380`, `bin/tests/test_lei_akita.sh:778`.
- Repo raso e ausência de `origin/main` já têm casos anteriores, que exigem `SystemExit` contendo `PADRAO_ERRO`; isso não equivale a testar falha do subprocesso `patch-id`. `bin/tests/test_lei_akita.sh:662`, `bin/tests/test_lei_akita.sh:703`, `bin/tests/test_lei_akita.sh:707`.
- O teste novo não cobre commits vazios, apenas modo, binário, raiz ou mudanças de whitespace semanticamente relevantes: planta adições textuais em dois arquivos diferentes. `bin/tests/test_lei_akita.sh:649`, `bin/tests/test_lei_akita.sh:778`.
- `MORDE_OK` é impresso mesmo após diagnósticos `MORDE_FALHOU`, mas o shell exige simultaneamente aquele marcador e ausência destes; portanto um diagnóstico de falha não vira verde apenas pela presença do OK. `bin/tests/test_lei_akita.sh:809`, `bin/tests/test_lei_akita.sh:827`.
- A desestruturação sem validação `pid, sha = ln.split()` pode lançar `ValueError`, que não é convertido em `PADRAO_ERRO`; o shell ainda reprova porque faltará `PADRAO_FIM`. `bin/tests/test_lei_akita.sh:384`, `bin/tests/test_lei_akita.sh:817`, `bin/tests/test_lei_akita.sh:823`.
- Os quatro mutantes e os logs RED/GREEN alegados no anexo não foram executados nem comprovados por esta leitura [não verificado].

## 12/11 09:38 · sessao 133de6d0f685 · - · - · origem=Ronald (tmux)

- `ENUMERADOS` é escrito como `len(hs)` e `JULGADOS` inicia em zero; commits sem linha `LEI-AKITA:` ficam fora de `JULGADOS`, duplicatas também; merges com linha entram antes de verificar seus hunks. Portanto `JULGADOS` não significa somente commits efetivamente aprovados/reprovados nem todos os commits com linha. (`bin/tests/test_lei_akita.sh:389`, `:398`, `:401`, `:404`, `:410`)
- `DUPLICATAS` é zerado por chamada, recebe pares `(sha local de 8 caracteres, sha publicado de 12)` e é lido na emissão normal e no MORDE novo. (`bin/tests/test_lei_akita.sh:374`, `:402`, `:794`, `:814`)
- O Python escreve `PADRAO_FALHA`, `PADRAO_DUPLICATA`, `PADRAO_JULGADOS N de M` e `PADRAO_FIM`; o shell transforma falhas em `FALHA: PADRAO-NA-CURA`, conta-as, transforma duplicatas em `ja julgado:` e exige o sentinela `PADRAO_FIM`. (`bin/tests/test_lei_akita.sh:813`, `:815`, `:816`, `:817`, `:820`, `:821`, `:822`, `:823`)
- `PADRAO_JULGADOS` é consumido pelo próprio selo para montar a frase final `padrao julgado em N de M commit(s) com linha LEI-AKITA, dos nao publicados`; o formato continua igual, mas a descrição fica incompleta porque agora exclui duplicatas com linha. (`bin/tests/test_lei_akita.sh:401`, `:404`, `:831`)
- `PADRAO_ERRO` não tem parser dedicado: sai no stderr capturado e a ausência de `PADRAO_FIM` gera falha. Uma exceção Python não convertida em `PADRAO_ERRO` também não torna verde por esse caminho. (`bin/tests/test_lei_akita.sh:819`, `:823`, `:830`, `:834`)
- MORDE lê `ENUMERADOS` nos casos de sincronização, outra ref e piso; não há asserção de `JULGADOS` nos casos existentes nem no novo caso de duplicata. O novo caso confere falhas e o par de duplicata. (`bin/tests/test_lei_akita.sh:676`, `:701`, `:794`, `:795`, `:802`)
- `bin/pre-push.sh` passa `PUSH_SHA` por `LEI_AKITA_HEAD` e decide exclusivamente pelo código de saída do selo; não interpreta suas linhas. (`bin/pre-push.sh:33`, `:38`)
- `bin/regua.sh` percorre `tests/test_*.sh`, decide pelo código de saída e só imprime a saída capturada quando o selo falha; não interpreta contagens ou prefixos do akita. (`bin/regua.sh:138`, `:140`, `:142`, `:143`)
- Assim, a adição de `ja julgado:` e a mudança semântica de `JULGADOS` não quebram esses dois leitores concretos. (`bin/pre-push.sh:38`; `bin/regua.sh:140`; `bin/tests/test_lei_akita.sh:822`, `:831`)
- A varredura da cópia não encontrou outro selo que execute akita ou grepe sua saída; ausência de leitor externo à cópia permanece **[não verificado]**. Não apareceu `AGENTS.md` na varredura **[não verificado]**.

## 12/11 09:34 · sessao 1406a8871f6f · - · - · origem=Ronald (tmux)

Auditoria `pwa.js` concluída, somente leitura.

| Assunto | Pré-v6 | V6 — número da linha nova do patch |
|---|---|---|
| Banco | `app/static/pwa.js:50`: `juliani-offline`, versão 2 | `pwa.js:+73`, mesmo banco/versão |
| Criação da fila | `pwa.js:54`: `batidas_pendentes`, `keyPath:'id'`, `autoIncrement:true` | `pwa.js:+77`, mesmos nomes |
| Criação de configuração | `pwa.js:56`: store `config`, chave `chave` | `pwa.js:+79` |
| Escritor de batida | `pwa.js:107`; `add` em `:113` | Função `+269`; `add` em `+276` |
| Leitor para sincronização | `pwa.js:132`: `getAll()` | Linha herdada **+313** |
| Exclusão após sucesso | `pwa.js:162`: `delete(b.id)` | Linha herdada **+343** |
| Exclusão após HTTP 400 | `pwa.js:172`: `delete(b.id)` | Linha herdada **+353** |
| Leitor do indicador | `pwa.js:190`: `count()` | Linha herdada **+371** |
| Configuração do relógio | Escritor `pwa.js:67`, leitores `:77`; chaves em `:92`, `:93`, `:96`, `:101`, `:104` | Escritor `+114`, leitor `+123`; mesmas chaves, funções do relógio preservadas/alteradas no hunk `-93/+159` |

As linhas herdadas acima são **derivadas dos offsets do patch**, não aparecem impressas individualmente nele: depois do grande hunk, pré `126` = v6 `307` (**+181**); três exports novos tornam o deslocamento **+184** até a inicialização; a linha adicional da inicialização torna-o **+185** depois dela (`@@ -308/+489`, `@@ -323/+507`).

A fila não é uma chave de `localStorage`: a chave dos registros é `id` no store IndexedDB `batidas_pendentes` (`pwa.js:50–54`; patch `+73–77`). O SW pede sincronização às páginas mediante mensagem, sem abrir diretamente esse banco (`app/static/sw.js:112–125`; receptor `pwa.js:15–16`).

O bloqueio anterior ao `add` já existia: `registrarBatidaOffline` aguarda `obterGap` (`pwa.js:108`), que pode chamar `calcularESalvarGap` (`:101–104`); esta aguarda `fetch('/hora/')` e `resp.json()` sem teto (`:86–88`). A v6 limita o gap na entrada do escritor (`patch pwa.js:+271`), usa `_noTeto` (`+188–195`) e transforma a espera do armazém em aviso, preservando a espera pelo evento definitivo (`+90–103`, `+239–286`). Isso explica uma fila potencialmente pendente, **não prova a causa de toques da home sem navegação** [nao verificado].

Sobre sintaxe e APIs:

- Optional chaining já existia em `pwa.js:16`, `:78`, `:140`, `:245`, `:261`, `:403`; nullish coalescing em `:78`. Na v6, o acesso de configuração vira `linha?.valor ?? null` (`patch +124`); os demais permanecem: `+16`, herdadas `+321`, `+426`, `+442`, `+588`. Um parser incompatível poderia rejeitar o script inteiro, inclusive por construções dentro de funções; atribuir isso ao iPhone do relato é [nao verificado]. Primeiro iOS suportado: [nao verificado].
- `async/await`, arrows, `let/const`, destructuring, spread e `catch` sem binding já aparecem no pré (`pwa.js:11`, `:50`, `:95`, `:209`, `:240`, `:286`). A v6 acrescenta chave computada (`patch +199`) e métodos de objeto (`+244`, `+253`); suporte exato no iOS: [nao verificado].
- A nova chamada `navigator.mediaDevices.getUserMedia` fica **dentro** de função assíncrona passada a `_noTeto` (`patch +211–217`, chamada `+214`), cujo mecanismo captura exceção/rejeição (`+194`). Não é chamada na avaliação inicial do script (`+211–217`).
- Os dois novos usos de `AbortController` são condicionais a `typeof AbortController === 'function'` (`patch +144`, `+297`). `navigator.storage` já tem guarda e `try/catch` no pré (`pwa.js:436–443`, herdadas v6 `+621–628`). `serviceWorker` já tem guarda no pré (`:12`, `:35`); a instalação do listener síncrono é antiga (`:41`).
- Não há novos `structuredClone`, `AbortSignal.timeout`, `crypto.randomUUID`, `Promise.any` ou top-level `await` no patch de `pwa.js`; os `await` novos estão em funções assíncronas (`patch +111–128`, `+142–160`, `+211–231`, `+269–305`, `+507–511`). Ausência desses símbolos no script pré conferida em `app/static/pwa.js:1–443`.
- Nenhuma nova API sem guarda foi identificada como chamada síncrona na avaliação inicial, antes dos exports; os novos corpos de câmera, armazenamento e rede executam ao chamar suas funções (`patch +66–128`, `+188–231`, `+269–305`). Erro real de avaliação no Safari específico: [nao verificado].
- O export `window.JulianiPWA` já é definido **antes** do IIFE que espera a hora (`pwa.js:305–326`); na v6, export e inicialização continuam nessa ordem (`patch +489–511`). Portanto `/hora/` pendurada na inicialização não impede, por essa cadeia, a criação de `window.JulianiPWA` (`pwa.js:305–326`; patch `+489–511`).

Achado que afeta diretamente o RED: navegações retornam cedo no SW (`app/static/sw.js:46–50`), antes do ramo que gravaria home e bater (`:68–77`). Abrir as páginas com rede **não executa aquele `cache.put` para requisições `navigate`**, e o precache só contém quatro recursos estáticos (`:3–8`). Assim, o preparo humano relatado não demonstra presença do HTML no Cache Storage desse SW; conteúdo efetivo do cache no aparelho permanece [nao verificado].

## 12/11 09:32 · sessao c24c40daec67 · - · - · origem=Ronald (tmux)

Cadeia efetiva da home:

- Home `/` é `core/urls.py:17` → `dashboard`, protegida por login em `core/views.py:117`, renderizando `core/dashboard_colaborador_v2.html` em `core/views.py:270-271`.
- Bater é `<a href="{% url 'ponto:bater_ponto_v2' %}">`, em `app/templates/core/dashboard_colaborador_v2.html:31`; destino `/ponto/bater-v2/`, por `app/ponto/urls.py:12`. Não é `/ponto/bater/`.
- Outros cartões também são links comuns: Espelho `:56`, Escala `:60`, Confirmar Batidas `:66`, Chamados `:78`, Justificar `:82`, Ausência `:88`, Holerites `:100`, Férias `:110`. Não têm handler PWA nem await antes da navegação nessas linhas.
- Script do relógio é independente: `app/templates/core/dashboard_colaborador_v2.html:152-159`; offline chama fallback e retorna `:153`, fetch `/hora/` apenas em `:155`.
- A home antiga também usa link comum a v2 em `app/templates/core/dashboard_colaborador.html:39`.
- PWA é carregado defer em `app/templates/base.html:20` e `app/templates/base_app.html:20`. Dropdowns têm registro próprio em script inline, `base.html:340-382`, `base_app.html:97-139`; não dependem do carregamento de `pwa.js`.
- `hasner-ui.js` impede clique padrão apenas em `.dropdown-toggle`: `app/static/js/hasner-ui.js:14-33`. Não intercepta os cartões acima.
- Botões efetivamente criados por PWA: convite “Ligar avisos” e fechar, `app/static/pwa.js:351-374`, handlers `:361` e `:365`.
- Há modal condicional que cobre a tela em `dashboard_colaborador_v2.html:121-141`, removido pelos onclick `:126`, `:138`. Sua presença no smoke é [nao verificado]; não apontar como causa.

SW e preparo:

- Precaching é só quatro arquivos estáticos, `app/static/sw.js:3-8`, gravados por `cache.addAll` em `:12`.
- Toda navegação retorna antes do ramo que grava HTML: `request.mode === 'navigate'` em `:46`, fetch com fallback em `:48`, return em `:50`.
- Ramo `comCache` para `/` e `/ponto/bater/`, `:68-79`, é alcançável apenas para GET que não seja navegação nem estático. Seu `cache.put` está em `:74`. Portanto abrir home/bater com internet em navegação comum não grava documentos nesse SW; isso permanece verdadeiro mesmo com o preparo relatado.
- Na falta de rede, navegação usa match da request, depois `Response.error()`, `sw.js:29-30,46-50`. Não há fallback de documento.
- `/ponto/bater-v2/` também passa pela navegação `:46-50`; não é incluído sequer na lista `comCache` de `:68`.
- Não há diferença desses arquivos na v6 no patch fornecido. Não existe linha de patch v6 para SW/home/views/URLs porque são inalterados.

Rotas, apto e redirects:

- `/ponto/bater/` → `views.bater_ponto`, `app/ponto/urls.py:11`; `/ponto/bater-v2/` mesma view com `ui_v2=True`, `:12`.
- `apto.html` é template escolhido, não URL independente: `app/ponto/views.py:225-235`; GET normal renderiza em `:358`, v2 em `:356-357`.
- Possíveis redirects home: login por decorator `core/views.py:117` com `LOGIN_URL='/login/'` em `config/settings/base.py:106`; troca de senha `core/views.py:122-123`; ciência `:136-140`; supervisor para outro painel `:229-230`.
- Possíveis redirects bater/ambas: login `ponto/views.py:241`; onboarding incompleto `:243-244`; ciência `:251-255`.
- Ciência constrói `/comunicados/ciencia/?next=...`, `app/comunicados/services.py:36-50`; depende de pendências `:48`.
- Não há redirect obrigatório para home `/` ou URLs canônicas com barra, conforme caminhos/render acima. Redirect efetivamente ocorrido no aparelho é [nao verificado].
- `CommonMiddleware` é configurado em `app/config/settings/base.py:57`; redirecionamento concreto sem barra depende de execução/configuração Django [nao verificado].
- Home Chamados pode acrescentar `?aba=cham`, `dashboard_colaborador_v2.html:78`; isso não é redirect.
- Template apto volta para home via `<a>` em `app/templates/ponto/triagem/apto.html:115-118`; após fila também usa `window.location.href` no timer em `:298-307`. Não aponta para template legado.

Template removido:

- Nenhum renderizador identificado para `ponto/bater_ponto.html`. View ativa renderiza v2 ou triagem (`app/ponto/views.py:225-235,356-358`).
- Referências literais existentes são ferramentas: `app/scripts/check_a_lei_batida.py:20-23` e `app/tools/check_frontend_urls.py:29`. Ambos são removidos no patch fornecido.
- Links dentro do próprio legado usam nome de URL `ponto:bater_ponto`, por exemplo `app/templates/ponto/bater_ponto.html:27` e `:201`, que resolve à view ativa; não são chamadas do arquivo legado.
- SW não precacheia template/página antiga (`app/static/sw.js:3-8`), não tem redirect para ela (`:29-30,46-50`); pwa faz sync para API (`app/static/pwa.js:153`).
- Exclusão não retira documento da lista de precache nem rompe render da view pelos caminhos acima. A alegação de 404/500/branco causado pela exclusão não é sustentada; comportamento real é [nao verificado].

Complemento útil: configuração prod/saas declara `NoStoreHTMLMiddleware` em `app/config/settings/prod.py:40-46` e `app/config/settings/saas.py:71-74`; middleware aplica `Cache-Control: no-store, no-cache, must-revalidate, max-age=0` a HTML em `app/core/middleware.py:207-213`. Uso efetivo desse perfil na implantação é [nao verificado].

## 12/11 09:31 · sessao 406759e16cfd · FABLE-REVISOR-DE-DESENHO · leitura · origem=Code

1. **Cobertura parcial.** Faltam explicitar a ordem de abertura, a pergunta sobre caminho que “o censo não viu”, o descarte com “1 linha de motivo” e o **NÃO MUDA**. O aval exige esses pontos; o contrato apenas o referencia. (`aval_fable_revisor_de_desenho_1211.txt:2`, `:3`, `:5`, `:8`; `contrato.txt:1`). Há diferença de momento: “ao abrir construtor” versus “paralelo ao censo”; E3 já aberto recebe entrada tardia. Não há ordem de segurar vaga, trocar papéis ou criar código: “ninguém”, “a vaga não espera”, “sem código, nada em bin/”; “nenhum hook” não é repetido. (`contrato.txt:1`; `aval_fable_revisor_de_desenho_1211.txt:1`, `:4`).

2. **A tabela de totais cabe; a avaliação fica incompleta.** “Achados/procedentes/que o opus pegaria/voltas…/tokens/tempo” são campos previstos, permitindo verificar procedentes zero. Porém “que o opus pegaria” não tem critério de apuração, e “voltas sem queda” não define como comparar com a referência. (`contrato.txt:1`; `aval_fable_revisor_de_desenho_1211.txt:6`, `:7`). Para dispensar reconstrução, faltam registros vinculados por obra: achado e evidência de novidade perante o censo, decisão/motivo, fundamento da atribuição ao opus e comparação das voltas com a referência. O contrato exige os totais, mas não esses vínculos. (`contrato.txt:1`).

3. **O repetido fica sem classificação.** “Sem resposta ou sem arquivo:linha = conta 0 achados” classifica os dois primeiros casos. Um achado correto que apenas repete o censo não recebe destino explícito; conferir no arquivo não demonstra novidade. (`contrato.txt:1`). A distinção importa porque a pergunta autorizada é precisamente o que “o censo não viu”. (`aval_fable_revisor_de_desenho_1211.txt:3`).

## 12/11 09:28 · sessao b63c886da997 · - · - · origem=Ronald (tmux)

- **`bater_ponto.html` não é o template da rota `/ponto/bater/`.** A URL `bater/` chama `views.bater_ponto`; `bater-v2/` chama a mesma view com `ui_v2=True` (`app/ponto/urls.py:11-12`). A view renderiza `ponto/bater_ponto_v2.html` no ramo v2; no outro ramo usa `_template_triagem(context)`, que pode devolver `ponto/triagem/apto.html` (`app/ponto/views.py:225-235`, `app/ponto/views.py:356-358`).

- **As referências executáveis literais encontradas ao arquivo apagado são verificadores de fonte:** `app/scripts/check_a_lei_batida.py:20-23` e `app/tools/check_frontend_urls.py:29`. O patch fornecido remove exatamente essas entradas, além de apagar o template; não altera `ponto/views.py`, `ponto/urls.py` nem `sw.js` (anexo `diff_v6_vs_pre.patch`, hunks desses arquivos). Referências dinâmicas não identificadas pela busca literal: [nao verificado].

- **O template apagado nunca esteve nessa lista de precache.** `STATIC_CACHE` contém somente `/static/pwa.js`, `/static/manifest.json`, `/static/icons/icon-192.png` e `/static/icons/icon-512.png` (`app/static/sw.js:3-8`). O `cache.addAll` usa essa lista (`app/static/sw.js:10-13`). A remoção de `bater_ponto.html` não remove entrada alguma dela; o patch não modifica `sw.js` (anexo `diff_v6_vs_pre.patch`).

- **Não há redirect offline para `bater_ponto.html`.** O fallback do SW procura a própria request e devolve `Response.error()` se não encontra cópia (`app/static/sw.js:29-30`, `app/static/sw.js:45-50`). O redirect após sucesso offline do apto aponta para `core:dashboard`, assim como seu link “Ir para o início” (`app/templates/ponto/triagem/apto.html:115`, `app/templates/ponto/triagem/apto.html:292-299`). O patch mantém o destino `core:dashboard` no hunk `@@ -263,41 +270,41 @@`.

- **A navegação HTML retorna antes do trecho que gravaria home/bater no cache.** `mode === 'navigate'` executa fetch com fallback e retorna (`app/static/sw.js:45-50`); o `comCache` com `cache.put` aparece depois (`app/static/sw.js:67-79`). Assim, esse `cache.put` não é atingido pelas navegações usuais; aparência efetiva do erro de rede no Safari: [nao verificado].

- **A fila offline usa IndexedDB, não uma chave de localStorage:** banco `juliani-offline`, versão `2`; store `batidas_pendentes`, chave primária `id` com `autoIncrement` (`app/static/pwa.js:50-54`). O escritor insere com `add` (`app/static/pwa.js:107-122`); sync lê por `getAll` (`app/static/pwa.js:126-134`), remove após sucesso (`app/static/pwa.js:158-164`) e também após HTTP 400 (`app/static/pwa.js:166-174`); indicador lê `count` (`app/static/pwa.js:184-192`).

- **Os chamadores de escrita pre-v6 são:** v2 no catch de rede (`app/templates/ponto/bater_ponto_v2.html:333`), v2 explicitamente offline (`app/templates/ponto/bater_ponto_v2.html:346`), apto offline (`app/templates/ponto/triagem/apto.html:282`) e os dois caminhos do template apagado, sem renderizador encontrado (`app/templates/ponto/bater_ponto.html:543`, `app/templates/ponto/bater_ponto.html:597`; cadeia de renderização em `app/ponto/views.py:356-358`).

- **O SW pede sincronização; quem acessa a fila é pwa.js.** O SW envia `SYNC_BATIDAS` aos clientes abertos (`app/static/sw.js:112-125`); pwa.js recebe a mensagem e chama `sincronizarBatidas` (`app/static/pwa.js:15-16`). Também dispara sync ao voltar online e no bootstrap online (`app/static/pwa.js:320`, `app/static/pwa.js:328`).

- **Na v6, nomes e identidade da fila permanecem; muda a espera da escrita.** O patch mantém `indexedDB.open('juliani-offline', 2)` e criação de `batidas_pendentes` no hunk `@@ -45,9 +45,32 @@`; troca a gravação direta pela função `_transacao`, com `complete`, `error` e `abort`, no hunk `@@ -55,37 +78,80 @@`; limita o gap antes do `add` e mostra aviso enquanto o armazenamento permanece pendente no hunk `@@ -104,23 +170,138 @@`. Os leitores/deletores abaixo de `sincronizarBatidas` não são alterados por esse patch (mesmo hunk termina imediatamente antes dessa função).

- **Linhas v6 dos consumidores inalterados, calculadas pelo deslocamento `+181` do patch:** `sincronizarBatidas` começa em `app/static/pwa.js:307`, `getAll` fica em `:313`, deletes em `:343` e `:353`, `count` em `:371`; pre-v6 correspondentes `:126`, `:132`, `:162`, `:172`, `:190` (anexo `diff_v6_vs_pre.patch`, hunk `@@ -308,6 +489,9 @@`, e `app/static/pwa.js:126-190`).

## 12/11 09:28 · sessao 29bcabb8275e · - · - · origem=Ronald (tmux)

- A home do colaborador renderizada é `core/dashboard_colaborador_v2.html` (`app/core/views.py:270-271`); ela estende `base.html` (`app/templates/core/dashboard_colaborador_v2.html:1`).
- Seus cards são links HTML nativos: Bater (`:31`), Espelho (`:56`), Escala (`:60`), Confirmar Batidas (`:66`), Chamados (`:78`), Justificar (`:82`), Ausência (`:88`), Holerites (`:100`), Benefícios (`:104`), Férias (`:110`), todos em `app/templates/core/dashboard_colaborador_v2.html`. Não registram handlers em `pwa.js`; a navegação efetiva no iPhone é [nao verificado].
- Os botões “×” e “Mais tarde” do modal usam `onclick` inline para remover `onb-modal`, independentes de `pwa.js` (`app/templates/core/dashboard_colaborador_v2.html:126`, `:138`); modal presente/bloqueando cliques no aparelho é [nao verificado].
- A home antiga também usa link nativo para Bater (`app/templates/core/dashboard_colaborador.html:39`) e fecha modal por `onclick` inline (`app/templates/core/dashboard_colaborador.html:187`, `:199`).
- `pwa.js` carrega com `defer` em ambas as cascas (`app/templates/base.html:20`; `app/templates/base_app.html:20`). Ajuda/Minha conta têm registro próprio no script inline da casca (`app/templates/base.html:341`, `:345-348`, `:364-370`); dropdowns de `base_app` também têm script separado (`app/templates/base_app.html:99`, `:121-127`).
- `hasner-ui.js` intercepta clique somente em `.dropdown-toggle`; não é um interceptador universal dos cards (`app/static/js/hasner-ui.js:14-32`). O `<body>` das duas cascas é simples, sem `hx-boost` (`app/templates/base.html:91`; `app/templates/base_app.html:83`).
- Os botões diretamente criados por `pwa.js` são o convite de notificações: fechar (`app/static/pwa.js:358-364`) e ligar avisos (`app/static/pwa.js:365-374`). Seu nascimento depende de a inicialização ultrapassar `await calcularESalvarGap()` (`app/static/pwa.js:323-340`). A v6 troca essa espera por `_noTeto` no hunk do patch `app/static/pwa.js @@ -323,7 +507,8`.
- O relógio da home possui script próprio: offline chama fallback local e retorna (`app/templates/core/dashboard_colaborador_v2.html:143-160`, particularmente `:153`); `AbortController` está dentro de `try` (`:154-157`).
- Optional chaining já existia no `pwa.js` pré-v6: `event.data?.type` (`app/static/pwa.js:16`), `req.result?.valor ?? null` (`:78`), extração CSRF (`:140`, `:245`, `:261`, `:403`). Compatibilidade exata com a versão iOS do aparelho é [nao verificado].
- A inicialização assíncrona não é top-level `await`: é uma IIFE `async` (`app/static/pwa.js:323-346`). As chamadas a câmera introduzidas pela v6 estão em funções executadas sob `_noTeto`, conforme os hunks fornecidos; sua execução real no Safari é [nao verificado].

## 12/11 09:27 · sessao d20c4f297a46 · - · - · origem=Ronald (tmux)

Dois achados introduzidos:

1. **Feriado pode trocar a geradora sem mudança de vínculo.** O serviço percorre todos os vínculos do colaborador e regenera as mesmas datas por cada um. Com vínculos sobrepostos declarados, vence a ordem de iteração. (`ponto/services/cadastro_futuro.py:49`, `ponto/services/cadastro_futuro.py:22`, `ponto/services/cadastro_futuro.py:30`) A porta substitui a geradora pela EC recebida quando cobre a data. (`ponto/portas/celula.py:263`) Para esse evento, selecionar datas agrupadas pela geradora atual.

2. **Foto parcial pode produzir fase errada no 12x36.** A sincronização materializa folgas somente em datas com célula `gerada`. (`escala/folgas.py:607`, `escala/folgas.py:623`) Se a única folga do calendário no mês tiver célula editada, ou estiver além da janela de células existentes, ela desaparece da alimentação. O cálculo então pode retornar à âncora, embora a foto mensal devesse determinar a fase. (`escala/models.py:1280`, `escala/models.py:1289`, `escala/models.py:1353`) Preservar células editadas e não criar células fora da janela permite materializar a `FolgaDia` necessária ao cálculo mensal.

Reprodução dinâmica sem banco: **[nao verificado]**. O decorator de `_hpd_pares` foi excluído dos achados por ser preexistente, conforme informação do agente principal. **[nao verificado por diff]**

## 12/11 09:27 · sessao 3d52e2f9914e · - · - · origem=Ronald (tmux)

Não identifiquei erro de compatibilidade ORM nos arquivos novos: campos e tipos da lavra correspondem ao modelo (`ponto/services/vigia_cadastro_futuro.py:38-46`, `inteligencia/models.py:87-96`), os receivers são carregados (`core/apps.py:8-10`) e o HAIKU lê snapshots (`api/views_mensageria.py:647-648`).

Pontos pendentes concretos:

- Cron continua reescrevendo; o novo teste acusa explicitamente essa pendência (`ponto/portas/celula.py:870-877`, `ponto/tests/test_cadastro_futuro_sem_banco.py:619-628`).
- Falhas de regeneração ainda podem deixar cadastro salvo e futuro antigo: `executar_vinculo` captura a exceção do suplemento (`colaboradores/services/vinculo.py:324-332`); `salvar_tipo` salva antes de capturar a propagação (`escala/services/cadastro_tipo.py:306-323`). O dublê usa transações sem rollback (`ponto/tests/test_cadastro_futuro_sem_banco.py:218`).
- `lavrar()` não tem teste executável nos novos casos; o fio do command é verificado apenas por AST (`ponto/services/vigia_cadastro_futuro.py:38-46`, `ponto/tests/test_cadastro_futuro_sem_banco.py:583-597`).
- O fake ignora ordenação, e o teste HAIKU oferece somente um snapshot; escolher a medição mais recente não está demonstrado (`ponto/tests/test_cadastro_futuro_sem_banco.py:105`, `:575-580`).
- Os contadores novos não fornecem `rotulo`, embora o contrato documentado do endpoint espere esse campo (`ponto/services/vigia_cadastro_futuro.py:33-35`, `api/views_mensageria.py:630-631`).

Comportamento Django real, rollback e custo em frota: [nao verificado]. Nenhum arquivo editado nesta revisão.

## 12/11 09:11 · sessao 3d52e2f9914e · - · - · origem=Ronald (tmux)

A escrita futura do cron está em `ponto/portas/celula.py:870-877`; o command apenas delega em `ponto/management/commands/gerar_celulas.py:309-313`. Alterar só o command exigiria contornar a operação existente; o escritor único está declarado em `core/portas.py:107-113` e cobrado por `ponto/tests/test_chokepoint_celula_gate.py:48-85`.

O leitor HAIKU adequado é `api/views_mensageria.py:627-646`: `api_mensageria_saude` entrega snapshots prontos. Molde: `escala/services/vigencia_impossivel.py:65-74`, incluindo `total=None, sem_medida=True` antes da primeira lavra. URL existente: `api/urls.py:16`.

Django, freezegun e pytest estão ausentes no interpretador consultado; execução ORM permanece [nao verificado]. Não executar `config.settings.ci`: cria diretórios temporários fora de `app/` (`config/settings/ci.py:25-33`) e configura PostgreSQL (`config/settings/ci.py:6-17`).

Execuções observadas, sem alterações de arquivo ou banco:

- `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_reverter_regen_sem_banco ponto.tests.test_reverter_exportada_sem_banco -v`: **36 OK**, 9,824 s. Contratos sem Django: `ponto/tests/test_reverter_regen_sem_banco.py:1-5`; I/O mockado no segundo: `ponto/tests/test_reverter_exportada_sem_banco.py:157-159`.
- `core/tests/test_medida_turno_do_par.py`: **35 OK**, 41,107 s, usando exatamente a adaptação documentada em `core/tests/test_medida_turno_do_par.py:20-68` (`SimpleTestCase` → `unittest.TestCase` em memória).
- `escala/tests/test_chokepoint_escala_gate.py` + `colaboradores/tests/test_chokepoint_cadastro_gate.py`: **12 OK**, 9,980 s, mesma substituição de base em memória; corpos originais preservados (`escala/tests/test_chokepoint_escala_gate.py:53-81`, `colaboradores/tests/test_chokepoint_cadastro_gate.py:50-90`).
- Cinco métodos puramente estáticos de `ponto/tests/test_chokepoint_celula_gate.py`: **5 OK**, 3,387 s (`:50-61`, `:87-97`). Os dois métodos que importam a porta real não foram executados (`:63-85`) [nao verificado].

Padrão para testes novos sem banco: compilar função real por AST com namespace controlado e trocar somente imports necessários; exemplo existente em `colaboradores/tests/test_ui_painel_mostra_o_app.py:74-98`. Helper de substituição reversível de módulos em `:25-42`; mutação de fonte somente em memória em `colaboradores/tests/ui_painel_fontes.py:38-53`.

Nenhum arquivo foi editado.

## 12/11 09:10 · sessao d20c4f297a46 · - · - · origem=Ronald (tmux)

| Caminho | Classificação | Evidência e encaixe |
|---|---|---|
| Feriado salvo/editado/excluído | **E** | Trabalho calculado consulta feriado do lugar do vínculo; alteração pode mudar `trabalha` futuro. (`escala/models.py:1369`, `escala/models.py:1394`) Hoje o receiver só reavalia chamados da data nova; exclusão não tem receiver. (`chamados/signals.py:322`, `chamados/signals.py:326`) Capturar data anterior no `pre_save`; regenerar futuras existentes nas datas anterior/nova no `post_save` e na data excluída no `post_delete`, pela porta comum e sem estender horizonte. |
| `Posto.opera_em_feriado` | **N**, para trabalho/geradora/DNA das células existentes | A flag do posto define somente o padrão ao **criar vínculo**; vínculo existente guarda sua própria `trabalha_em_feriado`. (`escala/models.py:904`, `escala/models.py:910`) O cálculo da célula lê essa flag do vínculo e o município, sem ler `opera_em_feriado`. (`escala/models.py:1389`, `escala/models.py:1394`) Há outro leitor da flag para cobrança do posto, fora desses campos. (`colaboradores/services/feriado_do_posto.py:60`) |
| `reclassificar_base` | **N**, para trabalho/geradora/DNA | Salva exclusivamente `tipo_base`. (`escala/services/cadastro_tipo.py:133`) Trabalho é calculado por ciclo/foto/âncora/feriado, e os marcos usam horários, overrides e modo de intervalo. (`escala/models.py:816`, `escala/models.py:1271`, `escala/models.py:326`) O produtor de DNA não inclui nem consulta `tipo_base`. (`ponto/management/commands/gerar_celulas.py:83`) Isso não prova ausência de efeitos no motor financeiro, que permanece fora desta classificação. (`ponto/motor_calculo_v2.py:3647`) |
| `TipoEscalaAdmin` | **E** | É `ModelAdmin` simples, sem encaminhamento de gravação/propagação. (`escala/admin.py:8`) Encaminhar `save_model` à porta de cadastro de template, preservando campos do admin; chamar apenas `super().save_model` mantém o buraco. A propagação existente fica em `_propagar_regeneracao_template`. (`escala/services/cadastro_tipo.py:327`) |
| `trocar_calendario_de_folga` | **E**, para tornar a porta autossuficiente | Atualmente salva somente FK e trilha; quem materializa é a view individual ou o lote. (`colaboradores/services/vinculo.py:434`, `colaboradores/views.py:239`, `colaboradores/views.py:245`, `escala/views.py:408`) O cálculo do dia usa a foto `FolgaDia`, não a FK. (`escala/models.py:1280`, `escala/models.py:1310`) |
| `salvar_calendario_de_folga` | **E**, para aplicar a edição às futuras existentes | Grava definição/tamanhos e retorna sem materializar foto; `semana_dia` tem retorno precoce. (`escala/folgas.py:548`, `escala/folgas.py:566`, `escala/folgas.py:574`) Reutilizar `datas_que_o_calendario_manda`. (`escala/folgas.py:43`) |

Para os dois calendários, o suplemento deve reconciliar apenas fotos futuras da janela com células existentes, preservando `FolgaDia` manual, e regenerar pela porta comum com `estender_horizonte=False`. Chamar `gerar_foto_folgas` de mês inteiro ampliaria a escrita sobre o passado: ela apaga/recria as fotos do mês e relavra o mês inteiro. (`escala/folgas.py:175`, `escala/folgas.py:180`, `escala/folgas.py:201`)

REDs sugeridos sem banco: feriado criado/removido/movido com células existentes nas duas datas; admin mudando horário com vínculo que começa amanhã; troca e edição de calendário movendo a folga de amanhã, preservando manual e célula editada. A execução desses testes permanece **[nao verificado]**. Nenhum arquivo foi editado. **[nao verificado por diff]** [nao verificado]

## 12/11 09:04 · sessao 100e569d9f3b · FIO-E0-V4-E3-LEITORES · censo · origem=Code

**Brief FIO-E0-V4-E3-LEITORES — somente censo.** Base informada: `087fe758a0488739ab22885a2f090b312b658a92`; identidade Git e pouso da E4B nesta cópia: [nao verificado]. As referências abaixo são da árvore lida; os anexos transcritos no pedido não foram conferidos contra arquivos originais [nao verificado].

**0. Portão de arquivo**

Há dois resultados distintos; o construtor precisa consumir o ampliado, porque o censo encontrou leitores além dos nomes iniciais. O calendário decide a oferta de “Resolver dia”; o copiloto publica vitalidade e retenção do dia. (`app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:476`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:101`)

| Recorte comparado com LASTRO fornecido | Arquivos em comum |
|---|---|
| Recorte nominal: seis caminhos MUDA, cinco respondedores, complemento 11–12 e testes identificados abaixo | **0 em comum**; estado da obra LASTRO [nao verificado]. | [nao verificado]
| Censo ampliado dos leitores de dia e testes das mesmas funções | `app/colaboradores/services/calendario.py`; `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py`; `app/ponto/services/dia_do_colab.py`; `app/core/tests/test_medida_turno_do_par.py`. Presença na lista LASTRO: anexo fornecido [nao verificado]. |
| Registro necessário ao PRONTO 1 | `app/docs/RELATO.md`, também listado no LASTRO fornecido [nao verificado]. |
| Ambos os recortes × caminhos de `bin/` | **0 em comum**. As referências encontradas em `bin/` descrevem o consumo do censo; não são os leitores de negócio listados aqui. (`bin/fio_estado.py:44`, `bin/tests/test_fio_estado.sh:38`) |

Lastro dos quatro arquivos do recorte ampliado:

- `app/colaboradores/services/calendario.py::contexto_calendario`: lê C6 e constrói `pode_ultimo_recurso`; isso participa da porta administrativa do dia. (`app/colaboradores/services/calendario.py:468`, `app/colaboradores/services/calendario.py:476`, `app/colaboradores/services/calendario.py:738`)
- `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py`: importa e exercita `contexto_calendario`; é teste do mesmo serviço, embora sua pergunta específica seja ausência/status. (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:14`, `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:43`)
- `app/ponto/services/dia_do_colab.py::dia_do_colab` e `::_pna_do_dia`: expõem C6 e `pergunta_no_app.do_dia` ao copiloto. (`app/ponto/services/dia_do_colab.py:62`, `app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:107`)
- `app/core/tests/test_medida_turno_do_par.py`: contém o sítio `tarefa_do_dia.py::instancias_do_turno` no universo da medição. Isso é acoplamento de teste, não demonstração de que esse arquivo precise ser editado. (`app/core/tests/test_medida_turno_do_par.py:380`)

**Decisão de portão:** não usar o “0” nominal como autorização para um diff que inclua esses arquivos adicionais. A colisão de escrita efetiva depende do diff final, ainda [nao verificado]. O aval fornecido determina espera/serialização quando houver arquivo em comum [nao verificado].

**1. Censo por AST e leitura dos sítios**

O script ao fim percorre Python sem importar a aplicação, resolve chamadas, aliases e reexports estáticos, imprime os chamadores e aponta operações candidatas a escrita. Reflexão dinâmica e cobertura completa de efeitos transitivos permanecem [nao verificado].

| Autoridade/sítio | O que responde; origem da resposta |
|---|---|
| `app/chamados/services/tarefa_do_dia.py:562::tarefa_do_dia` | Juiz do dia: ações por instância, dono, porta e desconhecimento explícito; delega à carga. (`app/chamados/services/tarefa_do_dia.py:32`, `app/chamados/services/tarefa_do_dia.py:564`, `app/chamados/services/tarefa_do_dia.py:572`) |
| `app/chamados/services/tarefa_do_dia.py:515::CargaDaTarefa.tarefa` | Calcula pelo corpo do mesmo juiz e acrescenta portas/verbos carregados do painel. (`app/chamados/services/tarefa_do_dia.py:523`, `app/chamados/services/tarefa_do_dia.py:528`, `app/chamados/services/tarefa_do_dia.py:532`) |
| `app/chamados/services/tarefa_do_dia.py:164::_tarefa_pura` | Decide exigibilidade lendo célula, lâmpada, teto, cobertura, decisões e efeitos; não é leitor externo. (`app/chamados/services/tarefa_do_dia.py:168`, `app/chamados/services/tarefa_do_dia.py:185`, `app/chamados/services/tarefa_do_dia.py:260`, `app/chamados/services/tarefa_do_dia.py:306`) |
| `app/chamados/services/tarefa_do_dia.py:538::_portas_da_tarefa` | Projeta portas das ações já julgadas; fila residual não cria tarefa. (`app/chamados/services/tarefa_do_dia.py:541`, `app/chamados/services/tarefa_do_dia.py:545`, `app/chamados/services/tarefa_do_dia.py:553`) |
| `app/chamados/juizes.py:1612::porta_alcancavel_do_dia` — C17 | Responde alcançabilidade por fila ou pergunta viva no app; deriva essa resposta sem ler `tarefa_do_dia`. (`app/chamados/juizes.py:1615`, `app/chamados/juizes.py:1646`, `app/chamados/juizes.py:1657`) |
| `app/chamados/juizes.py:862::pergunta_viva` — C6 | Vitalidade de uma pergunta; usa o corpo `_pergunta_viva_calc` e memo por instância. Não responde sozinho a exigibilidade completa do dia. (`app/chamados/juizes.py:863`, `app/chamados/juizes.py:889`, `app/chamados/juizes.py:1389`; `app/core/juizes.py:112`) |
| `app/chamados/juizes.py:1494::fio_vivo` — C15 | Redução de C6 sobre a coleção recebida; o universo pertence ao chamador. (`app/chamados/juizes.py:1499`, `app/chamados/juizes.py:1500`, `app/chamados/juizes.py:1503`) |
| `app/chamados/models.py:3366::PerguntaDisputa.viva` | Property que delega a C6; o SQL `q_vivas` é apenas pré-filtro generoso. (`app/chamados/models.py:3373`, `app/chamados/models.py:3376`) |
| `app/chamados/models.py:3035::PerguntaDisputa.estado` | Rótulo que combina carimbos e C6; não lê tarefa. (`app/chamados/models.py:3042`, `app/chamados/models.py:3045`) |

**Escritores de tarefa:** o contrato é **0 escritores persistentes**; a função não possui tabela de estado de tarefa. As operações `update` encontradas no módulo alteram os dicionários locais de saída, não registros ORM. Isso não certifica ausência de escrita em toda dependência transitiva. (`app/docs/JUIZES.md:84`; `app/chamados/services/tarefa_do_dia.py:182`, `app/chamados/services/tarefa_do_dia.py:205`, `app/chamados/services/tarefa_do_dia.py:249`, `app/chamados/services/tarefa_do_dia.py:333`, `app/chamados/services/tarefa_do_dia.py:553`)

Os consumidores atuais de produção de `tarefa_do_dia` encontrados são:

| Arquivo::função | Leitura |
|---|---|
| `app/chamados/management/commands/backfill_fio_morto_e4b.py::ato_do_dia` | Lê tarefa para planejar atos do passivo; não é escritor de tarefa. (`app/chamados/management/commands/backfill_fio_morto_e4b.py:82`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:87`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:97`) |
| `app/chamados/management/commands/diff_e3_sombra.py::Command.comparar` | Lê carga e tarefa para comparar projeções dos três consumidores C17. (`app/chamados/management/commands/diff_e3_sombra.py:115`, `app/chamados/management/commands/diff_e3_sombra.py:125`, `app/chamados/management/commands/diff_e3_sombra.py:127`) |

As chamadas diretas de teste à função estão em `test_e2_tarefa_do_dia`, `test_e3_c8c_orm` e `test_e3_portas_tarefa`; o script também imprime chamadas à carga e candidatos ao método `.tarefa`, incluindo wrappers de teste. (`app/chamados/tests/test_e2_tarefa_do_dia.py:139`, `app/chamados/tests/test_e2_tarefa_do_dia.py:321`, `app/chamados/tests/test_e3_c8c_orm.py:45`, `app/chamados/tests/test_e3_portas_tarefa.py:19`)

Os **três consumidores de produção de C17** são exatamente os três abaixo; a catraca atual exige sua permanência e deverá mudar junto com a migração. (`app/chamados/tests/test_e3_pendentes_app.py:19`, `app/chamados/tests/test_e3_pendentes_app.py:145`)

| Caminho MUDA/adicional | Sítio; saída atual | Derivação atual |
|---|---|---|
| Fio | `app/chamados/services/fio.py::montar_chamados_fio`: cartões, formulários, explicação, rótulos | C6, crédito, projeção de regularização e desenlace; não tarefa. (`app/chamados/services/fio.py:163`, `app/chamados/services/fio.py:238`, `app/chamados/services/fio.py:375`, `app/chamados/services/fio.py:390`) |
| Tela admin — fio/modal | `app/chamados/views.py::fio_colaborador`; `::modal_fio_colaborador._resp_modal_full` | Consomem a montagem acima; modal habilita explicação. (`app/chamados/views.py:677`, `app/chamados/views.py:889`) |
| Tela admin — drawer/ficha | `app/colaboradores/views.py::painel_colaborador`; `app/colaboradores/services/ficha.py::ficha_do_colab` | Drawer lê projeção do app; ficha deriva dono pelo verbo e pendência por texto/respostas sem veredito. (`app/colaboradores/views.py:940`, `app/colaboradores/services/ficha.py:154`, `app/colaboradores/services/ficha.py:159`, `app/colaboradores/services/ficha.py:169`) |
| Tela admin — calendário | `app/colaboradores/services/calendario.py::contexto_calendario`; `::resolver_dia_oferecido` | Deriva oferta de ação com janela, resposta, C6 e status do dia. (`app/colaboradores/services/calendario.py:97`, `app/colaboradores/services/calendario.py:467`, `app/colaboradores/services/calendario.py:738`) |
| API/app | `app/api/views.py::api_chamados_v2`; `::api_disputa_perguntas` | Leem `consulta_do_app`, não tarefa. (`app/api/views.py:992`, `app/api/views.py:2530`) |
| Badge dos dois kernels | `app/api/views.py::api_me`; `app/api/views_core.py::api_me` | Leem `projecao_do_app`. (`app/api/views.py:315`, `app/api/views_core.py:303`) |
| Badge/PWA dashboard | `app/core/views.py::dashboard` | Conta pendentes da consulta do app; template desenha essa contagem. (`app/core/views.py:252`, `app/templates/core/dashboard_colaborador_v2.html:78`) |
| Badge da navbar do colaborador | `app/core/context_processors.py::navbar_context._badge_colab` | Conta chamados na fila, unidade diferente das perguntas pendentes dos kernels. (`app/core/context_processors.py:185`, `app/core/context_processors.py:187`) |
| Fonte compartilhada API/badge | `app/chamados/services/pergunta_no_app.py::perguntas_do_app`; `::consulta_do_app`; `::projecao_do_app` | C6 → carimbo sem resposta + disputa aberta → contagem; não tarefa. (`app/chamados/services/pergunta_no_app.py:58`, `app/chamados/services/pergunta_no_app.py:71`, `app/chamados/services/pergunta_no_app.py:90`) |
| `o_que_segura` | `app/folha/fora_da_folha.py::o_que_segura` | Lê `_fio_do_dia`/`_porta`, que chegam a C17. (`app/folha/fora_da_folha.py:203`, `app/folha/fora_da_folha.py:221`, `app/folha/fora_da_folha.py:225`) |
| Fora da folha | `app/folha/fora_da_folha.py::_porta`; `::_fio_do_dia`; `::fora_da_folha` | C17 em `:67`; fio vem da resposta dessa porta. (`app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:77`, `app/folha/fora_da_folha.py:178`) |
| Cadastro realidade | `app/escala/services/cadastro_realidade.py::beco_em_lote` | Lê C17 e examina vivas/âncoras/cadastro; não tarefa. (`app/escala/services/cadastro_realidade.py:442`, `app/escala/services/cadastro_realidade.py:487`, `app/escala/services/cadastro_realidade.py:488`) |
| Vigia/contador | `app/chamados/services/vigia_fio.py::censo_alcance` | C17 em `:635`; depois decide futuro, baldes e causas até contar fio morto. (`app/chamados/services/vigia_fio.py:542`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/services/vigia_fio.py:648`, `app/chamados/services/vigia_fio.py:654`, `app/chamados/services/vigia_fio.py:695`) |
| Vigia/cron | `app/chamados/management/commands/vigiar_fio_morto.py::Command.handle` | Executa os censos; é consumidor e publicador da medição. (`app/chamados/management/commands/vigiar_fio_morto.py:174`, `app/chamados/management/commands/vigiar_fio_morto.py:176`; `app/config/crons.py:1721`) |
| Copiloto | `app/ponto/services/dia_do_colab.py::dia_do_colab`; `::_pna_do_dia` | Publica C6 e retenção própria de `pergunta_no_app.do_dia`. (`app/ponto/services/dia_do_colab.py:71`, `app/ponto/services/dia_do_colab.py:101`, `app/ponto/services/dia_do_colab.py:108`) |

Há ainda um caminho indireto de contador administrativo: navbar → partição de verbos/crédito, e uma lavra por comando. Migrar a fonte exige conferir também essa testemunha, sem executar sua escrita nesta obra de censo. (`app/core/context_processors.py:84`, `app/core/context_processors.py:90`, `app/core/management/commands/lavrar_badge_navbar.py:63`, `app/core/management/commands/lavrar_badge_navbar.py:91`)

**Cinco respondedores: origem e destino da certidão proposta**

| Respondedor atual | Quem chama | Para onde deve ir a pergunta sobre o dia |
|---|---|---|
| `fio.py::montar_chamados_fio`, ramo `todas_respondidas` | Atendimento, fio admin, modal e parcial. (`app/chamados/views.py:569`, `app/chamados/views.py:677`, `app/chamados/views.py:889`, `app/chamados/services/fio.py:446`) | Projeção de C19 para tarefa; complemento 12 consulta C6/C15 para vitalidade. O ramo atual usa timestamp cru. (`app/chamados/services/fio.py:340`; `app/chamados/services/tarefa_do_dia.py:32`; `app/chamados/juizes.py:1503`) |
| `raiox.py::creditos_perguntas` | `particionar_verbos` e montagem do fio. (`app/chamados/services/verbos.py:34`, `app/chamados/services/fio.py:390`) | Consumidores da tarefa leem a decisão C19; retirar a derivação por `respondida_em` do papel de respondedor do dia. (`app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:139`; `app/docs/JUIZES.md:78`) |
| `relatorios_disputa.py::classificar` | `coletar`. (`app/chamados/relatorios_disputa.py:47`) | Categorias de espera projetam ações/donos C19; fechamento continua C7. Hoje classifica por C6 + timestamp. (`app/chamados/relatorios_disputa.py:18`, `app/chamados/relatorios_disputa.py:21`; `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:78`) |
| `cobranca.py::_calados` | `linhas_para_a_pauta`. (`app/chamados/services/cobranca.py:257`) | Pendência do colaborador vem de C19; canal e SLA continuam perguntas próprias. Hoje usa `q_vivas`, resposta nula e disputa aberta. (`app/chamados/services/cobranca.py:228`, `app/chamados/services/cobranca.py:243`, `app/chamados/services/cobranca.py:246`) |
| `_card_atendimento.html` | Duas listas do atendimento. (`app/templates/chamados/meu_atendimento.html:127`, `app/templates/chamados/meu_atendimento.html:144`) | Receber projeção pronta do servidor; retirar derivações de documento/formulário/ação do dia. (`app/templates/chamados/partials/_card_atendimento.html:65`, `app/templates/chamados/partials/_card_atendimento.html:77`, `app/templates/chamados/partials/_card_atendimento.html:203`; `app/docs/JUIZES.md:78`) |

A certidão remove o papel de respondedor e o código sem chamadores no mesmo commit; não significa apagar serviços inteiros nem C6. O registro histórico também usa “cinco respondedores” para outra enumeração de autoridades, que não deve ser confundida com os cinco sítios do aval. (`CLAUDE.md:191`; `app/core/juizes.py:153`; `app/chamados/services/tarefa_do_dia.py:491`)

**Testes a carregar no brief** [nao verificado]

| Frente | Testes existentes | [nao verificado]
|---|---|
| Tarefa, portas e DIFF | `app/chamados/tests/test_e2_tarefa_do_dia.py:139`; `app/chamados/tests/test_e3_portas_tarefa.py:32`; `app/chamados/tests/test_e3_diff_sombra.py:53`; `app/chamados/tests/test_e3_c8b_sem_banco.py:117`; `app/chamados/tests/test_e3_c8c_orm.py:45` |
| API/badge/catraca | `app/chamados/tests/test_e3_pendentes_app.py:145`; `app/chamados/tests/test_e3_app_orm.py:28`; `app/chamados/tests/test_e3_v4_orm.py:20`; `app/core/tests/test_dashboard_badge.py:40`; `app/api/tests/test_api_me_badge.py:36`; `app/core/tests/test_badge_navbar_consome_particao.py:1` |
| Fio e explicação | `app/chamados/tests/test_fio_explicacao.py:24`; `app/chamados/tests/test_selo_identidade_fio_explicacao.py:72`; `app/chamados/tests/test_fio_ancora_destino.py:58`; `app/chamados/tests/test_fio_le_o_juiz.py:102` |
| Relatório/cartão | `app/chamados/tests/test_relatorio_pendencias.py:35`; `app/chamados/tests/test_o318_card_nao_oferece_o_que_a_porta_recusa.py:103`; `app/chamados/tests/test_o318_dia_misto_projecao_por_marco.py:124` |
| Vigia/folha/cadastro | `app/chamados/tests/test_n1_alimentacao_unica.py:77`; `app/chamados/tests/test_o261e_f5_alcance.py:100`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:84`; `app/chamados/tests/test_o261_vigia_fio.py:799`; `app/folha/tests/test_haiku_fora_da_folha.py:65`; `app/escala/tests/test_f3_beco1_folga_na_lista.py:163` |
| Admin | `app/colaboradores/tests/test_selo_situacao_e_lampada.py:118`; `app/colaboradores/tests/test_ficha_do_colab.py:39`; calendário e medição do par citados no portão. |

**JS/SW e CLASSE A:** há service worker no caminho PWA: registro em `pwa.js`, interceptação de GET e estratégias de rede/cache em `sw.js`. O `badge` do push nesse arquivo é um ícone. Não foi demonstrada necessidade de modificar JS/SW para a migração do servidor [nao verificado]. Se o diff tocar esses arquivos, aplica-se a CLASSE A e o smoke nas duas cascas exigidos pelo aval fornecido [nao verificado]. (`app/static/pwa.js:14`, `app/static/sw.js:33`, `app/static/sw.js:45`, `app/static/sw.js:82`, `app/static/sw.js:94`)

**2. CASO-7445: confirmação e juiz correto**

**Item 11 confirmado no código.** O mapa é por cartão; o cache é por disputa; o preenchimento do cache recebe as perguntas do cartão corrente. O primeiro cartão com lista não vazia pode definir a explicação reutilizada nos demais. O explicador aceita a lista fornecida e usa `fio_vivo(ps)` para dizer “Tudo resolvido”. (`app/chamados/services/fio.py:240`, `app/chamados/services/fio.py:305`, `app/chamados/services/fio.py:312`; `app/chamados/services/explicador.py:121`, `app/chamados/services/explicador.py:137`, `app/chamados/services/explicador.py:140`)

O diff deve alimentar `por_que_nao_fecha` com **todas as perguntas da disputa**, compartilhando suas instâncias; chave e universo precisam coincidir. C15 já declara que o universo é responsabilidade do chamador. O RED 34390/disputa 7445/duas vivas é dado do anexo; reprodução nesta cópia [nao verificado]. (`app/chamados/juizes.py:1500`; `app/chamados/tests/test_selo_identidade_fio_explicacao.py:93`)

**Item 12 confirmado no código.** `todas_respondidas` é `all(p.respondida_em ...)`, e o modal usa esse resultado no aviso de fechamento sem anuência. Pergunta morta por célula pode, portanto, continuar sendo tratada como ausência de resposta nesse cálculo. (`app/chamados/services/fio.py:340`; `app/templates/chamados/partials/modal_fio.html:334`)

O aviso deve consultar a vitalidade por C6/C15 no universo do cartão; C7 continua decidindo se a disputa pode fechar, incluindo sua guarda de resposta sem veredito. Não basta substituir o cálculo por `not fio_vivo(...)`: pergunta viva respondida e pergunta viva sem resposta exigem projeções diferentes. O RED 35602/pergunta 51407/`celula_absolveu` vem do anexo; reprodução [nao verificado]. (`app/chamados/juizes.py:1407`, `app/chamados/juizes.py:1494`; `app/chamados/models.py:1850`, `app/chamados/models.py:1866`)

**Registro:** `JUIZES.md` registra C7 e C19 e menciona C6; não contém verbete independente com os endereços literais C6/C15. O próprio documento manda consultar o registro vivo, onde constam `pergunta_viva` e `fio_vivo`. (`app/docs/JUIZES.md:14`, `app/docs/JUIZES.md:62`, `app/docs/JUIZES.md:78`, `app/docs/JUIZES.md:90`; `app/core/juizes.py:278`, `app/core/juizes.py:287`)

**3. DECISÕES técnicas, sem juiz novo**

1. **Quem responde o dia:** C19 permanece a autoridade. C17 só pode virar fachada depois do DIFF; a regra já resolve os quatro conflitos históricos. (`app/docs/RELATO.md:515`, `app/docs/RELATO.md:518`)

2. **Fila sem pergunta, fila A/pergunta B, verbos A/B e isento residual:** conservar as respostas de C19 e medir a mudança nos consumidores. As expectativas já estão escritas; sua incidência na frota atual está [aguarda sonda] [nao verificado]. (`app/chamados/tests/test_e3_portas_tarefa.py:32`, `app/chamados

[truncado em 20000 de 40250 caracteres]

_4135 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
