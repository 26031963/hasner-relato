# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 01:07** · publicado em **11/11/2026 01:08** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 01:07 · sessao 37bca8857cf8 · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**A c20 cura a passagem comum do agrupador, mas a cura na origem continua parcial:** declaração e pendentes alcançam os escritores de destino sem consultar o índice; o replay ainda rederiva episódio quando o valor conservado é `None`. (`app/chamados/services/disputa_emissao.py:1001`, `:300`, `:505`, `:791`, `:799`; `app/chamados/models.py:2866`.)

Nas referências abreviadas, `disputa_emissao.py`, `fio.py`, `descarte_disputa.py` e `declaracao_texto.py` ficam em `app/chamados/services/`; `models.py`, em `app/chamados/`; testes, em `app/chamados/tests/`; `auditar_invariantes_chamados.py`, em `app/chamados/management/commands/`. [nao verificado]

1. **Estado dos achados da preverif4.r.**

   | Achado anterior | Estado nesta cópia |
   |---|---|
   | Exceção do gerador deixar nascimento incompleto | **Curado no núcleo:** pai, DS e perguntas participam do atomic; exceção propagada desfaz o conjunto. `disputa_emissao.py:638`, `:668`, `:685`, `:689`. |
   | Geração vazia confirmar nascimento, inclusive pela chamada vazia da grade | **Curado no núcleo:** sem pergunta local admitida, desfaz o nascimento. `disputa_emissao.py:690`, `:696`, `:698`. |
   | Canônica estrangeira deixar nova DS sem pergunta | **Curado para esse mecanismo:** núcleo, declaração e agrupador desfazem o nascimento local; permanecem as ressalvas de replay e destino abaixo. `disputa_emissao.py:696`, `:725`, `:792`, `:795`, `:1487`. |
   | API pública de declaração aceitar ausência de pergunta | **Curado:** `pergunta` é obrigatório; o ensaio sem pergunta ficou privado. `disputa_emissao.py:738`, `:762`, `:766`. |
   | Seed `orfao_14h` indevidamente conservado no lote com seed estrangeiro | **Curado para motivo novo:** começa sem seed e só o introduz se o próprio seed não foi estrangeiro. Não remove cópias preexistentes. `disputa_emissao.py:1312`, `:1469`, `:1472`. |
   | Segundo agregador na âncora | **Curado:** âncora entrega `['ancora_ausente']` ao agregador; união/save ficaram nele. `disputa_emissao.py:575`, `:581`, `:1582`, `:1613`. |
   | Luz HX sem leitor | **Curado no instrumento:** I7a/I7b usam zero linhas de pergunta e distinguem ausência de ata de silêncio estrutural. Medida atual: **[nao verificado]**. `auditar_invariantes_chamados.py:62`, `:69`, `:73`. |
   | Dona fechada como pergunta de lei aberta | **Não identifico nova pergunta de lei; implementação parcial da trilha/alarme**, conforme item 2. O agregador não salva a fechada; reabertura do sistema continua em porta própria. `disputa_emissao.py:567`; `models.py:2163`. |
   | Replay rederivar episódio pelo contexto da dona | **Parcial:** episódio positivo é conservado; `None` ainda permite rederivação. `disputa_emissao.py:504`; `models.py:2866`. |
   | Agrupador analisar chamado após replay divergente | **Curado nesse ramo:** exige destino confirmado e não recusado. `disputa_emissao.py:1498`, `:1508`, `:1510`. |
   | Dois mutantes abortarem na montagem | **Curado:** montagem fora do runner; alvos precisam falhar por assertion, sem errors. `test_bo_disputa_volta5_memoria.py:389`, `:396`, `:415`. |
   | “Paridade histórica” comparar a fonte atual consigo mesma | **Curada a alegação:** agora se chama repetibilidade e declara o limite. Comparação histórica continua **[nao verificado]**. `test_bo_disputa_volta3_memoria.py:252`, `:254`. |
   | Pendentes fora do atomic do núcleo | **Igual:** wrapper chama depois do retorno; helper abre P e depois agrega DS, sem atomic próprio. `disputa_emissao.py:836`, `:850`, `:300`, `:314`. |
   | Novos motivos admitidos em dona estrangeira não chegarem ao push correspondente | **Igual no núcleo misto:** agregação estrangeira não coleta seus `novos`; wrapper usa os `novos` da local. `disputa_emissao.py:623`, `:662`, `:665`, `:851`. | [nao verificado]
   | Push imediato poder anteceder confirmação final | **Igual no agrupador:** complementos e principal notificam dentro do atomic externo, sem `on_commit`. `disputa_emissao.py:1238`, `:1268`, `:1516`, `:1537`. | [nao verificado]
   | Nem todos os retornos `(p, criada)` serem integralmente consumidos | **Igual literalmente:** primeira tentativa da declaração conserva P e descarta o booleano em `_`; confere a dona. `disputa_emissao.py:791`, `:792`. |
   | Doubles não certificarem cartório/signals/concorrência | **Igual como limite de prova.** O teste c20 declara essas fronteiras simuladas. `test_bo_disputa_volta5l_memoria.py:6`. | [nao verificado]
   | Protegidos historicamente intocados, efeitos externos, produção e suíte inteira | **[nao verificado]**. | [nao verificado]

   A ressalva do replay é concreta: o emissor conserva `batida_origem_id=None`, mas `_nascer` executa `valor or derivador(dona)`. Se a dona aponta uma batida 222, o replay ganha esse episódio; se ela pertence a outro turno, também pode mudar o dia efetivo. A guarda pode desfazer o replay divergente, mas isso não reproduz o fato original. (`disputa_emissao.py:504`, `:725`, `:726`; `models.py:2866`, `:2875`; `fio.py:1633`.)

   Reproduzi em memória a sequência `(pedido=None, efetivo=None)` → `(pedido=None, efetivo=222)`, terminando em `replay-divergente`, sem costura confirmada. O teste F2 existente cobre somente episódio positivo 111; ORM desse contraexemplo: **[nao verificado]**. (`test_bo_disputa_volta5f_memoria.py:115`, `:122`, `:130`; `disputa_emissao.py:729`.) [nao verificado]

2. **c15/c16 — trilha, contador e saídas sem trilha.**

   **Quem escreve:** o agregador encaminha a fechada ao registrador; este fotografa motivos/fechamento, elimina motivos já presentes e agenda a gravação depois do commit. A escrita persistente passa por `core.observ.evento`. (`disputa_emissao.py:567`; `descarte_disputa.py:9`, `:10`, `:31`, `:37`; `app/core/observ.py:140`.)

   A identidade deduplicada é **dona + motivo + pergunta**, inclusive pergunta `None`; origem é procedência. O callback trava C→D em transação própria. (`descarte_disputa.py:20`, `:23`, `:25`, `:27`.)

   **Quem lê:** I23 lê todas as linhas históricas daquela ação/modelo; o comando publica o resultado e está declarado no cron. Conta **linhas de descarte**, não donas distintas, motivos atualmente pendentes ou janela temporal. (`auditar_invariantes_chamados.py:81`, `:647`, `:893`, `:1925`; `app/config/crons.py:542`.)

   Portanto, **contador e trilha têm o mesmo universo das linhas efetivamente gravadas**. Não cobrem necessariamente todos os descartes tentados ou confirmados no negócio. (`descarte_disputa.py:31`; `auditar_invariantes_chamados.py:81`.)

   Há estas saídas sem nova linha:

   - **Deliberadas:** motivo já presente; identidade já registrada; savepoint desfeito. (`descarte_disputa.py:10`, `:27`, `:35`.)
   - **Falha da auditoria:** callback ocorre depois do commit; `engolir` captura falhas, e `evento` também pode retornar `None`, ignorado pelo registrador. O descarte pode ficar confirmado sem trilha; I23 fica cego. Ocorrência em produção: **[nao verificado]**. (`descarte_disputa.py:19`, `:31`, `:37`; `app/core/observ.py:92`, `:111`, `:139`, `:145`.)
   - **Dedup antes do agregador:** suponha dona fechada com P local viva cujo motivo falta em `DS.motivos`, num chamado que não segue o caminho por ata. O helper soma o motivo em memória, mas retorna `False` pelo dedup; o complemento termina sem criação, o agrupador não chama o agregador/trilha e pode excluir o chamado e retornar. Reprodução ORM: **[nao verificado]**. (`disputa_emissao.py:1003`, `:1007`, `:1069`, `:1278`, `:1292`, `:1300`, `:1304`, `:1661`.)

   **Alarme parcial:** I23 positivo aparece como `VIOL`, mas **sozinho não provoca o `SystemExit(2)`**: essa decisão consulta somente F1/F2. O teste 5g exige o alarme F1/F2, além da linha I23; não prova alarme independente de I23. Consumidor externo específico: **[nao verificado]**. (`auditar_invariantes_chamados.py:95`, `:1942`; `test_bo_disputa_volta5g.py:101`, `:104`, `:107`.) [nao verificado]

3. **c19 — seed e caminhos sem pergunta.**

   Nova ou aberta vazia começa sem motivos; recebe `orfao_14h` ao final, salvo reuso estrangeiro **do próprio seed**. Estrangeira de outro motivo não impede o seed; motivos preexistentes são preservados. (`disputa_emissao.py:1312`, `:1313`, `:1469`; `test_bo_disputa_volta5k_memoria.py:54`, `:79`.)

   | Caminho | Resultado |
   |---|---|
   | **HX-ATA-NAO-SABE**, abstenção por `sem_celula` | Marca abstenção; com possibilidade de fechamento, conserva aberta e acende luz. `disputa_emissao.py:1088`, `:1094`, `:1383`, `:1525`. |
   | **h2**, ata disponível e nada a perguntar | Nasce e fecha vazia; zero P permite o fechamento. `disputa_emissao.py:1524`, `:1530`; `models.py:1853`, `:1903`. |
   | Nova, somente perguntas estrangeiras | Desfaz nascimento local, repete na dona e retorna `None`. `disputa_emissao.py:1487`, `:1494`, `:1513`. |
   | Aberta preexistente sem P | Não recebe esse rollback, que exige `nova`; segue ao fechamento/HX. `disputa_emissao.py:1487`, `:1524`. |
   | Exceção propagada durante o nascimento | Desfaz o atomic; não confirma nascimento vazio. `disputa_emissao.py:1238`, `:1316`. |
   | **Recusa técnica sem testemunho de ata** | Falta de data/base, marcos inválidos ou falha de leitura pode terminar zero P e fechar. Não demonstra as premissas estritas de HX ou h2. `disputa_emissao.py:1054`, `:1083`, `:1362`, `:1418`, `:1445`, `:1448`, `:1524`. |

   **Existe caminho fora das duas premissas.** Contraexemplo: chamado `batida_ausente`, aberto, `contexto_json={}`, sem origem/célula/pergunta anterior. Regularizabilidade aceita pelo mapa; não segue ata; o laço sem origem abandona-o por falta de data. A DS recebe o seed e fecha vazia **sem consultar ata nem acender luz**. (`models.py:3667`; `disputa_emissao.py:925`, `:930`, `:949`, `:955`, `:1121`, `:1337`, `:1445`, `:1469`, `:1524`.)

   Esse caminho foi reproduzido com funções produtivas via AST e infraestrutura em memória: regularizável, zero P, fechada, zero chamadas à ata/luz. Isso não certifica os efeitos ORM/signals: **[nao verificado]**. A infraestrutura de fechamento é explicitamente double. (`test_bo_disputa_volta5b_orfaos_memoria.py:77`, `:84`, `:96`.)

4. **c20 — origem e equivalência de `(motivo, dia)`.**

   | Parte | Índice | Caminho B |
   |---|---|---|
   | Fonte | `ch.contexto_json['marcos_faltantes']`. `disputa_emissao.py:1252`. | A mesma lista do chamado sobrevivente. `disputa_emissao.py:1356`. |
   | Motivo | `tipo == 'S'` → saída; qualquer outro → volta. `:1259`. | Mesma expressão. `:1359`. |
   | Dia | `date.fromisoformat(marco.get('data'))`. `:1256`. | Mesma expressão, depois envolvida por `dia_do_turno_da_celula`. `:1361`, `:1367`. |
   | Exceções | `TypeError`, `ValueError`. `:1257`. | `Exception`. `:1362`. |
   | Universo | Regularizáveis antes dos complementos. `:1250`. | Sobreviventes do filtro posterior aos complementos. `:1291`, `:1297`, `:1351`. |

   **Para JSON ordinário, mesmo marco e estado estável, motivo/dia concordam.** `DiaDoTurno` é `NewType`; o construtor usado por B não converte a data em execução. Tipo ausente/desconhecido vira “volta” em ambos. (`disputa_emissao.py:1259`, `:1359`; `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2939`, `:2943`.)

   **Não há equivalência universal dos dois procedimentos:** diferem no tratamento de exceções e, principalmente, no universo e momento da leitura. Divergência de exceção com dado JSON persistido ordinário: **[nao verificado]**. (`disputa_emissao.py:1257`, `:1291`, `:1297`, `:1362`.)

   **Índice com chave e B sem execução correspondente:** intervalo regularizável já possui P não materializada e não segue `cobra_marcos_da_ata`. Entra no índice, pode ser complementado e depois é excluído pelo filtro; B não o percorre. O índice continua disponível e pode direcionar o complemento do chamado genérico para esse intervalo. (`disputa_emissao.py:1250`, `:1280`, `:1291`, `:1300`, `:1351`, `:1001`.)

   **B com chave ausente no índice**, mantendo estado e contexto estáveis entre as leituras: **[nao verificado]**. A implementação não fixa o resultado do predicado entre as passagens. (`disputa_emissao.py:1250`, `:1299`.)

   Havendo chave, a passagem comum substitui o destino antes de abrir e conserva-o no replay estrangeiro; sem chave, mantém o destino recebido, sem recusa adicional. Se dois intervalos oferecem a mesma chave, `setdefault` escolhe o primeiro do lote, inclusive para pedidos posteriores do segundo. (`disputa_emissao.py:1001`, `:1018`, `:1029`, `:1260`, `:1497`.)

5. **Censo de escritores de `PerguntaDisputa.chamado_destino`.**

   | Escritor persistente | Local e alcance |
   |---|---|
   | Nascimento | Construtor/inserção em `models.py:3014`, `:3017`. |
   | Reuso da canônica | Julga e atribui destino, conservando a dona da P. `models.py:2972`, `:2980`, `:2982`, `:3006`, `:3008`. |
   | `reapontar_resolvedora` | Julga, atribui e salva. `fio.py:907`, `:909`, `:925`, `:927`. |
   | Deleção da FK | Apagar o chamado produz `NULL`; não escolhe outro chamado. `models.py:3385`, `:3387`. |

   As atribuições de `fio.py:779`/`:783` são temporárias e restauradas; `models.py:366`/`:367` reassocia instância do mesmo PK. O próprio contrato registra os dois escritores produtivos e a FK. (`fio.py:807`, `:814`.)

   **Pedidos pela passagem comum:** complemento anterior ao filtro, B, ata, grupo por origem e caminho sem origem passam pelo override c20. (`disputa_emissao.py:1291`, `:1366`, `:1387`, `:1425`, `:1431`, `:1457`, `:1462`, `:1001`.)

   **Pedidos fora dela:**

   - Pendentes chamam o gerador diretamente; ele deriva destino, abre P e pode chamar a reata. O índice não chega a esse gerador. (`disputa_emissao.py:1486`, `:300`, `:412`, `:503`, `:505`, `:519`; `app/chamados/reconciliador.py:1642`, `:1655`.)
   - Declaração chama `abrir` e, condicionalmente, `reapontar_resolvedora`, sem consultar o índice. (`disputa_emissao.py:784`, `:786`, `:791`, `:799`.)

   **Sim, permanece pedido capaz de costurar no genérico uma pergunta do intervalo explícito:** P de `intervalo_saida` em B, já destinada ao chamado `intervalo`; declaração pelo chamado genérico `ch`, do mesmo colaborador/dia, casa o marco de intervalo da ata e passa `ch` como destino. A candidata estrangeira é recusada pelo helper local; `abrir` reutiliza a canônica e pode trocar seu destino para `ch`, inclusive no replay confirmado. (`declaracao_texto.py:250`, `:254`, `:341`, `:346`, `:365`; `disputa_emissao.py:791`, `:799`; `models.py:2982`, `:3006`.)

   O administrador faz o mesmo pedido. Os juízes conferem colaborador, turno e estado do fio; não elegem preferência pelo chamado com marco explícito de intervalo. (`declaracao_texto.py:650`, `:653`; `app/chamados/juizes.py:2185`, `:2208`, `:2217`; `fio.py:771`.)

   Reproduzido em memória: mesma P/dona, destino `intervalo` → destino `ch`. A porta de reuso é double; execução ORM: **[nao verificado]**. (`test_bo_disputa_volta5c_memoria.py:101`, `:104`; `disputa_emissao.py:799`.)

   Os contraexemplos pedidos **da passagem comum** passaram em memória: só `ch`, só intervalo, ambas as ordens, dona aberta/fechada e replay 2x; também outro dia e intervalo-volta. Isso não certifica o protegido ORM. (`test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:112`, `:119`, `:126`, `:136`; `test_bo_disputa_volta5b_orfaos.py:157`, `:159`.)

6. **Quem escreve, quem lê e significado dos campos.**

   | Campo | Escritores alcançáveis | Leitores que dão significado ao delta |
   |---|---|---|
   | `DS.motivos` | Agregador/nascimentos: `disputa_emissao.py:575`, `:685`, `:1337`, `:1605`; aparo/remoção/reabertura: `models.py:1966`, `:2063`, `:2159`. | União/admissão/seed/push: `disputa_emissao.py:571`, `:606`, `:1312`, `:1469`, `:1535`; auditor: `auditar_invariantes_chamados.py:50`; foto: `app/api/foto_recebida.py:61`. |
   | `DS.fechada_em` | Fechar/reabrir admin/sistema: `models.py:1932`, `:2149`, `:2173`. | Recusa/eleição/push: `disputa_emissao.py:567`, `:621`, `:645`, `:846`, `:1267`, `:1536`; receivers: `app/chamados/signals.py:28`, `:129`, `:153`; validação: `app/chamados/services/validacao.py:214`. |
   | `DS.contexto` | Agregador/nascimentos: `disputa_emissao.py:578`, `:685`, `:1337`, `:1605`; aparo/remoção: `models.py:1967`, `:2064`. | Resolução e episódio: `disputa_emissao.py:346`, `:373`; `models.py:3463`; auditor: `auditar_invariantes_chamados.py:31`. |
   | `P.disputa` | Inserção: `models.py:3014`, `:3017`; reuso preserva-a: `:3008`; deleção da DS usa CASCADE: `:2416`. | Encaminhamento: `disputa_emissao.py:613`, `:1022`; candidata: `declaracao_texto.py:254`; autorização/listagem: `fio.py:116`, `:598`. |
   | `P.chamado_destino` | Os quatro casos do item 5. `models.py:2982`, `:3014`, `:3387`; `fio.py:925`. | Complemento/filtro: `disputa_emissao.py:1280`, `:1301`; juiz: `app/chamados/juizes.py:2150`; autorização: `fio.py:598`; materialização: `app/chamados/services/materializacao.py:1076`. |
   | `P.motivo` | Inserção: `models.py:3014`, `:3017`. | Encaminhamento: `disputa_emissao.py:614`; identidade: `fio.py:1793`; candidata: `declaracao_texto.py:250`; materialização: `app/chamados/services/materializacao.py:1077`. |
   | `P.batida_origem_id` | Argumento conservado: `disputa_emissao.py:504`; nascimento/derivação: `models.py:2866`, `:2868`, `:3449`. | Identidade e dia: `models.py:2907`; `fio.py:1633`. |
   | `P.data_evento` | Porta de atribuição: `fio.py:790`, `:800`; nascimento passa por ela: `models.py:3016`. | Dedup/chave: `disputa_emissao.py:402`, `:1001`, `:1661`; juiz: `fio.py:1608`. |
   | Trilha nova | Registrador → `evento`: `descarte_disputa.py:31`; inserção: `app/core/observ.py:140`. | Dedup: `descarte_disputa.py:27`; I23: `auditar_invariantes_chamados.py:81`; apresentação: `:1925`. |

   **Semântica:** `DS.motivos` é lista de cenários; pode conter seed sem P daquele motivo. `P.motivo` participa da identidade de um fato datado/instância. Não são uma relação um a um nem uma chave global equivalente. (`models.py:1743`, `:2427`, `:3483`; `disputa_emissao.py:1469`.)

   **Dia civil versus turno:** conversa tem discriminador explícito e dia civil; pergunta datada usa o dia do fato/turno. Pendentes ignoram a data civil contextual e derivam pela batida. O problema residual é a origem rederivável no replay, não uma conversão feita por `DiaDoTurno`. (`models.py:2801`, `:2811`, `:2875`; `app/chamados/juizes.py:2188`; `disputa_emissao.py:275`, `:284`; `app/ponto/turnos.py:2943`.)

   O índice novo compara igualdade; não introduz intervalo inclusivo/exclusivo. Nos atos legados, o grupo por origem ainda classifica pela data civil antes de resolver o dia do turno. Equivalência dessas decisões para todo turno noturno: **[nao verificado]**. (`disputa_emissao.py:1001`, `:1411`, `:1412`, `:1417`.)

   Os receivers de fechamento leem o valor de `fechada_em`, sem conferir transição ou `update_fields`; salvar uma fechada não é automaticamente um no-op. O novo agregador evita esse save. (`app/chamados/signals.py:28`, `:129`, `:153`; `disputa_emissao.py:567`.)

7. **Classes de reprovação e mordida dos selos.**

   | Classe | Estado no delta/atos |
   |---|---|
   | Segundo juiz duplicando autoridade registrada | **Nenhum novo identificado no delta** para fechamento, regularizabilidade ou costura: continuam delegados. `disputa_emissao.py:966`, `:1524`; `models.py:2972`; `fio.py:907`. |
   | Segundo escritor fora da porta | **Agregação da âncora curada; nenhum novo escritor persistente de destino encontrado fora dos dois registrados/FK.** Isso não cura os pedidos divergentes que chegam a eles. `disputa_emissao.py:1613`; `fio.py:807`, `:814`; `models.py:2982`. |
   | Relógio fora da borda | **Nenhuma leitura nova identificada em c15–c20; presente nos atos legados:** contexto, seleção de passado e SLA. `disputa_emissao.py:554`, `:914`, `:679`, `:1331`. |
   | Derivação paralela de fonte única | **Presente na c20:** índice e B repetem motivo/dia, com filtros e capturas diferentes. `disputa_emissao.py:1250`, `:1256`, `:1259`, `:1359`, `:1361`. |
   | Selo/RED sem mordida | **Curado nos dois mutantes antigos e ausente nos quatro c20 exe

[truncado em 20000 de 22665 caracteres]

## 11/11 01:06 · sessao bd452f9f99a5 · FIO-E0-V4-E3 · leitura · origem=Code

**V=3, T=229, X=0; soma=232.** As causas se distribuem em **228 d, 2 c e 2 e**. (`sonda_232.out:5–236`; classificação fundamentada abaixo.)

- **V:** existe ação exigível pela lei — decisão do admin ou exposição de pergunta viva — que a tarefa omite. (`app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316`)
- **T:** nenhum marco ou decisão residual demonstrado exige ação; o vigia chama de morto um dia sem tarefa exigível. (`app/docs/LEIS.md:95`; `app/chamados/juizes.py:426–441`; `app/docs/CORTES.md:406`)
- **X:** caso que não cabe nesses critérios; **nenhum neste recorte**. (`sonda_232.out:5–236`, aplicando as regras acima.)

As referências `sonda_232.out:N` numeram **o anexo colado**, contando `99 objects...` como linha 1, a linha vazia como 2 e o primeiro `DIA` como 5. Não representam leitura de um arquivo local.

A tabela principal é esta; **todas as datas são de 2026**. (`sonda_232.out:5–236`)

| Grupo | Classe / quantidade | Colaborador/dia | Fundamentação |
|---|---:|---|---|
| Respondida viva, sem validação, âncora terminal | **V / 2** | col252/09-29; col841/10-10 | Há decisão residual do admin. A causa **c** vem depois de excluir resposta vazia; a tarefa para em `dia_sem_trabalho` antes de examinar a pergunta. (`sonda_232.out:154,224`; `app/chamados/services/vigia_fio.py:673–676`; `app/chamados/services/tarefa_do_dia.py:241–243`; `app/docs/RELATO.md:2336`) |
| Pergunta grave viva em disputa fechada | **V / 1** | col237/11-07 | A lei exige apresentação ao admin pela vida da pergunta, independentemente do fechamento da disputa. O motivo grave fica fora do mapa de marcos da tarefa. (`sonda_232.out:31`; `app/docs/CORTES.md:316`; `app/chamados/services/tarefa_do_dia.py:225–226`; `app/docs/RELATO.md:1736`) |
| Sem instâncias e sem pergunta viva | **T / 201** | Lista completa na tabela seguinte | Pelo fluxo executado: dia sem trabalho, nenhuma instância elegível, nenhuma ação residual demonstrada. (`sonda_232.out:5–236`, dias discriminados abaixo; `app/chamados/services/tarefa_do_dia.py:199–210,235–243`) |
| Instâncias presentes, todas encerradas por `dia_sem_trabalho` | **T / 16** | col97/11-09; col177/11-08; col292/10-22,10-28; col348/10-24,10-31,11-07,11-08; col354/10-30; col741/11-01,11-05,11-07,11-09; col866/11-06,11-08; col889/10-25 | Não há pergunta viva nesses dias; as instâncias não geram cobrança. (`sonda_232.out:9,20,42–43,54–57,59,86–92`; `app/chamados/juizes.py:426–441`) |
| Instâncias presentes, todas `aceso`, sem pergunta viva | **T / 11** | col354/10-21; col655/09-30,10-21; col922/11-01; col996/10-26; col205/09-21; col258/09-28,10-19; col259/10-14,10-15; col368/09-21 | Os marcos estão cumpridos; não há resposta viva residual demonstrada. (`sonda_232.out:58,85,93,113,138,155–158,189,223`; `app/docs/LEIS.md:95`; `app/docs/CORTES.md:406`) |
| Pergunta não respondida viva, mas todos os marcos acesos | **T / 1** | col962/10-25 | A pergunta `intervalo_saida` não cria falta de um marco já cumprido. Aplica-se a regra específica dos marcos acesos; não existe aqui resposta pendente da exceção acima. (`sonda_232.out:102`; `app/docs/CORTES.md:406`; `app/chamados/juizes.py:1364–1375`) |

Para expandir os **201 T sem instâncias**, use os conjuntos abaixo; cada colaborador indicado corresponde a **todas** as datas da respectiva célula. (`sonda_232.out`, referências por linha na tabela.)

- **A:** 10-21, 10-23, 10-25, 10-27, 10-29, 10-31, 11-02, 11-04, 11-06, 11-08. (`sonda_232.out:10–19`)
- **B:** 09-21, 09-23, 09-25, 09-27, 09-29, 10-01, 10-03, 10-05, 10-07, 10-09, 10-11, 10-13, 10-15, 10-17, 10-19. (`sonda_232.out:123–137`)

| Colaborador(es) | Datas | Quantidade | Fonte |
|---|---|---:|---|
| col144, col209, col268, col312, col427, col969 | A | 60 | `sonda_232.out:10–19,21–30,32–41,44–53,60–69,103–112` |
| col144, col209, col268, col312 | B | 60 | `sonda_232.out:123–137,139–153,159–188` |
| col427 | B, exceto 09-21 | 14 | `sonda_232.out:190–203` |
| col650 | 10-22,10-24,10-26,10-28,10-30,11-01,11-03,11-05,11-07,11-09 | 10 | `sonda_232.out:75–84` |
| col650 | 09-22,09-24,09-26,09-28,09-30,10-02,10-04,10-06,10-08,10-10,10-12,10-14,10-16,10-18,10-20 | 15 | `sonda_232.out:208–222` |
| col969 | 09-26,09-27,10-03,10-04,10-10,10-11,10-12,10-17,10-19 | 9 | `sonda_232.out:228–236` |
| col97 | 11-01,11-03,11-05,11-07 | 4 | `sonda_232.out:5–8` |
| col574 | 10-24,10-31,11-07 | 3 | `sonda_232.out:70–72` |
| col574 | 09-26,10-03,10-10,10-17 | 4 | `sonda_232.out:204–207` |
| col639 | 11-06,11-08 | 2 | `sonda_232.out:73–74` |
| col950 | 10-24,10-28,10-29,10-31,11-01,11-02,11-07,11-08 | 8 | `sonda_232.out:94–101` |
| col950 | 10-12,10-17,10-18 | 3 | `sonda_232.out:225–227` |
| col1005 | 11-02 | 1 | `sonda_232.out:114` |
| col1009, col1018 | 11-08 | 2 | `sonda_232.out:115–116` |
| col1019 | 11-04,11-06,11-08 | 3 | `sonda_232.out:117–119` |
| col1027 | 11-05,11-07,11-09 | 3 | `sonda_232.out:120–122` |
| **Total** | | **201** | Soma das linhas acima |

**1. Entrada no universo e causas do vigia.** Todos os 232 percorrem o mesmo caminho: empresas visíveis → competências de `competencias_a_lavrar` → `dias_que_seguram` → inserção de `(colaborador, data)` em `dias`. `dias_que_seguram` seleciona colaboradores classificados fora por `furo_espelho` e consulta os dias retidos por `motivos_retencao_celula`. O fato exigido para entrar é **retenção da folha**, não pergunta viva ou marco faltante. (`app/chamados/services/vigia_fio.py:581–587`; `app/folha/fora_da_folha.py:118–137`; `app/folha/fora_da_folha.py:95–99`)

Como todos têm célula, há dois ramos possíveis de retenção: **veredito acusador**, sem passar pelas dispensas de retenção; ou **veredito nulo**, `trabalha` preenchido e dia passado fora da borda de demissão. Qual desses dois introduziu **cada dia** é **[nao verificado]**: a sonda não imprime veredito, protestos nem `trabalha`. (`sonda_232.out:5–236`; `app/folha/export.py:1233–1256`)

Depois, o vigia consulta a porta; separa hoje/futuro; consulta os quatro baldes de responsabilidade já atribuída; somente então incrementa `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:635–660,695–699`)

| Causa | Condição, na ordem efetiva | Neste recorte |
|---|---|---:|
| **a** | Chamado vivo da célula ou âncora viva fora da fila. (`app/chamados/services/vigia_fio.py:663–670`) | 0 |
| **b** | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:673–674`) | 0 |
| **c** | Pergunta viva respondida, âncora terminal; resposta vazia já foi excluída. (`app/chamados/services/vigia_fio.py:673–676`) | 2 |
| **d** | Sem pergunta viva; dia fora da lista CxR; colaborador presente nela. Célula existente e ausência de chamado vivo decorrem dos testes anteriores. (`app/chamados/services/vigia_fio.py:667–678`) | 228 |
| **e** | Restante, com detalhe nomeado. Aqui: pergunta viva sob chamado terminal. (`app/chamados/services/vigia_fio.py:679–694`) | 2 |

As quantidades dessa tabela vêm das linhas do anexo. (`sonda_232.out:5–236`)

**A causa d não afirma que o dia exige ação, nem atribui dono CADASTRO.** Afirma presença do **colaborador** na CxR e ausência daquele **dia** na lista. A lista vem de `ListaDoAdmin → cadastro_realidade.lista → dias_na_lista`; os colaboradores são extraídos separadamente das linhas por posto. Somente assinaturas que nomeiam uma `data` põem o dia na lista. (`app/chamados/services/vigia_fio.py:205–218,230–251,677–678`; `app/escala/services/cadastro_realidade.py:1044–1064`)

**2. Caminho da tarefa.** A premissa `instancias=[]` vale para **202**, não para todos os 232: são os 201 T da tabela mais col237; os outros 30 têm instâncias com ações vazias. Todos têm `fio='nada'`, fila vazia, `nao_sei=False` e razão geral vazia. (`sonda_232.out:5–236`)

Nos 202 vazios, o fluxo permite inferir `sem_trabalho=True`: caso contrário, ausência de instâncias produziria `nao_venceu` ou `ata_sem_lampadas`. As instâncias são extraídas das células da grade, exigindo tipo E/S e hora; ausência de pergunta não é o critério que decide sua existência. (`app/chamados/services/tarefa_do_dia.py:62–67,84–103,199–210,390–403`)

Nos outros 30, a tarefa encerra cada instância em `dia_sem_trabalho` (**18**) ou `aceso` (**12**), antes de examinar pergunta e decisão. Sem ações, `_portas_da_tarefa` produz `fio='nada'`, fila e verbos vazios; fila residual não cria ação. (`sonda_232.out:9,20,42–43,54–59,85–93,102,113,138,154–158,189,223–224`; `app/chamados/services/tarefa_do_dia.py:241–246,268–283,418–434`)

O fato consultado para dispensar cobrança é `dia_sem_trabalho`, alimentado pela célula/ata; para cumprir marco é a lâmpada lavrada. `marcos_pendentes=[]` apenas informa ausência de marca `pendente` na ata: sozinho não demonstra ausência de obrigação. (`app/ponto/supra_juiz.py:65–71`; `app/chamados/juizes.py:426–441`; `app/ponto/services/lastro.py:138–150`)

**3. Por que os três V e o T de col962 diferem.** Em col252 e col841, a pergunta respondida continua viva. A regra preserva a decisão do admin quando a resposta não foi absolvida pela concordância com a luz e pelo dia coerente; `dia_sem_trabalho` não extingue automaticamente essa decisão residual. A tarefa a ignora porque termina antes da pergunta. (`sonda_232.out:154,224`; `app/docs/RELATO.md:2336`; `app/chamados/juizes.py:1341–1353`; `app/chamados/services/tarefa_do_dia.py:241–243`)

Em col237, disputa fechada e âncora resolvida não substituem resolução da pergunta viva. A lei manda expô-la ao admin; a tarefa restringe-se aos motivos de marco e não alcança `batida_fora_escala_grave`. O conteúdo da decisão que o admin tomará é **[nao verificado]**. (`sonda_232.out:31`; `app/docs/CORTES.md:316`; `app/chamados/services/tarefa_do_dia.py:225–226`; `app/chamados/services/declaracao_texto.py:42–47`)

Em col962, a pergunta não foi respondida e os quatro marcos estão acesos. A regra específica dispensa cobrança com todos os marcos cumpridos. O C6 pode manter vida pelo julgamento geral da célula, pois seu encerramento direto pelo marco aceso exige também `materializada_em`; isso não transforma o marco cumprido em falta. O ramo individual que manteve esse C6 vivo é **[nao verificado]**. (`sonda_232.out:102`; `app/docs/CORTES.md:406`; `app/chamados/juizes.py:1364–1375,1388–1400`)

**4. As quatro hipóteses anteriores não explicam este recorte.** Não há rota só-fila, porta A/B ou verbos A/B observados: todas as portas são nulas e todos os chamados/âncoras impressos estão terminais. Nenhum retorno passou pelo ramo de isenção. Portanto, nenhuma das quatro hipóteses se aplica como causa demonstrada destes 232 resultados. (`sonda_232.out:5–236`; `app/chamados/juizes.py:1578–1585`; `app/chamados/services/tarefa_do_dia.py:183–189`)

A LEI-C17-UM-JUIZ escolhe a tarefa nos quatro casos anteriormente medidos; ela não declara corretas todas as saídas atuais da tarefa. O aval desta fase exige justamente decidir as divergências pela regra. (`app/docs/RELATO.md:244`; `aval_e3_troca_pelo_diff_1111.txt:4–5`)

**5. Contraexemplo por ISENÇÃO: não aparece nestes 232.** Nenhum retorno tem razão geral `isento_ponto`, obrigatória naquele ramo. Os dois casos em que `dia_sem_trabalho` esconde decisão residual são col252 e col841, classificados V; **dia sem trabalho não equivale a isenção**. Existência desse contraexemplo fora do recorte: **[nao verificado]**. (`sonda_232.out:5–236`; `app/chamados/services/tarefa_do_dia.py:183–189,202,241–243`)

**6. Não falta regra escrita para estas classificações.** Há regra para a lâmpada, para resposta residual e para pergunta viva em disputa fechada. Falta **dado**, para todos os 232, para individualizar o ramo de retenção da folha; isso não é pergunta de lei. (`app/docs/LEIS.md:95`; `app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316,406`; `app/folha/export.py:1233–1256`; `sonda_232.out:5–236`)

Como **V não é vazio**, a condição “classe só T” não se verifica: pela ordem do aval, a cura na tarefa precede a troca dos leitores. (`aval_e3_troca_pelo_diff_1111.txt:5`)

## 11/11 01:04 · sessao 6585a794233b · - · - · origem=Ronald (tmux)

**Itens 2 e 3 — conclusões para consolidação.**

1. **Trilha e contador têm o mesmo universo somente depois de a gravação da auditoria ter sido bem-sucedida.** O agregador encaminha dona fechada para `registrar_descarte_em_dona_fechada`; o registrador elimina motivos já presentes, agenda `on_commit`, trava C→D e deduplica por `(dona,motivo,pergunta_id)` antes de chamar o escritor único `core.observ.evento`. Origem não participa da identidade. (`app/chamados/services/disputa_emissao.py:567`; `app/chamados/services/descarte_disputa.py:10`, `:23`, `:27`, `:31`, `:37`; `app/core/observ.py:100`, `:140`.)

   I23 lê **todas as linhas históricas** com ação/modelo correspondentes, sem filtro de estado atual, dia ou janela. Conta linhas, não donas distintas: duas perguntas/motivos da mesma dona podem produzir dois elementos iguais na amostra. O formatador publica `VIOL` se positivo, conserva a descrição integral e integra o comando executado pelo cron. (`app/chamados/management/commands/auditar_invariantes_chamados.py:77`, `:81`, `:89`, `:647`, `:893`, `:1925`; `app/config/crons.py:542`.)

   I23 positivo, isoladamente, **não provoca o `SystemExit(2)` deste comando**, que verifica somente F1/F2. Isso prova a diferença entre sinal visual e código de saída; alarme externo que leia a linha I23: **[nao verificado]**. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1942`.)

2. **Há saídas sem uma nova linha de trilha.**

   - Motivo já presente na dona: saída deliberada antes do callback; repetição da mesma identidade: dedup deliberado. Não são novos descartes registrados. (`app/chamados/services/descarte_disputa.py:10`, `:27`.)
   - Replay divergente: savepoint desfeito e nenhum novo encaminhamento confirmado; seus callbacks são cancelados. (`app/chamados/services/disputa_emissao.py:1496`, `:1501`; `app/chamados/services/descarte_disputa.py:35`.)
   - **Falha da própria trilha:** depois de o negócio confirmar, o callback pode falhar na trava/consulta; `engolir` captura. `evento` também captura sua falha de inserção e retorna `None`; o registrador ignora o retorno. Nesse caso, o descarte confirmado fica sem linha, e I23 não consegue contá-lo. O restante observável é ERROR e contador de engolidos do processo. Ocorrência em produção: **[nao verificado]**. (`app/chamados/services/descarte_disputa.py:19`, `:23`, `:31`, `:37`; `app/core/observ.py:79`, `:92`, `:111`, `:139`, `:145`.)
   - **Silêncio por dedup anterior ao agregador:** se o complemento anterior ao filtro encontra dona fechada com pergunta local viva cujo motivo falta em `DS.motivos`, o helper acrescenta o motivo em memória, mas retorna `False` pelo dedup. O complemento termina `criou=False`; o agrupador só agrega/registra quando `_cc=True`, depois exclui o chamado já coberto e pode retornar `None`. Não há incorporação nem trilha desse motivo faltante. Pressupõe esse acervo inconsistente e chamado que não segue `cobra_marcos_da_ata`; exemplo possível: categoria `intervalo_ausente`, subcategoria mapeada `batida_orfa`, módulo diferente de `batida_ausente`. Reprodução ORM: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1003`, `:1007`, `:1069`, `:1278`, `:1292`, `:1297`, `:1304`, `:1661`; `app/chamados/models.py:3678`; `app/chamados/juizes.py:1324`, `:1399`.)

3. **A pergunta de lei “dona FECHADA” está encerrada pelo aval fornecido; a implementação preserva a decisão e instrumenta o descarte.** O agregador não salva/reabre a dona fechada. O registro usa snapshot do fechamento encontrado e continua histórico mesmo que outra cascata reabra a dona depois. Persistência dessas cascatas nesta árvore: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:567`; `app/chamados/services/descarte_disputa.py:9`, `:22`; aval DUAS-LEIS fornecido, sem arquivo local: **[nao verificado]**.)

4. **Seed c19:** começa dos motivos existentes; somente nova/aberta vazia recebe `orfao_14h` ao final, salvo se o próprio motivo aparece em alguma estrangeira do ato. Estrangeira de outro motivo não impede o seed. O seed também pode permanecer como motivo sem pergunta específica quando há outra pergunta local. Isso é o comportamento explicitamente cobrado pelos selos K3, não exclusividade “um motivo = uma pergunta”. (`app/chamados/services/disputa_emissao.py:1312`, `:1467`; `app/chamados/tests/test_bo_disputa_volta5k_memoria.py:54`, `:79`.)

5. **Sem pergunta no agrupador:**

   | Caminho | Resultado sustentado |
   |---|---|
   | HX: `_ata_do_dia` recusa por `sem_celula` com escala vigente | `_absteve_por_ata=True`; conserva aberta e acende luz. (`app/chamados/services/disputa_emissao.py:1094`, `:1383`, `:1525`.) |
   | h2: ata disponível e nenhum fato a perguntar | Zero perguntas permite `pode_fechar`; fecha como `vazia`. (`app/chamados/services/disputa_emissao.py:1524`; `app/chamados/models.py:1848`, `:1903`.) |
   | Só estrangeiras, local nova sem P | Desfaz nascimento local e repete perguntas na dona; retorna `None`. (`app/chamados/services/disputa_emissao.py:1487`, `:1494`, `:1513`.) |
   | Local preexistente sem P | Não recebe a mesma guarda de rollback, que exige `nova`; passa ao auto-fecho/HX. (`app/chamados/services/disputa_emissao.py:1487`, `:1524`.) |
   | **Sem data/base, marcos inválidos ou erro de leitura** | Existem recusas técnicas sem testemunho de ata; podem terminar zero P e fechar, portanto não demonstram as premissas estritas HX ou h2. (`app/chamados/services/disputa_emissao.py:1054`, `:1083`, `:1356`, `:1362`, `:1413`, `:1418`, `:1445`, `:1448`, `:1451`, `:1524`.) |

6. **Contraexemplo concreto fora das premissas das duas leis:** chamado categoria `batida_ausente`, status `aberto`, `contexto_json={}`, sem célula, sem pergunta materializada nem disputa anterior. O juiz real de regularizabilidade permite-o pelo mapa; `cobra_marcos_da_ata=False`; `dia_do_fato=None`; o laço sem origem abandona-o por falta de data. A DS nasce, recebe seed e fecha vazia **sem consultar ata e sem luz**. Isso não prova “COM ATA e nada a perguntar”, exigido no aval h2. (`app/chamados/services/disputa_emissao.py:923`, `:930`, `:949`, `:955`, `:1121`, `:1337`, `:1445`, `:1469`, `:1524`; `app/chamados/models.py:3667`; `app/chamados/juizes.py:87`, `:133`, `:138`.)

   Executei o agrupador e os juízes indicados via AST em memória, com mapas reais e infraestrutura double: `regularizavel=True`, `cobra_ata=False`, `dia=None`, DS com `['orfao_14h']`, zero P, fechada, zero consultas de ata/luz. ORM/signals/banco: **[nao verificado]**. A infraestrutura usada explicita doubles de trava, origem e fechamento. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:77`, `:84`, `:96`.)

7. **Verificação executada:** 5g memória passou 10 testes e K3 passou 2 testes; resultados não certificam Django/PostgreSQL. A trilha double substitui `engolir` por `yield` e `evento` por append infalível, portanto esses verdes não cobrem a falha de auditoria descrita acima. (`app/chamados/tests/test_bo_disputa_volta5g_memoria.py:122`, `:240`; `app/chamados/tests/test_bo_disputa_volta5k_memoria.py:54`, `:98`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:290`, `:297`, `:302`.)

## 11/11 01:03 · sessao edfffd5339dd · - · - · origem=Ronald (tmux)

Minha leitura favorece **V nos dias col252/2026-09-29, col841/2026-10-10 e col237/2026-11-07; T para col962/2026-10-25**. Os demais dias sem pergunta viva, sem ações e sem marco pendente favorecem T; não medi a contagem total do anexo. [nao verificado]

- **col252 e col841: V.** A regra escrita preserva a resposta divergente ou sem hora comparável para decisão do admin; marco aceso sozinho não extingue essa pendência. Isso está explícito em `app/docs/RELATO.md:2336`, implementado em `app/chamados/juizes.py:1341`. A tarefa pula toda análise da pergunta quando `sem_trabalho`, em `app/chamados/services/tarefa_do_dia.py:241`. A sonda demonstra exatamente perguntas respondidas, não validadas, C6 vivas, embora a tarefa diga `dia_sem_trabalho`. [nao verificado — dado do prompt]

- **col237: V.** Pergunta viva em disputa fechada deve aparecer ao admin, sem reabrir e sem push; o contador só deve cair por resolução do admin (`app/docs/CORTES.md:316`; reiterado em `app/docs/RELATO.md:2223`). `batida_fora_escala_grave` é motivo fora do chamado de marco do dia, com porta própria (`app/docs/RELATO.md:1736`). A tarefa limita perguntas aos motivos de marco (`app/chamados/services/tarefa_do_dia.py:225`), explicando a cegueira. A pergunta viva fechada e o retorno vazio constam da sonda. [nao verificado — dado do prompt]

- **col962: T.** A sonda mostra quatro lâmpadas `True`, pergunta `intervalo_saida` não respondida e nenhum marco pendente. [nao verificado — dado do prompt] A regra escrita manda deixar de contar cobrança viva quando todos os marcos estão acesos neste julgamento (`app/docs/CORTES.md:406`). O C6 ainda pode devolver verdadeiro pelo veredito acusador geral (`app/chamados/juizes.py:1372`), pois a precedência do marco preenchido exige `materializada_em` (`app/chamados/juizes.py:1368`). Isso explica o conflito de juízes sem criar falta de marco já cumprido.

**Cuidado com uma falsa equivalência no teste:** `app/chamados/tests/test_e2_tarefa_do_dia.py:814` e `:868` dizem que lâmpada acesa responde antes da pergunta, inclusive respondida. Esse comentário genérico não revoga a regra explícita das respostas divergentes/sem comparação (`app/docs/RELATO.md:2336`). Logo não usaria o teste para classificar col252/841 como T.

**C17 não decide antecipadamente este recorte inteiro:** a decisão documentada abrange os quatro mecanismos nomeados e manda tarefa responder rota/fila/verbos; não diz que toda divergência futura é T (`app/docs/RELATO.md:244`). A própria tarefa declara C6 fora de seus insumos (`app/chamados/services/tarefa_do_dia.py:29`).

**Isenção:** o ramo de isento deixa razão global `isento_ponto` e retorna antes de julgar marcos (`app/chamados/services/tarefa_do_dia.py:183`). As linhas fornecidas têm razão global vazia; portanto não sustentam erro pelo ramo de isenção. [nao verificado — dado do prompt] `dia_sem_trabalho` não significa isenção: é outro predicado e outro ramo (`app/chamados/services/tarefa_do_dia.py:202`; `:241`).

Não encontrei necessidade de nova lei para esses quatro dias: há regras escritas para resposta residual, disputa fechada e marco cumprido (`app/docs/RELATO.md:2336`; `app/docs/CORTES.md:316`; `:406`). A projeção exata dessas regras sobre os demais dias depende da consolidação do anexo. [nao verificado]

## 11/11 01:03 · sessao 27bfee3f46b7 · - · - · origem=Ronald (tmux)

- `instancias=[]`, `nao_sei=False`, `razao=''` não vêm da isenção: esse ramo sempre grava `isento_ponto`; tampouco vêm de tranca, fora de vínculo, célula ausente ou ata de trabalho sem lâmpadas, que deixam razões próprias. Pelo fluxo, correspondem a `sem_trabalho=True` e nenhuma instância extraída da grade. A grade vem do leitor único, e instâncias exigem lâmpadas com tipo E/S e hora. (`app/chamados/services/tarefa_do_dia.py:62`, `:84`, `:94`, `:180`, `:183`, `:190`, `:199`, `:202`, `:235`, `:375`.)

- Com instâncias presentes, `dia_sem_trabalho` encerra cada linha antes da pergunta; `aceso` encerra antes de examinar decisões. Por isso há linhas existentes com ações vazias, além dos dias com lista de instâncias vazia. (`app/chamados/services/tarefa_do_dia.py:235`, `:241`, `:244`.)

- Sem ações, a etapa final produz `fio='nada'`, nenhuma porta, fila vazia e verbos vazios. Fila residual não cria ação. (`app/chamados/services/tarefa_do_dia.py:418`, `:425`, `:426`, `:429`, `:430`, `:431`, `:433`.)

- A tarefa carrega apenas perguntas `conversa=False`; depois aceita somente os quatro motivos do mapa de marcos. `batida_fora_escala_grave` está fora desse mapa. Decisões só são consultadas depois das guardas de dia sem trabalho, lâmpada, vencimento e cobertura. (`app/chamados/services/tarefa_do_dia.py:225`, `:241`, `:244`, `:258`, `:281`, `:323`; `app/chamados/services/declaracao_texto.py:42`.)

- Para `col252/2026-09-29` e `col841/2026-10-10`, a causa `c` comprova pergunta C6 viva, respondida e sob âncora terminal; resposta vazia teria sido causa `b` antes. A regra escrita preserva a decisão do admin quando resposta e batida discordam ou o dia não está coerente. São candidatos sólidos a **V**: a tarefa termina em `dia_sem_trabalho`, sem chegar à resposta pendente de decisão. (`app/chamados/services/vigia_fio.py:673`, `:675`; `app/chamados/juizes.py:1341`, `:1349`, `:1350`; `app/docs/PROMPTS.md:1442`.)

- Para `col962/2026-10-25`, pergunta viva sem resposta e marco aceso não demonstram falta daquele marco: a regra BUG117 diz “fato vence pergunta”. O C6 só aplica esse encerramento direto quando há `materializada_em`, enquanto a tarefa consulta a lâmpada antes. A classificação **T** por marco aceso tem suporte; eventual exigência diferente do marco precisa ser demonstrada pelo dado. (`app/chamados/juizes.py:868`, `:880`, `:1364`, `:1368`; `app/chamados/services/tarefa_do_dia.py:244`.)

- Para `col237/2026-11-07`, a tarefa omite o motivo grave por desenho do seu mapa. Há regra escrita exigindo porta para perguntas que continuem vivas pelo C6, incluindo “todo motivo do dia sem porta”; disputa fechada não resolve por si a pergunta. A natureza exata da ação exigível desse motivo não aparece no recorte: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:225`; `app/chamados/services/declaracao_texto.py:42`; `app/docs/PROMPTS.md:1652`; `app/docs/CORTES.md:316`.)

- Nenhum resultado fornecido tem `razao='isento_ponto'`; portanto nenhum desses resultados percorreu a guarda de isenção. Não há contraexemplo comprovado de erro **por isenção** neste recorte. (`app/chamados/services/tarefa_do_dia.py:183`, `:184`, `:188`, `:189`.)

- LEI-C17-UM-JUIZ decide a favor da tarefa nos quatro campos divergentes anteriores e exige DIFF antes da troca; ela não declara que toda saída atual da tarefa já satisfaz todas as regras. O contrato do juiz é responder ações ainda exigíveis após decisões e efeitos. (`app/docs/RELATO.md:244`; `app/docs/PROMPTS.md:2007`; `app/docs/CORTES.md:426`.)

## 11/11 01:02 · sessao d87f9eac0e8c · - · - · origem=Ronald (tmux)

O caminho do vigia é único para todos os dias: `censo_alcance → dias_que_seguram → classificar_export + motivos_retencao_celula(com_dias=True) → dias.setdefault((colab, data), competência)`. O vigia exige retenção da folha, e não existência de pergunta ou chamado vivo. (`app/chamados/services/vigia_fio.py:581-587`; `app/folha/fora_da_folha.py:118-137`; `app/folha/fora_da_folha.py:95-99`)

Com célula existente, há **dois ramos possíveis de entrada**: veredito acusador que não passa pelas isenções de retenção; ou célula sem veredito, `trabalha is not None`, passada e fora do dia de demissão. A sonda não expõe veredito nem `trabalha`, portanto o ramo individual entre esses dois permanece [nao verificado]. (`app/folha/export.py:1233-1256`)

Antes de contar morto, o vigia procura porta; separa hoje/futuro; e procura decisão pendente, ausência aberta, folga contestada validada ou dia na mesa do admin. Só depois escolhe causa e incrementa `fio_morto_alcance`. (`app/chamados/services/vigia_fio.py:635-660`; `app/chamados/services/vigia_fio.py:304-315`; `app/chamados/services/vigia_fio.py:695-699`)

| Causa | Condição efetiva e referência |
|---|---|
| a | Chamado vivo da célula ou âncora viva fora da fila; como a porta já falhou, é vivo sem porta alcançável. (`app/chamados/services/vigia_fio.py:663-670`) |
| b | Pergunta viva respondida sem texto. (`app/chamados/services/vigia_fio.py:671-674`) |
| c | Pergunta viva respondida, com âncora terminal; vem depois de excluir resposta vazia. (`app/chamados/services/vigia_fio.py:673-676`) |
| d | Nenhuma pergunta viva, dia ausente da lista CxR, colaborador presente nela; existência de célula e ausência de chamado vivo decorrem dos ramos anteriores. (`app/chamados/services/vigia_fio.py:667-678`) |
| e | Restante, discriminado por detalhe: sem célula, dia na lista com outro dono, sem pergunta viva, pergunta sob terminal, sem âncora, fora do app ou outra. (`app/chamados/services/vigia_fio.py:667-694`) |

A causa **d não atribui dono CADASTRO nem prova que nada é exigível**: seu rótulo efetivo é `colab_na_lista_cxr_dia_fora`, e o teste só combina presença do colaborador com ausência do dia. (`app/chamados/services/vigia_fio.py:538-539`; `app/chamados/services/vigia_fio.py:677-678`)

A “lista” vem de `ListaDoAdmin`, que chama `escala.services.cadastro_realidade.lista`, deriva os dias com `dias_na_lista` e separadamente extrai colaboradores das linhas por posto. Só assinaturas com `data` representam dias; assinatura do vínculo não prova presença daquele dia. (`app/chamados/services/vigia_fio.py:205-218`; `app/chamados/services/vigia_fio.py:230-251`; `app/escala/services/cadastro_realidade.py:1044-1064`)

Para a classificação, os dois dias de causa **c** merecem exame próprio: a lei C6 mantém resposta com texto viva para decisão do admin, salvo decisão explícita dele sobre o dia ou concordância com a luz nos termos do juiz; esse ramo precede a absolvição pelo veredito. Portanto `dia_sem_trabalho` sozinho não permite classificá-los T. (`app/chamados/juizes.py:1341-1353`; `app/chamados/juizes.py:1372-1387`)

A tarefa, entretanto, pula cada instância por `dia_sem_trabalho` ou `lampada is True` **antes** de procurar a pergunta e sua decisão. Isso explica como pode devolver nada apesar de C6 viva, sem demonstrar por si só qual autoridade está correta naquele caso. (`app/chamados/services/tarefa_do_dia.py:241-246`; `app/chamados/services/tarefa_do_dia.py:268-283`)

`marcos_pendentes=[]` informa apenas ausência de lâmpada marcada `pendente` na ata; não informa ausência de acusação, ausência de trabalho ou ausência de obrigação. (`app/ponto/services/lastro.py:138-150`)

## 11/11 01:02 · sessao 2dc0b65e065e · - · - · origem=Ronald (tmux)

1. **Há derivação paralela da mesma chave.** Índice: motivo por `tipo == 'S'` versus demais, dia por `date.fromisoformat(marco.get('data'))`; B repete essas duas expressões. Referências: `app/chamados/services/disputa_emissao.py:1256`, `:1259`, `:1359`, `:1361`.

   Para JSON ordinário e mesmo estado do chamado, as expressões produzem o mesmo motivo/dia. O construtor usado por B não altera o valor nem o tipo em execução: `DiaDoTurno` é `NewType` e `dia_do_turno_da_celula` apenas o retorna. Referências: `app/core/dia_do_turno.py:13`; `app/ponto/turnos.py:2939`, `:2943`.

   **Não há equivalência literal para todo input/universo:** índice captura somente `TypeError/ValueError`, B captura `Exception`; índice lê os regularizáveis antes dos complementos, enquanto B recebe os sobreviventes do filtro posterior. Referências: `app/chamados/services/disputa_emissao.py:1250`, `:1257`, `:1291`, `:1294`, `:1297`, `:1300`, `:1351`, `:1362`.

   Caso concreto de índice com chave sem execução correspondente de B: intervalo regularizável já possui pergunta não materializada e `cobra_marcos_da_ata(intervalo)==False`. Ele entra no índice, pode ser complementado, e é retirado do lote por `:1300`; B não o percorre. O índice continua disponível ao complemento do genérico e redireciona seu pedido ao intervalo. Referências: `app/chamados/services/disputa_emissao.py:1250`, `:1280`, `:1291`, `:1300`, `:1351`, `:1001`.

   Caso B com chave ausente do índice sem mudanças de estado entre essas leituras: **[nao verificado]**. Mudanças produzidas por complementos/signals ou predicado temporal podem mudar a elegibilidade; reprodução ORM: **[nao verificado]**.

2. **Censo persistente de destino confirmado.**

   - Nascimento: `app/chamados/models.py:3014`, `:3017`.
   - Reuso da canônica: `app/chamados/models.py:2968`, `:2972`, `:2982`, `:3006`.
   - Reaponte: `app/chamados/services/fio.py:907`, `:909`, `:925`, `:927`.
   - Exclusão do chamado: FK `SET_NULL`, `app/chamados/models.py:3385`, `:3387`.
   - Atribuições temporárias/restauradas: `app/chamados/services/fio.py:779`, `:783`; reassociação de mesmo PK: `app/chamados/models.py:366`, `:367`.

   O próprio contrato documenta os dois escritores produtivos e a deleção de FK: `app/chamados/services/fio.py:804`, `:807`, `:814`.

3. **A c20 protege a passagem comum, não todos os pedidos que chegam aos escritores.** O override acontece em `_criar_pergunta_se_couber`; o replay conserva esse destino. Referências: `app/chamados/services/disputa_emissao.py:1001`, `:1018`, `:1029`, `:1497`.

   Pendentes chamam o gerador diretamente, sem lhe passar o índice; o gerador deriva outro destino e chama `abrir`/eventual reata. Referências: `app/chamados/services/disputa_emissao.py:1486`, `:300`, `:412`, `:503`, `:505`, `:514`, `:519`. A reata usa a outra porta escritora. Referências: `app/chamados/reconciliador.py:954`, `:1642`, `:1655`; `app/chamados/services/fio.py:925`.

4. **Contraexemplo sobrevivente na declaração, alcançável pelos atos tocados.** A declaração do chamado genérico pode casar o marco de intervalo da ata, e passa sempre `ch` como destino. Se a canônica desse intervalo está em outra DS, `_candidata_reusavel` devolve `None`; `abrir` reaproveita essa pergunta e costura em `ch`, inclusive no replay confirmado. Referências: `app/chamados/services/declaracao_texto.py:341`, `:346`, `:362`, `:365`, `:250`, `:254`; `app/chamados/services/disputa_emissao.py:791`, `:799`; `app/chamados/models.py:2982`, `:3006`.

   Admin possui o mesmo pedido de destino. Referências: `app/chamados/services/declaracao_texto.py:618`, `:650`, `:653`.

   Reprodução executada somente em memória com `Ambiente5l`: P1 em dona B2, destino intervalo2 → declaração pelo ch1 → mesma P1/B2 com destino ch1. O ambiente usa a porta double que explicitamente grava a costura; portanto prova o pedido errado conservado pelo emissor, sem certificar ORM/signals. Referências do mecanismo executado: `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`, `:104`; `app/chamados/services/disputa_emissao.py:791`, `:799`. Reprodução ORM: **[nao verificado]**.

   Os juízes dos escritores conferem colaborador/dia/estado do fio, sem preferir o chamado com marco explícito de intervalo. Assim, chamado genérico vivo do mesmo dia satisfaz essas guardas. Referências: `app/chamados/juizes.py:2185`, `:2208`, `:2217`; `app/chamados/services/fio.py:771`; `app/chamados/models.py:2972`, `:2980`.

5. **Contraexemplos solicitados da passagem comum estão cobertos e passaram em memória.** Ambas as ordens, dona aberta/fechada, replay 2x, só genérico, só intervalo, outro dia e intervalo-volta: `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:81`, `:87`, `:109`, `:112`, `:119`, `:126`, `:136`. ORM e suíte completa: **[nao verificado]**.

6. **Executei os quatro mutantes c20, sem arquivos nem banco:** todos os controles passaram 5/5; `destino_generico` e `sem_indice` morreram com cinco failures, `indice_sem_dia` e `troca_marco` com um failure, sem errors. As assertions mordem os destinos em `app/chamados/tests/test_bo_disputa_volta5l_memoria.py:91`, `:132`, `:142`; montagem fora do runner em `:157`, `:159` e rejeição de errors em `:162`.

## 11/11 01:02 · sessao 3d3d25efdc7a · - · - · origem=Ronald (tmux)

Censo dos campos e classes, com endereços da cópia:

| Campo | Quem escreve | Quem lê nos caminhos relevantes |
|---|---|---|
| `DS.motivos` | Agregador `app/chamados/services/disputa_emissao.py:575`, save `:581`; nascimentos `:685`, `:1337`, `:1605`; aparo `app/chamados/models.py:1966`; remoção `:2063`; reabertura admin `:2159`. | União/admissão `app/chamados/services/disputa_emissao.py:571`, `:606`; seed `:1312`, `:1469`; push `:1535`; foto `app/api/foto_recebida.py:61`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:50`. |
| `DS.fechada_em` | Fechamento `app/chamados/models.py:1932`, save `:1936`; reabertura admin `:2149`, save `:2161`; sistema `:2173`, save `:2177`. | Eleição/recusa/push `app/chamados/services/disputa_emissao.py:567`, `:621`, `:645`, `:846`, `:1267`, `:1536`; receivers `app/chamados/signals.py:28`, `:129`, `:153`; validação `app/chamados/services/validacao.py:214`; autorização `app/chamados/services/fio.py:600`. |
| `P.disputa` | Construção e inserção `app/chamados/models.py:3014`, `:3017`; reuso devolve a linha preservando a dona `:3008`. Exclusão da DS apaga perguntas por CASCADE `:2416`. | Encaminhamento `app/chamados/services/disputa_emissao.py:613`; candidata `app/chamados/services/declaracao_texto.py:254`; listagem/autorização `app/chamados/services/fio.py:116`, `:598`; identidade `app/chamados/models.py:3430`. |
| `P.chamado_destino` | Nascimento `app/chamados/models.py:3014`; reuso `:2982`, save `:3006`; reaponte `app/chamados/services/fio.py:925`, save `:927`; exclusão de chamado produz NULL pela FK `app/chamados/models.py:3385`. | Índice c20 `app/chamados/services/disputa_emissao.py:1001`; complemento/filtro `:1280`, `:1301`; juiz `app/chamados/juizes.py:2150`; listagem/autorização `app/chamados/services/fio.py:117`, `:598`; materialização `app/chamados/services/materializacao.py:1076`. |
| `P.motivo` | Construção/inserção `app/chamados/models.py:3014`, `:3017`; a derivação de instância lê o motivo `:3437`. | Encaminhamento `app/chamados/services/disputa_emissao.py:614`; universo do fato `app/chamados/services/fio.py:1793`; motivo para marco `app/chamados/services/declaracao_texto.py:250`; materialização `app/chamados/services/materializacao.py:1077`. |
| Trilha nova | Porta `app/chamados/services/descarte_disputa.py:4`, gravação por `evento` `:31`, agendada `:37`. | Dedup da própria porta `app/chamados/services/descarte_disputa.py:27`; contador I23 `app/chamados/management/commands/auditar_invariantes_chamados.py:81`, ligado no comando `:893`. |

O mapa acima cobre os escritores identificados e leitores relevantes aos atos auditados; censo exaustivo de todos os leitores indiretos de toda a aplicação: **[nao verificado]**.

O significado de `DS.motivos` é “cenários ativos”, enquanto `P.motivo` participa da identidade do fato juntamente com dia e instância. O seed pode aparecer sem pergunta daquele motivo; logo, as listas não têm equivalência um a um. `app/chamados/models.py:1743`, `:2427`, `:3483`; `app/chamados/services/disputa_emissao.py:1469`.

O dia civil e o dia do turno estão distinguidos pelo campo `conversa`: conversa nasce com `localdate`, e o juiz não a compara ao turno do chamado; datadas passam por `dia_do_turno_da_linha`. Pendentes derivam o dia pela batida e ignoram a data civil contextual. `app/chamados/models.py:2801`, `:2811`, `:2875`; `app/chamados/juizes.py:2188`; `app/chamados/services/disputa_emissao.py:275`. Nova divergência inclusivo/exclusivo nesses campos: **[nao verificado]**; a seleção c20 usa igualdade de chave em `app/chamados/services/disputa_emissao.py:1001`.

Classes:

- **Segundo escritor da agregação na âncora: curado.** A âncora passa `['ancora_ausente']` ao agregador; a união e o save ficam no helper. `app/chamados/services/disputa_emissao.py:1582`, `:1613`, `:575`.
- **Derivação paralela: presente na c20.** Índice e caminho B derivam separadamente motivo/dia de `marcos_faltantes`; a análise de equivalência cabe ao explorador c20. `app/chamados/services/disputa_emissao.py:1252`, `:1256`, `:1259`, `:1356`, `:1359`, `:1361`.
- **Relógio novo no delta c15–c20: não identificado.** Leituras internas legadas continuam nos atos: contexto, seleção de passado e SLA. `app/chamados/services/disputa_emissao.py:554`, `:914`, `:679`, `:1331`, `:1599`. Ausência universal de relógio fora da borda: **[nao verificado]**.
- **Trava da trilha: C→D em transação própria, depois do commit.** `app/chamados/services/descarte_disputa.py:20`, `:23`, `:37`.
- **Trava do repasse: universo reunido e revalidado antes das escritas.** `app/chamados/services/disputa_emissao.py:1188`, `:1196`, `:1198`.
- **Pendentes ainda fora da fronteira do núcleo no wrapper.** O núcleo retorna antes da chamada; o helper abre perguntas e depois agrega DS, sem atomic próprio. `app/chamados/services/disputa_emissao.py:836`, `:850`, `:300`, `:314`.
- **Replay ainda parcial para episódio ausente.** O dict fixa `batida_origem_id`, mas `_nascer` usa `valor or derivador`; `None` permite derivar outro episódio da dona. O teste F2 cobre episódio positivo, não ausência. `app/chamados/services/disputa_emissao.py:504`; `app/chamados/models.py:2866`, `:3463`; `app/chamados/tests/test_bo_disputa_volta5f_memoria.py:110`.

## 11/11 01:00 · sessao 96abcaf59a57 · FIO-E0-V4-E4B · construtor · origem=Code

**PAREI em P4, conforme sua instrução.** Não localizei autoridade única para “tem resposta” incluindo texto sem timestamp: `respondida` lê só o carimbo; a recuperação pela trilha reconhece esse meio-estado como resposta existente ([models.py:3497](chamados/models.py:3497), [disputa_emissao.py:1661](chamados/services/disputa_emissao.py:1661)).

Registrei evidências, pendências P1–P6 e comandos em [E4B_ALCANCE_ENTREGA.txt:1](E4B_ALCANCE_ENTREGA.txt:1). O RED P4 falhou por assertion; os 21 testes preexistentes passaram. A cura permanece pendente, sem certificação. [nao verificado]

## 11/11 00:59 · sessao 9151e4f862dc · BRIEF-COM-PROCEDENCIA · censo · origem=Code

As citações abaixo vêm dos anexos transcritos no pedido. A numeração física de linhas desses anexos é **[nao verificado]**; indico arquivo, seção e trecho literal. Presença de citação e autorização da decisão são contagens distintas.

| Par | Erro | ORIGINAL | REPLAY | Fonte ou omissão? |
|---|---|---|---|---|
| 5a | E1 | **Contém:** “so sai se NESTE ato nasceu (`criada`) pelo menos uma pergunta em DS ABERTA” — `orig_5a.md`, D5. | **Repete a classe “critério próprio sem aval”**, embora mude o filtro: “minha decisão de desenho é” e “Não anunciar [...] sem pergunta respondível” — `replay_5a.r`, §6. | Cita código como motivação; a procedência confirma “condicionado a pergunta respondível: **decisões minhas**”. Aval desse critério: **[nao verificado]**. |
| 5a | E2 | **Contém:** “No REPLAY nenhuma pergunta nasce (so reuso), entao a cadeia e inalcancavel la” — TRIAGEM. | **Evitou:** “O gerador não escreve somente perguntas” e “Planejamento não pode chamar esse gerador” — replay, §2. | **Por fonte de código**, com cadeia e efeitos explicitados; não omite o tema. Referências citadas no replay: `disputa_emissao.py:472–477`, `reconciliador.py:405–412`; correspondência histórica: **[nao verificado]**. |
| 5a | E3 | **Fora deste brief:** o episódio indicado é de 21:2x. Há uma formulação anterior: “DECIDIDO PELA LEI [...] + cura mais restritiva; contestavel” — decisões. | Não impõe imutabilidade genérica: “Preserve as portas [...] e prove as cascatas do save, inclusive callbacks” — contrato, canônica fechada. | Evita a proibição genérica e cita código para examinar efeitos. A atribuição específica à corte das 21:2x: **[nao verificado]**. |
| 5a | E4 | **Fora deste brief:** órfãos estão “FORA DESTA VOLTA”. | **O tema específico do seed não aparece.** | Não conto ausência do tema como correção fundamentada — `orig_5a.md`, exclusões; `replay_5a.r`; linhas **[nao verificado]**. |
| 5a | E5 | **Fora deste brief;** ver registro de 5h abaixo. | Não atribui identificador novo ao contador. | `replay_5a.r`; conferência dos IDs existentes **[nao verificado]**. |
| 5f | E1 | **Fora do episódio das 19:27.** Contém outra decisão: “notificar de novo seria push duplicado do mesmo fato” — decisões. | **Evitou o filtro original:** “Não substituir essa política por ‘perguntas criadas nesta chamada’” — D6. | **Por fonte citada:** `opus5d.txt`, item 3, e endereços do código. Conteúdo integral do Opus e aval das decisões técnicas adicionais: **[nao verificado]**. | [nao verificado]
| 5f | E2 | **Fora deste brief.** | A conclusão específica “inalcançável” não reaparece. | Tema específico ausente — `orig_5f.md`; `replay_5f.r`; linhas **[nao verificado]**. |
| 5f | E3 | **Fora do episódio das 21:2x.** O texto posterior diz: “a dona FECHADA segue pergunta de lei aberta: nao mexer” — decisões. | **Evitou decidir:** “É permitido agregar o motivo mantendo-a fechada, ou qual saída deve valer?” — pergunta de lei. | **Por suspensão expressa da decisão**, acompanhada de referências; não apresenta uma solução como autorizada. Linhas dos anexos **[nao verificado]**. |
| 5f | E4 | **Contém a formulação ampla:** “Inclui o seed 'orfao_14h': a local so recebe motivo que tem pergunta LOCAL neste ato ou que ja tinha antes” — F1. | **Restringe a formulação:** “Eliminar o seed indevido [...] **no repasse com ata**”; D5 preserva as saídas sem ata e com ata completa. | **Por delimitação e fontes de código/testes**, não por omissão. O aval apenas chama o erro de “seed [...] largo”; o efeito concreto condenado não está detalhado: **[nao verificado]**. | [nao verificado]
| 5f | E5 | **Fora deste brief;** ver 5h abaixo. | Não atribui identificador novo ao contador. | `replay_5f.r`; conferência dos IDs existentes **[nao verificado]**. |
| 5g | E1 | **Fora do episódio das 19:27.** | Rejeita o filtro original: “Não troque tudo por perguntas criadas/localmente retornadas”. Porém acrescenta: “Minha decisão é aplicar o critério de agregação do wrapper à dona resultante” — D6. | Cita fonte para preservar a política; o acréscimo é declarado próprio. Aval desse acréscimo: **[nao verificado]**. |
| 5g | E2 | **Fora deste brief.** | O tema específico da triagem “inalcançável” não aparece. | `orig_5g.md`; `replay_5g.r`; linhas **[nao verificado]**. |
| 5g | E3 | **Fora do episódio das 21:2x.** Agora diz “dona fechada continua sem save”, com cabeçalho remetendo ao aval das duas leis. | Mantém o comportamento e identifica “Fonte decisória: `aval_duas_leis_1110.txt`, item 2” — D3. | **Evita a atribuição à corte anterior por citar outro aval.** O conteúdo integral e o alcance desse aval posterior: **[nao verificado]**. |
| 5g | E4 | **Fora do episódio das 22:1x;** não trata do seed. | O tema aparece: “nenhum seed indevido”, mas também “Cobrir também ausência/recusa sem admissão de motivo” — F1; D3 preserva HX/h2. | Há fontes e exceções explícitas; afirmar que isso eliminou exatamente o excesso chamado “largo” é **[nao verificado]**. |
| 5g | E5 | **Contém a ordem de alocação:** “G2. [...] **I8** 'motivo em dona FECHADA descartado'”. | Pede contador chamado `motivo_em_dona_fechada`, sem escolher novo identificador numérico. | Não cria o novo `I8`; execução de uma conferência de IDs: **[nao verificado]**. |

Sobre **E5**, o `orig_5h.md` **registra a repetição e manda corrigi-la**: “registra o contador novo como `I8`, mas `I8` ja e o fio solto” e “Renomear o NOVO para `I23`” — item 2b, linha **[nao verificado]**. A cópia confirma um `I8` de fio solto em [auditar_invariantes_chamados.py:860](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/management/commands/auditar_invariantes_chamados.py:860). **Nenhum dos três replays atribui um novo ID numérico sem conferência demonstrada**; a própria conferência não está demonstrada nos replays **[nao verificado]**.

Para a contagem, usei **unidade decisória**, pois contar quebras gráficas daria denominadores diferentes: cada parágrafo normativo, item normativo ou linha normativa de tabela conta uma unidade; continuações ficam juntas. Incluí ordens no CENSO, itens-pai com exigência própria e subitens; excluí títulos, metadados, descrições factuais, comandos shell e os resumos/tabelas finais de procedência dos replays. A numeração física dessas unidades nos anexos é **[nao verificado]**.

“Com fonte” significa citação de aval/lei identificada ou arquivo e linha **na unidade**, incluindo abreviações `:N` resolvidas pelo CENSO. Não significa que a fonte autoriza a decisão — caso do push no replay 5a, explicitamente declarado próprio (`replay_5a.r`, §6; linha **[nao verificado]**). [nao verificado]

| Par | Lado | Unidades decisórias | Com citação | Sem citação |
|---|---|---:|---:|---:|
| 5a | Original | 34 | 14 | 20 |
| 5a | Replay | 54 | 54 | 0 |
| 5f | Original | 12 | 6 | 6 |
| 5f | Replay | 46 | 45 | 1 |
| 5g | Original | 7 | 1 | 6 |
| 5g | Replay | 50 | 46 | 4 |
| **Total** | **Originais** | **53** | **21** | **32** |
| **Total** | **Replays** | **150** | **145** | **5** |

Contagens derivadas dos seis anexos transcritos, conforme o critério acima; localização física arquivo:linha **[nao verificado]**.

As cinco unidades dos replays sem fonte direta são: “Mutantes obrigatórios, aplicados à implementação produtiva” no 5f; e, no 5g, “Registre na RAIZ”, “Confira os oito casos”, “Execute os controles” e “O alvo ORM F1 [...] deve ser executado”. Elas remetem a relatos, referências internas ou aval não identificado naquela unidade (`replay_5f.r`, D7; `replay_5g.r`, D3/D7; linhas **[nao verificado]**). [nao verificado]

## 11/11 00:57 · sessao ec1fa2c5c443 · - · - · origem=Ronald (tmux)

Pelo critério integral combinado: **orig_5f = 13 unidades, 6 com fonte / 7 sem; replay_5f = 46 unidades, 45 com / 1 sem**. Unidades são parágrafos, itens e linhas de tabela normativos; não são linhas físicas. Fontes abreviadas com `:n` resolvidas pelo CENSO contam. Resumos e procedência final do replay ficam fora para evitar duplicação.

Inventário de **orig_5f**, com S/N para fonte direta:

| Nº | Âncora literal | Fonte |
|---:|---|:---:|
| 1 | “escopo = avais anexos” | N |
| 2 | “descartado (vai ao opus): push para novos motivos na dona estrangeira (:613)” | S | [nao verificado]
| 3 | “descartado (vai ao opus): perguntar_entradas_pendentes fora do atomic do nucleo (:830)” | S |
| 4 | “a dona FECHADA segue pergunta de lei aberta: nao mexer” | N |
| 5 | F1: “o motivo cuja pergunta foi REUSADA de outra disputa NAO entra na local” | S, `:1270-1460` contextual |
| 6 | F2: “o coletor de reuso conserva o EPISODIO” | N |
| 7 | F3: “com replay divergente, o chamado cuja costura foi desfeita NAO e analisado” | N |
| 8 | F4: “Ancora (:1564): os motivos da DS existente passam pelo _agregar_motivos_na_dona” | S |
| 9 | F5: “a forma publica exige pergunta” | S, `:764` contextual |
| 10 | F6: “nasce_sem_pergunta e replay_insere ancoram em sitio UNICO” | N |
| 11 | F7: “apague a afirmacao falsa (nome e asserts)” | N |
| 12 | F8: “PROIBIDO” | S, referências dos protegidos |
| 13 | “Feche com: arquivos tocados, por item o RED” | N |

A primeira unidade é limítrofe: contém o contrato “escopo = avais anexos”. Se a tratar como fato introdutório, excluir resulta em **12 = 6 S / 6 N**.

Inventário de **replay_5f**:

| Nº | Seção e âncora literal | Fonte |
|---:|---|:---:|
| 1 | D1: “NÃO LIBERA push nem pouso desta obra” | S | [nao verificado]
| 2 | D1: “Preserve as exceções expressamente decididas para HX” | S |
| 3 | D1: “Não tocar nas sete disputas existentes” | S |
| 4 | D2: “A próxima volta precisa eliminar as divergências” | S |
| 5 | D2: “Exigir um contrato comum de nascimento” | S |
| 6 | D2: “Fechar a API pública de declaração” | S |
| 7 | D3: “Adotar a direção pedida no aval da matriz” | S |
| 8 | D3: “começar o encaixe no serviço” | S |
| 9 | D3: “Conservar a identidade completa do fato” | S |
| 10 | D3: “Preservar a ordem global C → D → Ch → P” | S |
| 11 | D4: “Eliminar o seed indevido de `orfao_14h`” | S |
| 12 | D4: “Não converter isso em exclusividade global” | S |
| 13 | D4: “deixar o cartório ressuscitar e reabrir a dona” | S |
| 14 | D4: “Preservar os asserts atuais de reabertura e nota” | S |
| 15 | D5, estrangeiras: “A nova local não nasce” | S |
| 16 | D5, sem ata: “Continua nascendo aberta” | S |
| 17 | D5, ata completa: “preservar nascimento seguido de fechamento vazio” | S |
| 18 | D5: “Manter I7a como DS aberta sem nenhuma linha de pergunta” | S |
| 19 | D6: “Não substituir essa política” | S |
| 20 | D6: “O resultado da agregação precisa conservar, por dona” | S |
| 21 | D6: “Preservar declaração sem push” | S | [nao verificado]
| 22 | D6: “retirar o efeito externo de push de dentro da transação” | S | [nao verificado]
| 23 | D7: “Entregar a matriz completa” | S |
| 24 | D7, a: “Pai/DS/P persistem juntos” | S |
| 25 | D7, b: “Sem nascimento local vazio” | S |
| 26 | D7, c: “Separar o caso com reabertura pelo cartório” | S |
| 27 | D7, d: “Resolver cada fato e dona sem reentrada” | S |
| 28 | D7, e: “Batida anterior preservada” | S |
| 29 | D7, f: “protegida acusa I7a=0 e I7b=1” | S |
| 30 | D7, g: “Não restaurar o helper vazio removido” | S |
| 31 | D7, h: “Cobrir as três saídas de D5” | S |
| 32 | D7, i: “Distinguir fatos locais legítimos” | S |
| 33 | D7, j: “Destinatária, motivos, `nova`, repetição” | S |
| 34 | D7: “Acrescentar provas dos dois consumidores de declaração” | S |
| 35 | D7: “Mutantes obrigatórios, aplicados à implementação produtiva” | **N** | [nao verificado]
| 36 | D7, mutante: “Desligar a proteção transacional” | S |
| 37 | D7, mutante: “Trocar o episódio BATIDA pelo contexto da dona” | S |
| 38 | D7, mutante: “Desligar a trava real do repasse” | S |
| 39 | D7: “Controle verde seguido de falha comportamental por assertion é obrigatório” | S | [nao verificado]
| 40 | D7: “Não chamar de paridade histórica” | S |
| 41 | D8: “Não afrouxar S127” | S |
| 42 | D8: “Preservar a fixture já corrigida” | S |
| 43 | D8: “Pronto exige: matriz completa” | S |
| 44 | D8: “Executar as famílias memória pertinentes” | S |
| 45 | CENSO: “Não restaurá-lo para satisfazer uma matriz histórica” | S |
| 46 | Após pergunta de lei: “Não inventar reabertura”; “Registrar a linha”; “manter a liberação bloqueada” | S |

A unidade 35 inclui a condição normativa de aplicar mutantes à implementação produtiva, separada dos três filhos. Se for excluída como simples título, resulta em **45 = 45 S / 0 N**.

A pergunta residual em si — “É permitido agregar o motivo mantendo-a fechada, ou qual saída deve valer?” — não fixa uma decisão e não entra; seu parágrafo seguinte entra como unidade 46. Não produzi numeração física dos anexos: **[nao verificado]**.

## 11/11 00:57 · sessao e8000ca1facf · - · - · origem=Ronald (tmux)

Censo integral solicitado:

| Brief | Unidades decisórias | Com citação | Sem citação |
|---|---:|---:|---:|
| `orig_5a.md` | 34 | 14 | 20 |
| `replay_5a.r` | 54 | 54 | 0 |

Unidade: parágrafo normativo, item de lista ou linha de tabela normativa; continuação gráfica permanece na unidade. Em D3 contei abertura, (i) e (ii); em D7, abertura e (a)–(h). Incluí ordens explícitas dentro do CENSO. Excluí títulos, descrições puramente factuais, comandos shell e resumos/tabela final de procedência duplicados. Os anexos são transcrições do pedido, sem correspondência local de linhas conferida [nao verificado].

`orig_5a.md`, unidades com citação: lei inicial; justificativa da volta 5; D2; D3(ii); D6; abertura D7; TRIAGEM; quatro escritores do CENSO que ordenam SAI/NAO tocar/so LER; leitor da canônica “so chamar”; primeiras duas decisões finais.

`orig_5a.md`, unidades sem citação: D1; abertura D3; D3(i); D4; D5; testes (a)–(h); FORA DESTA VOLTA; PARE E RELATE; terceira decisão final; NAO tocar final; Rodar; PERGUNTA; ENTREGA. [nao verificado]

Todas as 54 unidades decisórias de `replay_5a.r` possuem referência direta de código ou aval nomeado; enumeração completa enviada na mensagem anterior. Isso mede **presença de citação**, não autorização: o critério de push é expressamente “minha decisão de desenho”, embora cite código (`replay_5a.r`, §6; anexo transcrito). Assim, S54/N0 não sustenta que o replay evitou E1. [nao verificado]

## 11/11 00:57 · sessao 313a7f40ade9 · - · - · origem=Ronald (tmux)

Pelo critério solicitado, **orig_5g = 7 unidades decisórias: 1 com fonte, 6 sem; replay_5g = 50: 46 com fonte, 4 sem**. Referências abaixo são às seções dos anexos transcritos; numeração física de arquivo:linha desses anexos [nao verificado].

Fonte significa citação na própria unidade de aval/lei nomeado ou `arquivo:linha`, inclusive endereço abreviado resolvível. Citar não significa validar a fonte externa [nao verificado].

| # | Original 5g — âncora literal | Fonte |
|---:|---|:---:|
| 1 | “FollowupChamado nao tem campo tipo [...] a trilha e LogAuditoria” | N |
| 2 | “comportamento NAO muda: dona fechada continua sem save” | N |
| 3 | G1 “grave UMA linha por core.observ.evento” | N |
| 4 | G2 “Contador no vigia [...] I8” | N |
| 5 | G3 “Casos que mordem” | N |
| 6 | G4 “PROIBIDO” — inclui endereços dos testes protegidos | S | [nao verificado]
| 7 | “Feche com: arquivos tocados” | N |

Inventário integral do replay:

| # | Replay 5g — âncora/seção | Fonte |
|---:|---|:---:|
| 1 | D0 “Primeiro confira o patch da 14” | S |
| 2 | D1 “Não volte à fase de ‘somente censo’” | S |
| 3 | “Não altere as sete disputas existentes” | S |
| 4 | D2.1 “pai, disputa e pergunta pertencem ao mesmo ato” | S |
| 5 | D2.2 “a FK da pergunta continua apontando” | S |
| 6 | D2.3 “congele episódio, dia do turno” | S |
| 7 | D2.4 “retorno com criada=True [...] não autoriza análise” | S |
| 8 | D2.5 “Agregador único de admissão” | S |
| 9 | D2.6 “exija pergunta no contrato público” | S |
| 10 | D3 “Sem ata [...] Com ata [...] h2” | S |
| 11 | “Registre na RAIZ-DA-DISPUTA F1 a alternativa” | **N** |
| 12 | “Dona fechada: mantenha o comportamento” | S |
| 13 | “Preserve a reabertura que já ocorre” | S |
| 14 | D4 “Preserve I7a [...] I7b” | S |
| 15 | “declare unidade de contagem, escritor da trilha” | S |
| 16 | D5 “Inclua pendentes na unidade transacional” | S |
| 17 | “Complete a avaliação exigida de resolver antes” | S |
| 18 | “A matriz deve concluir essa avaliação” | S |
| 19 | D6 “Mantenha os critérios históricos” | S |
| 20 | “O ato deve conservar os deltas por dona” | S |
| 21 | “Garanta que efeitos de um ato desfeito não notifiquem” | S |
| 22 | D7 “Confira os oito casos novos relatados pela 14” | **N** |
| 23 | Tabela F1 “nenhum seed indevido” | S |
| 24 | F2 “conservar identidade e dia” | S |
| 25 | F3 “nenhuma análise decorrente do fato recusado” | S |
| 26 | F4 “Mutante restaurando união/save próprios” | S |
| 27 | F5 “Chamada pública sem pergunta falha” | S |
| 28 | F6 “Mutação montada uma vez” | S |
| 29 | F7 “Paridade histórica exige fonte histórica distinta” | S |
| 30 | “Execute os controles e os mutantes relatados abaixo” | **N** |
| 31 | “Acrescente provas que removam a trilha” | S |
| 32 | “Exija ORM para o lote misto F1” | S |
| 33 | “O alvo ORM F1 [...] deve ser executado” | **N** |
| 34 | “Preserve os protegidos HX” | S |
| 35 | D8 “Entregue patch revisável da 14” | S |
| 36 | “Não libera pouso nem push agora” | S | [nao verificado]
| 37 | “O ! de PUSH-E4A2 [...] não é autorização” | S | [nao verificado]
| 38 | Após CENSO: “o comentário antigo [...] não deve ser tratado como estado atual” | S |
| 39 | “A matriz final deve preencher as seis colunas” | S |
| 40 | Matriz a “Nascer atomicamente se houver pergunta” | S |
| 41 | Matriz b “Agregar na dona” | S |
| 42 | Matriz c “Preservar comportamento; trilhar descarte” | S |
| 43 | Matriz d “Roteamento sem reentrada” | S |
| 44 | Matriz e “Desfazer emissão” | S |
| 45 | Matriz f “Abster-se e acender luz” | S |
| 46 | Matriz g “Não ressuscitar helper removido” | S |
| 47 | Matriz h “Lote misto e repasse” | S |
| 48 | Matriz h2 “Nascer e fechar vazia” | S |
| 49 | Matriz i “Distinguir duplicação de dias legítimos” | S |
| 50 | Matriz j “Declarar dona, motivos e momento” | S |

As quatro unidades N citam apenas relato/anexo sem endereço, referência interna ou nenhum fundamento direto. A unidade 11 fala em “relato do aval”, sem identificar qual aval dentro daquele item; a unidade 22 remete aos endereços de D0, indiretamente. [nao verificado]

Incluí as onze linhas da matriz final porque impõem operação/persistido exigido. Excluí fatos descritivos do CENSO, comandos shell, títulos, o resumo “Decisões do censo” e a tabela final de procedência. Se a matriz normativa também for tratada como repetição e excluída, o replay fica **39 = 35 com / 4 sem**. [nao verificado]

## 11/11 00:55 · sessao ec1fa2c5c443 · - · - · origem=Ronald (tmux)

Há três unidades distintas; não convém misturá-las.

**1. Só os blocos literalmente intitulados `decisoes:` / `Decisões`:**

| Anexo | Itens/linhas | Fonte direta | Sem fonte direta |
|---|---:|---:|---:|
| orig_5a | 3 | 2 | 1 |
| orig_5f | 3 | 0 | 3 |
| orig_5g | 2 | 0 | 2 |
| replay_5a | 1 parágrafo-resumo | 0 | 1 |
| replay_5f | 1 parágrafo-resumo | 1 | 0 |
| replay_5g | 1 parágrafo-resumo | 1 | 0 |

Critério estrito: fonte diretamente identificável na unidade — aval nomeado, lei nomeada ou `arquivo:linha`. Em orig_5f, os dois primeiros itens têm `(:613)` e `(:830)`/`6c0517eb9:685`; se aceitar endereço abreviado resolvido pelo contexto, vira **2 com / 1 sem**, em vez de 0/3. O resumo de replay_5a diz apenas “Fundamentação nos itens 1–10 deste brief e respectivas fontes”: referência indireta, sem fonte direta.

**2. Tabelas/listas finais de procedência dos replays, incluindo a linha CENSO:**

| Replay | Entradas | Fonte direta | Sem fonte direta |
|---|---:|---:|---:|
| 5a | 11 | 9 | 2 |
| 5f | 9 | 9 | 0 |
| 5g | 10 | 10 | 0 |

As duas sem fonte direta de replay_5a são:

- **8:** “Selos exigidos nos avais [...] testes existentes citados no item 8”; `entrega4.txt` aparece sem linha, com referência indireta ao corpo. [nao verificado]
- **CENSO:** “Escritores/leitores conferidos nos arquivos citados na tabela”; menciona `matriz1.r` e `matriz2.r` sem linha.

Aceitando fontes indiretas recuperadas no corpo, replay_5a passa a 11/11. A linha D2 da procedência de replay_5g identifica o “Aval de duas respostas”, embora sem nome de arquivo.

**3. Todo comando normativo do corpo:** os números acima **não** contam esse universo. Há decisões fora dos blocos explícitos, fontes que aparecem ao fim de parágrafos e resumos duplicando decisões anteriores. Não apresentaria nenhum dos denominadores anteriores como censo integral de todas as decisões.

Recomendação: declarar o critério escolhido e apresentar a contagem de blocos explícitos mais a procedência separada; contar procedência como se fosse todo o corpo favorece artificialmente o formato dos replays. Citação declarada não significa fonte validada. Os anexos transcritos não receberam numeração física de linhas nesta exploração: **[nao verificado]**.

## 11/11 00:55 · sessao e8000ca1facf · - · - · origem=Ronald (tmux)

Leitura dos anexos, sem conferir autenticidade externa dos avais citados [nao verificado]:

| Erro | Original | Replay |
|---|---|---|
| E1 / 5a | Contém o critério introduzido: “so sai se NESTE ato nasceu (`criada`) pelo menos uma pergunta em DS ABERTA” (`orig_5a.md`, D5; anexo transcrito). | Não repete o filtro literal `criada`, mas **repete a classe “critério novo sem aval”**: “minha decisão de desenho é” e “Não anunciar [...] sem pergunta respondível”. A procedência confirma: “Push por dona, após commit e condicionado a pergunta respondível: decisões minhas” (`replay_5a.r`, §6 e procedência 6; anexos transcritos). Citação de código demonstra motivação, não aval para o critério. | [nao verificado]
| E2 / 5a | Contém: “No REPLAY nenhuma pergunta nasce (so reuso), entao a cadeia e inalcancavel la” (`orig_5a.md`, TRIAGEM; anexo transcrito). | Evita por examinar a cadeia e citar arquivos: “O gerador não escreve somente perguntas [...] pode chamar reatamento”; “Planejamento não pode chamar esse gerador” (`replay_5a.r`, §2; anexo transcrito). Não omite o tema. |
| E3 / 5a | O episódio de 21:2x é posterior: **fora deste brief**, datado 19:29 no pedido. Entretanto, existe a formulação precursora: “dona FECHADA: nada salvo na DS (DECIDIDO PELA LEI [...] + cura mais restritiva; contestavel)” (`orig_5a.md`, decisões; anexo transcrito). | Não impõe “nenhum save”: “Preserve as portas [...] e prove as cascatas do save, inclusive callbacks” (`replay_5a.r`, contrato, canônica fechada; anexo transcrito). Evita a decisão de imutabilidade; mantém o tema com fontes de código. |
| E4 / 5a | **Fora deste brief**, anterior a 22:1x; órfãos estão explicitamente “FORA DESTA VOLTA” (`orig_5a.md`; anexo transcrito). | Não trata especificamente do seed `orfao_14h`; não há decisão desse seed identificada no replay transcrito. |
| E1 / 5f | Episódio das 19:27: **fora deste brief**. Há outra decisão sobre push: “notificar de novo seria push duplicado do mesmo fato” (`orig_5f.md`, decisões; anexo transcrito). Não equivale a prova de que toda pergunta preexistente recebeu notificação [nao verificado]. | Rejeita explicitamente a política criada nos briefs: “Não substituir essa política por ‘perguntas criadas nesta chamada’”; cita `opus5d.txt`, item 3 (`replay_5f.r`, D6; anexo transcrito). Preserva o tema com fonte alegada, cujo conteúdo integral não foi fornecido [nao verificado]. | [nao verificado]
| E2 / 5f | **Fora deste brief**; não contém a conclusão “inalcançável” identificada em 5a (`orig_5f.md`, anexo transcrito). | O tema específico da triagem “inalcançável” não reaparece (`replay_5f.r`, anexo transcrito). |
| E3 / 5f | Episódio de 21:2x: **fora deste brief**. Texto posterior não atribui a imutabilidade à corte: “a dona FECHADA segue pergunta de lei aberta: nao mexer” (`orig_5f.md`, decisões; anexo transcrito). | Evita decidir: “É permitido agregar o motivo mantendo-a fechada, ou qual saída deve valer?”; “Não inventar reabertura nem declarar uma exceção silenciosa ao aval” (`replay_5f.r`, pergunta de lei; anexo transcrito). É suspensão expressa da decisão, apoiada em referências; não solução concedida por fonte. |
| E4 / 5f | Contém ordem geral sobre o seed: “Inclui o seed 'orfao_14h': a local so recebe motivo que tem pergunta LOCAL neste ato ou que ja tinha antes” (`orig_5f.md`, F1; anexo transcrito). O rótulo “largo” não define sozinho qual alcance foi condenado [nao verificado]. | Restringe o caso: “Eliminar o seed indevido [...] **no repasse com ata**”; exige lote estrangeiro + intervalo local e preserva sem ata em D5 (`replay_5f.r`, D4–D5; anexo transcrito). Evita a generalização textual por delimitação e referências de código/testes; não por omissão. | [nao verificado]
| E1 / 5g | **Fora deste brief**; não introduz política de elegibilidade de push, apenas preserva “sem push” para dona fechada (`orig_5g.md`, decisões; anexo transcrito). | Preserva critérios históricos citando `opus5d.txt`, item 3, mas acrescenta decisão própria sobre delta da dona: “Minha decisão é aplicar o critério de agregação do wrapper à dona resultante” (`replay_5g.r`, D6; anexo transcrito). Não chamar isso integralmente de política avalizada [nao verificado]. | [nao verificado]
| E2 / 5g | **Fora deste brief**; tema específico ausente (`orig_5g.md`, anexo transcrito). | Tema específico ausente (`replay_5g.r`, anexo transcrito). |
| E3 / 5g | Episódio de 21:2x: **fora deste brief**. Agora há atribuição explícita ao aval posterior: cabeçalho cita `aval_duas_leis_1110.txt`, item 2, e decisões dizem “dona fechada continua sem save” (`orig_5g.md`; anexo transcrito). Conteúdo integral desse aval não fornecido [nao verificado]. | Mantém o comportamento e cita o mesmo aval posterior: “Fonte decisória: `aval_duas_leis_1110.txt`, item 2: [nao verificado]” (`replay_5g.r`, D3; anexo transcrito). Evita atribuição inventada à corte anterior; autenticidade/alcance do aval posterior [nao verificado]. |
| E4 / 5g | Episódio de 22:1x: **fora deste brief**; seed não tratado (`orig_5g.md`, anexo transcrito). | Retoma a correção: “nenhum seed indevido”; também manda “Cobrir [...] ausência/recusa sem admissão de motivo”, enquanto preserva HX/h2 em D3 (`replay_5g.r`, D7/F1 e D3; anexo transcrito). Repetição do excesso concreto chamado “largo”: [nao verificado]. |

E5: `orig_5h.md` **registra** a colisão, em vez de ordená-la: “registra o contador novo como `I8`, mas `I8` ja e o fio solto (:946)” e manda renomear para `I23` (`orig_5h.md`, 2b; anexo transcrito). A ordem que introduz `I8` está em `orig_5g.md`, G2. Nenhum replay transcrito aloca um novo identificador numérico de invariante: `replay_5g.r` usa o nome `motivo_em_dona_fechada`, deixando o identificador por escolher. Logo, não há replay criando ID novo sem conferir; execução de conferência dos IDs existentes [nao verificado].

Cautela para a consolidação: os anexos só estão transcritos no pedido. Não inventar `arquivo:linha` desses anexos; ou numerá-los explicitamente como linhas da transcrição, ou conservar `[nao verificado]` junto das referências de seção.

## 11/11 00:53 · sessao 79381fcc4cfc · FIO-E0-V4-E4B · verificador · origem=Code

**Há bloqueios de alcance, proteção de exportadas, relógio e idempotência.** A composição usa as autoridades existentes, mas a execução não satisfaz integralmente o contrato pedido. Referências: `app/chamados/services/disputa_emissao.py:1793`, `app/chamados/services/disputa_emissao.py:1875`, `app/chamados/models.py:1394`, `app/chamados/models.py:1566`.

1. **Quem reabre e quando**

   A âncora está correta: `chamado_destino` vence mesmo terminal; sem destino, vale o chamado-pai. A cura consulta `pergunta_viva`, `chamado_que_lista` e `VIVOS`; não reproduz o julgamento factual do C6. Referências: `app/chamados/services/fio.py:116`, `app/chamados/services/disputa_emissao.py:1793`.

   Com `respondida_em` preenchida, a cura nova não atua; a respondida segue pelo HX, que consulta `irmas_respondidas_sem_consumo` e pode reabrir o pai. Com C6 falso, a cura nova também não atua. Referências: `app/chamados/services/disputa_emissao.py:1797`, `app/chamados/services/disputa_emissao.py:1929`, `app/chamados/juizes.py:1949`.

   **Falha concreta de cobertura:** a chamada nova no S143 exclui perguntas de disputa fechada. O C6 não exige disputa aberta, e o fio administrativo explicitamente lista vivas em disputas fechadas. Portanto, há estado em que a pergunta continua viva e sua âncora terminal não recebe a cura. Referências: `app/chamados/services/disputa_emissao.py:1875`, `app/chamados/juizes.py:1324`, `app/chamados/services/fio.py:212`.

   Esse caso foi reproduzido em memória alterando apenas `fechada_em` da disputa na fixture: C6 verdadeiro, destino fechado, nenhuma reabertura ou nota. O corpo e a fixture utilizados estão em `app/chamados/tests/test_e4b_classe_e_memoria.py:100` e `app/chamados/tests/test_e4b_classe_e_memoria.py:165`. Existência desse estado nas 131: [nao verificado].

   **Resposta textual sem timestamp:** a cura verifica apenas `respondida_em`; com texto existente e timestamp nulo, reabre e escreve “sem resposta”. Esse meio-estado é reconhecido pela porta que recupera o timestamp pela trilha. A nota nova pode, portanto, descrever falsamente ausência de texto. Referências: `app/chamados/services/disputa_emissao.py:1797`, `app/chamados/services/disputa_emissao.py:1810`, `app/chamados/services/disputa_emissao.py:1661`.

   A sonda fornecida informa 131 perguntas sustentadas por célula acusadora e nenhum defeito C6 demonstrado; os valores de produção e `validada_em` não foram conferidos independentemente. [nao verificado] O retorno por célula acusadora permanece em `app/chamados/juizes.py:1372`; a guarda de validação o antecede em `app/chamados/juizes.py:1325`.

2. **Porta, escritor e trilha**

   A cura passa por `credor.reabrir()`, que chama `_gravar_estado`; não há segundo escritor de status nessa adição. O núcleo limpa `resolvido_em` e `via_encerramento`, restaura urgência e registra a transição. Referências: `app/chamados/services/disputa_emissao.py:1808`, `app/chamados/models.py:1442`, `app/chamados/models.py:247`, `app/chamados/models.py:292`.

   A trilha registra antes/depois do status; a causa específica fica na nota. O ator vem da requisição ou do processo `sistema:<comando>`, e o instante é carimbado automaticamente. Entretanto, a auditoria é **best effort**, podendo falhar sem impedir o estado. Referências: `app/chamados/models.py:292`, `app/chamados/services/disputa_emissao.py:1809`, `app/core/observ.py:94`, `app/core/observ.py:108`, `app/core/models.py:307`.

   A recusa fica registrada sem fallback explícito na cura. Porém, `reabrir()` recusado por `irmao_vivo` pode migrar perguntas para o irmão; “recusou” não significa “não escreveu”. Referências: `app/chamados/services/disputa_emissao.py:1813`, `app/chamados/models.py:1411`, `app/chamados/services/fio.py:2174`.

   Os dois tipos novos não têm consumidor nominal específico identificado fora da cura. `tipo` não é coluna persistida: participa da chave quando há `objeto_ref`. A nota de sucesso, sem referência, conserva texto e autoria; a recusa ganha chave. Referências: `app/chamados/services/disputa_emissao.py:1811`, `app/chamados/services/disputa_emissao.py:1817`, `app/chamados/services/nota.py:88`, `app/chamados/services/nota.py:95`, `app/chamados/models.py:1668`.

   Leitores genéricos incluem fio, histórico, Central, detalhes e templates; o SLA considera resposta de DP, portanto a nota de sistema não reinicia sua referência. Referências: `app/chamados/services/fio.py:174`, `app/chamados/services/historico.py:80`, `app/chamados/services/painel.py:362`, `app/chamados/views.py:352`, `app/templates/chamados/partials/modal_fio.html:461`, `app/chamados/services/sla.py:21`.

   **Registro à parte pedido pelo aval:** a ação administrativa aceita mudança de status com conteúdo vazio; `fechar()` só acrescenta motivo textual quando recebido. Referências: `app/chamados/services/acoes_chamado.py:286`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/models.py:452`.

3. **Idempotência e laços**

   No sucesso sequencial, a segunda passagem encontra o credor vivo e não reabre nem acrescenta nota. Isso sustenta uma transição e uma nota para esse caso; não certifica concorrência nem todas as cascatas. Referências: `app/chamados/services/disputa_emissao.py:1803`, `app/chamados/tests/test_e4b_classe_e_memoria.py:252`, `app/chamados/tests/test_b2d_resposta_visivel.py:104`.

   **Falha concreta nas recusas:** a cura consulta a recusa e chama `reabrir()` mesmo assim. Para `admin_sem_fato_novo` e `premissa_regularizada`, cada tentativa escreve nova `reabertura_recusada`. Duas passadas mantêm o estado e deduplicam a nota, mas duplicam a trilha da mesma recusa. Referências: `app/chamados/services/disputa_emissao.py:1805`, `app/chamados/models.py:1394`, `app/chamados/models.py:1400`, `app/chamados/services/nota.py:95`.

   **Não há fechamento pelo */15 atual:** o cron chama `reconciliar_chamados --alarme`; o comando consulta `reconciliaria()` e imprime/alarma. Referências: `app/config/crons.py:395`, `app/chamados/management/commands/reconciliar_chamados.py:38`, `app/chamados/management/commands/reconciliar_chamados.py:79`, `app/chamados/management/commands/reconciliar_chamados.py:118`.

   **Há caminho condicional de ping-pong por evento**, com esta sequência:

   - A cura reabre um agregador com pergunta viva sem resposta. `app/chamados/services/disputa_emissao.py:1879`.
   - Batida, outbox ou morte de filho dispara reconciliação. `app/chamados/reconciliador.py:1441`, `app/chamados/signals.py:363`.
   - A premissa de filhos vivos está morta; a retenção HX não reconhece pergunta sem resposta. `app/chamados/reconciliador.py:292`, `app/chamados/reconciliador.py:446`, `app/chamados/juizes.py:1951`.
   - Em exportada/indeterminada, F1 não retém; `reconciliar()` pode fechar. `app/chamados/models.py:364`, `app/chamados/reconciliador.py:523`.
   - S143 volta a pedir reabertura. `app/chamados/signals.py:115`.

   Isso é um encadeamento condicional existente no código; um ciclo completo demonstrado em ORM e sua ocorrência nas 131 são [nao verificado]. Não é universal: a porta pode recusar chamado de furo com premissa regularizada. `app/chamados/models.py:1348`.

   `arquivar_prescritos` só silencia e está desligado. Se reativado, pode silenciar credor antigo recém-reaberto porque seleciona nascimento/resgate, não o instante da reabertura. Referências: `app/chamados/management/commands/arquivar_prescritos.py:53`, `app/chamados/management/commands/arquivar_prescritos.py:94`, `app/config/crons.py:1453`.

4. **Backfill, exportadas, universo e travas**

   Sem competência, o comando lista; com competência e sem `--aplicar`, executa em transação e desfaz. `--aplicar` exige competência explícita. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:232`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:238`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:308`.

   O complemento exportado planeja com `executar=False`; exportada e indeterminada ficam fora dos executáveis, e o dia-alvo é conferido novamente antes da execução. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:163`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:262`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:293`.

   **Isso não garante “exportada nunca escrita”.** A reabertura relavra todas as perguntas não validadas daquele destino e pode ressuscitar perguntas materializadas sem texto, sem conferir competência individual. Com P aberta e Q exportada compartilhando o credor, executar P pode escrever Q. Referências: `app/chamados/models.py:309`, `app/chamados/veredito_lavrado.py:270`, `app/chamados/models.py:1451`, `app/chamados/models.py:1483`, `app/chamados/models.py:1566`.

   Compartilhar credor entre dias não exige necessariamente corrupção: a validação aceita conversas e chamados de escopo diferente de DIA. A reversão, entretanto, fotografa apenas perguntas do dia-alvo. Referências: `app/chamados/juizes.py:2191`, `app/chamados/juizes.py:2205`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:191`. Ocorrência concreta dessa composição no acervo: [nao verificado].

   **O universo não corresponde exatamente aos 45 dias/pares fornecidos.** A parte principal usa as competências do vigia; somente o complemento exportado aplica `[hoje−45, hoje)`. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:69`, `app/chamados/services/vigia_fio.py:583`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:154`.

   A trava explícita do lote respeita **C → D → Ch → P**, com PK crescente em cada classe. Porém, enumera e trava todo o histórico dos colaboradores executáveis, inclusive terminais, sem recorte de 45 dias. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:179`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:181`, `app/chamados/models.py:2666`, `app/chamados/models.py:2675`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`.

   Quantidade de linhas travadas, duração, contenção e equivalência concorrente de todas as cascatas: [nao verificado].

5. **Relógio e teto temporal**

   **Há perda da injeção do relógio.** O comando captura `agora/hoje`, mas planejamento e execução chamam a cura sem transmiti-los. A cura consulta `pergunta_viva(p)` sem `hoje`; no ramo da janela, isso chega a `date.today()`. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:234`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:83`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:123`, `app/chamados/services/disputa_emissao.py:1797`, `app/chamados/juizes.py:1389`, `app/chamados/juizes.py:577`.

   Replay com `--hoje` e travessia de meia-noite podem, portanto, julgar com relógios distintos. A cascata de relavra também lê `timezone.now()` internamente. Referências: `app/chamados/management/commands/backfill_fio_morto_e4b.py:235`, `app/chamados/veredito_lavrado.py:271`.

   O universo do backfill exclui hoje/futuro; a cura do evento delega a vitalidade ao C6 e não acrescenta julgamento de fato encerrado. Referências: `app/chamados/services/vigia_fio.py:648`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:156`, `app/chamados/services/disputa_emissao.py:1797`. Violação concreta de teto por turno ainda em curso nas 131: [nao verificado].

6. **RED e mordida dos testes** [nao verificado]

   O módulo de memória executou **21 testes OK**, incluindo o harness dos 12 mutantes. O harness exige original verde e falha mutada por `AssertionError` na mensagem esperada. `app/chamados/tests/test_e4b_classe_e_memoria.py:395`.

   Reconstruindo somente em memória a pré-imagem das duas adições de emissão, o RED falha no assert de recuperar a âncora. Isso prova mordida na pré-imagem do patch apresentado; identidade independente dessa pré-imagem com `adc0610e6` é [nao verificado]. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:170`, `app/chamados/services/disputa_emissao.py:1782`, `app/chamados/services/disputa_emissao.py:1870`.

   Na tabela, “pega” descreve a proteção dos asserts; as limitações decorrem das bordas dubladas. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:117`, `app/chamados/tests/test_e4b_classe_e_memoria.py:144`, `app/chamados/tests/test_e4b_classe_e_memoria.py:284`.

   | Teste — arquivo:linha | Pega | Não prova | [nao verificado]
   |---|---|---|
   | `test_e4b_classe_e_memoria.py:170` | Retirar cura; usar pai em vez da âncora; nota | Pai reaberto adicionalmente; trilha real |
   | `test_e4b_classe_e_memoria.py:181` | Não recuperar pai quando é âncora | Porta real |
   | `test_e4b_classe_e_memoria.py:188` | Pular C6 com célula absolvida | Todos os degraus C6 |
   | `test_e4b_classe_e_memoria.py:194` | Tentar reabrir âncora viva | Visibilidade real da fila |
   | `test_e4b_classe_e_memoria.py:208` | Curar terminal de outro fecho | Viva em disputa fechada |
   | `test_e4b_classe_e_memoria.py:213` | Usar pai vivo para dispensar destino terminal | Pai adicional indevido |
   | `test_e4b_classe_e_memoria.py:219` | Desconsiderar `registrado` como vivo | Todos os estados |
   | `test_e4b_classe_e_memoria.py:225` | Excluir viva só porque tem carimbo | Lastro real |
   | `test_e4b_classe_e_memoria.py:231` | Retirar guarda de pergunta em voo | Integração com materializador |
   | `test_e4b_classe_e_memoria.py:237` | Roubar respondida do HX | HX real; texto sem timestamp |
   | `test_e4b_classe_e_memoria.py:244` | Tranca pelo C6 | Guarda C7 isoladamente |
   | `test_e4b_classe_e_memoria.py:252` | Segunda reabertura/nota no sucesso | Duas recusas; auditoria real |
   | `test_e4b_classe_e_memoria.py:258` | Tipo/motivo da recusa; fallback explícito | Efeitos reais de `irmao_vivo` |
   | `test_e4b_classe_e_memoria.py:271` | F1 real retendo fecho sintético | Cadeia ORM completa |
   | `test_e4b_classe_e_memoria.py:345` | DRY persistente; medida fora do atomic sintético | Rollback real de notas/trilha |
   | `test_e4b_classe_e_memoria.py:355` | Backfill sem chamar porta; segunda passagem | Persistência PostgreSQL |
   | `test_e4b_classe_e_memoria.py:367` | Imprimir recusa como resolvida | Vigia real |
   | `test_e4b_classe_e_memoria.py:375` | Exportada desaparecer da lista | Escrita exportada por cascata |
   | `test_e4b_classe_e_memoria.py:382` | Remover reconferência de exportação | Cascatas sobre outras perguntas |
   | `test_e4b_classe_e_memoria.py:388` | Planejamento executar reabertura | Toda escrita indireta |
   | `test_e4b_classe_e_memoria.py:395` | Encolher catálogo; mutante morrer por erro inadequado | Mutantes fora do catálogo |
   | `test_b2d_resposta_visivel.py:79` | Destino vivo, nota única, pergunta sem carimbos | Pai permanecer terminal; trilha única |

   **Dois mutantes adicionais sobreviveram em memória:** retirar a guarda C7 isolada e reabrir também o pai após reabrir o destino. O primeiro sobrevive porque o teste mantém `trancado=True` nas duas rodadas; o segundo porque nenhum assert exige pai terminal. Referências: `app/chamados/tests/test_e4b_classe_e_memoria.py:244`, `app/chamados/tests/test_e4b_classe_e_memoria.py:170`.

   A expectativa antiga de **pai permanecer terminal** continua compatível com a cura quando existe destino: a autoridade manda recuperar o destino. O aval fornecido autoriza essa extensão, mas não exige retirar a proteção do pai; a versão nova deixa de verificá-la. Referências: `app/chamados/services/fio.py:117`, `app/chamados/tests/test_b2d_resposta_visivel.py:97`. Autenticação local do texto do aval: [nao verificado].

   Os 25 testes ORM verdes são informação fornecida; não foram reexecutados nesta verificação. Suite inteira verde: [nao verificado]. [nao verificado]

7. **Quem escreve, quem lê e semântica**

   Os nomes físicos nesta cópia são `status_local`, `resolvido_em` e `via_encerramento`. `resolucao` persistida pertence à disputa; o encerramento da pergunta é representado pelos carimbos/via e interpretado pelo C6/`estado`. Referências: `app/chamados/models.py:52`, `app/chamados/models.py:60`, `app/chamados/models.py:1773`, `app/chamados/models.py:3034`.

   | Campo/estado | Escritores | Leitores relevantes |
   |---|---|---|
   | Chamado: status, instante e via | Nascimento e `_gravar_estado`; gates delegam ao núcleo. `app/chamados/models.py:1095`, `app/chamados/models.py:228` | S143, reconciliador, F1, vigia, Central, fio e vigia de trilha. `signals.py:96`, `reconciliador.py:1453`, `models.py:361`, `vigia_fio.py:618`, `painel.py:155`, `fio.py:117`, `vigia_loop.py:117` |
   | `resolvido_em` | Núcleo carimba/limpa. `app/chamados/models.py:243` | Recusa administrativa, indicadores da Central e chave da nota recusada. `models.py:1342`, `painel.py:245`, `disputa_emissao.py:1818` |
   | `respondida_em` | Resposta humana/automática, recuperação pela trilha, correção, carimbo e ressuscitação. `disputa_emissao.py:1468`, `disputa_emissao.py:1653`, `disputa_emissao.py:1674`, `disputa_emissao.py:1593`, `models.py:3132`, `models.py:1566` | C6, cura, HX, estado, crédito e histórico. `juizes.py:1341`, `disputa_emissao.py:1797`, `juizes.py:1951`, `models.py:3045`, `raiox.py:139`, `historico.py:99` |
   | `chamado_destino` | Nascimento/reuso e porta de reaponte; exclusão da FK pode zerá-lo. `app/chamados/models.py:2982`, `app/chamados/models.py:3014`, `app/chamados/services/fio.py:925`, `app/chamados/models.py:3385` | Âncora, F1, C6/lastro, HX, vigia, Central e materialização. `fio.py:117`, `models.py:361`, `juizes.py:834`, `juizes.py:1952`, `vigia_fio.py:662`, `raiox.py:146`, `materializacao.py:1241` |
   | Encerramento da pergunta | Portas `carimbar`, `carimbar_identidade`, `descarimbar` e descarimbo em lote. `app/chamados/models.py:3103`, `app/chamados/models.py:3168`, `app/chamados/models.py:3203`, `app/chamados/models.py:3252` | C6 e projeção de estado; carimbo não equivale automaticamente a morte. `app/chamados/juizes.py:1368`, `app/chamados/juizes.py:1399`, `app/chamados/models.py:3044` |
   | Notas | `anexar_ocorrencia`; leitura marcada e exclusão de ensaio nas portas próprias. `app/chamados/services/nota.py:75`, `app/chamados/services/nota.py:126`, `app/chamados/services/nota.py:136` | Fio, histórico, Central, detalhes, SLA e templates. Referências do item 2 |
   | Disputa: fecho/resolução | Fechar, reabrir e limpar resolução residual. `app/chamados/models.py:1934`, `app/chamados/models.py:2151`, `app/chamados/models.py:2175`, `app/chamados/models.py:2265` | Nova seleção S143 e fio administrativo discordam sobre exigir aberta. `disputa_emissao.py:1876`, `fio.py:212` |

   **Raiox tem derivação paralela de âncora:** prefere destino somente quando vivo; senão credita ao pai. `chamado_que_lista` prefere destino independentemente do status. Assim, pai vivo com destino terminal pode receber crédito na Central enquanto o fio ancora no destino. Referências: `app/chamados/services/raiox.py:142`, `app/chamados/services/fio.py:117`.

   Isso é divergência preexistente de significado entre crédito e listagem, não um segundo C6 criado pela cura. A extensão continua necessária segundo a autoridade de listagem. Referências: `app/chamados/services/raiox.py:51`, `app/chamados/services/disputa_emissao.py:1799`.

   Exaustão de chamadas indiretas, reflexão, SQL dinâmico e consumidores externos à cópia: [nao verificado].

8. **Classes de reprovação e decisão**

   Não identifiquei juiz novo nem escritor adicional de status na composição nova; identifiquei relógio sem injeção, critério adicional de disputa aberta, divergência de âncora, proteção incompleta de exportadas e selos com lacunas. Referências: `app/chamados/services/disputa_emissao.py:1793`, `app/chamados/services/disputa_emissao.py:1808`, `app/chamados/services/disputa_emissao.py:1876`, `app/chamados/services/raiox.py:146`, `app/chamados/models.py:309`, `app/chamados/tests/test_e4b_classe_e_memoria.py:244`.

   O lote explícito respeita a ordem de trava, mas isso não certifica todas as cascatas concorr

[truncado em 20000 de 21359 caracteres]

## 11/11 00:53 · sessao 2a0e6c1e5875 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Entrega em [BO_DISPUTA_ENTREGA_C20.txt](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_ENTREGA_C20.txt:1): censo, cura, RED reproduzível e comandos.

[49 testes em memória passaram](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_C20_TESTES.txt:1); [quatro mutantes morreram](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_C20_MUTANTES.txt:1). [nao verificado]

ORM parou por [ausência de Django](/home/ronald/wt-codex-bo-disputa-sem-pergunta-221357/app/BO_DISPUTA_C20_TESTES.txt:103); resultado da suite [nao verificado]. [nao verificado]

## 11/11 00:52 · sessao 724fa4bf93fc · BRIEF-COM-PROCEDENCIA · leitura · origem=Code

**BRIEF — próxima volta do construtor Codex — BO-DISPUTA-SEM-PERGUNTA**

Referências abaixo usam os nomes dos arquivos da cópia. Nos avais de parágrafo único, `arquivo:1` identifica o parágrafo transcrito no pedido.

**D1 — Estado e limite do aval**

**NÃO LIBERA push nem pouso desta obra.** Construa a cura e suas provas na raia autorizada; o `!` de PUSH-E4A2 pertence à reescrita das mensagens e ao push dos 23 commits daquela operação. Ele não libera BO-DISPUTA-SEM-PERGUNTA. A obra continua atrás de BO-0611-05. A restrição desta sessão permanece somente leitura. (`aval_duas_respostas_push_bo_1110.txt:1`; instrução do pedido.) [nao verificado]

A cura aprovada cobre exceção do gerador e pergunta reaproveitada de outra disputa: pai, disputa e pergunta devem participar do mesmo ato transacional; se a pergunta pertence a outra disputa, a nova não nasce e o motivo segue para a dona. Preserve as exceções expressamente decididas para HX e o contraste protegido de dia completo. (`aval_duas_respostas_push_bo_1110.txt:1`; `test_hx_disputa_muda_sem_ata.py:51`; `test_hx_disputa_muda_sem_ata.py:63`.)

Não tocar nas sete disputas existentes por shell, script ou reparo oportunista. O número sete é a medição recebida de 10/11 às 14:51; quantidade, classificação por ata e estado às 22:14: **[nao verificado]**. Não há autorização de saneamento do acervo neste aval. (`aval.txt:1`.)

**D2 — Curar a origem e fechar todos os caminhos de nascimento**

A próxima volta precisa eliminar as divergências entre núcleo, órfãos, declaração, pendentes e âncora. Hoje existem três criações de DS no emissor; a declaração também consegue chamar o núcleo sem pergunta. Não basta corrigir mais uma chamada isolada ou ampliar a catraca para aceitar outra escrita. (`disputa_emissao.py:670`; `disputa_emissao.py:750`; `disputa_emissao.py:1299`; `disputa_emissao.py:1554`; `test_porta_disputa_emissao.py:83`.)

Exigir um contrato comum de nascimento e **um agregador de motivos/contexto da emissão**, preservando os adaptadores necessários de cada origem. A âncora deve perder sua união e seu `save` próprios; os nascimentos devem ficar submetidos ao mesmo contrato de saída. Isso não exige fundir os protocolos distintos de fechamento, remoção e reabertura administrativa. (`disputa_emissao.py:558`; `disputa_emissao.py:1531`; `disputa_emissao.py:1562`; `test_porta_disputa_ciclo.py:86`.)

Fechar a API pública de declaração que aceita `pergunta=None` e deixa nascer uma DS aberta sem pergunta fora de HX. Os dois consumidores produtivos já fornecem os argumentos da pergunta; um estágio interno incompleto só pode existir dentro do ato que o completa ou desfaz. Não atualizar testes para legitimar um nascimento incompleto persistido. (`disputa_emissao.py:747`; `disputa_emissao.py:754`; `disputa_emissao.py:764`; `declaracao_texto.py:362`; `declaracao_texto.py:650`.) [nao verificado]

**D3 — Desenho: resolver antes de escrever**

Adotar a direção pedida no aval da matriz: resolver primeiro as perguntas canônicas e suas donas, depois executar uma passada de escrita, sem reentrada entre disputas e sem guarda de visitadas como solução do ciclo. Usar as autoridades existentes de identidade, universo e eleição da canônica; não criar um segundo deduplicador. (`aval_matriz_antes_do_opus_1110.txt:1`; `fio.py:1225`; `fio.py:1608`; `fio.py:1784`.)

A cópia oferece essas leituras fora de `models.py`; portanto, começar o encaixe no serviço, reutilizando-as. A viabilidade integral sem alterar `models.py` precisa ser demonstrada pela matriz e pelos testes; **[nao verificado]**. Se houver impedimento concreto, apresentar a operação que falta e o motivo, em vez de copiar o juiz ou ampliar o modelo preventivamente. (`fio.py:1784`; `models.py:2905`; `aval_matriz_antes_do_opus_1110.txt:1`.) [nao verificado]

Conservar a identidade completa do fato ao longo do ato: colaborador, motivo, dia do turno, episódio BATIDA e demais argumentos da porta. O coletor atual conserva kwargs, mas omite `batida_origem_id`; `_nascer` pode então derivar outro episódio a partir do contexto da dona. Acrescentar uma reprodução com contextos de episódios diferentes. (`disputa_emissao.py:500`; `disputa_emissao.py:504`; `models.py:2866`; `models.py:3452`.)

Preservar a ordem global **C → D → Ch → P**, com o universo necessário lido e travado antes das escritas. Pendentes deve participar da fronteira transacional do ato, inclusive quando chamado pelo wrapper; hoje esse chamado ocorre depois de o núcleo retornar. (`models.py:2833`; `disputa_emissao.py:627`; `disputa_emissao.py:1156`; `disputa_emissao.py:650`; `disputa_emissao.py:830`.)

**D4 — Motivos: corrigir o lote misto e a dona reaberta**

Eliminar o seed indevido de `orfao_14h` no repasse com ata. Hoje uma local aberta com `motivos=[]` recebe esse seed; se a pergunta de `orfao_14h` pertence a B e outra pergunta nasce em A, o retorno conserva o seed em A e a pergunta local impede o rollback. O teste deve usar exatamente `orfao_14h` estrangeiro mais intervalo local, com A inicialmente vazia, além da variante com motivos anteriores. (`disputa_emissao.py:1274`; `disputa_emissao.py:1275`; `disputa_emissao.py:980`; `disputa_emissao.py:1007`; `disputa_emissao.py:1444`.) [nao verificado]

Não converter isso em exclusividade global de um motivo entre disputas. O mesmo motivo pode corresponder a fatos de dias diferentes; o contrato deve distinguir pergunta local legítima de motivo colocado na local apenas porque apareceu no lote estrangeiro. Não limpar duplicações antigas do acervo nesta obra. (`disputa_emissao.py:598`; `test_bo_disputa_volta4_memoria.py:36`; `aval.txt:1`.)

Nos E1/E2 de órfãos, deixar o cartório ressuscitar e reabrir a dona pela porta existente. **Depois dessa reabertura, garantir que o motivo chegue à dona**, usando o agregador comum e uma leitura atualizada do estado. Hoje a agregação ocorre enquanto ela está fechada e retorna `None`; a reabertura posterior não agrega motivos. (`disputa_emissao.py:563`; `disputa_emissao.py:1458`; `disputa_emissao.py:1459`; `models.py:1574`; `models.py:2173`.)

Preservar os asserts atuais de reabertura e nota quando houver chamado. Acrescentar neles a presença do motivo na dona após o cartório. Não voltar à exigência de `.values()` idêntico depois de `analisar()`: a reabertura prevista muda legitimamente o estado. O 5e prova a força desses asserts com doubles; não certifica a integração produtiva. (`test_bo_disputa_volta5b_orfaos.py:102`; `test_bo_disputa_volta5b_orfaos.py:126`; `test_bo_disputa_volta5b_orfaos.py:175`; `test_bo_disputa_volta5e_memoria.py:49`; `test_bo_disputa_volta5e_memoria.py:72`.)

**D5 — Três saídas distintas para órfãos sem pergunta**

| Estado | Contrato exigido |
|---|---|
| Só perguntas estrangeiras | A nova local não nasce; preservar a FK da pergunta, realizar a costura autorizada e encaminhar o motivo à dona. (`aval_duas_respostas_push_bo_1110.txt:1`; `test_bo_disputa_volta5b_orfaos.py:175`.) |
| Sem ata | Continua nascendo aberta, sem pergunta e sem fechamento; conservar o aviso e publicar I7b. Não fabricar pergunta para zerar o contador. (`test_hx_disputa_muda_sem_ata.py:51`; `auditar_invariantes_chamados.py:74`.) |
| Ata completa, nenhum marco faltante, nenhuma estrangeira | **DECIDIDO PELA LEI HX-DISPUTA-MUDA: preservar nascimento seguido de fechamento vazio, sem push \| contestável.** Não estender essa saída ao caso “só estrangeiras”. (`test_hx_disputa_muda_sem_ata.py:63`; `test_hx_disputa_muda.py:24`; `CLAUDE.md:266`.) | [nao verificado]

Manter I7a como DS aberta sem nenhuma linha de pergunta, responsabilidade ESTRUTURA, esperado zero; I7b publica a abstinência por falta de ata como INFO. Pergunta morta continua sendo linha do agregado. Não trocar “nenhuma pergunta” por “nenhuma pergunta viva”, nem classificar dia indeterminado como falta comprovada de ata. (`auditar_invariantes_chamados.py:61`; `auditar_invariantes_chamados.py:64`; `auditar_invariantes_chamados.py:73`; `auditar_invariantes_chamados.py:81`.)

**D6 — Push: preservar a política e provar o destinatário** [nao verificado]

A referência recebida do Opus distingue nascimento, agregação e órfãos: nascimento envia os motivos do nascimento; agregação envia apenas os novos, respeitando HX-MOTIVO-REPETIDO; órfãos usa `motivos_atualizados`. Não substituir essa política por “perguntas criadas nesta chamada” ou por outro filtro introduzido pelos briefs anteriores. A comparação histórica executável com `6c0517eb9`: **[nao verificado]**. (`opus5d.txt`, item 3; `disputa_emissao.py:832`; `disputa_emissao.py:1484`.)

O resultado da agregação precisa conservar, por dona, os motivos admitidos e os novos. A matriz deve mostrar como cada destinatária entra na política legada; não aceitar a perda silenciosa dos novos motivos estrangeiros, cujo retorno hoje só serve para escolher `ato['agregada']`. Qualquer alteração adicional de critério deve aparecer expressamente, sem ser atribuída ao aval. (`disputa_emissao.py:610`; `disputa_emissao.py:613`; `disputa_emissao.py:614`; `aval_matriz_antes_do_opus_1110.txt:1`.)

Preservar declaração sem push. Para os demais caminhos, decidir o envio sobre o estado persistido após as cascatas: uma instância devolvida ainda aberta não prova que outra instância não a fechou. Conservar também o caso em que o gerador de pendentes foi chamado, retornou vazio e fechou por cascata. (`disputa_emissao.py:734`; `disputa_emissao.py:827`; `disputa_emissao.py:835`; `test_bo_disputa_volta8_memoria.py:182`.) [nao verificado]

**Decisão técnica minha:** retirar o efeito externo de push de dentro da transação que ainda pode ser desfeita, usando entrega após commit, com destinatária e payload capturados e estado aberto conferido no envio. Provar rollback sem entrega e commit com a entrega prevista. Hoje os complementos são notificados antes do fim do atomic externo; vazamento externo concretamente ocorrido: **[nao verificado]**. (`disputa_emissao.py:1215`; `disputa_emissao.py:1230`; `disputa_emissao.py:1465`.) [nao verificado]

**D7 — Matriz, testes e mutantes obrigatórios** [nao verificado]

Entregar a matriz completa, com **estado → juiz → operação → persistido → retorno → prova**, antes de pedir verificação final. Para cada linha, informar o teste existente, o novo teste ou **AUSENTE**; não apresentar como anexadas `matriz1.r`, `matriz2.r` ou bases que não foram fornecidas. Seu conteúdo integral: **[nao verificado]**. (`aval_matriz_antes_do_opus_1110.txt:1`; `opus5d.txt`, itens 4 e 10.) [nao verificado]

| Linha | Prova exigida nesta volta |
|---|---|
| a — fato novo | Pai/DS/P persistem juntos; falha antes e depois da criação real de P desfaz o agregado. Antes de levantar, afirmar positivamente que P nasceu. (`test_bo_disputa_sem_pergunta.py:32`; `test_bo_disputa_sem_pergunta.py:55`; `test_bo_disputa_sem_pergunta_memoria.py:301`.) |
| b — canônica em outra aberta | Sem nascimento local vazio; FK preservada; motivo na dona; lote misto sem seed indevido em A. (`disputa_emissao.py:606`; `disputa_emissao.py:1275`.) |
| c — canônica em fechada | Separar o caso com reabertura pelo cartório do caso residual que permanece fechado; este depende da pergunta de lei abaixo. (`models.py:1574`; `test_bo_disputa_volta8_memoria.py:207`.) |
| d — ciclo A↔B | Resolver cada fato e dona sem reentrada, sem perda de motivos e sem nova DS vazia; repetir o ato e comparar o estado. (`test_bo_disputa_volta4_memoria.py:20`; `aval_matriz_antes_do_opus_1110.txt:1`.) |
| e — exceção após batida gravada | Batida anterior preservada; pai/DS/P/notas/logs do ato desfeitos; zero push. Demonstrar nova tentativa pelo emissor real; não prometer reemissão automática sem prova do chamador. (`test_bo_disputa_sem_pergunta.py:38`; `test_bo_disputa_sem_pergunta.py:49`; `test_bo_disputa_sem_pergunta.py:52`.) | [nao verificado]
| f — HX sem ata | Cena protegida acusa I7a=0 e I7b=1; defeito artificial com ata acusa I7a=1. (`test_bo_disputa_sem_pergunta.py:95`; `test_bo_disputa_sem_pergunta.py:100`.) |
| g — grade | Não restaurar o helper vazio removido; cobrir o nascimento pela fábrica produtiva da célula. (`reconciliar_grade.py:412`; `fabrica_da_celula.py:172`.) |
| h — órfãos | Cobrir as três saídas de D5, lote misto, reabertura real, nota, motivo na dona e repetição do ato. (`test_bo_disputa_volta5b_orfaos.py:126`; `test_bo_disputa_volta5b_orfaos.py:154`; `test_bo_disputa_volta5b_orfaos.py:175`; `test_bo_disputa_volta5b_orfaos.py:191`.) |
| i — motivo em duas disputas | Distinguir fatos locais legítimos de seed/encaminhamento indevido; nenhuma limpeza geral do acervo. (`test_bo_disputa_volta4_memoria.py:36`; `disputa_emissao.py:1007`.) |
| j — push | Destinatária, motivos, `nova`, repetição, fechamento por outra instância e rollback/commit; declaração permanece sem envio. (`disputa_emissao.py:734`; `disputa_emissao.py:838`; `test_bo_disputa_volta8_memoria.py:112`.) | [nao verificado]

Acrescentar provas dos dois consumidores de declaração, incluindo candidata existente e costura real; os doubles atuais não certificam esses caminhos ORM. Cobrir também âncora e pendentes autônomos no contrato transacional comum. (`declaracao_texto.py:362`; `declaracao_texto.py:650`; `test_bo_disputa_volta5c_memoria.py:77`; `test_bo_disputa_volta5c_memoria.py:89`; `disputa_emissao.py:257`; `disputa_emissao.py:1498`.)

Mutantes obrigatórios, aplicados à implementação produtiva: [nao verificado]

- Desligar a proteção transacional; permitir nascimento sem pergunta; manter o seed indevido na local; omitir o encaminhamento ou a agregação depois da reabertura. Cada cenário deve falhar pelo estado incorreto correspondente. (`disputa_emissao.py:623`; `disputa_emissao.py:681`; `disputa_emissao.py:1275`; `disputa_emissao.py:1458`.)
- Trocar o episódio BATIDA pelo contexto da dona; aceitar identidade divergente ou inserção onde se esperava reuso. Se o desenho eliminar replay, substituir o mutante antigo por seu equivalente no contrato novo, preservando a obrigação. (`models.py:2866`; `disputa_emissao.py:711`; `disputa_emissao.py:1454`.)
- Desligar a trava real do repasse; omitir o registro de perguntas criadas em pendentes; confundir I7a/I7b; permitir push após fechamento ou rollback. (`test_e1_fila_v2_2_integracao.py:211`; `test_bo_disputa_volta5b_orfaos_memoria.py:223`; `test_bo_disputa_sem_pergunta_memoria.py:469`; `test_bo_disputa_volta8_memoria.py:24`.) [nao verificado]

**Controle verde seguido de falha comportamental por assertion é obrigatório.** Consertar os seletores de `nasce_sem_pergunta`, `replay_insere` e do mutante E1: alvo ausente ou múltiplo é erro de montagem, não mutante morto. Não afrouxar as assertions para fazê-los passar. (`test_bo_disputa_volta5_memoria.py:66`; `test_bo_disputa_volta5_memoria.py:81`; `test_e1_fila_v2_2_integracao.py:213`; `test_bo_disputa_volta5b_orfaos_memoria.py:287`.) [nao verificado]

Não chamar de paridade histórica o teste que executa a fonte atual duas vezes. Renomear a prova como repetibilidade ou fornecer a fonte histórica efetivamente comparada. (`test_bo_disputa_volta3_memoria.py:236`; `test_bo_disputa_volta3_memoria.py:245`; `test_bo_disputa_volta3_memoria.py:300`.) [nao verificado]

**D8 — Proibidos e critérios de pronto**

Não afrouxar S127, alterar a identidade da pergunta, mover sua FK para fabricar pertencimento local, suprimir o cartório ou reabrir genericamente uma dona fechada pelo agregador. Não alterar os protegidos HX e PERGUNTA-NO-APP para acomodar a cura. (`juizes.py:2311`; `models.py:3008`; `models.py:1574`; `test_hx_disputa_muda_sem_ata.py:58`; `test_pergunta_no_app.py:89`.)

Preservar a fixture já corrigida na cópia: batidas primeiro, chamado depois, célula por último. A reprovação antiga da fixture não autoriza repetir a correção nem desligar o receiver. (`test_bo_disputa_volta5b_orfaos.py:32`; `test_bo_disputa_volta5b_orfaos.py:56`; `test_bo_disputa_volta5b_orfaos.py:63`; `signals.py:188`.)

Pronto exige: matriz completa; contrato comum demonstrado em todos os caminhos; agregador sem predicado paralelo na âncora; motive ownership correto; fronteiras transacionais e de push provadas; controles e mutantes válidos; regressões protegidas; decisão da linha de lei residual; verificação final com a matriz disponível. Gate AST verde isolado não satisfaz esses critérios. (`aval_matriz_antes_do_opus_1110.txt:1`; `test_porta_disputa_emissao.py:83`; `test_porta_disputa_emissao.py:128`; `disputa_emissao.py:1562`.) [nao verificado]

Executar as famílias memória pertinentes, os testes ORM de BO/declaração/órfãos/âncora, os gates de portas e a integração E1 em PostgreSQL de teste. Registrar comandos, revisão testada, resultados e mutantes. O log recebido mostra `Ran 679 tests` e três falhas transcritas; não certifica a árvore posterior. Os resultados locais do construtor13 certificam somente os comandos relatados de 5e. Suíte atual completa e execução ORM da cura: **[nao verificado]**. (`mod11_falhas.txt`, trecho final e três tracebacks; `construtor13.r`, bloco “Execução local”.) [nao verificado]

**CENSO**

Censo dos caminhos e campos relevantes à cura; exaustividade de todos os leitores do repositório e ESMERIL-SEMÂNTICA completo: **[nao verificado]**.

| Estado/campo | Escritores localizados | Leitores relevantes |
|---|---|---|
| Nascimento DS | Núcleo, órfãos e âncora. (`disputa_emissao.py:670`; `:1299`; `:1554`.) | Declaração entra pelo núcleo; fábrica da célula entra pelo wrapper. (`declaracao_texto.py:362`; `:650`; `fabrica_da_celula.py:172`.) |
| `DS.motivos` | Agregador; nascimentos; escrita paralela da âncora; aparo, remoção e reabertura administrativa. (`disputa_emissao.py:569`; `:671`; `:1301`; `:1556`; `:1562`; `models.py:1966`; `:2063`; `:2159`.) | Gerador, admissão local, push e resolução da foto. (`disputa_emissao.py:440`; `:598`; `:832`; `:1484`; `foto_recebida.py:63`.) | [nao verificado]
| `DS.contexto` | Agregador/nascimentos e portas de aparo, remoção e anotação. (`disputa_emissao.py:572`; `:672`; `:1302`; `:1557`; `models.py:1967`; `:2064`; `:2104`; `:2122`.) | Resolvedor da pergunta, derivador de episódio BATIDA e auditor. (`disputa_emissao.py:371`; `:386`; `models.py:3463`; `auditar_invariantes_chamados.py:31`.) |
| `DS.fechada_em` | Fechamento e reaberturas admin/sistema; desligamento chama a porta de fechamento. (`models.py:1932`; `:2149`; `:2173`; `desligamento.py:142`.) | Eleição, agregador, push, receivers, app e S127. (`disputa_emissao.py:630`; `:563`; `:827`; `signals.py:28`; `api/views.py:1059`; `juizes.py:2316`.) |
| `P.disputa` | Inserção; o reuso retorna a canônica preservando sua FK. (`models.py:3014`; `:3008`.) | Roteamento do motivo, autorização e listagem. (`disputa_emissao.py:606`; `fio.py:598`; `api/views.py:1063`.) |
| `P.chamado_destino` | Inserção, reuso e reapontamento pela porta. (`models.py:3014`; `:2982`; `fio.py:925`.) | Juiz da resolvedora e seleção do card. (`models.py:2972`; `api/views.py:1061`.) |
| Luz sem pergunta | Contadores I7a/I7b derivados da leitura das DS abertas sem linhas de pergunta. (`auditar_invariantes_chamados.py:56`; `:62`.) | Classificação INFO de I7b e saídas textual/HTML. (`auditar_invariantes_chamados.py:81`; `:1912`; `:1924`.) |

Para o motivo grave, as entradas atuais localizadas são `api/views.py:729` e `api/views_core.py:726`, que chegam ao wrapper em `:794`, ao núcleo em `disputa_emissao.py:817`, ao nascimento em `:670` e ao gerador em `:674`. Isso identifica o caminho atual; qual caminho produziu cada uma das sete e desde qual commit: **[nao verificado]**.

Os endereços antigos da grade não são escritores atuais: há lápide de remoção do helper vazio. Não restaurá-lo para satisfazer uma matriz histórica. (`reconciliar_grade.py:412`; `test_porta_disputa_emissao.py:49`.)

**Decisões:** preservar HX sem ata; preservar o contraste de ata completa; preservar a reabertura pelo cartório em E1/E2; curar o motivo perdido depois dela; eliminar os predicados paralelos da emissão; não transformar testes de imutabilidade genérica em lei. (`test_hx_disputa_muda_sem_ata.py:51`; `:63`

[truncado em 20000 de 24409 caracteres]

## 11/11 00:52 · sessao 342135715a21 · BRIEF-COM-PROCEDENCIA · leitura · origem=Code

Codex — **BO-DISPUTA-SEM-PERGUNTA: próxima volta de construção, após a parada da volta 5.** Base de trabalho: cópia informada `8e9df619a81701fa2a97676eb797a65bb7b4fb70`; identidade pelo histórico Git desta cópia: [nao verificado].

**1. Objetivo e autoridade.** Cure o nascimento do agregado: DS nova confirma com pergunta própria na mesma transação, ou seu nascimento não persiste. Preserve a exceção expressamente aprovada da HX-ATA-NAO-SABE: naquela cena, a DS continua aberta, sem pergunta, sem fechar, com warning e contador publicado. O aval posterior da matriz inclui os persistidos violadores encontrados em grade, órfãos e declaração; o descarte anterior “fora do aval” não os encerra. Fontes: `aval_duas_respostas_push_bo_1110.txt`, resposta 2; `aval_matriz_antes_do_opus_1110.txt`, itens 3–4 [nao verificado]. A cena HX está em `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51–61`.

A obra permanece atrás do BO-0611-05, que segue independente. Não afirme que essa obra anterior terminou: isso não consta dos insumos. Fonte: `aval_matriz_antes_do_opus_1110.txt`, item 5 [nao verificado].

**2. A parada da volta 5 procede; a premissa precisa ser corrigida.** O gerador não escreve somente perguntas: quando cria pergunta sem destino, pode chamar reatamento; esse caminho pode lavrar e fechar sua DS. Planejamento não pode chamar esse gerador, nem chamar a porta escritora dentro de rollback e apresentar isso como leitura. Separe resolução, emissão e efeitos delegados; mantenha os efeitos necessários na fase de escrita, com fronteira e ordem demonstradas. (`app/chamados/services/disputa_emissao.py:472–477`; `app/chamados/reconciliador.py:405–412`; `app/chamados/models.py:1932–1936`; `app/chamados/services/fio.py:1077–1081`.)

Essa cadeia **não prova fechamento de dona já fechada**: a lavra exige DS aberta. O problema separado é que salvar motivos/contexto de uma dona fechada pode reconciliar pai, agendar retomada de regularizações e materializar pendentes. Não declare esses receivers no-op por comentário de idempotência; use-os nos testes reais e apresente o persistido observado. (`app/chamados/reconciliador.py:411`; `app/chamados/services/disputa_emissao.py:578`, `:614`; `app/chamados/signals.py:28–45`, `:129–163`.)

**3. Desenho desta volta: resolver antes de escrever, sem reentrada.** Implemente no serviço, mantendo `models.py` fora do diff. Retire a exceção de descoberta `_PerguntaDeOutraDisputa`, a recuperação recursiva e `_donas_visitadas` quando o novo caminho estiver coberto. Não retire exceções legítimas de recusa das portas. Essa é a direção aprovada pela matriz, não uma promessa de que já exista um planejador completo. (`app/chamados/services/disputa_emissao.py:487–499`, `:619–629`, `:681–707`; `app/chamados/models.py:2819–2822`, `:2888–2890`, `:2954`; `aval_matriz_antes_do_opus_1110.txt`, item 3 [nao verificado].)

O ato deve seguir esta sequência:

- Abra a transação e trave C antes da leitura decisória. Resolva todos os itens do lote, inclusive dias pendentes, antes de começar suas escritas. Preserve identidade e insumos do pedido: colaborador, motivo, instância, episódio, dia do juiz, destino, texto e natureza fato/conversa. Trocar de dona não pode recalcular outro fato pelo contexto dela. (`app/chamados/models.py:2845`, `:2866–2878`; `app/chamados/services/disputa_emissao.py:335`, `:350–355`, `:278–294`.)
- Eleja a dona pelo mesmo universo da porta: `universo_do_fato` e `canonica_do_grupo`. Não substitua por `do_fato` isolado, filtro de abertas/vivas ou igualdade apenas de motivo/dia. Conversa conserva seu contrato próprio. (`app/chamados/models.py:2905–2921`; `app/chamados/services/fio.py:1225–1233`, `:1784–1837`; `app/chamados/models.py:2794–2802`.)
- Reúna o universo completo de travas antes das escritas: todos C → todas DS → todos Ch → todas P, por PK em cada grupo, usando somente `trava_na_ordem`. Inclua pais e destinos terminais das canônicas fechadas; o universo automático cobre pais de abertas e chamados vivos. Travar um item completo e depois descobrir a DS do segundo item não satisfaz a ordem global. (`app/chamados/models.py:2631–2647`, `:2654–2693`; `app/chamados/services/declaracao_texto.py:299–303`.)
- Escreva cada partição uma vez pelas portas existentes. Confira os retornos reais e o contrato final antes de confirmar. Divergência entre plano e porta é recusa transacional, não autorização para reentrada, desvio silencioso ou redistribuição pelo contexto de outra dona. O nome e a forma dessa recusa são decisão de implementação minha; sua necessidade decorre da eleição e das recusas ainda executadas pela porta. (`app/chamados/models.py:2907–2957`, `:3008–3017`.)

Não contorne a parada removendo reatamento, costura, lavra ou receivers para fazer a suite passar. Mostre como esses efeitos permanecem corretos no ato planejado e como não invalidam os itens seguintes. Se demonstrar impossibilidade concreta sem alterar a porta, entregue a cadeia e o requisito incompatível; não invente uma segunda identidade ou um bypass de escrita. (`app/chamados/services/disputa_emissao.py:466–482`; `app/chamados/models.py:2940–3008`; `app/chamados/signals.py:129–163`.)

**4. Contrato de saída a implementar e provar.**

| Estado | Resultado exigido |
|---|---|
| Fato novo aceito | DS, pergunta própria e efeitos de pai/trilha confirmam juntos. Exceção posterior dentro do ato desfaz suas escritas. Preserve a batida gravada antes dessa fronteira. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:32–66`.) |
| Canônica em outra DS aberta | Não nasce DS para esse fato; motivo e contexto pertinentes seguem à dona efetiva, sem trocar a FK da pergunta. (`app/chamados/models.py:3008`; `aval_duas_respostas_push_bo_1110.txt`, resposta 2 [nao verificado].) |
| Canônica em DS fechada | Reuso não reabre a dona. Não execute novamente o gerador a partir do contexto dela para descobrir o fato. Preserve as portas de rechaveamento/costura necessárias e prove as cascatas do save, inclusive callbacks. (`app/chamados/services/disputa_emissao.py:576–578`; `app/chamados/models.py:2940–3008`; `app/chamados/signals.py:129–163`.) |
| Lote com várias donas e itens novos | Particione pelo fato fixado. Itens com canônica seguem às respectivas donas; itens sem dona usam a aberta elegível ou uma DS que efetivamente receberá pergunta própria. Não despeje restantes na primeira dona fechada. Esta distribuição é decisão de desenho minha para substituir a recuperação atual. (`app/chamados/services/disputa_emissao.py:664–668`, `:695–707`.) |
| Catálogo/texto/dia indisponível ou lista vazia | Preserve a recusa de pergunta; impeça nascimento vazio. Não invente pergunta, motivo, destino ou dia de hoje. Não classifique recusa técnica como HX. (`app/chamados/services/disputa_emissao.py:323–329`, `:361–364`, `:404–406`, `:438–443`, `:651–680`.) |
| Pergunta já existente | Distinga dedup de ausência de insumo. Existência não significa resolução nem pergunta nova; o retorno deve identificar a pergunta e sua dona quando houver. (`app/chamados/services/disputa_emissao.py:415–419`, `:380–383`, `:483–484`.) |
| Sem ata, cena HX aprovada | DS aberta, zero perguntas, nenhum fecho, warning preservado; I7a=0 e I7b=1. Não transforme toda ausência de pergunta nessa exceção. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51–61`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:95–98`.) |
| Exceção de emissão após batida | Desfaça o agregado da emissão; não engula a exceção como sucesso. Reemissor universal e prazo de replay permanecem [nao verificado]. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:32–66`; `app/api/views_core.py:681`, `:794`.) |

O resultado interno deve permitir observar **cada dona e seus efeitos**, em vez de devolver a primeira DS com a união dos motivos de todas. Compatibilize os consumidores externos: DS válida, nenhum nascimento e recusa precisam continuar distinguíveis. Não devolva instância transitória como sucesso. Decisão de desenho minha, motivada pelo retorno atual e pelos leitores. (`app/chamados/services/disputa_emissao.py:695–707`, `:1135`; `app/chamados/services/acoes_disputa.py:222–231`; `app/chamados/services/cobrar_dia.py:49–55`.)

**5. Fronteiras obrigatórias da cura.**

- **Grade:** cure a chamada real de `_disputa`, que fornece `motivos=[]`; não basta testar diretamente o núcleo. Sem fatos perguntáveis, não deve fabricar DS/pai. Não apague o comando nem o descreva como incapaz de escrever: há caminhos com `--apply`. (`app/ponto/management/commands/reconciliar_grade.py:424–428`, `:250–254`, `:305–316`; `app/chamados/tests/test_porta_chamado_e3.py:91–98`.)
- **Órfãos:** substitua o booleano que ignora o retorno de `abrir` por resultado fiel à pergunta/dona efetiva. Preserve complementos legítimos em DS preexistentes e não marque chamado como analisado por um falso “criou local”. DS nova sem pergunta, fora HX, não pode persistir nem como `fechada/vazia`; pai, ocorrência e trilha da tentativa também não devem sobrar. Isso não autoriza desfazer complementos anteriores somente porque o agrupador terminou com `None`. (`app/chamados/services/disputa_emissao.py:931–950`, `:1184–1197`, `:1223–1231`, `:1261–1264`, `:1354–1359`, `:1385–1391`.)
- **Declaração do colaborador e do admin:** cure o ato completo, incluindo todos os returns normais após `abrir`. Escolha a dona antes do nascimento e use a dona efetiva na resposta e validação; conferir somente antes da porta ou validar depois com a DS antiga deixa o buraco. Preserve urgência normal, contexto preexistente e ausência de push/varredura genérica. (`app/chamados/services/declaracao_texto.py:250–254`, `:360–426`, `:659–697`; `app/chamados/services/validacao.py:385–392`; `app/chamados/tests/test_porta_disputa_emissao.py:194–230`.)
- **Pendentes, fábrica e smoke:** ajuste-os para consumir a emissão concluída, evitando redescoberta por outra dona ou segunda geração fora da fronteira planejada. Preserve a emissão dos demais dias legítimos. Não use essa integração para mudar a regra da fábrica ou seus tetos. (`app/chamados/services/disputa_emissao.py:278–296`, `:788`; `app/chamados/services/fabrica_da_celula.py:172–174`; `app/chamados/management/commands/smoke_chamados.py:99–102`.)
- **Âncora:** preserve conversa local no atomic e suas guardas; não aplique dedup de fato à conversa. O nascimento atual fica em `:1461`, não em `:1452` nesta cópia. (`app/chamados/services/disputa_emissao.py:1419–1434`, `:1461–1485`; `app/chamados/models.py:2794–2802`.)

**6. Luz e push.** Conserve I7a como esperado=0, dono ESTRUTURA; I7b como número publicado/INFO, dono ata/cartório. A medição é DS aberta com **zero linhas** de pergunta, não zero perguntas vivas. I7b exige conjunto de dias não vazio, sem `None`, e todos os dias com resposta `sem_celula` do juiz existente. Dia indeterminado permanece ESTRUTURA. Se extrair nova resolução comum, o auditor deve consumi-la sem perder essas distinções. (`app/chamados/management/commands/auditar_invariantes_chamados.py:35–83`.)

Para o push ao colaborador, minha decisão de desenho é: derivar a tentativa do resultado confirmado por dona, após commit; conservar o silêncio contextual, a ausência de push na declaração e o critério de motivo novo na agregação. Não anunciar “toque para responder” por dona fechada ou sem pergunta respondível; a abstenção HX continua acendendo sua luz, sem fabricar pergunta para justificar notificação. Exija controles positivos para impedir a solução “nunca notificar”. O código atual permite tentativa pela primeira dona e não possui essas conferências no helper. (`app/chamados/services/disputa_emissao.py:774–790`, `:1392–1393`, `:1854–1882`; `app/chamados/tests/test_porta_disputa_emissao.py:223–230`.) Entrega real de WebPush/FCM: [nao verificado].

**7. O que não pode fazer.** Não saneie as sete DS do relato por shell, migração, fechamento, exclusão, transferência de pergunta ou edição de motivos. Não afirme que esta cura já corrigiu o acervo ou que o escritor encontrado explica individualmente aquelas sete ocorrências. Fonte: `aval.txt`, “as 7 existentes não se mexem por shell” [nao verificado]. A associação causal por ocorrência não veio nos insumos [nao verificado].

Não torne `DS.motivos` uma chave exclusiva de fato, nem remova motivos preexistentes de outras DS para forçar exclusividade. Não altere HX, S127, eleição da canônica, índices, contratos de dia, juiz de fechamento ou ordem de travas. Não use `update`, SQL direto, desligamento de signals ou nova allowlist para escapar dessas portas. (`app/chamados/models.py:1743`, `:2631–2647`, `:2712–2725`, `:2810–2822`; `app/chamados/juizes.py:2308–2321`; `app/chamados/tests/test_e1_v7_catracas.py:387–392`.)

**8. RED, testes e mutantes exigidos.** Para cada cura, entregue cenário que falha na cópia anterior e passa no desenho novo. A prova principal deve atravessar as portas reais; doubles de memória continuam úteis, mas não certificam ORM, signals, callbacks, concorrência ou SQL. O teste ORM atual de retorno estrangeiro mocka a porta e o push. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:68–91`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:73`.)

| Prova obrigatória | Mutante que deve morrer |
|---|---|
| Falha antes e depois da primeira P; rollback de DS/pai/P/trilha, batida anterior preservada, nenhum push. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:32–66`.) | Tirar emissão da transação ou confirmar parcialmente o agregado. |
| Canônica estrangeira em aberta e fechada; lote com várias donas e restantes; nenhuma FK transferida nem DS fantasma. (`app/chamados/models.py:2907–3017`; `app/chamados/services/disputa_emissao.py:664–707`.) | Ignorar dona retornada, reunir tudo na primeira ou permitir nascimento vazio. |
| Ciclos A↔B nos dois sentidos, ciclo maior, contexto recebido vazio; fato inicial e episódio permanecem fixos, sem recursão. (`app/chamados/tests/test_bo_disputa_volta4_memoria.py:118–177`.) | Recalcular dia/episódio pelo contexto da dona ou reintroduzir reentrada. |
| Segundo item do lote exige DS fechada e destino terminal; observação de travas reais, com todos C antes de D/Ch/P. (`app/chamados/models.py:2631–2693`; `app/chamados/tests/test_e1_v15f.py:117`.) | Planejar o segundo item depois de adquirir Ch/P ou omitir destino terminal. |
| Gerador alcança lavra/fecho; dona fechada com pai/pendentes/regularizações aptos; callbacks executados e repetição com estado idempotente. (`app/chamados/reconciliador.py:405–412`; `app/chamados/signals.py:28–45`, `:129–163`.) | Antecipar pós-efeito no planejamento, reemitir fechamento indevido ou suprimir delegação necessária. |
| Grade real, catálogo ausente, texto/dia ausente e lista vazia; nenhum nascimento fora HX. (`app/ponto/management/commands/reconciliar_grade.py:424`; `app/chamados/services/disputa_emissao.py:323–329`, `:404–443`.) | Retirar guarda final ou trocar “não nasce” por `fechar(vazia)`. |
| Órfãos com estrangeira, dia completo e nenhum elegível; marcas e complementos pertencem ao efeito real. (`app/chamados/services/disputa_emissao.py:940–950`, `:1184–1197`, `:1385–1391`.) | Ignorar `(pergunta, criada)` ou declarar criação local incondicional. |
| Dois consumidores de declaração e seus returns normais; admin valida a dona correta. (`app/chamados/services/declaracao_texto.py:376–426`, `:666–697`.) | Conservar DS local vazia ou validar usando a DS antiga. |
| HX original aberta/zero P/warning; I7a=0/I7b=1. DS inválida injetada com ata acusa I7a=1. Morta continua sendo linha. (`app/chamados/tests/test_bo_disputa_sem_pergunta.py:95–114`.) | Contar toda muda como estrutura ou ocultar muda com pergunta morta por filtro de vitalidade. |
| `None` de helper, dia `None`, três fontes de órfãos, vazio e mistura com `sem_celula`; controles todos sem célula. (`app/chamados/tests/test_bo_disputa_volta4_memoria.py:185–238`.) | `any_sem_ata`, `all_vazio`, `descarta_none_motivo`, `descarta_none_orfao`. |
| Push permitido por dona e silêncio em rollback, dona fechada, nenhum nascimento e declaração. (`app/chamados/services/disputa_emissao.py:774–790`, `:1854–1882`.) | Push antecipado, união de todas as donas na primeira ou supressão total de push. |

Os mutantes novos acima são exigências minhas de verificação; sua implementação e eficácia ainda são [nao verificado]. Cada execução precisa de controle verde e mudança efetivamente aplicada; erro de montagem não é mutante morto. No desenho sem recursão, `RecursionError` deixa de ser resultado aceitável para provar a cura. (`app/chamados/tests/test_bo_disputa_volta3_memoria.py:461–467`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:271–280`.)

Adapte os testes que congelam o mecanismo antigo: duas transações exatas, última dona alcançada por reentrada e nascimento vazio isolado. Preserve as asserções de negócio e explique cada expectativa substituída. Não preserve DS fechada vazia só para manter teste legado verde, nem apague o controle HX para simplificar o invariante. (`app/chamados/tests/test_bo_disputa_volta3_memoria.py:245–246`, `:277–283`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:134–155`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:402–420`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63–79`.)

Preserve também universo/canônica, corrida com uma P e segunda `criada=False`, rechaveamento e recusas, único adquirente de travas, derivação pela data da pergunta, censo dos atos e âncora. (`app/chamados/tests/test_e1_v13.py:88`, `:98`, `:346`; `app/chamados/tests/test_e1_identidade_corrida.py:74–88`; `app/chamados/tests/test_e1_v15d.py:60–124`; `app/chamados/tests/test_e1_v7_catracas.py:387–392`; `app/chamados/tests/test_selo_derivacao_pela_data_da_pergunta.py:80–148`; `app/chamados/tests/test_porta_disputa_emissao.py:84–119`; `app/chamados/tests/test_fio_ancora.py:69–88`.)

**9. Pronto, certificação e entrega.** Entregue diff, matriz atualizada com estado → juiz → operação → persistido → retorno → prova, contrato de saída numerado, censo atualizado e comandos/resultados com suas fronteiras. Nenhuma linha violadora pode ser encerrada como “fora do aval”. Fonte: `aval_matriz_antes_do_opus_1110.txt`, itens 2–4 [nao verificado].

Execute as regressões locais e entregue ao verificador a suite ORM em ambiente de testes autorizado, incluindo as provas acima, BO voltas anteriores compatibilizadas, HX, portas, derivação, fábrica e selos SQL. Meça os cenários de 1 e 3 chamados e os callbacks; não transforme 145/219 em contagem certificada nem aumente tetos por conveniência. Se mover linhas, atualize somente os registros justificados no mesmo diff e mate a reversão do registro. (`app/ponto/tests/test_selo_chokepoint_escrita.py:180–199`, `:297`, `:399–439`; `app/core/tests/test_medida_turno_do_par.py:267–270`, `:535–545`.)

Os 37 testes de memória e 35 da catraca são resultados relatados da volta 4, não certificação desta volta. Suite ORM, SQL, concorrência e Opus final permanecem pendentes nos anexos. Fonte: `entrega4.txt`, seções 3–4 [nao verificado]. [nao verificado]

**10. Push/pouso.** Não pousar nem fazer push do BO nesta volta: primeiro entregar a construção revisável, depois certificação e Opus final com a matriz anexa. A abertura do Opus depende da matriz; a sequência de entrega é decisão de orquestração minha. Fonte: `aval_matriz_antes_do_opus_1110.txt`, item 4 [nao verificado]. [nao verificado]

O `!` de PUSH-E4A2 autoriza a reescrita e os 23 commits daquela outra obra; não é autorização de push deste BO. Não reescreva seus commits nem toque deploy por associação com aquele aval. Fonte: `aval_duas_respostas_push_bo_1110.txt`, resposta 1 [nao verificado]. [nao verificado]

## CENSO

Censo dos caminhos relevantes ao nascimento e aos campos envolvidos nesta cura; não declara inventário exaustivo de todas as mutações da aplicação. Commit de introdução do defeito e atribuição causal das sete DS: [nao verificado]. [nao verificado]

| Objeto | Escritores encontrados | Leitores e decisão |
|---|---|---|
| Motivo `batida_

[truncado em 20000 de 28756 caracteres]

## 11/11 00:51 · sessao 2d03e935d302 · BRIEF-COM-PROCEDENCIA · leitura · origem=Code

**BRIEF — próxima volta do construtor Codex — BO-DISPUTA-SEM-PERGUNTA**

**D0. Base e objetivo.** Trabalhe sobre a raia informada `e70f2e7ee` mais as mudanças ainda não commitadas descritas em `construtor14.r`. O anexo relata correções que a cópia ainda permite observar como defeitos: episódio implícito, seed de órfãos, análise após replay divergente, escrita própria da âncora e declaração pública sem pergunta. Primeiro confira o patch da 14; não declare essas curas certificadas apenas pelo relato. Aplicação e execução dos bytes finais: **[nao verificado]**. Referências da cópia: `app/chamados/services/disputa_emissao.py:500`, `:1275`, `:1459`, `:1562`, `:747`.

**D1. Escopo autorizado e limites.** A cura dos caminhos de exceção do gerador e pergunta estrangeira está aprovada pelo anexo `aval_duas_respostas_push_bo_1110.txt`, item 2; o aval das 22:16 resolve h2 e dona fechada e determina fechamento no escopo c11, sem nova lei. Não volte à fase de “somente censo”, nem reabra essas duas perguntas. Autorizações recebidas nos anexos: **[nao verificado]**.

Não altere as sete disputas existentes por shell, não execute reparo em produção e não misture esta obra com BO-0611-05 ou PUSH-E4A2. As sete ocorrências são o dado histórico recebido, não uma medição atual desta sessão. Fonte: `aval.txt`, “as 7 existentes nao se mexem por shell”; `aval_duas_respostas_push_bo_1110.txt`, separação dos itens 1 e 2: **[nao verificado]**. [nao verificado]

**D2. Contrato de saída que deve ficar provado.**

1. **Nascimento estrutural:** pai, disputa e pergunta pertencem ao mesmo ato transacional; exceção propagada desfaz o conjunto, preservando a batida anteriormente gravada. O teste deve testemunhar positivamente a inserção da pergunta antes de provocar a exceção. Referências: `app/chamados/services/disputa_emissao.py:623`, `:653`, `:670`, `:674`; lacuna da testemunha ORM em `app/chamados/tests/test_bo_disputa_sem_pergunta.py:55`.

2. **Pergunta estrangeira:** a FK da pergunta continua apontando para sua dona canônica; o motivo segue para essa dona. Ensaio local sem pergunta local não deixa nova disputa nem novo pai. Lote misto conserva somente os motivos efetivamente admitidos na local, sem seed estrangeiro. Não imponha exclusividade global por nome de motivo: perguntas locais do mesmo motivo em dias distintos podem justificar presença nas duas disputas. Referências: `app/chamados/models.py:3008`; `app/chamados/services/disputa_emissao.py:598`, `:606`, `:681`, `:1007`, `:1275`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:36`.

3. **Identidade conservada:** congele episódio, dia do turno, destino, opções e demais argumentos do fato antes de trocar a disputa recebida pela dona. Prove também o caso de episódio ausente: passar `None` não basta para impedir derivação pelo contexto da dona, pois a porta usa `campos.get(...) or derivador(...)`. Referências: `app/chamados/services/disputa_emissao.py:500`; `app/chamados/models.py:2866`, `:2875`.

4. **Divergência:** retorno com `criada=True` ou PK diferente da esperada não autoriza análise/cartório, agregação ou push daquele fato. Desfaça seus efeitos e preserve os efeitos válidos independentes previstos pelo contrato. A análise incondicional dos órfãos precisa desaparecer; a cura correspondente é relatada pela 14, ainda sem certificação nesta cópia. Referências: `app/chamados/services/disputa_emissao.py:1453`, `:1454`, `:1459`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:194`, `:221`; `construtor14.r`: **[nao verificado]**.

5. **Agregador único de admissão:** núcleo, órfãos, pendentes, declaração e âncora usam a mesma decisão de agregação. Elimine a união/save próprios da âncora e ajuste o censo para a estrutura resultante. Atualizar a lista permitida do gate, sozinho, não prova unicidade. Isso não exige fundir nesta volta todos os nascimentos nem as portas distintas de remoção e reabertura. Referências: `app/chamados/services/disputa_emissao.py:558`, `:1531`, `:1562`; `app/chamados/tests/test_porta_disputa_emissao.py:52`, `:83`, `:128`; `app/chamados/models.py:1971`, `:2126`.

6. **Declaração pública:** exija pergunta no contrato público antes de qualquer nascimento. Preserve os dois consumidores reais, sua costura e a ausência de push próprio. Atualize montagens de teste que usavam a chamada pública sem pergunta; não preserve um bypass produtivo para acomodá-las. Referências: `app/chamados/services/disputa_emissao.py:723`, `:734`, `:747`; `app/chamados/services/declaracao_texto.py:362`, `:650`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:397`.

**D3. Exceções já decididas: preservar, instrumentar e encaminhar.**

- **Sem ata:** continua nascendo aberta, sem pergunta e sem fechamento; preserve o aviso e publique I7b. **Com ata e nada a perguntar — h2:** continua nascendo e fechando vazia. Não aplique “não nasce” a h2 nesta obra. Referências: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`, `:58`, `:59`, `:63`, `:75`, `:78`; decisão explícita no anexo `aval_duas_leis_1110.txt`, item 1: **[nao verificado]**.
- Registre na RAIZ-DA-DISPUTA F1 a alternativa “h2 não nasce” junto da responsabilidade por fechar os chamados órfãos sem disputa. Leve os números recebidos — 300 vazias, 237 no mesmo minuto — como relato do aval; conteúdo da `sonda_h2.r`, não fornecido integralmente, e confirmação independente desses números: **[nao verificado]**.
- **Dona fechada:** mantenha o comportamento desta obra; acrescente trilha do motivo descartado e contador `motivo_em_dona_fechada`, esperado zero, com alarme e caso que morde. Não escolha reabertura, nova disputa ou Pauta. Encaminhe essa decisão à RAIZ F1 com o número efetivamente obtido; número atual: **[nao verificado]**. Fonte decisória: `aval_duas_leis_1110.txt`, item 2: **[nao verificado]**; ramo hoje silencioso em `app/chamados/services/disputa_emissao.py:563`.
- Preserve a reabertura que já ocorre pela ressuscitação/cartório, inclusive a nota quando há chamado-pai. Ela não autoriza o agregador a reabrir a dona por conta própria nem agrega automaticamente o motivo antes descartado. Referências: `app/chamados/models.py:1574`, `:2173`, `:2177`, `:2178`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:102`, `:133`, `:188`.

**D4. Contadores com escritor e leitor.** Preserve I7a como disputa **aberta sem nenhuma linha de pergunta**, esperado zero, dono ESTRUTURA; I7b publica a abstinência por falta de ata como INFO. Pergunta morta ainda é linha. Dia indeterminável não prova falta de ata; conjunto misto com algum dia com ata pertence à classificação estrutural. Referências: `app/chamados/management/commands/auditar_invariantes_chamados.py:62`, `:64`, `:68`, `:73`, `:74`, `:81`.

Para `motivo_em_dona_fechada`, declare unidade de contagem, escritor da trilha e consulta do vigia. A informação precisa sobreviver ao fim do processo; outro WARNING ou contador apenas em memória não satisfaz o aval. Reutilizar a trilha persistente existente é minha orientação de implementação; `core.observ.evento` grava `LogAuditoria`, mas é best-effort e seu retorno precisa ser considerado na prova. Referências: `app/core/observ.py:73`, `:100`, `:104`, `:140`, `:145`; exigência do novo contador no anexo das duas leis: **[nao verificado]**.

**D5. Fronteira transacional e desenho.** Inclua pendentes na unidade transacional do ato: hoje o wrapper pode chamá-los depois do atomic do núcleo, e o helper abre pergunta antes de agregar motivo/contexto. Preserve a batida fora do rollback da emissão e a ordem C → D → Ch → P, inclusive donas fechadas e o fecho alcançado pelas cascatas. Referências: `app/chamados/services/disputa_emissao.py:300`, `:311`, `:313`, `:623`, `:650`, `:830`, `:1167`, `:1173`, `:1178`; `app/chamados/models.py:2819`.

Complete a avaliação exigida de **resolver antes de escrever**, sem reentrada nem guarda de visitadas. Há juízes reutilizáveis para dia, universo e eleição canônica; isso indica cabimento sem mudar `models.py`, mas a integração ainda precisa ser demonstrada. Não substitua a eleição por consulta própria de `(motivo, dia)` ou por `.first()`/`.last()`. Referências: `app/chamados/services/fio.py:1608`, `:1784`, `:1225`; `app/chamados/models.py:2900`, `:2907`, `:2921`. Cabimento integrado: **[nao verificado]**.

A matriz deve concluir essa avaliação: se couber, cumpra a passagem de leitura das donas seguida de uma passada de escrita, conforme o aval; se houver impedimento concreto, registre-o com prova. As correções da 14 sobre identidade, admissão e efeitos continuam obrigatórias em qualquer desenho. Fonte: `aval_matriz_antes_do_opus_1110.txt`, item 3: **[nao verificado]**.

**D6. Push ao colaborador: preserve critérios e prove destinatário e momento.** Mantenha os critérios históricos descritos pelo opus: nascimento com seus motivos persistidos, agregação com os novos motivos, órfãos com `motivos_atualizados`. Não troque tudo por perguntas criadas/localmente retornadas. Motivo repetido em outro dia não ganha push pelo wrapper apenas por ter pergunta nova; órfãos conserva seu critério próprio. Referências: `app/chamados/services/disputa_emissao.py:832`, `:838`, `:1484`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:252`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:162`, `:173`; histórico descrito em `opus5d.txt`, item 3: **[nao verificado]**.

O ato deve conservar os deltas por dona, hoje descartados no encaminhamento estrangeiro, para que a matriz mostre explicitamente o destinatário e o critério aplicável. Minha decisão é aplicar o critério de agregação do wrapper à dona resultante quando houver novos motivos e ela permanecer aberta; isso não autoriza notificações adicionais indiscriminadas nos demais caminhos. Referências do defeito: `app/chamados/services/disputa_emissao.py:613`, `:614`, `:720`, `:823`, `:832`.

Garanta que efeitos de um ato desfeito não notifiquem. Minha orientação é entregar as notificações após confirmação da transação, mantendo os critérios de conteúdo acima e testando o estado persistido quando houver cascata. Há envio imediato dentro do atomic dos órfãos; vazamento externo efetivamente ocorrido: **[nao verificado]**. Referências: `app/chamados/services/disputa_emissao.py:1215`, `:1230`, `:1465`, `:1963`, `:1971`. Declaração continua sem push próprio: `app/chamados/services/disputa_emissao.py:734`.

**D7. Testes e mutantes exigidos.** Confira os oito casos novos relatados pela 14 e seus bytes finais. O arquivo `test_bo_disputa_volta5f_memoria.py` e seus resultados são evidência do anexo, ainda **[nao verificado]** nesta cópia; os defeitos anteriores estão nos endereços de D0. [nao verificado]

| Selo | Exigência |
|---|---|
| F1 | Local aberta com `motivos=[]`, `orfao_14h` estrangeiro e outro motivo local; nenhum seed indevido. Cobrir também ausência/recusa sem admissão de motivo. Contraexemplo: `app/chamados/services/disputa_emissao.py:980`, `:1007`, `:1275`. |
| F2 | Dois episódios distintos e contexto divergente da dona; conservar identidade e dia, incluindo ausência de episódio. `app/chamados/models.py:2866`; prova atual incompleta em `app/chamados/tests/test_bo_disputa_volta5_memoria.py:158`. |
| F3 | Retorno criado e PK divergente nos órfãos; nenhuma análise decorrente do fato recusado. `app/chamados/services/disputa_emissao.py:1454`, `:1459`. |
| F4 | Mutante restaurando união/save próprios da âncora precisa falhar. `app/chamados/services/disputa_emissao.py:1531`, `:1562`. |
| F5 | Chamada pública sem pergunta falha antes de escrita; ambos os consumidores reais continuam funcionando. `app/chamados/services/disputa_emissao.py:747`; `app/chamados/services/declaracao_texto.py:362`, `:650`. |
| F6 | Mutação montada uma vez, sítio pretendido validado e assertion comportamental no alvo. Falha de montagem não conta como mutante morto. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`, `:104`, `:401`. |
| F7 | A prova com a fonte atual duas vezes se chama repetibilidade. Paridade histórica exige fonte histórica distinta. `app/chamados/tests/test_bo_disputa_volta3_memoria.py:236`, `:245`, `:246`. |

Execute os controles e os mutantes relatados abaixo; se o desenho eliminar replay, adapte a montagem preservando a mordida contra segunda emissão e efeitos de retorno divergente. A lista vem de `construtor14.r`; resultado final: **[nao verificado]**.

```bash
red5f() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5f_memoria.py "$@"
}

red5f --item F1 --mutante seed_local
red5f --item F1 --mutante ausencia_admite_motivo
red5f --item F2 --mutante episodio_da_dona
red5f --item F3 --mutante analisa_divergente
red5f --item F4 --mutante ancora_save_proprio
red5f --item F5 --mutante publica_sem_pergunta
red5f --item F6
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py --mutante nasce_sem_pergunta
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py --mutante replay_insere
red5f --item F7 --mutante alega_paridade
```

Acrescente provas que removam a trilha e a inclusão no contador de dona fechada; cada remoção deve ser detectada. Prove zero em controle sem descarte e um no cenário de descarte. Para I7a/I7b, injete defeito estrutural e exija um em I7a; no cenário protegido sem ata, exija I7a=0 e I7b=1. Fontes: anexos `aval_duas_leis_1110.txt`, item 2, e `aval_duas_respostas_push_bo_1110.txt`, item 2: **[nao verificado]**; instrumentos em `app/chamados/management/commands/auditar_invariantes_chamados.py:73`.

Exija ORM para o lote misto F1, transação dos pendentes, receivers reais, ressuscitação com nota e entrega/cancelamento das notificações. Os doubles não certificam essas integrações. Preserve a fixture já corrigida — batidas, chamado, célula por último — e suas assertions sobre a ata real. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:32`, `:51`, `:53`, `:56`, `:64`; `app/chamados/signals.py:216`, `:238`, `:257`.

O alvo ORM F1 relatado pela 14 deve ser executado pelo ambiente de testes autorizado, nunca em produção; presença e execução desse alvo: **[nao verificado]**. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test chamados.tests.test_bo_disputa_volta5b_orfaos.OrfaosVolta5bTest.test_RED_F1_lote_misto_local_vazia_nao_ganha_seed_estrangeiro
```

Preserve os protegidos HX, PERGUNTA-NO-APP, reuso do pai/nota e releitura após gerador/pendentes, além das famílias BO de memória e ORM afetadas. Não reduza assertions para obter verde. Referências: `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:63`; `app/chamados/tests/test_hx_disputa_muda.py:24`, `:44`; `app/chamados/tests/test_pergunta_no_app.py:89`, `:103`; `app/chamados/tests/test_3b_reuso_746.py:23`; `app/chamados/tests/test_bo_disputa_volta8_memoria.py:162`, `:205`.

**D8. Pronto, pouso e push git.** Entregue patch revisável da 14 mais os resíduos desta volta, matriz completa, contrato numerado, censo atualizado e evidência dos controles/mutantes nos mesmos bytes finais. Separe resultados de memória, ORM e produção; suíte inteira, integridade histórica dos protegidos e paridade com `6c0517eb9` permanecem **[nao verificado]** enquanto não houver prova correspondente. A comparação disponível usa a mesma fonte: `app/chamados/tests/test_bo_disputa_volta3_memoria.py:245`.

**Não libera pouso nem push agora.** Submeta ao preverificador; o opus final recebe a matriz e os achados anteriores com estado e prova, inclusive as decisões das duas leis. A exigência de matriz antes do opus vem de `aval_matriz_antes_do_opus_1110.txt`, item 4: **[nao verificado]**. A liberação atual é decisão minha diante dos contratos e provas pendentes acima. [nao verificado]

O `!` de PUSH-E4A2 autoriza aquela reescrita e seus 23 commits; não é autorização de push deste BO. Autorização específica para push/deploy desta obra: **[nao verificado]**. Fonte: `aval_duas_respostas_push_bo_1110.txt`, item 1: **[nao verificado]**. [nao verificado]

**CENSO**

Os endereços abaixo descrevem a cópia; os efeitos posteriores do patch 14 precisam de conferência. Certificação do patch final: **[nao verificado]**.

| Estado/campo | Escritores identificados | Leitores/efeitos identificados |
|---|---|---|
| Nascimento de DS | Núcleo `app/chamados/services/disputa_emissao.py:670`; órfãos `:1299`; âncora `:1554` | Wrapper `:817`; HX/h2 `:1444`, `:1474`, `:1477`; âncora `:1575`, `:1577` |
| Admissão de motivos/contexto | Agregador `app/chamados/services/disputa_emissao.py:569`, `:572`, `:575`; escrita própria da âncora `:1562` | Admissão local `:598`; push `:832`, `:1484`; episódio `app/chamados/models.py:3463` |
| Remoção/contexto específico | `app/chamados/models.py:1966`, `:2063`, `:2104`, `:2122` | Foto `app/api/foto_recebida.py:63`; resolução `app/chamados/services/disputa_emissao.py:344`; auditor `app/chamados/management/commands/auditar_invariantes_chamados.py:31` |
| Fechamento/reabertura | `app/chamados/models.py:1932`, `:2149`, `:2173`; reabertura admin pode adicionar motivo em `:2159` | Agregador `app/chamados/services/disputa_emissao.py:563`; S127 `app/chamados/juizes.py:2316`; app `app/api/views.py:1059` |
| Dona de P | Nascimento `app/chamados/models.py:3014`; reuso preserva dona em `:3008` | Roteamento `app/chamados/services/disputa_emissao.py:606`; app `app/api/views.py:1063` |
| Destino de P | Reuso `app/chamados/models.py:2982`; costura `app/chamados/services/fio.py:925` | Autorização `app/chamados/services/fio.py:598`; escolha do card `app/api/views.py:1061` |
| Trilha/contadores | Porta persistente `app/core/observ.py:100`, `:140`; ramo fechado hoje retorna em `app/chamados/services/disputa_emissao.py:563` | I7a/I7b `app/chamados/management/commands/auditar_invariantes_chamados.py:73`; novo leitor de descarte: **[nao verificado]** |

O desligamento chama a porta de fechamento; o comentário antigo que o descreve como escritor bulk não deve ser tratado como estado atual. Referência: `app/colaboradores/services/desligamento.py:132`, `:142`. Commit introdutor do bug original e exaustividade sobre formas dinâmicas de escrita: **[nao verificado]**.

A matriz final deve preencher as seis colunas abaixo com o persistido real e o teste executado, sem promover esta especificação a resultado de teste. Exigência: `aval_matriz_antes_do_opus_1110.txt`, item 2: **[nao verificado]**. [nao verificado]

| Estado | Juiz | Operação exigida | Persistido exigido | Retorno | Prova/endereço |
|---|---|---|---|---|---|
| a. Fato novo | Resolvedor/porta | Nascer atomicamente se houver pergunta | Pai+DS+P ou nenhum | DS ou ausência | `app/chamados/services/disputa_emissao.py:674`, `:681` |
| b. Canônica em outra aberta | Identidade canônica | Agregar na dona | P mantém dona; sem nascimento local vazio | Dona informada ao ato | `app/chamados/models.py:3008`; `app/chamados/services/disputa_emissao.py:613` |
| c. Canônica em fechada | Porta + aval das duas leis | Preservar comportamento; trilhar descarte | Sem reabertura nova pelo agregador | Recusa observável | `app/chamados/services/disputa_emissao.py:563`; contador final **[nao verificado]** |
| d. Ciclo A↔B | Identidade de cada fato | Roteamento sem reentrada | Sem nascimento vazio nem duplicação indevida | Donas reais | `app/chamados/tests/test_bo_disputa_volta4_memoria.py:36` |
| e. Exceção após batida | Transação | Desfazer emissão | Batida preservada | Exceção tratada pela borda existente | `app/chamados/tests/test_bo_disputa_sem_pergunta.py:32`, `:55` |
| f. Sem ata | Juiz da ata | Abster-se e acender luz | DS aberta, zero P | DS | `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:59` |
| g. Grade | Censo atual | Não ressuscitar helper removido | Sem nascimento por esse helper | Não aplicável ao helper removido | `app/ponto/management/commands/reconciliar_grade.py:412` |
| h. Órfãos | Ata/classificação/porta | Lote misto e repasse | Motivos locais corretos; costura válida | DS/None/recusa declarada | `app/chamados/services/disputa_emissao.py:1195`, `:1444` 

[truncado em 20000 de 24224 caracteres]

_3574 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
