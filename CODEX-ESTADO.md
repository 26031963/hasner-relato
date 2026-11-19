# CODEX-ESTADO

> Ultima resposta processada: **19/11/2026 01:26** · publicado em **19/11/2026 01:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 19/11 01:26 · sessao ea68c280ade9 · ESCALA-NAO-DESCREVE · verificador · origem=Code

1. **PROCEDE, com ressalvas.** Os dois diffs fornecidos mostram alterações apenas em valores de `LINHAS_REGISTRADAS` no arquivo em conflito; não mostram mudança de lógica nesse arquivo. O diff da ESCALA registra `entradas_fora_do_inicio` de `[521]` para `[524]` e `assinaturas_do_colab` de `[170, 177]` para `[179, 186]`. Ambos correspondem a arquivos da lista de produção fornecida. Os anexos foram fornecidos no enunciado sem numeração de linhas; sua independência e completude em relação aos commits são **[não verificado]**. [nao verificado]

   O **MEDIDOR** é `core.medida_turno_do_par.censo(raiz)`: sua entrada recebe a pasta `app/`, lê os fontes e chama `censo_fontes` ([medida_turno_do_par.py:448](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/medida_turno_do_par.py:448)); o teste usa `cls.c = m.censo(_app())` e compara as linhas por igualdade ([test_medida_turno_do_par.py:502](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:502), [test_medida_turno_do_par.py:558](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:558)). Para obter o registro medido, executar dentro do `app/` da árvore desejada:

   ```bash
   PYTHONDONTWRITEBYTECODE=1 python3 -B -c "from core import medida_turno_do_par as m; from pprint import pprint; c=m.censo('.'); pprint({k:{(i['chave'], i.get('sitio', i['criterio'])): i['linhas'] for i in c[k]['itens']} for k in ('pareadores','derivacoes_de_dia')}, sort_dicts=False)"
   ```

   Rodei o medidor **na cópia disponível**, não numa árvore mergeada. Além de `autoridade_do_periodo` `[1029] → [1045]`, ele encontrou outras três divergências preexistentes na catraca: [nao verificado]

   | Item B1 | Registrado | Medido |
   |---|---:|---:|
   | `espelho.py::montar_dias` | `[426]` | `[440]` |
   | `turnos.py::_dia_das_batidas_puro` | `[2967]` | `[2971]` |
   | `turnos.py::dia_do_turno_da_batida` | `[2945]` | `[2949]` |

   Os registros estão em [test_medida_turno_do_par.py:324](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:324) e [test_medida_turno_do_par.py:336](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:336); as ocorrências reais estão em [espelho.py:1045](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/services/espelho.py:1045), [espelho.py:440](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/services/espelho.py:440), [turnos.py:2971](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/turnos.py:2971) e [turnos.py:2949](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/ponto/turnos.py:2949). A atribuição de cada deslocamento à ESPURIA-TELA por diff de commits é **[não verificado]**.

2. **NÃO PROCEDE a afirmação de colisão demonstrada com aval ou lei.** Os cortes vigentes da ESCALA tratam de botão, folga e ordenação ([CORTES.md:551](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/CORTES.md:551)); o BACKLOG registra a obra e “pouso so depois de 19/11” ([BACKLOG.md:2341](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/BACKLOG.md:2341)). A L-126 exige, entre outras condições, opus final e preserva proibições ao construtor ([LEIS.md:147](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/LEIS.md:147)). O item 4 fornecido prevê opus final e restringe a resolução a não mudar `.py` de produção; não identifiquei colisão literal nesses pontos. Conformidade do **delta futuro** e condições efetivas de pouso: **[não verificado]**.

3. **NÃO PROCEDE que as condições sejam impossíveis no literal; PROCEDE a ressalva de escopo.** O item 4 fornecido exige “numeros da catraca saem do MEDIDOR rodado na arvore mergeada, nunca digitados” e “nenhum .py de producao muda” [anexo `aval.txt`, numeração de linha não fornecida]. Essas condições são compatíveis com atualizar o arquivo de teste a partir da medição da árvore mergeada ([test_medida_turno_do_par.py:17](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:17)).

   Contudo, **resolver só os trechos conflitantes não basta para tornar a catraca verde na cópia medida**: há quatro registros já defasados, inclusive `[1029] → [1045]`, listados acima. Se persistirem na árvore mergeada, o medidor exigirá também esses quatro ajustes ([test_medida_turno_do_par.py:558](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/core/tests/test_medida_turno_do_par.py:558)). O texto “sobre a ponta” e “numeros ... na arvore mergeada” sustenta incluí-los na **atualização mecânica da catraca do item 4**; se a intenção era reservar os registros herdados à outra obra, o aval fornecido não o esclarece **[não verificado]**. Os valores finais após o merge também são **[não verificado]**.

4. **NÃO PROCEDE que o delta de resolução declarado toque, por si, competência exportada, dinheiro, motor, folha, export, DiaPago, dados de escala, vínculo ou `!` fora do declarado.** O item 4 fornecido limita o trabalho ao conflito do teste e diz “nenhum .py de producao muda” [anexo `aval.txt`, numeração de linha não fornecida]. Isso **não certifica a raia ESCALA inteira**: a lista fornecida inclui arquivos de produção, e seu conteúdo e efeitos no delta completo entre commits não foram disponibilizados **[não verificado]**. A L-126 mantém as proibições pertinentes ao construtor ([LEIS.md:147](/home/ronald/codex_ws/477a78ea597ee461d1c94621a2c610c603e6fd34/app/docs/LEIS.md:147)).

A cópia disponível não tem metadados Git acessíveis; portanto, hash, merge-base, conflito efetivo e árvore mergeada não puderam ser verificados independentemente **[não verificado]**. Não executei a suite nem alterei arquivos. [nao verificado]

**REVISAO item 4: PROCEDE 1 / NAO PROCEDE 3.**

## 19/11 00:56 · sessao 8e6cc7d6cd1a · QUEIMA-B3 · censo · origem=Code

**O censo avançou, mas ainda não está pronto para abrir todas as fatias.** A cópia permite localizar os sítios listados e identificar mudanças relevantes, mas não permite verificar o hash informado por Git, os pousos em produção, os casos literais da tabela única nem o efeito concreto da futura F5 da espinha em cada leitor. O levantamento de leitores e escritores abaixo é **parcial**, não um censo exaustivo. [não verificado]

A correção mais relevante em relação à volta 1 é que **A5 mudou de premissa**, não apenas de linha: o laço agora usa `q_nao_validada_pelo_ato()`, enquanto o anexo antigo descrevia `validada_em__isnull=True`. E a F3 da espinha, conforme o corte registrado, **não altera `marcos_do_lote` nem passa a ler a trilha**: constrói um selo bitemporal com mutante. [models.py:315](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:315), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

**Critério dos sítios:** `CONFERE` significa que a operação indicada no anexo foi encontrada na linha indicada; `MUDOU` significa linha ou trecho diferente. A classificação não afirma que todos os efeitos e condições da tabela única foram reconferidos. Os anexos fornecem a atribuição dos códigos; onde não fornecem a tradução, ela fica [não verificado].

**Condições comuns.** A ordem fornecida é F0 → F1 → F2 → F3 → F4 → F5 → F6 → F7; a B2b espera F6F7, F9, F5 e F3 da espinha. O backlog também registra a espera, mas o estado real dos pousos está [não verificado]. Antes de cada abertura, é necessário reconferir as linhas no HEAD do dia. Cada fatia tem pouso e suíte próprios, teto de dez arquivos; F4 vai sozinha e F5 admite até dois pousos. [BACKLOG.md:826](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:826); demais condições: anexo `BRIEF-FATIAS.md` fornecido.

A matriz fornecida deriva **39 de 45** retirando P21, X1 duplicado de A9c, P14 duplicado de P13, B9a/B9b e B8; ela própria qualifica a derivação como contestável. B8 continua explicitamente no brief de F5: a dedução para a contagem **não o retira por si só da fatia**. A abrangência efetiva exigida para B8 deve ser reconferida na tabela única [não verificado].

### F0 — instrumentos desligados

- **Sítios:** o brief nomeia instrumentos F6, F8, F9/F9b e F4, sem mapeá-los a sítios concretos existentes; identidade e contratos futuros [não verificado]. A cerca atual tem `ContextVar` com `default=None` e a guarda retorna sem consultar fora de contexto; seu estado contém `vetadas`, `memo` e `empresa`, sem identificador de sítio. [fio.py:1381](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1381), [fio.py:1402](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1402), [fio.py:1445](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1445).
- **Campos e dependências:** a guarda lê `conversa`, `data_evento`, colaborador e empresa, inclusive dia gravado e proposto; leitores e escritores completos dos futuros instrumentos [não verificado]. [fio.py:1408](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1408).
- **RED candidato:** coletor desligado preserva retorno, escrita e exceção observáveis, inclusive fora da cerca. Casos literais e resultado [não verificado].
- **Após F5/F3 da espinha:** a F3 registrada mantém `marcos_do_lote`; o que a F5 mudará nos leitores concretos destes instrumentos futuros [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F1 — carimbo no fecho

| Sítio | Conferência nesta cópia |
|---|---|
| X1 / A9c | **MUDOU:** mesmo laço S144, `p.carimbar` agora em [materializacao.py:2180](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2180). X1=A9c conforme matriz fornecida. |
| A9d | **CONFERE:** fecho por competência trancada em [competencia_trancada.py:229](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/competencia_trancada.py:229). |
| B1a / B1b | **CONFERE:** carimbo e fecho em [reconciliador.py:639](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:639) e [reconciliador.py:656](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:656). |
| B2a | **CONFERE:** carimbo e fecho em [reconciliador.py:407](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:407) e [reconciliador.py:414](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:414). |
| B3a / B3b | **CONFERE:** encerramento e fecho em [reconciliador.py:1192](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1192) e [reconciliador.py:1216](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1216). |
| X13 | **MUDOU:** fecho em [materializacao.py:2043](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2043). |
| X14 | **CONFERE:** fecho em [disputa_emissao.py:1298](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1298). |

- **Campos:** `carimbar` escreve `materializada_em`, `materializada_por`, `via_resolucao` e, conforme o argumento e texto, resposta e validação; pode relavrar. Um leitor de produção desses marcos é `marcos_do_lote`. O fecho escreve também os campos da disputa e, em certos motivos, registra SILÊNCIO. Censo completo dos demais escritores e leitores [não verificado]. [models.py:3299](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3299), [models.py:2080](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2080), [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201).
- **Dependência e RED:** depois de F0. Para A9d, candidato: disputa fecha, pergunta entregue que S144 tentaria carimbar permanece preservada e é contada. `RED-NOVO-2/3` são exigidos pelo brief, mas seus casos literais estão [não verificado]. [CORTES.md:556](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:556).
- **Após F5/F3 da espinha:** os marcos atuais seguem lendo os timestamps da projeção; a F3 prevista sela o comportamento bitemporal, sem adicionar leitura de `DecisaoPergunta`. Mudanças concretas nos leitores da F5 [não verificado]. [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F2 — relavra

| Sítio | Conferência nesta cópia |
|---|---|
| A4 | **MUDOU:** `reconciliar` em [models.py:654](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:654), decisão e gate em [models.py:676](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:676). |
| A5 | **MUDOU:** laço usa face SQL da validação pelo ato, em [models.py:315](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:315). |
| A6 / A7 | **MUDOU:** entradas em [models.py:1742](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1742) e [models.py:1759](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1759). |
| B5 / B6 | **CONFERE:** `lavrar` salva em [veredito_lavrado.py:275](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:275); lote usa `bulk_update` em [veredito_lavrado.py:250](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:250). |
| B7 | **CONFERE:** save de relavra em [relavra_via.py:127](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:127). |

- **Campos:** `lavrar` lê o julgamento e `data_evento`, escreve `veredito_lavrado`, `julgado_em` e `janela_expira_em`; B7 também escreve `via_resolucao`. `esta_stale` e `marcos_do_lote` são leitores relevantes. B6 não passa pela guarda de `save`; inserção e migration histórica também são exceções apontadas pela matriz, sem pretensão de exaustividade. [veredito_lavrado.py:213](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:213), [veredito_lavrado.py:250](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:250), [veredito_lavrado.py:270](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:270), [relavra_via.py:127](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:127), [models.py:3209](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3209), [models.py:3674](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3674).
- **Dependência e RED:** depois de F1. Candidato: lote com entregue e aberta, preservando a primeira e relavrando a segunda; incluir B6 e a seleção pelo ato de A5. A equivalência ao `RED-NOVO-1` literal [não verificado].
- **Após F5/F3 da espinha:** a F3 mantém os marcos atuais e sela a foto bitemporal; a F5 pode alterar leitores de resposta, mas o diff e seus pontos exatos [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F3 — reaponte da B2b

| Sítio | Conferência nesta cópia |
|---|---|
| B4 | **CONFERE:** atribuição e save em [fio.py:974](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:974). |
| B2b / B2d | **CONFERE:** chamadas em [reconciliador.py:998](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:998) e [reconciliador.py:1712](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:1712). |
| X6 / X7 | **MUDOU:** carimbo de duplicata em [fio.py:1502](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1502); reaponte da migração em [fio.py:2338](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:2338). |
| X8 | **MUDOU:** gate reaponta em [models.py:449](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:449). |
| X9 / X10 | **CONFERE:** chamadas em [limbo_folgas.py:245](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/limbo_folgas.py:245) e [limbo_folgas.py:283](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/limbo_folgas.py:283). |
| X12 | **CONFERE:** chamada do comando em [reatar_beco_folga_ao_limbo.py:135](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:135). |

- **Campos:** B4 escreve `chamado_destino` e a lavra, podendo datar a pergunta; X6 carimba identidade de duplicata e motivo. A guarda distingue conversa e lê dia anterior e proposto. Chamadores adicionais de produção incluem declaração humana: portanto, identificar o ator apenas pela porta B4 seria insuficiente. Censo completo dos leitores [não verificado]. [fio.py:955](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:955), [fio.py:974](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:974), [declaracao_texto.py:371](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/declaracao_texto.py:371), [fio.py:1408](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1408).
- **Dependência e RED:** depois de F2. Candidatos: dia anterior entregue versus dia novo aberto, e vice-versa; conversa; chamada humana e de sistema; duplicata preservada quando reaponte é recusado. Casos literais da tabela [não verificado].
- **Após F5/F3 da espinha:** o selo F3 protege a leitura bitemporal existente da lavra; o conjunto de leitores alterados pela F5 para cada chamador [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F4 — laço de materialização

| Sítio | Conferência nesta cópia |
|---|---|
| P11 / P12 | **MUDOU:** save de pendência em [materializacao.py:1366](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1366); carimbo de paridade em [materializacao.py:1474](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1474). |
| P13 / P14 | **MUDOU:** ramos da mesma função em [materializacao.py:1699](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1699) e [materializacao.py:1703](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1703); P14=P13 na dedução da matriz fornecida. |
| P15 | **MUDOU:** carimbo em [materializacao.py:1981](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1981). |

- **Campos:** P11 escreve `pendencia_admin` e `recusa_motivo`; P13/P14 escrevem identidade ou motivo da recusa; P15 carimba materialização e pode escrever resposta/validação pela porta comum. O laço lê resposta, validação e materialização e relê sob trava. Outros escritores e leitores por campo [não verificado]. [materializacao.py:1366](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1366), [materializacao.py:1683](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1683), [materializacao.py:2100](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2100).
- **Dependência e RED:** depois de F3 da B2b e em pouso isolado. Candidato: uma pergunta retida e outra aplicável no mesmo laço; verificar resultado individual, fecho e o `finally` que tenta registrar recusa. Efeito exato da segunda exceção [não verificado]. [materializacao.py:1993](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1993).
- **Após F5/F3 da espinha:** a lei de resposta exige instante **e** texto também para materialização; a migração concreta dos leitores pela F5 e seu diff para este laço [não verificado]. A F3 mantém os marcos existentes. [LEIS.md:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:151), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F5 — comandos e avulsos da B2b

| Sítio | Conferência nesta cópia |
|---|---|
| A8 | **MUDOU:** ressuscitação relê sob trava e grava compensação; o lote guarda `alvo_lido`, mas escreve só os IDs relidos. [models.py:1627](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1627), [models.py:1700](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1700). |
| P1 | **MUDOU:** save de reuso em [models.py:3198](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3198). |
| B8 | **CONFERE:** save em [fio.py:1092](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1092). |
| P3 | **MUDOU:** carimbo e fecho em [models.py:2224](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2224) e [models.py:2235](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2235). |
| P4 / P5 | **MUDOU:** portas em [models.py:3489](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3489) e [models.py:3530](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3530); correspondência completa dos chamadores e tratamento de exceção [não verificado]. |
| P17 | **MUDOU:** lote e células em [disputa_emissao.py:1656](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1656). |
| P18 / P19 | **MUDOU:** saves em [disputa_emissao.py:1685](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1685) e [disputa_emissao.py:1707](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1707). |
| P20 | **CONFERE:** descarimbo em [relavra_via.py:246](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:246). |

- **Campos:** A8 e P17 limpam carimbo e estados de resposta/validação e registram decisões; P18 escreve `resposta_colab` e `respondida_em`; P19 escreve o instante apoiado no texto e registra ato; B8 escreve dia, célula e lavra. Os leitores e escritores completos de cada campo [não verificado]. [models.py:1700](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1700), [disputa_emissao.py:1640](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1640), [disputa_emissao.py:1680](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:1680), [fio.py:1092](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1092).
- **Dependência e RED:** depois de F4; divisão em até dois pousos ainda [não verificado]. Candidatos: A8 com conjunto alterado entre leitura e trava, inclusive relido vazio; P17 com entregue e aberta sem trilha mentirosa; P18/P19 preservando o par legal; P4 com falha por item. Casos literais da tabela [não verificado].
- **Após F5/F3 da espinha:** A8 **já** lê `DecisaoPergunta.leitura` para a compensação nesta cópia; não atribuir essa leitura ao futuro pouso F5. O que cada outro sítio passará a ler pela F5 [não verificado]; F3 mantém os marcos. [models.py:1688](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:1688), [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).

### F6 — callback depois da cerca

- **X15 — MUDOU:** a evidência disponível identifica o problema como reset da cerca e callback, mas o anexo bruto não traduz X15 em função e linha. O reset atual ocorre no `finally` **depois** da saída do `atomic` interno; há callbacks `on_commit` em signals. A identidade do callback específico X15 permanece [não verificado]. [fio.py:1447](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1447), [fio.py:1459](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1459), [signals.py:240](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/signals.py:240), [RELATO.md:326](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/RELATO.md:326).
- **Campos:** estado transitório `vetadas`, `memo`, `empresa`; campos da pergunta efetivamente tocados pelo callback X15 e respectivos leitores/escritores [não verificado]. [fio.py:1445](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1445).
- **Dependência e RED:** depois de F5 da B2b. Candidato: callback executado no commit observa cerca já encerrada, sem herdar veto/memo do ato anterior; cobrir também transação externa e aninhamento. Caso literal [não verificado]. [fio.py:1431

[truncado em 20000 de 24084 caracteres]

## 19/11 00:54 · sessao 87eee57fdaea · QUEIMA-B2-chamado · censo · origem=Code

O script abaixo produz um inventário **sintático e conservador**; sua execução e seus totais estão [não verificado]. Os quatro campos selecionados correspondem às declarações em [app/chamados/models.py:58](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:58). O formato segue o molde fornecido no enunciado [não verificado].

```python
#!/usr/bin/env python3
# Uso: python3 -B script.py RAIZ
# Somente leitura; RAIZ deve conter app/.
import ast
import collections
import os
import re
import sys
import tokenize

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B script.py RAIZ\n")
    sys.exit(2)

RAIZ = os.path.abspath(sys.argv[1])
APP = os.path.join(RAIZ, "app")
if not os.path.isdir(APP):
    sys.stderr.write("ERRO: RAIZ deve conter app/\n")
    sys.exit(2)

CAMPOS = (
    "status_local",
    "via_encerramento",
    "respondido_por",
    "resolvido_em",
)
ALIAS = {campo: campo for campo in CAMPOS}
ALIAS["respondido_por_id"] = "respondido_por"

PADRAO = re.compile(
    r"(?<![A-Za-z0-9_])(?:"
    + "|".join(re.escape(c) for c in sorted(ALIAS, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)
SQL = re.compile(
    r"\b(select|insert|update|delete|with|returning|where|from|join)\b",
    re.I,
)
TAGS = re.compile(r"{{.*?}}|{%.*?%}", re.S)
ATRIBUTO_TEMPLATE = re.compile(
    r"\.\s*("
    + "|".join(re.escape(c) for c in sorted(ALIAS, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)

LEITURAS = {
    "filter", "exclude", "get", "values", "values_list", "order_by",
    "annotate", "alias", "only", "defer", "distinct", "earliest", "latest",
}
CADEIA = LEITURAS | {
    "all", "none", "select_for_update", "select_related", "prefetch_related",
    "using", "reverse", "iterator", "first", "last", "create",
    "get_or_create", "update_or_create", "update", "bulk_create", "bulk_update",
}
ESCOPOS = (ast.Module, ast.ClassDef, ast.FunctionDef, ast.AsyncFunctionDef,
           ast.Lambda)
FUNCOES = (ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda)
REGISTROS = set()
FALHAS = []
ARVORES = []
MODELOS = {"ChamadoColaborador"}


def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")


def categoria(relativo):
    partes = relativo.lower().replace("\\", "/").split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if ("tests" in partes or "test" in partes or nome.startswith("test_")
            or nome.endswith("_test.py")):
        return "teste" [nao verificado]
    return "prod"


def chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""


def ocorrencias(texto):
    for achado in PADRAO.finditer(texto):
        yield ALIAS[achado.group()], achado.start()


def campo_caminho(texto):
    """Retorna os campos presentes em segmentos Django, inclusive lookups."""
    if not isinstance(texto, str):
        return []
    partes = texto.lstrip("-").split("__")
    return [(ALIAS[p], i) for i, p in enumerate(partes) if p in ALIAS]


def nome_base(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""


def emitir(arquivo, linha, funcao, campo, papel, escopo, receptor, motivo, forma):
    REGISTROS.add((
        arquivo, int(linha), funcao or "<modulo>", campo, papel, escopo,
        receptor, motivo, forma,
    ))


def filhos_sem_escopos(no):
    pilha = list(ast.iter_child_nodes(no))
    while pilha:
        atual = pilha.pop()
        yield atual
        if not isinstance(atual, ESCOPOS):
            pilha.extend(ast.iter_child_nodes(atual))


# Examina a cópia inteira, incluindo testes e templates fora de app/. [nao verificado]
# Diretórios de metadados e caches são ignorados.
IGNORAR_DIRS = {".git", "__pycache__", ".venv", "venv", "node_modules"}
TEMPLATES = []
for diretorio, dirs, arquivos in os.walk(RAIZ, followlinks=False):
    dirs[:] = sorted(d for d in dirs if d not in IGNORAR_DIRS)
    for nome in sorted(arquivos):
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        if nome.endswith((".html", ".htm", ".django", ".jinja", ".jinja2")):
            TEMPLATES.append((relativo, caminho))
        if not nome.endswith(".py"):
            continue
        try:
            with tokenize.open(caminho) as arquivo:
                fonte = arquivo.read()
            arvore = ast.parse(fonte, filename=relativo)
        except (OSError, SyntaxError, UnicodeError, LookupError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        ARVORES.append((relativo, arvore))
        for no in ast.walk(arvore):
            if (isinstance(no, ast.ClassDef)
                    and any(nome_base(base) == "Model" for base in no.bases)):
                MODELOS.add(no.name)


for relativo, arvore in ARVORES:
    escopo_arquivo = categoria(relativo)
    pais = {}
    ambientes = {}
    nomes_escopo = {}
    classes = {}

    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    def indexar(no, pilha, classe, ambiente):
        if isinstance(no, ast.ClassDef):
            pilha = pilha + [no.name]
            classe = no.name if no.name in MODELOS else None
            ambiente = no
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            pilha = pilha + [no.name]
            ambiente = no
        elif isinstance(no, ast.Lambda):
            pilha = pilha + ["<lambda>"]
            ambiente = no
        ambientes[no] = ambiente
        nomes_escopo[no] = ".".join(pilha) or "<modulo>"
        classes[no] = classe
        for filho in ast.iter_child_nodes(no):
            indexar(filho, pilha, classe, ambiente)

    indexar(arvore, [], None, arvore)

    atribuicoes = collections.defaultdict(lambda: collections.defaultdict(list))
    anotacoes = collections.defaultdict(lambda: collections.defaultdict(list))
    acrescentados = collections.defaultdict(lambda: collections.defaultdict(list))

    for ambiente in [n for n in ast.walk(arvore) if isinstance(n, ESCOPOS)]:
        if isinstance(ambiente, FUNCOES):
            args = ambiente.args
            todos = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            if args.vararg:
                todos.append(args.vararg)
            if args.kwarg:
                todos.append(args.kwarg)
            for arg in todos:
                if arg.annotation is not None:
                    anotacoes[ambiente][arg.arg].append(arg.annotation)

        for no in filhos_sem_escopos(ambiente):
            if ambientes.get(no) is not ambiente:
                continue
            if isinstance(no, ast.Assign):
                for alvo in no.targets:
                    if isinstance(alvo, ast.Name):
                        atribuicoes[ambiente][alvo.id].append(no.value)
            elif isinstance(no, ast.AnnAssign) and isinstance(no.target, ast.Name):
                anotacoes[ambiente][no.target.id].append(no.annotation)
                if no.value is not None:
                    atribuicoes[ambiente][no.target.id].append(no.value)
            elif isinstance(no, (ast.For, ast.AsyncFor)) and isinstance(no.target, ast.Name):
                atribuicoes[ambiente][no.target.id].append(no.iter)
            elif (isinstance(no, ast.Call) and isinstance(no.func, ast.Attribute)
                  and isinstance(no.func.value, ast.Name)
                  and no.func.attr in ("append", "extend") and no.args):
                acrescentados[ambiente][no.func.value.id].append(no.args[0])

    def modelo_anotacao(no):
        # Somente uma anotação direta de modelo é tratada como prova.
        if isinstance(no, ast.Name) and no.id in MODELOS:
            return no.id
        if isinstance(no, ast.Attribute) and no.attr in MODELOS:
            return no.attr
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            partes = no.value.split(".")
            if partes[-1] in MODELOS and all(p.isidentifier() for p in partes):
                return partes[-1]
        return None

    def inferir(no, contexto, visitados=None):
        if no is None:
            return None
        if visitados is None:
            visitados = set()
        ambiente = ambientes.get(contexto, arvore)
        if isinstance(no, ast.Name):
            if no.id == "self" and classes.get(contexto):
                return classes[contexto]
            if no.id in MODELOS:
                if no.id not in atribuicoes[ambiente]:
                    return no.id
                return None
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return None
            visitados = visitados | {chave}
            anns = anotacoes[ambiente].get(no.id, [])
            vals = atribuicoes[ambiente].get(no.id, [])
            resultados = [modelo_anotacao(x) for x in anns]
            resultados += [inferir(x, x, visitados) for x in vals]
            if resultados and None not in resultados and len(set(resultados)) == 1:
                return resultados[0]
            return None
        if isinstance(no, ast.Attribute):
            if no.attr in MODELOS and isinstance(no.value, ast.Name):
                return no.attr
            if no.attr in ("objects", "_default_manager", "_base_manager"):
                return inferir(no.value, contexto, visitados)
            return None
        if isinstance(no, ast.Subscript):
            return inferir(no.value, contexto, visitados)
        if isinstance(no, ast.Call):
            if chamada(no) == "type" and len(no.args) == 1:
                return inferir(no.args[0], contexto, visitados)
            if (chamada(no) == "get_model" and no.args
                    and isinstance(no.args[-1], ast.Constant)
                    and isinstance(no.args[-1].value, str)):
                nome = no.args[-1].value.split(".")[-1]
                return nome if nome in MODELOS else None
            if isinstance(no.func, ast.Name) and no.func.id in MODELOS:
                return inferir(no.func, contexto, visitados)
            if isinstance(no.func, ast.Attribute):
                if no.func.attr in MODELOS:
                    return inferir(no.func, contexto, visitados)
                if no.func.attr in CADEIA:
                    return inferir(no.func.value, contexto, visitados)
        return None

    def receptor(no, contexto, caminho=None):
        if caminho and any(pos > 0 for _, pos in campo_caminho(caminho)):
            return "AMBIGUO", "caminho de relacao nao resolvido"
        modelo = inferir(no, contexto)
        if modelo:
            return "RECEPTOR=" + modelo, "inferencia sintatica"
        return "AMBIGUO", "receptor nao resolvido"

    def receptor_chamada(call, contexto, caminho=None):
        if isinstance(call, ast.Call) and isinstance(call.func, ast.Attribute):
            return receptor(call.func.value, contexto, caminho)
        return "AMBIGUO", "chamada sem receptor de modelo"

    def registrar(no, campo, papeis, forma, rec=None, motivo=None, linha=None):
        if rec is None:
            rec, motivo = "AMBIGUO", "receptor nao resolvido"
        for papel in papeis:
            emitir(
                relativo, linha or getattr(no, "lineno", 1),
                nomes_escopo.get(no, "<modulo>"), campo, papel,
                escopo_arquivo, rec, motivo or "", forma,
            )

    def chamada_externa(no):
        atual = no
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.Call) and chamada(atual) in CADEIA:
                return atual
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        return None

    def literais_resolvidos(no, contexto, visitados=None):
        """Reúne literais visíveis; não afirma que o fluxo os executa."""
        if visitados is None:
            visitados = set()
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            yield no
        elif isinstance(no, (ast.List, ast.Tuple, ast.Set)):
            for item in no.elts:
                yield from literais_resolvidos(item, contexto, visitados)
        elif isinstance(no, ast.Dict):
            for chave in no.keys:
                if chave is not None:
                    yield from literais_resolvidos(chave, contexto, visitados)
        elif isinstance(no, ast.Name):
            ambiente = ambientes.get(contexto, arvore)
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return
            visitados = visitados | {chave}
            for valor in (atribuicoes[ambiente].get(no.id, [])
                          + acrescentados[ambiente].get(no.id, [])):
                yield from literais_resolvidos(valor, valor, visitados)
        elif isinstance(no, ast.BinOp) and isinstance(no.op, ast.Add):
            yield from literais_resolvidos(no.left, contexto, visitados)
            yield from literais_resolvidos(no.right, contexto, visitados)

    def registrar_lista(call, valor, forma):
        rec, motivo = receptor_chamada(call, call)
        for literal in literais_resolvidos(valor, call):
            for campo, _ in campo_caminho(literal.value):
                registrar(call, campo, ("ESCRITA",), forma, rec, motivo)

    def classificar_string(no):
        valor = no.value
        atual = no
        palavra = None
        chave_dicionario = False
        while atual in pais:
            anterior = atual
            atual = pais[atual]
            if isinstance(atual, ast.Dict) and anterior in atual.keys:
                chave_dicionario = True
            if isinstance(atual, ast.keyword) and palavra is None:
                palavra = atual.arg
            if isinstance(atual, ast.Call):
                nome = chamada(atual)
                if nome in ("getattr", "setattr") and len(atual.args) >= 2:
                    if atual.args[1] is no:
                        rec, motivo = receptor(atual.args[0], no)
                        papel = "LEITURA" if nome == "getattr" else "ESCRITA"
                        return (papel,), nome, rec, motivo
                if nome in ("F", "OuterRef"):
                    externo = chamada_externa(atual)
                    rec, motivo = receptor_chamada(externo, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome in ("raw", "execute", "executemany") and SQL.search(valor):
                    return (("LEITURA", "ESCRITA"), "SQL cru",
                            "AMBIGUO", "SQL nao interpretado")
                if nome == "save" and palavra == "update_fields":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "save(update_fields)", rec, motivo
                if nome == "bulk_update":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "bulk_update(fields)", rec, motivo
                if nome == "bulk_create" and palavra in ("update_fields", "unique_fields"):
                    rec, motivo = receptor_chamada(atual, no)
                    papeis = ("ESCRITA",) if palavra == "update_fields" else ("LEITURA",)
                    return papeis, "bulk_create(" + palavra + ")", rec, motivo
                if chave_dicionario and nome in ("create", "update"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("ESCRITA",), nome + " dicionario", rec, motivo
                if chave_dicionario and nome in ("get_or_create", "update_or_create"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    if palavra in ("defaults", "create_defaults"):
                        return ("ESCRITA",), nome + " defaults", rec, motivo
                    return ("LEITURA", "ESCRITA"), nome + " dicionario", rec, motivo
                if nome in LEITURAS and campo_caminho(valor):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome == "Q" and campo_caminho(valor):
                    externo = chamada_externa(atual)
                    rec, motivo = receptor_chamada(externo, no, valor)
                    return ("LEITURA",), "Q", rec, motivo
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        if SQL.search(valor):
            return (("LEITURA", "ESCRITA"), "possivel SQL",
                    "AMBIGUO", "string SQL nao interpretada")
        return (("LEITURA", "ESCRITA"), "string fora de contexto reconhecido",
                "AMBIGUO", "uso da string nao resolvido")

    for no in ast.walk(arvore):
        if isinstance(no, ast.Call):
            nome = chamada(no)
            if nome == "save":
                for kw in no.keywords:
                    if kw.arg == "update_fields":
                        registrar_lista(no, kw.value, "save(update_fields) resolvido")
            elif nome == "bulk_update":
                if len(no.args) >= 2:
                    registrar_lista(no, no.args[1], "bulk_update(fields) resolvido")
                for kw in no.keywords:
                    if kw.arg == "fields":
                        registrar_lista(no, kw.value, "bulk_update(fields) resolvido")
            elif nome == "bulk_create":
                for kw in no.keywords:
                    if kw.arg == "update_fields":
                        registrar_lista(no, kw.value, "bulk_create(update_fields) resolvido")
            elif nome in ("create", "update", "get_or_create", "update_or_create"):
                for kw in no.keywords:
                    if kw.arg is None or kw.arg in ("defaults", "create_defaults"):
                        rec, motivo = receptor_chamada(no, no)
                        for literal in literais_resolvidos(kw.value, no):
                            for campo, pos in campo_caminho(literal.value):
                                r, m = (rec, motivo) if pos == 0 else (
                                    "AMBIGUO", "caminho de relacao nao resolvido"
                                )
                                papeis = ("ESCRITA",)
                                if kw.arg is None and nome in (
                                        "get_or_create", "update_or_create"):
                                    papeis = ("LEITURA", "ESCRITA")
                                registrar(no, campo, papeis,
                                          nome + " argumentos resolvidos", r, m)

        if isinstance(no, ast.Attribute) and no.attr in ALIAS:
            campo = ALIAS[no.attr]
            rec, motivo = receptor(no.value, no)
            if isinstance(no.ctx, ast.Store):
                papeis = ("ESCRITA",)
                if isinstance(pais.get(no), ast.AugAssign):
                    papeis = ("LEITURA", "ESCRITA")
            elif isinstance(no.ctx, ast.Del):
                papeis = ("ESCRITA",)
            else:
                papeis = ("LEITURA",)
            registrar(no, campo, papeis, "atributo", rec, motivo)

        elif isinstance(no, ast.keyword) and no.arg:
            campos = campo_caminho(no.arg)
            if not campos:
                continue
            call = pais.get(no)
            nome = chamada(call)
            for campo, _ in campos:
                if nome in ("Q", "When"):
                    externo = chamada_externa(call)
                    rec, motivo = receptor_chamada(externo, no, no.arg)
                    papeis = ("LEITURA",)
                elif nome in ("create", "update"):
                    rec, motivo = receptor_chamada(call, no, no.arg)
                    papeis = (

[truncado em 20000 de 23752 caracteres]

## 19/11 00:54 · sessao 13f739a36bbe · QUEIMA-B2-pergunta · censo · origem=Code

```python
#!/usr/bin/env python3
# Uso: python3 -B script.py RAIZ
# RAIZ deve conter app/. O programa apenas le arquivos.
import ast
import collections
import os
import re
import sys
import tokenize

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B script.py RAIZ\n")
    sys.exit(2)

RAIZ = os.path.abspath(sys.argv[1])
APP = os.path.join(RAIZ, "app")
if not os.path.isdir(APP):
    sys.stderr.write("ERRO: RAIZ deve conter app/\n")
    sys.exit(2)

CAMPOS = (
    "resposta_colab", "respondida_em", "validada_em", "validada_por",
    "data_conferida_em", "data_conferida_por", "pendencia_admin",
    "recusa_motivo", "via_resolucao", "veredito_lavrado", "julgado_em",
    "janela_expira_em", "materializada_em", "materializada_por",
)
ALIASES = {campo: campo for campo in CAMPOS}
for campo in ("validada_por", "data_conferida_por", "materializada_por"):
    ALIASES[campo + "_id"] = campo

PADRAO = re.compile(
    r"(?<![A-Za-z0-9_])(?:"
    + "|".join(re.escape(c) for c in sorted(ALIASES, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)
SQL = re.compile(
    r"\b(select|insert|update|delete|with|returning|where|from|join)\b",
    re.I,
)
TAGS = re.compile(r"{{.*?}}|{%.*?%}", re.S)
LEITURAS = {
    "filter", "exclude", "get", "values", "values_list", "order_by",
    "annotate", "alias", "only", "defer", "distinct", "earliest", "latest",
}
QUERYSET = LEITURAS | {
    "all", "none", "select_for_update", "select_related",
    "prefetch_related", "using", "reverse", "iterator", "first", "last",
    "create", "update", "get_or_create", "update_or_create",
    "bulk_create", "bulk_update",
}
ESCOPOS = (
    ast.Module, ast.ClassDef, ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda
)
REGISTROS = set()
FALHAS = []
ARVORES = []
MODELOS = {"PerguntaDisputa", "RegularizacaoExterna"}


def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")


def categoria(relativo):
    partes = relativo.lower().replace("\\", "/").split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if ("tests" in partes or "test" in partes
            or nome.startswith("test_") or nome.endswith("_test.py")):
        return "teste" [nao verificado]
    return "prod"


def nome_chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""


def nome_base(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""


def campo_caminho(texto):
    """Aceita um campo ou caminho ORM literal, com lookups e ordenacao."""
    if not isinstance(texto, str):
        return []
    partes = texto.lstrip("-").split("__")
    return [(ALIASES[p], i) for i, p in enumerate(partes) if p in ALIASES]


def achados(texto):
    if isinstance(texto, str):
        for m in PADRAO.finditer(texto):
            yield ALIASES[m.group()], m.start()


def emitir(arquivo, linha, funcao, campo, papel, escopo, rec, motivo, forma):
    REGISTROS.add((
        arquivo, int(linha), funcao or "<modulo>", campo, papel, escopo,
        rec, motivo, forma,
    ))


# Primeiro passe: fontes Python e nomes de modelos declarados.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith(".py"):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with tokenize.open(caminho) as arquivo:
                fonte = arquivo.read()
            arvore = ast.parse(fonte, filename=relativo)
        except (OSError, SyntaxError, UnicodeError, LookupError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        ARVORES.append((relativo, arvore))
        for no in ast.walk(arvore):
            if isinstance(no, ast.ClassDef):
                if any(nome_base(base) == "Model" for base in no.bases):
                    MODELOS.add(no.name)


for relativo, arvore in ARVORES:
    escopo_arquivo = categoria(relativo)
    pais = {}
    ambientes = {}
    funcoes = {}
    classes = {}

    def indexar(no, ambiente, pilha, classe):
        if isinstance(no, ast.ClassDef):
            ambiente = no
            pilha = pilha + [no.name]
            classe = no.name if no.name in MODELOS else None
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            ambiente = no
            pilha = pilha + [no.name]
        elif isinstance(no, ast.Lambda):
            ambiente = no
            pilha = pilha + ["<lambda>"]
        ambientes[no] = ambiente
        funcoes[no] = ".".join(pilha) or "<modulo>"
        classes[no] = classe
        for filho in ast.iter_child_nodes(no):
            pais[filho] = no
            indexar(filho, ambiente, pilha, classe)

    indexar(arvore, arvore, [], None)

    atribuicoes = collections.defaultdict(lambda: collections.defaultdict(list))
    anotacoes = collections.defaultdict(lambda: collections.defaultdict(list))

    for no in ast.walk(arvore):
        ambiente = ambientes[no]
        if isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda)):
            args = no.args
            todos = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            if args.vararg:
                todos.append(args.vararg)
            if args.kwarg:
                todos.append(args.kwarg)
            for arg in todos:
                if arg.annotation is not None:
                    anotacoes[no][arg.arg].append(arg.annotation)
        if isinstance(no, ast.Assign):
            for alvo in no.targets:
                if isinstance(alvo, ast.Name):
                    atribuicoes[ambiente][alvo.id].append(no.value)
        elif isinstance(no, ast.AnnAssign) and isinstance(no.target, ast.Name):
            anotacoes[ambiente][no.target.id].append(no.annotation)
            if no.value is not None:
                atribuicoes[ambiente][no.target.id].append(no.value)
        elif isinstance(no, (ast.For, ast.AsyncFor)):
            if isinstance(no.target, ast.Name):
                atribuicoes[ambiente][no.target.id].append(no.iter)
        elif isinstance(no, ast.comprehension) and isinstance(no.target, ast.Name):
            atribuicoes[ambiente][no.target.id].append(no.iter)

    def modelo_anotacao(no):
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            nomes = set(re.findall(r"[A-Za-z_]\w*", no.value))
        else:
            nomes = {
                x.id if isinstance(x, ast.Name) else x.attr
                for x in ast.walk(no)
                if isinstance(x, (ast.Name, ast.Attribute))
            }
        encontrados = nomes & MODELOS
        return next(iter(encontrados)) if len(encontrados) == 1 else None

    def inferir(no, contexto, visitados=None):
        """Inferencia local; nao executa codigo nem resolve relacoes Django."""
        if no is None:
            return None
        visitados = set() if visitados is None else visitados
        ambiente = ambientes.get(contexto, arvore)
        if isinstance(no, ast.Name):
            if no.id == "self" and classes.get(contexto):
                return classes[contexto]
            if no.id == "cls" and classes.get(contexto):
                if isinstance(ambiente, (ast.FunctionDef, ast.AsyncFunctionDef)):
                    if any(nome_base(d) == "classmethod" for d in ambiente.decorator_list):
                        return classes[contexto]
            if no.id in MODELOS and no.id not in atribuicoes[ambiente]:
                return no.id
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return None
            visitados = visitados | {chave}
            resultados = [
                modelo_anotacao(x) for x in anotacoes[ambiente].get(no.id, [])
            ]
            resultados += [
                inferir(x, x, visitados)
                for x in atribuicoes[ambiente].get(no.id, [])
            ]
            if resultados and None not in resultados and len(set(resultados)) == 1:
                return resultados[0]
            return None
        if isinstance(no, ast.Attribute):
            if no.attr in MODELOS and isinstance(no.value, ast.Name):
                return no.attr
            if no.attr in ("objects", "_default_manager", "_base_manager"):
                return inferir(no.value, contexto, visitados)
            return None
        if isinstance(no, ast.Subscript):
            return inferir(no.value, contexto, visitados)
        if isinstance(no, ast.Call):
            if isinstance(no.func, ast.Name) and no.func.id in MODELOS:
                return inferir(no.func, contexto, visitados)
            if isinstance(no.func, ast.Attribute):
                if no.func.attr in MODELOS:
                    return inferir(no.func, contexto, visitados)
                if no.func.attr in QUERYSET:
                    return inferir(no.func.value, contexto, visitados)
        return None

    def receptor(no, contexto, caminho=None):
        if caminho:
            if any(posicao > 0 for _, posicao in campo_caminho(caminho)):
                return "AMBIGUO", "caminho de relacao nao resolvido"
        modelo = inferir(no, contexto)
        if modelo:
            return "RECEPTOR=" + modelo, "inferencia sintatica local"
        return "AMBIGUO", "receptor nao resolvido"

    def receptor_call(call, contexto, caminho=None):
        if isinstance(call, ast.Call) and isinstance(call.func, ast.Attribute):
            return receptor(call.func.value, contexto, caminho)
        return "AMBIGUO", "chamada sem receptor resolvido"

    def registrar(no, campo, papeis, forma, rec, motivo, linha=None):
        for papel in papeis:
            emitir(
                relativo, linha or getattr(no, "lineno", 1),
                funcoes.get(no, "<modulo>"), campo, papel, escopo_arquivo,
                rec, motivo, forma,
            )

    def literais(no, contexto, visitados=None):
        """Segue apenas nomes com uma unica atribuicao local."""
        visitados = set() if visitados is None else visitados
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            yield no.value, no
        elif isinstance(no, (ast.List, ast.Tuple, ast.Set)):
            for item in no.elts:
                yield from literais(item, contexto, visitados)
        elif isinstance(no, ast.Name):
            ambiente = ambientes.get(contexto, arvore)
            chave = (id(ambiente), no.id)
            valores = atribuicoes[ambiente].get(no.id, [])
            if chave not in visitados and len(valores) == 1:
                yield from literais(valores[0], contexto, visitados | {chave})

    def pares_dict(no, contexto, visitados=None):
        """Extrai chaves literais, inclusive de **dicionario local simples."""
        visitados = set() if visitados is None else visitados
        if isinstance(no, ast.Dict):
            for chave, valor in zip(no.keys, no.values):
                if chave is None:
                    yield from pares_dict(valor, contexto, visitados)
                elif isinstance(chave, ast.Constant) and isinstance(chave.value, str):
                    yield chave.value, chave
        elif isinstance(no, ast.Name):
            ambiente = ambientes.get(contexto, arvore)
            chave = (id(ambiente), no.id)
            valores = atribuicoes[ambiente].get(no.id, [])
            if chave not in visitados and len(valores) == 1:
                yield from pares_dict(valores[0], contexto, visitados | {chave})

    def registrar_caminho(no, texto, papeis, forma, rec_no, contexto):
        for campo, _ in campo_caminho(texto):
            rec, motivo = receptor(rec_no, contexto, texto)
            registrar(no, campo, papeis, forma, rec, motivo)

    def chamada_orm_externa(no):
        atual = no
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.Call) and isinstance(atual.func, ast.Attribute):
                if nome_chamada(atual) in QUERYSET:
                    return atual
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        return None

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute) and no.attr in ALIASES:
            rec, motivo = receptor(no.value, no)
            if isinstance(no.ctx, ast.Store):
                papeis = ("ESCRITA",)
                if isinstance(pais.get(no), ast.AugAssign):
                    papeis = ("LEITURA", "ESCRITA")
            elif isinstance(no.ctx, ast.Del):
                papeis = ("ESCRITA",)
            else:
                papeis = ("LEITURA",)
            registrar(no, ALIASES[no.attr], papeis, "atributo", rec, motivo)

        if not isinstance(no, ast.Call):
            continue

        nome = nome_chamada(no)
        rec_no = no.func.value if isinstance(no.func, ast.Attribute) else None

        if nome in ("getattr", "setattr") and len(no.args) >= 2:
            papel = "LEITURA" if nome == "getattr" else "ESCRITA"
            for texto, origem in literais(no.args[1], no):
                for campo, pos in campo_caminho(texto):
                    rec, motivo = receptor(no.args[0], no, texto)
                    registrar(no, campo, (papel,), nome, rec, motivo)

        if nome in LEITURAS:
            for arg in no.args:
                for texto, origem in literais(arg, no):
                    registrar_caminho(
                        no, texto, ("LEITURA",), nome + " argumento",
                        rec_no, no,
                    )

        if nome in ("F", "Q"):
            externo = chamada_orm_externa(no)
            externo_rec = (
                externo.func.value
                if externo is not None and isinstance(externo.func, ast.Attribute)
                else None
            )
            if nome == "F":
                for arg in no.args:
                    for texto, origem in literais(arg, no):
                        registrar_caminho(
                            no, texto, ("LEITURA",), "F",
                            externo_rec, no,
                        )
            for kw in no.keywords:
                if kw.arg:
                    registrar_caminho(
                        no, kw.arg, ("LEITURA",), nome + " kwarg",
                        externo_rec, no,
                    )

        if nome in ("save", "bulk_update", "bulk_create"):
            argumentos_campos = []
            if nome == "save":
                argumentos_campos += [
                    kw.value for kw in no.keywords if kw.arg == "update_fields"
                ]
            elif nome == "bulk_update":
                if len(no.args) >= 2:
                    argumentos_campos.append(no.args[1])
                argumentos_campos += [
                    kw.value for kw in no.keywords if kw.arg == "fields"
                ]
            else:
                argumentos_campos += [
                    kw.value for kw in no.keywords
                    if kw.arg in ("update_fields", "unique_fields")
                ]
            for argumento in argumentos_campos:
                for texto, origem in literais(argumento, no):
                    papel = (
                        "LEITURA"
                        if nome == "bulk_create" and any(
                            kw.arg == "unique_fields" and kw.value is argumento
                            for kw in no.keywords
                        )
                        else "ESCRITA"
                    )
                    registrar_caminho(
                        no, texto, (papel,), nome + " campos", rec_no, no,
                    )

        for kw in no.keywords:
            if kw.arg is None:
                if nome in ("update", "create", "get_or_create", "update_or_create"):
                    for texto, origem in pares_dict(kw.value, no):
                        papeis = (
                            ("ESCRITA",) if nome in ("update", "create")
                            else ("LEITURA", "ESCRITA")
                        )
                        registrar_caminho(
                            no, texto, papeis, nome + " **dict", rec_no, no,
                        )
                continue

            if nome in ("get_or_create", "update_or_create") and kw.arg in (
                "defaults", "create_defaults"
            ):
                for texto, origem in pares_dict(kw.value, no):
                    registrar_caminho(
                        no, texto, ("ESCRITA",), nome + " " + kw.arg,
                        rec_no, no,
                    )
                continue

            if nome in ("Q", "F"):
                continue
            if nome in ("create", "update"):
                papeis = ("ESCRITA",)
            elif nome in ("get_or_create", "update_or_create"):
                papeis = ("LEITURA", "ESCRITA")
            elif nome in LEITURAS:
                papeis = ("LEITURA",)
            elif nome in MODELOS:
                papeis = ("ESCRITA",)
                rec_no = no.func
            else:
                continue
            registrar_caminho(no, kw.arg, papeis, nome + " kwarg", rec_no, no)

        # Um bulk_create sem lista explicita de campos pode inserir todas as
        # colunas. A evidencia sintatica isolada nao prova quais valores foram
        # enviados, portanto essas possibilidades permanecem ambiguas.
        if nome == "bulk_create":
            for campo in CAMPOS:
                registrar(
                    no, campo, ("ESCRITA",), "bulk_create insercao possivel",
                    "AMBIGUO", "campos inseridos nao resolvidos",
                )

    # Varredura conservadora de todas as strings, inclusive SQL, migrations,
    # nomes guardados em variaveis e contextos nao classificados acima.
    for no in ast.walk(arvore):
        if not isinstance(no, ast.Constant) or not isinstance(no.value, str):
            continue
        forma = "possivel SQL" if SQL.search(no.value) else "string literal"
        motivo = (
            "SQL nao interpretado" if forma == "possivel SQL"
            else "uso da string nao resolvido"
        )
        for campo, inicio in achados(no.value):
            linha = no.lineno + no.value[:inicio].count("\n")
            registrar(
                no, campo, ("LEITURA", "ESCRITA"), forma,
                "AMBIGUO", motivo, linha,
            )


# Templates: toda ocorrencia nas expressoes de tags e inventariada.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith((".html", ".htm", ".django", ".jinja", ".jinja2")):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with open(caminho, encoding="utf-8-sig") as arquivo:
                texto = arquivo.read()
        except (OSError, UnicodeError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        for tag in TAGS.finditer(texto):
            for campo, inicio in achados(tag.group()):
                linha = texto.count("\n", 0, tag.start() + inicio) + 1
                emitir(
                    relativo, linha, "<template>", campo, "LEITURA",
                    "template", "AMBIGUO",
                    "tipo e contexto da expressao nao resolvidos",
                    "expressao de template",
                )


print("arquivo\tlinha\tfuncao\tcampo\tpapel\tescopo\treceptor\tmotivo\tforma")
contagens = collections.Counter()
escritores = collections.defaultdict(set)
ambiguos = collections.Counter()

for registro in sorted(REGISTROS):
    print("\t".join(limpar(item

[truncado em 20000 de 21787 caracteres]

## 19/11 00:50 · sessao b9b3bde26b6e · QUEIMA-B1 · sonda · origem=Code

A condição “montar igual ao chamador de produção” é impossível nesta cópia: a busca encontrou a definição e chamadas nos testes, mas nenhum chamador de produção. O script abaixo usa uma **reconstrução a partir da ata lavrada**, pelo leitor existente; a matriz resultante mede a resposta do juiz a essa reconstrução, não uma chamada de produção nem necessariamente a grade original. Quando faltam campos para reconstruir o insumo, registra `NAO_MEDIDA_<motivo>`. A execução e os resultados estão [não verificado]. [app/escala/tests/test_orfa_classe.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/tests/test_orfa_classe.py:114) [app/escala/services/leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)

```python
# Executar dentro do envelope Django de leitura da sombra, tenant juliani.
# Insumo do juiz: reconstrução da ata pelo leitor existente, não chamador
# de produção. Não cria, altera nem apaga arquivos.

import collections
import datetime
import os
import sys

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from escala.models import CelulaDia
from escala.orfa_classe import classificar_orfas
from escala.services.leitor_celula import _celulas_da_ata
from ponto.juiz_batida import periodos_do_dia
from ponto.turnos import batidas_apuraveis, turnos_do_colab


INI = datetime.date(2026, 10, 21)
FIM = datetime.date(2026, 11, 20)
UM_DIA = datetime.timedelta(days=1)


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("settings")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("configuracao")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("conexao")


def minuto_local(instante):
    if timezone.is_aware(instante):
        instante = timezone.localtime(instante).replace(tzinfo=None)
    return instante.replace(second=0, microsecond=0)


def hora_valida(hora):
    if not isinstance(hora, str) or len(hora) != 5 or hora[2] != ":":
        return False
    try:
        return datetime.datetime.strptime(hora, "%H:%M").strftime("%H:%M") == hora
    except ValueError:
        return False


def indice_turnos(turnos):
    por_pk = collections.defaultdict(list)
    for turno in turnos:
        for batida in turno.batidas:
            por_pk[batida.pk].append((turno, batida))
    return por_pk


def situacao_pareador(registros):
    # Mesma precedência da sonda2.
    if len(registros) != 1:
        return "NAO_MEDIDA_MULTIPLOS_OU_SEM_TURNO"
    turno, batida = registros[0]
    if getattr(batida, "_intra_dur", False):
        return "NAO_MEDIDA_ORIGEM_PAUSA"
    if turno.entrada is not None and turno.saida is not None:
        return "PAR_FECHADO"
    if turno.entrada is not None and turno.saida is None:
        if batida.pk == turno.entrada.pk:
            return "TURNO_ABERTO"
        return "NAO_MEDIDA_BATIDA_INTERNA_TURNO_ABERTO"
    if turno.entrada is None and turno.saida is not None:
        if batida.pk == turno.saida.pk:
            return "SAIDA_SEM_ENTRADA"
        return "NAO_MEDIDA_BATIDA_INTERNA_SEM_ENTRADA"
    return "FORA_DA_ESCALA_SEM_ENQUADRAMENTO"


def classes_da_ata(ata, orfas):
    # Não chamar com tipo_dia ausente: o juiz assumiria folga.
    tipo = ata.get("tipo_dia")
    if not isinstance(tipo, str) or not tipo:
        return None, "NAO_MEDIDA_TIPO_DIA_AUSENTE"

    # Para tipo != trabalho, o juiz não consulta celulas.
    if tipo != "trabalho":
        dia = {"tipo_dia": tipo, "orfas": orfas, "celulas": []}
        return classificar_orfas(dia), None

    lampadas = ata.get("lampadas")
    if lampadas:
        if not isinstance(lampadas, list) or not all(
            isinstance(item, dict) for item in lampadas
        ):
            return None, "NAO_MEDIDA_LAMPADAS_FORMATO"
        # O leitor trata acesa=None como não missing. Se a ata não
        # permite afirmar o estado, não transformar desconhecido em fato.
        if any(item.get("acesa") is None for item in lampadas):
            return None, "NAO_MEDIDA_ESTADO_LAMPADA"
    else:
        n_missing = ata.get("n_missing")
        if type(n_missing) is not int or n_missing < 0:
            return None, "NAO_MEDIDA_N_MISSING_AUSENTE_OU_INVALIDO"

    # n_ok=0 basta para a pergunta booleana _tem_missing do juiz.
    celulas, _ = _celulas_da_ata(ata, 0)
    dia = {"tipo_dia": tipo, "orfas": orfas, "celulas": celulas}
    return classificar_orfas(dia), None


def registra(tabela, chave, exemplo):
    tabela[chave].append(exemplo)


def imprime_tabela(prefixo, tabela):
    for chave in sorted(tabela, key=lambda x: tuple(str(v) for v in x)):
        exemplos = sorted(set(tabela[chave]))[:2]
        campos = " ".join(str(v) for v in chave)
        print("%s %s ocorrencias=%d" % (prefixo, campos, len(tabela[chave])))
        for cid, dia, hora in exemplos:
            print(
                "%s_EXEMPLO %s col%d %s %s"
                % (prefixo, campos, cid, dia.strftime("%d/%m"), hora)
            )
        if len(exemplos) < 2:
            print("%s_EXEMPLOS_DISPONIVEIS %s n=%d"
                  % (prefixo, campos, len(exemplos)))


def main():
    onde = "GUARDA_SOMBRA"
    try:
        with transaction.atomic():
            guarda_sombra()

            onde = "CELULAS"
            celulas = list(
                CelulaDia.objects.filter(data__gte=INI, data__lte=FIM)
                .select_related("colaborador", "colaborador__empresa")
                .order_by("colaborador_id", "data")
            )
            por_pessoa = collections.defaultdict(list)
            for celula in celulas:
                por_pessoa[celula.colaborador_id].append(celula)

            matriz = collections.defaultdict(list)
            pausa = collections.defaultdict(list)
            inversa_dias = collections.defaultdict(set)
            inversa_batidas = collections.defaultdict(set)
            inversa_exemplos = collections.defaultdict(set)
            inversa_motivos = collections.Counter()
            empresas = set()
            pausa_total = 0

            lo = timezone.make_aware(datetime.datetime.combine(INI, datetime.time.min))
            hi = timezone.make_aware(
                datetime.datetime.combine(FIM + UM_DIA, datetime.time.min)
            )
            lo_pad = timezone.make_aware(
                datetime.datetime.combine(INI - UM_DIA, datetime.time.min)
            )
            hi_pad = timezone.make_aware(
                datetime.datetime.combine(FIM + 2 * UM_DIA, datetime.time.min)
            )

            for cid, cels in sorted(por_pessoa.items()):
                colab = cels[0].colaborador
                emp = colab.empresa_id
                empresas.add(emp)

                onde = "BATIDAS"
                por_minuto_sonda2 = collections.defaultdict(list)
                for batida in batidas_apuraveis(colab, lo, hi):
                    local = timezone.localtime(batida.timestamp)
                    if INI <= local.date() <= FIM:
                        por_minuto_sonda2[
                            (local.date(), local.strftime("%H:%M"))
                        ].append(batida)

                por_minuto_pad = collections.defaultdict(list)
                for batida in batidas_apuraveis(colab, lo_pad, hi_pad):
                    por_minuto_pad[minuto_local(batida.timestamp)].append(batida)

                onde = "TURNOS"
                turnos = turnos_do_colab(colab, INI - UM_DIA, FIM + UM_DIA)
                por_pk = indice_turnos(turnos)

                for cel in cels:
                    onde = "ATA"
                    if not isinstance(cel.ata, dict):
                        inversa_motivos[(emp, "ATA_AUSENTE")] += 1
                        continue

                    orfas = cel.ata.get("orfas")
                    if orfas is not None and not isinstance(orfas, list):
                        raise RuntimeError("orfas_formato")

                    if orfas is not None:
                        if not all(hora_valida(h) for h in orfas):
                            raise RuntimeError("hora_orfa")

                        onde = "CLASSIFICADOR"
                        classes, motivo = classes_da_ata(cel.ata, orfas)

                        for hora in orfas:
                            candidatos = por_minuto_sonda2.get(
                                (cel.data, hora), []
                            )
                            if not candidatos:
                                situacao = "SEM_BATIDA"
                                registros = []
                            elif len(candidatos) >= 2:
                                situacao = "AMBIGUO"
                                registros = []
                            else:
                                registros = por_pk.get(candidatos[0].pk, [])
                                situacao = situacao_pareador(registros)

                            razao = motivo if motivo else classes.get(hora)
                            if not razao:
                                razao = "NAO_MEDIDA_RESPOSTA_SEM_HORA"

                            exemplo = (cid, cel.data, hora)
                            chave = (
                                "emp=%s" % emp,
                                "pareador=%s" % situacao,
                                "razao_ata_reconstruida=%s" % razao,
                            )
                            registra(matriz, chave, exemplo)

                            if situacao == "NAO_MEDIDA_ORIGEM_PAUSA":
                                pausa_total += 1
                                batida_turno = registros[0][1]
                                if getattr(batida_turno, "_intra_ata", False):
                                    marca = "INTRA_ATA_SIM"
                                else:
                                    marca = "NAO_MEDIDA_OUTRA_ORIGEM_PAUSA"
                                chave_pausa = (
                                    "emp=%s" % emp,
                                    "marca=%s" % marca,
                                    "razao_ata_reconstruida=%s" % razao,
                                )
                                registra(pausa, chave_pausa, exemplo)

                    onde = "INVERSA_ATA"
                    resultado = periodos_do_dia(cel, None)
                    if resultado.get("desalinhado"):
                        inversa_motivos[(emp, "ATA_DESALINHADA")] += 1
                        continue
                    papeis = resultado.get("papeis")
                    if not isinstance(papeis, list):
                        raise RuntimeError("papeis_formato")
                    if orfas is None:
                        inversa_motivos[(emp, "LISTA_ORFAS_AUSENTE")] += 1
                        continue

                    for papel in papeis:
                        if not isinstance(papel, dict):
                            raise RuntimeError("papel_formato")
                        if papel.get("acesa") is not True:
                            continue
                        instante = papel.get("instante_luz")
                        if instante is None:
                            inversa_motivos[(emp, "LUZ_SEM_INSTANTE")] += 1
                            continue
                        if not isinstance(instante, datetime.datetime):
                            raise RuntimeError("instante_formato")
                        minuto = minuto_local(instante)
                        candidatos = por_minuto_pad.get(minuto, [])
                        if len(candidatos) != 1:
                            motivo_inv = (
                                "LUZ_SEM_BATIDA" if not candidatos
                                else "MINUTO_AMBIGUO"
                            )
                            inversa_motivos[(emp, motivo_inv)] += 1
                            continue
                        if minuto.strftime("%H:%M") in orfas:
                            inversa_motivos[(emp, "LUZ_TAMBEM_EM_ORFAS")] += 1
                            continue

                        batida = candidatos[0]
                        registros = por_pk.get(batida.pk, [])
                        if len(registros) != 1:
                            inversa_motivos[(emp, "MULTIPLOS_OU_SEM_TURNO")] += 1
                            continue

                        turno, bt = registros[0]
                        entrada_orfa = (
                            turno.entrada is not None
                            and turno.saida is None
                            and bt.pk == turno.entrada.pk
                        )
                        saida_orfa = (
                            turno.entrada is None
                            and turno.saida is not None
                            and bt.pk == turno.saida.pk
                        )
                        if not (entrada_orfa or saida_orfa):
                            continue

                        tipo_inv = (
                            "TURNO_ABERTO" if entrada_orfa else "SAIDA_SEM_ENTRADA"
                        )
                        chave_inv = (emp, tipo_inv)
                        inversa_dias[chave_inv].add((cid, cel.data))
                        inversa_batidas[chave_inv].add(batida.pk)
                        inversa_exemplos[chave_inv].add(
                            (cid, minuto.date(), minuto.strftime("%H:%M"))
                        )

            onde = "SAIDA"
            imprime_tabela("MATRIZ", matriz)
            imprime_tabela("PAUSA", pausa)
            print("PAUSA_TOTAL sobras=%d referencia_453=%s" % (
                pausa_total, "confere" if pausa_total == 453 else "diverge"
            ))

            for emp in sorted(empresas, key=str):
                total_dias = set()
                total_batidas = set()
                for tipo_inv in ("TURNO_ABERTO", "SAIDA_SEM_ENTRADA"):
                    chave_inv = (emp, tipo_inv)
                    dias = inversa_dias[chave_inv]
                    bats = inversa_batidas[chave_inv]
                    total_dias.update(dias)
                    total_batidas.update(bats)
                    print(
                        "INVERSA emp=%s pareador=%s dias=%d batidas=%d "
                        "razao=NAO_APLICAVEL_LUZ_NAO_ORFA"
                        % (emp, tipo_inv, len(dias), len(bats))
                    )
                    for cid, dia, hora in sorted(inversa_exemplos[chave_inv])[:2]:
                        print(
                            "INVERSA_EXEMPLO emp=%s pareador=%s col%d %s %s"
                            % (emp, tipo_inv, cid, dia.strftime("%d/%m"), hora)
                        )
                print(
                    "INVERSA_TOTAL emp=%s dias=%d batidas=%d"
                    % (emp, len(total_dias), len(total_batidas))
                )

            for (emp, motivo_inv), total in sorted(
                inversa_motivos.items(), key=lambda item: (str(item[0][0]), item[0][1])
            ):
                print(
                    "INVERSA_NAO_MEDIDA emp=%s motivo=%s ocorrencias=%d"
                    % (emp, motivo_inv, total)
                )

        print("ERROS=0")
        print("FIM_SONDA_B1")
        return 0

    except Exception:
        print("ERRO " + onde)
        print("ERROS=1")
        print("FIM_SONDA_B1")
        return 1


sys.exit(main())
```

Cada célula `MATRIZ` conta **ocorrências da lista de órfãs da ata**, por empresa: à esquerda, a situação da batida encontrada pelo mesmo casamento por minuto e precedência da sonda2 fornecida [não verificado]; à direita, a resposta do juiz sobre o `tipo_dia`, as órfãs e as células reconstruídas da ata. A classe não prova a causa da decisão do pareador. O juiz dá `folga_desmentida` para qualquer `tipo_dia` diferente de `trabalho`; para trabalho, dá `indefinida` sem missing, ou `eco_cluster`/`geometria` conforme a sequência de órfãs quando há missing. [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53)

A reconstrução tem limite material: o cartório lavra `n_missing` a partir das lâmpadas quando elas existem, e o leitor reconstrói células dessas lâmpadas ou da contagem agregada. Isso não demonstra que a lista reconstruída seja idêntica à grade que originou a ata. Por isso os rótulos dizem `razao_ata_reconstruida`. [app/ponto/services/cartorio.py:736](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/services/cartorio.py:736) [app/escala/services/leitor_celula.py:114](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/services/leitor_celula.py:114)

Na tabela `PAUSA`, `INTRA_ATA_SIM` informa que o pareador deixou o carimbo associado ao papel `X` ou `Xi`; não prova exclusividade de origem, pois outras regras também podem ter marcado `_intra_dur`. Sem esse carimbo, `NAO_MEDIDA_OUTRA_ORIGEM_PAUSA` preserva a origem desconhecida. A comparação com 453 é uma conferência da referência transcrita, cujo resultado nesta execução está [não verificado]. [app/ponto/turnos.py:1305](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1305) [app/ponto/turnos.py:453](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:453)

Na inversa, a batida medida acendeu uma luz e seu horário não consta nas órfãs daquela ata; o classificador recebe somente órfãs, portanto a classificação dessa luz é declarada não aplicável. Os motivos de exclusão são contagens de ocorrências, não necessariamente de dias distintos. [app/escala/orfa_classe.py:58](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:58) [não verificado]

**perguntas de lei: N** [não verificado]

## 19/11 00:49 · sessao fcbab515d637 · QUEIMA-B2-disputa · censo · origem=Code

O script abaixo é para ser executado pela sessão com `python3 -B script.py RAIZ`. Sua execução e seus resultados estão [não verificado].

```python
#!/usr/bin/env python3
# Uso: python3 -B script.py RAIZ
# Somente leitura. A saida e um inventario sintatico conservador.
import ast
import collections
import os
import re
import sys
import tokenize

sys.dont_write_bytecode = True

if len(sys.argv) != 2:
    sys.stderr.write("Uso: python3 -B script.py RAIZ\n")
    sys.exit(2)

RAIZ = os.path.abspath(sys.argv[1])
APP = os.path.join(RAIZ, "app")
if not os.path.isdir(APP):
    sys.stderr.write("ERRO: RAIZ deve conter app/\n")
    sys.exit(2)

CAMPOS = (
    "motivos",
    "fechada_em",
    "reaberturas_colab",
    "reaberturas_admin",
    "fechada_por",
    "resolucao",
    "motivo_fechamento",
)
ALVOS = set(CAMPOS)
# Django permite acessar uma ForeignKey tambem pelo sufixo _id.
ALIASES = {c: c for c in CAMPOS}
ALIASES["fechada_por_id"] = "fechada_por"

PADRAO = re.compile(
    r"(?<![A-Za-z0-9_])(?:"
    + "|".join(re.escape(c) for c in sorted(ALIASES, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)
SQL = re.compile(
    r"\b(select|insert|update|delete|with|returning|where|from|join)\b",
    re.I,
)
TAGS_TEMPLATE = re.compile(r"{{.*?}}|{%.*?%}", re.S)
ATRIBUTO_TEMPLATE = re.compile(
    r"\.\s*("
    + "|".join(re.escape(c) for c in sorted(ALIASES, key=len, reverse=True))
    + r")(?![A-Za-z0-9_])"
)

LEITURAS = {
    "filter", "exclude", "get", "values", "values_list", "order_by",
    "annotate", "alias", "only", "defer", "distinct", "earliest", "latest",
}
QUERYSET = LEITURAS | {
    "all", "none", "select_for_update", "select_related", "prefetch_related",
    "using", "exclude", "reverse", "iterator", "first", "last",
    "get_or_create", "update_or_create", "create", "update", "bulk_create",
    "bulk_update",
}
REGISTROS = set()
FALHAS = []
MODELOS = set()
ARVORES = []

def limpar(valor):
    return str(valor).replace("\t", " ").replace("\r", " ").replace("\n", " ")

def categoria(relativo):
    partes = relativo.lower().replace("\\", "/").split("/")
    nome = partes[-1]
    if "migrations" in partes:
        return "migration"
    if (
        "tests" in partes or "test" in partes
        or nome.startswith("test_") or nome.endswith("_test.py")
    ):
        return "teste" [nao verificado]
    return "prod"

def chamada(no):
    if not isinstance(no, ast.Call):
        return ""
    if isinstance(no.func, ast.Name):
        return no.func.id
    if isinstance(no.func, ast.Attribute):
        return no.func.attr
    return ""

def campo_exato(texto):
    if not isinstance(texto, str):
        return None
    return ALIASES.get(texto.lstrip("-"))

def ocorrencias(texto):
    for achado in PADRAO.finditer(texto):
        yield ALIASES[achado.group()], achado.start()

def emit(relativo, linha, escopo, campo, papel, classe, receptor, motivo, forma):
    REGISTROS.add((
        relativo, int(linha), escopo or "<modulo>", campo, papel, classe,
        receptor, motivo, forma,
    ))

def filhos_sem_escopos(no):
    """Percorre um escopo sem entrar em funcoes ou classes aninhadas."""
    pilha = list(ast.iter_child_nodes(no))
    while pilha:
        atual = pilha.pop()
        yield atual
        if not isinstance(
            atual, (ast.FunctionDef, ast.AsyncFunctionDef, ast.ClassDef, ast.Lambda)
        ):
            pilha.extend(ast.iter_child_nodes(atual))

def nome_base(no):
    if isinstance(no, ast.Name):
        return no.id
    if isinstance(no, ast.Attribute):
        return no.attr
    return ""

# Primeiro passe: ler fontes e identificar classes com base models.Model/Model.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith(".py"):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with tokenize.open(caminho) as arquivo:
                fonte = arquivo.read()
            arvore = ast.parse(fonte, filename=relativo)
        except (OSError, SyntaxError, UnicodeError, LookupError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        ARVORES.append((relativo, arvore))
        for no in ast.walk(arvore):
            if isinstance(no, ast.ClassDef):
                if any(nome_base(base) == "Model" for base in no.bases):
                    MODELOS.add(no.name)

MODELOS.add("DisputaSupervisao")

for relativo, arvore in ARVORES:
    classe_arquivo = categoria(relativo)
    pais = {}
    escopos = {}
    classes = {}
    ambientes = {}

    for pai in ast.walk(arvore):
        for filho in ast.iter_child_nodes(pai):
            pais[filho] = pai

    def indexar(no, pilha, classe_atual, ambiente):
        if isinstance(no, ast.ClassDef):
            pilha = pilha + [no.name]
            classe_atual = no.name if no.name in MODELOS else None
            ambiente = no
        elif isinstance(no, (ast.FunctionDef, ast.AsyncFunctionDef)):
            pilha = pilha + [no.name]
            ambiente = no
        elif isinstance(no, ast.Lambda):
            ambiente = no
        escopos[no] = ".".join(pilha) or "<modulo>"
        classes[no] = classe_atual
        ambientes[no] = ambiente
        for filho in ast.iter_child_nodes(no):
            indexar(filho, pilha, classe_atual, ambiente)

    indexar(arvore, [], None, arvore)

    # Cada nome guarda todas as atribuicoes visiveis no mesmo escopo.
    # Atribuicoes contraditorias ou nao resolvidas tornam o receptor ambiguo.
    atribuicoes = collections.defaultdict(lambda: collections.defaultdict(list))
    anotacoes = collections.defaultdict(lambda: collections.defaultdict(list))

    for ambiente in [
        n for n in ast.walk(arvore)
        if isinstance(n, (ast.Module, ast.ClassDef, ast.FunctionDef,
                          ast.AsyncFunctionDef, ast.Lambda))
    ]:
        if isinstance(ambiente, (ast.FunctionDef, ast.AsyncFunctionDef, ast.Lambda)):
            args = ambiente.args
            todos = list(args.posonlyargs) + list(args.args) + list(args.kwonlyargs)
            if args.vararg:
                todos.append(args.vararg)
            if args.kwarg:
                todos.append(args.kwarg)
            for arg in todos:
                if arg.annotation is not None:
                    anotacoes[ambiente][arg.arg].append(arg.annotation)

        for no in filhos_sem_escopos(ambiente):
            if ambientes.get(no) is not ambiente:
                continue
            if isinstance(no, ast.Assign):
                for alvo in no.targets:
                    if isinstance(alvo, ast.Name):
                        atribuicoes[ambiente][alvo.id].append(no.value)
            elif isinstance(no, ast.AnnAssign) and isinstance(no.target, ast.Name):
                anotacoes[ambiente][no.target.id].append(no.annotation)
                if no.value is not None:
                    atribuicoes[ambiente][no.target.id].append(no.value)
            elif isinstance(no, (ast.For, ast.AsyncFor)) and isinstance(no.target, ast.Name):
                atribuicoes[ambiente][no.target.id].append(no.iter)

    def modelo_anotacao(no):
        if isinstance(no, ast.Constant) and isinstance(no.value, str):
            texto = no.value
            encontrados = {m for m in MODELOS if re.search(
                r"(?<![A-Za-z0-9_])" + re.escape(m) + r"(?![A-Za-z0-9_])",
                texto,
            )}
        else:
            encontrados = {
                x.id for x in ast.walk(no)
                if isinstance(x, ast.Name) and x.id in MODELOS
            }
            encontrados |= {
                x.attr for x in ast.walk(no)
                if isinstance(x, ast.Attribute) and x.attr in MODELOS
            }
        return next(iter(encontrados)) if len(encontrados) == 1 else None

    def inferir(no, contexto, visitados=None):
        """Retorna nome de modelo apenas quando a sintaxe oferece evidencia."""
        if no is None:
            return None
        if visitados is None:
            visitados = set()
        ambiente = ambientes.get(contexto, arvore)
        if isinstance(no, ast.Name):
            if no.id == "self" and classes.get(contexto):
                return classes[contexto]
            if no.id in MODELOS:
                # Um nome local atribuido pode sombrear o nome da classe.
                if no.id not in atribuicoes[ambiente]:
                    return no.id
                return None
            chave = (id(ambiente), no.id)
            if chave in visitados:
                return None
            visitados = visitados | {chave}
            anns = anotacoes[ambiente].get(no.id, [])
            vals = atribuicoes[ambiente].get(no.id, [])
            resultados = [modelo_anotacao(x) for x in anns]
            resultados += [inferir(x, x, visitados) for x in vals]
            if resultados and None not in resultados and len(set(resultados)) == 1:
                return resultados[0]
            return None
        if isinstance(no, ast.Attribute):
            if no.attr in MODELOS and isinstance(no.value, ast.Name):
                return no.attr
            if no.attr in ("objects", "_default_manager", "_base_manager"):
                return inferir(no.value, contexto, visitados)
            return None
        if isinstance(no, ast.Subscript):
            return inferir(no.value, contexto, visitados)
        if isinstance(no, ast.Call):
            if isinstance(no.func, ast.Name) and no.func.id in MODELOS:
                return inferir(no.func, contexto, visitados)
            if isinstance(no.func, ast.Attribute):
                if no.func.attr in MODELOS:
                    return inferir(no.func, contexto, visitados)
                if no.func.attr in QUERYSET:
                    return inferir(no.func.value, contexto, visitados)
        return None

    def receptor(no, contexto, caminho=None):
        modelo = inferir(no, contexto)
        # Um caminho de relacao pode apontar para outro modelo.
        if caminho:
            partes = caminho.lstrip("-").split("__")
            posicoes = [i for i, p in enumerate(partes) if p in ALIASES]
            if posicoes and posicoes[0] > 0:
                return ("AMBIGUO", "caminho de relacao nao resolvido")
        if modelo:
            return ("RECEPTOR=" + modelo, "inferencia sintatica")
        return ("AMBIGUO", "receptor nao resolvido")

    def registrar(no, campo, papeis, forma, rec=None, motivo=None, linha=None):
        if rec is None:
            rec, motivo = "AMBIGUO", "receptor nao resolvido"
        for papel in papeis:
            emit(
                relativo, linha or getattr(no, "lineno", 1),
                escopos.get(no, "<modulo>"), campo, papel, classe_arquivo,
                rec, motivo or "", forma,
            )

    def contexto_externo(no):
        """Acha uma chamada ORM externa, por exemplo update(x=F(...))."""
        atual = no
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.Call) and isinstance(atual.func, ast.Attribute):
                if chamada(atual) in QUERYSET:
                    return atual
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        return None

    def receptor_chamada(call, contexto, caminho=None):
        if call and isinstance(call.func, ast.Attribute):
            return receptor(call.func.value, contexto, caminho)
        return ("AMBIGUO", "chamada sem receptor de modelo")

    def identificar_string(no):
        """Classifica uma string literal pelo contexto sintatico mais proximo."""
        valor = no.value
        atual = no
        keyword_visto = None
        while atual in pais:
            atual = pais[atual]
            if isinstance(atual, ast.keyword) and keyword_visto is None:
                keyword_visto = atual.arg
            if isinstance(atual, ast.Call):
                nome = chamada(atual)
                if nome in ("getattr", "setattr"):
                    if len(atual.args) >= 2 and atual.args[1] is no:
                        rec, motivo = receptor(atual.args[0], no)
                        papel = "LEITURA" if nome == "getattr" else "ESCRITA"
                        return (papel,), nome, rec, motivo
                if nome in ("F", "Q"):
                    externo = contexto_externo(atual)
                    rec, motivo = receptor_chamada(externo, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome in ("raw", "execute", "executemany") and SQL.search(valor):
                    return (
                        ("LEITURA", "ESCRITA"), "SQL cru",
                        "AMBIGUO", "SQL nao interpretado",
                    )
                if nome == "save" and keyword_visto == "update_fields":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "save(update_fields)", rec, motivo
                if nome == "bulk_update":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "bulk_update(fields)", rec, motivo
                if nome == "bulk_create" and keyword_visto == "update_fields":
                    rec, motivo = receptor_chamada(atual, no)
                    return ("ESCRITA",), "bulk_create(update_fields)", rec, motivo
                if nome in ("create", "update"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("ESCRITA",), nome, rec, motivo
                if nome in ("get_or_create", "update_or_create"):
                    rec, motivo = receptor_chamada(atual, no, valor)
                    if keyword_visto in ("defaults", "create_defaults"):
                        return ("ESCRITA",), nome + " defaults", rec, motivo
                    return ("LEITURA", "ESCRITA"), nome, rec, motivo
                if nome in LEITURAS:
                    rec, motivo = receptor_chamada(atual, no, valor)
                    return ("LEITURA",), nome, rec, motivo
                if nome in MODELOS:
                    rec, motivo = receptor(atual.func, no)
                    return ("ESCRITA",), "construtor de modelo", rec, motivo
            if isinstance(atual, (ast.stmt, ast.Lambda)):
                break
        if SQL.search(valor):
            return (
                ("LEITURA", "ESCRITA"), "possivel SQL",
                "AMBIGUO", "string SQL nao interpretada",
            )
        return (
            ("LEITURA", "ESCRITA"), "string fora de contexto reconhecido",
            "AMBIGUO", "uso da string nao resolvido",
        )

    for no in ast.walk(arvore):
        if isinstance(no, ast.Attribute) and no.attr in ALIASES:
            campo = ALIASES[no.attr]
            rec, motivo = receptor(no.value, no)
            if isinstance(no.ctx, ast.Store):
                papeis = ("ESCRITA",)
                if isinstance(pais.get(no), ast.AugAssign):
                    papeis = ("LEITURA", "ESCRITA")
            elif isinstance(no.ctx, ast.Del):
                papeis = ("ESCRITA",)
            else:
                papeis = ("LEITURA",)
            registrar(no, campo, papeis, "atributo", rec, motivo)

        elif isinstance(no, ast.keyword) and no.arg:
            campo = next((c for c, _ in ocorrencias(no.arg)), None)
            if not campo:
                continue
            call = pais.get(no)
            nome = chamada(call)
            if nome in ("Q", "F"):
                externo = contexto_externo(call)
                rec, motivo = receptor_chamada(externo, no, no.arg)
                papeis = ("LEITURA",)
            elif nome in ("create", "update"):
                rec, motivo = receptor_chamada(call, no, no.arg)
                papeis = ("ESCRITA",)
            elif nome in ("get_or_create", "update_or_create"):
                rec, motivo = receptor_chamada(call, no, no.arg)
                papeis = ("LEITURA", "ESCRITA")
            elif nome in LEITURAS:
                rec, motivo = receptor_chamada(call, no, no.arg)
                papeis = ("LEITURA",)
            elif nome in MODELOS:
                rec, motivo = receptor(call.func, no)
                papeis = ("ESCRITA",)
            else:
                rec, motivo = "AMBIGUO", "kwarg em chamada nao reconhecida"
                papeis = ("LEITURA", "ESCRITA")
            registrar(no, campo, papeis, "kwarg " + (nome or "?"), rec, motivo)

        elif isinstance(no, ast.Constant) and isinstance(no.value, str):
            achados = list(ocorrencias(no.value))
            if not achados:
                continue
            papeis, forma, rec, motivo = identificar_string(no)
            for campo, inicio in achados:
                linha = no.lineno + no.value[:inicio].count("\n")
                registrar(no, campo, papeis, forma, rec, motivo, linha)

# Templates em qualquer diretorio sob app/, inclusive templates de aplicativos.
for diretorio, dirs, arquivos in os.walk(APP):
    dirs.sort()
    for nome in sorted(arquivos):
        if not nome.endswith((".html", ".htm", ".django", ".jinja", ".jinja2")):
            continue
        caminho = os.path.join(diretorio, nome)
        relativo = os.path.relpath(caminho, RAIZ).replace(os.sep, "/")
        try:
            with open(caminho, encoding="utf-8-sig") as arquivo:
                texto = arquivo.read()
        except (OSError, UnicodeError) as erro:
            FALHAS.append((relativo, str(erro)))
            continue
        for tag in TAGS_TEMPLATE.finditer(texto):
            conteudo = tag.group()
            if conteudo.startswith("{%") and not re.match(
                r"{%\s*(if|elif)\b", conteudo
            ):
                continue
            for achado in ATRIBUTO_TEMPLATE.finditer(conteudo):
                linha = texto.count("\n", 0, tag.start() + achado.start()) + 1
                emit(
                    relativo, linha, "<template>", ALIASES[achado.group(1)],
                    "LEITURA", "template", "AMBIGUO",
                    "tipo da variavel do template nao resolvido",
                    "expressao de template",
                )

print("arquivo\tlinha\tfuncao\tcampo\tpapel\tescopo\treceptor\tmotivo\tforma")
contagens = collections.Counter()
for registro in sorted(REGISTROS):
    print("\t".join(limpar(item) for item in registro))
    _, _, _, campo, papel, escopo, _, _, _ = registro
    contagens[(campo, papel, escopo)] += 1

print("RESUMO\tcampo\tpapel\tescopo\tquantidade")
for campo in CAMPOS:
    for papel in ("ESCRITA", "LEITURA"):
        for escopo in ("prod", "teste", "migration", "template"): [nao verificado]
            print(
                "CONTAGEM\t%s\t%s\t%s\t%d"
                % (campo, papel, escopo, contagens[(campo, papel, escopo)])
            )

if FALHAS:
    for arquivo, erro in sorted(FALHAS):
        sys.stderr.write("ERRO\t%s\t%s\n" % (limpar(arquivo), limpar(erro)))
    sys.stderr.write("CENSO_INCOMPLETO\n")
    sys.exit(1)

print("FIM_CENSO_B2-disputa")
```

Limites declarados [não verificado]: o script não resolve fluxo entre funções, nomes ou SQL montados dinamicamente, conteúdo de variáveis usadas como `fields`, `defaults` ou `update_fields`, relações Django, herança de modelos, nem o tipo de variáveis de templates. Um `Q` guardado em variável, como o de [models.py:2860](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2860), permanece ambíguo mesmo quando aplicado depois em [models.py:2862](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2862). Strings fora dos contextos reconhecidos e SQL são classificados conservadoramente; o resumo conta ocorrências sintáticas, inclusive ambíguas, e não comprova persistência ou execução em produção [não verificado].

## 19/11 00:48 · sessao ccb234384925 · QUEIMA-B4 · sonda · origem=Code

A sonda abaixo não foi executada nesta sessão. [não verificado] Ela usa a competência que contém cada dia e o instante atual da leitura; os resultados serão a medida atual da sombra, sujeitos às falhas ou ausências que ela imprimir. [ponto/janelas.py:60] [ponto/management/commands/e6_oraculo.py:182]

```python
# Executar dentro do Django pelo envelope de leitura, no tenant juliani.
# Não cria, edita ou apaga arquivos; não imprime nomes nem CPF.
# A transação READ ONLY também impede escritas feitas por funções chamadas.
# Resultado atual, não reconstrução da decisão histórica.

import datetime as dt
import os

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone

from colaboradores.models import Colaborador
from escala.alimentacao import vinculo_do_dia_carregado
from escala.models import CelulaDia
from chamados.juizes import _slot_esperado
from chamados.services.validacao import classe_do_lote
from ponto import janelas
from ponto.management.commands.e6_oraculo import (
    InsumosDoDono,
    batida_fora_de_turno,
    dono_da_divergencia,
)
from ponto.models import Batida
from ponto.supra_juiz import cadastro_do_dia_carregado
from ponto.turnos import batidas_apuraveis


CASOS_A = {
    301: ("2026-10-22", "2026-10-29", "2026-11-05"),
    634: ("2026-10-27",),
    968: ("2026-11-02", "2026-11-03", "2026-11-05",
          "2026-11-10", "2026-11-12"),
}
CASOS_B = {
    129: ("2026-10-25", "2026-11-01", "2026-11-02",
          "2026-11-08", "2026-11-15"),
    155: ("2026-10-25", "2026-11-01", "2026-11-02", "2026-11-15"),
}
PARES = (
    (40, 139931, 135364),
    (60, 152873, 144882),
    (200, 146569, 144414),
    (263, 146777, 142735),
    (301, 151816, 149559),
    (444, 140817, 139776),
    (446, 144666, 136990),
    (549, 151894, 151065),
    (727, 140816, 136998),
    (881, 154095, 154081),
    (899, 151735, 136112),
    (909, 149856, 148650),
    (996, 153862, 140804),
    (996, 153847, 146242),
)

ARQ_DONO = "app/ponto/management/commands/e6_oraculo.py"
ARQ_LOTE = "app/chamados/services/validacao.py"
ERROS = 0


def guarda_sombra():
    if os.environ.get("DJANGO_SETTINGS_MODULE") != "config.settings.sombra":
        raise RuntimeError("SETTINGS")
    banco = settings.DATABASES["default"]
    if banco.get("NAME") != "sombra" or banco.get("HOST") != "sombra_db":
        raise RuntimeError("CONFIGURACAO")
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        banco_real, schema_real = cursor.fetchone()
    if banco_real != "sombra" or schema_real != "juliani":
        raise RuntimeError("CONEXAO")


def local(instante):
    if instante is None:
        return None
    if timezone.is_aware(instante):
        instante = timezone.localtime(instante)
    return instante.isoformat()


def batida_resumo(b):
    return {
        "pk": b.pk,
        "instante_local": local(b.timestamp),
        "tipo": b.tipo,
        "origem": b.origem,
    }


def janela_civil(dia):
    zona = timezone.get_current_timezone()
    inicio = timezone.make_aware(dt.datetime.combine(dia, dt.time.min), zona)
    fim = timezone.make_aware(
        dt.datetime.combine(dia + dt.timedelta(days=1), dt.time.min), zona
    )
    return inicio, fim


def clausula(horas, cadastro, fora, fase, cxr):
    if (cadastro["folga_nao_definida"]
            or cadastro["sem_vinculo_no_dia"]
            or cadastro["sem_vinculo_em_dia_de_trabalho"]):
        return ARQ_DONO + ":98"
    if fora:
        return ARQ_DONO + ":104"
    if len(horas or ()) % 2:
        return ARQ_DONO + ":106"
    if fase:
        return ARQ_DONO + ":108"
    if cxr is None:
        return "NAO_MEDIDA_CXR"
    return ARQ_DONO + ":110"


def medir_dono(etapa, cid, dia, agora):
    colab = Colaborador.objects.select_related("empresa").get(pk=cid)
    ini, fim = janelas.janela_atual(dia, colab.empresa)
    hoje = timezone.localtime(agora).date()
    ins = InsumosDoDono(colab, ini, fim, hoje, agora=agora)

    horas = ins.horas.get(str(dia), [])
    cadastro = cadastro_do_dia_carregado(cid, dia)
    fora = batida_fora_de_turno(colab, dia, ins.turnos())
    fase = str(dia)[:10] in ins.fase()

    # A chamada oficial decide quando precisa consultar CXR.
    dono = ins.dono(dia)

    # Também tentamos medir CXR quando a precedência não precisou dele.
    # O savepoint isola uma eventual tentativa de escrita ou erro do motor.
    cxr = None
    falha_cxr = None
    try:
        with transaction.atomic():
            cxr = str(dia) in ins.cxr()
    except Exception as exc:
        falha_cxr = type(exc).__name__

    linha = clausula(horas, cadastro, fora, fase, cxr)
    if linha != "NAO_MEDIDA_CXR":
        conferido = dono_da_divergencia(
            horas, cxr if cxr is not None else False,
            batida_fora_de_turno=fora,
            fase_deslocada=fase,
            **cadastro,
        )
        if conferido != dono:
            raise RuntimeError("DONO_DIVERGE_DA_RECONSTITUICAO")

    print(
        etapa, "col%d" % cid, dia,
        "competencia=", (str(ini), str(fim)),
        "hoje=", str(hoje), "agora=", local(agora),
        "horas_do_dia=", horas,
        "folga_nao_definida=", cadastro["folga_nao_definida"],
        "sem_vinculo_no_dia=", cadastro["sem_vinculo_no_dia"],
        "sem_vinculo_em_dia_de_trabalho=",
        cadastro["sem_vinculo_em_dia_de_trabalho"],
        "batida_fora_de_turno=", fora,
        "fase_deslocada=", fase,
        "em_cadastro_x_realidade=",
        cxr if falha_cxr is None else "NAO_MEDIDA_" + falha_cxr,
        "dono=", dono, "clausula=", linha,
        "porta=", ARQ_DONO + ":182",
    )

    if etapa != "B":
        return

    vinc = vinculo_do_dia_carregado(cid, dia)
    if vinc is None:
        print("B_VINCULO", "col%d" % cid, dia, "juiza=None")
    else:
        print(
            "B_VINCULO", "col%d" % cid, dia,
            "pk=", vinc.pk, "inicio=", vinc.data_inicio,
            "fim=", vinc.data_fim, "ativa=", vinc.ativa,
            "tipo_escala_id=", vinc.tipo_escala_id,
            "fonte=app/escala/alimentacao.py:296",
        )

    celulas = list(CelulaDia.objects.filter(
        colaborador_id=cid, data=dia
    ).order_by("pk"))
    if not celulas:
        print("B_CELULA", "col%d" % cid, dia, "AUSENTE")
    for cel in celulas:
        dna = cel.dna if isinstance(cel.dna, dict) else None
        # Chaves conhecidas do produtor; evita imprimir eventual texto pessoal.
        chaves = (
            "dna_versao", "vinculo_id", "tipo_escala_id", "tipo_ciclo",
            "marcos", "minutos_jornada", "intervalo_livre",
            "lampadas", "extra_declarada",
        )
        dna_saida = None if dna is None else {
            chave: dna[chave] for chave in chaves if chave in dna
        }
        print(
            "B_CELULA", "col%d" % cid, dia,
            "pk=", cel.pk, "trabalha=", cel.trabalha,
            "origem=", cel.origem,
            "escala_geradora_id=", cel.escala_geradora_id,
            "dna=", dna_saida,
            "fonte=app/escala/models.py:1681",
        )

    lo, hi = janela_civil(dia)
    batidas = batidas_apuraveis(colab, lo, hi).filter(timestamp__lt=hi)
    print(
        "B_BATIDAS_DIA_CIVIL", "col%d" % cid, dia,
        [batida_resumo(b) for b in batidas],
        "fonte=app/ponto/turnos.py:2868",
    )


def pergunta_resumo(p):
    return {
        "pergunta_pk": p.pk,
        "disputa_id": p.disputa_id,
        "celula_id": p.celula_id,
        "chamado_destino_id": p.chamado_destino_id,
        "batida_origem_id": p.batida_origem_id,
        "data_evento": str(p.data_evento) if p.data_evento else None,
        "motivo": p.motivo,
        "instancia_marco": p.instancia_marco,
        "tipo_resposta": p.tipo_resposta,
        "respondida_em": local(p.respondida_em),
        "validada_em": local(p.validada_em),
        "validada_por_id": p.validada_por_id,
        "materializada_em": local(p.materializada_em),
        "materializada_por_id": p.materializada_por_id,
        "via_resolucao": p.via_resolucao,
    }


def medir_c():
    dia = dt.date(2026, 11, 3)
    lo, hi = janela_civil(dia)
    batidas = list(Batida.objects.filter(
        colaborador_id=200,
        origem="disputa_s84_retro",
        timestamp__gte=lo,
        timestamp__lt=hi,
    ).select_related("pergunta_origem").order_by("timestamp", "pk"))

    if not batidas:
        print("C col200 2026-11-03 NENHUMA_BATIDA_COM_ESSA_ORIGEM")
    for b in batidas:
        print(
            "C_BATIDA", batida_resumo(b),
            "aprovada=", b.aprovada,
            "aprovada_por_id=", b.aprovada_por_id,
            "aprovada_em=", local(b.aprovada_em),
            "criada_em=", local(b.criado_em),
            "criador_id=NAO_EXISTE_CAMPO_NO_MODELO",
            "pergunta_origem_id=", b.pergunta_origem_id,
            "retratada_em=", local(b.retratada_em),
            "fonte=app/ponto/models.py:8",
        )
        p = b.pergunta_origem
        if p is None:
            print(
                "C_PERGUNTA batida_pk=", b.pk,
                "NAO_MEDIDA_PERGUNTA_ORIGEM_NULA",
                "fonte=app/ponto/models.py:40",
            )
            continue
        print(
            "C_PERGUNTA batida_pk=", b.pk, pergunta_resumo(p),
            "fonte=app/chamados/models.py:2593",
        )
        atos = list(p.decisoes.order_by("ato_em", "pk"))
        print(
            "C_ATOS pergunta_pk=", p.pk,
            [{
                "pk": a.pk,
                "ato": a.ato,
                "ato_em": local(a.ato_em),
                "ato_por_id": a.ato_por_id,
                "validada_original_em": local(a.validada_original_em),
                "validada_original_por_id": a.validada_original_por_id,
            } for a in atos],
            "fonte=app/chamados/models.py:4166",
            "vinculo_ato_batida=NAO_VERIFICADO",
        )


def medir_d(cid, plantada_pk, real_pk):
    b = Batida.objects.select_related(
        "colaborador", "pergunta_origem",
        "pergunta_origem__disputa",
        "pergunta_origem__disputa__colaborador",
    ).get(pk=plantada_pk, colaborador_id=cid)

    print(
        "D_PLANTADA", "col%d" % cid,
        "real_referencia_pk=", real_pk,
        "batida=", batida_resumo(b),
        "pergunta_origem_id=", b.pergunta_origem_id,
        "fonte=app/ponto/models.py:40",
    )
    p = b.pergunta_origem
    if p is None:
        print(
            "D_RESULTADO plantada_pk=", plantada_pk,
            "NAO_MEDIDA_PERGUNTA_ORIGEM_NULA",
            "slot=NAO_MEDIDO batidas=NAO_MEDIDAS classe=NAO_MEDIDA",
        )
        return
    if p.disputa.colaborador_id != cid:
        print(
            "D_RESULTADO plantada_pk=", plantada_pk,
            "NAO_MEDIDA_COLABORADOR_DA_DISPUTA_DIVERGENTE",
        )
        return

    print("D_PERGUNTA plantada_pk=", plantada_pk, pergunta_resumo(p))

    # Cada chamada fica isolada: uma falha do slot não impede tentar a classe.
    slot = None
    try:
        with transaction.atomic():
            slot, tipo_slot = _slot_esperado(p)
            if slot is None:
                print(
                    "D_SLOT plantada_pk=", plantada_pk,
                    "NAO_MEDIDA_SLOT_INDISPONIVEL",
                    "fonte=app/chamados/juizes.py:345",
                )
            else:
                perto = dt.timedelta(minutes=60)
                proximas = list(batidas_apuraveis(
                    p.disputa.colaborador, slot - perto, slot + perto
                ))
                print(
                    "D_SLOT plantada_pk=", plantada_pk,
                    "instante_local=", local(slot), "tipo=", tipo_slot,
                    "batidas_apuraveis_inclusivas_mais_menos_60min=",
                    [batida_resumo(x) for x in proximas],
                    "fonte=app/chamados/services/validacao.py:1204",
                )
    except Exception as exc:
        print(
            "D_SLOT plantada_pk=", plantada_pk,
            "NAO_MEDIDA_" + type(exc).__name__,
        )

    try:
        with transaction.atomic():
            classe, detalhe = classe_do_lote(p)
            # Não imprimir resposta livre, que pode conter nome ou CPF.
            detalhe_seguro = {
                k: detalhe[k] for k in
                ("delta_min", "ampm", "esperada", "sinal_min", "tipo",
                 "conflito", "porque")
                if k in detalhe
            }
            conflito = (
                detalhe["conflito"] if "conflito" in detalhe
                else "NAO_INFORMADO_PELA_FUNCAO"
            )
            print(
                "D_RESULTADO plantada_pk=", plantada_pk,
                "classe=", classe, "conflito=", conflito,
                "detalhe=", detalhe_seguro,
                "fonte=", ARQ_LOTE + ":1177",
                "estado=ATUAL",
            )
    except Exception as exc:
        print(
            "D_RESULTADO plantada_pk=", plantada_pk,
            "NAO_MEDIDA_" + type(exc).__name__,
        )


def executar_caso(rotulo, funcao, *args):
    global ERROS
    try:
        # Isola erro de SQL sem contaminar os demais casos.
        with transaction.atomic():
            funcao(*args)
    except Exception as exc:
        ERROS += 1
        print(rotulo, "NAO_MEDIDA_" + type(exc).__name__)


def main():
    global ERROS
    try:
        with transaction.atomic():
            guarda_sombra()
            agora = timezone.now()

            for etapa, casos in (("A", CASOS_A), ("B", CASOS_B)):
                for cid, dias in casos.items():
                    for texto_dia in dias:
                        dia = dt.date.fromisoformat(texto_dia)
                        executar_caso(
                            "%s col%d %s" % (etapa, cid, dia),
                            medir_dono, etapa, cid, dia, agora,
                        )

            executar_caso("C col200 2026-11-03", medir_c)

            for cid, plantada_pk, real_pk in PARES:
                executar_caso(
                    "D col%d plantada_pk=%d" % (cid, plantada_pk),
                    medir_d, cid, plantada_pk, real_pk,
                )
    except Exception as exc:
        ERROS += 1
        print("ERRO_GUARDA_OU_TRANSACAO", type(exc).__name__)

    # ERROS conta exceções não tratadas por caso ou na guarda; os campos
    # NAO_MEDIDA devem ser examinados mesmo quando ERROS=0.
    print("ERROS=%d" % ERROS)
    print("FIM_SONDA_B4")


main()
```

perguntas de lei: 0

## 19/11 00:48 · sessao 01dff5dc6f8e · QUEIMA-B2-pergunta · censo · origem=Code

**O censo permanece parcial.** A AST identificou as 14 colunas declaradas em `PerguntaDisputa` e as 11 que pertencem a outras entidades, mas não consegui classificar exaustivamente todos os escritores e leitores por coluna, nem produzir o script completo solicitado. Os exemplos abaixo são resultados verificados ou explicitamente marcados como ambíguos; não são uma lista exaustiva. [não verificado quanto à completude]

### 1. Declarações das 25 colunas

| Coluna | Entidade e declaração |
|---|---|
| `status_local` | `ChamadoColaborador`: [models.py:58](app/chamados/models.py#L58) |
| `resolvido_em` | `ChamadoColaborador`: [models.py:66](app/chamados/models.py#L66) |
| `via_encerramento` | `ChamadoColaborador`: [models.py:60](app/chamados/models.py#L60) |
| `respondido_por` | `ChamadoColaborador`: [models.py:64](app/chamados/models.py#L64) |
| `fechada_em` | `DisputaSupervisao`: [models.py:1894](app/chamados/models.py#L1894) |
| `fechada_por` | `DisputaSupervisao`: [models.py:1903](app/chamados/models.py#L1903) |
| `resolucao` | `DisputaSupervisao`: [models.py:1910](app/chamados/models.py#L1910) |
| `motivo_fechamento` | `DisputaSupervisao`: [models.py:1930](app/chamados/models.py#L1930) |
| `reaberturas_admin` | `DisputaSupervisao`: [models.py:1899](app/chamados/models.py#L1899) |
| `reaberturas_colab` | `DisputaSupervisao`: [models.py:1895](app/chamados/models.py#L1895) |
| `motivos` | `DisputaSupervisao`: [models.py:1878](app/chamados/models.py#L1878) |
| `materializada_em` | `PerguntaDisputa`: [models.py:3647](app/chamados/models.py#L3647); também `RegularizacaoExterna`: [models.py:4377](app/chamados/models.py#L4377) |
| `materializada_por` | `PerguntaDisputa`: [models.py:3651](app/chamados/models.py#L3651); também `RegularizacaoExterna`: [models.py:4378](app/chamados/models.py#L4378) |
| `via_resolucao` | `PerguntaDisputa`: [models.py:2748](app/chamados/models.py#L2748) |
| `respondida_em` | `PerguntaDisputa`: [models.py:2635](app/chamados/models.py#L2635) |
| `resposta_colab` | `PerguntaDisputa`: [models.py:2634](app/chamados/models.py#L2634) |
| `validada_em` | `PerguntaDisputa`: [models.py:2636](app/chamados/models.py#L2636) |
| `validada_por` | `PerguntaDisputa`: [models.py:3631](app/chamados/models.py#L3631) |
| `recusa_motivo` | `PerguntaDisputa`: [models.py:2747](app/chamados/models.py#L2747) |
| `pendencia_admin` | `PerguntaDisputa`: [models.py:2743](app/chamados/models.py#L2743) |
| `data_conferida_em` | `PerguntaDisputa`: [models.py:2645](app/chamados/models.py#L2645) |
| `data_conferida_por` | `PerguntaDisputa`: [models.py:2646](app/chamados/models.py#L2646) |
| `veredito_lavrado` | `PerguntaDisputa`: [models.py:2772](app/chamados/models.py#L2772) |
| `julgado_em` | `PerguntaDisputa`: [models.py:2777](app/chamados/models.py#L2777) |
| `janela_expira_em` | `PerguntaDisputa`: [models.py:2783](app/chamados/models.py#L2783) |

### 2. Ocorrências identificadas — parcial

**Produção, escritores:**

- `materializada_em`, `materializada_por` — escrita pelo atributo Django `materializada_por_id` — e `via_resolucao`: atribuições em [`_escrever_carimbo`, models.py:3295](app/chamados/models.py#L3295); há também `update()` dos três em [models.py:3485](app/chamados/models.py#L3485).
- `respondida_em`, `validada_em`, `validada_por`: atribuições em [models.py:3328](app/chamados/models.py#L3328) e [models.py:3331](app/chamados/models.py#L3331).
- `recusa_motivo`: atribuição e inclusão em `save(update_fields=...)` em [models.py:3384](app/chamados/models.py#L3384).
- `data_conferida_em`, `data_conferida_por`: atribuições e `save(update_fields=...)` em [validacao.py:996](app/chamados/services/validacao.py#L996); o carregamento de `PerguntaDisputa` aparece em [validacao.py:983](app/chamados/services/validacao.py#L983).
- `veredito_lavrado`, `julgado_em`, `janela_expira_em`: atribuições e `save(update_fields=...)` em [veredito_lavrado.py:270](app/chamados/veredito_lavrado.py#L270).
- **AMBÍGUO:** `pendencia_admin` e `recusa_motivo` são atribuídos ao receptor `p` em [materializacao.py:1365](app/chamados/services/materializacao.py#L1365); não confirmei a origem do receptor em todas as chamadas. [não verificado]
- **AMBÍGUO:** `resposta_colab` e `respondida_em` são atribuídos a `p` em [disputa_emissao.py:1481](app/chamados/services/disputa_emissao.py#L1481); a origem exata do receptor não foi confirmada. [não verificado]

**Produção, leitores:**

- `respondida_em`, `validada_em`, `materializada_em`: `getattr(p, campo, None)` percorre uma tupla desses nomes em [historico.py:96](app/chamados/services/historico.py#L96); `p` vem de queryset de `PerguntaDisputa` em [historico.py:96](app/chamados/services/historico.py#L96).
- `resposta_colab`: leitura de `p.resposta_colab` no mesmo fluxo em [historico.py:109](app/chamados/services/historico.py#L109).
- O template lê `data_conferida_em`, `data_conferida_por`, `resposta_colab`, `respondida_em`, `pendencia_admin`, `recusa_motivo` e `validada_em` em [modal_fio.html:170](app/templates/chamados/partials/modal_fio.html#L170), [modal_fio.html:219](app/templates/chamados/partials/modal_fio.html#L219) e [modal_fio.html:222](app/templates/chamados/partials/modal_fio.html#L222); o laço das perguntas aparece em [modal_fio.html:157](app/templates/chamados/partials/modal_fio.html#L157).
- O `Q` mencionado no anexo usa `fechada_em` para `DisputaSupervisao` e `status_local` para `ChamadoColaborador`, fora deste censo, conforme construção e aplicação em [models.py:2858](app/chamados/models.py#L2858) e [models.py:2867](app/chamados/models.py#L2867).

**Testes, exemplos:** [nao verificado]

- Escrita e `save` de `resposta_colab` e `respondida_em`: [test_b2d_resposta_visivel.py:63](app/chamados/tests/test_b2d_resposta_visivel.py#L63).
- Leitura de `materializada_em` e `via_resolucao`: [test_b2d_resposta_visivel.py:135](app/chamados/tests/test_b2d_resposta_visivel.py#L135).
- Filtro por `respondida_em` e `validada_em`: [test_reresposta_invalida_validacao.py:64](app/chamados/tests/test_reresposta_invalida_validacao.py#L64).

**Migrations, exemplos:**

- A criação do modelo declara `resposta_colab`, `respondida_em`, `validada_em` e `validada_por` em [0014_perguntadisputa.py:24](app/chamados/migrations/0014_perguntadisputa.py#L24).
- `materializada_em` e `materializada_por` são adicionados em [0016_s84_pergunta_chamado_destino.py:21](app/chamados/migrations/0016_s84_pergunta_chamado_destino.py#L21).
- A migration usa `via_resolucao` em `Q` aplicado ao modelo histórico de pergunta em [0053_pergunta_unica_por_familia.py:32](app/chamados/migrations/0053_pergunta_unica_por_familia.py#L32).

### 3. Script usado para as declarações

Executei o script abaixo passando `.` como raiz. Ele conta **declarações diretas de classe**, não escritores nem leitores; portanto, sua linha `FIM_CENSO_pergunta` sinaliza apenas o fim dessa saída parcial e **não comprova a conclusão do censo solicitado**. A contagem para `PerguntaDisputa` foi 1 para cada uma das 14 colunas identificadas e 0 para as outras 11, conforme as declarações da tabela. [models.py:2593](app/chamados/models.py#L2593)

```python
import ast, collections, os, sys

p = os.path.join(sys.argv[1], 'app/chamados/models.py')
t = ast.parse(open(p, encoding='utf-8').read())
names = (
    'status_local resolvido_em via_encerramento respondido_por '
    'fechada_em fechada_por resolucao motivo_fechamento '
    'reaberturas_admin reaberturas_colab motivos materializada_em '
    'materializada_por via_resolucao respondida_em resposta_colab '
    'validada_em validada_por recusa_motivo pendencia_admin '
    'data_conferida_em data_conferida_por veredito_lavrado '
    'julgado_em janela_expira_em'
).split()
counts = collections.Counter()

for c in t.body:
    if isinstance(c, ast.ClassDef):
        for a in c.body:
            if isinstance(a, (ast.Assign, ast.AnnAssign)):
                targets = a.targets if isinstance(a, ast.Assign) else [a.target]
                for n in targets:
                    if isinstance(n, ast.Name) and n.id in names:
                        print(f'{n.id}\t{c.name}\t{p}:{n.lineno}')
                        if c.name == 'PerguntaDisputa':
                            counts[n.id] += 1

for n in names:
    print(f'TOTAL_DECLARACOES\t{n}\t{counts[n]}')
print('FIM_CENSO_pergunta')
```

### 4. Alcance não coberto

O script apresentado não percorre o restante da cópia e não resolve receptores, fluxo de querysets ou `Q`, escritas por `update`, `defaults`, `bulk_update`, `F()` ou SQL, nomes dinâmicos, templates, testes ou migrations. Esses itens continuam sem censo completo. [não verificado] [nao verificado]

A AST Python, isoladamente, também não determina todos os valores e tipos em execução, nem interpreta por si só o contexto dos templates; os resultados dependentes desses fluxos exigem análise adicional e devem permanecer **AMBÍGUOS** enquanto não resolvidos. [não verificado]

## 19/11 00:47 · sessao 866cc71fa871 · - · - · origem=Ronald (tmux)

Levantamento de leitores e receptores ambíguos enviado ao agente principal, incluindo templates, `getattr` dinâmico, o fluxo de `Q` e usos de chaves estrangeiras com sufixo `_id` ([historico.py:96](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:96), [models.py:2858](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2858), [models.py:3322](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3322)).

## 19/11 00:47 · sessao 9407f254bfe2 · QUEIMA-B1 · sonda · origem=Code

**Não é possível determinar a causa de cada divergência nem produzir a matriz pedida com as funções existentes nesta cópia.** A busca pelo nome `razao_da_sobra` na árvore não encontrou definição nem chamada; portanto, não há classes desse juiz que eu possa listar ou chamar. A existência desse juiz sob outro nome está **[não verificado]**.

Também não há, nos dados fornecidos, o resultado por causa dos 455 dias. As categorias `JORNADA_ADICIONAL` e `PERIODO_FORA_ESCALA_CADASTRO` foram atribuídas pela `sonda2.py` a partir de `dias_cadastro_x_realidade`; elas não demonstram, por si, o mecanismo que levou cada batida a ser casada pelo pareador e ficar sem marco na ata. A sonda chama `classificar_orfas`, mas descarta sua resposta e passa apenas `{"orfas": orfas}`; com essa entrada, a função assume `tipo_dia` como folga. [anexo `sonda2.py` fornecido; não verificado] [app/escala/orfa_classe.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:53)

Os caminhos que a leitura permite estabelecer são:

| Pareador → ata | Caminho e limite da conclusão |
|---|---|
| **Par fechado → órfã** | O pareador constrói turnos a partir das batidas e pode usar papéis derivados da própria ata; a grade, por sua vez, põe em `orfas` batidas que não sentou em marcos e conserva sobras por data. Isso explica como as respostas podem divergir, mas não identifica qual decisão concreta ocorreu em cada um dos 455 dias. [app/ponto/turnos.py:1192](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1192) [app/escala/utils.py:631](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:631) [app/escala/utils.py:802](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/utils.py:802) |
| **Entrada de turno aberto ou saída sem entrada → luz casada** | A ata pode reconhecer uma luz mesmo quando o turno resultante não possui a outra borda. O pareador pode deixar de receber papéis de uma ata desalinhada, sem luz utilizável ou que falhe nas suas guardas; quando recebe papel, ainda aplica suas regras de intervalo e de abertura e fechamento. A causa específica de cada ocorrência inversa não foi medida no anexo. [app/ponto/juiz_batida.py:195](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/juiz_batida.py:195) [app/ponto/turnos.py:1631](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1631) [app/ponto/turnos.py:1371](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1371) |
| **Interna marcada como pausa → órfã** | `_intra_dur` pode vir de S→E por duração, do envelope S,S, de papel `X`/`Xi` da ata, de proximidade de marco ou de volta deslocada. O mesmo atributo não conserva uma origem exclusiva; `_intra_ata` distingue alguns casos alimentados pela ata, mas não separa todas as origens nem prova que apenas uma regra atuou. Para separar os caminhos com segurança seria necessário o pareador registrar as decisões e suas origens. [app/ponto/turnos.py:453](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:453) [app/ponto/turnos.py:476](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:476) [app/ponto/turnos.py:1310](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1310) [app/ponto/turnos.py:1356](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1356) [app/ponto/turnos.py:1408](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/turnos.py:1408) |

Existe outro classificador, `escala.orfa_classe.classificar_orfas`, com as classes `folga_desmentida`, `geometria`, `eco_cluster` e `indefinida`. Ele classifica a **procedência de órfãs da grade**; não é uma matriz de transições entre classes do pareador e da ata. [app/escala/orfa_classe.py:1](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:1) [app/escala/orfa_classe.py:21](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/escala/orfa_classe.py:21)

A volta 3 fornecida relata **453 sobras com origem de pausa não separada** e, na direção inversa, **333 dias e 336 batidas**, com cobertura parcial: 6.454 ocorrências de ata desalinhada, 2.385 de luz sem instante, 1.855 de ata ausente, 1.016 de luz sem batida, 18 de minuto ambíguo e 7 de luz também em órfãs. São ocorrências dos motivos de exclusão, não necessariamente dias distintos. Esses números são transcrição do anexo, sem acesso independente à sombra nesta sessão. [anexos `pronto.md` e `sonda3.r` fornecidos; não verificado]

Não devolvo um script apresentado como solução: chamar um juiz inexistente falharia, e inventar categorias ou reconstruir suas regras contrariaria o requisito de chamar as funções reais. Ficam, portanto, **pendentes** a classificação causal por empresa, os dois exemplos por causa e a matriz solicitada. [não verificado]

**perguntas de lei: N [não verificado]** — o anexo anterior informa zero, mas não estabeleci por leitura suficiente de `LEIS.md` e `CORTES.md` que todas as decisões necessárias para uma classificação causal estejam respondidas. [anexo `pronto.md` fornecido; não verificado]

## 19/11 00:47 · sessao 6aef94714198 · - · - · origem=Ronald (tmux)

A varredura encontrou candidatos em 286 arquivos de teste, incluindo outros modelos e receptores que exigem classificação; ela não constitui um censo completo [não verificado]. [nao verificado]

Exemplos de testes de `PerguntaDisputa`: [nao verificado]

- Escrita de `resposta_colab` e `respondida_em`, seguida de `save(update_fields=...)`: [test_b2d_resposta_visivel.py:63](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/tests/test_b2d_resposta_visivel.py:63).
- Leitura de `materializada_em` e `via_resolucao`: [test_b2d_resposta_visivel.py:135](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/tests/test_b2d_resposta_visivel.py:135).
- `update()` de `recusa_motivo`: [test_e1_volta2.py:105](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/tests/test_e1_volta2.py:105).
- `update()` de `materializada_em`, `materializada_por` e `via_resolucao`: [test_f2_p2_recusa_sob_a_cerca.py:127](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/tests/test_f2_p2_recusa_sob_a_cerca.py:127).

Em migration, o `RunPython` de 0053 lê `via_resolucao` por um `Q` aplicado ao queryset do modelo histórico `PerguntaDisputa`: [0053_pergunta_unica_por_familia.py:32](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0053_pergunta_unica_por_familia.py:32), [linha 37](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0053_pergunta_unica_por_familia.py:37) e [linha 56](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0053_pergunta_unica_por_familia.py:56).

## 19/11 00:47 · sessao ccff956dae09 · - · - · origem=Ronald (tmux)

Identifiquei os campos por AST e enviei ao agente principal as linhas das declarações e exemplos de escritores de produção. A busca de escritores comunicada é parcial: não cobre integralmente escritas dinâmicas, dicionários, `defaults` ou `update_fields`. [não verificado]

## 19/11 00:47 · sessao 7e1cda6cadea · QUEIMA-B3 · censo · origem=Code

**O censo não pode ser fechado com o material disponível.** O anexo fornecido lista os códigos dos sítios, mas a tabela que os traduz em arquivo, linha, função, premissa e casos não está na cópia acessível. O próprio código a referencia e descreve a B2b como aberta em [fio.py:1397](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1397). Assim, os sítios abaixo são **evidências parciais**, sem atribuição inventada aos códigos da tabela. O hash e os merges informados também não puderam ser conferidos: metadados Git [não verificado].

O anexo recebido determina F0 → F1 → F2 → F3 → F4 → F5 → F6 → F7, cada fatia depois da anterior no ar, e a abertura da B2b depois das obras F6F7, F9, F5 e F3. O estado real desses pousos e a revisão exigida [não verificado]. Os nomes **F3 e F5 da B2b** abaixo são distintos das **obras F3 e F5 da espinha**.

A redecisão de F7 fornecida prevalece sobre a linha anterior do próprio anexo: recusar fica livre; aceitar folga e conferir data ganham as regras próprias descritas ali. O texto de aval externo citado pelo anexo e seu registro na cópia [não verificado]. Nenhum teste foi executado; os testes citados são sítios existentes, não resultados. [nao verificado]

### F0 — instrumentos desligados

- **Premissas: [não verificado]** para a identidade e existência da futura função única dos dias, coletor de retidas, retorno `RETIDA` e trilha por pergunta. A cerca atual tem contexto com `default=None` e `vigiar_escrita_de_perguntas` retorna sem agir fora de contexto: **CONFERE para a cerca atual**, mas isso não demonstra a neutralidade de um futuro coletor. [fio.py:1381](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1381), [fio.py:1389](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1389), [fio.py:1402](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1402).
- **Censo:** os campos e contratos concretos dos instrumentos ainda não identificados, e seus leitores/escritores de produção e teste, [não verificado]. [nao verificado]
- **Dependências:** o anexo a coloca depois das obras prévias e antes de F1; estado dos pousos [não verificado].
- **RED candidato:** comparar o comportamento observável com coletor desligado ao comportamento anterior, incluindo retorno e escrita; o resultado e os casos exatos [não verificado].
- **Lei:** perguntas específicas dependentes dos contratos dos instrumentos [não verificado].

### F1 — carimbo no fecho

- **Premissas X1, A9c, A9d, B1a, B1b, B2a, B3a, B3b, X13 e X14: [não verificado]**. Sem a tabela, não há como conferir suas linhas antigas, trechos ou mudanças. A decisão escrita para A9d diz que a disputa fecha e a pergunta entregue que S144 carimbaria não é gravada e conta; não se identifica por isso o sítio completo da tabela. [CORTES.md:556](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:556).
- **Censo parcial, produção:** `PerguntaDisputa.carimbar` atribui materialização e via, podendo também validar e lavrar; o salvamento está em [models.py:3293](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3293) e [models.py:3348](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:3348). Há chamada no fecho por admin em [models.py:2080](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/models.py:2080) e no emissor em [disputa_emissao.py:2103](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:2103). Outros chamadores explícitos incluem [reconciliador.py:407](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/reconciliador.py:407), [materializacao.py:1981](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1981) e [validacao.py:520](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:520). Leitores incluem [raiox.py:135](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/raiox.py:135) e [disputa_emissao.py:228](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/disputa_emissao.py:228). A lista completa por campo, inclusive descarimbos e testes, [não verificado].
- **Dependências:** F0 e as obras anteriores do anexo; pouso [não verificado]. O campo `materializada_em` alimenta `marcos_do_lote`, relevante ao selo da obra F3. [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201).
- **RED candidatos:** RED-NOVO-2 e RED-NOVO-3 são nomes dados pelo anexo; seus casos literais [não verificado]. Para A9d, testar disputa fechada, pergunta entregue preservada e contagem conforme o corte citado. [CORTES.md:556](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:556).
- **Lei:** lacunas específicas dos demais sítios [não verificado].

### F2 — relavra

- **Premissas A4, A5, A6, A7, B5, B6 e B7: [não verificado]** sem a tabela.
- **Censo parcial, produção:** `relavrar` calcula nova via, altera `via_resolucao`, chama `lavrar`, salva a via e os campos de lavra e registra auditoria por pergunta. [relavra_via.py:106](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/relavra_via.py:106). `lavrar` atribui `veredito_lavrado`, `julgado_em` e `janela_expira_em`; `esta_stale` lê a lavra em comparação com os marcos. [veredito_lavrado.py:213](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:213), [veredito_lavrado.py:254](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:254). Há comando chamador em [relavrar_via_i6.py:102](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/management/commands/relavrar_via_i6.py:102). **Teste existente:** relavra no mesmo save e idempotência da trilha em [test_porta_veredito_pergunta.py:343](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_porta_veredito_pergunta.py:343) e [test_porta_veredito_pergunta.py:372](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_porta_veredito_pergunta.py:372). Exaustividade por campo [não verificado].
- **Dependências:** F1 antes; interação exata com a obra F3 depende do mapeamento dos sítios [não verificado].
- **RED candidato:** lote com pergunta entregue e aberta, verificando preservação da primeira e relavra coerente da segunda. Se corresponde ao RED-NOVO-1 [não verificado].
- **Lei:** lacunas por sítio [não verificado].

### F3 — reaponte da B2b

- **Premissas B4, B2b, B2d, X6, X7, X8, X9, X10 e X12: [não verificado]**; arquivo, linha, função e trechos da tabela ausente não podem ser confrontados.
- **Censo dos campos reapon­tados, produção e teste: [não verificado]** porque o anexo não identifica os campos de cada código. [nao verificado]
- **Dependências:** F2 antes; a dependência declarada com a **obra F3** é anterior à abertura da B2b. O código de `marcos_do_lote` lê, entre outros, `materializada_em` e `respondida_em` da pergunta; sua função não consulta `DecisaoPergunta` no trecho da implementação. [veredito_lavrado.py:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:151), [veredito_lavrado.py:201](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/veredito_lavrado.py:201). O backlog descreve o selo da obra F3 com mutante e sua posição depois de F9 e F5; sua execução ou pouso [não verificado]. [BACKLOG.md:2018](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/BACKLOG.md:2018).
- **RED:** casos da tabela [não verificado].
- **Lei:** perguntas específicas dos re­apontes [não verificado].

### F4 — laço de materialização

- **Premissas P11–P15: [não verificado]**. `materializar_perguntas_validadas_da_disputa` é um sítio pertinente ao tema, mas sua correspondência aos cinco códigos [não verificado]. [materializacao.py:2069](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2069).
- **Censo parcial, produção:** o laço lê resposta, validação e materialização, trava e relê perguntas; a unidade consulta o bloqueio antes de gravar e pode carimbar. [materializacao.py:2100](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:2100), [materializacao.py:1866](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1866), [materializacao.py:1981](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:1981). Chamadores incluem [fio.py:711](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:711) e [acoes_disputa.py:124](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/acoes_disputa.py:124). **Testes relacionados:** [test_pep_reentrada.py:103](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_pep_reentrada.py:103) e [test_o317_passivo_pela_porta.py:196](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_o317_passivo_pela_porta.py:196). Censo completo [não verificado].
- **Dependências:** F3 da B2b antes; o anexo exige pouso isolado. A L-130 define resposta pelo par instante e texto e nomeia materialização entre suas finalidades, motivo para observar os leitores da obra F5; estado de pouso [não verificado]. [LEIS.md:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:151).
- **RED candidato:** duas perguntas no laço, uma retida e outra aplicável, verificando cada resultado e o fecho da disputa. Casos literais da tabela [não verificado].
- **Lei:** perguntas específicas dos cinco sítios [não verificado].

### F5 — comandos e avulsos da B2b

- **Premissas A8, P1, B8, P3, P4, P5, P17, P18, P19 e P20: [não verificado]**; inclusive identidade dos comandos e campos por código.
- **Censo de escritores e leitores, produção e teste: [não verificado]** sem o mapa da tabela. O anexo admite até dois pousos para esta fatia; a divisão concreta [não verificado]. [nao verificado]
- **Dependências:** F4 antes. A **obra F5 dos leitores da pergunta** é anterior à abertura da B2b segundo o anexo; seu pouso [não verificado]. A lei escrita considera resposta com instante sem texto pendente, e o corte F5 explicita a porta e o painel do colaborador. [LEIS.md:151](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/LEIS.md:151), [CORTES.md:560](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/docs/CORTES.md:560).
- **RED:** casos da tabela [não verificado].
- **Lei:** perguntas específicas dos comandos e avulsos [não verificado].

### F6 — callback depois da cerca

- **Premissa X15: [não verificado]**. O anexo não fornece função, arquivo nem linha para identificar qual callback é X15.
- **Censo de estado e de leitores/escritores, produção e teste: [não verificado]** pela mesma razão. [nao verificado]
- **Dependências:** F5 da B2b antes; situação da cerca e ordem concreta do callback X15 [não verificado]. A cerca existente abre um escopo de execução próprio em [fio.py:1431](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:1431).
- **RED:** casos da tabela [não verificado].
- **Lei:** perguntas específicas de X15 [não verificado].

### F7 — portas humanas, redecisão fornecida

- **CONFERE:** `entrega_que_barra_a_validacao` existe em [validacao.py:209](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:209) e delega à composição de bloqueio. **NÃO CONFERE como regra universal para todos os motivos:** a composição atual consulta somente metades de intervalo. [materializacao.py:804](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:804), [materializacao.py:811](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:811).
- **CONFERE:** `_aceitar_a_folga` é chamado antes da guarda geral de entrega de `validar_pergunta`. [validacao.py:707](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:707), [validacao.py:740](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:740). A porta já recusa ausência de data, mas a aplicação da nova regra a todos os motivos **não confere no estado atual**. [validacao.py:499](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:499).
- **NÃO CONFERE literalmente com o código atual:** “conferir data sem dia passa e é a porta que dá o dia”. A porta atual não recebe nem grava `data_evento`; a detecção de divergência exige dia existente, e o modal não oferece entrada de data. [validacao.py:957](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:957), [validacao.py:983](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:983), [fio.py:87](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:87), [modal_fio.html:162](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templates/chamados/modal_fio.html:162).
- **Censo parcial, produção:** `competencia_do_fato` lê `data_evento`, calcula a janela e consulta `competencia_entregue`, retornando `None` sem data. [materializacao.py:788](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:788), [export.py:556](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/folha/export.py:556). `recusar_pergunta` grava motivo e reabre a pergunta; a existência de contador específico para recusa em competência entregue [não verificado]. [validacao.py:886](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:886), [validacao.py:916](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:916). A conferência grava `data_conferida_em/por`; leitores incluem materialização e modal. [validacao.py:996](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:996), [materializacao.py:166](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:166), [modal_fio.html:170](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templates/chamados/modal_fio.html:170). **Testes relacionados:** [test_porta_validacao_pergunta.py:309](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_porta_validacao_pergunta.py:309), [test_data_do_fato_divergente.py:191](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/tests/test_data_do_fato_divergente.py:191). Censo completo [não verificado].
- **Tela:** card e modal têm testemunhas para o bloqueio atual de validar; a existência de um mesmo bloqueio próprio para as duas novas portas **não confere no estado atual**. [chamados_tags.py:132](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templatetags/chamados_tags.py:132), [fio.py:375](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/fio.py:375), [modal_fio.html:252](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/templates/chamados/modal_fio.html:252).
- **Dependências:** F6 da B2b antes; o anexo registra espera de revisão e leitura para a F7 antiga, mas seu cumprimento ou efeito sobre a redecisão [não verificado]. A decisão fornecida distingue a composição existente de juiz novo, coerente com a descrição da função atual. [materializacao.py:788](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/materializacao.py:788).
- **RED candidatos:** recusar livre e contador incrementado uma vez; aceitar folga barrada; conferir sem dia e conferir data em competência entregue; mutantes sem regra e com bloqueios diferentes no card e modal. A montagem do caso “sem dia” depende da resposta abaixo. [validacao.py:957](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:957).
- **Pergunta de lei 1:** quando não há dia do fato, qual data a ação de conferir está autorizada a estabelecer e submeter ao juiz antes de gravar, considerando que a porta atual não recebe nem escreve essa data? [validacao.py:957](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:957), [validacao.py:983](/home/ronald/codex_ws/5bad81419976b0257182e8fd9598005dae16b4ad/app/chamados/services/validacao.py:983).

**perguntas de lei: N = 1 identificada nesta leitura; total da obra [não verificado]**, pois faltam as premissas e os casos da tabela única.

## 19/11 00:46 · sessao b1c65fbfae1c · QUEIMA-B2-chamado · censo · origem=Code

**Censo parcial:** das 25 colunas, quatro são declarações diretas de `ChamadoColaborador`. Não concluí a classificação exaustiva de escritores e leitores, nem produzi o script de censo completo solicitado; os totais por coluna e `FIM_CENSO_chamado` permanecem [não verificado].

### Campos declarados

Os quatro campos de `ChamadoColaborador` são `status_local`, `via_encerramento`, `respondido_por` e `resolvido_em`. [app/chamados/models.py:58–66](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:58)

Os outros 21 nomes aparecem como declarações diretas nas seguintes entidades:

| Entidade | Colunas e linhas em `app/chamados/models.py` |
|---|---|
| `DisputaSupervisao` | `motivos` :1878; `fechada_em` :1894; `reaberturas_colab` :1895; `reaberturas_admin` :1899; `fechada_por` :1903; `resolucao` :1910; `motivo_fechamento` :1930. [models.py:1878](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1878) |
| `PerguntaDisputa` | `resposta_colab` :2634; `respondida_em` :2635; `validada_em` :2636; `data_conferida_em` :2645; `data_conferida_por` :2646; `pendencia_admin` :2743; `recusa_motivo` :2747; `via_resolucao` :2748; `veredito_lavrado` :2772; `julgado_em` :2777; `janela_expira_em` :2783; `validada_por` :3631; `materializada_em` :3647; `materializada_por` :3651. [models.py:2634](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2634) [models.py:2743](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2743) [models.py:2772](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2772) [models.py:3631](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3631) |
| `RegularizacaoExterna` | Também declara `materializada_em` :4377 e `materializada_por` :4378. [models.py:4377](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:4377) |

### Usos identificados — não exaustivos

**Produção: escritores.**

- `status_local`: atribuições em [models.py:234](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:234), [models.py:368](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:368) e [models.py:383](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:383); criação em [models.py:1154](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1154). As duas atribuições de :368 e :383 alteram e restauram o valor em memória.
- `respondido_por`: atribuição em [models.py:237](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:237).
- `resolvido_em`: atribuições em [models.py:250](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:250) e [models.py:254](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:254).
- `via_encerramento`: atribuições em [models.py:251](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:251) e [models.py:255](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:255).

A lista de campos da transição é montada em [models.py:235](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:235), recebe `respondido_por` condicionalmente em [models.py:238](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:238) e é passada ao `save(update_fields=...)` em [models.py:302](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:302). O `create` também recebe `**campos`; seu conteúdo adicional é **AMBÍGUO**, pois não resolvi o fluxo dos argumentos. [models.py:1154](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1154)

**Produção: exemplos de leitores.**

- `status_local`: [historico.py:156](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:156) e `getattr` em [juizes.py:862](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/juizes.py:862).
- `resolvido_em`: template em [detalhe.html:56](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/templates/chamados/detalhe.html:56).
- `via_encerramento`: `getattr` em [disputa_emissao.py:1906](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1906) e [disputa_emissao.py:1944](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1944).
- `respondido_por`: template em [detalhe.html:57](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/templates/chamados/detalhe.html:57).

**Testes: exemplos de leitores.** `status_local` é lido em [test_criar_gate.py:23](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/tests/test_criar_gate.py:23); `resolvido_em` e `via_encerramento`, em [test_contract_status_nucleo.py:54](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/tests/test_contract_status_nucleo.py:54). Escritores de teste e leitores de teste de `respondido_por`: [não verificado].

**Migrations.** Há escritas de `status_local` em [0038_a3_registrado_backfill.py:10](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0038_a3_registrado_backfill.py:10), [linha 15](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0038_a3_registrado_backfill.py:15), [0039_a3_via_backfill.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:24) e [linha 33](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:33). A segunda migration também escreve `via_encerramento` nas linhas 24, 25, 27, 28 e 34 e filtra `respondido_por` na linha 27. [0039_a3_via_backfill.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:24)

O `getattr` dinâmico citado no anexo percorre `PerguntaDisputa`, obtida na consulta anterior; aqueles usos de `respondida_em`, `validada_em` e `materializada_em` ficam fora deste censo. [historico.py:96](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:96) [historico.py:98](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:98) [historico.py:105](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:105)

### Script executado

Este é o script AST usado para conferir as **declarações diretas** acima, executado pela entrada padrão com a raiz `.`. Salvo como `script.py`, aceita `python3 script.py <raiz>`. Ele não é o script de censo completo: não percorre usos, templates, SQL nem resolve receptores; por isso não emite totais de leitores/escritores nem o marcador solicitado. [não verificado]

```python
import ast
import sys
from pathlib import Path

colunas = '''status_local resolvido_em via_encerramento respondido_por
fechada_em fechada_por resolucao motivo_fechamento reaberturas_admin
reaberturas_colab motivos materializada_em materializada_por via_resolucao
respondida_em resposta_colab validada_em validada_por recusa_motivo
pendencia_admin data_conferida_em data_conferida_por veredito_lavrado
julgado_em janela_expira_em'''.split()
caminho = Path(sys.argv[1]) / 'app/chamados/models.py'
arvore = ast.parse(caminho.read_text(encoding='utf-8'), filename=str(caminho))
achados = {c: [] for c in colunas}
for classe in arvore.body:
    if not isinstance(classe, ast.ClassDef):
        continue
    for no in classe.body:
        if isinstance(no, ast.Assign) and isinstance(no.value, ast.Call):
            for alvo in no.targets:
                if isinstance(alvo, ast.Name) and alvo.id in achados:
                    achados[alvo.id].append((classe.name, no.lineno))
for coluna, locais in achados.items():
    print(coluna, *('%s@%s:%d' % (classe, caminho, linha)
                    for classe, linha in locais), sep='\t')
```

Ficaram fora da verificação completa o fluxo de `Q` e querysets, nomes e argumentos construídos dinamicamente, `bulk_update`, expressões `F()`, SQL cru ou montado dinamicamente, templates e a classificação de todos os receptores homônimos. A execução efetiva dos caminhos e o estado de produção também permanecem [não verificado].

## 19/11 00:46 · sessao 26ab5d5ff90d · QUEIMA-B5 · leitura · origem=Code

**As duas decisões de lei continuam abertas:** quem é responsável pelo órfão que não gera disputa e qual deve ser o destino de um motivo novo quando a dona está fechada. O backlog registra a escolha “não nasce” para a primeira e as alternativas reabrir, nova disputa ou Pauta para a segunda; o corte posterior registra a segunda como N5, sem decidi-la. ([BACKLOG.md:2115](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/BACKLOG.md:2115), [CORTES.md:515](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:515).)

A identidade desta cópia com o hash informado e os números atuais de produção são **[nao verificado]**. O dado raspado não traz os registros necessários para contar os dois universos. **[nao verificado]**

**1. Órfão que não gera disputa**

Uma `PerguntaDisputa` sem disputa não é um estado previsto pelo modelo: a chave `disputa` é obrigatória. “Pergunta órfã” também é usado no código para pergunta sem **chamado de destino**, que é uma situação diferente. Para medir o caso sem disputa, a unidade observável mais próxima é o **chamado de furo sem pergunta que o tenha como destino**; isso não prova, por si só, que não exista outro vínculo indireto ou outra representação da batida. ([models.py:2603](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2603), [reconciliador.py:1578](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/reconciliador.py:1578), [models.py:1303](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1303).)

Hoje o agrupador pode não criar disputa: exclui dias na lista do admin e, depois dos filtros, retorna se nada restar; a fábrica também exclui causas como chamado encerrado, dia sem adesão e turno resolvido na virada. Nos caminhos conferidos, **não aparece um fechador universal para todo chamado que ficou sem disputa**; afirmar que nenhum outro caminho o fecha seria **[nao verificado]**. ([disputa_emissao.py:761](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:761), [disputa_emissao.py:1059](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1059), [disputa_emissao.py:1100](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1100).)

O estado do chamado é `status_local`; a porta de estado e o `reconciliar()` são pontos a considerar para fechá-lo. O signal de chamado encerrado tenta resolver perguntas ligadas ao destino, mas uma pergunta inexistente não lhe fornece dona para fechar. Esse signal é best-effort; o resolvedor seleciona perguntas de disputas abertas e respeita C6 e respostas ainda sem testemunha. ([models.py:58](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:58), [models.py:335](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:335), [signals.py:93](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/signals.py:93), [disputa_emissao.py:1951](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1951), [disputa_emissao.py:2023](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:2023).)

Desenhos possíveis, ainda sem escolher o responsável:

- **Fechar pelo evento que resolve o fato:** o escritor seria a porta canônica do chamado, acionada pelo evento após consultar o juiz do fato; leitores do estado do chamado e o signal de encerramento observariam a mudança. Seria preciso conferir e eventualmente migrar todos os leitores que hoje inferem pendência por chamado ou pergunta; censo completo **[nao verificado]**. Toca L-007, L-002 e o corte que faz a vitalidade depender de C6. ([LEIS.md:65](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:65), [LEIS.md:60](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:60), [CORTES.md:415](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:415).)
- **Manter o chamado vivo e entregá-lo a uma fila responsável:** a porta do chamado escreveria eventual atribuição ou estado; a fila do admin ou DP leria esse estado e o juiz do fato. A definição da fila, do responsável e dos leitores a mudar é **[nao verificado]**. Toca L-002 e L-007; usar Pauta especificamente exigiria definir sua criação e leitura. ([LEIS.md:60](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:60), [LEIS.md:65](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:65).)
- **Nascer disputa e pergunta:** é uma capacidade existente do agrupador, mas conflita com a escolha registrada “não nasce” para o recorte h2; adotá-la para esse recorte exigiria novo corte. O escritor seria o agrupador e a porta de nascimento; app, fio e juiz C6 passariam a ler a pergunta. ([BACKLOG.md:2115](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/BACKLOG.md:2115), [disputa_emissao.py:1137](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1137), [pergunta_no_app.py:68](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/pergunta_no_app.py:68).)

**2. Motivo novo com dona fechada**

O comportamento depende da porta:

- Na emissão comum, o núcleo procura apenas disputa **aberta** do colaborador. Se encontra, agrega motivos; se não encontra, cria outra disputa e depois o chamador gera perguntas. Não procura a disputa fechada para reabri-la nesse seletor. ([disputa_emissao.py:540](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:540), [disputa_emissao.py:601](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:601), [disputa_emissao.py:676](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:676).)
- No agrupador, a primeira passagem encontra uma pergunta não materializada pelo destino e usa sua dona **sem filtrar `fechada_em`**. Se há ata e complementos criados, pode acrescentar motivos nessa dona e pedir perguntas. O nascimento pode reutilizar pergunta canônica existente, preservando sua dona, ou inserir outra; portanto, não se pode dizer que toda tentativa necessariamente cria pergunta nova. ([disputa_emissao.py:1079](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:1079), [disputa_emissao.py:837](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/disputa_emissao.py:837), [models.py:3198](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3198), [models.py:3206](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3206).)
- Existe reabertura administrativa explícita: valida motivo e justificativa, usa `reabrir_admin`, acrescenta o motivo e abre pergunta de conversa. A porta administrativa limita a duas reaberturas; há também porta de sistema separada. Isso demonstra capacidades, não uma regra geral automática para toda chegada de motivo novo. ([acoes_disputa.py:240](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/acoes_disputa.py:240), [models.py:2289](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2289), [models.py:2326](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2326).)

O app põe como pendente a pergunta viva sem resposta **com disputa aberta**; as demais vivas vão ao histórico. Já o corte do fio do admin determina apresentar pergunta viva em disputa fechada sem reabertura em lote; esse corte trata a apresentação e resolução de perguntas vivas, não decide N5. ([pergunta_no_app.py:68](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/pergunta_no_app.py:68), [CORTES.md:415](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:415), [CORTES.md:515](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:515).)

| Opção | Escrita e leitores a avaliar | Leis e cortes tocados |
|---|---|---|
| **Reabrir a dona** | A porta de reabertura escreveria fechamento e motivo; emissão/nascimento escreveriam pergunta. Mudariam a seleção da emissão e, conforme o desenho, app, fio e fechamento da disputa. | L-007 e L-002; L-130 se ressuscitar pergunta; o corte de ressuscitação distingue competência entregue e aberta. ([LEIS.md:151](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:151), [CORTES.md:545](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:545).) |
| **Nascer outra** | Núcleo e nascimento escreveriam a nova disputa/pergunta; seleção, deduplicação e leitores de dona precisam tratar o reuso que pode conservar a dona antiga. App e fio leriam o resultado. | L-007, L-002 e os invariantes de pergunta viva e eleição de destino registrados para o desenho. ([models.py:3198](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3198), [CORTES.md:513](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:513).) |
| **Pauta DP** | Uma porta de Pauta escreveria a pendência; a fila DP a leria. Emissão, app e fio precisariam saber que o motivo foi entregue ali, para não desaparecer nem ser cobrado em duplicidade; implementação e leitores exatos **[nao verificado]**. | L-002 e L-007; o corte de ressuscitação para competência entregue determina lista textual para DP naquele recorte, sem decidir N5 genericamente. ([CORTES.md:545](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:545).) |
| **Manter dona fechada e dar alcance ao admin** | Emissão/nascimento registrariam o motivo e a pergunta; fio e mesa do admin leriam C6 independentemente do fechamento. O app e o resolvedor devem ser revistos conforme a decisão, pois hoje distinguem disputa fechada. | Corte do fio em disputa fechada, L-002 e L-130; extensão desse tratamento a motivo novo permanece pergunta de lei. ([CORTES.md:415](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/CORTES.md:415), [LEIS.md:151](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/docs/LEIS.md:151).) |

A sonda abaixo entrega **recortes atuais**, não o número histórico pedido de chegadas. `motivos` é uma lista de estado e a pergunta não fornece, nos campos conferidos, um instante de nascimento que prove que ela ou o motivo chegou *depois* do fechamento; reaberturas também alteram o estado atual. Sem log ou uma trilha histórica suficiente, “quantos chegaram com a dona fechada e o que aconteceu então” é **[nao verificado]**. ([models.py:1875](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1875), [models.py:2593](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2593), [models.py:2289](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2289).)

Execute no processo Django já configurado pelo envelope. A transação exige PostgreSQL e é declarada `READ ONLY`; erros são contados sem imprimir conteúdo de exceção. O recorte de órfãos usa `e_chamado_de_furo()` e ausência de pergunta com aquele chamado como destino; `registrado` é não terminal em `VIVOS`, mas é contado à parte de `aberto` e `em_analise`. ([models.py:1303](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1303), [motor.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/catalogo/motor.py:24).)

```python
# Executar no envelope Django, por exemplo via manage.py shell.
# Somente leitura; nao altera arquivos nem chama portas de escrita.
from collections import Counter
from django.db import connection, transaction
from chamados.models import ChamadoColaborador, PerguntaDisputa
from chamados.catalogo.motor import VIVOS
from chamados.juizes import pergunta_viva, pergunta_respondida

erros = 0
try:
    if connection.vendor != "postgresql":
        raise RuntimeError("A sonda exige PostgreSQL READ ONLY")

    with transaction.atomic():
        with connection.cursor() as cursor:
            cursor.execute(
                "SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY"
            )

        # Chamados de furo sem pergunta que os aponte como destino.
        # A ausencia desse elo nao prova ausencia de outros vinculos.
        por_empresa = Counter()
        totais = Counter()
        chamados = (
            ChamadoColaborador.objects
            .filter(perguntas_que_resolvem__isnull=True)
            .select_related("colaborador")
            .order_by("pk")
        )
        for ch in chamados.iterator(chunk_size=1000):
            try:
                if not ch.e_chamado_de_furo():
                    continue
                estado = (
                    "aberto_ou_em_analise"
                    if ch.status_local in ("aberto", "em_analise")
                    else "registrado"
                    if ch.status_local == "registrado"
                    else "outro_estado"
                )
                emp = ch.colaborador.empresa_id
                totais[estado] += 1
                por_empresa[(emp, estado)] += 1
                print(
                    f"ORFAO_ATUAL ch={ch.pk} col{ch.colaborador_id} "
                    f"emp={emp} estado={estado}"
                )
            except Exception:
                erros += 1
                print(f"ERRO_ORFAO ch={ch.pk}")

        print("ORFAOS_SEM_PERGUNTA_DESTINO", dict(sorted(totais.items())))
        for (emp, estado), n in sorted(
            por_empresa.items(), key=lambda item: (str(item[0][0]), item[0][1])
        ):
            print(f"ORFAOS_EMPRESA emp={emp} estado={estado} n={n}")

        # Estoque atual sob dona fechada: nao e censo de chegadas posteriores
        # ao fechamento e nao estabelece motivo novo nem causalidade.
        estoque = Counter()
        perguntas = (
            PerguntaDisputa.objects
            .filter(disputa__fechada_em__isnull=False)
            .select_related("disputa", "disputa__colaborador", "celula")
            .order_by("pk")
        )
        for p in perguntas.iterator(chunk_size=500):
            try:
                viva = pergunta_viva(p)
                respondida = pergunta_respondida(p)
                materializada = p.materializada_em is not None
                destino_vivo = (
                    None if p.chamado_destino_id is None
                    else ChamadoColaborador.objects.filter(
                        pk=p.chamado_destino_id, status_local__in=VIVOS
                    ).exists()
                )
                chave = (
                    "viva" if viva else "nao_viva",
                    "respondida" if respondida else "nao_respondida",
                    "carimbada" if materializada else "sem_carimbo",
                    "sem_destino" if destino_vivo is None else
                    "destino_vivo" if destino_vivo else "destino_nao_vivo",
                )
                estoque[chave] += 1
                print(
                    f"PERGUNTA_DONA_FECHADA p={p.pk} d={p.disputa_id} "
                    f"col{p.disputa.colaborador_id} motivo={p.motivo} "
                    f"estado={'/'.join(chave)}"
                )
            except Exception:
                erros += 1
                print(f"ERRO_PERGUNTA p={p.pk}")

        for chave, n in sorted(estoque.items()):
            print(f"ESTOQUE_DONA_FECHADA estado={'/'.join(chave)} n={n}")

        print("CHEGADAS_COM_DONA_FECHADA=[nao verificado]")
        print("DESTINO_HISTORICO_DAS_CHEGADAS=[nao verificado]")
except Exception:
    erros += 1
    print("ERRO_SONDA: execucao incompleta")
finally:
    print(f"ERROS={erros}")
    print("FIM_SONDA_B5")
```

A sonda não foi executada contra a frota; sua execução e seus resultados são **[nao verificado]**.

perguntas de lei: 2

## 19/11 00:46 · sessao 8c1810bfebbb · QUEIMA-B6 · censo · origem=Code

O código guarda vários sinais diferentes de trabalho no mesmo dia: decisão administrativa, fato julgado pelo cartório, decisão sobre HE e rubricas apuradas. Eles podem coexistir e não têm o mesmo alcance; a matriz e a sonda preservam essa distinção. [app/escala/models.py:1748] [app/ponto/models.py:315] [app/ponto/models.py:1271]

**Limite do censo:** não verifiquei exaustivamente todos os escritores históricos, todos os consumidores de folha ou todos os estados indiretos. A sonda abaixo mede os estados persistidos identificados, com critérios explícitos; ela não estabelece o universo jurídico completo de trabalho reconhecido. [não verificado]

| Escritor, estado e chamadores | Efeitos: dinheiro, troca e apresentação | Revogação e o que desfaz |
|---|---|---|
| **Resolver dia — admin.** `veredito_celula` aceita `trabalhou` mediante suas verificações e chama `CelulaDia.lavrar_veredito`, gravando `veredito='trabalhou'` e `veredito_via='admin'`; a view chama o serviço por dia. [app/ponto/services/veredito_celula.py:211] [app/ponto/services/veredito_celula.py:228] [app/ponto/services/veredito_celula.py:351] [app/ponto/views.py:3466] | Pode inverter marcações, criar batidas manuais aprovadas, recalcular turnos e fechar o fio do dia; a decisão é lida pelo calendário e pelo app. Seu efeito financeiro depende da apuração posterior. [app/ponto/services/veredito_celula.py:313] [app/ponto/services/veredito_celula.py:330] [app/ponto/services/veredito_celula.py:346] [app/colaboradores/services/calendario.py:443] [app/api/views.py:1412] | Outra decisão administrativa pode sobrescrever o veredito; o cartório não prevalece sobre a decisão admin. O desfazimento das batidas criadas ou invertidas não foi demonstrado. [app/ponto/portas/celula.py:1032] [app/ponto/portas/celula.py:1060] [app/ponto/portas/celula.py:1092] [não verificado] |
| **Cartório — fato em célula.** `processar_cartorio` chama a mesma porta com `via='cartorio'`; `fato_sem_previsao` e `fato_em_ausencia` registram os fatos descritos no modelo. [app/ponto/services/cartorio.py:1854] [app/escala/models.py:1751] | `fato_sem_previsao` participa da classificação visual como folga trabalhada; `fato_em_ausencia` acusa uma contradição que pode reter a folha. O valor do dia reside em `DiaPago`, não no status. [app/ponto/status_dia.py:157] [app/escala/models.py:1754] [app/escala/models.py:1767] [app/ponto/status_dia.py:12] | Novo julgamento pode substituir o veredito do cartório; decisão admin prevalece. Não há histórico de revogação desses valores demonstrado neste censo. [app/ponto/services/cartorio.py:1854] [app/ponto/portas/celula.py:1060] [não verificado] |
| **HE — decisão admin.** `decidir_he` chama `_gravar` para `DecisaoHE.estado='autorizado'` ou `'nao'`; há chamadas pela tela e por portas de lote. A chave é colaborador e data. [app/ponto/portas/he.py:159] [app/ponto/portas/he.py:195] [app/ponto/portas/he.py:206] [app/ponto/views.py:532] [app/ponto/portas/he.py:265] [app/ponto/portas/he.py:595] [app/ponto/models.py:1353] | A autorização permite contar a parcela fora da janela no cálculo; `nao` registra recusa e não significa que o dia inteiro não foi trabalhado. A porta admin agenda recálculo; a gestão lê as decisões vigentes. [app/ponto/motor_calculo_v2.py:1397] [app/ponto/motor_calculo_v2.py:1694] [app/ponto/models.py:1283] [app/ponto/portas/he.py:211] [app/ponto/services/gestao_he.py:87] | Uma nova decisão admin substitui o estado pela mesma porta e pode mudar o resultado ao recalcular. A revogação formal examinada não se aplica à decisão de origem admin. [app/ponto/portas/he.py:195] [app/ponto/portas/he.py:206] [app/ponto/portas/he.py:229] [app/ponto/portas/he.py:478] |
| **HE — recusa automática.** `recusar_ponta_pequena` chama `_gravar` com `estado='nao'` e `origem='sistema'`; há chamador em comando de gestão. É decisão sobre HE, não reconhecimento positivo de trabalho. [app/ponto/portas/he.py:297] [app/ponto/portas/he.py:369] [app/ponto/management/commands/recusar_ponta_pequena.py:170] | Registra a recusa da ponta pequena; a pendência e os leitores de gestão consultam a vigência. [app/ponto/portas/he.py:313] [app/ponto/services/he_pendente_lavrado.py:71] [app/ponto/services/gestao_he.py:87] | `revogar_recusa_do_sistema` chama `_marcar_revogada` quando a premissa deixa de valer; preenche `revogada_em`, motivo e minutos, e `vigentes()` deixa de retornar a linha. A pendência volta, sem autorizar HE por si. [app/ponto/portas/he.py:375] [app/ponto/portas/he.py:421] [app/ponto/portas/he.py:494] [app/ponto/models.py:1265] [app/ponto/portas/he.py:434] |
| **DiaPago — lavratura motor e oráculo.** `lavrar` escreve linhas por dia e uma linha de ajuste, substituindo as linhas da competência e versão; o fechamento chama a lavratura. Uma linha pode conter abono, previsão ou zeros, de modo que sua existência isolada não prova trabalho. [app/ponto/services/dia_pago.py:135] [app/ponto/services/dia_pago.py:345] [app/ponto/services/dia_pago.py:357] [app/ponto/services/dia_pago.py:359] [app/ponto/services/fechamento.py:1191] [app/ponto/services/fechamento.py:1265] [app/ponto/models.py:340] | Guarda horas trabalhadas, extras, noturnas, folga trabalhada e outras rubricas; calendário, espelho e app leem o dinheiro lavrado. O export de folga trabalhada usa o fechamento e a rubrica 200. [app/ponto/models.py:325] [app/colaboradores/services/calendario.py:303] [app/ponto/services/espelho.py:809] [app/api/views.py:1445] [app/folha/export.py:439] | A relavratura apaga e recria as linhas da competência/versão; não encontrei campo de revogação histórica equivalente ao de `DecisaoHE`. O resultado anterior não é recuperável apenas da linha substituída. [app/ponto/services/dia_pago.py:359] [app/ponto/models.py:315] [não verificado] |
| **DiaPago — retrato de competência exportada.** `retratar_exportada` é outra porta de escrita: cria ou atualiza campos de grade e ajuste; é chamada pelo comando e pela porta de regeneração. [app/ponto/services/dia_pago.py:410] [app/ponto/services/dia_pago.py:466] [app/ponto/services/dia_pago.py:538] [app/ponto/management/commands/lavrar_dias_pagos.py:123] [app/ponto/portas/regen_exportada.py:193] | O retrato pode criar linha sem lavratura de rubricas; por isso, essa linha não deve ser contada automaticamente como trabalho. [app/ponto/services/dia_pago.py:472] | A substituição pela lavratura e a identificação histórica do escritor de cada valor após reescritas não foram estabelecidas para todos os casos. [app/ponto/services/dia_pago.py:359] [não verificado] |
| **Folga trabalhada — derivação da apuração.** O fechamento classifica períodos de folga conforme escala e feriado; `lavrar` recebe esses períodos e escreve as rubricas correspondentes em `DiaPago`. [app/ponto/services/fechamento.py:925] [app/ponto/services/fechamento.py:944] [app/ponto/services/dia_pago.py:223] | Com escala certa, pode alimentar `horas_folga_trabalhada`; sem escala certa, o fechamento encaminha para horas trabalhadas. A exportação usa `folga_trabalhada_diurna`, rubrica 200; a tela também deriva um status de folga trabalhada. [app/ponto/services/fechamento.py:965] [app/ponto/services/fechamento.py:967] [app/folha/export.py:439] [app/ponto/status_dia.py:139] | Uma nova apuração pode alterar a classificação e relavrar o dinheiro; não identifiquei uma marca própria de revogação da classificação derivada. [app/ponto/services/dia_pago.py:135] [app/ponto/services/dia_pago.py:359] [não verificado] |

**Leitores da ausência**

| Leitor | Consequência da ausência |
|---|---|
| Motor de ponto | Para dia obrigatório sem data de trabalho nem justificativa, considera falta e horas de falta; há reflexos no cálculo de DSR. [app/ponto/motor_calculo_v2.py:2571] [app/ponto/motor_calculo_v2.py:2595] |
| Status do dia e calendário | Dia encerrado sem certificação suficiente pode aparecer como `sem_certidao`; isso não afirma que a pessoa não trabalhou. [app/ponto/status_dia.py:45] [app/ponto/status_dia.py:291] |
| `DiaPago` e seus consumidores | A soma distingue `sem_lavratura` de `sem_apuracao`; calendário, espelho e export também tratam a falta de lavra. [app/ponto/services/dia_pago.py:752] [app/ponto/services/dia_pago.py:797] [app/colaboradores/services/calendario.py:310] [app/ponto/services/espelho.py:395] [app/folha/porta_export.py:259] |
| HE pendente | A ausência de decisão **vigente** permite que a pendência seja examinada como tal; uma linha revogada não é vigente. [app/ponto/services/he_pendente_lavrado.py:71] [app/ponto/services/he_pendente_lavrado.py:136] [app/ponto/models.py:1265] |
| Cobrança de marcos | O veredito `trabalhou` está entre os que vetam cobrança; sua ausência, isoladamente, não prova que a cobrança será emitida, pois há outras condições. [app/ponto/marcos.py:119] [app/ponto/marcos.py:154] |
| `veredito_do_dia` | O retorno `TRABALHOU` também pode ser o padrão quando nada se sabe; esse rótulo isolado não serve como prova positiva. [app/ponto/services/dia_decidido.py:337] [app/ponto/services/dia_decidido.py:409] |

A sonda seguinte conta **linhas atuais**, por empresa e categoria, e sobreposição por chave `(colaborador, data)` entre categorias positivas. `REVOGADOS=NA` significa que o estado não dispõe, nesta medição, de histórico de revogação consultável; não significa zero. Os contadores de `DiaPago` são *proxies* de rubricas positivas, separados por versão, e não distinguem todos os autores históricos da linha. A sonda não imprime colaboradores nem nomes; portanto, não expõe identificadores de colaborador além da forma `colN` solicitada. [app/ponto/models.py:315] [app/ponto/services/dia_pago.py:359] [não verificado]

O Python é somente leitura e foi preparado para o shell Django do envelope; **não foi executado**. O próprio `bin/sonda_leitura.sh --sombra` cria arquivos temporários, reescreve credencial/trilha e executa operações de configuração de role e grants, portanto não o executei sob a regra de não escrever. [bin/sonda_leitura.sh:115] [bin/sonda_leitura.sh:126] [bin/sonda_leitura.sh:140] [bin/sonda_leitura.sh:153] [bin/sonda_leitura.sh:216] [bin/sonda_leitura.sh:245]

```python
# Executar no shell Django do envelope de leitura, no schema desejado.
# Contagens sao linhas; sobreposicao e por colaborador + data.
# Nenhum nome, matricula ou identificador de colaborador e impresso.

from collections import Counter, defaultdict

erros = 0
contagens = Counter()
chaves = defaultdict(set)
categorias = set()

def registrar(empresa, categoria, estado, colaborador, data, positiva):
    empresa = str(empresa) if empresa is not None else "SEM_EMPRESA"
    categorias.add((empresa, categoria))
    contagens[(empresa, categoria, estado)] += 1
    if positiva and data is not None:
        chaves[(empresa, colaborador, data)].add(categoria)

try:
    from ponto.models import DecisaoHE, DiaPago
    from escala.models import CelulaDia

    try:
        campos = (
            "colaborador__empresa_id", "colaborador_id", "data",
            "origem", "estado", "revogada_em",
        )
        for empresa, col, dia, origem, estado, revogada in (
            DecisaoHE.objects.values_list(*campos).iterator(chunk_size=2000)
        ):
            categoria = "HE_%s_%s" % (origem, estado)
            situacao = "REVOGADO" if revogada is not None else "VIGENTE"
            registrar(
                empresa, categoria, situacao, col, dia,
                positiva=(revogada is None and estado == "autorizado"),
            )
    except Exception as exc:
        erros += 1
        print("ERRO|fonte=DecisaoHE|tipo=%s" % type(exc).__name__)

    try:
        qs = CelulaDia.objects.filter(
            veredito__in=("trabalhou", "fato_sem_previsao", "fato_em_ausencia")
        ).values_list(
            "colaborador__empresa_id", "colaborador_id", "data",
            "veredito_via", "veredito",
        )
        for empresa, col, dia, via, veredito in qs.iterator(chunk_size=2000):
            categoria = "CELULA_%s_%s" % (via, veredito)
            registrar(empresa, categoria, "ATUAL", col, dia, positiva=True)
    except Exception as exc:
        erros += 1
        print("ERRO|fonte=CelulaDia|tipo=%s" % type(exc).__name__)

    try:
        qs = DiaPago.objects.filter(tipo="dia", data__isnull=False).values_list(
            "colaborador__empresa_id", "colaborador_id", "data", "versao",
            "horas_trabalhadas", "horas_folga_trabalhada",
            "minutos_realizados",
        )
        for empresa, col, dia, versao, horas, folga, minutos in qs.iterator(
            chunk_size=2000
        ):
            # Cada linha pode entrar nas duas categorias; ambas sao proxies.
            if (horas or 0) > 0 or (minutos or 0) > 0:
                registrar(
                    empresa, "DIAPAGO_%s_TRABALHO" % versao, "ATUAL",
                    col, dia, positiva=True,
                )
            if (folga or 0) > 0:
                registrar(
                    empresa, "DIAPAGO_%s_FOLGA" % versao, "ATUAL",
                    col, dia, positiva=True,
                )
    except Exception as exc:
        erros += 1
        print("ERRO|fonte=DiaPago|tipo=%s" % type(exc).__name__)

    sobreposicoes = Counter()
    pares = Counter()
    for (empresa, _col, _dia), fontes in chaves.items():
        if len(fontes) > 1:
            for fonte in fontes:
                sobreposicoes[(empresa, fonte)] += 1
            fontes = sorted(fontes)
            for i, primeira in enumerate(fontes):
                for segunda in fontes[i + 1:]:
                    pares[(empresa, primeira, segunda)] += 1

    for empresa, categoria in sorted(categorias):
        atual = contagens[(empresa, categoria, "ATUAL")]
        vigente = contagens[(empresa, categoria, "VIGENTE")]
        revogado = (
            str(contagens[(empresa, categoria, "REVOGADO")])
            if categoria.startswith("HE_") else "NA"
        )
        print(
            "CENSO|empresa=%s|categoria=%s|atuais=%d|vigentes_HE=%d"
            "|revogados=%s|dias_sobrepostos=%d"
            % (
                empresa, categoria, atual, vigente, revogado,
                sobreposicoes[(empresa, categoria)],
            )
        )

    for (empresa, primeira, segunda), quantidade in sorted(pares.items()):
        print(
            "PAR|empresa=%s|categoria1=%s|categoria2=%s|dias=%d"
            % (empresa, primeira, segunda, quantidade)
        )

except Exception as exc:
    erros += 1
    print("ERRO|fonte=inicializacao|tipo=%s" % type(exc).__name__)
finally:
    print("ERROS=%d" % erros)
    print("FIM_SONDA_B6")
```

A sobreposição da sonda identifica coincidência de categorias no mesmo colaborador e dia; ela **não** prova pagamento duplicado nem independência dos eventos, pois uma lavratura pode derivar de outra decisão. [app/ponto/portas/he.py:211] [app/ponto/services/dia_pago.py:135]

perguntas de lei: 3

## 19/11 00:46 · sessao 7f1b82d865ae · QUEIMA-B2-disputa · censo · origem=Code

O modelo correspondente à Disputa é `DisputaSupervisao`. Das 25 colunas, **7 são campos dele**; as outras 18 pertencem a outros modelos. A classificação abaixo foi obtida por AST das declarações em [models.py](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1847).

**O censo de leitores e escritores está incompleto** [não verificado]. Confirmei os pontos internos de escrita abaixo, mas não rastreei todas as ocorrências de produção, testes, migrations e templates; portanto, não apresento totais de ocorrências nem `FIM_CENSO_disputa` [não verificado]. [nao verificado]

### Colunas de `DisputaSupervisao`

| Coluna | Declaração |
|---|---|
| `motivos` | [models.py:1878](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1878) |
| `fechada_em` | [models.py:1894](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1894) |
| `reaberturas_colab` | [models.py:1895](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1895) |
| `reaberturas_admin` | [models.py:1899](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1899) |
| `fechada_por` | [models.py:1903](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1903) |
| `resolucao` | [models.py:1910](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1910) |
| `motivo_fechamento` | [models.py:1930](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1930) |

### Fora deste censo

| Modelo | Colunas e declarações |
|---|---|
| `ChamadoColaborador` | `status_local` [models.py:58](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:58); `via_encerramento` [linha 60](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:60); `respondido_por` [linha 64](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:64); `resolvido_em` [linha 66](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:66). |
| `PerguntaDisputa` | `resposta_colab` [linha 2634](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2634); `respondida_em` [linha 2635](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2635); `validada_em` [linha 2636](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2636); `data_conferida_em` [linha 2645](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2645); `data_conferida_por` [linha 2646](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2646); `pendencia_admin` [linha 2743](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2743); `recusa_motivo` [linha 2747](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2747); `via_resolucao` [linha 2748](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2748); `veredito_lavrado` [linha 2772](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2772); `julgado_em` [linha 2777](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2777); `janela_expira_em` [linha 2783](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2783); `validada_por` [linha 3631](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3631); `materializada_em` [linha 3647](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3647); `materializada_por` [linha 3651](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:3651). |

`materializada_em` e `materializada_por` também são campos de `RegularizacaoExterna`: [models.py:4377](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:4377) e [linha 4378](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:4378).

### Pontos de escrita confirmados em produção — levantamento parcial

| Coluna | Atribuições e persistência encontradas no próprio modelo |
|---|---|
| `fechada_em` | Atribuições em [linha 2095](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2095), [linha 2312](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2312) e [linha 2336](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2336); respectivos `save` em [2099](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2099), [2324](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2324) e [2340](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2340). |
| `fechada_por` | Atribuições em [2096](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2096), [2313](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2313) e [2337](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2337); `save` em [2099](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2099), [2324](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2324) e [2340](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2340). |
| `resolucao` | Atribuições em [2097](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2097), [2314](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2314), [2338](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2338) e [2428](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2428); `save` em [2099](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2099), [2324](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2324), [2340](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2340) e [2429](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2429). |
| `motivo_fechamento` | Atribuições em [2098](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2098), [2315](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2315) e [2339](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2339); `save` em [2099](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2099), [2324](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2324) e [2340](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2340). |
| `reaberturas_admin` | Atribuição em [2316](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2316), incluída em `campos` em [2317](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2317) e no `save` em [2324](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2324). |
| `reaberturas_colab` | Atribuição em [2402](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2402) e `save` em [2403](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2403). |
| `motivos` | Atribuições em [2129](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2129), [2226](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2226) e [2322](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2322); `save` em [2131](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2131), [2228](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2228) e [2324](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2324). |

Um leitor de `fechada_em` que exige acompanhar fluxo é o `Q` atribuído a `q_disp`: ele é construído em [models.py:2860](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2860) e aplicado ao queryset de `DisputaSupervisao` em [linha 2862](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:2862). A lista completa de leitores, receptores ambíguos e ocorrências em testes e migrations permanece [não verificado].

### Script efetivamente usado

Este foi o código AST executado para classificar **declarações**, não o script completo de censo solicitado. Ele usa o diretório corrente, não recebe `<raiz>`, e não conta leitores ou escritores [não verificado].

```python
import ast

p = 'app/chamados/models.py'
t = ast.parse(open(p, encoding='utf-8').read())
cols = (
    'status_local resolvido_em via_encerramento respondido_por '
    'fechada_em fechada_por resolucao motivo_fechamento '
    'reaberturas_admin reaberturas_colab motivos materializada_em '
    'materializada_por via_resolucao respondida_em resposta_colab '
    'validada_em validada_por recusa_motivo pendencia_admin '
    'data_conferida_em data_conferida_por veredito_lavrado '
    'julgado_em janela_expira_em'
).split()

for n in t.body:
    if isinstance(n, ast.ClassDef):
        for x in n.body:
            if isinstance(x, (ast.Assign, ast.AnnAssign)):
                targets = x.targets if isinstance(x, ast.Assign) else [x.target]
                for a in targets:
                    if (
                        isinstance(a, ast.Name)
                        and a.id in cols
                        and isinstance(x.value, ast.Call)
                        and isinstance(x.value.func, ast.Attribute)
                        and isinstance(x.value.func.value, ast.Name)
                        and x.value.func.value.id == 'models'
                    ):
                        print(a.id, n.name, x.lineno)
```

O alcance dessa AST ficou limitado a atribuições diretas com chamada `models.<campo>` no corpo de classes de `models.py`; ela não percorreu os demais arquivos, templates, SQL cru, fluxo de querysets ou nomes calculados [não verificado]. Não houve acesso a banco ou logs de produção [não verificado].

## 19/11 00:46 · sessao 74ad20af0502 · QUEIMA-B4 · sonda · origem=Code

**O código identifica a cláusula que devolve ESTRUTURA nos nove dias, mas o material não contém os insumos brutos para uma autópsia completa por dia.** Também não identifica o autor humano da batida de col200 nem prova a decisão histórica da guarda de 60 minutos. O script abaixo mede o que é reproduzível com os dados fornecidos e marca os demais pontos como não verificados.

**(a) Nove dias ESTRUTURA.** Para col301 em 22/10, 29/10 e 05/11; col634 em 27/10; e col968 em 02/11, 03/11, 05/11, 10/11 e 12/11, o anexo informa ESTRUTURA. [não verificado: anexo fornecido sem arquivo:linha independente]

Se esse resultado veio da função da cópia indicada, a cláusula decisiva é o retorno final em [e6_oraculo.py:110](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:110). Por implicação do fluxo, em cada um dos nove julgamentos: as três flags de folga/vínculo eram falsas; `batida_fora_de_turno` era falsa; a quantidade de `horas_do_dia` era par, inclusive possivelmente zero; `fase_deslocada` era falsa; e `em_cadastro_x_realidade` era falsa. Isso decorre das condições anteriores e do retorno final em [e6_oraculo.py:98](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:98). A célula `trabalha=False, origem=gerada` é informada para os casos, mas não é argumento direto desse retorno. [não verificado: anexo sem arquivo:linha independente; valores brutos de cada chamada não fornecidos]

`InsumosDoDono.dono` obtém os demais sinais do realizado, da fase e do cadastro e consulta a lista cadastro × realidade quando necessária ([e6_oraculo.py:182](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:182)). Os horários individuais, a contagem exata e os registros que fundamentaram cada flag não foram fornecidos. [não verificado]

**(b) col129 × col155.** O anexo informa BATIDA para col129 em cinco dias e CADASTRO para col155 em quatro; não apresenta o resultado de col155 em 08/11. Também informa marco próximo de 21:30 E vazio, a aproximadamente 240 minutos das batidas de 17:30. [não verificado: col129_col155.txt foi transcrito no pedido, sem arquivo disponível na cópia]

O código dá uma explicação plausível para a diferença, mas não comprova os insumos desses nove julgamentos. A documentação de um teste descreve col129 com entrada 17:30 e saída 06:30 separadas pelo teto do dia, produzindo turno aberto e saída órfã; ela descreve também um caso de col155 com troca de vínculo e segmentos distintos ([test_col129_par_separado_pelo_teto.py:6](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/tests/test_col129_par_separado_pelo_teto.py:6)). Outro teste, **semelhante a col155**, descreve célula ainda ligada a geradora cuja vigência terminou, apesar de existir vínculo posterior; nesse cenário o juiz do vínculo retorna `None` e o dono é CADASTRO ([test_col129_teto_vira_lista.py:255](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/tests/test_col129_teto_vira_lista.py:255)).

A precedência explica como isso pode ocorrer: ausência de vínculo ou folga não definida leva a CADASTRO antes da avaliação de batida; fora disso, batida fora de turno ou quantidade ímpar leva a BATIDA ([e6_oraculo.py:98](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:98)). **Qual dessas condições efetivamente decidiu cada data, e os vínculos, DNA da célula e conjunto de batidas correspondentes, não foram fornecidos.** [não verificado]

**(c) col200, 03/11.** O dado identifica a plantada 146569 às 02:00 e atribui a origem `disputa_s84_retro`; o registro da pergunta, o autor da validação e o histórico do ato não estão disponíveis. [não verificado: envelope sem arquivo:linha]

Na cópia, o caminho compatível com essa origem é: validação chama o materializador em [validacao.py:786](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/validacao.py:786); o ramo de resposta `hora` chama `_materializar_resposta_hora` em [materializacao.py:1925](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/materializacao.py:1925); esse ramo deriva instante e tipo, procura batida existente e pode chamar `registrar_batida` com origem `disputa_s84_retro` e `request_user` como aprovador ([materializacao.py:1360](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/materializacao.py:1360), [materializacao.py:1487](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/materializacao.py:1487)). A porta cria a linha quando suas condições permitem, em [registro_batida.py:176](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/registro_batida.py:176).

Isso identifica **o caminho no código atual**, não prova quem criou a linha específica ou qual chamador histórico a alcançou. [não verificado]

**(d) As 14 plantadas.** A guarda de 60 minutos pertence a `classe_do_lote`: depois das decisões anteriores sobre resposta, marco e distância ao marco, ela procura qualquer batida apurável no intervalo inclusivo de 60 minutos em torno do **instante do slot derivado**; se encontrar, retorna lote B com `conflito=True` ([validacao.py:1194](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/validacao.py:1194), [validacao.py:1204](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/validacao.py:1204)). É uma regra de **classificação**, e pode nem ser alcançada se uma condição anterior já devolver lote B ([validacao.py:1199](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/validacao.py:1199)).

Em cada um dos 14 pares, a distância informada entre plantada e real supera 60 minutos; o script imprime a distância calculada a partir dos horários fornecidos. Isso mostra que **essa real específica estaria fora da janela se o slot fosse o instante da plantada**. O envelope não fornece prova da execução histórica, do slot então derivado, de todas as outras batidas apuráveis ou da cláusula que decidiu a classe em cada caso. Portanto, “por que cada uma passou” pela guarda histórica permanece [não verificado]. Os pares e horários são os transcritos no pedido. [não verificado: arquivo:linha do envelope] [nao verificado]

O script executa a função real `dono_da_divergencia` extraída por AST da cópia, com insumos **sintéticos explicitamente identificados**; calcula as 14 distâncias; e verifica estaticamente os pontos do caminho de escrita. Ele não acessa banco, não chama funções de escrita e não reconstrói dados ausentes. `ERROS` conta casos cuja autópsia histórica solicitada permanece incompleta; não representa falhas de execução. A extração evita importar Django e aplica exatamente o corpo da função existente em [e6_oraculo.py:53](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/ponto/management/commands/e6_oraculo.py:53). **O script não foi executado nesta sessão.** [não verificado]

```python
import ast
import datetime as dt
from pathlib import Path

# Executar na raiz da cópia. Somente leitura; não importa módulos do projeto.
raiz = Path(".")
erros = 0

def ler(caminho):
    return (raiz / caminho).read_text(encoding="utf-8")

def funcao_pura(caminho, nome):
    fonte = ler(caminho)
    arvore = ast.parse(fonte, filename=caminho)
    no = next(
        n for n in arvore.body
        if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef))
        and n.name == nome
    )
    modulo = ast.Module(body=[no], type_ignores=[])
    ast.fix_missing_locations(modulo)
    ambiente = {}
    exec(compile(modulo, caminho, "exec"), ambiente)
    return ambiente[nome], no.lineno

arquivo_juiz = "app/ponto/management/commands/e6_oraculo.py"
juiz, linha_juiz = funcao_pura(arquivo_juiz, "dono_da_divergencia")

# São chamadas sintéticas à função real, não reconstruções dos fatos.
print("JUIZ_SINTETICO",
      f"{arquivo_juiz}:{linha_juiz}",
      "par=0, cxr=False, demais flags=False ->",
      juiz([], False))
print("JUIZ_SINTETICO",
      "sem_vinculo=True ->",
      juiz([], False, sem_vinculo_no_dia=True))
print("JUIZ_SINTETICO",
      "uma hora ->",
      juiz([1], False))

casos_a = {
    "col301": ["2026-10-22", "2026-10-29", "2026-11-05"],
    "col634": ["2026-10-27"],
    "col968": [
        "2026-11-02", "2026-11-03", "2026-11-05",
        "2026-11-10", "2026-11-12",
    ],
}
for col, dias in casos_a.items():
    for dia in dias:
        print(
            "A", col, dia,
            "dono informado=ESTRUTURA",
            "clausula=app/ponto/management/commands/e6_oraculo.py:110",
            "inferido: flags cadastro=False, fora_turno=False,",
            "paridade=par, fase=False, cxr=False;",
            "insumos brutos=[nao verificado]",
        )
        erros += 1

casos_b = {
    "col129": [
        "2026-10-25", "2026-11-01", "2026-11-02",
        "2026-11-08", "2026-11-15",
    ],
    "col155": ["2026-10-25", "2026-11-01", "2026-11-02", "2026-11-15"],
}
for col, dias in casos_b.items():
    for dia in dias:
        dono = "BATIDA" if col == "col129" else "CADASTRO"
        print(
            "B", col, dia, "dono informado=" + dono,
            "clausula e insumos especificos=[nao verificado]",
            "precedencia=app/ponto/management/commands/e6_oraculo.py:98",
        )
        erros += 1

print(
    "C col200 2026-11-03 id=146569",
    "caminho compativel:",
    "app/chamados/services/validacao.py:786 ->",
    "app/chamados/services/materializacao.py:1925 ->",
    "app/chamados/services/materializacao.py:1488 ->",
    "app/ponto/registro_batida.py:176;",
    "autor e caminho historico=[nao verificado]",
)
erros += 1

# Dados transcritos do pedido: col, id plantada, instante plantada,
# id real, instante real. Os instantes são tratados na mesma zona.
pares = [
    ("col40", 139931, "2026-10-23 12:00:00", 135364, "2026-10-23 13:37:31"),
    ("col60", 152873, "2026-11-03 11:00:00", 144882, "2026-11-03 13:19:17"),
    ("col200", 146569, "2026-11-03 02:00:00", 144414, "2026-11-03 03:02:17"),
    ("col263", 146777, "2026-10-31 14:00:00", 142735, "2026-10-31 16:16:40"),
    ("col301", 151816, "2026-11-08 23:00:00", 149559, "2026-11-08 20:00:06"),
    ("col444", 140817, "2026-10-28 16:00:00", 139776, "2026-10-28 13:01:36"),
    ("col446", 144666, "2026-10-25 18:00:00", 136990, "2026-10-25 15:57:29"),
    ("col549", 151894, "2026-11-10 17:20:00", 151065, "2026-11-10 14:20:02"),
    ("col727", 140816, "2026-10-25 14:00:00", 136998, "2026-10-25 16:01:28"),
    ("col881", 154095, "2026-11-13 12:00:00", 154081, "2026-11-13 13:19:50"),
    ("col899", 151735, "2026-10-24 10:00:00", 136112, "2026-10-24 11:17:42"),
    ("col909", 149856, "2026-11-07 12:00:00", 148650, "2026-11-07 14:59:24"),
    ("col996", 153862, "2026-10-29 15:50:00", 140804, "2026-10-29 13:17:11"),
    ("col996", 153847, "2026-11-04 15:52:00", 146242, "2026-11-04 12:54:21"),
]
for col, plantada, tp, real, tr in pares:
    distancia = abs(
        (dt.datetime.fromisoformat(tr) - dt.datetime.fromisoformat(tp))
        .total_seconds()
    ) / 60
    print(
        "D", col, "plantada", plantada, "real", real,
        f"distancia={distancia:.2f}min",
        "fora_de_60=" + str(distancia > 60),
        "guarda=app/chamados/services/validacao.py:1204",
        "slot e decisao historica=[nao verificado]",
    )
    erros += 1

# Confirma que os símbolos e a constante ainda estão na cópia examinada.
verificacoes = [
    ("app/chamados/services/validacao.py", "def classe_do_lote("),
    ("app/chamados/services/validacao.py", "CONFLITO_LOTE_MIN = 60"),
    ("app/chamados/services/materializacao.py", "'disputa_s84_retro'"),
    ("app/ponto/registro_batida.py", "def registrar_batida("),
]
for caminho, trecho in verificacoes:
    if trecho not in ler(caminho):
        print("ERRO_COPIA", caminho, repr(trecho))
        erros += 1

print(f"ERROS={erros}")
print("FIM_SONDA_B4")
```

perguntas de lei: 0


## 19/11 00:45 · sessao 8883226ca162 · - · - · origem=Ronald (tmux)

Exemplos de leitores verificados foram enviados ao agente principal, com separação entre produção, testes e migrations. A busca não foi exaustiva [não verificado]. [nao verificado]

O `getattr` dinâmico de [historico.py:105](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:105) percorre objetos de `PerguntaDisputa` obtidos em [historico.py:96](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:96); as colunas iteradas aparecem em [historico.py:98](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/services/historico.py:98).

## 19/11 00:45 · sessao f5ddfa901c92 · - · - · origem=Ronald (tmux)

Escritores sintáticos dos quatro campos identificados em `ChamadoColaborador`:

- `status_local`: atribuições em [models.py:234](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:234), [models.py:368](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:368) e [models.py:383](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:383); criação em [models.py:1154](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:1154). As duas últimas atribuições alteram e restauram o valor em memória.
- `respondido_por`: [models.py:237](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:237).
- `resolvido_em`: [models.py:250](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:250) e [models.py:254](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:254).
- `via_encerramento`: [models.py:251](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:251) e [models.py:255](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:255).

A lista de campos da transição é salva em [models.py:302](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/models.py:302). O `create` recebe também `**campos`, cujo conteúdo adicional não foi resolvido [não verificado].

As migrations escrevem `status_local` em [0038_a3_registrado_backfill.py:10](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0038_a3_registrado_backfill.py:10), [0038_a3_registrado_backfill.py:15](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0038_a3_registrado_backfill.py:15), [0039_a3_via_backfill.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:24) e [0039_a3_via_backfill.py:33](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:33); e `via_encerramento` em [0039_a3_via_backfill.py:24](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:24), [linha 25](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:25), [linha 27](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:27), [linha 28](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:28) e [linha 34](/home/ronald/codex_ws/3f666ad84f2ac80044b91d79cbb73a0db67ea3ca/app/chamados/migrations/0039_a3_via_backfill.py:34).

A busca não estabelece ausência de escritores por argumentos dinâmicos ou SQL montado dinamicamente [não verificado].

_7218 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
