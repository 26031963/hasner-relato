# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 14:57** · publicado em **06/11/2026 14:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 14:57 · sessao c7446096827e · O10-C · verificador · origem=Code

**Laudo: há bloqueios em C.** O comando duplica uma autoridade registrada, deixa SOLO exportado fora da lista `!`, aceita lotes cuja repetição volta a escrever e não oferece reversão fiel das células editadas. Também viola o selo existente “todo command tem casa”. Referências detalhadas abaixo.

A comparação efetiva `320ef1c7..a7d1bfa2` ficou **[não verificado]**: a cópia não contém repositório Git. Execução da suíte, RED histórico, conteúdo dos 16 registros e efeitos em produção: **[não verificado]**.

Para encurtar as referências:

- **C** = [app/ponto/management/commands/trocas_plantao_lote_o10c.py](/home/ronald/codex_ws/a7d1bfa296b5a328978102842ba220e71b205514/app/ponto/management/commands/trocas_plantao_lote_o10c.py)
- **T** = [app/ponto/tests/test_o10c_lote_trocas.py](/home/ronald/codex_ws/a7d1bfa296b5a328978102842ba220e71b205514/app/ponto/tests/test_o10c_lote_trocas.py)
- **P** = [app/ponto/portas/celula.py](/home/ronald/codex_ws/a7d1bfa296b5a328978102842ba220e71b205514/app/ponto/portas/celula.py)
- **A** = `app/ponto/services/ausencia.py`
- **G** = `app/ponto/management/commands/gerar_celulas.py`

**1. Quem escreve e quem lê**

O quadro cobre os caminhos identificados relevantes à obra; exaustividade de todos os leitores e escritores históricos: **[não verificado]**.

| Campo/estado | Quem escreve | Quem lê |
|---|---|---|
| `Ausencia.status` | Criação recebe estado inicial; transição por `mudar_estado`, escritor declarado. `A:333–344,418,808–835`. | C classifica cedente e substituto e imprime estado. `C:66–71,83–84,210`. A porta não consulta status antes das edições. `P:653–675`. |
| `Ausencia.data` | Criação e `editar_ausencia_registro`. `A:342,418,499–509`. | Classificação, lavra, vínculo, snapshot e impressão; a porta escolhe o dia pela carta. `C:72–94,120,209`; `P:675`. |
| `Ausencia.data_fim` | Criação/edição; `Ausencia.save` também pode derivá-la de `dias_corridos`. `A:343,418,500–509`; `app/ponto/models.py:902–907`. | C barra fim diferente do início; cobertura lê o período. `C:72–74,85–87`; `app/ponto/turnos.py:2760–2766`. A troca usa somente `data`. `P:675`. |
| `Ausencia.documento` | Criação e `anexar_documento`. `A:350–351,418,846–859`. | C testa presença e imprime S/N; porta testa presença e inclui o caminho na trilha. `C:75,88,210`; `P:664–667,682–690`. |
| `Ausencia.tipo` | Criação e edição. `A:342,418,498–509`. | C seleciona universo e valida tipo do cedente; modelo consulta efeito no catálogo. `C:64–65,166`; `app/ponto/models.py:840`. |
| `Ausencia.colaborador_id` | Criação recebe `colaborador` e salva. `A:341–342,385,418`. | C compara pessoas, consulta autoridades, fotografa e imprime IDs; porta identifica e ordena os lados. `C:81,90–94,120,209`; `P:662,673–684`. |
| `CelulaDia.trabalha` | `editar_dia`; geração/regeneração no mesmo módulo da porta. `P:259–277,600–609`. | Snapshot/idempotência e juiz do previsto. `C:113–121`; `P:528,594`; `app/escala/models.py:1092–1096`. |
| `CelulaDia.origem` | Porta cria `gerada` ou grava `editada`. `P:259–260,600–609`. | Snapshot, idempotência, regeneração e reversão. `P:528,594,263,777`. |
| `CelulaDia.dna` | Produtor `montar_dna`; persistência pela porta. `G:73–103`; `P:591,600–609`. | Snapshot, idempotência e leitura dos marcos congelados. `P:528,594`; `app/escala/models.py:1122–1124`. |
| `CelulaDia.editada_em` | `editar_dia`. `P:598–609`. | Snapshot, carimbos de reescrita, frescor de veredito/fase. `P:530`; `app/escala/services/regeneracao.py:98–105`; `app/chamados/veredito_lavrado.py:151–154`; `app/escala/services/fase_12x36.py:188–192`. |
| `LogAuditoria`, ação `editar_dia` | Porta fornece antes/depois ao escritor `core.observ.evento`. `P:611–620`; `app/core/observ.py:134–148`. | Consulta genérica da auditoria; C não consulta a trilha. `app/core/views.py:705–710`; `C:124–143,155–228`. |
| `LogAuditoria`, ação `trocar_plantao` | Porta emite evento após as edições, com identificação das cartas. `P:682–693`; `app/core/observ.py:134–148`. | Consulta genérica da auditoria; C recebe apenas resultado/frases. `app/core/views.py:705–710`; `C:140–143`. |
| `ExportacaoDominio` | `registrar`, chamado pela exportação HTTP e pelo regerador. `app/folha/models.py:174–188`; `app/folha/views.py:297`; `app/folha/management/commands/regerar_txt_dominio.py:118`. | G enumera competências e pergunta ao juiz; C e P chamam G. `G:161–174`; `C:55–58`; `P:567–570`. |
| `Holerite` / publicação | Motor cria holerite; processador publica o lote. `app/holerite/motor.py:480–483`; `app/holerite/management/commands/processar_lote_holerite.py:123–146`. | G enumera holerites com lote publicado; juiz confirma pessoa/competência. `G:163–174`; `app/folha/export.py:612–620`. |

**Não identifiquei escrita desses campos pelo comando fora da porta.** C chama `trocar_plantao`; sua escrita própria é o arquivo JSONL. A porta tampouco altera as cartas nessa operação. `C:124–143,183,192–195`; `P:653–693`.

Isso atende à centralização da escrita, mas não resolve a duplicação de decisão descrita a seguir. O padrão declarado inclui escritor único e autoridade única por pergunta. `app/docs/GLOSSARIO.md:11–12,23`; `app/docs/PLANO-PADROES.md:46–53`.

**2. `classificar`: testemunha ou segundo juiz?**

**Há duplicação concreta:** `ABERTOS` copia literalmente `ABERTAS` e decide se a ausência está aberta. Essa pergunta já tem autoridade registrada. `C:50,68–69`; `A:765`; `app/core/juizes.py:244`.

O anexo confirma a autoridade e também estabelece que o registro vivo prevalece sobre o lookup. `app/docs/JUIZES.md:7–15,50–52`.

Para lavra e vínculo, C usa as mesmas funções que a porta:

- `_empresa_do_colab` + `_competencias_exportadas`; este último consulta `competencia_fechada_para_celula`, juiz registrado. `C:53–58`; `P:567–570`; `G:173`; `app/core/juizes.py:315–322`.
- `vinculo_do_dia_carregado`, carregador que encaminha ao juiz de vínculo. `C:63,93–96`; `P:556,584`; `app/escala/alimentacao.py:289–317`; `app/docs/JUIZES.md:101–105`.

**A declaração de equivalência com a porta é falsa para outros filtros.** Supondo cartas existentes, pessoas distintas, documentos, vínculo e competência aberta:

| Entrada | DRY | Porta |
|---|---|---|
| Cedente `aguardando_decisao` | `pedido`. `C:68–71`. | Aceita prosseguir: não verifica status. `P:653–675`. |
| Cedente rejeitado | `rejeitada`. `C:66–67`. | Também não verifica esse estado. `P:653–675`. |
| Cedente de outro tipo | `nao_e_troca`. `C:64–65`. | Não verifica tipo. `P:653–675`. |
| Qualquer carta com período de vários dias | `periodo`. `C:72–74,85–87`. | Edita somente `a.data`, sem verificar fim. `P:675`. |
| Substituto rejeitado | `carta_rejeitada`. `C:83–84`. | Não verifica status. `P:653–675`. |

Essas restrições podem representar critérios próprios do lote; apresentá-las como mera leitura do que a porta decidiria contraria a implementação e a regra “testemunha lê, não recalcula”. `C:17–24`; `CLAUDE.md:20–22`.

No sentido contrário, não identifiquei predicado estável omitido que, com autor válido e insumos inalterados, transforme `trocaria` em `recusada`: os filtros de documentos, pessoas, lavra e vínculo estão presentes. Contudo, vínculo/lavra podem mudar entre as consultas, produzindo recusa posterior. `C:75–96,134–142`; `P:664–677,568–590`. Além disso, `trocaria` pode resultar em `igual`, pois C não compara o estado final das células. `C:97`; `P:594–596,679–680`.

**Falha no aval “09 exportada = !”:** SOLO retorna antes da classificação de lavra; a coluna imprime `lavrada=S`, mas somente `classe == 'lavrada'` entra na lista `!` e no contador `bang`. Isso também afeta linhas rejeitadas, pedidos e outras classes anteriores à lavra. `C:66–92,209–225`. Quantos dos 16 registros cairiam nisso: **[não verificado]**.

**3. Atomicidade e concorrência de `aplicar_par`**

- **Ordem explícita correta entre os lados:** C trava as Ausências por PK; P repete essa ordem e edita os lados por `(colaborador_id, data)`. Não encontrei inversão explícita dessas ordens no caminho inspecionado. `C:128–130`; `P:657–658,670–675`.
- **A reclassificação protege as cartas:** ela ocorre depois das travas. Alteração anterior de estado/documento/período pode converter o par em `pulada`. `C:129–136`.
- **A foto não protege as células:** `_celula` usa `.first()` sem trava. Outro `editar_dia` pode modificar a célula entre a foto e a aquisição da trava pela porta; o JSONL então não será o “antes real” da edição. `C:113–138`; `P:592–597`.
- **Permanece TOCTOU de autoridades:** lavra, empresa, vínculo e DNA são consultados antes da trava da célula; não há trava correspondente sobre publicação/exportação ou cadastro nesse caminho. `C:90–96`; `P:567–593`. O risco da guarda anterior ao atomic já está registrado como pendência de B. `app/docs/BACKLOG.md:1790`.
- **Célula inexistente:** a consulta não reserva uma linha ausente; há criação posterior e restrição única por colaborador/data. Concorrência pode terminar em erro de integridade, em vez de serialização prévia. `P:593,599–601`; `app/escala/models.py:1833`. Ocorrência concreta: **[não verificado]**.
- **Rollback do par:** `_TrocaRecusada` desfaz o atomic interno da troca e retorna `recusada`; exceção propagada também atravessa o atomic externo de C. Falha da trilha crítica levanta exceção. O JSONL já escrito permanece. `P:619–620,652–699`; `C:128–143,193–195`.
- **Atomicidade é por par:** pares anteriores já podem ter sido confirmados quando um par posterior falha. `C:128,199–205`.

O rabo de `editar_dia` ocorre depois de seu atomic local, **ainda dentro dos atomics da troca e de C**. O julgamento inline participa dessa transação, embora suas falhas sejam capturadas. O dinheiro e a reavaliação são registrados em `on_commit`: aguardam o commit externo e seus callbacks são descartados se a transação correspondente voltar. `P:627–628,422–462,490–502`; `app/ponto/services/fechamento.py:1782–1808`.

Depois do commit, falha de recálculo/reavaliação não desfaz a troca já confirmada; os caminhos tratam falhas como best effort. Portanto “par confirmado” não prova “todas as projeções atualizadas com sucesso”. `app/ponto/services/fechamento.py:1791–1805`; `P:493–502`; `app/core/observ.py:85–91`.

**4. Idempotência e reversão**

**Um par isolado, sem mudanças externas:** a porta devolve `igual`, sem alterar células, trilha ou executar o rabo novamente. **O comando ainda cria outro snapshot e grava outra linha antes dessa comparação.** A promessa “segunda rodada não escreve nada” não vale para arquivo. `P:594–596,627–628,679–680`; `C:137–142,176–195`; `T:12,215–228`.

**O lote inteiro também não é necessariamente idempotente.** Contraexemplo estático:

1. A e B são cartas aprovadas, documentadas, de pessoas distintas, no mesmo dia aberto.
2. Declarar `--par A:B --par B:A` é aceito: o parser só proíbe repetir o mesmo cedente.
3. O primeiro par deixa A em folga/B trabalhando; o segundo deixa B em folga/A trabalhando.
4. A rodada seguinte repete as alterações e cria novas trilhas.

Não há detecção desse conflito entre pares. `C:100–110,199–205`; `P:673–691`. Execução desse contraexemplo contra banco: **[não verificado]**.

**Snapshot: pré-requisito temporal parcialmente atendido.** O arquivo nasce em `logs/o10c`, e cada foto recebe `flush/fsync` antes da chamada da porta. `C:137–142,176–195`; exigência em `CLAUDE.md:398–400`.

**Não considero demonstrada sua suficiência como reversão operacional**, porque:

- A foto pode estar obsoleta por concorrência. `C:113–138`; `P:593–597`.
- Guarda seis campos da célula, mas não os estados das projeções/chamados que o ato também pode modificar; tampouco registra resultado confirmado ou tenant no payload. `P:528–531,625–628,422–502`; `C:137–143`.
- A reversão existente exige `origem='gerada'`; a troca grava `editada`. `P:600–609,776–778`.
- Chamar `editar_dia` com o booleano anterior remonta DNA atual, força origem `editada` e renova autor/hora; não restaura fielmente a foto anterior. `P:591,598–609`.

Existe caminho para **editar novamente** uma célula editada, inclusive no sentido contrário. Não identifiquei caminho para **restaurar exatamente** seu estado anterior pelo snapshot. `P:594–609`; `C:149–153`.

Hash antes/depois da exportada, prova de frota e aplicação real dos demais requisitos de DINHEIRO: **[não verificado]**; são exigidos em `CLAUDE.md:398–400`.

**5. Os testes mordem?** [nao verificado]

Mordem partes relevantes: resultado real dos dois lados/trilha, chamada com cartas e autor e comportamento distinto para par aberto/exportado. Não são todos vazios. `T:132–143,275–287,301–314`.

A tabela abaixo indica mutantes que **o teste individual** não demonstraria detectar; sobrevivência por execução de mutação: **[não verificado]**. [nao verificado]

| Teste, identificado pela linha em T | Mutante plausível não coberto | [nao verificado]
|---|---|
| `dry_classifica…`, `T:88–107` | Criar arquivo também no DRY: não verifica filesystem. |
| `dry_com_par_completo…`, `T:109–117` | Remover guarda de lavra: fixture usa dia aberto. |
| `solo_em_competencia_lavrada…bang`, `T:119–127` | Remover emissão de `!`: só exige `so_um_lado` e `lavrada=S`. |
| `aprovado_com_par…`, `T:132–143` | Remover snapshot: não lê o arquivo. |
| `snapshot…antes_da_porta`, `T:145–167` | Remover `fsync`, ou omitir DNA/autor/hora/geradora da foto: asserções só cobrem par, trabalha e origem. |
| `rejeitado_e_aguardando…`, `T:169–177` | Aceitar substituto rejeitado: cedentes já saem antes dessa verificação. |
| `sem_a_carta…`, `T:179–189` | Remover reclassificação sob trava: ambos já são barrados na primeira classificação. |
| `dia_em_competencia_exportada…`, `T:191–204` | Consultar lavra só do cedente: ambos estão na mesma competência exportada. |
| `lado_do_substituto…bang`, `T:206–213` | Suprimir linha `!` para lavra apenas do substituto: só verifica classe. |
| `segunda_rodada…`, `T:215–228` | Aceitar pares invertidos conflitantes ou escrever novo snapshot: testa só um par e não filesystem. |
| `nenhum_colaborador…desconto`, `T:230–249` | Remover `fsync`: não inspeciona arquivo; tampouco cobre descontos fora dos três colaboradores/período fotografados. |
| `aplicar_exige_autor`, `T:251–253` | Aceitar username inexistente quando fornecido: só testa ausência de `--por`. |
| `par_repetido_ou_mal_escrito…`, `T:255–261` | Aceitar `1:2:3`: só cobre duplicata e `abc`. |
| `cedente_nao_e_troca…`, `T:263–270` | Remover tratamento de cedente inexistente: fixture contém registro existente. |
| `a_costura_MORDE`, `T:275–287` | Remover trava das Ausências ou atomic externo: porta está mockada, sem concorrência/rollback. |
| `comando_so_escreve_pela_porta`, `T:289–299` | Acrescentar SQL por `cursor.execute`: blacklist não inclui esse caminho. |
| `dry_responde_o_que_a_porta_faria`, `T:301–314` | Remover guarda de período: só compara par normal e exportado, sem período. |

**Há selo sem mordida no requisito específico SOLO/`!`:** o próprio teste espera uma classe que não alimenta `bang`. `T:126–127`; `C:214–225`. [nao verificado]

**A semântica real de `on_commit` não é testada:** `_run` substitui o callback por execução imediata, mascarando adiamento e descarte no rollback. `T:59–63`; `app/ponto/tests/test_o10b_editar_dia_troca.py:39–41`.

A regra dos casos está escrita no docstring; a anterioridade ao código e o RED evidenciado não podem ser confirmados nesta cópia. `T:5–14`; **[não verificado]** para histórico/execução. A exigência está em `CLAUDE.md:28–29,45–51`.

**6. Nome ou CPF**

Na saída normal não há leitura de `colaborador.nome` ou `cpf`: imprime IDs, datas, status e presença de carta. O snapshot usa IDs e o estado da célula; o produtor atual de DNA gera dados de escala/marcos. `C:119–121,137–138,207–212`; `P:528–531`; `G:73–103,119–122`.

**Isso não prova ausência absoluta de dado pessoal:** o DNA anterior é copiado integralmente, sem sanitização; erros reproduzem `--par` inválido e username fornecido. Conteúdo real desses valores: **[não verificado]**. `P:528`; `C:105,165`.

A trilha da porta também conserva `documento.name`; eventual nome/CPF embutido no caminho não é raspado. Isso é trilha, não o snapshot de C. `P:682–690`. Conteúdo dos caminhos reais: **[não verificado]**.

**7. Selos existentes**

| Selo | Resultado da leitura |
|---|---|
| **Todo command tem casa** | **Viola.** O varredor inclui o arquivo novo, mas ele não está nas listas aceitas. `app/chamados/tests/test_contract_crons.py:214–228`; `app/config/crons.py:281,769,1484,1755–1766`. Execução da suíte: **[não verificado]**. |
| **Chokepoint da célula** | Não identifiquei escrita externa no comando; o gate exige allowlist vazia. `C:124–143`; `app/ponto/tests/test_chokepoint_celula_gate.py:20–36,50–54`. |
| **Except-pass** | Não viola esse padrão: o handler usa `continue`, enquanto o selo acusa corpo `ast.Pass`. `C:180–188`; `app/ponto/tests/test_contract_no_except_pass.py:65–78`. |
| **Relógio/instante** | Há relógio em `aplicar_par` e `handle`, para carimbo/nome de arquivo. Não identifiquei violação do selo de grade: essas funções não chamam seus alvos. `C:137,179`; `app/escala/tests/test_selo_grade_pede_o_instante.py:24–38,93–119`. |
| **MAPA** | O comando não pertence ao universo obrigatório, derivado dos registros de juízes e portas. Comparação completa do diagrama regenerado: **[não verificado]**. `app/core/mapa/contrato.py:157–177`. |
| **Juiz de ausência** | Há cópia de autoridade, mas os padrões existentes não cobrem essa tupla de status; esse selo não basta para detectá-la. `C:50,68`; `app/core/juizes.py:244,1406–1420`. |

**8. Classes de reprovação presentes**

- **Segundo juiz / derivação paralela: sim**, pela cópia `ABERTOS` e pela classificação apresentada como equivalente à porta, embora tenha critérios diferentes. `C:17–24,50,64–89`; `app/core/juizes.py:244`; `CLAUDE.md:20–22`.
- **Segundo escritor fora da porta: não identificado** para os estados auditados. `C:124–143`; `P:600–618,685–693`.
- **Relógio fora da borda de negócio: não demonstrado.** As leituras novas servem ao arquivo e ficam no módulo de comando; não julgam dia ou dinheiro. `C:137,179`; escopo do corte em `app/docs/CORTES.md:370`.
- **Selo/RED sem mordida: sim, parcialmente**, no SOLO/`!`; também faltam provas da concorrência e do `on_commit` real. `T:119–127,61,275–314`.
- **Ordem de trava invertida: não identificada no par.** Permanecem foto sem trava e TOCTOU da lavra/vínculo/DNA. `C:113–138`; `P:567–597,657–675`.
- **Pouso antes do pré-requisito B: não demonstrado.** A cópia contém a porta, e o RELATO registra B no ar; o diff e a ancestralidade efetiva do commit são **[não verificado]**. `P:534,632`; `app/docs/RELATO.md:3,40–41`.
- **Pré-requisito de reversão antes do apply: insuficiente operacionalmente**, apesar do arquivo anterior à chamada: a foto pode ser obsoleta e o reversor existente recusa a origem produzida. `C:113–142,193–195`; `P:600–609,776–778`; `CLAUDE.md:398–400`.

## 06/11 14:53 · sessao 0cec89bc15c9 · - · - · origem=Ronald (tmux)

Mapa do caminho exercido pelo lote; inventário exaustivo de todos os leitores históricos do repositório: [não verificado].

| Estado/campo | Quem escreve no caminho de produção | Quem lê |
|---|---|---|
| `Ausencia.colaborador_id` | A criação recebe `colaborador`, monta `kwargs` e salva a instância pela porta `criar_ausencia` (`app/ponto/services/ausencia.py:130`, `:341`, `:385`, `:418`). | Lote compara os colaboradores, consulta lavra/vínculo, fotografa e imprime IDs (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:81`, `:90`, `:94`, `:120`, `:209`); porta usa os dois IDs (`app/ponto/portas/celula.py:662`, `:673`, `:683`). |
| `Ausencia.tipo` | Criação em `kwargs` e `save`; edição por `editar_ausencia_registro` (`app/ponto/services/ausencia.py:342`, `:418`, `:465`, `:498`, `:509`). | Seleção do universo e classificação do cedente (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:64`, `:166`); efeito por catálogo no modelo (`app/ponto/models.py:840`). A porta `trocar_plantao` não o consulta (`app/ponto/portas/celula.py:653-675`). |
| `Ausencia.data` | Criação e edição (`app/ponto/services/ausencia.py:342`, `:418`, `:499`, `:509`). | Classificação de período, lavra/vínculo, snapshot e impressão (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:72`, `:85`, `:90`, `:94`, `:120`, `:209`); porta escolhe o dia editado de cada carta (`app/ponto/portas/celula.py:675`). |
| `Ausencia.data_fim` | Criação e edição (`app/ponto/services/ausencia.py:343`, `:418`, `:500`, `:509`); `Ausencia.save` também deriva fim a partir de `dias_corridos` (`app/ponto/models.py:902-907`); `mudar_estado` inclui `data_fim` no `update_fields` para persistir derivações do modelo (`app/ponto/services/ausencia.py:834-835`). | Lote recusa `data_fim != data`, dos dois lados (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:72`, `:85`); cobertura usa o fim ou fallback da autoridade (`app/ponto/turnos.py:2760-2766`). A porta troca somente `a.data` (`app/ponto/portas/celula.py:675`). |
| `Ausencia.status` | Criação recebe estado ou `estado_inicial` (`app/ponto/services/ausencia.py:333`, `:344`, `:418`); transição pelo escritor declarado `mudar_estado` (`app/ponto/services/ausencia.py:808-835`). | Lote verifica cedente aprovada/rejeitada/aberta e substituto rejeitada, imprimindo status (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:66-71`, `:83`, `:210`). A porta não verifica status (`app/ponto/portas/celula.py:653-675`). |
| `Ausencia.documento` | Criação recebe anexo (`app/ponto/services/ausencia.py:350-351`, `:418`); atualização pela porta `anexar_documento` (`app/ponto/services/ausencia.py:846`, `:857-859`). | Lote testa presença de ambos, imprime apenas S/N (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:75`, `:88`, `:210`); porta exige presença e grava nome do arquivo na trilha do par (`app/ponto/portas/celula.py:664-667`, `:684`). |
| `CelulaDia.trabalha` | `editar_dia` cria ou atualiza (`app/ponto/portas/celula.py:600-609`); geração/regeneração pelo mesmo módulo (`app/ponto/portas/celula.py:259`, `:272-277`). | Lote fotografa pelo helper (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:113-121` → `app/ponto/portas/celula.py:528`); porta compara para idempotência (`app/ponto/portas/celula.py:594`); juiz previsto lê `cel.trabalha` (`app/escala/models.py:1069`, `:1092-1096`). |
| `CelulaDia.origem` | Porta cria como gerada ou editada, e edição força editada (`app/ponto/portas/celula.py:259-260`, `:600`, `:606`, `:609`). | Snapshot (`app/ponto/portas/celula.py:528`), idempotência (`:594`), regeneração limita a gerada (`:263`) e reversão de regeneração recusa editada (`:777`). |
| `CelulaDia.dna` | Produtor `montar_dna` é chamado pela edição; persistência fica na porta (`app/ponto/portas/celula.py:591`, `:600-609`); regeneração escreve pelo mesmo módulo (`:256`, `:272-277`). | Snapshot e comparação (`app/ponto/portas/celula.py:528`, `:594`); marcos do dia leem DNA congelado (`app/escala/models.py:1098`, `:1122-1124`). |
| `CelulaDia.editada_em` | `editar_dia` usa `timezone.now()` e persiste (`app/ponto/portas/celula.py:598-609`). | Snapshot (`app/ponto/portas/celula.py:530`); carimbos de reescrita (`app/escala/services/regeneracao.py:98-105`); fatos novos para veredito e fase (`app/chamados/veredito_lavrado.py:151-154`, `app/escala/services/fase_12x36.py:188-192`). |
| `LogAuditoria.acao='editar_dia'` | Porta emite `evento_kw` com antes/depois (`app/ponto/portas/celula.py:611-618`); adaptador vai ao escritor `evento`, que faz INSERT (`app/core/observ.py:134-148`). | Tela genérica consulta e permite filtrar ação (`app/core/views.py:705-710`); lote não consulta a trilha (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:124-143`, `:155-228`). |
| `LogAuditoria.acao='trocar_plantao'` | Porta emite após os lados; inclui IDs, datas, documento e resultado (`app/ponto/portas/celula.py:682-693`), pelo mesmo escritor (`app/core/observ.py:134-148`). | Mesma tela genérica (`app/core/views.py:705-710`); o lote somente recebe retorno/frases da porta (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:140-143`). |
| `ExportacaoDominio` | `registrar` cria registro (`app/folha/models.py:174-188`), chamado pela exportação HTTP (`app/folha/views.py:297`) e pelo regerador (`app/folha/management/commands/regerar_txt_dominio.py:118`). | `_competencias_exportadas` enumera mês/ano e pergunta ao juiz (`app/ponto/management/commands/gerar_celulas.py:161-174`); lote e porta chamam esse carregador (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:55-58`, `app/ponto/portas/celula.py:567-570`). |
| `Holerite` / publicação de lote | Motor cria documento associado ao lote (`app/holerite/motor.py:480-483`); job de publicação grava `LoteHolerite.publicado_em` (`app/holerite/management/commands/processar_lote_holerite.py:126-130`, `:146`); uploads também criam holerite (`app/holerite/views.py:1193-1203`, `:1259-1265`). | `_competencias_exportadas` enumera somente holerites com lote publicado (`app/ponto/management/commands/gerar_celulas.py:163-174`); o juiz faz o predicado final por pessoa e competência (`app/folha/export.py:612-620`). |

O lote não contém chamada direta de escrita ORM desses campos: o caminho de mutação é `celula.trocar_plantao(...)`; grava seu arquivo por `open/write/flush/fsync` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:140-143`, `:183`, `:192-195`). O selo novo verifica nomes de chamadas, não prova sozinho a ausência de qualquer escrita indireta (`app/ponto/tests/test_o10c_lote_trocas.py:289-299`).

O anexo é lookup; registro vivo manda (`app/docs/JUIZES.md:7-15`). Para “ausência aberta ou decidida”, ele aponta `services/ausencia.py::ABERTAS` (`app/docs/JUIZES.md:50-52`), igualmente registrado em `app/core/juizes.py:244`. O lote cria cópia literal `ABERTOS` e a consulta, portanto duplica essa autoridade mesmo com valores atualmente iguais (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:50`, `:68`; autoridade atual em `app/ponto/services/ausencia.py:765`).

O lote consulta corretamente a autoridade existente de lavra por meio de `_competencias_exportadas`, que chama `competencia_fechada_para_celula`; esse juiz está registrado como `_K9` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:55-58`, `app/ponto/management/commands/gerar_celulas.py:173`, `app/core/juizes.py:315-322`). Também usa o mesmo carregador de vínculo da porta (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:63`, `:93-96`; `app/ponto/portas/celula.py:556`, `:584`).

A declaração “pergunta a mesma função que a porta pergunta” não vale para todos os filtros: o lote verifica tipo, estado e período; a porta verifica existência, colaboradores diferentes, documentos, lavra e vínculo, sem ler tipo/status/fim (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:64-96`; `app/ponto/portas/celula.py:653-675`). Contraprovas estáticas, supondo demais condições válidas: cedente com carta e `aguardando_decisao` dá `pedido` no DRY, mas a porta aceita; cedente tipo diferente dá `nao_e_troca`, mas a porta aceita; qualquer carta com fim diferente da data dá `periodo`, mas a porta edita somente sua data; substituto rejeitado dá `carta_rejeitada`, mas a porta aceita (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:64-89`; `app/ponto/portas/celula.py:664-675`). Essas são divergências de contrato; sua ocorrência nos 16 registros reais é [não verificado].

Não há reversor fiel de edição identificado no caminho inspecionado: `reverter_regeneracao_dia` exige origem gerada (`app/ponto/portas/celula.py:776-778`); chamar `editar_dia` com booleano anterior remonta DNA atual e força origem editada, sem restaurar timestamp/origem/DNA antigos (`app/ponto/portas/celula.py:591`, `:598-609`).

## 06/11 14:53 · sessao 1597aea2e856 · - · - · origem=Ronald (tmux)

Achado concreto: o comando viola o selo **B6.1 — todo command tem casa**. O teste varre `*/management/commands/*.py` e exige presença em `todas_as_entradas()` ou `FORA_DE_PIPELINE`; o novo comando não aparece em nenhum dos dois, confirmado por consulta Python somente leitura aos dados de configuração. Portanto `test_todo_command_tem_casa` falharia (`app/chamados/tests/test_contract_crons.py:214–228`; fontes: `app/config/crons.py:281`, `:769`, `:1484`, `:1755–1766`).

Outras verificações:

- **Chokepoint:** o censo oficial aplicado somente ao novo fonte retornou `[]`; o comando só chama a porta em `:140`, sem verbo ORM de escrita. O gate da célula varre produção e tem allowlist vazia (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:128–143`; `app/ponto/tests/test_chokepoint_celula_gate.py:20–36`, `:50–54`; `app/core/censo_escritas.py:451–466`).
- **Except-pass:** nenhum caso no AST. O único handler trata colisão do nome do snapshot com `continue`, enquanto o selo acusa exclusivamente handler cujo corpo é um `ast.Pass` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:180–188`; `app/ponto/tests/test_contract_no_except_pass.py:65–78`).
- **Relógio:** há leitura em `aplicar_par`, fora de `handle`, e outra em `handle`; a primeira carimba o snapshot, sem escolher comportamento por data (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:137`, `:179`). O selo de instante examina somente funções que chamam seus `ALVOS`; `aplicar_par` não chama esses alvos (`app/escala/tests/test_selo_grade_pede_o_instante.py:25–38`, `:93–119`). Não constatei violação mecânica desse selo; conformidade com eventual lei mais ampla sobre borda do relógio: [não verificado].
- **Relógio dos testes:** o selo procura leituras diretas no próprio arquivo; o novo teste não as contém. A fixture importada tem `@freeze_time(HOJE)` (`app/core/tests/test_selo_teste_sem_relogio.py:32–45`; `app/ponto/tests/test_o10c_lote_trocas.py:33–34`; `app/ponto/tests/test_o10b_editar_dia_troca.py:68–69`).
- **MAPA:** o comando não pertence ao universo obrigatório de inscrição, que deriva exclusivamente de `core/juizes.py` e `core/portas.py`; consulta somente leitura confirmou isso (`app/core/mapa/contrato.py:157–184`; `app/core/tests/test_selo_mapa_contrato.py:41–49`). O MAPA commitado deliberadamente exclui contagens de leitores novos; elas vão à saída do gerador (`app/core/management/commands/gerar_diagrama.py:861–865`, `:936–939`; `app/core/tests/test_selo_diagrama_do_codigo.py:328–333`). Comparação completa do MAPA regenerado: [não verificado].
- **Cópia de juiz que passa pelos selos:** `ABERTOS` replica a pergunta registrada em `ABERTAS`; o comando declara a tupla em `:50` e a consulta em `:68`. A autoridade registrada é `ponto/services/ausencia.py::ABERTAS` (`app/core/juizes.py:244`). O selo não acusa essa cópia porque os padrões da família não incluem status aberto; análise de todos os padrões proibidos sobre o novo fonte retornou zero ocorrências (`app/core/juizes.py:1406–1420`; varredor `app/ponto/tests/test_contract_juiz_celula.py:48–60`).

## 06/11 14:53 · sessao 1e8ffbad88c7 · - · - · origem=Ronald (tmux)

Convenção: `T` = `app/ponto/tests/test_o10c_lote_trocas.py`; `C` = `app/ponto/management/commands/trocas_plantao_lote_o10c.py`; `P` = `app/ponto/portas/celula.py`. Os sobreviventes abaixo são previsão por leitura, sem execução de mutação: [não verificado].

| Teste novo | Mutante plausível que esse teste não pegaria | [nao verificado]
|---|---|
| `test_dry_classifica_cada_lancamento_e_nao_escreve` | Criar arquivo de snapshot também no DRY: verifica porta, origem editada, contagem da trilha e saída, mas não filesystem. `T:88-107`; abertura atual condicionada em `C:176-195`. |
| `test_dry_com_par_completo_diz_trocaria_e_nao_chama_a_porta` | Retirar a guarda de competência do classificador: fixture só usa novembro aberto. `T:109-117`; guarda `C:90-92`. |
| `test_solo_em_competencia_lavrada_diz_que_e_bang` | Apagar inteiramente a emissão da lista `!`: teste só exige `so_um_lado` e `lavrada=S`. `T:119-127`; emissão `C:214-221`. | [nao verificado]
| `test_aprovado_com_par_e_duas_cartas_vira_par` | Remover toda a gravação do snapshot: verifica células e trilha, sem ler arquivo. `T:132-143`; snapshot `C:137-138`. |
| `test_snapshot_das_duas_celulas_nasce_antes_da_porta` | Remover `os.fsync`: o teste abre arquivo no mesmo processo e verifica JSON visível, sem durabilidade. `T:153-167`; `C:193-195`. Também sobreviveria snapshot sem `dna`, `editada_em`, `editada_por_id` e geradora, porque só confere par, trabalha e origem cedente. `T:164-167`; formato `P:528-531`. | [nao verificado]
| `test_rejeitado_e_aguardando_nao_aplicam_mesmo_com_par` | Permitir substituto com status rejeitada: ambos os cedentes já são rejeitado/pedido, saindo antes de examinar o substituto. `T:169-177`; ordem `C:66-71`, guarda substituto `C:83-84`. |
| `test_sem_a_carta_de_um_lado_recusa_sem_escrever` | Retirar a segunda classificação sob trava: os dois exemplos já são excluídos pela primeira. `T:179-189`; seleção apply `C:202-205`; reclassificação `C:134-136`. |
| `test_dia_em_competencia_exportada_vai_a_lista_bang_e_nao_toca` | Consultar lavra apenas do cedente: os dois lados estão no mesmo dia/empresa exportada, suficiente para passar. `T:193-204`; consulta atual dos dois `C:90`. |
| `test_lado_do_substituto_em_lavrada_tambem_e_bang` | Não adicionar `!` quando somente substituto está exportado: teste só confere classe, sem linha `!`, contador, apply ou ausência de chamada da porta. `T:206-213`; `C:214-221`. | [nao verificado]
| `test_segunda_rodada_e_idempotente` | Criar novo snapshot e acrescentar linha na segunda rodada: já é o comportamento atual e passa pelas asserções, limitadas a quatro campos das células e IDs de LogAuditoria. `T:215-228`; `C:176-195`, `C:137-143`; porta retorna igual em `P:594-596`, `P:679-680`. |
| `test_nenhum_colaborador_ganha_desconto_novo` | Injetar novo desconto em colaborador fora dos três ou dia fora de 21/10–09/11: foto só cobre esse filtro. `T:231-239`, `T:246-249`. Alternativamente retirar fsync sobrevive porque o teste não inspeciona snapshots. `C:195`; `T:230-249`. | [nao verificado]
| `test_aplicar_exige_autor` | Aceitar username fornecido mas inexistente: teste só omite `--por`; não passa usuário desconhecido. `T:251-253`; ramo não coberto `C:163-165`. | [nao verificado]
| `test_par_repetido_ou_mal_escrito_e_erro` | Rejeitar qualquer repetição, mas aceitar valores negativos ou `1:2:3`: teste cobre duplicata exata e `abc`, sem esses formatos. `T:255-261`; parser `C:103-109`. | [nao verificado]
| `test_par_cujo_cedente_nao_e_troca_de_plantao_nao_aplica` | Remover tratamento de cedente inexistente: fixture é declaração existente. `T:263-270`; ramo `C:171-173`. |
| `test_a_costura_MORDE` | Remover `select_for_update` das Ausencias ou `atomic` externo: teste confere chamada e argumentos da porta mockada, sem concorrência/rollback. `T:275-287`; `C:128-130`. | [nao verificado]
| `test_o_comando_so_escreve_pela_porta` | Adicionar escrita por SQL cru, por exemplo `cursor.execute(...)`: blacklist olha nomes das chamadas, não SQL nem atribuições indiretas. `T:294-299`; ela proíbe save/create/update/etc., mas não `execute`. |
| `test_o_dry_responde_o_que_a_porta_faria` | Retirar reclassificação sob trava ou detecção de período: só testa um par normal e um par exportado, ambos sem período. `T:308-314`; `C:72-74`, `C:85-87`, `C:134-136`. |

O caso SOLO tem falha concreta, além da hipótese de mutação: retorna `so_um_lado` antes da avaliação de lavra (`C:77-80`, `C:90-92`), e só classe `lavrada` entra em `bang` (`C:214-221`). Logo `test_solo_em_competencia_lavrada_diz_que_e_bang` não prova o “bang” do próprio nome: explicitamente espera `so_um_lado` e só procura `lavrada=S` (`T:126-127`). A coluna mostra exportação, mas não há `!` nem incremento de bang nessa linha (`C:211`, `C:214-225`).

A costura não é inteiramente vazia: o teste exige uma chamada com as duas cartas/autor e exige células imóveis sob dublê (`T:279-287`); o teste de resultado exige a alteração efetiva dos dois lados e trilha com IDs (`T:136-143`). O par “trocaria/lavrada” também exige resultados distintos da porta (`T:311-314`). A declaração geral “DRY responde o que a porta faria” excede os dois exemplos cobertos (`T:301-314`): cedente rejeitado/pedido, tipo diferente e período têm guardas próprias em `C:64-74`, mas a porta só verifica existência, colaborador e documentos antes das edições (`P:653-675`). [nao verificado]

Nenhum desses testes exercita semântica real de `on_commit`: `_run` sempre substitui por execução imediata (`T:61`), e `_agora_mesmo` chama `fn()` diretamente (`app/ponto/tests/test_o10b_editar_dia_troca.py:39-41`). Assim, não demonstram descarte dos callbacks no rollback nem adiamento até o commit externo; também não há concorrência concorrente nos corpos dos 17 testes (`T:88-314`).

O snapshot é anterior à chamada e dá flush/fsync (`C:137-142`, `C:193-195`), mas células são fotografadas sem trava (`C:113-116`), ao passo que a porta só trava a célula depois de montar DNA (`P:591-593`): há janela para “antes” obsoleto perante outro escritor. A reversão existente rejeita origem diferente de `gerada` (`P:776-778`), enquanto a troca grava `editada` (`P:600-609`). Portanto essa operação de reversão não desfaz as células editadas pela troca; o comando também só oferece par/aplicar/por (`C:149-153`). Suficiência operacional do arquivo para reversão integral não está demonstrada pelos testes, que conferem apenas trabalha/origem (`T:164-167`). [nao verificado]

A regra de casos aparece no docstring (`T:5-14`), mas a afirmação “escrita antes do código” e uma execução RED histórica não podem ser confirmadas pela cópia sem Git/log fornecido: [não verificado]. A regra anti-vacuidade exige caso que morde para selo com mock ou saída (`CLAUDE.md:191`); os casos citados mordem parte do contrato, com a exceção evidente do SOLO/bang (`T:119-127`).

## 06/11 14:49 · sessao dda1e5a29e82 · V3-IMPACTO-COM-DONO · sonda · origem=Code

Todas as linhas `SOMA` da parte fornecida apresentam `True` em seus indicadores. [nao verificado]

Não há registro `INCOMPLETO` no texto fornecido. Há `COBERTURA_INTEGRAL=False` nos dois meses e `EXCLUIDOS_TOTAL|31`. [nao verificado]

Transcrição integral do dado fornecido, sem verificação independente em arquivo: [nao verificado]

```text
sonda_leitura: script.py [8c14bf8ab6205915] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|5|6
FINALIDADE|IMPACTO_NUNCA_VALIDACAO
BASE_SOMAS|COLABORADORES_MEDIDOS
COBERTURA_SOMA|MEDIDOS_MAIS_EXCLUIDOS_IGUAL_UNIVERSO_PARTE
ATO|hoje=2026-11-06|agora=2026-11-06T14:40:13.976256-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624
UNIVERSO_PARTE 2026-10 107
MEDIDO|2026-10|col28
MEDIDO|2026-10|col40
MEDIDO|2026-10|col70
MEDIDO|2026-10|col76
MEDIDO|2026-10|col82
DIA|2026-10|horas_intra_indenizada|col82|2026-10-06|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-08|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-12|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-14|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-15|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-16|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_intra_indenizada|col82|2026-10-20|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-10|horas_extras_50|col82|2026-10-06|CADASTRO|0|0.50064777777777847|0.50064777777777847
DIA|2026-10|horas_extras_50|col82|2026-10-08|CADASTRO|0|0.47197333333333424|0.47197333333333424
DIA|2026-10|horas_extras_50|col82|2026-10-14|CADASTRO|0|0.17922305555555529|0.17922305555555529
DIA|2026-10|horas_extras_50|col82|2026-10-15|CADASTRO|0|0.55751833333333323|0.55751833333333323
MEDIDO|2026-10|col94
MEDIDO|2026-10|col100
MEDIDO|2026-10|col106
MEDIDO|2026-10|col112
MEDIDO|2026-10|col118
MEDIDO|2026-10|col130
MEDIDO|2026-10|col136
MEDIDO|2026-10|col142
MEDIDO|2026-10|col148
MEDIDO|2026-10|col154
MEDIDO|2026-10|col166
MEDIDO|2026-10|col190
MEDIDO|2026-10|col196
MEDIDO|2026-10|col202
MEDIDO|2026-10|col214
MEDIDO|2026-10|col220
MEDIDO|2026-10|col226
MEDIDO|2026-10|col238
MEDIDO|2026-10|col250
MEDIDO|2026-10|col256
MEDIDO|2026-10|col262
RECUSA|2026-10|col268|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col268|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col274
DIA|2026-10|horas_atraso|col274|2026-10-04|ESTRUTURA|0|2.3303333333333334|2.3303333333333334
MEDIDO|2026-10|col280
MEDIDO|2026-10|col292
DIA|2026-10|horas_extras_100_noturna|col292|2026-10-12|ESTRUTURA|0|0.90987055555555552|0.90987055555555552
RECUSA|2026-10|col298|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col298|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col304
MEDIDO|2026-10|col310
HISTORICO_T|2026-10|col316|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col316|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col328
MEDIDO|2026-10|col334
MEDIDO|2026-10|col340
MEDIDO|2026-10|col346
MEDIDO|2026-10|col358
MEDIDO|2026-10|col364
RECUSA|2026-10|col370|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=28
EXCLUIDO|2026-10|col370|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col376
MEDIDO|2026-10|col382
MEDIDO|2026-10|col388
MEDIDO|2026-10|col394
RECUSA|2026-10|col400|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=1
EXCLUIDO|2026-10|col400|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col406
MEDIDO|2026-10|col418
MEDIDO|2026-10|col424
HISTORICO_T|2026-10|col430|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col430|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col442
MEDIDO|2026-10|col454
HISTORICO_T|2026-10|col460|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col460|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col466
MEDIDO|2026-10|col472
MEDIDO|2026-10|col478
MEDIDO|2026-10|col490
MEDIDO|2026-10|col496
MEDIDO|2026-10|col502
DIA|2026-10|horas_trabalhadas|col502|2026-10-10|CADASTRO|7.5691799794444456|2.8436924044444445|-4.7254875750000007
DIA|2026-10|horas_extras_50|col502|2026-10-10|CADASTRO|0.23584664611111161|0|-0.23584664611111161
MEDIDO|2026-10|col532
MEDIDO|2026-10|col556
MEDIDO|2026-10|col562
RECUSA|2026-10|col574|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=26
EXCLUIDO|2026-10|col574|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col592
DIA|2026-10|horas_trabalhadas|col592|2026-10-20|CADASTRO|7.3148602777777789|5.4212716666666676|-1.8935886111111113
MEDIDO|2026-10|col598
MEDIDO|2026-10|col610
MEDIDO|2026-10|col616
MEDIDO|2026-10|col622
MEDIDO|2026-10|col634
RECUSA|2026-10|col646|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col646|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col658
MEDIDO|2026-10|col670
MEDIDO|2026-10|col712
MEDIDO|2026-10|col718
MEDIDO|2026-10|col736
HISTORICO_T|2026-10|col760|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col760|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col784
MEDIDO|2026-10|col814
MEDIDO|2026-10|col820
MEDIDO|2026-10|col826
RECUSA|2026-10|col832|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col832|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col838
MEDIDO|2026-10|col844
MEDIDO|2026-10|col850
HISTORICO_T|2026-10|col862|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col862|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col868
MEDIDO|2026-10|col874
HISTORICO_T|2026-10|col880|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col880|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col892
MEDIDO|2026-10|col898
HISTORICO_T|2026-10|col904|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col904|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col910
MEDIDO|2026-10|col922
MEDIDO|2026-10|col928
MEDIDO|2026-10|col934
MEDIDO|2026-10|col940
MEDIDO|2026-10|col946
MEDIDO|2026-10|col952
MEDIDO|2026-10|col958
MEDIDO|2026-10|col964
DIA|2026-10|horas_noturnas|col964|2026-09-25|ESTRUTURA|1|1.1428571428571428|0.14285714285714279
DIA|2026-10|horas_noturnas|col964|2026-09-26|BATIDA|1.0158688888888889|1.1609930158730157|0.1451241269841268
DIA|2026-10|horas_noturnas|col964|2026-09-27|ESTRUTURA|0.97687750000000007|1.1164314285714285|0.13955392857142845
DIA|2026-10|horas_noturnas|col964|2026-09-28|CADASTRO|0.93996361111111115|1.074244126984127|0.13428051587301582
MEDIDO|2026-10|col970
MEDIDO|2026-10|col976
RECUSA|2026-10|col982|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=8
EXCLUIDO|2026-10|col982|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col988
DIA|2026-10|horas_trabalhadas|col988|2026-10-17|CADASTRO|6.1902003261111114|3.2689744558333333|-2.921225870277778
RECUSA|2026-10|col994|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=2
EXCLUIDO|2026-10|col994|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1000|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1000|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1006|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1006|PRODUTO:RECUSA_PORTA_SEM_DONO
COBERTURA_COLAB|2026-10|medidos=89|universo_parte=107|excluidos=18
COBERTURA_INTEGRAL|2026-10|False
PRODUTO|2026-10|DIA_FORA_UNIVERSO_PORTA|59|346
PRODUTO|2026-10|RECUSA_PORTA_SEM_DONO|11|155
PRODUTO|2026-10|SEM_DECISAO_horas_atraso|61|369
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100|60|365
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_feriado|60|365
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_noturna|60|365
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50|60|365
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50_noturna|60|365
PRODUTO|2026-10|SEM_DECISAO_horas_folga_trabalhada|59|346
PRODUTO|2026-10|SEM_DECISAO_horas_intra_indenizada|59|346
PRODUTO|2026-10|SEM_DECISAO_horas_noturnas|61|356
PRODUTO|2026-10|SEM_DECISAO_horas_saida_antecipada|61|369
PRODUTO|2026-10|SEM_DECISAO_horas_trabalhadas|59|346
PRODUTO|2026-10|SEM_RETORNO_FECHAMENTO|7|0
DIAS_NAO_LOCALIZADOS|2026-10|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=7
DONO|2026-10|horas_trabalhadas|CADASTRO|21.074240583333335|11.533938526944446|-9.5403020563888905|3|3
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|12414.601656410277|12414.601656410277|2.6645352591003757e-14|89|1333
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|73.246107319444448|73.246107319444448|0|59|346
PARCIAL|2026-10|horas_trabalhadas|v2_h=12435.675896993611|v3_h=12426.135594937223|autoridade2_h=12508.922004313055|autoridade3_h=12499.381702256667|porta_h=12426.135594937223|emitido_decidido_h=12426.135594937223|fm_dias=12508.95|fm_retorno=12508.95|colabs_div=3|dias_colab_div=3
RUBRICA|2026-10|horas_trabalhadas|12435.675896993611|12426.135594937223|-9.5403020563881|3|3
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=12416.[cpf]|v3_h=12426.135594937223|delta_h=10.122492290833179|colabs=89
DONO|2026-10|horas_intra_indenizada|CADASTRO|8.1666666666666679|0|-8.1666666666666679|1|7
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|361.38452278916668|361.38452278916668|0|89|1329
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0|0|0|59|346
PARCIAL|2026-10|horas_intra_indenizada|v2_h=369.55118945583331|v3_h=361.38452278916668|autoridade2_h=369.55118945583331|autoridade3_h=361.38452278916668|porta_h=361.38452278916668|emitido_decidido_h=361.38452278916668|fm_dias=369.56|fm_retorno=369.56|colabs_div=1|dias_colab_div=7
RUBRICA|2026-10|horas_intra_indenizada|369.55118945583331|361.38452278916668|-8.1666666666666288|1|7
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=366.55118945583331|v3_h=361.38452278916668|delta_h=-5.1666666666666288|colabs=89
DONO|2026-10|horas_extras_50|CADASTRO|0.23584664611111161|1.7093625000000012|1.4735158538888897|2|5
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|2.9002057363888882|2.9002057363888882|0|89|1312
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|2|2|0|60|365
PARCIAL|2026-10|horas_extras_50|v2_h=3.1360523825|v3_h=4.6095682363888901|autoridade2_h=5.1360523825|autoridade3_h=6.6095682363888901|porta_h=4.6095682363888901|emitido_decidido_h=4.6095682363888901|fm_dias=5.13|fm_retorno=5.13|colabs_div=2|dias_colab_div=5
RUBRICA|2026-10|horas_extras_50|3.1360523825|4.6095682363888901|1.4735158538888902|2|5
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=5.1360523825|v3_h=4.6095682363888892|delta_h=-0.52648414611111072|colabs=89
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|68.607693309444443|68.607693309444443|0|89|1317
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|3.1414417758333326|3.1414417758333326|0|60|365
PARCIAL|2026-10|horas_extras_100|v2_h=68.607693309444443|v3_h=68.607693309444443|autoridade2_h=71.749135085277771|autoridade3_h=71.749135085277771|porta_h=68.607693309444443|emitido_decidido_h=68.607693309444443|fm_dias=71.75|fm_retorno=71.75|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|68.607693309444443|68.607693309444443|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=71.749135085277771|v3_h=69.024955253888891|delta_h=-2.7241798313888808|colabs=89
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|68.607693309444443|68.607693309444443|0|89|1317
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|0|0|0|60|365
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=68.607693309444443|v3_h=68.607693309444443|autoridade2_h=68.607693309444443|autoridade3_h=68.607693309444443|porta_h=68.607693309444443|emitido_decidido_h=68.607693309444443|fm_dias=68.61|fm_retorno=68.61|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|68.607693309444443|68.607693309444443|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0|v3_h=69.024955253888891|delta_h=69.024955253888891|colabs=89
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0.3541233333333334|0.3541233333333334|0|89|1317
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0|0|0|60|365
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0.3541233333333334|v3_h=0.3541233333333334|autoridade2_h=0.3541233333333334|autoridade3_h=0.3541233333333334|porta_h=0.3541233333333334|emitido_decidido_h=0.3541233333333334|fm_dias=0.35|fm_retorno=0.35|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0.3541233333333334|0.3541233333333334|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0.3541233333333334|delta_h=0.3541233333333334|colabs=89
DONO|2026-10|horas_extras_100_noturna|ESTRUTURA|0|0.90987055555555552|0.90987055555555552|1|1
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|9.6304372222222234|9.6321452777777772|0.0017080555555537558|89|1316
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0|0|0|60|365
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=9.6304372222222234|v3_h=10.542015833333332|autoridade2_h=9.6304372222222234|autoridade3_h=10.542015833333332|porta_h=10.542015833333332|emitido_decidido_h=10.542015833333332|fm_dias=9.64|fm_retorno=9.64|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_noturna|9.6304372222222234|10.542015833333332|0.91157861111110883|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0|v3_h=10.542015833333332|delta_h=10.542015833333332|colabs=89
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0|0|0|89|1336
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0|0|0|59|346
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=92.908901666666665|v3_h=0|delta_h=-92.908901666666665|colabs=89
DONO|2026-10|horas_noturnas|BATIDA|1.0158688888888889|1.1609930158730157|0.1451241269841268|1|1
DONO|2026-10|horas_noturnas|CADASTRO|0.93996361111111115|1.074244126984127|0.13428051587301582|1|1
DONO|2026-10|horas_noturnas|ESTRUTURA|1.9768775000000001|2.2592885714285713|0.28241107142857125|1|2
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2343.5685435976188|2343.5685435976188|-2.6645352591003757e-15|89|1322
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0|0|0|61|356
PARCIAL|2026-10|horas_noturnas|v2_h=2347.5012535976189|v3_h=2348.0630693119047|autoridade2_h=2347.5012535976189|autoridade3_h=2348.0630693119047|porta_h=2348.0630693119047|emitido_decidido_h=2348.0630693119047|fm_dias=2347.51|fm_retorno=2347.51|colabs_div=1|dias_colab_div=4
RUBRICA|2026-10|horas_noturnas|2347.5012535976189|2348.0630693119047|0.56181571428578536|1|4
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2300.7555109388886|v3_h=2348.0630693119047|delta_h=47.307558373016036|colabs=89
DONO|2026-10|horas_atraso|ESTRUTURA|0|2.3303333333333334|2.3303333333333334|1|1
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|74.335833333333326|74.335448153055552|-0.0003851802777788127|88|1312
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0|0|0|61|369
PARCIAL|2026-10|horas_atraso|v2_h=74.335833333333326|v3_h=76.66578148638888|autoridade2_h=74.335833333333326|autoridade3_h=76.66578148638888|porta_h=76.66578148638888|emitido_decidido_h=76.66578148638888|fm_dias=74.36|fm_retorno=74.36|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_atraso|74.335833333333326|76.66578148638888|2.3299481530555539|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=74.350166666666667|v3_h=74.349781486388892|delta_h=-0.00038518027777456609|colabs=89
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|46.246166666666667|46.246252456944447|8.5790277777664237e-05|88|1313
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0|0|0|61|369
PARCIAL|2026-10|horas_saida_antecipada|v2_h=46.246166666666667|v3_h=46.246252456944447|autoridade2_h=46.246166666666667|autoridade3_h=46.246252456944447|porta_h=46.246252456944447|emitido_decidido_h=46.246252456944447|fm_dias=46.27|fm_retorno=46.27|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|46.246166666666667|46.246252456944447|8.5790277779551616e-05|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=32.592833333333331|v3_h=32.592919123611111|delta_h=8.5790277779551616e-05|colabs=89
UNIVERSO|2026-11|colabs_porta=578
UNIVERSO_PARTE 2026-11 99
MEDIDO|2026-11|col28
MEDIDO|2026-11|col40
MEDIDO|2026-11|col70
RECUSA|2026-11|col76|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-11|col76|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col82
DIA|2026-11|horas_trabalhadas|col82|2026-11-05|CADASTRO|7.4635486111111105|6.085836944444444|-1.3777116666666664
DIA|2026-11|horas_intra_indenizada|col82|2026-10-25|CADASTRO|0|0.20486055555555563|0.20486055555555563
DIA|2026-11|horas_intra_indenizada|col82|2026-10-27|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-11|horas_intra_indenizada|col82|2026-10-28|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-11|horas_intra_indenizada|col82|2026-10-29|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-11|horas_intra_indenizada|col82|2026-11-03|CADASTRO|1.1666666666666667|0|-1.1666666666666667
DIA|2026-11|horas_intra_indenizada|col82|2026-11-04|CADASTRO|1.1666666666666667|0|-1.1666666666666667
MEDIDO|2026-11|col94
MEDIDO|2026-11|col100
RECUSA|2026-11|col106|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=24
EXCLUIDO|2026-11|col106|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col112
MEDIDO|2026-11|col118
MEDIDO|2026-11|col130
MEDIDO|2026-11|col136
RECUSA|2026-11|col142|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,h

[truncado em 20000 de 44159 caracteres]

## 06/11 14:46 · sessao 3ad44ccc9689 · H12H06-B-PAUTA-08 · verificador · origem=Code

**PROCEDENTE, com alcance delimitado:** mover `_grava` para dentro do `atomic` cura a pauta que sobrevivia à falha de gravação, mas permite novamente PK morto no JSON quando a gravação termina e o commit posterior falha com rollback. O próprio comentário reconhece essa janela. [pauta_pago_x_devido_exportada.py:198](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198)

Análise estática da cópia; comparação com o pai, execução da suíte e ocorrência em produção: **[nao verificado]**. [nao verificado]

1. **Ordem das falhas e alcance da reversão**

   A ordem é: abrir `atomic` → criar cabeça → criar todas as filhas → acrescentar seus PKs a `rev['pautas']` → gravar JSON → sair do `atomic`. Portanto, se o JSON foi substituído e o commit depois falha com rollback, o arquivo conserva os PKs do colaborador desfeito, além dos anteriores já registrados. [pauta_pago_x_devido_exportada.py:182](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182), [pauta_pago_x_devido_exportada.py:191](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:191), [pauta_pago_x_devido_exportada.py:200](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:200)

   `_reverter` consulta cada PK: ausente ou cancelado é ignorado; feito é preservado; encontrado e pendente é passado a `cancelar`. Assim, **PK morto é tolerado**, embora a propriedade “arquivo não cita PK morto” deixe de valer nessa janela. [pauta_pago_x_devido_exportada.py:249](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:249)

   A queda durante a criação de uma filha continua protegida: acontece antes de `rev['pautas'].extend(novos)` e da atualização do arquivo. [pauta_pago_x_devido_exportada.py:194](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:194), [pauta_pago_x_devido_exportada.py:200](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:200)

   **Há caminho para atingir outro ato:** fornecer a este `--reverter` um JSON legítimo de `pauta_folga_nao_definida_exportada`. Esse produtor também registra seus PKs em `pautas`; o leitor daqui não verifica produtor, marca ou competência e chama `cancelar(p, p.autor_user)`, satisfazendo a guarda de autoria com o autor da própria pauta encontrada. Isso demonstra aceitação de arquivo alheio; a lista normalmente emitida por `_escrever` contém somente os PKs recém-criados. [pauta_folga_nao_definida_exportada.py:133](app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:133), [pauta_pago_x_devido_exportada.py:193](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193), [pauta_pago_x_devido_exportada.py:246](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:246), [services.py:288](app/pautas/services.py:288)

   Também falta vinculação ao tenant: o JSON não guarda schema, `Pauta` pertence a `TENANT_APPS` e a reversão consulta somente PK no contexto corrente. Com PK coincidente elegível em outro tenant, pode atingir pauta daquele tenant; existência desse cenário em produção: **[nao verificado]**. [pauta_pago_x_devido_exportada.py:155](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:155), [pauta_pago_x_devido_exportada.py:250](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:250), [saas.py:34](app/config/settings/saas.py:34)

2. **Cobertura e contagem de `sem_movimento`**

   Sim: `_medir` preenche `linhas[pk]` antes de colocar o PK em `sem_movimento`; o novo laço percorre integralmente esse balde. Os `continue` anteriores pertencem ao laço de `com_diferenca` e não saltam este. A conclusão pressupõe chegada normal ao trecho, sem exceção anterior. [pauta_pago_x_devido_exportada.py:133](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133), [pauta_pago_x_devido_exportada.py:162](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:162), [pauta_pago_x_devido_exportada.py:204](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:204)

   Cada PK com cabeça existente incrementa **uma vez** `ja_existia` e `ja_divergente`. Esses contadores passam a abranger fatos dos dois baldes; são indicadores sobrepostos aos baldes, não parcelas adicionais de `pedidos`. A soma dos quatro baldes permanece intacta; já `ja_existia <= com_diferenca` deixa de ser uma identidade válida. `escritas` conta cabeças/colaboradores, não filhas. [pauta_pago_x_devido_exportada.py:139](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139), [pauta_pago_x_devido_exportada.py:178](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:178), [pauta_pago_x_devido_exportada.py:202](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202), [pauta_pago_x_devido_exportada.py:208](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:208)

   Roda também com `--escrever`: não existe guarda `dry` nesse laço. Ali só há consulta, contagem e aviso; nenhuma criação, alteração ou cancelamento de pauta. A execução com `--escrever` já criou anteriormente o arquivo inicial de reversão. [pauta_pago_x_devido_exportada.py:158](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:158), [pauta_pago_x_devido_exportada.py:204](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:204)

3. **Os testes mordem pelo motivo?** [nao verificado]

   **`convergiu_ao_pago`: sim, pela inspeção.** Primeiro cria a pauta; depois recalcula o gravado e exige `sem_movimento=2`, `ja_divergente=1`, identificação da cabeça e `ATENCAO`. Sem o novo laço, faltariam o contador divergente e o aviso da cabeça. Ressalva: o teste faz **PAGO alcançar DEVIDO**, alterando o gravado; prova o tratamento da igualdade com pauta anterior, mas não reproduz especificamente uma mudança exclusiva de DEVIDO. [test_pauta_pago_x_devido_exportada.py:272](app/pautas/tests/test_pauta_pago_x_devido_exportada.py:272)

   **`queda_ao_gravar_a_reversao`: sim, pela inspeção.** O patch deixa passar a reserva inicial e lança `OSError` na atualização; exige chamadas `[True, False]` e nenhuma pauta remanescente. Com `_grava` depois do bloco, a exceção ocorreria sem desfazer as pautas daquele bloco. O teste não cobre gravação bem-sucedida seguida de falha no commit. [test_pauta_pago_x_devido_exportada.py:288](app/pautas/tests/test_pauta_pago_x_devido_exportada.py:288)

   **O `staticmethod` substitui efetivamente `self._grava`.** O patch instala o descritor na classe `Command`; o acesso pela instância entrega a função sem acrescentar `self`, compatível com `(caminho, rev, novo=False)`. As duas chamadas reais usam esse acesso pela instância. [test_pauta_pago_x_devido_exportada.py:293](app/pautas/tests/test_pauta_pago_x_devido_exportada.py:293), [test_pauta_pago_x_devido_exportada.py:300](app/pautas/tests/test_pauta_pago_x_devido_exportada.py:300), [pauta_pago_x_devido_exportada.py:159](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:159), [pauta_pago_x_devido_exportada.py:201](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:201)

4. **Quem escreve e quem lê**

   | Campo | Escritores | Leitores relevantes |
   |---|---|---|
   | `Pauta.texto` | O comando fornece cabeça/filhas; `services.escrever` persiste. Entrada humana e resposta livre também chegam ao mesmo escritor. [comando:191](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:191), [services.py:180](app/pautas/services.py:180), [views.py:109](app/pautas/views.py:109) | `_existente` lê prefixo da cabeça; `_diz_existente` compara textos das filhas; a view entrega texto à tela. [comando:224](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224), [comando:234](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:234), [views.py:40](app/pautas/views.py:40) |
   | `feito_em` | `marcar_feita`, mediante destinatário; também `marcar_feita_pelo_sistema`, com chamador concreto na pauta semanal. [services.py:240](app/pautas/services.py:240), [services.py:272](app/pautas/services.py:272), [semanal.py:226](app/pautas/semanal.py:226) | Avisos, reversão, filtro `vivas()`, estado da tela e guarda de cancelamento. Portanto, “feita pelo DP” é rótulo do comando, não autoria humana provada pelo campo. [comando:211](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211), [comando:253](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:253), [models.py:57](app/pautas/models.py:57), [services.py:290](app/pautas/services.py:290) |
   | `cancelado_em` | `services.cancelar`, chamado pela view e pela reversão. [services.py:294](app/pautas/services.py:294), [views.py:120](app/pautas/views.py:120), [comando:256](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:256) | Reversão e `nao_canceladas()`, utilizado na deduplicação e na comparação. [comando:251](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:251), [models.py:60](app/pautas/models.py:60), [comando:226](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:226) |
   | JSON de reversão | `_escrever` monta `mes`, `ano`, `gerado_em`, `plano` e `pautas`; acrescenta os novos PKs; `_grava` serializa em temporário e faz `replace`. [comando:155](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:155), [comando:200](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:200), [comando:274](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:274) | `_reverter` usa somente `pautas`; não usa os demais campos para validar a origem ou restaurar números. [comando:246](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:246) |

5. **Novidades introduzidas pelas curas**

   A novidade problemática demonstrável é a janela **JSON atualizado → commit/rollback**, sem transação comum entre arquivo e banco. Também passa a haver operação de filesystem enquanto a trava do colaborador continua retida; impacto operacional: **[nao verificado]**. [pauta_pago_x_devido_exportada.py:185](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:185), [pauta_pago_x_devido_exportada.py:201](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:201), [pauta_pago_x_devido_exportada.py:277](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:277)

   O novo laço introduz consulta e aviso para `sem_movimento`, além do alargamento dos contadores descrito acima; não introduz escrita nem cancelamento nesse caminho. Atribuir ao delta a falta de validação da origem do JSON depende do pai: **[nao verificado]**. [pauta_pago_x_devido_exportada.py:204](app/pautas/management/commands/pauta_pago_x_devido_exportada.py:204)

## 06/11 14:44 · sessao 268eb97a2155 · - · - · origem=Ronald (tmux)

- **Sim, reaparece a possibilidade de PK morto no arquivo**, especificamente depois de todas as filhas: cria cabeça/filhas, junta PKs, grava o JSON e só então sai do `atomic`. Se essa saída falhar e a transação for desfeita, o arquivo conserva os PKs daquela transação; o próprio comentário reconhece esse caso. A queda na filha continua ocorrendo antes da atualização do arquivo. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`, `:191`, `:194`, `:198`, `:200`, `:201`.)

- **Na reversão, PK ausente é ignorado**; também ignora cancelada e preserva feita. Se encontrar pauta viva com aquele PK, chama `cancelar(p, p.autor_user)`: não confere marca, competência, texto, plano ou identidade do ato. A guarda de autoria do service fica satisfeita pelo próprio autor da pauta encontrada. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:249`, `:251`, `:253`, `:256`; `app/pautas/services.py:288`, `:294`.)

- **Outro caminho comprovável para pauta de outro ato:** fornecer ao `--reverter` um JSON produzido por `pauta_folga_nao_definida_exportada`. Esse produtor também grava seus PKs em `rev['pautas']`; o leitor deste comando aceita a lista sem distinguir o produtor. Não exige adulteração do JSON, reaproveitamento de PK ou colisão entre tenants. (`app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:133`, `:136`, `:141`, `:149`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:246`, `:249`, `:256`.)

- **Também falta vínculo ao tenant:** JSON gerado não contém schema; `Pauta` integra `TENANT_APPS`, com backend por schema. Portanto, arquivo do tenant A levado à reversão no tenant B consulta os PKs de B. Existirem PKs coincidentes e pautas elegíveis em produção é **[nao verificado]**. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:155`, `:250`; `app/config/settings/saas.py:34`, `:47`, `:83`; `app/tenants/models.py:4`.)

- **Atribuição ao delta:** a ordem atual explica a nova janela entre gravação e commit; afirmar que os problemas de proveniência do JSON foram introduzidos agora depende da versão pai, **[nao verificado]**. O novo teste provoca a exceção **antes** da gravação real da atualização; não testa gravação bem sucedida seguida de falha no commit. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:293`, `:295`, `:297`, `:303`.)

- **A atomicidade banco/arquivo continua limitada:** `_grava` usa arquivo temporário e `os.replace`, sem mecanismo que reverta o arquivo em caso de rollback posterior. O `atomic` envolve as escritas do banco e a chamada ao filesystem, mas não estabelece uma transação comum entre os dois recursos. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`, `:201`, `:274`, `:277`.)

## 06/11 14:44 · sessao d36652b83984 · - · - · origem=Ronald (tmux)

Mapa de proveniência, limitado ao código lido:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Pauta.texto` | Este comando monta cabeça/filhas e chama `_ps.escrever` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:166`, `:168`, `:191`, `:195`; a persistência é `Pauta.objects.create(texto=texto[:500])` em `app/pautas/services.py:180`, `:185`. Entradas humanas passam texto livre da view para o mesmo service: `app/pautas/views.py:76`, `:81`, `:109`, `:113`; respostas passam por `app/pautas/services.py:192`, `:200`. | Deduplicação da cabeça por prefixo em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:226`; comparação das filhas em `:234`, `:236`. Payload da tela em `app/pautas/views.py:40`, `:47`; DOM via `textContent` em `app/templates/core/_copiloto.html:405`, `:424`. API de mensageria em `app/api/views_mensageria.py:1334`; detalhe do colaborador em `app/templates/colaboradores/detalhe.html:119`. |
| `feito_em` | Humano destinatário: `marcar_feita`, com guarda de destinatário e atualização em `app/pautas/services.py:240`, `:247`, `:248`. Sistema: `marcar_feita_pelo_sistema`, atualização em `app/pautas/services.py:256`, `:272`, `:273`; há chamador concreto em `app/pautas/semanal.py:226`, `:229`. View humana chama o service em `app/pautas/views.py:101`, `:103`. | Avisos deste comando em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:211`, `:238`; reversão pula feitas em `:253`. `PautaQuerySet.vivas()` em `app/pautas/models.py:57`, `:58`; estado/botões da tela em `app/pautas/views.py:43`, `:56`, `:58`. O próprio cancelamento recusa feita em `app/pautas/services.py:290`. |
| `cancelado_em` | `cancelar`: valida autoria, recusa feita e atualiza em `app/pautas/services.py:288`, `:290`, `:294`. View chama em `app/pautas/views.py:120`, `:123`; reversão chama em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:256`. | Reversão pula canceladas em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:251`; `nao_canceladas()` filtra em `app/pautas/models.py:60`, `:64`, usado pela deduplicação e comparação em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:226`, `:234`. |
| JSON de reversão | `_escrever` monta `mes`, `ano`, `gerado_em`, `pautas` e `plano` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:155`, `:157`; cria arquivo inicial em `:159`; acrescenta somente `novos`, obtidos das chamadas de escrita, em `:193`, `:195`, `:200`; serializa com `json.dump`, temporário e `os.replace` em `:274`, `:276`, `:277`. | `_reverter` carrega JSON em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:246`, `:247` e usa **somente `rev['pautas']`**, consultando PK em `:249`, `:250`. Não confere `mes`, `ano`, `plano`, schema, chave textual ou identidade do ato antes de cancelar; passa o próprio `p.autor_user` à porta em `:256`. |

O contador lê esses estados por `Pauta.objects.vivas()` em `app/pautas/services.py:348`; listas também passam por `nao_canceladas()`/`vivas()` em `:323`, `:327`. Isso é leitura de estado, sem evidência de novo ciclo de trava nestas linhas (`app/pautas/services.py:348`, `:352`, `:354`).

Consequência concreta para “pauta que não é deste ato”: um arquivo de outra competência/schema, contendo PK que existe no banco corrente, será resolvido exclusivamente por esse PK; se a pauta estiver não feita/não cancelada, `_reverter` chama `cancelar(p, p.autor_user)`, satisfazendo a guarda de autoria com o autor da própria pauta consultada (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:249`, `:250`, `:251`, `:253`, `:256`; `app/pautas/services.py:288`). Ocorrência desse uso em produção: [nao verificado].

## 06/11 14:40 · sessao d5d516c38a29 · V3-IMPACTO-COM-DONO · sonda · origem=Code

Todas as linhas `SOMA` da parte apresentam indicadores `True`. Não há ocorrência de `INCOMPLETO` no dado fornecido. [nao verificado]

Transcrição integral do dado recebido: [nao verificado]

```text
sonda_leitura: script.py [ed91ca5ab97aac60] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|4|6
FINALIDADE|IMPACTO_NUNCA_VALIDACAO
BASE_SOMAS|COLABORADORES_MEDIDOS
COBERTURA_SOMA|MEDIDOS_MAIS_EXCLUIDOS_IGUAL_UNIVERSO_PARTE
ATO|hoje=2026-11-06|agora=2026-11-06T14:30:57.486394-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624
UNIVERSO_PARTE 2026-10 96
MEDIDO|2026-10|col27
MEDIDO|2026-10|col33
MEDIDO|2026-10|col39
MEDIDO|2026-10|col51
MEDIDO|2026-10|col63
MEDIDO|2026-10|col69
RECUSA|2026-10|col75|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col75|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col81
MEDIDO|2026-10|col87
DIA|2026-10|horas_atraso|col87|2026-09-22|CADASTRO|0|0.186|0.186
DIA|2026-10|horas_saida_antecipada|col87|2026-09-22|CADASTRO|1.0746666666666667|0.88850000000000007|-0.18616666666666659
MEDIDO|2026-10|col93
MEDIDO|2026-10|col99
MEDIDO|2026-10|col111
MEDIDO|2026-10|col117
MEDIDO|2026-10|col123
MEDIDO|2026-10|col129
MEDIDO|2026-10|col141
MEDIDO|2026-10|col159
MEDIDO|2026-10|col165
MEDIDO|2026-10|col171
MEDIDO|2026-10|col177
MEDIDO|2026-10|col189
MEDIDO|2026-10|col195
MEDIDO|2026-10|col207
MEDIDO|2026-10|col219
RECUSA|2026-10|col225|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=25
EXCLUIDO|2026-10|col225|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col231
MEDIDO|2026-10|col237
MEDIDO|2026-10|col243
MEDIDO|2026-10|col249
MEDIDO|2026-10|col255
MEDIDO|2026-10|col267
RECUSA|2026-10|col291|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col291|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col297
MEDIDO|2026-10|col303
MEDIDO|2026-10|col309
MEDIDO|2026-10|col327
MEDIDO|2026-10|col345
MEDIDO|2026-10|col351
MEDIDO|2026-10|col357
MEDIDO|2026-10|col363
MEDIDO|2026-10|col369
HISTORICO_T|2026-10|col393|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col393|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col399
MEDIDO|2026-10|col411
RECUSA|2026-10|col417|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=10
EXCLUIDO|2026-10|col417|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col435
MEDIDO|2026-10|col441
MEDIDO|2026-10|col465
MEDIDO|2026-10|col489
DIA|2026-10|horas_noturnas|col489|2026-09-23|ESTRUTURA|6.0007740680555548|6.8580275063492051|0.85725343829365031
DIA|2026-10|horas_noturnas|col489|2026-09-25|ESTRUTURA|6.000494001111111|6.8577074298412688|0.85721342873015782
DIA|2026-10|horas_noturnas|col489|2026-09-27|ESTRUTURA|6.0004218163888892|6.8576249330158721|0.8572031166269829
MEDIDO|2026-10|col507
MEDIDO|2026-10|col549
HISTORICO_T|2026-10|col555|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col555|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col567
MEDIDO|2026-10|col579
MEDIDO|2026-10|col627
MEDIDO|2026-10|col639
RECUSA|2026-10|col645|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col645|PRODUTO:RECUSA_PORTA_SEM_DONO
HISTORICO_T|2026-10|col651|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col651|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col663
HISTORICO_T|2026-10|col669|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col669|PRODUTO:SEM_RETORNO_FECHAMENTO
RECUSA|2026-10|col675|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col675|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col699
RECUSA|2026-10|col717|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col717|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col723
MEDIDO|2026-10|col735
MEDIDO|2026-10|col741
MEDIDO|2026-10|col747
MEDIDO|2026-10|col789
MEDIDO|2026-10|col819
MEDIDO|2026-10|col825
DIA|2026-10|horas_extras_100_noturna|col825|2026-10-12|ESTRUTURA|0|0.56262055555555557|0.56262055555555557
MEDIDO|2026-10|col843
MEDIDO|2026-10|col849
DIA|2026-10|horas_intra_indenizada|col849|2026-10-09|CADASTRO|1|0|-1
DIA|2026-10|horas_extras_100_noturna|col849|2026-10-12|ESTRUTURA|0|0.84161578277777771|0.84161578277777771
MEDIDO|2026-10|col855
MEDIDO|2026-10|col861
MEDIDO|2026-10|col873
MEDIDO|2026-10|col879
MEDIDO|2026-10|col885
MEDIDO|2026-10|col891
MEDIDO|2026-10|col903
MEDIDO|2026-10|col909
MEDIDO|2026-10|col915
DIA|2026-10|horas_intra_indenizada|col915|2026-09-27|CADASTRO|0|0.18198583333333324|0.18198583333333324
DIA|2026-10|horas_intra_indenizada|col915|2026-10-01|CADASTRO|0|0.18274083333333344|0.18274083333333344
DIA|2026-10|horas_extras_50|col915|2026-09-27|CADASTRO|0.32020749999999981|0|-0.32020749999999981
DIA|2026-10|horas_extras_50|col915|2026-10-01|CADASTRO|0.51604972222222234|0.33330888888888904|-0.1827408333333333
MEDIDO|2026-10|col921
HISTORICO_T|2026-10|col927|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col927|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col933
MEDIDO|2026-10|col939
MEDIDO|2026-10|col945
MEDIDO|2026-10|col951
MEDIDO|2026-10|col957
EXCLUIDO|2026-10|col963|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
RECUSA|2026-10|col969|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=21
EXCLUIDO|2026-10|col969|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col975|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col975|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col981
MEDIDO|2026-10|col987
MEDIDO|2026-10|col993
RECUSA|2026-10|col999|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col999|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1005|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1005|PRODUTO:RECUSA_PORTA_SEM_DONO
COBERTURA_COLAB|2026-10|medidos=79|universo_parte=96|excluidos=17
COBERTURA_INTEGRAL|2026-10|False
PRODUTO|2026-10|DIA_FORA_UNIVERSO_PORTA|48|321
PRODUTO|2026-10|MOTOR_FECHAMENTO_horas_trabalhadas|1|19
PRODUTO|2026-10|RECUSA_PORTA_SEM_DONO|11|191
PRODUTO|2026-10|SEM_DECISAO_horas_atraso|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_feriado|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_noturna|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50_noturna|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_folga_trabalhada|48|321
PRODUTO|2026-10|SEM_DECISAO_horas_intra_indenizada|48|321
PRODUTO|2026-10|SEM_DECISAO_horas_noturnas|49|332
PRODUTO|2026-10|SEM_DECISAO_horas_saida_antecipada|49|340
PRODUTO|2026-10|SEM_DECISAO_horas_trabalhadas|48|321
PRODUTO|2026-10|SEM_RETORNO_FECHAMENTO|5|0
DIAS_NAO_LOCALIZADOS|2026-10|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=5
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|11069.925809861945|11069.925809861945|-1.865174681370263e-14|79|1236
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|54.076144343611112|54.076144343611112|0|48|321
PARCIAL|2026-10|horas_trabalhadas|v2_h=11069.925809861945|v3_h=11069.925809861945|autoridade2_h=11124.001954205556|autoridade3_h=11124.001954205556|porta_h=11069.925809861945|emitido_decidido_h=11069.925809861945|fm_dias=11123.99|fm_retorno=11123.99|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_trabalhadas|11069.925809861945|11069.925809861945|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=11034.459172286666|v3_h=11069.925809861945|delta_h=35.466637575278583|colabs=79
DONO|2026-10|horas_intra_indenizada|CADASTRO|1|0.36472666666666664|-0.63527333333333336|2|3
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|278.88592557722222|278.88592557722222|0|79|1233
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0|0|0|48|321
PARCIAL|2026-10|horas_intra_indenizada|v2_h=279.88592557722222|v3_h=279.25065224388891|autoridade2_h=279.88592557722222|autoridade3_h=279.25065224388891|porta_h=279.25065224388891|emitido_decidido_h=279.25065224388891|fm_dias=279.88|fm_retorno=279.88|colabs_div=2|dias_colab_div=3
RUBRICA|2026-10|horas_intra_indenizada|279.88592557722222|279.25065224388891|-0.63527333333331626|2|3
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=279.50901613277779|v3_h=279.25065224388891|delta_h=-0.25836388888887996|colabs=79
DONO|2026-10|horas_extras_50|CADASTRO|0.83625722222222221|0.33330888888888904|-0.50294833333333311|1|2
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|11.450561862500001|11.450561862499999|-1.7763568394002505e-15|79|1215
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0.43086277777777771|0.43086277777777771|0|49|340
PARCIAL|2026-10|horas_extras_50|v2_h=12.286819084722222|v3_h=11.783870751388887|autoridade2_h=12.717681862500001|autoridade3_h=12.214733529166665|porta_h=11.783870751388887|emitido_decidido_h=11.783870751388887|fm_dias=12.72|fm_retorno=12.72|colabs_div=1|dias_colab_div=2
RUBRICA|2026-10|horas_extras_50|12.286819084722222|11.783870751388887|-0.50294833333333422|1|2
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=12.717681862500001|v3_h=11.783870751388889|delta_h=-0.93381111111111181|colabs=79
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|56.767440009722222|56.767440009722222|-3.5527136788005009e-15|79|1217
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|0|0|0|49|340
PARCIAL|2026-10|horas_extras_100|v2_h=56.767440009722222|v3_h=56.767440009722222|autoridade2_h=56.767440009722222|autoridade3_h=56.767440009722222|porta_h=56.767440009722222|emitido_decidido_h=56.767440009722222|fm_dias=56.76|fm_retorno=56.76|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100|56.767440009722222|56.767440009722222|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=56.767440009722222|v3_h=56.767440009722222|delta_h=0|colabs=79
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|56.161124683055554|56.161124683055554|-3.5527136788005009e-15|79|1217
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|0|0|0|49|340
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=56.161124683055554|v3_h=56.161124683055554|autoridade2_h=56.161124683055554|autoridade3_h=56.161124683055554|porta_h=56.161124683055554|emitido_decidido_h=56.161124683055554|fm_dias=56.16|fm_retorno=56.16|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_feriado|56.161124683055554|56.161124683055554|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0|v3_h=56.161124683055554|delta_h=56.161124683055554|colabs=79
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0.47472906281506683|0.47472906281506683|0|79|1217
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0|0|0|49|340
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0.47472906281506683|v3_h=0.47472906281506683|autoridade2_h=0.47472906281506683|autoridade3_h=0.47472906281506683|porta_h=0.47472906281506683|emitido_decidido_h=0.47472906281506683|fm_dias=0.47|fm_retorno=0.47|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0.47472906281506683|0.47472906281506683|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0.47472906281506683|delta_h=0.47472906281506683|colabs=79
DONO|2026-10|horas_extras_100_noturna|ESTRUTURA|0|1.4042363383333334|1.4042363383333334|2|2
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|5.1377437149627116|5.1377437149627116|0|79|1215
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0|0|0|49|340
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=5.1377437149627116|v3_h=6.5419800532960446|autoridade2_h=5.1377437149627116|autoridade3_h=6.5419800532960446|porta_h=6.5419800532960446|emitido_decidido_h=6.5419800532960446|fm_dias=5.14|fm_retorno=5.14|colabs_div=2|dias_colab_div=2
RUBRICA|2026-10|horas_extras_100_noturna|5.1377437149627116|6.5419800532960446|1.4042363383333329|2|2
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0|v3_h=6.5419800532960446|delta_h=6.5419800532960446|colabs=79
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0|0|0|79|1236
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0|0|0|48|321
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=89.542781918888892|v3_h=0|delta_h=-89.542781918888892|colabs=79
DONO|2026-10|horas_noturnas|ESTRUTURA|18.001689885555557|20.573359869206346|2.571669983650791|1|3
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2489.2247011865875|2489.2247011865875|-7.9936057773011271e-15|79|1222
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0|0|0|49|332
PARCIAL|2026-10|horas_noturnas|v2_h=2507.2263910721426|v3_h=2509.7980610557938|autoridade2_h=2507.2263910721426|autoridade3_h=2509.7980610557938|porta_h=2509.7980610557938|emitido_decidido_h=2509.7980610557938|fm_dias=2507.23|fm_retorno=2507.23|colabs_div=1|dias_colab_div=3
RUBRICA|2026-10|horas_noturnas|2507.2263910721426|2509.7980610557938|2.5716699836511907|1|3
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2451.6395532943652|v3_h=2509.7980610557938|delta_h=58.158507761428609|colabs=79
DONO|2026-10|horas_atraso|CADASTRO|0|0.186|0.186|1|1
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|58.961666666666666|58.961705821666669|3.9154999999707618e-05|79|1216
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0|0|0|49|340
PARCIAL|2026-10|horas_atraso|v2_h=58.961666666666666|v3_h=59.147705821666669|autoridade2_h=58.961666666666666|autoridade3_h=59.147705821666669|porta_h=59.147705821666669|emitido_decidido_h=59.147705821666669|fm_dias=58.97|fm_retorno=58.97|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_atraso|58.961666666666666|59.147705821666669|0.18603915500000312|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=58.324833333333338|v3_h=58.510826655000002|delta_h=0.18599332166666471|colabs=79
DONO|2026-10|horas_saida_antecipada|CADASTRO|1.0746666666666667|0.88850000000000007|-0.18616666666666659|1|1
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|53.484833333333334|53.484736725277777|-9.6608055555502453e-05|79|1216
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0|0|0|49|340
PARCIAL|2026-10|horas_saida_antecipada|v2_h=54.5595|v3_h=54.373236725277778|autoridade2_h=54.5595|autoridade3_h=54.373236725277778|porta_h=54.373236725277778|emitido_decidido_h=54.373236725277778|fm_dias=54.54|fm_retorno=54.54|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_saida_antecipada|54.5595|54.373236725277778|-0.18626327472222215|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=54.248333333333335|v3_h=54.062082003055558|delta_h=-0.18625133027777707|colabs=79
UNIVERSO|2026-11|colabs_porta=578
UNIVERSO_PARTE 2026-11 91
MEDIDO|2026-11|col27
MEDIDO|2026-11|col33
MEDIDO|2026-11|col39
MEDIDO|2026-11|col51
MEDIDO|2026-11|col63
MEDIDO|2026-11|col69
RECUSA|2026-11|col75|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=16
EXCLUIDO|2026-11|col75|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col81
MEDIDO|2026-11|col87
DIA|2026-11|horas_intra_indenizada|col87|2026-11-03|CADASTRO|0|0.20321111111111112|0.20321111111111112
DIA|2026-11|horas_intra_indenizada|col87|2026-11-04|CADASTRO|0|0.20013194444444443|0.20013194444444443
DIA|2026-11|horas_atraso|col87|2026-10-26|CADASTRO|0|0.20800000000000002|0.20800000000000002
DIA|2026-11|horas_atraso|col87|2026-11-03|CADASTRO|0|0.33000000000000002|0.33000000000000002
DIA|2026-11|horas_atraso|col87|2026-11-04|CADASTRO|0|0.21733333333333332|0.21733333333333332
DIA|2026-11|horas_saida_antecipada|col87|2026-10-26|CADASTRO|1.0506666666666666|0.84249999999999992|-0.20816666666666672
DIA|2026-11|horas_saida_antecipada|col87|2026-11-03|CADASTRO|0.81133333333333335|0.48149999999999998|-0.32983333333333337
DIA|2026-11|horas_saida_antecipada|col87|2026-11-04|CADASTRO|0.88200000000000001|0.66466666666666674|-0.21733333333333327
MEDIDO|2026-11|col93
MEDIDO|2026-11|col99
MEDIDO|2026-11|col111
MEDIDO|2026-11|col117
MEDIDO|2026-11|col123
MEDIDO|2026-11|col129
RECUSA|2026-11|col141|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=24
EXCLUIDO|2026-11|col141|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col159
MEDIDO|2026-11|col165
MEDIDO|2026-11|col171
RECUSA|2026-11|col177|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=24
EXCLUIDO|2026-11|col177|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col189
MEDIDO|2026-11|col195
MEDIDO|2026-11|col207
MEDIDO|2026-11|col219
RECUSA|2026-11|col225|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=16
EXCLUIDO|2026-11|col225|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col231
MEDIDO|2026-11|col237
MEDIDO|2026-11|col243
MEDIDO|2026-11|col249
MEDIDO|2026-11|col255
MEDIDO|2026-11|col267
RECUSA|2026-11|col291|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-11|col291|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col297
RECUSA|2026-11|col303|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=23
E

[truncado em 20000 de 42137 caracteres]

## 06/11 14:30 · sessao c8fbea551bb5 · V3-IMPACTO-COM-DONO · sonda · origem=Code

Todas as linhas `SOMA` da parte apresentam `True`. Não há ocorrência de `INCOMPLETO` no dado fornecido. [nao verificado]

Transcrição integral do dado fornecido, sem verificação independente: [nao verificado]

```text
sonda_leitura: script.py [c4ac0aa8733fa696] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|3|6
FINALIDADE|IMPACTO_NUNCA_VALIDACAO
BASE_SOMAS|COLABORADORES_MEDIDOS
COBERTURA_SOMA|MEDIDOS_MAIS_EXCLUIDOS_IGUAL_UNIVERSO_PARTE
ATO|hoje=2026-11-06|agora=2026-11-06T14:21:50.685272-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624
UNIVERSO_PARTE 2026-10 102
MEDIDO|2026-10|col32
MEDIDO|2026-10|col38
MEDIDO|2026-10|col44
MEDIDO|2026-10|col50
MEDIDO|2026-10|col56
MEDIDO|2026-10|col62
MEDIDO|2026-10|col80
MEDIDO|2026-10|col92
MEDIDO|2026-10|col98
MEDIDO|2026-10|col104
RECUSA|2026-10|col110|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col110|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col116
RECUSA|2026-10|col128|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col128|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col134
MEDIDO|2026-10|col146
MEDIDO|2026-10|col152
MEDIDO|2026-10|col170
MEDIDO|2026-10|col188
MEDIDO|2026-10|col194
MEDIDO|2026-10|col200
MEDIDO|2026-10|col206
MEDIDO|2026-10|col212
MEDIDO|2026-10|col218
MEDIDO|2026-10|col224
MEDIDO|2026-10|col236
MEDIDO|2026-10|col242
MEDIDO|2026-10|col248
MEDIDO|2026-10|col254
MEDIDO|2026-10|col266
MEDIDO|2026-10|col272
MEDIDO|2026-10|col278
MEDIDO|2026-10|col284
MEDIDO|2026-10|col290
MEDIDO|2026-10|col296
MEDIDO|2026-10|col302
MEDIDO|2026-10|col320
MEDIDO|2026-10|col332
RECUSA|2026-10|col338|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=2
EXCLUIDO|2026-10|col338|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col344
MEDIDO|2026-10|col362
MEDIDO|2026-10|col368
DIA|2026-10|horas_atraso|col368|2026-09-21|ESTRUTURA|0|1.1068333333333333|1.1068333333333333
MEDIDO|2026-10|col374
MEDIDO|2026-10|col392
MEDIDO|2026-10|col398
MEDIDO|2026-10|col416
RECUSA|2026-10|col422|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=2
EXCLUIDO|2026-10|col422|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col428
RECUSA|2026-10|col440|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col440|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col446
MEDIDO|2026-10|col452
MEDIDO|2026-10|col464
EXCLUIDO|2026-10|col476|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
MEDIDO|2026-10|col482
HISTORICO_T|2026-10|col506|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col506|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col512
DIA|2026-10|horas_trabalhadas|col512|2026-10-11|ESTRUTURA|6.4782122222222229|11.442890555555556|4.9646783333333326
DIA|2026-10|horas_intra_indenizada|col512|2026-10-11|ESTRUTURA|0|1|1
DIA|2026-10|horas_saida_antecipada|col512|2026-10-11|ESTRUTURA|0.5571666666666667|0|-0.5571666666666667
MEDIDO|2026-10|col518
MEDIDO|2026-10|col566
MEDIDO|2026-10|col572
MEDIDO|2026-10|col584
MEDIDO|2026-10|col608
MEDIDO|2026-10|col626
MEDIDO|2026-10|col638
DIA|2026-10|horas_intra_indenizada|col638|2026-10-01|CADASTRO|1.5|0|-1.5
DIA|2026-10|horas_intra_indenizada|col638|2026-10-04|CADASTRO|0|0.49735333333333337|0.49735333333333337
DIA|2026-10|horas_intra_indenizada|col638|2026-10-11|CADASTRO|0|0.49678972222222223|0.49678972222222223
RECUSA|2026-10|col644|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col644|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col650|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col650|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col668
RECUSA|2026-10|col674|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col674|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col698
MEDIDO|2026-10|col704
MEDIDO|2026-10|col746
MEDIDO|2026-10|col752
RECUSA|2026-10|col758|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col758|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col782|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=3
EXCLUIDO|2026-10|col782|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col788
DIA|2026-10|horas_trabalhadas|col788|2026-09-24|BATIDA|8.9642727777777775|0.96427277777777776|-8
DIA|2026-10|horas_intra_indenizada|col788|2026-09-25|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col788|2026-10-01|CADASTRO|1|0|-1
DIA|2026-10|horas_intra_indenizada|col788|2026-10-05|BATIDA|1|0.44918999999999998|-0.55081000000000002
MEDIDO|2026-10|col824
DIA|2026-10|horas_intra_indenizada|col824|2026-10-04|BATIDA|1|0|-1
DIA|2026-10|horas_intra_indenizada|col824|2026-10-18|CADASTRO|1|0|-1
MEDIDO|2026-10|col830
DIA|2026-10|horas_extras_100|col830|2026-10-12|ESTRUTURA|0|12.149131944444443|12.149131944444443
DIA|2026-10|horas_extras_100_feriado|col830|2026-10-12|ESTRUTURA|0|12.149131944444443|12.149131944444443
MEDIDO|2026-10|col836
MEDIDO|2026-10|col848
MEDIDO|2026-10|col854
MEDIDO|2026-10|col860
MEDIDO|2026-10|col866
MEDIDO|2026-10|col872
MEDIDO|2026-10|col878
MEDIDO|2026-10|col884
MEDIDO|2026-10|col890
MEDIDO|2026-10|col902
MEDIDO|2026-10|col908
MEDIDO|2026-10|col920
MEDIDO|2026-10|col926
MEDIDO|2026-10|col932
MEDIDO|2026-10|col938
MEDIDO|2026-10|col944
MEDIDO|2026-10|col950
MEDIDO|2026-10|col956
MEDIDO|2026-10|col962
EXCLUIDO|2026-10|col968|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
MEDIDO|2026-10|col974
MEDIDO|2026-10|col980
MEDIDO|2026-10|col986
MEDIDO|2026-10|col992
RECUSA|2026-10|col998|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col998|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1004|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1004|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1010|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=10
EXCLUIDO|2026-10|col1010|PRODUTO:RECUSA_PORTA_SEM_DONO
COBERTURA_COLAB|2026-10|medidos=86|universo_parte=102|excluidos=16
COBERTURA_INTEGRAL|2026-10|False
PRODUTO|2026-10|DIA_FORA_UNIVERSO_PORTA|58|324
PRODUTO|2026-10|MOTOR_FECHAMENTO_horas_trabalhadas|2|42
PRODUTO|2026-10|RECUSA_PORTA_SEM_DONO|13|137
PRODUTO|2026-10|SEM_DECISAO_horas_atraso|61|370
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100|59|352
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_feriado|59|352
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_noturna|59|352
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50|59|352
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50_noturna|59|352
PRODUTO|2026-10|SEM_DECISAO_horas_folga_trabalhada|58|324
PRODUTO|2026-10|SEM_DECISAO_horas_intra_indenizada|58|324
PRODUTO|2026-10|SEM_DECISAO_horas_noturnas|59|335
PRODUTO|2026-10|SEM_DECISAO_horas_saida_antecipada|61|370
PRODUTO|2026-10|SEM_DECISAO_horas_trabalhadas|58|324
PRODUTO|2026-10|SEM_RETORNO_FECHAMENTO|1|0
DIAS_NAO_LOCALIZADOS|2026-10|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=1
DONO|2026-10|horas_trabalhadas|BATIDA|8.9642727777777775|0.96427277777777776|-8|1|1
DONO|2026-10|horas_trabalhadas|ESTRUTURA|6.4782122222222229|11.442890555555556|4.9646783333333326|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|11535.529855139999|11535.529855139999|6.2172489379008766e-15|86|1222
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|22.868550555555554|22.868550555555554|0|58|324
PARCIAL|2026-10|horas_trabalhadas|v2_h=11550.972340140001|v3_h=11547.937018473333|autoridade2_h=11573.840890695556|autoridade3_h=11570.805569028889|porta_h=11547.937018473333|emitido_decidido_h=11547.937018473333|fm_dias=11573.86|fm_retorno=11573.86|colabs_div=2|dias_colab_div=2
RUBRICA|2026-10|horas_trabalhadas|11550.972340140001|11547.937018473333|-3.0353216666680964|2|2
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=11417.777769862221|v3_h=11547.937018473333|delta_h=130.15924861111125|colabs=86
DONO|2026-10|horas_intra_indenizada|BATIDA|2|0.44918999999999998|-1.55081|2|2
DONO|2026-10|horas_intra_indenizada|CADASTRO|4.5|0.9941430555555556|-3.5058569444444445|3|6
DONO|2026-10|horas_intra_indenizada|ESTRUTURA|0|1|1|1|1
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|385.50810743694444|385.50810743694444|0|86|1215
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0|0|0|58|324
PARCIAL|2026-10|horas_intra_indenizada|v2_h=392.00810743694444|v3_h=387.95144049250001|autoridade2_h=392.00810743694444|autoridade3_h=387.95144049250001|porta_h=387.95144049250001|emitido_decidido_h=387.95144049250001|fm_dias=392.01|fm_retorno=392.01|colabs_div=4|dias_colab_div=9
RUBRICA|2026-10|horas_intra_indenizada|392.00810743694444|387.95144049250001|-4.0566669444444301|4|9
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=388.00810743694444|v3_h=387.95144049250001|delta_h=-0.056666944444430101|colabs=86
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|1|1|0|86|1196
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0|0|0|59|352
PARCIAL|2026-10|horas_extras_50|v2_h=1|v3_h=1|autoridade2_h=1|autoridade3_h=1|porta_h=1|emitido_decidido_h=1|fm_dias=1.0|fm_retorno=1.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50|1|1|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=1|v3_h=1|delta_h=0|colabs=86
DONO|2026-10|horas_extras_100|ESTRUTURA|0|12.149131944444443|12.149131944444443|1|1
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|46.352280248611109|46.352280248611109|-8.8817841970012523e-16|86|1195
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|0|0|0|59|352
PARCIAL|2026-10|horas_extras_100|v2_h=46.352280248611109|v3_h=58.50141219305555|autoridade2_h=46.352280248611109|autoridade3_h=58.50141219305555|porta_h=58.50141219305555|emitido_decidido_h=58.50141219305555|fm_dias=46.34|fm_retorno=46.34|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100|46.352280248611109|58.50141219305555|12.149131944444441|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=46.352280248611109|v3_h=58.50141219305555|delta_h=12.149131944444441|colabs=86
DONO|2026-10|horas_extras_100_feriado|ESTRUTURA|0|12.149131944444443|12.149131944444443|1|1
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|46.352280248611109|46.352280248611109|-8.8817841970012523e-16|86|1195
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|0|0|0|59|352
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=46.352280248611109|v3_h=58.50141219305555|autoridade2_h=46.352280248611109|autoridade3_h=58.50141219305555|porta_h=58.50141219305555|emitido_decidido_h=58.50141219305555|fm_dias=46.34|fm_retorno=46.34|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_feriado|46.352280248611109|58.50141219305555|12.149131944444441|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0|v3_h=58.50141219305555|delta_h=58.50141219305555|colabs=86
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0|0|0|86|1196
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0|0|0|59|352
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|delta_h=0|colabs=86
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|11.780694166666667|11.794853888888889|0.014159722222222726|86|1196
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0|0|0|59|352
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=11.780694166666667|v3_h=11.794853888888889|autoridade2_h=11.780694166666667|autoridade3_h=11.794853888888889|porta_h=11.794853888888889|emitido_decidido_h=11.794853888888889|fm_dias=11.78|fm_retorno=11.78|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_100_noturna|11.780694166666667|11.794853888888889|0.014159722222222726|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0|v3_h=11.794853888888889|delta_h=11.794853888888889|colabs=86
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0|0|0|86|1224
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0|0|0|58|324
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=148.95005888888889|v3_h=0|delta_h=-148.95005888888889|colabs=86
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|2495.6564157072617|2495.6564157072617|-2.6645352591003757e-15|86|1213
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0|0|0|59|335
PARCIAL|2026-10|horas_noturnas|v2_h=2495.6564157072617|v3_h=2495.6564157072617|autoridade2_h=2495.6564157072617|autoridade3_h=2495.6564157072617|porta_h=2495.6564157072617|emitido_decidido_h=2495.6564157072617|fm_dias=2495.65|fm_retorno=2495.65|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_noturnas|2495.6564157072617|2495.6564157072617|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=2484.4123923739285|v3_h=2495.6564157072621|delta_h=11.244023333333644|colabs=86
DONO|2026-10|horas_atraso|ESTRUTURA|0|1.1068333333333333|1.1068333333333333|1|1
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|51.558166666666665|51.557801868611108|-0.00036479805555503231|84|1177
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|0|0|0|61|370
PARCIAL|2026-10|horas_atraso|v2_h=51.558166666666665|v3_h=52.664635201944442|autoridade2_h=51.558166666666665|autoridade3_h=52.664635201944442|porta_h=52.664635201944442|emitido_decidido_h=52.664635201944442|fm_dias=51.53|fm_retorno=51.53|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_atraso|51.558166666666665|52.664635201944442|1.1064685352777772|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=47.964166666666664|v3_h=47.963786313055557|delta_h=-0.00038035361110644317|colabs=86
DONO|2026-10|horas_saida_antecipada|ESTRUTURA|0.5571666666666667|0|-0.5571666666666667|1|1
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|81.210833333333341|81.210943157499997|0.00010982416666610595|84|1177
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0|0|0|61|370
PARCIAL|2026-10|horas_saida_antecipada|v2_h=81.768000000000001|v3_h=81.210943157499997|autoridade2_h=81.768000000000001|autoridade3_h=81.210943157499997|porta_h=81.210943157499997|emitido_decidido_h=81.210943157499997|fm_dias=81.77|fm_retorno=81.77|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_saida_antecipada|81.768000000000001|81.210943157499997|-0.55705684250000331|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=78.090166666666661|v3_h=77.533010546666674|delta_h=-0.55715611999998771|colabs=86
UNIVERSO|2026-11|colabs_porta=578
UNIVERSO_PARTE 2026-11 97
MEDIDO|2026-11|col32
MEDIDO|2026-11|col38
MEDIDO|2026-11|col44
MEDIDO|2026-11|col50
MEDIDO|2026-11|col56
MEDIDO|2026-11|col62
MEDIDO|2026-11|col80
MEDIDO|2026-11|col92
MEDIDO|2026-11|col98
MEDIDO|2026-11|col110
MEDIDO|2026-11|col116
RECUSA|2026-11|col128|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=16
EXCLUIDO|2026-11|col128|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col134
MEDIDO|2026-11|col146
MEDIDO|2026-11|col152
MEDIDO|2026-11|col170
MEDIDO|2026-11|col188
MEDIDO|2026-11|col194
MEDIDO|2026-11|col200
MEDIDO|2026-11|col206
MEDIDO|2026-11|col212
MEDIDO|2026-11|col218
MEDIDO|2026-11|col224
RECUSA|2026-11|col236|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=27
EXCLUIDO|2026-11|col236|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col242
MEDIDO|2026-11|col248
MEDIDO|2026-11|col254
MEDIDO|2026-11|col266
MEDIDO|2026-11|col272
MEDIDO|2026-11|col278
MEDIDO|2026-11|col284
MEDIDO|2026-11|col290
MEDIDO|2026-11|col296
MEDIDO|2026-11|col302
MEDIDO|2026-11|col320
MEDIDO|2026-11|col332
RECUSA|2026-11|col344|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=23
EXCLUIDO|2026-11|col344|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col362
MEDIDO|2026-11|col368
MEDIDO|2026-11|col374
MEDIDO|2026-11|col392
MEDIDO|2026-11|col398
MEDIDO|2026-11|col416
MEDIDO|2026-11|col428
RECUSA|2026-11|col440|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=31
EXCLUIDO|2026-11|col440|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col446
DIA|2026-11|horas_extras_100_noturna|col446|2026-11-02|CADASTRO|0|1|1
MEDIDO|2026-11|col452
MEDIDO|2026-11|col464
MEDIDO|2026-11|col476
MEDIDO|2026-11|col482
MEDIDO|2026-11|col512
RECUSA|2026-11|col518|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=1
EXCLUIDO|2026-11|col518|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-11|col566
MEDIDO|2026-11|col572
MEDIDO|2026-11|col584
MEDIDO|2026-11|col608
MEDIDO|2026-11|col626
RECUSA|2026-11|col638|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trab

[truncado em 20000 de 41426 caracteres]

## 06/11 14:26 · sessao 1e74e491b7d2 · H12H06-B-PAUTA-08 · verificador · origem=Code

**Ainda não daria quitação integral:** a detecção de DEVIDO alterado não cobre a convergência com PAGO, e uma falha ao atualizar a reversão depois do `atomic` deixa pautas persistidas fora do arquivo. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133–135,162,198–199,239`)

A cópia não contém metadados Git; a comparação histórica exata com `5855dfc5` ficou **[não verificado]**. A suite Django também ficou **[não verificado]**; as verificações executadas foram em memória, sem criar arquivos **[não verificado]**.

1. **`continue`, variáveis e travas**

   O `continue` sai do contexto e passa à próxima iteração; não alcança o uso posterior de `novos` ou `p`. Todo caminho que chega a essas leituras passa antes pelas respectivas atribuições. Não há reutilização acidental nem variável indefinida nesse ramo. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182–201`)

   `@transaction.atomic` em `escrever` não equivale a travar uma Pauta existente: a porta insere Pauta, registra trilha e atualiza `ContadorPauta`. Portanto, além de Colaborador, há contenção no contador do DP. Não identifiquei caminho inverso concreto que prove um ciclo; ausência global de deadlocks permanece **[não verificado]**. (`app/pautas/services.py:152,180–188,344–358`)

2. **`_diz_existente`**

   `ja.respostas.nao_canceladas()` é compatível com o manager declarado: `respostas` é a relação reversa, `objects` deriva de `PautaQuerySet.as_manager()` e `nao_canceladas` é método público desse queryset. (`app/pautas/models.py:60–64,101–110`)

   O prefixo separa respostas comuns do DP, mas não identifica autoria: se o DP copiar um texto começando por `chave + ' > '`, ele entra na comparação e pode gerar aviso falso. A porta de resposta aceita texto livre. Isso não cria duplicata, pois o ramo existente termina em `continue`. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:187–190,224–230`; `app/pautas/services.py:192–200`)

   A comparação é estável para os mesmos valores e formatação: campos ordenados, packer determinístico, inserção sequencial e leitura por PK. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133,168–170,194–197,224–226`; `app/pautas/services.py:123–149`)

3. **`O_EXCL`, `exists` e reversão**

   A combinação é coerente para impedir duas primeiras gravações no mesmo caminho: `exists` antecipa a recusa; `O_EXCL` fecha a corrida; somente quem reservou o caminho chega ao `tmp + replace`. O `exists` pode permanecer. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:87–89,255–267`)

   **Permanece uma falha de recuperação:** se `_grava` falhar na linha 199, o `atomic` já terminou, mas o JSON anterior não contém os novos PKs. `_reverter` percorre exclusivamente `rev['pautas']`; não recupera pelo plano. A cura dos “PKs mortos” não garante reversão nesse intervalo. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198–199,239–246`)

   Há também um intervalo inicial com arquivo reservado vazio antes do `replace`; nesse ponto nenhuma pauta foi escrita. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:159,182,261–267`)

4. **FORA_DE_PIPELINE e diagrama**

   A entrada satisfaz os predicados de B6.1 e de entrada não podre: existe command correspondente e ele está declarado como ato manual em `FORA_DE_PIPELINE`. Os cálculos reproduzidos deram `sobra=[]` e `podres=[]`; execução dos testes Django **[não verificado]**. (`app/config/crons.py:781–786`; `app/chamados/tests/test_contract_crons.py:214–237`)

   Essa inclusão não altera os blocos do diagrama derivados de crons: eles leem `CRONS`, `ENCADEADOS`, `todas_as_entradas` e os registros de vigias. Os três blocos conferidos coincidiram com o `.mmd`; selo completo B9/MAPA **[não verificado]**. (`app/core/management/commands/gerar_diagrama.py:164–183,233–243,372–408`)

   `manage.py gerar_diagrama` escreve arquivos; a modalidade somente leitura é `--check`. (`app/core/management/commands/gerar_diagrama.py:921–934`)

5. **Descartes anteriores**

   **Não mantenho nenhum como bloqueante**, pelos motivos apresentados:

   - Autor: a criação eventual pertence à porta canônica; o command só a chama no caminho de escrita. (`app/core/utils.py:18`; `app/pautas/services.py:70–90`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175–181`)
   - Hash: `previsto_em` é exclusão expressa da autoridade utilizada. (`app/ponto/portas/regen_exportada.py:35–38,62–74`)
   - K6 fixo: o escopo documentado desta obra é a competência 08. (`app/docs/BACKLOG.md:1779`)
   - Mutante `desligado`: o fixture cobre essa situação; não demonstra outro caso de exclusão do universo. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:54,136–143`)
   - Nome no logger: está no fechamento; o command apresenta colaboradores por PK. (`app/ponto/services/fechamento.py:1287–1291`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:171`)

6. **O que continua sem cura integral**

   **DEVIDO que passa a igualar PAGO:** `_medir` classifica como `sem_movimento`, mas a consulta da pauta existente só acontece no laço de `com_diferenca`. A pauta antiga permanece sem `ja_divergente` ou `ATENCAO`. O teste novo altera **PAGO**, via `FechamentoMensal`, e não cobre essa convergência. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129–135,162,175–190`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:260–270`; `app/ponto/portas/regen_exportada.py:77–84`)

   **Reversão após falha de gravação:** permanece o intervalo descrito no item 3. Os testes novos cobrem queda na filha e preservação da feita, mas não falha ao atualizar o JSON depois do `atomic`. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:228–258`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198–199`)

   Outros itens eventualmente presentes na resposta integral da volta 1, além da triagem fornecida: **[não verificado]**.

## 06/11 14:25 · sessao 963078751906 · - · - · origem=Ronald (tmux)

- **`continue` correto:** o ramo `ja` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:187` termina em `continue` na linha 190; sai do contexto e passa à próxima iteração, sem executar as linhas 198–201. Portanto não reutiliza `novos`/`p` anteriores nem acessa variáveis indefinidas. `novos`/`p` são definidos no único ramo que alcança esse trecho, nas linhas 191–197.

- **A trava nova não é `Colaborador → SELECT FOR UPDATE Pauta`:** o command trava Colaborador na linha 185; `escrever`, embora decorado com `@transaction.atomic`, faz INSERT de Pauta e lavra contador (`app/pautas/services.py:152`, `app/pautas/services.py:180`, `app/pautas/services.py:188`). Pauta usa âncora textual e não FK para Colaborador (`app/pautas/models.py:82`, `app/pautas/models.py:101`).

- **Há também `Colaborador → ContadorPauta(dp)`:** o contador recebe UPDATE em `app/pautas/services.py:354`. Não encontrei caminho inverso concreto que demonstrasse deadlock com essa mudança; ausência global de ciclos permanece **[não verificado]**. As operações canônicas de concluir/cancelar Pauta atualizam Pauta, trilha e contador, sem adquirir explicitamente Colaborador (`app/pautas/services.py:247`, `app/pautas/services.py:272`, `app/pautas/services.py:294`).

- **Residual real da reversão:** após concluir o atomic, o command atualiza primeiro `rev` na memória e só depois o arquivo (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198`). Uma queda/falha na gravação da linha 199 deixa pautas persistentes fora da lista de PKs em disco; `_reverter` consulta exclusivamente essa lista (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:239`). Isso não invalida a cura de “PK morto”, mas permanece como janela de recuperação por arquivo; resistência a queda nesse intervalo **[não verificado]**.

## 06/11 14:24 · sessao 686d0af4b620 · - · - · origem=Ronald (tmux)

Q4: a entrada está correta: o command aparece em `FORA_DE_PIPELINE`, com justificativa de ato único por competência exportada; o B6.1 desconta essa lista dos commands vivos e o teste de entradas podres rejeita apenas entradas sem arquivo vivo ou já agendadas. (`app/config/crons.py:781`; `app/chamados/tests/test_contract_crons.py:214`; `app/chamados/tests/test_contract_crons.py:225`; `app/chamados/tests/test_contract_crons.py:230`)

Executei, somente em memória e com bytecode desativado, os mesmos cálculos: `sobra=[]`, `podres=[]`. A execução da suite Django está **[não verificado]**, pois Django não está instalado. [nao verificado]

A inclusão literal em `FORA_DE_PIPELINE` não altera as fontes que alimentam o desenho: pipeline usa `CRONS`/`ENCADEADOS`; tabuleiro usa `todas_as_entradas`; vigias usa `CRONS`, `AGENDADOS_FORA_DO_CRON` e comentários `contador=`. (`app/core/management/commands/gerar_diagrama.py:164`; `app/core/management/commands/gerar_diagrama.py:233`; `app/core/management/commands/gerar_diagrama.py:372`; `app/core/management/commands/gerar_diagrama.py:396`)

Também executei as três funções reais do gerador, carregando seu AST somente em memória e excluindo apenas o import Django e a classe `Command`: os três blocos gerados são idênticos aos presentes em `ARQUITETURA.mmd`. Execução completa do selo B9/MAPA: **[não verificado]**. O selo compara ambos os arquivos, portanto conferir apenas os blocos de crons não substitui B9 inteiro. (`app/core/tests/test_selo_diagrama_do_codigo.py:44`; `app/core/management/commands/gerar_diagrama.py:921`)

Atenção operacional: `manage.py gerar_diagrama` escreve arquivos; apenas `--check` conserva somente leitura. Não executei a modalidade que escreve. (`app/core/management/commands/gerar_diagrama.py:921`; `app/core/management/commands/gerar_diagrama.py:931`; `app/core/management/commands/gerar_diagrama.py:950`)

## 06/11 14:24 · sessao cb76d66c443b · - · - · origem=Ronald (tmux)

Pergunta 2:

- `ja.respostas.nao_canceladas()` é compatível com a modelagem: o FK reverso chama-se `respostas`, o manager padrão vem de `PautaQuerySet.as_manager()` e `nao_canceladas()` devolve um queryset filtrado. Não identifiquei defeito nessa chamada por inspeção; execução com Django não verificada. (`app/pautas/models.py:60-64`, `app/pautas/models.py:101-110`; [não verificado] quanto à execução.)

- A comparação é determinística para os mesmos dados: os campos são ordenados; os itens alimentam um packer que preserva ordem; as filhas são escritas sequencialmente e recuperadas por `pk`. O prefixo continua presente nas partes com `(cont.)`. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:168-170`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:194-197`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224-226`, `app/pautas/services.py:123-149`.)

- O prefixo separa respostas comuns do DP, mas **não identifica autoria**: uma resposta do DP que copie o início `chave + ' > '` entra em `velhas`, pois a porta aceita texto livre. Isso pode gerar aviso falso de divergência; não produz duplicata porque o ramo existente sempre retorna ao laço. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:187-190`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224-230`, `app/pautas/services.py:192-200`, `app/pautas/views.py:112-114`.)

- **Lacuna concreta da cura de DEVIDO:** se o cálculo de hoje passar a coincidir com PAGO, a pessoa cai em `sem_movimento`; como só `com_diferenca` percorre a deduplicação, a pauta antiga permanece sem `ja_divergente`/`ATENCAO`. A cura cobre mudanças que mantenham alguma diferença. (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129-135`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:162`, `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175-190`.)

- O teste denominado “devido que mudou” altera `FechamentoMensal.horas_reflexo_dsr`, isto é, PAGO; não demonstra alteração de DEVIDO nem o caso de convergência acima. (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:260-270`, `app/ponto/portas/regen_exportada.py:77-84`.)

## 06/11 14:21 · sessao f13f3e92b001 · V3-IMPACTO-COM-DONO · sonda · origem=Code

Todas as linhas `SOMA` da parte fornecida apresentam `True`. Não há ocorrência de `INCOMPLETO` no trecho fornecido. [nao verificado]

Transcrição integral do dado fornecido, sem verificação em arquivo. [nao verificado]

```text
sonda_leitura: script.py [4d10723901fb0ca1] no cpuset 4-7, lendo sombra como leitor (LEITURA)

PARTE|2|6
FINALIDADE|IMPACTO_NUNCA_VALIDACAO
BASE_SOMAS|COLABORADORES_MEDIDOS
COBERTURA_SOMA|MEDIDOS_MAIS_EXCLUIDOS_IGUAL_UNIVERSO_PARTE
ATO|hoje=2026-11-06|agora=2026-11-06T14:12:03.058619-03:00
FORA_DA_MEDIDA|horas_falta
FORA_DA_MEDIDA|horas_reflexo_dsr
FORA_DA_MEDIDA|saldo_banco_horas
UNIVERSO|2026-10|colabs_porta=624
UNIVERSO_PARTE 2026-10 109
MEDIDO|2026-10|col37
MEDIDO|2026-10|col43
MEDIDO|2026-10|col49
MEDIDO|2026-10|col55
MEDIDO|2026-10|col61
MEDIDO|2026-10|col67
MEDIDO|2026-10|col79
MEDIDO|2026-10|col85
HISTORICO_T|2026-10|col91|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col91|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col97
MEDIDO|2026-10|col109
MEDIDO|2026-10|col115
MEDIDO|2026-10|col121
MEDIDO|2026-10|col127
MEDIDO|2026-10|col133
MEDIDO|2026-10|col139
MEDIDO|2026-10|col145
MEDIDO|2026-10|col151
MEDIDO|2026-10|col157
MEDIDO|2026-10|col163
MEDIDO|2026-10|col169
RECUSA|2026-10|col181|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=22
EXCLUIDO|2026-10|col181|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col193|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=29
EXCLUIDO|2026-10|col193|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col199|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col199|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col205
DIA|2026-10|horas_atraso|col205|2026-09-21|CADASTRO|0.17816666666666664|1.1781666666666666|1
MEDIDO|2026-10|col211
MEDIDO|2026-10|col217
MEDIDO|2026-10|col235
MEDIDO|2026-10|col241
MEDIDO|2026-10|col247
MEDIDO|2026-10|col253
MEDIDO|2026-10|col259
DIA|2026-10|horas_atraso|col259|2026-10-14|ESTRUTURA|0|1.1696558333333336|1.1696558333333336
DIA|2026-10|horas_atraso|col259|2026-10-15|ESTRUTURA|0|1.585|1.585
MEDIDO|2026-10|col265
RECUSA|2026-10|col271|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col271|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col277
MEDIDO|2026-10|col283
MEDIDO|2026-10|col289
EXCLUIDO|2026-10|col301|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
MEDIDO|2026-10|col307
MEDIDO|2026-10|col325
MEDIDO|2026-10|col331
MEDIDO|2026-10|col343
MEDIDO|2026-10|col349
MEDIDO|2026-10|col355
RECUSA|2026-10|col367|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=5
EXCLUIDO|2026-10|col367|PRODUTO:RECUSA_PORTA_SEM_DONO
EXCLUIDO|2026-10|col373|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
RECUSA|2026-10|col379|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=29
EXCLUIDO|2026-10|col379|PRODUTO:RECUSA_PORTA_SEM_DONO
HISTORICO_T|2026-10|col385|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col385|PRODUTO:SEM_RETORNO_FECHAMENTO
RECUSA|2026-10|col391|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col391|PRODUTO:RECUSA_PORTA_SEM_DONO
HISTORICO_T|2026-10|col403|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col403|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col415
RECUSA|2026-10|col427|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=15
EXCLUIDO|2026-10|col427|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col439
EXCLUIDO|2026-10|col451|PRODUTO:MOTOR_FECHAMENTO_horas_trabalhadas
MEDIDO|2026-10|col457
MEDIDO|2026-10|col469
DIA|2026-10|horas_extras_100_noturna|col469|2026-10-12|CADASTRO|0|1|1
MEDIDO|2026-10|col475
MEDIDO|2026-10|col493
RECUSA|2026-10|col499|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=8
EXCLUIDO|2026-10|col499|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col511
MEDIDO|2026-10|col523
MEDIDO|2026-10|col529
HISTORICO_T|2026-10|col559|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col559|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col577
DIA|2026-10|horas_trabalhadas|col577|2026-10-15|CADASTRO|7.290805555555556|4.4485002777777778|-2.8423052777777782
MEDIDO|2026-10|col583
MEDIDO|2026-10|col601
RECUSA|2026-10|col607|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=29
EXCLUIDO|2026-10|col607|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col625
MEDIDO|2026-10|col631
RECUSA|2026-10|col643|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=10
EXCLUIDO|2026-10|col643|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col649|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col649|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col655
DIA|2026-10|horas_atraso|col655|2026-09-30|ESTRUTURA|0|0.8240599999999999|0.8240599999999999
RECUSA|2026-10|col667|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=30
EXCLUIDO|2026-10|col667|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col673
MEDIDO|2026-10|col697
MEDIDO|2026-10|col709
MEDIDO|2026-10|col727
DIA|2026-10|horas_extras_100|col727|2026-10-12|ESTRUTURA|0|10.985971388888888|10.985971388888888
DIA|2026-10|horas_extras_100_feriado|col727|2026-10-12|ESTRUTURA|0|10.985971388888888|10.985971388888888
DIA|2026-10|horas_noturnas|col727|2026-10-12|ESTRUTURA|5.9873025000000002|6.842631428571428|0.85532892857142784
DIA|2026-10|horas_noturnas|col727|2026-10-14|ESTRUTURA|6.0032244444444443|6.8608279365079357|0.8576034920634914
DIA|2026-10|horas_noturnas|col727|2026-10-16|ESTRUTURA|6.0689425000000004|6.9359342857142847|0.86699178571428437
MEDIDO|2026-10|col739
MEDIDO|2026-10|col751
MEDIDO|2026-10|col769
MEDIDO|2026-10|col787
MEDIDO|2026-10|col823
MEDIDO|2026-10|col829
MEDIDO|2026-10|col835
MEDIDO|2026-10|col841
MEDIDO|2026-10|col859
DIA|2026-10|horas_noturnas|col859|2026-09-21|ESTRUTURA|6.8570641269841266|5.9999311111111107|-0.85713301587301594
DIA|2026-10|horas_noturnas|col859|2026-09-22|ESTRUTURA|6.8580000000000005|6.0007499999999991|-0.8572500000000014
DIA|2026-10|horas_noturnas|col859|2026-09-24|ESTRUTURA|6.8443688888888889|5.9888227777777781|-0.8555461111111109
DIA|2026-10|horas_noturnas|col859|2026-09-25|ESTRUTURA|6.8567358730158734|5.9996438888888886|-0.85709198412698484
DIA|2026-10|horas_noturnas|col859|2026-09-26|ESTRUTURA|6.8677317460317466|6.0092652777777777|-0.85846646825396888
DIA|2026-10|horas_noturnas|col859|2026-09-28|ESTRUTURA|6.854342222222221|5.9975494444444442|-0.85679277777777685
MEDIDO|2026-10|col865
DIA|2026-10|horas_extras_100_noturna|col865|2026-10-12|CADASTRO|0|5.1646777861111115|5.1646777861111115
MEDIDO|2026-10|col871
MEDIDO|2026-10|col877
RECUSA|2026-10|col883|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=16
EXCLUIDO|2026-10|col883|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col889
HISTORICO_T|2026-10|col901|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col901|PRODUTO:SEM_RETORNO_FECHAMENTO
MEDIDO|2026-10|col907
MEDIDO|2026-10|col913
MEDIDO|2026-10|col919
MEDIDO|2026-10|col925
MEDIDO|2026-10|col931
HISTORICO_T|2026-10|col937|sem_resultado_do_fechamento|1
EXCLUIDO|2026-10|col937|PRODUTO:SEM_RETORNO_FECHAMENTO
RECUSA|2026-10|col943|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=4
EXCLUIDO|2026-10|col943|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col949
RECUSA|2026-10|col955|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col955|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col961
MEDIDO|2026-10|col967
MEDIDO|2026-10|col973
RECUSA|2026-10|col979|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=7
EXCLUIDO|2026-10|col979|PRODUTO:RECUSA_PORTA_SEM_DONO
MEDIDO|2026-10|col985
MEDIDO|2026-10|col991
RECUSA|2026-10|col997|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=1
EXCLUIDO|2026-10|col997|PRODUTO:RECUSA_PORTA_SEM_DONO
RECUSA|2026-10|col1003|oraculo|campos=horas_atraso,horas_extras_100,horas_extras_100_feriado,horas_extras_100_noturna,horas_extras_50,horas_extras_50_noturna,horas_folga_trabalhada,horas_intra_indenizada,horas_noturnas,horas_saida_antecipada,horas_trabalhadas|dias_tentados=0
EXCLUIDO|2026-10|col1003|PRODUTO:RECUSA_PORTA_SEM_DONO
COBERTURA_COLAB|2026-10|medidos=81|universo_parte=109|excluidos=28
COBERTURA_INTEGRAL|2026-10|False
PRODUTO|2026-10|DIA_FORA_UNIVERSO_PORTA|51|247
PRODUTO|2026-10|MOTOR_FECHAMENTO_horas_trabalhadas|3|54
PRODUTO|2026-10|RECUSA_PORTA_SEM_DONO|19|295
PRODUTO|2026-10|SEM_DECISAO_horas_atraso|55|271
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100|53|259
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_feriado|53|259
PRODUTO|2026-10|SEM_DECISAO_horas_extras_100_noturna|53|260
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50|53|260
PRODUTO|2026-10|SEM_DECISAO_horas_extras_50_noturna|53|260
PRODUTO|2026-10|SEM_DECISAO_horas_folga_trabalhada|51|247
PRODUTO|2026-10|SEM_DECISAO_horas_intra_indenizada|51|247
PRODUTO|2026-10|SEM_DECISAO_horas_noturnas|52|254
PRODUTO|2026-10|SEM_DECISAO_horas_saida_antecipada|55|271
PRODUTO|2026-10|SEM_DECISAO_horas_trabalhadas|51|247
PRODUTO|2026-10|SEM_RETORNO_FECHAMENTO|6|0
DIAS_NAO_LOCALIZADOS|2026-10|PRODUTO|SEM_RETORNO_FECHAMENTO|colabs=6
DONO|2026-10|horas_trabalhadas|CADASTRO|7.290805555555556|4.4485002777777778|-2.8423052777777782|1|1
RESIDUO|2026-10|horas_trabalhadas|ATE_LIMIAR|11300.858365358055|11300.858365358055|-3.730349362740526e-14|81|1254
RESIDUO|2026-10|horas_trabalhadas|SEM_DECISAO|80.640867326666665|80.640867326666665|0|51|247
PARCIAL|2026-10|horas_trabalhadas|v2_h=11308.149170913612|v3_h=11305.306865635834|autoridade2_h=11388.790038240279|autoridade3_h=11385.947732962501|porta_h=11305.306865635834|emitido_decidido_h=11305.306865635834|fm_dias=11388.82|fm_retorno=11388.82|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_trabalhadas|11308.149170913612|11305.306865635834|-2.8423052777779958|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_trabalhadas|v2_h=11352.388704578334|v3_h=11305.306865635834|delta_h=-47.081838942500326|colabs=81
RESIDUO|2026-10|horas_intra_indenizada|ATE_LIMIAR|316.36347211083336|316.36347211083336|0|81|1255
RESIDUO|2026-10|horas_intra_indenizada|SEM_DECISAO|0|0|0|51|247
PARCIAL|2026-10|horas_intra_indenizada|v2_h=316.36347211083336|v3_h=316.36347211083336|autoridade2_h=316.36347211083336|autoridade3_h=316.36347211083336|porta_h=316.36347211083336|emitido_decidido_h=316.36347211083336|fm_dias=316.34|fm_retorno=316.34|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_intra_indenizada|316.36347211083336|316.36347211083336|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_intra_indenizada|v2_h=316.36347211083336|v3_h=316.36347211083336|delta_h=0|colabs=81
RESIDUO|2026-10|horas_extras_50|ATE_LIMIAR|0.17511789305555586|0.17511789305555586|0|81|1242
RESIDUO|2026-10|horas_extras_50|SEM_DECISAO|0|0|0|53|260
PARCIAL|2026-10|horas_extras_50|v2_h=0.17511789305555586|v3_h=0.17511789305555586|autoridade2_h=0.17511789305555586|autoridade3_h=0.17511789305555586|porta_h=0.17511789305555586|emitido_decidido_h=0.17511789305555586|fm_dias=0.18|fm_retorno=0.18|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50|0.17511789305555586|0.17511789305555586|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50|v2_h=0.17511789305555586|v3_h=0.17511789305555586|delta_h=0|colabs=81
DONO|2026-10|horas_extras_100|ESTRUTURA|0|10.985971388888888|10.985971388888888|1|1
RESIDUO|2026-10|horas_extras_100|ATE_LIMIAR|83.48202486444444|83.48202486444444|1.7763568394002505e-15|81|1242
RESIDUO|2026-10|horas_extras_100|SEM_DECISAO|0|0|0|53|259
PARCIAL|2026-10|horas_extras_100|v2_h=83.48202486444444|v3_h=94.467996253333339|autoridade2_h=83.48202486444444|autoridade3_h=94.467996253333339|porta_h=94.467996253333339|emitido_decidido_h=94.467996253333339|fm_dias=83.48|fm_retorno=83.48|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100|83.48202486444444|94.467996253333339|10.985971388888899|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100|v2_h=79.54408042|v3_h=94.467996253333339|delta_h=14.923915833333339|colabs=81
DONO|2026-10|horas_extras_100_feriado|ESTRUTURA|0|10.985971388888888|10.985971388888888|1|1
RESIDUO|2026-10|horas_extras_100_feriado|ATE_LIMIAR|83.48202486444444|83.48202486444444|1.7763568394002505e-15|81|1242
RESIDUO|2026-10|horas_extras_100_feriado|SEM_DECISAO|0|0|0|53|259
PARCIAL|2026-10|horas_extras_100_feriado|v2_h=83.48202486444444|v3_h=94.467996253333339|autoridade2_h=83.48202486444444|autoridade3_h=94.467996253333339|porta_h=94.467996253333339|emitido_decidido_h=94.467996253333339|fm_dias=83.48|fm_retorno=83.48|colabs_div=1|dias_colab_div=1
RUBRICA|2026-10|horas_extras_100_feriado|83.48202486444444|94.467996253333339|10.985971388888899|1|1
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_feriado|v2_h=0|v3_h=94.467996253333339|delta_h=94.467996253333339|colabs=81
RESIDUO|2026-10|horas_extras_50_noturna|ATE_LIMIAR|0|0|0|81|1242
RESIDUO|2026-10|horas_extras_50_noturna|SEM_DECISAO|0|0|0|53|260
PARCIAL|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_extras_50_noturna|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_extras_50_noturna|v2_h=0|v3_h=0|delta_h=0|colabs=81
DONO|2026-10|horas_extras_100_noturna|CADASTRO|0|6.1646777861111115|6.1646777861111115|2|2
RESIDUO|2026-10|horas_extras_100_noturna|ATE_LIMIAR|11.916441291944444|11.916441291944444|0|81|1240
RESIDUO|2026-10|horas_extras_100_noturna|SEM_DECISAO|0|0|0|53|260
PARCIAL|2026-10|horas_extras_100_noturna|v2_h=11.916441291944444|v3_h=18.081119078055554|autoridade2_h=11.916441291944444|autoridade3_h=18.081119078055554|porta_h=18.081119078055554|emitido_decidido_h=18.081119078055554|fm_dias=11.92|fm_retorno=11.92|colabs_div=2|dias_colab_div=2
RUBRICA|2026-10|horas_extras_100_noturna|11.916441291944444|18.081119078055554|6.1646777861111097|2|2
INSTRUMENTO_HISTORICO|2026-10|horas_extras_100_noturna|v2_h=0|v3_h=18.081119078055554|delta_h=18.081119078055554|colabs=81
RESIDUO|2026-10|horas_folga_trabalhada|ATE_LIMIAR|0|0|0|81|1255
RESIDUO|2026-10|horas_folga_trabalhada|SEM_DECISAO|0|0|0|51|247
PARCIAL|2026-10|horas_folga_trabalhada|v2_h=0|v3_h=0|autoridade2_h=0|autoridade3_h=0|porta_h=0|emitido_decidido_h=0|fm_dias=0.0|fm_retorno=0.0|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_folga_trabalhada|0|0|0|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_folga_trabalhada|v2_h=40.339278106388889|v3_h=0|delta_h=-40.339278106388889|colabs=81
DONO|2026-10|horas_noturnas|ESTRUTURA|59.197712301587302|56.635356150793648|-2.5623561507936552|2|9
RESIDUO|2026-10|horas_noturnas|ATE_LIMIAR|3086.9939509036903|3086.9939509036903|4.4408920985006262e-15|81|1239
RESIDUO|2026-10|horas_noturnas|SEM_DECISAO|0|0|0|52|254
PARCIAL|2026-10|horas_noturnas|v2_h=3146.1916632052776|v3_h=3143.6293070544839|autoridade2_h=3146.1916632052776|autoridade3_h=3143.6293070544839|porta_h=3143.6293070544839|emitido_decidido_h=3143.6293070544839|fm_dias=3146.19|fm_retorno=3146.19|colabs_div=2|dias_colab_div=9
RUBRICA|2026-10|horas_noturnas|3146.1916632052776|3143.6293070544839|-2.5623561507936756|2|9
INSTRUMENTO_HISTORICO|2026-10|horas_noturnas|v2_h=3140.1567917438888|v3_h=3143.6293070544839|delta_h=3.4725153105950994|colabs=81
DONO|2026-10|horas_atraso|CADASTRO|0.17816666666666664|1.1781666666666666|1|1|1
DONO|2026-10|horas_atraso|ESTRUTURA|0|3.5787158333333333|3.5787158333333333|2|3
RESIDUO|2026-10|horas_atraso|ATE_LIMIAR|58.789333333333332|58.789851228611113|0.00051789527777812427|79|1227
RESIDUO|2026-10|horas_atraso|SEM_DECISAO|10.822000000000001|10.822000000000001|0|55|271
PARCIAL|2026-10|horas_atraso|v2_h=58.967500000000001|v3_h=63.546733728611109|autoridade2_h=69.789500000000004|autoridade3_h=74.368733728611105|porta_h=63.546733728611109|emitido_decidido_h=63.546733728611109|fm_dias=69.81|fm_retorno=69.81|colabs_div=3|dias_colab_div=4
RUBRICA|2026-10|horas_atraso|58.967500000000001|63.546733728611109|4.5792337286111078|3|4
INSTRUMENTO_HISTORICO|2026-10|horas_atraso|v2_h=68.168499999999995|v3_h=57.347020673055553|delta_h=-10.821479326944441|colabs=81
RESIDUO|2026-10|horas_saida_antecipada|ATE_LIMIAR|51.828333333333333|51.82854559194444|0.00021225861111040523|79|1231
RESIDUO|2026-10|horas_saida_antecipada|SEM_DECISAO|0|0|0|55|271
PARCIAL|2026-10|horas_saida_antecipada|v2_h=51.828333333333333|v3_h=51.82854559194444|autoridade2_h=51.828333333333333|autoridade3_h=51.82854559194444|porta_h=51.82854559194444|emitido_decidido_h=51.82854559194444|fm_dias=51.84|fm_retorno=51.84|colabs_div=0|dias_colab_div=0
RUBRICA|2026-10|horas_saida_antecipada|51.828333333333333|51.82854559194444|0.00021225861110707456|0|0
INSTRUMENTO_HISTORICO|2026-10|horas_saida_antecipada|v2_h=41.274166666666666|v3_h=41.274357500000001|delta_h=0.00019083333333469454|colabs=81
UN

[truncado em 20000 de 45239 caracteres]

## 06/11 14:21 · sessao d26eb9b3567a · R1-PUBLICA · verificador · origem=Code

**O delta resolve os achados 3 e 7; resolve parcialmente o 1 e o 8.** Os números conferem aritmeticamente com o anexo, mas a disjunção por `pk % 6` continua **[nao verificado]**. O recorte ainda elimina partes das provas. (`app/core/placar_estrutural.py:108`, `:118`; `app/core/tests/test_r1_termometro_uma_medicao.py:6`; `bin/gerar_estado.py:164`.)

**1. Situação dos achados da v2**

| Achado | Veredito sobre o delta | Evidência |
|---|---|---|
| **1 — truncamento** | **Parcialmente resolvido.** Agora aparecem os totais e ressalvas principais; continuam ausentes os caminhos completos, algumas chaves e detalhamentos. | Seis recortes abaixo; `bin/gerar_estado.py:164`. |
| **3 — atribuição da SOMA** | **Resolvido no texto.** `fonte` agora distingue impressão pelo comando de conferência pela sonda. O comando realmente imprime os totais lado a lado, sem comparação booleana. A implementação da sonda permanece **[nao verificado]**. | `app/core/placar_estrutural.py:108`, `:109`; `app/ponto/management/commands/e6_oraculo.py:415`, `:424`, `:426`. |
| **7 — docstring** | **Resolvido o resíduo.** A descrição “menciona … em qualquer posição” agora corresponde ao censo implementado. A nota também preserva a distinção entre declaração e cadência calculada. | `app/core/tests/test_r1_termometro_uma_medicao.py:6`, `:46`; `app/core/placar_estrutural.py:119`. |
| **8 — qualificações** | **Parcialmente resolvido.** A prova agora explicita, inclusive no recorte, que o percentual inclui ímpares. Entretanto, `o_que` continua formulado para batidas completas: a medida geral não demonstra o percentual exclusivo desse subconjunto. A nova explicação de partição ainda depende de código indisponível. | `app/core/espelho_verdade.py:168`, `:169`; `app/core/placar_estrutural.py:118`. |

Simulei em memória exatamente `prova[:150]`, sem acrescentar reticências. Cada célula abaixo contém os **150 caracteres efetivamente publicados** pela expressão de `bin/gerar_estado.py:164`:

| Item e origem | Publicação |
|---|---|
| E1-sem-previsao — `app/core/espelho_verdade.py:49` | ``TERMOMETRO R1 06/11: espelho 0 com trabalho no oraculo = 35 (10/2026) e 28 (11/2026, aberta); o previsto_cego nao entrou nesta medida. Chave `esp_zero`` |
| E3-turno-aberto — `app/core/espelho_verdade.py:82` | ``TERMOMETRO R1 06/11: dia impar comparado 347 (10/2026) e 262 (11/2026, aberta); dos impares que DIVERGEM, 31 e 25 fora da faixa piso..teto. Chaves `di`` |
| E6-sistema-bate — `app/core/espelho_verdade.py:169` | `TERMOMETRO R1 06/11: todo dia comparado, impar incluso, bate 95,95% (7180/7483) na 10 e 94,82% (3680/3881) na 11; ESTRUTURA 26/88,36h/16 e 9/40,62h/7.` |
| E6-exportavel — `app/core/espelho_verdade.py:173` | `TERMOMETRO R1 06/11: colabs divergentes (uniao) 144 na 10 = E16/C32/B113 e 105 na 11 = E7/C33/B78; a entrada efetiva na lista do admin nao e medida. E` |
| E6-batida-impar — `app/core/espelho_verdade.py:177` | `TERMOMETRO R1 06/11: impares comparados 347 = 148 batem + 199 divergem (10) e 262 = 123 + 139 (11); dias excluidos antes da comparacao nao medidos. Do` |
| E6-piso-185 — `app/core/espelho_verdade.py:179` | `TERMOMETRO R1 06/11: donos fecham o total -- 26+78+199 = 303 de 303 (10) e 9+72+120 = 201 de 201 (11); causa dos 26+9 de ESTRUTURA nao nomeada aqui (m` |

Assim, sobrevivem as ressalvas sobre `previsto_cego`, entrada efetiva no admin, excluídos e causa não nomeada. Continuam cortados **todos os caminhos da medição**, o critério “até 10 min” de E6 e a decomposição 198+1/137+2. (`app/core/espelho_verdade.py:49`, `:169`, `:173`, `:177`, `:179`; `bin/gerar_estado.py:164`.)

**2. Números e respaldo das novas afirmações**

Não encontrei número novo incompatível com o anexo fornecido. Recalculei as somas dos parciais e comparei com os campos publicados: [nao verificado]

| Conferência do anexo | 10/2026 | 11/2026 | Texto conferido |
|---|---:|---:|---|
| Universo | 624 | 578 | `app/core/placar_estrutural.py:118` |
| BATE/comparados | 7180/7483 = 95,95% | 3680/3881 = 94,82% | `app/core/espelho_verdade.py:169` |
| Dias E/C/B; total | 26/78/199; 303 | 9/72/120; 201 | `app/core/espelho_verdade.py:179` |
| Horas E/C/B; total | 88,36/306,53/961,16; 1356,05 | 40,62/347,49/643,06; 1031,17 | `app/core/placar_estrutural.py:118` |
| Colabs E/C/B; soma dos totais parciais | 16/32/113; 144 | 7/33/78; 105 | `app/core/espelho_verdade.py:173` |
| Espelho zero com trabalho | 35 | 28 | `app/core/espelho_verdade.py:49` |
| Ímpares: comparados = batem + divergem | 347 = 148+199 | 262 = 123+139 | `app/core/espelho_verdade.py:177` |
| Divergentes ímpares: contagem do dia/corte; fora da faixa | 198/1; 31 | 137/2; 25 | `app/core/espelho_verdade.py:177`, `:82` |

**A pendência é probatória:** escrever o filtro dentro de `R1.numero` não demonstra que ele foi executado. O arquivo citado, `logs/r1remede/p1/script.py`, não está disponível nesta cópia nem no anexo; a aplicação de `pk % 6` às seis partes permanece **[nao verificado]**. O comando disponível percorre colaboradores sem esse particionamento. (`app/core/placar_estrutural.py:118`; `app/ponto/management/commands/e6_oraculo.py:302`.)

Se cada parte retiver um dos seis resíduos distintos, a disjunção decorre da regra matemática. Falta verificar essa implementação, os parâmetros das seis execuções e a correspondência com a medida anexada: **[nao verificado]**. Portanto, **144/105 conferem como somas dos totais parciais**; sua interpretação como união global sem duplicação continua **[nao verificado]**. O código demonstra conjuntos e união dentro de uma execução, não entre seis sondas. (`app/ponto/management/commands/e6_oraculo.py:369`, `:429`; `app/core/placar_estrutural.py:118`.)

As demais reformulações têm respaldo: ímpares entram no denominador, horas acumulam diferenças absolutas e a destinação ESTRUTURA/admin corresponde à política declarada. Entrada efetiva no admin permanece **[nao verificado]**, como agora ressalvado. (`app/ponto/management/commands/e6_oraculo.py:347`, `:352`, `:353`, `:368`, `:418`; `app/docs/LEIS.md:122`; `app/core/espelho_verdade.py:173`.)

**3. Quem escreve e quem lê**

“Escritor” abaixo significa a declaração que fornece o campo; o comando de medição imprime resultados, enquanto esses valores estão escritos nos módulos. (`app/ponto/management/commands/e6_oraculo.py:395`, `:422`; declarações abaixo.)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `SOMA.fonte` | `app/core/placar_estrutural.py:108`, `:109` | `placar()` preserva o campo por cópia em `app/core/placar_estrutural.py:635`; selo genérico examina strings em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`. O publicador apenas menciona `fonte` no rodapé, sem imprimir o valor individual: `bin/gerar_estado.py:121`. |
| `R1.numero` | `app/core/placar_estrutural.py:118` | Presença usada na validação em `app/core/placar_estrutural.py:637`; renderização integral em `bin/gerar_estado.py:116`; selo genérico em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:67`. |
| Seis `ETAPAS[*].prova` | `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179` | Validação de presença e cópia em `app/core/espelho_verdade.py:201`, `:203`; publicação em `bin/gerar_estado.py:164`; censo e validação de caminhos em `app/core/tests/test_r1_termometro_uma_medicao.py:46`, `:64`, `:69`. |

**Nenhuma quebra de execução encontrada nesses leitores.** Os dois renderizadores funcionaram em memória, com data e ausência do log E6 simuladas; `R1.numero` apareceu integralmente e as seis provas apareceram com os recortes acima. Continua a perda de conteúdo pelo limite de publicação. (`bin/gerar_estado.py:80`, `:116`, `:145`, `:164`.)

Os cinco testes do selo passaram em memória, substituindo somente `SimpleTestCase` por `unittest.TestCase`; eles examinam as provas integrais, sem verificar o resultado truncado. Execução Django canônica e selos host: **[nao verificado]**. (`app/core/tests/test_r1_termometro_uma_medicao.py:49`, `:64`, `:69`; `bin/gerar_estado.py:164`.)

**4. Limite da data: custo, risco e responsabilidade**

O limite permanece: `_medido_em` consulta a glob e devolve a maior data extraída dos nomes, sem consultar provas. Com candidatos simulados de 06/11 e 07/11, devolveu **07/11**, embora as provas continuassem citando 06/11. Existência real desse candidato posterior: **[nao verificado]**. (`bin/gerar_estado.py:47`, `:51`, `:56`; `app/core/placar_estrutural.py:120`.)

Minha avaliação é de **baixo custo para um selo determinístico**: chamar a função real com glob simulada e cobrir coincidência, candidato posterior, dois arquivos diferentes no mesmo dia e ausência de fonte. Deve comparar identidade do arquivo, além da data, acompanhando o contrato atual de uma única medição. Acrescentar somente o teste exporia o defeito; não corrigiria a seleção. (`app/core/tests/test_r1_termometro_uma_medicao.py:67`, `:70`; `bin/gerar_estado.py:56`.)

A cura exige decidir uma política no publicador: vincular a data ao arquivo efetivamente citado ou denunciar explicitamente a divergência. Os riscos inferidos são esconder uma medição posterior ao simplesmente fixar a prova, tornar o selo dependente dos arquivos locais, ou derrubar todo o bloco para “indisponível” ao lançar uma exceção genérica. (`bin/gerar_estado.py:51`, `:56`, `:86`.)

**A responsabilidade técnica pertence ao publicador**, com teste de integração R1: ele escolhe a fonte e fornece a data; o placar recebe esses dados e permanece puro. Isso não exige repetir a medição. A separação formal em outra obra é **[nao verificado]**; a separação de responsabilidades está explícita no código. (`bin/gerar_estado.py:34`, `:80`; `app/core/placar_estrutural.py:618`, `:623`.)

## 06/11 14:19 · sessao 6af08ca7b704 · - · - · origem=Ronald (tmux)

O limite permanece: `_medido_em` lê somente `fonte_do_medido`, enumera a glob e devolve a maior data, sem consultar `prova`. `bin/gerar_estado.py:47`, `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`.

O selo atual valida correspondência com a glob e um único arquivo citado entre leitores; não chama `_medido_em`. `app/core/tests/test_r1_termometro_uma_medicao.py:33`, `app/core/tests/test_r1_termometro_uma_medicao.py:50`, `app/core/tests/test_r1_termometro_uma_medicao.py:67`.

Reproduzi em memória, sem arquivos nem bytecode: os cinco testes passam; com glob simulada contendo arquivos de 06/11 e 07/11, a função real retorna `{'R1': '2026-11-07'}`, mantendo provas em 06/11. Bases da reprodução: `app/core/tests/test_r1_termometro_uma_medicao.py:49`, `app/core/placar_estrutural.py:120`, `bin/gerar_estado.py:56`. Existência real do arquivo de 07/11: [nao verificado].

Proposta de selo: chamar o publicador real com glob simulada e provas sintéticas, cobrindo arquivo único coincidente, candidato posterior sem atualização da prova, dois arquivos da mesma data e ausência de arquivo datado. Comparar identidade de arquivo, não apenas data: o contrato atual exige uma única medição, não somente datas iguais. `app/core/tests/test_r1_termometro_uma_medicao.py:11`, `app/core/tests/test_r1_termometro_uma_medicao.py:70`.

Só acrescentar esse teste deixaria o caso posterior vermelho; fechar o limite exige mudar a seleção ou publicar explicitamente a divergência. A função atual escolhe o máximo e seu chamador usa esse resultado para data, cadência e veredito. `bin/gerar_estado.py:56`, `bin/gerar_estado.py:80`, `bin/gerar_estado.py:81`, `bin/gerar_estado.py:84`, `bin/gerar_estado.py:116`.

Custo estimado: baixo para caracterizar com teste isolado; maior para escolher e implementar a política de publicação e verificar seus consumidores. Prazo preciso: [nao verificado]. Riscos inferidos: um selo que consulte arquivos reais pode reprovar a cópia sem logs; simplesmente usar a prova pode esconder medição mais recente; lançar exceção indiscriminada pode substituir todo o placar por “indisponível”. Bases: `bin/gerar_estado.py:51`, `bin/gerar_estado.py:56`, `bin/gerar_estado.py:86`.

Minha recomendação é tratar a cura como extensão do **publicador**, acompanhada de teste de integração R1: a autoridade de I/O é explicitamente do publicador, enquanto o placar recebe data e relógio pelo chamador e permanece puro. `bin/gerar_estado.py:34`, `app/core/placar_estrutural.py:618`, `app/core/placar_estrutural.py:623`. A divisão formal da obra em outro documento: [nao verificado].

## 06/11 14:19 · sessao 9881bdce649f · - · - · origem=Ronald (tmux)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `SOMA.fonte` | Declaração em `app/core/placar_estrutural.py:108-109` | Copiado por `placar()` em `app/core/placar_estrutural.py:634-643`; selo genérico varre campos string em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65-75`. O publicador só menciona o campo no rodapé, sem renderizar seu valor: `bin/gerar_estado.py:121-123`. |
| `R1.numero` | Declaração em `app/core/placar_estrutural.py:118` | Presença usada no estado efetivo em `app/core/placar_estrutural.py:637-639`; cópia em `:635`; renderização integral em `bin/gerar_estado.py:112-117`; selo genérico em `bin/tests/test_placar_nao_afirma_fora_do_ar.sh:65-75`. |
| Seis `ETAPAS[*].prova` | `app/core/espelho_verdade.py:49`, `:82`, `:169`, `:173`, `:177`, `:179` | Presença para validar FEITO em `app/core/espelho_verdade.py:201-203`; publicação limitada a 150 caracteres em `bin/gerar_estado.py:164`; censo e validação de caminhos em `app/core/tests/test_r1_termometro_uma_medicao.py:45-46`, `:64`, `:69-71`. |

Não identifiquei quebra de execução na análise estática: continuam strings nas mesmas chaves; os consumidores usam presença, cópia ou formatação, e o selo R1 examina os caminhos nas provas. `app/core/placar_estrutural.py:635-643`; `app/core/espelho_verdade.py:201-203`; `bin/gerar_estado.py:116-117`, `:164`; `app/core/tests/test_r1_termometro_uma_medicao.py:64-71`.

O recorte de conteúdo permanece imposto pelo publicador, independentemente da redação das provas. `bin/gerar_estado.py:164`.

Handoff e selo R6 leem outros dados: regex da frase dos contratos e `R6.numero`, respectivamente. `bin/handoff_sessao.sh:179-181`; `app/core/tests/test_selo_r6_frase_do_juiz.py:17-24`.

Execução Django e selos host: [nao verificado].

## 06/11 14:17 · sessao 8bb22732cf87 · H12H06-B-PAUTA-08 · verificador · origem=Code

**Não considero a obra pronta:** o novo command deixa vermelho o contrato B6.1, e a idempotência não está protegida contra execuções concorrentes (`app/chamados/tests/test_contract_crons.py:220`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/models.py:123`).

Analisei a cópia disponível e reproduzi detectores em memória. Identidade do HEAD/parent e exclusividade dos três arquivos no diff: **[nao verificado]**. Suite Django e mutações de código: **[nao verificado]**. [nao verificado]

1. **Quem escreve e quem lê.**

   | Estado/valor | Escritor | Leitores relevantes |
   |---|---|---|
   | Todos os 26 `CAMPOS` de `FechamentoMensal` | Criação com mês/ano; cálculo e persistência pelo serviço de fechamento; restauração pela mesma porta. `app/ponto/management/commands/aplicar_09_corte_b.py:67`; `app/ponto/services/fechamento.py:889`; `app/ponto/services/fechamento.py:893`; `app/ponto/services/fechamento.py:1128`; `app/ponto/services/fechamento.py:1144`; `app/ponto/services/fechamento.py:1376` | PAGO, retorno de leitura, comparação nova e exportação TXT. `app/ponto/portas/regen_exportada.py:79`; `app/ponto/services/fechamento.py:1118`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/folha/export.py:722` |
   | `Pauta.texto`, `ancora_tipo`, `ancora_id`, `pai` | `escrever`, no `create` canônico. `app/pautas/services.py:180` | Deduplicação nova, API e seleção de cabeças. `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/views.py:40`; `app/pautas/services.py:435` |
   | `Pauta.cancelado_em` | `cancelar`. `app/pautas/services.py:294` | `nao_canceladas`, reversão. `app/pautas/models.py:60`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:214` |
   | `Pauta.feito_em` | `marcar_feita` e `marcar_feita_pelo_sistema`, ambos na porta. `app/pautas/services.py:247`; `app/pautas/services.py:272` | QuerySet, API, deduplicação e reversão. `app/pautas/models.py:57`; `app/pautas/views.py:43`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:178`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216` |
   | `ExportacaoDominio` | `registrar` cria conteúdo/hash/metadados; `invalidar` escreve invalidação/substituição. `app/folha/models.py:182`; `app/folha/models.py:158` | K6 consulta empresa/competência e vigência; o command lê exclusivamente sua resposta. `app/folha/export.py:581`; `app/folha/models.py:211`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:118` |

   **Não encontrei escritor novo de Pauta fora da porta:** cabeças/filhas passam por `escrever`, reversão por `cancelar` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:188`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`).

   A chamada de DEVIDO usa `somente_leitura=True`, com **`lavrar_dias_pagos=False` no default**. Ela monta instância destacada, evita `get_or_create`/`save`/`update` e não entra na lavra de `DiaPago` nem na trilha da guarda; células são consultadas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/ponto/services/fechamento.py:439`; `app/ponto/services/fechamento.py:881`; `app/ponto/services/fechamento.py:1112`; `app/ponto/services/fechamento.py:1175`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:595`).

   **Entretanto, escrever pauta também escreve `ContadorPauta` e `LogAuditoria`, e pode criar `auth.User`.** `autor_sistema` chama `get_user_sistema`, cujo código faz `get_or_create`; isso ocorre antes do `atomic` do command e não é desfeito pela reversão (`app/pautas/services.py:186`; `app/pautas/services.py:352`; `app/core/observ.py:134`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`; `app/core/utils.py:18`). Prova dinâmica de ausência de escrita em todas as tabelas: **[nao verificado]**.

2. **DEVIDO versus REGEN e `fechamento_lido`.**

   Os argumentos que definem a conta coincidem: mês/ano e lista unitária; `empresa_id=None` nos dois caminhos. `motivo_exportada` muda o registro do ato; `coletar_furos` coleta saída; `retornar_objeto` escolhe a representação. `fechamento_lido` chama a mesma função, acrescentando apenas `retornar_objeto=True` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:439`; `app/ponto/services/fechamento.py:538`; `app/ponto/services/fechamento.py:1111`; `app/ponto/services/fechamento.py:1119`; `app/ponto/services/fechamento.py:1743`).

   **Ressalva:** mesma conta não garante o mesmo resultado da retificação completa: REGEN primeiro regenera células, alterando os insumos. Além disso, a guarda da 09 pode bloquear a escrita enquanto permite leitura (`app/ponto/portas/regen_exportada.py:174`; `app/ponto/portas/regen_exportada.py:179`; `app/ponto/services/fechamento.py:530`; `app/ponto/services/fechamento.py:551`).

3. **Chaves e unidades.**

   Ambos projetam exatamente o mesmo `CAMPOS`; não há chave unilateral no código atual. Mês/ano são excluídos da comparação. Horas continuam horas, minutos continuam minutos e contadores continuam contadores, sem conversão pelo command (`app/ponto/portas/regen_exportada.py:84`; `app/ponto/services/fechamento.py:1120`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:42`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/ponto/models.py:412`; `app/ponto/models.py:432`; `app/ponto/models.py:465`).

   **Ausência versus zero pode desaparecer:** `_diferente(None, 0)` considera igualdade por usar `a or 0`. É comportamento da autoridade reutilizada, não divergência de chaves introduzida aqui (`app/ponto/portas/regen_exportada.py:248`).

4. **Derivação paralela e formatação.**

   Não identifiquei regra financeira própria: cálculo e tolerância vêm das autoridades; a soma local conta colaboradores nos baldes (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:104`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`).

   Para números finitos, `_fmt` não transforma diferença acusada em dois números iguais: arredonda a quatro casas, enquanto `_diferente` exige delta superior a `0.015` — inferência matemática dessas duas implementações (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:45`; `app/ponto/portas/regen_exportada.py:250`).

5. **Universo e ordem das checagens.**

   Em execução concluída, as quatro partes somam os **IDs distintos** pedidos. Inexistentes recusam; ausência de DEVIDO recusa o ato antes de `_escrever`; os demais entram em exatamente um balde (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:91`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:107`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:119`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:125`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:136`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`).

   Entregue antes de gravado é coerente com o escopo: K6 exige TXT vigente não invalidado; competência não entregue fica fora desta pauta, com explicação. Não significa proibir seu recálculo nem fazer Pauta pré-condição, revogada pela L-113 (`app/folha/export.py:583`; `app/core/juizes.py:312`; `CLAUDE.md:398`; `CLAUDE.md:403`; `app/docs/LEIS.md:150`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:121`).

   **A promessa sobre desligado é condicional:** se não entregue ou sem gravado, ele entra nesses baldes; somente quem chega ao cálculo sem retorno provoca recusa. Não é “todo desligado sempre recusa” (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:123`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:128`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`).

6. **Idempotência.**

   `col1` não casa com `col12`: a chave contém espaço depois do PK e competência completa; a busca acrescenta outro espaço delimitador. Feitas continuam participando porque a consulta usa `nao_canceladas` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/models.py:60`).

   **Não há unicidade garantida:** consulta e criação estão separadas, a consulta ocorre antes do `atomic`, e o modelo não tem constraint dessa chave. Duas execuções podem observar ausência e criar duas cabeças (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:183`; `app/pautas/models.py:123`).

   Pelo contrato declarado de chave do fato, uma segunda rodada com DEVIDO diferente **não cria outra pauta**: também não atualiza a antiga. Assim, a pauta persistida pode conservar o DEVIDO anterior; o teste de duas rodadas não muda o motor entre elas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:29`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:176`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188`).

7. **Reversão e falhas.**

   O arquivo com plano nasce antes da primeira pauta e antes de `autor_sistema`; cabeça e filhas ficam numa transação por colaborador. Rodadas parcialmente concluídas preservam os grupos anteriores e seus IDs no arquivo (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:158`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:183`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:191`).

   **Banco e arquivo não são atomicamente coerentes:** o JSON recebe IDs antes do commit; falha no commit pode deixá-lo apontando para registros inexistentes. O reverter ignora IDs ausentes. Há também corrida entre `exists` e a gravação: duas execuções com o mesmo caminho podem ultrapassar a recusa e sobrescrever o arquivo (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:192`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:213`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:87`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:231`).

   `cancelar(p, p.autor_user)` é permitido pela porta: ela exige igualdade com o autor gravado. O command pula feitas, cuja tentativa de cancelamento seria recusada (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:216`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`; `app/pautas/services.py:288`).

8. **Tamanho e identificação.**

   O teto é 500. As filhas passam pelo packer, que inclui prefixo e continuação no cálculo e recusa item impossível. A cabeça não passa pelo packer, mas seu molde fixo cabe; medi em memória 334 caracteres com PK de 19 dígitos e ano 9999 (`app/pautas/models.py:89`; `app/pautas/services.py:123`; `app/pautas/services.py:133`; `app/pautas/services.py:140`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:57`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:169`).

   Texto e stdout explícitos usam `colNNN`, sem nome/CPF. **O caminho indireto de falha pode registrar nome no logger do fechamento**; destino efetivo desse logger em stdout/stderr: **[nao verificado]** (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:167`; `app/ponto/services/fechamento.py:1287`).

   “PAGO” significa gravado de `FechamentoMensal`, não pagamento comprovado nem conteúdo recuperado do TXT; o próprio cabeçalho manda conferir no Domínio (`app/ponto/portas/regen_exportada.py:81`; `CLAUDE.md:410`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:60`).

9. **Os testes mordem, mas não fecham todas as propriedades.** [nao verificado]

   Os mutantes abaixo são candidatos por inspeção; sobrevivência efetivamente executada: **[nao verificado]**.

   | Propriedade | Mutante que a cobertura deixa aberto |
   |---|---|
   | Entregue | Fixar K6 em agosto/2026: todas as chamadas do helper usam essa competência. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:73` |
   | Gravado intacto | Alterar apenas `previsto_em`, `derivado_em` ou carimbos de aprovação: o hash os exclui e o teste não compara esses campos. `app/ponto/portas/regen_exportada.py:37`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:175` |
   | Dry não escreve | Antecipar `autor_sistema` para antes da saída dry, permitindo criar User: o teste conta somente Pauta. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:156` |
   | Duas rodadas | Deduplicar também pelo valor de DEVIDO: as duas rodadas do teste têm o mesmo DEVIDO. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188` |
   | Feita | Remover o salto da feita na reversão: o teste de feita apenas repete a emissão; não reverte. A porta real recusaria a tentativa. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:205`; `app/pautas/services.py:290` |
   | Reversão antes/coerência | Remover o `atomic`: a falha simulada ocorre já na primeira escrita, sem cabeça prévia para ficar órfã; não cobre falha na filha/commit. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:216` |
   | Recusa do desligado | Substituir a ausência no retorno por regra própria `situacao == 'desligado'`: o único fixture sem retorno usado nessa prova é desligado. `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:54`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:136` |

   Há mordida concreta para os dois números, exclusão da empresa aberta, duplicação sequencial e cancelamento sem apagar; não classifico a bateria inteira como decorativa (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:99`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:125`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188`).

10. **Contratos e selos.**

   - **B6.1 vermelho:** reproduzindo a expressão do contrato, sobra `pauta_pago_x_devido_exportada`. Todo command precisa estar nas entradas ou em `FORA_DE_PIPELINE`; o molde está cadastrado, o novo não (`app/chamados/tests/test_contract_crons.py:214`; `app/chamados/tests/test_contract_crons.py:225`; `app/config/crons.py:777`).
   - **S4 não acusa esta chamada:** libera a presença de `somente_leitura`; o novo command passa `True` (`app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:88`; `app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:116`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`).
   - **Pauta/censo não ganham escrita fora da porta:** o command delega as mutações; os contratos cobram esse limite (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`; `app/pautas/tests/test_contract_um_escritor.py:37`; `app/core/tests/test_censo_escritas.py:297`).
   - **Relógio:** o teste congela o tempo; o command lê `now` uma vez para o metadado da reversão, antes do laço. Os detectores da grade não apontaram ocorrência nova (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:34`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154`; `app/escala/tests/test_selo_grade_pede_o_instante.py:93`).

11. **Classes de reprovação anteriores.**

   Não identifiquei segundo juiz, segundo escritor de Pauta ou derivação financeira paralela: o command chama K6, fechamento, comparação e portas existentes. O registro vivo é `core/juizes.py`, conforme o próprio documento de consulta (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:103`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:129`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:133`; `app/docs/JUIZES.md:7`; `app/core/juizes.py:312`).

   **Aparecem lacunas de prova e de proteção concorrente:** falha intermediária/commit não exercitada, DEVIDO alterado entre rodadas não exercitado e deduplicação sem trava/constraint. Há ainda o pré-requisito estrutural B6.1 pendente (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:216`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`; `app/pautas/models.py:123`; `app/chamados/tests/test_contract_crons.py:220`). Ordem operacional das travas e pouso efetivo antes de pré-requisito: **[nao verificado]**.

## 06/11 14:15 · sessao f7903ca7ae65 · - · - · origem=Ronald (tmux)

Achado bloqueante: o command novo não tem casa no pipeline. Reproduzi em memória a expressão do contrato e obtive `['pauta_pago_x_devido_exportada']`; `test_todo_command_tem_casa` exige que todo command esteja nas entradas ou em `FORA_DE_PIPELINE` (`app/chamados/tests/test_contract_crons.py:214-228`). O molde está declarado; o novo falta (`app/config/crons.py:777-780`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:64`).

Demais verificações estruturais reproduzidas em memória, sem arquivos:

- S4: nenhum ofensor; chamada nova é `(129, True)`. O detector libera pela presença de `somente_leitura`, inclusive sem conferir se seu valor é `True` (`app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:88`; `app/ponto/tests/test_s4_ninguem_recalcula_para_ler.py:113-118`).
- Censo: nenhuma escrita direta no command; família pauta com sete sítios, sete na porta, zero fora. A asserção exige zero fora (`app/core/censo_escritas.py:451-466`; `app/core/tests/test_censo_escritas.py:297-298`). O command escreve e cancela pela porta (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:184-190`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`).
- Relógio do teste: detector retornou zero; arquivo congela o tempo (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:34`; `app/core/tests/test_selo_teste_sem_relogio.py:32-45`).
- Relógio produção: os três detectores da grade retornaram `[]`. O command lê `timezone.now()` uma vez, para `gerado_em`, antes do laço (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:154-161`). O selo da grade só examina alvos enumerados e funções que os chamam (`app/escala/tests/test_selo_grade_pede_o_instante.py:24-38`; `app/escala/tests/test_selo_grade_pede_o_instante.py:109-119`).

Q9 — os testes têm mordida, porém deixam estes mutantes candidatos sobreviverem por inspeção; **não executei mutações** `[nao verificado]`: [nao verificado]

| Propriedade | Mutante que a cobertura deixa aberto |
|---|---|
| Entregue | Fixar a consulta em mês 8/2026: todo `_roda` usa exatamente essa competência (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:73-75`). Os testes mordem empresa aberta e TXT invalidado, mas não outro mês (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:125-130`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:239-247`). |
| Gravado intacto | Alterar apenas `derivado_em`, `previsto_em`, `aprovado_em` ou `aprovado_por`: estão excluídos do hash usado na prova (`app/ponto/portas/regen_exportada.py:37-38`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:175-186`). |
| Dry não escreve | Inserir escrita em User, DiaPago, CelulaDia ou trilha durante dry: o teste só compara quantidade de Pauta (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:156-162`). |
| 2x | Deduplicar pelo conteúdo de PAGO/DEVIDO em vez de chave do fato: a segunda rodada conserva o mesmo motor/DEVIDO (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188-196`). |
| Feita | Na reversão, cancelar indevidamente uma feita: o teste de feita verifica somente não gerar outra cabeça; não chama reversão (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:205-214`). A porta verdadeira recusaria a tentativa (`app/pautas/services.py:288-291`). |
| Reversão antes | Remover `transaction.atomic` ou criar inicialmente arquivo sem plano: a falha simulada ocorre na primeira escrita e só exige `pautas == []`, sem exigir `plano` nem falhar na filha/commit (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:216-226`). |
| Recusa desligado | Duplicar a regra com `c.situacao == 'desligado'` em vez de tratar ausência no retorno: o único fixture ausente é desligado (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:54`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:136-143`). |

Sobre Q11: há cobertura incompleta para falha intermediária e mudança de DEVIDO entre rodadas, além do vermelho B6.1 comprovado; não sustento “RED sem mordida” em bloco, pois existem asserções de efeito para texto com os dois números, ausência de pauta em aberta, idempotência e cancelamento sem apagar (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:99-118`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:125-130`; `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:188-203`).

_2333 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
