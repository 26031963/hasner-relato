# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 18:23** · publicado em **05/11/2026 18:23** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 18:23 · sessao 3259fd454150 · - · - · origem=Ronald (tmux)

Conferência da seção A, somente leitura:

| Coincidência | Correspondência | Sítios conferidos |
|---|---|---|
| Decisão humana preservada contra instância antiga | pc1 A5 = pc2 A1 | `app/docs/PROMPTS.md:1882`; comentário da corrida e releitura no inverso em `app/chamados/services/relavra_via.py:214`, trava em `:221`. |
| Resposta nova invalida assinatura anterior | pc1 A6/A7 = pc2 A2/A3 | `app/chamados/services/disputa_emissao.py:1287`; limpeza condicional em `:1286`, `:1295`; save em `:1301`. |
| Repetição/idempotência | pc1 A8 = pc2 A4 | Regra em `CLAUDE.md:175`; carimbo comum sempre toma novo instante em `app/chamados/models.py:2570`; contraste com guarda administrativa em `:2502`. |
| Pergunta viva recusa carimbo | pc1 A1 = pc2 A5 | `CLAUDE.md:167`; `CLAUDE.md:170`; porta julga candidato em `app/chamados/models.py:2585`, recusa em `:2589`. |
| Marco resolvido mesmo com outro furo | pc1 A2 = pc2 A6 | `app/chamados/models.py:2257`; C6 em `app/chamados/juizes.py:1173`, condição em `:1177`. |
| Competência 21→20 | pc1 A9 = pc2 A7 | Janela em `app/ponto/janelas.py:10`, mudança de competência em `:27`, corte cadastrado em `:60`; tranca sem absolvição em `app/chamados/juizes.py:1142`. |
| Cross-meia-noite | pc1 A10 = pc2 A8 | Regra em `app/docs/LEIS.md:117`; teto em `CLAUDE.md:185`; encadeamento concreto em `app/escala/servico_jornada.py:80`, avanço D+1 em `:82`. |
| Provisório e chegada de batida | pc1 A11 = pc2 A9 | `app/docs/CORTES.md:413`; C6 em `app/chamados/juizes.py:1190`, condição em `:1195`. |
| Geradora histórica | pc1 A12 = pc2 A10, com ressalva abaixo | `app/escala/alimentacao.py:231`; `CLAUDE.md:146`. |
| Ausência parcial cobra resto | pc1 A15 = pc2 A13 | `app/chamados/juizes.py:403`, predicado em `:410`, consumo em `:426`; `app/docs/JUIZES.md:58`. |
| Resposta concordante/discordante | pc1 A3/A4 = pc2 A14 | `app/docs/PROMPTS.md:1442`, especificação adicional em `:1445`; concordância em `app/chamados/juizes.py:1090`, incomparável em `:1096`, consumo em `:1162`. |
| Erro restaura memória | pc1 A17 = pc2 A16 | `app/chamados/models.py:2586`, `:2599`; testes em `app/chamados/tests/test_o261e_f2_fila.py:179`, `:190`, `:200`. |
| Antiguidade não absolve | pc1 A18 = parte de pc2 A17 | Corte explícito sobre idade em `app/docs/PROMPTS.md:1317`. |

Divergências relevantes:

- **DESENHO — universo da ata vazia:** pc1 A14 deixa o resultado normativo de `trabalha=True`, veredito não acusador e `ata={}` como `[nao verificado]`; pc2 A11 exige desconhecimento e rejeita conclusão. Código mostra divergência entre autoridades: `_celula_furada` diz “ata nunca lavrada” e retorna `None` sem `lampadas` (`app/chamados/juizes.py:473`, `:477`); C6 retorna `False` quando `marcos_pendentes` é vazio (`:1195`), função que lê `lampadas` com default vazio (`app/ponto/services/lastro.py:148`). L-061 manda ler a célula/ata (`app/docs/LEIS.md:95`), mas não explicita isoladamente a solução dessa combinação contraditória. Não resolver por maioria ou pelo comportamento atual: L-110 em `app/docs/LEIS.md:136`.
- **DESENHO por omissão de fronteira — geradora fora da vigência:** pc1 A12 diz respeitar geradora/DNA; pc2 A10 explicita que geradora fora da vigência produz desconhecimento/CADASTRO. A função confere a vigência da geradora e devolve `None`, sem escolher B (`app/escala/alimentacao.py:272`, `:282`). Os textos são compatíveis dentro da vigência; a formulação irrestrita de pc1 não cobre essa fronteira.
- **Detalhe — resposta concordante e discordante:** pc1 separa em A3/A4; pc2 agrupa em A14. Mesma regra, sem mudança de juiz (`app/chamados/juizes.py:1087`, `:1162`).
- **Detalhe — concorrência/repetição:** pc1 A8 reúne duas tentativas e repetição; pc2 A4 enfatiza repetição sem renovar instante. A exigência de mesmo estado está em `CLAUDE.md:175`; a guarda do carimbo comum continua ausente no corpo em `app/chamados/models.py:2570`.

Só numa, na seção A:

- **pc1 A13:** variante explícita `trabalha=None` versus `False`. Corte direto em `app/docs/CORTES.md:416`; implementação em `app/chamados/juizes.py:418`, `:428`.
- **pc1 A16:** disputa encerrada por desligamento/admin não equivale a decisão do dia; resposta continua para Validar. Corte específico em `app/docs/CORTES.md:342`; C6 procura decisão na célula em `app/chamados/juizes.py:1148`.
- **pc2 A12:** ensaio explícito trancar/destrancar; pc1 aborda parte desse risco no contraexemplo D8. Leitura sem escrita e devolução da vitalidade estão documentadas em `app/chamados/juizes.py:919`, `:924`.
- **pc2 A15:** identidade com resposta pendente; precedência explicitada em `app/chamados/models.py:2617`, `:2619`; resposta antecede identidade no C6 em `app/chamados/juizes.py:1150`, `:1171`.
- **pc2 A17:** falta de credor junto com antiguidade; pc1 A18 só explicita idade. Fecho sem credor não pode carimbar/reabrir pergunta viva em `app/docs/PROMPTS.md:1777`.
- **pc2 A10:** fronteira da geradora fora da vigência, ausente de pc1 A12, conforme `app/escala/alimentacao.py:272`, `:282`.

Citações falsas da seção A: não identifiquei referência inequivocamente inexistente ou apontando para outro assunto entre os sítios conferidos. Ressalva: `app/docs/LEIS.md:95` sustenta a obrigação de ler a lâmpada, mas não demonstra sozinha o resultado normativo mais forte afirmado por pc2 A11; o comentário e retorno citados pertencem a `_celula_furada`, enquanto o sítio que o diff reutiliza é C6 (`app/chamados/juizes.py:473`, `:1135`, `:1195`).

## 05/11 18:23 · sessao 6a17109f59c4 · - · - · origem=Ronald (tmux)

- **DESENHO — cobertura:** PC1 rebaixa todas as facetas Python cobertas quando existe qualquer shell; PC2 exige coincidência lexical de componente do módulo. Isso muda quais facetas recebem contagem e quais recebem “nao varrido”; o código atual decide isso pelo tipo em `_faceta`. (`bin/quem_chama.py:691`, `bin/quem_chama.py:697`; propostas C fornecidas: [nao verificado] como arquivos locais)

- **DESENHO — diagnóstico dos símbolos:** PC1 aplica seu rebaixamento depois de `_tipo_simbolo`; PC2 altera o tipo do arquivo antes. Em PC2, `_tipo_simbolo` devolve imediatamente o tipo alterado e perde a distinção entre método, setting, registro dinâmico e decorador. (`bin/quem_chama.py:313`, `bin/quem_chama.py:315`, `bin/quem_chama.py:319`, `bin/quem_chama.py:321`, `bin/quem_chama.py:323`)

- **DETALHE — teste de faceta:** PC1 usa `awk` que aceita a faceta arquivo contada ou não varrida; PC2 exige `faceta.startswith('arquivo [')`, aceitando somente o formato contado. As negativas atuais identificam apenas texto da referência, sem seu contexto de faceta. (`bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`; formatos: `bin/quem_chama.py:692`, `bin/quem_chama.py:697`)

- **DETALHE — linhas após comentário:** PC1 acrescenta uma referência válida posterior ao comentário multilinha e exige sua linha preservada; PC2 verifica referência anterior e ausência dentro do comentário. A implementação preserva quebras de linha, mas a fixture atual usa comentários fechados na mesma linha. (`bin/quem_chama.py:521`, `bin/quem_chama.py:523`, `bin/tests/test_quem_chama.sh:61`)

- **Lacuna no selo PC2:** os três módulos acrescentados compartilham o componente `ilha`; pelo predicado proposto, uma única ocorrência desse componente rebaixa os três. As três asserções não demonstram independentemente o reconhecimento de cada forma shell. (`bin/quem_chama.py:347`, `bin/quem_chama.py:638`; predicado e fixture C fornecidos: [nao verificado] como arquivos locais)

- **Lacuna comum:** ambos descrevem shell sem extensão, mas os acréscimos de fixture usam somente nomes terminados em `.sh`; o código possui ramo independente para shebang sem extensão. (`bin/quem_chama.py:608`, `bin/quem_chama.py:612`; acréscimos C fornecidos: [nao verificado] como arquivos locais)

- **Citação que não sustenta isolamento/N=1:** PC2 A cita `bin/tests/test_quem_chama.sh:51` para uma fixture com somente importação. Essa linha cria também `Y = mudada()`, portanto contém dois sítios de referência; não comprova a fixture isolada descrita. (`bin/tests/test_quem_chama.sh:51`)

## 05/11 18:23 · sessao a051a99e8d29 · - · - · origem=Ronald (tmux)

- Sítios externos sustentam os casos: chamada em `manage.py shell --command` (`bin/placar_code.sh:384`); importação pelo pacote com apelido (`bin/placar_code.sh:484`, `bin/placar_code.sh:508`); heredoc com importação pelo pacote (`bin/gate_cobranca.sh:47`, `bin/gate_cobranca.sh:50`); régua chama os selos existentes (`bin/regua.sh:138`, `bin/regua.sh:140`).

- PC2 acrescenta exemplos concretos sustentados: carregamento do CSS (`app/templates/base.html:17`) e registro do filtro `espera` (`app/chamados/templatetags/chamados_tags.py:53`). Os números históricos **39, 12, 3, 13 e 15** não estão demonstrados pela sonda reproduzida nesta pergunta; permanecem **[nao verificado]**. Isso não demonstra citação falsa dos sítios, mas ausência da evidência quantitativa.

- A distinção entre censo e julgamento de domínio está correta: instrumento declara “lista é fato, não juiz” (`bin/quem_chama.py:6`); registro define autoridades de decisão (`app/core/juizes.py:3`, `app/core/juizes.py:7`); lookup documenta janela de folha, dia sem trabalho e folga do vínculo (`app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:55`, `app/docs/JUIZES.md:90`). A L-111 exige zero chamador **de produção** (`app/docs/LEIS.md:137`), enquanto o scanner admite qualquer `.py`, inclusive testes (`bin/quem_chama.py:606`, `bin/quem_chama.py:672`).

- **DESENHO:** PC1 rebaixa facetas Python antes cobertas quando existir qualquer shell; PC2 exige coincidência lexical com componente do módulo. O código atual reconhece shell por extensão ou shebang (`bin/quem_chama.py:608`, `bin/quem_chama.py:612`), mas não propaga exposição shell à cobertura: monta os tipos antes do despacho (`bin/quem_chama.py:637`, `bin/quem_chama.py:656`) e shell produz apenas achados da faceta arquivo (`bin/quem_chama.py:512`, `bin/quem_chama.py:674`). O universo elegível para N difere entre propostas porque `_faceta` decide pela presença do tipo em `VARRIDOS` (`bin/quem_chama.py:691`).

- **Detalhe diagnóstico:** PC1 aplica seu rebaixamento depois de `_tipo_simbolo`; PC2 altera o tipo do arquivo antes. Em PC2, o retorno precoce propagaria o rótulo genérico shell também a métodos/settings/convenções/decoradores, apagando seus motivos específicos; PC1 conservaria esses motivos já fora de `VARRIDOS` (`bin/quem_chama.py:313`, `bin/quem_chama.py:315`, `bin/quem_chama.py:317`, `bin/quem_chama.py:319`, `bin/quem_chama.py:323`).

- Exclusivos PC1 confirmados: função sob condicional pode faltar, pois o inventário só recursa em classes (`bin/quem_chama.py:264`, `bin/quem_chama.py:266`, `bin/quem_chama.py:277`); configuração recebe só busca textual (`bin/quem_chama.py:610`, `bin/quem_chama.py:678`); nome de arquivo com quebra de linha é excluído da leitura e depois ignorado (`bin/quem_chama.py:90`, `bin/quem_chama.py:668`).

- Exclusivos PC2 confirmados: processamento de templates linha a linha deixa sintaxe repartida sem cobertura (`bin/quem_chama.py:528`, `bin/quem_chama.py:531`, `bin/quem_chama.py:541`, `bin/quem_chama.py:562`); nomes calculados e reexports são exclusões expressas (`bin/quem_chama.py:65`, `bin/quem_chama.py:66`); range/revisão inválida vira `RuntimeError`, capturado como rc=2 (`bin/quem_chama.py:119`, `bin/quem_chama.py:120`, `bin/quem_chama.py:734`, `bin/quem_chama.py:736`).

- Contraexemplos coincidentes confirmados: AST ilegível devolve ausência de achados sem reclassificação (`bin/quem_chama.py:191`, `bin/quem_chama.py:393`); escrita concorrente usa abertura direta em `w` (`bin/quem_chama.py:742`); conteúdo consultado vem das revisões resolvidas (`bin/quem_chama.py:120`, `bin/quem_chama.py:624`).

- Não localizados os arquivos `avais.txt`, `otimiza3_reprovacoes.txt` e `pd_otimiza3.r`; suas alegações originais e respectivas linhas ficam **[nao verificado]**. Não classificar automaticamente como citação falsa.

- Não identifiquei citação externa demonstravelmente falsa entre os sítios abertos. A formulação PC2 de que `JUIZES.md` é lookup “desse registro” é mais forte que a referência isolada: a documentação afirma ser lookup de autoridades e declara origem em `CLAUDE.md`, sem afirmar sincronização com `core/juizes.py` (`app/docs/JUIZES.md:3`, `app/docs/JUIZES.md:7`).

## 05/11 18:22 · sessao dac32951643e · - · - · origem=Ronald (tmux)

| Insumo novo do snapshot | Fontes/escritores | Leitores principais |
|---|---|---|
| `motivo` | Nascimento pela porta `abrir`: `app/chamados/models.py:2465`; catálogo fornece motivo/tipo: `app/chamados/catalogo/perguntas.py:36`; emissor passa o motivo: `app/chamados/services/disputa_emissao.py:391`, `:772`. | Identidade do fato: `app/chamados/models.py:2381`; plantio: `app/chamados/services/materializacao.py:305`; derivação da via: `app/chamados/services/relavra_via.py:101`; elegibilidade sem turno: `app/chamados/reconciliador.py:362`. |
| `tipo_resposta` | Nascimento por `**campos`: `app/chamados/models.py:2466`; emissor entrega tipo do catálogo: `app/chamados/services/disputa_emissao.py:393`; emissão de hora: `:774`. | Validação da resposta: `app/chamados/services/disputa_emissao.py:1257`, `:1263`, `:1272`; ramos do plantio: `app/chamados/services/materializacao.py:1143`; lastro: `app/chamados/juizes.py:672`, `:674`; validação humana: `app/chamados/services/validacao.py:152`. |
| `data_evento` | Nascimento: `app/chamados/models.py:2465`; conversa recebe data local em `:2416`; resolvedora preenche apenas data ausente: `app/chamados/services/fio.py:832`, `:834`, persistida `:847`. | Identidade: `app/chamados/models.py:2382`; data do fato: `app/chamados/services/fio.py:87`; cruzamento de meia-noite: `app/chamados/juizes.py:263`, `:285`; janela de C6: `:1197`; materialização: `app/chamados/services/materializacao.py:345`. |
| `celula_id` | Nascimento resolve célula da pessoa/data: `app/chamados/models.py:2419`, `:2466`; reaproveitamento pode trocar célula: `:2425`, persistido `:2463`; FK tem `SET_NULL` ao excluir célula: `:2198`. Atribuição `:1439` é cópia para julgamento. | C6: `app/chamados/juizes.py:1147`, `:1181`; memo: `:815`; validação de pessoa/dia da célula: `:621`, `:625`; seleção de reconciliação: `app/chamados/reconciliador.py:585`. |
| `disputa_id` | Nascimento: `app/chamados/models.py:2465`; FK obrigatória e exclusão em cascata: `:2200`. Não localizei reatribuição persistida de disputa nas portas examinadas; existência de outra forma dinâmica [nao verificado]. | Fonte da pessoa para C6: `app/chamados/juizes.py:1200`; identidade de fato: `app/chamados/models.py:2381`; âncora: `app/chamados/services/fio.py:116`; plantio: `app/chamados/services/materializacao.py:514`; fila aberta: `app/chamados/services/disputa_emissao.py:1575`. |
| `chamado_destino_id` | Nascimento: `app/chamados/models.py:2466`; dedup reaponta em `:2439`, persistido `:2463`; porta `reapontar_resolvedora`: `app/chamados/services/fio.py:845`, persistido `:847`. Mutação temporária para consultar C6: `:773`, restaurada `:777`. | Âncora/listagem: `app/chamados/services/fio.py:117`; data divergente: `:80`; identidade da disputa: `:135`; plantio: `app/chamados/services/materializacao.py:761`; via automática: `:1185`; C6/memo: `app/chamados/juizes.py:817`; resolução de perguntas: `app/chamados/services/disputa_emissao.py:1573`. |

**Conferência de data não altera `data_evento`.** `conferir_data_do_fato` grava exclusivamente `data_conferida_em/por`; a data permanece a do fato já armazenada. `app/chamados/services/validacao.py:617`, `:619`.

**Lacuna a declarar em D:** incluir esses seis campos não detecta uma conferência humana intercalada que altere somente `data_conferida_em/por`; o materializador consulta essa autorização ao tratar divergência. `app/chamados/services/validacao.py:617`; `app/chamados/services/materializacao.py:1111`.

## 05/11 18:20 · sessao 9d38342fb640 · - · - · origem=Ronald (tmux)

- **Defeito confirmado:** shell passa por `varrer_linhas`, que só produz achados da faceta arquivo (`q=None`); `bin/placar_code.sh:384` importa `datas_cravadas_prod`, mas o símbolo permanece sem achado. A cura deve declarar a cobertura desconhecida, preservando achados avulsos. (`bin/quem_chama.py:510`, `bin/quem_chama.py:673`, `bin/quem_chama.py:690`)

- **Condição proposta pelo pai:** procurar lexicalmente o último componente do módulo nas linhas de shell cobre heredoc, `python3 -c`, `manage.py shell --command`, `from pacote import modulo` e pacote raiz. Os comentários shell já são retirados; os símbolos herdam o tipo desconhecido pelo classificador existente. (`bin/quem_chama.py:479`, `bin/quem_chama.py:124`, `bin/quem_chama.py:311`)

- **Borda restante dessa condição:** alvo `bin/foo.py`, leitor shell `python3 -mfoo` pode continuar como zero falso: o token `foo` está precedido por `m`, impedindo `_padrao_nome`. A execução de Python com `-m` colado foi confirmada com módulo inexistente, sem arquivos; esta confirmação não tem arquivo:linha. (`bin/quem_chama.py:347`; execução [nao verificado])

- **Preservação indispensável:** mudar o tipo de um templatetag para desconhecido não pode apagar sua identidade de biblioteca. Hoje `lib` depende de `tipo == 'py-templatetag'`; sem preservar o tipo estrutural, somem os achados de `{% load %}`, filtro e tag. (`bin/quem_chama.py:657`, `bin/quem_chama.py:373`, `bin/quem_chama.py:549`)

- **Outro erro de fonte comprovado:** `{% load m_tags %}` seguido apenas de `{% filter rot %}` registra o arquivo, mas não o filtro. A menor alteração é trocar `r'\|\s*'` por `r'(?:\|\s*|{%\s*filter\s+)'` no ramo de filtro. Os filtros seguintes do bloco já são alcançados pelo `|`. (`bin/quem_chama.py:559`, `bin/quem_chama.py:560`)

- **Comentários multilinha:** `{# ... #}` e `{% comment %}...{% endcomment %}` já são removidos preservando linhas; a lacuna demonstrada é de teste, não de fonte. (`bin/quem_chama.py:520`, `bin/tests/test_quem_chama.sh:61`)

- **Reprovação do selo procede:** os testes de símbolo parado e relativo parado procuram o texto em toda a saída; um mutante pode emitir `importa de m.lib` dentro da faceta de símbolo e passar. `_tem_sec` também só isola arquivo. Precisa isolar **arquivo + faceta**, exigir presença em `arquivo` e ausência em cada símbolo mudado. (`bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/tests/test_quem_chama.sh:210`)

- **Fixture sem `__init__`:** a fixture já não cria esses arquivos, mas `lib.py` contém usos próprios que podem mascarar falha de import externo. Acrescentar um alvo com símbolo sem uso local e um único chamador relativo permite provar a faceta isoladamente. (`bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:32`, `bin/tests/test_quem_chama.sh:55`, `bin/tests/test_quem_chama.sh:121`)

- **Limitação independente para D:** erro de sintaxe em um chamador Python faz `_parse` retornar `None` e `varrer_py` retornar lista vazia; a cobertura não é reduzida por isso, permitindo zero falso fora da classe shell. (`bin/quem_chama.py:186`, `bin/quem_chama.py:392`, `bin/quem_chama.py:691`)

## 05/11 18:20 · sessao dac32951643e · - · - · origem=Ronald (tmux)

- **Helper exige migração de selo:** `CHAMADORES` permite somente `PerguntaDisputa.carimbar` chamar `_escrever_carimbo` e `_desfazer_em_memoria`; mover o corpo reprova essa comparação exata. Atualizar as duas entradas e proteger o novo helper contra chamada externa é necessário; alternativa menor é manter o corpo dentro do `atomic`. `app/chamados/tests/test_o261e_f2_porta_carimbo.py:373`, `:381`.

- **Refresh depois do commit abre falha pós-gravação:** o carimbo pode ter sido salvo, mas uma falha no `refresh_from_db` fará a chamada lançar exceção; outro ato também pode alterar a linha entre commit e refresh. O contrato hoje promete restaurar memória quando a escrita/lavra falham, mas essa nova etapa ocorreria depois do save. `app/chamados/models.py:2598`, `:2599`; sucesso/erro dessa nova etapa [nao verificado].

- **Snapshot não cobre alteração da célula durante C6:** travar somente pergunta/disputa não trava `celula`; C6 lê seu veredito e ata por funções auxiliares. Serialização dessa fonte com o carimbo continua [nao verificado]. `app/chamados/services/relavra_via.py:221`; `app/chamados/juizes.py:1147`, `:1181`, `:1195`.

- **Tranca pode mudar após precheck:** os leitores atuais verificam tranca antes de entrar na porta; C6 também a dá por morta. O wrapper que apenas compara campos da pergunta não transforma a tranca em impedimento de carimbo. `app/chamados/reconciliador.py:616`; `app/chamados/services/disputa_emissao.py:1589`; `app/chamados/juizes.py:1140`.

- **Memória na recusa:** se fresh divergir e devolver `False`, apagar somente `_pv_memo` deixa resposta/validação/tripla da instância recebida antigas. Os chamadores que tratam `False` por `continue` reduzem o efeito, mas não oferecem contrato geral de sincronização. `app/chamados/services/disputa_emissao.py:1604`, `:1675`; `app/chamados/services/materializacao.py:546`.

- **Outer rollback permanece:** `_materializar_uma` envolve o carimbo em transação maior; sincronizar a instância antes do commit externo não impede que ela conserve o estado posteriormente revertido. `app/chamados/services/materializacao.py:1142`, `:1199`.

- **Mocks existentes continuam ativos:** os testes patcham `PerguntaDisputa.save` na classe, portanto fresh instance também recebe a falha; não há fundamento nessa suíte para alegar que o wrapper escaparia de mocks de instância. `app/chamados/tests/test_o261e_f2_fila.py:184`, `:211`.

- **Deadlock e custo precisam de prova:** o fechamento da disputa chama materialização sincronicamente e pode chegar ao carimbo já segurando disputa; a ordem efetiva e o número novo de consultas não foram medidos. `app/chamados/signals.py:125`, `:140`; `app/chamados/services/materializacao.py:1346`; [nao verificado].

## 05/11 18:18 · sessao 81b857609c61 · - · - · origem=Ronald (tmux)

- O contrato local exige lista determinística por AST/shell/crons/templates, “lista é fato, não juiz”, com zero autoridade nova; facetas fora da lista positiva dizem `nao varrido`, e ausência de achado avulso não prova ausência de chamador. (`bin/quem_chama.py:4`, `bin/quem_chama.py:8`, `bin/quem_chama.py:12`)
- A referência normativa é L-110; números vêm da regra antes do código. L-111 torna especialmente perigoso um zero falso porque ele pode fundamentar a eliminação do sítio. (`app/docs/LEIS.md:136`, `app/docs/LEIS.md:137`)
- A cura deve ocorrer na fonte, com um escritor, RED e censo de quem mais lê; instrumento declara `padrao=n/a (instrumento)`. (`CLAUDE.md:17`, `CLAUDE.md:28`, `CLAUDE.md:32`, `CLAUDE.md:62`, `CLAUDE.md:68`, `CLAUDE.md:193`)
- Reprovação final B3 gera pergunta de desenho e paralisa a obra; a ocorrência e liberação concretas de OTIMIZA-3 constam somente do anexo fornecido, sem linha local conferida. (`CLAUDE.md:271`; anexo `otimiza3_reprovacoes.txt` [nao verificado])
- Não encontrei juiz registrado para censo de chamadores: `core/juizes.py` registra perguntas de domínio; a ferramenta declara expressamente zero autoridade nova. O lookup informa que o registro vivo é `core/juizes.py`. (`app/core/juizes.py:3`, `app/core/juizes.py:7`, `app/docs/JUIZES.md:7`, `bin/quem_chama.py:6`)
- O chamador externo literal encontrado é o selo, configurável por `QUEM_CHAMA_PY`; suas chamadas verificam fixture, erro de saída, determinismo e históricos reais. A régua chama todos os selos `tests/test_*.sh`. (`bin/tests/test_quem_chama.sh:18`, `bin/tests/test_quem_chama.sh:25`, `bin/tests/test_quem_chama.sh:148`, `bin/tests/test_quem_chama.sh:253`, `bin/tests/test_quem_chama.sh:257`, `bin/tests/test_quem_chama.sh:262`, `bin/tests/test_quem_chama.sh:266`, `bin/regua.sh:138`)
- Os testes de símbolo parado e relativo parado verificam o texto da linha globalmente, sem prendê-lo à faceta; `_tem_sec` disponível atualmente restringe ao alvo, e os controles de tipos também trabalham nesse nível. (`bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/tests/test_quem_chama.sh:227`, `bin/tests/test_quem_chama.sh:239`)
- Leitores internos das classificações: `_faceta` decide contagem versus desconhecido, alimenta contadores, `render` chama uma vez por arquivo e símbolo e fecha o resumo; `main` lê `conta['nao']` na confirmação de gravação. (`bin/quem_chama.py:690`, `bin/quem_chama.py:713`, `bin/quem_chama.py:715`, `bin/quem_chama.py:718`, `bin/quem_chama.py:747`)
- O contrato temporal 21→20/cross-meia-noite se refere ao domínio de ponto; a regra de teto temporal é explicitamente para emissor/juiz de marco/célula, enquanto esta ferramenta consome revisões Git. (`CLAUDE.md:81`, `CLAUDE.md:185`, `bin/quem_chama.py:17`, `bin/quem_chama.py:109`)

## 05/11 18:18 · sessao dac32951643e · - · - · origem=Ronald (tmux)

Censo somente de código; não consultei banco/log nem escrevi arquivos. Os pontos abaixo referem-se à cópia lida; ocorrência de corridas em produção [nao verificado].

**Portas e escritores**

- Tripla: único atribuidor `_escrever_carimbo`, `app/chamados/models.py:2545`; carimbo comum `:2551`; identidade `:2614`; descarimbo `:2635`; lote `:2645`.
- Via isolada: `encerrar_por_fato_no_espelho`, `app/chamados/models.py:2662`; `relavrar`, `app/chamados/services/relavra_via.py:123`.
- Chamadores comuns: `app/chamados/models.py:2674`; `app/chamados/reconciliador.py:405`, `:620`; `app/chamados/services/disputa_emissao.py:1603`, `:1675`; `app/chamados/services/materializacao.py:546`, `:1199`, `:1346`.
- Chamadores identidade: `app/chamados/models.py:2511`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:891`, `:1066`.
- Inversos: `app/chamados/models.py:1444`, `:1493`, `:2685`, `:2702`, `:2709`; `app/chamados/services/relavra_via.py:231`; `app/chamados/services/disputa_emissao.py:1412`.
- Resposta humana escreve texto/instante e desfaz validação antiga: `app/chamados/services/disputa_emissao.py:1282`, `:1295`, `:1301`. Automática: `:1428`; recuperação pela trilha: `:1445`; correção/lote: `:1393`, `:1413`; ressuscitação: `app/chamados/models.py:1493`.
- Validação: `app/chamados/services/validacao.py:131`; devolução `:188`; preenchimento condicional pelo carimbo comum: `app/chamados/models.py:2578`, `:2581`; restauração: `:2609`.
- Lavra: atribuições `app/chamados/veredito_lavrado.py:227`; restauração `:243`; persistência normal `:231`; persistência lote `:207`. Backfill atribui em memória em `app/chamados/management/commands/backfill_veredito_lavrado.py:138`.
- Memo: chave em `app/chamados/juizes.py:811`; leitura `:836`; escrita `:847`; descarte `app/chamados/models.py:2612`, `:2638`.

**Concorrência e proposta mínima**

1. C6 retorna morto imediatamente pela validação (`app/chamados/juizes.py:1136`) e pela identidade (`:1171`); portanto só reler sob trava e chamar C6 não impede trocar uma decisão humana por via automática. A porta atualmente escreve a via antes de chamar C6, em `app/chamados/models.py:2575`, `:2585`.

2. Compare a fotografia da intenção recebida com a linha relida, antes de aplicar o carimbo: resposta, instante, validação/autor, tripla e identidade do fato/FKs. Se diferirem, recuse a intenção antiga sem escrever. É controle de concorrência, sem novo juiz; a necessidade de preservar decisão humana consta do caminho inverso em `app/chamados/services/relavra_via.py:214`.

3. Não inclua o veredito/ata atual da célula como motivo automático de recusa por divergência: `_materializar_uma` planta dentro de `atomic` antes do carimbo e precisa rejulgar a luz resultante. A sequência está em `app/chamados/services/materializacao.py:1142`, `:1199`; carimbo recusado levanta `_Reprovada` e desfaz o plantio em `:1201`.

4. Não encontrei escrita não persistida da resposta/validação/tripla nos oito chamadores antes de invocar `carimbar`; no materializador os atributos adicionais alterados são pendência/recusa, salvos imediatamente em `app/chamados/services/materializacao.py:817`, `:818`, `:1069`, `:1070`.

5. Uma proibição geral “já carimbada ⇒ False” seria uma regra adicional da porta pública. Os seletores correntes já excluem carimbo nos caminhos de lote em `app/chamados/services/materializacao.py:529`, `:1341`; `app/chamados/services/disputa_emissao.py:1574`, `:1657`; `app/chamados/reconciliador.py:366`, `:583`. Comparação de fotografia preserva mais do contrato existente.

6. **Ordem de trava não está provada.** `relavra_via` pede pergunta+disputa em um SELECT em `app/chamados/services/relavra_via.py:221`; a lista `of` não estabelece ordem física. O fechamento da disputa dispara materialização sincronicamente por `app/chamados/signals.py:125`, `:140`, cujo carimbo está em `app/chamados/services/materializacao.py:1346`; esse caminho pode já deter a disputa antes da pergunta. Prova concorrente e ausência de deadlock [nao verificado].

7. A resposta nova e a validação ainda decidem sobre instâncias sem releitura travada: `app/chamados/services/disputa_emissao.py:1285`, `:1301`; `app/chamados/services/validacao.py:359`, `:363`, `:395`. Um wrapper em `carimbar` não torna esses outros atos serializáveis.

**Leitores diretos adicionais — lista para consolidar B/D**

- Juízes: `app/chamados/juizes.py:277`, `:334`, `:647`, `:812`, `:1113`, `:1136`, `:1150`, `:1171`, `:1177`, `:1207`, `:1257`, `:1392`, `:1399`, `:1428`, `:1575`, `:1679`, `:1748`.
- Modelo/estado/query: `app/chamados/models.py:1424`, `:1773`, `:1843`, `:2488`, `:2735`, `:2807`.
- Filas/fluxos: `app/chamados/canal.py:65`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/views.py:1257`; `app/chamados/views_cobrar.py:169`; `app/chamados/templatetags/chamados_tags.py:128`.
- Serviços: `app/chamados/services/acoes_disputa.py:95`, `:130`, `:341`; `aviso_pendencia.py:13`; `becos.py:30`; `cobranca.py:229`; `competencia_trancada.py:251`; `declaracao_texto.py:211`, `:366`, `:635`; `disputa_emissao.py:695`, `:909`, `:929`, `:1152`, `:1481`, `:1596`, `:1657`; `explicador.py:32`, `:79`; `fabrica_da_celula.py:119`, `:138`; `fio.py:336`, `:656`, `:847`; `furo_sem_canal.py:81`; `historico.py:99`, `:103`; `materializacao.py:278`, `:305`, `:356`, `:400`, `:431`, `:467`, `:604`, `:938`, `:962`, `:1023`, `:1292`; `painel.py:67`, `:260`; `passivo_validado.py:64`, `:140`, `:515`; `raiox.py:31`, `:133`, `:173`; `regularizacao_ext.py:198`, `:219`; `relavra_via.py:116`, `:153`, `:179`, `:229`; `validacao.py:139`, `:180`, `:315`, `:451`, `:534`, `:541`, `:685`, `:706`, `:806`, `:877`; `verbos.py:240`, `:311`; `vigia_carimbo.py:98`; `vigia_fio.py:675`, `:783`; `veredito_lavrado.py:163`, `:179`.
- API: `app/api/views.py:1009`, `:1173`, `:1187`, `:2580`, `:2636`; `app/api/views_mensageria.py:567`.
- Outros domínios: `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/cartorio.py:1590`, `:1599`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/flip_auto.py:129`; `app/ponto/services/procedencia_batida.py:71`; `app/ponto/supra_juiz.py:545`.
- Commands: `app/chamados/management/commands/auditar_disputas.py:71`; `auditar_invariantes_chamados.py:177`, `:433`, `:538`, `:588`, `:905`, `:955`, `:1073`, `:1103`, `:1154`, `:1170`, `:1569`, `:1599`, `:1652`, `:1707`, `:1763`; `backfill_veredito_lavrado.py:120`; `curar_perguntas_em_limbo.py:46`; `dedup_perguntas_fato.py:71`; `encerrar_perguntas_com_fato.py:32`, `:66`; `reabrir_orfas_enterradas.py:67`, `:129`; `relavrar_via_i6.py:58`; `validar_classe_a.py:80`; `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/ponto/management/commands/reconciliar_grade.py:461`.
- Templates: `app/templates/chamados/partials/_pergunta_card.html:20`, `:29`, `:42`; `_card_atendimento.html:94`, `:100`, `:108`, `:135`, `:142`, `:148`, `:155`, `:176`, `:186`, `:384`; `_lote_preview.html:24`; `modal_fio.html:184`, `:212`, `:223`.

As referências de serviços e commands abreviadas na lista conservam os prefixos apresentados na respectiva linha; referências são ocorrências de leitura/escrita, não prova de que todas respondem à mesma pergunta de domínio. [nao verificado]

## 05/11 18:18 · sessao dbf814f9faed · - · - · origem=Ronald (tmux)

O aval já escolheu o desenho: `atomic`, releitura travada dentro de `carimbar`, novo julgamento pelo C6, conferência de `_pv_memo` e RED das intercalações; a ordem de trava precisa ser medida antes de afirmar verde. `app/docs/PROMPTS.md:1882`.

Casos pela regra, sem alegação de ocorrência em produção:

| Cadastro + estado | Ato | Resultado exigido e lastro |
|---|---|---|
| Pergunta com instância antiga; admin concluiu o mesmo dia entre coleta e escrita. | Tentar carimbo automático antigo. | Preservar decisão humana, sua autoria, instante e trilha; a porta inversa registra exatamente esse problema. `app/chamados/services/relavra_via.py:214`; `app/docs/LEIS.md:65`; `app/chamados/models.py:2502`. |
| Pergunta validada sobre resposta R1; colaborador responde R2 entre coleta e escrita. | Materializador antigo tenta `resposta='validar'`. | Validação de R1 não autoriza validação de R2; preservar voz nova, invalidar validação anterior e conservar trilha. `app/chamados/services/disputa_emissao.py:1287`; `app/chamados/services/disputa_emissao.py:1303`; `app/docs/CORTES.md:333`. |
| Dia acusador; lâmpada do marco perguntado acesa e outro marco apagado. | Carimbar a pergunta do marco provado. | Encerrar a pergunta cujo fato foi provado, preservando cobrança do outro furo. `app/chamados/juizes.py:1173`; `app/chamados/juizes.py:878`. |
| Dia provisoriamente não acusador; marco perguntado pendente, sem batida. | Tentar concluir, depois chegar a batida e rejulgar. | Antes da prova, não absolver por veredito provisório; após a chegada, absolver na passada seguinte. `app/chamados/juizes.py:1185`; `app/docs/CORTES.md:413`. |
| Empresa corte 21; fatos em 20/10 e 21/10; 10 trancada, 11 aberta. | Julgar/carimbar os dois fatos. | 20/10 pertence à 10; 21/10 pertence à 11; tranca silencia por leitura, sem fabricar resolução/batida, e reabrir devolve a pergunta. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/chamados/juizes.py:1138`. |
| 12x36 19–07, intervalo 23–00, turno iniciado em D. | Consumir resposta de volta 00:00. | A volta ocorre em D+1 e a jornada pertence ao dia de início; data sai da cadeia de marcos. `app/chamados/juizes.py:285`; `app/docs/CORTES.md:333`. |
| Célula gerada pelo vínculo A, encerrado hoje mas vigente em D; vínculo B atual com outros marcos. | Julgar/carimbar D. | A geradora A manda dentro da sua vigência; não substituir pelo vínculo hoje ativo. Se a geradora estiver fora da vigência, devolver desconhecimento/CADASTRO, sem escolher B silenciosamente. `app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`. |
| Célula de trabalho sem ata/lâmpadas lavradas. | Concluir por “não há lâmpada apagada”. | Ausência de prova é desconhecimento, não prova de resolução. O corte local explicita ata nunca lavrada = “não sei”. `app/chamados/juizes.py:473`; `app/chamados/juizes.py:878`. |
| Resposta não validada, discordante da luz, mesmo com carimbo de identidade. | Ler a pergunta. | Resposta continua exigindo ação do admin, salvo decisão humana explícita sobre o mesmo dia; identidade não apaga voz. `app/chamados/juizes.py:1150`; `app/chamados/models.py:2615`. |
| Ata de ausência parcial; marco fora do período coberto. | Absorver todas as perguntas pela ausência. | Cobrar o resto descoberto. `app/docs/JUIZES.md:59`. |
| Candidato aceito por C6; lavra ou save falha depois. | Retornar ao chamador. | Não deixar memória declarando carimbo/lavra não gravados. `app/chamados/models.py:2561`; `app/chamados/models.py:2599`; `app/chamados/veredito_lavrado.py:240`. |

Contraexemplos ao diff ingênuo “reler + aplicar candidato antigo + C6”:

1. **Valida a resposta errada.** `_materializar_uma` planta a partir da instância antiga antes de entrar em `carimbar`, depois pede `resposta='validar'`; ao reler R2 e preencher `validada_em`, C6 já devolve morto no primeiro degrau, sem avaliar se a decisão foi sobre R1. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1135`.

2. **Sobrescreve decisão humana mesmo relendo.** Banco já contém `admin_decidiu`; aplicar a via automática antes de C6 troca a identidade e perde a tripla humana. C6 verifica o candidato modificado, não a legitimidade temporal da intenção antiga; o juiz não fiscaliza autoria/instante do carimbo. `app/chamados/models.py:2545`; `app/chamados/models.py:2575`; `app/chamados/juizes.py:1147`; `app/chamados/models.py:2502`.

3. **Tranca não implica quitação definitiva.** C6 pode devolver morto porque a competência está trancada; isso não autoriza produzir carimbo permanente por uma via automática. A tranca é reversível por leitura. `app/chamados/juizes.py:1138`; `app/chamados/models.py:2514`.

4. **Ata vazia permanece divergência.** Hoje C6 encerra célula não acusadora quando `marcos_pendentes` é falso, inclusive ata `{}`; existe teste esperando isso. Esse teste caracteriza o código e não resolve a regra, pois L-110 proíbe código velho como gabarito. `app/chamados/juizes.py:1195`; `app/chamados/tests/test_o261_pergunta_viva_teto.py:80`; `app/docs/LEIS.md:136`.

5. **Memo incompleto.** `_chave_memo_pv` inclui veredito/via da célula, mas não `ata`, `dna` nem `motivo`, embora o julgamento do marco os leia. Reutilizar memória/célula antiga após refresh da pergunta pode dar resultado antigo com a mesma chave. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:886`; `app/chamados/juizes.py:893`; `app/chamados/juizes.py:910`.

6. **Transação não é identidade da decisão.** `validar_pergunta` coleta sem trava e só abre atomic depois das guardas; se concorrência modificou a resposta nesse intervalo, atomic isoladamente não vincula a decisão à resposta lida. `app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:363`; `app/chamados/services/validacao.py:395`.

7. **Ordem de trava ainda precisa de prova.** A referência escolhida trava pergunta e disputa no mesmo SELECT; outros fluxos travam disputa antes de emitir perguntas. Prova de ausência de deadlock não decorre só de copiar `of=('self','disputa')`. `app/chamados/services/relavra_via.py:221`; `app/chamados/services/disputa_emissao.py:467`; `app/chamados/services/disputa_emissao.py:1156`; `app/docs/PROMPTS.md:1882`.

Juízes existentes: C6 = `pergunta_viva`, C15 = `fio_vivo`, C17 = `porta_alcancavel_do_dia`, C18 = `dia_sem_trabalho`; não nasce juiz novo. `app/core/juizes.py:266`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`. JUIZES.md é lookup e o registro vivo manda; ali validação aponta a `validar_pergunta`, mas a porta `carimbar` também escreve os campos no modo `'validar'`, o que exige censo explícito, sem ocultar esse escritor. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/docs/JUIZES.md:38`; `app/chamados/models.py:2580`.

## 05/11 18:17 · sessao d4feb77f32e6 · - · - · origem=Ronald (tmux)

Duas falhas certas permanecem no candidato:

1. **Há outro emissor ativo dos mesmos chamados.** Os crons de alertas coletam todos os chamados `COBRANCA` regularizáveis do colaborador, podendo incluir `batida_ausente`, e chamam a fábrica de órfãos. Retirar o segundo ato do detector e a escrita do disparador não elimina esses caminhos. (`app/ponto/management/commands/processar_alertas_turno.py:372`; `app/ponto/management/commands/processar_alertas_turno.py:384`; `app/ponto/management/commands/processar_alertas_avancados.py:152`; `app/ponto/management/commands/processar_alertas_avancados.py:158`; `app/config/crons.py:362`; `app/config/crons.py:363`)

2. **`@atomic` sozinho não serializa a reabertura.** Duas chamadas diretas podem ler o estado terminal antes da escrita de qualquer uma e ambas gravar transição/trilha. A leitura atual não trava a linha; deve usar a recarga travada existente antes das recusas e de `decidir`. (`app/chamados/models.py:1322`; `app/chamados/models.py:1379`; `app/chamados/models.py:1382`; `app/chamados/models.py:275`; `app/chamados/models.py:668`)

Os publicadores devem ficar nas duas portas de carimbo: [nao verificado]

- `carimbar`: também grava `validada_em` fora de `validar_pergunta`, por chamadas de materialização e reconciliação. (`app/chamados/models.py:2580`; `app/chamados/services/materializacao.py:546`; `app/chamados/services/materializacao.py:1199`; `app/chamados/services/disputa_emissao.py:1603`)
- `carimbar_identidade`: `paridade_recusada` também pode matar a última pergunta; publicar apenas em `retratar_duplicata` deixa essa escrita descoberta. (`app/chamados/services/materializacao.py:891`; `app/chamados/models.py:2633`)

Publicar em toda lavra persistida resolve o contraexemplo de C6 mudar sem mudança de `veredito_lavrado`: uma pergunta materializada de tipo `escolha` mantém VL=`sim`, mas pode passar de viva para morta quando sai da janela. (`app/chamados/juizes.py:672`; `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:1214`; `app/chamados/veredito_lavrado.py:219`)

Não encontrei ciclo infinito demonstrável após essa correção: com publicação condicionada à mudança real na costura, fábrica sem motivos e descarimbo já consumado, a repetição tende a cessar; convergência executada permanece **[nao verificado]**. (`app/chamados/models.py:2454`; `app/chamados/services/fabrica_da_celula.py:174`; `app/chamados/services/relavra_via.py:223`)

## 05/11 18:16 · sessao cddac5a9e16a · PC2-OTIMIZA-3 · censo · origem=Code

HASH LIDO = d1354f77cf4c7214728709ad948145994a0c0366

## A CASOS PELA REGRA

O hash acima identifica a cópia informada; sua correspondência com um HEAD Git é [nao verificado], pois esta cópia não disponibiliza `.git`. Os resultados históricos abaixo são os fornecidos na sonda, não novas execuções dos ranges. A referência é o contrato: facetas cobertas recebem N; cobertura ausente recebe **“nao varrido”**, com eventuais achados avulsos. (`bin/quem_chama.py:8`, `bin/quem_chama.py:17`; `CLAUDE.md:45`)

| Alvo + estado + ato | Resposta exigida pela regra |
|---|---|
| Cada um dos 39 alvos documentais da primeira sonda: csv, json fora de `static/`, md, pdf e txt; consultar chamadores. | **“nao varrido” para cada arquivo**, inclusive quando há citações. Os 12 achados de `CORTES.json`, por exemplo, não fecham seu universo. A classificação é por classe, não por uma enumeração de exemplos excluídos. (`bin/quem_chama.py:284`, `bin/quem_chama.py:308`, `bin/quem_chama.py:697`; `otimiza3_reprovacoes.txt:1`, anexo) |
| `hasner-ponto.css`, carregado por `{% static %}`; consultar a faceta arquivo. | Varrido: **N=3 na sonda fornecida**, considerando os três sítios ali enumerados; não zero. O carregamento concreto permanece em `base.html:17`. (`app/templates/base.html:17`; `bin/quem_chama.py:49`, `bin/quem_chama.py:537`; N neste hash: [nao verificado]) |
| Biblioteca `chamados_tags` e filtro `espera`; consultar arquivo e símbolo separadamente. | A biblioteca conta seus carregadores/importadores; o filtro conta seus usos nas bibliotecas carregadas. Na sonda: **arquivo N=13; `espera` N=15**. Não limitar o símbolo ao teste Python. (`app/chamados/templatetags/chamados_tags.py:53`; `bin/quem_chama.py:43`, `bin/quem_chama.py:53`; N neste hash: [nao verificado]) |
| Fixture isolada: `from m.lib import mudada`, sem outros leitores. | **Arquivo N=1; símbolo `mudada` N=1**. São duas facetas, com o mesmo sítio de importação. (`bin/quem_chama.py:408`, `bin/quem_chama.py:414`; `bin/tests/test_quem_chama.sh:51`) |
| Fixture isolada: muda `mudada`, mas o leitor importa somente `parada`. | **Arquivo N=1; símbolo `mudada` N=0**. A linha “importa de m.lib” deve aparecer sob **arquivo**, nunca sob o símbolo alterado. A mesma obrigação vale para o import relativo. (`bin/quem_chama.py:408`, `bin/quem_chama.py:411`; `bin/tests/test_quem_chama.sh:52`, `bin/tests/test_quem_chama.sh:57`; `otimiza3_reprovacoes.txt:2`, anexo) |
| Símbolo ou módulo apagado/renomeado; ainda há leitor na ponta. | O nome antigo continua alvo e seu leitor deve entrar em N. Apagar a definição não autoriza perder o chamador sobrevivente. (`bin/quem_chama.py:17`, `bin/quem_chama.py:650`; `bin/tests/test_quem_chama.sh:172`) |
| `datas_cravadas_prod`; import dentro de `manage.py shell --command` em `.sh`. | **Arquivo e símbolo “nao varrido” quanto à exposição shell**; conservar a citação encontrada como avulsa. O chamador concreto impede afirmar zero. Não analisar o payload Python. (`bin/placar_code.sh:384`; `avais.txt:4`, item 8, anexo; `otimiza3_reprovacoes.txt:2`, anexo) |
| Três módulos isolados, sem `__init__.py`, import local ou leitura própria; um chamado por heredoc, outro por `python3 -c`, outro por `shell --command`. | **Arquivo e símbolo “nao varrido” nos três casos**, mesmo quando a busca textual encontra zero avulsos. `from pacote import modulo` também deve impedir o zero falso. (`bin/quem_chama.py:124`, `bin/quem_chama.py:510`, `bin/quem_chama.py:673`; `otimiza3_reprovacoes.txt:2`, anexo) |
| Módulo simples de `bin/`, citado em shell; ou shell sem extensão reconhecido pelo shebang. | Mesma declaração de cobertura incompleta; não depender da existência de ponto no nome do módulo nem exclusivamente da extensão `.sh`. (`bin/quem_chama.py:131`, `bin/quem_chama.py:642`, `bin/quem_chama.py:612`) |
| Método `GLPIClient.abrir_ticket`, lido por instância; consultar símbolo. | **“nao varrido”**, inclusive com zero avulsos. A contagem da classe não substitui a cobertura do método. (`bin/quem_chama.py:315`; `bin/tests/test_quem_chama.sh:225`) |
| Teste, módulo autod descoberto pelo Django, setting, convenção ou decorador não transparente. | A faceta correspondente recebe **“nao varrido”**; achados não a promovem automaticamente a varrida. (`bin/quem_chama.py:295`, `bin/quem_chama.py:299`, `bin/quem_chama.py:317`, `bin/quem_chama.py:319`, `bin/quem_chama.py:323`) |
| Filtro registrado `rot`; template carrega a biblioteca e usa `{% filter rot %}`. | Na fixture com apenas esse uso, **símbolo N=1**. A sintaxe de bloco não pode produzir zero falso. Sem carregar a biblioteca, não conta como uso desse registro. (`bin/quem_chama.py:53`, `bin/quem_chama.py:553`; `otimiza3_reprovacoes.txt:2`, fila, anexo) |
| Mesmo filtro escrito apenas dentro de `{# … #}` multilinha ou `{% comment %}`. | O comentário não acrescenta chamador; na fixture sem outro uso, **N=0**. A posição das linhas restantes precisa ser preservada. (`bin/quem_chama.py:21`, `bin/quem_chama.py:520`) |
| Management command, template, script ou artefato estático com referência literal em fonte/config admitida. | **Varrido com N dos sítios encontrados**, pelas respectivas facetas. JSON ser chamador não torna qualquer JSON um alvo coberto. (`bin/quem_chama.py:38`, `bin/quem_chama.py:599`) |
| Duas consultas concorrentes ao mesmo range, com disco sujo; passagem de meia-noite. | A consulta lê as revisões resolvidas, não o disco nem a hora corrente; para as mesmas revisões, deve devolver a mesma lista. Concorrência sobre o mesmo arquivo de `--saida` não tem garantia demonstrada. (`bin/quem_chama.py:619`, `bin/quem_chama.py:624`, `bin/quem_chama.py:742`; garantia de saída concorrente: [nao verificado]) |
| Bordas 21→20, vínculo trocado ou célula sem ata. | Não há caso trabalhista determinado nesta obra: o contrato recebe **range e arquivos**, não cadastro/batidas/ata. Se esses termos aparecem numa fonte, aplicam-se as regras de referência dessa fonte; resultados de folha são [nao verificado]. (`bin/quem_chama.py:24`, `bin/quem_chama.py:724`) |
| Range/revisão/raiz ilegível. | **Erro rc=2**, não censo vazio nem zero chamador. (`bin/quem_chama.py:26`, `bin/quem_chama.py:734`) |

“Zero chamador” do instrumento não é automaticamente “zero chamador **de produção**”: a varredura também lê testes. A L-111 exige a segunda afirmação antes de apagar sítio. (`bin/quem_chama.py:599`, `bin/quem_chama.py:606`; `app/docs/LEIS.md:137`)

## B SITIOS

| Sítio e responsabilidade | Escritores/leitores envolvidos |
|---|---|
| **Fonte da cobertura** | `VARRIDOS` declara classes cobertas; `tipo_do_arquivo` classifica arquivo; `_tipo_simbolo` classifica símbolo. `quem_chama` materializa esses tipos nos alvos. São os sítios que determinam a elegibilidade de N. (`bin/quem_chama.py:38`, `bin/quem_chama.py:284`, `bin/quem_chama.py:311`, `bin/quem_chama.py:637`, `bin/quem_chama.py:655`) |
| **Origem dos conteúdos e alvos** | `lados`, `tocados_do_diff`, `linhas_mudadas` e `_ler_rev` fornecem revisões, arquivos e linhas antigas/novas; `quem_chama` lê esses resultados. A exposição shell deve vir de `na_ponta`, mantendo a mesma fonte da consulta. (`bin/quem_chama.py:88`, `bin/quem_chama.py:109`, `bin/quem_chama.py:142`, `bin/quem_chama.py:157`, `bin/quem_chama.py:619`) |
| **Identidade Python e referências** | `modulo_de` fornece o módulo; `_padrao_nome` fornece limites lexicais; `nomes` recebe caminho, basename, comando, módulo e artefatos. `Alvos.__init__` indexa esses nomes; `citacoes` os lê. (`bin/quem_chama.py:124`, `bin/quem_chama.py:347`, `bin/quem_chama.py:366`, `bin/quem_chama.py:378`, `bin/quem_chama.py:633`) |
| **Símbolos e registro de templates** | `simbolos_mudados` escreve `nome/membro/decor/registro`; a montagem une base e ponta. `_tipo_simbolo`, `varrer_py`, `varrer_proprio` e `varrer_template` leem esses metadados. Não alterar registro ou identidade para disfarçar cobertura incompleta. (`bin/quem_chama.py:243`, `bin/quem_chama.py:253`, `bin/quem_chama.py:411`, `bin/quem_chama.py:442`, `bin/quem_chama.py:555`, `bin/quem_chama.py:650`) |
| **Biblioteca `lib`** | Hoje deriva do tipo original; `Alvos` a indexa e o scanner de templates a lê. Deve ser preservada **antes** de rebaixar a cobertura, para conservar achados avulsos. (`bin/quem_chama.py:657`, `bin/quem_chama.py:375`, `bin/quem_chama.py:549`) |
| **Shell** | `tipo_chamador` seleciona os shells; `_codigo_sh`/`_corta_comentario_sh` fornecem código textual. `varrer_linhas` procura citações e `artefatos_sh` procura saídas. A proposta reutiliza essa leitura, sem chamar `varrer_py` para payload shell. (`bin/quem_chama.py:479`, `bin/quem_chama.py:491`, `bin/quem_chama.py:510`, `bin/quem_chama.py:573`, `bin/quem_chama.py:608`, `bin/quem_chama.py:673`) |
| **Filtro e comentário multilinha** | `_sem_comentario_template` remove comentários; `varrer_template` lê `registro`, biblioteca carregada e regex de uso. A regex é o sítio fonte da correção de `{% filter %}`. (`bin/quem_chama.py:520`, `bin/quem_chama.py:526`, `bin/quem_chama.py:560`) |
| **Achados e contagem** | Os scanners produzem achados; `quem_chama` escreve os conjuntos `por`. `_faceta` permanece o emissor único de **N/“nao varrido”** e contador de `varridas/zeros/nao`. `render` e `main` leem esses resultados. (`bin/quem_chama.py:662`, `bin/quem_chama.py:683`, `bin/quem_chama.py:686`, `bin/quem_chama.py:690`, `bin/quem_chama.py:713`, `bin/quem_chama.py:718`, `bin/quem_chama.py:747`) |
| **Selo a corrigir** | A fixture e as asserções moram no selo existente. Precisam de alvos shell isolados, verificação de **alvo+faceta**, bloco `filter` e comentário multilinha. Não basta `_sec`, que delimita somente o alvo. (`bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/tests/test_quem_chama.sh:210`) |
| **Chamadores externos do instrumento** | O selo seleciona a fonte por `QUEM_CHAMA_PY` e a executa; a régua executa os selos pelo glob existente. Não precisam de novo chamador ou escritor. (`bin/tests/test_quem_chama.sh:25`, `bin/tests/test_quem_chama.sh:148`; `bin/regua.sh:138`) |
| **Consumidores do relatório** | `render` entrega texto a `main`, que escreve stdout ou o destino solicitado. Outros consumidores externos de `QUEM_CHAMA.txt` são [nao verificado]. (`bin/quem_chama.py:737`, `bin/quem_chama.py:738`, `bin/quem_chama.py:743`, `bin/quem_chama.py:750`) |

Não foi localizado juiz registrado específico para o censo nos dois registros consultados; o contrato declara expressamente **“lista é fato, não juiz”**. `core/juizes.py` registra autoridades de decisão, e `JUIZES.md` é lookup desse registro. Portanto a proposta não acrescenta juiz. (`bin/quem_chama.py:6`; `app/core/juizes.py:3`, `app/core/juizes.py:7`; `app/docs/JUIZES.md:7`)

As reprovações #3/#4, o teto de **duas voltas opus** e a proibição de analisar Python em `.sh` permanecem obrigações; este censo não libera o B3 nem constitui nova volta de construção. O anexo `pd_otimiza3.r` não foi disponibilizado nesta cópia/texto: [nao verificado]. (`otimiza3_reprovacoes.txt:1`, `otimiza3_reprovacoes.txt:2`, anexos; `avais.txt:3`, `avais.txt:4`, anexos; `CLAUDE.md:271`)

## C DIFF PROPOSTO EM TEXTO

A exposição é **lexical e conservadora**: qualquer componente literal do módulo encontrado em código shell torna suas facetas “nao varrido”. Isso cobre também `from pacote import modulo`; coincidências rebaixam precisão, sem inventar chamador. O registro da biblioteca permanece disponível e `_faceta` continua emitindo o resultado. (`bin/quem_chama.py:124`, `bin/quem_chama.py:347`, `bin/quem_chama.py:655`, `bin/quem_chama.py:657`, `bin/quem_chama.py:690`)

`LEI-AKITA: origem=bin/quem_chama.py::quem_chama/varrer_template; testemunha=aval MANHA-0511 item 8 + contrato positivo; RED=bin/tests/test_quem_chama.sh (proposto, execução integral [nao verificado]); quem-mais-le=seção B; juizes novos=0; padrao=n/a (instrumento).` (`CLAUDE.md:62`, `CLAUDE.md:68`; `avais.txt:4`, anexo)

```diff
--- a/bin/quem_chama.py
+++ b/bin/quem_chama.py
@@ -557,7 +557,7 @@
                 if not reg or not reg[1] or (nomes is not None and reg[1] not in nomes):
                     continue
                 papel, nome = reg
-                rx = re.compile(r'\|\s*' + re.escape(nome) + r'(?![\w])' if papel == 'filtro'
+                rx = re.compile(r'(?:\||{%\s*filter\s+)\s*' + re.escape(nome) + r'(?![\w])' if papel == 'filtro'
                                 else r'{%\s*' + re.escape(nome) + r'(?![\w])')
                 for n, ln in enumerate(linhas, start=1):
                     if rx.search(ln):
@@ -623,6 +623,10 @@
     arvore = [r for r in arvore if r]
     na_ponta = _ler_rev(raiz, ponta, arvore)
     na_base = _ler_rev(raiz, base_rev, tocados)
+    # So exposicao lexical: nao extrai nem analisa Python de shell.
+    codigo_shell = '\n'.join(
+        '\n'.join(_codigo_sh(f).values()) for r, f in sorted(na_ponta.items())
+        if tipo_chamador(r, f) == 'sh')
     por_base = {}
     for r in arvore:
         por_base[r.rsplit('/', 1)[-1]] = por_base.get(r.rsplit('/', 1)[-1], 0) + 1
@@ -652,9 +656,14 @@
                     simb.setdefault(q, info)
                     if info['registro'] and not simb[q]['registro']:
                         simb[q]['registro'] = info['registro']
+        lib = base[:-3] if tipo == 'py-templatetag' else None
+        # Componentes cobrem tambem `from pacote import modulo` e pacote sem __init__.
+        # Coincidencia lexical so perde precisao: vira desconhecido, nunca zero.
+        if tipo in ('py', 'py-command', 'py-templatetag') and any(
+                _padrao_nome(p).search(codigo_shell) for p in mod.split('.')):
+            tipo = 'Python citado em shell; cobertura nao varrida'
         for info in simb.values():
             info['tipo'] = _tipo_simbolo(rel, tipo, info)
-        lib = base[:-3] if tipo == 'py-templatetag' else None
         alvos.append({'rel': rel, 'existe': fonte is not None, 'modulo': mod, 'simbolos': simb, 'tipo': tipo,
                       'template': _template_de(rel), 'estatico': _estatico_de(rel), 'lib': lib,
                       'artefatos': artef, 'nomes': nomes})
--- a/bin/tests/test_quem_chama.sh
+++ b/bin/tests/test_quem_chama.sh
@@ -42,6 +42,18 @@
 printf 'def h():\n    return 1\n' > app/m/management/commands/cmd_x.py
 printf '<p>a</p>\n' > app/m/templates/m/alvo.html
 printf '#!/usr/bin/env bash\necho "a" > /dev/null\n' > "bin/ação.sh"
+# Sem __init__.py, import local ou uso no proprio alvo: cada forma tem seu modulo.
+mkdir -p app/ilha
+for n in heredoc inline comando; do
+  printf 'def exposta():\n    return 1\n' > "app/ilha/$n.py"
+done
+cat > bin/py_embutido.sh <<'EOS'
+python3 <<'PY_EMBUTIDO'
+from ilha.heredoc import exposta
+PY_EMBUTIDO
+python3 -c 'from ilha import inline; inline.exposta()'
+python manage.py shell --command 'from ilha.comando import exposta'
+EOS
 # leitores
 printf '# so cita bin/alvo.sh em comentario\nexit 0\n' > bin/prosa.sh
 printf '#!/usr/bin/env bash\nbash bin/alvo.sh\n' > bin/codigo.sh
@@ -103,7 +115,7 @@
 def nunca_usado(x):
     return x
 EOT
-printf '{%% load m_tags %%}\n{{ a|rot }}\n{%% tg %%}\n' > app/m/templates/m/usa_tag.html
+printf '{%% load m_tags %%}\n{{ a|rot }}\n{%% tg %%}\n{%% filter rot %%}x{%% endfilter %%}\n{#\n{{ a|rot }}\n#}\n' > app/m/templates/m/usa_tag.html
 printf '{{ a|rot }}\n{%% tg %%}\n' > app/m/templates/m/sem_load.html
 printf 'class K:\n    def met(self):\n        return 1\n' > app/m/cls.py
 printf 'from m.cls import K\nK().met()\n' > app/m/usa_k.py
@@ -137,6 +149,9 @@
 printf 'from x import receiver\n\n\n@receiver("s")\ndef r():\n    return 2\n' > app/m/sinais.py
 printf 'caf\351 2\n' > app/m/dados.txt
 git rm -q app/m/vazio.py
+for n in heredoc inline comando; do
+  sed -i 's/return 1/return 2/' "app/ilha/$n.py"
+done
 git commit -qam muda [nao verificado]
 # (f) disco sujo depois da ponta: um chamador NAO commitado nao entra; um chamador apagado so no disco segue [nao verificado]
 printf 'from m.lib import mudada\n' > app/m/sujo_sai.py && git add app/m/sujo_sai.py && git commit -qm sujo
@@ -156,16 +171,29 @@
 _naotem prosa "  bin/prosa.sh:"
 _naotem prosa "  app/m/doc.py:"
 # volta 4: quem importa um nome PARADO carrega o ARQUIVO (faceta arquivo), mas nao chama o simbolo mudado
-grep -F "  app/m/usa_outra.py:" "$T/f.txt" | grep -qv "^  app/m/usa_outra.py:1: importa de m.lib$" && _falha "simbolo-parado: MORDE -- usa_outra entrou fora da faceta arquivo"
-_tem   simbolo-parado "  app/m/usa_outra.py:1: importa de m.lib"
 _naotem fim-de-linha "  bin/fim_coment.sh:"
 _tem   fim-de-linha "  bin/codigo_coment.sh:2:"
 # (b) relativo e apelido
 _tem   relativo1 "  app/m/rel1.py:1: importa mudada"
 _tem   relativo2 "  app/m/sub/rel2.py:1: importa mudada"
 # volta 4: quem importa um nome PARADO carrega o ARQUIVO (faceta arquivo), mas nao chama o simbolo mudado
-grep -F "  app/m/rel_outra.py:" "$T/f.txt" | grep -qv "^  app/m/rel_outra.py:1: importa de m.lib$" && _falha "relativo-parado: MORDE -- rel_outra entrou fora da faceta arquivo"
-_tem   relativo-parado "  app/m/rel_outra.py:1: importa de m.lib"
+python3 - "$T/f.txt" <<'PY_FACETA' || _falha "parado: alvo/faceta incorretos"
+import sys
+esperados = {'app/m/usa_outra.py:1: importa de m.lib',
+             'app/m/rel_outra.py:1: importa de m.lib'}
+vistos = set()
+alvo = faceta = ''
+with open(sys.argv[1], encoding='utf-8') as f:
+    for ln in f:
+        if ln.startswith('== '):
+            alvo = ln[3:].split(' (', 1)[0]
+        elif ln.startswith('   '):
+            faceta = ln.strip()
+        elif ln.strip() in esperados:
+            assert alvo == 'app/m/lib.py' and faceta.startswith('arquivo ['), (alvo, faceta, ln)
+            vistos.add(ln.strip())
+assert vistos == esperados, esperados - vistos
+PY_FACETA
 _tem   apelido1 "  app/m/apelido1.py:2: usa L.mudada"
 _tem   apelido2 "  app/m/apelido2.py:2: usa L.mudada"
 # (c) lado velho
@@ -210,6 +238,13 @@
 _sec() { awk -v a="== $2 (" 'index($0,a)==1{on=1;print;next} /^== |^RESUMO/{on=0} on' "$1"; }
 _tem_sec()   { _sec "$1" "$2" | grep -qF -- "$3" || _falha "$4: secao $2 devia listar [$3]"; }
 _naotem_sec(){ _sec "$1" "$2" | grep -qF -- "$3" && _falha "$4: MORDE -- secao $2 listou [$3]"; }
+for n in heredoc inline comando; do
+  _tem_sec "$T/f.txt" "app/ilha/$n.py" "arquivo: nao varrido (Python citado em shell" "shell-$n-arquivo"
+  _tem_sec "$T/f.txt" "app/ilha/$n.py" "simbolo exposta: nao varrido (Python citado em shell" "shell-$n-simbolo"
+  _naotem_sec "$T/f.txt" "app/ilha/$n.py" "0 chamador(es)" "shell-$n-zero"
+done
+_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "  app/m/templates/m/usa_tag.html:4: usa o filtro |rot" filtro-bloco
+_naotem_sec "$T/f.txt" app/m/templatetags/m_tags.py "  app/m/templates/m/usa_tag.html:6:" comentario-multilinha
 _tem_sec "$T/f.txt" app/static/css/x.css "arquivo [estatico]:" r4-estatico-varrido
 _tem_sec "$T/f.txt" app/static/css/x.css "  app/m/templates/m/base.html:2:" r4-static-tag
 _tem_sec "$T/f.txt" app/static/css/so_js.css "  app/static/js/sw.js:1:" r4-static-js
```

A proposta foi exercitada somente em memória: sintaxe Python/Bash, classificação das formas shell, contadores, filtro, comentário multilinha e rejeição do mutante de faceta. **Suite integral, ruff, RED contra o hash e ranges reais: [nao verificado].** O selo completo cria/remove arquivos, portanto sua execução contrariaria esta chamada. (`bin/tests/test_quem_chama.sh:24`, `bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:148`)

## D CONTRA-EXEMPLOS

- **Rebaixamento excessivo:** um shell contendo `core.bar` também rebaixa um alvo `core.foo`, pelo componente compartilhado `core`; `echo modulo` pode rebaixar `bin/modulo.py`. É perda deliberada de precisão, devolvendo desconhecido, não evidência de chamada. O impacto quantitativo no range é [nao verificado]. (`bin/quem_chama.py:124`, `bin/quem_chama.py:347`; predicado proposto na seção C, hunk de `bin/quem_chama.py:652`)

- **Falha de parsing Python continu

[truncado em 20000 de 22197 caracteres]

## 05/11 18:14 · sessao 71dc9d2eb2f1 · PC2-PEP · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

## A CASOS PELA REGRA

O aval vigente escolheu **candidata 1: dentro de `carimbar`, transação, releitura da linha travada e novo julgamento por C6**. Também exige conferir `_pv_memo`, medir a ordem de travas e RED da intercalação entre decisão humana e resposta nova. A indefinição do `pd_pep.r` foi superada. (`app/docs/PROMPTS.md:1882`)

Os cadastros abaixo são especificações de ensaio pela regra, conforme L-110; sua ocorrência em produção é **[nao verificado]**. A correspondência do identificador da cópia com um objeto Git é **[nao verificado]**. (`app/docs/LEIS.md:136`)

| Caso | Cadastro + estado | Ato | Resultado exigido pela regra |
|---|---|---|---|
| A1 — decisão humana antes da trava | Pergunta coletada sem decisão; admin decide o mesmo dia antes do carimbo automático. | Consumidor tenta gravar usando sua instância antiga. | Preservar decisão, autor e trilha; o ato antigo não pode sobrescrevê-los. Releitura travada obrigatória. (`app/docs/PROMPTS.md:1882`; `app/docs/LEIS.md:65`; `app/chamados/services/relavra_via.py:214`) |
| A2 — resposta nova antes da trava | Consumidor conserva resposta A e sua validação; colaborador grava resposta B. | Consumidor tenta concluir com a autorização anterior. | B não herda a validação de A. A releitura não autoriza validar automaticamente uma fala diferente daquela julgada. (`app/docs/CORTES.md:333`; `app/chamados/services/disputa_emissao.py:1287`) |
| A3 — resposta nova depois da trava | Carimbo trava primeiro; escritor da resposta já conservava instância anterior sem validação. | Carimbo termina; depois o escritor grava resposta nova. | A resposta nova e sua trilha permanecem; a assinatura anterior não pode continuar valendo para ela. A propriedade deve valer nas duas ordens. (`app/docs/CORTES.md:333`; `app/chamados/services/disputa_emissao.py:1287`) |
| A4 — repetição | Mesmo cadastro e mesmos insumos. | Repetir o mesmo ato. | Mesmo estado, uma linha de trilha e zero duplicata; repetição não deve renovar artificialmente o instante. (`CLAUDE.md:175`) |
| A5 — pergunta ainda viva | Célula acusa o marco; nenhuma decisão humana ou testemunha o resolveu. | Tentar carimbo automático. | Não carimbar o que a lâmpada contradiz; não fabricar solução reabrindo outro continente. (`CLAUDE.md:167`; `CLAUDE.md:170`; `app/docs/PROMPTS.md:1777`) |
| A6 — marco resolvido, outro furo | Ata prova o marco desta pergunta; outro marco do dia continua apagado. | Concluir esta pergunta. | Encerrar a cobrança do marco provado, preservando a cobrança do outro. A pergunta cobra seu marco. (`CLAUDE.md:167`; `app/chamados/juizes.py:1173`) |
| A7 — competência 21→20 | Empresa cadastrada com corte 21; fatos em 20/10 e 21/10; competência 10 trancada, 11 aberta. | Julgar/carimbar ambas como “outubro”. | 20/10 pertence a 21/09–20/10; 21/10 pertence a 21/10–20/11. Aplicar o corte cadastrado. Tranca silencia por leitura e não prova quitação. (`app/docs/LEIS.md:77`; `app/docs/LEIS.md:123`; `app/chamados/juizes.py:1138`) |
| A8 — cross-meia-noite | Turno 12x36 19–07, intervalo 23–00, iniciado em D. | Receber volta às 00:00 ou concluir porque virou a data civil. | A volta ocorre em D+1 e integra a jornada de D. Meia-noite não encerra marco pendente. (`app/docs/LEIS.md:117`; `CLAUDE.md:185`; `app/docs/JUIZES.md:24`) |
| A9 — provisório e chegada da batida | Célula provisória não acusadora, marco ainda pendente, sem batida correspondente. | Tentar concluir; depois entregar a batida e rejulgar. | Antes, não absolver por veredito provisório; depois, reconhecer o marco respondido na passada seguinte. (`CLAUDE.md:185`; `app/docs/CORTES.md:413`) |
| A10 — vínculo trocado | Célula histórica gerada por A, vigente no fato e hoje inativo; B está ativo hoje. | Julgar usando B. | Respeitar A e o DNA histórico. Se A estiver fora da vigência do próprio fato, devolver desconhecimento/dono CADASTRO, sem escolher B silenciosamente. (`app/docs/LEIS.md:71`; `app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`) |
| A11 — célula sem ata | Dia de trabalho, `ata={}`, sem validação nem decisão humana. Considerar separadamente veredito vazio e veredito não acusador. | Concluir porque a lista vazia não contém lâmpada apagada. | Ata nunca lavrada é desconhecimento; vazio não prova resolução. O código antigo ou seu teste não podem fornecer o gabarito contrário. (`app/docs/LEIS.md:95`; `app/chamados/juizes.py:473`; `app/docs/LEIS.md:136`) |
| A12 — trancar/destrancar | Pergunta sem fato resolutivo novo. | Trancar e depois reabrir a competência. | A tranca não deve produzir carimbo definitivo; a reabertura devolve a vitalidade por leitura. (`app/chamados/juizes.py:919`; `app/chamados/juizes.py:1138`) |
| A13 — ausência parcial | Ausência cobre somente parte do turno; marco perguntado está fora dela. | Absorver a pergunta pelo rótulo “ausência”. | Cobrar o resto descoberto; ausência parcial não dispensa o dia inteiro. (`app/chamados/juizes.py:403`; `app/docs/JUIZES.md:58`) |
| A14 — resposta concordante/discordante | Marco aceso, resposta por validar; considerar concordância classe A, discordância e impossibilidade de comparar. | Concluir automaticamente. | A concordante pode encerrar pela regra existente; discordante ou incomparável continua para decisão administrativa. (`app/docs/PROMPTS.md:1442`; `app/docs/PROMPTS.md:1445`) |
| A15 — identidade com resposta pendente | Pergunta com via de duplicata e resposta ainda exigindo decisão. | Ler como “carimbada, logo morta”. | A identidade não apaga a voz pendente; a precedência da resposta continua sendo julgada por C6. (`app/chamados/models.py:2615`; `app/chamados/juizes.py:1150`) |
| A16 — erro após julgamento | Pergunta sem o novo carimbo; lavra/save falha. | Sair da porta com exceção. | Não deixar em memória carimbo ou lavra que o ato não gravou; erro não pode fabricar conclusão. (`app/chamados/models.py:2561`; `app/chamados/tests/test_o261e_f2_fila.py:179`; `app/chamados/tests/test_o261e_f2_fila.py:200`) |
| A17 — falta de credor ou antiguidade | Marco continua acusado, sem solução humana/testemunha. | Encerrar porque o chamado morreu ou o fato envelheceu. | Carimbo, idade e estado do chamado não substituem a lâmpada nem o fato resolutivo. (`app/docs/CORTES.md:64`; `CLAUDE.md:170`) |

## B SITIOS

**Fonte da mudança:** `PerguntaDisputa.carimbar`. Hoje ela fotografa e modifica a instância recebida, consulta C6 e salva; seu corpo não relê a linha sob trava. O caminho inverso já relê pergunta e disputa usando `select_for_update(of=('self', 'disputa'))`. (`app/chamados/models.py:2551`; `app/chamados/models.py:2572`; `app/chamados/models.py:2585`; `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`)

**Autoridades existentes — nenhuma precisa nascer nesta obra:**

| Pergunta | Autoridade |
|---|---|
| A pergunta vive? | C6, `chamados.juizes.pergunta_viva`. (`app/core/juizes.py:266`) |
| A disputa pode fechar? | C7, `DisputaSupervisao.pode_fechar`. (`app/core/juizes.py:267`) |
| Existe pergunta viva na coleção? | C15, `fio_vivo`. (`app/core/juizes.py:275`) |
| Há porta alcançável para o dia? | C17, `porta_alcancavel_do_dia`. (`app/core/juizes.py:277`) |
| O dia dispensa cobrança? | C18, `dia_sem_trabalho`. (`app/core/juizes.py:278`) |
| Qual competência pertence ao fato? | `janela_atual`; tranca é consultada por `dia_da_pergunta_trancado`. (`app/core/juizes.py:283`; `app/chamados/juizes.py:919`) |
| Qual vínculo rege o fato? | `vinculo_do_dia`. (`app/core/juizes.py:1285`) |
| Como encadear marcos após meia-noite? | `marcos_turno`, endereço já indicado na tabela de juízes. (`app/docs/JUIZES.md:24`) |
| Existe testemunha da materialização? | Função existente `_pergunta_materializada_de_fato`, também consumida pela lavra; pergunta distinta da vitalidade. (`app/chamados/juizes.py:632`; `app/chamados/veredito_lavrado.py:219`) |
| Quem valida a resposta? | Porta existente `validar_pergunta`; o lookup não elimina as outras atribuições hoje permitidas nos campos. (`app/docs/JUIZES.md:38`; `app/chamados/models.py:2580`) |

`JUIZES.md` é consulta de endereços; o registro vivo é `core/juizes.py`. Isso não transforma comportamento defeituoso do código em regra: o gabarito continua sendo L-110. (`app/docs/JUIZES.md:7`; `app/docs/LEIS.md:136`)

**Os oito pontos que chamam o carimbo comum:**

| Chamador | Sítio |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674` |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405` |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620` |
| `resolver_perguntas_por_chamado_resolvido`, dois ramos | `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675` |
| `_materializar_ausencia_de_resposta` | `app/chamados/services/materializacao.py:546` |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199` |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346` |

**Escritores dos campos lidos ou alterados pela proposta:**

| Campos | Escritores encontrados |
|---|---|
| `materializada_em`, `materializada_por`, `via_resolucao` | Atribuição pela porta `_escrever_carimbo`; persistência em `carimbar`, `carimbar_identidade` e `descarimbar`; limpeza em `descarimbar_em_lote`. (`app/chamados/models.py:2545`; `app/chamados/models.py:2598`; `app/chamados/models.py:2633`; `app/chamados/models.py:2641`; `app/chamados/models.py:2652`) |
| `via_resolucao` isolada | Também `encerrar_por_fato_no_espelho` e `relavrar`. Portanto, unicidade da tripla não significa uma única atribuição literal da via. (`app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`) |
| `resposta_colab` | Resposta humana, automática e limpeza para correção. (`app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1393`) |
| `respondida_em` | Carimbo opcional; resposta humana; automática; recuperação pela trilha; limpeza por correção/ressuscitação. (`app/chamados/models.py:2578`; `app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1429`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`) |
| `validada_em`, `validada_por` | `carimbar(resposta='validar')`, `_carimbar_validacao`, devolução da validação, resposta nova, correção e ressuscitação. (`app/chamados/models.py:2580`; `app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`) |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, restauração na própria autoridade e `gravar_lavras_em_lote`. (`app/chamados/veredito_lavrado.py:211`; `app/chamados/veredito_lavrado.py:240`; `app/chamados/veredito_lavrado.py:197`) |
| `_pv_memo` | C6 grava; desfazer em memória e descarimbar removem. (`app/chamados/juizes.py:847`; `app/chamados/models.py:2612`; `app/chamados/models.py:2638`) |
| `disputa`, `motivo`, `tipo_resposta` | Nascimento por `PerguntaDisputa.abrir`; tipo chega pelos emissores em `**campos`. (`app/chamados/models.py:2465`; `app/chamados/models.py:2466`; `app/chamados/services/disputa_emissao.py:393`; `app/chamados/services/disputa_emissao.py:774`; `app/chamados/services/acoes_disputa.py:262`; `app/chamados/services/declaracao_texto.py:325`; `app/chamados/services/declaracao_texto.py:614`) |
| `celula` | Nascimento e atualização da identidade existente em `abrir`; exclusão da célula pode produzir `SET_NULL`. (`app/chamados/models.py:2466`; `app/chamados/models.py:2425`; `app/chamados/models.py:2463`; `app/chamados/models.py:2198`) |
| `data_evento` | Nascimento; conversa recebe data local; reapontamento pode preencher data ausente. (`app/chamados/models.py:2416`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:832`; `app/chamados/services/fio.py:847`) |
| `chamado_destino` | Nascimento, deduplicação em `abrir`, `reapontar_resolvedora`; exclusão do chamado pode produzir `SET_NULL`. (`app/chamados/models.py:2466`; `app/chamados/models.py:2439`; `app/chamados/services/fio.py:845`; `app/chamados/models.py:2772`) |

A conferência humana da data escreve **`data_conferida_em/por`**, não `data_evento`; esses campos ficam fora do snapshot proposto abaixo, embora liberem a materialização. (`app/chamados/services/validacao.py:617`; `app/chamados/services/validacao.py:619`; `app/chamados/services/materializacao.py:1115`)

**Leitores e seletores do estado afetado, inclusive fora do diff:**

| Uso | Sítios encontrados |
|---|---|
| Estado, vitalidade, seleção SQL e prova | `app/chamados/models.py:2481`; `app/chamados/models.py:2713`; `app/chamados/juizes.py:326`; `app/chamados/juizes.py:632`; `app/chamados/juizes.py:781`; `app/chamados/juizes.py:875`; `app/chamados/juizes.py:1135` |
| Materialização, validação e reconciliação | `app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:529`; `app/chamados/services/materializacao.py:1183`; `app/chamados/services/materializacao.py:1294`; `app/chamados/services/validacao.py:136`; `app/chamados/services/validacao.py:363`; `app/chamados/services/validacao.py:422`; `app/chamados/reconciliador.py:364`; `app/chamados/reconciliador.py:583` |
| Lavra e validade da projeção | `app/chamados/veredito_lavrado.py:162`; `app/chamados/veredito_lavrado.py:166`; `app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:133`; `app/chamados/services/raiox.py:173` |
| Fábrica, reapontamento, duplicata e desfazimento | `app/chamados/services/disputa_emissao.py:695`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fio.py:336`; `app/chamados/services/fio.py:656`; `app/chamados/services/fio.py:882`; `app/chamados/services/relavra_via.py:116`; `app/chamados/services/relavra_via.py:223` |
| Resposta, cobrança e ação | `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/acoes_disputa.py:130`; `app/chamados/services/acoes_disputa.py:341`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/services/declaracao_texto.py:211`; `app/chamados/services/declaracao_texto.py:366`; `app/chamados/services/explicador.py:79`; `app/chamados/canal.py:65` |
| Histórico, painel e apresentação de verbos | `app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:103`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:240`; `app/chamados/services/verbos.py:311` |
| Vigias e passivo | `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:786`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/furo_sem_canal.py:81`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:140`; `app/chamados/services/passivo_validado.py:515` |
| Auditorias e comandos de cura | `app/chamados/management/commands/auditar_invariantes_chamados.py:538`; `app/chamados/management/commands/auditar_invariantes_chamados.py:905`; `app/chamados/management/commands/auditar_invariantes_chamados.py:955`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/dedup_perguntas_fato.py:71`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:33`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`; `app/chamados/management/commands/relavrar_via_i6.py:58`; `app/ponto/management/commands/reconciliar_fantasmas.py:212` |
| App, API, mensageria e telas | `app/chamados/services/pergunta_no_app.py:53`; `app/api/views.py:1173`; `app/api/views.py:2580`; `app/api/views.py:2636`; `app/api/views_mensageria.py:567`; `app/chamados/views.py:1257`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:291`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/chamados/templatetags/chamados_tags.py:128` |
| Folha, procedência, cartório e supra | `app/folha/previa.py:142`; `app/ponto/services/procedencia_batida.py:71`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/cartorio.py:1590`; `app/ponto/services/cartorio.py:1599`; `app/ponto/supra_juiz.py:545` |
| Templates | `app/templates/chamados/partials/_card_atendimento.html:94`; `app/templates/chamados/partials/_card_atendimento.html:186`; `app/templates/chamados/partials/_card_atendimento.html:384`; `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/modal_fio.html:212`; `app/templates/chamados/partials/_lote_preview.html:24` |

Além desses consumidores dos campos, C6 é chamado diretamente por fechamento/ressuscitação, reconciliação, tranca, app, fio e vigia; suas reduções `perguntas_vivas_de` e `fio_vivo` ampliam o alcance. (`app/chamados/models.py:351`; `app/chamados/models.py:1447`; `app/chamados/models.py:1728`; `app/chamados/reconciliador.py:727`; `app/chamados/reconciliador.py:1054`; `app/chamados/services/competencia_trancada.py:122`; `app/chamados/services/pergunta_no_app.py:111`; `app/chamados/services/fio.py:775`; `app/chamados/services/vigia_fio.py:755`; `app/chamados/juizes.py:1230`; `app/chamados/juizes.py:1242`)

**Sítios que precisam entrar na prova de concorrência:** carimbo; escritor da resposta; validação; inverso do carimbo; fechamento de chamado/disputa e seus signals; plantio anterior ao carimbo. O fechamento completo dessas intercalações não se prova modificando somente a primeira função. (`app/chamados/models.py:2551`; `app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/validacao.py:359`; `app/chamados/services/relavra_via.py:221`; `app/chamados/models.py:263`; `app/chamados/signals.py:75`; `app/chamados/signals.py:125`; `app/chamados/services/materializacao.py:1142`)

As catracas existentes cobrem escritores da tripla/via e uso do retorno; os testes de memória pós-erro também precisam ser preservados. Elas não substituem o RED concorrente solicitado. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:43`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:51`; `app/chamados/tests/test_o261e_f2_fila.py:50`; `app/chamados/tests/test_o261e_f2_fila.py:173`; `app/docs/PROMPTS.md:1882`)

Completude contra escrita dinâmica, SQL externo ou operações no banco não presentes na cópia: **[nao verificado]**.

## C DIFF PROPOSTO EM TEXTO

A proposta local concentra trava, releitura, rejeição do ato obsoleto e julgamento na porta existente. A comparação protege a autorização do ato recebido: simplesmente reler e preencher `validada_em` permitiria validar a resposta nova antes de C6 julgá-la. A identidade já carimbada também é preservada. (`app/docs/PROMPTS.md:1882`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1136`; `app/chamados/services/relavra_via.py:161`)

O `try` envolve também a saída da transação para preservar a restauração local em falhas do ato. Este é o diff do **PEP local**; os contraexemplos da seção D impedem tratá-lo como cura completa de todos os casos A. (`app/chamados/models.py:2561`; `app/chamados/services/materializacao.py:1142`; `app/docs/CORTES.md:333`)

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/models.py
@@ -2563,42 +2563,57 @@
         do chamador, que segue lendo a instancia (o veredito re-lavrado em memoria volta junto)."""
         from django.utils import timezone as _tz
         from chamados.juizes import pergunta_viva, texto_vazio
+        from chamados import veredito_lavrado as _vl
         if via in self.vias_que_encerram_por_identidade():
             raise ValueError('via %r: identidade sai por carimbar_identidade; a tranca nao tem escritor (L-111)' % via)
         if resposta not in (None, 'responder', 'validar'):
             raise ValueError('resposta=%r' % resposta)
-        quando = 

[truncado em 20000 de 31294 caracteres]

## 05/11 18:13 · sessao b58ebc44e738 · PC1-OTIMIZA-3 · censo · origem=Code

HASH LIDO = d1354f77cf4c7214728709ad948145994a0c0366

## A CASOS PELA REGRA

O hash identifica a cópia fornecida; a confirmação independente do HEAD por Git está **[nao verificado]**. As referências a `avais.txt` e `otimiza3_reprovacoes.txt` abaixo correspondem às linhas dos anexos da pergunta; `pd_otimiza3.r` não foi localizado na cópia **[nao verificado]**.

O contrato é censo factual, por faceta: **contagem quando o universo foi varrido; “nao varrido” quando não foi**, preservando achados avulsos. Não autoriza concluir automaticamente que um sítio pode ser apagado pela L-111. A referência dos casos é a regra, não o resultado atual. (`bin/quem_chama.py:4`, `bin/quem_chama.py:8`, `app/docs/LEIS.md:136`, `app/docs/LEIS.md:137`)

| Alvo + estado + ato | Resposta exigida pela regra |
|---|---|
| Módulo Python e função alterada; único chamador Python faz importação explícita da função; consultar o diff. | Faceta arquivo: **1 achado em 1 arquivo**; função: **1 achado em 1 arquivo**, desde que não haja outra classe de chamador sem cobertura. (`bin/quem_chama.py:405`, `bin/quem_chama.py:414`, `bin/quem_chama.py:690`) |
| Mesmo módulo; chamador importa somente função **não alterada**. | Achado pertence exclusivamente à **faceta arquivo**; não pode reaparecer sob o símbolo alterado, mesmo com o texto “importa de”. (`bin/quem_chama.py:409`, `bin/quem_chama.py:411`; reprovação: anexo `otimiza3_reprovacoes.txt:2`) |
| `datas_cravadas_prod`; chamada em Python dentro de `manage.py shell --command`; consultar o diff que a toca. | **“nao varrido”**, nunca zero de símbolo. O chamador está materializado na cópia; o aval proíbe implementar varredura de Python em `.sh`. (`bin/placar_code.sh:384`; anexos `avais.txt:4`, `otimiza3_reprovacoes.txt:2`) |
| Função chamada somente por heredoc, opção `-c` ou comando Django dentro de shell; sem importador `.py` e sem `__init__.py`. | Mesma resposta: **“nao varrido”**. A ausência de importador Python não pode transformar falta de cobertura em zero. (`bin/quem_chama.py:510`, `bin/quem_chama.py:673`; anexo `otimiza3_reprovacoes.txt:2`) |
| Módulo importado no shell pelo pacote, com apelido; não aparece seu nome pontilhado completo. | Também **“nao varrido”**. Marcar somente módulos encontrados pelas citações atuais deixa essa borda descoberta. Há essa forma em leitores reais. (`bin/placar_code.sh:484`, `bin/placar_code.sh:508`, `bin/gate_cobranca.sh:50`) |
| Script sem extensão, com shebang, contendo a mesma chamada Python. | Mesma declaração de cobertura incompleta: esse arquivo integra a classe de chamadores shell da ferramenta. (`bin/quem_chama.py:612`, `bin/quem_chama.py:673`) |
| Python sem chamador shell na ponta, somente leitores reconhecidos, nenhum uso da função alterada. | **Zero pode ser afirmado**, se a cobertura restante estiver completa; D enumera situações em que ainda não está. (`bin/quem_chama.py:11`, `bin/quem_chama.py:690`) |
| Estático carregado por template; template incluído; biblioteca carregada e filtro/tag usado. | Cada referência reconhecida entra na faceta correspondente com **N achados**, sem substituir uso de filtro por simples carregamento da biblioteca. (`bin/quem_chama.py:535`, `bin/quem_chama.py:549`, `bin/quem_chama.py:559`; anexo `otimiza3_reprovacoes.txt:1`) |
| Filtro usado como primeiro filtro de bloco de template, com biblioteca carregada. | Deve entrar como **uso do filtro**; com um único bloco, sem outros leitores e cobertura completa, **N=1**. A expressão atual reconhece somente o prefixo de filtro com barra vertical. (`bin/quem_chama.py:53`, `bin/quem_chama.py:560`; anexo `otimiza3_reprovacoes.txt:2`) |
| Referência dentro de comentário de template multilinha ou comentário/docstring excluído pelo contrato. | Não constitui chamador; referências posteriores mantêm sua linha original. O tratamento multilinha já existe; falta prendê-lo no selo. (`bin/quem_chama.py:20`, `bin/quem_chama.py:520`, `bin/tests/test_quem_chama.sh:61`) |
| Método de instância, setting, descoberta Django, teste ou tipo documental fora da lista positiva. | **“nao varrido”**, inclusive com zero achado avulso; zero avulso não fecha universo. (`bin/quem_chama.py:295`, `bin/quem_chama.py:315`, `bin/quem_chama.py:319`, `bin/quem_chama.py:697`) |
| Símbolo apagado/renomeado; arquivo vazio, binário ou só modo alterado. | O alvo permanece no censo; símbolos antigos são confrontados com leitores da ponta. Cobertura incompleta continua declarada. (`bin/quem_chama.py:142`, `bin/quem_chama.py:158`, `bin/quem_chama.py:650`) |
| Duas consultas concorrentes; troca de dia 21→20 ou passagem da meia-noite; mesmo par de revisões resolvidas. | O censo deve continuar determinado pelo conteúdo dessas revisões, sem julgamento temporal de folha. Escrita concorrente no mesmo arquivo de saída é outra questão, indicada em D. (`bin/quem_chama.py:109`, `bin/quem_chama.py:619`, `bin/quem_chama.py:624`) |
| Código sobre vínculo trocado ou célula sem ata é alterado. | A ferramenta devolve leitores dos arquivos/símbolos; não decide vínculo, competência ou validade da ata. Esses julgamentos pertencem às autoridades de domínio. (`bin/quem_chama.py:4`, `app/docs/JUIZES.md:35`, `app/docs/JUIZES.md:55`, `app/docs/JUIZES.md:90`) |

As reprovações publicadas exigem fechar **a classe**, corrigir a negativa que não prende faceta e respeitar **não analisar Python em `.sh` + teto de duas voltas Opus para instrumento**; a quantidade de voltas ainda disponível está **[nao verificado]**. (Anexos `otimiza3_reprovacoes.txt:1`, `otimiza3_reprovacoes.txt:2`, `avais.txt:4`) [nao verificado]

## B SITIOS

A fonte da correção é a classificação de cobertura **antes da apresentação**, preservando o contador único. Alterar somente `FORA` deixa a condição que imprime zero intacta. (`bin/quem_chama.py:637`, `bin/quem_chama.py:656`, `bin/quem_chama.py:691`)

| Sítio / dado | Escritor, funções lidas e consumidores |
|---|---|
| Contrato positivo e limites | `VARRIDOS` nasce em `bin/quem_chama.py:38`; `_faceta` o consulta em `bin/quem_chama.py:691`; `render` o apresenta em `bin/quem_chama.py:705`. `FORA` nasce em `bin/quem_chama.py:65` e é apresentado em `bin/quem_chama.py:706`. |
| Universo imutável da consulta | `lados`, `linhas_mudadas`, `tocados_do_diff`, `_git` e `_ler_rev` alimentam `quem_chama`. A lista e os conteúdos vêm da ponta; a base fornece o removido. (`bin/quem_chama.py:109`, `bin/quem_chama.py:142`, `bin/quem_chama.py:157`, `bin/quem_chama.py:619`) |
| Identificação de chamador shell | `tipo_chamador` reconhece `.sh` e script sem extensão com shebang; lê `_CONFIG_EXT`. O despacho atual chama somente `varrer_linhas` para shell. (`bin/quem_chama.py:72`, `bin/quem_chama.py:599`, `bin/quem_chama.py:673`) |
| Tipo do arquivo | `tipo_do_arquivo` lê caminho, extensão, `modulo_de` e `_COMANDO`; o tipo final é armazenado no alvo e lido por `render`. (`bin/quem_chama.py:284`, `bin/quem_chama.py:637`, `bin/quem_chama.py:658`, `bin/quem_chama.py:713`) |
| Símbolos e respectivos tipos | `simbolos_mudados` lê `_parse`, `_libs_de_template`, `_registro` e `_nome_decorador`; `_tipo_simbolo` lê convenções, decoradores e registro. A atribuição de `info['tipo']` fica em um sítio, consumido por `render`. (`bin/quem_chama.py:243`, `bin/quem_chama.py:311`, `bin/quem_chama.py:650`, `bin/quem_chama.py:656`, `bin/quem_chama.py:715`) |
| Biblioteca, nomes e índices | `lib`, `nomes`, `modulo`, `template`, `estatico` e `artefatos` compõem o alvo; `Alvos` indexa citações, módulos e bibliotecas. A declaração de cobertura não deve destruir esses índices nem os achados avulsos. (`bin/quem_chama.py:363`, `bin/quem_chama.py:633`, `bin/quem_chama.py:657`) |
| Leitores Python | `varrer_py` lê `_parse`, `_prosa`, `_resolve`, `_cadeia`, `_sim` e índices de `Alvos`; `varrer_proprio` acrescenta leitura interna dos símbolos. (`bin/quem_chama.py:390`, `bin/quem_chama.py:442`, `bin/quem_chama.py:684`) |
| Leitores shell/config | `_codigo_sh` usa `_corta_comentario_sh`; `varrer_linhas` emite apenas faceta arquivo; `artefatos_sh` acrescenta leitores de artefatos escritos. (`bin/quem_chama.py:479`, `bin/quem_chama.py:510`, `bin/quem_chama.py:573`, `bin/quem_chama.py:645`) |
| Leitor de template a corrigir | `varrer_template` lê `_sem_comentario_template`, `_INCLUDE`, `_STATIC`, `_LOAD` e registros dos símbolos; a regex de uso do filtro está em `bin/quem_chama.py:560`. |
| Achados e contadores | Achados são reunidos em conjuntos por `(arquivo, símbolo)` e ordenados. `_faceta` é o escritor das contagens; resumo e mensagem final leem essas contagens. (`bin/quem_chama.py:662`, `bin/quem_chama.py:681`, `bin/quem_chama.py:687`, `bin/quem_chama.py:694`, `bin/quem_chama.py:718`, `bin/quem_chama.py:748`) |
| Saída e integração externa | `main` chama censo e apresentação; grava opcionalmente ou imprime. O selo escolhe a ferramenta por `QUEM_CHAMA_PY`; a régua executa os selos de host. (`bin/quem_chama.py:733`, `bin/quem_chama.py:737`, `bin/quem_chama.py:742`, `bin/tests/test_quem_chama.sh:25`, `bin/regua.sh:138`) |
| Selo a tocar | Negativas sem faceta: `bin/tests/test_quem_chama.sh:159` e `bin/tests/test_quem_chama.sh:167`; expectativas de cobertura: `bin/tests/test_quem_chama.sh:218`, `bin/tests/test_quem_chama.sh:223`, `bin/tests/test_quem_chama.sh:224`, `bin/tests/test_quem_chama.sh:231`, `bin/tests/test_quem_chama.sh:279`; invariante e determinismo: `bin/tests/test_quem_chama.sh:239`, `bin/tests/test_quem_chama.sh:257`. |

**Juiz registrado:** não foi encontrada autoridade de censo de chamadores em `app/core/juizes.py` ou `app/docs/JUIZES.md` **[nao verificado]**. O registro trata decisões de domínio; o instrumento declara explicitamente “lista é fato, não juiz”. Portanto o diff não registra juiz novo. (`app/core/juizes.py:3`, `app/docs/JUIZES.md:7`, `bin/quem_chama.py:6`)

**LEI-AKITA:** origem = classificação em `quem_chama`; testemunha = revisões resolvidas; RED = casos shell e faceta no selo existente; quem-mais-lê = tabela acima; juízes novos = 0; padrão = n/a (instrumento). (`CLAUDE.md:17`, `CLAUDE.md:62`, `CLAUDE.md:68`, `bin/quem_chama.py:619`)

## C DIFF PROPOSTO EM TEXTO

A proposta declara conservadoramente incompletas as facetas Python quando existir chamador shell na ponta. Assim cobre também importação por pacote e script sem extensão, sem analisar payload Python. Os achados continuam disponíveis como avulsos; a contagem continua em `_faceta`. **Não constitui certificação universal: permanecem os contraexemplos de D.** (`bin/quem_chama.py:612`, `bin/quem_chama.py:656`, `bin/quem_chama.py:697`)

```diff
--- a/bin/quem_chama.py
+++ b/bin/quem_chama.py
@@ -65,1 +65,3 @@
-FORA = ('FORA (dentro dos tipos varridos): nome montado em execucao (getattr, string concatenada, include/static '
+FORA = ('FORA (dentro dos tipos varridos): Python embutido em shell nao e analisado; havendo chamador shell '
+        'na ponta, as facetas Python de VARRIDOS saem nao varridas, com achados avulsos; '
+        'nome montado em execucao (getattr, string concatenada, include/static '
@@ -329,1 +331,8 @@
+def _tipo_cobertura(tipo, ha_shell):
+    """Sem analisar payload shell, nao se afirma cobertura completa de faceta Python."""
+    if ha_shell and tipo in ('py', 'py-command', 'py-templatetag', 'simbolo-de-topo', 'filtro/tag'):
+        return tipo + '; Python em shell nao varrido'
+    return tipo
+
+
 def _prosa(arv):
@@ -560,1 +569,1 @@
-                rx = re.compile(r'\|\s*' + re.escape(nome) + r'(?![\w])' if papel == 'filtro'
+                rx = re.compile(r'(?:\|\s*|{%\s*filter\s+)' + re.escape(nome) + r'(?![\w])' if papel == 'filtro'
@@ -625,1 +634,2 @@
     na_base = _ler_rev(raiz, base_rev, tocados)
+    ha_shell = any(tipo_chamador(r, f) == 'sh' for r, f in na_ponta.items())
@@ -656,3 +666,4 @@
-            info['tipo'] = _tipo_simbolo(rel, tipo, info)
+            info['tipo'] = _tipo_cobertura(_tipo_simbolo(rel, tipo, info), ha_shell)
         lib = base[:-3] if tipo == 'py-templatetag' else None
-        alvos.append({'rel': rel, 'existe': fonte is not None, 'modulo': mod, 'simbolos': simb, 'tipo': tipo,
+        alvos.append({'rel': rel, 'existe': fonte is not None, 'modulo': mod, 'simbolos': simb,
+                      'tipo': _tipo_cobertura(tipo, ha_shell),
--- a/bin/tests/test_quem_chama.sh
+++ b/bin/tests/test_quem_chama.sh
@@ -45,1 +45,6 @@
+# Alvo sem __init__.py e sem importador .py: so payload shell.
+printf 'def f():\n    return 1\n' > app/m/so_shell.py
+printf "python3 <<'PAYLOAD'\nfrom m.so_shell import f\nf()\nPAYLOAD\n" > bin/heredoc.sh
+printf "python3 -c 'from m import so_shell as L; L.f()'\n" > bin/inline.sh
+printf 'python manage.py shell --command "from m.so_shell import f; f()"\n' > bin/manage_shell.sh
 # leitores
@@ -107,1 +112,3 @@
 printf '{{ a|rot }}\n{%% tg %%}\n' > app/m/templates/m/sem_load.html
+printf '{%% load m_tags %%}\n{%% filter rot %%}a{%% endfilter %%}\n' > app/m/templates/m/bloco_filter.html
+printf '{%% load m_tags %%}\n{#\n{{ a|rot }}\n#}\n{{ a|rot }}\n' > app/m/templates/m/coment_tag.html
@@ -118,1 +125,2 @@
 git add -A && git commit -qm base [nao verificado]
+printf 'def f():\n    return 2\n' > app/m/so_shell.py
@@ -150,1 +158,12 @@
 _naotem(){ grep -qF -- "$2" "$T/f.txt" && _falha "$1: MORDE -- [$2] entrou"; }
+_so_arquivo() {
+  awk -v p="  $1:" '
+    /^== / {alvo=$0; faceta=""}
+    /^   (arquivo|simbolo )/ {faceta=$0}
+    index($0,p)==1 {
+      n++
+      if (alvo !~ /^== app\/m\/lib[.]py / || faceta !~ /^   arquivo/) ruim=1
+    }
+    END {exit (ruim || !n)}
+  ' "$T/f.txt" || _falha "$2: achado fora da faceta arquivo"
+}
@@ -159,1 +178,1 @@
-grep -F "  app/m/usa_outra.py:" "$T/f.txt" | grep -qv "^  app/m/usa_outra.py:1: importa de m.lib$" && _falha "simbolo-parado: MORDE -- usa_outra entrou fora da faceta arquivo"
+_so_arquivo app/m/usa_outra.py simbolo-parado
@@ -167,1 +186,1 @@
-grep -F "  app/m/rel_outra.py:" "$T/f.txt" | grep -qv "^  app/m/rel_outra.py:1: importa de m.lib$" && _falha "relativo-parado: MORDE -- rel_outra entrou fora da faceta arquivo"
+_so_arquivo app/m/rel_outra.py relativo-parado
@@ -218,1 +237,1 @@
-_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "simbolo rot_f [filtro/tag]:" r4-filtro-varrido
+_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "simbolo rot_f: nao varrido (filtro/tag; Python em shell nao varrido)" r4-filtro-varrido
@@ -223,2 +242,2 @@
-_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "simbolo nunca_usado [filtro/tag]: 0 chamador(es)" r4-zero-so-varrido
-_tem_sec "$T/f.txt" app/m/cls.py "simbolo K [simbolo-de-topo]:" r4-classe-topo
+_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "simbolo nunca_usado: nao varrido (filtro/tag; Python em shell nao varrido)" r4-zero-so-varrido
+_tem_sec "$T/f.txt" app/m/cls.py "simbolo K: nao varrido (simbolo-de-topo; Python em shell nao varrido)" r4-classe-topo
@@ -231,1 +250,1 @@
-_tem_sec "$T/f.txt" app/m/lib.py "simbolo mudada [simbolo-de-topo]:" r4-topo-varrido
+_tem_sec "$T/f.txt" app/m/lib.py "simbolo mudada: nao varrido (simbolo-de-topo; Python em shell nao varrido)" r4-topo-varrido
@@ -251,1 +270,7 @@
 _invariante "$T/f.txt" fixture
+_tem_sec "$T/f.txt" app/m/so_shell.py "arquivo: nao varrido (py; Python em shell nao varrido)" shell-arquivo
+_tem_sec "$T/f.txt" app/m/so_shell.py "simbolo f: nao varrido (simbolo-de-topo; Python em shell nao varrido)" shell-simbolo
+_naotem_sec "$T/f.txt" app/m/so_shell.py "0 chamador" shell-nunca-zero
+_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "  app/m/templates/m/bloco_filter.html:2: usa o filtro |rot" bloco-filter
+_tem_sec "$T/f.txt" app/m/templatetags/m_tags.py "  app/m/templates/m/coment_tag.html:5: usa o filtro |rot" comentario-linhas
+_naotem_sec "$T/f.txt" app/m/templatetags/m_tags.py "  app/m/templates/m/coment_tag.html:3:" comentario-multilinha
@@ -260,1 +285,7 @@
+# Sem shell na ponta, o caso completo continua podendo afirmar zero.
+git -C "$F" rm -q -- '*.sh' && git -C "$F" commit -qm sem-shell [nao verificado]
+python3 "$Q" ponta~2..HEAD --raiz "$F" --saida "$T/sem_shell.txt" >/dev/null 2>"$T/err" || _falha "sem-shell rc!=0"
+_tem_sec "$T/sem_shell.txt" app/m/so_shell.py "simbolo f [simbolo-de-topo]: 0 chamador(es)" zero-completo
+_tem_sec "$T/sem_shell.txt" app/m/templatetags/m_tags.py "simbolo nunca_usado [filtro/tag]: 0 chamador(es)" filtro-zero-completo
+
 # smoke da arvore viva (so rc; nao prova comportamento)
@@ -279,1 +310,1 @@
-  _tem_sec "$T/real_9e12489c.txt" app/chamados/templatetags/chamados_tags.py "simbolo espera [filtro/tag]:" r4-real-tags
+  _tem_sec "$T/real_9e12489c.txt" app/chamados/templatetags/chamados_tags.py "simbolo espera: nao varrido (filtro/tag; Python em shell nao varrido)" r4-real-tags
```

A sintaxe da proposta foi conferida em memória por AST e `bash -n`; execução comportamental da proposta, mutantes, Ruff e suíte estão **[nao verificado]**. O selo existente cria e apaga arquivos temporários, portanto não foi executado nesta chamada de leitura. (`bin/tests/test_quem_chama.sh:24`, `bin/tests/test_quem_chama.sh:28`)

## D CONTRA-EXEMPLOS

1. **Declaração conservadora reduz a cobertura publicada.** Um shell sem qualquer Python já torna incompletas todas as facetas Python antes varridas; achados verdadeiros passam a avulsos. Isso evita o falso zero da classe shell, mas muda a utilidade dos relatórios históricos. É decisão de desenho a explicitar antes do construtor, conforme o complemento do piloto. (`bin/quem_chama.py:599`, `bin/quem_chama.py:697`; anexo `avais.txt:3`)

2. **Python ilegível pelo AST ainda pode produzir falso zero, sem shell na ponta.** Um chamador com sintaxe não aceita pelo interpretador é convertido em ausência de achados. C não corrige isso. (`bin/quem_chama.py:186`, `bin/quem_chama.py:393`)

3. **Declaração sob condicional pode desaparecer do inventário de símbolos.** A coleta percorre diretamente módulo e classes; não desce em corpo de condicional para descobrir a função alterada. A ausência dessa faceta não recebe declaração própria de cobertura incompleta. (`bin/quem_chama.py:257`, `bin/quem_chama.py:266`, `bin/quem_chama.py:277`)

4. **Python embutido em configuração não é coberto pela cura shell.** Em árvore sem shell, uma unidade de serviço pode executar Python importando módulo pelo pacote e usando apelido; configuração passa apenas pela busca textual. C mantém a possibilidade de zero falso nessa classe. (`bin/quem_chama.py:610`, `bin/quem_chama.py:678`, `bin/quem_chama.py:378`)

5. **Nome de arquivo contendo quebra de linha é omitido da leitura de conteúdo.** A lista de alvos usa separador NUL, mas `_ler_rev` exclui esses caminhos. Um chamador assim pode faltar sem declaração específica. C não altera essa fronteira. (`bin/quem_chama.py:90`, `bin/quem_chama.py:144`, `bin/quem_chama.py:668`)

6. **Concorrência na saída permanece sem proteção.** Duas consultas podem abrir o mesmo destino para escrita; o código não apresenta publicação atômica nem exclusão mútua. Isso é diferente da leitura por revisões imutáveis e não é resolvido por C. (`bin/quem_chama.py:624`, `bin/quem_chama.py:741`, `bin/quem_chama.py:742`)

7. **Zero da ferramenta não é zero de produção.** Leitores em testes entram no universo de chamadores Python; a ferramenta não separa essa contagem para executar a decisão da L-111. Tampouco um achado textual prova execução. (`bin/quem_chama.py:606`, `bin/quem_chama.py:672`, `bin/quem_chama.py:690`, `app/docs/LEIS.md:137`)

Quem mais lê o que C muda: `render`, resumo e mensagem final consomem os tipos/contadores; o selo consome os rótulos e referências; a régua consome o resultado do selo. Consumidores externos dos relatórios gravados não foram demonstrados no dado fornecido **[nao verificado]**. (`bin/quem_chama.py:713`, `bin/quem_chama.py:715`, `bin/quem_chama.py:718`, `bin/quem_chama.py:748`, `bin/tests/test_quem_chama.sh:148`, `bin/regua.sh:140`)

**O diff atende ao recorte shell e à fila explícita de filtro/faceta; os contraexemplos 2–5 impedem declarar cumprido o contrato universal “nunca zero falso”.** (`bin/quem_chama.p

[truncado em 20000 de 20104 caracteres]

## 05/11 18:12 · sessao 9134d244b2b0 · PC1-FIO-MUDO · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

## A — CASOS PELA REGRA

O hash identifica a cópia indicada; correspondência com commit Git **[não verificado]**. O universo **494** está no aval, mas o dado fornecido não contém seus registros: distribuição pelos cinco atos, exclusões legítimas e quantidade efetivamente defeituosa **[não verificado]**. (`app/docs/PROMPTS.md:1875`; `app/docs/PROMPTS.md:1882`)

Os casos abaixo são cenários de aceite, não registros encontrados em produção. A referência é a regra aplicada ao cadastro e aos fatos; o comportamento antigo não constitui gabarito. (`app/docs/LEIS.md:136`)

| Caso | Cadastro + estado + ato | Resposta pela regra |
|---|---|---|
| A1 — nascimento | Ativo, vínculo válido no dia, ata com marco vencido faltante, sem pergunta; abrir `batida_ausente`. | Entregar a pergunta pelo evento da abertura, lendo a dívida da célula; varredura apenas vigia. (`app/docs/PROMPTS.md:1875`; `CLAUDE.md:168`; `CLAUDE.md:169`) |
| A2 — nascimento concorrente | Mesmo cadastro e fato de A1; duas aberturas simultâneas. | Mesmo estado final, uma linha de trilha e nenhuma duplicata. (`CLAUDE.md:175`) |
| A3 — reabrir | Dívida persistente, chamado encerrado, nenhuma pergunta restaurável; reabrir. | A reabertura precisa conservar fio vivo ou ação administrativa efetivamente nomeada; gravar um status vivo não basta. (`app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:253`; `app/chamados/services/vigia_fio.py:283`) |
| A4 — retratar duplicata | A perde sua última pergunta; a canônica do mesmo fato está em B. | Preservar a cobrança alcançável do fato, sem fabricar uma pergunta para cada chamado duplicado. (`CLAUDE.md:175`; `app/docs/CORTES.md:253`; `app/docs/CORTES.md:404`) |
| A5 — duplicata respondida | Pergunta contém resposta discordante ainda sem decisão humana; retratar duplicata. | Preservar a resposta para decisão; o carimbo de identidade não basta para declarar a conversa encerrada. (`app/chamados/models.py:2617`; `app/chamados/juizes.py:1150`) |
| A6 — validação integral | Único marco devido; validação e materialização resolvem o fato. | Não perguntar novamente para zerar contador; cobrança sem premissa encerra por lastro. (`app/docs/CORTES.md:253`; `app/chamados/models.py:681`) |
| A7 — validação com resto | Saída e intervalo faltantes; validar/materializar somente a saída. | A saída atendida não volta a ser cobrada; o intervalo conserva pergunta ou ação administrativa nomeada. Resolver um marco não quita outro. (`app/chamados/juizes.py:1173`; `app/docs/CORTES.md:253`) |
| A8 — plantio recusado | Resposta validada, materialização recusada e dívida persistente. | Não produzir quitação fictícia nem retirar o dia da decisão humana; não concluir que basta emitir outra pergunta do mesmo fato. (`app/docs/CORTES.md:253`; `app/chamados/services/validacao.py:413`; `app/chamados/models.py:2388`) |
| A9 — reapontar | A perde sua última resolvedora para B. | Preservar a alcançabilidade do dia; examinar a origem e o destino, sem duplicar o fato. (`app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:404`; `CLAUDE.md:175`) |
| A10 — reapontar × encerrar | Pergunta viva; costura e encerramento do destino concorrem. | Não terminar com pergunta viva inacessível sob âncora terminal; preservar a porta ou recusar a costura com motivo. (`app/docs/CORTES.md:404`; `app/chamados/services/fio.py:743`) |
| A11 — morte por leitura | A última pergunta morre sem novo carimbo, por mudança dos insumos que C6 lê. | Consultar C6 e tratar a consequência no evento pertinente; C6 não vira escritor. Juiz novo implica PAREI. (`app/core/juizes.py:266`; `app/docs/PROMPTS.md:1882`; `CLAUDE.md:170`) |
| A12 — resposta e luz | Batida acende o marco perguntado; resposta concorda pela régua existente e o dia não acusa. | A pendência pode cessar. Discordância ou ausência de comparação mantém decisão humana pendente. (`app/chamados/juizes.py:1090`; `app/chamados/juizes.py:1096`; `app/chamados/juizes.py:1159`) |
| A13 — limite exato | Marco às 18h, margem vigente de 30 minutos; observar 18h29min59s e 18h30. | Não cobrar antes do vencimento; na igualdade, o marco venceu. Comparar instante, não somente data. (`CLAUDE.md:185`; `app/ponto/services/cartorio.py:828`; `app/ponto/services/cartorio.py:846`) |
| A14 — cross-meia-noite | Turno 22h–06h ancorado ontem; observar 00h01 e 06h10. | A virada civil não encerra o turno; às 06h10 a saída ainda não venceu a margem de 30 minutos. (`CLAUDE.md:185`; `app/chamados/services/fabrica_da_celula.py:18`) |
| A15 — janela 21→20 | Empresa com corte 21; trancar competência 10; comparar fatos de 20/10 e 21/10. | A competência 10 cobre 21/09–20/10; 21/10 pertence à próxima. Usar o corte cadastrado da empresa, sem fixar 21 universalmente. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:24`; `app/ponto/janelas.py:54`) |
| A16 — tranca | Última pergunta pertence a competência trancada e morre por leitura. | Não contornar a tranca reemitindo pergunta; sair da fila não planta batida nem absolve o fato. (`app/chamados/juizes.py:1138`; `app/chamados/juizes.py:1142`) |
| A17 — toque posterior | Dia trancado; admin realiza o toque humano previsto depois da tranca. | Aplicar a reversibilidade existente; resposta do colaborador ou cron não substitui toque humano. (`app/chamados/juizes.py:1033`; `app/chamados/juizes.py:1054`) |
| A18 — vínculo trocado | Célula gerada pelo vínculo antigo, dentro da vigência; outro vínculo está ativo hoje. | Vale a geradora daquele dia. Fora da vigência dela, não substituir silenciosamente pelo vínculo atual: o caso pertence ao cadastro. (`app/escala/alimentacao.py:231`; `app/escala/alimentacao.py:272`) |
| A19 — célula sem ata | Escala vigente, célula sem ata; tentar perguntar um marco. | Esperar a ata e denunciar sua ausência; não fabricar certeza de marco faltante. `trabalha=None` também não significa folga. (`app/chamados/services/disputa_emissao.py:818`; `app/chamados/services/disputa_emissao.py:842`; `app/core/juizes.py:139`) |
| A20 — parcial sem período | Ausência parcial sem período declarado, dia pendente. | Encaminhar à lista administrativa, sem pergunta, pai ou push; um chamado histórico pode permanecer vivo legitimamente. (`app/chamados/services/fabrica_da_celula.py:77`; `app/chamados/services/disputa_emissao.py:850`; `app/chamados/services/disputa_emissao.py:856`) |
| A21 — vivo legítimo | Registro sem ação, decisão do dia pendente, ausência aberta ou dono CADASTRO/BATIDA efetivamente na lista. | Excluir do universo defeituoso antes do RED, pela autoridade correspondente. `em_analise` sozinho não é ação nomeada. (`app/docs/PROMPTS.md:1882`; `app/chamados/services/painel.py:174`; `app/chamados/services/vigia_fio.py:274`) |
| A22 — hoje | Fato de hoje ainda no ciclo normal, mas algum marco pode já ter vencido. | Não antecipar o alarme de fio morto; a emissão pode cobrar os marcos já vencidos por instante. (`app/chamados/services/vigia_fio.py:420`; `app/chamados/services/fabrica_da_celula.py:8`) |
| A23 — histórico/exportada | Passado sem fato novo; separadamente, diferença reconhecida em competência exportada. | Não autorizar backfill geral nesta obra; conservar o gravado exportado e encaminhar a diferença pela porta pertinente. Exportação, isoladamente, não proíbe perguntar em dia de trabalho. (`app/docs/LEIS.md:125`; `app/docs/LEIS.md:145`; `app/chamados/services/fabrica_da_celula.py:167`) |

**Antes do RED:** os 494 precisam ser classificados, sem estreitar artificialmente `VIVOS` e sem converter toda ausência de pergunta em defeito. O juiz de alcance e os baldes de dono legítimo respondem perguntas diferentes: C17 exclui expressamente “Resolver dia”, que o vigia considera separadamente. Quantidades dessa partição **[não verificado]**. (`app/docs/PROMPTS.md:1882`; `app/chamados/juizes.py:1367`; `app/chamados/services/vigia_fio.py:283`)

## B — SÍTIOS

**Juiz novo não é necessário para julgar a morte por leitura:** C6 já responde sobre a pergunta; C15 reduz a coleção; C3 responde sobre premissa; C17 responde sobre alcance. O rótulo operacional “FIO MUDO C11=(i)” não substitui o C11 registrado, que trata de silêncio por ausência. (`app/core/juizes.py:263`; `app/core/juizes.py:266`; `app/core/juizes.py:271`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/docs/PROMPTS.md:1875`)

`JUIZES.md` é lookup; em divergência de endereço, manda o registro vivo. Isso não transforma o comportamento atual em gabarito dos casos. (`app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`; `app/docs/LEIS.md:136`)

| Pergunta | Autoridade existente |
|---|---|
| Estado vivo e transição | C1 `VIVOS`; C2 `decidir`. (`app/core/juizes.py:261`; `app/core/juizes.py:262`) |
| Premissa e encerramento | C3 `premissa_morta`; o fechamento do chamado passa por `reconciliar`, seus gates e o coletor `premissa_regularizada`. (`app/core/juizes.py:263`; `app/chamados/models.py:607`; `app/chamados/models.py:629`; `app/chamados/models.py:1202`) |
| Vida da pergunta | C6 `pergunta_viva`; cálculo em `_pergunta_viva_calc`. (`app/core/juizes.py:266`; `app/chamados/juizes.py:822`; `app/chamados/juizes.py:1135`) |
| Vida da coleção | C15 `fio_vivo`. (`app/core/juizes.py:275`; `app/chamados/juizes.py:1233`) |
| Alcance do dia | C17 `porta_alcancavel_do_dia`, compondo fila, verbo, pergunta no app e âncora. (`app/core/juizes.py:277`; `app/chamados/juizes.py:1341`) |
| Ausência, veto e dia sem trabalho | C11 `reconciliar_por_ausencia`; C16 `motivo_do_veto`; C18 `dia_sem_trabalho`. (`app/core/juizes.py:271`; `app/core/juizes.py:276`; `app/core/juizes.py:278`) |
| Vínculo, competência e vencimento | `vinculo_do_dia`; janela da empresa; `marcos_vencidos` com marcos encadeados. (`app/escala/alimentacao.py:228`; `app/ponto/janelas.py:54`; `app/ponto/services/cartorio.py:831`; `app/docs/JUIZES.md:24`) |

**Fontes e portas que a obra precisa considerar:**

| Sítio | Escrita/leitura e alcance |
|---|---|
| `ChamadoColaborador.abrir` | Trava colaborador, deduplica, pode renascer encerrado e cria chamado. O evento explícito de nascimento cobre somente `MODULOS_DE_TURNO`, sem `batida_ausente`. (`app/chamados/models.py:863`; `app/chamados/models.py:882`; `app/chamados/models.py:932`; `app/chamados/models.py:938`; `app/chamados/reconciliador.py:336`) |
| `ChamadoColaborador.criar` | Outra porta de nascimento; resolve célula/veto e termina após criar/trilhar, sem emissão explícita equivalente. (`app/chamados/models.py:1015`; `app/chamados/models.py:1026`; `app/chamados/models.py:1036`; `app/chamados/models.py:1048`) |
| Detector, grade e C2 | Emissores de `batida_ausente`. Detector tenta fio depois da abertura, absorvendo falha; grade retorna sem emissão direta; C2 reaponta pergunta existente. (`app/ponto/management/commands/detectar_ausencias.py:693`; `app/ponto/management/commands/detectar_ausencias.py:787`; `app/ponto/management/commands/detectar_ausencias.py:802`; `app/ponto/management/commands/reconciliar_grade.py:507`; `app/ponto/management/commands/reconciliar_grade.py:531`; `app/chamados/reconciliador.py:879`; `app/chamados/reconciliador.py:922`) |
| Sinal do chamado | Julga célula em criação/mudança de estado, mas está em `engolir` e retorna durante julgamento. Não prova entrega universal do fio. (`app/chamados/signals.py:164`; `app/chamados/signals.py:172`; `app/chamados/signals.py:174`) |
| Reabrir | Grava vivo e ressuscita candidatas existentes; não cria pergunta inexistente. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1423`) |
| Duplicata e reapontamento | `retratar_duplicata` carimba identidade; `reapontar_resolvedora` escreve destino/data e lavra, com guarda concorrente do destino. A perda no chamado antigo precisa ser considerada. (`app/chamados/services/fio.py:878`; `app/chamados/services/fio.py:891`; `app/chamados/services/fio.py:784`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:845`) |
| Validação/materialização | Assinatura, tentativa de plantio e devolução da recusa são etapas distintas; destinos/clones já reconciliam pela porta do chamado. (`app/chamados/services/validacao.py:127`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:422`; `app/chamados/services/validacao.py:430`; `app/chamados/services/materializacao.py:728`) |
| Morte por leitura — fatos | C6 lê tranca, decisão da célula, marco atendido, pendência da ata e janela. A cobertura não se limita a alterações da própria pergunta. (`app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1148`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1195`; `app/chamados/juizes.py:1197`) |
| Morte por leitura — tranca | Há fonte própria: `disparar_no_ato` e encerramento pela competência, sem novo carimbo da pergunta. (`app/chamados/services/competencia_trancada.py:178`; `app/chamados/services/competencia_trancada.py:276`) |
| Morte por leitura — relógio | `lavrar_perguntas_stale` já trata lavra vencida; seu julgamento de materialização não equivale a C6. Não basta acrescentar efeito ao getter. (`app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/chamados/veredito_lavrado.py:219`; `app/config/crons.py:390`) |
| Fábrica e cartório | A fábrica fornece seleção e escrita canônicas; o cartório só a chama para células julgadas naquela passada. (`app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/services/fabrica_da_celula.py:129`; `app/chamados/services/fabrica_da_celula.py:145`; `app/ponto/services/cartorio.py:981`; `app/ponto/services/cartorio.py:995`) |
| Evento e consumidor | Escritor existente `emitir_evento_celula`; consumidor existente relê fatos, retrata chamados e encerra órfãs, mas atualmente não sana FIO MUDO pela fábrica. (`app/ponto/portas/celula.py:664`; `app/chamados/reconciliador.py:1283`; `app/chamados/reconciliador.py:1326`; `app/chamados/reconciliador.py:1329`) |
| Comando e agenda | O ramo FIO MUDO ainda seleciona alvos e escreve em `--apply`; também continua registrado como juiz por varredura. Sua retirada exige cobertura pelos eventos, não apenas trocar argumentos do cron. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141`; `app/chamados/management/commands/disparar_perguntas_competencia.py:268`; `app/config/crons.py:1495`; `app/config/crons.py:2007`) |

**Campos envolvidos — escritores e leitores comprovados:**

| Campos | Escritores | Leitores |
|---|---|---|
| Chamado: identidade, módulo, contexto, célula | Nascimento por `abrir/criar`; contexto por `anotar`, `renovar`, `retirar`, `fundir`. (`app/chamados/models.py:913`; `app/chamados/models.py:1048`; `app/chamados/services/contexto.py:62`; `app/chamados/services/contexto.py:81`; `app/chamados/services/contexto.py:106`; `app/chamados/services/contexto.py:128`) | Dia do fato, fábrica, disparador e vigia. (`app/chamados/signals.py:178`; `app/chamados/services/disputa_emissao.py:683`; `app/chamados/management/commands/disparar_perguntas_competencia.py:150`; `app/chamados/services/vigia_fio.py:318`) |
| Chamado: estado e encerramento | `_gravar_estado`, pelas portas do modelo. (`app/chamados/models.py:195`; `app/chamados/models.py:620`; `app/chamados/models.py:1382`) | Dedup, sinais, fábrica, fila, gates concorrentes e consumidor. (`app/chamados/models.py:867`; `app/chamados/signals.py:169`; `app/chamados/services/disputa_emissao.py:691`; `app/chamados/services/painel.py:186`; `app/chamados/services/fio.py:764`; `app/chamados/reconciliador.py:1172`) |
| Chamado: silêncio | `arquivo.silenciar/levantar`; ausência usa essas portas. (`app/chamados/services/arquivo.py:141`; `app/chamados/services/arquivo.py:176`; `app/core/juizes.py:271`) | Fila e alimentação de alcance. (`app/chamados/services/painel.py:186`; `app/chamados/juizes.py:1282`) |
| Pergunta: identidade, célula, destino e data | `PerguntaDisputa.abrir`, inclusive reapontamento da identidade existente; `reapontar_resolvedora`. (`app/chamados/models.py:2387`; `app/chamados/models.py:2424`; `app/chamados/models.py:2439`; `app/chamados/services/fio.py:845`) | C6, identidade do fato, guarda da costura, fábrica e alcance. (`app/chamados/juizes.py:811`; `app/chamados/models.py:2370`; `app/chamados/services/fio.py:742`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/juizes.py:1382`) |
| Pergunta: resposta e instante | Resposta humana, automática, recomposição por trilha, correção e `carimbar(resposta=...)`. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/disputa_emissao.py:1362`; `app/chamados/services/disputa_emissao.py:1417`; `app/chamados/services/disputa_emissao.py:1434`; `app/chamados/models.py:2576`) | C6, validação, app e relatórios. (`app/chamados/juizes.py:1150`; `app/chamados/services/validacao.py:363`; `app/chamados/services/pergunta_no_app.py:53`; `app/chamados/relatorios_disputa.py:18`) |
| Pergunta: validação | Assinatura/devolução, resposta nova, correção, ressuscitação e modalidade do carimbo. (`app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`; `app/chamados/models.py:2580`) | C6, estado, validação e materialização. (`app/chamados/juizes.py:1136`; `app/chamados/models.py:2490`; `app/chamados/services/validacao.py:366`; `app/chamados/services/validacao.py:422`) |
| Pergunta: materialização, autor e via | `_escrever_carimbo` pelas portas; descarimbo em lote; troca de via em `encerrar_por_fato_no_espelho`. (`app/chamados/models.py:2545`; `app/chamados/models.py:2551`; `app/chamados/models.py:2614`; `app/chamados/models.py:2635`; `app/chamados/models.py:2644`; `app/chamados/models.py:2662`) | C6, seleção histórica da fábrica, ressuscitação e diagnóstico. (`app/chamados/juizes.py:1171`; `app/chamados/juizes.py:1177`; `app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/models.py:1423`; `app/chamados/services/pergunta_no_app.py:94`) |
| Pergunta: veredito lavrado e expiração | `lavrar`, lote de lavras e costura junto ao destino/data. (`app/chamados/veredito_lavrado.py:197`; `app/chamados/veredito_lavrado.py:211`; `app/chamados/services/fio.py:846`) | Raio-X, seleção stale e lavra vencida. (`app/chamados/services/raiox.py:173`; `app/chamados/veredito_lavrado.py:173`; `app/chamados/management/commands/lavrar_perguntas_stale.py:58`) |
| Célula: geradora, DNA, trabalho, ata, veredito e carimbos | Portas de geração, regeneração, reversão, lavra e carimbo. (`app/ponto/portas/celula.py:100`; `app/ponto/portas/celula.py:481`; `app/ponto/portas/celula.py:518`; `app/ponto/portas/celula.py:593`; `app/ponto/portas/celula.py:747`; `app/ponto/portas/celula.py:825`; `app/ponto/portas/celula.py:849`) | Vínculo, teto, fábrica, C6, premissa e vigias. (`app/escala/alimentacao.py:269`; `app/ponto/services/cartorio.py:841`; `app/chamados/services/fabrica_da_celula.py:89`; `app/chamados/juizes.py:1181`; `app/chamados/models.py:1230`; `app/chamados/services/vigia_fio.py:318`) |
| Evento: célula, colaborador, data, processamento e tentativas | Emissão pela porta; processamento/tentativas pelo consumidor. (`app/ponto/portas/celula.py:664`; `app/chamados/reconciliador.py:1337`) | Consumidor, eventos pendentes/mortos, idade, semeadura e vigias de cobertura. (`app/chamados/reconciliador.py:1214`; `app/chamados/reconciliador.py:1220`; `app/chamados/reconciliador.py:1226`; `app/chamados/reconciliador.py:1233`; `app/chamados/reconciliador.py:1246`; `app/chamados/reconciliador.py:1309`) |

Esse inventário comprova os sítios nomeados; **exaustividade de todos os leitores indiretos e aliases [não verificado]**. Portanto não declaro cumprido o requisito de censo fechado de escritores antes da mudança. (`CLAUDE.md:193`)

## C — DIFF PROPOSTO EM TEXTO

**Não entrego como correto um diff completo desta obra.** O candidato mínimo abaixo conecta o nascimento em `abrir` ao evento e o consumidor à fábrica existente, mas é **insuficiente**, pelos contraexem

[truncado em 20000 de 28343 caracteres]

## 05/11 18:12 · sessao 720daba17099 · PC1-PEP · censo · origem=Code

HASH LIDO = 9c0fd9d3e90f166cba697ec840422b5585a8ab09

## A CASOS PELA REGRA

O hash identifica a cópia informada; sua correspondência com um objeto Git não pôde ser conferida nesta árvore sem `.git`. [nao verificado]

A escolha está respondida: **candidata 1, dentro de `carimbar`, transação, releitura travada e novo julgamento por C6**, com conferência do memo e medição da ordem de trava antes do verde. A indefinição registrada no `pd_pep.r` foi superada pelo aval posterior. `app/docs/PROMPTS.md:1882`.

Os cadastros abaixo são cenários de ensaio; sua ocorrência em produção não está demonstrada pelo dado fornecido. Os resultados esperados vêm da regra, conforme L-110. `app/docs/LEIS.md:136`. Ocorrência em produção: [nao verificado].

| Caso | Cadastro + estado | Ato | Resultado exigido pela regra |
|---|---|---|---|
| A1 — furo continua | Dia de trabalho, fato encerrado, marco faltante lavrado; sem resposta nem decisão humana. | Carimbar `sem_turno_aberto`, `completude_turno` ou equivalente automático. | **Recusar enquanto a pergunta vive**; carimbo não substitui a resolução do fato. `CLAUDE.md:167`; `CLAUDE.md:170`; `app/docs/PROMPTS.md:1882`. |
| A2 — um marco resolvido | Batida acendeu o marco desta pergunta; outro marco mantém o dia acusador. | Encerrar a pergunta do marco aceso. | Reconhecer o fato deste marco, conservando a cobrança do outro. `CLAUDE.md:167`; `app/chamados/models.py:2257`; `app/docs/CORTES.md:413`. |
| A3 — resposta discordante | Pergunta respondida, sem validação; resposta discorda da batida ou não oferece hora comparável. | Carimbo automático. | Conservar a pergunta para decisão administrativa; não transformar silêncio do sistema em concordância. `app/docs/PROMPTS.md:1442`; `app/chamados/juizes.py:1090`; `app/chamados/juizes.py:1096`. |
| A4 — resposta concordante | Batida acendeu o marco, resposta concorda pela régua existente e o dia não acusa. | Rejulgar e encerrar. | Pode encerrar automaticamente pela regra já autorizada, sem tolerância nova. `app/docs/PROMPTS.md:1442`; `app/chamados/juizes.py:1090`. |
| A5 — decisão humana entre coleta e escrita | Consumidor conserva pergunta antiga; admin decide o mesmo dia e grava `admin_decidiu`. | Consumidor tenta seu carimbo automático antigo. | Reler sob trava e preservar decisão, autor e trilha; a tentativa antiga não pode substituir a via humana. `app/docs/PROMPTS.md:1882`; `app/docs/LEIS.md:65`; `app/chamados/services/relavra_via.py:214`. |
| A6 — resposta nova antes da trava | Consumidor conserva resposta A validada; colaborador grava resposta B e desfaz a validação anterior. | Consumidor tenta carimbar/validar usando a intenção formada sobre A. | A assinatura de A não valida B; conservar resposta nova e sua pendência. `app/docs/PROMPTS.md:1882`; `app/docs/CORTES.md:333`; `app/chamados/services/disputa_emissao.py:1287`. |
| A7 — resposta nova depois do carimbo | Escritor da resposta já carregou pergunta sem validação; outro ato valida/carimba antes de ele salvar. | Escritor antigo salva resposta nova. | A validação anterior não pode sobreviver como assinatura da resposta nova; a propriedade vale também nessa ordem de intercalação. `app/docs/CORTES.md:333`; `app/docs/LEIS.md:65`; `app/chamados/services/disputa_emissao.py:1287`. |
| A8 — concorrência e repetição | Duas tentativas sobre a mesma pergunta, sem novo fato entre elas. | Executar ambas; repetir a mesma entrada. | Decisão sobre linha travada; repetição equivalente preserva estado, uma trilha e zero duplicata. `app/docs/PROMPTS.md:1882`; `CLAUDE.md:175`. |
| A9 — competência 21→20 | Empresa cadastrada com corte 21; fatos em 20/10 e 21/10; competência 10 trancada e 11 aberta. | Julgar cada pergunta. | 20/10 pertence à competência 10; 21/10 à 11. Aplicar corte cadastrado e competência do fato. Tranca não fabrica batida ou quitação. `CLAUDE.md:81`; `app/ponto/janelas.py:10`; `app/chamados/juizes.py:1142`. |
| A10 — cross-meia-noite | 12x36 19–07, pausa 23–00; às 00:10 a saída das 07:00 ainda não venceu. | Encerrar porque a data civil mudou; consumir volta às 00:00. | A virada civil não encerra o fato; a volta é D+1 dentro da jornada iniciada em D. `CLAUDE.md:185`; `app/docs/JUIZES.md:24`; `app/docs/LEIS.md:117`. |
| A11 — provisório e chegada da batida | Célula provisória não acusa; marco perguntado continua pendente. Depois chega sua batida. | Tentar encerrar antes e rejulgar depois. | Antes, não absolver pelo provisório; depois, reconhecer a batida na passada seguinte. `CLAUDE.md:185`; `app/docs/CORTES.md:413`. |
| A12 — vínculo trocado | Célula histórica gerada pelo vínculo A; hoje existe B com outros marcos. | Julgar/carimbar o passado. | Respeitar geradora e DNA históricos; troca atual não autoriza reescrita silenciosa. Regeneração formal é outro ato. `app/docs/JUIZES.md:98`; `CLAUDE.md:148`; `app/escala/alimentacao.py:231`. |
| A13 — célula sem ata | `ata={}`, `trabalha=None`, sem outro fato resolutivo. Variante: `trabalha=False`. | Encerrar pela ausência de lâmpadas apagadas. | Com `None`, vazio não prova dispensa: **cobra**. Com `False`, cala pela declaração de não trabalho. `app/docs/CORTES.md:416`. |
| A14 — célula contraditória | `trabalha=True`, `veredito='concorde'`, ata vazia e nenhuma prova do marco. | Carimbar pelo veredito. | A ata vazia não prova resolução. Resultado exato dessa combinação contraditória: **[nao verificado]**; teste atual não pode virar regra. `app/docs/LEIS.md:95`; `app/docs/LEIS.md:136`; `app/docs/CORTES.md:416`. |
| A15 — ausência parcial | Ausência cobre parte do dia; marco perguntado está fora do período coberto. | Absorver/carimbar pela ausência. | Cobrar o resto descoberto; parcial não dispensa o dia inteiro. `app/docs/JUIZES.md:58`; `app/docs/JUIZES.md:60`. |
| A16 — continente fechado | Pergunta respondida; disputa encerrada por desligamento/admin, sem decisão do dia. | Transformar fechamento da disputa em `admin_decidiu`. | Não inventar decisão humana; a resposta continua para Validar. `app/docs/CORTES.md:342`. |
| A17 — erro e restauração | Candidato aplicado em memória; C6, lavra ou salvamento falha. | Abortar o ato. | Não deixar a instância declarando escrita que não persistiu; conservar coerência do estado e trilha. `app/docs/LEIS.md:65`; `CLAUDE.md:175`. |
| A18 — antiguidade | Pergunta antiga, marco ainda devido, sem decisão ou prova resolutiva. | Encerrar somente pela idade. | Idade não apaga a lâmpada nem substitui o fato. `app/docs/CORTES.md:253`; `app/docs/PROMPTS.md:1317`. |

## B SITIOS

**Fonte da alteração:** `PerguntaDisputa.carimbar`. O corpo atual aplica candidato, consulta C6 e salva a instância recebida; restaura carimbo e lavra nas exceções tratadas. A releitura travada deve entrar antes dessas mutações, preservando esse escritor. `app/chamados/models.py:2551`; `app/chamados/models.py:2572`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`; `app/chamados/models.py:2605`.

**Autoridades existentes:** C6 responde vitalidade; C7 fechamento da disputa; C15 vitalidade da coleção; C17 alcance da porta; C18 dia sem trabalho. Competência usa `janela_atual`; vínculo histórico usa `vinculo_do_dia`; meia-noite usa `marcos_turno`. O registro vivo prevalece sobre o lookup anexado. `app/core/juizes.py:266`; `app/core/juizes.py:267`; `app/core/juizes.py:275`; `app/core/juizes.py:277`; `app/core/juizes.py:278`; `app/core/juizes.py:283`; `app/docs/JUIZES.md:24`; `app/docs/JUIZES.md:98`; `app/docs/JUIZES.md:14`.

**Campos, escritores e leitores centrais:**

| Campo/insumo | Escritores encontrados | Leitores centrais |
|---|---|---|
| Tripla `materializada_em`, `materializada_por`, `via_resolucao` | `_escrever_carimbo`, chamado por carimbo comum, identidade, restauração e descarimbo; limpeza persistida por `descarimbar_em_lote`. `app/chamados/models.py:2545`; `app/chamados/models.py:2575`; `app/chamados/models.py:2608`; `app/chamados/models.py:2626`; `app/chamados/models.py:2637`; `app/chamados/models.py:2652`. | C6, lastro, estado, SQL de vivas e procedência. `app/chamados/juizes.py:647`; `app/chamados/juizes.py:1135`; `app/chamados/models.py:2488`; `app/chamados/models.py:2735`; `app/ponto/services/procedencia_batida.py:72`. |
| Via isolada | `encerrar_por_fato_no_espelho` e `relavra_via.relavrar`. Portanto, a via isolada ainda tem escritores além da porta da tripla. `app/chamados/models.py:2662`; `app/chamados/services/relavra_via.py:123`. | Relavra, elegibilidade do descarimbo, cartório e apresentação. `app/chamados/services/relavra_via.py:116`; `app/chamados/services/relavra_via.py:179`; `app/ponto/services/cartorio.py:1599`; `app/templates/chamados/partials/_card_atendimento.html:186`. |
| `respondida_em` | Carimbo/restauração; resposta humana; resposta automática; recuperação pela trilha; correção e ressuscitação. `app/chamados/models.py:2578`; `app/chamados/models.py:2609`; `app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1429`; `app/chamados/services/disputa_emissao.py:1445`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`. | C6, estado, validação, filas e API. `app/chamados/juizes.py:1150`; `app/chamados/models.py:2492`; `app/chamados/services/validacao.py:363`; `app/api/views.py:2636`. |
| `validada_em`, `validada_por` | Carimbo/restauração; validação/devolução; resposta nova; correção; ressuscitação. O lookup chama validação de escritor único, mas o carimbo também atribui esses campos. `app/chamados/models.py:2581`; `app/chamados/models.py:2610`; `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1493`; `app/docs/JUIZES.md:38`. | C6 antes dos demais degraus; estado; seleção do materializador; procedência. `app/chamados/juizes.py:1136`; `app/chamados/models.py:2488`; `app/chamados/services/materializacao.py:1293`; `app/ponto/services/procedencia_batida.py:72`. |
| `resposta_colab` | Resposta humana, automática e limpeza para correção. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1428`; `app/chamados/services/disputa_emissao.py:1393`. | Porta, concordância com luz, slot/lastro, materializador, API, calendário, ficha e prévia. `app/chamados/models.py:2576`; `app/chamados/juizes.py:1113`; `app/chamados/juizes.py:277`; `app/chamados/services/materializacao.py:1183`; `app/api/views.py:2580`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`. |
| Identidade: disputa, célula, destino, motivo, data e tipo | Nascimento por `abrir`; reaproveitamento muda célula/destino; reapontamento também pode preencher data. `app/chamados/models.py:2387`; `app/chamados/models.py:2425`; `app/chamados/models.py:2439`; `app/chamados/models.py:2465`; `app/chamados/services/fio.py:834`; `app/chamados/services/fio.py:845`. | C6, seleção do marco, lastro e validação da resolvedora. `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:1135`; `app/chamados/juizes.py:632`; `app/chamados/services/fio.py:838`. |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `lavrar`, restauração e persistência em lote; backfill prepara objetos para essa persistência. `app/chamados/veredito_lavrado.py:227`; `app/chamados/veredito_lavrado.py:243`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/management/commands/backfill_veredito_lavrado.py:138`. | `esta_stale`, raio-X e passivo/reversão. `app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:173`; `app/chamados/services/passivo_validado.py:64`. |
| `_pv_memo` | C6 grava; restauração e descarimbo removem. `app/chamados/juizes.py:847`; `app/chamados/models.py:2612`; `app/chamados/models.py:2638`. | `pergunta_viva` reutiliza por chave. A chave contém veredito/via da célula, mas omite ata/DNA, lidos na concordância com a luz. A releitura precisa eliminar o memo recebido. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:837`; `app/chamados/juizes.py:1105`; `app/chamados/juizes.py:1111`. |
| Célula, batidas e tranca consultadas por C6 | Célula tem portas próprias de lavra/regeneração; batida tem escritor canônico próprio. Não pertencem ao escritor do carimbo da pergunta. `app/ponto/portas/celula.py:747`; `app/ponto/portas/celula.py:100`; `CLAUDE.md:141`. | C6 consulta tranca, célula, fato do marco e lastro externo. `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1147`; `app/chamados/juizes.py:1178`; `app/chamados/juizes.py:1214`. |

**Todos os pontos diretos encontrados de chamada do carimbo comum:**

| Chamador | Sítio |
|---|---|
| `encerrar_fantasma` | `app/chamados/models.py:2674`. |
| `lavrar_sem_turno_aberto` | `app/chamados/reconciliador.py:405`. |
| `retratar_perguntas_absolvidas_por_celula` | `app/chamados/reconciliador.py:620`. |
| Resolução por chamado, ramo principal e ramo sem credor | `app/chamados/services/disputa_emissao.py:1603`; `app/chamados/services/disputa_emissao.py:1675`. |
| Absorção por ausência | `app/chamados/services/materializacao.py:546`. |
| `_materializar_uma` | `app/chamados/services/materializacao.py:1199`. |
| `materializar_pendentes_aprovadas` | `app/chamados/services/materializacao.py:1346`. |

O plantio individual já envolve batida e carimbo em transação e transforma `False` em reprovação para reverter a batida; essa composição deve permanecer. O inverso já relê pergunta/disputa travadas antes de C6. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`; `app/chamados/services/materializacao.py:1201`; `app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:227`.

**Demais consumidores encontrados dos campos, estados e funções afetados:**

| Grupo | Sítios |
|---|---|
| Estado e redução | Propriedades/queryset da pergunta, coleção viva e raio-X. `app/chamados/models.py:2481`; `app/chamados/models.py:2713`; `app/chamados/models.py:2807`; `app/chamados/juizes.py:1225`; `app/chamados/juizes.py:1257`; `app/chamados/services/raiox.py:133`. |
| Reconciliador e seleção de trabalho | `app/chamados/reconciliador.py:366`; `app/chamados/reconciliador.py:583`; `app/chamados/reconciliador.py:1017`; `app/chamados/reconciliador.py:1433`; `app/chamados/services/materializacao.py:433`; `app/chamados/services/materializacao.py:529`; `app/chamados/services/materializacao.py:604`; `app/chamados/services/materializacao.py:1292`; `app/chamados/services/materializacao.py:1341`; `app/chamados/services/validacao.py:422`. |
| Emissão e fábrica | `app/chamados/services/disputa_emissao.py:695`; `app/chamados/services/disputa_emissao.py:909`; `app/chamados/services/disputa_emissao.py:1152`; `app/chamados/services/fabrica_da_celula.py:119`; `app/chamados/services/fabrica_da_celula.py:138`. |
| Fio, cobrança e ações | `app/chamados/services/fio.py:336`; `app/chamados/services/fio.py:656`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/services/acoes_disputa.py:95`; `app/chamados/services/acoes_disputa.py:130`; `app/chamados/services/acoes_disputa.py:341`; `app/chamados/services/declaracao_texto.py:211`; `app/chamados/services/declaracao_texto.py:366`; `app/chamados/services/declaracao_texto.py:635`. |
| App e integrações | `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/views.py:1257`; `app/chamados/views.py:1280`; `app/chamados/views.py:1416`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:176`; `app/api/views.py:1009`; `app/api/views.py:1173`; `app/api/views.py:1187`; `app/api/views.py:2580`; `app/api/views.py:2636`; `app/api/views_mensageria.py:567`; `app/api/foto_recebida.py:48`. |
| Apresentação e relatórios | `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/explicador.py:32`; `app/chamados/services/explicador.py:79`; `app/chamados/services/explicador.py:95`; `app/chamados/canal.py:65`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:69`; `app/ponto/services/cartorio.py:1590`; `app/ponto/services/cartorio.py:1599`. |
| Histórico, painel e procedência | `app/chamados/services/historico.py:99`; `app/chamados/services/painel.py:67`; `app/chamados/services/painel.py:260`; `app/chamados/services/verbos.py:240`; `app/chamados/services/verbos.py:311`; `app/ponto/services/procedencia_batida.py:72`. |
| Vigias e passivo | `app/ponto/supra_juiz.py:545`; `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:783`; `app/chamados/services/vigia_carimbo.py:98`; `app/chamados/services/furo_sem_canal.py:81`; `app/ponto/services/flip_auto.py:129`; `app/chamados/services/passivo_validado.py:64`; `app/chamados/services/passivo_validado.py:140`; `app/chamados/services/relavra_via.py:179`. |
| Comandos | `app/ponto/management/commands/reconciliar_grade.py:461`; `app/ponto/management/commands/reconciliar_fantasmas.py:212`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`; `app/chamados/management/commands/encerrar_perguntas_com_fato.py:32`; `app/chamados/management/commands/validar_classe_a.py:80`; `app/chamados/management/commands/auditar_disputas.py:71`; `app/chamados/management/commands/auditar_disputas.py:150`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:67`; `app/chamados/management/commands/relavrar_via_i6.py:58`; `app/chamados/management/commands/dedup_perguntas_fato.py:74`. |
| Auditoria de invariantes | Carimbo sem testemunha, materializada viva, via ausente e validação sem plantio. `app/chamados/management/commands/auditar_invariantes_chamados.py:538`; `app/chamados/management/commands/auditar_invariantes_chamados.py:905`; `app/chamados/management/commands/auditar_invariantes_chamados.py:955`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1073`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1103`. |
| Templates | `app/templates/chamados/partials/modal_fio.html:184`; `app/templates/chamados/partials/modal_fio.html:212`; `app/templates/chamados/partials/modal_fio.html:223`; `app/templates/chamados/partials/_card_atendimento.html:94`; `app/templates/chamados/partials/_card_atendimento.html:100`; `app/templates/chamados/partials/_card_atendimento.html:137`; `app/templates/chamados/partials/_card_atendimento.html:155`; `app/templates/chamados/partials/_card_atendimento.html:186`; `app/templates/chamados/partials/_card_atendimento.html:384`; `app/templates/chamados/partials/_pergunta_card.html:20`; `app/templates/chamados/partials/_pergunta_card.html:42`. |
| Instrumentos fora de `app` | Censo de folha lê resposta/validação; raspagem reconhece o texto da resposta como dado pessoal. `bin/censo_folha.py:28`; `bin/censo_folha.py:29`; `bin/sombra_regras_pessoais.py:85`. |
| Eventos e selos | Fechos de chamado/disputa disparam consumidores; salvamento da pergunta agenda beco em `on_commit`. Selos restringem escritores/chamadores internos e verificam restauração após erro. `app/chamados/signals.py:76`; `app/chamados/signals.py:125`; `app/chamados/signals.py:192`; `app/chamados/signals.py:202`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:374`; `app/chamados/tests/test_o261e_f2_fila.py:179`; `app/chamados/tests/test_o261e_f2_fila.py:190`; `app/chamados/tests/test_o261e_f2_fila.py:200`. |

Completude para chamadas dinâmicas, SQL externo e caminhos ausentes da cópia: [nao verificado].

## C DIFF PROPOSTO EM TEXTO

Este é o incremento mínimo proposto na fonte para a candidata autorizada: trava/releitura, descarte do memo e recusa da intenção obsoleta, mantendo o corpo escritor e C6 existentes. **Não basta para declarar toda a concorrência resolvida**, pelos contraexemplos D1–D4. `app/docs/PROMPTS.md:1882`; `app/chamados/models.py:2575`; `app/chamados/models.py:2585`; `app/chamados/models.py:2598`.

```diff
--- a/app/chamados/models.py
+++ b/app/chamados/mo

[truncado em 20000 de 27571 caracteres]

## 05/11 18:11 · sessao 18d5620d9ed0 · - · - · origem=Ronald (tmux)

Complemento dos seis campos, por leitura do código; dados/execução em produção **[nao verificado]**.

| Campo de `PerguntaDisputa` | Escritores encontrados |
|---|---|
| `disputa_id` | Nascimento em `PerguntaDisputa.abrir`, que faz `objects.create(disputa=...)`. A identidade existente é devolvida sem trocar sua disputa. `app/chamados/models.py:2465`, `app/chamados/models.py:2464`. |
| `motivo` | Nascimento na mesma porta; participa da busca de identidade `(colaborador, motivo, data_evento)`. `app/chamados/models.py:2465`, `app/chamados/models.py:2381`. |
| `tipo_resposta` | Nascimento pelo `**campos` da mesma porta, fornecido pelos emissores. `app/chamados/models.py:2466`; `app/chamados/services/disputa_emissao.py:393`, `app/chamados/services/disputa_emissao.py:774`, `app/chamados/services/disputa_emissao.py:1199`; `app/chamados/services/acoes_disputa.py:262`; `app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:614`. |
| `celula_id` | Nascimento; atualização de identidade existente por `abrir` quando encontra a célula da pessoa/data; exclusão da célula produz `SET_NULL`. `app/chamados/models.py:2466`, `app/chamados/models.py:2419`, `app/chamados/models.py:2425`, `app/chamados/models.py:2463`, `app/chamados/models.py:2198`. |
| `data_evento` | Nascimento; conversa recebe `localdate`; `fio.reapontar_resolvedora` preenche somente quando a pergunta ainda não tem dia, antes do julgamento/lavra. `app/chamados/models.py:2416`, `app/chamados/models.py:2465`; `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:847`. |
| `chamado_destino_id` | Nascimento; reapontamento no dedup de `abrir`; `fio.reapontar_resolvedora`; exclusão do chamado produz `SET_NULL`. `app/chamados/models.py:2466`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`; `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:847`; `app/chamados/models.py:2772`. |

Não encontrei atualização persistida posterior de `disputa`, `motivo` ou `tipo_resposta` nos fontes de produção pesquisados; completude contra operações dinâmicas externas ou banco **[nao verificado]**. A porta de nascimento encontrada conserva os três quando deduplica e só inclui célula/destino no `update_fields`. `app/chamados/models.py:2422`, `app/chamados/models.py:2425`, `app/chamados/models.py:2439`, `app/chamados/models.py:2463`.

A exclusão da disputa remove a pergunta por `CASCADE`, podendo fazer a releitura por PK não encontrar linha; isso precisa ter resultado explícito no diff. `app/chamados/models.py:2200`.

**Nascimento: todos os chamadores encontrados da porta**

- Emissão geral: `app/chamados/services/disputa_emissao.py:389`.
- Pergunta de hora: `app/chamados/services/disputa_emissao.py:770`.
- Âncora sem entrada: `app/chamados/services/disputa_emissao.py:1195`.
- Reabertura administrativa: `app/chamados/services/acoes_disputa.py:259`.
- Declaração pelo texto, dois pontos: `app/chamados/services/declaracao_texto.py:325`, `app/chamados/services/declaracao_texto.py:614`.

**Transferência e lote**

A migração de resolvedoras leva perguntas entre chamados, pelo mesmo `reapontar_resolvedora`; não troca a FK `disputa`. Dedup compara motivo/data e retrata a excedente. `app/chamados/services/fio.py:937`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`. Os três chamadores de reparo do reconciliador delegam a essa porta, podendo preencher a data quando ausente. `app/chamados/reconciliador.py:922`, `app/chamados/reconciliador.py:1570`, `app/chamados/reconciliador.py:1582`.

O `bulk_update` encontrado de `PerguntaDisputa` escreve os três campos da lavra, não os seis identificadores propostos. `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`.

As atribuições de célula/destino nestes outros pontos são fotografias de julgamento em memória, não escritas dessas FKs no banco: célula do julgamento em cópia; chamado atual na cópia; destino candidato temporário da guarda e restauração. `app/chamados/models.py:1432`, `app/chamados/models.py:1439`, `app/chamados/models.py:350`; `app/chamados/services/fio.py:773`, `app/chamados/services/fio.py:777`.

**Conferência humana da data: campo fora do snapshot**

`conferir_data_do_fato` lê `data_evento` e o dia do chamado, mas grava somente `data_conferida_em`/`data_conferida_por`. O `data_evento` no log é evidência da data aceita, não atribuição ao modelo. `app/chamados/services/validacao.py:611`, `app/chamados/services/validacao.py:617`, `app/chamados/services/validacao.py:619`, `app/chamados/services/validacao.py:624`.

Assim, comparar somente aqueles seis campos não detecta uma conferência humana intercalada; a relevância dessa perda para o ato de carimbar depende do escopo do diff, **[nao verificado]**. A materialização usa essa conferência como liberação da divergência. `app/chamados/services/materializacao.py:1110`, `app/chamados/services/materializacao.py:1135`.

**Leitores/autoridades relevantes dos seis**

| Leitura | Autoridade e sítio |
|---|---|
| Identidade do fato | `PerguntaDisputa.do_fato`, usando colaborador da disputa, motivo e data. `app/chamados/models.py:2370`, `app/chamados/models.py:2381`. |
| C6 e memo | C6 lê célula, data, tipo, destino; a chave não contém `disputa_id` nem `motivo`. `app/chamados/juizes.py:811`, `app/chamados/juizes.py:815`, `app/chamados/juizes.py:817`, `app/chamados/juizes.py:1135`. C6 registrado: `app/core/juizes.py:266`. |
| Prova da célula pertence à pessoa/data | `_celulas_da_pergunta` recusa alimentar célula de outra pessoa/data ou incompleta. `app/chamados/juizes.py:621`, `app/chamados/juizes.py:625`, `app/chamados/juizes.py:627`. |
| Geometria do marco | `motivo` identifica marco; tipo/data/resposta participam do slot. `app/chamados/juizes.py:260`, `app/chamados/juizes.py:288`, `app/chamados/juizes.py:333`, `app/chamados/juizes.py:1177`. |
| Admissibilidade da costura | `resolvedora_valida`, chamada antes do nascimento/reapontamento. `app/chamados/juizes.py:1948`; `app/chamados/models.py:2402`, `app/chamados/models.py:2429`; `app/chamados/services/fio.py:838`. Não achei entrada nominal dela em `core/juizes.py`; registro nominal próprio **[nao verificado]**. |
| Data do fato da pergunta | `fio.data_do_fato_de` devolve data_evento, divergência e dia do chamado; não escolhe silenciosamente. `app/chamados/services/fio.py:40`, `app/chamados/services/fio.py:45`, `app/chamados/services/fio.py:51`, `app/chamados/services/fio.py:87`. Registro nominal próprio em core **[nao verificado]**. |
| Dia do chamado | Registro C5 em `catalogo/modulos.py::data_do_chamado`. `app/core/juizes.py:265`. `data_do_fato_de` consulta `dia_do_fato` para comparação. `app/chamados/services/fio.py:85`. |
| Tipo da resposta e escolha do plantio | Resposta humana valida segundo tipo; materialização escolhe hora/data_hora/escolha. `app/chamados/services/disputa_emissao.py:1257`, `app/chamados/services/disputa_emissao.py:1263`, `app/chamados/services/disputa_emissao.py:1272`; `app/chamados/services/materializacao.py:1143`. |
| Validação humana/implausibilidade | Lê tipo, motivo, destino, data e pessoa da disputa. `app/chamados/juizes.py:1565`, `app/chamados/juizes.py:1573`, `app/chamados/juizes.py:1698`, `app/chamados/juizes.py:1706`; `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:374`. |
| Marco histórico/virada da meia-noite | Cadeia `marcos_turno`; vínculo do dia. `app/chamados/juizes.py:288`, `app/chamados/juizes.py:302`; `app/docs/JUIZES.md:24`; `app/core/juizes.py:1285`; `app/docs/JUIZES.md:98`. |
| Reapontamento e frescor da lavra | Data/destino são insumos sem timestamp próprio; a porta relavra junto. `app/chamados/services/fio.py:798`, `app/chamados/services/fio.py:816`, `app/chamados/services/fio.py:846`. |
| Listagem/crédito | `data_do_fato_de` usa `chamado_que_lista`; vigia e porta alcançável consomem a âncora. `app/chamados/services/fio.py:77`; `app/chamados/juizes.py:1372`, `app/chamados/juizes.py:1379`; registro C17 `app/core/juizes.py:277`. |

As portas já são declaradas por dono em `core/portas.py`: nascimento/identidade no modelo; reapontamento em `fio.py`; conferência no dono da validação. Comparar os valores na porta existente não requer criar nova autoridade de vitalidade. `app/core/portas.py:241`, `app/core/portas.py:304`, `app/core/portas.py:362`; C6 já registrado em `app/core/juizes.py:266`.

## 05/11 18:10 · sessao 54bdf96d6ab9 · - · - · origem=Ronald (tmux)

Achados confirmados, sem escrita:

- `.sh` passa apenas por citação de linhas; não há AST nem atribuição de chamador ao símbolo Python. Heredoc com `from m.lib import mudada` produz somente faceta arquivo; `python3 -c 'from m import lib; lib.mudada()'` não produz achado. `bin/quem_chama.py:390`, `bin/quem_chama.py:510`, `bin/quem_chama.py:673`.
- A cura por qualquer componente literal de `modulo_de(rel)` em código shell é conservadora: reconhece import absoluto, `from core import placar_estrutural`, relativo `from . import lib` e módulo simples de `bin/`; nomes coincidentes também podem produzir “não varrido”. O predicado foi exercitado em memória com `_codigo_sh` e `_padrao_nome`. `bin/quem_chama.py:124`, `bin/quem_chama.py:347`, `bin/quem_chama.py:479`.
- Rebaixar `tipo` antes de `_tipo_simbolo` rebaixa arquivo e símbolos; guardar `lib` antes disso preserva achados dos templates. `_faceta` continua como único escritor de contagem/“não varrido” e alimenta o resumo existente. `bin/quem_chama.py:311`, `bin/quem_chama.py:655`, `bin/quem_chama.py:657`, `bin/quem_chama.py:690`, `bin/quem_chama.py:713`.
- Falha adicional confirmada: `{% load m_tags %}` seguido de `{% filter rot %}...{% endfilter %}` encontra a biblioteca, mas perde o uso do filtro porque a regex só aceita `|rot`. Correção mínima: aceitar `(?:\||{%\s*filter\s+)\s*` antes do nome escapado. `bin/quem_chama.py:526`, `bin/quem_chama.py:560`.
- Comentário `{# ... #}` multilinha é removido corretamente pelo motor; a fixture atual cobre comentários de uma linha, deixando a borda multilinha sem RED específico. `bin/quem_chama.py:520`, `bin/tests/test_quem_chama.sh:61`, `bin/tests/test_quem_chama.sh:79`.
- As asserções de símbolo parado prendem apenas o texto de saída, permitindo o mesmo texto na faceta errada; precisam selecionar alvo **e faceta** antes de afirmar presença/ausência. O helper posterior `_sec` também delimita somente o alvo. `bin/tests/test_quem_chama.sh:159`, `bin/tests/test_quem_chama.sh:167`, `bin/tests/test_quem_chama.sh:210`.
- A fixture já não cria `__init__.py`, mas `lib.py` contém leitura interna de `mudada`, que pode mascarar um teste negativo de zero; o RED shell precisa de alvo isolado sem leitura interna/import local. `bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:32`, `bin/tests/test_quem_chama.sh:121`.
- O selo inteiro cria diretório, arquivos e commits temporários; não deve ser executado nesta chamada de leitura estrita. `bin/tests/test_quem_chama.sh:24`, `bin/tests/test_quem_chama.sh:28`, `bin/tests/test_quem_chama.sh:118`.

## 05/11 18:10 · sessao 24d2e776eb62 · - · - · origem=Ronald (tmux)

Censo B e normas, somente por leitura:

- **Contrato do instrumento:** lista determinística de chamadores/leitores a partir do diff; “lista é fato, não juiz”, zero autoridade nova (`bin/quem_chama.py:4-6`). Facetas fora da lista positiva devem declarar “não varrido”, nunca usar ausência de achado como prova de zero (`bin/quem_chama.py:8-15`).
- **Juiz existente:** não encontrei registro específico de quem-chama em `app/core/juizes.py` ou `app/docs/JUIZES.md` [nao verificado quanto à exaustividade]. O registro vivo declara quem decide perguntas do domínio (`app/core/juizes.py:3-8`); JUIZES.md é lookup e remete ao registro vivo (`app/docs/JUIZES.md:7-12`). O próprio contrato exclui juiz novo (`bin/quem_chama.py:6`).
- **Leitor externo direto da fonte:** selo escolhe `QUEM_CHAMA_PY` ou `bin/quem_chama.py` (`bin/tests/test_quem_chama.sh:25`), invoca o programa com range, raiz e saída (`bin/tests/test_quem_chama.sh:148`). O selo é descoberto/executado pelo glob da régua (`bin/regua.sh:138-140`).
- **Sítio fonte de cobertura:** `VARRIDOS` é a declaração positiva (`bin/quem_chama.py:37-55`); `_tipo_simbolo` classifica facetas (`bin/quem_chama.py:311-326`); `quem_chama` grava `info['tipo']` e monta alvo (`bin/quem_chama.py:655-660`); despacho de shell usa somente citações de linhas, sem AST do payload (`bin/quem_chama.py:673-674`).
- **Único escritor de apresentação/contadores:** `_faceta` decide N versus “não varrido”, escreve `varridas`, `zeros` e `nao`, depois imprime achados (`bin/quem_chama.py:690-699`). `render` lê os tipos de arquivo e símbolo e chama essa mesma função (`bin/quem_chama.py:713-715`), produz resumo (`bin/quem_chama.py:717-721`). `main` lê o total de `conta['nao']` no resumo de gravação (`bin/quem_chama.py:747-748`). A cura deve alimentar esse escritor, sem apresentação paralela.
- **Leitores/dados próximos:** `Alvos` indexa biblioteca/modulo e citações (`bin/quem_chama.py:363-382`); `varrer_py` interpreta importações e símbolos (`bin/quem_chama.py:390-439`); `varrer_template` lê registros para filtro/tag (`bin/quem_chama.py:553-565`). Não alterar metadados `membro`, `registro`, `nome` para mascarar cobertura: são insumos dessas varreduras (`bin/quem_chama.py:411-414`, `bin/quem_chama.py:426-438`, `bin/quem_chama.py:555-560`).
- **Fragilidade de faceta já publicada:** asserções do selo buscam linha global e aceitam “importa de m.lib” sem vincular ao bloco da faceta (`bin/tests/test_quem_chama.sh:159-160`); caso relativo repete abordagem (`bin/tests/test_quem_chama.sh:166-168`). A reprovação fornecida exige prender a faceta [nao verificado em arquivo local do anexo].
- **Normas:** regra antes do código/L-110 (`CLAUDE.md:45-51`), zero de produção implica apagar pela L-111 (`CLAUDE.md:188`), censo de escritores antes da cura (`CLAUDE.md:193`), selo sobre saída exige MORDE (`CLAUDE.md:191`), escopo inclui escritores/leitores mesmo fora do diff (`CLAUDE.md:371-373`), instrumento usa `padrao=n/a (instrumento)` (`CLAUDE.md:65-70`).
- **Limites de autorização:** B3 suspende obra após reprovação final e exige pergunta de desenho (`CLAUDE.md:271-272`); contrato de instrumento tem cinco linhas (`CLAUDE.md:386`). Teto específico de duas voltas e proibição de varrer Python em `.sh` vêm do anexo MANHA-0511 fornecido pelo usuário [nao verificado em arquivo local]. Não confundir com teto local de dois **construtores**, outra unidade (`CLAUDE.md:288`).
- **Limites de execução:** programa lê ponta/base por Git (`bin/quem_chama.py:17-19`, `bin/quem_chama.py:619-625`); esta cópia não tem `.git`, conforme erro observado, sem arquivo:linha [nao verificado]. Executar o selo violaria “nenhum arquivo”: cria temporário e remove no trap (`bin/tests/test_quem_chama.sh:24`), cria fixture e Git (`bin/tests/test_quem_chama.sh:28-29`). `main --saida/--obra` também cria diretórios e grava (`bin/quem_chama.py:738-743`).

## 05/11 18:09 · sessao d4feb77f32e6 · - · - · origem=Ronald (tmux)

A cobertura deve usar C6 (`pergunta_viva`) para a morte da pergunta, C15 para o conjunto e C17 para distinguir fio alcançável/ação administrativa; C11 registrado continua sendo ausência. (`app/core/juizes.py:266`; `app/core/juizes.py:271`; `app/core/juizes.py:275`; `app/core/juizes.py:277`)

| Ato | Fonte e risco do diff |
|---|---|
| Reabrir | Depois de `_gravar_estado` e `ressuscitar_perguntas`; esta última pode devolver zero e não cria pergunta inexistente. (`app/chamados/models.py:1382`; `app/chamados/models.py:1391`; `app/chamados/models.py:1482`) |
| Retratar duplicata | Evento depois da porta `carimbar_identidade`; não presumir que duplicata respondida morreu, porque C6 avalia resposta antes da identidade. (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2617`; `app/chamados/juizes.py:1150`) |
| Validação | Observar o desfecho da materialização, nunca o carimbo intermediário. O `atomic` existe justamente para impedir morte temporária observada por sinais; o laço final atualmente fica fora desse bloco. (`app/chamados/services/validacao.py:387`; `app/chamados/services/validacao.py:395`; `app/chamados/services/validacao.py:430`) |
| Reapontar | Capturar a âncora anterior e emitir sobre origem e destino; hoje a porta trava e protege apenas o destino. Âncora é `chamado_destino` ou pai da disputa. (`app/chamados/services/fio.py:116`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:845`) |
| Morte por leitura | C6 já responde, mas morte pode decorrer de relógio sem `save`; sobrepor `save()` não basta. O caso está expressamente documentado no modelo. (`app/chamados/juizes.py:1197`; `app/chamados/juizes.py:1214`; `app/chamados/juizes.py:660`; `app/chamados/models.py:2346`) |

O menor encadeamento defensável reutiliza `emitir_evento_celula`, consumidor existente, portas de reata, `reconciliar()` e fábrica. O INSERT permanece único e deve compartilhar a transação do ato; o consumidor relê o estado atual. (`app/ponto/portas/celula.py:664`; `app/ponto/portas/celula.py:672`; `app/chamados/reconciliador.py:1283`; `app/chamados/models.py:607`)

Entretanto, **chamar apenas a fábrica nos seis eventos não é uma cura integral**: `motivos_a_perguntar` exclui identidades históricas sem consultar C6; a pergunta humana decidida não deve ganhar irmã nem ter validação apagada. A saída legítima deve ser comprovada pela porta administrativa C17, depois de tentar a emissão devida — usá-la antes da emissão faria todo chamado novo em fila parecer “já alcançável” e dispensaria a pergunta. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/juizes.py:1344`; `app/chamados/juizes.py:1375`)

A lavra temporal existente cobre parte da morte por relógio, sem juiz novo: `lavrar_perguntas_stale` chama `lavrar`, agendado a cada cinco minutos. Seu universo atual exclui perguntas sem destino explícito, mesmo quando o pai está vivo; ampliar esse leitor pela âncora existente é necessário para declarar cobertura dessa classe. O teto de 500 também impede prometer processamento imediato. (`app/chamados/management/commands/lavrar_perguntas_stale.py:35`; `app/chamados/management/commands/lavrar_perguntas_stale.py:56`; `app/chamados/management/commands/lavrar_perguntas_stale.py:87`; `app/config/crons.py:390`; `app/chamados/services/fio.py:116`)

Sobrepor `save()` dos dois modelos é maior que tocar as fontes e ainda deixa `QuerySet.update` descoberto; ressuscitação também ocorre diretamente pelo cartório. Comparar timestamps de lavra produziria eventos continuamente; comparar atributos fora de `update_fields` notificaria mudanças não persistidas. (`app/chamados/models.py:2652`; `app/chamados/models.py:1451`; `app/chamados/veredito_lavrado.py:229`; `app/chamados/veredito_lavrado.py:231`)

Se o diff tocar `EventoCelula`, os leitores adicionais são os vigias, métricas, semeadura e consumidor: `app/chamados/reconciliador.py:1163`, `:1214`, `:1222`, `:1229`, `:1241`, `:1263`, `:1276`, `:1309`, `:1338`, `:1353`; acionadores em `app/chamados/management/commands/consumir_eventos_celula.py:31` e `app/chamados/management/commands/reconciliar_chamados.py:62`.

A distribuição dessas classes nos 494 e a suficiência de qualquer diff sobre eles permanecem **[nao verificado]**.

## 05/11 18:08 · sessao 43bcd2a988ec · - · - · origem=Ronald (tmux)

Pontos críticos para C/D:

1. **Reler e chamar C6 não basta.** `resposta='validar'` preenche `validada_em` usando o texto que estiver na instância; C6 retorna morto imediatamente pela validação. Se a releitura trouxer uma resposta nova, a porta pode validar uma fala que o ato anterior não julgou. Precisa comparar os insumos da instância recebida com a linha travada antes de aplicar a candidatura. `app/chamados/models.py:2576`; `app/chamados/models.py:2580`; `app/chamados/juizes.py:1136`.

2. **Preservar identidade humana.** Aplicar o candidato automático antes de julgar troca `admin_decidiu` por outra via; a célula administrativa continua permitindo C6 morto. A fonte única das vias de identidade já existe; recusar sobreposição não precisa de juiz novo. `app/chamados/models.py:2545`; `app/chamados/models.py:2535`; `app/chamados/juizes.py:1147`; `app/chamados/services/relavra_via.py:161`.

3. **Forma curta da alteração na fonte:** inserir transação dentro de `carimbar`; fotografar campos do ato recebido; reler com `select_for_update(of=('self', 'disputa')).select_related('disputa', 'celula')`, equivalente à superfície de trava do caminho inverso; invalidar memo; recusar comando obsoleto ou sobreposição de identidade; aplicar o corpo canônico e consultar C6. `app/chamados/models.py:2551`; `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`.

Trecho textual de releitura possível, com compatibilidade de execução **[nao verificado]**; o projeto fixa Django 5.2.14. `app/requirements.txt:13`.

```diff
@@
-        quando = quando or _tz.now()
+        campos_do_ato = (
+            'disputa_id', 'celula_id', 'motivo', 'data_evento',
+            'tipo_resposta', 'chamado_destino_id', 'resposta_colab',
+            'respondida_em', 'validada_em', 'validada_por_id',
+            'materializada_em', 'materializada_por_id', 'via_resolucao',
+        )
+        recebida = tuple(getattr(self, campo) for campo in campos_do_ato)
+        # Dentro do atomic que envolve também julgamento e save:
+        self.refresh_from_db(
+            from_queryset=type(self).objects
+                .select_for_update(of=('self', 'disputa'))
+                .select_related('disputa', 'celula'))
+        self.__dict__.pop('_pv_memo', None)
+        if recebida != tuple(getattr(self, campo) for campo in campos_do_ato):
+            return False
+        if (self.materializada_em is not None
+                and self.via_resolucao in self.vias_de_identidade()):
+            return False
+        quando = quando or _tz.now()
```

O trecho acima é **parte** do diff: o `atomic` deve envolver até a escrita, e sua saída deve ficar dentro do `try` que restaura carimbo/lavra. Apenas adicionar `@transaction.atomic` cria um contraexemplo novo: erro no COMMIT acontece fora dos `except` atuais e pode deixar carimbo em memória após rollback. `app/chamados/models.py:2584`; `app/chamados/models.py:2594`; `app/chamados/models.py:2600`.

Não comparar todos os `concrete_fields`: lavra e recusa podem mudar durante o próprio plantio sem mudar a autorização sobre a resposta; a porta deve fotografar os insumos do ato, mantendo a lavra como resultado derivado. Os escritores desses resultados estão em `app/chamados/veredito_lavrado.py:211`; `app/chamados/services/materializacao.py:817`.

4. **Memo:** a chave cobre validação, resposta, via, materialização, data, tipo, célula/veredito e destino/status; omite motivo, ata/DNA/trabalha, tranca e instante. A limpeza no PEP impede reutilizar o resultado anterior nesse ato, mas não corrige globalmente a completude da chave. `app/chamados/juizes.py:811`; `app/chamados/juizes.py:835`; `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:1111`; `app/chamados/juizes.py:1140`; `app/chamados/juizes.py:1194`.

5. **Contraexemplo residual decisivo — ordem inversa.** Mesmo o PEP acima não fecha sozinho todas as intercalações:
   - T1 trava pergunta e prepara validação/carimbo.
   - T2, com instância antiga sem validação/carimbo, prepara resposta nova; seu UPDATE espera T1.
   - T1 grava e libera.
   - T2 salva texto/instante novos, mas não limpa a validação recém gravada: a seleção de campos ocorreu sobre a instância antiga.

Essa sequência é possível pela forma do escritor de resposta; ocorrência em produção **[nao verificado]**. Para cobrir as duas ordens, o escritor canônico da resposta precisa reler sob trava antes de decidir quais campos limpar — continua sendo o mesmo escritor. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1285`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/services/disputa_emissao.py:1300`.

Também `validar_pergunta` busca e testa resposta antes do `atomic`, permitindo julgamento sobre fotografia anterior à fala gravada. `app/chamados/services/validacao.py:359`; `app/chamados/services/validacao.py:363`; `app/chamados/services/validacao.py:395`.

6. **Ordem de trava não demonstrada:** o inverso declara `of=('self', 'disputa')`; isso não comprova por leitura a ordem efetiva de aquisição nem ausência de ciclo com chamadores que já escreveram chamado, disputa ou batida. Há comentário explícito exigindo chamado antes de pergunta, e `_materializar_uma` planta batida antes de entrar em `carimbar`. Medição exigida pelo aval permanece **[nao verificado]**. `app/chamados/services/relavra_via.py:221`; `app/chamados/models.py:263`; `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1199`.

7. **Rollback externo permanece fora do alcance isolado de `carimbar`:** o chamador pode abrir uma transação externa e revertê-la depois do retorno positivo; a instância permanece em memória. O materializador efetivamente mantém uma transação externa envolvendo plantio e carimbo. `app/chamados/services/materializacao.py:1142`; `app/chamados/services/materializacao.py:1201`.

_1933 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
