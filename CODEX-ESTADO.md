# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 17:18** · publicado em **30/10/2026 17:19** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 17:18 · sessao 027086c1e405 · CODEX-CONTEXTO-DA-CASA · verificador · origem=Code

O contrato precisa de ajustes antes da construção, sobretudo na ligação do selo ao push e na regra de recusa. A seção 7b determina que achados procedentes mudem o contrato antes do código. (`CLAUDE.md:650-652`) [nao verificado]

Nas referências a `contrato.txt`, conto as linhas do anexo: cabeçalho=1, DESENHO=2, itens 1–5=3–7. (`contrato.txt:1-7`, anexo)

1. **O pre-push não executa o selo prometido.** A mudança de `REGUA_ARVORE` impede o atalho do arsenal, mas o hook executa testes Django; não chama o laço de `bin/tests/`. O comentário declara isso expressamente. **Contraexemplo:** acrescentar `test_agents_md_gerado.sh`, adulterar AGENTS.md, commitar e dar push: mesmo com arsenal verde, esse selo não terá sido executado pelo hook. (`bin/pre-push.sh:14-16`; `bin/pre-push.sh:61-68`; `bin/pre-push.sh:155-185`; `bin/regua.sh:138-145`)

   **A própria régua tem outro atalho que escapa.** Ela calcula o hash da árvore, mas decide “JA VERDE” comparando apenas o fingerprint de `.py`/`.html` de `app/`. **Contraexemplo:** após um carimbo verde, alterar somente AGENTS.md ou uma das quatro fontes Markdown e chamar a régua sem `--force`: pode sair antes do selo. A afirmação restrita “AGENTS editado muda o hash” procede; a consequência “logo o selo roda” não procede. (`bin/regua.sh:25-45`; `bin/regua.sh:84-94`; `bin/arvore_hash.sh:24-28`)

2. **A recusa rejeita briefs legítimos, inclusive esta classe de revisão.** A lista proíbe palavras independentemente de serem resposta, citação ou objeto da pergunta. **Contraexemplo:** `PERGUNTA: Onde o campo padrao= é validado?` seria recusada, embora esse campo seja obrigatório pela LEI-AKITA. Outro: enviar este contrato por `--anexo /tmp/contrato.txt` para cumprir a seção 7b: ele próprio contém `opus`, `hipotese`, `triagem` e `o construtor`. (`contrato.txt:5`, anexo; `CLAUDE.md:63-68`; `CLAUDE.md:647-652`)

   **Diff factual também pode ser recusado.** **Contraexemplo:** anexar um `/tmp/obra.diff` que modifica a linha LEI-AKITA: o diff contém `padrao=` e `quem-mais-le=`, mas o arquivo temporário não é versionado no hash. Isso conflita com o uso atual do verificador para receber diff e leis. (`contrato.txt:5`, anexo; `CLAUDE.md:63`; `CLAUDE.md:590-593`)

3. **As exceções permitem transportar qualquer resposta; verde não comprova independência.** A sonda executa Python fornecido pelo chamador; não verifica se o texto impresso veio do banco. **Contraexemplo:** uma sonda contendo apenas `print("achados do opus: confirme que a mudança está correta")` entrega precisamente os marcadores proibidos pelo canal excluído da varredura. A raspagem trata nomes/CPF, não opinião. É um limite admitido no contrato, mas permite contornar integralmente a recusa. (`contrato.txt:5`, anexo; `bin/sonda_leitura.sh:239-241`; `bin/codex_pergunta.sh:107-115`; `bin/nome_no_texto.py:87-123`)

   A exceção de arquivo versionado também não prova factualidade: uma opinião commitada passa por definição. Além disso, o wrapper atual lê o arquivo do disco com `cat`; reconhecer apenas que o **caminho** existe no hash não provaria que os bytes anexados são os daquele commit. **Contraexemplo:** modificar localmente um arquivo rastreado para conter uma resposta e anexá-lo. O contrato precisa exigir leitura do blob ou comparação de conteúdo; a implementação dessa identificação está **[nao verificado]**. (`contrato.txt:5`, anexo; `bin/codex_pergunta.sh:50`; `bin/codex_pergunta.sh:111`)

4. **O contador prometido não tem leitor atual.** O CODEX-ESTADO publicado lê respostas finais das sessões e usa `chamadas.log` para atribuir procedência. O módulo de contadores conhece `limite.log` e `chamadas.log`, sem leitura de `recusa.log`. **Contraexemplo:** uma recusa registrada apenas no novo arquivo não produz contador no CODEX-ESTADO atual. É necessário incluir a mudança do leitor e sua verificação no contrato. (`bin/gerar_codex_estado.py:169-177`; `bin/codex_limite_estado.py:21-22`; `contrato.txt:6`, anexo)

   **Há também risco de contar recusa como uso do Codex.** “Antes do slot” não significa “antes do `ini`”: atualmente o `ini` é escrito antes da preparação do brief. **Contraexemplo:** inserir a recusa depois dele gera `ini/fim rc=5`, e o placar soma uma chamada de verificador sem execução do Codex, porque conta todo `ini`. A posição futura dessa recusa está **[nao verificado]**. (`bin/codex_pergunta.sh:88-89`; `bin/codex_pergunta.sh:118-138`; `bin/codex_limite_estado.py:71-78`)

5. **O orçamento de contexto já é problemático com estas fontes.** Numa extração direta em memória, usando ponto seguido de espaço para terminar a primeira frase de cada lei, os recortes somam **35.149 bytes**, ainda sem cabeçalho, hashes, ponteiros e separadores da tabela. O tamanho definitivo depende do extrator ainda proposto. **Contraexemplo:** essa interpretação determinística, aplicada às fontes atuais, já encontra o rc 1 previsto pelo contrato e não consegue gerar o contexto completo. É preciso definir precisamente “1a frase” e demonstrar que a projeção escolhida cabe. (`CLAUDE.md:14-89`; `CLAUDE.md:241-278`; `app/docs/LEIS.md:59-149`; `app/docs/MAPA.md:6-69`; `app/docs/GLOSSARIO.md:1-27`; `contrato.txt:5`, anexo)

6. **AGENTS público pode expor conteúdo que a publicação das fontes mascara.** O fluxo atual publica uma lista explícita de arquivos pelo raspador; AGENTS, CLAUDE e GLOSSARIO não estão nela. O raspador só processa os pares recebidos. **Contraexemplo sintético:** um CPF `[cpf]` na primeira frase de uma lei seria mascarado na LEIS pública, mas permaneceria na projeção crua descrita para AGENTS. Publicar essa projeção diretamente abre outra saída para o mesmo conteúdo. Publicação efetiva de AGENTS ou vazamento atual: **[nao verificado]**. (`bin/relato.sh:100-111`; `bin/relato.sh:115`; `bin/relato.sh:124-126`; `bin/raspar_publico.py:23-40`; `bin/nome_no_texto.py:110`; `contrato.txt:2`, anexo)

   Acrescentar AGENTS ao raspador também não lhe atribui automaticamente o tratamento de texto gerado: a classificação depende do nome do destino, e só `SESSAO.md` está em `GERADOS`. **Contraexemplo sintético:** o token isolado `Zebrante`, usado no próprio selo, é detectado em modo gerado; sob destino AGENTS.md recebe a política de prosa, que conserva essa forma. O selo consulta a mesma classificação. (`bin/raspar_publico.py:29-31`; `bin/nome_no_texto.py:39`; `bin/nome_no_texto.py:78-84`; `bin/tests/test_publicacao_sem_nome.sh:72`; `bin/tests/test_publicacao_sem_nome.sh:84-90`)

7. **Demais leitores e compatibilidade:**

   - **`chamadas.log`: sem achado de quebra**, preservando as posições TAB, a chave `(obra, hash, pid)` e o par `ini/fim`. Seus consumidores são o ESTADO e a atribuição de origem no CODEX-ESTADO; o parser aceita rc como texto, inclusive `5`. A distorção de contagem é a descrita no item 4. (`bin/codex_limite_estado.py:49-96`; `bin/gerar_estado.py:269-270`; `bin/gerar_codex_estado.py:175-176`)
   - **Formato do brief: sem achado de quebra nos leitores atuais examinados.** Ele segue para o stdin do CLI; os selos cobram substrings de segurança e presença dos dados, sem exigir a ordem inicial atual. Os casos existentes de sonda/leitura também não entram numa recusa restrita a `verificador`. (`bin/codex_pergunta.sh:157-158`; `bin/tests/test_codex_pergunta.sh:13-29`; `bin/tests/test_codex_sonda.sh:50`; `bin/tests/test_codex_sonda.sh:61-65`; `bin/tests/test_codex_limite.sh:24`)
   - **rc 75: sem achado de incompatibilidade com a preservação proposta.** Continua associado aos eventos de limite/indisponibilidade e coberto pelo selo atual. (`bin/codex_pergunta.sh:163-171`; `bin/tests/test_codex_limite.sh:28-39`)
   - **O leitor do protótipo foi identificado incorretamente na pergunta:** `~/codex_prototipo/gerar_codex_estado.py` lê `~/.codex/sessions`, selecionando respostas finais; não lê `logs/codex/`. Alterar brief ou acrescentar `recusa.log` não muda essa seleção. (`/home/ronald/codex_prototipo/gerar_codex_estado.py:17`; `/home/ronald/codex_prototipo/gerar_codex_estado.py:24`; `/home/ronald/codex_prototipo/gerar_codex_estado.py:34-45`)

## 30/10 17:16 · sessao 8ecc7054dbc7 · O261-V-FASE · leitura · origem=Code

**A árvore ainda não inclui a fase deslocada na lista: registra C1–C9, sem C10. Existe lavra de desvio 12x36, mas ela não contém o registro diário necessário para nomear dia, vínculo e proposta na lista.** (`app/escala/services/cadastro_realidade.py:36`; `app/escala/services/cadastro_realidade.py:513`; `app/ponto/management/commands/censo_fase_12x36.py:200`)

**1. Quem produz, quem escreve e quem lê**

**(a) `fase_12x36.fora_de_fase` e `ancora_sugerida`**

- `ancora_sugerida` produz um dicionário; não persiste. Exige piso de evidência, compara paridades e ganho e devolve âncora ou motivo. Seus leitores diretos são `fora_de_fase` e `propositor.propor`. (`app/escala/services/fase_12x36.py:45`; `app/escala/services/fase_12x36.py:67`; `app/escala/services/fase_12x36.py:203`; `app/escala/services/propositor.py:307`; `app/escala/services/propositor.py:330`)
- `fora_de_fase` lê vínculos ativos, batidas, células e folgas; exige os dois lados — trabalho sem turno e folga com turno — no piso configurado. Retorna contagens e sugestão, sem gravar, sem datas desses conjuntos e sem PK do vínculo no resultado. (`app/escala/services/fase_12x36.py:106`; `app/escala/services/fase_12x36.py:128`; `app/escala/services/fase_12x36.py:159`; `app/escala/services/fase_12x36.py:178`; `app/escala/services/fase_12x36.py:188`)
- Leitores diretos: API de mensageria, partição da fila por causa, propositor, pauta semanal e comando `vigiar_familia_escala`. A revisão semanal usa a pauta e pode escrevê-la por `escrever_semanal`; isso não grava fase no vínculo. (`app/api/views_mensageria.py:1669`; `app/chamados/services/fila_por_causa.py:173`; `app/escala/services/propositor.py:455`; `app/escala/services/fase_12x36.py:215`; `app/ponto/management/commands/vigiar_familia_escala.py:23`; `app/escala/services/fase_12x36.py:270`)

**(b) `fase_na_tela.fase_sugerida`**

Produz `(ancora, desde, motivo)`, sem persistir. Lê os turnos da janela, pergunta por troca de ritmo e possui decisões próprias por sequência de plantões e paridade. Seu leitor de produção é `colaboradores.views_fase.fase_12x36`. (`app/escala/services/fase_na_tela.py:50`; `app/escala/services/fase_na_tela.py:63`; `app/escala/services/fase_na_tela.py:70`; `app/escala/services/fase_na_tela.py:78`; `app/colaboradores/views_fase.py:62`)

A view monta o contexto e o template envia `data_ancora` em campo oculto. A escrita normal do vínculo pertence a `executar_vinculo`, que atribui a âncora declarada e salva o EC. (`app/colaboradores/views_fase.py:104`; `app/templates/colaboradores/partials/_fase_12x36.html:8`; `app/colaboradores/services/vinculo.py:167`; `app/colaboradores/services/vinculo.py:271`)

**(c) `cadastro_realidade.lista` e seus leitores**

`lista` monta um dicionário a partir das fontes cadastrais e lavradas; não persiste esse resultado. As assinaturas e os totais saem dessa montagem. (`app/escala/services/cadastro_realidade.py:502`; `app/escala/services/cadastro_realidade.py:513`; `app/escala/services/cadastro_realidade.py:575`; `app/escala/services/cadastro_realidade.py:646`)

| Leitor de produção | O que lê |
|---|---|
| Tela | `escala.views` chama `lista` com empresas visíveis e entrega `dados` ao template. (`app/escala/views.py:487`; `app/escala/views.py:490`; `app/escala/views.py:495`) |
| Template | Percorre postos, colaboradores e assinaturas e mostra detalhe, dono e ação. (`app/templates/escala/cadastro_x_realidade.html:21`) |
| PDF | Recebe o mesmo `dados` da chamada da view; `pdf_bytes` apenas o renderiza. (`app/escala/views.py:491`; `app/escala/services/cadastro_realidade.py:674`) |
| `fechar_avisos_regua_na_lista` | Chama a lista, seleciona C7 e cancela somente avisos comprovados nela. (`app/escala/management/commands/fechar_avisos_regua_na_lista.py:26`; `app/escala/management/commands/fechar_avisos_regua_na_lista.py:47`) |
| `fechar_chamados_vinculo_na_lista` | Exige lavra C8, chama a lista, seleciona C8 e cancela os comprovados. (`app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:30`; `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57`) |
| `vigia_fio.censo` | Lê uma vez por censo; usa `dias_na_lista` para reconhecer dia com dono BATIDA/CADASTRO. (`app/chamados/services/vigia_fio.py:244`; `app/chamados/services/vigia_fio.py:250`; `app/escala/services/cadastro_realidade.py:655`) |
| Placar estrutural R2 | A função aparece como **fonte textual declarada**, sem chamada executável a `lista` nesse registro. (`app/core/placar_estrutural.py:140`) |

O placar exibido pela própria lista usa `por_assinatura` e os conjuntos efetivamente incluídos na montagem. (`app/escala/services/cadastro_realidade.py:636`; `app/escala/services/cadastro_realidade.py:646`)

**(d) `aplicar_proposta_escala`**

É uma intenção cujo escritor registrado é `escala.services.propositor.aplicar`; seu juiz registrado é `propositor.propostas`. (`app/core/intake.py:49`)

Os caminhos de produção encontrados são: comando `propostas_de_escala`, aplicação pelo intake e aplicação pelo fio. Intake e fio revalidam a proposta antes de chamar o escritor; o formulário do fio envia a ação `aplicar_proposta_escala`. (`app/escala/management/commands/propostas_de_escala.py:25`; `app/escala/management/commands/propostas_de_escala.py:50`; `app/core/views_ia.py:636`; `app/core/views_ia.py:645`; `app/core/views_ia.py:654`; `app/chamados/services/proposta_no_fio.py:114`; `app/chamados/services/fio.py:505`; `app/templates/chamados/partials/modal_fio.html:50`)

O escritor chama a porta retroativa, grava `DecisaoProposta.ACEITA` e o evento `aplicar_proposta_escala`. (`app/escala/services/propositor.py:687`; `app/escala/services/propositor.py:695`; `app/escala/services/propositor.py:699`)

**(e) `regenerar_celulas_vinculo`**

O corpo escritor está em `ponto/portas/celula.py`; `gerar_celulas.py` o reexporta. (`app/ponto/portas/celula.py:100`; `app/ponto/management/commands/gerar_celulas.py:13`)

Chamadores diretos de produção encontrados:

| Caminho | Referência |
|---|---|
| Signals de vigência e âncora | `app/escala/signals.py:71`; `app/escala/signals.py:117` |
| Executor do vínculo | `app/colaboradores/services/vinculo.py:320` |
| Correção retroativa | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230` |
| Alteração do tipo de escala | `app/escala/services/cadastro_tipo.py:348` |
| Folgas | `app/escala/folgas.py:220` |
| Executor automático de escala | `app/escala/services/escala_auto_executor.py:104`; `app/escala/services/escala_auto_executor.py:143` |
| Porta de exportada | `app/ponto/portas/regen_exportada.py:174` |
| Wrapper de regeneração de um dia | `app/ponto/portas/celula.py:474` |

O rastro da regeneração é lido por `janelas_de_regeneracao`, a partir de `LogAuditoria`; a classificação de cobrança órfã usa preferencialmente o carimbo exato `regenerada_em` da célula. (`app/escala/services/regeneracao.py:36`; `app/escala/services/regeneracao.py:57`; `app/escala/services/regeneracao.py:71`)

**(f) Escolha do vínculo do dia**

`vinculo_do_dia` é função pura: lê a geradora da célula; se ela estiver ausente da alimentação ou fora de sua vigência, retorna `None`. Sem geradora, escolhe entre os vínculos que cobrem a data pelo máximo de `(ativa, data_inicio)`. (`app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:237`; `app/escala/alimentacao.py:247`; `app/escala/alimentacao.py:253`)

Os “afins” têm papéis distintos:

| Função | Papel |
|---|---|
| `tem_vigencia_declarada`, `vinculo_cobre_a_data` e irmãos SQL | Candidatura e cobertura temporal. (`app/escala/alimentacao.py:157`; `app/escala/alimentacao.py:167`; `app/escala/alimentacao.py:173`; `app/escala/alimentacao.py:187`) |
| `vinculo_do_dia_carregado` / plural | Carregam células, geradoras e vínculos e chamam a mesma juíza. (`app/escala/alimentacao.py:284`; `app/escala/alimentacao.py:297`) |
| `servico_jornada.escala_vigente` | Carrega e delega à juíza. (`app/escala/servico_jornada.py:38`; `app/escala/servico_jornada.py:57`) |
| `utils._esc_vigente_do_dia` | Carrega e delega à juíza. (`app/escala/utils.py:2114`; `app/escala/utils.py:2134`) |

Entre os leitores estão calculador, espelho, PDF, exportação, fechamento e censo de fase. (`app/ponto/calculador/alimentacao.py:176`; `app/ponto/services/espelho.py:297`; `app/relatorios/pdf_espelho.py:567`; `app/folha/export.py:348`; `app/ponto/services/fechamento.py:125`; `app/ponto/management/commands/censo_fase_12x36.py:111`)

Na lista, `_vinculos` pergunta por **hoje**, sem mapa de células; C9 conserva separadamente `ec_do_dia`, `template_do_dia` e `dono_do_dia`. O detector `fora_de_fase` usa o EC ativo em toda a janela. (`app/escala/services/cadastro_realidade.py:477`; `app/escala/services/cadastro_realidade.py:494`; `app/escala/services/cadastro_realidade.py:312`; `app/escala/services/fase_12x36.py:106`; `app/escala/services/fase_12x36.py:181`)

**2. LAVRA de C3/C7/C8/C9 e de fase deslocada**

As quatro fontes usam `MetricaSnapshot.detalhe`, um JSON do modelo de métricas. Os leitores procuram a lavra mais recente por data e cálculo. (`app/inteligencia/models.py:81`; `app/escala/services/previsto_cego.py:217`; `app/escala/services/regua_na_lista.py:57`; `app/ponto/services/vinculo_na_lista.py:125`; `app/ponto/services/teto_na_lista.py:120`)

| Assinatura | Campo/chave lido | Escritor e agenda declarada |
|---|---|---|
| **C3** | Chave `previsto_cego`, escopo `frota`; a lista formata o snapshot. (`app/escala/services/previsto_cego.py:58`; `app/escala/services/previsto_cego.py:217`; `app/escala/services/cadastro_realidade.py:381`) | `lavrar_previsto_cego --apply`, diariamente **07:33**. (`app/escala/management/commands/lavrar_previsto_cego.py:30`; `app/config/crons.py:495`) |
| **C7** | Chave `termometro_regua`, `detalhe.por_colab`: EC, classe, declarado, real, evidência e destino. (`app/escala/services/regua_na_lista.py:19`; `app/escala/services/regua_na_lista.py:31`; `app/escala/services/cadastro_realidade.py:276`) | `termometro_regua --avaliar --lavrar` → `regua_na_lista.lavrar`, diariamente **06:00**. (`app/escala/management/commands/termometro_regua.py:92`; `app/escala/services/regua_na_lista.py:42`; `app/config/crons.py:409`) |
| **C8** | Chave `vinculo_divergente`, `detalhe.por_colab`; compara o posto lavrado com o cadastro atual. (`app/ponto/services/vinculo_na_lista.py:21`; `app/ponto/services/vinculo_na_lista.py:138`; `app/escala/services/cadastro_realidade.py:294`) | `detectar_vinculo_divergente --lavrar` → `vinculo_na_lista.lavrar`, **segunda-feira 06:52**. (`app/ponto/management/commands/detectar_vinculo_divergente.py:35`; `app/ponto/services/vinculo_na_lista.py:111`; `app/config/crons.py:578`) |
| **C9** | Chave `par_separado_pelo_teto`, `detalhe.itens`, incluindo dia, EC/template e dono. (`app/ponto/services/teto_na_lista.py:25`; `app/ponto/services/teto_na_lista.py:75`; `app/escala/services/cadastro_realidade.py:306`) | `lavrar_teto_do_dia --apply` → `teto_na_lista.lavrar`, diariamente **07:42**. (`app/ponto/management/commands/lavrar_teto_do_dia.py:26`; `app/ponto/services/teto_na_lista.py:110`; `app/config/crons.py:502`) |

Esses são horários configurados; execução efetiva e conteúdo atual do banco **[não verificado]**. O timezone Django declarado é `America/Sao_Paulo`. (`app/config/settings/base.py:93`)

**Existe contrato de lavra de desvio 12x36:** `censo_fase_12x36 --lavrar` escreve a chave `desvio_de_cadastro_12x36`, escopo `tenant`, com contador e IDs dos colaboradores em desvio, automáticos e propostas. O cron está declarado diariamente às **07:38**, com `--min-plantoes 6 --lavrar`. (`app/ponto/management/commands/censo_fase_12x36.py:192`; `app/ponto/management/commands/censo_fase_12x36.py:200`; `app/config/crons.py:614`)

Esse snapshot **não guarda os dias dos furos/folgas, o EC e a âncora proposta por linha**. O censo calcula esses conjuntos, monta linhas com EC, segmentos, contagens, virada e proposta, mas o arquivo JSON só é escrito com `--json`, ausente na agenda. É o escritor existente mais próximo em assunto e alimentação por vínculo do dia. (`app/ponto/management/commands/censo_fase_12x36.py:102`; `app/ponto/management/commands/censo_fase_12x36.py:129`; `app/ponto/management/commands/censo_fase_12x36.py:153`; `app/ponto/management/commands/censo_fase_12x36.py:172`; `app/ponto/management/commands/censo_fase_12x36.py:203`; `app/config/crons.py:614`)

Há também lavra `fase_conflitante`, às **07:14**, mas ela compara **foto de folga × âncora**, não cadastro × turnos reais. Nenhuma dessas duas lavras entra na montagem atual de `lista`. (`app/escala/services/fase_conflitante.py:11`; `app/escala/services/fase_conflitante.py:86`; `app/config/crons.py:479`; `app/escala/services/cadastro_realidade.py:513`)

**3. Quantos juízes de sugestão existem**

**No conflito nomeado pelo aval, são dois:** `ancora_sugerida` e `fase_sugerida`. Não são cascas da mesma decisão: têm janelas, critérios e desempates diferentes. (`app/docs/PROMPTS.md:1669`; `app/escala/services/fase_12x36.py:32`; `app/escala/services/fase_12x36.py:67`; `app/escala/services/fase_na_tela.py:19`; `app/escala/services/fase_na_tela.py:70`; `app/escala/services/fase_na_tela.py:78`)

Contando também outros lugares que escolhem autonomamente uma âncora, há **quatro sítios de decisão** identificados:

| Sítio | Decisão e chamador |
|---|---|
| `fase_12x36.ancora_sugerida` | Melhor paridade com piso e ganho; chamado pelo detector e pelo propositor. (`app/escala/services/fase_12x36.py:75`; `app/escala/services/fase_12x36.py:203`; `app/escala/services/propositor.py:330`) |
| `fase_na_tela.fase_sugerida` | Troca de ritmo, sequência recente e paridade; chamado pela view do assistente. (`app/escala/services/fase_na_tela.py:70`; `app/escala/services/fase_na_tela.py:78`; `app/colaboradores/views_fase.py:62`) |
| `censo_fase_12x36.Command.handle` | Define `prop_anc` pelo último plantão real; executado pelo comando/cron. (`app/ponto/management/commands/censo_fase_12x36.py:143`; `app/config/crons.py:614`) |
| `propositor` | Além de chamar `ancora_sugerida`, escolhe âncora diretamente no ramo forçado e na busca de candidatos de template/ritmo. (`app/escala/services/propositor.py:173`; `app/escala/services/propositor.py:310`; `app/escala/services/propositor.py:345`) |

Nenhum desses está declarado como juiz de **“âncora/fase sugerida”** em `core/juizes.py`. A família escala declara somente “qual vínculo vale neste dia?” e “o vínculo cobre a data?”. (`app/core/juizes.py:1237`; `app/core/juizes.py:1245`)

A L-112 permite declarar início e fase e trata a derivação como sugestão; não declara ali uma dessas funções como autoridade única. (`app/docs/LEIS.md:149`)

**4. Aplicação no passado, EC encerrado e trilha**

`aplicar` usa o `vinculo_id` da proposta, chama a correção com `novo_trecho=False` e `ate=None`. Sem `desde` explícito usa `p['aplicar_desde']`; o produtor preenche esse valor pelo início da competência corrente. (`app/escala/services/propositor.py:517`; `app/escala/services/propositor.py:549`; `app/escala/services/propositor.py:677`; `app/escala/services/propositor.py:687`)

O EC é corrigido **no lugar**. Como `ate=None`, `novo_fim` preserva `ec.data_fim`; o escritor salva somente tipo, âncora e fim, sem reativar o vínculo nem mudar seu início. Portanto, EC encerrado com fim declarado continua encerrado. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:192`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211`; `app/colaboradores/services/vinculo.py:839`)

**A porta que reescreve o DNA passado é `regenerar_celulas_vinculo`, exceção formal à imutabilidade.** A geração ordinária preserva passado existente; a regeneração recalcula pelo cadastro corrigido e substitui células `origem='gerada'`, preservando as editadas. (`app/ponto/portas/celula.py:103`; `app/ponto/portas/celula.py:243`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:598`)

O comando pede regeneração do piso até `min(data_fim ou hoje, hoje)`, mas a porta pode estender o pedido até o horizonte corrente. Fora da vigência, pode neutralizar células geradas daquele EC, sem outro vínculo cobrindo o dia. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`; `app/ponto/portas/celula.py:171`; `app/ponto/portas/celula.py:210`; `app/ponto/portas/celula.py:225`)

As guardas distinguem:

- EC desativado **sem fim**: recusa e evento `regenerar_recusado_sem_vigencia`; EC encerrado **com fim** tem vigência declarada. (`app/ponto/portas/celula.py:130`; `app/escala/alimentacao.py:157`)
- Competência lavrada: datas são puladas normalmente; texto em `apesar_da_lavra` abre a guarda por ato explícito. A enumeração considera exportação e holerite publicado, delegando ao juiz da competência. (`app/ponto/portas/celula.py:196`; `app/ponto/portas/celula.py:238`; `app/ponto/management/commands/gerar_celulas.py:151`)

A trilha tem quatro partes:

| Registro | Conteúdo |
|---|---|
| EC | `LogAuditoria` de edição, com antes/depois da correção retroativa. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:206`) |
| Célula reescrita dentro da vigência | `dna_anterior`, `regenerada_em`, incremento de `regeneracoes`. (`app/ponto/portas/celula.py:259`) |
| Lote de regeneração | Evento `regenerar_celulas_vinculo`, quando houve alteração: janela, quantidade, barrados, autoridade da lavra, motivo de abertura e cobranças órfãs marcadas. (`app/ponto/portas/celula.py:297`) |
| Aceitação humana | `DecisaoProposta.ACEITA` e evento `aplicar_proposta_escala`, com usuário, origem e assinatura. (`app/escala/services/propositor.py:695`) |

Há uma diferença de trilha no ramo **fora da vigência**: ele salva somente `trabalha` e `dna`, sem atualizar aqueles três campos de histórico da célula. As cobranças dos dias reescritos são marcadas para rejulgamento; o comando também chama a reconciliação posterior. (`app/ponto/portas/celula.py:231`; `app/ponto/portas/celula.py:277`; `app/colaboradores/management/commands/corrigir_escala_retroativa.py:237`)

**5. Padrões realizados e furados**

| Padrão documentado | Situação encontrada |
|---|---|
| **Single source of truth / um juiz por pergunta** | A sugestão única exigida pelo aval corresponde ao norte do plano; as duas funções de sugestão atuais mantêm decisões diferentes, caracterizando derivação paralela. (`app/docs/PLANO-PADROES.md:46`; `app/docs/GLOSSARIO.md:23`; `app/escala/services/fase_12x36.py:67`; `app/escala/services/fase_na_tela.py:70`) |
| **Single writer / célula soberana** | A aplicação delega à porta existente, e a regeneração é a exceção formal nomeada no glossário. (`app/docs/GLOSSARIO.md:11`; `app/docs/GLOSSARIO.md:12`; `app/escala/services/propositor.py:687`; `app/ponto/portas/celula.py:100`) |
| **CQRS / projeção de leitura** | C3/C7/C8/C9 leem e formatam lavra pronta, coerente com separar cálculo e leitura; a própria lápide C3 proíbe a conta de frota no request. (`app/docs/GLOSSARIO.md:22`; `app/escala/services/cadastro_realidade.py:48`; `app/escala/services/cadastro_realidade.py:276`; `app/escala/services/cadastro_realidade.py:294`; `app/escala/services/cadastro_realidade.py:306`; `app/escala/services/cadastro_realidade.py:381`) |
| **Bitemporal / vínculo do dia × vínculo de hoje** | A juíza e seus carregadores realizam a distinção. O detector atual usa EC ativo na janela passada; a apresentação geral da lista resolve hoje, enquanto C9 preserva os identificadores do dia separadamente. (`app/docs/GLOSSARIO.md:24`; `app/escala/alimentacao.py:237`; `app/escala/services/fase_12x36.py:106`; `app/escala/services/fase_12x36.py:181`; `app/escala/services/cadastro_realidade.py:312`; `app/escala/services/cadastro_realidade.py:494`) |
| **Evento em vez de varredura / Transactional Outbox** | As lavras examinadas são alimentadas pelos crons declarados. Isso não constitui outbox: o glossário a declara “a construir” e o plano mantém seu contrato pendente. (`app/config/crons.py:409`; `app/config/crons.py:495`; `app/config/crons.py:502`; `app/conf

[truncado em 20000 de 20138 caracteres]

## 30/10 17:15 · sessao 5f7658c4d57d · - · - · origem=Ronald (tmux)

Achado de desenho: publicar o AGENTS cru pode expor conteúdo que a publicação atual raspa. O publicador só entrega pares explícitos ao raspador e não inclui AGENTS/CLAUDE/GLOSSARIO (`bin/relato.sh:100-111`); commit e push também usam lista explícita (`bin/relato.sh:115`, `bin/relato.sh:124-126`). O raspador não descobre arquivos: só processa os pares recebidos (`bin/raspar_publico.py:23-40`).

Contraexemplo concreto: colocar `CPF [cpf]` na primeira frase de uma lei. A LEIS pública seria mascarada pelo caminho atual (`bin/relato.sh:105`, `bin/nome_no_texto.py:41`, `bin/nome_no_texto.py:110`); a projeção determinística crua descrita no contrato conservaria esse CPF no AGENTS [nao verificado: gerador ainda ausente]. O contrato precisa declarar em qual repo esse arquivo vai ao ar e qual raspagem aplica; publicação efetiva ou vazamento atual do AGENTS: [nao verificado].

Mesmo acrescentar AGENTS ao raspador não lhe dá o tratamento estrito de texto gerado: a decisão usa o basename do destino (`bin/raspar_publico.py:29-31`), e `GERADOS` contém apenas `SESSAO.md` (`bin/nome_no_texto.py:39`). Contraexemplo sintético já presente no selo: `Zebrante` isolado em Title Case é detectado com `gerado=True` (`bin/tests/test_publicacao_sem_nome.sh:84-90`); com destino AGENTS.md, a regra mantém token que não está em caixa alta (`bin/nome_no_texto.py:78-84`). O selo de publicação usa exatamente a mesma classificação, portanto também pode aceitar essa forma (`bin/tests/test_publicacao_sem_nome.sh:71-72`).

Quanto a conteúdo pessoal efetivamente presente nos recortes propostos hoje: sem achado comprovado; não houve acesso à lista do banco [nao verificado]. Há ampliação de publicação: CLAUDE e GLOSSARIO não estão entre as fontes copiadas pelo publicador atual (`bin/relato.sh:100-111`), enquanto o desenho inclui seus textos no AGENTS [nao verificado: contrato anexado, sem arquivo local para citar].

## 30/10 17:15 · sessao a6ee853ff509 · SUITE-POR-IMPACTO · verificador · origem=Code

**Sim. O critério atual permite pular o arsenal sem provar que o push corresponde à árvore certificada:** ele compara o carimbo com a árvore **viva**, mas compara o push apenas com os `.py` e `.html` dela. Isso também deixa `bin/` fora da prova dos portões de host. ([bin/pre-push.sh:61](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/pre-push.sh:61), [bin/arvore_do_push.sh:85](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/arvore_do_push.sh:85))

Os caminhos abaixo são deduções do código; ocorrência efetiva em produção: [nao verificado].

1. **(C) “Mesmo hash da árvore do push” não é o critério implementado.** Considere certificado e árvore viva A, e push B com Python/HTML iguais, mas CSS, JS ou dados diferentes. O hash vivo casa com o carimbo; `--igual` não examina essas diferenças; `SKIP_ARSENAL=1` pode ser atribuído. Com diferença apenas em `bin/`, os portões executados sobre A também não certificam B. **Mudança no contrato:** substituir “o pre-push não muda de critério” por comparação explícita de `REGUA_ARVORE` com a árvore Git completa de `PUSH_SHA`; divergência ou erro impede o atalho. ([bin/pre-push.sh:62](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/pre-push.sh:62), [bin/arvore_do_push.sh:89](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/arvore_do_push.sh:89))

2. **(C) O novo modo precisa escapar do `JA VERDE` antigo.** Hoje só `--only` recebe tratamento específico; qualquer outro primeiro argumento deixa disponível o atalho por FP, que termina antes de `hooks.sh`, da pasta de selos e de `node_check`. Uma implementação que apenas substitua o `docker run` pode retornar sem validar o certificado nem executar esses portões. **Mudança:** reconhecer `--carimbar-da-copia` antes do cache e obrigar esse ramo a validar o certificado e executar todos os portões, independentemente do carimbo anterior. ([bin/regua.sh:46](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/regua.sh:46), [bin/regua.sh:84](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/regua.sh:84), [bin/regua.sh:130](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/regua.sh:130))

3. **(C) “Mesmo rabo” não assegura estabilidade da árvore inteira.** A régua captura o hash completo no começo, mas no fim compara somente FP de Python/HTML. Alterações de `bin/`, JS ou dados durante os portões ficam fora dessa comparação. Além disso, o pre-push decide o atalho antes de adquirir sua trava, sem repetir a decisão depois da espera. **Mudança:** vincular portões e publicação do carimbo à mesma árvore identificada, conferir novamente o hash completo e revalidar o atalho sob a trava. ([bin/regua.sh:33](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/regua.sh:33), [bin/regua.sh:45](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/regua.sh:45), [bin/regua.sh:191](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/regua.sh:191), [bin/pre-push.sh:76](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/pre-push.sh:76))

4. **(C e G) Hash inicial igual ao final não exclui A→B→A durante a execução.** A porta monta diretamente a cópia, de forma gravável; a trava serializa o comando de teste, sem impedir editores externos. Assim, os extremos iguais não provam quais bytes foram observados no meio. **Mudança:** exigir cópia privada materializada da identidade registrada, sem alterações externas durante coleta/suite/extração. Não basta impor `:ro`: há testes atuais que alteram e restauram arquivos da própria cópia; essas mutações internas precisam ser previstas. ([bin/suite.sh:74](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/suite.sh:74), [bin/suite.sh:100](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/suite.sh:100), [bin/trava_teste.sh:35](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/trava_teste.sh:35), [test_selo_diagrama_do_codigo.py:49](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/app/core/tests/test_selo_diagrama_do_codigo.py:49))

5. **(C) Chamar `node_check` não prova que ele examinou a árvore do push.** Ele lista JS na raiz local, mas executa o analisador dentro de `${CASCA:-saas_core}`. Não verifica a identidade entre esses arquivos locais e `/app` desse container. **Mudança:** executar o analisador sobre os bytes da árvore certificada ou provar essa identidade. O mount efetivo do container em produção é [nao verificado]. ([bin/node_check.sh:24](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/node_check.sh:24), [bin/node_check.sh:33](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/node_check.sh:33), [bin/node_check.sh:37](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/node_check.sh:37))

6. **(C) Hash Git de `app/` não identifica todo o ambiente observado.** A montagem sobrepõe `/app/staticfiles` com conteúdo da raiz do **script**, independentemente do `--dir`; esse diretório é ignorado pelo Git. A execução também usa a tag mutável `saas-hasner-core:latest`. **Mudança:** acrescentar ao certificado o ID efetivamente usado da imagem e a identidade das montagens relevantes, fixando-os durante a execução e conferindo-os na aceitação. Sem isso, limitar expressamente a promessa à subárvore Git; identidade de todo `/app` permanece [nao verificado]. ([bin/arvore_do_push.sh:19](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/arvore_do_push.sh:19), [bin/arvore_do_push.sh:55](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/arvore_do_push.sh:55), [.gitignore:13](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/.gitignore:13), [bin/suite.sh:101](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/suite.sh:101))

7. **(C) O auxiliar de hash não exige sucesso da coleta inteira.** Ele descarta o rc de `git add -A --ignore-errors` e depois executa `write-tree`; portanto, produzir um hash não comprova que a coleta anterior terminou sem erro. **Mudança:** qualquer erro de criação/população/escrita do índice invalida certificado e carimbo; hash parcial nunca autoriza o atalho. ([bin/arvore_hash.sh:27](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/arvore_hash.sh:27))

8. **Os selos “da cópia” podem ficar verdes lendo a principal.** Por exemplo, `test_lei_akita.sh` crava `/home/ronald/saas-hasner` e lê seu `CLAUDE.md`; `hooks.sh` também fixa essa raiz. Medir apenas quais selos ficam vermelhos numa cópia limpa não descobre esses verdes sobre outro alvo. **Mudança:** a classificação precisa verificar o alvo efetivamente lido, inclusive nos selos verdes; no pouso, esse alvo deve corresponder à árvore vinculada ao push. ([bin/tests/test_lei_akita.sh:17](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/tests/test_lei_akita.sh:17), [bin/tests/test_lei_akita.sh:28](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/tests/test_lei_akita.sh:28), [bin/hooks.sh:9](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/hooks.sh:9))

Quanto às outras novidades da v3:

- **`.py` A/D/renomeado e base do mapa:** não encontrei nova falha demonstrável nessas regras sob uma árvore estável; implementação e reprodução histórica permanecem [nao verificado]. Eu explicitaria que inventário de status, comparação de AST e seleção usam **todos o mesmo hash do mapa**. A necessidade de estabilizar a cópia continua, pois a porta monta seu diretório diretamente. ([bin/suite.sh:74](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/suite.sh:74), [bin/suite.sh:100](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/suite.sh:100))

- **Medida por obra:** o denominador precisa ter definição operacional. O código admite `tests.py`, e o runner CI poda casos em quarentena; contar arquivos por glob não equivale necessariamente ao universo executável. **Mudança:** registrar numerador, denominador e identidade do universo por chamada; definir OBRA ausente, inclusão de fallback INTEIRA como 100% e fórmula da média por obra. A implementação da agregação e o resultado esperado zero são [nao verificado]. ([bin/tests/test_labels_fonte_unica.sh:29](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/bin/tests/test_labels_fonte_unica.sh:29), [app/config/settings/ci.py:40](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/app/config/settings/ci.py:40), [app/core/runner_quarentena.py:75](/home/ronald/codex_ws/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/app/core/runner_quarentena.py:75))

## 30/10 17:15 · sessao 4558fd89c791 · - · - · origem=Ronald (tmux)

- **Contador de recusa ausente:** o produtor atual só lê sessões e a associação de origem em `chamadas.log`; não lê `recusa.log` (`bin/gerar_codex_estado.py:169`, `bin/gerar_codex_estado.py:176`). O módulo de contadores conhece apenas `limite.log` e `chamadas.log` (`bin/codex_limite_estado.py:21`, `bin/codex_limite_estado.py:22`). Contraexemplo: gravar uma recusa sem chamar Codex não acrescenta contador ao CODEX-ESTADO atual. O contrato precisa explicitar mudança do leitor e selo.

- **Leitor indicado incorretamente:** `~/codex_prototipo/gerar_codex_estado.py` lê `~/.codex/sessions` (`/home/ronald/codex_prototipo/gerar_codex_estado.py:17`, `:24`), filtra somente mensagens finais (`:34`, `:40`, `:45`) e escreve seus textos (`:59`); não lê `logs/codex/`. O leitor de produção é `bin/gerar_codex_estado.py`, que importa `codex_limite_estado` (`bin/gerar_codex_estado.py:41`).

- **`chamadas.log`: sem quebra inevitável**, preservado o esquema atual. O wrapper escreve pares ini/fim TAB (`bin/codex_pergunta.sh:53`, `:59`, `:88`). O parser usa posições fixas e chave `(obra, hash, pid)` (`bin/codex_limite_estado.py:56`, `:63`, `:67`). Os consumidores são o ESTADO (`bin/gerar_estado.py:269`) e a procedência do CODEX-ESTADO (`bin/gerar_codex_estado.py:175`). O rc é texto livre: `5` funciona (`bin/codex_limite_estado.py:67`, `:88`).

- **Armadilha de contagem, dependente da posição da nova recusa:** se inserida depois do ini atual (`bin/codex_pergunta.sh:88`), uma recusa sem Codex será contada como “chamada ao Codex”, pois o contador conta todo ini independentemente de rc (`bin/codex_limite_estado.py:71`, `:76`). Contraexemplo: um brief recusado gera ini/fim rc=5; o número de verificadores sobe embora o Codex não tenha sido chamado. A posição definitiva é **[nao verificado]**.

- **Formato do brief: sem achado de quebra nos leitores atuais.** O brief vai ao stdin do CLI (`bin/codex_pergunta.sh:158`); não há parser estrutural local. O selo estrutural exige substrings de segurança e cabeçalho (`bin/tests/test_codex_pergunta.sh:13`, `:26`). O selo de sonda verifica presença dos dados e ausência de vazamentos, sem exigir ordem inicial do brief (`bin/tests/test_codex_sonda.sh:61`, `:65`). Os selos de rc exercitam sonda/leitura, portanto a regra restrita a verificador não os recusa (`bin/tests/test_codex_sonda.sh:50`, `bin/tests/test_codex_limite.sh:24`).

## 30/10 17:15 · sessao e5ce1b72dc79 · - · - · origem=Ronald (tmux)

A premissa “editar AGENTS muda REGUA_ARVORE, logo o pre-push roda o selo” falha em dois pontos independentes: [nao verificado]

1. **O pre-push não chama a régua nem seu laço de selos.** O hash diferente apenas impede `SKIP_ARSENAL=1`; depois o hook roda testes Django do arsenal e do control-plane. O próprio comentário registra que o laço de `bin/tests/` não roda no pre-push. **Contraexemplo:** carimbo verde; editar manualmente AGENTS.md, commitar e dar push. O hash muda, o arsenal Django roda, mas `test_agents_md_gerado.sh`, mesmo acrescentado ao laço da régua, não é chamado. Referências: `bin/pre-push.sh:14-16`, `bin/pre-push.sh:61-68`, `bin/pre-push.sh:155-185`, `bin/pre-push.sh:194-195`; laço somente em `bin/regua.sh:138-145`.

2. **A própria régua pode sair verde antes do selo.** Calcula `ARVORE_FP`, mas seu atalho compara apenas `FP` de arquivos `.py`/`.html` dentro de `app/`, ignorando o hash da árvore. **Contraexemplo:** carimbo OK; editar só AGENTS.md ou qualquer uma das quatro fontes `.md`; executar `bash bin/regua.sh`. Se as guardas anteriores passam, sai “JA VERDE” em vez de chegar ao selo, pois `FP` permanece igual. Referências: `bin/regua.sh:25-35`, `bin/regua.sh:45`, `bin/regua.sh:84-94`, `bin/regua.sh:138-145`.

**A parte restrita sobre hashing procede:** um AGENTS.md normal, rastreado ou novo não ignorado, entra no índice descartável usado por `git add -A` e altera o `git write-tree`; não há filtro por extensão. Isso não estabelece que seu selo execute. Referência: `bin/arvore_hash.sh:24-28`.

**O selo atual dessa infraestrutura não protege esses dois vínculos.** Exercita mudanças em `.py`/`.js`, verifica presença textual de `REGUA_ARVORE` no pre-push e contagem de atribuições no carimbo, sem verificar que a régua compara esse campo no atalho ou que o pre-push executa o laço. Referências: `bin/tests/test_suite_uma_vez_por_arvore.sh:38-64`, `bin/tests/test_suite_uma_vez_por_arvore.sh:77-83`.

**Outro limite de escopo:** pre-push usa caminhos fixos em `~/saas-hasner` para carimbo, fingerprint e hashing. O helper `arvore_hash.sh` resolve a raiz pelo próprio script. Logo um push de outra worktree pode medir AGENTS.md da árvore principal, não o da worktree. Contraexemplo: editar AGENTS.md exclusivamente numa worktree; `ARVORE_AGORA` vem do script em `~/saas-hasner/bin/`, que mede a árvore principal. Referências: `bin/pre-push.sh:41`, `bin/pre-push.sh:46`, `bin/pre-push.sh:61`, `bin/arvore_hash.sh:21-24`.

## 30/10 17:14 · sessao 0a9c17b37d82 · - · - · origem=Ronald (tmux)

**`aplicar_proposta_escala`**

- O escritor declarado é `escala.services.propositor.aplicar`; juiz declarado `propostas`, gate `editar_escalas`. `app/core/intake.py:49`.
- Chamadores de produção: comando `propostas_de_escala --aplicar` (`app/escala/management/commands/propostas_de_escala.py:50`); chat/IA, após gate e revalidação da proposta (`app/core/views_ia.py:636`); fio, após gate e assinatura (`app/chamados/services/proposta_no_fio.py:114`, `app/chamados/services/proposta_no_fio.py:141`), acionado por `app/chamados/services/fio.py:505`; formulário em `app/templates/chamados/partials/modal_fio.html:50`.
- `aplicar` chama `corrigir_escala_retroativa.Command._rodar`, passando vínculo explícito, `novo_trecho=False`, `ate=None`, `apply=True`; usa o tenant corrente. `app/escala/services/propositor.py:687`.
- A porta corrige EC existente no lugar; preserva `data_fim` quando `ate=None`. Salva somente `tipo_escala`, `data_ancora_colaborador`, `data_fim`: portanto não reativa vínculo encerrado. `app/colaboradores/management/commands/corrigir_escala_retroativa.py:192`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:211`, `app/colaboradores/services/vinculo.py:839`.
- O comando pede regeneração de `desde` até `min(data_fim ou hoje, hoje)` e depois reconcilia. A porta de regeneração estende o horizonte por padrão até o fim da competência atual. `app/colaboradores/management/commands/corrigir_escala_retroativa.py:225`, `app/ponto/portas/celula.py:171`.
- Trilhas: log `editar` com antes/depois do EC (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:206`); `DecisaoProposta.ACEITA` e evento `aplicar_proposta_escala` com autor, antes/depois, origem e assinatura (`app/escala/services/propositor.py:695`).

**`regenerar_celulas_vinculo` / passado**

- Corpo escritor em `app/ponto/portas/celula.py:100`; reexportado em `app/ponto/management/commands/gerar_celulas.py:13`.
- Chamadores diretos de produção: sinais de vigência e âncora (`app/escala/signals.py:71`, `app/escala/signals.py:117`); executor de vínculo (`app/colaboradores/services/vinculo.py:320`); retificação exportada (`app/ponto/portas/regen_exportada.py:174`); cadastro do tipo (`app/escala/services/cadastro_tipo.py:348`); folgas (`app/escala/folgas.py:220`); escala automática aplicar/desfazer (`app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:143`); correção retroativa (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`); wrapper de um dia (`app/ponto/portas/celula.py:474`).
- Geração ordinária preserva passado existente; só enriquece DNA nulo. `app/ponto/portas/celula.py:598`.
- Regeneração é exceção formal: reescreve células `origem='gerada'`, inclusive passadas. Grava `dna_anterior`, `regenerada_em`, incrementa `regeneracoes`, muda `trabalha`, geradora e DNA. `app/ponto/portas/celula.py:103`, `app/ponto/portas/celula.py:251`.
- **Ramo fora da vigência:** célula gerada pelo próprio EC, ainda trabalhando, sem lavra e sem outro vínculo cobrindo é zerada (`trabalha=False`, DNA sem marcos), mas esse ramo salva somente `trabalha/dna`, sem os três campos de denúncia. `app/ponto/portas/celula.py:225`.
- EC desativado sem `data_fim` é recusado, mantendo células e registrando `regenerar_recusado_sem_vigencia`; EC encerrado com `data_fim` passa na guarda de vigência declarada. `app/ponto/portas/celula.py:130`, `app/escala/alimentacao.py:157`.
- Guarda de lavra carrega competências e pula datas, salvo `apesar_da_lavra` preenchido. `app/ponto/portas/celula.py:196`, `app/ponto/portas/celula.py:238`. Enumeração reúne TXT e holerite com lote publicado, delegando a `competencia_fechada_para_celula`; status de `FechamentoMensal` não é a guarda. `app/ponto/management/commands/gerar_celulas.py:132`, `app/ponto/management/commands/gerar_celulas.py:151`, `app/folha/export.py:578`.
- O bloco usa `transaction.atomic` e consulta simples das células; não contém `select_for_update`. `app/ponto/portas/celula.py:206`.
- Veredito humano protege o **rejulgamento**, enquanto a proteção de reescrita do DNA é `origem`. `app/ponto/portas/celula.py:251`, `app/ponto/portas/celula.py:365`.
- Log de regeneração só quando `n>0`, com janela, alteradas, barrados/fonte, motivo de abertura e cobranças órfãs; falha da trilha é logada sem abortar correção. `app/ponto/portas/celula.py:297`.
- Leitor do rastro: `janelas_de_regeneracao` consulta `LogAuditoria`; prefere `regenerada_em` da própria célula, usando janela somente como fallback. `app/escala/services/regeneracao.py:57`, `app/escala/services/regeneracao.py:80`. Consumidores incluem supra-juiz (`app/ponto/management/commands/supra_juiz.py:147`, `app/ponto/management/commands/supra_juiz.py:239`), reconciliador (`app/ponto/management/commands/reconciliar_grade.py:29`) e auditor (`app/chamados/management/commands/auditar_invariantes_chamados.py:1133`).
- A denúncia na cobrança grava `contexto_json.orfa_regeneracao`, incluindo instante e marcos da emissão, pela porta de contexto. `app/ponto/management/commands/gerar_celulas.py:255`.

**Vínculo do dia / afins**

- Juíza pura: `vinculo_do_dia`. Com geradora, escolhe o EC nomeado e verifica `data_inicio/data_fim`; geradora ausente da alimentação ou fora da vigência retorna `None`. Sem geradora, escolhe cobertura e desempata por `(ativa, data_inicio)`. `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:237`.
- Cobertura única: `tem_vigencia_declarada`, `vinculo_cobre_a_data` e versões SQL. `app/escala/alimentacao.py:157`, `app/escala/alimentacao.py:167`, `app/escala/alimentacao.py:173`, `app/escala/alimentacao.py:187`.
- Alimentação: plural de escalas filtra candidatura/janela e acrescenta geradoras via `mais`; singular é casca. `app/escala/alimentacao.py:116`, `app/escala/alimentacao.py:153`.
- `vinculo_do_dia_carregado` é casca do plural; plural carrega células, geradoras, escalas e chama a mesma juíza. `app/escala/alimentacao.py:284`, `app/escala/alimentacao.py:297`.
- `servico_jornada.escala_vigente` e `utils._esc_vigente_do_dia` são carregadores que delegam; wrapper do calculador também delega. `app/escala/servico_jornada.py:38`, `app/escala/utils.py:2114`, `app/ponto/calculador/alimentacao.py:411`.
- Autoridade usada no espelho, PDF, folha, fechamento, turnos e supra-juiz: `app/ponto/services/espelho.py:297`, `app/ponto/services/espelho.py:1069`, `app/relatorios/pdf_espelho.py:303`, `app/relatorios/pdf_espelho.py:567`, `app/folha/export.py:348`, `app/ponto/services/fechamento.py:125`, `app/ponto/services/fechamento.py:191`, `app/ponto/turnos.py:1932`, `app/ponto/turnos.py:2131`, `app/ponto/turnos.py:2325`, `app/ponto/supra_juiz.py:136`.
- A lista Cadastro x Realidade consulta a juíza para **hoje**, passando escalas sem mapa de células nesse ponto. `app/escala/services/cadastro_realidade.py:494`.

## 30/10 17:14 · sessao 40fe771ed92e · - · - · origem=Ronald (tmux)

- **A/D e base do mapa:** não encontrei contraexemplo à regra v3 sob árvore estável. `--no-renames` faz renomeação resultar em remoção/criação; o diff precisa manter **o hash do mapa** também na análise de status e na comparação do AST. A reprodução histórica 270f75b7 → 7a8e4604 permanece [nao verificado].

- **O mapa pode ficar associado a uma árvore que não produziu suas medições.** A porta monta diretamente a cópia mutável (`bin/suite.sh:74`, `bin/suite.sh:100`); a trava protege o comando de teste, sem impedir edições nessa árvore (`bin/trava_teste.sh:35`, `bin/trava_teste.sh:42`). G exige limpeza inicial, sem assegurar estabilidade durante a coleta [nao verificado: contrato anexado sem arquivo numerado]. **Contrato:** gerar sobre cópia privada materializada do hash registrado; proibir alterações externas e conferir identidade ao finalizar, abortando promoção se houver divergência.

- **Não recomendar montagem somente leitura como cura indiscriminada:** testes atuais editam e restauram arquivos da própria cópia (`app/core/tests/test_selo_diagrama_do_codigo.py:49`, `app/core/tests/test_selo_diagrama_do_codigo.py:59`, `app/core/tests/test_selo_diagrama_do_codigo.py:85`, `app/core/tests/test_selo_diagrama_do_codigo.py:95`). **Contrato:** separar mutações internas esperadas dos testes de edição concorrente externa; preservar a execução em cópia privada.

- **O denominador da porcentagem precisa ser definido pelo runner real.** CI usa `RunnerComQuarentena` (`app/config/settings/ci.py:40`), que remove casos/classes em quarentena (`app/core/runner_quarentena.py:65`, `app/core/runner_quarentena.py:75`, `app/core/runner_quarentena.py:86`). Contar simplesmente arquivos `test*.py` pode incluir módulos sem casos efetivamente executados, reduzindo artificialmente a porcentagem [inferência desses arquivos]. **Contrato:** universo = módulos com casos descobertos e habilitados pelo mesmo runner/configuração da chamada; registrar numerador, denominador e identidade dessa descoberta por obra.

## 30/10 17:14 · sessao 898b22993db6 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos por leitura estática:

1. **O atalho não prova igualdade com a árvore completa do push.** O hash completo comparado ao carimbo é da árvore **viva**; depois `--igual` compara somente `app/*.py` e `app/*.html`. Assim, live/certificado A e push B com mesmos Python/HTML, mas JS/CSS/dados diferentes, permitem `SKIP_ARSENAL=1`. Diferenças em `bin/` também permitem que portões executados sobre A liberem B. Evidência: `bin/pre-push.sh:61–66`; `bin/arvore_do_push.sh:85–91`. **Contrato:** retirar “pre-push não muda de critério” e exigir identidade entre `REGUA_ARVORE` e a árvore Git completa do push.

2. **“Mesmo rabo” mantém uma garantia insuficiente de estabilidade dos portões.** A régua captura o hash completo somente no início; no fim compara apenas fingerprint de `.py`/`.html`. Mudanças de JS/`bin/` durante os portões não são detectadas pelo rabo. Cenário condicional: árvore A no início, B durante um portão, A novamente antes do push; carimbo de A pode liberar árvore cujos portões observaram B. Evidência: `bin/regua.sh:33–35`, `bin/regua.sh:45`, `bin/regua.sh:191–198`. **Contrato:** executar portões sobre snapshot identificado e imutável; extremos iguais sozinhos não excluem A→B→A.

3. **O certificado com hash app início=fim também admite A→B→A durante a suíte.** A porta atual monta o diretório candidato de forma gravável; não congela seu conteúdo. A ausência efetiva de edição concorrente é `[nao verificado]`. Evidência: `bin/suite.sh:73–79`, `bin/suite.sh:100`. **Contrato:** materializar e testar snapshot imutável; início=fim fica como defesa adicional.

4. **Rodar `node_check` não prova que analisou a árvore pretendida.** Ele enumera JS na raiz do script, mas executa `node --check` dentro de `${CASCA:-saas_core}`, sem verificar identidade do `/app` desse container. O vínculo efetivo com a árvore viva/push é `[nao verificado]`. Evidência: `bin/node_check.sh:24–25`, `bin/node_check.sh:32–37`. **Contrato:** vincular explicitamente o analisador ao snapshot certificado.

5. **Hash Git de app não identifica todo `/app` observado pela suíte.** `--montagem <dir>` usa `staticfiles` da raiz do script e o sobrepõe em `/app/staticfiles`; esse diretório é ignorado pelo Git. Evidência: `bin/arvore_do_push.sh:19`, `bin/arvore_do_push.sh:28–30`, `bin/arvore_do_push.sh:50–55`; `.gitignore:13`. **Contrato:** registrar identidade dos mounts e entradas relevantes ou declarar precisamente que a identidade cobre somente a subárvore Git, sem chamar isso de igualdade de todo app efetivo.

6. **O modo novo precisa anteceder e desabilitar o cache da régua.** Atualmente a régua só interpreta `--only`; um argumento `--carimbar-da-copia` não impede a saída antecipada por fingerprint, anterior aos demais portões. Evidência: `bin/regua.sh:46–51`, `bin/regua.sh:84–94`, `bin/regua.sh:130`, `bin/regua.sh:138`, `bin/regua.sh:146`. **Contrato:** novo modo valida certificado e executa portões obrigatoriamente, sem passar pelo `JA VERDE`.

7. **O hash auxiliar não falha de forma fechada em toda etapa.** O rc de `git add -A --ignore-errors` é descartado; um `write-tree` posterior pode devolver um hash mesmo após erro de coleta. Evidência: `bin/arvore_hash.sh:27–28`. **Contrato:** todos os comandos de captura devem ter sucesso; erro invalida certificado/carimbo.

## 30/10 17:14 · sessao 7b71a8634ed9 · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **`node_check` pode conferir bytes de outra árvore.** Enumera os JS da raiz local, mas manda `node --check` ao container `${CASCA:-saas_core}`, sem montar a árvore candidata (`bin/node_check.sh:24-37`). A chamada obrigatória em C não prova identidade dos bytes. **Contrato:** conferir JS do snapshot certificado/push ou verificar explicitamente identidade do mount e conteúdo. Divergência real do container: [nao verificado].

2. **Hash de `app/` não identifica tudo que a suite leu.** A montagem suplementar usa `RAIZ` do script e monta seu `app/staticfiles`, independentemente do `--dir` recebido (`bin/arvore_do_push.sh:19`, `bin/arvore_do_push.sh:50-55`); `staticfiles/` é ignorado (`.gitignore:13`). A porta monta a candidata mais esses argumentos (`bin/suite.sh:74-79`, `bin/suite.sh:100`). **Contrato:** delimitar “árvore exata” como árvore efetivamente visível no container; identificar/fixar também montagens externas relevantes ou declará-las limite. Identidade atual dessas entradas entre cópia e pouso: [nao verificado].

3. **Certificado precisa identificar a execução, além de LABELS.** A porta usa imagem por tag mutável `latest`, env-files de sua própria raiz e banco opcional `REGUA_DB` (`bin/suite.sh:89`, `bin/suite.sh:98-103`). O runner muda a suite com `SO_QUARENTENA` e o relógio com `CONGELAR_EM` (`app/core/runner_quarentena.py:75-100`). **Contrato:** guardar imagem efetivamente usada e parâmetros que definem a execução; impedir certificado “inteira” produzido sob seleção informativa/argumentos filtrantes. Valores reais em produção: [nao verificado].

4. **Rodar um selo localizado na cópia não garante que ele examine a cópia.** Há raízes absolutas em `bin/tests/test_lei_akita.sh:17`, `bin/tests/test_cortes_registrados.sh:11`, `bin/tests/test_cortes_registrados.sh:24` e `bin/tests/test_fabricante_seco.sh:13`, `bin/tests/test_fabricante_seco.sh:23`. `hooks.sh check` também usa destino absoluto (`bin/hooks.sh:9-11`). **Contrato:** a classificação de “só na principal” deve auditar o alvo lido, inclusive selos verdes; medir apenas quais falham numa cópia limpa deixa passar verdes da árvore errada.

5. **Medida por obra exige definir o universo pelo runner.** O código admite módulos `tests.py`, além de `tests/test_*.py` (`bin/tests/test_labels_fonte_unica.sh:29-30`), e a suite real passa por poda de quarentena (`app/config/settings/ci.py:40`, `app/core/runner_quarentena.py:75-91`). **Contrato:** persistir numerador, denominador e identificação do universo por chamada; definir se o denominador é anterior ou posterior à quarentena, como tratar OBRA ausente e se INTEIRA por fallback entra como 100%. Implementação e agregação efetivas dessa medida: [nao verificado].

## 30/10 17:13 · sessao 786be39f25f8 · - · - · origem=Ronald (tmux)

- `lista()` lê fontes e monta o dicionário; não persiste resultado: `app/escala/services/cadastro_realidade.py:502`, `:513-575`, `:646-652`. As assinaturas hoje são C1–C9; não há C10 no registro nem ramo de fase deslocada na montagem: `app/escala/services/cadastro_realidade.py:36-107`, `:513-575`.

Leitores em produção:

| Leitor | Uso | Fonte |
|---|---|---|
| Tela | Chama `lista(empresas={visíveis})` e passa `dados` ao template | `app/escala/views.py:487-495` |
| PDF | O mesmo `dados`, gerado pela mesma chamada, vai a `pdf_bytes` | `app/escala/views.py:490-494`; renderização `app/escala/services/cadastro_realidade.py:674-723` |
| Template | Percorre postos, colaboradores e assinaturas; mostra detalhe/dono/ação; C7 tem ações POST adicionais | `app/templates/escala/cadastro_x_realidade.html:21-29` |
| `fechar_avisos_regua_na_lista` | Chama lista inteira, seleciona C7 e só cancela avisos cujos colaboradores ficaram comprovados | `app/escala/management/commands/fechar_avisos_regua_na_lista.py:26-49` |
| `fechar_chamados_vinculo_na_lista` | Exige lavra C8, chama lista inteira, seleciona C8 e só cancela comprovados | `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:30-59` |
| `vigia_fio.censo` | Chama lista uma vez por censo, com empresas visíveis; lê dias com dono lavrado BATIDA/CADASTRO; falha mantém alarme e conta ausência | `app/chamados/services/vigia_fio.py:244-266`; adaptador `app/escala/services/cadastro_realidade.py:655-671` |
| Placar estrutural R2 | **Referência textual**, não chamada executável de `lista`; `fonte` declara a função como origem | `app/core/placar_estrutural.py:140-149` |

LAVRA C3/C7/C8/C9:

| Assinatura | Campo/chave e leitura | Escritor | Cron declarado |
|---|---|---|---|
| C3 | `MetricaSnapshot.detalhe`, chave `previsto_cego`, escopo `frota`; último `data_ref/calculado_em`; formatador `_previsto_cego` | `lavrar_previsto_cego --apply`: `medir` e `update_or_create` | Diariamente 07:33 |
| | `app/escala/services/previsto_cego.py:58`, `:217-226`; `app/escala/services/cadastro_realidade.py:381-396` | `app/escala/management/commands/lavrar_previsto_cego.py:30-40` | `app/config/crons.py:495-496` |
| C7 | `MetricaSnapshot.detalhe.por_colab`, chave `termometro_regua`; itens contêm EC, classe, declarado/real, evidência, exemplo e destino | `termometro_regua --avaliar --lavrar` → `regua_na_lista.lavrar` | Diariamente 06:00 |
| | `app/escala/services/regua_na_lista.py:19-66`; `app/escala/services/cadastro_realidade.py:276-290` | `app/escala/management/commands/termometro_regua.py:41-78`, `:92-95` | `app/config/crons.py:409` |
| C8 | `MetricaSnapshot.detalhe.por_colab`, chave `vinculo_divergente`; `colabs_em_revisao` ainda compara posto lavrado com posto atual | `detectar_vinculo_divergente --lavrar` → `vinculo_na_lista.lavrar` | Segunda-feira 06:52 |
| | `app/ponto/services/vinculo_na_lista.py:21`, `:111-153`; `app/escala/services/cadastro_realidade.py:294-302` | `app/ponto/management/commands/detectar_vinculo_divergente.py:35-52` | `app/config/crons.py:578` |
| C9 | `MetricaSnapshot.detalhe.itens`, chave `par_separado_pelo_teto`; itens incluem data, EC/template do dia e dono do juiz | `lavrar_teto_do_dia --apply` → `teto_na_lista.medir/lavrar` | Diariamente 07:42 |
| | `app/ponto/services/teto_na_lista.py:25`, `:75-86`, `:110-130`; `app/escala/services/cadastro_realidade.py:306-318` | `app/ponto/management/commands/lavrar_teto_do_dia.py:26-34` | `app/config/crons.py:502-503` |

`MetricaSnapshot` tem campos `chave`, `escopo`, `escopo_id`, `data_ini`, `data_ref`, `numerador`, `denominador`, `valor`, JSON `detalhe` e `calculado_em`; o modelo não declara `db_table`: `app/inteligencia/models.py:81-110`. O timezone Django é `America/Sao_Paulo`: `app/config/settings/base.py:93`. Execução efetiva desses crons e horário do daemon no host: [não verificado].

**Existe dado lavrado de desvio 12x36, mas não o registro diário completo de `fora_de_fase`:**

- `censo_fase_12x36 --lavrar` grava snapshot chave `desvio_de_cadastro_12x36`, escopo `tenant`, `escopo_id=0`, com quantidade de linhas, IDs dos colaboradores em desvio, automáticos/propostas e esperado; **não grava datas dos furos/folgas, EC ou âncora sugerida por linha nesse snapshot**: `app/ponto/management/commands/censo_fase_12x36.py:192-209`.
- Esse é o escritor existente mais próximo em assunto: mede previsto por vínculo do dia, compara com turnos reais, calcula `furos`, `folga_trab`, proposta de âncora e paridade: `app/ponto/management/commands/censo_fase_12x36.py:102-168`. Seu cron roda diariamente 07:38, `--min-plantoes 6 --lavrar`: `app/config/crons.py:614-615`.
- O JSON opcional do censo contém EC, segmentos, âncora, contagens, virada e proposta; grava arquivo apenas com `--json`, opção ausente no cron: `app/ponto/management/commands/censo_fase_12x36.py:153-174`; `app/config/crons.py:614-615`.
- Há também snapshot `fase_conflitante`; responde FOTO versus ANCORA, não vida versus cadastro: `app/escala/services/fase_conflitante.py:11-24`, `:86-102`. Escritor `lavrar_fase_conflitante --apply`, diariamente 07:14: `app/escala/management/commands/lavrar_fase_conflitante.py:28-38`; `app/config/crons.py:479-480`.
- `fora_de_fase` calcula os dois conjuntos de dias, mas retorna apenas contagens e sugestão; não retorna datas nem `ec_id`, e não grava: `app/escala/services/fase_12x36.py:178-210`.
- A lavra do esmeril tem A2/A8 por colaborador, porém reduz assinaturas a contagens; snapshot e JSON preservam esse dicionário: `app/ponto/services/esmeril_espelho.py:214-231`, `:296-330`. A lista o lê pelo banco, nunca pelo arquivo: `app/ponto/services/esmeril_espelho.py:334-350`; `app/escala/services/cadastro_realidade.py:575-576`.
- Conteúdo real dos snapshots existentes no banco nesta instalação: [não verificado].

## 30/10 17:13 · sessao 4db30082a3b2 · - · - · origem=Ronald (tmux)

Padrões aplicáveis, com distinção entre intenção do desenho e execução atual:

- **Single Source of Truth / um juiz por pergunta** e **Single Writer Principle** são o norte explícito; “derivação paralela” é lógica de negócio duplicada proibida. O desenho de uma única sugestão realiza esse padrão, mas hoje as decisões estão duplicadas: `ancora_sugerida` exige piso/ganho e escolhe a melhor paridade, enquanto `fase_sugerida` decide por troca de ritmo, últimos dois plantões e desempate próprio. Referências: `app/docs/GLOSSARIO.md:12`, `app/docs/GLOSSARIO.md:23`, `app/docs/PLANO-PADROES.md:46`, `app/escala/services/fase_12x36.py:67`, `app/escala/services/fase_12x36.py:75`, `app/escala/services/fase_na_tela.py:62`, `app/escala/services/fase_na_tela.py:70`, `app/escala/services/fase_na_tela.py:78`, `app/escala/services/fase_na_tela.py:90`.

- **Bitemporal data / vínculo do dia x vínculo de hoje**: o desenho que escolhe o vínculo responsável pelo dia passado realiza o padrão. `vinculo_do_dia` privilegia `CelulaDia.escala_geradora`; sem geradora usa a vigência. O detector `fora_de_fase` hoje seleciona apenas vínculos ativos e usa aquele vínculo em toda a janela, incluindo os dias passados: esse caminho não faz a escolha por dia. Referências: `app/docs/GLOSSARIO.md:24`, `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:200`, `app/escala/alimentacao.py:212`, `app/escala/services/fase_12x36.py:106`, `app/escala/services/fase_12x36.py:173`, `app/escala/services/fase_12x36.py:181`.

- **CQRS / projeção de leitura**: lista lendo LAVRA pronta realiza a separação entre cálculo e leitura. C3, C7, C8 e C9 apenas formatam os dados lavrados; C9 conserva dia e dono do juiz. `dias_na_lista` também lê o resultado da lista, sem produzir um segundo retrato do dia. Referências: `app/docs/GLOSSARIO.md:22`, `app/docs/PLANO-PADROES.md:11`, `app/escala/services/cadastro_realidade.py:382`, `app/escala/services/cadastro_realidade.py:393`, `app/escala/services/cadastro_realidade.py:276`, `app/escala/services/cadastro_realidade.py:294`, `app/escala/services/cadastro_realidade.py:306`, `app/escala/services/cadastro_realidade.py:312`, `app/escala/services/cadastro_realidade.py:658`.

- **Célula soberana / fonte única e escritor único**: corrigir o passado pela porta formal de regeneração realiza a exceção expressamente documentada. A porta recalcula usando o cadastro corrigido, altera células geradas, registra DNA anterior, data e contador de regeneração, e grava auditoria; a documentação nomeia essa porta como exceção formal à imutabilidade. Referências: `app/docs/GLOSSARIO.md:11`, `app/ponto/portas/celula.py:44`, `app/ponto/portas/celula.py:46`, `app/ponto/portas/celula.py:100`, `app/ponto/portas/celula.py:243`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:299`.

- **Evento em vez de varredura / Transactional Outbox**: LAVRA por cron preserva cálculo fora do request, mas isso não demonstra realização de evento/outbox. O glossário aponta os crons como a varredura existente e declara outbox “a construir”; o plano mantém O-OUTBOX registrado e descreve o contrato ainda por escrever. Referências: `app/docs/GLOSSARIO.md:16`, `app/docs/GLOSSARIO.md:17`, `app/docs/PLANO-PADROES.md:10`, `app/docs/PLANO-PADROES.md:15`.

A lista atual monta os dados cadastrais de apresentação usando o vínculo **vigente hoje**; C9 conserva separadamente o vínculo e template do dia. Referências: `app/escala/services/cadastro_realidade.py:477`, `app/escala/services/cadastro_realidade.py:490`, `app/escala/services/cadastro_realidade.py:494`, `app/escala/services/cadastro_realidade.py:312`.

## 30/10 17:13 · sessao 6402924a7c99 · CODEX-CONTEXTO-DA-CASA · leitura · origem=Code

1) Sim. Recebi instruções apresentadas como AGENTS.md de `/tmp/codex_ctx_medida/6c37ad849616bb588f77ff3cc309c7a366cc3ad8/AGENTS.md`. [nao verificado]

2) Todas as linhas visíveis que começam com `PALAVRA-CODIGO`, literalmente: [nao verificado]

```text
PALAVRA-CODIGO-1: ARARA-TOPO
PALAVRA-CODIGO-2: BUGIO-31K
```

3) Há sinal de truncamento: a última linha visível está incompleta. Ela é, literalmente: [nao verificado]

```text
linha de enchimento para medir o corte do
```

## 30/10 17:06 · sessao 79968c2291fd · SUITE-POR-IMPACTO · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 30/10 17:04 · sessao 02042694a309 · - · - · origem=Ronald (tmux)

1. **“Inteira” precisa significar inventário efetivamente executado.** O CI usa `RunnerComQuarentena`, que poda testes e conserva o paralelismo; pode congelar o relógio. O mapa deve registrar testes descobertos/executados, skips, quarentena, labels, settings e parâmetros, recusando seleção quando a geração estiver incompleta ou incompatível. Fontes: `app/config/settings/ci.py:40`; `app/core/runner_quarentena.py:75`; `app/core/runner_quarentena.py:86`; `app/core/runner_quarentena.py:93`. A lista lê `ate` mas não filtra prazo em `ler_quarentena`; não presumir reinclusão automática só porque passou tempo: `app/core/quarentena_lista.py:20`; `app/core/quarentena_lista.py:25`.

2. **Import-time não equivale a arquivo sem contexto.** Há registro de signals durante `ready()` e decorators; há tuplas derivadas no import. Se outra função do mesmo arquivo executar em testes, o fallback “arquivo que NENHUM contexto tocou” não protege os leitores das partes importadas. Alterar contrato: dependências de import/configuração/registro precisam virar dependências conservadoras ou gatilho de inteira; contexto vazio não pode ser descartado como irrelevante. Fontes: `app/ponto/apps.py:7`; `app/ponto/signals.py:131`; `app/core/configuracao_efeito.py:278`; `app/core/configuracao_efeito.py:294`. O funcionamento interno de atribuição do contexto pela versão instalada de coverage não está na cópia: [não verificado].

3. **O scanner estático proposto perde contratos existentes.** Um teste chama o varredor por helper, ficando `os.walk` e `open` fora de sua fonte; outro inspeciona linhas das funções e o dispatch vivo sem `ast.parse`/`ast.walk`/`glob`. Alterar: inventário explícito dos consumidores de fonte, introspecção e helpers transitivos, obrigatório em toda seleção; descoberta incerta ⇒ inteira. Fontes: `app/core/tests/test_selo_sem_data_cravada.py:23`; `app/core/tests/test_selo_sem_data_cravada.py:28`; `app/core/datas_cravadas.py:80`; `app/core/datas_cravadas.py:96`; `app/ponto/tests/test_c3_ordem_dos_receivers_da_batida.py:56`; `app/ponto/tests/test_c3_ordem_dos_receivers_da_batida.py:72`; `app/ponto/tests/test_c3_ordem_dos_receivers_da_batida.py:93`.

4. **Cache e estado compartilhado tornam cobertura observada dependente da ordem.** O catálogo tem cache de módulo por 60 segundos; a IA evita executar o produtor quando encontra cache; a cifra usa `lru_cache`. Testes já limpam cache e registram cleanup, e há comentário documentando vazamento entre testes. Alterar: reconhecer que executar uma vez não identifica todos os consumidores; registrar políticas conservadoras para arquivos com caches/estado global e validar isolamento/ordem, com inteira quando não houver garantia. Fontes: `app/ponto/catalogo/ausencias.py:257`; `app/ponto/catalogo/ausencias.py:267`; `app/core/context_processors_ia.py:22`; `app/core/crypto_fields.py:10`; `app/ponto/tests/test_o124_medico_sai_do_cadastro.py:60`; `app/colaboradores/tests/test_porta_cadastro.py:109`.

5. **Prefixos de métodos não provam cobertura de todo o ciclo do teste.** A cópia usa `setUpTestData` herdado e cleanups; o contrato precisa provar atribuição correta de classe/módulo, setup de classe, corpo, teardown, fixtures e cleanup, inclusive em workers. O uso de `type(self)/cls.__module__` é compatível com a herança local; não há evidência para acusá-lo isoladamente de erro. Fontes: `app/ponto/tests/test_furo_parcial_sem_cobranca.py:34`; `app/ponto/tests/test_furo_parcial_sem_cobranca.py:87`; `app/ponto/tests/test_o124_medico_sai_do_cadastro.py:71`. Não encontrei fonte das bibliotecas na cópia que permita provar `start_dynamic_context`, propagação multiprocessing ou fechamento dos contextos: [não verificado]. Exigir prova pequena com diferentes módulos/workers e fases, confirmação de arquivos de dados por processo, ausência de contaminação entre contextos, caminho canônico `/app`→`app/`, isolamento entre gerações e rejeição de combine parcial. [não verificado]

6. **A configuração atual de coverage não serve como default silencioso.** Ela declara `source=apps` e omite config, `__init__.py`, migrations e testes; o container trabalha em `/app`. Alterar: rcfile novo integral e explícito, escopo real dos pacotes, política de exclusões e validação da presença de contextos de cada módulo esperado. Fontes: `app/.coveragerc:2`; `app/.coveragerc:4`; `app/.coveragerc:6`; `app/.coveragerc:8`; `app/Dockerfile:8`. Coverage 7.15.0, Django 5.2.14 e tblib 3.2.2 estão declarados, mas sua presença na imagem em execução segue [não verificado]: `app/requirements.txt:12`; `app/requirements.txt:13`; `app/requirements.txt:50`; o Dockerfile instala requirements em `app/Dockerfile:37`.

Também há testes lendo arquivo sem AST, como templates e requirements, e testes que pulam quando `bin/` não está montado. O contrato deve tratar isso como limite efetivo de observação, não como ausência de dependência. Fontes: `app/core/tests/test_r16_link_de_senha.py:35`; `app/core/tests/test_contract_tblib_no_paralelo.py:35`; `app/core/tests/test_apertos_esteira.py:26`; `app/core/tests/test_apertos_esteira.py:47`.

## 30/10 17:04 · sessao 20e314369dd1 · - · - · origem=Ronald (tmux)

1. **O diff prometido não é o diff descrito.** `git diff --name-only <base>` não inclui arquivos não rastreados; a porta atual já precisa consultar separadamente `git ls-files -o --exclude-standard` (`bin/arvore_do_push.sh:89-90`). Alterar contrato: unir diff tracked + untracked, usar caminhos terminados em NUL, contemplar exclusão/renomeação e erro de Git/base inexistente; qualquer erro ⇒ INTEIRA. O hash existente pretende cobrir tracked+untracked por índice descartável (`bin/arvore_hash.sh:4`, `bin/arvore_hash.sh:13-16`, `bin/arvore_hash.sh:27-28`).

2. **24 h é validade temporal, não validade causal.** O precedente de reutilização exige hash exato da árvore, impressão digital e status OK (`bin/pre-push.sh:61-64`); a régua recusa carimbo se o conteúdo muda durante o run (`bin/regua.sh:191-194`). Alterar contrato: armazenar hash de árvore do mapa, commit/base, versão de instrumento/plugin/configuração/labels/imagem; comparar mapa→cópia além do diff base→cópia e rejeitar geração se árvore mudou durante execução. Se base escolhida posterior ao mapa omite mudanças intermediárias, o mapa pode parecer jovem e servir uma dependência velha [não verificado: consequência lógica do contrato]. Data futura ou ilegível também deve invalidar mapa [não verificado: alteração proposta].

3. **A trava não pode envolver outra vez `suite.sh`.** A porta já executa a trava (`bin/suite.sh:96`); o risco de autoespera por aninhamento está explícito (`bin/suite.sh:17-19`). A trava global é `/tmp/juliani_db_test.lock`, espera 3600 s por default e retorna 75 sem executar comando quando não consegue a vez (`bin/trava_teste.sh:21-22`, `bin/trava_teste.sh:35-42`). Alterar contrato: uma única aquisição pela porta; propagar `--espera`; 75 ⇒ “não rodou”, sem publicar mapa ou classificar teste vermelho. `--quem` só lê um arquivo auxiliar (`bin/trava_teste.sh:28`), portanto não usá-lo como aquisição/precondição.

4. **`exec` final impede registrar medida depois.** `suite.sh` substitui seu processo pela trava/comando (`bin/suite.sh:96-103`). Alterar contrato: wrapper chama a porta como subprocesso, salva rc imediatamente e finaliza medida; não faz `exec suite.sh` se a mesma chamada precisa registrar duração/rc ao terminar. Usar arquivo de resultado por run e conclusão explícita; a semântica exata de `run_in_background` da sessão não está no código [não verificado].

5. **Veredito textual não substitui rc nem distingue cobertura de suite.** O pre-push guarda o rc real, filtra só para apresentação, trata 75 e mostra saída crua quando não há `Ran` (`bin/pre-push.sh:137-153`); a régua reconhece o erro de serialização `cannot pickle 'traceback' object` e ausência de `Ran` (`bin/regua.sh:172-179`). Alterar contrato: exigir rc da suite + `Ran` + último veredito compatíveis; separar `rc_suite`, `rc_coverage`, `rc_combine`, `rc_selos`. Coverage ausente, coleta parcial, morte de worker/OOM e suite interrompida não podem produzir mapa utilizável só porque existe JSON [não verificado: regras propostas]. Mapa vermelho pode ser preservado como diagnóstico, mas habilitar seleção com ele exige prova de completude [não verificado].

6. **A medida proposta não mede a meta global.** As suites existentes da régua e do pre-push continuam fora do novo wrapper (`bin/regua.sh:165-169`, `bin/pre-push.sh:182-184`); há ainda control-plane `tenants` com outro settings (`bin/pre-push.sh:194-195`). Alterar contrato: definir o que conta como suite inteira e instrumentar todas as portas que entram no numerador, incluindo geração, repetições e abortos que consumiram execução; acrescentar `run_id`, obra/cópia, árvore, labels e identificação do pouso. Separar espera, selos, execução Django e pós-processamento [não verificado: esquema proposto].

7. **“Log de pousos” precisa de definição.** O código registra lote e seus diretórios no integrador (`bin/integrador.sh:24-29`), enquanto o histórico append disponível é de deploy provado, condicionado por flags (`bin/deploy.sh:342-348`); o integrador acaba chamando o executor do lote (`bin/integrador.sh:51-52`). Alterar contrato: escolher fonte e definição de pouso, janela e regra de associação/deduplicação. Os valores “22 suites, 324 min, ~3,7/pouso” e a possibilidade de calcular ≤1,5 com os logs existentes não são verificáveis com o dado fornecido [não verificado].

8. **`ESCAPOU` precisa de observador, identidade e persistência.** O pre-push testa o app do commit exportado, separadamente da árvore de trabalho (`bin/pre-push.sh:27-38`), e a reutilização depende de igualdade comprovada (`bin/pre-push.sh:61-68`). Alterar contrato: correlação por hash da árvore completa e ambiente, lista de testes selecionados/executados e IDs das falhas; registrar suspensão persistente e consultá-la antes de toda seleção. “Desligar = não chamar” não força os demais construtores a obedecer [não verificado: lacuna do contrato]. Não há implementação atual de `ESCAPOU` verificada [não verificado].

9. **PRONTO 5 não é reproduzível só com os três nomes de módulos.** Os arquivos existem e usam datas relativas ao relógio (`app/ponto/tests/test_cartorio_veto_nao_e_emitido.py:33`, `app/ponto/tests/test_furo_parcial_sem_cobranca.py:48`, `app/ponto/tests/test_veto_que_cai_rejulga.py:41`). Alterar contrato: fixar árvore anterior, árvore defeituosa, base do mapa, momento da geração, data/configuração de reprodução e os cinco IDs exatos esperados; provar que a seleção executou esses IDs e reproduziu os cinco FAIL. O conteúdo dos commits históricos e os cinco FAIL alegados não foram fornecidos nem puderam ser verificados nesta cópia [não verificado].

## 30/10 17:04 · sessao e42d35092652 · - · - · origem=Ronald (tmux)

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 30/10 16:58 · sessao 0c34feaa35e2 · CALENDARIO-HOJE · verificador · origem=Code

**REPROVADO: procede a ressalva A5 sobre falta de teto absoluto para o caminho cego.** O selo citado pela triagem usa escala com fim declarado; o teste novo compara somente o custo de um e oito dias cegos. Nenhum deles limita o acréscimo constante exclusivo desse caminho. (`app/core/tests/test_selo_performance.py:283–300`, `:401–405`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:150–159`)

1. **O que o fallback mostra**

O ramo final agora mostra **`—` com `status_rotulo` no atributo `title`**. O motivo aparece no tooltip, sem virar texto visível permanente. Antes dele, ausência e palavra do dia podem interceptar a renderização, independentemente do status. (`app/templates/colaboradores/partials/_calendario_grade.html:97–107`)

| Status candidato ao fallback | Quem escreve o rótulo e qual texto chega |
|---|---|
| `sem_certidao`, presente em `_cego_folha` | `contexto_calendario`: **“Dia sem julgamento do cartorio -- a folha retem (sem_celula)”**. O mapa aceita exclusivamente o motivo `sem_celula` devolvido pela folha. (`app/colaboradores/services/calendario.py:64`, `:722–727`, `:899`) |
| `sem_certidao`, fora desse mapa | O mesmo produtor consulta `ROTULO_STATUS`: **“Sem certidao do dia”**. Esse status também nasce no caminho com batida sem classificação suficiente da grade. (`app/colaboradores/services/calendario.py:58`, `:159–169`, `:899–901`) |
| `sem_dados` | O mesmo produtor consulta `ROTULO_STATUS`: **“Sem dados”**. (`app/colaboradores/services/calendario.py:59`, `:158`, `:901`) |
| `ausencia` | Também não tem ramo próprio por status. Se não houver dicionário de ausência nem palavra que intercepte, chega **“Ausencia registrada — o dia tem abono proprio”**. Normalmente o dicionário produzido de `_a` ganha antes. (`app/templates/colaboradores/partials/_calendario_grade.html:97–107`; `app/colaboradores/services/calendario.py:57`, `:838–848`, `:901`) |

Esses rótulos não são vazios no produtor. A frase específica do cego está condicionada à resposta da folha, portanto não é aplicada indiscriminadamente a todo `sem_certidao`. (`app/colaboradores/services/calendario.py:57–64`, `:727`, `:899–901`)

Há uma ressalva semântica em `ausencia`: “tem abono próprio” não descreve todo efeito possível, pois `_classificar` aceita ausência que não desconta, incluindo tipos `SUPRIME`. Para esse texto aparecer no fallback, ainda precisam faltar ausência e palavra; ocorrência concreta no dado: **[não verificado]**. (`app/colaboradores/services/calendario.py:124–125`; `app/ponto/catalogo/ausencias.py:89–94`; `app/templates/colaboradores/partials/_calendario_grade.html:97–107`)

2. **Quem consome e o que muda**

- **Perfil do colaborador, no gestor:** a rota renderiza `calendario_partial.html`, que força `layout='mes'`: sete colunas, offsets e células de 132px. Recebe o novo tooltip. (`app/colaboradores/views.py:1945–1962`; `app/templates/colaboradores/calendario_partial.html:62`; `app/templates/colaboradores/partials/_calendario_grade.html:64–68`, `:107`)
- **Drawer do painel:** inclui com `layout='fita'`: faixa horizontal, células de 78px e marcações de mês/segunda. Recebe o mesmo tooltip. (`app/templates/colaboradores/partials/_drawer_diagnostico.html:35`; `app/templates/colaboradores/partials/_calendario_grade.html:63–68`, `:107`)
- **Cartão HTML da auditoria de espelhos:** inclui somente `dias/resumo`, sem layout; usa o grid automático de mínimo 62px. Também recebe o tooltip. O produtor declara `datas_em_aberto=NAO_PERGUNTADO`. (`app/templates/relatorios/furos.html:42`; `app/templates/colaboradores/partials/_calendario_grade.html:65`, `:107`; `app/relatorios/furos.py:184–188`)
- **PDF de furos:** não renderiza esse partial. Extrai apenas data/status e desenha via ReportLab; não recebe `status_rotulo`. `sem_certidao` cai nas cores padrão branco/cinza. O último ajuste do template não chega ao PDF. (`app/relatorios/views.py:1548–1562`; `app/relatorios/furos_pdf.py:11–21`, `:53–57`)

3. **O teste novo morde, com alcance limitado** [nao verificado]

Por inspeção, pega a regressão literal de devolver o fallback a `—` sem rótulo: exige `a folha retem (sem_celula)` no HTML dos três layouts. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:103–105`; `app/templates/colaboradores/partials/_calendario_grade.html:107`)

Deixa passar esta mutação:

```html
<span title="a folha retem (sem_celula)">—</span>
```

Ela abandona a leitura de `dia.status_rotulo`, mas preserva a substring exigida. Também passaria com essa frase em comentário HTML ou conteúdo oculto; a asserção não verifica atributo, elemento ou visibilidade. Os outros status do fallback não são exercitados pelo acréscimo. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99–105`)

Execução dos testes e resultados RED/GREEN alegados: **[não verificado]**. [nao verificado]

4. **Conferência da triagem5**

- **Item 2:** a justificativa corresponde ao código: ausência, feriado, folga humana e escala suspeita precedem `retido_folha`; o ramo cego da folha não consulta essas precedências. (`app/colaboradores/services/calendario.py:124–144`; `app/folha/export.py:1237–1238`)
- **Item 3:** a divergência contratual existe como possibilidade: a folha recorta admissão/demissão e excepciona a demissão no ramo cego; o calendário ainda pode alcançar `falta`. A atribuição histórica ao main: **[não verificado]**. (`app/folha/export.py:1180–1187`, `:1211–1214`, `:1237`; `app/colaboradores/services/calendario.py:133–148`)
- **Item 4 — justificativa incorreta:** existe teto 55, mas a fixture não prepara o dia sem fim previsto que aciona a consulta condicional à folha. A igualdade entre um e oito dias permite custo fixo arbitrariamente alto nesse ramo. Isso prova uma lacuna de cobertura, não um estouro efetivo em produção. (`app/core/tests/test_selo_performance.py:249`, `:283–300`, `:401–405`; `app/colaboradores/services/calendario.py:722–727`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:158–159`)
- **Item 5:** a limitação atual do PDF está confirmada; sua anterioridade é **[não verificado]**. A explicação de `resumo.faltas` corresponde ao cálculo: só conta `falta`, embora a interface chame esse número de “Sem registro”. (`app/relatorios/furos_pdf.py:11–21`; `app/colaboradores/services/calendario.py:950`; `app/templates/colaboradores/partials/_calendario_grade.html:145–147`)

## 30/10 16:57 · sessao 27e0f3394207 · - · - · origem=Ronald (tmux)

No caminho normal, o fallback recebe `sem_certidao` e `sem_dados`; os rótulos são preenchidos pelo produtor e não são vazios (`app/colaboradores/services/calendario.py:58-59`, `app/colaboradores/services/calendario.py:899-901`). Não achei consumidor de produção que monte esses dias sem passar pelo produtor (`app/colaboradores/views.py:1945-1962`; `app/colaboradores/services/drawer.py:120-121`; `app/relatorios/furos.py:184-188`).

`ausencia` também é candidato sintático ao fallback: não há ramo específico para esse status (`app/templates/colaboradores/partials/_calendario_grade.html:97-107`). Normalmente o dicionário `ausencia` intercepta antes, pois nasce de `_a` (`app/colaboradores/services/calendario.py:805`, `app/colaboradores/services/calendario.py:838-848`). Um caminho excepcional pode chegar ao fallback: falha na carga `_aus_map`, A2 disponível retornando `ausencia`, ausência de palavra ou veredito `em_aberto` (`app/colaboradores/services/calendario.py:593-600`, `app/colaboradores/services/calendario.py:124-125`, `app/templates/colaboradores/partials/_calendario_grade.html:97-98`). O rótulo seria “Ausencia registrada — o dia tem abono proprio”, potencialmente falso para pedido pendente ou efeito supressor; ocorrência concreta no dado: [não verificado].

Sobre o resumo, `faltas` conta somente status `falta`, e a interface o chama “Sem registro” (`app/colaboradores/services/calendario.py:950`; `app/templates/colaboradores/partials/_calendario_grade.html:145-147`). A justificativa da triagem5(b) corresponde ao cálculo; a nomenclatura é mais ampla que o conjunto contado, mas não demonstra regressão nova por si só (`app/colaboradores/services/calendario.py:135-158`, `app/colaboradores/services/calendario.py:950`).

A suspeita sobre triagem4 é procedente: o selo geral usa escala com fim declarado e não exercita o DNA sem `hf`; o teste novo compara apenas custo de um versus oito dias cegos. Uma despesa fixa arbitrariamente alta exclusivamente nesse ramo passa pelos dois testes (`app/core/tests/test_selo_performance.py:283-300`, `app/core/tests/test_selo_performance.py:401-405`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:150-159`).

## 30/10 16:56 · sessao 2f7bd9efcc83 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Triagem 4 tem uma justificativa incorreta:** há teto absoluto de 55 queries na rota, mas a fixture usa escala com início/fim completos e não prepara dia sem fim previsto; esse selo não cobre o custo absoluto da chamada condicional à folha. O teste novo compara somente custos de 1 e 8 dias e aceita qualquer acréscimo constante comum aos dois casos. (`app/core/tests/test_selo_performance.py:283`, `:297`, `:401`; `app/colaboradores/services/calendario.py:722`; `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:158`)
- O teste de render **detecta a regressão literal** de restaurar o fallback `—` sem o rótulo nos três layouts, porque exige a substring no HTML. **Deixa passar** um título fixo com `a folha retem (sem_celula)`, sem ler `dia.status_rotulo`; também aceita o texto em comentário ou elemento oculto e não exercita os outros status do fallback. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:99`, `:103`, `:105`; `app/templates/colaboradores/partials/_calendario_grade.html:107`)
- Triagem 2 é compatível com o código: ausência/feriado/folga humana e escala suspeita têm precedência sobre `retido_folha`; a retenção cega da folha não faz essas consultas. (`app/colaboradores/services/calendario.py:124`, `:131`, `:139`, `:141`; `app/folha/export.py:1237`)
- Triagem 3 descreve uma divergência possível: a folha recorta admissão/demissão e exclui demissão do ramo cego, enquanto a classificação do calendário pode chegar a falta. A afirmação histórica sobre o main não foi comprovada por histórico local; o diff fornecido mostra o ramo de falta preexistente à inserção. (`app/folha/export.py:1180`, `:1211`, `:1237`; `app/colaboradores/services/calendario.py:133`, `:148`; histórico: [não verificado])
- Triagem 5b é coerente: `resumo.faltas` conta exclusivamente status `falta`, portanto o dia classificado como `sem_certidao` sai dessa contagem. (`app/colaboradores/services/calendario.py:948`, `:950`)

Os resultados alegados de green7/green8 e custos efetivos não foram verificados. [não verificado]

_632 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
