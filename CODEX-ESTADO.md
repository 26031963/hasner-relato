# CODEX-ESTADO

> Ultima resposta processada: **16/11/2026 15:50** · publicado em **16/11/2026 15:50** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 16/11 15:50 · sessao de3443daed46 · O50-CELULA-APOS-DESLIGAMENTO · verificador · origem=Code

**O item 1 precisa voltar para fechar o alcance temporal, a associação das retratadas ao turno e a prova do efeito novo.** A operação existente muda conforme o dia, e as funções públicas de associação de batidas excluem retratadas. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`; `app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2942`.)

Identidade Git da cópia com o HASH informado, execução em produção e hashes antes/depois: [nao verificado].

**1. Premissas da operação e das entradas**

A operação é `regenerar_celulas_vinculo`, dentro da operação registrada `gerar` da porta canônica de `CelulaDia`; o command `gerar_celulas.py` apenas a reexporta. Chamá-la pelas portas de desligamento/vínculo preserva o escritor único. (`app/ponto/portas/celula.py:74`; `app/ponto/portas/celula.py:112`; `app/core/portas.py:112`; `app/ponto/management/commands/gerar_celulas.py:13`.)

| Dia fora da vigência | O que a porta grava hoje |
|---|---|
| **Futuro civil: `d > hoje`** | Conserva a linha; grava `trabalha=False`, `escala_geradora=None`, DNA sem vínculo, `dna_anterior`, `regenerada_em` e incremento de `regeneracoes`; chama o desfazimento do julgamento futuro. Exige origem gerada, geradora igual ao vínculo, competência não bloqueada e nenhum outro vínculo cobrindo o dia. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:267`.) |
| **Hoje/passado: `d <= hoje`** | Conserva a linha e a geradora; grava **somente** `trabalha=False` e `montar_dna(ec,d,False)`. Esse DNA mantém `vinculo_id=ec.id`; o ramo não grava os três campos de denúncia. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:277`; `app/ponto/management/commands/gerar_celulas.py:104`.) |

Portanto, “a operação que já existe grava DNA sem vínculo” confere **apenas para o ramo futuro civil**, não para qualquer dia posterior ao fim. “Após D” e “após hoje” são limites diferentes no código. (`app/ponto/portas/celula.py:229`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

Os caminhos já existem:

- `executar_desligamento` → `fechar_vinculos_ativos` → `fechar_vigencia`: fecha por `update()` e chama a regeneração de futuras existentes com `recalculo=False`. O desligamento agendado retorna antes desse caminho. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:126`; `app/colaboradores/services/desligamento.py:147`; `app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:662`; `app/colaboradores/services/vinculo.py:862`.)
- `encerrar_vinculo`: salva `ativa=False/data_fim`; o signal regenera hoje/passado e chama o suplemento das futuras. Já há efeito sobre células nessa entrada. (`app/colaboradores/services/vinculo.py:843`; `app/escala/signals.py:73`; `app/escala/signals.py:80`.)

Esses caminhos **não entregam automaticamente as novas exclusões**: os ramos fora da vigência não consultam batida, afastamento ou `competencia_ja_paga`; usam a guarda atual de lavra. (`app/ponto/portas/celula.py:218`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

A justificativa sobre leitores precisa ser estreitada: sem célula, `eh_dia_trabalho` consulta a **aritmética do vínculo**, não diretamente o template; `marcos_do_dia` consulta o template tanto sem célula quanto com `dna.marcos` vazio. Isso prova a possibilidade de retorno ao cálculo/template, não que “o previsto voltaria” em todo caso: o juiz de minutos retorna zero sem vínculo ou quando o dia não trabalha. (`app/escala/models.py:1094`; `app/escala/models.py:1096`; `app/escala/models.py:1124`; `app/escala/models.py:1133`; `app/escala/utils.py:2260`; `app/escala/utils.py:2266`.)

**2. Conferência literal das condições (1) a (5)**

**(1) Não paga.** `competencia_ja_paga` está registrada como obra, incluindo exportação vigente ou pagamento externo por cadastro; implementação disponível nessa cópia: [nao verificado]. O registro condiciona o pouso à espinha e exige migração conjunta dos leitores. Esperar esse pouso fecha como dependência futura; não autoriza substituí-lo pelos juízes atuais. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

Hoje a célula usa `_competencias_exportadas`, que enumera candidatos e pergunta a `competencia_fechada_para_celula`; esta compõe exportação vigente e holerite publicado da pessoa. Isso não constitui prova de pagamento externo cadastrado. (`app/ponto/portas/celula.py:221`; `app/ponto/management/commands/gerar_celulas.py:155`; `app/ponto/management/commands/gerar_celulas.py:181`; `app/folha/export.py:618`; `app/folha/export.py:622`.)

**(2) Protegidos.** Editada e outro vínculo têm guardas existentes nos dois ramos. `_outro_vinculo_cobre` mora no **command**, sendo importada pela porta; consulta outros vínculos e delega a cobertura a `vinculo_cobre_a_data`. (`app/ponto/portas/celula.py:142`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`; `app/ponto/management/commands/gerar_celulas.py:269`; `app/ponto/management/commands/gerar_celulas.py:283`.)

Excluir passivo não resolve sozinho o caso de **novo encerramento com fim passado**: a porta aceita `data_fim` validada contra o início, e o signal alcança dias até hoje. O contrato precisa dizer se esses dias entram no efeito novo; se entrarem, o ramo atual não fornece DNA sem vínculo nem denúncia da reescrita. (`app/colaboradores/services/vinculo.py:835`; `app/escala/signals.py:60`; `app/escala/signals.py:73`; `app/ponto/portas/celula.py:272`.)

**(3) Qualquer batida, pelo turno OU pelo civil.** As autoridades existentes respondem perguntas diferentes:

| Associação | Função e limite |
|---|---|
| Dia civil local | `data_local(timestamp)` usa `timezone.localtime(...).date()`. (`app/ponto/turnos.py:45`.) |
| Batidas reclamadas pelo turno | `batidas_do_dia_turno` lê `turnos_do_colab` e seleciona `data_turno == dia`; inclui a cauda noturna, mas exclui retratadas por construção. (`app/ponto/turnos.py:3096`; `app/ponto/turnos.py:3107`; `app/ponto/turnos.py:3115`.) |
| Dia de cada batida, incluindo órfã viva | `dia_das_batidas` entrega o dia do turno quando reclamado; para as demais, entrega o civil. **Não entrega os dois dias.** (`app/ponto/turnos.py:2898`; `app/ponto/turnos.py:2961`; `app/ponto/turnos.py:2967`.) |
| Retratada ligada ao turno | `dia_das_batidas` filtra retratadas e `dia_do_turno_da_batida` declara `None` para elas. Associação da retratada noturna à véspera: [nao verificado]. (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2939`.) |

A condição exige uma **união explícita**: uma saída de madrugada pode proteger tanto o dia do turno quanto seu dia civil. Consultar apenas `dia_das_batidas`, ou juntar turno vivo com cru civil, não prova a proteção da véspera para uma retratada. Essa associação precisa fechar sem clonar a geometria. (`app/ponto/turnos.py:2965`; `app/ponto/turnos.py:2967`; `app/ponto/turnos.py:2942`.)

**(4) Afastado.** Existe `para='cobranca'`, mas **não existe argumento `data=`**. A chamada real é `afastado_hoje(colab, dia=dia_celula, para='cobranca')`; retorna `Ausencia` ou `None`. Cobrança exclui apenas rejeitadas, e o afastamento consulta atualmente `afastamento_inss`. (`app/ponto/turnos.py:2737`; `app/ponto/turnos.py:2743`; `app/ponto/turnos.py:2530`; `app/ponto/turnos.py:2716`.)

**(5) Hash igual no efeito novo.** O hash existente `hash_do_gravado` cobre **somente `FechamentoMensal`**, com exclusões declaradas, incluindo carimbos de controle. Não cobre `CelulaDia` nem `DiaPago`; sozinho não certifica a condição composta. (`app/ponto/portas/regen_exportada.py:37`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/portas/regen_exportada.py:62`; `app/ponto/portas/regen_exportada.py:71`.)

Para provar o **efeito novo**, o brief precisa definir os campos/linhas protegidos e comparar o mesmo ato com e sem o novo efeito, incluindo callbacks após commit. Comparar simplesmente antes/depois de `encerrar_vinculo` mistura a obra com o comportamento anterior: o `save()` já invalida previsto sem filtro de competência, e a regeneração pode agendar recálculo. (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1607`; `app/ponto/portas/celula.py:483`; `app/ponto/services/fechamento.py:1824`.) Resultado dessa prova: [nao verificado].

**3. Cortes, dinheiro e retroatividade**

Há uma correção na pergunta: **LU-13 não contém “remover”** no texto disponível; fixa direção, “futuras = após D”, exceções e `!` retroativo. **“Remover com trilha” está na LU-14.** Conservar a linha respeita as exclusões da LU-13; equivaler a “remover” da LU-14 não está demonstrado no literal. Essa é a pergunta de interpretação que o próprio complemento manda levantar, sem presumir equivalência. (`app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

Se o efeito novo alcançar hoje/passado, reutilizar o ramo atual sem alteração confronta a exigência de denúncia por `regenerada_em/regeneracoes/dna_anterior`. A auditoria geral da regeneração também captura falha e segue; sua existência não garante gravação obrigatória da trilha em todo ato. (`CLAUDE.md:149`; `CLAUDE.md:152`; `app/ponto/portas/celula.py:277`; `app/ponto/portas/celula.py:369`; `app/ponto/portas/celula.py:388`.)

A proibição de resolver cadastro/vínculo futuro por código está alinhada à L-099; a porta atual preserva os vínculos que começam depois do fim informado. (`app/docs/LEIS.md:122`; `app/colaboradores/services/vinculo.py:665`.)

**Dinheiro é alcançável pelo caminho escolhido**, mesmo sem tocar `motor_calculo_v2.py`: o rabo da regeneração chama recálculo por competência; este grava `FechamentoMensal` e lavra `DiaPago`. A lavratura pode apagar e recriar todas as linhas daquela competência/versão. Logo, o alcance financeiro não fica limitado fisicamente à célula-dia. (`app/ponto/portas/celula.py:483`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1200`; `app/ponto/services/dia_pago.py:359`.)

A regra consolidada exige reversão anterior, hash de exportada intacta e prova de frota para dinheiro não exportado; mantém `!` para paga/vínculo ou escala retroativo e caminho próprio para alteração de exportada. Reutilizar uma porta existente não dispensa essas condições. (`CLAUDE.md:404`; `CLAUDE.md:406`; `CLAUDE.md:411`.)

**4. Quem escreve e quem lê os campos e funções envolvidos**

O mapa abaixo identifica consumidores concretos; exaustividade do futuro censo previsto no PREPARA: [nao verificado].

| Campo/função | Quem escreve ou produz | Quem lê |
|---|---|---|
| `Colaborador.situacao`, `data_demissao` | `executar_desligamento`, pela porta do colaborador. (`app/colaboradores/services/desligamento.py:129`.) | Situação exibida lê desligamento; grade lê a borda da demissão. (`app/colaboradores/services/situacao_exibida.py:56`; `app/escala/services/leitor_celula.py:389`.) |
| Vínculo: `data_inicio`, `data_fim`, `ativa`; cobertura do sucessor | Encerramento escreve `ativa/data_fim`; criação/alteração de `data_inicio` no escopo novo: [nao verificado]. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:843`.) | Signal compara vigência; `_outro_vinculo_cobre` chama `vinculo_cobre_a_data`. (`app/escala/signals.py:25`; `app/escala/signals.py:56`; `app/ponto/management/commands/gerar_celulas.py:283`; `app/escala/alimentacao.py:214`.) |
| `CelulaDia.colaborador/data/trabalha/escala_geradora/dna` | Nascimento e regeneração na porta; DNA produzido por `montar_dna`. (`app/ponto/portas/celula.py:293`; `app/ponto/portas/celula.py:259`; `app/ponto/management/commands/gerar_celulas.py:73`.) | Trabalho: `eh_dia_trabalho`; geradora: `vinculo_do_dia`; DNA/marcos: `marcos_do_dia`; cobrança: `dia_sem_trabalho`. (`app/escala/models.py:1094`; `app/escala/alimentacao.py:270`; `app/escala/models.py:1124`; `app/chamados/juizes.py:459`.) |
| DNA sem vínculo: `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `marcos`, `minutos_jornada`, `intervalo_livre`, `lampadas` | `montar_dna(None,...)` produz essas chaves nulas. (`app/ponto/management/commands/gerar_celulas.py:80`.) | Reversão lê `vinculo_id/marcos`; leitor de marcos lê `dna.marcos`; consumo exaustivo das demais chaves: [nao verificado]. (`app/ponto/portas/celula.py:829`; `app/ponto/portas/celula.py:832`; `app/escala/models.py:1124`.) |
| `origem`, `editada_em`, `editada_por` | `editar_dia`. (`app/ponto/portas/celula.py:638`; `app/ponto/portas/celula.py:14`.) | Regeneração protege origem não gerada; juiz de reescrita lê `editada_em`. (`app/ponto/portas/celula.py:251`; `app/escala/services/regeneracao.py:84`.) |
| `dna_anterior`, `regenerada_em`, `regeneracoes` | Regeneração futura e regeneração comum. (`app/ponto/portas/celula.py:262`; `app/ponto/portas/celula.py:307`.) | Reversão lê DNA anterior e contador; cobrança órfã lê carimbos de reescrita. (`app/ponto/portas/celula.py:818`; `app/ponto/portas/celula.py:835`; `app/escala/services/regeneracao.py:87`.) |
| Julgamento: `ata`, `veredito/via/em`, `protestos`, `status_dia/rotulo`; carimbos `julgada_em/impressao/ausencia/tem_entrada/tem_saida/insumos_em` | Operações `lavrar_veredito/carimbar`; desfazimento futuro zera a lista, preservando decisão via admin. (`app/ponto/portas/celula.py:1096`; `app/ponto/portas/celula.py:1112`; `app/ponto/portas/celula.py:1155`; `app/ponto/portas/celula.py:1165`.) | Grade lê ata/veredito; juiz de cobrança lê ata/trabalho; desfazimento lê os campos e a via. (`app/escala/services/leitor_celula.py:397`; `app/escala/services/leitor_celula.py:424`; `app/chamados/juizes.py:454`; `app/ponto/portas/celula.py:1149`.) |
| `Batida.colaborador/timestamp/tipo/retratada_em` | `registrar_batida`; retratação por `retratar_batida` → `anotar`. (`app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:158`; `app/ponto/registro_batida.py:276`.) | Apuração e associação por dia; as consultas públicas citadas filtram retratada. (`app/ponto/turnos.py:2888`; `app/ponto/turnos.py:2927`.) |
| `Turno.data_turno`; dia civil | `_mk` recebe a resposta de `_data_do_turno`; civil produzido por `data_local`. São respostas calculadas, não nova escrita em `Batida`. (`app/ponto/turnos.py:43`; `app/ponto/turnos.py:318`; `app/ponto/turnos.py:45`.) | `_dia_das_batidas_puro` e `batidas_do_dia_turno`. (`app/ponto/turnos.py:2965`; `app/ponto/turnos.py:2967`; `app/ponto/turnos.py:3117`.) |
| `Ausencia.tipo/data/data_fim/status`; `afastado_hoje` | Registro/edição pela porta de ausência; estado por `mudar_estado`. O juiz só consulta. (`app/ponto/services/ausencia.py:262`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:976`; `app/ponto/turnos.py:2743`.) | `afastamentos_no_dia/_pelo_efeito`; situação exibida chama o juiz. (`app/ponto/turnos.py:2727`; `app/ponto/turnos.py:2730`; `app/ponto/turnos.py:2530`; `app/colaboradores/services/situacao_exibida.py:63`.) |
| `competencia_ja_paga` e cadastro de pagamento externo | Escritor operacional: [nao verificado]; cadastro e trilha são obrigação registrada. (`app/docs/BACKLOG.md:1978`.) | Migração conjunta dos leitores, inclusive célula/fechamento/fio, está registrada; funcionamento após pouso: [nao verificado]. (`app/docs/BACKLOG.md:1978`.) |
| Valores de `FechamentoMensal` e rubricas/minutos de `DiaPago` | `recalcular_fechamento_mes` e `dia_pago.lavrar`; campos financeiros declarados em `CAMPOS_DIA/CAMPOS_AJUSTE`. (`app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1200`; `app/ponto/services/dia_pago.py:34`; `app/ponto/services/dia_pago.py:359`.) | Export lê fechamento; cartão lê fechamento/lavratura por seus serviços. (`app/folha/export.py:402`; `app/relatorios/cartao_pela_celula.py:79`; `app/relatorios/cartao_pela_celula.py:142`.) |
| `FechamentoMensal.previsto_em/atualizado_em` | `invalidar_previsto`, acionado pelo `save()` do vínculo, sem filtro de competência. (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1610`.) | Prontidão do fechamento. (`app/folha/export.py:1025`.) |
| Hash; lista nominal/Pauta DP da nova obra | Hash atual produzido por `hash_do_gravado`; escritor da lista/Pauta específica da O50: [nao verificado]. (`app/ponto/portas/regen_exportada.py:62`.) | Verificação dos três modelos e destino administrativo no efeito novo: [nao verificado]. |

**VEREDITO: AVAL-DEVOLVIDO**

- “Remover” da **LU-14** aceita neutralizar conservando a linha, ou exige exclusão física? O texto disponível da LU-13 não resolve essa equivalência. (`app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)
- O efeito novo alcança somente `dia > hoje`, ou também dias passados após um novo encerramento retroativo? No segundo caso, como fecha DNA sem vínculo, denúncia e autorização retroativa? (`app/ponto/portas/celula.py:272`; `CLAUDE.md:149`; `CLAUDE.md:404`.)
- Qual autoridade dará o dia do turno da **retratada**, compondo a união com o civil sem segundo juiz? (`app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2942`; `app/ponto/turnos.py:2967`.)
- Corrige-se a chamada para `afastado_hoje(colab, dia=dia_celula, para='cobranca')`? (`app/ponto/turnos.py:2737`.)
- Quais valores/linhas entram nos hashes dos três modelos e como se isola o efeito novo dos efeitos anteriores e após commit? (`app/ponto/portas/regen_exportada.py:71`; `app/ponto/services/fechamento.py:1610`; `app/ponto/services/fechamento.py:1824`.)

## 16/11 15:47 · sessao d773d8ad8c47 · - · - · origem=Ronald (tmux)

- A porta canônica de `CelulaDia` é `app/ponto/portas/celula.py`, com as operações registradas em `OPERACOES`; chamar esse módulo pelas portas de desligamento/vínculo mantém um escritor, enquanto escrita direta nessas portas contrariaria o registro. (`app/core/portas.py:107-113`; `app/ponto/portas/celula.py:74-84`.)

- **A premissa da operação existente é parcialmente correta.** Fora da vigência, **futuro** (`d > hoje`) exige célula gerada pelo vínculo encerrado, competência fora da lavra e ausência de outro vínculo; grava `trabalha=False`, `escala_geradora=None`, `dna=montar_dna(None,…)`, `dna_anterior`, `regenerada_em` e incremento de `regeneracoes`; depois desfaz o julgamento futuro. (`app/ponto/portas/celula.py:251-271`.) O DNA sem vínculo tem `vinculo_id`, `tipo_escala_id`, `tipo_ciclo`, `marcos`, `minutos_jornada`, `intervalo_livre` e `lampadas` nulos. (`app/ponto/management/commands/gerar_celulas.py:73-84`.)

- Para **hoje/passado** fora da vigência, a operação existente é diferente: exige `trabalha is not False`, conserva a geradora e grava somente `trabalha=False` e `montar_dna(ec,d,False)`; conserva `dna.vinculo_id=ec.id`, sem os três campos de denúncia nesse ramo. Portanto, “usa a operação existente, DNA sem vínculo” precisa explicitar seu alcance futuro ou reconhecer mudança dessa operação para hoje/passado. (`app/ponto/portas/celula.py:272-282`; `app/ponto/management/commands/gerar_celulas.py:104-112`.)

- Os dois ramos não consultam batidas, afastamento ou `competencia_ja_paga`; atualmente a guarda de competência vem de `_competencias_exportadas`, salvo `apesar_da_lavra` preenchido. (`app/ponto/portas/celula.py:218-221`; `app/ponto/portas/celula.py:251-279`.) `_competencias_exportadas` enumera TXT/holerite publicado e consulta `folha.export.competencia_fechada_para_celula`; isso não é a autoridade futura nomeada no contrato. (`app/ponto/management/commands/gerar_celulas.py:155-188`.)

- `executar_desligamento` já chama `fechar_vinculos_ativos`, que chama `fechar_vigencia`; o `update()` de vigência é seguido de `regenerar_futuras_do_cadastro(..., recalculo=False, substitui_dono=True)`. (`app/colaboradores/services/desligamento.py:147-149`; `app/colaboradores/services/vinculo.py:855-865`; `app/colaboradores/services/vinculo.py:641-664`.) Esse suplemento seleciona somente futuras geradas já existentes e chama a porta por dia, sem horizonte estendido. (`app/ponto/services/cadastro_futuro.py:26-45`.) O agendado retorna antes desse caminho; vínculos com início posterior ao fim são recusados e preservados. (`app/colaboradores/services/desligamento.py:107-126`; `app/colaboradores/services/vinculo.py:622`; `app/colaboradores/services/vinculo.py:665-670`.)

- `encerrar_vinculo` já salva `ativa=False/data_fim`; o signal de vigência chama a porta para hoje/passado e o suplemento para futuro. (`app/colaboradores/services/vinculo.py:835-851`; `app/escala/signals.py:54-83`.) Logo, já existe caminho acionável sem segundo escritor, mas reaproveitá-lo sem acrescentar as novas guardas não implementa as condições literais do aval. (`app/core/portas.py:112`; `app/ponto/portas/celula.py:251-279`.)

- `_outro_vinculo_cobre` **mora no command**, sendo importada pela porta; consulta outros vínculos da pessoa e pergunta a `escala.alimentacao.vinculo_cobre_a_data`. (`app/ponto/portas/celula.py:141-143`; `app/ponto/management/commands/gerar_celulas.py:269-287`.) A proteção vale nos dois ramos fora da vigência; geradora diferente também fica preservada. (`app/ponto/portas/celula.py:251-276`.)

- `eh_dia_trabalho` lê `cel.trabalha`; sem célula, volta a `eh_dia_trabalho_calculado`, **não diretamente ao template**. (`app/escala/models.py:1094-1096`.) `marcos_do_dia` usa o template sem célula **e também com célula cujo `dna.marcos` é vazio**; portanto conservar a linha não elimina sozinho todo retorno aos marcos do template. (`app/escala/models.py:1124-1133`.) O comentário documenta esse comportamento inclusive para folga. (`app/escala/models.py:1151-1158`.)

- A operação chama `_marcar_cobrancas_orfas` e registra evento de regeneração quando altera células; a trilha de regeneração é best-effort, com exceção capturada. (`app/ponto/portas/celula.py:327`; `app/ponto/portas/celula.py:369-389`.) O rabo comum pode rejulgar células passadas, recalcular dinheiro por competência e agendar rejunção/reavaliação de falta; `recalculo=False` elimina julgamento/dinheiro, mas não elimina todo o rabo de eventos. (`app/ponto/portas/celula.py:460-496`; `app/ponto/portas/celula.py:501-528`.)

Mapa de campos e consumidores para o censo:

| Campo/função | Quem escreve | Quem lê |
|---|---|---|
| `EscalaColaborador.ativa/data_fim` | `fechar_vigencia`, `encerrar_vinculo`. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:843-845`.) | Signal compara vigência anterior/atual; `_outro_vinculo_cobre` alimenta juiz de cobertura. (`app/escala/signals.py:25-28`; `app/escala/signals.py:56-68`; `app/ponto/management/commands/gerar_celulas.py:283-287`.) |
| `CelulaDia.trabalha` | `regenerar_celulas_vinculo`, nos ramos fora da vigência. (`app/ponto/portas/celula.py:259-266`; `app/ponto/portas/celula.py:277-279`.) | `eh_dia_trabalho`; juiz `dia_sem_trabalho`; leitor da grade. (`app/escala/models.py:1094-1096`; `app/chamados/juizes.py:459-460`; `app/escala/services/leitor_celula.py:398-428`.) |
| `CelulaDia.escala_geradora` | Ramo futuro limpa; ramo comum regenera. (`app/ponto/portas/celula.py:260-266`; `app/ponto/portas/celula.py:306-311`.) | `vinculo_do_dia` lê o carimbo. (`app/escala/alimentacao.py:270`.) |
| `CelulaDia.dna` | Porta usa produtor `montar_dna`. (`app/ponto/portas/celula.py:261`; `app/ponto/portas/celula.py:278`; `app/ponto/management/commands/gerar_celulas.py:73`.) | `marcos_do_dia`. (`app/escala/models.py:1124-1133`.) |
| `dna_anterior/regenerada_em/regeneracoes` | Ramo futuro e regeneração comum. (`app/ponto/portas/celula.py:262-266`; `app/ponto/portas/celula.py:307-311`.) | Carregador/juiz de reescrita consulta `regenerada_em`; reversão usa denúncia conforme contrato da função. (`app/escala/services/regeneracao.py:72-105`; `app/ponto/portas/celula.py:247-249`.) |
| `origem/editada_em/editada_por` | Operação humana declarada `editar_dia`. (`app/ponto/portas/celula.py:14-16`; `app/ponto/portas/celula.py:83-84`.) | Regeneração exige `origem='gerada'`; juiz de reescrita lê `editada_em`. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`; `app/escala/services/regeneracao.py:84-105`.) |
| Julgamento futuro (`ata`, `veredito`, `status_*`, carimbos etc.) | `desfazer_julgamento_da_futura` zera lista de campos por `salvar_e_emitir`, preservando veredito via admin com trilha obrigatória. (`app/ponto/portas/celula.py:1112-1113`; `app/ponto/portas/celula.py:1155-1175`.) | A própria operação verifica existência do julgamento e via admin. (`app/ponto/portas/celula.py:1149-1155`.) |
| `FechamentoMensal` previsto | `invalidar_previsto` é chamado em todo post-save do vínculo, sem filtro de competência no signal. (`app/escala/signals.py:137-146`.) | A leitura seguinte recalcula previsto conforme docstring do signal; comprovação do caminho completo cabe ao censo do fechamento. (`app/escala/signals.py:139-143`.) |

A enumeração acima demonstra consumidores concretos, não exaustividade do futuro censo de leitores/escritores pedido pelo contrato. [nao verificado]

## 16/11 15:47 · sessao 73548f0a5ded · - · - · origem=Ronald (tmux)

- **(4) há erro literal de argumento:** assinatura é `afastado_hoje(colab, dia=None, *, para)`; `para='cobranca'` existe, mas `data=` não existe. A forma executável é `afastado_hoje(colab, dia=dia_celula, para='cobranca')`; retorna `Ausencia` ou `None`, não booleano estrito. (`app/ponto/turnos.py:2737`; `app/ponto/turnos.py:2743`.)

- **O efeito cobrança é suportado:** exclui apenas ausência `status='rejeitada'`; folha aceita somente aprovada. Afastamento é restrito a `TIPOS_AFASTADO=('afastamento_inss',)`, com cobertura por data única, intervalo fechado ou aberto nos tipos autorizados; vence maior data e maior pk. (`app/ponto/turnos.py:2523`; `app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2716`; `app/ponto/turnos.py:2727`; `app/ponto/turnos.py:2730`; `app/ponto/turnos.py:2743`.)

- **(3) o dia do turno vivo já tem autoridade:** `turnos_do_colab` carrega dois dias anteriores e um posterior, pareia por vínculos/ata e devolve turnos com `data_turno` no intervalo; `batidas_do_dia_turno` coleta os pks desses turnos. Isso contempla saída no dia seguinte. (`app/ponto/turnos.py:1834`; `app/ponto/turnos.py:1847`; `app/ponto/turnos.py:1850`; `app/ponto/turnos.py:1997`; `app/ponto/turnos.py:2000`; `app/ponto/turnos.py:3115`.)

- **O dia civil é `data_local(timestamp)`, usando `timezone.localtime(...).date()`.** O juiz `dia_das_batidas` atribui primeiro o dia do turno por pk e somente às não reclamadas atribui o civil; sua saída não fornece a união turno **e** civil exigida pela cláusula “OU”. A união deve ser explícita: a mesma saída de madrugada pode proteger as duas células. (`app/ponto/turnos.py:45`; `app/ponto/turnos.py:48`; `app/ponto/turnos.py:2957`; `app/ponto/turnos.py:2965`; `app/ponto/turnos.py:2967`.)

- **Retratada permanece uma lacuna do desenho literal:** `batidas_apuraveis`, `turnos_do_colab` e `dia_das_batidas` excluem `retratada_em`; `dia_do_turno_da_batida` declara `None` para retratada. Portanto, ler o cru pelo civil não prova proteção da véspera à qual uma retratada noturna pertencia. O contrato precisa nomear como obter essa associação sem clonar juiz. (`app/ponto/turnos.py:2888`; `app/ponto/turnos.py:1868`; `app/ponto/turnos.py:1869`; `app/ponto/turnos.py:2927`; `app/ponto/turnos.py:2939`; `app/ponto/turnos.py:2942`.)

- **Não confundir com `turnos_de_batidas`:** aceita lista carregada e chama o pareador, mas alimenta marcos do template, enquanto `turnos_do_colab` consulta vínculo do dia e papel da ata. Sua equivalência para retratadas não está demonstrada. (`app/ponto/turnos.py:1517`; `app/ponto/turnos.py:1533`; `app/ponto/turnos.py:1535`; `app/ponto/turnos.py:1932`; `app/ponto/turnos.py:1944`; `app/ponto/turnos.py:1965`.)

- **`competencia_ja_paga`:** nenhuma ocorrência em `app/**/*.py` na busca desta cópia; implementação disponível: **[nao verificado]**. A obra e a definição “exportação vigente OU paga fora por cadastro com trilha” estão no BACKLOG; seu pouso continua condicionado à espinha no registro. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

Mapeamento de campos e funções destas condições:

| Dado/função lido | Quem escreve | Quem lê |
|---|---|---|
| `Batida.colaborador`, `timestamp`, `tipo` | `registrar_batida`, nos dois `create` (`app/ponto/registro_batida.py:152`; `app/ponto/registro_batida.py:158`) | `turnos_do_colab` (`app/ponto/turnos.py:1868`), `dia_das_batidas` (`app/ponto/turnos.py:2927`), `batidas_apuraveis` (`app/ponto/turnos.py:2888`) |
| `Batida.retratada_em`, `aprovada=False` | `retratar_batida` chama `anotar` (`app/ponto/registro_batida.py:269`; `app/ponto/registro_batida.py:276`) | Os filtros anteriores; significado registrado no modelo (`app/ponto/models.py:64`; `app/ponto/models.py:67`) |
| `Turno.data_turno` | `_mk` chama `_data_do_turno`, construindo o `namedtuple`, sem campo persistido em `Batida` (`app/ponto/turnos.py:43`; `app/ponto/turnos.py:318`; `app/ponto/turnos.py:323`) | `batidas_do_dia_turno` e `_dia_das_batidas_puro` (`app/ponto/turnos.py:3117`; `app/ponto/turnos.py:2965`) |
| `Ausencia.tipo`, `data`, `data_fim` | criação por `criar_ausencia` e `save`; edição em `editar_ausencia_registro` (`app/ponto/services/ausencia.py:262`; `app/ponto/services/ausencia.py:562`; `app/ponto/services/ausencia.py:648`; `app/ponto/services/ausencia.py:659`) | `afastamentos_no_dia` (`app/ponto/turnos.py:2727`; `app/ponto/turnos.py:2730`) |
| `Ausencia.status` | `mudar_estado`, que grava status e campos derivados (`app/ponto/services/ausencia.py:955`; `app/ponto/services/ausencia.py:976`; `app/ponto/services/ausencia.py:988`) | `_pelo_efeito`, usado pelo afastamento (`app/ponto/turnos.py:2528`; `app/ponto/turnos.py:2530`; `app/ponto/turnos.py:2727`) |
| `afastado_hoje` | função leitora, sem escrita no corpo (`app/ponto/turnos.py:2737`; `app/ponto/turnos.py:2743`) | situação exibida e aviso de batida (`app/colaboradores/services/situacao_exibida.py:63`; `app/ponto/services/afastado_avisa.py:39`) |
| `competencia_ja_paga` | escritor do futuro cadastro: **[nao verificado]** | migração dos leitores é obrigação da obra registrada (`app/docs/BACKLOG.md:1978`) |

O chokepoint registrado de `Batida` é `ponto/registro_batida.py`; não haveria motivo para a O50 escrever esses campos ao apenas protegê-los. (`app/core/portas.py:99`; `app/core/portas.py:103`.)

## 16/11 15:47 · sessao 8fec5aad9b26 · - · - · origem=Ronald (tmux)

- **Correção de referência:** o texto disponível da LU-13 não diz “remover”; define direção, “futuras = após D”, protegendo exportada/editada/com batida e exigindo medição antes e `!` para apply retroativo. “Remover com trilha” aparece na **LU-14**. Conservar a linha atende às exclusões da LU-13, mas sua equivalência literal a “remover” da LU-14 não está estabelecida; deve subir a pergunta prevista pelo próprio complemento, sem presumir revogação. (`app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

- **A operação não é uniforme para todo dia após `data_fim`:** no futuro civil (`d > hoje`), a porta grava `trabalha=False`, `escala_geradora=None`, `dna=montar_dna(None,...)`, `dna_anterior`, `regenerada_em` e `regeneracoes`, e desfaz o julgamento anterior. No dia corrente/passado, grava apenas `trabalha=False` e DNA com `ec`, conservando a geradora e sem os três campos de denúncia. Logo, a premissa genérica “operação existente = DNA sem vínculo” só confere para o ramo futuro. (`app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:259`; `app/ponto/portas/celula.py:265`; `app/ponto/portas/celula.py:267`; `app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:277`.)

- **Risco literal ainda aberto:** um novo encerramento informado com `data_fim` passada pode produzir dias novos da obra que também são passados civis; excluir o “passivo já existente” não fixa expressamente esse limite. O ramo existente não fornece a denúncia que a seção 4 exige para reescrita de passado. O contrato precisa restringir a atuação nova a `dia > hoje` ou explicar a autorização e a denúncia para o outro ramo. (`app/ponto/portas/celula.py:272`; `app/ponto/portas/celula.py:279`; `CLAUDE.md:149`; `CLAUDE.md:152`; `app/docs/CORTES.md:470`.)

- **Trilha atual não é garantia suficiente de sucesso:** existe chamada de auditoria ao final da regeneração, mas ela está em `try/except`, e sua falha é apenas registrada no logger; portanto, “com trilha obrigatória e atômica” não decorre dessa implementação. (`app/ponto/portas/celula.py:369`; `app/ponto/portas/celula.py:372`; `app/ponto/portas/celula.py:388`.)

- **Exportada e dinheiro:** a consolidação vigente mantém `!` para paga/vínculo ou escala retroativo; dinheiro não exportado tem preaprovação condicionada à reversão anterior, hash de exportada intacta e prova de frota no mesmo marco. Exportada que muda o gravado exige `!`, DIFF, reversão e porta própria. O novo contrato pode ser mais restritivo e excluir paga/exportada, mas não deve apresentar L-092 isolada como regra não alterada: L-113 revogou antigas recusas, enquanto a regra consolidada delimita os caminhos e aprovações atuais. (`CLAUDE.md:404`; `CLAUDE.md:406`; `CLAUDE.md:407`; `CLAUDE.md:411`; `CLAUDE.md:413`; `app/docs/LEIS.md:155`.)

- **Hash existente não cobre a condição (5) inteira:** `hash_do_gravado` consulta apenas `FechamentoMensal`, derivando campos concretos com uma lista de exclusões; entre os excluídos estão `previsto_em` e `derivado_em`. Não cobre `CelulaDia` nem `DiaPago`. Portanto, reaproveitá-lo sozinho não prova a condição conjunta do complemento; o futuro brief precisa nomear os campos, identidades e existência das linhas comparadas, além de isolar o efeito novo do efeito anterior do encerramento. (`app/ponto/portas/regen_exportada.py:37`; `app/ponto/portas/regen_exportada.py:56`; `app/ponto/portas/regen_exportada.py:62`; `app/ponto/portas/regen_exportada.py:71`.)

- **L-099:** proibir resolver cadastro/vínculo futuro por código e mandar os casos para administração está alinhado à separação ESTRUTURA/CADASTRO/BATIDA; alterar esse alcance exigiria outra autorização. (`app/docs/LEIS.md:122`; `CLAUDE.md:179`.)

- **Dependência de pagamento:** o cadastro “paga fora do sistema” e a migração conjunta dos leitores estão descritos na obra ainda registrada; o corte exige pouso atrás da espinha e medição na sombra. Não há base nesses registros para afirmar que já pousou. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

## 16/11 15:33 · sessao bdfad5118ce7 · UI-EDITOR-CALENDARIO-TRES-GRADES · verificador · origem=Code

1. **PROCEDE — curas do item 3; NÃO PROCEDE — cura completa do item 5.**

O `finally` cobre o reenvio síncrono por `requestSubmit()`: o guarda encontra `confirmado=true`, e a autorização é zerada ao retornar, inclusive se o `required` impedir o evento ou ocorrer exceção. Também cobre o retorno de `form.submit()`; esse fallback dispensa validação e evento `submit`, mas só é chamado após resposta afirmativa. Não identifiquei novo caminho do editor que envie grade vazia em `dias_mes` sem pergunta. (`app/templates/escala/folga_calendario_editar.html:138`, `app/templates/escala/folga_calendario_editar.html:143`, `app/templates/escala/folga_calendario_editar.html:147`, `app/templates/escala/folga_calendario_editar.html:151`)

Editar o nome agora apenas redesenha, sem repor `dias_*` após `fcModo()` esvaziá-los. (`app/templates/escala/folga_calendario_editar.html:128`, `app/templates/escala/folga_calendario_editar.html:180`)

**Resta uma lacuna no selo das grades:** ele verifica somente a abertura das `section`, sem medir sua visibilidade após o JS. Contraexemplo por inspeção: acrescentar ao final da inicialização a ocultação de `fc-tam-30` e `fc-tam-28` preservaria as asserções estáticas; os cliques programáticos do smoke continuariam alcançando seus dias. O delta protege `hidden/display:none` diretamente na seção inicial, mas ainda não protege integralmente “três grades visíveis ao mesmo tempo”. (`app/escala/tests/test_editor_calendario_tres_grades.py:37`, `app/escala/tests/test_editor_calendario_tres_grades.py:89`, `app/escala/tests/test_editor_calendario_tres_grades.py:92`, `app/escala/tests/test_smoke_clique_folga_calendario.py:51`)

Execução desse mutante: **[não verificado]**.

2. **PROCEDE — leitores e escritores depois do delta.**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `confirmado` | Inicialização em falso; resposta afirmativa põe verdadeiro; `finally` limpa. (`app/templates/escala/folga_calendario_editar.html:136`, `:150`, `:151`) | Guarda de `submit`. (`app/templates/escala/folga_calendario_editar.html:138`) |
| `dias_<tam>` no formulário | `syncHidden()` serializa `sel`; inicialização, alternar dia, repetir e voltar a `dias_mes` o acionam. `fcModo()` limpa em `semana_dia`. (`app/templates/escala/folga_calendario_editar.html:109`, `:110`, `:119`, `:125`, `:180`, `:181`) | Navegador/sonda serializam; view lê, filtra e entrega à porta; porta persiste `dias`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:56`; `app/escala/views.py:342`, `:347`, `:356`; `app/escala/folgas.py:594`) |
| Canônico do editor | `renderGrade()` usa seleção ordenada e nome atual; input do nome chama esse redesenho. (`app/templates/escala/folga_calendario_editar.html:80`, `:94`, `:106`, `:128`) | Exibição em `fc-canonico-*`; não é campo enviado à view. (`app/templates/escala/folga_calendario_editar.html:57`, `:106`; `app/escala/views.py:321`, `:342`) |
| Canônico da lista | View monta a partir de nome, tamanho e dias persistidos. (`app/escala/views.py:293`, `:297`) | Template da lista imprime `t.canonico`. (`app/templates/escala/folga_calendarios_lista.html:22`) |

3. **PROCEDE — o novo teste morde a autorização velha, por inspeção.** [nao verificado]

A sonda apaga o nome antes do primeiro Sim e restaura antes do segundo Salvar. Sem a limpeza no `finally`, o primeiro reenvio barrado pelo `required` deixa a autorização verdadeira; o segundo clique passa sem perguntar, faltando `modal2` e aparecendo POST. As duas asserções pertinentes exigem segundo modal e `SEM-POST`. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:151`; `app/escala/tests/test_smoke_clique_folga_calendario.py:209`, `:210`, `:212`, `:213`)

**NÃO PROCEDE — reabrir os descartes como bloqueadores:**

- Trocar para `semana_dia` enquanto o modal está pendente não demonstra envio indevido em `dias_mes`. Contudo, a justificativa “é overlay” não garante bloqueio do teclado: o componente foca o Sim, mas não implementa contenção de foco. (`app/templates/escala/folga_calendario_editar.html:139`; `app/templates/core/_dialogo.html:160`, `:195`)
- A contagem sem `_reenvia()` continua redundante; `SEM-POST` é a asserção efetiva. (`app/escala/tests/test_smoke_clique_folga_calendario.py:175`, `:192`, `:199`, `:200`)
- O smoke contém leitura real do modal no Chromium, além da regex; resultado efetivamente executado não foi fornecido. (`app/escala/tests/test_smoke_clique_folga_calendario.py:69`, `:71`, `:124`, `:187`)

GREEN, RED e mutante executados: **[não verificado]**.

4. **NÃO PROCEDE — nova infração identificada de LEIS-UI ou dos selos estruturais.**

A ajuda apontada na volta 1 foi removida, curando aquele item 6; o trecho agora entra diretamente no laço das grades. O delta mantém `hxConfirmar` e não introduz novo markup nas alterações do guarda. (`app/docs/LEIS-UI.md:77`; `app/templates/escala/folga_calendario_editar.html:39`, `:127`, `:146`, `:151`)

Execução dos selos de template: **[não verificado]**.

**VEREDITO: DEVOLVE 5.**

## 16/11 15:31 · sessao 20b59d9b1e2c · - · - · origem=Ronald (tmux)

**PROCEDE — cura da autorização.** `confirmado=true` agora vale apenas durante o reenvio e o `finally` o limpa mesmo quando `requestSubmit()` retorna sem gerar `submit` por causa do `required`, ou quando a chamada lança exceção. O guarda síncrono do reenvio encontra `true`. (`app/templates/escala/folga_calendario_editar.html:138`, `:150`, `:151`)

**PROCEDE, com limite explícito — fallback.** O `finally` também limpa após `form.submit()`. Esse fallback não passa pelo evento `submit` nem pela validação HTML, mas só é alcançado depois de `ok=true`; portanto não cria POST sem pergunta. A garantia de bloqueio pelo `required` não se aplica a esse fallback. (`app/templates/escala/folga_calendario_editar.html:146`, `:147`, `:151`)

**NÃO PROCEDE — novo caminho demonstrado de POST vazio sem pergunta em `dias_mes`.** No fluxo do formulário, o guarda consulta o modo, identifica seleção vazia e cancela antes de abrir a pergunta; Cancelar retorna sem reenvio. (`app/templates/escala/folga_calendario_editar.html:139`, `:140`, `:143`, `:146`, `:147`)

**PROCEDE — novo teste tem mordida por inspeção.** A sonda apaga o nome antes do primeiro Sim e o restaura antes de clicar novamente. Sem o `finally` — inclusive restaurando o código anterior que limpava somente no evento `submit` — o primeiro `requestSubmit()` é barrado pelo `required`, a autorização sobra, o segundo clique passa sem modal e a sonda registra POST. Assim falham `modal2` e/ou `SEM-POST`. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:151`; `app/escala/tests/test_smoke_clique_folga_calendario.py:209`, `:210`, `:212`, `:213`, `:52`, `:57`) Execução efetiva e mutante executado: **[não verificado]**.

**NÃO PROCEDE — descarte do modal/troca de modo como bloqueador desta obra.** O overlay existe, mas o componente não implementa contenção de foco; só trata Escape e Enter no campo do próprio modal. A possibilidade de troca por teclado não está eliminada. Contudo, confirmar e reenviar em `semana_dia` segue a regra desse modo, enquanto voltar a `dias_mes` e submeter normalmente encontra o guarda. (`app/templates/core/_dialogo.html:112`, `:160`, `:163`, `:195`; `app/templates/escala/folga_calendario_editar.html:139`, `:176`) Reprodução humana: **[não verificado]**.

**NÃO PROCEDE — contagem redundante como defeito bloqueador.** A comparação antes/depois não demonstra ausência de gravação HTTP: o POST Django só ocorre em `_reenvia()`, ausente nesse teste. A asserção efetiva é `SEM-POST`, alimentada pela sonda que respeita `defaultPrevented`. (`app/escala/tests/test_smoke_clique_folga_calendario.py:53`, `:165`, `:175`, `:192`, `:199`, `:200`)

**PROCEDE — smoke vai além da regex de `hxConfirmar`.** O código do smoke lê texto e botões do modal aberto no DOM do Chromium e exige sua presença; isso protege comportamento quando executado. Não há, no dado recebido, relatório de execução que permita afirmar GREEN/RED observado. (`app/escala/tests/test_smoke_clique_folga_calendario.py:69`, `:71`, `:124`, `:187`, `:188`) Execução observada: **[não verificado]**.

## 16/11 15:31 · sessao fa2574325564 · - · - · origem=Ronald (tmux)

**Pergunta 4 — NÃO PROCEDE: nova infração de LEIS-UI ou dos selos estruturais não identificada no delta.** A ajuda questionada desapareceu; o trecho agora passa diretamente do contêiner ao laço de grades. A L7 está em `app/docs/LEIS-UI.md:77`; trecho atual em `app/templates/escala/folga_calendario_editar.html:39`. As adições restantes são comentários e JS, sem diálogo nativo ou novo markup interpolado (`app/templates/escala/folga_calendario_editar.html:127`, `app/templates/escala/folga_calendario_editar.html:146`, `app/templates/escala/folga_calendario_editar.html:151`). Os selos examinam diálogo nativo, atributos inválidos, fonte Python, comentários vazados e interpolação crua (`app/core/tests/test_contract_l8_dialogo.py:42`; `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`, `:180`). Execução desses selos: **[não verificado]**.

**Cura das grades ocultas — PROCEDE para o mutante declarado.** `_secoes()` devolve somente a tag de abertura de cada seção, portanto a regex nova não acusa a classe `hidden` legítima dos spans internos; acusa `hidden` ou `display:none` na própria seção (`app/escala/tests/test_editor_calendario_tres_grades.py:37`, `:40`, `:92`; `app/templates/escala/folga_calendario_editar.html:47`).

A prova continua limitada: o selo não mede visibilidade calculada, ancestrais, CSS por seletor ou o próprio `fc-grade` escondido; verifica apenas ID da grade e atributos da seção. O smoke também usa `.click()` programático (`app/escala/tests/test_editor_calendario_tres_grades.py:89`, `:92`; `app/escala/tests/test_smoke_clique_folga_calendario.py:51`). Contraexemplo estático: adicionar `hidden` ao `div#fc-grade-*` preservaria essas asserções; execução do mutante: **[não verificado]**.

## 16/11 15:28 · sessao a6729505dd53 · O50-CELULA-APOS-DESLIGAMENTO · verificador · origem=Code

**O item 1 precisa voltar para fechar condições literais**, principalmente pagamento, células editadas e universo de batidas; aval condicional não autoriza completar essas condições por inferência. (`app/docs/LEIS.md:61`; `app/docs/CORTES.md:470`.)

Revisão estática da cópia. Identidade Git do HASH, comportamento em execução e impacto em produção: [nao verificado].

1. **As portas existem, mas não fazem hoje a remoção proposta.**

   `executar_desligamento` grava situação, data e motivo da demissão, bloqueia acesso, registra histórico e chama `fechar_vinculos_ativos`; também executa a cascata de chamados/disputas e cria o chamado de auditoria ao DP. O ramo agendado grava a intenção e retorna antes dessa cascata. (`app/colaboradores/services/desligamento.py:107`; `app/colaboradores/services/desligamento.py:128`; `app/colaboradores/services/desligamento.py:147`; `app/colaboradores/services/desligamento.py:164`; `app/colaboradores/services/desligamento.py:195`.)

   O desligamento fecha apenas vínculos ativos cujo início não seja posterior ao fim informado; vínculos futuros recusados permanecem. Esse caminho usa `update()` e regenera somente células futuras, com `recalculo=False`. Portanto, não equivale à proposta de remover células e atualizar os valores do fechamento. (`app/colaboradores/services/vinculo.py:641`; `app/colaboradores/services/vinculo.py:652`; `app/colaboradores/services/vinculo.py:662`; `app/colaboradores/services/vinculo.py:855`.)

   `encerrar_vinculo` valida a vigência, grava `ativa=False` e `data_fim` por `save()` e registra antes/depois. Esse `save()` dispara regeneração de células passadas e futuras, além da invalidação do previsto do fechamento. **Já há efeito indireto sobre célula e fechamento nessa porta.** (`app/colaboradores/services/vinculo.py:835`; `app/colaboradores/services/vinculo.py:842`; `app/escala/signals.py:72`; `app/escala/signals.py:80`; `app/escala/signals.py:145`.)

   A **porta da célula** é `ponto/portas/celula.py`, registrada para `CelulaDia`; gerar, regenerar, editar, lavrar e carimbar pertencem a esse módulo. `gerar_celulas.py` reexporta as funções, sem constituir outro escritor. Não há operação declarada de remoção de `CelulaDia`; fora da vigência, o código conserva a linha e altera trabalho/DNA. (`app/core/portas.py:107`; `app/ponto/portas/celula.py:74`; `app/ponto/management/commands/gerar_celulas.py:10`; `app/ponto/portas/celula.py:239`; `app/ponto/portas/celula.py:251`; `app/ponto/portas/celula.py:272`.)

   “Ninguém remove” seria absoluto demais: excluir o colaborador tem cascata sobre células; excluir o vínculo apenas anula sua geradora. (`app/escala/models.py:1681`; `app/escala/models.py:1692`.)

   A **porta do fechamento** é `ponto/services/fechamento.py`. `recalcular_por_evento` agenda, após commit, `recalcular_fechamento_mes` para o colaborador e a competência do fato; esta grava `FechamentoMensal` e também lavra `DiaPago`. Apenas invalidar carimbos não equivale a atualizar seus valores. (`app/ponto/services/fechamento.py:1315`; `app/ponto/services/fechamento.py:1798`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1191`; `app/ponto/services/fechamento.py:1652`.)

2. **A direção da obra é compatível, mas precisa conservar restrições vigentes.**

   A célula é dado congelado; a exceção formal para passado errado por cadastro exige denúncia da reescrita. LU-13/LU-14 autorizam a direção de remover com trilha, preservando **exportada, editada e com batida**, com medição prévia e `!` próprio para retroativo. O item 1 fornecido não repete a exceção “editada”; revogação dessa proteção: [nao verificado]. (`CLAUDE.md:149`; `app/docs/CORTES.md:470`; `app/docs/CORTES.md:471`.)

   DESLIGADO-NAO-TRAVA já faz o desligado retornar sem motivos de retenção e entrega os dias como informativo ao DP. Isso não depende de apagar células; a O50 não deve ser apresentada como condição para aquela liberação. (`app/folha/export.py:1197`; `app/folha/export.py:1207`; `app/folha/export.py:1216`.)

   A cura nas portas é coerente com L-099; usar código para escolher cadastro correto, apagar batida ou resolver vínculo futuro extrapolaria a separação entre ESTRUTURA e CADASTRO/BATIDA. (`app/docs/LEIS.md:122`; `app/colaboradores/services/vinculo.py:855`.)

   L-113 alterou as recusas históricas de L-092, mas o texto consolidado mantém alteração genérica de exportada fora da preaprovação, com porta e `!` específicos. Portanto, a rotina genérica de recálculo não pode servir como garantia automática de “exportada intacta”. (`app/docs/LEIS.md:155`; `CLAUDE.md:411`; `app/ponto/portas/celula.py:436`.)

3. **Há condições que ainda não fecham no literal.**

   **“Não paga”:** `competencia_entregue` reconhece TXT vigente; `competencia_fechada_para_celula` acrescenta holerite publicado da pessoa; `marcos_da_competencia['paga']` também consulta holerite publicado. Isso não comprova ausência de pagamento externo. A obra do juiz `competencia_ja_paga`, incluindo cadastro de pagamento externo, está registrada com pouso condicionado à espinha. (`app/folha/export.py:588`; `app/folha/export.py:618`; `app/folha/export.py:622`; `app/ponto/services/fechamento.py:1539`; `app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)

   **“Tenha batida”:** o universo apurável exclui retratadas, enquanto a regra distingue apuração de exibição crua. A associação ao dia também distingue batida reclamada pelo turno de órfã no dia civil. A proibição de remover precisa escolher explicitamente o universo e proteger a passagem da meia-noite. (`CLAUDE.md:158`; `CLAUDE.md:167`; `app/ponto/turnos.py:2888`; `app/ponto/turnos.py:2898`.)

   **“Afastado”:** há juiz registrado, `afastado_hoje`, com data e argumento obrigatório `para`; cobrança considera ausência não rejeitada, folha considera aprovada. Consultar apenas `situacao='afastado'` não representa esse contrato. Falta definir data e efeito do veto. (`app/core/juizes.py:272`; `app/ponto/turnos.py:2737`; `app/colaboradores/services/situacao_exibida.py:18`; `app/colaboradores/models.py:444`.)

   **“Posterior ao fim”:** a célula pertence à pessoa/dia, com geradora opcional; pode existir sucessor ou readmissão. A própria regeneração atual preserva dia coberto por outro vínculo. Remover indiscriminadamente toda célula após o fim de um vínculo perderia essa proteção. (`app/escala/models.py:1681`; `app/escala/models.py:1685`; `app/escala/models.py:1692`; `app/ponto/portas/celula.py:254`; `app/docs/BACKLOG.md:1411`.)

4. **Dinheiro está no escopo; seus efeitos ainda precisam ser delimitados.**

   Atualizar `FechamentoMensal` toca valores financeiros e pode alcançar a competência inteira e `DiaPago`. Valor efetivamente alterado ou garantia de nenhuma falta nova: [nao verificado]. A preaprovação exige reversão anterior, exportada intacta comprovada por hash e prova de frota no mesmo marco; paga e vínculo/escala retroativo continuam sujeitos a `!`. (`app/ponto/services/fechamento.py:902`; `app/ponto/services/fechamento.py:1160`; `app/ponto/services/fechamento.py:1191`; `CLAUDE.md:404`; `CLAUDE.md:406`.)

   A remoção também anula referências de chamados e perguntas; ausência da célula faz leitores recorrerem ao cálculo/template. Logo, “a linha sumiu” não prova sozinho que previsto, cobrança e dinheiro ficaram coerentes. (`app/chamados/models.py:50`; `app/chamados/models.py:2519`; `app/escala/models.py:1094`; `app/escala/models.py:1098`.)

**VEREDITO: AVAL-DEVOLVIDO**

- A execução aguardará `competencia_ja_paga` ou qual autoridade provará ausência de pagamento externo? (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.)
- Permanecem excluídas células editadas, dias de sucessor/readmissão e todo passivo reservado ao `!`? (`app/docs/CORTES.md:470`; `app/docs/BACKLOG.md:1411`.)
- “Tenha batida” inclui retratada e órfã, com associação ao dia por qual autoridade? (`CLAUDE.md:158`; `CLAUDE.md:167`; `app/ponto/turnos.py:2898`.)
- “Afastado” será consultado em qual data e com qual `para`? (`app/ponto/turnos.py:2737`; `app/colaboradores/services/situacao_exibida.py:18`.)
- “Exportada → nada muda” vale para todo o ato ou apenas para o efeito novo da O50, considerando que o `save()` atual invalida previsto sem filtro de competência? (`app/escala/signals.py:145`; `app/ponto/services/fechamento.py:1607`.)

## 16/11 15:27 · sessao 982166043b41 · - · - · origem=Ronald (tmux)

- A porta canônica é `app/ponto/portas/celula.py`, registrada para `CelulaDia` em `app/core/portas.py:112`; declara gerar, lavrar, carimbar e editar, sem operação de remover célula (`app/ponto/portas/celula.py:74`).
- O censo estático real `core.censo_escritas.varrer_arvore` encontrou 10 escritas atribuídas a `CelulaDia`, todas nessa porta, sem `delete`; o instrumento declara limite para operações não atribuíveis (`app/core/censo_escritas.py:43`). Os sítios são `app/ponto/portas/celula.py:265`, `:279`, `:293`, `:310`, `:638`, `:647`, `:836`, `:914`, `:926`, `:1237`.
- Fora da vigência, hoje a célula permanece: futuro zera previsto, limpa geradora, guarda DNA anterior e desfaz julgamento (`app/ponto/portas/celula.py:251`); passado grava `trabalha=False` e DNA sem marcos (`app/ponto/portas/celula.py:272`). O comentário explicita “A celula NAO se apaga” porque há perguntas apontando para ela (`app/ponto/portas/celula.py:239`).
- O `delete` da porta remove `FuroDiario`, não `CelulaDia` (`app/ponto/portas/celula.py:1253`, `:1266`).
- A guarda vigente da célula é `competencia_fechada_para_celula`: TXT vigente entregue OU holerite com lote publicado da pessoa; não representa pagamento externo declarado (`app/folha/export.py:597`, `:618`, `:622`). O atual `marcos_da_competencia()['paga']` também só consulta holerite com lote publicado (`app/ponto/services/fechamento.py:1539`). O corte COMPETENCIA-PAGA consta “construindo”, condicionado à espinha (`app/docs/CORTES.md:495`).
- “Tenha batida” não fecha por inferência: o universo apurável exclui retratadas (`app/ponto/turnos.py:2868`, `:2888`); associação ao dia distingue batida reclamada por turno e órfã no dia civil (`app/ponto/turnos.py:2898`, `:2957`). É preciso dizer se a proibição inclui retratadas e como protege órfãs e turnos que cruzam meia-noite; o literal do anexo não decide isso [nao verificado].
- Excluir a célula anula referências de chamados e perguntas (`app/chamados/models.py:50`, `:2519`). Excluir o colaborador causa cascata para células (`app/escala/models.py:1681`); excluir vínculo apenas anula `escala_geradora` (`app/escala/models.py:1692`). A restauração de snapshot apaga ECs e protege células editadas contra essa anulação (`app/colaboradores/services/vinculo.py:1022`).
- A seleção deve preservar dia de sucessor: o gerador trabalha por vínculo vigente e, se a célula não existe, cria uma (`app/ponto/portas/celula.py:906`, `:914`); a regeneração também cria célula ausente no dia coberto (`app/ponto/portas/celula.py:289`). A célula pertence à pessoa e ao dia, enquanto a geradora é proveniência opcional (`app/escala/models.py:1681`, `:1685`, `:1692`).
- Remoção precisa conferir leitores: sem célula, `eh_dia_trabalho` recai no cálculo do vínculo (`app/escala/models.py:1094`); `marcos_do_dia` declara retorno ao template vivo quando falta célula/DNA (`app/escala/models.py:1098`). Ausência da linha não prova ausência de previsto.
- O dinheiro deve ser acionado pelo ato, não por sinal de `CelulaDia` (`app/ponto/portas/celula.py:432`). A própria porta registra que o caminho ordinário hoje pode recalcular exportada, após L-113 (`app/ponto/portas/celula.py:436`); O50 necessita guarda explícita do seu alcance antes de chamar esse caminho.

## 16/11 15:27 · sessao c81e9bf48af4 · - · - · origem=Ronald (tmux)

Achados para consolidar, só item 1:

- **Restrição omitida no aval novo:** LU-13 manda O50 “só DIREÇÃO”, define futuras como após D e exclui **exportada/editada/com batida**, exige medir frota antes e `!` próprio para retroativo. A condição **editada** precisa continuar explícita; o item 1 fornecido não a nomeia. (`app/docs/CORTES.md:470`; cláusula do anexo novo sem arquivo local: [nao verificado].)
- **Irretroatividade não se resolve só por “posterior ao fim”:** célula é dado congelado e passado só se reescreve pela exceção formal de cadastro com denúncia; “posterior ao fim” pode alcançar datas já passadas. Precisa separar evento futuro, célula futura já existente e passivo reservado ao `!`, sem autorizar por inferência a remoção de passado. (`CLAUDE.md:149-156`; `app/docs/BACKLOG.md:1410-1411`; `app/docs/CORTES.md:470`.)
- **Paga é dependência real, sem juiz pousado nesta cópia:** a obra `COMPETENCIA-PAGA-UM-JUIZ` declara o futuro `competencia_ja_paga`, cuja definição é entregue/exportação vigente OU cadastro “paga fora do sistema”, e proíbe meia migração; ainda está registrada/construindo, com pouso depois da espinha. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:495`.) A busca em Python de produção não achou `competencia_ja_paga`, `CompetenciaPaga`, `paga_fora` ou `competencia_paga`: ausência absoluta além desses nomes [nao verificado].
- **Exportação tem juiz, pagamento não pode ser inferido dele:** `competencia_entregue` pergunta por TXT vigente não invalidado; `competencia_fechada_para_celula` compõe essa resposta com holerite publicado do próprio colaborador. Nenhum desses contratos diz representar pagamento externo. (`app/folha/export.py:556-594`, especialmente `:564-578`; `:597-628`; `app/core/juizes.py:343-353`.)
- **Afastado exige pergunta mais precisa:** `situacao='afastado'` permanece nas choices, mas o modelo declara que afastamento deixou de ser valor de cadastro e é lido do juiz. (`app/colaboradores/models.py:295-299`; `:444-449`.) O juiz registrado é `afastado_hoje`, que aceita data e exige `para`; tipos atualmente estreitados a `afastamento_inss`. (`app/core/juizes.py:272`; `app/ponto/turnos.py:2716-2744`.) `para='folha'` significa aprovada, `para='cobranca'` não rejeitada. (`app/ponto/turnos.py:2523-2532`.) Logo falta dizer se “tocar afastado” veta pela data do ato ou da célula e qual efeito/status; a apresentação dá precedência ao desligado, portanto não basta consultá-la depois de demitir. (`app/colaboradores/services/situacao_exibida.py:18-25`; `:56-63`.)
- **DESLIGADO-NAO-TRAVA é retentor, não licença de remoção:** hoje retorna vazio para desligado em qualquer dia da competência; informativo da rescisão só lê e declara proibido mexer em célula/batida e lançar falta automática naquela lei. O item 1 novo é autorização específica futura, não deve ser descrito como reparo de trava ainda vigente. (`app/folha/export.py:1197-1213`; `:1216-1228`; `app/docs/CORTES.md:440`.) O juiz do desligado lê só estado gravado e exclui agendado/afastado. (`app/colaboradores/services/desligamento.py:23-32`; `app/core/juizes.py:273-276`.)
- **Dinheiro está expressamente no escopo:** atualizar `FechamentoMensal` não é operação neutra. Regra consolidada exige `!` para paga e vínculo/escala retroativo; competência não exportada tem preaprovação condicionada a reversão antes, exportada intacta com hash e prova de frota no mesmo marco. Exportada só porta REGEN-EM-EXPORTADA com `!`, DIFF e trilha. (`CLAUDE.md:404-417`.) L-113 alterou as recusas históricas da L-092, mas não autoriza ignorar o item 1 que restringe expressamente exportada/paga; o texto consolidado atual é o acima. (`app/docs/LEIS.md:155`; `CLAUDE.md:411-422`.)
- **L-099 favorece cura na origem e lista para dado:** separar ESTRUTURA de CADASTRO/BATIDA está literalmente na lei; células com batida não viram caso especial de código. (`app/docs/LEIS.md:122`.)
- **Universo da guarda “tenha batida” continua ambíguo:** apuração canônica exclui retratadas, exibição usa cru. `batidas_apuraveis` confirma que pendente de DP conta, retratada não. Qual desses universos governa a proibição de apagar célula precisa ser fechado; não é lícito assumir que a lei de apuração responde pergunta de preservação de dado. (`CLAUDE.md:158-167`; `app/ponto/turnos.py:2868-2895`.)
- **Registro anterior de escopo maior:** O50-DESLIGAR-ENCERRA-TUDO absorve O50-CELULA e inclui encerrar vínculos vigentes/futuros; não transportar automaticamente esse escopo maior para esta revisão do item 1. (`app/docs/BACKLOG.md:1409`.) A preservação de último dia, turno noturno e readmissão já está declarada no complemento O50 e deve orientar os cenários do PREPARA. (`app/docs/BACKLOG.md:1411`.)

Perguntas mais fortes para AVAL-DEVOLVIDO: dependência do juiz paga; manutenção da exclusão editada e do limite direção/futuro; universo de batida e dia do turno; data/efeito do veto afastado. (`app/docs/BACKLOG.md:1978`; `app/docs/CORTES.md:470`; `CLAUDE.md:149-167`; `app/ponto/turnos.py:2523-2532`, `:2716-2744`, `:2868-2895`.)

## 16/11 15:16 · sessao ed7650d71c30 · UI-EDITOR-CALENDARIO-TRES-GRADES · verificador · origem=Code

Análise por leitura da cópia e do diff. Execução da suíte, navegador e mutantes: **[não verificado]**. Os números de produção citados nos comentários não têm demonstração no dado fornecido. (`app/escala/tests/test_editor_calendario_tres_grades.py:6`)

1. **PROCEDE — leitores e escritores identificados.**

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `dias_<tam>` | `syncHidden()` serializa `sel` em inputs. (`app/templates/escala/folga_calendario_editar.html:113`) | A view lê, filtra `1..tam` e entrega à porta; a porta persiste. (`app/escala/views.py:342`, `app/escala/views.py:347`, `app/escala/views.py:356`, `app/escala/folgas.py:594`) |
| `data-fc-vazia` | HTML inicial usa `t.marcados`; JS atualiza pela seleção atual. (`app/templates/escala/folga_calendario_editar.html:44`, `app/templates/escala/folga_calendario_editar.html:100`) | `_vazia()` nos testes. O guarda consulta diretamente `sel.size`. (`app/escala/tests/test_editor_calendario_tres_grades.py:44`, `app/templates/escala/folga_calendario_editar.html:140`) |
| `fc-marca-vazia` | Template cria o texto e a classe; `renderGrade()` atualiza a visibilidade. (`app/templates/escala/folga_calendario_editar.html:50`, `app/templates/escala/folga_calendario_editar.html:99`) | Usuário recebe o texto; teste procura o span fora dos scripts. (`app/templates/escala/folga_calendario_editar.html:51`, `app/escala/tests/test_editor_calendario_tres_grades.py:51`) |
| `sel[tam]` | Inicialização pelo seed, `fcTog()` e `fcRepetir()`. (`app/templates/escala/folga_calendario_editar.html:82`, `app/templates/escala/folga_calendario_editar.html:122`, `app/templates/escala/folga_calendario_editar.html:127`) | Grade, contagem, marca, chips, inputs, repetir e confirmação. (`app/templates/escala/folga_calendario_editar.html:89`, `:97`, `:116`, `:127`, `:140`) |
| `modo` | Radios; contexto vindo do calendário; persistência pela porta. (`app/templates/escala/folga_calendario_editar.html:20`, `app/escala/views.py:370`, `app/escala/folgas.py:585`, `app/escala/folgas.py:590`) | View, porta, guarda e alternância visual. (`app/escala/views.py:325`, `app/escala/folgas.py:584`, `app/templates/escala/folga_calendario_editar.html:139`, `:175`) |
| `t.marcados` | View projeta `cal.tamanhos.all()`/`t.dias`; ausência vira conjunto vazio. (`app/escala/views.py:362`, `app/escala/views.py:366`) | Estado inicial, contagem, marca e seed do JS. (`app/templates/escala/folga_calendario_editar.html:44`, `:49`, `:50`, `:77`) |

A origem persistida também pode nascer pelo minicadastro já existente; ele permanece em `folgas.py`. Isso não foi criado pelo diff. (`app/escala/folgas.py:645`, `app/escala/folgas.py:659`)

2. **NÃO PROCEDE — segundo escritor, juiz novo ou diálogo nativo.**

A view continua chamando `salvar_calendario_de_folga`; a gravação permanece nessa porta. As duas entidades têm `escala/folgas.py` como arquivo autorizado de escrita. (`app/escala/views.py:354`, `app/escala/folgas.py:594`, `app/core/portas.py:472`)

A seleção editável produz tanto a apresentação quanto os campos enviados e a pergunta sobre vazio; não surgiu outra derivação das datas efetivas de folga. Essa derivação continua em `datas_que_o_calendario_manda()`. (`app/templates/escala/folga_calendario_editar.html:97`, `:116`, `:140`; `app/escala/folgas.py:43`)

O diálogo utilizado é `hxConfirmar`, sem `confirm/alert/prompt/hx-confirm`. O próprio selo distingue essas chamadas. (`app/templates/escala/folga_calendario_editar.html:146`, `app/core/tests/test_contract_l8_dialogo.py:41`)

3. **PROCEDE COM RESSALVA — existe confirmação que pode ficar latente.**

No fluxo normal, clique, Enter e `requestSubmit()` encontram o guarda do formulário. Em `dias_mes`, grade vazia executa `preventDefault()` antes de chamar o modal; Cancelar não reenvia. (`app/templates/escala/folga_calendario_editar.html:137`, `:139`, `:143`, `:146`, `:147`)

Modal indisponível não libera esse envio: o cancelamento já aconteceu; quando o componente consegue responder à falta de elementos, devolve falso sem `naoBloquear`. (`app/templates/escala/folga_calendario_editar.html:143`, `:146`; `app/templates/core/_dialogo.html:184`)

**Contraexemplo por inspeção:** abrir a pergunta com nome válido, apagar o nome enquanto ela está pendente e confirmar. O `requestSubmit()` pode ser impedido pelo `required`, deixando `confirmado=true`; preencher novamente e salvar consome essa autorização antiga sem perguntar novamente. O booleano só é limpo no próximo evento `submit`. Reprodução em navegador: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:148`, `:149`; `app/templates/core/_dialogo.html:160`, `:195`)

Em `semana_dia`, uma nova submissão não abre o modal. Entretanto, uma pergunta aberta anteriormente em `dias_mes` não é fechada por `fcModo()` e pode permanecer após uma troca de modo pelo teclado. Reprodução: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`; `app/templates/core/_dialogo.html:193`)

Há outra inconsistência na sequência **trocar para semana → editar nome**: `fcModo()` limpa `dias_*`, mas o input do nome chama `renderTudo()` e os repõe. A view ignora esses campos em `semana_dia`; isso não demonstra gravação indevida. (`app/templates/escala/folga_calendario_editar.html:112`, `:130`, `:178`; `app/escala/views.py:327`)

4. **NÃO PROCEDE — repetir fora do tamanho pelo fluxo normal.**

Os dias clicáveis nascem em `1..t.n`; repetir conserva somente `d <= t.n`, exatamente o corte do criador. (`app/templates/escala/folga_calendario_editar.html:90`, `:127`; `app/escala/folgas.py:661`)

O corte não valida o limite inferior, e a leitura do legado não saneia os dias. Portanto, garantia absoluta para calendários previamente malformados não está demonstrada; existência desses registros: **[não verificado]**. (`app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:127`)

5. **PROCEDE — os quatro mutantes indicados têm mordida identificável, mas há lacunas.**

As falhas abaixo são previstas pela leitura; não são resultados de execução. **[não verificado]**

| Mutante | Asserção que deve falhar |
|---|---|
| Tirar a marca | Contagem de marcas vazias; se todas ficarem visíveis, contraponto do calendário completo. (`app/escala/tests/test_editor_calendario_tres_grades.py:99`, `:109`) |
| Tirar o guarda de submit | Exigência de modal e de `SEM-POST` ao cancelar. (`app/escala/tests/test_smoke_clique_folga_calendario.py:174`, `:183`, `:186`) |
| Repetir sem corte | Comparação dos campos capturados **antes** da filtragem da view. (`app/escala/tests/test_smoke_clique_folga_calendario.py:195`, `:196`) |
| Sonda em captura | Cancelar encontrará POST capturado em vez de `SEM-POST`. A sonda atual respeita `defaultPrevented` em bolha. (`app/escala/tests/test_smoke_clique_folga_calendario.py:51`, `:57`, `:186`) |

**Lacuna relevante:** esconder as seções 30/28 pode manter os testes verdes. O teste das três grades verifica seções/IDs; a marca “visível” considera apenas `hidden` no próprio span; o smoke usa `.click()` programático sem medir visibilidade. Assim, a promessa “três grades ao mesmo tempo” não está protegida contra ocultação. (`app/escala/tests/test_editor_calendario_tres_grades.py:82`, `:48`; `app/escala/tests/test_smoke_clique_folga_calendario.py:50`)

Há vacuidade **local** na contagem de registros antes/depois de Cancelar: esse teste nunca chama `_reenvia()`, que realiza o POST Django. A asserção `SEM-POST` continua útil. (`app/escala/tests/test_smoke_clique_folga_calendario.py:162`, `:179`, `:186`, `:187`)

A regex de `hxConfirmar` não prova execução; o `wraps` prova uma chamada real no cadastro, mas não exclui um segundo update redundante nem cobre esse spy na edição. (`app/escala/tests/test_editor_calendario_tres_grades.py:118`, `app/escala/tests/test_folga_cadastro.py:19`, `:43`, `:45`)

6. **PROCEDE — infração à L7.**

A ajuda adicional “Cada tamanho de mês...” está hardcoded em um `<div>`, fora de tooltip/componente e de mapa compartilhado. Isso confronta diretamente a L7. A marca visível exigida pelo aval é questão distinta. (`app/templates/escala/folga_calendario_editar.html:40`, `app/docs/LEIS-UI.md:77`)

**NÃO PROCEDE — infração identificada aos selos estruturais examinados.** Não há diálogo nativo ou `title` cru novo; não identifiquei atributo inválido, fonte Python ou comentário vazando. Resultado executado dos selos: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:50`, `:146`; `app/core/tests/test_contract_l8_dialogo.py:42`, `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`)

7. **NÃO PROCEDE — relógio novo fora da borda ou dois significados para vazio.**

`t.marcados` é a seleção carregada; `sel[tam]`, sua versão editável. Marca e atributo significam “seleção vazia” e acompanham o mesmo conjunto. Não significam “a pessoa não terá nenhuma outra origem de folga”. (`app/escala/views.py:362`, `app/templates/escala/folga_calendario_editar.html:51`, `:82`, `:97`, `:100`, `:145`)

O relógio civil permanece na porta e é repassado aos consumidores. A classe “selo sem mordida” procede parcialmente pelas lacunas descritas no item 5. (`app/escala/folgas.py:599`, `app/escala/folgas.py:601`; `app/escala/tests/test_editor_calendario_tres_grades.py:82`)

**VEREDITO: DEVOLVE 3, 5, 6.**

## 16/11 15:15 · sessao 5db8ccbd2ba6 · - · - · origem=Ronald (tmux)

Auditoria por leitura; testes não executados. [não verificado] [nao verificado]

**5 — PROCEDE: os quatro mutantes indicados têm mordida identificável.**

| Mutante | Teste que fica vermelho | [nao verificado]
|---|---|
| Retirar a marca | `app/escala/tests/test_editor_calendario_tres_grades.py:99`; remover apenas condição e deixar todas visíveis falha em `:109`. |
| Retirar guarda do submit | Smoke exige modal em `app/escala/tests/test_smoke_clique_folga_calendario.py:174` e `:183`; Cancelar exige `SEM-POST` em `:186`. A regex isolada `test_editor_calendario_tres_grades.py:121` sobreviveria se a chamada continuasse em código morto. |
| Repetir sem corte | FormData anterior à view precisa conter dias exatos em `app/escala/tests/test_smoke_clique_folga_calendario.py:195` e `:196`; a persistência isolada esconderia esse defeito, conforme `:193`. |
| Sonda em captura | Registraria o evento antes do guarda; Cancelar encontraria JSON em vez de `SEM-POST`, falhando em `app/escala/tests/test_smoke_clique_folga_calendario.py:186`. Ordem atual: listener de documento em bolha e respeito a `defaultPrevented` em `:51–57`; guarda no form em `app/templates/escala/folga_calendario_editar.html:137`. |

**PROCEDE: existem limites e afirmações excessivas nos selos.**

- A contagem antes/depois em Cancelar é vacuamente verdadeira quanto à persistência: esse teste não chama `_reenvia`, único ponto que POSTa ao Django; `app/escala/tests/test_smoke_clique_folga_calendario.py:162`, `:179`, `:187`. O `SEM-POST` continua sendo uma asserção útil, `:186`.
- “Três grades ao mesmo tempo” não está protegido contra ocultação: teste verifica seções/IDs, `app/escala/tests/test_editor_calendario_tres_grades.py:82`; `_marcas_visiveis` considera apenas `hidden` do próprio span, `:48`. Esconder seções 30/28 com CSS pode sobreviver, pois smoke usa `.click()` programático sem medir visibilidade, `app/escala/tests/test_smoke_clique_folga_calendario.py:50`.
- `wraps` e `call_count` provam chamada real e resultado do cadastro, não ausência universal de outros escritores: um segundo update com valores iguais sobreviveria; `app/escala/tests/test_folga_cadastro.py:43`, `:45`, `:49`. Edição não passa por esse spy, `:19`.

**6 — PROCEDE: L7 merece devolução.**

A ajuda adicional é prosa fixa em `<div>`, fora de tooltip/componente/mapa: `app/templates/escala/folga_calendario_editar.html:40`; L7 proíbe ajuda hardcoded por tela em `app/docs/LEIS-UI.md:77`. O texto de confirmação usa o componente autorizado, mas continua fixo no template sem mapa compartilhado: `app/templates/escala/folga_calendario_editar.html:144`. Não conte o texto visível da marca como infração por ser visível: foi exigido expressamente pelo usuário; sua implementação está em `:50`.

**NÃO PROCEDE: diálogo nativo/title cru novos.** A chamada é `hxConfirmar`, `app/templates/escala/folga_calendario_editar.html:146`; o varredor distingue essa chamada de `confirm/alert/prompt/hx-confirm`, `app/core/tests/test_contract_l8_dialogo.py:41`. A marca tem texto e não usa `title`, `app/templates/escala/folga_calendario_editar.html:50`.

**NÃO PROCEDE: violação estrutural identificada nos varredores examinados.** Novos elementos não trazem atributos inválidos, fonte Python ou comentário vazando; critérios em `app/core/tests/test_contract_template_bem_formado.py:162`, `:169`, `:174`. A regex real de interpolação crua não encontrou correspondência neste template, critério `:123` e teste `:180`; verificação feita em memória, sem arquivo. A confirmação resolve `hxConfirmar` no handler, `app/templates/escala/folga_calendario_editar.html:146`, sem captura no load proibida por `app/core/tests/test_hx_nao_se_captura_no_load.py:21`.

**7 — NÃO PROCEDE: relógio fora da borda introduzido.** `setTimeout(200)` serve à sonda, `app/escala/tests/test_smoke_clique_folga_calendario.py:67`; não lê data/hora civil, alvo do selo em `app/core/tests/test_selo_teste_sem_relogio.py:32`.

## 16/11 15:15 · sessao 3c88f6c0f911 · - · - · origem=Ronald (tmux)

**Q3 — NÃO PROCEDE no fluxo normal; há duas ressalvas de estado.**

- Clique, Enter e `requestSubmit()` encontram o guarda instalado no formulário; grade vazia em `dias_mes` cancela o envio antes de chamar `hxConfirmar`. (`app/templates/escala/folga_calendario_editar.html:137`, `:139`, `:140`, `:143`, `:146`)
- Cancelar não reenvia; o único `form.submit()` local fica depois da resposta positiva. (`app/templates/escala/folga_calendario_editar.html:147`, `:149`)
- Modal ausente não libera envio: `preventDefault()` já aconteceu; o componente retorna falso quando faltam seus elementos e `naoBloquear` não foi passado. Se a função inteira estiver indisponível, a exceção também acontece depois do cancelamento. (`app/templates/escala/folga_calendario_editar.html:143`, `:146`; `app/templates/core/_dialogo.html:184`)
- Submeter em `semana_dia` não abre confirmação. Voltar para `dias_mes` reconstitui os campos da seleção. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`, `:178`, `:179`)

**Ressalva 1 — PROCEDE: confirmação pode ficar latente.** `confirmado=true` só é consumido pelo próximo evento `submit`. Se o `requestSubmit()` não produzir esse evento por validação inválida, a autorização fica para uma tentativa posterior. Contraexemplo por inspeção: abrir o modal com nome válido, alcançar o nome pelo teclado e apagá-lo enquanto a promessa está pendente, confirmar, preencher novamente e salvar; não haverá nova pergunta. O componente não prende o foco nem torna o formulário inerte. Esse caminho já passou por uma confirmação anterior; não equivale a “nunca chamou `hxConfirmar`”. Execução em navegador: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:14`, `:138`, `:148`, `:149`; `app/templates/core/_dialogo.html:119`, `:160`, `:195`)

**Ressalva 2 — PROCEDE: trocar modo e depois editar nome repõe `dias_*`.** `fcModo()` limpa os campos em `semana_dia`, mas editar o nome chama `renderTudo()` → `syncHidden()` sem conferir o modo. A view ignora esses dias ao processar `semana_dia`, portanto isso não prova gravação indevida. (`app/templates/escala/folga_calendario_editar.html:112`, `:117`, `:130`, `:178`; `app/escala/views.py:327`, `:339`)

**Modal visível depois da troca para `semana_dia`:** pode permanecer um modal aberto anteriormente em `dias_mes`, pois `fcModo()` não o fecha; nova submissão em `semana_dia` não o abre. Reprodução por teclado: **[não verificado]**. (`app/templates/escala/folga_calendario_editar.html:139`, `:174`; `app/templates/core/_dialogo.html:193`)

## 16/11 15:14 · sessao c243c82dc789 · - · - · origem=Ronald (tmux)

1. **NÃO PROCEDE — duplicidade de autoridade nos campos.**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `dias_<tam>` | `syncHidden`, a partir de `sel[tam]`: `app/templates/escala/folga_calendario_editar.html:113` e `:117` | View lê POST em `app/escala/views.py:342`, valida em `:347`, repassa à porta em `:356`; porta persiste em `app/escala/folgas.py:593` |
| `data-fc-vazia` | SSR com `t.marcados`: template `:44`; atualização por `arr.length`: `:100` | Teste `_vazia`: `app/escala/tests/test_editor_calendario_tres_grades.py:44`; confirmação não consulta esse atributo, consulta `sel.size`: template `:140` |
| `fc-marca-vazia` | Elemento/classe no SSR: template `:50`; visibilidade atualizada por `arr.length`: `:99` | Leitura do HTML pelo teste: `app/escala/tests/test_editor_calendario_tres_grades.py:51`; usuário recebe texto em template `:51` |
| `sel[tam]` | Inicialização do seed: template `:82`; interação: `:122`; repetir: `:127` | Grade `:89`, ordenação `:85`, campos POST `:116`, confirmação `:140`, origem do repetir `:127` |
| `modo` | Radios template `:20`; persistência na porta `app/escala/folgas.py:585` e `:590`; contexto view `:370` | View POST `:325`; guarda JS template `:139`; troca visual `:175`; porta `app/escala/folgas.py:584` |
| `t.marcados` | View projeta `cal.tamanhos.all()` em `app/escala/views.py:362` e `:366`; origem persistida `app/escala/folgas.py:595` | SSR template `:44`, `:49`, `:50`; seed `:77`, posteriormente convertido em `sel`: `:82` |

2. **NÃO PROCEDE — segundo escritor persistente, juiz novo ou derivação decisória paralela.** A view continua entrando pela porta em `app/escala/views.py:354`; escrita fica em `app/escala/folgas.py:571` e `:594`. `data-fc-vazia`, marca e contagem são projeções do mesmo conjunto em template `:97`–`:100`; o guarda consulta esse conjunto em `:140`. A distinção entre lookup e registro vivo consta em `app/docs/JUIZES.md:7` e `:11`.

4. **NÃO PROCEDE — repetir sem corte no fluxo normal da UI.** Dias clicáveis nascem em `1..t.n`, template `:90`; o repetir filtra `d <= t.n`, `:127`, igual ao criador em `app/escala/folgas.py:661`. Ambos deixam de validar o limite inferior nesse corte; seed legado é carregado sem saneamento em `app/escala/views.py:362`. Existência de legado com dia zero/negativo: **[não verificado]**.

7. **NÃO PROCEDE — campo com dois significados ou relógio novo.** `t.marcados` representa a seleção carregada, `sel[tam]` a seleção editável; atributo, marca e contagem acompanham sua cardinalidade em template `:82`, `:97`–`:100`. O relógio existente permanece na porta em `app/escala/folgas.py:599`, repassado em `:601`.

**Observação de L7:** a nova ajuda hardcoded em `app/templates/escala/folga_calendario_editar.html:41` confronta a exigência de ajuda em componente/mapa de `app/docs/LEIS-UI.md:77`–`:85`; isso é distinto da marca visível explicitamente solicitada.

## 16/11 14:45 · sessao 427f2c880889 · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. A saída informa **`TOTAL|parte=3/3|dias=19|ok=19|erro=0`** e **`rc_sombra=0`**. A linha desses registros no arquivo não foi fornecida: [nao verificado]. **Disparo de tripwire: [nao verificado]**; não há resultado explícito desse controle no trecho apresentado.

2. Na tabela, **espelho = `minutos_realizados` e classe = `STATUS.veredito`**; o E6 é o inteiro da linha do universo, confirmado por `confere_e6_arquivo=true`. (`app/ponto/services/espelho.py:783-819,764-895`; `app/ponto/management/commands/e6_oraculo.py:263-269,401`)

| Código | Data | Espelho: minutos; classe | E6 universo | Categoria/causa apontada pelos detalhes |
|---|---|---|---:|---|
| col220 | 2026-11-10 | 337; `trabalhou` | 430 | **Pareamento e desconto diferentes:** espelho usa turno até 04:31 e desconta 60 min; E6 inclui 06:00 e forma dois pares, com pausa de 55,643 min. (`estrutura_dias.txt:39`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-09 | 551; `trabalhou` | 597 | **Pausa abaixo do piso:** espelho não reconhece a pausa curta e desconta 60 min declarados; E6 desconta 13,556 min entre batidas. (`estrutura_dias.txt:40`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-10 | 551; `trabalhou` | 598 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 13,463 min no E6. (`estrutura_dias.txt:41`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-11 | 550; `trabalhou` | 598 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 12,814 min no E6. (`estrutura_dias.txt:42`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col252 | 2026-11-13 | 551; `trabalhou` | 597 | **Pausa abaixo do piso:** desconto de 60 min no espelho versus 13,942 min no E6. (`estrutura_dias.txt:43`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-247`) |
| col296 | 2026-11-02 | 345; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 02 um turno de 03/11, 00:15–07:00, calculando 405−60; E6 tem zero turnos no dia 02. (`estrutura_dias.txt:44`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col37 | 2026-11-14 | 241; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 14 um turno de 15/11, 02:00–07:01, calculando 301−60; E6 tem zero turnos no dia 14. (`estrutura_dias.txt:45`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col382 | 2026-11-10 | 418; `trabalhou` | 0 | **Associação ao dia divergente:** espelho usa turno de 11/11, 00:02–07:51, com pausa reconhecida de 51 min: 469−51; E6 tem zero turnos no dia 10. (`estrutura_dias.txt:46`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col516 | 2026-10-23 | 361; `folga` | 0 | **Folga com trabalho apurado:** espelho conta 01:00–07:01, sem desconto; célula registra `fato_sem_previsao` e há alerta `folga_trabalhada`. E6 tem zero turnos nesse dia; causa completa desse zero: [nao verificado]. (`estrutura_dias.txt:47`; `app/ponto/turnos.py:725-767`; `app/escala/models.py:1652-1701`; `app/ponto/services/espelho.py:450-460,767`) |
| col516 | 2026-10-28 | 372; `trabalhou` | 0 | **Associação ao dia divergente:** espelho atribui ao dia 28 o turno de 29/10, 00:43–06:55, sem desconto; E6 tem zero turnos no dia 28. (`estrutura_dias.txt:48`; `app/ponto/services/espelho.py:267-294`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col727 | 2026-11-10 | 307; `em_aberto` | 0 | **Associação ao dia e classificação divergentes:** espelho atribui ao dia 10 o turno fechado de 11/11, 01:00–07:07, calculando 367−60; mantém classe `em_aberto`. E6 tem zero turnos no dia 10; o motor também aponta `dia_sem_previsto`/`folga_trabalhada`. (`estrutura_dias.txt:49`; `app/ponto/services/espelho.py:764-895,450-460`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col874 | 2026-10-22 | 667; `trabalhou` | 383 | **Corte e pareamento diferentes:** espelho conta 10:00–22:07 menos 60 min; no E6, 10:00 fecha fragmento iniciado em 21/10, e o dia 22 recebe somente 15:45–22:07. (`estrutura_dias.txt:50`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col884 | 2026-11-08 | 679; `rejeitado` | 699 | **Desconto e estado de ausência:** espelho desconta 60 min sem pausa reconhecida; E6 desconta 39,546 min posicionais. Há atestado `rejeitada` cobrindo o dia. (`estrutura_dias.txt:51`; `app/ponto/turnos.py:725-767,2798-2814`; `app/ponto/calculador/nucleo.py:224-247`; `app/ponto/models.py:690-705,743-754`) |
| col909 | 2026-11-11 | 831; `em_aberto` | 245 | **Borda posicional versus cortes:** realizado usa 07:00–23:00 menos 129 min, embora haja um turno fechado e outro aberto. E6 conta apenas 07:00–11:04 e registra `impar=true`, `corte_que_orfanou=true`. (`estrutura_dias.txt:52`; `app/ponto/turnos.py:598-601,707-767`; `app/ponto/calculador/nucleo.py:215-254`) |
| col922 | 2026-11-01 | 1382; `trabalhou` | 68 | **Turnos e pares diferentes:** realizado soma 901+481 min, sem desconto; espelho exibe apenas o turno noturno. E6 atribui ao dia somente o par 22:01–23:09, entre uma saída e uma entrada. (`estrutura_dias.txt:53`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col922 | 2026-11-04 | 676; `trabalhou` | 458 | **Turno adicional no realizado:** soma 218 min da madrugada e 458 min do turno noturno; espelho exibe só o noturno. E6 mantém a madrugada no fragmento iniciado em 03/11 e atribui ao dia 04 apenas o noturno. (`estrutura_dias.txt:54`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col989 | 2026-10-23 | 721; `trabalhou` | 567 | **Pareamento deslocado:** espelho conta entrada 17:59 até saída 06:00 seguinte, sem desconto; E6 pareia a saída 08:32 com a entrada 17:59 e corta antes de 06:00. (`estrutura_dias.txt:55`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col996 | 2026-10-28 | 0; `folga` | 580 | **Saída órfã versus par posicional:** juiz apresenta apenas saída às 21:18, sem entrada, e o espelho não exibe turno; E6 pareia essa saída com a entrada de 29/10 às 06:57. (`estrutura_dias.txt:56`; `app/ponto/services/espelho.py:267-294,450-460`; `app/ponto/calculador/nucleo.py:224-247`) |
| col996 | 2026-11-10 | 443; `em_aberto` | 1026 | **Par adicional no E6:** realizado conta o turno diurno, 533−90; mantém saída órfã às 21:16. E6 acrescenta o par dessa saída com a entrada de 11/11 às 06:59. (`estrutura_dias.txt:57`; `app/ponto/turnos.py:725-767`; `app/ponto/services/espelho.py:267-294`; `app/ponto/calculador/nucleo.py:224-247`) |

3. **Dias com `ERRO`: nenhum segundo o TOTAL informado (`erro=0`); portanto, nenhuma exclusão numérica por ERRO nesta parte.** Arquivo:linha do registro TOTAL: [nao verificado].

## 16/11 14:41 · sessao c91636553c1d · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. **TOTAL da parte:** `N=19; ok=19; erro=0; rc_sombra=0`, conforme as linhas finais fornecidas; localização dessas linhas em arquivo:linha **[nao verificado]**. **Tripwire:** disparo em algum dia **[nao verificado]**; `rc_sombra=0` não fornece, sozinho, esse resultado.

2. A classe abaixo é `STATUS.veredito`; os minutos do espelho são `minutos_realizados`. (`app/ponto/services/espelho.py:764-819`) A coluna e6 usa o inteiro conferido contra a linha do universo indicada em cada registro. (`app/ponto/management/commands/e6_oraculo.py:263-269,401`)

| Código, data | Espelho: minutos / classe | e6 do universo | Categoria/causa evidenciada na saída |
|---|---:|---:|---|
| col723, 2026-10-08 | 440 / trabalhou | 345 (`estrutura_dias.txt:20`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados, sem pausa reconhecida; e6 desconta 155,277 min entre os dois pares posicionais. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col746, 2026-09-24 | 289 / trabalhou | 193 (`estrutura_dias.txt:21`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados; e6 desconta 156,819 min entre os pares. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col841, 2026-10-20 | 681 / em_aberto | 522 (`estrutura_dias.txt:22`) | **Agrupamento e pareamento diferentes:** juiz mantém uma saída isolada às 11:00 e o turno 18:46–07:04, com desconto de 57 min; e6 pareia 11:00–18:46 e 21:44–22:41, cortando antes da saída das 07:04. (`app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col854, 2026-10-04 | 718 / trabalhou | 7 (`estrutura_dias.txt:23`) | **Corte do e6:** realizado conta 18:54–06:52, sem desconto; e6 pareia somente 18:54–19:01 e corta antes das 06:52, após um gap de 711,349 min. (`app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-247`) |
| col864, 2026-10-12 | 509 / feriado | 449 (`estrutura_dias.txt:24`) | **Intervalo sem desconto no realizado:** nenhuma pausa reconhecida e nenhuma janela declarada usada; e6 desconta 59,804 min. A célula consta como folga, sem marcos. (`app/ponto/turnos.py:707-771`; `app/ponto/calculador/nucleo.py:224-254`; `app/escala/models.py:1652-1701,1718-1724`) |
| col868, 2026-09-22 | 0 / em_aberto | 679 (`estrutura_dias.txt:25`) | **Formação de turno diferente:** juiz tem apenas saída sem entrada e nenhum turno exibido; e6 agrupa quatro batidas entre 18:48 e 07:01 do dia seguinte. O cadastro mostrado prevê jornada diurna 6x1; dono dessa divergência **[nao verificado]**. (`app/ponto/turnos.py:502-567,719`; `app/ponto/services/espelho.py:450-460,767`; `app/ponto/calculador/nucleo.py:224-247`; `app/escala/models.py:103-138,170-175`) |
| col876, 2026-09-21 | 550 / em_aberto | 665 (`estrutura_dias.txt:26`) | **Conjunto de batidas diferente:** realizado conta 18:29–04:39 menos 60 min e mantém a saída das 06:31 sem entrada; e6 também usa 05:36 e conta o segundo par até 06:31. Motivo de 05:36 não aparecer no turno do juiz **[nao verificado]**. (`app/ponto/turnos.py:502-567,725-771`; `app/ponto/calculador/nucleo.py:224-247`) |
| col1021, 2026-11-06 | 841 / folga | 0 (`estrutura_dias.txt:27`) | **Corte que orfanou no e6:** realizado mantém 05:55–19:56, sem desconto; e6 separa as duas batidas em fragmentos unitários, com `impar=true` e `corte_que_orfanou=true`. (`app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:215-254`) |
| col110, 2026-11-12 | 241 / em_aberto | 0 (`estrutura_dias.txt:28`) | **Atribuição ao dia:** juiz atribui a 12/11 o trecho 01:59–07:00 de 13/11 e desconta 60 min; e6 não tem turno atribuído a 12/11. Destino dessas batidas no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col112, 2026-10-21 | 546 / trabalhou | 838 (`estrutura_dias.txt:29`) | **Pareamento noturno no e6:** realizado conta 06:57–17:03 menos 60 min; e6 pareia a saída das 17:03 com a entrada das 07:01 de 22/10. A entrada das 06:57 aparece no fragmento anterior. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:215-247`) |
| col112, 2026-10-22 | 541 / trabalhou | 835 (`estrutura_dias.txt:30`) | **Pareamento noturno no e6:** realizado conta 07:01–17:02 menos 60 min; e6 pareia 17:02 com 06:56 de 23/10. A entrada das 07:01 aparece no fragmento anterior. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:215-247`) |
| col113, 2026-10-30 | 242 / em_aberto | 0 (`estrutura_dias.txt:31`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:00 e conta 03:00–07:02 de 31/10 em 30/10, sem desconto; e6 não tem turno em 30/10. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col113, 2026-11-01 | 241 / em_aberto | 0 (`estrutura_dias.txt:32`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:01 e conta 03:00–07:01 de 02/11 em 01/11, sem desconto; e6 não tem turno em 01/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |
| col114, 2026-11-08 | 439 / trabalhou | 429 (`estrutura_dias.txt:33`) | **Desconto de intervalo diferente:** realizado desconta 60 min declarados, sem pausa reconhecida; e6 desconta 70,666 min entre os pares. (`app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col114, 2026-11-15 | 499 / feriado | 440 (`estrutura_dias.txt:34`) | **Intervalo sem desconto no realizado:** realizado conta o envelope sem pausa reconhecida ou janela declarada; e6 desconta 59,538 min. (`app/ponto/turnos.py:707-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col130, 2026-10-29 | 335 / trabalhou | 672 (`estrutura_dias.txt:35`) | **Conjunto e pareamento diferentes:** juiz encerra às 01:31 e desconta 60 min; e6 inclui 07:06 e forma os pares 18:56–00:33 e 01:31–07:06. Motivo da exclusão de 07:06 pelo juiz **[nao verificado]**. (`app/ponto/turnos.py:502-567,725-771`; `app/ponto/calculador/nucleo.py:224-247`) |
| col200, 2026-10-29 | 180 / em_aberto | 0 (`estrutura_dias.txt:36`) | **Atribuição ao dia:** juiz conta 02:00–06:00 de 30/10 em 29/10 e desconta 60 min; e6 não tem turno em 29/10. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col200, 2026-11-02 | 180 / em_aberto | 0 (`estrutura_dias.txt:37`) | **Atribuição ao dia:** juiz mantém uma saída isolada e conta 02:00–06:00 de 03/11 em 02/11, menos 60 min; e6 não tem turno em 02/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-771`; `app/ponto/calculador/nucleo.py:224-254`) |
| col218, 2026-11-14 | 103 / em_aberto | 0 (`estrutura_dias.txt:38`) | **Atribuição ao dia:** juiz mantém saída isolada às 02:17 e conta 03:17–05:00 de 15/11 em 14/11, sem desconto; e6 não tem turno em 14/11. Destino no e6 **[nao verificado]**. (`app/ponto/turnos.py:502-567,719,725-767`; `app/ponto/calculador/nucleo.py:224-254`) |

**Dono ORACULO/SISTEMA/CADASTRO por causa:** **[nao verificado]**; os mecanismos acima não bastam para atribuir responsabilidade definitiva.

3. **Dias com ERRO:** nenhum registrado no `TOTAL` fornecido (`erro=0`); arquivo:linha desse registro **[nao verificado]**. Portanto, não há dia listado para invalidar por ERRO nessa parte; arquivo:linha **[nao verificado]**.

## 16/11 14:38 · sessao af08e3068de7 · PRIORIDADE-EV-ITEM1 · sonda · origem=Code

1. **TOTAL da parte:** `N=19 | ok=19 | erro=0 | rc_sombra=0`, conforme os registros finais fornecidos; arquivo:linha desses registros **[nao verificado]**. **Tripwire:** disparo **[nao verificado]**; não há resultado explícito dele na saída.

2. Na tabela, **minutos do espelho = `minutos_realizados`** e **classe = `STATUS.veredito`**, conforme as referências da sonda: `app/ponto/services/espelho.py:783-819,764-895`. As categorias abaixo descrevem os mecanismos apresentados, sem atribuir dono definitivo **[nao verificado]**.

| Código | Data | Espelho: minutos; classe | e6 do universo | Categoria/causa apontada |
|---|---|---|---:|---|
| col129 | 2026-10-12 | 0; `em_aberto` | 779 | **Pareamento diferente:** juiz recebeu duas saídas sem entrada; e6 pareou a saída de 17:30 com a saída de 06:30 do dia seguinte. `estrutura_dias.txt:1`; `app/ponto/turnos.py:502-567,719`; `app/ponto/calculador/nucleo.py:224-247`. |
| col218 | 2026-10-15 | 105; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 21h; espelho conta o par 03:15–05:00 do dia 16 no dia 15; e6 não apresenta turno no dia 15. `estrutura_dias.txt:2`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-23 | 548; `trabalhou` | 594 | **Desconto de intervalo:** realizado desconta 60 min declarados; pausa de 13,3619 min fica abaixo do piso reconhecido; e6 desconta essa pausa. `estrutura_dias.txt:3`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-24 | 552; `trabalhou` | 598 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 13,569383 min é descontada pelo e6. `estrutura_dias.txt:4`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-25 | 556; `trabalhou` | 604 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 12,438 min é descontada pelo e6. `estrutura_dias.txt:5`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-09-28 | 535; `trabalhou` | 581 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,634152 min é descontada pelo e6. `estrutura_dias.txt:6`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-01 | 545; `trabalhou` | 590 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,443733 min é descontada pelo e6. `estrutura_dias.txt:7`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-02 | 545; `trabalhou` | 593 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 13,069783 min é descontada pelo e6. `estrutura_dias.txt:8`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-06 | 551; `trabalhou` | 597 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 14,038883 min é descontada pelo e6. `estrutura_dias.txt:9`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col252 | 2026-10-09 | 544; `trabalhou` | 592 | **Desconto de intervalo:** realizado desconta 60 min; pausa abaixo do piso de 11,989283 min é descontada pelo e6. `estrutura_dias.txt:10`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col369 | 2026-10-12 | 476; `feriado` | 416 | **Intervalo não reconhecido no realizado:** célula de folga sem marcos; batidas internas E–S, nenhuma pausa reconhecida e nenhum desconto. e6 desconta 59,582783 min entre as batidas internas. `estrutura_dias.txt:11`; `app/escala/models.py:1652-1701`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col37 | 2026-10-05 | 241; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; juiz tem saída isolada às 02h e par 03:00–07:01 do dia 6 atribuído ao dia 5; e6 não apresenta turno no dia 5. `estrutura_dias.txt:12`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col37 | 2026-10-09 | 241; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 02h e par 03:00–07:01 do dia 10 atribuído ao dia 9; e6 não apresenta turno no dia 9. `estrutura_dias.txt:13`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col382 | 2026-09-30 | 364; `trabalhou` | 0 | **Atribuição do turno noturno com pausa reconhecida:** espelho atribui 01:01–07:50 do dia 1 ao dia 30 e calcula 409−45=364 min; e6 não apresenta turno no dia 30. `estrutura_dias.txt:14`; `app/ponto/turnos.py:725-771`; `app/ponto/calculador/nucleo.py:224-254`. |
| col451 | 2026-09-23 | 707; `em_aberto` | 718 | **Pareamento diferente:** espelho conta 18:58–06:45 do dia seguinte; e6 pareia a saída isolada de 07h com a entrada de 18:58 e corta antes da saída seguinte. `estrutura_dias.txt:15`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col451 | 2026-09-25 | 705; `em_aberto` | 718 | **Pareamento diferente:** espelho conta 19:00–06:45 do dia seguinte; e6 pareia a saída isolada de 07:02 com a entrada de 19h e corta antes da saída seguinte. `estrutura_dias.txt:16`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col478 | 2026-09-25 | 719; `trabalhou` | 8 | **Entrada adicional e corte do e6:** espelho usa 18:57–06:56, com a entrada de 19:04 marcada `X`; e6 pareia as duas entradas, obtém 7,766517 min e corta antes da saída. `estrutura_dias.txt:17`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:215-247`. |
| col515 | 2026-10-09 | 244; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 02h e par 02:59–07:03 do dia 10 atribuído ao dia 9; e6 não apresenta turno no dia 9. `estrutura_dias.txt:18`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |
| col515 | 2026-10-11 | 245; `em_aberto` | 0 | **Atribuição do turno noturno:** falta entrada das 19h; saída isolada às 01:50 e par 03:00–07:05 do dia 12 atribuído ao dia 11; e6 não apresenta turno no dia 11. `estrutura_dias.txt:19`; `app/ponto/services/espelho.py:764-895`; `app/ponto/turnos.py:502-567,725-767`; `app/ponto/calculador/nucleo.py:224-254`. |

3. **Dias com ERRO: nenhum**, conforme `TOTAL|parte=1/3|dias=19|ok=19|erro=0`; arquivo:linha desse registro **[nao verificado]**. Portanto, nenhuma exclusão de número por ERRO foi indicada nessa parte; arquivo:linha **[nao verificado]**.

## 16/11 14:36 · sessao a12a65a6e084 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

[nao verificado]
```text
PEND|col373|2026-08-04|08/2026|emp2
PEND|col60|2026-06-23|07/2026|emp3
PEND|col152|2026-06-23|07/2026|emp3
PEND|col152|2026-06-29|07/2026|emp3
PEND|col888|2026-08-28|09/2026|emp2
PEND|col920|2026-09-02|09/2026|emp2
PEND|col616|2026-09-05|09/2026|emp2
PEND|col920|2026-09-04|09/2026|emp2
PEND|col727|2026-09-04|09/2026|emp2
PEND|col439|2026-09-03|09/2026|emp2
PEND|col212|2026-08-26|09/2026|emp2
PEND|col112|2026-09-06|09/2026|emp3
PEND|col920|2026-09-06|09/2026|emp2
PEND|col926|2026-09-07|09/2026|emp2
PEND|col152|2026-09-07|09/2026|emp3
PEND|col236|2026-09-07|09/2026|emp2
PEND|col363|2026-09-07|09/2026|emp2
PEND|col358|2026-09-07|09/2026|emp2
PEND|col928|2026-09-07|09/2026|emp2
PEND|col878|2026-09-07|09/2026|emp2
PEND|col507|2026-09-07|09/2026|emp2
PEND|col49|2026-09-07|09/2026|emp3
PEND|col112|2026-09-08|09/2026|emp3
PEND|col920|2026-09-08|09/2026|emp2
PEND|col616|2026-09-07|09/2026|emp2
PEND|col148|2026-09-10|09/2026|emp3
PEND|col112|2026-09-10|09/2026|emp3
PEND|col920|2026-09-10|09/2026|emp2
PEND|col616|2026-09-09|09/2026|emp2
PEND|col920|2026-09-12|09/2026|emp2
PEND|col616|2026-09-11|09/2026|emp2
PEND|col862|2026-09-13|09/2026|emp2
PEND|col242|2026-09-13|09/2026|emp2
PEND|col901|2026-09-14|09/2026|emp2
PEND|col920|2026-08-27|09/2026|emp2
PEND|col920|2026-08-29|09/2026|emp2
PEND|col920|2026-08-31|09/2026|emp2
PEND|col207|2026-09-05|09/2026|emp2
PEND|col869|2026-09-06|09/2026|emp2
PEND|col869|2026-09-13|09/2026|emp2
PEND|col899|2026-09-05|09/2026|emp2
PEND|col903|2026-09-14|09/2026|emp2
PEND|col129|2026-08-30|09/2026|emp3
PEND|col129|2026-09-08|09/2026|emp3
PEND|col129|2026-09-13|09/2026|emp3
PEND|col129|2026-09-15|09/2026|emp3
PEND|col382|2026-09-03|09/2026|emp2
PEND|col491|2026-08-27|09/2026|emp2
PEND|col382|2026-09-10|09/2026|emp2
PEND|col382|2026-09-17|09/2026|emp2
PEND|col616|2026-09-17|09/2026|emp2
PEND|col901|2026-09-17|09/2026|emp2
PEND|col572|2026-09-18|09/2026|emp2
PEND|col648|2026-09-15|09/2026|emp2
PEND|col148|2026-09-13|09/2026|emp3
PEND|col902|2026-09-13|09/2026|emp2
PEND|col824|2026-09-15|09/2026|emp2
PEND|col824|2026-09-19|09/2026|emp2
PEND|col902|2026-09-19|09/2026|emp2
PEND|col499|2026-09-19|09/2026|emp2
PEND|col148|2026-09-19|09/2026|emp3
PEND|col822|2026-09-20|09/2026|emp2
PEND|col572|2026-09-20|09/2026|emp2
PEND|col491|2026-09-20|09/2026|emp2
PEND|col902|2026-09-20|09/2026|emp2
PEND|col148|2026-09-20|09/2026|emp3
PEND|col491|2026-09-21|10/2026|emp2
PEND|col616|2026-09-21|10/2026|emp2
PEND|col824|2026-09-21|10/2026|emp2
PEND|col903|2026-09-21|10/2026|emp2
PEND|col704|2026-09-21|10/2026|emp2
PEND|col857|2026-09-22|10/2026|emp4
PEND|col418|2026-09-22|10/2026|emp2
PEND|col245|2026-09-22|10/2026|emp2
PEND|col203|2026-09-22|10/2026|emp2
PEND|col616|2026-09-23|10/2026|emp2
PEND|col824|2026-09-23|10/2026|emp2
PEND|col859|2026-09-23|10/2026|emp2
PEND|col951|2026-09-24|10/2026|emp2
PEND|col616|2026-09-25|10/2026|emp2
PEND|col824|2026-09-05|09/2026|emp2
PEND|col824|2026-09-25|10/2026|emp2
PEND|col369|2026-09-25|10/2026|emp2
PEND|col885|2026-09-26|10/2026|emp2
PEND|col616|2026-09-27|10/2026|emp2
PEND|col242|2026-09-27|10/2026|emp2
PEND|col457|2026-09-27|10/2026|emp2
PEND|col155|2026-09-27|10/2026|emp3
PEND|col373|2026-09-27|10/2026|emp2
PEND|col666|2026-09-27|10/2026|emp2
PEND|col859|2026-09-02|09/2026|emp2
PEND|col859|2026-09-16|09/2026|emp2
PEND|col784|2026-09-28|10/2026|emp2
PEND|col857|2026-09-29|10/2026|emp4
PEND|col373|2026-09-29|10/2026|emp2
PEND|col89|2026-09-30|10/2026|emp3
PEND|col202|2026-10-02|10/2026|emp2
PEND|col876|2026-10-03|10/2026|emp3
PEND|col373|2026-10-06|10/2026|emp2
PEND|col131|2026-10-08|10/2026|emp3
PEND|col974|2026-10-10|10/2026|emp2
PEND|col155|2026-10-11|10/2026|emp3
PEND|col924|2026-10-10|10/2026|emp2
```

| Competência | Linhas PEND |
|---|---:|
| 07/2026 | 3 [nao verificado] |
| 08/2026 | 1 [nao verificado] |
| 09/2026 | 65 [nao verificado] |
| 10/2026 | 34 [nao verificado] |
| 11/2026 | 0 [nao verificado] |

[nao verificado]
```text
TOTAL_DIAS 253 TOTAL_PENDENTE 103 ERROS 0
FIM
```

## 16/11 14:34 · sessao 34889e7d5722 · PRIORIDADE-EV-ITEM6 · sonda · origem=Code

Referência: somente o bloco colado; sem arquivo:linha disponível, os resultados levam [nao verificado].

1. **Marcadores presentes:** `READ_ONLY on`, `ERROS 0` e `FIM`. Todas as linhas O199 têm `erro=0`; a saída passa pelo critério solicitado de integridade. [nao verificado]

2. **Conferência dos valores:** [nao verificado]

| competencia | empresa | exportada | dias | ja_folga | pendente | colabs_pendentes | referência |
|---|---:|:---:|---:|---:|---:|---:|---|
| 07/2026 | 3 | s | 3 | 0 | 3 | 2 | [nao verificado] |
| 08/2026 | 2 | s | 6 | 5 | 1 | 1 | [nao verificado] |
| 09/2026 | 2 | s | 91 | 39 | 52 | 29 | [nao verificado] |
| 09/2026 | 3 | s | 19 | 6 | 13 | 5 | [nao verificado] |
| 10/2026 | 2 | n | 74 | 47 | 27 | 20 | [nao verificado] |
| 10/2026 | 3 | n | 17 | 12 | 5 | 4 | [nao verificado] |
| 10/2026 | 4 | n | 3 | 1 | 2 | 1 | [nao verificado] |
| 11/2026 | 1 | n | 1 | 1 | 0 | 0 | [nao verificado] |
| 11/2026 | 2 | n | 29 | 29 | 0 | 0 | [nao verificado] |
| 11/2026 | 3 | n | 8 | 8 | 0 | 0 | [nao verificado] |
| 11/2026 | 4 | n | 2 | 2 | 0 | 0 | [nao verificado] |
| **TOTAL** | — | — | **253** | **150** | **103** | **62** | [nao verificado] |

**Os totais fecham:** soma de `dias=253`, soma de `pendente=103` e `150+103=253`. Em todas as linhas, `dias=ja_folga+pendente`. Os 62 são a soma de `colabs_pendentes` das linhas; quantidade de colaboradores únicos não está demonstrada. [nao verificado]

3. **Pendências e Corte C:** [nao verificado]

- **Exportadas (`s`): 69 dias pendentes**, destinados à lista do DP pelo critério solicitado. [nao verificado]
- **Abertas pelo flag (`n`): 34 dias pendentes**, todos em **10/2026**. [nao verificado]
- **11/2026: zero pendentes O199**; portanto, o recorte autorizado do Corte C apresenta **zero dias O199 para apply**, que continua condicionado ao `!` de escala. [nao verificado]

**Divergência:** o anexo chama 10 de competência paga, enquanto a saída marca 10 como `exportada=n`. Os 34 dias de outubro não podem ser considerados autorizados para apply pelo Corte C, limitado à 11. A saída também não apresenta medição identificável de `SEM-CELULA-11-DEVE-TER-CELULA (col305)`. [nao verificado]

## 16/11 14:33 · sessao fbd083b15a88 · ESPELHO-VERDADE-FECHO-V0 · leitura · origem=Code

A função é `espelho_do_colab`: a tela a chama diretamente e o PDF consome seus dias (`app/ponto/views.py:160-162`; `app/relatorios/pdf_espelho.py:596-603`). O patch abaixo intercepta somente as atribuições ao cache do catálogo, preservando sua leitura e os turnos reais (`app/ponto/catalogo/ausencias.py:275-281`).

```python
# OBRA: ESPELHO-VERDADE-FECHO-V0 — R3
# Arvore solicitada: 3580b35e028267d85318cb75fd5b3d0c40e04a00
# Texto para o envelope JA inicializado em SOMBRA/juliani.
# Execucao, desempenho e compatibilidade do script: [nao verificado].
# Auxiliares deste script, biblioteca padrao, sqlparse e implementacoes
# internas do Django fora da copia: [nao verificado].
#
# PARTE recorta estrutura_dias.txt:1-57 na ordem recebida: 19/19/19.
PARTE = 1

import sys
sys.dont_write_bytecode = True

import datetime as dt
import decimal
import json
import logging
import re
from contextlib import ExitStack, redirect_stderr, redirect_stdout
from unittest.mock import patch

NV = "[nao verificado]"
SAIDA = sys.stdout

# Mesma data civil do R1 recebido; instante/snapshot original: [nao verificado].
# O instante AGORA sera real, unico por execucao e publicado. [nao verificado]
HOJE = dt.date(2026, 11, 16)

# Transcricao dos campos identificadores e numeros de estrutura_dias.txt:1-57.
# Inclui col1021 e col909: o universo aqui e 57, nao os 55 do R2.
DADO = """col129|10/2026|2026-10-12|0|779
col218|10/2026|2026-10-15|105|0
col252|10/2026|2026-09-23|548|594
col252|10/2026|2026-09-24|552|598
col252|10/2026|2026-09-25|556|604
col252|10/2026|2026-09-28|535|581
col252|10/2026|2026-10-01|545|590
col252|10/2026|2026-10-02|545|593
col252|10/2026|2026-10-06|551|597
col252|10/2026|2026-10-09|544|592
col369|10/2026|2026-10-12|476|416
col37|10/2026|2026-10-05|241|0
col37|10/2026|2026-10-09|241|0
col382|10/2026|2026-09-30|364|0
col451|10/2026|2026-09-23|707|718
col451|10/2026|2026-09-25|705|718
col478|10/2026|2026-09-25|719|8
col515|10/2026|2026-10-09|244|0
col515|10/2026|2026-10-11|245|0
col723|10/2026|2026-10-08|440|345
col746|10/2026|2026-09-24|289|193
col841|10/2026|2026-10-20|681|522
col854|10/2026|2026-10-04|718|7
col864|10/2026|2026-10-12|509|449
col868|10/2026|2026-09-22|0|679
col876|10/2026|2026-09-21|550|665
col1021|11/2026|2026-11-06|841|0
col110|11/2026|2026-11-12|241|0
col112|11/2026|2026-10-21|546|838
col112|11/2026|2026-10-22|541|835
col113|11/2026|2026-10-30|242|0
col113|11/2026|2026-11-01|241|0
col114|11/2026|2026-11-08|439|429
col114|11/2026|2026-11-15|499|440
col130|11/2026|2026-10-29|335|672
col200|11/2026|2026-10-29|180|0
col200|11/2026|2026-11-02|180|0
col218|11/2026|2026-11-14|103|0
col220|11/2026|2026-11-10|337|430
col252|11/2026|2026-11-09|551|597
col252|11/2026|2026-11-10|551|598
col252|11/2026|2026-11-11|550|598
col252|11/2026|2026-11-13|551|597
col296|11/2026|2026-11-02|345|0
col37|11/2026|2026-11-14|241|0
col382|11/2026|2026-11-10|418|0
col516|11/2026|2026-10-23|361|0
col516|11/2026|2026-10-28|372|0
col727|11/2026|2026-11-10|307|0
col874|11/2026|2026-10-22|667|383
col884|11/2026|2026-11-08|679|699
col909|11/2026|2026-11-11|831|245
col922|11/2026|2026-11-01|1382|68
col922|11/2026|2026-11-04|676|458
col989|11/2026|2026-10-23|721|567
col996|11/2026|2026-10-28|0|580
col996|11/2026|2026-11-10|443|1026"""

TODOS = []
for numero, texto in enumerate(DADO.splitlines(), 1):
    codigo, competencia, data, esp_min, e6_min = texto.split("|")
    TODOS.append((numero, codigo, competencia, data, int(esp_min), int(e6_min)))

if PARTE not in (1, 2, 3):
    raise ValueError("ParteInvalida")
if len(TODOS) != 57 or len({r[1:4] for r in TODOS}) != 57:
    raise RuntimeError("UniversoIncompativel")
UNIVERSO = TODOS[(PARTE - 1) * 19:PARTE * 19]
N = len(UNIVERSO)
if N != 19:
    raise RuntimeError("FatiaIncompativel")


def imprimir(*partes):
    print(*partes, file=SAIDA, flush=True)


class Descartar:
    def write(self, texto):
        return len(texto)

    def flush(self):
        return None


class escrita_nao_interceptada(RuntimeError):
    pass


class CacheSomenteLeitura:
    """Proxy de escrita; a leitura vem INTEGRALMENTE do objeto original."""

    def __init__(self, original):
        self.original = original
        self.suprimidas = 0

    def __getitem__(self, chave):
        return self.original[chave]

    def __setitem__(self, chave, valor):
        if chave not in ("v", "t"):
            raise RuntimeError("EscritorDeCacheIncompativel")
        self.suprimidas += 1
        # SOMENTE as atribuicoes sao absorvidas; nenhum valor e inventado.
        # Escritor nominal: ponto.catalogo.ausencias._CACHE.__setitem__
        # nos sitios app/ponto/catalogo/ausencias.py:279-280.
        # vigente() ainda calcula v pela fonte real (:278) e retorna v (:281).


def classe_segura(exc):
    nome = type(exc).__name__
    return nome if re.fullmatch(r"[A-Za-z_][A-Za-z0-9_]{0,79}", nome) else "Excecao"


def sonda():
    ok = erro = 0
    feitos = set()
    guarda = None

    try:
        with (
            patch.object(logging.Logger, "handle", lambda *a, **k: None),
            redirect_stdout(Descartar()),
            redirect_stderr(Descartar()),
        ):
            from django.apps import apps
            from django.conf import settings
            from django.db import connection, connections, transaction
            from django.db.models import Q
            from django.utils import timezone
            import sqlparse
            from sqlparse import tokens as SQLTOK

            # Mesmo molde de ambiente do anexo r2_script_p1.py.
            # Configuracao: app/config/settings/sombra.py:49-50,65.
            # Envelope efetivo: verificado apenas quando este texto executar.
            if (
                not apps.ready
                or settings.SETTINGS_MODULE != "config.settings.sombra"
                or not settings.BANCO_DE_ENSAIO
                or connection.settings_dict["NAME"] != "sombra"
                or connection.schema_name != "juliani"
                or connection.in_atomic_block
                or not connection.get_autocommit()
            ):
                raise RuntimeError("AmbienteIncompativel")

            class GuardaSQL:
                def __init__(self):
                    self.tentou = False
                    self.permite_set_inicial = True

                def bloquear(self):
                    self.tentou = True
                    raise escrita_nao_interceptada()

                def __call__(self, execute, sql, params, many, context):
                    # execute_wrapper / sqlparse: implementacoes [nao verificado].
                    # O flag e aderente ao dia: except Exception internos nao
                    # podem converter uma tentativa de escrita em dia "ok".
                    # Exemplo de captura interna:
                    # app/ponto/catalogo/ausencias.py:246-254.
                    if context["connection"].alias != "default":
                        self.bloquear()

                    comandos = [
                        s for s in sqlparse.parse(str(sql))
                        if str(s).strip()
                    ]
                    if len(comandos) != 1:
                        self.bloquear()

                    partes = []
                    for token in comandos[0].flatten():
                        if token.is_whitespace:
                            continue
                        if token.ttype in SQLTOK.Comment:
                            continue
                        if token.ttype in SQLTOK.Literal.String:
                            continue
                        # Identificador entre aspas nao e comando.
                        if token.value.startswith('"'):
                            continue
                        partes.append(token.value)
                    plano = " ".join(partes).upper().strip().rstrip(";").strip()

                    # Inclui DML dentro de WITH e SELECT ... FOR UPDATE/SHARE.
                    # TRUNCATE/DDL/COPY/CALL tambem nao sao leitura autorizada.
                    if re.search(
                        r"\b(INSERT|UPDATE|DELETE|LOCK|MERGE|TRUNCATE|"
                        r"CREATE|ALTER|DROP|GRANT|REVOKE|COPY|CALL|DO|"
                        r"VACUUM|ANALYZE|REFRESH|REINDEX|CLUSTER|SHARE)\b",
                        plano,
                    ):
                        self.bloquear()
                    if re.search(r"\bPG_(?:TRY_)?ADVISORY_", plano):
                        self.bloquear()

                    if plano == "SET TRANSACTION READ ONLY":
                        if not self.permite_set_inicial:
                            self.bloquear()
                    elif not re.match(
                        r"^(SELECT|WITH|SHOW|SAVEPOINT|RELEASE|ROLLBACK)\b",
                        plano,
                    ):
                        self.bloquear()

                    return execute(sql, params, many, context)

            guarda = GuardaSQL()

            def serializar(valor):
                if isinstance(valor, dt.datetime):
                    # Mesmo uso local do E6:
                    # app/ponto/management/commands/e6_oraculo.py:152.
                    return timezone.localtime(valor).isoformat()
                if isinstance(valor, (dt.date, dt.time)):
                    return valor.isoformat()
                if isinstance(valor, decimal.Decimal):
                    return float(valor)
                # Nunca str(modelo), repr(modelo), nome ou texto de excecao.
                raise TypeError("TipoDeSaidaNaoAutorizado")

            def local(valor):
                return None if valor is None else timezone.localtime(valor).isoformat()

            def hora(valor):
                if valor is None or isinstance(valor, dt.time):
                    return valor
                if isinstance(valor, str) and re.fullmatch(
                    r"\d{2}:\d{2}(?::\d{2}(?:\.\d+)?)?", valor
                ):
                    return dt.time.fromisoformat(valor).isoformat()
                return NV

            def batida(b):
                if b is None:
                    return None
                # Campos: app/ponto/models.py:23-26,67.
                # Papeis em memoria: app/ponto/turnos.py:890-909.
                return {
                    "id": b.pk,
                    "hora_local": local(b.timestamp),
                    "tipo": b.tipo,
                    "is_intervalo": b.is_intervalo,
                    "retratada_em": local(b.retratada_em),
                    "papel_ata": b.__dict__.get("_papel_ata", NV),
                    "intra_ata": b.__dict__.get("_intra_ata", NV),
                    "intra_dur": b.__dict__.get("_intra_dur", NV),
                    "intra_declarado": b.__dict__.get("_intra_declarado", NV),
                    "eco_flush": b.__dict__.get("_eco_flush", NV),
                }

            def alertas_sem_nomes(alertas):
                # Publica categorias e numeros, nunca o texto livre.
                # Campo real: app/ponto/motor_calculo_v2.py:253.
                # Exemplos de produtores:
                # :1484-1486,2144-2147,2220-2231,2485-2490,
                # :2561-2563,2754,2779-2782,2814-2828,3188-3190.
                prefixos = (
                    ("DIA SEM JORNADA PREVISTA", "dia_sem_previsto"),
                    ("Entrada ", "horario_deslocado"),
                    ("Atraso:", "atraso"),
                    ("Saída antecipada:", "saida_antecipada"),
                    ("FOLGA TRABALHADA", "folga_trabalhada"),
                    ("Turno aberto", "turno_aberto"),
                    ("Período aberto", "periodo_aberto"),
                    ("Periodo aberto", "periodo_aberto"),
                    ("Intrajornada suprimida", "intra_suprimida"),
                    ("Feriado em", "feriado"),
                    ("HE semanal:", "he_semanal"),
                )
                saida = []
                for texto in alertas or ():
                    if not isinstance(texto, str):
                        raise TypeError("AlertaIncompativel")
                    codigo = next(
                        (cod for inicio, cod in prefixos if texto.startswith(inicio)),
                        "texto_livre_suprimido",
                    )
                    saida.append({
                        "codigo": codigo,
                        "numeros_do_texto": re.findall(
                            r"\d+(?:[.,:/]\d+)*", texto
                        ),
                    })
                return saida

            with ExitStack() as pilha:
                # Tripwire instalado antes dos imports de codigo da aplicacao.
                pilha.enter_context(connection.execute_wrapper(guarda))
                for alias in connections:
                    if alias != "default":
                        pilha.enter_context(connections[alias].execute_wrapper(guarda))

                with transaction.atomic():
                    with connection.cursor() as cursor:
                        cursor.execute("SET TRANSACTION READ ONLY")
                        guarda.permite_set_inicial = False
                        cursor.execute(
                            "SELECT current_database(), current_user, "
                            "current_schema(), "
                            "current_setting('transaction_read_only')"
                        )
                        if cursor.fetchone() != ("sombra", "leitor", "juliani", "on"):
                            raise RuntimeError("BancoPapelOuTenantIncompativel")

                    from ponto.catalogo import ausencias as catalogo
                    from ponto.services import espelho as esp
                    from ponto import turnos as geometria
                    from ponto.management.commands.e6_oraculo import (
                        minutos_do_espelho,
                        minutos_do_oraculo,
                    )
                    from ponto.janelas import janela_fechamento
                    from escala.alimentacao import (
                        q_vinculo_cobre_a_data,
                        vinculo_cobre_a_data,
                    )

                    # Modelos: app/colaboradores/models.py:283;
                    # app/escala/models.py:871,1635;
                    # app/ponto/models.py:115,660.
                    Colaborador = apps.get_model("colaboradores", "Colaborador")
                    CelulaDia = apps.get_model("escala", "CelulaDia")
                    EC = apps.get_model("escala", "EscalaColaborador")
                    Ausencia = apps.get_model("ponto", "Ausencia")
                    Justificativa = apps.get_model("ponto", "Justificativa")

                    # Falha explicitamente se os sitios usados pelo observador
                    # deixarem de corresponder ao codigo desta copia.
                    for funcao, linha in (
                        (esp.espelho_do_colab, 1194),
                        (esp._realizado_do_dia_tela, 267),
                        (geometria.realizado_do_dia, 774),
                        (geometria.realizado_dos_turnos, 707),
                        (minutos_do_oraculo, 42),
                    ):
                        if funcao.__code__.co_firstlineno != linha:
                            raise RuntimeError("SitioDaArvoreIncompativel")

                    # Patch NOMINAL apenas do destino das atribuicoes :279-280.
                    # _CACHE original continua sendo a fonte de todos os reads.
                    # Nao troca vigente(), _do_banco(), catalogo ou turnos.
                    cache = CacheSomenteLeitura(catalogo._CACHE)
                    pilha.enter_context(patch.object(catalogo, "_CACHE", cache))

                    # Os append de :1163,1906,1917 sao listas LOCAIS Python.
                    # Fontes: SELECT agregado e leitor de marcos; nao precisam
                    # GRANT e nao sao removidos:
                    # app/ponto/turnos.py:1154-1164,1901-1918.

                    AGORA = timezone.now()
                    if guarda.tentou:
                        raise escrita_nao_interceptada()

                    def ler_dia(registro):
                        numero, codigo, comp, texto_dia, esp_r1, e6_r1 = registro
                        cid = int(codigo[3:])
                        dia = dt.date.fromisoformat(texto_dia)
                        mes, ano = map(int, comp.split("/"))
                        chave = dia.isoformat()
                        linhas = []
                        inicio_cache = cache.suprimidas

                        def fato(nome, ref, **campos):
                            campos["ref"] = ref
                            linhas.append(
                                "DET|%s|%s|%s|%s" % (
                                    codigo, texto_dia, nome,
                                    json.dumps(
                                        campos, default=serializar,
                                        ensure_ascii=True, allow_nan=False,
                                        separators=(",", ":"),
                                    ),
                                )
                            )

                        c = Colaborador.objects.select_related("empresa").get(pk=cid)
                        # Funcao real: app/ponto/janelas.py:54-57.
                        ini, fim = janela_fechamento(mes, ano, c.empresa)
                        if not ini <= dia <= fim or dia > HOJE:
                            raise RuntimeError("DiaForaDaJanela")

                        captura = {
                            "raizes": [],
                            "corpos": [],
                            "descontos": [],
                            "oraculos": [],
                            "cortes_e6": [],
                            "pares_e6": [],
                        }
                        lendo_e6 = False

                        # Observacao de frames: nao altera argumentos, retornos,
                        # regras, pareamento ou objetos da aplicacao.
                        def observar_raiz(frame, evento, retorno):
                            if evento == "return" and retorno is not None:
                                L = frame.f_locals
                                captura["raizes"].append({
                                    "resultado": retorno,
                                    "ec": L.get("ec"),
                                    "celulas": L["celulas"],
                                    "turnos": tuple(
                                        t for t in L["turnos_juiz"]
                                        if t.data_turno == dia
                                    ),
                                })
                            return observar_raiz

                        def observar_corpo(frame, evento, retorno):
                            L = frame.f_locals
                            # Le pausas/bruto/fim JA decididos pelo corpo real,
                            # imediatamente antes de liquido = bruto - pausas:
                            # app/ponto/turnos.py:756-767.
                            if evento == "line" and frame.f_lineno == 767:
                                captura["descontos"].append({
                                    "modo": "turno",
                                    "entrada": L["t"].entrada.timestamp,
                                    "fim_contado": L["fim"].timestamp,
                                    "bruto_min": L["bruto"],
                                    "pausa_descontada_min": L["pausas"],
                                    "intervalos_reconhecidos": tuple(L["_itv_t"]),
                                })
                            if evento == "return" and retorno is not None:
                                captura["corpos"].append({
                                    "resultado": retorno,
                                    "livre": L["livre"],
                                    "intervalo"

[truncado em 20000 de 54344 caracteres]

_6087 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
