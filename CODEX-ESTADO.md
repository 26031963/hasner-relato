# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 17:34** · publicado em **30/10/2026 17:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 17:34 · sessao 7620c0310c9f · - · - · origem=Ronald (tmux)

**A2-2:** aplicabilidade integral **[não verificado]**. O leitor indireto já existe (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`), mas não localizei a linha LEI-AKITA específica do pouso VETO. A omissão de `reabrir/recusa_da_reabertura` na origem não pode ser retroagida automaticamente: o veto está em `_renascer_por_premissa_viva` (`app/chamados/models.py:778`), enquanto `reabrir` consulta outras recusas (`app/chamados/models.py:1119`) e escreve o estado (`app/chamados/models.py:1158`). As quatro respostas enumeram o leitor indireto, mas nenhuma acusa documentação desatualizada: **NAO_VIU**, pela leitura estrita; **EM_PARTE** somente se “tema” incluir censo dos mesmos leitores.

**A2-3:** **existia** quanto ao caminho: o cartório passa vínculo possivelmente nulo ao juiz e inclui seu resultado na impressão (`app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`). Ausência de teste próprio e sobrevivência efetiva do mutante: dadas pelo anexo, **[não verificado]** por execução. As quatro respostas: **EM_PARTE**. Todas explicam mudança da impressão com `ec=None`, mas os mutantes de teste que apresentam dizem respeito à **porta**, não à impressão do cartório. Exemplo par1/A: “`ec=None` muda o cartório fora da porta”; par1/B: “impressão ganha `veto:VETO_DEGENERADA`”; par2/A: “passa a incluir veto na impressão”; par2/B: “passa a acrescentar veto à impressão”.

**A2-5:** **existia**: `recusado_veto` é incrementado por exclusão de absolvição da célula, sem consultar a causa concreta da recusa (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:177`).

- par1/A: **EM_PARTE** — “Passa a recusar no dry/apply e contabilizar `recusado_veto`”, citando `:174`; não acusa eliminação.
- par1/B: **EM_PARTE** — “apply contabiliza em `:174`”; não acusa eliminação.
- par2/A: **EM_PARTE** — “Deixa de emitir e contabiliza recusa por veto”, citando `:174`; não acusa eliminação.
- par2/B: **NAO_VIU** — aborda nova recusa e dry, sem tratar a classificação por eliminação.

**A2-7:** **existia**, confirmado expressamente pela docstring que admite ausência de trilha própria (`app/ponto/services/furos_vetados.py:82`) e pela ação com motivo fixo (`app/chamados/views_arquivar.py:75`). As quatro respostas: **VIU**.

- par1/A: “falta mostrar e registrar o veto”, com `app/chamados/views_arquivar.py:75`.
- par1/B: “falta mostrar o veto e registrar o motivo específico da exceção”, com `app/chamados/views_arquivar.py:75`.
- par2/A: “falta mostrar o veto e registrar sua admissão”, com `app/chamados/views_arquivar.py:75`.
- par2/B: “A exceção humana não entrega aviso e trilha do veto”, com `app/chamados/views_arquivar.py:75`.

## 30/10 17:33 · sessao 05c0c9d21c2d · - · - · origem=Ronald (tmux)

NOVOS da par2, usando somente as afirmações e referências das respostas.

A — achados distintos:

- `app/ponto/management/commands/reconciliar_grade.py:522` — Grade chama `reabrir()` depois de o renascimento ter sido vetado e `abrir()` devolver o encerrado.
- `app/ponto/views.py:280` — `entrada_bloqueada_turno_aberto` automático nasce sem dia identificável e passa sem julgamento do veto.
- `app/chamados/services/disputa_emissao.py:1606` — Tratamento automático de respostas pendentes reabre pai diretamente, sem veto.
- `app/ponto/management/commands/processar_alertas_avancados.py:121` — Vínculo presente e suspeito antes admitido passa a ter emissão recusada, contrariando DIFF zero.
- `app/ponto/services/furos_vetados.py:98` — Vigia acusa como violação a reabertura humana autorizada.
- `app/ponto/management/commands/furos_vetados_por_regua.py:32` — Conjunto contendo `None` publica um vínculo inexistente.
- `bin/placar_code.sh:209` — Placar descarta o novo contador `chamados_vivos_em_dia_vetado`.
- `app/ponto/management/commands/emitir_furo_retroativo.py:143` — Comando incrementa `emitidos` mesmo quando o detector recusa o nascimento.
- `app/chamados/tests/test_o261_veto_na_porta.py:98` — AST não propaga alias transitivo do modelo.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não reconhece criação por alias do módulo.
- `app/chamados/tests/test_o261_veto_na_porta.py:28` — AST não detecta nascimento por construtor seguido de `save()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não acompanha alias do manager usado em `bulk_create`.
- `app/chamados/tests/test_o261_veto_na_porta.py:143` — AST não reconhece vivificação por `update(**campos)` com dicionário em variável.
- `app/chamados/tests/test_o261_veto_na_porta.py:166` — Detecção de cópia externa não reconhece pedido à porta por `ch.reabrir()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:302` — EC-NONE é testado diretamente no juiz, sem prova pela porta.
- `app/chamados/tests/test_o261_veto_na_porta.py:269` — Faltam casos funcionais de veto para `batida_ausente` e `intervalo_pendente`.
- `app/chamados/tests/test_o261_veto_na_porta.py:345` — Teste do contador não cobre exceção humana nem nascimento sem dia; pode ser consolidado com os dois defeitos correspondentes acima.

B — achados distintos:

- `app/chamados/services/disputa_emissao.py:1606` — Reabertura automática do pai no tratamento de resposta pendente não consulta o veto.
- `app/ponto/management/commands/reconciliar_grade.py:522` — Grade supera recusa da porta ao chamar `reabrir()` diretamente.
- `app/ponto/views.py:280` — Emissor automático não fornece dia, permitindo nascimento sem julgamento do veto.
- `app/ponto/management/commands/processar_alertas_avancados.py:121` — Emissão com vínculo presente e suspeito muda, contrariando DIFF zero.
- `app/ponto/management/commands/furos_vetados_por_regua.py:32` — `None` no conjunto é contado como vínculo existente.
- `app/ponto/services/furos_vetados.py:98` — Ato humano autorizado aparece como violação no vigia de esperado zero.
- `app/chamados/tests/test_o261_veto_na_porta.py:100` — AST não propaga alias encadeado do modelo.
- `app/chamados/tests/test_o261_veto_na_porta.py:165` — AST não reconhece modelo acessado por alias de módulo.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não acompanha manager atribuído a variável para `create()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:28` — AST não detecta construtor seguido de `save()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST perde origem de queryset atribuído a variável para `bulk_create()`; pode ser consolidado com alias do manager.
- `app/chamados/tests/test_o261_veto_na_porta.py:143` — AST não resolve variável de argumentos de `update(**kwargs)`.
- `app/chamados/tests/test_o261_veto_na_porta.py:166` — AST não reconhece instância nem contabiliza `ch.reabrir()` como escrita.
- `app/chamados/tests/test_o261_veto_na_porta.py:194` — Allowlist deduplica por função e permite acrescentar outra escrita dentro de função já autorizada.
- `app/chamados/tests/test_o261_veto_na_porta.py:302` — EC-NONE não é exercitado pela porta; mutante que libera vínculo ausente sobrevive aos cenários escritos.
- `app/chamados/tests/test_o261_veto_na_porta.py:269` — Faltam casos de porta para `batida_ausente` e `intervalo_pendente`.
- `app/chamados/tests/test_o261_veto_na_porta.py:295` — `criar` carece de controle positivo equivalente; a resposta não afirma vacuidade comprovada.

Não incluí como NOVO a ausência de aviso/trilha humana, coberta por A2-7, nem a alteração `ec=None` em cartório/furos/c2, coberta por m1. O caso humano testar somente `True` (`app/chamados/tests/test_o261_veto_na_porta.py:336`) reforça A2-7 e pode permanecer associado a esse item.

## 30/10 17:33 · sessao e66cc836b8db · - · - · origem=Ronald (tmux)

Novos claros de **par1/A**, com referências citadas pela própria resposta:

- `app/ponto/views.py:280` — Emissão automática de `entrada_bloqueada_turno_aberto` omite dia forte, passa sem veto e fica apenas em `sem_dia` no vigia.
- `app/ponto/management/commands/reconciliar_grade.py:522` — Grade chama `reabrir()` no encerrado devolvido após recusa do renascimento e supera o veto.
- `app/chamados/services/disputa_emissao.py:1606` — Tratamento automático de respostas pendentes reabre o pai diretamente, sem veto.
- `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` — Reparador supervisionado reabre pai do universo sem consultar o veto.
- `app/ponto/management/commands/processar_alertas_turno.py:317` — Renascimento recusado devolve encerrado; leitor prossegue com alerta, push e justificativa pendente.
- `app/ponto/services/furos_vetados.py:98` — Contador acusa ato humano autorizado como achado, sem distinguir vazamento automático de exceção humana.
- `app/ponto/management/commands/furos_vetados_por_regua.py:32` — Conjunto de vínculos inclui `None` e publica um vínculo inexistente.
- `app/chamados/tests/test_o261_veto_na_porta.py:100` — AST não acompanha alias transitivo da classe (`Alias = Ch`).
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não reconhece escrita via módulo importado (`models.ChamadoColaborador`).
- `app/chamados/tests/test_o261_veto_na_porta.py:163` — AST não detecta nascimento por construtor seguido de `save()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não acompanha alias de manager usado em `bulk_create`.
- `app/chamados/tests/test_o261_veto_na_porta.py:137` — AST não resolve variável de kwargs que vivifica via `update(**campos)`.
- `app/chamados/tests/test_o261_veto_na_porta.py:170` — AST não contabiliza `ch.reabrir()` como escrita.
- `app/chamados/tests/test_o261_veto_na_porta.py:152` — Varredura deixa escritas no topo do módulo fora da fiscalização.
- `app/chamados/tests/test_o261_veto_na_porta.py:131` — Varredura pula lambdas.
- `app/chamados/tests/test_o261_veto_na_porta.py:208` — Teto histórico fixo permite aumentar novamente a allowlist após uma redução.
- `app/chamados/tests/test_o261_veto_na_porta.py:194` — Deduplicação por função permite acrescentar escrita em função já permitida sem alterar contagem.
- `app/chamados/tests/test_o261_veto_na_porta.py:302` — EC-NONE é testado somente no juiz; mutante que libera vínculo ausente na porta sobrevive.
- `app/chamados/tests/test_o261_veto_na_porta.py:269` — Casos funcionais não exercitam veto para `batida_ausente` nem `intervalo_pendente`.
- `app/chamados/tests/test_o261_veto_na_porta.py:313` — Teste aprova emissão sem dia, consolidando o escape da invariante.

Novos claros de **par1/B**, com referências citadas pela própria resposta:

- `app/chamados/services/disputa_emissao.py:1606` — Signal pode provocar reabertura automática direta do pai, sem veto.
- `app/ponto/management/commands/reconciliar_grade.py:522` — Grade reabre diretamente o encerrado devolvido após recusa do renascimento.
- `app/ponto/views.py:280` — Emissão automática omite dia forte, passa sem julgamento e fica fora dos achados do vigia.
- `app/chamados/management/commands/reabrir_orfas_enterradas.py:161` — Recuperação supervisionada reabre pai do universo sem veto.
- `app/ponto/management/commands/furos_vetados_por_regua.py:49` — Placar descarta o novo contador `chamados_vivos_em_dia_vetado`.
- `app/ponto/management/commands/furos_vetados_por_regua.py:32` — `None` entra no conjunto e produz contagem de vínculo inexistente.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não resolve alias de módulo em `m.ChamadoColaborador.objects.create`.
- `app/chamados/tests/test_o261_veto_na_porta.py:100` — AST não acompanha alias transitivo da classe.
- `app/chamados/tests/test_o261_veto_na_porta.py:170` — AST não detecta construtor seguido de `save()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:168` — AST não acompanha alias de manager em `bulk_create`.
- `app/chamados/tests/test_o261_veto_na_porta.py:137` — AST não resolve variável passada a `update(**kw)`.
- `app/chamados/tests/test_o261_veto_na_porta.py:166` — Detecção de cópia de veto não reconhece pedido à porta feito por instância via `ch.reabrir()`.
- `app/chamados/tests/test_o261_veto_na_porta.py:209` — Teto cardinal não assegura redução monotônica da allowlist.
- `app/chamados/tests/test_o261_veto_na_porta.py:194` — Deduplicação por função deixa acrescentar escrita em função já permitida.
- `app/chamados/tests/test_o261_veto_na_porta.py:302` — EC-NONE fica restrito ao teste do juiz, sem exercício na porta.
- `app/chamados/tests/test_o261_veto_na_porta.py:269` — Ausência de casos de porta para `batida_ausente` e `intervalo_pendente`.
- `app/chamados/tests/test_o261_veto_na_porta.py:295` — Caso negativo de `criar` não tem controle positivo correspondente nem exige trilha do veto.
- `app/chamados/tests/test_o261_veto_na_porta.py:313` — Teste exige liberação sem dia e não demonstra o veto no emissor real.

Casos limítrofes para consolidação, por possível sobreposição temática com **m1**:

- **A:** `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` e `app/ponto/management/commands/processar_alertas_avancados.py:121` — Vínculo presente e suspeito passa a recusar turnos antes emitidos; cenário distinto da regressão `ec=None` descrita em m1.
- **B:** `app/ponto/management/commands/processar_alertas_avancados.py:121` — Mesmo cenário com vínculo presente e suspeito.
- **B:** `app/ponto/management/commands/processar_alertas_turno.py:100` — Fim previsto materializado permite chegar à emissão mesmo sem vínculo; a nova porta altera esse comportamento. É outro consumidor de `ec=None`, não explicitado no m1.

Não contei falta de aviso/trilha humana como novo, pois corresponde a **A2-7**. Não contei a mudança da impressão do cartório e dos totais de `furos_sem_cobranca`/c2, pois corresponde a **m1**. A lacuna EC-NONE **na porta** é distinta da lacuna de teste **no cartório** de **A2-3**. [nao verificado]

## 30/10 17:30 · sessao e65e8de89c72 · CODEX-CONTEXTO-DA-CASA-AB · verificador · origem=Code

**Veredito: reprova o pouso A.** Há reabertura automática que não consulta o veto, nascimento automático que atravessa a porta sem aplicá-lo e mutantes de escrita que o selo AST aceita. (`app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:893`; `app/chamados/tests/test_o261_veto_na_porta.py:168`)

**(a) Leitores cujo comportamento muda**

| Leitor | Mudança demonstrável |
|---|---|
| `app/ponto/services/furos_vetados.py:57` | Célula `furo` com `ec=None` passa a aparecer entre os vetados, com `vinculo=None`. |
| `app/ponto/services/cartorio.py:1013` | `ec=None` passa a acrescentar veto à impressão; uma célula antes estável pode deixar de ser pulada e ser rejulgada. (`app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`) |
| `app/ponto/services/furos_sem_cobranca.py:96` | Dia sem vínculo passa à porta `veto`, sai do total e deixa de ser rejulgado por `cobrar`. Afeta também os comandos de medida/cobrança. (`app/ponto/services/furos_sem_cobranca.py:109`; `app/ponto/services/furos_sem_cobranca.py:125`; `app/ponto/management/commands/furos_sem_cobranca_viva.py:14`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`) |
| `app/chamados/reconciliador.py:854` | O veto externo existente passa a recusar `ec=None`, antes de pedir a porta. |
| `app/chamados/models.py:896` | Novo leitor: aplica o juiz aos nascimentos e renascimentos que tenham dia identificado. |
| `app/ponto/services/furos_vetados.py:98` | Novo leitor: classifica chamados vivos pelo veto, incluindo os admitidos por ato humano. |
| `abrir`: `detectar_ausencias.py:688`, `reconciliar_grade.py:499`, `chamados/reconciliador.py:862` | Passam a receber a recusa nova da porta; o diff acrescenta tratamento de `None`. A grade ainda pode contornar a recusa de renascimento, descrita abaixo. |
| `abrir`: `app/ponto/management/commands/processar_alertas_turno.py:335`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Nascimentos antes admitidos podem passar a ser recusados pela régua. O dry do retroativo também muda. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`) |
| `criar`: `app/ponto/management/commands/processar_alertas_avancados.py:121` | Turno N5 elegível, com vínculo presente mas régua suspeita, passa a devolver `None`. |
| `_renascer_por_premissa_viva`: `app/chamados/models.py:686` | O dedup pode devolver o existente ainda encerrado após o veto ao renascimento. |
| `_renascer_por_premissa_viva`: `app/chamados/services/resposta_tardia.py:45` | Resposta que antes reabria o chamado passa ao tratamento de resposta tardia, com nota e, havendo conteúdo, pauta. (`app/chamados/services/resposta_tardia.py:52`; `app/chamados/services/resposta_tardia.py:58`) |

Desses leitores saem dois achados adicionais:

1. **O “DIFF zero por construção” não se sustenta.** Contra-exemplo: `TurnoMaterializado` aberto, entrada existente, `dt_fim_previsto` preenchido, vínculo posteriormente encerrado, instante no `pos_20`, sem ausência nem alerta anterior. O emissor só exige vínculo no ramo em que falta fim previsto; com o fim materializado, alcançava o nascimento. Agora a porta recebe `ec=None` e recusa. O veto novo cumpre o corte final, mas altera comportamento neste cenário. (`app/ponto/management/commands/processar_alertas_turno.py:100`; `app/ponto/management/commands/processar_alertas_turno.py:134`; `app/ponto/management/commands/processar_alertas_turno.py:141`; `app/ponto/services/furos_vetados.py:32`)

   Outro contra-exemplo: vínculo válido, régua suspeita, turno N5 elegível e célula que não absolve → antes `processar_alertas_avancados` criava; depois `criar` recusa. Quantidade desses casos em produção: **[não verificado]**. (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/models.py:838`)

2. **O contador passa a contar um vínculo inexistente.** Entrada: três células `furo`, todas sem vínculo → o serviço devolve três achados com `vinculo=None`; o comando forma `{None}` e imprime `furos_vetados_por_regua_vinculos=1`, embora existam zero vínculos. (`app/ponto/services/furos_vetados.py:61`; `app/ponto/management/commands/furos_vetados_por_regua.py:32`; `app/ponto/management/commands/furos_vetados_por_regua.py:33`)

**(b) Caminhos que escapam da proteção**

1. **Reabertura automática direta no tratamento de respostas pendentes.** O tratamento reabre o pai por `_pai.reabrir()`, sem passar por `_renascer_por_premissa_viva`. Esse tratamento é acionado pelo signal de encerramento e pelo reconciliador. (`app/chamados/services/disputa_emissao.py:1598`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/signals.py:92`; `app/chamados/reconciliador.py:538`)

   Contra-exemplo: pai `entrada_bloqueada_turno_aberto`, resolvido, dia conhecido sob régua suspeita, disputa aberta e irmã respondida aguardando consumo; nenhuma outra recusa de reabertura → grava `em_analise`. O veto novo está apenas no renascimento; `reabrir` consulta outras recusas e escreve o estado diretamente. (`app/chamados/models.py:778`; `app/chamados/models.py:1119`; `app/chamados/models.py:1158`)

2. **A grade pode superar uma recusa da própria porta.** `abrir` descarta o resultado booleano do renascimento e devolve o existente; a grade, ao recebê-lo encerrado, chama `reabrir()` novamente. O tratamento novo de `None` não cobre esse retorno. (`app/chamados/models.py:685`; `app/chamados/models.py:688`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`)

   Contra-exemplo concreto: chamado `batida_ausente` com JSON `data_turno=D`, mas célula ligada em `D−1`; vínculo saudável em D e suspeito em D−1; premissa ainda viva. A grade aceita D, o dedup encontra pelo JSON, o veto usa primeiro a célula e recusa D−1; a grade então reabre diretamente. Ocorrência desse cadastro em produção: **[não verificado]**. (`app/ponto/management/commands/reconciliar_grade.py:487`; `app/ponto/management/commands/reconciliar_grade.py:501`; `app/chamados/juizes.py:89`)

3. **Nascimento automático sem dia atravessa a porta sem consultar o juiz.** `bater_ponto` cria `entrada_bloqueada_turno_aberto` com somente `pendentes` e `total`, embora a triagem disponha das datas das entradas. Sem dia forte, `_veto_da_regua` retorna antes de consultar vínculo ou veto. (`app/ponto/views.py:280`; `app/ponto/views.py:291`; `app/ponto/services/triagem_batida.py:185`; `app/chamados/models.py:892`)

   Contra-exemplo: colaborador sem vínculo, entrada antiga aberta há mais de 14 horas, POST de nova entrada e nenhum chamado vivo → nasce chamado do universo. O vigia o coloca em `sem_dia`, fora dos achados de esperado zero; o teste novo aprova expressamente essa passagem. (`app/ponto/views.py:275`; `app/ponto/services/furos_vetados.py:94`; `app/chamados/tests/test_o261_veto_na_porta.py:313`)

4. **A exceção humana não entrega aviso e trilha do veto.** Desarquivar pode reabrir e responder sucesso sem consultar o veto, exigindo apenas as permissões existentes; registra um motivo fixo. A reabertura administrativa possui justificativa, mas também não registra o veto admitido. (`app/chamados/views_arquivar.py:68`; `app/chamados/views_arquivar.py:75`; `app/chamados/views_arquivar.py:80`; `app/chamados/services/acoes_disputa.py:245`; `app/chamados/services/acoes_disputa.py:270`)

   Contra-exemplo: admin desarquiva chamado de dia sob régua suspeita, com premissa viva → o chamado volta sem aviso do veto e sem motivo específico para admiti-lo. Depois aparece como violação no vigia de esperado zero, embora o ato humano esteja autorizado; o próprio serviço declara essa ausência de trilha. (`app/ponto/services/furos_vetados.py:82`; `app/ponto/services/furos_vetados.py:98`; `app/docs/CORTES.md:401`)

Nos demais `reabrir` diretos examinados, `ancora_limbo`, `sem_previsao_escala` e `ausencia_colab` ficam fora do universo; **sem achado nesses caminhos**. O comando de órfãs pode alcançar pai do universo, mas é descrito como operação manual de pouso, não como cron automático. (`app/ponto/management/commands/detectar_ausencias.py:273`; `app/chamados/services/adesao.py:75`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:29`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:1`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`)

**(c) O selo AST aceita mutantes plausíveis**

Nos exemplos abaixo, as operações ficam dentro de uma função de produção. O varredor reconhece raízes e alguns imports, mas não acompanha os valores atribuídos a aliases, instâncias ou variáveis de argumentos. (`app/chamados/tests/test_o261_veto_na_porta.py:87`; `app/chamados/tests/test_o261_veto_na_porta.py:147`)

| Mutante | Resultado do varredor |
|---|---|
| Importar como `Ch`; fazer `CC=Ch`; chamar `CC.objects.create(...)` | **Passa:** não propaga o alias encadeado. (`:100`) |
| `from . import models as m`; chamar `m.ChamadoColaborador.objects.create(...)` | **Passa:** a raiz é `m`. (`:165`; `:168`) |
| `manager=ChamadoColaborador.objects`; chamar `manager.create(...)` | **Passa:** não acompanha o manager. (`:168`) |
| `ch=ChamadoColaborador(...)`; chamar `ch.save()` | **Passa:** construtor e `save` não são classificados como nascimento. (`:28`; `:170`) |
| `qs=ChamadoColaborador.objects.all()`; chamar `qs.bulk_create(...)` | **Passa:** perde a origem do queryset. (`:168`) |
| `kwargs={'status_local':'aberto'}`; chamar `ChamadoColaborador.objects.filter(...).update(**kwargs)` | **Passa:** procura constantes dentro da chamada, sem resolver `kwargs`. (`:140`; `:143`) |
| Receber `ch` e chamar `ch.reabrir()` | **Passa:** não reconhece a instância nem contabiliza `reabrir` como escrita. (`:166`; `:170`) |

As referências abreviadas da tabela são de `app/chamados/tests/test_o261_veto_na_porta.py`.

O contra-exemplo de construtor seguido de `save()` nasce aberto pelo default do modelo. Também escapa do selo vizinho, que procura atribuição explícita a `.status_local` ou `update(status_local=...)`. (`app/chamados/models.py:46`; `app/chamados/tests/test_contract_status_nucleo.py:7`; `app/chamados/tests/test_contract_status_nucleo.py:23`)

Há dois controles relevantes: **sem achado** para import relativo direto — `from .models import ChamadoColaborador as Ch` —, `bulk_create` direto pelo modelo e `update(**{'status_local':'aberto'})` inline; esses formatos são detectados. (`app/chamados/tests/test_o261_veto_na_porta.py:92`; `app/chamados/tests/test_o261_veto_na_porta.py:144`; `app/chamados/tests/test_o261_veto_na_porta.py:170`)

A allowlist também não limita sítios dentro de uma função: acrescentar outra escrita a `0038...::desfazer` mantém o mesmo conjunto de nomes e passa. O varredor descarta a multiplicidade antes da comparação; o teto conta entradas da allowlist. (`app/chamados/tests/test_o261_veto_na_porta.py:41`; `app/chamados/tests/test_o261_veto_na_porta.py:194`; `app/chamados/tests/test_o261_veto_na_porta.py:210`)

**(d) Vacuidade e cobertura dos testes da porta** [nao verificado]

- **EC-NONE não é exercitado pela porta.** O teste passa `None` diretamente ao juiz, enquanto a fixture mantém vínculo válido. Mutante sobrevivente: `_veto_da_regua` faz `if ec is None: return None`; o teste direto do juiz continua passando e os cenários da porta não distinguem a retirada dessa proteção. (`app/chamados/tests/test_o261_veto_na_porta.py:266`; `app/chamados/tests/test_o261_veto_na_porta.py:302`; `app/chamados/models.py:896`)
- **Dois módulos ficam sem exercício:** `batida_ausente` e `intervalo_pendente`. Mutante que os exclui do veto não encontra cenário correspondente nessa bateria; os casos usam os três módulos de turno. (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:269`; `app/chamados/tests/test_o261_veto_na_porta.py:298`; `app/chamados/tests/test_o261_veto_na_porta.py:324`)
- **O caso humano só verifica `True`.** Não exige aviso, ator, motivo ou registro do veto admitido; portanto certifica apenas a passagem, não o contrato completo de 08:2x. (`app/chamados/tests/test_o261_veto_na_porta.py:336`; `app/docs/CORTES.md:401`)
- **Sem achado comprovado de recusa por guarda anterior tornando vácuo o teste de renascimento:** existe controle positivo com o mesmo módulo. Para `criar`, falta controle positivo equivalente, mas afirmar que o teste passaria no código antigo por outra recusa é **[não verificado]**. (`app/chamados/tests/test_o261_veto_na_porta.py:295`; `app/chamados/tests/test_o261_veto_na_porta.py:338`)

**Sem achado adicional comprovado** de juiz novo sem corte: o registro possui autorização literal. As três cópias externas estão explicitamente admitidas no pouso A, para retirada no B. (`app/core/juizes.py:248`; `app/docs/CORTES.md:401`; `app/chamados/tests/test_o261_veto_na_porta.py:46`)

Ordem de trava, relógio novo fora da borda, derivação paralela adicional e custo excessivo no request: **sem achado comprovado nesta revisão**. Execução da suíte Django, cumprimento dos pré-requisitos de pouso, contagens e impacto em produção: **[não verificado]**. [nao verificado]

## 30/10 17:30 · sessao f9acecce9cd2 · CODEX-CONTEXTO-DA-CASA-AB · verificador · origem=Code

**Reprovo o diff como pouso A:** ele muda comportamento, deixa reaberturas automáticas fora do veto e não entrega o selo que garante passagem pela porta. A exigência de comportamento igual/DIFF zero permanece registrada em `app/docs/CORTES.md:401`; os desvios estão abaixo. Impacto quantitativo na frota: **[não verificado]**.

**1. O pouso A já altera emissão e leitores existentes.** [nao verificado]

- **Turnos antes emitidos passam a ser recusados.** Entrada: turno aberto há 30 horas, vínculo existente com horários válidos, régua suspeita, sem ausência, sem irmão e célula que não absolve. O retroativo passa pelas verificações existentes, mas agora a porta recusa e o dry imprime `RECUSADO veto`. Antes desse diff, não havia essa recusa por régua nesse caminho. O N5 dos alertas avançados sofre a mesma mudança. Isso contradiz “comportamento igual” por construção. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:118`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`, `app/chamados/models.py:731`, `app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/ponto/management/commands/processar_alertas_avancados.py:141`.

- **`ec=None` muda o cartório fora da porta.** Entrada: célula já julgada, impressão estável, colaborador com batidas, vínculo ausente. O novo `VETO_DEGENERADA` entra na impressão; a impressão anterior deixa de coincidir e o cartório pode rejulgar onde antes pulava. Portanto, a alteração não está confinada ao nascimento/reabertura. `app/ponto/services/furos_vetados.py:32`, `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:329`, `app/ponto/services/cartorio.py:1092`.

**2. Há escapes automáticos da invariante — item (b).**

- **Nascimento sem dia passa pelo veto.** Entrada: POST de entrada, turno antigo aberto há pelo menos 14 horas, nenhum chamado vivo correspondente e régua suspeita. `bater_ponto` emite `entrada_bloqueada_turno_aberto` com apenas `pendentes` e `total`; sem dia forte, `_veto_da_regua` retorna `None` e o chamado nasce. Passa por `criar`, mas não pela decisão efetiva do veto. O contador também o deixa fora dos achados, somando apenas `sem_dia`. `app/ponto/views.py:275`, `app/ponto/views.py:280`, `app/ponto/views.py:291`, `app/chamados/models.py:892`, `app/ponto/services/furos_vetados.py:94`.

- **A grade pode desfazer uma recusa da porta.** `abrir` devolve o existente encerrado mesmo quando `_renascer_por_premissa_viva` retorna `False`; a proteção nova da grade só trata `None`, depois chama `ch.reabrir()` diretamente. Contraexemplo: chamado resolvido com JSON em D1 e célula ligada em D2; D1 possui vínculo bom e passa pelo veto externo, D2 possui vínculo suspeito e é o dia forte usado pela porta. A porta recusa; a grade recebe o encerrado e o reabre, satisfeitas as demais guardas. O mesmo desvio pode ocorrer com mudança de vínculo entre a carga do lote e a consulta da porta. Existência desses estados em produção: **[não verificado]**. `app/chamados/models.py:685`, `app/chamados/juizes.py:89`, `app/ponto/management/commands/reconciliar_grade.py:487`, `app/ponto/management/commands/reconciliar_grade.py:519`, `app/ponto/management/commands/reconciliar_grade.py:522`, `app/chamados/models.py:1119`.

- **Há outro automático genérico por `reabrir` direto.** Durante a resolução de perguntas, uma disputa aberta com pai não vivo e resposta aguardando consumo executa `_pai.reabrir()`, sem filtro de módulo. Entrada admitida pelo modelo: pai `entrada_bloqueada_turno_aberto` resolvido, com dia vetado, disputa aberta e resposta creditada ao pai, sem outra recusa → reabre sem consultar o veto. Ocorrência dessa associação na frota: **[não verificado]**. `app/chamados/services/disputa_emissao.py:1598`, `app/chamados/services/disputa_emissao.py:1606`, `app/chamados/models.py:1393`, `app/chamados/models.py:1092`.

- O reparador `reabrir_orfas_enterradas` também usa `recusa_da_reabertura` e `pai.reabrir()` sem veto. É um desvio mecânico adicional; por exigir apply explícito, não o classifico automaticamente como cron ou como exceção humana plenamente atendida. `app/chamados/management/commands/reabrir_orfas_enterradas.py:105`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:121`, `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`.

**3. Um leitor continua cobrando depois de o renascimento ser vetado.**

Entrada: `turno_aberto` resolvido para o dia, régua agora suspeita e `pos_20` ainda não registrado. A porta impede o renascimento, mas devolve o chamado encerrado. `_abrir_chamado_critico` devolve esse objeto; `_marco_critico` testa somente `None` e segue registrando alerta, enviando push e criando justificativa pendente ligada ao encerrado. A adaptação dos leitores ficou incompleta. `app/chamados/models.py:686`, `app/ponto/management/commands/processar_alertas_turno.py:387`, `app/ponto/management/commands/processar_alertas_turno.py:317`, `app/ponto/management/commands/processar_alertas_turno.py:319`, `app/ponto/management/commands/processar_alertas_turno.py:244`, `app/ponto/management/commands/processar_alertas_turno.py:391`.

**4. A exceção humana passa, mas falta mostrar e registrar o veto.**

Entrada: admin desarquiva chamado de dia suspeito, com as demais guardas satisfeitas. A ação passa por `reabrir`, levanta o silêncio e registra texto genérico; não consulta nem registra o motivo do veto. Reabrir disputa administrativa também registra justificativa humana, sem identificar o veto. Isso deixa incompleto o aval 08:2x registrado no corte. `app/chamados/views_arquivar.py:68`, `app/chamados/views_arquivar.py:75`, `app/chamados/views_arquivar.py:79`, `app/chamados/services/acoes_disputa.py:245`, `app/chamados/services/acoes_disputa.py:266`, `app/chamados/services/acoes_disputa.py:270`, `app/docs/CORTES.md:401`.

O novo contador inclui essa exceção humana autorizada como achado e admite expressamente adiar sua trilha própria para B. Assim, um achado não distingue vazamento automático de ato humano permitido. Contraexemplo: admin reabre um dia vetado → o contador sobe mesmo sem nascimento automático indevido. `app/ponto/services/furos_vetados.py:82`, `app/ponto/services/furos_vetados.py:98`, `app/ponto/services/furos_vetados.py:100`.

Não sustento um crash no formulário de abertura manual: ele filtra pelos tipos abríveis, e os cinco módulos não estão oferecidos nessa seleção. `app/chamados/services/abertura.py:149`, `app/chamados/catalogo/modulos.py:105`, `app/chamados/catalogo/modulos.py:182`, `app/chamados/catalogo/modulos.py:306`.

**5. Censo dos leitores com mudança — item (a).**

| Leitor de `motivo_do_veto` | Entrada → mudança |
|---|---|
| `furos_vetados_por_regua` | Célula `furo`, não desligado, sem vínculo → antes não aparecia; agora entra como `VETO_DEGENERADA`, com `vinculo=None`. `app/ponto/services/furos_vetados.py:53`, `app/ponto/services/furos_vetados.py:57`, `app/ponto/services/furos_vetados.py:61`. |
| Cartório, `_veto_do_dia` | Vínculo ausente → muda impressão e pode retirar célula do caminho de pular julgamento. `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1092`. |
| `furos_sem_cobranca.medir` | Dia elegível sem vínculo e sem cobrança → passa à porta `veto`, sai do total e deixa de ser enviado por `cobrar`. `app/ponto/services/furos_sem_cobranca.py:96`, `app/ponto/services/furos_sem_cobranca.py:109`, `app/ponto/services/furos_sem_cobranca.py:125`. |
| `reemitir_chamado_do_dia` | Sem chamado vivo e sem vínculo, satisfeitos os demais gates → agora retorna `False` antes de chamar a porta. `app/chamados/reconciliador.py:852`, `app/chamados/reconciliador.py:857`. |
| Novo `_veto_da_regua` | Módulo do universo e dia forte vetado → bloqueia nascimento/renascimento. `app/chamados/models.py:888`, `app/chamados/models.py:896`. |
| Novo `chamados_vivos_em_dia_vetado` | Chamado vivo com dia vetado → entra nos achados, incluindo o caso sem vínculo. `app/ponto/services/furos_vetados.py:93`, `app/ponto/services/furos_vetados.py:98`. |

| Leitor das portas alteradas | Mudança |
|---|---|
| Detector de ausências → `abrir` | Divergência entre vínculo entregue pelo chamador e vínculo consultado pela porta pode gerar nova recusa; agora retorna `VETO_NA_PORTA` ou encerrado que não renasceu. `app/ponto/management/commands/detectar_ausencias.py:688`, `app/ponto/management/commands/detectar_ausencias.py:729`, `app/ponto/management/commands/detectar_ausencias.py:732`. |
| Grade → `abrir` | Passa a tratar nascimento recusado, mas conserva a reabertura direta descrita acima. `app/ponto/management/commands/reconciliar_grade.py:499`, `app/ponto/management/commands/reconciliar_grade.py:519`. |
| c2 → `abrir` | Nova recusa retorna `False`, sem costurar a pergunta. `app/chamados/reconciliador.py:862`, `app/chamados/reconciliador.py:885`. |
| Alertas de turno → `abrir` | Novo nascimento vetado interrompe `pos_20`; dedup encerrado vetado ainda deixa alerta/justificativa prosseguirem. `app/ponto/management/commands/processar_alertas_turno.py:335`, `app/ponto/management/commands/processar_alertas_turno.py:317`. |
| Retroativo → `abrir` | Passa a recusar no dry/apply e contabilizar `recusado_veto`. `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`. |
| Alertas avançados N5 → `criar` | Régua vetada → recebe `None` e abandona os efeitos posteriores. `app/ponto/management/commands/processar_alertas_avancados.py:121`, `app/ponto/management/commands/processar_alertas_avancados.py:141`. |
| `abrir` → `_renascer_por_premissa_viva` | Recusa nova mantém o encerrado e devolve `(existente, False)`. `app/chamados/models.py:685`, `app/chamados/models.py:688`. |
| `resposta_tardia.receber` → `_renascer_por_premissa_viva` | Célula acusa, mas dia veta → resposta vira nota/trilha; texto livre também gera Pauta, em vez de renascer. A view retorna antes do processamento seguinte de hora/escape. `app/chamados/services/resposta_tardia.py:45`, `app/chamados/services/resposta_tardia.py:52`, `app/chamados/services/resposta_tardia.py:58`, `app/chamados/views.py:1053`. |

Os efeitos indiretos chegam aos comandos `furos_vetados_por_regua`, `furos_sem_cobranca_viva`, `cobrar_furos_sem_cobranca` e aos números consumidos pelo placar. `desfazer_corroboracao_regua` lê a lista ampliada, mas filtra por vínculo existente: **sem achado de mudança efetiva nesse leitor**. `app/ponto/management/commands/furos_vetados_por_regua.py:27`, `app/ponto/management/commands/furos_sem_cobranca_viva.py:14`, `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`, `bin/placar_code.sh:209`, `bin/placar_code.sh:277`, `app/escala/management/commands/desfazer_corroboracao_regua.py:38`.

Há ainda um erro concreto nesse leitor indireto: três células sem vínculo produzem `vinculo=None`; o comando adiciona `None` ao conjunto e imprime **um vínculo**, embora existam zero vínculos associados aos achados. `app/ponto/services/furos_vetados.py:61`, `app/ponto/management/commands/furos_vetados_por_regua.py:32`, `app/ponto/management/commands/furos_vetados_por_regua.py:33`.

**6. O selo deixa passar mutantes plausíveis — item (c).**

Nos exemplos abaixo, a emissão pode usar `turno_aberto`, dia vetado e colaborador válido; o varredor perde a escrita pelas regras indicadas. `app/chamados/tests/test_o261_veto_na_porta.py:87`, `app/chamados/tests/test_o261_veto_na_porta.py:147`.

| Mutante | Resultado e causa |
|---|---|
| Importa classe como `Ch`; faz `Alias = Ch`; usa `Alias.objects.create(...)` | **Passa.** Só acompanha atribuição cujo nome original é literalmente `ChamadoColaborador`, sem propagar alias. `app/chamados/tests/test_o261_veto_na_porta.py:100`. |
| `from . import models`; usa `models.ChamadoColaborador.objects.create(...)` | **Passa.** A raiz é `models`, que não entra nos nomes do modelo. `app/chamados/tests/test_o261_veto_na_porta.py:92`, `app/chamados/tests/test_o261_veto_na_porta.py:165`, `app/chamados/tests/test_o261_veto_na_porta.py:168`. |
| `ch = ChamadoColaborador(...)`; `ch.save()` | **Passa.** Construtor e `save` não são classificados como nascimento. O status padrão já é aberto; o selo antigo de status também não fiscaliza esse nascimento. `app/chamados/tests/test_o261_veto_na_porta.py:28`, `app/chamados/tests/test_o261_veto_na_porta.py:163`, `app/chamados/models.py:46`, `app/chamados/tests/test_contract_status_nucleo.py:7`. |
| `mgr = ChamadoColaborador.objects`; `mgr.bulk_create([...])` | **Passa.** Não resolve alias do manager. `app/chamados/tests/test_o261_veto_na_porta.py:98`, `app/chamados/tests/test_o261_veto_na_porta.py:168`. |
| `campos = {'status_local': 'aberto'}`; `ChamadoColaborador.objects.filter(...).update(**campos)` | **Passa.** `_vivifica` procura keyword explícita ou literal dentro da chamada, sem seguir `campos`. `app/chamados/tests/test_o261_veto_na_porta.py:137`. |
| Busca instância e executa `ch.reabrir()` | **Passa.** `reabrir` não é classificado como escrita; a lista de pedidos à porta só auxilia a detecção de cópia de veto. `app/chamados/tests/test_o261_veto_na_porta.py:166`, `app/chamados/tests/test_o261_veto_na_porta.py:170`. |
| Escrita no topo do módulo ou dentro de lambda | **Passa.** Só corpos de funções são fiscalizados e lambdas são puladas. `app/chamados/tests/test_o261_veto_na_porta.py:109`, `app/chamados/tests/test_o261_veto_na_porta.py:131`, `app/chamados/tests/test_o261_veto_na_porta.py:152`. |

**Sem achado** nas formas diretas `from .models import ChamadoColaborador as Ch`, `ChamadoColaborador.objects.bulk_create(...)` e `update(**{'status_local': 'aberto'})`: essas formas são reconhecidas pelas regras existentes. `app/chamados/tests/test_o261_veto_na_porta.py:92`, `app/chamados/tests/test_o261_veto_na_porta.py:143`, `app/chamados/tests/test_o261_veto_na_porta.py:170`.

A “catraca” também não garante monotonicidade: depois de reduzir de cinco sítios para quatro, outro commit pode acrescentar um sítio e voltar a cinco. A igualdade usa a allowlist corrente e o único teto histórico é o cinco fixo. Além disso, converter achados em `set` permite acrescentar outra escrita dentro de uma função já permitida sem mudar a contagem. `app/chamados/tests/test_o261_veto_na_porta.py:54`, `app/chamados/tests/test_o261_veto_na_porta.py:194`, `app/chamados/tests/test_o261_veto_na_porta.py:208`.

**7. A prova da porta tem lacunas — item (d).**

- **EC-NONE não atravessa a porta.** O teste chama somente `motivo_do_veto(..., None, ...)`; a fixture mantém vínculo válido. Mutante: `_veto_da_regua` resolve o vínculo e retorna `None` quando ele estiver ausente, mantendo o juiz correto → esse teste continua satisfeito, mas nascimento e renascimento sem vínculo passam. `app/chamados/tests/test_o261_veto_na_porta.py:266`, `app/chamados/tests/test_o261_veto_na_porta.py:302`, `app/chamados/models.py:896`.

- **Dois módulos não têm exercício funcional nessa suíte.** Excluir `batida_ausente` e `intervalo_pendente` do veto não seria distinguido pelos casos apresentados, que usam os outros três módulos. `app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/tests/test_o261_veto_na_porta.py:269`, `app/chamados/tests/test_o261_veto_na_porta.py:298`, `app/chamados/tests/test_o261_veto_na_porta.py:316`, `app/chamados/tests/test_o261_veto_na_porta.py:324`.

- **O escape sem dia é aprovado pelo próprio teste.** Régua suspeita + módulo do universo + contexto sem dia → exige nascimento bem-sucedido. Isso sela a exceção implementada, não a invariante universal anunciada. `app/chamados/tests/test_o261_veto_na_porta.py:313`, `app/chamados/models.py:892`.

- **Sem achado de vacuidade total** nos testes de suspeita, renascimento e contador: há controles positivos, exigência da trilha `VETO_REGUA` e comparação do contador com/sem veto. Também não passa um varredor inteiramente cego: as allowlists não vazias e o teste de encontrar as portas o recusariam. `app/chamados/tests/test_o261_veto_na_porta.py:200`, `app/chamados/tests/test_o261_veto_na_porta.py:211`, `app/chamados/tests/test_o261_veto_na_porta.py:279`, `app/chamados/tests/test_o261_veto_na_porta.py:290`, `app/chamados/tests/test_o261_veto_na_porta.py:338`, `app/chamados/tests/test_o261_veto_na_porta.py:345`.

Nas demais classes: **sem achado de segundo juiz novo não autorizado**, pois há corte e registro explícitos; **sem achado concreto de nova inversão de trava ou relógio fora da borda** na cadeia acrescentada. Há custo adicional no request de resposta tardia: ele já carrega a célula e o veto volta a consultá-la pelo carregador de vínculo; violação de orçamento ou impacto de latência: **[não verificado]**. `app/docs/CORTES.md:401`, `app/core/juizes.py:248`, `app/chamados/models.py:896`, `app/chamados/services/resposta_tardia.py:44`, `app/escala/servico_jornada.py:46`.

Execução da suíte Django, RED anterior, identidade Git do commit e DIFF real de produção: **[não verificado]**. [nao verificado]

## 30/10 17:29 · sessao dfa19a83b29b · CODEX-CONTEXTO-DA-CASA-AB · verificador · origem=Code

**Reprovo o pouso A:** há reabertura automática que contorna o veto, nascimento do universo sem dia que passa livre e mutantes de escritor que o selo não detecta. (`app/ponto/management/commands/reconciliar_grade.py:522`; `app/chamados/models.py:892`; `app/chamados/tests/test_o261_veto_na_porta.py:165`)

Os contraexemplos abaixo são entradas construídas a partir do código. Incidência em produção, DIFF de frota, execução da suíte Django e RED anterior: **[não verificado]**. [nao verificado]

**(b) Caminhos automáticos que escapam da guarda**

1. **A grade reabre depois de a porta recusar o renascimento.**

   `abrir` tenta `_renascer_por_premissa_viva`, descarta seu resultado e devolve o chamado encerrado. A nova condição da grade trata somente `ch is None`; recebendo o encerrado, chama `reabrir()` diretamente, sem consultar o veto. (`app/chamados/models.py:685`; `app/chamados/models.py:688`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`)

   **Contraexemplo:** `_emitir_furo(c, dia, marks, ec_saudavel)`, com `batida_ausente` resolvido e premissa ainda furada, enquanto o vínculo consultado pela porta está suspeito → os vetos externos passam pelo `ec_saudavel`; o renascimento recusa; a chamada direta seguinte reabre. A própria diferença entre o vínculo fornecido e o consultado é admitida pelo tratamento novo do detector. (`app/ponto/management/commands/reconciliar_grade.py:487`; `app/ponto/management/commands/reconciliar_grade.py:491`; `app/chamados/models.py:778`; `app/ponto/management/commands/detectar_ausencias.py:729`)

2. **`bater_ponto` continua criando automaticamente um dos cinco módulos sem submeter seus dias ao veto.**

   O contexto de `entrada_bloqueada_turno_aberto` contém somente resumo textual e total. `_veto_da_regua` não encontra dia e libera. Trata-se de passagem por `criar`, mas sem aplicação da invariante. (`app/ponto/views.py:280`; `app/ponto/views.py:291`; `app/chamados/models.py:892`)

   **Contraexemplo:** uma pendência de turno em dia vetado, contexto `{'pendentes': '20/10/2026 08:00', 'total': 1}` → nasce chamado automático e pode sair push. O contador também não o acusa: incrementa apenas `sem_dia`. (`app/ponto/views.py:301`; `app/ponto/views.py:305`; `app/ponto/services/furos_vetados.py:94`)

   O teste transforma essa fuga em comportamento esperado. (`app/chamados/tests/test_o261_veto_na_porta.py:313`)

3. **A reabertura automática para tornar resposta visível continua fora da guarda.**

   `resolver_perguntas_por_chamado_resolvido` pode chamar `_pai.reabrir()` sem restringir o módulo do pai nem consultar o veto. A função também é chamada automaticamente por signal e reconciliador. (`app/chamados/services/disputa_emissao.py:1601`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/signals.py:92`; `app/chamados/reconciliador.py:538`)

   **Contraexemplo estrutural:** disputa aberta com pai `turno_aberto` encerrado, resposta ainda aguardando validação, premissa de reabertura admissível e dia vetado → o pai reabre diretamente. Existência desse arranjo na produção: **[não verificado]**. (`app/chamados/services/disputa_emissao.py:1598`; `app/chamados/services/disputa_emissao.py:1604`; `app/chamados/models.py:1119`; `app/chamados/models.py:1158`)

**Outras falhas contra os avais**

4. **O pouso A altera comportamento; DIFF zero não decorre da construção.** [nao verificado]

   Os emissores de turno consultados não tinham a pergunta externa equivalente à nova guarda. Um vínculo existente e suspeito continua elegível para chegar à criação; agora a porta recusa. (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/models.py:838`)

   **Contraexemplo:** turno aberto há mais de 24 horas, vínculo válido com veto de suspeita, sem chamado existente, ausência ou absolvição da célula → antes o ramo criava; depois retorna `None` e pula os efeitos seguintes. É mudança coerente com o veto desejado, mas incompatível com “comportamento igual” no pouso A. (`app/ponto/management/commands/processar_alertas_avancados.py:94`; `app/ponto/management/commands/processar_alertas_avancados.py:114`; `app/ponto/management/commands/processar_alertas_avancados.py:141`; `app/docs/CORTES.md:401`)

5. **A exceção humana passa, mas falta mostrar o veto e registrar sua admissão.**

   Desarquivar e reabrir disputa admin usam `reabrir()` diretamente. Há trilhas existentes dessas ações, porém elas não identificam o veto admitido; o próprio contador declara que essa trilha própria foi adiada para B. (`app/chamados/views_arquivar.py:68`; `app/chamados/views_arquivar.py:75`; `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/acoes_disputa.py:270`; `app/ponto/services/furos_vetados.py:82`)

   **Contraexemplo:** admin desarquiva chamado de dia vetado cuja reabertura seja admissível → volta à fila sem aviso de veto na resposta e sem registro da exceção ao veto. O contador passa a acusar esse ato autorizado como vivo vetado. (`app/chamados/views_arquivar.py:68`; `app/chamados/views_arquivar.py:80`; `app/ponto/services/furos_vetados.py:98`)

   **Abertura manual: sem achado de regressão alcançável pelo formulário atual.** Ele valida o catálogo, e nenhum dos cinco módulos está declarado como abrível por humano; não há fundamento para atribuir a esse formulário um novo `None.pk`. (`app/chamados/services/abertura.py:149`; `app/chamados/catalogo/modulos.py:105`; `app/chamados/catalogo/modulos.py:182`; `app/chamados/catalogo/modulos.py:283`)

6. **O contador introduz duas inconsistências operacionais.**

   - **Vínculo inexistente contado como um vínculo:** dois furos com `ec=None` → a lista entrega dois achados com `vinculo=None`; o comando imprime dois furos e **um vínculo**, pelo conjunto `{None}`. (`app/ponto/services/furos_vetados.py:61`; `app/ponto/management/commands/furos_vetados_por_regua.py:32`; `app/ponto/management/commands/furos_vetados_por_regua.py:33`)
   - **Vigia novo descartado pelo placar:** um chamado vivo vetado e nenhuma célula furo → o comando imprime `chamados_vivos_em_dia_vetado=1`, mas o placar existente extrai somente `furos_vetados_por_regua=0`. (`app/ponto/management/commands/furos_vetados_por_regua.py:49`; `bin/placar_code.sh:209`; `bin/placar_code.sh:211`)

**(a) Censo dos leitores cujo comportamento muda**

A alteração `ec=None → VETO_DEGENERADA` alcança estes leitores diretos, além da porta. (`app/ponto/services/furos_vetados.py:32`)

| Leitor | Entrada → mudança observável |
|---|---|
| `app/ponto/services/cartorio.py:1013` | Célula já julgada, sem vínculo e demais insumos iguais → passa a incluir veto na impressão; deixa de ser pulada por estabilidade. (`app/ponto/services/cartorio.py:329`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`) |
| `app/ponto/services/furos_sem_cobranca.py:96` | Furo elegível, sem vínculo e sem outra exclusão → passa de porta de cobrança para `veto`; total pode cair de 1 para 0. (`app/ponto/services/furos_sem_cobranca.py:107`; `app/ponto/services/furos_sem_cobranca.py:109`) |
| `app/ponto/services/furos_vetados.py:57` | Furo sem vínculo → passa a integrar a lista CARTORIO-VETO, com vínculo nulo. (`app/ponto/services/furos_vetados.py:61`) |
| `app/chamados/reconciliador.py:854` | Reemissão elegível, sem vínculo e sem costura em vivo → retorna `False` pelo veto antes de pedir nascimento. (`app/chamados/reconciliador.py:852`; `app/chamados/reconciliador.py:855`) |
| `app/chamados/models.py:896` | Módulo do universo com dia forte e sem vínculo → recusa nascimento/renascimento. O dry novo também consulta essa função. (`app/chamados/models.py:889`; `app/chamados/models.py:900`) |
| `app/ponto/services/furos_vetados.py:98` | Chamado vivo com dia forte e sem vínculo → entra nos achados do contador novo. (`app/ponto/services/furos_vetados.py:100`) |

Os efeitos transitivos incluem: cobrança do passivo deixa de tentar itens classificados como veto; comandos e placar mudam seus totais. (`app/ponto/services/furos_sem_cobranca.py:125`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`; `app/ponto/management/commands/furos_sem_cobranca_viva.py:17`; `bin/placar_code.sh:277`; `bin/placar_code.sh:209`)

`desfazer_corroboracao_regua` também lê a lista, mas **sem mudança de saída por `ec=None`**: filtra por um PK de vínculo existente, excluindo os novos itens nulos. (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`)

Os leitores diretos afetáveis de `abrir/criar/_renascer_por_premissa_viva` são:

| Leitor | Efeito novo |
|---|---|
| `app/ponto/management/commands/detectar_ausencias.py:688` | Novo nascimento pode retornar `VETO_NA_PORTA`; dedup encerrado pode continuar encerrado. (`app/ponto/management/commands/detectar_ausencias.py:732`; `app/ponto/management/commands/detectar_ausencias.py:734`) |
| `app/ponto/management/commands/reconciliar_grade.py:499` | Recusa nascimento nulo, mas contorna recusa de renascimento — achado 1. (`app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`) |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | Novo chamado recusado → `_marco_critico` deixa de emitir alerta, push e justificativa. (`app/ponto/management/commands/processar_alertas_turno.py:317`) |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Novo chamado recusado → pula push e montagem do fio. (`app/ponto/management/commands/processar_alertas_avancados.py:141`) |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Deixa de emitir e contabiliza recusa por veto; dry também muda. (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`) |
| `app/chamados/reconciliador.py:862` | Recusa da porta → retorna `False`, sem costurar a pergunta. (`app/chamados/reconciliador.py:885`) |
| `app/chamados/models.py:686` | Dedup pode devolver o mesmo encerrado sem renascer. (`app/chamados/models.py:688`) |
| `app/chamados/services/resposta_tardia.py:45` | Encerrado com célula acusando e dia vetado → deixa de reabrir; grava nota tardia e, havendo texto, Pauta. (`app/chamados/services/resposta_tardia.py:52`; `app/chamados/services/resposta_tardia.py:58`) |

A resposta HTTP também muda: o estado tardio encerra o fluxo antes da declaração normal. (`app/chamados/views.py:1053`; `app/chamados/views.py:1055`)

O detector tem leitores transitivos no próprio comando, no cartório e no retroativo. Este último ignora o retorno: **recusa nova → continua incrementando `emitidos`**, embora nenhum chamado nasça. (`app/ponto/management/commands/detectar_ausencias.py:439`; `app/ponto/services/cartorio.py:1358`; `app/ponto/services/cartorio.py:1394`; `app/ponto/management/commands/emitir_furo_retroativo.py:142`; `app/ponto/management/commands/emitir_furo_retroativo.py:143`)

**(c) O selo AST deixa passar mutantes plausíveis**

As variantes abaixo foram conferidas com `varrer_fonte` em memória: as que escapam retornam `([], [])`. A limitação está no reconhecimento de raízes, aliases e argumentos da chamada. (`app/chamados/tests/test_o261_veto_na_porta.py:87`; `app/chamados/tests/test_o261_veto_na_porta.py:137`; `app/chamados/tests/test_o261_veto_na_porta.py:165`)

| Mutante dentro de função de produção | Resultado |
|---|---|
| `from chamados.models import ChamadoColaborador as Ch; C = Ch`; depois `C.objects.create(...)` | **Escapa:** alias transitivo não é resolvido. (`app/chamados/tests/test_o261_veto_na_porta.py:98`) |
| `from . import models as m`; depois `m.ChamadoColaborador.objects.create(...)` | **Escapa:** raiz `m` não é reconhecida como modelo. (`app/chamados/tests/test_o261_veto_na_porta.py:92`; `app/chamados/tests/test_o261_veto_na_porta.py:168`) |
| `ch = ChamadoColaborador(...); ch.save()` | **Escapa:** construtor e `save` não são classificados como nascimento; o default é aberto. (`app/chamados/tests/test_o261_veto_na_porta.py:28`; `app/chamados/models.py:46`) |
| `manager = ChamadoColaborador.objects`; depois `manager.bulk_create([...])` | **Escapa:** perde a raiz reconhecida. (`app/chamados/tests/test_o261_veto_na_porta.py:168`) |
| `campos = {'status_local': 'aberto'}`; depois `ChamadoColaborador.objects.filter(...).update(**campos)` | **Escapa:** a string é procurada somente dentro da chamada. (`app/chamados/tests/test_o261_veto_na_porta.py:143`) |

**Contraexemplo comum:** fornecer um dia vetado nessas criações, ou atualizar um encerrado para aberto → nasce/reabre sem consultar a porta, sem acrescentar ofensor ao selo. (`app/chamados/tests/test_o261_veto_na_porta.py:170`; `app/chamados/tests/test_o261_veto_na_porta.py:172`)

O selo anterior de status também não cobre construtor+`save` nem esse `update(**campos)`: procura atribuição explícita e `update(status_local=...)`. (`app/chamados/tests/test_contract_status_nucleo.py:7`)

**Sem achado nas formas diretas:** `from .models import ChamadoColaborador as Ch`, `Ch.objects.bulk_create(...)` e `update(**{'status_local': 'aberto'})` são detectados. (`app/chamados/tests/test_o261_veto_na_porta.py:92`; `app/chamados/tests/test_o261_veto_na_porta.py:143`; `app/chamados/tests/test_o261_veto_na_porta.py:170`)

Há ainda cegueira para cópia externa seguida de `ch.reabrir()`: `pede_porta` exige raiz com nome da classe, não uma instância. (`app/chamados/tests/test_o261_veto_na_porta.py:166`)

**(d) Vacuidade e cobertura dos testes da porta** [nao verificado]

- **VETO-EC-NONE não é provado na porta.** `test_sem_vinculo_veta` chama exclusivamente o juiz; a fixture mantém vínculo válido. Mutante: `_veto_da_regua` libera quando o vínculo é `None` → esse teste continua passando e os casos da porta não exercitam a condição. Execução da suíte mutante: **[não verificado]**. (`app/chamados/tests/test_o261_veto_na_porta.py:266`; `app/chamados/tests/test_o261_veto_na_porta.py:302`; `app/chamados/models.py:896`)

- **Os cinco módulos não estão selados por casos de veto.** Não há caso de bloqueio para `batida_ausente` ou `intervalo_pendente`. Mutante: limitar o universo aos três módulos exercitados → esses dois ficam livres sem contrariar os casos escritos. Execução da suíte mutante: **[não verificado]**. (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:269`; `app/chamados/tests/test_o261_veto_na_porta.py:298`; `app/chamados/tests/test_o261_veto_na_porta.py:324`)

- **Sem achado de vacuidade no par de renascimento.** Há negativo com suspeita e positivo sem suspeita usando o mesmo módulo. (`app/chamados/tests/test_o261_veto_na_porta.py:320`; `app/chamados/tests/test_o261_veto_na_porta.py:338`)

- **Sem achado de vacuidade no contador quanto a detectar um vivo vetado:** há controle com zero e caso com um achado. Ele não cobre a exceção humana nem o nascimento sem dia. (`app/chamados/tests/test_o261_veto_na_porta.py:345`; `app/ponto/services/furos_vetados.py:82`; `app/ponto/services/furos_vetados.py:94`)

- O teste “sem dia não veta” passaria sem a guarda nova; serve como compatibilidade, não como RED do veto. (`app/chamados/tests/test_o261_veto_na_porta.py:313`)

**Demais classes solicitadas**

- **Segundo juiz novo indevido: sem achado.** O registro tem o corte literal autorizado; o predicado compõe autoridades existentes. As três cópias externas estão explicitamente reservadas ao lote B. (`app/core/juizes.py:248`; `app/docs/CORTES.md:401`; `app/ponto/services/furos_vetados.py:25`; `app/chamados/tests/test_o261_veto_na_porta.py:46`)
- **Derivação paralela de vínculo: sem achado.** A nova guarda delega ao carregador e à autoridade existentes. (`app/ponto/services/furos_vetados.py:15`; `app/escala/servico_jornada.py:38`; `app/escala/servico_jornada.py:57`)
- **Relógio novo fora da borda e inversão nova de travas: sem achado no caminho acrescentado.** A guarda recebe o dia e não acrescenta trava explícita. (`app/chamados/models.py:878`; `app/chamados/models.py:896`)
- **Custo no request:** há nova consulta de vínculo, com consulta à célula e possível carga de escalas; impacto de latência e teto aceitável: **[não verificado]**. (`app/chamados/models.py:896`; `app/escala/servico_jornada.py:46`; `app/escala/servico_jornada.py:58`)
- **Pouso antes de pré-requisito:** ordem efetiva de pousos e evidência de RED anterior: **[não verificado]**. [nao verificado]

## 30/10 17:29 · sessao dc94147ddf7c · CODEX-CONTEXTO-DA-CASA-AB · verificador · origem=Code

**Reprovo o pouso A:** há reaberturas automáticas fora do veto, mudanças de comportamento incompatíveis com “DIFF zero por construção” e mutantes que o selo não detecta. (`app/chamados/services/disputa_emissao.py:1606`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/chamados/tests/test_o261_veto_na_porta.py:168`)

Os contraexemplos abaixo são entradas construídas a partir do código. Ocorrência, quantidade e impacto em produção, assim como resultado da suíte com banco: **[não verificado]**. [nao verificado]

**(b) Caminhos que escapam da porta**

1. **O signal reabre automaticamente por `reabrir()` direto.** Ao encerrar um chamado, o receiver chama `resolver_perguntas_por_chamado_resolvido`; essa função pode reabrir o pai da disputa diretamente. O veto foi colocado em `_renascer_por_premissa_viva`, mas `reabrir()` continua sem consultá-lo. (`app/chamados/signals.py:75`; `app/chamados/signals.py:92`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:778`; `app/chamados/models.py:1119`)

   **Contraexemplo:** pai `entrada_bloqueada_turno_aberto`, resolvido, com `data_turno=D`, régua suspeita, disputa aberta e resposta de irmã aguardando consumo; demais guardas permissivas → encerrar um chamado-destino aciona o signal e o pai volta para `em_analise`, apesar de `VETO_REGUA`. A condição da resposta pendente está em `app/chamados/services/disputa_emissao.py:1604`; a escrita, em `app/chamados/models.py:1158`.

2. **A grade pode desfazer a recusa do renascimento.** `abrir()` devolve o chamado encerrado mesmo quando `_renascer_por_premissa_viva()` recusa; a grade, recebendo esse encerrado, chama `reabrir()` diretamente. O novo tratamento de `None` não cobre esse retorno. (`app/chamados/models.py:685`; `app/chamados/models.py:688`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`)

   **Contraexemplo:** o `ec` recebido pela grade passa no pré-veto; o cadastro muda durante o processamento e a porta relê vínculo vetado → o renascimento recusa, `abrir()` devolve o encerrado e a tentativa direta posterior pode reabri-lo. O pré-veto usa o vínculo recebido; a porta rebusca o vínculo. Ocorrência desse interleaving: **[não verificado]**. (`app/ponto/management/commands/reconciliar_grade.py:487`; `app/ponto/management/commands/reconciliar_grade.py:491`; `app/chamados/models.py:896`)

3. **A emissão automática sem dia passa pela porta sem julgamento.** O POST de entrada cria `entrada_bloqueada_turno_aberto` com apenas resumo de pendentes e total; `_veto_da_regua()` libera quando não encontra dia forte. O contador também deixa esse chamado fora dos achados, registrando somente `sem_dia`. (`app/ponto/views.py:275`; `app/ponto/views.py:280`; `app/ponto/views.py:291`; `app/chamados/models.py:892`; `app/ponto/services/furos_vetados.py:93`)

   **Contraexemplo:** turno antigo D aberto há pelo menos 14 horas, vínculo de D ausente, nenhum chamado vivo e colaborador registra entrada → nasce o chamado automático e sai push, mas o vigia não o identifica como vetado. Isso não é exceção humana do admin. O catálogo declara `data_turno` para esse módulo, mas o emissor não a fornece. (`app/ponto/views.py:305`; `app/chamados/catalogo/modulos.py:182`)

4. **O comando de recuperação também reabre sem veto.** Ele consulta apenas `recusa_da_reabertura()` e depois chama `pai.reabrir()`, usando usuário sistema. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:105`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:139`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`)

   **Contraexemplo:** pergunta fantasma elegível, lâmpada apagada, C6 viva, pai encerrado de módulo do universo e régua suspeita → `--apply` reabre o pai. É comando supervisionado de uma vez; enquadrá-lo como exceção humana autorizada ao veto é **[não verificado]**. (`app/chamados/management/commands/reabrir_orfas_enterradas.py:17`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:77`)

O reverso da migration 0038 também escreve vitalidade diretamente, mas está nominalmente autorizado para o lote C; **sem achado de extrapolação do aval nesse caminho declarado**. (`app/chamados/migrations/0038_a3_registrado_backfill.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:41`)

**(a) Censo dos leitores cujo comportamento muda**

A alegação de comportamento igual não se sustenta para vínculo **presente e suspeito**: os emissores de turno antes chegavam ao nascimento; agora recebem recusa da porta. (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`; `app/chamados/models.py:838`)

| Leitor da porta | Entrada → mudança |
|---|---|
| `app/ponto/management/commands/detectar_ausencias.py:688` | Vínculo recebido passa, vínculo relido pela porta veta → retorna `VETO_NA_PORTA`, em `:733`. |
| `app/ponto/management/commands/reconciliar_grade.py:499` | Mesma divergência → trata `None` em `:519`; retorno encerrado ainda permite o escape de `:522`. |
| `app/chamados/reconciliador.py:862` | Sem vínculo, o pré-veto alterado em `:854` passa a interromper antes da abertura, em `:857`. |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | Turno cobravel com régua suspeita → nascimento recusado; `_marco_critico` interrompe antes do alerta e justificativa, em `:317`. |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Turno aberto em N5, vínculo presente suspeito, sem cobertura ou irmão → antes nascia; agora para em `:141`, antes do push e fio. |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Turno antigo elegível com vínculo suspeito → recusa; dry também muda em `:138`, apply contabiliza em `:174`. |
| `app/chamados/models.py:686` | Dedup encontra encerrado em dia vetado → renascimento recusa; método devolve o próprio encerrado em `:688`. |
| `app/chamados/services/resposta_tardia.py:45` | Célula acusa, chamado encerrado e vínculo vetado → deixa de retornar `reaberto`; grava nota/log, pode criar Pauta e retorna `tardia`, em `:52`, `:58` e `:67`. |

Há ainda uma brecha na justificativa específica de `ec=None`: em `processar_alertas_turno`, a ausência de vínculo só interrompe aquele ramo quando `dt_fim_previsto` está ausente. Com fim previsto materializado preservado, o fluxo pode alcançar a emissão; falha no recompute não encerra o processamento. **Contraexemplo:** TM aberto com fim previsto, vínculo ausente, janela `pos_20` e demais guardas permissivas → antes podia emitir; agora veta. Ocorrência real: **[não verificado]**. (`app/ponto/management/commands/processar_alertas_turno.py:68`; `:100`; `:107`; `:134`; `:335`)

Os demais leitores de `motivo_do_veto` também mudam com `ec=None`:

| Leitor | Entrada → mudança |
|---|---|
| `app/ponto/services/cartorio.py:1013` | Célula já julgada, vínculo ausente e demais insumos iguais → impressão ganha `veto:VETO_DEGENERADA`; deixa de satisfazer o skip e pode ser rejulgada. (`app/ponto/services/cartorio.py:329`; `:1083`; `:1092`) |
| `app/ponto/services/furos_sem_cobranca.py:96` | Ativo, furo com protesto, sem cobrança viva nem vínculo → passa à porta `veto`, excluída do total. (`app/ponto/services/furos_sem_cobranca.py:30`; `:109`) |
| `app/ponto/services/furos_vetados.py:57` | Furo sem vínculo → passa a integrar os achados com `vinculo=None`, em `:61`. |
| `app/chamados/reconciliador.py:854` | C2 sem vínculo e sem chamado vivo para costura → agora recusa em `:857`. |
| `app/chamados/models.py:896` | Novo leitor comum a nascimento, renascimento e consulta dry. (`app/chamados/models.py:731`; `:778`; `:838`; `:903`) |
| `app/ponto/services/furos_vetados.py:98` | Novo leitor do contador de chamados vivos vetados. |

O leitor indireto `desfazer_corroboracao_regua` descarta os novos achados `vinculo=None` ao filtrar por `ec.pk`: **sem achado de mudança no resultado desse leitor**. (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`)

As chamadas variáveis de abertura não provam quebra da UI humana atual: a abertura local não fornece dia; a abertura pelo DP filtra o catálogo antes de criar, e os módulos do universo não estão habilitados nesse formulário. **Sem achado de `None.pk` alcançável por essa UI atual.** (`app/chamados/services/abertura.py:47`; `:58`; `:149`; `app/chamados/catalogo/modulos.py:105`; `:281`; `:306`)

**A exceção humana ficou incompleta**

Reabrir/desarquivar continua passando, mas falta mostrar o veto e registrar o motivo específico da exceção. A reabertura administrativa recolhe justificativa genérica; desarquivar usa texto fixo. O próprio contador reconhece que a trilha específica foi deixada para depois. (`app/chamados/services/acoes_disputa.py:245`; `app/chamados/services/acoes_disputa.py:266`; `app/chamados/views_arquivar.py:75`; `app/ponto/services/furos_vetados.py:82`)

**Contraexemplo:** admin desarquiva chamado em dia vetado → volta à fila sem apresentação do veto e com motivo fixo “Chamado resgatado…”. Isso não cumpre integralmente o aval de 08:2x. (`app/chamados/views_arquivar.py:68`; `app/chamados/views_arquivar.py:77`)

**O vigia não chega ao placar, e o contador antigo inventa um vínculo**

O comando imprime `chamados_vivos_em_dia_vetado`, mas seu consumidor no placar extrai exclusivamente `furos_vetados_por_regua=`. **Contraexemplo:** o comando encontra um chamado vivo vetado → imprime o achado, mas o filtro descarta esse novo contador. (`app/ponto/management/commands/furos_vetados_por_regua.py:49`; `bin/placar_code.sh:209`; `bin/placar_code.sh:210`)

Além disso, `None` entra no conjunto de vínculos. **Contraexemplo:** dois furos sem vínculo → publica dois furos e **um vínculo**, embora nenhum exista. (`app/ponto/services/furos_vetados.py:61`; `app/ponto/management/commands/furos_vetados_por_regua.py:32`; `:33`)

**(c) O selo AST deixa passar mutantes plausíveis**

A execução isolada de `varrer_fonte`, em memória, confirmou os resultados abaixo; as lacunas correspondem ao reconhecimento limitado de aliases, métodos e argumentos. (`app/chamados/tests/test_o261_veto_na_porta.py:92`; `:137`; `:168`)

| Mutante | Resultado |
|---|---|
| Import direto `from chamados.models import ChamadoColaborador as Ch` e `Ch.objects.create(...)` | Detectado: **sem achado nessa forma**. (`:92`; `:170`) |
| Import relativo `from .models import ChamadoColaborador as Ch` | Detectado: **sem achado nessa forma**. (`:92`) |
| `from . import models as m; m.ChamadoColaborador.objects.create(...)` | **Passa**, retorno `([], [])`; alias do módulo não é resolvido. (`:92`; `:168`) |
| `from chamados.models import ChamadoColaborador as Ch; C = Ch; C.objects.create(...)` | **Passa**; atribuição só reconhece RHS literalmente `ChamadoColaborador`. (`:100`) |
| `obj = Ch(...); obj.save()` | **Passa**; `save` não é contado. O default já permite nascer `aberto`. (`:170`; `app/chamados/models.py:46`) |
| `Ch.objects.bulk_create(...)` | Detectado: **sem achado na chamada direta**. (`:28`; `:170`) |
| `manager = Ch.objects; manager.bulk_create(...)` | **Passa**; raiz `manager` não é reconhecida. (`:168`) |
| `kw = {'status_local': 'aberto'}; Ch.objects.filter(...).update(**kw)` | **Passa**; `_vivifica` não acompanha a variável. (`:137`) |
| `update(**{'status_local': 'aberto'})` | Detectado: **sem achado no dicionário inline**. (`:143`) |
| Veto externo seguido de `ch.reabrir()` | **Passa** na detecção de cópia; exige raiz com nome do modelo, não instância. (`:166`) |

**Contraexemplo operacional:** construtor + `save()` com módulo `turno_aberto`, dia D e régua suspeita → nasce fora do veto e passa também pelo selo complementar de estado, que procura apenas atribuição `.status_local =` ou `update(status_local=...)`. (`app/chamados/models.py:46`; `app/chamados/tests/test_contract_status_nucleo.py:7`; `:23`)

A catraca também não assegura “só desce”: verifica apenas cardinalidade `<= TETO`, e a varredura deduplica por função. **Contraexemplos:** substituir um nome permitido por outro, ou acrescentar outra escrita dentro de função já permitida → a proteção não demonstra redução dos caminhos. (`app/chamados/tests/test_o261_veto_na_porta.py:194`; `:209`)

**(d) Os testes não fecham a prova da porta** [nao verificado]

- **EC-NONE é testado somente no juiz.** A fixture da porta tem vínculo; `test_sem_vinculo_veta` chama diretamente `motivo_do_veto`. **Mutante:** `_veto_da_regua` retorna `None` quando o vínculo é ausente → viola o veto nas portas, mas esse teste continua distinguindo apenas o juiz. (`app/chamados/tests/test_o261_veto_na_porta.py:266`; `:302`; `app/chamados/models.py:896`)
- **Faltam casos de porta para `batida_ausente` e `intervalo_pendente`.** Excluí-los do veto preservaria os cenários de porta escritos nesse arquivo. (`app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:269`)
- **`criar` tem apenas a negativa do módulo testado**, sem controle positivo correspondente nem exigência da trilha do veto. Vacuidade efetiva por outra guarda nessa fixture: **[não verificado]**. (`app/chamados/tests/test_o261_veto_na_porta.py:295`)
- **Sem achado de vacuidade no par de renascimento com/sem veto**: há controle positivo correspondente. (`app/chamados/tests/test_o261_veto_na_porta.py:320`; `:338`)
- **O teste sem dia exige a liberação que deixa o emissor descoberto**, em vez de demonstrar o veto dos cinco módulos no fluxo real. (`app/chamados/tests/test_o261_veto_na_porta.py:313`; `app/ponto/views.py:280`)

Quanto às demais classes pedidas: **sem achado de juiz novo sem corte**, pois o aval final registra explicitamente `motivo_do_veto`; **sem achado demonstrado de inversão de trava ou relógio novo dentro do predicado**. Há carregamento adicional no request de resposta tardia, mas custo e cumprimento de teto são **[não verificado]**. (`app/docs/CORTES.md:401`; `app/core/juizes.py:248`; `app/chamados/models.py:669`; `app/ponto/services/furos_vetados.py:25`; `app/escala/servico_jornada.py:46`; `app/chamados/services/resposta_tardia.py:45`)

## 30/10 17:27 · sessao 50fa2c4cdd3a · - · - · origem=Ronald (tmux)

Achados confirmados, só por leitura:

1. **Reabertura automática foge do veto.** `resolver_perguntas_por_chamado_resolvido` reabre `_pai` diretamente, sem restringir módulo nem consultar veto (`app/chamados/services/disputa_emissao.py:1598`, `:1606`). É acionado automaticamente pelo signal de encerramento (`app/chamados/signals.py:75`, `:92`), pelo reconciliador (`app/chamados/reconciliador.py:538`, `:1547`) e pelo serviço de becos (`app/chamados/services/becos.py:49`). Contra-exemplo: pai `entrada_bloqueada_turno_aberto` resolvido, dia conhecido sob régua suspeita, disputa aberta e irmã respondida aguardando consumo, sem irmão vivo ou outras recusas → `_pai.reabrir()` grava `em_analise`; o veto colocado somente no `_renascer_por_premissa_viva` não participa (`app/chamados/models.py:778`, `:1119`, `:1158`).

2. **A grade contorna uma recusa do renascimento.** `abrir` devolve o existente encerrado mesmo quando `_renascer_por_premissa_viva` recusa (`app/chamados/models.py:685`, `:688`); a grade então chama `ch.reabrir()` (`app/ponto/management/commands/reconciliar_grade.py:521`). Contra-exemplo na interface `_emitir_furo`: `ec` fornecido saudável, chamado existente cujo dia pelo vínculo/célula é vetado, premissa ainda viva → a porta recusa; a grade reabre. Ambos os carregadores atuais usam o mesmo `vinculo_do_dia` (`app/ponto/management/commands/reconciliar_grade.py:196`, `app/escala/servico_jornada.py:57`); divergência de vínculo para o mesmo dia sem concorrência: **[não verificado]**. Um exemplo possível com estado inconsistente é JSON `data_turno=D`, célula ligada em `D-1`: dedup encontra por JSON, veto usa primeiro a célula (`app/chamados/models.py:670`, `app/chamados/juizes.py:89`).

3. **A invariante dos cinco módulos tem saída explícita sem dia.** O nascimento automático em `bater_ponto` grava `entrada_bloqueada_turno_aberto` com somente `pendentes` e `total` (`app/ponto/views.py:275`, `:280`, `:291`). Sem dia, `_veto_da_regua` retorna sem perguntar vínculo ou veto (`app/chamados/models.py:889`). Contra-exemplo: colaborador sem vínculo, entrada antiga aberta ≥14h, POST de nova entrada e nenhum chamado vivo → nasce chamado desse módulo. O teste aprova essa saída (`app/chamados/tests/test_o261_veto_na_porta.py:313`); o vigia coloca em `sem_dia`, fora dos achados (`app/ponto/services/furos_vetados.py:99`). É uma passagem pela porta sem aplicação da invariante, não um escritor externo.

4. **“DIFF zero por construção” não decorre do código.** `processar_alertas_turno` só cala por `ec=None` no ramo em que `tm.dt_fim_previsto` está vazio (`app/ponto/management/commands/processar_alertas_turno.py:100`). Contra-exemplo: TM aberto com fim previsto já materializado, vínculo posteriormente encerrado, relógio no `pos_20`, sem alerta/chamado existente → antes alcançava `abrir`, agora é recusado (`:118`, `:141`, `:316`, `:335`; `app/ponto/services/furos_vetados.py:32`). Mesmo com vínculo presente, régua suspeita antes não era consultada pelos emissores avançado e retroativo; agora veta (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `:121`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `:151`). Quantidade real afetada: **[não verificado]**.

5. **Exceção humana passa sem cumprir tela e trilha do veto.** Desarquivar chama `reabrir` e responde sucesso, sem consultar/exibir veto; grava motivo fixo (`app/chamados/views_arquivar.py:68`, `:75`, `:80`). Reabertura administrativa exige justificativa, mas também não consulta ou registra o veto admitido (`app/chamados/services/acoes_disputa.py:245`, `:266`, `:270`). Reabrir competência registra autor e competência, sem veto (`app/chamados/services/competencia_trancada.py:258`). Contra-exemplo: admin desarquiva chamado de dia sob régua suspeita → ele volta sem aviso do veto nem justificativa desse desvio. O próprio vigia declara a falta da trilha e conta esse ato humano no esperado zero (`app/ponto/services/furos_vetados.py:83`).

Classificação dos outros `reabrir` diretos:

- `detectar_ausencias.py:301`: exclusivo `ancora_limbo`, fora do universo (`:273`).
- `adesao.py:92`: exclusivo `sem_previsao_escala`, fora (`:75`).
- `reabrir_chamados_ausencia_aberta.py:49`: exclusivo `ausencia_colab`, fora (`:29`).
- `reabrir_orfas_enterradas.py:161`: pai genérico, pode pertencer ao universo; comando de uma vez com `--apply`, descrito como ato de pouso, sem prova de cron automático (`:1`, `:22`). Usa usuário de sistema (`:139`) e também não registra veto admitido. [nao verificado]
- `regularizacao_ext.py:246`: cria exclusivamente `regularizacao_externa`, fora (`:250`).
- `intervalo_pendente`: emissor aposentado; só reconciliação nesse command (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`).

Sem achado adicional comprovado de ordem de trava, relógio novo fora da borda ou custo excessivo no request; custo medido e impacto em produção: **[não verificado]**.

## 30/10 17:27 · sessao d0b017b42275 · - · - · origem=Ronald (tmux)

Achados para consolidação, por leitura estática:

1. **Reabertura automática escapa pelo signal.** `app/chamados/services/disputa_emissao.py:1606` chama `_pai.reabrir()` diretamente. Esse caminho é acionado ao resolver outro chamado pelo receiver `app/chamados/signals.py:75-92`. O veto novo está apenas em `_renascer_por_premissa_viva`, `app/chamados/models.py:778-781`; `reabrir` passa pelas guardas antigas e grava o estado em `app/chamados/models.py:1119-1158`.
   - Contraexemplo: pai resolvido de módulo `entrada_bloqueada_turno_aberto`, contexto `data_turno=D`, régua suspeita, disputa aberta, resposta de irmã ainda sem consumo e nenhuma outra guarda impeditiva → resolver um chamado-destino aciona o receiver e reabre o pai para `em_analise`, apesar de `VETO_REGUA`. A existência das respostas que sustentam esse caminho é avaliada em `app/chamados/juizes.py:1471-1477`.

2. **Comando de recuperação também reabre diretamente.** `app/chamados/management/commands/reabrir_orfas_enterradas.py:104-111` verifica as guardas antigas; `:139` usa o usuário sistema; `:161` chama `pai.reabrir()`, sem veto.
   - Contraexemplo: pergunta fantasma dentro da janela do comando, lâmpada apagada, C6 viva, pai encerrado de `entrada_bloqueada_turno_aberto`, dia com régua suspeita e demais guardas permissivas → `--apply` reabre o pai vetado. Trata-se de comando supervisionado de uma vez, não de cron, conforme `app/chamados/management/commands/reabrir_orfas_enterradas.py:1-22`; eventual enquadramento como exceção humana ao veto fica **[não verificado]**.

3. **Emissão automática sem dia atravessa o veto e sai do contador.** `app/ponto/views.py:275-302` cria `entrada_bloqueada_turno_aberto` com contexto contendo somente resumo de pendentes e total. A porta libera quando `dia_do_fato(fortes=True)` devolve `None`, em `app/chamados/models.py:892-894`; o contador também retira esses chamados dos achados em `app/ponto/services/furos_vetados.py:93-96`.
   - Contraexemplo: um turno antigo D aberto há pelo menos 14h, nenhum chamado vivo, vínculo de D ausente, colaborador registra entrada → nasce chamado automático, recebe push em `app/ponto/views.py:305-310`, e o vigia registra apenas `sem_dia`, sem achado vetado. A chave declarada para esse módulo é `data_turno`, em `app/chamados/catalogo/modulos.py:182`.

4. **A premissa de DIFF zero com `ec=None` não vale para todo emissor.** `app/ponto/management/commands/processar_alertas_turno.py:100-111` consulta vínculo somente quando `tm.dt_fim_previsto` está ausente. Com fim previsto já materializado, vínculo ausente não interrompe o caminho em `:134-141`; a emissão ocorre por `:335-353`.
   - Contraexemplo: turno materializado aberto, entrada presente, fim previsto conservado, vínculo do dia agora ausente, janela `pos_20`, sem alerta anterior/cobertura/isenção → antes podia nascer; agora a porta veta e `_marco_critico` termina em `app/ponto/management/commands/processar_alertas_turno.py:316-318`. É mudança coerente com o veto final, porém incompatível com a alegação de comportamento igual no pouso A.

5. **Exceção humana continua sem apresentação do veto.** A reabertura administrativa recolhe motivo e justificativa, mas não consulta/apresenta o veto: `app/chamados/services/acoes_disputa.py:245-270` e formulário `app/templates/chamados/partials/modal_fio.html:379-405`. Desarquivar usa motivo fixo e nenhuma justificativa fornecida pelo administrador em `app/chamados/views_arquivar.py:75-79`.
   - Contraexemplo: administrador desarquiva chamado vetado → volta à fila com texto genérico, sem apresentação do veto e sem motivo específico da exceção. O contador inclui a exceção permitida como violação; isso está explicitamente reconhecido em `app/ponto/services/furos_vetados.py:80-83`.

Classificação dos outros `reabrir`:

- `detectar_ausencias.py:301`: chamado `ancora_limbo`, conforme emissão em `app/ponto/management/commands/detectar_ausencias.py:271-273`; fora do universo.
- `reabrir_chamados_ausencia_aberta.py:49`: somente `ausencia_colab`, filtrado em `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:28-31`; fora do universo.
- `adesao.py:92`: pai `sem_previsao_escala`, emitido em `app/chamados/services/adesao.py:74-75`; fora do universo.
- `competencia_trancada.py:258`, `acoes_disputa.py:266` e `views_arquivar.py:68`: caminhos humanos previstos pelo aval; a falta da apresentação/trilha específica do veto permanece, conforme referências do achado 5.
- `reconciliar_grade.py:522`: bypass estrutural adicional. A cópia externa pergunta sobre o `ec` recebido em `app/ponto/management/commands/reconciliar_grade.py:486-492`; a porta pode recusar o renascimento e devolver o encerrado em `app/chamados/models.py:685-688`, mas a grade tenta `reabrir()` diretamente em `:521-522`. Contraexemplo: vínculo recebido válido e vínculo real relido pela porta vetado após mudança de cadastro durante o processamento → renascimento recusa e chamada direta posterior pode reabrir. Ocorrência real desse interleaving **[não verificado]**.

## 30/10 17:27 · sessao a50eb131225e · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **Pouso A não preserva comportamento por construção.** `emitir_turno_aberto_retroativo` verifica vínculo, ausência e irmão, mas não suspeita/degeneração (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `:108`, `:118`). Entrada: turno aberto há 30h, vínculo existente com horários válidos e régua suspeita, célula que não absolve, nenhum irmão → antes `abrir` criava; agora recusa (`app/chamados/models.py:731`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`). O mesmo vale para N5 de `processar_alertas_avancados` (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `:94`, `:121`, `:141`). Impacto efetivo na frota: [não verificado].

2. **Emissão automática sem dia passa, inclusive em módulo do universo.** No POST de bater ponto, `entrada_bloqueada_turno_aberto` recebe somente `pendentes` textual e `total`, sem `data_turno` (`app/ponto/views.py:275`, `:280`, `:291`). A porta retorna “sem veto” quando não resolve o dia forte (`app/chamados/models.py:892`). Entrada: entrada órfã há mais de 14h, nenhum chamado vivo desse módulo, vínculo suspeito → nascimento automático passa sem perguntar `motivo_do_veto`. O teste aceita expressamente esse escape (`app/chamados/tests/test_o261_veto_na_porta.py:315`). O contador também exclui o caso, acumulando apenas `sem_dia` (`app/ponto/services/furos_vetados.py:94`).

3. **Grade pode desfazer a recusa de renascimento.** `abrir` devolve o existente encerrado mesmo se renascer retornar `False` (`app/chamados/models.py:685`). A guarda adicionada na grade só trata `None`; depois chama `reabrir` diretamente (`app/ponto/management/commands/reconciliar_grade.py:519`). `reabrir` não pergunta o veto (`app/chamados/models.py:1058`, `:1119`, `:1158`). Contraexemplo estável: chamado `batida_ausente` resolvido com JSON apontando D1 e célula ligada D2; D1 com vínculo bom permite a cópia externa, D2 com vínculo suspeito faz a porta recusar; a grade recebe o encerrado e o reabre diretamente. O dia forte prefere a célula (`app/chamados/juizes.py:89`). Também ocorre se o vínculo mudar entre a carga do lote e a consulta da porta (`app/ponto/management/commands/reconciliar_grade.py:189`, `:196`, `:254`). Existência desse dado em produção: [não verificado].

4. **Outro caminho automático chama `reabrir` diretamente sem filtro de módulo.** Durante resolução de perguntas, disputa aberta, pai não vivo e irmã respondida sem consumo levam a `_pai.reabrir()` (`app/chamados/services/disputa_emissao.py:1598`). A FK admite qualquer `ChamadoColaborador` (`app/chamados/models.py:1393`). Entrada válida no modelo: pai `entrada_bloqueada_turno_aberto` resolvido, com dia forte vetado, disputa aberta e resposta creditada ao pai, sem outras recusas → reabre ignorando veto. Ocorrência real dessa relação: [não verificado]. `reabrir_orfas_enterradas` tem o mesmo desvio mecânico, mas é comando com `--apply` explícito; não o classificaria como emissão cron sem ressalvar essa distinção (`app/chamados/management/commands/reabrir_orfas_enterradas.py:104`, `:157`).

5. **Ato humano passa, mas o tratamento do veto continua ausente.** Desarquivar chama `reabrir`, levanta silêncio e registra texto genérico, sem consultar ou registrar o motivo do veto (`app/chamados/views_arquivar.py:68`, `:75`, `:79`). Reabrir disputa administra justificativa humana, mas não o motivo de veto (`app/chamados/services/acoes_disputa.py:245`, `:266`, `:270`). O contador declara expressamente que atos humanos ainda contarão como falha até pouso B (`app/ponto/services/furos_vetados.py:83`). Entrada: admin desarquiva chamado de dia suspeito com demais guardas satisfeitas → passa, contador sobe, trilha não identifica o veto (`app/chamados/views_arquivar.py:68`; `app/ponto/services/furos_vetados.py:100`).

Sem achado concreto de nova inversão de trava ou leitura de relógio no trecho acrescentado; a nova cadeia faz leituras por `escala_vigente` (`app/chamados/models.py:896`, `app/escala/servico_jornada.py:41`). Há custo adicional no request de resposta tardia: a célula é carregada em `receber`, depois recarregada pelo veto, mas latência/violação de orçamento são [não verificado] (`app/chamados/services/resposta_tardia.py:44`, `app/chamados/views.py:1050`, `app/escala/servico_jornada.py:46`).

Não achei emissor real do universo que use somente `contexto_json__data_turno` no lookup sem também gravar `defaults.contexto_json`; os emissores conferidos gravam ambos (`app/ponto/management/commands/reconciliar_grade.py:501`, `:508`; `app/ponto/management/commands/processar_alertas_turno.py:338`, `:348`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:153`, `:156`; `app/chamados/reconciliador.py:863`, `:872`).

## 30/10 17:27 · sessao 242d256e8b88 · - · - · origem=Ronald (tmux)

Censo de leitores com mudança inferível:

| Autoridade/leitor | Mudança e contraexemplo |
|---|---|
| `motivo_do_veto` → `furos_vetados_por_regua` | Célula `furo`, colaborador não desligado, vínculo ausente: antes não aparecia; agora aparece `VETO_DEGENERADA`, `vinculo=None` ([furos_vetados.py:32](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_vetados.py:32), [furos_vetados.py:53](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_vetados.py:53)). |
| `motivo_do_veto` → cartório | Vínculo ausente muda o veto usado na impressão; acrescenta `veto:VETO_DEGENERADA` ao hash e pode fazer célula antes estável voltar ao julgamento. Isso é efeito fora da porta ([cartorio.py:1013](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/cartorio.py:1013), [cartorio.py:1083](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/cartorio.py:1083), [cartorio.py:329](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/cartorio.py:329), [cartorio.py:1092](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/cartorio.py:1092)). |
| `motivo_do_veto` → `furos_sem_cobranca.medir` | Dia sem vínculo e sem cobrança sai da porta anterior para `veto`, diminuindo `total`; `cobrar` passa a ignorá-lo ([furos_sem_cobranca.py:96](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_sem_cobranca.py:96), [furos_sem_cobranca.py:109](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_sem_cobranca.py:109), [furos_sem_cobranca.py:125](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_sem_cobranca.py:125)). |
| `motivo_do_veto` → c2 | Pergunta viva, protesto de furo e vínculo ausente: antes podia chegar ao `abrir`; agora devolve `False` antes da porta ([reconciliador.py:852](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/reconciliador.py:852)). |
| Novos leitores | `_veto_da_regua` e `chamados_vivos_em_dia_vetado` consultam o juiz ([models.py:896](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/models.py:896), [furos_vetados.py:98](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_vetados.py:98)). |

Leitores indiretos relevantes:

- `furos_vetados_por_regua` muda o comando homônimo e seu número no placar ([furos_vetados_por_regua.py:27](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/furos_vetados_por_regua.py:27), [placar_code.sh:209](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/bin/placar_code.sh:209)).
- `furos_sem_cobranca.medir` muda a saída dos comandos de vigia e cobrança, além do placar; com `--apply`, dias classificados como veto deixam de ser enviados ao cartório ([furos_sem_cobranca_viva.py:14](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/furos_sem_cobranca_viva.py:14), [cobrar_furos_sem_cobranca.py:19](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19), [cobrar_furos_sem_cobranca.py:30](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30), [placar_code.sh:277](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/bin/placar_code.sh:277)).
- `desfazer_corroboracao_regua` lê a lista ampliada, mas filtra por vínculo existente; os novos itens com `None` são excluídos, portanto sem mudança efetiva inferível nesse leitor ([desfazer_corroboracao_regua.py:38](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/escala/management/commands/desfazer_corroboracao_regua.py:38)).

Leitores dos gates efetivamente atingidos pelo universo:

- `abrir`: detector de ausências, reconciliador da grade, c2, alerta de turno e emissor retroativo ([detectar_ausencias.py:688](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/detectar_ausencias.py:688), [reconciliar_grade.py:499](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/reconciliar_grade.py:499), [reconciliador.py:862](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/reconciliador.py:862), [processar_alertas_turno.py:335](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/processar_alertas_turno.py:335), [emitir_turno_aberto_retroativo.py:151](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151)).
- `criar`: alerta avançado N5 pode agora receber `None` em régua suspeita/degenerada; já trata `None` e abandona push/fio ([processar_alertas_avancados.py:121](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/processar_alertas_avancados.py:121), [processar_alertas_avancados.py:141](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/processar_alertas_avancados.py:141)).
- `_renascer_por_premissa_viva`: leitor interno `abrir` mantém o encerrado e devolve `(existente, False)`; leitor externo `resposta_tardia.receber` agora segue o ramo de nota/trilha/Pauta em dia acusado, porém vetado ([models.py:686](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/models.py:686), [resposta_tardia.py:45](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/services/resposta_tardia.py:45), [resposta_tardia.py:52](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/services/resposta_tardia.py:52)).
- A view de resposta devolve imediatamente `estado=resposta_tardia`; assim, não chega ao processamento seguinte de hora/escape ([views.py:1053](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/views.py:1053), [views.py:1063](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/views.py:1063)). Leitor dessa função na API: [não verificado].

Achados concretos adicionais:

1. **Alerta continua depois do renascimento vetado.** Entrada: chamado `turno_aberto` resolvido para o dia, régua agora suspeita, `pos_20` ainda não registrado. `abrir` recusa renascer, mas retorna o chamado encerrado; `_abrir_chamado_critico` o devolve, `_marco_critico` só testa `None` e prossegue com alerta e justificativa automática ligada ao cadáver ([models.py:686](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/models.py:686), [processar_alertas_turno.py:387](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/processar_alertas_turno.py:387), [processar_alertas_turno.py:316](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/processar_alertas_turno.py:316), [processar_alertas_turno.py:391](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/processar_alertas_turno.py:391)).

2. **Contador inventa um vínculo para ausência de vínculo.** Entrada: três células vetadas sem vínculo → cada achado tem `vinculo=None`, o comando adiciona `None` ao conjunto e imprime `furos_vetados_por_regua_vinculos=1`; são zero vínculos existentes ([furos_vetados.py:61](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/services/furos_vetados.py:61), [furos_vetados_por_regua.py:32](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/management/commands/furos_vetados_por_regua.py:32)).

3. **Emissor sem dia passa pelo gate sem aplicar veto.** `ponto.views` cria `entrada_bloqueada_turno_aberto` com `pendentes` resumidos e `total`, sem data; `_veto_da_regua` devolve `None` quando `dia_do_fato(fortes=True)` não responde. Entrada: tentativa de entrada com turno antigo aberto e régua vetada → chamado do universo ainda nasce ([views.py:280](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/views.py:280), [views.py:291](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/ponto/views.py:291), [models.py:892](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/models.py:892)).

Não sustentar crash da abertura manual atual: o formulário aceita somente `tipos_para_colaborador`, e nenhum dos cinco módulos do universo está marcado como abrível por humano no catálogo. A chamada genérica `criar` não distingue humano, mas sua exposição atual pelo formulário para esses módulos não está comprovada ([abertura.py:149](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/services/abertura.py:149), [modulos.py:105](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/catalogo/modulos.py:105), [modulos.py:182](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/catalogo/modulos.py:182), [modulos.py:306](/home/ronald/codex_ws/61ae629ccda4be2a4b6dc8199390290c5164ca34/app/chamados/catalogo/modulos.py:306)).

## 30/10 17:27 · sessao 4bed1eaae6ff · - · - · origem=Ronald (tmux)

Resultados de (c) e (d), só leitura.

**(c) O selo AST deixa passar mutantes plausíveis.** Executei `varrer_fonte` extraído do arquivo em memória, carregando o literal real `VIVOS`; os resultados abaixo vêm do algoritmo de `app/chamados/tests/test_o261_veto_na_porta.py:147`, com estados definidos em `app/chamados/catalogo/motor.py:24`.

| Mutante | Resultado |
|---|---|
| `from chamados.models import ChamadoColaborador as Ch; Ch.objects.create(...)` | Detectado. |
| `from .models import ChamadoColaborador as Ch; Ch.objects.create(...)` | Detectado: `ImportFrom.level` não impede o reconhecimento. |
| `CC=ChamadoColaborador; CC.objects.create(...)` | Detectado. |
| `from chamados.models import ChamadoColaborador as Ch; CC=Ch; CC.objects.create(...)` | **Não detectado:** retorna `([],[])`. |
| `from . import models as m; m.ChamadoColaborador.objects.create(...)` | **Não detectado:** retorna `([],[])`. |
| `manager=ChamadoColaborador.objects; manager.create(...)` | **Não detectado:** retorna `([],[])`. |
| `ch=ChamadoColaborador(...); ch.save()` | **Não detectado:** retorna `([],[])`. |
| `ChamadoColaborador.objects.bulk_create([ChamadoColaborador(...)])` | Detectado. |
| `qs=ChamadoColaborador.objects.all(); qs.bulk_create([ChamadoColaborador(...)])` | **Não detectado:** retorna `([],[])`. |
| `ChamadoColaborador.objects.filter(...).update(**{'status_local':'aberto'})` | Detectado. |
| `kwargs={'status_local':'aberto'}; ChamadoColaborador.objects.filter(...).update(**kwargs)` | **Não detectado:** retorna `([],[])`. |
| `def f(ch): ch.reabrir()` | **Não detectado:** retorna `([],[])`. |
| `ChamadoColaborador.objects.create(...)` no escopo do módulo | **Não detectado:** retorna `([],[])`. |
| `mv=motivo_do_veto; if mv(...): return; ChamadoColaborador.abrir(...)` | **Cópia de veto não detectada:** retorna `([],[])`. |

As causas e contra-exemplos podem ser agrupados:

1. **Rastreamento incompleto de aliases** — `app/chamados/tests/test_o261_veto_na_porta.py:87`. A atribuição só reconhece literalmente `v.id == MODELO`; não propaga aliases de modelo, manager, queryset ou função de veto. Entrada: `Ch` importado com alias, seguido de `CC=Ch` → `CC.objects.create(modulo_origem='batida_ausente', ...)` grava fora da porta e o selo não acusa. O alias simples importado e o import relativo direto são reconhecidos, portanto nesses dois casos específicos: **sem achado** (`:92`, `:100`, `:168`).

2. **Construtor seguido de `save()` é invisível** — `app/chamados/tests/test_o261_veto_na_porta.py:28` e `:170`. Entrada: `ch=ChamadoColaborador(colaborador=c, modulo_origem='batida_ausente', contexto_json={'data':'2026-10-20'}); ch.save()` → nascimento fora da porta, sem achado AST. O próprio comentário declara que `save` fica fora, e o selo vizinho só procura atribuição `.status_local =` ou `update(status_local=...)`; o exemplo não contém nenhuma delas (`app/chamados/tests/test_contract_status_nucleo.py:7`, `:23`). Não há override `save()` em `ChamadoColaborador`; os gates de nascimento usam managers em `app/chamados/models.py:735` e `:850`.

3. **`update(**kwargs)` perde a vitalidade** — `app/chamados/tests/test_o261_veto_na_porta.py:140`. Entrada: `kwargs={'status_local':'aberto'}; ChamadoColaborador.objects.filter(pk=1).update(**kwargs)` → reabertura direta, selo vazio. `_vivifica` só inspeciona constantes dentro da chamada, sem resolver a variável definida antes (`:143`). O formato inline é detectado; nesse formato: **sem achado** (`:144`).

4. **Reabertura por instância não entra na fiscalização de escrita** — `app/chamados/tests/test_o261_veto_na_porta.py:165`. Entrada: função automática recebe `ch` encerrado e executa `ch.reabrir()` → o scanner não reconhece a instância como modelo e tampouco classifica `reabrir` como escrita. Essa chamada escreve por `_gravar_estado` em `app/chamados/models.py:1158`, sem passar pelo veto acrescentado em `_renascer_por_premissa_viva` (`:778`).

5. **Escopo de módulo e lambdas não são inspecionados como escritores** — `app/chamados/tests/test_o261_veto_na_porta.py:109` e `:131`. Entrada: `ChamadoColaborador.objects.create(...)` diretamente no corpo de um módulo de produção → nenhuma função é visitada, lista vazia. O scanner só percorre corpos das funções enumeradas (`:152`).

6. **Allowlist é por função, não por sítio** — `app/chamados/tests/test_o261_veto_na_porta.py:194`. Entrada: inserir outra escrita direta dentro de `0038...::desfazer`, já permitido → `set(escritas)` continua com o mesmo nome, e o teste de igualdade continua verde (`:41`, `:200`). A contagem de sítios que `varrer_fonte` preserva é descartada por `varrer` (`:148`, `:194`).

**(d) Vacuidade efetiva versus cobertura faltante.**

- **`test_MORDE_criar_tambem_veta`: não há vacuidade efetiva comprovada.** A fixture não cria `CelulaDia` (`app/chamados/tests/test_o261_veto_na_porta.py:255`); a regeneração de células em signals de vínculo retorna em `created=True` (`app/escala/signals.py:54`, `:104`), e a absolvição anterior exige `celula_id` (`app/chamados/models.py:865`). Logo, não sustentar que o `None` observado viria necessariamente do guarda antigo. Falta o controle positivo `criar` no mesmo módulo/dia (`app/chamados/tests/test_o261_veto_na_porta.py:295`), mas passagem desse caso no código antigo por outra recusa: **[não verificado]**.

- **`_renascer` não é vácuo pelas guardas anteriores.** O caso vetado usa `entrada_bloqueada_turno_aberto` (`app/chamados/tests/test_o261_veto_na_porta.py:324`), e existe controle sem veto com o mesmo módulo (`:338`). O colaborador nasce ativo por default (`app/colaboradores/models.py:323`). Execução dos testes Django: **[não verificado]**.

- **EC ausente só é testado no juiz, não na porta.** `test_sem_vinculo_veta` chama diretamente `motivo_do_veto(self.colab, None, ...)` enquanto a fixture mantém um vínculo válido (`app/chamados/tests/test_o261_veto_na_porta.py:266`, `:302`). Contra-exemplo de mutante sobrevivente: `_veto_da_regua` resolve `ec`, faz `if ec is None: return None` e chama o juiz nos demais casos → todas as entradas atuais da porta continuam com vínculo; o teste direto do juiz permanece verde. É falha de cobertura da integração EC-NONE, não teste unitário do juiz vácuo (`app/chamados/models.py:896`).

- **Dois módulos do universo não são exercitados pelos testes da porta.** O universo inclui `batida_ausente` e `intervalo_pendente` (`app/chamados/catalogo/furo_modulos.py:15`), mas os casos de porta usam os três módulos de turno (`app/chamados/tests/test_o261_veto_na_porta.py:269`, `:298`, `:316`, `:324`). Contra-exemplo: `_veto_da_regua` retorna `None` especificamente para aqueles dois módulos → a bateria nova continua sem entrada que detecte essa retirada da proteção. É cobertura faltante; não demonstra vacuidade dos casos existentes.

## 30/10 17:27 · sessao 26d3e2e868c9 · - · - · origem=Ronald (tmux)

Achados do selo e dos testes, com execução apenas de AST em memória: [nao verificado]

1. **O selo aceita escritores fora da porta.** `_apelidos` reconhece `ImportFrom` e atribuição cujo valor literal é `ChamadoColaborador`; não acompanha alias em cadeia, módulo ou manager. `_raiz` retorna o primeiro nome da cadeia, e o varredor exige que esse nome esteja nos modelos conhecidos. Contraexemplos confirmados, todos com resultado `([], [])`:  
   - `from chamados.models import ChamadoColaborador as Ch; Alias = Ch; def f(c): Alias.objects.create(...)`;
   - `from . import models; def f(c): models.ChamadoColaborador.objects.create(...)`;
   - `mgr = ChamadoColaborador.objects; mgr.bulk_create([...])`.  
   Assim, um desses caminhos pode criar `turno_aberto` com dia vetado sem passar pelo veto, sem aumentar achados/allowlist. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:65`, `:87`, `:98`, `:165`, `:168`, `:170`.

2. **Construtor + `save()` passa.** `ch = ChamadoColaborador(colaborador=c, modulo_origem="turno_aberto", contexto_json={"data_turno": d}); ch.save()` devolve `([], [])`: `save` não pertence a `CRIAR`, e chamadas de construtor são `ast.Name`, descartadas antes da classificação. O selo antigo de status também não pega esse exemplo, pois procura `.status_local =` e `update(status_local=...)`, não nascimento por construtor. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:28`, `:163`, `:170`; `app/chamados/tests/test_contract_status_nucleo.py:7`, `:23`.

3. **`update(**kwargs)` variável passa.** Entrada:
   ```python
   campos = {"status_local": "aberto"}
   ChamadoColaborador.objects.filter(pk=1).update(**campos)
   ```
   Resultado confirmado: `([], [])`. `_vivifica` só reconhece keyword explícita ou string literal presente dentro da chamada; não segue a variável. Um chamado encerrado vetado pode ser reaberto por esse mutante sem mexer na allowlist. Referência: `app/chamados/tests/test_o261_veto_na_porta.py:137`.

4. **Reabertura por instância não é fiscalizada.** `ch = ChamadoColaborador.objects.get(pk=1); ch.reabrir()` também resulta `([], [])`. `PEDIDOS_A_PORTA` só participa da identificação de cópia de veto e ainda exige raiz igual ao nome do modelo; reabrir não é classificado como escrita. Isso permite novo automático usando `reabrir()` direto, sem pedir `_renascer_por_premissa_viva`. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:30`, `:166`, `:170`; `app/chamados/models.py:1092`, `:1158`.

5. **Outras duas cegueiras confirmadas:** escrita em nível de módulo e escrita dentro de lambda retornam listas vazias. Só funções são varridas; lambdas são explicitamente puladas. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:109`, `:131`, `:152`.

6. **Catraca não garante “só desce”.** O teste exige apenas quantidade atual ≤ 5. Se um lote reduzir para 4 e outro recolocar um sítio, voltando para 5, o teste permanece verde, desde que o varredor encontre a lista atual. A comparação não contém limite anterior nem identidade histórica. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:54`, `:194`, `:200`, `:205`, `:208`.

7. **EC-NONE não tem caso funcional da porta.** `test_sem_vinculo_veta` só chama o predicado diretamente, enquanto a fixture cria vínculo vigente. Mutante em `_veto_da_regua`: obter `ec`, devolver `None` se `ec is None`, mantendo `motivo_do_veto(None)` correto. O teste EC-NONE continuaria verde e os demais casos usam vínculo existente; nascimento/reabertura sem vínculo passariam. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:266`, `:302`; `app/chamados/models.py:896`. Essa conclusão é por leitura; execução da suíte Django [não verificado].

8. **Dois módulos ficam sem exercício funcional nessa suíte:** `batida_ausente` e `intervalo_pendente`; os casos reais da porta usam `turno_aberto`, `turno_aberto_24h` e `entrada_bloqueada_turno_aberto`. Mutante excluindo os dois primeiros de `_veto_da_regua` não seria distinguido por esses testes. Referências: `app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:269`, `:298`, `:316`, `:324`.

**Sem achado nos casos diretos específicos:** `from .models import ChamadoColaborador as Ch`, `ChamadoColaborador.objects.bulk_create(...)` e `update(**{"status_local": "aberto"})` foram detectados pelo varredor em memória. As regras responsáveis estão em `app/chamados/tests/test_o261_veto_na_porta.py:92`, `:143`, `:170`.

**Sem achado de vacuidade nos casos de suspeita/renascer apresentados:** há nascimento sem veto, trilha exigida com motivo `VETO_REGUA` e renascimento sem veto como controles. Referências: `app/chamados/tests/test_o261_veto_na_porta.py:279`, `:290`, `:338`. O AST da árvore atual reproduziu exatamente as duas allowlists; isso não elimina as cegueiras acima (`app/chamados/tests/test_o261_veto_na_porta.py:194`).

## 30/10 17:27 · sessao 0c0748b99169 · - · - · origem=Ronald (tmux)

Censo e achados, somente pelo código:

1. **Reabertura automática da grade contorna o veto.** `app/ponto/management/commands/reconciliar_grade.py:499` chama `abrir`; se já houver encerrado, `app/chamados/models.py:686` tenta `_renascer_por_premissa_viva`, mas descarta o resultado e devolve o encerrado em `:688`. A grade chama então `ch.reabrir()` diretamente em `app/ponto/management/commands/reconciliar_grade.py:522`; esse método não consulta `_veto_da_regua` (`app/chamados/models.py:1119`).
   
   Contraexemplo: `_emitir_furo(c, dia, marks, ec_saudavel)` com chamado `batida_ausente` resolvido, premissa ainda furada, enquanto `vinculo_do_emissor(c, dia)` resolve vínculo suspeito. Os vetos externos passam (`app/ponto/management/commands/reconciliar_grade.py:487` e `:491`); renascer recusa (`app/chamados/models.py:778`), mas a segunda chamada direta reabre (`app/ponto/management/commands/reconciliar_grade.py:522`). Divergência pode ser fornecida na entrada desse método; ocorrência em produção [não verificado].

2. **Nascimento automático `entrada_bloqueada_turno_aberto` escapa semanticamente do veto por ausência de dia.** `app/ponto/views.py:280` monta contexto somente com `pendentes` textual e `total`; `:291` chama `_Chamado.criar`, gravando esse contexto em `:301`. `_veto_da_regua` retorna sem veto quando não encontra dia (`app/chamados/models.py:892`). 
   
   Contraexemplo: colaborador tenta entrada com pendências de turnos de dias vetados; contexto `{'pendentes': '20/10/2026 08:00', 'total': 1}` → chamado automático nasce, porque nenhuma chave forte de dia existe (`app/ponto/views.py:280`, `app/chamados/models.py:892`). Não é escritor fora de `criar`; é falha de cobertura do veto dentro da porta. O teste explicitamente espera esse nascimento (`app/chamados/tests/test_o261_veto_na_porta.py:313`).

3. **Censo direto de leitores afetáveis da porta**:
   
   | Leitor | Efeito novo |
   |---|---|
   | `app/ponto/management/commands/detectar_ausencias.py:688` | Pode receber `None` em novo nascimento; retorna `VETO_NA_PORTA` em `:732`. Encerrado pode permanecer encerrado e retorna `VETO_ENCERRADO_NAO_RENASCEU` em `:734`. |
   | `app/ponto/management/commands/reconciliar_grade.py:499` | Novo nascimento recusado retorna `VETO_NA_PORTA` em `:519`; encerrado recusado é reaberto diretamente em `:522`, achado acima. |
   | `app/ponto/management/commands/processar_alertas_turno.py:335` | Novo nascimento pode retornar `None`; `_marco_critico` abandona alerta/push/justificativa em `:317`. |
   | `app/ponto/management/commands/processar_alertas_avancados.py:121` | Novo `turno_aberto_24h` pode ser recusado e o comando salta o restante em `:141`. |
   | `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Pode deixar de emitir e passa a contar `recusado_veto` em `:174`; dry consulta novo veto em `:138`. |
   | `app/chamados/reconciliador.py:862` | Reemissão pode retornar `None`; nova guarda devolve `False` em `:885`. |
   | `app/chamados/models.py:686` | Dedup de encerrado pode deixar de renascer, devolvendo o encerrado em `:688`. |
   | `app/chamados/services/resposta_tardia.py:45` | Pode deixar de reabrir mesmo com célula acusando; vira nota tardia em `:52`, eventualmente Pauta em `:58`. |

   Leitores transitivos do detector: `app/ponto/management/commands/detectar_ausencias.py:439`; `app/ponto/management/commands/emitir_furo_retroativo.py:142`; `app/ponto/services/cartorio.py:1358` e `:1394`. O retroativo ignora o retorno e incrementa emitidos incondicionalmente (`app/ponto/management/commands/emitir_furo_retroativo.py:143`), portanto uma nova recusa na porta pode continuar aparecendo como emissão.

   Leitor transitivo da resposta tardia: `app/chamados/views.py:1053`; estado `tardia` encerra a resposta HTTP antes da declaração normal (`:1054`). Contraexemplo: chamado encerrado, célula ainda acusa, vínculo suspeito e texto livre → antes podia renascer e seguir para declaração; agora registra nota e Pauta (`app/chamados/services/resposta_tardia.py:45`, `:52`, `:58`).

4. **Demais chamadas de produção de `abrir/criar`: sem mudança funcional pelo veto, por módulo fora do universo.** Incluem os gêmeos `app/api/views.py:746` e `app/api/views_core.py:746` (`batida_fora_escala_leve`), abertura rápida `app/chamados/views.py:973` (`mensagem_dp`), disputas `app/chamados/services/disputa_emissao.py:504`, `:908`, `:1128`, adesão `app/chamados/services/adesao.py:74`, ausência `app/ponto/services/ausencia.py:643`, e outros módulos igualmente externos; a exclusão ocorre em `app/chamados/models.py:889`.

5. **Correção importante sobre abertura manual:** não há contraexemplo atual comprovado de admin abrindo um dos cinco módulos pelo formulário. `abrir_chamado_para_colaborador` valida o catálogo antes de `criar` (`app/chamados/services/abertura.py:149`); os cinco módulos não possuem `abrivel_por_humano` (`app/chamados/catalogo/modulos.py:105`, `:107`, `:109`, `:111`, `:182`). Portanto não sustentar um suposto `None.pk` em `app/chamados/services/abertura.py:195` como bug atualmente alcançável por esse formulário.

6. **Outros `reabrir()` diretos automáticos merecem constar como cobertura ausente:** `app/chamados/services/disputa_emissao.py:1606` reabre o pai para resposta aguardando validação; não restringe o módulo do pai (`:1601`). Contraexemplo estrutural: disputa aberta cujo pai seja um `turno_aberto` encerrado, com resposta ainda creditada e dia vetado → chama reabrir sem veto. Existência dessa forma de pai na produção [não verificado]. Os outros `reabrir` automáticos de adesão e detector de limbo trabalham módulos fora do universo (`app/chamados/services/adesao.py:75`, `app/ponto/management/commands/detectar_ausencias.py:273`).

## 30/10 17:27 · sessao f983fcd63a51 · - · - · origem=Ronald (tmux)

Achados do selo e testes, somente por leitura e execução AST em memória, sem Django/banco: [nao verificado]

1. **Selo deixa nascer fora da porta por construtor+save.** Em `app/chamados/tests/test_o261_veto_na_porta.py:168-173`, só métodos de manager das listas `CRIAR`/`VIVIFICAR` são contados; `save()` fica fora. Contra-exemplo:

   ```python
   from chamados.models import ChamadoColaborador as Ch
   def f(c):
       obj = Ch(colaborador=c, modulo_origem='turno_aberto',
                contexto_json={'data_turno': '2026-10-20'})
       obj.save()
   ```

   `varrer_fonte` devolveu `([], [])`. O chamado nasce `aberto` pelo default de `app/chamados/models.py:46`, sem `_veto_da_regua`. O selo citado como complementar também não acusa: `app/chamados/tests/test_contract_status_nucleo.py:7-9` procura atribuição `.status_local =` ou `update(status_local=...)`, ausentes nesse mutante.

2. **Aliases deixam passar create/bulk_create.** `_apelidos` só reconhece `ImportFrom` do nome do modelo e atribuição cujo RHS é literalmente `ChamadoColaborador` (`app/chamados/tests/test_o261_veto_na_porta.py:92-104`). Mutantes comprovados retornando `([], [])`:

   ```python
   from . import models as m
   def f(c):
       m.ChamadoColaborador.objects.create(colaborador=c)
   ```

   ```python
   from chamados.models import ChamadoColaborador as Ch
   C = Ch
   def f(c):
       C.objects.create(colaborador=c)
   ```

   ```python
   from chamados.models import ChamadoColaborador as Ch
   def f(c):
       manager = Ch.objects
       manager.bulk_create([Ch(colaborador=c)])
   ```

   Import direto com `as`, import relativo `from .models import ChamadoColaborador as Ch` e `Ch.objects.bulk_create(...)` **são detectados** (`app/chamados/tests/test_o261_veto_na_porta.py:92-95`, `:170-171`), comprovado pela mesma execução.

3. **`update(**kwargs)` variável passa.** `_vivifica` procura `status_local` somente nos keywords ou constantes dentro da chamada (`app/chamados/tests/test_o261_veto_na_porta.py:137-144`). Entrada:

   ```python
   from chamados.models import ChamadoColaborador as Ch
   def f(c):
       kw = {'status_local': 'aberto'}
       Ch.objects.filter(pk=c.pk).update(**kw)
   ```

   Resultado comprovado: `([], [])`; pode vivificar chamado vetado sem porta. `update(**{'status_local': 'aberto'})` inline é detectado.

4. **Cópia do veto antes de reabrir uma instância fica invisível.** A detecção exige que a raiz do pedido à porta pertença ao conjunto de nomes do modelo (`app/chamados/tests/test_o261_veto_na_porta.py:165-167`).

   ```python
   from chamados.models import ChamadoColaborador
   from ponto.services.furos_vetados import motivo_do_veto as mv
   def f(ch, c, ec, d):
       if not mv(c, ec, d):
           ch.reabrir()
   ```

   Resultado comprovado: `([], [])`, apesar do veto externo e reabertura automática.

5. **Teste EC-NONE não testa a porta.** `test_sem_vinculo_veta` chama apenas o juiz (`app/chamados/tests/test_o261_veto_na_porta.py:302-306`). A fixture de todos os testes de porta tem vínculo (`:266-267`). Mutante em `_veto_da_regua`:

   ```python
   ec = vinculo_do_emissor(colab, dia)
   if ec is None:
       return None
   motivo = motivo_do_veto(colab, ec, dia)
   ```

   Deixa `abrir`, `criar` e `renascer` passarem sem vínculo, contrariando o aval; não é exercitado por nenhum caso de porta desse arquivo. O juiz continua correto e `test_sem_vinculo_veta` continua verde. Execução da suíte em banco: **[não verificado]**; lacuna demonstrável pelos argumentos dos testes e por `app/chamados/models.py:895-897`.

6. **Catraca não garante “só desce”.** `app/chamados/tests/test_o261_veto_na_porta.py:208-209` só confere soma `<=5`; trocar um nome por outro mantém verde. Além disso, `varrer` deduplica por função (`:194`): acrescentar outra escrita dentro de função já allowlisted não altera resultado, embora permita novo caminho de escrita.

7. **Limites dos demais testes.** O par de reabertura com/sem veto tem controle positivo correspondente (`app/chamados/tests/test_o261_veto_na_porta.py:320-343`): sem achado de vacuidade nesse par. `criar` só verifica `None` no caso suspeito, sem positivo correspondente do mesmo módulo (`:295-300`); vacuidade efetiva nesse fixture **[não verificado]**. `batida_ausente` e `intervalo_pendente`, que pertencem ao universo (`app/chamados/catalogo/furo_modulos.py:15-16`), não têm caso de veto da porta neste arquivo (`app/chamados/tests/test_o261_veto_na_porta.py:269-353`).

A varredura real em memória devolveu exatamente as duas funções de migration e três cópias já allowlisted; portanto os casos negativos atuais do selo não revelam os escapes acima (`app/chamados/tests/test_o261_veto_na_porta.py:40-54`, `:198-206`).

## 30/10 17:27 · sessao 3489199c380a · - · - · origem=Ronald (tmux)

Censo de leitores cujo resultado pode mudar:

- `motivo_do_veto`: [furos_vetados.py:57](app/ponto/services/furos_vetados.py:57), [cartorio.py:1013](app/ponto/services/cartorio.py:1013), [furos_sem_cobranca.py:96](app/ponto/services/furos_sem_cobranca.py:96), [reconciliador.py:854](app/chamados/reconciliador.py:854); novos leitores em [models.py:896](app/chamados/models.py:896) e [furos_vetados.py:98](app/ponto/services/furos_vetados.py:98).
- `abrir`: [detectar_ausencias.py:688](app/ponto/management/commands/detectar_ausencias.py:688), [reconciliar_grade.py:499](app/ponto/management/commands/reconciliar_grade.py:499), [reconciliador.py:862](app/chamados/reconciliador.py:862), [processar_alertas_turno.py:335](app/ponto/management/commands/processar_alertas_turno.py:335), [emitir_turno_aberto_retroativo.py:151](app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151).
- `criar`: [processar_alertas_avancados.py:121](app/ponto/management/commands/processar_alertas_avancados.py:121).
- `_renascer_por_premissa_viva`: dedup em [models.py:686](app/chamados/models.py:686); resposta tardia em [resposta_tardia.py:45](app/chamados/services/resposta_tardia.py:45). Com veto, esse último deixa de retornar `reaberto` e segue para nota/pauta tardia, [resposta_tardia.py:52](app/chamados/services/resposta_tardia.py:52).

Achados adicionais:

1. **Contador passa a inventar um vínculo.** O serviço agora devolve `vinculo=None` para célula sem vínculo, [furos_vetados.py:61](app/ponto/services/furos_vetados.py:61), mas o comando insere esse valor no conjunto de vínculos e imprime seu tamanho, [furos_vetados_por_regua.py:32](app/ponto/management/commands/furos_vetados_por_regua.py:32). Contra-exemplo: três células `furo`, todas com `vinculo_do_emissor=None` → três vetados e `furos_vetados_por_regua_vinculos=1`, embora nenhum vínculo exista.

2. **Mudança do juiz alcança passivo e julgamento, além do nascimento.** Entrada: célula estável de colaborador ativo, `ec=None`. Antes, veto ausente; depois, `VETO_DEGENERADA`. O cartório inclui o veto na impressão e deixa de reconhecer a impressão antiga como estável, [cartorio.py:1083](app/ponto/services/cartorio.py:1083), [cartorio.py:1092](app/ponto/services/cartorio.py:1092). O passivo passa para porta `veto`, sai do total e deixa de ser rejulgado pelo cobrador, [furos_sem_cobranca.py:96](app/ponto/services/furos_sem_cobranca.py:96), [furos_sem_cobranca.py:109](app/ponto/services/furos_sem_cobranca.py:109), [furos_sem_cobranca.py:125](app/ponto/services/furos_sem_cobranca.py:125). Leitores indiretos afetados: [furos_sem_cobranca_viva.py:14](app/ponto/management/commands/furos_sem_cobranca_viva.py:14) e [cobrar_furos_sem_cobranca.py:19](app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19).

3. **“DIFF zero por construção” não decorre do código dos emissores de turno.** `processar_alertas_avancados` exige vínculo, mas não pergunta veto de régua antes de criar, [processar_alertas_avancados.py:37](app/ponto/management/commands/processar_alertas_avancados.py:37), [processar_alertas_avancados.py:121](app/ponto/management/commands/processar_alertas_avancados.py:121). Contra-exemplo: vínculo existente, turno N5 elegível, célula não absolve e régua suspeita → antes cria; novo gate devolve `None`, [models.py:838](app/chamados/models.py:838). O mesmo alcance existe no emissor `processar_alertas_turno`, [processar_alertas_turno.py:335](app/ponto/management/commands/processar_alertas_turno.py:335). Existência desses casos em produção: [não verificado].

Sem alteração de resultado comprovada nestes leitores:

- `ponto/views.py:291` cria módulo do universo sem dia: seu contexto contém apenas `pendentes` e `total`, [views.py:280](app/ponto/views.py:280); a porta retorna antes de julgar veto quando não encontra dia, [models.py:892](app/chamados/models.py:892).
- `smoke_chamados` cria módulo `smoke`, fora do universo, [smoke_chamados.py:123](app/chamados/management/commands/smoke_chamados.py:123).
- A abertura manual para colaborador não admite atualmente esses cinco módulos pelo catálogo: eles não têm `abrivel_por_humano`, [modulos.py:105](app/chamados/catalogo/modulos.py:105), [modulos.py:182](app/chamados/catalogo/modulos.py:182), e a seleção filtra esse campo, [modulos.py:281](app/chamados/catalogo/modulos.py:281). Portanto, **não confirmei** o suposto `None.pk` nessa UI.
- `desfazer_corroboracao_regua` filtra os achados pelo vínculo existente, descartando `None`; sem mudança de resultado demonstrada, [desfazer_corroboracao_regua.py:38](app/escala/management/commands/desfazer_corroboracao_regua.py:38).

## 30/10 17:26 · sessao 8687781eb298 · - · - · origem=Ronald (tmux)

Achados do selo e testes, conferidos por AST puro em memória, sem Django/banco: [nao verificado]

1. **Selo deixa escritores fora da porta passarem.** `_apelidos` só reconhece import direto da classe, atribuição do nome literal `ChamadoColaborador` e `apps.get_model`; `varrer_fonte` exige raiz reconhecida e método de manager listado. Fontes: `app/chamados/tests/test_o261_veto_na_porta.py:87`, `:98`, `:165`, `:170`.

   Contraexemplos que retornaram `([], [])` em `varrer_fonte`:

   ```python
   from . import models as m
   def emitir(c):
       m.ChamadoColaborador.objects.create(
           colaborador=c, modulo_origem="turno_aberto")
   ```

   ```python
   from chamados.models import ChamadoColaborador as Ch
   C = Ch
   def emitir(c):
       C.objects.create(colaborador=c, modulo_origem="turno_aberto")
   ```

   ```python
   from chamados.models import ChamadoColaborador
   def emitir(c):
       ch = ChamadoColaborador(colaborador=c, modulo_origem="turno_aberto")
       ch.save()
   ```

   O último nasce aberto pelo default do modelo, sem chamar veto; o selo antigo de status também não acusa porque não há atribuição `.status_local =` nem `update(status_local=...)`. Fontes: `app/chamados/models.py:46`; `app/chamados/tests/test_contract_status_nucleo.py:7`; `app/chamados/tests/test_o261_veto_na_porta.py:28`.

2. **`update(**kwargs)` e `bulk_create` por manager destacado escapam.** `_vivifica` procura string literal apenas dentro da chamada, sem acompanhar a variável; manager destacado perde raiz reconhecida. Fontes: `app/chamados/tests/test_o261_veto_na_porta.py:137`, `:143`, `:168`.

   ```python
   def ressuscitar():
       campos = {"status_local": "aberto"}
       ChamadoColaborador.objects.filter(pk=1).update(**campos)
   ```

   ```python
   def emitir(c):
       manager = ChamadoColaborador.objects
       manager.bulk_create([
           ChamadoColaborador(colaborador=c, modulo_origem="turno_aberto")
       ])
   ```

   Ambos deram `([], [])`: entrada em dia vetado → escrita viva sem porta, selo verde. Os testes de mutantes só cobrem chamadas diretas e não esses formatos. Fonte: `app/chamados/tests/test_o261_veto_na_porta.py:217`.

3. **Cópia externa seguida de reabertura em instância também fica invisível.** `pede_porta` exige raiz pertencente aos nomes da classe; `ch.reabrir()` não satisfaz. Fonte: `app/chamados/tests/test_o261_veto_na_porta.py:166`.

   ```python
   def emitir(c, ec, d):
       if motivo_do_veto(c, ec, d):
           return
       ch = ChamadoColaborador.objects.get(pk=1)
       ch.reabrir()
   ```

   Deu `([], [])`. Alias transitivo do juiz (`vv = motivo_do_veto` ou alias importado) igualmente escapa da detecção da cópia. Fonte: `app/chamados/tests/test_o261_veto_na_porta.py:96`, `:159`.

4. **Sem achado nos formatos diretos:** `from .models import ChamadoColaborador as Ch`; `Ch.objects.bulk_create(...)`; `Ch.objects.update(**{"status_local": "aberto"})` foram detectados. Fontes: `app/chamados/tests/test_o261_veto_na_porta.py:92`, `:143`, `:170`.

5. **VETO-EC-NONE não é testado na porta.** `test_sem_vinculo_veta` chama exclusivamente o juiz; toda fixture da porta mantém EC válido. Fontes: `app/chamados/tests/test_o261_veto_na_porta.py:266`, `:302`.

   Contraexemplo: mutante em `_veto_da_regua` que retorna `None` quando `vinculo_do_emissor` retorna `None` → nascimento/reabertura automática sem EC passa; teste `test_sem_vinculo_veta` continua verde porque chama diretamente `motivo_do_veto`. O trecho mutável fica em `app/chamados/models.py:895`. Execução da suíte com esse mutante: [não verificado]; insuficiência da cobertura é visível nas chamadas dos testes.

6. **Cobertura não sela os cinco módulos.** Não há caso de veto na porta para `batida_ausente` nem `intervalo_pendente`, integrantes de `FURO_MODULES`; os casos exercitam `turno_aberto`, `turno_aberto_24h` e `entrada_bloqueada_turno_aberto`. Fontes: `app/chamados/catalogo/furo_modulos.py:15`; `app/chamados/tests/test_o261_veto_na_porta.py:269`, `:298`, `:324`.

   Contraexemplo: mutante limita universo às três strings exercitadas → furo automático nos outros dois nasce vetado sem qualquer caso novo falhar. Execução da suíte mutante: [não verificado]. [nao verificado]

7. **Não achei vacuidade no par principal de renascimento.** Mesmo módulo, mesma fixture, controle positivo sem veto em `test_sem_veto_renasce`, negativo com suspeita em `test_MORDE_renascer...`; módulo escolhido está fora de `CALADOS_POR_ISENCAO`, evitando recusa por premissa regularizada. Fontes: `app/chamados/tests/test_o261_veto_na_porta.py:320`, `:338`; `app/chamados/models.py:984`; `app/chamados/catalogo/modulos.py:235`. O teste de ausência de dia é compatibilidade e passaria sem a nova guarda, portanto não serve de RED dela. Fonte: `app/chamados/tests/test_o261_veto_na_porta.py:313`.

O censo AST do snapshot devolveu exatamente as duas entradas de escrita e as três de cópia das allowlists; isso confirma a lista atual, mas os contraexemplos acima mostram que não certifica ausência de outros formatos. Fontes: `app/chamados/tests/test_o261_veto_na_porta.py:40`, `:46`, `:194`.

## 30/10 17:26 · sessao 72294520087e · - · - · origem=Ronald (tmux)

Censo da porta no universo atual:

| Leitor | Mudança possível |
|---|---|
| `app/ponto/management/commands/detectar_ausencias.py:688` | `abrir` pode recusar depois do pré-veto feito com o `ec` recebido; retorna `VETO_NA_PORTA` em `:733`. O vínculo da porta é rebuscado por `vinculo_do_emissor`, podendo diferir do recebido (`app/chamados/models.py:896`). |
| `app/ponto/management/commands/reconciliar_grade.py:499` | Mesma diferença de vínculo; trata `None` em `:519`, mas, recebendo encerrado cujo renascimento foi recusado, chama `reabrir()` diretamente em `:522`. |
| `app/chamados/reconciliador.py:862` | C2; já pergunta `motivo_do_veto` em `:854`, que também muda com `ec=None`; recusa em `:857`. |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | Turno com vínculo suspeito deixa de nascer; `_marco_critico` recebe `None` e evita alerta/justificativa (`:316–321`). Antes só a absolvição pela célula podia provocar essa recusa. |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | `turno_aberto_24h` com vínculo presente mas suspeito deixa de nascer; interrompe antes do push e fio em `:141–146`. O guard anterior exclui somente vínculo ausente (`:37–39`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Vínculo presente suspeito antes passava; agora a porta recusa. Dry também muda em `:138`; apply classifica recusa em `:174`. O guard anterior só exclui vínculo ausente (`:104–107`). |
| `app/chamados/models.py:686` | Dedup de `abrir` chama `_renascer_por_premissa_viva`, descarta seu resultado e devolve o encerrado em `:688`. |
| `app/chamados/services/resposta_tardia.py:45` | Com célula acusando, chamado encerrado e vínculo suspeito/ausente, antes retornava `reaberto` (`:47`); agora recusa, grava nota/log (`:52–56`), pode criar Pauta (`:58–66`) e retorna `tardia` (`:67`). |

Demais leitores de `motivo_do_veto`:

- `app/ponto/services/cartorio.py:1013`: altera impressão passada em `:1083`; `impressao_insumos` acrescenta `veto:VETO_DEGENERADA` em `:329–330`. Entrada: célula previamente julgada, impressão igual e vínculo ausente → deixa de satisfazer o skip de `:1092`, sendo rejulgada. Não se limita a nascimento/reabertura.
- `app/ponto/services/furos_sem_cobranca.py:96`: ativo, furo com protesto, sem vivo e sem vínculo → passa para porta `veto`; essa porta sai do total em `:109`, por definição em `:30`.
- `app/ponto/services/furos_vetados.py:57`: furo sem vínculo passa a aparecer com `vinculo=None` (`:61`). O command adiciona `None` ao conjunto e publica um vínculo inexistente (`app/ponto/management/commands/furos_vetados_por_regua.py:32`). Contraexemplo: dois furos sem vínculo → `furos_vetados_por_regua=2`, `furos_vetados_por_regua_vinculos=1`.
- Novos leitores: porta (`app/chamados/models.py:896`) e contador (`app/ponto/services/furos_vetados.py:98`).
- Indireto `desfazer_corroboracao_regua`: descarta os achados novos de vínculo `None` pelo filtro `a['vinculo'] == ec.pk`; sem alteração de resultado identificada (`app/escala/management/commands/desfazer_corroboracao_regua.py:38`).

Entradas atuais sem mudança efetiva:

- `app/ponto/views.py:291`: cria `entrada_bloqueada_turno_aberto` com contexto contendo apenas `pendentes`/`total` (`:280`); sem dia forte, o veto retorna antes de consultar o juiz (`app/chamados/models.py:892`).
- `app/chamados/services/abertura.py:47`: módulo variável, contexto atual sem dia (`:58`).
- `app/chamados/services/abertura.py:172`: módulo variável, mas os módulos do universo não estão habilitados para abertura humana (`app/chamados/catalogo/modulos.py:105`); `tipos_para_colaborador` exige `abrivel_por_humano` (`:281`, `:306`), e o serviço rejeita módulo não listado antes de criar (`app/chamados/services/abertura.py:149`). Portanto, **não está provado** o contraexemplo de admin abrir manualmente `batida_ausente` por essa UI e receber `None.pk`.

Não há dados fornecidos que permitam quantificar impacto real de produção desses contraexemplos: [não verificado].

## 30/10 17:26 · sessao ae467ecf8e80 · - · - · origem=Ronald (tmux)

Achados e censo de `motivo_do_veto`:

1. **Mudança em leitores fora da porta, incompatível com afirmar DIFF zero por construção.** `ec=None` agora retorna `VETO_DEGENERADA` (`app/ponto/services/furos_vetados.py:32`). Isso muda:
   - **Cartório:** `_veto_do_dia` lê o juiz (`app/ponto/services/cartorio.py:1013`), inclui o resultado na impressão (`app/ponto/services/cartorio.py:1083`); o texto `veto:VETO_DEGENERADA` entra no hash (`app/ponto/services/cartorio.py:329`). Contraexemplo: célula já julgada, sem vínculo, demais insumos iguais → impressão muda e deixa de passar pelo skip de estabilidade (`app/ponto/services/cartorio.py:1092`).
   - **Passivo:** `medir` passa a classificar sem vínculo como `veto` (`app/ponto/services/furos_sem_cobranca.py:96`), porta excluída do total (`app/ponto/services/furos_sem_cobranca.py:30`, `:109`). Contraexemplo: célula furo/protesto cobrável, ativa, sem chamado vivo, sem vínculo, sem outra exclusão → antes `pergunta_ao_colab`/`supervisao_sem_push`, total 1; depois `veto`, total 0 (`app/ponto/services/furos_sem_cobranca.py:107`, `:110`).
   - **Cobrança do passivo:** `cobrar` pula `veto` (`app/ponto/services/furos_sem_cobranca.py:125`), portanto o comando que chama `medir` e `cobrar` deixa de tentar esses itens (`app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`, `:30`). A impressão do comando de medida também muda (`app/ponto/management/commands/furos_sem_cobranca_viva.py:14`, `:17`) e seu placar lê esse total (`bin/placar_code.sh:277`).
   - **Lista CARTORIO-VETO:** passa a incluir furo sem vínculo (`app/ponto/services/furos_vetados.py:57`, `:61`), alterando o total do comando (`app/ponto/management/commands/furos_vetados_por_regua.py:27`, `:31`) e o placar (`bin/placar_code.sh:209`). Esses efeitos são demonstráveis no código; quantidade atingida em produção: [não verificado].

2. **Contador inventa um vínculo para ausência de vínculo.** A lista nova entrega `vinculo=None` (`app/ponto/services/furos_vetados.py:61`), mas o comando adiciona esse valor ao conjunto e imprime `len(vinculos)` (`app/ponto/management/commands/furos_vetados_por_regua.py:32`, `:33`). Contraexemplo: dois furos sem vínculo → `furos_vetados_por_regua=2`, `furos_vetados_por_regua_vinculos=1`, embora não exista nenhum vínculo.

3. **Vigia novo fica invisível no placar existente.** O comando imprime `chamados_vivos_em_dia_vetado` (`app/ponto/management/commands/furos_vetados_por_regua.py:49`), porém o placar filtra exclusivamente `^furos_vetados_por_regua=...$` (`bin/placar_code.sh:209`, `:210`) e exibe só essa métrica (`bin/placar_code.sh:211`). A configuração identifica esse placar como casa de produção, sem cron (`app/config/crons.py:1143`). Contraexemplo: um chamado vivo vetado, nenhuma célula furo → comando informa novo vigia 1; placar informa somente furos vetados 0.

**Demais leitores diretos de produção identificados:**

- `reemitir_chamado_do_dia`: consulta o juiz somente quando não costura em chamado vivo (`app/chamados/reconciliador.py:852`, `:854`); com `ec=None`, passa a retornar `False` pelo veto (`app/chamados/reconciliador.py:855`).
- `_veto_da_regua`: novo leitor da porta, dependente de módulo e dia (`app/chamados/models.py:889`, `:892`, `:896`).
- `chamados_vivos_em_dia_vetado`: novo leitor, agora inclui chamado sem vínculo nos achados (`app/ponto/services/furos_vetados.py:98`, `:100`).

**Leitor transitivo que não muda a saída por `ec=None`:** `desfazer_corroboracao_regua` chama a lista, mas filtra os achados pelo PK de um vínculo existente; os novos itens `None` não passam (`app/escala/management/commands/desfazer_corroboracao_regua.py:26`, `:38`, `:39`).

**Juiz duplicado/relógio:** não encontrei nova decisão paralela dentro do juiz: ele compõe os predicados existentes (`app/ponto/services/furos_vetados.py:25`, `:32`, `:34`); `vinculo_do_emissor` delega a `escala_vigente` (`app/ponto/services/furos_vetados.py:15`), que carrega e consulta a autoridade `vinculo_do_dia` (`app/escala/servico_jornada.py:38`, `:57`). A nova leitura acrescenta consulta de vínculo na porta (`app/chamados/models.py:896`), com uma consulta à célula e possível consulta às escalas (`app/escala/servico_jornada.py:46`, `:58`); impacto real no request: [não verificado].

## 30/10 17:25 · sessao a4cbf709629a · CALENDARIO-HOJE · verificador · origem=Code

**REPROVADO.** O último commit pode mudar o encerramento usado pelo fechamento: ele passa a encaminhar um mapa com chave `data` a um leitor que procura `(colaborador_id, data)`, fazendo o DNA congelado cair no template. O resultado controla quais furos são enviados à lavratura. (`app/ponto/services/fechamento.py:359`, `app/ponto/turnos.py:2080`, `app/escala/models.py:1095`, `app/ponto/services/fechamento.py:1194`)

1. **Chamadores com `celulas=` alimentado**

| Chamador | O mapa cobre os dias perguntados? |
|---|---|
| Calendário → `dias_encerrados` | Sim: carrega `[data_ini, data_fim]`, pergunta essa mesma janela e reutiliza os turnos dela. Chaves por tupla, adequadas à leitura do DNA. (`app/colaboradores/services/calendario.py:316`, `app/colaboradores/services/calendario.py:410`, `app/colaboradores/services/calendario.py:712`) |
| Cartório → `dias_encerrados` | Contém as células de `cels`. Cobre os dias cujas respostas o cartório lê, mas não garante todas as células existentes entre os extremos: a segunda passada recebe somente as reconciliadas, enquanto o carregador julga todo o intervalo. (`app/ponto/services/cartorio.py:745`, `app/ponto/services/cartorio.py:830`, `app/ponto/services/cartorio.py:853`, `app/ponto/turnos.py:2136`) |
| Fechamento → `dia_encerrado` → `dia_encerrado_motivo` | Recorta as células existentes no pad `d−2..d+1`, **com chave `data`**. Cobre temporalmente a pergunta, mas o formato é incompatível com o novo destino. (`app/ponto/services/fechamento.py:334`, `app/ponto/services/fechamento.py:354`, `app/ponto/turnos.py:1845`, `app/ponto/turnos.py:2058`) |
| Vigia de hora → `dia_encerrado` → motivo | Fornece a célula do próprio dia por tupla: suficiente para o DNA perguntado; não fornece as células do pad que o pareador também consulta. (`app/ponto/services/vigia_de_hora.py:57`, `app/ponto/turnos.py:1857`) |

Suprajuiz, emissor retroativo e `par_completo_da_janela` **não alimentam `celulas=` nessas chamadas**; o carregador faz sua própria carga. (`app/ponto/management/commands/supra_juiz.py:108`, `app/ponto/management/commands/emitir_furo_retroativo.py:62`, `app/ponto/turnos.py:2150`, `app/ponto/turnos.py:2129`)

**O chamador que muda com o último commit é o fechamento.** Antes, `marcos_do_dia` sem alimentação consultava a célula; agora recebe `{data: célula}`, não encontra a chave por tupla e usa o template. (`app/ponto/turnos.py:2080`, `app/escala/models.py:1094`, `app/escala/models.py:1108`)

Contraexemplo: sem turno aberto, DNA `10–14`, template `19–07`, instante 15h. Antes: encerrado; agora, pelo mapa do fechamento: `relogio`. A combinação inversa pode antecipar a afirmação do furo. A mesma divergência DNA/template está na fixture nova, porém alimentada por tupla. (`app/ponto/turnos.py:2102`, `app/ponto/services/fechamento.py:361`, `app/ponto/tests/test_c1_dias_encerrados.py:60`)

A consequência alcança `coletar_furos` e `furos_do_motor` nas versões motor e oráculo. Ocorrência concreta em produção: **[nao verificado]**. (`app/ponto/services/fechamento.py:1095`, `app/ponto/services/fechamento.py:1194`, `app/ponto/services/fechamento.py:1261`)

2. **`retido_folha` com `cobertura=True`**

**Com as cargas concluídas normalmente, não há combinação desses fatos que alcance esse ramo.** A cobertura de cobrança exclui somente `rejeitada`; a A2 também. Ambas abrangem intervalos fechados e prolongam intervalos sem fim somente para `TIPOS_EM_ABERTO`; os demais tipos sem fim cobrem apenas o dia inicial. (`app/ponto/turnos.py:2490`, `app/ponto/turnos.py:2734`, `app/ponto/turnos.py:2741`, `app/ponto/precedencia.py:211`, `app/ponto/precedencia.py:278`)

Todos os tipos recebem faixa: os não nomeados caem em `atestado`; `folga_compensatoria` e `feriado_folga` entram em folga. As faixas de ausência retornam antes da retenção; a folga humana retira o dia do ramo de trabalho; feriado sem batida retorna folga. Sobreposições podem escolher ausências diferentes — cobertura prefere a mais recente, A2 prefere a faixa —, mas preservam essa barreira. (`app/ponto/precedencia.py:52`, `app/ponto/precedencia.py:79`, `app/ponto/precedencia.py:333`, `app/ponto/turnos.py:2750`, `app/colaboradores/services/calendario.py:124`, `app/colaboradores/services/calendario.py:127`, `app/colaboradores/services/calendario.py:131`)

Existe um caminho degradado: a carga A2 pode lançar exceção, que é capturada deixando `_fatos={}`; a cobertura permanece independente. Nesse caminho o classificador pode receber cobertura verdadeira sem a faixa que impediria a retenção. Ocorrência no dado fornecido: **[nao verificado]**. (`app/colaboradores/services/calendario.py:611`, `app/colaboradores/services/calendario.py:624`, `app/colaboradores/services/calendario.py:807`, `app/colaboradores/services/calendario.py:832`)

3. **Os testes do último commit mordem parcialmente** [nao verificado]

| Teste | Mutação que pega / que deixa passar | [nao verificado]
|---|---|
| Ausência pendente | Pega A2 restringida a aprovadas. **Deixa passar cobrança restringida a aprovadas**, pois a A2 ainda retorna `ausencia` antes de consultar a cobertura. Portanto não sela a igualdade dos universos prometida no comentário. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:133`, `app/colaboradores/services/calendario.py:124`) |
| Propriedade alimentada C1 | Pega substituição do DNA pelo template no caso das 15h. **Deixa passar retirar exatamente o novo encaminhamento singular**: o DNA alimentado foi gravado no banco, e a leitura sem alimentação encontra o mesmo valor. Também não exercita o mapa por `data` do fechamento. (`app/ponto/tests/test_c1_dias_encerrados.py:60`, `app/ponto/tests/test_c1_dias_encerrados.py:68`, `app/escala/models.py:1098`) |
| Render com sentinela | Pega `title` literal com a frase da fixture: exige que o campo alterado apareça no atributo nos três layouts. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:104`, `app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:108`) |
| Custo | A igualdade entre um e oito dias pega crescimento por dia; o novo `um <= 1` também pega acréscimo fixo superior a uma consulta. (`app/colaboradores/tests/test_calendario_hoje_nao_e_falta.py:176`) |

Execução integral da suíte Django e resultados dos logs citados nos comentários: **[nao verificado]**. [nao verificado]

4. **Quem escreve e quem lê os campos do dia**

- **Escreve `status`:** `_classificar`; `contexto_calendario` recebe o resultado e o coloca no dia. **Escreve `status_rotulo`:** o ternário que escolhe motivo da folha, dia em curso ou tabela. (`app/colaboradores/services/calendario.py:108`, `app/colaboradores/services/calendario.py:814`, `app/colaboradores/services/calendario.py:898`)
- **Leem `status`:** portas `status_do_dia` e `contar_por_status`, resumo, `resolver_dia_oferecido`, cores/ícones da grade e o PDF via mapa de semanas. (`app/colaboradores/services/calendario.py:228`, `app/colaboradores/services/calendario.py:239`, `app/colaboradores/services/calendario.py:949`, `app/colaboradores/services/calendario.py:205`, `app/templates/colaboradores/partials/_calendario_grade.html:69`, `app/relatorios/views.py:1550`, `app/relatorios/furos_pdf.py:53`)
- **Lê `status_rotulo`:** a grade compartilhada por perfil, drawer e relatório HTML, inclusive agora no fallback. O PDF continua consumindo status e cores, sem imprimir esse campo. (`app/templates/colaboradores/partials/_calendario_grade.html:99`, `app/templates/colaboradores/partials/_calendario_grade.html:107`, `app/templates/colaboradores/calendario_partial.html:62`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`, `app/templates/relatorios/furos.html:42`, `app/relatorios/furos_pdf.py:56`)

Não identifiquei leitor adicional desses dois campos além dos declarados. Há, porém, uma ressalva à alegação de invariância: o botão da tela pode desaparecer para dia passado ainda em curso, da competência anterior, que passe de `falta` a `previsto`. O vigia do fio filtra células com veredito acusatório, excluindo os novos casos sem veredito dessa leitura. (`app/colaboradores/services/calendario.py:196`, `app/colaboradores/services/calendario.py:204`, `app/colaboradores/services/calendario.py:715`, `app/colaboradores/services/calendario.py:914`, `app/chamados/services/vigia_fio.py:229`)

O achado bloqueante é o fechamento: seu contrato por `data` tornou o repasse singular uma mudança de valor, e o teste novo não cobre esse contrato. (`app/ponto/services/fechamento.py:334`, `app/ponto/turnos.py:2080`, `app/ponto/tests/test_c1_dias_encerrados.py:62`)

_656 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
