# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 14:40** · publicado em **11/11/2026 14:41** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 14:40 · sessao 57d93ad8e30c · - · - · origem=Ronald (tmux)

(b) **4 leituras comprovadas em 2 templates**, mais **1 expressão com variável não vinculada**:

| Local | Origem/contexto | Uso | Diverge para conversa |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:87` | `p` no laço `chamado.perguntas_disputa` (`:77`); perguntas carregadas por `montar_chamados_fio`, `app/chamados/services/fio.py:225`, agrupadas `:243`, anexadas `:253`; view `app/chamados/views.py:569`, contexto `:639`, `:640`; includes `app/templates/chamados/meu_atendimento.html:127`, `:144` | Testa presença para mostrar cabeçalho “Estamos falando do dia” (`_card_atendimento.html:89`) | **sim**: conversa possui data civil; juiz retorna `None`, `app/chamados/juizes.py:338` |
| `app/templates/chamados/partials/_card_atendimento.html:90` | Mesmo contexto | Exibe como dia de referência/fato sob aquele cabeçalho | **sim**, `app/chamados/juizes.py:338` |
| `app/templates/chamados/partials/_card_atendimento.html:164` | Mesmo contexto | Exibe dia na confirmação JS de opção “Não trabalhou nesse dia?” (`:160`) | **sim**, se pergunta de conversa alcançar esse ramo; ocorrência concreta em produção **[nao verificado]** |
| `app/templates/chamados/partials/_lote_preview.html:24` | `x.pergunta` de `linhas`; view `app/chamados/views_cobrar.py::lote_preview:361`, `:363`; serviço `app/chamados/services/validacao.py::fila_validar_classificada:921`, `:923` coloca instâncias de pergunta no dicionário | Exibe dia na prévia de validação | **sim**, se conversa alcançar classe A; alcance concreto **[nao verificado]**; juiz `app/chamados/juizes.py:338` |

A expressão `app/templates/chamados/meu_atendimento.html:19` usa `p.data_evento` fora de um laço de perguntas; seu formulário está no laço de `reg`, iniciado `:10`. O contexto de `meu_atendimento` não fornece `p` (`app/chamados/views.py:633–652`). Portanto, **não é leitura comprovada de PerguntaDisputa**; divergência **não se aplica**. Há cinco expressões `.data_evento` executáveis ao todo, mas quatro com origem comprovada.

`app/templates/chamados/partials/modal_fio.html:161` contém somente comentário; não integra leitores.

(c) **2 produtores de chave JSON `data_evento`, em 2 endpoints; 1 chave derivada `data_evento_label`; 1 consumidor explícito encontrado na cópia**:

| Local | Função/rota | Uso | Diverge para conversa |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2`, rota `app/api/urls.py:73` | Serializa `p.data_evento` em ISO ou `None`; perguntas pertencem a `perguntas_por_chamado`, `app/api/views.py:1040`, `:1046`, `:1174`; resposta `:1189` | **sim** no valor transmitido, juiz `app/chamados/juizes.py:338`; exibição pelo APK **[nao verificado]** |
| `app/api/views.py:1166` | Mesmo endpoint; chave derivada `data_evento_label` | Formata o mesmo valor como `%d/%m/%Y`; resposta `:1189` | **sim**, juiz `app/chamados/juizes.py:338`; exibição pelo APK **[nao verificado]** |
| `app/api/views_mensageria.py:568` | `_item_validar`; usado por `api_mensageria_validar`, `:546`; rota `app/api/urls.py:12`; resposta `views_mensageria.py:549` | Serializa ISO ou `''`; só repassa | **sim** no valor transmitido, juiz `app/chamados/juizes.py:338` |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` (`:98`) | Lê `i.get('data_evento')`, renomeia para `data_do_caso`, repassa ao contexto; recebe payload `:107`, percorre itens `:129` | **sim** no valor repassado; uso final pela IA **[nao verificado]** |

O consumidor HTTP da mensageria chama `/api/mensageria/validar/` em `mensageria/nucleo/core_client.py:50` e devolve JSON integral em `:55`. As outras chamadas à fila em `mensageria/nucleo/ferramentas.py:190` e `:1529` usam classe/referências/contagens, sem ler a chave (respectivamente `:194–205` e `:1532–1543`).

Consumidores JS ou código do APK para essas chaves: **[nao verificado]**. Não apareceu literal `data_evento` nos arquivos JS/TS/JSX/TSX/Vue/Dart/Kotlin/Swift pesquisados **[nao verificado]**.

Observação para contagem AST de (a): `api/views.py:1165`, `:1166` e `api/views_mensageria.py:568` contêm **dois acessos de atributo cada** (teste condicional + formatação), portanto **6 nós AST em 3 posições arquivo:linha**. O consumidor de dicionário é mais uma posição: `mensageria/nucleo/ferramentas.py:127`.

## 11/11 14:37 · sessao 889902a5ad4d · MAPA-20 · verificador · origem=Code

**BLOQUEANTE — a v5 ainda aceita N sem fundamento, X sem prova e declaração de encerramento referente a outra obra incompleta.** O tripwire não corrige essas falhas semânticas. (`bin/mapa20_valida.py:91–107`, `bin/mapa20_valida.py:209–228`; `bin/mapa20_montar.py:123–124`)

1. **Entradas concretas que passam com rc 0**

**BLOQUEANTE — N inventado e X sem prova.** `app/docs/BACKLOG.md:1` contém apenas o título do arquivo. Mesmo assim, uma linha com `1 de 1`, essa fonte e estado `encerrada` passa: o selo verifica caminho permitido e linha não vazia, sem contar PRONTO, identificar a obra ou consultar provas no RELATO. (`app/docs/BACKLOG.md:1`; `bin/mapa20_valida.py:91–107`, `bin/mapa20_valida.py:209–216`)

**BLOQUEANTE — encerramento atribuído a outra obra.** E3 em `1 de 1` pode conter `E4B: encerrada`, enquanto a própria linha E4B permanece em `1 de 2`. A autorização acompanha o índice da linha E3; o selo não identifica a obra mencionada na afirmação. (`bin/mapa20_valida.py:213–214`, `bin/mapa20_valida.py:225–228`)

Este gerador mostra as duas entradas completas, sem criar arquivo. Nas sondas, ambas retornaram **rc 0**; a segunda também conservou `E4B: encerrada` na renderização local de tabela. O leitor e o retorno usados são os de `bin/mapa20_valida.py:246–259`.

```bash
python3 -B - <<'PY'
import subprocess, sys
sys.path.insert(0, "bin")
import mapa20_valida as V

for caso in ("N_SEM_FUNDAMENTO", "OUTRA_OBRA"):
    linhas = [
        "## PRONTO AUSENTE", "", "- nenhuma", "", V.LEGENDA, "",
        "| " + " | ".join(V.CABECALHO) + " |",
        "|" + "---|" * len(V.CABECALHO),
    ]
    for obra in V.OBRAS:
        c = [obra] + ["nao medido"] * 8 + [
            "1 de 2", "app/docs/BACKLOG.md:1", "item 2"
        ]
        if obra == "E3":
            c[9], c[11] = "1 de 1", "nada"
            c[7] = ("encerrada" if caso == "N_SEM_FUNDAMENTO"
                    else "E4B: encerrada")
        linhas.append("| " + " | ".join(c) + " |")
    c = [V.SEM_DONO] + ["nao medido"] * 8 + [
        V.XN_SEM_DONO, "nao medido", "nao medido"
    ]
    linhas.append("| " + " | ".join(c) + " |")
    entrada = "\n".join(linhas) + "\n"
    r = subprocess.run(
        [sys.executable, "-B", "bin/mapa20_valida.py", "/dev/stdin"],
        input=entrada, text=True, capture_output=True
    )
    print(caso, "rc", r.returncode)
PY
```

São contraexemplos do **selo real**. Publicação dessas entradas em produção, com os dados vivos, [não verificado].

2. **Bytes publicados, corridas e retornos** [nao verificado]

| Situação | Resultado da leitura |
|---|---|
| Caminho novo aprovado | Raspa a fotografia, julga o temporário raspado e copia esse mesmo arquivo ao destino. Regravar posteriormente o mapa vivo não altera essa entrada publicada. (`bin/relato.sh:100`, `bin/relato.sh:106–111`, `bin/relato.sh:158`) |
| **BLOQUEANTE — mapa retido** | Regrava a cópia pública anterior após raspagem, **sem passar pelo selo**, e permite commit/push dela. Entrada concreta: mapa vivo inválido e cópia pública contendo `E3 encerrada — 1 de 2`. Esse caminho pode conservar ou republicar o texto, com rc 1 ou 2; não é o contraexemplo de rc 0 acima. (`bin/relato.sh:116–125`, `bin/relato.sh:167–180`) |
| Raspagem nova falha ou não escreve | Falha explícita recebe 2; retorno 0 sem produzir o temporário chega ao selo como arquivo ilegível e também recebe 2. (`bin/relato.sh:106–113`; `bin/mapa20_valida.py:246–250`) |
| Remoção falha deixando o mapa presente | Aborta antes do commit/push com 2. Não retira o que já estava publicado remotamente. (`bin/relato.sh:123–124`, `bin/relato.sh:178–179`) |
| **FILA — fonte fora da fotografia** | O selo lê BACKLOG durante o julgamento; a publicação lê novamente o BACKLOG vivo depois. Inserir uma linha entre essas leituras desloca a referência publicada. A fotografia protege o mapa, não suas fontes. Ocorrência real [não verificado]. (`bin/mapa20_valida.py:100–105`; `bin/relato.sh:155`; `bin/raspar_publico.py:28`) |

**FILA — rc 2 não vence rc 1 em todos os caminhos.**

- `cp` falhando por permissão ou espaço recebe 1, como se o mapa não existisse. (`bin/relato.sh:100–101`)
- `cmp` com erro operacional também recebe 1, como divergência de conteúdo. (`bin/relato.sh:104–105`)
- Raspagem do retido falhando, seguida de remoção bem-sucedida, conserva o rc anterior; pode continuar sendo 1. O teste exige justamente esse resultado. (`bin/relato.sh:119–125`; `bin/tests/test_mapa20_valida.sh:309`)

O valor acumulado chega aos dois finais normais: “nada mudou” e depois do push. Entretanto, as classificações acima já podem estar erradas. (`bin/relato.sh:173`, `bin/relato.sh:180`)

O chamador operacional encontrado na cópia é `hasner-relato.service`, diretamente por `ExecStart`, como `oneshot`. A unit não declara tratamento distinto para 1 e 2 nem ação de recuperação; o timer apenas agenda novas execuções. Configuração efetivamente instalada e reação observada em produção [não verificado]. (`bin/hasner-relato.service:12–21`; `bin/hasner-relato.timer:5–8`)

3. **Cura dos achados do opus**

| Achado anterior | Veredito por classe |
|---|---|
| Invisíveis e homóglifos indicados | **Curado.** Alfabeto fechado recusa essas classes antes do parser. (`bin/mapa20_valida.py:78–79`, `bin/mapa20_valida.py:145–149`) |
| `encerrada` fora da tabela | **Curado quanto às posições citadas**: título, lista, negrito e rodapé entram na varredura global. **Não curado semanticamente**: outra obra pode ser declarada encerrada dentro de uma linha completa, como no contraexemplo. (`bin/mapa20_valida.py:213–228`) |
| `splitlines()` versus GFM | **Curado.** Há divisão somente em LF e rejeição dos demais separadores pelo alfabeto. (`bin/mapa20_valida.py:145–150`) |
| N inventado | **Não curado — BLOQUEANTE.** Mudou a categoria de caminho permitida; ainda aceita título do BACKLOG e comentário do TICKETS. (`bin/mapa20_valida.py:91–107`; `app/docs/BACKLOG.md:1`; `app/docs/TICKETS.md:1`) |
| Dois escritores do mapa vivo | **Cura não demonstrada.** O tripwire compara bytes, não autoria. Remoção do antigo `logs/MAPA-20/montar_mapa.py` e escritores vivos [não verificado]. O próprio código registra sua existência anterior fora do repositório. (`bin/relato.sh:104–105`; `bin/mapa20_montar.py:4–8`; `.gitignore:53`) |
| rc 1 sem consumidor específico | **Não curado — FILA.** A unit chama o script, mas não distingue as duas classes de falha. (`bin/hasner-relato.service:12–21`) |
| `SEM DONO` aceita encerramento | **Curado.** Apesar do `continue`, a varredura global posterior também alcança essa linha. (`bin/mapa20_valida.py:202–205`, `bin/mapa20_valida.py:225–228`) |
| X sem prova no RELATO | **Não curado — BLOQUEANTE.** O ramo numérico compara valores e verifica a referência de N; não lê provas de itens. (`bin/mapa20_valida.py:206–216`) |
| Testemunha LEI-AKITA ligada a `e59c25049` | Cura histórica e validade dessa testemunha [não verificado]. A declaração de escritor único no código não demonstra remoção do escritor anterior. (`bin/mapa20_montar.py:2–8`) |
| Raspagem depois do julgamento | **Curado no caminho novo; não no caminho retido.** (`bin/relato.sh:106–120`, `bin/relato.sh:158`) |
| Isenção de `status/encerrada.sim` | **Curado.** Caminhos não são mais removidos antes da busca; os testes agora exigem rejeição. (`bin/mapa20_valida.py:82–88`; `bin/tests/test_mapa20_valida.sh:145–152`) |
| Ambiguidade de rc 1 | **Não curada — FILA**, pelas falhas de `cp`, `cmp` e raspagem do retido já descritas. (`bin/relato.sh:100–125`) |

4. **Testes e guardas sem mordida** [nao verificado]

**FILA — célula `…` passa no teste pelo motivo errado.** Esse caso fica vermelho pelo alfabeto, antes da verificação de célula vazia. Ele não testemunha a guarda de preenchimento; outros casos de pontuação ainda exercitam essa guarda. (`bin/tests/test_mapa20_valida.sh:66–67`; `bin/mapa20_valida.py:145–149`, `bin/mapa20_valida.py:198–200`)

**FILA — “só escreve a saída pedida” compara apenas caminhos.** `arvore()` não captura conteúdo. Uma regravação de arquivo existente, como acrescentar espaço ao JSON, preservaria a lista de caminhos e escaparia dessa asserção. Mutante não executado [não verificado]. (`bin/tests/test_mapa20_valida.sh:354–355`, `bin/tests/test_mapa20_valida.sh:358–375`)

**BLOQUEANTE — testes verdes legitimam fonte inválida.** O título do BACKLOG é a fonte padrão, e o comentário inicial de TICKETS também é aceito como verde. Portanto, a suíte incorpora o significado errado de “fonte do N”. (`bin/tests/test_mapa20_valida.sh:33`, `bin/tests/test_mapa20_valida.sh:55–60`)

Os **156 casos de conteúdo** reproduzidos por stdin deram os resultados esperados, apesar dos contraexemplos novos. Isso não equivale à execução da suíte shell completa, que cria arquivos. (`bin/tests/test_mapa20_valida.sh:25–189`)

5. **Quem escreve, quem lê e significado dos campos**

| Estado | Escritor identificado | Leitor identificado |
|---|---|---|
| `logs/MAPA-20/mapa.md` | O montador escreve no `argv[0]`; chamada efetiva para o caminho vivo [não verificado]. (`bin/mapa20_montar.py:160–161`) | Fotografia e tripwire. (`bin/relato.sh:100–104`) |
| `montador_dados.json` | A instrução atribui edição à sessão; produtor vivo [não verificado]. A fixture escreve esse JSON. (`bin/mapa20_montar.py:7–8`; `bin/tests/test_mapa20_valida.sh:351`) | `json.loads` do montador. (`bin/mapa20_montar.py:89`) |
| `MAPA-20.md` temporário novo | Raspador. (`bin/raspar_publico.py:40`) | Selo; depois cópia ao destino. (`bin/relato.sh:109`, `bin/relato.sh:158`) |
| `MAPA-20.md` público | Cópia do temporário aprovado ou raspagem/rename do retido. (`bin/relato.sh:119–120`, `bin/relato.sh:158`) | Raspagem de retenção e Git. (`bin/relato.sh:119`, `bin/relato.sh:168–179`) |
| BACKLOG | Gerador preserva o bloco manual e substitui o arquivo; `backlog_estado.py` altera a célula de estado. Autor das listas históricas de PRONTO [não verificado]. (`bin/gerar_backlog.py:103–105`, `bin/gerar_backlog.py:138–140`; `bin/backlog_estado.py:58–59`) | Montador localiza chave; selo abre linha citada; publicador raspa o arquivo. (`bin/mapa20_montar.py:58–61`; `bin/mapa20_valida.py:100–105`; `bin/relato.sh:155`) |
| TICKETS | Escritores de topo e rodapé; há também molde de inserção de linha. Autor das listas históricas de PRONTO [não verificado]. (`bin/tickets_placar.sh:82`; `bin/tickets_rodape.sh:78–79`; `bin/molde_autocura/ticket_rows.py:10–12`) | Selo abre a linha citada. (`bin/mapa20_valida.py:100–105`) |

Todos os campos abaixo vêm do JSON; seu escritor operacional permanece [não verificado]. Os consumidores são:

| Campo | Uso efetivo |
|---|---|
| `cabeca` | Prosa inicial. (`bin/mapa20_montar.py:138`) |
| `censo`, `alias_censo` | Leitura das linhas e resolução de nomes. (`bin/mapa20_montar.py:90–105`) |
| `ovr` | Sobrescrita por índice, inclusive identidade. (`bin/mapa20_montar.py:117–118`) |
| `xn[obra]` | Tripla textual X/N, fonte, falta; expansão de referência BACKLOG, sem derivar N. (`bin/mapa20_montar.py:123–124`) |
| `fonte_ausente`, `falta_ausente` | Células das obras sem `xn`. (`bin/mapa20_montar.py:127`) |
| `rascunho.arquivo`, `alias`, `origem` | Contagem do rascunho e anotação na lista superior. (`bin/mapa20_montar.py:65–84`, `bin/mapa20_montar.py:106`, `bin/mapa20_montar.py:132`) |
| Censo colunas 1–8 | Arquivos, cruzamentos, leis, dependência, movimentos, VISIVEL e voltas, conservados após sobrescritas. (`bin/mapa20_valida.py:53–55`; `bin/mapa20_montar.py:117–120`) |
| Censo coluna 9 | Etapa histórica, enviada à prosa; na tabela final, coluna 9 passa a receber X/N. (`bin/mapa20_montar.py:119–124`) |

**BLOQUEANTE — os consumidores não preservam a mesma identidade.** Esta entrada troca os rótulos após a canonicalização, mas continua buscando `xn` pelo nome anterior:

```json
"ovr": {
  "E3": {"0": "E4B"},
  "E4B": {"0": "E3"}
}
```

Com `xn.E3 = 1 de 1` e `xn.E4B = 1 de 2`, o montador produz E4B com dados e progresso de E3, e vice-versa. A sonda em memória do montador real seguida do selo retornou **rc 0**. (`bin/mapa20_montar.py:116–124`; `bin/mapa20_valida.py:188–214`)

O montador ser o **único escritor vivo** continua [não verificado]. `cmp` comprova igualdade naquela comparação; outro escritor produzindo os mesmos bytes não é identificado. (`bin/relato.sh:104–105`)

6. **Classes reincidentes**

| Classe | Resultado |
|---|---|
| Segundo escritor | Existência atual [não verificado]; unicidade não demonstrada pelo tripwire. (`bin/mapa20_montar.py:4–8`; `bin/relato.sh:104–105`) |
| Derivação paralela de fonte única | **BLOQUEANTE:** N declarado no JSON não é derivado da lista citada; apenas o N do rascunho é contado. (`bin/mapa20_montar.py:74–84`, `bin/mapa20_montar.py:123–124`) |
| Selo sem mordida | **BLOQUEANTE:** não exige prova de X nem fundamento quantitativo de N. (`bin/mapa20_valida.py:91–107`, `bin/mapa20_valida.py:209–216`) |
| Relógio fora da borda | **FILA:** fonte e mapa não compartilham fotografia até a publicação. Ocorrência produtiva [não verificado]. (`bin/mapa20_valida.py:100`; `bin/relato.sh:155`) |
| Campo com significados divergentes | **BLOQUEANTE:** `ovr[0]` muda a identidade exibida sem mudar a chave usada para selecionar progresso. (`bin/mapa20_montar.py:116–124`) |

## 11/11 14:33 · sessao a6c6e22ba20e · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — N sem fonte válida, rc 0.** Sonda real, somente memória: todas as obras com `1 de 1`, fonte `app/docs/BACKLOG.md:1`, falta `encerrada`; demais campos `nao medido`, SEM DONO canônica, legenda e cabeçalho oficiais. Resultado: `rc 0`, zero problemas. A fonte é apenas o título do BACKLOG, não PRONTO numerado (`app/docs/BACKLOG.md:1`). O selo confere caminho permitido e linha não vazia, sem relacionar obra, lista e N (`bin/mapa20_valida.py:91-107`). O teste legitima esse título como fonte (`bin/tests/test_mapa20_valida.sh:34`).

- **BLOQUEANTE — “E4B: encerrada” com E4B em 1 de 2, rc 0.** Entrada completa: E3 em `1 de 1`, coluna “arquivos que toca” com `E4B: encerrada`; E4B em `1 de 2`, falta `item 2`; outras linhas canônicas. Sonda real retornou `rc 0`, zero problemas. A permissão acompanha a linha E3, sem identificar a obra afirmada no texto (`bin/mapa20_valida.py:213-214`, `bin/mapa20_valida.py:225-228`).

- **BLOQUEANTE — X não confrontado com prova.** O ramo numérico confere intervalo, fonte e igualdade X/N; não consulta RELATO nem verifica itens comprovados (`bin/mapa20_valida.py:206-216`). A primeira entrada também demonstra aceitação sem apresentar prova de item algum.

- **FILA — caso sem mordida própria.** O caso `celula …` rejeita U+2026 pelo alfabeto antes da guarda de célula vazia (`bin/tests/test_mapa20_valida.sh:70`, `bin/mapa20_valida.py:146-149`). Mutante em memória que faz `_DADO.search` sempre retornar verdadeiro mantém esse caso vermelho; portanto ele não prova a guarda de `bin/mapa20_valida.py:198-200`.

- **Curados:** invisíveis/homóglifos indicados, pelo alfabeto fechado (`bin/mapa20_valida.py:78-79`); separadores de `splitlines`, pelo alfabeto e divisão somente em LF (`bin/mapa20_valida.py:145-150`); “encerrada” em SEM DONO, pela varredura global posterior ao `continue` (`bin/mapa20_valida.py:202-205`, `bin/mapa20_valida.py:225-228`).

Reproduzi os 156 casos de conteúdo construídos pela parte (a), usando `/dev/stdin`: todos deram o resultado esperado. A suíte permanece verde diante dos dois contraexemplos acima; construção e expectativas estão em `bin/tests/test_mapa20_valida.sh:25-189`.

## 11/11 14:33 · sessao 2a97dd5b67c4 · - · - · origem=Ronald (tmux)

BLOQUEANTE — N continua sem derivação da lista de PRONTO: o N aprovado vem da string `d['xn'][nome]`, copiada em `bin/mapa20_montar.py:123-124`. A contagem em `bin/mapa20_montar.py:74-84` aplica-se apenas ao RASCUNHO. O selo aceita caminho permitido + linha não vazia, sem verificar obra, lista numerada ou quantidade (`bin/mapa20_valida.py:91-107`). `app/docs/BACKLOG.md:1` é título; `app/docs/TICKETS.md:1` é comentário. Ambos são aceitos como fontes verdes por `bin/tests/test_mapa20_valida.sh:31` e `bin/tests/test_mapa20_valida.sh:66`. X não lê provas do RELATO: sua única utilização efetiva é comparação aritmética (`bin/mapa20_valida.py:209-215`).

BLOQUEANTE — `ovr` pode trocar a identidade da obra depois da canonicalização: `bin/mapa20_montar.py:116-118`. O `xn` continua escolhido pelo nome anterior (`bin/mapa20_montar.py:123-124`). Sonda em memória confirmou zero problemas para:

```json
"ovr": {
  "E3": {"0": "E4B"},
  "E4B": {"0": "E3"}
}
```

Com censo E3=`dado E3`, E4B=`dado E4B`, `xn.E3=["1 de 1","app/docs/BACKLOG.md:1","nada"]` e `xn.E4B=["1 de 2","app/docs/BACKLOG.md:1","item 2"]`, o montador produz E4B com dados/progresso de E3 e vice-versa; `problemas()` retorna `[]` (`bin/mapa20_montar.py:110`, `bin/mapa20_montar.py:117`, `bin/mapa20_montar.py:124`; execução somente em memória).

FILA — teste “só escreve saída pedida” compara exclusivamente nomes de arquivos: `arvore()` em `bin/tests/test_mapa20_valida.sh:354-355`, captura em `:358`, comparação em `:374`. Mutante concreto após `bin/mapa20_montar.py:152`:

```python
open(os.path.join(RAIZ, DADOS), 'a', encoding='utf-8').write(' ')
```

Modifica JSON existente, preserva JSON válido, saída e lista de caminhos; essa asserção não morde. Mutante não executado [não verificado].

Rastreio de estado:

| Estado/campo | Escritor | Leitor |
|---|---|---|
| `logs/MAPA-20/mapa.md` | Montador grava o `argv[0]` arbitrário, `bin/mapa20_montar.py:160-161`; chamada real para caminho vivo [não verificado] | Foto em `bin/relato.sh:100`; comparação em `:104` |
| `montador_dados.json` | “Sessão edita” é instrução em `bin/mapa20_montar.py:7-8`; escritor real [não verificado]. Teste escreve em `bin/tests/test_mapa20_valida.sh:351` | JSON em `bin/mapa20_montar.py:89` |
| `cabeca` | JSON acima | `bin/mapa20_montar.py:138` |
| `censo` | Prod [não verificado]; fixture `bin/tests/test_mapa20_valida.sh:341` | `bin/mapa20_montar.py:90`, `:93-103` |
| `alias_censo` | JSON | `bin/mapa20_montar.py:104-105`, `:135` |
| `ovr` | JSON | Índices arbitrários em `bin/mapa20_montar.py:117-118` |
| Censo colunas 0–8 | Censo, sobrescritas JSON | Nome em `bin/mapa20_montar.py:116`; demais conservadas por `:120` |
| Censo coluna 9 | Censo/`ovr["9"]` | Etapa em `bin/mapa20_montar.py:119`; removida em `:120` |
| Saída coluna 9 | `xn[0]` | Montador `bin/mapa20_montar.py:124`; selo `bin/mapa20_valida.py:201` |
| `xn` = XdeN/fonte/falta | JSON | Copiado em `bin/mapa20_montar.py:123-124`; selo `bin/mapa20_valida.py:206-218` |
| `fonte_ausente`, `falta_ausente` | JSON | `bin/mapa20_montar.py:127` |
| `rascunho.arquivo/alias/origem` | Prod [não verificado]; fixture `bin/tests/test_mapa20_valida.sh:342-348` | `bin/mapa20_montar.py:106`, `:132` |
| BACKLOG | Gerador `bin/gerar_backlog.py:138-140`, preservando bloco manual `:103-105`; estado em `bin/backlog_estado.py:58-59` | Localiza chave em `bin/mapa20_montar.py:58-61`; fonte em `bin/mapa20_valida.py:100-105`; publicador `bin/relato.sh:155` |
| TICKETS | Topo `bin/tickets_placar.sh:82`; rodapé `bin/tickets_rodape.sh:78-79`; template de linha `bin/molde_autocura/ticket_rows.py:10-12`; autores históricos das listas PRONTO [não verificado] | Fonte em `bin/mapa20_valida.py:100-105`; placa `bin/tickets_placar.sh:64`; contrato `bin/regua_tickets.sh:78` |
| `MAPA-20.md` público | Raspador `bin/raspar_publico.py:40`; regravação retido `bin/relato.sh:119-120`; cópia julgada `:158` | Selo `bin/relato.sh:109`; Git `:168`, `:178` |

Segundo escritor antigo efetivamente removido: [não verificado]. `logs/` não existe nesta cópia; é ignorado em `.gitignore:53`. O código apenas relata a existência anterior em `bin/mapa20_montar.py:4-5`. O tripwire detecta diferença de bytes (`bin/relato.sh:104-105`), não autoria: outro escritor que produza bytes iguais não é identificado por essa comparação.

Classes presentes: derivação paralela N declarado/N contado (`bin/mapa20_montar.py:80`, `:124`), selo sem prova de X/N (`bin/mapa20_valida.py:91-107`, `:209-215`) e identidade com dois consumidores divergentes por `ovr[0]` (`bin/mapa20_montar.py:118`, `:124`). Segundo escritor real e relógio produtivo [não verificado].

## 11/11 14:32 · sessao 0396d9139208 · - · - · origem=Ronald (tmux)

Achados do publicador, por leitura do código; execução em produção [não verificado]. [nao verificado]

- **BLOQUEANTE — bytes publicados sem julgamento no caminho retido.** O caminho novo raspa, julga e copia o mesmo temporário (`bin/relato.sh:106`, `:109`, `:111`, `:158`). Porém, quando retido, o documento público anterior passa apenas pela raspagem e por `mv`, sem selo (`bin/relato.sh:116`, `:119`, `:120`), e entra no commit/push (`bin/relato.sh:167`, `:168`, `:178`, `:179`). Entrada concreta: mapa vivo inválido e `DEST/MAPA-20.md` contendo `E3 encerrada — 1 de 2`; o segundo permanece/republica com rc 1, sem julgamento. O teste aceita até `MAPA VELHO`, sem tabela (`bin/tests/test_mapa20_valida.sh:275`, `:306`, `:307`).

- **FILA — classificação rc 1/2 ainda ambígua.** Falha operacional de `cp`, como EACCES/ENOSPC, vira rc 1 (`bin/relato.sh:100`, `:101`); erro operacional de `cmp` também vira rc 1 (`bin/relato.sh:104`, `:105`). Raspagem do retido falha, remoção funciona e conserva rc 1 (`bin/relato.sh:119`, `:123`, `:125`, `:180`), embora a declaração prometa rc 2 para falha de raspagem e precedência de 2 (`bin/relato.sh:96`, `:97`). O teste explicitamente legitima esse último caso: `raspa_falha … esperado 1` (`bin/tests/test_mapa20_valida.sh:309`).

- **FILA — rc chega, mas não há tratamento específico no chamador da cópia.** O retorno acumulado aparece tanto sem alteração quanto após push (`bin/relato.sh:173`, `:180`); falha de remoção que deixa o mapa presente aborta antes do push com 2 (`bin/relato.sh:123`, `:124`). A unit chama diretamente o script, como oneshot (`bin/hasner-relato.service:13`, `:15`); não declara diferenciação de 1 versus 2, alerta ou leitor que execute reação (`bin/hasner-relato.service:12`). O timer volta periodicamente (`bin/hasner-relato.timer:5`, `:6`); unit/timer vivos [não verificado].

- **FILA — fontes não compartilham a fotografia do mapa.** O selo abre a fonte citada durante julgamento (`bin/mapa20_valida.py:100`, `:101`); depois o publicador manda raspar novamente o BACKLOG vivo (`bin/relato.sh:155`; leitura em `bin/raspar_publico.py:28`). Entrada temporal concreta: inserir uma linha no BACKLOG entre esses passos muda a linha publicada, preservando a citação anteriormente aprovada. Ocorrência real [não verificado].

- **Curado no caminho novo:** regravar o mapa vivo depois do selo não muda o temporário publicado (`bin/relato.sh:100`, `:109`, `:158`); há teste que regrava a origem após o selo e cobra publicação da fotografia (`bin/tests/test_mapa20_valida.sh:244`, `:245`, `:311`). Raspador que retorna 0 sem escrever deixa o arquivo de julgamento ausente, conduzindo a rc 2 (`bin/relato.sh:109`, `:113`; `bin/tests/test_mapa20_valida.sh:262`, `:263`, `:317`). Raspagem que estraga a célula é julgada e retida (`bin/tests/test_mapa20_valida.sh:268`, `:269`, `:318`).

- **Limite da corrida:** o lock de publicação é comum ao produtor de CODEX-ESTADO no caminho de evento (`bin/relato.sh:27`; `bin/codex_estado.sh:65`, `:66`). Isso não fotografa BACKLOG/fontes e não verifica novamente o destino entre `cp`, `git add` e `git commit` (`bin/relato.sh:158`, `:168`, `:178`); outro escritor concreto do MAPA-20 público [não verificado].

## 11/11 14:30 · sessao c243052975dc · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**NÃO PROCEDE declarar o caminho inteiro fechado.** A cura limita `/hora/` e corrige a prova do aviso/controle dos mutantes; permanecem esperas anteriores à fila e ausência de uma identidade estável para o reenvio. (`app/static/pwa.js:93`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/static/pwa.js:48`)

Chromium, mortes efetivamente executadas, smoke e efeitos em produção: **[nao verificado]**. Não executei o navegador: seu executor cria arquivos e perfil temporários. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:279`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:286`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:295`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:298`)

1. **Fechamento de cada achado da volta 2, por inspeção.**

Na tabela, o veredito qualifica **o fechamento** do achado.

| Achado da volta 2 | Veredito, cura e teste correspondente | [nao verificado]
|---|---|
| Triagem offline escondia confirmar quando a câmera falhava | **PROCEDE.** Botão mostrado antes da abertura e preservado sem stream; M1 reinjeta a ocultação. (`app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:262`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:550`) |
| Triagem online presa em câmera pendente | **PROCEDE.** Abertura limitada, seguida da liberação sem stream; matriz inclui câmera pendente, M3 retira a liberação e Md retira o teto. (`app/static/pwa.js:159`, `app/static/pwa.js:162`; `app/templates/ponto/triagem/apto.html:164`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:55`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:560`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:626`) |
| Triagem online dependia exclusivamente de `toBlob` | **PROCEDE.** Captura central limitada, aguardada antes do submit; matriz e teste direto cobrem callback mudo/conversão falha. (`app/templates/ponto/triagem/apto.html:177`, `app/templates/ponto/triagem/apto.html:231`; `app/static/pwa.js:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:59`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`) |
| Triagem offline enfileirava somente dentro do callback | **PROCEDE.** Captura termina antes da chamada independente ao escritor; M4 reinjeta callback sem teto. (`app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:565`) |
| V2 vetava foto nula | **PROCEDE.** Requisitos avisam e prosseguem; M2 reinjeta o veto. (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:298`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:555`) |
| V2 pendurava com `toBlob` mudo e `toDataURL` lançando | **PROCEDE.** Exceção do fallback é absorvida pelo teto; cenário específico e Md cobrem o contrato. (`app/static/pwa.js:138`, `app/static/pwa.js:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:60`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:626`) |
| Barreiras no template legado | **PROCEDE por eliminação.** Seleção atual usa V2/triagem; M6 permanece apenas como histórico, sem ensaio comportamental próprio. (`app/ponto/views.py:356`, `app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:524`) |
| Captura/espera duplicadas nas telas | **PROCEDE para a operação da câmera.** A operação fica no PWA; censo exige lista fechada de produtores. Os textos próprios continuam, abaixo. (`app/static/pwa.js:157`, `app/static/pwa.js:168`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:641`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:661`) |
| Mutante com `await` infinito em requisitos | **PROCEDE quanto à prova prevista.** Ma injeta exatamente a espera; agora passa por controle acoplado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:595`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:545`) |
| Mutante sem aviso | **PROCEDE quanto à prova prevista.** Mb troca o aviso por `false` e exige sua falta na tela. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:600`) |
| `throw` no ramo sem foto | **PROCEDE quanto à prova prevista.** Mc reinjeta a exceção e exige ausência de envio. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:605`) |
| PWA não carregou | **NÃO PROCEDE o fechamento offline.** Ambas as telas dependem do escritor ausente; mostram erro sem gravação alternativa. O teste sem PWA continua somente online. (`app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:332`, `app/templates/ponto/bater_ponto_v2.html:338`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:465`) |
| `estadoPermissao` ainda participava do fluxo | **PROCEDE a retirada do fluxo examinado.** Requisitos consultam captura e GPS diretamente. (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:279`) |
| Transporte incompleto de timestamp/GPS | **NÃO PROCEDE.** Sync continua omitindo timestamp corrigido, gap, precisão e instante GPS. (`app/static/pwa.js:188`, `app/static/pwa.js:191`, `app/static/pwa.js:217`, `app/static/pwa.js:226`) |
| Textos de obrigatoriedade contraditórios | **NÃO PROCEDE.** Permanecem na triagem e na V2; os testes procuram o aviso, sem proibir essas mensagens contraditórias. (`app/templates/ponto/triagem/apto.html:66`, `app/templates/ponto/triagem/apto.html:124`, `app/templates/ponto/triagem/apto.html:194`, `app/templates/ponto/triagem/apto.html:250`; `app/templates/ponto/bater_ponto_v2.html:100`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:338`) |
| B1: `/hora/` pendurada antes do `add` | **PROCEDE o fechamento específico da espera HTTP/corpo.** `_noTeto` envolve ambos; FilaRealTest e Me exercitam a fila pelo escritor real. **Não fecha a espera total**, pois IndexedDB permanece sem teto. (`app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:113`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:501`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:610`) |
| B2: aviso invisível em atributo | **PROCEDE.** V2 acrescenta texto; oráculo usa `innerText`; Mf recoloca somente `title`. (`app/templates/ponto/bater_ponto_v2.html:250`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:621`) |
| B2: mutante morto sem controle verde acoplado | **PROCEDE.** `par` junta controle/mutante, exige controle verde/não vazio e exclui sonda sem veredito das mortes. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:451`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:453`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:455`) |

2. **PROCEDE a existência de caminhos sem teto antes da gravação.**

| Caminho | O que acontece antes da fila |
|---|---|
| Triagem online | Clique desabilita botão → GPS com timer de 8 s → selfie limitada → espera de 400 ms → submit nativo. Esse ramo **não possui fallback para a fila** se o envio falhar ou ficar pendente. (`app/templates/ponto/triagem/apto.html:212`, `app/templates/ponto/triagem/apto.html:218`, `app/templates/ponto/triagem/apto.html:231`, `app/templates/ponto/triagem/apto.html:240`, `app/templates/ponto/triagem/apto.html:241`) |
| Triagem offline | Primeiro clique abre confirmação; segundo clique → captura limitada → escritor da fila. Falha de câmera não impede essa chamada; falha do escritor mostra erro, sem gravação alternativa. (`app/templates/ponto/triagem/apto.html:243`, `app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:308`) |
| V2 offline | Clique → selfie limitada → GPS com tentativas de 10 e 12 s → escritor. Esses timers de GPS são próprios, além do teto de selfie/hora. (`app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:280`, `app/templates/ponto/bater_ponto_v2.html:281`, `app/templates/ponto/bater_ponto_v2.html:332`) |
| V2 online | Após requisitos, espera `fetch` **sem teto**. Somente rejeição chega ao fallback da fila. HTTP de erro espera `r.json()` também sem teto e, depois, retorna sem enfileirar. (`app/templates/ponto/bater_ponto_v2.html:298`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:314`, `app/templates/ponto/bater_ponto_v2.html:316`, `app/templates/ponto/bater_ponto_v2.html:319`) |
| Escritor PWA | `obterGap` → leitura de config → possível medição limitada → leitura/gravação de config → nova abertura do banco → `add` → `tx.oncomplete`. O teto HTTP não engloba essas operações de armazenamento. (`app/static/pwa.js:104`, `app/static/pwa.js:108`, `app/static/pwa.js:113`, `app/static/pwa.js:182`, `app/static/pwa.js:184`, `app/static/pwa.js:194`) |

**`abrirDB` continua sendo uma promessa potencialmente pendente:** resolve em `onsuccess`, rejeita em `onerror`; não possui `onblocked`, teto ou fechamento das conexões abertas. Se a abertura não chegar a nenhum desses dois eventos, `obterGap` já fica esperando antes de alcançar o teto HTTP. A transação da fila também não possui teto nem tratamento explícito de `onabort`. Ocorrência real dessas condições: **[nao verificado]**. (`app/static/pwa.js:48`, `app/static/pwa.js:58`, `app/static/pwa.js:59`, `app/static/pwa.js:74`, `app/static/pwa.js:113`, `app/static/pwa.js:185`, `app/static/pwa.js:195`)

**POST recebido, resposta perdida:**

- Se o `fetch` permanecer pendente, **não entra na fila**; o botão permanece desabilitado. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:295`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:317`)
- Se acabar rejeitando, **tenta entrar na fila**, com timestamp produzido naquele momento. O POST inicial não leva timestamp nem ID estável do clique. (`app/templates/ponto/bater_ponto_v2.html:102`, `app/templates/ponto/bater_ponto_v2.html:304`, `app/templates/ponto/bater_ponto_v2.html:319`; `app/static/pwa.js:182`, `app/static/pwa.js:189`)
- **Não procede “sempre duplica” nem “nunca duplica”.** As APIs deduplicam por **colaborador + tipo + timestamp efetivo em ±5 minutos**. A porta acrescenta mesmo tipo em ±60 segundos e igualdade exata **colaborador + timestamp**; essa última também é a constraint do modelo. Nenhuma é chave do clique original. (`app/api/views.py:539`, `app/api/views.py:542`, `app/api/views_core.py:526`, `app/api/views_core.py:529`; `app/ponto/registro_batida.py:106`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`; `app/ponto/models.py:106`)

Logo, demora antes da rejeição ou relógio desencontrado pode colocar o novo instante fora dessas janelas; o mesmo evento deixa de ser identificado pelo dedup. Duplicação observada em produção: **[nao verificado]**. (`app/static/pwa.js:183`, `app/static/pwa.js:189`; `app/api/views.py:539`; `app/ponto/registro_batida.py:120`)

3. **NÃO PROCEDE confundir os tetos com o gap; PROCEDE a diferença no instante gravado.**

| Valor | Significado, escritor e leitores |
|---|---|
| `HORA_TETO_MS` | Duração de **2000 ms**, declarada uma vez; `calcularESalvarGap` a passa para `_noTeto`, abrangendo fetch e corpo. (`app/static/pwa.js:88`, `app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:98`) |
| `SELFIE_TETO_MS` | Duração **base de 1500 ms**, declarada uma vez; captura lê diretamente e abertura lê multiplicada por dois. Portanto, não é um único teto absoluto para as duas operações. (`app/static/pwa.js:129`, `app/static/pwa.js:162`, `app/static/pwa.js:177`) |
| `timestamp_gap_ms` / `ntp_gap` | Offset assinado em milissegundos: servidor menos estimativa do relógio local. Não é duração de espera nem horário com fuso. (`app/static/pwa.js:101`, `app/static/pwa.js:102`, `app/static/pwa.js:103`, `app/static/pwa.js:190`; `app/ponto/models.py:76`) |

**No teto não se guarda um novo gap:** o código lê o anterior ou usa zero; também não renova `ntp_calculado_em`. Esse é o mesmo fallback da rejeição imediata de rede. Entretanto, `Date.now()` da batida só é lido **depois** dessa espera: `/hora/` pendurada desloca o instante amostrado pelo tempo esperado, aproximadamente 2 s no modelo dos timers. Não há identidade com o instante do clique nem com a execução que rejeitou imediatamente. (`app/static/pwa.js:98`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:182`, `app/static/pwa.js:183`)

**NÃO PROCEDE atribuir outra unidade/fuso ao servidor nesse caminho:** o sincronizador sequer transmite o gap ou o timestamp corrigido. Usa `b.timestamp` para ordenar e envia `timestamp_dispositivo` bruto. A API usada é `/api/ponto/bater/`; ambas as cascas entregam esse bruto a `hora_do_aparelho`. O endpoint offline antigo retorna 410. (`app/static/pwa.js:211`, `app/static/pwa.js:221`, `app/static/pwa.js:227`; `app/api/views.py:518`, `app/api/views_core.py:505`; `app/ponto/views.py:419`, `app/ponto/views.py:426`)

`hora_do_aparelho` aceita ISO ou epoch segundos/ms e decide entre instante bruto e chegada. Os leitores encontrados de `timestamp_gap_ms` mantêm milissegundos: modelo, tela e alerta de skew do **ping geo**, sem aplicação de fuso ao offset. O problema aqui é **transporte omitido**, não conversão nova de unidade. (`app/api/hora_do_aparelho.py:79`, `app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`; `app/ponto/models.py:76`; `app/templates/ponto/batidas_offline_pendentes.html:79`, `app/templates/ponto/batidas_offline_pendentes.html:80`; `app/api/views.py:2340`, `app/api/views.py:2371`)

4. **PROCEDE a mordida específica dos novos testes; NÃO PROCEDE tratá-los como prova de persistência real.** [nao verificado]

| Prova | Avaliação |
|---|---|
| FilaRealTest | Usa o escritor real e testa hora normal, fetch pendente e corpo pendente. Corrige a substituição que escondia B1. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:214`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:437`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:477`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:501`) |
| Me | `1e9` impede o teto dentro dos 15 s do ensaio; exige ausência de fila diretamente e nas duas telas offline, com controle verde acoplado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:439`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:610`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:616`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:618`) |
| Stub que ignora abort | Não dá passagem indevida: obriga `_noTeto` a resolver independentemente do abort. A promise HTTP original pode continuar pendente sem segurar o chamador limitado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:116`; `app/static/pwa.js:138`, `app/static/pwa.js:140`) |
| Mf | `title` não satisfaz `innerText`. Para câmera rejeitada, o aviso precisa estar presente antes do clique; o toast posterior não salva o mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:190`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:342`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:621`) |
| `_Base.par` | Controle e mutante são processos separados submetidos ao mesmo lote; controle vermelho impede declarar morte, e sonda sem veredito é filtrada. Promessa pendente **com veredito da sonda** continua sendo falha válida. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:298`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:318`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:351`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:455`) |

**Há um caso que o selo aceita sem provar a conclusão:** o fake marca fila dentro do `add`, antes de `tx.oncomplete`; `falhas_da_fila` não exige “fila resolveu”. Um escritor pendurado depois desse `add` satisfaria o oráculo, embora a função real ainda estivesse aguardando conclusão. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:135`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:151`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:215`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:484`; `app/static/pwa.js:194`)

O fake sempre abre com sucesso e não reproduz bloqueio, aborto ou rollback; os timers rodam em tempo virtual. O caso online novo injeta rejeição imediata, sem representar POST pendurado ou servidor que gravou antes da rejeição. Persistência nativa e comportamento em tempo real: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:109`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:160`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:173`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:299`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:506`)

A ressalva do frame “ok” permanece: stream vazio, dimensões falsificadas e `drawImage` real. Se isso falhar, agora o controle vermelho impede uma falsa morte; sucesso efetivo desse controle: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:80`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:87`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:452`; `app/static/pwa.js:172`)

5. **Escritores, leitores e autoridades.**

Além dos dois tetos descritos no item 3, os contratos relevantes são:

| Campo/valor | Quem escreve → quem lê |
|---|---|
| `/hora/`: `hora`, `data`, `timestamp` | `hora_servidor` produz strings de apresentação e ISO com offset → PWA usa timestamp para epoch-ms; relógios das telas usam hora/data. (`app/core/views.py:14`, `app/core/views.py:20`; `app/static/pwa.js:102`; `app/templates/ponto/bater_ponto_v2.html:169`; `app/templates/ponto/triagem/base_triagem.html:46`) |
| `t0`, `t1`, `medida`, `latencia`, `gap` | Medição PWA captura relógios/corpo e deriva um offset → grava config e devolve ao escritor. (`app/static/pwa.js:94`, `app/static/pwa.js:96`, `app/static/pwa.js:97`, `app/static/pwa.js:101`, `app/static/pwa.js:104`, `app/static/pwa.js:182`) |
| `ntp_gap`, `ntp_calculado_em` | `calcularESalvarGap`, via `salvarConfig`, escreve → `obterGap` lê validade de 30 min; fallback lê gap anterior. (`app/static/pwa.js:67`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:113`) |
| `{stream, aviso}` / `{foto, aviso}` | Operação central e `_respostaSelfie` produzem → triagem/V2 leem controles, foto e aviso. (`app/static/pwa.js:145`, `app/static/pwa.js:163`, `app/static/pwa.js:178`; `app/templates/ponto/triagem/apto.html:155`, `app/templates/ponto/triagem/apto.html:233`; `app/templates/ponto/bater_ponto_v2.html:250`, `app/templates/ponto/bater_ponto_v2.html:275`) |
| `_fotoBlob` / foto anterior | Input V2 e captura escrevem → captura preserva anterior; POST/fila transportam; sync anexa somente se presente. (`app/templates/ponto/bater_ponto_v2.html:255`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:306`, `app/templates/ponto/bater_ponto_v2.html:332`; `app/static/pwa.js:178`, `app/static/pwa.js:226`) |
| Registro IndexedDB | `registrarBatidaOffline` escreve tipo, horários, gap, coordenadas, precisão, instante GPS e foto → sync lê; horário corrigido apenas ordena, e três metadados não são transportados. (`app/static/pwa.js:187`, `app/static/pwa.js:211`, `app/static/pwa.js:217`, `app/static/pwa.js:226`) |
| `id` da fila | IndexedDB gera por autoincremento → sync usa para apagar localmente; não vira ID enviado ao servidor. (`app/static/pwa.js:54`, `app/static/pwa.js:217`, `app/static/pwa.js:236`) |
| `C.hora`, `C.post`, `C.fim` | Construtor de cenário escreve → fetch fake seleciona resposta/rejeição e timer emite veredito. (`app/ponto/tests/test_se

[truncado em 20000 de 24229 caracteres]

## 11/11 14:29 · sessao 9ba71d54c610 · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE — alternativa (a), pela leitura literal de que o RED deve evidenciar o defeito pelo selo definido no aval.** O aval exige painel renderizado × API para o mesmo colaborador; a PROPORCAO mantém “RED e GREEN”; a LEI-AKITA 5 determina: “Cura sem teste vermelho evidenciado nao existe”. (`aval_ui_painel_raspado.txt:10–11`; `aval_proporcao_por_risco_1111.txt:1`, item 2; `CLAUDE.md:28–29`). [nao verificado]

- **Não há evidência fornecida do ORM vermelho pelo defeito da obra antes da cura.** A docstring registra cinco falhas **em memória**, mas declara que esse selo não certifica ORM e que o “RED visual/ORM antes da cura NAO [foi] executado nesta copia”. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:3–11`).
- O único log ORM mostra quatro falhas por **“fixture invisivel”**, inclusive com mutante. No código, essa asserção está no `setUp`, antes da instalação do mutante e da renderização; o comentário atribui a ela os quatro RED v1. Isso evidencia falha de preparação, não ausência do rótulo/lista. (`v1_orm_falhas.txt:13–31`; `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:58–61`, `:93–105`).
- A L-006 explicita o precedente: “RED que nao se le nao vale como evidencia (LEI-AKITA 5)”, com reconstrução contra o código anterior. (`app/docs/LEIS.md:64`).
- O corpo do commit `984ec6938` ficou **[não verificado]**: `git log` não pôde consultá-lo nesta cópia sem repositório Git. **[não verificado]**

**A alternativa (b) não cabe nessa leitura:** a fila cobre “como o selo poderia ser enganado”; aqui falta comprovar o RED obrigatório do selo contratado. O vermelho em memória não o substitui, conforme a própria docstring. (`aval_proporcao_por_risco_1111.txt:1`, item 2; `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:3–4`).

*Referências aos anexos contam linhas desde a primeira linha do conteúdo fornecido.*

## 11/11 14:25 · sessao 0be98c4a67e3 · - · - · origem=Ronald (tmux)

**Servidor, timestamps e reenvio**

- **PROCEDE: POST online pode ficar indefinidamente antes da fila.** V2 espera `fetch` sem teto; só entra na fila no `catch`. Resposta pendente não executa o `catch`, e `_enviando`/botão permanecem bloqueados. HTTP de erro vai para `r.json()`, também sem teto, e depois retorna sem enfileirar. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:314`, `app/templates/ponto/bater_ponto_v2.html:317`)
- **Se POST recebido terminar em rejeição no cliente**, V2 enfileira; se apenas permanecer pendente, não enfileira. A fila calcula novo instante depois de `obterGap`; o POST inicial não carrega instante nem identificador estável do evento: o formulário tem tipo/coordenadas/foto e o envio só acrescenta precisão/foto. Portanto, a mesma tentativa não preserva uma chave de idempotência de ponta a ponta. (`app/templates/ponto/bater_ponto_v2.html:102`, `app/templates/ponto/bater_ponto_v2.html:304`, `app/templates/ponto/bater_ponto_v2.html:319`, `app/static/pwa.js:182`)
- **Não é correto dizer “sempre duplica” ou “nunca duplica”.** O dedup API procura `(colaborador, tipo)` com timestamp em ±5 minutos do instante efetivo e retorna `ja_registrada`; as duas cascas fazem isso. É proximidade temporal, não igualdade por ID do clique. Se a nova hora cair fora da janela — por demora antes de rejeitar ou relógio bruto desencontrado — essa primeira barreira não identifica a mesma tentativa. Incidência real: **[nao verificado]**. (`app/api/views.py:539`, `app/api/views.py:541`, `app/api/views.py:549`, `app/api/views_core.py:526`, `app/api/views_core.py:528`; `app/static/pwa.js:183`, `app/static/pwa.js:189`)
- A porta oferece mais duas barreiras: mesmo colaborador/tipo não retratado em ±60 s; depois igualdade exata `(colaborador, timestamp)`, inclusive outro tipo ou retratada. Colisão de tipo oposto usa 60 ou 300 s, salvo marco próprio; a constraint do modelo é `(colaborador, timestamp)`. Nenhuma dessas chaves é o ID do clique original. (`app/ponto/registro_batida.py:21`, `app/ponto/registro_batida.py:106`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:138`, `app/ponto/models.py:106`)

**Gap, unidades e leitores**

- `HORA_TETO_MS` é duração de espera em milissegundos, escrita uma vez em 2000 e lida na chamada `_noTeto` que engloba fetch **e** `json()`. Não é o offset `timestamp_gap_ms`. (`app/static/pwa.js:88`, `app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:98`)
- `SELFIE_TETO_MS` é duração base de 1500 ms; abertura lê multiplicada por dois, captura lê diretamente. O mecanismo `_noTeto` resolve timeout/erro, sem depender de o abort encerrar a promise original. (`app/static/pwa.js:129`, `app/static/pwa.js:134`, `app/static/pwa.js:138`, `app/static/pwa.js:140`, `app/static/pwa.js:162`, `app/static/pwa.js:177`)
- No teto da hora **não se grava um novo gap**: cai no `catch` e lê `ntp_gap || 0`. A fórmula corrigida permanece `Date.now()+gap`; assim, não surge conversão nova de unidade/fuso, mas uma espera de até 2 s antes da leitura do instante desloca a hora gravada frente à rejeição imediata sem rede. Não se preserva a hora do clique antes da espera. (`app/static/pwa.js:99`, `app/static/pwa.js:104`, `app/static/pwa.js:108`, `app/static/pwa.js:182`, `app/static/pwa.js:183`)
- Escritor dos configs: `calcularESalvarGap`, via `salvarConfig`, grava `ntp_gap` como diferença de epoch-ms e `ntp_calculado_em` como epoch-ms local. Leitores: `obterGap` decide validade de 30 min e recupera offset; fallback da própria medição recupera o offset. (`app/static/pwa.js:63`, `app/static/pwa.js:101`, `app/static/pwa.js:103`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:112`)
- **PROCEDE, transporte continua incompleto.** Fila grava timestamp ISO corrigido, timestamp ISO bruto, gap-ms, precisão e instante GPS; sync usa `b.timestamp` só para ordenação e transmite apenas tipo/coordenadas/timestamp bruto/foto/liveness, omitindo corrigido/gap/precisão/instante GPS. (`app/static/pwa.js:188`, `app/static/pwa.js:189`, `app/static/pwa.js:190`, `app/static/pwa.js:191`, `app/static/pwa.js:211`, `app/static/pwa.js:217`, `app/static/pwa.js:226`)
- API de sincronização efetivamente usada é `/api/ponto/bater/`, roteada para `api_bater_ponto`; endpoint antigo retorna 410. As duas cascas passam somente `timestamp_dispositivo` para `hora_do_aparelho` e gravam `ts_efetivo`/`ts_aparelho`, sem aplicar gap da fila. (`app/static/pwa.js:227`, `app/api/urls.py:67`, `app/ponto/views.py:419`, `app/ponto/views.py:426`, `app/api/views.py:518`, `app/api/views.py:666`, `app/api/views.py:678`, `app/api/views_core.py:505`, `app/api/views_core.py:658`, `app/api/views_core.py:670`)
- `hora_do_aparelho` aceita ISO e epoch em segundos/ms, torna naive aware, aceita hora até 48 h no passado e usa chegada para futuro/antiga demais. Isso opera sobre **timestamp bruto**, não pressupõe unidade/fuso de `timestamp_gap_ms`. (`app/api/hora_do_aparelho.py:72`, `app/api/hora_do_aparelho.py:79`, `app/api/hora_do_aparelho.py:85`, `app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`)
- Modelo define gap como milissegundos; tela mostra `ms` e compara com ±300000; JSON de detalhes entrega `gap_ms` sem conversão. As leituras API encontradas de `timestamp_gap_ms` são de **ping geo**, comparando ±300000 para aviso de skew; não são o kernel de bater/sincronizar e não aplicam fuso ao gap. (`app/ponto/models.py:76`, `app/templates/ponto/batidas_offline_pendentes.html:77`, `app/templates/ponto/batidas_offline_pendentes.html:79`, `app/templates/ponto/batidas_offline_pendentes.html:80`, `app/core/views.py:1022`, `app/api/views.py:2327`, `app/api/views.py:2340`, `app/api/views.py:2371`, `app/api/views_core.py:874`, `app/api/views_core.py:905`)

**Autoridades e derivação**

- Juiz registrado de selfie decide **ausência cobrável**, recebendo origem/canal/foto/duração; PWA = 0 min, APK/desconhecido = 90 min. Isso não é o timeout de câmera nem veto à gravação. As duas APIs consultam esse juiz sob foto ausente e depois chamam a porta de registro; foto é anexada condicionalmente. (`app/core/juizes.py:1192`, `app/core/juizes.py:1195`, `app/ponto/registro_batida.py:72`, `app/ponto/registro_batida.py:75`, `app/ponto/registro_batida.py:80`, `app/api/views.py:449`, `app/api/views.py:461`, `app/api/views.py:666`, `app/api/views.py:781`; `app/api/views_core.py:436`, `app/api/views_core.py:448`, `app/api/views_core.py:658`)
- Juiz GPS registrado considera zero/par parcial sem GPS e é consultado pelo geofence depois da gravação; adquirir coordenadas ou medir frescura no front é outra pergunta. (`app/core/juizes.py:1196`, `app/core/juizes.py:1201`, `app/ponto/services/geofence.py:126`, `app/ponto/services/geofence.py:145`, `app/ponto/services/geofence.py:269`, `app/api/views.py:789`)
- **Derivação paralela já existente:** V2 calcula distância e `dentro = d <= POSTO.raio` para badge próprio apesar do juiz registrado `classificar_posicao`. Esse trecho não é introduzido pelo delta fornecido; não confundir com a cura do timeout. (`app/templates/ponto/bater_ponto_v2.html:199`, `app/templates/ponto/bater_ponto_v2.html:217`, `app/templates/ponto/bater_ponto_v2.html:218`, `app/core/juizes.py:1191`)
- Textos contraditórios continuam: V2 exige verbalmente “Tire a selfie para registrar”, embora `garantirRequisitos` só avise e continue com foto ausente. (`app/templates/ponto/bater_ponto_v2.html:100`, `app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:276`)

## 11/11 14:25 · sessao f9c39a062fdf · - · - · origem=Ronald (tmux)

- **Textos contraditórios: NÃO fechado.** Triagem mantém “Tire sua selfie para liberar o ponto”, “Selfie obrigatória” e “Selfie obrigatoria — registre o ponto”; V2 mantém “Tire a selfie para registrar”, apesar de a fila aceitar foto nula. (`app/templates/ponto/triagem/apto.html:66`, `:124`, `:194`, `:250`; `app/templates/ponto/bater_ponto_v2.html:100`; `app/static/pwa.js:192`)

- **PWA ausente offline: NÃO fechado.** As duas telas dependem de `JulianiPWA.registrarBatidaOffline`; ausência do objeto cai no erro e exige nova tentativa, sem gravação alternativa. O teste sem PWA continua restrito ao online. (`app/templates/ponto/triagem/apto.html:289`, `:308`; `app/templates/ponto/bater_ponto_v2.html:319`, `:325`, `:332`, `:338`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:465`)

- **Legado eliminado: mantido.** A seleção de template retorna triagem ou V2; o comentário do verificador registra a exclusão de `ponto/bater_ponto.html`. O nome textual permanece no ticket histórico. (`app/ponto/views.py:225`, `:356`; `app/scripts/check_a_lei_batida.py:20`; `app/docs/TICKETS.md:693`)

- **`estadoPermissao`: não participa do fluxo examinado.** `garantirRequisitos` chama diretamente a captura central e as tentativas limitadas de GPS. (`app/templates/ponto/bater_ponto_v2.html:269`, `:274`, `:279`)

- **Teto da hora tem declaração única, significado de duração em milissegundos e um consumidor operacional:** `calcularESalvarGap` passa `HORA_TETO_MS` a `_noTeto`, envolvendo fetch e leitura do corpo. A V2 e a casca de triagem conservam literais independentes de 2000 ms para relógios de apresentação; eles não leem a constante e não calculam o gap persistido. Há parametrização paralela, mas são saídas distintas; não constitui, por si, duplicação do juiz registrado de selfie/GPS. (`app/static/pwa.js:88`, `:93`, `:98`, `:103`; `app/templates/ponto/bater_ponto_v2.html:160`, `:167`, `:169`; `app/templates/ponto/triagem/base_triagem.html:38`, `:41`, `:46`; `app/core/juizes.py:1195`, `:1201`)

- **`SELFIE_TETO_MS` é uma duração base, não um teto absoluto único para toda operação:** há uma declaração de 1500 ms; captura lê diretamente e abertura lê multiplicado por dois. Essa diferença é explícita no código e no oráculo. (`app/static/pwa.js:129`, `:162`, `:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:359`)

- **O fallback no teto não grava gap novo.** Sucesso escreve `ntp_gap` e `ntp_calculado_em`; timeout apenas lê o gap anterior ou devolve zero. Sem medição válida anterior, cada nova obtenção continua elegível à consulta, pois o fallback não renova `ntp_calculado_em`. (`app/static/pwa.js:104`, `:105`, `:108`, `:113`)

- **Timestamp não é o instante do clique.** O escritor espera `obterGap()` e somente depois lê `Date.now()` para `timestamp`; lê outro relógio para `timestamp_dispositivo`. Assim, para o mesmo gap, uma `/hora/` pendurada faz o timestamp refletir a espera de aproximadamente dois segundos, enquanto rejeição imediata segue antes. O comentário “caminho de hoje sem rede” descreve o valor do fallback, não identidade temporal das duas execuções. (`app/static/pwa.js:86`, `:98`, `:108`, `:182`, `:183`, `:189`)

- **Os testes novos não verificam esse valor temporal.** `falhas_da_fila` julga chegada, duração até `add` e presença de foto; não compara `timestamp`, `timestamp_dispositivo`, gap nem instante do clique. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:484`, `:490`, `:493`)

- **Autoridade servidor continua distinta:** `selfie_ausente` recebe dados e duração, sem ler relógio; decide cobrança vencida conforme canal, não admissibilidade do clique. `_respostaSelfie` produz foto/stream e aviso para as telas. Não há duplicação desse predicado pelo delta examinado. (`app/ponto/registro_batida.py:67`, `:72`, `:80`, `:86`; `app/static/pwa.js:144`; `app/core/juizes.py:1193`)

- **Aviso da V2 agora usa o mesmo valor produzido pelo PWA:** `_respostaSelfie` escreve `aviso`; `abrirCamera` o lê em `span.textContent`; o ramo de captura também o lê no toast. Foto anterior é escrita pelo input e passada à captura central. (`app/static/pwa.js:145`, `:178`; `app/templates/ponto/bater_ponto_v2.html:250`, `:255`, `:274`, `:276`)

- **Duas cascas incluem PWA**, porém o teste força a casca pelo loader, não demonstra a seleção real de casca em requisição de produção. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:255`, `:258`)

Execução do navegador, smoke real, hash do arquivo em produção e efeitos em produção: **[nao verificado]**.

## 11/11 14:25 · sessao 7fb9cd9072df · - · - · origem=Ronald (tmux)

**B1 e B2: a nova prova tem mordida para os defeitos específicos, por inspeção; execução e morte efetiva dos mutantes são [nao verificado].** O executor escreve página, script e perfil temporários, portanto não o executei sob a restrição de somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:279`, `:286`, `:295`, `:298`)

- **FilaRealTest / Me — PROCEDE a correção da lacuna anterior.** A sonda já não substitui `registrarBatidaOffline`: chama o escritor real e observa o `add` em `batidas_pendentes`; exercita `/hora/` normal, fetch pendurado e corpo pendurado. Me troca o teto por `1e9` e exige ausência de chegada à fila, tanto diretamente quanto nas duas telas offline. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:146`, `:151`, `:214`, `:437`, `:477`, `:501`, `:610`)

- **O stub que ignora abort fortalece a pergunta do teto**, porque `/hora/` permanece pendurada mesmo após `ctl.abort()`: o controle depende da resolução de `_noTeto`, e o mutante perde essa resolução dentro dos 15 segundos do ensaio. O corpo pendurado também está dentro do trabalho limitado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:113`, `:116`, `:119`, `:439`; `app/static/pwa.js:93`, `:97`, `:98`, `:139`)

- **Mf — PROCEDE a correção do aviso invisível.** O oráculo consulta `body.innerText`, enquanto o mutante substitui o `appendChild` visível por `title`. A câmera rejeitada precisa mostrar o aviso **antes de liberar o clique**; por isso o toast posterior de `garantirRequisitos` não salva o mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `:190`, `:342`, `:621`; `app/templates/ponto/bater_ponto_v2.html:250`, `:277`)

- **`_Base.par` — PROCEDE a correção do controle desacoplado.** Controle e mutante entram no mesmo `roda`; controle com qualquer falha ou sem cenário reprova; resultados “sonda não devolveu veredito” e “juiz sem veredito” são retirados das mortes. Os chamadores exigem mortes e padrão específico. “Mesmo lote” significa execuções Chromium separadas, cada qual com seu perfil temporário, submetidas ao mesmo executor paralelo. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `:451`, `:453`, `:455`, `:547`, `:548`, `:279`, `:298`, `:318`)

**Limitações concretas:**

- **“FilaReal” não prova persistência real nem conclusão do escritor.** O fake grava em objeto JS e marca sucesso dentro do `add`; `tx.oncomplete` vem depois. O oráculo verifica chegada, tempo e foto, mas não exige “fila resolveu”, embora o escritor real espere `tx.oncomplete`. Um cenário poderia passar tendo alcançado o fake `add` e ficando pendurado antes da conclusão da promessa. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:135`, `:150`, `:151`, `:215`, `:484`; `app/static/pwa.js:194`)

- **O fake encobre abertura bloqueada e falhas de armazenamento.** Seu `open` sempre chama sucesso após 1 ms; não modela upgrade bloqueado, quota, aborto ou rollback. O `abrirDB` real espera somente `onsuccess`/`onerror`, sem `onblocked` nem teto; `obterGap` já o espera antes da pergunta HTTP. Assim, a declaração ampla “do chamado ao add a espera é limitada” excede a prova e o código. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:160`, `:173`, `:473`; `app/static/pwa.js:48`, `:58`, `:59`, `:74`, `:113`, `:182`)

- **O caso V2 online cobre rejeição imediata, não POST pendurado ou resposta perdida.** O stub só retorna rejeição imediata ou resposta 200 imediata; não há cenário de servidor que recebe e deixa a resposta pendente. O `fetch` real anterior à fila não tem teto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:107`, `:109`, `:111`, `:506`; `app/templates/ponto/bater_ponto_v2.html:308`, `:319`)

- **O relógio do ensaio é virtual e o armazenamento é substituído justamente para acompanhá-lo.** Isso prova os timers JS exercitados nesse modelo; tempo real, suspensão da aba e durabilidade IndexedDB são [nao verificado]. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:126`, `:177`, `:299`)

- **Controle de câmera “ok”: ressalva ainda existente, resultado [nao verificado].** A sonda fornece `MediaStream()` vazio e falsifica dimensões; mantém `drawImage(video)` real. Uma falha nesse desenho poderia impedir alcançar `toBlob`; com `_Base.par`, porém, isso torna o controle vermelho e impede declarar o mutante morto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:80`, `:87`, `:89`, `:452`; `app/static/pwa.js:172`)

## 11/11 14:10 · sessao a833efdfbdac · FIO-E0-V4-E4B · construtor · origem=Code

Quatro correções implementadas: dia pelo juiz existente, trava por âncora, texto limitado e coleta confirmada após savepoint. [Recibo completo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c7.txt:20).

Observado em memória: quatro REDs na c6, seis mutantes detectados e 21 testes OK. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c7.txt:115). [nao verificado]

ORM não executado; comandos no recibo. Certificação Code/opus e concorrência real: [nao verificado].

## 11/11 13:53 · sessao cf07b64d85a6 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** Há dois bloqueantes: a idempotência não cobre fechamentos concorrentes da mesma competência, e o novo escritor interpreta o dia civil de uma conversa como competência de fato. (`app/chamados/services/fecho_runtime.py:30`, `app/chamados/services/fecho_runtime.py:35`, `app/pautas/models.py:123`, `app/chamados/models.py:2794`)

1. **Mudanças de produção e cumprimento do aval**

| Arquivo | Mudança, evento e avaliação |
|---|---|
| `disputa_emissao.py` | No S143, substitui o retorno antecipado pela coleta da recusa. Isso libera os passos seguintes também nos chamadores compartilhados. Cumpre a retirada do retorno exigida pelo aval. (`app/chamados/services/disputa_emissao.py:1945`, `app/chamados/services/disputa_emissao.py:1950`) |
| `fecho_runtime.py` | Acrescenta coletor em memória, agrupamento por competência, consulta de Pauta existente e escrita depois da transição, dentro do mesmo `atomic`. Cumpre o encaixe nas bordas; tem os dois bloqueantes indicados acima. (`app/chamados/services/fecho_runtime.py:14`, `app/chamados/services/fecho_runtime.py:21`, `app/chamados/services/fecho_runtime.py:66`) |
| `acoes_chamado.py` | A resposta livre do gestor passa a chamar o adaptador, conservando a transição existente. (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/acoes_chamado.py:316`) |
| `fio.py` | A resposta rápida do DP passa pelo mesmo adaptador após registrar a mensagem. (`app/chamados/services/fio.py:718`, `app/chamados/services/fio.py:722`) |

S128 continua lendo C6, tranca e testemunha da resposta; a porta `carimbar` reaplica C6 antes de persistir. HX-RESPOSTA-VISIVEL continua lendo `irmas_respondidas_sem_consumo` e chamando `reabrir`; HX-DISPUTA-ORFA continua lendo C6 e usando `carimbar`. Não achei veto novo de exportada/limbo nesses passos. (`app/chamados/services/disputa_emissao.py:1964`, `app/chamados/services/disputa_emissao.py:1968`, `app/chamados/services/disputa_emissao.py:1977`, `app/chamados/services/disputa_emissao.py:2007`, `app/chamados/services/disputa_emissao.py:2046`, `app/chamados/models.py:3139`)

C7 continua pelo próprio `pode_fechar`, com consulta fresca da tranca. Ele julga a **disputa**, enquanto a transição julga o **chamado**: a disputa mista pode permanecer aberta e o chamado fechar. O teste exige precisamente essa distinção. (`app/chamados/services/disputa_emissao.py:2079`, `app/chamados/services/disputa_emissao.py:2089`, `app/chamados/models.py:1848`, `app/chamados/tests/test_e4b_c6_orm.py:114`)

A trava do chamado permanece: a competência entregue continua fora de `_vivas_entre`. A nova Pauta usa exclusivamente `pautas.services.escrever`, destinatário `dp`, remetente `ti` e âncora de competência; o motivo limbo vem da recusa original. (`app/chamados/models.py:364`, `app/chamados/services/fecho_runtime.py:42`, `app/chamados/services/fecho_runtime.py:46`)

Não achei backfill, lista persistente nova ou escrita em `PENDENTES_RONALD` no módulo acrescentado. O contexto só é instalado no adaptador das duas bordas; repetir transição de chamado já encerrado não instala coleta. A correspondência integral da cópia ao commit/base permanece **[nao verificado]**, pois não achei metadados Git nesta cópia. (`app/chamados/services/fecho_runtime.py:11`, `app/chamados/services/fecho_runtime.py:63`, `app/chamados/services/fecho_runtime.py:68`)

2. **Quem escreve, quem lê e significado dos campos**

| Campo/valor | Escritor ou produtor | Leitor e significado |
|---|---|---|
| `_RECUSAS`, token | Declaração com `default=None`; instalação no início do ato e restauração em `finally`. (`app/chamados/services/fecho_runtime.py:11`, `:68`, `:72`) | `registrar_recusa` lê o contexto; fora dele não coleta. (`app/chamados/services/fecho_runtime.py:16`) |
| `ato['chamado']` | Recebe `chamado.pk`. (`app/chamados/services/fecho_runtime.py:67`) | É comparado ao PK do chamado que disparou o resolver; não é PK de disputa. (`app/chamados/services/fecho_runtime.py:17`) |
| `ato['perguntas']` | `registrar_recusa` escreve `{pergunta.pk: (objeto, motivo)}`; nova coleta do mesmo PK sobrescreve a anterior. (`app/chamados/services/fecho_runtime.py:18`) | `_escrever_pautas` percorre os valores após a transição. (`app/chamados/services/fecho_runtime.py:28`, `:73`) |
| `res`/`motivo` | S143 produz `recusa:credor_toca_exportada` ou `recusa:credor_carrega_limbo:<PKs>[:credor_toca_exportada]`. (`app/chamados/services/disputa_emissao.py:1835`, `:1840`, `:1860`) | O resolver reconhece os prefixos; o escritor remove apenas `recusa:` para compor o texto. (`app/chamados/services/disputa_emissao.py:1946`, `app/chamados/services/fecho_runtime.py:42`) |
| `p.pk`, `p.disputa` | Nascimento da pergunta por `_nascer`, com disputa e inserção. (`app/chamados/models.py:3014`, `:3017`) | Identificam pergunta e relação usada para chegar ao colaborador/empresa. (`app/chamados/services/fecho_runtime.py:29`, `:32`) |
| `p.data_evento` | Atribuição central em `gravar_dia_do_fato`; `abrir` recebe dia do turno, `abrir_conversa` fornece dia civil. (`app/chamados/services/fio.py:791`, `:801`, `app/chamados/models.py:2776`, `:2801`) | O novo escritor lê cru, sem `conversa`; aqui está a colisão de significados. (`app/chamados/services/fecho_runtime.py:30`) |
| Empresa e corte | Empresa vem da relação do colaborador; corte é cadastro de `Empresa`, escrito na manutenção cadastral. (`app/colaboradores/models.py:321`, `:46`, `app/colaboradores/views.py:523`) | `janela_atual` lê o corte por `corte_da_empresa`; o runtime usa a empresa da disputa. (`app/ponto/janelas.py:36`, `:60`, `app/chamados/services/fecho_runtime.py:29`) |
| Competência, `fim.year/month` | `janela_atual(data_evento, empresa)` fornece a janela; o runtime usa seu fim. (`app/chamados/services/fecho_runtime.py:30`) | Forma `<empresa>:<AAAA-MM>`; para perguntas de fato, é competência por corte, não mês civil. (`app/chamados/services/fecho_runtime.py:31`, `app/ponto/janelas.py:24`) |
| `MARCADOR`, `ancora_tipo`, `ancora_id` | Constante local e composição da âncora; persistência pela porta. (`app/chamados/services/fecho_runtime.py:10`, `:31`, `app/pautas/services.py:184`) | Deduplicação por marcador+âncora em Pautas vivas; leitores gerais usam âncora para localizar Pautas. (`app/chamados/services/fecho_runtime.py:35`, `app/pautas/services.py:334`) |
| Texto, destinatário, remetente, autor | Runtime monta texto e passa `dp`, `ti`, `autor_sistema('ti')`; `escrever` valida e persiste. (`app/chamados/services/fecho_runtime.py:43`, `:46`, `app/pautas/services.py:157`, `:180`) | Aba DP filtra destinatário; payload entrega texto e âncora. Autor é informação de trilha. (`app/pautas/services.py:308`, `app/pautas/views.py:39`, `app/pautas/models.py:105`) |
| `feito_em`, `cancelado_em` | Portas de estado de Pauta; `marcar_feita` escreve o primeiro. (`app/pautas/services.py:233`, `:247`) | `.vivas()` significa não feita e não cancelada; portanto uma Pauta feita não impede outra entrega posterior. (`app/pautas/models.py:57`, `:64`, `app/chamados/services/fecho_runtime.py:36`) |
| `status_local`, `novo_status`, usuário | As bordas fornecem destino/usuário; `transicionar` delega ao escritor `_gravar_estado`, que salva status e campos associados. (`app/chamados/services/acoes_chamado.py:316`, `app/chamados/models.py:199`, `:228`, `:296`) | Adaptador lê `VIVOS→ENCERRADOS`; motor define os conjuntos; signal lê o status salvo. (`app/chamados/services/fecho_runtime.py:63`, `app/chamados/catalogo/motor.py:24`, `:30`, `app/chamados/signals.py:96`) |

**Não achei troca de PK de chamado por PK de disputa** nessa composição: a âncora do fio devolve chamado, o coletor guarda chamado, e a âncora da Pauta guarda empresa+competência. (`app/chamados/services/fio.py:117`, `app/chamados/services/fecho_runtime.py:67`, `app/pautas/models.py:34`)

**Achei dia civil tratado como dia de fato.** O discriminador já existe: `_dia_de_turno_da_pergunta` devolve `None` para conversa; o novo escritor não o considera. Os leitores crus anteriores estão declarados em censo, mas isso não elimina o novo uso incorreto. (`app/chamados/juizes.py:332`, `app/chamados/services/fecho_runtime.py:30`, `app/chamados/services/fio.py:1389`)

A deduplicação também não agrega novas recusas ao texto existente: encontrou marcador+âncora viva, executa `continue`. Portanto entrega posterior de outro chamado da mesma competência não acrescenta seus identificadores à Pauta existente. Isso segue a deduplicação escolhida, mas limita a rastreabilidade por chamado. (`app/chamados/services/fecho_runtime.py:35`, `app/chamados/services/fecho_runtime.py:37`)

3. **Fechamentos fora das duas bordas**

Fora do contexto, a recusa continua deixando nota no credor, mas sua coleta para Pauta é no-op. A diferença c6 nesses caminhos é liberar S128/HX/C7 após a recusa; não acrescentar entrega DP. (`app/chamados/services/disputa_emissao.py:1855`, `app/chamados/services/disputa_emissao.py:1950`, `app/chamados/services/fecho_runtime.py:17`)

| Caminho | Efeito depois da cura |
|---|---|
| S143 por save de status | Continua condicionado a status encerrado e escrita da coluna; executa resolver em savepoint. Sem contexto, nenhuma Pauta E4B. (`app/chamados/signals.py:96`, `:104`, `:114`, `app/chamados/services/fecho_runtime.py:17`) |
| `becos.fechar_beco` | Trabalha com chamados já terminais, chama resolver e depois C7; pode realizar limpezas antes interrompidas. Não instala coletor. (`app/chamados/services/becos.py:43`, `:54`, `:55`) |
| Reconciliador `_retratar` | Após retenção e reconciliação permitirem, chama resolver e segue para nota; ignora seu retorno. (`app/chamados/reconciliador.py:518`, `:523`, `:536`, `:538`) |
| Reconciliador/costura | Após reaponte aceito para candidato resolvido, chama resolver e retorna `True`. (`app/chamados/reconciliador.py:1642`, `:1648`, `:1649`) |
| `reconciliar_fantasmas` | No apply, pode chamar resolver diretamente; contagens e evento podem agora refletir limpezas antes suprimidas. (`app/ponto/management/commands/reconciliar_fantasmas.py:269`, `:281`, `:282`, `:288`) |
| Outros atos humanos | Exceção, agenda, escala, geofence e benefício continuam usando transição direta; `fechar()` também. (`app/chamados/services/acoes_chamado.py:70`, `:108`, `:149`, `:222`, `:277`, `app/colaboradores/views.py:2068`, `app/chamados/models.py:454`) |
| Signals e cascatas | Disputa fechada reconcilia chamado; desligamento/vínculo usam portas existentes, sem coletor E4B. (`app/chamados/signals.py:42`, `app/colaboradores/services/desligamento.py:155`, `app/colaboradores/services/vinculo.py:387`, `app/chamados/models.py:1619`) |
| Commands/cron | Meio período usa `fechar`; becos usa sua porta. O cron de becos está configurado como alarme. (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:109`, `app/chamados/management/commands/fechar_becos_disputa.py:57`, `app/config/crons.py:663`) |
| API | Não achei fechamento direto de chamado no endpoint consultado: seu POST anexa nota. Há fechamento indireto por disputa/regularização. (`app/api/views.py:988`, `app/api/foto_recebida.py:80`, `app/chamados/services/regularizacao_ext.py:205`) |
| Django admin | Registro de `ChamadoColaborador` no Django admin: **não achei**. [nao verificado: ausência por pesquisa textual] |

**Sim, permanece possível fechar com viva exportada sem entrega E4B:** usar `ch.fechar(admin)` no cenário c6 passa pela transição e pelo S143, mas não instala coletor; a exportada fica fora da F1. Isso é lacuna de alcance anterior, preservada pela delimitação às duas bordas, não nova supressão de uma entrega existente. Reprodução ORM dessa variante: **[nao verificado]**. (`app/chamados/models.py:454`, `app/chamados/models.py:364`, `app/chamados/signals.py:115`, `app/chamados/services/fecho_runtime.py:17`)

O teste de passivo não cobre todos esses fechamentos futuros: planta o chamado já fechado por SQL e só depois chama save/resolver/beco. (`app/chamados/tests/test_e4b_c6_orm.py:160`, `app/chamados/tests/test_e4b_c6_orm.py:163`)

4. **Atomicidade e ordem**

- **Se `escrever` levantar exceção:** ela sai do adaptador e desfaz a transição no banco, porque ambas estão no `atomic` externo. Não fica chamado fechado por esse ato com a Pauta falhada. (`app/chamados/services/fecho_runtime.py:66`, `app/chamados/services/fecho_runtime.py:70`, `app/chamados/services/fecho_runtime.py:73`)
- **Se a transição levantar exceção:** o `finally` restaura o contexto e a escrita de Pauta não é alcançada. (`app/chamados/services/fecho_runtime.py:69`, `app/chamados/services/fecho_runtime.py:72`)
- **Limite dessa garantia:** erro interno do S143 é engolido pelo signal. Antes da coleta, pode deixar o chamado fechado sem entrega; depois da coleta, pode desfazer as limpezas e ainda permitir Pauta, pois o dicionário em memória não participa do rollback. Esses cenários de falha não foram demonstrados no ORM fornecido. (`app/chamados/signals.py:114`, `app/chamados/signals.py:116`, `app/chamados/services/fecho_runtime.py:18`, `app/chamados/services/fecho_runtime.py:73`)
- **Concorrência:** não há garantia de uma Pauta por marcador+âncora. A consulta e a criação são separadas; o modelo tem índice de âncora, mas não unicidade dessa entrega. Atualizar o contador depois da criação não revalida a existência. (`app/chamados/services/fecho_runtime.py:35`, `app/pautas/services.py:180`, `app/pautas/models.py:116`, `app/pautas/models.py:123`, `app/pautas/services.py:188`)

5. **Classes de reprovação e força das provas**

Não achei segundo juiz nem segundo escritor de Pauta fora da porta. A competência por corte usa `janela_atual`; o defeito temporal é o **insumo civil da conversa**, não uma fórmula nova de mês. (`app/chamados/services/disputa_emissao.py:1964`, `app/chamados/services/disputa_emissao.py:2089`, `app/chamados/services/fecho_runtime.py:25`, `app/chamados/services/fecho_runtime.py:46`)

Não achei leitura nova de relógio no módulo c6. Entretanto continua existindo relógio dentro do C6, fora da borda: S143 passa `hoje=None` e `_dentro_da_janela_viva` pode usar `date.today()`. É comportamento preexistente, não corrigido nesta volta. (`app/chamados/services/disputa_emissao.py:1945`, `app/chamados/juizes.py:577`)

Não achei inversão nova da ordem Colaborador→Disputa→Chamado→Pergunta. A trava existente ocorre antes do save; porém ela não serializa colaboradores distintos pela âncora empresa+competência da Pauta. (`app/chamados/models.py:289`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`)

Os dois mutantes c6 têm mordida direta: retorno antecipado impede o carimbo da morta; `sem_pauta` elimina a entrega. O protocolo rejeita erro, precondição quebrada, sobrevivência e assertion de outro alvo. (`app/chamados/tests/e4b_c6_verificar.py:24`, `app/chamados/tests/e4b_c6_verificar.py:27`, `app/chamados/tests/e4b_c6_verificar.py:114`, `app/chamados/tests/test_e4b_c6_protocolo.py:39`)

O dado anexado demonstra GREEN 383, as quatro mortes c6 declaradas e oito mortes c4; não demonstra suíte inteira. No recorte c6, os dois ensaios da **base** não aparecem explicitamente certificados. Suíte inteira e execução independente nesta cópia: **[nao verificado]**. (`anexo c6_orm.log:2`; roteiro exigido em `app/chamados/tests/e4b_c6_verificar.py:109`)

Não achei teste c6 de concorrência, falha real de escrita da Pauta ou conversa. Os testes existentes exercitam reentrega serial e erro dentro da transição em memória. Logo seus verdes não fecham os bloqueantes encontrados. (`app/chamados/tests/test_e4b_c6_orm.py:136`, `app/chamados/tests/test_e4b_c6_memoria.py:82`, `app/chamados/tests/test_e4b_c6_memoria.py:93`)

6. **Contraexemplos concretos**

- **Duplicação concorrente — bloqueante.** Empresa 2, competência `2026-09`, dois chamados de colaboradores distintos, ambos com recusa exportada e nenhuma Pauta existente. A e B observam `exists=False`; A cria/commita; B continua sem reler e cria outra. Resultado permitido: **duas Pautas vivas com o mesmo marcador e âncora**. As travas por colaborador não impedem essa intercalação. Reprodução concorrente ORM: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:35`, `app/chamados/services/fecho_runtime.py:46`, `app/chamados/models.py:2666`, `app/pautas/models.py:123`)

- **Competência da conversa — bloqueante.** Empresa com corte 24; conversa `ancora_ausente` criada em 23/09, destino A e pai B distinto, sem resposta/carimbo/célula; depois competência 09 entregue. Essa combinação nasce pela porta existente. Ao fechar A, S127 não encontra disputa com pai A e F1 exclui a pergunta pela data civil entregue; C6 conserva a conversa viva. O runtime escreve Pauta `empresa:2026-09`, atribuindo competência de fato à conversa que não possui dia de turno. Ocorrência no dado de produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1339`, `app/chamados/services/disputa_emissao.py:1371`, `app/chamados/juizes.py:2322`, `app/chamados/models.py:364`, `app/chamados/juizes.py:336`, `app/chamados/juizes.py:1405`, `app/chamados/services/fecho_runtime.py:30`)

- **Motivo truncado — defeito adicional.** Vinte perguntas recusadas e motivo limbo contendo quarenta PKs de seis dígitos excedem o campo: limitar vinte IDs não limita a lista dentro do motivo. A porta grava apenas os primeiros 500 caracteres, podendo perder PKs e o sufixo `credor_toca_exportada`. O código permite essa perda; ocorrência em produção: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/fecho_runtime.py:39`, `app/chamados/services/fecho_runtime.py:42`, `app/pautas/services.py:185`)

**Veredito final: REPROVADO**, pelos bloqueantes de idempotência concorrente e interpretação do dia civil da conversa; a suíte inteira também permanece pendente de comprovação. (`app/chamados/services/fecho_runtime.py:30`, `app/chamados/services/fecho_runtime.py:35`; suíte inteira **[nao verificado]**)

## 11/11 13:49 · sessao 23a023a5f323 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- A mudança no resolver retira apenas o retorno antecipado e entrega a recusa ao coletor; S128, HX e C7 continuam nos juízes existentes. C6/C7/tranca estão registrados, sem autoridade paralela acrescentada. (`app/chamados/services/disputa_emissao.py:1945-1950`, `:1964`, `:1968`, `:1977`, `:2007`, `:2046`, `:2089`; `app/core/juizes.py:278-279`, `:292`)

- A porta de carimbo reaplica C6 após aplicar os campos em memória e desfaz se a pergunta continuar viva; HX resposta conserva a porta `reabrir` e suas recusas. (`app/chamados/models.py:3128-3145`, `:1390`; `app/chamados/services/disputa_emissao.py:2007-2009`)

- A ordem de trava continua Colaborador → Disputa → Chamado → Pergunta; o escritor de estado trava antes de salvar e o resolver reutiliza o mesmo protocolo. Não achei alteração dessa ordem no trecho c6. (`app/chamados/models.py:280-296`, `:2665-2693`; `app/chamados/services/disputa_emissao.py:1877-1879`)

- Não achei leitura nova de relógio no runtime: `janela_atual` recebe `p.data_evento`; o relógio de fallback C6 já existia. `janela_atual(None)` falha em `data.day`, sem substituir por hoje. (`app/chamados/services/fecho_runtime.py:30`; `app/chamados/juizes.py:577`; `app/ponto/janelas.py:24-27`)

- Há limitação de atomicidade das limpezas: S143 tem savepoint próprio e engole exceções. Uma recusa coletada antes de erro posterior permanece na ContextVar, embora as limpezas S128/HX/C7 daquele savepoint sejam desfeitas; a borda ainda pode escrever sua Pauta. Não implica chamado sem Pauta, mas impede equiparar “Pauta escrita” a “limpezas concluídas”. (`app/chamados/signals.py:106-117`; `app/chamados/services/fecho_runtime.py:18`, `:70-73`)

- **Novo leitor incorreto do campo com dois significados:** `abrir` grava dia do turno; `abrir_conversa` grava dia civil, e o juiz declara conversa sem competência de fato. O novo escritor usa `data_evento` cru sem distinguir `conversa`, materializando competência civil como competência de fato. (`app/chamados/models.py:2783-2802`; `app/chamados/juizes.py:332-336`; `app/chamados/services/fecho_runtime.py:29-31`)

Contraexemplo estrutural alcançável:

1. Empresa corte 24; em 23/09 criar conversa `ancora_ausente` com destino A e pai B distinto pela porta existente; depois entregar competência 09. A porta constrói precisamente pai B e destino A. (`app/chamados/services/disputa_emissao.py:1338-1362`, `:1371-1377`; `app/chamados/models.py:2801`)
2. Admin fecha A: S127 procura disputa cujo **pai** seja A, e F1 exclui a pergunta pelo mês entregue calculado sobre sua data civil; escolher `fechado` também evita a guarda S122 específica de `resolvido`. (`app/chamados/juizes.py:2322-2324`; `app/chamados/models.py:364`, `:181`)
3. A conversa sem resposta/carimbo/célula continua viva: seu dia do turno é `None`, portanto C6 não entra na janela viva e termina em `True`. (`app/chamados/juizes.py:336`, `:1394-1406`)
4. S143 recusa pela competência civil e o novo runtime escreve Pauta 09, embora a conversa não tenha competência de fato. Existência desse caso na produção fornecida: [nao verificado]. (`app/chamados/services/disputa_emissao.py:1835-1844`; `app/chamados/services/fecho_runtime.py:29-48`; `app/chamados/juizes.py:333-335`)

Os leitores crus preexistentes estão declarados em censo; isso distingue dívida anterior do **novo escritor** c6, ausente daquele censo. (`app/chamados/services/fio.py:1389-1410`)

## 11/11 13:49 · sessao 16d6a2088751 · - · - · origem=Ronald (tmux)

Censo de fechamento fora das duas bordas, somente leitura:

- **A mudança no resolver alcança todos os chamadores.** A recusa agora registra em memória e prossegue para os passos seguintes, em vez de devolver `{0,0}`; S128, HX e C7 continuam em `app/chamados/services/disputa_emissao.py:1950`, `:1964`, `:2007`, `:2046`, `:2089`. Fora de coletor, o registro é no-op (`app/chamados/services/fecho_runtime.py:11`, `:16`, `:17`).

- **Somente duas entradas produtivas instalam o coletor:** resposta livre do detalhe e resposta rápida do fio (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`). O único `.set()` está em `app/chamados/services/fecho_runtime.py:68`, o reset em `:72`, e a escrita depois da transição em `:73`. Outra entrada produtiva desse coletor: **não achei** [nao verificado: pesquisa textual não prova chamadas dinâmicas].

- **S143 por qualquer save de status encerrado:** aceita apenas `ENCERRADOS`, exclui raw, exige que o save tenha escrito status, executa resolver dentro de savepoint e engole exceções (`app/chamados/signals.py:96`, `:104`, `:114`, `:115`, `:116`). `.update()` não emite esse evento (`app/chamados/signals.py:95`). Sem coletor humano não nasce Pauta E4B (`app/chamados/services/fecho_runtime.py:17`).

- **Becos:** não fecha o chamado; trabalha com pais/destinos já terminais, trava a ordem, chama resolver e só então relê C7 e fecha disputa (`app/chamados/services/becos.py:43`, `:46`, `:51`, `:54`, `:55`, `:56`, `:57`). Após cura, recusa deixa de suprimir limpezas posteriores; não instala coletor (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/becos.py:54`; `app/chamados/services/fecho_runtime.py:17`).

- **Reconciliador `_retratar`:** retenção pode impedir; depois `reconciliar()` precisa devolver True; segue rejeição automática da justificativa pendente, resolver explícito e nota (`app/chamados/reconciliador.py:518`, `:523`, `:529`, `:532`, `:536`, `:538`). Retorno do resolver é ignorado (`:536`). Não passa a escrever Pauta E4B (`app/chamados/services/fecho_runtime.py:17`).

- **Reconciliador/costura:** reaponte ao candidato histórico precisa ser aceito; depois chama resolver do chamado já resolvido e retorna True (`app/chamados/reconciliador.py:1642`, `:1643`, `:1648`, `:1649`). Agora pode executar as limpezas após a recusa; sem coletor, nenhuma entrega E4B (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:17`).

- **`reconciliar_fantasmas`:** só aplica após opção aplicar; tenta `retratar`; se este devolver falso, chama resolver diretamente; contagens, nota e evento dependem do resultado (`app/ponto/management/commands/reconciliar_fantasmas.py:269`, `:280`, `:281`, `:282`, `:286`, `:288`). Pode passar a contar/carimbar/fechar disputas que antes o return impedia; não passa a entregar Pauta E4B (`app/chamados/services/disputa_emissao.py:1950`, `:2092`; `app/chamados/services/fecho_runtime.py:17`).

- **Outros fechamentos humanos continuam diretos:** aprovação de exceção (`app/chamados/services/acoes_chamado.py:70`), agenda (`:108`), escala (`:149`), autorização geofence (`:222`), recusa geofence (`:277`) e avaliação de benefício (`app/colaboradores/views.py:2068`). A porta `ChamadoColaborador.fechar` também chama diretamente `transicionar` (`app/chamados/models.py:454`). Nenhum desses instala coletor E4B (`app/chamados/services/fecho_runtime.py:68`, únicos consumidores em `app/chamados/services/acoes_chamado.py:316` e `app/chamados/services/fio.py:722`).

- **Signals fechadores:** disputa fechada tenta `ch.reconciliar()` antes da nota (`app/chamados/signals.py:28`, `:42`, `:45`); evento de batida chama reconciliador de chamados (`app/ponto/signals.py:214`, `:215`). O gate sistêmico decide premissa e pergunta viva e escreve pela porta de estado (`app/chamados/models.py:682`, `:685`, `:654`). Sem borda humana, não há entrega E4B (`app/chamados/services/fecho_runtime.py:17`).

- **Cascade e ajuste de vínculo:** desligamento percorre `NAO_FECHADOS` e usa `encerrar()` (`app/colaboradores/services/desligamento.py:150`, `:155`); troca de escala encerra chamados anteriores (`app/colaboradores/services/vinculo.py:384`, `:387`). `encerrar()` conserva gate de pergunta viva e escritor central (`app/chamados/models.py:1617`, `:1619`).

- **Commands/cron:** comando de meio período usa `ch.fechar()` no apply (`app/ponto/management/commands/fechar_chamados_meio_periodo.py:94`, `:109`); comando de becos usa `fechar_beco()` no apply (`app/chamados/management/commands/fechar_becos_disputa.py:54`, `:57`). Cron de becos está configurado `--alarme` (`app/config/crons.py:663`); comando de ausência com `--alarme` apenas relata e retorna (`app/ponto/management/commands/fechar_chamados_ausencia.py:35`, `:40`), e apply delega à porta existente (`:44`; `app/ponto/services/ausencia.py:694`). Nenhuma dessas entradas ganha coletor E4B (`app/chamados/services/fecho_runtime.py:68`).

- **API:** fechamento direto de `ChamadoColaborador` em `app/api` **não achei** [nao verificado: pesquisa textual]. `api_chamados_v2` POST apenas anexa nota (`app/api/views.py:988`, `:990`); upload separado de foto pode fechar disputa (`app/api/foto_recebida.py:80`), cujo signal tenta reconciliar chamado-pai (`app/chamados/signals.py:42`). Resposta de regularização chama materializador (`app/api/views.py:3000`), que pode encerrar chamado pela porta (`app/chamados/services/regularizacao_ext.py:205`).

- **Django admin:** `app/chamados/admin.py` está vazio [nao verificado: não há linha citável]; registro de `ChamadoColaborador` no Django admin **não achei** [nao verificado: pesquisa textual global].

**Pode haver novo fechamento sem entrega da viva exportada fora das duas bordas.** A exclusão da exportada da F1 é explícita (`app/chamados/models.py:364`); `fechar`, `encerrar` e `reconciliar` não instalam coletor (`app/chamados/models.py:454`, `:1619`, `:654`). Assim, no cenário c6, chamar diretamente `ch.fechar(admin)` fecha sem Pauta E4B: transição → save → S143 → recusa → coletor ausente (`app/chamados/models.py:454`, `:296`; `app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:17`). Execução desse variante: [nao verificado]. A ausência de entrega por essas entradas é anterior; não identifiquei nova supressão de entrega que já existisse [nao verificado: análise estática].

O teste de “passivo” c6 não cobre esse fechamento novo: planta o chamado já fechado por SQL antes de emitir o save/resolver/beco (`app/chamados/tests/test_e4b_c6_orm.py:160`, `:163`, `:164`, `:165`). Ele prova que passivo não recebe Pauta (`:166`), sem certificar todos os fechamentos futuros fora das bordas.

## 11/11 13:49 · sessao 6255d01866f6 · - · - · origem=Ronald (tmux)

- Os RED c6 mordem diretamente a cura: `retorno_antecipado` recoloca o retorno no lugar da coleta; `sem_pauta` elimina a chamada do escritor. As assertions correspondentes exigem morta carimbada e Pauta presente. O protocolo exige processo `rc=1`, exatamente um failure, caso e mensagem esperados e ausência de falha de precondição. (`app/chamados/tests/e4b_c6_verificar.py:24`, `:27`, `:114`; `app/chamados/tests/test_e4b_c6_orm.py:111`, `:122`)

- A sobreposição da base troca integralmente os três módulos produtivos preexistentes; `sobrepor_modulo` restaura globais e troca aliases já importados. Não achei atalho que deixe o adaptador c6 nas bordas da base. (`app/chamados/tests/e4b_c6_verificar.py:54`; `app/chamados/tests/e4b_c4_base.py:28`, `:35`, `:39`)

- O RED ORM usa C6/K6 reais como precondição, porta real de transição nas duas telas e spy de `reabrir` que delega ao método real. Confere carimbos, C7, chamado fechado, zero reabertura, Pauta com âncora e motivo, e exportação/FM inalterados. (`app/chamados/tests/test_e4b_c6_orm.py:76`, `:80`, `:86`, `:106`, `:111`, `:124`, `:131`)

- Limbo é exercitado pela irmã com texto sem `respondida_em`, com viva exportada no mesmo credor; o teste exige `credor_carrega_limbo:<pk>` no texto. Não prova limbo sem viva exportada nem pergunta sem data. (`app/chamados/tests/test_e4b_c6_orm.py:73`, `:82`, `:127`, `:147`; comportamento desses outros cenários [nao verificado])

- Não achei teste c6 com escrita real da Pauta falhando, rollback ORM desse erro ou concorrência. O teste de erro em memória falha dentro da transição e só confere restauração do ContextVar/ausência da chamada ao escritor; a idempotência exercitada é sequencial. (`app/chamados/tests/test_e4b_c6_memoria.py:61`, `:82`; `app/chamados/tests/test_e4b_c6_orm.py:136`)

- **Bloqueante de idempotência concorrente:** dois chamados de colaboradores diferentes na mesma empresa/competência podem ambos observar `exists=False`; depois cada um chama `escrever`, que faz `Pauta.objects.create`. Não há releitura nem exclusão mútua pela âncora. A trava do resolver é por colaborador; os constraints de Pauta só validam destinatário. Resultado permitido: duas Pautas vivas com mesmo marcador e âncora. Reprodução efetiva [nao verificado]. (`app/chamados/services/fecho_runtime.py:35`, `:46`; `app/pautas/services.py:180`; `app/chamados/services/disputa_emissao.py:1878`; `app/pautas/models.py:123`)

- A atualização comum do contador ocorre depois da criação da Pauta e não corrige essa corrida; eventual espera pela linha do contador não reexecuta o teste de existência. (`app/pautas/services.py:180`, `:188`, `:352`, `:354`)

- Competência usa a autoridade existente: `janela_atual(p.data_evento, empresa)` e ano/mês do fim da janela, mesma composição já usada por `competencia_do_dia_entregue`. O corte vem do cadastro da empresa. Não achei derivação nova por mês civil. (`app/chamados/services/fecho_runtime.py:29`; `app/chamados/services/vigia_fio.py:822`, `:838`; `app/ponto/janelas.py:24`, `:36`, `:60`)

- A âncora `competencia` significa empresa+competência. A leitura do DP filtra o departamento; o payload leva texto e âncora. “Viva” significa não feita e não cancelada; portanto marcar feita permite uma entrega posterior criar outra Pauta. (`app/pautas/models.py:34`, `:57`; `app/pautas/services.py:304`, `:322`; `app/pautas/views.py:39`; `app/chamados/services/fecho_runtime.py:35`)

- O texto pode exceder 500 caracteres: só os IDs das perguntas têm limite de vinte, enquanto os motivos carregam listas de limbo sem teto; a porta corta `texto[:500]`. Possível truncamento de IDs/motivos é consequência do código, mas não achei RED desse tamanho. (`app/chamados/services/fecho_runtime.py:39`, `:42`; `app/chamados/services/disputa_emissao.py:1840`; `app/pautas/services.py:185`)

- O recorte anexado demonstra GREEN 383 e quatro mortes c6 — dois mutantes × dois motivos —, além das oito mortes c4. Não mostra os dois rótulos de morte c6 da base nem a execução integral da suíte; esses resultados permanecem [nao verificado]. Os runners locais são explicitamente recortados por labels. (`app/chamados/tests/e4b_c6_verificar.py:97`, `:109`; `app/chamados/tests/e4b_c4_verificar.py:124`; execução: `c6_orm.log` anexado [nao verificado quanto a linhas não fornecidas])

## 11/11 13:45 · sessao 08cbd3a06898 · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

A cura dos bloqueios de câmera está na árvore. Permanecem uma espera sem teto antes da gravação offline e lacunas na prova dos testes. (`app/static/pwa.js:122`, `:170`, `:86`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:118`, `:94`)

Executei os dois testes estáticos de GPS, ambos verdes, e o censo de câmera em memória. Não executei o Chromium: seu executor cria arquivos, perfil e diretório temporário, contrariando a regra desta revisão. Execução dos testes de navegador, smoke nas duas cascas e efeitos em produção: **[nao verificado]**. (`app/ponto/tests/test_l030_forma_js.py:72`, `:77`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:483`, `:213`, `:229`, `:232`)

**1. Achados da volta 1: cura e mordida prevista**

“Fechado no código” abaixo não significa teste de navegador executado nesta revisão. **[nao verificado]** [nao verificado]

| Achado anterior | Situação em 618b13d98 e teste correspondente |
|---|---|
| Triagem offline esconde confirmar quando a câmera falha | **Fechado no código:** mostra o botão antes de abrir a câmera e conserva-o sem stream. Mutante M1 recoloca a ocultação e exige ausência de envio. (`app/templates/ponto/triagem/apto.html:252`, `:259`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:403`) |
| Triagem online fica presa em `getUserMedia` pendente | **Fechado no código:** abertura passa pelo teto de 3 segundos; sem stream, libera a batida. Matriz inclui câmera pendente; M3 remove a liberação. (`app/static/pwa.js:145`, `:150`; `app/templates/ponto/triagem/apto.html:154`, `:164`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:51`, `:356`, `:413`) |
| Triagem online depende exclusivamente do callback de `toBlob` | **Fechado no código:** libera controles antes da captura e aguarda a resposta limitada do PWA. Matriz inclui `toBlob` mudo e conversão lançando; teste direto verifica o teto. (`app/templates/ponto/triagem/apto.html:176`, `:177`, `:231`; `app/static/pwa.js:165`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:55`, `:56`, `:371`) |
| Triagem offline só enfileira dentro do callback | **Fechado no código:** captura resolve primeiro; enfileiramento fica fora do callback. M4 reinsere a espera infinita pelo callback. (`app/templates/ponto/triagem/apto.html:276`, `:289`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:418`) |
| V2 veta `_fotoBlob` nulo | **Fechado no código:** `garantirRequisitos` não retorna recusa por foto; o chamador prossegue. M2 reinsere o veto. (`app/templates/ponto/bater_ponto_v2.html:268`, `:297`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:408`) |
| V2 fica pendente quando `toBlob` cala e `toDataURL` lança | **Fechado no código:** fallback do timer está protegido e termina em `null`. Matriz cobre essa combinação; Md remove o teto. (`app/static/pwa.js:126`, `:165`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:56`, `:371`, `:461`) |
| Legado online/offline conserva barreiras | **Eliminado com o template**, sem renderizador vivo encontrado; M6 aparece como histórico da remoção. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/views.py:356`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`) |
| Aviso centralizado, mas captura e espera duplicadas | **Corrigido para a operação da câmera:** abertura, captura, conversão e teto estão no PWA; censo fecha os produtores. Persistem textos próprios contraditórios, tratados adiante. (`app/static/pwa.js:122`, `:145`, `:156`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:476`, `:497`; `app/templates/ponto/triagem/apto.html:124`) |
| Mutante vivo: `await` infinito em `garantirRequisitos` | **Agora tem teste específico:** Ma injeta exatamente essa espera e exige “NAO chegou” na V2 online/offline. Morte efetivamente executada: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:446`) |
| Mutante vivo: `semSelfie → false` | A função antiga saiu; **Mb ataca o equivalente atual**, trocando `aviso` por `false` em `_respostaSelfie`. Morte efetivamente executada: **[nao verificado]**. (`app/static/pwa.js:132`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:451`) |
| `throw` no ramo sem foto | **Agora tem teste específico:** Mc injeta a exceção e exige ausência de envio. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:456`) |
| PWA não carregou | **Online protegido; offline não fechado:** chamadas de selfie têm guarda; o teste sem PWA cobre apenas online. A fila continua dependendo do escritor desse script. (`app/templates/ponto/triagem/apto.html:154`, `:232`, `:289`; `app/templates/ponto/bater_ponto_v2.html:273`, `:331`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:360`) |
| `estadoPermissao` ainda tinha chamador | Definição e chamada não permanecem no fluxo atual; `garantirRequisitos` consulta diretamente captura e GPS. (`app/templates/ponto/bater_ponto_v2.html:268`) |
| Transporte incompleto de timestamps/GPS | **Não fechado:** fila guarda timestamp corrigido, gap, precisão e instante GPS; sync transmite timestamp bruto, sem esses três últimos insumos. (`app/static/pwa.js:176`, `:179`, `:205`, `:209`) |

**2. Quem decide hoje**

| Sítio | Predicado e efeito |
|---|---|
| Juiz registrado do servidor | `origem == 'app'`, ausência de ambas as fontes de foto e `decorrido_min >= prazo`. Prazo PWA: **0 minutos**; APK/desconhecido: **90 minutos**, com sobreposição explícita para APK/desconhecido. Decide **cobrança**, não admissibilidade. (`app/core/juizes.py:1192`; `app/ponto/registro_batida.py:71`, `:80`) |
| API principal | Sob `not foto`, consulta o juiz com `tem_foto=False`, duração zero; acrescenta motivo/contexto. Criação segue pela porta; anexação é condicional. (`app/api/views.py:436`, `:449`, `:461`, `:666`, `:781`) |
| API core | Mesmo predicado e comportamento. (`app/api/views_core.py:423`, `:436`, `:448`, `:658`, `:776`) |
| Retro | Consulta o juiz usando canal gravado, `FotoBatida OR Batida.foto` e duração em minutos; emite cobrança pela porta de disputa. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:74`, `:96`) |
| POST web da triagem | Grava primeiro; `foto_sessao or request.FILES.get("foto")` controla somente anexação. Não cobra selfie inline nesse ramo. (`app/ponto/services/triagem_batida.py:701`, `:748`) |
| PWA | Sem frame, usa foto anterior ou `null`; captura/conversão falha ou pendente termina pelo teto; ausência produz aviso. (`app/static/pwa.js:126`, `:133`, `:158`, `:165`) |
| Triagem online | `r.stream` escolhe confirmação de selfie ou liberação sem câmera; `sel.foto` decide anexação, sem recusa por ausência. (`app/templates/ponto/triagem/apto.html:155`, `:164`, `:233`, `:241`) |
| Triagem offline | Confirmar permanece disponível; `sel.foto` ou `null` vai ao escritor da fila. (`app/templates/ponto/triagem/apto.html:252`, `:277`, `:289`) |
| V2 online/offline | Captura pode atualizar `_fotoBlob` e agendar aviso; não veta foto nula. Online anexa condicionalmente; offline entrega o valor à fila. (`app/templates/ponto/bater_ponto_v2.html:273`, `:275`, `:297`, `:305`, `:331`) |
| Confirmação de turno | Formulário envia entrada sem campo de foto, captura ou fila própria. (`app/templates/ponto/triagem/turno_confirmar.html:8`) |
| Fila e sincronizador | Escritor normaliza ausência para `null`; sincronizador omite arquivo quando `!b.foto` e envia o restante. (`app/static/pwa.js:180`, `:214`, `:215`) |

**`_respostaSelfie` isoladamente não é o único juiz operacional:** ela apenas monta `{stream|foto, aviso}`. A operação central está no conjunto `_noTeto`/`abrirCameraSelfie`/`capturarSelfie`; as telas ainda controlam apresentação e transporte. Não encontrei nelas veto independente por permissão negada, câmera ocupada, câmera pendente ou conversão falha: esses resultados passam pela mesma resposta limitada. Câmera ocupada não tem cenário nominal próprio no teste; sua execução específica é **[nao verificado]**. (`app/static/pwa.js:122`, `:132`, `:145`, `:156`; `app/templates/ponto/triagem/apto.html:154`, `:255`; `app/templates/ponto/bater_ponto_v2.html:240`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:50`)

A linguagem ainda decide por conta própria em alguns pontos: “Selfie obrigatória” e “Tire a selfie para registrar” contradizem o comportamento permissivo. São mensagens próprias, embora não constituam veto técnico. (`app/templates/ponto/triagem/apto.html:66`, `:124`, `:194`, `:250`; `app/templates/ponto/bater_ponto_v2.html:100`)

**3. Teto, porta e clique perdido**

O valor mora em `SELFIE_TETO_MS = 1500`, no PWA: captura usa **1,5 segundo**; abertura usa **3 segundos**. `_noTeto` resolve erros e expiração como valor ou `null`; ambas as operações passam depois por `_respostaSelfie`. (`app/static/pwa.js:117`, `:122`, `:150`, `:165`)

Esse teto limita a selfie, não o clique inteiro: a triagem pode esperar GPS por 8 segundos, e a V2 faz duas tentativas de 10 e 12 segundos antes do envio. (`app/templates/ponto/triagem/apto.html:218`; `app/templates/ponto/bater_ponto_v2.html:279`)

A batida liberada sem foto segue **pela mesma porta de sua rota normal**: triagem usa o mesmo formulário e `registrar_batida`; V2 usa `/api/ponto/bater/`; fila sincroniza nesse endpoint, cujos dois kernels chamam `registrar_batida`. Não há porta especial para “estourou o teto”. (`app/templates/ponto/triagem/apto.html:241`; `app/ponto/services/triagem_batida.py:701`; `app/templates/ponto/bater_ponto_v2.html:307`; `app/static/pwa.js:215`; `app/api/views.py:666`; `app/api/views_core.py:658`)

**B1 — permanece um caminho de clique sem gravação offline:** `registrarBatidaOffline` espera `obterGap()` antes do `add`. Com gap ausente/vencido, esse caminho espera `fetch('/hora/')` sem timeout. Se a requisição ficar pendente, a selfie já terá resolvido, mas a fila ainda não terá recebido a batida. É contraexemplo condicional pelo código; incidência em produção: **[nao verificado]**. (`app/static/pwa.js:86`, `:100`, `:170`, `:175`)

O teste de navegador não detecta B1: substitui o escritor por uma função que marca “fila” e resolve imediatamente. Também conserva `/hora/` respondendo na sonda, inclusive nos cenários classificados como offline. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:106`, `:118`)

Há outras limitações de entrega já presentes: sem PWA não existe fila; triagem online usa submit nativo; V2 enfileira rejeição de rede, mas respostas HTTP de erro apenas mostram erro e liberam nova tentativa. Chegada real ao servidor nesses caminhos: **[nao verificado]**. (`app/templates/ponto/triagem/apto.html:241`, `:289`; `app/templates/ponto/bater_ponto_v2.html:313`, `:316`, `:331`)

**4. Template apagado**

Confirmo o censo estático sem referência viva a `ponto/bater_ponto.html`: a view seleciona V2 ou templates de triagem; os nomes de URL `bater_ponto`/`bater_ponto_v2` continuam sendo rotas, não referências ao arquivo removido. (`app/ponto/views.py:225`, `:356`; `app/ponto/urls.py:11`)

As três referências textuais restantes são comentário de exclusão, comentário histórico do M6 e ticket “MATAR”. O verificador de URLs já o retirou da lista. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`; `app/docs/TICKETS.md:693`; `app/tools/check_frontend_urls.py:25`)

**5. Escritores, leitores e significados**

| Campo/valor | Quem escreve → quem lê |
|---|---|
| `{stream, aviso}` / `{foto, aviso}` | PWA produz; triagem/V2 leem para controles, aviso e transporte. Ausência é `null`; aviso é texto ou `null`. (`app/static/pwa.js:133`, `:151`, `:166`; `app/templates/ponto/triagem/apto.html:155`, `:233`; `app/templates/ponto/bater_ponto_v2.html:274`) |
| Foto anterior/arquivo | Input da câmera do sistema escreve `_fotoBlob`; captura preserva `anterior` quando necessário; V2 envia ou enfileira. (`app/templates/ponto/bater_ponto_v2.html:252`, `:273`, `:305`, `:331`; `app/static/pwa.js:166`) |
| `fila.foto = null` | `registrarBatidaOffline` é o escritor IndexedDB; sync lê e omite arquivo. Ausência já pertence ao contrato da fila. (`app/static/pwa.js:175`, `:180`, `:194`, `:214`) |
| POST sem foto | Triagem/V2/sync deixam de anexar; APIs leem com `request.FILES.get`. A criação não exige foto. (`app/templates/ponto/triagem/apto.html:233`; `app/templates/ponto/bater_ponto_v2.html:305`; `app/static/pwa.js:214`; `app/api/views.py:436`, `:666`; `app/api/views_core.py:423`, `:658`) |
| Foto em sessão | View escreve base64/nome; serviço recupera, converte e escolhe entre sessão e upload. (`app/ponto/views.py:324`; `app/ponto/services/triagem_batida.py:512`, `:748`) |
| Foto persistente | `anexar_foto` escreve `Batida.foto` pela porta e, conforme `registro`, `FotoBatida`. Modelo aceita nulo; retro lê ambas as fontes. (`app/ponto/registro_batida.py:296`; `app/ponto/models.py:78`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:75`) |
| `foto_ausente` | APIs/retro entregam motivo/contexto à disputa; catálogo pede confirmação de presença; upload posterior apara/fecha disputas vinculadas à batida. É **ausência cobrável**, não booleano físico da foto. (`app/api/views.py:464`; `app/api/views_core.py:451`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`; `app/chamados/catalogo/perguntas.py:93`; `app/api/foto_recebida.py:57`) |
| Sync/remoção | PWA lê fila, envia e remove após sucesso ou HTTP 400; rede/5xx conservam item. SW delega às páginas; sem página, notifica. (`app/static/pwa.js:188`, `:220`, `:228`, `:239`; `app/static/sw.js:112`) |

Nos leitores examinados, ausência não exige foto sempre presente. Quando a foto da fila é truthy, o multipart pressupõe Blob/File. Upload separado exige foto para **anexar**, sem transformar isso em exigência para criar batida. Consumidores externos à cópia: **[nao verificado]**. (`app/static/pwa.js:214`; `app/ponto/models.py:78`; `app/api/views.py:850`; `app/api/views_core.py:835`)

**6. Checklist das classes**

| Classe | Resultado |
|---|---|
| Segundo juiz duplicando registrado | Não identifiquei duplicação da cobrança: servidor decide prazo cobrável; PWA limita captura e devolve resultado/aviso. (`app/core/juizes.py:1192`; `app/ponto/registro_batida.py:80`; `app/static/pwa.js:122`, `:133`) |
| Segundo escritor fora da porta | Cura usa escritor existente da fila; servidor continua criando/anexando pelas portas existentes. (`app/static/pwa.js:169`; `app/api/views.py:666`, `:784`; `app/api/views_core.py:658`, `:779`; `app/ponto/services/triagem_batida.py:701`, `:751`) |
| Relógio fora da borda | Captura usa timer de duração; juiz servidor recebe duração. Permanecem relógios próprios de feedback e timestamp da fila, sem unificação nova nesta cura. (`app/static/pwa.js:127`, `:171`; `app/ponto/registro_batida.py:80`; `app/templates/ponto/triagem/apto.html:290`; `app/templates/ponto/bater_ponto_v2.html:332`) |
| Derivação paralela | Câmera/conversão centralizadas; textos próprios de obrigatoriedade permanecem. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:497`; `app/templates/ponto/triagem/apto.html:124`; `app/templates/ponto/bater_ponto_v2.html:100`) |
| Selo/RED sem mordida suficiente | **B2:** aviso “visto” é busca em `body.innerHTML`; aceita aviso somente em `title`. `_morre` executa apenas a mutação, sem exigir controle original verde no mesmo ensaio. Assim, aviso invisível pode satisfazer o oráculo e falha anterior pode aparentar matar mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:94`, `:270`, `:387`, `:398`; `app/templates/ponto/bater_ponto_v2.html:249`) |
| Campo com dois significados | `foto_ausente` mantém sentido de cobrança; `foto` mantém Blob/File/null nas bordas. Não confundir ausência física imediata com ausência vencida do APK. (`app/ponto/registro_batida.py:80`; `app/static/pwa.js:166`, `:180`, `:214`) |
| Duas cascas | Ambas incluem PWA e diálogo comum; teste compõe cada casca real. Porém força essa composição pelo loader: não prova seleção real da casca. Os dois kernels API preservam o mesmo predicado de selfie. (`app/templates/base.html:20`, `:388`; `app/templates/base_app.html:20`, `:145`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:185`, `:333`; `app/api/views.py:461`; `app/api/views_core.py:448`) |

Existe ainda uma suspeita sobre o controle “câmera ok”: a sonda fornece `MediaStream()` vazio e falsifica dimensões, enquanto `drawImage(video)` permanece real. Se esse desenho falhar, o controle perde foto antes da conversão; resultado efetivo no Chromium: **[nao verificado]**. Não uso essa suspeita como bloqueante comprovado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:75`, `:82`; `app/static/pwa.js:160`)

REPROVADO — bloqueantes: **B1**, espera sem teto antes de gravar a fila, encoberta pela substituição do escritor no smoke (`app/static/pwa.js:86`, `:170`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:118`); **B2**, prova de aviso aceita atributo invisível e mutantes não exigem controle verde acoplado (`app/ponto/tests/test_selfie_nao_barra_chromium.py:94`, `:387`; `app/templates/ponto/bater_ponto_v2.html:249`).

## 11/11 13:44 · sessao 530cb5ae113b · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE — alternativa (a).** O selo aceita uma fonte fora das previstas pelo aval. (`bin/mapa20_valida.py:73`; `aval_mapa20_pronto_x_de_n_1111.txt:1`, anexo, item 1.)

1. **Critério aplicado:** a célula deve conter ao menos uma correspondência de `arquivo:linha`; todas as correspondências devem ter caminho relativo sem componente `..`, arquivo legível em UTF-8 e linha existente, ≥ 1 e não vazia. Não verifica se é uma fonte autorizada nem se contém o PRONTO da obra. (`bin/mapa20_valida.py:51`; `bin/mapa20_valida.py:73–90`.)

2. **Entrada concreta aceita:** `X de N = 1 de 2`, **fonte = `bin/relato.sh:2`**, falta = `item 2: prova`. A linha citada é apenas o comentário “PUBLICA o relato e os indices…”. Essa é a entrada padrão do teste “verde”, esperado rc 0; reproduzida em memória, passou com zero problemas. A aceitação da fonte termina em `return ''`; sem problemas, o processo retorna 0. (`bin/relato.sh:2`; `bin/tests/test_mapa20_valida.sh:27`; `bin/tests/test_mapa20_valida.sh:42`; `bin/mapa20_valida.py:90`; `bin/mapa20_valida.py:225`.)

3. **Leitura literal:** o aval define “N = itens numerados do PRONTO da obra (fonte: o aval da obra, app/docs/TICKETS.md ou BACKLOG, com linha citada)” e exige rc 1 “se N sem fonte citada”. O item 4 usa a fonte definida no item 1; não cabe interpretá-lo como autorização para qualquer comentário existente. O código substitui essa condição pela mera existência da referência, deixando passar a entrada acima. (`aval_mapa20_pronto_x_de_n_1111.txt:1`, anexo, itens 1 e 4; `bin/mapa20_valida.py:73–90`.)

## 11/11 13:41 · sessao f25cd63cd977 · - · - · origem=Ronald (tmux)

Auditoria estática dos testes, sem execução: [nao verificado]

- **Ma agora tem mordida prevista:** injeta `await` infinito em `garantirRequisitos`, exercita V2 online/offline e exige “NAO chegou”. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:446`)
- **Mb agora tem mordida prevista:** troca aviso por `false`, exercita V2 online e apto offline e exige ausência do aviso. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:451`)
- **Demais regressões nomeadas:** M1 botão offline oculto (:403), M2 veto sem foto (:408), M3 online não libera (:413), M4 espera callback (:418), M5 texto paralelo (:424), M7 exceção no juiz (:429, :434), M8 juiz sem guarda (:441), Mc `throw` sem foto (:456), Md teto removido (:461). M6 é declarado eliminado junto com o legado (:377). Todas as referências deste item são `app/ponto/tests/test_selfie_nao_barra_chromium.py`.
- **Duas cascas estão efetivamente compostas e exercitadas:** loader troca `base.html` pelo conteúdo da casca escolhida; matriz percorre cascas × rotas × câmeras. Mutantes usam somente `base_app.html`. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:185`, `:333`, `:356`, `:391`)
- **Persistência não é testada pelo smoke:** a sonda substitui `registrarBatidaOffline` por função que marca alcance e retorna `Promise.resolve()`. Portanto esse verde prova chamada à fila, não escrita IndexedDB, sync ou chegada ao servidor. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:116`)
- **Controle verde não está acoplado aos mutantes:** `_morre` só executa fonte mutada; exige alguma falha e regex. O original tem testes separados. Se o original já falhar pelo mesmo motivo, o mutante pode parecer morto sem causalidade demonstrada. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:387`, `:398`, `:356`)
- **Oráculo de aviso aceita conteúdo invisível:** `temAviso()` procura texto em `body.innerHTML`; V2 grava o aviso em `title`. Esse atributo basta para “aviso visto” e “aviso ao liberar”, sem toast visível. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:94`, `:131`, `:163`, `:270`; `app/templates/ponto/bater_ponto_v2.html:249`)
- **Possível falso RED que exige execução para confirmar:** câmera “ok” devolve `new MediaStream()` sem tracks; a sonda falsifica dimensões, mas deixa `drawImage(video)` real. Se o canvas rejeitar vídeo sem frame, até o controle “ok” perde foto e os mutantes podem morrer pelo motivo preexistente. Resultado no Chromium: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:75`, `:82`; `app/static/pwa.js:160`)
- **`test_l030_*` conserva fiscalização estática de GPS:** o teste renderizado aplica o mesmo oráculo de tokens; fiscalização selfie foi retirada e apontada ao Chromium. (`app/ponto/tests/test_l030_templates_renderizados.py:14`, `:37`; `app/ponto/tests/test_l030_forma_js.py:117`)

Execução efetiva de qualquer teste ou mutante: **[nao verificado]**. [nao verificado]

## 11/11 13:41 · sessao b08d61d550d2 · - · - · origem=Ronald (tmux)

O censo não encontrou uso vivo do template apagado `bater_ponto.html`: a view renderiza V2 ou o seletor de triagem; as rotas conservam nomes `bater_ponto`/`bater_ponto_v2`, que não são referências ao arquivo removido. (`app/ponto/views.py:225`, `app/ponto/views.py:356`; `app/ponto/urls.py:11`)

As referências restantes ao arquivo removido são comentário de exclusão, comentário histórico dos mutantes e ticket “MATAR”; o verificador de URLs já não o inclui. (`app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:379`; `app/docs/TICKETS.md:693`; `app/tools/check_frontend_urls.py:25`)

No front de batida, as chamadas reais a câmera/conversão estão concentradas em PWA; a outra ocorrência é onboarding, declarado como foto de referência no censo fechado. (`app/static/pwa.js:137`, `app/static/pwa.js:148`, `app/static/pwa.js:163`; `app/templates/core/onboarding.html:154`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:476`)

As telas continuam interpretando a resposta para apresentação e transporte, mas não retornam recusando por foto nula: triagem libera sem stream, aguarda captura protegida e envia; offline mantém confirmar e entrega Blob/null à fila; V2 aguarda requisitos e anexa somente se houver foto. (`app/templates/ponto/triagem/apto.html:154`, `app/templates/ponto/triagem/apto.html:231`, `app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:273`, `app/templates/ponto/bater_ponto_v2.html:297`, `app/templates/ponto/bater_ponto_v2.html:305`)

Persistem textos contraditórios próprios: “Tire sua selfie para liberar o ponto”, “Selfie obrigatória”, o texto de refazer e “Tire a selfie para registrar”; não constituem veto técnico, mas a linguagem de obrigatoriedade não foi centralizada. (`app/templates/ponto/triagem/apto.html:66`, `app/templates/ponto/triagem/apto.html:124`, `app/templates/ponto/triagem/apto.html:194`, `app/templates/ponto/triagem/apto.html:250`; `app/templates/ponto/bater_ponto_v2.html:100`)

As duas cascas carregam o mesmo PWA e diálogo global; diferem na estrutura de navegação e outros scripts, sem outra política de câmera localizada. Os testes trocam o loader de `base.html` pela casca real, portanto verificam composição nas duas cascas; seleção real da casca e execução dos testes: [nao verificado]. (`app/templates/base.html:20`, `app/templates/base.html:385`, `app/templates/base.html:388`; `app/templates/base_app.html:20`, `app/templates/base_app.html:142`, `app/templates/base_app.html:145`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:185`)

A confirmação de turno aberto continua podendo enviar formulário sem foto, fora do juiz visual da câmera; não contém captura nem fila local. (`app/templates/ponto/triagem/turno_confirmar.html:8`)

Continuam riscos fora do teto da câmera: V2 não enfileira HTTP de erro resolvido, apenas rejeição de `fetch`; triagem online usa submit nativo; todos os caminhos offline dependem do escritor PWA. (`app/templates/ponto/bater_ponto_v2.html:307`, `app/templates/ponto/bater_ponto_v2.html:313`, `app/templates/ponto/bater_ponto_v2.html:316`; `app/templates/ponto/triagem/apto.html:241`, `app/templates/ponto/triagem/apto.html:289`; `app/templates/ponto/bater_ponto_v2.html:331`)

## 11/11 13:41 · sessao 8bdba0451286 · PROPORCAO-POR-RISCO · verificador · origem=Code

A v3 fecha vários exemplos da v2, mas ainda permite escrita com **rc 0 deduzido por leitura**. A seleção continua dependendo do nome imediato da chamada e da interseção dos nós reconhecidos com linhas adicionadas. (`bin/selo_proporcao.py:64`, `bin/selo_proporcao.py:69`, `bin/selo_proporcao.py:130`, `bin/selo_proporcao.py:132`)

Não executei o selo, testes ou exemplos; os retornos abaixo são deduções. Execução efetiva, efeitos persistidos e situação em produção: **[nao verificado]**. [nao verificado]

**1. Fechamento dos achados da volta 2**

| Achado da v2 | Situação na v3 e cura | Caso que o morde |
|---|---|---|
| Continuação `()` sobre atributo antigo | **Fecha o exemplo direto:** a `Call` reconhecida cobre sua extensão inteira. (`bin/selo_proporcao.py:69`, `bin/selo_proporcao.py:132`) | `continuacao_sobre_attr_antigo`. (`bin/tests/test_selo_proporcao.sh:60`) |
| Argumento novo em chamada escritora antiga | **Fecha quando a função é `Name`/`Attribute` reconhecido.** A chamada aninhada continua escapando, demonstrada abaixo. (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`) | `argumento_novo_em_update_antigo`, sobre preparo antigo. (`bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:61`) |
| `exec("obj.save()")` | **Fecha o literal direto**, por `OPACO`. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:82`) | `exec_string`. (`bin/tests/test_selo_proporcao.sh:62`) |
| `eval("obj.save()")` | **Fecha o literal direto**, pelo mesmo mecanismo. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`) | `eval_string`. (`bin/tests/test_selo_proporcao.sh:63`) |
| Decorador da v2 com `@eval(...)` | **Fecha aquele exemplo**, pois contém `eval`; não fecha decoradores em geral. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:82`) | `eval_string` cobre o gatilho, mas não reproduz o decorador. (`bin/tests/test_selo_proporcao.sh:63`) |
| Nome de método em f-string | **Fecha o exemplo:** segundo argumento de `getattr` não é `Constant`. (`bin/selo_proporcao.py:73`) | `fstring_nome`. (`bin/tests/test_selo_proporcao.sh:66`) |
| `setattr` instala alias escritor | **Fecha o exemplo direto**, conservadoramente pelo próprio `setattr`. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:71`) | `setattr_alias`; não instala nem invoca um escritor real, mas isola esse gatilho. (`bin/tests/test_selo_proporcao.sh:64`) |
| QuerySet com método calculado | **Fecha o exemplo**, sem precisar resolver o tipo de `qs`. (`bin/selo_proporcao.py:73`) | `getattr_calculado`. (`bin/tests/test_selo_proporcao.sh:65`) |
| Alias novo de cursor e SQL fragmentado | **Fecha aquele diff** pela referência nova a `.execute`. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:80`) | `cursor_execute` contém somente a referência, suficiente para provar esse mecanismo. (`bin/tests/test_selo_proporcao.sh:68`) |
| SQL na f-string `UPDATE {tabela} SET ...` | **Fecha:** substitui expressões por ` x ` antes de procurar SQL; `.execute` também passa a ser reconhecido. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:76`) | `sql_fstring` isola a f-string, sem cursor. (`bin/tests/test_selo_proporcao.sh:67`) |
| Payload `.pyw` carregado por `runpy.run_path` | **Fecha o executor apresentado**, por `run_path`; `.pyw` continua fora da análise. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:124`) | `import_dinamico` testa `import_module`, **não** `run_path` nem o payload `.pyw`. (`bin/tests/test_selo_proporcao.sh:70`) |
| Arquivo `.sql` e executor | **Fecha o exemplo:** destino `.sql` é A, e `.execute` novo também seria detectado. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:122`) | `arquivo_sql` prova a regra por caminho. (`bin/tests/test_selo_proporcao.sh:69`) |
| Escritor novo em teste importado por linha antiga | **Não fecha; passou a limite declarado.** O arquivo continua excluído. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:124`) | Nenhum caso reproduz essa costura; `prod_importa_teste` adiciona somente o import. (`bin/tests/test_selo_proporcao.sh:49`) |
| `import *` via reexportação antiga de teste | **Não fecha e não está explicitamente declarado no docstring.** Import de módulo comum não revela a origem reexportada. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:124`) | Sem caso correspondente. (`bin/tests/test_selo_proporcao.sh:29`, `bin/tests/test_selo_proporcao.sh:70`) |
| Template chama método escritor | **Não fecha; limite declarado.** HTML continua excluído. (`bin/selo_proporcao.py:23`, `bin/selo_proporcao.py:124`) | `tela_pura` só muda texto; não chama método. (`bin/tests/test_selo_proporcao.sh:29`) |
| Argumentos excedentes | **Fecha:** exige três ou quatro argumentos. (`bin/selo_proporcao.py:94`) | `arg_a_mais`. (`bin/tests/test_selo_proporcao.sh:78`) |
| Retorno de `bash -e` não afirmado | **Fecha a ausência da asserção**, mantendo problema no preparo especial. (`bin/tests/test_selo_proporcao.sh:76`, `bin/tests/test_selo_proporcao.sh:77`) | `rc_bash_e` agora exige exatamente 1. (`bin/tests/test_selo_proporcao.sh:77`) |
| Normalização universal de erros | **Não fecha:** wrapper permanece um `exec`, sem normalização de falhas de lançamento. (`bin/selo_proporcao.sh:4`) | Sem caso de Python ausente, execução recusada ou sinal. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:79`) |

As curas anteriores permanecem: oito métodos ORM, espaço após ponto, alias por referência, `getattr` literal, `#` em string, signals, caminhos NUL, migration por rename, `--text` e revisão iniciada por opção. Os mecanismos estão em `bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:106`, `bin/selo_proporcao.py:118` e `bin/selo_proporcao.py:56`; os respectivos casos estão em `bin/tests/test_selo_proporcao.sh:30`, `bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:35`, `bin/tests/test_selo_proporcao.sh:36`, `bin/tests/test_selo_proporcao.sh:37`, `bin/tests/test_selo_proporcao.sh:39`, `bin/tests/test_selo_proporcao.sh:44`, `bin/tests/test_selo_proporcao.sh:46`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:48` e `bin/tests/test_selo_proporcao.sh:73`.

Continuam sem reprodução específica: `(obj.save)()`, caminho com tabulação e conteúdo falso `+++ /dev/null`; os casos próximos não têm essas formas. (`bin/tests/test_selo_proporcao.sh:33`, `bin/tests/test_selo_proporcao.sh:34`, `bin/tests/test_selo_proporcao.sh:47`, `bin/tests/test_selo_proporcao.sh:48`)

Quanto ao custo dos limites:

- **Helper que já grava:** está compreensível: chamada comum pode passar, e o texto atribui a detecção ao smoke. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:26`)
- **Template escritor:** a exclusão está explícita, mas deveria dizer diretamente que renderizar o template pode causar escrita real não detectada; existe método `rebaixar()` que chega a `self.save()`. Renderização e objeto no contexto: **[nao verificado]**. (`bin/selo_proporcao.py:23`, `app/chamados/models.py:488`, `app/chamados/models.py:498`)
- **Teste importado por linha antiga:** explica a exclusão técnica, mas chamar isso de “custo do aval, não fuga” omite a consequência: **escrita nova no corpo do teste pode alcançar produção com B verde**. Isso é mais amplo que apenas chamar helper que já escrevia. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:124`)
- **`import *`:** falta declaração expressa sobre reexportação e seu custo. Aceitação específica desse limite pelo Ronald: **[nao verificado]**. (`bin/selo_proporcao.py:22`, `bin/selo_proporcao.py:85`)

**2. Novos contraexemplos à v3**

Cada diff abaixo é independente e declarado B. Os `.py` ficam fora dos caminhos especiais e de testes; os objetos mencionados precisam existir no contexto. Os **rc 0 são deduzidos** das regras de seleção; funcionamento desses contextos em produção: **[nao verificado]**. (`bin/selo_proporcao.py:118`, `bin/selo_proporcao.py:124`, `bin/selo_proporcao.py:132`, `bin/selo_proporcao.py:145`)

**a) Chamada aninhada, alterando somente argumento.** Em um `ModelForm`, o exemplo muda a opção de persistência; presença desse formulário na aplicação: **[nao verificado]**.

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,3 +1,3 @@
 getattr(form, "save")(
-    commit=False, [nao verificado]
+    commit=True, [nao verificado]
 )
```

O `Call` externo cruza a linha adicionada, mas sua função é outro `Call`; `nome_chamado` devolve `None`. O `getattr` e a constante `"save"` estão inteiramente na linha antiga. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:132`)

**b) Lambda antiga passa a ser executada.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,2 +1,2 @@
 (lambda: obj.save()
-)
+)()
```

A chamada tem `Lambda` como função; o atributo escritor permanece na linha antiga. A alteração passa a invocar a expressão que antes apenas construía a função. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:132`)

**c) Decorador passa a executar a lambda escritora.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,5 +1,5 @@
 @(
     lambda f, acao=(lambda: obj.save()):
-        f
+        (acao(), f)[1]
 )
 def tela(): pass
```

A referência `.save` está na linha antiga; a chamada nova chama `acao`, nome comum. Nem as lambdas nem a tupla são classificadas como escrita. **Rc 0.** (`bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:79`, `bin/selo_proporcao.py:90`, `bin/selo_proporcao.py:132`)

**d) `__dict__` e `vars()`, com nome fragmentado.** São alternativas independentes para um contexto com `Model` e `obj` disponíveis. Disponibilidade concreta: **[nao verificado]**.

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+Model.__dict__["sa" + "ve"](obj)
```

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+vars(Model)["sa" + "ve"](obj)
```

`__dict__`, `vars` e as constantes separadas não pertencem aos conjuntos; a função externa é um `Subscript`. **Rc 0 em ambas.** Usar `"save"` inteiro seria detectado. (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:83`)

**e) `operator.methodcaller`.**

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+import operator
+operator.methodcaller("sa" + "ve")(obj)
```

`methodcaller` não está nos conjuntos, o nome é fragmentado e a chamada externa tem outra `Call` como função. **Rc 0.** (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`)

**f) Reflexão por `type(...)`.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1,0 +2 @@
+type(obj).__getattribute__(obj, "sa" + "ve")()
```

`type` e `__getattribute__` não são reconhecidos; a chamada final tem uma `Call` como função. **Rc 0.** (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:65`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:83`)

**g) Alias de `exec` por import.**

```diff
--- /dev/null
+++ b/app/x/novo.py
@@ -0,0 +1,2 @@
+from builtins import exec as rodar
+rodar("obj.save()")
```

O import só verifica origens de teste/signals; não aplica `OPACO` aos nomes importados. A chamada vê `rodar`, e a string `"obj.save()"` não é um nome exato nem SQL. **Rc 0.** Em contraste, `rodar = exec` seria detectado pelo `Name exec`. (`bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:82`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:85`)

**h) `.sql` renomeado para sufixo ignorado.**

```diff
diff --git a/app/x/q.sql b/app/x/q.txt
rename from app/x/q.sql
rename to app/x/q.txt
```

Considere `q.sql` contendo escrita e um executor antigo que só consome `q.txt` quando esse arquivo existe. A renomeação pode ativar o payload sem mudar Python; existência desse executor: **[nao verificado]**.

O selo considera o destino; `.txt` é ignorado. Mesmo que Git reporte deleção e adição, a deleção é ignorada e o destino continua fora. **Rc 0.** Rename mantendo destino `.sql` dá **rc 1**, portanto não constitui escape. (`bin/selo_proporcao.py:110`, `bin/selo_proporcao.py:116`, `bin/selo_proporcao.py:122`, `bin/selo_proporcao.py:124`)

**i) F-string sem SQL nas partes constantes.**

```diff
--- a/app/x/views.py
+++ b/app/x/views.py
@@ -1 +1,2 @@
 run = cur.execute
+run(f"{'DE' + 'LETE'} {'FR' + 'OM'} tabela")
```

A referência `.execute` é antiga; `run` não é resolvido como alias. O `JoinedStr` vira aproximadamente ` x   x  tabela`, e nenhuma constante contém SQL completo. **Rc 0.** (`bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:76`, `bin/selo_proporcao.py:83`, `bin/selo_proporcao.py:132`)

**3. Falsos positivos e controles**

| Código adicionado em `.py` comum | Rc B deduzido |
|---|---|
| `getattr(obj, "x")` | **0**: segundo argumento continua sendo `Constant`, sem nome reconhecido. (`bin/selo_proporcao.py:73`, `bin/selo_proporcao.py:83`) |
| `getattr(obj, nome)` usado apenas para leitura | **1, novo falso positivo:** nome calculado basta. (`bin/selo_proporcao.py:73`) |
| `motor_de_template.execute()` sem persistência | **1, novo falso positivo:** não resolve o tipo do receptor. (`bin/selo_proporcao.py:36`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`) |
| `texto = f"UPDATE {nome} SET rótulo"` apenas para exibição | **1, novo falso positivo:** o texto montado satisfaz a regex SQL. (`bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:76`) |
| `texto = f"update {nome}"` | **0**: não contém `SET`, e a constante `"update "` não é o nome exato `"update"`. (`bin/selo_proporcao.py:41`, `bin/selo_proporcao.py:76`, `bin/selo_proporcao.py:83`) |
| `texto = f"update{nome}"` | **1, já ocorria na v2:** a constante exata `"update"` basta. (`bin/selo_proporcao.py:35`, `bin/selo_proporcao.py:83`) |
| Palavra `update` em template `.html` | **0**: HTML permanece fora. (`bin/selo_proporcao.py:124`) |
| Referência a `exec`, `eval` ou `compile` sem execução | **1:** `Name` em `OPACO` basta. (`bin/selo_proporcao.py:38`, `bin/selo_proporcao.py:82`) |
| `.sql` contendo apenas `SELECT`, comentário ou texto | **1:** regra incondicional por caminho. (`bin/selo_proporcao.py:122`) |

Pelo critério fornecido, esses falsos positivos que empurram para A não são bloqueantes; o código também declara essa direção conservadora. (`bin/selo_proporcao.py:27`)

**4. Retornos**

- **Argumento a mais agora dá 2**, inclusive na chamada testada com `repo extra`. (`bin/selo_proporcao.py:94`, `bin/tests/test_selo_proporcao.sh:78`)
- **Argumentos inválidos, revisão inválida, Git não zero, parse inválido e exceções ordinárias alcançadas pela fronteira Python dão 2.** (`bin/selo_proporcao.py:46`, `bin/selo_proporcao.py:94`, `bin/selo_proporcao.py:102`, `bin/selo_proporcao.py:134`, `bin/selo_proporcao.py:152`)
- **Não são todos os caminhos de erro:** falha ao lançar Python pode preservar 126/127; término por sinal não passa pela captura de `Exception`. Ocorrência concreta: **[nao verificado]**. O wrapper permanece sem normalização. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:152`)
- **`bash -e` recebe o rc 1 inteiro**, se o selo o produzir: `exec` preserva o retorno, e o shell externo captura a saída do shell com `-e`. O teste agora afirma exatamente 1. (`bin/selo_proporcao.sh:4`, `bin/selo_proporcao.py:143`, `bin/tests/test_selo_proporcao.sh:77`)
- **Rc 1 continua significando classificação conservadora**, incluindo falsos positivos; não comprova escrita efetiva. (`bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:143`)

**5. Qualidade dos casos**

| Caso ou anúncio | Avaliação |
|---|---|
| `so_remocao` | **Corrigido:** base contém `a = 1` e `o.save()`; `sed` só remove a segunda linha. O arquivo permanece Python válido, sem linha adicionada. (`bin/tests/test_selo_proporcao.sh:16`, `bin/tests/test_selo_proporcao.sh:51`) |
| `apagou_migration` | **Corrigido:** migration existe na base e é realmente removida. Exercita o ramo `D`. (`bin/tests/test_selo_proporcao.sh:15`, `bin/tests/test_selo_proporcao.sh:56`, `bin/selo_proporcao.py:116`) |
| `sql_escrita` | **Ganhou duplicidade na v3:** `.execute` e a constante SQL bastam independentemente. Não isola a regex SQL. (`bin/tests/test_selo_proporcao.sh:38`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`, `bin/selo_proporcao.py:83`) |
| `signal_nome` | **Duplicidade permanece:** módulo terminado em `signals` ou nome importado `post_save` bastam independentemente. (`bin/tests/test_selo_proporcao.sh:40`, `bin/selo_proporcao.py:88`) |
| Chamadas diretas ORM | Agora podem disparar por `Attribute` e por `Call`; não isolam a nova regra de extensão. Os dois casos com chamada antiga isolam melhor essa cura. (`bin/tests/test_selo_proporcao.sh:31`, `bin/tests/test_selo_proporcao.sh:60`, `bin/tests/test_selo_proporcao.sh:61`, `bin/selo_proporcao.py:71`, `bin/selo_proporcao.py:80`) |
| `command_alias` | Continua provando atributo genérico `.create`, sem invocar alias e sem provar mecanismo específico de command. (`bin/tests/test_selo_proporcao.sh:45`, `bin/selo_proporcao.py:80`) |
| `prod_importa_teste` | Continua sem criar módulo escritor: prova a regra conservadora do import, não a costura produção→teste→escrita. (`bin/tests/test_selo_proporcao.sh:49`) |
| Preparo comum | Checkout, resultado final do script, commit e mudança de HEAD são conferidos; comandos anteriores separados por `;` ainda podem falhar e ser mascarados pelo último. (`bin/tests/test_selo_proporcao.sh:22`, `bin/tests/test_selo_proporcao.sh:23`, `bin/tests/test_selo_proporcao.sh:24`, `bin/tests/test_selo_proporcao.sh:46`) |
| Preparo de `rc_bash_e` | **Continua sem conferência:** checkout, escrita, add e commit estão encadeados por `;`. A asserção do rc existe, mas pode julgar outra árvore se esse preparo falhar. (`bin/tests/test_selo_proporcao.sh:76`, `bin/tests/test_selo_proporcao.sh:77`) |
| “Cada escape” e “um mecanismo por caso” | O anúncio continua mais forte que a cobertura: persistem duplicidades e ausências específicas descritas acima. (`bin/tests/test_selo_proporcao.sh:3`, `bin/tests/test_selo_proporcao.sh:4`, `bin/tests/test_selo_proporcao.sh:38`, `bin/tests/test_selo_proporcao.sh:40`) |

**6. Quem escreve, chama e lê**

- **Produz o veredito:** Python imprime achados e retorna 0/1/2; o wrapper executa Python. (`bin/selo_proporcao.py:137`, `bin/selo_proporcao.py:141`, `bin/selo_proporcao.py:145`, `bin/selo_proporcao.py:152`, `bin/selo_proporcao.sh:4`)
- **Chamador concreto encontrado:** `test_selo_proporcao.sh` define `SELO`, chama sobre repositório sintético, captura `$?` e compara com o esperado. (`bin/tests/test_selo_proporcao.sh:8`, `bin/tests/test_selo_proporcao.sh:10`, `bin/tests/test_selo_proporcao.sh:25`, `bin/tests/test_selo_proporcao.sh:26`)
- **Outros leitores diretos:** as verificações de argumentos/revisões e as duas verificações de propagação do rc. (`bin/tests/test_selo_proporcao.sh:71`, `bin/tests/test_selo_proporcao.sh:77`, `bin/tests/test_selo_proporcao.sh:78`, `bin/tests/test_selo_proporcao.sh:79`)
- **Leitor indireto:** a régua chama o teste pelo glob e transforma falha do teste em rc 1 da régua. Isso testa cenários sintéticos; não classifica o diff real de pouso. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/regua.sh:143`, `bin/tests/test_selo_proporcao.sh:25`)
- **Consumidor do pouso real:** o contrato exige que merge+deploy espere o rc; chamada operacional, leitor efetivo e registro no RELATO continuam **[nao verificado]**. (`contrato.txt:2`, `contrato.txt:3`, anexo) [nao verificado]
- **CORTES/RELATO:** não encontrei o registro específico PROPORCAO-POR-RISCO na busca textual desses documentos; atendimento desses itens do aval: **[nao verificado]**.
- **Pre-push:** o código declara que não roda o loop de host e chama outros selos individualmente; uso deste selo pelo hook instalado: **[nao verifi

[truncado em 20000 de 21776 caracteres]

_3907 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
