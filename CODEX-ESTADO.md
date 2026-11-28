# CODEX-ESTADO

> Ultima resposta processada: **28/11/2026 04:02** · publicado em **28/11/2026 04:09** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 28/11 04:02 · sessao f08fe66d4eaf · CUSTO-DE-COORDENACAO · verificador · origem=Code

**Parecer pré-verificador 5(a), volta 1: bloqueia.** Há caminhos em que o B3 aceita como mão dupla um merge que toca matéria de mão única, o CENSO aprova pergunta de lei sem corte e a cura do selo deixa leitores reais fora da varredura. `bin/mao_unica.py:39` · `bin/disjuntor.py:1303` · `bin/disjuntor.py:1681` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:63`

Conferi o diff fornecido contra a cópia em leitura. O selo do RELATO saiu com rc 0, um leitor e quatro contratos. A sintaxe dos seis scripts shell alterados passou em `bash -n`. A cópia veio sem `.git`. A cadeia de commits, os demais rc de execução, os falsos de host, os avais em `logs/`, o dossiê e as duas métricas de produção ficam [nao verificado]. `bin/tests/test_relato_guarda_pedido_de_patch.sh:228` · `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:236`

## 1. Aderência ao brief

1. **Nit — F1 (1), ficaram restos do freio.** Os cortes de cota saíram da decisão do Agent, do Stop e do Codex. A palavra `escada` continua acionando o B4, o parser compartilhado continua alimentando ANEXO-ENXUTO e o ESTADO conserva as duas medidas. O caso de 99% cobre o PreToolUse do Agent e a permissão Bash para chamar o wrapper. `VOLTA_CONTRATO` ainda diz que acima do orçamento se vai sem Codex, e `codex_decide` ainda repassa relógio e transcript a uma função que deixou de usá-los. As referências restantes a B2 no hook e no disjuntor são lápides. `bin/disjuntor.py:1422` · `bin/disjuntor.py:1449` · `bin/disjuntor.py:1481` · `bin/disjuntor.py:1580` · `bin/disjuntor.py:1603` · `bin/disjuntor.py:2648` · `bin/disjuntor.py:2684` · `bin/tests/test_disjuntor.sh:525`

2. **Bloqueia — F1 (2a–e), o conteúdo ainda depende da forma do rótulo.** O coletor pega `c1/brief.md` sem citação, usa o `mtime` do descritor, herda referências e exige decisões no vivo. Também recusa ausência de brief, de referências e de corte nas perguntas que reconhece. Contraexemplo verificado em memória: um brief com leitores, escritores e decisões válidos, seguido de `### Perguntas de lei` e `- Pode gravar?`, é aprovado. O cabeçalho sem `:` é descartado e a lista vira zero perguntas. Uma linha de prosa entre `perguntas de lei:` e a lista também zera o rótulo. Além do pedido, o glob restringe o universo a `.md` e todo empate de `mtime` vira vários briefs vivos. Um único `brief.txt` válido fica fora. `bin/disjuntor.py:1630` · `bin/disjuntor.py:1656` · `bin/disjuntor.py:1696` · `bin/disjuntor.py:1746` · `bin/tests/test_censo_antes_da_vaga.sh:151` · `bin/tests/test_censo_antes_da_vaga.sh:173`

3. **Bloqueia — F1 (2d), o caminho pode escolher outra obra.** Com `OBRA-X` e `CODEX-OBRA-X` abertas, `/home/ronald/wt-codex-obra-x-000001` resolve para `CODEX-OBRA-X`: o código disputa o slug bruto com o slug sem `codex-` e prefere o token mais longo. Se só `CODEX-OBRA-X` tiver CENSO aprovado, o Agent da cópia de `OBRA-X` recebe a aprovação da outra obra. A via wrapper mantém o ID declarado e passa a mesma leitura da fila ao juiz. `bin/disjuntor.py:1804` · `bin/disjuntor.py:2274` · `bin/disjuntor.py:2716` · `bin/codex_pergunta.sh:487`

4. **Fila — F1 (3), o universo do diff diverge do comando literal.** O brief pede `HEAD MERGE_HEAD`. O código usa a união de `HEAD...MERGE_HEAD` com `--cached HEAD` e soma a classificação de exclusões de arquivos presumidos de produção, coerente com a regra já escrita na PARADA 2. Exemplo da diferença: um `c.py` introduzido só na principal depois da bifurcação aparece como `D` entre pontas e fica fora da união. Uma edição nova de resolução entra pelo índice. O caminho de mão dupla grava o aviso e retorna antes de `estado_b3`; o merge continua exigindo `OBRA:`. As perdas de guarda estão na pergunta 2. `bin/mao_unica.py:30` · `bin/mao_unica.py:85` · `bin/tests/test_mao_unica.sh:47` · `bin/tests/test_mao_unica.sh:64` · `bin/disjuntor.py:1297` · `CLAUDE.md:278`

5. **Bloqueia — F1 (4), o Stop compara o texto apresentado, perde mudanças de estado e mantém o topo antigo em leitores do painel.** Trocar só a coluna de estado de `**em voo**` para `**em voo** volta 2` não gera aviso, e o selo exige esse silêncio. Uma nova reprovação da mesma obra já armada também pode manter o mesmo texto B3. `MERGE` e `ERRO-BOBO` aparecem como instruções estáticas dentro do B3, sem aviso próprio com estado próprio. No BACKLOG da cópia, `ORDEM-VIVA-1511-NOITE` segue como topo e o juiz retorna `ok`. Num exemplo com `**SUPERADA**`, o juiz esvazia `EM CURSO`, mas o handoff ainda imprime o ID em `ORDEM VIVA (topo declarado)` e a lista dos andáveis ainda o inclui. Com `SUPERADA` sem negrito, até `EM CURSO` permanece. `bin/hook_stop_fila1.py:56` · `bin/hook_stop_fila1.py:226` · `bin/hook_stop_fila1.py:251` · `bin/hook_stop_fila1.py:1385` · `bin/hook_stop_fila1.py:1596` · `bin/disjuntor.py:990` · `bin/disjuntor.py:1317` · `bin/tests/test_hook_stop_borda.sh:47` · `bin/handoff_sessao.sh:63` · `app/docs/BACKLOG.md:8` · `app/docs/BACKLOG.md:1926`

6. **Fila — documentação e Pronto.** O diff atualiza o PRINCÍPIO, a PARADA 4 e o DOSSIE/CENSO. A frase “cota 7d e orçamento Codex só no ESTADO” omite o aviso de cota do Agent na trilha. O selo de 60 Stops conta apenas `ALLOW AVISO fila 1 com item aberto`, e o ESTADO soma negativas de CENSO sem separar as de forma. Esses contadores deixam sem prova `< 50 avisos/dia` e `CENSO de forma = 0`. As alterações em `app/pautas/tests/` e no selo do RELATO pertencem às fatias adicionais identificadas na entrega. `CLAUDE.md:264` · `CLAUDE.md:288` · `CLAUDE.md:448` · `bin/disjuntor.py:1443` · `bin/disjuntor.py:2513` · `bin/tests/test_hook_stop_borda.sh:34` · `bin/tests/test_hook_stop_borda.sh:45` · `app/pautas/tests/test_contract_vocabulario.py:14` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:51`

## 2. Guarda de mão única

7. **Bloqueia — o classificador perde alterações no juiz da célula.** Uma mudança de uma linha em `EscalaColaborador.eh_dia_trabalho`, por exemplo trocar a leitura de `cel.trabalha`, toca a autoridade registrada da célula. O único caminho do diff, `app/escala/models.py`, resulta em mão dupla. Para uma obra já armada, o novo retorno `0` pula o B3. `CLAUDE.md:257` · `app/core/juizes.py:190` · `app/escala/models.py:1080` · `app/escala/models.py:1105` · `bin/mao_unica.py:39` · `bin/disjuntor.py:1303`

8. **Bloqueia — a representação do caminho pode enfraquecer até a regra literal.** `_name_status` lê texto com aspas e escapes sem decodificá-los. Com saída `M<TAB>"ponto/a\tb.py"` — `\t` representando um tab no nome — o primeiro segmento fica `"ponto` e o classificador devolve mão dupla. `app/relatorios/célula.py` também dá mão dupla mesmo quando fornecido já decodificado, pois o termo procurado é `celula`. O caso `None` preserva o B3 e uma exceção que escapa faz o hook recusar pelo rc. Os dois contraexemplos chegam como dados aceitos, portanto passam pelo retorno de mão dupla. `bin/mao_unica.py:19` · `bin/mao_unica.py:39` · `bin/mao_unica.py:55` · `bin/mao_unica.py:70` · `bin/disjuntor.py:1303` · `bin/commit_msg.sh:36`

9. **Bloqueia — a árvore julgada pode divergir da árvore que será gravada.** `commit_msg.sh` faz `git write-tree` com o ambiente recebido, depois apaga `GIT_INDEX_FILE`. O Python consulta novamente `--cached HEAD`. Num merge com alteração documental na raia e resolução em `app/ponto/` presente apenas no índice alternativo usado pelo commit, o B3 pode ver só a alteração documental. O teste de resolução usa um único índice, e o selo limpa `GIT_*` na preparação. `bin/commit_msg.sh:19` · `bin/commit_msg.sh:27` · `bin/mao_unica.py:94` · `bin/tests/test_mao_unica.sh:18` · `bin/tests/test_mao_unica.sh:69`

## 3. Escritores, leitores e significados

10. **Fila — inventário dos contratos.** As divergências de unidade, borda ou significado com contraexemplo estão nas linhas da tabela. O fuso efetivo dos processos em produção fica [nao verificado].

| Campo ou arquivo | Quem escreve, quem lê e qual é o contrato |
|---|---|
| `cota.json` | A status line grava `seven_day_pct` em %, `seven_day_resets_at` e `hora` em segundos Unix, além de `contexto_tokens`, `contexto_pct` e `session_id`. O disjuntor usa só o par semanal, com `reset > agora`. O aviso inclui `pct >= 75` e converte o restante em horas. O ESTADO arredonda o percentual para exibição. Contraexemplo de validação divergente: o produtor rejeita `bool`, mas o leitor aceita `true` como percentual num JSON legado ou inválido. `bin/statusline_cota.py:26` · `bin/statusline_cota.py:45` · `bin/disjuntor.py:1329` · `bin/disjuntor.py:1439` · `bin/disjuntor.py:2680` |
| Cota e orçamento Codex | O produtor externo do JSONL de sessão fica [nao verificado]. `leituras_semanais` lê `primary` de 10.080 minutos, timestamp convertido a epoch, porcentagem e reset. `orcamento_do_dia` usa meia-noite local, delta em pontos percentuais, orçamento em pp/dia, comparação estrita `>` e reset exclusivo. `ler` e `_codex_txt` publicam a medida. `bin/codex_limite_estado.py:131` · `bin/codex_limite_estado.py:165` · `bin/disjuntor.py:2441` · `bin/disjuntor.py:2648` |
| `disjuntor_estado.json` | `_com_estado` grava sob `flock`. B1 grava por agente `checado` em epoch, `disparado` e `motivo`; o Stop lê os disparados ainda vivos. B3 acumula IDs de reprovas e voltas, SHA de marcas consumidas e `disparado_em` em epoch. `decide_b3` libera com `mtime >= disparado_em` e SHA novo, e é consultado pelo Stop, PreToolUse e merge de mão única. `b4_para.desde` é escrito e lido por `b4_stop` como epoch do primeiro Stop da ESCADA; o `mtime` do handoff considera igualdade suficiente. O produtor de `b4.negou_desde` saiu. `bin/disjuntor.py:334` · `bin/disjuntor.py:570` · `bin/disjuntor.py:810` · `bin/disjuntor.py:988` · `bin/disjuntor.py:1024` · `bin/disjuntor.py:1080` · `bin/disjuntor.py:1460` · `bin/hook_stop_fila1.py:1020` · `bin/hook_stop_fila1.py:1385` · `bin/handoff_sessao.sh:330` |
| `triagem.log` e contador B3 | `triagem` escreve instante local, obra, ID da volta e veredito, usando o sidecar do wrapper. `_triagens` lê o último veredito por volta sob a trava do estado e usa `b3_triagem_linhas` como piso de integridade do total de linhas. `decide_b3` conta voltas procedentes distintas. `bin/disjuntor.py:868` · `bin/disjuntor.py:891` · `bin/disjuntor.py:920` · `bin/disjuntor.py:980` |
| `disjuntor.log` e ESTADO | `trilha` grava data local sem offset, com resolução de segundos. `ler` conta `DISJUNTOR`, `NEGOU`, `LIBERA` e falhas de RELATO. O B4 no placar agora conta disparos de ESCADA no **Stop**. A negativa ao Agent segue em `NEGOU` sem novo disparo B4. `_censo_24h` recorta `corte <= t <= agora`, embora um comentário declare limite inferior aberto. Contraexemplo: uma linha à hora exata do corte entra nos dois contadores e uma linha futura entra em `ler`, mas fica fora do CENSO. `bin/disjuntor.py:192` · `bin/disjuntor.py:1469` · `bin/disjuntor.py:2285` · `bin/disjuntor.py:2316` · `bin/disjuntor.py:2377` · `bin/disjuntor.py:2510` · `bin/disjuntor.py:2668` · `bin/gerar_estado.py:294` |
| Briefs, `CENSO APROVADO` e `chamadas.log` | O escritor concreto dos briefs vivos fica [nao verificado]. O CENSO lê conteúdo e `mtime` em segundos Unix, ordena por `mtime` e grava aprovação com obra, via e nomes dos briefs. O wrapper fornece PID e hash e grava `ini` e `fim` em hora local. `_linhas_chamadas` lê o formato e `_censo_24h` casa por obra, PID e hash, até 600 s antes do `ini`, com ambos os limites inclusivos. `APROVADO` do wrapper significa brief aceito. `ini` significa tentativa iniciada. `bin/disjuntor.py:1660` · `bin/disjuntor.py:1890` · `bin/disjuntor.py:2460` · `bin/disjuntor.py:2482` · `bin/disjuntor.py:2542` · `bin/codex_pergunta.sh:487` · `bin/codex_pergunta.sh:566` · `bin/codex_pergunta.sh:613` · `bin/codex_limite_estado.py:49` |
| RELATO, marcas e BACKLOG | `relato_negacao` formata a negativa e usa `esteira_vigia.no_relato`. A marca `PAREI: censo-…` usa `no_topo`, sob a mesma trava, e `prova_parei_censo` exige forma literal e processo vivo. Stop, handoff e alarme consultam essa porta. B3 lê `DESENHO` e `ERRO-BOBO` pelo topo e compara a marca com a fotografia do disparo. `ORDEM-VIVA-TOPO` tem escrita manual declarada e juiz `_item_da_ordem_viva`, consumido por Stop, handoff e ESTADO. As marcas de `.rc` de `ESPERA_UNICA` são lidas por obra, caminho e `HH:MM` local do `mtime`; o produtor dessas linhas específicas fica [nao verificado]. `bin/disjuntor.py:273` · `bin/disjuntor.py:300` · `app/core/esteira_vigia.py:846` · `bin/hook_stop_fila1.py:773` · `bin/handoff_sessao.sh:139` · `bin/alarme_sessao_ociosa.py:141` · `bin/disjuntor.py:723` · `bin/disjuntor.py:1024` · `bin/gerar_estado.py:233` · `bin/interrompidas_estado.py:93` · `bin/disjuntor.py:1963` · `bin/rc_sem_colher.py:25` · `bin/rc_sem_colher.py:76` |
| `hook_stop_borda.tsv`, trilha e saída do Stop | `_avisos_por_borda` lê via `_borda_le` e grava `tipo\|obra<TAB>sha1[:16]`. `_borda_fecha` apaga a memória nas saídas antecipadas. `main` consome os avisos novos. `_trilha` grava hora local, decisão, motivo truncado, `item` truncado e HEAD. O cliente recebe JSON `decision=block` ou silêncio de stdout com rc 0. O auxiliar dos selos lê ambos os `ALLOW` da fila como `siga-aviso`, inclusive o que significa silêncio de aviso. `bin/hook_stop_fila1.py:39` · `bin/hook_stop_fila1.py:137` · `bin/hook_stop_fila1.py:884` · `bin/hook_stop_fila1.py:918` · `bin/hook_stop_fila1.py:1530` · `bin/hook_stop_fila1.py:1588` · `.claude/settings.json:9` · `bin/tests/_saida_do_stop.sh:11` |

## 4. Classes de reprovação

11. **Bloqueia — segunda leitura da fonte e transição sem trava.** O Stop calcula `item` a partir de uma leitura do BACKLOG e relê o arquivo para a chave. Se o marcador passar de `A` para `B` entre as duas leituras, grava a impressão de `A` sob `FILA1|B`. Para inferir a obra do Agent, o CENSO também lê a fila uma vez na inferência e outra na porta. Na borda, dois Stops podem ler o mesmo TSV anterior e ambos emitir o mesmo aviso. `os.replace` protege a troca do arquivo. A sequência de ler, decidir e gravar pode intercalar com outra sequência ou com a remoção no `finally`. `bin/hook_stop_fila1.py:313` · `bin/hook_stop_fila1.py:890` · `bin/hook_stop_fila1.py:918` · `bin/hook_stop_fila1.py:1565` · `bin/hook_stop_fila1.py:1596` · `bin/disjuntor.py:1812` · `bin/disjuntor.py:1875` · `bin/disjuntor.py:2276`

12. **Bloqueia — o RED da cura passa com a porta sempre falsa.** Substituí `_alcanca_o_vivo` por `return False` em memória. Resultado: `ZERO DECLARADO`, rc 0. A saída pelo universo vazio vem antes dos nove casos que deveriam julgar a porta, e apaga da verificação até o leitor real que cobra quatro contratos. `bin/tests/test_relato_guarda_pedido_de_patch.sh:189` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:215` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:258` · `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:236`

13. **Fila — há mutantes com caso correspondente e lacunas concretas.** Mão única sempre falsa, herança desligada, inferência desligada, aviso repetido, marcador superado e volta do freio têm casos declarados que exercitam a mudança. O selo da borda aceita a perda da coluna de estado e testa `parado` só pelo retorno do juiz, por isso deixa passar o topo antigo no handoff. O mutante de freio Codex consulta a **cota Claude**, e a checagem AST vigia uma lista fixa de funções. Um freio em `_b4_so_aviso` que negasse só a 100% passaria pelo AST e pelos percentuais exercitados. `bin/tests/test_mao_unica.sh:129` · `bin/tests/test_censo_antes_da_vaga.sh:760` · `bin/tests/test_hook_stop_borda.sh:47` · `bin/tests/test_hook_stop_borda.sh:101` · `bin/tests/test_hook_stop_borda.sh:139` · `bin/tests/test_disjuntor.sh:451` · `bin/tests/test_disjuntor.sh:531` · `bin/tests/test_disjuntor.sh:553` · `bin/tests/test_disjuntor.sh:1655`

14. **Fila — allowlists, juízes, relógio e `except`.** Há duas exceções de classificação: os três caminhos de diário da F4 e a presunção de que `tests/`, `test*.py` e `.md` estão fora de produção para fins de exclusão. Exemplo do segundo caso: um módulo importado em produção chamado `app/core/testemunho.py` seria dispensado dessa guarda. No diff fornecido, contei **0 tratadores novos**, **1 `except` reposicionado** na F4 e **1 tratador removido** do B4. A pergunta do classificador é sobre o diff, distinta das perguntas de domínio do registro. O relógio novo do merge continua vindo da entrada CLI. A dupla leitura de relógio do Stop e a leitura própria de `ler_chamadas` já aparecem nas rotas existentes. `app/pautas/tests/test_contract_vocabulario.py:29` · `app/pautas/tests/test_contract_vocabulario.py:47` · `bin/mao_unica.py:9` · `bin/mao_unica.py:23` · `app/core/juizes.py:190` · `bin/disjuntor.py:2285` · `bin/disjuntor.py:2776` · `bin/hook_stop_fila1.py:1381` · `bin/hook_stop_fila1.py:1537` · `bin/hook_stop_fila1.py:1589` · `bin/codex_limite_estado.py:71`

## 5. Cura de `_alcanca_o_vivo`

15. **Bloqueia — uma lista de inclusão é tomada por exclusão.** A forma abaixo lê o RELATO vivo se `paths` contém os caminhos sob `APP`, como numa varredura filtrada. A porta devolveu `False` em memória. O único uso de `D` está à direita de `in`, e a análise ignora que esse ramo executa a leitura. A forma também fica fora pela peneira anterior, que exige o literal isolado `'RELATO.md'`. `bin/tests/test_relato_guarda_pedido_de_patch.sh:63` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:84` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:189`

    ```python
    D = ('docs/RELATO.md',)
    for rel in paths:
        if rel in D:
            relato = open(os.path.join(APP, rel), encoding='utf-8').read()
    ```

16. **Bloqueia — `a+` também lê e passa pela peneira anterior.** `with open(os.path.join(APP, 'docs', 'RELATO.md'), 'a+') as f:`, seguido de `f.seek(0)` e `f.read()`, foi classificado `False` pela porta em memória. O teste de modo vê o `a` e ignora o `+`. Esse contraexemplo contém `'RELATO.md'` isolado, portanto a exclusão vem da cura. O leitor real encontrado na cópia usa `io.open(...).read()` e entra. `bin/tests/test_relato_guarda_pedido_de_patch.sh:71` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:95` · `bin/tests/test_relato_guarda_pedido_de_patch.sh:189` · `app/ponto/tests/test_tela_gestao_he_fatia2_lote_limite.py:236`

## 6. F4 e o vocabulário

17. **Resposta: sim, dentro do universo varrido. Fila — cobertura parcial da lista.** Uma docstring Python ou prosa em `app/docs/OUTRO.md` contendo `post-it` derruba o teste. Ele varre `.py`, `.html`, `.js` e `.md` sob `app/`, com os filtros gerais de diretório e a exclusão do nome do próprio selo. Quem define diário de máquina é a tupla `DIARIOS_DE_MAQUINA`, por três caminhos exatos: `app/docs/RELATO.md`, `app/docs/RELATO-ARQUIVO.md` e `app/docs/HANDOFF-SESSAO.md`. A dispensa vale para o arquivo inteiro, inclusive prosa escrita à mão. O código cita o aval F4, cujo teor fica [nao verificado]. O fixture cobre só o primeiro diário. O mutante que remove a dispensa do arquivo morto e do handoff sobreviveria nesta cópia: a leitura direta dos dois arquivos encontrou zero ocorrências pelos padrões do selo. `app/pautas/tests/test_contract_vocabulario.py:14` · `app/pautas/tests/test_contract_vocabulario.py:22` · `app/pautas/tests/test_contract_vocabulario.py:29` · `app/pautas/tests/test_contract_vocabulario.py:39` · `app/pautas/tests/test_contract_vocabulario.py:66` · `CLAUDE.md:500` · `bin/handoff_sessao.sh:330`

## 7. Outros defeitos

18. **Bloqueia — o Stop pode usar a própria atividade como prova de trabalho.** `hook_stop_borda.tsv` ficou fora dos arquivos recusados por `_prova_arquivo`. Com `ESPERANDO: agente-x | arquivo=logs/hook_stop_borda.tsv`, se o TSV já existe, um Stop registra seu `mtime` e segue sem prova. O próprio Stop substitui o TSV mesmo com conteúdo idêntico. No próximo Stop, o `mtime` maior pode provar a espera sem atividade do agente. `bin/hook_stop_fila1.py:644` · `bin/hook_stop_fila1.py:666` · `bin/hook_stop_fila1.py:701` · `bin/hook_stop_fila1.py:831` · `bin/hook_stop_fila1.py:930` · `bin/hook_stop_fila1.py:1560`

19. *

[truncado em 20000 de 20434 caracteres]

## 28/11 02:29 · sessao a7747a4bca90 · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

```python
# SONDA_ARVORE=/caminho/da/copia/app bin/sonda_leitura.sh --sombra /caminho/s12.py
# O executor monta /app e /sonda em leitura (bin/sonda_leitura.sh:226-240).
# O executor prepara credencial e grants, copia a sonda para um temporário e
# grava a trilha (bin/sonda_leitura.sh:115-143,153-181,216-218,245-250).

import logging
import sys
from collections import Counter
from datetime import date

from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

# Guarda da sombra (app/config/settings/sombra.py:29,49-50,60).
if (
    getattr(settings, "HASNER_SOMBRA", False) is not True
    or connection.settings_dict["NAME"] != "sombra"
):
    raise SystemExit(1)


def falha_sem_dados(tipo, _valor, _traceback):
    print(f"S12_FALHA|{tipo.__name__}", file=sys.stderr)


# Suprime logs e mensagens de falha que podem conter nomes
# (app/ponto/services/fechamento.py:1327-1332, app/folha/export.py:23-33).
logging.disable(logging.CRITICAL)
sys.excepthook = falha_sem_dados

from colaboradores.models import Empresa
from folha.export import (
    classificar_export,
    janela_competencia,
    montar_linhas_export,
    validar_layout_dominio,
)
from ponto.services.fechamento import recalcular_fechamento_mes


def tipo_layout(erro):
    # Rótulos derivados dos tipos fixos do validador (app/folha/export.py:266-307).
    if erro.startswith("reg10 campo "):
        campo = erro.split(" ", 3)[2]
        return f"reg10_campo_{campo}_nao_numerico"
    for prefixo, tipo in (
        ("reg10 len ", "reg10_len"),
        ("reg10 const ", "reg10_const"),
        ("reg11 soma ", "reg11_soma_8792"),
        ("reg11 len ", "reg11_len"),
        ("reg11 malformado:", "reg11_malformado"),
        ("reg11 data invalida:", "reg11_data_invalida"),
        ("linha desconhecida:", "linha_desconhecida"),
    ):
        if erro.startswith(prefixo):
            return tipo
    raise SystemExit("S12_FALHA|tipo_layout_desconhecido")


mes, ano = 11, 2026
erros_por_col = []

# Schema e READ ONLY do molde (bin/sonda_leitura.sh:239-240,
# bin/gate_cobranca.sh:23-25). REPEATABLE READ mantém o mesmo snapshot
# nas remontagens de diagnóstico (app/folha/export.py:720-754).
with schema_context("juliani"), transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY, ISOLATION LEVEL REPEATABLE READ")

    # Lookup por pk da entrega (app/folha/views.py:123).
    empresa = Empresa.objects.get(pk=2)

    # Guarda da janela do anexo (app/folha/export.py:47-51, app/ponto/janelas.py:36-57).
    if janela_competencia(empresa, mes, ano) != (date(2026, 10, 21), date(2026, 11, 20)):
        raise SystemExit(2)

    # Mesma chamada da entrega (app/folha/views.py:215-217,
    # app/ponto/services/fechamento.py:468-471,1152-1160,1333-1334).
    fechamentos = list(
        recalcular_fechamento_mes(
            mes, ano, empresa_id=empresa.pk, somente_leitura=True, retornar_objeto=True
        ).values()
    )

    # Mesmo veredito da entrega (app/folha/views.py:219-220, app/folha/export.py:774-798).
    itens = classificar_export(empresa, mes, ano, fechamentos=fechamentos)

    # Mesmos itens e fechamentos em memória (app/folha/views.py:273-276,
    # app/folha/export.py:699-755).
    linhas, entra_ids = montar_linhas_export(
        empresa, mes, ano, itens=itens, fechamentos=fechamentos
    )

    # A view valida quando há linhas (app/folha/views.py:278-283,
    # app/folha/export.py:266-307).
    erros_layout = validar_layout_dominio(linhas) if linhas else []

    if erros_layout:
        posicao = 0
        erros_conferidos = []
        for fech in fechamentos:
            pk = fech.colaborador_id
            if pk not in entra_ids:
                continue

            # Atribui pelo pk do fechamento. O montador concatena os blocos
            # na ordem recebida, e cada bloco não vazio começa com reg10
            # (app/folha/export.py:34-45,215-221,726-755).
            bloco, _ = montar_linhas_export(
                empresa, mes, ano, itens=itens, fechamentos=[fech]
            )
            fim = posicao + len(bloco)
            if linhas[posicao:fim] != bloco:
                raise SystemExit("S12_FALHA|blocos_txt_divergentes")
            posicao = fim

            # O erro de soma se refere ao reg10 seguido dos reg11
            # (app/folha/export.py:266-307).
            for erro in validar_layout_dominio(bloco):
                erros_conferidos.append(erro)
                erros_por_col.append((tipo_layout(erro), pk))

        if posicao != len(linhas) or erros_conferidos != erros_layout:
            raise SystemExit("S12_FALHA|atribuicao_layout_divergente")

    transaction.set_rollback(True)

# Campos do veredito (app/folha/export.py:847-859,876-891).
# O motivo None é impresso como "None".
contagens = Counter((i["status"], str(i["motivo"])) for i in itens)
for (status, motivo), total in sorted(contagens.items()):
    print(f"CONTAGEM|{status}|{motivo}|{total}")

invalidos = sorted(
    (i["colaborador_id"], i["status"], i["motivo"])
    for i in itens
    if i["motivo"] == "codigo_dominio_invalido"
)
for pk, status, motivo in invalidos:
    print(f"ITEM|col{pk:03d}|{status}|{motivo}")

col1041 = next((i for i in itens if i["colaborador_id"] == 1041), None)
if col1041 is None:
    print("COL1041|ausente")
else:
    print(f"COL1041|{col1041['status']}|{col1041['motivo']}")

print(f"TXT|linhas={len(linhas)}|erros_layout={len(erros_layout)}")
for tipo, pk in erros_por_col:
    print(f"ERRO|{tipo}|col{pk:03d}")

# entra_ids conta o veredito, inclusive aptos sem linha (app/folha/export.py:725,734-755).
entra = len(entra_ids)
print(
    f"S12|itens={len(itens)}|entra={entra}|fora={len(itens) - entra}"
    f"|codigo_dominio_invalido={len(invalidos)}"
    f"|col1041_presente={int(col1041 is not None)}"
    f"|linhas={len(linhas)}|erros_layout={len(erros_layout)}"
)

# Premissas
# - A cópia corresponde ao HEAD f0d213df9 e é a montada em /app [nao verificado].
# - A sombra tem empresa pk=2, janela esperada e integração Domínio ativa [nao verificado].
# - Execução na sombra, cobertura do recálculo e resultados de S12, inclusive
#   presença e motivo de col1041 [nao verificado].
# - Paridade dos dados da sombra com produção e liberação da entrega pela view
#   [nao verificado].
```

## 28/11 02:27 · sessao 3c40eae37ee0 · CODIGO-DOMINIO-VALIDO · construtor · origem=Code

O diff altera somente as três docstrings solicitadas. A comparação com `f0d213df9` confirmou que o texto fora delas é idêntico à base e que o parágrafo da O256 foi restaurado literalmente. Suíte e RED [não verificado]. [cadastro.py:54](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/services/cadastro.py:54), [retrato_folha.py:90](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:90), [retrato_folha.py:1100](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:1100)

```diff
diff --git a/app/colaboradores/services/cadastro.py b/app/colaboradores/services/cadastro.py
index 196fb6eaf..e6e9c608c 100644
--- a/app/colaboradores/services/cadastro.py
+++ b/app/colaboradores/services/cadastro.py
@@ -51,7 +51,12 @@ NOME_NO_MODELO = {'codigo_folha': 'codigo_dominio'}
 
 
 def codigo_dominio_e_numerico(codigo):
-    """O código, após strip(), contém um ou mais dígitos ASCII."""
+    """Responde se o código contém um ou mais dígitos ASCII.
+
+    O chamador entrega o código já sem espaços após `strip()` em `colaboradores/views.py`
+    ou `folha/export.py`. O limite de 43 caracteres da linha do layout em `folha/export.py`
+    não é conferido aqui.
+    """
     return codigo.isascii() and codigo.isdigit()
 
 
diff --git a/app/folha/retrato_folha.py b/app/folha/retrato_folha.py
index e53c46436..cce2d3312 100644
--- a/app/folha/retrato_folha.py
+++ b/app/folha/retrato_folha.py
@@ -87,7 +87,10 @@ def rotulo_do_motivo(motivo):
 
 
 def detalhe_do_motivo(linha):
-    """Complemento do rótulo com o código da linha lavrada."""
+    """Para `codigo_dominio_invalido`, exibe o VALOR GRAVADO da chave `codigo` da linha lavrada,
+    quando presente, com o prefixo fixo `codigo Dominio = ` e sem gerar texto, conforme o aval
+    CODIGO-DOMINIO-VALIDO-2811-0115 para o Retrato e o PDF.
+    """
     if linha.get('motivo') == 'codigo_dominio_invalido' and linha.get('codigo'):
         return f"codigo Dominio = {linha['codigo']}"
     return ''
@@ -1094,8 +1097,14 @@ def o_que_fazer(linha, hoje=None):
     escolhe nada: cada linha e' (chave GRAVADA -> juiz que ja existe -> rotulo do mapa que ja
     existe).
 
-    Os valores sao ROTULOS de mapa, as mesmas palavras da coluna "Por quê" e das pilulas da barra.
-    O complemento do motivo é um dado da linha lavrada, exibido ao lado do rótulo.
+    NENHUM TEXTO SE GERA (ordem Ronald 13/10 01:3x, literal: *"montado SO das chaves gravadas na
+    linha do retrato (motivo, dono, tipo, datas), sem texto gerado"*). Os valores sao ROTULOS de
+    mapa -- as MESMAS palavras da coluna "Por quê" e das pilulas da barra --, nunca frase montada.
+    A frase e' o item (4) da mesma ordem (o Haiku por cima deste bloco), e escrever prosa aqui seria
+    fazer o (4) fora de ordem.
+    Pelo aval CODIGO-DOMINIO-VALIDO-2811-0115, a exceção é o campo `detalhe` do item 'motivo', que
+    exibe o VALOR GRAVADO da chave `codigo` da linha lavrada, com prefixo fixo e sem gerar texto,
+    conforme a exibição do código gravado no Retrato e no PDF.
 
     VALOR VAZIO CONTINUA APARECENDO, com o travessao: linha sem `dono` gravado (retrato legado) ja
     tem resposta pelos juizes (`fio_da_linha` cai no `_fio_cru`), e o que nao tem resposta e' um
```

**Casos RED e mutantes para o opus**

- **P1.** No Quem Cobrar, exigir que a coluna Folha mostre o rótulo de `codigo_dominio_invalido`. Mutante proposto: remover a entrada do mapa local para que a coluna mostre a chave crua. [relatorios/views.py:1729](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729), [relatorios/views.py:1739](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1739)
- **P2.** No Raio-X do Retrato, renderizar uma retenção com `codigo` gravado como `VIGIA` e exigir `codigo Dominio = VIGIA`. Mutante proposto: remover a renderização de `l.detalhe_motivo` do template. [folha/retrato_folha.py:94](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:94), [templates/folha/_retrato_raiox.html:73](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_raiox.html:73)
- **Rescisão + `VIGIA`.** Para um colaborador que sai pelo módulo de rescisão e tem código `VIGIA`, exigir `status='fora'` e `motivo='rescisao_modulo_proprio'`. Mutante proposto: mover a guarda de código inválido para antes da guarda de rescisão. [folha/export.py:876](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/folha/export.py:876), [folha/export.py:886](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:886)

## 28/11 01:56 · sessao b70766398533 · CODIGO-DOMINIO-VALIDO · verificador · origem=Code

**Parecer 5a — a retenção funciona no caminho do TXT, com pendências no censo e na cobertura.** `classificar_export` continua sendo o juiz registrado e o montador pula o colaborador que ele marcou `fora`. A Prévia da proposta pode contar esse mesmo colaborador como alguém que passaria a entrar. Dois arquivos de produção ficaram sem teste que acuse a reversão da mudança. `app/core/juizes.py:331–337`, `app/folha/export.py:723–737,882–891`, `app/folha/previa.py:117–134,170–174`, `app/relatorios/tests/test_quem_cobrar_folha_juiz.py:34–66`, `app/folha/tests/test_retrato_raiox.py:134–141,162–207`.

Conferi em somente leitura as 15 pós-imagens pelos hashes de blob do `obra.diff` anexo e executei verificações isoladas em memória. Commit e ancestrais, reversões com a suíte Django, GREEN de 7.853 testes e ocorrência em produção [não verificado]. [nao verificado]

## 1. Escritores e leitores

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| `codigo_folha` → `Colaborador.codigo_dominio` | O formulário de admissão lê o POST e o de edição exibe o valor da instância. As duas views atribuem o código aparado e persistem por `salvar_campos` ou `salvar_inteiro`. Na recusa, os aliases só preenchem o rascunho em memória. O Django Admin usa `SoLeitura`. `app/templates/colaboradores/admitir.html:58`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1117–1139,1239–1245,1287–1298,1364–1375`, `app/core/reexibir.py:21–46`, `app/colaboradores/services/colaborador.py:140–160`, `app/colaboradores/admin.py:47–48`, `app/core/admin.py:53–75`. |
| Código aparado e matrícula do TXT | O juiz copia o valor aparado para o item. O montador relê o campo do colaborador e aplica `zfill(10)`. A prévia e o PDF de retidos mostram o código do item. O PDF de validação, o PDF histórico e a comparação histórica usam código aparado e preenchido para conciliar matrículas. O holerite configurável também pode casar por `codigo_dominio`. `app/folha/export.py:723–737,847–852`, `app/folha/views.py:424–430`, `app/folha/retidos_pdf.py:88–95`, `app/folha/validacao_pdf.py:65–72`, `app/folha/pdf_export.py:144–150`, `app/folha/haiku_export.py:109–117`, `app/holerite/views.py:1741`, `app/holerite/motor.py:158–164,216–225`. |
| `status`, chave `motivo`, texto `detalhe` e `travas_aprovacao` | `classificar_export` emite os quatro. O montador lê `status`. O POST usa elegibilidade e travas para o carimbo. Prévia, PDF de retidos e lista do fechamento leem a chave e o detalhe. `_K2` é a entrada no TXT e `_K1` é a pergunta de aprovação. Ambos apontam para a mesma função. `app/docs/JUIZES.md:119–122`, `app/core/juizes.py:331–337`, `app/folha/export.py:840–859,876–891`, `app/folha/views.py:223–276,356–374,424–430,885–886`, `app/folha/retidos_pdf.py:83–95`, `app/ponto/services/fechamento_lista.py:32–41,103–106`. |
| `RetratoFolha.linhas` e complemento | O lavrador persiste `codigo` aparado, `status`, chave `motivo`, `dono`, `tipo` e `saldo`. `TIPO_POR_MOTIVO` dá `cadastro`, que alimenta também o saldo, e `_fio_cru` dá `admin`. `detalhe_do_motivo` lê o código lavrado e monta `codigo Dominio = …`. Lista, PDF, Raio-X do Retrato e drawer individual leem essa projeção. `app/folha/retrato_folha.py:89–93,221–235,380–397,430–433,1121–1129,1243–1257`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79`, `app/folha/views_retrato.py:194–201,286–293,421–433,508–515`, `app/colaboradores/views.py:997–1017`, `app/templates/folha/_retrato_lista.html:65`, `app/templates/folha/_retrato_raiox.html:73`, `app/templates/folha/_retrato_o_que_fazer.html:37`. |
| Mensagem da recusa | `_ERRO_CODIGO_DOMINIO` sai pelas duas views antes da criação ou da edição. `resposta_render` entrega a mensagem pelo Django messages ou pelo gatilho HTMX e registra a recusa pela porta existente. `app/colaboradores/views.py:1104,1135–1139,1205–1212,1295–1298,1365–1375`, `app/core/respostas.py:74–85`, `app/core/recusa_critica.py:70–86`. |

**Atribuição explícita e escrita física.** Admissão e edição são as atribuições explícitas encontradas. Vincular posto, transferir, desligar e reativar também podem regravar o campo por `save()` integral. Exemplo estático: a transferência carrega `VIGIA`, uma edição grava `00123`, e a transferência então adquire a trava e salva a instância antiga. A guarda relê a situação, portanto esse caminho pode restaurar `VIGIA`. Ocorrência em produção [não verificado]. `app/colaboradores/views.py:331–346,1239–1245,1365–1375,1410–1428`, `app/colaboradores/services/colaborador.py:84–91,152–160,184–200`, `app/colaboradores/services/desligamento.py:128–133,235–247`.

**Listas, rótulos e contagens.**

- **Cobertos:** `MOTIVO_RH` dá o rótulo à prévia, ao PDF de retidos e ao Retrato. O fechamento tem destino e ramo próprios. Pendências de Cadastro inclui a nova natureza para pessoas `apurado` e mostra a descrição e a recomendação. Quem Cobrar mostra o rótulo na coluna e no PDF. Sua contagem e seu filtro “Sem código” comparam o literal do rótulo antigo, então contam apenas ausentes. O Raio-X ao vivo conta a chave em “Outras pendências” e mostra o detalhe. `app/folha/views.py:15–25,374,415–430,616–628,663–668,718–720,885–886`, `app/folha/retidos_pdf.py:29–38,83–95`, `app/folha/retrato_folha.py:76–86`, `app/ponto/services/fechamento_lista.py:25–41`, `app/templates/ponto/_fech_linha.html:15–19`, `app/relatorios/views_pend.py:48–71,79–85`, `app/templates/relatorios/pendencias_cadastro.html:29,47–49`, `app/relatorios/views.py:1724–1746,1763–1767`, `app/templates/relatorios/ranking_furos.html:48,68,78`.
- **Chave crua em dois leitores.** A mensageria omite a chave em `EM_PORTUGUES` e devolve `codigo_dominio_invalido` ao copiloto pelo fallback. A API e o copiloto preservam a contagem. A Prévia da proposta imprime a chave no HTML e no CSV. Ela pode ainda somar em `mudariam_de_lado` um `VIGIA` com batida e proposta que cubra todos os dias acusados pelo espelho. A projeção considera os dias para qualquer item `fora`, e o cadastro continuaria barrando o TXT. Nesses dois caminhos, a chave nova chega por fallback ou cópia direta, sem `KeyError`. `mensageria/nucleo/ferramentas.py:1196–1203,1246–1253,1264–1267`, `app/api/views_mensageria.py:223–229,261–276,283–302`, `app/folha/views.py:758–771,812–820,835–839`, `app/folha/previa.py:97–134,170–174`, `app/folha/export.py:886–891`.
- **Outras contagens.** `fora_da_folha`, as métricas de prontidão, o simulador, o censo e o vigia preservam chaves dinamicamente. Já o contador cadastral `travam_a_folha` mede código faltante e deixa de contar `VIGIA` nesse recorte. O diagnóstico de cadastro da mensageria conta `NULL` e `''`, portanto também deixa de fora um código feito só de espaços. `app/folha/fora_da_folha.py:147–173,249`, `app/inteligencia/calculadores.py:210–234,271–296`, `app/folha/management/commands/simular_folha.py:82–90,133–152`, `bin/censo_folha.py:14–23,40–47`, `app/folha/services/vigia_txt.py:107–116,231–232`, `app/colaboradores/services/cadastro.py:79–92,115–134`, `app/api/views_mensageria.py:1275–1288`.

## 2. Precedência

A ordem efetiva é `rescisao_modulo_proprio` → `sem_codigo_dominio` → `codigo_dominio_invalido` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → `entra`. Demissão até o fim da janela pode coexistir com código inválido e vence. Ausência e inválido preenchido são mutuamente exclusivos após `strip()`. As condições posteriores podem coexistir com o inválido e perdem para ele. As travas de aprovação são calculadas antes da cadeia e seguem em campo próprio. `app/folha/export.py:54–60,840–859,876–900,918–996`.

**Contraexemplo concreto da fixture:** `VIGIA` com batida em 03/06 durante férias de 01/06 a 10/06 cairia em `ferias_com_batida` com o ramo novo removido. Com o ramo, cai em `codigo_dominio_invalido`. O controle `00123` com as mesmas férias e batida segue em `ferias_com_batida`. Equivalente em produção [não verificado]. `app/folha/export.py:882–900`, `app/folha/tests/test_codigo_dominio_invalido.py:128–157`.

## 3. Predicado e semântica

Os três chamadores fazem `strip()` e compartilham um único teste de conteúdo para código preenchido. O helper exige `isascii() and isdigit()` e recebe a string já aparada. `app/colaboradores/services/cadastro.py:53–55`, `app/colaboradores/views.py:1133–1139,1295–1298`, `app/folha/export.py:847–850,882–891`.

| Entrada | Juiz, após a guarda de rescisão | Admissão | Edição |
|---|---|---|---|
| `00123`, ` 00123 ` | Usa `00123` e segue os demais crivos. | Grava `00123`. | Grava `00123`. `app/folha/export.py:850,882–892`, `app/colaboradores/views.py:1135–1139,1239–1245,1295–1298,1364–1375`. |
| `00 123`, `VIGIA`, `１２３`, `²` | `codigo_dominio_invalido`. | Recusa com mensagem. | Recusa com mensagem. `app/colaboradores/services/cadastro.py:53–55`, `app/folha/export.py:886–891`, `app/colaboradores/views.py:1135–1139,1295–1298`. |
| `''`, só espaços, campo omitido | Código vazio dá `sem_codigo_dominio`. | Cadastro novo fica com o padrão `''`. | Conserva o código anterior, inclusive um inválido legado. `app/folha/export.py:850,882–885`, `app/colaboradores/models.py:311–314`, `app/colaboradores/views.py:1135–1139,1211–1245,1295–1298,1364–1375`. |
| `None` literal | Vira vazio. | Campo omitido cai no padrão `''`. Se um POST sintético devolver `None` literal, `.strip()` levanta `AttributeError`. | Vira vazio e conserva o anterior. `app/folha/export.py:850,882–885`, `app/colaboradores/views.py:1135,1295–1298,1364–1375`. |

**O layout tem `isdigit()` próprio.** Ele checa fatias do registro serializado, inclusive matrícula. O parser também tem essa checagem. Em memória, `１２３` e `²` foram rejeitados pelo helper cadastral e aceitos pelo layout de uma linha sintética de 43 caracteres. `[cpf]` passou pelo helper, produziu 44 caracteres e foi recusado pelo layout. O modelo declara até 20 caracteres. Portanto há um teste cadastral compartilhado e checagens de formato com domínio diferente. O código do Retrato é o valor aparado na lavra. A matrícula do TXT recebe preenchimento até dez caracteres, sem truncamento. `app/colaboradores/models.py:311–314`, `app/colaboradores/services/cadastro.py:53–55`, `app/folha/export.py:34–45,246–254,272–279,737,850`, `app/folha/retrato_folha.py:89–93,1243–1247`.

## 4. TXT, contadores, relógio e trava

- **O item com motivo novo não emite linha.** O juiz dá `fora`. `montar_linhas_export` forma `entra_set` apenas de `entra` e pula o colaborador antes da matrícula, dos eventos e das faltas. No POST, juiz e montador recebem os mesmos fechamentos em memória. O validador roda sobre as linhas restantes antes da entrega. Em chamadas do montador sem `fechamentos=`, a função consulta os fechamentos novamente e continua filtrando pelos IDs do veredito recebido. `app/folha/export.py:723–755,886–891`, `app/folha/views.py:215–223,273–289`.
- **Os totais têm universos diferentes.** A prévia de exportação conta todos os itens do juiz sobre `FechamentoMensal` gravado. O POST injeta o recálculo em memória, e o montador restringe a emissão a `aberto` e `aprovado`. Exemplo estático: um fechamento gravado `exportado` pode ser elegível na prévia e o montador que leia esse gravado pula o status. Além disso, `colabs` da trilha conta IDs elegíveis, `linhas` conta registros emitidos, e `retidos` da trilha e do aviso conta só `furo_espelho`. O motivo novo soma em “Ficam de fora” da prévia e fica fora desse contador do POST. Paridade na frota [não verificado]. `app/ponto/models.py:398–410`, `app/folha/export.py:696,723–748,793–798`, `app/folha/views.py:215–223,265–276,297–313,356–372,397–399`.
- **Relógio e ordem de trava.** Os ramos novos de validação e apresentação não introduzem leitura de relógio nem aquisição de trava. O fallback preexistente do retentor ainda pode ler `localdate()` abaixo da borda. No caminho de entrega, a ordem existente é transação → trava da empresa. A revisão de batidas ordena empresa → colaborador → batidas. A edição usa a trava do colaborador no serviço, com a ressalva da instância carregada antes da trava descrita acima. **`except` novo no diff anexo: 0.** `app/colaboradores/views.py:1135–1139,1295–1303,1371–1384`, `app/folha/export.py:556–562,886–891,984–989,1247–1254,1317–1323`, `app/folha/retrato_folha.py:89–93`, `app/folha/views.py:103–105,133–136`, `app/ponto/services/par_relampago_na_lista.py:172–187`, `app/colaboradores/services/colaborador.py:72–91,152–160`.

## 5. Mordida por arquivo

A tabela indica a falha esperada ao retirar **um arquivo de produção por vez**, mantendo os REDs. Conferi em memória as mutações simples dos mapas e a busca do ramo do template de Ponto. Reversão integral por arquivo com execução Django [não verificado]. `app/folha/tests/test_codigo_dominio_invalido.py:163–200`.

| Arquivo | Teste ou falha esperada | [nao verificado]
|---|---|
| `app/colaboradores/services/cadastro.py:53–55` | A retirada do helper quebra os imports. Os REDs de admissão, edição e juiz dão erro antes da asserção. `app/colaboradores/views.py:14`, `app/folha/export.py:791`, `app/colaboradores/tests/test_codigo_dominio_numerico.py:85–92,118–125`, `app/folha/tests/test_codigo_dominio_invalido.py:90–94`. |
| `app/colaboradores/views.py:1135–1139,1295–1298` | REDs da admissão e edição, que exigem recusa e integridade dos registros. `app/colaboradores/tests/test_codigo_dominio_numerico.py:85–92,118–125`. |
| `app/folha/export.py:882–891` | REDs de letras, TXT, Unicode, precedência e presença da chave na fonte por AST. `app/folha/tests/test_codigo_dominio_invalido.py:90–105,122–125,152–157,194–200`. |
| `app/folha/retrato_folha.py:89–93,221–227,1123–1125` | A reversão integral quebra o import em `app/folha/views_retrato.py:49`. A retirada isolada do destino também falha no RED de tipo e no selo de exaustividade. `app/folha/tests/test_codigo_dominio_invalido.py:168–171,194–200`, `app/folha/tests/test_retrato_folha_quadros.py:323–329`. |
| `app/folha/views.py:15–18` | RED de presença e texto do rótulo. `app/folha/tests/test_codigo_dominio_invalido.py:163–166`. |
| `app/folha/views_retrato.py:291–292,423–426,511–512` | REDs de lista e PDF. A projeção do Raio-X fica sem mordida própria. `app/folha/tests/test_codigo_dominio_invalido.py:246–278`, `app/folha/tests/test_retrato_raiox.py:134–141,162–207`. |
| `app/ponto/services/fechamento_lista.py:25–41` | RED da pílula falha na primeira asserção, ao receber `furo_espelho`. `app/folha/tests/test_codigo_dominio_invalido.py:177–183`. |
| `app/relatorios/views.py:1725–1739` | **Sem mordida encontrada.** O teste de Quem Cobrar usa código `77` e cobre rescisão, apto e sem fechamento. `app/relatorios/tests/test_quem_cobrar_folha_juiz.py:39–46,59–66`. |
| `app/relatorios/views_pend.py:48–51` | RED da presença da chave no mapa. `app/folha/tests/test_codigo_dominio_invalido.py:190–192`. |
| `app/templates/folha/_retrato_lista.html:65` | RED exige `VIGIA` lavrado no HTML depois da troca cadastral. `app/folha/tests/test_codigo_dominio_invalido.py:237–256`. |
| `app/templates/folha/_retrato_o_que_fazer.html:37` | RED do drawer exige a frase lavrada no HTML. `app/folha/tests/test_codigo_dominio_invalido.py:280–292`. |
| `app/templates/folha/_retrato_raiox.html:73` | **Sem mordida encontrada.** As fixtures da rota têm `codigo=''` e não exercitam a chave nova. `app/folha/tests/test_retrato_raiox.py:38,134–141,162–207`, `app/folha/tests/test_retrato_folha_quadros.py:42–50,86–97`. |
| `app/templates/ponto/_fech_linha.html:15–19` | RED da pílula falha na segunda asserção, que procura a chave por regex. `app/folha/tests/test_codigo_dominio_invalido.py:184–188`. |

O selo de relógio acusou uma chamada direta no controle sintético e retornou zero novos soltos na varredura isolada em memória. Ele aceita um marcador de congelamento no arquivo inteiro e não acompanha relógios acionados indiretamente. A fixture da lavratura congela o tempo. O RED do PDF verifica a *story* do ReportLab e o HTTP 200. Corte visual do PDF [não verificado]. `app/core/tests/test_selo_teste_sem_relogio.py:23–45,48–84`, `app/folha/tests/test_codigo_dominio_invalido.py:203–212,215–229,258–278`.

## 28/11 01:54 · sessao a4b647160404 · - · - · origem=Ronald (tmux)

- **Precedência confirmada.** Rescisão vence quando a demissão é até o fim da janela. Vazio após `strip()` e inválido preenchido são mutuamente exclusivos. O motivo novo prevalece sobre férias com batida, cadastro zero, furo de espelho e rubrica não homologada. A fixture `VIGIA` com batida em 03/06 durante férias de 01/06 a 10/06 muda de `ferias_com_batida` para `codigo_dominio_invalido`. O controle `00123` continua em férias com batida. [app/folha/export.py:54–60, 876–995; app/folha/tests/test_codigo_dominio_invalido.py:128–157]

- **Predicado compartilhado e comportamentos de vazio diferentes.** Os três chamadores aparam as bordas e usam `isascii() and isdigit()` para código preenchido. ` 00123 ` preserva `00123`, `1 23`, `１２３` e `²` são inválidos. Vazio, espaços e ausência deixam o padrão `''` na admissão, preservam o anterior na edição e produzem `sem_codigo_dominio` no juiz quando a rescisão não prevalece. `None` literal é normalizado no juiz e na edição. A admissão usa `.get(..., '').strip()`, cujo padrão trata a chave ausente, e falharia se um POST sintético entregasse `None` literal. [app/colaboradores/services/cadastro.py:53–55; app/colaboradores/models.py:311–314; app/colaboradores/views.py:1135–1139, 1239–1245, 1295–1298, 1364–1375; app/folha/export.py:850, 876–891]

- **Juiz único, validador de layout com predicado diferente.** `_K2` registra `classificar_export`. O layout verifica `.isdigit()` sobre a matrícula serializada, e o parser também. A execução isolada das funções extraídas em memória confirmou que o layout aceita matrículas de largura total e sobrescrito rejeitadas no cadastro. `[cpf]` passa no cadastro e pode produzir registro de 44 caracteres, que o layout rejeita. O Retrato mostra o código aparado que foi lavrado e o TXT preenche à esquerda a partir do campo de cadastro. [app/docs/JUIZES.md:119–121; app/core/juizes.py:181, 336–337; app/folha/export.py:34–45, 247–254, 272–279, 737, 850; app/folha/retrato_folha.py:89–93, 1243–1247]

- **Há outros escritores físicos.** Vincular e transferir posto e desligar efetivamente passam por `salvar_inteiro`, que usa `save()` sem `update_fields`. Reativação também usa `save()` completo. A porta guarda `situacao` após travar, sem atualizar `codigo_dominio` da instância. Uma transferência que carregue `VIGIA` antes de uma edição que corrige e grava o código pode restaurar `VIGIA` ao gravar a instância antiga. [app/colaboradores/views.py:331–346, 1410–1428; app/colaboradores/services/colaborador.py:84–91, 152–160, 184–200; app/colaboradores/services/desligamento.py:128–133, 235–247]

- **Prévia da proposta tem uma lacuna funcional.** Exibe a chave crua no CSV e no HTML. Sua contagem `mudariam_de_lado` pode incluir pessoa com código inválido se todos os dias de retenção da célula estiverem cobertos pela proposta, pois só testa os dias. O código permaneceria inválido no juiz do TXT. [app/folha/previa.py:97–103, 117–134, 150–174; app/folha/views.py:763–771, 815–817, 835–839; app/folha/export.py:886–891]

Os 15 blobs finais da cópia coincidem com os prefixos do anexo. Commit, ancestrais, suíte e ocorrência em produção [não verificado]. [nao verificado]

## 28/11 01:54 · sessao a3d93132ff87 · - · - · origem=Ronald (tmux)

**Três leitores laterais merecem destaque.**

- O copiloto recebe `codigo_dominio_invalido` cru. `EM_PORTUGUES` omite a chave e o fallback devolve a própria chave. A contagem inclui o caso entre os bloqueios e na soma da frota. A API preserva motivos dinamicamente nas respostas por empresa, por frota e na leitura ao vivo. `mensageria/nucleo/ferramentas.py:1196-1203,1246-1253,1264-1267,1297-1304`; `app/api/views_mensageria.py:223-229,261-276,283-302`.
- A **Prévia da proposta** também mostra a chave crua em HTML e CSV. Um colaborador com `VIGIA`, batida e propostas cobrindo todos os dias que o espelho acusa pode aparecer como `passaria a entrar` e somar em `mudariam_de_lado`. A projeção testa o espelho para qualquer item `fora`, sem considerar o motivo principal. Corrigir esses dias ainda deixa a retenção de cadastro ativa. `app/folha/views.py:758-771,812-820,835-839`; `app/folha/previa.py:97-122,133,170-174`; `app/folha/export.py:886-891`.
- Dois leitores de cadastro medem ausência. O contador `travam_a_folha` marca só `codigo_folha` faltante e pode deixar de contar um ativo com `VIGIA`. `_cadastro_incompleto` usa SQL `NULL` ou `''`, portanto deixa de fora até um código composto só de espaços. Esses leitores partem do cadastro ativo, com recortes próprios. `app/colaboradores/services/cadastro.py:26-30,79-92,104-134`; `app/api/views_mensageria.py:1275-1288,1703-1724`.

**Campo, autoridade e apresentação**

- Admissão e edição são as atribuições explícitas de `Colaborador.codigo_dominio` encontradas. `salvar_campos` persiste a lista de campos na admissão e `salvar_inteiro` persiste o objeto completo na edição. O Django Admin lê o campo pelo `SoLeitura`. Operações de posto, desligamento e reativação também regravam o objeto inteiro pela mesma porta. A trava relê `situacao`, então uma cópia carregada antes de uma edição concorrente pode regravar o código anterior. `app/colaboradores/views.py:346,1239-1245,1365-1375,1428`; `app/colaboradores/services/colaborador.py:55-69,84-91,140-160,184-200`; `app/colaboradores/services/desligamento.py:133,247`; `app/colaboradores/admin.py:47-48`; `app/core/admin.py:53-75`.
- A frase de recusa vem de `_ERRO_CODIGO_DOMINIO`. O envelope a envia ao Django messages ou ao evento HX e à trilha `ponto.recusa_critica`. Os retornos acontecem antes de criar ou editar o cadastro. `app/colaboradores/views.py:1104,1135-1139,1205-1212,1295-1298,1365-1375`; `app/core/respostas.py:74-85`; `app/core/recusa_critica.py:34,70-86`.
- `classificar_export` é `_K2`, a autoridade do TXT, e `_K1` para a pergunta distinta das travas de aprovação. Ele emite `status`, a chave `motivo` e o texto `detalhe`, além do código aparado. O lavrador copia código aparado, status e chave para `RetratoFolha.linhas` e grava dono, tipo e saldo. `TIPO_POR_MOTIVO` mapeia a chave para `cadastro`, também lido por `MOTIVOS_DE_CADASTRO` e `_saldo_cru`. `_fio_cru` resolve Admin para essa retenção. `app/docs/JUIZES.md:119-122`; `app/core/juizes.py:336-337`; `app/folha/export.py:847-859,882-891`; `app/folha/retrato_folha.py:221-235,380-397,430-433,1218-1257`; `app/folha/management/commands/lavrar_retrato_folha.py:65,72-79`.
- A prévia de exportação e o PDF de retidos agrupam por chave presente e usam `MOTIVO_RH`. O Retrato traduz com `rotulo_do_motivo` e forma o complemento a partir de `linha['codigo']` da lavra, usado em lista, PDF, Raio-X do Retrato e drawer individual. `app/folha/views.py:15-24,356-374,415-430,885-886`; `app/folha/retidos_pdf.py:29-38,83-95`; `app/folha/retrato_folha.py:76-93,1121-1125`; `app/folha/views_retrato.py:194-201,286-293,421-433,508-515`; `app/colaboradores/views.py:997-1017`; `app/templates/folha/_retrato_lista.html:65`; `app/templates/folha/_retrato_raiox.html:73`; `app/templates/folha/_retrato_o_que_fazer.html:37`.
- A lista do fechamento leva a chave e o detalhe à pílula própria. O Quem Cobrar usa o rótulo na coluna e no PDF, e a contagem e filtro “Sem código” comparam literalmente o rótulo antigo, delimitando os ausentes. Pendências de Cadastro usa a nova natureza `CODIGO_DOMINIO_INVALIDO` para linhas `apurado`, e renderiza natureza, descrição e recomendação. O Raio-X ao vivo usa o complemento de `_COM_PILULA`, contando o caso em “Outras pendências” e exibindo o detalhe. `app/ponto/services/fechamento_lista.py:25-41`; `app/templates/ponto/_fech_linha.html:15-19`; `app/relatorios/views.py:1724-1746,1763-1767`; `app/templates/relatorios/ranking_furos.html:48,68,78,104`; `app/relatorios/views_pend.py:48-71,79-85`; `app/templates/relatorios/pendencias_cadastro.html:25-29,47-49`; `app/folha/views.py:616-628,663-668,718-720`.
- Agrupam ou registram a chave dinamicamente `fora_da_folha`, as duas métricas de prontidão, simulador, censo e vigia TXT. Diagnósticos de ponto consomem o par status/motivo em mapas dinâmicos. `app/folha/fora_da_folha.py:147-155,173,249,273,303-304`; `app/inteligencia/calculadores.py:210-234,271-296`; `app/folha/management/commands/simular_folha.py:89-90,133-146,208-217,260`; `bin/censo_folha.py:14-23,40-47`; `app/folha/services/vigia_txt.py:98-116,168-176,231-232`; `app/ponto/management/commands/celula_veredito_velho.py:81-82,151-152`; `app/ponto/management/commands/diff_reclassificar_partido.py:152-153,208-209`; `app/ponto/management/commands/aplicar_09_corte_b.py:189-190,272-273`; `app/ponto/management/commands/desvio_o68b.py:190-191,216-220`.
- Leitores laterais do código incluem o PDF do TXT histórico, a comparação histórica, o PDF de validação e o match configurável de holerite. O parser Domínio padrão usa nome. `app/folha/pdf_export.py:144-150`; `app/folha/haiku_export.py:109-117`; `app/folha/validacao_pdf.py:65-72,91-103`; `app/holerite/views.py:1741`; `app/holerite/motor.py:158-164,216-225`.

## 28/11 01:53 · sessao 5302d656a3d6 · - · - · origem=Ronald (tmux)

Dos 13 arquivos de produção do diff anexo, 11 têm uma falha identificável sob reversão isolada. As duas lacunas são Quem Cobrar e o template do Raio-X do Retrato. As falhas abaixo vêm da leitura do diff e dos testes. A execução integral e o job informado ficam [não verificado]. [nao verificado]

| Arquivo revertido | Teste que acusa a reversão | [nao verificado]
|---|---|
| `app/colaboradores/services/cadastro.py:53` | A retirada do símbolo quebra o import da view e do juiz. Os REDs de admissão, edição e classificação dão erro, antes da asserção. `app/colaboradores/views.py:14`, `app/folha/export.py:791`, `app/colaboradores/tests/test_codigo_dominio_numerico.py:85-92,118-125`, `app/folha/tests/test_codigo_dominio_invalido.py:90-94`. |
| `app/colaboradores/views.py:1135-1139,1295-1298` | REDs da admissão e da edição verificam recusa e integridade dos registros. `app/colaboradores/tests/test_codigo_dominio_numerico.py:85-92,118-125`. |
| `app/folha/export.py:882-891` | REDs de letras, TXT, Unicode, precedência e enumeração AST do motivo. `app/folha/tests/test_codigo_dominio_invalido.py:90-105,122-125,152-157,194-200`. |
| `app/folha/retrato_folha.py:89-93,221-227,1123-1125` | RED do tipo e selo do destino, além do erro na importação do complemento pela view. `app/folha/tests/test_codigo_dominio_invalido.py:168-171,194-200`, `app/folha/tests/test_retrato_folha_quadros.py:323-328`, `app/folha/views_retrato.py:49`. |
| `app/folha/views.py:15-18` | RED da presença e do texto do rótulo. `app/folha/tests/test_codigo_dominio_invalido.py:163-166`. |
| `app/folha/views_retrato.py:291-292,423-426,511-512` | REDs de lista e PDF cobrem as duas primeiras projeções. O trecho do Raio-X fica sem mordida própria. `app/folha/tests/test_codigo_dominio_invalido.py:246-278`, `app/folha/tests/test_retrato_raiox.py:134-141,162-207`. |
| `app/ponto/services/fechamento_lista.py:25-41` | RED da pílula falha na primeira asserção, ao receber `furo_espelho`. `app/folha/tests/test_codigo_dominio_invalido.py:177-183`. |
| `app/relatorios/views.py:1725-1739` | Sem teste identificado para a entrada nova. O teste da coluna Folha usa código `77` em todos os colaboradores e confere rescisão, apto e sem fechamento. `app/relatorios/tests/test_quem_cobrar_folha_juiz.py:39-46,59-66`. |
| `app/relatorios/views_pend.py:48-51` | RED da presença da chave no mapa de cadastro. `app/folha/tests/test_codigo_dominio_invalido.py:190-192`. |
| `app/templates/folha/_retrato_lista.html:65` | RED da lista exige `VIGIA` lavrado após trocar o cadastro. `app/folha/tests/test_codigo_dominio_invalido.py:237-256`. |
| `app/templates/folha/_retrato_o_que_fazer.html:37` | RED do painel exige a frase lavrada no HTML. `app/folha/tests/test_codigo_dominio_invalido.py:280-292`. |
| `app/templates/folha/_retrato_raiox.html:73` | Sem teste identificado para o complemento. A rota carrega `LINHAS`, com `codigo=''` e sem a chave nova. `app/folha/tests/test_retrato_raiox.py:38,134-141,162-207`, `app/folha/tests/test_retrato_folha_quadros.py:42-50,86-97`. |
| `app/templates/ponto/_fech_linha.html:15-19` | O mesmo RED da pílula falha na segunda asserção, que busca a chave declarada por regex. `app/folha/tests/test_codigo_dominio_invalido.py:184-188`. |

Confirmei em memória as cinco mutações simples dos mapas e do template de Ponto nas asserções indicadas. O RED do PDF inspeciona a *story* do ReportLab e a resposta 200. Corte visual do PDF [não verificado]. O RED do drawer confirma `Admin` na saída do helper e a frase no HTML. A pílula de Ponto tem selo estrutural por regex. `app/folha/tests/test_codigo_dominio_invalido.py:163-200,203-212,265-292`.

A lógica do selo do relógio, executada isoladamente em memória, encontrou os mesmos 452 arquivos da lista, com zero novos e zero curados. O controle com `datetime.date.today()` acusou uma leitura. O detector conta chamadas diretas sem argumento, aceita `freeze_time` por texto no arquivo inteiro e não acompanha comandos ou requisições chamados pelo teste. O RED da lavratura congela o relógio explicitamente. `app/core/tests/test_selo_teste_sem_relogio.py:23-45,48-84`, `app/folha/tests/test_codigo_dominio_invalido.py:215-229,249-250`.

## 28/11 01:43 · sessao db69fbcafc44 · FERIADO-UM-JUIZ · leitura · origem=Code

**O DIFF deve rodar duas vezes sobre a mesma sombra**. Monte o `app/` da árvore viva para o **ANTES** e o `app/` de `d765154a3` para o **DEPOIS**, via `SONDA_ARVORE`. O gravado da sombra é a terceira coluna, usada para estimar o que o apply escreveria. Deltas reais, lista do TXT e hashes de hoje seguem [não verificado]. `bin/sonda_leitura.sh:226-240` `bin/diff_o124_frota.py:4-14` `app/ponto/services/diff_frota.py:7-12`

### 1. Instrumento e universo

- **`bin/diff_o124_frota.py` é o precedente para comparar duas árvores.** Ele roda o escritor real em modo leitura em cada montagem e compara as duas fotos por colaborador e campo. O script original fixa uma competência por rodada e chama o recalculador sem filtro de empresa. **`diff_frota.medir`** é o serviço da casa para gravado, hoje e novo quando a mudança é uma mutação dentro de uma única árvore. Ele parte de quem já tem mensal gravado. `bin/diff_o124_frota.py:10-14` `bin/diff_o124_frota.py:20-42` `bin/diff_o124_frota.py:47-69` `app/ponto/services/diff_frota.py:45-89`
- **`diff_calculador` compara motor e calculador por dia e rubrica dentro do código carregado.** Recebe um par `--mes/--ano` por chamada, percorre todas as empresas ativas e parte dos IDs com mensal gravado. `--contra-gravado` acrescenta a comparação do calculador com o mensal. `--periodos-do-fechamento` captura os períodos da mesma árvore e intercepta a lavratura. `app/ponto/management/commands/diff_calculador.py:25-56` `app/ponto/management/commands/diff_calculador.py:98-135` `app/ponto/management/commands/diff_calculador.py:541-588`
- **`sombra.sh --cobre INI FIM` consulta o carimbo de cobertura.** Use a janela de cada empresa, incluindo o dia adicional em cada ponta que o fechamento consulta. A vigência começa em 11/2026 e alcança 12/2026. Com corte 21, 20/11 é a véspera da janela da 12 e entra na busca de batidas. O corte efetivo das três empresas e a cobertura de hoje seguem [não verificado]. `bin/sombra.sh:219-233` `app/core/sombra_cobertura.py:32-49` `app/ponto/janelas.py:10-21` `app/ponto/janelas.py:54-57` `app/ponto/services/fechamento.py:155-173` `app/ponto/services/fechamento.py:601-604` `app/ponto/services/fechamento.py:685-699` `app/folha/export.py:383-391`

### 2. Sonda de impacto

`--sombra` seleciona `sombra_db/sombra`. Por padrão, a sonda monta `$RAIZ/app:/app:ro`, onde `$RAIZ` é a raiz do **wrapper chamado**. A opção de árvore é `SONDA_ARVORE=<cópia>/app`. O `--dir` é da suíte. O HEAD vivo `93e305860` e a disponibilidade dessa opção no wrapper instalado seguem [não verificado]. `bin/sonda_leitura.sh:31` `bin/sonda_leitura.sh:38-45` `bin/sonda_leitura.sh:66-71` `bin/sonda_leitura.sh:93-97` `bin/sonda_leitura.sh:226-240` `bin/suite.sh:170`

A sonda usa o universo integral do recalculador para as empresas 2, 3 e 4 nas competências 11 e 12, inclusive quem ainda está sem mensal. Os totais incluem diferenças de **0,01 h** e mostram quantos valores preenchidos entraram em cada soma. No TXT 11, segue a chamada do gerador a `classificar_export` sobre os mensais em memória. A marca `ITEM_8` mostra quando zerar só a HE100 feriado numa cópia em memória devolve o motivo `cadastro_zero`. `SEM_CELULA` distingue esse caso de cadastro sem previsto com célula. A coluna `TXT` preserva o veredito real, inclusive quando uma retenção posterior ainda barra o colaborador. `app/ponto/services/fechamento.py:490-507` `app/ponto/services/fechamento.py:619-622` `app/ponto/services/fechamento.py:962-973` `app/ponto/services/fechamento.py:1016-1021` `app/ponto/management/commands/recalcular_fechamento.py:54-55` `app/folha/views.py:214-222` `app/folha/export.py:924-926` `app/folha/export.py:948-1005`

Conteúdo de `impacto_feriado.py` para a sessão. Sintaxe, comparador e projeção do item 8 conferidos com dados sintéticos. Execução com Django e a sombra real [não verificado].

```python
import json
import sys
from copy import copy
from decimal import Decimal

ANO, MESES, EMPRESAS = 2026, (11, 12), (2, 3, 4)


def projetar(registros, campos):
    saida = {}
    for f in registros:
        valores = (getattr(f, campo) for campo in campos)
        saida[str(f.colaborador_id)] = [None if v is None else str(v) for v in valores]
    return saida


def vereditos(empresa, fechamentos):
    from folha.export import classificar_export

    itens = classificar_export(empresa, 11, ANO, fechamentos=fechamentos)
    candidatos = []
    for f in fechamentos:
        if f.horas_trabalhadas == 0 and f.horas_extras_100_feriado > 0:
            copia = copy(f)
            copia.horas_extras_100_feriado = 0
            candidatos.append(copia)
    retidos_sem_he = {
        i["colaborador_id"]: (
            "SEM_CELULA" if i["detalhe"].startswith("Sem NENHUMA celula") else "SEM_PREVISTO"
        )
        for i in classificar_export(empresa, 11, ANO, fechamentos=candidatos)
        if i["motivo"] == "cadastro_zero"
    }
    return {
        str(i["colaborador_id"]): [
            i["status"], i["motivo"],
            retidos_sem_he.get(i["colaborador_id"]) if i["motivo"] != "cadastro_zero" else None,
        ]
        for i in itens
    }


def coletar():
    from django.db import connection, transaction

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cur.execute("SELECT current_database(), current_schema(), current_user, "
                        "current_setting('transaction_read_only')")
            if cur.fetchone() != ("sombra", "juliani", "leitor", "on"):
                raise RuntimeError("Sonda exige sombra/juliani com leitor em READ ONLY")

        from colaboradores.models import Empresa
        from ponto.janelas import janela_fechamento
        from ponto.management.commands.recalcular_fechamento import RUBRICAS
        from ponto.models import FechamentoMensal
        from ponto.services.fechamento import recalcular_fechamento_mes, universo_da_competencia

        dados = {}
        for mes in MESES:
            for eid in EMPRESAS:
                empresa = Empresa.objects.get(pk=eid)
                janela = janela_fechamento(mes, ANO, empresa)
                gravado = FechamentoMensal.objects.filter(
                    mes=mes, ano=ANO, colaborador__empresa_id=eid
                )
                lidos = recalcular_fechamento_mes(
                    mes, ANO, empresa_id=eid, somente_leitura=True,
                    retornar_objeto=True, lavrar_dias_pagos=False, propagar_falha=True,
                )
                esperados = set(universo_da_competencia(mes, ANO, eid).values_list("pk", flat=True))
                if lidos.keys() != esperados:
                    raise RuntimeError(f"Leitura incompleta emp{eid} {mes:02d}/{ANO}")
                dados[f"{mes:02d}/{ANO} emp{eid}"] = {
                    "janela": [d.isoformat() for d in janela],
                    "gravado": projetar(gravado, RUBRICAS),
                    "lido": projetar(lidos.values(), RUBRICAS),
                    "txt": vereditos(empresa, list(lidos.values())) if mes == 11 else {},
                }
        transaction.set_rollback(True)

    print("IMPACTO_JSON", json.dumps({"campos": RUBRICAS, "dados": dados}, sort_keys=True))


def delta(antes, depois):
    return None if antes is None or depois is None else Decimal(depois) - Decimal(antes)


def linha(*valores):
    print(*(v if v is not None else "-" for v in valores), sep="\t")


def comparar(antes, depois):
    campos = antes["campos"]
    a, b = antes["dados"], depois["dados"]
    if campos != depois["campos"] or a.keys() != b.keys():
        raise SystemExit("Escopos ou rubricas de comparação diferentes")
    if any(a[k]["gravado"] != b[k]["gravado"] or a[k]["janela"] != b[k]["janela"] for k in a):
        raise SystemExit("O gravado ou a janela mudou entre as leituras")

    linha("FOLHA", "comp empresa", "colaborador", "rubrica",
          "gravado", "antes", "depois", "efeito", "delta_apply")
    for grupo in sorted(a):
        linha("JANELA", grupo, *a[grupo]["janela"])
        gravado, velho, novo = a[grupo]["gravado"], a[grupo]["lido"], b[grupo]["lido"]
        fotos = (gravado, velho, novo)
        vazio = [None] * len(campos)
        for cid in sorted(gravado.keys() | velho.keys() | novo.keys(), key=int):
            col = f"col{int(cid):03d}"
            if not all(cid in foto for foto in fotos):
                linha("UNIVERSO", grupo, col, *(cid in foto for foto in fotos))
            for pos, campo in enumerate(campos):
                g, v, n = (foto.get(cid, vazio)[pos] for foto in fotos)
                linha("FOLHA", grupo, col, campo, g, v, n, delta(v, n), delta(g, n))

        for pos, campo in enumerate(campos):
            colunas = [[Decimal(v[pos]) for v in foto.values() if v[pos] is not None] for foto in fotos]
            g, v, n = (sum(valores, Decimal(0)) for valores in colunas)
            linha("TOTAL", grupo, "frota", campo, g, v, n, n - v, n - g,
                  *(len(valores) for valores in colunas))

        ta, tb = a[grupo]["txt"], b[grupo]["txt"]
        for cid in sorted(ta.keys() | tb.keys(), key=int):
            va = ta.get(cid, ["sem_fechamento", None, None])
            vb = tb.get(cid, ["sem_fechamento", None, None])
            movimento = "ENTROU" if vb[0] == "entra" and va[0] != "entra" else (
                "SAIU" if va[0] == "entra" and vb[0] != "entra" else "-"
            )
            linha("TXT", grupo, f"col{int(cid):03d}", movimento, *va[:2], *vb[:2])
            if va[2] or vb[2]:
                linha("ITEM_8", grupo, f"col{int(cid):03d}", movimento, va[2], vb[2])


if sys.argv[1:] == ["--comparar"]:
    comparar(json.loads(next(sys.stdin)), json.loads(next(sys.stdin)))
else:
    coletar()
```

As duas rodadas devem usar a mesma foto da sombra, sem refresh ou outro escritor no intervalo. A igualdade do gravado e das janelas detecta parte das mudanças de entrada. Cada rodada tem seu próprio snapshot e o fechamento usa o relógio para o teto de furos, relevante na 12 em curso. `app/ponto/services/fechamento.py:679-680` `app/ponto/services/fechamento.py:1147-1154` `bin/diff_o124_frota.py:10-14`

**Há uma diferença operacional importante.** O Python acima abre `READ ONLY` e termina com rollback. O wrapper provisiona role e GRANTs, reescreve `logs/.env_leitor`, copia a sonda para um temporário e grava trilha. Se a proibição de escrita inclui esses atos administrativos, use a montagem direta abaixo com leitor e rede **já provisionados**. Ela também evita depender de `app/logs` e `app/media` na cópia, usados como pontos de montagem pelo wrapper. A disponibilidade desses pontos e do leitor provisionado na sessão segue [não verificado]. `bin/sonda_leitura.sh:115-181` `bin/sonda_leitura.sh:184-203` `bin/sonda_leitura.sh:216-219` `bin/sonda_leitura.sh:245-250` `bin/sombra.sh:180-191`

```bash
(
  set -euo pipefail
  viva=/home/ronald/saas-hasner
  obra=/home/ronald/codex_ws/d765154a302318df66e6fd7c1a8ab01dab660004
  sonda=/caminho/da/sessao/impacto_feriado.py

  test "$(git -C "$viva" rev-parse --short=9 HEAD)" = 93e305860

  rodar() {
    SONDA_ARVORE="$1" bash "$viva/bin/sonda_leitura.sh" --sombra "$sonda"
  }

  medir() {
    rodar "$1" 2>&1 | awk '
      /^IMPACTO_JSON / { sub(/^IMPACTO_JSON /, ""); print; n++ }
      END { if (n != 1) exit 1 }
    '
  }

  antes=$(medir "$viva/app") || { echo "Falhou a leitura ANTES" >&2; exit 1; }
  depois=$(medir "$obra/app") || { echo "Falhou a leitura DEPOIS" >&2; exit 1; }
  printf '%s\n%s\n' "$antes" "$depois" | python3 -B "$sonda" --comparar
)
```

Para a montagem direta, substitua `rodar` no bloco acima pela função a seguir. Ela usa a imagem, a rede e a credencial de leitor da sonda, com a árvore em `/codigo:ro` e logs em tmpfs. Execução desse comando na sessão [não verificado]. `bin/sonda_leitura.sh:42-51` `bin/sonda_leitura.sh:232-240` `bin/recursos.sh:81` `app/config/settings/sombra.py:47-51` `app/config/settings/saas.py:118-143`

```bash
rodar() {
  (
    source "$viva/bin/recursos.sh"
    docker run --rm --read-only $TESTE_DOCKER \
      --network sonda_leitura --env-file "$viva/logs/.env_leitor" \
      -e POSTGRES_DB=sombra -e TZ=America/Sao_Paulo -e PYTHONDONTWRITEBYTECODE=1 \
      --mount "type=bind,src=$1,dst=/codigo,readonly" \
      --mount "type=bind,src=$sonda,dst=/sonda.py,readonly" \
      --tmpfs /app/logs --tmpfs /app/media --workdir /codigo \
      saas-hasner-core:latest \
      python /codigo/manage.py tenant_command shell --schema=juliani \
      --settings=config.settings.sombra \
      --command "exec(open('/sonda.py').read())"
  )
}
```

### 3. Reversão e apply da 11

- **Escritor genérico da foto pré-apply.** `carimbo_gravado --empresa ID --mes 11 --ano 2026` usa `foto()` e grava valores por colaborador e campo em `logs/carimbo_gravado/`. A obra da janela de HE é um precedente explícito. Seu apply grava as fotos `antes` em `logs/janela_he/` antes de alterar cadastro e chamar o recalculador. `recalcular_fechamento --apply` só grava seu próprio JSON **depois** do recálculo. `app/ponto/management/commands/carimbo_gravado.py:29-56` `app/ponto/management/commands/carimbo_gravado.py:101-113` `app/ponto/management/commands/carimbo_gravado.py:145-155` `app/ponto/management/commands/aplicar_janela_he_total.py:75-90` `app/ponto/management/commands/aplicar_janela_he_total.py:116-146` `app/ponto/management/commands/recalcular_fechamento.py:130-151`
- **Precedentes de reversão em `logs/`.** `bin/snapshot_relavratura_10_2026.py` escreve `/app/logs/reversao_relavratura_10_2026.json` e `bin/snapshot_o263_comp10.py` escreve `/app/logs/reversao_o263_comp10.json`. Ambos guardam mensal, `DiaPago.motor` e IDs sem mensal anterior. Para esta obra, a foto precisa cobrir também `DiaPago.oraculo`, pois a porta relavra as duas versões. `restaurar_fechamento` repõe campos de linhas mensais existentes. A existência de um arquivo de reversão específico da 11 em `logs/*/reversao*` segue [não verificado]. `bin/snapshot_relavratura_10_2026.py:25-64` `bin/snapshot_o263_comp10.py:53-84` `app/ponto/services/fechamento.py:1235-1239` `app/ponto/services/fechamento.py:1262-1266` `app/ponto/services/fechamento.py:1300-1319` `app/ponto/services/dia_pago.py:368-373` `app/ponto/services/fechamento.py:1398-1437`
- **Porta do apply.** É `tenant_command recalcular_fechamento --schema=juliani --mes 11 --ano 2026 --empresa ID --apply`, uma chamada por empresa do escopo. A chamada encaminha apenas `(11, 2026, ID)` e grava mensal e lavra nessa chave. A 12 pode mudar por uma chamada independente após o deploy, por exemplo, pelo recálculo por evento que resolve a competência da data do fato. Os contadores `recusas_oraculo` e `falhas_tecnicas_oraculo` saem no resumo da lavra. Seus valores e a prova pós-apply seguem [não verificado]. `app/ponto/management/commands/recalcular_fechamento.py:77-81` `app/ponto/management/commands/recalcular_fechamento.py:110-133` `app/ponto/services/fechamento.py:962-973` `app/ponto/services/fechamento.py:1235-1239` `app/ponto/services/fechamento.py:1320-1351` `app/ponto/services/fechamento.py:1836-1853`

### 4. Hash do FechamentoMensal de 06, 07, 08 e 09

A função da casa é **`ponto.portas.regen_exportada.hash_do_gravado(colaborador, mes, ano)`**. Ela consulta o mensal gravado e retorna 12 caracteres do MD5. O conjunto exclui chaves e carimbos como `previsto_em`, `derivado_em` e `atualizado_em`. Para um digest por empresa e competência, use SHA-256 do JSON ordenado de `carimbo_gravado.foto(eid, mes, 2026)` e publique também a contagem de linhas. Repita nos quatro meses, no mesmo banco antes e depois do apply, incluindo todas as empresas para a prova histórica. `app/ponto/portas/regen_exportada.py:35-38` `app/ponto/portas/regen_exportada.py:56-74` `app/ponto/portas/regen_exportada.py:121-122` `app/ponto/portas/regen_exportada.py:194-195` `app/ponto/management/commands/carimbo_gravado.py:29-56`

```python
from hashlib import sha256
import json

from colaboradores.models import Empresa
from ponto.management.commands.carimbo_gravado import foto

for mes in (6, 7, 8, 9):
    for eid in Empresa.objects.order_by("pk").values_list("pk", flat=True):
        mensal = foto(eid, mes, 2026)
        dados = json.dumps(mensal, sort_keys=True, separators=(",", ":")).encode()
        print(f"{mes:02d}/2026 emp{eid} linhas={len(mensal)} sha256={sha256(dados).hexdigest()}")
```

`ExportacaoDominio.hash_sha256` é o campo do **TXT**. Os hashes do mensal antes e depois do apply seguem [não verificado]. `app/folha/models.py:75-84` `app/ponto/portas/regen_exportada.py:62-74`

## 28/11 01:43 · sessao a0c0bb04d7f1 · RETRATO-PDF-CORTA-POR-QUE · censo · origem=Code

# Dossiê antes do construtor · RETRATO-PDF-CORTA-POR-QUE, item 2

O vínculo **dia → motivo** já existe na fonte. Ele se perde em `fora_da_folha`, que entrega à lavra os motivos agregados e as datas planas. O aval fornecido autoriza preservar esse vínculo em chave opcional, usar um mapa de rótulos junto ao produtor e mostrar no **Por quê** apenas as datas da janela. `app/folha/export.py:1268` · `app/folha/fora_da_folha.py:214` · `app/folha/fora_da_folha.py:234` · `app/folha/fora_da_folha.py:246` · `aval.txt:2`

Usei o aval anexado. A entrada indicada em `PROMPTS.md` está [nao verificado] na cópia, cujo arquivo termina na linha 3194. O SHA Git da cópia, as linhas de produção e a execução da suíte estão [nao verificado]. `app/docs/PROMPTS.md:3194`

## 1. Lavrador e contrato da fonte

- **Escritor único em produção.** `lavrar_retrato_folha --apply` chama `montar_linhas` e persiste `linhas` e `lavrado_em` por `empresa`, `mes` e `ano` com `update_or_create`. O cron usa o mesmo comando. O selo AST cobra um único importador de produção de `montar_linhas`. A linha nasce em `montar_linhas`; `o_que_fazer` é leitor. `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/management/commands/lavrar_retrato_folha.py:72` · `app/folha/management/commands/lavrar_retrato_folha.py:77` · `app/config/crons.py:1659` · `app/folha/tests/test_retrato_folha_um_derivador.py:68` · `app/folha/retrato_folha.py:1237` · `app/folha/retrato_folha.py:1079`
- **`tipo`.** A lavra grava um valor por colaborador. Para `furo_espelho`, `_tipo_cru` consulta `TIPO_POR_CAUSA` com a `causa` já gravada em `FechamentoMensal.causa_espelho`. Para os demais bloqueios, usa `TIPO_POR_MOTIVO`. Há `sem_causa` para bloqueio sem tipo conhecido e `''` para quem entra ou sai da conta. `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:1167` · `app/folha/retrato_folha.py:1250`
- **`datas`.** Para `furo_espelho`, `fora_da_folha` já tem `_dias` vindos de `com_dias=True` e projeta `[str(d['data']) for d in _dias]`. Quando `_c.get('dias')` é `None`, a lavra tenta `dias_ferias_com_batida` do item do export. Grava uma lista plana `AAAA-MM-DD` ou `None` quando falta forma de dia. Retratos anteriores à E2 podem vir sem `datas`. O `informativo` de rescisão tem chave própria. `app/folha/fora_da_folha.py:115` · `app/folha/fora_da_folha.py:246` · `app/folha/export.py:885` · `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1243` · `app/folha/retrato_folha.py:1255` · `app/folha/models.py:334`
- **Retorno exato de `motivos_retencao_celula(com_dias=True)`.** Tupla `(motivos, dias)`. `motivos` é `list[str]`, sem duplicatas e ordenada lexicalmente. `dias` é `list[dict]`, em ordem cronológica, com um registro por **dia de retenção do colaborador**. As chaves são `data: datetime.date`, `motivo: str`, `veredito: str | None` e `celula_id: int | None`. `data` é o dia civil da célula, sem hora, ainda em Python. Na falta da célula, os dois últimos campos são `None`; célula existente sem veredito conserva o ID. Isento, desligado, intervalo colapsado ou nenhum dia retido resultam em `([], [])`. O modo padrão devolve só a lista de motivos. `app/folha/export.py:1214` · `app/folha/export.py:1247` · `app/folha/export.py:1265` · `app/folha/export.py:1268` · `app/folha/export.py:1273` · `app/folha/export.py:1304` · `app/escala/models.py:1696` · `app/escala/models.py:1783` · `app/escala/apps.py:5`

## 2. Leitores do JSON e tolerância a chave nova

`RetratoFolha.linhas` é `JSONField` sem validador de esquema por linha na declaração. `linhas_do_retrato` devolve os dicionários pelo recorte e valida o argumento `filtro`. `recortar` valida grupos e valores do filtro. A chave extra atravessa esses leitores. `app/folha/models.py:350` · `app/folha/retrato_folha.py:98` · `app/folha/retrato_folha.py:647`

| Leitor | O que faz com a chave desconhecida |
|---|---|
| **Tela inteira, parcial, barra e contadores** | `_recorte_da_barra` abre as linhas e `_cadastro` copia o dicionário. A barra lê os eixos conhecidos; `_tabela` projeta campos explícitos e hoje passa `motivo` a `rotulo_do_motivo`. A chave extra passa pelo recorte e fica fora da saída. `app/folha/views_retrato.py:116` · `app/folha/views_retrato.py:194` · `app/folha/views_retrato.py:278` · `app/folha/views_retrato.py:324` · `app/folha/views_retrato.py:345` · `app/folha/retrato_folha.py:114` |
| **PDF do retrato e Raio-X** | Usam `_recorte_da_barra`. O PDF extrai as células explicitamente. O Raio-X agrupa por `tipo` e projeta `motivo` e `dias`. Ambos toleram a chave e hoje a ignoram. `app/folha/views_retrato.py:388` · `app/folha/views_retrato.py:414` · `app/folha/views_retrato.py:489` · `app/folha/views_retrato.py:500` |
| **Drawer do colaborador** | `linha_do_colab` devolve o dicionário inteiro. `o_que_fazer`, `portas_da_linha` e o template leem campos específicos. A chave extra chega à linha e hoje fica sem consumo. O drawer mostra o rótulo agregado, o tipo único e as datas planas em ISO. `app/colaboradores/views.py:994` · `app/folha/retrato_folha.py:950` · `app/folha/retrato_folha.py:1047` · `app/folha/retrato_folha.py:1112` · `app/templates/folha/_retrato_o_que_fazer.html:42` |
| **Comandos e frescor** | `vigiar_retido_calado` lê os bloqueados e campos específicos, com tratamento próprio para ausência de `dias_sem_leitura`. Tolera a chave extra. `lavrar_retrato_folha` conta a linha recém-montada em memória, inclusive no dry-run. O frescor consulta o carimbo `lavrado_em`. `app/folha/management/commands/vigiar_retido_calado.py:30` · `app/folha/vigia_retido_calado.py:77` · `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/retrato_folha.py:1413` |
| **TXT** | A geração segue `classificar_export` → `montar_linhas_export` e valida o layout do TXT. A descarga do histórico devolve `ExportacaoDominio.conteudo`. A chave do JSON do retrato está fora das duas rotas. `app/folha/views.py:219` · `app/folha/views.py:275` · `app/folha/views.py:282` · `app/folha/export.py:719` · `app/folha/views.py:574` |
| **Testes** | O selo da lavra compara o **conjunto exato de chaves** de uma linha e acusa a adição nessa fixture. O teste de idempotência compara duas lavras completas. O teste de lookup do drawer exige que a linha volte inteira. Os testes de período e de literal de mapa cobram a saída exibida. Os demais consumidores acima leem campos selecionados. `app/folha/tests/test_retrato_folha_quadros.py:1425` · `app/folha/tests/test_retrato_folha_contador.py:270` · `app/folha/tests/test_retrato_folha_drawer.py:140` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/folha/tests/test_retrato_folha_periodo.py:371` |

## 3. Universo diário e molde AST

**Oito códigos possíveis na cópia.** `sem_celula` sai de dois caminhos do laço. Os outros sete são `espelho_furo`, `espelho_discordante`, `espelho_indefinida`, `espelho_cobranca_indevida`, `espelho_cobrado`, `espelho_nunca_bateu` e `espelho_fato_em_ausencia`, formados por `'espelho_' + c.veredito` para a tupla fechada `CelulaDia.VEREDITOS_ACUSAM`. Os protestos não cobráveis e a isenção de pausa podem suprimir o dia. `furo_espelho` é o motivo agregado do item do export. `ferias_com_batida` tem sua própria fonte de datas. `app/folha/export.py:1281` · `app/folha/export.py:1294` · `app/folha/export.py:1302` · `app/escala/models.py:1780` · `app/folha/export.py:977` · `app/folha/export.py:885`

`MotivoExaustivoTest` fica em `app/folha/tests/test_retrato_folha_quadros.py:314`. `_motivos_da_fonte` faz `ast.parse` de `export.py`, localiza `classificar_export` e colhe strings não vazias de atribuições a `['motivo']` e de pares literais em dicionários. O teste antívacuidade exige pelo menos seis códigos e dois exemplos concretos. O teste de cobertura subtrai os destinos declarados e exige conjunto vazio. Para o selo diário, o mesmo molde precisa colher as chamadas a `_retem` em `_dias_sem_decisao` e resolver a concatenação pelo AST de `VEREDITOS_ACUSAM`. Assim um emissor novo sem rótulo deixa o selo vermelho. Os textos dos oito rótulos estão [nao verificado]. `app/folha/tests/test_retrato_folha_quadros.py:255` · `app/folha/tests/test_retrato_folha_quadros.py:317` · `app/folha/tests/test_retrato_folha_quadros.py:323` · `app/folha/export.py:1268` · `app/folha/export.py:1302` · `app/escala/models.py:1780` · `aval.txt:2`

## 4. Janela do Q2

É o intervalo inclusivo `de_data` / `ate_data` do calendário do filtro universal. `cfiltro.ler(request.GET)` valida as datas ISO e troca pontas invertidas. Uma ponta só já recorta. `_recorte_da_barra` passa os limites a `no_periodo`, que usa as `datas` planas para ajustar `dias` e `saldo` em cópia e conserva a lista integral. A janela sozinha mantém as linhas no universo. A janela da **competência**, usada pela fonte na lavra, vem de `janela_fechamento` e do corte da empresa. `app/templates/core/_filtro_campos.html:43` · `app/core/filtro.py:68` · `app/core/filtro.py:98` · `app/folha/views_retrato.py:216` · `app/folha/retrato_folha.py:1334` · `app/folha/fora_da_folha.py:146` · `app/ponto/janelas.py:54`

Tela, PDF e Raio-X recebem esse recorte. Os links da lista e do Raio-X abrem o drawer com `mes` e `ano`. O drawer busca a linha integral e hoje mostra as datas ISO de toda a competência. `app/folha/views_retrato.py:230` · `app/folha/views_retrato.py:337` · `app/templates/folha/_retrato_lista.html:60` · `app/templates/folha/_retrato_raiox.html:68` · `app/colaboradores/views.py:994` · `app/templates/folha/_retrato_o_que_fazer.html:42`

## 5. Matriz de RED

| Caso | Asserção |
|---|---|
| **Linha nova** | Dar à fonte `03/11` e `05/11` com `espelho_furo` e `12/11` com `espelho_cobrado`. Conferir que a lavra preserva os três pares da chamada já feita e que o **Por quê** usa os rótulos do mapa do produtor, com cada data sob seu motivo. Cobrir também férias, cuja lista plana vem de outra fonte. `app/folha/fora_da_folha.py:115` · `app/folha/fora_da_folha.py:214` · `app/folha/retrato_folha.py:1231` · `aval.txt:2` |
| **Linha antiga sem a chave** | Abrir a mesma linha legada. Conferir o rótulo agregado na tela e no PDF, o tipo único e as datas ISO planas no drawer. Incluir legado anterior à E2, com `datas` ausente e Dias vazio sob janela. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/retrato_folha.py:1118` · `app/templates/folha/_retrato_o_que_fazer.html:42` · `app/folha/tests/test_retrato_folha_periodo.py:172` |
| **Janela que corta** | Com `05/11` a `12/11`, exigir `05/11` e `12/11` em **Por quê**, dois dias contados e a linha gravada intacta. Cobrir uma ponta só e janela que corta todos os dias. O selo atual de período compara todas as colunas textuais salvo Dias e precisa contemplar a mudança autorizada em **Por quê**. A saída textual quando sobram zero pares está [nao verificado]. `app/folha/retrato_folha.py:1334` · `app/folha/tests/test_retrato_folha_periodo.py:155` · `app/folha/tests/test_retrato_folha_periodo.py:371` · `aval.txt:2` |
| **Motivo sem rótulo** | Retirar uma entrada do mapa ou acrescentar um emissor no produtor. O selo AST deve falhar informando o código faltante. Uma varredura vazia também deve falhar. `app/folha/tests/test_retrato_folha_quadros.py:317` · `app/folha/tests/test_retrato_folha_quadros.py:323` · `aval.txt:2` |
| **Tela, PDF e drawer leem o mesmo** | Alimentar as três portas com a mesma linha persistida. Cobrar igualdade de rótulos e datas em tela e PDF sob a mesma query, e leitura dos mesmos pares no drawer. Cobrar zero chamadas ao retentor nesses leitores. A igualdade de apresentação e a janela no drawer dependem da pergunta 1. `app/folha/views_retrato.py:260` · `app/folha/views_retrato.py:388` · `app/colaboradores/views.py:1001` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_folha_drawer.py:213` |

## 6. Arquivos × obras em voo

| Área | Toque previsto e cruzamento |
|---|---|
| **Fonte e lavra** | `app/folha/export.py` para o mapa, `app/folha/fora_da_folha.py` para transportar os pares já disponíveis e `app/folha/retrato_folha.py` para persistir e ler a chave. Cruzamento informado com **CODIGO-DOMINIO-VALIDO** em `export.py` e `retrato_folha.py`, que hoje também tratam o bloqueio de cadastro e sua projeção. `app/folha/export.py:881` · `app/folha/export.py:1185` · `app/folha/fora_da_folha.py:234` · `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:1237` |
| **Tela, PDF e Raio-X** | `app/folha/views_retrato.py` contém as três projeções e é o cruzamento informado com **RETRATO c1**. Os outros sítios informados da c1 são `app/relatorios/utils_audit.py` e `app/config/settings/base.py`. A ligação atual com o PDF auditável passa por `gerar_pdf_auditavel` e `get_commit_git`. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/views_retrato.py:443` · `app/folha/views_retrato.py:503` · `app/relatorios/pdf_base.py:135` · `app/relatorios/utils_audit.py:7` · `app/config/settings/base.py:4` |
| **Drawer e cadastro** | Se o drawer herdar a janela, os pontos são os links da lista e do Raio-X, `app/colaboradores/views.py` e o parcial `_retrato_o_que_fazer.html`. `colaboradores/views.py` também contém a edição do código de folha. `folha/views.py` contém o fluxo do TXT. O diff efetivo das duas obras em voo e a sobreposição exata nos trechos de views e cadastro estão [nao verificado]. `app/templates/folha/_retrato_lista.html:60` · `app/templates/folha/_retrato_raiox.html:68` · `app/colaboradores/views.py:994` · `app/colaboradores/views.py:1356` · `app/templates/folha/_retrato_o_que_fazer.html:42` · `app/folha/views.py:275` |

Os pontos de teste a tocar são o contrato de chaves da lavra, o molde AST, a janela, a paridade do PDF e o contrato do drawer conforme o alcance aprovado. A regra herdada é a testemunha ler a mesma autoridade, com um escritor canônico por estado. `app/folha/tests/test_retrato_folha_quadros.py:314` · `app/folha/tests/test_retrato_folha_quadros.py:1425` · `app/folha/tests/test_retrato_folha_periodo.py:371` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/docs/LEIS.md:60` · `app/docs/LEIS.md:65`

## 7. Perguntas de lei

**0 para abrir a lavra e o recorte de tela/PDF.** Q1 e Q2 estão decididas no anexo. Restam dois detalhes de saída para fechar a matriz inteira. `aval.txt:2` · `CLAUDE.md:443`

1. **Drawer.** A paridade pedida inclui exibir os pares sob a mesma janela, inclusive ao abrir pelo Raio-X? Se inclui, o **Por quê** pode exibir rótulo com datas no próprio valor ou os pares devem aparecer em linhas de detalhe? O corte atual e o selo exigem valor literal de mapa no bloco. `app/docs/CORTES.md:405` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/templates/folha/_retrato_raiox.html:68`
2. **Janela sem pares.** Para uma linha nova cujos pares ficam todos fora da janela, o **Por quê** fica vazio ou conserva o rótulo agregado, sem datas? O código atual define o vazio da coluna Dias; a saída de **Por quê** nesse caso está [nao verificado]. `app/folha/retrato_folha.py:1343` · `app/folha/views_retrato.py:291` · `aval.txt:2`

## 28/11 01:39 · sessao fb68df3fdb61 · RETRATO-PDF-CORTA-POR-QUE · verificador · origem=Code

**Parecer de verificação — RETRATO-PDF-CORTA-POR-QUE. O item 5 volta.** No bind-mount documentado, o leitor novo procura em `app/logs/` no host e o deploy grava em `logs/` na raiz do repositório. O RED do item 5 substitui o caminho padrão e deixa essa ligação sem prova. No item 4, os contadores vêm da barra, mas a precedência das linhas é definida na view do PDF e o RED deixa a posição de `None` sem cobertura. `app/docs/INFRA.md:22–30`, `app/config/settings/base.py:4–5`, `bin/deploy.sh:432–436`, `app/folha/views_retrato.py:399–420`, `app/folha/tests/test_retrato_pdf_pacote.py:60–84,228–267,274–315`.

Conferi a cópia e o diff anexo em leitura. A identidade Git da cópia, a execução do RED em `31ef2f199`, o GREEN informado de 358 testes e a configuração efetiva dos containers estão [nao verificado].

## 1. Quem escreve e quem lê

`JUIZES.md` é a tabela de consulta. O registro vivo mantém `classificar_export` como autoridade do fechamento e `tarefa_do_dia` como autoridade da porta diária. A permissão da tela é `tem_acao`, chamada tanto pela tela quanto pelo PDF. O PDF lê o retrato lavrado e delega a classificação visual a `led_da_linha`. Não encontrei uma segunda implementação desses juízes no diff. `app/docs/JUIZES.md:10–15,51,81–90,119–122`, `app/core/juizes.py:311,326–340`, `app/folha/views_retrato.py:257,377,391–405`, `app/folha/retrato_folha.py:516–532`.

| Campo ou valor | Quem produz ou grava | Quem lê |
|---|---|---|
| `RetratoFolha.linhas`, `colab`, `status`, `motivo`, `dias`, `datas`, `tipo`, `saldo`, `dono` | `classificar_export` entrega ID, estado e motivo. `fora_da_folha` entrega dias, datas e grupo de fio para o furo. `montar_linhas` compõe o JSON e `lavrar_retrato_folha --apply` grava por empresa, mês e ano. `app/folha/export.py:846–858,875–893,968–990`, `app/folha/fora_da_folha.py:146–165,210–253`, `app/folha/retrato_folha.py:1222–1255`, `app/folha/management/commands/lavrar_retrato_folha.py:65–79` | `linhas_do_retrato` abre o JSON. `_recorte_da_barra` monta a lista que tela, PDF e Raio-X leem. O drawer do colaborador e o vigia também passam pelo leitor do retrato. `app/folha/retrato_folha.py:98–111,930–953`, `app/folha/views_retrato.py:194–232,260–268,391–397,507–524`, `app/colaboradores/views.py:998–1004`, `app/folha/vigia_retido_calado.py:76–79` |
| `dono` gravado, dono de UI, cor e rótulo | `montar_linhas` grava o **fio** em `dono`. `fio_da_linha` o lê, com fallback para retrato legado. `dono_da_linha` projeta o balde da UI, incluindo `chamado → Admin` e `ninguem → Hoje` quando a primeira data é hoje ou posterior. `led_da_linha` acrescenta Entram e Rescisão como estados de linhas sem dono. `app/folha/retrato_folha.py:191–201,436–493,516–532,1246–1248` | Filtro, barra e drawer leem o dono de UI. Tela e PDF leem cor e rótulo via `desenho_do_led`. O PDF também lê a chave do LED para ordenar. O texto de `motivo` passa por `rotulo_do_motivo`, que lê `MOTIVO_RH`, também usado pela prévia e pelo PDF de retidos. `app/folha/retrato_folha.py:559–566,587–597,908–915,1112–1126,76–86`, `app/folha/views_retrato.py:286–292,399–405,435–446`, `app/folha/views.py:15–24,373,415,884–887` |
| Empresa, competência, filtros, `vis`, `universo`, `hoje` | `_recorte_da_barra` lê GET e preferência, usa `janela_atual` para o default, lê um instante e monta o recorte com `core.filtro`, `no_periodo`, `ler_grupos` e `recortar`. Quando vem `empresa=`, a própria porta chama `PreferenciaUI.set_pref`, inclusive no acesso pelo PDF. `app/folha/views_retrato.py:149–184,216–247`, `app/colaboradores/models.py:204–216` | Tela, PDF e Raio-X leem o recorte. `qs_do_recorte` produz o endereço do botão e a querystring guardada na auditoria. `core.filtro.chips` produz os rótulos e textos dos filtros ativos usados pela tela e pelo PDF. `app/folha/views_retrato.py:260–269,295–300,337,391–417,461–467,507–509`, `app/folha/retrato_folha.py:775–782`, `app/core/filtro.py:232–275` |
| `titulo`, `itens`, `ativo`, `rotulo`, `numero` da barra | `quadros_barra_retrato` monta o quadro. `_pilula` produz `numero` em **pessoas**. Para a pilula acesa, conta o recorte atual. Para a apagada, conta o recorte que sua ativação abriria. `app/folha/retrato_folha.py:785–813,838–845,879–915` | A barra e o PDF leem o mesmo `numero`. O PDF usa `ativo` para descrever o recorte e copia os pares rótulo/número de “Depende de”. O `n/total` do cabeçalho é outra medida, `len(vis)/len(linhas)`, tal como no filtro da tela. `app/templates/core/_barra_gestao.html:65–67`, `app/folha/views_retrato.py:315,324–325,411–430` |
| Razão social, nome, posto e descrição da escala | As portas de cadastro gravam empresa e posto, colaborador, vínculo de escala e `TipoEscala.descricao`. `_cadastro` projeta nome, posto e tipo de escala em memória, escolhendo o vínculo ativo mais recente por `data_inicio` e usando o posto do colaborador como fallback. `app/colaboradores/services/estrutura.py:13–22,43–45,67–79`, `app/colaboradores/views.py:1331–1333,1362–1365`, `app/colaboradores/services/colaborador.py:152–160,204–206`, `app/colaboradores/services/vinculo.py:274–292`, `app/escala/services/cadastro_tipo.py:184–188,307,382–401,431–446`, `app/folha/views_retrato.py:99–123` | O PDF usa a razão social no título e lê nome, posto e descrição nas células. A tela lê a mesma projeção cadastral. O item 3 passa a descrição por `escape` e `Paragraph` na coluna de 38 mm. Seu RED exige `Paragraph` e texto longo integral. A quebra visual na página e o comportamento com `<`, `>` e `&` estão [nao verificado]. `app/folha/views_retrato.py:395,424–425,435–450`, `app/templates/folha/_retrato_lista.html:60–66`, `app/folha/tests/test_retrato_pdf_pacote.py:47–48,210–222` |
| PDF, metadados e registro auditável | `retrato_pdf` fornece título, usuário, período, `tipo='outro'`, recorte e totais. `gerar_pdf_auditavel` escreve `RegistroPDFAuditavel`, incluindo `filtros_json`, `total_registros`, `commit_git` e o SHA-256 do PDF canônico. A view escreve `Content-Disposition` na resposta. `app/folha/views_retrato.py:461–475`, `app/relatorios/pdf_base.py:133–178` | O rodapé lê o commit e o SHA-256. A verificação pública busca pelo SHA-256 do PDF e exibe o commit armazenado, os filtros e o total. `app/relatorios/pdf_base.py:72–87`, `app/chamados/views.py:1202–1222`, `app/templates/chamados/verificar_pdf.html:37–50` |

## 2. Item 4 — `dias=None`, ordem e RED

- **O leitor que produz `None` é `no_periodo`.** Com pelo menos um limite, ele copia a linha e põe `dias=None` se `datas` estiver ausente, nula, vazia ou sem interseção com a janela inclusiva. Isso vale para qualquer motivo. Sem janela, conserva o valor lavrado. A cópia preserva `datas`, então uma linha pode continuar no dono Hoje com a coluna de dias vazia. `app/folha/retrato_folha.py:486–493,1334–1347`.
- **A lavra também produz `None`.** Entram, Rescisão, `sem_codigo_dominio`, `cadastro_zero` e `nao_homologado` chegam sem lista de dias. Férias com batida entrega datas. Para `furo_espelho`, `fora_da_folha` pode entregar o número `0` se a lista estiver vazia, e a lavra preserva esse zero. Tela e PDF exibem `None` em branco e `0` como número. Quantas linhas caem em cada caso em produção [nao verificado]. `app/folha/export.py:846–858,875–893,968–990`, `app/folha/fora_da_folha.py:146–165,234–246`, `app/folha/retrato_folha.py:1227–1243`, `app/templates/folha/_retrato_lista.html:66`, `app/folha/views_retrato.py:443`.
- **O RED de ordem deixa `None` sem cobertura.** A bancada tem `None` só em Entram e Rescisão, sem números nesses mesmos estados. Rodei em memória a chave extraída do código com a bancada, retirando apenas `dias is None`. A ordem esperada continuou igual. No controle sintético de mesmo dono, com ID 1 em `None` e ID 2 em `0`, a chave atual ordenou `[2, 1]` e a mutação ordenou `[1, 2]`. Os selos anteriores cobrem a produção de `None` na janela e na lavra. A decisão de posição no papel fica [nao verificado]. `app/folha/views_retrato.py:399–406`, `app/folha/tests/test_retrato_pdf_pacote.py:52–99,228–243`, `app/folha/tests/test_retrato_folha_periodo.py:126–153`, `app/folha/tests/test_retrato_folha_quadros.py:1409–1418`.
- **A ordem da tela ficou intacta.** `_recorte_da_barra` continua a fazer `ordenar(recortar(...))`, por balde, faixa crescente de saldo e ID. A view projeta `_vis` na mesma sequência e o template percorre `tabela` na página cheia e no parcial. Só o PDF cria outra lista com `sorted`. A guarda do pacote exige as mesmas pessoas e uma ordem diferente da do papel. O teste anterior de `ordenar` fixa a sequência esperada. `app/folha/views_retrato.py:225–232,260–268,278–293,318–346,391–406,435`, `app/folha/retrato_folha.py:754–772`, `app/templates/folha/_retrato_lista.html:35`, `app/templates/folha/retrato_folha.html:53`, `app/templates/folha/_retrato_parcial.html:7`, `app/folha/tests/test_retrato_pdf_pacote.py:245–249`, `app/folha/tests/test_retrato_folha_quadros.py:566–591`.
- **Fonte única.** O PDF lê o recorte existente e os números publicados pelo quadro. O RED compara esses números em dois recortes. O selo AST guarda a porta do universo, e a origem dos contadores é demonstrada pelo código. Para a ordem das linhas, a view lê `vis`, `dias`, `colab` e a chave do LED, mas escreve localmente a precedência `Sem ação → Sem leitura → Admin → Colab → Hoje → Entram → Rescisão`. O quadro publica os donos em outra sequência. Assim, a ordem reaproveita o julgamento de dono e tem regra de apresentação própria. Se o checklist exige que **a precedência** também venha publicada por `quadros_barra_retrato` ou `_recorte_da_barra`, essa exigência fica aberta. `app/folha/views_retrato.py:399–420`, `app/folha/retrato_folha.py:191–201,590–591,908–915`, `app/folha/tests/test_retrato_pdf_pacote.py:251–267`, `app/folha/tests/test_retrato_pdf.py:301–350`.

## 3. Item 5 — carimbo, alcance e efeitos

- **Caminho em produção.** A configuração SaaS herda `BASE_DIR / 'logs' / 'deploy.stamp'`, que no container vira `/app/logs/deploy.stamp`. O bind documentado o resolve para `/home/ronald/saas-hasner/app/logs/deploy.stamp`. `bin/deploy.sh` grava `/home/ronald/saas-hasner/logs/deploy.stamp`, via arquivo temporário e `mv`. Esse bind não expõe o arquivo escrito. Compose efetivo, montagem adicional, cópia manual e ambiente `COMMIT_GIT` do worker [nao verificado]. `app/config/settings/base.py:4–5`, `app/config/settings/saas.py:10,118`, `app/docs/INFRA.md:22–30`, `bin/deploy.sh:432–436`, `.gitignore:40–41`.
- **Comparação com o ramo antigo.** No cenário dado, sem `COMMIT_GIT`, sem `/app/.git` e sem arquivo no novo caminho, ambos retornam `''`. O rodapé segue `n/d` e o registro segue vazio. Um `COMMIT_GIT` não vazio continua prioritário, com `strip()` e limite de 40 caracteres. Onde `/app/.git` existia e funcionava, perde-se o fallback antigo. Se o carimbo ficar acessível, falha de leitura ou UTF-8 e `QUANDO` não numérico podem agora interromper a geração. O parser devolve `{}` quando falta `QUANDO`. Falhas reais em produção [nao verificado]. `obra.diff` anexo, hunk de `app/relatorios/utils_audit.py:1`, `app/relatorios/utils_audit.py:11–20`, `app/core/esteira_vigia.py:1032–1042`, `app/relatorios/pdf_base.py:72–77,135,164`.
- **Quem escreve e lê `COMMIT`.** O deploy escreve epoch em `QUANDO`, `git rev-parse HEAD` inteiro em `COMMIT` e o assunto em `TITULO`. O PDF reaproveita o parser do vigia e usa só `COMMIT`, limitado a 40 caracteres e sem validação de formato. O registro conserva esse valor e o rodapé exibe oito caracteres. O vigia lê no host e usa o SHA inteiro em `git revert`. A guarda de auth e o verificador de imports também leem o SHA do host, e o handoff o abrevia para exibição. Quem injeta `COMMIT_GIT` em produção [nao verificado]. `bin/deploy.sh:432–436`, `app/core/esteira_vigia.py:64,1032–1042,1202,1297–1309,1355`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/models.py:56`, `app/relatorios/pdf_base.py:72,164`, `bin/janela_auth.sh:39–45`, `bin/import_tardio_contra_o_ar.py:45–55`, `bin/handoff_sessao.sh:274–277`.
- **Dois chamadores diretos.** `gerar_pdf_auditavel` usa o mesmo commit nas duas passagens e no registro. Alcança retrato, espelho mensal, histórico de chamados, gestão de férias e gestão de HE. Se o commit impresso mudar, muda também a entrada do SHA-256 canônico. O outro chamador é o espelho informacional, usado no avulso por datas e no lote. Ele usa o commit só no rodapé. Uma falha comum aos dois caminhos pode fazer o lote pular a pessoa. `app/relatorios/pdf_base.py:133–178`, `app/folha/views_retrato.py:461–467`, `app/relatorios/pdf_espelho.py:1066–1074,1106–1116`, `app/relatorios/pdf_chamados.py:158–176`, `app/relatorios/services.py:235–255,338–347`, `app/relatorios/views.py:1135–1164`, `app/ponto/views.py:886–894`.
- **RED e ordem de publicação.** O RED prova leitor, persistência e passagem do SHA aos dois objetos de rodapé, com arquivo temporário e `override_settings`. O controle prova precedência da variável. O caminho padrão e os caracteres efetivamente impressos ficam fora das asserções. O deploy recarrega e prova as rotas **antes** de trocar o carimbo. Mesmo se o arquivo ficar visível, essa ordem permite código novo com carimbo anterior ou ausente. No fluxo `--reload-copia`, o reload ocorre antes do commit da cópia, e o carimbo lê o `HEAD` naquele momento. Ocorrência no ar [nao verificado]. `app/folha/tests/test_retrato_pdf_pacote.py:274–322`, `bin/deploy.sh:357–373,416–436`, `bin/molde_fatia/cadeia.sh:35–48,62–69`.

## 4. Import, relógio e travas

O import novo é `utils_audit → core.esteira_vigia`. O topo do vigia só importa biblioteca padrão, registra um logger, inicializa constantes e compila sete regex. A cadeia estática não volta a `relatorios`. O corpo do módulo não lê os arquivos operacionais nem resolve settings, e o CLI está protegido por `__main__`. Na chamada sem `COMMIT_GIT`, o helper resolve o setting, consulta o caminho e lê o arquivo se presente. Num processo local novo, `-X importtime` mediu cerca de **26 ms cumulativos** para importar o vigia e suas dependências, sem carregar Django. Custo incremental no worker e memória [nao verificado]. `app/relatorios/utils_audit.py:2–20`, `app/core/esteira_vigia.py:27–36,38–109,351–357,605,1202,1297,1355,1653–1654`.

O `hoje` novo na ordenação vem do instante já capturado na borda de `_recorte_da_barra`. Esse mesmo dia chega à barra e ao LED. O timestamp da auditoria segue uma leitura própria, explicitamente fora do contrato do recorte. Os trechos novos não adquirem trava. O pipeline mantém bloqueios fornecidos pelo chamador antes da geração, com a gravação entre as duas passagens. O retrato não fornece callbacks de bloqueio. O selo de relógio verifica o congelamento em testes, e a bancada está sob `freeze_time`. `app/folha/views_retrato.py:146–147,177–184,244–247,399–446,461–467`, `app/folha/retrato_folha.py:452–465`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/pdf_base.py:127–178`, `app/core/tests/test_selo_teste_sem_relogio.py:67–84`, `app/folha/tests/test_retrato_pdf_pacote.py:121–123`.

## 5. Item 1 — alcance do `attachment`

**Nenhum outro consumidor do partial encontrado aponta para essa rota.** `retrato_pdf` escreve o novo header apenas na própria resposta. A barra já fornece `pdf_href`, que o partial renderiza como âncora comum, sem `target`. Os outros consumidores continuam usando seus próprios destinos e headers, como Gestão HE (`inline`) e Furos (`attachment`). O RED exige `attachment` com o nome canônico e verifica a âncora na página cheia e no parcial OOB. O download efetivo no navegador [nao verificado]. `app/folha/urls.py:25`, `app/folha/views_retrato.py:337,468–475`, `app/templates/folha/_retrato_barra.html:50`, `app/templates/core/_btn_pdf.html:34–46`, `app/ponto/views.py:899`, `app/relatorios/views.py:1632`, `app/folha/tests/test_retrato_pdf_pacote.py:187–204`.

## 28/11 01:38 · sessao 39f9bf608e62 · CODIGO-DOMINIO-VALIDO · construtor · origem=Code

A cura está no diff da cópia. A revisão estática encontrou **13 arquivos de produção alterados e 0 `except` adicionados**. Os dois módulos do contrato declaram **22 casos, dos quais 15 têm prefixo `test_RED_`**. Resultado da suíte e GREEN [nao verificado]. [REDs da Folha](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:90) [REDs do Cadastro](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:76) [nao verificado]

### Diff por item

- **Juiz e TXT.** Um predicado compartilhado exige dígitos ASCII após `strip()`, sem impor comprimento. `classificar_export` preserva o valor no item e atribui o motivo literal `codigo_dominio_invalido` entre código ausente e férias com batida. O montador já seleciona os itens `entra`. [colaboradores/services/cadastro.py:53](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/services/cadastro.py:53) [folha/export.py:850](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:850) [folha/export.py:882](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:882) [folha/export.py:723](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/export.py:723)
- **Cadastro.** Admissão e edição reexibem o formulário com a mensagem «Código Domínio deve conter apenas números.» antes dos respectivos pontos de gravação. O campo vazio segue aceito na admissão e preserva o anterior na edição. A validação fica nas duas portas de tela. [colaboradores/views.py:1135](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1135) [colaboradores/views.py:1239](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1239) [colaboradores/views.py:1296](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1296) [colaboradores/views.py:1365](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/views.py:1365) [core/respostas.py:74](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/core/respostas.py:74)
- **Retrato.** `MOTIVO_RH` dá o rótulo, `TIPO_POR_MOTIVO` dá `cadastro` e o fio existente dá `admin`. Um único leitor de `linha['codigo']` monta `codigo Dominio = <valor>` para lista, PDF, drawer e Raio-X. No drawer, `valor` continua sendo o rótulo literal do mapa. [folha/views.py:18](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views.py:18) [folha/retrato_folha.py:223](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:223) [folha/retrato_folha.py:382](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:382) [folha/retrato_folha.py:89](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/retrato_folha.py:89) [templates/folha/_retrato_lista.html:65](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_lista.html:65) [folha/views_retrato.py:423](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views_retrato.py:423) [templates/folha/_retrato_o_que_fazer.html:37](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_o_que_fazer.html:37) [templates/folha/_retrato_raiox.html:73](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/folha/_retrato_raiox.html:73)
- **Listas fechadas.** `_MOTIVO_NA_TELA` ganhou pílula própria, `_PORTAS_CADASTRO` ganhou a natureza correspondente e Quem Cobrar lê o rótulo de `MOTIVO_RH`. `MotivoExaustivo` já colhe a atribuição literal por AST e verifica seu destino no mapa. O Raio-X do export já conta a chave em **Outras pendências**. [ponto/services/fechamento_lista.py:28](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/ponto/services/fechamento_lista.py:28) [templates/ponto/_fech_linha.html:19](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/templates/ponto/_fech_linha.html:19) [relatorios/views_pend.py:50](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views_pend.py:50) [relatorios/views.py:1729](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729) [folha/tests/test_retrato_folha_quadros.py:325](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_retrato_folha_quadros.py:325) [folha/views.py:625](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/views.py:625)

### REDs e mutantes para o Opus

| Caso RED | Mutante a derrubar |
|---|---|
| No lote misto, `VIGIA` e `１２３` ficam fora, `00123` gera `0000000123` e o layout retorna `[]`. Em colisão com férias com batida, vence o código inválido. Vazio e brancos mantêm `sem_codigo_dominio`. | Usar só `isdigit()`, remover a retenção, mover a checagem para depois de férias ou eliminar também o controle numérico. [TXT](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:90) [Precedência](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:152) |
| Conferir rótulo, tipo, pílula, natureza e presença literal na fonte. Em Quem Cobrar, conferir o rótulo na linha retida. | Apagar uma entrada de mapa, apontar a pílula para espelho ou trocar a atribuição literal do motivo por uma referência indireta. [Mapas e AST](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:163) [Quem Cobrar](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/relatorios/views.py:1729) |
| Lavrar `VIGIA` com tipo `cadastro` e dono `admin`, corrigir o cadastro para `00123` e conferir o valor histórico na lista, PDF e drawer. | Buscar o código atual, omitir o complemento em um leitor ou interpolá-lo em `valor` no drawer. [Retrato](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:241) |
| Enviar `VIGIA` nas duas portas e exigir formulário HTTP 200 e mensagem visível. Na admissão, conferir as contagens de usuários e colaboradores. Na edição, conferir código e telefone anteriores. | Validar depois da gravação, redirecionar a recusa ou levar a regra ao modelo e impedir a criação do legado usado na fixture. [Admissão](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:85) [Edição](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/colaboradores/tests/test_codigo_dominio_numerico.py:118) [Fixture legada](/home/ronald/wt-codex-codigo-dominio-valido-013223/app/folha/tests/test_codigo_dominio_invalido.py:64) |

### Comando do GREEN

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-codigo-dominio-valido-013223 --only "folha.tests.test_codigo_dominio_invalido colaboradores.tests.test_codigo_dominio_numerico folha.tests.test_export ponto relatorios core.tests.test_selo_teste_sem_relogio"
```

## 28/11 01:37 · sessao 8ac04350367a · - · - · origem=Ronald (tmux)

**Item 4. O RED deixa a posição de `dias=None` em aberto.** O teste de ordem exige a sequência por dono, dias decrescentes e desempate por ID. Na bancada, `None` só aparece em Entram e Rescisão, sem dias numéricos nesses estados. Retirar a chave `dias is None` preservaria a ordem esperada da bancada. Para distinguir a chave de `-(dias or 0)` em um teste, o mesmo estado precisa de `0` e `None` com o ID menor no `None`. O zero é possível em `furo_espelho` cujo relatório devolve `len([])` e chega à lavra como zero. `app/folha/tests/test_retrato_pdf_pacote.py:52–98`, `:228–243`, `app/folha/views_retrato.py:399–406`, `app/folha/fora_da_folha.py:210–246`, `app/folha/retrato_folha.py:1227–1236`.

**Origens de `None`.** Na lavra, `montar_linhas` recebe a contagem de `furo_espelho` de `fora_da_folha` e usa a lista de férias quando essa contagem vem ausente. Sem as duas fontes, grava `None`. Isso abrange Entram, Rescisão, `sem_codigo_dominio`, `cadastro_zero` e `nao_homologado`. O leitor `no_periodo`, com pelo menos um limite temporal, cria uma cópia com `dias=None` quando `datas` está ausente/nula, vazia ou fora da janela. Isso se aplica a qualquer motivo. Sem janela, mantém os valores da lavra. Há testes para esses vazios, inclusive retrato legado, e para a faixa Cadastro permanecer. `app/folha/export.py:846–858`, `:875–893`, `:968–989`, `app/folha/fora_da_folha.py:146–165`, `:234–246`, `app/folha/retrato_folha.py:1222–1243`, `:1334–1347`, `app/folha/tests/test_retrato_folha_periodo.py:126–153`, `:172–183`, `app/folha/tests/test_retrato_folha_quadros.py:1394–1417`.

**Ordem da tela.** `_recorte_da_barra` segue produzindo `ordenar(recortar(...))`, com ordem por balde, faixa de saldo e ID. A tela projeta `_vis` na mesma ordem e o template percorre `tabela`. O PDF cria outra lista com `sorted(_vis, ...)`. O teste da tela no pacote confere mesmas pessoas e ordem diferente do PDF. A ordem exata da função e seu comportamento com o recorte de datas estão em testes anteriores. `app/folha/views_retrato.py:225–232`, `:260–268`, `:278–293`, `:318–346`, `:391–406`, `:435`, `app/folha/retrato_folha.py:754–772`, `app/templates/folha/_retrato_lista.html:35`, `app/folha/tests/test_retrato_pdf_pacote.py:245–249`, `app/folha/tests/test_retrato_folha_quadros.py:566–590`, `app/folha/tests/test_retrato_folha_periodo.py:354–368`.

**Juízes, campos e contadores.** O gravado `dono` é o fio escrito por `montar_linhas` e lido por `fio_da_linha`. O dono de UI é produzido por `dono_da_linha` e usado pela barra e pelo LED, com dois estados adicionais do LED para Entram e Rescisão. `hoje` vem da borda e é passado a todos os leitores. `_pilula` publica `numero` como quantidade de pessoas do recorte atual se acesa ou do recorte após ativação se apagada. Tela e PDF leem o número de `quadros_barra_retrato`. A ordem do PDF usa a chave e as linhas de suas fontes, com uma precedência local de apresentação distinta da sequência publicada no quadro. O RED de contadores compara resultados em dois recortes. O selo AST anterior guarda a fonte do universo, enquanto a dependência de `quadros_barra_retrato` é comprovada pelo código. `app/folha/management/commands/lavrar_retrato_folha.py:65–79`, `app/folha/retrato_folha.py:191–201`, `:436–493`, `:516–532`, `:588–597`, `:816–842`, `:904–915`, `:1246–1248`, `app/folha/views_retrato.py:177–182`, `:246–247`, `:324–325`, `:399–420`, `app/templates/core/_barra_gestao.html:67`, `app/folha/tests/test_retrato_pdf_pacote.py:251–267`, `app/folha/tests/test_retrato_pdf.py:301–350`.

**Item 3.** O RED verifica `Paragraph` e preservação do texto, com um exemplo longo sem metacaracteres. A troca para `Paragraph(escape(descricao))` acontece no leitor PDF. A geometria da quebra nas páginas e um exemplo com `<`, `>` ou `&` não recebem asserção direta. `app/folha/tests/test_retrato_pdf_pacote.py:46–48`, `:164–173`, `:210–222`, `app/folha/views_retrato.py:424`, `:441`, `:449–450`. Execução da suíte e distribuição em produção [nao verificado].

## 28/11 01:37 · sessao ca3696a25ee3 · - · - · origem=Ronald (tmux)

- **Import.** `utils_audit` importa o vigia no nível do módulo. O vigia importa apenas biblioteca padrão na carga, inicializa logger, constantes e sete regexes. A leitura do carimbo e o acesso ao valor em `settings` acontecem na chamada de `get_commit_git()` sem `COMMIT_GIT`. A rotina CLI fica sob `__main__`. A cadeia estática desse import não tem volta a `relatorios`. Tempo e memória adicionais [nao verificado]. `app/relatorios/utils_audit.py:2-20`, `app/core/esteira_vigia.py:27-36,38-81,83,90,101,104-109,351-357,605,1202,1297,1355,1653-1654`.
- **Attachment.** Só a resposta de `folha:retrato_pdf` recebe o novo `Content-Disposition`. A barra passa o URL dessa rota como `pdf_href`, que já era âncora direta sem `target`. Os modos submit e `hxAbrir` continuam definidos pelo mesmo partial e nenhum outro consumidor encontrado usa essa rota. Há exemplos independentes de inline em Cadastro × Realidade, HE e follow-up, e de attachment em Furos, Inteligência e histórico de exportações. `app/folha/urls.py:25`, `app/folha/views_retrato.py:337,461-475`, `app/templates/folha/_retrato_barra.html:50`, `app/templates/core/_btn_pdf.html:34-46`, `app/escala/views.py:528`, `app/ponto/views.py:899`, `app/relatorios/views.py:1110,1632`, `app/inteligencia/views.py:643`, `app/folha/views.py:861`.
- **Mordida.** O RED do item 1 exige attachment com nome canônico. A verificação da âncora é guarda de markup da página cheia e do parcial OOB. Download efetivo no navegador [nao verificado]. O RED de Escala exige `Paragraph` e texto integral com um caso longo. Quebra visual e caracteres especiais [nao verificado]. `app/folha/tests/test_retrato_pdf_pacote.py:47-49,187-204,210-222`.
- **Relógio e ordem do deploy.** O instante do recorte vem da borda, vira `hoje` e alimenta as novas chamadas. O timestamp da auditoria tem relógio próprio preexistente e exclusão explícita do contrato do recorte. Os trechos novos não adquirem locks. O reload e as sondas de rota vêm antes do `mv` do carimbo no host. Se esse carimbo passar a ser visível no container, há uma janela possível de código novo com stamp velho ou ausente. `app/folha/views_retrato.py:146-147,177-184,244-247,399-446`, `app/relatorios/pdf_base.py:129-135`, `app/relatorios/utils_audit.py:13-20`, `bin/deploy.sh:357-373,416-419,432-436`.

## 28/11 01:36 · sessao 5e85d18b9370 · - · - · origem=Ronald (tmux)

**Item 5 deve voltar.** A montagem documentada leva `/home/ronald/saas-hasner/app` a `/app`. O novo default aponta para `/app/logs/deploy.stamp`, equivalente a `app/logs/deploy.stamp` no host. `bin/deploy.sh` grava `logs/deploy.stamp` na raiz do repositório, por arquivo temporário e `mv`. Assim, o bind-mount documentado não expõe o carimbo ao PDF. O Compose efetivo, montagens extras e `COMMIT_GIT` do processo ficam [nao verificado]. `app/docs/INFRA.md:22–30`, `app/ponto/management/commands/apurar_furo_da_exportada.py:25–28`, `app/config/settings/base.py:4–5`, `app/config/settings/saas.py:10`, `bin/deploy.sh:432–436`, `.gitignore:40–41`.

- **Efeito na produção descrita.** Com `COMMIT_GIT`, a precedência e o limite de 40 caracteres seguem iguais. Sem a variável, sem `/app/.git` e sem carimbo no caminho novo, os dois ramos devolvem `''`. O rodapé fica `n/d` e o registro guarda vazio. Ambientes com checkout em `/app/.git` perdem aquele fallback. Se o carimbo passar a ser acessível, `read_text` sem tratamento pode propagar falha de leitura ou UTF‑8. `ler_deploy_stamp` devolve `{}` sem `QUANDO` e pode levantar `ValueError` com `QUANDO` não numérico. [nao verificado] o estado real desses arquivos. `obra.diff` anexo, `app/relatorios/utils_audit.py:11–20`, `app/core/esteira_vigia.py:1032–1042`, `app/relatorios/pdf_base.py:72–77,135,164`.
- **Consumidores.** `gerar_pdf_auditavel` lê uma vez e passa o valor às duas fases e a `RegistroPDFAuditavel.commit_git`. Isso alcança retrato, espelho mensal, histórico de chamados, gestão de férias e gestão de HE. O espelho informacional é o outro chamador direto e usa o valor só no rodapé. O lote usa os dois caminhos e pode pular o colaborador se ambos levantarem exceção. `app/relatorios/pdf_base.py:133–170`, `app/folha/views_retrato.py:461–467`, `app/relatorios/pdf_espelho.py:1041–1074,1106–1116`, `app/relatorios/pdf_chamados.py:158–176`, `app/relatorios/services.py:235–255,338–347`, `app/ponto/views.py:886–894`.
- **Semântica.** O shell escreve `git rev-parse HEAD` no `COMMIT`. O helper retorna até 40 caracteres sem validar formato. O rodapé mostra oito, a verificação pública mostra o valor persistido inteiro e a consulta usa o SHA‑256 do PDF canônico. O vigia passa o `COMMIT` inteiro ao `git revert` e usa oito só nas mensagens. `bin/deploy.sh:432–436`, `app/relatorios/utils_audit.py:13–20`, `app/relatorios/models.py:56`, `app/relatorios/pdf_base.py:72,147–164`, `app/chamados/views.py:1202–1222`, `app/templates/chamados/verificar_pdf.html:37,49`, `app/core/esteira_vigia.py:1297–1309`.
- **Alcance do RED.** Os testes criam carimbo temporário e substituem o setting. Checam o valor devolvido, o registro e o argumento recebido pelos dois objetos de rodapé. O controle cobre a precedência de `COMMIT_GIT`. O caminho padrão, a montagem e o texto efetivamente renderizado estão fora dessas asserções. `app/folha/tests/test_retrato_pdf_pacote.py:274–322`.
- **Import.** O topo de `core.esteira_vigia` carrega biblioteca padrão, cria logger, constantes e compila sete regex. As leituras ficam dentro de funções e o `main` é protegido por `__name__`. A inspeção estática não mostra ciclo nem leitura de disco ou settings pelo corpo do módulo. `utils_audit` resolve o setting na chamada. Tempo de import [nao verificado]. `app/core/esteira_vigia.py:27–36,64,83–109,351–357,605,1653–1654`, `app/relatorios/utils_audit.py:6–20`.

## 28/11 01:34 · sessao df0356fdefca · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

```python
# Execução na sombra e contagens do recorte [nao verificado].
import re
from datetime import date

from django.conf import settings
from django.db import connection, transaction
from django_tenants.utils import schema_context

# Guarda da sombra (app/config/settings/sombra.py:29,49,60).
if (
    getattr(settings, "HASNER_SOMBRA", False) is not True
    or connection.settings_dict["NAME"] != "sombra"
):
    raise SystemExit(1)

from colaboradores.models import Empresa
from folha.export import classificar_export, janela_competencia

mes, ano = 11, 2026
entra = 0
maior_que_10 = []
nao_ascii_digito = []

# Schema do executor (bin/sonda_leitura.sh:239-240).
with schema_context("juliani"), transaction.atomic(durable=True):
    with connection.cursor() as cursor:
        # Transação somente leitura (bin/gate_cobranca.sh:23-25).
        cursor.execute("SET TRANSACTION READ ONLY")

    empresa = Empresa.objects.get(pk=2)
    # Janela pelo corte da empresa (app/folha/export.py:47-51 e app/ponto/janelas.py:36-57).
    if janela_competencia(empresa, mes, ano) != (date(2026, 10, 21), date(2026, 11, 20)):
        raise SystemExit(2)

    # Fechamentos gravados e código já normalizado com strip (app/folha/export.py:774-797,846-850).
    for item in classificar_export(empresa, mes, ano):
        if item["status"] != "entra":
            continue
        entra += 1
        codigo = item["codigo_dominio"]
        pk = item["colaborador_id"]
        if len(codigo) > 10:
            maior_que_10.append(pk)
        if re.fullmatch(r"[0-9]+", codigo) is None:
            nao_ascii_digito.append(pk)

    transaction.set_rollback(True)

for pk in sorted(maior_que_10):
    print(f"maior_que_10|col{pk:03d}")
for pk in sorted(nao_ascii_digito):
    print(f"nao_ascii_digito|col{pk:03d}")

print(
    f"S11|entra={entra}|maior_que_10={len(maior_que_10)}"
    f"|nao_ascii_digito={len(nao_ascii_digito)}"
)
```

## 28/11 01:25 · sessao 5f422d0683f6 · RETRATO-PDF-CORTA-POR-QUE · construtor · origem=Code

Implementei os itens 1, 3, 4 e 5 em três arquivos de produção. O item 2 e os arquivos de teste ficaram fora do diff. `folha/views_retrato.py:350` · `relatorios/utils_audit.py:11` · `config/settings/base.py:5`

### Cura por item

- **1 — Download.** A rota responde `attachment; filename="retrato-folha-emp<pk>-<MM>-<AAAA>.pdf"`. O link continua no partial único, sem `target` nem atributos `hx-*`. `folha/views_retrato.py:473` · `templates/folha/_retrato_barra.html:50` · `templates/core/_btn_pdf.html:34`
- **3 — Escala.** A descrição inteira entra em `Paragraph(escape(...))` na coluna de 38 mm. `folha/views_retrato.py:424` · `folha/views_retrato.py:441` · `folha/views_retrato.py:450`
- **4 — Ordem e contadores.** Só o papel ordena por Sem ação → Sem leitura → Admin → Colab → Hoje → Entram → Rescisão, com mais dias primeiro e ID crescente no empate. Dentro de cada dono, coloquei `dias=None` depois das linhas com número. O topo imprime `Rótulo: N` com o `numero` de cada pílula de `Depende de` retornada por `quadros_barra_retrato`. A ordem da tela, o recorte, a contagem e o cabeçalho da tabela foram preservados. `folha/views_retrato.py:232` · `folha/views_retrato.py:324` · `folha/views_retrato.py:399` · `folha/views_retrato.py:418` · `folha/views_retrato.py:432` · `folha/views_retrato.py:467`
- **5 — Carimbo.** `COMMIT_GIT` preenchida tem precedência. Em seguida, o leitor único usa `ler_deploy_stamp` pelo caminho `settings.HASNER_COMMIT_STAMP`. Arquivo ausente retorna `''`. O padrão resolve para `/app/logs/deploy.stamp` no container. O escritor grava no `logs/` da raiz do host e o bind documentado monta `app/` em `/app`. A exposição do carimbo nesse caminho em produção está [nao verificado]. `relatorios/utils_audit.py:11` · `core/esteira_vigia.py:1032` · `config/settings/base.py:4` · `bin/deploy.sh:436` · `CLAUDE.md:88`

### Leitores

- `_recorte_da_barra` é chamado pela tela, pelo PDF e pelo Raio‑X. `folha/views_retrato.py:260` · `folha/views_retrato.py:391` · `folha/views_retrato.py:507`
- `get_commit_git` é chamado diretamente por `gerar_pdf_auditavel` e pelo espelho informacional. Pela porta auditável, a mudança alcança o retrato, o espelho individual, o lote de espelhos, o histórico de chamados e o PDF de gestão de HE. `relatorios/pdf_base.py:34` · `relatorios/pdf_base.py:135` · `relatorios/pdf_espelho.py:1070` · `folha/views_retrato.py:461` · `relatorios/pdf_espelho.py:1106` · `relatorios/services.py:338` · `relatorios/pdf_chamados.py:168` · `ponto/views.py:887`

### REDs e mutantes para o opus

| RED | Caso e mutantes que deve pegar |
|---|---|
| **1. Download** | Conferir o `attachment` e o nome exato pela rota, além do link no HTML cheio e no parcial filtrado. Mutantes `inline`, nome alterado ou `target`/`hx-*` no link. `folha/tests/test_retrato_pdf_pacote.py:187` |
| **3. Escala** | Usar descrição longa com `<` e `&`, conferir o texto integral no `Paragraph` e a quebra dentro da coluna. Mutantes de string crua, truncamento ou retirada de `escape`. `folha/tests/test_retrato_pdf_pacote.py:210` · `folha/views_retrato.py:441` |
| **4a. Ordem** | Intercalar os sete estados, incluir dias distintos na mesma faixa e um empate com nomes em ordem inversa à dos IDs. Conferir a ordem da tela e a igualdade do conjunto de IDs. Mutantes de ordem compartilhada, dias crescentes, ordenação por faixa ou desempate por nome. `folha/tests/test_retrato_pdf_pacote.py:228` · `folha/tests/test_retrato_pdf_pacote.py:245` |
| **4b. Contadores** | Comparar todos os `Rótulo: N` com a tela, usando filtro universal, janela e Admin ativo. Mutantes que contam a partir das linhas visíveis, usam o universo inteiro ou imprimem só a pílula ativa. `folha/tests/test_retrato_pdf_pacote.py:251` · `folha/views_retrato.py:411` |
| **5a. Fonte do SHA** | Ler um carimbo válido sem `COMMIT_GIT`, repetir com a variável preenchida e com o arquivo ausente. Mutantes que ignoram o caminho configurado, preferem o carimbo ao ambiente ou voltam a consultar Git. `folha/tests/test_retrato_pdf_pacote.py:292` · `folha/tests/test_retrato_pdf_pacote.py:317` · `relatorios/utils_audit.py:11` |
| **5b. Trilha** | Conferir o SHA inteiro no registro e nas duas chamadas do rodapé, e os oito primeiros caracteres no texto impresso. Mutantes que gravam o SHA só no registro, omitem uma passagem ou truncam o registro. `folha/tests/test_retrato_pdf_pacote.py:300` · `relatorios/pdf_base.py:72` · `relatorios/pdf_base.py:139` · `relatorios/pdf_base.py:164` · `relatorios/pdf_base.py:170` |

**Diff** com **0 `except` adicionados e 1 removido**. A sintaxe dos três arquivos e `git diff --check` foram conferidos. Resultado da suíte e renderização real do PDF [nao verificado]. `relatorios/utils_audit.py:11` · `folha/views_retrato.py:350` · `config/settings/base.py:5`

```bash
bash bin/suite.sh --dir /home/ronald/wt-codex-retrato-pdf-corta-por-que-012113 --only "folha.tests.test_retrato_pdf_pacote folha.tests.test_retrato_pdf relatorios core.tests.test_selo_teste_sem_relogio"
```

## 28/11 01:24 · sessao d2ce5ce5364d · INVENTARIO-PELA-REGRA · leitura · origem=Code

```python
import sys

sys.dont_write_bytecode = True

import json
import logging
from collections import defaultdict
from datetime import date
from functools import lru_cache
from html.parser import HTMLParser
from types import SimpleNamespace
from typing import NamedTuple


def parar(fonte, motivo):
    print(f"BECO|ABORTOU|{fonte}|{motivo}", flush=True)
    raise SystemExit(1) from None


def fonte_segura(caminho, linha):
    _, separador, relativo = caminho.rpartition("/app/")
    return f"{'app/' + relativo if separador else caminho.rsplit('/', 1)[-1]}:{linha}"


class FalhasSemDados:
    def __enter__(self):
        self.anterior = logging.getLogRecordFactory()

        def fabrica(*args, **kwargs):
            registro = self.anterior(*args, **kwargs)
            if registro.levelno >= logging.ERROR or (
                registro.exc_info and registro.exc_info[0]
            ):
                parar(fonte_segura(registro.pathname, registro.lineno), "erro_do_leitor")
            registro.msg, registro.args = "", ()
            registro.levelno = logging.NOTSET - 1
            return registro

        logging.setLogRecordFactory(fabrica)

    def __exit__(self, tipo, erro, quadro):
        logging.setLogRecordFactory(self.anterior)
        if tipo is None or (tipo is SystemExit and erro.code == 1):
            return False
        while quadro.tb_next is not None:
            quadro = quadro.tb_next
        parar(fonte_segura(quadro.tb_frame.f_code.co_filename, quadro.tb_lineno), tipo.__name__)


with FalhasSemDados():
    from django.conf import settings
    from django.db import connection
    from django.utils import timezone
    from django_tenants.utils import schema_context

    if getattr(settings, "HASNER_SOMBRA", False) is not True:
        parar("bin/sonda_leitura.sh:94", "HASNER_SOMBRA_ausente")
    db = connection.settings_dict
    if (db["NAME"], db["HOST"]) != ("sombra", "sombra_db"):
        parar("bin/sonda_leitura.sh:94", "conexao_fora_da_sombra")
    if connection.in_atomic_block or not connection.get_autocommit():
        parar("app/chamados/models.py:761", "sonda_exige_transacao_propria")

    from django.contrib.auth.models import User
    from django.db.models import Q
    from django.template import Context, engines

    from chamados.catalogo.motor import TERMINAIS, VIVOS, decidir
    from chamados.juizes import (
        _dia_de_turno_da_pergunta,
        competencia_do_chamado,
        dia_do_fato,
        pergunta_viva,
        perguntas_vivas_de,
    )
    from chamados.models import (
        ChamadoColaborador,
        DecisaoPergunta,
        DisputaSupervisao,
        PerguntaDisputa,
        motivo_implausivel_validacao,
    )
    from chamados.reconciliador import (
        _veredito_premissa,
        eventos_pendentes,
        retencao_da_retratacao,
    )
    from chamados.services.acoes_chamado import montar_contexto_detalhe
    from chamados.services.fio import (
        chamado_que_lista,
        contexto_parcial_fio,
        data_do_fato_de,
        disputas_do_chamado_em_lote,
    )
    from chamados.services.materializacao import (
        _data_em_disputa,
        estado_aplicacao_pergunta,
        espera_declarado_x_gravado,
    )
    from chamados.services.tarefa_do_dia import (
        carga_da_tarefa,
        exigencia_das_perguntas,
        porta_da_tarefa,
    )
    from chamados.services.validacao import (
        _e_aceite_de_folga,
        _resposta_de_hora,
        entrega_que_barra_a_validacao,
        fila_validar_classificada,
    )
    from chamados.services.verbos import particionar_verbos
    from chamados.utils import pode_agir_chamado
    from chamados.views import _is_gestor
    from chamados.views_cobrar import _pode_validar_lote
    from colaboradores.mixins import tem_acao
    from colaboradores.models import Empresa
    from folha.fora_da_folha import dias_que_seguram
    from folha.models import RetratoFolha
    from folha.retrato_folha import TIPO_SEM_CAUSA, TIPOS, dono_da_linha
    from ponto.janelas import janela_fechamento
    from ponto.services.declarar_dia import checar_declaracao, o_dia_ja_e_folga
    from ponto.supra_juiz import perguntas_que_seguram_csf


MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
GESTOR_S1_PK = 909
NV = "[nao verificado]"
FAMILIAS = ("a", "b", "c", "d", "e", "e_retido_pelo_fechamento",
            "e_retido_na_retratacao", "f")
OBRAS = {
    "a": ("O261-FIO-MORTO-PELO-RETRATO", 1132),
    "b": ("ORFAS-VOLTOU", 2102),
    "c": ("FDS-S1-C2-SEM-ACAO", 2197),
    "d": ("O261-E-CELULA-20-UM-DONO", 1012),
    "e": ("FDS-S1-C3-PREMISSA-MORTA", 2198),
    "f": ("FDS-S1-C1-SEM-SAIDA", 2196),
}
ACOES = {"validar_pergunta", "recusar_pergunta", "corrigir_resposta", "conferir_data"}


class Linha(NamedTuple):
    colab: int
    dia: date | None
    referencia: str
    motivo: str
    fonte: str
    obra: tuple


def ref(pk):
    return str(pk) if pk is not None else "-"


def dia_pergunta(p):
    # O fallback pela celula e a costura da S1. app/chamados/juizes.py:58,339
    dia = _dia_de_turno_da_pergunta(p)
    if dia is None and not p.conversa and p.celula_id:
        return p.celula.data, "celula"
    return dia, "dia_do_turno"


def consulta_perguntas(emp):
    return PerguntaDisputa.objects.filter(disputa__colaborador__empresa_id=emp).select_related(
        "disputa__colaborador__empresa", "disputa__chamado",
        "chamado_destino__colaborador", "chamado_destino__celula", "celula",
    ).prefetch_related("decisoes")


class Cartoes(HTMLParser):
    def __init__(self, html, chamado_local=None):
        super().__init__()
        self.perguntas = defaultdict(set)
        self.pares = defaultdict(set)
        self.formularios = defaultdict(list)
        self.chamado_local = chamado_local
        self.formulario = None
        self.feed(html)
        self.close()

    def handle_starttag(self, tag, atributos):
        a = dict(atributos)
        if "data-beco-pergunta" in a:
            self.perguntas[int(a["data-beco-pergunta"])].add(int(a["data-beco-chamado"]))
        if "data-beco-par" in a:
            self.pares[int(a["data-beco-par"])].add(int(a["data-beco-chamado"]))
        if tag == "form":
            self.formulario = {}
            if "data-beco-conferir" in a:
                self.formulario.update(
                    acao="conferir_data",
                    pergunta_id=a["data-beco-conferir"],
                    chamado_id=a["data-beco-chamado"],
                )
        elif tag == "input" and self.formulario is not None and a.get("type") == "hidden":
            nome = a.get("name")
            if nome in {"acao", "pergunta_id", "chamado_id", "confirmar_implausivel"}:
                self.formulario[nome] = a.get("value", "")

    def handle_endtag(self, tag):
        if tag != "form" or self.formulario is None:
            return
        f, self.formulario = self.formulario, None
        if f.get("acao") in ACOES:
            cid = int(f.get("chamado_id", self.chamado_local))
            self.formularios[int(f["pergunta_id"])].append(
                (cid, f["acao"], f.get("confirmar_implausivel") == "1")
            )


@lru_cache(maxsize=2)
def template_marcado(modal):
    # Marcadores em memoria nas ramificacoes reais. O HTML fica em memoria.
    # app/templates/chamados/partials/modal_fio.html:160,173,219,470
    # app/templates/chamados/partials/_pergunta_card.html:28,31
    engine = engines["django"].engine
    nome = "modal_fio.html" if modal else "_pergunta_card.html"
    caminho = f"chamados/partials/{nome}"
    fonte = engine.get_template(caminho).source
    pk = "perg.pk"
    card = "chamado.pk"

    def marca(tipo, pergunta=pk):
        return f'<span data-beco-{tipo}="{{{{ {pergunta} }}}}" data-beco-chamado="{{{{ {card} }}}}"></span>'

    resposta = (
        "<strong>Resposta:</strong> {{ perg.resposta_colab }}" if modal
        else "<strong>Resposta do colab:</strong> {{ perg.resposta_display }}"
    )
    pergunta = "{{ perg.pergunta }}</div>"
    trocas = [(pergunta, marca("pergunta") + pergunta), (resposta, marca("par") + resposta)]
    if modal:
        gaveta = "{{ it.dia }}</span>"
        conferir = """<form hx-post="{% url 'chamados:conferir_data_pergunta' perg.pk %}" """
        trocas.extend((
            (gaveta, marca("pergunta", "it.pergunta_id") + gaveta),
            (conferir, conferir + 'data-beco-conferir="{{ perg.pk }}" data-beco-chamado="{{ chamado.pk }}" '),
        ))
    for original, marcado in trocas:
        if fonte.count(original) != 1:
            parar(f"app/templates/{caminho}:1", "marcador_do_template_mudou")
        fonte = fonte.replace(original, marcado, 1)
    return engine.from_string(fonte)


@lru_cache(maxsize=None)
def chamado_de(pk):
    return ChamadoColaborador.objects.select_related(
        "colaborador__empresa", "celula"
    ).get(pk=pk)


def validar_em_leitura(p, confirma):
    # RECONSTRUIDO: testemunhas da S1, sem executar o ato sob trava.
    # app/chamados/services/validacao.py:218,755,779,782
    if not p.respondida or DecisaoPergunta.composta(p).validada_em is not None:
        return False
    if not confirma and motivo_implausivel_validacao(p):
        return False
    if _e_aceite_de_folga(p):
        dia, colab = p.data_evento, p.disputa.colaborador
        return bool(dia and (
            o_dia_ja_e_folga(colab, dia)
            or checar_declaracao(colaborador=colab, dias=[dia], tipo="folga")["aceitos"]
        ))
    if entrega_que_barra_a_validacao(p):
        return False
    if _resposta_de_hora(p):
        if espera_declarado_x_gravado(p):
            return False
        aplicacao = estado_aplicacao_pergunta(p.disputa, p)
        if aplicacao is None:
            parar("app/chamados/services/materializacao.py:880", "sem_testemunha_da_resposta_de_hora")
        return aplicacao["estado"] in ("aplicavel", "ja_satisfeita", "aplicada") [nao verificado]
    return not (p.tipo_resposta == "data_hora" and _data_em_disputa(p))


class TelaGestor:
    def __init__(self, conta, verbos, lote):
        self.conta = conta
        self.req = SimpleNamespace(user=conta, GET={})
        self.verbos = verbos
        self.lote = lote
        self.pode_inline = tem_acao(conta, "gerir_chamados")

    @lru_cache(maxsize=None)
    def modal(self, colab):
        # O GET marca lidas. O construtor do parcial le o mesmo fio.
        # app/chamados/views.py:870,881,890; app/chamados/services/fio.py:573,610
        ctx = contexto_parcial_fio(self.req, colab)
        ctx["cor_disputas_map"] = DisputaSupervisao.cor_map_por_colaborador(colab)
        for ch in ctx["chamados"]:
            if ch.disputa_obj is not None and ch.explicacao is None:
                parar("app/chamados/services/fio.py:437", "explicador_sem_resposta")
            for p in ch.perguntas_disputa:
                estado = p.estado_aplicacao or {}
                if estado.get("detalhe") == "estado de aplicacao indisponivel -- sem validar":
                    parar("app/chamados/services/fio.py:470", "erro_na_testemunha_da_aplicacao")
        return Cartoes(template_marcado(True).render(Context(ctx, use_l10n=False)))

    @lru_cache(maxsize=None)
    def local(self, pk):
        ch = chamado_de(pk)
        # Colisao GLPI -> detalhe local, igual a casca. app/chamados/views.py:219
        if ch.ticket_id_glpi is not None:
            alvo = ChamadoColaborador.objects.filter(
                pk=ch.ticket_id_glpi, ticket_id_glpi__isnull=True
            ).values_list("pk", flat=True).first()
            return self.local(alvo) if alvo is not None else Cartoes("")
        ctx = montar_contexto_detalhe(self.req, ch, True, ())
        html = "".join(
            template_marcado(False).render(Context({**ctx, "perg": p}, use_l10n=False))
            for p in ctx["perguntas_disputa"]
        )
        return Cartoes(html, chamado_local=pk)

    @lru_cache(maxsize=None)
    def eleita(self, pk):
        return disputas_do_chamado_em_lote([pk]).get(pk)

    @lru_cache(maxsize=None)
    def inline(self, pk):
        ch = chamado_de(pk)
        if not (self.pode_inline and self.verbos.get(pk) == "validar"
                and pode_agir_chamado(self.conta, ch)):
            return set()
        d = self.eleita(pk)
        if d is None or d.colaborador_id != ch.colaborador_id:
            return set()
        return {p.pk for p in perguntas_vivas_de(d) if p.respondida}

    def formularios(self, p, cartoes, canal):
        ancora = chamado_que_lista(p)
        for cid, acao, confirma in cartoes.formularios.get(p.pk, ()):
            if acao == "conferir_data":
                # O POST e por pergunta, com a permissao de tela. app/chamados/views.py:1288
                if self.pode_inline and data_do_fato_de(p)[1] and p.data_conferida_em is None:
                    yield acao
                continue
            if cid != ancora or not pode_agir_chamado(self.conta, chamado_de(cid)):
                continue
            if canal == "local" and not p.disputa.aberta:
                continue
            if acao == "validar_pergunta":
                if canal == "fio" and chamado_de(cid).colaborador_id != p.disputa.colaborador_id:
                    continue
                if validar_em_leitura(p, confirma):
                    yield acao
                continue
            # app/chamados/services/fio.py:694,709,776
            if not p.disputa.aberta or (canal == "fio" and self.eleita(cid) is None):
                continue
            if DecisaoPergunta.composta(p).validada_em is not None:
                continue
            if acao == "recusar_pergunta" and p.respondida and not p.fora_do_fato():
                yield acao
            if acao == "corrigir_resposta" and p.tipo_resposta == "hora":
                yield acao

    def consultar(self, p, ch=None, *, medir_acoes=False):
        ids = {chamado_que_lista(p), p.disputa.chamado_id, ch.pk if ch else None} - {None}
        colabs = {p.disputa.colaborador} | {chamado_de(pk).colaborador for pk in ids}
        canais = [("fio", self.modal(colab)) for colab in sorted(colabs, key=lambda c: c.pk)]
        canais += [("local", self.local(pk)) for pk in sorted(ids)]
        no_lote = p.pk in self.lote  # app/chamados/views_cobrar.py:371
        visivel = no_lote or any(p.pk in c.perguntas for _, c in canais)
        par = no_lote or any(p.pk in c.pares for _, c in canais)
        acoes = set()
        if medir_acoes and par:
            acoes = {acao for canal, c in canais for acao in self.formularios(p, c, canal)}
            if (no_lote or any(p.pk in self.inline(pk) for pk in ids)) and validar_em_leitura(p, False):
                acoes.add("validar_pergunta")
        return visivel, par, acoes


def medir_a(empresa, janela, agora, hoje, vivos, perguntas_dia, tela):
    universo = dias_que_seguram(empresa, MES, ANO, hoje=hoje)
    ancoras = defaultdict(set)
    for ch in vivos.values():
        dia = dia_do_fato(ch, fortes=True)
        if dia is not None:
            ancoras[ch.colaborador_id, dia].add(ch.pk)
    for chave, perguntas in perguntas_dia.items():
        for p, _ in perguntas:
            pk = chamado_que_lista(p)
            if pk in vivos and vivos[pk].colaborador_id == chave[0]:
                ancoras[chave].add(pk)

    candidatos = {}
    for cid, (_, dias) in universo["dias"].items():
        for r in dias:
            chave = (cid, r["data"])
            if chave not in ancoras:
                candidatos[chave] = r
    if not candidatos:
        return []

    colabs = universo["colabs"]
    carga = carga_da_tarefa(
        [colabs[cid] for cid in sorted({cid for cid, _ in candidatos})],
        *janela, agora,
    )
    linhas = []
    for (cid, dia), r in sorted(candidatos.items()):
        tarefa = carga.tarefa(colabs[cid], dia)
        porta = porta_da_tarefa(tarefa)
        fila = (porta or {}).get("fila") or ()
        if any(pk in vivos and vivos[pk].colaborador_id == cid for pk in fila):
            continue
        ps = [p for p, viva in perguntas_dia.get((cid, dia), ()) if viva]
        if any(tela.consultar(p)[0] for p in ps):
            continue
        linhas.append(Linha(
            cid, dia, "-",
            f"{r['motivo']} celula={ref(r['celula_id'])} "
            f"rota_tarefa={(porta or {}).get('rota') or '-'} "
            f"fio_tarefa={(porta or {}).get('fio') or '-'} nao_sei={int(tarefa['nao_sei'])} "
            "RECONSTRUIDO: juncao_colab_dia_ancora_e_superficies_admin",
            "app/folha/export.py:1240", OBRAS["a"],
        ))
    return linhas


def medir_c(emp, janela, agora, hoje, vivos, tela, sem_dia):
    pares = defaultdict(set)
    for ch in vivos.values():
        motor = decidir(ch.status_local, "RECONCILIAR")
        if motor.permitido and motor.novo_estado in TERMINAIS:
            plano = ch._plano_pergunta_viva(motor.novo_estado)
            if not ch._gate_pergunta_viva(motor.novo_estado, registrar=False, plano=plano):
                for p in plano[0]:
                    pares[ch.pk, p.pk].add("pergunta_viva_na_competencia_aberta")
        if not ch._gate_conversa_do_pai(registrar=False):
            for pk in perguntas_que_seguram_csf(ch):
                pares[ch.pk, pk].add("conversa_exige_decisao_humana")

    # A pergunta e relida no estado corrente, pois o plano julgou o pos-estado.
    ps = consulta_perguntas(emp).in_bulk({pk for _, pk in pares})
    encontrados = []
    for (ch_pk, p_pk), guardas in sorted(pares.items()):
        p = ps.get(p_pk)
        if p is None or not p.respondida or not pergunta_viva(p, hoje=hoje, agora=agora):
            continue
        ch = vivos[ch_pk]
        dia, fonte_dia = dia_pergunta(p)
        if dia is None:
            comp = competencia_do_chamado(ch)
            dia = comp.dia if comp is not None else None
            fonte_dia = "competencia_do_chamado"
        if dia is not None and not janela[0] <= dia <= janela[1]:
            continue
        _, par, acoes = tela.consultar(p, ch, medir_acoes=True)
        if not par or acoes:
            continue
        if dia is None:
            sem_dia["c"] += 1
            continue
        encontrados.append((ch, p, dia, fonte_dia, guardas))

    razoes = exigencia_das_perguntas({p.pk for _, p, _, _, _ in encontrados}, agora)
    linhas = []
    for ch, p, dia, fonte_dia, guardas in encontrados:
        razao, _ = razoes[p.pk]
        nascente = (
            "app/chamados/models.py:429" if "pergunta_viva_na_competencia_aberta" in guardas
            else "app/ponto/supra_juiz.py:561"
        )
        linhas.append(Linha(
            p.disputa.colaborador_id, dia, str(ch.pk),
            f"pergunta={p.pk} disputa={p.disputa_id} ancora={ref(chamado_que_lista(p))} "
            f"guardas={','.join(sorted(guardas))} tarefa={razao.partition(':')[0]} "
            f"RECONSTRUIDO: alcance_do_par_gestor_S1_sem_POST dia_por={fonte_dia}",
            nascente, OBRAS["c"],
        ))
    return linhas


def medir_e(janela, vivos, com_evento, grupos, sem_dia):
    # Ordem de producao. app/chamados/management/commands/reconciliar_chamados.py:62,73,79
    for ch in vivos.values():
        if ch.colaborador_id in com_evento:
            continue
        comp = competencia_do_chamado(ch)
        dia = comp.dia if comp is not None else None
        if dia is not None and not janela[0] <= dia <= janela[1]:
            continue
        veredito = _veredito_premissa(ch)
        if veredito is None or veredito.viva:
            continue
        ret = retencao_da_retratacao(ch)
        if ret is not None:
            fam, det = "e_retido_na_retratacao", f"retencao={ret[0]}"
        elif not ch.reconciliaria():
            fam, det = "e_retido_pelo_fechamento", "reconciliaria=False"
        else:
            fam, det = "e", "reconciliaria=True"
        if dia is None:
            sem_dia[fam] += 1
            continue
        grupos[fam].append(Linha(
            ch.colaborador_id, dia, str(ch.pk), f"{ver

[truncado em 20000 de 32189 caracteres]

## 28/11 01:19 · sessao 49d483221237 · CODIGO-DOMINIO-VALIDO · leitura · origem=Code

# Dossiê — CODIGO-DOMINIO-VALIDO

**O ponto de mudança é `classificar_export`, na resposta de quem entra no TXT.** Hoje o juiz só verifica se o código do colaborador está vazio depois de `strip()`. Um valor preenchido como `VIGIA` pode passar por essa etapa e, se houver rubrica a emitir, fazer a validação recusar o TXT inteiro. O Retrato já grava o código na linha e já atribui `Admin` aos bloqueios de cadastro. [app/folha/export.py:849](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/export.py:849) [app/folha/export.py:881](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/export.py:881) [app/folha/views.py:282](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/views.py:282) [app/folha/retrato_folha.py:1239](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/retrato_folha.py:1239) [app/folha/retrato_folha.py:374](/home/ronald/codex_ws/93e3058609b2dc9cd2ef551217e26ce8339babf9/app/folha/retrato_folha.py:374)

Leitura da cópia e prova em memória do reg10, sem alteração de arquivos. Hash do conteúdo `[nao verificado]`, pois a cópia veio sem metadados Git. O anexo entregue contém a tabela `JUIZES.md`, cujo verbete de fechamento consta na cópia. Ocorrências de `VIGIA` em produção, quantidade de pessoas afetadas, empresa, competência e frequência de uso real `[nao verificado]`. `app/docs/JUIZES.md:119`, `CLAUDE.md:35`.

## A. Onde o juiz decide e onde o layout acusa

- **Precedência atual:** `rescisao_modulo_proprio` → `sem_codigo_dominio` → `ferias_com_batida` → `cadastro_zero` → `furo_espelho` → `nao_homologado` → `entra`. A rescisão vence quando a data de demissão existe e é até o fim da janela. O ramo seguinte usa `if not item['codigo_dominio']`, com o item já preenchido por `(colab.codigo_dominio or '').strip()`. Cada retenção encerra a classificação daquela pessoa. `app/folha/export.py:54–60`, `app/folha/export.py:774–782`, `app/folha/export.py:846–893`, `app/folha/export.py:968–990`.
- **Matrícula do reg10:** `montar_linhas_export` pega os IDs com `status == 'entra'` e lê `(fech.colaborador.codigo_dominio or '').strip().zfill(10)`. O reg10 põe a matrícula em `[2:12]`. `validar_layout_dominio` exige 43 caracteres na linha e aplica `.isdigit()` a esse campo. `VIGIA` vira `00000VIGIA` e dá `reg10 campo matric nao-numerico`. A view devolve `Falha de layout no export` antes de entregar ou registrar o TXT. O command de regeração também recusa erro de layout. `app/folha/export.py:34–45`, `app/folha/export.py:272–279`, `app/folha/export.py:723–753`, `app/folha/views.py:275–304`, `app/folha/management/commands/regerar_txt_dominio.py:94–96`.
- **Predicados existentes:** a checagem numérica da matrícula está no validador de layout e também em `partes_da_linha`, que lê um reg10 já montado. `codigo_no_sistema_dominio` procura a integração ativa **da empresa**, acusa ausência de configuração e preenche com zeros o código da integração. `nao_homologado` consulta códigos **de rubrica**. `app/folha/export.py:23–33`, `app/folha/export.py:245–254`, `app/folha/export.py:277–279`, `app/folha/export.py:983–987`, `app/folha/export.py:1003`.
- **Cadastro hoje:** admissão e edição tratam o campo como texto, aplicam `strip()` e atribuem o valor se preenchido. A validação de obrigatórios só trata nome e CPF, e a de pendências verifica presença. O campo é `CharField(max_length=20, blank=True, default='')`, sem validador numérico, e o `save()` do colaborador não valida esse campo. O `clean()` em `models.py:515` pertence a `HistoricoSalario`. Na admissão o código fica fora dos dados enviados à guarda de tamanho. Na edição o POST usa `codigo_folha` e a guarda compara nomes exatos do modelo. `app/colaboradores/views.py:1131–1138`, `app/colaboradores/views.py:1169–1185`, `app/colaboradores/views.py:1231–1237`, `app/colaboradores/views.py:1297–1306`, `app/colaboradores/views.py:1353–1365`, `app/colaboradores/services/cadastro.py:22–30`, `app/colaboradores/services/cadastro.py:49–87`, `app/core/validacao.py:46–59`, `app/colaboradores/models.py:311–314`, `app/colaboradores/models.py:415–439`, `app/colaboradores/models.py:478`, `app/colaboradores/models.py:515`.

## B. Censo do campo `Colaborador.codigo_dominio`

| Sítio | Leitura ou escrita encontrada |
|---|---|
| **Admissão** | A rota `novo/` recebe `codigo_folha` no input de texto. A view cria o colaborador e depois atribui o código preenchido por `salvar_campos(..., 'cadastro', ...)`. `app/colaboradores/urls.py:49`, `app/templates/colaboradores/admitir.html:58`, `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1204–1237`. |
| **Edição** | A rota `editar/` exibe o código atual no input `codigo_folha`. A view atribui o valor preenchido e chama `salvar_inteiro(..., 'cadastro')`. POST vazio mantém o código anterior. Os aliases usados para reexibir a admissão e a edição preenchem rascunhos em memória. `app/colaboradores/urls.py:50`, `app/templates/colaboradores/editar.html:62`, `app/colaboradores/views.py:1113–1118`, `app/colaboradores/views.py:1267–1285`, `app/colaboradores/views.py:1353–1365`. |
| **Persistência genérica** | `cadastro` inclui o campo, e `gravar`, `salvar_campos`, `atualizar_em_lote` e `criar_colaborador` podem transportá-lo via argumentos. `salvar_inteiro` e `salvar_reativacao` fazem `save()` completo e podem regravar o valor que já estava carregado. A migration declara o campo e o modelo fornece o vazio inicial. `app/colaboradores/services/colaborador.py:46`, `app/colaboradores/services/colaborador.py:126–173`, `app/colaboradores/services/colaborador.py:184–206`, `app/colaboradores/migrations/0026_colaborador_codigo_dominio.py:15`, `app/colaboradores/models.py:311–314`. |
| **Admin, importadores, commands, API e signals** | Admin Django lê o campo por `SoLeitura`. O importador Nexti encontrado grava telefone e e-mail. O signal de `Colaborador` localizado trata isenção de ponto. A API de cadastro incompleto consulta o campo e o upload de holerite usa o código para casar páginas. Outros escritores explícitos do campo nessas categorias não apareceram na busca na cópia. Escritas por fora da cópia `[nao verificado]`. `app/colaboradores/admin.py:47–48`, `app/core/admin.py:53–75`, `app/colaboradores/management/commands/importar_telefones_nexti.py:113`, `app/chamados/signals.py:270–281`, `app/api/views_mensageria.py:1275–1288`, `app/holerite/motor.py:216–230`. |
| **Export, prévias e auditoria do TXT** | O juiz lê o código para classificar, e o montador lê para preencher a matrícula. A prévia nominal, a proposta em tela/CSV e o PDF de retidos leem o campo do item. O PDF de validação e o vigia indexam pela matrícula preenchida com zeros. O PDF do TXT gravado e `haiku_export` também consultam o cadastro para cruzar matrículas. `app/folha/export.py:737`, `app/folha/export.py:849–884`, `app/folha/views.py:427–429`, `app/folha/previa.py:97–105`, `app/folha/views.py:762–768`, `app/folha/views.py:813–837`, `app/folha/retidos_pdf.py:88–95`, `app/folha/validacao_pdf.py:65–72`, `app/folha/services/vigia_txt.py:98–116`, `app/folha/pdf_export.py:147–148`, `app/folha/haiku_export.py:111–116`. |
| **Retrato, fechamento e relatórios** | O Retrato grava `codigo` do item. Fechamento, Pendências de Cadastro e Quem Cobrar leem o veredito do juiz. O contador de cadastro lê o valor pelo alias `codigo_folha`, e a API de cadastro incompleto tem filtro próprio de `NULL`/`''`, sem `strip()`. `app/folha/retrato_folha.py:1212–1243`, `app/ponto/services/fechamento_lista.py:25–37`, `app/ponto/services/fechamento_lista.py:65–73`, `app/relatorios/views_pend.py:47–69`, `app/relatorios/views.py:1723–1737`, `app/colaboradores/services/cadastro.py:74–87`, `app/colaboradores/services/cadastro.py:99–120`, `app/api/views_mensageria.py:1275–1288`. |
| **Holerite** | O parser Domínio extrai um identificador numérico do PDF. A configuração padrão Domínio casa por nome completo, com CPF quando fornecido pela página. O modo configurável oferece `codigo_dominio` e pode filtrar o colaborador por esse campo e pela empresa. `app/holerite/motor.py:96–118`, `app/holerite/motor.py:161–163`, `app/holerite/motor.py:192–230`, `app/holerite/motor.py:298–313`, `app/holerite/views.py:1741`, `app/templates/holerite/upload_lote_v2.html:46`. |

O corte anterior `EXPORT-UM-JUIZ-DO-CODIGO` registra um censo de **8 escritores, 20 leitores e 4 predicados vivos**, sem enumerá-los naquela linha. Equivalência entre esse levantamento e o censo da cópia `[nao verificado]`. `app/docs/CORTES.md:587`.

## C. Quem lê o vocabulário do motivo

| Sítio | Efeito de emitir `codigo_dominio_invalido` |
|---|---|
| **`MOTIVO_RH` e `rotulo_do_motivo`** | A entrada no mapa dá o rótulo humano à prévia do export, ao PDF de retidos e, via `rotulo_do_motivo`, à lista, ao PDF, ao Raio-X e ao drawer do Retrato. Chave desconhecida aparece crua. `app/folha/views.py:12–24`, `app/folha/views.py:373`, `app/folha/views.py:415–429`, `app/folha/views.py:884–885`, `app/folha/retidos_pdf.py:83–95`, `app/folha/retrato_folha.py:76–86`, `app/folha/retrato_folha.py:1119`, `app/folha/views_retrato.py:291`, `app/folha/views_retrato.py:424`, `app/folha/views_retrato.py:503`. |
| **Tipo e faixa** | A nova chave precisa de destino `cadastro` em `TIPO_POR_MOTIVO`, junto de `sem_codigo_dominio`. `_tipo_cru` usa esse mapa para retenções de cadastro e `TIPO_POR_CAUSA` para `furo_espelho`. `MOTIVOS_DE_CADASTRO` deriva do primeiro mapa e `_saldo_cru` já devolve `cadastro` mesmo com `dias=None`. `app/folha/retrato_folha.py:210–230`, `app/folha/retrato_folha.py:379–390`, `app/folha/retrato_folha.py:422–433`. |
| **Dono e fio** | `_fio_cru` já devolve `admin` para bloqueio diferente de `furo_espelho`, excetuado o módulo próprio. `montar_linhas` grava esse fio em `dono`, `DONO_DO_FIO` o leva a `admin` e o rótulo é `Admin`. `models.py:321` documenta a regra e dá `sem_codigo_dominio` como exemplo. `app/folha/retrato_folha.py:33`, `app/folha/retrato_folha.py:194–201`, `app/folha/retrato_folha.py:372–376`, `app/folha/retrato_folha.py:486–493`, `app/folha/retrato_folha.py:1248–1251`, `app/folha/models.py:315–323`. |
| **Listas e rótulos locais** | `_MOTIVO_NA_TELA` do fechamento cai na pílula `espelho` para chaves novas. `_PORTAS_CADASTRO` filtra apenas as chaves declaradas, então Pendências de Cadastro omite o novo motivo. Quem Cobrar e `EM_PORTUGUES` da mensageria caem na chave crua. Esses sítios precisam conhecer a chave para mostrá-la com o mesmo sentido. A API que conta apenas código faltante tem outro universo e outro predicado. `app/ponto/services/fechamento_lista.py:21–37`, `app/templates/ponto/_fech_linha.html:11`, `app/relatorios/views_pend.py:47–69`, `app/relatorios/views.py:1727–1737`, `app/relatorios/views.py:1762`, `mensageria/nucleo/ferramentas.py:1196–1203`, `mensageria/nucleo/ferramentas.py:1246–1253`, `app/api/views_mensageria.py:1275–1288`. |
| **Agrupadores e vigias** | O Raio-X do export põe chaves fora de `_COM_PILULA` em **Outras pendências**. `fora_da_folha`, a API de prontidão, as métricas e o simulador agrupam pela chave recebida. O vigia do TXT transporta o motivo. A enumeração em `vigia_pausa.py:29` é documentação da precedência. O contador daquele vigia usa `motivos_retencao_celula`. `app/folha/views.py:615–627`, `app/folha/views.py:661–667`, `app/folha/fora_da_folha.py:147–173`, `app/api/views_mensageria.py:283–302`, `app/inteligencia/calculadores.py:210–220`, `app/folha/management/commands/simular_folha.py:133–146`, `app/folha/services/vigia_txt.py:114–116`, `app/folha/services/vigia_pausa.py:26–32`, `app/folha/services/vigia_pausa.py:123–132`. |

## D. Como mostrar o valor gravado

`montar_linhas` já grava `codigo` ao lado de `motivo`. O valor vem do item do juiz depois de `strip()`, antes de `zfill(10)`. `detalhe` também existe no item, nasce vazio e é preenchido pelos ramos, inclusive pelo de espelho a partir de `:977`. A linha JSON do Retrato usa as chaves estruturadas e deixa `detalhe` no item do export. `app/folha/export.py:846–858`, `app/folha/export.py:881–884`, `app/folha/export.py:977–987`, `app/folha/retrato_folha.py:1237–1256`, `app/folha/models.py:350–354`.

**Já há forma para tela e PDF.** A projeção compartilhada do *Por quê* pode entregar o rótulo literal de `MOTIVO_RH` e, para a nova chave, o rótulo fixo `codigo Dominio =` com o dado `linha['codigo']`. A lista, o PDF e o Raio-X recebem os mesmos componentes. No drawer, `r.valor` continua literal do mapa, com o código exibido como dado ao lado. O próprio drawer já exibe datas gravadas como dados e o PDF de retidos já tem coluna estruturada de código. Isso resolve `Código Domínio inválido · codigo Dominio = VIGIA` sem prosa livre nem consulta ao cadastro atual. Interpolar o código dentro de `r.valor` quebraria o selo literal assim que a nova linha entrasse na fixture. `app/folha/views_retrato.py:278–293`, `app/folha/views_retrato.py:414–425`, `app/folha/views_retrato.py:500–506`, `app/folha/retrato_folha.py:1079–1093`, `app/templates/folha/_retrato_o_que_fazer.html:33–50`, `app/folha/retidos_pdf.py:88–95`, `app/folha/tests/test_retrato_folha_drawer.py:213–231`, `app/docs/CORTES.md:405`.

## E. Leis e juiz registrado

- **L-001, L-002, L-011 e L-095.** A cura da classificação fica na fonte, as testemunhas leem a resposta, autoridade nova exigiria corte e o dinheiro lavrado segue sendo lido. O registro vivo põe `_K2` **quem entra no TXT** e `_K1` **o que bloqueia aprovação/tranca** na mesma função. O motivo pertence a `_K2`. `_K1` sai em `travas_aprovacao`, montada antes dos retornos antecipados. `app/docs/LEIS.md:59–60`, `app/docs/LEIS.md:69`, `app/docs/LEIS.md:113`, `app/core/juizes.py:180–181`, `app/core/juizes.py:326–337`, `app/folha/export.py:779–782`, `app/folha/export.py:839–858`, `app/docs/DOSSIES.md:85–89`, `app/docs/DOSSIES.md:118–122`.
- **L-099, O256 e Saldo Cadastro.** L-099 separa a inconsistência estrutural do valor de cadastro, cuja correção fica com o Admin. A O256 manda o drawer ler os dados lavrados. O complemento põe cadastro sem forma de dia na faixa **Cadastro** e sela a igualdade entre a soma do Saldo e Bloqueados. `app/docs/LEIS.md:122`, `app/docs/CORTES.md:392`, `app/docs/CORTES.md:405`, `app/folha/tests/test_retrato_folha_quadros.py:1133–1160`.
- **L-026/L-090, L-092/L-113 e L-121.** A janela vem do corte da empresa. Para exportadas, a L-113 e sua emenda qualificam a regra histórica da L-092, preservam o TXT anterior e tratam DIFF e aviso quando aplicáveis. L-121 liga cura de juiz à reconstrução das projeções pela porta. `app/docs/LEIS.md:77`, `app/docs/LEIS.md:123–125`, `app/docs/LEIS.md:142`, `app/docs/LEIS.md:155–156`, `app/folha/export.py:47–51`.
- **Diferença entre corte e cópia.** `RESPOSTAS-COERENCIA-1.6` manda levar `STATUS_NO_TXT` para dentro do juiz. Nesta cópia o juiz consulta todos os fechamentos da competência e o montador aplica `STATUS_NO_TXT = ('aberto', 'aprovado')`. O dossiê usa esse comportamento observado como linha de base. `app/docs/CORTES.md:523`, `app/folha/export.py:696`, `app/folha/export.py:723–731`, `app/folha/export.py:791–797`.

## F. Selos atuais e REDs descritos

**Selos atuais.** `MotivoExaustivoTest` colhe por AST as seis chaves literais atuais e fica vermelho se a nova chave vier sem destino no eixo Tipo. Os testes de dono e faixa têm listas literais de exemplos, e a asserção que deriva Cadastro do mapa já cobre entradas novas. O selo Saldo = Bloqueados só observa a nova linha se ela entrar na fixture. O selo do drawer cobra o valor literal em mapa e cinco linhas do bloco. `app/folha/tests/test_retrato_folha_quadros.py:255–280`, `app/folha/tests/test_retrato_folha_quadros.py:131–136`, `app/folha/tests/test_retrato_folha_quadros.py:314–335`, `app/folha/tests/test_retrato_folha_quadros.py:375–398`, `app/folha/tests/test_retrato_folha_quadros.py:1133–1160`, `app/folha/tests/test_retrato_folha_drawer.py:208–231`.

**Precedência já selada em partes.** Há casos de rescisão, código vazio, vazio simultâneo a turno aberto com preservação de `travas_aprovacao`, férias com batida, cadastro zero antes de furo e rubrica não homologada. Há também selo de motivo desconhecido que continua visível e selo de PDF que lê o mesmo recorte sem chamar o juiz. Esses testes protegem regressões, mas nenhum desses casos exige o novo motivo. `app/folha/tests/test_rescisao_modulo_proprio.py:24–38`, `app/folha/tests/test_classificar_export.py:34–66`, `app/folha/tests/test_classificar_export.py:113–130`, `app/folha/tests/test_contrato_export_um_juiz.py:132–150`, `app/folha/tests/test_ferias_com_batida.py:74–94`, `app/folha/tests/test_cadastro_zero_com_batida.py:161–168`, `app/folha/tests/test_integracao.py:143–152`, `app/folha/tests/test_retrato_pdf.py:138–150`, `app/folha/tests/test_retrato_pdf.py:330–345`.

| Entrada de teste | Saída esperada | [nao verificado]
|---|---|
| **`VIGIA`, fechamento apto nos demais critérios e rubrica que geraria reg10** | Um item `fora/codigo_dominio_invalido`, com `codigo_dominio='VIGIA'`. O ID sai do conjunto de aptos e nenhuma linha leva `00000VIGIA`. A rubrica positiva faz o caso detectar a falha atual de layout. `app/folha/export.py:723–753`, `app/folha/export.py:849–884`, `app/folha/export.py:277–279`. |
| **Controle numérico `00123`, mesma janela e demais insumos** | Continua `entra`, matrícula `0000000123`, reg10 idêntico ao controle anterior. No lote misto com esse controle o TXT tem linha, `validar_layout_dominio(linhas) == []` e a rota entrega o arquivo. `app/folha/export.py:34–45`, `app/folha/export.py:725–755`, `app/folha/views.py:275–292`, `app/folha/tests/test_integracao.py:211`. |
| **`''` e `'   '`** | Ambos continuam `fora/sem_codigo_dominio`, sem linha no TXT. `app/folha/export.py:849`, `app/folha/export.py:881–884`, `app/folha/tests/test_classificar_export.py:40–45`. |
| **Retrato lavrado com `VIGIA`, depois cadastro alterado para `00123`** | Na lista, no PDF do mesmo recorte e no drawer, verificar o rótulo do motivo, o valor `VIGIA` da foto e `Depende de = Admin`. A linha está em **Cadastro**, com `dias`/`datas` vazios. No drawer, verificar também que o valor de mapa segue literal. `app/folha/retrato_folha.py:1212–1251`, `app/folha/views_retrato.py:194–201`, `app/folha/views_retrato.py:278–293`, `app/folha/views_retrato.py:414–425`, `app/folha/tests/test_retrato_folha_drawer.py:213–231`. |
| **POST real de admissão e edição com os outros campos válidos** | `VIGIA` retorna mensagem visível, por exemplo `Código Domínio deve conter apenas números.` Na admissão, não nasce colaborador nem usuário. Na edição, o registro anterior fica intacto e o valor digitado reaparece. `00123` grava exatamente `00123` nas duas portas. Casos de regressão para manter a admissão sem código e a edição com campo vazio conforme o comportamento atual. `app/colaboradores/views.py:1131`, `app/colaboradores/views.py:1165–1168`, `app/colaboradores/views.py:1190–1237`, `app/colaboradores/views.py:1267–1285`, `app/colaboradores/views.py:1353–1365`, `app/core/respostas.py:58–65`, `app/core/respostas.py:74–85`. |

**Bordas verificadas em memória com o código da cópia.** `' 00123 '` e `'00123'` saem como `0000000123`. `'00000'` sai como `0000000000`. Espaço interno e sinal são rejeitados pelo `.isdigit()` do layout. Onze dígitos produzem reg10 de 44 caracteres e falham no comprimento. O `.isdigit()` local aceita dígitos Unicode, inclusive `１２３` e `²`. Aceitação desses caracteres pelo importador externo `[nao verificado]`. Os REDs de integração e a suíte ficaram `[nao verificado]`. `app/folha/export.py:34–45`, `app/folha/export.py:272–279`, `app/folha/export.py:737`, `app/folha/export.py:849–884`.

## G. Contrato de entrada e perguntas de lei

- **Fonte e unidade.** Para o TXT, `FechamentoMensal` e `Colaborador.codigo_dominio`, uma decisão por colaborador × empresa × competência. O caminho normal consulta o fechamento gravado e o caminho de export pode injetar os fechamentos que já tem em mãos. Para o cadastro, uma submissão de `codigo_folha` na admissão ou edição. 

[truncado em 20000 de 22700 caracteres]

## 28/11 01:10 · sessao d4649f8dfc9b · RETRATO-PDF-CORTA-POR-QUE · leitura · origem=Code

## RETRATO-PDF-CORTA-POR-QUE

A linha lavrada tem **um `tipo` por pessoa e uma lista plana de `datas`**. O vínculo entre motivo e data existe na fonte anterior à lavra, mas fica de fora do JSON. O anexo `JUIZES.md` localiza a autoridade de entrada no TXT. Linhas reais, contadores de produção, PDF renderizado em produção e ambiente da casca estão `[nao verificado]`. `app/folha/export.py:1202` · `app/folha/fora_da_folha.py:234` · `app/folha/retrato_folha.py:1237` · `app/docs/JUIZES.md:119`

### A. Mapa das colunas e do vocabulário

Tela e PDF passam por `_recorte_da_barra`. Ele lê `RetratoFolha.linhas` via `linhas_do_retrato`, junta o cadastro, aplica o filtro universal, passa por `no_periodo` e entrega `ordenar(recortar(...))`. A tela projeta `_tabela` e usa `_retrato_lista.html`, tanto na página cheia quanto no parcial. O PDF monta a tabela em `_story`, dentro de `retrato_pdf`. `app/folha/retrato_folha.py:98` · `app/folha/views_retrato.py:194` · `app/folha/views_retrato.py:225` · `app/folha/views_retrato.py:278` · `app/folha/views_retrato.py:345` · `app/templates/folha/retrato_folha.html:53` · `app/templates/folha/_retrato_parcial.html:7` · `app/folha/views_retrato.py:406`

| Coluna | Origem e saída |
|---|---|
| LED na tela / **Depende de** no PDF | `desenho_do_led` lê `led_da_linha`, que usa `dono_da_linha` para bloqueados e o próprio balde para `Entram` e `Rescisão`. A tela desenha o ponto com cor, rótulo e link de filtro. O PDF imprime o rótulo na cor do LED. `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:531` · `app/folha/retrato_folha.py:559` · `app/templates/folha/_retrato_lista.html:47` · `app/folha/views_retrato.py:418` |
| **Colaborador** | `_cadastro` junta `Colaborador.nome` pelo ID `colab`. As duas saídas usam `(cadastro ausente)` se o nome estiver vazio. Na tela, o nome abre o drawer com mês e ano. `app/folha/views_retrato.py:99` · `app/folha/views_retrato.py:118` · `app/folha/views_retrato.py:287` · `app/folha/views_retrato.py:421` · `app/templates/folha/_retrato_lista.html:60` |
| **Posto** | `_cadastro` prefere o posto do vínculo ativo mais recente e, na falta dele, usa o posto do colaborador. As duas saídas exibem `posto.nome`. `app/folha/views_retrato.py:107` · `app/folha/views_retrato.py:115` · `app/templates/folha/_retrato_lista.html:63` · `app/folha/views_retrato.py:422` |
| **Escala** | `_cadastro` pega `tipo_escala` daquele vínculo ativo. As duas saídas exibem `descricao`. `app/folha/views_retrato.py:107` · `app/folha/views_retrato.py:122` · `app/templates/folha/_retrato_lista.html:64` · `app/folha/views_retrato.py:423` |
| **Por quê** | Ambas aplicam `rotulo_do_motivo(linha['motivo'])`. O HTML imprime `l.motivo`. O rótulo visível **“Espelho com pendencia”** vem de `MOTIVO_RH['furo_espelho']`. O export também monta um `detalhe` com esse prefixo e os códigos, mas essa chave fica fora da linha lavrada. `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/templates/folha/_retrato_lista.html:65` · `app/folha/retrato_folha.py:76` · `app/folha/views.py:18` · `app/folha/export.py:977` · `app/folha/retrato_folha.py:1237` |
| **Dias que seguram** | `dias` vem da lavra. Com janela, `no_periodo` conta as datas no intervalo inclusivo e recalcula a faixa `saldo`. `None` sai em branco nas duas saídas. `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1334` · `app/templates/folha/_retrato_lista.html:66` · `app/folha/views_retrato.py:425` |

- **Pílulas e Tipo.** `ROTULO_DO_DONO` alimenta o quadro `Depende de` e os LEDs. `ROTULO_DO_TIPO` contém `Escala`, `Folga`, `Sem escala`, `Não bate`, `Falta`, `Cadastro`, `Férias` e `Sem causa`. O eixo `tipo` continua no recorte, no Raio-X e no drawer. A barra atual desenha `dono` e, com Bloqueados ativo, `saldo`. O **Por quê** consulta o mapa `MOTIVO_RH`. As expressões do exemplo “Batida faltando” e “pergunta aberta” não são entradas desses mapas. `app/folha/retrato_folha.py:200` · `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:587` · `app/folha/retrato_folha.py:906` · `app/folha/retrato_folha.py:1118` · `app/folha/views_retrato.py:500` · `app/folha/views.py:15`
- **Forma exata da linha construída.** As chaves são `colab`, `codigo`, `status`, `motivo`, `dias`, `datas`, `dias_sem_leitura`, `dias_sem_porta`, `dono`, `acoes`, `tipo`, `saldo` e `informativo`. `datas` é lista plana ISO `AAAA-MM-DD` ou `None` quando a fonte não dá datas, com possibilidade de a chave faltar em retratos legados. As duas listas `dias_sem_*` registram sem leitura e sem porta. `acoes` guarda as portas publicadas, algumas com `dia_do_turno`. `informativo` é uma lista ISO separada para rescisão. Nenhuma dessas chaves guarda a associação geral `data → motivo/tipo de retenção`. `app/folha/retrato_folha.py:1227` · `app/folha/retrato_folha.py:1237` · `app/folha/retrato_folha.py:1288` · `app/folha/fora_da_folha.py:215` · `app/folha/models.py:334`
- **Onde havia o par.** `motivos_retencao_celula(com_dias=True)` devolve motivos agregados e registros `{data, motivo, veredito, celula_id}`. `fora_da_folha` tem esses registros em `_dias`, mas publica no resumo por pessoa `motivos` agregados e `datas` planas. O lavrador leva as datas ao retrato e usa `dias_ferias_com_batida` como segunda fonte. A função avulsa `o_que_segura` expõe `data`, `motivo`, `veredito` e `fio` por dia e consulta o export em sua própria execução. `app/folha/export.py:1195` · `app/folha/export.py:1268` · `app/folha/fora_da_folha.py:214` · `app/folha/fora_da_folha.py:234` · `app/folha/fora_da_folha.py:257` · `app/folha/fora_da_folha.py:273` · `app/folha/fora_da_folha.py:292` · `app/folha/retrato_folha.py:1231`

### B. Censo de escritores e leitores

| Campo | Escrita e leitura na cópia |
|---|---|
| `motivo` | `classificar_export` produz a chave no item. `montar_linhas` copia o valor para a linha. `_balde` usa o motivo para reconhecer rescisão. `fio_da_linha`, `tipo_da_linha`, `saldo_da_linha` e `no_periodo` também o consultam. Tela, PDF, Raio-X e drawer passam o valor a `rotulo_do_motivo`. `app/folha/fora_da_folha.py:147` · `app/folha/export.py:977` · `app/folha/retrato_folha.py:1225` · `app/folha/retrato_folha.py:89` · `app/folha/retrato_folha.py:444` · `app/folha/retrato_folha.py:496` · `app/folha/retrato_folha.py:1338` · `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` · `app/folha/views_retrato.py:503` · `app/folha/retrato_folha.py:1116` |
| `dono` | `_fio_cru` grava o **fio** (`admin`, `colab`, `chamado`, `ninguem`, `nao_sei` ou vazio). `fio_da_linha` lê a chave, com tratamento de legado. `dono_da_linha` traduz o fio para o balde da UI e trata `Hoje` usando `datas`. Esses leitores alimentam o recorte, os contadores, o LED, o drawer e o vigia. `app/folha/retrato_folha.py:178` · `app/folha/retrato_folha.py:351` · `app/folha/retrato_folha.py:436` · `app/folha/retrato_folha.py:486` · `app/folha/retrato_folha.py:588` · `app/folha/retrato_folha.py:1112` · `app/folha/retrato_folha.py:1248` · `app/folha/vigia_retido_calado.py:77` |
| `tipo` | `_tipo_cru` grava um tipo por pessoa a partir do motivo ou de `FechamentoMensal.causa_espelho['causa']`. `_causas_gravadas` lê essa causa. `tipo_da_linha` é o leitor do recorte, do agrupamento do Raio-X e dos rótulos e portas do drawer. `app/folha/retrato_folha.py:379` · `app/folha/retrato_folha.py:496` · `app/folha/retrato_folha.py:588` · `app/folha/retrato_folha.py:1066` · `app/folha/retrato_folha.py:1124` · `app/folha/retrato_folha.py:1167` · `app/folha/retrato_folha.py:1250` · `app/folha/views_retrato.py:506` |
| `datas` | `montar_linhas` guarda a lista. `no_periodo` lê a lista e devolve cópias com `dias` e `saldo` ajustados, mantendo `datas` integral. `dono_da_linha` lê a lista para `Hoje`. O drawer localiza a linha via `linha_do_colab` e imprime as datas em ISO. `app/folha/retrato_folha.py:1231` · `app/folha/retrato_folha.py:489` · `app/folha/retrato_folha.py:1334` · `app/colaboradores/views.py:1001` · `app/templates/folha/_retrato_o_que_fazer.html:42` |
| `motivos_espelho` | Chave do **item do export**, fora do JSON do retrato. `classificar_export` a preenche com a lista de códigos agregados de `motivos_retencao_espelho`, que delega ao retentor. Leitores diretos do item incluem a prévia da folha, o PDF legado de retidos e `fechamento_lista._folha_do_juiz`. Há um campo homônimo em `FechamentoMensal`, preenchido por `recalcular_fechamento_mes` a partir do mesmo juiz. Tela e PDF do retrato leem `motivo` para a coluna. `app/folha/export.py:977` · `app/folha/export.py:1310` · `app/folha/views.py:267` · `app/folha/views.py:424` · `app/folha/retidos_pdf.py:90` · `app/ponto/services/fechamento_lista.py:28` · `app/ponto/models.py:451` · `app/ponto/services/fechamento.py:1091` · `app/ponto/services/fechamento.py:1210` · `app/folha/retrato_folha.py:1237` · `app/folha/views_retrato.py:291` · `app/folha/views_retrato.py:424` |

O escritor persistente do retrato é `lavrar_retrato_folha --apply`, com `update_or_create` por empresa, mês e ano. `montar_linhas` constrói o JSON. O cron chama a mesma porta. O selo AST exige um único chamador de produção de `montar_linhas`. `app/folha/management/commands/lavrar_retrato_folha.py:65` · `app/folha/management/commands/lavrar_retrato_folha.py:72` · `app/config/crons.py:1659` · `app/folha/tests/test_retrato_folha_um_derivador.py:68`

**Contadores de dono.** A tela chama `quadros_barra_retrato`, e `_pilula` calcula `numero = len(recortar(...))` sobre o universo após filtro universal e janela. A pílula ativa conta o recorte atual. Cada pílula inativa conta o recorte que seu clique abriria, substituindo o dono e mantendo os outros eixos. O PDF chama a mesma função, com o mesmo universo e grupos, mas consome apenas título e rótulo dos filtros ativos. Hoje imprime `n de total`, sem os `numero` por dono. `contadores(retrato)` é outra função e conta os baldes gerais `entram`, `bloqueado`, `modulo_proprio` e `total`. `app/folha/views_retrato.py:324` · `app/folha/retrato_folha.py:808` · `app/folha/retrato_folha.py:838` · `app/templates/core/_barra_gestao.html:67` · `app/folha/views_retrato.py:399` · `app/folha/views_retrato.py:409` · `app/folha/retrato_folha.py:114`

### C. Como abre e precedente de `attachment`

O botão da barra é `<a href="{{ pdf_href }}">` do partial `core/_btn_pdf.html`, sem `target="_blank"`, sem `download` e sem `onclick` nesse ramo. A view monta o `href` da rota `/folha/exportar/retrato.pdf` com `qs_do_recorte`; o mesmo botão volta no parcial. A resposta atual é `application/pdf` com `Content-Disposition: inline; filename="retrato-folha-emp…-MM-AAAA.pdf"`. Resultado do clique no navegador de produção `[nao verificado]`. `app/templates/folha/_retrato_barra.html:50` · `app/templates/core/_btn_pdf.html:34` · `app/templates/folha/_retrato_parcial.html:8` · `app/folha/views_retrato.py:337` · `app/config/urls.py:34` · `app/folha/urls.py:25` · `app/folha/views_retrato.py:450`

Na mesma casa, o PDF do histórico do TXT, `retidos_pdf` e `validar_export_pdf` usam `application/pdf` com `Content-Disposition: attachment; filename="…"`. O `retidos_pdf` legado chama `classificar_export` ao vivo. O precedente para este item é o cabeçalho HTTP. `app/folha/views.py:860` · `app/folha/views.py:884` · `app/folha/views.py:911`

### D. Rodapé e fontes de commit [nao verificado]

`gerar_pdf_auditavel` lê `get_commit_git()` uma vez, passa o resultado aos rodapés das duas passagens e grava `RegistroPDFAuditavel.commit_git`. `RodapeAuditavel.__call__` imprime os oito primeiros caracteres ou `n/d` quando recebe vazio. O helper prioriza `COMMIT_GIT` e, na falta dela, tenta `git -C /app rev-parse HEAD` se `/app/.git` existir. Se nenhum caminho entregar valor, retorna `''`. `app/relatorios/pdf_base.py:72` · `app/relatorios/pdf_base.py:133` · `app/relatorios/pdf_base.py:152` · `app/relatorios/pdf_base.py:170` · `app/relatorios/utils_audit.py:7`

A montagem documentada leva `app/` a `/app`, enquanto o repositório fica no diretório pai no host. O Dockerfile usa `python:3.13-slim`, copia o contexto e não declara `COMMIT_GIT` nem instala `git` explicitamente. Isso explica o caminho de `n/d` quando o ambiente efetivo também não fornece a variável. O compose de produção é excluído da cópia versionada. A variável, o executável, `/app/.git` e o SHA efetivo da casca de produção estão `[nao verificado]`. `CLAUDE.md:88` · `app/.dockerignore:8` · `app/Dockerfile:1` · `app/Dockerfile:30` · `app/Dockerfile:40` · `.gitignore:40` · `app/relatorios/utils_audit.py:12`

| Fonte existente | O que identifica |
|---|---|
| `AMBIENTE sha=` | `HASNER_SHA` injetado pela suite para a árvore de teste medida. O teste imprime a variável e `pista_certifica.sh` compara o valor com o SHA do pouso. A disponibilidade na casca web de produção está `[nao verificado]`. `bin/suite.sh:109` · `bin/suite.sh:566` · `app/core/tests/test_ambiente_do_run.py:52` · `bin/pista_certifica.sh:48` |
| Carimbo de deploy | `bin/deploy.sh` escreve o `COMMIT` lido do Git do **host** em `logs/deploy.stamp`. O vigia já lê esse carimbo. A correspondência com o código carregado pelo worker que gera um PDF está `[nao verificado]`. `bin/deploy.sh:430` · `app/core/esteira_vigia.py:1032` · `app/core/esteira_vigia.py:1202` |
| `/health/` e versão do app | O health do core devolve `ok`, `schema` e `host`. O de mensageria devolve `ok` e `servico`. A versão da API é a versão do aplicativo Android/iOS. Esses retornos não incluem SHA do servidor. `app/config/urls_core.py:5` · `app/core/views.py:1306` · `mensageria/config/urls.py:7` · `app/api/views.py:3068` |

### E. Escala no PDF

A célula recebe `getattr(_l.get('_escala'), 'descricao', '') or ''` como **string crua**. As larguras são `26 / 68 / 48 / 38 / 60 / 24 mm`, com **38 mm para Escala** e **60 mm para Por quê**. A tabela repete o cabeçalho, usa fonte 7 e alinhamento vertical no topo. Os `Paragraph` dessa função estão no título e no resumo, fora da tabela. O PDF legado de retidos mostra o uso de `Paragraph(escape(...))` dentro de célula. O vazamento em um PDF real está `[nao verificado]`. `app/folha/views_retrato.py:406` · `app/folha/views_retrato.py:423` · `app/folha/views_retrato.py:431` · `app/folha/retidos_pdf.py:91`

### F. Ordem atual

- **Linhas da tela e do PDF.** Ambos iteram o mesmo `_vis`, ordenado por **Bloqueados → Entram → Rescisão**, depois pelas faixas **1 → 2 a 5 → 6 ou mais → Cadastro → sem faixa**, depois pelo ID do colaborador. Dois bloqueados na mesma faixa são ordenados pelo ID, mesmo com quantidades de dias diferentes. `app/folha/views_retrato.py:232` · `app/folha/views_retrato.py:293` · `app/templates/folha/_retrato_lista.html:35` · `app/folha/views_retrato.py:417` · `app/folha/retrato_folha.py:754`
- **Donos na barra.** **Admin → Colab → Hoje → Sem ação disponível → Sem leitura (sistema)**. A ordem vem de `DONOS` via `VALORES_DO_GRUPO` e `quadros_barra_retrato`. `Entram` e `Rescisão` são baldes sem dono, com rótulos próprios no PDF. `app/folha/retrato_folha.py:191` · `app/folha/retrato_folha.py:200` · `app/folha/retrato_folha.py:308` · `app/folha/retrato_folha.py:590` · `app/folha/retrato_folha.py:908`

### G. Leis e cortes

- **L-002, L-007 e L-011.** “TESTEMUNHA LE, NAO RECALCULA” exige uma autoridade comum para tela, PDF, TXT, badge e contador. L-007 exige um escritor canônico por estado. L-011 exige a frase de corte para registrar um juiz novo. `classificar_export` está registrado em K1 e K2. O corte `O256-LEGENDA-E-PDF-ANTES-DO-RAIO-X` manda o PDF respeitar os filtros da barra e fixa “Juizes novos = 0”. `app/docs/LEIS.md:60` · `app/docs/LEIS.md:65` · `app/docs/LEIS.md:69` · `app/core/juizes.py:336` · `app/docs/CORTES.md:407`
- **L-026/L-090 e L-038.** As duas primeiras protegem a janela da empresa em `ponto/janelas.py`, usada na entrada do recorte. L-038 diz “o PDF nao tem regra de dia; le o MESMO juiz do espelho” e nomeia `relatorios/pdf_espelho.py::_coletar_dados_espelho`. A paridade específica do PDF do retrato está em L-002 e no corte O256 acima. `app/docs/LEIS.md:77` · `app/docs/LEIS.md:86` · `app/docs/LEIS.md:123` · `app/folha/views_retrato.py:177` · `app/docs/CORTES.md:407`
- **Leis de UI L1, L2, L6, L7 e L9.** Contador e lista vêm da mesma fonte, tela e ação consultam a mesma função, o filtro é cumulativo e único, a ajuda sai do mesmo mapa, e o botão PDF usa um partial único com rótulo `PDF` e ajuda no `title`. `RETRATO-DONO-SAO-TRES` registrou a soma dos donos com os bloqueados, e LU-05 deu balde próprio ao dia sem porta. A cópia também declara o balde `Sem leitura (sistema)`. `app/docs/LEIS-UI.md:12` · `app/docs/LEIS-UI.md:23` · `app/docs/LEIS-UI.md:68` · `app/docs/LEIS-UI.md:77` · `app/docs/LEIS-UI.md:95` · `app/docs/CORTES.md:408` · `app/docs/CORTES.md:539` · `app/folha/retrato_folha.py:191`
- **“NENHUM TEXTO SE GERA”.** O literal de `O256-ORDEM-DRAWER-PRIMEIRO` se refere ao bloco **O que fazer** do drawer, “montado SO das chaves gravadas na linha do retrato (motivo, dono, tipo, datas), sem texto gerado”. A docstring diz “os valores sao ROTULOS de mapa [...] nunca frase montada”. O selo do drawer exige que o valor inteiro pertença literalmente a um mapa existente. O exemplo do item 2 tem a forma de **rótulos concatenados com datas**. A permissão para essa composição na coluna da tela e no PDF, e sua eventual extensão ao drawer, fica em aberto. `app/docs/CORTES.md:405` · `app/folha/retrato_folha.py:1089` · `app/folha/tests/test_retrato_folha_drawer.py:213`

### H. Testes atuais e o que fica vermelho [nao verificado]

- **PDF.** `test_retrato_pdf.py` cobre bytes `%PDF`, tipo, nome canônico no header, hash e registro, igualdade de tamanho do recorte com a tela, filtros na trilha, zero chamada à fonte, papel vazio, permissão, link no parcial e recorte comum por AST. O teste de header procura o **nome** e aceita `attachment` com o mesmo nome. O teste de clique segue o `href` com `client.get`. Ele não mede abas ou navegação de navegador. `app/folha/tests/test_retrato_pdf.py:93` · `app/folha/tests/test_retrato_pdf.py:99` · `app/folha/tests/test_retrato_pdf.py:108` · `app/folha/tests/test_retrato_pdf.py:127` · `app/folha/tests/test_retrato_pdf.py:138` · `app/folha/tests/test_retrato_pdf.py:152` · `app/folha/tests/test_retrato_pdf.py:163` · `app/folha/tests/test_retrato_pdf.py:221` · `app/folha/tests/test_retrato_pdf.py:252` · `app/folha/tests/test_retrato_pdf.py:330`
- **Por quê.** Há assert do cabeçalho e do vínculo com `MOTIVO_RH`. O teste da listagem verifica a presença do rótulo de cadastro, sem exigir igualdade da célula inteira. Se a composição for aplicada ao valor do **drawer**, o selo de literal de mapa fica vermelho. Se a janela passar a mudar o texto de **Por quê** na linha comparada pelo teste de período, o assert que preserva todas as colunas salvo Dias fica vermelho. `app/folha/tests/test_retrato_folha_tela2.py:253` · `app/folha/tests/test_retrato_folha_contador.py:190` · `app/folha/tests/test_retrato_folha_drawer.py:181` · `app/folha/tests/test_retrato_folha_drawer.py:213` · `app/folha/tests/test_retrato_folha_periodo.py:371`
- **Ordem, contagem e botão.** Alterar `DONOS` para obter a ordem do PDF quebra o assert da ordem da barra. Mudar a ordem compartilhada de forma que a tela deixe de seguir as faixas quebra as expectativas de ordenação e período. Escrever um botão PDF fora do partial quebra L9. Os REDs abaixo são previsões por leitura. Execução da suíte `[nao verificado]`. `app/folha/tests/test_retrato_folha_dono_sao_tres.py:47` · `app/folha/tests/test_retrato_folha_dono_sao_tres.py:105` · `app/folha/tests/test_retrato_folha_quadros.py:569` · `app/folha/tests/test_retrato_folha_periodo.py:354` · `app/core/tests/test_contract_btn_pdf.py:87`

### I. Matriz de REDs descritos

| Item | Entrada de teste | Saída esperada | [nao verificado]
|---|---|---|
| **1. Download** | Admin com empresa, competência e pílula ativos. Clicar no PDF após atualizar o filtro. | Mesmo `href` do recorte vigente, PDF válido, `attachment` com o nome canônico, mesma URL da tela e mesma quant

[truncado em 20000 de 24044 caracteres]

## 28/11 00:31 · sessao 48b9f6bb0a8f · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**APROVADO para a cura c12 no recorte `base.html` a 1366 px, página e drawer. Nenhum bloqueante de código encontrado.** O BLOQ 1 está curado pela ordem de pintura. A execução do GREEN e do RED da c12, a suíte inteira e o clique real pré-pouso seguem [nao verificado]. O clique real continua como pré-requisito do deploy. `aval_desktop.txt:1` · `ver3_triagem.md:3` · `app/templates/core/_drawer_generico.html:3` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `CLAUDE.md:206` · `CLAUDE.md:427`

### 1. Ordem de pintura e chamadores

O cabeçalho continua `sticky; top:0` e passa de `auto` para `z-index:1` **dentro** do drawer `10999`. Toda caixa de célula passa atrás do cabeçalho quando há sobreposição. A célula mensal isolada fica na camada `0` do drawer. O `1` do gatilho e o `0` da região rolável são internos à célula. O título e o × pertencem ao mesmo cabeçalho. No Retrato, o calendário entra direto no corpo rolável. No Espelho, entra pela aba com rolagem própria. `app/templates/core/_drawer_generico.html:3` · `app/templates/core/_drawer_generico.html:8` · `app/templates/core/_drawer_generico.html:134` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/templates/colaboradores/partials/_calendario_grade.html:95` · `app/templates/colaboradores/partials/painel_colaborador.html:182` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/folha/retrato_folha.py:1019`

**A célula continua abaixo do cabeçalho com o popover aberto. O painel destacado fica acima.** O JS move esse painel para o `body` em `11000`. A confirmação em `100000` cobre a viewport e recebe o clique sobre o × enquanto estiver visível. O posicionador limita o popover ao retângulo visível do drawer sem reservar a faixa do cabeçalho. Se o painel cruzar o ×, ele ganha o hit-test. O cruzamento efetivo a 1366 px [nao verificado]. O detalhe geo hospedado no corpo também já fica acima do cabeçalho pelo `9999` local. `app/static/js/popover-acao.js:36` · `app/static/js/popover-acao.js:67` · `app/static/js/popover-acao.js:127` · `app/templates/core/_dialogo.html:112` · `app/templates/core/_dialogo.html:119` · `app/templates/colaboradores/calendario_partial.html:70` · `app/templates/colaboradores/partials/_geo_modal.html:2`

**A promoção também alcança outros conteúdos na camada `0`.** A barra inferior `sticky` de Ausências e os controles do painel do colaborador enquanto recebem `transform` podiam pintar acima do cabeçalho por ordem de árvore. Agora pintam abaixo dele se houver interseção. O cabeçalho da tabela da seção de Férias (`2`), as sugestões de Posto (`5`) e as de Vínculo (`50`) continuam acima em caso de interseção. Com o cabeçalho subindo de `0` para `1`, nenhum conteúdo passa de baixo para cima. Sobreposição efetiva nesses caminhos e as camadas geradas pelas bibliotecas externas de mapa [nao verificado]. `app/templates/core/_drawer_generico.html:4` · `app/templates/ponto/partials/painel_ausencia.html:286` · `app/templates/colaboradores/partials/painel_colaborador.html:147` · `app/templates/ferias/_secao_colab.html:112` · `app/static/css/hasner-ponto.css:433` · `app/templates/colaboradores/partials/painel_posto.html:57` · `app/templates/colaboradores/partials/painel_vinculo.html:153` · `app/templates/colaboradores/postos.html:74`

| Caminho | Chamadores na cópia |
|---|---|
| Inclusão do genérico | As duas cascas incluem o drawer no corpo. A inclusão textual da Central está dentro do `<title>`. A de Folha está fora dos blocos do template filho. O drawer ativo nesses dois caminhos vem de `base.html`. `app/templates/base.html:7` · `app/templates/base.html:399` · `app/templates/base_app.html:154` · `app/templates/chamados/painel_gestao.html:1` · `app/templates/folha/index.html:1` |
| Colaboradores | Lista via `abrirPainelColab`, Benefícios, Postos e diagnóstico pelas duas entradas. `app/templates/colaboradores/partials/_lista_tabela.html:16` · `app/templates/core/_drawer_generico.html:91` · `app/templates/core/_drawer_generico.html:106` · `app/templates/colaboradores/beneficios_lista.html:86` · `app/templates/colaboradores/postos.html:65` · `app/templates/colaboradores/partials/_ps_linha.html:4` · `app/templates/colaboradores/partials/pendencias_disputa.html:9` |
| Folha e Retrato | Raio-X da exportação, Raio-X do Retrato, ficha pela lista ou pelo Raio-X, e links de ação para Fio, Vínculo, Cadastro, Calendário e Ausência. `app/templates/folha/index.html:26` · `app/templates/folha/_retrato_barra.html:73` · `app/templates/folha/_retrato_lista.html:61` · `app/templates/folha/_retrato_raiox.html:70` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/folha/retrato_folha.py:989` · `app/folha/retrato_folha.py:1003` · `app/folha/retrato_folha.py:1016` |
| Chamados e mapas | Arquivados do atendimento, lote, busca de arquivo e Raio-X da Central, troca de empresa na lente e três entradas que escrevem direto no drawer para mostrar mapa. `app/templates/chamados/meu_atendimento.html:45` · `app/templates/chamados/painel_gestao.html:14` · `app/templates/chamados/painel_gestao.html:416` · `app/templates/chamados/painel_gestao.html:629` · `app/templates/chamados/partials/_raiox_lente.html:5` · `app/templates/chamados/painel_gestao.html:574` · `app/templates/chamados/fio_colaborador.html:304` · `app/static/js/geo-colab-map.js:33` |
| Ausências, Férias e Holerite | Painel de Ausências, seção de Férias pela lista e pela aba da ficha, upload de lote pela lista DP e pelo histórico, e detalhe da matriz por modo. `app/templates/ponto/ausencias.html:217` · `app/templates/ferias/lista.html:81` · `app/templates/core/_drawer_generico.html:147` · `app/templates/holerite/lista_dp.html:12` · `app/templates/holerite/historico_lotes.html:7` · `app/templates/holerite/_quadrinho_celula.html:17` |

### 2. Quem escreve e quem lê

| Valor | Escrita e leitura |
|---|---|
| Estilo do cabeçalho e camadas | O template escreve `sticky`, `top:0` e o novo `z-index:1`. O navegador consome essas declarações para pintar e resolver o hit-test. O mutante lê o retorno de `removeProperty` e remove o valor inline apenas no DOM do ensaio. Drawer, painel destacado e confirmação têm seus próprios escritores de camada. `app/templates/core/_drawer_generico.html:3` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:214` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/static/js/popover-acao.js:133` · `app/templates/core/_dialogo.html:112` |
| Seletores do cabeçalho e do drawer | O genérico fornece `#drawer-geral`, `#drawer-geral-backdrop`, `#drawer-geral-titulo` e o botão que chama `fecharDrawer`. A sonda lê o drawer e o fundo e acha o botão no pai do título. A abertura padrão e os três caminhos de mapa escrevem texto no mesmo título. O botão continua chamando a função que recolhe o drawer e emite `drawer:fechou`. `app/templates/core/_drawer_generico.html:2` · `app/templates/core/_drawer_generico.html:11` · `app/templates/core/_drawer_generico.html:71` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:210` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:246` · `app/templates/chamados/painel_gestao.html:576` · `app/templates/chamados/fio_colaborador.html:306` · `app/static/js/geo-colab-map.js:35` |
| Seletores e dia do feriado | O parcial do feriado escreve `data-feriado-vira-trabalho` e `value` no campo oculto `dia`, com a data ISO. A grade escreve `data-celula-dia` no ancestral. Python escreve `DIA.isoformat()` no JSON. As duas sondas usam os mesmos seletores para achar ação e célula. O formulário envia a mesma data à view. O delta reaproveita os atributos existentes. `app/templates/ponto/partials/_feriado_vira_trabalho.html:9` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:212` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:298` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:756` · `app/ponto/views.py:3584` · `app/ponto/views.py:3615` |
| Entrada e saída novas do smoke | Python escreve `cabecalhoDrawer` no JSON para selecionar o cenário do cabeçalho. A sonda o lê para buscar `urlCalendario` direto e entrar na medição. `urlPainel` continua indicando que o cenário usa drawer e `respostas` serve os HTMLs do Client. A sonda escreve `gridArea`, solicita a rolagem, relê `scrollTop` e mede os centros após um quadro. Ela escreve `fecharNoCabecalho` e `tituloNoCabecalho` como resultados de hit-test. O driver CDP transmite os valores e Python os compara. Os booleanos medem quem recebe o ponto. O fechamento do drawer e o texto do título são observações distintas. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:124` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:218` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:244` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:756` · `app/core/tests/smoke_feriado_chromium.js:52` |

### 3. Mordida e cobertura

- **Tirar o `z-index`** é o RED explícito. A sonda tira a declaração inline, exige que a célula esteja sob os dois pontos e o `MORDE` exige `False` nos dois hit-tests. Uma falha de montagem ou a ausência prévia da declaração produz outra mensagem e reprova o controle negativo. A execução do par GREEN/RED [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:214` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:765`
- **Pôr `z-index` abaixo de `1`** deve derrubar o positivo. Em `0`, a célula posterior volta a ganhar por ordem de árvore. Com valor negativo, o cabeçalho desce ainda mais. **Tirar o `sticky`** também deve derrubá-lo, pela guarda de sobreposição. Cabeçalho e célula passam a rolar juntos e a célula deixa de cruzar os centros do título e do ×. São consequências da geometria e das asserções. A execução desses dois mutantes [nao verificado]. `app/templates/core/_drawer_generico.html:3` · `app/templates/colaboradores/partials/_calendario_grade.html:68` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:219` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:716`
- **Lacuna no desktop.** O novo teste carrega o parcial direto numa casca de ensaio, alarga uma célula e mede os dois centros com o painel fechado. O smoke anterior cobre o popover na ficha e no Espelho, e fecha o drawer chamando `window.fecharDrawer()`. Falta o percurso integrado pelo link real do Retrato, com a grade intacta, rolagem e clique no ×, inclusive a disputa com o popover e a confirmação. O submit do feriado tem hit-test no navegador e teste do POST com motivo no Client Django. O percurso inteiro pelo navegador segue [nao verificado]. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:208` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:421` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:488` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:731` · `app/templates/folha/_retrato_o_que_fazer.html:73` · `app/ponto/tests/test_kaua_feriado_pela_tela.py:149`

### 4. Classes que já reprovaram obra

- **Segundo juiz, segundo escritor, relógio fora da borda e derivação paralela** — zero introduzidos no delta fornecido. A grade e o POST leem a oferta de `contexto_calendario`. O POST captura `agora` uma vez e passa esse instante à consulta e à porta. A porta grava a célula e a trilha. `app/templates/colaboradores/partials/_calendario_grade.html:96` · `app/colaboradores/services/calendario.py:791` · `app/ponto/views.py:3622` · `app/ponto/portas/celula.py:650` · `app/ponto/portas/celula.py:665`
- **Campo com dois significados, selo sem mordida e exceções** — zero campos ambíguos encontrados. O novo flag escolhe o cenário, e os dois campos de saída têm uma única leitura de hit-test. O RED exige a falha específica nos dois alvos. O delta introduz **0 `except` Python e 0 `catch` JavaScript**. O tratamento existente propaga a falha da sonda como `motivo`, que o positivo exige nulo. `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:235` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:502` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:579` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:612`
- **Ordem de trava e pré-requisitos do pouso** — a porta entra em `atomic`, lê com `select_for_update`, aplica `so_se_folga` e então grava. A ordem operacional exigida é suíte inteira verde, revisão final e pouso, com o smoke humano antes do push. O cumprimento dessa ordem e a identidade Git do hash nesta cópia seguem [nao verificado]. A leitura usa o delta fornecido e as linhas correspondentes da cópia. `app/ponto/portas/celula.py:642` · `app/ponto/portas/celula.py:655` · `CLAUDE.md:206` · `CLAUDE.md:421` · `CLAUDE.md:427`

### 5. Lista do pouso [nao verificado]

- **360 px e `base_app` real.** Conferir página e drawer a 360, incluindo rolagem e acionabilidade. A ficha tem coluna inicial de `420px` e o drawer é limitado a `92vw`. Conferir também a entrada e o bootstrap do WebView. O ensaio existente de `base_app` usa desktop a 1366 e desativa o PWA. Resultados reais [nao verificado]. `aval_desktop.txt:1` · `app/templates/colaboradores/detalhe.html:87` · `app/templates/core/_drawer_generico.html:3` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:699` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:745`
- **Smoke humano pré-pouso.** Clique do Ronald no 20/11 real nas duas cascas, com as verificações pendentes do drawer em rolagem. Resultado [nao verificado]. `ver3_triagem.md:3` · `app/docs/LEIS.md:99` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:231`
- **Lacunas da ver3.** Tab, POST com motivo pelo Chromium, paginação real da lista com drawer aberto e reabertura após restaurar histórico. Snapshot antigo com painel órfão também fica na lista. O fechamento pré-snapshot atua sobre o popover corrente. Ocorrência de snapshot antigo afetado [nao verificado]. `ver3_triagem.md:4` · `ver3_triagem.md:5` · `app/templates/colaboradores/partials/_lista_tabela.html:50` · `app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:742` · `app/static/js/popover-acao.js:144` · `app/static/js/popover-acao.js:176`

_8837 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
