# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 20:37** · publicado em **06/11/2026 20:37** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 20:37 · sessao b86b0bc41185 · L-123-MEDIDA-EM-PY · verificador · origem=Code

**Parecer: reprova a volta 2. Ainda passam mudanças de comportamento pelo juiz de `padrao`: há uma brecha de linha compartilhada e há decisões dentro dos contêineres declarados como dado.** (`bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:206`, `app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:676`)

1. **Caminhos de aceite e recusa**

O contraexemplo mais direto é alterar o fechamento de `RESULTADOS`, na linha 624, de `]` para `]; CADENCIA_TERMOMETRO_DIAS = 700`. O `Assign` de `RESULTADOS` torna a linha livre; a segunda atribuição torna a mesma linha presa; o retorno conserva a linha porque não subtrai `preso` de `livre`. A conferência dos hunks aceita essa linha nos dois lados. (`app/core/placar_estrutural.py:624`, `bin/tests/test_lei_akita.sh:200`, `bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:230`)

**Confirmei em memória**, usando as funções reais: a linha 624 permanece livre antes/depois; com `hoje=2026-11-06` e R1 medido em `2026-10-01`, o original retorna AMARELO e o alterado retorna VERDE. O vencimento consulta precisamente essa constante. (`bin/tests/test_lei_akita.sh:194`, `app/core/placar_estrutural.py:668`, `app/core/placar_estrutural.py:686`, `app/core/placar_estrutural.py:694`)

Outro caminho dispensa instrução executável: trocar o `papel` do R1 de `TERMOMETRO` para `PRINCIPAL`. Ambos são constantes-string elegíveis; a alteração mantém dado puro, retira R1 dos termômetros e permite chegar ao VERDE sem termômetro. Também confirmei AMARELO → VERDE em memória no cenário acima. (`app/core/placar_estrutural.py:61`, `app/core/placar_estrutural.py:121`, `bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:203`, `app/core/placar_estrutural.py:676`, `app/core/placar_estrutural.py:694`)

Na tabela, “aceita” significa concessão da exceção pelo juiz de `padrao`, satisfeitas as demais condições do arquivo. **Não significa régua inteira verde**: os testes L114 exigem R1 como único termômetro e verificam a borda de sete dias e as cores. (`bin/tests/test_lei_akita.sh:244`, `app/core/tests/test_l114_placar_papel_e_termometro.py:34`, `app/core/tests/test_l114_placar_papel_e_termometro.py:51`, `app/core/tests/test_l114_placar_papel_e_termometro.py:71`)

| Caso | Resultado |
|---|---|
| Constante-string com outro `Assign`, `AnnAssign` ou `AugAssign`, inclusive dentro de função com `global` | **Recusa o uso desse Name como dado puro**: a contagem deixa de ser um. O percurso alcança qualquer escopo. (`bin/tests/test_lei_akita.sh:151`, `bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:185`) |
| Constante-string também ligada por `for`, `with … as`, `import … as` ou walrus; ou removida por `del` | **Aceita indevidamente o Name como constante**, quando essas instruções já existem e só o contêiner muda: `_atribuicoes` não conta esses nós. Isso também afeta a suposta unicidade de `PAPEL_DO_MODULO` e `DADO_MEDIDO`. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:185`) |
| Acrescentar essas instruções em linhas próprias no commit julgado | **Recusa**: esses nós não são docstring nem `Assign` elegível. A linha compartilhada descrita acima continua sendo exceção indevida. (`bin/tests/test_lei_akita.sh:201`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:206`) |
| `R = X = [...]`, mesmo com ambos os nomes declarados | **Recusa**: exige exatamente um alvo. (`bin/tests/test_lei_akita.sh:202`) |
| `R[...] = ...` ou `R.attr = ...` em linha própria | **Recusa**: exige alvo `Name`. (`bin/tests/test_lei_akita.sh:202`) |
| Dict com chamada, lambda ou Name sem ser constante-string | **Recusa**: esses nós não passam em `_dado_puro`. (`bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:192`) |
| `AnnAssign` executável em linha própria | **Recusa**: somente `Assign` pode ser dado livre. (`bin/tests/test_lei_akita.sh:202`) |
| Chamada, import ou monkeypatch depois de `;` na linha de dado | **Aceita indevidamente**, pela sobreposição entre `livre` e `preso`. (`bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`) |
| Alterar `papel`, `estado`, `prova`, `id` ou `fonte_do_medido` dentro de `RESULTADOS` | **Aceita**, conservada a pureza sintática. Esses campos controlam classificação, rebaixamento, contagem e obtenção da data. (`bin/tests/test_lei_akita.sh:203`, `app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:650`, `app/core/placar_estrutural.py:700`, `bin/gerar_estado.py:47`, `bin/gerar_estado.py:56`) |
| Alterar `estado`/`prova` em `ETAPAS` | **Aceita**; pode alterar estado efetivo, feitos e percentual. (`app/core/espelho_verdade.py:209`, `app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:217`, `bin/tests/test_lei_akita.sh:203`) |
| Mudar ou remover chave de dict | **Aceita**, se continuar puro. Não há validação de esquema; leitores acessam diretamente chaves como `id`, `estado` e `itens`, podendo mudar resultado ou levantar exceção. (`bin/tests/test_lei_akita.sh:188`, `app/core/placar_estrutural.py:650`, `app/core/espelho_verdade.py:208`, `app/core/espelho_verdade.py:209`) |
| String contendo código | **Aceita como string**. Nas rotas rastreadas, a publicação usa templates fixos com `%`; interpretação por `eval` ou por template fornecido pelo dado: **[não verificado]**. (`bin/tests/test_lei_akita.sh:188`, `bin/gerar_estado.py:101`, `bin/gerar_estado.py:115`, `bin/gerar_estado.py:163`) |

Uso desses contêineres para decidir **dinheiro em produção**: **[não verificado]**. O uso para decidir cor, vencimento e classificação do placar está demonstrado nos leitores acima. (`app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:679`)

2. **Nomes lidos no ALVO**

A autorização vem do ALVO; a pureza e os intervalos vêm separadamente do pai e do commit julgado. Portanto, a retroatividade não transforma diretamente uma chamada dentro do valor em dado puro: esse valor continua sendo examinado na revisão histórica. (`bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:221`, `bin/tests/test_lei_akita.sh:223`)

**A garantia semântica, porém, é maior que a prova disponível.** O ALVO fornece nomes, sem comprovar que eles tinham função de medição no commit antigo. Não há restrição que impeça declarar uma constante de decisão como nome elegível; a validação exige apenas uma tupla não vazia de strings, seguida do teste sintático do `Assign`. Assim, um ALVO que liste `CADENCIA_TERMOMETRO_DIAS` pode autorizar retroativamente sua troca literal. Esse cenário decorre do predicado; sua ocorrência histórica nesta obra é **[não verificado]**. (`bin/tests/test_lei_akita.sh:169`, `bin/tests/test_lei_akita.sh:173`, `bin/tests/test_lei_akita.sh:202`, `app/core/placar_estrutural.py:668`)

As falhas de contagem de vínculos também permitem que a declaração sintática encontrada pelo selo divirja do valor executado no ALVO. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:163`)

Merge continua filtrando caminhos iguais a pais publicados; os caminhos restantes são comparados com o primeiro pai e julgados dos dois lados. Rename usa `--no-renames`; origem ausente no ALVO recusa. Restauração posterior pode habilitar uma exclusão histórica composta só de linhas livres, conforme a retroatividade estabelecida. (`bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:217`, `bin/tests/test_lei_akita.sh:230`, `bin/tests/test_lei_akita.sh:296`, `bin/tests/test_lei_akita.sh:303`, `bin/tests/test_lei_akita.sh:310`)

3. **O MORDE não morde cada cláusula nova**

O mapeamento abaixo é **por inspeção dos casos**, não resultado de executar versões mutantes. Os casos chamam as funções reais e o shell rejeita qualquer `MORDE_FALHOU`. (`bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:569`)

| Caso existente | Mutante que distingue |
|---|---|
| `so_numero`, `estado`, `comentario`, `doc` | Desligar a concessão; proibir constante-string; retirar liberação de comentário/docstring. (`bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:406`) |
| `na_def` | Liberar corpo de função. (`bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:407`) |
| `tira_def` | Examinar somente o lado novo. (`bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:407`) |
| `com_import`, `com_lambda`, `com_chamada`, `com_nome` | Liberar, respectivamente, import, lambda, chamada ou Name não constante-string. (`bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:408`) |
| `decisao` | Liberar atribuição fora dos nomes declarados. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:409`) |
| `monkey` | Liberar atribuição a atributo em linha própria. (`bin/tests/test_lei_akita.sh:399`, `bin/tests/test_lei_akita.sh:409`) |
| `sem_decl`, `mais_fech`, rótulo instrumento | Dispensar papel `medida`, ignorar outro arquivo de negócio ou estender a exceção a instrumento. (`bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`, `bin/tests/test_lei_akita.sh:411`) |
| ALVO sem declaração | Ler declaração exclusivamente em `h`. (`bin/tests/test_lei_akita.sh:417`, `bin/tests/test_lei_akita.sh:418`) |
| Retroativo número/decisão | Exigir declaração em `h`, ou liberar decisão fora do contêiner. (`bin/tests/test_lei_akita.sh:423`, `bin/tests/test_lei_akita.sh:431`) |

**`redecl` não prova a unicidade no ALVO.** O caso acrescenta outra declaração em `h`, mas o preparo restaura HEAD à base antes de julgá-lo. Ele já reprova pela linha presa; retirar a checagem de unicidade no ALVO não é distinguido por esse caso. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`)

Faltam casos para: vínculos por `for`/`with`/import/walrus/`del`; reatribuição de constante-string; duplicação de ambos os metadados no ALVO; múltiplos alvos; Subscript; AnnAssign; instrução presa na mesma linha livre; DADO inválido/aninhado; classe/decorador; comentário dentro de função; docstring posterior; diff sem hunk; e dado puro usado como decisão. A matriz atual não planta esses cenários. (`bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:412`)

Os testes antigos cobrem merge/rename com `n/a (instrumento)`, mas não a elegibilidade AST de medida nesses cenários. Os casos novos de medida passam mensagem e arquivos manualmente; seus commits usam mensagem `c`, sem exercitar essa integração pelo universo. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:414`, `bin/tests/test_lei_akita.sh:480`, `bin/tests/test_lei_akita.sh:513`, `bin/tests/test_lei_akita.sh:525`)

Há ainda divergência no limite de erro: `show`/`diff` emitem `PADRAO_ERRO`, mas qualquer erro de `cat-file` vira AST ausente e qualquer erro da sonda de pai vira `False`. Recusam a concessão, mas não cumprem literalmente “erro Git = PADRAO_ERRO”. (`bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:219`)

Execução integral do MORDE e dos dois mutantes vermelhos prometidos: **[não verificado]**. Não executei o shell, cujo preparo cria arquivos e repositórios temporários. (`bin/tests/test_lei_akita.sh:362`, `bin/tests/test_lei_akita.sh:374`, `bin/tests/test_lei_akita.sh:376`)

4. **Os três R1**

`36c90132`, `6ed6adbe` e `a16637ca` tocarem somente linhas dentro de `RESULTADOS`/`ETAPAS`: **[não verificado]**. O histórico Git e os diffs individuais não estão disponíveis nesta cópia **[não verificado]**.

O relato cita as origens, um comparativo de três arquivos e as correções, mas não apresenta os hunks desses commits. Isso não sustenta responder “sim”. (`app/docs/RELATO.md:864`, `app/docs/RELATO.md:866`, `app/docs/RELATO.md:869`)

5. **Quem escreve, quem lê e ESMERIL-SEMÂNTICA**

| Símbolo | Escritores/declarações | Leitores |
|---|---|---|
| `PAPEL_DO_MODULO` | Dois módulos; fixture e alterações do MORDE. (`app/core/placar_estrutural.py:38`, `app/core/espelho_verdade.py:30`, `bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:429`) | `_dado_medido`, por AST do ALVO. (`bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:209`) |
| `DADO_MEDIDO` | Dois módulos e fixture MORDE. (`app/core/placar_estrutural.py:39`, `app/core/espelho_verdade.py:31`, `bin/tests/test_lei_akita.sh:370`) | `_dado_medido` extrai nomes; `_linhas_livres` usa-os para liberar atribuições. (`bin/tests/test_lei_akita.sh:173`, `bin/tests/test_lei_akita.sh:203`) |
| `RESULTADOS` | Definição de topo; teste L114 reatribui e restaura. Responsabilidade declarada: “quem mede”. (`app/core/placar_estrutural.py:28`, `app/core/placar_estrutural.py:71`, `app/core/tests/test_l114_placar_papel_e_termometro.py:117`, `app/core/tests/test_l114_placar_papel_e_termometro.py:123`) | `placar`; selo de afirmações fora do ar; testes L114, R1 e R6. (`app/core/placar_estrutural.py:644`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:83`, `app/core/tests/test_l114_placar_papel_e_termometro.py:96`, `app/core/tests/test_r1_termometro_uma_medicao.py:41`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`) |
| `ETAPAS` | Definição de topo. Responsabilidade declarada: “quem fecha a fatia”. (`app/core/espelho_verdade.py:19`, `app/core/espelho_verdade.py:35`) | `placar` e teste de costura R1. (`app/core/espelho_verdade.py:206`, `app/core/tests/test_r1_termometro_uma_medicao.py:45`) |

`gerar_estado` executa os módulos, chama as funções e publica suas projeções. O handoff também lê o texto bruto de `placar_estrutural.py` por regex. (`bin/gerar_estado.py:77`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:84`, `bin/gerar_estado.py:144`, `bin/gerar_estado.py:145`, `bin/handoff_sessao.sh:179`)

**“Medida/dado” não mantém o mesmo alcance.** Nos comentários, promete número medido e prova; no selo, significa expressão sintaticamente pura dentro de nome autorizado; nos leitores, inclui parâmetros de classificação, descoberta da data, rebaixamento e percentual. `fonte_do_medido`, por exemplo, é um glob operacional, não apenas texto publicado. (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`, `bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:203`, `bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `app/core/espelho_verdade.py:211`)

6. **Classes de reprovação**

- **Segundo juiz:** o fluxo analisado conserva `julgar` e usa um predicado de elegibilidade no mesmo caminho; não demonstra um segundo juiz de negócio. (`bin/tests/test_lei_akita.sh:253`, `bin/tests/test_lei_akita.sh:311`)
- **Derivação paralela:** o selo examina sintaxe e linhas; não introduz nesse fluxo outro cálculo monetário ou de vencimento. Uma duplicação fora desse escopo é **[não verificado]**. (`bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:207`)
- **Selo sem mordida:** há mordida para os casos plantados, mas falta discriminar as brechas demonstradas e a unicidade no ALVO. (`bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:414`)
- **Campo com dois significados / ESMERIL-SEMÂNTICA: reprova.** A autorização chamada “dado medido” alcança conteúdo que decide comportamento, e a classificação por linha ainda libera instrução executável colada ao dado. (`bin/tests/test_lei_akita.sh:203`, `bin/tests/test_lei_akita.sh:206`, `app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:676`)

## 06/11 20:37 · sessao 325c532adcd0 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**A V4 corrige a reprovação estática do selo:** a avaliação em memória encontrou `novos=[]` e `curados=[]`, satisfazendo os dois testes perguntados (`app/core/tests/test_selo_teste_sem_relogio.py:67`, `:72`). A garantia de execução dos hooks congelados e dos REDs contra os pais permanece **[nao verificado]**.

1. **Selo: os dois ficam verdes na avaliação estática.**

   | Arquivo de teste | `le_relogio_solto` | Em `soltos()` | Em `lista()` | [nao verificado]
   |---|---:|---|---|
   | API original | 11 | Sim | Sim |
   | API novo | 0 | Não | Não |
   | A2 original | 2 | Sim | Sim |
   | A2 novo | 0 | Não | Não |

   Os valores foram calculados pelas funções reais do selo, em memória. O detector conta chamadas por AST e considera qualquer ocorrência de `freeze_time` suficiente para retornar zero (`app/core/tests/test_selo_teste_sem_relogio.py:28`, `:32`, `:58`, `:62`). Os originais continuam lendo relógio e listados; os novos têm decoração de classe (`app/api/tests/test_api_bater_ponto.py:222`; `app/chamados/tests/test_a2_data_evento_batida.py:31`, `:48`; `app/core/tests/testes_sem_relogio.txt:5`, `:26`; `app/api/tests/test_api_bater_sse_sem_dia.py:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`).

   Na cópia inteira, `soltos()` e `lista()` têm as mesmas 458 entradas. Os três métodos do selo passaram usando `unittest.TestCase` como substituto da base Django, sem executar Django ou acessar banco (`app/core/tests/test_selo_teste_sem_relogio.py:66`). Igualdade byte a byte com a lista de `8050df3f`: **[nao verificado]**.

2. **Congelamento: a posição agora é adequada; cobertura efetiva dos hooks não foi comprovada aqui.**

   O decorador envolve a classe que contém `setUpTestData` na API e a classe que contém `setUp` no A2, além dos respectivos corpos (`app/api/tests/test_api_bater_sse_sem_dia.py:19`, `:22`, `:33`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:19`, `:25`).

   O comportamento esperado do freezegun para `unittest.TestCase` é cobrir `setUpClass` — incluindo `setUpTestData` —, `setUp` e corpo; essa implementação das dependências não está disponível na cópia: **[nao verificado]**. As versões estão fixadas em Django 5.2.14 e freezegun 1.5.5 (`app/requirements.txt:13`, `:43`).

   Os campos automáticos relevantes chamam-se `criado_em` e usam `auto_now_add`; receberem efetivamente o instante congelado depende daquela cobertura **[nao verificado]** (`app/colaboradores/models.py:380`; `app/ponto/models.py:79`). “Arquivo inteiro congelado” também não significa importação congelada: os imports antecedem a decoração (`app/api/tests/test_api_bater_sse_sem_dia.py:9`, `:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:8`, `:17`).

3. **Mordida preservada dedutivamente; fixtures equivalentes, mas não literalmente iguais.**

   **API:** mantém patch retornando `None`, URL `/api/ponto/bater/`, S três horas retroativa e os três asserts (`app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:40`, `:41`, `:45`). O patch alcança os imports locais dos dois gêmeos; o código atual trata `None` antes de serializar (`app/api/views.py:776`, `:778`; `app/api/views_core.py:776`, `:778`). Contra o pai descrito com `None.isoformat()`, a mordida continua dedutiva; objeto e execução contra `39324b76`: **[nao verificado]**. O teste simula a resposta `None`, sem construir uma retratação real (`app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:37`).

   **A2:** conserva ID sem linha, timestamp de agosto e os asserts que exigem outro dia, igual ao dia corrente congelado (`app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:28`, `:32`). Hoje a busca exige PK e colaborador; sem linha, o juiz retorna `None`, permitindo a conversa datada por `localdate()` (`app/chamados/services/disputa_emissao.py:379`, `:417`; `app/ponto/turnos.py:2921`; `app/chamados/models.py:2426`). Um pai que derive diretamente do timestamp falharia no primeiro assert; implementação e execução contra `8050df3f`: **[nao verificado]**. A2 chama o helper diretamente, sem patch nem URL (`app/chamados/tests/test_a2_batida_sem_linha.py:30`).

   Os rótulos mudaram e o username API também mudou na cópia; os campos funcionais das fixtures foram preservados (`app/api/tests/test_api_bater_ponto.py:199`, `:206`; `app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:26`; `app/chamados/tests/test_a2_data_evento_batida.py:11`; `app/chamados/tests/test_a2_batida_sem_linha.py:19`). A repetição entre classes não deve colidir sob o isolamento esperado de `TestCase`; ambas usam essa base, mas seu rollback efetivo aqui é **[nao verificado]** (`app/api/tests/test_api_bater_sse_sem_dia.py:20`; `app/chamados/tests/test_a2_batida_sem_linha.py:18`).

4. **Não identifiquei contrato que quebre pelos novos arquivos ou nomes de classe.**

   O runner delega a descoberta ao `DiscoverRunner`; o CRON-15 só recolhe métodos com sua marca; o smoke de portas agrega URLs dos arquivos de teste (`app/core/runner_quarentena.py:72`, `:76`; `app/core/tests/test_marca_cron15.py:37`, `:41`; `app/core/tests/test_contract_smoke_porta.py:48`). Em memória, o censo CRON-15 continuou exatamente 15/15 e o contrato de smoke passou (`app/core/tests/test_marca_cron15.py:48`; `app/core/tests/test_contract_smoke_porta.py:38`). O contrato de imports também varre testes, mas verifica nomes importados, sem exigir as classes antigas (`app/core/tests/test_contract_import_local_resolve.py:62`, `:71`, `:81`). Suíte completa: **[nao verificado]**.

5. **Classes que já reprovaram a obra.**

   - **Selo/RED sem mordida:** a falsa “cura” dos arquivos antigos foi eliminada; a mordida permanece dedutiva. O RED API ainda não exige que o mock tenha sido chamado (`app/core/tests/test_selo_teste_sem_relogio.py:34`, `:73`; `app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:45`).
   - **Relógio fora da borda:** as fixtures agora estão dentro das classes decoradas; os arquivos antigos continuam explicitamente soltos e listados. A cobertura efetiva dos hooks permanece **[nao verificado]** (`app/api/tests/test_api_bater_sse_sem_dia.py:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`; `app/core/tests/testes_sem_relogio.txt:5`, `:26`).
   - **Paridade dos gêmeos:** o contrato AST passou em memória; isso comprova igualdade estrutural, sem substituir duas execuções comportamentais (`app/api/tests/test_contract_paridade_kernels.py:26`, `:34`).

## 06/11 20:35 · sessao ce334d3b3633 · - · - · origem=Ronald (tmux)

- **API: mordida dedutiva preservada.** Patch, POST, tipo S retroativo três horas e asserts foram mantidos (`app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:41`, `:45`). O atraso cabe na janela de 48 h (`app/api/hora_do_aparelho.py:63`, `:141`) e evita a correção ao vivo de 300 s (`app/api/views_core.py:592`). Sem vínculo na fixture, a correção retroativa não entra (`app/api/tests/test_api_bater_sse_sem_dia.py:23`; `app/api/views_core.py:611`). O fallback sem batidas dá motivo (`app/escala/servico_jornada.py:469`, `:471`, `:545`), permitindo criar SSE (`app/api/views_core.py:649`). O patch alcança os imports locais nos dois gêmeos (`app/api/views.py:776`; `app/api/views_core.py:776`). Atual trata `None`; o pai descrito, com `None.isoformat()`, seria RED (`app/api/views_core.py:778`; `app/api/tests/test_api_bater_sse_sem_dia.py:36`). Objeto Git e execução contra `39324b76`: **[nao verificado]**.

- **A2: a mudança de arquivo preserva o contraexemplo.** Continua fornecendo ID sem linha, timestamp de agosto e exigindo data diferente de agosto e igual ao dia congelado de outubro (`app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:28`, `:32`). Atual busca a linha por PK e colaborador; sem linha, o helper retorna `None` (`app/chamados/services/disputa_emissao.py:379`; `app/ponto/turnos.py:2921`). O motivo é de escolha (`app/chamados/catalogo/perguntas.py:82`), permitindo `abrir_conversa`, que grava `localdate()` (`app/chamados/services/disputa_emissao.py:410`, `:417`; `app/chamados/models.py:2426`). Se o pai deriva diretamente do timestamp, falha no primeiro assert; implementação do pai e execução contra `8050df3f`: **[nao verificado]**.

- **Fixtures equivalentes funcionalmente, não literalmente iguais.** A API mudou rótulos e username, mantendo campos funcionais; CPF/CNPJ repetem (`app/api/tests/test_api_bater_ponto.py:199`, `:206`, `:207`; `app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:26`, `:27`). A2 mudou rótulos, mantendo os demais parâmetros (`app/chamados/tests/test_a2_data_evento_batida.py:12`; `app/chamados/tests/test_a2_batida_sem_linha.py:20`). Ambos herdam `TestCase`; garantia de isolamento depende da implementação Django indisponível, portanto **[nao verificado]** além da declaração de herança (`app/api/tests/test_api_bater_sse_sem_dia.py:20`; `app/chamados/tests/test_a2_batida_sem_linha.py:18`).

- **Limite do RED API:** não há `assert_called` no patch; a passagem efetiva pela guarda é deduzida do fluxo, não medida pelo teste (`app/api/tests/test_api_bater_sse_sem_dia.py:37`, `:45`). O teste simula retorno `None`, sem construir uma batida retratada real (`app/api/tests/test_api_bater_sse_sem_dia.py:23`, `:37`).

## 06/11 20:35 · sessao fa77ffc8e719 · - · - · origem=Ronald (tmux)

Achado principal: **o selo aceita alteração de decisão dentro de `RESULTADOS`**. Trocar somente `'papel': TERMOMETRO` por `PRINCIPAL` no R1 conserva dado puro, mas remove o único termômetro da seleção e pode trocar AMARELO por VERDE. (`app/core/placar_estrutural.py:121`, `app/core/placar_estrutural.py:61`, `bin/tests/test_lei_akita.sh:174`, `bin/tests/test_lei_akita.sh:202`, `app/core/placar_estrutural.py:672`, `app/core/placar_estrutural.py:676`, `app/core/placar_estrutural.py:686`, `app/core/placar_estrutural.py:694`)

Verifiquei a mudança **somente em memória**, executando AST original e AST com esse valor trocado: com `hoje=2026-11-06`, medição R1 `2026-10-01`, original retorna AMARELO/vencido; mutado retorna VERDE/em dia. A explicação está na classificação por `papel` e nas duas listas consultadas pelo veredito. (`app/core/placar_estrutural.py:651`, `app/core/placar_estrutural.py:676`, `app/core/placar_estrutural.py:686`, `app/core/placar_estrutural.py:690`, `app/core/placar_estrutural.py:694`)

**Limite:** esse contraexemplo passa pelo juiz de `padrao`; o teste existente L114 exige R1 como único termômetro e exige os dois resultados de cor, portanto não afirmar que passa pela régua inteira. (`bin/tests/test_lei_akita.sh:243`, `app/core/tests/test_l114_placar_papel_e_termometro.py:34`, `app/core/tests/test_l114_placar_papel_e_termometro.py:35`, `app/core/tests/test_l114_placar_papel_e_termometro.py:71`, `app/core/tests/test_l114_placar_papel_e_termometro.py:75`)

| Símbolo | Quem escreve | Quem lê |
|---|---|---|
| `PAPEL_DO_MODULO` | Declarações dos módulos; fixture e alterações do MORDE. (`app/core/placar_estrutural.py:38`, `app/core/espelho_verdade.py:30`, `bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:403`, `bin/tests/test_lei_akita.sh:429`) | `_dado_medido` via `_atribuicoes`, sintaticamente no AST do ALVO. (`bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:165`, `bin/tests/test_lei_akita.sh:209`) |
| `DADO_MEDIDO` | Declarações dos módulos e fixture MORDE. (`app/core/placar_estrutural.py:39`, `app/core/espelho_verdade.py:31`, `bin/tests/test_lei_akita.sh:370`) | `_dado_medido` extrai nomes; `_linhas_livres` confere alvo do `Assign`. (`bin/tests/test_lei_akita.sh:162`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:173`, `bin/tests/test_lei_akita.sh:203`) |
| `RESULTADOS` | Definição de topo; teste reatribui e restaura. A responsabilidade declarada é “quem mede”. (`app/core/placar_estrutural.py:28`, `app/core/placar_estrutural.py:71`, `app/core/tests/test_l114_placar_papel_e_termometro.py:117`, `app/core/tests/test_l114_placar_papel_e_termometro.py:123`) | `placar`; selo de afirmação fora do ar; testes L114, R1 e R6. (`app/core/placar_estrutural.py:644`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:83`, `app/core/tests/test_l114_placar_papel_e_termometro.py:36`, `app/core/tests/test_l114_placar_papel_e_termometro.py:96`, `app/core/tests/test_r1_termometro_uma_medicao.py:41`, `app/core/tests/test_selo_r6_frase_do_juiz.py:17`) |
| `ETAPAS` | Definição de topo; responsabilidade declarada é “quem fecha a fatia”. (`app/core/espelho_verdade.py:19`, `app/core/espelho_verdade.py:35`) | `placar`; teste de costura da mesma medição R1. (`app/core/espelho_verdade.py:206`, `app/core/tests/test_r1_termometro_uma_medicao.py:45`) |

Os consumidores indiretos executam os módulos e chamam seus leitores: `gerar_estado` usa `placar`, `linha_do_placar`, `veredito` e publica as duas projeções. O handoff também lê texto do arquivo por regex para extrair a frase `contratos_estruturais`. (`bin/gerar_estado.py:77`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:83`, `bin/gerar_estado.py:84`, `bin/gerar_estado.py:144`, `bin/gerar_estado.py:145`, `bin/handoff_sessao.sh:179`)

Outros controles inseridos nos contêineres:

- `papel` determina classificação e seleção de resultados; `estado` determina contagens; `numero`/`prova` determinam rebaixamento a PENDENTE. (`app/core/placar_estrutural.py:646`, `app/core/placar_estrutural.py:647`, `app/core/placar_estrutural.py:649`, `app/core/placar_estrutural.py:672`, `app/core/placar_estrutural.py:700`, `app/core/placar_estrutural.py:701`)
- `fonte_do_medido` é padrão de glob usado para ler nomes de arquivos e derivar a data; `id` indexa o mapa de datas; essa data determina vencimento. Não é apenas prosa de prova. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:52`, `bin/gerar_estado.py:56`, `app/core/placar_estrutural.py:650`, `app/core/placar_estrutural.py:668`)
- Em `ETAPAS`, `estado` e `prova` controlam rebaixamento e percentual; `itens`, `nome` e `porta` são chaves acessadas diretamente. Renomear chave ainda é sintaticamente puro, podendo mudar resultado ou causar exceção. (`app/core/espelho_verdade.py:208`, `app/core/espelho_verdade.py:209`, `app/core/espelho_verdade.py:211`, `app/core/espelho_verdade.py:214`, `app/core/espelho_verdade.py:215`, `app/core/espelho_verdade.py:217`)
- `fonte_do_medido` adulterado pode mudar o universo de arquivos consultados; demonstrar uma nova data real sem acesso aos logs é **[não verificado]**. (`bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`)
- Nos leitores rastreados, strings entram em templates fixos com `%`, não como expressão Python; não achei interpretação `eval`/`exec` nessas rotas. A ausência fora das rotas rastreadas é **[não verificado]**. (`bin/gerar_estado.py:101`, `bin/gerar_estado.py:115`, `bin/gerar_estado.py:153`, `bin/gerar_estado.py:163`, `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`)

**ESMERIL:** nos comentários dos módulos, “medida” promete número medido e texto de prova; no selo significa qualquer expressão pura atribuída ao contêiner nomeado; nos leitores, esses contêineres também fornecem parâmetros de classificação, descoberta da data, rebaixamento e percentual. Portanto pureza sintática não garante ausência de decisão. (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`, `bin/tests/test_lei_akita.sh:186`, `bin/tests/test_lei_akita.sh:202`, `app/core/placar_estrutural.py:651`, `bin/gerar_estado.py:47`, `app/core/espelho_verdade.py:211`)

## 06/11 20:35 · sessao 26d44a2e25e0 · - · - · origem=Ronald (tmux)

Não identifiquei contrato que quebre pela criação dos dois arquivos ou pelos novos nomes de classe; execução da suíte completa: [nao verificado]. [nao verificado]

| Consumidor | Efeito do delta |
|---|---|
| Descoberta da suíte | Labels abrangem `api` e `chamados`; o runner delega a descoberta ao `DiscoverRunner`, sem inventário fechado dos arquivos/classes (`bin/regua.sh:23`; `bin/suite.sh:102`; `app/core/runner_quarentena.py:72`, `:76`). |
| Selo TESTE-SEM-RELOGIO | Enumera nomes `test*.py`, portanto inclui ambos os novos arquivos; avalia seu conteúdo, não exige nomes de classe específicos (`app/core/tests/test_selo_teste_sem_relogio.py:48`, `:53`, `:59`). |
| Lista CRON-15 | Varre arquivos e classes de `api/tests` e `chamados/tests`, mas só coleta métodos decorados com `falha_esperada_cron15`. Os novos métodos não possuem essa marca, portanto não alteram o conjunto comparado (`app/core/tests/test_marca_cron15.py:25`, `:37`, `:41`, `:50`; `app/api/tests/test_api_bater_sse_sem_dia.py:33`; `app/chamados/tests/test_a2_batida_sem_linha.py:25`). |
| Cobertura POST das portas | Agrega URLs dos `test_*.py`; o POST `/api/ponto/bater/` continua presente no novo arquivo, preservando a contribuição do teste movido (`app/core/tests/test_contract_smoke_porta.py:48`, `:51`; `app/api/tests/test_api_bater_sse_sem_dia.py:41`). |
| Imports locais | Varre inclusive testes e verifica os nomes importados; os novos arquivos usam modelos existentes, sem dependência dos nomes das classes antigas (`app/core/tests/test_contract_import_local_resolve.py:62`, `:71`, `:81`; `app/colaboradores/models.py:40`, `:177`, `:219`, `:283`; `app/ponto/models.py:8`; `app/chamados/models.py:2205`). |
| Censo dos contratos | Seleciona somente `test_contract_*.py`; os dois novos nomes ficam fora desse conjunto (`app/core/tests/test_o20_contrato_julga_codigo.py:160`, `:164`; `app/api/tests/test_api_bater_sse_sem_dia.py:1`; `app/chamados/tests/test_a2_batida_sem_linha.py:1`). |
| P13 `data_evento` | Exclui diretórios `tests`, portanto os arquivos novos não entram no censo de escritores de produção (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:114`, `:115`). |
| Diagrama/cobertura por família | Lê `*/tests/test_*.py`, porém só associa selos que referenciam o registro `core.juizes`; os novos imports não fazem essa ligação (`app/core/management/commands/gerar_diagrama.py:450`, `:458`, `:665`, `:674`, `:708`, `:719`; `app/api/tests/test_api_bater_sse_sem_dia.py:15`; `app/chamados/tests/test_a2_batida_sem_linha.py:13`). |
| Guarda de RELATO | Varre testes, mas ignora fontes sem o literal `RELATO.md`; os arquivos novos não contêm esse literal (`bin/tests/test_relato_guarda_pedido_de_patch.sh:144`, `:146`; `app/api/tests/test_api_bater_sse_sem_dia.py:1`; `app/chamados/tests/test_a2_batida_sem_linha.py:1`). |
| Testes fora do Git | Compara arquivos encontrados com `git ls-files`; estado efetivamente versionado nesta cópia: [nao verificado] (`bin/testes_fora_do_git.sh:46`, `:48`, `:49`). |

O integrador também deriva automaticamente labels de módulos para arquivos em `tests`, permitindo selecionar os dois novos módulos sem cadastro adicional (`app/core/integrador_lote.py:45`, `:57`, `:58`).

## 06/11 20:35 · sessao 8fabf48e1494 · - · - · origem=Ronald (tmux)

Auditoria por inspeção; execução do MORDE e de mutantes: **[não verificado]**.

**Falha adicional crítica:** `livre` prevalece sobre `preso` quando dois nós ocupam a mesma linha. `_linhas_livres` adiciona as faixas dos nós aos dois conjuntos e retorna `livre | (soltas - preso)`, sem subtrair `preso` de `livre`. Assim, terminar o contêiner com `]; CAD = 700` ou `}; funcao()` deixa essa linha livre pelo `Assign` permitido, embora a segunda instrução seja presa; o verificador aceita a alteração por linha. (`bin/tests/test_lei_akita.sh:198`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:230`)

**O MORDE não cobre cada cláusula nova.** Os casos de medida chamam as funções reais, mas recebem mensagens/arquivos montados manualmente; os commits têm mensagem `c`, portanto não exercitam a integração de medida com `falhas_do_universo`. (`bin/tests/test_lei_akita.sh:383`, `bin/tests/test_lei_akita.sh:404`, `bin/tests/test_lei_akita.sh:413`, `bin/tests/test_lei_akita.sh:414`)

| Caso | Mutante que mata por inspeção |
|---|---|
| `so_numero`, `estado`, `comentario`, `doc` | Recusa incondicional de medida; proibição de constante-string; retirada da liberação de comentário/docstring. (`bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:389`, `bin/tests/test_lei_akita.sh:390`, `bin/tests/test_lei_akita.sh:391`, `bin/tests/test_lei_akita.sh:406`) |
| `na_def` | Liberação de corpo de função. (`bin/tests/test_lei_akita.sh:392`, `bin/tests/test_lei_akita.sh:407`) |
| `tira_def` | Julgar somente o lado novo, ignorando linhas presas removidas. (`bin/tests/test_lei_akita.sh:393`, `bin/tests/test_lei_akita.sh:407`) |
| `com_import`, `com_lambda`, `com_chamada`, `com_nome` | Liberar respectivamente import, lambda, chamada ou Name sem ser constante-string. (`bin/tests/test_lei_akita.sh:394`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:396`, `bin/tests/test_lei_akita.sh:397`, `bin/tests/test_lei_akita.sh:407`, `bin/tests/test_lei_akita.sh:408`) |
| `decisao` | Liberar Assign fora dos nomes declarados. (`bin/tests/test_lei_akita.sh:398`, `bin/tests/test_lei_akita.sh:409`) |
| `monkey` | Liberar atribuição a Attribute. (`bin/tests/test_lei_akita.sh:399`, `bin/tests/test_lei_akita.sh:409`) |
| `redecl` | Liberar a linha histórica que acrescenta outra declaração; **não demonstra unicidade no ALVO**. O HEAD é restaurado à base após criar cada commit, e a declaração é lida desse HEAD. (`bin/tests/test_lei_akita.sh:74`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:409`) |
| `sem_decl` | Aceitar `PAPEL_DO_MODULO='juiz'` no ALVO. (`bin/tests/test_lei_akita.sh:373`, `bin/tests/test_lei_akita.sh:401`, `bin/tests/test_lei_akita.sh:410`) |
| `mais_fech` | Ignorar outro arquivo de negócio. (`bin/tests/test_lei_akita.sh:402`, `bin/tests/test_lei_akita.sh:410`) |
| `so_numero` com instrumento | Estender a exceção a `n/a (instrumento)`. (`bin/tests/test_lei_akita.sh:411`) |
| ALVO sem declaração | Ler a declaração apenas de `h`, dispensando a do ALVO. (`bin/tests/test_lei_akita.sh:417`, `bin/tests/test_lei_akita.sh:418`) |
| Retroativo número/decisão | Exigir declaração em `h`; ou liberar constante de decisão fora de DADO. (`bin/tests/test_lei_akita.sh:423`, `bin/tests/test_lei_akita.sh:429`, `bin/tests/test_lei_akita.sh:431`) |

**Mutantes/lacunas que sobrevivem ou não são discriminados pelos casos existentes:**

- Retirar a unicidade de `PAPEL_DO_MODULO` ou `DADO_MEDIDO` no ALVO: nenhum caso planta ALVO com duplicação; `redecl` falha por linha presa em `h`. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:385`, `bin/tests/test_lei_akita.sh:400`, `bin/tests/test_lei_akita.sh:417`, `bin/tests/test_lei_akita.sh:429`)
- Retirar a contagem de atribuições de constantes-string: a fixture contém constantes atribuídas uma vez; nenhum caso introduz reatribuição. (`bin/tests/test_lei_akita.sh:185`, `bin/tests/test_lei_akita.sh:371`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:403`)
- Aceitar Assign com múltiplos alvos ou Subscript; liberar AnnAssign; liberar nós presos na mesma linha de dado; aceitar declaração aninhada ou DADO inválido; liberar classe/decorador, comentário dentro de função, docstring posterior, ou diff sem hunk: faltam casos específicos. (`bin/tests/test_lei_akita.sh:163`, `bin/tests/test_lei_akita.sh:169`, `bin/tests/test_lei_akita.sh:199`, `bin/tests/test_lei_akita.sh:201`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:206`, `bin/tests/test_lei_akita.sh:225`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:411`)
- Não há caso de medida com rebinding por for/with/import/walrus/del, nem leitor que interprete dado como decisão/código/chave. (`bin/tests/test_lei_akita.sh:370`, `bin/tests/test_lei_akita.sh:372`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:411`)
- O comentário “nem concessão retroativa” ficou contraditório ao teste seguinte, que exige concessão retroativa para número. (`bin/tests/test_lei_akita.sh:416`, `bin/tests/test_lei_akita.sh:420`, `bin/tests/test_lei_akita.sh:423`)

**Merge/rename/restauração:**

- Universo enumera todos os pais; começa com os caminhos diferentes do primeiro pai e remove os iguais a qualquer outro pai publicado; os restantes passam pelo predicado de medida. (`bin/tests/test_lei_akita.sh:285`, `bin/tests/test_lei_akita.sh:293`, `bin/tests/test_lei_akita.sh:296`, `bin/tests/test_lei_akita.sh:303`, `bin/tests/test_lei_akita.sh:308`, `bin/tests/test_lei_akita.sh:311`)
- `medida_do_commit` compara linhas com o primeiro pai e `--no-renames`, inclusive ambos os lados e contagem zero. (`bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:217`, `bin/tests/test_lei_akita.sh:221`, `bin/tests/test_lei_akita.sh:228`, `bin/tests/test_lei_akita.sh:230`)
- Rename mantém origem e destino; origem ausente no ALVO recusa, mas origem restaurada/declarada posteriormente pode permitir exclusão histórica se só houver linhas livres. Criação que inclua declarações normais, função ou import recusa pelas linhas presas; ausência de arquivo num lado só é tolerada quando a contagem alterada desse lado é zero. (`bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:209`, `bin/tests/test_lei_akita.sh:211`, `bin/tests/test_lei_akita.sh:217`, `bin/tests/test_lei_akita.sh:230`)
- O MORDE antigo cobre primeiro pai, merge limpo/maligno e renames com `n/a (instrumento)`; não cobre elegibilidade AST de medida nesses cenários. (`bin/tests/test_lei_akita.sh:480`, `bin/tests/test_lei_akita.sh:486`, `bin/tests/test_lei_akita.sh:513`, `bin/tests/test_lei_akita.sh:520`, `bin/tests/test_lei_akita.sh:525`, `bin/tests/test_lei_akita.sh:536`, `bin/tests/test_lei_akita.sh:540`)

**Erro Git:** `show` e `diff` novos emitem `PADRAO_ERRO`; `cat-file -e` com qualquer rc retorna AST ausente, e `rev-parse h^1` com qualquer rc retorna False. Estes últimos recusam a medida, mas não cumprem literalmente “erro de Git = PADRAO_ERRO”. Os erros dessas novas operações não têm injeção específica no MORDE. (`bin/tests/test_lei_akita.sh:138`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:142`, `bin/tests/test_lei_akita.sh:143`, `bin/tests/test_lei_akita.sh:213`, `bin/tests/test_lei_akita.sh:214`, `bin/tests/test_lei_akita.sh:218`, `bin/tests/test_lei_akita.sh:219`, `bin/tests/test_lei_akita.sh:388`, `bin/tests/test_lei_akita.sh:433`)

**Integração e resultado:** o universo real fornece o callback de medida; o shell exige `PADRAO_FIM`; o MORDE precisa produzir `MORDE_OK` e nenhuma linha `MORDE_FALHOU`. A régua percorre `test_*.sh`, e o pre-push passa `PUSH_SHA` ao selo. (`bin/tests/test_lei_akita.sh:311`, `bin/tests/test_lei_akita.sh:565`, `bin/tests/test_lei_akita.sh:568`, `bin/tests/test_lei_akita.sh:569`, `bin/regua.sh:138`, `bin/pre-push.sh:38`)

## 06/11 20:35 · sessao 34ca66140e6e · - · - · origem=Ronald (tmux)

- Os dois decoradores agora estão nas classes; a fixture API está em `setUpTestData` e a fixture A2 em `setUp`, com os corpos dentro das respectivas classes (`app/api/tests/test_api_bater_sse_sem_dia.py:19`, `:22`, `:33`; `app/chamados/tests/test_a2_batida_sem_linha.py:17`, `:19`, `:25`).

- A cópia fixa `freezegun==1.5.5` e `Django==5.2.14` (`app/requirements.txt:43`, `:13`). O comportamento esperado é o decorador de classe do freezegun envolver `setUpClass` — que inclui `setUpTestData` no Django —, `setUp` e corpo; contudo as implementações das duas dependências não estão instaladas/encontráveis neste ambiente, portanto essa semântica não pôde ser comprovada pelo código disponível: **[nao verificado]**. Nenhuma execução Django/DB foi realizada: **[nao verificado]**.

- Há campos automáticos relevantes: `Colaborador.criado_em = DateTimeField(auto_now_add=True)` e `Batida.criado_em` idem (`app/colaboradores/models.py:380`; `app/ponto/models.py:79`). A afirmação de que recebem o instante congelado depende daquela semântica de biblioteca: **[nao verificado]**. O nome de campo efetivo nesses modelos é `criado_em` (`app/colaboradores/models.py:380`; `app/ponto/models.py:79`).

- Evitar a formulação absoluta “o arquivo inteiro não lê relógio real”: os imports são executados antes da aplicação do decorador; o que o código mostra diretamente é decoração da classe, não congelamento do import do módulo (`app/api/tests/test_api_bater_sse_sem_dia.py:9`, `:19`; `app/chamados/tests/test_a2_batida_sem_linha.py:8`, `:17`). Cobertura de imports, `_pre_setup` do Django, rotinas do runner, bibliotecas ignoradas, threads e relógio externo: **[nao verificado]**.

- As fixtures API **não são literalmente idênticas**: os nomes têm sufixo `2` e o username mudou; os campos de empresa, praça, posto, colaborador, flags e ausência de criação de escala/batida continuam correspondentes (`app/api/tests/test_api_bater_ponto.py:197`; `app/api/tests/test_api_bater_sse_sem_dia.py:21`). Não reproduzir os identificadores pessoais encontrados na cópia (`app/api/tests/test_api_bater_ponto.py:206`; `app/api/tests/test_api_bater_sse_sem_dia.py:26`).

- A2 também muda os rótulos para sufixo `2`; mantém os demais parâmetros de criação e o usuário `sistema` (`app/chamados/tests/test_a2_data_evento_batida.py:11`; `app/chamados/tests/test_a2_batida_sem_linha.py:19`).

- CNPJ e CPF são realmente únicos no modelo; portanto haveria colisão se as linhas fossem compartilhadas no mesmo estado persistente (`app/colaboradores/models.py:43`, `:319`). Ambas as classes novas e as antigas herdam de `django.test.TestCase`, sem substituição local dessa base (`app/api/tests/test_api_bater_sse_sem_dia.py:10`, `:20`; `app/api/tests/test_api_bater_ponto.py:194`; `app/chamados/tests/test_a2_batida_sem_linha.py:9`, `:18`; `app/chamados/tests/test_a2_data_evento_batida.py:5`, `:10`). Isolamento transacional/limpeza entre classes é o comportamento esperado do Django TestCase, mas sua implementação e execução não puderam ser verificadas nesta cópia: **[nao verificado]**.

## 06/11 20:34 · sessao 5a2662cb0fd2 · - · - · origem=Ronald (tmux)

Os três commits R1 tocarem somente `RESULTADOS`/`ETAPAS`: **[não verificado]**. A cópia não contém `.git`, nem os patches individuais. O relato cita as origens e as duas correções, mas não prova seus diffs. (`app/docs/RELATO.md:864`, `app/docs/RELATO.md:869`)

Os contêineres atuais ocupam `app/core/placar_estrutural.py:71` até `:624` e `app/core/espelho_verdade.py:35` até `:195`. A justificativa da retroatividade cita os três R1, sem demonstrar suas linhas históricas. (`bin/tests/test_lei_akita.sh:133`)

**Achado adicional: executável na mesma linha passa.** Alterar o fechamento `]` de `RESULTADOS` para `]; print(123)`, `]; f.x = 1` ou `]; import os` mantém a linha simultaneamente livre e presa; o retorno não subtrai `preso` de `livre`. Confirmei os três resultados em memória com as funções reais, sem escrever arquivos. O selo Git integral permanece **[não verificado]**. (`bin/tests/test_lei_akita.sh:202`, `bin/tests/test_lei_akita.sh:204`, `bin/tests/test_lei_akita.sh:206`)

## 06/11 20:28 · sessao a13400452ea1 · BUG-PLANTIO-NOTURNO-MAIS-1 · verificador · origem=Code

**O tratamento de `None` está coerente com o fluxo atual, mas a V3 tem uma reprovação independente no selo de relógio:** o novo `freeze_time` faz o varredor considerar o arquivo API “curado”, enquanto ele permanece na lista de exceções (`app/api/tests/test_api_bater_ponto.py:242`; `app/core/tests/test_selo_teste_sem_relogio.py:28`, `:72`; `app/core/tests/testes_sem_relogio.txt:5`). Execução Django completa, execução contra o pai e efeitos em produção: [nao verificado].

1. **Movimento do bloco e existência de `batida`.**

   Entre a posição nova e a antiga existem três operações relevantes: `em_disputa = False`, leitura de `motivos_anomalia` pelo `if` e escrita de `contexto_anomalia['ultima_batida']`, lendo `batida.pk`, `tipo` e `agora`. Portanto, **há leitura/escrita entre as posições**; antecipar a normalização permite que a remoção do último motivo impeça a entrada nesse `if` (`app/api/views.py:785`; `app/api/views_core.py:785`).

   **A batida já passou pela persistência.** A API chama `registrar_batida` antes; a porta devolve uma existente ou executa `Batida.objects.create` antes de retornar. Se devolver `None`, a API responde e sai antes de alcançar o bloco novo (`app/api/views.py:684`, `:699`; `app/api/views_core.py:681`, `:696`; `app/ponto/registro_batida.py:113`, `:120`, `:157`).

   “Já gravada” inclui **linha preexistente**, inclusive chave retratada devolvida pela busca de mesma chave; não significa necessariamente criação nesta requisição (`app/ponto/registro_batida.py:116`). Confirmação de commit em produção: [nao verificado].

2. **Outras origens de `orfao_14h` e segurança da remoção.**

   Existe outra origem sem `entrada_sem_saida`: `detectar_anomalias` recebe bloqueio órfão, acrescenta `orfao_14h` e grava contexto sob **`orfao_14h`**. Se ocorrer exceção depois do append, o `except` também pode preservar o motivo sem completar esse contexto (`app/chamados/detector_anomalias.py:31`, `:34`, `:49`, `:54`).

   **Esse órfão não é apagado pelo bloco novo no fluxo atual.** A escrita de `saida_sem_entrada` exige previamente `'orfao_14h' not in motivos_anomalia`. Se o detector já trouxe o motivo, não nasce esse contexto; sem ele, o novo bloco não executa (`app/api/views.py:652`, `:655`, `:775`; `app/api/views_core.py:649`, `:652`, `:775`).

   A origem E sem S acrescenta o motivo junto de `entrada_sem_saida`, explicitamente preservado pela remoção. Além disso, E e S são ramos distintos pelo mesmo `tipo` (`app/api/views.py:635`, `:663`, `:668`, `:780`; `app/api/views_core.py:632`, `:660`, `:665`, `:780`).

   Assim, **não aparece apagamento de órfão legítimo de outra origem nos caminhos atuais**. A proteção depende da guarda que impede criar SSE quando o motivo já existe; o código não mantém uma contagem de origens do motivo (`app/api/views.py:652`, `:780`; `app/api/views_core.py:649`, `:780`).

   A remoção também atua na lista local da requisição: o helper agrega motivos à disputa existente por união; não remove automaticamente motivos antigos persistidos (`app/api/views.py:781`, `:795`; `app/chamados/services/disputa_emissao.py:506`).

3. **Quem consome depois e se exige motivo para toda SSE.**

   | Consumidor | Comportamento |
   |---|---|
   | Guarda da API | Lista não vazia decide escrever `ultima_batida`, chamar o helper e marcar `em_disputa` (`app/api/views.py:787`; `app/api/views_core.py:787`). |
   | JSON | Devolve a lista resultante em `motivos_disputa` (`app/api/views.py:848`; `app/api/views_core.py:848`). |
   | Aviso | Recebe a lista; nesse argumento, consulta apenas `em_ferias` (`app/api/views.py:838`; `app/ponto/services/aviso_da_batida.py:69`). |
   | Disputa/perguntas | Helper aceita lista vazia retornando `None`; com motivos, copia/deduplica, abre/agrega e gera perguntas (`app/chamados/services/disputa_emissao.py:626`, `:629`, `:632`, `:640`, `:652`). |
   | Push | É chamado no nascimento ou para motivos novos; usa o primeiro motivo e possui mensagem genérica para lista vazia (`app/chamados/services/disputa_emissao.py:641`, `:656`, `:1574`). |
   | Chamado de auditoria de desvio leve | É criado **antes** do bloco novo, usando `ctx_desvio`, `batida` e `tipo`; não lê SSE nem `motivos_anomalia` (`app/api/views.py:735`; `app/api/views_core.py:732`). |
   | Auditor de disputas | Examina motivos efetivamente persistidos e sua correspondência com contexto (`app/chamados/management/commands/auditar_disputas.py:85`). |

   **Esses consumidores não exigem que toda S sem entrada produza motivo:** lista vazia e ausência de emissão são caminhos explícitos. Contrato de clientes externos que recebem o JSON: [nao verificado] (`app/api/views.py:787`, `:848`; `app/chamados/services/disputa_emissao.py:626`).

4. **Mordida do RED, patch e roteamento.**

   **Há mordida dedutiva contra o pai descrito no anexo; execução contra `39324b76`: [nao verificado].** A fixture cria colaborador sem escala/batidas e envia S três horas retroativa. Esse atraso é aceito e evita a correção de tipo ao vivo; sem escala, a correção retroativa não altera S (`app/api/tests/test_api_bater_ponto.py:197`, `:243`; `app/api/hora_do_aparelho.py:141`; `app/api/views_core.py:592`, `:611`).

   Sem batidas/marcos, a classificação tem fallback que fornece motivo; a guarda pode então criar SSE. Com o patch devolvendo `None`, o trecho antigo apresentado no anexo tentaria `None.isoformat()`, produzindo erro — o cliente de testes pode propagá-lo antes dos asserts, continuando RED (`app/escala/servico_jornada.py:469`, `:544`; `app/api/views_core.py:649`; `app/api/tests/test_api_bater_ponto.py:244`). Correspondência do trecho antigo com o objeto Git do pai: [nao verificado].

   **O patch alcança os dois imports locais:** ele fica ativo durante o POST, e ambos importam o atributo de `ponto.turnos` dentro da função, imediatamente antes de chamá-lo (`app/api/tests/test_api_bater_ponto.py:243`; `app/api/views.py:776`; `app/api/views_core.py:776`).

   **Qual gêmeo resolve depende do settings:**

   | Execução | Destino |
   |---|---|
   | Esteira `bin/suite.sh` | Usa `config.settings.ci` → `config.urls_ci` → `api.urls` → **`views.py`** (`bin/suite.sh:103`; `app/config/settings/ci.py:5`; `app/config/urls_ci.py:18`; `app/api/urls.py:2`, `:67`). |
   | `config.settings_test` | Usa `config.urls_test`, concatenando rotas core primeiro → **`views_core.py`** (`app/config/settings_test.py:9`; `app/config/urls_test.py:8`, `:11`, `:15`; `app/api/urls_core.py:5`, `:14`). |

   Settings efetivamente usado numa execução do RED anexado: [nao verificado].

   **O outro gêmeo tem selo de paridade AST.** O selo compara funções homônimas dos dois arquivos, com whitelist vazia; a comparação em memória confirmou igualdade atual de `api_bater_ponto`. Isso comprova igualdade estrutural, não duas execuções comportamentais (`app/api/tests/test_contract_paridade_kernels.py:12`, `:18`, `:26`).

   O RED simula **a resposta `None`**; não prepara uma chave retratada real nem prova integralmente esse cenário de persistência (`app/api/tests/test_api_bater_ponto.py:197`, `:244`, `:254`).

5. **Classes de reprovação.**

   | Classe | Resultado da inspeção |
   |---|---|
   | Segundo juiz | O delta consulta a autoridade existente. O helper recorta `dia_das_batidas` por PK; essa fonte usa `t.data_turno`, com dia civil para batida que nenhum turno reclama (`app/api/views.py:776`; `app/ponto/turnos.py:2917`, `:2935`). |
   | Segundo escritor fora da porta | O delta altera lista/dicionário locais; Batida continua entrando por `registrar_batida`, e a disputa pelo helper existente (`app/api/views.py:684`, `:779`, `:795`; `app/api/views_core.py:681`, `:779`, `:795`). |
   | Relógio fora da borda | O bloco novo não consulta relógio; o placeholder civil anterior permanece (`app/api/views.py:655`, `:775`; `app/api/views_core.py:652`, `:775`). |
   | Derivação paralela | O juiz de slot consulta `_ts_tipo_do_plantio`; o plantio aplica o ajustador overnight existente (`app/chamados/juizes.py:344`; `app/chamados/services/materializacao.py:418`). |
   | Selo/RED sem mordida | RED tem mordida dedutiva; paridade verifica AST. O selo P13 fiscaliza escrita de `data_evento`, sem provar a semântica de `SSE.data` (`app/api/tests/test_api_bater_ponto.py:236`; `app/api/tests/test_contract_paridade_kernels.py:26`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:25`, `:58`). |

   **Reprovação concreta do selo de relógio:** a avaliação em memória das funções reais encontrou `curados=['api/tests/test_api_bater_ponto.py']`; a asserção exige lista vazia. O regex considera o arquivo inteiro congelado ao encontrar qualquer `freeze_time`, mas a entrada API continua na lista (`app/core/tests/test_selo_teste_sem_relogio.py:28`, `:32`, `:72`; `app/core/tests/testes_sem_relogio.txt:5`).

   A retirada de A2 satisfaz esse critério textual, mas **não demonstra congelamento integral**: somente um método está decorado; outros ainda usam `now()` e `localdate()` fora dele (`app/chamados/tests/test_a2_data_evento_batida.py:32`, `:49`, `:51`; `app/core/tests/test_selo_teste_sem_relogio.py:34`).

   **ESMERIL-SEMANTICA — literalmente, não são todos os escritores de `SSE.data` que escrevem dia do turno.** Os gêmeos primeiro escrevem `agora_local.date()`, depois sobrescrevem pelo juiz ou removem a chave antes da emissão. O desvio intermediário usa contexto próprio, sem ler essa data provisória (`app/api/views.py:655`, `:729`, `:775`, `:793`; `app/api/views_core.py:652`, `:726`, `:775`, `:793`).

   Os demais escritores identificados usam dia do juiz ou da célula; os leitores usam a data para texto, destino, `data_evento` e plantio (`app/ponto/services/triagem_batida.py:726`; `app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/disputa_emissao.py:283`, `:292`, `:342`, `:364`; `app/chamados/catalogo/query.py:58`; `app/chamados/services/materializacao.py:343`).

   Há ainda uma distinção preexistente: **a API escreve contexto SSE, mas acrescenta motivo `orfao_14h`**. Como o gerador percorre os motivos, ele não lê automaticamente a chave SSE; pode usar os fallbacks do órfão. O catálogo distingue pergunta de saída e de entrada (`app/api/views.py:654`; `app/api/views_core.py:651`; `app/chamados/services/disputa_emissao.py:312`, `:337`, `:364`; `app/chamados/catalogo/perguntas.py:48`, `:56`).

6. **Quem escreve e quem lê os campos tocados.**

   | Campo | Quem escreve | Quem lê |
   |---|---|---|
   | `motivos_anomalia` | Detector inicial; acréscimos API de foto/GPS/device/liveness, SSE, E sem S e desvio grave; nova remoção (`app/chamados/detector_anomalias.py:26`; `app/api/views.py:482`, `:490`, `:507`, `:514`, `:521`, `:654`, `:671`, `:733`, `:781`). | Guardas de duplicidade/remoção, guarda de emissão, helper, aviso e JSON (`app/api/views.py:652`, `:669`, `:780`, `:787`, `:795`, `:838`, `:848`). |
   | `contexto_anomalia` | Detector inicial, detalhes das anomalias API, normalização SSE e `ultima_batida` (`app/api/views.py:446`, `:483`, `:491`, `:508`, `:515`, `:522`, `:655`, `:672`, `:734`, `:779`, `:783`, `:788`). | Bloco novo e helper de disputa (`app/api/views.py:775`, `:780`, `:795`). |
   | `saida_sem_entrada` e `.data` | API cria placeholder; delta remove ou substitui `.data`. PWA, fábrica e emissão também podem escrever a chave por motivo (`app/api/views.py:655`, `:779`, `:783`; `app/api/views_core.py:652`, `:779`, `:783`; `app/ponto/services/triagem_batida.py:738`; `app/chamados/services/fabrica_da_celula.py:139`; `app/chamados/services/disputa_emissao.py:292`, `:345`). | Normalizador; leitores genéricos de texto, destino/data, plantio e auditoria (`app/api/views.py:775`; `app/chamados/catalogo/query.py:58`; `app/chamados/services/disputa_emissao.py:191`, `:364`; `app/chamados/services/materializacao.py:343`; `app/chamados/management/commands/auditar_disputas.py:75`). |
   | `entrada_sem_saida` | Ramo E, incluindo `entrada_id`, `entrada_aberta_em` e `hora_tentativa` (`app/api/views.py:672`; `app/api/views_core.py:669`). | Guarda de preservação do motivo; derivação do dia pela batida de origem (`app/api/views.py:780`; `app/api/views_core.py:780`; `app/chamados/services/disputa_emissao.py:59`, `:68`). |
   | `batida` / PK / timestamp | Porta devolve linha existente ou criada; o delta não altera seus campos (`app/ponto/registro_batida.py:113`, `:120`, `:157`; `app/api/views.py:684`; `app/api/views_core.py:681`). | Novo helper, que lê timestamp e procura o PK no mapa do juiz (`app/api/views.py:777`; `app/api/views_core.py:777`; `app/ponto/turnos.py:2921`). |
   | `_dia_sse` | Retorno de `dia_do_turno_da_batida` (`app/api/views.py:777`; `app/api/views_core.py:777`). | Teste de `None` e serialização ISO (`app/api/views.py:778`, `:783`; `app/api/views_core.py:778`, `:783`). |
   | `ultima_batida`, `em_disputa`, `motivos_disputa` | API escreve contexto, flag e resposta após normalização (`app/api/views.py:786`, `:788`, `:797`, `:847`; `app/api/views_core.py:786`, `:788`, `:797`, `:847`). | Emissão consulta `ultima_batida.id`; o RED verifica status, motivos e existência de S (`app/chamados/services/disputa_emissao.py:72`, `:379`; `app/api/tests/test_api_bater_ponto.py:252`). |

   A persistência dos motivos/contextos ocorre no merge ou nascimento da disputa; esses pontos copiam/agregam os valores recebidos, sem reinterpretar `SSE.data` (`app/chamados/services/disputa_emissao.py:506`, `:518`, `:550`, `:557`).

## 06/11 20:26 · sessao f234c37c52bf · - · - · origem=Ronald (tmux)

1. **O RED tem mordida por dedução; execução contra `39324b76` é [nao verificado].** A fixture cria colaborador sem escala/batidas (`app/api/tests/test_api_bater_ponto.py:197-211`). Envia S retroativa em 3h sob relógio congelado (`:243-251`); 3h cabe na janela de 48h (`app/api/hora_do_aparelho.py:63,141-146`), escapa da correção ao vivo (`app/api/views_core.py:592-600`) e, sem escala, escapa da correção retroativa (`:611-617`). `_motivo_da_lei` delega à autoridade (`:374-382`); sem escala não há veto noturno nem teto (`app/escala/servico_jornada.py:224-227,366-369`), e o fallback sem batidas/marcos devolve motivo (`:396-426,469-471`). Portanto SSE nasce em `app/api/views_core.py:649-655` e alcança o bloco que o diff anexo mostra chamando `None.isoformat()` no pai. Isso causa `AttributeError`; no cliente de testes pode propagar como erro antes dos asserts, ainda RED. Histórico Git indisponível nesta cópia e execução real no pai [nao verificado].

2. **O patch atinge o import local dos dois gêmeos.** O patch substitui o atributo do módulo durante o POST (`app/api/tests/test_api_bater_ponto.py:243-251`); ambos executam `from ponto.turnos import dia_do_turno_da_batida as _dtb_sse` dentro da função e chamam o alias imediatamente (`app/api/views.py:775-777`; `app/api/views_core.py:775-777`). A chamada anterior possível é detector→resolver, mas depende de entrada órfã preexistente, ausente nessa fixture (`app/chamados/detector_anomalias.py:29-48`; `app/escala/servico_jornada.py:485-488`). Os demais usos na emissão de disputa ocorrem após o bloco API (`app/api/views_core.py:793-795`; `app/chamados/services/disputa_emissao.py:379-384`).

3. **Qual gêmeo depende do settings; a esteira atual resolve `views.py`.**
   - Runner canônico usa `--settings=config.settings.ci` (`bin/suite.sh:102-103`); CI define `ROOT_URLCONF='config.urls_ci'` (`app/config/settings/ci.py:5`); esse URLconf inclui `api.urls` (`app/config/urls_ci.py:18`); `api.urls` importa `views` e liga `/ponto/bater/` (`app/api/urls.py:2,67`).
   - Sob `config.settings_test`, resolve `views_core.py`: ROOT em `app/config/settings_test.py:9`; core primeiro em `app/config/urls_test.py:8-15`; import e rota em `app/api/urls_core.py:5,14`.
   - O comando/settings efetivamente usado no RED anexado é [nao verificado].

4. **O outro gêmeo tem selo AST real, mas não segunda execução comportamental neste teste.** O selo lê funções de topo dos dois arquivos, compara nomes comuns, ignora comentários via AST e tem whitelist vazia (`app/api/tests/test_contract_paridade_kernels.py:12-39`). Conferi exclusivamente em memória com `python3 -B` e AST: `api_bater_ponto AST identical: True`. Isso cobre a igualdade de implementação atual; executar POST contra ambos permanece [nao verificado].

5. **Há reprovação determinística do selo de relógio nesta árvore.** Extraí por AST, sem importar apps/Django nem escrever arquivos, as funções reais de `app/core/tests/test_selo_teste_sem_relogio.py` e executei a varredura: `novos=[]`; `curados=['api/tests/test_api_bater_ponto.py']`. O novo `freeze_time` faz o regex global considerar o arquivo API inteiro congelado (`app/core/tests/test_selo_teste_sem_relogio.py:28-35`), mas ele continua na lista (`app/core/tests/testes_sem_relogio.txt:5`); `test_a_lista_so_encolhe` exige lista de curados vazia (`app/core/tests/test_selo_teste_sem_relogio.py:72-74`). Portanto essa asserção falha com a cópia atual; execução Django completa [nao verificado].

6. **A retirada de A2 da lista satisfaz o selo por texto, não prova congelamento integral.** A2 importa `freeze_time` (`app/chamados/tests/test_a2_data_evento_batida.py:8`), mas só decora um método (`:51-52`). Outros métodos continuam chamando `timezone.now()` e `timezone.localdate()` fora desse congelamento (`:32,49`). O regex devolve zero para o arquivo inteiro ao encontrar qualquer ocorrência (`app/core/tests/test_selo_teste_sem_relogio.py:28-35`). A retirada consta no delta fornecido; o detector real confirmou em memória que A2 retorna zero e já não está na lista.

## 06/11 20:26 · sessao 7995a06ebdd3 · - · - · origem=Ronald (tmux)

**Q2 — outras origens e segurança da remoção**

- Outra fonte sem `entrada_sem_saida`: `detectar_anomalias()` chama `detectar_bloqueio_turno_orfao`, acrescenta `orfao_14h` e escreve contexto sob **`orfao_14h`**, não `entrada_sem_saida`; uma exceção posterior ao append também preserva o motivo e é capturada (`app/chamados/detector_anomalias.py:31-55`). O resultado entra na API em `app/api/views.py:446` / `app/api/views_core.py:433`.
- A fonte E sem S acrescenta o motivo junto de `entrada_sem_saida` (`app/api/views.py:663-675`; `app/api/views_core.py:660-672`).
- **Não achei caminho alcançável para a nova remoção apagar o órfão do detector:** o escritor de `saida_sem_entrada` exige que `orfao_14h` ainda **não** exista; se o detector já o trouxe, não escreve SSE, logo não entra no novo bloco (`app/api/views.py:652-655`, `775-781`; `app/api/views_core.py:649-652`, `775-781`). A proteção por `entrada_sem_saida` é conservadora; E e S são ramos mutuamente exclusivos pelo `tipo` nessas linhas.
- A garantia depende desse invariante local; uma futura fonte que acrescente órfão depois de escrever SSE precisaria preservar sua origem [nao verificado].

**Q3 — consumidores posteriores**

- API: o teste de lista não vazia decide gravar `ultima_batida`, chamar o helper e marcar `em_disputa`; depois `aviso_do_dia` lê a lista e o JSON a devolve (`app/api/views.py:787-797`, `838-848`; mesmos números no core).
- `aviso_do_dia` consulta apenas a presença de `em_ferias` nessa lista (`app/ponto/services/aviso_da_batida.py:69-71`).
- Helper: retorna sem efeitos com lista vazia; cria uma cópia `sorted(set(motivos))`; abre/agrega, gera perguntas, manda push quando nasce ou aparecem motivos novos (`app/chamados/services/disputa_emissao.py:626-657`; reexport em `app/chamados/models.py:3349`).
- Persistência: agrega motivos em disputa existente (`disputa_emissao.py:506-512`), funde contexto (`518-535`); no nascimento grava chamado e disputa com motivos/contexto (`538-561`).
- Push usa o primeiro motivo e admite lista vazia com frase genérica (`disputa_emissao.py:1574-1589`). [nao verificado]
- Chamado de auditoria de desvio leve vem **antes** do novo bloco e usa `ctx_desvio`, `batida`, `tipo`; não usa `motivos_anomalia` nem SSE (`app/api/views.py:735-768`; core `732-768`).
- Auditor de disputas avalia somente motivos efetivamente persistidos e aceita ausência de pergunta quando o contexto não permite texto datado (`app/chamados/management/commands/auditar_disputas.py:85-95`, `188-195`).
- Esses consumidores não impõem que qualquer S sem entrada necessariamente gere motivo: lista vazia é tratada explicitamente pelo fluxo (`views.py:787`, `disputa_emissao.py:626-627`).

**ESMERIL — `saida_sem_entrada.data`**

A afirmação literal “dia do turno em **todos os escritores**” é falsa para o valor transitório: os gêmeos inicialmente escrevem **dia do relógio**, `agora_local.date()`, e depois normalizam pelo `_T1` ou removem o contexto (`app/api/views.py:655-657`, `775-783`; core `652-654`, `775-783`). Não identifiquei leitor dessa data provisória entre a escrita e a normalização; os consumidores aparecem após `787` e o desvio usa contexto próprio (`views.py:729-768`, `787-795`; core `726-768`, `787-795`).

Outros escritores:

- PWA: consulta `_T1` e só escreve SSE com resposta não nula (`app/ponto/services/triagem_batida.py:726-740`).
- Fábrica genérica: `{motivo: {'data': dia.isoformat()}}`; o caminho do julgamento passa `cel.data` (`app/chamados/services/fabrica_da_celula.py:129-139`, `174-176`).
- Pendentes: lê pks, consulta `_T1`, chama classificação e grava `contexto[_motivo]['data']`; `_motivo` pode ser SSE (`app/chamados/services/disputa_emissao.py:277-296`).
- Fallback do gerador: cria contexto **local**, não o persiste diretamente, com dia obtido de `_T1` e confirmado pela grade (`disputa_emissao.py:65-86`, `342-346`).
- Merge/nascimento persistem esses valores sem reinterpretar a data (`disputa_emissao.py:518-560`).

Leitores:

- Texto da pergunta: `contexto[motivo]['data']`, formatação sem deslocar dia (`app/chamados/catalogo/query.py:58-64`).
- Destino/dia da pergunta: contexto como fallback, comparação com `dia_do_fato(ch)` e conversão para `date` (`disputa_emissao.py:191-196`, `230-250`).
- `data_evento`: contexto → parse; sem resposta e pergunta de hora, não cria pergunta (`disputa_emissao.py:364-367`, `407-430`).
- Plantio: prefere `p.data_evento`; fallback em `contexto[p.motivo]['data']` (`app/chamados/services/materializacao.py:341-349`).
- Auditoria: lê a existência e o valor, sem alterar dia (`app/chamados/management/commands/auditar_disputas.py:75-95`).

Detalhe semântico preexistente: API escreve SSE no **contexto**, mas acrescenta **`orfao_14h`** na lista; o gerador itera a lista por motivo, logo esse caso usa o caminho/fallback de `orfao_14h`, não automaticamente a chave SSE (`app/api/views.py:654-655`; `app/chamados/services/disputa_emissao.py:312`, `337-345`, `364`). A pergunta catalogada para órfão pede **saída**; a de SSE pede **entrada** (`app/chamados/catalogo/perguntas.py:48-58`). Isso não é introduzido pelo delta (`views.py:652-657`; delta fornecido).

**Campos diretamente tocados pelo delta**

| Campo | Escritores | Leitores |
|---|---|---|
| `motivos_anomalia` | detector `detector_anomalias.py:26-55`; API anomalias `views.py:482-522`, SSE `654`, E sem S `671`, grave `733`; nova remoção `781` (core correspondentes `474-514`, `651`, `668`, `730`, `781`) | guardas de duplicidade `views.py:652`, `669`; nova remoção `780`; helper/JSON/aviso `787-848` |
| `contexto_anomalia['saida_sem_entrada']` | API `views.py:655-657` / core `652-654`; delta remove `779`, atualiza `.data` `783` | delta `775`; persistência helper `795`; leitores genéricos acima |
| `contexto_anomalia['entrada_sem_saida']` | API `views.py:672-675` / core `669-672` | delta `780`; `_data_furo_da_grade` `disputa_emissao.py:59`, `68-71` |
| `batida` | retorno da porta `views.py:684-698` / core `681` em diante | consulta nova `_dtb_sse` `777`; guarda `699` / core `696`; helper/JSON `789`, `841-844` |
| `_dia_sse` | `_T1`, ambos `777` | comparação `778`, `.isoformat()` `783`; autoridade em `ponto/turnos.py:2917-2926` |

## 06/11 20:26 · sessao 13599b75567f · - · - · origem=Ronald (tmux)

| Classe | Conclusão estática |
|---|---|
| Segundo juiz | Delta pergunta ao juiz existente; `dia_do_turno_da_batida` apenas recorta `dia_das_batidas` por PK, cujo resultado vem de `t.data_turno`, com dia civil apenas para batida que nenhum turno reclama (`app/api/views.py:776`, `app/api/views_core.py:776`, `app/ponto/turnos.py:2917`, `app/ponto/turnos.py:2935`). |
| Segundo escritor fora da porta | Delta modifica estruturas locais; Batida entra por `registrar_batida`, que devolve existente ou executa o `create` antes do retorno (`app/api/views.py:684`, `app/api/views.py:779`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:157`). |
| Relógio fora da borda | Bloco novo não consulta relógio: consulta batida e juiz; o placeholder civil já existia e continua antes dele (`app/api/views.py:655`, `app/api/views.py:775`, `app/api/views_core.py:652`, `app/api/views_core.py:775`). |
| Derivação paralela | Juiz de slot consulta `_ts_tipo_do_plantio`; esse plantio chama o ajustador overnight; validação consulta o mesmo slot (`app/chamados/juizes.py:344`, `app/chamados/services/materializacao.py:418`, `app/chamados/services/validacao.py:822`). |
| Selo sem mordida | Selo P13 possui mutantes, mas vigia escrita de `PerguntaDisputa.data_evento`, não semântica de `contexto['saida_sem_entrada']['data']`; execução dos selos [nao verificado] (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:25`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:144`). |
| Campo com dois significados | Literalmente, **nem todo escritor de SSE.data escreve dia do turno**: primeiro ambos os kernels escrevem dia civil e depois o sobrescrevem com `_T1` ou removem; emissão fica após essa normalização (`app/api/views.py:655`, `app/api/views_core.py:652`, `app/api/views.py:775`, `app/api/views.py:793`). |

Semântica no PWA já nasce normalizada: predicado SSE é calculado antes da escrita, mas contexto só nasce após `registrar_batida` e `_T1 != None` (`app/ponto/services/triagem_batida.py:649`, `app/ponto/services/triagem_batida.py:701`, `app/ponto/services/triagem_batida.py:726`, `app/ponto/services/triagem_batida.py:738`).

Leitores tratam o valor como data da pergunta/turno: catálogo interpola `ctx[motivo].data`; emissão converte essa data e passa a `abrir`; plantio usa `p.data_evento` ou o mesmo contexto e aplica a cadeia overnight uma vez (`app/chamados/catalogo/query.py:58`, `app/chamados/services/disputa_emissao.py:363`, `app/chamados/services/disputa_emissao.py:407`, `app/chamados/services/materializacao.py:343`, `app/chamados/services/materializacao.py:418`).

`DiaDoTurno` é somente `NewType`, portanto não valida procedência semântica em runtime; a porta `abrir` rejeita `None`, enquanto `abrir_conversa` continua gravando dia civil, com perguntas de hora sem dia barradas pelo emissor (`app/core/dia_do_turno.py:13`, `app/chamados/models.py:2413`, `app/chamados/models.py:2419`, `app/chamados/services/disputa_emissao.py:410`).

## 06/11 20:21 · sessao 11bb1a203949 · L-123-MEDIDA-EM-PY · verificador · origem=Code

**Parecer: reprova. Há caminho que altera comportamento e recebe `n/a (medida)`: o selo considera livre qualquer atribuição de topo sem `lambda`, sem restringir o destino nem a expressão executada.** Isso excede “número medido e texto de prova”. (`bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:188`)

**1) Caminhos de aceite e recusa**

Contraexemplo concreto: mudar `CADENCIA_TERMOMETRO_DIAS = 7` para `700` passa pela exceção. Essa constante decide vencimento e cor do placar; com `hoje=2026-11-06` e R1 medido em `2026-10-01`, o veredito muda de AMARELO para VERDE, sem editar função. ([placar_estrutural.py:61](/home/ronald/codex_ws/c74fb85e2a25a875ce4ad571926ba2c7be820832/app/core/placar_estrutural.py:61), `app/core/placar_estrutural.py:667`, `app/core/placar_estrutural.py:685`, `bin/tests/test_lei_akita.sh:152`)

Na tabela, “aceita” significa que **o juiz de `padrao` concede a exceção**, considerando satisfeitas as demais condições; o callback remove esses arquivos da lista de negócio. (`bin/tests/test_lei_akita.sh:186`)

| Caso | Resultado pelo código |
|---|---|
| Dict com chamada: `R = {'n': fechar()}` | **Aceita.** `Assign` sem lambda; chamadas não são vetadas. (`bin/tests/test_lei_akita.sh:152`) |
| Reatribuir nome usado por função | **Aceita.** Não examina dependências entre atribuição e função. (`bin/tests/test_lei_akita.sh:152`) |
| Monkeypatch: `motor.fechar = outra` | **Aceita.** Não restringe os alvos da atribuição. Chamada solta `fechar()` seria recusada. (`bin/tests/test_lei_akita.sh:151`, `bin/tests/test_lei_akita.sh:152`) |
| Anotação executável: `x: fechar() = 1` | **Aceita.** `AnnAssign` também é livre sem lambda. (`bin/tests/test_lei_akita.sh:152`) |
| Merge | **Depende das linhas restantes.** Compara com o primeiro pai e elimina caminhos iguais a outro pai já publicado. Nas diferenças restantes, atribuições perigosas passam; função/import/classe alterados recusam. (`bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:236`, `bin/tests/test_lei_akita.sh:247`) |
| Rename | **Recusa normalmente:** `--no-renames` conserva origem e destino; origem ausente no ALVO não declara medida. Pode aceitar uma exclusão histórica de dados se a origem tiver sido restaurada e declarada no ALVO posterior. (`bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:164`, `bin/tests/test_lei_akita.sh:238`) |
| Arquivo novo que se declara medida | **Aceita se todas as linhas forem livres**, inclusive atribuições com chamadas. Novo com função/import/classe recusa. Commit raiz sem primeiro pai recusa. (`bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:169`, `bin/tests/test_lei_akita.sh:174`) |
| Declaração reatribuída depois | **Aceita.** `PAPEL_DO_MODULO='medida'` seguido de `PAPEL_DO_MODULO='negocio'` satisfaz o `any`; não verifica o último valor. (`bin/tests/test_lei_akita.sh:141`) |
| Decorador | **Recusa.** O intervalo preso começa no primeiro decorador. (`bin/tests/test_lei_akita.sh:149`, `bin/tests/test_lei_akita.sh:153`) |
| Linha de continuação | **Segue o nó AST inteiro:** atribuição com chamada aceita; continuação de import/def/class recusa. (`bin/tests/test_lei_akita.sh:150`, `bin/tests/test_lei_akita.sh:152`) |
| Hunk `-U0` com contagem zero | **Ignora corretamente esse lado**, conferindo o outro. A âncora não é tratada como linha modificada; isso permite criação e exclusão quando o lado existente é livre. (`bin/tests/test_lei_akita.sh:171`, `bin/tests/test_lei_akita.sh:174`) |
| Docstring que não é a primeira | **Recusa** a string solta após outro nó: `eh_doc` exige `i == 0`. Comentários anteriores à docstring inicial não são nós AST. (`bin/tests/test_lei_akita.sh:148`, `bin/tests/test_lei_akita.sh:151`) |
| Arquivo deletado | **Recusa se continua ausente no ALVO.** Se restaurado no ALVO, uma exclusão histórica composta somente por linhas livres pode passar; excluir função continua recusado pelo lado antigo. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:139`, `bin/tests/test_lei_akita.sh:174`) |
| Comentário/branco dentro de função/classe | **Recusa.** `preso` prevalece sobre `soltas`. (`bin/tests/test_lei_akita.sh:154`) |

Há outra abertura: **diff bem-sucedido sem nenhum cabeçalho `@@` chega a `True`**, pois não existe exigência de encontrar hunks. A produção desse cenário pelo Git real nesta cópia é **[não verificado]**. (`bin/tests/test_lei_akita.sh:171`, `bin/tests/test_lei_akita.sh:176`)

Também passam alterações de `estado`, `papel`, constantes e estrutura de `RESULTADOS`/`ETAPAS`, embora determinem contagens, classificação e percentual publicados. (`bin/tests/test_lei_akita.sh:152`, `app/core/placar_estrutural.py:643`, `app/core/placar_estrutural.py:699`, `app/core/espelho_verdade.py:205`, `app/core/espelho_verdade.py:213`)

**2) Declaração no ALVO**

**Abre classificação retroativa.** Um commit anterior que mudou uma atribuição de negócio pode passar quando um commit posterior acrescenta a declaração no ALVO. Retirá-la no ALVO também pode reprovar medidas anteriores. Os intervalos continuam sendo analisados em pai/`h`; a autorização vem de outra revisão. (`bin/tests/test_lei_akita.sh:140`, `bin/tests/test_lei_akita.sh:158`, `bin/tests/test_lei_akita.sh:168`, `bin/tests/test_lei_akita.sh:255`)

O MORDE exige explicitamente esse comportamento ao julgar `so_numero` depois de retirar a declaração no ALVO. Falta o sentido inverso: ALVO concedendo declaração a um `h` que não a possuía. (`bin/tests/test_lei_akita.sh:349`)

**3) O MORDE morde quais cláusulas?**

Ele chama as funções reais; o shell reprova qualquer `MORDE_FALHOU`, mesmo havendo `MORDE_OK` ao final. (`bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:473`, `bin/tests/test_lei_akita.sh:488`)

| Caso | Mutante que distingue por inspeção |
|---|---|
| `so_numero`, `comentario`, `doc` | Matam **aceite desligado**. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:341`) |
| `na_def` | Mata **todas as linhas livres** ou liberação de corpo de função. (`bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:342`) |
| `com_import` | Mata liberação de import. (`bin/tests/test_lei_akita.sh:334`, `bin/tests/test_lei_akita.sh:342`) |
| `com_lambda` | Mata retirada do veto a lambda. (`bin/tests/test_lei_akita.sh:335`, `bin/tests/test_lei_akita.sh:342`) |
| `sem_decl` | Mata dispensa da declaração. (`bin/tests/test_lei_akita.sh:336`, `bin/tests/test_lei_akita.sh:343`) |
| `mais_fech` | Mata ignorar o segundo arquivo de negócio. (`bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:343`) |
| Número com `n/a (instrumento)` | Mata extensão da exceção ao outro rótulo. (`bin/tests/test_lei_akita.sh:344`) |
| ALVO sem declaração | Mata leitura da declaração exclusivamente em `h`. (`bin/tests/test_lei_akita.sh:349`) |

**Não cobre cada cláusula.** Faltam casos de chamada em atribuição, monkeypatch, reatribuição funcional, declaração anulada depois, classe/decorador, docstring posterior, arquivo novo/deletado, retroatividade e ausência de hunks. Os casos específicos de medida também não cobrem merge/rename nem erro Git. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:349`)

Falta um negativo que dependa **somente do lado antigo**: `na_def` é preso dos dois lados; import e lambda já recusam pelo novo. A inserção de comentário exercita contagem zero, mas não demonstra todas as suas bordas. (`bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:333`)

A integração com o universo tampouco é exercitada nesses casos: os commits têm mensagem `"c"`, e `julgar` recebe mensagem e arquivos montados manualmente. (`bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:347`)

A promessa “erro Git = `PADRAO_ERRO`” não vale literalmente para todas as novas operações: `show` com erro vira AST ausente; erro de `rev-parse` vira `False`; o novo `diff` emite `PADRAO_ERRO`. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:165`)

Execução integral, três commits verdes e execução dos dois mutantes vermelhos: **[não verificado]**. [nao verificado]

**4) Leitores, escritores e quebra pelo nome novo**

| Símbolo | Quem escreve/define | Quem lê/chama |
|---|---|---|
| `PAPEL_DO_MODULO` | Declarações reais nos dois módulos; fixture e retirada no MORDE. (`app/core/placar_estrutural.py:38`, `app/core/espelho_verdade.py:30`, `bin/tests/test_lei_akita.sh:314`, `bin/tests/test_lei_akita.sh:338`) | `declara_medida`, por AST no ALVO. Aceita a presença sintática, sem avaliar o valor final executado. (`bin/tests/test_lei_akita.sh:139`) |
| `medida_do_commit` | Definida no selo. (`bin/tests/test_lei_akita.sh:156`) | Universo real e MORDE; resultado consumido pelo callback de `_julgar_linha`. (`bin/tests/test_lei_akita.sh:255`, `bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:351`, `bin/tests/test_lei_akita.sh:188`) |

**O nome adicional não quebra os leitores identificados:** `gerar_estado` executa os módulos e acessa atributos explícitos; os testes citados acessam `RESULTADOS`, `ETAPAS`, funções e constantes específicas. (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`, `app/core/tests/test_r1_termometro_uma_medicao.py:40`, `app/core/tests/test_r1_termometro_uma_medicao.py:44`, `app/core/tests/test_l114_placar_papel_e_termometro.py:30`)

Contrato que enumere exatamente todos os nomes globais desses módulos e quebra na suíte completa: **[não verificado]**. [nao verificado]

Régua e pre-push consomem o resultado do selo: a régua percorre `test_*.sh`; o pre-push passa o SHA empurrado. (`bin/regua.sh:138`, `bin/pre-push.sh:38`)

**5) Classes de reprovação**

- **Segundo juiz / derivação paralela:** a alteração mantém `julgar` e acrescenta um predicado de elegibilidade no mesmo fluxo. Não demonstra duplicação de autoridade registrada de cálculo, dinheiro ou batida. (`bin/tests/test_lei_akita.sh:177`, `bin/tests/test_lei_akita.sh:255`, `app/core/juizes.py:7`)
- **Selo sem mordida:** há mordida efetiva para os casos plantados, mas ela deixa sobreviver os contraexemplos de atribuição executável e de controle do placar. (`bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:152`)
- **ESMERIL-SEMÂNTICA: reprova.** Nos módulos, “medida” promete número medido e prova; no selo, significa qualquer `Assign`/`AnnAssign` sem lambda. O mesmo rótulo autoriza mudanças de decisão e efeitos executáveis. (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`, `bin/tests/test_lei_akita.sh:152`)

## 06/11 20:21 · sessao 15727da88a0e · V2-BUGS-DO-MOTOR · sonda · origem=Code

Não executei Django nem as sondas; resultados de produção: [não verificado]. [nao verificado]

O universo exige `batidas_apuraveis`, remoção de duplicatas, pareamento, eventual substituição pela ata e filtro pelo dia da jornada. Os scripts percorrem o fechamento real em leitura. (app/ponto/services/fechamento.py:623; app/ponto/motor_calculo_v2.py:530, 531, 2351, 2429, 2458)

**Script 1.** Separa adicional, HE e chamadas auxiliares dos intervalos. As somas são dos retornos reais das chamadas, antes do desconto de intervalos e da conversão para hora reduzida. O replay do calculador usa os argumentos do fechamento e aparece separado; seu ramo começa em 10/2026. (app/ponto/motor_calculo_v2.py:983–987; app/ponto/services/fechamento.py:1249–1260)

```python
import logging
import re
import sys
from collections import defaultdict
from datetime import datetime, time, timedelta

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from ponto import motor_calculo_v2 as motor
from ponto.calculador import regras
from ponto.calculador.alimentacao import (
    insumos_do_calculador, rubricas_por_dia,
)
from ponto.janelas import janela_fechamento
from ponto.services.fechamento import (
    recalcular_fechamento_mes, universo_da_competencia,
)

FONTE = (
    "app/ponto/motor_calculo_v2.py:82,110,112,191,441,983;"
    "app/ponto/services/fechamento.py:373,623,745,1253"
)

def emitir(*args):
    print(*args, "|", FONTE)

# Redação apenas na saída de erros; nenhum nome/CPF é impresso.
privados = [
    str(valor)
    for linha in Colaborador.objects.values_list("nome", "cpf")
    for valor in linha if valor
]
privados.sort(key=len, reverse=True)

def repr_seguro(exc):
    texto = repr(exc)
    for valor in privados:
        texto = texto.replace(valor, "[removido]")
    return re.sub(r"\d{3}\.?\d{3}\.?\d{3}-?\d{2}", "[removido]", texto)

def bloquear_escrita(execute, sql, params, many, context):
    comando = sql.lstrip().upper()
    permitidos = (
        "SELECT", "SHOW", "SET ", "SAVEPOINT ",
        "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT ",
    )
    if not comando.startswith(permitidos):
        raise RuntimeError("SQL de escrita bloqueado pela sonda")
    return execute(sql, params, many, context)

class Reverter(BaseException):
    pass

COD_FECHAMENTO = recalcular_fechamento_mes.__code__
COD_MINUTOS = motor.minutos_noturnos.__code__
COD_INTERVALOS = motor.minutos_noturnos_dos_intervalos.__code__
COD_PRINCIPAL = motor.MotorBase.calcular_noturnas.__code__
COD_HE = motor.he_noturna.__code__
COD_CALCULADOR = regras.noturnas_do_dia.__code__
COD_MES = motor.MotorBase.calcular_mes.__code__

def excesso_22_24(inicio, fim, prorrogacao):
    # Mesmas condições e mesmas bordas civis de minutos_noturnos:
    # app/ponto/motor_calculo_v2.py:89-117.
    if inicio >= fim or fim - inicio > timedelta(hours=26):
        return 0.0
    total = 0.0
    dia = inicio.date()
    while dia <= fim.date():
        zero = motor.TZ_LOCAL.localize(datetime.combine(dia, time()))
        cinco = motor.TZ_LOCAL.localize(datetime.combine(dia, time(5)))
        vinte_duas = motor.TZ_LOCAL.localize(
            datetime.combine(dia, time(22))
        )
        proximo_zero = motor.TZ_LOCAL.localize(
            datetime.combine(dia + timedelta(days=1), time())
        )
        if prorrogacao and max(inicio, zero) < cinco and fim > cinco:
            a = max(inicio, vinte_duas)
            b = min(fim, proximo_zero)
            if a < b:
                total += (b - a).total_seconds() / 60.0
        dia += timedelta(days=1)
    return total

def periodos_finais(locais):
    resultado = locais["resultado"]
    objetos = list(resultado.periodos) + list(resultado.periodos_ft)
    finais = {id(p): ("motor", p) for p in objetos}

    # O escritor só incorpora os dias-alvo do motor órfão:
    # app/ponto/services/fechamento.py:829-833.
    if "_r_orfa" in locais:
        r = locais["_r_orfa"]
        alvo = locais["_alvo"]
        for p in list(r.periodos) + list(r.periodos_ft):
            if timezone.localtime(p.entrada).date() in alvo:
                finais[id(p)] = ("motor_orfao", p)
    return finais

def sondar(colab, mes, empresa_id):
    ctx = {
        "locais": None, "chamadas": [], "erros": [],
        "fase": "motor", "resultado": None,
    }

    def periodo_do_motor(frame):
        periodo = None
        parcela_r3 = False
        f = frame
        while f is not None:
            candidato = f.f_locals.get("periodo")
            if periodo is None and isinstance(
                candidato, motor.PeriodoCalculo
            ):
                periodo = candidato
            # A parcela R3 é calculada em objeto temporário e creditada
            # no primeiro aberto ordenado: motor_calculo_v2.py:2553-2560.
            if f.f_code is COD_MES and 2517 <= f.f_lineno <= 2564:
                abertos = f.f_locals.get("_abs")
                if abertos:
                    periodo = min(abertos, key=lambda p: p.entrada)
                    parcela_r3 = True
            f = f.f_back
        if periodo is None:
            raise RuntimeError("Chamada noturna sem período identificável")
        return periodo, parcela_r3

    def periodo_do_calculador(dono):
        dados = dono.f_locals
        if dono.f_code is COD_HE:
            entrada, saida = dados["entrada"], dados["saida"]
        else:
            entrada, saida = dados["_e"], dados["_s"]

        dia = None
        f = dono
        while f is not None:
            if f.f_code is rubricas_por_dia.__code__:
                dia = f.f_locals.get("k")
                break
            f = f.f_back

        resultado = ctx["resultado"]
        mapa = getattr(resultado, "dia_da_jornada", {}) or {}
        candidatos = {}
        for p in list(resultado.periodos) + list(resultado.periodos_ft):
            e = getattr(p, "efetivo_entrada", None)
            s = getattr(p, "efetivo_saida", None)
            if (
                e is not None and s is not None
                and e <= entrada <= saida <= s
                and (dia is None or mapa.get(id(p)) == dia)
            ):
                candidatos[id(p)] = p
        if len(candidatos) != 1:
            raise RuntimeError(
                "Recorte do calculador sem associação unívoca ao turno: "
                "%d candidatos" % len(candidatos)
            )
        return next(iter(candidatos.values()))

    def perfil(frame, evento, retorno):
        if evento != "return":
            return
        try:
            if frame.f_code is COD_FECHAMENTO:
                ctx["locais"] = dict(frame.f_locals)
                return
            if frame.f_code is not COD_MINUTOS:
                return

            auxiliar = frame.f_back.f_code is COD_INTERVALOS
            dono = frame.f_back
            while dono is not None and dono.f_code not in (
                COD_PRINCIPAL, COD_HE, COD_CALCULADOR
            ):
                dono = dono.f_back
            if dono is None:
                raise RuntimeError("Chamador noturno não catalogado")

            if ctx["fase"] == "motor":
                p, r3 = periodo_do_motor(frame.f_back)
                canal = (
                    "motor/adicional"
                    if dono.f_code is COD_PRINCIPAL else "motor/HE"
                )
                if r3:
                    canal += "/parcela_R3"
            else:
                p = periodo_do_calculador(dono)
                canal = (
                    "replay_calculador/HE"
                    if dono.f_code is COD_HE
                    else "replay_calculador/adicional"
                )
            if auxiliar:
                canal += "/auxiliar_intervalos"

            dados = frame.f_locals
            ctx["chamadas"].append((
                canal, p, dados["inicio"], dados["fim"],
                dados["prorrogacao"], float(retorno),
            ))
        except Exception as exc:
            ctx["erros"].append(exc)

    def rastrear(frame, evento, argumento):
        if frame.f_code not in (
            COD_FECHAMENTO, motor._periodos_pelo_marco.__code__
        ):
            return None
        frame.f_trace_lines = False
        if evento == "exception" and isinstance(argumento[1], Exception):
            if not isinstance(argumento[1], StopIteration):
                ctx["erros"].append(argumento[1])
        return rastrear

    anterior_perfil = sys.getprofile()
    anterior_trace = sys.gettrace()
    try:
        try:
            with transaction.atomic():
                sys.setprofile(perfil)
                sys.settrace(rastrear)
                retorno = recalcular_fechamento_mes(
                    mes, 2026, empresa_id=empresa_id,
                    colaborador_ids=[colab.pk],
                    somente_leitura=True,
                    lavrar_dias_pagos=False,
                    retornar_objeto=True,
                )
                if colab.pk not in retorno or ctx["locais"] is None:
                    raise RuntimeError("Fechamento não devolveu o colaborador")

                locais = ctx["locais"]
                ctx["resultado"] = locais["resultado"]

                # Replay apenas do ramo que o fechamento possui:
                # app/ponto/services/fechamento.py:1249-1260.
                if mes >= 10 and not ctx["erros"]:
                    ctx["fase"] = "calculador"
                    insumos = insumos_do_calculador(
                        colab, locais["resultado"],
                        locais["_data_ini"], locais["_data_fim"],
                        celulas=locais["_cels_lote"],
                    )
                    rubricas_por_dia(
                        colab, locais["resultado"],
                        locais["_data_ini"], locais["_data_fim"],
                        insumos=insumos,
                        celulas=locais["_cels_lote"],
                        certa_cache=locais["_certa_cache"],
                        tipo_escala=locais["tipo_escala"],
                        escala_colaborador=locais["esc"],
                        feriados=locais["feriados"],
                        batidas_locais=[
                            timezone.localtime(b.timestamp)
                            for b in locais["batidas"]
                        ],
                        dias_com_ausencia_inteira=locais["_dias_aus_inteira"],
                    )
                raise Reverter()
        except Reverter:
            pass
    finally:
        sys.setprofile(anterior_perfil)
        sys.settrace(anterior_trace)

    if ctx["erros"]:
        raise ctx["erros"][0]
    return periodos_finais(ctx["locais"]), ctx["chamadas"]

def novo_resumo():
    return {"turnos": set(), "colabs": set(), "chamadas": 0,
            "minutos": 0.0, "excesso": 0.0}

desabilitado = logging.root.manager.disable
logging.disable(10**9)
try:
    with connection.execute_wrapper(bloquear_escrita):
        for mes in (9, 10, 11):
            erros = []
            universo = 0
            origens = defaultdict(int)
            canais = defaultdict(novo_resumo)
            casos = defaultdict(novo_resumo)
            tentados = 0

            for empresa_id in (2, 3, 4):
                try:
                    empresa = Empresa.objects.get(pk=empresa_id)
                    ini, fim = janela_fechamento(mes, 2026, empresa)
                    emitir(
                        "JANELA", "%02d/2026" % mes,
                        "empresa", empresa_id, ini.isoformat(), fim.isoformat(),
                        "corte_cadastrado", empresa.dia_inicio_competencia,
                    )
                    colabs = list(
                        universo_da_competencia(mes, 2026, empresa_id)
                        .order_by("pk")
                    )
                except Exception as exc:
                    erros.append(repr_seguro(exc))
                    continue

                for colab in colabs:
                    tentados += 1
                    try:
                        finais, chamadas = sondar(colab, mes, empresa_id)
                        # Só incorpora um colaborador depois de validar tudo.
                        preparados = []
                        for canal, p, a, b, prorroga, minutos in chamadas:
                            if id(p) not in finais:
                                continue  # período que não entrou na apuração
                            origem = finais[id(p)][0]
                            canal = origem + "/" + canal
                            longo = b - a > timedelta(hours=26)
                            excesso = excesso_22_24(a, b, prorroga)
                            preparados.append((
                                canal, (colab.pk, id(p)),
                                minutos, longo, excesso,
                            ))

                        universo += len(finais)
                        for origem, p in finais.values():
                            origens[origem] += 1

                        for canal, chave, minutos, longo, excesso in preparados:
                            r = canais[canal]
                            r["turnos"].add(chave)
                            r["colabs"].add(colab.pk)
                            r["chamadas"] += 1
                            for caso, ocorre in (("a", longo), ("b", excesso > 0)):
                                if not ocorre:
                                    continue
                                r = casos[(canal, caso)]
                                r["turnos"].add(chave)
                                r["colabs"].add(colab.pk)
                                r["chamadas"] += 1
                                r["minutos"] += minutos
                                r["excesso"] += excesso
                    except Exception as exc:
                        erros.append(("col%d" % colab.pk, repr_seguro(exc)))

            validade = "INVALIDO" if erros else "VALIDO"
            emitir(
                "COMPETENCIA", "%02d/2026" % mes, validade,
                "colabs_tentados", tentados,
                "UNIVERSO_TURNOS", universo,
                "por_origem", dict(origens),
            )
            # Os mesmos turnos podem aparecer em mais de um canal.
            # Auxiliares e parcelas R3 não criam turnos no universo.
            for canal in sorted(canais):
                r = canais[canal]
                emitir(
                    "CANAL", canal,
                    "turnos_avaliados", len(r["turnos"]),
                    "chamadas_avaliadas", r["chamadas"],
                )
                for caso in ("a", "b"):
                    r = casos[(canal, caso)]
                    emitir(
                        "CASO", caso, canal,
                        "n_turnos", len(r["turnos"]),
                        "n_colabs", len(r["colabs"]),
                        "colabs", ["col%d" % pk for pk in sorted(r["colabs"])],
                        "n_chamadas", r["chamadas"],
                        "soma_retornos_min", r["minutos"],
                        "soma_excesso_22_24_min", r["excesso"],
                    )
            if not canais:
                emitir("SEM_CANAIS_OBSERVADOS", validade)
            if mes == 9:
                emitir(
                    "REPLAY_CALCULADOR",
                    "NAO_APLICAVEL_AO_RAMO_DO_FECHAMENTO_09",
                )
            emitir("ERROS", len(erros), "primeiro_repr", erros[0] if erros else None)
finally:
    logging.disable(desabilitado)
```

Funções reais chamadas/observadas pelo Script 1:

- `universo_da_competencia`: app/ponto/services/fechamento.py:373; `recalcular_fechamento_mes`: :439; `apurar_por_fatia`: :132.
- `janela_fechamento`: app/ponto/janelas.py:54; `corte_da_empresa`: :36; `periodo_apuracao`: :10.
- `batidas_apuraveis`: app/ponto/turnos.py:2844; `parear_turnos`: :1190.
- `turnos_via_autoridade`: app/ponto/motor_calculo_v2.py:453; `_periodos_via_autoridade`: :509; `_periodos_pelo_marco`: :534; `MotorBase.calcular_mes`: :2265.
- `minutos_noturnos`: app/ponto/motor_calculo_v2.py:82; `minutos_noturnos_dos_intervalos`: :121; `calcular_noturnas`: :969; `he_noturna`: :409; `_inicio_do_trecho_extra`: :378.
- `insumos_do_calculador`: app/ponto/calculador/alimentacao.py:305; `rubricas_por_dia`: :353; `noturnas_do_dia`: app/ponto/calculador/regras.py:66.

Os chamadores diretos são `calcular_noturnas`, `he_noturna`, `minutos_noturnos_dos_intervalos` e `noturnas_do_dia`. O primeiro recebe pontas ajustadas; HE recebe a cauda calculada; os intervalos usam prefixos; o calculador pode fornecer segmentos separados pelas pausas. `MotorPlantao` delega ao cálculo do 12x36. (app/ponto/motor_calculo_v2.py:983, 440–443, 191–196, 2759, 3510–3511; app/ponto/calculador/regras.py:85; app/ponto/calculador/alimentacao.py:128–158)

**Script 2.** Usa a distribuição real de `lavrar`, interrompendo-a antes da primeira escrita. Também envolve cada apuração em `transaction.atomic()` com exceção final para rollback e bloqueia SQL de escrita. Imprime o censo pelo juiz de turno e, separadamente, pelos períodos abertos finais do motor. Essas duas representações podem diferir. (app/ponto/services/dia_pago.py:135, 333–358; app/ponto/motor_calculo_v2.py:867–882)

```python
import inspect
import logging
import re
import sys
from collections import defaultdict
from datetime import date

from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador, Empresa
from ponto import motor_calculo_v2 as motor
from ponto.janelas import janela_fechamento
from ponto.services.dia_pago import lavrar
from ponto.services.fechamento import (
    recalcular_fechamento_mes, universo_da_competencia,
)

FONTE = (
    "app/ponto/services/fechamento.py:439,623,1185;"
    "app/ponto/motor_calculo_v2.py:867,2495,2558;"
    "app/ponto/services/dia_pago.py:135,343,357"
)

def emitir(*args):
    print(*args, "|", FONTE)

privados = [
    str(valor)
    for linha in Colaborador.objects.values_list("nome", "cpf")
    for valor in linha if valor
]
privados.sort(key=len, reverse=True)

def repr_seguro(exc):
    texto = repr(exc)
    for valor in privados:
        texto = texto.replace(valor, "[removido]")
    return re.sub(r"\d{3}\.?\d{3}\.?\d{3}-?\d{2}", "[removido]", texto)

def bloquear_escrita(execute, sql, params, many, context):
    comando = sql.lstrip().upper()
    if not comando.startswith((
        "SELECT", "SHOW", "SET ", "SAVEPOINT ",
        "RELEASE SAVEPOINT ", "ROLLBACK TO SAVEPOINT ",
    )):
        raise RuntimeError("SQL de escrita bloqueado pela sonda")
    return execute(sql, params, many, context)

class Capturado(BaseException):
    pass

class Reverter(BaseException):
    pass

COD_FECHAMENTO = recalcular_fechamento_mes.__code__
COD_JUIZ = motor.MotorBase._turnos_do_juiz.__code__
COD_LAVRAR = lavrar.__code__

# Descobre a barreira no fonte carregado; falha se não for unívoca.
fonte_lavrar, primeira_linha = inspect.getsourcelines(lavrar)
barreiras = [
    primeira_linha + indice
    for indice, linha in enumerate(fonte_lavrar)
    if "DiaPago.objects.filter(" in linha and ".delete()" in linha
]
if len(barreiras) != 1:
    raise RuntimeError("ERROS 1: barreira de escrita de lavrar não identificada")
LINHA_ANTES_DA_ESCRITA = barreiras[0]

def sondar(colab, mes, empresa_id):
    ctx = {
        "locais": None, "turnos": {}, "linhas": None,
        "erros": [],
    }

    def perfil(frame, evento, retorno):
        if evento != "return":
            return
        try:
            if frame.f_code is COD_FECHAMENTO:
                ctx["locais"] = dict(frame.f_locals)
            elif frame.f_code is COD_JUIZ and retorno is not None:
                ini = frame.f_locals["data_ini"]
                fim = frame.f_locals["data_fim"]
                for turno in retorno:
                    if turno.data_turno is None or not (
                        i

[truncado em 20000 de 32436 caracteres]

## 06/11 20:20 · sessao 3487bdb015de · - · - · origem=Ronald (tmux)

Considere abaixo “aceita” como `julgar` devolvendo `None` para `padrao=n/a (medida)`, com o arquivo no conjunto de negócio e sem outro bloqueador no commit. A exceção remove arquivos cujo callback `medida(a)` retorna verdadeiro. (`bin/tests/test_lei_akita.sh:186-191`)

| Caso | Resultado e fundamento |
|---|---|
| Dict com chamada, `R={'n': fechar()}` | **Aceita**: qualquer `Assign`/`AnnAssign` de topo sem `Lambda` é livre; não examina chamadas nem efeitos. (`bin/tests/test_lei_akita.sh:152-155`) |
| Reatribuir global usado por função | **Aceita**, inclusive `FATOR=2`, sem alterar a função: não analisa dependências dos leitores. (`bin/tests/test_lei_akita.sh:152-155`) |
| Monkeypatch de topo, `motor.fechar=outro` | **Aceita**: alvos de atribuição não são restringidos a nomes de dados. (`bin/tests/test_lei_akita.sh:152-155`) |
| Anotação executável, `x: fechar()=1` | **Aceita**: `AnnAssign` inteiro é livre, exceto se houver `Lambda`. (`bin/tests/test_lei_akita.sh:152-155`) |
| Chamada solta `fechar()` | **Recusa**: `Expr(Call)` entra em `preso`. (`bin/tests/test_lei_akita.sh:151-155,174-175`) |
| Função/classe/import mexido | **Recusa** se houver linha desse nó efetivamente alterada: seu intervalo fica preso. (`bin/tests/test_lei_akita.sh:148-155,174-175`) |
| Declaração seguida de `PAPEL_DO_MODULO='negocio'` | **Aceita**: basta existir *algum* `Assign` literal de topo igual a `'medida'`; não verifica a última atribuição nem o valor efetivo. (`bin/tests/test_lei_akita.sh:139-144`) |
| Decorador | **Recusa**: intervalo preso da função começa no menor `lineno` entre função e decoradores. (`bin/tests/test_lei_akita.sh:149-155`) |
| Continuação de atribuição com chamada | **Aceita**: todo o intervalo até `end_lineno` fica livre. (`bin/tests/test_lei_akita.sh:150-155`) |
| Continuação de import/def/class | **Recusa**: todo o intervalo fica preso. (`bin/tests/test_lei_akita.sh:148-155`) |
| Docstring inicial | **Aceita**; comentários anteriores não contam como elementos de `t.body`. (`bin/tests/test_lei_akita.sh:148-153`) |
| String que não é o primeiro nó do módulo | **Recusa**: `eh_doc` exige `i == 0`. (`bin/tests/test_lei_akita.sh:151-155`) |
| Comentário/branco dentro de def/class | **Recusa**: `soltas` perde para `preso`. (`bin/tests/test_lei_akita.sh:154-155`) |
| Hunk `-U0` com contagem zero | **Ignora corretamente esse lado**: `if n` impede cobrar a linha âncora; o outro lado continua sendo conferido. Não é, sozinho, um bypass. (`bin/tests/test_lei_akita.sh:171-175`) |
| Arquivo novo que se declara medida | **Aceita se todo o arquivo novo for livre**, inclusive atribuições com chamadas/monkeypatch; lado pai ausente é ignorado por `-0,0`. Novo arquivo com def/import/classe é recusado. Commit raiz sem primeiro pai é recusado. (`bin/tests/test_lei_akita.sh:158-175`) |
| Arquivo deletado e ainda ausente no ALVO | **Recusa** antes do diff: não existe declaração no ALVO. (`bin/tests/test_lei_akita.sh:131-144,158-159`) |
| Deleção histórica, arquivo restaurado no ALVO | **Pode aceitar**: declaração restaurada satisfaz a guarda; exclusão de arquivo composto somente por linhas livres passa, pois `+0,0` ignora AST ausente em `h`. Excluir função/import/classe continua recusado pelo lado pai. (`bin/tests/test_lei_akita.sh:139-175`) |
| Rename | `--no-renames` conserva origem e destino na lista; origem ausente no ALVO normalmente faz **recusar**. Se origem for restaurada no ALVO e ambos os lados forem compostos de linhas livres, pode **aceitar**. (`bin/tests/test_lei_akita.sh:139-175,238-242,255`) |
| Merge | Usa diff contra primeiro pai. Depois remove caminhos iguais a qualquer outro pai já publicado; diferenças inéditas restantes passam pelo mesmo callback vulnerável a atribuições executáveis. Não há aceitação geral de merges, mas tampouco proteção adicional dos RHS. (`bin/tests/test_lei_akita.sh:236-255`) |
| Diff bem-sucedido sem nenhum `@@` | **Aceita** depois da declaração e da existência do pai, pois o laço não roda e chega a `return True`. Não exige ao menos um hunk nem rejeita saída “Binary files differ”; alteração apenas de modo também não tem linhas para cobrar. (`bin/tests/test_lei_akita.sh:158-176`) |

A leitura no ALVO abre **classificação retroativa**: adicionar hoje a declaração a um arquivo de negócio faz commits anteriores de atribuições desse arquivo passarem como medida; tirar hoje a declaração faz commits anteriores de medida serem recusados. A remoção em `h` também pode passar se o ALVO posterior restaurou a declaração. Não migra a análise das linhas: os dois ASTs ainda são pai e `h`, mas a autorização vem do ALVO. (`bin/tests/test_lei_akita.sh:139-144,156-175,255`)

Confirmei em memória as classificações AST e, com `subprocess.run` simulado, os casos novo só atribuições=True; novo com def=False; deleção ausente no ALVO=False; deleção restaurada=True; diff sem hunk=True. Isso valida a implementação da função, **não** a produção real dessas saídas pelo Git nesta cópia sem `.git`. (`bin/tests/test_lei_akita.sh:139-176`; execução Git real desses cenários: [não verificado])

## 06/11 20:20 · sessao 42b265fa899f · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **“Medida” está semanticamente alargada:** os módulos dizem “NUMERO MEDIDO e texto de PROVA” (`app/core/placar_estrutural.py:33`, `app/core/espelho_verdade.py:25`), mas o selo libera qualquer `Assign`/`AnnAssign` sem lambda (`bin/tests/test_lei_akita.sh:152`). Isso inclui `CADENCIA_TERMOMETRO_DIAS = 7` (`app/core/placar_estrutural.py:61`), usada para decidir vencimento (`app/core/placar_estrutural.py:667`) e cor/veredito (`app/core/placar_estrutural.py:685`). Trocar a cadência recebe a exceção apesar de alterar decisão do leitor, sem tocar `def`. (`bin/tests/test_lei_akita.sh:188`)

- **Outros dados liberados também decidem:** `RESULTADOS` alimenta estado efetivo e papel (`app/core/placar_estrutural.py:643`), e a linha pública conta estados usando constantes globais (`app/core/placar_estrutural.py:699`). No espelho, `ETAPAS` e `FEITO` determinam feitos e percentual (`app/core/espelho_verdade.py:205`, `app/core/espelho_verdade.py:210`, `app/core/espelho_verdade.py:213`). O selo não distingue esses controles de número/prova. (`bin/tests/test_lei_akita.sh:152`)

- **Escritores de `PAPEL_DO_MODULO`:** declarações reais em `app/core/placar_estrutural.py:38` e `app/core/espelho_verdade.py:30`; fixture do MORDE escreve o literal em `bin/tests/test_lei_akita.sh:314` e produz sua retirada em `bin/tests/test_lei_akita.sh:338`. **Leitor semântico:** `declara_medida`, por AST no `ALVO`, em `bin/tests/test_lei_akita.sh:139`. Não lê o valor executado: basta existir algum `Assign` correspondente no corpo. (`bin/tests/test_lei_akita.sh:141`)

- **Escritor de `medida_do_commit`:** definição em `bin/tests/test_lei_akita.sh:156`; **leitores/chamadores:** universo real em `bin/tests/test_lei_akita.sh:255`; casos MORDE em `bin/tests/test_lei_akita.sh:347`; retirada da declaração no ALVO em `bin/tests/test_lei_akita.sh:351`. A exceção é aplicada por `_julgar_linha` ao callback `medida`, em `bin/tests/test_lei_akita.sh:188`.

- **O nome novo, isoladamente, não interfere nos leitores identificados:** `gerar_estado` executa os módulos e escolhe atributos explícitos, sem enumerar nomes globais (`bin/gerar_estado.py:75`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:142`, `bin/gerar_estado.py:145`). Core testes escolhem `RESULTADOS`, `ETAPAS`, funções e constantes explícitas (`app/core/tests/test_r1_termometro_uma_medicao.py:40`, `app/core/tests/test_r1_termometro_uma_medicao.py:44`, `app/core/tests/test_selo_r6_frase_do_juiz.py:16`, `app/core/tests/test_l114_placar_papel_e_termometro.py:30`). O selo de afirmações lê só `mod.RESULTADOS` (`bin/tests/test_placar_nao_afirma_fora_do_ar.sh:80`); handoff usa regex específica da frase dos contratos (`bin/handoff_sessao.sh:178`). Contrato que enumere exatamente o conjunto de nomes globais desses dois módulos não foi encontrado; ausência absoluta e execução integral dos testes: **[não verificado]**.

- **`regua` e pre-push são consumidores indiretos do resultado do selo:** a régua percorre `bin/tests/test_*.sh` (`bin/regua.sh:138`) e o pre-push chama especificamente o selo com o SHA empurrado (`bin/pre-push.sh:38`); nenhum desses trechos interpreta `PAPEL_DO_MODULO`. (`bin/regua.sh:140`, `bin/pre-push.sh:38`)

- **Juiz duplicado/derivação paralela:** a mudança reaproveita `_julgar_linha` e o universo existente, adicionando callback e predicado de elegibilidade (`bin/tests/test_lei_akita.sh:177`, `bin/tests/test_lei_akita.sh:255`). Não demonstra uma segunda autoridade de cálculo/dinheiro/batida. O registro vivo define autoridades por pergunta (`app/core/juizes.py:3`, `app/core/juizes.py:7`), enquanto o anexo de consulta declara sua própria natureza de lookup (`app/docs/JUIZES.md:7`). A reprovação demonstrável é a divergência entre a promessa de “medida” e a sintaxe aceita, acima. (`bin/tests/test_lei_akita.sh:124`, `bin/tests/test_lei_akita.sh:152`)

## 06/11 20:19 · sessao edb5b413dba1 · - · - · origem=Ronald (tmux)

O MORDE reutiliza o juiz real `julgar` e `medida_do_commit`; o portão rejeita qualquer `MORDE_FALHOU`, embora `MORDE_OK` seja impresso ao final independentemente das falhas. Portanto a impressão de `MORDE_OK` isoladamente não prova sucesso. (`bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:473`, `bin/tests/test_lei_akita.sh:488`)

| Caso | Mutante que o caso distingue, por inspeção |
|---|---|
| `so_numero`, `comentario`, `doc` verdes | Aceite desligado, ou recusar toda atribuição/comentário/docstring. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:341`) |
| `na_def` vermelho | Liberar corpo de função; também mata “todas as linhas livres”. (`bin/tests/test_lei_akita.sh:333`, `bin/tests/test_lei_akita.sh:342`) |
| `com_import` vermelho | Liberar import de topo. (`bin/tests/test_lei_akita.sh:334`, `bin/tests/test_lei_akita.sh:342`) |
| `com_lambda` vermelho | Remover veto a lambda dentro de atribuição. (`bin/tests/test_lei_akita.sh:335`, `bin/tests/test_lei_akita.sh:342`) |
| `sem_decl` vermelho | Dispensar declaração. (`bin/tests/test_lei_akita.sh:336`, `bin/tests/test_lei_akita.sh:343`) |
| `mais_fech` vermelho | Ignorar outro arquivo de negócio ou examinar só o primeiro arquivo. (`bin/tests/test_lei_akita.sh:337`, `bin/tests/test_lei_akita.sh:343`) |
| Número com `n/a (instrumento)` vermelho | Estender a exceção ao outro rótulo. (`bin/tests/test_lei_akita.sh:344`) |
| ALVO sem declaração, julgando `so_numero`, vermelho | Ler declaração em `h` em vez de ALVO. (`bin/tests/test_lei_akita.sh:349`) |

São distinções deduzidas do código; a execução dos dois mutantes alegados, seus resultados vermelhos e os três commits verdes permanecem **[não verificado]**. O caminho `logs/akmed/red_vivo.log` citado no anexo não está disponível nesta cópia **[não verificado]**.

O bloco não cobre individualmente classe, decorador, docstring não inicial, chamada em atribuição, `AnnAssign`, reatribuição de nome funcional, monkeypatch, declaração anulada depois, novo arquivo, exclusão, rename, merge de medida, erro git nem concessão retroativa de declaração pelo ALVO. Seu catálogo completo contém somente os nove casos e a checagem adicional descritos acima. (`bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:349`)

Há uma atribuição multilinha positiva (`PL`/`so_numero`) e inserção de comentário; falta caso negativo que dependa exclusivamente do lado antigo do diff. `na_def` tem linhas presas nos dois lados, enquanto import e lambda são negativos pelo lado novo. (`bin/tests/test_lei_akita.sh:314`, `bin/tests/test_lei_akita.sh:330`, `bin/tests/test_lei_akita.sh:331`, `bin/tests/test_lei_akita.sh:333`)

Os casos de medida não testam a integração com a enumeração do universo: os commits recebem mensagem `"c"` e o teste chama diretamente `julgar` com mensagem e lista de arquivos construídas manualmente. (`bin/tests/test_lei_akita.sh:325`, `bin/tests/test_lei_akita.sh:340`, `bin/tests/test_lei_akita.sh:347`)

A cláusula “erro de git = PADRAO_ERRO” não é cumprida literalmente por todas as novas operações: falha de `git show` vira AST ausente; qualquer falha de `rev-parse h^1` vira `False`; apenas o novo `git diff` levanta explicitamente `PADRAO_ERRO`. Esses dois primeiros caminhos normalmente recusam a exceção, mas não emitem a classificação de erro prometida. (`bin/tests/test_lei_akita.sh:131`, `bin/tests/test_lei_akita.sh:160`, `bin/tests/test_lei_akita.sh:164`)

## 06/11 20:16 · sessao 3375283c7246 · - · - · origem=Ronald (tmux)

Chamadores diretos de produção de `minutos_noturnos`:

- `MotorBase.calcular_noturnas`, definição `app/ponto/motor_calculo_v2.py:969`, chamada `:983`: recebe `[e,s]` após `_janela_do_par`, aplicado em `:2759`, `:2886`, `:3211`. Os escritores chamam essa função em `:2792`, `:2896`, `:3247`.
- `he_noturna`, definição `app/ponto/motor_calculo_v2.py:409`, chamada `:441`: recebe `[_inicio_do_trecho_extra(...), saida]`, calculado em `:440`. `_aplicar_he_noturna` passa `periodo.entrada/saida` em `:1015`.
- `minutos_noturnos_dos_intervalos`, definição `app/ponto/motor_calculo_v2.py:121`: mede prefixos `[entrada,b]` e `[entrada,a]` em `:191–192`, além do total `[entrada,saida]` em `:196`.
- `noturnas_do_dia`, definição `app/ponto/calculador/regras.py:66`, chamada `:85`: recebe cada par fornecido pelo calculador. O caminho de produção pode fornecer segmentos cortados pelos intervalos: `app/ponto/calculador/alimentacao.py:128`, `:133`, `:137`, `:158`; a função `noturno_no_par` fornece o par inteiro: `app/ponto/calculador/periodo.py:488`, `:510`.

`MotorPlantao` usa a conta de `Motor12x36.calcular_periodo`, por delegação explícita em `app/ponto/motor_calculo_v2.py:3510–3511`; portanto não constitui outro chamador direto.

O universo real exige mais que `batidas_apuraveis + parear_turnos`: `calcular_mes` remove duplicatas em `app/ponto/motor_calculo_v2.py:2351`, monta períodos em `:2354`, e `_periodos_via_autoridade` aplica `periodos_de_turnos(turnos_via_autoridade(...))` em `:530`, seguido de `_periodos_pelo_marco` em `:531`. O filtro por dia da jornada antecede a chamada de `calcular_periodo`: `:2403`, `:2429–2432`, `:2458`.

Recomendação de instrumentação: `sys.setprofile` observando os objetos `__code__` reais permite coletar os argumentos e retornos de `minutos_noturnos`, sem reconstruir os recortes e sem alterar módulos. Contar separadamente os canais principal, HE e prefixos de intervalos; prefixos não são novos turnos. A HE pode ser chamada novamente sobre o último período após agregação diária/semanal: `app/ponto/motor_calculo_v2.py:3147`, `:3178`, `:3399`.

Para o excesso de (b), reproduzir apenas a geometria da sobreposição, após verificar o limite de 26h de `app/ponto/motor_calculo_v2.py:94`. Para cada dia civil com a condição de prorrogação de `:110`, somar:

```python
max(0.0, (min(fim, meia_noite_seguinte)
          - max(inicio, inicio_do_dia, hora_22)).total_seconds() / 60)
```

As duas parcelas que produzem a duplicidade são explicitamente somadas em `app/ponto/motor_calculo_v2.py:112–116`.

O fechamento com `somente_leitura=True` e `lavrar_dias_pagos=False` não percorre o calculador. O chamador do calculador fica no ramo de lavratura em `app/ponto/services/fechamento.py:1253`, `:1255`, condicionado a competência ≥10/2026 em `:1249`. O calculador de HE chama a mesma `he_noturna` em `app/ponto/calculador/regras.py:832`, usando `he_pontas` formadas com as pontas efetivas em `app/ponto/calculador/alimentacao.py:83`, `:101–106`. Portanto medir somente o fechamento em leitura não autoriza imprimir zero para os chamadores do calculador.

## 06/11 20:16 · sessao 5f84fc60e33a · - · - · origem=Ronald (tmux)

Desenho recomendado para o SCRIPT 2:

1. Chamar `recalcular_fechamento_mes(mes, 2026, empresa_id=empresa, colaborador_ids=[pk], somente_leitura=True, lavrar_dias_pagos=False, retornar_objeto=True)`. O universo é `universo_da_competencia`; a flag `lavrar_dias_pagos=True` altera o comportamento da guarda de competências exportadas, portanto prejudica a equivalência do universo. `app/ponto/services/fechamento.py:373`, `app/ponto/services/fechamento.py:530`, `app/ponto/services/fechamento.py:545`.

2. Capturar `dict(frame.f_locals)` apenas no evento `return` de `recalcular_fechamento_mes.__code__`, mediante `sys.setprofile`. Isso entrega o `resultado` composto e os mapas usados pelo escritor de dias: `_ft_certa`, `_ft_feriado`, `_ft_sem_escala`, `_orfaos_por_dia`, `_prev_dia_m`, `_real_dia_m`. As entradas efetivas da lavratura estão em `app/ponto/services/fechamento.py:1185`.

3. Para obter valores diários sem copiar aritmética, chamar a função **real** `dia_pago.lavrar` sobre uma fachada temporária de `ponto.models.DiaPago`. A fachada instancia objetos reais não salvos; somente `.objects.filter(...).delete()` e `.objects.bulk_create(...)` são substituídos por operações em memória. A função real monta todos os números antes dessas duas operações. `app/ponto/services/dia_pago.py:135`, `app/ponto/services/dia_pago.py:173`, `app/ponto/services/dia_pago.py:267`, `app/ponto/services/dia_pago.py:283`, `app/ponto/services/dia_pago.py:333`, `app/ponto/services/dia_pago.py:357`.

```python
from unittest.mock import patch
import ponto.models as modelos
from ponto.services.dia_pago import lavrar

RealDiaPago = modelos.DiaPago
capturadas = []

class Memoria:
    def filter(self, **kwargs):
        return self

    def delete(self):
        return 0, {}

    def bulk_create(self, objetos):
        capturadas.extend(objetos)
        return objetos

class DiaPagoEmMemoria:
    TIPO_DIA = RealDiaPago.TIPO_DIA
    TIPO_AJUSTE = RealDiaPago.TIPO_AJUSTE
    objects = Memoria()

    def __new__(cls, *args, **kwargs):
        return RealDiaPago(*args, **kwargs)

# L = snapshot dos locals de recalcular_fechamento_mes,
# rodado exclusivamente para este colaborador.
r = L["resultado"]
with patch.object(modelos, "DiaPago", DiaPagoEmMemoria):
    lavrar(
        colab, mes, 2026,
        periodos=r.periodos,
        ft_certa=L["_ft_certa"],
        ft_feriado=L["_ft_feriado"],
        ft_sem_escala=L["_ft_sem_escala"],
        orfaos_por_dia=L["_orfaos_por_dia"],
        previsto_por_dia=L["_prev_dia_m"],
        realizado_por_dia=L["_real_dia_m"],
        dia_da_jornada=getattr(r, "dia_da_jornada", None),
        versao="motor",
    )
```

4. Detectar aberto pela saída **real** de `MotorBase._turnos_do_juiz`, capturada no evento `return` pelo objeto de código. O retorno contém `Turno(data_turno, entrada, saida, batidas, aberto, cross_meianoite)`; filtrar `entrada is not None`, `saida is None`, `aberto`, e `data_ini <= data_turno <= data_fim` dos argumentos desse frame. A geometria desta porta responde pelo dia e pelo intervalo; os períodos e minutos continuam sendo os do motor. `app/ponto/turnos.py:41`, `app/ponto/motor_calculo_v2.py:867`, `app/ponto/motor_calculo_v2.py:882`, `app/ponto/motor_calculo_v2.py:2346`.

5. Como informação adicional, comparar com `p.turno_aberto` nos períodos finais, agrupados por `resultado.dia_da_jornada[id(p)]`. O mapa preserva os mesmos objetos na composição de fatias. `app/ponto/services/fechamento.py:292`, `app/ponto/services/fechamento.py:302`.

O código já admite crédito de horas em período ainda marcado aberto, a partir dos pares fechados existentes no turno; isso impede interpretar automaticamente `aberto E horas>0` como pagamento de um dia cheio. A conclusão para col882 e para a frota depende da execução da sonda, ainda [não verificado]. `app/ponto/motor_calculo_v2.py:2495`, `app/ponto/motor_calculo_v2.py:2514`, `app/ponto/motor_calculo_v2.py:2558`.

Erros internos do fechamento podem ser capturados e consumidos pelo próprio serviço; retorno ausente para o colaborador deve virar erro, nunca zero. A grade também pode ficar sem resposta (`minutos_* = None`), e os mapas vazios não devem ser apresentados como minutos zero. `app/ponto/services/fechamento.py:1071`, `app/ponto/services/fechamento.py:1078`, `app/ponto/services/fechamento.py:1287`.

## 06/11 20:11 · sessao f922a2f8576e · PUSH-PRESO-14 · verificador · origem=Code

**Meu veredito: os três commits R1 publicam ou corrigem medições; o quarto implementa idempotência pela chave do fato e reutiliza autoridades existentes.** Evidências abaixo usam `hash:arquivo:linha` com a numeração nova dos diffs anexados. (`36c90132:app/core/placar_estrutural.py:106–120`; `6ed6adbe:app/core/placar_estrutural.py:106–119`; `a16637ca:app/core/placar_estrutural.py:108–118`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130–139,190–213`)

1. **`36c90132` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele substitui números e provas, troca `fonte_do_medido` e remove afirmações sobre destinos que a nova medição não responde; os estados dos itens permanecem iguais. (`36c90132:app/core/placar_estrutural.py:106–120`; `36c90132:app/core/espelho_verdade.py:168–180`)

   Dois candidatos seriam **Single Source of Truth / fonte única** e **Architectural Invariants + Fitness Functions**, existentes no vocabulário. Porém, atualizar referências para a mesma medição não implementa uma autoridade única; publicar o resultado da SOMA também não implementa sua verificação. Neste diff, ambos permanecem conteúdo do placar. (`app/docs/GLOSSARIO.md:11,14`; `36c90132:app/core/placar_estrutural.py:106–120`)

2. **`6ed6adbe` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele reorganiza provas para apresentar números primeiro, restringe afirmações ao universo medido e corrige a atribuição da conferência da SOMA para a sonda. (`6ed6adbe:app/core/espelho_verdade.py:49,82,168–180`; `6ed6adbe:app/core/placar_estrutural.py:106–119`)

   Os candidatos seriam novamente **fonte única** e **fitness function / selo**. O diff corrige a descrição das fontes e verificações, mas não altera o mecanismo que impõe uma fonte única nem acrescenta a verificação executável; as mudanças de teste narradas na mensagem não aparecem no diff anexado. (`app/docs/GLOSSARIO.md:11`; `app/docs/PLANO-PADROES.md:48`; `6ed6adbe:app/core/placar_estrutural.py:106–119`; `6ed6adbe:app/core/espelho_verdade.py:49,82,168–180`)

3. **`a16637ca` — nenhum termo arquitetural é realizado pelo diff mostrado.** Ele compacta números e ressalvas nas provas, corrige `SOMA.fonte` e documenta a partição `pk % 6`; não implementa essa partição. A compactação tem efeito concreto na publicação porque o leitor recorta a prova em 150 caracteres. (`a16637ca:app/core/espelho_verdade.py:49,82,168–180`; `a16637ca:app/core/placar_estrutural.py:108–118`; `bin/gerar_estado.py:164`)

   Dois candidatos seriam **fitness function / selo** e **Shadow mode / Parallel run — sombra**. O diff apenas relata a conferência e a execução em sombra; não acrescenta um selo nem um caminho de execução paralela. (`app/docs/PLANO-PADROES.md:48`; `app/docs/GLOSSARIO.md:19`; `a16637ca:app/core/placar_estrutural.py:108–118`)

4. **`5a3f8d0e` — realiza principalmente `idempotencia pela chave do fato` / `dedup do fato`.** A chave contém marca, colaborador e competência; a busca acrescenta a âncora da empresa/competência e considera cabeças não canceladas, inclusive feitas. Dentro da transação, trava o colaborador, consulta novamente a existência e só cria quando ausente. Isso implementa o mecanismo, além de declará-lo. (`app/docs/GLOSSARIO.md:27`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53–54,173–174,190–213,240–243`)

   O segundo candidato, **single source of truth / um juiz por pergunta**, também é realizado no consumo das autoridades: PAGO vem de `valores_do_gravado`, DEVIDO do recalculador existente em modo leitura, DIFERE de `_diferente` e ENTREGUE de `competencia_entregue`. Para escolher um único rótulo principal, escolheria **`padrao=idempotencia pela chave do fato`**, pela implementação explícita de deduplicação serializada. (`app/docs/PLANO-PADROES.md:46–47`; `5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:109–139,190–213`)

Quanto ao comportamento:

- **Os três R1 alteram informação e apresentação do placar, sem modificar cálculo financeiro ou registro de batidas.** No primeiro, trocar a fonte também pode mudar a data reconhecida e, consequentemente, o veredito de cadência do termômetro; portanto há efeito no placar além da prosa. (`36c90132:app/core/placar_estrutural.py:111`; `6ed6adbe:app/core/espelho_verdade.py:49`; `a16637ca:app/core/espelho_verdade.py:168`; `bin/gerar_estado.py:47–56`; `app/core/placar_estrutural.py:676–684`)
- **O quarto não é só medida:** por padrão é leitura, mas `--escrever` cria pautas DP e `--reverter` cancela pautas. Ele consulta o cálculo financeiro em `somente_leitura=True`, sem habilitar a lavratura de `DiaPago`; não introduz alteração de regra de dinheiro nem escrita de batidas. (`5a3f8d0e:app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135,161–188,207–217,259–283`; `app/ponto/services/fechamento.py:439–442,1112–1128,1175`)
- Execução efetiva desses caminhos e efeitos observados em produção: **[não verificado]**.

A recusa mecânica dos três primeiros decorre da classificação por caminho: `app/`, fora de testes e documentos, é “negócio” e recusa `n/a`. Isso não demonstra que realizem um padrão; a própria L-123 exige conferir o rótulo contra o diff. (`bin/tests/test_lei_akita.sh:122–135`; `app/docs/PLANO-PADROES.md:53,58`)

_2522 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
