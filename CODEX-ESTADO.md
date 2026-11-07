# CODEX-ESTADO

> Ultima resposta processada: **07/11/2026 04:39** · publicado em **07/11/2026 04:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 07/11 04:39 · sessao e0720c2a392d · FIO-E0-V4-E2 · verificador · origem=Code

**A cura do leitor está presente; a cura integral de `data_evento` não está fechada:** ainda existe caminho de nascimento que aceita dia civil do marco, e o migrador deixa partes do acervo nesse significado. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/disputa_emissao.py:1037`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:186`)

A cópia não contém histórico Git utilizável: o diff exato `0c0db89f → cdff0483 → 129754b9`, a atribuição histórica dos resíduos e os RED/GREEN citados ficam **[não verificado]**. Execução da bateria, custo em produção e ocorrência dos contraexemplos no acervo também ficam **[não verificado]**.

1. **Procedentes da volta 2**

   - **b — releitura civil/véspera: curada no leitor.** A pergunta entra exclusivamente por `p.data_evento == dia`; a carga filtra `[ini, fim]`, sem acrescentar D+1. Não achei a heurística anterior nesses caminhos. Isso depende de os escritores entregarem o dia correto — há resíduos descritos abaixo. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:252`; `app/chamados/services/tarefa_do_dia.py:292`)
   - **d — forma agregada: explicação coerente com o comportamento.** O leitor agregado devolve marcos vazios; a tarefa responde `nao_sei/ata_sem_lampadas` em dia de trabalho sem instâncias. Não achei contraexemplo nesses ramos. O teste E2 usa lâmpadas vazias, sem montar explicitamente a ata agregada com contagens. (`app/escala/services/leitor_celula.py:170`; `app/chamados/services/tarefa_do_dia.py:160`; `app/chamados/tests/test_e2_tarefa_do_dia.py:382`)
   - **f — limite inferior: curado.** A guarda é bilateral e o teste exige `ValueError` tanto em D−1 quanto em D+1; o mutante `dia > fim` seria alcançado pelo caso D−1. (`app/chamados/services/tarefa_do_dia.py:290`; `app/chamados/tests/test_e2_tarefa_do_dia.py:435`)
   - **Derivação paralela de posição: removida entre os dois consumidores.** Ambos chamam `posicao_do_marco`; o selo troca essa autoridade e observa os dois leitores. A cobertura dos argumentos tem uma lacuna, detalhada no item 5. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453`)

2. **Quem escreve `PerguntaDisputa.data_evento`**

   O censo AST localizou três sítios de persistência: `_nascer`, `reapontar_resolvedora` e `rechavear_data_evento`; são as três portas declaradas pelo selo. O selo verifica o endereço da escrita, sem validar a semântica do valor recebido. (`app/chamados/models.py:2578`; `app/chamados/services/fio.py:838`; `app/chamados/services/fio.py:904`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:136`)

   | Porta/caminho | Origem efetiva e conclusão |
   |---|---|
   | `abrir → _nascer` | Exige data não nula e grava o argumento; não confere sua origem. A anotação usa um `NewType`, e o construtor de dia da célula apenas devolve o dia recebido. (`app/chamados/models.py:2481`; `app/chamados/models.py:2578`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2943`) |
   | Emissão por batida e entradas pendentes | Consultam `dia_do_turno_da_batida`; sem resposta do juiz, não produzem a pergunta datada correspondente. (`app/escala/servico_jornada.py:485`; `app/chamados/services/disputa_emissao.py:1094`; `app/chamados/services/disputa_emissao.py:282`) |
   | Complementos e declarações | Usam o dia retornado pelo marco faltante da célula; o complemento exige igualdade com o dia solicitado. (`app/chamados/services/disputa_emissao.py:850`; `app/chamados/services/declaracao_texto.py:315`; `app/chamados/services/declaracao_texto.py:343`; `app/chamados/services/declaracao_texto.py:631`) |
   | Geração geral | Aceita dia da grade, depois contexto do motivo, depois batida; o dia resultante segue para `abrir`. Contexto antigo não recebe conversão semântica nesse percurso. (`app/chamados/services/disputa_emissao.py:350`; `app/chamados/services/disputa_emissao.py:367`; `app/chamados/services/disputa_emissao.py:385`; `app/chamados/services/disputa_emissao.py:433`) |
   | Sem batida de origem | Usa `data_evento_do_contexto`, que tenta cinco chaves, incluindo `horario_previsto`; pode receber o dia civil desse instante e passá-lo ao nascimento. (`app/chamados/data_evento.py:27`; `app/chamados/data_evento.py:39`; `app/chamados/services/disputa_emissao.py:1121`; `app/chamados/services/disputa_emissao.py:1133`) |
   | `abrir_conversa` | **Grava dia civil**, por `localdate()`. Inclui reabertura administrativa e âncora ausente; essas linhas novas ficam fora da E2 por `conversa=False`. (`app/chamados/models.py:2494`; `app/chamados/services/acoes_disputa.py:260`; `app/chamados/services/disputa_emissao.py:1251`; `app/chamados/services/tarefa_do_dia.py:253`) |
   | `reapontar_resolvedora` | Só preenche data ausente. Recebe dia do chamado/contexto no reconciliador ou dia do marco na declaração; não corrige data antiga já preenchida. (`app/chamados/services/fio.py:836`; `app/chamados/reconciliador.py:941`; `app/chamados/reconciliador.py:1625`; `app/chamados/reconciliador.py:1641`; `app/chamados/services/declaracao_texto.py:336`) |
   | Rechaveamento | Grava o argumento, após gates de colisão, célula e resolvedora. A migração normal calcula o novo dia pelo juiz; `--reverter` restaura a data antiga do JSON e pode reintroduzir dia de saída. (`app/chamados/services/fio.py:893`; `app/chamados/services/fio.py:898`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:142`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:336`) |

   **Escritor vivo ainda incorreto para pergunta de marco:** o ramo `intervalo_ausente` lê `marcos_faltantes[*].data`, passa por `dia_do_turno_da_celula` e cria a pergunta. Esse helper não converte datas; o formato do detector contém a data civil do instante encadeado do marco. A emissão do detector foi aposentada, mas o consumidor de chamados legados continua executável. (`app/chamados/services/disputa_emissao.py:1027`; `app/chamados/services/disputa_emissao.py:1037`; `app/chamados/services/disputa_emissao.py:1043`; `app/ponto/turnos.py:2939`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`; `app/ponto/management/commands/detectar_intervalo_ausente.py:149`)

   **Contraexemplo estático:** turno D, intervalo ausente às 01h de D+1, contexto com `data=D+1`: nasce pergunta de intervalo em D+1. O E4-c permite a diferença de um dia quando o turno cruza meia-noite, inclusive quando não sabe; portanto esse gate não garante a correção. Ocorrência produtiva: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1035`; `app/chamados/services/disputa_emissao.py:1043`; `app/chamados/juizes.py:1993`)

   **Absorção e reabertura não saneiam a data.** A absorção lê a data existente, cria a ausência nela e registra a decisão; a reabertura altera resposta/carimbos, preservando o dia. O backfill E1 de decisões é somente DRY, e a 0051 preenche colaborador/instância, preservando a indistinguibilidade das conversas antigas. (`app/chamados/services/materializacao.py:517`; `app/chamados/services/materializacao.py:522`; `app/chamados/services/materializacao.py:560`; `app/chamados/services/disputa_emissao.py:1462`; `app/chamados/services/disputa_emissao.py:1495`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:84`; `app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/migrations/0051_decisao_pergunta.py:25`)

   **O migrador deixa de fora:**

   - Intervalos/outros motivos, tipos diferentes de `hora`, datas nulas e recortes fora de `--desde/--ate`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:35`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:186`)
   - Absorções sem batida plantada viva, fontes ausentes/ambíguas, `orfao_14h` materializada, colisões e competências exportadas; a porta ainda pode recusar célula/destino. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:51`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:257`; `app/chamados/services/fio.py:895`)
   - Algumas linhas desaparecem da classificação sem registro `FORA`: retornos `None` de `_fonte_viva` e materializadas cuja saída não coincide com a data gravada. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:136`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:140`)

   Há um contraexemplo adicional: pergunta antiga do turno D21 gravada em D22, existindo outro turno aberto D22. `_fonte_viva` considera a pergunta coerente pela existência desse segundo turno antes de procurar a saída prevista de D21; a classificação a ignora. Ocorrência produtiva: **[não verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:90`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`)

   **A tarefa não transforma essas exclusões automaticamente em `nao_sei`.** Ela atribui a pergunta ao dia gravado. Em D21, marco apagado/vencido, sem cobertura nem outra pergunta, responde `sem_pergunta → perguntar`; em D22 pode consumir a pergunta legada como pertencente a D22. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:220`; `app/chamados/services/tarefa_do_dia.py:257`)

3. **`posicao_do_marco`**

   A regra atual prioriza primeiro índice, depois último, depois tipo: `inicio`; `fim`; `int_ida` para `S`, senão `int_volta`. Para `i=0,n=1`, vence `inicio`. Os dois consumidores passam os três argumentos correspondentes. A identidade universal com o corpo histórico de `0c0db89f` permanece **[não verificado]**. (`app/chamados/services/declaracao_texto.py:79`; `app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`)

   Não achei terceiro sítio da mesma regra na busca por `i == 0`, `n - 1`, `int_ida` e `int_volta`; os dois consumidores encontrados delegam ao helper. A tabela congelada da migração é motivo→instância, sem julgamento por índice. Completude além desses padrões: **[não verificado]**. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/migrations/0051_decisao_pergunta.py:14`)

4. **Carga e queries**

   Não achei leitura de `inst_por_dia` de outro dia: os dois acessos usam `dia`, e a carga entrega somente `{dia: instancias(...)}`. (`app/chamados/services/tarefa_do_dia.py:161`; `app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/services/tarefa_do_dia.py:292`)

   O selo continua mordendo um N+1 genérico por colaborador ou dia: mede construção **e consumo**, comparando dois colaboradores × três dias com um × um. Porém sua fixture não acrescenta decisões; um SELECT por pergunta apenas no ramo de validação escaparia desse selo de custo. Execução: **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:537`; `app/chamados/tests/test_e2_tarefa_do_dia.py:543`; `app/chamados/tests/test_e2_tarefa_do_dia.py:550`; `app/chamados/services/tarefa_do_dia.py:126`)

5. **Mordida dos testes** [nao verificado]

   Os testes reescritos atingem diretamente a releitura D22→D21 e a guarda inferior; o selo novo atinge a substituição do helper por uma cópia local. (`app/chamados/tests/test_e2_tarefa_do_dia.py:200`; `app/chamados/tests/test_e2_tarefa_do_dia.py:206`; `app/chamados/tests/test_e2_tarefa_do_dia.py:435`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453`)

   **Mutante sobrevivente à bateria E2, por inspeção:** trocar a chamada de `_marcos_faltantes` para `posicao_do_marco(i, n, 'S')`. O substituto do selo ignora `tipo`; no uso real, uma entrada intermediária passaria de `int_volta` para `int_ida`. A suíte mais ampla contém teste que deveria matá-lo, esperando explicitamente `int_volta/E`; execução **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:453`; `app/chamados/services/declaracao_texto.py:83`; `app/chamados/services/declaracao_texto.py:134`; `app/chamados/tests/test_hx_ata_nao_sabe.py:69`)

   Outro sobrevivente E2 por inspeção é inverter a prioridade dos dois `if`: altera o caso `i=0,n=1`, ausente das montagens de dois/quatro marcos dessa bateria. Execução: **[não verificado]**. (`app/chamados/services/declaracao_texto.py:79`; `app/chamados/tests/test_e2_tarefa_do_dia.py:65`; `app/chamados/tests/test_e2_tarefa_do_dia.py:176`)

6. **Checklist**

   - **Segundo juiz:** não achei clone novo de teto, vínculo ou cobertura; esses caminhos delegam às autoridades. Há derivador paralelo preexistente no caminho escritor: `data_evento_do_contexto` tenta chaves próprias, enquanto o juiz registrado lê a chave declarada no catálogo. (`app/ponto/turnos.py:3052`; `app/ponto/turnos.py:3059`; `app/ponto/turnos.py:3072`; `app/chamados/data_evento.py:27`; `app/chamados/catalogo/modulos.py:219`; `app/docs/JUIZES.md:56`)
   - **Segundo escritor:** não achei persistência fora das três portas declaradas no censo; o erro de intervalo atravessa uma porta permitida, portanto o selo P13 não o detecta. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`; `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:136`; `app/chamados/services/disputa_emissao.py:1043`)
   - **Relógio fora da borda:** a E2 passa `agora` à lavra vigente, fila e teto. Permanece a questão externa já triada: o escritor da lavra usa `date.today()`/`timezone.now()`, enquanto a validade lê `agora.date()`. (`app/chamados/services/tarefa_do_dia.py:270`; `app/chamados/services/tarefa_do_dia.py:278`; `app/chamados/services/tarefa_do_dia.py:285`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:182`; `app/chamados/veredito_lavrado.py:228`)
   - **Fonte única e selo sem mordida:** posição centralizada; selo de delegação incompleto quanto aos argumentos; custo não cobre o ramo de validação. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453`; `app/chamados/tests/test_e2_tarefa_do_dia.py:537`)
   - **ESMERIL-SEMÂNTICA:** o campo ainda comporta dia civil da conversa e dia do turno do fato; `conversa` distingue as linhas novas, mas o legado não foi distinguido pela 0051. Além disso, o caminho de intervalo pode introduzir dia civil dentro de pergunta do fato. (`app/chamados/models.py:2483`; `app/chamados/models.py:2494`; `app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/services/disputa_emissao.py:1043`)

## 07/11 04:38 · sessao 2e3f23bb9008 · - · - · origem=Ronald (tmux)

Censo somente leitura de `app/`, migradores e chamadas às portas: encontrei estas escritas persistentes e origens.

- **Nascimento:** `PerguntaDisputa._nascer` grava `data_evento` no `create`; `abrir` exige apenas valor não nulo, sem conferir dinamicamente que seja `DiaDoTurno`. A anotação sozinha não converte nem valida datas. (`app/chamados/models.py:2470`, `app/chamados/models.py:2481`, `app/chamados/models.py:2578`)
- **Costura de pergunta sem data:** `reapontar_resolvedora` só grava o parâmetro quando `p.data_evento` está vazio; data entra antes do E4-c e da lavra. Chamadores com parâmetro estão no reconciliador e na declaração. (`app/chamados/services/fio.py:836`, `app/chamados/services/fio.py:851`, `app/chamados/reconciliador.py:941`, `app/chamados/reconciliador.py:1625`, `app/chamados/reconciliador.py:1641`, `app/chamados/services/declaracao_texto.py:336`)
- **Rechaveamento:** grava o valor trazido pelo chamador, recusa colisão/ausência de célula quando havia célula anterior/E4-c, relavra e salva. A função não consulta a origem para garantir que o novo valor é dia de turno. (`app/chamados/services/fio.py:884`, `app/chamados/services/fio.py:893`, `app/chamados/services/fio.py:898`, `app/chamados/services/fio.py:904`)
- **Conversa continua escrevendo dia civil:** `abrir_conversa` passa `timezone.localdate()` com `conversa=True`; isso inclui confirmação de reabertura administrativa e âncora ausente. A carga E2 exclui as conversas pela coluna. (`app/chamados/models.py:2487`, `app/chamados/models.py:2494`, `app/chamados/services/acoes_disputa.py:259`, `app/chamados/services/disputa_emissao.py:1251`, `app/chamados/services/tarefa_do_dia.py:252`)
- **Absorção não escreve `data_evento`:** lê a data da pergunta, cria ausência nessa data, carimba irmãs da mesma data e registra a decisão de absorção. Se o dia gravado já era civil errado, este caminho o propaga à ausência. (`app/chamados/services/materializacao.py:517`, `app/chamados/services/materializacao.py:521`, `app/chamados/services/materializacao.py:532`, `app/chamados/services/materializacao.py:558`)
- **Reaberturas de pergunta preservam o dia:** `reabrir_fantasma`, `reabrir_por_resposta`, `reabrir_por_competencia` delegam a descarimbo, sem nova atribuição de `data_evento` nesses métodos. (`app/chamados/models.py:2825`, `app/chamados/models.py:2834`, `app/chamados/models.py:2837`, `app/chamados/models.py:2851`, `app/chamados/models.py:2853`)
- **Backfill E1 não escreve datas:** a 0051 preenche colaborador e instância e deixa conversa antiga indistinguível, com default `False`; `backfill_decisoes_pergunta` é apenas DRY de linhas e colisões. (`app/chamados/migrations/0051_decisao_pergunta.py:4`, `app/chamados/migrations/0051_decisao_pergunta.py:22`, `app/chamados/migrations/0051_decisao_pergunta.py:99`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:26`)

**Portas cuja origem está correta:**

- Emissão agrupada por batida usa `resolver_data_evento`, que delega à autoridade `dia_do_turno_da_batida`; sem resposta não emite. (`app/chamados/services/disputa_emissao.py:1092`, `app/escala/servico_jornada.py:485`)
- Entradas pendentes são resolvidas pelos PKs das batidas, e não pela data civil do item. (`app/chamados/services/disputa_emissao.py:275`, `app/chamados/services/disputa_emissao.py:282`)
- Complementos leem o dia retornado por `_marcos_faltantes`, exigem igualdade com o dia pedido e o passam como dia da célula. (`app/chamados/services/disputa_emissao.py:836`, `app/chamados/services/disputa_emissao.py:850`)
- Declaração direta e aplicação de proposta usam o dia do marco faltante convertido nominalmente por `dia_do_turno_da_celula`. (`app/chamados/services/declaracao_texto.py:315`, `app/chamados/services/declaracao_texto.py:341`, `app/chamados/services/declaracao_texto.py:629`)

**Escritor vivo que ainda aceita dia civil do marco:**

O ramo dedicado a `intervalo_ausente` lê `contexto_json.marcos_faltantes[*]['data']`, passa esse mesmo dia por `_dtc_i` e cria pergunta de intervalo. Esse construtor **não converte**: retorna `DiaDoTurno(dia)`. O formato produzido por `furos_intervalo` contém explicitamente a data local do instante encadeado do marco — D+1 em intervalo após meia-noite. O emissor daquele command está aposentado, mas o consumidor que cria perguntas de chamados legados continua no código executável. (`app/chamados/services/disputa_emissao.py:1027`, `app/chamados/services/disputa_emissao.py:1037`, `app/chamados/services/disputa_emissao.py:1041`, `app/ponto/turnos.py:2939`, `app/ponto/management/commands/detectar_intervalo_ausente.py:77`, `app/ponto/management/commands/detectar_intervalo_ausente.py:82`, `app/ponto/management/commands/detectar_intervalo_ausente.py:149`)

Contraexemplo: chamado de intervalo do turno D com marco ausente às 01h de D+1 e `marcos_faltantes.data=D+1` → a pergunta nasce com D+1. O gate E4-c permite diferença simétrica de um dia se o turno cruza meia-noite, inclusive quando não sabe; portanto não bloqueia esta gravação. Existência de chamado produtivo nessa configuração: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1035`, `app/chamados/services/disputa_emissao.py:1043`, `app/chamados/juizes.py:1993`)

Há ainda fallback pelo derivador antigo `data_evento_do_contexto`: hierarquia por cinco chaves, terminando em `horario_previsto`, cujo dia pode ser o dia civil do marco. Esse valor alimenta tanto complementos de pergunta existente quanto novas perguntas sem origem. É derivação paralela ao juiz registrado do dia do chamado, sem validação da origem semântica da data. Ocorrência produtiva com fallback incorreto: **[não verificado]**. (`app/chamados/data_evento.py:16`, `app/chamados/data_evento.py:27`, `app/chamados/data_evento.py:39`, `app/chamados/services/disputa_emissao.py:935`, `app/chamados/services/disputa_emissao.py:963`, `app/chamados/services/disputa_emissao.py:1121`, `app/chamados/services/disputa_emissao.py:1133`, `app/chamados/catalogo/modulos.py:219`)

**Migração não cobre todo o acervo:**

- Consulta só `orfao_14h`/`saida_sem_entrada`, tipo `hora`, data não nula; intervalos e outros motivos ficam fora desde a consulta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:35`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:186`)
- Materializada sem batida plantada viva — incluindo absorção — fica fora; duas fontes ou ausência de turno também. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:47`)
- `orfao_14h` materializada explicitamente não migra; colisão também não. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:150`)
- Materializada cuja saída não coincide com o dia gravado é ignorada; vivas sem candidato também podem sumir por `continue`, sem registro FORA. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:84`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:104`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:136`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:140`)
- Competência exportada em dia velho ou novo fica fora; no apply, recusas da porta não contam automaticamente como erro de rollback, embora erros e descarimbo contem. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:233`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:253`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:226`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:289`)
- `--reverter` pode reintroduzir o significado velho, pois lê a data anterior do JSON e a passa diretamente a `rechavear_data_evento`. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:336`)

Outro contraexemplo estático: pergunta antiga do noturno D21 gravada D22, com um segundo turno aberto D22. `_fonte_viva` a considera coerente pela existência do turno D22 antes de procurar a saída esperada de D21, devolve `None` e a classificação a ignora. Ocorrência em produção: **[não verificado]**. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:90`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:136`)

A E2 **não marca automaticamente essas exclusões como `nao_sei`**: carrega por `data_evento` literal e associa por igualdade literal. Assim, pergunta errada em D+1 é considerada em D+1; em D, se o marco está vencido/apagado, sem cobertura e sem outra pergunta, responde `sem_pergunta` com ação `PERGUNTAR`. Isso é consequência do contrato sem releitura, mas depende de a E1/migração entregar a data correta. (`app/chamados/services/tarefa_do_dia.py:252`, `app/chamados/services/tarefa_do_dia.py:176`, `app/chamados/services/tarefa_do_dia.py:220`)

O selo P13 prova endereço do escritor, não a semântica da origem que chega a ele: sua whitelist permite `_nascer`, `reapontar_resolvedora`, `rechavear_data_evento`; o ramo intervalo passa pela porta permitida e não é pego por ele. (`app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:30`, `app/chamados/tests/test_selo_p13_data_evento_porta_unica.py:136`)

## 07/11 04:37 · sessao 69241ecf9b7c · - · - · origem=Ronald (tmux)

- **Não achei acesso a `inst_por_dia` de outro dia.** Os únicos acessos são `.get(dia)` em `app/chamados/services/tarefa_do_dia.py:161` e `:178`; `CargaDaTarefa.tarefa` fornece exatamente `{dia: self.instancias(cid, dia)}` em `:292`. A carga de perguntas também usa o intervalo fechado `[ini, fim]`, sem `d+1`, em `:252-255`; a guarda recusa ambos os lados fora da janela em `:290-291`.

- **O selo ainda morde um N+1 genérico por colaborador ou dia.** A medição envolve construir a carga e percorrer cada colaborador/dia (`app/chamados/tests/test_e2_tarefa_do_dia.py:543-549`), comparando 2 colaboradores × 3 dias com 1 × 1 (`:550`). Uma query por colaborador adicionaria 2 contra 1; por célula, 6 contra 1. A execução e seu resultado são **[não verificado]**.

- **Limite do selo:** os três fatos da fixture são perguntas sem decisões explicitamente acrescentadas (`app/chamados/tests/test_e2_tarefa_do_dia.py:537-539`); a função só chega aos ramos de validação depois de haver decisão (`app/chamados/services/tarefa_do_dia.py:115-126`). Um mutante que acrescentasse um `SELECT` por chamada dentro do ramo `ult.ato == D.VALIDACAO` (`:126`) escaparia desse selo de queries. A passagem pela bateria inteira é **[não verificado]**, mas os testes de validação examinados não impõem contagem total de queries; os selos SQL específicos vetam `eventocelula` (`app/chamados/tests/test_e2_tarefa_do_dia.py:313`) ou `folha_` (`:491`), e o selo de pureza veta somente escrita/lock (`:505-508`).

- **Não achei segundo juiz para teto/vínculo/cobertura nos caminhos examinados.** O vínculo delega a `vinculo_do_dia` (`app/ponto/turnos.py:3049-3054`); o teto delega a `cartorio.marcos_com_teto` (`:3056-3062`), que chama `marcos_turno` (`app/ponto/services/cartorio.py:848-859`); a cobertura reaproveita `_pelo_efeito`, `_q_cobertura` e `_cobertura_pura` (`app/ponto/turnos.py:3064-3077`). A autoridade da tarefa consta do registro vivo (`app/core/juizes.py:150-155`, `:285`) e da tabela (`app/docs/JUIZES.md:73-80`).

- **Relógio da E2:** a carga entrega `agora` explicitamente a `esta_stale` (`app/chamados/services/tarefa_do_dia.py:267-270`), à fila (`:278-279`) e ao teto (`:285`). A casca recusa `None` e carga aberta em outro instante (`:309-314`). O relógio civil preexistente continua no escritor da lavra: `expira_em` usa `datetime.date.today()` (`app/chamados/veredito_lavrado.py:112`), e `lavrar` usa `timezone.now()` (`:228`); o leitor de validade compara `agora.date()` (`:181-185`). A E2 lê a lavra; não chama `lavrar` no caminho examinado (`app/chamados/services/tarefa_do_dia.py:266-270`).

- **Fontes únicas adicionais:** o passivo vem de `DecisaoPergunta.linhas_da_projecao_sem_linha` (`app/chamados/services/tarefa_do_dia.py:264-265`), cuja identificação é por ato e instante (`app/chamados/models.py:3409-3424`); o fechamento vem da lavra vigente (`app/chamados/services/tarefa_do_dia.py:126-134`, `:266-270`), sem rejulgar localmente o efeito.

- Não executei testes nem consultei banco/log; execução, RED/GREEN e custo em produção são **[não verificado]**. [nao verificado]

## 07/11 04:36 · sessao b74c85bf8bb9 · - · - · origem=Ronald (tmux)

- A extração está centralizada na cópia: `posicao_do_marco` prioriza `i == 0`, depois `i == n - 1`, depois `tipo == 'S'`; `_marcos_faltantes` passa `(i, n, tipo)` e `_posicoes` passa `(i, n, lp['tipo'])`. Para `i=0,n=1`, o resultado atual é `inicio`. Não certifiquei identidade universal com o predicado de `0c0db89f`, pois o histórico Git não está disponível. (`app/chamados/services/declaracao_texto.py:75-83`, `:129-134`; `app/chamados/services/tarefa_do_dia.py:55-58`; histórico: [não verificado].)

- Não achei terceiro sítio com essa regra na busca por `i == 0`, `n - 1`, `int_ida`, `int_volta` em arquivos Python. Os dois consumidores localizados usam o helper; a migração tem somente a tabela motivo→posição. (`app/chamados/services/declaracao_texto.py:134`; `app/chamados/services/tarefa_do_dia.py:58`; `app/chamados/migrations/0051_decisao_pergunta.py:15-19`; completude além dos padrões buscados: [não verificado].)

- O selo novo morde a reintrodução da cópia nos dois consumidores: substitui o helper por posições `P0…P3` e exige que ambos acompanhem. Mas o substituto ignora `n` e `tipo`; logo não demonstra preservação dos três argumentos. (`app/chamados/tests/test_e2_tarefa_do_dia.py:445-456`.)

- Mutante concreto sobrevivente à bateria E2 por inspeção: trocar, em `_marcos_faltantes`, `posicao_do_marco(i, n, tipo)` por `posicao_do_marco(i, n, 'S')`. O único exercício de `_marcos_faltantes` na bateria acontece sob o substituto que ignora `tipo`; em produção, uma entrada intermediária viraria `int_ida`. A suíte mais ampla contém teste que deveria matá-lo: espera explicitamente `int_volta/E` na terceira posição. (`app/chamados/services/declaracao_texto.py:83`, `:134`; `app/chamados/tests/test_e2_tarefa_do_dia.py:453-456`; `app/chamados/tests/test_hx_ata_nao_sabe.py:63-69`; execução: [não verificado].)

- Outro mutante sobrevivente à bateria E2 por inspeção: inverter a prioridade dos dois `if` do helper, testando fim antes de início. Só muda a interseção `i=0,n=1`, ausente das montagens da bateria, que usam quatro marcos ou os dois marcos do noturno. (`app/chamados/services/declaracao_texto.py:79-82`; `app/chamados/tests/test_e2_tarefa_do_dia.py:65-66`, `:176`, `:355`; execução: [não verificado].)

- Os testes reescritos atacam diretamente a releitura civil: pergunta de D22 não entra no turno D21, inclusive com D22 trabalhando; o teste de janela exige erro tanto em D−1 quanto em D+1. (`app/chamados/tests/test_e2_tarefa_do_dia.py:200-212`, `:431-437`.)

- A alteração documental sobre a forma agregada está coerente com o leitor: a tarefa retorna `ata_sem_lampadas` quando falta instância em dia de trabalho; o leitor agregado devolve marcos vazios, sem identificar quais faltaram. (`app/chamados/services/tarefa_do_dia.py:160-163`; `app/escala/services/leitor_celula.py:146-172`.)

- O selo de queries mede carga e consumo para 2 colaboradores × 3 dias contra 1 × 1, ambos com perguntas; assim, um SELECT adicional por colaborador ou dia nesse percurso aumentaria a contagem comparada. Não prova percursos que a montagem não ativa. (`app/chamados/tests/test_e2_tarefa_do_dia.py:531-550`.)

## 07/11 04:28 · sessao efc6cfc6aeb8 · FIO-E0-V4-E2 · verificador · origem=Code

**Não aprovo a volta 2 nesta cópia.** O principal problema é a nova ambiguidade aplicada a perguntas cujo nascimento já recebeu explicitamente o **dia do turno**. O teste novo exige essa ambiguidade e consolida o erro (`app/chamados/models.py:2477`; `app/chamados/tests/test_e2_tarefa_do_dia.py:351`; `app/chamados/tests/test_e2_tarefa_do_dia.py:355`).

A cópia não contém `.git`; comparação efetiva com `9bd00eca`, exclusividade dos três arquivos e execução da bateria/mutantes: **[nao verificado]**. Abaixo julgo o código disponível; não infiro ocorrências em produção.

**1. Procedentes a–g: cura no sítio e contraexemplo**

| Item | Julgamento |
|---|---|
| **a — conversa** | **Curado para `conversa=True`**: a query agora exclui essas perguntas; o teste verifica que o marco continua sem pergunta (`app/chamados/services/tarefa_do_dia.py:278`; `app/chamados/tests/test_e2_tarefa_do_dia.py:338`). **Residual:** conversa antiga recebe `False` na migração e pode continuar ocupando o marco; a própria migração declara essa indistinguibilidade (`app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/migrations/0051_decisao_pergunta.py:100`). |
| **b — noturnos seguidos** | **A restrição proposta está implementada**, mas não constitui cura semântica completa: havendo posição nos dois dias, devolve `None` (`app/chamados/services/tarefa_do_dia.py:113`). **Contraexemplo:** dois noturnos 22–06, saídas apagadas, pergunta **nova** aberta para o turno d, sem atos: seu dono é conhecido pela porta de nascimento, mas a tarefa devolve `pergunta_de_turno_ambiguo`. O próprio teste fabrica esse caso e exige a resposta errada (`app/chamados/models.py:2477`; `app/chamados/tests/test_e2_tarefa_do_dia.py:346`; `app/chamados/services/tarefa_do_dia.py:232`). |
| **c — documento** | **Curado nos dois sítios:** cobertura e absorção entregam `entregar_documento` ao colaborador (`app/chamados/services/tarefa_do_dia.py:164`; `app/chamados/services/tarefa_do_dia.py:225`). Não achei contraexemplo nessa troca de dono; examinei estado inicial, transições, reabertura e os dois testes correspondentes (`app/ponto/services/ausencia.py:779`; `app/ponto/services/ausencia.py:767`; `app/ponto/services/ausencia.py:1119`; `app/chamados/tests/test_e2_tarefa_do_dia.py:357`; `app/chamados/tests/test_e2_tarefa_do_dia.py:366`). |
| **d — ata sem lâmpadas** | **Curado o “vazio conhecido”**: retorna `nao_sei=True` (`app/chamados/services/tarefa_do_dia.py:182`). Não achei contraexemplo ao resultado conservador; achei erro na explicação “ata não lavrada”: existe forma antiga **já lavrada**, agregada, sem lâmpadas (`app/escala/services/leitor_celula.py:146`; `app/chamados/services/tarefa_do_dia.py:183`). |
| **e — quatro mutantes** | Os testes têm asserções que distinguem fila vazia, expiração ignorada, decisões ordenadas por PK e supressão de folga removida (`app/chamados/tests/test_e2_tarefa_do_dia.py:392`; `app/chamados/tests/test_e2_tarefa_do_dia.py:409`; `app/chamados/tests/test_e2_tarefa_do_dia.py:415`; `app/chamados/tests/test_e2_tarefa_do_dia.py:424`). O relato de sete failures não foi reproduzido **[nao verificado]**. Há outros sobreviventes, no item 6. |
| **f — índice e janela** | **Implementados:** agrupamento por `(colaborador_id,data_evento)` e rejeição de dia fora dos dois limites (`app/chamados/services/tarefa_do_dia.py:282`; `app/chamados/services/tarefa_do_dia.py:315`). Não achei contraexemplo à guarda atual; o teste cobre somente o limite superior (`app/chamados/tests/test_e2_tarefa_do_dia.py:431`). |
| **g — caso 10** | **Curado o exagero textual:** a docstring admite que não fabrica competência exportada; o teste verifica ação exigível e ausência de consulta à folha (`app/chamados/tests/test_e2_tarefa_do_dia.py:27`; `app/chamados/tests/test_e2_tarefa_do_dia.py:459`). Exportação efetiva continua **[nao verificado]**. |

**2. Dono da pergunta: tabela completa da decisão**

Para `d = p.data_evento`, a função reduz todas as combinações a dois predicados:

- **V:** existe, na véspera, a posição pedida com `data_civil == d`.
- **D:** existe essa posição no turno d, em qualquer dia civil.

Essa é a decisão efetiva; não consulta batidas, atos, `trabalha`, tipo do dia nem a FK da célula da pergunta (`app/chamados/services/tarefa_do_dia.py:109`; `app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/services/tarefa_do_dia.py:113`).

| Posição | V | D | Dono devolvido |
|---|---:|---:|---|
| início | qualquer | qualquer | d |
| pausa/retorno/fim | falso | qualquer | d |
| pausa/retorno/fim | verdadeiro | falso | d−1 |
| pausa/retorno/fim | verdadeiro | verdadeiro | `None` |

A tabela é exaustiva para essas posições, conforme os únicos retornos da função (`app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/services/tarefa_do_dia.py:115`).

O cruzamento dos cadastros pedidos entra assim nessa tabela:

| Situação em cada dia | Como participa |
|---|---|
| **Diurno** | As posições ficam no próprio dia civil; portanto o diurno da véspera normalmente não satisfaz V (`app/escala/servico_jornada.py:80`). |
| **Noturno** | Fim e pausas satisfazem V somente quando seu instante encadeado caiu em d. Pausa anterior à meia-noite não satisfaz V (`app/escala/servico_jornada.py:75`; `app/escala/servico_jornada.py:82`). |
| **Folga** | Não possui tratamento especial no dono. Sem lâmpadas, não oferece posição; conservando lâmpadas antigas, participa como qualquer turno (`app/chamados/services/tarefa_do_dia.py:86`; `app/chamados/services/tarefa_do_dia.py:111`). O cadastro de folga com lâmpadas antigas existe na própria bateria (`app/chamados/tests/test_e2_tarefa_do_dia.py:421`). |
| **Sem célula** | Produz lista vazia de instâncias: V ou D fica falso, conforme o lado (`app/chamados/services/tarefa_do_dia.py:84`). |
| **Ata sem lâmpadas E/S** | Também produz lista vazia; isso é indistinguível de “posição inexistente” para o dono (`app/chamados/services/tarefa_do_dia.py:54`; `app/chamados/services/tarefa_do_dia.py:86`). |
| **Duas pausas** | As duas ocorrências têm a mesma `pos`, embora `instancia` receba sufixo `:2`; o dono compara `pos`, sem identificar qual pausa (`app/chamados/services/tarefa_do_dia.py:62`; `app/chamados/services/tarefa_do_dia.py:64`; `app/chamados/services/tarefa_do_dia.py:112`). |

Portanto, **noturno→diurno** e **noturno→noturno** ficam igualmente ambíguos para uma posição pós-meia-noite existente nos dois turnos; **noturno→folga sem lâmpadas**, **sem célula** ou **ata vazia** entrega à véspera. Os demais cruzamentos seguem diretamente V/D, inclusive folgas com lâmpadas residuais (`app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/services/tarefa_do_dia.py:115`).

**A premissa das “duas leituras vivas” está desatualizada para o nascimento atual.** O emissor chama `resolver_data_evento`, que agora devolve o dia do turno pela autoridade da batida; complementos da célula também passam seu dia do turno (`app/chamados/services/disputa_emissao.py:1094`; `app/escala/servico_jornada.py:474`; `app/escala/servico_jornada.py:487`; `app/chamados/services/disputa_emissao.py:858`). Há código específico para migrar o significado antigo; sua aplicação no acervo é **[nao verificado]** (`app/chamados/services/fio.py:859`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:8`).

Assim:

- **Decidível tornado ambíguo:** a pergunta nova dos dois noturnos, já citada; `abrir` recebe dia do turno e vincula a célula desse dia, mas o dono ignora essa informação (`app/chamados/models.py:2477`; `app/chamados/models.py:2530`; `app/chamados/services/tarefa_do_dia.py:101`).
- **Atribuição errada pelo contrato atual:** véspera noturna, célula d trabalhando mas com ata vazia, pergunta nova de fim para d: o dono entrega a d−1. Uma decisão de silêncio dessa pergunta pode então encerrar a exigência da véspera (`app/chamados/services/tarefa_do_dia.py:115`; `app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:198`).
- **Terceiro turno, fora de ambas as datas candidatas:** não achei; os únicos retornos são d, d−1 e `None`. Isso não prova pertencimento quando nenhuma posição foi identificada (`app/chamados/services/tarefa_do_dia.py:115`).
- **Segunda pausa:** continua incompleta antes mesmo do dono, pois `marcos_turno` monta somente o primeiro intervalo; a segunda pode ficar sem teto e sair como `marco_fora_do_turno` (`app/escala/servico_jornada.py:75`; `app/chamados/services/tarefa_do_dia.py:215`). Se a pergunta trouxer explicitamente `int_ida:2`, a comparação com `i['pos']` também não casa (`app/chamados/services/tarefa_do_dia.py:98`; `app/chamados/services/tarefa_do_dia.py:112`).

**3. Escritores e leitores dos dois campos**

**`PerguntaDisputa.conversa`:**

- **Escritores atuais:** `abrir` passa `False`; `abrir_conversa` passa `True`; `_nascer` persiste o valor (`app/chamados/models.py:2483`; `app/chamados/models.py:2494`; `app/chamados/models.py:2578`).
- **Portas de conversa encontradas:** reabertura administrativa, emissão sem dia resolvido e âncora ausente. Todas expressam conversa sem identidade de marco (`app/chamados/services/acoes_disputa.py:259`; `app/chamados/services/disputa_emissao.py:420`; `app/chamados/services/disputa_emissao.py:1251`).
- **Leitores explícitos encontrados:** identidade dos fatos, trava/deduplicação no nascimento, derivação da instância, censo de colisões e carga E2 (`app/chamados/models.py:2466`; `app/chamados/models.py:2508`; `app/chamados/models.py:2528`; `app/chamados/models.py:2972`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`; `app/chamados/services/tarefa_do_dia.py:278`).

**`True = sem fato` está coerente nesses escritores. O inverso `False = fato` não é garantido para o legado:** a migração atribui `False` às conversas antigas e deriva a instância somente pelo motivo (`app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/migrations/0051_decisao_pergunta.py:26`). O novo teste só força `True` numa pergunta moderna; não cobre esse residual (`app/chamados/tests/test_e2_tarefa_do_dia.py:341`).

**`Ausencia.status='aguardando_documento'`:**

- **Decisão e escrita:** `estado_inicial` determina o estado; criação grava esse resultado; alteração de tipo e reabertura voltam ao mesmo juiz; `mudar_estado` efetiva as transições (`app/ponto/services/ausencia.py:795`; `app/ponto/services/ausencia.py:333`; `app/ponto/services/ausencia.py:344`; `app/ponto/services/ausencia.py:543`; `app/ponto/services/ausencia.py:1119`; `app/ponto/services/ausencia.py:824`). O backfill histórico também classifica pelo documento/tipo (`app/ponto/migrations/0057_a4_backfill_pendente.py:35`).
- **Leitores coerentes:** modelo, contadores, escalonador, cobrança ao colaborador, mensagens de tela, endpoint de anexo e os dois ramos E2 (`app/ponto/models.py:701`; `app/ponto/services/ausencia.py:965`; `app/ponto/services/ausencia.py:993`; `app/ponto/services/ausencia.py:1019`; `app/ponto/views.py:2591`; `app/api/views.py:3117`; `app/chamados/services/tarefa_do_dia.py:164`; `app/chamados/services/tarefa_do_dia.py:225`).

**Não estão todos os leitores alinhados:** o payload dos chamados do app oferece “Anexar comprovante” somente para `status == 'pendente'`, ausente do vocabulário atual. Uma ausência `aguardando_documento` não recebe esse bloco (`app/api/views.py:1087`; `app/api/views.py:1091`; `app/ponto/models.py:701`).

**Não achei outro status válido não final cuja obrigação seja do colaborador:** os únicos abertos são documento e decisão; este último pertence ao DP. Permitir anexar durante a decisão não cria outra obrigação de documento (`app/ponto/services/ausencia.py:765`; `app/ponto/models.py:702`; `app/api/views.py:3117`).

**4. Ata lavrada com zero lâmpada E/S**

**Sim, o formato antigo agregado é reconhecido explicitamente.** Ele sabe quantidades, mas não identifica os marcos; logo `nao_sei` é defensável, enquanto “não foi lavrada” é falso como explicação geral (`app/escala/services/leitor_celula.py:146`; `app/escala/services/leitor_celula.py:216`; `app/chamados/tests/test_e2_tarefa_do_dia.py:38`).

**Ausência total:** o caminho examinado transforma o dia em `ausencia` e limpa suas colunas; `dia_sem_trabalho` reconhece esse tipo antes da nova guarda. Não achei falso positivo causado somente por esse caminho (`app/escala/utils.py:1220`; `app/escala/utils.py:1251`; `app/chamados/juizes.py:429`; `app/chamados/services/tarefa_do_dia.py:181`).

**Somente lâmpadas de outro tipo:** o ramo alternativo aceita tipos vindos dos marcos/colunas e existem colunas `I`; não achei trajetória comprovada de trabalho legítimo com somente `I`, sem E/S. Ocorrência em produção: **[nao verificado]** (`app/ponto/services/cartorio.py:688`; `app/escala/utils.py:1383`).

A cadeia de escrita encontrada é:

- **Produção:** `lampadas_do_dia`, pelo DNA ou pelas colunas; `ata_do_dia` incorpora a lista. A forma individual contém `tipo`, `hora`, `acesa`, `luz` e `tipo_real`; `acesa` pode ser indeterminada (`app/ponto/services/cartorio.py:565`; `app/ponto/services/cartorio.py:638`; `app/ponto/services/cartorio.py:677`; `app/ponto/services/cartorio.py:688`; `app/ponto/services/cartorio.py:704`).
- **Modificadores:** dispensa por ausência/justificativa acrescenta a dispensa; teto converte apagadas ainda não exigíveis em indeterminadas (`app/ponto/services/cartorio.py:773`; `app/ponto/services/cartorio.py:873`).
- **Persistência:** a porta da célula recebe/copia a ata e a salva (`app/ponto/portas/celula.py:986`; `app/ponto/portas/celula.py:1027`). A montagem da grade também chama o mesmo produtor numa cópia em memória (`app/escala/utils.py:1886`).

**5. Índice, ordem e janela**

**A ordem atual é global por PK**, mesmo juntando perguntas de d e d+1: a query ordena, os buckets preservam seus elementos, a junção é novamente ordenada e a seleção usa `ps[-1]`. Não achei inversão introduzida pelo agrupamento (`app/chamados/services/tarefa_do_dia.py:280`; `app/chamados/services/tarefa_do_dia.py:282`; `app/chamados/services/tarefa_do_dia.py:319`; `app/chamados/services/tarefa_do_dia.py:249`). Igualdade textual/comportamental contra o pai: **[nao verificado]**.

Os consumidores de lote encontrados são os testes, com datas internas, além do caso deliberadamente inválido. A casca abre janela exatamente no dia solicitado (`app/chamados/tests/test_e2_tarefa_do_dia.py:427`; `app/chamados/tests/test_e2_tarefa_do_dia.py:503`; `app/chamados/tests/test_e2_tarefa_do_dia.py:520`; `app/chamados/services/tarefa_do_dia.py:338`). A alimentação adjacente interna chama **`instancias`**, não `tarefa`, portanto não é bloqueada pelo `ValueError` (`app/chamados/services/tarefa_do_dia.py:318`).

Chamadas legítimas da sonda externa ou de futuros leitores fora da janela: **[nao verificado]**.

**6. Os testes mordem? Mutante sobrevivente** [nao verificado]

**Mordem os resultados locais declarados**, com a ressalva de que o caso dos dois noturnos exige uma ambiguidade indevida para pergunta recém-criada (`app/chamados/tests/test_e2_tarefa_do_dia.py:338`; `app/chamados/tests/test_e2_tarefa_do_dia.py:355`; `app/chamados/models.py:2477`).

Dois sobreviventes por **análise estática da bateria deste arquivo**:

- Trocar a guarda por `if dia > self.fim`. Retira a proteção de `d < ini`, preservando o único teste externo à janela, que solicita D+1 (`app/chamados/services/tarefa_do_dia.py:315`; `app/chamados/tests/test_e2_tarefa_do_dia.py:431`).
- Trocar `pos != 'inicio'` por `pos == 'fim'`. Perde o tratamento de pausas noturnas; os cadastros noturnos da bateria têm somente entrada e saída (`app/chamados/services/tarefa_do_dia.py:111`; `app/chamados/tests/test_e2_tarefa_do_dia.py:169`; `app/chamados/tests/test_e2_tarefa_do_dia.py:347`).

Passagem efetiva da bateria inteira com esses mutantes: **[nao verificado]**.

**7. Checklist**

| Verificação | Resultado |
|---|---|
| **Segundo juiz** | O juiz agregado está autorizado/registrado. Entretanto, `dono_da_pergunta` volta a inferir o dia já entregue pela autoridade de turno, produzindo a divergência demonstrada (`app/core/juizes.py:285`; `app/chamados/services/tarefa_do_dia.py:101`; `app/escala/servico_jornada.py:487`). |
| **Segundo escritor** | Não achei escritor persistente na E2: a carga consulta e monta estruturas em memória (`app/chamados/services/tarefa_do_dia.py:277`; `app/chamados/services/tarefa_do_dia.py:307`; `app/chamados/services/tarefa_do_dia.py:320`). |
| **Relógio fora da borda** | A E2 passa `agora` explicitamente. Permanece a pendência conhecida: expiração produzida com `date.today()` e lida com `agora.date()`, sem conversão local nesses pontos (`app/chamados/services/tarefa_do_dia.py:295`; `app/chamados/services/tarefa_do_dia.py:303`; `app/chamados/veredito_lavrado.py:112`; `app/chamados/veredito_lavrado.py:182`). |
| **Derivação paralela** | A classificação primeiro/último/meio está repetida em `_posicoes` e `_marcos_faltantes`; sua introdução neste delta é **[nao verificado]** (`app/chamados/services/tarefa_do_dia.py:57`; `app/chamados/services/declaracao_texto.py:123`). |
| **Selo/RED sem mordida** | Limite inferior da janela e pausas noturnas ficam descobertos pelos mutantes acima; o selo dos noturnos também cristaliza a premissa antiga (`app/chamados/tests/test_e2_tarefa_do_dia.py:431`; `app/chamados/tests/test_e2_tarefa_do_dia.py:347`; `app/chamados/tests/test_e2_tarefa_do_dia.py:355`). |
| **ESMERIL-SEMÂNTICA** | `conversa=False` mistura fato moderno e conversa legada; `data_evento` tem contrato atual único, mas a E2 o trata como duplo; `aguardando_documento` tem dono definido, com leitor do app ainda preso ao status morto (`app/chamados/migrations/0051_decisao_pergunta.py:5`; `app/chamados/models.py:2477`; `app/chamados/services/tarefa_do_dia.py:104`; `app/ponto/models.py:701`; `app/api/views.py:1087`). |

## 07/11 04:24 · sessao 85f50560761b · - · - · origem=Ronald (tmux)

Achado principal: o teste novo de dois noturnos cristaliza a semântica antiga. Ele cria pergunta nova com `data_evento=D22` pela porta `abrir` e exige ambiguidade; essa porta já define o parâmetro como dia do turno e vincula a célula desse dia. Portanto a pergunta recém-criada é decidível pelo contrato atual, independentemente da existência de passivo antigo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:107-111,346-355`; `app/chamados/models.py:2477-2484,2528-2530`.)

A premissa “disputa_emissao ainda grava dia civil” está desatualizada: o emissor chama `resolver_data_evento`, que entrega o dia do turno pela autoridade `dia_do_turno_da_batida`. (`app/chamados/services/disputa_emissao.py:1092-1104`; `app/escala/servico_jornada.py:474-488`.) O código reconhece acervo antigo e contém migração própria, com exclusões explícitas; isso demonstra a possibilidade de passivo, sem demonstrar sua quantidade nem aplicação em produção. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:8-20`; `app/chamados/services/fio.py:859-876`; estado de produção `[nao verificado]`.)

Mutantes sobreviventes por inspeção da bateria do arquivo:

- Trocar `if not (self.ini <= dia <= self.fim)` por `if dia > self.fim`: o novo teste só tenta `D+1`; as outras chamadas usam dias internos ou a casca que abre janela exata. O limite inferior fica sem mordida. (`app/chamados/services/tarefa_do_dia.py:315-316`; `app/chamados/tests/test_e2_tarefa_do_dia.py:427-432,491-525`.)
- Trocar `pos != 'inicio'` por `pos == 'fim'`: todos os cadastros noturnos da bateria têm apenas entrada e saída, sem pausa. Assim, perder o tratamento de intervalo noturno não altera esses resultados. (`app/chamados/services/tarefa_do_dia.py:111-115`; `app/chamados/tests/test_e2_tarefa_do_dia.py:168-177,346-355`.)

Execução desses mutantes e da suíte `[nao verificado]`; não executei testes que escrevem. [nao verificado]

Checklist:

- O juiz agregado está registrado; a convivência temporária com os cinco respondedores está declarada. (`app/core/juizes.py:150-155,285`; `app/docs/JUIZES.md:73-80`.)
- Há reinterpretacão paralela do dia já produzido pela autoridade: `dono_da_pergunta` volta a inferir o dono por geometria, enquanto o nascimento atual recebe dia do turno. (`app/chamados/services/tarefa_do_dia.py:101-115`; `app/escala/servico_jornada.py:474-488`; `app/chamados/models.py:2477-2484`.)
- Há também cópia da derivação de posição primeiro/último/meio, já presente em `_marcos_faltantes`; sem o pai, o nascimento dessa cópia no delta é `[nao verificado]`. (`app/chamados/services/tarefa_do_dia.py:54-64`; `app/chamados/services/declaracao_texto.py:110-128`.)
- Não achei escritor persistente em `tarefa_do_dia`: os trechos examinados fazem consultas e montam estruturas em memória. (`app/chamados/services/tarefa_do_dia.py:277-304,307-321`.)
- O relógio é passado explicitamente aos leitores usados pela carga; permanece a diferença entre `agora.date()` no consumidor e `date.today()` na produção da expiração, já deixada em fila pela triagem. (`app/chamados/services/tarefa_do_dia.py:295,303,310`; `app/chamados/veredito_lavrado.py:112,181-185`.)
- A documentação do teste ainda equipara ata sem lâmpadas a ata nunca lavrada; o leitor antigo reconhece expressamente ata agregada sem lâmpadas. (`app/chamados/tests/test_e2_tarefa_do_dia.py:38`; `app/chamados/services/declaracao_texto.py:112-115`.)

## 07/11 04:24 · sessao dca9a40ed118 · - · - · origem=Ronald (tmux)

Q4 — A frase “ata sem lâmpadas = ata não lavrada” é excessiva: o leitor reconhece explicitamente **ata antiga agregada**, já lavrada, sem identidade individual dos marcos. `nao_sei` continua coerente; a explicação causal é que está errada (`app/escala/services/leitor_celula.py:146-172`; `app/chamados/services/tarefa_do_dia.py:182-184`; `app/chamados/tests/test_e2_tarefa_do_dia.py:38`).

- Produtor das lâmpadas: `lampadas_do_dia`, ramo DNA e ramo alternativo; `ata_do_dia` incorpora a lista na ata (`app/ponto/services/cartorio.py:565`, `app/ponto/services/cartorio.py:622-704`). O vocabulário do DNA é E/S e contempla duas pausas (`app/escala/models.py:1652-1669`).
- O ramo alternativo aceita o tipo/hora do marco ou da coluna, portanto pode produzir outros tipos (`app/ponto/services/cartorio.py:682-692`). Existem colunas I para intervalo livre; não achei trajeto comprovado de trabalho com **somente** I e nenhuma E/S (`app/escala/utils.py:1383-1390`, `app/escala/utils.py:1823-1829`); ocorrência real `[nao verificado]`.
- Intermitente usa tipos E/S e horas posicionais `·I1`, `·I2`; não confundir esses rótulos com tipo I (`app/escala/utils.py:1392-1401`).
- Ausência total aprovada altera o dia para `ausencia` e limpa suas colunas; esse tipo satisfaz `dia_sem_trabalho`, então não cai no novo retorno antecipado (`app/escala/utils.py:1220`, `app/escala/utils.py:1251-1253`; `app/chamados/juizes.py:429`; `app/chamados/services/tarefa_do_dia.py:181-185`).
- Há modificadores da lista: dispensa por ausência/justificativa altera `acesa`, `luz`, `dispensada`; teto altera apagadas para `acesa=None, pendente=True` (`app/ponto/services/cartorio.py:766-779`, `app/ponto/services/cartorio.py:873-883`).
- Persistência passa por `lavrar_veredito`, que recebe e copia a ata (`app/ponto/portas/celula.py:986-1003`, `app/ponto/portas/celula.py:1027-1032`). A montagem na grade usa cópia em memória e o mesmo produtor, sem persistência (`app/escala/utils.py:1886-1897`). `carimbar_precedencia` preserva a lista e altera apenas `precedencia` (`app/ponto/portas/celula.py:1071-1082`).

Q5 — A implementação atual preserva ordem global por PK mesmo ao juntar perguntas de d e d+1: consulta ordenada, agrupamento por `(colaborador_id,data_evento)`, concatenação seguida de `sorted(key=pk)`; a escolha continua sendo `ps[-1]` (`app/chamados/services/tarefa_do_dia.py:277-282`, `app/chamados/services/tarefa_do_dia.py:319`, `app/chamados/services/tarefa_do_dia.py:249`). Igualdade com o pai `[nao verificado]`.

Os consumidores de lote encontrados estão nos testes: o caso explícito fora da janela e os dois selos com datas internas; a casca abre janela exatamente no dia solicitado (`app/chamados/tests/test_e2_tarefa_do_dia.py:427-432`, `app/chamados/tests/test_e2_tarefa_do_dia.py:491-505`, `app/chamados/tests/test_e2_tarefa_do_dia.py:507-525`; `app/chamados/services/tarefa_do_dia.py:337-341`). Sonda externa e futuros consumidores `[nao verificado]`.

A alimentação interna adjacente usa `instancias(d-1/d/d+1)`, não `tarefa(d-1/d+1)`, portanto não é barrada pelo novo `ValueError` (`app/chamados/services/tarefa_do_dia.py:307-318`).

Q6 — Mutante sobrevivente por inspeção: trocar a condição da janela para `if dia > self.fim`, deixando d<ini passar. O único teste dessa guarda solicita D+1; os demais lotes encontrados usam datas internas (`app/chamados/services/tarefa_do_dia.py:315-316`; `app/chamados/tests/test_e2_tarefa_do_dia.py:427-432`, `app/chamados/tests/test_e2_tarefa_do_dia.py:503-505`, `app/chamados/tests/test_e2_tarefa_do_dia.py:520-523`). Execução da bateria com esse mutante `[nao verificado]`.

## 07/11 04:24 · sessao 8cd90ff2efca · - · - · origem=Ronald (tmux)

Q3 — censo e conclusões, somente leitura:

- **Escritores atuais de `conversa` são coerentes com `True = sem identidade de fato`**: `abrir` passa `False`, `abrir_conversa` passa `True` e data civil de emissão; `_nascer` grava a coluna (`app/chamados/models.py:2483`, `app/chamados/models.py:2494`, `app/chamados/models.py:2578`). Chamadores de conversa: reabertura administrativa, até com motivo escolhido no catálogo (`app/chamados/services/acoes_disputa.py:247`, `app/chamados/services/acoes_disputa.py:260`); emissão sem dia resolvido (`app/chamados/services/disputa_emissao.py:420`); `ancora_ausente` (`app/chamados/services/disputa_emissao.py:1251`).
- **O inverso `False = pergunta do fato` não vale para o acervo**. A migration introduz a coluna com `default=False` para perguntas antigas (`app/chamados/migrations/0051_decisao_pergunta.py:99`), preenche instância apenas por motivo (`app/chamados/migrations/0051_decisao_pergunta.py:26`) e confessa que conversa antiga é indistinguível (`app/chamados/migrations/0051_decisao_pergunta.py:5`). Assim, conversa histórica com motivo `orfao_14h` e data coincidente ainda entra no filtro novo (`app/chamados/services/tarefa_do_dia.py:278`); pode ocupar a posição de pergunta do marco e gerar responder/validar segundo seus atos (`app/chamados/services/tarefa_do_dia.py:198`, `app/chamados/services/tarefa_do_dia.py:249`). A existência desse caso no dado de produção fornecido é `[nao verificado]`.
- **O teste novo não cobre esse legado**: cria pergunta pela porta moderna e força `conversa=True` (`app/chamados/tests/test_e2_tarefa_do_dia.py:341`, `app/chamados/tests/test_e2_tarefa_do_dia.py:342`). O backfill de decisões apenas lista colisões, igualmente filtrando `conversa=False`; não classifica conversas antigas (`app/chamados/management/commands/backfill_decisoes_pergunta.py:74`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:84`).
- **Leitores explícitos da coluna**: identidade `linhas_dos_fatos` (`app/chamados/models.py:2466`); trava/dedup no nascimento (`app/chamados/models.py:2508`, `app/chamados/models.py:2528`); derivação de instância na inserção (`app/chamados/models.py:2972`); censo de colisões (`app/chamados/management/commands/backfill_decisoes_pergunta.py:74`); carga E2 (`app/chamados/services/tarefa_do_dia.py:278`). O modelo documenta que a coluna identifica a porta que fez nascer a pergunta (`app/chamados/models.py:2247`).

`Ausencia`:

- **`aguardando_documento` corresponde à obrigação de documento do colaborador**, como a E2 agora supõe: choices nomeiam explicitamente os donos (`app/ponto/models.py:701`), `estado_inicial` retorna documento quando o tipo exige e não há anexo (`app/ponto/services/ausencia.py:779`, `app/ponto/services/ausencia.py:795`). Criação chama esse juiz e grava o estado (`app/ponto/services/ausencia.py:333`, `app/ponto/services/ausencia.py:344`); reabertura chama o mesmo juiz (`app/ponto/services/ausencia.py:1119`); alteração de tipo rejulga abertos (`app/ponto/services/ausencia.py:541`, `app/ponto/services/ausencia.py:543`).
- **Transição do status**: `mudar_estado` escreve em `ausencia.status` e salva (`app/ponto/services/ausencia.py:824`, `app/ponto/services/ausencia.py:835`). Anexo move documento→decisão (`app/ponto/services/ausencia.py:873`); escalonador vencido move documento→decisão do DP (`app/ponto/services/ausencia.py:1002`); `_reabrir` grava o estado calculado (`app/ponto/services/ausencia.py:1141`). Migration histórica classifica pendentes pelo anexo e tipo (`app/ponto/migrations/0057_a4_backfill_pendente.py:35`, `app/ponto/migrations/0057_a4_backfill_pendente.py:43`). Materialização ATESTADO deixa status vazio para o juiz, enquanto FALTA_DIA é aprovada (`app/chamados/services/materializacao.py:594`).
- **Leitores coerentes**: `pode_aprovar` aceita só `aguardando_decisao` (`app/ponto/models.py:741`); `esta_aberta` usa `ABERTAS` (`app/ponto/models.py:719`); contadores discriminam estados e vencimento (`app/ponto/services/ausencia.py:963`); cobrança faz push ao colaborador pedindo documento (`app/ponto/services/ausencia.py:1019`); view informa falta de documento (`app/ponto/views.py:2555`); reversão comunica explicitamente colaborador versus DP (`app/ponto/views.py:2591`); endpoint de anexo aceita ambos os estados abertos (`app/api/views.py:3117`). A E2 trata documento nos dois lugares: absorção (`app/chamados/services/tarefa_do_dia.py:164`) e cobertura (`app/chamados/services/tarefa_do_dia.py:225`).
- **Há leitor incoerente anterior ao delta**: o payload dos chamados do app só oferece “Anexar comprovante” quando `Ausencia.status == 'pendente'` (`app/api/views.py:1087`, `app/api/views.py:1091`), embora `pendente` não esteja nos choices atuais (`app/ponto/models.py:701`) nem em `ABERTAS` (`app/ponto/services/ausencia.py:765`). Uma ausência válida `aguardando_documento` não recebe esse bloco do payload; a própria intenção do bloco está descrita no comentário (`app/api/views.py:1071`). Isso não muda o dono semântico, mas impede afirmar alinhamento de todos os leitores.
- **Não achei outro status válido não final cuja bola seja do colaborador**: vocabulário tem documento, decisão, aprovada e rejeitada (`app/ponto/models.py:701`); `ABERTAS` contém somente documento e decisão (`app/ponto/services/ausencia.py:765`). Anexo também é permitido durante `aguardando_decisao`, mas possibilidade de anexar não equivale a obrigação pendente (`app/api/views.py:3117`).
- **Cuidado no “escritor único” absoluto**: além de nascimento e migrations, existe baixa de férias por `QuerySet.update(status='rejeitada')` (`app/ponto/services/ausencia.py:1283`); esse bypass não escreve `aguardando_documento`, nem demonstra um segundo escritor introduzido pela E2 (`app/ponto/services/ausencia.py:1283`).

## 07/11 04:12 · sessao 47a2809364aa · FIO-E0-V4-E2 · verificador · origem=Code

**Reprovo a E2 como concluída:** há atribuição de pergunta ao turno errado, desconhecimento devolvido como ausência de pendência e perda da distinção entre pendência de documento e decisão do DP. (`app/chamados/services/tarefa_do_dia.py:111`, `app/chamados/services/tarefa_do_dia.py:173`, `app/chamados/services/tarefa_do_dia.py:216`; `app/ponto/services/ausencia.py:779`)

A cópia não contém metadados Git acessíveis; identidade do HEAD e comparação com o pai: **[não verificado]**. Não executei a bateria Django nem consultei banco/logs; execução integral, RED anterior ao código, medida de frota e divergências de produção: **[não verificado]**. Executei somente corpos extraídos por AST, em memória, para conferir contraexemplos; nenhum arquivo foi criado, alterado ou apagado. **[não verificado por registro em arquivo]** [nao verificado]

**1. Procedência dos campos e compatibilidade de significado.**

O quadro registra escritores e leitores localizados e conferidos. A enumeração exaustiva de **todos** os leitores indiretos da árvore não ficou certificada: **[não verificado]**.

| Campo/valor | Quem escreve | Quem lê e significado encontrado |
|---|---|---|
| `CelulaDia.ata['lampadas']`: `acesa`, `tipo`, `hora` | `lampadas_do_dia` produz a lista; `ata_do_dia` a incorpora; a porta da célula persiste a ata. O teto pode transformar lâmpadas pendentes em `acesa=None`. (`app/ponto/services/cartorio.py:565`, `:704`, `:873`; `app/ponto/portas/celula.py:1027`) | E2 lê a ordem, tipo E/S, hora local e valor de `acesa`; lastro e leitor da célula também consomem a lista. **`acesa=True` não significa necessariamente batida ocorrida:** ausência/justificativa pode dispensar o marco, com `luz=None`. O docstring novo simplifica indevidamente isso para “aconteceu”; o comportamento de não cobrar o marco dispensado é compatível. (`app/chamados/services/tarefa_do_dia.py:10`, `:52`, `:89`, `:202`; `app/ponto/services/lastro.py:89`; `app/escala/services/leitor_celula.py:115`; `app/ponto/services/cartorio.py:752`, `:774`) |
| `ata['tipo_dia']` | A ata copia o tipo produzido pela grade; a porta da célula a grava. (`app/ponto/services/cartorio.py:704`; `app/ponto/portas/celula.py:1028`) | E2 chama `dia_sem_trabalho` e `ausencia_parcial_cobra_o_resto`; o leitor da célula transporta o tipo. Tipo preenchido diferente de `trabalho` cala, exceto ausência parcial; não é uma data nem um instante. (`app/chamados/services/tarefa_do_dia.py:178`; `app/chamados/juizes.py:411`, `:423`; `app/escala/services/leitor_celula.py:380`) |
| `CelulaDia.trabalha` | Geração, regeneração, edição humana e reversão passam pela porta da célula. (`app/ponto/portas/celula.py:259`, `:272`, `:603`, `:788`, `:851`, `:868`) | O juiz de dia sem trabalho distingue `False` de `None`: `False` é folga; `None` é indeterminado e cobra. E2 delega corretamente esse predicado. (`app/escala/models.py:1686`; `app/chamados/juizes.py:419`, `:429`; `app/chamados/services/tarefa_do_dia.py:178`) |
| `dna['marcos']` | `montar_dna` produz horários `HH:MM`; as operações da porta persistem o blob. (`app/ponto/management/commands/gerar_celulas.py:79`, `:83`, `:100`; `app/ponto/portas/celula.py:272`, `:604`, `:870`) | `EscalaColaborador.marcos_do_dia`, `marcos_turno`, cartório e E2 pelo teto. São horas de parede ancoradas no **dia do turno**; `marcos_turno` constrói instantes e avança a data quando necessário. Há incompatibilidade de cardinalidade: o DNA admite duas pausas, mas `marcos_turno` monta somente a primeira. (`app/escala/models.py:1652`, `:1124`; `app/escala/servico_jornada.py:69`, `:74`; `app/chamados/services/tarefa_do_dia.py:299`) |
| `PerguntaDisputa.data_evento` | Nascimento em `_nascer`; preenchimento no reapontamento; correção em `rechavear_data_evento`. A porta atual declara **dia do turno**; conversa grava o dia local em que foi aberta. (`app/chamados/models.py:2477`, `:2494`, `:2578`; `app/chamados/services/fio.py:838`, `:898`) | E2 usa em `dono_da_pergunta`; materialização e fio também o tratam como data do fato. **Não existe significado único efetivo:** o caminho de intervalo ainda entrega dia civil do marco, descrito abaixo. (`app/chamados/services/tarefa_do_dia.py:106`; `app/chamados/services/fio.py:45`; `app/chamados/services/materializacao.py:345`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`) |
| `.instancia_marco` e `.motivo` | O nascimento grava motivo e deriva a instância pelo catálogo quando vazia; a migração E1 também preenche instância. (`app/chamados/models.py:2578`, `:2972`; `app/chamados/migrations/0051_decisao_pergunta.py:27`) | E2 usa instância ou posição derivada do motivo. **Posição e ocorrência não coincidem:** o escritor usual produz `int_ida`, enquanto E2 cria `int_ida:2`; a associação posterior continua indexada pela posição. (`app/chamados/services/declaracao_texto.py:52`, `:55`; `app/chamados/services/tarefa_do_dia.py:62`, `:96`, `:192`, `:227`) |
| `.via_resolucao` | Porta do carimbo, descarimbo em lote, `encerrar_por_fato_no_espelho` e relavra de via. (`app/chamados/models.py:2662`, `:2801`, `:2811`; `app/chamados/services/relavra_via.py:123`) | E2 exclui vias fora do fato; o juiz de materialização interpreta vias com/sem testemunha. É vocabulário de resolução, não prova isolada de efeito físico. (`app/chamados/services/tarefa_do_dia.py:183`; `app/chamados/juizes.py:648`) |
| `.respondida_em` | Resposta normal, automática, recuperação pela trilha, reabertura/ressuscitação e carimbo automático; há restauração em memória. (`app/chamados/services/disputa_emissao.py:1346`, `:1462`, `:1516`, `:1537`; `app/chamados/models.py:1515`, `:2691`, `:2722`) | E1 verifica correspondência entre projeção e linha; `marcos_do_lote` agrega o instante; leitores antigos continuam usando a projeção. É instante de resposta, podendo ser retirado na projeção sem apagar a decisão histórica. (`app/chamados/models.py:2983`, `:3416`; `app/chamados/veredito_lavrado.py:166`; `app/chamados/services/disputa_emissao.py:1463`) |
| `.validada_em` | Validação, devolução, resposta nova que desfaz validação, reabertura/ressuscitação e carimbo automático. (`app/chamados/services/validacao.py:134`, `:199`; `app/chamados/services/disputa_emissao.py:1358`, `:1462`; `app/chamados/models.py:1515`, `:2694`) | E1 confere correspondência com o ato; leitores antigos filtram validação nula. E2 não toma esse carimbo como decisão vigente: usa a sequência. (`app/chamados/models.py:3420`, `:2895`; `app/chamados/services/tarefa_do_dia.py:137`, `:279`) |
| `.materializada_em` | `_escrever_carimbo` e descarimbo em lote, pelas portas do modelo. (`app/chamados/models.py:2660`, `:2801`) | **A premissa do pedido está errada:** a função de passivo E1 não lê esse campo; E2 o alcança por `marcos_do_lote`. Juiz e agrupamento legado de absorção também o leem. O carimbo registra processamento/resolução e pode existir sem batida. (`app/chamados/models.py:3409`, `:3424`; `app/chamados/services/tarefa_do_dia.py:282`; `app/chamados/veredito_lavrado.py:162`; `app/chamados/juizes.py:648`; `app/chamados/services/materializacao.py:624`) |
| `.chamado_destino_id` | Nascimento/dedup e reapontamento; há simulações temporárias em memória para julgar a costura. (`app/chamados/models.py:2550`, `:2578`, `:354`; `app/chamados/services/fio.py:777`, `:849`) | E2 chama `chamado_que_lista`, que usa destino ou chamado da disputa. É identidade da âncora de apresentação, sem unidade temporal. (`app/chamados/services/tarefa_do_dia.py:131`, `:290`; `app/chamados/services/fio.py:116`) |
| `DecisaoPergunta.ato/ato_em/ato_texto/absorcao_ausencia` | Porta física `registrar`; produtores: resposta, validação/devolução, reabertura/recusa, silêncio, retratação e absorção. (`app/chamados/models.py:3428`, `:1869`, `:1989`; `app/chamados/services/disputa_emissao.py:1369`, `:1478`, `:1519`, `:1540`; `app/chamados/services/validacao.py:139`, `:205`; `app/chamados/services/materializacao.py:560`) | E2 ordena por `(ato_em,pk)`, escolhe a última e lê o status atual da ausência vinculada. A tela lê respostas anteriores pela mesma sequência. `ato_em` é instante; `ato_texto` é o texto tocado pelo ato; absorção é ligação persistida, não agrupamento por coincidência de horário. (`app/chamados/services/tarefa_do_dia.py:137`, `:158`, `:275`; `app/chamados/models.py:2985`, `:3338`, `:3374`) |
| `Ausencia.status/data/data_fim/tipo` | Criação/edição/mudança de estado na porta de ausência; `Ausencia.save` também deriva `data_fim`. (`app/ponto/services/ausencia.py:341`, `:498`, `:824`; `app/ponto/models.py:907`) | Cobertura lê tipo, estado e intervalo **inclusivo**, com aberto somente para tipos próprios; E2 lê status para definir ação. **Há divergência de dono:** `aguardando_documento` pertence ao colaborador, mas E2 o transforma em `decidir_ausencia/admin`. (`app/ponto/turnos.py:2511`, `:2771`, `:2780`; `app/ponto/services/ausencia.py:779`; `app/chamados/services/tarefa_do_dia.py:158`, `:216`) |
| `Batida.pergunta_origem/retratada_em` | Registro/anotação pela porta; materialização vincula a pergunta; retratação carimba e desfazer limpa o instante. (`app/ponto/registro_batida.py:94`, `:235`, `:274`; `app/chamados/services/materializacao.py:937`; `app/ponto/services/retratar_humano.py:207`) | E2 lê origem dentro de `batidas_apuraveis`; juiz lê batidas plantadas; staleness agrega retratação. `retratada_em=NULL` inclui a batida na apuração; origem liga a pergunta, mas não prova sozinha o slot correto. (`app/chamados/services/tarefa_do_dia.py:287`; `app/ponto/turnos.py:2876`; `app/chamados/juizes.py:719`; `app/chamados/veredito_lavrado.py:140`) |
| `ChamadoColaborador.status_local/silenciado_ate` | Estado pela porta `_gravar_estado`; silêncio e levantamento por `arquivo`. (`app/chamados/models.py:199`, `:226`; `app/chamados/services/arquivo.py:166`, `:201`) | E2 delega à fila: estado de cobrança e exclusão quando silêncio é **estritamente posterior** a `agora`; igualdade já permite retorno. Silêncio é véu de apresentação, distinto do estado. (`app/chamados/services/tarefa_do_dia.py:292`; `app/chamados/services/painel.py:188`; `app/chamados/services/arquivo.py:154`) |

Os três campos da lavra estão discriminados no item 7. (`app/chamados/veredito_lavrado.py:194`)

**O erro concreto de `data_evento`:**

- O escritor atual `abrir` exige dia do turno, e `rechavear_data_evento` existe para normalizar o legado para esse significado. (`app/chamados/models.py:2477`; `app/chamados/services/fio.py:859`)
- Continua havendo alimentação civil: detector de intervalo grava `localtime(marco).date()`; emissão lê essa data e apenas a envolve em `dia_do_turno_da_celula`, que expressamente não converte. (`app/ponto/management/commands/detectar_intervalo_ausente.py:78`; `app/chamados/services/disputa_emissao.py:1037`; `app/ponto/turnos.py:2939`)
- Contraexemplo: turnos noturnos consecutivos de 21 e 22/10, ambos 22h–06h; pergunta de fim gravada corretamente com `data_evento=22/10`. Como o fim de 21 cai civilmente em 22 e o fim de 22 cai em 23, `dono_da_pergunta` devolve **21/10**. A heurística não consulta a célula/proveniência da pergunta; portanto **não cobre os dois significados sem atribuição errada**. (`app/chamados/services/tarefa_do_dia.py:106`, `:109`, `:114`)

Há ainda derivação paralela da identidade: E1 exclui `conversa=True` em `linhas_dos_fatos`; E2 recompõe o filtro sem essa exclusão e recupera posição pelo motivo. Uma conversa com motivo de marco pode ser tratada como pergunta factual. (`app/chamados/models.py:2465`, `:2487`; `app/chamados/services/tarefa_do_dia.py:94`, `:182`)

**2. Pureza, escrita e relógio.**

O **núcleo** `_tarefa_pura` recebe fatos carregados e não contém escrita de modelo. A função pública consulta banco; logo “pura” é defensável como decisão sem efeitos sobre fatos carregados, mas não como função matemática exclusivamente de `(colab,dia,agora)`. Banco e fuso ativo são entradas implícitas. (`app/chamados/services/tarefa_do_dia.py:167`, `:267`, `:318`, `:72`; `app/escala/servico_jornada.py:81`)

Não encontrei relógio corrente nem escrita persistente nos corpos alcançados de `vinculo_do_dia`, `marcos_turno`, `_pelo_efeito`, `batidas_apuraveis`, `chamado_que_lista`, `dia_sem_trabalho`, `ausencia_parcial_cobra_o_resto`, `texto_vazio` e `instancia_do_motivo`. (`app/escala/alimentacao.py:269`; `app/escala/servico_jornada.py:61`; `app/ponto/turnos.py:2511`, `:2856`; `app/chamados/services/fio.py:116`; `app/chamados/juizes.py:401`, `:414`, `:1251`; `app/chamados/services/declaracao_texto.py:55`)

`qs_na_fila` possui fallback de relógio, mas E2 fornece `agora`; `esta_stale` também possui fallback, igualmente evitado nesse caminho. (`app/chamados/services/painel.py:190`; `app/chamados/veredito_lavrado.py:181`; `app/chamados/services/tarefa_do_dia.py:284`, `:292`, `:322`)

Há memorização **por instância da carga**, em `_inst` e `_cob`; não localizei memorização global nesse caminho. Assim, a descrição literal “nenhum cache” omite essas memórias locais. (`app/chamados/services/tarefa_do_dia.py:8`, `:296`; `app/ponto/turnos.py:3068`)

**3. Equivalência da carga com cobertura e teto.**

**Cobertura:** dentro da mesma janela e sobre os mesmos fatos, os dois caminhos usam exatamente `_pelo_efeito`, `_q_cobertura` e `_cobertura_pura`. Preservam filtro, intervalo inclusivo e desempate pelo maior `(data,pk)`; a ordenação adicional por colaborador somente agrupa o lote. (`app/ponto/turnos.py:2753`, `:2761`, `:2780`, `:3072`)

**Teto:** compartilham o corpo, a ordem de `marcos_turno` e o limite inclusivo `marco + teto <= agora`. Entretanto, **a alimentação não é exatamente igual**: `marcos_vencidos` passa a célula recebida à escolha do vínculo, mas não a passa a `marcos_com_teto`; a carga passa seu snapshot de células. (`app/ponto/services/cartorio.py:841`, `:843`, `:858`; `app/ponto/turnos.py:3061`)

Contraexemplo construído: célula/carga em memória com DNA A; banco alterado para DNA B. A carga usa A; a casca pode escolher o vínculo com a célula A e buscar os marcos B no banco. Isso refuta equivalência incondicional da alimentação, sem demonstrar ocorrência em produção. (`app/escala/models.py:1119`, `:1122`, `:1124`; `app/ponto/services/cartorio.py:843`; `app/ponto/turnos.py:3062`)

O pad é `[ini−2,fim+1]`, mas a cobertura é expandida apenas em `[ini,fim]`. A API não impede consulta fora da janela: carga `[21,21]`, tarefa de 22 com célula no pad e ausência em 22 pode cobrar sem enxergar essa ausência. É uso fora do contrato documentado, aceito silenciosamente pela implementação. (`app/ponto/turnos.py:1847`, `:2977`, `:3073`; `app/chamados/services/tarefa_do_dia.py:302`, `:318`)

Queries constantes também não provam tempo linear no tamanho variável da janela: cada tarefa diária volta a percorrer todas as perguntas carregadas daquele colaborador. A meta de frota `≤1 query/colab-dia` não é assegurada pelo teste existente. (`app/chamados/services/tarefa_do_dia.py:306`; `app/chamados/tests/test_e2_tarefa_do_dia.py:397`)

**4. Checklist das classes de reprovação.**

| Classe | Resultado |
|---|---|
| Segundo juiz de autoridade registrada | Não achei duplicação do predicado de dia sem trabalho, cobertura, teto ou fila: E2 chama essas autoridades. O predicado de posição primeiro/último/meio é repetido; registro dele como juiz próprio: **[não verificado]**. (`app/chamados/services/tarefa_do_dia.py:49`, `:170`, `:292`, `:299`, `:307`; `app/chamados/services/declaracao_texto.py:118`) |
| Segundo escritor fora da porta | Não localizado no arquivo novo; o `out.update` é mutação do dict de saída. (`app/chamados/services/tarefa_do_dia.py:173`, `:176`) |
| Relógio fora da borda | Não localizado no caminho normal E2; existem fallbacks evitados pelo argumento obrigatório. (`app/chamados/services/tarefa_do_dia.py:322`; `app/chamados/services/painel.py:190`; `app/chamados/veredito_lavrado.py:181`) |
| Derivação paralela | Identidade factual é recomposta sem o filtro de conversa; dono do turno é inferido por heurística incompatível com dado normalizado. (`app/chamados/models.py:2466`; `app/chamados/services/tarefa_do_dia.py:99`, `:182`) |
| Campo com dois significados | `data_evento`: turno na porta atual; civil ainda vindo da emissão de intervalo. (`app/chamados/models.py:2477`; `app/chamados/services/disputa_emissao.py:1037`; `app/ponto/management/commands/detectar_intervalo_ausente.py:82`) |
| Desconhecimento tratado como resposta definitiva | Célula existente de trabalho com ata vazia/lâmpadas vazias retorna `instancias=[]`, `nao_sei=False`, razão vazia. O leitor anterior distingue ausência de lâmpadas como desconhecimento. (`app/chamados/services/tarefa_do_dia.py:84`, `:173`, `:193`; `app/chamados/services/declaracao_texto.py:82`, `:112`) |
| “Cada instância” incompleto | A segunda pausa recebe sufixo na saída, mas associação continua por posição; a geometria usada pelo teto também não inclui a segunda pausa do DNA. (`app/chamados/services/tarefa_do_dia.py:62`, `:192`, `:227`; `app/escala/models.py:1652`; `app/escala/servico_jornada.py:74`) |

**5. Bateria: o que responde pela regra e o que apenas acompanha a implementação.**

| Caso | Avaliação |
|---|---|
| 1 — dois marcos | Exercita independência entre marcos e ações distintas após resposta de um deles. (`app/chamados/tests/test_e2_tarefa_do_dia.py:129`) |
| 2 — noturno | Exercita teto por instante, mas aceita os dois significados de `data_evento` como equivalentes e sanciona a heurística. Não enfrenta dois noturnos consecutivos nem pergunta já normalizada. (`app/chamados/tests/test_e2_tarefa_do_dia.py:167`, `:174`, `:189`) |
| 3 — devolução | Exige retorno ao admin após ato de devolução. (`app/chamados/tests/test_e2_tarefa_do_dia.py:198`) |
| 4 — ausência | Exercita não rejeitada/aprovada/rejeitada, mas somente ausência de um dia e sem `aguardando_documento`. Não prova intervalos, desempate ou parcial. (`app/chamados/tests/test_e2_tarefa_do_dia.py:210`, `:215`, `:233`) |
| 5 — troca | A folga possui lâmpadas vazias; portanto não testa o juiz calando lâmpadas antigas apagadas. (`app/chamados/tests/test_e2_tarefa_do_dia.py:243`) |
| 6 — concorrência | Testa idempotência de atos duplicados inseridos sequencialmente; corrida real: **[não verificado]**. (`app/chamados/tests/test_e2_tarefa_do_dia.py:253`) |
| 7 — efeito | Distingue veredito NÃO, SIM com batida e lavra vencida por alteração de insumo; a lavra fresca é artificialmente futura. (`app/chamados/tests/test_e2_tarefa_do_dia.py:269`, `:108`) |
| 8 — passivo | Verifica projeção respondida sem linha, exigindo desconhecimento. (`app/chamados/tests/test_e2_tarefa_do_dia.py:325`) |
| 9 — desligamento | Verifica pendência histórica do admin até silêncio. (`app/chamados/tests/test_e2_tarefa_do_dia.py:336`) |
| 10 — exportada | Não cria competência exportada: avança quarenta dias e verifica ausência de SQL da folha. O título excede a evidência do caso. (`app/chamados/tests/test_e2_tarefa_do_dia.py:349`) |
| 11 — via sem testemunha | Protege contra rederivar materialização somente pela existência de batida. Não testa expiração civil da lavra. (`app/chamados/tests/test_e2_tarefa_do_dia.py:299`, `:108`) |

O erro de regra mais direto é o dono da ausência: o texto da bateria universaliza “DP decide” para ausência não rejeitada, enquanto o juiz de estado distingue documento devido pelo colaborador. E2 reproduz essa universalização nos dois ramos, cobertura e absorção. (`app/chamados/tests/test_e2_tarefa_do_dia.py:5`; `app/ponto/services/ausencia.py:779`; `app/chamados/services/tarefa_do_dia.py:158`, `:216`)

Mutantes que a inspeção indica deixarem as expectativas atuais intactas — execução desses mutantes: **[não verificado]**:

- `_fila = set()` sempre: fixtures não criam âncoras e o helper de comparação descarta `porta`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:95`, `:124`; `app/chamados/services/tarefa_do_dia.py:292`)
- Remover verificação de `janela_expira_em`: o helper sempre a grava como `None`. (`app/chamados/tests/test_e2_tarefa_do_dia.py:108`; `app/chamados/veredito_l

[truncado em 20000 de 27153 caracteres]

## 07/11 04:08 · sessao dd6f157c1bd2 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos obtidos só por leitura; executei apenas dois corpos extraídos por AST em `python3 -B`, sem Django, banco ou escrita. A comparação com o pai permanece [não verificado]. [nao verificado]

1. **`dono_da_pergunta` atribui perguntas normalizadas ao turno errado.** A porta atual recebe explicitamente dia do turno, e a porta de migração corrige o legado para esse significado. Mesmo assim, a heurística desloca para a véspera quando nela existe aquela posição no dia civil indicado. Para dois noturnos consecutivos, `p.data_evento=05/11`, fim de 04/11 em 05/11 e fim de 05/11 em 06/11, o corpo executado devolveu **04/11**, embora a pergunta atual seja de 05/11. (`app/chamados/models.py:2470`, `app/chamados/models.py:2483`, `app/chamados/services/fio.py:859`, `app/chamados/services/fio.py:898`, `app/chamados/services/tarefa_do_dia.py:106`, `app/chamados/services/tarefa_do_dia.py:111`.)

2. **Ainda há escritor atual de dia civil no campo.** O detector de intervalo escreve `localtime(marco).date()` em `marcos_faltantes[].data`; o emissor lê essa data e chama `_criar_pergunta_se_couber`, envolvendo-a em `dia_do_turno_da_celula`. Esse construtor declara expressamente que não converte nada. A gravação chega a `PerguntaDisputa.abrir(data_evento=...)`. (`app/ponto/management/commands/detectar_intervalo_ausente.py:78`, `app/ponto/management/commands/detectar_intervalo_ausente.py:82`, `app/chamados/services/disputa_emissao.py:1037`, `app/chamados/services/disputa_emissao.py:1043`, `app/ponto/turnos.py:2939`, `app/ponto/turnos.py:2943`, `app/chamados/services/disputa_emissao.py:814`, `app/chamados/services/disputa_emissao.py:821`.)

3. **O caso 2 sanciona a heurística, sem provar identificação correta.** Aceita D21 e D22 como a mesma pergunta de D21 e testa ambiguidade contra um turno diurno em D22. Falta o contraexemplo de dois noturnos consecutivos ou acervo já rechaveado. (`app/chamados/tests/test_e2_tarefa_do_dia.py:160`, `app/chamados/tests/test_e2_tarefa_do_dia.py:174`, `app/chamados/tests/test_e2_tarefa_do_dia.py:189`.)

4. **VALIDAÇÃO lê a lâmpada do juiz; não rederiva “materializou?”.** A carga aceita apenas `sim`/`nao` com `esta_stale(..., agora)==False`; o ramo usa esse valor, e a existência de batida apenas distingue `efeito_sem_lampada` dentro do SIM. Sem lâmpada confiável, retorna desconhecimento. (`app/chamados/services/tarefa_do_dia.py:146`, `app/chamados/services/tarefa_do_dia.py:152`, `app/chamados/services/tarefa_do_dia.py:281`.)

5. **Esse caminho não alcança `date.today()` do juiz.** `_lavra` não chama `_pergunta_materializada_de_fato`; `esta_stale` recebe o `agora` obrigatório. O fallback `timezone.now()` existe, mas não é usado nessa chamada normal. (`app/chamados/services/tarefa_do_dia.py:281`, `app/chamados/services/tarefa_do_dia.py:284`, `app/chamados/services/tarefa_do_dia.py:322`, `app/chamados/veredito_lavrado.py:181`.) O juiz direto usa `date.today()` em `_dentro_da_janela_viva`. (`app/chamados/juizes.py:563`.)

6. **`esta_stale` mistura calendários e depende da representação de fuso do mesmo instante.** Usa `agora.date()` e `julgado_em.date()` sem conversão local. Com julgado em 06/11 12Z, expiração 07/11 e agora 07/11 00:30Z, o corpo executado deu `True`; com o mesmo agora convertido para −03, deu `False`. O projeto configura São Paulo e `USE_TZ=True`; a expiração é uma `DateField` calculada a partir de `date.today()` do processo, não de `timezone.localdate()`. Fuso efetivo do processo de produção: [não verificado]. (`app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:182`, `app/chamados/veredito_lavrado.py:185`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`, `app/chamados/models.py:2399`.)

7. **A invalidação não cobre todos os insumos do juiz.** O marco agrega criação/retratação de batida, cinco timestamps da célula, materialização e resposta. Não agrega decisões, via, logs de retratação, estado/destino. O juiz lê via e materialização, log de retratação e estado do destino. Para destino, existe relavra por evento na porta; isso não transforma o predicado de staleness em cobertura completa de insumos. (`app/chamados/veredito_lavrado.py:137`, `app/chamados/veredito_lavrado.py:151`, `app/chamados/veredito_lavrado.py:162`, `app/chamados/juizes.py:283`, `app/chamados/juizes.py:648`, `app/chamados/juizes.py:744`, `app/chamados/services/fio.py:849`.) Uma decisão posterior a `julgado_em` não invalida diretamente a lavra; o juiz antigo tampouco lê `DecisaoPergunta`, e a tarefa não compara `ult.ato_em` à data da lavra. (`app/chamados/veredito_lavrado.py:179`, `app/chamados/services/tarefa_do_dia.py:137`, `app/chamados/services/tarefa_do_dia.py:146`, `app/chamados/juizes.py:633`.)

8. **A premissa de leitura de `materializada_em` somente pelo passivo não corresponde ao código.** A autoridade E1 `linhas_da_projecao_sem_linha` lê resposta e validação, sem consultar materialização; a carga da tarefa consulta indiretamente materialização por `marcos_do_lote`. (`app/chamados/models.py:3409`, `app/chamados/models.py:3416`, `app/chamados/models.py:3420`, `app/chamados/services/tarefa_do_dia.py:279`, `app/chamados/services/tarefa_do_dia.py:282`, `app/chamados/veredito_lavrado.py:162`.)

9. **Teste da validade da lâmpada não prova suas fronteiras temporais.** O helper escreve `julgado_em` dez anos no futuro e `janela_expira_em=None`. O caso de staleness usa retratação posterior à lavra; nenhum caso do arquivo exercita expiração. Portanto, o mutante que remove a verificação de `janela_expira_em` de `esta_stale` passaria esses casos E2. (`app/chamados/tests/test_e2_tarefa_do_dia.py:108`, `app/chamados/tests/test_e2_tarefa_do_dia.py:112`, `app/chamados/tests/test_e2_tarefa_do_dia.py:284`, `app/chamados/veredito_lavrado.py:182`.)

Censo localizado dos três campos da lavra:

- **Atribuição de valores:** `lavrar`, restauração em memória e backfill; persistência normal por `save`, e lote pela porta `gravar_lavras_em_lote`. (`app/chamados/veredito_lavrado.py:227`, `app/chamados/veredito_lavrado.py:243`, `app/chamados/management/commands/backfill_veredito_lavrado.py:138`, `app/chamados/veredito_lavrado.py:207`.)
- **Persistidores da lavra já calculada:** modelo no reapontamento e carimbo, fio no reapontamento/rechaveamento, e relavra de via. (`app/chamados/models.py:2571`, `app/chamados/models.py:2708`, `app/chamados/services/fio.py:850`, `app/chamados/services/fio.py:903`, `app/chamados/services/relavra_via.py:124`.)
- **Leitores dos três campos:** `esta_stale`, foto da lavra, carga nova, varredura do raio X e backfill. (`app/chamados/veredito_lavrado.py:179`, `app/chamados/veredito_lavrado.py:237`, `app/chamados/services/tarefa_do_dia.py:283`, `app/chamados/services/raiox.py:31`, `app/chamados/management/commands/backfill_veredito_lavrado.py:120`.)
- **Leitores adicionais do veredito:** crédito/ausência de ação do raio X, auditoria de invariantes e comparação/trilha da relavra de via. (`app/chamados/services/raiox.py:133`, `app/chamados/services/raiox.py:182`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1170`, `app/chamados/services/relavra_via.py:121`.)

## 07/11 04:08 · sessao c6c0a6f64a84 · - · - · origem=Ronald (tmux)

Achados para itens 4–6, somente por leitura, sem executar a bateria Django:

- **O selo de pureza não cobre ramo com pergunta.** O teste de zero escritas cria apenas célula e chama dois instantes; nenhuma pergunta/decisão existe nesse teste (`app/chamados/tests/test_e2_tarefa_do_dia.py:363–374`). Uma escrita condicionada à entrada em `_pela_decisao` não seria observada por esse selo (`app/chamados/services/tarefa_do_dia.py:121`). Mutante passar na bateria inteira: [não verificado].
- **A fila e suas portas ficam sem mordida na bateria.** `pergunta()` cria disputa com `contexto={}`, sem chamado (`app/chamados/tests/test_e2_tarefa_do_dia.py:95–99`); `acoes()` descarta `porta` antes das comparações (`:124–126`). Por inspeção, substituir `_fila` por `set()` deixaria as expectativas da bateria inalteradas; execução do mutante [não verificado]. O código cuja integração fica desprotegida é `app/chamados/services/tarefa_do_dia.py:290–293`.
- **“Concorrência” não prova corrida.** O caso 6 registra validações e silêncios sequencialmente e compara saídas; não abre transações concorrentes (`app/chamados/tests/test_e2_tarefa_do_dia.py:253–265`).
- **“Competência exportada” não monta uma competência exportada.** O caso apenas avança `agora` quarenta dias, verifica ação e ausência de SQL da folha (`app/chamados/tests/test_e2_tarefa_do_dia.py:349–357`). A independência diante de um registro real de exportação fica [não verificado].
- **A ordem das decisões é exercitada somente com relógio e PK concordantes.** O helper aumenta `ato_em` junto da quantidade de linhas existentes (`app/chamados/tests/test_e2_tarefa_do_dia.py:103–106`). Não há caso retrodatado nem empate; portanto, trocar a carga de `order_by('pergunta_id','ato_em','pk')` por PK provavelmente passaria, por inspeção; mutante executado [não verificado]. A ordem do modelo é `ato_em,pk` (`app/chamados/models.py:3374`) e a carga a respeita (`app/chamados/services/tarefa_do_dia.py:275–277`).
- **A lavra “fresca” dos testes é artificialmente futura.** O helper fixa `julgado_em` em D+3650 e apaga `janela_expira_em` (`app/chamados/tests/test_e2_tarefa_do_dia.py:108–113`), deixando sem teste a expiração civil e enfraquecendo a integração real de staleness.
- **O custo medido no teste é constância, não a meta ≤1.** A única asserção compara 2 colaboradores × 3 dias com 1 × 1; não impõe teto por colab-dia, nem mede frota (`app/chamados/tests/test_e2_tarefa_do_dia.py:397–415`). Resultado 6 do aval: [não verificado].
- **Falta caso de célula existente sem lâmpadas.** Nesse cenário `instancias_do_turno` devolve `[]`, e `_tarefa_pura` termina com `nao_sei=False` (`app/chamados/services/tarefa_do_dia.py:79–90`, `:173–177`, `:193–248`). O leitor antigo distingue ata sem lâmpadas como `ESTADO_SEM_CELULA` (`app/chamados/services/declaracao_texto.py:82–83`, `:110–116`), e o registro do novo juiz promete “Não sabe -> nao_sei” (`app/docs/JUIZES.md:77`).

A bateria tem mordida para soberania das lâmpadas por instância (caso 1: `app/chamados/tests/test_e2_tarefa_do_dia.py:129–147`), teto por instante/noturno (`:167–172`) e fronteira 14:00/14:01 (`:363–370`), cobertura não rejeitada (`:215–240`), devolução ao admin (`:198–206`) e passivo sem inventar decisão (`:325–333`). Não encontrei caso de ausência parcial, `trabalha=None`, ata de folga com `trabalha=True`, duas pausas, empate/retrodatação de decisões, nem status/silêncio reais de chamado no arquivo completo (`app/chamados/tests/test_e2_tarefa_do_dia.py:1–415`). Aprovação 10/10, RED e cronologia “teste antes do código”: [não verificado].

Contratos:

| Contrato | Alcança o arquivo novo? | O que efetivamente garante |
|---|---|---|
| leitores_vitalidade | Sim: varre os `.py` de produção de toda a árvore (`app/chamados/tests/test_contract_leitores_vitalidade.py:282–307`). | Só compara `materializada_em/validada_em` com None ou `__isnull` (`:14`, `:298–305`). O novo arquivo não faz isso; lê passivo pela E1 (`app/chamados/services/tarefa_do_dia.py:279`). Não prova pureza, relógio ou validade de `_lavra`. |
| censo_escritas | Sim (`app/core/censo_escritas.py:469–491`). | Rodei apenas o analisador estático com `python3 -B`: ele devolve `(176,None,'update','out.update()')`, falso positivo do dict de retorno (`app/chamados/services/tarefa_do_dia.py:173–176`). O varredor documenta que `update` só kwargs desconhecido vai ao balde (`app/core/censo_escritas.py:49–50`); entidade None é ignorada na agregação de famílias (`:505–509`). Nenhuma escrita de modelo detectada nesse arquivo por esse analisador. |
| escritores_estado | Sim: app `chamados`, descida por diretórios (`app/core/tests/test_contract_escritores_estado.py:12–13`, `:184–197`). | Regex de atribuição/`update` para campos enumerados (`:177–181`, `:201–208`); não há atribuição desses campos no novo módulo. Execução do contrato: [não verificado]. |
| B5j universo de apuração | Não seleciona esse módulo: `_producao` exige texto `parear_turnos` ou `_turnos_do_colab_puro` (`app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `:135–148`). | O novo módulo usa a porta correta `batidas_apuraveis` (`app/chamados/services/tarefa_do_dia.py:287–289`), mas esse tripwire não protege seu universo. |
| bocais_apuracao | Não: lista fechada de seis arquivos, sem tarefa (`app/ponto/tests/test_tripwire_bocais_apuracao.py:13–24`, `:27–33`). | Nenhuma garantia sobre o arquivo novo. |
| registro de juízes | Sim: `_C19` declarado (`app/core/juizes.py:150–155`, `:285`), lookup atualizado (`app/docs/JUIZES.md:73–78`), corte escrito (`app/docs/CORTES.md:424`). | O teste de existência percorre toda família (`app/chamados/tests/test_contract_juiz_chamado.py:24–27`), mas a procura de juiz duplicado só casa regex de SLA e data crua (`app/core/juizes.py:1397–1404`; algoritmo em `app/ponto/tests/test_contract_juiz_celula.py:48–60`). Não detectaria duplicação semântica de `_C19`. |

Não achei duplicação dos juízes registrados de dia sem trabalho, cobertura, teto ou fila no corpo principal: ele chama `dia_sem_trabalho`/`ausencia_parcial_cobra_o_resto` (`app/chamados/services/tarefa_do_dia.py:170–179`), usa carga de cobertura/teto (`:299`, `:307`) e fila (`:292`). `_posicoes` repete a regra de primeiro/último/meio de `_marcos_faltantes` (`app/chamados/services/tarefa_do_dia.py:49–62`; `app/chamados/services/declaracao_texto.py:118–128`), mas não encontrei essa classificação registrada como juiz próprio; violação de autoridade registrada por essa repetição: [não verificado].

## 07/11 04:07 · sessao 867869b107c9 · - · - · origem=Ronald (tmux)

Itens 2/3/custo, somente leitura; nenhum teste executado. [não verificado] [nao verificado]

**Pureza e relógio**

- Não encontrei escrita no banco nem leitura do relógio corrente no caminho executado por `tarefa_do_dia`: o wrapper exige `agora`, a carga o repassa a `qs_na_fila` e `esta_stale`, e o corpo recebe os fatos carregados. `app/chamados/services/tarefa_do_dia.py:259`, `app/chamados/services/tarefa_do_dia.py:281`, `app/chamados/services/tarefa_do_dia.py:292`, `app/chamados/services/tarefa_do_dia.py:318`.
- `qs_na_fila` só executa `timezone.now()` quando `agora is None`; a tarefa fornece valor obrigatório. `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:190`, `app/chamados/services/tarefa_do_dia.py:292`, `app/chamados/services/tarefa_do_dia.py:322`.
- `esta_stale` possui fallback `timezone.now()`, mas recebe o `agora` obrigatório da carga; não chama `lavrar`, `expira_em` nem `_pergunta_materializada_de_fato`. Portanto o `date.today()` de `expira_em` e do juiz não é alcançado por este caminho. `app/chamados/veredito_lavrado.py:112`, `app/chamados/veredito_lavrado.py:173`, `app/chamados/veredito_lavrado.py:181`, `app/chamados/services/tarefa_do_dia.py:281`.
- `vinculo_do_dia` escolhe entre fatos carregados; `marcos_turno` encadeia datas recebidas; `_pelo_efeito` e `batidas_apuraveis` constroem queryset; `chamado_que_lista`, `dia_sem_trabalho`, `ausencia_parcial_cobra_o_resto`, `texto_vazio` e `instancia_do_motivo` não leem relógio nesses corpos. `app/escala/alimentacao.py:269`, `app/escala/servico_jornada.py:68`, `app/ponto/turnos.py:2516`, `app/ponto/turnos.py:2875`, `app/chamados/services/fio.py:116`, `app/chamados/juizes.py:411`, `app/chamados/juizes.py:423`, `app/chamados/juizes.py:1253`, `app/chamados/services/declaracao_texto.py:57`.
- Há memorização **na instância da carga**, em `_inst` e `_cob`; não encontrei mutação global no caminho. A descrição literal “nenhum cache” precisa distinguir essas memórias por carga. `app/chamados/services/tarefa_do_dia.py:8`, `app/chamados/services/tarefa_do_dia.py:296`, `app/ponto/turnos.py:3068`.
- A função pública consulta banco; somente `_tarefa_pura` recebe todos os fatos. A pureza matemática de apenas `(colab,dia,agora)` não existe: há banco e fuso ativo implícitos. `localtime` e `make_aware` não recebem fuso explícito. `app/chamados/services/tarefa_do_dia.py:72`, `app/chamados/services/tarefa_do_dia.py:90`, `app/chamados/services/tarefa_do_dia.py:267`, `app/escala/servico_jornada.py:81`.

**Equivalência**

- Cobertura: dentro da janela declarada, ambas usam exatamente `_pelo_efeito`, `_q_cobertura` e `_cobertura_pura`. A ordem extra `colaborador_id` só agrupa; dentro de cada pessoa preserva `(data,pk)`, e o último sobrescreve. O intervalo expandido é inclusivo. `app/ponto/turnos.py:2753`, `app/ponto/turnos.py:2761`, `app/ponto/turnos.py:2771`, `app/ponto/turnos.py:2780`, `app/ponto/turnos.py:3072`.
- Teto: ambas usam `cartorio.marcos_com_teto`, cujo limite é inclusivo `marco + teto <= agora`, na ordem de `marcos_turno`. `app/ponto/services/cartorio.py:843`, `app/ponto/services/cartorio.py:858`, `app/ponto/turnos.py:3061`, `app/escala/servico_jornada.py:74`.
- **Não são exatamente a mesma alimentação:** `marcos_vencidos` usa a `cel` recebida para escolher vínculo, mas não a passa ao corpo; `marcos_do_dia` então busca DNA no banco. A carga passa suas células já carregadas. Contraexemplo: carga/célula em memória com DNA A, banco depois alterado para DNA B; a carga calcula A e a casca calcula B, mesmo colab-dia/agora. É um contraexemplo de código, não ocorrência de produção. `app/ponto/services/cartorio.py:841`, `app/ponto/services/cartorio.py:843`, `app/ponto/turnos.py:3062`, `app/escala/models.py:1119`, `app/escala/models.py:1122`.
- O pad é `[ini−2,fim+1]`, mas cobertura só cobre `[ini,fim]`; não existe guarda contra consulta fora da janela. Exemplo: carga `[21,21]`, consulta dia22 com célula no pad e ausência no dia22: a tarefa não vê cobertura, ao contrário da casca aberta para22. Isso é uso fora do contrato documentado, permitido silenciosamente pela API atual. `app/ponto/turnos.py:1847`, `app/ponto/turnos.py:2977`, `app/ponto/turnos.py:3073`, `app/chamados/services/tarefa_do_dia.py:302`, `app/chamados/services/tarefa_do_dia.py:318`.

**Custo e testes** [nao verificado]

- A carga faz um número de queries independente do número de colaboradores nos corpos examinados; sua construção inclui carga de vínculos/ata não utilizados diretamente pela tarefa. Não há prova fornecida de `<=1 query/colab-dia` na frota. O teste só exige igualdade entre dois colaboradores×três dias e um colaborador×um dia, sem limite absoluto. `app/ponto/turnos.py:2986`, `app/ponto/turnos.py:2989`, `app/ponto/turnos.py:2992`, `app/chamados/tests/test_e2_tarefa_do_dia.py:397`, `app/chamados/tests/test_e2_tarefa_do_dia.py:415`.
- A tarefa percorre todas as perguntas daquele colaborador em cada dia consultado, após filtrar em memória; queries constantes não demonstram complexidade linear no tamanho variável da janela. `app/chamados/services/tarefa_do_dia.py:306`.
- Mutante que remove o tratamento `dia_sem_trabalho` pode passar a bateria: os casos de folga têm lâmpadas vazias, logo nenhuma instância cuja cobrança precise ser suprimida. Falta `trabalha=False` com lâmpadas antigas apagadas, e também ausência parcial. `app/chamados/tests/test_e2_tarefa_do_dia.py:165`, `app/chamados/tests/test_e2_tarefa_do_dia.py:245`, `app/chamados/services/tarefa_do_dia.py:199`.
- Mutantes de cobertura por intervalo aberto/fechado e desempate `(data,pk)` também não são atingidos pela ausência do caso4, que cria apenas ausência de um dia. `app/chamados/tests/test_e2_tarefa_do_dia.py:210`, `app/chamados/tests/test_e2_tarefa_do_dia.py:233`.

## 07/11 02:23 · sessao 939c1ffb8eb7 · V2-BUGS-I2 · verificador · origem=Code

**REPROVADO na pré-verificação.** Há acréscimo novo de HE100 noturna em dia lavrado não feriado, promoção do partido que continua lendo zero e casos em que o split escrito viola a regra do subconjunto. (`app/ponto/services/dia_pago.py:252`; `app/ponto/services/fechamento.py:964`; `app/ponto/motor_calculo_v2.py:1015`)

1. **O diff pode mudar valor fora de feriado em dobra.**

   Contraexemplo: COM; **06/10 é folga, não feriado**; **07/10 é feriado cadastrado**; vínculo não trabalha no feriado; escala certa; batidas **06/10 13–23 / 07/10 03–05**. A cauda herda 06/10 pelo intervalo de quatro horas e é classificada como FT dessa jornada. (`app/ponto/motor_calculo_v2.py:2403`; `app/ponto/motor_calculo_v2.py:2407`; `app/ponto/motor_calculo_v2.py:2448`; `app/ponto/motor_calculo_v2.py:2468`)

   Entretanto, o comercial calcula a dobra da cauda pelo **dia civil 07/10**, escrevendo 120 minutos de HE100 noturna; o fechamento também seleciona a promoção pela data civil. A linha nova da lavra acrescenta esses **2,00 h ao DiaPago de 06/10**, que não é feriado. Antes, esse acréscimo era zero. O erro de seleção já existia; **o novo valor fora do escopo nasce neste diff**. (`app/ponto/motor_calculo_v2.py:3264`; `app/ponto/motor_calculo_v2.py:3268`; `app/ponto/motor_calculo_v2.py:3294`; `app/ponto/services/fechamento.py:947`; `app/ponto/services/fechamento.py:962`; `app/ponto/services/dia_pago.py:245`; `app/ponto/services/dia_pago.py:252`)

   Esse percurso foi exercido com métodos reais extraídos por AST, em memória, com insumos controlados; execução integral pelo ORM e ocorrência em produção: **[não verificado]**.

2. **Os totais anteriores são conservados; a HE100 noturna não está correta em todos os caminhos.**

   Nos ramos diários modificados, substituir `ultimo = total` por parcelas de cada período conserva HE100 e HE100 feriado: o fechamento soma todos os períodos. A promoção mantém os totais anteriores e acrescenta somente o subconjunto noturno. O 12x36 mantém sua dobra seguida de split. (`app/ponto/motor_calculo_v2.py:3100`; `app/ponto/motor_calculo_v2.py:3357`; `app/ponto/motor_calculo_v2.py:329`; `app/ponto/motor_calculo_v2.py:332`; `app/ponto/services/fechamento.py:955`; `app/ponto/services/dia_pago.py:249`; `app/ponto/motor_calculo_v2.py:2855`)

   Permanecem três falhas:

   - **FT do partido continua zerado.** NPART, feriado em que o vínculo folga, batidas **21–00**: 180 trabalhados, 120 noturnos; promoção de **3 h HE100 feriado**, mas **0 h HE100 noturna**, quando deveria carregar **2 h**. `calcular_periodo` não escreve HE; o laço novo percorre apenas `resultado.periodos`; FT recebe somente o mapa do dia. Mensal e DiaPago leem o mesmo zero. (`app/ponto/motor_calculo_v2.py:2893`; `app/ponto/motor_calculo_v2.py:2897`; `app/ponto/motor_calculo_v2.py:2951`; `app/ponto/motor_calculo_v2.py:3083`; `app/ponto/services/fechamento.py:964`; `app/ponto/services/dia_pago.py:252`)
   - **Janela efetiva e split discordam.** COM no feriado, **08–12/13–23**, janela recortando a saída às **22**, limite deslocado por extra declarada de 300 minutos: trabalhado efetivo **780**, noturno **zero**; a dobra chama o split no par bruto até 23 e escreve **60 minutos HE100 noturna**. A parte fica maior que o noturno do dia. (`app/ponto/motor_calculo_v2.py:3224`; `app/ponto/motor_calculo_v2.py:3238`; `app/ponto/motor_calculo_v2.py:3260`; `app/ponto/motor_calculo_v2.py:3357`; `app/ponto/motor_calculo_v2.py:3361`; `app/ponto/motor_calculo_v2.py:1015`)
   - **Recorte longo continua duplicando minutos.** Com prorrogação e todo o par em dobra, **04–23**, HE100=1140, produz split **1200**; **04–01(+1)**, HE100=1260, produz **1440**. O segmento prorrogado alcança `fim` e os segmentos seguintes são somados novamente. As novas chamadas propagam isso para HE100 noturna. O noturno principal também contém a duplicação; comparar somente `noturnas/fator` não detecta esse defeito. (`app/ponto/motor_calculo_v2.py:111`; `app/ponto/motor_calculo_v2.py:112`; `app/ponto/motor_calculo_v2.py:116`; `app/ponto/motor_calculo_v2.py:441`; `app/ponto/motor_calculo_v2.py:448`; `app/ponto/motor_calculo_v2.py:3104`; `app/ponto/motor_calculo_v2.py:3361`)

3. **Escritores e leitores: há um consumidor incompatível com a redistribuição.**

   | Campos | Quem escreve | Quem lê |
   |---|---|---|
   | `minutos_extra`, `_100`, `_100_feriado` | Dobras por período do 12x36/comercial e laços diários do partido/comercial. (`app/ponto/motor_calculo_v2.py:2810`; `app/ponto/motor_calculo_v2.py:3269`; `app/ponto/motor_calculo_v2.py:3100`; `app/ponto/motor_calculo_v2.py:3357`) | Somatórios, lavra, alimentação, espelho e contadores. (`app/ponto/motor_calculo_v2.py:329`; `app/ponto/services/dia_pago.py:182`; `app/ponto/calculador/alimentacao.py:102`; `app/ponto/services/espelho.py:112`; `app/folha/porta_export.py:542`) |
   | `_50_noturna`, `_100_noturna` | Casca canônica, além dos resets diários. (`app/ponto/motor_calculo_v2.py:1020`; `app/ponto/motor_calculo_v2.py:1022`; `app/ponto/motor_calculo_v2.py:2937`; `app/ponto/motor_calculo_v2.py:3336`) | Somatórios e lavra; o calculador chama a mesma função por ponta e soma. (`app/ponto/motor_calculo_v2.py:340`; `app/ponto/motor_calculo_v2.py:344`; `app/ponto/services/dia_pago.py:181`; `app/ponto/services/dia_pago.py:252`; `app/ponto/calculador/regras.py:827`) |
   | `horas_extras_100_noturna` mensal/DiaPago | Fechamento e porta `lavrar`. (`app/ponto/services/fechamento.py:924`; `app/ponto/services/fechamento.py:962`; `app/ponto/services/dia_pago.py:184`; `app/ponto/services/dia_pago.py:252`) | Comparação mensal versus soma dos dias. (`app/folha/porta_export.py:388`) |

   **A6 quebra o contrato:** anuncia “HE idêntica em 3+ dias”, mas conta períodos. No trecho real do leitor, `[0,0,540]` em um feriado não gera A6; após redistribuir para `[180,180,180]`, gera **A6=[d,d,d]**, exibindo três ocorrências de um único dia. O contador foi exercido em memória; a fixture completa pelo pareador: **[não verificado]**. (`app/ponto/services/esmeril_espelho.py:20`; `app/ponto/services/esmeril_espelho.py:174`; `app/ponto/services/esmeril_espelho.py:199`)

   Os somatórios e `regras.py` aceitam vários períodos; **não exigem a dobra no último**. Entretanto, o calculador recebe pontas efetivas, enquanto a casca do motor recebe pontas brutas: mesma função, insumos diferentes. (`app/ponto/motor_calculo_v2.py:329`; `app/ponto/calculador/alimentacao.py:83`; `app/ponto/calculador/alimentacao.py:106`; `app/ponto/calculador/regras.py:832`; `app/ponto/motor_calculo_v2.py:1015`)

   Tela, calendário e PDF usam totais; TXT trata HE noturna como subconjunto e não emite uma segunda linha dessas horas. Não há soma adicional do subconjunto nesses trechos. (`app/templates/ponto/espelho.html:279`; `app/colaboradores/services/calendario.py:651`; `app/relatorios/pdf_espelho.py:844`; `app/folha/export.py:424`)

   Mensal e DiaPago recebem a mesma partição e somam o mesmo campo, portanto **podem concordar no número errado**. A concordância não resolve o zero do partido nem o dia incorreto do item 1. (`app/ponto/services/fechamento.py:980`; `app/ponto/services/fechamento.py:1203`; `app/ponto/services/fechamento.py:964`; `app/ponto/services/dia_pago.py:252`)

4. **A retirada deixou uma referência órfã; o censo foi restaurado coerentemente.**

   O helper F10h ainda diz **“batidas do F10f”**, embora F10f tenha sido retirado. O comentário deve deixar de apontar para um caso inexistente. (`app/ponto/tests/test_o280_bateria_da_lei.py:1580`)

   O contrato voltou a cobrar **10** ajudantes privados, compatível com a contagem AST da cópia. MATRIZ/CASOS dos casos sobreviventes têm testes assertivos correspondentes; não encontrei caso declarado sem exercício nesse conjunto. (`app/ponto/tests/test_contract_juiz_batida.py:326`; `app/ponto/tests/test_o280_bateria_da_lei.py:210`; `app/ponto/tests/test_o280_bateria_da_lei.py:212`; `app/ponto/tests/test_o280_bateria_da_lei.py:1272`)

   “**Era 240**” em F11d descreve uma cura intermediária, não 19aeede1: a base fornecida dava `continue` sem split, portanto era **zero**. A versão da medida precisa ficar explícita. (`app/ponto/tests/test_o280_bateria_da_lei.py:1776`; `app/ponto/motor_calculo_v2.py:3114`; anexo `diff_total_v6.patch`, hunk `@@ -3090,14 +3090,27 @@`)

5. **Os testes principais mordem por valor; o pino não isola a causa da falha.** [nao verificado]

   F11, F11b, F11c e F11d cobram respectivamente **120 min, 60 min, 1 h mensal/DiaPago e 360 min**. Pela base do diff, os quatro teriam zero no subconjunto; F11r é referência preexistente de **300 min**, não RED da cura. Execução dos testes contra 19aeede1: **[não verificado]**. (`app/ponto/tests/test_o280_bateria_da_lei.py:1670`; `app/ponto/tests/test_o280_bateria_da_lei.py:1708`; `app/ponto/tests/test_o280_bateria_da_lei.py:1748`; `app/ponto/tests/test_o280_bateria_da_lei.py:1779`; `app/ponto/tests/test_o280_bateria_da_lei.py:1692`)

   O gêmeo F10h fixa **570 HE100 feriado / 390 noturna**, incluindo cauda zero. O pino compara valores, mas `expectedFailure` aceita falha em outro campo ou erro; também exige **660 HE100 feriado**, mudança de pertença fora desta cura. Assim, ele pode continuar esperado-vermelho mesmo após acertar o subconjunto noturno. O gêmeo protege a fixture compartilhada; o conjunto não é vazio. (`app/ponto/tests/test_o280_bateria_da_lei.py:1602`; `app/ponto/tests/test_o280_bateria_da_lei.py:1606`; `app/ponto/tests/test_o280_bateria_da_lei.py:1611`; `app/ponto/tests/test_o280_bateria_da_lei.py:1624`)

   F10i cobra **90/102,86**, comportamento preservado; é guarda, não RED do item 2. F11c cobre promoção comercial com escala certa forçada, deixando descoberta a promoção do partido. (`app/ponto/tests/test_o280_bateria_da_lei.py:1643`; `app/ponto/tests/test_o280_bateria_da_lei.py:1717`; `app/ponto/tests/test_o280_bateria_da_lei.py:1744`)

6. **Classes de reprovação presentes e limites do veredito.**

   Há **derivação concorrente do dia** exposta pelo novo valor: classificação FT pela jornada, promoção pelo civil e lavra pelo mapa. Há também **ESMERIL-SEMÂNTICA**: `minutos_extra` passa a carregar parcelas por período, mas A6 continua interpretando cada parcela como ocorrência diária. (`app/ponto/motor_calculo_v2.py:2407`; `app/ponto/services/fechamento.py:947`; `app/ponto/services/dia_pago.py:245`; `app/ponto/services/esmeril_espelho.py:177`)

   Nos acréscimos examinados não nasce porta de escrita, juiz ou leitura de relógio: são chamadas à casca existente e somas nas portas existentes. Isso não elimina os erros de insumo e significado acima. (`app/ponto/motor_calculo_v2.py:3104`; `app/ponto/motor_calculo_v2.py:3361`; `app/ponto/services/fechamento.py:962`; `app/ponto/services/dia_pago.py:252`)

   **DIFF `par_v6` por dono, col922, reversão anterior ao veredito, ordem operacional de travas, pouso e suíte completa: [não verificado].** Esses resultados não foram fornecidos no dado raspado; não atribuo à volta 6 as medições históricas da volta reprovada. **[não verificado]** [nao verificado]

## 07/11 02:22 · sessao 8827481b08b7 · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Há novo valor em dia lavrado que não é feriado em dobra.** Caso: COM, d0=06/10 folga e não feriado; batidas d0 13–23, d1 03–05; d1=07/10 feriado, vínculo folga no feriado e escala certa. O intervalo de 4h produz cauda que herda d0 pela regra ≤6h; ela é FT de d0 (`app/ponto/motor_calculo_v2.py:2403`, `:2407`, `:2448`, `:2468`). Entretanto `MotorComercial.calcular_periodo` julga a cauda pelo dia civil d1 e escreve HE100 noturna=120 (`:3264`, `:3268`, `:3294`). O fechamento seleciona `ft_feriado` pelo mesmo civil d1 (`app/ponto/services/fechamento.py:935`, `:947`, `:949`), e as linhas novas passam a acrescentar **2h** no mensal e no DiaPago de d0 (`:962`; `app/ponto/services/dia_pago.py:245`, `:252`). Baseline: a seleção errada já existia, mas o acréscimo de HE100 noturna era zero; volta6: 2h. Medido em memória com AST do `calcular_periodo`, helpers e prefixo da lavra; não foi execução integral ORM. (`app/ponto/motor_calculo_v2.py:3268`; `app/ponto/services/dia_pago.py:252`.)

2. **Promoção FT do partido continua sem HE100 noturna.** `MotorTurnoPartido.calcular_periodo` escreve trabalho e noturno, devolvendo sem split (`app/ponto/motor_calculo_v2.py:2893`, `:2896`, `:2897`). O cálculo diário percorre apenas `resultado.periodos`; FT recebe somente mapa de dia (`:2928`, `:2944`, `:2951`, `:2955`, `:3083`). Caso NPART 21–00 em feriado no qual o vínculo folga: 180 trabalhados, 120 noturnos; a promoção paga 3h HE100 feriado e lê **0h** HE100 noturna, onde a regra exige **2h** (`app/ponto/services/fechamento.py:954`, `:964`; `app/ponto/services/dia_pago.py:250`, `:252`). A retirada do escritor do partido explica a lacuna; o leitor novo não a resolve (`app/ponto/motor_calculo_v2.py:2897`, `:2955`).

3. **O split novo usa par bruto e pode pagar noturno inexistente no trabalhado efetivo.** Caso COM feriado 08–12/13–23, janela corta saída às22, HE extra declarada de300 conforme fixture B-SPLIT retirada: efetivo08–22, trabalho780, noturno0; a dobra grava HE100780 e chama split sobre bruto08–23, produzindo **HE100 noturna60**, violando `noturnas/fator=0` (`app/ponto/motor_calculo_v2.py:3224`, `:3238`, `:3260`, `:3357`, `:3361`, `:1015`). Medido por AST das funções reais: `he_noturna(08–23, HE100780, intervalo12–13)=60`; `minutos_noturnos(08–22)=0` (`:440`, `:441`).

4. **Recorte longo continua contando noturno duas vezes e agora a dobra diária escreve esse resultado.** `minutos_noturnos` estende o primeiro segmento até `fim` e soma também22–24 (`app/ponto/motor_calculo_v2.py:111`, `:112`, `:116`). Medido: feriado04–23, toda duração HE1001140 → split **1200**, regra1140; feriado04–01(+1), HE1001260 → split **1440**, regra1260 (`:440`, `:441`, `:448`). Os loops novos chamam esse split (`:3104`, `:3361`). O noturno principal também permanece duplicado; igualdade contra o próprio campo não certifica o minuto contado uma vez (`:983`, `:986`).

5. **Os totais HE100/HE100 feriado são conservados nos caminhos modificados por dia**, pois o novo loop distribui `minutos_trabalhados` de cada período e o mensal soma os campos; não há cálculo novo de duração (`app/ponto/motor_calculo_v2.py:3100`, `:3102`, `:3357`, `:3359`, `:329`, `:332`). A promoção mantém as atribuições anteriores de total e adiciona apenas noturna (`app/ponto/services/fechamento.py:955`, `:956`, `:962`; `app/ponto/services/dia_pago.py:249`, `:252`). O 12x36 permanece com dobra por período seguida de split (`app/ponto/motor_calculo_v2.py:2809`, `:2855`).

Nenhuma execução integral de banco/fechamento, produção, DIFF de frota ou validação do hash foi feita. [não verificado]

## 07/11 02:21 · sessao 4c69df77b36c · - · - · origem=Ronald (tmux)

Achado concreto no leitor de alertas: A6 diz “HE idêntica em 3+ dias”, mas conta períodos, sem agrupar nem deduplicar datas. Redistribuir a dobra cria falso positivo: três períodos do mesmo feriado com 180 min cada produzem `A6=[d,d,d]`; antes `[0,0,540]` não produzia A6. Executei em memória o trecho real extraído por AST: resultados `{}` antes e três datas iguais depois. Exemplo completo pelo pareador [não verificado]. Fontes: `app/ponto/services/esmeril_espelho.py:20`, `:174-180`; contador mostrado como ocorrências em `:199`; novos escritores `app/ponto/motor_calculo_v2.py:3097-3104` e `:3354-3361`.

Mapa dos campos:

| Campo | Escritores | Leitores |
|---|---|---|
| `minutos_extra`, `_100`, `_100_feriado` | 12x36 `motor_calculo_v2.py:2810`; partido reset `:2933`, dobra `:3100`, excedente `:3153`; comercial período `:3269`, reset `:3332`, dobra `:3357`, excedente `:3416` | Soma mensal `:325-344`; DiaPago `dia_pago.py:180-184`; calculador `alimentacao.py:102-106`; espelho `espelho.py:112-114`; invariante export `porta_export.py:542`; alerta A6 `esmeril_espelho.py:176` |
| `_50_noturna`, `_100_noturna` | Casca canônica `motor_calculo_v2.py:1014-1022`; resets partido `:2937-2938`, comercial `:3336-3337` | Soma mensal `:340-344`; DiaPago `dia_pago.py:181`, `:184`, `:252` |
| `FechamentoMensal.horas_extras_100_noturna` | `fechamento.py:924`, promoção `:962-964`; persistência `:1169` | Comparação com DiaPago pela porta `porta_export.py:388`; TXT não emite esse subconjunto separadamente `folha/export.py:424-447` |
| `DiaPago.horas_extras_100_noturna` | `dia_pago.py:184`, promoção `:252`; eventual substituição pelo calculador `:322-330` | Campo incluído no universo lavrado `:36`; presença de dinheiro `:863` |

Os leitores de soma aceitam múltiplos períodos: `ResultadoMes` soma todos em `motor_calculo_v2.py:325-344`; a alimentação entrega todas as pontas em `calculador/alimentacao.py:102-106`; `calculador/regras.py:827-835` chama `he_noturna` por ponta e soma. Não há exigência de último período nesses trechos. O produtor alternativo v3 ainda grava a dobra inteira no último em `calculador/periodo.py:841-844`; isso é um escritor próprio preexistente, não uma suposição desses leitores.

Mensal e DiaPago recebem a mesma partição `ft_feriado`, disjunta dos outros grupos em `fechamento.py:975-981`, e ambos somam o mesmo campo do período: `fechamento.py:962-964` e `dia_pago.py:244-253`. A igualdade numérica em produção [não verificado].

Há outra divergência de contrato já existente: motor mede o split no par **cru**, `motor_calculo_v2.py:1015`; calculador recebe o par **efetivo**, `calculador/alimentacao.py:83`, `:106`, e o mede em `regras.py:832`. As novas chamadas passam a expor esse descompasso nos feriados curados. Contraprova com janela real depende do restante da auditoria [não verificado].

Tela, calendário e PDF leem totais lavrados e não somam os subconjuntos novamente: `templates/ponto/espelho.html:279-283`; `colaboradores/services/calendario.py:651-656`; `relatorios/pdf_espelho.py:837-859`; `dia_pago.py:914-928`. TXT mantém HE100 total/feriado e adicional noturno, sem nova linha para HE noturna: `folha/export.py:420-447`.

O invariante da porta ainda agrupa períodos pela data civil da entrada, `porta_export.py:538-544`, enquanto a lavratura usa o mapa da jornada em `dia_pago.py:174`. Redistribuir HE entre pares que cruzam a meia-noite muda os agrupamentos desse contador; falso positivo concreto dele [não verificado].

## 07/11 02:21 · sessao 2195e8a35400 · - · - · origem=Ronald (tmux)

- Referência órfã: `_dia_do_F10h` diz “batidas do F10f”, mas esse teste foi retirado; corrigir o comentário em `app/ponto/tests/test_o280_bateria_da_lei.py:1580`. Ausência do símbolo confirmada por busca; demais órfãos [não verificado].
- Censo coerente: contrato restaurado para **10** ajudantes em `app/ponto/tests/test_contract_juiz_batida.py:326`; AST contou exatamente 10 em `MotorBase`. MATRIZ preserva apenas F11/F11b/F11c/F11d/F11r e F10h/F10i, todos exercidos por comparações de valores (`app/ponto/tests/test_o280_bateria_da_lei.py:210`, `:212`, `:1272`, `:1287`).
- RED estático da base: F11 **0→120 min**, F11b **0→60 min**, F11c mensal/DiaPago **0→1 h**, F11d **0→360 min**; o diff fornecido mostra os ramos anteriores sem split, e as expectativas estão em `app/ponto/tests/test_o280_bateria_da_lei.py:1670`, `:1708`, `:1748`, `:1779`. Execução na base [não verificado].
- F11d “Era 240” descreve a primeira cura, que chamou split só no último período; **19aeede1 tinha zero**, pois dava `continue` sem split. Convém explicitar a versão em `app/ponto/tests/test_o280_bateria_da_lei.py:1776`.
- F11r é referência preexistente **300 min**, não RED do item (`app/ponto/tests/test_o280_bateria_da_lei.py:1676`, `:1692`).
- F10h gêmeo fixa **570 HE100 feriado / 390 noturna**, incluindo cauda zero (`app/ponto/tests/test_o280_bateria_da_lei.py:1602`, `:1606`). O pino compara valores, mas `expectedFailure` aceita outras falhas; também exige **660 HE100 feriado**, mudança de pertença retirada do escopo (`:1611`, `:1624`). O gêmeo exercita a mesma fixture, portanto o conjunto não é vazio (`:1601`, `:1623`).
- F10i exige **90/102,86**, comportamento preservado e guarda de outro escopo; não é RED do item (`app/ponto/tests/test_o280_bateria_da_lei.py:1643`).
- Os testes de mutantes longos recuperaram a mordida anterior, coerente com a retirada do teto em `minutos_noturnos`: `app/ponto/tests/test_v3_parei_p5_parte_menor_que_todo.py:365`, `:368`, `:376`; regra ainda sem teto em `app/ponto/motor_calculo_v2.py:110`. Suite/banco não executados [não verificado].

## 07/11 01:07 · sessao 9e4c27bca95a · FIO-E0-V4-E1 · verificador · origem=Code

**Não considero a cura integralmente fechada.** Restam contraexemplos de reabertura repetida, domínio reconstruído antes da porta canônica e ordem de travas Chamado ↔ Colaborador. A preservação do legado também depende de não reutilizar a instância após rollback. (`app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/services/fio.py:766`; `app/chamados/services/disputa_emissao.py:1342`)

1. Para cada **PROCEDENTE-CURADO**, minha conferência é:

| Achado da triagem | Veredito sobre a cura |
|---|---|
| Perda do legado no primeiro uso | **Fecha a sobrescrita sequencial normal:** captura antes de alterar e registra junto da resposta nova. **Não fecha retry da mesma instância após rollback**, explicado no item 2. (`app/chamados/services/disputa_emissao.py:1341`; `app/chamados/services/disputa_emissao.py:1362`) |
| Reabertura sem decisão | **Fecha irmã muda absorvida e validação sem texto.** Porém `desfazer_absorcao=True` torna o predicado incondicional e permite outra linha numa segunda chamada sobre pergunta já pendente. (`app/chamados/services/disputa_emissao.py:1475`; `app/chamados/services/disputa_emissao.py:1485`) |
| Ressuscitação apaga projeções | **Fecha a perda das projeções selecionadas:** registra antes de descarimbar, no mesmo `atomic`. Não adiciona uma decisão própria de ressuscitação; preserva respostas/validações preexistentes. (`app/chamados/models.py:1512`; `app/chamados/models.py:1513`; `app/chamados/models.py:1515`) |
| Absorção não atômica | **Fecha o par carimbo/ABSORCAO de cada irmã:** ambos estão dentro do mesmo `atomic`. A função inteira continua contendo trabalho anterior e várias unidades por irmã. (`app/chamados/services/materializacao.py:521`; `app/chamados/services/materializacao.py:532`; `app/chamados/services/materializacao.py:557`) |
| Autoria descartada | **Fecha os dois chamadores apontados:** correção pelo colaborador transmite autor/motivo; devolução da ausência transmite usuário sistema/motivo. (`app/chamados/models.py:2165`; `app/chamados/services/materializacao.py:673`) |
| Ciclo Disputa ↔ Colaborador | **Fecha os caminhos alterados; não estabelece ordem global.** Resta Chamado ↔ Colaborador, detalhado no item 4. (`app/chamados/services/disputa_emissao.py:507`; `app/chamados/services/disputa_emissao.py:945`; `app/chamados/services/disputa_emissao.py:1208`; `app/chamados/services/fio.py:766`) |
| DRY agrupa NULL que o índice não iguala | **Fecha a apresentação enganosa:** a colisão sai explicitamente como `SEM-DIA(o indice nao ve)`. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:116`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:117`) |
| DRY quebra com dia nulo | **Fecha esse caminho:** não chama competência para `dia=None`, usa balde próprio e trata a ordenação/impressão. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:51`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:94`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:95`) |
| DRY mede qualquer linha, não correspondência | **Fecha a divergência de predicado:** usa o mesmo helper do escritor. Herda, porém, suas limitações de significado e correspondência. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:45`; `app/chamados/models.py:3412`) |
| Lote da declaração deriva domínio paralelo | **Fecha esse lote:** ele chama a autoridade comum. Outros filtros manuais permanecem, inclusive antes do nascimento. (`app/chamados/services/declaracao_texto.py:547`; `app/chamados/services/disputa_emissao.py:388`; `app/chamados/services/fabrica_da_celula.py:118`) |
| RED não cobre primeira sobrescrita legada | **Fecha a lacuna específica do cenário:** monta projeção sem decisões e exige o texto antigo no histórico após sobrescrever. Execução RED/GREEN: **[não verificado]**. (`app/chamados/tests/test_e1_volta2.py:43`; `app/chamados/tests/test_e1_volta2.py:58`; `app/chamados/tests/test_e1_volta2.py:66`) |
| Inventário/máscara quebrados | **Cura estática coerente:** nome/campos acompanham o rename; escrita deixa de usar o queryset que recusa `bulk_update`. Execução na sombra: **[não verificado]**. (`bin/sombra_regras_pessoais.py:139`; `bin/sombra_mascara_pessoal.py:62`; `app/chamados/models.py:3320`) |
| Conversa reutilizada como fato | **Fecha na porta canônica**, que agora exclui conversa. Não fecha leitores anteriores à porta que ainda montam sua própria coleção. (`app/chamados/models.py:2466`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/services/fabrica_da_celula.py:118`) |
| Unidade do teto | **Comentário corrigido:** custo por chamada a `_nascer`, inclusive reuso. O delta quantitativo alegado continua **[não verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:172`; `app/ponto/tests/test_selo_chokepoint_escrita.py:175`) |

2. Os escritores dos quatro campos lidos pelo helper são:

| Campo | Escritores persistentes identificados |
|---|---|
| `resposta_colab` | Resposta humana: `app/chamados/services/disputa_emissao.py:1342`; automática: `app/chamados/services/disputa_emissao.py:1509`; limpeza na reabertura: `app/chamados/services/disputa_emissao.py:1459`, aplicada em `app/chamados/services/disputa_emissao.py:1487` ou `app/chamados/services/disputa_emissao.py:1489`. |
| `respondida_em` | Humana: `app/chamados/services/disputa_emissao.py:1343`; automática: `app/chamados/services/disputa_emissao.py:1510`; recuperação da trilha: `app/chamados/services/disputa_emissao.py:1531`; carimbo: `app/chamados/models.py:2691`; limpeza na reabertura: `app/chamados/services/disputa_emissao.py:1459`; ressuscitação: `app/chamados/models.py:1515`. |
| `validada_em` | Validação: `app/chamados/services/validacao.py:134`; devolução: `app/chamados/services/validacao.py:199`; resposta nova: `app/chamados/services/disputa_emissao.py:1355`; carimbo: `app/chamados/models.py:2694`; reabertura: `app/chamados/services/disputa_emissao.py:1459`; ressuscitação: `app/chamados/models.py:1515`. |
| `validada_por` | Validação: `app/chamados/services/validacao.py:135`; devolução: `app/chamados/services/validacao.py:200`; resposta nova: `app/chamados/services/disputa_emissao.py:1356`; carimbo: `app/chamados/models.py:2695`; reabertura: `app/chamados/services/disputa_emissao.py:1459`; ressuscitação: `app/chamados/models.py:1515`. |

Há atribuições somente em memória na simulação da ressuscitação e na restauração após tentativa recusada de carimbo; elas não devem ser contadas como outro escritor persistente. (`app/chamados/models.py:1462`; `app/chamados/models.py:1463`; `app/chamados/models.py:2722`; `app/chamados/models.py:2724`)

Os leitores identificados que afetam o contrato são:

| Campo | Leitores e usos |
|---|---|
| `resposta_colab` | Helper: `app/chamados/models.py:3418`, `app/chamados/models.py:3421`; vitalidade: `app/chamados/juizes.py:1114`, `app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1259`; fechamento: `app/chamados/models.py:1797`; carimbo: `app/chamados/models.py:2689`; display: `app/chamados/models.py:3006`; declaração: `app/chamados/services/declaracao_texto.py:377`; materialização: `app/chamados/services/materializacao.py:278`, `app/chamados/services/materializacao.py:400`; foto da reabertura: `app/chamados/services/disputa_emissao.py:1479`; guardas automática/trilha: `app/chamados/services/disputa_emissao.py:1504`, `app/chamados/services/disputa_emissao.py:1527`. |
| `respondida_em` | Helper: `app/chamados/models.py:3409`, `app/chamados/models.py:3416`; vitalidade: `app/chamados/juizes.py:1152`, `app/chamados/juizes.py:1259`; estado/propriedade: `app/chamados/models.py:2605`, `app/chamados/models.py:2983`; fechamento: `app/chamados/models.py:1796`; carimbo: `app/chamados/models.py:2690`; validação/recusa: `app/chamados/services/validacao.py:381`, `app/chamados/services/validacao.py:555`; reabertura/trilha: `app/chamados/services/disputa_emissao.py:1479`, `app/chamados/services/disputa_emissao.py:1527`; materialização: `app/chamados/services/materializacao.py:1361`; DRY: `app/chamados/management/commands/backfill_decisoes_pergunta.py:37`. |
| `validada_em` | Helper: `app/chamados/models.py:3409`, `app/chamados/models.py:3419`; vitalidade: `app/chamados/juizes.py:1137`; estado/SQL: `app/chamados/models.py:2601`, `app/chamados/models.py:2895`; coleção ancorada/fechamento: `app/chamados/models.py:331`, `app/chamados/models.py:1795`; carimbo: `app/chamados/models.py:2693`; resposta nova: `app/chamados/services/disputa_emissao.py:1345`; reabertura: `app/chamados/services/disputa_emissao.py:1480`; validação/devolução/recusa: `app/chamados/services/validacao.py:147`, `app/chamados/services/validacao.py:195`, `app/chamados/services/validacao.py:384`, `app/chamados/services/validacao.py:552`; declaração: `app/chamados/services/declaracao_texto.py:221`; materialização: `app/chamados/services/materializacao.py:1362`; reconciliador: `app/chamados/reconciliador.py:620`, `app/chamados/reconciliador.py:1039`; DRY: `app/chamados/management/commands/backfill_decisoes_pergunta.py:37`. |
| `validada_por` | Helper: `app/chamados/models.py:3421`; foto do carimbo: `app/chamados/models.py:2686`; foto da resposta/reabertura: `app/chamados/services/disputa_emissao.py:1345`, `app/chamados/services/disputa_emissao.py:1480`; foto da validação: `app/chamados/services/validacao.py:147`; devolução da ausência: `app/chamados/services/materializacao.py:667`; procedência da batida: `app/ponto/services/procedencia_batida.py:72`; painel: `app/chamados/services/painel.py:260`; contagem por autor: `app/chamados/services/verbos.py:240`, `app/chamados/services/verbos.py:244`; auditoria: `app/chamados/management/commands/auditar_invariantes_chamados.py:1074`. |

Também há consumidores fora dessas portas:

- APIs leem texto/instante, e a API de correção usa o carimbo como condição. (`app/api/views.py:1189`; `app/api/views.py:1191`; `app/api/views.py:1203`; `app/api/views.py:2596`; `app/api/views.py:2598`; `app/api/views_mensageria.py:567`; `app/api/views_mensageria.py:569`)
- Histórico lê dinamicamente os dois timestamps e o texto; métricas usam resposta/validação e autoria. (`app/chamados/services/historico.py:99`; `app/chamados/services/historico.py:101`; `app/chamados/services/historico.py:105`; `app/chamados/services/historico.py:109`; `app/chamados/services/verbos.py:311`; `app/chamados/services/painel.py:268`)
- Cobrança, avisos, becos, relatório, raio X e vigia usam `respondida_em`. (`app/chamados/services/cobranca.py:229`; `app/chamados/services/aviso_pendencia.py:13`; `app/chamados/services/becos.py:30`; `app/chamados/relatorios_disputa.py:21`; `app/chamados/services/raiox.py:136`; `app/chamados/services/vigia_fio.py:675`)
- Canal, explicador, calendário, ficha, prévia da folha, retrato do dia e cartório usam o texto. (`app/chamados/canal.py:65`; `app/chamados/services/explicador.py:79`; `app/colaboradores/services/calendario.py:474`; `app/colaboradores/services/ficha.py:171`; `app/folha/previa.py:142`; `app/ponto/services/dia_do_colab.py:70`; `app/ponto/services/cartorio.py:1590`)
- Lavra, suprajuiz e reconciliação de grade usam `respondida_em`; vigia do carimbo e flip usam `validada_em`. (`app/chamados/veredito_lavrado.py:167`; `app/ponto/supra_juiz.py:545`; `app/ponto/management/commands/reconciliar_grade.py:461`; `app/chamados/services/vigia_carimbo.py:98`; `app/ponto/services/flip_auto.py:130`)
- Apresentação lê projeção diretamente e também por `resposta_display`. (`app/templates/chamados/partials/_card_atendimento.html:100`; `app/templates/chamados/partials/_pergunta_card.html:23`; `app/templates/chamados/partials/_pergunta_card.html:31`; `app/templates/chamados/partials/modal_fio.html:218`)

Para os campos produzidos, estes são os escritores da sequência, todos passando pelo INSERT de `registrar`:

| Origem | Campos fornecidos |
|---|---|
| Helper, resposta recuperada | `ato`, `ato_em`, `ato_texto`, `ato_motivo`; autor fica no default nulo. (`app/chamados/models.py:3417`; `app/chamados/models.py:3418`; `app/chamados/models.py:3359`) |
| Helper, validação recuperada | Os cinco, usando `validada_por_id`. (`app/chamados/models.py:3420`; `app/chamados/models.py:3421`; `app/chamados/models.py:3422`) |
| Resposta humana | Ato, instante, autor, texto; motivo fica no default vazio. (`app/chamados/services/disputa_emissao.py:1366`; `app/chamados/models.py:3361`) |
| Resposta automática / trilha | Ato, instante, texto, motivo; autor fica nulo. (`app/chamados/services/disputa_emissao.py:1513`; `app/chamados/services/disputa_emissao.py:1534`; `app/chamados/models.py:3359`) |
| Validação | Ato, instante, autor, texto; motivo vazio. (`app/chamados/services/validacao.py:139`; `app/chamados/models.py:3361`) |
| Devolução / reabertura / recusa | Os cinco campos. (`app/chamados/services/validacao.py:205`; `app/chamados/services/disputa_emissao.py:1476`) |
| Silêncio / retratação | Os cinco campos. (`app/chamados/models.py:1870`; `app/chamados/models.py:1990`) |
| Absorção | Ato, instante, autor, texto; motivo vazio. (`app/chamados/services/materializacao.py:560`; `app/chamados/models.py:3361`) |

`registrar` consome esses dicionários sem deduplicação; `ato/ato_em` alimentam correspondência e ordenação, `ato` dirige o histórico, e o template consome os cinco campos. A máscara reescreve texto/motivo na sombra. (`app/chamados/models.py:3428`; `app/chamados/models.py:3413`; `app/chamados/models.py:3374`; `app/chamados/models.py:2994`; `app/templates/chamados/partials/modal_fio.html:208`; `app/templates/chamados/partials/modal_fio.html:211`; `bin/sombra_regras_pessoais.py:139`)

**A correspondência não possui significado uniforme entre escritores:**

- Resposta humana e validação usam o instante de processamento do ato. (`app/chamados/services/disputa_emissao.py:1337`; `app/chamados/services/validacao.py:134`)
- Upload automático compartilha o `agora` obtido antes do lote. (`app/api/foto_recebida.py:42`; `app/api/foto_recebida.py:48`)
- Recuperação usa `criado_em` da primeira nota encontrada, não o instante da recuperação. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:34`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`)
- `carimbar` usa instante do carimbo, fornecido ou obtido internamente, para preencher resposta/validação ausentes. Essa porta não registra esses dois atos na sequência. (`app/chamados/models.py:2683`; `app/chamados/models.py:2691`; `app/chamados/models.py:2694`; `app/chamados/models.py:2711`)

O helper transforma todas essas origens em `resposta`/`validacao`, com motivo genérico. Sua igualdade compara apenas pergunta, ato e instante: **não verifica texto, autor nem natureza do evento**. (`app/chamados/models.py:3396`; `app/chamados/models.py:3412`; `app/chamados/models.py:3417`; `app/chamados/models.py:3420`)

Os contraexemplos são:

- **Duas consultas antes de inserir:** ambas calculam a mesma linha ausente; ambas podem inseri-la. O SELECT não reserva correspondência, `registrar` faz `bulk_create`, e não há UNIQUE dessa tripla. (`app/chamados/models.py:3412`; `app/chamados/models.py:3375`; `app/chamados/models.py:3428`)
- **Pergunta repetida no lote:** `[p, p]` produz duplicatas porque `ja` não é atualizado a cada `append`. Chamador atual passando esse lote: **[não verificado]**. (`app/chamados/models.py:3412`; `app/chamados/models.py:3415`; `app/chamados/models.py:3417`)
- **Mesmo instante, conteúdo diferente:** uma linha existente com a tripla impede preservar outra projeção com texto/autor diferente. Estado real assim em produção: **[não verificado]**. (`app/chamados/models.py:3413`; `app/chamados/models.py:3416`; `app/chamados/models.py:3419`)
- **Trilha 2x:** mesma instância ou releitura atualizada para na guarda; duas instâncias antigas com timestamp nulo podem passar e registrar duas vezes a mesma correspondência. (`app/chamados/services/disputa_emissao.py:1527`; `app/chamados/services/disputa_emissao.py:1531`; `app/chamados/services/disputa_emissao.py:1534`)
- **Automática 2x:** mesma instância atualizada para pelo texto; duas instâncias antigas não são serializadas por essa função. (`app/chamados/services/disputa_emissao.py:1504`; `app/chamados/services/disputa_emissao.py:1508`; `app/chamados/services/disputa_emissao.py:1513`)
- **Reabertura da absorção 2x:** queryset estável por PK registra outra reabertura mesmo após a primeira limpar tudo, devido ao `or desfazer_absorcao`. O chamador da ausência normalmente recompõe o universo pelo carimbo; isso protege sua repetição sequencial normal, mas não a porta com queryset estável nem duas capturas anteriores à limpeza. (`app/chamados/services/disputa_emissao.py:1485`; `app/chamados/services/materializacao.py:650`; `app/chamados/services/materializacao.py:662`; `app/chamados/services/materializacao.py:673`)
- **Rollback seguido de retry da mesma instância:** parta de legado `08:00/T0`; a tentativa altera `p` para `09:00/T1`; `registrar` levanta e o banco volta, mas não há restauração de `p`. Na repetição, o helper lê `09:00/T1` da memória e pode sobrescrever `08:00/T0` sem preservá-lo. Na automática, a repetição pode simplesmente retornar `False` pelo texto que só ficou em memória. Ocorrência real: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1341`; `app/chamados/services/disputa_emissao.py:1342`; `app/chamados/services/disputa_emissao.py:1362`; `app/chamados/services/disputa_emissao.py:1504`; `app/chamados/services/disputa_emissao.py:1509`; `app/chamados/models.py:3407`)

3. `do_fato`, `linhas_dos_fatos` e o lote **agora compartilham o predicado**: colaborador em coluna, pares exatos motivo/dia, instância calculada, conversa excluída e vias fora do fato excluídas. (`app/chamados/models.py:2445`; `app/chamados/models.py:2465`; `app/chamados/models.py:2466`; `app/chamados/models.py:2467`; `app/chamados/services/declaracao_texto.py:547`)

O DRY contém dois universos diferentes: `linhas_a_criar` recupera projeções respondidas/validadas, inclusive fora do domínio de identidade; `grupos_de_colisao` recompõe conversa/vias e agrupa a instância **armazenada**. Com instância divergente do catálogo, essa segunda consulta não equivale estritamente ao helper que exige instância calculada. Existência desse acervo: **[não verificado]**. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:37`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:73`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`; `app/chamados/models.py:2465`)

**Leitores manuais sobrevivem e afetam produção:**

- `gerar_perguntas_disputa` faz dois `exists()` por disputa/motivo/dia antes de chamar `abrir`. Remover um motivo mantendo outro na disputa, e depois recolocá-lo no mesmo dia, deixa a retratada satisfazer o `exists`; a nova pergunta é suprimida, embora `do_fato` exclua aquela linha. (`app/chamados/models.py:1987`; `app/chamados/models.py:1994`; `app/chamados/services/disputa_emissao.py:388`; `app/chamados/services/disputa_emissao.py:404`; `app/chamados/services/disputa_emissao.py:423`; `app/chamados/models.py:2467`)
- A fábrica monta `ja` excluindo somente duplicata: retratada, conversa ou instância divergente podem calar o motivo. (`app/chamados/services/fabrica_da_celula.py:118`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:126`)
- `_candidata_reusavel` e `_ja_perguntou` também recompõem a coleção por disputa/motivo/dia. (`app/chamados/services/declaracao_texto.py:236`; `app/chamados/services/disputa_emissao.py:1290`)

4. A porta comum, o agrupador e a âncora foram corrigidos localmente. `ChamadoColaborador.abrir` e a abertura de formulário também pedem Colaborador primeiro. (`app/chamados/services/disputa_emissao.py:507`; `app/chamados/services/disputa_emissao.py:945`; `app/chamados/services/disputa_emissao.py:1208`; `app/chamados/models.py:872`; `app/chamados/se

[truncado em 20000 de 30793 caracteres]

## 07/11 01:03 · sessao 8a23f7624596 · - · - · origem=Ronald (tmux)

Todos os locais pedidos são leitores de `PerguntaDisputa`, confirmados pela origem do queryset/relacionamento:

| Leitor | Campos e efeito |
|---|---|
| API de chamados | `resposta_colab` em `app/api/views.py:1189`; `respondida_em` em `:1191`; exige todas respondidas para solicitar correção em `:1203`. |
| API de disputa | Texto em `app/api/views.py:2596`; timestamp em `:2598`. |
| Fila de validação | `app/api/views_mensageria.py:567` lê texto; `:569` lê timestamp; `:570` calcula dias esperando. |
| Histórico | Queryset explícito de PD em `app/chamados/services/historico.py:96`; escolhe dinamicamente `respondida_em`/`validada_em` em `:99`/`:101`, lê via `getattr` em `:105` e texto em `:109`. |
| Métrica de cobrança respondida | `app/chamados/services/verbos.py:309` consulta PD; filtra/projeta `respondida_em` em `:311`/`:312`; compara com última cobrança em `:318`. |
| Calendário | `app/colaboradores/services/calendario.py:473` consulta PD; `:474` exclui texto vazio; `:476`/`:477` usa vitalidade para bloquear último recurso. |
| Ficha | `app/colaboradores/services/ficha.py:166` recebe perguntas do app; `:171` deriva “sem resposta” pelo texto. |
| Prévia da folha | `app/folha/previa.py:142` lê texto; `:143` classifica como “resposta do colaborador”. |
| Suprajuiz | `app/ponto/supra_juiz.py:543`/`:545` e `:547`/`:548` percorrem perguntas dos relacionamentos e vetam execução CSF quando `respondida_em` existe. |
| Retrato do dia | Queryset PD em `app/ponto/services/dia_do_colab.py:64`; serialização do texto em `:70`. |
| Cartório | `app/ponto/services/cartorio.py:1586`/`:1590` define presença de resposta pelo texto; recebe perguntas em `:1594` e veta reconciliação em `:1597`. |

Há consequência concreta para “campo com dois significados”: o histórico rotula qualquer `respondida_em` como ato de “o colaborador” e qualquer `validada_em` como ato de “o admin”, independentemente do escritor automático (`app/chamados/services/historico.py:99`, `:101`, `:113`, `:114`). Isso é incompatível com interpretar automaticamente todos esses timestamps como instantes de ação humana, pois `PerguntaDisputa.carimbar` também os escreve (`app/chamados/models.py:2691`, `:2694`). Não foi demonstrado caso real nessa cópia/dado [não verificado].

O inventário anterior de leitores era parcial: essa leitura encontrou inclusive acesso indireto por `getattr(p, campo)`, que uma busca apenas por atributos diretos perde (`app/chamados/services/historico.py:105`). Exaustividade absoluta de leitores indiretos/dinâmicos permanece [não verificado].

## 07/11 01:02 · sessao 8a23f7624596 · - · - · origem=Ronald (tmux)

1. Escritores dos campos da projeção, em produção/comandos:

| Campo | Quem escreve |
|---|---|
| `resposta_colab` | Resposta humana: `app/chamados/services/disputa_emissao.py:1342`; automática: mesmo arquivo `:1509`; limpeza na reabertura: `:1459`, aplicada por `:1487`/`:1489`. |
| `respondida_em` | Resposta humana: `disputa_emissao.py:1343`; automática: `:1510`; recuperação da trilha: `:1531`; carimbo automático condicionado a texto: `app/chamados/models.py:2691`; limpeza na reabertura: `disputa_emissao.py:1459`; ressuscitação: `models.py:1515`. |
| `validada_em` | Validação: `app/chamados/services/validacao.py:134`; devolução: `:199`; resposta nova desfaz: `disputa_emissao.py:1355`; carimbo automático: `models.py:2694`; reabertura: `disputa_emissao.py:1459`; ressuscitação: `models.py:1515`. |
| `validada_por` | Validação: `validacao.py:135`; devolução: `:200`; resposta nova desfaz: `disputa_emissao.py:1356`; carimbo automático: `models.py:2695`; reabertura: `disputa_emissao.py:1459`; ressuscitação: `models.py:1515`. |

Há também atribuições exclusivamente em memória: simulação da ressuscitação em `models.py:1462`/`:1463`; restauração da fotografia anterior à tentativa de `carimbar` em `models.py:2722`/`:2724`. Os defaults/campos ficam em `models.py:2256`/`:2258` e `:2913`.

O comando de acervo que recupera `respondida_em` delega a `carimbar_resposta_pela_trilha`, passando `nota.criado_em`: `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. Não encontrei outro escritor direto desses quatro campos nos arquivos Python de produção examinados; escritas dinâmicas não detectáveis por nomes literais permanecem [não verificado].

2. Leitores relevantes por campo, incluindo autoridades e consumidores que distinguem seu significado:

- `resposta_colab`: C6/juízes em `app/chamados/juizes.py:1114`, `:1152`, `:1259`; carimbo em `models.py:2689`; display em `models.py:3006`; materialização em `services/materializacao.py:278`, `:400`, `:562`; lote/declaração em `services/declaracao_texto.py:377`; fotografia de reabertura em `disputa_emissao.py:1479`; recuperação da trilha em `:1527`/`:1535`; helper em `models.py:3418`/`:3421`.
- `respondida_em`: C6 em `juizes.py:1152`/`:1259`; estado da pergunta em `models.py:2605`/`:2608` e propriedade `:2983`; entrada da validação em `validacao.py:381`; carimbo em `models.py:2690`; foto da reabertura em `disputa_emissao.py:1479`; guarda da recuperação em `:1527`; helper em `models.py:3409`, `:3416`, `:3417`; DRY em `management/commands/backfill_decisoes_pergunta.py:37`.
- `validada_em`: C6 em `juizes.py:1137`; estado em `models.py:2601`; SQL de vitalidade em `models.py:2895`; guarda da validação em `validacao.py:384`; devolução em `:195`; carimbo em `models.py:2693`; foto da reabertura em `disputa_emissao.py:1480`; helper em `models.py:3409`/`:3419`/`:3420`; DRY em `backfill_decisoes_pergunta.py:37`.
- `validada_por`/`_id`: foto da validação em `validacao.py:147`; foto antes de resposta nova em `disputa_emissao.py:1345`; foto de reabertura em `:1480`; carimbo em `models.py:2686`; helper em `models.py:3421`; procedência da batida em `app/ponto/services/procedencia_batida.py:72`; painel distingue usuário sistema em `services/painel.py:260`; verbos contabilizam autores staff em `services/verbos.py:240`/`:244`; auditoria distingue usuários sistema em `management/commands/auditar_invariantes_chamados.py:1074`.

Essa lista cobre os leitores relevantes ao contrato auditado; inventário integral de todos os leitores de apresentação/filtros, incluindo templates e leitores indiretos, não foi completamente consolidado [não verificado].

3. Campos produzidos e consumidores:

| Campo | Produção no helper | Consumo |
|---|---|---|
| `ato` | `models.py:3417`/`:3420` | `registrar`, `models.py:3428`; correspondência em `:3413`; leitor histórico em `:2994`/`:2998`; DRY em `backfill_decisoes_pergunta.py:61`; template em `app/templates/chamados/partials/modal_fio.html:208`. |
| `ato_em` | `models.py:3417`/`:3420` | `registrar :3428`; correspondência `:3413`; ordenação `:3374`; template `modal_fio.html:209`/`:211`. |
| `ato_por` | Só validação, via `ato_por_id`, `models.py:3421`; resposta recuperada não recebe autor e usa NULL (`:3359`). | `registrar :3428`; prefetch em `services/fio.py:232`; template `modal_fio.html:211`. |
| `ato_texto` | `models.py:3418`/`:3421` | `registrar :3428`; template `modal_fio.html:209`/`:211`; máscara em `bin/sombra_regras_pessoais.py:139`. |
| `ato_motivo` | `models.py:3418`/`:3422`, constante `:3396` | `registrar :3428`; template `modal_fio.html:211`; máscara `bin/sombra_regras_pessoais.py:139`. |

O ramo `resposta` do template mostra texto e instante, mas não mostra `ato_motivo`; o motivo “projeção sem linha” fica oculto nesse ramo (`modal_fio.html:208`/`:211`).

4. Os instantes não têm o mesmo significado:

- Humana: relógio do processamento do ato, `_tz.now()`, em `disputa_emissao.py:1337`, compartilhado com projeção e linha em `:1343`/`:1366`.
- Automática do upload: um `agora` obtido antes do lote em `app/api/foto_recebida.py:42`, passado a cada resposta em `:48`; escritor grava o mesmo instante em `disputa_emissao.py:1510`/`:1513`.
- Recuperação: instante histórico da primeira nota encontrada, ordenada por `criado_em`, em `curar_perguntas_em_limbo.py:31`/`:34`, passado em `:67`; gravação em `disputa_emissao.py:1531`/`:1534`.
- Carimbo: instante de materialização, fornecido pelo chamador ou relógio interno, em `models.py:2683`; preenche `respondida_em` e `validada_em` em `:2691`/`:2694`, sem registrar `RESPOSTA`/`VALIDACAO` nessa porta (`:2711`/`:2716`).
- Validação humana: relógio de validação em `validacao.py:134`, também usado na linha em `:139`.

Portanto, o helper normaliza origens diferentes como `resposta`/`validacao`, com motivo genérico que admite “carimbo sem ato” (`models.py:3396`, `:3417`/`:3422`). A igualdade por `(pergunta, ato, ato_em)` é uma heurística de correspondência; não demonstra igualdade de texto, autor ou natureza do evento, pois só consulta esses três valores (`models.py:3412`/`:3413`).

5. Duplicação/omissão e repetição:

- **Reabertura da absorção 2x: duplicação certa por inspeção.** Com queryset estável, a segunda chamada com `desfazer_absorcao=True` registra de novo mesmo com texto/timestamps/carimbo já vazios, porque o predicado termina em `or desfazer_absorcao` (`disputa_emissao.py:1485`). A inserção antecede a limpeza em `:1475`/`:1489`; não há unicidade da correspondência nas constraints (`models.py:3375`/`:3385`). O teste chama somente uma vez (`app/chamados/tests/test_e1_volta2.py:202`).
- **Helper não é deduplicador do próprio lote.** `[p, p]` devolve duas linhas por ato ausente: `ja` é calculado uma vez e nunca atualizado durante os `append` (`models.py:3412`/`:3422`). Não encontrei chamador atual que passe esse lote duplicado [não verificado].
- **Dois cálculos antes de registrar duplicam.** Duas chamadas ao helper antes de qualquer inserção devolvem ambas a mesma pendência; `registrar` insere sem resolução de conflito (`models.py:3428`). O teste verifica o caso estritamente sequencial “calcular, registrar, calcular” (`test_e1_volta2.py:90`/`:92`), não esse intercalamento. [nao verificado]
- **Trilha 2x com duas instâncias antigas:** ambas podem entrar com `respondida_em=None`, passar a guarda `disputa_emissao.py:1527`, e inserir a mesma correspondência em `:1534`. Na mesma instância ou recarregando após a primeira escrita, a guarda evita a segunda chamada (`:1527`/`:1531`). Concorrência/ocorrência real [não verificado].
- **Automática 2x na mesma instância ou recarregada:** texto existente produz no-op (`disputa_emissao.py:1504`/`:1505`). Duas instâncias antigas sem texto não são serializadas nem recarregadas antes de salvar/inserir (`:1508`/`:1514`).
- **Falha seguida de retry na mesma instância perde a cura do legado:** o core altera `p` antes do `atomic` (`disputa_emissao.py:1342`/`:1360`); se `registrar` levantar em `:1366`, o banco volta mas o objeto Python continua novo. Retry lê essa projeção falsa no helper (`models.py:3407`/`:3418`) e pode deixar de preservar o legado real do banco. Em `responder_automaticamente`, a primeira tentativa também muta antes do atomic (`:1509`/`:1511`); após rollback, retry na mesma instância retorna `False` por texto já preenchido em memória (`:1504`). Trata-se de contraexemplo inferido do código; execução/prod [não verificado].
- **Omissão por correspondência insuficiente:** existindo linha com mesmos três valores, texto/autor divergentes não motivam nova linha; esses campos não participam da consulta (`models.py:3412`/`:3422`). Existência desse estado em produção [não verificado].

Não usei banco, logs nem testes com escrita; os contraexemplos acima são análise estática [não verificado]. [nao verificado]

## 07/11 01:02 · sessao b6f748cc49ac · - · - · origem=Ronald (tmux)

3. **As três consultas passaram a compartilhar o domínio; o DRY tem dois universos distintos.** `do_fato` delega a `linhas_dos_fatos`; esta exige coluna `colaborador`, `conversa=False`, par exato motivo/dia, instância derivada do motivo e exclusão de `VIAS_FORA_DO_FATO`. O lote chama a mesma função. (`app/chamados/models.py:2445`, `app/chamados/models.py:2465`, `app/chamados/services/declaracao_texto.py:547`)

- `linhas_a_criar` do DRY procura qualquer projeção respondida/validada, inclusive conversa e pergunta retirada do fato: sua finalidade é recuperar decisões, e chama `linhas_da_projecao_sem_linha`. Não corresponde ao domínio de identidade. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:37`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:45`)
- `grupos_de_colisao` recompõe manualmente conversa/vias e agrupa a **instância armazenada**, enquanto `linhas_dos_fatos` só encontra a instância calculada de cada motivo. Portanto não são rigorosamente o mesmo domínio para acervo com instância divergente; ocorrência no dado fornecido [não verificado]. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:73`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:74`, `app/chamados/models.py:2465`)
- **Contraexemplo novo:** a geração ainda usa `exists()` manual por disputa/motivo/dia e desiste antes de chegar ao gate. Retire um motivo mantendo outro na disputa aberta: a pergunta vira `motivo_retratado`; se o mesmo motivo voltar no mesmo dia, a retratada satisfaz o `exists` e impede a nova pergunta, embora `do_fato` a exclua. (`app/chamados/models.py:1987`, `app/chamados/models.py:1994`, `app/chamados/services/disputa_emissao.py:388`, `app/chamados/services/disputa_emissao.py:404`, `app/chamados/services/disputa_emissao.py:423`, `app/chamados/models.py:2467`)
- Outra derivação manual permanece na fábrica: monta `ja` por disputa/colaborador e dia, excluindo **somente duplicata**; retratada, conversa ou instância divergente podem calar a emissão de motivo. (`app/chamados/services/fabrica_da_celula.py:118`, `app/chamados/services/fabrica_da_celula.py:123`, `app/chamados/services/fabrica_da_celula.py:126`)
- `_candidata_reusavel` e `_ja_perguntou` também continuam montando coleções por disputa/motivo/dia sem usar o domínio central. (`app/chamados/services/declaracao_texto.py:236`, `app/chamados/services/disputa_emissao.py:1290`)

4. **A cura fecha os três caminhos alterados, mas não estabelece ordem global.** A porta comum, o agrupador e a âncora passaram a pedir Colaborador antes da Disputa. `ChamadoColaborador.abrir` e a abertura de formulário também começam pelo Colaborador. (`app/chamados/services/disputa_emissao.py:507`, `app/chamados/services/disputa_emissao.py:945`, `app/chamados/services/disputa_emissao.py:1208`, `app/chamados/models.py:872`, `app/chamados/services/abertura.py:164`)

**Contraexemplo residual Chamado ↔ Colaborador, inferido do fluxo de produção:**

- B: a tela chama `chamado.analisar()`; este grava o status em `atomic`. O `UPDATE` de Chamado ocorre antes do receiver síncrono que chama o cartório. (`app/chamados/views.py:1513`, `app/chamados/models.py:767`, `app/chamados/models.py:773`, `app/chamados/models.py:278`, `app/chamados/models.py:283`, `app/chamados/signals.py:164`, `app/chamados/signals.py:180`)
- B: em dia passado cuja célula acusa um marco ainda sem pergunta, cartório → fábrica → emissão pede Colaborador enquanto o Chamado continua travado pelo `UPDATE`. (`app/ponto/services/cartorio.py:1989`, `app/ponto/services/cartorio.py:997`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:176`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:507`)
- A: `_nascer` segura Colaborador; ao reutilizar pergunta e trocar seu destino para esse Chamado, `_costura_nao_mata_o_fio` pede `SELECT FOR UPDATE` do Chamado. B espera Colaborador de A; A espera Chamado de B. (`app/chamados/models.py:2507`, `app/chamados/models.py:2509`, `app/chamados/models.py:2538`, `app/chamados/models.py:2547`, `app/chamados/models.py:2548`, `app/chamados/services/fio.py:765`, `app/chamados/services/fio.py:766`, `app/chamados/services/fio.py:767`)

Esse é um contraexemplo de ordem pelo código; execução concorrente e ocorrência em produção [não verificado].

O censo “dois sítios de trava da Disputa” também omite `desfazer_carimbo_sem_lastro`, que trava conjuntamente Pergunta e Disputa. Não provei que seu atomic isolado peça Colaborador depois; portanto sua simples existência não prova outro ciclo. (`app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`)

A trava de matrícula em `Colaborador.save` seleciona a última matrícula existente quando a instância não possui matrícula; não há nesse corpo nascimento de pergunta ou chamado. Sua transação interna termina antes de `super().save`, salvo transação externa do chamador. (`app/colaboradores/models.py:427`, `app/colaboradores/models.py:429`, `app/colaboradores/models.py:431`, `app/colaboradores/models.py:436`, `app/colaboradores/models.py:439`)

_2659 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
