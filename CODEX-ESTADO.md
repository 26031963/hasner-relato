# CODEX-ESTADO

> Ultima resposta processada: **18/11/2026 09:01** · publicado em **18/11/2026 09:00** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 18/11 09:01 · sessao d526f93f08a1 · ORDEM-1811-ITEM8 · sonda · origem=Code

Os dados das tabelas vêm da sonda fornecida no enunciado; sua leitura de produção não foi verificada independentemente. **[não verificado]** Os marcos faltantes são inferências pela ordem e pelo tipo das batidas, não a lista de `marcos_faltantes` gravada em cada chamado. A convenção é `hi` = entrada, `hii` = saída para intervalo, `hfi` = volta e `hf` = saída final. [app/ponto/tests/test_intervalo_ausente.py:17](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/tests/test_intervalo_ausente.py:17)

Nas tabelas, `E` = entrada e `S` = saída; todas as batidas listadas são de app e os horários são locais. Datas são de 2026. Os donos são os retornos informados pela sonda. **[não verificado]**

### col868

Marcos esperados: `hi` 19:00, `hii` 01:00, `hfi` 02:00, `hf` 07:00. **[não verificado]**

| Dia | Marco faltante inferido | Batidas | Dono |
|---|---|---|---|
| 23/10 | hii | 23/10 18:58 E; 24/10 01:46 E, 02:45 E, 06:53 S | CADASTRO |
| 25/10 | hii | 25/10 18:58 E; 26/10 01:45 E, 02:37 E, 06:53 S | CADASTRO |
| 27/10 | hii | 27/10 18:59 E; 28/10 01:47 E, 02:49 E, 06:51 S | CADASTRO |
| 29/10 | hii | 29/10 18:45 E; 30/10 01:50 E, 03:25 E, 07:00 S | CADASTRO |
| 31/10 | hii | 31/10 18:54 E; 01/11 02:11 E, 03:25 E, 06:55 S | CADASTRO |
| 02/11 | hii | 02/11 18:58 E; 03/11 01:46 E, 02:49 E, 06:51 S | CADASTRO |
| 04/11 | hii | 04/11 19:14 E; 05/11 01:55 E, 02:53 E, 06:59 S | CADASTRO |
| 06/11 | hii | 06/11 18:45 E; 07/11 02:00 E, 03:05 E, 06:53 S | CADASTRO |
| 08/11 | hii, hfi, hf | 08/11 18:57 E | BATIDA |
| 14/11 | hii, hfi | 14/11 18:42 E; 15/11 07:03 S | CADASTRO |
| 16/11 | Indeterminado: há quatro batidas e o agregado informa um furo e uma órfã; a sonda não mostra o casamento por marco. **[não verificado]** | 16/11 19:16 E; 17/11 03:39 S, 04:35 E, 06:49 S | CADASTRO |

Nos oito primeiros dias, não há `S` na sequência do intervalo; há duas entradas intermediárias. Qual delas foi associada a `hfi` e qual ficou órfã não é mostrado. **[não verificado]**

### col934

Marcos esperados: `hi` 23:00, `hii` 05:00, `hfi` 06:00, `hf` 11:00. **[não verificado]**

| Dia | Marcos faltantes inferidos | Batidas | Dono |
|---|---|---|---|
| 06/11 | hi, hii, hfi, hf | Nenhuma | CADASTRO |
| 07/11 | hii, hfi | 07/11 23:00 E; 08/11 10:45 S | CADASTRO |
| 09/11 | hii, hfi | 09/11 22:57 E; 10/11 11:09 S | CADASTRO |
| 11/11 | hii, hfi | 11/11 23:01 E; 12/11 11:00 S | CADASTRO |
| 13/11 | hii, hfi | 13/11 22:57 E; 14/11 11:00 S | CADASTRO |
| 15/11 | hii, hfi | 15/11 22:58 E; 16/11 11:04 S | CADASTRO |
| 17/11 | hii, hfi, hf | 17/11 23:27 E | BATIDA |

### Chamados

Todos têm tipo **Ausência / Batida não realizada** e campo `motivo = None`; a sonda mostra que existe a chave `marcos_faltantes` no contexto, mas não seu conteúdo. Os dias e donos abaixo são os informados pela sonda. **[não verificado]**

| Colab | ID | Dia | Motivo | Dono |
|---|---:|---|---|---|
| col868 | 39641 | 23/10 | None | CADASTRO |
| col868 | 39642 | 25/10 | None | CADASTRO |
| col868 | 39643 | 27/10 | None | CADASTRO |
| col868 | 39644 | 29/10 | None | CADASTRO |
| col868 | 39645 | 31/10 | None | CADASTRO |
| col868 | 39646 | 02/11 | None | CADASTRO |
| col868 | 39647 | 04/11 | None | CADASTRO |
| col868 | 39648 | 06/11 | None | CADASTRO |
| col868 | 39649 | 08/11 | None | BATIDA |
| col868 | 39650 | 14/11 | None | CADASTRO |
| col868 | 39651 | 16/11 | None | CADASTRO |
| col934 | 39652 | 07/11 | None | CADASTRO |
| col934 | 39653 | 09/11 | None | CADASTRO |
| col934 | 39654 | 11/11 | None | CADASTRO |
| col934 | 39655 | 13/11 | None | CADASTRO |
| col934 | 39656 | 15/11 | None | CADASTRO |
| col934 | 39664 | 17/11 | None | BATIDA |

Não há chamado listado para o furo de col934 em 06/11; a sonda não informa a razão. **[não verificado]** Os chamados 39641–39650 aparecem criados às 08:19, com precisão de minuto, portanto não se pode estabelecer se foram criados estritamente depois de 08:19:00. **[não verificado]**

A sonda não mostra o casamento efetivo das batidas nem o conteúdo dos marcos cobrados. Também não resolve um dono que dependa da lista `cxr` quando ela não está disponível com `sem_motor=True`: nesse ramo, o código retorna `None`; nenhum dos donos exibidos nas tabelas veio como `None` ou `ESTRUTURA`. [app/ponto/management/commands/e6_oraculo.py:189](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/e6_oraculo.py:189) **[não verificado]**

## 18/11 09:01 · sessao 04f507b131aa · ESCALA-NAO-DESCREVE · censo · origem=Code

**O censo não está fechado.** A inspeção encontrou consumidores relevantes e uma segunda escrita da chave na sombra, mas não permite afirmar uma lista exaustiva de acessos dinâmicos ou de todos os escritores dos campos de `EscalaColaborador`. O diretório fornecido também não é um repositório Git identificável por `git rev-parse`; a identidade do HEAD e do hash informado fica **[não verificado]**.

Os caminhos abaixo são relativos a `app/`, salvo `bin/`. Os dados de produção atuais, inclusive o conteúdo efetivo da lavra e a contagem de órfãs, permanecem **[não verificado]**.

### 1. Escritores e leitores encontrados, por objeto

**(a) Snapshot e arquivo da lavra do esmeril.**

- `anomalias_da_frota` monta `lavrado_em`, `universo`, `colabs_com_anomalia_recorrente`, `por_assinatura`, `por_colab` e `destino`; só coloca uma pessoa em `por_colab` e chama o proponente quando há `sig`. [ponto/services/esmeril_espelho.py:206](app/ponto/services/esmeril_espelho.py#L206)
- `lavrar` chama o montador e `gravar_lavra`; depois escreve o mesmo objeto em arquivo temporário e o substitui no caminho configurado. `gravar_lavra` faz `update_or_create` do snapshot e grava `detalhe=d`. [ponto/services/esmeril_espelho.py:242](app/ponto/services/esmeril_espelho.py#L242), [ponto/services/esmeril_espelho.py:296](app/ponto/services/esmeril_espelho.py#L296), [ponto/services/esmeril_espelho.py:317](app/ponto/services/esmeril_espelho.py#L317)
- **Outra escrita da mesma chave existe na sombra:** `bin/sombra.sh` copia o `detalhe` de produção e faz `update_or_create` diretamente na chave, além de gravar o marcador de MD5. Isso é um caminho de reposição da sombra, não uma chamada a `gravar_lavra`. [bin/sombra.sh:138](bin/sombra.sh#L138), [bin/sombra.sh:151](bin/sombra.sh#L151)
- `ler_lavra` busca o snapshot mais recente, verifica o MD5 quando em sombra e devolve `detalhe`; o código informa que deixou de usar o JSON como fonte. A busca pelo caminho literal e pela configuração identificou o escritor do arquivo, mas nenhum leitor atual do JSON com esse nome; leitores por caminho construído dinamicamente ficam **[não verificado]**. [ponto/services/esmeril_espelho.py:334](app/ponto/services/esmeril_espelho.py#L334)
- Leitores identificados do snapshot por `ler_lavra`: lista Cadastro x Realidade; Pauta do esmeril; lista resumida para a revisão semanal; bloco “sem marco”; propostas automáticas; API de anomalias; comando do esmeril. [escala/services/cadastro_realidade.py:872](app/escala/services/cadastro_realidade.py#L872), [ponto/services/pautas_do_esmeril.py:52](app/ponto/services/pautas_do_esmeril.py#L52), [ponto/services/esmeril_espelho.py:356](app/ponto/services/esmeril_espelho.py#L356), [chamados/services/validacao.py:1218](app/chamados/services/validacao.py#L1218), [escala/services/escala_auto.py:163](app/escala/services/escala_auto.py#L163), [api/views_mensageria.py:1786](app/api/views_mensageria.py#L1786), [ponto/management/commands/esmeril_espelho.py:15](app/ponto/management/commands/esmeril_espelho.py#L15)
- `bin/sombra.sh` também lê diretamente o snapshot e serializa o detalhe inteiro; o placar encontrado chama o comando e extrai seu contador textual. A função genérica `inteligencia.metricas.serie(chave, ...)` pode buscar snapshots pela chave recebida; uso dela com esta chave por chamador dinâmico fica **[não verificado]**. [bin/sombra.sh:141](bin/sombra.sh#L141), [bin/placar_code.sh:389](bin/placar_code.sh#L389), [inteligencia/metricas.py:62](app/inteligencia/metricas.py#L62)

**Colisões de nome:** `ata.orfas` é a lista da célula usada no cálculo de A5; não é o novo mapa superior proposto. O `destino` da C7 vem da lavra do termômetro, não do mapa `destino` do esmeril. A `proposta` lida por `_destino` é o resultado do proponente, antes de virar frase. [ponto/services/esmeril_espelho.py:115](app/ponto/services/esmeril_espelho.py#L115), [escala/services/cadastro_realidade.py:314](app/escala/services/cadastro_realidade.py#L314), [ponto/services/esmeril_espelho.py:234](app/ponto/services/esmeril_espelho.py#L234)

**(b) `assinaturas_do_colab` e `janela`.** A função retorna um dicionário de assinaturas; há retorno antecipado sem células e outro para `so_escala=True`, antes do laço que percorre órfãs. `janela` retorna início da competência aberta mais antiga alcançável e ontem. Chamadores identificados: `anomalias_do_colab` e `anomalias_da_frota` chamam ambas; `escala_certa` chama assinaturas com `so_escala=True`. [ponto/services/esmeril_espelho.py:38](app/ponto/services/esmeril_espelho.py#L38), [ponto/services/esmeril_espelho.py:79](app/ponto/services/esmeril_espelho.py#L79), [ponto/services/esmeril_espelho.py:192](app/ponto/services/esmeril_espelho.py#L192), [ponto/services/esmeril_espelho.py:206](app/ponto/services/esmeril_espelho.py#L206), [escala/services/escala_certa.py:27](app/escala/services/escala_certa.py#L27)

**(c) `lista`, `pdf_bytes`, `dias_na_lista`, `_vinculos`, `acao`.** Os consumidores encontrados e o contrato de `postos` estão no item 4. `_vinculos` consulta o vínculo vigente e fornece os dados usados por `lista`; `acao` transforma código e vínculo em rótulo e URL. A2/A8 hoje apontam ao vínculo; A11 e C11 apontam ao Plano de Folgas. [escala/services/cadastro_realidade.py:748](app/escala/services/cadastro_realidade.py#L748), [escala/services/cadastro_realidade.py:656](app/escala/services/cadastro_realidade.py#L656), [escala/services/cadastro_realidade.py:708](app/escala/services/cadastro_realidade.py#L708)

**(d) Porta do vínculo.** Os chamadores e escritas identificados estão no item 5. A relação global de todos os escritores desses mesmos campos no projeto fica **[não verificado]**; o código da própria porta chama `fechar_vigencia` e `absorver_vigencias_posteriores`, além de criar ou atualizar o vínculo. [colaboradores/services/vinculo.py:211](app/colaboradores/services/vinculo.py#L211), [colaboradores/services/vinculo.py:253](app/colaboradores/services/vinculo.py#L253), [colaboradores/services/vinculo.py:259](app/colaboradores/services/vinculo.py#L259)

**(e) View e template.** A tela exige `ver_escala`, chama `lista` com empresas visíveis e passa o resultado ao template ou PDF. As rotas C7 usam POST e `_is_gestor`; essa função aceita `is_staff` ou grupo `dp_rh`, `gestor` ou `admin_ti`. A realocação C7 lê destino da lavra do termômetro pelo vínculo, não do corpo do POST. [escala/views.py:440](app/escala/views.py#L440), [escala/views.py:462](app/escala/views.py#L462), [escala/views.py:475](app/escala/views.py#L475), [chamados/views.py:24](app/chamados/views.py#L24), [chamados/views.py:35](app/chamados/views.py#L35), [escala/urls.py:14](app/escala/urls.py#L14)

O template estende `base.html` e inclui `core/_btn_pdf.html`; a referência de produção encontrada que o renderiza é a view citada. Inclusões ou extensões por nome montado dinamicamente ficam **[não verificado]**. [templates/escala/cadastro_x_realidade.html:1](app/templates/escala/cadastro_x_realidade.html#L1), [templates/escala/cadastro_x_realidade.html:11](app/templates/escala/cadastro_x_realidade.html#L11), [escala/views.py:492](app/escala/views.py#L492)

### 2. Efeito das duas chaves novas no topo

**Sim, há mudança observável na sombra.** `_md5_da_lavra` serializa o detalhe inteiro com chaves ordenadas: acrescentar `orfas` e `proposta` altera o hash. Se detalhe e marcador forem copiados juntos, ambos usam o mesmo cálculo; se apenas um mudar, a guarda levanta `SemLavra`. Além disso, a reposição em `bin/sombra.sh` grava `valor=len(detalhe)`, que cresce em dois para um detalhe antes sem essas chaves. [ponto/services/esmeril_espelho.py:260](app/ponto/services/esmeril_espelho.py#L260), [ponto/services/esmeril_espelho.py:266](app/ponto/services/esmeril_espelho.py#L266), [bin/sombra.sh:155](bin/sombra.sh#L155)

A API de anomalias seleciona explicitamente quatro chaves e não inclui os novos mapas; o comando do placar também lê campos existentes, e o shell extrai somente a linha de contagem. Esses leitores identificados não passam a expor as chaves novas por adicioná-las ao detalhe. [api/views_mensageria.py:1794](app/api/views_mensageria.py#L1794), [ponto/management/commands/esmeril_espelho.py:21](app/ponto/management/commands/esmeril_espelho.py#L21), [bin/placar_code.sh:389](bin/placar_code.sh#L389)

Que **nenhum outro** leitor genérico do snapshot itere ou serialize o detalhe inteiro fica **[não verificado]**. [inteligencia/metricas.py:62](app/inteligencia/metricas.py#L62)

### 3. Pressuposto sobre `por_colab` e `destino`

**Sim.** O montador só insere em `por_colab` quando `sig` é verdadeiro e usa `len(por_colab)` como número de pessoas com anomalia recorrente; `gravar_lavra` também usa esse comprimento como numerador. A Pauta percorre as assinaturas de cada entrada, e o bloco “sem marco” trata a presença da pessoa como indicação de assinaturas. [ponto/services/esmeril_espelho.py:220](app/ponto/services/esmeril_espelho.py#L220), [ponto/services/esmeril_espelho.py:323](app/ponto/services/esmeril_espelho.py#L323), [ponto/services/pautas_do_esmeril.py:27](app/ponto/services/pautas_do_esmeril.py#L27), [chamados/services/validacao.py:1229](app/chamados/services/validacao.py#L1229)

Manter `por_colab` e `destino` byte a byte equivalentes enquanto os mapas novos ficam em chaves separadas preserva **essas leituras identificadas**; isso não preserva o MD5 do detalhe inteiro, como acima. Preservação de todos os consumidores possíveis fica **[não verificado]**. [escala/services/cadastro_realidade.py:872](app/escala/services/cadastro_realidade.py#L872), [ponto/services/esmeril_espelho.py:260](app/ponto/services/esmeril_espelho.py#L260)

### 4. Retorno de `cadastro_realidade.lista`

O retorno atual é `None` se não houver `por_colab` nem cadastro; caso contrário, contém `lavrado_em`, `universo`, `colabs_com_anomalia_recorrente`, `colabs_do_cadastro`, `colabs_listados`, `por_assinatura`, `previsto_cego` e `postos`. `postos` é lista de pares `(nome_do_posto, lista_de_linhas)`, com linhas ordenadas por nome dentro de cada posto. [escala/services/cadastro_realidade.py:872](app/escala/services/cadastro_realidade.py#L872), [escala/services/cadastro_realidade.py:930](app/escala/services/cadastro_realidade.py#L930), [escala/services/cadastro_realidade.py:956](app/escala/services/cadastro_realidade.py#L956)

Cada linha começa com `colab_id`, `nome`, `matricula`, `empresa_id`, `busca`, `posto`, `posto_id`, `template`, `template_id`; recebe `destino` e `assinaturas`. Assinaturas carregam campos comuns como código, descrição, dono, ocorrências, ação e URL, podendo incluir campos específicos da classe. [escala/services/cadastro_realidade.py:754](app/escala/services/cadastro_realidade.py#L754), [escala/services/cadastro_realidade.py:887](app/escala/services/cadastro_realidade.py#L887), [escala/services/cadastro_realidade.py:912](app/escala/services/cadastro_realidade.py#L912)

Leitores identificados:

- Template: contadores, `previsto_cego`, `por_assinatura`, pares de `postos`, nome, matrícula, template, destino e campos das assinaturas, inclusive os dados C7 para formulários. [templates/escala/cadastro_x_realidade.html:8](app/templates/escala/cadastro_x_realidade.html#L8), [templates/escala/cadastro_x_realidade.html:17](app/templates/escala/cadastro_x_realidade.html#L17), [templates/escala/cadastro_x_realidade.html:21](app/templates/escala/cadastro_x_realidade.html#L21)
- PDF: contadores, `lavrado_em`, `postos`, nome, matrícula, template, assinaturas, ação e destino; percorre a ordem que recebe. [escala/services/cadastro_realidade.py:1073](app/escala/services/cadastro_realidade.py#L1073), [escala/services/cadastro_realidade.py:1083](app/escala/services/cadastro_realidade.py#L1083)
- `dias_na_lista`: pares de `postos`, `colab_id`, assinaturas com `data`, `codigo` e `dono_do_dia`. O vigia chama `lista` e `dias_na_lista` e também extrai diretamente os IDs das linhas. [escala/services/cadastro_realidade.py:1030](app/escala/services/cadastro_realidade.py#L1030), [chamados/services/vigia_fio.py:190](app/chamados/services/vigia_fio.py#L190)
- Comandos de fechamento de avisos C7 e chamados C8: pares de `postos`, `colab_id`, `assinaturas`, `codigo` e `detalhe`. [escala/management/commands/fechar_avisos_regua_na_lista.py:27](app/escala/management/commands/fechar_avisos_regua_na_lista.py#L27), [ponto/management/commands/fechar_chamados_vinculo_na_lista.py:31](app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py#L31)

Assim, remover ou mudar a forma de `postos` sem adaptar esses leitores altera ou quebra seus percursos; acrescentar uma lista separada e conservar a forma antiga evita essa incompatibilidade específica. [escala/services/cadastro_realidade.py:1044](app/escala/services/cadastro_realidade.py#L1044), [chamados/services/vigia_fio.py:196](app/chamados/services/vigia_fio.py#L196)

### 5. Chamadas, datas, autorização e trilha do vínculo

- **C7 da lista:** `realocar_regua_da_lista` obtém o destino do termômetro e chama `realocar_vinculo_para_destino`; o núcleo usa `timezone.localdate()` como início. A borda exige login, POST e `_is_gestor`. [escala/views.py:440](app/escala/views.py#L440), [escala/views.py:462](app/escala/views.py#L462), [escala/regua_defesa.py:310](app/escala/regua_defesa.py#L310)
- **Chamado antigo:** `realocar_para_destino` usa `contexto_json['destino']`, chama o mesmo núcleo com início hoje e, no sucesso, anexa ocorrência com origem, destino, data e usuário. A view exige gestor e aplica a checagem contextual indicada no código. [chamados/views.py:418](app/chamados/views.py#L418), [chamados/views.py:449](app/chamados/views.py#L449), [escala/regua_defesa.py:326](app/escala/regua_defesa.py#L326)
- **Tela de vínculo:** passa `request.POST` ao juiz, portanto a data é a fornecida no formulário; exige POST e `editar_escalas`. [colaboradores/views.py:391](app/colaboradores/views.py#L391), [colaboradores/views.py:428](app/colaboradores/views.py#L428)
- **Chat:** o preview chama o juiz; na confirmação, a data vem da proposta de sessão e a borda exige `editar_escalas`. O executor registra ainda um log específico de troca pelo chat. [colaboradores/services/troca_pelo_chat.py:50](app/colaboradores/services/troca_pelo_chat.py#L50), [core/views_ia.py:945](app/core/views_ia.py#L945), [core/views_ia.py:997](app/core/views_ia.py#L997)
- **Comando de cura de fase:** usa `x['virada']` como início e `x['proposta_ancora']` como âncora; seleciona o primeiro superusuário ativo para assinar a trilha. [ponto/management/commands/censo_fase_12x36.py:363](app/ponto/management/commands/censo_fase_12x36.py#L363), [ponto/management/commands/censo_fase_12x36.py:423](app/ponto/management/commands/censo_fase_12x36.py#L423)

A porta não faz, por si, uma checagem geral de staff ou de `editar_escalas`: o juiz lê data e template e aplica seus vetos de domínio; as permissões citadas estão nas bordas. O núcleo de realocação recusa destino cíclico e passa o perfil e o request do chamador. [colaboradores/services/vinculo.py:52](app/colaboradores/services/vinculo.py#L52), [escala/regua_defesa.py:303](app/escala/regua_defesa.py#L303)

Na execução, vínculos anteriores são fechados em D−1 e posteriores podem ser absorvidos; o existente da mesma data recebe tipo, posto, `ativa=True` e `data_fim=None`, ou um novo é criado com posto, tipo, início e ativo. A trilha usa o usuário do request: fechamento registra antes/depois de fim e ativo com `quem` e motivo; absorção registra antes/depois e histórico; o vínculo criado ou editado recebe log por PK e foto dos campos, além de `registrar_mudanca` com texto anterior/posterior e motivo `correcao` ou `mudanca`. [colaboradores/services/vinculo.py:211](app/colaboradores/services/vinculo.py#L211), [colaboradores/services/vinculo.py:259](app/colaboradores/services/vinculo.py#L259), [colaboradores/services/vinculo.py:638](app/colaboradores/services/vinculo.py#L638), [colaboradores/services/vinculo.py:798](app/colaboradores/services/vinculo.py#L798), [colaboradores/services/vinculo.py:349](app/colaboradores/services/vinculo.py#L349)

### 6. Identificação de 6x1 no Plano de Folgas

O campo é `ec.tipo_escala.tipo_ciclo`, comparado com `TipoEscala.CICLO_6x1`, cujo valor é `'6x1'`. O grid do Plano chama `ec.defeito_folga_nao_definida(...)`; esse método faz a comparação e verifica folga semanal ou `FolgaDia` no mês. A view não faz ela mesma uma comparação direta com `CICLO_6x1` no trecho do grid. [escala/models.py:24](app/escala/models.py#L24), [escala/models.py:117](app/escala/models.py#L117), [escala/models.py:1241](app/escala/models.py#L1241), [escala/views_plano_folgas.py:83](app/escala/views_plano_folgas.py#L83)

### 7. Furos encontrados além do anexo AST

- O script exclui chamadas simples pelo nome quando não há import reconhecido. Por isso, as chamadas locais de `janela`, `assinaturas_do_colab`, `anomalias_da_frota`, `gravar_lavra` e `ler_lavra` dentro do próprio módulo precisam ser seguidas manualmente. [ponto/services/esmeril_espelho.py:192](app/ponto/services/esmeril_espelho.py#L192), [ponto/services/esmeril_espelho.py:303](app/ponto/services/esmeril_espelho.py#L303), [ponto/services/esmeril_espelho.py:356](app/ponto/services/esmeril_espelho.py#L356)
- O alias local `_lp` para `linhas_para_a_pauta` não estava no conjunto de nomes-alvo; ele leva à leitura da lavra na revisão semanal. [escala/services/fase_12x36.py:240](app/escala/services/fase_12x36.py#L240), [ponto/services/esmeril_espelho.py:356](app/ponto/services/esmeril_espelho.py#L356)
- Template Django, shell, serialização integral e `json.load` da cópia da sombra estão fora da varredura Python do anexo. [templates/escala/cadastro_x_realidade.html:21](app/templates/escala/cadastro_x_realidade.html#L21), [bin/sombra.sh:141](bin/sombra.sh#L141), [bin/sombra.sh:155](bin/sombra.sh#L155)
- A API usa import de módulo com alias `_es` e constrói resposta com compreensão e `**`; ela seleciona explicitamente as quatro chaves indicadas acima. [api/views_mensageria.py:1791](app/api/views_mensageria.py#L1791)
- Um reexport por `__init__`, import relativo ou outro alias que alcance estes objetos além dos exemplos acima permanece **[não verificado]**. O mesmo vale para leitores genéricos por chave recebida em tempo de execução. [inteligencia/metricas.py:62](app/inteligencia/metricas.py#L62)

A descrição em `config/crons.py` sobre `horas_folga_trabalhada` é texto de diagnóstico; o código atual de `ler_lavra` registra que o consumidor de dinheiro deixou de ler essa lavra, e `escala_certa` mede assinaturas diretamente. A execução efetiva em produção permanece **[não verificado]**. [config/crons.py:1064](app/config/crons.py#L1064), [ponto/services/esmeril_espelho.py:348](app/ponto/services/esmeril_espelho.py#L348), [escala/services/escala_certa.py:27](app/escala/services/escala_certa.py#L27)

censo fechado: nao -- faltam exaustividade de acessos dinamicos/reexports e de escritores globais dos campos do vinculo; identidade do HEAD e estado de producao nao verificados.

## 18/11 09:01 · sessao d57fe0d8679b · DIA-SEM-TRABALHO-PORTA · censo · origem=Code

HASH LIDO = dd489c627bb0a8d5f8782945c7e240e6621c98ed

O hash acima é o informado no pedido; a cópia não contém metadados Git para confirmá-lo com `git rev-parse`. [não verificado]

O censo encontrou a diferença central: `_protesta` considera `cel.trabalha` e os marcos acesos, mas não consulta `tipo_dia` da ata; o juiz `dia_sem_trabalho` também considera o tipo da ata e a exceção de ausência parcial. [cartorio.py:1458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458), [juizes.py:445](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:445)

### 1. Script e resultado da leitura

O script abaixo faz uma varredura sintática de todos os `app/**/*.py`, inclusive testes, sem regex. Resolve imports com `as`, inclusive imports locais e relativos, e imprime também acessos às chaves em objetos que **podem não ser atas**; essa classificação exige leitura do contexto. Ele registra literais de dicionário como escritas. Chamadas indiretas, chaves construídas dinamicamente e alterações por métodos como `update` com dicionário dinâmico não são provadas por esta análise. **Esta versão consolidada do script não foi executada**; executei uma versão preliminar, cujo resultado parcial está resumido abaixo. [não verificado] [nao verificado]

```python
import ast
import importlib.util
from pathlib import Path

ROOT = Path("app")
KEYS = {"tipo_dia", "cobertura_parcial"}
TARGETS = {
    "chamados.juizes.dia_sem_trabalho",
    "chamados.juizes.ausencia_parcial_cobra_o_resto",
    "ponto.supra_juiz._dia_sem_trabalho_pelo_juiz",
}


def module_name(path):
    parts = path.relative_to(ROOT).with_suffix("").parts
    if parts[-1] == "__init__":
        parts = parts[:-1]
    return ".".join(parts)


def absolute_import(module, level, current, is_package):
    if not level:
        return module or ""
    package = current if is_package else current.rpartition(".")[0]
    try:
        return importlib.util.resolve_name(
            "." * level + (module or ""), package
        )
    except (ImportError, ValueError):
        return "." * level + (module or "")


def bindings(body, current, is_package):
    """Imports e definições diretamente no corpo de um escopo."""
    found = {}
    for node in body:
        if isinstance(node, ast.Import):
            for alias in node.names:
                local = alias.asname or alias.name.split(".")[0]
                found[local] = alias.name if alias.asname else local
        elif isinstance(node, ast.ImportFrom):
            base = absolute_import(
                node.module, node.level, current, is_package
            )
            for alias in node.names:
                if alias.name != "*":
                    found[alias.asname or alias.name] = (
                        f"{base}.{alias.name}" if base else alias.name
                    )
        elif isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef,
                               ast.ClassDef)):
            found[node.name] = f"{current}.{node.name}"
    return found


class Census(ast.NodeVisitor):
    def __init__(self, path, tree):
        self.path = path
        self.current = module_name(path)
        self.is_package = path.name == "__init__.py"
        self.scopes = [
            bindings(tree.body, self.current, self.is_package)
        ]
        self.names = []
        self.rows = []

    def resolve(self, node):
        if isinstance(node, ast.Name):
            for scope in reversed(self.scopes):
                if node.id in scope:
                    return scope[node.id]
            return node.id
        if isinstance(node, ast.Attribute):
            base = self.resolve(node.value)
            return f"{base}.{node.attr}" if base else node.attr
        return ""

    def add(self, node, what, action):
        self.rows.append((
            str(self.path), node.lineno,
            ".".join(self.names) or "<módulo>", what, action
        ))

    def scoped(self, node):
        self.names.append(node.name)
        self.scopes.append(
            bindings(node.body, self.current, self.is_package)
        )
        # Visita também decoradores e valores padrão.
        self.generic_visit(node)
        self.scopes.pop()
        self.names.pop()

    visit_FunctionDef = scoped
    visit_AsyncFunctionDef = scoped
    visit_ClassDef = scoped

    def visit_Call(self, node):
        target = self.resolve(node.func)
        if target in TARGETS:
            self.add(node, target, "chamada")
        if (
            isinstance(node.func, ast.Attribute)
            and node.func.attr == "get"
            and node.args
            and isinstance(node.args[0], ast.Constant)
            and node.args[0].value in KEYS
        ):
            self.add(node, node.args[0].value, "leitura .get")
        self.generic_visit(node)

    def visit_Subscript(self, node):
        if isinstance(node.slice, ast.Constant) and node.slice.value in KEYS:
            action = (
                "escrita" if isinstance(node.ctx, ast.Store)
                else "remoção" if isinstance(node.ctx, ast.Del)
                else "leitura"
            )
            self.add(node, node.slice.value, action)
        self.generic_visit(node)

    def visit_Dict(self, node):
        for key in node.keys:
            if isinstance(key, ast.Constant) and key.value in KEYS:
                self.add(key, key.value, "escrita em literal")
        self.generic_visit(node)


rows = []
errors = []
for path in sorted(ROOT.rglob("*.py")):
    try:
        tree = ast.parse(path.read_bytes(), filename=str(path))
    except (SyntaxError, UnicodeDecodeError) as exc:
        errors.append((path, exc))
        continue
    visitor = Census(path, tree)
    visitor.visit(tree)
    rows.extend(visitor.rows)

print("arquivo:linha\tfunção/classe\titem\toperação")
for path, line, function, item, action in sorted(rows):
    print(f"{path}:{line}\t{function}\t{item}\t{action}")
for path, error in errors:
    print(f"ERRO DE PARSE: {path}: {error}")
print(f"TOTAL: {len(rows)}; ERROS: {len(errors)}")
```

A versão preliminar executada encontrou 240 ocorrências sintáticas, incluindo testes e acessos a grades, mas não detectou a chamada local de `ausencia_parcial_cobra_o_resto` em [juizes.py:458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:458), nem literais de dicionário. Portanto, **não é a tabela completa pedida**. [não verificado]

Principais ocorrências de produção confirmadas na leitura:

| Item | Sítios e função/operação |
|---|---|
| Juiz `dia_sem_trabalho` | [juizes.py:498](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:498) `_celula_furada`; [models.py:1331](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1331) `premissa_regularizada`; [fabrica_da_celula.py:112](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:112) e [201](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:201); [materializacao.py:431](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:431); [detectar_intervalo_ausente.py:51](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/detectar_intervalo_ausente.py:51); [reconciliar_grade.py:282](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:282); [cluster_espurio.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:25); adaptador [supra_juiz.py:84](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:84). |
| Adaptador `_dia_sem_trabalho_pelo_juiz` | Leituras em [cartorio.py:1255](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1255), [supra_juiz.py:509](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:509) e [tarefa_do_dia.py:233](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:233), [505](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:505), [517](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:517). |
| `tipo_dia` na ata | Escrita por cópia em [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738); leitura pelo juiz em [juizes.py:442](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:442) e [460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:460). Outros acessos da varredura são também a dias da grade e precisam de classificação individual. [não verificado] |
| `cobertura_parcial` | Escrita na grade em [utils.py:1230](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1230), copiada para a ata em [cartorio.py:766](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:766), transportada pelo leitor em [leitor_celula.py:433](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/leitor_celula.py:433); leitura pelo predicado em [juizes.py:442](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:442). Demais acessos: [não verificado]. |
| `ausencia_parcial_cobra_o_resto` | Chamadas em [juizes.py:458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:458), [declaracao_texto.py:120](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/declaracao_texto.py:120), [tarefa_do_dia.py:242](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/tarefa_do_dia.py:242), [furos_diarios.py:49](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/services/furos_diarios.py:49), [calculadores.py:50](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/calculadores.py:50) e [149](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/inteligencia/calculadores.py:149), [furos.py:199](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/furos.py:199). |

### 2. Escrita do `tipo_dia` da ata deste julgamento

`_ata_agora` é composta por `ata_do_dia`, `dispensar_por_ausencia` e `_ata_ate_o_teto` em [cartorio.py:1436](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1436). A chave é copiada de `dia.get('tipo_dia')` por `ata_do_dia` em [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738); as etapas de dispensa e teto alteram lâmpadas e contagens, preservando essa chave em [cartorio.py:800](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:800) e [cartorio.py:907](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:907).

A origem do valor é a grade montada em [cartorio.py:1175](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1175):

| Valor | Escrita na grade |
|---|---|
| `folga` | Construtores em [utils.py:760](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:760) e [utils.py:1801](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1801); folga lançada válida em [utils.py:1151](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1151). |
| `trabalho` | Construtores em [utils.py:764](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:764) e [utils.py:1807](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1807). |
| `falta` | Cobertura classificada como falta, respeitada a condição sobre folga, em [utils.py:1210](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1210). |
| `ausencia` | Cobertura sobreposta pela grade em [utils.py:1220](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1220). |
| Outros valores na ata deste caminho | [não verificado]. `ata_do_dia` aceita `None` se receber um `dia` arbitrário sem a chave, mas isso não demonstra que a grade deste julgamento produza tal entrada: [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738). |

A sobreposição de folga ou cobertura na grade pode produzir tipo diferente de `trabalho` enquanto `cel.trabalha` é `True` ou `None`, pois o cartório passa separadamente o atributo da célula e a ata construída da grade: [utils.py:1147](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1147), [utils.py:1210](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1210), [cartorio.py:1464](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1464). Há teste explícito de `True` com ausência parcial em [test_emis_ausencia_parcial_cobra.py:78](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_emis_ausencia_parcial_cobra.py:78). A ocorrência em produção é [não verificado].

### 3. Consumidores de `_protesta` e efeitos

| Consumidor | Uso |
|---|---|
| `_protestam` | Filtra `vivos` em [cartorio.py:1460](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1460); seu tamanho alimenta `chamados_vivos` em [cartorio.py:1464](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1464). |
| Lâmpada `_vd` | Se não há códigos e há vivos, um chamado com fio que protesta produz `cobrado` em [cartorio.py:1479](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1479) e [1502](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1502). |
| `_nao_protestam` / `_adiar` | Separa os que não protestam e pode adiar o lote em [cartorio.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1533); processamento posterior em [cartorio.py:999](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:999). |
| `_porm` | Tenta transicionar os não adiados para `resolvido`; os efetivamente transicionados saem de `vivos` e provocam rejulgamento em [cartorio.py:1539](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1539). |

Em `classificar_dia`, `chamados_vivos` participa dos códigos `COBRANCA_SEM_FURO`, `COBRANCA_EM_DIA_SEM_TRABALHO` e `COBRANCA_EM_DIA_COBERTO`: [supra_juiz.py:480](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:480), [507](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:507), [543](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/supra_juiz.py:543).

`cobrado` integra `VEREDITOS_ACUSAM` em [models.py:1767](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1767). Entre os leitores identificados estão folha/export em [export.py:1273](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/export.py:1273); status em [status_dia.py:68](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/status_dia.py:68); relatório/tela em [dia_da_frota.py:51](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/relatorios/dia_da_frota.py:51); vigias em [vigia_pausa.py:155](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/services/vigia_pausa.py:155), [vigia_ausencia_veredito.py:41](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/vigia_ausencia_veredito.py:41) e [vigia_fio.py:286](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/vigia_fio.py:286). A lista completa de leitores diretos e indiretos é [não verificado].

No fechamento, `transicionar_com_pauta` executa o ato e encaminha recusas à pauta em [fecho_runtime.py:146](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fecho_runtime.py:146). `reconciliar` aplica as guardas e grava o estado quando permitido em [models.py:642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:642); ao resolver, o receiver chama a resolução das perguntas em [signals.py:91](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/signals.py:91). A relação exaustiva das reações a jusante é [não verificado].

### 4. Predicados próprios

- `_protesta`: `cel.trabalha is not False` **e** `marcos_acesos(x, ata_deste_julgamento) is not True`; não consulta `dia_sem_trabalho`: [cartorio.py:1458](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1458).
- No ramo de nunca bateu, `_coberto_inteiro_nb` testa `tipo_dia in ('ausencia', 'falta')` e ausência de `cobertura_parcial`; serve à decisão de `sem_adesao`, não equivale ao juiz geral: [cartorio.py:1237](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1237), [1257](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1257).
- `marcos_acesos` examina o lastro dos marcos cobrados, com resultado indeterminado quando não consegue decidir; é uma decisão por marco: [lastro.py:153](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/lastro.py:153).

A inexistência de outros predicados próprios nos três arquivos é [não verificado].

### 5. Origem da ata entregue ao juiz

O juiz lê `cel.ata or {}` e `cel.trabalha`; para ausência parcial, só `trabalha is False` torna o dia sem trabalho; nos demais casos também basta `tipo_dia` preenchido e diferente de `trabalho`: [juizes.py:454](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:454).

| Caminho | Origem identificada |
|---|---|
| `_celula_furada` e `premissa_regularizada` | Usam `celula_do_dia_agora`; a porta pode projetar a ata do julgamento em curso sobre a célula e, fora dele, conservar a lavrada: [juizes.py:407](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:407), [491](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/juizes.py:491), [models.py:1327](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:1327). |
| Materialização, comando de intervalo e reconciliação de grade | Consultam células carregadas do banco em [materializacao.py:421](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/materializacao.py:421), [detectar_intervalo_ausente.py:46](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/detectar_intervalo_ausente.py:46), [reconciliar_grade.py:189](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/management/commands/reconciliar_grade.py:189). |
| Fábrica e cluster | Recebem uma célula como argumento em [fabrica_da_celula.py:92](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:92), [178](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/services/fabrica_da_celula.py:178), [cluster_espurio.py:25](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cluster_espurio.py:25); a origem em **cada** chamador desses serviços é [não ve

[truncado em 20000 de 21190 caracteres]

## 18/11 09:00 · sessao fabad78fa82a · ESPURIA-TELA · censo · origem=Code

```python
#!/usr/bin/env python3
# Salve este código como censo.py fora do repositório.
# Na raiz da cópia do repositório, execute: python3 /caminho/para/censo.py
# Para salvar a saída: python3 /caminho/para/censo.py > censo.tsv

import ast
import collections
import pathlib
import re
import sys

ROOT = pathlib.Path.cwd()
APP = ROOT / "app"
TEMPLATES = APP / "templates"
STATIC_JS = APP / "static" / "js"

NAMES = {
    "dia_decidido",
    "veredito_do_dia",
    "do_dia",
    "decididos_do_periodo",
    "palavra_dia",
    "cor_dia",
    "veredito_dia",
    "precedencia",
    "fatos_do_periodo",
}
NAME_RE = re.compile(
    r"(?<![A-Za-z0-9_])(" + "|".join(sorted(map(re.escape, NAMES))) +
    r")(?![A-Za-z0-9_])"
)

if not APP.is_dir():
    sys.exit("Erro: execute na raiz do repositório, com o código em app/.")


def rel(path):
    return path.relative_to(ROOT).as_posix()


def read(path):
    try:
        return path.read_text(encoding="utf-8")
    except (OSError, UnicodeError) as exc:
        print("ERRO_LEITURA\t{}\t{}".format(rel(path), exc), file=sys.stderr)
        return None


def is_test(path):
    relative = path.relative_to(APP)
    return "tests" in relative.parts[:-1] or path.name.startswith("test_")


def dotted(node):
    if isinstance(node, ast.Name):
        return node.id
    if isinstance(node, ast.Attribute):
        prefix = dotted(node.value)
        return prefix + "." + node.attr if prefix else None
    return None


def module_name(path):
    parts = list(path.relative_to(APP).with_suffix("").parts)
    if parts and parts[-1] == "__init__":
        parts.pop()
    return ".".join(parts)


def resolve_import(path, node):
    module = node.module or ""
    if not node.level:
        return module
    package = module_name(path).split(".")
    if path.name != "__init__.py":
        package = package[:-1]
    up = node.level - 1
    if up:
        package = package[:-up] if up <= len(package) else []
    return ".".join(package + ([module] if module else []))


class PythonCensus(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.rows = set()
        self.parents = []
        self.scopes = [dict()]
        self.functions = []

    def emit(self, node, name, kind):
        if name in NAMES:
            self.rows.add((
                rel(self.path),
                getattr(node, "lineno", 0),
                ".".join(self.functions) or "<modulo>",
                name,
                kind,
            ))

    def lookup(self, name):
        for scope in reversed(self.scopes):
            if name in scope:
                return scope[name]
        return None

    def canonical(self, node):
        if isinstance(node, ast.Name):
            return self.lookup(node.id) or (node.id if node.id in NAMES else None)
        if isinstance(node, ast.Attribute):
            if node.attr in NAMES:
                return node.attr
            return None
        return None

    def visit(self, node):
        if node is None:
            return None
        self.parents.append(node)
        try:
            return super().visit(node)
        finally:
            self.parents.pop()

    def visit_Import(self, node):
        for alias in node.names:
            parts = alias.name.split(".")
            for part in parts:
                self.emit(node, part, "import")
            bound = alias.asname or parts[0]
            target = parts[-1] if alias.asname else parts[0]
            self.scopes[-1][bound] = target if target in NAMES else None

    def visit_ImportFrom(self, node):
        module = resolve_import(self.path, node)
        for part in module.split("."):
            self.emit(node, part, "import")
        for alias in node.names:
            self.emit(node, alias.name, "import")
            bound = alias.asname or alias.name
            self.scopes[-1][bound] = alias.name if alias.name in NAMES else None

    def visit_FunctionDef(self, node):
        self._function(node)

    def visit_AsyncFunctionDef(self, node):
        self._function(node)

    def _function(self, node):
        self.emit(node, node.name, "definicao")
        self.scopes[-1][node.name] = node.name if node.name in NAMES else None
        for decorator in node.decorator_list:
            self.visit(decorator)
        for default in node.args.defaults + node.args.kw_defaults:
            self.visit(default)
        if node.returns:
            self.visit(node.returns)
        self.functions.append(node.name)
        self.scopes.append({})
        for arg in (
            node.args.posonlyargs + node.args.args + node.args.kwonlyargs +
            ([node.args.vararg] if node.args.vararg else []) +
            ([node.args.kwarg] if node.args.kwarg else [])
        ):
            self.scopes[-1][arg.arg] = None
            if arg.annotation:
                self.visit(arg.annotation)
        for statement in node.body:
            self.visit(statement)
        self.scopes.pop()
        self.functions.pop()

    def visit_ClassDef(self, node):
        self.emit(node, node.name, "definicao")
        for decorator in node.decorator_list:
            self.visit(decorator)
        for base in node.bases:
            self.visit(base)
        for keyword in node.keywords:
            self.visit(keyword.value)
        self.scopes.append({})
        for statement in node.body:
            self.visit(statement)
        self.scopes.pop()

    def visit_Name(self, node):
        name = self.canonical(node)
        if name:
            parent = self.parents[-2] if len(self.parents) > 1 else None
            kind = (
                "chamada" if isinstance(parent, ast.Call) and parent.func is node
                else "definicao" if isinstance(node.ctx, (ast.Store, ast.Del))
                else "atributo"
            )
            self.emit(node, name, kind)

    def visit_Attribute(self, node):
        name = self.canonical(node)
        if name:
            parent = self.parents[-2] if len(self.parents) > 1 else None
            kind = (
                "chamada" if isinstance(parent, ast.Call) and parent.func is node
                else "definicao" if isinstance(node.ctx, (ast.Store, ast.Del))
                else "atributo"
            )
            self.emit(node, name, kind)
        self.visit(node.value)

    def visit_Call(self, node):
        if (
            isinstance(node.func, ast.Name)
            and node.func.id == "getattr"
            and len(node.args) >= 2
            and isinstance(node.args[1], ast.Constant)
            and isinstance(node.args[1].value, str)
        ):
            value = node.args[1].value
            if value in NAMES:
                parent = self.parents[-2] if len(self.parents) > 1 else None
                kind = (
                    "chamada" if isinstance(parent, ast.Call) and parent.func is node
                    else "string"
                )
                self.emit(node.args[1], value, kind)
        self.generic_visit(node)

    def visit_Dict(self, node):
        for key, value in zip(node.keys, node.values):
            if isinstance(key, ast.Constant) and isinstance(key.value, str):
                if key.value in NAMES:
                    self.emit(key, key.value, "string")
            self.visit(key)
            self.visit(value)

    def visit_Subscript(self, node):
        if isinstance(node.slice, ast.Constant) and isinstance(node.slice.value, str):
            if node.slice.value in NAMES:
                self.emit(node.slice, node.slice.value, "string")
        self.generic_visit(node)


python_rows = {"producao": set(), "testes": set()} [nao verificado]
python_sources = {}
python_trees = {}
for path in sorted(APP.rglob("*.py")):
    source = read(path)
    if source is None:
        continue
    python_sources[path] = source
    try:
        tree = ast.parse(source, filename=rel(path))
    except SyntaxError as exc:
        print(
            "ERRO_AST\t{}\t{}\t{}".format(rel(path), exc.lineno, exc.msg),
            file=sys.stderr,
        )
        continue
    python_trees[path] = tree
    census = PythonCensus(path)
    census.visit(tree)
    python_rows["testes" if is_test(path) else "producao"].update(census.rows) [nao verificado]

template_sources = {}
for path in sorted(TEMPLATES.rglob("*.html")):
    source = read(path)
    if source is not None:
        template_sources[path.relative_to(TEMPLATES).as_posix()] = source

text_rows = []
for name, source in template_sources.items():
    for number, line in enumerate(source.splitlines(), 1):
        for match in NAME_RE.finditer(line):
            text_rows.append((rel(TEMPLATES / name), number, match.group(1)))

for path in sorted(STATIC_JS.rglob("*.js")):
    source = read(path)
    if source is None:
        continue
    for number, line in enumerate(source.splitlines(), 1):
        for match in NAME_RE.finditer(line):
            text_rows.append((rel(path), number, match.group(1)))

# As arestas apontam do template que fornece a casca para quem o estende,
# e do template que inclui para o template incluído.
edges = collections.defaultdict(set)
tag_re = re.compile(r"{%\s*(extends|include)\s+(.+?)\s*%}", re.DOTALL)
literal_re = re.compile(r"""^\s*(['"])(.*?)\1""", re.DOTALL)
default_re = re.compile(r"""\bbase_tpl\s*\|\s*default\s*:\s*(['"])(.*?)\1""")

for name, source in template_sources.items():
    for match in tag_re.finditer(source):
        kind, expression = match.groups()
        literal = literal_re.match(expression)
        if literal:
            target = literal.group(2)
            if kind == "extends":
                edges[target].add(name)
            else:
                edges[name].add(target)
        elif kind == "extends" and default_re.search(expression):
            edges["base.html"].add(name)
            edges["base_app.html"].add(name)

shells = collections.defaultdict(set)
roots = {"base.html": "base", "base_app.html": "app"}
for root, shell in roots.items():
    if root not in template_sources:
        print("ROOT_AUSENTE\t{}".format(root), file=sys.stderr)
    else:
        shells[root].add(shell)


def propagate_edges():
    changed = False
    for origin, targets in edges.items():
        for target in targets:
            before = len(shells[target])
            shells[target].update(shells[origin])
            changed |= len(shells[target]) != before
    return changed


# Agrupa referências literais por função ou classe de view.
# Para render(), o template é o segundo argumento; render_to_string()
# usa o primeiro. Também aceita os respectivos argumentos nomeados.
view_templates = collections.defaultdict(set)


def literal_html(node):
    if isinstance(node, ast.Constant) and isinstance(node.value, str):
        return node.value if node.value.endswith(".html") else None
    return None


class ViewTemplates(ast.NodeVisitor):
    def __init__(self, path):
        self.path = path
        self.scope = []

    def key(self):
        return (rel(self.path), ".".join(self.scope) or "<modulo>")

    def visit_FunctionDef(self, node):
        self.scope.append(node.name)
        self.generic_visit(node)
        self.scope.pop()

    visit_AsyncFunctionDef = visit_FunctionDef

    def visit_ClassDef(self, node):
        self.scope.append(node.name)
        self.generic_visit(node)
        self.scope.pop()

    def visit_Call(self, node):
        func = dotted(node.func) or ""
        leaf = func.rsplit(".", 1)[-1]
        index = 1 if leaf == "render" else 0
        if leaf in {"render", "render_to_string"}:
            candidates = []
            if len(node.args) > index:
                candidates.append(node.args[index])
            candidates.extend(
                kw.value for kw in node.keywords
                if kw.arg in {"template_name", "template"}
            )
            for candidate in candidates:
                name = literal_html(candidate)
                if name:
                    self.view_templates_add(name)
        self.generic_visit(node)

    def visit_Assign(self, node):
        if any(
            isinstance(target, ast.Name) and target.id == "template_name"
            or isinstance(target, ast.Attribute) and target.attr == "template_name"
            for target in node.targets
        ):
            name = literal_html(node.value)
            if name:
                self.view_templates_add(name)
        self.generic_visit(node)

    def visit_AnnAssign(self, node):
        target = node.target
        if (
            isinstance(target, ast.Name) and target.id == "template_name"
            or isinstance(target, ast.Attribute) and target.attr == "template_name"
        ):
            name = literal_html(node.value)
            if name:
                self.view_templates_add(name)
        self.generic_visit(node)

    def view_templates_add(self, name):
        view_templates[self.key()].add(name)
        # Métodos e atributos da mesma classe compartilham a casca da view.
        if len(self.scope) >= 2:
            view_templates[(rel(self.path), self.scope[0])].add(name)


for path, tree in python_trees.items():
    if not is_test(path):
        ViewTemplates(path).visit(tree)

# Compartilhar cascas somente dentro da mesma view; depois propagar includes
# e herança novamente até atingir o ponto fixo.
while True:
    changed = propagate_edges()
    for names in view_templates.values():
        combined = set()
        for name in names:
            combined.update(shells[name])
        for name in names:
            before = len(shells[name])
            shells[name].update(combined)
            changed |= len(shells[name]) != before
    if not changed:
        break


def shell_name(name):
    value = shells[name]
    if value == {"base", "app"}:
        return "ambas"
    if value == {"base"}:
        return "base"
    if value == {"app"}:
        return "app"
    return "nenhuma"


print("PYTHON_PRODUCAO")
print("arquivo\tlinha\tfuncao_envolvente\tnome\ttipo")
for row in sorted(python_rows["producao"]):
    print(*row, sep="\t")

print("PYTHON_TESTES")
print("arquivo\tlinha\tfuncao_envolvente\tnome\ttipo")
for row in sorted(python_rows["testes"]): [nao verificado]
    print(*row, sep="\t")

print("HTML_JS")
print("arquivo\tlinha\tnome")
for row in sorted(text_rows):
    print(*row, sep="\t")

matched_templates = sorted({
    path[len("app/templates/"):]
    for path, _, _ in text_rows
    if path.startswith("app/templates/")
})
print("CASCAS_TEMPLATES")
print("arquivo\tcasca")
for name in matched_templates:
    print("app/templates/{}\t{}".format(name, shell_name(name)))

print("CONTAGEM_CASCA")
print("casca\ttemplates")
shell_counts = collections.Counter(shell_name(name) for name in matched_templates)
for shell in ("base", "app", "ambas", "nenhuma"):
    print("{}\t{}".format(shell, shell_counts[shell]))

print("CONTAGEM_TIPO")
print("grupo\ttipo\tocorrencias")
for group in ("producao", "testes"): [nao verificado]
    counts = collections.Counter(row[4] for row in python_rows[group])
    for kind in ("definicao", "import", "chamada", "atributo", "string"):
        print("{}\t{}\t{}".format(group, kind, counts[kind]))

print("CONTAGEM_HTML_JS")
print("tipo\tocorrencias")
print("html\t{}".format(sum(p.startswith("app/templates/") for p, _, _ in text_rows)))
print("js\t{}".format(sum(p.startswith("app/static/js/") for p, _, _ in text_rows)))
```

## 18/11 08:59 · sessao ad8a49d19ed1 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Limite da conferência:** a cópia fornecida não contém `.git`; `git rev-parse HEAD` falhou. Trato os arquivos lidos como a cópia identificada no pedido, mas a identidade independente com `dd489c627` é **[nao verificado]**. As referências `brief` e `matriz` abaixo são as linhas dos textos raspados, contadas conforme a remedição fornecida; sua correspondência com arquivos originais é **[nao verificado]**.

Julgo as **45 premissas da remedição**. `MOVEU` identifica o sítio que passou a usar outro juiz ou mudou de linha; quando a premissa era a existência do antigo redutor por atos, essa parte **caiu** mesmo no sítio movido. [nao verificado]

| # | Premissa e origem no brief/matriz | Julgamento na cópia |
|---|---|---|
| 1 | Contrato F1 `LeituraDosAtos` — brief:76–78; matriz:14,16–18 | **CAIU:** o juiz disponível é booleano sobre o par projetado (`app/chamados/juizes.py:1544`, `:1552`). |
| 2 | Redutor F1 limpa estados por ato — brief:84–88; matriz:19–20 | **CAIU** como interface demonstrada; a leitura temporal examinada usa projeção (`app/chamados/juizes.py:1389`, `:1400`). |
| 3 | Conservação por `_leitura_conservada` — brief:78,83; matriz:21–22 | **CAIU** nesse formato; existe correspondência entre projeção e linhas de decisão (`app/chamados/models.py:4121`). |
| 4 | Faces SQL R consultam atos — brief:93–94; matriz:23–25 | **CAIU:** as faces consultam os campos projetados (`app/chamados/juizes.py:1565`, `:1575`). |
| 5 | Espera ficou metade projeção, metade ato — brief:20; matriz:26–28,35 | **CAIU:** os dois termos atuais examinam a projeção (`app/chamados/services/fio.py:187`; `app/chamados/juizes.py:1531`). |
| 6 | F2 já fornece `estado` por ato — matriz:29 | **CAIU:** o estado usa campos projetados e o juiz de respondida (`app/chamados/models.py:3159`, `:3675`). |
| 7 | POST usa a leitura antiga — brief:21; matriz:36 | **MOVEU:** guarda usa `pergunta_respondida`; o redutor antigo não foi demonstrado (`app/chamados/services/fio.py:707`). |
| 8 | Canônica usa texto do redutor — brief:22; matriz:37 | **MOVEU:** seleção usa o juiz do par projetado (`app/chamados/services/fio.py:1281`). |
| 9 | Mérito perdedor usa redutor — brief:22; matriz:38 | **MOVEU:** usa o juiz do par (`app/chamados/services/fio.py:1293`). |
| 10 | Lavra sem turno lê texto do redutor — brief:23; matriz:39 | **MOVEU:** guarda de resposta usa o juiz; C continua campo projetado (`app/chamados/reconciliador.py:365`, `:368`). |
| 11 | Universo absolvível dispõe de SQL V por ato — brief:24; matriz:40 | **CAIU:** filtro usa V e C projetados (`app/chamados/reconciliador.py:673`). |
| 12 | Órfãs abertas consomem o redutor — brief:25; matriz:41 | **CAIU:** filtro é C/V, não R/C como descrevia a matriz (`app/chamados/reconciliador.py:1093`). |
| 13 | Veredito de órfã lê texto do ato — brief:26; matriz:42 | **MOVEU:** guarda usa o juiz projetivo (`app/chamados/reconciliador.py:1126`). |
| 14 | Fotos de órfãs usam `LeituraDosAtos` — brief:27; matriz:43 | **CAIU:** a trilha fotografa C e via da pergunta (`app/chamados/reconciliador.py:1189`, `:1199`). |
| 15 | C em SQL permanece — brief:28,94–95; matriz:44,90 | **CONFERE:** órfãs reatáveis filtram C (`app/chamados/reconciliador.py:1549`). |
| 16 | Explicador herda estado e resposta por ato — brief:29; matriz:45 | **CAIU:** usa `estado` e texto projetado (`app/chamados/services/explicador.py:95`, `:102`; `app/chamados/models.py:3159`). |
| 17 | Guarda de validação dispõe do instante do redutor — brief:30,87; matriz:46 | **CAIU:** testa V projetada (`app/chamados/services/declaracao_texto.py:228`). |
| 18 | Declaração compara última resposta do redutor — brief:31; matriz:47 | **MOVEU:** a guarda consulta o juiz e compara texto projetado (`app/chamados/services/declaracao_texto.py:418`). |
| 19 | Aplicador dispõe da resposta do redutor — brief:32; matriz:48 | **MOVEU:** usa o juiz antes de chamar a porta (`app/chamados/services/declaracao_texto.py:688`). |
| 20 | Causa `c` lê R cru — brief:33; matriz:49 | **MOVEU:** agora usa `pergunta_respondida` (`app/chamados/services/vigia_fio.py:636`). |
| 21 | Vigia de resposta vazia mede integridade da projeção — brief:52–54; matriz:50,89 | **CONFERE:** conta o par projetado; escolha técnica final da F5 continua **[nao verificado]** (`app/chamados/services/vigia_fio.py:739`, `:747`). |
| 22 | Resposta vazia permanece fora das pendentes — brief:85,105–108; matriz:51,69–71,82 | **CAIU na implementação para uma pergunta viva de disputa aberta:** a negação do juiz inclui o meio par nas pendentes (`app/chamados/services/pergunta_no_app.py:79`; `app/chamados/juizes.py:1544`). O corte diz fora do app; ver §4 (`app/docs/CORTES.md:509`). |
| 23 | Beco dispõe do redutor por ato — brief:37; matriz:52 | **MOVEU:** usa a negação do juiz projetivo (`app/chamados/services/becos.py:29`). |
| 24 | F1 fornece accessor ao histórico por atos — brief:38; matriz:53,85 | **CAIU:** histórico ainda emite por campos atuais (`app/chamados/services/historico.py:98`, `:109`). |
| 25 | Vigia carimbo dispõe da face V da F2 — brief:39,87; matriz:54 | **CAIU como suporte demonstrado:** o universo continua em V projetada (`app/chamados/services/vigia_carimbo.py:98`). |
| 26 | `_pela_decisao` já lê decisões — brief:46,78; matriz:55 | **CONFERE** (`app/chamados/services/tarefa_do_dia.py:142`, `:158`). |
| 27 | Ordenação DP `(ato_em, pk)` — brief:76; matriz:73 | **CONFERE** (`app/chamados/models.py:4082`). |
| 28 | C não tem ato de materialização e permanece conservado — brief:89,94–95; matriz:76 | **CONFERE** quanto à enumeração e à projeção atual (`app/chamados/models.py:4037`; `app/chamados/juizes.py:1390`). |
| 29 | Arquivos F5 não persistem diretamente R/T/V/C — brief:64–72 | **CONFERE no recorte de escrita direta examinado**; declarador e reconciliador chamam portas existentes. Exaustividade de nomes dinâmicos: **[nao verificado]** (`app/chamados/services/declaracao_texto.py:688`; `app/chamados/reconciliador.py:404`). |
| 30 | Inventário antigo esgota escritores e atos cobrem toda escrita — brief:64–72 | **CAIU:** `carimbar` pode preencher R/V e salvar; conservação também produz linhas (`app/chamados/models.py:3249`, `:3271`, `:4121`). |
| 31 | Furo sem canal e competência trancada não têm migração R/V/C própria identificada — brief:45–46; matriz:57–58 | **CONFERE nos corpos examinados** (`app/chamados/services/furo_sem_canal.py:94`; `app/chamados/services/competencia_trancada.py:276`). |
| 32 | Tarefa, cobrança e cascas herdam a partição — brief:41–46; matriz:55–56,94–95 | **MOVEU:** carga da tarefa está em `:535`; consumidores seguem delegando (`app/chamados/services/tarefa_do_dia.py:535`; `app/chamados/services/cobranca.py:48`; `app/core/views.py:253`; `app/api/views.py:998`, `:2538`). |
| 33 | F/VIA/X são classes distintas de R/V/C — brief:47–50; matriz:60 | **CONFERE como distinção de campos**; a validade normativa de toda a allowlist atual é **[nao verificado]** (`app/chamados/services/explicador.py:108`, `:113`, `:118`). |
| 34 | Guarda se chama `recusa_respondida_sem_texto` e é unilateral — brief:50; matriz:60 | **CAIU:** é `recusa_par_ilegal`, simétrica (`app/chamados/services/fio.py:1699`). |
| 35 | R3 ainda não pousou — brief:126,154–155; matriz:6–9 | **CAIU segundo o registro local**; ancestralidade Git independente **[nao verificado]** (`app/docs/RELATO.md:577`). |
| 36 | F1 antiga é a entrega ainda aguardada — brief:127,154–155; matriz:6–7 | **CAIU:** B1 registrada e juiz real diferente (`app/docs/CORTES.md:513`; `app/chamados/juizes.py:1544`). |
| 37 | F2 antiga já fornece suporte V/estado pressuposto — brief:128; matriz:108 | **CAIU:** há expand de campos, mas V e estado continuam com as leituras citadas acima (`app/chamados/models.py:4065`; `app/chamados/services/vigia_carimbo.py:98`). |
| 38 | BUG-PAR ainda está fora — brief:129; matriz:7 | **CAIU segundo o registro local**; ancestralidade independente **[nao verificado]** (`app/docs/RELATO.md:32`). |
| 39 | BUG-PAR cruza explicador/fio — brief:129–132 | **CONFERE no código atual:** testemunha e explicação existem (`app/chamados/services/fio.py:376`; `app/chamados/services/explicador.py:124`). |
| 40 | Censo e offsets antigos bastam — brief:12–15,58–62; matriz:8–10 | **CAIU:** partição e consumidor de badge têm sítios e comportamento que exigem recenso (`app/chamados/services/pergunta_no_app.py:79`, `:101`). |
| 41 | Pergunta específica original da resposta vazia foi cortada — brief:136–138; matriz:98–102,110–111 | **CONFERE como resposta escrita:** corte manda fora do app e dono admin; implementação atual merece reconciliação, §4 (`app/docs/CORTES.md:509`; `app/chamados/services/pergunta_no_app.py:82`). |
| 42 | Cobertura da mudança de badge pode ser presumida — brief:150–152 | **CAIU como dispensa de conferência:** há contrato de fases públicas e corte específico (`app/docs/BACKLOG.md:2091`; `app/docs/CORTES.md:509`). Cobertura literal do aval antigo **[nao verificado]**. |
| 43 | Ordem histórica ainda é a fila completa — brief:132,154–155; matriz:92–93 | **CAIU:** registro diz B2a no ar e B2b aberta; situação Git de RELAVRA-11 **[nao verificado]** (`app/docs/RELATO.md:5`). |
| 44 | Sombra aproximada antiga é medida atual pela função real — brief:139–146; matriz:77–78,91 | **CAIU:** o juiz disponível é outro e valores atuais são **[nao verificado]** (`app/chamados/juizes.py:1544`). |
| 45 | Há rito escrito de suíte e deploy — brief:156–157 | **CONFERE como regra escrita**, sem afirmar execução nesta leitura (`CLAUDE.md:90`, `:95`). | [nao verificado]

CONFERE 11, MOVEU 10, CAIU 24

## 2 O que mudou

**O diff real pedido permanece [nao verificado].** A cópia não é repositório Git, então não foi possível executar `git show` ou `git diff` entre `fbc5c76ce` e `dd489c627`, nem confirmar os commits do anexo independentemente. O `diffstat_F5.txt` fornecido é evidência do stat declarado, não substitui a leitura do diff real.

O confronto entre a remedição raspada e o código da cópia mostra estas mudanças de sítio e efeito; a atribuição de cada mudança a um commit específico é **[nao verificado]**: [nao verificado]

- **Leitores de resposta:** POST, canônica, mérito, lavra sem turno, veredito de órfã, declaração, beco, partição e causa `c` usam agora o juiz `pergunta_respondida`. Ele exige instante e texto na **projeção**; isso não implementa o antigo fold de atos (`app/chamados/juizes.py:1544`; `app/chamados/services/fio.py:707`, `:1281`, `:1293`; `app/chamados/reconciliador.py:365`, `:1126`; `app/chamados/services/declaracao_texto.py:418`, `:688`; `app/chamados/services/becos.py:29`; `app/chamados/services/pergunta_no_app.py:79`; `app/chamados/services/vigia_fio.py:636`).
- **V e C:** os universos de órfãs e absolvição ainda filtram V/C projetados; o histórico ainda usa os carimbos atuais. A migração geral a atos não está demonstrada (`app/chamados/reconciliador.py:673`, `:1093`, `:1549`; `app/chamados/services/historico.py:98`; `app/chamados/services/vigia_carimbo.py:98`).
- **Expand F2:** `da_ressuscitacao` e `resposta_mantem_validacao` foram acrescentados ao modelo. A resposta humana declara `False`; respostas automática e pela trilha declaram `True`; a conservação também fornece `True` (`app/chamados/models.py:4065`, `:4071`, `:4077`, `:4143`; `app/chamados/services/disputa_emissao.py:1527`, `:1681`, `:1703`). A carga da tarefa lê `es_da_ressuscitacao` e exclui o ato marcado antes de escolher o último; a própria documentação do modelo diz que a marca `True` ainda não é gravada no P1 e que não há leitor de `resposta_mantem_validacao` nesse estágio (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/models.py:4067`, `:4103`).
- **Cerca B2a:** o registro local informa pouso parcial e deixa 39 sítios para B2b; portanto, não se deve tratar a B2 inteira como concluída (`app/docs/RELATO.md:5`). O conteúdo exato do diff de cerca e seus demais escritores/leitores novos é **[nao verificado]**.
- **Duas cascas e badge:** a consulta é consumida por `core/views`, pelos dois sítios de `api/views`, por cobrança e aviso; `projecao_do_app` também a transforma em contagem e badge (`app/core/views.py:253`; `app/api/views.py:998`, `:2538`, `:315`; `app/chamados/services/cobranca.py:48`; `app/chamados/services/aviso_pendencia.py:9`; `app/chamados/services/pergunta_no_app.py:101`, `:109`). A API ainda usa o juiz para filtrar respondidas (`app/api/views.py:2578`, `:2621`).

A remedição antiga descreve outros escritores e leitores, mas uma lista **exaustiva de novos sítios no intervalo** depende do diff real indisponível: **[nao verificado]**. Números atuais de produção e execução de testes também são **[nao verificado]**. [nao verificado]

## 3 Nao verificado de ALCANCE

1. **Silêncio/absorção — ponto localizado:** o `[nao verificado]` está na matriz de transição do **brief:90**, repetido no contraexemplo do **brief:118**: “silêncio = histórico; absorção rejeitada = pendente” na partição. O fechamento registra `SILENCIO`; a materialização registra `ABSORCAO`; `_pela_decisao` lê último ato e status da ausência (`app/chamados/models.py:2011`; `app/chamados/services/materializacao.py:1048`; `app/chamados/services/tarefa_do_dia.py:158`, `:176`). A consulta primeiro restringe a perguntas vivas e a partição lê juiz de resposta e fechamento, sem ler esses atos (`app/chamados/services/pergunta_no_app.py:45`, `:61`, `:76`). **O alcance de escrita e leitura examinado fecha; a transposição normativa não fecha** e está formulada como pergunta na §4.

2. **Cascas e consumidor omitido:** fechados os sítios identificados de consulta, contagem e badge nas duas cascas, cobrança, aviso e carga da tarefa (`app/core/views.py:253`; `app/api/views.py:315`, `:998`, `:2538`; `app/chamados/services/pergunta_no_app.py:101`; `app/chamados/services/tarefa_do_dia.py:535`; `app/chamados/services/cobranca.py:48`; `app/chamados/services/aviso_pendencia.py:9`). Exaustividade de consumidores construídos dinamicamente: **[nao verificado]**; falta censo completo no ambiente com os artefatos e referências necessários.

3. **Arquivos cruzados e novos escritores/leitores no intervalo:** **[nao verificado]**. O stat fornecido identifica arquivos alterados, e os sítios atuais citados na §2 identificam parte do alcance; falta o diff real e sua conferência, inclusive para F3/F6/F8/F9.

4. **Vigia de projeção:** quem lê o par e sua finalidade estão identificados (`app/chamados/services/vigia_fio.py:739`, `:747`); se a F5 o preservará até E4d ou o substituirá continua **[nao verificado]**. Falta a decisão técnica da implementação.

**alcance nao verificado restante: 3** — itens 2, 3 e 4; o item 1 passa à pergunta de lei, sem contar sua decisão como alcance de código fechado.

## 4 Perguntas de lei

A busca em `LEIS.md`, `CORTES.md` e `DOSSIES.md` encontrou respostas escritas para os seguintes pontos:

- **DECIDIDO PELA LEI L-130:** respondida exige instante e texto não vazio, com juiz único para as finalidades listadas (`app/docs/LEIS.md:151`).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 5:** resposta vazia fica fora do app na F5; a ação é do admin. O mesmo corte registra, sem decidir, se ela deveria voltar ao colaborador (`app/docs/CORTES.md:509`).
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 3:** o predicado de absorção por ato pertence à E4d (`app/docs/CORTES.md:509`).
- **DECIDIDO PELA LEI LU-16:** silêncio não fecha; só fato na célula (`app/docs/CORTES.md:495`). O corte posterior fornecido no brief resolve o dono admin do silêncio na tarefa; o registro local também o identifica (`app/docs/RELATO.md:202`). Essas respostas não especificam por si a classificação condicional na partição do app (`app/chamados/services/pergunta_no_app.py:76`).

Perguntas ainda abertas:

1. **Silêncio/absorção:** se uma pergunta cujo último ato é `SILENCIO` ou `ABSORCAO` chega viva à partição, a F5 deve aplicar o juiz atual de resposta e fechamento, ou os resultados propostos no brief — silêncio no histórico e absorção rejeitada pendente? O código da partição não lê esses atos, e o corte reserva o predicado de absorção por ato à E4d (`app/chamados/services/pergunta_no_app.py:76`; `app/docs/CORTES.md:509`).

2. **Resposta vazia viva e disputa aberta:** como conciliar a exclusão expressa do app no corte F5 com a L-130 e a partição atual, cuja negação do juiz inclui o par com instante e texto vazio nas pendentes? O corte decide o caso original, mas essa combinação posterior de regras e implementação não está reconciliada nas fontes examinadas (`app/docs/CORTES.md:509`; `app/docs/LEIS.md:151`; `app/chamados/services/pergunta_no_app.py:79`; `app/chamados/juizes.py:1544`).

3. **Devolução ao colaborador:** resposta vazia deve voltar ao colaborador? O corte registra expressamente a pergunta como não decidida junto de `RESSUSCITAR-DEVOLVE-AO-COLABORADOR` (`app/docs/CORTES.md:509`; `app/docs/LEIS.md:151`).

perguntas de lei: 3

## 18/11 08:59 · sessao 99111fae192f · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**Limite da identificação:** a cópia não contém metadados Git; portanto, a identidade do HEAD `dd489c627`, a ancestralidade e o diff real entre hashes estão **[nao verificado]**. Os caminhos abaixo são os arquivos presentes na cópia. Os números `brief.md` e `matriz.md` referem-se ao texto fornecido, contado a partir do título; sua correspondência com arquivos externos está **[nao verificado]**.

A contagem ao fim desta seção cobre os **21 degraus D1–D21 da matriz**, cada um contado uma vez. **MOVEU** significa que o sítio ou comportamento observado mudou; **CAIU** significa que a premissa descrita para o sítio deixou de valer. O contrato antigo `atos_pergunta.leitura` é julgado separadamente, pois é uma dependência comum, não um degrau adicional.

| Degrau / premissa e sítio antigo | Julgamento na cópia |
|---|---|
| D1, validar testa somente instante de resposta — `matriz.md:16–21`, `brief.md:57` | **CAIU:** consulta `pergunta_respondida`, que exige instante e texto: `app/chamados/services/validacao.py:681`, `app/chamados/juizes.py:1544`. |
| D2, guarda de validação corrente lê V — `matriz.md:24`, `brief.md:58` | **MOVEU:** continua lendo `validada_em`: `app/chamados/services/validacao.py:685`. |
| D3, recusa de pergunta validada lê V — `matriz.md:29`, `brief.md:59` | **MOVEU:** `app/chamados/services/validacao.py:898`. |
| D4, recusa testa somente instante de resposta — `matriz.md:33`, `brief.md:59` | **CAIU:** usa o juiz de resposta: `app/chamados/services/validacao.py:901`. |
| D5, recusa fora do fato retorna antes da escrita — `matriz.md:37`, `brief.md:16–18` | **MOVEU:** `app/chamados/services/validacao.py:906`, escrita posterior em `app/chamados/services/validacao.py:916`. |
| D6, materialização conserva leitura de C — `matriz.md:39–41`, `brief.md:61` | **MOVEU:** `app/chamados/services/validacao.py:769`. |
| D7, lote ainda lê V nos dois pontos — `matriz.md:44–46` | **MOVEU:** `app/chamados/services/validacao.py:1064`, `app/chamados/services/validacao.py:1085`. |
| D8, classe lê texto projetado — `matriz.md:49` | **MOVEU:** `app/chamados/services/validacao.py:1183`. |
| D9, aceite de folga compara texto projetado — `matriz.md:53–54` | **MOVEU:** `app/chamados/services/validacao.py:465`. |
| D10, idade da fila lê instante projetado — `matriz.md:57` | **MOVEU:** `app/chamados/services/validacao.py:1256`. |
| D11, placar conta V corrente com staff — `matriz.md:71`, `brief.md:62` | **CONFERE:** `app/chamados/services/verbos.py:263`. |
| D12, aceite usa instante de resposta após cobrança — `matriz.md:77–81`, `brief.md:63` | **MOVEU:** conserva a comparação temporal, mas agora exige também o juiz SQL de resposta: `app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:344`. |
| D13, marcador agrega R/V/C projetados — `matriz.md:84–87`, `brief.md:64` | **CONFERE:** `app/chamados/services/painel.py:64`. |
| D14, ETag consome esse marcador — `matriz.md:89` | **CONFERE:** `app/chamados/services/painel.py:91`. |
| D15, painel conta V sem exigir staff — `matriz.md:92–96`, `brief.md:62` | **CONFERE:** `app/chamados/services/painel.py:261`, `app/chamados/services/painel.py:270`. |
| D16, reabrir conta R nulo — `matriz.md:101` | **CAIU:** conta a negação SQL do juiz; o universo continua sendo todas as perguntas da disputa, antes de buscar corrigíveis: `app/chamados/services/acoes_disputa.py:97`. |
| D17, regularizar testa R nulo — `matriz.md:103` | **CAIU:** consulta o juiz nas perguntas vivas: `app/chamados/services/acoes_disputa.py:132`. |
| D18, corrigir consulta V e texto — `matriz.md:106–107` | **MOVEU:** `app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/acoes_disputa.py:357`. |
| D19, marcos agregam C/R e respeitam `ate` — `matriz.md:111–113`, `brief.md:65` | **CONFERE:** `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:202`. |
| D20, A20 usa campos próprios — `matriz.md:61`, `brief.md:19–21` | **MOVEU:** `app/chamados/services/validacao.py:994`. A implementação integral de LU-08 está **[nao verificado]**; o corte diz que a conferência vale para o par conferido: `app/docs/CORTES.md:487`. |
| D21, escrita de validação/devolução e leituras internas — `matriz.md:65–67` | **MOVEU:** validação registra ato em `app/chamados/services/validacao.py:173`; devolução ainda lê C, motivo e V em `app/chamados/services/validacao.py:262` e registra ato em `app/chamados/services/validacao.py:279`. |

A F1 antiga pressuposta em `brief.md:57–59,66–67,85–87,129` e `matriz.md:9,128–129` **CAIU como contrato disponível**: o juiz encontrado responde se o par projetado tem instante e texto; a cópia não tem `app/chamados/services/atos_pergunta.py` **[nao verificado quanto ao histórico]**. Ele não demonstra o antigo redutor de validação corrente: `app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1551`.

O teste antigo “ato RESPOSTA presente com projeção vazia deve validar” não decorre desse juiz; em contrapartida, a premissa anterior de que as portas aceitavam instante sem texto também caiu: `app/chamados/juizes.py:1552`, `app/chamados/services/validacao.py:681`.

**CONFERE 5, MOVEU 12, CAIU 4.**

## 2 O que mudou

O anexo `diffstat_F3.txt` informa alterações em 17 arquivos entre `fbc5c76ce` e `dd489c627`; **o diff real e a atribuição dessas alterações a commits não puderam ser lidos**, porque a cópia não é um repositório Git **[nao verificado]**. A comparação abaixo usa a remedição fornecida como descrição da base antiga e os corpos presentes como estado observado.

- **Resposta nas portas e ações:** a remedição descrevia guardas por instante; agora validar, recusar, reabrir e regularizar consultam o juiz de resposta, inclusive sua face SQL: `app/chamados/services/validacao.py:681`, `app/chamados/services/validacao.py:901`, `app/chamados/services/acoes_disputa.py:97`, `app/chamados/services/acoes_disputa.py:133`. Isso altera o tratamento de instante sem texto; não entrega a leitura histórica de atos pretendida pela F3: `app/chamados/juizes.py:1544`.
- **Aceite:** além da comparação com a última cobrança, há agora `q_respondida_com_texto`; colaboradores continuam deduplicados: `app/chamados/services/verbos.py:333`, `app/chamados/services/verbos.py:339`.
- **Novo escritor encontrado na cópia:** a porta de estoque L-130 conserva linhas faltantes e zera o par de resposta pelo lote: `app/chamados/services/validacao.py:408`, `app/chamados/services/validacao.py:436`. A recusa de pares ilegais é consultada no nascimento, no lote e no `save`: `app/chamados/models.py:2956`, `app/chamados/models.py:3381`, `app/chamados/models.py:3599`.
- **Novos campos e leitor de atos:** o modelo declara `da_ressuscitacao` e `resposta_mantem_validacao`; a tarefa consulta o primeiro, e os escritores de resposta declaram o segundo: `app/chamados/models.py:4065`, `app/chamados/services/tarefa_do_dia.py:421`, `app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1679`. Isso é relevante para qualquer futura leitura da sequência; não demonstra que a F3 já a consome.
- **Leitores de placar e marcos:** permanecem projetivos nos sítios observados: `app/chamados/services/verbos.py:263`, `app/chamados/services/painel.py:64`, `app/chamados/veredito_lavrado.py:202`. A ausência de mudança histórica nesses arquivos está **[nao verificado]** sem o diff.
- **Furo do carimbo:** a remedição antiga relata que `carimbar(..., resposta='validar')` escreve V sem registrar `VALIDACAO`; o método atual e a chamada de absorção estão em `app/chamados/models.py:3222` e `app/chamados/services/materializacao.py:1048`. A quantidade atual de casos está **[nao verificado]**.

Os números de frota do brief são resultados históricos fornecidos, não medições atuais. Banco, logs e execução de testes: **[nao verificado]**. [nao verificado]

## 3 Nao verificado de ALCANCE

- **Duas cascas de foto — fechado estaticamente:** a rota do kernel inclui `api.urls_core`, que aponta a foto para `views_core`; as duas views chamam o serviço comum, que chama `validar_pergunta`: `app/config/urls_core.py:6`, `app/api/urls_core.py:8`, `app/api/views_core.py:844`, `app/api/views.py:860`, `app/api/foto_recebida.py:51`. Alcance efetivamente percorrido em produção: **[nao verificado]**.
- **Classe e folga fora da fatia — fechado nos chamadores identificados:** mensageria chama `classe_do_lote`; `vigia_fio` importa e chama o mesmo helper de aceite de folga usado pela validação: `app/api/views_mensageria.py:545`, `app/chamados/services/vigia_fio.py:247`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/validacao.py:711`.
- **Marcos e placar — fechado nos consumidores citados:** raio-X, fio, tarefa e auditoria consomem `marcos_do_lote`; painel, views e retrato consomem os placares: `app/chamados/services/raiox.py:38`, `app/chamados/services/fio.py:282`, `app/chamados/services/tarefa_do_dia.py:440`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1165`, `app/chamados/services/painel.py:298`, `app/chamados/views.py:1488`, `app/folha/views_retrato.py:515`. O retorno de marcos é temporal e é comparado a `julgado_em`: `app/chamados/veredito_lavrado.py:152`, `app/chamados/veredito_lavrado.py:232`.
- **Escritores e leitores globais — aberto:** a remedição fornece um censo anterior, mas não foi possível reconferir integralmente cada campo, alias, escrita ORM e template contra o delta real. Faltam histórico Git e um novo censo completo da cópia; nomes construídos dinamicamente também permanecem **[nao verificado]**. Exemplos novos observados estão em `app/chamados/services/validacao.py:408` e `app/chamados/services/tarefa_do_dia.py:421`.
- **Arquivos cruzados com B2/F2/F4/F7/F5/F6/F8/F9 — aberto:** a tabela histórica da remedição não prova os diffs ou escopos finais dessas obras. Faltam seus diffs e briefs correspondentes **[nao verificado]**.
- **Retroatividade de D06 — aberto como verificação de alcance e contrato:** `marcos_do_lote` exige `datetime` e corte `ate`, enquanto a conservação pode inserir uma linha com instante antigo: `app/chamados/veredito_lavrado.py:151`, `app/chamados/models.py:4122`. Falta demonstrar como uma alteração retroativa alcançará cada consumidor preservando esses contratos **[nao verificado]**.

**alcance nao verificado restante: 3** — censo global, cruzamento final das obras e propagação de ato retroativo. A ocorrência real das rotas em produção também permanece **[nao verificado]**, mas não impede o fechamento estático das duas cascas.

## 4 Perguntas de lei

A busca por resposta em `LEIS.md`, `CORTES.md` e `DOSSIES.md` encontrou:

- **DECIDIDO PELA LEI L-130:** resposta corrente exige instante e texto; o corte registra a mesma definição: `app/docs/LEIS.md:151`, `app/docs/CORTES.md:507`.
- **DECIDIDO PELA LEI LU-08:** conferência vale para o par conferido: `app/docs/CORTES.md:487`.
- **DECIDIDO PELO CORTE D04:** duas validações com devolução no meio contam como dois atos; o registro localizado está no backlog, não nas três fontes de lei pesquisadas: `app/docs/BACKLOG.md:2137`. A exceção por mesmo request não aparece nesse registro **[nao verificado]**.
- O requisito de staff é identificado no código por `EQUIPE-STAFF-0708`, mas o literal desse corte nas três fontes pesquisadas está **[nao verificado]**: `app/chamados/services/verbos.py:265`.
- O brief determina contar resposta posterior à cobrança sem filtro de autor (`brief.md:63`); `ato_por` admite nulo: `app/chamados/models.py:4054`. Essa definição fornecida não exige presumir a identidade do autor nulo.

Perguntas ainda abertas, sem propor resposta:

1. **Qual é o predicado autorizado para validação corrente a partir da sequência de atos?** O contrato antigo não está disponível na cópia, e as guardas ainda usam V projetada: `app/chamados/juizes.py:1544`, `app/chamados/services/validacao.py:685`, `app/chamados/services/validacao.py:898`.
2. **No aceite por atos, uma resposta histórica posterior à cobrança conta mesmo se a resposta corrente não satisfaz L-130?** O brief pede ato posterior à cobrança (`brief.md:63`), enquanto a implementação atual exige o juiz de resposta corrente: `app/chamados/services/verbos.py:333`, `app/docs/LEIS.md:151`. A conciliação dessas duas respostas candidatas está **[nao verificado]**.

perguntas de lei: 2

## 18/11 08:59 · sessao c4e7a94d2442 · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, com referências aos predicados e às origens das atas.

## 18/11 08:58 · sessao c7370679eb16 · - · - · origem=Ronald (tmux)

Rastreamento da pergunta 3 enviado ao agente principal, com referências aos quatro consumidores de `_protesta`, aos três ramos de `classificar_dia`, aos leitores de `cobrado` identificados e à cadeia de fechamento, pauta e perguntas. A exaustividade dos leitores indiretos permanece não verificada.

## 18/11 08:58 · sessao d60df4f05b8c · - · - · origem=Ronald (tmux)

A ata deste julgamento recebe `tipo_dia` em [cartorio.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:738), copiando o valor do dia da grade. A grade é montada e indexada em [cartorio.py:1175](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1175); `_ata_agora` passa a ata por dispensa de ausência e teto em [cartorio.py:1436](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1436). Essas duas funções alteram lâmpadas e contagem de faltantes, preservando o tipo: [cartorio.py:800](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:800), [cartorio.py:907](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:907).

Os valores da grade incluem `folga` e `trabalho` nos construtores em [utils.py:760](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:760) e [utils.py:1801](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1801); folga lançada válida pode sobrepor `folga` em [utils.py:1147](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1147); cobertura pode sobrepor `falta` ou `ausencia` em [utils.py:1210](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1210). No construtor por turno, os overlays de folga e ausência são aplicados nessa ordem em [utils.py:1843](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/utils.py:1843).

Esses overlays permitem que o tipo da ata seja diferente de `trabalho` mesmo com `cel.trabalha` verdadeiro ou indefinido; o teste com `trabalha=True` e ausência parcial está em [test_emis_ausencia_parcial_cobra.py:78](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/tests/test_emis_ausencia_parcial_cobra.py:78). A ocorrência dessas combinações nos dados de produção fornecidos é [não verificado].

## 18/11 08:58 · sessao 42fbf1d34662 · ESPINHA-REMEDIDA-1811 · censo · origem=Code

## 1 Premissas e sitios

**A remedição está incompleta.** A cópia contém os arquivos consultados, mas não contém metadados Git; por isso, a identidade da árvore como `dd489c627` e a comparação com os hashes anteriores estão **[nao verificado]**. As referências `brief` e `matriz` abaixo são às linhas dos anexos transcritos no pedido; sua correspondência com arquivos externos à cópia está **[nao verificado]**.

| Premissa e sítio do anexo | Julgamento na cópia |
|---|---|
| Contrato de entrada D01 `leitura(...decisoes, agora...)` e `LeituraDosAtos` — brief:106–107; matriz:22–28 | **CAIU** como descrição do juiz encontrado: `pergunta_respondida` devolve booleano a partir de instante e texto da projeção. A existência de outro redutor equivalente em toda a árvore permanece **[nao verificado]**. (`app/chamados/juizes.py:1544`, `:1552`) |
| Juiz do slot usa embrulho com texto D01 — matriz:91 | **CAIU**: passa a própria pergunta ao derivador de hora; o ramo `data_hora` lê `resposta_colab`. (`app/chamados/juizes.py:363`, `:364`, `:376`) |
| Materializador e juiz já leem fontes diferentes por causa da F1 — matriz:94–99 | **CAIU** para os sítios examinados: ambos leem a projeção. O corte ainda registra a ordem de pouso ou o contador J1. (`app/chamados/juizes.py:364`, `:376`; `app/chamados/services/materializacao.py:625`, `:678`, `:1580`; `app/docs/CORTES.md:509`) |
| Fila seleciona pelo D01 — matriz:107 | **CAIU**: a fila usa as duas faces SQL da resposta, validação e carimbo da projeção; o meio par com instante sem texto entra para recusa com dono. (`app/chamados/services/materializacao.py:2087`, `:2091`) |
| Três chamadas ao derivador bastam — brief:115–118; matriz:110 | **CAIU**: há cinco sítios diretos identificados: juiz, duas metades do par, testemunha e plantio. (`app/chamados/juizes.py:376`; `app/chamados/services/materializacao.py:214`, `:222`, `:898`, `:1348`) |
| Ninguém lê o ato ABSORCAO — matriz:43, M9b | **CAIU**: a tarefa lê esse ato. (`app/chamados/services/tarefa_do_dia.py:178`) |
| Absorvidas continuam agrupadas pelo carimbo — brief:38; matriz:44, M10 | **MOVEU** para `app/chamados/services/materializacao.py:1088`; a seleção usa carimbo, via e texto, e exclui pergunta com batida viva. (`app/chamados/services/materializacao.py:1108`, `:1112`, `:1119`) |
| Guardas de reparo permanecem na projeção — brief:166–169; matriz:72–80 | **MOVEU** para `app/chamados/services/disputa_emissao.py:1664`, `:1694`, `:1864`; a permanência foi decidida no corte. (`app/docs/CORTES.md:509`) |
| Texto de hora e de data/hora é lido pela projeção — matriz:34–38, 49, M1/M2/M5/M16 | **MOVEU** para `app/chamados/services/materializacao.py:546`, `:625`, `:678`, `:1580`. |
| Reabertura limpa resposta e validação e registra a fotografia — matriz:71–73, E8–E10 | **MOVEU** para `app/chamados/services/disputa_emissao.py:1610`, `:1632`, `:1636`. |
| Respondedor humano grava resposta e limpa validação anterior — matriz:64–66, E1/E3 | **CONFERE** nos sítios `app/chamados/services/disputa_emissao.py:1481`, `:1484`, `:1494`, `:1525`. |
| L-130 exige instante e texto para “respondida” — remedição: premissa central | **CONFERE** em `app/chamados/juizes.py:1544`, `:1552` e `app/docs/LEIS.md:151`. |

Estas 12 linhas são os **sítios que consegui julgar**, não a totalidade das premissas do brief e da matriz. A contagem abaixo não é o fecho integral pedido; faltam os demais degraus e a conferência histórica. **CONFERE 2, MOVEU 4, CAIU 6**.

## 2 O que mudou

O anexo `diffstat_F4.txt` informa 11 arquivos alterados entre `520d7b557` e `dd489c627`, mas **não consegui ler o diff real**: `git diff` e `git show` não funcionam nesta cópia sem `.git`, e não encontrei outro repositório local na busca realizada. Portanto, atribuir cada comportamento abaixo a um commit ou afirmar que nasceu nesse intervalo fica **[nao verificado]**.

O que a árvore disponível mostra, relevante para a fatia:

- A fila de materialização reconhece a resposta pelo juiz L-130 e encaminha o meio par com instante sem texto à recusa com dono; os três ramos de materialização consultam essa guarda. (`app/chamados/services/materializacao.py:1269`, `:1342`, `:1577`, `:1661`, `:2087`)
- A testemunha usa o mesmo juiz para decidir se há resposta e consulta a composição de competência antes de derivar o plantio. (`app/chamados/services/materializacao.py:869`, `:888`, `:898`)
- O predicado K6 ainda exclui U163 antes de consultar o dia do fato. A composição de K6 com F6 está em outro sítio; a ampliação determinada para U163, portanto, não está demonstrada nesse predicado. (`app/chamados/services/materializacao.py:792`, `:795`, `:806`, `:818`; `app/docs/RELATO.md:370`)
- As respostas humana, automática e pela trilha declaram `resposta_mantem_validacao` com valores diferentes; o modelo também contém `da_ressuscitacao`, lido pela carga da tarefa. Esses são escritores e leitor adicionais a considerar na fatia de atos; sua data de entrada no intervalo é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1525`, `:1527`, `:1679`, `:1681`, `:1701`, `:1703`; `app/chamados/models.py:4071`, `:4077`; `app/chamados/services/tarefa_do_dia.py:426`)
- Além dos chamadores antigos, há duas chamadas do derivador para o par e consumidores da testemunha no contador, no explicador e em tag de template. Sua novidade histórica é **[nao verificado]**. (`app/chamados/services/materializacao.py:214`, `:222`, `:339`; `app/chamados/services/explicador.py:124`; `app/chamados/templatetags/chamados_tags.py:136`)

## 3 Nao verificado de ALCANCE

Parte do alcance foi fechada pela leitura:

- **ABSORCAO:** o produtor registra o ato e seus vínculos; a tarefa o lê; o leitor usado para devolução continua selecionando pelo carimbo. Isso corrige a afirmação de que ninguém lia o ato. (`app/chamados/services/materializacao.py:1048`, `:1050`, `:1088`; `app/chamados/services/tarefa_do_dia.py:178`)
- **Cascas:** o fio prepara `estado_aplicacao`; o modal o exibe e lê a resposta; o cartão de atendimento também exibe a resposta. Isso identifica leitores nas duas cascas, mas não constitui censo exaustivo delas. (`app/chamados/services/fio.py:376`; `app/templates/chamados/partials/modal_fio.html:217`, `:219`, `:230`; `app/templates/chamados/partials/_card_atendimento.html:106`, `:141`)
- **Costura de hora:** foram encontrados cinco sítios diretos de chamada, inclusive as duas metades do par. O parse de `data_hora` continua separado entre juiz e materializador. (`app/chamados/juizes.py:363`, `:376`; `app/chamados/services/materializacao.py:214`, `:222`, `:898`, `:1348`, `:1580`)

Continuam abertos **[nao verificado]**: exaustividade dos escritores e leitores dos campos e funções, chamadas indiretas e dinâmicas, todos os arquivos cruzados e todos os leitores das duas cascas. Falta concluir o censo global e conferir cada ocorrência contra o modelo e a porta correspondentes. O diff histórico também é necessário para distinguir sítio novo no censo de sítio efetivamente introduzido no intervalo. (`app/chamados/models.py:3216`, `:3222`, `:3371`, `:4122`, `:4152`)

**alcance nao verificado restante: [nao verificado]**

## 4 Perguntas de lei

A busca nos arquivos de lei e cortes encontrou respostas para as duas perguntas antigas da matriz:

- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 3:** M9b e M10 ficam no carimbo nesta F4; o predicado de absorção por ato pertence à E4d. (`app/docs/CORTES.md:509`)
- **DECIDIDO PELA LEI CORTES-F2-F9-1611, item 4:** as guardas E11, E14 e E17 ficam na projeção por allowlist de reparo. (`app/docs/CORTES.md:509`)
- **DECIDIDO PELA LEI L-130:** “respondida” exige instante e texto não vazio; isso não demonstra, por si só, o contrato antigo D01. (`app/docs/LEIS.md:151`; `app/chamados/juizes.py:1544`)

As duas respostas U163 fornecidas no pedido estão **respondidas** e não entram na contagem: o RED fixa a metade respondida, sem validação ou materialização anteriores, com declarado igual ao gravado e demais guardas liberadas; A4 exige também nenhuma batida do mesmo tipo em ±20 minutos, aprovada ou não. O A4 registrado acrescenta ausência de qualquer batida no instante exato. O código consultado sustenta as duas guardas de batida. (`app/chamados/juizes.py:239`; `app/ponto/registro_batida.py:144`; registro ORDEM-VIVA e A4 no dado fornecido)

Não posso afirmar um total integral de perguntas de lei: a revisão de **todas** as premissas e do alcance não foi concluída, e o diff real exigido não estava disponível. Qualquer pergunta adicional ou conclusão de que não existe outra permanece **[nao verificado]**.

perguntas de lei: [nao verificado]

## 18/11 08:58 · sessao 6f4ff414d080 · ESPURIA-TELA · censo · origem=Code

**O censo não está fechado.** A busca textual foi rodada e encontrou 925 linhas candidatas; a análise AST da cópia não encontrou erros de sintaxe nos arquivos Python. Não concluí a resolução dos apelidos, a classificação de cada ocorrência, o grafo completo das cascas nem a contagem de leitores. Portanto, esses 925 resultados **não são uma contagem de leitores**. [não verificado]

O comando abaixo é o levantamento parcial que rodei, sem Django e sem gravar arquivos. Ele **não é o script de censo solicitado**: conta candidatos, mas não lista nem classifica os achados, não resolve aliases e não constrói o grafo de templates. [não verificado] [nao verificado]

```python
import ast
import pathlib
import subprocess

root = pathlib.Path("app")
pat = (
    r"dia_decidido|veredito_do_dia|precedencia|"
    r"decididos_do_periodo|\bdo_dia\b|palavra_dia"
)

p = subprocess.run(
    [
        "rg", "-i", "-n", "--no-heading",
        "--glob", "*.py", "--glob", "*.html", "--glob", "*.js",
        pat, str(root),
    ],
    capture_output=True,
    text=True,
)

print("rg exit:", p.returncode, "ocorrencias:", len(p.stdout.splitlines()))

errors = []
for f in root.rglob("*.py"):
    try:
        ast.parse(f.read_text(encoding="utf-8"))
    except (SyntaxError, UnicodeError) as e:
        errors.append((str(f), str(e)))

print("erros AST:", len(errors))
for e in errors:
    print(*e)
```

Resultado: `rg exit: 0 ocorrencias: 925`; `erros AST: 0`. A saída integral dos achados e a prova de completude ficam **[não verificado]**. A cópia não tem `.git`, de modo que não pude confirmar o hash pedido por `git rev-parse`. [não verificado]

As sete sementes existem nas linhas indicadas:

- [espelho.py:328](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:328) importa `do_dia`; a chamada está em [espelho.py:340](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:340).
- [espelho.py:646](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/espelho.py:646) importa `fatos_do_periodo`; a linha seguinte importa `decididos_do_periodo`.
- [calendario.py:533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:533), [calendario.py:563](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:563) e [calendario.py:738](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:738) importam, respectivamente, fatos, decisões e `do_dia`; as chamadas aparecem em [calendario.py:539](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:539), [calendario.py:566](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:566) e [calendario.py:748](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:748).
- [_calendario_grade.html:98](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_calendario_grade.html:98) lê `palavra_dia`, `veredito_dia` e `cor_dia`.
- [espelho.html:1](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/ponto/espelho.html:1) estende `base_tpl`, com `base.html` como padrão.

Sobre as cascas, o espelho pode usar **ambas**: o `context_processor` escolhe `base_app.html` ou `base.html` para `base_tpl` em [context_processors.py:195](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/core/context_processors.py:195). A grade é incluída por [furos.html:42](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/relatorios/furos.html:42), que estende `base.html` em [furos.html:1](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/relatorios/furos.html:1), e também por [calendario_partial.html:67](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/calendario_partial.html:67) e [_drawer_diagnostico.html:35](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_drawer_diagnostico.html:35). A classificação de todos os caminhos desses dois partials, o grafo completo e as quatro contagens permanecem **[não verificado]**.

Há evidência de montagem visual que precisa entrar na revisão L-061: [dia_decidido.py:376](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/dia_decidido.py:376) escolhe ramos a partir de cobertura, decisão e `fatos['tipo_do_dia']`, inclusive folga e feriado, sem que esse trecho leia uma lâmpada. A grade pinta fundos segundo `dia.status` em [_calendario_grade.html:69](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/templates/colaboradores/partials/_calendario_grade.html:69); esse status é carregado por `status_dos_dias_lavrados` em [calendario.py:635](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/calendario.py:635). Esses trechos são **candidatos para o censo**, não uma lista completa de violações L-061 nem prova de que cada cor corresponda ao tipo da lâmpada. [não verificado]

perguntas de lei: 0

Essa contagem segue o aval fornecido, que diz “Nenhuma lei nova”; como o censo ficou incompleto, a existência de perguntas reveladas pelos leitores ainda não examinados permanece **[não verificado]**.

## 18/11 08:58 · sessao a57d05368f66 · DIA-SEM-TRABALHO-PORTA-L1 · leitura · origem=Code

A cópia aberta não contém metadados Git; portanto, **o HEAD `dd489c627` está [não verificado]**. Os caminhos e linhas abaixo foram conferidos na cópia disponível. O brief e `pd_item4.r` foram fornecidos como texto raspado, mas não foram localizados como arquivos nessa cópia; suas linhas no HEAD estão [não verificado]. [app/ponto/services/veredito_celula.py:211](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/veredito_celula.py:211)

### Matriz de transições

Os quatro estados são os **resultados pedidos**, usados aqui para organizar a análise; sua representação unificada e suas transições na porta atual estão [não verificado]. As referências numeradas da última coluna comprovam as células da linha, na ordem em que são apresentadas. [app/escala/models.py:1759](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/escala/models.py:1759)

| Estado ou transição | Juiz: quem decide | Operação que roda hoje | Persistido | Retorno ou recusa | Prova |
|---|---|---|---|---|---|
| **Ainda sem decisão** | `decisao_do_dia` identifica veredito humano por `veredito_via`; isso responde ao Resolver dia, não aos quatro resultados pedidos. [1] | A grade conserva sobras como órfãs; o cartório monta a ata. A oferta de Resolver dia exige último recurso, passado e status elegível. [2] | `CelulaDia.ata`, inclusive tipo e órfãs, pode ser lavrada; veredito automático não é, por si, decisão do admin. [3] | `decisao_do_dia` devolve mapa vazio quando não encontra veredito humano; a oferta é booleana e pode ser falsa. [1, 4] | [1: veredito_celula.py:85](app/ponto/services/veredito_celula.py#L85); [2: utils.py:790](app/escala/utils.py#L790), [cartorio.py:730](app/ponto/services/cartorio.py#L730); [3: celula.py:1032](app/ponto/portas/celula.py#L1032); [4: calendario.py:94](app/colaboradores/services/calendario.py#L94) |
| **Sem decisão → trabalho reconhecido** | A decisão específica de reconhecer trabalho neste universo está [não verificado]; o vocabulário humano existente inclui `trabalhou`. [1] | `veredito_celula(..., veredito='trabalhou')` avalia a grade. [2] | Quando `tipo_dia != 'trabalho'`, retorna antes de lavrar o veredito ou registrar a trilha dessa operação. [2, 3] | Recusa com `ok=False` e “a escala nao preve trabalho nesse dia”. [2] | [1: models.py:1759](app/escala/models.py#L1759); [2: veredito_celula.py:221](app/ponto/services/veredito_celula.py#L221); [3: veredito_celula.py:351](app/ponto/services/veredito_celula.py#L351) |
| **Sem decisão → escala a corrigir → dia normal** | A autoridade do previsto é a célula; ela admite geração e edição humana. Uma decisão DST “escala a corrigir” está [não verificado]. [1] | A correção concreta e seu encadeamento a partir de Resolver dia estão [não verificado]. Se a grade passar a `trabalho`, a recusa específica acima deixa de incidir; as demais condições de `veredito_celula` continuam aplicáveis. [2] | A célula guarda o previsto; a batida não escreve a célula. A trilha específica da decisão DST está [não verificado]. [1] | Resultado completo da correção e transição automática para dia normal: [não verificado]. | [1: models.py:1635](app/escala/models.py#L1635); [2: veredito_celula.py:242](app/ponto/services/veredito_celula.py#L242) |
| **Sem decisão → batida invalidada** | Uma decisão DST com o significado completo “não vale” está [não verificado]. A operação existente de retratação atua sobre a batida. [1] | `retratar_batida` anota `aprovada=False` e `retratada_em` e aciona fechamento por evento. [1] | Campos da `Batida` e trilha pela operação `anotar`; a batida retratada sai da apuração. A retenção de uma decisão DST anterior está [não verificado]. [1] | Devolve a batida; se já retratada, devolve-a sem repetir a alteração. O pagamento final deste caso está [não verificado]. [1] | [1: registro_batida.py:287](app/ponto/registro_batida.py#L287) |
| **Nova execução da mesma decisão** | Na lavra da célula, a comparação é feita por `lavrar_veredito`; na HE, a vigência vem de `DecisaoHE.objects.vigentes()`. [1, 2] | A mesma lavra pode não escrever; `decidir_he` com mesmo estado vigente retorna antes de `_gravar`. A operação completa `veredito_celula` também chama `registrar_log`, mesmo após a lavra: não se pode concluir uma única linha de trilha pela idempotência da lavra. [1, 3, 4] | Lavra idêntica sem mudança de status: nada; repetição de HE vigente: nenhuma nova trilha de HE. A trilha única da porta DST está [não verificado]. [1, 4] | Lavra retorna `[]`; HE retorna `no_op=True`, depois das validações, inclusive minutos positivos. [1, 4] | [1: celula.py:1075](app/ponto/portas/celula.py#L1075); [2: models.py:1265](app/ponto/models.py#L1265); [3: veredito_celula.py:351](app/ponto/services/veredito_celula.py#L351); [4: he.py:186](app/ponto/portas/he.py#L186) |
| **Revogação ou troca de decisão** | A lei de revogação dos quatro estados DST está [não verificado]. Para HE, `vigentes()` exclui revogadas. [1] | `decidir_he` troca `autorizado`/`nao`; `revogar_recusa_do_sistema` tem alcance limitado à recusa do sistema e recusa tocar decisão de admin. [2, 3] | A troca atualiza a linha `DecisaoHE`; revogação conserva a linha. `_gravar` tenta trilha, mas falha de trilha não desfaz a decisão. [4, 3] | Mesma HE vigente: `no_op`; HE diferente: `trocada`; revogação de decisão de admin: `revogada=False`, motivo `decisao do admin`. Transições entre os quatro estados DST: [não verificado]. [2, 3] | [1: models.py:1251](app/ponto/models.py#L1251); [2: he.py:195](app/ponto/portas/he.py#L195); [3: he.py:421](app/ponto/portas/he.py#L421); [4: he.py:61](app/ponto/portas/he.py#L61) |
| **HE decidida e vigente + batida órfã na ata em dia sem turno** | `vigentes()` responde à vigência da HE; `dia_sem_trabalho` responde se a célula cala cobrança, com exceção para ausência parcial. Nenhum dos dois, pelos trechos abertos, responde aos quatro estados DST. [1, 2] | A grade põe a sobra em `orfas`; o cartório a conta e a inclui na ata; o supra-juiz a vê como fato. `perguntar_entradas_pendentes` só emite quando `classificar_falta` devolve motivo; com `trabalha=False`, este retorna motivo nulo. [3, 4, 5] | A ata guarda órfãs; a `DecisaoHE` vigente permanece sendo uma decisão sobre minutos fora da janela. [6, 7] | `estado_por_dia` vê a HE vigente. `_celula_furada`, em dia sem trabalho, consulta a véspera antes de chegar à leitura das órfãs do próprio dia. `veredito_celula('trabalhou')` recusa se a grade diz tipo diferente de trabalho. O efeito conjunto na porta DST está [não verificado]. [8, 9, 10] | [1: models.py:1265](app/ponto/models.py#L1265); [2: juizes.py:445](app/chamados/juizes.py#L445); [3: utils.py:790](app/escala/utils.py#L790), [cartorio.py:1424](app/ponto/services/cartorio.py#L1424), [supra_juiz.py:392](app/ponto/supra_juiz.py#L392); [4: disputa_emissao.py:257](app/chamados/services/disputa_emissao.py#L257); [5: servico_jornada.py:308](app/escala/servico_jornada.py#L308); [6: cartorio.py:738](app/ponto/services/cartorio.py#L738); [7: models.py:1270](app/ponto/models.py#L1270); [8: gestao_he.py:63](app/ponto/services/gestao_he.py#L63); [9: juizes.py:497](app/chamados/juizes.py#L497); [10: veredito_celula.py:243](app/ponto/services/veredito_celula.py#L243) |

A porta de HE recebe os minutos do chamador e exige valor positivo; isso não demonstra que uma órfã em dia sem turno produza minutos elegíveis nem que a autorização de HE resolva a validade da batida. [app/ponto/portas/he.py:159](app/ponto/portas/he.py#L159), [app/ponto/portas/he.py:186](app/ponto/portas/he.py#L186)

### Lei que falta

**Nome de trabalho:** `decisao_dia_sem_trabalho`. **Predicado:** qual decisão vigente do admin se aplica ao fato: ainda sem decisão, trabalho reconhecido, escala a corrigir ou batida invalidada. **Universo:** pessoa × dia do turno cuja célula diz sem trabalho e que contém batida apurável. A existência de uma autoridade que já responda a essa pergunta completa está [não verificado]. A unidade e os resultados são os do brief fornecido; o arquivo do brief no HEAD está [não verificado]. [app/escala/models.py:1759](app/escala/models.py#L1759), [app/ponto/models.py:1270](app/ponto/models.py#L1270)

A alternativa existente mais próxima para **decisão humana por pessoa e dia** é `decisao_do_dia`: ela lê o veredito humano da célula e sua autoria na trilha, mas seu vocabulário é `trabalhou`, `faltou`, `justificado`; além disso, a operação que o grava recusa `trabalhou` quando a grade diz tipo diferente de trabalho. Seu universo são os dias com veredito humano, sem exigir a combinação dia sem trabalho + batida apurável. [app/ponto/services/veredito_celula.py:85](app/ponto/services/veredito_celula.py#L85), [app/ponto/services/veredito_celula.py:18](app/ponto/services/veredito_celula.py#L18), [app/ponto/services/veredito_celula.py:243](app/ponto/services/veredito_celula.py#L243)

PERGUNTA DE DESENHO DST-L1: juiz decisao_dia_sem_trabalho com predicado qual decisão vigente do admin se aplica ao fato entre sem decisão, trabalho reconhecido, escala a corrigir e batida invalidada; universo pessoa × dia do turno com célula sem trabalho e batida apurável; alternativa existente decisao_do_dia não responde porque lê outro vocabulário e outro universo, e sua operação recusa trabalhou quando a grade não diz trabalho ([veredito_celula.py:85](app/ponto/services/veredito_celula.py#L85), [veredito_celula.py:243](app/ponto/services/veredito_celula.py#L243); definição do universo no arquivo do brief: [não verificado]).

## 18/11 08:58 · sessao afdbea80ef90 · - · - · origem=Ronald (tmux)

Resultados enviados ao agente principal, incluindo os sítios dos escritores e leitores de silêncio/absorção e a divergência entre a partição atual e o corte sobre resposta vazia. A confirmação do HEAD e a leitura do diff real por Git ficaram **[nao verificado]**, pois a cópia disponível não contém um repositório Git.

## 18/11 08:57 · sessao 7eb2c5c2649f · - · - · origem=Ronald (tmux)

Conferência das premissas 1–23 enviada ao agente principal, com referências à árvore e contagem parcial: **CONFERE 2, MOVEU 9, CAIU 12**.

O diretório fornecido não contém `.git`; portanto, a verificação independente do HEAD e do diff real por Git permanece **[nao verificado]**.

## 18/11 08:57 · sessao 07a910eb066e · - · - · origem=Ronald (tmux)

O ponto não verificado está no **brief anexado, linhas 90 e 118**: a proposta de classificar silêncio como histórico e absorção rejeitada como pendente na partição do app.

Os escritores e leitores foram identificados: o fechamento por administrador registra `SILENCIO` em `app/chamados/models.py:2010`–`2015`; a materialização registra `ABSORCAO` em `app/chamados/services/materializacao.py:1046`–`1051`; `_pela_decisao` lê o último ato e, para absorção, o status da ausência em `app/chamados/services/tarefa_do_dia.py:158` e `:176`–`:186`.

A transposição para o app **não fecha pelo código**: a consulta primeiro seleciona perguntas vivas (`app/chamados/services/pergunta_no_app.py:45`–`:61`), e a partição usa o juiz de resposta e o fechamento da disputa, sem ler aqueles atos (`app/chamados/services/pergunta_no_app.py:76`–`:85`). Não se pode presumir que toda pergunta com esses atos chegue à partição.

O corte posterior resolve o dono administrativo do silêncio na tarefa (`app/docs/RELATO.md:202`; `app/docs/GRADE-MESTRA.md:1820`), mas não determina expressamente a classificação dessa pergunta na partição. O corte F4 reserva o predicado de absorção por ato para E4d (`app/docs/CORTES.md:509`, item 3).

Pergunta a levar à seção 4: **se uma pergunta cujo último ato é SILENCIO ou ABSORCAO chega viva à partição, a F5 deve classificá-la pelo juiz atual de resposta e fechamento, ou pelos resultados propostos no brief — silêncio no histórico e absorção rejeitada pendente?**

## 18/11 08:57 · sessao eed6dcb33eaa · ORDEM-1811-ITEM7 · leitura · origem=Code

**Há falha de leitura identificada, mas os anexos não permitem classificar todos os 21 colaboradores como falha nossa ou resposta correta da lei.** O resultado `nao_sei` é a resposta correta do juiz quando ele não consegue ler o dia; a falta dessa leitura é um problema de ESTRUTURA, com esperado zero. Já `nada` ou porta `None` significa ausência de porta publicada, mas, sozinho, não prova que uma porta era devida. A L-099 distingue defeito estrutural de problema de cadastro ou batida (`app/folha/fora_da_folha.py:55`, `app/chamados/services/tarefa_do_dia.py:718`, `app/docs/LEIS.md:122`).

As referências `sonda_a2.compacto.txt:linha` abaixo numeram as linhas de dados do **anexo transcrito na pergunta**, começando pela primeira linha `emp=1 col950`; não são linhas verificadas em arquivo local. O anexo informa o resultado atual da tarefa, mas não fornece as lâmpadas, instâncias, perguntas, estado da ausência, oferta do calendário ou rota. Por isso, onde esses insumos decidem entre os ramos, a causa exata está **[não verificado]**.

A regra comum de porta é esta: a tarefa coleta ações alcançáveis e portas residuais; se não encontra dono, escolhe `nao_sei` quando o indicador de desconhecimento está ligado, senão `nada`. `porta_da_tarefa` devolve `None` quando não há rota nem desconhecimento; se há desconhecimento, publica um envelope mesmo sem rota (`app/chamados/services/tarefa_do_dia.py:633`, `app/chamados/services/tarefa_do_dia.py:675`, `app/chamados/services/tarefa_do_dia.py:718`). `tarefa.fio=None` no anexo **não basta para identificar qual condição anterior eliminou a porta**.

## Grupos por célula, veredito e status

Datas sem ano são de 2026. “Obra” significa correspondência demonstrada pelo texto da obra com os fatos disponíveis, não apenas semelhança do rótulo.

| Grupo e dados do anexo | Causa lida no código; obra e julgamento |
|---|---|
| **Sem veredito/status, trabalha=True, ausência=None, 0 batidas, `ata_sem_lampadas`**: emp1 **col950** 21, 22, 23, 27 e 30/10; 17/11 (`sonda_a2.compacto.txt:1`, `sonda_a2.compacto.txt:11`). | Para dia que o juiz não considera sem trabalho, sem instâncias de lâmpada e sem o retorno de teto ainda não vencido, `_tarefa_pura` retorna `nao_sei/ata_sem_lampadas` (`app/chamados/services/tarefa_do_dia.py:233`). **Falha nossa de leitura**, ainda que `nao_sei` seja a resposta correta diante do desconhecimento; col950 é teste, conforme o enunciado. **Sem obra comprovada**: não há dado que identifique uma das obras listadas como cura desse padrão. |
| **Sem veredito/status, trabalha=False, ausência=None, 0 batidas, `nada`**: emp1 **col950** 24, 28, 29 e 31/10; 01/11 (`sonda_a2.compacto.txt:4`, `sonda_a2.compacto.txt:10`). | Quando o juiz reconhece dia sem trabalho, a guarda de `ata_sem_lampadas` não se aplica; sem ação alcançável ou oferta de resolver, o resultado pode ser `nada` (`app/chamados/services/tarefa_do_dia.py:233`, `app/chamados/services/tarefa_do_dia.py:288`, `app/chamados/services/tarefa_do_dia.py:675`). O relato identifica **folga sem batida retendo folha sem porta** como ESTRUTURA e destino DIA-SEM-TRABALHO-PORTA; aqui, a natureza efetiva de folga também depende do juiz **[não verificado]**, embora `trabalha=False` seja compatível (`app/docs/RELATO.md:2585`). **Obra candidata: DIA-SEM-TRABALHO**, condicionada à confirmação do mesmo padrão. |
| **`nunca_bateu` / `irregular`, trabalha=True, ausência=None, 0**: emp2 **col1009** 13, 14, 16 e 17/11; **col1048** 16/11 (`sonda_a2.compacto.txt:12`, `sonda_a2.compacto.txt:24`). | No ramo sem adesão, sem cobertura integral e sem dia sem trabalho, o cartório grava `nunca_bateu`; esse veredito pode virar `irregular` (`app/ponto/services/cartorio.py:1211`, `app/ponto/services/cartorio.py:1289`, `app/ponto/status_dia.py:147`). Porta atual `None`; a condição específica que a suprimiu é **[não verificado]**. `nunca_bateu` tem encaminhamento de cadastro/lista do DP descrito no backlog; falta verificar se cada pessoa está nessa lista, portanto **falha de destino individual [não verificado]** (`app/docs/BACKLOG.md:1257`). **Sem obra comprovada entre as opções.** |
| **`indefinida` / `escala_suspeita`, trabalha=True, ausência=None**: emp2 **col639** 06, 08, 10/11, 1 batida e `nada`; 14 e 16/11, 0 e `None`; **col97** 01/11, 0 e `nada`; 03, 05, 07, 09, 11 e 13/11, 1 e `nada`; 17/11, 1 e `None`; **col476** 25/10, 0 e `None` (`sonda_a2.compacto.txt:16`, `sonda_a2.compacto.txt:20`, `sonda_a2.compacto.txt:28`, `sonda_a2.compacto.txt:34`, `sonda_a2.compacto.txt:36`, `sonda_a2.compacto.txt:46`). | Os códigos de dia indefinido/fase não declarada/parcial sem período derivam `indefinida`; sem precedência humana que vença, o status é `escala_suspeita` (`app/escala/models.py:1615`, `app/ponto/status_dia.py:143`). O juiz de porta não usa esse veredito, o número de batidas ou a cor como prova de ação: a guarda exata em cada dia é **[não verificado]**. `indefinida` pode ser cadastro com destino à lista do admin, mas vínculo, motivo e existência de destino nestes casos são **[não verificado]** (`app/docs/BACKLOG.md:1121`, `app/docs/LEIS.md:122`). **Sem obra comprovada**: ESCALA-NAO-DESCREVE exige órfã da grade, fato não fornecido (`app/docs/BACKLOG.md:2327`). |
| **`indefinida` / `folga_trabalhada`, trabalha=True, ausência=None, 1, `nada`**: emp2 **col97** 15/11 (`sonda_a2.compacto.txt:35`). | Em `indefinida`, folga humana/feriado com batida tem precedência e produz `folga_trabalhada`; sem dono alcançável, o fio pode ser `nada` (`app/ponto/status_dia.py:143`, `app/chamados/services/tarefa_do_dia.py:675`). **DIA-SEM-TRABALHO L1 é candidata**, pois nomeia batida em folga do calendário ou feriado sem trabalho e porta do admin; o tipo exato da folga e a existência de decisão prévia são **[não verificado]**, então a falha individual não está provada (`app/docs/BACKLOG.md:1968`). |
| **`indefinida` / `escala_suspeita`, trabalha=True, ausência preenchida, 4, `None`**: emp2 **col327** 10/11 (`sonda_a2.compacto.txt:26`). | A mesma precedência produz `indefinida/escala_suspeita`; uma FK de ausência, por si só, não informa cobertura integral, estado ou ação exigível. A tarefa trata cobertura aprovada, aguardando documento e a decidir de formas diferentes (`app/ponto/status_dia.py:143`, `app/chamados/services/tarefa_do_dia.py:309`). Guarda concreta, dono e acerto da ausência de porta **[não verificado]**. **Sem obra comprovada.** |
| **`cobrado` / `irregular`, trabalha=True, ausência preenchida, 0, `None`**: emp2 **col496** e **col558**, ambos 24/10 (`sonda_a2.compacto.txt:21`, `sonda_a2.compacto.txt:27`). | Na lavra normal, `cobrado` é gravado quando não há códigos e existe cobrança viva que protesta; o status pode ser `irregular` (`app/ponto/services/cartorio.py:1478`, `app/ponto/services/cartorio.py:1502`, `app/ponto/status_dia.py:147`). A FK não prova que a ausência esteja aprovada ou cubra a jornada; por isso, o ramo que resultou em porta `None` e se é correto estão **[não verificado]** (`app/chamados/services/tarefa_do_dia.py:309`). **Sem obra comprovada.** |
| **`cobrado` / `irregular`, trabalha=True, ausência=None, 4, `nao_sei`**: emp2 **col265** 31/10 (`sonda_a2.compacto.txt:22`). | A produção de `cobrado/irregular` segue as regras do grupo anterior; o `nao_sei` atual revela leitura desconhecida, mas a razão específica está vazia no anexo e a instância não foi fornecida, logo o predicado concreto é **[não verificado]** (`app/ponto/services/cartorio.py:1478`, `app/chamados/services/tarefa_do_dia.py:298`). **Falha nossa de leitura; `nao_sei` é a resposta esperada ao desconhecimento** (`app/folha/fora_da_folha.py:55`). **Sem obra comprovada.** |
| **`cobrado` / `irregular`, trabalha=True, ausência=None, 2, `None`**: emp2 **col719** 01, 03 e 05/11 (`sonda_a2.compacto.txt:43`). | O cartório pode gravar `cobrado` pela cobrança viva e `irregular` pelo status; o resultado `None` diz que não foi publicada porta, mas a razão particular e se a cobrança ainda é exigível são **[não verificado]** (`app/ponto/services/cartorio.py:1478`, `app/chamados/services/tarefa_do_dia.py:718`). **Sem obra comprovada; falha individual [não verificado].** |
| **`cobrado` / `irregular`, trabalha=True, ausência=None, 0, tarefa atual `colab`**: emp2 **col905** 05/11 (`sonda_a2.compacto.txt:53`). | O retrato gravado registra sem porta, mas a sonda posterior registra fio atual `colab`; a seleção atual de `colab` exige ação de responder e pergunta pendente no app (`app/chamados/services/tarefa_do_dia.py:657`, `app/chamados/services/tarefa_do_dia.py:669`). A rota concreta, o instante e a causa da mudança desde 06:45 são **[não verificado]**. **Sem obra comprovada; não é possível afirmar que continua sem porta.** |
| **`fato_em_ausencia` / `irregular`, trabalha=True, ausência preenchida, 2, `None`**: emp2 **col994** 14/11; **col504** 29/10 e 11/11 (`sonda_a2.compacto.txt:23`, `sonda_a2.compacto.txt:41`). | `FATO_EM_AUSENCIA` deriva esse veredito, que pode produzir `irregular`; a tarefa só cria sua ação especial sem pergunta viva quando o juiz diz `sem_trabalho` e a competência não está exportada (`app/escala/models.py:1619`, `app/ponto/status_dia.py:159`, `app/chamados/services/tarefa_do_dia.py:352`). Se a ausência estava decidida e se o juiz respondeu sem trabalho são **[não verificado]**; ausência preenchida não basta. **DIA-SEM-TRABALHO** nomeia ausência lançada como dia decidido sem porta, mas a correspondência individual é **[não verificado]** (`app/docs/BACKLOG.md:1968`). |
| **Mesmo padrão, com 1 batida**: emp3 **col177** 08/11 (`sonda_a2.compacto.txt:55`). | Mesma regra e mesma limitação do grupo anterior: a contagem isolada não identifica o estado ou a cobertura da ausência, nem satisfaz por si o predicado especial da tarefa (`app/chamados/services/tarefa_do_dia.py:242`, `app/chamados/services/tarefa_do_dia.py:357`). **DIA-SEM-TRABALHO candidata; acerto ou falha [não verificado]** (`app/docs/BACKLOG.md:1968`). |
| **`furo` / `irregular`, trabalha=True, ausência=None, `nao_sei`**: emp2 **col192** 15/11, 4 batidas; **col407** 21/10 e 02/11, 1; 29/10, 0 (`sonda_a2.compacto.txt:25`, `sonda_a2.compacto.txt:38`). | Códigos de furo derivam `furo`, incluído no conjunto de status irregular; o envelope sem leitura é o resultado esperado para desconhecimento, mas o anexo não traz qual lâmpada ou autoridade disparou `nao_sei` (`app/escala/models.py:1612`, `app/ponto/status_dia.py:68`, `app/chamados/services/tarefa_do_dia.py:298`). **Falha nossa de leitura**, com causa específica **[não verificado]**; **sem obra comprovada** (`app/folha/fora_da_folha.py:55`). |
| **`furo` / `irregular`, trabalha=False, ausência=None, 2, `None`**: emp2 **col348** 24 e 31/10; 07, 08 e 14/11 (`sonda_a2.compacto.txt:47`). | O veredito deriva de códigos de furo, mas a ação especial de dia sem trabalho só contempla `fato_em_ausencia`, não `furo`; sem outra ação ou porta alcançável, a projeção pode ser `None` (`app/escala/models.py:1612`, `app/chamados/services/tarefa_do_dia.py:352`, `app/chamados/services/tarefa_do_dia.py:718`). **DIA-SEM-TRABALHO L1 é candidata** se essas datas forem folga do calendário ou feriado abrangido, sem decisão; esses fatos são **[não verificado]**, assim como a classificação definitiva de cada dia (`app/docs/BACKLOG.md:1968`). |
| **`furo` / `irregular`, trabalha=True, ausência=None, 1, `None`**: emp3 **col61** 03/11 (`sonda_a2.compacto.txt:54`). | O mapa grava `furo` e o status pode ser `irregular`; a sonda não mostra lâmpadas, perguntas ou oferta do calendário que expliquem a falta de porta (`app/escala/models.py:1612`, `app/chamados/services/tarefa_do_dia.py:633`). **Sem obra comprovada; falha ou resposta correta [não verificado].** |
| **`discordante` / `irregular`, trabalha=True, ausência=None, 0, `None`**: emp2 **col347** 13/11 (`sonda_a2.compacto.txt:37`). | O mapa admite `discordante` por órfã fora da tolerância, realizado inflado ou realizado zero com turno; a sonda não traz o código de protesto que escolheu a alternativa nem a razão da porta `None` (`app/escala/models.py:1616`, `app/chamados/services/tarefa_do_dia.py:718`). **Sem obra comprovada; dono e falha [não verificado].** |
| **`cobranca_indevida` / `ok`, trabalha=True, ausência=None, 4, `None`**: emp2 **col246** 01/11 (`sonda_a2.compacto.txt:52`). | `COBRANCA_EM_DIA_SEM_TRABALHO` ou `COBRANCA_SEM_FURO` deriva `cobranca_indevida`; com batida e sem precedência de folga, o status pode ser `ok`, embora o veredito conste entre os acusadores da retenção (`app/escala/models.py:1613`, `app/ponto/status_dia.py:157`, `app/escala/models.py:1767`). Qual código foi lavrado, se a retenção é devida e por que não há porta são **[não verificado]**; **sem obra comprovada**. |

## O que o retrato e a simulação mostram

O retrato de 06:45 registra **4 linhas** em `sem_leitura_sistema` — col950, col265, col192 e col407 — e **17** em `sem_acao_disponivel`; isso é uma classificação por pessoa, enquanto a tabela acima separa os dias. Em particular, col950 mistura dias sem leitura e sem porta: sem ação publicada, o grupo da pessoa é o de sem leitura se houver ao menos um `nao_sei` (`sonda_a.saida.txt:4`, `sonda_a.saida.txt:29`, `app/folha/fora_da_folha.py:94`).

A LU-05 determina o balde “Sem ação disponível” para o fio gravado `ninguem`; não transforma esse rótulo em prova de que a ausência de solução seja aceitável. A tela do retrato exibe o dado gravado e marca datas `Sem porta`, mas não destaca individualmente as datas sem leitura; o balde da pessoa pode, ainda assim, mostrar “Sem leitura (sistema)” (`app/folha/retrato_folha.py:191`, `app/folha/fora_da_folha.py:94`, `app/templates/folha/_retrato_o_que_fazer.html:43`).

**Quinta, 19/11:** a simulação específica pretendida, seus parâmetros, estado dos dados naquele momento e a tela que o admin consultará estão **[não verificado]**. Se for o comando `simular_folha` da cópia, ele calcula linhas e retidos para comparação, mas seu resultado declarado não inclui os fios, baldes ou razões da tarefa; portanto **não há base para afirmar que ele mostrará ao admin essas falhas de porta/leitura** (`app/folha/management/commands/simular_folha.py:7`, `app/folha/management/commands/simular_folha.py:133`, `app/folha/management/commands/simular_folha.py:143`). O retrato pode mostrá-las conforme seus campos gravados, sujeito a um retrato existente e atualizado; a tela não recalcula sua ausência (`app/templates/folha/_retrato_lista.html:17`).

Não atribuí E4C-F2-P2, U163/F4, espinha F3–F9, [nome], ESPURIA-TELA, ESCALA-NAO-DESCREVE ou o RESÍDUO a um grupo apenas pela ordem de construção: os textos citados exigem fatos adicionais, como órfã, papel do par ou célula futura além da competência, que os anexos não demonstram (`app/docs/BACKLOG.md:2326`, `app/docs/BACKLOG.md:2327`, `app/docs/BACKLOG.md:2713`, `app/docs/RELATO.md:2607`).

## 18/11 08:54 · sessao 4b495287bfab · ORDEM-1811-ITEM7 · leitura · origem=Code

**A cerca B2a não bloqueia todo ato sobre uma competência entregue.** Ela só julga escritas de perguntas feitas dentro dos quatro atos cercados; fora deles, a guarda retorna sem consultar a competência. A própria cópia identifica os demais sítios da tabela como B2b ainda aberta. [app/chamados/services/fio.py:1389](app/chamados/services/fio.py#L1389), [app/chamados/services/fio.py:1431](app/chamados/services/fio.py#L1431)

**Limite da leitura:** esta cópia não contém `.git`. Não foi possível executar `git show --stat` ou `git grep`, nem confirmar que seu conteúdo corresponde exatamente ao commit informado; fiz busca textual com `rg` na cópia. A alegação de que o commit está no ar é **[não verificado]**. [nao verificado]

### 1. O que é e quando fica verdadeiro

`competencia_entregue` é uma **função calculada**, não um campo persistido: `folha.export.competencia_entregue(empresa_id, mes, ano, registro=False, *, exportacoes=None)`. Com `registro=False`, devolve bool; com `registro=True`, devolve o registro que satisfaz a condição ou `None`. [app/folha/export.py:556](app/folha/export.py#L556)

A condição literal é: entre as `ExportacaoDominio` da tripla empresa/mês/ano, a versão vigente — a mais recente por `(gerado_em, pk)` — existe **e** tem `invalidada_em is None`. O código não procura uma versão mais antiga não invalidada quando a mais recente está invalidada. [app/folha/export.py:580](app/folha/export.py#L580), [app/folha/models.py:192](app/folha/models.py#L192), [app/folha/models.py:203](app/folha/models.py#L203)

Num primeiro **Gerar TXT bem-sucedido**, sem registro anterior para a tripla, a view chama `ExportacaoDominio.registrar`, que cria a linha; a partir dessa criação, a condição fica verdadeira, desde que a linha continue sendo a vigente e não invalidada. Um clique não basta: a view pode retornar antes, por exemplo, se não houver linhas ou se o layout for inválido. A criação do registro é a evidência usada pelo código; recebimento ou importação pelo Domínio é **[não verificado]**. [app/folha/views.py:272](app/folha/views.py#L272), [app/folha/views.py:297](app/folha/views.py#L297), [app/folha/models.py:173](app/folha/models.py#L173)

### 2–3. Os quatro abridores e a visibilidade da recusa

A recusa ocorre **quando o ato efetivamente tenta alterar uma pergunta abrangida**, não simplesmente porque alguma competência da empresa está entregue. A guarda examina os dias da instância e da linha gravada, exclui conversa conforme cada discriminador e veta se algum dia cai em competência entregue **ou se algum dos dias examinados é `None`**. Para alteração de pergunta existente, ela levanta `EscritaEmCompetenciaEntregue`; o abridor desfaz o savepoint inteiro mesmo se uma exceção intermediária tiver sido engolida. Inserção de pergunta nova não passa por essa guarda do `save`. [app/chamados/services/fio.py:1358](app/chamados/services/fio.py#L1358), [app/chamados/services/fio.py:1402](app/chamados/services/fio.py#L1402), [app/chamados/services/fio.py:1443](app/chamados/services/fio.py#L1443), [app/chamados/models.py:3555](app/chamados/models.py#L3555)

| Abridor | O que deixa de ser gravado e retorno |
|---|---|
| **Retração:** `retratar_com_motivo` | Desfaz a transição do chamado e as demais escritas da unidade, incluindo rejeição da justificativa, resolução de perguntas e ocorrência. Retorna `(False, 'competencia_entregue')`. [app/chamados/reconciliador.py:522](app/chamados/reconciliador.py#L522), [app/chamados/reconciliador.py:553](app/chamados/reconciliador.py#L553), [app/chamados/reconciliador.py:563](app/chamados/reconciliador.py#L563) |
| **Fecho de beco:** `fechar_beco` | Desfaz os resolvers e o fechamento da disputa daquela unidade; relê a disputa e retorna se ela está fechada, normalmente `False` na recusa de uma disputa aberta. [app/chamados/services/becos.py:37](app/chamados/services/becos.py#L37) |
| **Resolver chamado resolvido:** `resolver_perguntas_por_chamado_resolvido` | Desfaz as escritas do resolver e seus efeitos dentro da unidade; quando ele próprio abre a cerca, devolve `{'perguntas_resolvidas': 0, 'disputas_fechadas': 0, 'competencia_entregue': vetadas}`. Se estiver aninhado, a recusa sobe à cerca externa. Há saída própria para `VIA_COMPETENCIA_TRANCADA`, antes da abertura da cerca. [app/chamados/services/disputa_emissao.py:1890](app/chamados/services/disputa_emissao.py#L1890), [app/chamados/services/fio.py:1443](app/chamados/services/fio.py#L1443) |
| **Reata pelo histórico:** ramo HIST de `reatar_pergunta_orfa` | Desfaz juntos o reaponte da pergunta e o resolver chamado em seguida; atualiza a instância e devolve `False`. [app/chamados/reconciliador.py:1690](app/chamados/reconciliador.py#L1690) |

Nos quatro casos, o código **tenta registrar** `LogAuditoria` com ação `retratacao_retida_competencia_entregue`, alvo `ChamadoColaborador`, descrição de ato desfeito e `valor_depois` com `empresas_vetadas`. A mesma lista que a última linha daquela chave não gera outra linha. No beco, tenta para cada chamado pertinente; se não houver chamado na lista nem chamado pai, não há alvo para registrar por esse laço. [app/chamados/reconciliador.py:459](app/chamados/reconciliador.py#L459), [app/chamados/services/becos.py:66](app/chamados/services/becos.py#L66)

A escrita de `evento` é best-effort: falha gera `ERROR` e retorna `None`; porém a **consulta de deduplicação anterior** está fora dessa proteção e pode lançar erro. A existência real de uma linha ou log em produção é **[não verificado]**. [app/core/observ.py:73](app/core/observ.py#L73), [app/core/observ.py:94](app/core/observ.py#L94), [app/chamados/reconciliador.py:489](app/chamados/reconciliador.py#L489)

Quanto à tela, **não encontrei mensagem de tela específica da cerca nesses quatro retornos; apresentação por todos os possíveis chamadores é [não verificado]**. Há saídas explícitas em comandos: `reconciliar_fantasmas` imprime a recusa e a contagem; `reconciliar_geofence` imprime a contagem de recusados. O callback de beco chama a porta sob `engolir`, sem encaminhar seu bool a uma tela; o receiver S143 ignora o retorno normal e registra exceções inesperadas. [app/ponto/management/commands/reconciliar_fantasmas.py:281](app/ponto/management/commands/reconciliar_fantasmas.py#L281), [app/ponto/management/commands/reconciliar_geofence.py:46](app/ponto/management/commands/reconciliar_geofence.py#L46), [app/chamados/signals.py:112](app/chamados/signals.py#L112), [app/chamados/signals.py:229](app/chamados/signals.py#L229)

A **validação humana** tem uma regra própria e uma mensagem própria para o subconjunto que consulta K6; ela não é uma mensagem dos quatro abridores. O caminho de aceite de folga retorna antes dessa guarda. [app/chamados/services/validacao.py:209](app/chamados/services/validacao.py#L209), [app/chamados/services/validacao.py:711](app/chamados/services/validacao.py#L711), [app/chamados/services/materializacao.py:800](app/chamados/services/materializacao.py#L800), [app/templates/chamados/partials/_pergunta_card.html:44](app/templates/chamados/partials/_pergunta_card.html#L44)

A busca textual em `ponto/registro_batida.py` não encontrou `competencia_entregue`, `cerca` ou `vigiar_escrita`: **a porta de batida não consulta diretamente essa cerca**. Isso não demonstra que toda cadeia posterior disparada por uma batida esteja livre de outras recusas. [app/ponto/registro_batida.py:1](app/ponto/registro_batida.py#L1)

### 4. `invalidar`

A assinatura é `invalidar(self, *, motivo, substituida_por=None, por=None)`. Exige motivo não vazio; se já invalidada, retorna a própria instância sem regravar. Caso contrário, grava data, motivo e eventual substituta, tenta um evento `txt_invalidado` e preserva conteúdo e hash. A própria função não verifica permissão de usuário. [app/folha/models.py:143](app/folha/models.py#L143)

Na busca de código de produção, o chamador encontrado é `regerar_txt_dominio`: em `--aplicar`, exige motivo e username existente, cria o novo TXT e invalida o anterior apontando para o novo. O trecho não exige que esse usuário seja staff; quem tem acesso operacional para executar o comando em produção é **[não verificado]**. [app/folha/management/commands/regerar_txt_dominio.py:33](app/folha/management/commands/regerar_txt_dominio.py#L33), [app/folha/management/commands/regerar_txt_dominio.py:116](app/folha/management/commands/regerar_txt_dominio.py#L116)

`invalidar` **marca uma versão**, não grava um bool de competência. Se a versão invalidada for a mais recente e não surgir outra mais recente válida, K6 passa a `False`; se houver uma substituta mais recente não invalidada, continua `True`. Invalidar uma versão antiga não altera por si a resposta sobre a vigente. [app/folha/models.py:143](app/folha/models.py#L143), [app/folha/export.py:588](app/folha/export.py#L588), [app/folha/models.py:203](app/folha/models.py#L203)

### 5. Outros efeitos após o primeiro TXT da competência 11

A partir da criação de um TXT válido para a empresa e competência, **outros leitores podem mudar sua resposta**; quais caminhos serão efetivamente acionados para a competência 11 e quais empresas já têm TXT são **[não verificado]**. [app/folha/export.py:580](app/folha/export.py#L580)

- O preview passa a poder mostrar “Já exportado em … por …”; o comparador de TXT passa a ter uma versão válida contra a qual comparar o cálculo atual. [app/folha/views.py:370](app/folha/views.py#L370), [app/folha/export.py:631](app/folha/export.py#L631)
- O juiz de fechamento de célula combina K6 **ou** holerite publicado; a validação e outras portas que compõem K6 podem recusar nos seus próprios recortes. [app/folha/export.py:597](app/folha/export.py#L597), [app/chamados/services/materializacao.py:776](app/chamados/services/materializacao.py#L776)
- No passivo validado, `_esta_exportada` usa **outro leitor**, `haiku_export`, que escolhe a versão mais recente sem testar `invalidada_em`. Se considerar exportada e houver plantio, chama a porta REGEN-EM-EXPORTADA; a porta exige staff e motivo de pelo menos dez caracteres. Assim, invalidar a última versão pode fazer K6 ficar falso enquanto esse leitor ainda a considera vigente. [app/chamados/services/passivo_validado.py:116](app/chamados/services/passivo_validado.py#L116), [app/folha/haiku_export.py:59](app/folha/haiku_export.py#L59), [app/chamados/services/passivo_validado.py:442](app/chamados/services/passivo_validado.py#L442), [app/ponto/portas/regen_exportada.py:107](app/ponto/portas/regen_exportada.py#L107)
- Recalcular fechamento em exportada não é genericamente proibido: o código registra warning. A guarda temporária explícita na cópia está configurada para **09/2026**, não para 11/2026. [app/ponto/services/fechamento.py:485](app/ponto/services/fechamento.py#L485), [app/ponto/services/fechamento.py:539](app/ponto/services/fechamento.py#L539), [app/ponto/services/fechamento.py:36](app/ponto/services/fechamento.py#L36)
- O retrato de folha tem escritor declarado no comando `lavrar_retrato_folha --apply`; o modelo diz que view e signal não o escrevem. A geração também pode aprovar fechamentos elegíveis antes de registrar o TXT; seus efeitos em cada caso são **[não verificado]**. [app/folha/models.py:285](app/folha/models.py#L285), [app/folha/views.py:248](app/folha/views.py#L248)

**Censo de chamadas diretas de `competencia_entregue` no código Python de produção**, obtido por busca textual na cópia, excluindo testes e migrações; não é um `git grep` e não pretende listar todos os leitores indiretos nem toda menção documental: [nao verificado]

- Composições e apresentação: [app/folha/export.py:618](app/folha/export.py#L618), [app/folha/export.py:655](app/folha/export.py#L655), [app/folha/views.py:374](app/folha/views.py#L374), [app/folha/services/ranking_he.py:89](app/folha/services/ranking_he.py#L89).
- Perguntas e fluxo: [app/chamados/services/materializacao.py:789](app/chamados/services/materializacao.py#L789), [app/chamados/services/vigia_fio.py:805](app/chamados/services/vigia_fio.py#L805), [app/chamados/services/vigia_fio.py:807](app/chamados/services/vigia_fio.py#L807), [app/chamados/services/foto_e4b.py:245](app/chamados/services/foto_e4b.py#L245), [app/chamados/services/tarefa_do_dia.py:485](app/chamados/services/tarefa_do_dia.py#L485), [app/chamados/management/commands/backfill_decisoes_pergunta.py:471](app/chamados/management/commands/backfill_decisoes_pergunta.py#L471), [app/chamados/management/commands/backfill_decisoes_pergunta.py:498](app/chamados/management/commands/backfill_decisoes_pergunta.py#L498).
- Ponto e fechamento: [app/ponto/services/fechamento.py:1537](app/ponto/services/fechamento.py#L1537), [app/ponto/services/esmeril_espelho.py:58](app/ponto/services/esmeril_espelho.py#L58), [app/ponto/services/he_pendente_lavrado.py:223](app/ponto/services/he_pendente_lavrado.py#L223), [app/ponto/management/commands/lavrar_status_competencia.py:43](app/ponto/management/commands/lavrar_status_competencia.py#L43).
- Pautas: [app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124](app/pautas/management/commands/pauta_pago_x_devido_exportada.py#L124), [app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:93](app/pautas/management/commands/pauta_folga_nao_definida_exportada.py#L93).

## 18/11 08:54 · sessao 2ea5f277f5aa · ORDEM-1811-ITEM8 · sonda · origem=Code

A sonda mostra **76 células com `trabalha` diferente da reversão** e **32 chamados de ausência resolvidos via sistema** nos quatro colaboradores. Os quatro fechamentos de novembro estão abertos e têm `derivado_em` às 11:20. A atribuição desses efeitos à cura é compatível com a sequência e com o código, mas a sonda não identifica o chamador exato de cada fechamento. [não verificado: a saída fornecida não tem arquivo:linha acessível para citação]

A seguir, **“sonda”** significa o dado raspado incluído na pergunta. Como ele não foi fornecido como arquivo com linhas numeradas, suas observações recebem a marca **[não verificado]** exigida para alegações sem arquivo:linha. Isso distingue a origem da evidência; não acrescenta uma consulta a produção.

## Vigências e células

**Fatos da sonda.** As mudanças de células abaixo são comparações com a reversão; todas as listadas estavam com origem `gerada`. Os horários das alterações aparecem como 11:19 ou 11:20. [não verificado]

| Colaborador | Vigência atual observada | Células cujo `trabalha` mudou |
|---|---|---|
| **col868** | 1603 ativa desde 21/10, sem fim, âncora 16/11, escala 380; anterior 1377 inativa até 20/10 | **31**, de 21/10 a 20/11: datas ímpares de outubro e pares de novembro `False → True`; as demais `True → False`. |
| **col934** | 1604 ativa desde 07/11, sem fim, âncora 17/11, escala 590; anterior 1531 inativa até 06/11 | **14**, de 07/11 a 20/11: ímpares `False → True`; pares `True → False`. |
| **col1017** | 1605 ativa desde 04/11, sem fim, âncora 16/11, escala 182; anterior 1509 inativa até 03/11 | **17**, de 04/11 a 20/11: pares `False → True`; ímpares `True → False`. |
| **col1028** | 1606 ativa desde 07/11, sem fim, âncora 17/11, escala 264; anterior 1528 inativa até 06/11 | **14**, de 07/11 a 20/11: ímpares `False → True`; pares `True → False`. |

A sonda mostra as **vigências atuais**, mas não fornece a foto anterior delas; portanto, quais campos de cada vigência foram criados ou alterados pela cura permanecem **[não verificado]**. O código fecha vigências anteriores e cria ou atualiza a vigência alvo, conforme o caso; depois chama as rotinas de regeneração e pós-vínculo. [vinculo.py:211](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/vinculo.py:211), [vinculo.py:259](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/vinculo.py:259), [vinculo.py:315](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/vinculo.py:315).

## FechamentoMensal — competência 11

**Fatos da sonda.** Todos aparecem com `status=aberto`, `derivado_em=2026-11-18 11:20` e `previsto_em=2026-11-18 11:20`. [não verificado]

| Colaborador | Abertos / motivos do espelho | `derivado_em` |
|---|---|---|
| col868 | 1 / `espelho_furo` | 18/11 11:20 |
| col934 | 3 / `espelho_cobrado` | 18/11 11:20 |
| col1017 | 0 / nenhum | 18/11 11:20 |
| col1028 | 0 / nenhum | 18/11 11:20 |

**Interpretação.** O carimbo indica uma gravação da derivação na mesma janela da cura; para fechamento aberto, o escritor atribui `timezone.now()` a `derivado_em` ao salvar. A sonda só apresenta o minuto do carimbo, então não prova que cada derivação ocorreu **depois da conclusão integral** da cura, nem identifica individualmente seu chamador. [fechamento.py:1137](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/fechamento.py:1137); **[não verificado]** quanto à ordenação por segundo e ao chamador.

## Chamados e perguntas

**Fatos da sonda.** Há chamados “Ausência / Batida não realizada” com estado `resolvido`, `via_encerramento=sistema`, `respondido_por=None` e resolução às 11:19–11:20: **14** para col868, **5** para col934, **7** para col1017 e **6** para col1028. A sonda também mostra novos chamados abertos para col868 e col934. Não há, na saída apresentada, log individual que identifique qual chamada de código fechou cada um dos 32. [não verificado]

O escritor compatível com esses campos é `ChamadoColaborador.reconciliar()`: ele passa a via `RECONCILIAR` ao núcleo `_gravar_estado`; o catálogo mapeia essa via para `sistema`, e o núcleo só altera `respondido_por` quando `set_user=True`. O cartório tem caminhos que chamam essa reconciliação, inclusive para cobrança em dia sem trabalho. Isso estabelece o **caminho possível no código**, não prova qual ramo executou para cada ID da sonda. [models.py:642](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:642), [motor.py:88](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/catalogo/motor.py:88), [models.py:228](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:228), [cartorio.py:1533](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/ponto/services/cartorio.py:1533).

**Perguntas:** a sonda **não mostra nenhuma pergunta comprovadamente fechada ou retirada após a cura**. As linhas exibem resposta e validação, mas não exibem `via_resolucao`, `materializada_em` nem uma comparação de perguntas antes/depois; assim, `resp=None` e `valid=None` não bastam para concluir que continuam vivas ou que foram retiradas. A única resposta e validação datadas mostradas são da pergunta 51008, em 28/10, antes da cura. [não verificado] O código prevê, por exemplo, retração de pergunta por motivo retratado, preservando a linha com uma via de resolução; não há dado suficiente para afirmar que essa via foi usada aqui. [models.py:2612](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/chamados/models.py:2612).

**Logs de pós-vínculo.** A sonda apresenta os eventos 967122, 967170, 967199 e 967225, respectivamente para os quatro colaboradores, com `sistema:censo_fase_12x36`, motivo `executar_vinculo` e `reconciliados=0` em todos; os pares `furos_antes → furos_depois` são **0→14, 20→13, 14→0 e 10→0**. [não verificado] Esses números são somas de **marcos furados**, calculadas no pós-vínculo, e o contador `reconciliados` dessa rotina não contabiliza fechamentos ocorridos nos dias já julgados pela regeneração; portanto, zero nesse log não contradiz os chamados resolvidos observados. [pos_vinculo.py:61](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/pos_vinculo.py:61), [pos_vinculo.py:79](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/colaboradores/services/pos_vinculo.py:79).

## 18/11 08:54 · sessao 8ac634ce6422 · ORDEM-1811-ITEM7 · leitura · origem=Code

**Conclusão:** as quatro linhas `sem_leitura_sistema` revelam uma falha de leitura atribuída à estrutura pelo próprio sistema; isso não determina, por si só, o dono L-099 da divergência subjacente. Nas 17 linhas `sem_acao_disponivel`, o balde informa que não há ação alcançável publicada, mas não decide se o problema subjacente é ESTRUTURA, CADASTRO ou BATIDA. A divisão do painel não tem mapeamento automático para os donos da L-099. ([fora_da_folha.py:39](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:39), [RELATO.md:308](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:308), [LEIS.md:122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:122))

**Origem dos rótulos.** `DONO_DO_FIO` mapeia `ninguem` para `sem_acao_disponivel` e `nao_sei` para `sem_leitura_sistema`; `fio_da_linha` lê o campo `dono` gravado, e `dono_da_linha` aplica o mapa, com exceção para `ninguem` quando todos os dias são de hoje em diante. ([retrato_folha.py:177](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:177), [retrato_folha.py:194](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:194), [retrato_folha.py:436](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:436), [retrato_folha.py:468](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:468))

- **`ninguem` / `nada`:** a tarefa não aponta uma ação alcançável para o dia; a ausência de porta ou rota não prova, isoladamente, a causa dessa ausência. ([fora_da_folha.py:54](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:54), [fora_da_folha.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:76))
- **`nao_sei`:** a porta veio sem rota e marcada `nao_sei`; o sistema descreve isso como leitura do dia a ser restabelecida pela estrutura. ([fora_da_folha.py:56](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:56), [fora_da_folha.py:86](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:86))

Na agregação por pessoa, uma ação publicada prevalece; sem ação, basta um dia `nao_sei` para o grupo ser E, enquanto D exige todos os fios `nada`. Assim, `col950` pode ter dias dos dois tipos e aparecer em `sem_leitura_sistema`. ([fora_da_folha.py:94](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:94), dado raspado fornecido pelo usuário — sem arquivo:linha, **[não verificado]**)

**As 21 linhas.** Os valores de pessoa, balde, motivo, tipo, saldo e dias abaixo transcrevem o dado raspado fornecido; não há arquivo:linha da sonda disponível para conferi-los nesta resposta, portanto essa transcrição é **[não verificada]**. Em todas, `motivo=furo_espelho`. As causas expressam o que o fio permite concluir pelo código, não uma causa raiz demonstrada. ([fora_da_folha.py:76](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:76))

| Colaborador | Balde | Linha: tipo / saldo / dias | Causa sustentada pelo dado e código | Obra que cobre esta linha |
|---|---|---|---|---|
| col950 | sem_leitura_sistema | sem_causa / 6mais / 11 | Seis dias sem leitura e cinco sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col1009 | sem_acao_disponivel | falta / 2a5 / 4 | Quatro dias sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col639 | sem_acao_disponivel | sem_causa / 2a5 / 5 | Cinco dias sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col496 | sem_acao_disponivel | falta / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col265 | sem_leitura_sistema | sem_causa / 1 / 1 | Um dia sem leitura; causa raiz **[não verificado]**. | **[não verificado]** |
| col994 | sem_acao_disponivel | folga / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col1048 | sem_acao_disponivel | nao_bate / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col192 | sem_leitura_sistema | falta / 1 / 1 | Um dia sem leitura; causa raiz **[não verificado]**. | **[não verificado]** |
| col327 | sem_acao_disponivel | sem_causa / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col558 | sem_acao_disponivel | falta / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col97 | sem_acao_disponivel | sem_causa / 6mais / 9 | Nove dias sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col347 | sem_acao_disponivel | sem_causa / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col407 | sem_leitura_sistema | falta / 2a5 / 3 | Três dias sem leitura; causa raiz **[não verificado]**. | **[não verificado]** |
| col504 | sem_acao_disponivel | sem_causa / 2a5 / 2 | Dois dias sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col719 | sem_acao_disponivel | sem_causa / 2a5 / 3 | Três dias sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col476 | sem_acao_disponivel | folga / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col348 | sem_acao_disponivel | nao_bate / 2a5 / 5 | Cinco dias sem porta; causa raiz para essas datas **[não verificado]**. | **[não verificado]** |
| col246 | sem_acao_disponivel | sem_causa / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col905 | sem_acao_disponivel | falta / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col61 | sem_acao_disponivel | folga / 1 / 1 | Um dia sem porta; causa raiz **[não verificado]**. | **[não verificado]** |
| col177 | sem_acao_disponivel | sem_causa / 1 / 1 | Um dia sem porta; há evidência histórica específica abaixo, mas seu estado na lavra de 18/11 é **[não verificado]**. | **[não verificado]** |

Para **col177 em 08/11**, o RELATO já havia registrado esse mesmo dia como `trabalha=True`, `fato_em_ausencia`, sem pergunta nem chamado, dono ESTRUTURA, com encaminhamento na E3 c8b; o dado raspado atual não mostra esses campos nem comprova que permaneciam iguais na lavra de 18/11, tampouco demonstra qual das obras oferecidas cobre a linha atual. ([RELATO.md:1906](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:1906); ligação com o estado atual **[não verificado]**)

A escolha da obra exige o fato que define seu alcance: por exemplo, a obra de dia sem trabalho cobre uma porta de decisão para certas batidas em folga ou feriado, e o RELATO também registra uma classe de folgas sem batida que seguravam a folha; o tipo agregado `folga` da tabela não demonstra qual desses fatos ocorreu. ([BACKLOG.md:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1968), [RELATO.md:2585](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:2585); aplicação individual **[não verificado]**)

Os escopos registrados de [nome], ESCALA-NAO-DESCREVE, ESPURIA-TELA, U163/F4 e FUTURO-RESIDUO dependem, respectivamente, de fatos sobre o casador e o papel do par, órfã e escala, apresentação da tela, perguntas U163 em competência entregue, ou células futuras além da competência; esses predicados não constam da sonda por linha. Os briefs `logs/ESPINHA-BRIEFS` não foram encontrados na cópia examinada, de modo que o alcance completo das fatias F3–F9 fica **[não verificado]**. ([BACKLOG.md:2326](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2326), [BACKLOG.md:2327](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2327), [BACKLOG.md:1970](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1970), [RELATO.md:370](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:370), [BACKLOG.md:2713](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2713); ausência e alcance dos briefs **[não verificado]**)

**Para a simulação de quinta:** se ela abrir exatamente as mesmas linhas gravadas, todas as datas fornecidas são anteriores a 19/11, de modo que a exceção que muda `ninguem` para `hoje` não as deslocaria por essa regra. O retrato efetivamente aberto ou relavrado na quinta é **[não verificado]**. ([retrato_folha.py:489](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/retrato_folha.py:489); datas no dado fornecido **[não verificado]**)

Para **col950, col265, col192 e col407**, a resposta do sistema é que há leitura a restabelecer pela estrutura; o dono do fato que originou cada retenção segue **[não verificado]**. Para as **17 linhas sem ação**, inclusive os tipos `folga`, `falta` e `nao_bate`, a escolha entre falha estrutural e resposta correta a CADASTRO/BATIDA segue **[não verificado]** por pessoa e dia: a L-099 exige essa separação, e o próprio registro do painel diz que seus baldes não fornecem o mapeamento. ([fora_da_folha.py:44](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/folha/fora_da_folha.py:44), [LEIS.md:122](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/LEIS.md:122), [RELATO.md:308](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:308))

## 18/11 08:53 · sessao 7f39d5b24237 · ORDEM-1811-ITEM7 · leitura · origem=Code

A lista fornecida tem **33 dias e 23 colaboradores distintos**, identificados pelo par empresa–colaborador; o RELATO registra 33 dias em 10 colaboradores. As seis partes somam 26 dias, sete a menos que a lista. A causa concreta da diferença entre as execuções é **[não verificado]**. [Anexo `s.r`, arquivo e linhas de origem não verificados]; [RELATO.md:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:14); [anexo `r.md`, linhas não verificadas].

| Contagem | Resultado |
|---|---:|
| Total | 33 dias |
| `e6_zero_esp_trabalho` | 12 |
| `diverge_acima_60` | 11 |
| `diverge_acima_60/FORA` | 1 |
| `diverge_10_60` | 8 |
| `esp_zero_e6_trabalho` | 1 |
| Empresa 1 | 1 dia, 1 colaborador |
| Empresa 2 | 20 dias, 14 colaboradores |
| Empresa 3 | 11 dias, 7 colaboradores |
| Empresa 4 | 1 dia, 1 colaborador |

A distribuição por classe confere com os quatro totais do RELATO quando o `/FORA` é contado separadamente: o RELATO informa 11 na classe acima de 60 e não inclui essa linha em seu subtotal de classes, que soma 32. [RELATO.md:14](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/RELATO.md:14); [anexo `s.r`, linhas não verificadas].

Na tabela, as descrições são **sinais observados**, não diagnósticos: o CSV não traz os horários, a escala, a ata ou a trilha necessários para estabelecer o mecanismo de cada divergência. Por isso, **“sem obra” significa que não foi possível comprovar a cobertura daquele dia**, e a causa provável permanece **[não verificado]** em todas as linhas. Os dados de cada linha vêm do anexo `s.r`, cuja origem e numeração de linhas são **[não verificado]**.

| emp\|colaborador | Data | Classe | Sinal observado; causa provável | Obra que cobre | Declaração de cobertura |
|---|---|---|---|---|---|
| 3\|col110 | 2026-11-12 | `e6_zero_esp_trabalho` | Espelho 241 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col113 | 2026-10-30 | `e6_zero_esp_trabalho` | Espelho 242 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col113 | 2026-11-01 | `e6_zero_esp_trabalho` | Espelho 241 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col200 | 2026-10-29 | `e6_zero_esp_trabalho` | Espelho 180 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col200 | 2026-11-02 | `e6_zero_esp_trabalho` | Espelho 180 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col218 | 2026-11-14 | `e6_zero_esp_trabalho` | Espelho 103 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col296 | 2026-11-02 | `e6_zero_esp_trabalho` | Espelho 345 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 4\|col37 | 2026-11-14 | `e6_zero_esp_trabalho` | Espelho 241 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col382 | 2026-11-10 | `e6_zero_esp_trabalho` | Espelho 418 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col516 | 2026-10-23 | `e6_zero_esp_trabalho` | Espelho 361 min, oráculo zero, sem batidas e veredito de folga; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col727 | 2026-11-10 | `e6_zero_esp_trabalho` | Espelho 307 min, oráculo zero, sem batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col841 | 2026-11-16 | `e6_zero_esp_trabalho` | Espelho 178 min, oráculo zero, sem batidas e veredito de folga; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col114 | 2026-11-08 | `diverge_10_60` | Espelho 439 min e faixa do oráculo 429–499 min; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col114 | 2026-11-15 | `diverge_10_60` | Espelho 499 min, faixa 440–499 min e veredito de feriado; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col252 | 2026-11-09 | `diverge_10_60` | Espelho 551 min e faixa 597–610 min; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col252 | 2026-11-10 | `diverge_10_60` | Espelho 551 min e faixa 598–611 min; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col252 | 2026-11-11 | `diverge_10_60` | Espelho 550 min e faixa 598–611 min; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col252 | 2026-11-13 | `diverge_10_60` | Espelho 551 min e faixa 597–611 min; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col252 | 2026-11-17 | `diverge_10_60` | Espelho 553 min e faixa 598–612 min; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col884 | 2026-11-08 | `diverge_10_60` | Espelho 679 min, faixa 699–739 min e veredito rejeitado; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col112 | 2026-10-21 | `diverge_acima_60` | Espelho 546 min e oráculo 838 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col112 | 2026-10-22 | `diverge_acima_60` | Espelho 541 min e oráculo 835 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col130 | 2026-10-29 | `diverge_acima_60` | Espelho 335 min e faixa 672–730 min, com quatro batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col220 | 2026-11-10 | `diverge_acima_60` | Espelho 337 min e faixa 430–486 min, com quatro batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col334 | 2026-11-16 | `diverge_acima_60` | Espelho 209 min, faixa 591–683 min e veredito de folga; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col874 | 2026-10-22 | `diverge_acima_60` | Espelho 667 min e oráculo 383 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col909 | 2026-11-11 | `diverge_acima_60/FORA` | Espelho 831 min, faixa 245–374 min e paridade ímpar no corte do oráculo; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col922 | 2026-11-01 | `diverge_acima_60` | Espelho 1.382 min e oráculo 68 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col922 | 2026-11-04 | `diverge_acima_60` | Espelho 676 min e oráculo 458 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 1\|col950 | 2026-11-11 | `diverge_acima_60` | Espelho 480 min e oráculo 240 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 2\|col989 | 2026-10-23 | `diverge_acima_60` | Espelho 721 min e oráculo 567 min, com duas batidas; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col996 | 2026-10-28 | `esp_zero_e6_trabalho` | Espelho zero, oráculo 580 min e veredito de folga; causa [não verificado]. | sem obra | [não verificado] |
| 3\|col996 | 2026-11-10 | `diverge_acima_60` | Espelho 443 min e faixa 1.026–1.116 min, com seis batidas; causa [não verificado]. | sem obra | [não verificado] |

As declarações disponíveis descrevem mecanismos que *podem* ser investigados, mas não ligam os sinais acima a cada dia: [nome] cobre o casador que desfaz o papel do par; DIA-SEM-TRABALHO-PORTA trata, entre outros casos, batida em folga ou feriado; MOTOR-LE-O-TURNO trata leituras de par e dia. A presença de veredito de folga ou de várias batidas, isoladamente, não demonstra esses mecanismos. [BACKLOG.md:2326](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2326); [BACKLOG.md:1968](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:1968); [BACKLOG.md:2043](/home/ronald/codex_ws/dd489c627bb0a8d5f8782945c7e240e6621c98ed/app/docs/BACKLOG.md:2043). Os diretórios `logs/ESPINHA-BRIEFS` e `logs/AVAIS-1515` não estavam disponíveis nos caminhos consultados; seu conteúdo e eventual vínculo nominal com os dias ficam **[não verificado]**.

**Dias sem obra comprovada:** todos os 33 listados na tabela. Isso não prova que inexista cobertura; significa que a correspondência dia → causa → obra não pôde ser demonstrada com o material disponível **[não verificado]**.

_6814 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
