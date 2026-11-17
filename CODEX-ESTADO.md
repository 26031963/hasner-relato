# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 11:39** · publicado em **17/11/2026 11:39** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 11:39 · sessao 6423bcf443b1 · CORTE-FOLHA-ESPELHO · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1 e 3:** faltam fechar, respectivamente, o universo dos fatos da disputa e o significado de competência aberta. O item 2 confere como efeito proposto para o pré-desenho. (`app/chamados/juizes.py:75`; `app/chamados/models.py:2528`; `app/ponto/services/fechamento.py:1523`; anexo textual sem arquivo:linha [nao verificado])

**Item 1 — CORTE**

1. **As premissas conferem? Parcialmente.** Hoje são perguntas diferentes: o export consulta células da janela e suas exceções; o PDF consulta qualquer disputa da pessoa com `fechada_em=NULL`, sem competência nem data do fato. O wrapper do export ignora os três primeiros argumentos e delega à retenção por célula. (`app/folha/export.py:1247`; `app/folha/export.py:1273`; `app/folha/export.py:1301`; `app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`)

   A janela pertence a `janela_fechamento → periodo_apuracao`, usando o corte da empresa, limitado a 2–28; as bordas são inclusivas. Não é mês civil nem necessariamente corte 21. (`app/ponto/janelas.py:10`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:54`; `app/folha/export.py:1265`)

   **Não há um dia escalar próprio da disputa:** ela reúne perguntas, cada uma podendo ter `data_evento`. Para pergunta de fato, esse campo representa o dia do turno; conversa grava o dia civil da conversa. A atribuição canônica passa por `gravar_dia_do_fato`, após resolução pelo juiz do turno. (`app/chamados/models.py:2521`; `app/chamados/models.py:2888`; `app/chamados/models.py:2900`; `app/chamados/services/fio.py:837`; `app/chamados/services/fio.py:1698`)

2. **Colide com lei ou aval?** Unificar os leitores concorda com LEI-AKITA 2. Entretanto, o SIM não pode ser simplesmente “aberta + algum dia dentro”: a L-033 mantém pagável o dia com entrada e saída batidas, faltando somente intervalo, com pergunta aberta. Hoje essa exceção existe no retentor da célula, mediante protestos exatamente `{'FURO_PARCIAL'}` e conferência da ata pelo juiz de marcos; não está no bloqueador global do PDF. (`CLAUDE.md:20`; `app/docs/LEIS.md:81`; `app/folha/export.py:1150`; `app/folha/export.py:1173`; `app/folha/export.py:1293`; `app/chamados/services/disputa_emissao.py:40`)

   Os juízes registrados para pergunta viva, fechamento da disputa, tarefa e respostas sem veredito respondem perguntas distintas. O helper global existente tampouco responde ao novo recorte temporal. Acrescentar janela e exceção muda predicado e universo; não cabe apresentá-lo como mera declaração de autoridade já existente com comportamento idêntico. (`app/core/juizes.py:291`; `app/core/juizes.py:304`; `app/core/juizes.py:308`; `app/core/juizes.py:329`; `CLAUDE.md:279`)

3. **O literal fecha? Não.** Precisam ficar determinados:

   - **Vários fatos, dentro e fora:** o resolvedor do chamado-pai devolve o **menor** dia das perguntas de fato. Usá-lo para perguntar “há algum dentro?” pode esconder outro fato dentro da janela. (`app/chamados/juizes.py:75`; `app/chamados/juizes.py:85`)
   - **Quais perguntas contam:** todas da disputa aberta, somente vivas ou somente sem decisão? “Disputa aberta” hoje testa apenas `fechada_em`; C7 consulta separadamente perguntas vivas e respostas sem veredito. (`app/chamados/models.py:1888`; `app/chamados/models.py:1928`)
   - **Sem dia resolvível:** o campo permite NULL e o resolvedor pode retornar `None`; conversa não fornece dia de fato. O destino desses casos não foi decidido pelo dado fornecido. (`app/chamados/models.py:2528`; `app/chamados/juizes.py:79`; `app/chamados/juizes.py:85`; [nao verificado])
   - **Exceção L-033:** “preservada” precisa compor a resposta única; deixá-la somente no antigo ramo da célula permite que o novo bloqueio a contorne. (`app/folha/export.py:1173`; `app/folha/export.py:1293`)

4. **Toca exportada, dinheiro ou `!` fora do declarado?** Construído, o corte muda quem pode produzir linhas financeiras, pois o montador só admite os classificados como ENTRA. Isso está no efeito pretendido; não autoriza alterar valores, batidas ou o gravado de exportadas. Trocar o helper global também alcançaria leitores além dos dois declarados, como API e permissão de edição. (`app/folha/export.py:716`; `app/folha/export.py:726`; `CLAUDE.md:265`; `app/api/views.py:1336`; `app/colaboradores/mixins.py:307`)

**Item 2 — EFEITO**

1. **As premissas conferem?** O mecanismo existente permite excluir a pessoa pelo resultado `status='fora'` com motivo e detalhe; o montador omite suas linhas. No PDF, o bloqueio auditável provoca erro; o lote captura a falha e tenta o informacional. O efeito proposto é novo, mas esses caminhos existem. (`app/folha/export.py:837`; `app/folha/export.py:970`; `app/folha/export.py:726`; `app/relatorios/pdf_espelho.py:1087`; `app/relatorios/services.py:244`; `app/relatorios/services.py:250`)

2. **Colide com lei ou aval?** Não identifiquei colisão própria do efeito, desde que o SIM respeite a L-033 e o limite de competência definido no item 3. A exclusão deve continuar passando por `classificar_export`, autoridade registrada de quem entra no TXT. (`app/docs/LEIS.md:81`; `app/core/juizes.py:329`; `app/folha/export.py:716`)

3. **O literal fecha?** Fecha como direção para o pré-desenho, com duas ressalvas:

   - “Com NÃO, vale só o veredito da célula” **não descreve literalmente toda a classificação atual**: existem rescisão, código Domínio, férias com batida, cadastro e homologação, além da porta global. A leitura compatível com “como hoje” é: NÃO remove apenas este novo impedimento por disputa, preservando os demais crivos. (`app/folha/export.py:866`; `app/folha/export.py:872`; `app/folha/export.py:877`; `app/folha/export.py:959`; `app/folha/export.py:974`; `app/folha/views.py:128`)
   - Chave, texto e orientação do motivo novo ainda não estão especificados no anexo [nao verificado]. São entregas do pré-desenho; precisam alcançar apresentação e encaminhamento: hoje motivo diferente de `furo_espelho` recebe dono `admin`, e motivo sem mapeamento de tipo cai em `sem_causa`. Isso pode orientar mal uma disputa que espera resposta do colaborador. (`app/folha/retrato_folha.py:374`; `app/folha/retrato_folha.py:388`; `app/folha/fora_da_folha.py:50`)

   NÃO também não garante PDF auditável: o lote cai no informacional por qualquer exceção da tentativa auditável. (`app/relatorios/services.py:244`)

4. **Toca exportada, dinheiro ou `!` fora do declarado?** Excluir do TXT muda os eventos enviados e pode mudar o conjunto que recebe aprovação automática. Não significa zerar rubricas nem reescrever o fechamento. Esses efeitos precisam constar do pré-desenho, preservando a proteção do gravado exportado. (`app/folha/export.py:726`; `app/folha/views.py:215`; `app/folha/views.py:246`; `CLAUDE.md:411`)

**Item 3 — COMPETÊNCIA ENTREGUE / ABERTA**

1. **As premissas conferem?** ENTREGUE tem autoridade precisa: K6, existência de `ExportacaoDominio` vigente e não invalidada para empresa, mês e ano. Não equivale a `FechamentoMensal.status='exportado'`, aprovação, tranca ou holerite; TXT vigente invalidado sem substituta deixa de entregar. K9, separado, compõe K6 com holerite publicado para proteger a célula. (`app/folha/export.py:556`; `app/folha/export.py:591`; `app/folha/export.py:597`; `app/docs/CORTES.md:342`; `app/docs/CORTES.md:343`)

2. **Colide com lei ou aval?** Preservar o gravado exportado concorda com a L-092 consolidada. Porém, “nada muda” deve limitar **esta obra**, sem proibir correções pelas portas vigentes: a L-113 alterou a L-092, permitindo recalcular e refazer exportação com as garantias exigidas; a regra consolidada mantém `!` e porta específica para mover o gravado exportado. (`app/docs/LEIS.md:125`; `app/docs/LEIS.md:155`; `CLAUDE.md:411`; `CLAUDE.md:418`)

3. **O literal fecha? Não.** “Competência aberta” admite recortes diferentes no código:

   | Recorte | Significado atual |
   |---|---|
   | Marco `aberta` | Nenhum marco aprovada/exportada/trancada/paga verdadeiro. (`app/ponto/services/fechamento.py:1523`; `app/ponto/services/fechamento.py:1543`) |
   | Não entregue por K6 | Ausência de TXT vigente não invalidado. (`app/folha/export.py:591`) |
   | Não trancada | Ausência da tranca vigente; aprovação e exportação são outros marcos. (`app/ponto/services/fechamento.py:1473`) |
   | Escrita permitida por F6 | Não haver fechamento aprovado/exportado da pessoa na competência do dia. (`app/ponto/janelas.py:85`; `app/ponto/janelas.py:87`) |

   Falta escolher explicitamente o alcance para aprovada sem TXT, trancada sem K6, paga só por holerite, competência antiga sem entrega e TXT invalidado sem substituta. O anexo não fornece essa escolha [nao verificado].

4. **Toca exportada, dinheiro ou `!` fora do declarado?** O limite proposto evita movimento do gravado, mas isso sozinho não protege o comportamento dos leitores históricos: a geração recalcula objetos em leitura, e o PDF consulta a disputa atual. É necessário aplicar o recorte também nessas leituras; nenhum `save` financeiro precisa ocorrer para uma nova regra mudar quem entra num TXT regenerado ou qual modalidade de PDF é emitida. (`app/folha/views.py:207`; `app/folha/views.py:212`; `app/folha/views.py:268`; `app/relatorios/pdf_espelho.py:1085`)

**Escritores e leitores dos campos determinantes**

| Campo/valor | Quem escreve | Quem lê neste corte |
|---|---|---|
| Disputa: `colaborador`, `chamado`, `motivos`, `contexto` | Emissão cria e agrega; há também nascimentos por órfãos e âncora. (`app/chamados/services/disputa_emissao.py:550`; `app/chamados/services/disputa_emissao.py:576`; `app/chamados/services/disputa_emissao.py:601`; `app/chamados/services/disputa_emissao.py:1137`; `app/chamados/services/disputa_emissao.py:1372`) | Helper global lê colaborador; resolução do dia do chamado pode consultar contexto e perguntas. (`app/chamados/services/disputa_emissao.py:40`; `app/chamados/juizes.py:101`; `app/chamados/juizes.py:134`) |
| Disputa: `aberta_em` | ORM, `auto_now_add`. (`app/chamados/models.py:1820`) | Não integra o predicado atual do PDF nem o retentor do export. (`app/chamados/services/disputa_emissao.py:40`; `app/folha/export.py:1273`) |
| Disputa: `fechada_em` / aberta | `fechar` preenche; reaberturas admin e sistema zeram. (`app/chamados/models.py:2013`; `app/chamados/models.py:2230`; `app/chamados/models.py:2254`) | Property `aberta` e helper usado pelo PDF. (`app/chamados/models.py:1888`; `app/chamados/services/disputa_emissao.py:41`) |
| Pergunta: `data_evento` | Atribuição em `gravar_dia_do_fato`; nascimento, costura e re-chaveamento persistem. (`app/chamados/services/fio.py:837`; `app/chamados/models.py:3123`; `app/chamados/services/fio.py:926`; `app/chamados/services/fio.py:1090`) | Resolvedor do dia e juiz do turno da linha; os bloqueadores atuais de export/PDF não o consultam diretamente. (`app/chamados/juizes.py:81`; `app/chamados/services/fio.py:1698`; `app/folha/export.py:1273`; `app/chamados/services/disputa_emissao.py:40`) |
| Pergunta: `conversa`, `celula` | Portas de abertura distinguem fato/conversa; nascimento e re-chaveamento vinculam célula. (`app/chamados/models.py:2894`; `app/chamados/models.py:2906`; `app/chamados/models.py:3124`; `app/chamados/services/fio.py:1085`) | Resolvedor exclui conversa e usa célula como alternativa ao dia ausente. (`app/chamados/juizes.py:79`; `app/chamados/juizes.py:83`) |
| Empresa: `dia_inicio_competencia`; janela resultante | Formulário alimenta portas de criar/atualizar empresa; janela é derivada. (`app/colaboradores/views.py:515`; `app/colaboradores/views.py:542`; `app/colaboradores/views.py:557`; `app/colaboradores/services/estrutura.py:13`) | `corte_da_empresa`, `janela_fechamento`, export e lote PDF. (`app/ponto/janelas.py:50`; `app/ponto/janelas.py:57`; `app/folha/export.py:1306`; `app/relatorios/services.py:214`) |
| Célula: `veredito`, `protestos`, `ata`; isenção L-033 | Porta `lavrar_veredito`; isenção é derivada, sem reescrever a célula. (`app/ponto/portas/celula.py:1032`; `app/ponto/portas/celula.py:1090`; `app/folha/export.py:1173`) | Retentor e juiz de marcos usado na exceção. (`app/folha/export.py:1273`; `app/folha/export.py:1286`; `app/folha/export.py:1150`) |
| Nova resposta única SIM/NÃO | Escritor/derivador e endereço ainda não definidos no anexo [nao verificado]. | Leitores pretendidos: export e espelho PDF; leitores atuais seguem distintos. (`app/folha/export.py:968`; `app/relatorios/pdf_espelho.py:1085`) |
| Export: `status`, `motivo`, `detalhe`, `travas_aprovacao` | `classificar_export`. (`app/folha/export.py:837`; `app/folha/export.py:968`) | Montador, aprovação automática e painel. (`app/folha/export.py:716`; `app/folha/views.py:244`; `app/folha/views.py:330`) |
| Rótulo e orientação; retrato `dono`, `acoes`, `tipo`, `saldo`, `dias`, `datas` | Rótulos em `MOTIVO_RH`; ações vêm da tarefa; `montar_linhas` monta projeção, e comando grava retrato. (`app/folha/views.py:14`; `app/folha/fora_da_folha.py:217`; `app/folha/retrato_folha.py:1237`; `app/folha/management/commands/lavrar_retrato_folha.py:77`) | Tela lê retrato; PDF de retidos lê itens e rótulos. A orientação nova ainda não tem texto definido [nao verificado]. (`app/folha/views_retrato.py:278`; `app/folha/views.py:859`) |
| Modalidade do espelho e registro auditável | Bloqueio decide admissão; lote escolhe tentativa informacional; gerador auditável grava registro/hash. (`app/relatorios/pdf_espelho.py:1113`; `app/relatorios/services.py:244`; `app/relatorios/pdf_base.py:152`) | Lote agrega bytes; informacional segue seu gerador próprio. (`app/relatorios/services.py:241`; `app/relatorios/services.py:250`) |
| Exportação: empresa/mês/ano, conteúdo, hash, autoria e instante | `ExportacaoDominio.registrar`, chamado pelo gerador; instante pelo ORM. (`app/folha/models.py:80`; `app/folha/models.py:182`; `app/folha/views.py:297`) | K6 lê tripla e vigência; desempate usa instante e PK. (`app/folha/export.py:581`; `app/folha/export.py:588`; `app/folha/models.py:220`) |
| Exportação: invalidação, substituta, vigência e ENTREGUE | `invalidar` grava invalidação/substituta; `anotar_vigencia` e K6 derivam resultados. (`app/folha/models.py:158`; `app/folha/models.py:162`; `app/folha/models.py:224`; `app/folha/export.py:591`) | K6 e `marcos_da_competencia`. (`app/folha/export.py:591`; `app/ponto/services/fechamento.py:1537`) |
| Fechamento: status; tranca/reabertura; marco aberta | Portas de aprovar/reabrir e trancar/reabrir período; marco é derivado. (`app/ponto/services/fechamento.py:1335`; `app/ponto/services/fechamento.py:1349`; `app/ponto/services/fechamento.py:1400`; `app/ponto/services/fechamento.py:1432`; `app/ponto/services/fechamento.py:1543`) | F6, K4/K7 e marcos; não substituem K6. (`app/ponto/janelas.py:87`; `app/ponto/services/fechamento.py:1470`; `app/ponto/services/fechamento.py:1480`; `app/folha/export.py:591`) |

Os quantitativos individuais, causas dos dias irresolvíveis, valores financeiros e distribuição por empresa não podem ser recuperados do resumo raspado fornecido; não atribuo essas características aos 49 ou aos 58 [nao verificado].

VEREDITO por item: 1 PROCEDE; 2 CONFERE; 3 PROCEDE

## 17/11 11:38 · sessao 0e5cc5138206 · - · - · origem=Ronald (tmux)

Censo dos indiretos, agrupado pela pergunta herdada. **Estes locais hoje perguntam “entra/retém pela célula”; não perguntam existência de disputa aberta.** O novo juiz de disputa só os alcançará se compuser `classificar_export`; migrá-los individualmente duplicaria a pergunta. (`app/folha/export.py:968`, `app/folha/export.py:1307`)

| Leitor/callsite | Efeito da resposta atual |
|---|---|
| `app/folha/export.py:714` | Montador usa `status=entra`; ignora pessoas fora. (`app/folha/export.py:716`, `app/folha/export.py:726`) |
| `app/folha/views.py:212` | Gerar TXT classifica os objetos recalculados em memória; admissão no TXT e aprovação automática têm respostas separadas. (`app/folha/views.py:215`, `app/folha/views.py:244`) |
| `app/folha/views.py:331` | Preview conta entram/fora, agrupa todos motivos e mostra detalhe; K6 gera apenas aviso de exportação anterior. (`app/folha/views.py:332`, `app/folha/views.py:340`, `app/folha/views.py:354`, `app/folha/views.py:403`) |
| `app/folha/views.py:589` | Raio X separa furo, férias, rescisão e complemento de outros motivos. (`app/folha/views.py:591`, `app/folha/views.py:600`) |
| `app/folha/views.py:858` | `retidos_pdf` entrega PDF dos classificados fora. (`app/folha/views.py:840`) |
| `app/folha/validacao_pdf.py:65` | PDF de validação monta o mesmo TXT com esses itens e o traduz. (`app/folha/validacao_pdf.py:66`, `app/folha/validacao_pdf.py:68`) |
| `app/folha/porta_export.py:469` | Medição da porta usa apenas ENTRA como universo das comparações espelho/fechamento; não é um bloqueio de disputa próprio. (`app/folha/porta_export.py:470`, `app/folha/porta_export.py:476`) |
| `app/folha/services/vigia_txt.py:231` | Vigia compara TXT vigente gravado com TXT que seria montado hoje; invalidado sem substituto vira INCOMPARÁVEL. (`app/folha/services/vigia_txt.py:220`, `app/folha/services/vigia_txt.py:232`, `app/folha/services/vigia_txt.py:244`, `app/folha/services/vigia_txt.py:254`) |
| `app/folha/fora_da_folha.py:147` | Conta entram e fora por motivo; só `furo_espelho` ganha carga de dias/tarefa. (`app/folha/fora_da_folha.py:151`, `app/folha/fora_da_folha.py:156`, `app/folha/fora_da_folha.py:165`) |
| `app/folha/fora_da_folha.py:273` | `o_que_segura` retorna situação/motivo do export e lista diária enriquecida. (`app/folha/fora_da_folha.py:275`, `app/folha/fora_da_folha.py:302`) |
| `app/folha/previa.py:43` | Prévia de proposta cruza veredito com células/perguntas da janela. (`app/folha/previa.py:50`, `app/folha/previa.py:64`) |
| `app/ponto/services/fechamento_lista.py:73` | Lista de fechamento lê tique de entrada e travas de aprovação; é alias `_classificar`. (`app/ponto/services/fechamento_lista.py:100`, `app/ponto/services/fechamento_lista.py:102`) |
| `app/ponto/views.py:1299` | Aprovação em lote lê somente `travas_aprovacao`, aplica seleção e decide quais aprovar; disputa não é veto atual. (`app/ponto/views.py:1301`, `app/ponto/views.py:1311`, `app/ponto/views.py:1328`) |
| `app/ponto/views.py:1412` | Localiza aprovados com trava `conta_vazia` para reabertura. (`app/ponto/views.py:1413`, `app/ponto/views.py:1426`) |
| `app/ponto/views.py:1509` | Aprovação individual lê apenas travas do objeto recalculado e recusa ou aprova. (`app/ponto/views.py:1511`, `app/ponto/views.py:1513`, `app/ponto/views.py:1518`) |
| `app/ponto/services/dias_em_aberto.py:104` | Lista dias em aberto somente de quem entra no TXT. (`app/ponto/services/dias_em_aberto.py:107`, `app/ponto/services/dias_em_aberto.py:112`) |
| `app/relatorios/views.py:1660` | Ranking de furos publica coluna Folha APTO/motivo, inclusive PDF. (`app/relatorios/views.py:1668`, `app/relatorios/views.py:1671`, `app/relatorios/views.py:1677`) |
| `app/relatorios/views_pend.py:58` | Pendências de cadastro selecionam só FORA por motivos de cadastro conhecidos; hoje apenas sem código Domínio. (`app/relatorios/views_pend.py:48`, `app/relatorios/views_pend.py:59`, `app/relatorios/views_pend.py:68`) |
| `app/api/views_mensageria.py:283` | API devolve total, entram, retidos e agrupamento por motivo com exemplos; pode servir snapshot em ramo anterior. (`app/api/views_mensageria.py:270`, `app/api/views_mensageria.py:288`, `app/api/views_mensageria.py:299`) |
| `app/inteligencia/calculadores.py:210` | Métrica da competência anterior conta bloqueios e exemplos por motivo. (`app/inteligencia/calculadores.py:206`, `app/inteligencia/calculadores.py:217`) |
| `app/inteligencia/calculadores.py:271` | Métrica da competência atual conta entram/total e bloqueios. (`app/inteligencia/calculadores.py:267`, `app/inteligencia/calculadores.py:277`, `app/inteligencia/calculadores.py:281`) |
| `app/folha/management/commands/simular_folha.py:133` | Fotografia de TXT, hash, fora por motivo e retidos; pode usar lido ou gravado. (`app/folha/management/commands/simular_folha.py:134`, `app/folha/management/commands/simular_folha.py:146`, `app/folha/management/commands/simular_folha.py:155`) |
| `app/relatorios/management/commands/cartao_x_txt_divergentes.py:53` | ENTRA define universo de comparação cartão/TXT e contador de dias em aberto. (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:57`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:64`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:68`) |
| `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:58` | ENTRA define universo da comparação de conteúdo PDF/espelho, via coletores, não geração auditável. (`app/relatorios/management/commands/pdf_x_espelho_divergentes.py:61`, `app/relatorios/management/commands/pdf_x_espelho_divergentes.py:73`) |
| `app/ponto/management/commands/e6_oraculo.py:309` | Com `--so-txt`, restringe universo aos ENTRA. (`app/ponto/management/commands/e6_oraculo.py:307`, `app/ponto/management/commands/e6_oraculo.py:313`) |
| `app/ponto/management/commands/folga_que_sumiu.py:82` | Marca casos/valor de folga sumida que chegam ao TXT. (`app/ponto/management/commands/folga_que_sumiu.py:84`, `app/ponto/management/commands/folga_que_sumiu.py:96`) |
| `app/ponto/management/commands/aplicar_09_corte_b.py:190`, `app/ponto/management/commands/aplicar_09_corte_b.py:273` | Fotografa status/motivo antes/depois do corte de cálculo. |
| `app/ponto/management/commands/desvio_o68b.py:191`, `app/ponto/management/commands/desvio_o68b.py:217` | Fotografa status/motivo e reporta mudança após reversão. (`app/ponto/management/commands/desvio_o68b.py:218`) |
| `app/ponto/management/commands/celula_veredito_velho.py:82`, `app/ponto/management/commands/celula_veredito_velho.py:152` | Fotografa classificação antes/depois de recalcular células/fechamentos. |
| `app/ponto/management/commands/diff_reclassificar_partido.py:153`, `app/ponto/management/commands/diff_reclassificar_partido.py:209` | Fotografa classificação antes/depois da reclassificação/recalculo de escala. |

Leitores diretos da retenção da célula, além do export:

| Callsite | Efeito |
|---|---|
| `app/folha/fora_da_folha.py:121` | `_dias_que_retem`: motivos e dias para agrupamento, retrato e orientação. (`app/folha/fora_da_folha.py:165`) |
| `app/folha/previa.py:170` | Decide se os dias propostos cobrem **todos** os dias que retêm; preserva L-033/data passada/admissão/demissão herdados. (`app/folha/previa.py:171`) |
| `app/folha/services/vigia_pausa.py:125` | Conta barrados, retidos só por pausa e livres com dia só de pausa, conferindo exceção L-033. (`app/folha/services/vigia_pausa.py:127`, `app/folha/services/vigia_pausa.py:128`, `app/folha/services/vigia_pausa.py:133`) |
| `app/ponto/services/fechamento.py:1067` | Alias `_mre`: materializa `FechamentoMensal.motivos_espelho`; não é critério novo de disputa. (`app/ponto/services/fechamento.py:1066`) |

Cascas do PDF, todas herdando a pergunta global atual da geração auditável:

- Service `app/relatorios/services.py:70` → gera bytes/registro e eventualmente anexa ao chamado. (`app/relatorios/services.py:74`, `app/relatorios/services.py:88`)
- Lote `app/relatorios/services.py:237` → auditável; exceção → informacional. (`app/relatorios/services.py:244`, `app/relatorios/services.py:250`)
- Job `app/relatorios/lote_espelho.py:205` → chama lote e conta informacionais para o resultado do job. (`app/relatorios/lote_espelho.py:227`)
- Anexar ao chamado `app/relatorios/views.py:1003` → erro e redirect em disputa. (`app/relatorios/views.py:1007`)
- Avulso competência `app/relatorios/views.py:1127` → formulário com erro em disputa. (`app/relatorios/views.py:1131`)
- Avulso período livre `app/relatorios/views.py:1098` → sempre informacional, não faz essa pergunta. (`app/relatorios/views.py:1064`)

Interação isento/desligado:

- O retentor da célula sai com lista vazia para `isento_ponto(colab)` ou `esta_desligado(colab)` antes de percorrer a janela. (`app/folha/export.py:1205`, `app/folha/export.py:1207`, `app/folha/export.py:1211`)
- Desligado sem decisão aparece pelo ramo `informativo_rescisao`, explicitamente sem retenção; a lei no código diz “não retém ... em NENHUM dia da competência”. (`app/folha/export.py:1197`, `app/folha/export.py:1216`, `app/folha/export.py:1225`)
- Rescisão do export vem antes e pode retirar a pessoa por módulo próprio, independente de disputa. (`app/folha/export.py:866`)
- Portanto um novo bloqueio de disputa que só preserve L-033, mas pule as exclusões isento/desligado, exige esclarecer a interação com essas leis existentes; não assumir revogação implícita [nao verificado].

## 17/11 11:37 · sessao 7d012196cc1a · - · - · origem=Ronald (tmux)

**Item 2: CONFERE.** O efeito SIM constitui a nova política submetida; chave, texto do motivo e orientação são pendências do pré-desenho, não contradições que exijam devolução [nao verificado — literal fornecido].

Leia o NÃO como ausência **deste impedimento por disputa**, preservando os demais crivos atuais; “só célula” não descreve literalmente toda a classificação existente. (`app/folha/export.py:837`, `app/folha/export.py:968`, `app/folha/export.py:716`)

## 17/11 11:37 · sessao b82be6056e62 · - · - · origem=Ronald (tmux)

Censo de apresentação/API e eventos, com `A = DisputaSupervisao.fechada_em IS NULL`; `aberta` é exatamente isso, sem olhar estado do chamado (`app/chamados/models.py:1886-1888`).

| Leitor | Predicado/data | Uso da resposta |
|---|---|---|
| Helper público | `colaborador=... AND A`, `.first()`, sem datas (`app/chamados/services/disputa_emissao.py:34-42`) | Retorna disputa ou `None`; reexportado por `chamados.models` (`app/chamados/models.py:4260`). |
| Helper do ponto | Mesmo helper; colaborador `None` retorna `None` (`app/ponto/services/disputa.py:18-31`) | Nome sugere bloqueio, execução só retorna disputa (`app/ponto/services/disputa.py:29-31`). |
| Espelho HTML | Helper, somente quando colaborador e modo não gestor; sem recorte da competência (`app/ponto/views.py:140-144`) | Mostra banner e continua exibindo cálculos (`app/ponto/views.py:180-183`; `app/templates/ponto/espelho.html:13-18`). |
| API espelho | Helper; sem data (`app/api/views.py:1333-1336`) | Só sinaliza `em_disputa`, ID e abertura; calcula normalmente (`app/api/views.py:1333-1334`, `app/api/views.py:1670-1672`). |
| Gate cadastro/benefício | POST + colaborador + helper; isento passa; sem data (`app/colaboradores/mixins.py:300-312`) | Recusa com mensagem e redirecionamento (`app/colaboradores/mixins.py:308-312`). Afeta transferência de posto, conta bancária, lançamento e cancelamento de benefício (`app/colaboradores/views.py:1394`, `app/colaboradores/views.py:1641`, `app/colaboradores/views.py:1807`, `app/colaboradores/views.py:1915`). Reativar e desligar são explicitamente isentos (`app/colaboradores/views.py:1442`, `app/colaboradores/views.py:1463`). |
| Painel situacional lote | `A AND colaborador_id IN IDs`; sem datas (`app/colaboradores/services/situacional.py:117`) | LED vermelho por existência (`app/colaboradores/services/situacional.py:56-62`, `app/colaboradores/services/situacional.py:210`). Contador “em turno” usa juiz próprio e não a cor (`app/colaboradores/services/situacional.py:208-214`). |
| Painel live-cell | Colaborador + A, `.exists()`, sem datas (`app/colaboradores/painel_cell.py:31-32`) | Mesmo LED vermelho (`app/colaboradores/painel_cell.py:68-70`). |
| Drawer operacional | Colaborador + A, mais recente por `-aberta_em`; sem janela (`app/colaboradores/services/drawer.py:71-73`) | Banner dos motivos das perguntas vivas e link do pai (`app/colaboradores/services/drawer.py:76-87`, `app/colaboradores/services/drawer.py:127-132`). |
| Relatório de pendências | Todas A, sem datas (`app/chamados/relatorios_disputa.py:45-50`) | Separa sem pergunta, pronta fechar por C7, aguarda colaborador e aguarda supervisão (`app/chamados/relatorios_disputa.py:11-28`). Tela e command usam mesmo coletor (`app/colaboradores/views.py:3040-3053`; `app/chamados/management/commands/relatorio_pendencias_disputa.py:15`). |
| Contador modal fio | Colaborador + A, `.count()`, sem datas; mensagens ao lado têm janela (`app/chamados/views.py:903-912`) | Contador mostrado como “disputa(s) aberta(s)” (`app/chamados/views.py:947`; `app/templates/chamados/partials/modal_fio.html:19`). |
| Detalhe chamado | `chamado=... AND A`, `.first()`, sem datas (`app/chamados/services/acoes_chamado.py:345-347`) | Carrega perguntas C6 para tela e habilita bloco de regularização (`app/chamados/services/acoes_chamado.py:350-358`, `app/chamados/services/acoes_chamado.py:417-419`; `app/templates/chamados/detalhe_local.html:296-326`). Texto da tela ainda promete “motor não calcula ... fechamento mensal pula” (`app/templates/chamados/detalhe_local.html:304`), contrariando execução atual documentada pela própria view (`app/ponto/views.py:181-183`). |
| Ação regularizar | Chamado + A; sem datas (`app/chamados/views.py:470-475`) | Recusa se não existe, senão delega ação (`app/chamados/views.py:473-475`). |
| Validar/recusar/corrigir pergunta | PK pergunta + disputa A, depois `chamado_que_lista` igual ao chamado; sem janela (`app/chamados/views.py:493-501`, `app/chamados/views.py:510-518`, `app/chamados/views.py:528-536`) | Autoriza verbo administrativo ou recusa pergunta inválida nas mesmas linhas. |
| Validar inline | Busca A por pai; fallback colaborador + pergunta destinada ao chamado; sem data (`app/chamados/views_cobrar.py:160-167`) | Trabalha perguntas vivas respondidas; se não encontra retorna erro (`app/chamados/views_cobrar.py:166-176`). |
| Solicitar correção web/API | PK disputa + próprio colaborador + A; sem datas (`app/chamados/views.py:1328-1334`; `app/api/views.py:2677-2686`) | Recusa fechada/inexistente; chama `reabrir_por_colab` nas mesmas linhas. |
| API chamados | Todas A do colaborador, sem data na consulta de disputas (`app/api/views.py:1016-1022`) | Associa perguntas do app aos destinos vivos ou ao pai; pula disputa fechada (`app/api/views.py:1033-1040`). |
| API perguntas | **Outro predicado:** perguntas C6 vivas particionadas em `respondida_em IS NULL AND disputa.fechada_em IS NULL`, sem janela própria (`app/chamados/services/pergunta_no_app.py:52-61`, `app/chamados/services/pergunta_no_app.py:76-85`) | `em_disputa` é `bool(pendentes)`, não “existe disputa aberta” (`app/api/views.py:2537-2539`, `app/api/views.py:2567-2578`). |
| Badge API e dashboard | Mesma partição do app (`app/chamados/services/pergunta_no_app.py:100-119`) | API principal, kernel core, painel colaborador e dashboard contam pendentes (`app/api/views.py:315-319`; `app/api/views_core.py:303-307`; `app/colaboradores/views.py:959-960`; `app/core/views.py:252-253`). |
| Aviso após texto livre | Mesmas pendentes do app (`app/chamados/services/aviso_pendencia.py:7-20`) | Avisa que texto livre não respondeu questionário; leitores web/API (`app/chamados/views.py:1119`; `app/api/views.py:987`; `app/api/views.py:2211`). |
| Aparência e botões modal | Property `.aberta` sem data (`app/chamados/models.py:1886-1888`) | Cores abertas/fechadas e ações administrativas (`app/templates/chamados/partials/modal_fio.html:76`, `app/templates/chamados/partials/modal_fio.html:353`, `app/templates/chamados/partials/modal_fio.html:406`; `app/chamados/templatetags/chamados_tags.py:105`). |
| Reativar posto | A + posto + motivo `posto_desativado`; sem data (`app/colaboradores/signals.py:71-75`) | Remove motivo pela porta (`app/colaboradores/signals.py:76-82`). |
| Preencher âncora | A + colaborador + motivo `ancora_ausente`; sem data (`app/escala/signals.py:168-172`) | Remove motivo pela porta (`app/escala/signals.py:173-182`). |
| Receber foto | A + colaborador + `foto_ausente`; depois pergunta do episódio `batida_origem=batida`; sem janela (`app/api/foto_recebida.py:65-74`) | Valida, poda motivo e tenta fechar por C7 (`app/api/foto_recebida.py:75-87`). |
| Desligar | Todas A do colaborador, sem data (`app/colaboradores/services/desligamento.py:164`) | Fecha pela porta de desligamento (`app/colaboradores/services/desligamento.py:167`). |
| Regularização externa | Colaborador + A, `.first()`, sem data (`app/chamados/services/regularizacao_ext.py:59-61`) | Bloqueia materialização externa e retorna orientação (`app/chamados/services/regularizacao_ext.py:62-66`). |
| Utilitário censo folha | Pergunta do colaborador + resposta NULL + disputa A; sem data nesse contador (`bin/censo_folha.py:28`) | Acrescenta agravante `PERG_VIVAS` e grava CSV se executado (`bin/censo_folha.py:37`, `bin/censo_folha.py:44-47`). Não foi executado [nao verificado]. |

Alertas e escalonamento encontrados, para complementar o censo do pai: alerta diário seleciona todas A sem data do fato (`app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`), envia push/agrupa empresa e mede idade por abertura (`app/ponto/management/commands/alertar_disputas_pendentes.py:48-74`); escalonador exige disputa A por chamado, sem data, para evitar órfão e marcar incidente (`app/ponto/management/commands/escalonar_chamados_supervisao.py:57-60`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:148-163`).

Ponto de atenção: `lancar_beneficio` fornece ao gate `POST['colaborador_id']`, mas o corpo lê `POST['colaborador']`; o alcance efetivo do gate para esse formulário depende do payload, não demonstrado no dado (`app/colaboradores/views.py:1807`, `app/colaboradores/views.py:1813`).

## 17/11 11:37 · sessao f42d81e1bbd4 · - · - · origem=Ronald (tmux)

**Juiz mais próximo e ausência do predicado completo**

- `colaborador_em_disputa(colaborador)` retorna `.first()` de `DisputaSupervisao` por `colaborador` e `fechada_em__isnull=True`; não considera datas, motivos, perguntas, janela ou competência. É o corpo mais próximo da pergunta simples “tem disputa aberta?”. (`app/chamados/services/disputa_emissao.py:34`, `app/chamados/services/disputa_emissao.py:40`)
- Estado aberto também existe como propriedade `DisputaSupervisao.aberta`: `self.fechada_em is None`. (`app/chamados/models.py:1886`)
- `disputa_viva_do_chamado` responde outra pergunta: busca disputa aberta do chamado e a retorna somente se não houver perguntas ou se `fio_vivo(perguntas)`; serve à trava do encerramento do pai, sem janela. (`app/chamados/juizes.py:2458`, `app/chamados/juizes.py:2466`)
- O registro vivo contém C6=`pergunta_viva`, C7=`pode_fechar`, K2=`classificar_export`, K6=`competencia_entregue`; não encontrei autoridade registrada para “disputa aberta com fato dentro da janela”. A ausência em toda combinação dinâmica possível permanece [nao verificado]. (`app/core/juizes.py:291`, `app/core/juizes.py:292`, `app/core/juizes.py:330`, `app/core/juizes.py:343`)
- Há leitores de perguntas datadas na janela, porém não são o predicado novo: prévia busca `data_evento__range=(ini,fim)` sem filtrar disputa aberta; pauta de competência trancada também busca datas e depois julga perguntas, sem exigir recipiente aberto. (`app/folha/previa.py:64`, `app/chamados/services/competencia_trancada.py:119`)

**Janela e K6**

- `janela_fechamento(mes,ano,empresa)` chama `periodo_apuracao(mes,ano,corte_da_empresa(empresa))`. (`app/ponto/janelas.py:54`)
- `corte_da_empresa` lê `Empresa.dia_inicio_competencia`, admite empresa ou PK, usa padrão 21 e limita a 2–28. (`app/ponto/janelas.py:36`, `app/ponto/janelas.py:49`, `app/ponto/janelas.py:50`)
- A janela começa no dia do corte do mês anterior e termina no corte−1 do mês da competência, inclusive os dois limites. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:15`)
- `janela_atual(dia,empresa)` encontra a competência que contém o dia, usando o mesmo corte; não equivale ao mês civil. (`app/ponto/janelas.py:24`, `app/ponto/janelas.py:60`)
- K6 é `competencia_entregue`: existe `ExportacaoDominio` vigente para empresa/mês/ano e o vigente não está invalidado; mais recente desempata por `(gerado_em,pk)`. Não equivale a `FechamentoMensal.status='aprovado'` nem ao juiz F6. (`app/folha/export.py:556`, `app/folha/export.py:588`, `app/folha/export.py:591`, `app/folha/models.py:217`, `app/ponto/janelas.py:75`)
- A composição existente `competencia_do_dia_entregue` faz dia→`janela_atual`→K6 e devolve `None` sem dia. (`app/chamados/services/vigia_fio.py:785`, `app/chamados/services/vigia_fio.py:797`)

**De onde vem o fato**

- A disputa não tem campo próprio `data_fato`; os campos temporais do recipiente são `aberta_em` e `fechada_em`. O campo factual das perguntas é `PerguntaDisputa.data_evento`, nullable. (`app/chamados/models.py:1820`, `app/chamados/models.py:2528`)
- Uma disputa acumula perguntas de vários dias; `contexto[motivo]['data']` guarda uma data, a última, e não representa o conjunto. (`app/chamados/services/disputa_emissao.py:174`)
- `data_evento` deve ser lido com `conversa`: conversa grava o dia civil de criação, embora não tenha fato de turno; usar simplesmente `data_evento` não nulo classificaria conversa como fato. (`app/chamados/models.py:2540`, `app/chamados/models.py:2899`, `app/chamados/models.py:2906`)
- No nascimento datado, emissão deriva dia da grade, contexto do motivo, origem da batida e/ou chamado de destino e passa-o a `PerguntaDisputa.abrir`; hora sem dia não nasce, demais perguntas sem fato vão a `abrir_conversa`. (`app/chamados/services/disputa_emissao.py:359`, `app/chamados/services/disputa_emissao.py:380`, `app/chamados/services/disputa_emissao.py:396`, `app/chamados/services/disputa_emissao.py:419`, `app/chamados/services/disputa_emissao.py:433`, `app/chamados/services/disputa_emissao.py:448`, `app/chamados/services/disputa_emissao.py:453`)
- A porta recalcula o dia pelo juiz `fio.dia_do_turno_da_linha`, antes de gravar. Com episódio, consulta `ponto.turnos.dia_do_turno_da_batida`; com marco, dia da célula; legado sem marco/episódio pode depender da ponte A2; pode faltar resposta ou haver recusa por alcançar vários dias. (`app/chamados/models.py:2985`, `app/chamados/services/fio.py:1676`, `app/chamados/services/fio.py:1699`)
- `chamados.juizes.dia_do_fato(chamado)` é outra autoridade, sobre chamado: célula→chave de catálogo/data_turno→origens fracas. `fortes=True` para antes dos degraus fracos. Seu fallback `_dia_da_disputa` retorna **a menor** data das perguntas não conversa, não o conjunto necessário ao corte. (`app/chamados/juizes.py:88`, `app/chamados/juizes.py:128`, `app/chamados/juizes.py:57`, `app/chamados/juizes.py:75`)

**Escritores e leitores dos insumos**

| Campo | Escritor | Leitores/observações |
|---|---|---|
| Disputa `colaborador`, `motivos`, `contexto`, `chamado` | Nascimento central: `_abrir_ou_agregar_disputa`; outro nascimento de agrupamento manual de órfãos. (`app/chamados/services/disputa_emissao.py:601`, `app/chamados/services/disputa_emissao.py:1137`) | Helper lê colaborador; agregador reutiliza aberta, agrega motivos e funde contexto. (`app/chamados/services/disputa_emissao.py:40`, `app/chamados/services/disputa_emissao.py:540`, `app/chamados/services/disputa_emissao.py:550`, `app/chamados/services/disputa_emissao.py:576`) |
| `aberta_em` | ORM `auto_now_add`. (`app/chamados/models.py:1820`) | Ordenação padrão `-aberta_em`; helper `.first()` herda essa ordenação, não significa fato mais recente. (`app/chamados/models.py:1868`, `app/chamados/services/disputa_emissao.py:42`) |
| `fechada_em` | `fechar` grava agora; `reabrir_admin` e `reabrir_sistema` zeram. (`app/chamados/models.py:2013`, `app/chamados/models.py:2230`, `app/chamados/models.py:2254`) | `.aberta`, helper público e `disputa_viva_do_chamado`. (`app/chamados/models.py:1888`, `app/chamados/services/disputa_emissao.py:41`, `app/chamados/juizes.py:2466`) |
| Pergunta `data_evento` | **Atribuição única** `fio.gravar_dia_do_fato`; salva no nascimento, na costura que data e no rechaveamento. (`app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:3126`, `app/chamados/models.py:3127`, `app/chamados/services/fio.py:926`, `app/chamados/services/fio.py:974`, `app/chamados/services/fio.py:1084`, `app/chamados/services/fio.py:1090`) | Prévia por janela; dia do chamado como fallback; materialização e K6 do dia. (`app/folha/previa.py:65`, `app/chamados/juizes.py:81`, `app/chamados/services/materializacao.py:550`, `app/chamados/services/vigia_fio.py:779`) |
| Pergunta `conversa` | `abrir` passa False; `abrir_conversa` passa True; `_nascer` insere. (`app/chamados/models.py:2894`, `app/chamados/models.py:2906`, `app/chamados/models.py:3124`) | `_dia_da_disputa` exclui; juiz de dia distingue civil e turno. (`app/chamados/juizes.py:79`, `app/chamados/services/fio.py:1699`) |
| `celula`, `batida_origem`, `instancia_marco` | `_nascer` deriva origem e consulta juiz; rechaveamento muda célula junto ao dia. (`app/chamados/models.py:2976`, `app/chamados/models.py:2984`, `app/chamados/models.py:3124`, `app/chamados/services/fio.py:1085`) | `dia_do_turno_da_linha`/`_dia_do_fato` usam episódio ou marco para resolver dia. (`app/chamados/services/fio.py:1534`, `app/chamados/services/fio.py:1542`, `app/chamados/services/fio.py:1701`) |
| Empresa `dia_inicio_competencia` | Form recebe e limita; portas estrutura criam/atualizam. (`app/colaboradores/views.py:515`, `app/colaboradores/views.py:542`, `app/colaboradores/views.py:557`, `app/colaboradores/services/estrutura.py:13`, `app/colaboradores/services/estrutura.py:18`) | `corte_da_empresa` e as janelas. (`app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`) |
| ED `empresa`, `mes`, `ano`, `gerado_em`, `conteudo`, hash | `ExportacaoDominio.registrar`, ORM grava data. (`app/folha/models.py:80`, `app/folha/models.py:174`, `app/folha/models.py:182`) | K6 e anotação de vigência. (`app/folha/export.py:588`, `app/folha/models.py:203`) |
| ED `invalidada_em`, `substituida_por` | `ExportacaoDominio.invalidar`. (`app/folha/models.py:143`, `app/folha/models.py:158`) | K6 descarta vigente invalidada. (`app/folha/export.py:591`) |

**Duas decisões que o literal parece não encerrar**

1. “Quando não sabemos de que dia é o problema, a pessoa entra ou fica fora e o PDF pode ser auditável?” O anexo manda medir sem decidir o destino; nenhuma resposta deve ser presumida. [nao verificado — anexo item 5]
2. “Se a disputa continua aberta por problema de outro mês, mas guarda uma pergunta deste mês já resolvida, essa pergunta antiga ainda tira a pessoa da folha?” O literal não define se o conjunto usado contém todas as perguntas ou somente os fatos ainda pendentes; essa diferença existe porque recipiente aberto e pergunta viva têm juízes separados. (`app/chamados/models.py:1888`, `app/core/juizes.py:291`; alcance do aval [nao verificado])

Mistura de vários dias com **ao menos um fato efetivamente em disputa dentro da janela** é o existencial do corte; selecionar apenas a menor data, a última do contexto ou a data de abertura perderia casos. (`app/chamados/services/disputa_emissao.py:174`, `app/chamados/juizes.py:85`; interpretação do corte [nao verificado])

## 17/11 11:37 · sessao 0e5cc5138206 · - · - · origem=Ronald (tmux)

Achados para consolidar, somente da cópia:

1. **Export atual**: `classificar_export` inicia `entra` e segue prioridade rescisão → código → férias com batida → cadastro zero → célula → homologação; disputa aberta não é um ramo. O retentor efetivo lê `CelulaDia` na janela, não disputa: acusação retém salvo protestos não cobráveis/L-033; ausência de célula/veredito tem regras próprias. (`app/folha/export.py:768`, `app/folha/export.py:841`, `app/folha/export.py:866`, `app/folha/export.py:968`, `app/folha/export.py:1247`, `app/folha/export.py:1273`, `app/folha/export.py:1293`, `app/folha/export.py:1295`)

2. `motivos_retencao_espelho` ignora os três primeiros argumentos e delega à célula com `janela_competencia`; esta delega a `janela_fechamento`. (`app/folha/export.py:1301`, `app/folha/export.py:1306`, `app/folha/export.py:47`)

3. O TXT recalcula em memória, classifica e monta com os mesmos objetos; só `entra` chega à montagem, há filtro `STATUS_NO_TXT=('aberto','aprovado')`, zeros não geram rubricas. O painel prévio e o retrato usam fechamento gravado, dívida reconhecida no próprio código. (`app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`, `app/folha/export.py:687`, `app/folha/export.py:716`, `app/folha/export.py:726`, `app/folha/export.py:733`, `app/folha/views.py:198`)

4. **PDF atual**: `gerar_pdf_espelho_bytes` chama `motor_bloqueado_por_disputa(colab)` sem competência e levanta `ValueError`; o bloqueio é passado à geração auditável. (`app/relatorios/pdf_espelho.py:1082`, `app/relatorios/pdf_espelho.py:1085`, `app/relatorios/pdf_espelho.py:1087`, `app/relatorios/pdf_espelho.py:1113`)

5. **Importante: PDF informacional não acontece em toda porta.** Lote tenta auditável e, em qualquer exceção, cai no informacional da janela; competência avulsa devolve formulário com erro; anexo ao chamado devolve erro/redirect. Período livre já gera sempre informacional, independentemente de disputa. (`app/relatorios/services.py:214`, `app/relatorios/services.py:237`, `app/relatorios/services.py:244`, `app/relatorios/services.py:250`, `app/relatorios/views.py:1126`, `app/relatorios/views.py:1131`, `app/relatorios/views.py:1002`, `app/relatorios/views.py:1007`, `app/relatorios/views.py:1064`, `app/relatorios/views.py:1098`)

6. **K6 não é status do fechamento.** É existência de exportação vigente não invalidada para empresa/mês/ano; `registro=True` devolve o registro. A seleção usa anotação de vigência; comentários documentam desempate por `(gerado_em,pk)`. (`app/folha/export.py:556`, `app/folha/export.py:564`, `app/folha/export.py:588`, `app/folha/export.py:591`)
   
   O preview consulta K6 só para aviso “Já exportado”; gerar TXT permite reexportação, grava outro registro e avisa depois. Logo não presumir que K6 hoje congela a classificação ou a geração. (`app/folha/views.py:354`, `app/folha/views.py:361`, `app/folha/views.py:295`, `app/folha/views.py:306`, `app/folha/models.py:178`, `app/folha/models.py:182`)
   
   Cada geração grava cópia fiel em `ExportacaoDominio.conteudo` e hash; invalidar altera campos de invalidação e preserva conteúdo/hash. (`app/folha/models.py:86`, `app/folha/models.py:143`, `app/folha/models.py:158`, `app/folha/models.py:182`)

7. **Retrato/admin — risco concreto do motivo próprio**:
   - `fora_da_folha` conta todos motivos, mas só enriquece pessoas retidas por `furo_espelho` com dias/fio/ações. (`app/folha/fora_da_folha.py:147`, `app/folha/fora_da_folha.py:156`, `app/folha/fora_da_folha.py:210`, `app/folha/fora_da_folha.py:234`)
   - `montar_linhas` copia status/motivo dos itens; dias/datas vêm dos enriquecidos por furo ou férias com batida. Novo motivo de disputa, sozinho, terá `dias=None`, `datas=None`, `acoes=[]`. (`app/folha/retrato_folha.py:1214`, `app/folha/retrato_folha.py:1227`, `app/folha/retrato_folha.py:1232`, `app/folha/retrato_folha.py:1237`, `app/folha/retrato_folha.py:1249`)
   - Qualquer motivo diferente de `furo_espelho` cai em fio `admin`, mesmo se disputa depender de resposta do colaborador. Tipo novo não mapeado vira `sem_causa`; saldo sem dia fica vazio. (`app/folha/retrato_folha.py:374`, `app/folha/retrato_folha.py:388`, `app/folha/retrato_folha.py:214`, `app/folha/retrato_folha.py:426`)
   - O lavrador único grava `linhas` e `lavrado_em` por `update_or_create`, sem consulta K6 nesse laço. (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `app/folha/management/commands/lavrar_retrato_folha.py:77`)
   - Tela admin lê os retratos gravados pelo accessor e o painel do colaborador lê a mesma linha para “o que fazer”/portas; não recalcula disputa. (`app/folha/views_retrato.py:194`, `app/folha/views_retrato.py:200`, `app/colaboradores/views.py:1001`, `app/colaboradores/views.py:1005`)
   - “O que fazer” lê motivo, dono, destrava, tipo e saldo das chaves do retrato; links vêm de `acoes` ou fallback do fio/tipo. (`app/folha/retrato_folha.py:1044`, `app/folha/retrato_folha.py:1066`, `app/folha/retrato_folha.py:1112`, `app/folha/retrato_folha.py:1119`)
   - Preview agrupa todo motivo desconhecido e exibe chave crua + aviso de falta de rótulo, portanto pessoa não some, mas orientação/rótulo precisam chegar. Seu contador “dias” conta férias ou motivos, não novos dias da disputa. (`app/folha/views.py:339`, `app/folha/views.py:389`, `app/folha/views.py:398`)
   - Toast de retenção e contador no log do gerar TXT atualmente só agregam `furo_espelho`; motivo novo ficaria fora desse aviso. (`app/folha/views.py:257`, `app/folha/views.py:259`, `app/folha/views.py:293`, `app/folha/views.py:302`)

8. **L-033 existente**: exceção é `set(c.protestos or ()) == {'FURO_PARCIAL'} and _furo_so_da_pausa(c)`; esta lê `periodos_do_dia`, exige sem desalinhamento/abertura/desconhecimento e todas lâmpadas apagadas pertencendo aos pares de pausa. Dia misto continua retendo. (`app/folha/export.py:1150`, `app/folha/export.py:1151`, `app/folha/export.py:1153`, `app/folha/export.py:1155`, `app/folha/export.py:1173`, `app/folha/export.py:1293`)
   
   Há testes concretos de só intervalo, volta faltante, ponta faltante, misto e sem ata; eles cobrem o retentor da célula, não o novo bloqueio compartilhado com PDF. (`app/folha/tests/test_furo_so_intervalo_nao_retem.py:83`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:88`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:94`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:111`, `app/folha/tests/test_furo_so_intervalo_nao_retem.py:117`)

REDs propostos, sem construção nem execução:

- Disputa aberta, dia na janela, célula concorde/nenhuma retenção: hoje export entra e lote PDF é informacional; depois motivo próprio exclui TXT e PDF segue informacional. Morde falta de ramo em `classificar_export`. (`app/folha/export.py:968`; `app/relatorios/pdf_espelho.py:1085`)
- Disputa aberta só fora da janela, célula limpa: hoje PDF recusa; depois juiz responde NÃO e auditável pode seguir. Morde helper sem competência. (`app/relatorios/pdf_espelho.py:1085`)
- Dia do fato em novembro e `aberta_em` em setembro: mesmo resultado do primeiro caso; fronteiras `ini` e `fim` inclusive, véspera e dia seguinte excluídos. O caso col193 consta apenas no anexo [nao verificado]; implementação da janela deve vir do juiz existente. (`app/folha/export.py:47`)
- Vários dias: um dentro e outro fora → SIM; todos fora → NÃO; todos fechados → NÃO pela disputa, mas célula acusando continua retendo. Predicado existencial e fechamento devem ser testados junto ao juiz; sem isso troca pode consultar só primeiro dia [nao verificado].
- L-033 com disputa aberta do mesmo furo só de intervalo: hoje export não retém mas PDF recusa; depois juiz comum preserva exceção nos dois leitores. Ponta faltante/motivo adicional devem continuar SIM. (`app/folha/export.py:1173`, `app/relatorios/pdf_espelho.py:1085`)
- K6 verdadeiro + disputa nova dentro da janela: manter TXT gravado/hash/linhas, classificação/retrato preservados conforme alcance do aval; hoje adicionar condição só em export sem guarda mudaria novas classificações e lavratura. K6 invalidado sem substituto não equivale entregue. (`app/folha/export.py:591`, `app/folha/models.py:86`, `app/folha/management/commands/lavrar_retrato_folha.py:77`)
- Avulso e anexo de competência com SIM: hoje erro, depois informacional se “espelho sai informacional” abrange essas portas. (`app/relatorios/views.py:1007`, `app/relatorios/views.py:1131`)
- Motivo próprio chega ao preview, retidos PDF, contagem/log/toast, retrato e drawer com dia/ação e rótulo; teste aguardar colaborador vs aguardar admin morde falso Admin automático e ausência de dias/ações. (`app/folha/views.py:259`, `app/folha/views.py:389`, `app/folha/retrato_folha.py:374`, `app/folha/retrato_folha.py:1232`, `app/folha/retrato_folha.py:1249`)
- Catraca de autoridade: export/PDF devem ler exatamente a mesma resposta; leitores globais de disputa para outras finalidades não podem receber competência por acidente [nao verificado].
- Dias vazios: medir sem atribuir destino; RED comportamental fica pendente da decisão de lei, não inventar “fora da janela” ou “sem disputa” [nao verificado].

Teste atual do PDF cria disputa sem pergunta/data e exige bloqueio; ele deverá ser revisto com a decisão dos sem dia, não simplesmente reescrito para liberar. (`app/relatorios/tests/test_relatorios.py:84`, `app/relatorios/tests/test_relatorios.py:86`, `app/relatorios/tests/test_relatorios.py:90`)

## 17/11 11:36 · sessao 7d012196cc1a · - · - · origem=Ronald (tmux)

Achados de lei e alcance para a consolidação:

- **L-033 vigente:** entrada e saída batidas, faltando somente intervalo, é pagável e a pergunta segue aberta sem bloquear. O campo “NENHUM” na coluna antiga está superado pelo rodapé da própria lei, que declara a isenção implementada; não usar a coluna isoladamente. (`app/docs/LEIS.md:81`, `app/docs/LEIS.md:194`)
- **Aplicação atual da L-033:** `nao_retem_pela_pausa(c)` exige protestos exatamente `{'FURO_PARCIAL'}` e `_furo_so_da_pausa(c)`; este consulta `periodos_do_dia`, recusa desalinhamento, turno aberto e desconhecimento e exige ao menos uma lâmpada apagada, todas de pausa. O retentor consulta essa função no ramo de célula acusadora. (`app/folha/export.py:1150`, `app/folha/export.py:1155`, `app/folha/export.py:1173`, `app/folha/export.py:1287`, `app/folha/export.py:1293`)
- A segunda cláusula da L-033, pergunta aberta, não foi alterada nessa aplicação; segue sem selo de ponta a ponta, conforme documentação expressa. (`app/core/juizes.py:1280`, `app/docs/LEIS.md:81`)

**Item 1:** há risco literal de colisão com L-033: o SIM existencial por qualquer disputa aberta na janela também encontra uma disputa que só pergunta intervalo. “Preservada a L-033” precisa determinar como a exceção entra antes da resposta única; o código atual aplica a exceção sobre célula/ata/protestos, não sobre uma disputa inteira. (`app/folha/export.py:1173`, `app/folha/export.py:1287`; literal do novo aval fornecido [nao verificado])

**Juízes existentes:** o registro vivo declara C6 `pergunta_viva`, C7 `pode_fechar`, C15 `fio_vivo`, C19 `tarefa_do_dia`, C20 `dia_da_pergunta_trancado` e `respostas_sem_veredito`. São perguntas distintas de “disputa aberta impede a folha desta competência”; K1/K2 continuam em `classificar_export`. (`app/core/juizes.py:285`, `app/core/juizes.py:291`, `app/core/juizes.py:300`, `app/core/juizes.py:304`, `app/core/juizes.py:308`, `app/core/juizes.py:329`)
O helper atual `colaborador_em_disputa` retorna a primeira disputa com `fechada_em IS NULL`, sem janela, pergunta ou data de fato. (`app/chamados/services/disputa_emissao.py:34`, `app/chamados/services/disputa_emissao.py:40`)
A autoridade nova demanda corte de nascimento; a declaração autônoma de função existente exige mesmo predicado e mesmo universo, condições que não se demonstram ao acrescentar janela/exceção. (`app/docs/LEIS.md:69`, `CLAUDE.md:279`)

**Item 3 / K6:** ENTREGUE é existir `ExportacaoDominio` vigente e não invalidada para empresa+competência. O último TXT revogado sem substituta não entrega. Não equivale a status do `FechamentoMensal`, tranca ou holerite publicado. (`app/folha/export.py:556`, `app/folha/export.py:565`, `app/folha/export.py:587`, `app/folha/export.py:590`; `app/docs/CORTES.md:342`; `app/core/juizes.py:343`)
K9 responde outra pergunta: fechamento da célula = K6 OU holerite do colaborador com lote publicado. (`app/folha/export.py:598`, `app/folha/export.py:618`, `app/folha/export.py:621`; `app/docs/CORTES.md:343`)

**“Competência aberta” não está fechada no literal.** Há sentidos distintos no código:

- `marco='aberta'`: nenhum dos marcos aprovada/exportada/trancada/paga está verdadeiro. (`app/ponto/services/fechamento.py:1523`, `app/ponto/services/fechamento.py:1543`)
- F6 permite ajuste por pessoa quando não há FM aprovado/exportado da competência do dia. (`app/ponto/janelas.py:75`, `app/ponto/janelas.py:85`, `app/ponto/janelas.py:87`)
- K4 diz fechada = trancada; aprovada/exportada/paga são marcos. (`app/ponto/services/fechamento.py:1473`, `app/ponto/services/fechamento.py:1476`)
- K6 responde entrega por TXT vigente. (`app/folha/export.py:556`)

Assim, item 3 precisa dizer o destino de aprovada não exportada, trancada sem K6, paga somente por holerite, competência anterior sem entrega e TXT invalidado sem substituta; a escolha pretendida não consta do anexo [nao verificado].

**L-092 e dinheiro:** L-092 protege o gravado da competência exportada; a regra consolidada exige `!`, porta REGEN-EM-EXPORTADA, DIFF anterior, reversão e trilha para esse movimento. (`app/docs/LEIS.md:125`, `CLAUDE.md:411`)
L-113 alterou a L-092: recalcular/exportar de novo deixou de ser genericamente proibido; permanecem versão anterior intacta, trilha e DIFF de impacto antes de ato de frota. Não interpretar “nada muda no gravado” deste novo corte como revogação das outras portas já autorizadas. (`app/docs/LEIS.md:155`, `CLAUDE.md:417`)
O aval fornecido só autoriza registrar, pré-desenhar e medir, e exige aval posterior de construção; implementação/apply não estão autorizados nele [nao verificado — literal fornecido].

**Escritores/leitores K6:**

| Campo/valor | Escreve | Lê |
|---|---|---|
| ED `empresa`, `mes`, `ano`, `conteudo`, `hash_sha256`, `gerado_por`, `reexportacao` | `ExportacaoDominio.registrar`, chamado pela geração TXT. (`app/folha/models.py:174`, `app/folha/models.py:182`; `app/folha/views.py:297`) | K6 filtra tripla e recebe vigência; comparação TXT/gravado lê registro do K6. (`app/folha/export.py:582`, `app/folha/export.py:587`, `app/folha/export.py:655`) |
| ED `gerado_em` | ORM `auto_now_add`. (`app/folha/models.py:80`) | Desempate da vigência por `gerado_em,pk`. (`app/folha/models.py:136`) |
| ED `invalidada_em`, `invalidada_motivo`, `substituida_por` | Porta `invalidar`, exige motivo e preserva conteúdo/hash. (`app/folha/models.py:143`, `app/folha/models.py:151`, `app/folha/models.py:158`, `app/folha/models.py:162`) | K6 recusa vigente invalidado. (`app/folha/export.py:590`) |
| `vigente`, `_vigente_anotado` | Derivação em memória por `anotar_vigencia`. (`app/folha/models.py:203`, `app/folha/models.py:224`) | `e_vigente`, `so_vigentes`, K6. (`app/folha/models.py:123`, `app/folha/models.py:192`; `app/folha/export.py:587`) |
| Marco `exportada` | Resultado derivado por `marcos_da_competencia`; não grava. (`app/ponto/services/fechamento.py:1537`) | `empresas_exportadas_no_escopo`. (`app/ponto/services/fechamento.py:1515`) |

A documentação lookup manda consultar o registro vivo quando houver divergência. (`app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:14`)

## 17/11 11:36 · sessao a5addc8e4de4 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos apenas por leitura:

| Campo/pergunta | Quem escreve | Quem lê / alcance |
|---|---|---|
| Disputa `colaborador`, `motivos`, `contexto`, `chamado` | `criar_ou_atualizar_disputa`: `app/chamados/services/disputa_emissao.py:601`; agrupador de órfãos: `:1137`; âncora: `:1372`; agregação posterior: `:550`, `:576`, `:1145`, `:1380` | `colaborador_em_disputa` consulta somente pessoa e `fechada_em IS NULL`, sem data nem perguntas: `app/chamados/services/disputa_emissao.py:40`. |
| Disputa `aberta_em` | ORM `auto_now_add`: `app/chamados/models.py:1820` | Não integra o predicado de disputa aberta: `app/chamados/services/disputa_emissao.py:40`. |
| Disputa `fechada_em` | Fechamento: `app/chamados/models.py:2013`; reabertura admin: `:2230`; reabertura sistema: `:2254` | Property `aberta`: `:1888`; helper usado pelo PDF: `app/chamados/services/disputa_emissao.py:41`. |
| Pergunta `data_evento` | Atribuição única em `fio.gravar_dia_do_fato`: `app/chamados/services/fio.py:837`, `:847`. Persistem nascimento: `app/chamados/models.py:3123`; costura de nulo: `app/chamados/services/fio.py:914`, `:926`; re-chaveamento: `:1084`, `:1090` | Juiz do dia do chamado lê perguntas da disputa: `app/chamados/juizes.py:75`; portas de identidade leem pelo juiz do turno: `app/chamados/services/fio.py:1676`, `:1698`. |
| Pergunta `conversa` | Porta datada passa False: `app/chamados/models.py:2894`; `abrir_conversa` passa True e data civil atual: `:2906` | O dia da disputa ignora conversas: `app/chamados/juizes.py:79`; o juiz do escritor conserva o dia civil quando conversa: `app/chamados/services/fio.py:1699`. |
| Pergunta `celula` | Nascimento: `app/chamados/models.py:3124`; re-chaveamento: `app/chamados/services/fio.py:1085` | É fallback quando `data_evento` falta no dia da disputa: `app/chamados/juizes.py:83`; célula do chamado precede todos os demais degraus: `:99`. |

**Dia do fato não é um campo escalar de `DisputaSupervisao`.** A disputa tem uma relação 1:N com perguntas, cujos `data_evento` são nullable; o agrupador explicitamente reúne vários chamados numa disputa. (`app/chamados/models.py:2521`, `:2528`; `app/chamados/services/disputa_emissao.py:1127`, `:1130`, `:1137`)

**Há dois alcances que não devem ser confundidos:**

- `dia_do_fato(chamado)` retorna um dia do chamado: célula, contexto do catálogo/`data_turno`, batida, disputa, contexto fraco. O degrau da disputa ignora conversa, coleta `data_evento` ou data da célula de todas as perguntas e retorna **o mínimo**. Portanto, aplicar esse escalar para decidir “tem algum fato dentro da janela” perde um fato dentro quando outro mais antigo cai fora. (`app/chamados/juizes.py:75`, `:85`, `:99`, `:114`, `:130`, `:134`)
- Para a identidade e escrita da pergunta, o dia é o **dia do turno**: episódio usa `_T1`; marco usa `dia_do_turno_da_celula`; linha sem marco/instante pode passar pela ponte A2; dois dias alcançados causam recusa. Conversa usa dia civil e não representa fato. (`app/chamados/services/fio.py:1516`, `:1534`, `:1542`, `:1698`, `:1710`, `:1715`; `app/chamados/models.py:2898`)

`data_do_chamado` sozinho não resolve `disputa_supervisao`: esses módulos deixam `chave_data=None` de propósito, pois o dia vem da batida/disputa; `data_do_chamado` retorna None sem chave declarada. (`app/chamados/catalogo/modulos.py:49`, `:219`, `:223`)

**Vazio continua possível na representação de legado**, embora a porta atual datada recuse None e o contrato de saída exija dia não nulo. Dia da disputa sem pergunta de fato datada/com célula retorna None; usar a data civil de conversa como substituto inventaria um fato. (`app/chamados/models.py:2528`, `:2892`, `:2915`; `app/chamados/juizes.py:79`, `:85`)

**O literal novo precisa dizer qual universo de fatos interessa:** todas as perguntas da disputa ainda aberta ou apenas as vivas/sem decisão. Hoje “disputa aberta” é somente `fechada_em=None`; o juiz de fechamento C7 consulta separadamente perguntas vivas e respostas sem veredito. Uma disputa pode agregar perguntas antigas já julgadas e outras pendentes. (`app/chamados/models.py:1888`, `:1928`, `:1933`; `app/chamados/services/disputa_emissao.py:544`, `:1145`)

**L-033:** entrada e saída batidas, faltando só intervalo, é pagável; pergunta continua aberta sem bloquear. A lei está marcada PELA-METADE porque falta selo ponta a ponta da pergunta, enquanto a isenção de retenção está aplicada. (`app/docs/LEIS.md:81`, `:194`)

A implementação atual exige `protestos == {'FURO_PARCIAL'}` e `_furo_so_da_pausa`: lê `periodos_do_dia`, recusa desalinhamento, turno aberto ou desconhecimento e exige ao menos uma lâmpada apagada, todas de pausa. A retenção consulta isso antes de reter a célula. (`app/folha/export.py:1150`, `:1151`, `:1155`, `:1173`, `:1293`)

O vigia usa o mesmo predicado, sem reimplementá-lo. (`app/folha/services/vigia_pausa.py:48`, `:62`, `:127`, `:157`)

Há caminho explícito de disputa para intervalo ausente: o agrupador trata essa categoria e cria pergunta por marco de intervalo. Assim, um SIM automático por “disputa aberta + dia dentro” pode contornar a L-033 se a exceção só continuar no ramo antigo de célula. (`app/chamados/services/disputa_emissao.py:1149`; `app/folha/export.py:1293`)

Para o item 1, recomendo **PROCEDE** por literal incompleto no domínio dos dias/múltiplos fatos, nulos e conversas; a preservação declarada da L-033 precisa compor o juiz do SIM. Isso é inferência dos alcances divergentes demonstrados acima. (`app/chamados/juizes.py:85`; `app/chamados/models.py:2528`, `:2906`; `app/folha/export.py:1293`)

## 17/11 11:25 · sessao 26b2f5915fe9 · PERSPECTIVA-ESPINHA · censo · origem=Code

PERGUNTA-SEM-SELO | GRADE-MESTRA.md:37 | PENDENTE REAL | Faltam classes além dos pousos parciais (app/docs/BACKLOG.md:806); apenas C7 tem censo fechado (app/docs/RELATO.md:2015); espera de lei/! e censo integral [nao verificado]; HEAD/git log desta cópia [nao verificado] | outras | lei_ou_bang=N | censo=N
JUIZ-DO-PAR-DUAS-ENTRADAS | GRADE-MESTRA.md:38 | SUPERADA/ABSORVIDA | Absorvida por JUIZ-DO-PAR-CURA-NA-FONTE (app/docs/RELATO.md:1142); implementação em app/escala/utils.py:590 | - | lei_ou_bang=N | censo=N
COBERTURA-PARCIAL-UM-JUIZ | GRADE-MESTRA.md:39 | FEITA | Fechada após reabertura, pouso 97bcb01c com prova de frota (app/docs/RELATO.md:8908; app/docs/TICKETS.md:156); juiz em app/ponto/services/cartorio.py:398 | - | lei_ou_bang=N | censo=N
FIO-LE-O-JUIZ | GRADE-MESTRA.md:40 | FEITA | Pouso 7c9e4c27 (app/docs/RELATO.md:3835); push posteriormente concluído em 9185652e por PUSH-AGORA-SEM-O-CLIQUE, mantendo clique como smoke de prova (app/docs/RELATO.md:3741) | - | lei_ou_bang=N | censo=N
CXR-TELA-LE-O-MOTOR | GRADE-MESTRA.md:41 | FEITA | Pouso 94d41ea5 (app/docs/RELATO.md:2031); resíduos transferidos por aval a CXR-PRONTO1-COL841 e CXR-PRONTO5-DEFINICAO (app/docs/RELATO.md:2038; app/docs/RELATO.md:2039); primeira lavra posterior [nao verificado] | - | lei_ou_bang=N | censo=N
C6-TETO-MARCO-PENDENTE | GRADE-MESTRA.md:42 | PENDENTE REAL | Falta curar tratamento do marco pendente no C6 (app/docs/BACKLOG.md:906); discordância com lâmpada registrada em app/docs/RELATO.md:3380; espera de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PASSIVO-VALIDADO-K6 | GRADE-MESTRA.md:43 | PENDENTE REAL | Falta substituir cópia pelo juiz K6 (app/docs/BACKLOG.md:907); predicado copiado permanece em app/chamados/services/passivo_validado.py:127; espera de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PROGRAMA-CELULA-E-O-DIA | GRADE-MESTRA.md:44 | PENDENTE REAL | Escopo inclui E1 e pista V3 (app/docs/BACKLOG.md:909; app/docs/BACKLOG.md:910), apesar da catraca de status vazia (app/core/medida_leitor_do_dia.py:340); leis C16/C06 abertas (app/docs/RELATO.md:837); censo integral fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
GRADE-FOLGA-JUIZ | GRADE-MESTRA.md:45 | FEITA | Pouso 6f583cbc após opus aprovado e prova de frota (app/docs/RELATO.md:8803; app/docs/RELATO.md:8804); leitor do juiz em app/ponto/services/declarar_dia.py:73 | - | lei_ou_bang=N | censo=N
A2-RAISE-NO-FUROS | GRADE-MESTRA.md:46 | PENDENTE REAL | Falta tratar recusa A2 na tela (app/docs/BACKLOG.md:971); chamada em app/relatorios/furos.py:198 e raise em app/escala/services/leitor_celula.py:288; espera de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL | GRADE-MESTRA.md:47 | PENDENTE REAL | Falta registrar juiz do clique morto sob TRAVA JUIZ-NOVO, com corte (app/docs/BACKLOG.md:984); função em app/chamados/services/raiox.py:160; censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
E1-PERGUNTA-AO-DP-CONTADOR | GRADE-MESTRA.md:48 | PENDENTE REAL | Faltam respostas legais do DP/contador (app/docs/BACKLOG.md:986; app/docs/RELATO.md:2985); C16/C06 constam abertas (app/docs/RELATO.md:837); censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
V-FASE-AS-OF-CADASTRO-SEM-LINHA | GRADE-MESTRA.md:49 | PENDENTE REAL | Falta levar ao admin dias passados com dono CADASTRO após relavra (app/docs/BACKLOG.md:991); limitação de janela em app/escala/services/cadastro_realidade.py:390; espera de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
BUG-ENC-CELULAS-FORMA | GRADE-MESTRA.md:50 | FEITA | Pouso 7825fec5b, árvore certificada 2b80dcdca, deploy registrado (app/docs/RELATO.md:256); forma corrigida em app/ponto/turnos.py:2155 | - | lei_ou_bang=N | censo=N
MODO-COMUM-OUT-DO-LOTE | GRADE-MESTRA.md:51 | FEITA | Pouso fc62165a e prova posterior do cartório (app/docs/RELATO.md:8939); portão cumprido, zero decisão lendo chave do lote (app/docs/BACKLOG.md:1036); selo em app/ponto/tests/test_out_do_lote.py:223 | - | lei_ou_bang=N | censo=N
BACKFILL-UNICO | GRADE-MESTRA.md:52 | PENDENTE REAL | Faltam porta única, lista com dono, tratamento dos dormentes e definição do alcance em exportada; obra somente registrada (app/docs/BACKLOG.md:1037); espera atual de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
ISENTO-PONTO-JUIZ-REGISTRADO | GRADE-MESTRA.md:53 | PENDENTE REAL | Falta registrar autoridade após comparar predicado e universo (app/docs/BACKLOG.md:1045); função em app/core/constants.py:13 e leitura crua em app/ponto/services/cartorio.py:1272; espera de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
RESPONDIDA-SEM-TEXTO-DUAS-COPIAS | GRADE-MESTRA.md:54 | FEITA | Cura pelo juiz no marco E3 (app/docs/RELATO.md:1109), merge 90f3fa66b e deploy (app/docs/RELATO.md:614; app/docs/RELATO.md:597); predicado central importado pelo vigia (app/chamados/juizes.py:1531; app/chamados/services/vigia_fio.py:734) | - | lei_ou_bang=N | censo=N
DIA-DA-PERGUNTA-E-O-TURNO | GRADE-MESTRA.md:55 | PENDENTE REAL | Falta escritor único de data_evento/célula/dedup pelo data_turno; cópia anterior reprovada (app/docs/BACKLOG.md:1115); censo histórico dos escritores em app/docs/RELATO.md:3309; espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
O261-VETO-NA-PORTA | GRADE-MESTRA.md:56 | PENDENTE REAL | A pousou em cdc7d8e2; faltam comparação posterior e lotes B/C (app/docs/BACKLOG.md:1116), mostra/trilha do ato humano (app/docs/BACKLOG.md:1106); censo em app/docs/RELATO.md:3338; espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
EXPIRACAO-DA-ABERTA-COPIA-ANTIGA | GRADE-MESTRA.md:57 | PENDENTE REAL | Falta substituir cópia pelo juiz e guarda AST (app/docs/BACKLOG.md:1166); cálculo inline persiste em app/ponto/selecao_periodo.py:288; espera de lei/! e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
JUIZ-PERGUNTA-VIVA-MUDA-SEM-FATO | GRADE-MESTRA.md:58 | PENDENTE REAL | Cura pousada, mas restam passivo, cache, RED e trilha; cinco disputas ainda têm classe a medir (app/docs/RELATO.md:3821; app/docs/RELATO.md:3823); espera de lei/! e fecho integral do censo [nao verificado] | outras | lei_ou_bang=N | censo=N
JUIZ-DE-DONO-DO-DIA | GRADE-MESTRA.md:59 | PENDENTE REAL | Falta predicado autorizado para dono do dia da pergunta (app/docs/BACKLOG.md:1170); parada aceita sem novo predicado (app/docs/RELATO.md:3842); censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
VINCULO-LINHA-DO-TEMPO | GRADE-MESTRA.md:60 | PENDENTE REAL | Faltam migrar leitores B, medir passivo e instalar tripwire/EXCLUDE; censo atualizado aponta cerca de 55 sítios B e constraint ausente (app/docs/RELATO.md:283); espera de lei/! e censo integral fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O-DIA-PAGO | GRADE-MESTRA.md:61 | SUPERADA/ABSORVIDA | Pai explicitamente fechado por fatias, com estado delegado a S3/S4/S5/S5b (app/docs/BACKLOG.md:1221); isso não prova troca integral: S5b ainda pede ! (app/docs/BACKLOG.md:1231; app/docs/BACKLOG.md:1378) | - | lei_ou_bang=N | censo=N
PLACAR-ESTRUTURAL | GRADE-MESTRA.md:62 | PENDENTE REAL | Faltam R3/R4/R6 e ! dos casos pagos/exportados do R4 (app/docs/BACKLOG.md:1263); bateria e R4 permanecem parciais (app/core/placar_estrutural.py:99; app/core/placar_estrutural.py:211); censo global fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
L111-ORFAOS-DO-MOTOR | GRADE-MESTRA.md:63 | PENDENTE REAL | Falta conferir O275/O277 e remover os dois sítios com lápide; censo explicitamente fechado dos 202 nomes (app/docs/BACKLOG.md:1271); definições persistem (app/ponto/calculador/regras.py:302; app/ponto/motor_calculo_v2.py:334); espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
O219 | GRADE-MESTRA.md:64 | PENDENTE REAL | Regra/horizonte implementados (app/ponto/services/cartorio.py:142); faltam TXT novo da 09 e Pauta DP (app/docs/BACKLOG.md:1287); espera atual de lei/! e censo integral fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O228 | GRADE-MESTRA.md:65 | FEITA | Fecho inteiro escrito nos commits b4d938aa, 7cb0efbb e 31fce87c (app/docs/BACKLOG.md:1291; app/docs/RELATO-ARQUIVO.md:39600); lápides em app/ponto/precedencia.py:151 e app/ponto/precedencia.py:548 | - | lei_ou_bang=N | censo=N
O218 | GRADE-MESTRA.md:66 | FEITA | Fechada no main em 1b6d5d02, com bateria e prova de frota (app/docs/TICKETS.md:196); cenários e ponto fixo em app/ponto/tests/test_bateria_lavratura.py:169 e app/ponto/tests/test_bateria_lavratura.py:193 | - | lei_ou_bang=N | censo=N
O221 | GRADE-MESTRA.md:67 | FEITA | Três pousos documentados: bb0bd0fa, d0625307 e 52bfc524 (app/docs/TICKETS.md:193; app/docs/BACKLOG.md:1305); papel prazo presente em app/config/crons.py:2128 | - | lei_ou_bang=N | censo=N
O129 | GRADE-MESTRA.md:68 | PENDENTE REAL | Falta declarar os 28 campos censados do wizard (app/docs/BACKLOG.md:1310); wizard permanece fora das telas declaradas (app/core/configuracao_efeito.py:239); espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
O117 | GRADE-MESTRA.md:69 | SEM PROVA | Censo histórico encontrou zero casos (app/docs/TICKETS.md:250), mas fecho ficou condicional (app/docs/RELATO-ARQUIVO.md:24143; app/docs/RELATO-ARQUIVO.md:24145); encerramento efetivado ou obra sucessora [nao verificado] | - | lei_ou_bang=N | censo=S
O105 | GRADE-MESTRA.md:70 | PENDENTE REAL | Espera ! entre substituir TXT e autopsiar datas_falta; comparação histórica isolou uma linha divergente (app/docs/BACKLOG.md:1336); derivação persiste em app/folha/export.py:740; censo integral fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
O44 | GRADE-MESTRA.md:71 | PENDENTE REAL | Pouso 70d159b9, mas faltam baixa dos dois sítios mantidos, caso 1 pela tela e smoke (app/docs/TICKETS.md:168); censo fechado (app/docs/RELATO-ARQUIVO.md:974); espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
O54 | GRADE-MESTRA.md:72 | PENDENTE REAL | Programa parcial 20/29 (app/docs/RELATO.md:254); faltam portas V0–V4 (app/docs/BACKLOG.md:2267; app/docs/BACKLOG.md:2274), com 12 dias sem nome (app/docs/BACKLOG.md:2270); quatro esperas Ronald (app/docs/RELATO.md:246); censo integral fechado [nao verificado] | espelho-verdade (O54) | lei_ou_bang=S | censo=N
O95 | GRADE-MESTRA.md:73 | PENDENTE REAL | DIFF do resgate entregue sem apply (app/docs/RELATO.md:278); candidata exigir volta ainda sem código e em conflito com selo (app/docs/RELATO.md:282); faltam definição e fecho da cura; espera de lei/! e censo integral fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O92 | GRADE-MESTRA.md:74 | PENDENTE REAL | Falta alimentação em lote dos quatro leitores censados (app/docs/BACKLOG.md:1466); chamadas por dia persistem, por exemplo em app/ponto/calculador/alimentacao.py:516; cópia inline já removida (app/escala/utils.py:2126); espera atual de lei/! [nao verificado] | outras | lei_ou_bang=N | censo=S
O136 | GRADE-MESTRA.md:75 | PENDENTE REAL | Faltam consulta nos dois sentidos, DIFF e ! do motor; censo histórico de 341 templates isolou dois casos (app/docs/BACKLOG.md:1480); consulta ainda funciona como veto do ramo turno_partido (app/ponto/motor_calculo_v2.py:3659) | outras | lei_ou_bang=S | censo=S
CHAMADO-EM-RAIA | GRADE-MESTRA.md:76 | PENDENTE REAL | Escritor fechado por ESCRITOR-UNICO-DE-CHAMADO (app/docs/RELATO.md:5686; app/docs/RELATO.md:5690), mas contrato juiz permanece declarado PARCIAL, condicionado a O168/O169/LASTRO (app/core/contratos_estruturais.py:270); espera atual de lei/! e censo restante fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
QUADRO lote 1: FEITA=10 PENDENTE=27 SUPERADA=2 SEM_PROVA=1 TOTAL=40

## 17/11 11:25 · sessao b0698118728f · RESPOSTAS-1120 · verificador · origem=Code

**AVAL-DEVOLVIDO: itens 2 e 3.** O item 2 conserva uma igualdade que a F5 passaria a violar; o item 3 precisa fechar a identificação e a leitura do processo da medição. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319–335`; `app/folha/porta_export.py:768–821`.)

A revisão abaixo vale para os arquivos desta cópia. A identidade Git do HEAD, a implantação e os efeitos reais em produção são [nao verificado].

**Item 1 — CONFERE, com “cobrar” no sentido literal do botão existente.** O botão envia uma notificação; não transfere a tarefa ao colaborador. (`app/chamados/views_cobrar.py:34–57`.)

**1. Premissas.** Conferem os dois ramos: sem decisão → `aguarda_resposta`, ação `RESPONDER`, dono `COLABORADOR`; último ato SILENCIO → `silencio_decidido`, ações vazias. Isso descreve a pergunta; não prova ausência de qualquer outra tarefa naquele dia, pois ações residuais e Resolver dia também podem produzir dono. (`app/chamados/services/tarefa_do_dia.py:156–177`, `:665–686`.)

O K6 está registrado e lê exportação vigente, não invalidada, por empresa/mês/ano. A tarefa já converte o dia do fato para competência usando `janela_atual` e consulta esse juiz. (`app/core/juizes.py:343`; `app/folha/export.py:580–594`; `app/chamados/services/tarefa_do_dia.py:467–483`.)

Não encontrei definição ou chamada Python de `competencia_ja_paga` nesta cópia; a existência da implementação é [nao verificado]. O documento local registra a obra e seu pouso pendente. (`app/docs/BACKLOG.md:1992`.)

**2. Colisão com aval.** Na transcrição fornecida, o item 1 substitui expressamente o item 1 do CORTE-RESSUSCITAR-TEM-DONO: preserva o colaborador onde ele já recebe `responder` e atribui ao admin o silêncio ressuscitado. Não revoga o ato compensatório exigido pelo CORTE-F2. Vigência além dos anexos fornecidos: [nao verificado]. A separação entre preservar a fila existente e construir o novo encaminhamento enfrenta, porém, o conflito de teste descrito no item 2. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319–335`.)

**3. Literal.** O botão existe, recebe um **chamado**, envia push genérico ao atendimento e limita a cobrança a uma por **colaborador/dia**. Ele não grava decisão nem transforma SILENCIO em `RESPONDER`. Sua apresentação atual está no ramo `verbo == "cobrar"`, não no ramo `decidir`; disponibilizá-lo na futura lista administrativa ainda precisa ser construído. (`app/chamados/views_cobrar.py:24–57`; `app/templates/chamados/partials/_lista_chamados.html:83–86`.)

Também não basta interpretar qualquer SILENCIO como “ressuscitada”: a ressuscitação atual limpa a projeção e registra auditoria; a carga da tarefa lê perguntas e decisões. A identificação que a F5 usará deve ser coerente com o ato compensatório da F2; essa costura futura é [nao verificado]. (`app/chamados/models.py:1613–1630`, `:1651–1662`; `app/chamados/services/tarefa_do_dia.py:405–430`.)

**4. Exportada, dinheiro ou `!`.** O encaminhamento pode consultar entregue sem escrever nela: o K6 é leitura. A ressalva é não confundir este corte de dono com autorização para executar ressuscitação ou corrigir estoque entregue: a porta atual de ressuscitação efetivamente limpa resposta, validação e materialização. Efeito monetário ou passivo real: [nao verificado]. (`app/folha/export.py:580–594`; `app/chamados/models.py:1629–1630`, `:3391–3393`.)

**Quem escreve e quem lê os valores do item 1:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Dia do fato, `data_evento` | Nascimento chama `gravar_dia_do_fato`. (`app/chamados/models.py:3123–3127`.) | Carga agrupa perguntas pelo dia; competência sai de `janela_atual`. (`app/chamados/services/tarefa_do_dia.py:405–412`, `:467–483`.) |
| SILENCIO e seus `ato_em`, autor, texto e motivo | Encerramento administrativo chama `DecisaoPergunta.registrar`; esta porta faz `bulk_create`. (`app/chamados/models.py:1999–2007`, `:4110–4122`.) | Carga ordena por `ato_em, pk`, limita por `agora`; `_pela_decisao` interpreta o último ato. (`app/chamados/services/tarefa_do_dia.py:421–424`, `:158–177`.) |
| Resposta, validação e carimbo removidos ao ressuscitar | `ressuscitar_perguntas` chama `descarimbar_em_lote`, após conservar a projeção anterior na trilha. (`app/chamados/models.py:1613–1630`, `:3391–3393`.) | Carga da tarefa lê projeção e sequência de decisões. (`app/chamados/services/tarefa_do_dia.py:405–430`.) |
| Evidência atual de “ressuscitada” | Auditoria `perguntas_ressuscitadas`, com IDs antes/depois. (`app/chamados/models.py:1651–1662`.) | Leitor dessa auditoria para o novo encaminhamento: [nao verificado]. A carga atual usa perguntas/decisões. (`app/chamados/services/tarefa_do_dia.py:405–430`.) |
| `razao`, `acoes`, `acao`, `de`, `porta` e dono publicado | Produção em memória por `_pela_decisao` e projeção da porta; não é gravação de coluna “dono”. (`app/chamados/services/tarefa_do_dia.py:142–177`, `:662–708`, `:733–736`.) | `porta_da_tarefa`. Novo leitor/lista administrativa: [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:712–725`.) |
| Entregue pelo K6 | `ExportacaoDominio.registrar`; invalidação por `invalidar`. (`app/folha/models.py:143–162`, `:174–187`.) | `competencia_entregue`, chamado pela tarefa. (`app/folha/export.py:580–594`; `app/chamados/services/tarefa_do_dia.py:479–483`.) |
| Cobrança `[COBRANCA]` | View envia push e chama `nota.anexar_ocorrencia`, que grava `FollowupChamado`. (`app/chamados/views_cobrar.py:44–56`; `app/chamados/services/nota.py:75–109`.) | View aplica limite diário; painel monta `cobrado_hoje`; template mostra a marca/botão. (`app/chamados/views_cobrar.py:36–43`; `app/chamados/services/painel.py:349–359`, `:395–396`; `app/templates/chamados/partials/_lista_chamados.html:84`.) |
| Lista textual DP por pergunta/colaborador; novo registro “paga fora” | Escritores e leitores dessa entrega futura: [nao verificado]. | [nao verificado]. |

**Item 2 — PROCEDE.**

**1. Premissas.** O teste existe, mas sua exigência é mais forte que “não devolver ao colaborador”: guarda a tarefa anterior e exige **igualdade integral** depois da ressuscitação. O caso SILENCIO usa exatamente essa comparação. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319–335`.)

As matrizes próprias F5/F6 não estão disponíveis para conferência literal nesta cópia: os documentos apontam para `logs/ESPINHA-BRIEFS/<f>/matriz.md`. Conteúdo atual e marcação `RESOLVIDO-PELO-CORTE`: [nao verificado]. (`app/docs/RELATO.md:298`, `:313–314`, `:425`.)

**2. Colisão com aval.** Separar F2 e F5 preserva a fronteira de construção, mas não resolve a árvore final: se a F5 fizer aquela ressuscitada com SILENCIO passar ao admin, o teste preservado falhará. A transcrição do CORTE-F2 também exige conservação da fila; vigência externa desse anexo: [nao verificado]. A contradição executável está na igualdade e no caso SILENCIO. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:329–335`.)

**3. Literal.** Falta declarar como a prova de conservação da F2 coexistirá com a mudança autorizada na F5: escopo distinto demonstrável ou revisão expressa da expectativa após a F5. Apenas acrescentar outro teste não permite que a mesma tarefa seja simultaneamente igual e diferente antes/depois. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319–335`.)

**4. Exportada, dinheiro ou `!`.** Registro documental e testes não demonstram alteração monetária. Entretanto, mudar a tarefa pode alcançar leitores além da lista: o registro vigente já associa F6 ao dono publicado pela tarefa. Alcance real e diferença em produção: [nao verificado]. (`app/docs/CORTES.md:488`; `app/chamados/services/tarefa_do_dia.py:708–725`.)

**Quem escreve e quem lê os valores do item 2:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Expectativa de invariância | Fixture captura `antes`; ressuscitação altera a pergunta. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319–328`.) | Asserção compara `_fila(p)` com `antes`; caso SILENCIO chama essa prova. (`:329–335` do mesmo arquivo.) |
| Novo caso SILENCIO → admin | Produtor futuro na F5: [nao verificado]. Produtor atual retorna ações vazias. (`app/chamados/services/tarefa_do_dia.py:176–177`.) | Novo teste F5 e lista administrativa: [nao verificado]. |
| `aguarda_admin` da F6 | Hoje o serializador calcula por `p.estado == RESPONDIDA`. (`app/api/views.py:2549–2553`.) | Consumidor cliente desse JSON: [nao verificado]. |
| `RESOLVIDO-PELO-CORTE` nas matrizes | Registro solicitado, ainda [nao verificado]. | Leitor automatizado ou conteúdo efetivo das matrizes: [nao verificado]; seus endereços são apenas referenciados localmente. (`app/docs/RELATO.md:298`, `:313`.) |

**Item 3 — PROCEDE; a troca da unidade da frase confere.**

**1. Premissas.** `N` soma ocorrências heterogêneas, incluindo listas de divergências e, quando habilitados, dias de HE; não conta colaboradores únicos. Retirar “colaborador(es)” confere. A frase atual do código é “discordam em N ponto(s)”; os estados nomeados pertencem à construção proposta. (`app/folha/porta_export.py:687–695`, `:762–764`, `:865–867`.)

**2. Colisão com aval.** Na transcrição fornecida, o novo item altera expressamente a frase e acrescenta FALHOU/R7 ao aval anterior; vigência externa: [nao verificado]. Isso preserva a porta existente somente se falha e processo morto continuarem bloqueando TXT: atualmente negativo e positivo bloqueiam, zero libera. (`app/folha/porta_export.py:818–821`, `:867`.)

**3. Literal — faltam estas costuras:**

- **Quem sabe que morreu?** O carimbo atual não contém identidade de job/PID nem estado de execução. Existe padrão concreto da casa: serviço grava PID/início, modelo consulta `os.kill(pid, 0)`, vigia converte morte em erro e a tela chama o vigia. Sua ligação à medição do export é [nao verificado]. (`app/inteligencia/models.py:87–105`; `app/relatorios/lote_espelho.py:66–80`, `:117–138`; `app/relatorios/models.py:140–161`; `app/relatorios/views.py:1270–1278`.)
- **Erro não é sinônimo de saída não zero.** O command retorna `SystemExit(2)` tanto por erro de medição quanto por divergência. Além disso, mede todas as empresas ativas e pode carimbar uma antes de falhar na seguinte. FALHOU precisa ser decidido pelo resultado da empresa/competência/tentativa correspondente, não apenas pelo término do command. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86–95`, `:178–183`.)
- **Qual logger avisa quem?** Há logger `folha.porta_export`, atualmente usado com WARNING, e configuração de destino `/app/logs/errors.log`. O emissor CRITICAL da nova falha, seu leitor operacional e a comprovação de “suporte foi avisado” são [nao verificado]. (`app/folha/porta_export.py:83–85`, `:97–99`; `app/config/settings/saas.py:136–142`, `:183`.)

**4. Exportada, dinheiro ou `!`.** O escritor mostrado grava snapshot; a medição já separa contadores de exportada que ficam fora de `falhas`. Alterar esses contadores, o cálculo ou o TXT ultrapassaria o controle de execução e a apresentação descritos. Implementação futura e efeito real: [nao verificado]. (`app/folha/porta_export.py:705–712`, `:772–796`.)

**Quem escreve e quem lê os valores do item 3:**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| Chave, empresa, dia, mês/ano, `valor`, listas em `detalhe` | `lavrar`; competência hoje fica no detalhe, fora da chave única. (`app/folha/porta_export.py:772–796`; `app/inteligencia/models.py:102–105`.) | `conferir`; view lê sua mensagem para bloquear. (`app/folha/porta_export.py:807–821`; `app/folha/views.py:128–133`.) |
| `N` | `medir` calcula `falhas`; `lavrar` persiste em `valor`. (`app/folha/porta_export.py:687–691`, `:762–764`, `:774`.) | `conferir` lê e formata o número. (`app/folha/porta_export.py:821`, `:865–866`.) |
| Hora da conferência | Modelo atualiza `calculado_em` automaticamente. (`app/inteligencia/models.py:96`.) | Leitor futuro da hora na tela: [nao verificado]. |
| EM CONFERENCIA, PRONTA, DIVERGENTE, FALHOU; tentativa, PID e liberação para repetir | Escritor da porta futura: [nao verificado]. O snapshot atual tem somente campos genéricos. (`app/inteligencia/models.py:87–105`.) | Leitor/vigia da porta futura: [nao verificado]. O padrão disponível escreve PID no serviço e o lê no modelo. (`app/relatorios/lote_espelho.py:138`; `app/relatorios/models.py:151–161`.) |
| Erro da medição | `medir` produz `erro_cartao_x_txt`, mas `lavrar` não o inclui no detalhe. (`app/folha/porta_export.py:737`, `:776–796`.) | `conferir` tenta ler esse erro e pode cair em “motivo não registrado”. (`app/folha/porta_export.py:818–820`.) |
| Linha CRITICAL / aviso ao suporte | Novo emissor: [nao verificado]. Destino configurado: `errfile` pela raiz. (`app/config/settings/saas.py:136–142`, `:183`.) | Leitor operacional e aviso efetivo: [nao verificado]. |

VEREDITO por item: 1 CONFERE; 2 PROCEDE; 3 PROCEDE

## 17/11 11:24 · sessao 830b3ff7a75c · PERSPECTIVA-ESPINHA · censo · origem=Code

DOSSIES-DAS-CELULAS | GRADE-MESTRA.md:77 | SEM PROVA | Seis itens permanecem EM CURSO; falta prova consolidada do fecho, inclusive dieta de prosa (app/docs/BACKLOG.md:1487); HEAD/git log desta cópia [nao verificado] | - | lei_ou_bang=N | censo=N
O142 | GRADE-MESTRA.md:78 | FEITA | Pouso d1689254 (app/docs/RELATO-ARQUIVO.md:37752); borda incorporada à cura (app/docs/RELATO-ARQUIVO.md:38964; app/ponto/turnos.py:1135); três relavras aplicadas (app/docs/RELATO-ARQUIVO.md:38695) | - | lei_ou_bang=N | censo=N
O177 | GRADE-MESTRA.md:79 | SUPERADA/ABSORVIDA | Absorvida pela O119: duas pausas lidas nos dois sítios (app/ponto/juiz_batida.py:119; app/ponto/juiz_batida.py:271), NO AR 801253ed (app/docs/BACKLOG.md:1316), com DIFF, reversão e exportada intacta (app/docs/RELATO-ARQUIVO.md:24025) | - | lei_ou_bang=N | censo=N
O178 | GRADE-MESTRA.md:80 | PENDENTE REAL | Faltam prazo, dono e contador de pauta sem leitura; registro ABERTA (app/docs/BACKLOG.md:1524), campos ainda opcionais (app/pautas/models.py:90); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O179 | GRADE-MESTRA.md:81 | PENDENTE REAL | Falta medir horas reais dos três colaboradores no espelho e decidir dono pela L-099; causa não medida (app/docs/BACKLOG.md:1525); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O180 | GRADE-MESTRA.md:82 | PENDENTE REAL | Falta entregar lista dos 102 colaboradores; regeneração do passado espera ! (app/docs/BACKLOG.md:1526); fecho do censo [nao verificado] | outras | lei_ou_bang=S | censo=N
O183 | GRADE-MESTRA.md:83 | PENDENTE REAL | Guarda presente (app/chamados/models.py:1132); faltam DIFF e tratamento do estoque de 494; censo dos quatro escritores fechado por AST (app/docs/BACKLOG.md:1529) | outras | lei_ou_bang=N | censo=S
O229 | GRADE-MESTRA.md:84 | PENDENTE REAL | Faltam curas e RED por classe dos nove achados; itens 1, 2 e 6 pedem censo próprio (app/docs/BACKLOG.md:1557); filtro exclusivo de chegada permanece (app/ponto/services/espelho.py:1261); censo integral fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O216 | GRADE-MESTRA.md:85 | PENDENTE REAL | Falta remedir pela função real os três números da entrada da O191 após O195 e republicar; dívida ABERTA e pré-aprovada (app/docs/BACKLOG.md:1559); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
SOBRA-NA-BORDA-O-QUE-FICOU | GRADE-MESTRA.md:86 | PENDENTE REAL | Item a exige obra de rejulgamento quando mudam batidas; tentativa não certificada e cinco demais achados guardados (app/docs/BACKLOG.md:1584); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
ATA-NAO-DISPENSA-JUSTIFICADA | GRADE-MESTRA.md:87 | FEITA | Cura NO AR 44e268a8 (app/docs/RELATO.md:5397); acervo da 10 APLICADO com reversão e 09 intacta (app/docs/RELATO.md:5360); resumo posterior contradiz essa aplicação detalhada (app/docs/RELATO.md:838) | - | lei_ou_bang=N | censo=N
CONCORDE-PREVISTO-ZERO | GRADE-MESTRA.md:88 | PENDENTE REAL | Falta investigar os 730 dias concorde com previsto zero, excluídos sem investigação (app/docs/BACKLOG.md:1608); censo fechado [nao verificado] | espelho-verdade (O54) | lei_ou_bang=N | censo=N
ESPELHO-VERDADE-REMEDIDO | GRADE-MESTRA.md:89 | FEITA | Atualizações registradas no placar (app/core/espelho_verdade.py:117; app/core/espelho_verdade.py:141; app/core/espelho_verdade.py:183; app/core/espelho_verdade.py:187); termômetro publicado no pouso R1-PUBLICA a16637ca (app/docs/RELATO.md:3128) | - | lei_ou_bang=N | censo=N
C5-LISTA-DE-FROTA | GRADE-MESTRA.md:90 | SUPERADA/ABSORVIDA | Universo de 27 dias/18 colaboradores (app/docs/BACKLOG.md:1620) retomado pela C5-FALTA-MAIOR-QUE-PREVISTO, com remedida após os pousos e dono L-099 por caso (app/docs/BACKLOG.md:1816) | - | lei_ou_bang=N | censo=N
FERIADO-DIA-CHAVE | GRADE-MESTRA.md:91 | PENDENTE REAL | Falta unificar dia-chave e predicado e provar caso atravessando meia-noite (app/docs/BACKLOG.md:1623); diferença permanece nos leitores (app/ponto/calculador/alimentacao.py:521; app/ponto/services/fechamento.py:949); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
LISTA-POR-COMPETENCIA | GRADE-MESTRA.md:92 | PENDENTE REAL | Falta uma linha por competência aberta (app/docs/BACKLOG.md:1625); lavra ainda mede janela agregada por colaborador (app/ponto/services/esmeril_espelho.py:38; app/ponto/services/esmeril_espelho.py:219); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
REGUA-DO-PERIODO-COMPLEMENTO-19-10 | GRADE-MESTRA.md:93 | PENDENTE REAL | Faltam comprovar conclusão dos leitores restantes, censo das quatro testemunhas em 09/10 e encaminhamento/apply por dono (app/docs/BACKLOG.md:1706; app/docs/BACKLOG.md:1725); antigos ! atendidos (app/docs/RELATO.md:7507); censo integral fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O327 | GRADE-MESTRA.md:94 | PENDENTE REAL | Falta lei do ancoramento do trecho extra e cobertura explícita dos não julgados (app/docs/BACKLOG.md:1753); recusa persiste em medição posterior (app/docs/RELATO.md:1294); censo fechado [nao verificado] | espelho-verdade (O54) | lei_ou_bang=S | censo=N
RETIFICACAO-DA-10-8069-PRIMEIRO | GRADE-MESTRA.md:95 | PENDENTE REAL | Faltam retificação pela porta, DIFF/reversão/prova e corte da 243 (app/docs/BACKLOG.md:1757); premissa do sinal da 8069 foi corrigida (app/docs/RELATO.md:6020); censo fechado [nao verificado] | folha-online (O48) | lei_ou_bang=S | censo=N
REABRIR-PERGUNTA-A-PREMISSA | GRADE-MESTRA.md:96 | PENDENTE REAL | Código NO AR bdfad18f, mas falta prova do portão: contador zero e sete dias sem laço (app/docs/BACKLOG.md:1777; app/docs/RELATO.md:4751); censo fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
LOTE-VAZA-ENTRE-COLABS | GRADE-MESTRA.md:97 | PENDENTE REAL | Falta medir causa da divergência lote versus individual e apresentar cura (app/docs/BACKLOG.md:1800); pouso e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PASSADO-SEM-MARCOS-SEM-TETO | GRADE-MESTRA.md:98 | SUPERADA/ABSORVIDA | Tratamento incorporado à E4a-1, NO AR 892159c5f (app/docs/RELATO.md:1729), e ao MODO-COMUM-PASSADO-SEM-TURNO FECHADO (app/docs/BACKLOG.md:2147); passado sem turno encaminhado ao cadastro (app/chamados/services/fabrica_da_celula.py:20) | - | lei_ou_bang=N | censo=N
LEGADO-51-ACESAS | GRADE-MESTRA.md:99 | PENDENTE REAL | Falta medir as 51 pela função real e fechar pela porta o que a lei encerra (app/docs/BACKLOG.md:1815); aplicação e censo fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
DIA-DA-BATIDA-JUIZ-UNICO | GRADE-MESTRA.md:100 | PENDENTE REAL | Faltam censo AST classificado, migração dos juízos paralelos, selo e DIFF por migração (app/docs/BACKLOG.md:1818); rejulgamento prévio de col33 comprovado (app/docs/RELATO.md:4265); fecho do censo [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
PERGUNTA-DO-DIA-EM-CURSO | GRADE-MESTRA.md:101 | FEITA | 165fee20 NO AR e push provado (app/docs/RELATO.md:4236); pouso documentado (app/docs/RELATO.md:4255); teto por instante presente (app/chamados/services/fabrica_da_celula.py:8) | - | lei_ou_bang=N | censo=N
R1-ESTRUTURA-91 | GRADE-MESTRA.md:102 | PENDENTE REAL | A-i e flip fechados (app/docs/BACKLOG.md:1790; app/docs/BACKLOG.md:1839); faltam causas/cura do restante, com 57 dias ESTRUTURA e investigação incompleta (app/docs/RELATO.md:229; app/docs/RELATO.md:221); lei/! e censo fechado desse restante [nao verificado] | espelho-verdade (O54) | lei_ou_bang=N | censo=N
BATERIA-QUEIXAS-ADMIN | GRADE-MESTRA.md:103 | PENDENTE REAL | Faltam validação Ronald+DP e selos após FIO MUDO (app/docs/BACKLOG.md:1901; app/docs/RELATO.md:2481); conclusão dos casos e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
TIPO-NA-FONTE | GRADE-MESTRA.md:104 | PENDENTE REAL | Falta corte LU2-03 entre selo do valor e acessor tipado, seguido de construção em cópia nova (app/docs/RELATO.md:344); censo do item 3 explicitamente fechado (app/docs/RELATO.md:827) | contrato 20 | lei_ou_bang=S | censo=S
MOTOR-LE-O-TURNO | GRADE-MESTRA.md:105 | PENDENTE REAL | Faltam migrações PAR/DIA, RED, DIFF por dono e certificação DP (app/docs/BACKLOG.md:1997); herança própria de dia permanece (app/ponto/motor_calculo_v2.py:2407); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
FIO-E0-V4-E1-ENTRADAS | GRADE-MESTRA.md:106 | FEITA | FECHADA expressamente: núcleo a8a692f55 e E1-FILA pousados; 16 complementos em linhas próprias (app/docs/BACKLOG.md:2008; app/docs/RELATO.md:284) | - | lei_ou_bang=N | censo=N
FIO-E0-V4-E4-ESCRITORES | GRADE-MESTRA.md:107 | PENDENTE REAL | Faltam leitores E4c e Contract E4d (app/docs/BACKLOG.md:2035); B2 em construção (app/docs/RELATO.md:30); apply exportado espera ! (app/docs/RELATO.md:285); censos E4c/E4d fechados na base indicada, com revalidação pós-E3 requerida (app/docs/RELATO.md:833) | E4c | lei_ou_bang=S | censo=S
RAIZ-DA-DISPUTA | GRADE-MESTRA.md:108 | PENDENTE REAL | Faltam etapas do programa F0–F7, incluindo F5 com E4d (app/docs/BACKLOG.md:2055); BO-DISPUTA exige matriz conferida e aval (app/docs/BACKLOG.md:2049); censo de escritores/eleitores fechado (app/docs/RELATO.md:827) | outras | lei_ou_bang=S | censo=S
LAVRA-PREMATURA-CONCORDE | GRADE-MESTRA.md:109 | PENDENTE REAL | Falta decisão de desenho sobre concorde com marco vencido faltando durante turno em curso; construção vedada (app/docs/BACKLOG.md:2141); censo fechado [nao verificado] | outras | lei_ou_bang=S | censo=N
RETRATO-POR-EVENTO | GRADE-MESTRA.md:110 | PENDENTE REAL | Faltam evento empresa×competência, consumidor e frescor por evento (app/docs/BACKLOG.md:2199; app/docs/RELATO.md:1812), além de construção e certificação (app/docs/RELATO.md:1774); censo fechado [nao verificado] | folha-online (O48) | lei_ou_bang=N | censo=N
JV1-MERGE-FILA | GRADE-MESTRA.md:111 | PENDENTE REAL | Faltam RED da janela, alimentação além do envelope, contador do corte 21 e alimentação dos feriados em espelho/PDF (app/docs/BACKLOG.md:2221; app/docs/RELATO.md:2477); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
O168 | GRADE-MESTRA.md:117 | FEITA | Pouso 89a5b0d85 registrado (app/docs/BACKLOG.md:1513), NO AR d33c0a899 (app/docs/RELATO.md:890); guarda no gate do pai (app/chamados/models.py:718); restantes em linha própria (app/docs/BACKLOG.md:1514) | - | lei_ou_bang=N | censo=N
O169 | GRADE-MESTRA.md:118 | FEITA | C20-O169 FECHADA (app/docs/BACKLOG.md:1913), pouso bede09aa mantido (app/docs/RELATO.md:2403); lastro do próprio dia consultado (app/ponto/services/cluster_espurio.py:53); carimbo persistente separado em O169-FECHADA (app/docs/RELATO.md:284) | - | lei_ou_bang=N | censo=N
FIO-E0-V4-E3-LEITORES | GRADE-MESTRA.md:119 | FEITA | Recorte autorizado com restos em obras próprias (app/docs/BACKLOG.md:742); merge 90f3fa66b (app/docs/RELATO.md:614), deploy e smoke (app/docs/RELATO.md:597); R2 separado e R3 pousado (app/docs/BACKLOG.md:761; app/docs/BACKLOG.md:770); censo fechado (app/docs/RELATO.md:2001) | - | lei_ou_bang=N | censo=S
E3-CADASTRO-ADMIN | GRADE-MESTRA.md:120 | PENDENTE REAL | Falta correção administrativa dos cadastros listados; fonte determina que não se cura por código (app/docs/BACKLOG.md:2028); fecho do censo [nao verificado] | rabos da E3 | lei_ou_bang=N | censo=N
E3-ENTRADA-ORFA-SEM-TURNO | GRADE-MESTRA.md:121 | PENDENTE REAL | Falta lei para dia sem turno COM batida, fora da lei de passado sem marcos (app/docs/BACKLOG.md:2176); emissor permanece (app/chamados/services/disputa_emissao.py:257); censo fechado [nao verificado] | rabos da E3 | lei_ou_bang=S | censo=N
QUADRO lote 2: FEITA=8 PENDENTE=28 SUPERADA=3 SEM_PROVA=1 TOTAL=40

## 17/11 11:24 · sessao 1592a5b9720c · E4C-F2 · verificador · origem=Code

REPROVADO

1. **Bloqueia — o compensador pode não desfazer o estado anterior.** Ele grava `ato_em=agora`, enquanto leitura e SQL ordenam por `ato_em/pk`, incluindo atos futuros. Exemplo: VALIDACAO já existente às 13h; ressuscitação às 12h. A REABERTURA fica antes da validação, que continua vigente na trilha, embora a projeção seja limpa. O mesmo problema permite uma RESPOSTA futura continuar vigente. (`app/chamados/models.py:1662`, `app/chamados/models.py:1673`, `app/chamados/models.py:4273`, `app/chamados/models.py:4279`, `app/chamados/models.py:3530`)

   Isso pode reproduzir a divergência C6 × `q_vivas`: C6 lê a projeção limpa; o filtro SQL ainda encontra validação vigente. Com RESPOSTA futura, a limpeza da materialização também pode fazer a pergunta passar a segurar CSF. Ocorrências no estoque/exportadas: **[nao verificado]**. (`app/chamados/juizes.py:1389`, `app/chamados/models.py:3591`, `app/chamados/models.py:1982`)

2. **Bloqueia — `ato_motivo` continua com função de texto e de discriminador de estado.** Em RESPOSTA, motivo vazio desfaz validação; motivo preenchido conserva. A face SQL repete esse critério. Acrescentar ou retirar uma explicação textual pode, portanto, mudar a validação vigente. (`app/chamados/models.py:4241`, `app/chamados/models.py:4251`, `app/chamados/models.py:3529`)

   Os escritores dependem dessa convenção: resposta humana omite motivo; resposta automática, recuperação pela trilha e reconstrução da projeção o preenchem. Não existe campo explícito para essa diferença. (`app/chamados/services/disputa_emissao.py:1525`, `app/chamados/services/disputa_emissao.py:1678`, `app/chamados/services/disputa_emissao.py:1699`, `app/chamados/models.py:4224`)

   O teste novo chamado `achado5` compara duas **REABERTURAS** com motivo igual. Ele protege a identificação da ressuscitação, mas não ataca a dupla semântica de **RESPOSTA** que motivou a reprovação anterior. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:346`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:358`)

3. **Bloqueia — RED(b) pode passar sem comparar com a main.** Todas as comparações dependem de `if os.path.exists(GOLDEN)`. Se o arquivo faltar, não ocorre falha nem comparação main × árvore. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:149`)

   Nesta cópia, o golden existe, contém tarefa/fila e CSF não vazios; o teste compara os resultados inteiros quando o encontra. A guarda de CSF vazio tem mordida explícita por `assertIn('P4', ...)`. A tarefa não tem guarda própria contra vazio; fica protegida pela comparação condicional com o golden. (`app/chamados/tests/f2_ressuscitar_tarefa_main.json:9`, `app/chamados/tests/f2_ressuscitar_tarefa_main.json:71`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:147`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:153`)

   O teste cobre três ressuscitações sintéticas. A geração efetiva do golden pela main e a igualdade sobre as 2.025 medidas fornecidas permanecem **[nao verificado]**. (`app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:111`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:146`)

4. **Bloqueia — a cobertura do ato não fecha a matriz fornecida.** A premissa do item 4 não corresponde ao código: pergunta sem validação, instante e texto vigentes executa `continue` e **não recebe REABERTURA**. Isso inclui pergunta sem atos e SILENCIO sem resposta/validação vigentes. Entretanto, a seleção exige materialização e a operação a retira. (`app/chamados/models.py:1543`, `app/chamados/models.py:1668`, `app/chamados/models.py:1680`, `app/chamados/models.py:3457`)

   Portanto não há “ato a mais” nesses casos; há retirada do carimbo sem novo ato por pergunta. A porta comum de reabertura, por comparação, considera expressamente a materialização retirada como condição suficiente para registrar ato. (`app/chamados/services/disputa_emissao.py:1638`, `app/chamados/services/disputa_emissao.py:1646`)

5. **Fila — permanecem leitores em laço sem prefetch de decisões.**

   - **Modal admin, disputa fechada:** o fio chama o explicador sem fornecer perguntas; ele as consulta sem decisões e lê `estado` em laço. O prefetch realizado por `pode_fechar` acontece depois desse laço. (`app/chamados/services/fio.py:348`, `app/chamados/services/explicador.py:160`, `app/chamados/services/explicador.py:171`, `app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:177`)
   - **Vigia:** `perg_todas` chega sem decisões; `dono_fora_do_fio` lê `p.estado` em `any`, podendo consultar decisões por pergunta. (`app/chamados/services/vigia_fio.py:570`, `app/chamados/services/vigia_fio.py:615`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/models.py:4267`)

   O teto novo mede exclusivamente `/api/disputa/perguntas/`; não cobre esses caminhos. O caminho principal do fio já carrega decisões e autores em lote. Contagens executadas desses outros caminhos: **[nao verificado]**. (`app/api/tests/test_f2_teto_consultas_app.py:49`, `app/chamados/services/fio.py:263`)

6. **Fila — o dono apresentado pelo explicador muda para colaborador.** Quando a compensação faz a validada ressuscitada passar a PENDENTE, o explicador devolve `dono='colaborador'` e “Cobrar o colaborador”. O modal apresenta “Aguardando o COLABORADOR responder”. A descrição anterior de VALIDADA seguia os ramos de sistema/cadastro. (`app/chamados/models.py:3213`, `app/chamados/services/explicador.py:95`, `app/chamados/services/explicador.py:137`, `app/templates/chamados/partials/modal_fio.html:467`)

   Isso é mudança concreta do **dono descrito**. Não demonstra mudança da fila oficial: o próprio explicador declara seu caráter descritivo, e `CargaDaTarefa` exclui a compensadora antes de escolher o último ato. (`app/chamados/services/explicador.py:174`, `app/chamados/services/tarefa_do_dia.py:426`)

7. **Fila — a foto pode guardar o autor errado em empate de instante.** `_vpor` escolhe a primeira VALIDACAO com o instante vigente. A ordem padrão é crescente por instante/pk, enquanto a leitura faz prevalecer a última. Duas validações no mesmo instante, autores A/B, podem produzir validação vigente B e foto atribuída a A; o modal mostra esse autor. Estoque desse caso: **[nao verificado]**. (`app/chamados/models.py:1670`, `app/chamados/models.py:4165`, `app/chamados/models.py:4273`, `app/chamados/models.py:4279`, `app/templates/chamados/partials/modal_fio.html:212`)

8. **Fila — censo dos campos gravados e lidos.** A nova linha é construída em `ressuscitar_perguntas` e persistida pela porta genérica `registrar`; as fotos legadas RESPOSTA/VALIDACAO são preservadas antes dela. (`app/chamados/models.py:1635`, `app/chamados/models.py:1672`, `app/chamados/models.py:1678`, `app/chamados/models.py:4298`)

   | Campo/valor | Escritor e leitores |
   |---|---|
   | `da_ressuscitacao` | Default falso no modelo/migration; verdadeiro no compensador. O predicado lê tipo + campo; a carga da tarefa usa o predicado. (`app/chamados/models.py:4160`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:18`, `app/chamados/models.py:1677`, `app/chamados/models.py:4186`, `app/chamados/services/tarefa_do_dia.py:426`) |
   | `ato=REABERTURA`, `ato_em` | Escritor usa agora. Leitura, SQL, histórico e tarefa ordenam/interpretam o ato; a tarefa descarta a marcada. (`app/chamados/models.py:1673`, `app/chamados/models.py:4273`, `app/chamados/models.py:3530`, `app/chamados/models.py:3779`, `app/chamados/services/tarefa_do_dia.py:422`) |
   | `ato_por` | Compensador grava nulo; o modal mostra autor quando existe. Autor não identifica ressuscitação. (`app/chamados/models.py:1673`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4186`) |
   | `ato_motivo` | Compensador grava texto com número do chamado; modal o mostra. A identificação da compensadora não o consulta, mas RESPOSTA ainda o usa para decidir validação. (`app/chamados/models.py:1677`, `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4186`, `app/chamados/models.py:4251`) |
   | `ato_texto` | Recebe resposta vigente da leitura; leitura e histórico consomem o texto. Na REABERTURA ele conserva a foto, enquanto o efeito do tipo limpa a resposta vigente. (`app/chamados/models.py:1674`, `app/chamados/models.py:4269`, `app/chamados/models.py:4283`, `app/templates/chamados/partials/modal_fio.html:212`) |
   | `respondida_original_em`, `validada_original_em`, `validada_original_por` | Compensador copia os valores anteriores; a reabertura comum também grava essas fotos. São conservação histórica, não os campos que a leitura usa para restaurar estado. (`app/chamados/models.py:1674`, `app/chamados/services/disputa_emissao.py:1632`, `app/chamados/models.py:4269`, `app/templates/chamados/partials/modal_fio.html:212`) |
   | `ato_instante` | Não integra a declaração deste modelo; o instante persistido é `ato_em`. Outro significado pretendido: **[nao verificado]**. (`app/chamados/models.py:4139`, `app/chamados/models.py:4141`) |

9. **Fila — censo dos leitores e efeitos.**

   | Leitor | Vê a compensadora? Resultado e alcance |
   |---|---|
   | `DecisaoPergunta.leitura` | Sim; não distingue marca. REABERTURA limpa validação, instante e texto quando vem depois na ordenação. (`app/chamados/models.py:4267`, `app/chamados/models.py:4281`) |
   | `estado`, `respondida`, `resposta_display` | Herdam a limpeza: deixam de conservar validação/resposta antigas; podem devolver PENDENTE, falso e texto vazio. (`app/chamados/models.py:3213`, `app/chamados/models.py:3764`, `app/chamados/models.py:3788`) |
   | `CargaDaTarefa` | Descarta a marcada antes do último ato; REABERTURA comum continua devolvendo ao colaborador. Preservação integral da frota: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:426`, `app/chamados/services/tarefa_do_dia.py:163`) |
   | C6 | Não lê a linha: lê projeção temporal. A limpeza da projeção muda seus insumos. (`app/chamados/juizes.py:1389`, `app/chamados/juizes.py:1404`) |
   | `q_vivas` e fio | A face SQL reconhece REABERTURA como retirada de validação; pode reincluir a ressuscitada no universo largo. O fio usa esse universo em disputas fechadas e depois consulta C6. (`app/chamados/models.py:3529`, `app/chamados/models.py:3591`, `app/chamados/services/fio.py:255`, `app/chamados/services/fio.py:270`) |
   | `perguntas_que_seguram_csf` | Delega a `sem_veredito_entre`, que exige resposta vigente, sem validação nem materialização. A compensadora normalmente impede que uma resposta só na trilha reapareça como bloqueio após a limpeza. Conservação sobre todas as exportadas: **[nao verificado]**. (`app/ponto/supra_juiz.py:573`, `app/chamados/models.py:1982`) |
   | `materializacao.py` | O rótulo BUG-PAR reconhece somente DEVOLUCAO com prefixo próprio; ignora a compensadora. A ausência rejeitada continua produzindo REABERTURA comum, com devolução ao colaborador. (`app/chamados/services/materializacao.py:275`, `app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1167`) |
   | `backfill_decisoes_pergunta` | Correspondência procura RESPOSTA/VALIDACAO; testemunho de `admin_decidiu` procura SILENCIO/VALIDACAO. REABERTURA não substitui esses testemunhos; ressuscitar remove via/materialização do universo. (`app/chamados/models.py:4216`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:314`, `app/chamados/models.py:3457`) |
   | `curar_perguntas_em_limbo` | Seleciona pela projeção e conjunto nominal; não interpreta a marca. Usa a porta comum para limpar. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:46`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`) |
   | `auditar_invariantes_chamados` | I18/I20 leem projeção; não conferem se a compensação efetivamente prevalece na sequência. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1076`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1106`) |
   | Histórico / foto E4b | REABERTURA integra `ATOS_COM_FOTO` e aparece em respostas anteriores. Foto E4b captura todas as decisões e campos concretos, incluindo a marca. (`app/chamados/models.py:4137`, `app/chamados/models.py:3779`, `app/chamados/services/foto_e4b.py:210`, `app/chamados/services/foto_e4b.py:280`) |

   A inclusão no histórico não incrementa o limite de reaberturas do colaborador: esse limite vem do campo da Disputa, não da contagem de REABERTURAS da sequência. (`app/chamados/services/fio.py:388`, `app/chamados/models.py:2294`)

10. **Fila — atomicidade e idempotência sequencial conferem.** Fotos, compensador e limpeza estão no mesmo `atomic`; a trava antecede as escritas e segue Colaborador → Disputa → Chamado → Pergunta. Uma exceção propagada no bloco não confirma apenas metade da operação. (`app/chamados/models.py:1629`, `app/chamados/models.py:1634`, `app/chamados/models.py:1678`, `app/chamados/models.py:1680`, `app/chamados/models.py:2831`, `app/chamados/models.py:2840`, `app/chamados/models.py:2853`, `app/chamados/models.py:2857`)

    Na segunda chamada sequencial, a pergunta já não tem materialização, sai do universo e não recebe outra REABERTURA. Há teste que exige retorno zero e quantidade de decisões inalterada. Execução: **[nao verificado]**. (`app/chamados/models.py:1543`, `app/chamados/models.py:1616`, `app/chamados/models.py:3457`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:381`)

11. **Fila — demais classes e selos.** `es_da_ressuscitacao` é predicado sobre procedência persistida; não julga respondida, validada ou viva. O relógio novo é consultado no escritor; a leitura da sequência não consulta relógio. (`app/chamados/models.py:4186`, `app/chamados/models.py:1662`, `app/chamados/models.py:4264`)

    A constraint garante apenas “marca implica REABERTURA”. Não garante a porta escritora: `registrar` aceita dicionários genéricos. O tripwire fiscaliza escritas literais, exige um sítio autorizado e contém exemplos externos; não constitui restrição de execução. Segundo escritor concreto: **[nao verificado]**. (`app/chamados/models.py:4178`, `app/chamados/models.py:4298`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:28`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:70`, `app/chamados/tests/test_f2_ressuscitacao_um_escritor.py:73`)

    RED(a), controle(c) e teto do app têm asserções substantivas; o teste de ordem alterado esvazia o universo da compensação e não exercita a inserção nova. Resultado executado da suíte: **[nao verificado]**. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:424`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:449`, `app/api/tests/test_f2_teto_consultas_app.py:56`, `app/chamados/tests/test_e4_expand.py:323`)

    A migration adiciona campo/default antes da constraint; o deploy oficial aplica migrations antes do HUP. Não há pouso invertido demonstrado nesse caminho. Aplicação da 0054 em produção e conservação efetiva de CSF/folha exportada: **[nao verificado]**. (`app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:20`, `bin/deploy.sh:317`, `bin/deploy.sh:360`)

## 17/11 11:24 · sessao 7178b1856410 · FOLHA-ENTRA-COM-DISPUTA-ABERTA · leitura · origem=Code

A causa é a diferença entre as perguntas dos dois juízes: **o export consulta a célula da janela; o PDF auditável consulta a existência de qualquer disputa aberta da pessoa, sem recorte temporal**. (`app/folha/export.py:1247`, `app/folha/export.py:1273`; `app/chamados/services/disputa_emissao.py:40`; `app/relatorios/pdf_espelho.py:1085`)

As referências abaixo são da cópia fornecida; sua correspondência ao HEAD do hash informado não pôde ser confirmada sem metadados Git [nao verificado].

**(i) Por que ENTRA e qual retenção não pegou**

`classificar_export` nasce com `status='entra'`. O motivo existente que poderia retirar essas pessoas é **`furo_espelho`**, acionado somente quando `motivos_retencao_espelho(...)` devolve motivos. (`app/folha/export.py:841`, `app/folha/export.py:968`)

Apesar do nome e dos argumentos recebidos, `motivos_retencao_espelho` **ignora `turnos_abertos`, `inconsistencias` e a grade passada**. Resolve a janela e delega a `motivos_retencao_celula`. Assim, nem disputa aberta nem contador de inconsistências elevado bastam para esse motivo pegar. (`app/folha/export.py:1301`)

O retentor efetivo percorre as células da janela e retém por:

- Célula ausente em dia passado com vínculo, ou veredito ausente nas condições previstas: `sem_celula`. (`app/folha/export.py:1252`, `app/folha/export.py:1267`, `app/folha/export.py:1295`)
- Veredito pertencente a `VEREDITOS_ACUSAM`, descontadas as exceções dos protestos não cobráveis e do furo só de pausa. (`app/escala/models.py:1767`; `app/folha/export.py:1273`, `app/folha/export.py:1293`)
- Isento e desligado saem antes, sem retenção por esse juiz. (`app/folha/export.py:1205`)

**A disputa não chega diretamente a esse predicado.** O comentário do fechamento espera que uma disputa que “sujou a janela” resulte em retenção por `furo_espelho`; entretanto, o leitor atual exige a acusação da célula, com aquelas exceções. (`app/ponto/services/fechamento.py:576`; `app/folha/export.py:1273`)

Portanto, dado o ENTRA informado pela sonda, esse ramo não encontrou motivo retentor naquela execução. **Qual célula/veredito/protesto explica individualmente cada um dos 49 não está no dado fornecido** [nao verificado]. Não é possível afirmar que todos tinham célula concorde, decisão administrativa ou uma mesma exceção [nao verificado].

`porta_export.py` exerce outra função: confere o carimbo da medição e pode recusar o arquivo inteiro. Seu total de falhas soma divergências entre leitores e a condição de HE configurada; não soma a existência de disputa aberta. (`app/folha/porta_export.py:687`, `app/folha/porta_export.py:807`, `app/folha/porta_export.py:818`; `app/folha/views.py:128`)

**(ii) Qual valor chega ao TXT**

**No caminho atual do TXT, a disputa não bloqueia o motor. O valor é recalculado na geração.** O fechamento executa `apurar_por_fatia`, que chama `motor.calcular_mes`; o gerador solicita esse fechamento com `somente_leitura=True, retornar_objeto=True` e passa os mesmos objetos à classificação e à montagem. (`app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:754`; `app/folha/views.py:207`, `app/folha/views.py:268`)

A promessa de “bypass total” está na docstring antiga do helper; sua execução apenas retorna a disputa aberta. Até a tela continua chamando o espelho, e registra expressamente que o comentário antigo estava ultrapassado. (`app/ponto/services/disputa.py:4`, `app/ponto/services/disputa.py:31`; `app/ponto/views.py:160`, `app/ponto/views.py:181`)

O caminho do valor é:

1. **Par/dia:** `calcular_periodo` produz os campos do período, depois acrescentado a `resultado.periodos` ou `periodos_ft`. (`app/ponto/motor_calculo_v2.py:2458`, `app/ponto/motor_calculo_v2.py:2492`)
2. **Turno aberto:** pode receber crédito **parcial dos pares já fechados**. O motor calcula esses pares e escreve `minutos_trabalhados` no período aberto; sem crédito, não faz essa atribuição. Isso é condição do turno, não consequência automática da disputa. (`app/ponto/motor_calculo_v2.py:2542`, `app/ponto/motor_calculo_v2.py:2553`, `app/ponto/motor_calculo_v2.py:2556`, `app/ponto/motor_calculo_v2.py:2559`)
3. **Total mensal:** as propriedades de `ResultadoMes` somam os períodos por rubrica; o fechamento escreve os atributos mensais correspondentes, inclusive no objeto entregue em leitura. (`app/ponto/motor_calculo_v2.py:295`, `app/ponto/motor_calculo_v2.py:321`, `app/ponto/motor_calculo_v2.py:324`; `app/ponto/services/fechamento.py:902`, `app/ponto/services/fechamento.py:918`, `app/ponto/services/fechamento.py:1135`)
4. **Evento exportável:** `eventos_do_fechamento` lê HE, folga trabalhada, noturno, intra e atraso/saída antecipada. `horas_trabalhadas` não tem evento próprio nessa lista. (`app/folha/export.py:439`)
5. **Linha:** o montador pula valor zero, resolve a rubrica e chama `montar_linha_10`; horas são multiplicadas por 100, arredondadas e formatadas em nove posições. A linha identifica competência e rubrica, sem data do dia. (`app/folha/export.py:730`, `app/folha/export.py:733`, `app/folha/export.py:738`, `app/folha/export.py:34`)

**Resposta entre as alternativas:** não existe “zero por disputa” nem preservação do “último gravado por disputa” nesse fluxo; existe cálculo atual, podendo conter crédito parcial de turno aberto. **Os valores efetivos dos dias dos 49 são [nao verificado].** (`app/folha/views.py:207`, `app/folha/views.py:268`; `app/ponto/motor_calculo_v2.py:2559`)

`DiaPago` é um ramo irmão: recebe os mesmos períodos quando há persistência ou lavratura expressamente solicitada. O montador do TXT recebe o objeto mensal calculado; não busca o valor diário no `DiaPago`. (`app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1201`; `app/ponto/services/dia_pago.py:173`; `app/folha/views.py:268`)

**(iii) Lei já escrita**

**lei: nenhuma** encontrada nos três documentos para decidir a regra geral “disputa aberta cujo fato pertence à janela, por si só, retém a pessoa” [nao verificado].

As candidatas têm estes alcances:

| Texto | O que já decide |
|---|---|
| **L-033** | Entrada e saída batidas, faltando só intervalo: pagável, mantendo a pergunta aberta sem bloquear. É exceção que impede equiparar toda pergunta/disputa aberta a retenção. (`app/docs/LEIS.md:81`) |
| **L-095** | Lavratura e leitura do valor; não estabelece esse predicado de retenção. (`app/docs/LEIS.md:113`) |
| **L-099** | Coerência e encaminhamento de CADASTRO/BATIDA ao admin. (`app/docs/LEIS.md:122`) |
| **L-113** | Calcular o que existe e permitir refazer a exportação. (`app/docs/LEIS.md:155`) |
| **L-129** | Dono da tarefa; não substitui o juiz de retenção. (`app/docs/LEIS.md:149`; `app/folha/fora_da_folha.py:115`) |
| **Corte do carimbo** | `turno_aberto` continua barrando o carimbo; trata de turno aberto, não de disputa aberta. (`app/docs/CORTES.md:317`) |
| **Dossiê da folha** | O crivo é o ENTRA de `classificar_export`; aprovação é carimbo, e TXT/recalculo não consultam tranca. (`app/docs/DOSSIES.md:120`, `app/docs/DOSSIES.md:122`) |

**(iv) Matriz do comportamento atual**

Aqui, “na janela” significa **data do fato**. O anexo distingue esse recorte de `aberta_em`: col193 pertence aos 49 com fato em novembro e abertura em setembro; col222 tem fato desconhecido e `aberta_na_janela=True` [nao verificado — anexo fornecido, sem arquivo:linha].

| Estado da disputa | Juiz do export | Juiz do espelho/PDF | O que pode sair no TXT |
|---|---|---|---|
| Aberta; fato na janela | O estado da disputa sozinho não retém. A célula da janela pode produzir `furo_espelho`. (`app/folha/export.py:968`, `app/folha/export.py:1273`) | Recusa auditável; lote tenta informacional. (`app/relatorios/pdf_espelho.py:1084`; `app/relatorios/services.py:244`) | Se ENTRA e demais condições liberarem: rubricas do cálculo atual, omitindo zeros. Se FORA: nenhuma linha dessa pessoa. (`app/folha/export.py:716`, `app/folha/export.py:726`, `app/folha/export.py:733`) |
| Aberta; fato fora da janela | Mesmo critério: o retentor examina as células da competência pedida. (`app/folha/export.py:1247`) | Mesma recusa: consulta sem filtro de datas. (`app/chamados/services/disputa_emissao.py:40`) | Mesmo comportamento; disputa fora da janela não impõe valor ou exclusão próprios. (`app/folha/export.py:968`, `app/folha/export.py:730`) |
| Fechada; sem outra aberta | Fechar a disputa não substitui a leitura da célula; acusação remanescente ainda pode reter. (`app/folha/export.py:1273`) | Esse bloqueio deixa passar; outras falhas ainda podem levar ao informacional. (`app/chamados/services/disputa_emissao.py:40`; `app/relatorios/services.py:244`) | Mesmo cálculo atual se admitida; nenhuma linha se retida. (`app/folha/views.py:207`, `app/folha/views.py:268`; `app/folha/export.py:726`) |

A emissão ainda filtra fechamento em estado admitido no TXT e depende da porta global; ENTRA não garante linha, pois eventos zero são omitidos. (`app/folha/export.py:717`, `app/folha/export.py:733`; `app/folha/views.py:128`)

**Quem escreve e quem lê os campos determinantes**

| Campo/resultado | Quem escreve | Quem lê neste caminho |
|---|---|---|
| Disputa: `colaborador`, `aberta_em` | Emissão cria o vínculo; ORM grava abertura com `auto_now_add`. (`app/chamados/services/disputa_emissao.py:601`; `app/chamados/models.py:1820`) | Juiz do PDF filtra colaborador; não usa abertura. (`app/chamados/services/disputa_emissao.py:40`) |
| Disputa: `fechada_em` | `fechar` preenche; reaberturas administrativa e sistêmica zeram. (`app/chamados/models.py:2013`, `app/chamados/models.py:2230`, `app/chamados/models.py:2254`) | `colaborador_em_disputa` testa NULL; PDF usa seu resultado. (`app/chamados/services/disputa_emissao.py:41`; `app/relatorios/pdf_espelho.py:1085`) |
| Disputa: `motivos`, `contexto` | Emissão cria e agrega. (`app/chamados/services/disputa_emissao.py:550`, `app/chamados/services/disputa_emissao.py:552`, `app/chamados/services/disputa_emissao.py:603`) | Não participam do predicado do PDF nem do retentor da célula. (`app/chamados/services/disputa_emissao.py:40`; `app/folha/export.py:1273`) |
| Sonda: `data_fato`, `aberta_na_janela` | Código da sonda não fornecido; escritor e derivação exatos [nao verificado]. | Usados no anexo; não integram os predicados citados. (`app/chamados/services/disputa_emissao.py:40`; `app/folha/export.py:1273`) |
| Célula: `veredito`, `protestos`, `ata` | Porta `lavrar_veredito`; decisão administrativa prevalece sobre cartório. (`app/ponto/portas/celula.py:1032`, `app/ponto/portas/celula.py:1060`, `app/ponto/portas/celula.py:1090`) | Retentor lê veredito/protestos; exceção da pausa consulta o juiz da ata. (`app/folha/export.py:1273`, `app/folha/export.py:1286`, `app/folha/export.py:1150`) |
| Períodos e números mensais | Motor calcula períodos; fechamento escreve os totais. (`app/ponto/motor_calculo_v2.py:2458`; `app/ponto/services/fechamento.py:902`) | Propriedades mensais somam; export lê eventos do fechamento. (`app/ponto/motor_calculo_v2.py:295`; `app/folha/export.py:439`) |
| `status` de classificação e `motivo` | `classificar_export`. (`app/folha/export.py:837`, `app/folha/export.py:970`) | Montador seleciona ENTRA. (`app/folha/export.py:716`) |
| Carimbo: `valor`, competência e detalhes | `porta_export.lavrar`. (`app/folha/porta_export.py:768`) | `conferir`, chamado pelo gerador. (`app/folha/porta_export.py:807`; `app/folha/views.py:128`) |

Resta uma decisão: **disputa aberta cujo fato pertence à janela deve, por si só, reter a pessoa, preservada a exceção L-033?** Essa decisão geral não foi localizada [nao verificado].

perguntas de lei: 1

## 17/11 11:23 · sessao 8710aac280f2 · - · - · origem=Ronald (tmux)

O219 | GRADE-MESTRA.md:64 | PENDENTE REAL | Versão/horizonte no código (app/ponto/services/cartorio.py:142); faltam TXT novo da 09 e Pauta DP (app/docs/BACKLOG.md:1287); espera atual [nao verificado]; fecho de censo integral [nao verificado] | outras | lei_ou_bang=N | censo=N
O228 | GRADE-MESTRA.md:65 | FEITA | Fecho inteiro escrito: b4d938aa, 7cb0efbb, 31fce87c (app/docs/BACKLOG.md:1291; app/docs/RELATO-ARQUIVO.md:39600); lápides presentes (app/ponto/precedencia.py:151; app/ponto/precedencia.py:548) | - | lei_ou_bang=N | censo=N
O218 | GRADE-MESTRA.md:66 | FEITA | FECHADA no main 1b6d5d02, bateria e prova de frota publicadas (app/docs/TICKETS.md:196); cenários/ponto fixo no código (app/ponto/tests/test_bateria_lavratura.py:169; app/ponto/tests/test_bateria_lavratura.py:193); registro antigo contraditório em app/docs/BACKLOG.md:1304 | - | lei_ou_bang=N | censo=N
O221 | GRADE-MESTRA.md:67 | FEITA | Pousos bb0bd0fa, d0625307, 52bfc524 documentados (app/docs/TICKETS.md:193; app/docs/RELATO-ARQUIVO.md:33156; app/docs/RELATO-ARQUIVO.md:33159); papel prazo permanece no código (app/config/crons.py:2128) | - | lei_ou_bang=N | censo=N
O129 | GRADE-MESTRA.md:68 | PENDENTE REAL | Declarar os 28 campos do wizard (app/docs/BACKLOG.md:1310); wizard continua fora das telas declaradas (app/core/configuracao_efeito.py:239); censo histórico de 28 em app/docs/BACKLOG.md:1310; espera atual [nao verificado] | outras | lei_ou_bang=N | censo=S
O117 | GRADE-MESTRA.md:69 | SEM PROVA | Medição encontrou zero casos e fecho foi condicional, sem encerramento efetivado demonstrado (app/docs/TICKETS.md:250; app/docs/RELATO-ARQUIVO.md:24143; app/docs/RELATO-ARQUIVO.md:24145); cura ou obra sucessora [nao verificado] | - | lei_ou_bang=N | censo=S
O105 | GRADE-MESTRA.md:70 | PENDENTE REAL | Espera ! entre substituir TXT e autopsiar datas_falta; comparação das três empresas isolou uma linha divergente (app/docs/BACKLOG.md:1336); export ainda deriva datas_falta durante geração (app/folha/export.py:740) | outras | lei_ou_bang=S | censo=S
O44 | GRADE-MESTRA.md:71 | PENDENTE REAL | Pousou 70d159b9, mas faltam baixa dos dois sítios mantidos, caso 1 pela tela e smoke do Ronald (app/docs/TICKETS.md:168); censo explicitamente fechado (app/docs/RELATO-ARQUIVO.md:974); complemento QUARENTENA interrompido por desenho (app/docs/BACKLOG.md:872) | outras | lei_ou_bang=S | censo=S
O54 | GRADE-MESTRA.md:72 | PENDENTE REAL | Programa parcial 20/29 (app/docs/RELATO.md:254); faltam portas V0–V4 (app/docs/BACKLOG.md:2267; app/docs/BACKLOG.md:2274), com 12 dias ainda sem nome (app/docs/BACKLOG.md:2270); quatro esperas Ronald registradas (app/docs/RELATO.md:246); censo integral fechado [nao verificado] | espelho-verdade (O54) | lei_ou_bang=S | censo=N
O95 | GRADE-MESTRA.md:73 | PENDENTE REAL | Resgate existe (app/ponto/turnos.py:1106; app/ponto/turnos.py:1502); DIFF do resgate entregue sem apply (app/docs/RELATO.md:278), enquanto candidata exigir volta não está escrita e contradiz selo (app/docs/RELATO.md:282); faltam definição/fecho da cura e censo integral; espera atual [nao verificado] | outras | lei_ou_bang=N | censo=N
O92 | GRADE-MESTRA.md:74 | PENDENTE REAL | Restam alimentação em lote dos quatro leitores enumerados (app/docs/BACKLOG.md:1466); chamadas por dia persistem (app/ponto/calculador/alimentacao.py:516; app/folha/porta_export.py:548; app/relatorios/pdf_espelho.py:457; app/ponto/models.py:895); cópia inline já removida pela CASA-ESCALA-E4 (app/escala/utils.py:2126); espera atual [nao verificado] | outras | lei_ou_bang=N | censo=S
O136 | GRADE-MESTRA.md:75 | PENDENTE REAL | Falta dispatch consultar partido nos dois sentidos, DIFF e ! do motor; censo de 341 templates isolou dois casos (app/docs/BACKLOG.md:1480); código ainda consulta como veto do ramo turno_partido (app/ponto/motor_calculo_v2.py:3659) | outras | lei_ou_bang=S | censo=S
CHAMADO-EM-RAIA | GRADE-MESTRA.md:76 | PENDENTE REAL | Escritor fechado por ESCRITOR-UNICO-DE-CHAMADO (app/docs/RELATO.md:5686; app/docs/RELATO.md:5690), mas célula juiz permanece parcial com condições O168/O169/LASTRO (app/core/contratos_estruturais.py:270); censo original publicado (app/chamados/tests/test_chokepoint_chamado_gate.py:32), fecho do censo restante e espera atual [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N

Contagem deste recorte: FEITA=3 PENDENTE=9 SUPERADA=0 SEM_PROVA=1 TOTAL=13. Quatro documentos consultados; histórico git direto indisponível nesta cópia sem `.git` [nao verificado].

## 17/11 11:22 · sessao 18b8ff1982d2 · - · - · origem=Ronald (tmux)

A premissa “motor bloqueado pela disputa” não descreve o caminho atual do TXT: a disputa só é consultada como flag na tela, que continua chamando o espelho; o fechamento calcula por fatias sem consultar disputa. A docstring S95 do wrapper promete bypass, mas sua execução apenas retorna a disputa aberta. (`app/ponto/views.py:142`, `app/ponto/views.py:161`, `app/ponto/views.py:181`, `app/ponto/services/disputa.py:18`, `app/ponto/services/disputa.py:31`, `app/ponto/services/fechamento.py:279`, `app/ponto/services/fechamento.py:286`.)

O TXT real usa o cálculo de agora, não o último gravado: `gerar_dominio_txt` chama `_recalc(...somente_leitura=True, retornar_objeto=True)` e passa seus objetos para `classificar_export` e `montar_linhas_export`. A consulta anterior de fechamentos persistidos serve à guarda de existência/carimbo; a montagem recebe `_fechs_lido`. (`app/folha/views.py:147`, `app/folha/views.py:163`, `app/folha/views.py:207`, `app/folha/views.py:212`, `app/folha/views.py:268`.)

Caminho do dia/par até a linha:

1. `calcular_periodo` escreve `PeriodoCalculo.minutos_trabalhados`; sem saída, retorna período aberto com campos numéricos inicialmente zero. Há três implementações: `app/ponto/motor_calculo_v2.py:213`, `:2750`, `:2752`, `:2785`, `:2877`, `:2879`, `:2893`, `:3215`, `:3217`.
2. O motor acrescenta o período a `resultado.periodos` ou `resultado.periodos_ft`. Para turno aberto, R3 pode calcular os pares já fechados e escrever o crédito parcial no período aberto; se não há crédito, não o atribui. Portanto, “turno aberto” também não implica zero obrigatório. (`app/ponto/motor_calculo_v2.py:2458`, `:2492`, `:2494`, `:2514`, `:2542`, `:2553`, `:2556`, `:2559`.)
3. As propriedades `ResultadoMes` leem os períodos e somam por rubrica: trabalhadas, noturnas, HE, atraso, saída antecipada e intra. (`app/ponto/motor_calculo_v2.py:295`, `:303`, `:321`, `:324`, `:328`, `:347`, `:351`, `:360`, `:363`, `:370`.)
4. `recalcular_fechamento_mes` escreve os atributos do objeto mensal a partir dessas propriedades — também em modo leitura. Há complementos de folga/feriado e crédito parcial. (`app/ponto/services/fechamento.py:890`, `:902`, `:918`, `:920`, `:923`, `:955`, `:965`, `:984`, `:1001`, `:1002`, `:1004`.) Em modo leitura, entrega o objeto e não executa os ramos de persistência. (`app/ponto/services/fechamento.py:1128`, `:1135`, `:1137`.)
5. `eventos_do_fechamento` lê os atributos mensais e emite eventos de HE50, HE100, feriado, folga, noturno, intra e atraso+saída antecipada. **Não existe evento próprio de `horas_trabalhadas` nessa lista.** (`app/folha/export.py:439`, `:441`, `:446`, `:447`, `:448`, `:451`.)
6. `montar_linhas_export` lê `(evento, valor)`, pula valor zero, resolve rubrica e chama `montar_linha_10`; esta converte horas em centesimal, arredonda e escreve o campo de nove posições. O TXT agrega por competência/rubrica, não produz uma linha por dia. (`app/folha/export.py:723`, `:730`, `:733`, `:735`, `:738`, `:34`, `:42`, `:43`, `:45`.)

`DiaPago` é ramo irmão: o mesmo fechamento chama `dia_pago.lavrar` quando há escrita ou pedido explícito de lavratura; `lavrar` lê os minutos dos períodos, soma por dia e substitui as linhas da competência. No fluxo puro de geração do TXT, `somente_leitura=True` não entra nesse ramo; o montador do TXT lê o objeto mensal injetado. (`app/ponto/services/fechamento.py:1191`, `:1201`, `:1203`, `app/ponto/services/dia_pago.py:173`, `:178`, `:180`, `:185`, `:359`, `:360`, `app/folha/views.py:208`, `:268`.)

Conclusão admissível: **valor recalculado por rubrica; pode haver crédito parcial em dia aberto; zero é omitido; não há mecanismo atual de preservar último valor por disputa. Os valores efetivos dos dias dos 49 são [nao verificado].** (`app/ponto/motor_calculo_v2.py:2559`, `app/folha/export.py:733`, `app/folha/views.py:208`, `:268`.)

## 17/11 11:22 · sessao 31c5238abdd1 · - · - · origem=Ronald (tmux)

Item 1: premissas centrais conferem; há duas lacunas de desenho que precisam ficar explícitas antes da construção. A existência do botão não prova transferência de tarefa, e o leitor atual não identifica ressuscitação por ato próprio. (`app/chamados/views_cobrar.py:34-57`; `app/chamados/services/tarefa_do_dia.py:142-177`; `app/chamados/models.py:1613-1630`.)

- **Sem ato:** `_pela_decisao` produz `aguarda_resposta`, ação `RESPONDER`, dono `COLABORADOR`. **SILENCIO:** produz `silencio_decidido`, ações vazias. A fila atual das duas classes está assim no código. (`app/chamados/services/tarefa_do_dia.py:156-177`.)
- **Escritor do SILENCIO:** encerramento da disputa por desligamento/admin, chamando `DecisaoPergunta.registrar` com `ato`, `ato_em`, `ato_por`, texto e motivo. O escritor final da sequência é `registrar`, que faz `bulk_create`. **Leitor:** carga ordenada por `ato_em, pk`, cortada pelo `agora`, seguida de `_pela_decisao`. (`app/chamados/models.py:1999-2007`; `app/chamados/models.py:4110-4120`; `app/chamados/services/tarefa_do_dia.py:421-424`; `app/chamados/services/tarefa_do_dia.py:158-177`.)
- **Escritor da ressuscitação:** `ChamadoColaborador.ressuscitar_perguntas`; conserva as antigas RESPOSTA/VALIDACAO na sequência e limpa `validada_em`, `validada_por`, `respondida_em`, `resposta_colab`, mais carimbo materializado/via, usando `descarimbar_em_lote`. Não grava ato compensatório nesta cópia. (`app/chamados/models.py:1613-1630`; `app/chamados/models.py:3391-3393`.)
- **Como reconhecer “ressuscitada”:** a prova atual específica é `LogAuditoria`, ação `perguntas_ressuscitadas`, com IDs antes/depois; a carga da tarefa lê perguntas e decisões, sem consultar essa prova. A seleção/discriminante que fará somente a ressuscitada com SILENCIO ir ao admin ainda não está definido no código. (`app/chamados/models.py:1651-1662`; `app/chamados/services/tarefa_do_dia.py:405-430`.)
- **Botão cobrar:** recebe `cid` de chamado, busca colaborador, verifica cobrança por colaborador/dia, envia push genérico ao atendimento e grava apenas nota `[COBRANCA]`; não muda o SILENCIO nem grava ação RESPONDER. Na lista, o botão está no ramo `verbo == "cobrar"`; o ramo `decidir` não o apresenta. Se “cobra colaborador” significa só notificar, confere; se significa devolver a tarefa, não fecha com este botão. (`app/chamados/views_cobrar.py:24-57`; `app/templates/chamados/partials/_lista_chamados.html:83-86`; `app/chamados/services/tarefa_do_dia.py:176-177`.)
- **Escritor/leitor da cobrança:** `views_cobrar` chama `nota.anexar_ocorrencia`, que cria `FollowupChamado`; o próprio `views_cobrar` lê essa nota para throttle e o painel lê para `cobrado_hoje`. (`app/chamados/views_cobrar.py:36-55`; `app/chamados/services/nota.py:75-109`; `app/chamados/services/painel.py:349-359`; `app/chamados/services/painel.py:395-396`.)
- **Dia do fato/competência:** nascimento passa o dia por `gravar_dia_do_fato`; a tarefa consulta `janela_atual(dia, empresa)` e o K6 em lote para saber entregue. O ponto de nascimento e o leitor estão identificados. (`app/chamados/models.py:3123-3127`; `app/chamados/services/tarefa_do_dia.py:467-483`; `app/ponto/janelas.py:60-63`.)
- **K6:** registro vivo aponta para `folha/export.py::competencia_entregue`; ele lê `ExportacaoDominio` vigente por empresa/mês/ano, recusando vigente invalidada. Escritor do registro é `ExportacaoDominio.registrar`; escritor da invalidação é `invalidar`. (`app/core/juizes.py:343`; `app/folha/export.py:580-594`; `app/folha/models.py:174-187`; `app/folha/models.py:143-162`.)
- **`competencia_ja_paga`:** busca em `app/**/*.py` não encontrou definição/chamada; implementação presente [não verificado]. O BACKLOG registra obra em cópia própria, com pouso pendente e cadastro “paga fora”. Não há dado raspado de cadastro real ou estoque nesta solicitação. (`app/docs/BACKLOG.md:1992`; dado real [não verificado].)
- **Lista DP/admin nova:** escritor da lista textual e novo ramo que produzirá ação/admin ainda não existem identificados; são construção futura [não verificado]. A fila administrativa atual vem de `qs_na_fila`, restrita a estados COBRANCA sem silêncio temporal; a propriedade “admin alcançável” também exige porta/tela reconhecida. (`app/chamados/services/painel.py:188-190`; `app/chamados/services/tarefa_do_dia.py:654-659`.)

Item 2 tem conflito executável preciso: o teste guarda `antes`, ressuscita e exige igualdade integral da tarefa depois; o caso SILENCIO usa essa função. F5 que transfira essa mesma classe ao admin fará a igualdade falhar quando coexistirem. Dizer “o teste segue como está” e apenas acrescentar outro em F5 não resolve a contradição final; seria preciso explicitar contexto distinto ou ajustar o contrato do teste. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319-335`.)

## 17/11 11:22 · sessao 1811a98d7545 · - · - · origem=Ronald (tmux)

Correção à linha O142, após conferir a revisão explícita do censo e o apply:

O142 | GRADE-MESTRA.md:78 | FEITA | Pouso d1689254 (app/docs/RELATO-ARQUIVO.md:37752); borda incorporada à cura (app/docs/RELATO-ARQUIVO.md:38964; app/ponto/turnos.py:1135) e três relavras aplicadas (app/docs/RELATO-ARQUIVO.md:38695) | - | lei_ou_bang=N | censo=N

## 17/11 11:22 · sessao 8b7303df858b · - · - · origem=Ronald (tmux)

Juiz do espelho é global por colaborador: `motor_bloqueado_por_disputa` chama `colaborador_em_disputa`; este seleciona qualquer disputa com `fechada_em IS NULL`, sem consultar `aberta_em`, data do fato, mês ou janela (`app/ponto/services/disputa.py:18`, `app/ponto/services/disputa.py:31`, `app/chamados/services/disputa_emissao.py:34`, `app/chamados/services/disputa_emissao.py:40`).

Matriz do espelho:

| Estado | Juiz do espelho | Resultado |
|---|---|---|
| Aberta, fato dentro da janela | Recusa auditável | Lote tenta informacional (`app/relatorios/pdf_espelho.py:1084`, `app/relatorios/services.py:244`, `app/relatorios/services.py:250`). |
| Aberta, fato fora da janela | Mesma recusa: filtro não tem data | Mesmo fallback (`app/chamados/services/disputa_emissao.py:40`, `app/relatorios/services.py:250`). |
| Fechada, sem outra aberta | Esse bloqueio deixa passar | Auditável pode sair; outras falhas ainda causam fallback (`app/chamados/services/disputa_emissao.py:40`, `app/relatorios/pdf_espelho.py:1085`, `app/relatorios/services.py:244`). |

O auditável passa `_bloqueio_disputa` ao pipeline e levanta `ValueError` com a disputa aberta (`app/relatorios/pdf_espelho.py:1084`, `app/relatorios/pdf_espelho.py:1113`). O pipeline executa bloqueios antes de criar o registro (`app/relatorios/pdf_base.py:129`); `RegistroPDFAuditavel` só nasce depois (`app/relatorios/pdf_base.py:152`). O informacional gera bytes diretamente e usa rodapé sem hash (`app/relatorios/pdf_espelho.py:1046`, `app/relatorios/pdf_espelho.py:1063`, `app/relatorios/pdf_espelho.py:1070`). Portanto ter um registro auditável antigo não prova que o bloqueio atual passou (`app/relatorios/pdf_base.py:152`, `app/chamados/services/disputa_emissao.py:40`).

Quem escreve e quem lê:

| Campo | Escrita | Leitura relevante |
|---|---|---|
| `DisputaSupervisao.colaborador` | `_abrir_ou_agregar_disputa` cria vínculo (`app/chamados/services/disputa_emissao.py:601`) | `colaborador_em_disputa` filtra pessoa (`app/chamados/services/disputa_emissao.py:40`). |
| `fechada_em` | Nasce nulo pela definição (`app/chamados/models.py:1821`); `fechar` grava instante (`app/chamados/models.py:2013`); `reabrir_admin` e `reabrir_sistema` zeram (`app/chamados/models.py:2230`, `app/chamados/models.py:2254`). | Juiz global lê nulo (`app/chamados/services/disputa_emissao.py:41`), alcançando o bloqueio do auditável pelo wrapper (`app/ponto/services/disputa.py:31`, `app/relatorios/pdf_espelho.py:1085`). |
| `aberta_em` | ORM `auto_now_add` no nascimento (`app/chamados/models.py:1820`, `app/chamados/services/disputa_emissao.py:601`). | Não é lida pelo juiz do espelho (`app/chamados/services/disputa_emissao.py:40`). |
| `motivos` | Nascimento (`app/chamados/services/disputa_emissao.py:603`), agregação (`app/chamados/services/disputa_emissao.py:550`); há também métodos de alteração, portanto não afirmar escritor único absoluto desse campo (`app/chamados/models.py:2033`, `app/chamados/models.py:2236`). | Não participa da decisão do espelho; qualquer disputa aberta serve (`app/chamados/services/disputa_emissao.py:40`). |
| `contexto` | Nascimento (`app/chamados/services/disputa_emissao.py:604`), merge da varredura (`app/chamados/services/disputa_emissao.py:552`, `app/chamados/services/disputa_emissao.py:576`). | Não participa do juiz do espelho (`app/chamados/services/disputa_emissao.py:40`). |
| `RegistroPDFAuditavel` | Pipeline `gerar_pdf_auditavel` (`app/relatorios/pdf_base.py:152`). | O retorno do serviço carrega registro (`app/relatorios/services.py:92`); o anexo lê existência histórica, mas esse leitor da sonda não está demonstrado em arquivo da cópia [nao verificado]. |

Cuidado com a definição temporal do pedido: os 49 são classificados no anexo pela **data do fato**; não são necessariamente disputas cujo `aberta_em` está na janela. Exemplo dado: col193 tem fato 15/11 e abertura 21/09; col222 tem fato ausente e `aberta_na_janela=True` por abertura 09/11. A implementação da sonda não foi fornecida, então não atribuir a `aberta_na_janela` significado diferente do que esses exemplos demonstram [nao verificado].

## 17/11 11:22 · sessao 49b177462bcd · - · - · origem=Ronald (tmux)

Achados de F5/F6, só leitura:

- As matrizes próprias não vieram materialmente nesta cópia: os documentos apontam para `logs/ESPINHA-BRIEFS/<f>/matriz.md`, fora de `app/docs`; conteúdo literal atual dessas matrizes = **[não verificado]**. Ponteiros: `app/docs/RELATO.md:298`, `app/docs/RELATO.md:313`, `app/docs/RELATO.md:425`. Portanto não afirmar que “F5 e F6 já estão RESOLVIDO-PELO-CORTE” nem certificar todo o conteúdo delas.
- O registro existente diz que as perguntas de lei de F5/F6 já passaram a zero pelo corte anterior; isto é registro documental, sem medição nova: `app/docs/RELATO.md:214`, `app/docs/CORTES.md:488`. O corte anterior de F5 conserva a resposta vazia fora do app e dá a ação ao admin; F6 manda derivar `aguarda_admin` do dono publicado por `tarefa_do_dia`, com contador zero para estados sem dono (`app/docs/CORTES.md:488`).
- O código atual confirma `sem decisão -> aguarda_resposta / responder / colaborador` e `SILENCIO -> silencio_decidido / []`: `app/chamados/services/tarefa_do_dia.py:156`, `app/chamados/services/tarefa_do_dia.py:176`. É incorreto estender “sem ninguém” a todo o dia: os verbos residuais da fila podem produzir dono admin/chamado/sistema (`app/chamados/services/tarefa_do_dia.py:665`), e a oferta de resolver dia pode dar admin mesmo sem ação da pergunta (`app/chamados/services/tarefa_do_dia.py:684`).
- A distinção “F2 conserva, F5 altera” é compatível como fronteira de construção, mas há condição literal importante: a asserção em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:329` compara a tarefa completa antes/depois e é aplicada justamente ao SILENCIO em `:334-335`. Se a futura F5 identificar a ressuscitação dessa fixture e trocar sua tarefa para admin, manter esse teste **literalmente inalterado na árvore combinada** conflita com o resultado novo. Falta explicitar o escopo temporal da prova de invariância ou como a prova F5 convive com ela; resultado na árvore futura = **[não verificado]**.
- O escritor atual da ressuscitação é `ChamadoColaborador.ressuscitar_perguntas`: conserva resposta/validação anteriores na trilha por `DecisaoPergunta.registrar` (`app/chamados/models.py:1613`), limpa `validada_em/validada_por/respondida_em/resposta_colab` pela porta de lote (`app/chamados/models.py:1629`); a documentação do método diz expressamente que ainda não grava ato de devolução (`app/chamados/models.py:1565`). O leitor da fila lê as decisões ordenadas por `ato_em, pk` e recorta por `agora` (`app/chamados/services/tarefa_do_dia.py:421`).
- Campos derivados `razao/acoes/acao/de/porta`: escritor em memória `_pela_decisao/_acao` (`app/chamados/services/tarefa_do_dia.py:135`, `:142`), sem escritor persistente (`:733-736`); projeção da porta publica `rota/fila/verbos/fio/nao_sei/acao/ausencia` (`:708`, `:712-725`).
- `aguarda_admin` hoje ainda é montado no endpoint como `p.estado == PerguntaDisputa.RESPONDIDA`, não pela tarefa (`app/api/views.py:2549-2553`). O endpoint também lê `resposta_colab/respondida_em` diretamente (`:2561-2563`); seleção do app lê `respondida_em is None` e disputa aberta (`app/chamados/services/pergunta_no_app.py:76-85`). Isso dá conteúdo concreto à migração F6, mas o cliente que consome o JSON nesta cópia = **[não verificado]**.
- Porta única da trilha: `DecisaoPergunta.registrar` escreve por `bulk_create` e acorda as células (`app/chamados/models.py:4110-4122`); campos disponíveis `ato/ato_em/ato_por/ato_texto/ato_motivo` em `:4038-4043`. Não há campo persistente “dono” mostrado pela tarefa: o dono é calculado (`app/chamados/services/tarefa_do_dia.py:662-670`).

## 17/11 11:22 · sessao 2384d85bc413 · - · - · origem=Ronald (tmux)

FERIADO-DIA-CHAVE | GRADE-MESTRA.md:91 | PENDENTE REAL | Unificar dia-chave e predicado: ainda há k + conjunto de feriados no calculador versus data da entrada no fechamento (app/ponto/calculador/alimentacao.py:521; app/ponto/services/fechamento.py:949); falta caso atravessando meia-noite (app/docs/BACKLOG.md:1623); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
LISTA-POR-COMPETENCIA | GRADE-MESTRA.md:92 | PENDENTE REAL | Falta uma linha por competência aberta (app/docs/BACKLOG.md:1625); a lavra ainda mede uma janela agregada por colaborador (app/ponto/services/esmeril_espelho.py:38; app/ponto/services/esmeril_espelho.py:219); censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
REGUA-DO-PERIODO-COMPLEMENTO-19-10 | GRADE-MESTRA.md:93 | PENDENTE REAL | Complemento exige leitores por fatia, censo 09/10 e encaminhamento/apply por dono (app/docs/BACKLOG.md:1725); a obra-mãe declara três leitores restantes (app/docs/BACKLOG.md:1706), e o vigia ainda usa um vínculo para a janela inteira (app/ponto/management/commands/vigiar_motor_autoridade.py:51); fechamento integral do censo e trava vigente da pergunta 18 [nao verificado] | outras | lei_ou_bang=N | censo=N
O327 | GRADE-MESTRA.md:94 | PENDENTE REAL | Falta cobertura completa e publicação das recusas nomeadas (app/docs/BACKLOG.md:1753); registro posterior ainda aponta 87 recusas idênticas nos dois lados e espera lei do ancoramento (app/docs/RELATO.md:1294); censo fechado atual [nao verificado] | espelho-verdade (O54) | lei_ou_bang=S | censo=N
RETIFICACAO-DA-10-8069-PRIMEIRO | GRADE-MESTRA.md:95 | PENDENTE REAL | Falta resolver o resíduo e retificar pela porta; 243 espera corte do Ronald (app/docs/BACKLOG.md:1757); premissa da 8069 foi corrigida: candidato estrutural ≤10,67 h, sem código/TXT/gravado alterado (app/docs/RELATO.md:6008; app/docs/RELATO.md:6015); censo fechado atual [nao verificado] | outras | lei_ou_bang=S | censo=N
REABRIR-PERGUNTA-A-PREMISSA | GRADE-MESTRA.md:96 | PENDENTE REAL | Porta no ar bdfad18f (app/docs/RELATO.md:4749), mas falta prova do PRONTO: contador zero em 24 h e sete dias sem laço (app/docs/RELATO.md:4751; app/docs/BACKLOG.md:1777); resultado final e censo fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
LOTE-VAZA-ENTRE-COLABS | GRADE-MESTRA.md:97 | PENDENTE REAL | Falta medir causa e provar lote igual ao individual: divergência de uma linha registrada no HEAD da medição (app/docs/BACKLOG.md:1800; app/docs/RELATO.md:4361); cura/pouso e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
PASSADO-SEM-MARCOS-SEM-TETO | GRADE-MESTRA.md:98 | SUPERADA/ABSORVIDA | Tratamento incorporado à E4a-1, no ar 892159c5f (app/docs/RELATO.md:1729), e MODO-COMUM-PASSADO-SEM-TURNO fechado (app/docs/BACKLOG.md:2147; app/docs/RELATO.md:1722); passado sem turno vai ao cadastro sem pergunta ao colaborador (app/chamados/services/fabrica_da_celula.py:20) | - | lei_ou_bang=N | censo=N
LEGADO-51-ACESAS | GRADE-MESTRA.md:99 | PENDENTE REAL | Falta medir as 51 pela função real e fechar pela porta as que a lei encerra (app/docs/BACKLOG.md:1815); resultado desse universo, pouso/aplicação e censo fechado [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
DIA-DA-BATIDA-JUIZ-UNICO | GRADE-MESTRA.md:100 | PENDENTE REAL | Falta concluir censo AST com classificação dos sítios, migrar juízos paralelos e entregar selo/DIFF por migração (app/docs/BACKLOG.md:1818); rejulgamento prévio de col33 tem prova (app/docs/RELATO.md:4262), mas fechamento da obra e do censo [nao verificado] | contrato 20 | lei_ou_bang=N | censo=N
PERGUNTA-DO-DIA-EM-CURSO | GRADE-MESTRA.md:101 | FEITA | 165fee20 no ar e push provado (app/docs/RELATO.md:4236); pouso documentado (app/docs/RELATO.md:4255); teto por instante e veto ao futuro presentes no código (app/chamados/services/fabrica_da_celula.py:8) | - | lei_ou_bang=N | censo=N
R1-ESTRUTURA-91 | GRADE-MESTRA.md:102 | PENDENTE REAL | A-i e flip têm fechamento próprio (app/docs/BACKLOG.md:1790; app/docs/BACKLOG.md:1839), mas faltam causas/cura das divergências restantes: R1 atual totaliza 57 dias ESTRUTURA e investigação ainda incompleta (app/docs/RELATO.md:229; app/docs/RELATO.md:227; app/docs/RELATO.md:221); lei/! vigente e censo fechado desse restante [nao verificado] | espelho-verdade (O54) | lei_ou_bang=N | censo=N
BATERIA-QUEIXAS-ADMIN | GRADE-MESTRA.md:103 | PENDENTE REAL | Faltam validação Ronald+DP e selos após FIO MUDO (app/docs/BACKLOG.md:1901; app/docs/RELATO.md:2481); conclusão dos casos Q1–Q4, validação e censo fechado [nao verificado] | outras | lei_ou_bang=N | censo=N
TIPO-NA-FONTE | GRADE-MESTRA.md:104 | PENDENTE REAL | Falta corte LU2-03 entre selo do valor e acessor tipado, seguido da construção em cópia nova (app/docs/RELATO.md:344); censo explicitamente fechado, item 3 apenas na cópia 12e424d5f (app/docs/RELATO.md:827); dependências e parada registradas (app/docs/BACKLOG.md:1928) | contrato 20 | lei_ou_bang=S | censo=S

Notas para consolidação: foram pesquisados também RELATO-ARQUIVO.md e TICKETS.md, sem referência nominal às 14 obras. A atribuição dos grupos acima é inferência temática; principalmente “outras” versus O54 e “contrato 20” para TIPO/REABRIR deve seguir a convenção do consolidador. Ausência de censo fechado significa ausência de prova localizada, não prova de inexistência. A referência do rejulgamento em DIA-DA-BATIDA deve ser preferencialmente app/docs/RELATO.md:4263–4266 após conferência; não consegui fixar a linha individual no último recorte.

_6514 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
