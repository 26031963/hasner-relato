# CODEX-ESTADO

> Ultima resposta processada: **12/11/2026 10:41** · publicado em **12/11/2026 10:43** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 12/11 10:41 · sessao 8f661e04ae0f · - · - · origem=Ronald (tmux)

- O escritor funcional é `retratar_perguntas_absolvidas_por_celula`: percorre as perguntas absolvíveis, obtém `get_user_sistema()` e chama `p.carimbar(PerguntaDisputa.VIA_CELULA_ABSOLVEU, _user)`, **sem pedir escrita de resposta**. `app/chamados/reconciliador.py:550`, `:585`, `:588`, `:596`.
- A porta `carimbar` tem `resposta=None`; escolhe `quando` ou `timezone.now()`, chama `_escrever_carimbo`, consulta `pergunta_viva` e só salva se a pergunta resultar morta. A atribuição literal de `materializada_em` está em `_escrever_carimbo`; a persistência está em `self.save(update_fields=campos)`. `app/chamados/models.py:3104`, `:3125`, `:3130`, `:3100`, `:3140`, `:3144`, `:3153`.
- `respondida_em` só recebe instante dentro de `if resposta and not texto_vazio(self.resposta_colab)`. Como o chamador desta via não passa `resposta`, é coerente haver `materializada_em` preenchido com `respondida_em` nulo. `app/chamados/models.py:3131`, `:3132`, `:3133`; `app/chamados/reconciliador.py:596`.
- A seleção exige carimbo e validação ainda nulos, célula vinculada, julgada e com veredito que não acusa; depois o gerador consulta o juiz real e exclui dia trancado. `app/chamados/reconciliador.py:628`, `:630`, `:631`, `:632`, `:638`, `:640`, `:642`, `:662`, `:668`.
- O juiz pode dar a pergunta por morta por célula com veredito que não acusa e sem marcos pendentes; isso não exige resposta nem plantio de batida. `app/chamados/juizes.py:1438`, `:1440`, `:1451`, `:1452`, `:1453`.
- `celula_absolveu` pertence explicitamente a `VIAS_SEM_TESTEMUNHA`: o comentário identifica a célula julgada como testemunha e explica que não há batida a exigir num dia absolvido. Portanto, nesta via, “materializada” é carimbo de processamento/resolução pela célula, e não prova de declaração do colaborador ou batida plantada. `app/chamados/models.py:3731`, `:3741`, `:3742`, `:3743`, `:3747`.
- O `help_text` do campo ainda diz “batida criada + chamado fechado”, descrição mais estreita que o comportamento executável acima. `app/chamados/models.py:3394`, `:3396`.
- Isso não altera o predicado solicitado no aval; invalida usar `materializada_em != NULL` como sinônimo de “existe batida plantada”. O comando existente faz essa divisão em `classificar`, mandando qualquer carimbada para `_fonte_materializada`, que reprova ausência de batida plantada viva. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:151`, `:152`, `:68`, `:69`, `:72`.
- O caminho histórico exato que carimbou cada uma das 931 perguntas, sua data e seu autor no banco não foi comprovado nesta inspeção. [nao verificado]

## 12/11 10:41 · sessao 9913f3b3b51d · - · - · origem=Ronald (tmux)

- `classificar()` considera “materializada” apenas pela presença de `materializada_em` e escolhe `_fonte_materializada`; essa função exige batida plantada viva e lança `sem_fonte` quando ela falta. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147`, `:151`, `:65`, `:68`, `:72`)

- `_fonte_viva` começa com turnos que têm entrada, não têm saída e possuem `data_turno`. Se **qualquer** candidato tem `data_turno == data_evento`, retorna `None` antes de procurar um candidato de outro dia. Logo, se coexistirem um aberto no próprio dia e outro aberto cujo dia diverge, esse segundo não será avaliado nessa chamada; a existência dessa configuração nas 931 é [nao verificado]. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`, `:109`, `:110`)

- Há outros cortes: ignora candidatos com `data_turno >= data_evento`; dos anteriores, aceita somente aqueles cuja saída prevista é igual a `data_evento`. A previsão vem do último marco de `marcos_turno`, convertido em data local. Se não encontrar correspondência e todos os candidatos examinados tiverem marcos, retorna `None`. Esses critérios são mais restritivos que simplesmente exigir dia do turno diferente e turno ainda sem saída. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:113`, `:115`, `:118`, `:122`, `:125`, `:82`, `:90`, `:91`)

- Se não houver correspondência, mas existir candidato anterior sem marcos, lança `sem_fonte`; se houver mais de uma correspondência, lança `ambigua`. Portanto, os cortes com retorno `None` devem ser distinguidos dessas exceções. (`app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:116`, `:120`, `:123`)

- **Contador potencialmente ocultador na v3: `coerente_ou_sem_turno_aberto` (860 no JSON fornecido).** A associação exata entre os retornos `None` e esse contador é [nao verificado]: `sonda_medida3.py` e o texto do contador não foram encontrados na cópia; o usuário forneceu somente a saída da sonda. A quantidade das 860 que satisfaria o predicado do aval também é [nao verificado].

- O juiz efetivo consulta `dia_das_batidas` para a batida por PK; a implementação atribui a cada batida reclamada o `data_turno` do turno e usa a data local para as demais. (`app/ponto/turnos.py:2927`, `:2935`, `:2945`, `:2953`, `:2955`)

## 12/11 10:39 · sessao aa1c00a286c9 · ABSOLVIDA-PELA-FOLGA · sonda · origem=Code

1. Transcrição integral do JSON fornecido; resultado de produção sem verificação independente. [nao verificado]

```json
{"caso_51407": [{"em_classificar": true, "no_universo": true}], "casos": [], "contagem": {"linhas_classificar": 931, "materializadas_no_universo": 931, "sem_dia_novo": 931, "universo_base": 931}, "foras_sem_dia": {"sem_fonte": 931}}
```

2. **`classificar` não é um medidor geral do predicado do aval.** Ela seleciona a fonte conforme `materializada_em`: preenchida → `_fonte_materializada`; ausente → `_fonte_viva`. `via_resolucao=celula_absolveu` e `respondida_em` nula não alteram essa escolha. [classificar:147](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:147)

Para `orfao_14h` **viva**, `_fonte_viva` procura turnos com entrada, sem saída e com `data_turno`. Ela:

- Devolve `None` imediatamente se **qualquer** candidato tem `data_turno == data_evento`, considerando a pergunta coerente. [fonte_viva:108](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108)
- Entre os candidatos anteriores a `data_evento`, aceita somente aqueles cuja saída prevista cai exatamente em `data_evento`; essa previsão vem do último marco da escala. [fonte_viva:112](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:112), [saida_esperada:82](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:82)
- Marca `fora=ambigua` se houver mais de um encaixe; sem encaixe, marca `fora=sem_fonte` se algum candidato examinado não tiver marcos. Sem encaixe e sem essa falta de marcos, devolve `None`, inclusive quando existem turnos abertos que não satisfazem o encaixe exigido. Um único encaixe devolve sua entrada e seu turno. [fonte_viva:120](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120)

Para **materializada**, a fonte exigida é uma batida viva vinculada por `Batida.pergunta_origem`, não a entrada aberta encontrada por `_fonte_viva`. Sem essa batida, `celula_absolveu` cai em `sem_fonte`; múltiplas batidas geram `ambigua`; uma batida ausente dos turnos gera `sem_fonte`, e presente em múltiplos turnos gera `ambigua`. [classificar:138](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138), [fonte_materializada:65](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65)

Obtida a fonte, `novo` só é preenchido depois de o juiz devolver um dia não nulo, diferente de `data_evento` e igual ao `data_turno` encontrado. Juiz sem dia gera `sem_fonte`; divergência entre juiz e turno gera `ambigua`, ambos ainda sem `novo`. Depois de preencher `novo`, `orfao_14h` materializada recebe `fora_do_aval`; para quem passar dessa condição, fato já existente no destino gera `colisao`. Portanto, **`novo` preenchido não significa ausência de `fora`**. [classificar:160](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160)

Precisão sobre `None`: quem o devolve é `_fonte_viva`; `classificar` responde com uma lista e omite essa pergunta por `continue`. Também omite materializada cujo turno não tenha saída ou cuja saída não caia em `data_evento`, e qualquer pergunta cujo dia devolvido pelo juiz já coincida com o gravado. [classificar:154](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:154), [classificar:176](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:176)

**Sim, o predicado amplo do aval pode sofrer descarte silencioso:** uma materializada com fonte encontrada, turno ainda aberto e dia diferente é omitida antes de consultar o juiz, porque `t.saida is None`. Nas vivas, uma entrada aberta com dia diferente também pode ser omitida quando sua saída prevista não coincide com `data_evento`; existir outro turno aberto no próprio `data_evento` provoca o retorno antecipado de “coerente”. Essas condições são mais restritivas que apenas “entrada aberta cujo dia difere do gravado”. [classificar:158](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:158), [fonte_viva:108](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108)

3. **O bloqueio equivalente nesta saída é `sem_dia_novo=931`, explicado por `foras_sem_dia.sem_fonte=931`.** Os mesmos 931 constam como materializados e como linhas retornadas; portanto, o agregado fornecido não mostra perda de perguntas entre universo e retorno, mas mostra todas sem dia novo. [nao verificado]

Como materializadas seguem `_fonte_materializada`, chamar a função real **não garante chamar o caminho que procura entrada aberta**; a exigência de batida plantada pode barrar a medição antes disso. [classificar:151](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:151), [fonte_materializada:68](/home/ronald/codex_ws/e4fd9f1b5c0fae57791b510c4c2b816dd0590b21/app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:68)

Assim, `casos: []` **não demonstra zero casos do aval**. O JSON só confirma, para #51407, presença no universo e no retorno; não fornece seu dia novo, sua entrada aberta nem separação entre exportadas e não exportadas. O número que satisfaz efetivamente o predicado do aval permanece [nao verificado].

## 12/11 10:38 · sessao 5cbda7b3d038 · - · - · origem=Ronald (tmux)

Entradas já alcançam a relavra comum: lançar [escala/folgas.py:337], alterar/remover [:380], lote [:418], reparentar [:523]. A relavra suplementa futuras em [:298]. Assim, testes comportamentais de amanhã devem ficar GREEN no HEAD; RED no HEAD desses comportamentos é [nao verificado]. A deficiência apontada é de evidência individual. [nao verificado]

Para ampliar a suíte pura: [nao verificado]

- Carregar `_foto_do_dia`, `lancar_folga`, `alterar_folga`, `aplicar_folgas_em_lote`, `reparentar_folga` e **a relavra real**, usando `_dias_dos_meses_que_sairam=lambda *a: []`; o teste atual da relavra já usa esse preparo. [ponto/tests/test_cadastro_futuro_sem_banco.py:584] [nao verificado]
- Reutilizar `calendario()`: seu `eh_dia_trabalho_calculado` consulta as fotos do vínculo, permitindo verificar que amanhã muda de trabalho para folga. [ponto/tests/test_cadastro_futuro_sem_banco.py:480]
- Acrescentar `Query.get_or_create`, necessário para lançamento. [escala/folgas.py:335]
- Usar foto com `save/delete`, `pk`, `tipo` e `calendario_origem=None`; `Foto` atual fornece somente a propriedade do calendário. [ponto/tests/test_cadastro_futuro_sem_banco.py:176]
- Dublar `ponto.precedencia.veto_de_lancamento` e `core.models.LogAuditoria`; a porta de reparentamento exige também `vinculo_do_dia_carregado`, retornando o vínculo novo pela autoridade previamente carregada. [escala/folgas.py:328] [escala/folgas.py:479] [escala/folgas.py:493]

Casos individuais recomendados:

1. `lancar_folga(novo, AMANHA, 'F')`: começar com amanhã coerente, trabalhando, nomeando `novo`; verificar foto criada e célula não trabalhando. [escala/folgas.py:335]
2. `alterar_folga(novo, AMANHA, 'F2')`: foto manual já existente e célula com DNA anterior incoerente; verificar tipo e DNA atualizados. [escala/folgas.py:364]
3. Remoção: foto manual existente, célula coerente como folga; `alterar_folga(..., '')` remove foto e amanhã volta a trabalhar. [escala/folgas.py:353]
4. `aplicar_folgas_em_lote(novo, [(AMANHA, 'F')], hoje=HOJE)`: conferir lista gravada, foto e célula. [escala/folgas.py:405]
5. Reparentar: foto manual de amanhã no vínculo velho encerrado hoje; célula de amanhã já nomeia `novo`; conferir retorno `migrada`, FK da foto e célula. [escala/folgas.py:487] [escala/folgas.py:515]
6. Aplicar calendário: carregar função real da view sem decoradores; publicar serviços reais preparados em `sys.modules`; fornecer `request.method='POST'` e `POST['tipo_escala_id']`; dublar somente `JsonResponse`, `get_object_or_404` e relógio. [escala/views.py:376] [escala/views.py:388]

O mutante comum apropriado é envolver a chamada futura da relavra em `if ec.data_inicio <= _hoje_teste`. O mutador atual apenas remove essa chamada porque `relavrar_celulas_da_foto` não integra seu conjunto de guardas; precisa receber essa variante. [ponto/tests/test_cadastro_futuro_sem_banco.py:87] [escala/folgas.py:298]

Aplicar calendário tem redundância legítima: troca a FK com sincronização futura, materializa fotos que também relavram e chama relavra explícita. Mutar somente a chamada explícita pode sobreviver graças aos outros caminhos; o teste deve executar tudo e aplicar a guarda aos transportes futuros comuns. [escala/views.py:409] [escala/views.py:411] [escala/views.py:416] [colaboradores/services/vinculo.py:454] [escala/folgas.py:215] [nao verificado]

## 12/11 10:38 · sessao b118550ea971 · - · - · origem=Ronald (tmux)

Item 4(c) está bloqueado pelo limite de escrita: a regeneração fora da vigência conserva `escala_geradora` e só salva se `trabalha` ainda não for falso ([celula.py:238](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:238)). A juíza devolve `None` para a geradora encerrada ([alimentacao.py:282](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/escala/alimentacao.py:282)), portanto o contador permanece positivo corretamente ([vigia_cadastro_futuro.py:36](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/services/vigia_cadastro_futuro.py:36)). Nenhuma operação existente permite limpar o carimbo: a reversão também só atribui geradora quando o identificador restaurado é verdadeiro ([celula.py:789](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:789)). Resolver sem editar esse arquivo exigiria outro escritor ou alterar artificialmente julgamento/contador.

Item 5 admite solução na casca do comando: dentro do atomic existente, regenerar somente células geradas existentes de hoje, chamando a porta atual com `recalculo=False` e `estender_horizonte=False`; a chamada ficaria junto ao gerador ([gerar_celulas.py:310](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/management/commands/gerar_celulas.py:310)). Assim o cron mantém a execução pelo comando, sem editar sua configuração. A seleção deve preservar o universo ativo do gerador ([celula.py:830](/home/ronald/wt-codex-bug-cadastro-nao-alcanca-o-futuro-090745/app/ponto/portas/celula.py:830)); usar a geradora carimbada julgada pode divergir do comportamento anterior em sobreposições. Equivalência completa desse caminho alternativo: [nao verificado].

## 12/11 10:36 · sessao 0d7a98f2dd10 · ABSOLVIDA-PELA-FOLGA · sonda · origem=Code

**931 no universo; 931 em `sem_batida_origem`; listas de casos vazias.** Transcrição do JSON fornecido, sem conferência em produção. [nao verificado]

```json
{"caso_51407": [], "casos": [], "contagem": {"sem_batida_origem": 931, "universo_base": 931}}
```

- **Exportada × não exportada:** os grupos não constam da saída; não podem ser transcritos como zero. Nenhuma tupla `(colNNN, #pk, data_evento, dia do turno, competência)` foi retornada. [nao verificado]
- **#51407/col999 não aparece.** A saída não identifica seu contador individual. Se integrou `universo_base`, os totais são compatíveis com exclusão em `sem_batida_origem`; confirmar isso exige o script ou o diagnóstico individual. [nao verificado]

1. **A saída não comprova a medida do aval.** O conteúdo de `sonda_medida1.py` não está no texto recebido nem foi localizado nesta cópia; seus filtros de `via_resolucao`, ausência de resposta e ordem dos contadores permanecem [nao verificado].

- **A FK `PerguntaDisputa.batida_origem` não é a fonte garantida de `orfao_14h`.** Esse motivo pertence à família MARCO; a FK identifica episódios BATIDA, e seu preenchimento automático retorna `None` para MARCO. Exigir essa FK descarta perguntas sem avaliar o turno que elas cobram. — `app/chamados/catalogo/motivos.py:34`; `app/chamados/models.py:2437`; `app/chamados/models.py:3462`.
- **O argumento homônimo `resolver_data_evento(..., batida_origem=...)` representa, sim, a entrada aberta de `orfao_14h`.** Ele consulta `dia_do_turno_da_batida`; isso não significa que aquela entrada esteja gravada na FK da pergunta. — `app/escala/servico_jornada.py:495`; `app/escala/servico_jornada.py:499`; `app/escala/servico_jornada.py:509`.
- **Buscar o turno por `pk` em `t.batidas` usa a mesma associação do juiz.** Porém, o juiz do dia monta um mapa por PK e atribui o dia local às batidas não reclamadas por turno; exigir exatamente um turno acrescenta uma restrição. A equivalência também depende da janela utilizada pela sonda. — `app/ponto/turnos.py:2933`; `app/ponto/turnos.py:2952`; `app/ponto/turnos.py:2955`. Janela e restrições efetivas da sonda: [nao verificado].
- **A migração existente não exige essa FK para encontrar a entrada aberta:** procura candidato único com entrada, sem saída e último marco previsto no dia gravado, retornando `t.entrada`; também exclui explicitamente `orfao_14h` materializada. — `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:115`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:126`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`.
- **A separação deve consultar `marcos_da_competencia(...)[“exportada”]`.** O campo `marco` pode indicar uma etapa posterior, como paga; a competência da data segue a janela da empresa. — `app/ponto/services/fechamento.py:1537`; `app/ponto/services/fechamento.py:1543`; `app/ponto/services/dia_pago.py:716`.

2. **Os números e as listas são exatamente os transcritos acima; o total real do predicado continua indeterminado.** [nao verificado]

3. **O contador capaz de esconder casos é `sem_batida_origem`: 931/931, ou 100% do universo reportado.** [nao verificado] A exclusão por FK nula não demonstra ausência do predicado, pois essa família não recebe a FK automaticamente. — `app/chamados/models.py:3462`.

- **`juiz_sem_dia`, `ambiguo` e `com_saida` não foram publicados:** tamanho e participação nas exclusões individuais são [nao verificado]. [nao verificado]
- **`juiz_sem_dia` e `ambiguo` representam falta de determinação, não prova de inexistência do caso:** o juiz pode devolver `None`, e a busca da fonte pode encontrar múltiplos candidatos. — `app/ponto/turnos.py:2936`; `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120`.
- **`com_saida` excluiria corretamente apenas quando fosse a saída do turno certo da fonte**, pois o predicado exige turno ainda sem saída. O código distingue candidatos por entrada e ausência de saída; se a sonda aplicou isso ao turno correto permanece [nao verificado]. — `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`.

## 12/11 10:35 · sessao 5f24455a0385 · - · - · origem=Ronald (tmux)

- `marcos_da_competencia` separa exportada pelo booleano `['exportada']`, delegado a `competencia_entregue(empresa_id, mes, ano)`. Comparar `['marco'] == 'exportada'` perderia competências cujo marco mais adiantado é `trancada` ou `paga`. — `app/ponto/services/fechamento.py:1537`, `app/ponto/services/fechamento.py:1543`
- Entregue significa existência de `ExportacaoDominio` vigente com `invalidada_em is None`; existência de qualquer exportação ou `FechamentoMensal.status='exportado'` não substitui esse juiz. — `app/folha/export.py:588`, `app/folha/export.py:591`, `app/ponto/services/fechamento.py:1533`
- Competência do fato: `competencia_do_fato(colaborador, dia)` chama `competencias_do_fato`; a derivação usa `janela_atual(dia, empresa)` e toma mês/ano do fim da janela. — `app/ponto/services/fechamento.py:1602`, `app/ponto/services/fechamento.py:1599`, `app/ponto/services/dia_pago.py:716`
- `competencia_do_dia_entregue` já compõe janela da empresa e juiz de entrega; `None` significa ausência de dia, não comprovação de não exportada. — `app/chamados/services/vigia_fio.py:822`, `app/chamados/services/vigia_fio.py:834`, `app/chamados/services/vigia_fio.py:838`
- Datas 28/10 e 29/10 pertencem ambas à competência 11 do mesmo ano para qualquer corte permitido neste código: corte máximo 28; dia a partir do corte avança o mês. Isso não informa se a competência foi exportada. — `app/ponto/janelas.py:26`, `app/ponto/janelas.py:27`, `app/ponto/janelas.py:32`
- Conteúdo de `sonda_medida1.py`, seu cálculo da competência e seu uso efetivo de `marcos_da_competencia`: [não verificado].

## 12/11 10:35 · sessao 3e1d1c1a78a4 · - · - · origem=Ronald (tmux)

- `orfao_14h` pertence à família MARCO; sua identidade é pessoa + dia do turno + motivo + instância. A coluna `PerguntaDisputa.batida_origem` identifica episódios da família BATIDA, portanto não é uma fonte obrigatória nem garantida para `orfao_14h`. `app/chamados/catalogo/motivos.py:15`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/models.py:2437`.
- O preenchimento automático da FK devolve `None` para motivos fora da família BATIDA. Logo excluir `orfao_14h` por FK nula exclui linhas por algo esperado no modelo, sem verificar se têm turno aberto no dia errado. `app/chamados/models.py:3453`, `app/chamados/models.py:3461`.
- O acervo ficou com FK nula deliberadamente: o contexto da disputa muda e não permite reconstruir retrospectivamente o episódio antigo. `app/chamados/migrations/0052_pergunta_batida_origem.py:1`.
- O argumento homônimo `resolver_data_evento(..., batida_origem=...)` significa a entrada aberta de `orfao_14h`; ele chama `dia_do_turno_da_batida`. Esse argumento não equivale à FK da pergunta. `app/escala/servico_jornada.py:495`, `app/escala/servico_jornada.py:499`, `app/escala/servico_jornada.py:506`.
- Um emissor encontra a entrada aberta por `entradas_pendentes[].pk` e passa a batida a esse argumento. A lista contém `_t.entrada.pk` de turnos abertos. `app/chamados/detector_anomalias.py:35`, `app/chamados/detector_anomalias.py:43`, `app/chamados/detector_anomalias.py:47`, `app/ponto/utils_turno_orfao.py:30`, `app/ponto/utils_turno_orfao.py:38`.
- Outro emissor encontra a entrada com `_achar_batida_origem(chamado)`, que consulta `turnos_do_colab`, seleciona turnos abertos com entrada anterior à criação do chamado e retorna a entrada candidata; o objeto é usado em `resolver_data_evento`, sem ser passado à criação da pergunta como FK. `app/chamados/services/materializacao.py:110`, `app/chamados/services/materializacao.py:124`, `app/chamados/services/materializacao.py:125`, `app/chamados/services/materializacao.py:140`, `app/chamados/services/disputa_emissao.py:1187`, `app/chamados/services/disputa_emissao.py:1205`, `app/chamados/services/disputa_emissao.py:1213`.
- A migração já identifica fonte de `orfao_14h` viva por turno com entrada e sem saída cujo último marco previsto cai em `data_evento`, exigindo candidato único, e retorna `t.entrada`; ela não depende de `p.batida_origem`. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:82`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:108`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:115`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:120`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:126`.
- Para materializada, a migração usa batida viva com `pergunta_origem_id=p.pk`, encontra o turno por `pk` em `t.batidas` e exige exatamente um turno; após obter o dia pelo juiz, exclui explicitamente `orfao_14h` materializada como `fora_do_aval`. `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:65`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:76`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:138`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:160`, `app/chamados/management/commands/migrar_data_evento_dia_do_turno.py:168`.
- Não foi possível auditar filtros e ordem dos contadores da sonda: seu conteúdo não está no dado recebido nem foi localizado na cópia. [não verificado]

## 12/11 10:25 · sessao 9a2b122ced99 · TRIAGEM-OFFLINE-RED-HEADLESS · construtor · origem=Code

Criado [test_sw_offline_headless_red.py](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:1), somente em `core/tests/`. Não executei testes, Chromium ou Ruff; resultados abaixo são expectativas **[nao verificado]**. [nao verificado]

O harness original usa `file://`, incompatível com este registro de SW; o ajuste ficou no teste novo: servidor HTTP em localhost e CDP para clique real e corte de rede nas abas e no worker. [Harness original](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_smoke_chromium.py:115), [ajuste](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:195), [offline](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:464). [nao verificado]

A garantia da **FONTE** vem da leitura de `SW`, do redirecionamento da raiz da view real e da comparação do corpo HTTP com fonte + build. O mutante altera apenas uma cópia temporária. [Leitura e isolamento](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:226), [comparação pelo navegador](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:437).

Comandos para a sessão, a partir de `app/`:

```bash
MUTANTE_SW= PYTHONDONTWRITEBYTECODE=1 python manage.py test core.tests.test_sw_offline_headless_red --settings=config.settings_test --parallel=1 -v 2
```

Esperado: controle verde; offline RED com `PASSO 3 OFFLINE C1: clique Bater ponto deve entregar documento /ponto/bater-v2/` e `PASSO 4 OFFLINE C1: reload da home deve entregar documento nao-branco`. O reload continua sendo tentado após a falha do clique. [Asserções](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:490). **[nao verificado]** [nao verificado]

```bash
MUTANTE_SW= PYTHONDONTWRITEBYTECODE=1 python manage.py test core.tests.test_sw_offline_headless_red.SWOfflineHeadlessRedTest.test_controle_com_rede_home_e_bater --settings=config.settings_test --parallel=1 -v 2
```

Esperado: verde com rede. [Controle](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:443). **[nao verificado]** [nao verificado]

```bash
MUTANTE_SW=navega_guarda PYTHONDONTWRITEBYTECODE=1 python manage.py test core.tests.test_sw_offline_headless_red --settings=config.settings_test --parallel=1 -v 2
```

Esperado: controle e offline verdes; o mutante aguarda `cache.put` no ramo `navigate`. [Trecho do mutante](/home/ronald/wt-codex-triagem-offline-red-headless-101137/app/core/tests/test_sw_offline_headless_red.py:70). **[nao verificado]**

```bash
PYTHONDONTWRITEBYTECODE=1 python -m ruff check core/tests/test_sw_offline_headless_red.py
```

Escritores e leitores das dependências:

| Dado | Quem escreve/define | Quem lê/consome |
|---|---|---|
| Registro do SW | `static/pwa.js:14` | Inicialização em `static/pwa.js:324`; teste em `core/tests/test_sw_offline_headless_red.py:354` |
| Scope `/` | `static/pwa.js:14`; `core/views.py:574` | `static/sw.js:23`; teste em `core/tests/test_sw_offline_headless_red.py:374` |
| Build/cache | `core/views.py:555`, `:572`; `static/sw.js:1` | `static/sw.js:12`, `:20`, `:74`; teste em `core/tests/test_sw_offline_headless_red.py:440` |
| Home efetiva | `core/urls.py:17`; `core/views.py:270` | Teste em `core/tests/test_sw_offline_headless_red.py:408` |
| Link Bater ponto | `templates/core/dashboard_colaborador_v2.html:31`; `ponto/urls.py:12` | Clique em `core/tests/test_sw_offline_headless_red.py:381`; render em `ponto/views.py:356`; elemento em `templates/ponto/bater_ponto_v2.html:109` |

Os dois modos de sessão são exercitados, mas essas rotas herdam literalmente `base.html`; cobertura de `base_app.html` não é demonstrada aqui. (`templates/core/dashboard_colaborador_v2.html:1`; `templates/ponto/bater_ponto_v2.html:1`; `core/tests/test_sw_offline_headless_red.py:397`.)

## 12/11 10:24 · sessao 08ce782c30e6 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · verificador · origem=Code

**BLOQUEIA — o admin de template passa a alcançar o passado, há portas sem atomicidade entre cadastro e cura, e a suíte em memória contém um erro antes do julgamento do feriado.** (`app/escala/services/cadastro_tipo.py:337`, `app/escala/services/cadastro_tipo.py:373`, `app/colaboradores/services/vinculo.py:326`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`)

Identidade do hash/base por Git, estado atual de produção e execução da suíte Django: [nao verificado]. As comparações com `087fe758a` abaixo usam o diff fornecido, sem certificar sua completude por Git: [nao verificado].

1. Para as portas do censo, o transporte comum seleciona células **existentes, geradas, estritamente futuras, dentro da janela corrente do gerador**, e chama a mesma porta com `[dia,dia]`, sem estender horizonte. (`app/ponto/services/cadastro_futuro.py:18`, `app/ponto/services/cadastro_futuro.py:22`, `app/ponto/services/cadastro_futuro.py:30`)

Na tabela, **G** significa mutante que recoloca `data_inicio <= hoje`; **M** significa mutante que remove a chamada da cura. Ambos modificam em memória o corpo lido da produção; somente cinco funções recebem G. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:25`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:73`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:87`)

| Porta do censo | Sítio que alcança o futuro | RED e achado |
|---|---|---|
| Escala/vínculo — `executar_vinculo` | `app/colaboradores/services/vinculo.py:326` | G morto pelo teste `app/ponto/tests/test_cadastro_futuro_sem_banco.py:339`. **BLOQUEIA:** cura fora do atomic do cadastro, detalhada no item 4. |
| Flag `trabalha_em_feriado` | `app/colaboradores/services/vinculo.py:878` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:352`. **BLOQUEIA:** save e cura separados. |
| Município do posto | `app/colaboradores/services/vinculo.py:928` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:356`. **BLOQUEIA:** save e cura separados. |
| Template — `salvar_tipo` | `app/escala/services/cadastro_tipo.py:361` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:361`. **BLOQUEIA:** universo limitado a `ativa=True`, em `app/escala/services/cadastro_tipo.py:334`. |
| Template pelo admin | `app/escala/admin.py:12`, `app/escala/services/cadastro_tipo.py:373` | Teste de amanhã em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:392`. **BLOQUEIA:** passou a regenerar também o passado, em `app/escala/services/cadastro_tipo.py:348`. |
| Signal de vigência | `app/escala/signals.py:77` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:576`; G específico não demonstrado pelo mutador. |
| Signal de âncora | `app/escala/signals.py:128` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:569`; G específico não demonstrado pelo mutador. |
| Foto mensal de folgas | `app/escala/folgas.py:215`, `app/escala/folgas.py:298` | M da relavra morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584`; reconciliação mensal cobrada em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:612`. |
| Lançar folga | `app/escala/folgas.py:337` → relavra comum | **BLOQUEIA por evidência incompleta:** sem RED individual “amanhã + mutante” demonstrado; o novo teste chama diretamente a relavra, em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:586`. |
| Alterar/remover folga | `app/escala/folgas.py:380` → relavra comum | **BLOQUEIA por evidência incompleta:** teste anterior cobra idempotência, não a guarda futura por essa entrada: `app/escala/tests/test_bug136_plano_em_lote_relavra.py:105`. |
| Folgas em lote | `app/escala/folgas.py:418` → relavra comum | **BLOQUEIA por evidência incompleta:** teste anterior usa o início da competência; não demonstra o mutante desta obra: `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`. |
| Reparentar folga | `app/escala/folgas.py:523` → relavra comum | **BLOQUEIA por evidência incompleta:** há RED anterior de rejulgamento mensal, sem mordida futura desta obra demonstrada: `app/escala/tests/test_f3_beco1_cadastro_entrou.py:201`. |
| Aplicar calendário por tipo | `app/escala/views.py:409`, `app/escala/views.py:416` | **BLOQUEIA por evidência incompleta:** teste anterior cobra FK/foto, sem conferir célula futura sob mutante: `app/escala/tests/test_folga_aplicar.py:39`. |
| Editor de calendário | `app/escala/folgas.py:598` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:509`. |
| Trocar calendário do vínculo | `app/colaboradores/services/vinculo.py:454` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:497`. |
| Cadastrar/editar/importar feriado | Saves de formulário/importação atravessam o signal: `app/core/views_config.py:68`, `app/core/views_config.py:81`, `app/core/views_config.py:141`, `app/core/signals.py:29` | M do signal morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:429`. **BLOQUEIA:** prova específica de obedecer ao silêncio da juíza dá ERROR antes da chamada: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`. |
| Excluir feriado | `app/core/signals.py:35` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:435`. |
| Fechar vigência por `update` | `app/colaboradores/services/vinculo.py:639` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:366`. **BLOQUEIA:** encerramento sem sucessor deixa contador positivo, detalhado no item 5. |
| Recusa/restauração de snapshot | `app/colaboradores/services/vinculo.py:997` | M morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:448`. |
| Correção retroativa com piso amanhã | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:243` | G morto em `app/ponto/tests/test_cadastro_futuro_sem_banco.py:517`. |
| ESCALA-AUTO — aplicar/desfazer | Chamada direta à mesma porta, ainda com horizonte: `app/escala/services/escala_auto_executor.py:104`, `app/escala/services/escala_auto_executor.py:143` | **FILA:** comportamento anterior mantido; sem RED desta obra por mutante demonstrado. Os testes existentes conferem cadastro/passado: `app/escala/tests/test_escala_auto_executor.py:66`, `app/escala/tests/test_escala_auto_executor.py:117`. |

**BLOQUEIA — template não cobre todo vínculo que responde pelo futuro.** Contra-exemplo: hoje 12/11; vínculo inativo, mas com fim em 20/11; célula gerada de 13/11 nomeando esse vínculo. A juíza aceita a geradora dentro da vigência, mas a propagação do template a exclui por `ativa=True`; mudar 08:00 para 09:00 deixa o DNA antigo. (`app/escala/alimentacao.py:282`, `app/escala/services/cadastro_tipo.py:334`)

**FILA — demais entradas encontradas pelo censo:** troca de `Colaborador.posto` não muda o posto declarado no vínculo; `Posto.opera_em_feriado` define o padrão de vínculos novos; reclassificação grava apenas `tipo_base`. Não há suplemento nesses caminhos; não ficou comprovada mudança de célula vigente exigida por eles. (`app/colaboradores/services/colaborador.py:47`, `app/escala/models.py:905`, `app/colaboradores/services/estrutura.py:80`, `app/escala/services/cadastro_tipo.py:134`)

**FILA — portas técnicas adicionais:** correção de um dia continua sem horizonte; regeneração em exportada mantém horizonte apenas para vínculo ativo; cura BUG115 chama o gerador diretamente. Não ganharam RED específico desta obra demonstrado. (`app/ponto/portas/celula.py:734`, `app/ponto/portas/regen_exportada.py:174`, `app/ponto/management/commands/curar_residuo_bug115.py:135`)

2. Nos contra-exemplos do aval:

- **Nada — suplemento não cria célula fora da janela nem preenche lacunas futuras:** ele percorre somente datas existentes e desliga o horizonte. O teste isolado “início além da janela” passa; trocar para horizonte habilitado é morto pelos testes que detectam novas células entre 14 e 20/11. (`app/ponto/services/cadastro_futuro.py:22`, `app/ponto/services/cadastro_futuro.py:31`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:374`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:386`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584`)
- **FILA — a garantia não vale universalmente para AUTO:** `_ate` retorna pelo menos o início, mesmo sem célula; início além da janela cria aquela célula. É comportamento anterior preservado, não introduzido pelo suplemento. (`app/escala/services/escala_auto_executor.py:52`, `app/escala/services/escala_auto_executor.py:104`, `app/ponto/portas/celula.py:259`)
- **Nada — `origem='editada'` fica fora da cura explícita:** seleção e escritor a preservam; snapshot e desfazer agora recusam apagar sua geradora. (`app/ponto/services/cadastro_futuro.py:23`, `app/ponto/portas/celula.py:263`, `app/colaboradores/services/vinculo.py:990`, `app/colaboradores/services/vinculo.py:1062`)
- **BLOQUEIA — admin de template amplia o passado:** o hook novo chama `_propagar_regeneracao_template`, que começa na competência anterior e regenera até hoje. Contra-exemplo: célula gerada de ontem de vínculo ativo; editar horário pelo admin agora reescreve seu DNA, embora essa entrada antes apenas salvasse o template no diff fornecido. (`app/escala/admin.py:12`, `app/escala/services/cadastro_tipo.py:337`, `app/escala/services/cadastro_tipo.py:348`, `app/escala/services/cadastro_tipo.py:373`)
- **BLOQUEIA — “só futuro” não limita todos os efeitos de escrita:** `fechar_vigencia` ganhou regeneração com recálculo padrão; a cauda chama o fechamento da competência inteira e este grava fechamento/dias pagos. Encerrar hoje, tocando amanhã, pode relavrar dias anteriores da mesma competência por esse caminho novo. Movimento efetivo de valores em produção: [nao verificado]. (`app/colaboradores/services/vinculo.py:639`, `app/ponto/portas/celula.py:445`, `app/ponto/portas/celula.py:458`, `app/ponto/services/fechamento.py:1810`, `app/ponto/services/fechamento.py:1144`, `app/ponto/services/fechamento.py:1201`)
- **Nada — cron continua cadastrado às 05:50, cria faltantes e deixou de reescrever existentes de hoje/futuro; preserva editadas e ainda enriquece DNA histórico nulo.** Não há desligamento nesse caminho. (`app/config/crons.py:426`, `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:864`, `app/ponto/portas/celula.py:870`)
- **BLOQUEIA — a retirada alcançou também hoje:** feriado cadastrado para hoje é excluído pelo suplemento; o cron novo também pula a célula existente de hoje. Contra-exemplo: célula de 12/11 `trabalha=True`; salvar feriado de 12/11 não a cura por esses dois caminhos, enquanto o ramo removido do cron a reescrevia. (`app/ponto/services/cadastro_futuro.py:44`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`)

3. Escritores, leitores e significados conferidos:

| Campo/valor | Quem escreve ou produz | Quem lê |
|---|---|---|
| `escala_geradora` | Regeneração, edição humana, reversão e criação do cron; há também `SET_NULL` implícito ao excluir vínculo. (`app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:791`, `app/ponto/portas/celula.py:854`, `app/escala/models.py:1693`) | Juíza, carregadores, feriado e vigia. (`app/escala/alimentacao.py:270`, `app/escala/alimentacao.py:356`, `app/ponto/services/cadastro_futuro.py:53`, `app/ponto/services/vigia_cadastro_futuro.py:28`) |
| `dna` | Produtor `montar_dna`; persistência pela porta nas operações acima e no enriquecimento histórico. (`app/ponto/management/commands/gerar_celulas.py:73`, `app/ponto/portas/celula.py:256`, `app/ponto/portas/celula.py:865`) | Marcos, pausas e extra do vínculo; grade; cartório; juízes; tripwire; vigia. (`app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1235`, `app/escala/services/leitor_celula.py:398`, `app/ponto/services/cartorio.py:1251`, `app/chamados/juizes.py:948`, `app/ponto/management/commands/tripwire_celulas.py:30`, `app/ponto/services/vigia_cadastro_futuro.py:40`) |
| `trabalha` | Cálculo cadastral alimenta regeneração/cron; edição e reversão também escrevem. (`app/ponto/portas/celula.py:255`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:788`, `app/ponto/portas/celula.py:856`) | Método do vínculo, semana do posto, precedência, juízes e suprajuiz. (`app/escala/models.py:1095`, `app/escala/services/semana_do_posto.py:85`, `app/ponto/precedencia.py:348`, `app/chamados/juizes.py:455`, `app/ponto/supra_juiz.py:62`) |
| `origem` | Criação grava `gerada`; edição humana grava `editada`. (`app/ponto/portas/celula.py:260`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:856`) | Guardas da regeneração, reversão, cron, suplemento e exclusões protegidas. (`app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:263`, `app/ponto/portas/celula.py:777`, `app/ponto/portas/celula.py:859`, `app/ponto/services/cadastro_futuro.py:23`, `app/colaboradores/services/vinculo.py:990`) |
| `regenerada_em`, `regeneracoes`, `dna_anterior` | Regeneração e reversão; reversão consome e limpa o DNA anterior. (`app/ponto/portas/celula.py:273`, `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:792`) | Cobrança órfã, ponte do dia, cartório, invalidadores e comandos de correção/medição. (`app/escala/services/regeneracao.py:104`, `app/escala/services/regeneracao.py:119`, `app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/cartorio.py:393`, `app/chamados/veredito_lavrado.py:193`, `app/escala/services/fase_12x36.py:189`, `app/ponto/management/commands/aplicar_col900.py:173`, `bin/r5_idempotencia_frota.py:57`) |
| `FolgaDia`, `calendario_origem` | Foto automática grava calendário; lançamento manual deixa origem nula; reparentamento troca o vínculo. (`app/escala/folgas.py:194`, `app/escala/folgas.py:335`, `app/escala/folgas.py:360`, `app/escala/folgas.py:515`) | Reconciliação distingue manual/calendário; gerador lê foto do vínculo; precedência/grade aceitam manual da pessoa e calendário do vínculo do dia. (`app/escala/folgas.py:180`, `app/escala/folgas.py:187`, `app/escala/models.py:1286`, `app/escala/models.py:1356`, `app/ponto/precedencia.py:237`) |
| `vinculo_do_dia` | Resposta produzida pela autoridade existente; carregadores apenas montam alimentação. (`app/escala/alimentacao.py:228`, `app/escala/alimentacao.py:358`) | Feriado, vigia, reparentamento, calendário, grade e fechamento. (`app/ponto/services/cadastro_futuro.py:54`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/escala/folgas.py:493`, `app/colaboradores/services/calendario.py:389`, `app/escala/services/leitor_celula.py:411`, `app/ponto/services/fechamento.py:191`) |
| `hoje` | Cron usa `date.today`; serviços usam `timezone.localdate`; porta também possui leituras internas. (`app/ponto/management/commands/gerar_celulas.py:304`, `app/ponto/services/cadastro_futuro.py:16`, `app/escala/folgas.py:280`, `app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:296`) | Seleção futura, horizonte, julgamento e vigia. (`app/ponto/services/cadastro_futuro.py:18`, `app/ponto/portas/celula.py:190`, `app/ponto/portas/celula.py:431`, `app/ponto/services/vigia_cadastro_futuro.py:20`) |

**Nada — divergência nova de limites inclusivos:** início e fim são inclusivos nas duas autoridades; lâmpadas noturnas deslocam instantes para D+1, mantendo a chave do turno; feriado é consultado pela data recebida do dia de escala. (`app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/management/commands/gerar_celulas.py:67`, `app/escala/models.py:1395`)

**FILA — calendário nulo tem documentação ambígua:** a porta descreve `None` como padrão do template, mas o sincronizador lê a FK literal e o interpretador trata `None` como calendário vazio; a view resolve o padrão explicitamente. (`app/colaboradores/services/vinculo.py:441`, `app/escala/folgas.py:625`, `app/escala/folgas.py:52`, `app/colaboradores/views.py:236`)

**BLOQUEIA — nem todos usam o mesmo significado de cobertura:** geradora carimbada inativa sem fim é aceita por `vinculo_do_dia`, mas rejeitada por `vinculo_cobre_a_data` e pela regeneração. Esse desacordo atinge diretamente o contador novo. (`app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/portas/celula.py:146`)

4. Nas classes de reprovação:

- **Nada — segundo juiz novo no feriado/vigia:** ambos perguntam à autoridade registrada; o desacordo de cobertura acima é entre autoridades existentes. (`app/ponto/services/cadastro_futuro.py:54`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/core/juizes.py:1304`, `app/core/juizes.py:1305`)
- **Nada — derivação paralela nova de previsto/GRADE:** vigia compara trabalho/DNA pelos mesmos produtores usados pelo escritor; não calcula minutos previstos por uma regra nova. (`app/ponto/services/vigia_cadastro_futuro.py:39`, `app/ponto/portas/celula.py:255`)
- **FILA — segundo escritor implícito permanece:** excluir vínculo aciona `SET_NULL` fora da porta. As duas exclusões explícitas protegidas agora recusam referências editadas; referências geradas históricas continuam sujeitas ao comportamento anterior. (`app/escala/models.py:1693`, `app/colaboradores/services/vinculo.py:988`, `app/colaboradores/services/vinculo.py:992`, `app/colaboradores/services/vinculo.py:1064`)
- **FILA — reescrita sem denúncia permanece:** fora da vigência, o escritor altera apenas `trabalha/dna`, sem os três carimbos. Contra-exemplo: encerrar hoje uma geradora de amanhã sem sucessor apaga os marcos sem registrar essa regeneração nos campos que os leitores consultam; dívida anterior já documentada. (`app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:245`, `app/docs/LEIS.md:163`, `app/escala/services/regeneracao.py:119`)
- **FILA — fallbacks anteriores permanecem:** trabalho sem célula; marcos com DNA/marcos nulos mesmo havendo célula; pausas/extra sem célula; vínculo pela vigência quando não há geradora. Nada novo nesses fallbacks no diff fornecido. (`app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/models.py:1208`, `app/escala/models.py:1239`, `app/escala/alimentacao.py:285`)
- **FILA — relógio fora da borda:** os novos serviços possuem `localdate` próprio; rejulgamento e cauda releem o relógio apesar de receberem dias derivados de `hoje`. Django usa São Paulo; fuso do SO em produção: [nao verificado]. Contra-exemplo condicional: SO em UTC já está em 13/11, enquanto São Paulo ainda está em 12/11. (`app/ponto/services/cadastro_futuro.py:16`, `app/ponto/services/cadastro_futuro.py:42`, `app/escala/folgas.py:616`, `app/ponto/portas/celula.py:296`, `app/ponto/portas/celula.py:426`, `app/config/settings/base.py:93`)
- **BLOQUEIA — cura e cadastro não compartilham atomicidade em todas as portas:** vínculo sai do atomic antes da cura e engole falha; template salva antes da propagação e também engole falha; flag de feriado, município e formulário de feriado salvam antes da cura sem atomic abrangente próprio. Contra-exemplo: falha na regeneração após o save deixa cadastro novo e célula antiga, com sucesso possível nos caminhos que engolem a exceção. (`app/colaboradores/services/vinculo.py:188`, `app/colaboradores/services/vinculo.py:299`, `app/colaboradores/services/vinculo.py:331`, `app/escala/services/cadastro_tipo.py:306`, `app/escala/services/cadastro_tipo.py:322`, `app/colaboradores/services/vinculo.py:862`, `app/colaboradores/services/estrutura.py:68`, `app/core/views_config.py:68`)
- **Nada — nas portas com atomic abrangente, a escrita futura é síncrona:** calendário do vínculo, fechamento, snapshot, folgas e admin abrangem a cura. `on_commit` agenda efeitos posteriores, não a escrita futura do DNA. (`app/colaboradores/services/vinculo.py:450`, `app/colaboradores/services/vinculo.py:623`, `app/colaboradores/services/vinculo.py:984`, `app/escala/folgas.py:332`, `app/escala/services/cadastro_tipo.py:369`, `app/ponto/services/cadastro_futuro.py:21`, `app/ponto/portas/celula.py:502`)
- **BLOQUEIA — ordem inversa de travas:** snapshot trava célula C e depois exclui vínculo E; fechamento atualiza E e 

[truncado em 20000 de 24767 caracteres]

## 12/11 10:21 · sessao 1c8e308c2f1a · - · - · origem=Ronald (tmux)

BLOQUEIA — Há inversão de travas entre snapshot e fechamento: snapshot trava célula e depois exclui vínculo; fechamento atualiza vínculo e depois trava célula. (`app/colaboradores/services/vinculo.py:984`; `app/colaboradores/services/vinculo.py:988`; `app/colaboradores/services/vinculo.py:992`; `app/colaboradores/services/vinculo.py:623`; `app/colaboradores/services/vinculo.py:624`; `app/colaboradores/services/vinculo.py:638`; `app/ponto/services/cadastro_futuro.py:22`.)

Contraexemplo: célula C gerada, futura e dentro da janela, nomeando vínculo E que o snapshot removerá; T1 trava C; T2 fecha E e trava E; T1 tenta excluir E e espera T2; T2 tenta travar C e espera T1. A seleção futura admite C, e o snapshot só recusa antecipadamente referências editadas. (`app/ponto/services/cadastro_futuro.py:18`; `app/ponto/services/cadastro_futuro.py:23`; `app/colaboradores/services/vinculo.py:990`.) Execução e detecção efetiva do deadlock no banco: [nao verificado].

## 12/11 10:20 · sessao 3e264079821f · - · - · origem=Ronald (tmux)

BLOQUEIA — Encerramento legítimo deixa contador positivo após o Salvar: célula de 13/11, `trabalha=True`, geradora própria; encerre o vínculo em 12/11 sem sucessor. A porta grava `trabalha=False` e DNA sem marcos, mas mantém a geradora encerrada; `vinculo_do_dia=None` e contador de cobertura=1: `app/colaboradores/services/vinculo.py:637`, `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:245`, `app/escala/alimentacao.py:282`, `app/ponto/services/vigia_cadastro_futuro.py:36`.

Reproduzido no harness existente, sem banco nem escrita de arquivos. Repetir o evento não zera: a cura exige `trabalha is not False`, e a célula já foi apagada logicamente: `app/ponto/portas/celula.py:239`. O teste da porta aprova esse estado sem medir o contador: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:366`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:372`.

Isso mostra incompatibilidade entre o esperado zero e o estado que a porta preserva deliberadamente; a regra de conservar a célula sem marcos já está documentada no escritor: `app/ponto/portas/celula.py:232`.

## 12/11 10:20 · sessao 1c8e308c2f1a · - · - · origem=Ronald (tmux)

BLOQUEIA — Template não alcança vínculo inativo cuja vigência ainda cobre amanhã: `_propagar_regeneracao_template` filtra `ativa=True`, mas `vinculo_do_dia` aceita geradora inativa dentro das datas. Exemplo: hoje 12/11, EC inativa até 20/11, célula de 13/11; salvar horário 09:00 deixa DNA antigo. Reproduzido com o harness em memória: juíza respondeu EC 2, propagação retornou 0, célula teve 0 saves. (`app/escala/services/cadastro_tipo.py:334`; `app/escala/alimentacao.py:276`; `app/escala/alimentacao.py:282`; harness `app/ponto/tests/test_cadastro_futuro_sem_banco.py:361`.)

BLOQUEIA — Contador de cobertura dá falso zero para geradora inativa sem `data_fim`: a cobertura real retorna falso, mas `vinculo_do_dia` aceita a geradora nomeada porque só compara datas nesse ramo; o vigia conta apenas `None`/PK diferente. Exemplo reproduzido em memória: EC inativa, início anterior a amanhã, fim nulo, célula de amanhã nomeando essa EC → `vinculo_cobre_a_data=False`, `vinculo_do_dia=EC`, contador de cobertura 0. A regeneração também recusa esse EC. (`app/escala/alimentacao.py:196`; `app/escala/alimentacao.py:214`; `app/escala/alimentacao.py:282`; `app/ponto/services/vigia_cadastro_futuro.py:31`; `app/ponto/services/vigia_cadastro_futuro.py:36`; `app/ponto/portas/celula.py:146`.)

BLOQUEIA — Cadastro e cura futura não formam uma transação em várias portas: `executar_vinculo` termina o atomic antes da regeneração e engole a exceção; `salvar_tipo` salva antes da propagação e também engole a exceção; flag de feriado e município salvam antes do suplemento, sem atomic próprio abrangente. Contraexemplo: regeneração falha depois do commit do cadastro → cadastro novo, célula futura antiga, resposta de sucesso possível nas portas que engolem. (`app/colaboradores/services/vinculo.py:188`; `app/colaboradores/services/vinculo.py:299`; `app/colaboradores/services/vinculo.py:326`; `app/colaboradores/services/vinculo.py:331`; `app/escala/services/cadastro_tipo.py:306`; `app/escala/services/cadastro_tipo.py:320`; `app/escala/services/cadastro_tipo.py:322`; `app/colaboradores/services/vinculo.py:862`; `app/colaboradores/services/vinculo.py:878`; `app/colaboradores/services/estrutura.py:68`.)

FILA — `@transaction.atomic` na parte superior de cadastro_tipo decora `_hpd_pares`, não `salvar_tipo`; não comprova atomicidade do Salvar. (`app/escala/services/cadastro_tipo.py:41`; `app/escala/services/cadastro_tipo.py:43`; `app/escala/services/cadastro_tipo.py:140`.)

Mapa de alcance, sempre helper → porta existente em `app/ponto/services/cadastro_futuro.py:30`:

| Porta | Sítio futuro |
|---|---|
| Escala/vínculo | `app/colaboradores/services/vinculo.py:324` |
| Trabalha em feriado | `app/colaboradores/services/vinculo.py:877` |
| Município | `app/colaboradores/services/vinculo.py:927` |
| Template | `app/escala/services/cadastro_tipo.py:360` |
| Admin de template | `app/escala/services/cadastro_tipo.py:367` |
| Signal vigência | `app/escala/signals.py:77` |
| Signal âncora | `app/escala/signals.py:128` |
| Foto/lançar/alterar/lote/reparentar/aplicar calendário | `app/escala/folgas.py:298`; chamadores `:215`, `:337`, `:380`, `:418`, `:523`; `app/escala/views.py:416` |
| Editor calendário | `app/escala/folgas.py:598` |
| Troca calendário vínculo | `app/colaboradores/services/vinculo.py:454` |
| Feriado | `app/ponto/services/cadastro_futuro.py:54` |
| Fechar vigência | `app/colaboradores/services/vinculo.py:637` |
| Snapshot | `app/colaboradores/services/vinculo.py:984` |
| Correção retroativa | `app/colaboradores/management/commands/corrigir_escala_retroativa.py:242` |

FILA — Não há RED novo individual para lançar/alterar/lote/reparentar/aplicar calendário; o teste cobre diretamente a função comum `relavrar_celulas_da_foto`. Tampouco há RED novo de aplicação/desfazimento completo AUTO. (`app/ponto/tests/test_cadastro_futuro_sem_banco.py:584`; `app/ponto/tests/test_cadastro_futuro_sem_banco.py:466`.)

FILA — AUTO continua criando faltantes além da janela corrente: `_ate` usa última célula, no mínimo início; aplicação/desfazimento chamam regeneração com horizonte habilitado. Exemplo: início depois da janela sem célula alguma → cria a célula inicial. Esse comportamento já consta do censo anterior fornecido. (`app/escala/services/escala_auto_executor.py:52`; `app/escala/services/escala_auto_executor.py:104`; `app/escala/services/escala_auto_executor.py:143`; `app/ponto/portas/celula.py:259`.)

Nada de ampliação passada identificado nos recortes alterados: folgas antes percorriam desde..máximo(ate,fim competência), agora desde..hoje; o suplemento seleciona somente `d>hoje`. (`app/escala/folgas.py:293`; `app/ponto/portas/celula.py:187`; `app/ponto/services/cadastro_futuro.py:18`.)

Nada de novo juiz de vínculo no suplemento de feriado: consulta `vinculo_do_dia`, preservando silêncio da autoridade. (`app/ponto/services/cadastro_futuro.py:54`.)

Nada de criação futura pelo suplemento fora das células existentes: seleciona geradas existentes e chama `[dia,dia]` sem horizonte; editadas ficam fora. (`app/ponto/services/cadastro_futuro.py:22`; `app/ponto/services/cadastro_futuro.py:30`.)

FILA — `None` no calendário tem sentidos documentais divergentes: porta diz “padrão do template”, mas sincronização usa FK literal e o interpretador transforma `None` em conjunto vazio. A view resolve explicitamente o padrão antes da chamada; não há herança dinâmica no interpretador. (`app/colaboradores/services/vinculo.py:441`; `app/escala/folgas.py:625`; `app/escala/folgas.py:52`; `app/colaboradores/views.py:236`.)

Nada de falta de regeneração por mudar `Posto.opera_em_feriado` comprovada: o campo decide o padrão somente de vínculos novos, explicitamente; a porta só salva esse campo. Troca de `Colaborador.posto` também não muda o posto já declarado no vínculo. (`app/escala/models.py:905`; `app/escala/models.py:910`; `app/colaboradores/services/estrutura.py:80`; `app/colaboradores/services/colaborador.py:47`.)

## 12/11 10:20 · sessao 1406670eefaa · - · - · origem=Ronald (tmux)

BLOQUEIA — contador novo não equivale a “geradora cobre o dia”: `vinculo_do_dia` aceita geradora carimbada `ativa=False,data_fim=None`, ou com `data_inicio=None`; `vinculo_cobre_a_data` rejeita esses casos. Reproduzi o primeiro com AST puro: cobertura=False; vínculo do dia=pk1. O vigia pergunta à função real, porém conta zero cobertura nesse caso. `app/escala/alimentacao.py:196`, `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:279`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/services/vigia_cadastro_futuro.py:36`.

BLOQUEIA — hoje perde cura do cron e não recebe suplemento de feriado: feriado salvo para 12/11 é descartado pelo filtro estritamente futuro; cron agora também preserva a célula existente de 12/11, diferentemente do diff-base fornecido. Contraexemplo: célula hoje trabalha=True; salva feriado hoje; segue True pelos dois caminhos. `app/ponto/services/cadastro_futuro.py:44`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`.

FILA — relógio fora da borda e dois referenciais possíveis: novos helpers leem `timezone.localdate`; cron/porta ainda usam `date.today`; `rejulgar` relê relógio apesar de `hoje` injetado. Configuração Django é São Paulo, USE_TZ=True; fuso do SO em produção [nao verificado]. Contraexemplo condicional: SO UTC em 13/11 01h, São Paulo ainda 12/11; suplemento trata 13/11 como futuro, cron como hoje. `app/ponto/services/cadastro_futuro.py:16`, `app/ponto/services/cadastro_futuro.py:42`, `app/escala/folgas.py:280`, `app/escala/folgas.py:616`, `app/ponto/management/commands/gerar_celulas.py:304`, `app/ponto/portas/celula.py:186`, `app/ponto/portas/celula.py:296`, `app/ponto/portas/celula.py:426`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`.

FILA — ramo fora da vigência grava trabalha/dna sem `dna_anterior/regenerada_em/regeneracoes`; dívida anterior documentada, agora também acionada pelo suplemento futuro. Contraexemplo: encerra hoje geradora de amanhã, sem sucessor; amanhã perde marcos sem os carimbos que consumidores usam para identificar reescrita. `app/ponto/portas/celula.py:238`, `app/ponto/portas/celula.py:243`, `app/docs/LEIS.md:163`, `app/escala/services/regeneracao.py:119`, `app/ponto/services/cartorio.py:393`.

FILA — escritor implícito continua: FK da geradora é SET_NULL; delete de EC modifica a célula fora da porta. Proteção nova recusa editadas no snapshot e no desfazer; células geradas históricas continuam sujeitas ao SET_NULL preexistente. `app/escala/models.py:1693`, `app/colaboradores/services/vinculo.py:988`, `app/colaboradores/services/vinculo.py:990`, `app/colaboradores/services/vinculo.py:992`, `app/colaboradores/services/vinculo.py:1062`, `app/colaboradores/services/vinculo.py:1064`.

Nada — segundo juiz novo nos dois serviços: feriado e vigia chamam `vinculo_do_dia`; registro vivo já nomeia essa função e a cobertura separadamente. Nada — derivação paralela nova de minutos previstos/grade no diff: comparação do vigia usa os mesmos produtores de trabalho/DNA que a regeneração. `app/ponto/services/cadastro_futuro.py:54`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/core/juizes.py:1304`, `app/core/juizes.py:1305`, `app/ponto/services/vigia_cadastro_futuro.py:39`, `app/ponto/portas/celula.py:255`.

Inventário dos campos, com complementos enviados à raiz:

| Campo | Escritores | Leitores principais |
|---|---|---|
| geradora/dna/trabalha/origem | regeneração cria/reescreve; edição humana; reversão; cron cria e enriquece DNA histórico nulo. `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:272`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:603`, `app/ponto/portas/celula.py:786`, `app/ponto/portas/celula.py:854`, `app/ponto/portas/celula.py:865`. | juíza; métodos EC; grade; semana; vigia. `app/escala/alimentacao.py:270`, `app/escala/models.py:1095`, `app/escala/models.py:1124`, `app/escala/models.py:1189`, `app/escala/models.py:1235`, `app/escala/services/leitor_celula.py:398`, `app/escala/services/leitor_celula.py:411`, `app/escala/services/semana_do_posto.py:85`, `app/ponto/services/vigia_cadastro_futuro.py:40`. |
| regenerada_em/regeneracoes/dna_anterior | regeneração e reversão. `app/ponto/portas/celula.py:273`, `app/ponto/portas/celula.py:787`, `app/ponto/portas/celula.py:792`. | cobrança órfã/ponte do dia, cartório, invalidadores, comandos. `app/escala/services/regeneracao.py:72`, `app/escala/services/regeneracao.py:104`, `app/ponto/services/dia_do_colab.py:94`, `app/ponto/services/cartorio.py:393`, `app/chamados/veredito_lavrado.py:193`, `app/escala/services/fase_12x36.py:189`, `app/chamados/management/commands/materializar_passivo_validado.py:220`, `app/ponto/management/commands/aplicar_col900.py:173`, `bin/r5_idempotencia_frota.py:57`. |
| FolgaDia/calendario_origem | foto automática; manual; reparentamento. `app/escala/folgas.py:180`, `app/escala/folgas.py:194`, `app/escala/folgas.py:335`, `app/escala/folgas.py:360`, `app/escala/folgas.py:515`. | calendário/manual discriminados pelo nulo; gerador usa foto mensal do vínculo; precedência aceita manual da pessoa e calendário do vínculo do dia. `app/escala/folgas.py:103`, `app/escala/folgas.py:330`, `app/escala/folgas.py:351`, `app/escala/folgas.py:484`, `app/escala/views_plano_folgas.py:113`, `app/ponto/services/declarar_dia.py:115`, `app/escala/models.py:1286`, `app/escala/models.py:1310`, `app/escala/models.py:1356`, `app/ponto/precedencia.py:237`. |

FILA — fallbacks anteriores permanecem: trabalho sem célula; marcos com DNA/marcos nulos mesmo havendo célula; pausa/extra somente sem célula; vínculo por vigência somente sem geradora. Nenhum desses foi introduzido no diff fornecido. `app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/models.py:1208`, `app/escala/models.py:1239`, `app/escala/alimentacao.py:285`.

Nada — inclusividade nova divergente: cobertura e geradora usam limites inclusivos; lâmpadas deslocam marcos noturnos para D+1, preservando a chave do turno; feriado usa a data recebida do dia de escala. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/management/commands/gerar_celulas.py:67`, `app/escala/models.py:1369`, `app/escala/models.py:1395`.

O varredor puro encontrou nove operações atribuídas a CelulaDia, todas dentro da porta, além do SET_NULL explicitado acima; atribuição dinâmica não é certificada por esse censo [nao verificado]. `app/ponto/portas/celula.py:245`, `app/ponto/portas/celula.py:259`, `app/ponto/portas/celula.py:276`, `app/ponto/portas/celula.py:600`, `app/ponto/portas/celula.py:609`, `app/ponto/portas/celula.py:794`, `app/ponto/portas/celula.py:854`, `app/ponto/portas/celula.py:866`, `app/ponto/portas/celula.py:1106`.

## 12/11 10:19 · sessao 3e264079821f · - · - · origem=Ronald (tmux)

BLOQUEIA — A suíte pura não está verde: 43 testes executados, 42 passaram e `test_feriado_pergunta_a_juiza_e_respeita_o_silencio` deu ERROR em `copy.deepcopy(vars(self.cel))`, antes de chamar a porta. O dublê `EC` exige argumentos na reconstrução: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:206`. O mutante `feriado_toma_geradora` repete o mesmo ERROR; esse RED não demonstra mordida diferencial: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:316`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:599`.

BLOQUEIA — O contador com nome “geradora que não cobre o dia” mede a resposta de `vinculo_do_dia`, cujo ramo com geradora diverge da autoridade de cobertura: geradora `ativa=False`, `data_fim=None`, início anterior ao dia retorna `False` em `vinculo_cobre_a_data`, mas é aceita por `vinculo_do_dia`; o vigia devolve ambos os contadores zero se DNA/trabalho coincidirem. Reproduzido em memória usando as funções reais e dublês existentes: `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:282`, `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/services/vigia_cadastro_futuro.py:36`. Caso concreto: célula 13/11 nomeia EC iniciada 01/11, inativa sem fim; há geradora sem vigência declarada, mas contador de cobertura = 0: `app/escala/alimentacao.py:196`, `app/ponto/services/vigia_cadastro_futuro.py:37`.

FILA — Universo da métrica inclui **hoje**, não só `d > hoje`: células existentes `origem='gerada'`, no intervalo recebido, com `data >= hoje`; filtro opcional por colaborador. Unidade = uma célula; editadas excluídas, sem geradora separadas em `sem_geradora`, sem entrar no contador de cobertura: `app/ponto/services/vigia_cadastro_futuro.py:20`, `app/ponto/services/vigia_cadastro_futuro.py:27`, `app/ponto/services/vigia_cadastro_futuro.py:34`. Contra-exemplo: célula inválida de hoje incrementa “celula_futura…”: `app/ponto/services/vigia_cadastro_futuro.py:20`.

FILA — `MUTANTE_CADASTRO` existe apenas no módulo de teste, mas não é somente mudança de expectativa: o carregador lê e modifica AST/fonte das funções reais da produção e executa essas cópias em memória, publicadas nos módulos dublês. Não altera arquivo nem habilita mutante na produção instalada: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:25`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:67`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:92`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:305`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:320`. Os decoradores são removidos e `atomic`/`on_commit` são dublês sem transação; os testes não comprovam ordem transacional real: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:31`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:258`.

Mordidas verificadas em execução em memória:

| Porta/mutante | Resultado | RED |
|---|---|---|
| `executar_vinculo`, guarda `data_inicio <= hoje` | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:339` |
| `marcar_trabalha_em_feriado`, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:352` |
| município, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:356` |
| propagação template, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:361` |
| correção retroativa `_aplicar`, mesma guarda | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:517` |
| `fechar_vigencia`, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:366` |
| trocar calendário, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:497` |
| salvar calendário, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:509` |
| snapshot, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:448` |
| signal âncora, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:569` |
| signal vigência, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:576` |
| relavra foto, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584` |
| feriado save/delete, remoção suplemento | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:429`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:435` |
| esquecer data anterior feriado | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:529` |
| feriado escolher EC ativa sobreposta | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:590` |
| vigia tomar geradora sem perguntar | FAIL esperado | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:688` |
| foto sem recorte / sem relavra mensal / somente dias com célula | FAIL esperado em cada caso | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:538`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:612`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:625` |
| suplemento com `estender_horizonte=True` | FAIL; cria indevidamente 14–20/11 | `app/ponto/tests/test_cadastro_futuro_sem_banco.py:386`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:584` |

A implementação do mutador coloca a guarda somente nos cinco nomes indicados; nas demais portas ele remove a chamada. Assim não afirmar “guarda <= hoje voltou” em todos os casos: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:73`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:80`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:87`.

RED específico “vigência amanhã + guarda <= hoje” para `lancar_folga`, `alterar_folga`, lote, reparentamento, aplicação coletiva de calendário e executor automático: não demonstrado pela nova suíte. Existem testes anteriores, mas os examinados exercitam começo da competência/FK/foto/cadastro, sem provar a troca de geradora futura sob aquele mutante: `app/escala/tests/test_bug136_plano_em_lote_relavra.py:68`, `app/escala/tests/test_bug136_plano_em_lote_relavra.py:105`, `app/escala/tests/test_folga_aplicar.py:39`, `app/escala/tests/test_escala_auto_executor.py:66`, `app/escala/tests/test_escala_auto_executor.py:117`. Execução desses testes Django: [nao verificado].

Contador/cron/copiloto: `handle` mede antes de gerar e grava os dois snapshots na mesma transação do comando; API lê o último snapshot da frota e distingue ausência de medida de zero: `app/ponto/management/commands/gerar_celulas.py:310`, `app/ponto/services/vigia_cadastro_futuro.py:47`, `app/ponto/services/vigia_cadastro_futuro.py:63`, `app/api/views_mensageria.py:647`. O endpoint está exposto em `/api/mensageria/saude/`: `app/api/urls.py:16`. Consumo efetivo pelo copiloto/HAIKU em produção: [nao verificado].

Caso mordente do contador: geradora velha termina hoje e célula amanhã ainda a nomeia; teste cobra cobertura=1, nenhuma escrita; após apontar geradora nova com DNA velho cobra divergência cadastral=1: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:243`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:247`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:658`. Ele chama autoridades reais, sem réplica local do julgamento: `app/ponto/services/vigia_cadastro_futuro.py:31`, `app/ponto/services/vigia_cadastro_futuro.py:39`.

Cron preservado às 05:50, continua criando faltantes, preservando editadas e enriquecendo DNA nulo do passado, e agora pula existentes de hoje/futuro: `app/config/crons.py:426`, `app/ponto/portas/celula.py:853`, `app/ponto/portas/celula.py:859`, `app/ponto/portas/celula.py:861`, `app/ponto/portas/celula.py:870`. Os testes puros de criação, editada e cron sem reescrita passaram: `app/ponto/tests/test_cadastro_futuro_sem_banco.py:797`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:803`, `app/ponto/tests/test_cadastro_futuro_sem_banco.py:813`.

## 12/11 10:09 · sessao 86c3a6ecfcf4 · BUG-CADASTRO-NAO-ALCANCA-O-FUTURO · construtor · origem=Code

1. **Censo:** feriado e vigia agora perguntam a `vinculo_do_dia`. O vigia compara diretamente o carimbo com a resposta; a variável `geradora` apenas alimenta a juíza (`ponto/services/cadastro_futuro.py:54`, `ponto/services/vigia_cadastro_futuro.py:31`).

2. **Folgas:** a sincronização futura reutiliza `_gerar_foto_folgas_no_ato`, com recorte de datas. A reconciliação mensal existente mantém sua relavra (`escala/folgas.py:166`, `escala/folgas.py:637`).

3. **Feriado relâmpago:** caracteriza cron preservando a divergência e o formulário real `form_feriado` corrigindo no Salvar (`escala/tests/test_celula_dia.py:373`).

4. **Stale:** cobra contador positivo após o cron e zero após `salvar_tipo` (`escala/tests/test_celula_dia.py:328`).

5. **DNA:** cobra futuro atualizado pela porta, cron sem reescrita e DNA histórico preservado (`escala/tests/test_celula_dia.py:146`).

No fluxo alterado:

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `CelulaDia.escala_geradora` | Porta da célula (`ponto/portas/celula.py:272`) | Juíza e vigia (`escala/alimentacao.py:269`, `ponto/services/vigia_cadastro_futuro.py:36`) |
| `dna` | Porta, usando `montar_dna` (`ponto/portas/celula.py:256`) | Vigia (`ponto/services/vigia_cadastro_futuro.py:40`) |
| `trabalha` | Porta (`ponto/portas/celula.py:272`) | Vigia (`ponto/services/vigia_cadastro_futuro.py:40`) |
| `FolgaDia.calendario_origem` | Escritor existente (`escala/folgas.py:195`) | Escritor e juiz da folga (`escala/folgas.py:187`, `ponto/precedencia.py:231`) |

Comandos para a sessão. **Testes, RED na base e morte dos mutantes: [não verificado]**; não os executei, conforme solicitado. [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
C2=ponto.tests.test_cadastro_futuro_sem_banco

python3 manage.py test \
  escala.tests.test_contract_juiz_escala \
  ponto.tests.test_catraca_folga_lancada_um_juiz \
  escala.tests.test_celula_dia

python3 -m unittest "$C2"
```

Mutantes em memória e seus testes mordentes (`ponto/tests/test_cadastro_futuro_sem_banco.py:34`, `:309`, `:647`):

```bash
MUTANTE_CADASTRO=feriado_toma_geradora python3 -m unittest "${C2}.EventosFuturosTest.test_feriado_pergunta_a_juiza_e_respeita_o_silencio"

MUTANTE_CADASTRO=feriado_escolhe_outro python3 -m unittest "${C2}.EventosFuturosTest.test_feriado_nao_escolhe_outra_geradora_sobreposta"

MUTANTE_CADASTRO=vigia_toma_geradora python3 -m unittest "${C2}.VigiaTest.test_vigia_compara_carimbo_com_resposta_da_juiza"

MUTANTE_CADASTRO=foto_no_ato_sem_recorte python3 -m unittest "${C2}.EventosFuturosTest.test_calendario_preserva_foto_manual_e_passado"

MUTANTE_CADASTRO=foto_no_ato_sem_relavra_mensal python3 -m unittest "${C2}.EventosFuturosTest.test_foto_mensal_continua_relavrando_e_poupando_manual"

MUTANTE_CADASTRO=foto_so_dias_com_celula python3 -m unittest "${C2}.EventosFuturosTest.test_foto_mensal_inclui_folga_sem_celula_fora_da_janela"

MUTANTE_CADASTRO=_propagar_regeneracao_template python3 -m unittest "${C2}.EventosFuturosTest.test_propagar_regeneracao_template"
```

## 12/11 10:08 · sessao c626fa8aa4e0 · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**BLOQUEIA o pouso:** permanecem duas fontes para órfãs, regra própria no PDF e um ramo que impede a recusa nomeada (`app/relatorios/pdf_espelho.py:474`, `:517`; `app/ponto/services/espelho.py:522`, `:563`). O par financeiro obrigatório da competência 11 está **[nao verificado]**.

1. Classificação dos dez:

| Nº | Classe | Conferência |
|---|---|---|
| 1 | **H correta** | Harness substitui também atributos dos pacotes carregados; conserva contador=1 e ausência de `explicados`. Retirar a correção em memória produz falha de busca proibida. Nada. `app/ponto/tests/test_lastro_c12_sem_banco.py:89`, `:103`; `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. |
| 2 | **H correta** | Conserva posto presente, buracos=1 e explicados=0; reversão do harness falha por grade proibida. Nada. `app/ponto/tests/test_lastro_c12_sem_banco.py:109`; `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. |
| 3 | **H correta** | Conserva recusa nomeada e vizinho reconhecido explicado; reversão do harness falha por busca proibida. Nada. `app/ponto/tests/test_lastro_c12_sem_banco.py:141`; `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. |
| 4 | **C correta** | A inversão exige recusa nomeada, `sem_certidao` e veredito vazio: corresponde à leitura sem concluir precedência do aval 4; não virou sucesso genérico. Nada. `app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`. Execução Django/ORM: **[nao verificado]**. |
| 5 | **C correta** | `licenca especial` normaliza para `licenca_especial`, fora do mapa; o sucesso agora usa `reciclagem`, já mapeado, e verifica palavra. O par exige recusa nomeada sem gravação: corresponde ao aval 5. Nada. `app/ponto/views_tipos_ausencia.py:92`; `app/ponto/precedencia.py:70`; `app/ponto/tests/test_tela_tipos.py:29`, `:51`. Execução HTTP/ORM: **[nao verificado]**. |
| 6 | **R correta** | A chamada do contrato voltou; o selo original passou adaptado sem banco. Ressalvas de produto no item 3 abaixo. `app/relatorios/pdf_espelho.py:474`; `app/ponto/tests/test_lastro_c13_sem_banco.py:130`. |
| 7 | **R correta** | Mesmo resultado: contrato restaurado, selo original preservado. Isso não prova ausência de regras próprias. `app/relatorios/tests/test_cartao_pela_celula.py:176`; `app/ponto/tests/test_lastro_c13_sem_banco.py:130`. |
| 8 | **R correta quanto ao sintoma** | A expectativa original continua `orfa=True`; a costura restaurada exige isso, mas preserva divergência com a tela, abaixo. `app/relatorios/tests/test_cartao_marca_espuria.py:59`; `app/ponto/tests/test_lastro_c11_sem_banco.py:383`. |
| 9 | **R correta quanto ao sintoma** | A expectativa original continua zero turno aberto por espúria; costura passou. Prova nativa do PDF binário: **[nao verificado]**. `app/relatorios/tests/test_cartao_marca_espuria.py:75`, `:84`; `app/ponto/tests/test_lastro_c11_sem_banco.py:385`. |
| 10 | **H compatível com o delta** | O registro atual aponta a consulta de feriados, e a igualdade estrita permanece. Recolocar `[1032]` em memória causa falha, sem erro de harness. Nada no instrumento atual. `app/ponto/services/espelho.py:1039`; `app/core/tests/test_medida_turno_do_par.py:542`. Identidade AST histórica c9/c12: **[nao verificado]**. |

2. Escritores, leitores e `None`:

- **Nada — escritor do dict:** A2 escreve `recusas[data]`; sem destinatário, levanta. O rótulo nasce na exceção. `app/ponto/precedencia.py:423`, `:422`, `:102`.
- **Nada — transporte:** `_marcar_ausencias` escreve `dia['recusa']` e `tipo_dia=None`; PDF lê a recusa e entrega ao compositor, que escreve palavra nomeada e veredito vazio. `app/escala/utils.py:1219`, `:1220`; `app/relatorios/pdf_espelho.py:567`; `app/ponto/services/espelho.py:339`.
- **Nada — leitores internos alcançados:** legacy preserva `None`; isenção não concede flag de dia de trabalho para esse tipo. `app/escala/utils.py:1046`, `:889`.
- **FILA — incompatibilidade latente:** `classificar_orfas({'tipo_dia': None, 'orfas': ['16:19']})` classifica como `FOLGA_DESMENTIDA`; `por_dia_da_grade` descarta esse tipo sem `ausencia_tipo`. São interpretações incompatíveis com “precedência recusada”. `app/escala/orfa_classe.py:65`; `app/ponto/services/dia_pago.py:392`. Alcance desses leitores pelo canal novo do PDF: **[nao verificado]**.
- **Nada por inspeção do delta com `recusas=None`:** retorno antecipado, condição de consulta A2 e chamada sem argumento conservam os ramos anteriores; o ramo novo fica desativado. `app/escala/utils.py:1205`, `:1213`, `:1218`, `:2114`, `:2117`, `:2365`. Equivalência dinâmica em todo chamador: **[nao verificado]**.
- **Unidades dos contadores:** cartório conta colaboradores isolados; `tipos_ativos_sem_faixa` conta códigos ativos do cadastro, inclusive sem lançamento. Dois colaboradores com um código representam 2 no primeiro universo e 1 no segundo. `app/ponto/management/commands/processar_cartorio.py:73`; `app/ponto/services/vigia_ausencia.py:155`, `:168`.

3. PDF, contrato e igualdade com a tela:

- **BLOQUEIA — duas fontes para a mesma órfã.** PDF reconstrói pelo contrato; tela lê a célula na competência. Contra-exemplo já fixado pelo teste alterado: tela tem `orfas_grade=[]`, enquanto PDF marca a batida como órfã e conta zero aberto. A igualdade `pdf['dias']==dias` compara a lista compartilhada que o próprio coletor modifica. `app/relatorios/pdf_espelho.py:474`, `:571`; `app/ponto/services/espelho.py:522`; `app/ponto/tests/test_lastro_c11_sem_banco.py:326`, `:382`, `:383`, `:384`.

- **BLOQUEIA — regra própria de turno aberto permanece.** PDF une órfãs de todas as datas por HH:MM e usa esse conjunto para excluir abertos. Contra-exemplo executado em memória: órfã em **10/11 07:02** elimina um aberto legítimo de **11/11 07:02**, mesmo sem órfã nesse segundo dia; a tela conta esse aberto. Regra herdada, ainda presente: `app/relatorios/pdf_espelho.py:174`, `:480`, `:517`; `app/ponto/services/espelho.py:436`. Contraria a testemunha sem regra própria: `CLAUDE.md:20`.

- **BLOQUEIA — rótulo não garantido no ramo de ata agregada.** O handler chama `_pela_grade()` sem `recusas`; uma ausência aprovada `fantasma_xyz` levanta `TipoSemFaixa` antes do transporte nomeado. PDF também passa por essa tela depois de obter sua grade. Contra-exemplo: ata agregada que exige o builder + ausência aprovada sem faixa. `app/ponto/services/espelho.py:495`, `:563`, `:653`; `app/relatorios/pdf_espelho.py:479`. Ocorrência produtiva: **[nao verificado]**.

- **Nada no canal coberto do rótulo:** tela e PDF transportam o mesmo `TipoSemFaixa.rotulo` ao mesmo compositor; PDF imprime a palavra resultante. `app/ponto/precedencia.py:102`; `app/ponto/services/espelho.py:880`, `:339`; `app/relatorios/pdf_espelho.py:567`, `:886`.

4. Classes de reprovação:

- **BLOQUEIA — juiz/derivação paralela:** materializações distintas de órfãs e exclusão própria de turno aberto, demonstradas acima. `app/ponto/services/espelho.py:522`; `app/relatorios/pdf_espelho.py:474`, `:517`.
- **BLOQUEIA — selo insuficiente para igualdade:** o teste aceita explicitamente respostas diferentes para a órfã; portanto verde não certifica “cartão=espelho”. `app/ponto/tests/test_lastro_c11_sem_banco.py:383`, `:384`.
- **Segundo escritor fora da porta: nada no delta inspecionado.** A grade copia a recusa da A2; cadastro continua validando antes de gravar. `app/escala/utils.py:1219`; `app/ponto/services/ausencia.py:1312`.
- **Relógio novo fora da borda: nada no delta inspecionado.** Contrato usa o `agora` recebido. `app/escala/utils.py:2359`, `:2364`.
- **Campo com dois significados:** FILA latente descrita no item 2; divergência alcançável causada pelo novo `None`: **[nao verificado]**.

5. Dinheiro:

- **Nada de caminho demonstrado recebendo o novo `None`:** `simular_folha` passa pelo fechamento; fechamento lê `grade_do_fechamento`, cuja fonte é `grade_da_celula`. `app/folha/management/commands/simular_folha.py:122`, `:130`; `app/ponto/services/fechamento.py:1015`, `:1048`; `app/folha/export.py:335`.
- **BLOQUEIA pouso — condição financeira pendente:** par `simular_folha` da competência 11, efeito `abona=0` e diferenças por rubrica: **[nao verificado]**, conforme o dado fornecido. Ausência de movimento financeiro: **[nao verificado]**. [nao verificado]

6. Fora dos dez e do mínimo:

- **Nada identificado no delta fornecido:** alterações produtivas concentram-se no transporte da recusa e na leitura do contrato pelo PDF; testes auxiliares acompanham esses caminhos. `app/escala/utils.py:1159`, `:2350`; `app/relatorios/pdf_espelho.py:474`; `app/ponto/tests/test_lastro_c13_sem_banco.py:177`. Comparação histórica integral da cópia com `86726416c`: **[nao verificado]**.
- **Limite da verificação:** passaram 72 testes sem banco e 35 da catraca; os mutantes executados ficaram vermelhos. Os builders da costura são reduzidos às chamadas extraídas, sem geometria/ORM completos. `app/ponto/tests/test_lastro_c13_sem_banco.py:73`, `:196`, `:200`; `app/core/tests/test_medida_turno_do_par.py:525`. Suite integral, HTTP/ORM e PDF binário: **[nao verificado]**.

## 12/11 10:06 · sessao 0ab2e5092d0c · - · - · origem=Ronald (tmux)

1–3 H: corretas. O defeito é o atributo do pacote já carregado: API importa pelos pais (`app/api/views_mensageria.py:2077`, `:2100`); o harness agora substitui ambos e restaura no cleanup (`app/ponto/tests/test_lastro_c12_sem_banco.py:89`). Asserções continuam exigindo 1 buraco, zero explicados e vizinhos preservados (`:103`, `:109`, `:141`). Reverti somente esse bloco em AST: três falhas de busca/grade proibidas, zero erros; a fixture adversarial está em `app/ponto/tests/test_lastro_c13_sem_banco.py:92`. Achado: nada.

4 C: correta por inspeção; a inversão exige recusa nomeada, `sem_certidao` e veredito vazio (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:59`). O calendário já recebe a recusa A2 e transporta seu rótulo (`app/colaboradores/services/calendario.py:510`, `:607`, `:714`). Não apagou o caso nem substituiu por sucesso genérico (`app/colaboradores/tests/test_calendario_ausencia_pela_autoridade.py:52`). Execução Django/ORM: [nao verificado]. Achado: nada.

5 C: correta. `licenca especial` vira `licenca_especial` pela view (`app/ponto/views_tipos_ausencia.py:92`); esse código não consta do mapa (`app/ponto/precedencia.py:52`). O novo sucesso cria `reciclagem`, cuja faixa já existe (`app/ponto/tests/test_tela_tipos.py:31`; `app/ponto/precedencia.py:70`), verifica crescimento e palavra pelo compositor (`app/ponto/tests/test_tela_tipos.py:37`, `:47`). O par exige não gravar e erro nomeado (`:51`). A porta consulta o juiz antes de gravar (`app/ponto/services/ausencia.py:1312`). Execução HTTP/ORM: [nao verificado]. Achado: nada.

6–7 R: corretas quanto à perda/restauração do contrato; os selos originais exigem o contrato (`app/escala/tests/test_f1c_contrato.py:70`; `app/relatorios/tests/test_cartao_pela_celula.py:176`) e o coletor voltou a chamá-lo (`app/relatorios/pdf_espelho.py:474`). A nova prova executa as mesmas funções dos selos (`app/ponto/tests/test_lastro_c13_sem_banco.py:130`). Achado dos testes: nada; isso sozinho não certifica ausência de regras próprias no restante do PDF (`app/escala/tests/test_f1c_contrato.py:68`).

8–9 R: corretas quanto às expectativas preservadas: espúria deve ser marcada e não produzir turno aberto (`app/relatorios/tests/test_cartao_marca_espuria.py:59`, `:75`). O substituto sem banco agora exige `orfa=True`/zero aberto (`app/ponto/tests/test_lastro_c11_sem_banco.py:384`); suprimir as órfãs restauradas é o mutante (`:68`) e ficou vermelho nesta reexecução. O teste nativo também exige o texto e a batida no PDF binário (`app/relatorios/tests/test_cartao_marca_espuria.py:84`); essa execução está [nao verificado]. Achado dos testes: nada.

10 H: atualização do registro preserva a igualdade estrita (`app/core/tests/test_medida_turno_do_par.py:542`); a ocorrência atual é a consulta de feriados em `app/ponto/services/espelho.py:1039`. Rodei os 35 testes existentes: verdes; recolocar somente `[1032]` em memória causa uma falha de igualdade, zero erros (`app/core/tests/test_medida_turno_do_par.py:541`). A identidade AST histórica c9/c12 está [nao verificado], pois esta cópia não contém `.git`; o delta fornecido não altera o produto `espelho.py` [nao verificado]. Achado: nada.

Verificação própria: os 72 testes C5/C7/C10/C11/C12/C13 passaram; os 11 mutantes C13 e os oito vizinhos executados ficaram vermelhos nas asserções/canais correspondentes (`app/ponto/tests/test_lastro_c13_sem_banco.py:45`, `:73`; `app/ponto/tests/test_lastro_c12_sem_banco.py:34`; `app/ponto/tests/test_lastro_c10_sem_banco.py:296`; `app/ponto/tests/test_lastro_c11_sem_banco.py:68`). São costuras: o teste dos builders reduz seus corpos às chamadas extraídas, portanto não executa a geometria/ORM completa (`app/ponto/tests/test_lastro_c13_sem_banco.py:196`, `:200`).

FILA — portabilidade do replay: `--red-c12` e `--mutante harness_c12` dependem de `git show`, indisponível nesta cópia sem repositório (`app/ponto/tests/test_lastro_c13_sem_banco.py:27`, `:40`). Contra-exemplo: invocar o replay nesta cópia falha antes de exercer a cura; substituí a reversão por AST em memória para verificar a mordida [nao verificado quanto à execução do replay original].

## 12/11 10:06 · sessao 38bc72b0ad37 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — PDF mantém regra própria de órfã/turno aberto, preexistente e não curada.** `_entrada_espuria` compara apenas HH:MM (app/relatorios/pdf_espelho.py:174–178), depois da união das órfãs de todas as datas (:480–481), para excluir turnos abertos (:517–519). Contraexemplo executado em memória: órfã em 10/11 às 07:02; turno aberto legítimo em 11/11 às 07:02, sem órfã nesse dia → função retorna `True` e exclui o turno. Tela conta `p.turno_aberto` sem essa guarda (app/ponto/services/espelho.py:436–437). Viola testemunha sem regra própria (CLAUDE.md:20–22).

- **BLOQUEIA — c13 restaura o contrato no PDF, mas conserva duas respostas para a mesma órfã.** PDF usa builder `grade_espelho_janela` (app/relatorios/pdf_espelho.py:474); tela usa `grade_da_celula`/`remapear_na_regua` (app/ponto/services/espelho.py:522–530). O novo teste fixa concretamente `orfas_grade=[]` para tela e `batida.orfa=True`, `turnos_abertos=0` para PDF (app/ponto/tests/test_lastro_c11_sem_banco.py:383–385). O teste chama tela por `Mock` (:326) e fornece órfã pronta ao builder (:356–357); não certifica convergência dos leitores. Classificações R de 6–9 fazem sentido, mas a cura dos sintomas não fecha a classe.

- **BLOQUEIA — rótulo não está garantido no ramo de ata agregada, defeito anterior não alcançado pelo canal c13.** Tela chama contrato sem `recusas` em `_pela_grade` (app/ponto/services/espelho.py:495–496). Se `grade_da_celula` levanta `AtaAgregadaSemMarcos`, seu handler chama `_pela_grade` (:563); uma `TipoSemFaixa` levantada ali escapa ao `except Exception` irmão (:572). PDF passa pelo contrato com `recusas={}` (app/relatorios/pdf_espelho.py:474), mas depois chama essa tela (:479). Contraexemplo: ata agregada + ausência aprovada sem faixa → leitura pode levantar em vez de apresentar o rótulo. O comportamento sem destinatário está explicitamente preservado e cobrado (app/ponto/tests/test_lastro_c13_sem_banco.py:126–127). Ocorrência produtiva: [nao verificado].

- **Rótulo no canal coberto:** nasce de `TipoSemFaixa.rotulo` (app/ponto/precedencia.py:102), chega ao PDF por `dia['recusa']` (app/escala/utils.py:1219; app/relatorios/pdf_espelho.py:567) e ao compositor único (app/ponto/services/espelho.py:339–341), que escreve palavra e veredito vazio. Tela transporta o mesmo atributo (app/ponto/services/espelho.py:880). Não encontrei contraexemplo novo de apagamento do rótulo por sobreposição em :567 com mesmas entradas/janela; universalidade fora desses canais: [nao verificado].

## 12/11 10:06 · sessao e928c079842b · - · - · origem=Ronald (tmux)

- BLOQUEIA — leitura nomeada incompleta no espelho/PDF: `_pela_grade()` chama o contrato **sem `recusas`** em `app/ponto/services/espelho.py:495`; esse caminho roda no trecho anterior à competência (`app/ponto/services/espelho.py:534`) ou na ata agregada (`app/ponto/services/espelho.py:563`), antes da carga nomeada (`app/ponto/services/espelho.py:653`). Contra-exemplo reproduzido em memória: ausência aprovada `fantasma_xyz` faz `_pela_grade()` levantar `TipoSemFaixa`; o PDF também chama esse espelho (`app/relatorios/pdf_espelho.py:479`). Execução completa com ORM: [nao verificado].

- Escritor do dicionário `recusas[data]`: A2, `app/ponto/precedencia.py:423`; sem destinatário, levanta em `app/ponto/precedencia.py:422`. A recusa impede produzir fato nesse dia (`app/ponto/precedencia.py:425`).

- Escritor de `dia['recusa']` e `dia['tipo_dia']=None`: `app/escala/utils.py:1219` e `app/escala/utils.py:1220`. Leitor produtivo encontrado de **essa recusa da grade**: `app/relatorios/pdf_espelho.py:567`; o compositor imprime a palavra e deixa veredito vazio em `app/ponto/services/espelho.py:339`.

- `tipo_dia=None` também é lido internamente pelo overlay de isenção: não ganha a flag de dia de trabalho (`app/escala/utils.py:889`), chamado depois da ausência (`app/escala/utils.py:1875`); o legacy preserva `None`, pois o default de `.get()` só cobre chave ausente (`app/escala/utils.py:1046`).

- Nenhum caminho financeiro demonstrado recebe o novo `None`: o único chamador produtivo encontrado que abre `recusas` na grade é o PDF (`app/relatorios/pdf_espelho.py:474`); `grade_do_fechamento()` lê `grade_da_celula` (`app/folha/export.py:335`, `app/folha/export.py:338`). Fechamento lê essa grade em `app/ponto/services/fechamento.py:1015` e seus mapas em `app/ponto/services/fechamento.py:1048`; `simular_folha --recalcular/--lido` passa por esse fechamento (`app/folha/management/commands/simular_folha.py:122`, `app/folha/management/commands/simular_folha.py:130`).

- Leitores potencialmente incompatíveis existem, porém alcance pelo canal novo não foi demonstrado: `por_dia_da_grade` descarta `None` sem `ausencia_tipo` (`app/ponto/services/dia_pago.py:392`); `classificar_orfas` traduz `None` para folga (`app/escala/orfa_classe.py:65`). Nenhum chamador produtivo de `classificar_orfas` encontrado no censo textual: [nao verificado]. Não classificar isso como regressão alcançável da c13.

- Sem `recusas`, o delta conserva os guards e chamadas anteriores: retorno cedo em `app/escala/utils.py:1205`, condição de consulta A2 em `app/escala/utils.py:1213`, ramo novo desativado em `app/escala/utils.py:1218`; os transportes omitem o argumento quando `None` (`app/escala/utils.py:808`, `app/escala/utils.py:1010`, `app/escala/utils.py:1019`, `app/escala/utils.py:1873`, `app/escala/utils.py:2114`, `app/escala/utils.py:2117`, `app/escala/utils.py:2365`). Equivalência dinâmica de todos os chamadores: [nao verificado].

- BLOQUEIA pouso financeiro — par `simular_folha` da competência 11 e efeito `abona=0`: [nao verificado], conforme dado fornecido. O comparador mede TXT e retidos (`app/folha/management/commands/simular_folha.py:160`, `app/folha/management/commands/simular_folha.py:218`); inspeção de código não substitui esse par: [nao verificado].

_4164 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
