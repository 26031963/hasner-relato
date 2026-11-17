# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 10:57** · publicado em **17/11/2026 10:57** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 10:57 · sessao 627e545c26e3 · RESSUSCITAR-DONO · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 1 e 2:** faltam fechar a classificação “paga fora”, a compatibilidade da atribuição ao admin com a conservação da tarefa e a indicação literal da fatia pelo brief. (`app/docs/BACKLOG.md:1992`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:329`; linha do brief: [nao verificado].)

**Item 1 — PROCEDE**

1. **As premissas conferem parcialmente.** A lei é a mesma: ressuscitar deve devolver a pergunta ao colaborador? O registro acrescenta a pergunta sobre resposta vazia, sem decidi-la; L-130 determina limpar o par inteiro e conservar o anterior na trilha, mas deixa a devolução pendente. (`app/docs/RELATO.md:188`; `app/docs/RELATO.md:191`; `app/docs/CORTES.md:488`; `app/docs/LEIS.md:151`.)

   Existe autoridade para **ENTREGUE no dia do fato**: `competencia_do_dia_entregue` compõe `janela_atual(dia, empresa)` com `competencia_entregue`, cujo predicado é exportação vigente não invalidada. (`app/chamados/services/vigia_fio.py:785`; `app/chamados/services/vigia_fio.py:801`; `app/folha/export.py:588`; `app/folha/export.py:591`.)

   **“Não entregue” não demonstra “ABERTA e não paga fora”.** O cadastro e o juiz geral `competencia_ja_paga`, que incorporarão pagamento externo, constam como obra pendente. O outro classificador existente chama de “paga” a competência com holerite de lote publicado, outro fato. (`app/docs/BACKLOG.md:1992`; `app/docs/CORTES.md:496`; `app/ponto/services/fechamento.py:1539`.) Portanto, a saída item7 permite citar **1116 ENTREGUE e 1058 ABERTA**, mas não comprova que as 1058 excluem pagamento externo: critério do script [nao verificado].

   Há **lista e botão existentes**: painel de gestão em `painel_gestao`/`montar_contexto_painel`; pergunta ancorada no chamado de destino ou, na ausência dele, no container; botão de cobrança na lista e no modal. (`app/chamados/views.py:721`; `app/chamados/views.py:727`; `app/chamados/services/fio.py:117`; `app/templates/chamados/partials/_lista_chamados.html:84`; `app/templates/chamados/partials/modal_fio.html:376`.) Isso não comprova a exposição de **toda ressuscitada**: a lista filtra status, verbo e arquivamento; a tarefa só oferece `porta_admin` quando a âncora pertence à fila. (`app/chamados/services/painel.py:316`; `app/chamados/services/painel.py:320`; `app/chamados/services/painel.py:328`; `app/chamados/services/tarefa_do_dia.py:153`.)

2. **Há colisão de comportamento a resolver.** Hoje, sem decisão, a tarefa exige resposta do colaborador; com SILENCIO, não publica ação. O teste de ressuscitação exige conservar exatamente a tarefa anterior nas duas classes. Torná-las obrigatoriamente do admin muda esse contrato. (`app/chamados/services/tarefa_do_dia.py:156`; `app/chamados/services/tarefa_do_dia.py:177`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:329`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:331`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:334`.)

   **Ignorar o novo ato e atribuir dono admin não são logicamente incompatíveis**, desde que exista outra regra explícita de encaminhamento. Porém, o leitor atual escolhe o último ato; ignorar o compensador, sozinho, conserva os ramos anteriores e não produz essa atribuição. Essa outra regra e sua compatibilidade com “a fila não muda” estão [nao verificado]. (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:164`.) Os originais de CORTE-F2-RESSUSCITAR-GRAVA-ATO e RESPOSTAS-1711-1050, com suas linhas, estão [nao verificado]; a conservação da fila está registrada na cópia. (`app/chamados/models.py:1622`; `app/docs/RELATO.md:188`.)

   **L-130 não decide o dono:** limpar resposta e decidir quem deve agir são perguntas distintas no próprio registro. (`app/docs/LEIS.md:151`; `app/docs/RELATO.md:191`.)

3. **O literal operacional ainda não fecha.** O botão existente recebe **chamado**, envia push ao **colaborador** e limita a cobrança a uma por colaborador/dia; não recebe pergunta nem registra mudança de dono. Assim, ele comprova a cobrança manual disponível, mas não uma devolução individual de responsabilidade “caso a caso”. (`app/chamados/urls.py:25`; `app/chamados/views_cobrar.py:29`; `app/chamados/views_cobrar.py:36`; `app/chamados/views_cobrar.py:44`; `app/chamados/views_cobrar.py:55`.)

   As 1058 perguntas também não equivalem a 1058 **linhas novas** do admin: faltam agrupamento por colaborador-dia/empresa e comparação com o que já aparece. Esse incremento está [nao verificado]; a lista atual pagina chamados e também dispõe de resumo por colaborador. (`app/chamados/services/painel.py:335`; `app/chamados/services/painel.py:339`; `app/chamados/services/painel.py:342`.)

4. **O item 1(a), como lista textual sem escrita, não autoriza executar tratamento de estoque.** O precedente de competência trancada encerra chamados, fecha disputas e encaminha Pauta DP; reutilizá-lo ultrapassaria “sem escrita”. (`corte_ressuscitar_tem_dono_1711.txt:6`; `app/chamados/services/competencia_trancada.py:13`; `app/chamados/services/competencia_trancada.py:16`; `app/chamados/services/competencia_trancada.py:20`.)

   O futuro 1(b) alcança indiretamente a **apresentação da Folha**, porque ela lê o fio publicado pela tarefa. Isso não demonstra alteração de valores ou TXT: impacto monetário/exportação [nao verificado]. (`app/folha/fora_da_folha.py:70`; `app/folha/fora_da_folha.py:76`.) O corte também não substitui o `!` específico reservado ao estoque entregue de L-130. (`app/docs/LEIS.md:151`.)

**Item 2 — PROCEDE**

1. **Resolver a pergunta nominal nas duas matrizes é coerente com o registro**, mas resposta vazia e ressuscitação foram reunidas como perguntas relacionadas; não se demonstrou que decidir apenas “pergunta ressuscitada” esgota todo o alcance registrado. (`app/docs/CORTES.md:488`; `app/docs/RELATO.md:191`; `f5_matriz.md:111`; `remedida.md (F6):3`.)

2. **Registrar “resolvido” não elimina a colisão do item 1.** F6 deve consumir o dono declarado pela tarefa, sem tabela própria; portanto precisa encontrar a atribuição ao admin resolvida nessa autoridade. (`app/docs/CORTES.md:488`.) O leitor atual ainda conserva colaborador/nenhuma ação nos casos apontados acima. (`app/chamados/services/tarefa_do_dia.py:157`; `app/chamados/services/tarefa_do_dia.py:177`.)

3. **A fatia indicada pela cópia é F5:** o RELATO afirma expressamente que F5 toca `tarefa_do_dia.py` e muda tarefa/cobrança. (`app/docs/RELATO.md:506`; `app/docs/RELATO.md:577`.) **Linha do brief que declara esse arquivo a construir e o leitor do dono por pergunta: [nao verificado].** A GRADE apenas referencia o brief ausente; não satisfaz a exigência literal do item 2. (`app/docs/GRADE-MESTRA.md:1807`.)

   **“Lei 0 nas duas” fecha para a pergunta efetivamente cortada; como encerramento global, não está demonstrado.** A remedida F5 mantém SILENCIO/ABSORCAO como `[nao verificado]`; isso não prova outra lei aberta, mas também não autoriza apagá-la como resolvida por este corte. (`remedida.md (F5):3`.) Os ramos são diferentes: SILENCIO não exige ação; ABSORCAO varia conforme o estado da ausência. (`app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:178`.)

4. **O registro documental não constitui autorização adicional para escrita em estoque, exportada ou dinheiro.** Esse alcance continua limitado pela lista sem escrita e pelas restrições já registradas para o estoque entregue. (`corte_ressuscitar_tem_dono_1711.txt:6`; `app/docs/LEIS.md:151`.)

VEREDITO por item: 1 PROCEDE; 2 PROCEDE; FATIA-DO-1b F5

## 17/11 10:54 · sessao d14155118690 · - · - · origem=Ronald (tmux)

A premissa **ENTREGUE tem juiz existente**, mas **ENTREGUE ou paga fora → ABERTA** não fecha como classificação única disponível nesta cópia. O código mede ENTREGUE; o cadastro/juiz que incluirá pagamento fora consta como obra ainda por pousar. (`app/folha/export.py:556`; `app/docs/CORTES.md:496`; `app/docs/RELATO.md:104`)

| Pergunta | Autoridade e limite |
|---|---|
| Qual competência contém o dia do fato? | `ponto.janelas.janela_atual(dia, empresa)` respeita o corte da empresa. (`app/ponto/janelas.py:60`; `app/ponto/janelas.py:63`) O adaptador de evento `competencia_do_fato(colaborador, quando)` devolve `(mes, ano)` e usa o dia do fato. (`app/ponto/services/fechamento.py:1592`; `app/ponto/services/fechamento.py:1602`) |
| Foi ENTREGUE? | `folha.export.competencia_entregue`: ExportacaoDominio vigente e não invalidada, por empresa/mes/ano. (`app/folha/export.py:556`; `app/folha/export.py:588`; `app/folha/export.py:591`; `app/folha/export.py:594`) Registro K6: `app/core/juizes.py:343`. |
| Dia do fato está em competência ENTREGUE? | `vigia_fio.competencia_do_dia_entregue` compõe janela + K6; retorna `None` sem dia. (`app/chamados/services/vigia_fio.py:785`; `app/chamados/services/vigia_fio.py:797`; `app/chamados/services/vigia_fio.py:801`; `app/chamados/services/vigia_fio.py:804`) |
| ABERTA é a negação do K6? | No censo `carimbos_sem_lastro_por_dono`, sim, como **rótulo de medida**, com sem dia também classificado aberta. Isso não demonstra “não paga fora”. (`app/chamados/services/vigia_fio.py:764`; `app/chamados/services/vigia_fio.py:765`; `app/chamados/services/vigia_fio.py:779`) |
| Existe outro ABERTA? | `marco_da_competencia` usa `marcos_da_competencia`, cujo `'aberta'` significa nenhum dos marcos aprovada/exportada/trancada/paga; “paga” ali significa holerite de lote publicado, não cadastro de pagamento externo. (`app/ponto/services/fechamento.py:1519`; `app/ponto/services/fechamento.py:1523`; `app/ponto/services/fechamento.py:1539`; `app/ponto/services/fechamento.py:1543`; `app/ponto/services/fechamento.py:1553`) |
| FechamentoMensal aberto decide? | Não substitui K6 nem pagamento externo: os estados são aberto/aprovado/exportado por colaborador. (`app/ponto/models.py:398`; `app/ponto/models.py:399`; `app/ponto/models.py:405`) `competencia_fechada` barra aprovado/exportado do FM da competência do dia, outra pergunta. (`app/ponto/janelas.py:75`; `app/ponto/janelas.py:85`; `app/ponto/janelas.py:87`) |
| Paga fora está implementada pelo juiz geral? | Implementação nesta cópia de `competencia_ja_paga`/`CompetenciaPaga`: **[nao verificado]**. O corte manda cadastro pela tela e diz COMPETENCIA-PAGA por pousar atrás da espinha. (`app/docs/CORTES.md:496`) O RELATO manda o cadastro cobrir anteriores a 09/2026 e 10/2026, sem construtor naquele registro. (`app/docs/RELATO.md:104`; `app/docs/RELATO.md:171`) |

Consequência para a medida item7: a saída raspada 1116 ENTREGUE + 1058 ABERTA não demonstra que a segunda classe excluiu pagamentos externos; o script/critério das classes não foi fornecido **[nao verificado]**. Há precedente documental explícito de 10/2026 paga fora e K6 dizendo não entregue. (`app/docs/CORTES.md:496`; `app/docs/RELATO.md:130`)

O precedente de competência trancada cria Pauta DP e escreve em chamados/disputas; portanto não serve como implementação automática da **lista textual sem escrita** deste aval. (`app/chamados/services/competencia_trancada.py:13`; `app/chamados/services/competencia_trancada.py:20`; `app/chamados/services/competencia_trancada.py:30`)

## 17/11 10:54 · sessao 065567330d1c · - · - · origem=Ronald (tmux)

Fatia indicada pela cópia: **F5**. O registro diz “F5 toca chamados/services/tarefa_do_dia.py” (`app/docs/RELATO.md:506`), “E4c F5 muda chamados/services/tarefa_do_dia.py” (`app/docs/RELATO.md:525`) e “F5 MUDA tarefa_do_dia/cobranca” (`app/docs/RELATO.md:577`).

**Linha do brief: [nao verificado].** Os briefs originais não estão na cópia; o catálogo apenas referencia `logs/ESPINHA-BRIEFS/F5/brief.md` (`app/docs/GRADE-MESTRA.md:1807`). Portanto citar o RELATO como se fosse linha do brief não fecha o pedido literal.

O leitor atual escolhe a última decisão (`app/chamados/services/tarefa_do_dia.py:158`), devolve RECUSA/REABERTURA ao colaborador (`app/chamados/services/tarefa_do_dia.py:164`) e conserva SILENCIO sem ações (`app/chamados/services/tarefa_do_dia.py:177`). A exposição inclui pergunta e razão (`app/chamados/services/tarefa_do_dia.py:277`).

Os textos CORTE-F2-RESSUSCITAR-GRAVA-ATO e RESPOSTAS-1711-1050 não foram encontrados na cópia; sua redação integral e linhas originais são **[nao verificado]**. O registro disponível preserva a fila e declara a lei ainda não decidida (`app/docs/RELATO.md:188`; `app/docs/RELATO.md:191`).

## 17/11 10:53 · sessao f35bcb1d3b86 · PORTA-EXPORT · verificador · origem=Code

**AVAL-DEVOLVIDO no item 2, com reflexo no R3/RED:** o carimbo atual não fornece “N colaboradores distintos da empresa”, e os três estados propostos não cobrem medição que falhou. São lacunas do literal, não motivo para afrouxar a porta. (`app/folha/porta_export.py:687-691`, `:753-764`, `:818-821`; `app/docs/LEIS.md:61-62`)

Esta é revisão do aval, não certificação da construção. HEAD criptograficamente conferido, execução de testes/mutantes, instalação, duração atual e estado de produção: **[nao verificado]**. [nao verificado]

As premissas centrais do preparo conferem: `lavrar` escreve o carimbo; o comando chama `medir → lavrar`; `conferir` lê e a view consome sua recusa antes da montagem. O auxiliar exclusivo de testes também passa por `lavrar`. (`app/folha/porta_export.py:768-797`, `:800-821`, `:870-881`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `:86-95`; `app/folha/views.py:128-133`)

A identidade atual é **chave + escopo + escopo_id + data_ref**: `chave='porta_export_leitores'`, `escopo='empresa'`, `escopo_id=empresa.pk`, dia atual. Competência fica somente em `detalhe`; portanto B substitui A na mesma empresa/dia. (`app/folha/porta_export.py:18`, `:772-776`, `:807-817`; `app/inteligencia/models.py:102-105`)

1. **MUDA 1 — CONFERE, com encaixe técnico necessário.**

   O trabalho longo deve ir para job; a medição do selo dentro do request é expressamente rejeitada. O fluxo atual apenas confere e devolve a mensagem técnica, confirmando o problema descrito. (`CLAUDE.md:89`; `app/folha/porta_export.py:7-12`, `:809-820`; `app/folha/views.py:124-133`)

   O “padrão da casa” existe em **`LoteEspelho`**, seguindo mecanismos de **`EnvioHolerite`/`LoteHolerite`**: modelo persistido com estados, PID e progresso; execução de management command por `Popen`, `nice 10` e sessão separada. **Sai do processo do request, mas continua no container da UI**; não é uma fila genérica com worker independente. O precedente declara que o job morre se o container cair. (`app/relatorios/models.py:100-132`, `:143-146`; `app/relatorios/lote_espelho.py:117-138`; `app/holerite/views.py:1686-1711`)

   O mesmo comando pode ser preservado, mas hoje só aceita mês/ano e mede **todas as empresas ativas**. Para cumprir o recorte por empresa e coordenar clique/cron, será necessário adaptar sua seleção e exclusão, conservando `lavrar` como escritor único; dispará-lo sem adaptação também carimba outras empresas. Isso é encaixe técnico, não impossibilidade literal. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`, `:86-92`; `CLAUDE.md:276-280`; `app/docs/LEIS.md:65`)

2. **MUDA 2 — PROCEDE.**

   Há precedente visual com polling e estados terminais, sem diálogo nativo. A hora pode vir de **`MetricaSnapshot.calculado_em`**. Tela e botão devem consultar a mesma autoridade. (`app/templates/relatorios/_lote_espelho_progresso.html:12-14`, `:26-40`; `app/inteligencia/models.py:96`; `app/docs/LEIS-UI.md:23-29`, `:87-93`)

   **O N não está disponível com a unidade prometida.** `valor` soma ocorrências de listas, dias de HE e `cartao_x_txt`, que abrange toda a competência. Um colaborador pode contribuir várias ocorrências; o contador global pode incluir outra empresa. A mensagem vigente chama corretamente isso de **“ponto(s)”**. Usar `m.valor` como “N colaboradores” colidiria com L-003. (`app/folha/porta_export.py:687-691`, `:725-727`, `:753-764`, `:865-866`; `app/docs/LEIS.md:61`)

   O comando subordinado oferece `--ids`, mas `medir` não o solicita: captura somente o escalar, que é persistido no carimbo. Portanto os IDs globais necessários à deduplicação e ao recorte não estão disponíveis no snapshot atual. (`app/relatorios/management/commands/cartao_x_txt_divergentes.py:20-24`, `:51-74`; `app/folha/porta_export.py:667-670`, `:776-795`)

   **Também falta destino para falha operacional.** O código distingue divergência positiva de “não pude medir”, representado por valor negativo. Uma exceção pode encerrar o comando antes de qualquer lavratura; o padrão de job ainda prevê processo morto. Nenhum desses casos pode significar honestamente “N colaboradores divergentes” ou permanecer indefinidamente “EM CONFERÊNCIA”. O literal precisa definir esse resultado antes da construção do item. (`app/folha/porta_export.py:774`, `:818-821`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88-95`; `app/relatorios/lote_espelho.py:66-78`; `app/docs/LEIS.md:62`)

   Linha crítica e comunicação efetiva ao suporte no novo fluxo: **[nao verificado]**. “Leva alguns minutos” para 11/2026: **[nao verificado]**.

3. **MUDA 3 — CONFERE; cabe sem migration.**

   O campo exato pode ser **`MetricaSnapshot.chave`**, `CharField(max_length=60)`: por exemplo, `porta_export_leitores:2026-11`, com 29 caracteres. Mantêm-se `escopo='empresa'`, `escopo_id=empresa.pk` e `data_ref=hoje`; a competência passa a participar da unicidade pela chave. É viabilidade inferida diretamente do modelo existente. (`app/inteligencia/models.py:87-105`)

   Escritor e leitor precisam usar a mesma formação da chave. Alterar apenas `detalhe` não resolve; alterar apenas um dos dois quebra a consulta. Não é necessário ampliar o escopo nem mudar a restrição única. (`app/folha/porta_export.py:772-776`, `:807-817`; `app/inteligencia/models.py:102-105`; `app/docs/LEIS.md:62`)

4. **MUDA 4 — CONFERE como requisito a construir; trava específica existente: não.**

   O selo atual não registra job em curso nem possui exclusão no comando. A constraint parcial de `LoteEspelho` protege pedidos do caderno PDF, não medições da porta. Desabilitar o botão também não garante exclusão entre requests concorrentes. (`app/folha/views.py:128-133`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86-95`; `app/relatorios/models.py:167-171`; `app/relatorios/lote_espelho.py:22-25`)

   Há precedente de exclusão sem migration por `pg_try_advisory_lock`, com resposta `em_curso` e liberação ao terminar. Outro consumidor explicita que a chave precisa considerar o schema, pois a trava pertence ao banco. Isso permite um encaixe para schema × empresa × competência; o protocolo de reserva antes do disparo e sua coordenação com cron ainda precisam ser construídos e provados. (`app/ponto/services/vigia_de_hora.py:100-104`, `:137-139`; `app/chamados/reconciliador.py:1216-1243`)

   A inexistência atual da trava não invalida uma ordem para construí-la. A garantia futura de uma única medição, inclusive na corrida clique/clique e clique/cron, permanece **[nao verificado]**.

5. **MUDA 5 — CONFERE, usando as autoridades existentes.**

   A exclusão atual está efetivamente em `FORA_DE_PIPELINE`, com justificativa contra agendamento. É uma decisão de implementação documentada; proibição nominal superior de agendar esse selo: **[nao verificado]**. O novo aval fornecido manda substituir essa classificação. (`app/config/crons.py:887-897`; aval fornecido sem arquivo:linha local: **[nao verificado]**)

   Existe fonte para competência corrente, chamada de “ABERTA” pelo helper: **`competencia_de_hoje`**, que consulta `janela_atual` e respeita o corte da empresa. `em_rollout` é campo explícito de `Empresa`, e há consumidor combinando esse filtro com a janela por empresa. Não precisa nascer juiz novo. (`app/ponto/services/gestao_he.py:410-429`; `app/ponto/janelas.py:36-63`; `app/colaboradores/models.py:45`; `app/ponto/services/furos_sem_cobranca.py:59-60`)

   **Corrente e estado “aberta” são perguntas diferentes:** para o estado existe `marcos_da_competencia`/`marco_da_competencia`. O cron deve usar a autoridade correspondente ao literal, sem inventar que mês civil ou ausência de tranca significa competência corrente. O comando atual ainda não faz essa seleção. (`app/ponto/services/fechamento.py:1473-1480`, `:1518-1555`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27-29`, `:86`)

   A entrada precisa declarar estágio e papel, sair de `FORA_DE_PIPELINE` e passar pelos contratos. `EMPRESAS=(2,3,4)` é lista estática, não substitui o campo dinâmico de rollout. Instalação possui caminho com diff e backup em `bin/crons.sh install`; instalação efetiva: **[nao verificado]**. (`app/config/crons.py:16-17`, `:29-33`, `:2363-2368`; `app/chamados/tests/test_contract_crons.py:220-237`; `app/core/tests/test_contract_tabuleiro.py:15-19`; `bin/crons.sh:35-40`)

As colisões de lei e de aval ficam assim:

- **Teto:** L-126 estabelece dois construtores. A exceção nominal do terceiro consta do anexo fornecido; seu registro local: **[nao verificado]**. Não há colisão se limitada à obra e ao construtor opus indicado. (`app/docs/LEIS.md:147`)
- **ZONA de crons:** o wrapper proíbe `app/config/crons.py` ao **Codex construtor**. Isso não impede o construtor opus declarado; esta revisão continua sendo só leitura. (`bin/codex_pergunta.sh:343-350`)
- **`core/_dialogo.html`:** a intocabilidade está registrada no contexto UI-VINCULO. Os estados locais desta obra não exigem alterar esse componente; mexer em include de base ainda acionaria a exigência de smoke nas duas cascas. (`app/docs/RELATO.md:152`; `app/templates/relatorios/_lote_espelho_progresso.html:12-40`; `CLAUDE.md:202-203`)
- **Leis dos sítios:** a busca encontra `porta_export.py` em **L-094/L-095/L-097** e crons em **L-060/L-101**. O corte que cita `folha/views.py` preserva turno aberto barrando o **carimbo da porta cega**, que não pode ser confundido com o snapshot desta revisão. (`app/docs/LEIS.md:94`, `:112-115`, `:127`; `app/docs/CORTES.md:317`)
- **Lei vigente de exportada:** L-113 altera L-092 e revoga a recusa geral de recalcular exportada; preserva versão anterior do TXT, trilha e DIFF antes de ato de frota. Isso não amplia este aval, cujo escopo continua literal. (`app/docs/LEIS.md:155`, `:62`; `CLAUDE.md:265-268`)

Para os RED, **CONFERE significa que o caso solicitado é válido**, não que já esteja verde; execução e mutantes mortos: **[nao verificado]**. [nao verificado]

| RED | Revisão |
|---|---|
| **R1 — CONFERE** | Hoje a ausência retorna comando técnico e a view o encaminha à resposta. O RED deve observar a resposta renderizada e o estado persistido em curso. (`app/folha/porta_export.py:809-813`; `app/folha/views.py:128-133`; `app/templates/folha/exportar.html:10`) |
| **R2 — CONFERE** | Zero libera esta porta; a fixture ainda precisa satisfazer integração, fechamentos, linhas e layout para produzir arquivo. (`app/folha/porta_export.py:818-867`; `app/folha/views.py:138`, `:163`, `:268-279`) |
| **R3 — PROCEDE** | A recusa por divergência existe, mas o estado (c) depende do N e do tratamento de falha devolvidos no item 2. (`app/folha/porta_export.py:818-821`, `:865-866`; `app/folha/views.py:130-133`) |
| **R4 — CONFERE** | Hoje B sobrescreve A; o caso deve atravessar `lavrar(A) → lavrar(B) → conferir(A)` para provar a identidade corrigida. (`app/folha/porta_export.py:772-776`, `:807-817`) |
| **R5 — CONFERE** | Dois cliques devem compartilhar a medição; acrescentar corrida concorrente e clique/cron é necessário para provar a exclusão, pois o precedente identifica explicitamente a corrida entre threads. (`app/relatorios/lote_espelho.py:22-25`, `:101-113`; `CLAUDE.md:28-29`) |
| **R6 — CONFERE** | Presença no pipeline e ausência nas exclusões correspondem ao contrato existente; o recorte rollout/competência também precisa ser exercitado. (`app/chamados/tests/test_contract_crons.py:220-237`; `app/colaboradores/models.py:45`; `app/ponto/services/gestao_he.py:410-429`) |

**SELO — CONFERE como contrato; hoje há violação demonstrável.** Ausência, competência incompatível e medição impossível incluem `docker exec`/`Rode:`; a mensagem atravessa a view e o armazenamento de feedback até o template. O teste precisa cobrir esses caminhos renderizados, com allowlist vazia; retirar a string apenas do primeiro caminho deixa os outros dois expostos. (`app/folha/porta_export.py:809-820`; `app/folha/views.py:133`; `app/core/respostas.py:68-71`; `app/templates/folha/exportar.html:10`)

Quanto a **exportada, dinheiro e `!`**, o encaixe examinado escreve carimbo e estado operacional; não fornece autorização para alterar cálculo, gravado ou montagem. Depois da porta, a view vigente já recalcula em modo leitura, aprova fechamentos, monta e registra o TXT: esses efeitos devem ser preservados. O recálculo de montagem dentro do request é distinto da medição do selo proibida pelo aval. Ausência de efeitos indiretos em todos os leitores transitivos e comportamento da futura implementação: **[nao verificado]**. (`app/folha/porta_export.py:772-796`; `app/folha/views.py:124-127`, `:207-209`, `:246-268`, `:295-300`; `CLAUDE.md:265-268`)

VEREDITO por item: 1 CONFERE; 2 PROCEDE; 3 CONFERE; 4 CONFERE; 5 CONFERE; RED PROCEDE; SELO CONFERE

## 17/11 10:51 · sessao ed7b73e6dca4 · - · - · origem=Ronald (tmux)

Padrão de job confirmado: `relatorios.models.LoteEspelho`, com estados `pedido/gerando/concluido/erro`, progresso, PID e datas; o modelo declara copiar `EnvioHolerite`, que usa o mecanismo de `LoteHolerite`. Não é fila/worker genérico: o disparador executa um management command por `subprocess.Popen`, `nice 10`, `start_new_session=True`, `sys.executable`, no diretório Django. Sai do processo do request; não sai do container da UI: o próprio padrão declara que o job morre se o container cair. (`app/relatorios/models.py:82`, `app/relatorios/models.py:100`, `app/relatorios/models.py:118`, `app/relatorios/lote_espelho.py:117`, `app/relatorios/lote_espelho.py:127`, `app/relatorios/lote_espelho.py:132`, `app/holerite/views.py:1686`, `app/holerite/views.py:1694`)

A tela do padrão faz polling htmx a cada dois segundos enquanto em curso e para em estado terminal; a view lê o estado e escolhe página ou parcial. Há componente da casa para adaptar sem tocar `_dialogo.html`. (`app/templates/relatorios/_lote_espelho_progresso.html:12`, `app/templates/relatorios/_lote_espelho_progresso.html:26`, `app/templates/relatorios/_lote_espelho_progresso.html:39`, `app/relatorios/views.py:1270`)

MUDA 1 exige adaptar o command: hoje ele só recebe `--mes/--ano` obrigatórios e mede todas as empresas ativas; não recebe empresa nem filtra rollout. Dispará-lo literalmente para um clique de empresa mede e carimba outras empresas. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`)

MUDA 4 não tem trava existente no selo: o caminho atual apenas confere e devolve mensagem; não registra job. Existe dedup de `LoteEspelho` por `UniqueConstraint(chave)` parcial sobre estados vivos, implementada por transação/`IntegrityError`, mas pertencente ao pedido do caderno PDF. Não pode ser declarada trava pronta do selo. (`app/folha/views.py:128`, `app/relatorios/models.py:168`, `app/relatorios/lote_espelho.py:83`, `app/relatorios/lote_espelho.py:101`)

Há precedente de exclusão sem migration: `vigia_de_hora` toma `pg_try_advisory_lock`, devolve `em_curso` quando ocupado e libera ao terminar. O consumidor de eventos tem context manager semelhante e inclui schema na chave, pois advisory lock pertence ao banco. Adaptar chave schema×empresa×competência é tecnicamente possível; persistência do estado antes do spawn e protocolo que cubra corrida clique/cron ainda são [nao verificado]. Ausência da trava hoje não colide necessariamente com MUDA 4, que manda construí-la; apenas MUDA 3 condiciona devolução à necessidade de migration. (`app/ponto/services/vigia_de_hora.py:100`, `app/ponto/services/vigia_de_hora.py:137`, `app/chamados/reconciliador.py:1216`, `app/chamados/reconciliador.py:1225`; condição do anexo [nao verificado])

MUDA 5 tem autoridades existentes:

- Competência corrente por empresa: `ponto.janelas.janela_atual`, que usa o corte cadastrado; `gestao_he.competencia_de_hoje` já expõe mês/ano pelo fim dessa janela. Resolver dentro do laço de empresas, pois cortes podem diferir. (`app/ponto/janelas.py:36`, `app/ponto/janelas.py:60`, `app/ponto/services/gestao_he.py:410`, `app/ponto/services/gestao_he.py:439`)
- Competência “aberta” como estado: `marcos_da_competencia()['marco']`, que retorna aberta quando nenhum marco é verdadeiro. Isso não equivale automaticamente a janela corrente. “Fechada” significa trancada; aprovada/exportada/paga são marcos diferentes. (`app/ponto/services/fechamento.py:1518`, `app/ponto/services/fechamento.py:1543`, `app/ponto/services/fechamento.py:1473`)
- Rollout é campo explícito `Empresa.em_rollout`, default falso; consumidor existente combina `filter(em_rollout=True)` com `janela_atual(hoje, emp)`. O selo ainda não usa esse recorte. (`app/colaboradores/models.py:45`, `app/ponto/services/furos_sem_cobranca.py:59`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`)

Hoje selo consta em `FORA_DE_PIPELINE`, com justificativa contra agendamento. O contrato B6 exige uma casa e rejeita simultaneidade entre pipeline e exclusões, portanto R6 corresponde ao contrato existente. Novo cron também precisa estágio e `PAPEL_DO_CRON`, além do recorte correto: `EMPRESAS=(2,3,4)` é universo estático e não juiz dinâmico de rollout. (`app/config/crons.py:887`, `app/chamados/tests/test_contract_crons.py:220`, `app/chamados/tests/test_contract_crons.py:230`, `app/config/crons.py:16`, `app/config/crons.py:17`, `app/config/crons.py:2363`, `app/core/tests/test_contract_tabuleiro.py:15`)

Instalação com trilha tem ferramenta existente: `bin/crons.sh install` mostra diff, guarda backup e instala o render. O envelope de execução registra início/fim/duração/exit e marca corrida em curso; essa marca contém PID no nome, logo é observação, não dedup por empresa/competência. Instalação efetiva em produção permanece [nao verificado]. (`bin/crons.sh:35`, `bin/crons.sh:38`, `bin/cron_run.sh:8`, `bin/cron_run.sh:13`)

## 17/11 10:51 · sessao c6860ae34daf · E4C-F2 · verificador · origem=Code

**Item 1 — CONFERE.** A exceção fecha o alcance sobre o leitor; preservar a tarefa inteira ainda exige o selo proposto. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:582`)

1. **Premissas:** a sequência é montada em `CargaDaTarefa.__init__`, ordenada por pergunta, instante e PK, admitindo apenas `ato_em <= agora`. Excluir **somente a REABERTURA marcada como ressuscitação antes de acrescentá-la à sequência** permite recuperar o último ato anterior tanto em `_pela_decisao` quanto na checagem de última RETRATAÇÃO. Ignorar apenas o ramo REABERTURA depois de escolher o último ato seria insuficiente. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:423`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:259`)

2. **Avais vigentes:** pelo texto recebido, o novo item 1 excepciona nominalmente “mudar tarefa_do_dia”, proibido pelo complemento de 16/11; o CORTE anterior excepciona o ato novo. Isso fecha a lacuna da revisão anterior, sem autorizar mudança de dono. Conferência dos arquivos originais dos anexos **[nao verificado]**.

3. **Literal:** a marca própria ainda precisa ser construída: os campos atuais da decisão não incluem esse discriminador. Autor “sistema” não serve, pois ausência rejeitada já produz reabertura para devolver ao colaborador. Filtrar o compensador resolve a seleção do último ato; não demonstra sozinho igualdade integral, pois a tarefa também recebe passivo, lavra, efeitos e pendências do app. (`app/chamados/models.py:4110`; `app/chamados/services/materializacao.py:1158`; `app/chamados/services/materializacao.py:1167`; `app/chamados/services/tarefa_do_dia.py:582`; `app/chamados/services/tarefa_do_dia.py:593`)

4. **Exportada/dinheiro/!:** igualdade da fila não prova conservação do CSF: ele consome resposta/validação pela leitura da sequência. Neutralidade financeira/exportada do futuro compensador **[nao verificado]**; continua obrigatório DIFF zero em exportada ou PAREI ! com tabela. (`app/chamados/models.py:1953`; `app/ponto/supra_juiz.py:573`; `app/docs/CORTES.md:487`)

**Item 2 — CONFERE como condição de prova executável; resultado e main distinta [nao verificado].**

1. **Premissas:** na cópia existem `ressuscitar_perguntas(self)` e `tarefa_do_dia(colab, dia_do_turno, agora, *, carga=None)`. Ressuscitar não recebe relógio por argumento, mas o teste existente fixa `timezone.now()` ao executá-lo. Portanto essa diferença de assinatura não impede controlar o relógio. (`app/chamados/models.py:1579`; `app/chamados/services/tarefa_do_dia.py:733`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:325`)

   A guarda de exportada existe **como obrigação de conservação**; o leitor real do supra é chamável e já integra o selo. Não há bloqueio automático de exportada em `perguntas_que_seguram_csf`: a comparação deve selecionar o universo exportado e confrontar seus resultados. Há leitor disponível para identificar esse universo. (`app/docs/CORTES.md:487`; `app/ponto/supra_juiz.py:561`; `app/ponto/supra_juiz.py:573`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:292`; `app/chamados/services/vigia_fio.py:786`)

2. **Avais vigentes:** pelo texto recebido, substituir a comparação estática pela transição completa atende à devolução anterior e mantém a guarda de exportada; não dispensa os demais selos de conservação dos complementos. Conferência dos arquivos originais **[nao verificado]**.

3. **Literal — o que comparar na main:** compara-se a tarefa antes/depois da **operação de ressuscitar**, sem exigir o compensador novo na main. A operação já preserva RESPOSTA/VALIDACAO e limpa a projeção nesta cópia; o compensador é justamente o acréscimo proposto. Existência e assinatura da operação e da tarefa numa main distinta **[nao verificado]**. (`app/chamados/models.py:1633`; `app/chamados/models.py:1651`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:281`)

   A prova deve confrontar os quatro resultados integrais — main antes/depois e candidata antes/depois — com entradas equivalentes, relógio controlado e cargas novas. Reutilizar a carga pode devolver dados anteriores; o teste atual compara somente razão/ações de uma instância. Igualdade integral e cobertura das 2.025 medidas **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:597`; `app/chamados/services/tarefa_do_dia.py:739`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`)

4. **Exportada/dinheiro/!:** comparar também a lista completa do CSF evita esconder troca de perguntas sob um booleano igual. Esse caminho pode permitir retratação; nenhuma mudança em exportada fica autorizada pelo teste. Neutralidade efetiva **[nao verificado]**. (`app/ponto/supra_juiz.py:575`; `app/ponto/supra_juiz.py:588`; `app/ponto/management/commands/supra_juiz.py:261`; `app/ponto/management/commands/supra_juiz.py:286`; `app/docs/CORTES.md:487`)

VEREDITO por item: 1 CONFERE; 2 CONFERE

## 17/11 10:51 · sessao 49afd70744ef · - · - · origem=Ronald (tmux)

Premissas e lacunas da porta, com evidência:

- **Escritor e leitor únicos conferem no censo funcional:** comando importa `medir, lavrar`, mede e chama `lavrar`; view importa e chama somente `conferir`. O auxiliar de teste também chama `lavrar` e se declara exclusivo de testes. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:92`, `app/folha/views.py:128`, `app/folha/porta_export.py:870`, `app/folha/porta_export.py:879`)

- **Item 3 cabe sem migration, precisamente no campo `MetricaSnapshot.chave`:** `CharField(max_length=60)`, integrante da restrição única com `escopo`, `escopo_id`, `data_ref`. Exemplo de chave de competência: `porta_export_leitores:2026-11`, mantendo `escopo='empresa'` e `escopo_id=empresa.pk`. Essa é viabilidade inferida do esquema, exige alterar consistentemente escritor e leitor e os testes; não precisa alterar `detalhe` como identidade. Hoje mês e ano estão apenas em `defaults.detalhe`, portanto A e B sobrescrevem a mesma linha diária. (`app/inteligencia/models.py:87`, `app/inteligencia/models.py:104`, `app/folha/porta_export.py:772`, `app/folha/porta_export.py:776`, `app/folha/porta_export.py:807`, `app/folha/porta_export.py:815`)

- **Item 2 estado (c), N literal, não fecha com o carimbo atual:** `valor` é soma de ocorrências em cinco testemunhas, dias de HE e contador global da competência; não é cardinalidade de colaboradores distintos da empresa. A mensagem atual corretamente chama isso de “ponto(s)”. `cartao_x_txt` pode vir de outra empresa; seus IDs não entram no snapshot. O comando subordinado oferece `--ids`, mas `medir` o chama sem isso e só captura o escalar. Portanto exibir `N=m.valor` como “N colaboradores com número diferente entre espelho e folha” seria semanticamente incorreto. (`app/folha/porta_export.py:687`, `app/folha/porta_export.py:667`, `app/folha/porta_export.py:670`, `app/folha/porta_export.py:725`, `app/folha/porta_export.py:753`, `app/folha/porta_export.py:764`, `app/folha/porta_export.py:779`, `app/folha/porta_export.py:865`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:23`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:51`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:72`)

- **Item 2, hora de conclusão tem campo:** `MetricaSnapshot.calculado_em` usa `auto_now=True`; pode informar a hora da lavratura. Duração real em produção permanece [nao verificado]. (`app/inteligencia/models.py:96`)

- **Item 2, três estados não cobrem falha:** o código distingue ausência, competência incompatível, “não pude medir” (`valor<0`) e divergência (`valor>0`). Se `medir` lança, o comando termina antes de `lavrar`; se retorna `cartao_x_txt=None`, lavra `-1` e termina. Falha de worker não deve ficar eternamente “EM CONFERÊNCIA” nem virar falsa divergência de colaboradores. A decisão literal do anexo para isso é [nao verificado]. (`app/folha/porta_export.py:811`, `app/folha/porta_export.py:815`, `app/folha/porta_export.py:818`, `app/folha/porta_export.py:821`, `app/folha/porta_export.py:774`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:88`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:92`)

- **Itens 1/4/5, escopo do mesmo command não fecha pronto:** ele só aceita `--mes` e `--ano` e percorre todas as empresas `ativa=True`; não aceita empresa, não filtra `em_rollout`, não escolhe competência aberta e não tem trava no próprio comando. Manter esse comando exige ampliar explicitamente a seleção, conservando `lavrar` como único escritor. (`app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:27`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:86`)

- **SELO/R1 hoje falhariam:** ausência, incompatibilidade e erro de medição retornam mensagem com `Rode:` e `docker exec`; view passa a mensagem intacta, resposta a armazena e template imprime `{{ m }}`. O teste vigente chega a exigir que a recusa nomeie o command. (`app/folha/porta_export.py:809`, `app/folha/porta_export.py:813`, `app/folha/porta_export.py:816`, `app/folha/porta_export.py:819`, `app/folha/views.py:133`, `app/core/respostas.py:69`, `app/templates/folha/exportar.html:10`, `app/folha/tests/test_porta_do_export.py:48`)

- **R2 tem base de porta, mas não prova o novo fluxo completo:** teste existente assegura que carimbo zero faz `conferir` retornar vazio; montagem depende ainda de integração, fechamentos, classificação, linhas e layout. (`app/folha/tests/test_porta_do_export.py:508`, `app/folha/views.py:138`, `app/folha/views.py:163`, `app/folha/views.py:268`, `app/folha/views.py:270`, `app/folha/views.py:275`)

- **R3 tem base de recusa, sem o estado novo:** teste existente exige `TXT RECUSADO` com lista; `valor>0` recusa e a view para antes da montagem. Estado (c), aviso ao suporte e linha critical no fluxo novo permanecem [nao verificado]. (`app/folha/tests/test_porta_do_export.py:50`, `app/folha/porta_export.py:821`, `app/folha/porta_export.py:865`, `app/folha/views.py:130`)

- **R4 hoje falha por construção:** A e B atualizam mesma identidade e `conferir(A)` reprova depois de B; teste atual cobre apenas recusa de outra competência, não coexistência A/B. (`app/folha/porta_export.py:772`, `app/folha/porta_export.py:815`, `app/folha/tests/test_porta_do_export.py:514`)

- **“Medir dentro do request” deve ser interpretado com precisão:** medição do selo é vedada pelos comentários atuais; depois da porta a montagem vigente já executa `_recalc(...somente_leitura=True)` sincronicamente, com justificativa explícita. Mover ou reescrever essa montagem extrapolaria o anexo. (`app/folha/views.py:124`, `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:36`, `app/folha/views.py:182`, `app/folha/views.py:207`)

- **Exportada/dinheiro:** `medir` lê autoridades e tem tratamento de exportada; a escrita explícita da porta é snapshot. Depois da guarda a view existente aprova fechamentos, monta conteúdo e registra exportação; conservar esses efeitos e as guardas evita mudança fora da porta. Ausência de efeitos indiretos em todos os leitores transitivos: [nao verificado]. (`app/folha/porta_export.py:316`, `app/folha/porta_export.py:341`, `app/folha/porta_export.py:422`, `app/folha/porta_export.py:772`, `app/folha/views.py:246`, `app/folha/views.py:268`, `app/folha/views.py:281`, `app/folha/views.py:297`)

## 17/11 10:51 · sessao f13d2f17017c · RESP-1050 · verificador · origem=Code

**Item 6 — CONFERE.** A inclusão corrige a lacuna de encaminhamento e pode usar o juiz existente, sem criar outra autoridade. `app/ponto/management/commands/e6_oraculo.py:53`; `app/escala/services/cadastro_realidade.py:1047`.

1. **Premissas:** conferem como diagnóstico. `dono_da_divergencia` ainda não recebe FATO_EM_AUSENCIA; `InsumosDoDono.dono()` não transporta essa classificação. A9 exige `ausencia_id` e pelo menos duas batidas, reduz as datas a contagens por colaborador e chega à lista sem dia/dono diário, com ação “em cura” sem URL. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:182`; `app/ponto/services/esmeril_espelho.py:133`; `app/ponto/services/esmeril_espelho.py:222`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:728`.

   A rota diária existe: `lista()` → `dias_na_lista()` → `ListaDoAdmin.dia_do_admin()` → `fio_do_admin`. Exige assinatura com `data` e dono CADASTRO/BATIDA; o A9 agregado não satisfaz esse contrato. `app/escala/services/cadastro_realidade.py:1047`; `app/chamados/services/vigia_fio.py:190`; `app/chamados/services/vigia_fio.py:200`; `app/chamados/services/vigia_fio.py:261`.

2. **Colisão:** não há, considerando o corte CADASTRO fornecido no RESPOSTAS-1711-1005. L-099 destina CADASTRO/BATIDA à lista; ampliar os insumos do mesmo juiz preserva a autoridade única. O próprio juiz já recebeu extensão semelhante para ler fase lavrada. `app/docs/LEIS.md:122`; `app/ponto/management/commands/e6_oraculo.py:90`. O corte novo e a proibição específica do brief foram fornecidos no pedido, sem arquivo local verificável. `[nao verificado]`

3. **Literal:** fecha tomando “caso” como a classificação existente FATO_EM_AUSENCIA. Não basta ausência coexistir com batida: parcial sem período tem código próprio; parcial fora do período não é FATO; também existe FATO com batida sem par, além do alcance do A9. `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:417`; `app/ponto/supra_juiz.py:450`.

   Ao conjunto de arquivos a alterar, a inclusão acrescenta **`app/ponto/management/commands/e6_oraculo.py`**, pelo juiz e carregador; **`app/escala/services/cadastro_realidade.py`** já integrava a cura C5 e ganha o produtor diário e seu encaminhamento. Fixar CADASTRO apenas na lista deixaria o juiz divergente. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:182`; `app/docs/RELATO.md:421`; `app/escala/services/cadastro_realidade.py:978`.

   O cruzamento de impacto deve abranger também consumidores do juiz: `teto_na_lista.py`, `cxr_na_lista.py`, `pautas_discordantes.py`, `censo_fase_12x36.py` e `listar_ferias_com_batida.py`; isso não demonstra necessidade de editá-los. `app/ponto/services/teto_na_lista.py:72`; `app/ponto/services/cxr_na_lista.py:99`; `app/ponto/management/commands/pautas_discordantes.py:102`; `app/ponto/management/commands/censo_fase_12x36.py:287`; `app/ferias/management/commands/listar_ferias_com_batida.py:53`. Arquivos exatos de testes e cruzamento atual com B2+delta/F2: `[nao verificado]`.

4. **Exportada, dinheiro ou `!`:** o encaminhamento demonstrado é leitura da lista e classificação do destino; não executa a correção administrativa. `app/chamados/services/vigia_fio.py:194`; `app/chamados/services/vigia_fio.py:261`. Corrigir ausência pode acordar projeções; descartar batida deve usar retratação, que preserva a linha e provoca atualização do dia. Eventos podem recalcular competência exportada. Portanto, não há certificado de dinheiro zero nem autorização implícita para esses atos. `app/ponto/services/ausencia.py:1123`; `app/ponto/registro_batida.py:287`; `app/ponto/registro_batida.py:297`; `app/ponto/services/fechamento.py:1790`; `CLAUDE.md:265`. Alcance real em exportadas: `[nao verificado]`.

**Item 7 — CONFERE.** A regra pode acrescentar a leitura antecipada e usar a classificação de postmortem existente, preservando o opus final. `CLAUDE.md:376`; `CLAUDE.md:378`; `CLAUDE.md:383`.

1. **Premissas:** a triagem e a medida estão nos itens 5 e 6 da seção CODEX: descarte exige motivo, segue anexo ao opus e entra no postmortem por obra. `CLAUDE.md:375`; `CLAUDE.md:382`.

   Os scripts têm papéis distintos: o disjuntor registra **voltas**, inclusive quando uma resposta contém vários achados; grava data/obra/volta/veredito, sem motivo ou prova por achado. `gerar_codex_estado.py` publica respostas finais dos JSONL; `gerar_estado.py` delega o painel ao disjuntor. Não constituem um contador automático de “visto e descartado”. `bin/disjuntor.py:869`; `bin/disjuntor.py:910`; `bin/gerar_codex_estado.py:77`; `bin/gerar_codex_estado.py:132`; `bin/gerar_estado.py:277`. O alerta de descartes em 24 horas permanece registrado no backlog. `app/docs/BACKLOG.md:1094`. A comprovação independente dos “4 dos 5” da F2 não foi fornecida. `[nao verificado]`

2. **Colisão:** não há se a leitura antecipada for adicional e restrita aos descartes por argumento. SUITE-ANTES-DO-OPUS exige verde antes da **leitura final**; continuam obrigatórios o opus final e o anexo de **todos** os descartes, inclusive os sustentados por prova. A leitura antecipada não dispensa essas etapas. `app/docs/PISTA.md:113`; `CLAUDE.md:356`; `CLAUDE.md:376`; `CLAUDE.md:378`; `CLAUDE.md:381`.

3. **Literal e contador:** fecha pela classe **(b), “viu e a triagem descartou”**, entre os achados da reprovação do opus. Quem classifica é o **Codex, papel censo**, recebendo achados do opus, todas as perguntas/respostas `.q/.r` e linhas de triagem; a tabela vai ao RELATO e alimenta a medida publicada pelo Code. Assim, esperado zero se refere a essa falha de triagem, não ao total de descartes legítimos. `CLAUDE.md:383`; `CLAUDE.md:385`; `CLAUDE.md:388`; `app/docs/PISTA.md:124`.

   Contar linhas `descartado` do disjuntor produziria outra métrica: a unidade ali é volta, e prevalece o último veredito. A fonte, unidade, universo e exclusões devem ficar explícitos; eventual automação exige contrato de instrumento. `bin/disjuntor.py:871`; `bin/disjuntor.py:922`; `CLAUDE.md:192`; `CLAUDE.md:394`.

4. **Exportada, dinheiro ou `!`:** o recorte demonstrado é processo de revisão, classificação e publicação; não inclui alteração de cálculo ou dado de negócio. `CLAUDE.md:375`; `CLAUDE.md:382`; `bin/gerar_codex_estado.py:156`. Implementação futura e efeitos financeiros medidos: `[nao verificado]`.

VEREDITO por item: 6 CONFERE|PROCEDE; 7 CONFERE|PROCEDE

## 17/11 10:51 · sessao b82fe6b8e43b · - · - · origem=Ronald (tmux)

- **Teto:** regra vigente é teto 2, 1 opus + 1 Codex; exceção nominal do terceiro está no anexo fornecido, sem linha local conferida. Não configura colisão se restrita a esta obra e ao construtor opus declarado. (`CLAUDE.md:291`; `app/docs/LEIS.md:147`; exceção local: [nao verificado])

- **Item 1:** trabalho longo deve ir para job; medir dentro do POST é explicitamente rejeitado pela porta e pela view. Chamar o mesmo comando preserva o escritor `lavrar`; executar `medir` diretamente na view contrariaria o literal. (`CLAUDE.md:89`; `app/folha/porta_export.py:7-12`; `app/folha/views.py:124-133`; `app/relatorios/management/commands/selo_leitores_no_mesmo_numero.py:33`, `:86-92`)

- **Item 2:** estados visíveis e componente da casa atendem L5 e L8 da UI; tela e botão precisam ler a mesma autoridade. O contador “N colaboradores” exige fonte com essa unidade: `valor` atualmente representa pontos de falha, não pessoas. (`app/docs/LEIS-UI.md:23-29`, `:56-66`, `:87-93`; `app/folha/porta_export.py:865-866`; `app/docs/LEIS.md:61`) O componente `_dialogo.html` é compartilhado pelas duas cascas; alterá-lo exigiria smoke de clique nas duas, e sua intocabilidade no COMPLEMENTO-2 UI-VINCULO está registrada no RELATO. Mostrar estados locais não exige alterá-lo. (`app/templates/core/_dialogo.html:12`; `CLAUDE.md:202-203`; `app/docs/RELATO.md:51`, `:152`)

- **Item 3:** preservar A após B corrige a identidade hoje insuficiente: competência está em `detalhe`, enquanto o lookup usa chave, escopo, empresa e dia. Lei do escopo literal permanece aplicável; decidir qual campo compatível usar é encaixe técnico autorizado, sem ampliar o aval. (`app/folha/porta_export.py:772-776`, `:807-817`; `app/docs/LEIS.md:62`; `CLAUDE.md:276-280`)

- **Item 4:** desabilitar botão é requisito de feedback, não prova de exclusão concorrente no servidor; precisa preservar escritor único e assegurar idempotência. (`app/docs/LEIS-UI.md:56-60`; `CLAUDE.md:32-34`, `:249`) Existência da trava específica: [nao verificado].

- **Item 5 / ZONA:** `config/crons.py` é fonte única, instalação deve passar por `bin/crons.sh install`, com backup; cron deve ser idempotente e declarar seu papel. A exclusão atual é decisão documentada “SELO DE PORTA”, que o novo aval manda substituir. (`CLAUDE.md:239-249`; `app/config/crons.py:1-14`, `:887-897`; `app/docs/LEIS.md:94`, `:127`) `app/config/crons.py` é proibido ao **Codex construtor pelo wrapper**; o anexo designa construtor opus, portanto essa zona não impede a execução pelo papel declarado. (`bin/codex_pergunta.sh:343-347`) Juízes concretos de competência aberta/em_rollout: [nao verificado].

- **Leis por sítio:** a busca exigida é LEIS/DOSSIES/CORTES; `porta_export.py` aparece em L-094, L-095 e L-097; crons em L-060/L-101. O corte sobre `folha/views.py` mantém turno aberto barrando o **carimbo da porta cega**, cuja regra não pode ser alterada por esta obra. (`CLAUDE.md:190`; `app/docs/LEIS.md:112-115`, `:94`, `:127`; `app/docs/CORTES.md:317`; `app/docs/DOSSIES.md:88`, `:362`)

- **R1-R6 e SELO:** nenhuma colisão normativa no objetivo; RED com mordida e adversarial são exigências vigentes, e saída renderizada precisa de caso que morde. R2 depende das outras guardas existentes da montagem, portanto selo zero não basta sozinho para gerar qualquer fixture. (`CLAUDE.md:28-29`, `:193-194`; `app/folha/views.py:134-153`) Execução dos RED/mutantes e allowlist vazia: [nao verificado].

- **Exportada/dinheiro/!:** escrever só estado de job e snapshot não é autorização para recalcular o gravado, alterar TXT, escala ou vínculo. Alterar `FechamentoMensal` exportado exige `!` e porta própria; dinheiro em competência não exportada tem requisitos próprios de reversão e prova de frota. (`CLAUDE.md:265-268`, `:406-417`; `app/docs/LEIS.md:125`) Efeitos atuais de `medir` sobre dados financeiros: [nao verificado].

## 17/11 10:50 · sessao ed7678d8f2c6 · - · - · origem=Ronald (tmux)

Recomendo **CONFERE nos dois itens como revisão da coerência do aval**, preservando expressamente as limitações de evidência.

- **Item 1:** filtrar exclusivamente o compensador ao montar `_dec` mantém disponível o ato anterior para os dois usos de último ato, inclusive RETRATAÇÃO (`app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:260`). A exceção nominal consta do anexo fornecido; original em arquivo [nao verificado].

- **Item 2:** “mesmo ressuscitar” pode executar a operação antiga na main e a operação com compensador na candidata; não exige produzir o novo ato na main. A operação já existe na cópia, e o teste demonstra controle do relógio (`app/chamados/models.py:1579`; `app/chamados/models.py:1633`; `app/chamados/models.py:1651`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:325`). Existência e assinatura numa main independente [nao verificado].

- **Selo:** comparar o resultado integral com cargas novas é tecnicamente possível; o teste atual compara somente razão e ações de uma instância (`app/chamados/services/tarefa_do_dia.py:733`; `app/chamados/services/tarefa_do_dia.py:739`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`). Igualdade integral efetiva nas duas árvores e nas 2.025 unidades [nao verificado].

- **Exportada:** a guarda é normativa; os leitores necessários à comparação são chamáveis. Os métodos CSF não contêm seleção automática por competência exportada (`app/docs/CORTES.md:487`; `app/ponto/supra_juiz.py:561`; `app/ponto/supra_juiz.py:573`; `app/ponto/supra_juiz.py:578`; `app/ponto/supra_juiz.py:588`). Neutralidade efetiva [nao verificado].

A falta de execução do selo não demonstra contradição do aval; também não autoriza afirmar que o selo passou [nao verificado]. [nao verificado]

## 17/11 10:50 · sessao 5fe54c645365 · - · - · origem=Ronald (tmux)

Item 7 — evidências para consolidação:

1. **Premissas parcialmente conferidas.** A triagem vigente pede conferência de arquivo:linha, correção do procedente e motivo escrito para descarte; todo descarte segue como anexo ao opus. A medida “opus reprovou depois do Codex aprovar” é publicada por obra no RELATO, com classificação independente pelo Codex. `CLAUDE.md:375-388`. [nao verificado]

   O `disjuntor` registra **voltas**, não achados: uma resposta com N achados vale uma volta; `triagem.log` guarda somente data, obra, identificação da volta e veredito. Não guarda motivo, prova, autor do descarte ou vínculo com achado do opus. `bin/disjuntor.py:869-872`; `bin/disjuntor.py:910-912`; `bin/disjuntor.py:945-948`.

   `gerar_codex_estado.py` publica respostas finais dos JSONL; `gerar_estado.py` compõe painéis delegando ao disjuntor. Nenhum desses mecanismos é fonte suficiente para contar “visto e descartado”. `bin/gerar_codex_estado.py:77-103`; `bin/gerar_codex_estado.py:132-159`; `bin/gerar_estado.py:277-283`; `bin/disjuntor.py:2233-2246`.

   O alerta “descartes do Codex nas últimas 24 h” continua registrado como obra de instrumento. `app/docs/BACKLOG.md:1094`. O número motivador “4 dos 5” consta apenas do anexo fornecido; a demonstração independente não foi fornecida. `[nao verificado]`

2. **Não há colisão necessária:** a leitura antecipada, restrita aos descartes por argumento, pode ser adicional. Permanecem a suíte inteira verde antes do **opus final**, a obrigação de realizar o final e o envio de **todos** os descartes como anexo a esse final. A antecipada não pode substituir essas obrigações. `CLAUDE.md:356`; `CLAUDE.md:376-381`. [nao verificado]

3. **O literal fecha pela classificação já existente**, se “visto e descartado” significar a classe **(b)** dos achados da reprovação do opus, e não todo descarte legítimo do Codex. Quem classifica é o Codex, papel `censo`, recebendo achados do opus, todas as `.q/.r` e linhas de triagem; a tabela vai ao RELATO e alimenta a medida, publicada pelo Code. `CLAUDE.md:383-388`; `app/docs/PISTA.md:124-127`.

   A fonte não pode ser uma contagem de linhas `descartado` do disjuntor: sua unidade é volta e o último veredito prevalece. `bin/disjuntor.py:871-872`; `bin/disjuntor.py:922`; `bin/disjuntor.py:948`.

   A especificação da métrica deve explicitar fonte, unidade, universo e exclusões; eventual automação constitui instrumento e deve cumprir seu contrato próprio. Isso não demonstra exigência de automatização pelo anexo. `CLAUDE.md:192`; `CLAUDE.md:394`; `[nao verificado: exigência de automação no anexo]`.

4. **Não identifica alteração de negócio/exportada/dinheiro no recorte da regra de processo.** A implementação futura e seus efeitos não foram fornecidos. `[nao verificado]` O escopo demonstrado hoje é triagem, classificação e publicação de respostas/painéis. `CLAUDE.md:375-391`; `bin/disjuntor.py:910-912`; `bin/gerar_codex_estado.py:156`; `bin/gerar_estado.py:281`.

**Conclusão recomendada: item 7 CONFERE**, como extensão da conduta, preservando o opus final e usando a classe (b) existente como fonte do novo campo. Não atribuir ao código atual um contador automático que ele não demonstra. `CLAUDE.md:376-388`; `bin/disjuntor.py:869-872`; `bin/gerar_codex_estado.py:132-159`.

## 17/11 10:49 · sessao 1b461d96035a · - · - · origem=Ronald (tmux)

**Item 6: CONFERE**, como inclusão futura do caso no juiz existente; a rota prometida ainda não está implementada nesta cópia. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:182`; `app/escala/services/cadastro_realidade.py:927`.

1. **Premissas conferem.** O juiz único é `dono_da_divergencia`; sua assinatura não recebe FATO_EM_AUSENCIA e `InsumosDoDono.dono` não carrega esse veredito. Hoje ele pode devolver BATIDA ou ESTRUTURA para esse recorte. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:98`; `app/ponto/management/commands/e6_oraculo.py:182`.

   A9 é sinal distinto, com dono literal `juiz`, formado por `ausencia_id` e pelo menos duas batidas; sua lavra reduz datas a contagens por colaborador. Na lista ele entra sem `data` e sem `dono_do_dia`, caindo na ação sem URL “Com o sistema: em cura”. `app/ponto/services/esmeril_espelho.py:23`; `app/ponto/services/esmeril_espelho.py:133`; `app/ponto/services/esmeril_espelho.py:222`; `app/escala/services/cadastro_realidade.py:923`; `app/escala/services/cadastro_realidade.py:728`.

   A rota comprovada é `lista` → `dias_na_lista` → `ListaDoAdmin.dia_do_admin` → `fio_do_admin`: exige assinatura datada e dono CADASTRO/BATIDA. `app/escala/services/cadastro_realidade.py:1047`; `app/chamados/services/vigia_fio.py:190`; `app/chamados/services/vigia_fio.py:200`; `app/chamados/services/vigia_fio.py:261`.

   Há exposição administrativa restrita já existente para veredito `fato_em_ausencia`, dia sem trabalho, não exportado e sem pergunta viva; isso não é a rota diária geral da lista. `app/chamados/services/tarefa_do_dia.py:352`.

2. **Não há colisão necessária com L-099 ou com “juízes novos 0”.** L-099 já nomeia esse juiz e manda CADASTRO/BATIDA à lista. Acrescentar um insumo e ramo ao mesmo juiz preserva uma autoridade; derivar novamente FATO na lista ou fixar o dono só nela duplicaria a decisão. `app/docs/LEIS.md:122`; `app/ponto/management/commands/e6_oraculo.py:53`; `app/escala/services/cadastro_realidade.py:978`.

   A proibição específica da CLASSE-D consta do brief fornecido, sem arquivo local numerado verificado. `[nao verificado]`

3. **O literal fecha**, se “caso” significar a classe já lavrada `fato_em_ausencia`, não qualquer ausência com batida. O suprajuiz separa parcial sem período, parcial fora do período e FATO; também classifica FATO com batida sem par, que A9 não representa integralmente. `app/ponto/supra_juiz.py:392`; `app/ponto/supra_juiz.py:395`; `app/ponto/supra_juiz.py:403`; `app/ponto/supra_juiz.py:413`; `app/ponto/supra_juiz.py:450`; `app/escala/models.py:1619`.

   Ao cruzamento da obra deve entrar **`app/ponto/management/commands/e6_oraculo.py`**, contendo juiz e montagem de insumos; **`app/escala/services/cadastro_realidade.py`** permanece, agora também pelo produtor diário de FATO, ação e resolução comum do dono. `app/ponto/management/commands/e6_oraculo.py:53`; `app/ponto/management/commands/e6_oraculo.py:182`; `app/escala/services/cadastro_realidade.py:869`; `app/escala/services/cadastro_realidade.py:891`; `app/escala/services/cadastro_realidade.py:968`.

   `vigia_fio.py` é consumidor a cruzar, sem necessidade demonstrada de editá-lo. Alterações em `esmeril_espelho.py`, `supra_juiz.py` e arquivos exatos de testes dependem do desenho; não são impostas pelo literal caso se consuma o veredito existente. `app/chamados/services/vigia_fio.py:174`; `app/escala/models.py:1619`; `[nao verificado]`

   A lista atual de arquivos de B2+delta/F2 e cruzamento efetivamente zero não são demonstrados pelo material fornecido. `[nao verificado]`

4. **A inclusão é leitura/encaminhamento; não autoriza correção de dados ou ato financeiro.** O vigia declara ausência de escrita/recalculo e a lista publica ações. As ações posteriores têm efeito financeiro: alterar ausência acorda projeção; retratar batida preserva a linha, retira-a da apuração e agenda fechamento. `app/chamados/services/vigia_fio.py:269`; `app/escala/services/cadastro_realidade.py:912`; `app/ponto/services/ausencia.py:987`; `app/ponto/services/ausencia.py:1123`; `app/ponto/registro_batida.py:287`; `app/ponto/registro_batida.py:297`.

   Não cabe certificar exportadas protegidas genericamente: o evento pode recalcular competência exportada, com trilha. Tocar gravado exportado, escala/vínculo ou motor continua na lista explícita de `!`. `app/ponto/services/fechamento.py:1790`; `app/docs/LEIS.md:155`; `CLAUDE.md:265`.

## 17/11 10:49 · sessao 2b999944b5ce · - · - · origem=Ronald (tmux)

- A guarda vigente é normativa: “supra_juiz em competência exportada não muda; se o DIFF mostrar que muda, PAREI ! com a tabela” (`app/docs/CORTES.md:487`). Os complementos fornecidos mantêm essa obrigação; conferência de originais dos anexos [nao verificado].

- O leitor existe e é chamável: `perguntas_que_seguram_csf(ch)` devolve a lista ordenada das perguntas que seguram CSF, reunindo origem e destino por `sem_veredito_entre(..., ignorar_tranca=True)`; `pode_executar_csf(ch)` devolve a negação dessa lista (`app/ponto/supra_juiz.py:561`, `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:575`, `app/ponto/supra_juiz.py:578`, `app/ponto/supra_juiz.py:588`).

- Já há adaptador de comparação no selo F2: `_csf(ch,p)` chama o leitor real e mede pertencimento da pergunta; integra os sete leitores registrados (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:292`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:320`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:339`). Para a guarda integral, comparar a lista completa e o booleano evita deixar passar mudança de identidade com booleano igual; implementação dessa comparação integral exportada [nao verificado].

- **Não existe guarda automática de exportada nesses dois métodos**: recebem somente `ch`, e a seleção mostrada não testa competência exportada; a guarda do CSF é de conversa sem veredito, não de competência (`app/ponto/supra_juiz.py:561`, `app/ponto/supra_juiz.py:573`, `app/ponto/supra_juiz.py:578`, `app/ponto/supra_juiz.py:588`). A afirmação precisa ser “o leitor necessário ao selo é chamável”, e não “supra possui um `if exportada` pronto” (`app/ponto/supra_juiz.py:573`).

- O universo exportado também tem leitores chamáveis: `competencia_do_dia_entregue(dia,empresa,memo=None)` compõe janela da empresa e juiz `competencia_entregue`, retornando `None` sem dia; `empresas_exportadas_no_escopo(mes,ano,empresa_id=None,colaborador_ids=None)` retorna IDs por leitura (`app/chamados/services/vigia_fio.py:786`, `app/chamados/services/vigia_fio.py:798`, `app/chamados/services/vigia_fio.py:802`, `app/ponto/services/fechamento.py:1496`, `app/ponto/services/fechamento.py:1515`).

- O compensador pode alcançar CSF porque `sem_veredito_entre` lê resposta/validação pela sequência; requer resposta vigente, nenhuma validação e nenhuma materialização. Logo a igualdade da tarefa não basta para provar conservação desse caminho (`app/chamados/models.py:1953`, `app/chamados/models.py:1954`, `app/chamados/models.py:1955`, `app/ponto/supra_juiz.py:573`). O executor consulta a guarda antes de retratar, mostrando consequência operacional possível (`app/ponto/management/commands/supra_juiz.py:257`, `app/ponto/management/commands/supra_juiz.py:261`, `app/ponto/management/commands/supra_juiz.py:286`).

- Não foi medida neutralidade financeira/exportada nem executado selo; resultados de produção, total de exportadas alcançadas e DIFF zero [nao verificado].

## 17/11 10:49 · sessao b11b33339ec1 · - · - · origem=Ronald (tmux)

- A **operação** já existe na cópia como `ChamadoColaborador.ressuscitar_perguntas(self)`; ela preserva linhas de resposta/validação e limpa a projeção. A ausência da nova REABERTURA compensatória não significa ausência de ressuscitar (`app/chamados/models.py:1579`; `app/chamados/models.py:1633`; `app/chamados/models.py:1651`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:281`).

- A tarefa recebe relógio explicitamente: `tarefa_do_dia(colab, dia_do_turno, agora, *, carga=None)`. Ressuscitar não recebe `agora`, mas o teste existente demonstra fixação de `timezone.now()` na operação; isso torna possível usar o mesmo relógio sem mudar sua assinatura (`app/chamados/services/tarefa_do_dia.py:733`; `app/chamados/services/tarefa_do_dia.py:736`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:325`).

- O comparando correto na main é **a tarefa antes/depois da operação antiga**, sem exigir que ela produza o novo compensador. Na árvore candidata, compara-se a tarefa antes/depois da operação acrescida do compensador; depois confrontam-se os respectivos resultados das versões. A cópia fornece ambas as portas necessárias para esse desenho, mas a existência e a assinatura delas numa main distinta permanecem **[nao verificado]** (`app/chamados/models.py:1579`; `app/chamados/services/tarefa_do_dia.py:733`). Não existe metadado Git acessível nesta cópia; identidade de uma main externa **[nao verificado]**.

- Reusar a carga anterior invalidaria a prova: o construtor materializa perguntas e decisões, e a tarefa mantém memoização. É necessário abrir carga nova a cada leitura; a chamada pública sem `carga` já o faz (`app/chamados/services/tarefa_do_dia.py:406`; `app/chamados/services/tarefa_do_dia.py:421`; `app/chamados/services/tarefa_do_dia.py:575`; `app/chamados/services/tarefa_do_dia.py:739`).

- O teste existente não satisfaz “tarefa inteira”: extrai somente razão e ações de uma instância. O resultado integral também contém os demais campos da tarefa, incluindo rota, fila, verbos e fio (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:316`; `app/chamados/services/tarefa_do_dia.py:42`; `app/chamados/services/tarefa_do_dia.py:45`).

- Portanto, a proposta é **tecnicamente executável pelas interfaces da cópia**, sem necessidade de existir o mesmo compensador na main; sua executabilidade concreta em **duas árvores identificadas**, igualdade integral e cobertura das 2.025 medidas permanecem **[nao verificado]** (`app/chamados/models.py:1579`; `app/chamados/services/tarefa_do_dia.py:733`).

## 17/11 10:34 · sessao 3694bf491c94 · E4C-F2 · leitura · origem=Code

**F2 toca menos leitores: 1, desde que a origem identifique exclusivamente a ressuscitação.** `REABERTURA` já desfaz resposta e validação na leitura, na face SQL e no histórico; a adaptação fica na seleção das decisões da tarefa do dia. Identificar esse ato apenas por “usuário sistema” quebraria a reabertura de ausência rejeitada, que deliberadamente devolve ao colaborador. (`app/chamados/models.py:4191`, `app/chamados/models.py:4235`, `app/chamados/models.py:3500`, `app/chamados/models.py:3750`; `app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1167`)

F2 **não existe pronta** nessa cópia: `DecisaoPergunta` não tem campo `origem`; tem `ato_por`, `ato_motivo` e os demais campos da decisão. A distinção precisaria ser persistida nominalmente pelo escritor de ressuscitar, sem inferi-la do autor nem acrescentar outro significado a `ato_motivo`. (`app/chamados/models.py:4110`, `app/chamados/models.py:4115`, `app/chamados/models.py:4195`)

**(i) Censo dos leitores**

A tabela separa leitores que interpretam diretamente o tipo do ato dos consumidores que recebem seu resultado; essa distinção fundamenta a contagem de leitores tocados. (`app/chamados/models.py:3500`, `app/chamados/models.py:4230`; `app/chamados/services/tarefa_do_dia.py:158`)

| Leitor direto | O que lê hoje | F1: tipo próprio, sem adaptação | F2: `REABERTURA`, sem adaptação |
|---|---|---|---|
| `DecisaoPergunta.leitura` | `RESPOSTA` grava instante/texto; `VALIDACAO` grava validação; `DEVOLUCAO` limpa validação; `RECUSA/REABERTURA` limpam resposta inteira e validação; `SILENCIO/RETRATACAO/ABSORCAO` não alteram esses três valores. | Ignora o tipo novo; resposta/validação anteriores continuam vigentes. | Limpa resposta inteira e validação, independentemente do autor. (`app/chamados/models.py:4191`, `app/chamados/models.py:4230`) |
| `q_validada_pelo_ato` e sua negação | Procura `VALIDACAO` sem ato posterior que a desfaça; inclui o passivo sem linha correspondente. Usa o mesmo conjunto `_LIMPAM_VALIDACAO` da leitura. | Não desfaz a validação anterior. | Desfaz a validação anterior. (`app/chamados/models.py:3491`, `app/chamados/models.py:3500`, `app/chamados/models.py:3513`) |
| `respostas_anteriores` | `RESPOSTA` acompanha a resposta vigente; `RECUSA/REABERTURA`, integrantes de `ATOS_COM_FOTO`, expõem a foto retirada e encerram a vigente. | Ignora o novo tipo; seria necessário reconhecer a retirada da resposta. | Reconhece a foto e encerra a vigente. (`app/chamados/models.py:3745`, `app/chamados/models.py:3750`, `app/chamados/models.py:4108`) |
| Tarefa: carregamento, `_tarefa_pura` e `_pela_decisao` | Seleciona a última decisão até `agora`. `RESPOSTA` distribui entre responder/validar conforme texto; `RECUSA/REABERTURA` devolvem ao colaborador; `DEVOLUCAO` volta ao admin; `VALIDACAO` confere lavra/efeito; `SILENCIO` não exige ação; `ABSORCAO` segue o estado da ausência. Última `RETRATACAO` retira a pergunta da tarefa. | Encobre a decisão anterior e cai em `ato_desconhecido`, com ações indeterminadas. | Encobre a decisão anterior e vira `devolvida_ao_colaborador`. **Ambas exigem adaptação.** (`app/chamados/services/tarefa_do_dia.py:156`, `:159`, `:163`, `:167`, `:176`, `:178`, `:187`, `:259`, `:421`) |
| `linhas_da_projecao_sem_linha` | Só considera correspondência de `RESPOSTA/VALIDACAO` pelo mesmo instante; produz as linhas antigas que faltam. | Não interpreta o compensador como resposta/validação. | Idem. Deve continuar preservando o anterior antes da limpeza. (`app/chamados/models.py:4166`, `app/chamados/models.py:4170`, `app/chamados/models.py:4175`) |
| Backfill: `admin_decidiu_sem_ato` e guarda do apply | Só `SILENCIO/VALIDACAO` testemunham a decisão procurada. A seleção exige via `admin_decidiu` e materialização. | Não conta como esse testemunho. | Idem. Ressuscitar limpa a via/materialização, retirando a pergunta desse universo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:99`, `:101`, `:315`; `app/chamados/models.py:1651`) |
| `materializacao.rotulo_bug_par_103` | Só reconhece `DEVOLUCAO` com o prefixo nominal BUG-PAR-103. | Ignora. | Ignora. (`app/chamados/services/materializacao.py:275`) |
| Foto de conservação E4b | Captura todas as decisões, sem selecionar tipo. | Captura a linha nova. | Captura a linha nova. (`app/chamados/services/foto_e4b.py:280`) |

A apresentação do histórico distingue `RESPOSTA` dos demais itens recebidos e mostra texto, foto, rótulo, autor e motivo. F2 já aparece como “Reaberta para correcao”; F1 precisaria primeiro entrar no leitor do histórico. (`app/templates/chamados/partials/modal_fio.html:205`, `:209`, `:212`; `app/chamados/models.py:4103`, `app/chamados/models.py:3750`)

Os consumidores restantes recebem esses resultados:

| Consumidor | Efeito de F1/F2 |
|---|---|
| **Juiz de respondida, C21** | O juiz cru não interpreta ato: exige instante **e** texto após trim; sua face SQL também lê a projeção. A property `respondida` aplica esse juiz à `leitura`. Portanto F1 cru pode continuar respondida pela linha antiga; F2 deixa de estar respondida. (`app/chamados/juizes.py:1552`, `:1580`; `app/chamados/models.py:3735`) |
| **`estado` e `resposta_display`** | Ambos leem a sequência. F1 cru conserva a validação/resposta anterior; F2 permite `PENDENTE` e display vazio após a limpeza, conforme os demais estados da pergunta. (`app/chamados/models.py:3184`, `:3185`, `:3190`, `:3759`) |
| **C6 `pergunta_viva`** | Não lê `DecisaoPergunta`: recebe validação/resposta/materialização da projeção temporal. O compensador isolado não o muda; a limpeza já executada por ressuscitar é que o altera. F1 e F2 têm o mesmo resultado no C6, mantendo essa limpeza. (`app/chamados/juizes.py:1389`, `:1404`, `:1423`) |
| **`q_vivas`** | O degrau da validação usa `q_nao_validada_pelo_ato`; o da resposta usa a projeção. F1 cru pode excluir uma pergunta que o C6 considera viva; F2 remove essa validação antiga do filtro. (`app/chamados/models.py:3547`, `:3562`) |
| **Sem veredito, supra_juiz e irmãs** | `sem_veredito_entre` exige resposta vigente, sem validação nem materialização. F2 elimina a resposta retirada; F1 depende da adaptação de `leitura`. Supra e irmãs delegam a esse mesmo leitor. (`app/chamados/models.py:1953`; `app/ponto/supra_juiz.py:573`, `:574`, `:588`; `app/chamados/juizes.py:2117`) |
| **Fechamento, reconciliação e desligamento** | Herdam a decisão sobre respostas sem veredito; não precisam reconhecer outro tipo de ato. (`app/chamados/models.py:1964`, `:1997`; `app/chamados/reconciliador.py:1166`; `app/colaboradores/services/desligamento.py:165`) |
| **`_ancoradas`, gate e relavra S2** | Filtram validação pela face SQL. F1 cru continua excluindo a validada ressuscitada; F2 a torna novamente elegível aos demais filtros. (`app/chamados/models.py:317`, `:341`, `:357`) |
| **Relavra de via** | Consulta C6 antes de descarimbar e delega a lavra ao leitor existente; não contém outro switch de `DecisaoPergunta`. (`app/chamados/services/relavra_via.py:126`, `:242`, `:246`; `app/chamados/management/commands/relavrar_via_i6.py:102`) |
| **App** | Seleciona por `q_vivas` e depois C6; a partição pendentes/histórico usa C21 sobre a projeção. F2 corrige a exclusão pela validação antiga; F1 exige corrigir a face SQL. (`app/chamados/services/pergunta_no_app.py:52`, `:61`, `:82`) |
| **API** | Serializa `estado` e `resposta_display` derivados da sequência, mas também resposta/instante projetados. Com F2 e projeção limpa, essas faces deixam de conservar a resposta retirada. (`app/api/views.py:2550`, `:2561`, `:2562`, `:2563`; outra exposição em `:1164`) |
| **Fio e templates** | O fio carrega decisões em lote; histórico, estado e resposta são apresentados pelos leitores anteriores. (`app/chamados/services/fio.py:263`; `app/chamados/services/disputa_emissao.py:2123`; `app/templates/chamados/partials/modal_fio.html:205`, `:217`; `app/templates/chamados/partials/_pergunta_card.html:29`) |

Os demais consumidores de `q_vivas` herdam a correção da face SQL: fila de respostas a validar, fio, janela, vigia, raio X, cobrança, app e fechamento do dia. Não precisam acrescentar interpretação própria do novo ato. (`app/chamados/juizes.py:1763`; `app/chamados/services/fio.py:256`; `app/chamados/services/janela_colab.py:63`; `app/chamados/services/vigia_fio.py:711`; `app/chamados/services/raiox.py:74`; `app/chamados/services/cobranca.py:237`; `app/chamados/services/pergunta_no_app.py:53`; `app/ponto/services/veredito_celula.py:181`)

O mascaramento da sombra conhece `ato_texto/ato_motivo`, independentemente do tipo; não decide estado nem autoria humana. (`bin/sombra_regras_pessoais.py:142`)

**(ii) Matriz de ressuscitar**

Há duas restrições essenciais do universo atual: precisa existir materialização, e resposta legal projetada — instante e texto — fica fora por `q_nao_respondida`. Validada com resposta legal também fica fora; validada sem resposta pode entrar. Além disso, a seleção exclui vias fora do fato, protege marco aceso e exige C6 vivo sobre a cópia limpa. (`app/chamados/models.py:1542`, `:1545`, `:1547`, `:1569`, `:1572`, `:1576`)

Na matriz, **R** significa o compensador da ressuscitação: F2 já tem sua semântica em `REABERTURA`; F1 precisaria incorporá-la aos leitores. Os resultados previstos pressupõem pergunta efetivamente selecionada e compensador posterior aos atos que desfaz. (`app/chamados/models.py:4227`, `:4235`, `:4237`; `app/chamados/models.py:1576`)

| Partida | Ato gravado → respondida? → validada? → viva pelo C6? | Tarefa do dia / comparação com main | Prova que deve morder |
|---|---|---|---|
| **Validada, sem resposta legal, elegível** | Preservar a validação antiga que faltar e gravar **R** → **não** → **não** → **sim**, no julgamento que autorizou a seleção. F2 já limpa os dois estados; F1 cru não fecha essa linha. (`app/chamados/models.py:1633`, `:1576`, `:4235`) | O compensador deve ser invisível à seleção da fila. Se o último ato aplicável continuar sendo `VALIDACAO`, a tarefa conserva sua conferência de lavra/efeito; não vira responder/colaborador. Igualdade efetiva com main: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:167`) | Inverter a caracterização existente: leitura sem validação, estado pendente, C6 vivo e presença em `q_vivas`; acrescentar controle validada não ressuscitada. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413`, `:423`, `:426`, `:428`) |
| **Respondida legal, sem validação, na projeção** | **Nenhum ato novo:** fica fora do universo; continua respondida e sem validação. Vida continua dependente dos demais degraus do C6. (`app/chamados/models.py:1545`; `app/chamados/juizes.py:1552`, `:1423`) | Sem alteração por essa porta. Quando aplicável, última `RESPOSTA` com texto continua pedindo validar/admin. (`app/chamados/services/tarefa_do_dia.py:159`) | Controle negativo: seleção não inclui a pergunta, retorno zero e nenhuma decisão adicional; apoiar no predicado e nos controles de idempotência existentes. Execução: **[nao verificado]**. (`app/chamados/models.py:1545`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:352`) |
| **Respondida apenas pela sequência, projeção não respondida; ou resposta chegando na corrida declarada** | Se passar pelo universo, preservar o anterior e gravar **R** → **não** → **não** → **sim**, nas condições da seleção. A corrida entre universo e trava já é reconhecida pelo escritor. (`app/chamados/models.py:1545`, `:1642`, `:4237`) | Conservar a decisão anterior aplicável à tarefa; não transformar R em nova devolução. Igualdade com main: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:158`, `:163`) | Caso com resposta antiga só no ato e caso de corrida, conferindo projeção/leitura/SQL e fila. O selo atual já constrói divergência entre ato e projeção, mas não prova essa compensação. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:437`, `:447`) |
| **Legado instante sem texto, sem validação** | Pode entrar; gravar **R** → **não** → **não** → **sim**, se selecionada. Esse meio-par não é resposta pelo C21. (`app/chamados/juizes.py:1547`; `app/chamados/models.py:1545`, `:1576`) | Preservar o ramo anterior aplicável; `RESPOSTA` vazia pede responder/colaborador. Main x árvore: **[nao verificado]**. (`app/chamados/services/tarefa_do_dia.py:160`) | Ampliar o controle existente para exigir o compensador e seu efeito na sequência. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388`, `:399`, `:410`) |
| **Silêncio decidido, elegível** | Conservar `SILENCIO` histórico e gravar **R** → **não** → **não** → **sim**, se selecionada. (`app/chamados/models.py:1576`, `:1651`; `app/chamados/models.py:4235`) | Último ato **aplicável à tarefa** continua `SILENCIO`: `silencio_decidido`, ações vazias. O compensador não pode assumir esse lugar. (`app/chamados/services/tarefa_do_dia.py:176`) | Adaptar o teste existente de silêncio antes/depois, exigindo também presença do compensador e ausência de devolução. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319`, `:334`) |
| **Sem ato, sem resposta/validação, mas materializada e elegível** | Gravar **R** mesmo sem resposta anterior → **não** → **não** → **sim**, se selecionada. Existe estado retirado: a materialização. (`app/chamados/models.py:1543`, `:1651`, `:1576`) | Excluído R da sequência da tarefa, continua sem decisão aplicável: `aguarda_resposta`, responder/colaborador. (`app/chamados/services/tarefa_do_dia.py:156`) | Ampliar o teste sem decisão para exigir uma compensação e segunda chamada sem duplicação. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:331`, `:352`) |
| **Sem ato e sem materialização** | Não entra: nenhum compensador por essa porta. (`app/chamados/models.py:1543`) | Sem alteração causada por ressuscitar. (`app/chamados/models.py:1614`) | Controle negativo do universo; execução: **[nao verificado]**. (`app/chamados/models.py:1542`) |

Texto sem instante **e sem validação** permanece excluído pelo filtro nominal já existente; incluí-lo seria ampliar o universo, não completar o ato da ressuscitação atual. (`app/chamados/models.py:1553`, `:1558`)

A comparação correta precisa distinguir conservação do **ramo da fila** de igualdade integral do resultado: a tarefa também lê C6, passivo, lavra, efeitos e fila dos chamados. Filtrar o compensador evita a devolução nova, mas não constitui prova de main x árvore. (`app/chamados/services/tarefa_do_dia.py:271`, `:430`, `:435`, `:437`, `:395`)

Os testes existentes comparam razão e ações da instância para sem decisão e silêncio; não provam o resultado integral das 2.025 unidades. O total 2.025 vem do anexo fornecido; a distribuição por estado, o subconjunto validado, os 321 últimos silêncios e o diff zero de main x árvore permanecem **[nao verificado]**. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`, `:319`, `:331`)

A sonda necessária é por pergunta/medida identificada: mesma entrada, relógio e operação nas duas versões, comparando o resultado integral da tarefa, além de leitura, validação SQL, C6 e inclusão em `q_vivas`; o código exige essa continência do SQL sobre toda viva. Essa sonda de integração não foi executada. **[nao verificado]** (`app/chamados/services/fio.py:255`)

**(iii) Contagem e forma mínima**

Contando **leitores cuja regra precisa ser adaptada**, incluindo o histórico solicitado no censo:

| Forma | Leitores tocados | Fundamentação |
|---|---:|---|
| **F1** | **4** | Leitura vigente; validação SQL; histórico anterior; seleção das decisões da tarefa. Leitura e SQL continuam sendo dois leitores, embora compartilhem o conjunto de tipos. (`app/chamados/models.py:4235`, `:3500`, `:3750`; `app/chamados/services/tarefa_do_dia.py:421`) |
| **F2** | **1** | `CargaDaTarefa.__init__`, ao montar a sequência aplicável à tarefa. Leitura, SQL e histórico já reconhecem `REABERTURA`. (`app/chamados/services/tarefa_do_dia.py:392`, `:421`; `app/chamados/models.py:4191`, `:3500`, `:4108`) |

A adaptação mínima de F2 é excluir **somente a REABERTURA nominal de ressuscitar** da sequência da tarefa, antes de escolher o último ato, preservando `ato_em <= agora`. Ignorar apenas dentro do ramo `REABERTURA` perderia a decisão anterior; também deixaria encoberta a checagem de última `RETRATACAO`. (`app/chamados/services/tarefa_do_dia.py:158`, `:259`, `:423`)

Escritor, persistência da origem e testes não entram nessa contagem de leitores. Ressuscitar continua no escritor existente, preservando o anterior e acrescentando o compensador no mesmo ato transacional; a porta comum `reabrir_para_correcao` já comporta foto, mas não substitui automaticamente os demais efeitos próprios de ressuscitar. (`app/chamados/models.py:1627`, `:1633`, `:1654`, `:1660`; `app/chamados/services/disputa_emissao.py:1631`)

Não há pergunta de lei necessária para essa representação: o universo permanece o do código e a fila conserva a decisão aplicável anterior; a decisão de devolver ao colaborador continua fora deste pré-desenho, conforme o anexo fornecido. **[nao verificado]**

**(iv) Outros escritores automáticos e confusão com humano**

| Escritor/fluxo | Ato e autoria registrados |
|---|---|
| Resposta automática e recuperação pela trilha | `RESPOSTA` sem autor explícito; motivos próprios. (`app/chamados/services/disputa_emissao.py:1678`, `:1699`) |
| Foto recebida e lote classe A | Chamam o dono da validação; o lote recebe autor nominal. (`app/api/foto_recebida.py:48`, `:51`; `app/chamados/management/commands/validar_classe_a.py:89`, `:100`; `app/chamados/services/validacao.py:173`) |
| Passivo, nascimento, ressuscitar e backfills | Reconstituem `RESPOSTA/VALIDACAO`; a validação conserva o autor original. (`app/chamados/models.py:4175`, `:4181`, `:3160`, `:1633`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`) |
| Fechamento de disputa e do fio do dia | `SILENCIO` com autor recebido pelo fluxo. (`app/chamados/models.py:2036`; `app/ponto/services/veredito_celula.py:195`) |
| Backfill de `admin_decidiu` | `SILENCIO` com instante e autor originais do carimbo. (`app/chamados/management/commands/backfill_decisoes_pergunta.py:324`) |
| Remoção de motivo | `RETRATACAO` com ator recebido e texto vigente. (`app/chamados/models.py:2168`) |
| Absorção das irmãs pela ausência | `ABSORCAO` com autoria herdada e ligações ao escape/ausência. (`app/chamados/services/materializacao.py:1046`, `:1048`) |
| Validação recusada pela materialização | `DEVOLUCAO` com autor recebido; preserva a resposta. (`app/chamados/services/validacao.py:269`, `:279`) |
| Ausência rejeitada | `REABERTURA` pelo usuário sistema **para devolver ao colaborador**. (`app/chamados/services/materializacao.py:1158`, `:1167`) |
| Cura de perguntas em limbo | Pode chamar a reabertura sem `por`, portanto com autoria nula. (`app/chamados/management/commands/curar_perguntas_em_limbo.py:69`) |

Hoje a tarefa confunde semanticamente o novo compensador com uma devolução comum: não consulta autoria nesse ramo. Autor sistema ou nulo também não serve de discriminador, pelos dois últimos escritores da tabela. (`app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/materializacao.py:1167`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`)

Leitura e SQL não classificam `REABERTURA` como humana: aplicam seu efeito pelo tipo. A classificação frágil por motivo vazio está em `RESPOSTA`, onde `_resposta_desfaz_validacao` e a face SQL usam `ato_motivo`; F2 não precisa acrescentar significado a esse campo. (`app/chamados/models.py:4235`, `:4205`, `:3500`)

O histórico mostra o rótulo genérico e o autor/motivo disponíveis; já o juiz de toque humano da tranca consulta `LogAuditoria` e assinaturas próprias, não `DecisaoPergunta`. Portanto o novo ato isolado não constitui toque humano nesse juiz. (`app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/juizes.py:1241`, `:1250`, `:1260`)

O custo adicional no app continua pendente: a consulta não faz prefetch de decisões, enquanto `estado` e `resposta_display` chamam

[truncado em 20000 de 20292 caracteres]

## 17/11 10:30 · sessao 92df86040ee8 · E4C-F2 · verificador · origem=Code

**AVAL-DEVOLVIDO nos itens 2 e 4:** a exceção precisa fechar o alcance sobre o leitor da fila, e o RED (b) precisa medir a transição de ressuscitar. A premissa do item 1 confere (`app/chamados/models.py:1633`; `app/chamados/models.py:1651`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319`).

**Item 1 — CONFERE.**

1. **Premissas:** ressuscitar fotografa RESPOSTA/VALIDACAO e depois limpa validação, instante e texto na projeção. Falta o ato **compensatório em DecisaoPergunta**; existe auditoria genérica, portanto “sem nenhuma trilha” seria incorreto. A leitura conserva a validação fotografada, exatamente como caracteriza o teste (`app/chamados/models.py:1633`; `app/chamados/models.py:1651`; `app/chamados/models.py:1675`; `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:425`).

2. **Avais:** limpar a resposta inteira corresponde à L-130. A proibição anterior de ato novo recebe exceção nominal no item 4 do novo anexo; isso sustenta a proposta, condicionada ao pré-desenho. O registro local mantém pendente a lei de devolver ao colaborador (`app/docs/LEIS.md:151`; exceção recebida no anexo, conferência do arquivo original [nao verificado]).

3. **Literal:** a divergência C6/ato confere: C6 lê a projeção; `q_vivas` exclui validada pelo ato, embora deva conter toda viva. O universo já testa C6 numa cópia com V/R/texto limpos. As **2.025 ressuscitáveis são dado fornecido**, não medida reproduzida; quantidade de validadas nesse conjunto [nao verificado] (`app/chamados/juizes.py:1389`; `app/chamados/juizes.py:1404`; `app/chamados/models.py:3562`; `app/chamados/services/fio.py:255`; `app/chamados/models.py:1572`; referência fornecida `r2/f2.r:14` [nao verificado]).

4. **Exportada/dinheiro/!:** o alcance não se limita à trilha: a face filtra `_ancoradas`, e a leitura alimenta respostas sem veredito usadas pelo CSF. Neutralidade do futuro ato [nao verificado]; permanece a guarda de exportada/PAREI ! (`app/chamados/models.py:357`; `app/chamados/models.py:1953`; `app/ponto/supra_juiz.py:573`; `app/docs/CORTES.md:487`).

**Item 2 — PROCEDE.**

1. **Premissas:** “mantém a fila” está registrado. Entretanto, hoje REABERTURA/RECUSA manda o colaborador responder, DEVOLUCAO manda o admin validar e tipo desconhecido retorna “não sei”. A carga inclui todos os atos; não há distinção pronta por origem (`app/chamados/models.py:1644`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/tarefa_do_dia.py:165`; `app/chamados/services/tarefa_do_dia.py:187`; `app/chamados/services/tarefa_do_dia.py:421`).

2. **Colisão:** o complemento 1 fornecido proíbe **mudar tarefa_do_dia**; a exceção nominal do novo item 4 menciona apenas **ato novo**. Fazer o leitor distinguir/ignorar o compensatório exige alteração que hoje não existe. O aval precisa explicitar essa exceção técnica, preservando o resultado; não basta inferi-la de “tocar menos leitores” (`app/chamados/services/tarefa_do_dia.py:158`; `app/chamados/services/tarefa_do_dia.py:421`; redação dos dois anexos recebidos, arquivos originais [nao verificado]).

3. **Literal:** **não validada pelo ato + viva pelo C6 + fila igual não é contradição lógica**: vitalidade e exigibilidade leem fontes distintas, e SILENCIO pode deixar nenhuma ação. Porém nenhuma das formas atuais preserva genericamente a fila. O pré-desenho precisa demonstrar a transparência do compensatório também na carga e nas exposições administrativas (`app/chamados/juizes.py:1404`; `app/chamados/services/tarefa_do_dia.py:176`; `app/chamados/services/tarefa_do_dia.py:267`; `app/chamados/services/tarefa_do_dia.py:421`).

4. **Exportada/dinheiro/!:** igualdade da fila não prova neutralidade dos demais leitores; CSF usa outro caminho. Impacto financeiro/exportado do futuro desenho [nao verificado]; a guarda vigente continua obrigatória (`app/ponto/supra_juiz.py:573`; `app/docs/CORTES.md:487`).

**Item 4 — PROCEDE.**

1. **Premissas:** o RED (a) tem caso concreto: a caracterização atual espera projeção sem validação, leitura validada e exclusão de `q_vivas`. O universo admite validada sem resposta completa; resposta legal completa fica fora pela face `q_nao_respondida`. A matriz deve distinguir esse ramo dos pares ilegais (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:425`; `app/chamados/models.py:1542`; `app/chamados/models.py:1545`; `app/chamados/models.py:1558`).

2. **Avais:** a exceção nominal fecha a proibição de ato novo, mas deixa a lacuna do leitor da fila apontada no item 2. Também não dispensa os demais selos de conservação nem a guarda de exportada (`app/docs/CORTES.md:487`; anexos fornecidos, conferência dos arquivos originais [nao verificado]).

3. **Literal do RED (b):** **“diff 0 main × árvore sobre as 2.025” não prova, sozinho, “fila igual antes e depois de ressuscitar”**. Uma comparação estática pode não executar o escritor que produzirá o compensatório. O selo precisa fixar conjunto por identidade e relógio, executar a mesma ressuscitação nas duas versões e comparar a tarefa completa antes/depois e entre versões. O teste existente faz explicitamente leitura → ressuscitação → nova leitura; apenas contar ressuscitáveis não substitui essa transição (`app/chamados/models.py:1633`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:323`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:326`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:329`). Identidades das 2.025 e cobertura dos estados de partida [nao verificado].

4. **Exportada/dinheiro/!:** os zeros relatados na entrega anterior não medem o compensatório futuro [nao verificado]. O novo selo deve conservar a guarda já vigente: qualquer diferença em supra_juiz de competência exportada exige PAREI ! com tabela; igualdade apenas da tarefa é insuficiente (`app/ponto/supra_juiz.py:573`; `app/docs/CORTES.md:487`).

VEREDITO por item: 1 CONFERE; 2 PROCEDE; 4 PROCEDE

## 17/11 10:30 · sessao 5ce08cc5d9ea · - · - · origem=Ronald (tmux)

F2 exige origem **nominal de ressuscitação**; `ato_por=sistema` não distingue o evento: a ausência rejeitada já grava REABERTURA pelo usuário sistema para devolver ao colaborador (`app/chamados/services/materializacao.py:1158`, `:1167`). `DecisaoPergunta` atualmente não possui campo de origem (`app/chamados/models.py:4110`).

Censo consolidado:

| Leitor | Local | F1 / F2 |
|---|---|---|
| Resposta/validação vigentes | `app/chamados/models.py:4230` | F1 exige reconhecimento; F2 já limpa ambos. |
| Face SQL de validação | `app/chamados/models.py:3500` | F1 exige inclusão; F2 já reconhece. |
| Histórico anterior | `app/chamados/models.py:3750` | F1 exige reconhecimento e compatibilidade da foto; F2 já reconhece REABERTURA. |
| Fila por última decisão | `app/chamados/services/tarefa_do_dia.py:159` | Ambos precisam preservar a decisão anterior para a fila. |
| Rótulo BUG-PAR | `app/chamados/services/materializacao.py:275` | Só lê DEVOLUCAO com prefixo; ignora ambos. |
| Backfill SILENCIO | `app/chamados/management/commands/backfill_decisoes_pergunta.py:99`, `:315` | Só considera SILENCIO/VALIDACAO; ressuscitada sai do universo ao limpar via/materialização (`:101`). |
| Foto E4b | `app/chamados/services/foto_e4b.py:280` | Captura ambos independentemente do tipo. |

Leitores indiretos que herdam o resultado, sem precisar de alteração própria:

- `estado`, `respondida`, `resposta_display`: `app/chamados/models.py:3184`, `:3735`, `:3759`.
- Respostas sem veredito: `app/chamados/models.py:1953`; wrapper `:1928`; CSF `app/ponto/supra_juiz.py:573`; irmãs `app/chamados/juizes.py:2117`; fechar `app/chamados/models.py:1964`, `:1997`; reconciliador `app/chamados/reconciliador.py:1166`; desligamento `app/colaboradores/services/desligamento.py:165`.
- Âncoras e relavra S2: `app/chamados/models.py:357`, `:315`.
- C6 lê a projeção, não os atos: `app/chamados/juizes.py:1389`, `:1404`; relavra usa esse juiz (`app/chamados/services/relavra_via.py:242`).
- Consumidores de `q_vivas`: `app/chamados/juizes.py:1763`; `app/chamados/services/fio.py:256`; `janela_colab.py:63`; `vigia_fio.py:711`; `raiox.py:74`; `cobranca.py:237`; `pergunta_no_app.py:53`; `app/ponto/services/veredito_celula.py:181`.
- API: `app/api/views.py:2550`, `:2562`, `:2578`; outra exposição de display em `:1164`.
- Histórico: wrapper `app/chamados/services/disputa_emissao.py:2123`; apresentação `app/templates/chamados/partials/modal_fio.html:205`, `:212`.
- Templates de resposta: `app/templates/chamados/partials/modal_fio.html:217`; `_pergunta_card.html:29`; `_card_atendimento.html:106`.
- App continua sem prefetch de decisões na consulta, enquanto a serialização pede `estado` e `resposta_display`: `app/chamados/services/pergunta_no_app.py:52`, `app/api/views.py:2550`, `:2562`.

Contagem por **leitor lógico**, incluindo os dois leitores que compartilham `_LIMPAM_VALIDACAO`: F1 = **4**; F2 = **1**, alterando a seleção da última decisão em `CargaDaTarefa.carregar`, com identificação exclusiva da ressuscitação. A evidência dos pontos de reconhecimento está em `app/chamados/models.py:3500`, `:3750`, `:4230` e `app/chamados/services/tarefa_do_dia.py:159`.

## 17/11 10:30 · sessao eed7ba59b0a8 · - · - · origem=Ronald (tmux)

A fila precisa ignorar o ato compensatório **antes de escolher a última decisão aplicável**: hoje `_pela_decisao` lê `decisoes[-1]`; F1 desconhecido cai em `ato_desconhecido`/`None`, e F2 `REABERTURA` cai em `devolvida_ao_colaborador`/`RESPONDER`. (`app/chamados/services/tarefa_do_dia.py:156–187`)

O filtro pode ficar no carregamento único da sequência da tarefa, preservando o horizonte `ato_em <= agora` e a decisão anterior; isso também evita encobrir `RETRATACAO`, que `_tarefa_pura` verifica pela última linha. Para F1 filtra o tipo novo; para F2 filtra apenas a `REABERTURA` identificada explicitamente como a ressuscitação automática. (`app/chamados/services/tarefa_do_dia.py:259–261`, `:421–426`)

| Última decisão aplicável anterior | Comportamento a conservar na tarefa |
|---|---|
| `VALIDACAO` | Sem lavra vigente: `nao_sei`; lavra `nao`: materializar/sistema; lavra `sim` com efeito: rejulgar/sistema; `sim` sem efeito: encerrada. (`app/chamados/services/tarefa_do_dia.py:167–175`) |
| `RESPOSTA` | Texto vazio: responder/colaborador; texto não vazio: validar/admin. (`app/chamados/services/tarefa_do_dia.py:159–162`) |
| `SILENCIO` | `silencio_decidido`, ações vazias. (`app/chamados/services/tarefa_do_dia.py:176–177`) |
| Nenhuma | `aguarda_resposta`, responder/colaborador. (`app/chamados/services/tarefa_do_dia.py:156–157`) |

A resposta legal — instante **e** texto — fica fora do universo normal de ressuscitar: há `materializada_em != NULL` e filtro `q_nao_respondida`; texto sem instante/validação também fica fora. Portanto a linha “respondida sem validação” exige separar resposta legal, que é **NO-OP**, do legado instante sem texto, que pode entrar. (`app/chamados/models.py:1542–1559`)

`DecisaoPergunta` não tem campo `origem`; há `ato_por` e `ato_motivo`. F2 precisa tornar concreta a distinção técnica escolhida, sem pressupor que `ato_por` humano significa evento humano; o código documenta automatismo herdando usuário de request em outro fluxo. (`app/chamados/models.py:4110–4123`; `app/chamados/juizes.py:1251–1253`)

Provas já existentes:

- `test_f1nova_b1_juiz_respondida.py:311–329` mede somente razão/ações da instância; `:331–335` compara antes/depois para sem decisão e `SILENCIO`; `:352–358` verifica segunda chamada sem duplicar atos. Não prova igualdade integral de `tarefa_do_dia` nas 2.025 unidades. (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311–358`)
- Controle de `REABERTURA` comum: continua devolvendo para responder/colaborador. (`app/chamados/tests/test_e2_tarefa_do_dia.py:849–865`)
- Teto de consultas constante entre lote pequeno e maior já tem selo. (`app/chamados/tests/test_e2_tarefa_do_dia.py:913–938`)
- A caracterização de validada ressuscitada afirma expressamente a divergência atual: projeção limpa; leitura mantém validação; estado `VALIDADA`; sai de `q_vivas`. O teste precisa virar RED da compensação, preservando também controle não ressuscitado. (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413–428`)

Executei apenas uma sonda em memória, `python3 -B`, extraindo AST de `_pela_decisao` e substituindo seus três imports por objetos simples: confirmou os ramos acima nos quatro casos, sem arquivos nem banco. Sua abrangência é local à função; igualdade de integração/main permanece `[nao verificado]`. (`app/chamados/services/tarefa_do_dia.py:142–187`)

A cópia não oferece `.git`; `git branch --all` falhou. Comparação real com main e sombra das 2.025 medidas: `[nao verificado]`.

## 17/11 10:29 · sessao c8e40a3bbf61 · - · - · origem=Ronald (tmux)

Achados de escritores e matriz:

- `ressuscitar_perguntas` grava somente o passivo R/V antes de limpar a projeção; não grava ato compensatório. O próprio código explica que evitou `REABERTURA` porque a fila a interpreta como devolução ao colaborador. `app/chamados/models.py:1633`, `app/chamados/models.py:1644`, `app/chamados/models.py:1651`.
- Respondida legal — instante E texto — sem validação não entra no universo normal: exige `q_nao_respondida()`. Validada com resposta legal também fica fora; validada sem resposta pode entrar. A resposta que chega entre a seleção do universo e a trava é corrida explicitamente reconhecida no comentário. `app/chamados/models.py:1542`, `app/chamados/models.py:1545`, `app/chamados/models.py:1642`.
- “Sem ato” não equivale a qualquer pendente: precisa de `materializada_em` preenchida, passar pelos filtros de via, marco apagado e C6 na cópia limpa. `app/chamados/models.py:1543`, `app/chamados/models.py:1547`, `app/chamados/models.py:1569`, `app/chamados/models.py:1572`.

**Armadilha decisiva de F2:** já existe uma `REABERTURA` automática por usuário sistema que deliberadamente devolve ao colaborador: ausência rejeitada. O motivo diz “voltam ao colaborador”, e a escrita passa `por=get_user_sistema()`. Portanto, ignorar toda `REABERTURA` cujo autor seja sistema quebra comportamento existente. `app/chamados/services/materializacao.py:1158`, `app/chamados/services/materializacao.py:1167`.

`DecisaoPergunta` não possui campo `origem`; tem `ato_por` e `ato_motivo`. Autor nulo também já é aceito em reabertura legítima: command de cura chama `reabrir_para_correcao` sem `por`. Assim, F2 exige identificação nominal inequívoca da ressuscitação; autor sistema/nulo isoladamente não basta. `app/chamados/models.py:4110`, `app/chamados/models.py:4113`, `app/chamados/models.py:4115`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:69`.

Escritores automáticos encontrados:

| Escritor | Ato/autoria |
|---|---|
| `responder_automaticamente` | `RESPOSTA`, sem autor, motivo fixo “resposta automatica…”; chamado no upload de foto e seguido de validação pelo dono. `app/chamados/services/disputa_emissao.py:1678`; `app/api/foto_recebida.py:48`; `app/api/foto_recebida.py:51`. |
| `carimbar_resposta_pela_trilha` | `RESPOSTA`, sem autor, motivo “pela trilha…”, instante testemunhado pela nota; command de cura chama essa porta. `app/chamados/services/disputa_emissao.py:1683`; `app/chamados/services/disputa_emissao.py:1699`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:67`. |
| `linhas_da_projecao_sem_linha` | Reconstitui `RESPOSTA` sem autor e `VALIDACAO` preservando `validada_por_id`; motivo fixo de passivo. `app/chamados/models.py:4175`; `app/chamados/models.py:4179`. |
| Nascimento / ressuscitação / backfill fio morto | Registram as linhas da porta de passivo. `app/chamados/models.py:3160`; `app/chamados/models.py:1633`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:141`. |
| Fechar disputa por desligamento/admin | `SILENCIO`, autor recebido pelo fechamento; usa resposta vigente da sequência. `app/chamados/models.py:2029`; `app/chamados/models.py:2036`. |
| Fechar fio do dia | `SILENCIO`, autor herdado do ato de decidir o dia. `app/ponto/services/veredito_celula.py:190`; `app/ponto/services/veredito_celula.py:195`. |
| Backfill de admin decidiu | `SILENCIO`, instante e autor originais do carimbo, motivo identificando backfill. `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`. |
| Remover motivo | `RETRATACAO`, autor recebido, texto vigente; carimba identidade retratada. `app/chamados/models.py:2166`; `app/chamados/models.py:2168`; `app/chamados/models.py:2173`. |
| Absorver irmãs pela ausência | `ABSORCAO`, autor herdado da declaração; liga escape e ausência. `app/chamados/services/materializacao.py:1046`; `app/chamados/services/materializacao.py:1048`. |
| Devolver validação recusada pela materialização | `DEVOLUCAO`, preserva resposta, autor herdado da chamada e motivo da recusa. `app/chamados/services/validacao.py:269`; `app/chamados/services/validacao.py:279`. |
| Ausência rejeitada | `REABERTURA`, usuário sistema, com finalidade explícita de devolver ao colaborador. `app/chamados/services/materializacao.py:1158`; `app/chamados/services/materializacao.py:1167`; `app/chamados/services/disputa_emissao.py:1631`. |
| Command `validar_classe_a` | Usa o mesmo dono de `VALIDACAO`, com autor nominal passado por argumento. `app/chamados/management/commands/validar_classe_a.py:89`; `app/chamados/management/commands/validar_classe_a.py:100`; `app/chamados/services/validacao.py:173`. |

Confusão humano/sistema:

- A fila não lê autoria: qualquer `RECUSA`/`REABERTURA` última é `devolvida_ao_colaborador`; qualquer `DEVOLUCAO` última é materialização recusada para admin. `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:165`.
- `leitura` e a face SQL distinguem resposta humana por `ato_motivo` vazio, não por autoria. Automáticas e passivo usam motivo não vazio. `app/chamados/models.py:4195`, `app/chamados/models.py:4205`, `app/chamados/models.py:3500`.
- O histórico exibe o rótulo do ato, autor quando presente e motivo quando presente; uma `REABERTURA` automática continua com o rótulo genérico “Reaberta para correcao”. `app/templates/chamados/partials/modal_fio.html:212`, `app/chamados/models.py:4103`.
- O juiz de toque humano na tranca consulta `LogAuditoria`, não `DecisaoPergunta`; além da autoria, exige assinatura de ação humana no caso de resolver dia. O ato novo isolado não é toque humano nesse leitor. `app/chamados/juizes.py:1241`, `app/chamados/juizes.py:1250`, `app/chamados/juizes.py:1263`.

Provas existentes aproveitáveis:

- Defeito validada ressuscitada já tem caracterização que afirma projeção limpa, leitura ainda validada e exclusão de `q_vivas`; deve inverter como RED do novo corte. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:413`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:423`.
- Controle atual de instante sem texto ressuscitado verifica projeção limpa, não respondida pelo ato, pendente e presente no SQL largo e nas ancoradas. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:399`, `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:410`.
- Comparação da fila antes/depois existe para sem decisão e último `SILENCIO`, com relógio fixado; compara razão e ações. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:311`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:331`.
- O teste antigo também proíbe `REABERTURA`/`RECUSA` e exige somente fotos R/V; esse contrato de teste precisará ser alinhado à exceção nominal do novo aval, se F2 for escolhida. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:270`, `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:280`.

## 17/11 10:29 · sessao cec9f1117055 · - · - · origem=Ronald (tmux)

Achados só por leitura:

- A coexistência “não validada pelo ato E viva pelo C6” com fila conservada é possível como desenho: C6 lê a projeção (`app/chamados/juizes.py:1389`, `:1404`), enquanto tarefa lê a última decisão (`app/chamados/services/tarefa_do_dia.py:158`). A porta já inclui somente perguntas que C6 considera vivas numa cópia com V/R/T zerados e carimbo retirado (`app/chamados/models.py:1567`, `:1572`, `:1576`).
- Nenhuma forma existente implementa essa conservação genericamente: REABERTURA/RECUSA manda responder o colaborador (`app/chamados/services/tarefa_do_dia.py:163`); DEVOLUCAO manda validar o admin (`:165`); tipo novo cai em `ato_desconhecido`/`None` (`:187`). A carga lê todos os atos, sem filtro por origem (`:421`–`:424`).
- Portanto, “REABERTURA de origem sistema que o leitor distingue” demanda alteração desse leitor: atualmente ele não olha ator/motivo ao decidir REABERTURA (`app/chamados/services/tarefa_do_dia.py:158`, `:163`). Não existe campo próprio de origem; existem ator e motivo (`app/chamados/models.py:4113`, `:4115`).
- Há tensão literal com a proibição de mudar `tarefa_do_dia` do COMPLEMENTO-1 fornecido: o novo item 4 explicita exceção somente para ato novo [nao verificado]. Para preservar fila com o novo ato, a exceção relativa ao leitor deveria ser explicitada; nenhuma implementação pronta disso está na cópia (`app/chamados/services/tarefa_do_dia.py:163`, `:187`, `:421`).
- “Respondida sem validação” precisa separar resposta legal dos pares ilegais: resposta legal fica fora do universo por `q_nao_respondida` (`app/chamados/models.py:1542`, `:1545`); texto sem instante e sem validação também é excluído (`:1558`). O caso instante sem texto é exercitado pelo controle existente (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:388`, `:396`).
- Os testes existentes de fila comparam antes/depois da execução real de ressuscitar para sem decisão e SILENCIO (`app/chamados/tests/test_f1nova_b1_juiz_respondida.py:319`, `:326`, `:329`, `:331`, `:334`). Esse é o formato necessário para provar o evento. Comparar main/árvore sobre registros ainda não ressuscitados pode dar diff zero sem exercitar o ato novo; não demonstra, sozinho, conservação após a transição [nao verificado].
- A caracterização de validada ressuscitada confirma V projetada nula, V do ato preservada e exclusão de `q_vivas` (`app/chamados/tests/test_e4c_f2_leitura_do_ato.py:423`, `:425`, `:426`, `:428`). A própria anotação “0 ressuscitáveis com validação” do teste não é medida comprovada por esse código (`:417`–`:418`).
- `fio.py:255` declara literalmente a continência e usa `q_vivas` antes de C6 nas disputas fechadas (`app/chamados/services/fio.py:255`, `:256`, `:269`). A implementação de `q_vivas` exige não validada pelo ato (`app/chamados/models.py:3562`), enquanto C6 usa V projetada (`app/chamados/juizes.py:1404`–`:1406`).

_6480 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
