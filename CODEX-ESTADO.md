# CODEX-ESTADO

> Ultima resposta processada: **17/11/2026 00:01** · publicado em **17/11/2026 00:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 17/11 00:01 · sessao f0af060a5ee8 · REVISAO-AVAL-LU207 · verificador · origem=Code

1. **Premissas do CORTE-LU2-07-EMPRESA-1 conferem no código:** `em_rollout` significa “Empresa em produção”, e a tela diz “Em operação” (`app/colaboradores/models.py:45`; `app/templates/colaboradores/empresa_form.html:35`). O vigia usa `empresas_visiveis()`, que não filtra esse campo; o cron mantém `(2,3,4)` literal (`app/chamados/services/vigia_fio.py:517`; `app/colaboradores/queries.py:85`; `app/config/crons.py:17`). Os 7 colaboradores, 3 ativos e valores atuais das flags constam apenas do anexo [nao verificado].

2. **Não identifiquei colisão do novo aval com o corte vigente.** A pergunta aberta é precisamente se emp1 opera (`app/docs/RELATO.md:354`). O anexo anterior foi sucedido pela REEMISSAO-1215; não autoriza reaplicar seus cortes: LU2-08 registra agora 21 aprovadas, zero sem aprovação, com parte ainda aberta (`app/docs/RELATO.md:341`; `app/docs/RELATO.md:352`). Permanecem D1/D2 e o complemento LU2-08; R1 já pode seguir independentemente dessas LU2 (`app/docs/RELATO.md:421`; `app/docs/RELATO.md:422`). A F2 também já foi liberada da espera pela CLASSE-D (`app/docs/RELATO.md:4`).

3. **Nenhuma condição impossível no literal localizada.** Os 46 são subconjunto: o alcance documentado de emp1 é 80 dias, portanto não se pode prometer redução total de exatamente 46 (`app/docs/RELATO.md:448`). A recontagem pode usar `censo_alcance`, que percorre empresa, competência e dia retido (`app/chamados/services/vigia_fio.py:487`; `app/chamados/services/vigia_fio.py:534`). Censo prévio, recontagem atual e brief integral: [nao verificado].

4. **O recorte do vigia comporta mudar o alarme sem relavrar dinheiro:** a função declara nenhuma escrita nem recálculo (`app/chamados/services/vigia_fio.py:490`). Alterar globalmente `empresas_visiveis()` ampliaria o alcance ao lavrador do retrato da folha; isso ultrapassaria a alteração localizada no vigia (`app/folha/management/commands/lavrar_retrato_folha.py:51`). Não há necessidade demonstrada de tocar exportada, dinheiro ou `!` para esse recorte de leitura (`app/chamados/services/vigia_fio.py:490`).

VEREDITO: CONFERE

## 17/11 00:01 · sessao 472843b72c27 · - · - · origem=Ronald (tmux)

- O novo aval preserva as pendências do brief: D1 (ramo C, 18 dias), D2 (ramo B, 26) e o complemento LU2-08; resolver LU2-07 não libera automaticamente toda a CLASSE-D. R1 já foi declarado independente de LU2-07/08. Referências: `app/docs/RELATO.md:421`, `app/docs/RELATO.md:422`. O brief literal indicado em `logs/CLASSE-D/brief/brief.md` não está disponível nesta cópia [nao verificado].

- O anexo antigo CORTES-LU2-07-08-10 foi superado pela REEMISSAO-1215; LU2-07/08 permaneceram abertas e LU2-10 ficou respondida como fato, sem ação. Portanto, não usar o anexo antigo como autorização vigente para retirar os 22 CADASTRO nem para concluir genericamente que toda ausência aprovada elimina retenção. Referências: `app/docs/RELATO.md:341`, `app/docs/RELATO.md:343`.

- LU2-08 atual distingue 10 PARCIAL_SEM_PERIODO, já decididos como CADASTRO, de 10 FATO_EM_AUSENCIA ainda esperando corte; contabiliza 21 com ausência aprovada, zero sem, e dos 38 “tarefa nada” apenas nove pertencem aos 21. Isso contraria a descrição antiga “19 aprovados + 2 sem aprovação”, mas o novo aval não a reaplica. Referências: `app/docs/RELATO.md:352`, `app/docs/RELATO.md:422`.

- O campo citado existe e diz literalmente “Empresa em producao no Hasner (piloto/rollout)”. O código também define colaboradores em operação usando empresa com `em_rollout=True`, além das condições próprias do colaborador. Referências: `app/colaboradores/models.py:45`, `app/colaboradores/queries.py:50`, `app/colaboradores/queries.py:65`.

- O vigia realmente usa `empresas_visiveis`, uma função declarada como fonte de UI. O censo soma dias retidos por empresa e competência, sem recalcular folha. Portanto, excluir empresa do universo do alarme não remove a retenção da folha nem altera os dados empresariais por essa leitura. Referências: `app/colaboradores/queries.py:78`, `app/chamados/services/vigia_fio.py:490`, `app/chamados/services/vigia_fio.py:517`, `app/chamados/services/vigia_fio.py:534`.

- Os 46 são subconjunto dos 177; o alcance total documentado da empresa 1 é 80 dias. O aval diz que os 46 sairão, sem prometer que serão os únicos; a recontagem pela função real antes da construção continua necessária. Referências: `app/docs/RELATO.md:354`, `app/docs/RELATO.md:448`. A futura contagem efetiva não foi executada [nao verificado].

- O alcance do censo usa competência atual e anterior quando ainda não entregue; a alteração proposta no vigia não implica relavrar `DiaPago`, `FechamentoMensal` nem TXT. Referências: `app/chamados/services/vigia_fio.py:535`, `app/ponto/services/he_pendente_lavrado.py:209`, `app/ponto/services/he_pendente_lavrado.py:223`.

Não encontrei condição adicional procedente no novo aval dentro deste recorte; a restrição “só registrar agora” e as pendências preservadas impedem tratá-lo como autorização imediata de construção. Referências: `app/docs/RELATO.md:421`, `app/docs/RELATO.md:422`; texto do novo aval fornecido pelo usuário [nao verificado em arquivo].

## 17/11 00:00 · sessao 68162d6ca69e · - · - · origem=Ronald (tmux)

- Premissa de cadastro confere: `Empresa.em_rollout` significa empresa em produção; a tela o apresenta como “Em operacao (rollout)” e o grava pela porta existente (`app/colaboradores/models.py:45`; `app/templates/colaboradores/empresa_form.html:34`; `app/colaboradores/views.py:513`; `app/colaboradores/views.py:539`; `app/colaboradores/services/estrutura.py:18`).
- `empresas_visiveis()` define universo de UI, filtrando atividade e diagnóstico; não consulta `em_rollout`. O vigia de alcance e a lista do admin consultam essa função (`app/colaboradores/queries.py:78`; `app/chamados/services/vigia_fio.py:189`; `app/chamados/services/vigia_fio.py:517`).
- O terceiro sítio literal existe: `EMPRESAS = (2, 3, 4)` e seu comentário o denomina universo em operação (`app/config/crons.py:17`).
- Os números atuais — sete colaboradores, três ativos, flags nas quatro empresas — dependem exclusivamente do anexo [nao verificado]. O registro anterior declara dois ativos, 80 dias de alcance e 46 dos 177; portanto o efeito total da futura mudança de universo não se resume automaticamente aos 46 (`app/docs/RELATO.md:448`).
- A recontagem exigida antes de construir é pertinente: `censo_alcance` percorre os dias retidos por empresa e competência; a função declara nenhuma escrita nem recálculo (`app/chamados/services/vigia_fio.py:487`; `app/chamados/services/vigia_fio.py:529`).
- A futura alteração deve atingir o consumidor pretendido. Mudar globalmente `empresas_visiveis()` também alcançaria retrato e outros contadores, que consultam a mesma função (`app/folha/management/commands/lavrar_retrato_folha.py:51`; `app/folha/vigia_retido_calado.py:54`; `app/folha/retrato_folha.py:1411`).
- Não identifiquei impossibilidade literal ou contradição semântica no primeiro aval: a unidade empresa corresponde ao campo; `colaboradores_em_operacao()` acrescenta escala vigente apenas para a unidade colaborador (`app/colaboradores/models.py:45`; `app/colaboradores/queries.py:50`).

## 17/11 00:00 · sessao 0d96f86729be · - · - · origem=Ronald (tmux)

- O aval anterior `CORTES-LU2-07-08-10` foi superado pela `REEMISSAO-1215`: LU2-07/08 voltaram abertas ao leitor; LU2-10 ficou respondida como fato, sem ação. Não usar o anexo antigo como corte vigente. `app/docs/RELATO.md:341`, `app/docs/RELATO.md:343`.
- LU2-07 vigente pergunta exatamente “emp1 está em operação?” e registra os 46 dias; o novo aval responde essa pergunta. Não responde o restante dos 155: 79 sob L-124, 42 emp1 e 34 retidos corretamente por ausência de assinatura datada no C5. `app/docs/RELATO.md:354`.
- Há diferença documental nos ativos: registro anterior diz **2 ativos**; novo dado diz **3 ativos**. Atualização entre sondas ou erro [nao verificado]; não tratar a diferença como contradição provada. `app/docs/RELATO.md:448`.
- O antigo LU2-08 não confere com a consolidação vigente: **21 aprovadas, zero sem aprovação**, dez PARCIAL_SEM_PERIODO = CADASTRO, dez FATO_EM_AUSENCIA ainda abertas; dos 38 sem tarefa, nove pertencem aos 21 e 29 não têm ausência. `app/docs/RELATO.md:352`.
- A lei vigente exige parcial sem período = CADASTRO, nunca FATO_EM_AUSENCIA. A referência `CORTES.md:370` no RELATO envelheceu; endereço atual **`app/docs/CORTES.md:386`**.
- O brief resumido contém mais dependências: R1/C5 diário, InsumosDoDono, contador tela/PDF, D1 ramo C18 e D2 ramo B26. A expressão do novo aval “só o que mais estiver aberto no brief” preserva essas pendências; seria incorreto resumir para “só LU2-08”. `app/docs/RELATO.md:421`, `app/docs/RELATO.md:422`.
- A ordem antiga “CLASSE-D antes da F2” continua no BACKLOG, mas foi expressamente superada quanto à espera: complemento PREPARA 23:5x permite F2 sem esperar CLASSE-D. Novo aval apenas “segue a fila” não colide com essa ordem atual. `app/docs/BACKLOG.md:1990`, `app/docs/RELATO.md:4`.
- L-006 dá suporte a cadastro pela UI; L-099 separa ESTRUTURA de CADASTRO/BATIDA. Nenhuma dessas linhas autoriza reaplicar o corte antigo superado nem extinguir as dependências restantes. `app/docs/LEIS.md:64`, `app/docs/LEIS.md:122`, `app/docs/RELATO.md:341`, `app/docs/RELATO.md:421`.
- Arquivos primários `logs/AVAIS-*`, `logs/CLASSE-D/brief/brief.md` e entregas referenciadas não estão disponíveis nesta cópia; conteúdo primário [nao verificado]. O resumo do brief existe em `app/docs/RELATO.md:421`.

## 16/11 23:54 · sessao 0f433dc81e6f · REVISAO-AVAL-2355 · verificador · origem=Code

1. **Premissas: conferem como mudança a construir.** K6 hoje exclui U163; o segundo laço do comando também exclui U163 e imprime uma lista textual `PAUTA DP`. Incluir esse comando no escopo resolve a lacuna da lista, sem criar `Pauta`. (`app/chamados/services/materializacao.py:795`; `app/chamados/management/commands/bug_par_103.py:72`; `app/chamados/management/commands/bug_par_103.py:85`)

   Os baldes entregue/sem dia existem, mas atualmente atendem ao ramo materializado elegível U103; uma U163 não materializada bloqueada cai em `outras` e continua em `pendentes_aval`. O complemento define o destino antes faltante; a implementação deverá conservar `pendentes_aval + resolvidas + entregue + sem_dia = congelados_no_recorte`. (`app/chamados/services/materializacao.py:316`; `app/chamados/services/materializacao.py:326`; `app/chamados/services/materializacao.py:347`; `app/chamados/services/materializacao.py:356`)

2. **Colisões: há sucessão explícita da exceção anterior.** O registro anterior mantém U163 entregue sob F6, com aberto seguindo; o corte U163-NO-K6 já está registrado como ampliação para L1/L5, complemento da F4 sobre B2. A exceção e a matriz precisam acompanhar essa sucessão. (`app/docs/RELATO.md:44`; `app/docs/RELATO.md:52`; `app/docs/BACKLOG.md:823`; `app/docs/RELATO.md:21`)

   Continua vigente a condição de pousar F4 logo atrás de F1/F2 **ou** manter a costura J1 sob contador esperado zero no intervalo. (`app/docs/CORTES.md:487`)

3. **Literal: PROCEDE quanto ao RED.** “Irmã também U163 ou sem irmã” elimina o contraexemplo do K6 da irmã; o controle fora163 entregue confere. (`app/chamados/services/validacao.py:224`; `app/chamados/services/materializacao.py:795`; `app/chamados/tests/test_bug_par_volta.py:145`)

   Entretanto, isso ainda não garante **“hoje valida e grava”**: declarado diferente do gravado continua bloqueado pela espera U163; sem irmã, uma vizinha do mesmo tipo pode impedir a paridade; uma batida aprovada existente produz `ja_satisfeita`, sem nova batida. (`app/chamados/services/validacao.py:609`; `app/chamados/services/materializacao.py:936`; `app/chamados/services/materializacao.py:903`; `app/chamados/services/materializacao.py:928`; `app/chamados/services/materializacao.py:898`)

   O RED precisa fixar uma metade respondida, sem validação/materialização anterior, declarado igual ao gravado e plantio permitido pelas demais guardas, demonstrando criação de batida antes da mudança. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:615`; `app/chamados/services/validacao.py:681`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:914`) A comprovação desse caso em produção permanece [nao verificado].

4. **Exportada, dinheiro e `!`: não aparece ampliação além do bloqueio declarado.** A guarda atua antes do carimbo/recalculo no admin e antes das escritas do materializador. (`app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:655`; `app/chamados/services/validacao.py:666`; `app/chamados/services/materializacao.py:1829`) O recalculo herdado já está registrado; alterar `FechamentoMensal` exportado continua sob `!`. (`app/docs/RELATO.md:52`; `CLAUDE.md:411`) Impacto monetário efetivo: [nao verificado].

**VEREDITO: PROCEDE (item 3: a condição corrigida da irmã ainda não garante “hoje valida e grava”; declarado × gravado pode bloquear antes — `app/chamados/services/validacao.py:609`).**

## 16/11 23:54 · sessao c83d223213b2 · E4C-F1-B2 · verificador · origem=Code

**Parecer: devolver a volta 2 para correção.** Há três problemas no delta: aplicação de pergunta sem resposta no materializador, apagamento de texto sem preservação na trilha e aquisição de travas fora da ordem no lote. (`app/chamados/services/materializacao.py:2072`; `app/chamados/models.py:1629`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73`)

A cópia não contém metadados Git acessíveis; HEAD e comparação independente com `086f13522` ficaram **[nao verificado]**. Considerei como delta o patch anexado. Não há, nos dados fornecidos, linhas de produção que permitam medir os casos abaixo; incidência e quantidade reais são **[nao verificado]**.

Os três achados são:

- **Materialização aplica uma `escolha` com I sem T.** A seleção ampliada admite I sem T de qualquer tipo, desde que validada e não materializada. A recusa por C21 existe no ramo `hora`; o ramo `escolha` reconcilia destino/clones e retorna sucesso sem exigir resposta. O chamador dispensa o juiz de lastro para esse tipo e solicita o carimbo. (`app/chamados/services/materializacao.py:2071`; `app/chamados/services/materializacao.py:1315`; `app/chamados/services/materializacao.py:1643`; `app/chamados/services/materializacao.py:1903`; `app/chamados/services/materializacao.py:1948`) A validação faz C6 considerar a pergunta morta, permitindo gravar materialização/via sem corrigir o par ilegal. (`app/chamados/juizes.py:1405`; `app/chamados/models.py:3241`; `app/chamados/models.py:3263`)

- **Ressuscitação pode apagar T sem I sem guardar o texto anterior.** `q_nao_respondida()` passou a incluir esse legado no universo; a porta posteriormente zera o texto. Contudo, `linhas_da_projecao_sem_linha()` descarta perguntas sem instante e sem validação; o log posterior guarda apenas IDs e estados. Assim, para legado sem outra trilha, o texto desaparece, contrariando sua preservação exigida pela L-130. (`app/chamados/models.py:1533`; `app/chamados/models.py:1613`; `app/chamados/models.py:1629`; `app/chamados/models.py:4095`; `app/chamados/models.py:1656`; `app/docs/LEIS.md:151`)

- **O `atomic` externo corrigiu a atomicidade, mas introduziu ordem de trava incorreta entre itens.** O comando mantém os dois atos na mesma transação, enquanto cada chamada trava somente C/D/Ch/P daquele item. Para disputas distintas, fica `C → D1 → P1 → D2 → P2`, adquirindo D2 depois de P1. O próprio teste cria uma disputa para cada alvo. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73`; `app/chamados/services/validacao.py:429`; `app/chamados/models.py:2779`; `app/chamados/models.py:2796`; `app/chamados/tests/test_f1nova_b2_leitores.py:199`; `app/chamados/tests/test_f1nova_b2_leitores.py:208`) A ordem declarada exige travar o conjunto antes da primeira pergunta. Donos/disputas reais dos dois PKs e ocorrência de deadlock: **[nao verificado]**. (`app/chamados/models.py:2736`; `app/chamados/models.py:2759`)

**1. Os leitores migrados usam o mesmo significado de respondida, mas isso não garante que todos os efeitos respeitem esse significado.** C21 exige instante e texto após `strip()`. As faces SQL usam a classe de brancos derivada de `str.isspace()`, incluindo NBSP; a negativa explicita os dois ramos. (`app/chamados/juizes.py:1528`; `app/chamados/juizes.py:1552`; `app/chamados/juizes.py:1560`; `app/chamados/juizes.py:1580`; `app/chamados/juizes.py:1590`)

Os escritores dos valores consumidos nessa cadeia são:

| Valor | Quem escreve |
|---|---|
| `respondida_em` e `resposta_colab` na resposta humana | `responder_pergunta_disputa_core`: valida entrada, atribui ambos e grava resposta/trilha. (`app/chamados/services/disputa_emissao.py:1446`; `app/chamados/services/disputa_emissao.py:1481`; `app/chamados/services/disputa_emissao.py:1522`) |
| Par na resposta automática | `responder_automaticamente`, após consultar a porta de integridade. (`app/chamados/services/disputa_emissao.py:1672`; `app/chamados/services/disputa_emissao.py:1674`) |
| Limpeza do par, materialização e `via_resolucao` | `descarimbar_em_lote`, chamada pela ressuscitação e pela devolução do estoque. (`app/chamados/models.py:3392`; `app/chamados/models.py:1629`; `app/chamados/services/validacao.py:440`) |
| Materialização e `via_resolucao` | Portas do carimbo; a atribuição central fica em `_escrever_carimbo`. (`app/chamados/models.py:3208`; `app/chamados/models.py:3214`) |
| Status do chamado | `_gravar_estado`, alcançado pelos gates de reconciliar, reabrir e encerrar. (`app/chamados/models.py:201`; `app/chamados/models.py:228`; `app/chamados/models.py:1681`) |
| Fechamento da disputa | `DisputaSupervisao.fechar`, gravando instante, autor, resolução e motivo juntos. (`app/chamados/models.py:2013`) |
| Pauta de recusa de reabertura | `_escrever_pautas`, pela porta `pautas.services.escrever`, agrupando pela competência do dia do fato. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:139`) |

Para cada sítio de produção tocado pelo delta:

| Sítio | Leitura e consequência |
|---|---|
| `api_disputa_perguntas` | Lê I/T por C21 para `respondidas`; também serializa o par cru. Não grava esses campos. (`app/api/views.py:2561`; `app/api/views.py:2578`) |
| `api_disputa_responder` | Lê C21 para distinguir resposta/correção; entrega a escrita ao core acima. (`app/api/views.py:2622`; `app/api/views.py:2629`) |
| `canal.perguntas_mudas_com_resposta` | Lê a face negativa e cruza com respostas do fio; produz diagnóstico, sem copiar texto para a pergunta. (`app/chamados/canal.py:64`; `app/chamados/canal.py:76`) |
| `juizes.q_nao_respondida` | Define a face SQL negativa; não escreve. (`app/chamados/juizes.py:1583`) |
| Auditor: contraprova I21a | Lê resposta por C21 e cobertura de ausência; produz alarme. (`app/chamados/management/commands/auditar_invariantes_chamados.py:176`) |
| Auditor: I2 | Lê materialização, status terminal do destino, C21 e fechamento da disputa para separar piso/violação. (`app/chamados/management/commands/auditar_invariantes_chamados.py:588`) |
| Auditor: F1 | Lê status vivo, vitalidade e C21 para classificar fio mudo. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1632`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1658`) |
| Comando de estoque | Seleciona I sem T; lê via/par para reversão e delega a escrita à porta. DRY retorna antes da escrita. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:37`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:51`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:76`) |
| `_universo_da_ressuscitacao` | Lê materialização, face negativa e vias fora do fato; projeta o estado limpo antes de perguntar C6. A ressuscitação efetiva limpa par/carimbo e reabre a disputa. (`app/chamados/models.py:1530`; `app/chamados/models.py:1553`; `app/chamados/models.py:1629`; `app/chamados/models.py:1633`) |
| `acao_regularizar_disputa` | Lê C21 nas vivas; bloqueia sem resposta ou chama o materializador. (`app/chamados/services/acoes_disputa.py:134`; `app/chamados/services/acoes_disputa.py:143`) |
| `becos.classificar` | Lê C21, status do pai/destino e C7. A classificação não fecha; `fechar_beco` executa as portas. (`app/chamados/services/becos.py:30`; `app/chamados/services/becos.py:54`) |
| `declarar_hora_colab` | Lê C21 e igualdade do conteúdo; entrega resposta nova ao core. (`app/chamados/services/declaracao_texto.py:418`; `app/chamados/services/declaracao_texto.py:420`) |
| `aplicar_declaracao` | Lê C21 para decidir se precisa registrar resposta pela porta canônica. (`app/chamados/services/declaracao_texto.py:688`) |
| `reabrir_credor_de_pergunta_viva` | Lê C21, C6, status da âncora, competência e limbo; reabre pela porta ou registra recusa. (`app/chamados/services/disputa_emissao.py:1843`; `app/chamados/services/disputa_emissao.py:1857`; `app/chamados/services/disputa_emissao.py:1873`) |
| `processar_post_fio` | Lê C21 nas vivas para liberar regularização; chama materialização. (`app/chamados/services/fio.py:707`; `app/chamados/services/fio.py:717`) |
| Materialização da disputa | Lê as duas faces, validação e materialização. Pode escrever carimbo/via e reconciliar chamados; contém o primeiro achado. (`app/chamados/services/materializacao.py:2071`; `app/chamados/services/materializacao.py:2092`) |
| `particao_do_app` | Lê C21 e fechamento da disputa; somente particiona pendentes/histórico. (`app/chamados/services/pergunta_no_app.py:82`) |
| `impedimento_estoque_l130` | Lê integridade, validação, materialização, fora do fato, C6 e competência entregue; somente seleciona. (`app/chamados/services/validacao.py:390`) |
| `devolver_instante_sem_texto_l130` | Relê o impedimento sob trava; preserva projeção anterior, limpa pela porta do lote e exige trilha. (`app/chamados/services/validacao.py:429`; `app/chamados/services/validacao.py:436`; `app/chamados/services/validacao.py:455`) |
| `validar_pergunta` / `recusar_pergunta` | Leem C21 antes de validar/materializar ou devolver pela reabertura. (`app/chamados/services/validacao.py:682`; `app/chamados/services/validacao.py:743`; `app/chamados/services/validacao.py:902`; `app/chamados/services/validacao.py:925`) |
| `placar_aceite` | Lê face positiva e instante para contagem temporal; não grava resposta/status. (`app/chamados/services/verbos.py:333`; `app/chamados/services/verbos.py:344`) |
| `vigia_fio.censo_alcance` | Lê integridade, C21 e status terminal da âncora para nomear causas. (`app/chamados/services/vigia_fio.py:634`) |
| View de resposta | Lê C21 e entrega a escrita ao core. (`app/chamados/views.py:1280`; `app/chamados/views.py:1283`) |
| `validar_inline` | Seleciona vivas respondidas por C21 e chama a ação de validação. (`app/chamados/views_cobrar.py:170`; `app/chamados/views_cobrar.py:210`) |
| `ficha_do_colab` | Lê a lista sem veredito e C21 para apresentar quem deve agir. (`app/colaboradores/services/ficha.py:169`; `app/colaboradores/services/ficha.py:172`; `app/colaboradores/services/ficha.py:191`) |

A property atual **delega**, sem copiar a expressão. Entretanto, a seleção ampliada do materializador permite aplicar algo que o próprio juiz considera não respondido. (`app/chamados/models.py:3665`; `app/chamados/services/materializacao.py:2072`)

**2. Restaram decisões próprias de produção fora de C21.** Encontrei duas que precisam migrar ou mudar explicitamente de finalidade:

| Sítio | Classificação |
|---|---|
| `espera_o_texto_do_colaborador` | **Deve migrar.** Reimplementa `não respondida` como ausência de I ou I sem T. O resultado é equivalente hoje, inclusive no trim, mas continua sendo outro corpo decisor. Alimenta confirmação de envio, correção e cobrança. (`app/chamados/services/fio.py:187`; `app/chamados/services/fio.py:359`; `app/chamados/services/fio.py:386`; `app/chamados/services/cobranca.py:241`) |
| Card vivo de hora: “Você respondeu… escolha de novo para corrigir” | **Deve migrar ou rotular conteúdo legado.** A frase depende só de texto truthy; T sem I pode aparecer como respondida enquanto o botão continua “Enviar resposta”. (`app/templates/chamados/partials/_card_atendimento.html:141`; `app/templates/chamados/partials/_card_atendimento.html:161`; `app/chamados/services/fio.py:359`) |

Os demais usos diretos encontrados têm outra pergunta legítima:

| Usos | Classificação |
|---|---|
| Serialização de `respondida_em` | Exposição do cru, sem decidir respondida. (`app/api/views.py:1165`; `app/api/views.py:2563`; `app/api/views_mensageria.py:569`) |
| Ordenação, idade e comparação de instantes | Leitura temporal legítima após seleção própria da finalidade. (`app/chamados/veredito_lavrado.py:207`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:344`; `app/chamados/services/validacao.py:1256`) |
| Histórico/projeção e backfill de decisões | Reconstrução do ato ocorrido; não devem usar apenas C21 atual. Há, porém, a lacuna de preservação de T sem I apontada acima. (`app/chamados/models.py:4095`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/services/historico.py:99`) |
| Construção/conferência do par nas portas | Integridade de escrita, legítima. (`app/chamados/models.py:3241`; `app/chamados/models.py:3371`; `app/chamados/models.py:3586`) |
| Proteção contra sobrescrever texto na resposta automática | Preservação de conteúdo, legítima mesmo para legado sem I. (`app/chamados/services/disputa_emissao.py:1663`) |
| Cura por trilha e bloqueio de limbo na reabertura | Integridade/histórico, não definição geral de respondida. (`app/chamados/services/disputa_emissao.py:1692`; `app/chamados/services/disputa_emissao.py:1861`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46`) |
| Censo I sem T | Integridade pelo predicado real, legítima. (`app/chamados/services/vigia_fio.py:747`) |
| `resposta_display`, seletores e resposta no cartão histórico | Exibição/interpretação do conteúdo cru, legítima. (`app/chamados/models.py:3692`; `app/templates/chamados/partials/_card_atendimento.html:114`; `app/templates/chamados/partials/_card_atendimento.html:154`; `app/templates/chamados/partials/_card_atendimento.html:182`) |
| Parsing de hora, escapes e folga | Interpretação do valor, não classificação de respondida. (`app/chamados/services/materializacao.py:625`; `app/chamados/services/materializacao.py:1562`; `app/chamados/services/validacao.py:467`) |
| Guarda de testemunha de aplicação indisponível | Conserva bloqueio de ação quando existe conteúdo a analisar; finalidade distinta de C21. (`app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139`; `app/chamados/templatetags/chamados_tags.py:166`) |

O alarme I20 merece ajuste separado: procura texto diferente de `''`, sem trim, portanto também acusa espaços/NBSP sem I como preenchimento. É divergência de **integridade**, não outro leitor geral de respondida. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1106`; `app/chamados/juizes.py:1541`)

A afirmação de ausência absoluta de outros leitores, inclusive formas dinâmicas, fica **[nao verificado]**.

**3. No desligamento, os meios-pares deixam de reter a disputa; o resultado final também depende de vitalidade, tranca e outras perguntas.** A tabela assume pergunta não validada, não materializada e dia não trancado. (`app/chamados/models.py:1923`)

| Par | Retém por resposta sem veredito? | Consequência da cascata |
|---|---|---|
| I e T | Sim. | Pula o fechamento da disputa. (`app/colaboradores/services/desligamento.py:165`) |
| I sem T | Não. | Sem outra resposta legal retendo, a disputa pode fechar pela exceção de desligamento. (`app/chamados/models.py:1925`; `app/chamados/models.py:1999`) |
| T sem I | Não. | Mesma consequência; o legado deixou de ser retenção. (`app/chamados/models.py:1925`; `app/chamados/tests/test_e4b_c12_orm.py:229`) |
| Nenhum | Não. | Segue a porta de encerramento do silêncio. (`app/chamados/models.py:1999`) |

Na exceção, somente as perguntas que C6 considera vivas recebem `admin_decidiu`, linha SILENCIO e auditoria; depois fecha a disputa e tenta encerrar os chamados. Perguntas já mortas não entram nesse laço. (`app/chamados/models.py:1977`; `app/chamados/models.py:2000`; `app/chamados/models.py:2005`; `app/colaboradores/services/desligamento.py:187`)

**I∧T não garante manter aberto um chamado de competência entregue.** A guarda `_vivas_entre` exclui entregue, e `encerrar` não consulta o gate da conversa do pai. Logo pode permanecer disputa aberta com o chamado encerrado nesse recorte. (`app/chamados/models.py:364`; `app/chamados/models.py:1669`)

A Pauta desse caminho nasce de **recusa de reabertura coletada durante o ato**, agrupada pela competência; não nasce automaticamente porque existe uma pergunta exportada. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:139`; `app/chamados/services/fecho_runtime.py:165`)

Assim, há perda de Pauta no cenário T sem I: agora a disputa fecha antes, a pergunta viva recebe decisão e desaparece a recusa que originava a Pauta. O novo teste exige precisamente fechamento, `admin_decidiu` e zero Pauta. Isso é coerente com a substituição da retenção I OU T por I∧T e com a condição concreta do coletor de Pauta. (`app/chamados/tests/test_e4b_c12_orm.py:229`; `app/chamados/tests/test_e4b_c12_orm.py:232`; `app/chamados/models.py:1925`; `app/chamados/services/fecho_runtime.py:128`)

Isso **não** significa preservação integral de perguntas em entregue: a porta de desligamento pode gravar decisão/carimbo nelas. Preservação de exportações e fechamentos é uma afirmação mais estreita que preservação de perguntas. (`app/chamados/models.py:1999`; `app/chamados/tests/test_e4b_c12_orm.py:230`; `app/chamados/tests/test_e4b_c12_orm.py:233`)

**4. As mudanças dos três testes destacados não têm a mesma natureza.** [nao verificado]

- **`test_e3_degrau_dono_e_porta.py`:** conserva sujeito e assertions de partição, carga, C6 e relógio; acrescenta T à fixture que representa resposta legal. Não identifiquei enfraquecimento dessas assertions. (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:262`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:271`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:289`; `app/chamados/tests/test_e3_degrau_dono_e_porta.py:320`)

- **`test_e1_v10.py`:** altera a fixture `_ln` para fornecer I junto de T; mantém as verificações de mérito, partição e retratação. (`app/chamados/tests/test_e1_v10.py:118`; `app/chamados/tests/test_e1_v10.py:152`; `app/chamados/tests/test_e1_v10.py:169`)

- **`test_e4b_c12_orm.py`:** troca efetivamente o cenário geral de desligamento para resposta legal, mas essa resposta adicional está em **competência aberta**. O legado anterior permanece em teste separado. A mudança é compatível com I∧T, porém o `test_RED_desligamento` deixou de provar emissão de Pauta nesse gatilho e não prova retenção de uma resposta ancorada somente em entregue. (`app/chamados/tests/test_e4b_c12_orm.py:57`; `app/chamados/tests/test_e4b_c12_orm.py:148`; `app/chamados/tests/test_e4b_c12_orm.py:174`; `app/chamados/tests/test_e4b_c12_orm.py:207`)

Há **redução objetiva de cobertura** em `test_o261_resposta_vazia`: o estado completo incluía materialização, autor, via, I, validação e T; agora a comparação verifica apenas I/T. O fechamento permitido explica parte da mudança, mas faltam assertions específicas para os demais campos e para o novo fechamento da disputa nesse caso. (`app/chamados/tests/test_o261_resposta_vazia.py:74`; `app/chamados/tests/test_o261_resposta_vazia.py:113`)

`e4_apoio.juiz_respondida()` executa os corpos **reais extraídos da fonte** de `pergunta_respondida` e `texto_vazio`; não reimplementa a regra. É execução isolada desses corpos, não importação integral do módulo Django. (`app/chamados/tests/e4_apoio.py:8`; `app/chamados/tests/e4_apoio.py:19`)

O novo teste sentinela da property melhora a prova: exige delegação, impedindo que uma cópia da expressão passe apenas por produzir valores iguais. (`app/chamados/tests/test_f1nova_b2_leitores.py:87`)

**5. As classes de reprovação aparecem, com estas distinções:**

| Classe | Resultado |
|---|---|
| Segundo juiz/derivação paralela | Restou o decisor de `fio.espera_o_texto_do_colaborador`; o template também mantém uma decisão por texto cru. (`app/chamados/services/fio.py:187`; `app/templates/chamados/partials/_card_atendimento.html:141`) |
| Segundo escritor fora da porta | Os novos caminhos examinados delegam às portas existentes; os defeitos encontrados são de seleção, trilha e ordem, mesmo usando essas portas. (`app/chamados/services/validacao.py:440`; `app/chamados/services/materializacao.py:1948`) |
| Relógio fora da borda | O delta captura `hoje` no comando e o passa à seleção e à porta; não encontrei novo relógio dentro de C21. (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:36`; `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:7

[truncado em 20000 de 23857 caracteres]

## 16/11 23:54 · sessao d698d1708027 · REVISAO-AVAL-2355 · verificador · origem=Code

1. **Premissas:** as quatro condições conferem com a enumeração da BUG-PAR; o complemento agora as atribui expressamente à F2, respondendo à devolução. LU2-07 permanece aberta. A sequência histórica completa até F7 é **[nao verificado]**; BUG-PAR → B2 → F2 → F4 tem respaldo documental. (app/docs/RELATO.md:158; app/docs/RELATO.md:4; app/docs/RELATO.md:349; app/docs/CORTES.md:495; anexo complemento1_prepara_f2_f4_f7_1611.txt:1)

2. **Colisão:** a precedência anterior era F1 → CLASSE-D → F2 → E4d. O complemento a altera nominalmente quanto à F2 e preserva CLASSE-D antes da E4d; portanto resolve a colisão apontada. Também substitui o encaixe técnico anterior de R1 entre F1/F2, que independia de LU2-07/08: esperar LU2-07 passa a ser ordem expressa, não dependência técnica demonstrada. (app/docs/BACKLOG.md:1990; app/docs/RELATO.md:368; app/docs/RELATO.md:416; anexo complemento1_prepara_f2_f4_f7_1611.txt:1)

3. **Literal:** fecha como autorização condicional. B2 consta aberta e “em voo”; seu pouso é **[nao verificado]**. Lei 0 da F2 está registrada na base `520d7b557`; cumprimento das quatro condições no HEAD pós-B2 é **[nao verificado]**. Cortar LU2-07 tampouco comprova fechamento integral da CLASSE-D: LU2-08 conserva parte aberta. São condições ainda por conferir, sem invalidar a resposta à devolução. (app/docs/RELATO.md:25; app/docs/RELATO.md:21; app/docs/RELATO.md:11; app/docs/RELATO.md:347; app/docs/RELATO.md:4)

4. **Exportada, dinheiro ou `!`:** o complemento declara registro e reordenação; não autoriza esses atos adicionais. Permanecem a guarda da F2 sobre exportada — DIFF mudou, PAREI `!` — e a conferência do recorte `_nascer`. Efeitos de execução futura sobre dinheiro ou gravado são **[nao verificado]**. (anexo complemento1_prepara_f2_f4_f7_1611.txt:1; app/docs/CORTES.md:487; app/docs/RELATO.md:468; CLAUDE.md:265)

**VEREDITO: CONFERE**

## 16/11 23:54 · sessao 5c40158a9144 · REVISAO-AVAL-2355 · verificador · origem=Code

1. **Premissas: parcialmente conferidas.** A recusa de `4633d2c17`, as docstrings nos dois arquivos e os 30+ descendentes/B2/AUSÊNCIA estão registrados em `app/docs/RELATO.md:8`. A classificação desses caminhos como negócio confere com `bin/tests/test_lei_akita.sh:152`; eles estão fora do registro de medida (`bin/tests/test_lei_akita.sh:172`). SHA completo, diff histórico, descendência e aceitação efetiva pelo hook: **[não verificado]**.

2. **Colisão: item 2 precisa de ressalva.** A nova exceção nominal segue a forma dos precedentes: crescimento exige novo aval, sem regravar histórico (`app/docs/CORTES.md:449`; `app/docs/CORTES.md:484`). Porém, “n/a medida ou instrumento tocando arquivo de negócio = recusa”, como proibição absoluta por arquivo, elimina a permissão vigente de alterar dados medidos nos módulos registrados: o juiz expressamente admite esse caso (`bin/tests/test_lei_akita.sh:348`; `app/docs/BACKLOG.md:1955`; `app/docs/RELATO.md:2095`). O literal precisa preservar L-123-MEDIDA-EM-PY.

3. **Condições literais: o predicado compartilhado já existe.** O hook chama o mesmo juiz utilizado pelo push (`bin/commit_msg.sh:29`; `bin/tests/test_lei_akita.sh:529`). Há limites declarados para `--amend` e cherry-pick/rebase; a causa concreta deste caso permanece **[não verificado]** (`bin/commit_msg.sh:10`). A catraca atual é 12, e o teste acrescenta um item dinamicamente, compatível com cobrar o 14º após a alteração (`bin/tests/test_lei_akita.sh:409`; `bin/tests/test_lei_akita.sh:990`). Os **45 commits** não estão comprovados: a tentativa documentada tinha 34 (`app/docs/RELATO.md:20`; **[não verificado]**).

4. **Exportada, dinheiro ou outro `!`: efeito adicional [não verificado].** O `!` documentado é justamente exceção de selo ou regravação (`app/docs/RELATO.md:7`). O pouso próprio do instrumento respeita o corte vigente (`app/docs/CORTES.md:304`); integridade financeira/exportada do pacote completo: **[não verificado]**.

**VEREDITO: PROCEDE (item 2: preservar explicitamente a permissão L-123-MEDIDA-EM-PY ao definir a recusa; `bin/tests/test_lei_akita.sh:348`).**

## 16/11 23:53 · sessao 24271795b704 · - · - · origem=Ronald (tmux)

**Item 3: PROCEDE — o RED ainda não está suficientemente condicionado.** Irmã U163/ausente elimina o bloqueio herdado pelo K6 da irmã, porque o predicado atual pula U163; o controle com irmã fora163 entregue confere. (`app/chamados/services/materializacao.py:795`; `app/chamados/services/validacao.py:224`; `app/chamados/tests/test_bug_par_volta.py:145`)

O literal ainda admite casos que **hoje não gravam**: U163 com declarado diferente do gravado espera mesmo com F6 aberto; ausência de irmã com vizinha do mesmo tipo impede fechar a paridade; batida aprovada existente resulta em `ja_satisfeita`, sem nova batida. (`app/chamados/services/materializacao.py:933`; `app/chamados/services/materializacao.py:936`; `app/chamados/services/materializacao.py:194`; `app/chamados/services/materializacao.py:903`; `app/chamados/services/materializacao.py:928`; `app/chamados/services/materializacao.py:898`)

Para tornar o RED discriminante, explicitar: metade de intervalo respondida, sem validação/materialização anterior, declarado igual ao gravado, plantio permitido pelas guardas de data/paridade/colisão, sem reuso e implausibilidade ausente ou devidamente confirmada; F6 aberto deve corresponder à competência do instante local de cada plantio. (`app/chamados/services/validacao.py:594`; `app/chamados/services/validacao.py:597`; `app/chamados/services/validacao.py:609`; `app/chamados/services/validacao.py:615`; `app/chamados/services/validacao.py:681`; `app/chamados/services/materializacao.py:891`; `app/chamados/services/materializacao.py:914`; `app/ponto/janelas.py:84`)

A existência/execução de tal RED e de pergunta real que cumpra todas essas condições permanece [nao verificado].

## 16/11 23:53 · sessao 30eb8b95c3ab · - · - · origem=Ronald (tmux)

2. O complemento responde às três dúvidas registradas e mantém o encaixe F4 sobre B2; a exceção anterior U163 entregue/F6 precisa ser sucedida no registro e na matriz. (`app/docs/RELATO.md:5`; `app/docs/RELATO.md:21`; `app/docs/RELATO.md:44`; `app/docs/RELATO.md:52`; `app/docs/BACKLOG.md:823`) Continua vigente F4 logo atrás de F1/F2 **ou** contador J1 esperado zero no intervalo. (`app/docs/CORTES.md:487`)

4. O bloqueio ocorre antes do carimbo/recalculo no admin e antes de recusa/pendência/carimbo no escritor; corresponde à preservação declarada. (`app/chamados/services/validacao.py:652`; `app/chamados/services/validacao.py:655`; `app/chamados/services/validacao.py:666`; `app/chamados/services/materializacao.py:1829`) Recalculo herdado já está apontado; o complemento não autoriza alterar FechamentoMensal exportado, que continua sob `!`. (`app/docs/RELATO.md:44`; `app/docs/RELATO.md:52`; `CLAUDE.md:411`) Impacto monetário real: [nao verificado].

Cuidado de implementação: remover simplesmente a exclusão U163 do segundo laço faz sobreposição com U103 já inserida pelo primeiro; a lista deve conservar uma ocorrência por pergunta. (`app/chamados/management/commands/bug_par_103.py:53`; `app/chamados/management/commands/bug_par_103.py:63`; `app/chamados/management/commands/bug_par_103.py:71`; `app/chamados/management/commands/bug_par_103.py:77`; `app/chamados/services/universo_bug_par.py:30`)

## 16/11 23:53 · sessao 8556b2524238 · - · - · origem=Ronald (tmux)

- **Premissas documentadas:** `RELATO.md:7-8` registra a recusa de `4633d2c17`, `padrao=n/a (medida)`, somente docstrings nos dois arquivos, proposta de catraca 12→13 e descendência das cópias B2/AUSENCIA; os fatos de produção permanecem **[não verificado]**.
- **Precedentes compatíveis:** crescer a exceção exige novo aval e não regravar histórico (`CORTES.md:449`); a proibição literal posterior refere-se a commit novo **sem `padrao=`**, condição diferente de `n/a (medida)` (`CORTES.md:484`; `RELATO.md:8`). [nao verificado]
- **Causa já prevista:** o hook deveria recusar `n/a (instrumento)` tocando negócio no commit (`BACKLOG.md:778`); seus limites conhecidos incluem `--amend` julgando somente o delta e diferença de vocabulário (`BACKLOG.md:780`; `bin/commit_msg.sh:10-11`).
- **Implementação ainda futura:** catraca atual 12 (`bin/tests/test_lei_akita.sh:409`); o caso de item extra é construído dinamicamente, portanto pode continuar cobrando o 14º após a lista passar a 13 (`bin/tests/test_lei_akita.sh:990-999`).
- **Condição dos 45:** a tentativa anterior documentada tinha 34 commits (`RELATO.md:20`); quantidade atual 45 e presença no ar **[não verificado]**. [nao verificado]
- **Registro e pouso:** frase em CORTES deve entrar pela geração a partir de JSON (`CORTES.md:3`); pouso próprio do instrumento respeita o corte vigente (`CORTES.md:304`). [nao verificado]
- **Escopo:** o bloqueio declarado é justamente `!` para exceção de selo/regravação (`RELATO.md:7`); efeito adicional em dinheiro/exportadas pelo push completo **[não verificado]**. [nao verificado]

## 16/11 23:53 · sessao c14be20c4fcf · - · - · origem=Ronald (tmux)

- A devolução anterior está transcrita em `app/docs/RELATO.md:4`: faltava declarar aplicação das quatro condições à F2 e resolver a precedência CLASSE-D. O complemento fornecido responde expressamente aos dois pontos; sua presença física na cópia é **[nao verificado]**.
- A enumeração original confere: lei 0, revisão sem procedência, cruzamento 0 e vaga livre, para BUG-PAR (`app/docs/RELATO.md:158`). F2 atualmente registra lei 0 sobre base `520d7b557`, mas isso não comprova as quatro condições no futuro HEAD pós-B2 (`app/docs/RELATO.md:11`).
- B2 está documentalmente **aberta**, com construtor na cópia `wt-e4c-b2-215705`; pouso não demonstrado (`app/docs/RELATO.md:25`). O texto “contra o HEAD com B2 pousada” é condição futura, não alegação de pouso já cumprido.
- A precedência antiga é explícita: F1 → CLASSE-D → F2 → E4d (`app/docs/RELATO.md:368`; `app/docs/PROMPTS.md:2331`). O complemento a modifica nominalmente quanto à F2 e conserva CLASSE-D antes E4d; não cabe reapresentar a mera mudança autorizada como conflito não resolvido.
- Atenção ao recorte R1: seu encaixe técnico manda pousar sozinho entre F1/F2, **sem esperar LU2-07/08**, pois não depende deles (`app/docs/RELATO.md:416`). O complemento agora adia CLASSE-D até LU2-07; isso altera também esse encaixe. Como o novo texto trata CLASSE-D inteira, pode ser reordenação expressa suficiente; não presumir exclusão do R1.
- LU2-07 segue aberta, com questão emp1 e parte relativa à competência 10 paga; não é lei 0 da CLASSE-D (`app/docs/RELATO.md:349`). LU2-08 também conserva parte aberta (`app/docs/RELATO.md:347`); cortar LU2-07 não demonstra automaticamente fechamento integral da CLASSE-D.
- F2 preserva a exigência de conferência do recorte `_nascer`; F4 exige kernel e `!` para retificação exportada; F7 exige smoke posterior com reversão (`app/docs/RELATO.md:468`). O complemento de ordem não demonstra executar esses atos nem os dispensa.
- A ordem literal completa “BUG-PAR, F1 B2, F2, F4, F7”, atribuída à tarde de 16/11, não foi localizada em RELATO/PROMPTS: **[nao verificado]**. Há ordem madrugada BUG-PAR → B2 em `app/docs/RELATO.md:238`, exigência F7 depois F1+F2+BUG-PAR em `app/docs/RELATO.md:425`, e a preparação atual em `app/docs/PROMPTS.md:2445`.

## 16/11 23:53 · sessao b5d6d387d6a2 · - · - · origem=Ronald (tmux)

Achados na base indicada:

- A devolução é precisamente a dúvida respondida pelo complemento: quais quatro condições valem para F2 e se espera CLASSE-D. A cópia registra F2 bloqueada até essa resposta. (`app/docs/RELATO.md:4`)
- A enumeração anterior pertence à BUG-PAR: lei 0, revisão sem procedência, cruzamento 0 e vaga livre. O anexo agora as atribui expressamente à F2; não precisa inferir aplicação automática. (`app/docs/RELATO.md:158`; complemento anexo sem arquivo:linha: [nao verificado])
- CLASSE-D estava literalmente após F1 e antes F2. A exceção nominal do complemento substitui essa precedência; conservar CLASSE-D antes E4d é compatível com o registro. (`app/docs/BACKLOG.md:1990`; `app/docs/RELATO.md:368`)
- LU2-07 continua aberta na cópia. Entretanto R1 foi destacado como trabalho independente que não espera LU2-07/08, por decisão técnica contestável. O complemento agora posterga CLASSE-D nominalmente; sua justificativa não demonstra que toda a obra dependia da lei. (`app/docs/RELATO.md:349`; `app/docs/RELATO.md:416`)
- B2 já consta **aberta**, com as quatro condições e cópia nomeada; o estado posterior ainda a chama de “em voo”. Pouso B2: **[nao verificado]**. Assim “HEAD com B2 pousada” funciona como condição futura, não como estado comprovado. (`app/docs/RELATO.md:25`; `app/docs/RELATO.md:21`)
- F2 possui remedida registrada com lei 0 na base `520d7b557`; isso não comprova lei 0, revisão, cruzamento e vaga no futuro HEAD com B2 pousada. (`app/docs/RELATO.md:11`; `app/docs/RELATO.md:25`)
- BUG-PAR consta pousada. PREPARA F2/F4/F7 e U163 como complemento da F4 constam no BACKLOG; a devolução U163 é separada e permanece explícita. (`app/docs/BACKLOG.md:2084`; `app/docs/BACKLOG.md:822`; `app/docs/BACKLOG.md:823`; `app/docs/RELATO.md:5`)
- A ordem BUG-PAR, F1 B2, F2 e F4 é citada pelo corte de competência paga, que proíbe essa lateral entrar na frente. F7 após F4 especificamente: **[nao verificado]** nesta leitura do BACKLOG. (`app/docs/CORTES.md:495`)
- Restrições de pouso já registradas permanecem: F2 exige conferência do recorte `_nascer`; F4 envolve kernel e `!` para retificação exportada; F7 exige smoke depois e reversão. A nova regra de abertura não declara dispensa dessas condições. (`app/docs/RELATO.md:468`)

## 16/11 23:53 · sessao f7cea2c7c8fd · - · - · origem=Ronald (tmux)

- O hook já chama o mesmo `julgar_commit` usado no push; a divergência não prova predicados distintos. O próprio contrato declara limites em `--amend` e cherry-pick/rebase sem `--edit`. Causa concreta da aceitação de `4633d2c17`: [não verificado]. (`bin/commit_msg.sh:10`, `bin/commit_msg.sh:29`, `bin/tests/test_lei_akita.sh:437`, `bin/tests/test_lei_akita.sh:529`).
- A recusa de `n/a (medida)` em docstring desses módulos é compatível com o juiz: caminho em `app/` fora de testes/docs é negócio; os dois módulos não pertencem ao registro fechado; mudança de docstring tampouco é dado medido. (`bin/tests/test_lei_akita.sh:152`, `bin/tests/test_lei_akita.sh:165`, `bin/tests/test_lei_akita.sh:172`, `bin/tests/test_lei_akita.sh:322`, `bin/tests/test_lei_akita.sh:347`).
- A cura literal “n/a medida ou instrumento tocando arquivo de negócio = recusa” precisa preservar a exceção vigente de dados registrados; transformá-la em recusa indiscriminada de arquivos `app/` conflita com L-123-MEDIDA-EM-PY já pousada. (`bin/tests/test_lei_akita.sh:347`, `app/docs/BACKLOG.md:1955`, `app/docs/RELATO.md:2095`).
- A forma nominal proposta corresponde ao precedente: SHA completo, motivo, conjunto fechado, catraca e caso adicional vermelho. A cópia ainda contém 12 entradas, sem o novo SHA; o MORDE atual é do 13º item. (`app/docs/CORTES.md:484`, `bin/tests/test_lei_akita.sh:395`, `bin/tests/test_lei_akita.sh:409`, `bin/tests/test_lei_akita.sh:986`).
- A exceção histórica só age no push, depois de julgar o SHA; o modo commit julga sem allowlist/catraca. Portanto a nova exceção não cura, por si, a aceitação indevida de commit novo. (`bin/tests/test_lei_akita.sh:529`, `bin/tests/test_lei_akita.sh:536`).
- `RELATO.md:8` registra recusa, docstrings 3+6 e descendência de 30+ commits/B2/AUSÊNCIA. Logs originais, SHA completo, diff histórico e execução efetiva do hook: [não verificado]. [nao verificado]
- O selo do hook testa `n/a (instrumento)` em negócio, mas sua bateria não inclui `n/a (medida)` em negócio; esse cenário não está coberto ali. (`bin/tests/test_hook_mensagem_em_toda_copia.sh:74`).

Possível fundamento de **PROCEDE, item 2**: predicado compartilhado já existe, causa não demonstrada e a redação da cura precisa manter L-123-MEDIDA-EM-PY. (`bin/commit_msg.sh:29`, `bin/tests/test_lei_akita.sh:347`, `bin/tests/test_lei_akita.sh:437`; causa: [não verificado]).

## 16/11 23:53 · sessao 57325c0c3f95 · - · - · origem=Ronald (tmux)

- A precedência anterior está literal: CLASSE-D após F1 e antes da F2; E4d depois. O complemento fornece exceção nominal expressa a essa posição, preservando antes da E4d. A posição antiga, por si, não gera colisão residual. ([BACKLOG.md:1990](app/docs/BACKLOG.md:1990); [RELATO.md:368](app/docs/RELATO.md:368); exceção: anexo fornecido, sem arquivo:linha da cópia [nao verificado]).
- LU2-07 está ABERTA. Há, porém, registro de que **R1 da CLASSE-D independe de LU2-07/08** e podia pousar entre F1/F2. Assim, “a CLASSE-D tem pergunta aberta” confere, mas “toda CLASSE-D depende do corte LU2-07” não se demonstra. O anexo ordena esperar esse corte; isso é nova ordem expressa, não consequência técnica provada. ([RELATO.md:349](app/docs/RELATO.md:349); [RELATO.md:416](app/docs/RELATO.md:416)).
- A regra de seguir outro item sem depender da pergunta está literal; ela sustenta continuar F2 quando a pergunta da CLASSE-D não a bloqueia. ([CLAUDE.md:269](CLAUDE.md:269); [CLAUDE.md:270](CLAUDE.md:270)).
- A exceção de ordem não afasta as guardas de F2: selo da face SQL versus juiz na sombra inteira; supra_juiz em exportada não muda, DIFF que muda implica PAREI `!`. ([CORTES.md:487](app/docs/CORTES.md:487)).
- O complemento não declara apply de dinheiro, alteração de escala/vínculo ou exportada; efeitos de execução futura permanecem [nao verificado]. Essas escritas exigem a guarda específica; exportada gravada exige `!`, e dinheiro em não exportada exige reversão, hash da exportada intacta e prova de frota. ([CLAUDE.md:265](CLAUDE.md:265); [CLAUDE.md:406](CLAUDE.md:406); [CLAUDE.md:411](CLAUDE.md:411)).

## 16/11 23:51 · sessao e6a9726f3f13 · - · - · origem=Ronald (tmux)

Confirmei a violação de ordem introduzida pelo `atomic` externo do delta: o comando mantém uma transação para os dois alvos, mas adquire as travas por pergunta, sem travar antes a união do lote (`app/chamados/management/commands/estoque_l130_instante_sem_texto.py:73`, `app/chamados/services/validacao.py:429`).

No próprio cenário de teste, cada alvo nasce em disputa distinta (`app/chamados/tests/test_f1nova_b2_leitores.py:199`, `app/chamados/tests/test_f1nova_b2_leitores.py:208`). A sequência fica:

`C → D1 → P38251 → C(reentrante) → D2(nova) → P38252`.

Isso contraria a ordem explícita de adquirir todas as disputas/chamados antes da primeira pergunta; o helper ordena apenas os conjuntos recebidos em **cada chamada** (`app/chamados/models.py:2736`, `app/chamados/models.py:2770`, `app/chamados/models.py:2796`). Se os donos forem diferentes, também falta travar todos os colaboradores por PK no topo (`app/chamados/models.py:2759`).

A correção preserva o `atomic` externo e pré-trava a união dos dois alvos antes do laço; as portas depois reentram nas mesmas linhas (`app/chamados/models.py:2741`).

Donos/disputas reais de 38251/38252 e ocorrência de deadlock: [não verificado]. A catraca estrutural existente não certifica essa sequência entre chamadas; seu observador por tabela declara que não vê outra PK da mesma tabela adquirida depois (`app/chamados/tests/test_e1_v7_catracas.py:39`).

## 16/11 23:49 · sessao 0e37165938ec · - · - · origem=Ronald (tmux)

Refinamento: `_card_atendimento.html:141-144` deve migrar ou receber rótulo explícito de texto legado. O gate externo é `p.viva` (`:77`) e tipo hora (`:117`), sem exigir `p.respondida`/`resposta_enviada`. T sem I pode ser viva numa célula acusadora (`app/chamados/juizes.py:1423`, `:1454-1457`) e PENDENTE (`app/chamados/models.py:3155-3156`). Nesse caso a tela diz “Você respondeu […] escolha de novo para corrigir” enquanto o botão diz “Enviar resposta” (`app/templates/chamados/partials/_card_atendimento.html:141`, `:161`; `app/chamados/services/fio.py:187`, `:359`).

“ Sua resposta” em `_card_atendimento.html:182-184` está no `else` de `p.viva` (`:179`), no cartão histórico sem formulário: exposição legítima do conteúdo cru, distinguível do primeiro ramo.

Nenhum novo teste foi executado nesta revisão. [não verificado] [nao verificado]

## 16/11 23:49 · sessao f82102ce8d71 · - · - · origem=Ronald (tmux)

Dois achados concretos do delta:

1. **Materializador aplica pergunta “escolha” sem resposta, embora o comentário prometa apenas recusa.** A união nova seleciona qualquer `tipo_resposta` com I, texto vazio, validação e sem materialização (`app/chamados/services/materializacao.py:2071`). O despacho encaminha escolha (`:1896`); seu ramo ignora a resposta, reconcilia destino e retorna `True` (`:1638-1647`). O juiz do lastro é dispensado para escolha (`:1903`), e o carimbo é solicitado (`:1948`). `carimbar` conserva o par vazio (`app/chamados/models.py:3241`), C6 devolve morta pela validação (`app/chamados/juizes.py:1405`) e salva materialização/via (`app/chamados/models.py:3263`). Isso pode alcançar entregue: K6 desse caminho só consulta metades de intervalo fora163 (`app/chamados/services/materializacao.py:792-807`); escolha comum não planta batida, portanto também não passa pela guarda F6 do registro de batida. Incidência real: [não verificado].

   Reproduzi os corpos AST reais `_materializar_uma` e `_materializar_resposta_escolha`, sem Django/banco, com dependências laterais falsas: escolha, I presente, `T=''`, validada, não materializada → retorno `True`, chamadas `reconciliar_destino` e `carimbar(... resposta='validar', lavrar_veredito=True)`, erros `[]`. O juiz do lastro, instrumentado para explodir se chamado, não foi chamado. A limitação é que não houve execução ORM. (`app/chamados/services/materializacao.py:1813`, `:1630`.)

2. **Ressuscitação passa a apagar texto sem instante sem garantir preservar o texto anterior.** A seleção nova `q_nao_respondida` inclui T¬I materializada (`app/chamados/models.py:1530-1536`); a projeção zera o par e, se C6 aprovar, inclui o PK (`:1553-1557`). A porta registra somente `linhas_da_projecao_sem_linha` (`:1613`) e depois zera texto (`:1629`). Essa função descarta pergunta sem I e sem validação (`:4095-4097`); o log posterior guarda IDs/estados, não texto (`:1653-1662`). `linhas_do_evento` guarda somente célula/colaborador/data (`:3336-3339`), e o lote usa `update`, sem `save` (`:3392-3394`).

   Reproduzi a função AST real `linhas_da_projecao_sem_linha` para T=`'08:00'`, I=None, validação=None: retorno `[]`, sem chamada ORM. (`app/chamados/models.py:4085`.)

   Caminho cartório alcançável: T¬I materializada por `admin_decidiu`, sem validação, marco apagado, célula acusando; C6 atual morta pela identidade (`app/chamados/juizes.py:1443-1445`), C6 projetado limpa a identidade e fica viva (`:1454-1457`). Cartório passa por `_fio_vivo=False`, juiz da resposta=False, não é tudo duplicata, chama ressuscitar (`app/ponto/services/cartorio.py:1643-1674`). Esse bloco não consulta entregue; cron carrega todos os dias passados (`app/ponto/management/commands/processar_cartorio.py:49-62`). Impressão igual pode impedir executar agora, mas insumo novo ou `--forcar` permite chegar (`app/ponto/services/cartorio.py:1409-1415`). Incidência em produção: [não verificado].

Sobre desligamento e os quatro pares:

- Assumindo não validada, não materializada e dia não trancado: I∧T entra em `respostas_sem_veredito`; I¬T, ¬IT e nenhum ficam fora (`app/chamados/models.py:1923-1926`). Dia trancado também fica fora; `ignorar_tranca=True` é usado por CSF/retratação, não pelo desligamento (`:1926`; `app/ponto/supra_juiz.py:573-574`).
- I∧T: desligamento pula o fechamento da disputa (`app/colaboradores/services/desligamento.py:164-167`). O chamado só é retido pela guarda da pergunta viva **em competência aberta** (`app/chamados/models.py:364-368`), pois `encerrar` não usa gate da conversa (`:1669-1683`). Portanto não generalizar “resposta legal segura chamado exportado”.
- Outros três pares: se não houver outra resposta legal segurando a disputa, a porta de exceção fecha e carimba **as perguntas vivas** como `admin_decidiu`, registra SILENCIO e auditoria (`app/chamados/models.py:1999-2017`); depois `encerrar` fecha o chamado (`app/colaboradores/services/desligamento.py:182-190`). Pergunta morta não recebe esse carimbo, pois o laço é só de `vivas` (`app/chamados/models.py:1976-1977`, `:2000`).
- Relativo à regra antiga I OU T, os dois meios-pares deixam de reter. A coerência com “respondida=I∧T” existe; disso não decorre ausência de escrita em entregue. A porta desligamento não filtra competência antes de matar mudas (`app/chamados/models.py:1999-2017`).
- A Pauta é de **recusa de reabertura coletada no ato**, não consequência automática de ser exportada (`app/chamados/services/fecho_runtime.py:16-20`, `:118-141`, `:161-169`; `app/chamados/services/disputa_emissao.py:1969-1974`). Se a disputa fecha antes e a pergunta morre, não sobra a recusa que antes gerava Pauta; o teste legado confirma esse novo efeito (`app/chamados/tests/test_e4b_c12_orm.py:225-234`).

O teste C12 não prova retenção de resposta na própria entregue: cria a pergunta principal exportada (`app/chamados/tests/test_e4b_c12_orm.py:49-56`) e a resposta legal adicional **na aberta** (`:57-62`). Seu zero Pauta e chamado aberto dependem desta segunda pergunta (`:167-175`). O comentário “exportada ou não” é mais amplo que o cenário exercitado; falta o caso resposta legal ancorada somente em entregue. (`app/chamados/models.py:364-365`.)

Quanto aos crons:

- `fechar_becos_disputa` instalado usa `--alarme` (`app/config/crons.py:663`); isso vence `--apply`, e portanto essa execução não fecha (`app/chamados/management/commands/fechar_becos_disputa.py:30`). O uso explícito de `--apply` pode fechar por porta (`:57`; `app/chamados/services/becos.py:54-58`).
- Vigia `censo_alcance` declara leitura pura e muda classificação, não estado de pergunta (`app/chamados/services/vigia_fio.py:490-493`, `:634-637`).
- Desligamento agendado instalado roda `--apply` (`app/config/crons.py:440`), chama a mesma cascata (`app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`).
- Cartório pode escrever via ressuscitação, inclusive entregue, conforme achado 2. (`app/ponto/services/cartorio.py:1674`.)
- Materialização pode escrever por seleção ampliada, inclusive entregue no ramo escolha, conforme achado 1. (`app/chamados/services/materializacao.py:2071`, `:1630`.)
- CSF/reconciliador usam retenção comum: pares ilegais não seguram mais CSF/retratação (`app/ponto/supra_juiz.py:573-588`; `app/chamados/reconciliador.py:448-451`, `:520-526`). Porém a alteração do corpo `sem_veredito_entre` não aparece no delta anexado, logo atribuí-la especificamente à volta 2 seria [não verificado]. O delta de property/function explícita, por si, pode ser equivalente caso a property da base já desse I∧T. Base Git indisponível: [não verificado].

## 16/11 23:49 · sessao e6a9726f3f13 · - · - · origem=Ronald (tmux)

Achado bloqueante no delta: a união em `materializacao.py:2072` admite **toda** pergunta validada com I sem T, embora o comentário prometa admiti-la só para recusa. A recusa pelo juiz está apenas no ramo `hora` (`app/chamados/services/materializacao.py:1314`). No ramo `escolha`, a função reconcilia destino/clones e retorna `True` sem exigir resposta (`app/chamados/services/materializacao.py:1630`). O chamador dispensa o juiz de lastro para esse tipo (`app/chamados/services/materializacao.py:1903`).

Esse caminho realmente pode carimbar: a pergunta selecionada já é validada (`app/chamados/services/materializacao.py:2073`), condição que faz C6 retornar morta imediatamente (`app/chamados/juizes.py:1404`). `carimbar` preserva o par I sem T e salva materialização/via/lavra (`app/chamados/models.py:3241`, `app/chamados/models.py:3263`); a guarda do par retorna quando `update_fields` não contém seus campos (`app/chamados/models.py:3576`). K6 só alcança metades de intervalo fora163, portanto não barra uma escolha `gps_ausente` exportada (`app/chamados/services/materializacao.py:792`, `app/chamados/services/materializacao.py:1833`).

Reprodução executada em memória, sem arquivos/banco: extraí por AST as funções reais `_materializar_uma`, `_materializar_resposta_escolha`, `carimbar`, `_escrever_carimbo`, `_recusa_par_ilegal`, `_pergunta_viva_calc` e `texto_vazio`; I/O/contextos foram fakes. Para `escolha/gps_ausente`, I sem T e validada: C6=False, materialização=True, chamadas capturadas de reconciliação/lavra/save, via final `chamado_encerrado`, nenhum erro. Seleção SQL real, banco, ocorrência e quantidade em produção: [não verificado].

Sobre os testes: [nao verificado]

- `e4_apoio.juiz_respondida` executa o juiz **real extraído da fonte**, com `texto_vazio` real; não reimplementa a expressão (`app/chamados/tests/e4_apoio.py:8`, `app/chamados/tests/e4_apoio.py:19`). `fonte_real` também respeita mutações em memória (`app/chamados/tests/e4_catracas.py:8`).
- `test_e3_degrau_dono_e_porta` conserva sujeito e assertions de partição, carga, C6 e relógio; acrescenta texto ao instante que representa resposta legal (`app/chamados/tests/test_e3_degrau_dono_e_porta.py:262`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:289`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:320`).
- `test_e1_v10` modifica somente `_ln`, dando instante à fixture textual; assertions de mérito/partição/retratação permanecem (`app/chamados/tests/test_e1_v10.py:118`, `app/chamados/tests/test_e1_v10.py:143`, `app/chamados/tests/test_e1_v10.py:169`).
- `test_e4b_c12_orm` troca explicitamente o cenário geral do desligamento para resposta legal num dia aberto adicional (`app/chamados/tests/test_e4b_c12_orm.py:59`, `app/chamados/tests/test_e4b_c12_orm.py:148`). Isso explica conservar chamado aberto/zero Pauta (`app/chamados/tests/test_e4b_c12_orm.py:171`). O cenário ilegal anterior continua em teste separado, com fechamento da disputa/chamado, via `admin_decidiu`, zero Pauta e exportação/fechamento intactos (`app/chamados/tests/test_e4b_c12_orm.py:207`). A alteração reflete L-130 quanto à retenção, porém `test_RED_desligamento` deixa de exercitar emissão de Pauta nesse gatilho (`app/chamados/tests/test_e4b_c12_orm.py:174`). Mordida atual dos mutantes ORM: [não verificado].
- Há enfraquecimento objetivo adicional: `test_o261_resposta_vazia` passa de comparação do estado inteiro para apenas I e T; perde assertions sobre validação/materialização/via e também remove a assertion de disputa aberta sem acrescentar a de disputa fechada (`app/chamados/tests/test_o261_resposta_vazia.py:74`, `app/chamados/tests/test_o261_resposta_vazia.py:113`). Parte da mudança se justifica pelo novo fecho; a cobertura ficou menor.
- O teste sentinela novo prova delegação da property e evita cópia que apenas produza os mesmos valores (`app/chamados/tests/test_f1nova_b2_leitores.py:87`).
- O teste de face SQL negada inclui NBSP e compara conjuntos pk a pk (`app/chamados/tests/test_f1nova_b2_leitores.py:134`); execução ORM: [não verificado].

Validação sem banco executada: `PYTHONPATH=app python3 -B -m unittest chamados.tests.test_e3_catraca_respondedores` → **17 testes, OK, rc0**. O zero dessa catraca não certifica “todo leitor usa o juiz”: uma expressão própria que mencione I e T recebe papel `PAR` e fica fora de `RESPONDEDOR` (`app/chamados/tests/test_e3_catraca_respondedores.py:381`).

## 16/11 23:47 · sessao 0e37165938ec · - · - · origem=Ronald (tmux)

Achado remanescente: há decisão própria de produção fora de C21, embora equivalente no resultado. `espera_o_texto_do_colaborador()` retorna `p.respondida_em is None or respondida_sem_texto(p)`, em vez de delegar a `not pergunta_respondida(p)`. Alimenta `resposta_enviada`, aviso/correção do cartão e `_calados`, que alimenta pauta da supervisão. Deve migrar por unicidade; não é exposição do cru. (`app/chamados/services/fio.py:177`, `app/chamados/services/fio.py:187`, `app/chamados/services/fio.py:359`, `app/chamados/services/fio.py:386`, `app/chamados/services/cobranca.py:227`, `app/chamados/services/cobranca.py:241`.)

Classificação dos demais usos diretos encontrados:

| Sítio | Classificação |
|---|---|
| `app/api/views.py:1165`, `app/api/views.py:2563`; `app/api/views_mensageria.py:569` | Serialização do instante, legítima; não decide respondida. |
| `app/chamados/veredito_lavrado.py:207`; `app/chamados/services/painel.py:67`; `app/chamados/services/verbos.py:344`; `app/chamados/services/validacao.py:1256` | Tempo, Max, idade e ordenação: legítimos. |
| `app/chamados/models.py:4095`, `app/chamados/models.py:4102`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:56`; `app/chamados/services/historico.py:99` | Trilha/projeção de ato ocorrido, legítima; não substituir pela situação atual C21. |
| `app/chamados/models.py:3241`, `app/chamados/models.py:3242`; `app/chamados/models.py:3371`; `app/chamados/models.py:3586` | Porta de escrita, construção/conferência do par: legítimos. |
| `app/chamados/services/disputa_emissao.py:1663` | Preservação do texto existente na resposta automática: legítima proteção de conteúdo, inclusive legado; não transformar em C21 e permitir sobrescrever texto sem I. |
| `app/chamados/services/disputa_emissao.py:1692`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:46` | Cura histórica do meio-par e conferência da lista fixa: legítima pergunta de integridade/trilha. |
| `app/chamados/services/disputa_emissao.py:1861` | Detecta limbo para impedir reabertura e preservar texto; legítima integridade, embora possa chamar `texto_sem_carimbo`. |
| `app/chamados/services/disputa_emissao.py:2079` | Texto para relato do ato e porta que carimba resposta: legítimo contexto de escritor. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1106` | Alarme legado de meio-par: não é decisão respondida. Contudo usa `!= ''`, sem trim; também conta espaço/NBSP sem I como “preenchida”, divergindo da integridade L130. |
| `app/chamados/services/vigia_fio.py:747`, `app/chamados/services/vigia_fio.py:750` | Censo de integridade I sem T, com `texto_vazio` real: legítimo. |
| `app/chamados/models.py:3692`; `app/templates/chamados/partials/_card_atendimento.html:141`, `app/templates/chamados/partials/_card_atendimento.html:182`; seletores em `:114` e `:154` | Renderização/seleção do conteúdo cru: legítimos para esse propósito. As frases “Você respondeu”/“Sua resposta” ainda aparecem por texto truthy, independentemente de I. |
| `app/chamados/services/fio.py:377`; `app/chamados/templatetags/chamados_tags.py:139` | Guarda conservadora de indisponibilidade da testemunha de aplicação, não classificação geral de respondida: legítima. |
| `app/chamados/templatetags/chamados_tags.py:166`; `app/chamados/services/materializacao.py:770` | Atalho por ausência de conteúdo para análise/parsing da hora: legítimo; não afirmar C21 por ele. |
| `app/chamados/services/validacao.py:467`, `:1183`, `:1194`; `app/chamados/services/materializacao.py:625`, `:953`, `:1562` | Interpretação do valor (folga, escape, hora/data), legítima. |
| `app/chamados/services/acoes_disputa.py:357`; `app/ponto/services/dia_do_colab.py:70` | Foto/texto exibido ou conservado: legítimos. |

Os templates que decidem estado por `.respondida` passam pela property delegadora (`app/templates/chamados/partials/modal_fio.html:217`; `app/templates/chamados/partials/_pergunta_card.html:24`; `app/chamados/models.py:3664`). Não encontrei `~q_respondida_com_texto()` executável nos leitores pesquisados; há menção proibitiva na docstring (`app/chamados/juizes.py:1585`). A varredura global dos campos não encontrou usos executáveis adicionais em folha/escala/ferias fora das famílias catalogadas; isso é alcance de busca, não prova dinâmica. [não verificado]

NBSP está contemplado no código: Python usa `.strip()`; SQL constrói a classe completa a partir de `str.isspace()`, em vez de `\s` do banco. As faces positiva e negativa empregam essa mesma classe (`app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1560`, `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1590`). Há teste pk a pk da face negativa com `' \xa0 '` e testes da face positiva/ilegais, mas não executei testes de banco (`app/chamados/tests/test_f1nova_b2_leitores.py:137`; `app/chamados/tests/test_e3_forma_sql_do_juiz.py:89`; `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:34`). Execução SQL nesta cópia: [não verificado].

O selo de respondedores tem ponto cego concreto: conta apenas decisões sobre `respondida_em`, considera `PAR` qualquer expressão que também leia texto, e o teste explicitamente preserva `espera_o_texto_do_colaborador` nessa classe. Logo zero não significa “todos delegam ao juiz”; um clone de I∧T continua verde (`app/chamados/tests/test_e3_catraca_respondedores.py:51`, `:356`, `:381`, `:505`, `:512`). A mudança de `PAROU_EM` para zero não fortaleceu esse contrato (`app/chamados/tests/test_e3_catraca_respondedores.py:54`).

Executei o módulo sem banco e sem bytecode: 17 testes, OK, rc=0. O resultado da execução não tem artefato porque a tarefa proíbe criar arquivos. [não verificado] [nao verificado]

_6244 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
