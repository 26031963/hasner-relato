# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 21:20** · publicado em **05/11/2026 21:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 21:20 · sessao d0c9cbba8c22 · H12H06-A · verificador · origem=Code

**O delta corrige o caso do teste, mas a correção de duração continua incompleta:** o template ainda decide primeiro por `dias_corridos`, antes de consultar `fim_aberto`. (`app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`)

O material recebido não traz linhas da sonda identificando registros dessa rodada; apontar qual `colNN`/ID permanece errado em produção: **[não verificado]**.

**1. Quais casos ficam corrigidos ou errados**

As conclusões abaixo são por inspeção, sem execução: **[não verificado: resultado executado]**.

| Registro/condição | Resultado na célula após o delta |
|---|---|
| Tipo aberto, `data_fim=None`, `dias_corridos<=1`, sem parcialidade | Exibe “—”: corrige o caso denunciado. (`app/ponto/views.py:2243`, `app/templates/ponto/ausencias.html:164`) |
| Tipo aberto, `data_fim=None`, `dias_corridos=N>1` | **Continua exibindo “N dias”**, embora a cobertura prossiga até o fim da janela consultada. O primeiro ramo impede alcançar `fim_aberto`. (`app/templates/ponto/ausencias.html:160`, `app/ponto/turnos.py:2781`) |
| Range fechado de vários dias, mas `dias_corridos<=1` | **Continua exibindo “1 dia”**: `fim_aberto=False`, e a parcialidade também é falsa quando o fim difere do início. (`app/ponto/views.py:2243`, `app/ponto/turnos.py:2599`, `app/templates/ponto/ausencias.html:165`) |
| Range fechado com `dias_corridos=N>1` divergente das datas | **Continua exibindo N**, sem calcular a extensão inclusiva do range. A cobertura usa `data_fim`. (`app/templates/ponto/ausencias.html:161`, `app/ponto/turnos.py:2779`) |
| Tipo fora do catálogo, sem fim e `dias_corridos>1` | Exibe “N dias”, mas o juiz expande somente o dia inicial. Com `dias_corridos<=1`, “1 dia” coincide com essa expansão. (`app/templates/ponto/ausencias.html:160`, `app/ponto/catalogo/ausencias.py:194`, `app/ponto/turnos.py:2783`) |
| Tipo simultaneamente aberto e lançável por minutos, sem fim, com `0<minutos<480` e `dias_corridos<=1` | Exibe minutos antes de alcançar `fim_aberto`; a abertura continua sem representação na célula. Essa combinação de cadastro é oferecida sem exclusão entre os campos. (`app/ponto/views_tipos_ausencia.py:76`, `app/ponto/views_tipos_ausencia.py:78`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:163`) |

**Tipo inexistente não recebe permissão de abertura:** `fim_de` devolve `POR_LEI`, portanto `em_aberto_permitido` devolve falso. Ausência no catálogo atual também pode decorrer de inativação, pois a carga considera somente tipos ativos. (`app/ponto/catalogo/ausencias.py:194`, `app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:249`)

A divergência entre datas e `dias_corridos` é normalizada no `save`, mas a lista apenas lê o registro: o delta não normaliza nem salva os valores carregados. A existência desses registros em produção permanece **[não verificado]**. (`app/ponto/models.py:894`, `app/ponto/models.py:900`, `app/ponto/views.py:2238`, `app/ponto/views.py:2244`)

**2. Quem escreve e quem lê**

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| `fim_aberto` | Atribuição transitória em cada objeto da lista; não é gravação no banco. (`app/ponto/views.py:2243`) | Ramo da duração no template. (`app/templates/ponto/ausencias.html:164`) |
| `Ausencia.data_fim` | Criação: `derivar_data_fim` → kwargs → `save`. Edição: rederiva, atribui e salva. O modelo também deriva fim quando há dias acima de 1 em tipo não aberto; transição de estado inclui o campo em `update_fields`; `rederivar_fim` reaplica o `save`. (`app/ponto/services/ausencia.py:197`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:499`, `app/ponto/services/ausencia.py:508`, `app/ponto/models.py:907`, `app/ponto/services/ausencia.py:818`, `app/ponto/services/ausencia.py:1280`) | No caminho da lista: seleção por vigência, juiz de parcialidade e cálculo de `fim_aberto`. Na cobertura: `dias_da_ausencia` e `cobertura_ausencia_periodo`. Há também consumidores no export, relatórios e drawer. (`app/ponto/turnos.py:3087`, `app/ponto/turnos.py:2599`, `app/ponto/views.py:2243`, `app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2760`, `app/folha/export.py:70`, `app/relatorios/views.py:205`, `app/templates/ponto/partials/painel_ausencia.html:124`) |
| Retorno de `em_aberto_permitido(tipo)` | A função calcula `fim_de(tipo) == INDETERMINADO`; não persiste o booleano. A alimentação vem do catálogo vigente, carregado do cadastro ativo ou do fallback. (`app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:192`, `app/ponto/catalogo/ausencias.py:249`, `app/ponto/catalogo/ausencias.py:278`) | A view nova, a validação de `indeterminado` no formulário e o comando que procura bloqueantes sem fim. (`app/ponto/views.py:2243`, `app/ponto/forms.py:85`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:24`) |

Quem alimenta `TipoAusencia.fim` pela tela é `salvar` do cadastro, passando os campos para `gravar_tipo_ausencia`, que cria ou altera o modelo. (`app/ponto/views_tipos_ausencia.py:76`, `app/ponto/views_tipos_ausencia.py:86`, `app/ponto/views_tipos_ausencia.py:94`, `app/ponto/services/ausencia.py:1289`)

**Sim: para o mesmo catálogo vigente, `em_aberto_permitido(tipo)` e a pertinência a `Ausencia.TIPOS_EM_ABERTO` respondem à mesma pergunta com o mesmo conjunto:** tipos cujo `fim` é `indeterminado`. O segundo é alias de um conjunto vivo, não uma lista congelada diferente. (`app/ponto/catalogo/ausencias.py:205`, `app/ponto/catalogo/ausencias.py:434`, `app/ponto/catalogo/ausencias.py:464`, `app/ponto/catalogo/ausencias.py:403`, `app/ponto/models.py:689`)

Isso responde à **permissão do tipo**. A duração do registro ainda depende de existir `data_fim`; `dias_da_ausencia` prioriza esse fim explícito. (`app/ponto/turnos.py:2779`, `app/ponto/turnos.py:2781`)

**3. Mordida do teste** [nao verificado]

**Por inspeção, morde o comportamento anterior descrito no patch:** cria `afastamento_inss` sem fim, consulta a lista, isola a célula e rejeita “1 dia”, que era o fallback anterior. RED efetivamente executado em `58ac8543` e GREEN nesta cópia: **[não verificado]**. (`app/chamados/tests/test_h06_falta_pela_grade.py:168`, `app/chamados/tests/test_h06_falta_pela_grade.py:171`, `app/chamados/tests/test_h06_falta_pela_grade.py:177`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`; comportamento anterior no patch fornecido)

**Passaria com outra regressão:** célula vazia, “0 dias” ou “2 dias” satisfazem a única asserção sobre o conteúdo. O teste exige uma célula, mas não exige “—” nem prova a extensão da cobertura. Também não exercita os casos divergentes da tabela. (`app/chamados/tests/test_h06_falta_pela_grade.py:168`, `app/chamados/tests/test_h06_falta_pela_grade.py:178`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`)

**4. Outro defeito no delta**

O defeito funcional demonstrável é a **correção incompleta pela ordem dos ramos**, acompanhado da asserção insuficiente do teste; outro defeito introduzido pelo delta: **[não verificado]**. (`app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`, `app/chamados/tests/test_h06_falta_pela_grade.py:179`)

A célula aberta também mantém `data-value` baseado em `dias_corridos`, mesmo exibindo “—”; esse atributo já está fora do hunk alterado, portanto não o atribuo como regressão nova deste delta. (`app/templates/ponto/ausencias.html:159`, `app/templates/ponto/ausencias.html:164`; patch fornecido)

## 05/11 21:12 · sessao a965ac098cf9 · H12H06-A · verificador · origem=Code

**Retifico a v1: a contagem do TXT, isoladamente, não demonstra regressão de A.** Para a mesma falta aprovada e as mesmas datas, o 8792 recebe a mesma quantidade com 720 ou zero minutos. A troca altera as horas calculadas, mas não introduziu a contagem por calendário. (`app/folha/export.py:70`, `app/folha/export.py:121`, `app/folha/export.py:218`, `app/folha/export.py:732`)

**1. Condição para falta aprovada com zero**

No escape da resposta, `FALTA_DIA` solicita `tipo='falta', status='aprovada'`. O materializador omite minutos; a porta fornece zero e `Ausencia.save` substitui esse valor pelo previsto do dia. **Uma falta nova permanece aprovada com zero se o previsto consultado nessa gravação resultar em zero.** (`app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:580`, `app/ponto/services/ausencia.py:130`, `app/ponto/models.py:877`, `app/ponto/models.py:885`)

O sítio `declara_falta` não faz a falta nascer aprovada: omite status, a porta aplica `estado_inicial` e a aprovação posterior passa novamente pelo `save`, incluindo a persistência de minutos derivados. (`app/chamados/services/materializacao.py:484`, `app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:818`)

O juiz devolve zero sem vínculo, sem tipo, quando `eh_dia_trabalho(...) is not True` ou quando faltam início/fim. **Com previsto positivo preservado até a gravação, esses sítios não produzem falta nova aprovada com zero.** (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/ponto/models.py:885`)

Antes disso, os emissores de falta impõem guardas que a v1 deixou de considerar:

- O detector exige vínculo, resultado de dia de trabalho e início previsto; seu funil exige célula, com exceção explícita para limbo. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/detectar_ausencias.py:305`, `app/ponto/management/commands/detectar_ausencias.py:312`, `app/ponto/management/commands/detectar_ausencias.py:656`)
- A emissão retroativa exige `FURO_SEM_COBRANCA`. Esse ramo exige dia de cobrança, previsto não zero, realizado zero, nenhuma batida, ausência de cobertura integral e dia encerrado; depois o emissor exige vínculo e `eh_dia_trabalho is True`. (`app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:313`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:139`)
- A porta de nascimento também veta chamados dos módulos abrangidos quando o dia está identificado e falta vínculo ou a régua é degenerada. (`app/chamados/models.py:928`, `app/chamados/models.py:1097`, `app/ponto/services/furos_vetados.py:40`)

Essas condições não constituem uma guarda universal `previsto > 0` para toda pergunta: a fábrica da célula consulta `dia_sem_trabalho`, situação, data, veredito e marcos; a fábrica de órfãos existentes admite explicitamente continuar sem vínculo. **Isso não prova nascimento atual de chamado sem escala através da porta de emissão.** (`app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:840`, `app/chamados/models.py:1105`)

Há uma possibilidade condicional no código de produção: pergunta já existente continua aceita, mas o previsto na materialização passa a zero. Por exemplo, uma pergunta anteriormente respondida, sem validação, sem tranca e sem decisão administrativa pode continuar viva pelo texto; os materializadores não revalidam previsto positivo. Ocorrência desse cenário no dado fornecido: **[não verificado]**. (`app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1150`, `app/api/views.py:2631`, `app/chamados/services/materializacao.py:516`)

O teste sem escala cria chamado e pergunta diretamente; portanto não demonstra que os emissores atuais produziriam essa situação. (`app/api/tests/test_falta_na_resposta.py:26`, `app/api/tests/test_falta_na_resposta.py:30`, `app/api/tests/test_falta_na_resposta.py:34`)

**2. T1–T4 e alterações numéricas**

- **T1:** os leitores citados realmente usam minutos não zero para crédito e para pular a justificação integral. Com os valores novos descritos — falta derivada e atestado zero — não aparece a divergência apontada para atestados antigos de 720. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2598`)
  
  **A ressalva é causal:** A também muda números de **atestados novos desses sítios**, depois de aprovados. Antes entravam com 720; agora entram com zero. Nos leitores, isso troca crédito 720 por zero e permite justificar o dia quando o efeito é ABONA. Não depende de migrar legado. (`app/chamados/services/materializacao.py:522`, `app/ponto/models.py:877`, `app/ponto/services/fechamento.py:669`, `app/ponto/services/fechamento.py:690`)
  
  Exemplo deduzido: dia obrigatório sem trabalho, atestado aprovado ABONA, sem justificativa ou cobertura concorrente. A inclusão nas justificadas remove o dia das faltas injustificadas; se era a única falta injustificada da semana, altera também DSR perdido para DSR OK, contadores persistidos pelo fechamento. **É alteração numérica causada pelo diff; por si só, não demonstra defeito.** (`app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2629`, `app/ponto/motor_calculo_v2.py:2654`, `app/ponto/services/fechamento.py:1085`)

- **T2:** a expansão usa datas e calendário, sem minutos ou grade; o TXT conta `len(datas)`. A divergência entre horas zero e quantidade de dias não foi criada por A. (`app/ponto/turnos.py:2774`, `app/folha/export.py:70`, `app/folha/export.py:121`, `app/folha/export.py:218`)

- **T3:** os sítios citados permanecem fora dos hunks funcionais de A; os dois materializadores alterados criam ausência de um dia, sem solicitar range. Não identifiquei nesses pontos outro caminho numérico introduzido pelo diff. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/folha/export.py:118`, `app/ponto/models.py:902`; hunks fornecidos no anexo)

- **T4:** o teste chama atestado aprovado diretamente; o escape real usa status vazio e deixa `estado_inicial` decidir. Em ambos, atestado permanece com zero porque a derivação exige tipo literal `falta`. (`app/chamados/tests/test_h06_falta_pela_grade.py:101`, `app/chamados/services/materializacao.py:581`, `app/ponto/services/ausencia.py:344`, `app/ponto/models.py:877`)

**3. Achado novo no diff**

**Duração de afastamento aberto passa a ser apresentada como “1 dia”.** Um registro de tipo em `TIPOS_EM_ABERTO`, com `dias_corridos=1`, minutos zero e `data_fim=None`, é permitido pela porta; sua cobertura pode continuar por vários dias. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:70`, `app/ponto/models.py:902`, `app/ponto/turnos.py:2781`)

A view aplica `cobre_parte_do_dia` a todos os registros. Zero retorna falso e o novo `else` imprime “1 dia”; no hunk anterior, esse caso imprimia “—”. O predicado de parcialidade não determina a duração total de um afastamento aberto. **É uma regressão de apresentação causada pelo diff.** Ocorrência em produção e impacto financeiro: **[não verificado]**. (`app/ponto/views.py:2238`, `app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`; comportamento anterior no hunk fornecido)

## 05/11 21:11 · sessao 0a411a29b6d1 · - · - · origem=Ronald (tmux)

Para falta **nova**, o escape `FALTA_DIA` passa `('falta', 'aprovada')`; ambos os sítios omitem minutos, a porta aplica default zero e `save` substitui esse zero pela soma do previsto. Portanto fica aprovada com zero quando o juiz responde zero na ocasião da gravação. O ramo `declara_falta` nasce `aguardando_decisao`; só deriva quando posteriormente aprovado. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/chamados/services/materializacao.py:580`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:781`, `app/ponto/models.py:877`, `app/ponto/models.py:885`)

O previsto pode ser zero por ausência de vínculo/tipo, por `eh_dia_trabalho(...) is not True` ou ausência de início/fim. Esses materializadores não perguntam novamente se existe furo, célula de trabalho ou previsto positivo. Em `retornar_mesmo_tipo`, o veto de precedência nem é executado; uma linha existente também pode voltar sem novo `save`. (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/chamados/services/materializacao.py:467`, `app/chamados/services/materializacao.py:516`, `app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:377`)

**A guarda dos emissores de falta não pode ser omitida**, como foi na v1:

- O detector exige vínculo, dia com trabalho e início previsto; o funil ainda exige célula, ressalvada a exceção declarada de limbo. (`app/ponto/management/commands/detectar_ausencias.py:139`, `app/ponto/management/commands/detectar_ausencias.py:305`, `app/ponto/management/commands/detectar_ausencias.py:312`, `app/ponto/management/commands/detectar_ausencias.py:656`)
- A emissão retroativa exige `FURO_SEM_COBRANCA`; esse diagnóstico exige dia de cobrança, previsto não zero, realizado zero, nenhuma batida, ausência de cobertura integral e turno encerrado. Depois, o emissor exige vínculo, `eh_dia_trabalho is True` e início. (`app/ponto/management/commands/emitir_furo_retroativo.py:101`, `app/ponto/management/commands/emitir_furo_retroativo.py:111`, `app/ponto/supra_juiz.py:304`, `app/ponto/supra_juiz.py:313`, `app/ponto/management/commands/emitir_furo_retroativo.py:130`, `app/ponto/management/commands/emitir_furo_retroativo.py:139`)
- Os crons de turno aberto também calam sem vínculo; a porta de nascimento do chamado consulta `motivo_do_veto`, que veta vínculo ausente. (`app/ponto/management/commands/processar_alertas_avancados.py:37`, `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:104`, `app/chamados/models.py:1097`, `app/chamados/models.py:1105`, `app/ponto/services/furos_vetados.py:40`)

**As guardas de perguntas são mais amplas que as do detector de falta total.** A fábrica da célula exige ativo, dia que `dia_sem_trabalho` não dispense, data não futura e veredito vazio ou acusador; não consulta diretamente `minutos_previstos > 0`. Dia passado sem marcos pode seguir por `classificar_falta`, e `perguntar` fornece contexto datado para criar disputa/pergunta, inclusive sem destino derivável. (`app/chamados/services/fabrica_da_celula.py:27`, `app/chamados/services/fabrica_da_celula.py:49`, `app/chamados/services/fabrica_da_celula.py:89`, `app/chamados/services/fabrica_da_celula.py:99`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/disputa_emissao.py:389`)

Na fábrica de órfãos existentes, `_tem_base` aceita batidas sem escala e `_ata_do_dia` permite explicitamente continuar sem célula quando não há vínculo. `classificar_falta` também tem ramo binário por E/S e fallback sem marcos. Isso demonstra tratamento de **chamado existente**, não prova que um chamado novo ultrapasse o veto atual de nascimento sem vínculo. (`app/chamados/services/disputa_emissao.py:750`, `app/chamados/services/disputa_emissao.py:753`, `app/chamados/services/disputa_emissao.py:833`, `app/chamados/services/disputa_emissao.py:840`, `app/escala/servico_jornada.py:453`, `app/escala/servico_jornada.py:478`, `app/chamados/models.py:1105`)

Assim, o caminho defensável para zero é uma pergunta já disponível cujo previsto seja zero quando respondida/aprovada — por exemplo, alteração posterior de vínculo/grade — porque o materializador não revalida as condições de emissão. Ocorrência desse cenário na produção fornecida: **[não verificado]**. O teste sem escala fabrica diretamente chamado e pergunta; não demonstra sua emissão pelos crons. (`app/api/tests/test_falta_na_resposta.py:27`, `app/api/tests/test_falta_na_resposta.py:30`, `app/api/tests/test_falta_na_resposta.py:34`, `app/chamados/services/materializacao.py:516`, `app/ponto/models.py:885`)

Não encontrei emissor atual do motivo `declara_falta` no catálogo de perguntas; o caminho materializador continua presente. A existência/origem de perguntas desse motivo em produção permanece **[não verificado]**. (`app/chamados/catalogo/perguntas.py:34`, `app/chamados/services/materializacao.py:1047`)

## 05/11 21:10 · sessao a8bef660087b · - · - · origem=Ronald (tmux)

Há um achado novo de apresentação: **ausência com término aberto pode aparecer como “1 dia”**. A porta aceita `dias_corridos=1`, `minutos=0` e preserva `data_fim=None` para tipo em `TIPOS_EM_ABERTO`; a cobertura desse registro continua até o limite consultado, podendo abranger vários dias. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:70`, `app/ponto/models.py:902`, `app/ponto/turnos.py:2781`)

A view aplica `cobre_parte_do_dia` a todos os registros; com zero minutos, o predicado retorna falso. O novo `else` do template apresenta então **“1 dia”**, enquanto o hunk anterior fornecido apresentava “—”. O predicado responde parcialidade do dia, sem determinar duração total de um afastamento aberto. (`app/ponto/views.py:2237`, `app/ponto/views.py:2240`, `app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:160`, `app/templates/ponto/ausencias.html:164`; comportamento anterior: `diff_A.patch`, hunk de `app/templates/ponto/ausencias.html:160` fornecido pelo usuário)

O caminho HTTP permite esse registro: dias omitidos viram 1; minutos vazios viram 0; o parâmetro indeterminado chega à porta de criação. Ocorrência em produção e impacto financeiro desse achado: **[não verificado]**. (`app/ponto/views.py:2380`, `app/ponto/services/ausencia.py:95`, `app/ponto/views.py:2405`, `app/ponto/views.py:2412`)

Em H12, os sítios inspecionados usam o mesmo predicado e a janela da empresa: classificação, resumos, contador e marcação da linha. Não identifiquei outro defeito causal nesses hunks por inspeção. (`app/folha/export.py:60`, `app/folha/export.py:807`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:104`, `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/_fech_linha.html:2`)

## 05/11 21:10 · sessao bbbf9f695f68 · - · - · origem=Ronald (tmux)

T1 está correta sobre a idade dos leitores e a concordância com os valores novos de A: falta deriva minutos, mas não pertence a `TIPOS_PAGOS`; atestado zero não gera crédito e, sendo ABONA, justifica o dia. O juiz de cobertura também considera zero dia inteiro. (`app/ponto/models.py:683`; `app/ponto/catalogo/ausencias.py:95`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`; `app/ponto/turnos.py:2598`)

**Ressalva de causalidade:** isso não significa ausência de delta financeiro em A. O diff troca **os atestados novos desses sítios de 720 para zero**; portanto muda entradas dos leitores antigos mesmo sem migrar as 18 linhas antigas. Os sítios omitem minutos, a porta usa zero e o modelo só deriva tipo `falta`. (`app/chamados/services/materializacao.py:484`; `app/chamados/services/materializacao.py:520`; `app/ponto/services/ausencia.py:130`; `app/ponto/services/ausencia.py:343`; `app/ponto/models.py:877`)

Exemplo deduzido do código, sem afirmar ocorrência em produção: atestado aprovado com efeito ABONA, dia obrigatório sem trabalho, sem Justificativa e sem cobertura concorrente:

- Com 720: crédito de 720 e exclusão da justificação; com zero: crédito zero e inclusão nas justificadas. (`app/ponto/services/efeito_ausencia.py:44`; `app/ponto/services/efeito_ausencia.py:51`; `app/ponto/services/efeito_ausencia.py:55`; `app/ponto/services/credito_parcial.py:24`; `app/ponto/services/fechamento.py:665`; `app/ponto/services/fechamento.py:685`)
- No motor, esse dia sai de `faltas_injustificadas` e passa a `_abono`; mudam contagem e horas de falta/abono. Se era o único dia injustificado da semana, muda também semana DSR perdido para semana DSR OK. (`app/ponto/motor_calculo_v2.py:2600`; `app/ponto/motor_calculo_v2.py:2605`; `app/ponto/motor_calculo_v2.py:2610`; `app/ponto/motor_calculo_v2.py:2620`; `app/ponto/motor_calculo_v2.py:2629`; `app/ponto/motor_calculo_v2.py:2654`)
- O fechamento persiste os contadores de DSR do motor; a sua `horas_falta` vem separadamente das faltas declaradas, portanto não se deve confundir a alteração de `resultado.horas_falta` com alteração automática desse campo persistido. (`app/ponto/services/fechamento.py:866`; `app/ponto/services/fechamento.py:987`; `app/ponto/services/fechamento.py:1085`)

Esse delta é coerente com atestado de dia inteiro; **não constitui por si só novo defeito**, mas uma descrição de T1 que atribua toda alteração numérica apenas à migração do legado fica incompleta. (`app/ponto/turnos.py:2583`; `app/ponto/services/fechamento.py:685`; `app/ponto/motor_calculo_v2.py:2600`)

T2: os fatos citados estão certos. `datas_no_periodo` usa `dias_da_ausencia`; a expansão usa calendário e limites do registro/janela, sem minutos e sem grade; TXT usa `len(datas)`. Uma mesma falta aprovada de um dia gera a mesma quantidade TXT com 720 ou zero. (`app/folha/export.py:63`; `app/ponto/turnos.py:2774`; `app/folha/export.py:87`; `app/folha/export.py:218`; `app/folha/export.py:732`)

T4: a fraqueza do teste existe, mas não altera o resultado de minutos. O teste usa atestado aprovado diretamente; a tabela real usa status vazio; a porta aplica `estado_inicial`, documento anexado conduz a `aguardando_decisao`, aprovação passa por `mudar_estado`, e nenhuma dessas etapas deriva minutos de atestado. (`app/chamados/tests/test_h06_falta_pela_grade.py:101`; `app/chamados/services/materializacao.py:581`; `app/ponto/services/ausencia.py:344`; `app/ponto/services/ausencia.py:763`; `app/ponto/services/ausencia.py:857`; `app/ponto/services/ausencia.py:808`; `app/ponto/models.py:877`)

A devolução de registro já existente preserva seu valor, sem `save`, de modo que repetir a resposta não converte atestado antigo 720 em zero. (`app/ponto/services/ausencia.py:294`)

## 05/11 21:09 · sessao 052f591a4700 · H12H06-SEPARA-TRAVA · censo · origem=Code

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 05/11 21:05 · sessao a77a07cefa5d · - · - · origem=Ronald (tmux)

Confirmado: o ramo `adesao is None` apenas lavra/carimba `CelulaDia`, condicionado a `apply_`, e retorna antes das emissões: `app/ponto/services/cartorio.py:1162`, `:1207`, `:1242`, `:1264`, `:1268`. A linha `:1223` é comentário.

Esse ramo não preenche `out['cods']`; o ramo normal preenche em `cartorio.py:1426`. Cluster e fábrica posteriores dependem dessas datas (`:986`, `:990`, `:996`). No signal, `julgar_celula` passa somente `[cel]` (`:1989`); assim, o ramo nunca-bateu puro não alcança Colaborador ou Chamado pelo cartório.

FKs confirmadas:

- `CelulaDia.ausencia → Ausencia`, `SET_NULL`: `app/escala/models.py:1796`, `:1797`.
- `Ausencia.documento_referencia → self`, `SET_NULL`: `app/ponto/models.py:789`, `:790`.
- `Ausencia.chamado_origem → ChamadoColaborador`, `SET_NULL`: `app/ponto/models.py:784`, `:785`.

Essas declarações incluem atualizações referenciais no escopo das exclusões; SQL emitido, ordem do Collector, posição desses updates relativamente ao `post_delete` e primeira aquisição efetiva de travas: **[nao verificado]**.

## 05/11 21:05 · sessao b5677bb09b8e · H12H06-A · verificador · origem=Code

**Parecer: reprovo A como concluído.** H12 está coerente por inspeção. Em H06, falta aprovada com zero minutos ainda pode produzir **um dia de falta no TXT**, embora o fechamento compute zero horas; também sobrevivem leitores financeiros com regra própria de parcialidade. (`app/folha/export.py:121`, `app/folha/export.py:218`, `app/folha/export.py:732`, `app/ponto/services/fechamento.py:866`, `app/ponto/services/fechamento.py:665`)

Execução da suíte, RED/GREEN na base e ocorrência/impacto em produção: **[não verificado]**. [nao verificado]

**1. Quem escreve e quem lê**

O censo abaixo distingue quem fornece os valores de quem efetivamente os persiste: as portas recebem tipo/minutos/status; `criar_ausencia`, `editar_ausencia_registro`, `mudar_estado` e `Ausencia.save` fazem a gravação. (`app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:341`, `app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:792`, `app/ponto/models.py:868`)

| Campo/valor | Escritores | Leitores |
|---|---|---|
| `Ausencia.minutos` | Default zero; criação recebe `minutos`; edição recebe/zera; `save` deriva falta aprovada zero. (`app/ponto/models.py:755`, `app/ponto/services/ausencia.py:343`, `app/ponto/services/ausencia.py:507`, `app/ponto/models.py:877`) | Rateio da falta, fechamento, efeitos, crédito parcial, cobertura, apresentação/API/CSV/auditoria, discriminados abaixo. (`app/folha/export.py:121`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2600`) |
| `Ausencia.status` | Criação usa status fornecido ou `estado_inicial`; transição por `mudar_estado`; rejeição em lote de férias usa `update`. Migration histórica 0057 também escreve status. (`app/ponto/services/ausencia.py:344`, `app/ponto/services/ausencia.py:763`, `app/ponto/services/ausencia.py:808`, `app/ponto/services/ausencia.py:1265`, `app/ponto/migrations/0057_a4_backfill_pendente.py:43`) | Derivação, cobertura, precedência, decisão visual, cobrança, filtros/telas, relatórios e vigias. (`app/ponto/models.py:877`, `app/ponto/turnos.py:2514`, `app/ponto/precedencia.py:273`, `app/ponto/services/dia_decidido.py:481`, `app/chamados/services/silencio_ausencia.py:77`, `app/ponto/services/vigia_ausencia.py:108`) |
| `Ausencia.tipo` | Criação e edição; os dois materializadores fornecem o tipo. (`app/ponto/services/ausencia.py:342`, `app/ponto/services/ausencia.py:497`, `app/chamados/services/materializacao.py:485`, `app/chamados/services/materializacao.py:521`) | Derivação literal de `falta`, catálogo/efeito, cobertura/range, precedência, leitores financeiros, cobrança e apresentação. (`app/ponto/models.py:877`, `app/ponto/models.py:842`, `app/ponto/turnos.py:2532`, `app/ponto/precedencia.py:381`, `app/ponto/services/fechamento.py:685`, `app/chamados/services/materializacao.py:602`) |
| `Colaborador.data_demissao` | Agendamento, desligamento efetivo e reativação, persistidos pela porta de colaborador. (`app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/colaborador.py:32`) | Predicado H12, prontidão, recortes de grade/cobrança/universo, ciclo do desligamento e apresentação, discriminados abaixo. (`app/folha/export.py:60`, `app/folha/export.py:1060`, `app/ponto/services/fechamento.py:423`, `app/escala/services/leitor_celula.py:351`, `app/folha/views.py:658`) |
| Chave `rescisao` | Somente `montar_resumos`. (`app/ponto/services/fechamento_lista.py:109`) | Linha/cor/badge e contador da view; o atributo HTML alimenta o filtro. (`app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:117`) |
| `n_rescisao` | Soma de `r['rescisao']` na view. (`app/ponto/views.py:1215`) | Pílula do fechamento. (`app/templates/ponto/fechamento.html:65`) |
| `_DECLARA_FALTA_MAPA` | Declaração em `chamados/models.py`. (`app/chamados/models.py:3018`) | Materializador da pergunta; o selo inspeciona sua AST. (`app/chamados/services/materializacao.py:468`, `app/chamados/tests/test_h06_selo_minutos_sem_literal.py:50`) |
| `cobre_parte` | Atributo transitório preenchido na lista pela chamada ao juiz existente. (`app/ponto/views.py:2240`) | Duração apresentada na lista. (`app/templates/ponto/ausencias.html:163`) |
| `sai_pela_rescisao` | Definição única do predicado. (`app/folha/export.py:54`) | `classificar_export` e `montar_resumos`. (`app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`) |
| `janela_competencia` | Wrapper de `janela_fechamento`; corte vem de `Empresa.dia_inicio_competencia`. (`app/folha/export.py:47`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`) | Export, resumos, prontidão, prévia, PDFs, relatórios e comandos, discriminados abaixo. (`app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:104`, `app/folha/export.py:1073`, `app/folha/previa.py:42`, `app/folha/validacao_pdf.py:42`, `app/relatorios/services.py:214`) |

Os fornecedores de `tipo/minutos/status` fora do diff também passam pela porta:

| Origem | Sítio |
|---|---|
| Lançamento administrativo, colaborador e API | `app/ponto/views.py:2412`, `app/ponto/views.py:2533`, `app/api/views.py:1820` |
| Veredito da célula e copiloto de ausência | `app/ponto/services/veredito_celula.py:315`, `app/ponto/services/checar_ausencia.py:46` |
| Regularização e abandono provisório | `app/ponto/services/regularizacao.py:118`, `app/ponto/management/commands/marcar_abandono_provisorio.py:76` |
| Espelho de férias | `app/ferias/models.py:385`, `app/ponto/services/ausencia.py:1186` |
| Edição de tipo/minutos | `app/ponto/views.py:2791` |
| Decisões individuais/em lote | `app/ponto/views.py:85`, `app/ponto/views.py:2287`, `app/ponto/views.py:2316`, `app/ponto/views.py:2640`, `app/ponto/views.py:2698` |
| Reverter rejeição; rejeitar férias | `app/ponto/views.py:2581`, `app/ferias/services.py:356` |
| Rejulgar status e rederivar fim | `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:46` |

Os demais leitores de **minutos**, inclusive fora do diff, são:

- Propriedade `horas`, admin e fotografias de auditoria. (`app/ponto/models.py:865`, `app/ponto/admin.py:44`, `app/ponto/services/ausencia.py:446`, `app/ponto/views.py:2347`, `app/ponto/views.py:2730`, `app/ponto/views.py:2768`, `app/ponto/views.py:2811`)
- Palavra/cartão/histórico, payload da API e CSV. (`app/ponto/services/dia_decidido.py:423`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`)
- Juiz de parcialidade, lista e drawer. (`app/ponto/turnos.py:2598`, `app/templates/ponto/ausencias.html:163`, `app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:149`, `app/templates/ponto/partials/painel_ausencia.html:299`)

Os demais leitores de **tipo/status** concentram-se nestes sítios:

| Domínio | Leitores fora do diff |
|---|---|
| Cobertura, sobreposição e afastamento | `app/ponto/turnos.py:2509`, `app/ponto/turnos.py:2521`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2705`, `app/ponto/turnos.py:2733`, `app/ponto/turnos.py:2793`, `app/ponto/turnos.py:3087` |
| Propriedades e apresentação | `app/ponto/models.py:711`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/services/dia_decidido.py:453`, `app/ponto/services/dia_decidido.py:476`, `app/api/views.py:1873`, `app/ponto/admin.py:44` |
| Precedência, grade, furo e cartório | `app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:339`, `app/escala/servico_jornada.py:340`, `app/escala/utils.py:1209`, `app/escala/utils.py:1231`, `app/ponto/services/cartorio.py:355` |
| Chamados e cobrança | `app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/historico.py:129`, `app/escala/services/furos_diarios.py:223` |
| Relatórios, calendário, inteligência e férias | `app/relatorios/views.py:197`, `app/relatorios/views.py:277`, `app/colaboradores/services/calendario.py:674`, `app/colaboradores/services/detalhe.py:104`, `app/ponto/services/alertas_ausencia.py:48`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:516`, `app/ferias/models.py:144` |
| Vigias e comandos | `app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:104`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:115`, `app/ponto/services/vigia_ausencia.py:120`, `app/ponto/services/vigia_ausencia_veredito.py:46`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`, `app/inteligencia/management/commands/calcular_scores.py:12`, `app/inteligencia/management/commands/gerar_alertas.py:55` |

Para **`data_demissao`**, além dos leitores financeiros já citados, há leitores de disponibilidade, cadastro, julgamento, holerite e ciclo do vínculo. (`app/ponto/utils.py:180`, `app/escala/services/cadastro_realidade.py:444`, `app/ponto/services/cartorio.py:1096`, `app/ponto/supra_juiz.py:177`, `app/ponto/supra_juiz.py:194`, `app/holerite/matriz.py:88`, `app/colaboradores/services/vinculo.py:92`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:41`, `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:55`)

Os demais consumidores de **`janela_competencia`** incluem ausências, grade, férias, retenção, histórico da folha e serviços/comandos auxiliares. (`app/folha/export.py:166`, `app/folha/export.py:185`, `app/folha/export.py:337`, `app/folha/export.py:486`, `app/folha/export.py:1266`, `app/folha/views.py:529`, `app/folha/views.py:588`, `app/folha/services/vigia_pausa.py:123`, `app/folha/services/sem_furo.py:47`, `app/relatorios/cartao_pela_celula.py:24`, `app/ponto/services/vigia_pareador_celula.py:59`, `app/ponto/management/commands/censo_col900.py:89`, `app/ponto/management/commands/gerar_celulas.py:176`, `app/ponto/management/commands/lavrar_status_competencia.py:40`, `app/ponto/management/commands/mapa_divergencia.py:61`, `app/ponto/management/commands/mapa_divergencia.py:93`)

**Sim, sobraram regras próprias contraditórias:** os leitores financeiros de parcialidade; o TXT que conta falta zero; e apresentação que ainda ensina `>0 = parcial`. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/folha/export.py:218`, `app/templates/ponto/lancar_ausencia.html:70`)

**2. Condições exatas de `Ausencia.save` e caminhos de gravação**

A derivação acontece em **qualquer `save`**, sem exigir detectar um flip, quando simultaneamente:

- `status == 'aprovada'`;
- `tipo == 'falta'`;
- `not self.minutos`.

Ela soma o juiz do previsto, inclusivamente, de `data` até `data_fim or data`. Valores não zero são preservados. (`app/ponto/models.py:877`, `app/ponto/models.py:882`, `app/ponto/models.py:885`)

Para uma **falta nova** dos dois materializadores, não encontrei bypass: ambos omitem minutos, a porta aplica zero, instancia `Ausencia` e chama `save()` inteiro. A aprovação posterior inclui `minutos` e demais derivados em `update_fields`. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`, `app/ponto/services/ausencia.py:818`)

Há duas distinções necessárias:

- **Atestado aprovado zero não deriva**, porque o teste exige tipo literal `falta`. Isso é o comportamento introduzido pelos sítios, não um bypass de criação. (`app/ponto/models.py:877`, `app/chamados/services/materializacao.py:523`)
- **Registro existente pode ser devolvido sem `save`**, inclusive uma falta já aprovada com zero. Portanto repetir a materialização não garante rederivação de legado. (`app/ponto/services/ausencia.py:294`, `app/ponto/services/ausencia.py:299`)

O `update` de produção identificado rejeita exclusivamente férias; o admin é somente leitura; os receivers de ausência julgam/reconciliam consumidores. Não encontrei `bulk_create` ou `update` que aprove faltas oriundas desses sítios. Ausência absoluta de SQL dinâmico ou chamadas não atribuíveis: **[não verificado]**. (`app/ponto/services/ausencia.py:1265`, `app/ponto/admin.py:38`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/core/censo_escritas.py:43`)

Para **atestado aprovado com efeito ABONA no catálogo**:

| Leitor | Zero | 720 |
|---|---|---|
| Juiz de cobertura | Dia inteiro | Dia inteiro. (`app/ponto/turnos.py:2598`) |
| Lista | “1 dia” | “1 dia”. (`app/ponto/views.py:2240`, `app/templates/ponto/ausencias.html:163`) |
| Fechamento | Justifica o dia; não soma crédito parcial | Soma 720 como crédito e pula justificação do dia. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`) |
| `efeitos_da_ausencia` | Justifica o dia | Crédito 720; pula justificação. (`app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`) |
| `creditos_por_dia` | Nenhum crédito | Crédito 720 no dia inicial do registro. (`app/ponto/services/credito_parcial.py:24`) |
| Teto de pontualidade | Dia inteiro | Também dia inteiro, pelo juiz — divergindo da regra de justificação acima. (`app/ponto/turnos.py:2668`, `app/ponto/turnos.py:2689`, `app/ponto/services/fechamento.py:683`) |
| Drawer | Oculta detalhe de minutos; não pede período parcial | Mostra 720; também não pede período parcial. (`app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:296`, `app/ponto/views.py:2084`) |
| Palavra do dia | Sem duração numérica | Acrescenta 12h. (`app/ponto/services/dia_decidido.py:230`, `app/ponto/services/dia_decidido.py:488`) |
| API/CSV/admin | Valor cru zero | Valor cru 720. (`app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`, `app/ponto/admin.py:44`) |

O TXT efetivo recebe os eventos do fechamento e as datas de falta; não lê diretamente minutos de atestado nesse laço. Assim, a diferença zero/720 chega a ele pelos resultados anteriores do fechamento. (`app/folha/export.py:722`, `app/folha/export.py:732`)

**3. Folga, sem escala e sem célula**

Sem escala vigente, sem tipo, com `eh_dia_trabalho(...) is not True`, ou sem marcos de início/fim, o juiz devolve zero; a falta aprovada grava esse zero após passar pela derivação. (`app/escala/utils.py:2282`, `app/escala/utils.py:2284`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`, `app/ponto/models.py:887`)

**Sem célula não implica zero:** a autoridade existente pode consultar o cálculo do vínculo/template. O diff reutiliza essa autoridade; não acrescenta fallback próprio. (`app/escala/models.py:1096`, `app/escala/models.py:1133`, `app/escala/utils.py:2290`, `app/ponto/models.py:881`)

O contraexemplo relevante é:

1. Os materializadores pedem `retornar_mesmo_tipo`; o veto de precedência só é executado no modo `veto`. Logo esses sítios não passam por esse veto ao criar a linha. (`app/chamados/services/materializacao.py:490`, `app/chamados/services/materializacao.py:526`, `app/ponto/services/ausencia.py:377`)
2. A falta pode nascer aprovada zero sem escala — situação expressamente aceita pelo teste alterado. (`app/api/tests/test_falta_na_resposta.py:44`, `app/api/tests/test_falta_na_resposta.py:48`)
3. O fechamento soma zero minutos/horas. (`app/folha/export.py:121`, `app/ponto/services/fechamento.py:866`, `app/ponto/services/fechamento.py:987`)
4. O TXT seleciona o registro por tipo/status/data e conta `len(datas)`: **um dia**, se o colaborador entrar no crivo e existir a rubrica. (`app/folha/export.py:87`, `app/folha/export.py:160`, `app/folha/export.py:218`, `app/folha/export.py:732`)
5. Em folga, a grade mantém explicitamente **folga**, apesar da falta aprovada; o seletor de datas do TXT não consulta essa decisão. (`app/escala/utils.py:1227`, `app/escala/utils.py:1231`, `app/folha/export.py:181`)

Portanto, **sim: zero significa nada nas horas, mas pode significar um dia no TXT**. Essa divergência impede certificar a migração dos leitores de H06. (`app/ponto/services/fechamento.py:866`, `app/folha/export.py:218`)

**4. H12: pergunta e janela**

Os quatro sítios pedidos estão alinhados: predicado único `data_demissao is not None and data_demissao <= fim`, classificador, chave dos resumos, linha e contador. (`app/folha/export.py:60`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`, `app/templates/ponto/_fech_linha.html:2`, `app/ponto/views.py:1215`)

A janela também coincide: classificador usa a empresa recebida; resumos usam `colab.empresa`, com cache por empresa; ambos chegam a `janela_fechamento`. Portanto respeitam cortes distintos, sem fixar 21 nesses leitores novos. (`app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:103`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:57`)

Os baldes de rescisão de `folha/views`, relatórios e retrato acompanham o classificador. (`app/folha/views.py:589`, `app/folha/views.py:594`, `app/relatorios/views.py:1658`, `app/relatorios/views_pend.py:58`, `app/folha/fora_da_folha.py:119`, `app/folha/retrato_folha.py:1157`)

Persistem duas diferenças, com perguntas distintas:

- `prontidao` chama de `"rescisao"` a cegueira de quem saiu **antes do início**; não usa o predicado H12. Isso não decide entrada no TXT, mas o vocabulário não representa a mesma classificação. (`app/folha/export.py:1060`, `app/folha/export.py:1081`, `app/folha/export.py:1083`)
- O raio-X ainda escreve `†deslig.` pela mera presença da data, inclusive futura; seu balde de rescisão, entretanto, lê o juiz. (`app/folha/views.py:658`, `app/folha/views.py:594`)

O corte de aposentadoria continua pendente na cópia: o universo exclui desligados anteriores à janela, mas o classificador ainda consulta seus fechamentos gravados; não existe `encerrado` nas escolhas e o vigia continua incluindo as projeções nulas. Isso é dívida anterior, não regressão do predicado H12 de A. (`app/docs/CORTES.md:270`, `app/ponto/services/fechamento.py:423`, `app/folha/export.py:777`, `app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1680`)

**5. Classes de reprovação**

| Classe | Verificação |
|---|---|
| Segundo juiz | Não identifiquei juiz novo no diff: H12 extrai uma condição compartilhada; H06 chama o previsto e a cobertura existentes. K1/K2 continuam registrados em `classificar_export`. (`app/folha/export.py:54`, `app/folha/export.py:858`, `app/ponto/views.py:2240`, `app/core/juizes.py:298`) |
| Segundo escritor | Os sítios alterados continuam pela porta; aprovação persiste os derivados do modelo. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:818`) |
| Relógio fora da borda | Não aparece leitura nova de relógio nos hunks funcionais: o predicado H12 recebe fim; a derivação usa as datas da ausência. (`app/folha/export.py:54`, `app/ponto/models.py:882`, `app/ponto/services/fechamento_lista.py:104`) |
| Derivação paralela | Os dois literais foram removidos, mas sobreviveram os julgamentos financeiros de parcialidade e o contador de dias independente do valor zero. (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/credito_parcial.py:24`, `app/folha/export.py:218`) |


[truncado em 20000 de 24439 caracteres]

## 05/11 21:05 · sessao 2b546d724cfa · - · - · origem=Ronald (tmux)

Consolidação das direções inversas; todas são estáticas, execução/primeira aquisição concreta de linhas **[nao verificado]**.

| Par/direção | Testemunha na mesma transação |
|---|---|
| **Colaborador → Empresa** | `normalizar_identificadores.py:106` atomic → C U `:109` → `colaboradores/services/colaborador.py:88` → E U command `:112` → `colaboradores/services/estrutura.py:27`. |
| **Empresa → Colaborador/Ausencia** | Não encontrei testemunha; zero global **[nao verificado]**. Escritores e exclusões administrativas detalhados abaixo. |
| **Chamado/Colaborador → Ausencia nova** | `regularizacao.py:89` atomic → Justificativa I `:91` ou U `:106` → receiver `ponto/signals.py:162/:165` → cartório K, com possíveis L de Chamado/Colaborador → loop `regularizacao.py:110/:111/:118` → `ausencia.py:461` → I `:418` ou reabertura U via `:291`. |
| **Chamado/Colaborador → Ausencia de dia seguinte** | `marcar_abandono_provisorio.py:22` atomic abrangente → loops pessoas `:43` e dias `:70` → garantir `:76`; K do fato anterior precede Ausencia seguinte. Condicionado ao ramo de K que efetivamente trava e à existência de outra iteração. |
| **Chamado/Colaborador → Ausencia de férias seguinte** | Atomic externo dos serviços de férias → `ausencia.py:1182` loop → criar `:1186`; K do dia anterior precede I/U seguinte. Atomic, override e famílias específicos pertencem ao inventário do agente de escritores. |
| **Chamado/Colaborador → nova Ausencia após D antiga** | `ferias/services.py:375` atomic → apagar `:385` → `ausencia.py:1254` D → K → `ferias/services.py:387` ag.save → `ferias/models.py:437` sync → `ausencia.py:1186` I em data nova. Condicionado ao K e ao dia novo faltante. |
| **Chamado/Colaborador → Ausencia de outra irmã** | `ponto/views.py:2631` atomic → loop `:2632/:2633`; K após primeiro U `ausencia.py:819` pode travar Chamado/Colaborador antes do U da próxima irmã. |
| **FechamentoMensal → Ausencia de outra irmã** | Mesmo atomic/loop `views.py:2631/:2632/:2633`: `ausencia.py:895` → U `:819`, carimbos U `:911`, invalidar `:930` → FM U `fechamento.py:1640` → próxima irmã U `ausencia.py:819`. Requer ≥2 irmãs e FM atingida. |
| **Colaborador → FechamentoMensal** | `justificativa.py:125` atomic → registrar batida `:173` → I `registro_batida.py:152` ou `:157` → onboarding `colaboradores/signals.py:87/:102` → C U `colaboradores/services/colaborador.py:61` → `Colaborador.save`, `models.py:439` → invalidar `justificativa.py:200` → FM U `fechamento.py:1640`. Guards onboarding: batida criada `signals.py:94`, onboarding false `:97`, usuário com last_login `:99`. |
| **FechamentoMensal → Colaborador** | `views.py:1793` atomic → invalidar `:1806` → FM U `fechamento.py:1640` → se saída informada e sem batida gerada `views.py:1809`, registrar `:1816` → I `registro_batida.py:152/:157` → mesmo receiver onboarding e C U acima. O K anterior do `j.save()` em `views.py:1800` pode já ter adquirido C; primeira aquisição C posterior a FM **[nao verificado]**. |
| **Colaborador → CelulaDia** | J1 acima: onboarding C U durante registrar batida `justificativa.py:173`; após retorno, `j.save()` `:193` → `ponto/signals.py:162/:165` → K → lavra `cartorio.py:1822` → `portas/celula.py:682` U. Requer guards onboarding e julgamento que efetivamente lavra. |

**Regravação, separada de primeira aquisição:** no `decidir`, K após U `ausencia.py:819` pode travar C/Chamado antes do segundo U `:911`; a própria Ausencia já estava adquirida. Também `_reabrir` pode fazer saves sucessivos `ausencia.py:1124/:1125/:1132`, cada um com K, mas isso não prova primeira aquisição inversa.

**Empresa — fechamento das entradas encontradas:**

- `atualizar_empresa` efetua `empresa.save()` em `app/colaboradores/services/estrutura.py:20`; normalização efetua queryset update `:27`.
- Formulário chama atualização em `app/colaboradores/views.py:521`, sem atomic próprio abrangente na função `:485`.
- `diff_janela_he` tem atomic `app/ponto/management/commands/diff_janela_he.py:74`, E U `:101/:103`, depois `_tot` `:102/:104` → leitura `autoridade_do_periodo` `:59`. Não demonstra C/A U/L.
- `diff_janela_he_total.py:106` atualiza E pela mutação executada dentro de `app/ponto/services/diff_frota.py:72` atomic, callback `:86`; cálculo posterior recebe `somente_leitura=True` `:87/:88`. Não demonstra C/A U/L.
- `aplicar_janela_he_total.py:119` atualiza E, sem atomic abrangente próprio; delegação de recálculo começa em `:137`. Não uni as transações.
- `EmpresaAdmin` herda `ModelAdmin` padrão em `app/colaboradores/admin.py:7/:8`; implementação SQL/atomic/cascatas do Django **[nao verificado]**. `Colaborador.empresa` usa PROTECT em `app/colaboradores/models.py:321`.
- Admin de Colaborador `app/colaboradores/admin.py:48` e de Ausencia `app/ponto/admin.py:38` usam SoLeitura; delete negado por `app/core/admin.py:65/:66`.

**Caminho excluído da prova de mesma transação:** `responder_pergunta_disputa_core` chama `_ressuscitar_por_resposta` em `app/chamados/services/disputa_emissao.py:1326` → Chamado U `app/chamados/services/arquivo.py:208`, depois materializa Ausencia em `disputa_emissao.py:1330` → `materializacao.py:519`. Não há atomic no core definido em `disputa_emissao.py:1247`; as chamadas web/API `app/chamados/views.py:1283` e `app/api/views.py:2643` não forneceram atomic abrangente. Os wrappers transacionais `declaracao_texto.py:302/:605` aceitam/produzem HH:MM (`:276`, `:368`, `:641`), não os escapes de ausência. Atomic externo efetivo desse caminho **[nao verificado]**.

## 05/11 21:04 · sessao fb67b25b570c · - · - · origem=Ronald (tmux)

Inventário da porta e chamadores, sem expansão do cartório.

**Escritores `Ausencia` no código de aplicação**

| Operação | Local | Atomic comprovado |
|---|---|---|
| I, criação nova | `app/ponto/services/ausencia.py:385`, save em `:418` | Próprio em `:417`; inclui receivers desse save. |
| U, edição | `app/ponto/services/ausencia.py:508` | Não possui atomic próprio; chamador HTTP em `app/ponto/views.py:2784` também não o delimita. Atomic abrangente nesse caminho: [nao verificado]. |
| U, prazo após rejulgamento | `app/ponto/services/ausencia.py:536` | Não possui atomic próprio; chamada de edição em `:513`, command em `app/ponto/management/commands/rejulgar_status_por_tipo.py:31`. Atomic abrangente nesses chamadores: [nao verificado]. |
| U, estado | `app/ponto/services/ausencia.py:819` | Herda aprovação de irmãos `app/ponto/views.py:2631` → `:2633` → helper `:85` → `ausencia.py:895`; também herda wrappers quando `_reabrir` é chamado neles, conforme abaixo. |
| U, documento | `app/ponto/services/ausencia.py:843` | Sem atomic próprio; HTTP em `app/ponto/views.py:2151` e API em `app/api/views.py:1853`, `:3130`. Atomic abrangente: [nao verificado]. |
| U, carimbos/período | `app/ponto/services/ausencia.py:911` | Aprovação de irmãos sob `app/ponto/views.py:2631`/`:2633`; chama fechamento em `ausencia.py:930`. |
| U, reabertura: documento/descrição/estado/prazo | `app/ponto/services/ausencia.py:1114`, `:1124`, `:1125` → `:819`, `:1132` | **O ramo de reabertura sai em `:291`, antes do atomic próprio `:417`.** Só compartilha transação quando existe wrapper externo comprovado. |
| D, férias inativas | `app/ponto/services/ausencia.py:1174` | Herda wrapper do gravador de `AgendamentoFerias`; cadeia `app/ferias/models.py:436` → `:437` → `:385` → `ausencia.py:1172`/`:1174`. |
| D, férias fora de desejadas | `app/ponto/services/ausencia.py:1197` | Ramo escrito, mas execução não demonstrada: `existentes` já está no intervalo atual em `:1168`, e `desejadas` percorre exatamente esse intervalo em `:1178`–`:1181`. |
| D, janela de férias | `app/ponto/services/ausencia.py:1254` | Correção sob `app/ferias/services.py:375` → `:385`. Também override de delete em `app/ferias/models.py:439` → `:441`; atomic abrangente desse override isolado: [nao verificado]. |
| U em massa, rejeitar férias | `app/ponto/services/ausencia.py:1267` | `app/ferias/services.py:352` → `:356`; **não emite save/signal**, explicitado em `ausencia.py:1259`/`:1260`. |
| D, ensaio | `app/ponto/services/ausencia.py:1274` | `app/ponto/management/commands/zerar_dados_operacionais.py:80` → `:90`; guarda de ensaio em `:27`, reforçada por `ausencia.py:1272`/`:1273`. |
| U, rederivar fim | `app/ponto/services/ausencia.py:1280` | Chamador em `app/ponto/management/commands/curar_ausencia_sem_fim.py:46`; não delimita atomic próprio. Atomic abrangente: [nao verificado]. |

`Ausencia.save` termina no `super().save` em `app/ponto/models.py:906`, sem atomic explícito no override iniciado em `:866`. Não atribuir retenção da trava da ausência durante receivers a saves isolados fora dos wrappers comprovados. `app/ponto/models.py:866`, `:906`.

**Todas as famílias de criação com atomic comprovado**

- Toda criação realmente nova entra em `app/ponto/services/ausencia.py:417` → `:418`, seja pela HTTP administrativa `app/ponto/views.py:2405`, HTTP colaborador `:2526`, API `app/api/views.py:1820`, IA `app/core/views_ia.py:985` → `app/ponto/services/checar_ausencia.py:46`, ou pelos chamadores abaixo.
- Regularização: `app/ponto/services/regularizacao.py:89` → justificativa I/U `:91`/`:106` → laço `:111` → `:118` → `app/ponto/services/ausencia.py:461` → criação I `:418` **ou reabertura U `:291`**.
- Abandono: decorator `app/ponto/management/commands/marcar_abandono_provisorio.py:22` → laço de colaboradores `:43` → laço de dias `:70` → `:76` → `ausencia.py:461` → I `:418` ou U `:291`; rollback sem `--apply` em `marcar_abandono_provisorio.py:98`.
- Veredito falta/justificado: `app/ponto/services/veredito_celula.py:204` → ramo `:309` → `:315` → I `ausencia.py:418` ou U `:291`; depois fecha fio em `veredito_celula.py:325` e lavra célula em `:330`.
- **Materialização de escolha `declara_falta`:** `app/chamados/services/materializacao.py:1142` → ramo escolha `:1148` → `:1044`/`:1045` → `:484` → I `ausencia.py:418` ou U `:291`; resposta sem tipo em `materializacao.py:468`/`:469` e ausência de data em `:472`–`:478` não escrevem ausência.
- Materialização de escape da resposta: `app/chamados/services/disputa_emissao.py:1327` → `:1330` → `app/chamados/services/materializacao.py:519` → I `ausencia.py:418` ou U `:291`. **Esse core não tem atomic próprio**, e a reabertura U não fica protegida pelo atomic de criação nova. Wrapper abrangente dos HTTP web/API `app/chamados/views.py:1283`, `app/api/views.py:2643`: [nao verificado]. Os chamadores atômicos de declaração de hora passam uma hora escolhida, em `app/chamados/services/declaracao_texto.py:368`, `:641`; alcance do ramo de escape nesses chamadores: [nao verificado].
- Férias: `app/ferias/services.py:277` → `:298`; `:315` → `:321`; wrapper adicional `:156` → `:157`; remarcar `:399` → `:406`; corrigir `:375` → `:387`; aprovar solicitação `app/ferias/views.py:717` → `:731` → `app/ferias/services.py:447`. Todos seguem `app/ferias/models.py:436` → `:437` → `:385` → `app/ponto/services/ausencia.py:1186` → I `:418` ou U `:291`.

**Repetições que acrescentam direções aos pares**

- Aprovação de irmãos não acaba na primeira ausência: `app/ponto/views.py:2631` mantém o atomic em todo o laço `:2632`; cada `:2633` executa `app/ponto/services/ausencia.py:819`, eventualmente `:911`, e fechamento em `:930` → `app/ponto/services/fechamento.py:1640`. Havendo ao menos dois irmãos gravados e fechamento atingido no primeiro, existe **Fechamento → Ausencia seguinte**, além de Ausencia → Fechamento. Existência dos irmãos/linhas de fechamento em produção: [nao verificado].
- Os laços de regularização `app/ponto/services/regularizacao.py:111` e abandono `app/ponto/management/commands/marcar_abandono_provisorio.py:43`, `:70` também mantêm uma transação ao gravar ausências subsequentes. Se o cartório da primeira ausência adquire Colaborador/Chamado/CelulaDia, essas operações precedem a próxima gravação de Ausencia. O ramo exato do cartório deve ser juntado às cadeias deste inventário; execução de produção: [nao verificado].
- O espelho ativo de férias percorre dias em `app/ponto/services/ausencia.py:1182`, criando faltantes em `:1186`; sob os wrappers de férias acima, o cartório de um dia precede a ausência seguinte.
- Cancelamento administrativo: atomic `app/ferias/services.py:352` → U Ausencia `:356`/`app/ponto/services/ausencia.py:1267` **sem receiver** → `Agendamento.save :362` → sync → D Ausencia `ausencia.py:1174` com receiver.
- Correção de férias: atomic `app/ferias/services.py:375` → D da janela antiga `:385`/`ausencia.py:1254` com receivers → agendamento `:387` → sync de novas ausências `ausencia.py:1186`, ou reabertura `:291`.

**Caminhos de ensaio e migrations**

- Ensaio: `app/ponto/management/commands/zerar_dados_operacionais.py:80` → D Chamado `:84` → D Ausencia `:90` → D Fechamento `:93`. É um caminho separado, condicionado à guarda `:27`; não descartá-lo do universo da cópia nem apresentá-lo como executável em produção sem dado. A FK `Ausencia.chamado_origem` declara `SET_NULL` em `app/ponto/models.py:784`/`:785`; SQL/ordem internos do Collector: [nao verificado].
- Migrations históricas fazem U em massa: `app/ponto/migrations/0057_a4_backfill_pendente.py:43`, `:65`; `0058_a4_relogio_recomeca.py:42`, `:63`; `0059_a4_d0_no_primeiro_dia_do_cron.py:36`. Registram `RunPython` em `0057:76`, `0058:75`; não há save para expandir pelo cartório nesses updates. Atomic efetivo do runner e execução no HEAD de produção: [nao verificado].

**Limites para consolidar**

`AgendamentoFerias.save` roda sync mesmo quando só muda confirmação ou status: `app/ferias/services.py:491`, `:500`, `:504`, `:522` → `app/ferias/models.py:436`/`:437`. Esses serviços não têm atomic próprio; **cada I novo** continua com o atomic `ausencia.py:417`, mas uma transação única envolvendo agendamento e todos os dias nesses chamadores é [nao verificado].

A FK de Colaborador em Ausencia é `PROTECT`, não cascata de exclusão: `app/ponto/models.py:743`/`:744`. A autorreferência `documento_referencia` declara `SET_NULL`: `:789`/`:790`; ausência de save/signal no SQL interno dessa atualização e sua ordem efetiva dependem do Django instalado: [nao verificado].

## 05/11 21:04 · sessao a77a07cefa5d · - · - · origem=Ronald (tmux)

Expansão síncrona comum: `Ausencia.save/delete → app/ponto/signals.py:147/:152 → _julgar_dias :85/:93 → cartorio.julgar_celula app/ponto/services/cartorio.py:1960/:1989 → julgar_colab :948 → _julgar_colab_corpo`.

Guards: save `raw` para em `signals.py:145`; cartório já ativo para em `signals.py:90`; futuro/sem célula para em `cartorio.py:1974/:1977`; hoje com trabalho e turno ainda não encerrado para em `:1982/:1986`. A seleção dos dias usa `signals.py:123/:124`, com filtro por células quando há mais de três dias em `:125/:127/:128`. Impressão igual pode impedir escrita em `cartorio.py:1203` e `:1362`; ausência de dia da grade para em `:1366`.

**Colaborador L — quatro cadeias confirmadas.**

1. Emissão no corpo: `cartorio.py:1740` ou `:1778 → detectar_ausencias.py:693 → chamados/models.py:863/:866`. Primeiro ramo exige `_cobraria_furo` (`cartorio.py:1696`), não afastado (`:1714`), vínculo/dia de trabalho/marco inicial (`:1717/:1718/:1721`), `apply_` e fora de parcial só lista (`:1725`). Segundo exige `FURO_PARCIAL`, não desligado/fora do piso (`:1762`), não afastado (`:1767`), marcos parciais (`:1769`), sem vivos/fora de parcial só lista (`:1777`). Vetos anteriores ao lock: `detectar_ausencias.py:600/:604/:616/:628/:632/:659`. O lock ocorre mesmo quando `abrir` encontra chamado existente e retorna pelo dedup (`models.py:866/:867/:885`).
2. Fio do emissor: `detectar_ausencias.py:800/:801 → disputa_emissao.py:894/:943 → models.py:866`; condicionado a órfãos restantes e inexistência de disputa aberta (`disputa_emissao.py:895/:942`).
3. Pós-lavra, cluster: `cartorio.py:1822 → portas/celula.py:849/:681/:682 → cartorio.py:990/:994 → cluster_espurio.py:130 → models.py:866`. Exige célula julgada (`cartorio.py:990`), colaborador ativo e escala ativa sem fim (`cluster_espurio.py:117/:119`), assinatura da ata (`:33/:35`), fora de parcial só lista (`:128`). O laço abre sobre `achados`, inclusive chamado existente (`:127/:130`), embora o relatório de pendências também calcule `a_abrir` (`:107`).
4. Pós-lavra, fábrica: `cartorio.py:997 → fabrica_da_celula.py:176 → :139 → disputa_emissao.py:598/:504 → models.py:866`, no ramo sem disputa aberta (`disputa_emissao.py:467/:471/:502`). Mesmo com disputa existente, existe C2: fábrica `:139/:141 → disputa_emissao.py:606/:618/:389/:404/:408/:409 → reconciliador.py:1600/:900 → models.py:866`. Condições C2: `reconciliador.py:798/:802/:810/:822/:830/:840/:845/:859/:872/:876/:893/:896/:898`. Os gates da fábrica estão em `fabrica_da_celula.py:91/:93/:99/:102`; limbo entregue desvia para pauta em `:171/:172`.

**Colaborador U — FCM síncrono, adicional ao lock.**

`cartorio.py:1740/:1778 → detectar_ausencias.py:801 → disputa_emissao.py:876` (`notificar=True` por default) `→ :1123/:1124 → :1543 → fcm_utils.py:106/:93/:41 → colaboradores/fcm.py:61 → colaboradores/services/colaborador.py:88`.

Exige órfãos restantes (`detectar_ausencias.py:800`), não autoencerrar antes do envio (`disputa_emissao.py:1116/:1122`), push não silenciado (`:1521/:1524`), token presente (`fcm_utils.py:104`), erro Firebase considerado inválido (`:89/:90/:92`) e colaboradores ainda com token (`colaboradores/fcm.py:55/:58`). O queryset da limpeza usa token, podendo abranger várias pessoas: `fcm_utils.py:41`, `colaboradores/fcm.py:55/:61`. Resposta real do provedor e linhas atingidas: **[nao verificado]**.

A fábrica usa `push_disputa_silenciado` (`fabrica_da_celula.py:175`), portanto sua chamada de envio para em `disputa_emissao.py:1521/:1524`. `push_supervisao` do emissor (`detectar_ausencias.py:778`) chama apenas o caminho webpush (`push_utils.py:110/:111`), cuja limpeza exclui `PushSubscription` (`:51`), sem atualizar Colaborador. [nao verificado]

**Chamado L/U/I — antes e depois da célula.**

- Antes da lavra, reconciliações: `cartorio.py:1488/:1501/:1522/:1600/:1615 → chamados/models.py:616/:617/:638/:674/:675`, com U somente se gates permitirem (`:648/:651/:620 → :274/:279`). Os ramos são não protestam/não adiados (`cartorio.py:1482/:1488`), marcos inexistentes (`:1500/:1501`), códigos cobertura indevida/sem furo (`:1514/:1522`), duplicatas com fio morto (`:1591/:1599/:1600/:1615`).
- Emissão: `cartorio.py:1740/:1778 → detectar_ausencias.py:693 → models.py:866` seguido de nascimento I `:932`, ou renascimento `:883` conforme guards da porta. Espelhamento de marcos atualiza Chamado: `cartorio.py:1789/:1798 → detectar_ausencias.py:558 → contexto.py:100/:102`; só quando lista mudou (`detectar_ausencias.py:556`).
- Pós-lavra, adiados: `cartorio.py:952/:959/:963`; E2 pode fechar Disputa em `reconciliador.py:601/:602 → models.py:1865 → chamados/signals.py:28/:33/:42 → models.py:674/:279`.
- Pós-lavra, cluster abre/fecha Chamado (`cartorio.py:994 → cluster_espurio.py:124/:130`), e fábrica abre pai/C2 pelas cadeias acima.
- Receivers de fechamento de Chamado podem reabrir outro pai (`chamados/signals.py:92 → disputa_emissao.py:1628`) ou fechar Disputas (`:1695/:1696`) que voltam ao receiver de Chamado (`chamados/signals.py:42`). Não há reentrada infinita do cartório: `_tl.ativo=True` em `cartorio.py:926`, guard em `chamados/signals.py:174`, restauração em `cartorio.py:1075`.

**CelulaDia U — cadeia e ramos.**

Ramo sem batida na vida: `cartorio.py:1162/:1207/:1242/:1264 → portas/celula.py:849/:681/:682`; ramo normal: `cartorio.py:1799/:1802/:1822 → mesma porta`. `save=False` na lavra grava apenas em memória até `carimbar` (`cartorio.py:1802/:1822`; `portas/celula.py:816/:829/:849`). Idempotência pode retornar sem save (`portas/celula.py:838/:841`). Após o corpo, pode haver outras passadas (`cartorio.py:973/:1038`) e a precedência ainda pode salvar célula (`:1997 → :1949 → portas/celula.py:858/:863/:864 → :682`).

O save da célula insere somente evento quando há campos que acordam (`portas/celula.py:680/:683/:684 → :673`); não consome o evento ali. `CelulaDia → Colaborador` está demonstrado pelos ramos pós-lavra cluster/fábrica acima; `Colaborador → CelulaDia` está demonstrado pela emissão antes da lavra (`cartorio.py:1740/:1778 → models.py:866`, depois `cartorio.py:1822 → portas/celula.py:682`). Todos compartilham o atomic externo do escritor de Ausencia quando ele existe; a porta da célula tem atomic próprio em `portas/celula.py:681`.

**Empresa e FechamentoMensal.**

Não identifiquei lock/U de Empresa nem U de FechamentoMensal na expansão síncrona do cartório examinada; alcance além dessa expansão: **[nao verificado]**. Leituras da empresa no cartório estão em `cartorio.py:1145/:1676`; consulta de entrega na fábrica está em `fabrica_da_celula.py:187/:188`. Não constituem escrita.

O consumidor de Ausencia é posterior ao commit: `ponto/signals.py:276/:279` e `:283/:284 → :272`; suas chamadas em `:262/:269/:271` não justificam seta dentro do atomic emissor.

Invalidação de Fechamento conhecida é chamada explícita da porta de aprovação, não do receiver/cartório: escritor `ponto/services/fechamento.py:1635/:1640`; seu próprio comentário confirma chamadores dentro do atomic do ato em `:1626`. Deve entrar no inventário dos escritores/chamadores, que ficou com o outro agente.

## 05/11 21:01 · sessao 01ebc10b506c · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Falta aprovada com minutos=0 recebe respostas incompatíveis conforme o leitor.** O rateio devolve zero e o fechamento grava `horas_falta=0`; o TXT seleciona pela data/tipo/status e emite `len(datas)` dias, sem consultar minutos ou grade. Portanto, se o colaborador passar no crivo e houver rubrica, uma falta de um dia com zero minutos pode virar **1 dia de falta no TXT**. A lista também mostra “1 dia”. (`app/folha/export.py:87`, `app/folha/export.py:121`, `app/folha/export.py:160`, `app/folha/export.py:215`, `app/folha/export.py:732`, `app/ponto/services/fechamento.py:864`, `app/ponto/services/fechamento.py:987`, `app/templates/ponto/ausencias.html:163`)

- **Na folga, a grade mantém folga apesar da falta aprovada**, enquanto `datas_falta` continua contando o lançamento. Isso reforça a divergência anterior entre previsto/horas e dias do TXT. (`app/escala/utils.py:1227`, `app/escala/utils.py:1231`, `app/folha/export.py:169`)

- **Leitores financeiros ainda têm regra própria de parcial.** `TIPOS_PAGOS and minutos` classifica atestado de 720 minutos como crédito parcial e impede que justifique o dia; o juiz `cobre_parte_do_dia` considera tanto 0 quanto 720 como dia inteiro. O diff migra a lista, mas deixa esses leitores. (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/turnos.py:2583`, `app/ponto/turnos.py:2600`)

Comportamento de atestado aprovado:

| Leitor | Minutos=0 | Minutos=720 |
|---|---|---|
| Juiz de cobertura | Inteiro | Inteiro (`app/ponto/turnos.py:2598`) |
| Lista | “1 dia” | “1 dia” (`app/ponto/views.py:2240`, `app/templates/ponto/ausencias.html:163`) |
| Fechamento | Justifica dia; não soma crédito | Soma 720 de crédito; pula justificação (`app/ponto/services/fechamento.py:665`, `app/ponto/services/fechamento.py:685`) |
| `efeitos_da_ausencia` | Justifica dia | Crédito 720; pula justificação (`app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/efeito_ausencia.py:51`) |
| `creditos_por_dia` | Nenhum crédito | Crédito 720 no início do registro (`app/ponto/services/credito_parcial.py:24`) |
| Teto de pontualidade | Dia inteiro | Dia inteiro: ambos entram pelo mesmo juiz, criando resposta diferente da justificação anterior (`app/ponto/turnos.py:2668`, `app/ponto/turnos.py:2689`, `app/ponto/services/fechamento.py:683`) |
| Drawer | Oculta campo de detalhes “Minutos”; não abre edição de período parcial | Mostra 720 em detalhes; não abre edição de período parcial (`app/templates/ponto/partials/painel_ausencia.html:125`, `app/templates/ponto/partials/painel_ausencia.html:296`, `app/ponto/views.py:2084`) |
| Palavra/histórico | “Atestado (abonado)” | “Atestado 12h (abonado)” (`app/ponto/services/dia_decidido.py:230`, `app/ponto/services/dia_decidido.py:488`) |
| API/CSV | Devolve zero cru | Devolve 720 cru (`app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`) |

O TXT efetivo não consulta diretamente minutos de atestado: emite eventos do fechamento e faltas decididas. A função `datas_abonadas` conta dias sem olhar minutos, mas não localizei chamador de produção dela; não afirmar emissão atual da rubrica 8932. (`app/folha/export.py:201`, `app/folha/export.py:722`, `app/folha/export.py:732`; chamador de produção de `datas_abonadas`: [não verificado])

**Censo consolidado de leitores fora do diff:**

- Minutos: propriedade `horas`; admin; rateio de falta; fechamento; efeito da ausência; crédito parcial; palavra/cartão/histórico; payload API; CSV; fotografias de auditoria; drawer. (`app/ponto/models.py:865`, `app/ponto/admin.py:44`, `app/folha/export.py:121`, `app/ponto/services/fechamento.py:665`, `app/ponto/services/efeito_ausencia.py:44`, `app/ponto/services/credito_parcial.py:24`, `app/ponto/services/dia_decidido.py:423`, `app/ponto/services/dia_decidido.py:488`, `app/ponto/services/dia_decidido.py:512`, `app/api/views.py:1878`, `app/ponto/services/atestados_lote.py:128`, `app/ponto/views.py:2347`, `app/ponto/views.py:2730`, `app/ponto/views.py:2768`, `app/ponto/views.py:2811`, `app/ponto/services/ausencia.py:446`, `app/templates/ponto/partials/painel_ausencia.html:125`)
- Tipo/status na cobertura: `_pelo_efeito`, cobertura unitária/período, sobreposição, afastamento e vigência. (`app/ponto/turnos.py:2509`, `app/ponto/turnos.py:2521`, `app/ponto/turnos.py:2536`, `app/ponto/turnos.py:2705`, `app/ponto/turnos.py:2733`, `app/ponto/turnos.py:2793`, `app/ponto/turnos.py:3087`)
- Tipo/status no estado apresentado e portas: propriedades `esta_aberta`, `exige_documento`, `pode_aprovar`; decisão visual/histórico; painel/lista; filtros de aprovação/rejeição/cancelamento; API e admin. (`app/ponto/models.py:711`, `app/ponto/models.py:723`, `app/ponto/models.py:732`, `app/ponto/services/dia_decidido.py:453`, `app/ponto/services/dia_decidido.py:476`, `app/ponto/services/dia_decidido.py:514`, `app/ponto/services/dia_decidido.py:542`, `app/ponto/services/dia_decidido.py:602`, `app/ponto/views.py:2096`, `app/ponto/views.py:2185`, `app/ponto/views.py:2279`, `app/ponto/views.py:2313`, `app/ponto/views.py:2342`, `app/ponto/views.py:2603`, `app/ponto/views.py:2667`, `app/ponto/views.py:2684`, `app/api/views.py:1873`, `app/ponto/admin.py:44`)
- Precedência/grade/juízo de furo/cartório: catálogo vivo, `fatos_do_periodo`, falta classificada, grade, dispensas e sinais. (`app/ponto/precedencia.py:127`, `app/ponto/precedencia.py:273`, `app/ponto/precedencia.py:339`, `app/ponto/precedencia.py:408`, `app/escala/servico_jornada.py:340`, `app/escala/utils.py:1209`, `app/escala/utils.py:1231`, `app/ponto/services/cartorio.py:355`, `app/ponto/signals.py:124`, `app/ponto/signals.py:250`)
- Chamados/cobrança: materialização, devolução de absorvidas, silêncio de ausência, histórico, furos diários. (`app/chamados/services/materializacao.py:78`, `app/chamados/services/materializacao.py:602`, `app/chamados/services/materializacao.py:638`, `app/chamados/services/silencio_ausencia.py:65`, `app/chamados/services/silencio_ausencia.py:77`, `app/chamados/services/silencio_ausencia.py:93`, `app/chamados/services/historico.py:129`, `app/escala/services/furos_diarios.py:223`)
- Relatórios/calendário/inteligência/férias: relatórios por tipo/status; alertas médicos; calendário; resumo e consultas de inteligência; períodos de férias e detalhe de colaborador. (`app/relatorios/views.py:197`, `app/relatorios/views.py:277`, `app/ponto/services/alertas_ausencia.py:48`, `app/colaboradores/services/calendario.py:674`, `app/inteligencia/resumo.py:39`, `app/inteligencia/views.py:71`, `app/inteligencia/views.py:239`, `app/inteligencia/views.py:516`, `app/ferias/models.py:144`, `app/colaboradores/services/detalhe.py:104`)
- Vigias/comandos: rejeitada/viva, aguardando decisão/documento, aprovada sem documento/decisor, tipo válido, ausência versus veredito, ausência em folga, ausência sem fim e inteligência. (`app/ponto/services/vigia_ausencia.py:59`, `app/ponto/services/vigia_ausencia.py:104`, `app/ponto/services/vigia_ausencia.py:108`, `app/ponto/services/vigia_ausencia.py:115`, `app/ponto/services/vigia_ausencia.py:120`, `app/ponto/services/vigia_ausencia_veredito.py:46`, `app/ponto/services/vigia_ausencia_veredito.py:54`, `app/ponto/management/commands/rejulgar_ausencia_em_folga.py:56`, `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:41`, `app/ponto/management/commands/ausencia_bloqueante_sem_fim.py:27`, `app/ponto/management/commands/rejulgar_status_por_tipo.py:23`, `app/ponto/management/commands/curar_ausencia_sem_fim.py:29`, `app/inteligencia/management/commands/calcular_scores.py:12`, `app/inteligencia/management/commands/gerar_alertas.py:55`)
- Outra regra própria de apresentação: formulário ainda diz “>0 = parcial”, contrariando o juiz para 720; espelho ainda testa o status inexistente `pendente`. (`app/templates/ponto/lancar_ausencia.html:70`, `app/ponto/turnos.py:2600`, `app/templates/ponto/espelho.html:191`, `app/ponto/models.py:700`)

**Achado secundário preexistente de range:** `save` soma previsto dia a dia, mas o rateio financeiro divide igualmente por dias corridos; um range com previstos 420/420/0 vira 280/280/280. O teste novo só cobre três dias iguais e não detecta isso. Os dois sítios de chamados auditados criam dia único. (`app/ponto/models.py:882`, `app/ponto/models.py:885`, `app/folha/export.py:118`, `app/folha/export.py:121`, `app/chamados/tests/test_h06_falta_pela_grade.py:128`, `app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:519`)

**Testes:** o teste de range não constitui RED próprio desta troca, pois chama a porta já existente sem passar pelos sítios alterados; os testes dos dois sítios e da lista atingem os comportamentos alterados. O novo caso de atestado chama diretamente o helper com status aprovado, enquanto o fluxo real de escape deixa o estado inicial decidir, normalmente aguardando documento. (`app/chamados/tests/test_h06_falta_pela_grade.py:76`, `app/chamados/tests/test_h06_falta_pela_grade.py:98`, `app/chamados/tests/test_h06_falta_pela_grade.py:110`, `app/chamados/tests/test_h06_falta_pela_grade.py:128`, `app/chamados/tests/test_h06_falta_pela_grade.py:142`, `app/chamados/services/materializacao.py:580`)

Não executei testes Django/banco nem escrevi arquivos; resultado de execução da suíte: [não verificado]. [nao verificado]

## 05/11 21:00 · sessao a3ff12ba523a · - · - · origem=Ronald (tmux)

Censo H06 de escritores concluído por leitura e AST, sem banco e sem gravação de arquivos. A varredura canônica atribuiu 14 escritas de `Ausencia`, todas em `app/ponto/services/ausencia.py`; o próprio censo documenta o limite das chamadas não atribuíveis (`app/core/censo_escritas.py:41`).

- **Derivação exata:** `status == 'aprovada'`, `tipo == 'falta'` e `not self.minutos`. Soma `minutos_previstos_do_dia` inclusivamente entre `data` e `data_fim or data`; não requer detectar mudança de estado e preserva números não zero (`app/ponto/models.py:877`).
- **Dois sítios curados:** omitem minutos e passam pela porta com default zero; a porta instancia e executa `save()` inteiro (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:130`, `app/ponto/services/ausencia.py:385`, `app/ponto/services/ausencia.py:418`).
- **Aprovação posterior:** `mudar_estado` inclui explicitamente `minutos`, `data_fim`, `dias_corridos` e `aprovada_em` em `update_fields`; portanto persiste a derivação (`app/ponto/services/ausencia.py:818`).
- **Edição para falta aprovada:** escreve o tipo, zera/recebe minutos e executa `save()` inteiro; deriva quando o valor enviado for zero (`app/ponto/services/ausencia.py:497`, `app/ponto/services/ausencia.py:507`).
- **Atestado zero aprovado:** não entra na derivação, por teste literal do tipo; esse caminho existe por desenho (`app/ponto/models.py:877`, `app/chamados/services/materializacao.py:523`).
- **Bypass encontrado:** o único `QuerySet.update` atribuível de Ausencia rejeita exclusivamente férias; não cria/aprova falta ou atestado (`app/ponto/services/ausencia.py:1265`). Admin é somente leitura (`app/ponto/admin.py:38`). Os receivers julgam/reconciliam outros estados e não escrevem minutos/status/tipo de Ausencia (`app/ponto/signals.py:143`, `app/ponto/signals.py:250`).

Chamadores que fornecem tipo/minutos/status:

| Sítio | Entrada |
|---|---|
| `app/ponto/views.py:2412` | Administração: tipo e minutos recebidos, aprovada |
| `app/ponto/views.py:2533` | Colaborador: tipo/minutos recebidos, estado inicial pela porta |
| `app/api/views.py:1820` | API: tipo/minutos recebidos, estado inicial pela porta |
| `app/chamados/services/materializacao.py:484` | Mapa de resposta, minutos omitidos, estado inicial |
| `app/chamados/services/materializacao.py:520` | Tipo/status recebidos do fluxo, minutos omitidos |
| `app/ponto/services/veredito_celula.py:315` | Falta ou tipo informado, minutos omitidos, aprovada |
| `app/ponto/services/checar_ausencia.py:46` | Copiloto: tipo informado, minutos omitidos, aprovada |
| `app/ponto/services/regularizacao.py:118` | Falta zero, estado inicial |
| `app/ponto/management/commands/marcar_abandono_provisorio.py:76` | Falta zero, estado inicial |
| `app/ferias/models.py:385` → `app/ponto/services/ausencia.py:1186` | Férias zero aprovadas |
| `app/ponto/views.py:2791` | Edição de tipo e minutos pela porta |
| `app/ponto/views.py:85` | Casca comum de decisão; usada em `:2287`, `:2316`, `:2348`, `:2640`, `:2670`, `:2698`, `:2731` |
| `app/ponto/views.py:2581` | Reverter rejeição pela porta |
| `app/ferias/services.py:356` | Rejeição de férias pela porta |
| `app/ponto/management/commands/curar_ausencia_sem_fim.py:46` | `rederivar_fim`: save inteiro (`app/ponto/services/ausencia.py:1280`) |
| `app/ponto/management/commands/rejulgar_status_por_tipo.py:31` | Rejulga status aberto pela porta |

Fora de `app`, há cópia da tabela para sombra, sem decisão de domínio: `bin/sombra_incremental.sh:36`. Nenhum outro escritor por nome/constructor/ORM foi identificado; ausência absoluta de escrita por SQL dinâmico ou objetos não atribuíveis é [não verificado].

**Sem grade:** sem escala vigente, sem tipo, dia com trabalho diferente de `True`, ou sem marcos de início/fim retorna zero (`app/escala/utils.py:2282`, `app/escala/utils.py:2287`, `app/escala/utils.py:2291`). **Sem célula não significa necessariamente zero**: há fallback preexistente ao cálculo do vínculo e ao template (`app/escala/models.py:1096`, `app/escala/models.py:1133`). A derivação nova reutiliza essa autoridade, sem introduzir fallback próprio (`app/ponto/models.py:881`).

**Mapa:** único escritor funcional é a declaração em `app/chamados/models.py:3018`; único leitor funcional identificado é `app/chamados/services/materializacao.py:468`. As demais referências são teste/documentação (`app/chamados/tests/test_h06_selo_minutos_sem_literal.py:50`).

**Migration:** a única 0077 de `ponto` é a fornecida; depende de 0076 e contém somente `AlterField` de `minutos` com novo help_text (`app/ponto/migrations/0077_ausencia_minutos_help_text_grade.py:8`, `:12`). Tipo/default permanecem iguais à criação original do campo (`app/ponto/migrations/0008_ausencia.py:23`). SQL efetivamente produzido pelo Django é [não verificado].

**Outro defeito preexistente:** chamada direta a `Ausencia.save` com `dias_corridos > 1`, `data_fim=None`, falta aprovada e minutos zero deriva primeiro somente `data`, depois constrói `data_fim`; o total fica apenas do primeiro dia (`app/ponto/models.py:882`, `app/ponto/models.py:902`). Os dois sítios curados não fornecem range, e a porta normal calcula `data_fim` antes do save (`app/chamados/services/materializacao.py:484`, `app/chamados/services/materializacao.py:520`, `app/ponto/services/ausencia.py:197`).

## 05/11 21:00 · sessao c389e01a9572 · - · - · origem=Ronald (tmux)

H12 — núcleo coerente, com duas divergências antigas de leitura e um corte ainda não realizado na cópia.

- `sai_pela_rescisao` responde exclusivamente `data_demissao != None and data_demissao <= fim_janela`; é extração da porta existente, chamada por `classificar_export` e `montar_resumos`, sem segunda fórmula nesses consumidores. `app/folha/export.py:54`, `app/folha/export.py:60`, `app/folha/export.py:858`, `app/ponto/services/fechamento_lista.py:109`.
- A janela coincide: export calcula por `empresa`; resumos calcula por `colab.empresa`, com cache por `empresa_id`; ambos chamam `janela_competencia`, que delega a `janela_fechamento`, cujo corte vem da empresa e é limitado a `[2,28]`. `app/folha/export.py:47`, `app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:103`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`.
- `_fech_linha.html` usa `r.rescisao` tanto no filtro/cor quanto no badge; `fechamento_mensal` soma a mesma chave; `fechamento.html` mostra o contador e seu filtro. `app/templates/ponto/_fech_linha.html:2`, `app/templates/ponto/_fech_linha.html:4`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:65`.
- `rescisao` só é escrita no dict de `montar_resumos`; seus leitores de produção são os pontos acima; `n_rescisao` só é escrito na view e lido no template acima. `app/ponto/services/fechamento_lista.py:109`, `app/ponto/views.py:1215`, `app/templates/ponto/fechamento.html:65`.

Censo de `Colaborador.data_demissao`:

- Declaração: `app/colaboradores/models.py:325`.
- Escritores de negócio: desligamento agendado pela porta `gravar`; desligamento efetivo e reativação pela porta `salvar_inteiro`. `app/colaboradores/services/desligamento.py:85`, `app/colaboradores/services/desligamento.py:105`, `app/colaboradores/services/desligamento.py:108`, `app/colaboradores/services/desligamento.py:211`, `app/colaboradores/services/desligamento.py:214`.
- Escritor genérico: operação `situacao` autoriza o campo; `gravar`, `salvar_campos`, `salvar_inteiro`, `atualizar_em_lote` e construtor `criar_colaborador` persistem objetos/campos recebidos. Não encontrei outro sítio de produção atribuindo explicitamente `data_demissao`. `app/colaboradores/services/colaborador.py:32`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/colaborador.py:65`, `app/colaboradores/services/colaborador.py:73`, `app/colaboradores/services/colaborador.py:80`, `app/colaboradores/services/colaborador.py:91`.
- O Django admin é somente leitura por herança. `app/colaboradores/admin.py:48`.
- Leitores financeiros: predicado H12 e detalhe do export; `prontidao`; recorte de retenção da célula; raio-X da folha; matriz de holerites. `app/folha/export.py:60`, `app/folha/export.py:862`, `app/folha/export.py:1060`, `app/folha/export.py:1079`, `app/folha/export.py:1200`, `app/folha/export.py:1229`, `app/folha/views.py:612`, `app/folha/views.py:658`, `app/holerite/matriz.py:88`.
- Leitores de universo/cadastro: fechamento, disponibilidade para substituição, grade da célula, alimentadores de cadastro, comandos de diagnóstico/lavra. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:429`, `app/ponto/utils.py:180`, `app/escala/services/leitor_celula.py:351`, `app/escala/services/leitor_celula.py:372`, `app/escala/services/cadastro_realidade.py:444`, `app/ponto/services/cartorio.py:1096`, `app/ponto/supra_juiz.py:177`, `app/ponto/supra_juiz.py:194`, `app/ponto/management/commands/supra_juiz.py:178`, `app/ponto/management/commands/emitir_furo_retroativo.py:73`.
- Leitores do ciclo do desligamento/vínculo: validação de idempotência, texto do veto de novo vínculo, efetivação agendada, fechamento retroativo de vínculo e tripwire. `app/colaboradores/services/desligamento.py:18`, `app/colaboradores/services/vinculo.py:92`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:38`, `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:51`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:41`, `app/colaboradores/management/commands/fechar_vinculo_de_desligado.py:56`, `app/colaboradores/management/commands/tripwire_vinculo_orfao.py:55`.
- Apresentação e entrada: edição de colaborador e badge do fechamento; o catálogo de chamados usa uma chave contextual homônima para revisão de desligamento, não lê diretamente o campo do colaborador. `app/templates/colaboradores/editar.html:274`, `app/templates/colaboradores/editar.html:282`, `app/templates/ponto/_fech_linha.html:4`, `app/chamados/catalogo/modulos.py:141`.

Leitores de rescisão fora do diff:

- Raio-X separa rescisões pelo veredito do juiz; relatórios “quem cobrar” também; pendências de cadastro consultam o mesmo juiz; retrato recebe os itens de `fora_da_folha`, que chama `classificar_export`. Portanto migram automaticamente com H12. `app/folha/views.py:589`, `app/folha/views.py:594`, `app/relatorios/views.py:1658`, `app/relatorios/views.py:1659`, `app/relatorios/views_pend.py:58`, `app/folha/fora_da_folha.py:119`, `app/folha/retrato_folha.py:1157`, `app/folha/retrato_folha.py:1181`.
- `prontidao` ainda dá motivo `"rescisao"` apenas quando a demissão é **anterior ao início** da janela; demissão dentro da janela com denominador zero pode dar `"sem_escala"`, embora H12 dê `rescisao=True`. É regra própria sobrevivente, mas responde ao motivo de cegueira do denominador, não decide entrada no TXT. `app/folha/export.py:1060`, `app/folha/export.py:1081`, `app/folha/export.py:1100`.
- O raio-X ainda acrescenta `†deslig.` só pela presença da data; uma demissão futura fora da janela pode ser anotada como desligamento. Essa anotação não decide o balde de rescisão, que lê o juiz. `app/folha/views.py:658`, `app/folha/views.py:594`.
- Universo da tela exclui desligado cuja demissão antecede o início; o classificador consulta todos os fechamentos gravados da competência e marca esse registro antigo como rescisão. Assim, tela e export podem ter universos diferentes, ainda que o predicado H12 seja único. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430`, `app/ponto/views.py:1114`, `app/folha/export.py:777`, `app/folha/export.py:858`.

`janela_competencia` — demais leitores de produção:

- Export: ausências por tipo/falta, grade do fechamento, férias com batida, classificação, prontidão e retenção. `app/folha/export.py:166`, `app/folha/export.py:185`, `app/folha/export.py:337`, `app/folha/export.py:486`, `app/folha/export.py:807`, `app/folha/export.py:1058`, `app/folha/export.py:1073`, `app/folha/export.py:1266`.
- Prévia, validação PDF, views da folha e serviços auxiliares. `app/folha/previa.py:42`, `app/folha/validacao_pdf.py:42`, `app/folha/views.py:529`, `app/folha/views.py:588`, `app/folha/services/vigia_pausa.py:123`, `app/folha/services/sem_furo.py:47`.
- Relatórios, cartão e fechamento. `app/relatorios/services.py:214`, `app/relatorios/cartao_pela_celula.py:24`, `app/ponto/services/fechamento_lista.py:104`.
- Vigia/comandos: pareador, censo, geração de células, lavra de status e mapa de divergência. `app/ponto/services/vigia_pareador_celula.py:59`, `app/ponto/management/commands/censo_col900.py:89`, `app/ponto/management/commands/gerar_celulas.py:176`, `app/ponto/management/commands/lavrar_status_competencia.py:40`, `app/ponto/management/commands/mapa_divergencia.py:61`, `app/ponto/management/commands/mapa_divergencia.py:93`.

Classes de reprovação:

- Nenhum juiz adicional de negócio identificado em H12: `sai_pela_rescisao` concentra a condição que `classificar_export` lê; o registro já atribui a este último entrada TXT e travas. `app/folha/export.py:54`, `app/folha/export.py:858`, `app/core/juizes.py:298`, `app/core/juizes.py:299`.
- Travas continuam computadas antes da saída antecipada por rescisão e presentes em todos os itens; H12 altera apenas a condição do primeiro gate, preservando precedência. `app/folha/export.py:822`, `app/folha/export.py:840`, `app/folha/export.py:858`, `app/folha/export.py:864`.
- As três portas continuam lendo `travas_aprovacao` do classificador: aprovação em lote, tranca pelo lote de vazios e aprovação individual. `app/ponto/views.py:1296`, `app/ponto/views.py:1298`, `app/ponto/views.py:1409`, `app/ponto/views.py:1506`, `app/ponto/views.py:1508`.
- RED por inspeção: os casos de demissão posterior falhariam na base mostrada no diff (`is not None`); os três testes da linha também falhariam na base, por chave ausente/badge pela data/contador igual a dois. Execução na base: [não verificado]. `app/folha/tests/test_rescisao_modulo_proprio.py:75`, `app/folha/tests/test_rescisao_modulo_proprio.py:79`, `app/ponto/tests/test_h12_rescisao_na_linha.py:41`, `app/ponto/tests/test_h12_rescisao_na_linha.py:45`, `app/ponto/tests/test_h12_rescisao_na_linha.py:51`.
- Os testes antes/dentro/último dia permaneceriam verdes na base; são regressão, não evidência RED. `app/folha/tests/test_rescisao_modulo_proprio.py:66`, `app/folha/tests/test_rescisao_modulo_proprio.py:69`, `app/folha/tests/test_rescisao_modulo_proprio.py:72`.
- H12 não introduz leitura de relógio nem dependência visível das funções reservadas ao POUSO B: o novo predicado só recebe colaborador e fim, e os leitores fornecem mês/ano explícitos. `app/folha/export.py:54`, `app/folha/export.py:807`, `app/ponto/services/fechamento_lista.py:40`, `app/ponto/services/fechamento_lista.py:104`.

Corte antigo não realizado:

- O corte `DESLIGADO-FORA-DA-JANELA-APOSENTA-A-LINHA` manda aposentar a linha anterior à janela, com `status=encerrado`, `derivado_em=NULL`, usando o universo único. A cópia ainda só oferece aberto/aprovado/exportado; o vigia segue contando todos os NULL e sua docstring diz que a aposentadoria espera dono. Isso é dívida preexistente da árvore, não deve ser apresentado como alteração de H12: o escopo fornecido preserva demissões anteriores no predicado `<= fim`. `app/docs/CORTES.md:270`, `app/ponto/models.py:399`, `app/ponto/services/fechamento.py:1680`, `app/ponto/services/fechamento.py:1692`, `app/ponto/services/fechamento.py:1700`, `app/folha/export.py:60`.

## 05/11 20:58 · sessao df69f701d606 · H12H06-SEPARA-TRAVA · censo · origem=Code

**Encontrei 24 chamadas executáveis de `select_for_update`. Há ordens inversas no código, inclusive `PeriodoAquisitivo → AgendamentoFerias` e `AgendamentoFerias → PeriodoAquisitivo`.** As referências estão no inventário abaixo e em `app/ferias/services.py:355`, `app/ferias/services.py:362`, `app/ferias/services.py:406`, `app/ferias/services.py:409`.

**Não apresento os itens 2–4 como censo integral fechado:** a exaustividade das expansões transitivas e os totais globais por par permanecem **[nao verificado]**. Distingo abaixo chamadas explícitas, sequências comprovadas e limites, sem transformar ordem textual de operações em primeira aquisição comprovada de todas as linhas.

A correspondência desta cópia ao HEAD/hash informado é **[nao verificado]**: a cópia disponível não contém metadados Git utilizáveis. Execuções, linhas atingidas, esperas e deadlocks de produção são **[nao verificado]**.

Notação: `L` = `select_for_update`; `U` = atualização de existente; `D` = exclusão; `I` = inserção. `I` não será contado automaticamente como trava de linha preexistente de outra entidade.

**1. Inventário de `select_for_update`**

Todas as chamadas abaixo usam `()`, exceto a linha com `of=`. Não encontrei chamadas executáveis adicionais com `nowait`, `skip_locked` ou `no_key` na varredura da cópia; geração dinâmica/dependências externas: **[nao verificado]**.

| Arquivo:linha | Função | Modelo/alvos | Argumentos |
|---|---|---|---|
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100` | `Command._rodar` | `ChamadoColaborador` | `()` |
| `app/chamados/models.py:674` | `ChamadoColaborador._recarregar_estado_se_mudou` | `ChamadoColaborador`, por `type(self)` | `()` |
| `app/chamados/models.py:866` | `ChamadoColaborador.abrir` | `Colaborador` | `()` |
| `app/chamados/services/abertura.py:164` | `abrir_chamado_para_colaborador` | `Colaborador` | `()` |
| `app/chamados/services/disputa_emissao.py:467` | `_abrir_ou_agregar_disputa` | `DisputaSupervisao` | `()` |
| `app/chamados/services/disputa_emissao.py:1156` | `criar_disputa_ancora_de_chamado` | `DisputaSupervisao` | `()` |
| `app/chamados/services/fio.py:762` | `_costura_nao_mata_o_fio` | `type(chamado)`; `ChamadoColaborador` nos caminhos examinados | `()` |
| `app/chamados/services/relavra_via.py:221` | `desfazer_carimbo_sem_lastro` | `PerguntaDisputa` **e** `DisputaSupervisao` | `of=('self', 'disputa')` |
| `app/colaboradores/models.py:431` | `Colaborador.save` | `Colaborador` | `()` |
| `app/core/services/aplicacao_convencao.py:115` | `cadastrar` | `AplicacaoConvencao` | `()` |
| `app/ferias/views.py:718` | `avaliar_solicitacao` | `SolicitacaoFerias` | `()` |
| `app/holerite/management/commands/processar_lote_holerite.py:54` | `Command._rodar` | `LoteHolerite`, alias `L` | `()` |
| `app/holerite/management/commands/processar_lote_holerite.py:124` | `Command._rodar` | `LoteHolerite` | `()` |
| `app/ponto/portas/he.py:174` | `decidir_he` | `DecisaoHE` | `()` |
| `app/ponto/portas/he.py:318` | `recusar_ponta_pequena` | `DecisaoHE` | `()` |
| `app/ponto/portas/he.py:445` | `revogar_recusa_do_sistema` | `DecisaoHE` | `()` |
| `app/ponto/portas/he.py:555` | `autorizar_em_lote` | `DecisaoHE` | `()` |
| `app/ponto/services/justificativa.py:38` | `encerrar_chamado_de_origem` | `ChamadoColaborador` | `()` |
| `app/ponto/services/justificativa.py:70` | `_encerrar_bloqueio_de_entrada` | `ChamadoColaborador` | `()` |
| `app/ponto/services/justificativa.py:114` | `travar_origem_antes` | `ChamadoColaborador` | `()` |
| `app/ponto/services/justificativa.py:127` | `aprovar_justificativa_dp` | `Justificativa` | `()` |
| `app/ponto/services/justificativa.py:262` | `rejeitar_justificativa_dp` | `Justificativa` | `()` |
| `app/ponto/services/justificativa.py:329` | `materializar_saida_dp` | `Justificativa` | `()` |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | `Justificativa` | `()` |

Particularidades que alteram a interpretação:

- `_recarregar_estado_se_mudou` e `_costura_nao_mata_o_fio` só aplicam `select_for_update` quando `connection.in_atomic_block` está ativo: `app/chamados/models.py:673`, `app/chamados/services/fio.py:761`.
- `of=('self', 'disputa')` pede os dois alvos **na mesma consulta**. A tupla não comprova `PerguntaDisputa → DisputaSupervisao`; ordem interna **[nao verificado]**. `CelulaDia` aparece no `select_related`, mas está fora do `of`: `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:222`.
- `Colaborador.save` encerra seu atomic interno antes de `super().save`. A trava e a gravação só ficam numa transação abrangente quando existe atomic externo: `app/colaboradores/models.py:429`, `app/colaboradores/models.py:436`, `app/colaboradores/models.py:439`.
- `ChamadoColaborador.criar` não contém a trava de Colaborador de `abrir`: `app/chamados/models.py:1055`, `app/chamados/models.py:1056`, `app/chamados/models.py:866`.
- Consulta de trava que não encontra linha não comprova aquisição sobre entidade existente. Isso importa nos ramos “disputa ausente → criar”: `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:1156`, `app/chamados/services/disputa_emissao.py:1163`.

**2. Locks e escritas implícitas**

| Local | Operação e abrangência |
|---|---|
| `app/ponto/services/vigia_de_hora.py:99` | `pg_try_advisory_lock(7180000 + empresa)`, chave em `:97`; liberação em `:136`. O corpo chama julgamentos em `:131`; não há atomic único abrangendo esse corpo. |
| `app/chamados/reconciliador.py:1224` | `pg_try_advisory_lock(chave)`, chave por schema em `:1210`; liberação em `:1231`. |
| Ausência na varredura | Não encontrei ocorrência executável de `LOCK TABLE`, `pg_advisory_lock` bloqueante ou `pg_advisory_xact_lock`; SQL externo/dinâmico: **[nao verificado]**. |

O inventário confirmado de atualizações/exclusões sob atomic — próprio ou externo identificado — é:

| Entidade | Escrita | Atomic/caminho demonstrado |
|---|---|---|
| `ChamadoColaborador` | `save` em `app/chamados/models.py:279` | `_gravar_estado`, atomic em `:274`; gates abrangentes em `:616`, `:744`, `:1542`, `:1558`. |
| `ChamadoColaborador` | `update` em `app/chamados/models.py:602` | Receiver de resolução chamado pelo save acima: `app/chamados/signals.py:72`. |
| `ChamadoColaborador` | `save` em `app/chamados/services/contexto.py:77`, `:102`, `:124`, `:159` | Atomics em `:75`, `:100`, `:122`, `:157`. |
| `ChamadoColaborador` | `save` em `app/chamados/services/vinculos.py:88`, `:99`, `:114`, `:131`, `:142` | Atomics em `:86`, `:97`, `:112`, `:128`, `:139`. |
| `ChamadoColaborador` | `save` em `app/chamados/services/texto.py:35` | Atomic em `:31`. |
| `ChamadoColaborador` | Arquivamento `save` em `app/chamados/services/arquivo.py:170` | Chamadores atômicos: `app/chamados/views_arquivar.py:134`, `app/chamados/services/silencio_isentos.py:41`. |
| `PerguntaDisputa` | `save` em `app/chamados/models.py:2609`, `:2644`, `:2652`; `update` em `:2663` | Chamadas sob os atomics de fechamento `:1846`, costura `app/chamados/services/fio.py:837`, reabertura `app/chamados/services/disputa_emissao.py:1401`, materialização `app/chamados/services/materializacao.py:1142`. |
| `PerguntaDisputa` | Lavra `save` em `app/chamados/veredito_lavrado.py:231` | Relavra dentro do atomic de estado: `app/chamados/models.py:290`, `app/chamados/models.py:295`. |
| `PerguntaDisputa` | Validação `save` em `app/chamados/services/validacao.py:133`; devolução em `:190` | Atomics em `:262`, `:289`, `:395`. |
| `DisputaSupervisao` | `save` em `app/chamados/models.py:1865` | `fechar`, atomic em `:1846`. |
| `DisputaSupervisao` | `save` em `app/chamados/services/disputa_emissao.py:501`, `:923`, `:971`, `:999`, `:1090`, `:1191` | Atomics em `:466`, `:898`, `:1148`. |
| `RegularizacaoItemBatida` | `save` em `app/chamados/services/regularizacao_ext.py:139`, `:163`, `:188` | Atomic abrangente em `:121`. |
| `RegularizacaoExterna` | `save` em `app/chamados/services/regularizacao_ext.py:200`, `:257` | Atomics em `:121`, `:235`. |
| `Batida` | Correção `save` em `app/chamados/services/regularizacao_ext.py:155`; exclusão em `:136` | Atomic externo em `:121`; savepoint adicional em `:152`. |
| `Batida` | Atualização em `app/ponto/registro_batida.py:254` | Por exemplo, flip sob `app/ponto/services/veredito_celula.py:204`/`:295` ou command `app/ponto/management/commands/flip_automatico.py:65`/`:68`. |
| `CelulaDia` | Atualizações em `app/ponto/portas/celula.py:239`, `:270` | Regeneração, atomic em `:212`. |
| `CelulaDia` | Atualização em `app/ponto/portas/celula.py:578` | Reversão, atomic em `:569`. |
| `CelulaDia` | Atualização em `app/ponto/portas/celula.py:682` | `salvar_e_emitir`, atomic em `:681`. |
| `CelulaDia` | Atualizações em `app/ponto/portas/celula.py:645`, `:655` | Command abrangente `app/ponto/management/commands/gerar_celulas.py:310`, chamada em `:311`. |
| `TurnoMaterializado` | DELETE SQL em `app/ponto/nucleo.py:80`; reposição em `:59` | `recompute_turnos`, atomic em `:57`. |
| `FuroDiario` | `update_or_create` em `app/ponto/portas/celula.py:906`; exclusão em `:914` | Atomic em `:904`. |
| `DecisaoHE` | `save` em `app/ponto/portas/he.py:84`, `:375` | Decoradores em `:137`, `:268`, `:392`; lote em `:548`. |
| `Justificativa` | `save` em `app/ponto/services/justificativa.py:193`, `:272`, `:371` | Atomics em `:125`, `:258`, `:327`. |
| `Justificativa` | `save` em `app/ponto/views.py:1800`, `:1833` | Atomic por item em `:1793`. |
| `Justificativa` | `save` em `app/chamados/reconciliador.py:535` | `_retratar`, atomic em `:522`. |
| `Justificativa` | `save` em `app/ponto/services/regularizacao.py:106` | Atomic em `:89`. |
| `Ausencia` | Atualização de status em `app/ponto/services/ausencia.py:819`; carimbos em `:911` | Aprovação de irmãos sob atomic externo `app/ponto/views.py:2631`, chamada em `:2633`. |
| `Ausencia` | Atualização em `app/ponto/services/ausencia.py:1267`; exclusões em `:1254`, `:1174` | Cancelamento/correção de férias: `app/ferias/services.py:352`, `:375`. |
| `FechamentoMensal` | Atualização em `app/ponto/services/fechamento.py:1640` | Atomic por competência em `:1635`; pode herdar atomic de justificativa/ausência. |
| `FechamentoMensal` | Atualização em `app/ponto/services/fechamento.py:1597` | Receiver síncrono de escala em atomic do emissor: `app/escala/signals.py:133`. |
| `FechamentoMensal` | Gravações em `app/ponto/services/fechamento.py:1128`, `:1144` | Atomic externo, por exemplo `app/ponto/portas/regen_exportada.py:169`, chamada em `:179`. |
| `DiaPago` | Exclusão/reposição em `app/ponto/services/dia_pago.py:357`, `:358` | Sob cálculo com atomic externo, por exemplo o da regeneração exportada acima. |
| `DiaPago` | `bulk_update`/`save` em `app/ponto/services/dia_pago.py:536`, `:540` | Atomic em `:532`. |
| `DiaPago` | Atualizações em `app/ponto/services/dia_pago.py:669`, `:671` | Atomic em `:667`. |
| `FolgaDia` | Exclusão/save em `app/escala/folgas.py:333`, `:342` | Atomics de alteração/lote em `:354`, `:382`. |
| `FolgaDia` | Atualização em `app/escala/folgas.py:493` | Reparentamento, atomic em `:490`. |
| `FolgaDia` | Exclusão/reposição em `app/escala/folgas.py:183`, `:184` | Geração de foto, atomic em `:162`. |
| `FolgaCalendario` | Saves em `app/escala/folgas.py:560`, `:564`, `:569` | Atomic em `:555`; primeiro save pode inserir, conforme `:556`. |
| `FolgaCalendarioTamanho` | Exclusão em `app/escala/folgas.py:565`; `update_or_create` em `:571` | Mesmo atomic em `:555`. |
| `EscalaColaborador` | Saves em `app/colaboradores/services/vinculo.py:274`, `:445`, `:799`; updates em `:615`, `:691`, `:753`, `:991` | Atomics em `:187`, `:443`, `:796`, `:614`, `:690`, `:752`, `:989`. |
| `AgendamentoFerias` | Saves em `app/ferias/services.py:362`, `:387`, `:406`; exclusão em `:483` | Atomics em `:352`, `:375`, `:399`, `:482`. |
| `PeriodoAquisitivo` | Saves em `app/ferias/services.py:302`, `:326`, `:355`, `:389`, `:409`, `:450`, `:475`; exclusão em `:484` | Atomics de férias indicados no catálogo seguinte; avaliação sob `app/ferias/views.py:717`. |
| `SolicitacaoFerias` | Save em `app/ferias/services.py:422` | View abrangente `app/ferias/views.py:717`. |
| `Colaborador` | Escritores em `app/colaboradores/services/colaborador.py:61`, `:69`, `:76`, `:88` | Chamadores atômicos de edição, desligamento, transferência e normalização descritos adiante. |
| `Empresa` | Update em `app/colaboradores/services/estrutura.py:27` | Command `app/colaboradores/management/commands/normalizar_identificadores.py:106`, chamada em `:112`. |
| `User` | Save de acesso em `app/core/credenciais.py:114`; save na edição em `app/colaboradores/views.py:1345` | Desligamento/reativação em `app/colaboradores/services/desligamento.py:103`, `:209`; edição em `app/colaboradores/views.py:1340`. |
| `AgendaDia` | Updates em `app/escala/services/agenda.py:33`, `:45` | Proposta em `app/colaboradores/views.py:2358`; resposta em `app/chamados/services/acoes_chamado.py:102`. |
| `SolicitacaoBeneficio` | Update em `app/colaboradores/services/beneficios.py:32` | View abrangente `app/colaboradores/views.py:2065`, chamada em `:2086`. |
| `LoteHolerite` | Saves em `app/holerite/management/commands/processar_lote_holerite.py:68`, `:146`; motor em `app/holerite/motor.py:409`, `:487` | **Três transações distintas**: command `:53`, motor `:385`, command `:123`. |
| `Holerite` | Save em `app/holerite/motor.py:461`; updates em `app/holerite/servicos.py:113`, `:121`, `:129` | Motor atomic `app/holerite/motor.py:385`; envio atomic `app/holerite/servicos.py:144`. |
| `PushSubscription` | Save em `app/ponto/webpush.py:66` | Pode ocorrer no envio de holerites, pelo fallback `app/holerite/servicos.py:42`, dentro do atomic `:144`. |
| `AplicacaoConvencao` | Updates em `app/core/services/aplicacao_convencao.py:132`, `:155` | Decoradores em `:100`, `:137`. |
| `Pauta` | Updates em `app/pautas/services.py:215`, `:247`, `:272`, `:294` | Decoradores em `:203`, `:232`, `:255`, `:280`. |
| `ContadorPauta` | Update em `app/pautas/services.py:354` | Chamado por esses quatro atos: `:218`, `:251`, `:276`, `:297`. |
| `EventoCelula` | Updates em `app/chamados/reconciliador.py:1374`, `:1380` | Atomic próprio em `:1371`, depois do processamento dos chamados. |
| `PlacarSituacional` | `update_or_create` e exclusão em `app/core/management/commands/lavrar_placar_situacional.py:105`, `:110` | Atomic em `:101`; Empresa é consultada em `:100`. |

Outras entidades **inseridas** nos caminhos examinados: `LogAuditoria` (`app/core/observ.py:134`), `FollowupChamado` (`app/chamados/services/nota.py:103`, `:107`), `HistoricoVinculo` (`app/colaboradores/services/historico_vinculo.py:25`), `OrdemSubstituicao` (`app/ponto/services/cobertura.py:100`, `:289`), `Beneficio` (`app/colaboradores/services/beneficios.py:11`), `LoteEspelho` (`app/relatorios/lote_espelho.py:103`), `RespostaAnterior` (`app/chamados/services/disputa_emissao.py:1403`). Não atribuí a essas inserções uma ordem de travas de FK: **[nao verificado]**.

**Signals síncronos que escrevem outra entidade**

Nas linhas com “cartório”, a expansão está detalhada no começo do item 3.

| Emissor/evento | Receiver arquivo:linha | Outra entidade escrita/travada |
|---|---|---|
| `Setor/post_save` | `app/colaboradores/signals.py:10` | `Group.save` em `:21` ou `get_or_create` em `:25`; permissões M2M via `:30` → `app/colaboradores/mixins.py:397`; atualização de Setor em `signals.py:32`. |
| `Setor/post_delete` | `app/colaboradores/signals.py:36` | Exclusão de `Group` em `:43`; cascatas internas **[nao verificado]**. |
| `Posto/post_save` | `app/colaboradores/signals.py:51` | `DisputaSupervisao.remover_motivo` em `:77`: exclui `PerguntaDisputa`, salva Disputa e pode fechá-la, em `app/chamados/models.py:1976`, `:1979`, `:1986`. |
| `Batida/post_save` | `app/colaboradores/signals.py:87` | Atualiza `Colaborador` por `:102` → `app/colaboradores/services/colaborador.py:61`, sob os guards de `signals.py:94`, `:97`, `:99`. |
| `DeviceColaborador/post_save` | `app/colaboradores/signals.py:106` | Atualiza `Colaborador` por `:119` → `app/colaboradores/services/colaborador.py:61`. |
| `Batida/post_save` | `app/ponto/signals.py:35` | `TurnoMaterializado` D/I, via `:26` → `app/ponto/nucleo.py:57`; `EventoCelula` I condicional via `nucleo.py:66`. |
| `Batida/post_delete` | `app/ponto/signals.py:49` | Mesmo recompute, chamado em `:50`. |
| `EscalaColaborador/post_save` | `app/ponto/signals.py:73` | Mesmo recompute, chamado em `:64`. |
| `Batida/post_save` | `app/ponto/signals.py:132` | Cartório, por `:135` → `:93`. |
| `Batida/post_delete` | `app/ponto/signals.py:139` | Cartório, por `:140`. |
| `Ausencia/post_save` | `app/ponto/signals.py:144` | Cartório, por `:147`. |
| `Ausencia/post_delete` | `app/ponto/signals.py:151` | Cartório, por `:152`. |
| `Justificativa/post_save` | `app/ponto/signals.py:162` | Cartório, por `:165`. |
| `Batida/post_save` | `app/ponto/signals.py:207` | Reconcilia chamados em `:215` e fecha adesão em `:234`; a unidade de retratação pode atualizar Justificativa em `app/chamados/reconciliador.py:535`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:35` | Regenera `CelulaDia` pela chamada `:71`; marca contexto de `ChamadoColaborador` órfão pela cadeia `app/ponto/portas/celula.py:283` → `app/ponto/management/commands/gerar_celulas.py:256`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:80` | Mesma regeneração, chamada em `:117`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:125` | Atualiza `FechamentoMensal`, por `:133` → `app/ponto/services/fechamento.py:1597`. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:143` | Remove motivo de Disputa por `:161`; escreve Pergunta/Disputa pelo método citado acima. |
| `EscalaColaborador/post_save` | `app/escala/signals.py:173` | Atualiza `FolgaDia`, por `:191` → `app/escala/folgas.py:493`; segue para relavra em `folgas.py:500`. |
| `DisputaSupervisao/post_save` | `app/chamados/signals.py:19` | Trava/atualiza Chamado-pai por `:42` → `app/chamados/models.py:674`, `:279`; insere nota em `signals.py:45`. |
| `DisputaSupervisao/post_save` | `app/chamados/signals.py:125` | Carimba Perguntas pela chamada `:140` → `app/chamados/services/materializacao.py:1346` → `app/chamados/models.py:2609`. |
| `ChamadoColaborador/post_save` | `app/chamados/signals.py:76` | Escreve Perguntas e pode fechar Disputas pela chamada `:92` → `app/chamados/services/disputa_emissao.py:1603`, `:1689`. |
| `ChamadoColaborador/post_save` | `app/chamados/signals.py:165` | Cartório pela chamada `:180`, salvo guards em `:169`, `:174`. |

Receivers que **agendam** o efeito por `on_commit`, portanto não justificam uma seta de trava na transação emissora:

| Emissor | Receiver | Agendamento |
|---|---|---|
| `DisputaSupervisao` | Retomar regularizações, `app/chamados/signals.py:98` | `app/chamados/signals.py:121` |
| `PerguntaDisputa` | Fechar beco, `app/chamados/signals.py:192` | `app/chamados/signals.py:202` |
| `Colaborador` | Silenciar isento, `app/chamados/signals.py:214` | `app/chamados/signals.py:224` |
| `ChamadoColaborador` | Silenciar isento, `app/chamados/signals.py:228` | `app/chamados/signals.py:251` |
| `Feriado` | Reavaliar dia, `app/chamados/signals.py:270` | `app/chamados/signals.py:280` |
| `ChamadoColaborador` | Acordar pai, `app/chamados/signals.py:295` | `app/chamados/signals.py:314` |
| `Ausencia/save/delete` | Consumidores, `app/ponto/signals.py:276`, `:283` | `app/ponto/signals.py:272` |
| `Posto` | Reavaliar geofence, `app/ponto/signals.py:292` | `app/ponto/signals.py:307` |

Dois `pre_save` examinados apenas leem/capturam estado: `app/escala/signals.py:19` e `app/chamados/signals.py:156`. O receiver de `TipoAusencia`, registrado pelos aliases de save/delete em `app/ponto/models.py:1233`, `:1234`, apenas invalida cache em `:1236`.

Os receivers de login chamam exclusão de `Session` ou alteram a sessão da request: `app/core/signals.py:14`, `:40`, `app/core/view

[truncado em 20000 de 45552 caracteres]

## 05/11 20:51 · sessao 35b27ecbba74 · - · - · origem=Ronald (tmux)

Complemento dos commands solicitados:

| Caminho | Atomic e escritas comprovadas |
|---|---|
| `marcar_abandono_provisorio.handle` | Atomic cobre **o comando inteiro** em `app/ponto/management/commands/marcar_abandono_provisorio.py:22`; User `get_or_create` na linha 30 → loop de colaboradores nas linhas 35/43 → loop de dias na linha 70 → `garantir_ausencia` na linha 76. Signals síncronos de Ausencia entram nessa transação: `app/ponto/signals.py:143`/147. Sem `--apply`, rollback explícito na linha 98; com `--apply`, não há esse rollback. Ordem dos colaboradores não traz `order_by` nesse queryset, linha 35. |
| `escalonar_chamados_supervisao`, N2 | Atomic `app/ponto/management/commands/escalonar_chamados_supervisao.py:131`: `escalar_urgencia` linha 132 → Chamado SAVE `app/chamados/models.py:521` → LogAuditoria por `models.py:524`/`app/core/observ.py:134` → `anotar_contexto` na linha 138 → Chamado SAVE `app/chamados/services/contexto.py:77`. N3 escreve contexto de Disputa na linha 159 e de Chamado na linha 163 **fora desse atomic**. |
| `triagem_batida.ramo_solicitar_anuencia` | Atomic `app/ponto/services/triagem_batida.py:368`: escala linha 369 → Chamado SAVE `app/chamados/models.py:521` → LogAuditoria via linha 524 → renova contexto na linha 371 → Chamado SAVE `app/chamados/services/contexto.py:102`. Followup é anexado na linha 351, antes desse bloco. |
| `curar_residuo_bug115._ambiguo` | Atomic `app/ponto/management/commands/curar_residuo_bug115.py:120`: renovar contexto linha 121 → Chamado SAVE `app/chamados/services/contexto.py:102` → LogAuditoria via comando linha 124 e `app/core/observ.py:134`. Outros ramos geram/julgam célula nas linhas 135/143, fora desse atomic. |
| `reconciliar_grade._relavrar_orfa` | Atomic `app/ponto/management/commands/reconciliar_grade.py:472`: renovar contexto linha 473 → Chamado SAVE `app/chamados/services/contexto.py:102`; retirar contexto linha 474 → Chamado SAVE `contexto.py:124`. Trilha `evento_kw` na linha 477 ocorre depois do bloco. |
| `lavrar_placar_situacional.handle` | Atomic `app/core/management/commands/lavrar_placar_situacional.py:101`: loop PlacarSituacional `update_or_create` linha 105 → DELETE de PlacarSituacional linha 110. Empresa apenas consultada na linha 100. Não aparece segunda entidade escrita nesse bloco. |

Dois comandos são barrados em produção pelo código:

- `zerar_dados_operacionais`: exige ensaio em `app/ponto/management/commands/zerar_dados_operacionais.py:27`; a guarda recusa se `BANCO_DE_ENSAIO` não estiver ativo **ou** o banco for `saas_hasner`, em `app/ponto/registro_batida.py:326`. No atomic da linha 80, sequência das chamadas explícitas: Followup DELETE linha 83 → Chamado DELETE linha 84 → AlertaGeofence DELETE linha 85 → AlertaTurno DELETE linha 87 → PushSubscription DELETE linha 88 → Ausencia DELETE linha 90 → FechamentoMensal DELETE linha 93 → Justificativa DELETE linha 95 → Batida DELETE linha 96 → ImportacaoAEJ DELETE linha 97 → PingGeo DELETE linha 98. Deleções reais nos helpers: `app/chamados/services/nota.py:141`, `app/chamados/models.py:539`, `app/ponto/services/alerta_turno.py:32`, `app/ponto/services/ausencia.py:1274`, `app/ponto/services/fechamento.py:1752`, `app/ponto/registro_batida.py:345`. Signals de Ausencia/Batida podem intercalar escritas nessas chamadas; ordem SQL interna das cascatas [não verificado].
- `gerar_batidas_smoke_motor`: guarda de ensaio em `app/ponto/management/commands/gerar_batidas_smoke_motor.py:45`, mais exigência de host `juliani_db_test` nas linhas 47/48. Atomic da linha 106 chama `semear` na linha 107 → Batida `bulk_create` em `app/ponto/registro_batida.py:335` → trilha na linha 336. Não é caminho executável no banco `saas_hasner` pela guarda citada.

A referência `app/chamados/models.py:849` afirma, em comentário, ausência de atomic envolvendo laços sobre colaboradores; o comando `marcar_abandono_provisorio` efetivamente tem esse formato nas linhas 22,35,43,76. Para a análise de ordem, usar o fluxo executável do comando.

## 05/11 20:50 · sessao e492c32c6d22 · - · - · origem=Ronald (tmux)

Censo ponto/escala, somente por leitura do código; `I` abaixo significa INSERT, `U` UPDATE/save de existente, `D` DELETE. Não executei banco, Django nem comandos da aplicação. Ordem SQL interna e frequência de execução em produção: [não verificado]. [nao verificado]

**`select_for_update` encontrado nestes domínios**

| Arquivo:linha | Função | Modelo | Argumentos | Avaliação |
|---|---|---|---|---|
| `app/ponto/portas/he.py:174` | `decidir_he` | DecisaoHE | `()` | `.first()`; atomic decorator `:137` |
| `app/ponto/portas/he.py:318` | `recusar_ponta_pequena` | DecisaoHE | `()` | `.first()`; atomic `:268` |
| `app/ponto/portas/he.py:445` | `revogar_recusa_do_sistema` | DecisaoHE | `()` | `.first()`; atomic `:392` |
| `app/ponto/portas/he.py:555` | `autorizar_em_lote` | DecisaoHE | `()` | `.first()` `:556`; atomic por item `:548` |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | Justificativa | `()` | `.get()`; atomic `:1793`; outro explorer cobre expansão |

`autorizar_em_lote` volta a chamar `decidir_he` depois da primeira trava (`app/ponto/portas/he.py:562`), portanto toma a mesma entidade novamente (`:174`), dentro do mesmo atomic do dia (`:548`).

**Advisory**

`app/ponto/services/vigia_de_hora.py:79`, `rodar`: chave `7180000 + int(empresa)` (`:97`), `SELECT pg_try_advisory_lock(%s)` (`:99`), retorna sem processar se ocupado (`:100`), julga células enquanto segura a trava (`:131`), libera em finally com `pg_advisory_unlock` (`:136`). Não há `atomic` abrangendo o corpo dessa função (`:98`, `:102`, `:134`); o advisory abrange várias transações das portas chamadas, não deve virar uma transação única.

**Escritas existentes sob atomic direto ou por porta chamada**

| Modelo | Escrita | Atomic/caminho |
|---|---|---|
| CelulaDia | `portas/celula.py:239`, `:270` U | `regenerar_celulas_vinculo`, atomic `:212` |
| CelulaDia | `portas/celula.py:578` U | `reverter_regeneracao_dia`, atomic `:569` |
| CelulaDia | `portas/celula.py:682` U | `salvar_e_emitir`, atomic `:681` |
| CelulaDia | `portas/celula.py:645`, `:655` U | `gerar_celulas_periodo`; comando `management/commands/gerar_celulas.py:310` atomic, chamada `:311` |
| CelulaDia | `portas/celula.py:885` bulk U | `carimbar_insumos_em_lote`; não possui atomic próprio (`:868`, `:885`); transação depende do chamador [não verificado neste censo] |
| FuroDiario | `portas/celula.py:906` update_or_create, `:914` D | `gravar_furos`, atomic `:904` |
| DecisaoHE | `portas/he.py:84` U; criação `:66` | `_gravar`, chamado por `decidir_he:181`/`recusar_ponta_pequena:341` sob decorators `:137`/`:268` |
| DecisaoHE | `portas/he.py:375` U | `_marcar_revogada`, chamada `:472` sob decorator `:392` |
| Justificativa | `services/regularizacao.py:106` U | `regularizar_turno_orfao`, atomic `:89` |
| Batida | `registro_batida.py:254` U | `anotar`, quando chamado pelo atomic externo: `veredito_celula.py:204`, flip `:295`; comando `flip_automatico.py:65`, chamada `:68`; outros callers ficam com raiz |
| FechamentoMensal | `services/fechamento.py:1640` U | `invalidar_fechamento`, atomic `:1635` |
| FechamentoMensal | `services/fechamento.py:1597` U | `invalidar_previsto`, quando receiver de EscalaColaborador é executado em atomic do emissor (`escala/signals.py:124`, `:133`) |
| FechamentoMensal | `services/fechamento.py:1128` U/save, `:1144` U | `recalcular_fechamento_mes` SEM atomic próprio; externos listados adiante |
| DiaPago | `services/dia_pago.py:536` bulk U, `:540` U, `:543` I | `retratar_exportada`, atomic `:532` |
| DiaPago | `services/dia_pago.py:669`, `:671` U | `apurar_furo_da_exportada`, atomic `:667` |
| DiaPago | `services/dia_pago.py:357` D, `:358` bulk I | `lavrar`, sem atomic próprio; via recalcular sob atomic externo |
| FolgaDia | `escala/folgas.py:342` U / `:333` D | `_foto_do_dia`; chamados sob atomic de `alterar_folga:354`/`:356`, `aplicar_folgas_em_lote:382`/`:389` |
| FolgaDia | `escala/folgas.py:493` U | `reparentar_folga`, atomic `:490` |
| FolgaDia | `escala/folgas.py:183` D, `:184` bulk I | `_gerar_foto_folgas_no_ato`, atomic em `gerar_foto_folgas:162`, chamada `:163` |
| FolgaCalendario | `escala/folgas.py:560`, `:564`, `:569` save | `salvar_calendario_de_folga`, atomic `:555`; primeiro save pode ser I conforme `cal is None:556` |
| FolgaCalendarioTamanho | `escala/folgas.py:565` D, `:571` update_or_create | mesmo atomic `:555` |
| EscalaColaborador | `colaboradores/services/vinculo.py:160` U | `trocar_calendario_de_folga`, chamado pela view atomic `escala/views.py:407`, chamada `:409` |
| EscalaColaborador | `colaboradores/services/vinculo.py:615` U, `:1010` I | `ajustar_vinculo_pelo_sistema:1008`, chamado em `escala_auto_executor.py:103` sob atomic `:101` |
| EscalaColaborador | `colaboradores/services/vinculo.py:1020` D, `:1021` U; ou `:1023` fechar_vigencia e `:1025` I | `desfazer_ajuste_do_sistema`, chamado em `escala_auto_executor.py:142` sob atomic `:141` |

Os paths nessa tabela levam prefixo `app/`.

**Decorador deslocado que invalida leitura ingênua do grep**

`app/escala/services/cadastro_tipo.py:41` `@transaction.atomic` decora **`_hpd_pares` em `:43`**, helper puro. `salvar_tipo` começa em `:140` e salva em `:306`; não está protegido por esse decorator. `_propagar_regeneracao_template` começa em `:327`; suas portas têm atomics internos, mas não há atomic abrangente de `salvar_tipo` estabelecido por `:41`.

**Signals síncronos e entidades escritas**

| Emissor | Receiver | Efeito síncrono |
|---|---|---|
| Batida post_save | `app/ponto/signals.py:35` `_nucleo_post_save_batida` | `_recompute_around:26` → TurnoMaterializado D/I em `app/ponto/nucleo.py:58`, `:59`, SQL DELETE `:80`; EventoCelula I condicional `:66`→`:93`→`portas/celula.py:673` |
| Batida post_delete | `app/ponto/signals.py:49` | mesmo recompute via `:50` |
| EscalaColaborador post_save | `app/ponto/signals.py:73` | TurnoMaterializado D/I, EventoCelula I via `_recompute_escala:64` |
| Batida post_save | `app/ponto/signals.py:132` | cartório via `:135`→`:93`; escreve CelulaDia/EventoCelula, e condicionalmente chamados/disputas/perguntas pelo fluxo abaixo |
| Batida post_delete | `app/ponto/signals.py:139` | cartório via `:140` |
| Ausencia post_save | `app/ponto/signals.py:144` | cartório via `:147` |
| Ausencia post_delete | `app/ponto/signals.py:151` | cartório via `:152` |
| Justificativa post_save | `app/ponto/signals.py:162` | cartório via `:165` |
| Batida post_save | `app/ponto/signals.py:207` | `reconciliar_chamados_colab:215`; depois `fechar_adesao_com_batida:234`, entidades de chamados; raiz expande |
| EscalaColaborador post_save | `app/escala/signals.py:35` `_ec_vigencia_regenera_celulas` | regenera CelulaDia via `:71`, e marca ChamadoColaborador órfão via `portas/celula.py:283` |
| EscalaColaborador post_save | `app/escala/signals.py:80` `_ec_ancora_regenera_celulas` | mesma porta via `:117` |
| EscalaColaborador post_save | `app/escala/signals.py:125` `_ec_invalida_previsto` | FechamentoMensal U via `:133`→`services/fechamento.py:1597` |
| EscalaColaborador post_save | `app/escala/signals.py:143` `_ec_revalidar_disputas_ancora` | DisputaSupervisao/PerguntaDisputa e chamadas subsequentes pela porta `d.remover_motivo:161`; raiz expande |
| EscalaColaborador post_save | `app/escala/signals.py:173` `_ec_folgas_seguem_a_pessoa` | FolgaDia U via `:191`→`folgas.py:493`, depois CelulaDia/Chamado pela re-lavra `:500` |

`EscalaColaborador pre_save` `app/escala/signals.py:19` captura estado anterior; não escreve outra entidade (`:20` em diante).

**Fora da transação emissora por `on_commit`**

- Ausencia post_save/post_delete consumidor `app/ponto/signals.py:276`, `:283`: `_consumidor_ausencia` agenda callback em `:272`; dentro dele devolve perguntas (`:262`), reconcilia (`:269`) e fecha chamado (`:271`).
- Posto post_save geofence `app/ponto/signals.py:292`: reavaliar chamados `:306` é callback agendado em `:307`.
- Dinheiro de batida/HE/regeneração: `app/ponto/services/fechamento.py:1808` agenda recalcular; a chamada `:1794` ocorre no callback. Não juntar FechamentoMensal/DiaPago ao atomic original por essas chamadas.
- Regeneração agenda rejunção/falta espúria no callback `app/ponto/portas/celula.py:473`, funções em `:469`, `:472`.

**Catálogo de sequências para consolidação**

1. **PE01 — `salvar_e_emitir`**: atomic `app/ponto/portas/celula.py:681`; **CelulaDia U `:682` → EventoCelula I `:673`**, chamada `:684`. Segunda entidade é INSERT, não segunda trava comprovada de linha existente.

2. **PE02 — `recompute_turnos`**: atomic `app/ponto/nucleo.py:57`; **TurnoMaterializado D SQL `:80` → TurnoMaterializado I `:59` → EventoCelula I `portas/celula.py:673`**, quando `fechados` (`nucleo.py:65`), chamada `:66` e seleção ordenada por data `:92`. Não escreve CelulaDia; ela é somente consultada em `:92`.

3. **PE03 — `regenerar_celulas_vinculo`**: atomic `app/ponto/portas/celula.py:212`; **CelulaDia U `:239`/`:270` (ou I `:253`) → ChamadoColaborador U** via `_marcar_cobrancas_orfas:283` → `app/ponto/management/commands/gerar_celulas.py:256` `renovar_contexto`. Esse é caminho concreto **CelulaDia antes de Chamado**. Ordem das células é calendário crescente (`:214`, `:215`, `:275`); queryset de chamados órfãos não declara `.order_by` (`gerar_celulas.py:244`).

4. **PE04 — `alterar_folga`**: atomic `app/escala/folgas.py:354`; **FolgaDia U `:342`/D `:333` → PE03 CelulaDia → Chamado** pela chamada `:357`→`:272`. Pode realizar cartório depois, ainda no atomic externo.

5. **PE05 — `aplicar_folgas_em_lote`**: atomic `app/escala/folgas.py:382`; **FolgaDia U/D/I**, ordem da lista fornecida (`:384`, escrita `:389`) → **PE03 CelulaDia → Chamado** (`:395`→`:272`).

6. **PE06 — `gerar_foto_folgas`**: atomic `app/escala/folgas.py:162`; **FolgaDia D `:183` → FolgaDia I `:184` → PE03 CelulaDia → Chamado**, chamada re-lavra `:203`→`:272`.

7. **PE07 — `lancar_folga`**: atomic `app/escala/folgas.py:309`; **FolgaDia get_or_create `:312` → PE03 CelulaDia → Chamado** via `:314`. Se FolgaDia já existe, `get_or_create` não é save existente aqui; não marcar como trava U comprovada.

8. **PE08 — `reparentar_folga`**: atomic `app/escala/folgas.py:490`; **FolgaDia U `:493` → LogAuditoria I por `evento:494` → PE03 CelulaDia → Chamado** pela re-lavra `:500`. As folgas externas percorrem `.order_by('data')` `:515`.

9. **PE09 — calendário editor**: atomic `app/escala/folgas.py:555`; **FolgaCalendario save `:560`/`:564`/`:569` → FolgaCalendarioTamanho D `:565` ou update_or_create `:571`**. A ordem dos tamanhos é do dict recebido (`:570`), dependente do input.

10. **PE10 — view `aplicar_folga_calendario`**: atomic `app/escala/views.py:407`; **EscalaColaborador U** via `:409`→`colaboradores/services/vinculo.py:160`, **signals síncronos acima** → **PE06 FolgaDia/CelulaDia/Chamado** via `views.py:411`, `:412`, nova re-lavra `:416`. Ordem entre signal em `ponto.signals` e em `escala.signals` exige configuração de carregamento; não fixei cross-module.

11. **PE11 — `_escrever_folga` em `declarar_dia`**: atomic `app/ponto/services/declarar_dia.py:188`; **PE07 FolgaDia/CelulaDia/Chamado** via `:189` → **PerguntaDisputa fechamento admin** em `services/veredito_celula.py:177` via `declarar_dia.py:204` → **Chamado reconciliar** `veredito_celula.py:183` → RespostaChamado I via nota `:185`.

12. **PE12 — `veredito_celula`, ramo trabalhou**: atomic `app/ponto/services/veredito_celula.py:204`; **Batida U** via `flip_tipo:295`→`services/flip_batida.py:30`→`registro_batida.py:254`, ou Batida I via `veredito_celula.py:301`→`registro_batida.py:157` → **signals Batida**, PE02 e cartório → **TurnoMaterializado D/I** recompute explícito `veredito_celula.py:308` → **PerguntaDisputa → Chamado**, fechar fio `:325`→`:177`/`:183` → **CelulaDia → EventoCelula**, lavra `:330`→`portas/celula.py:817`→`:682`/`:673` → auditoria `veredito_celula.py:334`. Ramos e número de flips/batidas dependem do dado (`:290`, `:291`, `:299`).

13. **PE13 — `veredito_celula`, faltou/justificado**: mesmo atomic `:204`; **Ausencia I** via `:315` → signal cartório Ausencia `signals.py:144` → depois fechar fio **PerguntaDisputa → Chamado** `veredito_celula.py:325` → **CelulaDia/EventoCelula** `:330`. Não chamar Ausencia INSERT de trava de uma linha existente.

14. **PE14 — `regularizar_turno_orfao`**: atomic `app/ponto/services/regularizacao.py:89`; **Justificativa I `:91` ou U `:106`** → signal cartório Justificativa `signals.py:162` → **Ausencia get_or_create** via `regularizacao.py:118` (I quando criada, signals) → **ChamadoColaborador U analisar `:137`** → RespostaChamado I nota `:139` → auditoria `:145` em diante. Não é uma simples sequência Justificativa→Ausencia→Chamado porque os signals se intercalam em cada save.

15. **PE15 — `regenerar_em_exportada`**: atomic `app/ponto/portas/regen_exportada.py:169`; **PE03 CelulaDia → Chamado** via `:174` → cartório pós-regeneração, ainda sob atomic externo (`portas/celula.py:402`) → **FechamentoMensal U** `services/fechamento.py:1128`/`:1144`, chamado `regen_exportada.py:179` → **DiaPago D/I** `services/dia_pago.py:357`/`:358` via `fechamento.py:1185` e `:1261` → **DiaPago U** via `regen_exportada.py:188`→`dia_pago.py:536`/`:540` → LogAuditoria I `regen_exportada.py:199`.

16. **PE16 — `aplicar_no_vinculo` executor escala**: atomic `app/escala/services/escala_auto_executor.py:101`; **TipoEscala I `cadastro_tipo.py:416`**, AgendaBloco I `:424` por derivação `executor.py:102`→`:84`; **EscalaColaborador U** fechar vigência `colaboradores/services/vinculo.py:615`, chamada `:1008` → **EscalaColaborador I `:1010`**, via executor `:103`, com signals → **PE03 CelulaDia → Chamado** executor `:104` → HistoricoVinculo I `:107`.

17. **PE17 — `desfazer` executor**: atomic `app/escala/services/escala_auto_executor.py:141`; **EscalaColaborador D/U** `colaboradores/services/vinculo.py:1020`/`:1021` ou U/I `:1023`/`:1025`, chamada executor `:142` → **PE03 CelulaDia → Chamado** chamada `:143` → HistoricoVinculo I `:145`.

18. **PE18 — HE individual**: **DecisaoHE SELECT FOR UPDATE `portas/he.py:174` → U `:84` ou I `:66` → LogAuditoria I por `:94`**. FechamentoMensal/DiaPago estão no callback de `on_commit` (`:202`→`fechamento.py:1808`), não nesta transação.

19. **PE19 — ato HE período**: atomic `services/autorizacao_he_periodo.py:321` (prévia rollback `:323`) ou `:343` (confirmar); **DecisaoHE** via `_ato:277`→`portas/he.py:555`→`:562`→PE18; depois **Pauta I** somente exportada com autorizados (`autorizacao_he_periodo.py:299`, chamada `:300`, escritor `:244`), e sua expansão deve vir do censo pautas da raiz. O antes/depois `_medir` `:271`, `:280` não é o recalcular fechamento gravado.

20. **PE20 — designar cobertura**: atomic `app/ponto/services/cobertura.py:99`; **OrdemSubstituicao I `:100` → ChamadoColaborador reconciliar `:122` → RespostaChamado I `:132`**. `declarar_cobertura_passada` tem atomic `:283`, **OrdemSubstituicao I `:289` → Pauta existente marcar feita `:305` → Chamado reconciliar `:317` → RespostaChamado I `:321`**, ou sem substituto Pauta I `:333`.

**Cartório: subgrafo fundamental para os signals**

`app/ponto/services/cartorio.py:948` chama corpo. Nele, **Chamado/Pergunta antes de CelulaDia** pode ocorrer por `reconciliar:1488`, `:1501`, `:1522`, `:1600`; `ressuscitar_perguntas:1622`; emissor `:1740`/`:1778`; espelhar marcos `:1789`/`:1798`; só então **CelulaDia U** `carimbar:1822`→`portas/celula.py:849`→`:682`, **EventoCelula I** `:673`.

Depois da primeira lavra, pode ocorrer **CelulaDia antes de Pergunta/Chamado/Colaborador**: cartório `_e2(c):959`, reconciliar `:963`, cluster `:994`, fábrica `perguntar_pela_celula:997`; depois nova lavra pela releitura `:1038`. O ramo `nunca_bateu` grava célula `:1264` e retorna `:1268`; portanto não permite assumir que toda execução passa no mesmo conjunto de passos.

Esse subgrafo só pertence à transação emissora quando ela existe: callbacks `on_commit` já foram separados acima. Há ordens de **operações** nos dois sentidos, mas contagem de **primeira aquisição** de locks requer considerar travas já tomadas antes no mesmo caminho; não equiparar “save depois” a “primeira trava depois”.

**Commands com atomic externo chamando cálculo gravado**

Todos permitem **FechamentoMensal U `app/ponto/services/fechamento.py:1128`/`:1144` → DiaPago D/I `app/ponto/services/dia_pago.py:357`/`:358`** dentro da transação externa, ao tomar o ramo de escrita:

- `app/ponto/management/commands/desvio_o68b.py:93`, chamada cálculo `:94`.
- `app/ponto/management/commands/diff_reclassificar_partido.py:160`/`:198`, chamadas `:161`/`:206`.
- `app/ponto/management/commands/celula_veredito_velho.py:133`, cálculo `:149`.
- `app/ponto/management/commands/folga_que_sumiu.py:69`, cálculo `:70`.
- `app/ponto/management/commands/plano_b_no_dinheiro.py:109`, cálculo `:110`.
- `app/ponto/management/commands/aplicar_09_corte_b.py:197`, cálculo `:212`.
- `app/ponto/portas/regen_exportada.py:169`, cálculo `:179`, já PE15.

O rollback deliberado de sonda não apaga a aquisição durante a execução (`autorizacao_he_periodo.py:321`/`:323`; commands com respectivos blocos citados). Não executei essas sondas. [nao verificado]

**Limitações para raiz registrar**

- Os dados fornecidos não escolhem os ramos `if` dos caminhos; execução concreta, linhas realmente existentes e contagens de produção: [não verificado].
- Querysets múltiplos sem `order_by` não têm ordem de linhas estabelecida no código: órfãos `gerar_celulas.py:244`, perguntas fechamento admin `veredito_celula.py:174`, chamados `:148`/`:155`.
- Decisões HE em lote seguem lista recebida, `portas/he.py:546`, sem ordenação global.
- TipoEscala salvar não ganha atomic pelo decorator `cadastro_tipo.py:41`; esse pertence ao helper `:43`.
- Atomics internos da regeneração terminam em `portas/celula.py:348`; cartório `:402` ocorre depois. Ele compartilha o atomic apenas se existir wrapper externo, como PE04–PE08, PE10, PE15–PE17.
- `PeriodoFechado` `services/fechamento.py:1403`/`:1425` e FechamentoMensal `:1436` não têm transação própria nesse caminho; `reabrir_periodo` declara isso em `:1419`.
- `update_or_create` está presente em `portas/celula.py:906` e `escala/folgas.py:571`; a implementação Django/SQL interno não foi executada ou inspecionada aqui: [não verificado].
- INSERT de filho não comprova, por si só, trava de linha pai no instante previsto; efeito FK/constraints, triggers e plano PostgreSQL: [não verificado].

## 05/11 20:50 · sessao 18cfca111f7f · - · - · origem=Ronald (tmux)

Censo do recorte atribuído, somente por leitura do código. Os IDs abaixo identificam unidades de transação; INSERT está separado de escrita de linha existente. Receivers de ponto/chamados precisam ser expandidos pelo explorador responsável antes de transformar estas sequências em contagens globais. Os pontos de disparo estão em `app/ponto/services/justificativa.py:193`, `app/ferias/models.py:436`, `app/colaboradores/services/colaborador.py:61`.

**Correção:** `ChamadoColaborador.criar()` não implica a trava explícita de Colaborador de `abrir()`; não atribuir essa trava a C11. A trava explícita pertence a `app/chamados/models.py:866`.

### Selects explícitos do recorte

Todos os argumentos de `select_for_update()` abaixo são vazios:

| Local | Função | Modelo | Avaliação |
|---|---|---|---|
| `app/colaboradores/models.py:431` | `Colaborador.save` | Colaborador | `values_list(...).first()` em `:435-436`; atomic `:429`; **super.save em `:439` fica depois desse atomic** |
| `app/core/services/aplicacao_convencao.py:115` | `cadastrar` | AplicacaoConvencao | `.first()` `:116`; decorator atomic `:100` |
| `app/ferias/views.py:718` | `avaliar_solicitacao` | SolicitacaoFerias | `.first()` `:720`; atomic `:717` |
| `app/holerite/management/commands/processar_lote_holerite.py:54` | `Command._rodar` | LoteHolerite, alias L `:51` | `.get()`; atomic `:53` |
| mesmo arquivo `:124` | `Command._rodar` | LoteHolerite | `.get()`; outro atomic `:123` |
| `app/ponto/services/justificativa.py:38` | `encerrar_chamado_de_origem` | ChamadoColaborador | `.first()` |
| mesmo arquivo `:70` | `_encerrar_bloqueio_de_entrada` | ChamadoColaborador | `.order_by('-criado_em').first()` `:73` |
| mesmo arquivo `:114` | `travar_origem_antes` | ChamadoColaborador | `list(...order_by('pk').values_list(...))` |
| mesmo arquivo `:127` | `aprovar_justificativa_dp` | Justificativa | `.first()` `:129`; atomic `:125` |
| mesmo arquivo `:262` | `rejeitar_justificativa_dp` | Justificativa | `.first()` `:264`; atomic `:258` |
| mesmo arquivo `:329` | `materializar_saida_dp` | Justificativa | `.get()`; atomic `:327` |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | Justificativa | `.get()`; atomic `:1793` |

### Signals do recorte

Registro de colaboradores: `app/colaboradores/apps.py:8`. Registro de core: `app/core/apps.py:9`.

| Emissor | Receiver | Outra entidade escrita |
|---|---|---|
| Setor / post_save | `app/colaboradores/signals.py:10` | Group.save `:21`, ou Group.get_or_create `:25`; Group–Permission M2M pelo helper `:30` → `app/colaboradores/mixins.py:397`; volta a atualizar Setor `signals.py:32` |
| Setor / post_delete | `app/colaboradores/signals.py:36` | Group.delete `:43` |
| Posto / post_save | `app/colaboradores/signals.py:51` | DisputaSupervisao.remover_motivo `:77`, quando Posto existente está ativo e disputa casa com filtro `:71`; expansão do método pertence ao domínio chamados |
| Batida / post_save | `app/colaboradores/signals.py:87` | Colaborador via `_gravar_colab` `:102` → `app/colaboradores/services/colaborador.py:61`; somente criada, onboarding incompleto e usuário já logou, guards `signals.py:94`, `:97`, `:99` |
| DeviceColaborador / post_save | `app/colaboradores/signals.py:106` | Colaborador via `:119` → `services/colaborador.py:61`; guards criada/incompleto `signals.py:113`, `:116` |
| user_logged_in | `app/core/signals.py:14` | chama invalidar_sessoes_anteriores `:20`; Session em `app/core/views.py:30`; ocorrência dentro de atomic não demonstrada neste recorte [não verificado] |
| user_logged_in | `app/core/signals.py:40` | atribui sessão de request `:48`; persistência dentro da mesma transação [não verificado] |

`AgendamentoFerias.save()` **é override, não receiver**: primeiro `super().save()` em `app/ferias/models.py:436`, depois `_sync_ausencia()` `:437`; helper `:384-385` delega a `espelhar_ferias`. Este insere Ausencia por `app/ponto/services/ausencia.py:1186` → `:418`; apaga dias removidos em `:1197`; agendamento cancelado apaga existentes em `:1174`.

### Unidades de férias

O override acima introduz signals de Ausencia no meio dos passos seguintes. Essa expansão é necessária para a ordem completa. `app/ferias/models.py:437`, `app/ponto/services/ausencia.py:418`, `:1174`.

| ID / entrada | Atomic | Sequência textual comprovada |
|---|---|---|
| F1 `executar_cancelamento_admin`; views `app/ferias/views.py:365`, `:407`, `:559` | `app/ferias/services.py:352` | PeriodoAquisitivo **UPDATE** `:355` → Ausencia **UPDATE** chamada `:356`, escritor `app/ponto/services/ausencia.py:1267` → AgendamentoFerias **UPDATE** `services.py:362` → Ausencia **DELETE** pelo override/sync `models.py:437`, `ausencia.py:1174` |
| F2 `remarcar_agendamento`; view `app/ferias/views.py:668` | `app/ferias/services.py:399` | AgendamentoFerias **UPDATE** `:406` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:409` |
| F3 `corrigir_datas_agendamento`; view `app/ferias/views.py:490` | `app/ferias/services.py:375` | Ausencia **DELETE** chamada `:385`, escritor `ausencia.py:1254` → AgendamentoFerias **UPDATE** `services.py:387` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:389` |
| F4 `registrar_gozo_historico`; view `app/ferias/views.py:210` | `app/ferias/services.py:277` | AgendamentoFerias **INSERT** `:298` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:302` |
| F5 `executar_agendamento`; views `app/ferias/views.py:298`, `:617`, wrapper `:263` | `app/ferias/services.py:315` | AgendamentoFerias **INSERT** `:321` → Ausencia **INSERT/DELETE condicional** pelo sync → PeriodoAquisitivo **UPDATE** `:326` |
| F6 `agendar_ferias_porta` | `app/ferias/services.py:156` | chamada F5 `:157` → PeriodoAquisitivo **UPDATE adicional** `:165`, se dias vendidos |
| F7 view `avaliar_solicitacao` | `app/ferias/views.py:717` | SolicitacaoFerias **S4U** `:718` → SolicitacaoFerias **UPDATE** `app/ferias/services.py:422` → AgendamentoFerias **INSERT** `:447` → sync de Ausencia → PeriodoAquisitivo **UPDATE** `:450`, somente ramo com materialização |
| F8 `excluir_periodo_aquisitivo` | `app/ferias/services.py:482` | AgendamentoFerias **DELETE queryset** `:483` → PeriodoAquisitivo **DELETE** `:484` |

Não atribuir `AgendamentoFerias.delete()` `app/ferias/models.py:439` ao `QuerySet.delete()` de F8: o chamador efetivo é `periodo.agendamentos.all().delete()` `app/ferias/services.py:483`; execução do override por esse queryset [não verificado].

Escritas atômicas de uma entidade existente: `editar_periodo_aquisitivo`, atomic `app/ferias/services.py:470`, PeriodoAquisitivo.save `:475`; sincronização de ciclos sob atomic `app/ferias/views.py:164`, chamada `:180` → PeriodoAquisitivo.update `app/ferias/services.py:266`.

**Não unir cancelar_agendamento_dp numa transação:** Periodo.save `app/ferias/services.py:335`, Agendamento.save `:337`; função `:330` e view `app/ferias/views.py:321` não delimitam atomic; transação envolvente [não verificado].

### Unidades de justificativa e regularização

| ID / entrada | Atomic | Sequência textual comprovada |
|---|---|---|
| J1 view `app/ponto/views.py:1917` → `aprovar_justificativa_dp` | `app/ponto/services/justificativa.py:125` | Chamado **S4U por pk** chamada `:126` → escritor `:114`; Justificativa **S4U** `:127`; Batida **INSERT condicional** chamada `:173`; Justificativa **UPDATE** `:193`; FechamentoMensal **UPDATE** chamada `:200` → `app/ponto/services/fechamento.py:1640`; Chamado origem **S4U** chamada `justificativa.py:202` → `:38`; Chamado bloqueio **S4U** chamada `:203` → `:70` |
| J2 view `app/ponto/views.py:1929` → `rejeitar_justificativa_dp` | `app/ponto/services/justificativa.py:258` | Chamado **S4U por pk** chamada `:261` → `:114`; Justificativa **S4U** `:262`; Justificativa **UPDATE** `:272`; efeitos B5 ficam após saída do atomic, início `:274` |
| J3 view `app/ponto/views.py:3136` → `materializar_saida_dp` | `app/ponto/services/justificativa.py:327` | Chamado **S4U por pk** chamada `:328` → `:114`; Justificativa **S4U** `:329`; Batida **INSERT** chamada `:346`; Justificativa **UPDATE** `:371`; FechamentoMensal **UPDATE** chamada `:375` → fechamento `:1640`; Chamados fechamento `justificativa.py:385`, origem `:393` |
| J4 view `aprovar_lote_justificativas` | `app/ponto/views.py:1793`, por item | Chamado **S4U por pk** chamada `:1794` → justificativa `:114`; Justificativa **S4U** `views.py:1795`; Justificativa **UPDATE** `:1800`; FechamentoMensal **UPDATE** chamada `:1806` → fechamento `:1640`; Batida **INSERT condicional** chamada `views.py:1816`; Justificativa **UPDATE** `:1833`; Chamado origem **S4U** chamada `:1838` → justificativa `:38` |
| J5 `regularizar_turno_orfao`; web `app/ponto/views.py:3205`, API `app/api/views.py:2877` | `app/ponto/services/regularizacao.py:89` | Justificativa **INSERT** `:91` ou, quando existente sem saída, **UPDATE** `:106`; Ausencia **INSERT** chamada `:118` → `app/ponto/services/ausencia.py:461` → `:418`; Chamado.analisar `regularizacao.py:137`; nota `:139`; LogAuditoria `:150` |

Os saves/INSERTs de Justificativa e Batida disparam signals antes do passo textual seguinte; não chamar J1/J4 de ordem final de Fechamento vs Batida sem expandi-los. Os locais efetivos são `app/ponto/services/justificativa.py:173`, `:193`, `app/ponto/views.py:1800`, `:1816`.

`invalidar_fechamento` faz atomic de leitura `app/ponto/services/fechamento.py:1632` e atomic por competência `:1635`; escrita efetiva `:1640`. Sob J1/J3/J4 esses atomics são aninhados aos atomics dos chamadores citados.

### Colaboradores / Empresa

Escritores: Colaborador.save por campos `app/colaboradores/services/colaborador.py:61`, `:69`; save inteiro `:76`; update queryset `:88`. Empresa.update `app/colaboradores/services/estrutura.py:27`. HistoricoVinculo INSERT `app/colaboradores/services/historico_vinculo.py:25`. User.save de acesso `app/core/credenciais.py:114`.

| ID / entrada | Atomic | Sequência textual comprovada |
|---|---|---|
| C1 command `normalizar_identificadores` | `app/colaboradores/management/commands/normalizar_identificadores.py:106` | Colaborador **UPDATE** chamada `:109` → escritor `services/colaborador.py:88`; depois Empresa **UPDATE** chamada `:112` → `services/estrutura.py:27` |
| C2 view `editar_colaborador` | `app/colaboradores/views.py:1340` | Colaborador **UPDATE** chamada `:1342` → escritor `services/colaborador.py:76`; User **UPDATE** `views.py:1345`, se CPF mudou e há usuário |
| C3 service `executar_reativacao`; view `app/colaboradores/views.py:1417` | `app/colaboradores/services/desligamento.py:209` | Colaborador **UPDATE** chamada `:214` → escritor `services/colaborador.py:76`; User **UPDATE condicional** chamada `desligamento.py:216` → `core/credenciais.py:114`; HistoricoVinculo **INSERT** chamada `desligamento.py:219` → escritor `services/historico_vinculo.py:25` |
| C4 service `executar_desligamento` imediato | `app/colaboradores/services/desligamento.py:103` | Colaborador **UPDATE** chamada `:108` → escritor `colaborador.py:76`; User **UPDATE condicional** chamada `desligamento.py:111` → credenciais `:114`; HistoricoVinculo **INSERT** chamada `desligamento.py:114`; EscalaColaborador **UPDATE** chamada `:123` → `services/vinculo.py:816` → `:615`; DisputaSupervisao.fechar `desligamento.py:142`, por pk `:139`; Chamado.encerrar `:155`; Chamado **INSERT** chamada `.criar` `:163` |
| C5 desligamento agendado | `app/colaboradores/services/desligamento.py:83` | Colaborador **UPDATE** chamada `:85` → `colaborador.py:61`; HistoricoVinculo **INSERT** chamada `desligamento.py:88` |
| C6 view `posto_add_colab` | `app/colaboradores/views.py:643` | Colaborador **UPDATE** chamada `:645` → `colaborador.py:61`; HistoricoVinculo **INSERT** chamada `views.py:647` |
| C7 view `posto_remove_colab` | `app/colaboradores/views.py:668` | Colaborador **UPDATE** chamada `:670` → `colaborador.py:61`; HistoricoVinculo **INSERT** chamada `views.py:672` |
| C8 view `transferir_posto` | `app/colaboradores/views.py:1384` | Colaborador **UPDATE** chamada `:1388` → `colaborador.py:76`; HistoricoVinculo **INSERT** chamada `views.py:1393` |
| C9 view admitir | `app/colaboradores/views.py:1175` | User **INSERT** `:1176`; Colaborador **S4U condicional** `app/colaboradores/models.py:431`; Colaborador **INSERT** chamada `views.py:1182` → `services/colaborador.py:93` → `models.py:439` |
| C10 view `avaliar_solicitacao_beneficio` | `app/colaboradores/views.py:2065` | Chamado.transicionar `:2068`; nota/FollowUp `:2071`; Beneficio **INSERT condicional** chamada `:2075` → `services/beneficios.py:11`; SolicitacaoBeneficio **UPDATE** chamada `views.py:2086` → `services/beneficios.py:32` |
| C11 view `agenda_enviar_aceite` | `app/colaboradores/views.py:2358` | Chamado **INSERT** chamada `:2363`; AgendaDia **UPDATE** chamada propor_agenda `:2378`; escritor no domínio escala |
| C12 `bootstrap_tenant` | `app/tenants/bootstrap.py:25` | Empresa **INSERT** chamada `:27`; Setor **INSERT** chamada `:33`, com receiver Group/M2M/Setor descrito acima; repete por competência `:32`; User **INSERT** `:34` |

Em C9, **o atomic termina antes de `app/colaboradores/views.py:1209`**. O save adicional `:1215` e Histórico `:1218` não estão nesse bloco.

Escritas atômicas adicionais de EscalaColaborador:

- `executar_vinculo`, atomic `app/colaboradores/services/vinculo.py:187`: fecha vigências em `:205`, `:232` → UPDATE `:615`; absorve posteriores `:244` → UPDATE `:753`; EscalaColaborador existente save `:274` ou INSERT `:278`; copia folga `:275`/`:289` → save `:160`. Expansão de signals do save compete ao domínio escala.
- `trocar_calendario_de_folga`, atomic `:443`, save `:445`, LogAuditoria `:448`.
- `fechar_vigencia`, atomic `:614`, update `:615`, LogAuditoria `:621`.
- `reabrir_vigencia_impossivel`, atomic `:690`, update `:691`, LogAuditoria `:697`, HistoricoVinculo INSERT chamada `:701` → `historico_vinculo.py:13`.
- `absorver_vigencias_posteriores`, atomic `:752`, update `:753`, LogAuditoria `:763`, HistoricoVinculo INSERT chamada `:771` → `historico_vinculo.py:13`.
- `encerrar_vinculo`, atomic `:796`, save `:799`, LogAuditoria `:802`.
- `gravar_regua`, atomic `:989`, update `:991`, LogAuditoria `:998`.

Outras escritas sob atomic no recorte:

- Command regerar_senhas_iniciais, atomic `app/colaboradores/management/commands/regerar_senhas_iniciais.py:64`, chama resetar_para_cpf `:70`; escritor User.save em `app/core/credenciais.py:55` ou `:71`, conforme função.
- Command marcar_feriado_do_posto, atomic `app/colaboradores/management/commands/marcar_feriado_do_posto.py:129`, Posto.save chamada `:131` → `services/estrutura.py:82`; receiver Posto pode escrever Disputa/Chamado. O loop de Chamado começa em **outro atomic por chamado**, command `:139`, reconciliar `:140`.
- Command lavrar_placar_situacional, atomic `app/core/management/commands/lavrar_placar_situacional.py:101`, PlacarSituacional.update_or_create `:105`, delete de outros `:110`.

### Holerite / AplicacaoConvencao

| ID | Atomic | Sequência |
|---|---|---|
| H1 publicação `motor.processar` | `app/holerite/motor.py:385` | LoteHolerite **UPDATE** `:409` se lote recebido, ou **INSERT** `:400`; Holerite **UPDATE condicional** `:461` para anexos; Holerite **INSERT** via arquivo.save `:483`; LoteHolerite **UPDATE** `:487` |
| H2 `validar_e_enviar`; view `app/holerite/views.py:618` | decorator `app/holerite/servicos.py:144` | `enviar` chamada `:155`, laço por holerite `:100`: push `:109`; pode Colaborador **UPDATE** por token inválido e PushSubscription **UPDATE** por endpoint morto; depois Holerite **UPDATE** `:113`, `:121` ou `:129`; LogAuditoria **INSERT** `:134` |
| H3 command backfill_empresa_folha | `app/holerite/management/commands/backfill_empresa_folha.py:55` | Holerite **UPDATE** `:56` |
| A1 AplicacaoConvencao.cadastrar | decorator `app/core/services/aplicacao_convencao.py:100` | AplicacaoConvencao **S4U** `:115`; UPDATE `:132` ou INSERT `:120`; LogAuditoria INSERT chamada `_trilha` `:133` → evento `:94` → `app/core/observ.py:134` |
| A2 AplicacaoConvencao.desativar | decorator `app/core/services/aplicacao_convencao.py:137` | AplicacaoConvencao **UPDATE** `:155`; LogAuditoria INSERT chamada `:156` → evento `:94` → observ `:134` |

Em H1, o command faz **três transações separadas**: command `app/holerite/management/commands/processar_lote_holerite.py:53` (S4U `:54`, save `:68`); publicação chamada `:89`, atomic no motor `:385`; finalização command atomic `:123` (S4U `:124`, save `:146`). Não unir a primeira e terceira com a publicação.

Expansão de H2:

1. FCM `_push` `app/holerite/servicos.py:38` → `app/chamados/fcm_utils.py:106` → erro token inválido chama limpeza `:93`.
2. `_limpar_token` chama `app/chamados/fcm_utils.py:41` → `app/colaboradores/fcm.py:61` → Colaborador.update `app/colaboradores/services/colaborador.py:88`.
3. Fallback web `_push` `app/holerite/servicos.py:42` → PushSubscription.save `app/ponto/webpush.py:66` quando erro contém 410/404 `:59`.
4. Holerite.update em `app/holerite/servicos.py:113`, `:121`, `:129`.

O laço de H2 permite `Holerite da pessoa anterior → Colaborador/PushSubscription da pessoa seguinte`, pois o laço está em `app/holerite/servicos.py:100`, push `:109`, update `:113`. Quais ramos ocorreram não está no dado fornecido [não verificado].

### Ausencia / Fechamento e limites úteis para consolidação

- `criar_ausencia` constrói objeto novo em `app/ponto/services/ausencia.py:385` e faz **INSERT** dentro do atomic `:417`, save `:418`. Não classificar esse local como UPDATE de Ausencia existente.
- `garantir_ausencia` delega à criação com retorno de existente em `app/ponto/services/ausencia.py:461`; não demonstrou UPDATE do existente.
- Writes de Ausencia existente em `app/ponto/services/ausencia.py:508`, `:536`, `:819`, `:843`, `:911`, `:1114`, `:1124`, `:1132`, `:1280`, `:1292` não possuem atomic próprio no método; só entram numa transação compartilhada quando há chamador externo demonstrado.
- Ausencia.decidir invalida Fechamento em `app/ponto/services/ausencia.py:930`; escritor Fechamento.update em `app/ponto/services/fechamento.py:1640`. Sem atomic externo que cubra as duas operações, não contar automaticamente `Ausencia → Fechamento` na mesma transação.
- Ausencia.save só deriva campos e grava em `app/ponto/models.py:906`; não localizei nele escrita direta de Colaborador.
- LogAuditoria sempre nasce pelo INSERT `app/core/observ.py:134`, dentro de atomic próprio `:133`; se chamado sob atomic externo, é etapa da transação externa.
- A única ordem de duas entidades **existentes** envolvendo Empresa que confirmei neste recorte é `Colaborador → Empresa`, C1, `normalizar_identificadores.py:109`, `:112`. Ordem inversa [não verificado].
- O par AgendamentoFerias/PeriodoAquisitivo tem ambas as ordens textuais: F1 `Periodo :355 → Agendamento :362`; F2 `Agendamento :406 → Periodo :409`, todos em `app/ferias/services.py`. F1 não depende de INSERT para demonstrar a ordem; F2 também não. Os sinais/sync podem introduzir outras entidades intermediárias `app/ferias/models.py:437`.
- Ordem entre linhas tocadas por um UPDATE queryset não é expressa pelo Python; exemplos `app/colaboradores/services/vinculo.py:615`, `:753`, `app/ponto/services/ausencia.py:1267`. Execução real de linhas e IDs atingidos [não verificado].

## 05/11 20:48 · sessao 35b27ecbba74 · - · - · origem=Ronald (tmux)

Censo independente, somente leitura:

**24 chamadas reais de `select_for_update`**, aferidas por AST fora de `tests/`, arquivos `test*` e migrations; todos argumentos vazios, salvo `of=('self', 'disputa')` em `app/chamados/services/relavra_via.py:221`. Não há argumentos `nowait`, `skip_locked` ou `no_key` nessas chamadas. [não verificado: chamadas geradas dinamicamente/dependências externas]

| Arquivo:linha | Função | Modelo |
|---|---|---|
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:100` | `_rodar` | ChamadoColaborador |
| `app/chamados/models.py:674` | `_recarregar_estado_se_mudou` | `type(self)`; ChamadoColaborador neste método |
| `app/chamados/models.py:866` | `abrir` | Colaborador |
| `app/chamados/services/abertura.py:164` | `abrir_chamado_para_colaborador` | Colaborador |
| `app/chamados/services/disputa_emissao.py:467` | `_abrir_ou_agregar_disputa` | DisputaSupervisao |
| `app/chamados/services/disputa_emissao.py:1156` | `criar_disputa_ancora_de_chamado` | DisputaSupervisao |
| `app/chamados/services/fio.py:762` | `_costura_nao_mata_o_fio` | `type(chamado)` |
| `app/chamados/services/relavra_via.py:221` | `desfazer_carimbo_sem_lastro` | PerguntaDisputa **e DisputaSupervisao**, aquisição na mesma consulta |
| `app/colaboradores/models.py:431` | `save` | Colaborador |
| `app/core/services/aplicacao_convencao.py:115` | `cadastrar` | AplicacaoConvencao |
| `app/ferias/views.py:718` | `avaliar_solicitacao` | SolicitacaoFerias |
| `app/holerite/management/commands/processar_lote_holerite.py:54` | `_rodar` | LoteHolerite, alias `L` importado na linha 51 |
| `app/holerite/management/commands/processar_lote_holerite.py:124` | `_rodar` | LoteHolerite |
| `app/ponto/portas/he.py:174` | `decidir_he` | DecisaoHE |
| `app/ponto/portas/he.py:318` | `recusar_ponta_pequena` | DecisaoHE |
| `app/ponto/portas/he.py:445` | `revogar_recusa_do_sistema` | DecisaoHE |
| `app/ponto/portas/he.py:555` | `autorizar_em_lote` | DecisaoHE |
| `app/ponto/services/justificativa.py:38` | `encerrar_chamado_de_origem` | ChamadoColaborador |
| `app/ponto/services/justificativa.py:70` | `_encerrar_bloqueio_de_entrada` | ChamadoColaborador |
| `app/ponto/services/justificativa.py:114` | `travar_origem_antes` | ChamadoColaborador |
| `app/ponto/services/justificativa.py:127` | `aprovar_justificativa_dp` | Justificativa |
| `app/ponto/services/justificativa.py:262` | `rejeitar_justificativa_dp` | Justificativa |
| `app/ponto/services/justificativa.py:329` | `materializar_saida_dp` | Justificativa |
| `app/ponto/views.py:1795` | `aprovar_lote_justificativas` | Justificativa |

`relavra_via.py:221` não permite afirmar ordem PerguntaDisputa→DisputaSupervisao: é uma consulta com dois alvos em `of`; a ordem interna de aquisição está [não verificado]. O `select_related('disputa','celula')` da linha 222 não inclui CelulaDia no `of`. `models.py:673` e `fio.py:761` condicionam o uso de `select_for_update` a `connection.in_atomic_block`.

**Advisory e configurações**

- `app/ponto/services/vigia_de_hora.py:99`: `SELECT pg_try_advisory_lock(%s)`; unlock na linha 136.
- `app/chamados/reconciliador.py:1224`: `SELECT pg_try_advisory_lock(%s)`; unlock na linha 1231; chave por schema calculada em `app/chamados/reconciliador.py:1205`.
- Nenhuma ocorrência executável de `LOCK TABLE` ou advisory `xact_lock` encontrada na varredura da cópia. [não verificado: dependências externas/SQL dinâmico]
- Os dicionários de banco `app/config/settings/saas.py:83` e `app/config/settings/prod.py:9` não definem `ATOMIC_REQUESTS`; não apareceu declaração desse parâmetro na árvore pesquisada. Configuração efetiva de produção [não verificado].
- Padrão de `DJANGO_SETTINGS_MODULE`: SaaS em `app/manage.py:9` e `app/config/wsgi.py:14`; variáveis externas podem substituir o padrão, portanto perfil efetivo [não verificado].

**Domínio residual Pautas: quatro caminhos de UPDATE de entidades existentes**

Todos passam por trilha inserida em LogAuditoria (`app/pautas/services.py:369` → `app/core/observ.py:133`/134), entre Pauta e ContadorPauta.

| Caminho/entrada | Sequência executada dentro do atomic |
|---|---|
| `marcar_lida`, atomic `app/pautas/services.py:203`; view `app/pautas/views.py:95` | Pauta UPDATE `services.py:215` → LogAuditoria INSERT via `services.py:217` → ContadorPauta UPDATE `services.py:354`, chamado na linha 218 |
| `marcar_feita`, atomic `services.py:232`; view `views.py:103` | Pauta UPDATE `services.py:247` → LogAuditoria INSERT via linha 250 → ContadorPauta UPDATE linha 354, chamado na linha 251 |
| `marcar_feita_pelo_sistema`, atomic `services.py:255` | Pauta UPDATE `services.py:272` → LogAuditoria INSERT via linha 275 → ContadorPauta UPDATE linha 354, chamado na linha 276 |
| `cancelar`, atomic `services.py:280`; view `views.py:123` | Pauta UPDATE `services.py:294` → LogAuditoria INSERT via linha 296 → ContadorPauta UPDATE linha 354, chamado na linha 297 |

ContadorPauta pode nascer por `get_or_create` na linha 352, mas recebe UPDATE subsequente na linha 354.

Se INSERT também for considerado na sequência de escritas: `escrever`, atomic `app/pautas/services.py:152`, faz Pauta INSERT linha 180 → LogAuditoria INSERT por linha 186 → ContadorPauta UPDATE linha 354 por chamada linha 188. Entradas: view `app/pautas/views.py:76`, resposta `views.py:112` → service `services.py:194`. Command `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:133` envolve cabeçalho e filhas num atomic externo e chama `escrever` nas linhas 134 e 139.

Contagem local estrita de atualizações: **Pauta antes de ContadorPauta = 4 caminhos; ContadorPauta antes de Pauta = 0 caminhos**; os quatro caminhos e referências estão acima. Ao contar o INSERT de `escrever`, a primeira direção ganha um quinto caminho.

**Relatórios e Mensageria**

- `app/relatorios/lote_espelho.py:102`: atomic de `pedir` contém somente INSERT de LoteEspelho na linha 103. O UPDATE de lote antigo chamado por `vigiar` está antes do bloco (`lote_espelho.py:96`/97 → linha 75).
- Não encontrei atomic declarado em Mensageria; suas escritas sem atomic explícito incluem `mensageria/nucleo/acoes.py:21`, `management/commands/cruzar.py:37`/46, `enviar_outbox.py:71`/77 e `coletar_sinais.py:28`. Inclusão em transação por configuração efetiva externa [não verificado].

**Scripts operacionais fora de app**

Não omitir se “produção” abranger ferramentas de reversão:

- `bin/restore_relavratura_10_2026.py:69`: atomic, itera arquivo externo da linha 32; FechamentoMensal SAVE existente linha 81 → FechamentoMensal DELETE linha 83 → DiaPago DELETE linha 85 → DiaPago bulk INSERT linha 86. Ordem entre FechamentoMensal individuais depende da lista externa [não verificado].
- `bin/restore_o263_comp10.py:40`: atomic; FechamentoMensal SAVE linha 50 → FechamentoMensal DELETE linha 52 → DiaPago DELETE linha 54 → DiaPago bulk INSERT linha 56. Dados vêm do arquivo externo linha 24; asserts de banco SaaS nas linhas 17/18.
- `bin/restore_badge_linha_09_10.py:59`: atomic; somente DiaPago DELETE linha 61 → DiaPago bulk INSERT linha 63.

Esses dois restores dão **FechamentoMensal antes de DiaPago = 2 caminhos; sentido inverso = 0**, sem incluir cascatas de DELETE não avaliadas.

Ferramentas explicitamente de banco sombra:

- `bin/sombra_mascara.py:38`/39 exige banco sombra; atomic linha 51; Colaborador UPDATE linha 59 → User UPDATE linha 60 → repetições Colaborador/User linhas 66/70/72/76 → PushSubscription UPDATE linha 84 → RegistroPDFAuditavel UPDATE linha 88.
- `bin/sombra_mascara_pessoal.py:10` exige sombra; atomic linha 16; modelos determinados pelas tabelas ordenadas de regras em `bin/sombra_regras_pessoais.py:49`,70,60,66,79,80,81,138; UPDATEs/bulk_updates em `sombra_mascara_pessoal.py:20`,25,33,36,41,59,61. Não confundir com caminho de aplicação de produção.
- `bin/sondas_rollback.py:49` envolve código `SONDA_SRC` dinâmico executado na linha 53, sempre termina com `raise _Rollback` linha 61. Entidades/ordens do payload [não verificado].

**Registro de signals e ordem**

Ordem declarada no perfil base: core → colaboradores → ponto → escala → chamados (`app/config/settings/base.py:15`,16,19,20,21). Ordem declarada no SaaS: colaboradores → chamados → escala → ponto → core (`app/config/settings/saas.py:37`,38,39,40,49; composição final linha 53).

Importação dos módulos:
`app/core/apps.py:9`, `app/colaboradores/apps.py:8`, `app/ponto/apps.py:8`, `app/escala/apps.py:9`, `app/chamados/apps.py:10`.

Perfil carregado, registro efetivo e interferência de imports prévios [não verificado]. Portanto não converter ordem de aplicativos declarada automaticamente em ordem efetiva comprovada de disparo entre módulos.

Signal adicional registrado por aliases fora de `signals.py`: TipoAusencia post_save/post_delete, `app/ponto/models.py:1233`/1234, receiver `_invalida_cat_ausencia` linha 1235, só `_CAT.invalidar_cache()` linha 1236; sem escrita de outra entidade nesse corpo. Não encontrei `m2m_changed`, `.connect()` manual de signals ou `Signal()` próprio no código de produção pesquisado. [não verificado: registros externos/dinâmicos]

**Alertas de abrangência**

AST detectou **181 declarações/blocos atomic** em `app/` de produção. Vários usam aliases e escapam de busca literal por `transaction.atomic`; em Chamados, inspecionar especialmente:

- `app/chamados/services/acoes_chamado.py:102`,140;
- `app/chamados/services/regularizacao_ext.py:121`,152,235;
- `app/chamados/services/materializacao.py:1142`;
- `app/chamados/services/fio.py:837`;
- `app/chamados/services/disputa_emissao.py:408`,466,898,1148;
- `app/chamados/models.py:274`,290,616,744,1542,1558,1846.

A cópia não contém `.git` e `git rev-parse HEAD` falhou; correspondência ao HEAD/hash informado [não verificado].

## 05/11 20:11 · sessao 4823c9d8907d · HJ-INGESTAO-0511 · censo · origem=Code

HJ-INGESTAO-0511, padrão=n/a (medida). Inventário: **80 unidades; 50 com colaborador e dia; 30 puladas**. Conferi sintaxe e contagem em memória; execução contra banco e resultados de produção permanecem `[nao verificado]`.

Assinaturas conferidas na cópia:

| Item | Assinatura / modelo real | Prova |
|---|---|---|
| a | `status_do_dia_lavrado(colaborador, data, *, agora=None)` | `app/ponto/status_dia.py:201` |
| b | `dono_da_divergencia(horas_do_dia, em_cadastro_x_realidade, folga_nao_definida=False, sem_vinculo_no_dia=False, sem_vinculo_em_dia_de_trabalho=False, batida_fora_de_turno=False, fase_deslocada=False)` | `app/ponto/management/commands/e6_oraculo.py:53` |
| b, alimentação | `InsumosDoDono.__init__(self, colaborador, ini, fim, hoje, espelho=None, *, agora, dias_de_fase=None, cadastro=None)`; `dono(self, data)` | `app/ponto/management/commands/e6_oraculo.py:128`; `app/ponto/management/commands/e6_oraculo.py:172` |
| c | `classificar_export(empresa, mes, ano, fechamentos=None)` | `app/folha/export.py:746` |
| d | `dias_que_seguram(empresa, mes, ano)` | `app/folha/fora_da_folha.py:102` |
| e | `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` | `app/chamados/juizes.py:1341` |
| f | ORM de `ChamadoColaborador`; campos `modulo_origem` e `status_local`; `data_do_chamado(modulo_origem, contexto_json)` | `app/chamados/models.py:21`; `app/chamados/models.py:40`; `app/chamados/models.py:46`; `app/chamados/catalogo/modulos.py:219` |

Em **b**, o caminho é exatamente `espelho_do_colab → InsumosDoDono → dono`, usado pelo comando. Os insumos vêm das batidas apuráveis no envelope ampliado, dos turnos carregados, do resultado do espelho, das lavras de fase e do cadastro do dia; a sonda não monta outra classificação. `app/ponto/management/commands/e6_oraculo.py:142`; `app/ponto/management/commands/e6_oraculo.py:148`; `app/ponto/management/commands/e6_oraculo.py:154`; `app/ponto/management/commands/e6_oraculo.py:164`; `app/ponto/management/commands/e6_oraculo.py:172`; `app/ponto/management/commands/e6_oraculo.py:304`; `app/ponto/management/commands/e6_oraculo.py:316`; `app/ponto/management/commands/e6_oraculo.py:366`.

Em **c/d**, uma memória local preserva a única execução real do classificador por empresa/competência, inclusive sua exceção: `dias_que_seguram` importa e chama esse mesmo classificador internamente. A competência vem de `janela_atual`; corte diferente de 21 produz erro em b/c/d. `app/folha/fora_da_folha.py:115`; `app/folha/fora_da_folha.py:119`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:60`.

Cole o `exec` abaixo no `manage.py shell` já aberto. `processadas` significa unidades sem erro; as outras categorias são `com_erro` e `puladas`, exclusivas entre si. A leitura usa o instante da execução, sem reconstrução histórica do banco. `[nao verificado]`

===SONDA===
```python
exec(r'''
import sys
sys.dont_write_bytecode = True
import datetime as dt
import json
import logging
from unittest.mock import patch
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

# Inventario literal do anexo fornecido; fonte textual [nao verificado].
UNIDADES = [
    ("HJ-001-U1", 836, "2026-10-13"),
    ("HJ-001-U2", 836, "2026-10-14"),
    ("HJ-002-U1", 909, "2026-10-16"),
    ("HJ-003-U1", 407, "2026-10-21"),
    ("HJ-004-U1", 789, "2026-10-11"),
    ("HJ-004-U2", 789, None),
    ("HJ-005-U1", 42, "2026-10-01"),
    ("HJ-005-U2", 42, "2026-10-08"),
    ("HJ-005-U3", 42, "2026-10-09"),
    ("HJ-005-U4", 42, "2026-10-15"),
    ("HJ-005-U5", 42, "2026-10-22"),
    ("HJ-006-U1", 226, "2026-10-21"),
    ("HJ-007-U1", 253, None),
    ("HJ-008-U1", 283, None),
    ("HJ-009-U1", 504, "2026-10-21"),
    ("HJ-010-U1", 255, "2026-10-03"),
    ("HJ-011-U1", 709, "2026-10-22"),
    ("HJ-012-U1", 239, "2026-10-07"),
    ("HJ-012-U2", 239, "2026-10-12"),
    ("HJ-013-U1", 43, "2026-10-10"),
    ("HJ-013-U2", 43, "2026-10-12"),
    ("HJ-013-U3", 43, "2026-10-26"),
    ("HJ-014-U1", 956, "2026-10-16"),
    ("HJ-014-U2", 956, "2026-10-22"),
    ("HJ-015-U1", 39, "2026-10-15"),
    ("HJ-016-U1", 297, "2026-09-21"),
    ("HJ-016-U2", 297, "2026-10-06"),
    ("HJ-016-U3", 297, "2026-10-13"),
    ("HJ-016-U4", 297, "2026-10-20"),
    ("HJ-016-U5", 297, "2026-10-27"),
    ("HJ-017-U1", 923, "2026-09-27"),
    ("HJ-017-U2", 923, None),
    ("HJ-018-U1", 130, "2026-10-05"),
    ("HJ-019-U1", 29, "2026-10-15"),
    ("HJ-020-U1", 189, "2026-09-21"),
    ("HJ-020-U2", 189, "2026-10-19"),
    ("HJ-021-U1", 204, "2026-09-21"),
    ("HJ-022-U1", 502, "2026-10-09"),
    ("HJ-023-U1", 222, "2026-10-12"),
    ("HJ-023-U2", 256, "2026-10-12"),
    ("HJ-024-U1", 55, "2026-10-03"),
    ("HJ-025-U1", 270, None),
    ("HJ-026-U1", 129, None),
    ("HJ-027-U1", 292, "2026-09-21"),
    ("HJ-027-U2", 292, "2026-10-09"),
    ("HJ-028-U1", 174, "2026-10-25"),
    ("HJ-028-U2", 174, "2026-10-26"),
    ("HJ-029-U1", 33, "2026-10-03"),
    ("HJ-029-U2", 33, "2026-10-04"),
    ("HJ-030-U1", 94, "2026-10-16"),
    ("HJ-031-U1", 247, "2026-10-24"),
    ("HJ-032-U1", 163, None),
    ("HJ-033-U1", 960, None),
    ("HJ-034-U1", 873, "2026-09-21"),
    ("HJ-034-U2", 873, "2026-09-26"),
    ("HJ-035-U1", 949, None),
    ("HJ-036-U1", 207, None),
    ("HJ-037-U1", 332, None),
    ("HJ-038-U1", 446, None),
    ("HJ-039-U1", 89, None),
    ("HJ-040-U1", 92, None),
    ("HJ-041-U1", 231, None),
    ("HJ-042-U1", 214, None),
    ("HJ-043-U1", 235, None),
    ("HJ-044-U1", 246, None),
    ("HJ-045-U1", 250, None),
    ("HJ-046-U1", 673, None),
    ("HJ-047-U1", 194, None),
    ("HJ-048-U1", 219, None),
    ("HJ-049-U1", 288, "2026-10-04"),
    ("HJ-050-U1", 523, "2026-10-25"),
    ("HJ-050-U2", 523, "2026-11-07"),
    ("HJ-051-U1", 123, None),
    ("HJ-052-U1", 1023, None),
    ("HJ-052-U2", 78, None),
    ("HJ-053-U1", None, None),
    ("HJ-054-U1", None, None),
    ("HJ-055-U1", None, None),
    ("HJ-056-U1", 270, None),
    ("HJ-056-U2", None, None),
]
assert len(UNIDADES) == len({u[0] for u in UNIDADES}) == 80
for uid, cid, iso in UNIDADES:
    assert cid is None or type(cid) is int
    if iso is not None:
        assert dt.date.fromisoformat(iso).isoformat() == iso

def erro(exc):
    # Sem mensagem SQL, nomes ou documentos na saida.
    return {"ERRO": type(exc).__name__,
            "sqlstate": getattr(exc, "sqlstate", None)
                        or getattr(getattr(exc, "__cause__", None), "pgcode", None)}

def somente_select(execute, sql, params, many, context):
    if not sql.lstrip().upper().startswith("SELECT"):
        raise RuntimeError("SQL_fora_da_leitura")
    return execute(sql, params, many, context)

def leitura(fn):
    if connection.vendor != "postgresql" or connection.in_atomic_block:
        raise RuntimeError("exige_postgresql_sem_transacao_previa")
    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION READ ONLY")
        with connection.execute_wrapper(somente_select):
            with connection.cursor() as cur:
                cur.execute("SELECT current_schema()")
                if cur.fetchone()[0] != "juliani":
                    raise RuntimeError("schema_diferente_de_juliani")
            return fn()

def tentar(fn):
    try:
        return True, leitura(fn)
    except Exception as exc:
        return False, erro(exc)

def uma_vez(memo, chave, fn):
    if chave not in memo:
        try:
            memo[chave] = (True, fn())
        except Exception as exc:
            memo[chave] = (False, exc)
    ok, valor = memo[chave]
    if not ok:
        raise valor
    return valor

def tem_erro(valor):
    if isinstance(valor, dict):
        return "ERRO" in valor or any(tem_erro(x) for x in valor.values())
    if isinstance(valor, (list, tuple)):
        return any(tem_erro(x) for x in valor)
    return False

def emitir(obj):
    print(json.dumps(obj, ensure_ascii=False, default=str), flush=True)

def executar():
    from colaboradores.models import Colaborador
    from escala.models import CelulaDia
    from ponto.status_dia import status_do_dia_lavrado
    from ponto.management.commands.e6_oraculo import InsumosDoDono
    from ponto.services.espelho import espelho_do_colab
    from ponto.janelas import corte_da_empresa, janela_atual
    from folha import export
    from folha.fora_da_folha import dias_que_seguram
    from chamados.juizes import porta_alcancavel_do_dia
    from chamados.models import ChamadoColaborador
    from chamados.catalogo.modulos import data_do_chamado

    agora = timezone.now()
    hoje = timezone.localdate(agora)
    real_classificar = export.classificar_export
    memo_export, memo_dias = {}, {}
    totais = {"processadas": 0, "com_erro": 0, "puladas": 0}

    # Proxy apenas em memoria: a chamada interna de d reutiliza c.
    # app/folha/fora_da_folha.py:115 e :119.
    def classificar_uma_vez(empresa, mes, ano, fechamentos=None):
        if fechamentos is not None:
            raise RuntimeError("sonda_exige_fechamentos_gravados")
        return uma_vez(
            memo_export, (empresa.pk, mes, ano),
            lambda: real_classificar(empresa, mes, ano),
        )

    def competencia(c, dia):
        corte = corte_da_empresa(c.empresa)
        if corte != 21:
            raise ValueError("corte_da_empresa_diverge_de_21")
        ini, fim = janela_atual(dia, c.empresa)
        return ini, fim, fim.month, fim.year

    def dono(c, dia, comp):
        ini, fim, mes, ano = comp
        # Mesmo caminho E6: e6_oraculo.py:304, :316 e :366.
        esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
        ins = InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
        return {"dono": ins.dono(dia)}

    def balde(c, comp):
        ini, fim, mes, ano = comp
        itens = export.classificar_export(c.empresa, mes, ano)
        achados = [i for i in itens if i["colaborador_id"] == c.pk]
        if len(achados) > 1:
            raise ValueError("mais_de_um_item_do_colaborador")
        cab = {"empresa_id": c.empresa_id, "mes": mes, "ano": ano,
               "inicio": ini, "fim": fim}
        if not achados:
            return {**cab, "estado": "sem item no universo"}
        i = achados[0]
        return {**cab, "status": i["status"], "motivo": i["motivo"],
                "travas_aprovacao": i["travas_aprovacao"]}

    def segura(c, dia, comp):
        ini, fim, mes, ano = comp
        universo = uma_vez(
            memo_dias, (c.empresa_id, mes, ano),
            lambda: dias_que_seguram(c.empresa, mes, ano),
        )
        itens = [i for i in universo["itens"]
                 if i["colaborador_id"] == c.pk]
        if not itens:
            return {"estado": "indeterminado",
                    "motivo": "sem item no universo", "segura": None}
        if c.pk not in universo["dias"]:
            if c.pk in universo["furo"]:
                raise RuntimeError("furo_sem_dias_carregados")
            return {"segura": False, "dias": [],
                    "escopo": "fora do universo furo_espelho",
                    "motivo_export": itens[0]["motivo"]}
        motivos, dias = universo["dias"][c.pk]
        encontrados = [x for x in dias if x["data"] == dia]
        return {"segura": bool(encontrados),
                "dias": encontrados, "motivos_competencia": motivos}

    def porta(c, dia):
        try:
            cel = CelulaDia.objects.only("pk").get(
                colaborador_id=c.pk, data=dia)
        except CelulaDia.DoesNotExist:
            return "sem celula"
        return {"celula_id": cel.pk,
                "porta": porta_alcancavel_do_dia(cel.pk)}

    def chamados(c, dia):
        encontrados, sem_dia, erros = [], [], []
        qs = ChamadoColaborador.objects.filter(
            colaborador_id=c.pk
        ).only("pk", "modulo_origem", "contexto_json", "status_local").order_by("pk")
        for ch in qs:
            try:
                raw = data_do_chamado(ch.modulo_origem, ch.contexto_json)
                if raw is None or raw == "":
                    sem_dia.append(ch.pk)
                    continue
                data = dt.date.fromisoformat(str(raw)[:10])
                if data == dia:
                    encontrados.append({
                        "id": ch.pk, "tipo": ch.modulo_origem,
                        "status_local": ch.status_local,
                    })
            except Exception as exc:
                erros.append({"chamado_id": ch.pk, **erro(exc)})
        saida = {"chamados": encontrados, "sem_dia_catalogado_ids": sem_dia}
        return {**saida, "ERRO": erros} if erros else saida

    with schema_context("juliani"), patch.object(
        export, "classificar_export", classificar_uma_vez
    ), patch.object(timezone, "now", return_value=agora):
        for uid, cid, iso in UNIDADES:
            linha = {"uid": uid, "col": cid, "dia": iso}
            if cid is None or iso is None:
                motivos = []
                if cid is None:
                    motivos.append("nao casado")
                if iso is None:
                    motivos.append("sem dia")
                emitir({**linha, "estado": "indeterminado",
                        "motivo": "; ".join(motivos)})
                totais["puladas"] += 1
                continue

            dia = dt.date.fromisoformat(iso)
            linha["instante_leitura"] = agora.isoformat()
            linha["data_futura_na_leitura"] = dia > hoje
            ok_c, c = tentar(lambda: Colaborador.objects.select_related(
                "empresa").get(pk=cid))
            if not ok_c:
                for letra in "abcdef":
                    linha[letra] = {**c, "dependencia": "colaborador"}
            else:
                ok_comp, comp = tentar(lambda: competencia(c, dia))
                chamadas = {
                    "a": lambda: status_do_dia_lavrado(c, dia, agora=agora),
                    "e": lambda: porta(c, dia),
                    "f": lambda: chamados(c, dia),
                }
                if ok_comp:
                    chamadas = {
                        **chamadas,
                        "b": lambda: dono(c, dia, comp),
                        "c": lambda: balde(c, comp),
                        "d": lambda: segura(c, dia, comp),
                    }
                for letra in "abcdef":
                    if letra not in chamadas:
                        linha[letra] = {**comp, "dependencia": "competencia"}
                    else:
                        ok, valor = tentar(chamadas[letra])
                        linha[letra] = valor

            if tem_erro(linha):
                totais["com_erro"] += 1
            else:
                totais["processadas"] += 1
            emitir(linha)

    soma = sum(totais.values())
    assert soma == 80
    emitir({"resumo": {**totais, "soma": soma}})

# Evita escrita pelos handlers de arquivo durante as chamadas.
# Configuracao da copia: app/config/settings/saas.py:138 e :162.
nivel_anterior = logging.root.manager.disable
try:
    logging.disable(sys.maxsize)
    executar()
finally:
    logging.disable(nivel_anterior)
''')
```
===FIM===

A ligação abaixo é **por classe sugerida**. As referências comprovam o escopo documental da obra; o enquadramento atual de cada unidade continua `[nao verificado]`. “Cond.” indica uma condição ainda não demonstrada. “Sem obra” significa ausência de associação sustentada ao recorte `backlog_obras.txt` fornecido. `[nao verificado]`

Preservo as três unidades não classificadas e a unidade futura do censo; a unidade futura continua elegível para chamadas na sonda porque possui colaborador e data. `[nao verificado]`

| uid | grupo | obra | prova |
|---|---|---|---|
| HJ-001-U1 | G2 | O261-E-CELULA-20-UM-DONO; F3-PORTA-E-CRON-PRAZO, cond. | `app/docs/BACKLOG.md:859`; `app/docs/BACKLOG.md:1739`; caso `[nao verificado]` |
| HJ-001-U2 | G2 | O261-E-CELULA-20-UM-DONO; F3-PORTA-E-CRON-PRAZO, cond. | `app/docs/BACKLOG.md:859`; `app/docs/BACKLOG.md:1739`; caso `[nao verificado]` |
| HJ-002-U1 | G2 | O261-E-CELULA-20-UM-DONO, cond. | `app/docs/BACKLOG.md:859`; caso `[nao verificado]` |
| HJ-003-U1 | G2 | O261-E-CELULA-20-UM-DONO, cond. | `app/docs/BACKLOG.md:859`; caso `[nao verificado]` |
| HJ-004-U1 | G3 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-004-U2 | G3 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-005-U1 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-005-U2 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:763`; caso `[nao verificado]` |
| HJ-005-U3 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; caso `[nao verificado]` |
| HJ-005-U4 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:763`; caso `[nao verificado]` |
| HJ-005-U5 | G4 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:763`; caso `[nao verificado]` |
| HJ-006-U1 | G1 | sem obra | Tela e natureza do “pendente” `[nao verificado]` |
| HJ-007-U1 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-008-U1 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-009-U1 | G1 | sem obra | Origem do cartão amarelo `[nao verificado]` |
| HJ-010-U1 | G1 | sem obra | Porta/tela concreta `[nao verificado]` |
| HJ-011-U1 | G1 | C20-FIO-MUDO, se faltar emissão de pergunta | `app/docs/BACKLOG.md:1762`; caso `[nao verificado]` |
| HJ-012-U1 | G5 | sem obra | Classe específica de QRF/saída `[nao verificado]` |
| HJ-012-U2 | G5 | sem obra | Decisão e eventual defeito de feriado `[nao verificado]` |
| HJ-013-U1 | G1 | sem obra | Tipo da retratação e tela `[nao verificado]` |
| HJ-013-U2 | G5 | sem obra | Decisão de feriado/diária `[nao verificado]` |
| HJ-013-U3 | G1 | sem obra | Tipo da retratação e tela `[nao verificado]` |
| HJ-014-U1 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-014-U2 | G1 | O256; O267 | `app/docs/BACKLOG.md:1066`; `app/docs/BACKLOG.md:1508`; caso `[nao verificado]` |
| HJ-015-U1 | G4 | sem obra | Ato de ausência e elo sem efeito `[nao verificado]` |
| HJ-016-U1 | G3 | O276 | `app/docs/BACKLOG.md:1517`; estado atual `[nao verificado]` |
| HJ-016-U2 | G5 | O214 | `app/docs/BACKLOG.md:1134`; caso `[nao verificado]` |
| HJ-016-U3 | G5 | O214 | `app/docs/BACKLOG.md:1134`; caso `[nao verificado]` |
| HJ-016-U4 | G5 | O214 | `app/docs/BACKLOG.md:1134`; caso `[nao verificado]` |
| HJ-016-U5 | G5 | O214 | `app/docs/BACKLOG.md:1134`; autorização individual `[nao verificado]` |
| HJ-017-U1 | G3 | O232 | `app/docs/BACKLOG.md:1157`; caso `[nao verificado]` |
| HJ-017-U2 | G3 | O232 | `app/docs/BACKLOG.md:1157`; dia individual `[nao verificado]` |
| HJ-018-U1 | G5 | BO-ADMIN-2710 | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:757`; estado atual `[nao verificado]` |
| HJ-019-U1 | G3 | O259, quanto ao par | `app/docs/BACKLOG.md:1445`; divergência individual `[nao verificado]` |
| HJ-020-U1 | G5 | BO-ADMIN-2710, quanto à auditoria sem ação | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:757`; caso `[nao verificado]` |
| HJ-020-U2 | G5 | BO-ADMIN-2710, quanto à auditoria sem ação | `app/docs/BACKLOG.md:754`; `app/docs/BACKLOG.md:757`; caso `[nao verificado]` |
| HJ-021-U1 | G1 | O267 | `app/docs/BACKLOG.md:1508`; dias responsáveis `[nao verificado]` |
| HJ-022-U1 | G1 | O267 | `app/docs/BACKLOG.md:1508`; dias responsáveis `[nao verificado]` |
| HJ-023-U1 | G5 | sem obra | Causa individual da diferença entre competências `[nao verificado]` |
| HJ-023-U2 | G5 | sem obra | Causa individual da diferença entre competências `[nao verificado]` |
| HJ-024-U1 | G1 | sem obra | Classe herdada do censo; contradição concreta `[nao verificado]` |
| HJ-025-U1 | G5 | H12-DEMISSAO-NA-JANELA | `app/docs/BACKLOG.md:1741`; data de desligamento `[nao verificado]` |
| HJ-026-U1 | G3 | O259, quanto ao par | `app/docs/BA

[truncado em 20000 de 23935 caracteres]

_2000 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
