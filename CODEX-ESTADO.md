# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 23:14** · publicado em **30/10/2026 23:14** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 23:14 · sessao f4fbc1b704dd · - · - · origem=Ronald (tmux)

Achados em `models.py`, todos por leitura; ocorrência em produção [nao verificado].

1. **Pouso antes do pré-requisito — bloqueia.** S2 salva a lavra em `app/chamados/models.py:279`, antes do `atomic` que salva o chamado (`app/chamados/models.py:286`). A escrita efetiva ocorre em `app/chamados/veredito_lavrado.py:231`. `reabrir()` chama esse núcleo sem transação própria (`app/chamados/models.py:1362`). Contraexemplo: chamado fechado não-furo, pergunta não-validada com slot não-derivável; a lavra usa o destino já vivo em memória, depois `save()` do chamado falha. O banco conserva o chamado fechado e a lavra do estado vivo. O juiz usa precisamente esse estado em `app/chamados/juizes.py:699`.

2. **Ordem de trava invertida — bloqueia.** Caminho A: `abrir()` mantém transação (`app/chamados/models.py:843`), encontra encerrado e renasce (`app/chamados/models.py:862`), chama `reabrir()` (`app/chamados/models.py:960`): UPDATE pergunta (`app/chamados/models.py:279`) antes de UPDATE chamado (`app/chamados/models.py:291`). Caminho B: `renovar_contexto()` mantém transação e atualiza chamado (`app/chamados/services/contexto.py:100`), S143 executa mesmo sem alteração de status (`app/chamados/signals.py:81`), e atualiza pergunta (`app/chamados/services/disputa_emissao.py:1583`). Contraexemplo: chamado não-furo fechado, pergunta não-materializada em disputa aberta e célula absolvida; A segura pergunta e espera chamado; B segura chamado e espera pergunta. Um emissor real desse tipo é `app/chamados/services/adesao.py:74`.

3. **Cache enfraquece o tripwire — bloqueia.** O plano guarda lista em `app/chamados/models.py:359`; o núcleo reutiliza sem consultar novamente (`app/chamados/models.py:212`). Contraexemplo: A inicia `cancelar()`, lê zero ancoradas; B reaponta pergunta viva do mesmo colaborador/dia para A ainda vivo e confirma (`app/chamados/services/fio.py:828`); A fecha com a lista vazia. S143 encontra a pergunta, mas preserva a viva (`app/chamados/services/disputa_emissao.py:1547`). Antes do delta, a consulta adicional do tripwire podia detectar esse reapontamento ocorrido antes dele. Nenhuma trava coordena essas duas portas (`app/chamados/models.py:728`; `app/chamados/services/fio.py:814`).

Mapa para consolidação:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `_f1_ancoradas` | `app/chamados/models.py:359`; limpa em `:211`, `:400`, `:647` | `app/chamados/models.py:210`, `:270` |
| `_f1_dedup` | `app/chamados/models.py:641`, `:699` | `app/chamados/models.py:644`, `:650` |
| `status_local` | núcleo `app/chamados/models.py:222`, persistência `:291`; troca temporária `:335`, restauração `:348` | núcleo `:201`, `:213`, `:265`; gates `:394`, `:633`; juiz `app/chamados/juizes.py:700` |
| Âncora, `validada_em`, `data_evento` | destino existente: `app/chamados/services/fio.py:828`; nascimento e dedup: `app/chamados/models.py:2372`, `:2398` | `app/chamados/models.py:323`, `:338`, `:341`; autoridade da âncora `app/chamados/services/fio.py:117` |
| Lavra: `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `app/chamados/veredito_lavrado.py:227` | `app/chamados/veredito_lavrado.py:179`, `:182`, `:189` |
| Pergunta viva | autoridade registrada em `app/core/juizes.py:238` | composição F1 em `app/chamados/models.py:345` |
| Cobrança viva do dia | consulta `app/chamados/juizes.py:1621`; identidade registrada em `app/core/juizes.py:240` | `app/chamados/models.py:367`, `:691` |

**Segundo escritor persistente introduzido pelo trecho de modelos: nenhum.** `chamado_destino = self` é memória (`app/chamados/models.py:278`); a lavra salva somente seus três campos (`app/chamados/veredito_lavrado.py:231`). A troca temporária de status está declarada nos dois selos (`app/chamados/tests/test_contract_status_nucleo.py:9`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:40`).

**Segundo juiz introduzido pelos métodos de modelos: nenhum identificado.** A composição chama C6 (`app/chamados/models.py:345`) e competência entregue pela composição existente (`app/chamados/models.py:341`; `app/chamados/services/vigia_fio.py:469`).

**Callback síncrono tornando o cache velho: nenhum identificado.** O receiver de pergunta agenda o trabalho para `on_commit` (`app/chamados/signals.py:202`); o contraexemplo do achado 3 exige concorrência.

## 30/10 23:14 · sessao 15cd36045653 · - · - · origem=Ronald (tmux)

Achados no cartório:

1. **Fila da segunda passada perde ordem — bloqueia.** Dias já reconciliados entram em `rejulgar` no corpo (`app/ponto/services/cartorio.py:1201`); os adiados entram depois (`app/ponto/services/cartorio.py:758`). Contraexemplo: dia 18 adiado e dia 25 encerrado imediatamente produzem `[25,18]`. O consumidor toma primeiro/último como limites (`app/ponto/services/cartorio.py:846`), gerando início 24/10, fim 21/10 e `n_dias=-5` (`app/ponto/services/cartorio.py:871`, `app/ponto/services/cartorio.py:889`). A grade enumera `range(dias)` (`app/escala/utils.py:1257`), e o cartório pula dia ausente (`app/ponto/services/cartorio.py:1111`). Validei as atribuições extraídas por AST, sem banco nem arquivos. Existência desse lote em produção: [nao verificado].

2. **E2 pode fechar o adiado antes do contador/rejulgamento — fila.** E2 fecha a disputa (`app/chamados/reconciliador.py:635`), cujo signal reconcilia o chamado-pai por outra instância (`app/chamados/signals.py:32`, `app/chamados/signals.py:42`). Se esse pai é o chamado adiado, o posterior `x.reconciliar()` (`app/ponto/services/cartorio.py:755`) recarrega o terminal e devolve `False` (`app/chamados/models.py:632`, `app/chamados/models.py:635`, `app/chamados/models.py:613`). `_fech` fica vazio: não conta nem pede segunda passada (`app/ponto/services/cartorio.py:756`). O signal do cartório também retorna durante julgamento (`app/chamados/signals.py:174`). Resultado concreto: impressão anteriormente gravada com chamado aberto permanece até outro julgamento (`app/ponto/services/cartorio.py:1099`, `app/ponto/services/cartorio.py:1459`).

3. **Cura não alcança impressão antiga intacta — fila.** A regra da lâmpada muda (`app/ponto/services/cartorio.py:1174`), mas a versão continua 2 (`app/ponto/services/cartorio.py:100`). A impressão inclui essa versão (`app/ponto/services/cartorio.py:342`) e o pulo ocorre antes da cura (`app/ponto/services/cartorio.py:1108`). Contraexemplo: célula já lavrada como `cobrado`, todos os marcos acesos, fio vivo e insumos intactos: cron comum pula, não executa adiamento/E2. A própria documentação identifica esse mecanismo de congelamento (`app/ponto/services/cartorio.py:104`). Quantos casos existem: [nao verificado].

Mapa de donos do trecho:

| Dado | Escritor | Leitor/juiz |
|---|---|---|
| `status_local` | núcleo chamado, `app/chamados/models.py:614` | `VIVOS`, registro `app/core/juizes.py:233`; cartório `app/ponto/services/cartorio.py:1085` |
| Perguntas do fio | relação destino `app/chamados/models.py:2596` | `fio_vivo`, cartório `app/ponto/services/cartorio.py:1090`; autoridade `app/core/juizes.py:247` |
| Marcos cobrados, `ata.lampadas` | ata produzida `app/ponto/services/cartorio.py:1142`; persistida pela porta `app/ponto/portas/celula.py:690` | `marcos_acesos`, `app/ponto/services/lastro.py:170` |
| `veredito`, `veredito_via`, `veredito_em`, `protestos`, `ata` | `app/ponto/portas/celula.py:690` | C6 `app/chamados/juizes.py:1054`; E2 `app/chamados/reconciliador.py:594` |
| `impressao`, `julgada_em`, `ausencia`, `tem_entrada/saida`, `insumos_em` | `app/ponto/portas/celula.py:721` | pulo `app/ponto/services/cartorio.py:1108` |
| `materializada_em/por`, `via_resolucao` | E2 `app/chamados/reconciliador.py:617` | C6 `app/chamados/juizes.py:1044`; respostas pendentes `app/chamados/models.py:1713` |
| `fechada_em/por`, `resolucao`, `motivo_fechamento` | porta disputa `app/chamados/models.py:1791` | signal pai `app/chamados/signals.py:28` |

No trecho do cartório: **segundo juiz duplicado: nenhum; segundo escritor fora da porta: nenhum; relógio de decisão novo fora da borda: nenhum**. O filtro novo chama o juiz existente `marcos_acesos` (`app/ponto/services/cartorio.py:1174`, `app/ponto/services/lastro.py:153`). E2 usa `now()` como carimbo (`app/chamados/reconciliador.py:617`); seu caminho C6 retorna antes do ramo de janela temporal porque exige célula que não acusa e sem marcos pendentes (`app/chamados/reconciliador.py:594`, `app/chamados/reconciliador.py:607`, `app/chamados/juizes.py:1064`). Não inflaria isso como defeito novo de relógio.

## 30/10 23:14 · sessao 5df2ea80c0a6 · - · - · origem=Ronald (tmux)

Achado bloqueia: `app/chamados/services/limbo_folgas.py:228` reata perguntas antes e fora da transação do fechamento; `reapontar_resolvedora` persiste imediatamente em `app/chamados/services/fio.py:828`, enquanto o `atomic` de `reconciliar` só começa em `app/chamados/models.py:610`. Contraexemplo: p1 reata; o save de p2 ou do fechamento lança erro; p1 permanece no LIMBO, o furo permanece vivo e a próxima tentativa retorna antes de reparar porque `agrupado_em` já foi gravado (`app/chamados/services/limbo_folgas.py:217`, `app/chamados/services/contexto.py:99`). Classe: escrita antes do pré-requisito/atomicidade da reata com fechamento; a própria porta exige esse par no mesmo ato (`app/chamados/models.py:390`). Ocorrência em produção: [nao verificado].

Achado fila: o selo aceita guarda comprovadamente inoperante. Extraí e executei em memória a função de `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24`; tanto a condição original quanto mutantes `and []` e `if novo == "__estado_inexistente__"` retornaram `True`. Ele só rejeita constantes `False/None/0` (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:37`) e depois verifica presença sintática da chamada e do raise (`app/chamados/tests/test_o261e_f1_selo_encerramento.py:39`). Isso é insuficiência específica do selo; os testes comportamentais ainda podem detectar esses mutantes (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`).

O espião do evento observa depois de `_gravar_estado` voltar, portanto não demonstra literalmente “em nenhuma escrita” (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:378`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:383`). Porém S143 atual só materializa perguntas que C6 já considera mortas, logo não comprova mascaramento da violação nessa fixture (`app/chamados/services/disputa_emissao.py:1547`). Não classificaria o RED inteiro como sem mordida: ele exige fechamento final, e a base anterior mantém o ciclo que deixa o chamado aberto (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:390`); execução contra a base: [nao verificado].

Mapa dos campos destes serviços:

| Campo/valor | Quem escreve | Quem lê |
|---|---|---|
| `contexto_json.agrupado_em`, `defeito_cadastro` | Solicitação em `app/chamados/services/limbo_folgas.py:219`; persistência em `app/chamados/services/contexto.py:99` | Guarda de repetição em `app/chamados/services/limbo_folgas.py:217`; pai vivo em `app/chamados/models.py:758`; via em `app/chamados/juizes.py:1166` |
| `chamado_destino` | Porta chamada em `app/chamados/services/limbo_folgas.py:229`; atribuição/save em `app/chamados/services/fio.py:828` | Âncora em `app/chamados/services/fio.py:117`; legitimidade pelo juiz em `app/chamados/juizes.py:1687` |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` da reata | Lavra/save da mesma porta em `app/chamados/services/fio.py:829` | C6 consulta seu resultado por `app/chamados/juizes.py:791` [leitores adicionais não recenseados nesta subanálise] |
| `perg.disputa.colaborador`, `perg.data_evento`, `autor` | O delta recebe esses valores em `app/chamados/services/validacao.py:249` | Novo fechamento do fio em `app/chamados/services/validacao.py:271`; seleção por colaborador/data em `app/ponto/services/veredito_celula.py:174` |
| `validada_em`, `validada_por` | Porta de validação em `app/chamados/services/validacao.py:131` | C6 em `app/chamados/juizes.py:1006` |
| `materializada_em`, `materializada_por`, `via_resolucao` | `fechar_por_admin` em `app/chamados/models.py:2444`, chamada pelo fio em `app/ponto/services/veredito_celula.py:177` | Idempotência em `app/chamados/models.py:2435`; C6 em `app/chamados/juizes.py:1039` |
| `status_local` | Fio chama a porta `reconciliar` em `app/ponto/services/veredito_celula.py:183`; núcleo em `app/chamados/models.py:614` | Universo vivo em `app/ponto/services/veredito_celula.py:148` |
| Nota de fechamento | Só após reconciliar retornar verdadeiro em `app/ponto/services/veredito_celula.py:183` | Teste existente exige uma nota para quatro aceites em `app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:185` |

Nos dois serviços examinados, nenhum segundo escritor direto novo: o delta chama as portas existentes de contexto, reata e fechamento (`app/chamados/services/limbo_folgas.py:219`, `app/chamados/services/limbo_folgas.py:229`, `app/chamados/services/validacao.py:271`). O teste existente das quatro irmãs já cobra chamado resolvido e uma nota, dando testemunha ao novo fechamento repetido da folga (`app/chamados/tests/test_o199_aceite_de_folga_decide_o_dia.py:167`).

Demais classes neste recorte: segundo juiz duplicado — nenhum; relógio novo fora da borda — nenhum; derivação paralela — nenhum; ordem de trava — nenhum. As novas operações relevantes chamam autoridades existentes e não acrescentam aquisição explícita de trava (`app/chamados/services/limbo_folgas.py:227`, `app/chamados/services/validacao.py:270`).

## 30/10 23:07 · sessao 86789d0f7e2d · DISJUNTOR · verificador · origem=Code

**O encaixe direto quebra principalmente na liberação de CLEAR, nas guardas de trabalho em curso e na identidade da sessão.** A máquina atual ainda usa os gatilhos e a janela antigos. (`bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:866`, `bin/orcamento_contexto.py:44`)

Os cenários abaixo são contraprovas por leitura do código; ocorrência efetiva em produção: **[não verificado]**.

1. **C1 dispara pela medida errada.** Hoje dispara por **60% da janela OU 6 horas OU três deploys**, não por 150.000 tokens. Cenário: marco recente, nenhum deploy, último assistente principal com 150.000 tokens e janela de 1 milhão → não dispara; Haiku com janela de 200.000 e 120.000 tokens → dispara antes do contratado. (`bin/orcamento_contexto.py:36`, `bin/orcamento_contexto.py:41`, `bin/orcamento_contexto.py:209`, `bin/orcamento_contexto.py:212`)

2. **Libera CLEAR sem handoff novo — ou sem handoff algum.** Stop S1 grava `disparo`; antes de S2 escreve-se somente `PAREI: ! CLEAR` no topo. S2 libera verificando o disparo e o prazo, sem consultar `HANDOFF-SESSAO.md`. Portanto, apenas trocar a mensagem por “SO bin/handoff_sessao.sh” não cria a condição de liberação. (`bin/hook_stop_fila1.py:878`, `bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:835`)

3. **Continua liberando depois dos dez minutos.** Com disparo ativo, S2 vê CLEAR em t0; S3 em t0+11 minutos ainda libera. O prazo atual é 30 minutos e começa no primeiro Stop que observa o PAREI. (`bin/orcamento_contexto.py:47`, `bin/hook_stop_fila1.py:777`, `bin/hook_stop_fila1.py:829`, `bin/hook_stop_fila1.py:836`)

4. **As guardas ficam depois da liberação de um disparo ativo.** Cenário: S1 dispara sem trabalho em curso; começa uma suite/push; S2 encontra CLEAR e libera antes de consultar trabalho em curso. A avaliação de ESPERANDO/suite/push protege o primeiro disparo, mas não essa liberação posterior. Acrescentar DIFF somente junto dessas guardas repetiria o defeito. (`bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:866`)

5. **O sinal novo não cobre automaticamente o DIFF existente.** `diff_janela_he_total.sh` toma `logs/sombra.lock`; seu caminho mostrado não publica `logs/sessao/diff_apply/<obra>`. Cenário: esse DIFF roda, o sinal novo está ausente e chega Stop com gatilho alto → a guarda baseada somente no sinal novo deixa passar o disparo no meio do DIFF. O handoff já consulta a trava efetivamente usada. (`bin/diff_janela_he_total.sh:17`, `bin/diff_janela_he_total.sh:38`, `bin/sombra_trava.sh:16`, `bin/sombra_trava.sh:26`, `bin/handoff_sessao.sh:223`) Escritor e limpeza do novo sinal: **[não verificado]**.

6. **Agente sem entrega não deixa C1 PENDENTE hoje.** Cenário: existe `subagents/agent-X.jsonl` sem entrega, topo sem ESPERANDO e nenhuma suite/push; o avaliador alcança `DISPARO`, porque os adiamentos atuais são ESPERANDO provado, PAREI comum e suite/push. Não há leitura desses JSONL nessa decisão. (`bin/hook_stop_fila1.py:866`, `bin/hook_stop_fila1.py:878`) Formato de “turno final”, escritor desses JSONL e sobrevivência ao `/clear`: **[não verificado]**.

7. **CLEAR de outra sessão pode atender o disparo errado.** Cenário condicional: A dispara; B usa a mesma RAIZ e grava SessionStart `clear`; próximo Stop de A aceita o marco de B, registra ATENDIDO e elimina o disparo de A. O Stop extrai somente `transcript_path`; embora o marco guarde `session_id`, não há comparação com a identidade do Stop. (`bin/hook_stop_fila1.py:1018`, `bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:45`, `bin/hook_stop_fila1.py:802`, `bin/hook_stop_fila1.py:810`, `bin/hook_stop_fila1.py:821`)

8. **Compact encerra e rearma sem `/clear`.** S1 dispara; ocorre SessionStart `compact`; S2 encontra marco novo, registra RESET e apaga o disparo anterior. Assim, conservar a máquina atual permite escapar da cobrança de CLEAR por compact. O painel também passa a mostrar tempo desde compact sob o rótulo “desde o clear”. (`bin/marco_de_clear.py:32`, `bin/marco_de_clear.py:44`, `bin/hook_stop_fila1.py:819`, `bin/hook_stop_fila1.py:821`, `bin/orcamento_contexto.py:245`, `bin/orcamento_contexto.py:265`)

9. **Estado ilegível permite contar novamente o mesmo ciclo.** S1 grava DISPARO; `orcamento_estado.json` fica truncado antes de S2; a medida continua alta. S2 transforma a leitura inválida em `{}` e grava outro DISPARO. O flock serializa acessos, mas não recupera a identidade perdida do disparo. (`bin/hook_stop_fila1.py:779`, `bin/hook_stop_fila1.py:783`, `bin/hook_stop_fila1.py:878`, `bin/hook_stop_fila1.py:880`)

10. **“Caminho de hoje com TETO” não garante saída.** Se o checador do RELATO retorna erro ou excede o timeout, cada Stop bloqueia **antes** de C1 e do contador do TETO. Repetir essa condição prende o encerramento independentemente do tratamento de exceção do disjuntor. (`bin/hook_stop_fila1.py:1007`, `bin/hook_stop_fila1.py:1009`, `bin/hook_stop_fila1.py:1029`, `bin/hook_stop_fila1.py:1034`) Separadamente, SessionStart pode esperar indefinidamente pelo flock do marco: sua aquisição é bloqueante, sem prazo no código. (`bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:40`)

11. **“Trabalho em curso” pode ser de outra obra, ou ficar invisível.** O detector varre `ps` global por texto, sem identidade de obra/sessão. Uma suite alheia mantém C1 PENDENTE e impede a saída pelo TETO; falha de `ps` vira `False`, podendo autorizar o disparo durante trabalho real não observado. (`bin/hook_stop_fila1.py:386`, `bin/hook_stop_fila1.py:392`, `bin/hook_stop_fila1.py:395`, `bin/hook_stop_fila1.py:388`, `bin/hook_stop_fila1.py:869`, `bin/hook_stop_fila1.py:1104`)

12. **O painel atual não entrega as medidas prometidas.**
    - Soma ATENDIDO/SEGUIU da trilha inteira, sem janela de 24 horas; não conta DISPARO. Um evento antigo continua no total. (`bin/orcamento_contexto.py:235`, `bin/orcamento_contexto.py:239`)
    - Publica percentual/modelo/janela, sem os tokens e o horário da última observação. Stop barrado pela prova, orçamento desligado ou lock ocupado conserva uma medida anterior, embora o rótulo proposto diga “último Stop”. (`bin/orcamento_contexto.py:255`, `bin/hook_stop_fila1.py:823`, `bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:753`, `bin/hook_stop_fila1.py:765`)
    - Subtrair CLEAR-LIBERA de ATENDIDO mede intervalo entre observações por Stops. Exemplo: primeiro Stop vê PAREI às 12:05, `/clear` às 12:08, próximo Stop às 12:20 → diferença dos eventos dá 15 minutos, não três. ATENDIDO não registra a hora exata do clear; ela está no marco. (`bin/hook_stop_fila1.py:831`, `bin/hook_stop_fila1.py:817`, `bin/hook_stop_fila1.py:694`, `bin/marco_de_clear.py:56`)
    - O formatador atual não calcula cache diário nem média de espera; `gerar_estado.py` chama esse formatador. O valor de referência “390M” e sua deduplicação não são demonstrados pelo dado fornecido. (`bin/orcamento_contexto.py:265`, `bin/gerar_estado.py:274`) **[não verificado]**

13. **B3 exige uma convenção que o verificador atual não exige.** A instrução pede “id da obra”, sem obrigar `OBRA=<id>`. Cenário permitido: prompt “verifique a obra O261…” e resposta cuja primeira linha é `REPROVADO`; o reconhecimento contratado, condicionado a `OBRA=O261`, perde essa reprovação. (`.claude/agents/verificador.md:3`, `.claude/agents/verificador.md:18`) Payload efetivo de SubagentStop, recuperação do prompt e deduplicação por agente/entrega: **[não verificado]**.

14. **B1/B2/B4 ainda não têm comportamento auditável nesse material.** Não localizei `bin/disjuntor.py`, `bin/statusline_cota.py`, `bin/tests/test_disjuntor.sh` ou `.claude/settings.json`; implementação, instalação e desligamento por `DISJUNTOR=0`: **[não verificado]**. O contrato fornecido não define medida/limiar de B1, regra de B2 ou limiar de B4; sem isso não há como construir um caso numérico de “deveria negar”. **[não verificado]** A redação de frescor de B4 também precisa precisão: comparar literalmente mtime com um `resets_at` futuro classificaria como velha uma amostra recém-gravada antes do reset; payload e comparação efetivos: **[não verificado]**.

Quanto aos donos dos arquivos, a distinção entre código existente e intenção do contrato é esta:

| Arquivo | Quem ESCREVE | Quem LÊ |
|---|---|---|
| `logs/sessao/orcamento_estado.json` | Stop, `_orc_salva`, substituição atômica. (`bin/hook_stop_fila1.py:717`) | Stop e leitor do painel. (`bin/hook_stop_fila1.py:779`, `bin/orcamento_contexto.py:228`) |
| `logs/sessao/orcamento.log` | Stop, `_orc_trilha`. (`bin/hook_stop_fila1.py:693`) | `orcamento_contexto.ler`, chamado por `gerar_estado.py`. (`bin/orcamento_contexto.py:235`, `bin/gerar_estado.py:276`) |
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py`; também lê o anterior para conservar clears. (`bin/marco_de_clear.py:50`, `bin/marco_de_clear.py:61`) | `orcamento_contexto.ler_marco`, usado pelo Stop e painel. (`bin/orcamento_contexto.py:145`, `bin/hook_stop_fila1.py:794`, `bin/orcamento_contexto.py:232`) |
| `app/docs/HANDOFF-SESSAO.md` | `handoff_sessao.sh`. (`bin/handoff_sessao.sh:296`, `bin/handoff_sessao.sh:300`) | O selo exige `cat` em SessionStart clear/compact; configuração instalada **[não verificado]**. (`bin/tests/test_handoff_sessao.sh:69`, `bin/tests/test_handoff_sessao.sh:72`) |
| `logs/sessao/diff_apply/<obra>` | Não identificado no código inspecionado. **[não verificado]** | Guarda pretendida dos disjuntores. Implementação **[não verificado]**. |
| `subagents/agent-*.jsonl` | Escritor efetivo **[não verificado]**. | Leitor pretendido de entrega em C1; implementação **[não verificado]**. |
| `logs/sessao/cota.json` | Contrato atribui à statusLine; implementação **[não verificado]**. | Contrato atribui a B4; implementação **[não verificado]**. |
| `logs/sessao/disjuntor_estado.json` | Contrato atribui ao disjuntor com flock; implementação **[não verificado]**. | Reavaliação/contadores do disjuntor; implementação **[não verificado]**. |
| `logs/sessao/disjuntor.log` | Contrato atribui ao disjuntor; implementação **[não verificado]**. | Leitura nova por `gerar_estado.py` **[não verificado]**; a integração atual chama o leitor do ORÇAMENTO. (`bin/gerar_estado.py:274`) |

Há ainda uma exceção que precisa permanecer explícita: após disparar, sem topo CLEAR, a máquina volta ao caminho normal e pode liberar pelo TETO no sexto bloqueio contado, sem handoff. Isso corresponde à válvula existente, mas impede interpretar “PAREI só libera com handoff” como garantia de **toda** liberação do Stop. (`bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:1097`, `bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1113`)

## 30/10 23:06 · sessao c03f8040050b · - · - · origem=Ronald (tmux)

Achados para consolidar, distinguindo comportamento atual de integração ainda não verificável:

1. **O sinal de DIFF proposto não acompanha ao menos um DIFF atual.** `diff_janela_he_total.sh` adquire a trava da sombra (`bin/diff_janela_he_total.sh:17-18`); o escritor mantém `logs/sombra.lock` por descritor/flock (`bin/sombra_trava.sh:16`, `bin/sombra_trava.sh:26-38`). O handoff lê essa trava (`bin/handoff_sessao.sh:223-233`). Cenário: esse DIFF está rodando, não existe `logs/sessao/diff_apply/<obra>`, chega Stop acima do limiar; uma implementação que só consulta o sinal novo considera ausente a proteção enquanto o DIFF existente continua. Escritor/leitor atuais do novo sinal: **[não verificado]**.

2. **O estado de CLEAR não é isolado por sessão.** Stop extrai apenas `transcript_path` do payload (`bin/hook_stop_fila1.py:1016-1023`); estado e trilha são únicos por RAIZ (`bin/hook_stop_fila1.py:675-676`). `marco_de_clear.py` grava o `session_id`, mas no mesmo arquivo global (`bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:44-61`). Cenário: sessão A dispara; sessão B dá `/clear`; próximo Stop A lê outro `marco_ref` e pode registrar ATENDIDO pelo clear B; compact B produz RESET e rearmamento (`bin/hook_stop_fila1.py:800-821`). Ocorrência real em produção: **[não verificado]**.

3. **O reconhecimento B3 proposto exige um identificador que a instrução atual do verificador não obriga.** A descrição pede “id da obra”, sem exigir a sintaxe `OBRA=<id>` (`.claude/agents/verificador.md:3`); a saída exige primeira linha APROVADO/REPROVADO (`.claude/agents/verificador.md:18`). Cenário permitido pela instrução atual: prompt “verifique a obra O261…” e resposta `REPROVADO`; um contador condicionado a `OBRA=O261` não associa essa reprovação. Payload efetivo de SubagentStop e implementação do contador: **[não verificado]**.

4. **Não há prova da instalação dos novos hooks nesta cópia.** `.claude/settings.json` não foi encontrado; a configuração é local e ignorada pelo Git (`.gitignore:66`; `bin/tests/test_handoff_sessao.sh:15-19`). Esse selo verifica Stop, PreCompact e SessionStart clear/compact (`bin/tests/test_handoff_sessao.sh:49-79`), sem validar PreToolUse, SubagentStop ou statusLine nesse trecho. Instalação real do contrato: **[não verificado]**.

5. **B1/B2/B4 e a contagem B3 não podem receber veredicto de implementação atual.** `bin/disjuntor.py`, `bin/statusline_cota.py` e `bin/tests/test_disjuntor.sh` não foram encontrados na inspeção: **[não verificado]**. O contrato fornecido também não apresenta a medida/limiar de B1, definição operacional de B2 ou limiar de B4: **[não verificado]**. Não atribuir ao harness campos, reentregas ou garantias que o dado não mostrou.

Escritores/leitores dos estados novos:

| Arquivo citado | Escritor/leitor verificável |
|---|---|
| `logs/sessao/cota.json` | Statusline escritor; disjuntor leitor somente no contrato; implementação **[não verificado]**. |
| `logs/sessao/disjuntor_estado.json` | Disjuntor com flock somente no contrato; implementação **[não verificado]**. |
| `logs/sessao/disjuntor.log` | Disjuntor escritor e `gerar_estado.py` leitor somente no contrato; implementação **[não verificado]**. |
| `logs/sessao/diff_apply/<obra>` | Contrato não nomeia escritor; leitor/implementação **[não verificado]**. |
| `subagents/agent-*.jsonl` | Harness escritor e consumidor de “turno final” **[não verificado]**; o dado não inclui exemplar nem definição de entrega. |

Complementos verificáveis: `logs/sessao/ultimo_clear.json` é lido e substituído pelo escritor `marco_de_clear.py` (`bin/marco_de_clear.py:50-61`); `orcamento_estado.json` é escrito pelo Stop (`bin/hook_stop_fila1.py:710-726`); `orcamento.log` recebe os eventos após a gravação do estado (`bin/hook_stop_fila1.py:724-726`).

## 30/10 23:05 · sessao 8e779091d3c1 · - · - · origem=Ronald (tmux)

Achados de C1 e Stop, por inspeção estática:

1. **O gatilho atual mede outra regra.** São 60% da janela **ou** 6 h **ou** três deploys, com expiração de 30 min: `bin/orcamento_contexto.py:44`, `bin/orcamento_contexto.py:209`. Com janela de 1 milhão e 150 mil tokens, idade abaixo de 6 h e menos de três deploys, C1 não dispara; com Haiku de 200 mil e 120 mil tokens, dispara abaixo dos 150 mil contratados: `bin/orcamento_contexto.py:36`, `bin/orcamento_contexto.py:41`, `bin/orcamento_contexto.py:110`.

2. **CLEAR libera sem HANDOFF e por 30 min.** Stop S1 grava `disparo`; S2 encontra `PAREI: ! CLEAR` e libera exclusivamente pela existência do disparo, sem verificar HANDOFF nem a idade dele; S3 aos 11 min ainda libera: `bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:835`, `bin/hook_stop_fila1.py:836`, `bin/orcamento_contexto.py:47`. O HANDOFF é escrito em `app/docs/HANDOFF-SESSAO.md` por `bin/handoff_sessao.sh:18`; essa exigência não aparece na decisão de liberação: `bin/hook_stop_fila1.py:827`.

3. **As guardas só protegem o primeiro disparo.** ESPERANDO/suite/push são consultados depois do tratamento de `disparo` ativo e CLEAR: `bin/hook_stop_fila1.py:827`, `bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:866`. Cenário: S1 dispara sem suite; inicia-se suite; S2 com CLEAR libera antes de consultar a suite: `bin/hook_stop_fila1.py:835`. DIFF/apply e entrega de subagents não são consultados na decisão de disparo, cujos únicos adiamentos são ESPERANDO, PAREI comum e suite/push: `bin/hook_stop_fila1.py:866`.

4. **TETO pode liberar o ciclo sem HANDOFF ou CLEAR.** Após S1 disparar, Stops seguintes sem CLEAR caem no caminho normal com lembrete: `bin/hook_stop_fila1.py:850`, `bin/hook_stop_fila1.py:1116`. Mantendo HEAD e sem suite, o contador cresce e libera quando `n > 5`, isto é, no sexto Stop contado; não verifica HANDOFF nessa saída: `bin/hook_stop_fila1.py:1097`, `bin/hook_stop_fila1.py:1104`, `bin/hook_stop_fila1.py:1113`.

5. **Há bloqueio sem TETO anterior ao instrumento.** Se `relato_afirma_com_prova` falha, excede timeout ou retorna não zero, o Stop bloqueia antes de avaliar C1 e antes de incrementar o TETO: `bin/hook_stop_fila1.py:1007`, `bin/hook_stop_fila1.py:1009`, `bin/hook_stop_fila1.py:1029`. Repetir esse estado repete bloqueio indefinidamente; portanto “caminho de hoje com TETO” não garante saída dessa sessão: `bin/hook_stop_fila1.py:1034`.

6. **Processos de outra obra podem manter C1 pendente e desativar TETO.** A detecção examina todos os argumentos de `ps`, sem escopo de obra/sessão, por substring `manage.py test` ou `git push`: `bin/hook_stop_fila1.py:386`, `bin/hook_stop_fila1.py:392`, `bin/hook_stop_fila1.py:395`. Um desses processos alheios causa PENDENTE e impede liberação por TETO enquanto persistir: `bin/hook_stop_fila1.py:869`, `bin/hook_stop_fila1.py:1092`, `bin/hook_stop_fila1.py:1104`. Se `ps` falha, retorna False e pode disparar durante trabalho real não observado: `bin/hook_stop_fila1.py:387`.

7. **Um CLEAR de outra sessão atende e rearma a sessão errada.** O payload Stop usa somente `transcript_path`: `bin/hook_stop_fila1.py:1018`. O marco é global e recebe o `session_id` do escritor: `bin/marco_de_clear.py:20`, `bin/marco_de_clear.py:45`. Cenário: A dispara; B dá `/clear`; o próximo Stop de A encontra nova referência, registra ATENDIDO pelo clear de B e apaga o disparo de A, sem comparar identidades: `bin/hook_stop_fila1.py:802`, `bin/hook_stop_fila1.py:810`, `bin/hook_stop_fila1.py:817`, `bin/hook_stop_fila1.py:821`. Se A continua acima do limiar, ainda pode disparar novamente nessa avaliação: `bin/hook_stop_fila1.py:822`, `bin/hook_stop_fila1.py:878`.

8. **Estado ilegível permite redisparo/contagem duplicada.** `orcamento_estado.json` truncado ou JSON inválido vira `{}`: `bin/hook_stop_fila1.py:779`, `bin/hook_stop_fila1.py:783`. Se S1 já registrou DISPARO, o estado corrompe antes de S2 e a medida continua alta, S2 registra outro DISPARO no mesmo ciclo; flock não recupera a memória perdida: `bin/hook_stop_fila1.py:878`, `bin/hook_stop_fila1.py:880`.

9. **“Contexto no último Stop” pode ser medida antiga.** `ultima_medida` só atualiza dentro da avaliação do orçamento: `bin/hook_stop_fila1.py:822`. Stop bloqueado por prova, orçamento desligado ou lock ocupado não atualiza a medida: `bin/hook_stop_fila1.py:1034`, `bin/hook_stop_fila1.py:753`, `bin/hook_stop_fila1.py:765`. O painel reutiliza essa medida e não mostra seu horário: `bin/orcamento_contexto.py:243`, `bin/orcamento_contexto.py:253`.

10. **Os contadores publicados não são disparos em 24 h nem espera média.** O leitor soma todas as linhas ATENDIDO/SEGUIU da trilha sem corte temporal e publica os totais; não calcula duração: `bin/orcamento_contexto.py:235`, `bin/orcamento_contexto.py:239`, `bin/orcamento_contexto.py:265`. Além disso, “desde o clear” pode usar compact, porque a base é qualquer marco e o rótulo permanece clear: `bin/orcamento_contexto.py:199`, `bin/orcamento_contexto.py:265`.

Escritores/leitores atuais pertinentes:

- `logs/sessao/orcamento_estado.json`: escreve o hook por substituição atômica e lê o próprio hook; `orcamento_contexto.ler` lê para publicação: `bin/hook_stop_fila1.py:717`, `bin/hook_stop_fila1.py:779`, `bin/orcamento_contexto.py:228`.
- `logs/sessao/orcamento.log`: escreve o hook; `orcamento_contexto.ler` lê; `gerar_estado.py` chama esse leitor: `bin/hook_stop_fila1.py:693`, `bin/orcamento_contexto.py:235`, `bin/gerar_estado.py:274`.
- `logs/sessao/ultimo_clear.json`: escreve e lê o marco para conservar os últimos clears; lê o avaliador, chamado pelo hook e pelo painel: `bin/marco_de_clear.py:50`, `bin/marco_de_clear.py:61`, `bin/orcamento_contexto.py:145`, `bin/hook_stop_fila1.py:794`.
- Locks do orçamento e do marco: adquiridos pelos respectivos escritores; o orçamento usa aquisição não bloqueante, o marco usa espera bloqueante: `bin/hook_stop_fila1.py:755`, `bin/hook_stop_fila1.py:759`, `bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:40`.

## 30/10 23:05 · sessao c9d7251a31dd · - · - · origem=Ronald (tmux)

Achados de medição e estado, por cenário hipotético derivado do código:

1. **C1 não tem o gatilho contratado de 150.000 tokens.** Continua com 60% da janela, 6 horas ou 3 marcos (`bin/orcamento_contexto.py:44`, `bin/orcamento_contexto.py:209`). Assim, último assistente principal de 150.000 tokens no modelo de janela 1M, marco recente e nenhum deploy não dispara; 120.000 no Haiku 200k dispara antes de 150.000 (`bin/orcamento_contexto.py:36`, `bin/orcamento_contexto.py:41`). Modelo desconhecido com 200.000 tokens retorna tokens mais “não sei”, mas não percentual: não dispara por contexto (`bin/orcamento_contexto.py:115`, `bin/orcamento_contexto.py:209`).

2. **Janela é de 30 minutos; não 10.** Disparo ativo + `PAREI: ! CLEAR` visto em t0: Stop em t0+11 min ainda libera; o código usa `EXPIRA_CLEAR_MIN=30` e comparação `<= janela` (`bin/orcamento_contexto.py:47`, `bin/hook_stop_fila1.py:777`, `bin/hook_stop_fila1.py:836`). O início também é o primeiro Stop que lê PAREI, e não o disparo nem a escrita da linha (`bin/hook_stop_fila1.py:829`).

3. **O painel não mede os rótulos prometidos.** Ele conta ATENDIDO/SEGUIU da trilha inteira, sem corte de 24h e sem contar DISPARO; apresenta percentual/modelo/janela, sem os tokens da última medida; não calcula média de espera nem cache diário (`bin/orcamento_contexto.py:233`, `bin/orcamento_contexto.py:255`, `bin/orcamento_contexto.py:265`). `gerar_estado.py` usa precisamente esse leitor/formatador (`bin/gerar_estado.py:274`). Cenário: ATENDIDO de ontem segue no contador de hoje; DISPARO atual sem desfecho não aparece como contagem de disparos (`bin/orcamento_contexto.py:239`).

4. **`orcamento.log` não contém os dois horários exatos necessários à média PAREI→/clear.** CLEAR-LIBERA é carimbado pelo Stop que viu PAREI; ATENDIDO só é apendado no Stop que percebe o novo marco, com detalhe sem o horário do clear (`bin/hook_stop_fila1.py:831`, `bin/hook_stop_fila1.py:817`). Cenário: PAREI escrito 12:00, primeiro Stop 12:05, /clear 12:08, próximo Stop 12:20; subtrair os eventos daria 15 minutos, enquanto escrita→clear foi 8. O horário real está em `ultimo_clear.json`, não na linha ATENDIDO (`bin/marco_de_clear.py:44`, `bin/marco_de_clear.py:56`).

5. **“Desde o clear” também significa desde compact.** O escritor grava tanto `clear` como `compact`; o leitor toma sempre `marco['hora']` como base e a frase imprime “desde o clear” (`bin/marco_de_clear.py:32`, `bin/marco_de_clear.py:44`, `bin/orcamento_contexto.py:245`, `bin/orcamento_contexto.py:265`). Cenário: seis horas sem /clear, compact agora; painel apresenta quase zero horas “desde o clear” (`bin/orcamento_contexto.py:247`).

6. **Estado global permite que um clear de outra sessão atenda o disparo.** Marco e estado são únicos por RAIZ (`bin/marco_de_clear.py:20`, `bin/hook_stop_fila1.py:675`); session_id participa da referência, mas o hook não exige igualdade com a sessão do Stop e aceita qualquer clear posterior ao disparo (`bin/orcamento_contexto.py:149`, `bin/hook_stop_fila1.py:802`, `bin/hook_stop_fila1.py:810`). Cenário condicional: A dispara; B grava SessionStart clear na mesma RAIZ; próximo Stop de A registra ATENDIDO e rearma A (`bin/hook_stop_fila1.py:817`, `bin/hook_stop_fila1.py:821`). Existência de sessões concorrentes na produção: [não verificado].

7. **Retenção limitada de clears pode mudar o desfecho.** Escritor conserva só os últimos 20 horários (`bin/marco_de_clear.py:57`). Se um clear em janela é seguido por 20 clears tardios antes do próximo Stop, o primeiro desaparece; o hook passa a escolher um clear tardio e registra SEGUIU (`bin/hook_stop_fila1.py:810`, `bin/hook_stop_fila1.py:813`). Quantidade real de clears: [não verificado].

**Donos dos estados existentes:**

| Arquivo | Escreve | Lê |
|---|---|---|
| `logs/sessao/ultimo_clear.json` | `marco_de_clear.py`, leitura+gravação sob seu flock, substituição atômica (`bin/marco_de_clear.py:38`, `bin/marco_de_clear.py:61`) | `orcamento_contexto.ler_marco`, chamado pelo hook e painel (`bin/orcamento_contexto.py:145`, `bin/hook_stop_fila1.py:794`, `bin/orcamento_contexto.py:232`) |
| `logs/sessao/orcamento_estado.json` | hook, `_orc_salva` por substituição atômica (`bin/hook_stop_fila1.py:708`, `bin/hook_stop_fila1.py:717`) | hook e `orcamento_contexto.ler`, usado pelo painel (`bin/hook_stop_fila1.py:779`, `bin/orcamento_contexto.py:228`, `bin/gerar_estado.py:276`) |
| `logs/sessao/orcamento.log` | hook, `_orc_trilha` (`bin/hook_stop_fila1.py:687`) | `orcamento_contexto.ler`, usado pelo painel (`bin/orcamento_contexto.py:235`, `bin/gerar_estado.py:276`) |
| `app/docs/HANDOFF-SESSAO.md` | `handoff_sessao.sh`: corte direto ou `mv` do temporário (`bin/handoff_sessao.sh:296`, `bin/handoff_sessao.sh:300`) | contrato de configuração esperado pelo teste é `cat` em SessionStart clear/compact (`bin/tests/test_handoff_sessao.sh:69`, `bin/tests/test_handoff_sessao.sh:72`); configuração efetivamente instalada: [não verificado] |

`cota.json`, `disjuntor_estado.json`, `disjuntor.log`, `diff_apply/<obra>`, `statusline_cota.py`, `disjuntor.py` e `.claude/settings.json` não foram encontrados nesta cópia por busca de arquivos/referências; donos operantes e acionamento real: [não verificado]. O código atual do painel só conhece a trilha/estado do ORÇAMENTO nessa seção (`bin/gerar_estado.py:271`).

## 30/10 22:57 · sessao abbc739c1d90 · FIO-ESTADO · verificador · origem=Code

**O desenho deixa três lacunas centrais: o esperado novo não está demonstrado, o leitor atual ignora `alcance` e não existe `cor` na sua saída.** (`app/chamados/management/commands/vigiar_fio_morto.py:91`; `bin/fio_estado.py:69–92`.) O valor **388** aparece como requisito de teste no contrato; uma medição atual de produção permanece **[não verificado]**.

1. **Quem escreve e quem lê cada campo**

   | Campo do carimbo | Escritor | Leitor atual / leitura pretendida |
   |---|---|---|
   | `alcance.alcance_universo`, `alcance.alcance_cego` | No anexo, `censo_alcance` produz; `vigiar_fio_morto` grava `alcance=alc`. (`anexo_f5.txt:200–225`; implementação integral **[não verificado]**.) | O leitor atual ignora ambos; usa `censo.cego`, que pertence ao universo antigo. (`bin/fio_estado.py:69–92`.) |
   | `alcance.fio_morto_alcance`, `alcance.alcance_a`…`alcance_e` | Mesmo produtor e escritor do anexo. (`anexo_f5.txt:200–225`; classificação completa **[não verificado]**.) | Hoje não são lidos; o contrato pretende usá-los como manchete e partição **[não verificado: leitor proposto não fornecido]**. (`bin/fio_estado.py:86–92`, `:101–104`.) |
   | `alcance.alcance_porta_fila`, `alcance_porta_pergunta`, `alcance_de_hoje_pra_frente` e os quatro `alcance_<balde>` | Mesmo fluxo do anexo. (`anexo_f5.txt:200–225`; incrementos **[não verificado]**.) | Hoje ignorados; seriam as parcelas da conferência do universo **[não verificado: implementação futura]**. (`bin/fio_estado.py:69–92`.) |
   | `alcance.alcance_competencias`, `alcance_e_detalhe`, `alcance_dias` | O anexo os inclui no dicionário carimbado. (`anexo_f5.txt:200–225`; preenchimento integral **[não verificado]**.) | Hoje ignorados; o contrato não especifica quais desses campos serão publicados **[não verificado]**. (`bin/fio_estado.py:82–92`.) |
   | `censo.fio_morto_alcance` | A F5 copia `alc['fio_morto_alcance']` para o censo carimbado. (`anexo_f5.txt:200–225`.) | Hoje só participaria de `fora_do_esperado` **se também existir a chave em `esperado`**; não é exportado como contador próprio. (`bin/fio_estado.py:70–73`, `:82–92`.) |
   | `esperado.fio_morto_alcance` | **Não há escritor demonstrado para essa chave:** a declaração atual não a contém, e o trecho F5 apenas copia `ESPERADO_DO_CARIMBO`. (`app/chamados/management/commands/vigiar_fio_morto.py:86–91`; `anexo_f5.txt:200–225`.) | A comparação genérica atual a leria automaticamente, caso existisse; a manchete continuaria antiga. (`bin/fio_estado.py:70–73`, `:101–108`.) |

   Portanto, o texto “esperado 0” de `linhas_alcance` não demonstra `esperado.fio_morto_alcance` no JSON **[não verificado: declaração F5 não fornecida]**. O contrato exige esse dado no carimbo; usar `0` como substituto no leitor contrariaria o desenho atual de expectativa transportada pelo escritor. (`bin/fio_estado.py:14–22`, `:70–73`.)

2. **Estados do carimbo e sua apresentação**

   A coluna “contrato” abaixo descreve a especificação fornecida; sua implementação futura está **[não verificado]**. Hoje o JSON não contém `cor`: “amarelo/verde” aparecem como convenções nos comentários e no selo. (`bin/fio_estado.py:67–92`; `bin/tests/test_fio_estado.sh:14`, `:56–60`.)

   | Estado | Código atual | Contrato |
   |---|---|---|
   | Ausente, sem permissão ou JSON sintaticamente inválido | `medido=False`, texto **NUNCA MEDIDO**. (`bin/fio_estado.py:63–68`, `:97–100`.) | AMARELO. |
   | Antigo: sem `alcance` | Publica o contador antigo normalmente; não reconhece “F5 não pousou”. Pode satisfazer o caso chamado verde pelo selo. (`bin/fio_estado.py:101–104`; `bin/tests/test_fio_estado.sh:40–60`.) | Aviso “carimbo sem alcance”; nunca VERDE. |
   | Novo, íntegro, com universo positivo | Continua publicando `censo.fio_morto`; ignora a partição. Só acusa o contador novo se ele estiver em `esperado`. (`bin/fio_estado.py:70–73`, `:101–108`.) | Manchete de alcance contra esperado lido; VERDE depende das validações. |
   | Novo, cego ou universo zero/ausente | Ignora a cegueira de alcance; só escreve **CEGO** quando `censo.cego` é verdadeiro. (`bin/fio_estado.py:91`, `:109–111`.) | VERMELHO. |
   | Partição quebrada ou espelho `censo` divergente | Não há essas conferências. (`bin/fio_estado.py:69–92`.) | VERMELHO. |
   | Rodada com `--empresa` | Não altera o carimbo: o painel continua lendo a última fotografia de frota, ou fica nunca medido se ela não existir. (`app/chamados/management/commands/vigiar_fio_morto.py:194–212`; `bin/fio_estado.py:62–68`.) | Não constitui um formato novo de carimbo. |
   | Temporalmente velho | Acima de 26 horas, marca `velho=True` e avisa no texto. Data ausente/inválida deixa idade desconhecida e `velho=False`. (`bin/fio_estado.py:56`, `:74–84`, `:112–117`.) | A precedência entre idade, erro estrutural e cor não foi especificada **[não verificado]**. |

3. **“Nenhum caminho levanta exceção para o ESTADO” não vale hoje**

   A proteção cobre abertura e decodificação, mas não valida os tipos depois disso: JSON `[1]` falha no `.get`; `censo=[1]` falha na comparação; `medido_em=7` causa `TypeError`, enquanto o tratamento captura apenas `ValueError`. (`bin/fio_estado.py:63–80`.)

   Essas exceções chegam à composição do ESTADO porque importação, leitura e formatação do fio são chamadas sem proteção nessa seção. (`bin/gerar_estado.py:260–261`.) O contrato precisa cobrir também estrutura, tipos e composição da linha; tratar apenas JSON ilegível não basta. (`bin/fio_estado.py:69–120`.)

4. **Há consumidores que mudam de comportamento**

   `gerar_estado` consome o dicionário Python por `ler`/`linha`, não o JSON serializado nem uma chave `cor`; a nova manchete chegará ao painel pela alteração de `linha`. Apenas acrescentar `cor` não a publica. (`bin/gerar_estado.py:260–261`, `:313–315`, `:350`.)

   O consumidor explícito do JSON encontrado é o selo, que lê `medido`, `velho` e as chaves de `fora_do_esperado`. Acrescentar campos preserva essa interface; alterar esses campos muda o teste. Consumidores externos à cópia: **[não verificado]**. (`bin/tests/test_fio_estado.sh:51`.)

   Reaproveitar a chave pública `fio_morto` para guardar alcance mudaria sua semântica: hoje ela significa exatamente `censo.fio_morto`. Preservá-la e acrescentar `fio_morto_alcance` mantém a distinção entre os universos. (`bin/fio_estado.py:86`, `:101–104`.)

5. **Há uma ambiguidade entre manchete nova e regressão antiga**

   Mesmo com alcance correto, o leitor atual continua comparando **todas** as expectativas antigas. Assim, alcance conforme e contador antigo divergente ainda produzem `fora_do_esperado`. (`bin/fio_estado.py:70–73`; `app/chamados/management/commands/vigiar_fio_morto.py:86–91`.)

   O contrato manda rotular o antigo como regressão, mas não determina se essa regressão participa da nova `cor` **[não verificado]**. Essa decisão precisa ficar explícita; trocar apenas a manchete preserva um veredito com escopo antigo. (`bin/fio_estado.py:101–108`.)

6. **Conferir a soma pode ser propriedade fixa, sem virar segundo censo**

   Conferir, entre campos já carimbados, `a+b+c+d+e == fio_morto_alcance`, as partes contra o universo e a igualdade dos dois exemplares do contador é validação de integridade. A L-110 nomeia expressamente “partes somam o total” como propriedade fixa. (`CLAUDE.md:45–51`.)

   A fronteira da LEI-AKITA 2 é reconstruir universo ou classificação: percorrer `alcance_dias`, reaplicar datas, decidir causas ou corrigir os contadores seria derivação própria. (`CLAUDE.md:20–22`; `bin/fio_estado.py:29–34`.) Uma soma que fecha também não prova que os dias foram classificados corretamente; o trecho integral dessa classificação não foi fornecido **[não verificado]**.

7. **Onde pode aparecer “VERDE sem medida”** [nao verificado]

   Hoje, estes casos satisfazem o critério `medido=True`, `velho=False`, `fora_do_esperado={}` que o selo chama verde — embora o leitor não emita uma cor propriamente dita. (`bin/tests/test_fio_estado.sh:51–60`.)

   - `{}` ou `null`: tornam-se registro vazio, com `medido=True`, expectativa vazia e idade desconhecida. (`bin/fio_estado.py:65`, `:69–84`.)
   - `esperado` ausente/vazio: nenhuma comparação ocorre, mesmo havendo contador não zero. (`bin/fio_estado.py:70–73`.)
   - Carimbo antigo com contadores conformes: nenhuma medida de alcance é exigida. (`bin/fio_estado.py:69–92`.)
   - Alcance cego ou universo vazio com censo antigo conforme: a cegueira nova é ignorada. (`bin/fio_estado.py:86–92`, `:109–111`.)
   - `censo.cego=True`: aparece o aviso textual, mas não entra automaticamente em `fora_do_esperado`; o escritor atual não inclui `cego` em `esperado`. (`app/chamados/management/commands/vigiar_fio_morto.py:80–91`; `bin/fio_estado.py:72–73`, `:109–111`.)
   - Medição sem data válida: `velho=False` não significa que a fotografia seja recente. (`bin/fio_estado.py:74–84`.)

   Há ainda um desencontro possível de destino: o escritor aceita `VIGIA_FIO_CARIMBO`, enquanto o leitor usa caminho fixo; o selo compara somente as constantes. Se houver redirecionamento, o painel pode continuar lendo outra fotografia. Configuração de produção: **[não verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:97–107`; `bin/fio_estado.py:55`, `:62`; `bin/tests/test_fio_estado.sh:93–102`.)

8. **O selo atual e a promessa de pre-push não fecham a garantia nova** [nao verificado]

   O selo fabrica somente formato antigo; não consulta `cor`; seu caso cego exige apenas a palavra **CEGO**. Ele pode continuar passando sem provar alcance, vermelho por cegueira ou aviso de formato antigo. (`bin/tests/test_fio_estado.sh:40–51`, `:104–107`.) As provas por mutação dos casos novos estão **[não verificado]**; a regra exige que o selo morda a costura. (`CLAUDE.md:350–355`.)

   Colocar o novo selo em `bin/tests/test_*.sh` o inclui na régua, mas **não garante execução direta no pre-push**: o próprio script declara que esse loop não roda ali. (`bin/regua.sh:138–145`; `bin/pre-push.sh:12–16`.)

9. **O primeiro carimbo novo não basta para apagar o ramo antigo**

   Uma rodada de recorte não substitui o arquivo; o escritor atual ainda produz o formato antigo. Portanto, observar um primeiro carimbo F5 não demonstra que nenhum antigo possa voltar a existir. (`app/chamados/management/commands/vigiar_fio_morto.py:194–212`.) Essa impossibilidade operacional permanece **[não verificado]**; a L-111 exige retirar o sítio quando ele perde o último uso, junto com testes e pendência. (`CLAUDE.md:341–344`.)

## 30/10 22:55 · sessao bfc8fe859b37 · - · - · origem=Ronald (tmux)

- **Soma como propriedade fixa:** `CLAUDE.md:45-51` define expressamente “partes somam o total” como propriedade fixa da L-110. Conferir igualdades entre contadores já carimbados valida integridade; reconstruir universo, classificar dias ou corrigir números seria derivação própria proibida por `CLAUDE.md:20-22`. Há precedente no selo do produtor: `app/chamados/tests/test_o261_vigia_fio.py:647-668`.

- **Selo atual pode continuar verde sem validar alcance:** gera exclusivamente carimbos antigos (`bin/tests/test_fio_estado.sh:40-48`), observa somente `medido`, `velho` e `fora_do_esperado` (`bin/tests/test_fio_estado.sh:51`) e considera casos antigos “VERDE” (`bin/tests/test_fio_estado.sh:56-91`). Acrescentar `cor=AMARELO` sem alterar essas três saídas não faria esses casos falharem (`bin/tests/test_fio_estado.sh:51`).

- **Cegueira atual não prova vermelho:** o caso apenas procura `CEGO` no texto, sem conferir campo `cor` ou veredito (`bin/tests/test_fio_estado.sh:104-107`). O selo exige caso que morda a costura; texto presente sozinho não demonstra a cor exigida (`CLAUDE.md:350-355`).

- **Mutações prometidas:** o arquivo atual encerra declarando sete casos, mas não executa mutações de leitor (`bin/tests/test_fio_estado.sh:109`). A comprovação por mutação dos novos casos permanece **[não verificado]**.

- **L-111:** apagar o ramo antigo exige demonstrar que perdeu seu último uso; primeiro carimbo novo isoladamente não demonstra impossibilidade de carimbo antigo retornar **[não verificado]**. A lei manda retirar sítio sem chamador, testes e pendência no mesmo commit (`CLAUDE.md:341-344`). [nao verificado]

- **Entrada dos selos:** a régua enumera `bin/tests/test_*.sh` (`bin/regua.sh:138`); o pre-push documenta que esse diretório não roda diretamente ali (`bin/pre-push.sh:15`).

## 30/10 22:55 · sessao 70a74bc5d59a · - · - · origem=Ronald (tmux)

- O consumidor de produção encontrado é `gerar_estado`: importa `ler`/`linha`, passa o dicionário inteiro a `linha` e publica o texto resultante. Não consulta `cor`, `alcance` ou contadores diretamente; a mudança de manchete chega ao ESTADO pela alteração de `linha`. (`bin/gerar_estado.py:260-261`, `bin/gerar_estado.py:313-315`, `bin/gerar_estado.py:350`)

- O consumidor do JSON encontrado é o selo atual: consulta somente `medido`, `velho` e as chaves de `fora_do_esperado`; também importa `CARIMBO`. Acrescentar campos preservando esses campos mantém essa interface; renomeá-los/removê-los rompe o selo. (`bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:97`)

- O JSON público atual contém `fio_morto=censo.fio_morto`; a linha também lê esse campo. Reutilizar `fio_morto` para conter alcance mudaria sua semântica; acrescentar `fio_morto_alcance` e preservar o antigo evita essa troca. Consumidores externos à cópia: [não verificado]. (`bin/fio_estado.py:86`, `bin/fio_estado.py:101-104`, `bin/fio_estado.py:129`)

- F5 só escrever `esperado.fio_morto_alcance` e `censo.fio_morto_alcance` já faria o leitor atual acrescentar esse desvio a `fora_do_esperado`, porque ele itera todas as expectativas. Isso não muda a manchete, que continua no contador antigo. (`bin/fio_estado.py:69-73`, `bin/fio_estado.py:101-108`)

- O selo antigo só cria carimbos sem `alcance`. Seus casos chamados VERDE verificam `True|False|-`; não verificam `cor`. Portanto acrescentar `cor=AMARELO` para formato antigo pode preservar esses testes, mas o selo continuará chamando esses casos de VERDE sem testar o novo significado. (`bin/tests/test_fio_estado.sh:40-48`, `bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:56-60`, `bin/tests/test_fio_estado.sh:82-86`)

- A régua executa automaticamente `bin/tests/test_*.sh`; o pre-push não executa esse loop e declara isso expressamente. Colocar o novo selo em `bin/tests/` garante sua descoberta pela régua, mas não sua execução direta a cada pre-push. O hook instalado real é [não verificado]. (`bin/regua.sh:138-145`, `bin/pre-push.sh:12-19`, `bin/pre-push.sh:61-66`)

- O instalador vigente usa `bin/pre-push.sh` como fonte e gera wrapper que executa esse caminho; a cópia histórica de `app/core/hooks/pre-push.sh` não é o alvo declarado por esse instalador. (`bin/hooks.sh:9-11`, `bin/hooks.sh:51-56`)

## 30/10 22:54 · sessao 9ae75d7ba88f · O261-V-FASE · verificador · origem=Code

**Sem achado procedente no delta examinado:** C9 e C10 passam pelo mesmo mapa dono→porta; BATIDA abre regularização, ESTRUTURA fica sem URL e somente CADASTRO recebe a porta específica da assinatura (`app/escala/services/cadastro_realidade.py:481`, `app/escala/services/cadastro_realidade.py:490`, `app/escala/services/cadastro_realidade.py:518`).

1. **Quem escreve e quem lê**

- **C9:** `lavrar_teto_do_dia` chama medição e persistência; a medição pergunta a `InsumosDoDono.dono()` e grava `dono` no detalhe do snapshot (`app/ponto/management/commands/lavrar_teto_do_dia.py:28`, `app/ponto/management/commands/lavrar_teto_do_dia.py:34`, `app/ponto/services/teto_na_lista.py:86`, `app/ponto/services/teto_na_lista.py:113`). `_teto_do_dia()` lê a lavra e copia esse valor para `dono_do_dia` (`app/escala/services/cadastro_realidade.py:320`, `app/escala/services/cadastro_realidade.py:325`).
- **C10:** o censo monta `InsumosDoDono` com os dias de fase daquela corrida, pergunta o dono de cada dia e grava `dono_do_dia`; `fase_12x36.lavrar()` persiste o detalhe (`app/ponto/management/commands/censo_fase_12x36.py:260`, `app/ponto/management/commands/censo_fase_12x36.py:262`, `app/ponto/management/commands/censo_fase_12x36.py:272`, `app/escala/services/fase_12x36.py:90`). `_fase_deslocada()` lê a lavra até `hoje` e preserva o dono (`app/escala/services/cadastro_realidade.py:343`, `app/escala/services/cadastro_realidade.py:346`).
- **Juiz comum:** `InsumosDoDono.dono()` chama `dono_da_divergencia()`; a lista recebe o resultado lavrado de cada assinatura e calcula `acao`/`url` (`app/ponto/management/commands/e6_oraculo.py:166`, `app/ponto/management/commands/e6_oraculo.py:173`, `app/escala/services/cadastro_realidade.py:657`, `app/escala/services/cadastro_realidade.py:661`, `app/escala/services/cadastro_realidade.py:664`).
- **Tela/PDF:** a view fornece a mesma lista aos dois destinos. O template lê dono, ação e URL; o PDF imprime dono e ação, sem hyperlink da porta (`app/escala/views.py:490`, `app/escala/views.py:492`, `app/escala/views.py:495`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:794`, `app/escala/services/cadastro_realidade.py:797`).
- **Vigia:** lê `dono_do_dia` por `dias_na_lista()`, sem consultar o texto do botão. C9/C10 conflitantes resultam em `None`; somente BATIDA/CADASTRO entram em `fio_do_admin`, mantendo ESTRUTURA no alarme (`app/escala/services/cadastro_realidade.py:728`, `app/escala/services/cadastro_realidade.py:731`, `app/escala/services/cadastro_realidade.py:753`, `app/chamados/services/vigia_fio.py:259`, `app/chamados/services/vigia_fio.py:266`, `app/chamados/services/vigia_fio.py:314`).

2. **Mapa, rótulos e leitores antigos**

Não restaram dois mapas nesses caminhos. As diferenças entre C9 e C10 são os destinos de **CADASTRO**: template/vínculo na C9; fio ou “só listado”, conforme `aplicavel`, na C10. BATIDA passa à regularização inclusive antes do piso; ESTRUTURA e dono ausente não recebem porta de cadastro (`app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:484`, `app/escala/services/cadastro_realidade.py:519`).

O novo rótulo “o do juiz do dia, e6_oraculo…” alimenta `por_assinatura` e aparece no **title do selo superior**. O PDF imprime o dono efetivo do dia — BATIDA/CADASTRO/ESTRUTURA — e não essa frase agregada (`app/escala/services/cadastro_realidade.py:118`, `app/escala/services/cadastro_realidade.py:699`, `app/templates/escala/cadastro_x_realidade.html:19`, `app/escala/services/cadastro_realidade.py:796`).

Não localizei leitor executável dependente dos textos antigos. Os testes C9 encontrados prendem dono e URLs; “Regularizar a saida do dia” permanece no relato histórico (`app/ponto/tests/test_col129_teto_vira_lista.py:215`, `app/ponto/tests/test_col129_teto_vira_lista.py:226`, `app/docs/RELATO.md:151`). A prosa alterada explicita a morte de `fora_de_fase` e a leitura da lavra (`app/chamados/services/proposta_no_fio.py:22`, `app/core/tests/test_selo_performance.py:141`).

3. **Os testes mordem?** [nao verificado]

**Sim, para os dois defeitos visados:** um cria batida extra real, percorre censo→lavra→lista e exige dono BATIDA com URL de regularização; o outro exige identificação do juiz no rótulo agregado (`app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:206`).

A cobertura nova é parcial: não prende ESTRUTURA, dono ausente, BATIDA antes do piso nem renderização do title/PDF; o teste do rótulo aceita uma substring (`app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:209`).

Exercitei em memória 20 combinações de C9/C10, piso e dono, coerentes com o mapa; a comparação isolada reproduziu base→fio e cópia→regularização para C10/BATIDA (`app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:511`). **Execução dos testes Django e renderização real: [não verificado]**; faltam as dependências neste ambiente.

## 30/10 22:53 · sessao d7a63fdce16d · - · - · origem=Ronald (tmux)

Nenhum defeito concreto encontrado neste recorte.

- Os dois testes novos exercitam comportamento: Batida extra real → censo/lavra → lista → dono BATIDA e URL de regularização; outro verifica a identificação do juiz no rótulo agregado. `app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:206`.
- Confirmei por execução de AST em memória, usando `python3 -B`, 32 combinações entre base/alvo, C9/C10, quatro donos e piso aplicável/inaplicável: base encaminhava C10 aplicável BATIDA/ESTRUTURA/sem dono ao fio; alvo encaminha BATIDA à regularização, ESTRUTURA/sem dono a nenhuma URL, preservando CADASTRO conforme piso. Código exercitado: `app/escala/services/cadastro_realidade.py:478`, `app/escala/services/cadastro_realidade.py:511`. Não executei a integração Django: `[não verificado]`.
- Lacunas dos dois testes adicionados: não verificam ESTRUTURA, ausência de dono, BATIDA anterior ao piso, renderização do `title` nem extração do PDF; a asserção do rótulo é somente substring. `app/escala/tests/test_o261_v_fase_na_lista.py:193`, `app/escala/tests/test_o261_v_fase_na_lista.py:209`.
- O texto novo aparece no `title` do selo agregado, lido de `por_assinatura`; a linha da tela usa dono efetivo. `app/templates/escala/cadastro_x_realidade.html:19`, `app/templates/escala/cadastro_x_realidade.html:28`.
- O PDF não imprime o rótulo novo agregado: imprime `a['dono']` por dia e `a['acao']`, após a sobreposição do dono lavrado. `app/escala/services/cadastro_realidade.py:660`, `app/escala/services/cadastro_realidade.py:666`, `app/escala/services/cadastro_realidade.py:794`.
- Não achei leitor/teste do texto antigo exato; única ocorrência restante de “Regularizar a saida do dia” está no relato histórico. `app/docs/RELATO.md:151`. Os testes C9 existentes dependem das URLs e donos, preservados: `app/ponto/tests/test_col129_teto_vira_lista.py:215`, `app/ponto/tests/test_col129_teto_vira_lista.py:226`.
- A prosa alterada declara a morte de `fora_de_fase` e a leitura da lavra; o parágrafo anterior permanece como explicação histórica de custo. `app/chamados/services/proposta_no_fio.py:15`, `app/chamados/services/proposta_no_fio.py:22`, `app/core/tests/test_selo_performance.py:141`.

## 30/10 22:53 · sessao ba5d8129f418 · - · - · origem=Ronald (tmux)

Não encontrei defeito procedente na cadeia dono→porta examinada; C9 e C10 chamam o mesmo mapa, que só entrega a porta de cadastro para `CADASTRO` (`app/escala/services/cadastro_realidade.py:481`, `:490`, `:518`).

- **C9 escreve:** `medir()` monta `InsumosDoDono` e grava seu resultado em `dono` (`app/ponto/services/teto_na_lista.py:72`, `:86`); `lavrar()` persiste o detalhe em `MetricaSnapshot` (`:110`, `:113`). O comando chama medição e grava somente com `--apply` (`app/ponto/management/commands/lavrar_teto_do_dia.py:28`, `:31`, `:34`).
- **C10 escreve:** o censo identifica os dias e respectivos vínculos/templates (`app/ponto/management/commands/censo_fase_12x36.py:210`), pergunta pelo dono ao mesmo `InsumosDoDono`, passando os dias de fase desta corrida (`:255`, `:260`, `:262`), e inclui `dono_do_dia` nos itens (`:272`). `fase_12x36.lavrar()` persiste esse detalhe (`app/escala/services/fase_12x36.py:84`, `:90`).
- **Juiz comum:** `InsumosDoDono.dono()` passa as informações de batida, fase e cadastro para `dono_da_divergencia()` (`app/ponto/management/commands/e6_oraculo.py:166`, `:169`, `:173`, `:176`). A precedência implementada é cadastro inválido, batida fora do turno/contagem ímpar, fase, demais casos (`:97`).
- **Leitura e composição:** C9 lê o último snapshot (`app/ponto/services/teto_na_lista.py:120`) e copia `dono` para `dono_do_dia` (`app/escala/services/cadastro_realidade.py:318`, `:325`); C10 lê a lavra até `hoje` (`app/escala/services/fase_12x36.py:96`, `:103`; `app/escala/services/cadastro_realidade.py:343`, `:346`). A lista passa o dono de cada dia à ação e sobrepõe o dono genérico da assinatura (`app/escala/services/cadastro_realidade.py:653`, `:657`, `:660`, `:661`, `:664`).
- **Porta comum:** BATIDA→`Regularizar a batida do dia` + regularização do colaborador; CADASTRO→porta específica recebida; ESTRUTURA→sistema sem URL; ausente→aguardar lavra sem URL (`app/escala/services/cadastro_realidade.py:518`). A porta CADASTRO da C9 é o template do dia ou vínculo (`:481`); da C10 é fio se aplicável, senão só listado (`:490`).
- **Tela e PDF:** a view entrega a mesma lista para HTML ou PDF (`app/escala/views.py:490`, `:491`, `:495`). O HTML da linha lê dono efetivo, ação e URL (`app/templates/escala/cadastro_x_realidade.html:28`); PDF lê dono efetivo e ação, sem URL (`app/escala/services/cadastro_realidade.py:794`, `:796`, `:797`).
- **Novo rótulo C10:** a frase “o do juiz do dia…” está na definição (`app/escala/services/cadastro_realidade.py:118`), alimenta `por_assinatura` (`:699`, `:700`) e aparece no `title` do selo superior (`app/templates/escala/cadastro_x_realidade.html:19`). Não aparece como frase no PDF; o PDF usa o dono efetivo da linha (`app/escala/services/cadastro_realidade.py:796`).
- **Vigia:** `dias_na_lista()` lê `dono_do_dia` da própria lista e resolve conflitos entre C9/C10 sem depender da ordem (`app/escala/services/cadastro_realidade.py:728`, `:731`, `:748`, `:753`). O vigia chama lista e leitor uma vez e aceita exclusivamente BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:255`, `:259`, `:266`); ESTRUTURA permanece em `fio_morto` (`:314`, `:324`).
- **Texto antigo:** a busca encontrou “Regularizar a saida do dia” apenas em relato histórico, sem dependência de código identificada nessa busca (`app/docs/RELATO.md:151`). O novo texto vem do helper único (`app/escala/services/cadastro_realidade.py:520`).

Executei, sem importar Django e sem escrita, as funções extraídas por AST numa matriz de 16 casos: C9/C10 × aplicável/não aplicável × BATIDA/CADASTRO/ESTRUTURA/ausente. Os resultados confirmaram o mapa acima (`app/escala/services/cadastro_realidade.py:478`, `:511`); integração com banco/renderização real não executada [não verificado].

## 30/10 22:40 · sessao bc2cecd364ad · O261-E-CELULA-20 · verificador · origem=Code

**O delta não garante “papel lido da porta” nem “um dono”: há atribuições locais de `admin`, dois contraexemplos de responsável e possibilidade de `admin+colaborador`.** (`app/chamados/services/vigia_fio.py:481`, `:574`, `:579`)

**1. Papel por rota e quem pode agir**

| Rota | Verificação |
|---|---|
| `porta_fila` | Grava `admin` diretamente. `qs_na_fila` informa presença na fila, sem papel; o leitor existente distingue `colab/cobrar` de `admin_validar/validar`. A tela administrativa permite agir conforme departamento ou gestor geral. (`app/chamados/services/vigia_fio.py:574`; `app/chamados/services/painel.py:186`; `app/chamados/catalogo/natureza.py:41`; `app/chamados/utils.py:52`; `app/chamados/views.py:418`) |
| `porta_pergunta` | Deriva colaborador/admin de `respondida_em`. Há testemunha equivalente na API: `p.estado` e `aguarda_admin`. Pendente: colaborador responde; respondida: autoridade administrativa competente valida, e o colaborador ainda pode corrigir. (`app/chamados/services/vigia_fio.py:579`; `app/api/views.py:2621`; `app/chamados/views.py:1239`, `:1280`, `:483`; `app/templates/chamados/partials/_card_atendimento.html:101`) |
| `de_hoje_pra_frente` | Grava `nenhuma porta` depois de falharem as duas portas anteriores, **antes de consultar os quatro baldes**. Portanto não demonstra ausência de toda ação possível naquele dia; quem pode agir além dessas duas consultas: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:582`, `:588`) |
| `decisao_do_dia_pendente` | O leitor comprova oferta de “Resolver dia”, mas exclui expressamente a permissão do usuário. Tela e POST exigem `regularizar_batida`, concedida à supervisão na semeadura padrão. `admin` vem do mapa local. (`app/colaboradores/services/calendario.py:203`; `app/templates/colaboradores/partials/_calendario_grade.html:125`; `app/ponto/views.py:3305`; `app/colaboradores/mixins.py:355`; `app/chamados/services/vigia_fio.py:481`) |
| `aguarda_decisao_de_ausencia` | O leitor aceita os dois estados de `ABERTAS`: documento devido pelo colaborador e decisão devida pelo DP. Colaborador anexa pela API; usuários com `editar_ausencias` anexam/decidem pelo painel. O mapa escreve `admin` para ambos. (`app/chamados/services/vigia_fio.py:258`; `app/ponto/services/ausencia.py:719`, `:749`; `app/api/views.py:3125`; `app/ponto/views.py:2139`, `:2589`; `app/chamados/services/vigia_fio.py:481`) |
| `escala_nao_descreve_o_dia` | Basta pergunta validada com aceite de folga; esse ramo não consulta lista nem porta de correção. Existem wizard com `editar_escalas` e edição de vínculo com `editar_cadastro`; qual porta está oferecida para cada dia: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:261`; `app/escala/views_wizard.py:63`; `app/colaboradores/views.py:16`) |
| `fio_do_admin` | Lê presença do dia e dono BATIDA/CADASTRO, não perfil administrativo. A lista aponta regularização, template ou vínculo, sujeitos às permissões correspondentes. A regularização é explicitamente descrita como porta da supervisão. (`app/chamados/services/vigia_fio.py:204`; `app/escala/services/cadastro_realidade.py:445`, `:20`; `app/colaboradores/views.py:2948`; `app/escala/views_wizard.py:63`; `app/colaboradores/views.py:16`) |
| `sem_porta` | `nenhuma porta` significa que falharam as portas e leis consultadas pelo censo. Não identifica um responsável por reparar o caso; isso permanece `[nao verificado]`. (`app/chamados/services/vigia_fio.py:572`, `:588`, `:632`) |

Dois contraexemplos demonstráveis pelo código:

- **Fila com pergunta pendente:** chamado aberto ligado à célula + pergunta pendente recebe `porta_fila/admin`, porque esse ramo vence antes de olhar perguntas. O leitor existente daria `colab/cobrar`. (`app/chamados/services/vigia_fio.py:572`; `app/chamados/catalogo/natureza.py:48`, `:58`)
- **Ausência aguardando documento:** recebe `admin`, embora a próxima obrigação seja do colaborador; aprovação nesse estado é recusada pela máquina. (`app/chamados/services/vigia_fio.py:258`, `:481`; `app/ponto/services/ausencia.py:719`, `:751`, `:795`)

A exclusão categórica de supervisor também não se sustenta como descrição de quem pode agir: há portas da supervisão e permissões concedidas a ela. Os usuários efetivamente autorizados em produção são `[nao verificado]`. (`app/escala/services/cadastro_realidade.py:20`; `app/colaboradores/mixins.py:355`, `:322`)

**2. Dias e contadores**

Na execução normal, com universo dentro do `limite`, **nenhum dia some nem duplica**: o universo usa chave `(colab_id, data)`; cada ramo acrescenta uma linha e encerra aquela iteração, ou chega ao `sem_porta`. Dias sem célula também entram, com `celula_id=None`. (`app/chamados/services/vigia_fio.py:518`, `:524`, `:564`, `:569`, `:572`, `:576`, `:582`, `:590`, `:600`, `:632`)

Pelo delta fornecido, **os contadores preservam a classificação anterior**: `any(...)` virou lista com o mesmo predicado, e o limite continua aplicado somente à inclusão na lista. Mudou o contrato da saída: `alcance_dias_sem_porta` virou `alcance_dias`, abrangendo todas as rotas. (`app/chamados/services/vigia_fio.py:513`, `:565`, `:576`, `:628`)

Uma linha pode conter **`admin+colaborador`** quando há perguntas elegíveis pendentes e respondidas no mesmo dia. Isso não duplica o dia, mas tampouco entrega “um dono”. Ocorrência em produção: `[nao verificado]`. (`app/chamados/services/vigia_fio.py:579`)

**3. Testes que aceitam papel errado** [nao verificado]

**Sim.** O teste novo `test_supervisor_nunca_e_escrito...` aceita, por exemplo, trocar o papel de `aguarda_decisao_de_ausencia`, `escala_nao_descreve_o_dia` ou `fio_do_admin` para `colaborador`: ele só proíbe `supervisor` e compara as chaves do mapa. Os outros dois testes novos não verificam essas três rotas. (`app/chamados/tests/test_o261e_f5_alcance.py:281`, `:291`, `:299`)

O teste da fila exige `admin` num chamado **sem pergunta**; não cobre a fila com pergunta pendente, onde diverge do leitor de dono. O teste de decisão pendente usa um booleano mockado, sem conferir permissão ou ação pela tela. (`app/chamados/tests/test_o261e_f5_alcance.py:293`, `:296`; `app/chamados/catalogo/natureza.py:50`)

Resultado da suíte Django e quantidades/ocorrências reais em produção: `[nao verificado]`. [nao verificado]

## 30/10 22:39 · sessao 44e003a85fc7 · O261-V-FASE · verificador · origem=Code

**Não encontrei novo procedente nos caminhos examinados da cópia.** A conferência integral do diff `aa96b93c..9c9e030e` fica **[nao verificado]**: esta cópia não oferece o histórico Git necessário.

1. **Escritores, leitores e corte literal**

   - **Fase:** o censo produz os dias de desvio e injeta os dias desta corrida em `InsumosDoDono`, evitando consultar a lavra anterior enquanto escreve (`app/ponto/management/commands/censo_fase_12x36.py:210`, `:260`). Nos demais usos, `InsumosDoDono.fase()` lê `ultimo_lavrado(self.hoje)`, filtra pelo colaborador e transforma a presença do dia no booleano `fase_deslocada` (`app/ponto/management/commands/e6_oraculo.py:159`, `:169`).
   - **Dono lavrado:** o censo consulta `_ins.dono()` para cada dia, grava o resultado em `dono_do_dia` e persiste pelo serviço `lavrar()` (`app/ponto/management/commands/censo_fase_12x36.py:262`, `:273`; `app/escala/services/fase_12x36.py:90`).
   - **Leitores operacionais do juiz:** e6, pautas discordantes e teto usam `InsumosDoDono.dono()`; o pré-filtro das pautas também passa explicitamente a fase (`app/ponto/management/commands/e6_oraculo.py:309`, `:359`; `app/ponto/management/commands/pautas_discordantes.py:96`, `:109`; `app/ponto/services/teto_na_lista.py:72`, `:86`).
   - **Leitores do dono da lavra:** C10 preserva `dono_do_dia`; `lista()` o exibe; `dias_na_lista()` consolida por colaborador/dia; o vigia aceita somente BATIDA/CADASTRO (`app/escala/services/cadastro_realidade.py:343`, `:650`, `:742`; `app/chamados/services/vigia_fio.py:259`, `:266`).
   - **Demais consumidores da fase:** API, fila, propositor, pauta semanal e vigia da família leem o snapshot; não arbitram o dono do dia (`app/api/views_mensageria.py:1672`; `app/chamados/services/fila_por_causa.py:174`; `app/escala/services/propositor.py:492`; `app/escala/services/fase_12x36.py:124`; `app/ponto/management/commands/vigiar_familia_escala.py:24`).

   **Não identifiquei leitor e6 inconsistente com o corte:** BATIDA por turno aberto/batida real ou contagem ímpar precede CADASTRO pela fase. Permanecem os ramos anteriores de cadastro já ressalvados na triagem (`app/ponto/management/commands/e6_oraculo.py:97`, `:103`, `:105`, `:107`).

2. **Sim, o teste novo morde `lista()`.** [nao verificado]

   Ele chama `lista(hoje=30/10)`, substitui apenas `ultimo_lavrado` e exige que esse leitor receba `30/10`; o relógio está congelado em `05/11` (`app/escala/tests/test_o261_v_fase_na_lista.py:177`, `:208`). A cadeia real repassa `hoje` nos dois elos; retirar qualquer repasse deixa de satisfazer a asserção (`app/escala/services/cadastro_realidade.py:611`, `:340`; `app/escala/tests/test_o261_v_fase_na_lista.py:214`). Execução da suíte Django: **[nao verificado]**.

3. **Nenhum novo procedente identificado nesse recorte.**

   Conferi também: pré-filtro das pautas consulta fase antes de pedir motor; C10 recebe o `hoje` da lista; C9/C10 têm igual autoridade e divergência resulta em `None`, preservando o alarme independentemente da ordem (`app/ponto/management/commands/pautas_discordantes.py:96`; `app/escala/services/cadastro_realidade.py:611`, `:710`, `:721`; `app/chamados/services/vigia_fio.py:266`). Efeitos em produção e resultados históricos além do anexo: **[nao verificado]**.

## 30/10 22:39 · sessao 4f714b67b5f3 · - · - · origem=Ronald (tmux)

Achado principal: `porta_fila → admin` não é leitura do responsável. `qs_na_fila()` filtra `VIVOS/COBRANCA` e silêncio, sem retornar papel (`app/chamados/services/painel.py:186-188`); o censo escreve `admin` diretamente e interrompe antes de examinar perguntas (`app/chamados/services/vigia_fio.py:572-581`).

Contraexemplo verificável por código: chamado `batida_ausente`, aberto, ligado à célula, com pergunta pendente ancorada nele. O censo informa `porta_fila/admin`; o leitor de dono existente informa `colab`, convertido em `cobrar` (`app/chamados/catalogo/natureza.py:41-60`). A própria tela considera `cobrar` trabalho do colaborador e só `validar/decidir` exigem admin (`app/chamados/services/verbos.py:17-24`). Helpers permitem construir exatamente esse estado (`app/chamados/tests/test_o261e_f5_alcance.py:66-82`). Não executei Django/testes: resultado de execução `[nao verificado]`.

Para `porta_pergunta`, a distinção pendente/respondida tem testemunha existente: a API serve `p.estado` e publica `aguarda_admin` quando `RESPONDIDA` (`app/api/views.py:2620-2624`); a propriedade do modelo traduz pergunta viva por `respondida_em` (`app/chamados/models.py:2197-2211`). O censo não consulta essa tradução: repete a decisão por carimbo (`app/chamados/services/vigia_fio.py:579-580`).

`admin` também não identifica a autoridade de validação. O catálogo atribui `batida_ausente` a `colab` (`app/chamados/catalogo/modulos.py:105`), que o guard administrativo mapeia a `supervisao` (`app/chamados/utils.py:27-43`); o guard exige departamento correspondente ou gestor geral (`app/chamados/utils.py:47-72`). A tela aplica esse guard (`app/chamados/views.py:418-421`) e oferece validar (`app/chamados/views.py:483-501`; `app/templates/chamados/partials/_pergunta_card.html:92-97`). O supervisor pode entrar como gestor porque `supervisor_posto` recebe `is_staff=True` e `_is_gestor` aceita staff (`app/core/credenciais.py:132-140`; `app/chamados/views.py:35-38`). A afirmação “supervisor nunca sai de nenhuma porta” não descreve essas autoridades.

Mesmo respondida, a pergunta continua corrigível pelo colaborador: a tela diz “Aguardando a supervisao conferir” e permite responder de novo (`app/templates/chamados/partials/_card_atendimento.html:94-101`, `:154-155`); a porta HTML encaminha `ja_respondida=True` ao core (`app/chamados/views.py:1280-1283`), que grava nova resposta (`app/chamados/services/disputa_emissao.py:1247-1249`). Assim, `admin` pode representar o próximo responsável por validar; não representa sozinho todos que podem agir.

Pluralidade: duas perguntas elegíveis do mesmo dia, uma pendente e outra respondida, produzem literalmente `admin+colaborador` (`app/chamados/services/vigia_fio.py:576-580`). Isso mantém uma linha por dia, porém não garante “um dono”; ocorrência no dado de produção `[nao verificado]`.

Testes novos: [nao verificado]

- O teste fila cria chamado sem pergunta e exige `admin`; não cobre o contraexemplo de fila com pergunta pendente (`app/chamados/tests/test_o261e_f5_alcance.py:291-297`).
- O teste pergunta respondida exige `admin`, embora sua fixture use `batida_ausente`, cuja autoridade administrativa é supervisão; não consulta guard/tela nem atua com usuário (`app/chamados/tests/test_o261e_f5_alcance.py:66-82`, `:281-289`; `app/chamados/utils.py:27-43`).
- O teste “supervisor nunca” examina apenas os valores das constantes dos baldes, sem executar censo nem testar as duas rotas de porta (`app/chamados/tests/test_o261e_f5_alcance.py:299-304`). Poderia passar com papel errado em `porta_fila` ou `porta_pergunta`, pois não observa esses resultados (`app/chamados/services/vigia_fio.py:574`, `:579-580`).

## 30/10 22:39 · sessao 913a8ab46598 · - · - · origem=Ronald (tmux)

Os quatro baldes recebem `'admin'` por mapa local; o retorno de `dono_fora_do_fio` é apenas o nome do balde, sem papel/RBAC (`app/chamados/services/vigia_fio.py:256-265`, `:481-482`, `:588-592`).

| Balde | Leitura efetiva | Porta e quem pode agir |
|---|---|---|
| `decisao_do_dia_pendente` | `resolver_dia_oferecido_na_data` monta o calendário e lê o predicado; a função declara expressamente que **não verifica `pode_veredito`**, pois é permissão de quem vê (`app/colaboradores/services/calendario.py:199-216`). | O calendário exige `ver_cadastro` e oferece Resolver dia a quem tem `regularizar_batida` (`app/colaboradores/views.py:1935-1957`); template verifica `pode_veredito` (`app/templates/colaboradores/partials/_calendario_grade.html:125-126`); POST exige `regularizar_batida` (`app/ponto/views.py:3302-3306`). Supervisão recebe essa permissão na semeadura padrão (`app/colaboradores/mixins.py:355-362`). |
| `aguarda_decisao_de_ausencia` | `ausencia_cobre(...para='cobranca')` e status em `ABERTAS` (`app/chamados/services/vigia_fio.py:258-260`). `ABERTAS` reúne **aguardando_documento** e **aguardando_decisao** (`app/ponto/services/ausencia.py:749`). | Documento é devido pelo colaborador, decisão pelo DP (`app/ponto/services/ausencia.py:719-722`). Colaborador autenticado anexa na própria ausência pela API (`app/api/views.py:3125-3144`, `:3153-3156`); anexar passa o estado para decisão (`app/ponto/services/ausencia.py:857-859`). Quem tem `editar_ausencias` também anexa/decide pelo painel (`app/ponto/views.py:2134-2152`, `:2589-2599`), mas aprovar diretamente `aguardando_documento` é transição recusada (`app/ponto/services/ausencia.py:751-753`, `:792-806`). **Há atribuição indistinta de admin a estado cuja próxima obrigação é do colaborador.** |
| `escala_nao_descreve_o_dia` | Basta alguma pergunta `VALIDADA` com resposta `FOLGA_CONTESTA`; esse ramo **não consulta presença na lista nem uma porta de correção** (`app/chamados/services/vigia_fio.py:261-264`; código da resposta em `app/chamados/services/validacao.py:216-218`). | A função comenta que o dono é CADASTRO (`app/chamados/services/vigia_fio.py:245-246`). Portas de cadastro existentes: wizard exige `editar_escalas` (`app/escala/views_wizard.py:63-65`, `:95-98`) e vínculo exige `editar_cadastro` (`app/colaboradores/views.py:16-20`). Qual delas está realmente oferecida **para cada dia desse balde**: `[nao verificado]`; não é lido no ramo (`app/chamados/services/vigia_fio.py:261-262`). |
| `fio_do_admin` | Lista é carregada e reduzida a `(colab,data)->dono_do_dia`; predicado só exige dono BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:168`, `:193-210`; `app/escala/services/cadastro_realidade.py:655-670`). | Lista mostra links `a.url` (`app/templates/escala/cadastro_x_realidade.html:28`); C9/BATIDA aponta regularização e C9/CADASTRO aponta template/vínculo (`app/escala/services/cadastro_realidade.py:445-456`). Regularização é declarada porta da supervisão (`app/escala/services/cadastro_realidade.py:20-22`) e exige `regularizar_batida` (`app/colaboradores/views.py:2948-2958`); wizard exige `editar_escalas`, vínculo `editar_cadastro` (linhas acima). Portanto BATIDA/CADASTRO não equivalem a um perfil exclusivo admin. |

As permissões reais são dinâmicas via `user.get_all_permissions()`, com passe de superuser; os perfis efetivamente autorizados em produção são `[nao verificado]` (`app/colaboradores/mixins.py:322-331`). A semeadura padrão concede `regularizar_batida`, `editar_escalas` e `editar_ausencias` à supervisão (`:355-362`), `editar_ausencias`/`editar_cadastro` ao DP (`:363-370`) e conjunto amplo a TI (`:371-382`). Logo a frase “supervisor nunca sai de nenhuma porta” não é sustentada como exclusão de capacidade: a própria porta C9/BATIDA é apresentada como regularização da supervisão (`app/escala/services/cadastro_realidade.py:20-22`).

## 30/10 22:38 · sessao 97f9cda9c180 · O261-E-CELULA-20 · verificador · origem=Code

**Parecer: REPROVA o delta para fechamento da F1.** Há uma regressão demonstrável na relavra S2, perda de isolamento no E2 e um caminho de segunda passada que deixa a impressão anterior ao fechamento. (`app/chamados/models.py:261`, `:278–288`, `:1774`; `app/ponto/services/cartorio.py:751–759`, `:1187–1191`, `:1449`)

**1. Achados**

- **Cache velho: o ponto comprovado é depois do `save`, não entre gate e núcleo.** O gate guarda as perguntas em `_f1_ancoradas`; durante `self.save()`, S143 busca outras instâncias e pode mudar `materializada_em`, `via_resolucao`, `validada_em` e a lavra S2. Ao retornar, o núcleo relavra os objetos anteriores, sem recarregar. (`app/chamados/models.py:357`, `:261`, `:278–288`; `app/chamados/signals.py:89–92`; `app/chamados/services/disputa_emissao.py:1536–1583`)

  Contraexemplo pelo código: pergunta de escolha ainda sem materialização, com célula que absolve e sem marcos pendentes. C6 permite fechar; S143 carimba e lavra **SIM**; a relavra final usa `materializada_em=None` do snapshot e grava **NÃO**. O novo `julgado_em` pode ainda ficar posterior ao carimbo, impedindo que a comparação de marcos detecte esse envelhecimento. Reprodução em banco e ocorrência no dado fornecido: **[não verificado]**. (`app/chamados/juizes.py:628–629`, `:1054–1065`; `app/chamados/services/disputa_emissao.py:1562–1583`; `app/chamados/veredito_lavrado.py:188–190`, `:219–231`)

- **`atomic(savepoint=False)` remove a recuperação local do fechamento de disputa chamado pelo E2.** E2 captura exceções de `DisputaSupervisao.fechar`; agora, havendo transação externa, uma exceção que atravesse o bloco sem savepoint pode marcar essa transação para rollback, embora E2 continue. É a classe de falha já documentada pelo próprio mecanismo de auditoria. Falha efetivamente provocada nesta cópia: **[não verificado]**. (`app/chamados/reconciliador.py:632–640`; `app/chamados/models.py:1774–1797`; `app/core/observ.py:124–133`)

  Isso **não** demonstra defeito nos dois receivers protegidos: Defesa 1 e S143 conservam seus próprios `atomic()`. (`app/chamados/signals.py:41–46`, `:88–94`)

- **A segunda passada pode consumir o fechamento e descartar a convergência.** Se E2 matar a pergunta, mas não fechar seu chamado-destino — E2 fecha o pai da disputa, que pode ser outro chamado —, `_porm` fecha o destino na segunda passada e reenfileira a célula. O wrapper descarta essa nova fila em `rejulgar_descartado`. A impressão foi calculada antes do fechamento e é gravada depois; sem emissão nova, a releitura final não entra. Quantidade de ocorrências: **[não verificado]**. (`app/chamados/reconciliador.py:623–638`; `app/chamados/signals.py:32–42`; `app/ponto/services/cartorio.py:1089–1094`, `:1187–1191`, `:751–759`, `:804–817`, `:1449–1450`)

- **O ciclo de `cobranca_indevida` permanece.** O corte altera `cobrado` no ramo `not cods`; `COBRANCA_SEM_FURO` e `COBRANCA_EM_DIA_SEM_TRABALHO` continuam dependendo do chamado vivo. Ambos derivam `cobranca_indevida`, que mantém C6 vivo e fica excluída do E2. (`app/ponto/services/cartorio.py:1141–1165`; `app/ponto/supra_juiz.py:418–445`; `app/escala/models.py:1581`, `:1735–1736`; `app/chamados/juizes.py:1054–1057`; `app/chamados/reconciliador.py:598`)

**2. Quem escreve e quem lê**

| Estado | Escrita | Leitura relevante |
|---|---|---|
| `CelulaDia.veredito`, incluindo `cobrado` | Porta `lavrar_veredito`; cartório solicita o valor e depois carimba. (`app/ponto/portas/celula.py:678–695`; `app/ponto/services/cartorio.py:1164–1165`, `:1434–1450`) | C6 e seleção do E2. (`app/chamados/juizes.py:1017–1032`, `:1054–1065`; `app/chamados/reconciliador.py:594–599`) |
| `materializada_em` / `via_resolucao` neste E2 | Escritor existente `retratar_perguntas_absolvidas_por_celula`; o delta acrescenta seu chamador. (`app/chamados/reconciliador.py:617–621`; `app/ponto/services/cartorio.py:749–750`) | C6 e juiz usado pela lavra S2. (`app/chamados/juizes.py:1044–1052`, `:1076–1083`; `app/chamados/veredito_lavrado.py:219–221`) |
| `chamado_destino` no agrupamento | `agrupar_furo` chama `reapontar_resolvedora`, que consulta E4-c, atribui, relavra e salva. (`app/chamados/services/limbo_folgas.py:228–230`; `app/chamados/services/fio.py:821–831`) | Âncora canônica `chamado_que_lista` e guarda F1. (`app/chamados/services/fio.py:97–117`; `app/chamados/models.py:336–343`) |
| Outros escritores de `chamado_destino` | `PerguntaDisputa.abrir` também escreve no dedup e no nascimento; portanto a exclusividade da porta não cobre literalmente toda escrita existente. (`app/chamados/models.py:2370–2397`) | Guarda de costura no dedup. (`app/chamados/models.py:2367–2369`) |
| `status_local` | Transição persistente em `_gravar_estado`; troca temporária para julgar o pós-estado em `_vivas_entre`, restaurada no `finally`. (`app/chamados/models.py:222–261`, `:333–346`) | Motor dos gates, tripwire, juiz de materialização e receivers. (`app/chamados/models.py:215`, `:631`, `:1469`; `app/chamados/juizes.py:699`; `app/chamados/signals.py:70–81`) |
| `_f1_ancoradas` | Lista escrita pelo plano; limpa no consumo, recusa e dry-run. (`app/chamados/models.py:357`, `:211`, `:398`, `:645`) | Tripwire e relavra S2. (`app/chamados/models.py:210–218`, `:278–288`) |
| `_f1_dedup` | Premissa prepara o payload; decisão inicializa, consome e limpa. (`app/chamados/models.py:697–701`, `:639–642`) | Decisão emite `fechamento_por_dedup`. (`app/chamados/models.py:648–650`) |
| `fechamento_recusado` | `_registrar_recusa` solicita; `evento` persiste `LogAuditoria`. (`app/chamados/models.py:372–383`; `app/core/observ.py:134–138`) | A própria porta lê último payload e estados posteriores para deduplicar. (`app/chamados/models.py:378–381`) |
| `fechamento_por_dedup` | Decisão solicita após a guarda, **antes** de `_gravar_estado`; persistência por `evento`. (`app/chamados/models.py:643–650`, `:612`; `app/core/observ.py:134–138`) | Testes conferem sobrevivente e idempotência. (`app/chamados/tests/test_fechar3_retorno_do_gate.py:109–117`) |

**3. Limites e efeitos que não devem virar acusação sem prova**

- **Não demonstrei nova travessia vivo→terminal com pergunta viva causada pelo delta.** Antes do núcleo, a reata modifica as mesmas instâncias do cache; o receiver de pergunta agenda `on_commit` sob o atomic aberto. Um mutador concorrente nessa janela permanece **[não verificado]**. (`app/chamados/models.py:408–419`, `:608–612`; `app/chamados/services/fio.py:828–831`; `app/chamados/signals.py:191–202`)

- **Há caminho preexistente que conserva terminal com viva:** nascimento de pergunta com destino terminal continua fora da guarda F1; depois, `resolvido→fechado` não passa pela verificação reservada à saída de VIVOS. Não é prova de regressão nova deste delta. (`app/chamados/models.py:2396–2397`, `:392–393`, `:215`, `:1468–1477`; `app/chamados/services/fio.py:748`)

- **E2 dentro do julgamento não faz apenas o carimbo.** Sua consulta abrange o colaborador inteiro; ele pode fechar disputas, reconciliar pais e acrescentar notas. Com `_tl.ativo=True`, o receiver suprime o julgamento provocado por esses fechamentos; a segunda passada recebe apenas `rejulgar`. Dano concreto em outros dias: **[não verificado]**. (`app/chamados/reconciliador.py:584–599`, `:632–638`; `app/chamados/signals.py:41–45`, `:174–175`; `app/ponto/services/cartorio.py:738–752`)

- **A trilha dedup adiada corrige a emissão quando a guarda recusa**, mas ainda não é confirmação posterior ao save: continua dependente da transação concluir corretamente. (`app/chamados/models.py:643–650`, `:608–613`)

- **LIMBO e O199 usam as portas existentes solicitadas:** reata antes da reconciliação no agrupamento; novo pedido a `fechar_o_fio_do_dia` após o aceite da irmã no dia já folga. Resultado executado e impacto em produção: **[não verificado]**. (`app/chamados/services/limbo_folgas.py:228–230`; `app/chamados/services/validacao.py:261–271`)

- **Não há prova adicional de segundo juiz, segundo escritor de materialização, inversão de trava ou mudança de teto temporal.** O filtro novo chama `marcos_acesos`; E2 conserva a guarda de marcos pendentes. Regressão de relógio/ordem de trava: **[não verificado]**. (`app/ponto/services/cartorio.py:1140`, `:1164`, `:1181`; `app/chamados/reconciliador.py:604–614`)

**4. Testes vermelhos e cobertura** [nao verificado]

- **Vermelho relatado no anexo e sustentado pelo mecanismo:** `PortaSegundoAtoTest.test_MORDE_derruba_a_cobranca_viva_do_dia_que_virou_folga`, que exige a saída de VIVOS. Reexecução independente: **[não verificado]**. (`app/colaboradores/tests/test_contract_porta_2ato.py:147–160`; `app/ponto/supra_juiz.py:441–445`)

- **Não confirmo HX/B2.d como vermelhos atuais:** suas fixtures já plantam explicitamente o passivo terminal, sem depender de `encerrar()` fabricá-lo. Resultado atual: **[não verificado]**. (`app/chamados/tests/test_laco_hx_x_bug31.py:37–43`; `app/chamados/tests/test_b2d_resposta_visivel.py:16–22`)

- **Tetos do chokepoint e contagem das órfãs:** resultado atual **[não verificado]**; não há medição fornecida que autorize afirmar novo excesso ou novo total. Os testes do chokepoint exigem fechamento efetivo antes de aceitar a contagem. (`app/ponto/tests/test_selo_chokepoint_escrita.py:157–164`, `:360–368`)

- **O teste novo da lâmpada não prende a convergência:** verifica fechamento e ausência de viva, mas não `rejulgar_descartado`, impressão estável ou segunda chamada sem escrita. O selo AST prende chamada/raise antes da escrita; não verifica a atualidade dos objetos reutilizados na S2. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:375–390`; `app/chamados/tests/test_o261e_f1_selo_encerramento.py:24–47`)

A exigência de idempotência é “mesmo estado, uma trilha, zero duplicação”; reutilizar o snapshot após os receivers e terminar com impressão anterior ao fechamento deixa essa exigência sem cumprimento demonstrado. (`CLAUDE.md:299–302`; `app/chamados/models.py:278–288`; `app/ponto/services/cartorio.py:757–759`, `:1449–1450`)

## 30/10 22:38 · sessao d814bc670e38 · - · - · origem=Ronald (tmux)

Não encontrei leitor operacional do e6 que omita a fase na cópia atual: e6 usa `InsumosDoDono` e `.dono()` (`app/ponto/management/commands/e6_oraculo.py:309`, `:359`); pautas usam a montagem, incluem fase no pré-filtro e só pedem motor para ESTRUTURA (`app/ponto/management/commands/pautas_discordantes.py:82`, `:96`, `:98`, `:100`, `:109`); teto também usa a montagem (`app/ponto/services/teto_na_lista.py:72`, `:86`); censo injeta os dias do julgamento corrente e grava o resultado do juiz (`app/ponto/management/commands/censo_fase_12x36.py:260`, `:262`, `:273`).

A fase vem de `ultimo_lavrado(self.hoje)`, filtrada pelo colaborador e cacheada (`app/ponto/management/commands/e6_oraculo.py:159`). Ela entra nas duas chamadas finais possíveis ao juiz, inclusive quando o e6 fornece o espelho previamente (`app/ponto/management/commands/e6_oraculo.py:169`, `:173`, `:176`). A precedência efetiva mantém os ramos anteriores de cadastro, depois BATIDA por turno aberto/batida real ou paridade ímpar, depois CADASTRO pela fase (`app/ponto/management/commands/e6_oraculo.py:97`, `:103`, `:105`, `:107`).

O leitor indireto preserva o dono lavrado da C10, mostra esse dono na lista e resolve C9/C10 no mesmo nível; divergência produz `None`, independentemente da ordem (`app/escala/services/cadastro_realidade.py:343`, `:650`, `:710`, `:713`, `:738`). O vigia consulta exatamente `dias_na_lista(lista(hoje=hoje,...))` e só retira do alarme donos BATIDA/CADASTRO (`app/chamados/services/vigia_fio.py:259`, `:266`).

Não achei residual procedente novo nesses caminhos; o pré-filtro ainda omite os demais insumos de cadastro, situação já registrada na triagem fornecida (`app/ponto/management/commands/pautas_discordantes.py:98`; classificação histórica: [nao verificado]). Não executei suíte; aprovação dinâmica [nao verificado].

## 30/10 22:38 · sessao e312cdb2f279 · - · - · origem=Ronald (tmux)

O teste novo morde `lista()` diretamente: importa o chamador, substitui apenas o leitor `fase_12x36.ultimo_lavrado`, chama `lista(hoje=30/10)` e exige que esse leitor receba `30/10` (`app/escala/tests/test_o261_v_fase_na_lista.py:208-214`). O relógio está congelado em `05/11`, portanto usar o dia corrente não satisfaz a expectativa (`app/escala/tests/test_o261_v_fase_na_lista.py:177`).

A cadeia efetiva é `lista()` → `_fase_deslocada(empresas, hoje)` → `ultimo_lavrado(hoje)` (`app/escala/services/cadastro_realidade.py:611`, `:335-340`). O leitor aplica `data_ref__lte=ate` (`app/escala/services/fase_12x36.py:102-105`). Retirar o repasse em qualquer elo produz chamada com `None` e deixa de satisfazer a asserção (`app/escala/tests/test_o261_v_fase_na_lista.py:214`). Execução RED/GREEN: `[nao verificado]`.

Não encontrei outro residual procedente na resolução dos donos da lista: C9 e C10 participam igualmente como donos do juiz; divergência resulta em `None`, independentemente da ordem (`app/escala/services/cadastro_realidade.py:710-721`). Os testes cobrem as duas ordens e o controle de concordância (`app/escala/tests/test_o261_v_fase_na_lista.py:334-343`). O vigia só considera o dia atendido pela lista quando o resultado é `BATIDA` ou `CADASTRO`; `None` mantém o alarme (`app/chamados/services/vigia_fio.py:263-266`).

_802 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
