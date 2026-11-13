# CODEX-ESTADO

> Ultima resposta processada: **13/11/2026 11:31** · publicado em **13/11/2026 11:32** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 13/11 11:31 · sessao b8e26257f1cc · LEI-DESLIGADO-NAO-TRAVA · censo · origem=Code

**A cópia ainda permite dias retidos de colaborador desligado:** o retentor não consulta `situacao`; recorta pela demissão e pode reter células acusadoras até essa data. A porta de rescisão do TXT responde outra pergunta, usando apenas `data_demissao <= fim_da_janela`. (`app/folha/export.py:60`, `app/folha/export.py:1200-1266`)

Censo estático **FECHADO nesta cópia**: AST dos 939 arquivos Python fora dos diretórios `tests/` e `migrations/`, sem erro de parse, cruzada com `rg` global, imports, apelidos e chamadas por atributo. Foram encontrados oito sítios de chamada; `_mre` é o único apelido dessas funções. Método e contagem são resultados desta execução, sem artefato persistido [nao verificado]. A identidade da cópia com o HASH informado não pôde ser autenticada por Git [nao verificado].

**1. Todos os chamadores diretos, inclusive por apelido**

| Função | Chamador | O que faz com o retorno |
|---|---|---|
| `motivos_retencao_celula` | `app/folha/export.py:1275` | `motivos_retencao_espelho` devolve os motivos; seus três parâmetros antigos são ignorados. (`:1270-1275`) |
| mesma | `app/folha/fora_da_folha.py:98`, import `:97` | `_dias_que_retem` repassa `(motivos, dias)`. (`:98-99`) |
| mesma | `app/folha/previa.py:170`, import `:169` | Compara as datas retidas com as propostas; alimenta “passaria a entrar” e `mudariam_de_lado`. (`:119-121`, `:133`, `:171-173`) |
| mesma | `app/folha/services/vigia_pausa.py:125`, import `:48` | Alimenta contadores de barrados e dias retidos que seriam somente pausa. (`:126-136`) |
| `motivos_retencao_espelho` | `app/folha/export.py:968` | Retorno não vazio produz `status='fora'`, `motivo='furo_espelho'`, detalhe e `motivos_espelho`; encerra a classificação daquela pessoa. (`:969-973`) |
| mesma | `app/ponto/services/fechamento.py:1067`, import **`as _mre`** em `:1066` | Materializa `FechamentoMensal.motivos_espelho`, depois persiste esse campo. (`:1067-1068`, `:1186`) |
| `_dias_que_retem` | `app/folha/fora_da_folha.py:137` | Guarda motivos/dias por colaborador, somente para os classificados como `furo_espelho`. (`:119-139`) |
| mesma | `app/folha/fora_da_folha.py:217` | `o_que_segura` entrega data, veredito, motivo, fio, ação para destravar e chamados; junta o status/motivo do export. (`:215-231`) |

As cadeias indiretas principais são:

- **TXT:** `classificar_export` fornece os entrantes; `montar_linhas_export` exclui os demais. (`app/folha/export.py:714-727`)
- **Retrato/lavra:** `fora_da_folha` expõe quantidade e datas; `montar_linhas` transfere status, motivo, dias, datas, dono, tipo e saldo para o retrato. (`app/folha/fora_da_folha.py:184-197`, `app/folha/retrato_folha.py:1156-1188`)
- **Vigia de alcance:** lê `dias_que_seguram`, deduplica pares pessoa/data e procura a porta de atendimento. (`app/chamados/services/vigia_fio.py:563`, `:582-618`)
- **Perguntas/chamados:** a fábrica consulta situação e acusação da célula, sem chamar essas três funções. (`app/chamados/services/fabrica_da_celula.py:105-125`)

**2. Sítios que interpretam “retido/bloqueado/furo” sem chamar diretamente as três funções**

É necessário distinguir **leitura do resultado do juiz**, **interpretação de retrato** e **acusação da célula**: esses caminhos têm fontes diferentes. (`app/folha/export.py:968-973`, `app/folha/retrato_folha.py:89-111`, `app/chamados/services/fabrica_da_celula.py:120`)

| Sítio | Fonte e decisão |
|---|---|
| `app/folha/retrato_folha.py:89-95` | `_balde`: `entra` → entrantes; rescisão → módulo próprio; restante → bloqueado. **Interpreta resultado gravado**, sem reler célula. |
| `app/folha/retrato_folha.py:98-121` | Acessor e contadores: leem `retrato.linhas`; contador é o tamanho da mesma lista filtrada. |
| `app/escala/services/cadastro_realidade.py:181-200` | C5 começa em `veredito='nunca_bateu'`; para desligados, conserva exclusivamente datas presentes em linha **bloqueada/furo_espelho do retrato**. **Lê a lavra**, não o juiz vivo. |
| `app/folha/vigia_retido_calado.py:46-55` | Conta bloqueados com fio ninguém e desconta pessoas roteadas à C5. **Lê retrato e C5**. |
| `app/chamados/management/commands/disparar_perguntas_competencia.py:54-72` | Contador `nunca_bateu_sem_adesao`: desligado fora dos pares da C5 é barrado. **Lê C5**, sem recalcular retenção. |
| `app/folha/views.py:259-260` | Seleciona `furo_espelho` para aviso de exportação. **Lê classificação**. |
| `app/folha/views.py:331-341`, `:589-601` | Painel e raio X agrupam status/motivos do juiz. |
| `app/folha/retidos_pdf.py:32-38` | Chama **todo `status='fora'`** de retido, inclusive rescisão. **Lê classificação**. |
| `app/ponto/services/fechamento_lista.py:28-37` | Traduz o resultado do juiz para a tela; motivos sem tradução própria aparecem visualmente como `furo_espelho`, preservando `motivo_juiz`. |
| `app/relatorios/views.py:1658-1669` | Coluna Folha de “Quem cobrar”: traduz o resultado do juiz. |
| `app/relatorios/views_pend.py:58-69` | Pendências cadastrais: seleciona portas cadastrais da classificação. |
| `app/inteligencia/calculadores.py:210-234`, `:271-296` | Métricas encerrada/corrente contam status e motivos do juiz. |
| `app/api/views_mensageria.py:257-275` | Com snapshot, deriva `retidos = denominador − numerador`. **Lê projeção gravada da métrica**. |
| `app/api/views_mensageria.py:283-301` | Sem snapshot, consulta o juiz e conta todos os `fora`. |
| `mensageria/nucleo/ferramentas.py:1196`, `:1247-1268` | Separa rescisão como ressalva e soma outros motivos como “travam de fato”. **Interpreta a resposta da API**. |
| `app/folha/management/commands/simular_folha.py:74-90` | Registra os não entrantes e seus motivos/valores na foto de simulação. |
| `app/folha/services/vigia_txt.py:113-117`, `:231-232` | Compara status/motivo e TXT usando os mesmos itens do juiz. |
| `bin/censo_folha.py:14-16`, `:40-43` | Seleciona os `fora` e conta motivos; os “agravantes” são diagnóstico adicional. |

Há uma contagem com nome enganoso: painel/PDF usam `len(motivos_espelho)` como “dias”, mas o retentor devolve **motivos distintos**, enquanto datas ficam no segundo componente de `com_dias=True`. (`app/folha/views.py:398`, `app/folha/retidos_pdf.py:90`, `app/folha/export.py:1227-1230`, `:1266`)

Os seguintes caminhos **derivam acusação/cobrança, não retenção da folha**, e não recebem automaticamente as isenções do retentor:

- `vigia_fio.censo` antigo lê `VEREDITOS_ACUSAM`; o `censo_alcance` lê os dias do juiz da folha. (`app/chamados/services/vigia_fio.py:331`, `:378-380`, `:585`)
- `furo_sem_canal` seleciona vereditos próprios e ausência de pergunta/chamado. (`app/chamados/services/furo_sem_canal.py:35`, `:70-86`)
- `furos_sem_cobranca` seleciona furo/protestos e situação ativo. (`app/ponto/services/furos_sem_cobranca.py:61-63`)
- Fábrica e emissão de disputa leem acusação para nascimento/alcance de pergunta. (`app/chamados/services/fabrica_da_celula.py:112-125`, `app/chamados/services/disputa_emissao.py:757-773`)
- Vitalidade, resposta e reconciliação também leem acusação diretamente. (`app/chamados/juizes.py:1375`, `:1448-1451`; `app/chamados/models.py:3358-3359`; `app/chamados/reconciliador.py:642`, `:1028`; `app/chamados/services/limbo_folgas.py:195`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/silencio_ausencia.py:42`)
- Auditorias/relatórios de célula fazem leituras semelhantes. (`app/ponto/services/vigia_ausencia_veredito.py:41`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`; `app/relatorios/dia_da_frota.py:55`; `app/chamados/management/commands/auditar_invariantes_chamados.py:198`, `:343`, `:1391`, `:1605`, `:1647`)

Outro escritor real de `motivo='furo_espelho'`, além de `classificar_export`, não foi encontrado na varredura [nao verificado]; o escritor encontrado está em `app/folha/export.py:970`.

**3. Ordem das portas e os quatro casos de desligado**

O universo de `classificar_export` são os `FechamentoMensal` existentes da empresa/competência, ou os objetos injetados; ele não exige situação ativo. (`app/folha/export.py:783-788`)

Antes das saídas, calcula separadamente `travas_aprovacao`: conta vazia, turno aberto e ausência de âncora 12×36 — esta última somente para ativos. Essas travas acompanham inclusive quem sai pela rescisão. (`app/folha/export.py:806-848`)

| Ordem | Porta |
|---|---|
| 1 | Demissão não nula e `<= fim` → `rescisao_modulo_proprio`. (`app/folha/export.py:60`, `:866-871`) |
| 2 | Código Domínio vazio → `sem_codigo_dominio`. (`:872-875`) |
| 3 | Batida durante férias → `ferias_com_batida`. (`:876-884`) |
| 4 | Horas zero, pelo menos duas batidas e nenhum previsto → `cadastro_zero`. (`:902-967`) |
| 5 | Motivos do retentor não vazios → `furo_espelho`. (`:968-973`) |
| 6 | Código de rubrica fora do catálogo → `nao_homologado`. (`:974-979`) |
| 7 | Caso contrário → `entra`. (`:841-842`, `:980`) |

Para uma pessoa com **situação atual desligado**, havendo fechamento no universo:

| Demissão | Resultado atual | Pode virar `furo_espelho`? |
|---|---|---|
| **(a) Dentro da janela** | Sai na primeira porta, pela rescisão. O retentor chamado isoladamente ainda pode devolver dias anteriores e célula acusadora no dia da demissão. (`app/folha/export.py:866-871`, `:1208-1264`) | **Não na classificação**, porque a rescisão vem antes. (`:866-871`, `:968`) |
| **(b) Depois do fim** | Não sai pela rescisão desta competência; percorre as demais portas. (`:60`, `:866-968`) | **Sim**, se alcançar a porta do espelho e houver motivos. (`:968-973`) |
| **(c) Sem data** | Não sai pela rescisão; percorre as demais portas. Novo fechamento normalmente não nasce para esse desligado, mas fechamento existente/injetado é classificado. (`:60`, `:783-788`; `app/ponto/services/fechamento.py:432-441`) | **Sim**, com fechamento existente/injetado e motivos. (`app/folha/export.py:968-973`) |
| **(d) Antes do início** | Fechamento existente sai pela rescisão; a geração normal o exclui. O retentor isolado retorna vazio pela janela colapsada. (`app/folha/export.py:60`, `:866-871`, `:1208-1213`; `app/ponto/services/fechamento.py:432-441`) | **Não**. (`app/folha/export.py:866-871`) |

**4. Onde se decide “desligado”; afastado/INSS**

**Não há uma função única compartilhada para “desligado”.** Existem decisões pelo campo atual, pelo campo apresentado e exclusivamente pela data de demissão. (`app/colaboradores/services/situacao_exibida.py:56-63`, `app/ponto/services/fechamento.py:432-441`, `app/folha/export.py:60`)

Inventário dos predicados explícitos encontrados, agrupado pelo efeito:

| Domínio | Locais |
|---|---|
| Retenção, universo e contagem da folha | `app/ponto/services/fechamento.py:432`, `:440`; `app/folha/export.py:60`, `:810`; `app/ponto/views.py:1119-1121` |
| Emissão/vigias/listas | `app/ponto/services/cartorio.py:1748`, `:1813`; `app/ponto/services/vigia_de_hora.py:117`; `app/escala/services/cadastro_realidade.py:186`, `:198`, `:603`; `app/chamados/management/commands/disparar_perguntas_competencia.py:59`; `app/ponto/services/furos_vetados.py:62`; `app/ponto/services/vinculo_na_lista.py:71`, `:152` |
| Adesão/invariante de cobrança | `app/chamados/services/adesao.py:67`; `app/chamados/management/commands/auditar_invariantes_chamados.py:653` |
| Cadastro, vínculo, desligamento e reativação | `app/colaboradores/services/vinculo.py:91`; `app/colaboradores/services/desligamento.py:18`; `app/colaboradores/views.py:1417`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:40`; `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35`; `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:47-48` |
| Holerite, busca, indicadores, relatórios e férias | `app/holerite/matriz.py:97`; `app/core/busca.py:104`; `app/inteligencia/views.py:83`; `app/relatorios/views.py:833`, `:848`, `:857`; `app/ferias/views.py:605` |
| Filtros/painéis de colaboradores | `app/colaboradores/views.py:1046`, `:2850`, `:2900`, `:2916`; `app/colaboradores/services/situacao_exibida.py:33`, `:57` |
| Templates | `app/templates/colaboradores/partials/painel_colaborador.html:90`, `:163`; `app/templates/colaboradores/editar.html:12`, `:211`; `app/templates/colaboradores/partials/_lista_tabela.html:38` |
| Comando operacional fora de `tests/` | `app/ponto/management/commands/testar_pwa.py:150` |

Há guardas mais amplas que **não significam exclusivamente desligado**: API aceita apenas ativo/afastado; login, reabertura e parte da emissão exigem ativo. (`app/api/views.py:403-405`, `app/api/views_core.py:389-391`, `app/api/login_helpers.py:141`, `app/api/views.py:116`, `app/chamados/models.py:1006`, `app/chamados/reconciliador.py:821`, `app/chamados/services/fabrica_da_celula.py:112`)

**INSS é outra condição:** o campo admite `afastado`; o juiz temporal consulta `Ausencia.tipo='afastamento_inss'` cobrindo o dia. Para folha, a ausência precisa estar aprovada; para cobrança, não rejeitada. A apresentação preserva desligado antes de consultar afastamento. (`app/colaboradores/models.py:295-299`, `app/ponto/turnos.py:2516-2518`, `:2704-2732`, `app/colaboradores/services/situacao_exibida.py:56-63`)

**5. Superfícies atuais e informativo DP/rescisão**

| Superfície | Comportamento atual |
|---|---|
| **Supervisor / Quem cobrar** | Seu universo limita a ativos, portanto exclui desligados. A coluna Folha consulta o juiz. (`app/escala/services/furos_diarios.py:259`, `:273-275`; `app/relatorios/views.py:1658-1669`) |
| **Lista administrativa C5 — Cadastro × Realidade** | Pode mostrar desligado com dia `nunca_bateu` quando o retrato conserva aquele dia como bloqueado/furo. (`app/escala/services/cadastro_realidade.py:181-200`, `:876-879`; `app/escala/views.py:487-492`) |
| **Central/chamados/perguntas** | Novas perguntas pela fábrica exigem ativo; os emissores de furo do cartório calam desligados. A vida das perguntas existentes tem decisões próprias por acusação. (`app/chamados/services/fabrica_da_celula.py:112`; `app/ponto/services/cartorio.py:1748`, `:1813`; `app/chamados/juizes.py:1448-1451`) |
| **`o_que_segura`** | Pode devolver dias retidos mesmo quando seu campo global informa saída por rescisão: consulta classificação e dias separadamente. (`app/folha/fora_da_folha.py:215-231`) |
| **Drawer** | Lê o retrato gravado; **não chama `o_que_segura`**. Exibe as datas gravadas. (`app/colaboradores/views.py:977-989`; `app/templates/folha/_retrato_o_que_fazer.html:43-46`) |
| **Retrato/tela da folha** | Rescisão fica em `modulo_proprio`, fora de `bloqueado`; datas de furo vêm dos dias do retentor. (`app/folha/retrato_folha.py:33`, `:89-95`, `:1157-1188`) |
| **`vigia_fio`** | Alcance lê dias retidos do juiz; o contador antigo continua baseado em acusação/chamados. (`app/chamados/services/vigia_fio.py:331`, `:585-588`) |
| **`vigia_retido_calado`** | Lê bloqueados do retrato e desconta a C5 como roteamento. (`app/folha/vigia_retido_calado.py:46-55`) |
| **E-SEM-VIVA-B** | O contador operacional encontrado chama-se `nunca_bateu_sem_adesao`; lê a C5 e barra desligados fora dela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:39-72`) |
| **ESTADO** | Lê o carimbo do vigia; manchete usa `fio_morto_alcance`, e divergências usam o esperado gravado. (`bin/gerar_estado.py:260-261`; `bin/fio_estado.py:121-126`, `:148-154`, `:191-193`) |

Implementação de **`desligados_retidos`** não foi encontrada na cópia [nao verificado]. O carimbo atual inclui `fio_morto_alcance`, `fio_mudo_alvos` e `nunca_bateu_sem_adesao`. (`app/chamados/management/commands/vigiar_fio_morto.py:233-243`)

Há superfícies próximas ao informativo pedido:

- **Módulo próprio/rescisão:** já separa a pessoa da conta de bloqueados, mas não fornece seus dias sem batida. (`app/folha/retrato_folha.py:33`, `:89-95`, `:1171-1183`)
- **Chamado `desligamento_revisao`, para `dp_rh`:** registra desligamento, data, motivo e autoria; não enumera dias sem batida. (`app/colaboradores/services/desligamento.py:164-183`)
- **Pautas:** aceita destinatário DP e âncora pessoa/dia/competência; já existe entrega de outro tipo de dia sem pergunta ao DP. (`app/pautas/models.py:30-40`, `:76-89`; `app/chamados/services/fabrica_da_celula.py:268-284`)

Uma superfície **dedicada a dias sem batida de desligado como informativo rescisório** não foi encontrada [nao verificado]. Usar Pautas ou o chamado de revisão para isso seria alteração de comportamento, não descrição do que existe hoje [nao verificado].

**6. Quem escreve, quem lê e diferenças de significado**

**Escritores de `Colaborador.situacao` e `data_demissao`:**

| Escritor | Escrita |
|---|---|
| Modelo/admissão | Default ativo; admissão cria ativo pela porta. (`app/colaboradores/models.py:323`; `app/colaboradores/views.py:1185`, `:1208`; `app/colaboradores/services/colaborador.py:91-93`) |
| Agendamento de desligamento | Escreve data/motivo, mantendo situação até a efetivação. (`app/colaboradores/services/desligamento.py:87-102`) |
| Desligamento efetivo | Escreve situação desligado e data; salva pela operação situação. (`app/colaboradores/services/desligamento.py:109-113`) |
| Cron agendado | Seleciona demissão `<= hoje`, situação ainda diferente de desligado, e chama o mesmo executor. (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38-57`) |
| Reativação | Volta ativo e limpa demissão. (`app/colaboradores/services/desligamento.py:215-219`) |
| Lançamento INSS | Cria ausência aprovada e grava afastado. (`app/ponto/views.py:2420`, `:2433-2435`) |
| Reversor de afastado | Grava ativo quando o juiz não sustenta o afastamento. (`app/ponto/management/commands/reverter_situacao_afastado.py:73`; `app/ponto/services/afastado_avisa.py:126-129`) |

A porta genérica declara ambos os campos na operação `situacao` e executa atribuição/salvamento. O Django admin é somente leitura. (`app/colaboradores/services/colaborador.py:32`, `:55-76`; `app/colaboradores/admin.py:48`; `app/core/admin.py:59-74`)

**Leitores relevantes e semântica:**

| Leitor | Significado usado |
|---|---|
| Universo do fechamento | Situação **atual** combinada com demissão `>= início`; desligado sem data fica fora. (`app/ponto/services/fechamento.py:432-441`) |
| TXT/rescisão | Apenas data `<= fim`; ignora situação. (`app/folha/export.py:60`) |
| Retentor | Demissão limita fim **inclusivamente**. No dia da demissão, dispensa ausência de célula/veredito, mas não dispensa célula acusadora. (`app/folha/export.py:1208-1213`, `:1233-1264`) |
| Grade | Depois da demissão fica fora; no próprio dia, fica fora se não houver fato realizado/lavrado. (`app/escala/services/leitor_celula.py:365`, `:385-389`) |
| Suprajuiz | Sem data é tratado como nunca desligado; a exclusão temporal usa dia **estritamente posterior** à demissão. (`app/ponto/supra_juiz.py:154`, `:177`, `:194-197`) |
| Alimentadores do suprajuiz | Entregam `data_demissao`. (`app/ponto/services/cartorio.py:1145`; `app/escala/services/cadastro_realidade.py:511`; `app/ponto/management/commands/supra_juiz.py:178`; `app/ponto/management/commands/emitir_furo_retroativo.py:73`) |
| Prontidão e raio X | Recortam na demissão; janela colapsada recebe tratamento rescisório. (`app/folha/export.py:1068`, `:1087-1089`; `app/folha/views.py:612-615`) |
| Encerramento de vínculo | Grava `data_fim=data_demissao`; excedentes são células com data `> demissão`; vigência é fechada nas pontas. (`app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/vinculo.py:514`, `:855-864`; `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46`, `:56`) |
| Holerite | Demissão igual ao início ainda inclui a competência: exclui com `demissão < início`. (`app/holerite/matriz.py:102`) |
| Cobertura de posto | Exige ativo e demissão nula ou **estritamente posterior** ao fim. (`app/ponto/utils.py:170`, `:180`) |

**RetratoFolha — escritor e leitores diretos fechados:**

- **Escritor:** `lavrar_retrato_folha`, somente com `--apply`, grava `linhas` e `lavrado_em`; `montar_linhas` produz status/motivo/dias/datas. (`app/folha/managemen

[truncado em 20000 de 21101 caracteres]

## 13/11 11:28 · sessao 9cd45b1f4852 · - · - · origem=Ronald (tmux)

Censo 4/6, somente leitura. Varri AST dos 881 `.py` de `app/`, `mensageria/`, `bin/`, excluindo diretórios `tests/`, `migrations/` e arquivos `test_*`/`tests*`; zero falhas de parse. Cruzei `rg` nos demais arquivos e predicados por constante (`DESLIGADO`). Não há `.git` acessível para autenticar o hash [nao verificado].

**Não há função única que responda “desligado” para todos os domínios.** Há o campo atual, um apresentador que preserva esse campo e um predicado de rescisão baseado exclusivamente em data. O retentor não testa situação: recorta pela demissão e continua lendo células acusadoras. Fontes: `app/colaboradores/models.py:323`, `app/colaboradores/services/situacao_exibida.py:56`, `app/folha/export.py:60`, `app/folha/export.py:1208`, `app/folha/export.py:1241`.

**Predicados de desligado relevantes à retenção/cobrança e às superfícies:**

- Universo de apuração: `~Q(situacao='desligado') | (Q(situacao='desligado') & data_demissao>=início)`; aceita demissão posterior ao fim, exclui desligado sem data ou antes do início. `app/ponto/services/fechamento.py:432`, `app/ponto/services/fechamento.py:440`.
- Rescisão/TXT: `data_demissao is not None and data_demissao<=fim`; independe da situação. A função específica é `sai_pela_rescisao`. `app/folha/export.py:54`, `app/folha/export.py:60`.
- Trava por âncora só considera situação ativo. `app/folha/export.py:810`.
- Cartório exclui desligado da emissão de furo e de furo parcial, embora a célula possa acusar/reter. `app/ponto/services/cartorio.py:1748`, `app/ponto/services/cartorio.py:1813`.
- Vigia de hora pula desligado. `app/ponto/services/vigia_de_hora.py:117`.
- C5 “nunca bateu”: identifica desligados pelo campo atual, lê dias de `RetratoFolha` bloqueado/furo_espelho e conserva apenas esses pares. `app/escala/services/cadastro_realidade.py:186`, `app/escala/services/cadastro_realidade.py:191`, `app/escala/services/cadastro_realidade.py:195`, `app/escala/services/cadastro_realidade.py:198`.
- Vigia/contador da competência: desligado fora da C5 é barrado para RH. `app/chamados/management/commands/disparar_perguntas_competencia.py:59`.
- Censo de afastado com furo exclui desligado antes de unir juiz/campo afastado. `app/escala/services/cadastro_realidade.py:603`, `app/escala/services/cadastro_realidade.py:606`.
- Invariante I5a acusa cobrança viva de desligado. `app/chamados/management/commands/auditar_invariantes_chamados.py:653`.
- Adesão recusa desligado. `app/chamados/services/adesao.py:67`.
- Cobranças vetadas excluem desligado. `app/ponto/services/furos_vetados.py:62`.
- Reabertura automática exige situação ativo; logo também barra afastados/férias, não identifica exclusivamente desligado. `app/chamados/models.py:1006`.
- Reconciliador que emite C2 também exige ativo. `app/chamados/reconciliador.py:821`.
- Lista de evidências de vínculo dá precedência ao desligamento atual; carregamento exclui desligados. `app/ponto/services/vinculo_na_lista.py:71`, `app/ponto/services/vinculo_na_lista.py:152`.
- Contadores da tela antiga distinguem ativo/desligado/outros pelo campo atual. `app/ponto/views.py:1119`, `app/ponto/views.py:1120`, `app/ponto/views.py:1121`.

Outros locais de decisão explícita pelo campo: impedimento de novo vínculo `app/colaboradores/services/vinculo.py:91`; guarda do desligamento `app/colaboradores/services/desligamento.py:18`; guarda da reativação `app/colaboradores/views.py:1417`; cron agendado `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:40`; fechamento de vínculo legado `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:35`; tripwire `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:47`, `:48`; matriz de holerite, quando faltam ambas as datas, `app/holerite/matriz.py:97`; busca `app/core/busca.py:104`; indicadores `app/inteligencia/views.py:83`; relatórios `app/relatorios/views.py:833`, `:848`, `:857`; férias `app/ferias/views.py:605`; filtros/painéis de colaboradores `app/colaboradores/views.py:1046`, `:2850`, `:2900`, `:2916`. Comando de teste operacional fora de `tests/`: `app/ponto/management/commands/testar_pwa.py:150`.

Apresentação derivada: `DESLIGADO='desligado'`; `palavra` lê campo cru e faz desligado vencer afastamento. `app/colaboradores/services/situacao_exibida.py:33`, `:56`, `:57`. Templates que decidem esse ramo: `app/templates/colaboradores/partials/painel_colaborador.html:90`, `:163`; `app/templates/colaboradores/editar.html:12`, `:211`; `app/templates/colaboradores/partials/_lista_tabela.html:38`. API usa gate mais amplo “situação não é ativo/afastado” e mapa desligado: `app/api/views.py:403`, `:405`; clone `app/api/views_core.py:389`, `:391`. Login/token exige ativo: `app/api/login_helpers.py:141`, `app/api/views.py:116`.

**Afastado/INSS não é desligado.** O campo admite `afastado`, `ferias`, `ativo`, `desligado`. O juiz temporal consulta `Ausencia.tipo='afastamento_inss'`, com cobertura por dia; `afastado_hoje` devolve ausência ou `None`. Para folha exige aprovada; para cobrança aceita não rejeitada. `app/colaboradores/models.py:295`, `app/ponto/turnos.py:2704`, `:2715`, `:2718`, `:2725`, `:2731`, `:2516`, `:2518`. Badge/filtro usam efeito cobrança; desligado vence. `app/colaboradores/services/situacao_exibida.py:31`, `:46`, `:57`, `:63`. Há ainda leitura de divergência do campo cru: ausência não cobre hoje mas `situacao='afastado'` gera `situacao_sem_ausencia`. `app/ponto/services/afastado_avisa.py:39`, `:41`.

**Escritores de `situacao` / `data_demissao`:**

| Escritor | Escrita |
|---|---|
| Modelo/admissão | Default ativo em `app/colaboradores/models.py:323`; admissão passa ativo a `criar_colaborador` em `app/colaboradores/views.py:1185`, `:1208`; porta cria em `app/colaboradores/services/colaborador.py:91`. |
| Agendamento | Grava só data/motivo, conserva situação e acesso até a data. `app/colaboradores/services/desligamento.py:87`, `:90`, `:91`. |
| Desligamento efetivo | Atribui desligado/data, salva inteiro pela operação situação. `app/colaboradores/services/desligamento.py:109`, `:110`, `:113`. |
| Cron agendado | Seleciona data<=hoje e situação diferente de desligado; chama o mesmo juiz/executor. `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `:40`, `:51`, `:57`. |
| Reativação | Volta ativo e limpa demissão. `app/colaboradores/services/desligamento.py:215`, `:216`, `:219`. |
| Lançamento INSS | Após criar ausência aprovada, grava afastado. `app/ponto/views.py:2420`, `:2433`, `:2435`. |
| Reversor afastado | Grava ativo quando ambos os efeitos do juiz não cobrem hoje. `app/ponto/management/commands/reverter_situacao_afastado.py:73`; decisão `app/ponto/services/afastado_avisa.py:126`, `:127`, `:129`. |

Porta genérica atribui/salva em `app/colaboradores/services/colaborador.py:55`, `:60`, `:61`; operação declara os campos em `:32`; `salvar_inteiro` salva o objeto inteiro em `:73`, `:76`. Edição de cadastro salva inteiro, mas não atribui situação/demissão no seu formulário: `app/colaboradores/views.py:1314`, `:1345`. Admin herda `SoLeitura`: `app/colaboradores/admin.py:48`; bloqueia add/change/delete e torna todos os campos readonly: `app/core/admin.py:59`, `:62`, `:65`, `:74`.

**Leitores de data relevantes e semânticas divergentes:**

- Retenção: fim recortado na demissão, laço inclusivo `d<=fim`; no próprio dia da demissão dispensa célula ausente/sem veredito, mas célula acusadora continua retendo. `app/folha/export.py:1208`, `:1233`, `:1237`, `:1239`, `:1241`, `:1261`, `:1263`.
- Grade: data posterior à demissão sempre fora; próprio dia da demissão fora apenas sem fato realizado/lavrado. `app/escala/services/leitor_celula.py:365`, `:385`, `:386`, `:389`.
- Suprajuiz: `None` é tratado como nunca desligado; só dia estritamente posterior à demissão deixa de acusar cadastro por trabalho sem vínculo. `app/ponto/supra_juiz.py:154`, `:177`, `:194`, `:197`. Alimentadores: `app/ponto/services/cartorio.py:1145`, `app/escala/services/cadastro_realidade.py:511`, `app/ponto/management/commands/supra_juiz.py:178`, `app/ponto/management/commands/emitir_furo_retroativo.py:73`.
- Raio-X da folha recorta na demissão inclusivamente; demissão anterior à janela pula colaborador. `app/folha/views.py:612`, `:613`, `:615`.
- Prontidão fallback recorta fim na demissão; janela colapsada vira rescisão. `app/folha/export.py:1087`, `:1088`, `:1089`.
- Prontidão materializada usa demissão anterior ao início como motivo rescisão. `app/folha/export.py:1068`.
- Fechamento de vínculo usa `data_fim=data_demissao`; contador de células excedentes usa `data__gt=demissão`. `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:46`, `:56`; vínculo fecha pelas portas `app/colaboradores/services/desligamento.py:128`, `app/colaboradores/services/vinculo.py:855`, `:864`. Vigência declarada fechada nas duas pontas: `app/colaboradores/services/vinculo.py:514`.
- Holerite inclui competência se demissão coincide com início (`dem<ini` é que exclui). `app/holerite/matriz.py:102`.
- Cobertura de posto exige ativo e demissão nula ou **estritamente posterior** ao fim. `app/ponto/utils.py:170`, `:180`.

Portanto, não supõem todos o mesmo significado: situação é corrente no cadastro/emissão, universo usa situação corrente combinada com início da competência, TXT usa só data contra fim, e próprio dia da demissão tem tratamento diferente na grade e no retentor. Fontes contrastáveis acima: `app/ponto/services/fechamento.py:440`, `app/folha/export.py:60`, `app/escala/services/leitor_celula.py:389`, `app/folha/export.py:1237`.

**Superfície existente para DP/rescisão:** o desligamento cria chamado de auditoria `dp_rh`, módulo `desligamento_revisao`, com data/motivo/autor; não enumera dias sem batida. Pode ser superfície próxima, mas acomodar o informativo pedido é proposta, não comportamento atual. `app/colaboradores/services/desligamento.py:164`, `:168`, `:178`, `:181`, `:183`. A existência de outra superfície dedicada a dias sem batida para rescisão não foi encontrada na varredura [nao verificado].

## 13/11 11:28 · sessao eae6cc30235b · - · - · origem=Ronald (tmux)

Censo deste recorte fechado por `rg` em produção e AST Python (excluindo componentes `tests`, `migrations`, ambientes e `.git`), resolvendo `from ... import RetratoFolha as apelido`; os usos AST de `RetratoFolha` foram exatamente os abaixo. As referências seguintes são à cópia local; números reais de banco/log permanecem [nao verificado].

**RetratoFolha — escritor e todos os leitores diretos**

| Sítio | O que faz |
|---|---|
| `app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74-76` | Único escritor encontrado: monta linhas, depois `update_or_create(empresa,mes,ano, defaults={linhas,lavrado_em})`, somente `--apply`. `:68` instancia retrato sem salvar para imprimir contadores. |
| `app/folha/retrato_folha.py:1156-1188` | Produz `linhas`: chama `fora_da_folha(...,com_itens=True)`; grava status/motivo, `dias`, `datas`, dono/tipo/saldo. Datas de furo vêm de `colabs`; férias com batida usam segunda fonte do item (`:1171-1176`). |
| `app/folha/views_retrato.py:190-196` | Lê retratos por competência/empresas e passa pelo acessor `linhas_do_retrato`. Mesma função de recorte alimenta tela (`:254`), PDF (`:382`) e raio X (`:482`). |
| `app/colaboradores/views.py:977-989` | Drawer: lê o retrato da competência da URL, busca linha, monta `o_que_fazer` e portas. **Não chama `o_que_segura`**. Template mostra as datas gravadas em `app/templates/folha/_retrato_o_que_fazer.html:43-46`; incluído por `app/templates/colaboradores/partials/painel_colaborador.html:26`. |
| `app/folha/vigia_retido_calado.py:33-55` | Lê todos os retratos da última data de lavra por empresa; pelo acessor filtra bloqueados e fio `ninguem`; desconta C5 como roteados. |
| `app/escala/services/cadastro_realidade.py:185-200` | C5: identifica desligados atuais, lê linhas bloqueadas do retrato e mantém somente dias nas `datas` de `motivo=furo_espelho`. É leitura da lavra, não juiz vivo. |
| `app/folha/retrato_folha.py:1296-1309` | Frescor: lê apenas existência/`lavrado_em`, compara ao fechamento mais novo; não julga retenção. |

A porta comum de leitura está em `app/folha/retrato_folha.py:98-111`. O balde local `_balde` lê `status='entra'`, depois `motivo in FORA_DA_CONTA`, senão **bloqueado** (`:89-95`); `FORA_DA_CONTA=('rescisao_modulo_proprio',)` em `:33`. Isso interpreta resultado gravado do juiz, não reconstitui retenção por célula. Contadores são `len` da porta (`:114-121`); recorte conta datas ISO inclusivamente (`:1237-1245`).

**Superfícies**

- **Supervisor / Quem cobrar:** `app/relatorios/views.py:1641-1642` obtém ranking e universo; `app/escala/services/furos_diarios.py:259`, `:273-275` limita a `situacao='ativo'`, eliminando desligados. A coluna Folha lê `classificar_export` em `app/relatorios/views.py:1658`, traduz `furo_espelho` em `:1660` e projeta apto/retido em `:1663-1669`; PDF usa a mesma coluna em `:1672-1677`. Essa lista não deriva retenção por veredito.
- **Lista administrativa Cadastro x Realidade/C5:** `app/escala/services/cadastro_realidade.py:181-200` começa em dias `veredito='nunca_bateu'`; ativo segue sem perguntar ao retrato; desligado só fica se constar no retrato bloqueado/furo. Formata assinatura C5 em `:204-209`, incorpora em `lista` em `:876-879`; tela/PDF em `app/escala/views.py:487-492`.
- **Retido calado:** `app/folha/vigia_retido_calado.py:46-55` conta bloqueados/fio ninguém menos C5; imprime `retidos_calados` em `:66`. Não deriva o dia retido.
- **Vigia fio/alcance:** `app/chamados/services/vigia_fio.py:563`, `:582-588` usa `dias_que_seguram` por empresa/competência aberta, deduplica pares colab/data; depois pergunta pelo alcance da porta (`:600-618`) e classifica sem porta (`:667-699`). Não reconstitui retenção por `VEREDITOS_ACUSAM`.
- **E-SEM-VIVA-B:** nome de teste/comentário, não nome de contador de produção encontrado. O contador vivo é `nunca_bateu_sem_adesao`: `app/chamados/management/commands/disparar_perguntas_competencia.py:39-72` lê C5, barra desligado que não está nela (`:59-61`), compara interseção de pares. O vigia imprime em `app/chamados/management/commands/vigiar_fio_morto.py:210-213`, grava número/esperado/C5 em `:233-243`. Pipeline lê o mesmo carimbo em `app/core/management/commands/pipeline_placar.py:85-106`, `:135-136`.
- **ESTADO:** `bin/gerar_estado.py:260-261` lê `fio_estado`; manchete usa `fio_morto_alcance` e esperado do carimbo (`bin/fio_estado.py:148-154`, `:175-183`). A lista genérica de fora do esperado lê o carimbo inteiro (`:191-193`), portanto pode expor discrepância `nunca_bateu_sem_adesao`. Não encontrei implementação de `desligados_retidos` via buscas textuais em produção; existência fora da cópia [nao verificado].
- **Tela Folha:** `app/folha/views.py:589-600` particiona `classificar_export`; a grade de furo chama `furos_por_dia` após clamp inclusivo na demissão (`:605-616`). Isso dá detalhe de furos, não nova decisão de quem está retido. PDF antigo lê o juiz vivo (`:858-859`); `retidos_pdf.agrupar_retidos` conta **todo** `status='fora'`, incluindo rescisão (`app/folha/retidos_pdf.py:32-38`), diferentemente do retrato que exclui rescisão do balde bloqueado.
- **TXT / aviso:** após gerar TXT, a view registra quantidade de retidos (`app/folha/views.py:289-293`) e emite toast por cookie (`:302-305`).
- **Mensageria:** fallback vivo lê `classificar_export` e conta qualquer `status='fora'` como retido (`app/api/views_mensageria.py:283-301`); portanto também mistura rescisão com bloqueio na terminologia da saída.

**Acusação/chamados não é retenção TXT**

- `app/chamados/services/fabrica_da_celula.py:105-113` cala `nunca_bateu` e situação diferente de ativo; `:120-121` usa `VEREDITOS_ACUSAM` para decidir nascimento de pergunta.
- `app/chamados/services/disputa_emissao.py:757-773` usa nunca_bateu/acusação para alcance da fábrica; não afirma retenção de folha.
- `app/chamados/juizes.py:1375` trata coerência da resposta; `:1448-1451` usa acusação para lastro/vitalidade da pergunta.
- `app/chamados/models.py:3357-3359` constrói predicados de célula acusa/absolve para vitalidade.
- `app/relatorios/pendencias.py:135-158` lista furos por `fur`/`orf` do ranking, não por retenção; o universo vem apenas dos ativos (`app/relatorios/views_pend.py:23-26`, `app/escala/services/furos_diarios.py:259`).
  
Esses sítios decidem cobrança/pergunta/pendência e precisam ser distinguidos de um derivador próprio do retido da folha.

**Informativo DP/rescisão existente**

- Existe balde por **pessoa**, `modulo_proprio`, com ajuda “Rescisão na janela” (`app/folha/retrato_folha.py:33`, `:41`, `:89-95`), e etiquetas de rescisão na tela antiga (`app/templates/ponto/_fech_linha.html:4`); não entrega lista de dias sem batida.
- Existe infraestrutura genérica **Pautas** para DP com âncora dia/colaborador/competência (`app/pautas/models.py:30-40`, `:76-89`); escritor aceita departamento e âncora (`app/pautas/services.py:153-185`). Exemplo vigente de dia sem pergunta entregue ao DP: limbo em competência exportada (`app/chamados/services/fabrica_da_celula.py:268-284`).
- C5 é hoje lista de ação/cadastro vinculada a retenção para desligado (`app/escala/services/cadastro_realidade.py:195-200`), não o informativo rescisório pedido.
- Não encontrei superfície dedicada que entregue **dias sem batida de desligado como informativo rescisório** na cópia. Pautas é uma superfície já existente tecnicamente apta a receber um dia destinado ao DP; decisão de produto de usá-la [nao verificado].
- O comentário de `app/folha/retrato_folha.py:1219` diz que o drawer lê `o_que_segura`, mas o código efetivo lê somente o retrato (`app/colaboradores/views.py:979-987`); não repetir o comentário como fato.

## 13/11 11:28 · sessao 62d12c0494c2 · - · - · origem=Ronald (tmux)

Censo direto FECHADO: AST de todos os `.py` da árvore, excluindo diretórios `tests/` e `migrations/`: 939 arquivos, zero erros de parse; cruzamento com `rg` global dos três nomes, imports e chamadas por atributo. Resultado: oito sítios de chamada. A contagem/método vêm da execução, sem artefato salvo [nao verificado].

| Função chamada | Chamador de produção | Uso do retorno |
|---|---|---|
| `motivos_retencao_celula` | `app/folha/export.py:1275` | Compatibilidade `motivos_retencao_espelho`: retorna os motivos do juiz; ignora os três parâmetros antigos (`:1270`). |
| idem | `app/folha/fora_da_folha.py:98`, import `:97` | `_dias_que_retem` repassa `(motivos,dias)`. |
| idem | `app/folha/previa.py:170`, import `:169` | Compara conjunto de dias retidos com dias propostos (`:171`–`:173`); controla “passaria a entrar” (`:119`–`:121`) e contador `mudariam_de_lado` (`:133`). |
| idem | `app/folha/services/vigia_pausa.py:125`, import `:48` | Motivos/dias alimentam `barrados`, `dias_retidos_que_sao_pausa`, `barrados_so_por_pausa`, `livres_com_dia_so_pausa` (`:126`–`:136`). |
| `motivos_retencao_espelho` | `app/folha/export.py:968` | Retorno verdadeiro determina `fora/furo_espelho`, `motivos_espelho`, detalhe e `continue` (`:969`–`:974`). |
| idem | `app/ponto/services/fechamento.py:1067`, import **`as _mre`** `:1066` | Materializa `fechamento.motivos_espelho`; persiste esse campo em `:1186`. |
| `_dias_que_retem` | `app/folha/fora_da_folha.py:137` | Guarda por colaborador, exclusivamente dos itens que `classificar_export` classificou `furo_espelho` (`:119`–`:138`). |
| idem | `app/folha/fora_da_folha.py:217` | `o_que_segura`: monta datas, veredito, motivo, fio, destrava e chamado (`:218`–`:232`), juntamente com status/motivo do juiz do export (`:215`). |

**Cadeias indiretas e sítios de projeção da retenção:**

- `dias_que_seguram` lê `classificar_export` e seleciona motivo `furo_espelho` (`app/folha/fora_da_folha.py:119`, `:128`). `fora_da_folha` usa esse universo (`:161`), conta grupos/fios/dias e publica `datas` (`:171`–`:197`). Não deriva retenção da célula.
- Retrato: `app/folha/retrato_folha.py:1157` lê `fora_da_folha(com_itens=True)`; grava status/motivo/dias/datas/dono/tipo/saldo nas linhas montadas (`:1162`–`:1188`). Leitura do juiz.
- **C5 de desligado:** `app/escala/services/cadastro_realidade.py:185` seleciona desligados; `:191`–`:196` lê `RetratoFolha` pela porta `linhas_do_retrato(r,'bloqueado')`, motivo `furo_espelho`, acumulando `datas`; `:198` exige o dia nessa lavra. Leitura do retrato, não nova derivação de retenção.
- **Vigia retido calado:** `app/folha/vigia_retido_calado.py:46` lê linhas bloqueadas do retrato, `:47` fio `ninguem`, `:48`–`:53` desconta roteados à C5 e conta calados. Leitura do retrato.
- **Vigia fio — alcance:** `app/chamados/services/vigia_fio.py:585` lê `dias_que_seguram`; `:586`–`:589` deduplica e conta dias retidos. Leitura do juiz.
- **API/métrica:** `app/api/views_mensageria.py:257`–`:275` lê `MetricaSnapshot`, inclusive `retidos=denominador-numerador` (`:272`); sem snapshot, pergunta `classificar_export` (`:283`) e conta status/motivos (`:285`–`:301`). A métrica vem do juiz em `app/inteligencia/calculadores.py:210`–`:234` e `:271`–`:282`.
- **Mensageria:** `mensageria/nucleo/ferramentas.py:1247`–`:1268` separa `rescisao_modulo_proprio` como ressalva (`NAO_E_BLOQUEIO`, `:1196`) e soma os outros motivos como “travam de fato”. Projeção da métrica, não leitura de célula.
- **Tela antiga/painel/raio-X:** `app/folha/views.py:259`–`:260` conta furo para aviso; `:331`–`:341` agrupa fora por motivo; `:589`–`:601` separa furo, férias, rescisão e outros. Leitura de `classificar_export`.
- **PDF retidos:** `app/folha/views.py:858`–`:859` pergunta o juiz; `app/folha/retidos_pdf.py:32`–`:38` agrupa status/motivo. `:90` chama `len(motivos_espelho)` de dias; o painel também (`app/folha/views.py:398`). Isso conta **motivos distintos**, não datas retidas, pois o juiz devolve `sorted(motivos)` (`app/folha/export.py:1266`).
- **Coluna folha do “quem cobrar”:** `app/relatorios/views.py:1658` lê juiz, `:1664`–`:1669` projeta status/motivo. **Pendências cadastro:** `app/relatorios/views_pend.py:58`–`:69` lê somente portas de cadastro do juiz.
- **Simulação:** `app/folha/management/commands/simular_folha.py:74` seleciona os não entrantes dos itens do juiz; `:90` registra motivo/horas/prontidão. **Vigia TXT:** `app/folha/services/vigia_txt.py:231` pergunta juiz, `:232` monta TXT com os mesmos itens; `:113`–`:117` leva status/motivo ao diff.
- **TXT:** `app/folha/export.py:714` pergunta juiz, `:716` monta `entra_set`, `:726` exclui os demais. **Validação PDF:** `app/folha/validacao_pdf.py:65`–`:66` usa o mesmo resultado. **Porta de conferência:** `app/folha/porta_export.py:469`–`:475` limita universo aos entrantes. **Dias em aberto:** `app/ponto/services/dias_em_aberto.py:104`–`:108` faz o mesmo.

**Regras paralelas sobre célula/chamado/pergunta: não são chamadas do juiz de retenção. Não confundir “célula acusa” com “retém folha”:**

- `app/chamados/services/vigia_fio.py:331`, `:378`–`:380`: o `censo` antigo conta lâmpadas acusadoras dos chamados vivos; difere do `censo_alcance` que lê retenção (`:585`).
- `app/chamados/services/furo_sem_canal.py:35`, `:70`–`:86`: universo próprio de vereditos `furo/cobrado` sem pergunta aberta nem chamado vivo; não pergunta retenção.
- `app/ponto/services/furos_sem_cobranca.py:61`–`:63`: filtra `furo`, protestos e colaborador `ativo`; `:89`–`:107` separa afastado/vetos/pergunta/supervisão. Pergunta sobre emissão, não retenção.
- `app/chamados/services/fabrica_da_celula.py:112` exclui situação diferente de `ativo`; `:120` lê `VEREDITOS_ACUSAM`; `:123`–`:125` pode mandar apenas à lista do admin.
- `app/chamados/services/disputa_emissao.py:772` usa `VEREDITOS_ACUSAM` para decidir se chamado cuja pergunta foi encerrada ainda deve entrar na fábrica.
- `app/chamados/juizes.py:1375` usa veredito na concordância da resposta; `:1450`–`:1451` pode conservar pergunta viva por célula acusadora. Não consulta retenção.
- Outros leitores de `VEREDITOS_ACUSAM`: `app/chamados/models.py:3358`–`:3359`, `app/chamados/reconciliador.py:642`, `:1028`, `app/chamados/services/limbo_folgas.py:195`, `app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/silencio_ausencia.py:42`; são decisões de fila/vida/resposta/reconciliação, não novo `furo_espelho`.
- Vigias/auditorias de célula também não são retenção: `app/ponto/services/vigia_ausencia_veredito.py:41`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:36`, `app/relatorios/dia_da_frota.py:55`, `app/chamados/management/commands/auditar_invariantes_chamados.py:198`, `:343`, `:1391`, `:1605`, `:1647`.

Não encontrei outro escritor literal de `furo_espelho` no código de produção além do ramo de `classificar_export` (`app/folha/export.py:970`); os demais sítios encontrados são leitura, agrupamento, tradução ou comentário. A inexistência foi verificada por AST/`rg`, sem artefato salvo [nao verificado].

## 13/11 11:27 · sessao 6daf6a687306 · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

**A. Os três itens PROCEDE**

| Item | O delta cura? | O que ficou de fora |
|---|---|---|
| **1 — Âncora do mutante** | **Sim.** A ocorrência única está em `app/colaboradores/services/vinculo.py:975`; o teste exige uma ocorrência, remove a flag e cobra o achado em `criar_trecho_retroativo`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:209` | O mutante chega a ser exercitado: reproduzi os corpos dos testes com armazenamento virtual em memória. Execução pelo runner Django original: **[nao verificado]**. |
| **2 — Formas invisíveis** | **Parcialmente.** Cadeias sobre gerente e a seleção de `create_defaults` passaram a ser reconhecidas. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:97`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110` | Permanecem formas invisíveis **não declaradas**, incluindo nascimento pelo gerente reverso e atualização por `update_or_create`; detalhadas em B. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:24`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:136` |
| **3 — Relógio na porta** | **Sim para a leitura direta apontada.** `hoje` tornou-se keyword obrigatória; a porta não captura mais o dia. Os dois comandos o capturam uma vez. `app/colaboradores/services/vinculo.py:869`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:48` | **Não cura a cadeia inteira.** Há nova captura decisória de dia dentro da célula, além dos carimbos transitivos. `app/ponto/portas/celula.py:464`, `app/ponto/portas/celula.py:308`, `app/ponto/services/fechamento.py:1613` |

A redução da catraca está coerente: executei seus helpers AST em memória; censo, soma dos tetos e total resultam em **43**, com igualdade exata. Isso verifica ocorrências estáticas, não quantidade de leituras por execução. `app/ponto/tests/test_cadastro_futuro_c5e.py:374`, `app/ponto/tests/test_cadastro_futuro_c5e.py:574`, `app/ponto/tests/test_cadastro_futuro_c5e.py:591`

**B. Limites que o selo ainda não declara**

Os seguintes contraexemplos produziram **zero achados** na execução em memória do scanner. Sua ocorrência efetiva como caminhos de produção nesta cópia é **[nao verificado]**; são lacunas demonstradas da proteção, não escritores atuais comprovados. O reconhecimento relevante está em `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:124`.

1. **`update_or_create` escrevendo a flag de vínculo existente.** Este formato passa:

   ```python
   EC.objects.update_or_create(
       pk=1,
       defaults={"trabalha_em_feriado": True},
       create_defaults={"trabalha_em_feriado": origem.trabalha_em_feriado},
   )
   ```

   O selo verifica o nascimento usando `create_defaults`, mas não examina a escrita de `defaults` no existente. Os ramos de fiscalização de escrita reconhecem `update`, `bulk_update`, `setattr` e atribuição; não essa atualização. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:139`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:147`

2. **Sobreposição do valor encontrado por `_kw`.** Passa uma keyword de busca com cópia válida acompanhada de `defaults` com literal `True`: a keyword direta vence na análise. Também passam dicionários cuja primeira chave válida é sobrescrita por chave repetida ou `**kw`: o scanner retorna a primeira ocorrência literal. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:112`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:116`

3. **Nascimento por gerente reverso**, como `colaborador.escalas.create(...)`. Essa relação existe, mas o reconhecimento exige gerente acessado pelo modelo identificado. `app/escala/models.py:872`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:43`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:97`

4. **Modelo ou criador resolvido indiretamente:** alias anotado (`EC: type = EscalaColaborador`), função ligada (`criar = EC.objects.create`), modelo recebido por parâmetro ou obtido por `apps.get_model`. O rastreamento acompanha imports e atribuições simples de modelos já reconhecidos; não essas formas. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:57`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:68`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:76`

5. **Nomes das APIs assíncronas:** `acreate`, `aget_or_create`, `aupdate_or_create`, `abulk_create`, `aupdate` e `abulk_update` não entram nas listas e comparações do scanner. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:42`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:136`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:147`

6. **Escritas com outra forma de AST:** atribuição desempacotada (`ec.trabalha_em_feriado, ec.ativa = ...`), alteração de `ec.__dict__`, `setattr` com nome do campo em variável e `bulk_update` com lista de campos em variável. A fiscalização exige atributo diretamente no alvo ou nome literal do campo. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:149`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:153`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:156`

Essas lacunas são adicionais às expressamente declaradas — `.update(**kw)`, cópia por `pk=None`, gerente por variável e aceitação do juiz/cópia pelo nome. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:24`

Os casos novos mordem o seguinte:

| Caso | Resultado efetivamente cobrado |
|---|---|
| **n** | Positivo: cadeia `objects.all().create` sem flag. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:235`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:254` |
| **o** | Positivo: `_base_manager.filter().get_or_create` sem flag. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:236`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:255` |
| **p** | Positivo: flag somente em `defaults`, ausente em `create_defaults`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:237`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:256` |
| **q** | Controle negativo: aceita a flag em `create_defaults`; não exercita atualização indevida da flag no existente. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:239`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:257` |
| **r** | Positivo conservador: acusa falta de keyword literal, mesmo que `kw` contenha a flag correta. Isso está declarado. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:25`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:241` |
| **s** | Controle negativo para receptor não reconhecido; não demonstra que um parâmetro nunca receberá o próprio modelo. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:242`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:257` |

São provas sobre **strings analisadas por AST**, sem execução dessas operações no ORM. Os seis corpos originais do selo passaram na reprodução com I/O virtual; suíte Django e comportamento ORM: **[nao verificado]**. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:219`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:243`

**C. Todos os chamadores de `marcar_trabalha_em_feriado`**

O censo encontrou **16 pontos de chamada: dois de produção e 14 de teste**, todos passando `hoje`. A assinatura exige o argumento sem default. `app/colaboradores/services/vinculo.py:869`

| Chamador | Origem de `hoje` e leituras diretas para capturá-lo |
|---|---|
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:58` | `localdate()` em `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56`: **uma por apply**, zero no DRY. |
| `app/escala/management/commands/aplicar_feriado_lei_1609.py:73` | `localdate()` em `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`: **uma por execução**, inclusive DRY; reutilizada antes/depois. |
| `app/chamados/tests/test_jv1_feriado_vira_vigia.py:89` | **Uma por `_marcar`**, sob freeze definido em `app/chamados/tests/test_jv1_feriado_vira_vigia.py:38`. |
| `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:133` | **Uma por `_marcar`**, sob freeze definido em `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:124`. |
| `app/escala/tests/test_porta_escala.py:62` e `app/escala/tests/test_porta_escala.py:65` | Data literal `2026-11-13`: **zero**. |
| `app/ponto/tests/test_cadastro_futuro_c3.py:114` | `HOJE` importado da base: **zero**. `app/ponto/tests/test_cadastro_futuro_c3.py:15` |
| `app/ponto/tests/test_cadastro_futuro_c5b.py:124`, `app/ponto/tests/test_cadastro_futuro_c5b.py:138`, `app/ponto/tests/test_cadastro_futuro_c5b.py:144` | `HOJE` importado da base: **zero**. `app/ponto/tests/test_cadastro_futuro_c5b.py:25` |
| `app/ponto/tests/test_cadastro_futuro_sem_banco.py:446`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:469`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:476`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:481`, alias `evento` em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:662` e `app/ponto/tests/test_cadastro_futuro_sem_banco.py:664` | Literal `HOJE = 2026-11-12`: **zero**. `app/ponto/tests/test_cadastro_futuro_sem_banco.py:20` |

Os chamadores indiretos pelos comandos também usam essas bordas: dois `call_command` do sincronizador; um DRY e o helper do comando novo. `app/colaboradores/tests/test_feriado_admin_sync.py:41`, `app/colaboradores/tests/test_feriado_admin_sync.py:45`, `app/escala/tests/test_feriado_lei_1609.py:102`, `app/escala/tests/test_feriado_lei_1609.py:140`

**Uma captura de `hoje` na borda não significa uma leitura total por ato:**

- Cada vínculo alterado salva a flag e dispara uma leitura `now()` no invalidador, mesmo sem fechamento correspondente para atualizar. `app/colaboradores/services/vinculo.py:894`, `app/escala/signals.py:146`, `app/ponto/services/fechamento.py:1613`
- Cada célula reescrita recebe carimbo próprio. `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:308`
- Cada chamada de regeneração com dias tocados pode reler **o dia decisório** em `_o_chao_do_dia_mudou`; futuras chamam regeneração por dia. `app/ponto/portas/celula.py:460`, `app/ponto/portas/celula.py:464`, `app/ponto/services/cadastro_futuro.py:43`
- O fechamento agendado após commit captura outro instante para o teto do fato e outros para seus carimbos. `app/ponto/services/fechamento.py:616`, `app/ponto/services/fechamento.py:1086`, `app/ponto/services/fechamento.py:1143`, `app/ponto/services/fechamento.py:1824`

O total executado depende de vínculos, células, competências e callbacks: **[nao verificado]**. O no-op retorna antes desses efeitos. `app/colaboradores/services/vinculo.py:890`

**D. Quem escreve, quem lê e significado**

| Valor | Escritores/produtores | Leitores e significado |
|---|---|---|
| **`trabalha_em_feriado`** | Default ainda `True`; nascimento novo pergunta ao juiz; três nascimentos copiam origem; alteração direta fica na porta. `app/escala/models.py:896`, `app/colaboradores/services/vinculo.py:296`, `app/colaboradores/services/vinculo.py:975`, `app/colaboradores/services/vinculo.py:1093`, `app/colaboradores/services/vinculo.py:1114`, `app/colaboradores/services/vinculo.py:893` | Lida no no-op/trilha, seleção/reversão do lote, sincronizador, contador antigo e predicado do efeito. `app/colaboradores/services/vinculo.py:890`, `app/escala/models.py:961`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:70`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:48`, `app/escala/models.py:929`, `app/escala/models.py:1427` |
| **`hoje` / janela** | Produzido nas duas bordas por `localdate`; configuração usa São Paulo e timestamps com fuso. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:56`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95` | O mesmo `hoje` alimenta universo, janela anterior, regeneração até hoje e seleção de futuras. `app/escala/models.py:949`, `app/colaboradores/services/vinculo.py:900`, `app/colaboradores/services/vinculo.py:906`, `app/ponto/services/cadastro_futuro.py:26` |
| **`FechamentoMensal.previsto_em`** | Recalculo preenche, falha limpa, persistência salva/atualiza; invalidador também limpa. `app/ponto/services/fechamento.py:1086`, `app/ponto/services/fechamento.py:1099`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1188`, `app/ponto/services/fechamento.py:1613` | `prontidao` decide entre snapshot e cálculo vivo pela presença do carimbo; fotografia e hashes o tratam como controle mutável. `app/folha/export.py:1045`, `app/folha/export.py:1081`, `app/chamados/services/foto_e4b.py:15`, `app/ponto/portas/regen_exportada.py:37` |

Quanto à compatibilidade dos significados:

- **Flag:** o posto vence no nascimento em qualquer ciclo; o efeito de virar folga exige flag `False`, ciclo elegível e feriado do lugar do vínculo. Gerador, precedência e separação da rubrica perguntam ao mesmo predicado. Portanto, `False` em 12x36 sem transformar o plantão em folga é expressamente previsto. `app/escala/models.py:911`, `app/escala/models.py:1427`, `app/escala/models.py:1406`, `app/ponto/precedencia.py:377`, `app/ponto/services/fechamento.py:949`
- **Intervalos:** vigência inclui início e fim; apuração vai do corte ao corte−1; regeneração inclui `até`; futuras começam em `hoje+1`. Não encontrei conflito inclusivo/exclusivo nesses limites. `app/escala/alimentacao.py:214`, `app/ponto/janelas.py:10`, `app/ponto/portas/celula.py:228`, `app/ponto/services/cadastro_futuro.py:27`
- **Horizonte:** passado usa a janela anterior da empresa do vínculo; futuras usam a união das competências correntes das empresas ativas. São alcances distintos, expressos no código. `app/colaboradores/services/vinculo.py:900`, `app/ponto/management/commands/gerar_celulas.py:299`
- **Dia do ato:** há divergência possível à meia-noite: universo/regeneração usam a captura da borda, enquanto julgamento compara com outro `localdate`. Ocorrência real: **[nao verificado]**. `app/colaboradores/services/vinculo.py:906`, `app/ponto/portas/celula.py:464`, `app/ponto/portas/celula.py:469`
- **Dia civil versus turno:** a regeneração trabalha pela data da célula; a separação financeira citada usa a data local da entrada do período. Equivalência quando a entrada atravessa a data prevista do turno: **[nao verificado]**. `app/ponto/portas/celula.py:289`, `app/ponto/services/fechamento.py:947`
- **Fuso:** o contador antigo ainda tem fallback `date.today()`, diferente da captura local explícita do novo lote; fuso efetivo do sistema operacional: **[nao verificado]**. `app/escala/models.py:926`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`
- **`previsto_em`:** significa frescor do previsto, separado de `derivado_em`, que representa derivação do registro inteiro. Não há fundamento para tratá-lo como garantia de frescor de todo o fechamento. `app/ponto/models.py:453`, `app/ponto/models.py:454`

**E. Classes de reprovação**

| Classe | Constatação |
|---|---|
| **Segundo juiz / derivação paralela** | O contador novo consulta o juiz existente; não duplica sua decisão. Já o sincronizador preserva decisão concorrente por cargo, capaz de contrariar posto/ciclo. `app/escala/models.py:960`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`, `app/escala/models.py:911` |
| **Segundo escritor fora da porta** | O selo permite variantes de `update_or_create` que escrevem a flag fora da porta; ocorrência real dessas variantes: **[nao verificado]**. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:139`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:147` |
| **Relógio fora da borda** | Continua presente e decisório, além dos carimbos. `app/ponto/portas/celula.py:464`, `app/ponto/portas/celula.py:469` |
| **Selo/RED sem mordida** | O mutante reparado e os positivos n/o/p/r têm mordida AST. A proteção geral continua incompleta pelos caminhos de B. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:209`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:254` |
| **Ordem de trava** | A sequência demonstrável é transação → vínculo → invalidação dos fechamentos → células/trava das futuras. A guarda de exportada é consulta, não trava de competência. Violação concreta de ordem global/deadlock: **[nao verificado]**. `app/colaboradores/services/vinculo.py:868`, `app/colaboradores/services/vinculo.py:894`, `app/ponto/services/fechamento.py:1613`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/portas/celula.py:221` |
| **Pouso antes de pré-requisito** | O comando confere universo e cria reversão exclusiva antes de chamar a porta. Publicação prévia da lista, execução do lote e pouso efetivo: **[nao verificado]**. `app/escala/management/commands/aplicar_feriado_lei_1609.py:61`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:69` |
| **Campo com dois significados** | Não demonstrei contradição intrínseca na flag ou em `previsto_em`; há bases temporais distintas e releitura de dia, conforme D. `app/escala/models.py:1427`, `app/ponto/models.py:454`, `app/ponto/portas/celula.py:464` |

**F. Descartes e fila**

- **4 — FILA: anterioridade sustentada; justificativa do hash não sustentada.** A flag salva dispara invalidação de todos os fechamentos com previsto preenchido do colaborador, sem excluir exportados. Ela zera `previsto_em` e **avança**, não zera, `atualizado_em`. `app/colaboradores/services/vinculo.py:894`, `app/escala/signals.py:146`, `app/ponto/services/fechamento.py:1611`, `app/ponto/services/fechamento.py:1613`

  O apply mede **contagens**, não hash do exportado; o hash disponível ainda exclui justamente os dois carimbos. Portanto, não comprova fechamento integralmente intocado. A fila é defensável para dívida anterior de cache, mas não serve como prova de preservação integral; alteração de dinheiro/TXT em produção: **[nao verificado]**. `app/escala/management/commands/aplicar_feriado_lei_1609.py:75`, `app/ponto/portas/regen_exportada.py:37`, `app/folha/export.py:1045`

- **5 — DESCARTADO: sustentado.** O universo usa o juiz de cobertura registrado, com início/fim inclusivos e vigência declarada; isso permite `ativa=False` com fim ainda vigente e exclui início futuro. Não demonstra bloqueio autônomo de pouso. `app/escala/models.py:949`, `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:219`

- **6 — FILA: sustentada como comportamento anterior e comando sob ordem.** Continua sendo decisão por cargo, embora escreva pela porta; ficar fora do cron não unifica o juiz. A classificação operacional consta no cadastro de comandos. Não transforma o delta em criador dessa dívida, mas impede afirmar eliminação da derivação paralela. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:58`, `app/config/crons.py:1406`

A prova disponível de Finados é fixture de teste. Rejulgamento real de 02/11, conferência de 20/11, encaminhamento dos sete 12x36 e preservação medida dos exportados: **[nao verificado]**. `app/escala/tests/test_feriado_lei_1609.py:114`, `app/escala/tests/test_feriado_lei_1609.py:159`

**REPROVA — B: catraca aceita nascimentos e escritas fora da porta sem declarar esses limites, especialmente gerente reverso e atualização por `update_or_create` (`app/escala/models.py:872`; `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:24`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:136`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:147`).**

## 13/11 11:25 · sessao 038c6893fc6f · - · - · origem=Ronald (tmux)

Confirmei em memória, com `python3 -B` e sem escrita: o helper real retorna censo **43**, soma dos tetos **43**, `TOTAL_RELOGIO=43` e igualdade exata entre censo e tetos, sem diferenças (`app/ponto/tests/test_cadastro_futuro_c5e.py:334`, `:374`, `:574`, `:591`).

`marcar_trabalha_em_feriado` retorna zero leituras no scanner; a regeneração retorna os dois carimbos `now()` de `app/ponto/portas/celula.py:263` e `:308`, compatíveis com teto 2 (`app/ponto/tests/test_cadastro_futuro_c5e.py:369`).

Foi execução dos helpers AST, não execução da suíte Django [nao verificado]. [nao verificado]

## 13/11 11:25 · sessao 9e470830c035 · LEI-DESLIGADO-NAO-TRAVA · sonda · origem=Code

Fonte: saída fornecida na mensagem, sem referência arquivo:linha; uso o marcador exigido `[nao verificado]`.

(a) Casos solicitados — dias retornados por `motivos_retencao_celula` e resultado de `classificar_export`. [nao verificado]

| Colaborador | situacao | data_demissao | Competência | Dias | Motivos | Export: status/motivo | Referência |
|---|---|---|---|---:|---|---|---|
| col718 | desligado | 2026-11-05 | 10/2026 | 12 | `espelho_furo` | `fora/furo_espelho` | [nao verificado] |
| col718 | desligado | 2026-11-05 | 11/2026 | 8 | `espelho_furo` | `fora/rescisao_modulo_proprio` | [nao verificado] |
| col270 | desligado | 2026-10-30 | 10/2026 | 9 | `espelho_furo` | `fora/furo_espelho` | [nao verificado] |
| col270 | desligado | 2026-10-30 | 11/2026 | 3 | `espelho_furo` | `fora/rescisao_modulo_proprio` | [nao verificado] |
| col373 | desligado | 2026-10-31 | 10/2026 | 8 | `espelho_cobrado`, `espelho_furo` | `fora/furo_espelho` | [nao verificado] |
| col373 | desligado | 2026-10-31 | 11/2026 | 1 | `espelho_indefinida` | `fora/rescisao_modulo_proprio` | [nao verificado] |

(b) Frota — contagens exatamente como impressas. [nao verificado]

| Competência | Empresa | Desligados com célula | Retidos | Dias | Motivos e contagens | Export: status/motivo e contagens | Referência |
|---|---|---:|---:|---:|---|---|---|
| 10/2026 | emp2 | 77 | 42 | 228 | `espelho_furo`: 121; `espelho_fato_em_ausencia`: 2; `espelho_nunca_bateu`: 55; `espelho_indefinida`: 47; `espelho_cobrado`: 3 | `fora/furo_espelho`: 14; `fora/rescisao_modulo_proprio`: 28 | [nao verificado] |
| 10/2026 | emp3 | 19 | 10 | 40 | `espelho_furo`: 37; `espelho_cobrado`: 2; `espelho_indefinida`: 1 | `fora/rescisao_modulo_proprio`: 7; `fora/furo_espelho`: 3 | [nao verificado] |
| 10/2026 | emp4 | 2 | 1 | 6 | `espelho_furo`: 3; `espelho_fato_em_ausencia`: 1; `espelho_indefinida`: 2 | `fora/furo_espelho`: 1 | [nao verificado] |
| 11/2026 | emp2 | 40 | 13 | 88 | `espelho_furo`: 45; `espelho_nunca_bateu`: 26; `espelho_fato_em_ausencia`: 3; `espelho_indefinida`: 7; `espelho_cobrado`: 7 | `fora/rescisao_modulo_proprio`: 12; `fora/furo_espelho`: 1 | [nao verificado] |
| 11/2026 | emp3 | 15 | 6 | 21 | `espelho_furo`: 14; `espelho_cobrado`: 1; `espelho_indefinida`: 6 | `fora/rescisao_modulo_proprio`: 6 | [nao verificado] |
| 11/2026 | emp4 | 2 | 1 | 5 | `espelho_indefinida`: 1; `espelho_furo`: 4 | `fora/rescisao_modulo_proprio`: 1 | [nao verificado] |

(c) Vocabulário de `situacao`. [nao verificado]

| situacao | Quantidade | Referência |
|---|---:|---|
| `ativo` | 547 | [nao verificado] |
| `desligado` | 386 | [nao verificado] |
| `afastado` | 11 | [nao verificado] |

(d) A sonda informa **`ERROS 0`**. [nao verificado]

## 13/11 11:24 · sessao 0faef1817065 · - · - · origem=Ronald (tmux)

**Âncora/mutante curados.** A âncora de criação fora de `executar_vinculo` ocorre uma vez em `vinculo.py:975`; o teste agora exige uma ocorrência e retira a flag desse trecho, cobrando especificamente `criar_trecho_retroativo` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:209`). Reproduzi os seis corpos de teste com armazenamento temporário em memória: passaram; o mutante do trecho retornou o achado esperado. Execução pelo runner Django original: [nao verificado].

**n–s:**

| Caso | O que realmente cobra |
|---|---|
| n | Detecta `EC.objects.all().create(...)` sem flag (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:235`, `:254`). |
| o | Detecta cadeia `_base_manager.filter(...).get_or_create(...)` sem flag (`:236`, `:255`). |
| p | Detecta nascimento sem flag quando só `defaults` tem a flag e `create_defaults` não tem (`:237`, `:256`). |
| q | Controle negativo: aceita flag em `create_defaults`; não testa escrita indevida em existente (`:239`, `:257`). |
| r | Detecta ausência de keyword literal; acusa até `**kw` correto, como declarado (`:25`, `:241`, `:257`). |
| s | Controle negativo de receptor não identificado como modelo; não prova que `c` nunca representará um modelo (`:242`, `:257`). |

São exemplos AST, não exercícios de criação no ORM: são strings entregues ao scanner (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:219`, `:243`).

**Buracos não declarados, reproduzidos em memória com resultado `[]`:**

1. **Atualização pelo `update_or_create`.** Exemplo:

   ```python
   EC.objects.update_or_create(
       pk=1,
       defaults={"trabalha_em_feriado": True},
       create_defaults={"trabalha_em_feriado": origem.trabalha_em_feriado},
   )
   ```

   O scanner aprova a origem do valor de nascimento e não examina a atualização de existente por essa API; só há branches de escrita para `update`, `bulk_update`, `setattr` literal e atribuição direta (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`, `:139`, `:147`, `:156`).

2. **Prioridade incorreta de busca versus defaults.** `get_or_create(trabalha_em_feriado=origem.trabalha_em_feriado, defaults={"trabalha_em_feriado": True})` passa porque `_kw` retorna primeiro a keyword de busca (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:112`).

3. **Sobreposição dentro do próprio dict.** Passam tanto `defaults={"trabalha_em_feriado": origem.trabalha_em_feriado, **kw}` como uma segunda chave literal com valor `True`: `_kw` aceita a primeira ocorrência literal e não resolve sobreposições (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:116`).

4. **Gerente reverso.** `colaborador.escalas.create(posto=p, tipo_escala=t, data_inicio=d)` escapa: a relação existe (`app/escala/models.py:872`), mas `_do_gerente` exige acesso reconhecido pelo modelo e pelos nomes `objects`, `_default_manager`, `_base_manager` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:43`, `:97`).

5. **Resolução indireta do modelo/construtor.** Alias anotado (`EC: type = EscalaColaborador`), função ligada (`criar = EC.objects.create`) e modelo obtido por `apps.get_model(...)` passam. `_apelidos` só acompanha `ImportFrom` e atribuição simples cujo valor já seja reconhecido como modelo (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:57`, `:68`, `:76`).

6. **APIs assíncronas.** Nomes como `acreate`, `aget_or_create`, `aupdate_or_create`, `abulk_create`, `aupdate` e `abulk_update` não entram nas listas/branches (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:42`, `:136`, `:147`, `:149`).

7. **Escritas indiretas/desempacotadas.** Passam atribuição `ec.trabalha_em_feriado, ec.ativa = True, True`, `ec.__dict__["trabalha_em_feriado"] = True`, `setattr(ec, campo, True)` e `bulk_update(objs, campos)` com o nome armazenado em variável. A detecção exige alvo `ast.Attribute` direto e constantes literais nos dois últimos casos (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:149`, `:153`, `:156`).

Esses limites não aparecem na lista declarada (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:24`). São possibilidades demonstradas do scanner; uso atual dessas formas para escrever a flag na produção da cópia: [nao verificado].

O censo do scanner atual retorna zero achados e um escritor. Os quatro nascimentos explícitos estão em `app/colaboradores/services/vinculo.py:287`, `:971`, `:1090`, `:1111`; a atribuição direta da porta está em `:893`. Essa contagem não fecha os buracos acima, pois usa justamente o scanner limitado (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:124`).

## 13/11 11:23 · sessao c09f9bf8868b · - · - · origem=Ronald (tmux)

F e pontos de E/D auditados:

- **4 — FILA com ressalva.** A porta salva a flag (`app/colaboradores/services/vinculo.py:894`); o signal chama a invalidação (`app/escala/signals.py:137-146`), que alcança todos os fechamentos com previsto preenchido do colaborador, sem excluir exportados (`app/ponto/services/fechamento.py:1607-1613`). Ela **zera `previsto_em`, mas atualiza `atualizado_em` para agora** (`app/ponto/services/fechamento.py:1613`). O comportamento é anterior no delta fornecido; a execução de produção é [nao verificado].

- A alegação da triagem de que **“o apply mede hash do exportado antes/depois” não está implementada nesse comando**: seu corpo mede universo/contagens e escreve reversão, sem hash (`app/escala/management/commands/aplicar_feriado_lei_1609.py:43-77`). Além disso, os hashes usuais excluem deliberadamente `previsto_em` e `atualizado_em` (`app/ponto/portas/regen_exportada.py:35-38`, `app/ponto/portas/regen_exportada.py:62-74`; `app/ponto/management/commands/carimbo_gravado.py:24-36`). Portanto, mesmo um hash desses intacto não provaria preservação dos carimbos.

- O motivo para tratar 4 como dívida anterior é defensável **se preservar exportada significa preservar dinheiro/TXT**, pois `previsto_em` é explicitamente o frescor do previsto, separado de `derivado_em`, carimbo do registro inteiro (`app/ponto/models.py:430-431`, `app/ponto/models.py:453-464`). Entretanto, sua alteração tem leitor real: `prontidao` abandona snapshot e recalcula ao vivo quando o campo é nulo (`app/folha/export.py:1045-1049`, `app/folha/export.py:1081`). Não declarar o fechamento inteiro literalmente intocado.

- **5 — DESCARTE sustentado pelo código.** O universo usa a autoridade `q_vinculo_cobre_a_data` (`app/escala/models.py:947-950`). Cobertura exige início ≤ data, fim nulo ou ≥ data, e vigência declarada: `ativa` **ou** fim não nulo (`app/escala/alimentacao.py:195-196`, `app/escala/alimentacao.py:214-225`). Portanto, um `ativa=False` com fim hoje/futuro é vigente e entra; um `ativa=True` com início futuro não entra. As duas pontas são inclusivas (`app/escala/alimentacao.py:214-225`). Não há bloqueio autônomo de pouso demonstrado por esse descarte.

- **6 — FILA sustentada quanto à anterioridade e ausência de cron**, pois o delta fornecido só injeta o dia nesse comando e o cadastro o classifica como sincronização sob ordem (`app/config/crons.py:1406`). A divergência continua viva: cargo decide o desejado (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19-21`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42-58`), enquanto a lei decide por posto/ciclo (`app/escala/models.py:905-917`). Exemplo lógico: cargo administrativo em posto que declara operar recebe False no sincronizador e True no juiz (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`; `app/escala/models.py:911-913`). É segunda derivação do desejado em sítio tocado, embora a escrita continue pela porta (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:58`).

- **Ordem de trava:** comprova-se `atomic` → UPDATE vínculo → UPDATE FechamentoMensal → escrita de células / trava das futuras (`app/colaboradores/services/vinculo.py:868`, `app/colaboradores/services/vinculo.py:894`; `app/ponto/services/fechamento.py:1613`; `app/ponto/portas/celula.py:308-311`; `app/ponto/services/cadastro_futuro.py:30-36`). **Não encontrei trava de competência nesse caminho**: a guarda de exportada é consulta, seguida de `atomic`, sem `select_for_update` (`app/ponto/portas/celula.py:218-226`). O dinheiro é agendado para depois do commit (`app/ponto/portas/celula.py:483-496`; `app/ponto/services/fechamento.py:1798-1824`). Deadlock concreto/violação de ordem global: [nao verificado]. Não confundir consulta de guarda com aquisição de trava.

- **Pré-requisitos:** lista esperada é conferida antes da primeira escrita; reversão é criada com modo exclusivo antes do primeiro vínculo (`app/escala/management/commands/aplicar_feriado_lei_1609.py:61-73`). O comando publica contagens por grupo e lista plana de IDs; não oferece hash, conferência explícita de 20/11 nem relatório do rejulgamento de 02/11 (`app/escala/management/commands/aplicar_feriado_lei_1609.py:36-77`). Há teste de Finados em fixture, sem equivaler a produção (`app/escala/tests/test_feriado_lei_1609.py:112-177`). Aplicação, publicação da lista, reversão efetiva, 7 casos 12x36 encaminhados, resultado de 02/11 e conferência de 20/11: [nao verificado].

- **Um significado da flag:** o nascimento consulta posto em todo ciclo (`app/escala/models.py:905-917`), mas o efeito de virar folga é condicionado à mesma flag False, ciclo elegível e feriado do município do vínculo (`app/escala/models.py:1410-1432`). A precedência chama esse predicado (`app/ponto/precedencia.py:369-377`). False em 12x36 com efeito nulo está expressamente previsto, não demonstra por si campo com dois significados (`app/escala/models.py:1427-1430`).

## 13/11 11:23 · sessao 038c6893fc6f · - · - · origem=Ronald (tmux)

Censo C: 16 pontos de chamada, todos com `hoje` explícito; não encontrei chamador sem ele. A assinatura exige keyword sem default (`app/colaboradores/services/vinculo.py:869`).

| Chamador | Origem de `hoje` | Leituras diretas para `hoje` |
|---|---|---|
| `app/colaboradores/management/commands/sincronizar_feriado_admin.py:58` | `timezone.localdate()` em `:56` | 1 por execução com `--apply`; 0 no DRY |
| `app/escala/management/commands/aplicar_feriado_lei_1609.py:73` | `timezone.localdate()` em `:48` | 1 por execução, inclusive DRY; mesmo valor antes `:49` e depois `:75` |
| `app/chamados/tests/test_jv1_feriado_vira_vigia.py:89` | `timezone.localdate()`, sob freeze de `:33`/`:38` | 1 por `_marcar`; chamadas em `:110`, `:124`, `:139`, `:147`, `:149` |
| `app/chamados/tests/test_jv1_volta2_lugar_e_ciclo.py:133` | `timezone.localdate()`, freeze em `:124` | 1 por `_marcar`; chamadas em `:154`, `:168` |
| `app/escala/tests/test_porta_escala.py:62`, `:65` | Literal `datetime.date(2026,11,13)` | 0 |
| `app/ponto/tests/test_cadastro_futuro_c3.py:114` | `HOJE` importado da base em `:15` | 0 |
| `app/ponto/tests/test_cadastro_futuro_c5b.py:124`, `:138`, `:144` | `HOJE` importado da base em `:25` | 0 |
| `app/ponto/tests/test_cadastro_futuro_sem_banco.py:446`, `:469`, `:476`, `:481`, alias `evento` em `:662`, `:664` | Literal `HOJE = date(2026,11,12)` em `:20`; alias em `:661` | 0 |

Chamadores indiretos dos comandos: `call_command` de admin em `app/colaboradores/tests/test_feriado_admin_sync.py:41` e `:45` usa o relógio real do comando; aplicar é chamado em `app/escala/tests/test_feriado_lei_1609.py:102` e no helper `:140`, a segunda classe sob freeze `:112`. O helper é usado em `:150`, `:157`, `:164`, `:183`, `:189` e `:190`.

A alegação “um relógio por ato” só ficou verdadeira para a captura explícita de `hoje` nessas bordas, não para a cadeia:

- A porta salva a flag em `app/colaboradores/services/vinculo.py:894`; o signal `app/escala/signals.py:137` chama `invalidar_previsto` em `:146`, que lê `timezone.now()` em `app/ponto/services/fechamento.py:1613`, por vínculo alterado.
- A regeneração carimba cada célula reescrita com `timezone.now()` em `app/ponto/portas/celula.py:308` — outro ramo em `:263`.
- Há uma leitura **decisória**, não apenas carimbo: `_o_chao_do_dia_mudou` não recebe `hoje` em `app/ponto/portas/celula.py:404`; relê `timezone.localdate()` em `:464` e usa esse novo dia para separar passado/futuro em `:469`.
- O suplemento futuro chama a regeneração uma vez por dia em `app/ponto/services/cadastro_futuro.py:43`; cada dia tocado pode chegar novamente à leitura de `app/ponto/portas/celula.py:464`.
- O recalculo financeiro também não recebe o instante da borda: `app/ponto/services/fechamento.py:1798`, callback `:1807`, chamada `:1810`; lê relógio para teto do fato em `:616`, previsto em `:1086`, derivação em `:1143` ou `:1189`, atualização em `:1161`. Número total executado por ato depende das células, competências e callbacks [nao verificado].
- O no-op retorna antes desses efeitos em `app/colaboradores/services/vinculo.py:890`.

D, significado e fluxo:

- `hoje` das bordas é dia civil local: `timezone.localdate` em ambos comandos; configuração `America/Sao_Paulo` e `USE_TZ=True` em `app/config/settings/base.py:93` e `:95`.
- A porta usa `janela_anterior(hoje, empresa)` em `app/colaboradores/services/vinculo.py:900`; começa no máximo entre início do vínculo e início anterior em `:901`; vai até `hoje` em `:902`/`:906`.
- Janelas são inclusivas: corte do mês anterior a corte−1 do mês corrente, `app/ponto/janelas.py:10` a `:21`; atual e anterior em `:60` e `:66`. A célula percorre `while d <= ate` em `app/ponto/portas/celula.py:228` e barra exportadas usando `a <= d <= z` em `:284`.
- Futuro exclui hoje, começa em `hoje+1`, `app/ponto/services/cadastro_futuro.py:26`–`:27`; seleciona geradas existentes com `data__range` em `:31`–`:32`. Seu horizonte é união das competências correntes das empresas ativas, `app/ponto/management/commands/gerar_celulas.py:290`–`:300`, não estritamente a janela da empresa do vínculo.
- Vigência cobre início e fim inclusivos, `app/escala/alimentacao.py:214`–`:216`; SQL equivalente em `:219`–`:225`. `ativa=False` com fim futuro pode entrar legitimamente.
- `previsto_em` é carimbo de frescor do previsto, não garantia de frescor do registro inteiro; isso está explícito em `app/ponto/models.py:430`, `:453`–`:464`.
- Escritores: recalculo atribui timestamp em `app/ponto/services/fechamento.py:1086`, limpa em falha `:1099`, persiste via `save` em `:1144` ou update `:1188`; invalidador limpa em `:1613`.
- Leitor operacional da prontidão: `app/folha/export.py:1045` aceita snapshot quando `previsto_em` existe e minutos não são `None`; explica a invalidação ativa em `:1046`–`:1049`.
- Fotografia E4B trata `previsto_em` como carimbo mutável da leitura: `app/chamados/services/foto_e4b.py:10`–`:15`.
- Hash financeiro o exclui deliberadamente, juntamente com `atualizado_em`: `app/ponto/portas/regen_exportada.py:37`–`:38`, `:62`–`:74`.
- Flag é lida para no-op/trilha em `app/colaboradores/services/vinculo.py:890`/`:892`; lida/copiada nos nascimentos `:975`, `:1093`, `:1114`; nascimento novo pergunta ao juiz em `:296`; escritor de alteração `:893`–`:894`.
- Juiz padrão posto/ciclo em `app/escala/models.py:905`–`:917`; contador lê a flag em `:961`, usa esse juiz em `:960`; consumidor efetivo `feriado_vira_folga` lê a flag em `:1427` e restringe ciclos em `:1429`. O campo tem help text restrito a weekday em `:898`; a flag pode ser False em 12x36 sem tornar aquele plantão folga (`:1429`).
- Há divisão de instante dentro do ato: hoje da borda rege universo/regeneração e o novo hoje de `celula.py:464` rege julgamento; execução atravessando meia-noite pode discordar nesses limites, pela comparação `:469`. Ocorrência real [nao verificado].

F4: a justificativa “o apply mede hash do exportado antes/depois” **não aparece no comando**: `app/escala/management/commands/aplicar_feriado_lei_1609.py:43`–`:77` mede contador antes/depois, não hash. O hash padrão excluiria exatamente `previsto_em` e `atualizado_em` (`app/ponto/portas/regen_exportada.py:37`–`:38`). O signal atinge todos os fechamentos do colaborador com previsto preenchido, sem guarda de exportado/competência (`app/escala/signals.py:137`–`:146`; `app/ponto/services/fechamento.py:1610`–`:1613`). Ele zera `previsto_em`, **empurra**, não zera, `atualizado_em` (`:1613`).

## 13/11 11:14 · sessao 488ea9e39dfa · FERIADO-USA-A-LEI-DE-1609 · verificador · origem=Code

A. Os quatro nascimentos localizados em produção são:

| Sítio | Origem de `trabalha_em_feriado` |
|---|---|
| `executar_vinculo` | Pergunta a `padrao_trabalha_em_feriado(template, colaborador.posto)`. `app/colaboradores/services/vinculo.py:287`, `app/colaboradores/services/vinculo.py:296`. |
| `criar_trecho_retroativo` | Copia `ec.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:970`, `app/colaboradores/services/vinculo.py:974`. |
| `ajustar_vinculo_pelo_sistema` | Copia `ec.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:1089`, `app/colaboradores/services/vinculo.py:1092`. |
| `desfazer_ajuste_do_sistema` | Copia `antigo.trabalha_em_feriado`. `app/colaboradores/services/vinculo.py:1110`, `app/colaboradores/services/vinculo.py:1113`. |

Nenhum desses quatro depende do default; o default `True` continua no modelo. O censo declarado pelo próprio selo também nomeia esses quatro nascimentos. `app/escala/models.py:897`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:16`.

A catraca reconhece importação com apelido, atribuições de apelidos e os managers `objects`, `_default_manager`, `_base_manager`. Construtor reconhecido sem keyword da flag é denunciado, mesmo seguido de `save`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:33`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:47`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`.

Há uma imprecisão no limite declarado: `EC.objects.create(**kw)` **é visto**, mas `_kw` não resolve o dicionário e acusa ausência da flag; pode reprovar inclusive um dicionário correto. Já `.update(**kw)` escapa à busca de keyword explícita. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:20`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:116`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:122`.

**Formas invisíveis e não declaradas no limite:**

- `EC.objects.all().create(...)`: o detector exige o manager imediatamente antes do criador. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:110`.
- Cópia por `ec.pk = None; ec.save()`: não há fiscalização de reset da PK nem de nascimento por `save`. Ocorrência real dessa forma para `EscalaColaborador`: `[nao verificado]`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:108`, `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:131`.
- `update_or_create(..., defaults={flag: origem.flag}, create_defaults={...})`: `create_defaults` não é examinado; a flag de `defaults` pode satisfazer a catraca sem garantir a criação. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`.
- A autenticidade do juiz e da cópia não é comprovada: qualquer chamada terminada em `.padrao_trabalha_em_feriado(...)` e qualquer atributo terminado em `.trabalha_em_feriado` são aceitos pelo nome. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:70`.

B. O universo implementado é:

`data_inicio <= hoje`, término nulo ou `>= hoje`, vigência declarada, colaborador com `situacao='ativo'` e ciclo em `CICLOS_FERIADO_VIRA_FOLGA`. Vigência declarada significa `ativa=True` **ou** término preenchido. `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:219`, `app/escala/models.py:949`.

Portanto, **não equivale a `ativa=True`**: admite inativo com intervalo cobrindo hoje; exclui ativo futuro ou cujo término já passou. São contraexemplos do predicado, não constatações de produção. A equivalência pretendida entre “ATIVOS” do aval e esse recorte está `[nao verificado]`. `app/escala/alimentacao.py:214`, `app/escala/models.py:949`.

O mapa de escrita e leitura dos insumos é:

| Campo | Escritores | Leitores relevantes |
|---|---|---|
| `ativa` | Nascimentos; reativação; fechamento; absorção; snapshot; desfazer. `app/colaboradores/services/vinculo.py:276`, `:292`, `:641`, `:730`, `:799`, `:843`, `:1031`, `:1106`. | Juiz de cobertura e seleção do vínculo. `app/escala/alimentacao.py:196`, `:214`, `:286`. |
| `data_inicio` | Nascimentos e restauração do snapshot. `app/colaboradores/services/vinculo.py:291`, `:972`, `:1032`, `:1090`, `:1111`. | Cobertura e início da regeneração. `app/escala/alimentacao.py:214`, `app/colaboradores/services/vinculo.py:900`. |
| `data_fim` | Reativação, fechamento/reabertura, encerramento, correção retroativa, snapshot e desfazer. `app/colaboradores/services/vinculo.py:277`, `:641`, `:730`, `:844`, `:982`, `:1032`, `:1106`. | Cobertura e guarda de regeneração. `app/escala/alimentacao.py:214`, `app/ponto/portas/celula.py:229`. |
| `situacao` | Cadastro pela porta do colaborador; desligamento e reativação. `app/colaboradores/services/colaborador.py:32`, `:55`, `:73`, `:91`; `app/colaboradores/services/desligamento.py:109`, `:215`. | Filtro do contador e julgamento de vínculo. `app/escala/models.py:949`, `app/colaboradores/services/vinculo.py:91`. |
| `tipo_ciclo` e `tipo_base` | Cadastro do tipo; cópia do tipo. Edição pelo cadastro conserva o par existente. `app/escala/services/cadastro_tipo.py:152`, `:177`, `:179`, `:409`. | Universo, classificação, juiz padrão e feriado virar folga. `app/escala/models.py:916`, `:950`, `:955`, `:1429`. |
| `opera_em_feriado` | Criação/atualização genérica do posto e porta específica acionada pelo command. `app/colaboradores/services/estrutura.py:43`, `:67`, `:83`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:132`. | Juiz padrão, classificação DP, contador antigo, veto de cobrança e relatório de presença. `app/escala/models.py:911`, `:932`, `:956`; `app/colaboradores/services/feriado_do_posto.py:69`, `:127`. |
| Posto do vínculo | Nasce do cadastro ou é copiado do vínculo; atualização do vínculo pode substituir pelo posto do colaborador. `app/colaboradores/services/vinculo.py:275`, `:289`, `:971`, `:1090`, `:1111`. | É o posto usado pelo contador e pelo juiz padrão; também determina o lugar do feriado daquele vínculo. `app/escala/models.py:956`, `:960`, `:1420`. |
| Posto do colaborador | Cadastro e operações de troca pela porta do colaborador. `app/colaboradores/services/colaborador.py:33`, `:73`, `:91`; `app/colaboradores/views.py:327`, `:1388`. | Alimenta criação/atualização do vínculo; **não é consultado diretamente pelo contador**. `app/colaboradores/services/vinculo.py:275`, `:296`; `app/escala/models.py:960`. |

O valor **devido** vem exclusivamente da chamada ao juiz: posto declarado vence; sem declaração, somente 5x2/6x1 com base comercial recebem `False`; demais recebem `True`. `app/escala/models.py:911`, `app/escala/models.py:916`, `app/escala/models.py:960`.

A leitura direta de `opera_em_feriado` para separar o DP **não calcula outro devido**: identifica a ausência de declaração exigida pelo recorte e interrompe antes do julgamento. Inclui todos os 6x1 sem declaração, mesmo os cuja flag já coincidiria com o padrão. `app/escala/models.py:956`, `app/escala/models.py:960`.

Quanto a `posto=None`: a FK do vínculo é obrigatória. O ramo defensivo, se receber efetivamente `None`, encaminha 6x1 para `dp[None]`; nos demais ciclos o juiz usa o tipo. Existência de vínculo assim em produção: `[nao verificado]`. `app/escala/models.py:873`, `app/escala/models.py:911`, `app/escala/models.py:956`.

C. O command altera a flag somente chamando `marcar_trabalha_em_feriado`; a atribuição e o `save` estão nessa porta. `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:893`.

A reversão usa modo exclusivo `'x'`, grava os pares e fecha o arquivo antes da primeira chamada à porta. Lista diferente do universo interrompe antes da reversão e das escritas de negócio. Não há transação ou bloqueio abrangendo o lote inteiro; a comparação protege a fotografia consultada. `app/escala/management/commands/aplicar_feriado_lei_1609.py:64`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:68`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:69`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:72`.

O command publica os grupos antes, os IDs com seus valores desejados, a lista DP por posto e as contagens depois. `app/escala/management/commands/aplicar_feriado_lei_1609.py:36`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:51`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:55`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:75`.

**Relógio:** há uma leitura de `localdate()` no command e outra por vínculo efetivamente alterado na porta: **`1 + N` leituras diretas**. Não há `now()` diretamente nesses dois corpos; o `hoje` do command não é transmitido à porta. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:890`, `app/colaboradores/services/vinculo.py:897`.

Existem leituras transitivas adicionais: `now()` na invalidação do previsto, `now()` por célula reescrita e `localdate()` no julgamento da regeneração. Logo, “UM relógio” não descreve o ato completo. `app/ponto/services/fechamento.py:1613`, `app/ponto/portas/celula.py:308`, `app/ponto/portas/celula.py:464`.

O primeiro dia **solicitado** é `max(data_inicio, início da janela anterior)`. Com hoje em 13/11/2026, corte 21 e vínculo iniciado antes disso, resulta em **21/09/2026**; primeiro dia efetivamente alterado na frota: `[nao verificado]`. `app/colaboradores/services/vinculo.py:899`, `app/ponto/janelas.py:10`, `app/ponto/janelas.py:66`.

A guarda das células considera TXT entregue ou holerite publicado; `FechamentoMensal.status` sozinho não a fecha. Os dias barrados são pulados antes da regeneração. `app/ponto/management/commands/gerar_celulas.py:135`, `app/folha/export.py:618`, `app/ponto/portas/celula.py:284`.

**O `FechamentoMensal` exportado não fica integralmente intacto:** o `save` da flag dispara `invalidar_previsto`, que zera `previsto_em` e altera `atualizado_em` em todas as competências do colaborador com previsto preenchido, sem filtro de exportação. `app/colaboradores/services/vinculo.py:894`, `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1611`.

Se houver somente status exportado, sem entrega reconhecida pela guarda, a regeneração pode chegar ao recálculo; o escritor atualiza inclusive fechamentos de status diferente de aberto. Isso é possibilidade do caminho, não ocorrência comprovada na frota. `app/ponto/management/commands/gerar_celulas.py:141`, `app/ponto/services/fechamento.py:1809`, `app/ponto/services/fechamento.py:1145`, `app/ponto/services/fechamento.py:1160`.

Para o futuro, a porta alcança somente células geradas **já existentes**, dentro da janela selecionada. O teste novo monta e confere 02/11; conferência de 20/11 e resultados reais dos corrigidos estão `[nao verificado]`. `app/ponto/services/cadastro_futuro.py:26`, `app/ponto/services/cadastro_futuro.py:31`, `app/escala/tests/test_feriado_lei_1609.py:113`, `app/escala/tests/test_feriado_lei_1609.py:159`.

D. **12x36 fica fora** pelo filtro de ciclos; **6x1 sem declaração fica fora do lote** pelo `continue` para DP. `app/escala/models.py:32`, `app/escala/models.py:950`, `app/escala/models.py:957`.

“Outros” recebe somente **semanal** no conjunto atual. 24x48, personalizado e intermitente também ficam fora do universo; não entram escondidos em “outros”. O aval fornecido admite o grupo “outros”; sua população real está `[nao verificado]`. `app/escala/models.py:26`, `app/escala/models.py:32`, `app/escala/models.py:964`.

E. As classes de reprovação ficam assim:

- **Segundo escritor da flag:** os nascimentos passam valor explícito; a alteração posterior localizada está na porta. O command novo não cria escritor cru. `app/colaboradores/services/vinculo.py:296`, `:893`, `:974`, `:1092`, `:1113`; `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`.
- **Derivação paralela:** `sincronizar_feriado_admin` continua calculando a flag pelo cargo, sem consultar o juiz de posto/ciclo, e depois chama a porta. Pode contrariar o valor de 16/09, embora use o escritor correto; execução desse command em produção está `[nao verificado]`. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `:47`, `:56`; `app/escala/models.py:911`.
- **Segundo julgamento da cobrança:** após consultar `fatos_do_dia`, o emissor ainda aplica `cobra_no_dia`, que veta por posto fechado independentemente da flag. Isso é um veto paralelo vivo, declarado no comentário, e não a classificação DP. O juiz registrado para supressão por feriado é `fatos_do_dia`. `app/ponto/management/commands/detectar_ausencias.py:616`, `:619`, `:628`; `app/colaboradores/services/feriado_do_posto.py:69`; `app/core/juizes.py:296`.
- **Relógio fora da borda:** presente na porta e na regeneração, apesar da leitura inicial do command. `app/colaboradores/services/vinculo.py:897`, `app/ponto/portas/celula.py:464`.
- **Selo/RED:** os mutantes de remoção da keyword e literal `True` são denunciados pelo detector, mas o teste exige **duas** ocorrências de uma âncora que possui apenas **uma**. A outra criação tem `folga_calendario` depois da flag. Assim, o teste falha antes de alcançar o mutante fora de `executar_vinculo`. Suite Django executada: `[nao verificado]`. `app/escala/tests/test_feriado_nasce_do_juiz_selo.py:177`, `:184`, `:185`, `:186`; `app/colaboradores/services/vinculo.py:974`, `:1092`.
- **Semântica:** `trabalha_em_feriado` é autorização do vínculo com efeito limitado aos ciclos weekday; `opera_em_feriado` é declaração triestado do posto, usada tanto para inicialização quanto como veto direto de cobrança. Não são sinônimos nem uma cópia permanentemente sincronizada. `None` no posto significa “cobra” no veto, mas pode produzir folga pelo ciclo no juiz padrão. `app/escala/models.py:898`, `:911`, `:1427`; `app/colaboradores/models.py:256`; `app/colaboradores/services/feriado_do_posto.py:63`.

F. **A entrada nova não quebra o contrato enumerador encontrado.** O command ganha casa em `FORA_DE_PIPELINE`; o contrato exige justamente estar agendado ou nesse dicionário, e proíbe exclusão podre ou simultaneamente agendada. A enumeração de agendados contém `CRONS + ENCADEADOS`. `app/config/crons.py:1407`, `app/config/crons.py:1805`, `app/chamados/tests/test_contract_crons.py:220`, `app/chamados/tests/test_contract_crons.py:230`.

**REPROVA — bloqueantes: selo com âncora inválida (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:185`); nascimentos invisíveis não declarados (`:87`, `:110`); relógio relido fora da borda (`app/colaboradores/services/vinculo.py:897`); fechamento exportado alcançado pela invalidação (`app/ponto/services/fechamento.py:1611`); universo não equivalente a `ativa=True` (`app/escala/models.py:949`); derivação paralela por cargo (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:47`).**

## 13/11 11:11 · sessao 4564e305e0e5 · - · - · origem=Ronald (tmux)

A/E — resultados do explorador, somente leitura:

- Há quatro nascimentos explícitos em produção, todos por `EscalaColaborador.objects.create`: `executar_vinculo` pergunta ao juiz usando `template` e `colaborador.posto` (`app/colaboradores/services/vinculo.py:287`, `:296`); `criar_trecho_retroativo` copia `ec.trabalha_em_feriado` (`:970`, `:974`); `ajustar_vinculo_pelo_sistema` copia `ec.trabalha_em_feriado` (`:1089`, `:1092`); `desfazer_ajuste_do_sistema` copia `antigo.trabalha_em_feriado` (`:1110`, `:1113`). Nenhum desses usa o default, que permanece `True` (`app/escala/models.py:896`).
- O selo reconhece apelidos importados e atribuídos, os três managers nomeados, construtor sem keyword e chamadas diretas `create(**kw)`; esta última é acusada como ausência de valor explícito, contrariando a descrição de “não vê” (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:47`, `:87`, `:110`, `:116`). Executei as funções AST em memória: censo atual `achados=[]`; escritor contado uma vez (`:98`, `:131`). Suite Django não executada `[nao verificado]`.
- Formas invisíveis **não declaradas**: `EC.objects.all().create(...)`; clone `ec.pk=None; ec.save()`; `update_or_create(defaults={flag: origem.flag}, create_defaults={"ativa":True})`. As duas primeiras escapam porque o detector exige manager imediatamente antes da criação e não observa `save`/reset de PK; a terceira porque `_kw` ignora `create_defaults` (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:87`, `:110`, `:131`). Reprodução em memória confirmou `[]` nas três.
- A autenticidade da fonte também não é provada: qualquer `unrelated.padrao_trabalha_em_feriado(...)` ou atributo `qualquer_objeto.trabalha_em_feriado` é aceito apenas pelo nome (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:70`).
- **Bloqueante determinístico do selo:** o teste exige duas ocorrências da string terminada em `ec.trabalha_em_feriado)\n`, mas há uma só; a ocorrência do ajuste leva `folga_calendario` na sequência (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:184`, `:185`; `app/colaboradores/services/vinculo.py:974`, `:1092`). Portanto esse teste falha antes de exercitar o mutante externo pretendido (`app/escala/tests/test_feriado_nasce_do_juiz_selo.py:186`). Contagem literal por leitura confirmou `1`.
- O command antigo deriva flag por cargo, depois escreve pela porta: é derivação paralela da política da flag, embora não seja segundo escritor (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `:47`, `:56`).
- `cobra_no_dia` está vivo: `_abrir_chamado` é chamado no command e aplica primeiro `fatos_do_dia`, depois veto independente do posto (`app/ponto/management/commands/detectar_ausencias.py:440`, `:616`, `:628`). O código declara expressamente que posto fechado veta mesmo vínculo com flag de trabalho (`:619`). `opera_em_feriado=False` é, assim, também veto direto de cobrança; `None` cobra nesse predicado, enquanto o juiz padrão usa o ciclo (`app/colaboradores/services/feriado_do_posto.py:60`; `app/escala/models.py:911`).

## 13/11 11:11 · sessao 733cc442c44b · - · - · origem=Ronald (tmux)

C — conclusões por leitura:

- `--apply` escreve a flag exclusivamente pela porta `marcar_trabalha_em_feriado`; a reversão usa criação exclusiva (`'x'`) e termina antes do primeiro chamado à porta. Universo diferente da lista interrompe antes dessas escritas. Não há transação nem bloqueio do lote inteiro. `app/escala/management/commands/aplicar_feriado_lei_1609.py:61-73`.
- Relógios diretos: **1 `localdate()` no command + 1 por vínculo efetivamente alterado na porta**, total `1+N`; nenhum `now()` direto nesses dois corpos. O command não transmite seu `hoje` à porta. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:890`, `app/colaboradores/services/vinculo.py:897`.
- Há outras leituras transitivas: `now()` na invalidação do previsto disparada pelo save, `now()` por célula reescrita e `localdate()` no julgamento posterior. Portanto, o comentário “UM relógio” não descreve o ato completo. `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`, `app/ponto/portas/celula.py:308`, `app/ponto/portas/celula.py:464`.
- Primeiro dia **solicitado**: `max(ec.data_inicio, início da janela anterior à atual)`. Em 13/11/2026 com corte 21 e vínculo já iniciado, isso resulta em 21/09/2026. Primeiro dia efetivamente alterado em produção: `[nao verificado]`. `app/colaboradores/services/vinculo.py:897-905`, `app/ponto/janelas.py:10-33`, `app/ponto/janelas.py:66-72`.
- Células de competência entregue por TXT vigente ou holerite publicado são barradas; `FechamentoMensal.status='exportado'` sozinho **não** participa dessa guarda. `app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284-288`, `app/ponto/management/commands/gerar_celulas.py:135-188`, `app/folha/export.py:597-628`.
- **FechamentoMensal exportado não fica integralmente preservado:** todo save da flag chama `invalidar_previsto`, que altera `previsto_em` e `atualizado_em` de todas as competências do colaborador com previsto preenchido, sem filtro por status/exportação. `app/colaboradores/services/vinculo.py:894`, `app/escala/signals.py:137-146`, `app/ponto/services/fechamento.py:1607-1613`.
- Quando uma competência chega ao recalculo por evento, o dinheiro também não guarda exportada: chama o escritor, cujo ramo de status diferente de aberto atualiza os campos calculados, preservando status e aprovação. As células de competências entregues normalmente ficam fora de `_tocadas` pela guarda anterior; apenas status exportado não oferece essa proteção. `app/ponto/portas/celula.py:483-496`, `app/ponto/services/fechamento.py:1798-1810`, `app/ponto/services/fechamento.py:1137-1190`.
- Futuro: regenera somente células geradas **já existentes**, desde o máximo entre início da janela, amanhã e `desde`, até a união das janelas correntes das empresas ativas. A existência de 20/11 em produção é `[nao verificado]`. `app/ponto/services/cadastro_futuro.py:9-45`, `app/ponto/management/commands/gerar_celulas.py:290-300`.
- A prova nova monta e confere 02/11; não demonstra a conferência de 20/11. Execução real, resultados de produção e aprovação dos testes: `[nao verificado]`. `app/escala/tests/test_feriado_lei_1609.py:113-134`, `app/escala/tests/test_feriado_lei_1609.py:159-177`.

Bloqueante firme: **relógio relido fora da borda**, com `hoje` do command descartado na chamada da porta. A alegação de preservar integralmente `FechamentoMensal` exportado também é falsa pelos metadados; preservação das células depende da guarda de entrega, não do status do fechamento. `app/escala/management/commands/aplicar_feriado_lei_1609.py:48`, `app/escala/management/commands/aplicar_feriado_lei_1609.py:73`, `app/colaboradores/services/vinculo.py:897`, `app/ponto/services/fechamento.py:1607-1613`.

## 13/11 11:10 · sessao 554163499bde · - · - · origem=Ronald (tmux)

F: **não encontrei quebra de contrato pela entrada nova**: ela declara o comando sob ordem em `FORA_DE_PIPELINE`, com motivo (`app/config/crons.py:1407`).

O contrato B6.1 enumera arquivos de comandos (`app/chamados/tests/test_contract_crons.py:214`), exige casa em agendados ou `FORA_DE_PIPELINE` (`app/chamados/tests/test_contract_crons.py:220`) e proíbe entrada simultaneamente agendada ou sem arquivo vivo (`app/chamados/tests/test_contract_crons.py:230`). A fonte dos agendados contém somente `CRONS + ENCADEADOS` (`app/config/crons.py:1805`); o render percorre somente `CRONS` (`app/config/crons.py:1872`).

Reproduzi essas duas operações B6.1 por `python3 -B`, compilando a fonte em memória: `sem casa=[]`, `podres=[]`; `aplicar_feriado_lei_1609` está vivo, está fora do pipeline e não está agendado. Resultado de execução transitória, sem arquivo de evidência persistido: `[nao verificado]`. Os predicados reproduzidos estão em `app/chamados/tests/test_contract_crons.py:225` e `app/chamados/tests/test_contract_crons.py:234`.

Os demais contratos encontrados sobre estágio, dependências, horários e render examinam `CRONS`, portanto a entrada somente no dicionário de exclusões não os amplia (`app/chamados/tests/test_contract_crons.py:77`, `app/chamados/tests/test_contract_crons.py:269`, `app/chamados/tests/test_contract_crons.py:301`, `app/chamados/tests/test_contract_crons.py:330`).

## 13/11 10:52 · sessao bf56601fcd1a · FERIADO-NASCE-DA-ESCALA · verificador · origem=Code

**REPROVA.** Há caminhos que mantêm ou recriam `5x2=True`, herança sem escolha obrigatória, reversão incompleta e escolha humana sem efeito em dois ciclos. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1078`, `app/colaboradores/services/vinculo.py:185`, `app/escala/models.py:1422`)

A correspondência da cópia com o hash Git informado, execução da suíte, opus, deploy, aplicação dos 16 e resultados posteriores em produção permanecem **[não verificado]**. [nao verificado]

**A. Todos os nascimentos e reaproveitamentos encontrados**

| Sítio de produção | Origem de `trabalha_em_feriado` |
|---|---|
| `executar_vinculo`: criação | `v['trabalha_em_feriado']`, calculado por `_feriado_do_veredito`. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:338`) |
| `executar_vinculo`: reaproveitamento da mesma data | Troca o tipo e recebe a flag do mesmo veredito. (`app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`) |
| `criar_trecho_retroativo` | `feriado_do_trecho(tipo_destino, flag_original)`. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1021`) |
| `ajustar_vinculo_pelo_sistema` | `feriado_do_trecho(tipo_derivado, flag_original)`. (`app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1139`) |
| `desfazer_ajuste_do_sistema`: renascimento | `feriado_do_trecho(tipo_antigo, flag_antiga)`. (`app/colaboradores/services/vinculo.py:1158`, `app/colaboradores/services/vinculo.py:1161`) |
| `desfazer_ajuste_do_sistema`: reabertura | Reativa o antigo, mantendo sua flag sem novo julgamento. (`app/colaboradores/services/vinculo.py:1149`, `app/colaboradores/services/vinculo.py:1154`) |
| `corrigir_vinculo_retroativo`: troca de tipo | **Mantém a flag anterior sem consultar o juiz.** (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`) |
| `reverter_para_snapshot`: restauração de tipo | **Mantém a flag atual; o snapshot não contém a anterior.** (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:1078`) |

Nos quatro `create` encontrados, a flag é explícita. Não encontrei outro nascimento por `get_or_create`, `update_or_create`, construtor ou `bulk_create` na produção examinada; ausência de caminhos dinâmicos adicionais é **[não verificado]**. Os quatro nascimentos estão em `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1136` e `app/colaboradores/services/vinculo.py:1158`.

**Bloqueante 1 — troca retroativa fora do julgamento:** `6x1=True → 5x2` pela correção no lugar continua `True`. O comando chama exatamente essa função. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`)

**Bloqueante 2 — reversão do aceite incompleta:** reaproveitar `5x2=False` como `12x36=True` e depois recusar restaura o tipo `5x2`, mas deixa `True`. Uma mudança somente da flag também não é desfeita. O snapshot omite a flag e a restauração não a escreve. (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:317`, `app/colaboradores/services/vinculo.py:1078`, `app/chamados/services/acoes_chamado.py:149`)

O default perigoso continua `True`; a proteção depende das portas e do selo, que tem escapes descritos em G. (`app/escala/models.py:897`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`)

**B. Matriz completa e identidade do substituído**

A função não recebe `acao`: a tabela abaixo vale igualmente quando chamada durante `criar`, `atualizar` ou o candidato inicial a `no_op`. `R` significa `ValueError`; `S` significa a flag armazenada no substituído. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:165`)

| Juiz | Substituído | `''`/ausente | `'1'` | `'0'` | Inválida |
|---|---|---:|---:|---:|---:|
| `True` | Nenhum / mesmo ciclo / outro ciclo | `True` | `True` | R | R |
| `False` | Nenhum / mesmo ciclo / outro ciclo | `False` | R | `False` | R |
| `None` | Nenhum | R | `True` | `False` | R |
| `None` | Mesmo ciclo | S | `True` | `False` | R |
| `None` | Outro ciclo | R | `True` | `False` | R |

A validação da escolha inválida precede o juiz; as respostas fixas vêm antes da herança; o mesmo ciclo somente é considerado quando o juiz retorna `None`. (`app/colaboradores/services/vinculo.py:173`, `app/colaboradores/services/vinculo.py:178`, `app/colaboradores/services/vinculo.py:185`)

O produto cartesiano tem combinações inalcançáveis pelo chamador:

- **`criar`:** não existe vínculo da mesma data; o substituído pode ser ausente, ativo de mesmo ciclo ou ativo de outro ciclo. A ação continua `criar`. (`app/colaboradores/services/vinculo.py:102`, `app/colaboradores/services/vinculo.py:125`, `app/colaboradores/services/vinculo.py:141`)
- **`atualizar`:** existe vínculo da mesma data, inclusive inativo; ele sempre ganha a preferência como substituído. Pode ter mesmo ciclo ou outro ciclo, mas não ser `None`. (`app/colaboradores/services/vinculo.py:122`, `app/colaboradores/services/vinculo.py:141`)
- **Candidato a `no_op`:** existente ativo, mesmo template e âncora compatível; portanto substituído existente e mesmo ciclo. Se a flag calculada difere, vira `atualizar`; caso contrário permanece `no_op`. (`app/colaboradores/services/vinculo.py:119`, `app/colaboradores/services/vinculo.py:145`)
- **Outras ações:** `julgar_vinculo` somente produz essas três. (`app/colaboradores/services/vinculo.py:120`)

**Bloqueante 3 — nascimento sem escolha:** `criar` um novo vínculo `6x1` em outra data, tendo substituído `6x1`, aceita escolha vazia e herda a flag. Não exige prova de escolha humana; um `True` legado pelo default passa como escolha. Isso é uma exceção implementada à obrigatoriedade de escolha no nascimento. (`app/colaboradores/services/vinculo.py:125`, `app/colaboradores/services/vinculo.py:170`, `app/colaboradores/services/vinculo.py:185`)

**O substituído não representa sempre o vínculo cujo período está sendo substituído:**

- Sem existente na mesma data, a consulta pega o ativo mais recente **sem limitar seu início a D**. Se ele começa depois de D, `fechar_vigencia` o recusa e `absorver_vigencias_posteriores` o absorve. A afirmação de que seria “o ativo que o executor fecha” é falsa nesse caso. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:664`, `app/colaboradores/services/vinculo.py:830`)
- Uma vigência inativa anterior que cobre D também pode ser encerrada pelo executor, mas não participa dessa busca de substituído. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:282`)
- Existindo vínculo **inativo na mesma data**, ele vence qualquer ativo que também será fechado ou absorvido. Há um objeto reaproveitado definido, mas isso não prova que sua flag é a escolha do período efetivamente substituído. (`app/colaboradores/services/vinculo.py:102`, `app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:255`, `app/colaboradores/services/vinculo.py:294`)

Consequência derivável: um futuro `6x1=True` pode fornecer a herança para um novo `6x1` anterior à sua vigência; um futuro de outro ciclo pode provocar recusa mesmo havendo uma vigência anterior `6x1` cobrindo D. São caminhos demonstráveis pelo código; ocorrência nos dados fornecidos é **[não verificado]**. (`app/colaboradores/services/vinculo.py:141`, `app/colaboradores/services/vinculo.py:185`, `app/colaboradores/services/vinculo.py:282`)

**`no_op → atualizar` não executa uma operação restrita à flag.** Ele percorre fechamento e absorção, redefine posto, ativa o vínculo, apaga `data_fim`, pode copiar calendário e recalibrar âncora. Essas operações são incondicionais no executor. (`app/colaboradores/services/vinculo.py:255`, `app/colaboradores/services/vinculo.py:294`, `app/colaboradores/services/vinculo.py:316`, `app/colaboradores/services/vinculo.py:319`, `app/colaboradores/services/vinculo.py:326`, `app/colaboradores/services/vinculo.py:351`)

A regeneração passada e futura é acionada, e a trilha técnica inclui a flag. Entretanto, o histórico classifica essa alteração como `ancora`, enquanto o impacto não recebe a nova flag e calcula o “depois” pela aritmética do template. Logo a prévia não representa fielmente uma alteração somente de feriado. (`app/colaboradores/services/vinculo.py:370`, `app/colaboradores/services/vinculo.py:380`, `app/colaboradores/services/vinculo.py:395`, `app/colaboradores/services/vinculo.py:418`, `app/escala/services/impacto_troca.py:22`, `app/escala/services/impacto_troca.py:40`)

**C. Chamadores da porta**

| Chamador | Passa a escolha? Efeito |
|---|---|
| Tela Vincular | Encaminha `request.POST`; os rádios enviam `'1'`/`'0'`, sem pré-seleção. (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/templates/colaboradores/partials/painel_vinculo.html:176`) |
| Chat: prévia | Não. `_post` monta template, início, modo e eventualmente âncora; a recusa retorna em `erros`. (`app/colaboradores/services/troca_pelo_chat.py:11`, `app/colaboradores/services/troca_pelo_chat.py:58`, `app/colaboradores/services/troca_pelo_chat.py:60`) |
| Chat: execução | Não; a interface nem recebe argumento de feriado. Novo vínculo sem substituído ou troca de ciclo para um ciclo sem padrão passa a ser recusado. Mesmo ciclo pode herdar. (`app/colaboradores/services/troca_pelo_chat.py:64`, `app/colaboradores/services/troca_pelo_chat.py:72`, `app/colaboradores/services/vinculo.py:185`) |
| Régua de defesa/realocar | Não. O dict omite a flag e captura a nova recusa. Destinos cíclicos já são recusados antes; destinos sem padrão precisam satisfazer a herança. (`app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:313`, `app/escala/regua_defesa.py:316`) |
| `censo_fase_12x36` | Não. O dict tem template/início/âncora; para destino 12x36 o juiz fornece `True`. (`app/ponto/management/commands/censo_fase_12x36.py:419`, `app/ponto/management/commands/censo_fase_12x36.py:422`, `app/escala/models.py:921`) |

Não encontrei chamador direto adicional por API/importação nas buscas estáticas; caminhos dinâmicos permanecem **[não verificado]**. O Django admin do vínculo é somente leitura. (`app/escala/admin.py:18`)

**D. Trechos, passado e competência exportada**

`criar_trecho_retroativo` realmente alimenta regeneração do passado: o comando cria o trecho e chama a porta até `min(fim, hoje)`, além do suplemento futuro. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:194`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:223`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:242`)

O novo `feriado_do_trecho` pode mudar a flag copiada: destino 5x2 recebe `False`; 12x36/24x48 recebe `True`; demais destinos recebem `bool(flag_original)`. Assim, também cria `12x36=True → 6x1=True` sem perguntar ao DP, embora a porta principal recuse troca de ciclo sem escolha. (`app/escala/models.py:921`, `app/escala/models.py:928`, `app/escala/models.py:934`, `app/colaboradores/services/vinculo.py:188`)

Por padrão, a regeneração consulta as competências protegidas e pula seus dias; células humanas também não são reescritas pelo ramo de regeneração. A autoridade considera TXT entregue e holerite publicado. (`app/ponto/portas/celula.py:221`, `app/ponto/portas/celula.py:284`, `app/ponto/portas/celula.py:297`, `app/folha/export.py:617`)

Existe uma exceção explícita preexistente: `corrigir_escala_retroativa --apesar-da-lavra` passa o motivo à porta, que esvazia a lista de proteção. Portanto “nunca toca exportada” não é verdade para todas as opções desse comando. Uso dessa exceção nesta obra é **[não verificado]**. (`app/colaboradores/management/commands/corrigir_escala_retroativa.py:42`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:233`, `app/ponto/portas/celula.py:218`)

Já os chamadores do ajuste automático trabalham somente no futuro: aplicação exige `inicio > hoje`; desfazer usa `max(inicio, hoje+1)` e regenera dali em diante. (`app/escala/services/escala_auto_executor.py:100`, `app/escala/services/escala_auto_executor.py:107`, `app/escala/services/escala_auto_executor.py:143`, `app/escala/services/escala_auto_executor.py:147`)

**E. Auditoria, autoria e idempotência**

Com `autor` autenticado, `_ReqDoAutor` fornece o usuário, `registrar_log` o encaminha como ator e `evento` o grava em `LogAuditoria.usuario`. Com `autor=None`, uma request corrente pode fornecer o usuário pelo contexto; fora dela, fica a identificação do processo, sem usuário humano. (`app/colaboradores/services/vinculo.py:19`, `app/colaboradores/services/vinculo.py:939`, `app/core/auditoria.py:24`, `app/core/observ.py:108`, `app/core/observ.py:118`)

`sincronizar_feriado_admin` chama sem autor e sem motivo. Não há uma segunda gravação explícita de auditoria nesse loop; a nova linha vem da porta. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:58`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/colaboradores/services/vinculo.py:939`)

Mesmo valor **na instância recebida** retorna antes de save, trilha e regeneração. A porta não relê nem trava o valor persistido antes dessa comparação; portanto isso não prova idempotência sob instância desatualizada ou concorrência. (`app/colaboradores/services/vinculo.py:932`, `app/colaboradores/services/vinculo.py:936`)

**Bloqueante 4 — trilha não é garantia da transação:** `registrar_log` captura falhas; `evento` usa savepoint e engole falha da própria auditoria. A flag pode confirmar sem `LogAuditoria`, apesar do `@atomic` da porta. (`app/colaboradores/services/vinculo.py:910`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)

O sincronizador também continua derivando a escolha dos ciclos sem padrão pelo cargo. Pode sobrescrever `6x1` sem resposta DP; não há bloqueio que o restrinja à “só lista” desta obra. Execução dele nos 82 é **[não verificado]**. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:51`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/escala/models.py:934`)

**F. Lote, reversão, atomicidade e relógio**

- **Universo:** vínculo que cobre hoje, colaborador ativo, ciclo 5x2 e flag `True`. Não é simplesmente `ativa=True`: pode incluir inativo com fim vigente e excluir ativo futuro. Igualdade com os “16 ativos” raspados é **[não verificado]**. (`app/escala/models.py:943`, `app/escala/models.py:945`, `app/escala/alimentacao.py:219`)
- **Lista:** compara exatamente universo e `--esperado`, antes de escrever. Imprime vínculo/colaborador/tipo/valor, mas não verifica publicação prévia no RELATO. Publicação da lista e do censo é **[não verificado]**. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:35`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:43`)
- **Reversão antes da escrita:** correto na ordem do programa. O JSON completo fecha antes do loop; modo `x` impede sobrescrita. É uma lista de valores anteriores, sem diário de progresso. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:55`)
- **Atomicidade:** por vínculo, não pelo lote. Queda após N commits deixa N aplicados e os demais intactos; a reversão contém todos. Repetir a lista original falha na igualdade do universo; reutilizar o arquivo falha no `open('x')`. Não há retomada automática. (`app/colaboradores/services/vinculo.py:910`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:38`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:51`)
- **Efeitos incompletos:** rejulgamento captura falhas; dinheiro e E2 têm efeitos após commit que também podem falhar sem desfazer a flag. Repetir `marcar(False)` depois retorna imediatamente, sem reparar esses efeitos. Contador zero não prova cura completa. (`app/ponto/portas/celula.py:475`, `app/ponto/portas/celula.py:540`, `app/ponto/services/fechamento.py:1818`, `app/colaboradores/services/vinculo.py:932`)
- **Exportadas:** a regeneração ordinária do lote mantém a guarda, pois não passa `apesar_da_lavra`. Entretanto, o `save()` também invalida o previsto dos fechamentos do colaborador, sem filtro de competência exportada: muda `previsto_em`/`atualizado_em`, embora isso não demonstre alteração monetária. (`app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:951`, `app/ponto/portas/celula.py:218`, `app/escala/signals.py:138`, `app/ponto/services/fechamento.py:1611`)
- **20/11:** o suplemento só regenera células futuras geradas **já existentes**, na janela calculada. Não cria uma célula ausente. As 16 células de 20/11 com `False` são **[não verificado]**. (`app/ponto/services/cadastro_futuro.py:26`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/services/cadastro_futuro.py:43`)

**Bloqueante 5 — relógio fora da borda:** o comando chama o contador duas vezes sem fornecer hoje; cada marcação lê outro hoje depois de salvar; o signal do save lê seu próprio hoje; o rejulgamento interno lê novamente. Uma passagem pela meia-noite pode usar datas distintas no mesmo lote. (`app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:36`, `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:57`, `app/escala/models.py:943`, `app/colaboradores/services/vinculo.py:944`, `app/ponto/signals.py:63`, `app/ponto/portas/celula.py:464`)

**G. Alcance real do selo AST**

| Construção | Resultado do scanner |
|---|---|
| `from ... import EscalaColaborador as EC`; `m.EscalaColaborador.objects.create` | Reconhece. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:34`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:44`) |
| `EC = EscalaColaborador`; manager guardado em variável; manager custom/`_default_manager` | Escapa: nascimento exige `.objects` diretamente no nome reconhecido. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`) |
| `_meta.model`, `__class__` | Escapa ao reconhecimento do modelo. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:44`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`) |
| Construtor reconhecido sem flag, seguido de `save()` | Pega o construtor. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:93`) |
| Instância carregada/copied/refresh, `pk=None`, `save()` | Escapa: não acompanha identidade ou `save()`. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`) |
| `qs.update(trabalha_em_feriado=...)`, queryset em variável | Pega; não precisa conhecer a origem do queryset. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`) |
| `qs.update(**dados)`, `bulk_update`, escrita por `__dict__` | Escapa às formas examinadas. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:108`) |

**Bloqueante 6 — procedência falsificável:** qualquer `v['trabalha_em_feriado']` é aceito, mesmo vindo de um dict literal; qualquer chamada chamada `feriado_do_trecho` também é aceita, sem conferir módulo ou autoridade. O selo não garante que o valor veio do veredito real. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:48`)

**O mutante literal morde:** retirar a keyword ou substituí-la por `True` no nascimento atual gera achado. O teste faz essas duas mutações; isso não cobre os escapes acima nem equivale a uma campanha de mutação do juiz. (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:137`, `app/escala/tests/test_feriado_nasce_da_escala_selo.py:143`)

O RED “col148” cria colaborador, vínculo e célula sintéticos. Verifica flag

[truncado em 20000 de 27330 caracteres]

## 13/11 10:50 · sessao 3dc36bb74794 · O168 · verificador · origem=Code

**APROVA o delta — sem bloqueantes demonstrados.** A referência nua deixou de contar como chamada, e os dois pontos de atribuição de erros passaram a usar o mesmo separador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`.)

**A. Contador e fila.** `qs_na_fila` aceita `_base`: repassa-o a `qs_vivos`, que usa esse queryset diretamente e aplica `VIVOS`; depois restringe a `COBRANCA` e exclui silêncio futuro. O juiz da fila é `chamados/services/painel.py::qs_na_fila`. (`app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:188`; `app/chamados/services/painel.py:190`.)

O significado **muda**: de recusados ainda vivos para recusados ainda cobrados na fila, excluindo `registrado` e silenciados no futuro. A docstring declara o universo novo. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`.)

O leitor executável localizado é `_estado` do selo, que publica o número como `segurados`; não localizei consumidor de placar. Há também uma referência documental em `_registrar_recusa`, sem chamada ao contador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:67`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/models.py:712`.) Consumidores externos à cópia: **[não verificado]**.

**B. Escritores e donos.** A entrada é `erros_proprios` da unidade de materialização mais `res['erros']` da disputa. O import por `models.py` reexporta a implementação de `materializacao.py`; não constitui outro escritor. (`app/chamados/services/validacao.py:486`; `app/chamados/services/validacao.py:492`; `app/chamados/services/validacao.py:493`; `app/chamados/models.py:4143`.)

Na tabela, **S(e)** significa todos os pks cuja marca `Pergunta #<pk>` era substring da mensagem — incluindo prefixos numéricos e perguntas citadas depois. A cura atribui exclusivamente ao primeiro identificador admissível. (`app/chamados/services/validacao.py:113`; `app/chamados/services/validacao.py:116`; `app/chamados/tests/test_bug29_30_log_validacao.py:117`; `app/chamados/tests/test_bug29_30_log_validacao.py:124`.)

| Escritor/caso que alimenta `erros` | Arquivo:linha | Dono antes → depois |
|---|---|---|
| `_ts_tipo_do_plantio`: sem data | `app/chamados/services/materializacao.py:352` | S(e) → `p.pk` |
| Resposta de escape | `app/chamados/services/materializacao.py:361` | S(e) → `p.pk` |
| Default do relógio sem marco correspondente | `app/chamados/services/materializacao.py:377` | S(e) → `p.pk` |
| Hora inválida, incluindo exceção | `app/chamados/services/materializacao.py:421` | S(e) → `p.pk` |
| `_declarar_ausencia_de_pergunta`: sem data | `app/chamados/services/materializacao.py:484` | S(e) → `p.pk` |
| `_materializar_resposta_hora`: sem resposta | `app/chamados/services/materializacao.py:843` | S(e) → `p.pk` |
| Vizinha do mesmo tipo/paridade | `app/chamados/services/materializacao.py:884` | S(e) → `p.pk` |
| Conflito devolvido por `registrar_batida` | `app/chamados/services/materializacao.py:942` | S(e) → `p.pk` |
| `_materializar_resposta_data_hora`: parse inválido | `app/chamados/services/materializacao.py:1010` | S(e) → `p.pk` |
| Conflito no plantio da âncora | `app/chamados/services/materializacao.py:1027` | S(e) → `p.pk` |
| Gate `DATA_EM_DISPUTA` | `app/chamados/services/materializacao.py:1201`; `app/chamados/services/materializacao.py:1205` | S(e) → `p.pk` |
| `_Reprovada`: não gerou batida | `app/chamados/services/materializacao.py:1245`; `app/chamados/services/materializacao.py:1290` | S(e) → `p.pk` |
| `_Reprovada`: C6 ainda considera viva | `app/chamados/services/materializacao.py:1285`; `app/chamados/services/materializacao.py:1290` | S(e) → `p.pk` |
| Outras exceções capturadas pela unidade | `app/chamados/services/materializacao.py:1291`; `app/chamados/services/materializacao.py:1292` | S(e) → `p.pk` |

Todos esses formatos colocam o identificador do dono antes dos conteúdos dinâmicos. Conflitos e exceções são embrulhados depois de `Pergunta #p.pk:`; portanto uma pergunta citada dentro deles não antecede a dona. (`app/chamados/services/materializacao.py:421`; `app/chamados/services/materializacao.py:943`; `app/chamados/services/materializacao.py:1010`; `app/chamados/services/materializacao.py:1028`; `app/chamados/services/materializacao.py:1292`.)

As particularidades são:

- **`__REUSO__Pergunta #p.pk`** escreve temporariamente em `erros`, mas o `finally` remove a mensagem dessa lista e a transfere para `reusos`. Se entregue diretamente ao helper, seria S(e) → `p.pk`. (`app/chamados/services/materializacao.py:872`; `app/chamados/services/materializacao.py:1297`; `app/chamados/services/materializacao.py:1301`; `app/chamados/services/validacao.py:106`.)
- **`DATA_EM_DISPUTA Pergunta #p.pk`** permanece reconhecido: o prefixo não contém outro identificador de pergunta. (`app/chamados/services/materializacao.py:1201`; `app/chamados/tests/test_bug29_30_log_validacao.py:136`.)
- **`DecisaoPergunta #N`** é escrito pelas exceções de imutabilidade de outra entidade. Isolado, antes casava com a pergunta N e seus prefixos; agora não tem dono. Capturado pela unidade, recebe antes o prefixo da pergunta processada. Ocorrência efetiva dessa exceção nesse caminho: **[não verificado]**. (`app/chamados/models.py:3961`; `app/chamados/models.py:3965`; `app/chamados/services/materializacao.py:1292`; `app/chamados/tests/test_bug29_30_log_validacao.py:134`.)
- **Sem pk:** se recebido, não era próprio de pergunta alguma e continua sem dono, entrando em `irmas`. Não identifiquei escritor direto desse formato no fluxo enumerado. (`app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:124`; `app/chamados/services/materializacao.py:1392`.)
- **Fallback de `erros_da_pergunta`:** é mensagem de saída, não novo insumo da materialização; abre pelo pk consultado e informa ausência de causa própria. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:145`.)

**C. Leitores de `irmas`.** Para mensagens com um único identificador legítimo, sem coincidência de prefixo, permanecem os mesmos itens, textos e ordem. O separador acrescenta o objeto original à lista escolhida. (`app/chamados/services/validacao.py:120`; `app/chamados/services/validacao.py:124`.)

Os leitores não refazem a partição: o log da recusa usa `erros_da_pergunta`; o log das irmãs concatena a lista; `traduzir_recusa` classifica motivos, sem conhecer pk; a tela consome esses resultados. (`app/chamados/services/validacao.py:519`; `app/chamados/services/validacao.py:532`; `app/chamados/catalogo/recusa.py:79`; `app/chamados/catalogo/recusa.py:81`; `app/chamados/services/acoes_disputa.py:34`; `app/chamados/services/acoes_disputa.py:59`.)

**Não há invariância total fora do prefixo numérico:** `Pergunta #5: ... Pergunta #7` deixa de ser próprio da #7. Essa mudança é deliberadamente cobrada pelo teste novo. Antes, a mensagem podia ser própria em duas consultas diferentes; dentro de uma mesma chamada, `in`/`not in` já produziam listas complementares. (`app/chamados/tests/test_bug29_30_log_validacao.py:125`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`; `app/chamados/tests/test_bug29_30_log_validacao.py:129`; `app/chamados/services/validacao.py:113`.)

**Ressalva do aviso:** `aviso_das_irmas` continua coletando todas as menções, não apenas donos. Recebendo aquele erro como irmã da #7, pode anunciar “1 pergunta” com IDs `#5, #7`, incluindo a própria #7. É uma inconsistência demonstrável com o exemplo sintético; ocorrência produtiva: **[não verificado]**. Esse coletor apresenta referências e não constitui outro particionador de erros. (`app/chamados/services/validacao.py:547`; `app/chamados/services/validacao.py:549`; `app/chamados/tests/test_bug29_30_log_validacao.py:127`.)

**D. Lookbehind.** Não perde nenhum escritor legítimo identificado. Veta somente letra ASCII imediatamente anterior; espaço, `_` e dígito são aceitos. Assim, `__REUSO__Pergunta`, `_Pergunta` e `3Pergunta` são reconhecidos; `DecisaoPergunta` é excluído. O uso de `search` encontra o primeiro token admissível, sem exigir início literal da string. (`app/chamados/services/validacao.py:106`; `app/chamados/services/validacao.py:116`; `app/chamados/tests/test_bug29_30_log_validacao.py:134`; `app/chamados/tests/test_bug29_30_log_validacao.py:135`.)

**E. Outro separador por substring.** Na varredura textual da cópia, não localizei outro sítio de produção com essa separação. Os dois pontos migrados delegam ao helper; o regex restante do aviso enumera menções. Ausência de variantes construídas dinamicamente: **[não verificado]**. (`app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`; `app/chamados/services/validacao.py:547`.)

**F. Classes de reprovação.** Não demonstrei novo juiz paralelo nem campo simultaneamente operando com dois significados: o contador consulta a fila existente, e a atribuição de erros tem implementação compartilhada. A mudança de universo do contador está declarada. (`app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`; `app/chamados/services/validacao.py:142`; `app/chamados/services/validacao.py:510`.)

O bloqueante da volta 4 foi curado: o fecho exige `ast.Call`, e o teste rejeita a auxiliar apenas referenciada. Permanece o limite anterior de aceitar uma chamada cujo resultado seja descartado; ele não foi introduzido pelo delta. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`.)

A catraca mantém a linha na identidade e `EXCECOES` vazio; as fixtures passam a fornecer os campos do juiz, delegar ao corpo canônico e zerar também o texto quando pretendem representar silêncio. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; `app/chamados/tests/e4b_c12_ast.py:222`; `app/ponto/tests/test_supra_juiz_executor.py:24`; `app/ponto/tests/test_supra_juiz_executor.py:46`; `app/chamados/tests/test_pep_reentrada.py:280`.)

Executei em memória os seis testes puros de atribuição e os três adversariais do AST: passaram. Restaurar a substring reprova os testes de prefixo e segunda citação; restaurar o fecho por referência nua reprova a isca. Há mordida nessas curas. (`app/chamados/tests/test_bug29_30_log_validacao.py:104`; `app/chamados/tests/test_bug29_30_log_validacao.py:117`; `app/chamados/tests/test_bug29_30_log_validacao.py:124`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:369`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`.)

Execução integral Django/ORM, banco, logs e identidade Git do HEAD: **[não verificado]**.

## 13/11 10:48 · sessao 85aeb4ade445 · - · - · origem=Ronald (tmux)

APROVA no escopo do delta: o exemplo multipergunta testa deliberadamente a propriedade exclusiva do primeiro identificador; `irmas` não é invariável nesses casos. (`app/chamados/services/validacao.py:116`; `app/chamados/services/validacao.py:123`.)

O aviso que inclui a própria pergunta merece ressalva, mas, sem escritor real alcançável produzindo esse formato, não demonstra bloqueante atual; ocorrência produtiva: [não verificado].

## 13/11 10:47 · sessao 259a91e99da0 · - · - · origem=Ronald (tmux)

G — Selo AST

- Executei somente a função `achados` em memória, sem Django, banco ou arquivos temporários: árvore de produção retornou `[]`; isso confirma apenas o alcance implementado pelo scanner, que percorre `.py` e exclui testes/migrations/logs (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:28`, `:77`).
- Apelido de import de classe (`from ... import EscalaColaborador as EC`) e módulo (`m.EscalaColaborador`) são reconhecidos; atribuição posterior `EC = EscalaColaborador` escapa (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:34`, `:44`).
- Escapam `manager = EscalaColaborador.objects; manager.create(...)`, manager custom/`_default_manager`, `ec._meta.model.objects.create(...)` e `ec.__class__.objects.create(...)`: o nascimento exige `.objects` diretamente ligado ao nome reconhecido (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:90`).
- `qs.update(trabalha_em_feriado=True)` é pego mesmo com queryset em variável; `qs.update(**dados)`, `bulk_update`, alteração por `__dict__`, cópia/refresh com `pk=None` seguida de `save()` escapam. O scanner só examina keyword literal de `update`, `setattr` com string literal e atribuição direta ao atributo; não rastreia `save`, dados nem identidade da instância (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:102`).
- Construtor reconhecido sem flag é pego antes do `save`; construtor aceito seguido de `save` não tem sua procedência revalidada (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:93`).
- A fonte também é falsificável: `v = {'trabalha_em_feriado': True}` ou `evil.feriado_do_trecho(...)` passam fora da porta. Aceitação compara sintaxe/nome, sem provar que `v` veio de `julgar_vinculo` ou que a função pertence ao modelo (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:48`).
- O mutante literal pedido morde: retirar o keyword ou trocá-lo por `True` na âncora da criação produz achado; isso foi confirmado também em memória. O teste existente faz essas duas substituições, mas não muta o juiz para retornar True em 5x2 nem testa os escapes acima (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:137`).
- Portanto há mordida real e cobertura incompleta, não selo completamente vacuoso (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:143`, `:90`, `:102`).

H — Semântica e leitores

**Bloqueante:** a tela exige resposta para personalizado/intermitente, mas a resposta “Não” não torna o feriado folga nesses ciclos. Só 6x1/5x2/semanal integram `CICLOS_FERIADO_VIRA_FOLGA`; o predicado retorna False para os demais independentemente da flag. A precedência mantém trabalho previsto nesses feriados por `not ec.feriado_vira_folga(data)` (`app/templates/colaboradores/partials/painel_vinculo.html:175`, `app/escala/models.py:32`, `:1420`, `app/ponto/precedencia.py:376`).

- 6x1/semanal: a escolha humana tem efeito consistente sobre o previsto e sobre a partição de pagamento, porque os consumidores consultam `feriado_vira_folga`; não encontrei consumidor que leia o `default` do campo para decidir (`app/escala/models.py:1399`, `:1420`, `app/ponto/precedencia.py:377`, `app/ponto/services/fechamento.py:949`, `app/ponto/calculador/alimentacao.py:521`).
- 24x48 passa a nascer True, porém o predicado já ignora sua flag por não ser weekday; True/False não altera essa supressão do feriado. O código adiciona 24x48 à resposta obrigatória True do juiz de nascimento (`app/escala/models.py:906`, `:921`, `:1422`).
- `feriado_do_posto` não lê a flag do vínculo: usa `Posto.opera_em_feriado`; posto fechado veta cobrança inclusive de vínculo que trabalha em feriado. Há veto explícito adicional depois da consulta à precedência, portanto “flag True” não garante cobrança (`app/colaboradores/services/feriado_do_posto.py:67`, `app/ponto/management/commands/detectar_ausencias.py:617`, `:619`, `:629`).
- Contador antigo continua classificando 5x2/6x1 True em “declarados” conforme o posto, embora agora 6x1 True possa resultar de escolha humana independente do posto. É indicador com critério anterior preservado (`app/escala/models.py:950`, `:959`, `:962`).
- O contador novo existe como método/comando, mas o placar continua chamando somente o antigo; a integração do contador novo ao ESTADO/copiloto não foi encontrada na cópia. O ponto concreto de integração existente é `bin/placar_code.sh:298`; registro novo só declara a “casa” no placar (`app/config/crons.py:1227`). Publicação/exibição efetiva do novo contador: [não verificado].

Inventário direto de leituras da flag em produção:

| Uso | Referências |
|---|---|
| Foto para trilha | `app/colaboradores/services/vinculo.py:43` |
| Comparação no_op / herança | `app/colaboradores/services/vinculo.py:145`, `:187` |
| Idempotência / antes da mudança | `app/colaboradores/services/vinculo.py:932`, `:934` |
| Cópias para trechos | `app/colaboradores/services/vinculo.py:1021`, `:1139`, `:1161` |
| Contadores | `app/escala/models.py:945`, `:959` |
| Supressão do previsto no feriado | `app/escala/models.py:1420` |
| Lote: listagem / reversão | `app/escala/management/commands/aplicar_feriado_nasce_da_escala.py:44`, `:52` |
| Sincronização: comparação / listagem | `app/colaboradores/management/commands/sincronizar_feriado_admin.py:53`, `:57` |
| Entrada humana de UI | `app/templates/colaboradores/partials/painel_vinculo.html:176` |

Leituras indiretas relevantes:

| Consumidor | Referências |
|---|---|
| Geração/regeneração de célula | `app/ponto/portas/celula.py:289`, `:911` |
| Precedência | `app/ponto/precedencia.py:366`, `:377` |
| Fechamento | `app/ponto/services/fechamento.py:949` |
| Calculador diário | `app/ponto/calculador/alimentacao.py:521`, `app/ponto/calculador/regras.py:684` |
| Rubrica de exportação, sem leitura direta da flag | `app/folha/export.py:420`, `:422` |

I — Classes presentes/limites

- Campo com significado incompatível com a pergunta da UI para personalizado/intermitente: presente e demonstrável pelo fluxo acima (`app/templates/colaboradores/partials/painel_vinculo.html:175`, `app/escala/models.py:1422`).
- Selo com escapes: presente; o mutante literal morde, mas a alegação de impedir todo nascimento/escrita fora da porta excede a implementação (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:4`, `:90`, `:102`).
- O RED “col148” é fixture sintética, não replay do EC854: cria outro colaborador/vínculo e célula artificial; verifica flag, célula e tipo de dia, mas não uma cobrança/veredito previamente lavrado de produção nem 20/11 (`app/escala/tests/test_feriado_padrao_por_escala.py:188`, `:198`, `:230`). Resultado real col148, suite completa/opus, publicação de censo, lista dos 16, deploy e apply: [não verificado].
- Nenhuma ocorrência de `FERIADO-NASCE-DA-ESCALA`/contador novo foi encontrada em `app/docs`, `CLAUDE.md` ou `bin`; isso não prova inexistência de relato fora da cópia. Censo e listas publicados: [não verificado]. [nao verificado]
- Não executei os testes existentes porque os testes de selo escrevem arquivos temporários e os funcionais escrevem banco, contrariando a regra de somente leitura (`app/escala/tests/test_feriado_nasce_da_escala_selo.py:115`, `app/escala/tests/test_feriado_padrao_por_escala.py:198`).

## 13/11 10:47 · sessao 85aeb4ade445 · - · - · origem=Ronald (tmux)

A: `qs_na_fila` aceita `_base` e entrega a interseção desse queryset com `VIVOS`, `COBRANCA` e ausência de silêncio futuro; `_base` substitui a montagem padrão, sem exigir as anotações do painel. O juiz da fila é `chamados/services/painel.py::qs_na_fila`. (`app/chamados/services/painel.py:153`; `app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:158`; `app/chamados/services/painel.py:188`; `app/chamados/services/painel.py:189`; `app/chamados/services/painel.py:190`.)

O contador muda de “recusados ainda vivos” para “recusados ainda cobrados na fila”: exclui `registrado` e silenciados no futuro. `VIVOS` contém `registrado`, enquanto `COBRANCA` contém somente `aberto` e `em_analise`. A nova docstring declara esse universo. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/juizes.py:2512`; `app/chamados/juizes.py:2525`.)

O leitor executável encontrado é `_estado`, no selo O168, que publica o retorno como `segurados`; as expectativas continuam sendo zero/um nos cenários existentes. A outra ocorrência produtiva é documentação de `_registrar_recusa`, sem chamada ao contador. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:67`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:75`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:122`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:159`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:187`; `app/chamados/models.py:712`.)

F/pre-opus: o bloqueante da volta 4 foi curado: o fecho agora segue somente `ast.Call` com função nominal definida no módulo; a referência nua `_isca` deixou de contar. Rodei por extração AST, exclusivamente em memória, os três métodos adversariais: passaram. No fonte atual, obtive `raw=False`, `judge=True`, fecho composto por `perguntas_que_seguram_csf` e `pode_executar_csf`; a isca deu `judge=False`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:288`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:290`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:357`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:360`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:369`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:389`; `app/ponto/supra_juiz.py:558`; `app/ponto/supra_juiz.py:559`; `app/ponto/supra_juiz.py:573`.)

Permanece o limite já descrito na volta 4: uma chamada ao juiz cujo resultado é descartado ainda satisfaz `_pergunta_ao_juiz`; o teste exige existência da chamada, sem vínculo com o resultado. Reproduzi `ch.sem_veredito_entre(); return True` dando `judge=True`. Isso não foi introduzido pela alteração de `ast.Name` para `ast.Call`. (`app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:332`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:334`; `app/chamados/tests/test_o168_pai_com_conversa_no_gate.py:336`; dado fornecido, preverif4.r, item C.)

As nove entradas de `CORPO` correspondem às chamadas e linhas atuais; a mudança atualiza a identidade de linha, sem adicionar exceções. O scanner usa a tupla completa e `EXCECOES` continua vazio. (`app/chamados/tests/e4b_c12_ast.py:18`; `app/chamados/tests/e4b_c12_ast.py:29`; `app/chamados/tests/e4b_c12_ast.py:222`; `app/chamados/tests/e4b_c12_ast.py:223`; `app/chamados/models.py:199`; `app/chamados/models.py:296`; `app/chamados/models.py:450`; `app/chamados/models.py:655`; `app/chamados/models.py:835`; `app/chamados/models.py:853`; `app/chamados/models.py:1489`; `app/chamados/models.py:1666`; `app/chamados/models.py:1686`.)

A fixture do executor fornece os campos reais do juiz e `_D` delega ao corpo canônico, sem copiar seu predicado. Rodei seus seis testes em memória com as funções reais extraídas por AST e módulos sintéticos somente para resolver imports e o patch da tranca: passaram. Execução Django integral: [não verificado]. (`app/ponto/tests/test_supra_juiz_executor.py:24`; `app/ponto/tests/test_supra_juiz_executor.py:45`; `app/ponto/tests/test_supra_juiz_executor.py:58`; `app/ponto/tests/test_supra_juiz_executor.py:62`; `app/ponto/tests/test_supra_juiz_executor.py:79`; `app/chamados/models.py:1899`.)

A fixture PEP agora zera também `resposta_colab`, coerentemente com o juiz que considera texto sem carimbo uma resposta. O teste preserva controles da irmã carimbada e da pergunta em voo sem escrita. (`app/chamados/tests/test_pep_reentrada.py:280`; `app/chamados/tests/test_pep_reentrada.py:294`; `app/chamados/tests/test_pep_reentrada.py:296`; `app/chamados/models.py:1900`; `app/chamados/models.py:1901`.)

Não demonstrei bloqueante novo em A nem nessa parte de F: o contador passou a consultar o juiz existente e a fixture passou a delegar ao predicado existente. (`app/chamados/juizes.py:2525`; `app/ponto/tests/test_supra_juiz_executor.py:46`.)

## 13/11 10:47 · sessao 5f5a2ad0669a · - · - · origem=Ronald (tmux)

A/C/E — achados confirmados pela cópia:

- **Bloqueante: troca retroativa deixa flag incompatível.** `corrigir_vinculo_retroativo` troca `tipo_escala` e salva apenas tipo/âncora/fim, sem consultar juiz nem atualizar flag. Assim um vínculo 6x1 True corrigido para 5x2 continua True. O comando chama essa função na correção “no lugar”. (`app/colaboradores/services/vinculo.py:1024`, `app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`, `app/colaboradores/management/commands/corrigir_escala_retroativa.py:212`)

- **Bloqueante: recusa do aceite não restaura a flag.** O snapshot guarda pk/tipo/início/fim/ativa, sem `trabalha_em_feriado`; a reversão restaura tipo sem flag. Caso concreto derivável: reaproveitar vínculo 5x2 False como 12x36 True em modo aceite; recusar restaura 5x2, mas deixa True. Troca somente da flag também não é desfeita pela recusa. (`app/colaboradores/services/vinculo.py:108`, `app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`, `app/colaboradores/services/vinculo.py:1078`, `app/chamados/services/acoes_chamado.py:143`, `app/chamados/services/acoes_chamado.py:149`)

Inventário de nascimento encontrado na produção, inclusive procurando aliases de importação por AST em memória:

| Sítio | Origem da flag |
|---|---|
| `executar_vinculo`, criação | `v['trabalha_em_feriado']`, vindo de `_feriado_do_veredito`. (`app/colaboradores/services/vinculo.py:144`, `app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:338`) |
| `executar_vinculo`, reaproveitamento | Recebe o mesmo valor do veredito. (`app/colaboradores/services/vinculo.py:315`, `app/colaboradores/services/vinculo.py:317`) |
| `criar_trecho_retroativo` | `feriado_do_trecho(tipo_destino, flag_original)`. (`app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1021`) |
| `ajustar_vinculo_pelo_sistema` | `feriado_do_trecho(tipo_derivado, flag_original)`. (`app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1139`) |
| `desfazer_ajuste_do_sistema`, renascimento | `feriado_do_trecho(tipo_antigo, flag_antiga)`. (`app/colaboradores/services/vinculo.py:1158`, `app/colaboradores/services/vinculo.py:1161`) |
| `desfazer_ajuste_do_sistema`, reabertura | Reativa o antigo e mantém a flag antiga sem juiz. (`app/colaboradores/services/vinculo.py:1149`, `app/colaboradores/services/vinculo.py:1154`) |
| `corrigir_vinculo_retroativo`, troca tipo | Mantém flag anterior sem juiz — falha acima. (`app/colaboradores/services/vinculo.py:1027`, `app/colaboradores/services/vinculo.py:1030`) |
| `reverter_para_snapshot`, troca tipo | Mantém flag atual sem restaurar flag anterior — falha acima. (`app/colaboradores/services/vinculo.py:1078`) |

Nos quatro `create` encontrados, nenhum usa diretamente o default. O default ainda existe como True, sem enforcement no modelo para impedir caminhos não capturados pelo selo. (`app/colaboradores/services/vinculo.py:329`, `app/colaboradores/services/vinculo.py:1017`, `app/colaboradores/services/vinculo.py:1136`, `app/colaboradores/services/vinculo.py:1158`, `app/escala/models.py:896`)

`feriado_do_trecho` trata qualquer booleano herdado como “escolha humana”, mas não consulta prova da escolha: para juiz None retorna `bool(escolha)`. Portanto legado que nasceu pelo default pode ser reproduzido como escolha. (`app/escala/models.py:928`, `app/escala/models.py:933`)

Chamadores de `julgar_vinculo`/`executar_vinculo`:

| Chamador | Passa flag? Efeito |
|---|---|
| Tela Vincular | Encaminha `request.POST`; template tem radios 1/0 sem pré-seleção. (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/templates/colaboradores/partials/painel_vinculo.html:176`, `app/templates/colaboradores/partials/painel_vinculo.html:177`) |
| Chat, prévia | Não: `_post` só monta template/início/modo/âncora; erro vira lista `erros`. (`app/colaboradores/services/troca_pelo_chat.py:11`, `app/colaboradores/services/troca_pelo_chat.py:58`, `app/colaboradores/services/troca_pelo_chat.py:59`) |
| Chat, execução | Não; mesma montagem. Novo vínculo ou troca para ciclo sem padrão passa a ser recusado; mesmo ciclo pode herdar. Não há argumento nessa interface para fornecer escolha. (`app/colaboradores/services/troca_pelo_chat.py:64`, `app/colaboradores/services/troca_pelo_chat.py:72`, `app/colaboradores/services/troca_pelo_chat.py:75`, `app/colaboradores/services/vinculo.py:177`) |
| Régua/realocar | Não: dict sem flag. Recusa captura ValueError; mesma restrição em novos/novos ciclos sem padrão. (`app/escala/regua_defesa.py:313`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:317`, `app/escala/regua_defesa.py:321`) |
| Comando `censo_fase_12x36` | Não: dict tem template/início/âncora; 12x36 recebe True do juiz. (`app/ponto/management/commands/censo_fase_12x36.py:419`, `app/ponto/management/commands/censo_fase_12x36.py:422`, `app/ponto/management/commands/censo_fase_12x36.py:423`, `app/escala/models.py:921`) |

Chamador direto adicional por API/importação não encontrado nas buscas estáticas; caminhos dinâmicos permanecem [não verificado]. O Django admin de vínculo é somente leitura. (`app/escala/admin.py:18`)

E:

- Mesmo valor em memória retorna antes de save/regeneração/log. Não há releitura/trava do valor persistido para validar idempotência com instância stale ou concorrência. (`app/colaboradores/services/vinculo.py:932`, `app/colaboradores/services/vinculo.py:935`)
- `autor` autenticado ganha `_ReqDoAutor.user`, chega em `evento(... ator=request.user)` e vira `LogAuditoria.usuario`. (`app/colaboradores/services/vinculo.py:19`, `app/colaboradores/services/vinculo.py:939`, `app/core/auditoria.py:24`, `app/core/observ.py:118`, `app/core/observ.py:135`)
- `autor=None`: dentro de request, ContextVar fornece usuário; fora de request, usuário fica None e `ator` identifica o processo/comando. (`app/core/observ.py:108`, `app/core/observ.py:113`, `app/core/observ.py:117`, `app/core/observ.py:123`)
- `sincronizar_feriado_admin` chama sem autor e sem motivo; não tem log próprio nessa aplicação, portanto não encontrei duplicação explícita. Ele continua podendo alterar 6x1 pelo cargo, sem resposta humana DP: o juiz None leva ao booleano do cargo. (`app/colaboradores/management/commands/sincronizar_feriado_admin.py:51`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:61`, `app/escala/models.py:934`)
- Auditoria é best-effort: falha pode ser engolida e a flag ainda confirmar. `@atomic` não garante que a trilha exista se seu escritor engoliu a falha. (`app/colaboradores/services/vinculo.py:910`, `app/core/auditoria.py:17`, `app/core/auditoria.py:25`, `app/core/observ.py:105`, `app/core/observ.py:133`, `app/core/observ.py:139`)
- Relógio oculto: `ec.save` dispara `_nucleo_post_save_escala` e `_recompute_escala`, que lê outro `timezone.localdate()` antes do `hoje` da porta; portanto não há efetivamente um único relógio no ato completo. (`app/colaboradores/services/vinculo.py:936`, `app/colaboradores/services/vinculo.py:944`, `app/ponto/signals.py:63`, `app/ponto/signals.py:72`)

_4552 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
