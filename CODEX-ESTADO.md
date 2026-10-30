# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 13:40** · publicado em **30/10/2026 13:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 13:40 · sessao bdc58fb8ddda · O261-VETO-A2 · censo · origem=Code

As referências aos anexos contam a primeira linha após cada cabeçalho `== anexo: ... ==` como linha 1.

1. **RED contra a base — classe (c), fora do alcance.**  
   **Pergunta:** incluía “selo/RED sem mordida”. (`pergunta_final.q:8`)  
   **Resposta:** delimitou “Execução da bateria Django [...] [nao verificado]”. Não há trecho que identifique o RED executado na cópia intermediária. (`codex_final.txt:3`)  
   O achado depende da execução histórica e da base efetivamente usada, informadas somente no relato do Opus; sua reprodução independente é **[nao verificado]**. (`opus_achados.txt:2`)

2. **Linha LEI-AKITA desatualizada — classe (a), não apontado com o tema presente.**  
   **Pergunta:** exigia “censo quem-mais-le incompleto” e “QUEM ESCREVE e QUEM LE cada campo/valor”. (`pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** identificou expressamente o leitor: “desfazer corroboracão filtra pelo vínculo”, citando `desfazer_corroboracao_regua.py:38`; também examinou a reabertura direta. Portanto, viu esses caminhos, mas não apontou sua ausência na declaração LEI-AKITA. (`codex_final.txt:21`; `codex_final.txt:50`)  
   A omissão documental específica não consta da triagem fornecida; não há sustentação para classificá-la como descarte pela triagem. (`triagem_final.txt:2–16`)

3. **Impressão com `ec=None` sem teste próprio — classe (a), não apontado com o tema presente.**   [nao verificado]
   **Pergunta:** nomeava explicitamente “ec=None vale TAMBEM para a impressao”, os leitores de cartório/impressão e a classe “selo/RED sem mordida”. (`pergunta_final.q:6`; `pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** descreveu que o cartório “Acrescenta `veto:VETO_DEGENERADA` à impressão do ramo normal”, mas não denunciou a falta de teste desse contrato nem o mutante indicado pelo Opus. (`codex_final.txt:28`; `opus_achados.txt:4`)   [nao verificado]
   O achado colocado em FILA foi outro: impressão do ramo `nunca_bateu` sem veto. Isso não demonstra descarte do achado sobre cobertura de `ec=None` no ramo normal. (`codex_final.txt:58`; `triagem_final.txt:8`)

4. **Helper com folga no fim de semana — classe (a), não apontado com o tema presente.**  
   **Pergunta:** nomeava o helper e perguntava se algum teste “ficou verde pelo motivo errado” ou “ficou vacuo”. (`pergunta_final.q:12`)   [nao verificado]
   **Resposta:** declarou “sem achado de intenção ‘sem vínculo/sem escala’ mascarada pela migração” e apontou dois testes que não provavam nascimento; não apontou a dependência do calendário descrita pelo Opus. (`codex_final.txt:36`; `codex_final.txt:38`; `opus_achados.txt:5`)   [nao verificado]
   Classifico a configuração do helper como inspecionável pelo código. A contagem histórica de 27 módulos e o resultado efetivo da suíte permanecem **[nao verificado]**. A cópia atual já usa sete dias de trabalho e atribui essa alteração ao Opus final. (`app/chamados/tests/_vinculo_do_dia.py:5–6`; `app/chamados/tests/_vinculo_do_dia.py:18`)

5. **`recusado_veto` contado por eliminação — classe (a), não apontado com o tema presente.**  
   **Pergunta:** exigia examinar quem determina e lê cada campo/valor, incluindo os “emissores”, além de procurar derivações paralelas de fonte única. (`pergunta_final.q:8`; `pergunta_final.q:11`)  
   **Resposta:** examinou nominalmente o retroativo: “lê indiretamente o juiz pelo dry `veto_no_nascimento`”. Não apontou a classificação da recusa no caminho de aplicação. (`codex_final.txt:34`)  
   A cópia atual contém uma consulta explícita ao juiz antes de incrementar `recusado_veto`, com comentário atribuindo a correção ao Opus; o defeito anterior é o registrado no anexo. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:179–187`; `opus_achados.txt:6`)

6. **Condições operacionais do pouso e medidas — classe (c), fora do alcance, com risco já visto.**   [nao verificado]
   **Pergunta:** exigia “reversao antes, exportada intacta por hash” e procurar “pouso antes do pre-requisito”; também nomeava `furos_sem_cobranca`. (`pergunta_final.q:6`; `pergunta_final.q:8`; `pergunta_final.q:11`)   [nao verificado]
   **Resposta:** registrou “Reversão anterior, preservação por hash e diferença à Pauta DP: [nao verificado]”. Na DELTA, reiterou: “Item 9 continua pendente de pouso, embora esteja como MEDINDO”. (`codex_final.txt:64`; `codex_delta.txt:45`)   [nao verificado]
   A triagem manteve a medida “antes de qualquer pouso”; portanto, há evidência de reconhecimento da pendência, sem evidência de descarte. A execução das condições, a divergência “63 × 1.576” e os resultados antes/depois não são demonstrados pelo material fornecido: **[nao verificado]**. (`triagem_final.txt:15–16`; `opus_achados.txt:7`) [nao verificado]

7. **Ato humano sem mostrar/gravar o veto ultrapassado — classe (a), não apontado com o tema presente.**  
   **Pergunta:** trazia literalmente “ato humano passa com trilha”; a DELTA 2 voltou a pedir conferência das três cadeias `humano=True`. (`pergunta_final.q:5`; `pergunta_delta2.q:4`)  
   **Resposta:** declarou “sem achado nas três cadeias humanas atuais”, verificando permissões e origem humana, mas não apontou a ausência do veto ultrapassado na apresentação/auditoria. (`codex_delta2.txt:7–9`)  
   Na cópia, `humano=True` dispensa a consulta ao veto nessa guarda; as notas e auditorias examinadas registram o ato administrativo sem nomear esse veto. Isso sustenta a distinção entre registrar a ação humana e registrar qual veto ela ultrapassou. (`app/chamados/models.py:1090–1093`; `app/chamados/services/acoes_disputa.py:266–270`; `app/chamados/services/competencia_trancada.py:258–264`)

| Achado | Classe | Referência principal |
|---|---|---|
| 1. RED contra a base | **(c)** | `codex_final.txt:3`; `opus_achados.txt:2` |
| 2. LEI-AKITA desatualizada | **(a)** | `pergunta_final.q:8`; `codex_final.txt:21` |
| 3. Teste da impressão com `ec=None` | **(a)** | `pergunta_final.q:6`; `codex_final.txt:28` | [nao verificado]
| 4. Helper dependente do calendário | **(a)** | `pergunta_final.q:12`; `codex_final.txt:36–38` |
| 5. Contador por eliminação | **(a)** | `pergunta_final.q:11`; `codex_final.txt:34` |
| 6. Condições e medidas do pouso | **(c)**, com pendência reconhecida | `codex_delta.txt:45`; `triagem_final.txt:15–16` | [nao verificado]
| 7. Trilha do veto ultrapassado pelo humano | **(a)** | `pergunta_final.q:5`; `codex_delta2.txt:7–9` |

## 30/10 13:36 · sessao ff42a8bee5de · O261-VETO-A2 · sonda · origem=Code

`sonda_fsc.py` não foi localizado na cópia; não houve nova execução. Segue integralmente a saída fornecida. [nao verificado]

```text
sonda_leitura: sonda_fsc.py [fe36a5107d6c7951] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TOTAL=61
POR_PORTA|pergunta_ao_colab=45
POR_PORTA|supervisao_sem_push=16
POR_PORTA|veto_celula=1
POR_EMPRESA|2=61
ITENS=62
FIM-DA-SONDA
```

## 30/10 13:30 · sessao 3a14b0861375 · CALENDARIO-HOJE · censo · origem=Code

Analisei a árvore disponível em modo de leitura. A correspondência exata dela com o commit `d2c37edd16a8b0ca3aa26c2eab9f60de661fc675` ficou **[nao verificado]**, pois o snapshot não oferece os metadados Git necessários à conferência.

**1. Classificador e resultado para HOJE**

A função é `_classificar(f)`: recebe os fatos já carregados e devolve uma string de status. O calendário passa `'futuro': data > hoje`; portanto, **HOJE recebe `False`**. (`app/colaboradores/services/calendario.py:104`, `app/colaboradores/services/calendario.py:779`, `app/colaboradores/services/calendario.py:785`)

Para trabalho sem batida, sem isenção, escala suspeita, cobertura ou veredito que altere o resultado:

| Valor de `futuro` | Resultado |
|---|---|
| `True` | `previsto`. (`app/colaboradores/services/calendario.py:130`, `app/colaboradores/services/calendario.py:131`) |
| `False` | `falta`. É o caminho comum de HOJE na implementação atual. (`app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:140`, `app/colaboradores/services/calendario.py:785`) |

Essas três premissas — hoje, trabalho e nenhuma batida — não bastam para garantir `falta`: com `futuro=False`, isento recebe `ok`, escala suspeita recebe `escala_suspeita`, cobertura sem desconto recebe `ausencia`; o veredito também pode substituir o status. Ausências e feriado têm precedência anterior; cobertura sem desconto pode transformar até `previsto` em `ausencia`. (`app/colaboradores/services/calendario.py:120`, `app/colaboradores/services/calendario.py:123`, `app/colaboradores/services/calendario.py:133`, `app/colaboradores/services/calendario.py:135`, `app/colaboradores/services/calendario.py:164`, `app/colaboradores/services/calendario.py:175`, `app/colaboradores/services/calendario.py:179`)

Os status existentes são **`ok`, `irregular`, `folga_trabalhada`, `falta`, `escala_suspeita`, `aberto`, `previsto`, `folga`, `ausencia`, `sem_certidao`, `sem_dados`**. Hora extra é selo separado, não status. (`app/colaboradores/services/calendario.py:44`, `app/colaboradores/services/calendario.py:60`, `app/colaboradores/services/calendario.py:116`)

**2. Quem lê este calendário**

A chave **`futuro` é interna ao classificador**: não integra o dicionário exportado de cada dia. Os leitores recebem `status`, `status_rotulo`, `eh_trabalho`, `resolver_dia_oferecido` e os demais campos; outras grades têm suas próprias chaves chamadas `futuro`. (`app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:855`, `app/colaboradores/services/calendario.py:889`, `app/colaboradores/services/calendario.py:904`)

| Leitor | O que mostra ou decide | Regra própria de hoje/futuro |
|---|---|---|
| **Perfil do colaborador no admin** | A view monta o calendário e renderiza o partial; a grade usa status para fundo/borda e ícone: falta vermelha, previsto azul, ok verde, irregular amarelo etc. Ausência e palavra do dia podem ocupar o lugar do ícone. (`app/colaboradores/views.py:1945`, `app/colaboradores/views.py:1962`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/templates/colaboradores/partials/_calendario_grade.html:97`) | A view acrescenta outro `hoje=localdate()` quando há permissão de regularização; a grade lê o carimbo `resolver_dia_oferecido`, sem refazer a comparação temporal para oferecer os botões. (`app/colaboradores/views.py:1957`, `app/colaboradores/views.py:1960`, `app/templates/colaboradores/partials/_calendario_grade.html:125`) |
| **Gaveta de diagnóstico/painel** | Monta calendário customizado e reutiliza a mesma grade em fita, com os mesmos status e totais. (`app/colaboradores/services/drawer.py:120`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`) | Usa `localdate()` para escolher competência atual e anterior e aplicar piso visual; não reclassifica o status recebido. (`app/colaboradores/services/drawer.py:111`, `app/colaboradores/services/drawer.py:113`, `app/colaboradores/services/drawer.py:118`) |
| **Mapa de furos HTML** | Cada card inclui este calendário. Entretanto, seleção dos cards, lista de marcos faltantes, `nfuro` e ordenação vêm de `grade_da_celula → estado_do_dia`, independentemente do status deste calendário. (`app/relatorios/furos.py:141`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/relatorios/furos.py:184`, `app/relatorios/furos.py:190`, `app/templates/relatorios/furos.html:42`) | A view usa `localdate()`; o período padrão termina hoje, enquanto o período explícito não recebe ali um teto em hoje. (`app/relatorios/views.py:1518`, `app/relatorios/views.py:1531`, `app/relatorios/views.py:1537`, `app/relatorios/views.py:1541`) |
| **PDF do mapa de furos** | Extrai `data → status` dos dias do calendário e pinta o minicalendário conforme esse status. Status sem entrada nos mapas de cores recebem fundo branco/texto cinza. O número de furos continua vindo da outra contagem. (`app/relatorios/views.py:1548`, `app/relatorios/views.py:1551`, `app/relatorios/furos_pdf.py:11`, `app/relatorios/furos_pdf.py:21`, `app/relatorios/furos_pdf.py:56`, `app/relatorios/furos_pdf.py:66`) | O gerador recebe o período da view e percorre as semanas prontas; não compara com o relógio nesse render. (`app/relatorios/views.py:1562`, `app/relatorios/furos_pdf.py:25`, `app/relatorios/furos_pdf.py:47`) |
| **Resolver dia / Pauta DP** | O serviço deriva `resolver_dia_oferecido` para status `falta`, `irregular`, `aberto`, `sem_certidao`, exigindo último recurso e trabalho. O template acrescenta a permissão e oferece seleção em lote, Resolver dia e DP. (`app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:196`, `app/templates/colaboradores/partials/_calendario_grade.html:125`) | Exige **passado estrito: `data < hoje`**. Assim, falta de HOJE não oferece essa ação por este predicado. (`app/colaboradores/services/calendario.py:196`) |
| **Vigia do fio morto** | Chama a porta de um dia `resolver_dia_oferecido_na_data`; resultado verdadeiro entra em `decisao_do_dia_pendente` e deixa de seguir como fio morto. (`app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:208`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:298`) | Tem `hoje` próprio: conta `data > hoje` no tripwire de futuro e separa `data >= hoje` antes de perguntar ao calendário. O calendário chamado captura outro relógio. (`app/chamados/services/vigia_fio.py:181`, `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:289`, `app/colaboradores/services/calendario.py:249`) |
| **Contadores internos / rodapé** | `resumo` conta literalmente os status; HE conta o selo. O rodapé mostra `faltas` como **“Sem registro”**, além de Ok, HE, trabalho em folga, irregulares, escala suspeita, folgas e abertos; `ausencias` e `previstos` são calculados, mas não têm bloco nesse rodapé. (`app/colaboradores/services/calendario.py:908`, `app/colaboradores/services/calendario.py:923`, `app/colaboradores/services/calendario.py:925`, `app/templates/colaboradores/partials/_calendario_grade.html:141`, `app/templates/colaboradores/partials/_calendario_grade.html:145`, `app/templates/colaboradores/partials/_calendario_grade.html:173`) | A contagem percorre os status já produzidos, sem corte temporal adicional. (`app/colaboradores/services/calendario.py:908`) |
| **Portas `status_do_dia` / `contar_por_status`** | Montam respectivamente um dia ou um período; devolvem o status ou `Counter(status)`. (`app/colaboradores/services/calendario.py:211`, `app/colaboradores/services/calendario.py:220`, `app/colaboradores/services/calendario.py:223`, `app/colaboradores/services/calendario.py:231`) | Herdam o relógio de `contexto_calendario`. Não localizei chamadores de produção dessas duas portas; ausência de chamadas dinâmicas **[nao verificado]**. (`app/colaboradores/services/calendario.py:217`, `app/colaboradores/services/calendario.py:228`, `app/colaboradores/services/calendario.py:249`) |

O comando `vigiar_fio_morto` está declarado na corrente automática, com `--alarme --alarme-passivo`; execução efetiva dessa configuração no ambiente de produção **[nao verificado]**. (`app/config/crons.py:1654`)

**App/API e cartão de ponto seguem outra cadeia:**

| Superfície | Fonte e comportamento |
|---|---|
| **API do app `/api/espelho/v2/`** | Lê `espelho_do_colab`, não este calendário. Para dia sem batidas, calcula `_futuro = data > hoje`; em HOJE de trabalho, mantém “futuro” se não houver fim previsto ou `agora < fim_turno`. Nesse caminho entrega `previsto`; depois do fim pode entregar `falta`, observadas isenção/cobertura. (`app/api/urls.py:81`, `app/api/views.py:1363`, `app/api/views.py:1369`, `app/api/views.py:1629`, `app/api/views.py:1632`, `app/api/views.py:1635`, `app/api/views.py:1650`, `app/api/views.py:1653`) |
| **API, dias com batidas** | Usa vocabulário próprio: `aberto`, `alerta` ou `ok`; publica `status` no JSON. A apresentação efetiva no cliente móvel externo **[nao verificado]**. (`app/api/views.py:1570`, `app/api/views.py:1591`, `app/api/views.py:1700`) |
| **Cartão/PDF de ponto** | Lê os dias de `espelho_do_colab`. Aplica `max(data_ini, ate_ontem(data_fim, hoje=localtime(agora).date()))`; `ate_ontem` retorna `min(data_fim, hoje-1)`. O `max` exige ressalva para janela integralmente futura: o corte isoladamente não garante excluir todo hoje/futuro nesse caso. (`app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/cartao_pela_celula.py:37`) |
| **Palavra/contador “Em aberto” compartilhado** | Vem de `folha_manda → datas_em_aberto_de`, usando teto `ate_ontem` para furos e retirando o turno em curso; não deriva de `status == 'falta'` deste calendário. (`app/relatorios/cartao_pela_celula.py:395`, `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:418`, `app/ponto/services/dia_decidido.py:311`) |

Outra chave homônima é a de `clipar_grade_janela`, que neutraliza células futuras da grade do espelho; outra ainda pertence ao calendário de fase, cujo template lê `c.futuro` e `c.hoje`. Não são o fato interno passado a `_classificar`. (`app/escala/utils.py:934`, `app/escala/utils.py:942`, `app/ponto/services/espelho.py:688`, `app/templates/colaboradores/partials/_calendario_fase.html:12`, `app/templates/colaboradores/partials/_calendario_fase.html:14`)

**3. Outros sítios que usam DATA para decidir falta/furo ou se ainda cabe cobrança**

| Sítio | Comparação e efeito |
|---|---|
| **API do app** | `data > hoje`; HOJE recebe também teste por instante contra `fim_turno`, conforme descrito acima. (`app/api/views.py:1629`, `app/api/views.py:1632`) |
| **Espelho `_status_dia`** | Após as precedências de ausência, inconsistência, batidas, isenção e folga, previsto com `data < hoje` recebe `sem_registro`; HOJE sem esses fatos cai em `vazio`. (`app/ponto/services/espelho.py:152`, `app/ponto/services/espelho.py:169`) |
| **Grade `clipar_grade_janela`** | `dia > hoje` marca futuro, troca células por `None` e limpa órfãs, retirando indicação de marco faltante. HOJE passa por esse corte. (`app/escala/utils.py:942`) |
| **Cartório `julgar_celula`** | Recusa `data > hoje`. Para HOJE de trabalho, sem `teto_de_hoje`, espera fim do turno; folga pula essa espera. Essa guarda usa `localdate()`/`now()` próprios, inclusive quando há argumento `agora`. (`app/ponto/services/cartorio.py:1536`, `app/ponto/services/cartorio.py:1545`, `app/ponto/services/cartorio.py:1553`, `app/ponto/services/cartorio.py:1557`) |
| **Cartório `julgar_colab`** | Distingue hoje/ontem para marcos vencidos; passado cujo turno não encerrou é marcado em curso por `data < hoje and not _turno_encerrado(data)`. (`app/ponto/services/cartorio.py:1048`, `app/ponto/services/cartorio.py:1053`, `app/ponto/services/cartorio.py:1082`) |
| **Fábrica da célula** | `cel.data > hoje` não gera motivos. Sem marcos, passado recebe `None` para seguir o caminho da lei das batidas; hoje/futuro recebe conjunto vazio e espera. (`app/chamados/services/fabrica_da_celula.py:28`, `app/chamados/services/fabrica_da_celula.py:30`, `app/chamados/services/fabrica_da_celula.py:80`, `app/chamados/services/fabrica_da_celula.py:85`) |
| **Cobrança manual `cobrar_dia`** | Recusa `data >= localdate()`, afirmando que o turno ainda não terminou. (`app/chamados/services/cobrar_dia.py:16`, `app/chamados/services/cobrar_dia.py:26`) |
| **Retenção de pergunta no app** | Chamado de furo sem pergunta com `dia >= hoje` não recebe causa de retenção: ainda não é hora. (`app/chamados/services/pergunta_no_app.py:56`, `app/chamados/services/pergunta_no_app.py:64`) |
| **Reconciliador de órfãs** | `data_evento >= localtime(agora).date()` retorna `TETO` antes de consultar `dia_encerrado_motivo`. (`app/chamados/reconciliador.py:1036`, `app/chamados/reconciliador.py:1038`) |
| **Suprajuiz** | Furos usam o resultado de encerramento; sobrevive comparação civil na acusação de cadastro de trabalho sem vínculo/previsto: `not (hoje and data and data >= hoje)`. (`app/ponto/supra_juiz.py:253`, `app/ponto/supra_juiz.py:254`, `app/ponto/supra_juiz.py:405`) |
| **Apuração `furos_diarios`** | Corta em `min(fim, hoje-1)`; HOJE inteiro fica fora da apuração/contagem. A borda reaparece na manutenção e no universo. (`app/escala/services/furos_diarios.py:24`, `app/escala/services/furos_diarios.py:26`, `app/escala/services/furos_diarios.py:99`, `app/escala/services/furos_diarios.py:282`) |
| **Cartão e “Em aberto”** | `ate_ontem` corta em hoje−1; `datas_em_aberto_de` limita furos apurados a `d <= ate`, enquanto turnos abertos e saídas órfãs entram sem esse teto civil. (`app/relatorios/cartao_pela_celula.py:37`, `app/ponto/services/dia_decidido.py:311`, `app/ponto/services/dia_decidido.py:316`) |
| **Validação de cartão/PDF** | Recusa fim posterior a ontem; tem mensagens distintas para HOJE e futuro. O default de hoje usa `date.today()`. (`app/relatorios/services.py:35`, `app/relatorios/services.py:38`, `app/relatorios/services.py:41`, `app/relatorios/services.py:45`) |
| **Prévia da folha** | Exclui `d >= hoje` antes de propor preenchimento dos marcos apagados. (`app/folha/previa.py:77`, `app/folha/previa.py:79`, `app/folha/previa.py:84`) |
| **Retenção no export da folha** | Célula ausente/sem veredito gera retenção `sem_celula` apenas com `d < hoje`, ressalvado o dia da demissão; célula já acusando segue outro ramo. (`app/folha/export.py:1211`, `app/folha/export.py:1215`, `app/folha/export.py:1237`) |
| **Regularização de retorno** | Interrompe a criação de faltas intermediárias em `dia >= hoje`. (`app/ponto/services/regularizacao.py:108`, `app/ponto/services/regularizacao.py:113`, `app/ponto/services/regularizacao.py:118`) |
| **Abandono provisório** | Cria faltas até ontem; para em `dia >= hoje`, pois ele ainda pode aparecer hoje. (`app/ponto/management/commands/marcar_abandono_provisorio.py:69`, `app/ponto/management/commands/marcar_abandono_provisorio.py:72`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76`) |
| **Declaração manual/chat** | Recusa falta apenas com `data > hoje`; essa guarda permite HOJE. (`app/ponto/services/declarar_dia.py:94`, `app/ponto/services/declarar_dia.py:100`) |

Também há cortes civis nos comandos e contadores:

| Sítio | Corte |
|---|---|
| `reconciliar_grade` | Pula `dt >= hoje`; depois verifica vencimento por instante dos marcos restantes. (`app/ponto/management/commands/reconciliar_grade.py:134`, `app/ponto/management/commands/reconciliar_grade.py:169`) |
| `processar_cartorio` | Lê `localdate()` e `now()` separadamente e varre somente `data__lt=hoje`. (`app/ponto/management/commands/processar_cartorio.py:38`, `app/ponto/management/commands/processar_cartorio.py:39`, `app/ponto/management/commands/processar_cartorio.py:50`) |
| `furos_sem_cobranca` | Considera furos até ontem. (`app/ponto/services/furos_sem_cobranca.py:53`, `app/ponto/services/furos_sem_cobranca.py:61`) |
| `vigia_entrada` | Considera células de trabalho anteriores a hoje. (`app/ponto/services/vigia_entrada.py:73`, `app/ponto/services/vigia_entrada.py:79`) |
| `folha.services.sem_furo` | Consulta até `min(fim, ontem)`. (`app/folha/services/sem_furo.py:39`, `app/folha/services/sem_furo.py:51`) |
| `emitir_turno_aberto_retroativo` | Se o fim pedido for `>= hoje`, recua para ontem. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:72`) |
| `retratar_chamados_lei_cala` | Usa `date.today()` e pula `D >= hoje` antes de classificar a falta. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:32`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:43`, `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`) |
| `mapa_divergencia` | Separa `data > hoje` e limita a análise de divergências ao restante; HOJE entra no grupo passado. (`app/ponto/management/commands/mapa_divergencia.py:162`, `app/ponto/management/commands/mapa_divergencia.py:164`) |

Para “ainda pode bater”, há ainda decisões por **instante**: `faltas_de_hoje_sem_cobranca` só conta após vencer a entrada prevista mais sua tolerância; `vigia_de_hora` julga marcos de hoje/ontem e consulta encerramento com `agora - 2*teto`. O antigo corte civil de `flip_auto` foi substituído pela chamada a `dia_encerrado`. (`app/ponto/services/faltas_de_hoje.py:28`, `app/ponto/services/faltas_de_hoje.py:48`, `app/ponto/services/faltas_de_hoje.py:68`, `app/ponto/services/vigia_de_hora.py:46`, `app/ponto/services/vigia_de_hora.py:57`, `app/ponto/services/vigia_de_hora.py:130`, `app/ponto/services/flip_auto.py:285`)

**4. `dia_encerrado` e `dia_encerrado_motivo`**

Ambas recebem `(colaborador, data, agora=None, celulas=None)`. `agora` deve ser datetime aware e, omitido, nasce de `timezone.now()`. `dia_encerrado` devolve exatamente `dia_encerrado_motivo(...) is None`. O mapa de células é repassado a `turnos_do_colab`, cujo contrato é `{data: CelulaDia}`. (`app/ponto/turnos.py:2014`, `app/ponto/turnos.py:2052`, `app/ponto/turnos.py:2058`, `app/ponto/turnos.py:2073`, `app/ponto/turnos.py:2077`, `app/ponto/turnos.py:1852`)

A ordem efetiva é:

| Condição | `dia_encerrado_motivo` | `dia_encerrado` |
|---|---|---|
| Há aberta viva com expiração prevista | `turno_aberto_relogio` | `False`. (`app/ponto/turnos.py:2097`, `app/ponto/turnos.py:2099`) |
| Há aberta viva sem expiração prevista | `turno_aberto` | `False`. (`app/ponto/turnos.py:2101`) |
| Sem aberta viva, mas sem fim previsto | `sem_previsto` | `False`. (`app/ponto/turnos.py:2102`, `app/ponto/turnos.py:2103`) |
| Sem aberta viva, `agora < fim` | `relogio` | `False`. (`app/ponto/turnos.py:2105`) |
| Sem aberta viva, `agora >= fim` | `None` | `True`. (`app/ponto/turnos.py:2105`, `app/ponto/turnos.py:2058`) |

A aberta expira pelo **fim do template + 4 horas**, somente quando `agora > expiração`: no instante exato ainda está viva. Já o fim previsto usado no segundo teste lê o vínculo e seu DNA de célula. (`app/ponto/turnos.py:1981`, `app/ponto/turnos.py:1992`, `app/ponto/turnos.py:2007`, `app/ponto/turnos.py:2011`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1099`)

Na virada da meia-noite, os marcos são encadeados: cada horário que não for posterior ao anterior avança um dia. Portanto, turno de D pode continuar vivo em D+1; fechar o primeiro bloco de um turno partido também não basta antes do fim previsto do conjunto. (`app/escala/servico_jornada.py:74`, `app/escala/servico_jornada.py:81`, `app/escala/servico_jornada.py:89`, `app/ponto/turnos.py:2033`)

**Sem célula não implica `False`; folga também não tem retorno especial.** O julgamento não consulta `trabalha`: sem DNA de marcos, `EC.marcos_do_dia` recorre ao template. Se não houver vínculo/tipo/fim, retorna `sem_previsto`, salvo a precedência de uma aberta viva. (`app/ponto/turnos.py:2095`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1097`

[truncado em 20000 de 26230 caracteres]

## 30/10 13:27 · sessao 62633b9a6473 · - · - · origem=Ronald (tmux)

**Entradas e retorno.** `dia_encerrado(colaborador, data, agora=None, celulas=None)` retorna booleano pela expressão `dia_encerrado_motivo(...) is None`; ambos compartilham assinatura e carregamento, com `agora` omitido resolvido por `timezone.now()`. O contrato documentado pede datetime aware; `celulas` segue para `turnos_do_colab`, cujo formato é `{date: CelulaDia}`. (`app/ponto/turnos.py:2014`, `app/ponto/turnos.py:2052`, `app/ponto/turnos.py:2058`, `app/ponto/turnos.py:2073`, `app/ponto/turnos.py:2077`, `app/ponto/turnos.py:1852`.)

**Motivos e precedência.** O julgamento primeiro pergunta por aberta viva: havendo aberta com expiração prevista, retorna `'turno_aberto_relogio'`; aberta que não expira retorna `'turno_aberto'`. Sem aberta viva, ausência de fim previsto retorna `'sem_previsto'`; fim previsto ainda não alcançado retorna `'relogio'`; `agora >= fim` retorna `None`, portanto `dia_encerrado=True`. (`app/ponto/turnos.py:2066`, `app/ponto/turnos.py:2070`, `app/ponto/turnos.py:2095`.)

**Bordas temporais.** A aberta expira somente quando `agora > fim_previsto + 4h`, portanto continua viva no instante exato da expiração; o outro teste usa `agora >= fim`. Expiração da aberta consulta o template; o fim do dia consulta `ec.marcos_do_dia`, isto é, DNA da célula quando disponível. (`app/ponto/turnos.py:1981`, `app/ponto/turnos.py:1991`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2007`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:68`, `app/escala/models.py:1099`.)

**Meia-noite e partido.** `fim_turno` toma o último marco de `marcos_turno`; a sequência avança um dia sempre que o próximo horário não é posterior ao anterior. Assim, noite de D pode continuar aberta em D+1; um primeiro bloco fechado não basta para encerrar um dia partido. Há testes explícitos para noturno aberto às 06h de D+1 e partido ainda aberto às 23h. (`app/escala/servico_jornada.py:74`, `app/escala/servico_jornada.py:81`, `app/escala/servico_jornada.py:89`, `app/ponto/tests/test_juiz_dia_encerrado.py:103`, `app/ponto/tests/test_juiz_dia_encerrado.py:120`.)

**Sem célula, folga ou sem escala.** Não há consulta a `trabalha` ou `eh_dia_trabalho` no julgamento: ele decide por aberta e marcos. Sem célula, `EC.marcos_do_dia` recorre ao template; portanto “sem célula” não significa automaticamente “não encerrado”. O template explicitamente não decide trabalho/folga. Se não houver vínculo/tipo/fim, o motivo será `'sem_previsto'`, salvo aberta viva, que tem precedência. Célula de folga também é julgada pelos marcos efetivamente presentes no DNA/template, sem tratamento específico de folga nesta função. (`app/ponto/turnos.py:2095`, `app/escala/models.py:1097`, `app/escala/models.py:1108`, `app/escala/models.py:326`.)

**Chamadores atuais em código de produção.**

| Leitor | Uso |
|---|---|
| `detectar_ausencias::_hora_de_cobrar` | Só cobra entrada se houver marco vencido e o dia **não** estiver encerrado. (`app/ponto/management/commands/detectar_ausencias.py:460`, `app/ponto/management/commands/detectar_ausencias.py:470`.) |
| `flip_auto::avaliar_fila` | Pula a batida e conta `dia_em_curso` quando o juiz retorna falso. (`app/ponto/services/flip_auto.py:285`.) |
| Fechamento, filtro de faltas afirmáveis | Acrescenta a data à lista somente quando encerrada; passa células recortadas pelo pad. (`app/ponto/services/fechamento.py:349`, `app/ponto/services/fechamento.py:353`, `app/ponto/services/fechamento.py:361`.) |
| `vigia_de_hora::_ontem_em_janela` | Mantém ontem na janela enquanto não estava encerrado em `agora - 2*TETO_MARCO_MIN`; contém atalho anterior ao fim + dois tetos. (`app/ponto/services/vigia_de_hora.py:26`, `app/ponto/services/vigia_de_hora.py:50`.) |
| `reconciliador::veredito_orfa` | Único chamador externo direto do motivo encontrado: após teto próprio `data_evento >= localtime(agora).date()`, encerra com motivo `None`, adia com `'relogio'`/`'turno_aberto_relogio'`, e deixa sem encerramento nos demais motivos. (`app/chamados/reconciliador.py:1008`, `app/chamados/reconciliador.py:1036`, `app/chamados/reconciliador.py:1041`.) |

**Versão de período.** Existe `dias_encerrados(colaborador, ini, fim, agora=None, celulas=None) -> {date: bool}`: lê `agora` uma vez, carrega turnos uma vez e percorre o intervalo inclusivo usando o mesmo julgamento. É por colaborador, não um carregador de frota. (`app/ponto/turnos.py:2108`, `app/ponto/turnos.py:2118`, `app/ponto/turnos.py:2124`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:2129`.)

Seus leitores são cartório, comando `supra_juiz`, emissor retroativo e `par_completo_da_janela`; os três primeiros fornecem o resultado como `turno_encerrado` a `classificar_dia`, e o último exclui dias não encerrados do mapa de pares completos. (`app/ponto/services/cartorio.py:850`, `app/ponto/services/cartorio.py:969`, `app/ponto/services/cartorio.py:1117`, `app/ponto/management/commands/supra_juiz.py:108`, `app/ponto/management/commands/supra_juiz.py:215`, `app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/management/commands/emitir_furo_retroativo.py:108`, `app/ponto/turnos.py:2143`.)

**Queries: contagem estática, não medição de execução [nao verificado].**

Defina `T = 3 + C + E + M + U` para `turnos_do_colab`, contando:

- Base 3: batidas, vínculos candidatos e vínculo usado pela ata. (`app/ponto/turnos.py:1858`, `app/ponto/turnos.py:1883`, `app/ponto/turnos.py:1892`, `app/ponto/turnos.py:1911`, `app/ponto/turnos.py:1591`, `app/ponto/turnos.py:1556`.)
- `C=1` sem células fornecidas; zero com alimentação. (`app/ponto/turnos.py:1880`.)
- `E=1` se geradoras faltantes exigirem a segunda busca dos vínculos. (`app/escala/alimentacao.py:123`.)
- `M=1` se algum segmento usar o mapa de DNA, buscado e memoizado para a janela; segmentos cujo tipo tem segundo intervalo completo pulam esse mapa. (`app/ponto/turnos.py:1944`, `app/ponto/turnos.py:1966`, `app/escala/utils.py:100`.)
- `U=1` se o pareador invocar o callback de última batida; ele é memoizado por chamada. (`app/ponto/turnos.py:1903`, `app/ponto/turnos.py:1973`.)

Para uma chamada unitária: `Q = T + V + F`. `V=1` para `escala_vigente` com célula e geradora, ou `2` quando precisa buscar vínculos; `F=1` se alcançar `fim_turno` com tipo, porque `EC.marcos_do_dia` volta a consultar a célula; caso contrário `F=0`. O mapa recebido por `dia_encerrado` **não** é repassado nem a `escala_vigente` nem a `fim_turno`. (`app/ponto/turnos.py:2079`, `app/escala/servico_jornada.py:46`, `app/escala/servico_jornada.py:55`, `app/ponto/turnos.py:2102`, `app/escala/servico_jornada.py:69`, `app/escala/models.py:1097`.)

Exemplos estáticos: com célula geradora, sem alimentação, sem segunda busca/callback, usando mapa DNA e alcançando fim previsto: **7 queries**; com aberta viva: **6**; sem célula, mas com vínculo/tipo e alcançando fim: **8**. Alimentar corretamente o mapa reduz somente `C`, uma query. (Derivação das buscas citadas acima; execução não medida [nao verificado].)

No período, `Q = T + L + 1 + K`: `L=1` se não recebeu células; `1` para todos os vínculos; `K` é o número de dias que chegam a `fim_turno` com tipo. O lote evita recarregar turnos/vínculos por dia, mas ainda pode pagar **uma consulta de célula por dia** no julgamento. Sem alimentação, o caso comum é `7 + K`; há duas cargas de célula porque a linha 2123 cria `cels`, enquanto 2126 passa o argumento original `celulas`, possivelmente `None`, ao carregador de turnos. (`app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2124`, `app/ponto/turnos.py:2126`, `app/ponto/turnos.py:2131`, `app/ponto/turnos.py:2102`, `app/escala/models.py:1097`.)

**Ressalva estrutural de alimentação.** `turnos_do_colab` espera chaves `date` e monta `{(cid, d): cel}`; o lote e os chamadores cartório/vigia usam mapas com chave `(cid, date)`. Isso produz chaves aninhadas quando repassado e pode impedir o reconhecimento da geradora na geometria; efeito em dados reais não medido [nao verificado]. (`app/ponto/turnos.py:1852`, `app/ponto/turnos.py:1928`, `app/ponto/turnos.py:2123`, `app/ponto/turnos.py:2126`, `app/ponto/services/cartorio.py:853`, `app/ponto/services/vigia_de_hora.py:57`.)

## 30/10 13:27 · sessao 168a5d3a8d7b · - · - · origem=Ronald (tmux)

Censo fora de `calendario.py`, somente leitura. As comparações abaixo são código executável; comentários históricos sobre regras já removidas foram separados. (`app/api/views.py:1629`; `app/ponto/supra_juiz.py:254`)

**Leitor do calendário em vigia/cron**

- `chamados.services.vigia_fio.censo`: importa `resolver_dia_oferecido_na_data` e, quando o calendário oferece “Resolver dia”, conta `decisao_do_dia_pendente` e pula a classificação como fio morto. Tem relógio próprio: `hoje = hoje or timezone.localdate()`; conta `data > hoje` como futuro no universo e retira `data >= hoje` do contador alarmado antes de perguntar ao calendário. (`app/chamados/services/vigia_fio.py:181`, `:238`, `:278`, `:289`, `:294`, `:298`)
- O comando desse vigia participa da corrente automática como `ENC('vigiar_fio_morto', ... args='--alarme --alarme-passivo')`. (`app/config/crons.py:1654`)
- Não encontrei outros consumidores executáveis de `status_do_dia` ou `contar_por_status` fora do próprio módulo: busca retornou definições, docstrings e registro em `core/juizes.py`. Exaustividade por análise estática: [nao verificado].

**Outros classificadores/decisores por DATA**

| Sítio | Regra e consequência |
|---|---|
| API do app, preenchimento de dias sem batidas | `_futuro = _d > _hoje`; HOJE em trabalho usa `fim_turno`: ausência de fim ou `agora < fim` mantém futuro. Trabalho passado/hoje após fim gera `falta`, exceto isento ou cobertura; futuro gera `previsto`. (`app/api/views.py:1627`, `:1629`, `:1632`, `:1635`, `:1636`, `:1643`, `:1649`, `:1651`) |
| `ponto.services.espelho._status_dia` | Depois de ausência, inconsistência, batidas, isenção e folga: previsto com `data < hoje` vira `sem_registro`; demais viram `vazio`. (`app/ponto/services/espelho.py:156`, `:169`, `:171`) |
| `escala.utils.clipar_grade_janela` | `_dia > hoje` marca `futuro`, zera órfãs e substitui células por `None`, apagando indicação de marco faltante; HOJE passa. (`app/escala/utils.py:934`, `:942`) |
| `cartorio.julgar_celula` | `hoje` próprio via `localdate()`, futuro retorna `None`; HOJE de trabalho, sem `teto_de_hoje`, espera `fim_turno`, comparando `_tz.now()` diretamente — **essa guarda não usa o argumento `agora`**. Folga (`trabalha is False`) pula a espera. (`app/ponto/services/cartorio.py:1531`, `:1536`, `:1545`, `:1553`, `:1557`, `:1560`) |
| `cartorio.julgar_colab` | Com `teto_de_hoje`, distingue HOJE e ONTEM para julgar só marcos vencidos; passado com turno vivo recebe `_em_curso = _pend is None and data < hoje and not _turno_encerrado(data)`. (`app/ponto/services/cartorio.py:1048`, `:1053`, `:1082`) |
| `chamados.services.fabrica_da_celula.motivos_a_perguntar` | `cel.data > hoje` não pergunta. Com marcos, usa vencimento por instante; sem marcos retorna `None` para passado e conjunto vazio para hoje/futuro. (`app/chamados/services/fabrica_da_celula.py:28`, `:30`, `:68`, `:80`, `:85`) |
| `chamados.services.cobrar_dia.cobrar_dia` | Recusa cobrança manual se `data >= timezone.localdate()`, dizendo que turno ainda não terminou; não consulta encerramento efetivo nessa guarda. (`app/chamados/services/cobrar_dia.py:16`, `:26`) |
| `chamados.services.pergunta_no_app.retencao` | Para chamado vivo de furo sem pergunta, `dia >= hoje` retorna `None`: “ainda não é hora”, em vez de causa de retenção. (`app/chamados/services/pergunta_no_app.py:56`, `:64`) |
| `chamados.reconciliador.veredito_orfa` | Depois de verificar fato e pergunta viva, `data_evento >= localtime(agora).date()` retorna `TETO`; só depois chama `dia_encerrado_motivo`. (`app/chamados/reconciliador.py:1028`, `:1032`, `:1036`, `:1038`) |
| `ponto.supra_juiz` | Ramo geral de furo usa `turno_encerrado`; regra por data sobrevive somente na acusação de cadastro para célula de trabalho sem vínculo e sem previsto: `not (hoje and data and data >= hoje)`. (`app/ponto/supra_juiz.py:253`, `:254`, `:280`, `:405`) |
| `escala.services.furos_diarios.apurar_colab` | Corta período em `min(fim, hoje - 1)` e lê ata/célula para contar marcos previstos e furados; exclui HOJE inteiro. Mesma borda reaparece na manutenção e no universo. (`app/escala/services/furos_diarios.py:24`, `:26`, `:35`, `:44`, `:99`, `:282`) |
| Cartão `ate_ontem` | `hoje or timezone.localdate()`, devolve `min(data_fim, hoje - 1)`; PDF aplica corte usando a data local do mesmo `agora` do ato. (`app/relatorios/cartao_pela_celula.py:37`, `:40`, `:41`; `app/relatorios/pdf_espelho.py:285`, `:287`) |
| `dia_decidido.datas_em_aberto_de` | Derivador puro limita **furo apurado** a `d <= ate`; turnos abertos e saídas órfãs entram sem esse teto. O escritor fornece `ate_ontem(data_fim_pedido)` e retira o turno vivo. (`app/ponto/services/dia_decidido.py:311`, `:314`, `:315`, `:318`; `app/relatorios/cartao_pela_celula.py:395`, `:397`, `:412`, `:418`) |
| Validação de cartão/PDF | `data_fim > ontem` recusa: mensagem específica para HOJE (“ainda está sendo batido”) e outra para futuro; `hoje` default usa `date.today()`. (`app/relatorios/services.py:35`, `:38`, `:41`, `:45`) |
| `folha.previa` | Exclui `d >= hoje` antes de propor preenchimento dos marcos apagados da ata. (`app/folha/previa.py:41`, `:77`, `:79`, `:84`) |
| `folha.export.motivos_retencao_celula` | Célula inexistente ou ainda sem veredito só causa retenção `sem_celula` com `d < hoje` (exceto dia da demissão). Célula já acusando pode reter independentemente desse teste. (`app/folha/export.py:1179`, `:1211`, `:1213`, `:1215`, `:1237`) |
| Regularização de retorno | Cria faltas intermediárias só antes de HOJE; `dia >= hoje` interrompe laço. (`app/ponto/services/regularizacao.py:74`, `:108`, `:113`, `:118`) |
| Abandono provisório | Cria falta de D+1 até ontem, com guarda `dia >= hoje: break`; comentário expressamente diz “não inclui hoje, ele pode aparecer”. (`app/ponto/management/commands/marcar_abandono_provisorio.py:69`, `:72`, `:76`) |
| Declaração manual/chat | Só recusa `tipo == 'falta' and data > hoje`; HOJE é permitido por essa guarda. (`app/ponto/services/declarar_dia.py:94`, `:100`) |

**Vigias, crons e comandos com janela civil própria**

- `reconciliar_grade`: pula `dt >= hoje`, depois verifica também vencimento de cada marco por instante, inclusive noturno. Está agendado às 06:20 sem `--apply`. (`app/ponto/management/commands/reconciliar_grade.py:134`, `:169`; `app/config/crons.py:443`)
- `processar_cartorio`: lê `localdate()` e `now()` separadamente, varre apenas células `data__lt=hoje`, entrega `hoje/agora` ao serviço; agendado às 06:27. Isso é janela de varredura além da regra do serviço. (`app/ponto/management/commands/processar_cartorio.py:38`, `:39`, `:50`, `:62`, `:65`; `app/config/crons.py:597`)
- `ponto.services.furos_sem_cobranca.medir`: só considera `veredito='furo'` até ontem (`data__lte=ontem`). (`app/ponto/services/furos_sem_cobranca.py:53`, `:54`, `:61`)
- `ponto.services.vigia_entrada.por_dia_da_semana`: só células de trabalho `data__lt=hoje`; contador termina ontem. (`app/ponto/services/vigia_entrada.py:73`, `:79`, `:86`)
- `folha.services.sem_furo`: consulta dias com furo até `min(fim, ontem)`. (`app/folha/services/sem_furo.py:39`, `:40`, `:51`)
- `emitir_turno_aberto_retroativo`: se fim pedido `>= hoje`, recua para ontem por considerar HOJE não encerrado. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:72`, `:73`)
- `retratar_chamados_lei_cala`: compara contra `date.today()`, pula `D >= hoje`, só então pergunta `classificar_falta` e pode retratar. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:32`, `:43`, `:45`, `:59`)
- `mapa_divergencia`: separa `data > hoje` como futuro e só examina divergências de chaves/marcos se não futuro; HOJE fica no balde passado. (`app/ponto/management/commands/mapa_divergencia.py:162`, `:164`)

**Regras por INSTANTE que também importam para “ainda pode bater”**

- `faltas_de_hoje_sem_cobranca`: relógio é `localtime(agora or now())`; fixa HOJE e só conta ausência de batida após a entrada prevista vencer por `horas` (padrão constante do módulo), com exclusões de isento, adesão, batida, cobertura e fato vencedor. Não lê o status do calendário. (`app/ponto/services/faltas_de_hoje.py:13`, `:28`, `:29`, `:48`, `:49`, `:55`, `:57`, `:59`, `:61`, `:68`)
- `vigia_de_hora`: consulta células de hoje/ontem, e para ontem exige último marco + teto a partir da meia-noite atual; conserva candidato até dois tetos após encerramento respondido por `dia_encerrado(agora - 2*teto)`. Julga com `teto_de_hoje=True`. (`app/ponto/services/vigia_de_hora.py:23`, `:46`, `:51`, `:52`, `:54`, `:57`, `:94`, `:130`)
- **Não confundir comentário antigo com código atual:** `flip_auto` antes tinha `dia >= hoje`; agora chama `dia_encerrado`. (`app/ponto/services/flip_auto.py:279`, `:285`)
- **Regra adjacente, sem classificar falta/furo:** declaração retrospectiva de cobertura recusa `dia >= hoje` e manda usar designação de cobertura. (`app/ponto/services/cobertura.py:271`, `:272`)

## 30/10 13:27 · sessao c707ea85e5a1 · - · - · origem=Ronald (tmux)

Censo de leitores, somente leitura. A chave `'futuro'` de `calendario.py:785` é fato interno de `_classificar`; não é exportada nos dicionários de `dias` (estes exportam `status`, `status_rotulo`, `eh_trabalho` etc.). **Não confundir com outras chaves `futuro` de outras grades.** — `app/colaboradores/services/calendario.py:779`, `app/colaboradores/services/calendario.py:785`, `app/colaboradores/services/calendario.py:855`, `app/colaboradores/services/calendario.py:889`.

| Leitor | Caminho e decisão/exibição | Regra temporal própria |
|---|---|---|
| Perfil do colaborador na tela de gestão | `detalhe.html` carrega endpoint HTMX; view chama `contexto_calendario` e renderiza partial; partial inclui grade. Status define fundo/borda e ícone/title: `ok` verde/check, `irregular` amarelo/aviso, `folga_trabalhada` laranja, `falta` vermelho/ícone falta, `escala_suspeita` roxo pontilhado/`?`, `aberto` amarelo/cadeado aberto, `previsto` azul/ícone previsto, `ausencia` roxo, `folga` cinza. Ausência/palavra podem tomar precedência sobre ícone. — `app/templates/colaboradores/detalhe.html:207`, `app/templates/colaboradores/detalhe.html:215`, `app/colaboradores/views.py:1945`, `app/colaboradores/views.py:1962`, `app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/templates/colaboradores/partials/_calendario_grade.html:97`, `app/templates/colaboradores/partials/_calendario_grade.html:107` | View põe outro `hoje=localdate()` no contexto quando usuário pode regularizar, mas grade atual só lê `dia.resolver_dia_oferecido` para botões, sem comparar data com hoje. — `app/colaboradores/views.py:1957`, `app/colaboradores/views.py:1960`, `app/templates/colaboradores/partials/_calendario_grade.html:125` |
| Gaveta de diagnóstico / painel | Chama `contexto_calendario` em período custom e inclui a mesma grade em fita, com mesmos status, totais e botões. — `app/colaboradores/services/drawer.py:120`, `app/colaboradores/services/drawer.py:121`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:33`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` | `localdate()` define janela atual + anterior, limitada pelo piso visual; isso escolhe período, sem reclassificar status. — `app/colaboradores/services/drawer.py:111`, `app/colaboradores/services/drawer.py:113`, `app/colaboradores/services/drawer.py:118` |
| Auditoria de espelhos / mapa de furos HTML | `dataset_furos` chama calendário por colaborador e template inclui a mesma grade. **Seleção de cards, `nfuro`, ordenação e lista de marcos faltantes não usam status calendário:** vêm de `grade_da_celula` → `estado_do_dia`. — `app/relatorios/furos.py:141`, `app/relatorios/furos.py:160`, `app/relatorios/furos.py:164`, `app/relatorios/furos.py:165`, `app/relatorios/furos.py:184`, `app/relatorios/furos.py:190`, `app/templates/relatorios/furos.html:42`, `app/templates/relatorios/furos.html:43` | View lê `localdate`; período padrão termina hoje, mas período explícito não recebe teto em hoje nessa view. — `app/relatorios/views.py:1518`, `app/relatorios/views.py:1531`, `app/relatorios/views.py:1537`, `app/relatorios/views.py:1541` |
| PDF do mapa de furos | View extrai **status do calendário** para `card['semanas']`; gerador pinta dia conforme esse estado. Mapas têm `ok`, `irregular`, `folga_trabalhada`, `falta`, `aberto`, `previsto`, `folga`, `sem_dados`, `sem_marcos`; outros caem no fundo branco/texto cinza. `nfuro` continua sendo outra contagem (acima). — `app/relatorios/views.py:1548`, `app/relatorios/views.py:1550`, `app/relatorios/views.py:1551`, `app/relatorios/views.py:1562`, `app/relatorios/furos_pdf.py:11`, `app/relatorios/furos_pdf.py:21`, `app/relatorios/furos_pdf.py:47`, `app/relatorios/furos_pdf.py:56`, `app/relatorios/furos_pdf.py:66` | Herda período da view; gerador não compara data com relógio. — `app/relatorios/views.py:1562`, `app/relatorios/furos_pdf.py:25`, `app/relatorios/furos_pdf.py:47` |
| Botão Resolver dia e Pauta DP | Calendário usa status para derivar `resolver_dia_oferecido`: `pode_ultimo_recurso && eh_trabalho && data < hoje && status in STATUS_QUE_PEDEM_DECISAO`. Template acrescenta permissão `pode_veredito`, oferecendo checkbox, Resolver dia e botão DP. — `app/colaboradores/services/calendario.py:191`, `app/colaboradores/services/calendario.py:196`, `app/colaboradores/services/calendario.py:197`, `app/colaboradores/services/calendario.py:904`, `app/templates/colaboradores/partials/_calendario_grade.html:125`, `app/templates/colaboradores/partials/_calendario_grade.html:126` | Exige passado estrito `data < hoje` no serviço. — `app/colaboradores/services/calendario.py:196` |
| Vigia do fio morto | Censo chama `resolver_dia_oferecido_na_data`, que monta calendário de um dia e lê carimbo; true entra em `decisao_do_dia_pendente` e deixa de seguir para fio morto. — `app/chamados/services/vigia_fio.py:238`, `app/chamados/services/vigia_fio.py:294`, `app/chamados/services/vigia_fio.py:298`, `app/colaboradores/services/calendario.py:205`, `app/colaboradores/services/calendario.py:208` | Antes dessa chamada, dias `>= hoje` sem fio vivo vão ao contador `fio_morto_de_hoje_pra_frente`; `> hoje` também incrementa tripwire `dias_futuros_no_universo`. Command captura `agora=now`, deriva `hoje=localdate(agora)` e passa ao censo; calendário da porta curta captura seu próprio hoje (não recebe o do vigia). — `app/chamados/services/vigia_fio.py:278`, `app/chamados/services/vigia_fio.py:289`, `app/chamados/services/vigia_fio.py:292`, `app/chamados/management/commands/vigiar_fio_morto.py:158`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/colaboradores/services/calendario.py:205` |
| Contadores do calendário | `resumo` conta literalmente status para `ok`, `faltas`, `escala_suspeita`, `folgas_trabalhadas`, `irregulares`, `folgas`, `ausencias`, `abertos`, `previstos`; HE é selo separado. Template mostra `faltas` como “Sem registro”, além de Ok, HE, Trab. folga, Irreg., escala?, Folgas e Abertos; não há bloco exibindo `previstos` ou `ausencias` nesse rodapé. — `app/colaboradores/services/calendario.py:908`, `app/colaboradores/services/calendario.py:925`, `app/templates/colaboradores/partials/_calendario_grade.html:141`, `app/templates/colaboradores/partials/_calendario_grade.html:145`, `app/templates/colaboradores/partials/_calendario_grade.html:173` | Sem relógio próprio no rodapé. — `app/templates/colaboradores/partials/_calendario_grade.html:137` |
| Portas `status_do_dia` e `contar_por_status` | Primeira monta um dia e devolve status ou `sem_dados`; segunda monta período e `Counter(status)`, invertendo datas se necessário. — `app/colaboradores/services/calendario.py:211`, `app/colaboradores/services/calendario.py:217`, `app/colaboradores/services/calendario.py:220`, `app/colaboradores/services/calendario.py:223`, `app/colaboradores/services/calendario.py:226`, `app/colaboradores/services/calendario.py:231` | Nenhum chamador de produção localizado por busca de referências; ausência absoluta/dinâmica [nao verificado]. |

**App/API e cartão PDF não são leitores desse classificador.** A API `api_espelho_v2` usa `espelho_do_colab`, `grade_da_celula` e classificação própria; cartão PDF também lê `espelho_do_colab`. — `app/api/views.py:1363`, `app/api/views.py:1369`, `app/api/views.py:1500`, `app/api/views.py:1570`, `app/api/views.py:1627`, `app/relatorios/pdf_espelho.py:592`, `app/relatorios/pdf_espelho.py:598`.

- API do app: captura `agora=localtime(now())`; monta mês civil; para dia com batidas escolhe `aberto` se turno aberto, senão `alerta` se inconsistente, senão `ok`. Para dia sem batidas usa `_futuro = data > hoje`; **hoje de trabalho é considerado futuro até `fim_turno`** (`fim is None or agora < fim`), depois pode virar falta. Trabalho futuro recebe `previsto`; ausência/isento/vínculo/folga modulam a resposta. Payload inclui `status` e `falta`; vocabulário é diferente do calendário. — `app/api/views.py:1350`, `app/api/views.py:1355`, `app/api/views.py:1356`, `app/api/views.py:1570`, `app/api/views.py:1597`, `app/api/views.py:1627`, `app/api/views.py:1629`, `app/api/views.py:1632`, `app/api/views.py:1635`, `app/api/views.py:1643`, `app/api/views.py:1656`, `app/api/views.py:1673`, `app/api/views.py:1674`.
- API retorna `dias` em JSON; rota `/api/espelho/v2/`. A exibição efetiva no cliente móvel externo não está verificada neste snapshot. — `app/api/views.py:1700`, `app/api/views.py:1729`, `app/api/urls.py:81`; cliente [nao verificado].
- Cartão PDF: gera `agora=now()` uma vez por ato, corta fim por `max(data_ini, ate_ontem(fim, hoje=localtime(agora).date()))`, lê dias do espelho e recorta janela. `ate_ontem` devolve `min(fim, hoje-1)`. Ressalva real: o `max(data_ini, ...)` impede afirmar genericamente “nenhum hoje/futuro pode entrar” para janela integralmente futura. — `app/relatorios/pdf_espelho.py:287`, `app/relatorios/pdf_espelho.py:593`, `app/relatorios/pdf_espelho.py:594`, `app/relatorios/pdf_espelho.py:598`, `app/relatorios/pdf_espelho.py:1041`, `app/relatorios/pdf_espelho.py:1088`, `app/relatorios/cartao_pela_celula.py:37`, `app/relatorios/cartao_pela_celula.py:41`.
- Palavra “Em aberto” compartilhada por espelho/cartão/app vem de outra derivação: `folha_manda` usa seu próprio `ate_ontem(fim)` e passa ao `datas_em_aberto_de`; não deriva de `status=='falta'` do calendário. — `app/relatorios/cartao_pela_celula.py:412`, `app/relatorios/cartao_pela_celula.py:418`, `app/relatorios/cartao_pela_celula.py:421`.
- Outra chave `futuro`: `escala/utils.py::clipar_grade_janela` marca dias `> hoje` e neutraliza células/órfãs; `ponto/services/espelho.py:688` ignora grade marcada. Não é a chave interna do `_classificar`. — `app/escala/utils.py:934`, `app/escala/utils.py:942`, `app/escala/utils.py:943`, `app/escala/utils.py:945`, `app/ponto/services/espelho.py:688`.
- Calendário de fase lê `c.futuro` para cinzar números e `c.hoje` para borda; é outro componente/contrato (cal.semanas), não os `dias/status` de `contexto_calendario`. — `app/templates/colaboradores/partials/_calendario_fase.html:5`, `app/templates/colaboradores/partials/_calendario_fase.html:12`, `app/templates/colaboradores/partials/_calendario_fase.html:14`.

O command do vigia imprime linhas do censo, carimba todos os contadores e só alarma de acordo com constantes; `decisao_do_dia_pendente` é informativo, enquanto `dias_futuros_no_universo` alarma sempre e fio morto só com flag de passivo. Agendamento real em crontab não foi verificado neste snapshot. — `app/chamados/management/commands/vigiar_fio_morto.py:80`, `app/chamados/management/commands/vigiar_fio_morto.py:81`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/management/commands/vigiar_fio_morto.py:165`, `app/chamados/management/commands/vigiar_fio_morto.py:205`; crontab [nao verificado].

## 30/10 13:21 · sessao abaa92c92829 · COL309-SETEMBRO · sonda · origem=Code

Não houve nova execução: o lançador escreve arquivos ([bin/sonda_leitura.sh:132](bin/sonda_leitura.sh:132), [bin/sonda_leitura.sh:217](bin/sonda_leitura.sh:217)). Abaixo, a saída fornecida integralmente; execução e dados de produção [nao verificado].

```text
sonda_leitura: sonda_classe.py [1401c8b1fb95d59a] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

TRILHA_LINHAS_DESDE_28_09=610 COM_BARRADOS=29 DIAS_BARRADOS=563 VINCULOS=25
BARR log=595390 ec=1327 dias=1 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-20..2026-10-20 apesar=False
BARR log=595957 ec=1328 dias=31 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False
BARR log=596310 ec=1330 dias=4 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-17..2026-10-20 apesar=False
BARR log=597279 ec=1331 dias=19 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-02..2026-10-20 apesar=False
BARR log=597355 ec=1333 dias=30 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-22..2026-10-20 apesar=False
BARR log=597406 ec=1334 dias=30 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-22..2026-10-20 apesar=False
BARR log=597429 ec=1335 dias=8 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-13..2026-10-20 apesar=False
BARR log=597451 ec=1336 dias=14 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-07..2026-10-20 apesar=False
BARR log=597484 ec=1337 dias=10 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-11..2026-10-20 apesar=False
BARR log=597680 ec=1333 dias=30 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-22..2026-10-20 apesar=False
BARR log=597757 ec=1339 dias=22 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-30..2026-10-20 apesar=False
BARR log=601349 ec=1255 dias=10 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-11..2026-10-20 apesar=False
BARR log=602270 ec=1340 dias=5 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-16..2026-10-20 apesar=False
BARR log=602379 ec=1341 dias=20 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-01..2026-10-20 apesar=False
BARR log=603110 ec=1220 dias=19 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-02..2026-10-20 apesar=False
BARR log=613403 ec=1328 dias=31 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False
BARR log=640685 ec=1365 dias=1 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-09-20..2026-10-20 apesar=False
BARR log=653981 ec=1380 dias=31 lavra=exportacao (TXT do Dominio), competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False
BARR log=660581 ec=1397 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=660739 ec=1263 dias=1 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-20..2026-10-20 apesar=False [nao verificado]
BARR log=662019 ec=1405 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=668800 ec=1407 dias=4 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-17..2026-10-20 apesar=False [nao verificado]
BARR log=668822 ec=1408 dias=5 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-16..2026-10-20 apesar=False [nao verificado]
BARR log=668849 ec=1409 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=692598 ec=1416 dias=20 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-09-01..2026-10-20 apesar=False [nao verificado]
BARR log=698178 ec=1420 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=699261 ec=1420 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=724164 ec=1437 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
BARR log=724205 ec=1437 dias=31 lavra=exportacao (TXT do Dominio) e holerite PUBLICADO deste colaborador, competencia 09/2026 janela=2026-08-21..2026-10-20 apesar=False [nao verificado]
FORA_DA_VIGENCIA|emp=1|comp=2026-10|aberta celulas=21 colabs=1
FORA_DA_VIGENCIA|emp=2|comp=2026-08|EXPORTADA celulas=643 colabs=52
FORA_DA_VIGENCIA|emp=2|comp=2026-09|EXPORTADA celulas=1036 colabs=53
FORA_DA_VIGENCIA|emp=2|comp=2026-10|aberta celulas=1272 colabs=56
FORA_DA_VIGENCIA|emp=2|comp=2026-11|aberta celulas=894 colabs=31
FORA_DA_VIGENCIA|emp=2|comp=2026-12|aberta celulas=110 colabs=6
FORA_DA_VIGENCIA|emp=2|comp=2027-01|aberta celulas=33 colabs=3
FORA_DA_VIGENCIA|emp=3|comp=2026-08|aberta celulas=161 colabs=11
FORA_DA_VIGENCIA|emp=3|comp=2026-09|EXPORTADA celulas=216 colabs=12
FORA_DA_VIGENCIA|emp=3|comp=2026-10|aberta celulas=206 colabs=10
FORA_DA_VIGENCIA|emp=3|comp=2026-11|aberta celulas=475 colabs=18
FORA_DA_VIGENCIA|emp=3|comp=2026-12|aberta celulas=110 colabs=4
FORA_DA_VIGENCIA|emp=3|comp=2027-01|aberta celulas=44 colabs=4
FORA_DA_VIGENCIA|emp=4|comp=2026-08|aberta celulas=11 colabs=2
FORA_DA_VIGENCIA_TOTAL=5232 COLABS=173
COL309_FORA=27
ERROS=0
FIM-DA-SONDA
```

Trilha: 563 dias em 29 registros, abrangendo 25 vínculos, conforme dado fornecido [nao verificado].  
Fora da vigência da geradora: 5.232 células = 1.895 em competências exportadas + 3.337 em abertas, somadas do dado fornecido [nao verificado].  
COL309_FORA=27, conforme dado fornecido [nao verificado].  
ERROS=0, conforme dado fornecido [nao verificado].

## 30/10 12:57 · sessao 8ab81fa72246 · O261-VETO-A2 · verificador · origem=Code

1. **Não há garantia absoluta: resta um escape concorrente por `analisar()`.** O detector carrega chamados vivos e os entrega à fábrica; esta pode chamar `ch.analisar()` usando a instância antiga. Se outro escritor encerrar o chamado nesse intervalo, `analisar()` decide pelo estado em memória e grava `em_analise` sem consultar veto nem reler o banco. É um caminho automático que permanece fora de `reabrir()`; ocorrência em produção **[não verificado]**. (`app/ponto/management/commands/detectar_ausencias.py:790–796`; `app/chamados/services/disputa_emissao.py:971–981`; `app/chamados/models.py:566–572`, `:192`, `:231`)

   O escape obrigatório do callback está coberto: o receiver usa usuário de sistema, chama o resolver, e este executa `_pai.reabrir()` com o padrão `humano=False`; o veto retorna antes da escrita de estado e da ressurreição de perguntas. (`app/chamados/signals.py:89–92`; `app/chamados/services/disputa_emissao.py:1601–1608`; `app/chamados/models.py:1090–1093`, `:1143–1146`, `:1185–1194`)

   No percurso sequencial com estado atual, os demais eventos automáticos do motor não fazem encerrado → vivo; essa transição pertence a `REABRIR`. (`app/chamados/catalogo/motor.py:110–117`)

2. **sem achado nas três cadeias humanas atuais.** Desarquivar exige login, POST, permissão e acesso à área; reabrir disputa exige autorização sobre o chamado, staff/admin_ti, motivo e justificativa; reabrir competência exige login, POST, `reabrir_periodo` e motivo, repassando `request.user` até o serviço. (`app/chamados/views_arquivar.py:51–68`; `app/chamados/services/fio.py:552–577`; `app/chamados/services/acoes_disputa.py:242–266`; `app/ponto/views.py:1520–1544`; `app/ponto/services/fechamento.py:1415–1416`; `app/chamados/services/competencia_trancada.py:258`)

   Há uma fragilidade local, sem chamador automático demonstrado: o serviço de competência aceita `autor=None`, substitui por usuário de sistema e mesmo assim passa `humano=True`; portanto a classificação humana depende da cadeia acima. Uso automático externo **[não verificado]**. (`app/chamados/services/competencia_trancada.py:228`, `:239`, `:258`)

3. **sem achado nos leitores.** `reabrir()` trata `veto_do_dia` antes de qualquer uso do segundo elemento como chamado. `reabrir_orfas_enterradas` desempacota `_irmao`, mas não o utiliza: qualquer motivo provoca recusa e `continue`. (`app/chamados/models.py:1143–1146`, `:1164–1178`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:105–111`)

4. **Há contraexemplos na deduplicação, inferidos do código; ocorrência em produção [não verificado].**

   **Duplicação concorrente:** duas chamadas diretas podem consultar antes de qualquer uma inserir e ambas registrar a mesma recusa. O helper não serializa consulta/inserção, e o modelo não declara unicidade dessa chave. (`app/chamados/models.py:925–937`; `app/core/observ.py:133–138`; `app/core/models.py:309–316`)

   **Linha extra sem passagem daquele dia:** no nascimento, basta nascer chamado de outro dia do mesmo colaborador/módulo; faltam filtros de dia/célula em `_passou`. Na reabertura, basta renovar o SLA mantendo o chamado encerrado: isso altera `atualizado_em` e habilita outra linha. (`app/chamados/models.py:931–934`, `:378–385`)

   **Ciclo calado por concorrência:** uma recusa consulta a trilha e fica suspensa; ocorre nascimento permitido; a recusa antiga grava sua linha depois. Quando o veto volta, o nascimento fica anterior à última linha e não provoca novo registro. A comparação usa o horário da inserção tardia, sem serialização com o nascimento. (`app/chamados/models.py:925–937`; `app/core/models.py:307`)

   **Custo do helper:** um `SELECT`; existindo linha anterior, mais um `EXISTS`; havendo registro, mais um `INSERT`, além do controle transacional de `evento()`. Os índices declarados não cobrem a chave JSON com a ordenação utilizada; plano e latência reais **[não verificado]**. (`app/chamados/models.py:925–937`; `app/core/observ.py:133–138`; `app/core/models.py:313–316`)

5. **sem achado quanto a novo chamado, nota ou pergunta proibidos.** O novo ramo de veto escreve auditoria e retorna `False` antes dos efeitos de reabertura. O juiz consultado retorna apenas o motivo; a pergunta e as notas administrativas aparecem como contexto preexistente no diff fornecido. (`app/chamados/models.py:1143–1146`, `:937–941`, `:1185–1194`; `app/ponto/services/furos_vetados.py:27–44`; `app/chamados/services/acoes_disputa.py:259–270`; `app/chamados/services/competencia_trancada.py:258–260`)

## 30/10 12:55 · sessao 9ed2fa415911 · - · - · origem=Ronald (tmux)

3. **Sem achado.** `reabrir()` trata `veto_do_dia` antes dos ramos que usam `_irmao` como chamado; portanto a tupla do veto não chega a `_irmao.pk` (`app/chamados/models.py:1143`, `app/chamados/models.py:1144`, `app/chamados/models.py:1164`). `reabrir_orfas_enterradas` desempacota o segundo elemento mas não o usa: qualquer `motivo` provoca `PAI_RECUSA` e `continue` (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:107`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:111`).

4. **Há contraexemplos de duplicação e de recusa calada, inferidos do código; ocorrência em produção [não verificado].**

- **Duplicação concorrente:** duas chamadas diretas a `criar()` ou `reabrir()` podem ler a mesma última linha antes de qualquer delas inserir; o helper não serializa consulta e inserção, e `LogAuditoria` não possui unicidade dessa chave (`app/chamados/models.py:925`, `app/chamados/models.py:937`, `app/core/observ.py:133`, `app/core/models.py:313`). O bloqueio do colaborador existe em `abrir()`, mas não cobre esses dois caminhos diretos (`app/chamados/models.py:666`, `app/chamados/models.py:669`, `app/chamados/models.py:818`, `app/chamados/models.py:1115`).
- **Duplicação sem passagem pela porta:** após uma recusa de reabertura, basta renovar o prazo do chamado, mantendo-o encerrado; `definir_prazo_sla()` muda `atualizado_em`, e a próxima recusa considera isso `_passou=True` (`app/chamados/models.py:378`, `app/chamados/models.py:385`, `app/chamados/models.py:931`). Existe chamada a essa renovação em `recobrar_questionario()` sem guarda do estado do chamado naquele ponto (`app/chamados/services/disputa_emissao.py:1448`, `app/chamados/services/disputa_emissao.py:1450`).
- **Nascimento de outro dia:** a prova de passagem filtra somente colaborador, módulo e criação posterior; um chamado de outro dia/célula torna a próxima recusa da chave original uma linha nova, sem provar mudança daquele dia vetado (`app/chamados/models.py:925`, `app/chamados/models.py:933`).
- **Recusa calada por interleaving:** uma recusa pode consultar a trilha, ficar suspensa, ocorrer nascimento permitido e só depois gravar sua linha; a próxima recusa compara o nascimento com o horário dessa linha tardia e o ignora por não ser posterior (`app/chamados/models.py:925`, `app/chamados/models.py:934`, `app/chamados/models.py:937`, `app/core/models.py:307`). É contraexemplo possível; reprodução/ocorrência [não verificado].
- **Custo do helper:** uma consulta da última linha; quando encontrada, mais um `EXISTS`; quando emite, mais um `INSERT`, com controle transacional de `evento()` e possível acesso ao usuário lazy (`app/chamados/models.py:925`, `app/chamados/models.py:929`, `app/chamados/models.py:937`, `app/core/observ.py:106`, `app/core/observ.py:133`). Os índices declarados de auditoria não cobrem a chave JSON nem sua ordenação; plano e latência reais [não verificado] (`app/core/models.py:313`).

## 30/10 12:55 · sessao 1f14b6ff2d74 · - · - · origem=Ronald (tmux)

Q1: o escape nomeado do callback está barrado: o receiver automático chama o resolver com usuário sistema (`app/chamados/signals.py:81-92`); o resolver chama `_pai.reabrir()` sem `humano=True` (`app/chamados/services/disputa_emissao.py:1601-1608`); o veto é primeira guarda e retorna `False` antes da escrita/ressuscitação (`app/chamados/models.py:1090-1093,1143-1146,1185-1194`).

Há ressalva à afirmação **“toda reabertura automática”**: `analisar()` consulta apenas o estado da instância e escreve `em_analise`, sem reler banco nem perguntar veto (`app/chamados/models.py:566-572`; `app/chamados/catalogo/motor.py:112`). Contraexemplo por concorrência: detector carrega `batida_ausente` vivo e o entrega à fábrica (`app/ponto/management/commands/detectar_ausencias.py:790-796`); outro escritor encerra; fábrica mantém a instância antiga e chama `ch.analisar()` (`app/chamados/services/disputa_emissao.py:971-981`), que grava vivo diretamente (`app/chamados/models.py:192,231`). `batida_ausente` pertence ao universo (`app/chamados/catalogo/furo_modulos.py:15`; `app/ponto/services/furos_vetados.py:74-81`). Essa possibilidade é anterior ao diff; ocorrência em produção [não verificado].

No percurso sequencial, os outros gates automáticos que chamam `_gravar_estado` não reabrem encerrado: `RECONCILIAR`, `ANALISAR`, `CANCELAR`, `SUPERAR` e `ENCERRAR` têm mapas sem transição de encerrado para vivo; só `REABRIR` tem essa transição (`app/chamados/catalogo/motor.py:110-117`). `transicionar()` pode reabrir, mas os usos encontrados com destino vivo vêm de requisição/ação humana (`app/chamados/services/acoes_chamado.py:313-315`; `app/chamados/services/fio.py:702-715`); não encontrei um escritor automático sequencial de estado encerrado para vivo por `update` fora do gate (`app/chamados/models.py:185-231,429-430`).

## 30/10 12:55 · sessao 0d8cfe19a610 · - · - · origem=Ronald (tmux)

2. **sem achado nas cadeias atuais da cópia.**

- Desarquivar: `login_required`, POST, `gerir_chamados` e recusa por área precedem `reabrir(humano=True)`; o usuário segue para a trilha e a nota (`app/chamados/views_arquivar.py:51`, `:54`, `:56`, `:60`, `:68`, `:75`, `:79`).
- Reabrir disputa: as duas entradas exigem login e gestor; o POST delega ao fio, que verifica `pode_agir_chamado`; o serviço exige `is_staff`/grupo `admin_ti`, motivo válido e justificativa, e passa `request.user` à reabertura administrativa (`app/chamados/views.py:658`, `:662`, `:667`, `:674`, `:868`, `:872`, `:951`; `app/chamados/services/fio.py:560`, `:575`; `app/chamados/services/acoes_disputa.py:242`, `:248`, `:250`, `:256`, `:266`).
- Competência: login, POST, permissão `reabrir_periodo` e motivo obrigatório precedem a chamada com `por=request.user`; o serviço de fechamento repassa esse usuário como `autor` ao serviço que usa `humano=True` (`app/ponto/views.py:1520`, `:1523`, `:1525`, `:1537`, `:1544`; `app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:258`).
- Não localizei chamada automática nessas cadeias; uso dinâmico externo é **[não verificado]**. A função de competência isoladamente não exige humano: aceita `autor=None` e adota usuário de sistema; isso constitui possibilidade de uso futuro, não um chamador automático demonstrado (`app/chamados/services/competencia_trancada.py:228`, `:239`).

5. **sem achado no diff fornecido quanto a novo chamado, nota ou pergunta.** O ramo novo de recusa chama somente a trilha e retorna `False`; a trilha escreve `LogAuditoria` (`app/chamados/models.py:1143`, `:1145`, `:1146`, `:937`; `app/core/observ.py:133`). A pergunta e a nota da reabertura administrativa, assim como a nota da competência e a nota do resgate, já aparecem como contexto anterior no diff fornecido; anterioridade histórica além desse anexo é **[não verificado]** (`app/chamados/services/acoes_disputa.py:259`, `:270`; `app/chamados/services/competencia_trancada.py:259`; `app/chamados/views_arquivar.py:79`).

Detalhe adicional de trilha: `via='reabrir'` produz ação `reabertura_recusada`, mas descrição “nascimento recusado (reabrir)”, porque o texto distingue somente `renascer` dos demais valores (`app/chamados/models.py:919`, `:938`).

## 30/10 12:41 · sessao b5030ad3bef7 · O261-VETO-A2 · verificador · origem=Code

1. **Não: o selo não cobre toda reabertura.** Ele reconhece apenas chamadas com a forma `.reabrir(...)`, registra a função e elimina repetições. Acrescentar outra reabertura dentro de função já classificada também passa sem alterar o inventário. (`app/chamados/tests/test_o261_veto_na_porta.py:251`, `:256`, `:275`, `:313`)

   Mutante plausível que escapa:

   ```python
   def automatico(ch):
       ressuscitar = ch.reabrir
       ressuscitar()
   ```

   A chamada usa `ast.Name`, enquanto o detector exige `ast.Attribute`; `getattr(ch, 'reabrir')()` também escapa. (`app/chamados/tests/test_o261_veto_na_porta.py:256`)

   **Existe caminho real omitido:** a resposta livre aceita `novo_status` e chama `transicionar`; o motor permite `resolvido → em_analise`, chegando a `_gravar_estado` sem passar por `.reabrir`. É uma reabertura humana que falta no censo anunciado como completo. (`app/chamados/services/acoes_chamado.py:287`, `:313`, `:315`; `app/chamados/catalogo/motor.py:102`; `app/chamados/models.py:183`, `:260`)

   Existe ainda chamada indireta por alias: `fechamento.reabrir_periodo` chama `_reabrir_fila`, que reabre chamados internamente. O selo classifica o destino, sem verificar novos chamadores dele. (`app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:258`)

2. **As classes precisam destas ressalvas:**

   | Sítio | Conferência |
   |---|---|
   | `_renascer_por_premissa_viva` | **sem achado**: consulta o veto antes de reabrir. (`app/chamados/models.py:780`, `:784`) |
   | `desarquivar_chamado` | **sem achado**: ação da tela com permissão e tratamento da recusa. (`app/chamados/views_arquivar.py:51`, `:70`) |
   | `competencia_trancada.reabrir` | **Humano na cadeia atual**: POST, permissão e motivo → `reabrir_periodo` → serviço. A função isolada aceita `autor=None` e assume sistema; a classe não garante a origem de futuros chamadores. (`app/ponto/views.py:1523`, `:1525`, `:1536`, `:1544`; `app/ponto/services/fechamento.py:1415`; `app/chamados/services/competencia_trancada.py:239`) |
   | `acao_reabrir_disputa_admin` | Humano procede: exige administrador, motivo e justificativa. **Há falha no resultado**, descrita abaixo. (`app/chamados/services/acoes_disputa.py:242`, `:245`, `:250`) |
   | `ponto.views.reabrir_colaborador` | **sem achado**: o objeto reaberto é `FechamentoMensal`. (`app/ponto/views.py:1439`, `:1453`; `app/ponto/services/fechamento.py:1317`) |
   | `sincronizar_adesao` | Automatismo, mas reabre `sem_previsao_escala`, fora dos cinco módulos vetados. (`app/chamados/services/adesao.py:74`, `:92`; `app/ponto/services/furos_vetados.py:79`) |
   | `detectar_ausencias.handle` | Esse sítio reabre `ancora_limbo`, também fora do universo. (`app/ponto/management/commands/detectar_ausencias.py:271`, `:301`; `app/ponto/services/furos_vetados.py:79`) |
   | `resolver_perguntas_por_chamado_resolvido` | **sem achado na classe**: reabertura direta provocada pelo processamento da resposta. (`app/chamados/services/disputa_emissao.py:1598`, `:1606`) |
   | `reconciliar_grade._emitir_furo` | **sem achado na classe**: pode reabrir diretamente o encerrado devolvido por `abrir`. (`app/ponto/management/commands/reconciliar_grade.py:499`, `:522`) |
   | `reabrir_orfas_enterradas.handle` | Lote com dry padrão e `--apply`; a classificação não demonstra execução autônoma por cron. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:48`, `:157`, `:161`; `app/config/crons.py:743`) |
   | `reabrir_chamados_ausencia_aberta.handle` | Também é lote dry/apply e reabre `ausencia_colab`, fora dos cinco módulos. (`app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:19`, `:29`, `:49`; `app/ponto/services/furos_vetados.py:79`) |

   **Falha em `acao_reabrir_disputa_admin`:** abre a disputa, cria pergunta, ignora o retorno de `ch.reabrir()` e anuncia sucesso. Contraexemplo: pai encerrado pelo admin, com dia identificado e sem fato novo; `reabrir()` retorna `False`, deixando disputa aberta com pai encerrado. (`app/chamados/services/acoes_disputa.py:256`, `:259`, `:265`, `:295`; `app/chamados/models.py:1075`, `:1080`, `:1123`, `:1128`) Ocorrência em produção: `[nao verificado]`.

3. **sem achado de quebra de leitor/teste pela chave `via`.** O teste que exige uma linha repete apenas `abrir`, mantendo a mesma chave. Os contadores de recusa por célula absolvida usam outro método, cuja chave permanece intacta. (`app/chamados/tests/test_o261_veto_na_porta.py:430`, `:439`; `app/chamados/models.py:942`; `app/ponto/tests/test_o_outbox_celula.py:348`, `:544`)

   A cardinalidade muda intencionalmente: `abrir` e `criar` recusados para o mesmo objeto/módulo/dia/veto podem produzir duas linhas. A auditoria geral conta essas linhas; não exige unicidade por veto. (`app/chamados/models.py:733`, `:842`, `:923`; `app/core/views.py:705`, `:727`) Resultado da suíte: `[nao verificado]`.

4. **Eu retiraria dois itens da FILA para fechar o contrato declarado de A:**

   - **Item 5 — VETO-TRILHA-CICLO.** O método promete deduplicação “enquanto nada muda”, mas consulta todo o histórico. Contraexemplo: `criar` recusado → correção permite `criar` → mesmo veto retorna → novo `criar` recusado sem nova trilha. A primeira linha histórica suprime o novo ciclo. A chave `via` não cura isso. (`app/chamados/models.py:835`, `:840`, `:910`, `:923`, `:926`)

   - **Item 6 — VETO-VIGIA-NO-PLACAR**, para aceitar A com seu vigia operacional. Contraexemplo: chamado vivo em dia vetado, com célula `cobrado`. O vigia novo encontra esse chamado; o contador ligado ao placar consulta somente células `furo` e pode mostrar zero. Assim, a exceção admitida pelo Strangler pode ficar invisível no placar. (`app/ponto/services/furos_vetados.py:61`, `:95`, `:106`; `bin/placar_code.sh:209`) Existência desse estado em produção: `[nao verificado]`.

   **Item 9 continua pendente de pouso, embora esteja como MEDINDO.** O cron declara `--apply`; o comando percorre células passadas sem filtro de exportação; mudança no veto altera a impressão e pode chegar à lavra. Contraexemplo: célula exportada com impressão antiga, cujo veto agora muda, é rejulgada automaticamente. (`app/config/crons.py:597`; `app/ponto/management/commands/processar_cartorio.py:49`, `:61`; `app/ponto/services/cartorio.py:329`, `:1083`, `:1092`, `:1409`) Quantidade afetada e resultado da medida anunciada: `[nao verificado]`.

## 30/10 12:39 · sessao 7a382b3d18df · - · - · origem=Ronald (tmux)

Classificações:

| Sítio | Veredito |
|---|---|
| `_renascer_por_premissa_viva` | **sem achado**: consulta veto antes de `reabrir`. `app/chamados/models.py:780-784`. |
| `desarquivar_chamado` | **sem achado**: POST autenticado, permissão e área; reabre encerramento por tranca e trata recusa. `app/chamados/views_arquivar.py:51-70`. |
| `competencia_trancada.reabrir` | **Humano na cadeia atual**, pois POST+RBAC+motivo da view chama `fechamento.reabrir_periodo`, que chama esse serviço. `app/ponto/views.py:1520-1544`; `app/ponto/services/fechamento.py:1415-1416`; `app/chamados/services/competencia_trancada.py:258`. **Não é garantia da função**: `autor=None` vira usuário sistema. `app/chamados/services/competencia_trancada.py:228,239`. |
| `acao_reabrir_disputa_admin` | **Humano procede**: exige staff/admin_ti, motivo e justificativa; fio delega à ação. `app/chamados/services/acoes_disputa.py:242-256`; `app/chamados/services/fio.py:575-577`. |
| `ponto.views.reabrir_colaborador` | **sem achado**: `_fech.reabrir(f)` recebe `FechamentoMensal`; a função do serviço só altera esse fechamento. `app/ponto/views.py:1439-1453`; `app/ponto/services/fechamento.py:1317-1323`. |
| `sincronizar_adesao` | Automatismo sem consulta ao veto, mas reabre **`sem_previsao_escala`**, fora do universo dos cinco. `app/chamados/services/adesao.py:74-92`; universo: `app/ponto/services/furos_vetados.py:74-81`, `app/chamados/catalogo/furo_modulos.py:15-16`, `app/chamados/reconciliador.py:336-337`. |
| `resolver_perguntas_por_chamado_resolvido` | **sem achado na classe**: resposta esperando consumo provoca reabertura direta do pai. `app/chamados/services/disputa_emissao.py:1598-1608`. |
| `detectar_ausencias.handle` | Automatismo sem consulta ao veto, mas esse sítio reabre **`ancora_limbo`**, fora dos cinco módulos. `app/ponto/management/commands/detectar_ausencias.py:271-301`; universo acima. |
| `reconciliar_grade._emitir_furo` | **sem achado na classe**: depois de `abrir`, se recebeu encerrado, chama `reabrir` diretamente; apenas `None` encerra o caminho. `app/ponto/management/commands/reconciliar_grade.py:499-522`. |
| `reabrir_orfas_enterradas.handle` | **Rótulo “automático” impreciso**: comando de uma vez, dry por padrão e `--apply`; reabre pai por critério do lote. `app/chamados/management/commands/reabrir_orfas_enterradas.py:1,48-49,157-166`; registrado fora de pipeline em `app/config/crons.py:743-750`. |
| `reabrir_chamados_ausencia_aberta.handle` | **Rótulo “automático” impreciso**: dry por padrão, `--apply` explícito, registrado como ato único; ainda reabre somente **`ausencia_colab`**, fora dos cinco. `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:19-20,28-49`; `app/config/crons.py:1394-1395`; universo acima. |

**Falha adicional concreta em `acao_reabrir_disputa_admin`:** classificação humana não garante reabertura efetiva do pai. A ação abre a disputa e cria pergunta **antes** de chamar `ch.reabrir()`, ignora o retorno e anuncia sucesso. `app/chamados/services/acoes_disputa.py:256-266,293-295`; gravação da disputa: `app/chamados/models.py:1773-1785`.

Contraexemplo estático: pai encerrado por admin, com data resolvida e sem fato novo; `recusa_da_reabertura` retorna `admin_sem_fato_novo` e `reabrir()` retorna `False`. A disputa já ficou aberta e a pergunta nova existe, mas o pai permanece encerrado. `app/chamados/models.py:1075-1080,1123-1128`; `app/chamados/services/acoes_disputa.py:256-266`. Ocorrência em produção: `[nao verificado]`.

## 30/10 12:39 · sessao 0414381eb7ad · - · - · origem=Ronald (tmux)

Q1 — **não acha toda reabertura; acha funções contendo a sintaxe `.reabrir(...)`**, sem resolver alvo, aliases ou fluxo entre funções. Também elimina multiplicidade por função (`any`) e pelo `set`; nova chamada dentro de função já classificada passa sem mudar o resultado. [app/chamados/tests/test_o261_veto_na_porta.py:251](app/chamados/tests/test_o261_veto_na_porta.py:251), [app/chamados/tests/test_o261_veto_na_porta.py:275](app/chamados/tests/test_o261_veto_na_porta.py:275).

**Contraexemplo existente:** `acoes_chamado` lê `novo_status` do POST e chama `chamado.transicionar(novo_status, request.user)` quando gestor; o motor permite `resolvido → em_analise`. Essa reabertura passa por `_aplicar → _gravar_estado`, sem `.reabrir` e sem classificação nesse selo. [app/chamados/services/acoes_chamado.py:287](app/chamados/services/acoes_chamado.py:287), [app/chamados/services/acoes_chamado.py:313](app/chamados/services/acoes_chamado.py:313), [app/chamados/catalogo/motor.py:102](app/chamados/catalogo/motor.py:102), [app/chamados/models.py:183](app/chamados/models.py:183), [app/chamados/models.py:260](app/chamados/models.py:260). Existe caminho equivalente no fio. [app/chamados/services/fio.py:713](app/chamados/services/fio.py:713).

**Mutante plausível que escapa:**

```python
def automatico(ch):
    ressuscitar = ch.reabrir
    ressuscitar()
```

O detector exige `ast.Call.func` do tipo `ast.Attribute` com `attr == 'reabrir'`; a chamada acima tem `ast.Name`. `getattr(ch, 'reabrir')()` e `ch._gravar_estado('em_analise')` também escapam pela mesma condição. [app/chamados/tests/test_o261_veto_na_porta.py:256](app/chamados/tests/test_o261_veto_na_porta.py:256). O núcleo aceita o estado recebido e o persiste; não consulta veto. [app/chamados/models.py:192](app/chamados/models.py:192), [app/chamados/models.py:231](app/chamados/models.py:231).

Uma lambda imediata contendo `ch.reabrir()` também escapa: `_corpo_proprio` exclui `Lambda`, enquanto `_funcoes` só registra `FunctionDef`/`AsyncFunctionDef`. [app/chamados/tests/test_o261_veto_na_porta.py:168](app/chamados/tests/test_o261_veto_na_porta.py:168), [app/chamados/tests/test_o261_veto_na_porta.py:182](app/chamados/tests/test_o261_veto_na_porta.py:182).

Há alias real de outra função que reabre internamente: `competencia_trancada.reabrir as _reabrir_fila` em fechamento; o chamador não aparece no censo, embora o destino apareça. [app/ponto/services/fechamento.py:1415](app/ponto/services/fechamento.py:1415), [app/chamados/services/competencia_trancada.py:258](app/chamados/services/competencia_trancada.py:258).

Executei apenas o AST extraído em memória com `python3 -B`: direto detectado; alias, `getattr`, lambda, núcleo e `transicionar` retornaram lista vazia. Resultado de execução sem arquivo persistido: `[nao verificado]`; as condições que determinam esses resultados estão nas linhas citadas acima. [nao verificado]

## 30/10 12:39 · sessao 0865afc7bf13 · - · - · origem=Ronald (tmux)

3. **sem achado de quebra de leitor/teste existente por `valor_depois__via`.** A deduplicação passa a distinguir `abrir` de `criar`; o JSON gravado continua incluindo `via` e os demais campos (`app/chamados/models.py:923-931`). O teste que exige uma linha repete somente `abrir`, portanto a chave nova mantém sua expectativa (`app/chamados/tests/test_o261_veto_na_porta.py:430-439`).

- **Mudança real de cardinalidade:** para o mesmo objeto/módulo/dia/veto, `abrir` seguido de `criar` agora registra duas linhas, pois as entradas passam respectivamente `via='abrir'` e `via='criar'` (`app/chamados/models.py:733`, `app/chamados/models.py:842`, `app/chamados/models.py:923-931`). Não há teste misto exigindo uma linha nas referências localizadas; resultado da execução da suíte: `[nao verificado]`.
- Os testes antigos que contam `nascimento_recusado` sobre célula absolvida exercitam `_trilha_da_recusa`, cuja chave continua módulo/veredito e não recebeu a mudança (`app/ponto/tests/test_o_outbox_celula.py:348-353`, `app/ponto/tests/test_o_outbox_celula.py:544-552`, `app/chamados/models.py:934-949`).
- O histórico do chamado consulta apenas `estado_chamado`, excluindo essas recusas (`app/chamados/services/historico.py:44`, `app/chamados/services/historico.py:86-95`). A tela geral de auditoria conta linhas reais e não pressupõe uma linha por veto; mostrará as duas operações separadas (`app/core/views.py:705-727`).

**FILA VETO-TRILHA-CICLO continua procedente.** Contraexemplo: veto por `VETO_REGUA` em `abrir` → correção permite nascimento → veto idêntico retorna e outro `abrir` é recusado. A nova recusa encontra a primeira linha histórica e retorna sem registro, pois o `exists()` não considera correção, intervalo ou último evento (`app/chamados/models.py:923-926`). Isso contraria a promessa “enquanto nada muda” do próprio método; não demonstra falha do bloqueio do nascimento, que continua retornando antes da criação (`app/chamados/models.py:910`, `app/chamados/models.py:730-735`).

## 30/10 12:07 · sessao 227f3905545f · O261-VETO-A2 · verificador · origem=Code

**REPROVADO para fechamento:** há reabertura automática que contorna o veto e o selo não a detecta; a promessa de cadastro para ATIVO sem vínculo também não está atendida nos fluxos examinados. (`app/chamados/services/disputa_emissao.py:1606`; `app/chamados/tests/test_o261_veto_na_porta.py:207`; `app/colaboradores/queries.py:64`)

Correspondência da cópia com HEAD/hash e base d2c37edd: [nao verificado]. Execução da bateria Django, ocorrências em produção, reversão prévia e hashes do gravado exportado: [nao verificado].

Sobre a **pergunta 1**, o registro declara `motivo_do_veto` como juiz; o lookup JUIZES.md manda prevalecer o registro vivo. (`app/core/juizes.py:248`; `app/docs/JUIZES.md:11`; `app/docs/JUIZES.md:14`)

A cadeia dos campos e valores afetados é esta:

| Campo/valor | Quem escreve ou determina | Quem lê no trecho afetado |
|---|---|---|
| Universo dos módulos | `FURO_MODULES` + `MODULOS_DE_TURNO`, unidos por `modulos_do_veto`. (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/reconciliador.py:336`; `app/ponto/services/furos_vetados.py:79`) | Porta, contador e teste dos cinco módulos. (`app/chamados/models.py:889`; `app/ponto/services/furos_vetados.py:95`; `app/chamados/tests/test_o261_veto_na_porta.py:384`) |
| `colaborador`, `modulo_origem`, `contexto_json`, `celula` do chamado | Nascimento por `abrir`/`criar`; `_celula_de` resolve a associação pelo dia. (`app/chamados/models.py:725`; `app/chamados/models.py:735`; `app/chamados/models.py:830`; `app/chamados/models.py:850`; `app/chamados/models.py:974`) | `_veto_da_regua` e, indiretamente, `dia_do_fato`. (`app/chamados/models.py:889`; `app/chamados/models.py:892`; `app/chamados/models.py:895`) |
| Dia do fato | `dia_do_fato(fortes=True)` lê primeiro `celula.data`, depois o contexto conforme o catálogo. (`app/chamados/juizes.py:78`; `app/chamados/juizes.py:89`; `app/chamados/juizes.py:92`) | Porta e contador usam essa mesma autoridade. (`app/chamados/models.py:892`; `app/ponto/services/furos_vetados.py:101`) |
| Vínculo do dia, inclusive `None` | `vinculo_do_emissor` delega a `escala_vigente`, que alimenta `vinculo_do_dia`; geradora e vigência continuam nessa cadeia. (`app/ponto/services/furos_vetados.py:23`; `app/escala/servico_jornada.py:38`; `app/escala/servico_jornada.py:55`; `app/escala/alimentacao.py:196`) | Juiz, cartório, c2, passivo e contador. (`app/ponto/services/furos_vetados.py:65`; `app/ponto/services/cartorio.py:1013`; `app/chamados/reconciliador.py:854`; `app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_vetados.py:106`) |
| Template/marcos usados para degeneração | `TipoEscala.marcos_do_dia`; nas fixtures novas, `dar_vinculo` grava template, posto e vínculo ativo desde `desde`. (`app/escala/models.py:326`; `app/chamados/tests/_vinculo_do_dia.py:14`; `app/chamados/tests/_vinculo_do_dia.py:23`) | `regua_degenerada`, chamado pelo juiz e pela função com efeito de cadastro. (`app/escala/regua_defesa.py:23`; `app/escala/regua_defesa.py:26`; `app/ponto/services/furos_vetados.py:40`; `app/escala/regua_defesa.py:46`) |
| `regua_avaliada_em`, confiança, dias baixos e corroborada | Avaliação/corroborar/desfazer usam `gravar_regua`, que persiste os campos. (`app/escala/regua_defesa.py:662`; `app/escala/regua_defesa.py:719`; `app/escala/regua_defesa.py:765`; `app/colaboradores/services/vinculo.py:911`) | Veto de suspeita lê avaliação/faixa/dias baixos; o relatório também lê `regua_corroborada_em`. (`app/escala/regua_defesa.py:560`; `app/escala/regua_defesa.py:644`; `app/ponto/services/furos_vetados.py:70`) |
| `VETO_DEGENERADA` / `VETO_REGUA` / `None` | Retorno do juiz; não é campo persistido por ele. (`app/ponto/services/furos_vetados.py:40`) | Leitores discriminados na tabela seguinte. (`app/chamados/models.py:896`; `app/ponto/services/cartorio.py:1013`) |
| `status_local` e vitalidade | Nascimento nas duas portas; transição em `_gravar_estado`. As duas escritas históricas da 0038 permanecem na allowlist. (`app/chamados/models.py:735`; `app/chamados/models.py:850`; `app/chamados/models.py:192`; `app/chamados/migrations/0038_a3_registrado_backfill.py:10`; `app/chamados/migrations/0038_a3_registrado_backfill.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:40`) | Dedup, emissores, c2 e contador. (`app/chamados/models.py:685`; `app/ponto/management/commands/detectar_ausencias.py:734`; `app/ponto/management/commands/reconciliar_grade.py:521`; `app/chamados/reconciliador.py:888`; `app/ponto/services/furos_vetados.py:95`) |
| Trilha: ação/modelo/objeto, `modulo_origem`, `dia`, `veto`, `via`, colaborador | `_trilha_do_veto` monta; `evento` grava `LogAuditoria`. (`app/chamados/models.py:915`; `app/chamados/models.py:924`; `app/core/observ.py:134`) | A própria deduplicação e os testes novos consultam o veto persistido. (`app/chamados/models.py:921`; `app/chamados/tests/test_o261_veto_na_porta.py:362`) |
| Impressão e carimbos da célula | Cartório calcula a impressão; `carimbar` persiste impressão, julgada/insumos, ausência e entrada/saída; `lavrar_veredito` escreve veredito/protestos/ata. (`app/ponto/services/cartorio.py:1083`; `app/ponto/portas/celula.py:721`; `app/ponto/portas/celula.py:689`) | A igualdade da impressão decide o pulo; as fontes de células/protestos alimentam os dois leitores de furos. (`app/ponto/services/cartorio.py:1092`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/furos_sem_cobranca.py:61`) |
| Achados e contadores | `furos_vetados_por_regua` e `chamados_vivos_em_dia_vetado` montam os resultados em memória. (`app/ponto/services/furos_vetados.py:69`; `app/ponto/services/furos_vetados.py:108`) | Comando de relatório; desfazer corroboracão filtra pelo vínculo; placar extrai somente o contador antigo. (`app/ponto/management/commands/furos_vetados_por_regua.py:29`; `app/ponto/management/commands/furos_vetados_por_regua.py:44`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`; `bin/placar_code.sh:210`) |

Os leitores de produção do **juiz**, incluindo aliases, têm estes efeitos quando `ec=None` passa a devolver `VETO_DEGENERADA`:

| Leitor | Mudança |
|---|---|
| Porta `_veto_da_regua` | Recusa nascimento por `abrir`/`criar` e renascimento por `_renascer_por_premissa_viva`, havendo dia forte. (`app/chamados/models.py:731`; `app/chamados/models.py:778`; `app/chamados/models.py:838`; `app/chamados/models.py:893`) |
| Cartório `_veto_do_dia` | Acrescenta `veto:VETO_DEGENERADA` à impressão do ramo normal e pode provocar relavratura. Isso não determina, sozinho, qual será o novo veredito. (`app/ponto/services/cartorio.py:1013`; `app/ponto/services/cartorio.py:329`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1108`) |
| `furos_vetados_por_regua` | Passa a incluir furo sem vínculo, retornando vínculo/corroborada `None`; o comando conta o furo sem contar um vínculo inexistente. (`app/ponto/services/furos_vetados.py:65`; `app/ponto/services/furos_vetados.py:69`; `app/ponto/management/commands/furos_vetados_por_regua.py:33`) |
| `furos_sem_cobranca.medir` | Classifica como porta `veto`, fora do total esperado zero e fora da cobrança pelo reparador. (`app/ponto/services/furos_sem_cobranca.py:30`; `app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_sem_cobranca.py:109`; `app/ponto/services/furos_sem_cobranca.py:125`) |
| c2 | Barra nascer/renascer; a costura em chamado já vivo continua dispensando esse veto. (`app/chamados/reconciliador.py:808`; `app/chamados/reconciliador.py:852`; `app/chamados/reconciliador.py:855`) |
| `chamados_vivos_em_dia_vetado` | Passa a listar o vivo sem vínculo, mantendo os chamados sem dia em contador separado. (`app/ponto/services/furos_vetados.py:101`; `app/ponto/services/furos_vetados.py:106`) |

Detector e grade ainda usam as funções antigas antes de pedir a porta; são as cópias nominalmente toleradas pelo lote A. O retroativo lê indiretamente o juiz pelo dry `veto_no_nascimento`, mas já descarta `ec=None` antes desse ponto. (`app/ponto/management/commands/detectar_ausencias.py:600`; `app/ponto/management/commands/reconciliar_grade.py:487`; `app/chamados/tests/test_o261_veto_na_porta.py:46`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`)

Sobre a **pergunta 2: sem achado de intenção “sem vínculo/sem escala” mascarada pela migração**. O caso explícito “sem escala” de F4 não passa pelo helper que acrescenta vínculo; o caso novo sem vínculo cria outro colaborador e exige `(None, False)`. “Tranca sem cadastro” testa a exclusão dos avisos de cadastro da tranca. (`app/chamados/tests/test_selo_f4_fatia4.py:67`; `app/chamados/tests/test_selo_f4_fatia4.py:131`; `app/chamados/tests/test_o261_veto_na_porta.py:395`; `app/chamados/tests/test_tranca_sem_cadastro.py:39`; `app/chamados/tests/test_tranca_sem_cadastro.py:50`)

Há, entretanto, **prova insuficiente de nascimento em dois testes tocados**, preexistente à inserção do helper no diff fornecido: ambos prometem nascimento no nome, mas somente chamam `_ctx_c2` e verificam o dicionário. Contraexemplo: a porta recusar todo nascimento enquanto `_ctx_c2` continua retornando aquele contexto; as asserções continuam satisfeitas. (`app/chamados/tests/test_emissor_nomeia_o_marco.py:88`; `app/chamados/tests/test_bug125_cobranca_cega.py:176`)

Sobre a **pergunta 3, há achado: o escritor existe, mas ATIVO sem vínculo não o alcança pelos fluxos normais examinados**.

`vetar_cobranca_se_degenerada` aceita `None` e abre `regua_degenerada` com template ausente; a nova porta não veta esse módulo. Seus chamadores são detector e grade. (`app/escala/regua_defesa.py:45`; `app/escala/regua_defesa.py:51`; `app/chamados/models.py:889`; `app/ponto/management/commands/detectar_ausencias.py:600`; `app/ponto/management/commands/reconciliar_grade.py:487`)

O detector exige vínculo cobrindo hoje e ainda pula `ec=None`; a grade descarta escala ausente antes da emissão; o cartório também exige vínculo antes de chamar o detector. **Contraexemplo:** ATIVO com batidas, empresa em rollout e nenhum vínculo cobrindo hoje/ontem: não chega ao escritor de cadastro; uma tentativa de nascimento na nova porta é recusada pelo juiz puro, sem abrir cadastro. Essa ausência de alcance já está nos trechos anteriores ao diff; não é uma regressão nova demonstrada. (`app/colaboradores/queries.py:64`; `app/ponto/management/commands/detectar_ausencias.py:140`; `app/ponto/management/commands/reconciliar_grade.py:157`; `app/ponto/services/cartorio.py:1348`; `app/ponto/services/cartorio.py:1446`; `app/ponto/services/furos_vetados.py:40`)

O teste de cadastro prova a função isolada com vínculo e template degenerado; o outro só procura o nome da função no texto dos emissores. Não prova ATIVO sem vínculo chegando automaticamente ao escritor. (`app/ponto/tests/test_f3_regua_defesa.py:49`; `app/ponto/tests/test_f3_regua_defesa.py:53`; `app/ponto/tests/test_f3_regua_defesa.py:61`)

Sobre a **pergunta 4, há dois escapes de reabertura**:

- **HX-RESPOSTA-VISIVEL automático:** o signal de fechamento chama o resolver, que chama `_pai.reabrir()` diretamente. O veto novo está no renascer; `reabrir()` consulta outras recusas e grava a vitalidade sem perguntar esse veto. **Contraexemplo:** pai legado `entrada_bloqueada_turno_aberto`, encerrado por sistema, dia forte, sem vínculo, sem irmão/agrupamento, disputa aberta com resposta pendente creditada ao pai após o destino encerrar. Esse módulo pode ficar fora da guarda de premissa de furo; o pai reabre apesar de `VETO_DEGENERADA`. Ocorrência em produção: [nao verificado]. (`app/chamados/signals.py:89`; `app/chamados/services/disputa_emissao.py:1598`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:778`; `app/chamados/models.py:984`; `app/chamados/models.py:1078`; `app/chamados/models.py:1158`; `app/chamados/juizes.py:1471`)

- **Grade pode desfazer a recusa do renascimento:** `abrir()` devolve o existente encerrado mesmo quando o renascer recusou; a grade então chama `ch.reabrir()`. A guarda nova `ch is None` não cobre esse retorno. **Contraexemplo:** vínculo saudável pré-carregado pela grade; cadastro muda antes da releitura da porta; a cópia antiga passa, a porta veta o renascimento e devolve o encerrado, e a grade tenta reabrir sem veto. Mantendo célula furada e demais guardas permissivas, esse caminho contorna a recusa. Ocorrência em produção: [nao verificado]. (`app/ponto/management/commands/reconciliar_grade.py:189`; `app/ponto/management/commands/reconciliar_grade.py:487`; `app/chamados/models.py:685`; `app/chamados/models.py:896`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`; `app/chamados/models.py:1039`)

Há ainda estes achados nos sítios criados/tocados:

- **Selo sem cobertura dos escapes acima.** O varredor reconhece vivificação por gestores e construtor+save; não classifica `pai.reabrir()` como escrita. Contraexemplo pelo próprio varredor: `def automatico(pai): pai.reabrir()` produz listas vazias. As allowlists podem continuar verdes com esse escape presente; o MORDE atual não testa essa forma. (`app/chamados/tests/test_o261_veto_na_porta.py:29`; `app/chamados/tests/test_o261_veto_na_porta.py:207`; `app/chamados/tests/test_o261_veto_na_porta.py:209`; `app/chamados/tests/test_o261_veto_na_porta.py:264`)

- **Impressão de `nunca_bateu` não recebe o novo veto.** O ramo calcula `imp0` sem `veto`, compara a igualdade e retorna antes do leitor do juiz. Contraexemplo: célula já carimbada de colaborador sem batidas e sem vínculo, demais insumos iguais; a mudança para `VETO_DEGENERADA` não altera sua impressão e ela continua pulada. (`app/ponto/services/cartorio.py:907`; `app/ponto/services/cartorio.py:941`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:1003`; `app/ponto/services/cartorio.py:1013`)

- **Trilha deduplicada além do contrato declarado.** A promessa é por operação e “enquanto nada muda”, mas o filtro omite `via` e pesquisa toda a história. Contraexemplo: `abrir` recusado, depois `criar` recusado para a mesma chave: a segunda operação não deixa linha; corrigir e posteriormente voltar ao mesmo veto também encontra a recusa antiga. (`app/chamados/models.py:908`; `app/chamados/models.py:921`; `app/chamados/models.py:927`)

- **Contador novo não chega ao placar existente.** O comando imprime `chamados_vivos_em_dia_vetado`, porém o consumidor captura apenas `furos_vetados_por_regua`. Contraexemplo: surge um vivo vetado sem aumentar o contador antigo; a saída mostrada pelo placar permanece igual. (`app/ponto/management/commands/furos_vetados_por_regua.py:52`; `bin/placar_code.sh:209`)

A relavratura **não fica restrita à competência aberta pelo comando existente**: ele percorre todas as células passadas; no ramo normal, a impressão nova pode romper o pulo e chegar ao carimbo. Isso demonstra alteração possível da célula histórica, não alteração comprovada do gravado financeiro — o hash desse gravado mede `FechamentoMensal`. Reversão anterior, preservação por hash e diferença à Pauta DP: [nao verificado]. (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`; `app/ponto/services/cartorio.py:1424`; `app/ponto/portas/regen_exportada.py:62`)

Juiz paralelo novo, derivação paralela do vínculo/universo, relógio novo fora da borda e inversão nova de trava: **sem achado** nos trechos acrescentados; a trava do colaborador precede a leitura/dedup, e o veto delega às autoridades citadas. Custo efetivo no request e pré-requisitos operacionais de pouso: [nao verificado]. (`app/chamados/models.py:666`; `app/chamados/models.py:888`; `app/ponto/services/furos_vetados.py:79`)

## 30/10 12:04 · sessao dd342ba74c7a · O261-VETO-A2 · leitura · origem=Code

**Sim: o cron configurado pode relavrar células antigas, inclusive de competência exportada ou trancada. A seleção não tem piso de competência nem guarda de exportação/tranca.** (`app/ponto/management/commands/processar_cartorio.py:49-65`; `app/ponto/portas/celula.py:647-729`)

Correspondência desta cópia com o hash informado, instalação/execução efetiva dos crons e quantidade de células afetadas em produção: **[nao verificado]**.

**1) Caminho de rejulgamento e comandos**

- No ramo normal, o cartório consulta `motivo_do_veto` pelo vínculo do dia, inclui `veto:VETO_DEGENERADA` na impressão e compara com a gravada. A impressão diferente permite novo julgamento por `classificar_dia`, seguido de lavra e carimbo. (`app/ponto/services/cartorio.py:1011-1014`, `:1083-1108`, `:1406-1425`; `app/ponto/services/cartorio.py:329-330`)
- **Exceção:** o ramo sem adesão, `nunca_bateu`, calcula a impressão sem passar o veto. Portanto, essa alteração isolada não muda necessariamente a impressão de toda célula sem vínculo. (`app/ponto/services/cartorio.py:907`, `:941-943`)
- **Cron:** `processar_cartorio --apply`, diariamente às **06:27 / 06:29 / 06:31**, respectivamente para empresas **2 / 3 / 4**, pela expansão com passo de dois minutos. Varre **todas as células existentes com `data < hoje`**, sem data inicial e sem filtro de situação do colaborador. (`app/config/crons.py:17`, `:29-33`, `:597`, `:1785-1794`; `app/ponto/management/commands/processar_cartorio.py:49-65`)
- Também existe `vigia_de_hora --apply`, a cada dez minutos, cobrindo candidatas de hoje e certos turnos noturnos de ontem, pelo mesmo `julgar_celula`. Eventos de batida, ausência, justificativa e criação/mudança de status de chamado também acionam julgamento unitário. (`app/config/crons.py:348-349`; `app/ponto/services/vigia_de_hora.py:108-130`; `app/ponto/signals.py:131-165`; `app/chamados/signals.py:164-180`)

Há dois comandos relevantes:

| Comando | Argumentos e comportamento |
|---|---|
| `processar_cartorio` | `--empresa N` obrigatório; `--limite N`; `--forcar` ignora impressão igual. Sem `--apply`, julga/reporta sem persistir; com ele, grava. (`app/ponto/management/commands/processar_cartorio.py:25-29`, `:65`) |
| `celula_veredito_velho` | `--mes M --ano A`, opcionais `--empresa N`, `--json`, `--sem-recalculo`, `--so-recalculo`; aplicação por `--aplicar --antes ARQUIVO`. Recorta a janela da competência por empresa até ontem. (`app/ponto/management/commands/celula_veredito_velho.py:37-54`, `:75-80`) |

No segundo, sem `--aplicar`, o ensaio força a relavratura e, por padrão, recalcula o fechamento dentro de transação com **rollback**; `--sem-recalculo` evita esse recálculo. Com `--aplicar`, exige snapshot `--antes` e **desliga o recálculo**, gravando a relavratura forçada. (`app/ponto/management/commands/celula_veredito_velho.py:90-114`, `:133-159`)

**2) Exportada ou trancada: alcança; não há guarda nessa relavratura**

O lote e as portas de lavra/carimbo não consultam `ExportacaoDominio` nem `PeriodoFechado`. `--forcar` também não introduz proteção de competência. (`app/ponto/management/commands/processar_cartorio.py:49-65`; `app/ponto/services/cartorio.py:1092-1097`; `app/ponto/portas/celula.py:647-729`)

Existe proteção da **decisão administrativa**: `veredito_via='admin'` preserva o veredito humano, mas permite atualizar a ata e depois o carimbo. Existe ainda `PISO_EMISSAO='2026-09'`, que restringe emissão automática de chamados, sem impedir a relavratura da célula. (`app/ponto/portas/celula.py:671-677`, `:708-728`; `app/ponto/services/cartorio.py:190`, `:1319-1327`)

**3) Dinheiro gravado e recálculo**

A relavratura direta escreve campos de `CelulaDia` e pode emitir `EventoCelula`; **não regrava por si só `FechamentoMensal`, `DiaPago` ou o conteúdo/hash de uma exportação existente**. (`app/ponto/services/cartorio.py:1406-1425`; `app/ponto/portas/celula.py:623-642`, `:647-729`)

Quem persiste os valores calculados é `recalcular_fechamento_mes`: salva/atualiza `FechamentoMensal` e chama a lavratura de `DiaPago`, que substitui suas linhas por colaborador/competência/versão. (`app/ponto/services/fechamento.py:1105-1169`; `app/ponto/services/dia_pago.py:357-358`)

Os gatilhos incluem:

- Botão **Recalcular** e command `recalcular_fechamento --mes M --ano A [--empresa N] [--colabs IDs] --apply`; sem `--apply`, esse command apenas fotografa o gravado. Não há cron desse comando no registro. (`app/ponto/views.py:1227-1253`; `app/ponto/management/commands/recalcular_fechamento.py:77-120`; `app/config/crons.py:1071-1078`)
- `recalcular_por_evento`, que agenda em `on_commit` o recálculo do colaborador na competência do fato, por exemplo após registrar batida. (`app/ponto/services/fechamento.py:1766-1792`; `app/ponto/registro_batida.py:160`, `:175-178`)

**Há uma ressalva indireta:** a lavra pode alimentar o consumidor de eventos, que pode fechar disputa; esse fechamento pode retomar regularização já confirmada, criar batida e assim disparar recálculo. A existência desse encadeamento nos casos afetados é **[nao verificado]**. (`app/chamados/reconciliador.py:1296-1305`, `:1118-1120`; `app/chamados/signals.py:97-121`; `app/chamados/services/regularizacao_ext.py:218-224`, `:76-77`; `app/ponto/registro_batida.py:160`)

No escritor financeiro, há **guarda específica para 09/2026 nas empresas exportadas**; exportação não é bloqueio genérico. (`app/ponto/services/fechamento.py:36-43`, `:523-559`)

**Um próximo TXT pode mudar sem recálculo previamente persistido:** gerar TXT calcula o fechamento em modo leitura, monta suas rubricas e cria outro `ExportacaoDominio` com conteúdo/hash próprios; o registro anterior permanece. (`app/folha/views.py:207-212`, `:268-299`; `app/folha/models.py:174-187`)

**4) Campos gravados e reversão**

| Operação | Campos |
|---|---|
| Lavra | `veredito`, `veredito_via`, `veredito_em`, `protestos`; `ata` quando diferente. (`app/ponto/portas/celula.py:669-695`) |
| Carimbo | `julgada_em`, `impressao`, `ausencia`, `tem_entrada`, `tem_saida`, `insumos_em`, junto dos campos lavrados. (`app/ponto/portas/celula.py:721-728`) |

O julgamento unitário também pode atualizar `ata.precedencia`. Essa relavratura não regenera DNA/origem nem produz `dna_anterior`. (`app/ponto/services/cartorio.py:1563-1568`, `:719-724`)

Há `LogAuditoria`, ação `veredito_celula`, com antes/depois **somente de veredito, via e protestos**. A trilha é best-effort e não constitui reversão completa de ata, impressão, insumos e carimbos. `EventoCelula` também não guarda antes/depois. (`app/ponto/portas/celula.py:657`, `:699-702`; `app/core/observ.py:94-105`; `app/escala/models.py:1800-1815`)

O snapshot exigido por `celula_veredito_velho --aplicar` guarda decisão, protestos, ata, impressão e datas de veredito/julgamento, mas **omite `ausencia`, `tem_entrada`, `tem_saida` e `insumos_em`**: sua cobertura de reversão é parcial. Snapshot efetivamente produzido para esta obra: **[nao verificado]**. (`app/ponto/management/commands/celula_veredito_velho.py:100-114`; `app/ponto/portas/celula.py:721-728`)

## 30/10 12:04 · sessao 1415e2588123 · - · - · origem=Ronald (tmux)

**Q3 — relavratura e dinheiro**

- **Relavrar a célula não chama diretamente o recálculo financeiro.** O cartório lavra veredito/ata e carimba os insumos; as portas persistem campos da célula, auditoria e `EventoCelula`. Referências: `app/ponto/services/cartorio.py:1406-1425`; `app/ponto/portas/celula.py:634-642,647-729`.
- O consumidor desse evento chama retratação dos chamados e encerramento de perguntas órfãs; não chama diretamente `recalcular_por_evento`. Referência: `app/chamados/reconciliador.py:1296-1305`.
- **Não prometer ausência absoluta de efeito indireto:** o consumidor pode fechar disputa (`app/chamados/reconciliador.py:1118-1120`); seu signal agenda retomada de regularizações anteriormente confirmadas (`app/chamados/signals.py:97-121`); a retomada pode criar batida pela porta (`app/chamados/services/regularizacao_ext.py:218-225,70-81`), que dispara recálculo (`app/ponto/registro_batida.py:157-160,175-178`). Existência dessa situação nos dados fornecidos: **[nao verificado]**.

**Quem recalcula e quando**

- `recalcular_por_evento` agenda em `transaction.on_commit` o recálculo de **um colaborador na competência do fato**, por `competencia_do_fato`; captura falhas sem desfazer o fato. Referência: `app/ponto/services/fechamento.py:1766-1795`.
- Seus gatilhos de produção encontrados são: batida (`app/ponto/registro_batida.py:175-178`); decisão de HE (`app/ponto/portas/he.py:201-202`); folga aceita e resposta validada (`app/chamados/services/validacao.py:323-324,399-401`); correção de vínculo, uma chamada por competência tocada (`app/ponto/portas/celula.py:384-399`).
- Também há botão POST com permissão `editar_folha`, chamando o mesmo serviço (`app/ponto/views.py:1227-1253`), e command `recalcular_fechamento --mes --ano [--empresa] [--colabs ids] [--apply] [--motivo-exportada texto]`; sem `--apply`, apenas fotografa o gravado, sem simular recálculo (`app/ponto/management/commands/recalcular_fechamento.py:77-120`).
- O registro declara que `recalcular_fechamento` não tem cron deliberadamente (`app/config/crons.py:1071-1078`). O cron **`pre_fechamento --apply`, às 05:25**, escreve apenas Pauta DP; lê bloqueios e marcos, sem chamar o recálculo financeiro (`app/config/crons.py:387`; `app/folha/management/commands/pre_fechamento.py:36-77,85-112`). Comentários antigos dizendo que “o cron rega o gravado” não correspondem a um cron financeiro identificado nesses caminhos (`app/ponto/registro_batida.py:170-173` versus `app/config/crons.py:1071-1078`).

**O que o recálculo financeiro escreve**

- `FechamentoMensal`: salva os campos derivados quando aberto; quando aprovado, atualiza os campos calculados por `update`, preservando o carimbo de aprovação. Referência: `app/ponto/services/fechamento.py:1105-1158`.
- `DiaPago`: o mesmo serviço chama `dia_pago.lavrar`, com os resultados usados no fechamento; essa lavratura substitui as linhas do colaborador/competência/versão. Referências: `app/ponto/services/fechamento.py:1159-1205`; `app/ponto/services/dia_pago.py:335-358`.
- **Guarda temporária atual:** competência **09/2026**, nas empresas que a exportaram, tem colaboradores excluídos da escrita financeira, inclusive backfill de `DiaPago`; há trilha de recusa. Não é guarda genérica de toda competência exportada. Referências: `app/ponto/services/fechamento.py:36-43,523-559`.
- `celula_veredito_velho --aplicar --antes arquivo` **desliga recálculo financeiro** e grava snapshot da célula antes de rejulgar. Sem `--aplicar`, o ensaio pode executar recálculo dentro de `atomic`, seguido de rollback. Referência: `app/ponto/management/commands/celula_veredito_velho.py:90-114,133-159`.

**TXT e rubricas**

- Relavratura/recálculo não reescrevem o `conteudo`/`hash_sha256` de um TXT antigo nesses caminhos; cada nova exportação cria seu próprio registro, conteúdo e hash. Até a porta de invalidação preserva conteúdo/hash, alterando apenas os campos de invalidação. Referência: `app/folha/models.py:143-187`.
- **Importante:** um **novo Gerar TXT já deriva os números online**, com `somente_leitura=True`, e monta suas linhas/rubricas a partir desses objetos; portanto o próximo TXT pode refletir a célula relavrada **sem que o fechamento gravado tenha sido previamente recalculado**. Referências: `app/folha/views.py:207-212,265-268,280-300`. O mapeamento cadastrado de rubricas é apenas consultado pela montagem (`app/folha/export.py:958-965`).
- Efeito monetário concreto dessa alteração do veto no dado fornecido: **[nao verificado]**.

## 30/10 12:04 · sessao 381499a71b54 · - · - · origem=Ronald (tmux)

Achados para consolidação:

1. **Reabertura automática escapa do veto pelo HX-RESPOSTA-VISIVEL.** O signal de fechamento chama automaticamente o resolver (`app/chamados/signals.py:75-94`); ele reabre o pai por `_pai.reabrir()` direto quando há resposta pendente creditada (`app/chamados/services/disputa_emissao.py:1598-1608`). O veto novo está somente em `_renascer_por_premissa_viva` (`app/chamados/models.py:778-781`); `reabrir` consulta outras recusas e grava vida sem perguntar o veto (`app/chamados/models.py:1119-1158`).

   **Contraexemplo:** pai legado `entrada_bloqueada_turno_aberto`, encerrado por sistema, dia forte no contexto, sem vínculo no dia, sem agrupamento/irmão vivo; disputa aberta com pergunta respondida e não validada cujo destino acabou de encerrar. O juiz devolveria `VETO_DEGENERADA` (`app/ponto/services/furos_vetados.py:40-41`), mas o resolver chama `reabrir` diretamente (`app/chamados/services/disputa_emissao.py:1606`). Esse módulo não pertence a `CALADOS_POR_ISENCAO` (`app/chamados/catalogo/modulos.py:235-241`), portanto, sem categoria/subcategoria legada de furo, também evita a guarda de premissa (`app/chamados/models.py:984-990`, `app/chamados/models.py:1078-1080`). A resposta pendente que ativa o ramo é selecionada sem consultar vínculo/veto (`app/chamados/juizes.py:1471-1477`). Ocorrência em produção: [nao verificado].

2. **Selo não fiscaliza esse escape.** O varredor só trata `update`/`bulk_update` como vivificação (`app/chamados/tests/test_o261_veto_na_porta.py:29`) e identifica pedidos à porta apenas quando a cadeia nomeia o modelo (`app/chamados/tests/test_o261_veto_na_porta.py:207-208`); chamadas `pai.reabrir()` não entram no conjunto de escritas (`app/chamados/tests/test_o261_veto_na_porta.py:209-219`). O caso MORDE de escrita cobre gestores e construtor+save, sem reabertura de instância (`app/chamados/tests/test_o261_veto_na_porta.py:264-297`).

   **Contraexemplo:** `def automatico(pai): pai.reabrir()` produz `([], [])` pelo próprio `varrer_fonte`; conferido executando somente as funções AST em memória, com `python3 -B`, sem Django/banco nem arquivos escritos. A limitação está nas condições citadas (`app/chamados/tests/test_o261_veto_na_porta.py:207-219`).

3. **Grade conserva outra chamada automática direta que pode desfazer a recusa da porta.** `abrir` devolve o chamado encerrado mesmo quando `_renascer_por_premissa_viva` recusou (`app/chamados/models.py:685-688`); `_emitir_furo` então chama `ch.reabrir()` se ele continuar encerrado (`app/ponto/management/commands/reconciliar_grade.py:519-522`). A nova guarda `ch is None` não cobre esse retorno de dedup (`app/ponto/management/commands/reconciliar_grade.py:519-520`).

   **Contraexemplo:** o vínculo saudável recebido em `ec` passa pela cópia de veto (`app/ponto/management/commands/reconciliar_grade.py:486-492`), mas o vínculo relido pela porta é ausente/suspeito (`app/chamados/models.py:896`): a porta recusa o renascimento, devolve o encerrado, e o ramo seguinte tenta reabrir sem veto. O caller pré-carrega o mapa de vínculos antes da emissão (`app/ponto/management/commands/reconciliar_grade.py:187-197`), permitindo divergência se o cadastro mudar entre as leituras; ocorrência concreta em produção e efeito final após signals: [nao verificado].

4. **Trilha prometida por operação não inclui operação na deduplicação.** A docstring promete uma linha por objeto/módulo/dia/veto/operação (`app/chamados/models.py:908-910`), mas a consulta ignora `via` (`app/chamados/models.py:921-922`), embora a grave (`app/chamados/models.py:927-928`).

   **Contraexemplo:** `abrir` recusado e depois `criar` recusado para mesmo colaborador/célula, módulo, dia e motivo: a segunda operação não deixa linha própria (`app/chamados/models.py:921-923`). Também não nasce nova linha depois de correção e posterior regressão ao mesmo veto, pois a busca cobre toda a história (`app/chamados/models.py:921-923`).

Abertura manual atual bloqueada com data: **sem achado concreto**. A UI do DP restringe módulos aos tipos abríveis do catálogo (`app/chamados/services/abertura.py:148-153`); os cinco módulos do universo não têm abertura declarada (`app/chamados/catalogo/modulos.py:105-112`, `app/chamados/catalogo/modulos.py:182-183`). A abertura do colaborador passa contexto sem dia (`app/chamados/services/abertura.py:58-64`), e a API cria `app_colaborador`, fora do universo (`app/api/views.py:2237-2246`). A limitação genérica de `criar` existe porque ele aplica o veto sem distinguir autoria (`app/chamados/models.py:838-841`), mas um fluxo humano atual do universo com dia que a acione está [nao verificado].

## 30/10 12:03 · sessao dfd42513de12 · - · - · origem=Ronald (tmux)

Achei dois problemas de alcance; a ausência de vínculo continua sem chamado de cadastro no fluxo normal, e uma impressão de célula continua sem consultar o veto. (`app/colaboradores/queries.py:64`; `app/ponto/services/cartorio.py:941`)

1. **Cadastro ATIVO sem vínculo: o escritor existe, mas os fluxos automáticos não chegam nele.** `vetar_cobranca_se_degenerada` aceita `ec=None`, chama `regua_degenerada(None, data)` e abre `regua_degenerada`, deduplicado por `tipo_escala_id=None`; esse módulo fica fora do universo vetado pela porta. (`app/escala/regua_defesa.py:45`; `app/escala/regua_defesa.py:51`; `app/chamados/models.py:889`)

   Seus dois chamadores de produção são detector e grade. O detector consulta `colaboradores_em_operacao`, que exige vínculo cobrindo hoje, e ainda pula `ec=None` no laço. A grade descarta `ev=None`/`te=None` antes de chamar `_emitir_furo`. O cartório também exige vínculo para chamar o detector, tanto no furo integral quanto no parcial. (`app/ponto/management/commands/detectar_ausencias.py:98`; `app/colaboradores/queries.py:64`; `app/ponto/management/commands/detectar_ausencias.py:139`; `app/ponto/management/commands/reconciliar_grade.py:157`; `app/ponto/marcos.py:107`; `app/ponto/management/commands/reconciliar_grade.py:254`; `app/ponto/services/cartorio.py:1347`; `app/ponto/services/cartorio.py:1446`)

   **Contraexemplo:** colaborador ATIVO, empresa em rollout, usuário operacional, com batidas existentes, mas sem qualquer vínculo cobrindo hoje/ontem. O detector o exclui; a grade o inclui pelas batidas, porém não chega ao escritor; uma tentativa direta de nascimento do universo é vetada pelo juiz puro, que não abre cadastro. Portanto a promessa “ativo sem vínculo segue abrindo `regua_degenerada`” não está atendida nesses caminhos. A falha já existia antes do diff apresentado; regressão nova dessa ausência: [nao verificado]. (`app/colaboradores/queries.py:25`; `app/colaboradores/queries.py:64`; `app/ponto/management/commands/reconciliar_grade.py:84`; `app/ponto/management/commands/reconciliar_grade.py:106`; `app/ponto/management/commands/reconciliar_grade.py:157`; `app/chamados/models.py:896`; `app/ponto/services/furos_vetados.py:40`)

   O teste atual prova a função isolada com **vínculo existente e template degenerado**; o outro apenas procura o nome da função no texto dos emissores. Nenhum deles prova o caminho automático de ATIVO sem vínculo. (`app/ponto/tests/test_f3_regua_defesa.py:49`; `app/ponto/tests/test_f3_regua_defesa.py:53`; `app/ponto/tests/test_f3_regua_defesa.py:61`)

2. **Impressão de `nunca_bateu` continua fora do veto.** Sem batida apurável na vida, o cartório entra no ramo `adesao is None`, calcula `imp0` sem `veto=`, pode pular por igualdade e retorna antes da definição/cache de `_veto_do_dia`. (`app/ponto/services/cartorio.py:899`; `app/ponto/services/cartorio.py:907`; `app/ponto/services/cartorio.py:941`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:1003`; `app/ponto/services/cartorio.py:1011`)

   **Contraexemplo:** célula já carimbada de colaborador sem batidas e sem vínculo; todos os demais insumos permanecem iguais. A mudança `ec=None → VETO_DEGENERADA` não entra em `imp0`, e a célula continua sendo pulada. O ramo normal inclui o veto e reage à mudança; esse ramo não. (`app/ponto/services/cartorio.py:941`; `app/ponto/services/cartorio.py:943`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/furos_vetados.py:40`)

Quanto à relavratura, o próximo `processar_cartorio --apply` percorre todas as células passadas, sem piso de competência; no ramo normal, a nova palavra do veto altera a impressão e ultrapassa o pulo, chegando à lavra/carimbo. Isso prova alteração possível da própria célula exportada, mas **não prova alteração de `FechamentoMensal`, `DiaPago` ou TXT**. Reversão prévia, hash das exportadas e diferença enviada à Pauta DP: [nao verificado]. (`app/ponto/management/commands/processar_cartorio.py:49`; `app/ponto/management/commands/processar_cartorio.py:61`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`; `app/ponto/services/cartorio.py:1409`; `app/ponto/services/cartorio.py:1424`; `app/ponto/portas/celula.py:721`)

_478 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
