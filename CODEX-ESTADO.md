# CODEX-ESTADO

> Ultima resposta processada: **23/11/2026 14:25** · publicado em **23/11/2026 14:29** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 23/11 14:25 · sessao c62153d77d08 · PAR-PELO-TURNO · censo · origem=Code

# PAR-PELO-TURNO · Censo da cópia

O censo usa o código da cópia e o recorte raspado do enunciado. A reprodução foi isolada, com as duas batidas informadas por noite e a escala fornecida. A correspondência da cópia com o SHA, os insumos completos de produção e os valores lavrados ficam **[nao verificado]**.

## 1. O que o pareador devolve hoje

O fechamento principal lê `hi/hf` do mapa pela **data local da entrada**. Com duas bordas distintas, calcula `(hf − hi) módulo 24 h + 4 h`. Sem essas bordas, usa 26 h. A L-084 também faz voltar a 26 h se **as duas** pontas estiverem a **mais de 180 min** dos respectivos marcos. A separação ocorre quando a duração real **excede** o teto escolhido. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:326`, `app/ponto/turnos.py:1430`, `app/ponto/motor_calculo_v2.py:1931`.

| Caso | Resultado e ramo |
|---|---|
| **col129 · 15→16/11** | **Dois objetos `Turno`, zero pares fechados.** Em 15/11, `E 17:30` fica aberta e sem saída. Em 16/11, `S 06:30` fica órfã e sem entrada. A escala 21:30–05:42 dá **492 + 240 = 732 min**. O vão real é **780 min** e as distâncias são **240 min na E e 48 min na S**. A S ancora a jornada, portanto a exceção das duas pontas distantes fica inativa. O teto de 732 min separa, carimba a S e impede o pós-fechamento. O teste de outubro usa as mesmas horas e fixa esse resultado. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1440`, `app/ponto/turnos.py:1451`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:52`. |
| **col155 · 15→16/11** | Com a mesma escala no mapa, **dois objetos `Turno`, zero pares fechados**. `E 17:29` fica aberta em 15/11 e `S 06:32` fica órfã em 16/11. São **783 min** contra **732 min**, com distâncias **241 e 50 min**. A reprodução isolada retornou o mesmo ramo e o mesmo carimbo. Ata, marcas adicionais e eventual segmentação real por vínculo **[nao verificado]**. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1451`, `app/ponto/turnos.py:1962`. |
| **col214 · 15/11** | O recorte dá a **órfã 07:00**. Tipo efetivo, par candidato, bordas previstas, quantidade de turnos e ramo exato **[nao verificado]**. Uma S sem abertura já vira `Turno` sem entrada. A grade também pode publicar sobra por falta de atribuição a um marco. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1495`, `app/escala/utils.py:635`, `app/escala/utils.py:1822`. |
| **col107 · 15/11** | O recorte dá **um par conhecido, 10:13–14:25**, e as órfãs **07:05 e 09:15**. Elas antecedem a entrada real em **188 e 58 min**. A distância ao marco **previsto**, os tipos e papéis das órfãs, o total de objetos `Turno` e o ramo que as deixou de fora ficam **[nao verificado]**. A reconciliação existente busca as próprias bordas de um par fechado quando elas aparecem como órfãs da grade. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:1417`, `app/escala/utils.py:1956`, `app/escala/utils.py:1986`, `app/escala/utils.py:2010`. |

Na reprodução das duas noites, passar só os marcos do template, sem `marcos_por_dia`, produziu **um par fechado em cada caso** pelo envelope de 26 h. Essa diferença de alimentação importa para o censo de leitores. `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1492`.

## 2. Adicional noturno e prorrogação após 05:00

**A conta está em `motor_calculo_v2.py`, na zona que exige aval explícito. O par fechado já chega à conta pelo caminho existente.** O encadeamento de produção é o seguinte. `CLAUDE.md:143`, `app/ponto/services/fechamento.py:286`, `app/ponto/services/fechamento.py:735`.

1. `calcular_mes` → `montar_periodos` da classe do motor → `_periodos_via_autoridade` → `turnos_via_autoridade` → `parear_turnos`. O motor alimenta o pareador com os marcos disponíveis, os parâmetros da classe e o papel da ata quando carregado. `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:509`, `app/ponto/motor_calculo_v2.py:2354`, `app/ponto/motor_calculo_v2.py:2748`, `app/ponto/motor_calculo_v2.py:2875`, `app/ponto/motor_calculo_v2.py:3213`, `app/ponto/motor_calculo_v2.py:3532`.
2. `adaptador_motor.periodos_de_turnos` devolve os timestamps e as pausas julgadas por `intervalos_do_turno`. A **S órfã só gera anomalia**. A **E aberta vira `(entrada, None)`**. Em seguida, `_periodos_pelo_marco` pode substituir os períodos do dia pelo par de `juiz_batida.periodos_do_dia`, se a ata estiver alinhada, com as duas bordas, correspondência com as batidas e cobertura do miolo. Ata aberta ou desalinhada deixa o resultado geométrico seguir. `app/ponto/adaptador_motor.py:24`, `app/ponto/adaptador_motor.py:29`, `app/ponto/motor_calculo_v2.py:530`, `app/ponto/motor_calculo_v2.py:616`, `app/ponto/motor_calculo_v2.py:632`, `app/ponto/motor_calculo_v2.py:669`, `app/ponto/motor_calculo_v2.py:685`.
3. `calcular_periodo` retorna antes da conta quando falta S. Para um par fechado, apura sobre as **bordas efetivas após a janela de HE**, com as pausas recortadas. `_escrever_noturnas` chama `calcular_noturnas`, que usa `minutos_noturnos`, desconta o noturno das pausas e aplica o fator legal ou convencional. `minutos_noturnos` cruza o período com **22:00–05:00** e, com a flag ativa e interseção no segmento **00:00–05:00**, estende esse segmento até a saída efetiva. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:969`, `app/ponto/motor_calculo_v2.py:994`, `app/ponto/motor_calculo_v2.py:1914`, `app/ponto/motor_calculo_v2.py:2458`, `app/ponto/motor_calculo_v2.py:2750`, `app/ponto/motor_calculo_v2.py:2792`, `app/ponto/motor_calculo_v2.py:2877`, `app/ponto/motor_calculo_v2.py:3215`, `app/ponto/motor_calculo_v2.py:3260`.
4. Fora da zona, `calculador.regras.noturnas_do_dia` também soma os pares fechados do dia, usando a mesma função de minutos e o mesmo desconto de pausas do motor. A alimentação vem dos segmentos efetivos do `ResultadoMes`. O adicional mensal soma os minutos legais dos períodos normais e de folga trabalhada. O subconjunto de **HE noturna** tem a função própria `he_noturna`. `app/ponto/calculador/alimentacao.py:35`, `app/ponto/calculador/alimentacao.py:525`, `app/ponto/calculador/regras.py:66`, `app/ponto/calculador/regras.py:844`, `app/ponto/motor_calculo_v2.py:303`, `app/ponto/motor_calculo_v2.py:409`, `app/ponto/services/fechamento.py:918`.

**Para col129, sim, se o par fechado chegar à chamada do motor e sobreviver à seleção pela ata.** Nesse caso a conta de noturno já é chamada, sem alteração em `motor_calculo_v2.py`. A prorrogação paga depende ainda da saída efetiva e da régua. O piso CLT do código desliga a prorrogação no 12x36 e a CCT usa o flag cadastrado. Na reprodução aritmética com `17:30–06:30` integral e sem pausa, a função contou **510 min de relógio com prorrogação e 420 sem ela**. Ciclo, regime, CCT, pausa, clipe e rubrica efetiva de col129 **[nao verificado]**. `app/ponto/motor_calculo_v2.py:82`, `app/ponto/motor_calculo_v2.py:1752`, `app/ponto/motor_calculo_v2.py:2750`, `app/core/regua_cct.py:153`, `app/core/regua_cct.py:450`, `app/core/regua_cct.py:466`.

## 3. Teto de fechamento e leitores que podem divergir

| Sítio | Quem calcula, quem lê e o efeito |
|---|---|
| **Teto da jornada** | `_teto_da_jornada` calcula duração prevista bruta + 4 h ou devolve o envelope recebido. Tem duas chamadas AST, a do fechamento principal e a de `_teto_s_da_jornada`. Esta última tem três chamadas, nos dois pós-fechamentos e em `_cabe_na_jornada_da_vespera`. `app/ponto/turnos.py:63`, `app/ponto/turnos.py:66`, `app/ponto/turnos.py:230`, `app/ponto/turnos.py:1086`, `app/ponto/turnos.py:1177`, `app/ponto/turnos.py:1434`. |
| **Fechamento principal** | `parear_turnos` escolhe o mapa da E, passa **26 h** como envelope, mede a L-084 com `_dist_marco` e compara a duração. `_dist_marco` devolve a menor distância em minutos ao marco nas três datas em torno da batida. O corte de **180 min** é declarado por `MotorBase`. As 26 h são fallback do ramo e critério de carimbo, sem `min(teto_do_dia, 26 h)` no cálculo. `app/ponto/turnos.py:78`, `app/ponto/turnos.py:326`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1451`, `app/ponto/motor_calculo_v2.py:1931`. |
| **Pós-fechamentos** | `_fechar_aberto_com_saida_seguinte` tenta a S órfã seguinte ou a candidata posicional da L-115. `_fechar_aberto_na_pausa_sem_volta` tenta a última ida à pausa nas condições de fato encerrado. Ambos medem da E real e usam o teto do `data_turno`, com fallback `cont_max_s`. A exceção das duas pontas distantes está no fechamento principal. `app/ponto/turnos.py:943`, `app/ponto/turnos.py:1064`, `app/ponto/turnos.py:1084`, `app/ponto/turnos.py:1165`, `app/ponto/turnos.py:1177`, `app/ponto/turnos.py:1440`, `app/ponto/turnos.py:1501`. |
| **Saída pertence à véspera?** | `_mk` chama `_data_do_turno`, que chama `_cabe_na_jornada_da_vespera` para a S órfã e para a entrada de madrugada com S em condição de herdar o dia. Usa o mesmo número, ancorado no **`hi` previsto da véspera**, inclusive quando a E real falta. Decide atribuição de dia. `app/ponto/turnos.py:216`, `app/ponto/turnos.py:285`, `app/ponto/turnos.py:298`, `app/ponto/turnos.py:302`, `app/ponto/turnos.py:318`. |
| **`cont_max_s` e absorção** | O padrão do pareador e `MotorBase.AUT_CONT_MAX_S` são **14 h**. `MotorPlantao` fornece **26 h** pelo adaptador do motor. `_marca_intra_duracao` lê esse envelope nas guardas de continuidade, inclusive na sequência examinada antes de classificar pausa deslocada. A guarda de absorção pode deixar o turno aberto antes do ramo de fechamento se a marca ultrapassa o envelope e está a até 90 min de marco interno. `_marca_ecos_flush` tem 14 h no próprio default e os seus três chamadores o deixam no default. `app/ponto/turnos.py:344`, `app/ponto/turnos.py:370`, `app/ponto/turnos.py:414`, `app/ponto/turnos.py:451`, `app/ponto/turnos.py:482`, `app/ponto/turnos.py:1349`, `app/ponto/turnos.py:1475`, `app/ponto/turnos.py:1533`, `app/ponto/turnos.py:1941`, `app/ponto/turnos.py:2284`, `app/ponto/motor_calculo_v2.py:505`, `app/ponto/motor_calculo_v2.py:729`, `app/ponto/motor_calculo_v2.py:3513`. |
| **L-084 em ata, grade e dinheiro** | `papel_por_minuto_da_ata` e a reconciliação da grade leem os mesmos 180 min para saber se os marcos descrevem o turno. O motor lê `_cadastro_nao_descreve` para janela de HE e pontualidade. A duração total admissível e a distância a um marco são perguntas distintas. `app/ponto/turnos.py:1695`, `app/escala/utils.py:2001`, `app/ponto/motor_calculo_v2.py:1562`, `app/ponto/motor_calculo_v2.py:1606`, `app/ponto/motor_calculo_v2.py:1933`, `app/ponto/motor_calculo_v2.py:1988`. |
| **Carimbo, lista C9 e cron** | `_declara_separado_pelo_teto` escreve na instância da S quando o teto do dia foi decisivo e o vão cabia em 26 h. `pares_separados_pelo_teto` lê o resultado final e requer que a S continue órfã e a E continue aberta. Seu único chamador direto de produção é `teto_na_lista.medir`. `lavrar_teto_do_dia` grava `MetricaSnapshot`, `ultimo_lavrado` lê e `cadastro_realidade._teto_do_dia` mostra a C9. O texto da C9 também explicita a regra antiga. `app/ponto/turnos.py:165`, `app/ponto/turnos.py:183`, `app/ponto/turnos.py:1451`, `app/ponto/services/teto_na_lista.py:68`, `app/ponto/services/teto_na_lista.py:110`, `app/ponto/services/teto_na_lista.py:120`, `app/ponto/management/commands/lavrar_teto_do_dia.py:26`, `app/escala/services/cadastro_realidade.py:104`, `app/escala/services/cadastro_realidade.py:344`, `app/escala/services/cadastro_realidade.py:924`. |
| **Outras guardas sobre o vão total** | `_papeis_por_alternancia` recusa `>26 h` ao atribuir papéis a múltiplos períodos na grade. `_orfao_resolvido_cross_meianoite` descarta um **par já fechado** de `>26 h` para decidir se retira a órfã da fábrica. `minutos_noturnos` devolve zero para recorte `>26 h`. `MAX_TURNO_MIN` está declarado no motor, com leitor de produção **[nao verificado]**. No oráculo sem pares informados, existe corte de vão `>14 h`; a alimentação monetária passa `pares=`. `app/escala/utils.py:477`, `app/escala/utils.py:493`, `app/escala/utils.py:608`, `app/chamados/services/disputa_emissao.py:722`, `app/chamados/services/disputa_emissao.py:794`, `app/ponto/motor_calculo_v2.py:94`, `app/ponto/motor_calculo_v2.py:204`, `app/ponto/calculador/nucleo.py:110`, `app/ponto/calculador/nucleo.py:215`, `app/ponto/calculador/alimentacao.py:449`, `app/ponto/management/commands/e6_oraculo.py:153`. |

**Onde o cadastro precisa decidir.** O fechamento principal, os dois pós-fechamentos e o envelope de absorção precisam receber a mesma resposta empresarial sempre que decidirem se uma marca pertence à mesma jornada. A atribuição à véspera também precisa consumir essa resposta onde usa a duração admissível para decidir pertencimento, observando que hoje mede a partir de `hi`. A grade por alternância e o filtro de órfã cross-meia-noite têm veto próprio de 26 h e podem contrariar um par admitido acima disso. A proteção de 26 h do noturno também teria efeito monetário nesse cenário. C9, o snapshot e a apresentação leem o **motivo e o teto já decididos**. O valor inicial do cadastro e a possibilidade de superar 26 h ficam **[nao verificado]**. `aval_v2_anexo.txt:1`, `app/ponto/turnos.py:216`, `app/ponto/turnos.py:1086`, `app/ponto/turnos.py:1177`, `app/ponto/turnos.py:1349`, `app/ponto/turnos.py:1430`, `app/escala/utils.py:493`, `app/chamados/services/disputa_emissao.py:727`, `app/ponto/motor_calculo_v2.py:94`, `app/ponto/services/teto_na_lista.py:68`.

**Vão de 11 h.** O pareador atual não consulta `S→E >= 11 h`. A linha **2400** do motor itera sobre períodos já montados, e **2405–2423** permitem que um período herde o dia do anterior quando o vão é de **0 a 6 h** e há uma saída anterior real. Isso muda a atribuição de dia. A inferência de pausa do pareador tem outra régua e a expiração da aberta usa **fim previsto + 4 h**. As 11 h aparecem no mapa do art. 66 e no corte de permuta, e o helper de cobertura declara expressamente que não as valida. `app/ponto/motor_calculo_v2.py:2400`, `app/ponto/motor_calculo_v2.py:2405`, `app/ponto/motor_calculo_v2.py:2423`, `app/ponto/turnos.py:382`, `app/ponto/turnos.py:1408`, `app/ponto/turnos.py:2021`, `app/docs/mapa_seletor_cobranca.md:27`, `app/docs/PROMPTS.md:2015`, `app/ponto/utils.py:141`.

## 4. Cadastro por empresa e molde da UI

- **`Empresa`** reúne `regime_trabalhista`, o corte de competência e os parâmetros da janela de HE, incluindo vigência, piso, tetos das pontas e limite de decisão. O motor lê a empresa em `_janela_do_dia`. O pareador recebe batidas, marcos, papéis e parâmetros numéricos, sem campo de empresa na assinatura atual. O campo **“maior jornada contínua real”** no model e no formulário atual **[nao verificado]**. `app/colaboradores/models.py:40`, `app/colaboradores/models.py:46`, `app/colaboradores/models.py:58`, `app/colaboradores/models.py:82`, `app/colaboradores/models.py:117`, `app/colaboradores/models.py:150`, `app/ponto/motor_calculo_v2.py:1380`, `app/ponto/motor_calculo_v2.py:1650`, `app/ponto/turnos.py:1192`.
- **Régua CCT e `Sindicato`.** `regua_para` lê `Empresa.regime_trabalhista`. Em CCT, `_aplicacao_vigente` resolve `AplicacaoConvencao` da empresa e o recorte de praça declarado. `get_motor_cct` injeta a régua de excedente e os flags aplicáveis de noturno e feriado. `Sindicato.teto_prorrogacao_dia_min` de 120 min refere-se à prorrogação diária do art. 59 e está declarado como **sem efeito no cálculo**, assim como os campos de compensação da CCT. `app/core/models.py:63`, `app/core/models.py:70`, `app/core/models.py:119`, `app/core/regua_cct.py:72`, `app/core/regua_cct.py:231`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:428`, `app/core/regua_cct.py:466`.
- **Molde existente com nome e leitor.** O template `empresa_form.html` exibe **Regime trabalhista** e o campo numérico **Dia de início da apuração (corte da folha)**. As rotas de nova e editar Empresa chamam `form_empresa`. Ela lê o POST, valida e delega para `criar_empresa` ou `atualizar_empresa`. Os leitores operacionais são `regua_para` e `janelas.corte_da_empresa`, respectivamente. Os parâmetros `janela_he_*` são outro precedente no model e no motor. `app/templates/colaboradores/empresa_form.html:39`, `app/templates/colaboradores/empresa_form.html:53`, `app/colaboradores/urls.py:28`, `app/colaboradores/views.py:504`, `app/colaboradores/views.py:531`, `app/colaboradores/views.py:539`, `app/colaboradores/views.py:550`, `app/colaboradores/services/estrutura.py:13`, `app/core/regua_cct.py:352`, `app/ponto/janelas.py:42`, `app/ponto/motor_calculo_v2.py:1650`.

## 5. Como chega à Gestão de HE

**O caminho já existe, mas fechar o par não garante que as duas pontas apareçam.** A tela lê o **retrato lavrado** por empresa e competência, via `he_pendente_lavrado.ler` → `gestao_he.enriquecer` → `gestao_he.html`. O escritor `lavrar_he_pendente` chama `apurar`, que passa por `espelho_do_colab` → `montar_dias` e recolhe `dia['he_fora_da_janela']`. Grava o retrato em `MetricaSnapshot`. A leitura da tela só vê o novo resultado depois dessa lavra. `app/ponto/views.py:948`, `app/ponto/views.py:969`, `app/ponto/views.py:978`, `app/ponto/management/commands/lavrar_he_pendente.py:62`, `app/ponto/management/commands/lavrar_he_pendente.py:70`, `app/ponto/services/he_pendente_lavrado.py:87`, `app/ponto/services/he_pendente_lavrado.py:132`, `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/services/he_pendente_lavrado.py:193`, `app/ponto/services/espelho.py:1298`.

Com a janela empresarial **ativa e vigente**, o espelho marca as pontas reconhecidas na grade acima do piso, descontando a extra declarada no vínculo efetivo. Para marco faltante, pode aproveitar o relato de Cadastro × Realidade do motor se ele nomear o mesmo marco e a ponta ainda não estiver marcada. O motor só publica essa causa no relato quando a sobra **passa do teto daquela ponta**. A saída depende ainda de `janela_he_saida_ativa`. Assim, uma ponta sem atribuição na grade e sem relato elegível pode continuar fora da Gestão de HE mesmo com o par fechado. Os flags e o retrato dos quatro casos **[nao verificado]**. `app/ponto/services/espelho.py:735`, `app/ponto/services/espelho.py:754`, `app/ponto/services/espelho.py:790`, `app/ponto/janela_he.py:156`, `app/ponto/janela_he.py:267`, `app/ponto/janela_he.py:289`, `app/ponto/janela_he.py:293`, `app/ponto/motor_calculo_v2.py:1702`, `app/ponto/motor_calculo_v2.py:1733`, `app/ponto/motor_calculo_v2.py:1747`.

**A classificação é do motor**, pela escala efetiva, extra declarada, janela, pausas, excedente e regras da família, inclusive banco no comercial. A Gestão de HE apresenta as pontas para decisão. `decidir_he` grava a decisão por pessoa e dia e aciona o recálculo. A compensação cadastrada em `Sindicato` está sem efeito no cálculo. `aval_v2_anexo.txt:1`, `app/escala/models.py:1221`, `app/ponto/motor_calculo_v2.py:1752`, `app/ponto/motor_calculo_v2.py:2793`, `app/ponto/motor_calculo_v2.py:3386`, `app/ponto/motor_calculo_v2.py:3426`, `app/core/regua_cct.py:77`, `app/ponto/portas/he.py:159`, `app/ponto/portas/he.py:186`, `app/ponto/portas/he.py:230`.

## 6. Censo AST e alcance das mudanças

**13 chamadas diretas a `parear_turnos` em código de produção e comandos**, contando aliases, fora de testes e migrações. As três que passam `marcos_por_dia` são **motor**, **batidas carregadas** e **juiz do colaborador**. As demais podem mudar se o par que elas efetivamente recebem passar a fechar. Para as duas noites reproduzidas, o caminho sem mapa já fechava nas condições isoladas da pergunta 1. `app/ponto/motor_calculo_v2.py:503`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1535`, `app/ponto/turnos.py:1997`, `app/ponto/turnos.py:2295`.

| Apelidos das chamadas diretas | Resultado que pode mudar |
|---|---|
| **Motor v2**, `turnos_via_autoridade` · **Batidas carregadas**, `turnos_de_batidas` · **Juiz do colaborador/lote**, `_turnos_do_colab_puro` | Períodos monetários, mapa de dia e geometria consumida pelos leitores. O mapa do motor 

[truncado em 20000 de 30444 caracteres]

## 23/11 13:49 · sessao dd8f4a95c65d · A-TELA-DO-DIA-LE-UM-JUIZ · leitura · origem=Code

A sonda registra **370 dias com “Em aberto” e outro status**. São 138 `ok`, 231 `escala_suspeita` e 1 `folga_trabalhada`. Para o colaborador 945, os três dias têm `ok` e `resolver_dia_oferecido=false`. Em 24/10 há uma `E` e `minutos_pagos=null`. Em 26 e 28/10 há `E–S–E` e, respectivamente, 238,20 e 229,60 minutos pagos no dado raspado. `sonda.out:4–7` (anexo).

A correspondência da cópia com o hash informado está **[nao verificado]**. A cópia não contém os metadados Git necessários para confirmar o `HEAD`.

## 1. Como o cartório grava `ok` e quem escreve a coluna

O caminho automático é `processar_cartorio` → `julgar_colab` → `classificar_dia` → `lavrar_veredito` → `carimbar`. O lote seleciona células de datas passadas. O cartório monta a grade com batidas apuráveis, conta marcos casados e órfãs e entrega ao classificador, entre outros insumos, `turno_encerrado`. `app/ponto/management/commands/processar_cartorio.py:49–65`, `app/ponto/services/cartorio.py:1016,1180–1184,1199–1204,1453–1455,1500–1511,1898–1922`.

- **Uma `E` e `E–S–E` podem chegar a `ok` pela mesma sequência.** Com `turno_encerrado=False`, os dois ramos de `FURO_PARCIAL` ficam fora, tanto o de realizado positivo quanto o de batida sem par fechado. Se o julgamento ficar sem códigos e sem fio que proteste, `derivar_veredito([])` devolve `concorde`. A regra do status recebe `tem_batida` como booleano. Com batida e sem a precedência de folga trabalhada, `concorde`, `coberta` e `cobranca_indevida` retornam `ok`. A regra também tem o ramo `ok` por isenção sem batida. `app/ponto/supra_juiz.py:291,392–439,450–463`, `app/ponto/services/cartorio.py:1514–1546`, `app/escala/models.py:1635–1639`, `app/ponto/portas/celula.py:1013–1020`, `app/ponto/status_dia.py:147–163,185–192`.
- **Com o turno encerrado**, os mesmos ramos podem emitir `FURO_PARCIAL` se as demais guardas passarem. O código vira veredito `furo` e, no ramo com batida, status `irregular`. A própria rotina pode preservar uma lavra anterior ao pular uma célula cuja impressão segue igual. O marcador de turno em curso entra na impressão normal apenas para `data < hoje`. O caminho avulso também admite julgar no próprio dia depois do fim previsto. O veredito, os protestos, a impressão, a via e o instante que produziram os três `ok` do 945 estão **[nao verificado]**. `app/ponto/supra_juiz.py:392–439,450–463`, `app/escala/models.py:1622–1627`, `app/ponto/status_dia.py:67–70,157–162`, `app/ponto/services/cartorio.py:1425–1444,2081–2090`, `sonda.out:5–7` (anexo).

**Escritores da coluna.** `_aplicar_status` atribui `status_dia` e `status_rotulo`. A lavra normal e o ramo `nunca_bateu` passam pela porta e persistem com `carimbar`. O D5, chamado por `lavrar_status_competencia`, completa a coluna nula com o veredito já gravado e o carimbo em dia. A decisão do admin passa pela mesma porta e mapeia `trabalhou → ok`, `faltou → falta` e `justificado → ausencia`. O cartório preserva essa precedência. A regeneração ou reversão de célula futura também pode escrever `NULL` via `desfazer_julgamento_da_futura`, preservando decisão de admin. A coluna nasce anulável. `app/ponto/portas/celula.py:267,314,877,1045–1065,1096–1134,1148,1182–1203,1217–1237`, `app/ponto/services/cartorio.py:939–956,1321–1326,1438–1441,1898–1922`, `app/ponto/management/commands/lavrar_status_competencia.py:56–57`, `app/ponto/services/veredito_celula.py:352`, `app/ponto/status_dia.py:65,98–101`, `app/escala/models.py:1827`.

## 2. Autoridades e ponto da divergência

| Pergunta | Autoridade registrada |
|---|---|
| A que turno a batida pertence e como ele foi pareado? | `parear_turnos` e `turnos_do_colab`. `app/docs/JUIZES.md:20–24`, `app/core/juizes.py:216` |
| Há turno aberto **vivo agora**? | `turno_aberto_de`. `app/docs/JUIZES.md:20`, `app/core/juizes.py:219`, `app/ponto/turnos.py:2300–2309` |
| Que marco do dia faltou? | `classificar_falta`. Para uma `E` sem `S` devolve falta de saída. Com `E` e `S`, também consulta `t.aberto` antes da contagem, quando há marcos esperados e as guardas deixam chegar ao ramo. `app/docs/JUIZES.md:132`, `app/core/juizes.py:200`, `app/escala/servico_jornada.py:401–415,448–486` |
| Qual é o status do dia? | `status_do_dia_lavrado`, cuja implementação é `status_dos_dias_lavrados`. `app/docs/JUIZES.md:133–134`, `app/core/juizes.py:207`, `app/ponto/status_dia.py:201–221` |

**O D4 consulta a geometria para saber se o dia está em curso.** Ele usa `dias_encerrados`, que consulta `turno_aberto_vivo`. Onde há janela de expiração, a aberta deixa de ser viva depois do fim previsto mais quatro horas. O D4 então pode devolver a coluna lavrada. `classificar_falta` e `datas_em_aberto_de` ficam fora desse caminho. `app/core/juizes.py:243`, `app/ponto/turnos.py:2005–2035,2113–2130,3063–3067`, `app/ponto/status_dia.py:269–295`.

**“Em aberto” usa a pendência histórica.** No calendário, a entrada é `t.aberto` da geometria, mais saídas órfãs e furos apurados pelo motor. `folha_manda` publica a união, subtraindo datas decididas e o dia do turno ainda vivo. O teto de data e a isenção se aplicam à parcela de furo apurado. Assim, uma aberta expirada pode continuar na lista enquanto o D4 já entrega `ok`. O calendário usa a pertinência na lista para a palavra e o D4 para o status. `app/colaboradores/services/calendario.py:600–616,639–640,748–763`, `app/relatorios/cartao_pela_celula.py:373–374,395–422`, `app/ponto/services/dia_decidido.py:241–246,346–355,398–430`.

## 3. `escala_suspeita` com saída órfã

O pareador cria a saída órfã com `entrada=None` e `saida` preenchida. Isso dá `t.aberto=False`, mas `datas_saida_orfa` inclui seu `data_turno` na união de “Em aberto”. Uma saída órfã sozinha, portanto, entra na lista histórica sem acionar o motivo de turno vivo no D4. A lei do furo, se chegar ao ramo de `S` sem `E`, devolve falta de **entrada**. `app/ponto/turnos.py:234–245,312–324,1495–1498,2011–2018`, `app/ponto/services/dia_decidido.py:346–355`, `app/ponto/status_dia.py:273–289`, `app/escala/servico_jornada.py:459–468`.

Na lavra, `escala_suspeita` vem do veredito `indefinida`, depois das guardas de falta, ausência e folga humana. Seu rótulo fixo começa com “Sem batida”, e o ramo pode ser alcançado com `tem_batida=True`. Já `BATIDA_ORFA_FORA_TOLERANCIA` é um código sobre **órfã da grade**. Sozinho, vira `discordante` e, no ramo com batida, `irregular`. Se coexistir com código de `indefinida`, este tem precedência no mapa sobre `discordante`. `app/ponto/status_dia.py:38–39,139–146,157–163,185–190`, `app/ponto/supra_juiz.py:527–537`, `app/escala/models.py:1622–1642`.

A linha de 31/10 do colaborador 941 tem `batidas_do_turno=[]`. O agregado de 231 conta `escala_suspeita` com “Em aberto”, sem identificar a parcela que pôs cada dia na lista. Um caso individual de `escala_suspeita` **com saída órfã** na sonda está **[nao verificado]**. `sonda.out:4,8` (anexo).

## 4. Leitores do status, do botão e de `datas_em_aberto`

| Leitor | O que usa |
|---|---|
| Calendário e espelho | O calendário lê D4 para fundo, ícone e contador. A faixa semanal do espelho também lê D4 e aplica a classe `st-ok`. O miolo do espelho monta `palavra_dia` pela lista do resumo. `app/colaboradores/services/calendario.py:639,696–697,762–763,817`, `app/templates/colaboradores/partials/_calendario_grade.html:68–74,99,152–155`, `app/ponto/services/espelho.py:170–174,1327`, `app/templates/ponto/espelho.html:113,151` |
| **Resolver dia** | Exige dia passado, anterior ao início da competência corrente, sem pergunta viva com resposta, e status em `falta`, `irregular`, `aberto` ou `sem_certidao`. `ok` e `escala_suspeita` ficam fora. O template ainda exige `pode_veredito`. A tarefa do dia reutiliza a oferta do calendário. `app/colaboradores/services/calendario.py:94–104,117–122,493–508,774,812–813`, `app/templates/colaboradores/partials/_calendario_grade.html:134–135`, `app/chamados/services/tarefa_do_dia.py:697–712` |
| Cobrança | A auditoria de espelhos seleciona só `irregular` no D4 e lê os marcos apagados na ata. O ranking “Quem cobrar” agrega `FuroDiario` e fila de chamados. A emissão efetiva segue códigos, guardas e ata no cartório, e a emissão de disputa consulta `classificar_falta`. Cobrança efetiva nos casos da sonda **[nao verificado]**. `app/relatorios/furos.py:127–145,164–168`, `app/relatorios/views.py:1643`, `app/escala/services/furos_diarios.py:178–213`, `app/ponto/services/cartorio.py:1783–1841,1861–1879`, `app/chamados/services/disputa_emissao.py:813–820,1214,1251` |
| Folha | A retenção do TXT consulta `veredito`, `protestos` e ausência de célula ou de veredito. A aprovação também tem trava por `FechamentoMensal.turnos_abertos`. O Raio-X lê marcos da ata, e o detalhamento de destrava pode herdar o D4 via tarefa e oferta de Resolver. Os minutos do calendário vêm de `DiaPago`. O motor tem uma regra própria para creditar pares fechados em dia de turno aberto. A origem dos valores exatos da sonda e seu efeito no TXT estão **[nao verificado]**. `app/folha/export.py:830–848,968–973,1265–1307`, `app/folha/views.py:637`, `app/relatorios/furos.py:190–214`, `app/folha/fora_da_folha.py:273–280`, `app/colaboradores/services/calendario.py:319,688–689,781–783`, `app/ponto/motor_calculo_v2.py:2511–2564` |
| App e Painel Operacional | A API publica o D4 em `status_dia`, traduz para `status` e leva a palavra do espelho. O painel lê D4 para os dias encerrados que selecionou. `app/api/views.py:1361,1387–1395,1496,1548,1561–1563,1621–1626`, `app/ponto/status_dia.py:79–91`, `app/core/services/painel_op.py:329–338` |

- **Censo AST de `status_dia == 'ok'`.** Zero comparações literais ou filtros ORM de execução contra `ok` na coluna. O leitor funcional da coluna é o D4. O leitor Python que compara a projeção do D4 com `ok` é o contador do calendário, `d['status'] == 'ok'`. A porta, o D5 e a fotografia E4B também leem a coluna para seus próprios fins. `app/ponto/status_dia.py:287–289`, `app/colaboradores/services/calendario.py:696–697,762,817`, `app/ponto/portas/celula.py:1056–1065,1082`, `app/ponto/services/cartorio.py:946`, `app/chamados/services/foto_e4b.py:247–252,269`.
- **Censo AST de `datas_em_aberto`.** Os leitores de execução são o calendário, por parâmetro ou pelo resumo, o miolo do espelho, o PDF, o passivo da Pauta DP e o comando `diff_pdf_x_espelho`, que lê a chave dinamicamente e compara comprimentos. A API herda a palavra do espelho. PDF, placar da folha e comando cartão × TXT também leem a contagem derivada `dias_em_aberto`. `app/colaboradores/services/calendario.py:595–616,749`, `app/ponto/services/espelho.py:330–345,1327`, `app/relatorios/pdf_espelho.py:638,791–792`, `app/ponto/services/dias_em_aberto.py:112`, `app/pautas/management/commands/pauta_dia_em_aberto.py:82`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:36,93–102`, `app/api/views.py:1361,1387–1395`, `app/folha/porta_export.py:621`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:64`.

## 5. `Falta (desconta Nh)` e minutos do dia

| Leitura | Unidade e consumidores |
|---|---|
| **Rótulo de falta e cartão diário** | `Ausencia.minutos` é a duração **do registro**. Para falta aprovada com zero minutos, `save` soma o previsto de `data` a `data_fim`. `rotulo_de` passa o total cru para `palavra_do_dia`, que imprime `Falta (desconta Nh)`. `cartao_do_dia` também expõe o total cru, e `decididos_do_periodo` monta esse cartão para cada data da faixa. Assim, o rótulo diário pode repetir o total da faixa no calendário, espelho, PDF e app. `app/ponto/models.py:750–759,887–897`, `app/ponto/services/dia_decidido.py:156–169,249–256,473–478,533–546,561–569,605–611`, `app/colaboradores/services/calendario.py:748–757`, `app/ponto/services/espelho.py:329–346,1327`, `app/relatorios/pdf_espelho.py:638,923–924`, `app/api/views.py:1387–1400` |
| **Leituras diretas com guarda de um dia** | `cobre_parte_do_dia` exige tipo habilitado, uma única data e `0 < minutos < 480`. `creditos_por_dia` soma por `a.data` para abater atraso no próprio dia. `efeitos_da_ausencia` soma essas parciais no período para o PDF. A lista e o painel de ausências exibem os minutos sob a guarda de parcialidade. `app/ponto/turnos.py:2599–2619`, `app/ponto/services/credito_parcial.py:24–28`, `app/ponto/services/fechamento.py:994–1001`, `app/ponto/services/dia_pago.py:188–195`, `app/ponto/services/efeito_ausencia.py:46–50,63–65`, `app/relatorios/pdf_espelho.py:332–336,479`, `app/ponto/views.py:2112,2340–2344`, `app/templates/ponto/ausencias.html:159–165`, `app/templates/ponto/partials/painel_ausencia.html:310–327` |
| **Rateio de falta por dia** | `minutos_de_falta_por_dia` seleciona ausências aprovadas que descontam, divide o total pela quantidade de datas da expansão **inteira** do registro e soma as parcelas por data da janela. A única chamada direta de execução encontrada é no recálculo do fechamento. Alimenta `FechamentoMensal.horas_falta`, `DiaPago.horas_falta` e `detalhe['falta_min']`. A rubrica 8792 do TXT usa `datas_falta` e monta o bloco de datas. `app/folha/export.py:87–92,113–143,740–744`, `app/ponto/services/fechamento.py:873–875,1003,1200–1206,1277–1282`, `app/ponto/services/dia_pago.py:274–277` |

A ocorrência em produção de uma falta de várias datas com o rótulo diário divergente do rateio está **[nao verificado]**. `sonda.out:3–12` (anexo).

## 23/11 13:48 · sessao 492e33b59d8e · - · - · origem=Ronald (tmux)

- A porta que grava `CelulaDia.status_dia` é `lavrar_veredito`, via `_aplicar_status`. Entram por ela o julgamento normal do cartório, o ramo sem batida na vida, o complemento D5 e a decisão humana. O desfazimento de julgamento futuro também grava `NULL` pelo `setattr` da mesma porta. A varredura em memória do selo AST percorreu 828 fontes de produção sem encontrar escritor fora da porta. `app/ponto/portas/celula.py:1045–1065`, `:1096–1134`, `:1148–1149`, `:1182–1203`, `app/ponto/services/cartorio.py:939–956`, `:1321–1326`, `:1898–1922`, `app/ponto/services/veredito_celula.py:352`, `app/ponto/tests/test_selo_veredito_so_pela_porta.py:171–194`.

- No fluxo do cartório, lista de códigos vazia produz `concorde`. Havendo batida, `concorde`, `coberta` e `cobranca_indevida` podem produzir `ok`. O status recebe presença como booleano, sem a quantidade de marcas faltantes. `app/ponto/services/cartorio.py:1500–1547`, `app/escala/models.py:1635–1642`, `app/ponto/status_dia.py:157–163`, `:185–192`. Com as demais guardas satisfeitas, o classificador atual acusa `FURO_PARCIAL` tanto para E sem par e realizado zero quanto para E-S-E com realizado positivo. Ambos os ramos dependem de `dia_encerrado` e de `not sem_ancora`. O ramo isento devolve lista vazia. `app/ponto/supra_juiz.py:229–250`, `:392–463`. Qual guarda, decisão humana ou fotografia da lavra escreveu o `ok` do 945 é [não verificado].

- O próprio código permite lavrar enquanto o turno de um dia anterior ainda vive. Nesse caso o término muda `enc=0` para ausente e habilita rejulgamento. Há outra trajetória no avulso do próprio dia, após o fim previsto mas dentro da margem da abertura. Nela `enc=0` fica ausente por `data < hoje`, assim como na próxima avaliação já encerrada. Com o restante da impressão igual, a avaliação seguinte pula o dia. `app/ponto/services/cartorio.py:528–529`, `:1424–1444`, `:1500–1511`, `:2073–2090`, `app/ponto/tests/test_celula_concorde_meio_turno.py:44–67`. Ocorrência dessa trajetória no dado fornecido [não verificado].

- A autoridade registrada de furo é `classificar_falta`, a de presença viva é `turno_aberto_de`, e a de status é `status_do_dia_lavrado`. D4 consulta o juiz de encerramento, que usa `turno_aberto_vivo` e expira a abertura depois do fim da janela do template mais quatro horas, então lê a coluna. `datas_em_aberto_de` usa abertura geométrica, saída órfã e furo do motor, com suas respectivas exclusões. A abertura geométrica expirada pode permanecer nessa lista. `app/docs/JUIZES.md:20–24`, `:132–134`, `app/core/juizes.py:200–219`, `:243`, `app/ponto/status_dia.py:269–290`, `app/ponto/turnos.py:2005–2035`, `:2113–2130`, `app/colaboradores/services/calendario.py:600–616`, `:748–763`, `app/relatorios/cartao_pela_celula.py:373–422`, `app/ponto/services/dia_decidido.py:346–355`.

- A S órfã forma turno com `entrada=None`, `saida` preenchida e `aberto=False`. Pode entrar na palavra por `datas_saida_orfa` sem bloquear a leitura da coluna no D4. `escala_suspeita` deriva de `indefinida` no ramo que passou pelas precedências. A órfã da grade, em jornada prevista de trabalho com zero realizado, pode gerar `BATIDA_ORFA_FORA_TOLERANCIA`, que isolado leva a `discordante` e `irregular`. Se coexistir código de `indefinida`, este tem prioridade sobre `discordante` no mapa. `app/ponto/turnos.py:234–245`, `:312–324`, `:1495–1498`, `app/ponto/status_dia.py:139–146`, `:159–160`, `app/ponto/supra_juiz.py:527–537`, `app/escala/models.py:1622–1642`. A sonda não mostra uma S órfã no exemplo 941 de 31/10, nem discrimina por S órfã os 231 casos [não verificado].

## 23/11 13:47 · sessao 5b8da13f9df7 · - · - · origem=Ronald (tmux)

**Leitores do D4.** O único leitor funcional direto da coluna para classificação é `status_dos_dias_lavrados`, que devolve `cel.status_dia` quando o relógio deixa ler a lavra. A porta e a retrolavra também leem a coluna para comparar, preservar ou completar o registro. `app/ponto/status_dia.py:272–295`, `app/ponto/portas/celula.py:1056–1065,1082`, `app/ponto/services/cartorio.py:946`.

- **Calendário.** Lê o D4 e publica `status`/`status_rotulo`. A palavra vem, em paralelo, da pertinência em `_em_aberto` passada a `do_dia`. O template usa a palavra como título da célula e o status para fundo e ícone. `app/colaboradores/services/calendario.py:639,696–697,748–763`, `app/templates/colaboradores/partials/_calendario_grade.html:68–74,97–104`.
- **Espelho e app.** A faixa semanal do espelho lê status/rotulo do D4, e o miolo aplica a palavra pelo resumo. O app lê esse miolo e consulta o D4 separadamente, publicando os dois sinais e ainda `turno_aberto` por geometria. O adaptador do app mantém `ok` e traduz `escala_suspeita` como `neutro`. O Painel Operacional lê D4 nos dias encerrados. `app/ponto/views.py:162–164,203`, `app/ponto/services/espelho.py:170–174,1327`, `app/templates/ponto/espelho.html:151–153`, `app/api/views.py:1361,1387–1395,1496,1512–1516,1547–1548,1560–1563,1610–1626`, `app/ponto/status_dia.py:79–91`, `app/core/services/painel_op.py:329–338`.
- **Resolver dia.** O gate da oferta exige dia passado, anterior ao início da competência corrente, sem pergunta viva com resposta, e status em `falta`, `irregular`, `aberto` ou `sem_certidao`. `ok` e `escala_suspeita` ficam fora. O template exige também `pode_veredito`, oriundo da permissão `regularizar_batida`. A tarefa pergunta pela mesma oferta nos dias elegíveis sem rota. O POST tem seus próprios controles de permissão, datas e veredito e delega à porta de ação. `app/colaboradores/services/calendario.py:94–104,117–122,493–508,774,812–813,157–161`, `app/colaboradores/views.py:1990–2000`, `app/templates/colaboradores/partials/_calendario_grade.html:134–135`, `app/chamados/services/tarefa_do_dia.py:697–712`, `app/ponto/views.py:3491–3520`.
- **Cobrança.** A auditoria de espelhos seleciona só `irregular` no D4, lê horários apagados da ata e passa `NAO_PERGUNTADO` ao calendário. O ranking «Quem cobrar» agrega `FuroDiario` e a fila de chamados. A emissão efetiva segue códigos do julgamento, guardas e marcos apagados no cartório, e o agrupador de órfãos consulta `classificar_falta` e a ata. Ocorrência efetiva de cobrança nos registros da sonda [nao verificado]. `app/relatorios/furos.py:127–145,164–168`, `app/relatorios/views.py:1643`, `app/escala/services/furos_diarios.py:25–57,178–186,193–213`, `app/ponto/services/cartorio.py:1783–1796,1813–1841,1861–1879`, `app/chamados/services/disputa_emissao.py:813–820,1214–1233,1251–1264`.
- **Folha.** A retenção do TXT usa `CelulaDia.veredito`, protestos e guardas de célula/veredito ausente. O Raio‑X lê os marcos apagados da ata. O detalhamento de destrava lê a tarefa e pode herdar indiretamente a oferta de Resolver produzida pelo calendário. `app/folha/export.py:968–973,1205–1213,1265–1307`, `app/folha/views.py:637`, `app/relatorios/furos.py:190–214`, `app/folha/fora_da_folha.py:273–280`, `app/chamados/services/tarefa_do_dia.py:697–712`.

**Censo AST.** Nas 3.752 árvores Python parseadas em memória, não apareceu comparação literal `status_dia == 'ok'` ou invertida. O consumidor Python do predicado equivalente sobre a projeção D4 é o contador `d['status'] == 'ok'`. No template do calendário, ele pinta o fundo e o check, e aparece também num laço sem saída renderizada antes da leitura do contador. `app/colaboradores/services/calendario.py:696–697,762,817`, `app/templates/colaboradores/partials/_calendario_grade.html:69,99,152–155`.

- **Acessos de execução a `datas_em_aberto`.** Calendário recebe a lista por parâmetro ou lê o resumo e testa pertinência. Espelho transforma a lista em conjunto e a passa à palavra. PDF reaplica com a lista do seu resumo. Pauta DP consome a lista no diagnóstico. O comando de diff acessa a chave indiretamente e compara comprimentos. `app/colaboradores/services/calendario.py:595–597,616,749`, `app/ponto/services/espelho.py:330,341,1327`, `app/relatorios/pdf_espelho.py:638`, `app/ponto/services/dias_em_aberto.py:112`, `app/pautas/management/commands/pauta_dia_em_aberto.py:82`, `app/relatorios/management/commands/diff_pdf_x_espelho.py:36,94–101`.
- `folha_manda` grava a lista efetiva e a contagem. O PDF tem placeholder anterior e lê a palavra na coluna Obs. Auditoria de furos passa uma sentinela para pular a lista. A contagem derivada é lida no PDF, placar e comando cartão × TXT. `app/relatorios/cartao_pela_celula.py:419–422`, `app/relatorios/pdf_espelho.py:468,791–792,923–924`, `app/relatorios/furos.py:168`, `app/folha/porta_export.py:621`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:64`.

**Saída órfã.** `datas_saida_orfa` coleta a `data_turno` quando há saída sem entrada no par. Calendário, espelho e PDF publicam essa parcela, unida com turno aberto e furo apurado, retirando datas decididas e o turno em curso. O teste estático usa `S–E–S` noturno ancorado na véspera e exige `em_aberto` e parcial positivo, sem afirmar `escala_suspeita`. A sonda mostra o agregado de 231 e um exemplo de `escala_suspeita` com `batidas_do_turno=[]`. Caso individual de `escala_suspeita` com saída órfã na sonda [nao verificado]. `app/ponto/turnos.py:234–245`, `app/colaboradores/services/calendario.py:607–613`, `app/ponto/services/espelho.py:630–631`, `app/relatorios/pdf_espelho.py:619–622`, `app/ponto/services/dia_decidido.py:346–355`, `app/ponto/tests/test_saida_orfa_dia_em_aberto.py:91–104`, `sonda.out:4,8` (anexo).

## 23/11 13:46 · sessao dc35802cd5c9 · - · - · origem=Ronald (tmux)

**5. Rótulo de falta e minutos por dia**

- `Ausencia.minutos` é a duração do registro. Ao salvar falta aprovada com zero minutos, `Ausencia.save` soma os previstos entre `data` e `data_fim`. `rotulo_de` entrega esse total a `palavra_do_dia`, que formata `Falta (desconta Nh)`. A única supressão de minutos em `rotulo_de` é para abono que cobre dia inteiro. Um registro descontado que cobre várias datas pode, portanto, exibir seu total em cada linha de dia. `app/ponto/models.py:750-759,887-897`; `app/ponto/services/dia_decidido.py:156-169,249-256,398-399,473-476,533-546`; `app/ponto/turnos.py:2752-2778`.
- `cartao_do_dia` também expõe `minutos` crus e `decididos_do_periodo` põe esse cartão em cada data da faixa. Para o rótulo diário, os leitores são calendário, espelho, PDF e API do app. `app/ponto/services/dia_decidido.py:561-569,605-611`; `app/colaboradores/services/calendario.py:748-757`; `app/templates/colaboradores/partials/_calendario_grade.html:97-98`; `app/ponto/services/espelho.py:329-346,1327`; `app/relatorios/pdf_espelho.py:636-638,923-924`; `app/api/views.py:1387-1400`.
- Outras leituras de minutos como duração **do próprio dia** têm a guarda de ausência parcial, que exige tipo habilitado, uma única data e `0 < minutos < 480`. `cobre_parte_do_dia` lê o campo para essa classificação. `creditos_por_dia` soma o valor por `a.data` para abater o atraso daquele dia, `efeitos_da_ausencia` soma o valor das parciais do período para o PDF, e lista e painel mostram os minutos quando a guarda permite. `app/ponto/turnos.py:2599,2602,2617-2619`; `app/ponto/services/credito_parcial.py:24-28`; `app/ponto/services/fechamento.py:994-1001`; `app/ponto/services/dia_pago.py:188-195`; `app/ponto/services/efeito_ausencia.py:46-50,63-65`; `app/relatorios/pdf_espelho.py:332-336,479`; `app/ponto/views.py:2112,2340-2344`; `app/templates/ponto/ausencias.html:159-165`; `app/templates/ponto/partials/painel_ausencia.html:310-313,323-327`.
- `minutos_de_falta_por_dia` seleciona ausências aprovadas que descontam, divide os minutos do registro pelo número de datas de sua expansão integral e soma os valores por data da janela. A chamada direta de produção está em `recalcular_fechamento_mes`, que usa o total para `FechamentoMensal.horas_falta` e passa o mapa para `DiaPago.horas_falta` e `detalhe['falta_min']`. O resumo do cartão lê o fechamento ou a soma das diárias. A rubrica 8792 do TXT usa `datas_falta` e emite contagem e datas. `app/folha/export.py:87-92,113-122,125-143`; `app/ponto/services/fechamento.py:873-875,1003,1200-1206,1277-1282`; `app/ponto/services/dia_pago.py:274-277`; `app/relatorios/cartao_pela_celula.py:115,143-146,183-185`; `app/folha/export.py:215-221,740-744`.
- Leituras por registro incluem serialização da API, admin, detalhe e edição, auditoria, duração em horas, fotografia do insumo da folha, histórico, CSV de atestados e espelhamento da tabela. O ramo de pendência lê o campo, mas o rótulo pendente não o usa. `app/api/views.py:1882`; `app/ponto/admin.py:44`; `app/templates/ponto/partials/painel_ausencia.html:125,163`; `app/ponto/views.py:2476,2910,2950,2993`; `app/ponto/services/ausencia.py:119,126-139,590`; `app/ponto/models.py:874-876`; `app/ponto/services/dia_decidido.py:259-264,465-469,648,665-675`; `app/ponto/services/atestados_lote.py:54-58,118-128`; `bin/sombra_incremental.sh:36`. Ocorrência em produção de falta de várias datas com o rótulo divergente do valor por dia [nao verificado].

## 23/11 13:37 · sessao c751a3c4fda1 · A-TELA-DO-DIA-LE-UM-JUIZ · sonda · origem=Code

**Script A — sombra.** O anexo registrou 14 dias com pago ≥ previsto e 111 dias com pago e minutos dos pares. O histograma usa a diferença com sinal, arredondada a nove casas, e inclui 10, 60 e 240 na faixa que termina nesse valor. O pago soma `horas_trabalhadas`, `horas_folga_trabalhada` e `detalhe.folga_feriado_min`, após a checagem das rubricas de sustentação. `sonda.out:4`, `sonda_pago_pares_sombra.py:130`, [app/ponto/services/dia_pago.py:861](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/services/dia_pago.py:861).

```python
# sonda_pago_pares_sombra.py
import json
import logging
from bisect import bisect_left
from collections import Counter
from unittest.mock import patch

from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from colaboradores.services.calendario import (
    STATUS_QUE_PEDEM_DECISAO,
    contexto_calendario,
)
from escala.utils import minutos_previstos_do_dia
from ponto.services.dia_pago import _RUBRICAS_DE_DINHEIRO, pago_do_dia
from ponto.turnos import CargaDaJanela, realizado_do_dia

MES, ANO = 11, 2026
LIMITES = (0, 10, 60, 240)
FAIXAS = ("<=0", "0-10", "10-60", "60-240", ">240")


def falhar_em_log_de_erro(_logger, registro):
    if registro.exc_info and registro.exc_info[1] is not None:
        _, erro, traceback = registro.exc_info
        raise erro.with_traceback(traceback)
    if registro.levelno >= logging.ERROR:
        raise RuntimeError(f"Erro registrado em {registro.pathname}:{registro.lineno}")


def ler_grade(colab, agora):
    ctx = contexto_calendario(
        colab, {"modo": "competencia", "mes": str(MES), "ano": str(ANO)},
        agora=agora,
    )
    for dia in ctx["dias"]:
        if not dia["em_aberto_perguntado"] or dia["resolver_dia_oferecido"] is None:
            raise RuntimeError(f"Grade sem resposta para pk={colab.pk}, dia={dia['data']}")
    return ctx


def fonte_do_pago(linha):
    return {
        "dia_pago_pk": linha.pk,
        "versao": linha.versao,
        "evento_origem": linha.evento_origem,
        "parcelas_somadas": [
            "horas_trabalhadas", "horas_folga_trabalhada", "detalhe.folga_feriado_min",
        ],
        "rubricas_horas": {campo: getattr(linha, campo) for campo in _RUBRICAS_DE_DINHEIRO},
        "detalhe": {"folga_feriado_min": (linha.detalhe or {}).get("folga_feriado_min")},
    }


def imprimir(obj):
    print(json.dumps(obj, ensure_ascii=False), flush=True)


def sondar():
    if connection.settings_dict["NAME"] != "sombra":
        raise RuntimeError("Use o banco de ensaio sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transação própria.")

    with (
        patch.object(logging.Logger, "handle", falhar_em_log_de_erro),
        schema_context("juliani"),
        transaction.atomic(),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco/schema diferente de sombra/juliani.")

        agora = timezone.now()
        qs = Colaborador.objects.select_related("empresa", "posto__praca").order_by("pk")
        colabs = list(qs.filter(situacao="ativo"))
        comparacao = Counter()
        histograma = dict.fromkeys(FAIXAS, 0)
        diferencas = []
        for colab in colabs:
            ctx = ler_grade(colab, agora)
            turnos = None
            for dia in ctx["dias"]:
                status = dia["status"]
                texto = dia["palavra_dia"]
                if "Em aberto" not in texto or status in STATUS_QUE_PEDEM_DECISAO:
                    continue

                d = dia["data"]
                previsto = minutos_previstos_do_dia(colab, d)
                linha = dia["dia_pago"]
                horas = pago_do_dia(linha)
                if horas is None:
                    comparacao["dias_sem_minutos_pagos_sustentados"] += 1
                    continue
                pago = round(horas * 60, 9)
                ge = pago >= previsto
                comparacao["dias_pago_ge_previsto" if ge else "dias_pago_lt_previsto"] += 1

                if turnos is None:
                    turnos = CargaDaJanela([colab], ctx["data_ini"], ctx["data_fim"]).turnos(colab)
                pares = realizado_do_dia(colab, d, turnos=turnos, previsto_min=previsto).minutos
                chave_dia = {
                    "colaborador_pk": colab.pk,
                    "data": d.isoformat(),
                    "status": status,
                }
                if ge:
                    imprimir({
                        **chave_dia,
                        "texto": texto,
                        "batidas_do_turno": [
                            [timezone.localtime(b.timestamp).isoformat(), b.tipo]
                            for b in dia["batidas"]
                        ],
                        "minutos_pagos": pago,
                        "minutos_previstos": previsto,
                        "minutos_dos_pares": pares,
                        "fonte_pago": fonte_do_pago(linha),
                    })

                if pares is None:
                    comparacao["dias_com_pago_sem_minutos_dos_pares"] += 1
                    continue
                diferenca = round(pago - pares, 9)
                histograma[FAIXAS[bisect_left(LIMITES, diferenca)]] += 1
                diferencas.append({**chave_dia, "diferenca_minutos": diferenca})

    diferencas.sort(key=lambda item: (-item["diferenca_minutos"], item["colaborador_pk"], item["data"]))
    imprimir({
        "colaboradores_no_universo": len(colabs),
        "dias_pago_ge_previsto": comparacao["dias_pago_ge_previsto"],
        "dias_pago_lt_previsto": comparacao["dias_pago_lt_previsto"],
        "dias_sem_minutos_pagos_sustentados": comparacao["dias_sem_minutos_pagos_sustentados"],
        "dias_com_pago_sem_minutos_dos_pares": comparacao["dias_com_pago_sem_minutos_dos_pares"],
        "dias_com_pago_e_minutos_dos_pares": len(diferencas),
        "histograma_pago_menos_pares": histograma,
        "maiores_diferencas": diferencas[:10],
    })


sondar()
```

**Script B — produção.** Para `para="folha"`, o campo decisivo é `Ausencia.status`, com valor `aprovada`. Entre as aprovadas que cobrem a data, vence a de maior `data` e, no empate, de maior `pk`. Uma ausência anterior a novembro com `data_fim=NULL` só alcança novembro se o tipo tiver fim indeterminado no catálogo vivo. A sonda imprime essas linhas candidatas com `toca_novembro="[nao verificado]"`. [app/ponto/turnos.py:2528](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/turnos.py:2528), [app/ponto/turnos.py:2786](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/turnos.py:2786), [app/ponto/catalogo/ausencias.py:430](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/catalogo/ausencias.py:430).

Os rótulos de ausência dependem do catálogo vivo de `TipoAusencia`. `rotulo_de` consulta o efeito e, quando aplicável, a regra de parcialidade. `palavra_do_dia` recebe um veredito e pode consultar o nome do tipo. A dependência é do catálogo. Com as duas tabelas pedidas, a sonda emite `[nao verificado]` para os rótulos quando há linhas candidatas. `null` indica que a consulta encontrou zero candidatas naquele dia. [app/ponto/services/dia_decidido.py:217](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/services/dia_decidido.py:217), [app/ponto/services/dia_decidido.py:502](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/services/dia_decidido.py:502), [app/ponto/turnos.py:2602](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/turnos.py:2602), [app/ponto/catalogo/ausencias.py:240](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/ponto/catalogo/ausencias.py:240), `sonda_ausencias_941_prod.py:47`.

```python
# sonda_ausencias_941_prod.py
import json
import logging
from datetime import date
from unittest.mock import patch

from django.db import connection, transaction
from django.db.models import F, Q
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from ponto.models import Ausencia

INICIO, FIM = date(2026, 11, 1), date(2026, 11, 30)
DIAS = tuple(date(2026, 11, dia) for dia in range(16, 20))
NAO_VERIFICADO = "[nao verificado]"


def falhar_em_log_de_erro(_logger, registro):
    if registro.exc_info and registro.exc_info[1] is not None:
        _, erro, traceback = registro.exc_info
        raise erro.with_traceback(traceback)
    if registro.levelno >= logging.ERROR:
        raise RuntimeError(f"Erro registrado em {registro.pathname}:{registro.lineno}")


def iso(valor):
    return valor.isoformat() if valor is not None else None


def imprimir(obj):
    print(json.dumps(obj, ensure_ascii=False), flush=True)


def ler_dia(dia, ausencias):
    candidatas = [
        a for a in ausencias
        if a.data <= dia and (a.data_fim is None or dia <= a.data_fim)
    ]
    confirmadas = [a.pk for a in candidatas if a.data_fim is not None or a.data == dia]
    sem_fim_a_confirmar = [a.pk for a in candidatas if a.data_fim is None and a.data < dia]
    aprovadas = [a for a in candidatas if a.status == "aprovada"]
    cobertura_folha_pk = None
    if aprovadas:
        ultima = aprovadas[-1]
        cobertura_folha_pk = ultima.pk if ultima.pk in confirmadas else NAO_VERIFICADO
    rotulo = NAO_VERIFICADO if candidatas else None
    return {
        "data": dia.isoformat(),
        "ausencias_pks_confirmadas": confirmadas,
        "ausencias_pks_sem_fim_a_confirmar": sem_fim_a_confirmar,
        "ausencia_cobertura_folha_pk": cobertura_folha_pk,
        "rotulo_de": rotulo,
        "palavra_do_dia": rotulo,
    }


def sondar():
    if connection.settings_dict["NAME"] != "saas_hasner":
        raise RuntimeError("Use o banco de produção saas_hasner.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transação própria.")

    with (
        patch.object(logging.Logger, "handle", falhar_em_log_de_erro),
        schema_context("juliani"),
        transaction.atomic(),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("saas_hasner", "juliani"):
                raise RuntimeError("Banco/schema diferente de saas_hasner/juliani.")

        pk = Colaborador.objects.values_list("pk", flat=True).get(pk=941)
        ausencias = list(
            Ausencia.objects.filter(colaborador_id=pk, data__lte=FIM)
            .filter(
                Q(data_fim__isnull=True)
                | Q(data_fim__gte=INICIO, data__lte=F("data_fim"))
            )
            .only("tipo", "status", "data", "data_fim", "minutos", "criado_em", "atualizado_em")
            .order_by("data", "pk")
        )
        for a in ausencias:
            imprimir({
                "colaborador_pk": pk,
                "pk": a.pk,
                "tipo": a.tipo,
                "status": a.status,
                "data": iso(a.data),
                "data_fim": iso(a.data_fim),
                "minutos": a.minutos,
                "criado_em": iso(a.criado_em),
                "atualizado_em": iso(a.atualizado_em),
                "toca_novembro": (
                    True if a.data_fim is not None or a.data >= INICIO else NAO_VERIFICADO
                ),
            })
        for dia in DIAS:
            imprimir({"colaborador_pk": pk, **ler_dia(dia, ausencias)})


sondar()
```

Execução das duas novas sondas em banco e registros de `Ausencia` do 941 em produção [nao verificado].

## 23/11 13:25 · sessao 0ec30af5c5d8 · PAR-PELO-TURNO · leitura · origem=Code

lei: falta o corte que diz **qual vão `S→E` separa duas jornadas reais** para o PAR-PELO-TURNO. Os números existentes respondem a perguntas diferentes. **11h** é descanso entre jornadas citado no repo, **2h** classifica os blocos do turno previsto, **3h** é o teto padrão para inferir pausa, **6h** pode fazer o motor atribuir dois períodos ao mesmo dia e **previsto + 4h / 26h** limita o fechamento `E→S`. Qual desses critérios vale para juntar os blocos do aval fica **[nao verificado]**. `aval.txt:1` (anexo colado), `app/docs/mapa_seletor_cobranca.md:27`, `app/escala/servico_jornada.py:130`, `app/ponto/turnos.py:382`, `app/ponto/motor_calculo_v2.py:2400`, `app/ponto/turnos.py:1430`.

Leitura da cópia no diretório informado. A correspondência com o SHA está **[nao verificado]** porque a cópia não tem metadados Git.

## 1. Onde o par é decidido e por que sobram as marcações

`parear_turnos` ordena por instante e usa o papel da ata quando disponível, senão o tipo gravado. Pausas e ecos reconhecidos são tratados antes das bordas. Uma `E` de borda abre um turno e deixa uma abertura anterior sem saída. Uma `S` de borda fecha a abertura corrente, sujeita às guardas de duração e pausa. Sem abertura corrente, a `S` sai como `Turno` sem entrada. Um vão de **até 60 segundos** também deixa a `S` órfã, mantendo a abertura corrente disponível. `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1304`, `app/ponto/turnos.py:1377`, `app/ponto/turnos.py:1398`, `app/ponto/turnos.py:1482`, `app/ponto/turnos.py:117`, `app/ponto/management/commands/detectar_par_relampago.py:49`.

- **Teto da `S` que fecha.** Busca `hi/hf` em `marcos_por_dia` pela **data local da entrada**. Com duas bordas distintas, o teto é `(hf − hi) módulo 24h + 4h`, usando o vão previsto bruto. Com borda ausente ou coincidente, usa **26h**. Se **ambas** as pontas distam estritamente mais de **180 min** de seus respectivos marcos, a L-084 volta a usar **26h**. O ramo separa se o vão real for **maior** que o teto escolhido. As duas passagens finais ainda podem recuperar uma saída órfã antes da próxima entrada, inclusive pela exceção posicional, ou uma ida à pausa sem volta com fato encerrado. Ambas conferem o teto do dia, com `cont_max_s` de fallback. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:326`, `app/ponto/turnos.py:1430`, `app/ponto/motor_calculo_v2.py:1931`, `app/docs/LEIS.md:116`, `app/ponto/turnos.py:1029`, `app/ponto/turnos.py:1064`, `app/ponto/turnos.py:1165`.
- **Janela prevista e órfã da grade.** `janela_turno_de` monta os instantes previstos a partir de `hi/hf`, com a virada de data. A grade, no caminho sem alinhamento por papel, busca candidatas entre o primeiro marco **−90 min** e o último **+90 min**, e conserva sobras na lista `orfas`. Na reconciliação, só um par fechado pode recuperar suas bordas, com condições de marco faltante, presença na lista de órfãs, proximidade e L-084. Assim, uma órfã da grade pode existir mesmo quando o pareador fechou outro par no dia. `app/ponto/selecao_periodo.py:132`, `app/escala/utils.py:595`, `app/escala/utils.py:635`, `app/escala/utils.py:1288`, `app/escala/utils.py:1824`, `app/escala/utils.py:1955`, `app/escala/utils.py:2001`.
- **`pares_separados_pelo_teto` é leitor do resultado.** O ramo de fechamento carimba a `S` se o teto do dia foi decisivo e o vão era **≤26h**. O leitor exige que a `S` continue sem entrada e que a `E` carimbada continue aberta no resultado final. Compara por PK quando disponível. A lista Cadastro × Realidade consome esse resultado por `teto_na_lista`. `app/ponto/turnos.py:165`, `app/ponto/turnos.py:183`, `app/ponto/turnos.py:1447`, `app/ponto/services/teto_na_lista.py:47`.

| Caso do aval | O que a cópia permite concluir |
|---|---|
| **col129, 17:30 → 06:30** | A escala 21:30–05:42 tem vão de **492 min** e teto de **732 min**. O par tem **780 min**. As distâncias são **240 e 48 min**, então a L-084 não devolve o teto a 26h. Resultado do ramo: `E` aberta e `S` órfã, com carimbo. O teste da cópia fixa exatamente esses horários e esse resultado. `aval.txt:1`, `app/ponto/turnos.py:78`, `app/ponto/turnos.py:1440`, `app/ponto/tests/test_col129_par_separado_pelo_teto.py:52` |
| **col155, 17:29 → 06:32** | Com a mesma escala alimentada, são **783 min** contra **732 min**, com distâncias de **241 e 50 min**. A execução isolada do corpo do pareador com essas duas marcas deu a mesma separação. Os demais insumos do caso real ficam **[nao verificado]**. `aval.txt:1`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:1447` |
| **col214, 07:00** | O aval confirma uma órfã. Ele não dá tipo, marca complementar, bordas previstas ou papel da ata. O ramo exato fica **[nao verificado]**. Uma `S` sem abertura e uma marca não atribuída pela grade são caminhos distintos que produzem sobras. `aval.txt:1`, `app/ponto/turnos.py:1495`, `app/escala/utils.py:1824` |
| **col107, 07:05 e 09:15 antes de 10:13–14:25** | As marcas antecedem o início informado em **188 e 58 min**. O ramo E/S não anexa pelo simples fato de estarem antes de um turno já fechado, e a reconciliação da grade recupera as próprias bordas do par. A marca das 09:15 estar a 58 min do início real não prova que estava a 58 min de um **marco previsto**. Tipos, DNA e ramo efetivo ficam **[nao verificado]**. `aval.txt:1`, `app/ponto/turnos.py:1417`, `app/ponto/turnos.py:1491`, `app/escala/utils.py:635`, `app/escala/utils.py:1985`, `app/escala/utils.py:2010` |

## 2. Limites escritos e quem os lê

| Candidata | Número e leitura atual |
|---|---|
| **Interjornada, art. 66** | **11h** no mapa de cobrança e no corte de **permuta** registrado em `PROMPTS.md`. O helper de cobertura cita expressamente que a validação de 11h está fora dele. Leitura pelo juiz do par ou por juiz vivo de interjornada **[nao verificado]**. `app/docs/mapa_seletor_cobranca.md:27`, `app/docs/PROMPTS.md:2015`, `app/ponto/utils.py:141` |
| **Intervalo previsto / turno partido, art. 71** | Vão declarado **≤120 min** é contínuo e **>120 min** é partido. O juiz `eh_turno_partido` usa `hii/hfi` circulares do desenho. Tem corte assinado e leitores de cadastro e rótulo de escala. O predicado classifica **dois blocos do mesmo turno previsto**. `app/escala/servico_jornada.py:114`, `app/escala/servico_jornada.py:130`, `app/escala/servico_jornada.py:193`, `app/escala/services/cadastro_tipo.py:113`, `app/core/juizes.py:228`, `app/docs/CORTES.md:359` |
| **Vão real `S→E` como pausa** | **3h** no pareador e no `MotorBase`, **8h** no `MotorPlantao`, **0** no `MotorTurnoPartido`, que desliga a inferência por duração. Há guardas de distância dos marcos e de continuidade. Com `hii` e `hfi` acesos, a ata pode declarar a pausa acima do teto de inferência. Para descontar como pausa gozada, `intervalos_do_turno` exige **15 min** ou mais. `app/ponto/turnos.py:382`, `app/ponto/turnos.py:1402`, `app/ponto/turnos.py:1466`, `app/ponto/turnos.py:1816`, `app/ponto/turnos.py:537`, `app/ponto/motor_calculo_v2.py:728`, `app/ponto/motor_calculo_v2.py:2859`, `app/ponto/motor_calculo_v2.py:3511` |
| **Duração total `E→S`** | **Previsto bruto + 4h** ou fallback de **26h** no fechamento principal, com a exceção L-084 de **180 min nas duas pontas**. `cont_max_s` vale **14h** por padrão e **26h** no plantão para guardas de absorção e fallback dos pós-fechos. A vigência do turno aberto tem outro relógio, **fim previsto + 4h**. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1192`, `app/ponto/turnos.py:1349`, `app/ponto/turnos.py:1430`, `app/ponto/turnos.py:2021`, `app/ponto/motor_calculo_v2.py:3513` |
| **Vão entre períodos no motor** | **0 a 6h** desde uma saída real permite herdar `data_jornada` em `MotorBase.calcular_mes`. Esse trecho mantém os dois períodos e altera a atribuição ao dia. `app/ponto/motor_calculo_v2.py:2399`, `app/ponto/motor_calculo_v2.py:2423` |
| **Empresa, Sindicato e régua CCT** | Empresa tem piso de HE **0 min**, limite padrão de decisão **15 min** e tetos de Cadastro × Realidade **60 min** por ponta. Sindicato e `REGUA_LEGAL` têm intrajornada **mínima de 60 min** e teto de **prorrogação de 120 min**. A régua marca estes dois campos de Sindicato como sem efeito no cálculo e `get_motor_cct` injeta outros parâmetros. A aplicação por empresa/praça seleciona a convenção. Um campo que fixe o vão entre jornadas **[nao verificado]**. `app/colaboradores/models.py:58`, `app/colaboradores/models.py:69`, `app/colaboradores/models.py:101`, `app/core/models.py:63`, `app/core/models.py:119`, `app/core/regua_cct.py:39`, `app/core/regua_cct.py:72`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:428` |

## 3. Gestão de HE e desconto da pausa

- **Minutos e dias da tela.** `views.gestao_he` lê `he_pendente_lavrado.ler` e passa o retrato a `gestao_he.enriquecer`. A fonte da pendência é **`dia['he_fora_da_janela']`** de `espelho.montar_dias`. Com a janela empresarial ativa e vigente, ele publica as pontas da grade, complementa pontas faltantes pelo relato de Cadastro × Realidade do motor e incorpora o dia inteiro sem previsto quando o motor o publica. O retrato da competência soma as pontas por dia e é gravado em `MetricaSnapshot` por `lavrar_he_pendente`. `app/ponto/views.py:948`, `app/ponto/views.py:969`, `app/ponto/services/espelho.py:735`, `app/ponto/services/espelho.py:790`, `app/ponto/janela_he.py:194`, `app/ponto/janela_he.py:267`, `app/ponto/services/he_pendente_lavrado.py:55`, `app/ponto/services/he_pendente_lavrado.py:84`, `app/ponto/services/he_pendente_lavrado.py:132`, `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/management/commands/lavrar_he_pendente.py:62`.
- **Decisão e DST-L1.** A porta é `ponto/portas/he.py::decidir_he` e o modelo é `DecisaoHE`, por pessoa e dia. `gestao_he.estado_por_dia` lê `objects.vigentes()` e `estado_do_dia` publica `sem_decisao` quando a chave está ausente. O espelho usa o mesmo estado e o link DST-L1 leva à Gestão de HE com pessoa e dia em foco quando há ponta sem decisão. Para folga ou feriado elegível, com previsto zero, janela ativa e período calculado, o motor publica como pendência os minutos que pagaria, já líquidos. O escritor exige minutos positivos. A entrada isolada que o corte DST-L1 manda à porta do admin, por si só, não produz esse período. `app/docs/CORTES.md:575`, `app/ponto/portas/he.py:159`, `app/ponto/portas/he.py:186`, `app/ponto/models.py:1265`, `app/ponto/models.py:1270`, `app/ponto/services/gestao_he.py:69`, `app/ponto/services/gestao_he.py:98`, `app/ponto/services/espelho.py:936`, `app/ponto/views.py:170`, `app/ponto/motor_calculo_v2.py:1422`, `app/ponto/adaptador_motor.py:24`.
- **Intervalo efetivo.** `intervalos_do_turno` entrega os pares internos julgados ao adaptador, por timestamp de entrada. O motor também pode receber pausas pela ata. `calcular_periodo` aplica a janela de HE às bordas, recorta os intervalos ao trecho efetivo e subtrai `_intra_real(intervalos)` da duração. O desconto aparece no **12x36 acima de 360 min**, no **comercial acima de 240 min**, e o **plantão** delega ao 12x36. A decisão de HE altera o clipe, com recálculo por evento. A configuração de HE e os minutos líquidos dos quatro casos ficam **[nao verificado]**. `app/ponto/turnos.py:502`, `app/ponto/adaptador_motor.py:26`, `app/ponto/motor_calculo_v2.py:685`, `app/ponto/motor_calculo_v2.py:1892`, `app/ponto/motor_calculo_v2.py:2759`, `app/ponto/motor_calculo_v2.py:3224`, `app/ponto/motor_calculo_v2.py:3534`, `app/ponto/portas/he.py:206`.

## 4. Censo AST e ponto comum

O AST encontrou **13 chamadas diretas** a `parear_turnos`, contando aliases e deixando fora testes e migrações. O documento anterior é referenciado no backlog e o RELATO confirma a mesma contagem histórica. O arquivo `logs/PAR-CONFIRMADO/censo.md` está ausente da cópia entregue e seu conteúdo fica **[nao verificado]**. `app/docs/BACKLOG.md:2242`, `app/docs/RELATO.md:2857`, `.gitignore:53`.

| Apelido | Chamada a `parear_turnos` |
|---|---|
| Fila por causa | `particionar` · `app/chamados/services/fila_por_causa.py:197` |
| Observado de meio período | `observado_meio_periodo` · `app/escala/services/meio_periodo.py:168` |
| Semana do posto | `_bateram` · `app/escala/services/semana_do_posto.py:120` |
| Reconciliar fantasmas | `_completude` · `app/ponto/management/commands/reconciliar_fantasmas.py:99` |
| Motor v2 | `turnos_via_autoridade` · `app/ponto/motor_calculo_v2.py:503` |
| Diagnóstico de escala | `diagnostico_escala` · `app/ponto/services/diagnostico_escala.py:142` |
| Lastro | `julgar` · `app/ponto/services/lastro.py:268` |
| Piso de intervalo | `pares_abaixo_do_piso` · `app/ponto/services/piso_intervalo.py:48` |
| Triagem da tela de bater | `triagem_bater_ponto`, alias `_pt` · `app/ponto/services/triagem_batida.py:178` |
| Batidas já carregadas | `turnos_de_batidas` · `app/ponto/turnos.py:1535` |
| Juiz do colaborador / lote | `_turnos_do_colab_puro` · `app/ponto/turnos.py:1997` |
| Turno aberto vivo | `_turno_aberto_puro` · `app/ponto/turnos.py:2295` |
| Bloqueio de órfão | `detectar_bloqueio_turno_orfao` · `app/ponto/utils_turno_orfao.py:31` |

**`batidas_apuraveis` teve 57 chamadas diretas**, incluindo um script de `bin/`. A função seleciona marcações sem retratação e aplica os cortes de instante solicitados. A casca avulsa `turnos_do_colab` usa uma consulta própria com filtro de retratação e janela com bordas, enquanto `CargaDaJanela` chama a porta diretamente. `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:2877`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3038`, `bin/r3_turno_aberto_alcanca.py:61`.

| Apelidos dos leitores de `batidas_apuraveis` | Sítios |
|---|---|
| APIs e painel | Bater e justificativas `app/api/views.py:531`, `app/api/views.py:2030`, `app/api/views_core.py:517`. Painel operacional e dashboard `app/core/services/painel_op.py:83`, `app/core/services/painel_op.py:269`, `app/core/views.py:114`. Detalhe `app/colaboradores/services/detalhe.py:49`. |
| Chamados | Fato `app/chamados/juizes.py:1825`. Fio `app/chamados/services/fio.py:1673`, `app/chamados/services/fio.py:2054`. Vizinhas `app/chamados/services/materializacao.py:112`, `app/chamados/services/materializacao.py:115`. Tarefa e validação `app/chamados/services/tarefa_do_dia.py:443`, `app/chamados/services/validacao.py:1207`. |
| Escala e grade | Defesa `app/escala/regua_defesa.py:153`, `app/escala/regua_defesa.py:523`. Fase, previsto cego, posto e grade `app/escala/services/fase_na_tela.py:47`, `app/escala/services/previsto_cego.py:159`, `app/escala/services/semana_do_posto.py:117`, `app/escala/utils.py:1495`. |
| Folha, férias, pauta e relatórios | `app/folha/export.py:499`, `app/folha/export.py:914`, `app/folha/previa.py:58`, `app/ferias/management/commands/listar_ferias_com_batida.py:47`, `app/pautas/services.py:431`, `app/relatorios/pdf_espelho.py:272`, `app/relatorios/views.py:502`, `app/relatorios/management/commands/diff_extrato_parcial.py:37`. |
| Operação do ponto | Abertura, cartório, cobertura, deslogados e dia `app/ponto/services/abriu_nao_bateu.py:174`, `app/ponto/services/cartorio.py:1201`, `app/ponto/services/cobertura.py:301`, `app/ponto/services/deslogados.py:172`, `app/ponto/services/dia_do_colab.py:60`. Diagnóstico, passivo, esmeril, espelho, faltas e fechamento `app/ponto/services/diagnostico_escala.py:142`, `app/ponto/services/dias_em_aberto.py:122`, `app/ponto/services/esmeril_espelho.py:165`, `app/ponto/services/espelho.py:1058`, `app/ponto/services/faltas_de_hoje.py:57`, `app/ponto/services/fechamento.py:633`, `app/ponto/services/fechamento.py:811`. Triagem, vigia, lote e bloqueio `app/ponto/services/triagem_batida.py:175`, `app/ponto/services/triagem_batida.py:666`, `app/ponto/services/triagem_batida.py:716`, `app/ponto/services/vigia_de_hora.py:79`, `app/ponto/turnos.py:3020`, `app/ponto/turnos.py:3038`, `app/ponto/utils_turno_orfao.py:28`. |
| Comandos de ponto e script | `app/ponto/management/commands/aplicar_09_corte_b.py:126`, `app/ponto/management/commands/censo_col900.py:128`, `app/ponto/management/commands/diff_espelho_alimentacao.py:92`, `app/ponto/management/commands/e6_oraculo.py:152`, `app/ponto/management/commands/e6_oraculo.py:212`, `app/ponto/management/commands/mapa_divergencia.py:96`, `app/ponto/management/commands/reconciliar_fantasmas.py:96`, `app/ponto/management/commands/tripwire_celulas.py:283`, `app/ponto/management/commands/vigiar_motor_autoridade.py:54`, `bin/r3_turno_aberto_alcanca.py:61`. |

**Leitores indiretos e alcance do ponto único.** Espelho e cartão PDF consultam `turnos_do_colab` e também leem a grade. O cartório monta a grade e passa o dia a `supra_juiz.classificar_dia`, que consome `dia['orfas']`. Nos chamados, `perguntar_entradas_pendentes` resolve o dia pelo juiz do turno e consulta `classificar_falta`. `_orfao_resolvido_cross_meianoite` lê os pares fechados, com guarda própria de 26h. O núcleo materializa `TurnoMaterializado` a partir de `turnos_do_colab`. `app/ponto/services/espelho.py:520`, `app/ponto/services/espelho.py:625`, `app/relatorios/pdf_espelho.py:211`, `app/relatorios/pdf_espelho.py:475`, `app/ponto/services/cartorio.py:1201`, `app/ponto/services/cartorio.py:1500`, `app/ponto/supra_juiz.py:522`, `app/chamados/services/disputa_emissao.py:257`, `app/chamados/services/disputa_emissao.py:721`, `app/ponto/nucleo.py:40`.

**O sítio comum da regra geométrica é `parear_turnos`, com `_teto_da_jornada` compartilhado pelos fechamentos.** A mesma resposta para todos exige também a mesma alimentação e o consumo do par devolvido. Há chamadas sem DNA ou ata, `turnos_do_colab` segmenta a janela, e o v2 monta períodos por `turnos_via_autoridade` e pode substituí-los por `juiz_batida.periodos_do_dia`. Ele obtém o mapa de dia em outra chamada e ainda aplica a herança de 6h. Portanto, mudar só o teto alcança a regra compartilhada, mas não garante par idêntico em todos os leitores. `MOTOR-LE-O-TURNO` já registra a convergência de motor e DiaPago para o par e o dia de `turnos_do_colab`. O arquivo do motor é zona inviolável e o aval registrado é para esse escopo. `app/ponto/turnos.py:66`, `app/ponto/turnos.py:1997`, `app/ponto/utils_turno_orfao.py:31`, `app/ponto/motor_calculo_v2.py:453`, `app/ponto/motor_calculo_v2.py:611`, `app/ponto/motor_calculo_v2.py:679`, `app/ponto/motor_calculo_v2.py:2346`, `app/ponto/motor_calculo_v2.py:2400`, `app/core/juizes.py:215`, `app/docs/BACKLOG.md:2173`, `CLAUDE.md:142`.

## 5. Lista de órfãs e vigia

**Existe lista real por dia.** A grade publica `dia['orfas']`, o cartório grava as horas e `n_orfas` em `CelulaDia.ata`, e `grade_da_celula` as devolve por `_orfas_da_ata`. `datas_saida_orfa` é uma leitura mais estreita, só dos dias de turno em que o pareador devolveu uma `S` sem `E`. `app/escala/utils.py:1818`, `app/escala/utils.py:1824`, `app/ponto/services/cartorio.py:755`, `app/escala/services/leitor_celula.py:180`, `app/escala/services/leitor_celula.py:420`, `app/ponto/turnos.py:234`.

O vigia `vigia_pareador_celula.censo` lista `(colab, dia_turno, borda, hora)` quando **a própria borda de um par fechado** consta como órfã na ata e sua lâmpada correspondente está apagada. Ele publica `cego` quando falta universo observável. `tripwire_celulas.medir_sobra_com_tipo_trocado` mede outra classe específica, com órfã no raio de ponta apagada, confirmação por instante e par-relâmpago de **até 10 min**. Um contador geral de **órfã contígua a qualquer turno** e o **zero da frota da comp. 11** ficam **[nao verificado]**. `app/ponto/services/vigia_pareador_celula.py:42`, `app/ponto/services/vigia_pareador_celula.py:93`, `app/ponto/management/commands/tripwire_celulas.py:199`, `aval.txt:1`.

## 6. DNA, ata e competência exportada

- **DNA e janela.** `CelulaDia.dna.marcos` congela o previsto. `EscalaColaborador.marcos_do_dia` lê o DNA e cai no template na ausência de marcos. O lote `marcos_dna_periodo` alimenta o teto do pareador quando disponível. Há caminhos que usam o template, inclusive a guarda de segundo intervalo na alimentação do pareador. A janela prevista pode vir do DNA, enquanto a janela de consulta de `turnos_do_colab` é ou

[truncado em 20000 de 22133 caracteres]

## 23/11 13:19 · sessao a29591adb793 · A-TELA-DO-DIA-LE-UM-JUIZ · leitura · origem=Code

```python
import json
import logging
from collections import Counter
from datetime import date
from math import fsum
from unittest.mock import patch

from django.db import connection, transaction
from django.db.models import Max
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Colaborador
from colaboradores.services.calendario import (
    STATUS_QUE_PEDEM_DECISAO,
    contexto_calendario,
)
from escala.alimentacao import celulas_do_periodo
from escala.utils import minutos_previstos_do_dia
from folha.export import competencia_entregue
from ponto.janelas import janela_atual, janela_fechamento
from ponto.models import Batida
from ponto.services.dia_pago import pago_do_dia
from ponto.turnos import CargaDaJanela, cobertura_ausencia_periodo, realizado_do_dia

MES, ANO = 11, 2026
DIAS_AUSENCIA_941 = tuple(date(2026, 11, d) for d in range(16, 20))
CASOS = {
    945: (date(2026, 10, 24), date(2026, 10, 26), date(2026, 10, 28)),
    941: (date(2026, 10, 31), *DIAS_AUSENCIA_941),
}


def falhar_em_log_de_erro(_logger, registro):
    # A tela tem fallbacks com exc_info, por exemplo em app/colaboradores/services/calendario.py:622.
    if registro.exc_info and registro.exc_info[1] is not None:
        _, erro, traceback = registro.exc_info
        raise erro.with_traceback(traceback)
    if registro.levelno >= logging.ERROR:
        raise RuntimeError(f"Erro registrado em {registro.pathname}:{registro.lineno}")


def ler_grade(colab, mes, ano, agora):
    ctx = contexto_calendario(
        colab, {"modo": "competencia", "mes": str(mes), "ano": str(ano)},
        agora=agora,
    )
    for dia in ctx["dias"]:
        if not dia["em_aberto_perguntado"] or dia["resolver_dia_oferecido"] is None:
            raise RuntimeError(f"Grade sem resposta para pk={colab.pk}, dia={dia['data']}")
    return ctx


def minutos_pagos(dia):
    horas = pago_do_dia(dia["dia_pago"])
    return None if horas is None else round(horas * 60, 9)


def batidas(batidas_da_grade):
    return [
        [timezone.localtime(b.timestamp).isoformat(), b.tipo]
        for b in batidas_da_grade
    ]


def registro_ausencia(a):
    if a is None:
        return None
    return {
        "pk": a.pk,
        "data": a.data.isoformat(),
        "data_fim": a.data_fim.isoformat() if a.data_fim is not None else None,
        "minutos": a.minutos,
        "tipo": a.tipo,
        "status": a.status,
    }


def imprimir(obj):
    print(json.dumps(obj, ensure_ascii=False), flush=True)


def sondar():
    if connection.settings_dict["NAME"] != "sombra":
        raise RuntimeError("Use o banco de ensaio sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transação própria.")

    with (
        patch.object(logging.Logger, "handle", falhar_em_log_de_erro),
        schema_context("juliani"),
        transaction.atomic(),
    ):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco/schema diferente de sombra/juliani.")

        agora = timezone.now()
        ultima = Batida.objects.aggregate(ultima=Max("timestamp"))["ultima"]
        qs = Colaborador.objects.select_related("empresa", "posto__praca").order_by("pk")
        colabs = list(qs.filter(situacao="ativo"))
        por_pk = {c.pk: c for c in colabs}
        colabs_dos_casos = {
            pk: por_pk[pk] if pk in por_pk else qs.get(pk=pk)
            for pk in CASOS
        }
        empresas = {
            c.empresa_id: c.empresa for c in [*colabs, *colabs_dos_casos.values()]
        }
        cabecalho_empresas = []
        for pk, empresa in sorted(empresas.items()):
            ini, fim = janela_fechamento(MES, ANO, empresa)
            cabecalho_empresas.append({
                "empresa_pk": pk,
                "inicio": ini.isoformat(),
                "fim": fim.isoformat(),
                "exportada": competencia_entregue(pk, MES, ANO),
            })
        imprimir({
            "dump_referencia_max_batida_timestamp": (
                timezone.localtime(ultima).isoformat() if ultima is not None else None
            ),
            "competencia": "2026-11",
            "empresas": cabecalho_empresas,
            "colaboradores_no_universo": len(colabs),
        })

        por_status = Counter()
        afetados = 0
        comparacao = Counter()
        excessos = []
        grades_dos_casos = {}
        for colab in colabs:
            ctx = ler_grade(colab, MES, ANO, agora)
            if colab.pk in CASOS:
                grades_dos_casos[colab.pk] = {
                    (MES, ANO): {dia["data"]: dia for dia in ctx["dias"]}
                }
            divergentes = [
                dia for dia in ctx["dias"]
                if "Em aberto" in dia["palavra_dia"]
                and dia["status"] not in STATUS_QUE_PEDEM_DECISAO
            ]
            if not divergentes:
                continue
            afetados += 1
            turnos = None
            for dia in divergentes:
                por_status[dia["status"]] += 1
                d = dia["data"]
                previsto = minutos_previstos_do_dia(colab, d)
                pago = minutos_pagos(dia)
                if pago is None:
                    comparacao["sem_pago"] += 1
                    continue
                comparacao["ge" if pago >= previsto else "lt"] += 1
                if turnos is None:
                    turnos = CargaDaJanela([colab], ctx["data_ini"], ctx["data_fim"]).turnos(colab)
                realizado = realizado_do_dia(colab, d, turnos=turnos, previsto_min=previsto)
                if realizado.minutos is None:
                    comparacao["sem_realizado_dos_pares"] += 1
                else:
                    excessos.append(max(0, pago - realizado.minutos))

        saida_casos = []
        for pk, datas in CASOS.items():
            colab = colabs_dos_casos[pk]
            grades = grades_dos_casos.get(pk, {})
            celulas = celulas_do_periodo([pk], min(datas), max(datas))
            cobertura = (
                cobertura_ausencia_periodo(
                    colab, min(DIAS_AUSENCIA_941), max(DIAS_AUSENCIA_941), para="folha",
                ) if pk == 941 else {}
            )
            for d in datas:
                _, fim = janela_atual(d, colab.empresa)
                competencia = (fim.month, fim.year)
                if competencia not in grades:
                    ctx = ler_grade(colab, fim.month, fim.year, agora)
                    grades[competencia] = {dia["data"]: dia for dia in ctx["dias"]}
                dia = grades[competencia][d]
                celula = celulas.get((pk, d))
                caso = {
                    "colaborador_pk": pk,
                    "data": d.isoformat(),
                    "status": dia["status"],
                    "texto": dia["palavra_dia"],
                    "resolver_dia_oferecido": dia["resolver_dia_oferecido"],
                    "batidas_do_turno": batidas(dia["batidas"]),
                    "batidas_retratadas_do_dia_civil": batidas(dia["retratadas"]),
                    "celula_status_dia": celula.status_dia if celula is not None else None,
                    "minutos_pagos": minutos_pagos(dia),
                    "minutos_previstos": minutos_previstos_do_dia(colab, d, celulas=celulas),
                }
                if pk == 941 and d in DIAS_AUSENCIA_941:
                    caso["ausencia_cobertura_folha"] = registro_ausencia(cobertura.get(d))
                saida_casos.append(caso)

    imprimir({
        "dias_divergentes_por_status": dict(sorted(por_status.items())),
        "colaboradores_com_divergencia": afetados,
        "divergencia_status_x_em_aberto_total": sum(por_status.values()),
        "dias_pago_ge_previsto": comparacao["ge"],
        "dias_pago_lt_previsto": comparacao["lt"],
        "dias_sem_minutos_pagos_sustentados": comparacao["sem_pago"],
        "dias_com_pago_sem_minutos_dos_pares": comparacao["sem_realizado_dos_pares"],
        "dias_com_pago_e_minutos_dos_pares": len(excessos),
        "soma_minutos_pagos_acima_dos_pares": round(fsum(excessos), 9),
    })
    for caso in saida_casos:
        imprimir(caso)


sondar()
```

**Sítios no repositório**

- **Universo.** Adotei a lista padrão de Colaboradores, `situacao="ativo"`, com todas as páginas e sem filtros opcionais. As contagens usam esse universo. Os casos literais são consultados por PK mesmo quando estão fora dele, como permite a entrada do calendário do perfil. `app/colaboradores/views.py:1044`, `app/colaboradores/views.py:1048`, `app/colaboradores/views.py:1066`, `app/colaboradores/views.py:1077`, `app/colaboradores/views.py:1984`.
- **Calendário.** `contexto_calendario` recebe o colaborador, um dicionário de parâmetros e um instante comum à rodada. É a chamada direta usada pelo drawer, sem request ou usuário para produzir os campos pedidos. A sonda lê `palavra_dia`, `status` e `resolver_dia_oferecido` da própria grade. A permissão do usuário é aplicada pelo template ao exibir o botão. `app/colaboradores/services/drawer.py:122`, `app/colaboradores/services/calendario.py:188`, `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:754`, `app/colaboradores/services/calendario.py:809`, `app/templates/colaboradores/partials/_calendario_grade.html:68`, `app/templates/colaboradores/partials/_calendario_grade.html:134`.
- **Competência e exportação.** `janela_fechamento` usa o corte da empresa. `janela_atual` localiza a competência de cada data literal. `competencia_entregue` verifica o TXT vigente para empresa, mês e ano. `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/ponto/janelas.py:60`, `app/colaboradores/services/calendario.py:204`, `app/folha/export.py:556`, `app/folha/export.py:588`.
- **Pago e previsto.** A grade chama `linhas_do_periodo` e `rubricas_do_dia` e entrega o próprio `DiaPago`. Sobre essa linha, `pago_do_dia` lê as horas pagas de trabalho, incluindo folga trabalhada e folga em feriado. A sonda conta separadamente os dias em que o leitor devolve `None`. O previsto vem de `minutos_previstos_do_dia`. `app/colaboradores/services/calendario.py:319`, `app/colaboradores/services/calendario.py:688`, `app/colaboradores/services/calendario.py:781`, `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:867`, `app/ponto/services/dia_pago.py:901`, `app/escala/utils.py:2241`.
- **Pares fechados.** `CargaDaJanela.turnos` usa o mesmo carregador da grade. `realizado_do_dia` fornece os minutos realizados pelo pareador, como no espelho. A soma acumula diferenças positivas nos dias em que ambas as fontes devolvem minutos. `app/colaboradores/services/calendario.py:249`, `app/ponto/turnos.py:2980`, `app/ponto/turnos.py:3044`, `app/ponto/turnos.py:774`, `app/ponto/services/espelho.py:276`, `app/ponto/services/espelho.py:412`.
- **Casos literais.** `celulas_do_periodo` carrega a `CelulaDia` e seu `status_dia` persistido. As batidas seguem a grade, que atribui as vivas ao dia do turno e as retratadas ao dia civil local. `cobertura_ausencia_periodo(..., para="folha")` usa a mesma cobertura aprovada que alimenta a palavra do dia. `app/escala/alimentacao.py:54`, `app/escala/models.py:1827`, `app/colaboradores/services/calendario.py:643`, `app/colaboradores/services/calendario.py:658`, `app/colaboradores/services/calendario.py:765`, `app/colaboradores/services/calendario.py:561`, `app/colaboradores/services/calendario.py:748`, `app/ponto/turnos.py:2752`, `app/ponto/models.py:749`, `app/ponto/models.py:809`.
- **Sombra e erros.** A configuração `config.settings.sombra` aponta para o banco `sombra`, com schema `juliani`. A transação segue o padrão `REPEATABLE READ, READ ONLY` da sonda da cópia. O tratamento de log relança as exceções registradas com `exc_info` nas degradações da tela. O cabeçalho usa o maior `Batida.timestamp` como referência de data do dump. `app/config/settings/sombra.py:13`, `app/config/settings/sombra.py:29`, `app/config/settings/sombra.py:47`, `app/chamados/tests/cef_c3_sonda.py:21`, `app/colaboradores/services/calendario.py:509`, `app/colaboradores/services/calendario.py:617`, `app/ponto/models.py:23`.

Execução na sombra, contagens, data efetiva do dump e confirmação independente do hash [não verificado].

## 23/11 13:18 · sessao c69dd7bd31d0 · VIGIA-DO-TMUX · leitura · origem=Code

Usei a cópia no diretório `eac7a67419eb54c3af01f061dde311b5bae06717`. O procedimento do repo entrega essas cópias via `git archive`. A confirmação independente do hash por Git e o estado atual do host ficam [nao verificado]. As linhas dos anexos contam a partir do primeiro conteúdo de cada anexo. [CLAUDE.md:354](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/CLAUDE.md:354)

## 1. O contrato cobre o aval?

**Parcialmente.** Ele cobre o agendamento, os dois caminhos de ação e os registros. Faltam condições explícitas para autorizar o `/clear` e critérios do pouso. O contrato também acrescenta escrita no RELATO quando nenhuma tecla foi enviada. `aval.txt:1`, `contrato.txt:2–6`. [nao verificado]

| Exigência ou proibição do aval | Cobertura no contrato |
|---|---|
| Descongelar só esta obra. Manter `AUTO-CLEAR-NO-MARCO` congelado e substituído. | **Lacuna.** As cinco linhas descrevem o vigia novo sem declarar o limite do descongelamento ou a substituição. O item antigo consta no BACKLOG da cópia. `aval.txt:1`, `contrato.txt:2–6`. [app/docs/BACKLOG.md:2853](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/BACKLOG.md:2853) |
| Vigia externo, somente na sessão tmux `code`, lendo o topo a cada poucos minutos. Reversível ao desligar. | **Coberto no desenho.** Cron de host a cada 3 min, nenhum chamado pelo Code, leitura das 15 primeiras linhas e pausa por arquivo. Retirar a declaração exige reinstalar o crontab para surtir efeito. `aval.txt:1`, `contrato.txt:2–3,6`. [bin/crons.sh:29](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/crons.sh:29) |
| `ESPERANDO` com produtor morto → frase exata com PID e arquivo. | **Coberto no texto.** A frase é literal. A prova proposta, `kill -0`, consulta o PID e deixa aberta a identidade do produtor. O Stop atual também admite espera com só `pid=` ou só `arquivo=`. A frase do vigia exige os dois. `aval.txt:1`, `contrato.txt:3–4`. [bin/hook_stop_fila1.py:809](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:809) |
| `/clear` só com `PAREI: ! CLEAR` e “nada roda -- PODE /clear” escrito pelo próprio Code. Depois, ler `HANDOFF-SESSAO` e o topo do RELATO, seguir a fila, parar só em lei ou `!`. | **Parcial.** O item 2 exige o texto autorizador e ordena `/clear` antes da retomada. Falta exigir a autoria do Code e fixar o conteúdo da retomada. O Stop da cópia orienta escrever `PAREI: ! CLEAR \| espera Ronald`, sem a autorização literal nova. `aval.txt:1`, `contrato.txt:3`. [bin/hook_stop_fila1.py:1116](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:1116) |
| Digitar somente o autorizado, nunca no meio de turno ativo, e preservar a config do Code. | **Parcial.** “Nada mais é digitado” é explícito. A guarda de turno ativo fica sem definição do indicador e da caixa vazia. A proibição de mexer na config do Code fica implícita no desenho de host. `aval.txt:1`, `contrato.txt:2–4`. |
| Registrar cada ação no RELATO e em log próprio. Nunca repetir o empurrão da mesma linha. | **Previsto.** O item 4 dá os dois destinos. O item 3 dá o SHA da linha, mas omite a coordenação entre a tecla e a gravação do estado. **Além do aval**, o item 4 manda gravar também cada adiamento e recusa nos dois destinos. `aval.txt:1`, `contrato.txt:4–5`. |
| Contrato de cinco linhas **no RELATO**, selo de host que morde os dois casos e o caso sem ação, pouso próprio e no ar antes da noite se couber no teto. | **Parcial.** O anexo tem os cinco campos e especifica o selo com tmux falso e os três grupos de casos. Ainda faltam no texto o pouso próprio e a condição de prazo e vaga. Registro no RELATO, selo executado, pouso e vaga real [nao verificado]. `aval.txt:1`, `contrato.txt:2–6`. [app/docs/PISTA.md:170](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/PISTA.md:170), [CLAUDE.md:297](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/CLAUDE.md:297), [CLAUDE.md:331](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/CLAUDE.md:331) |

## 2. Onde entra o cron de host de 3 minutos?

- **Aceita comando fora do Django.** `C(...)` gera `tenant_command`. `RAW(...)` aceita script de host e `linhas()` o passa ao `cron_run.sh`, que executa o comando e mede a corrida. `bin/crons.sh` usa Django para **renderizar** e instala o texto resultante no crontab do host. `vigia_arvore.sh` já entra por `RAW`, na hora cheia mais cinco minutos. O alarme da sessão por `RAW` é o precedente mais próximo. [app/config/crons.py:19](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:19), [app/config/crons.py:102](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:102), [app/config/crons.py:292](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:292), [app/config/crons.py:311](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:311), [app/config/crons.py:1895](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:1895), [bin/cron_run.sh:5](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/cron_run.sh:5), [bin/crons.sh:23](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/crons.sh:23)
- **O sítio do contrato é `CRONS` em `app/config/crons.py`, com `RAW('*/3 * * * *', ...)`.** Conferi em memória que `log='logs/vigia_tmux/vigia.log'` resolve para o caminho previsto. Um relativo com barra é resolvido a partir da raiz do repo, então `vigia_tmux/vigia.log` cairia fora de `logs/`. `vigia_esteira.sh` tem outro mecanismo, uma unidade systemd de usuário declarada em `AGENDADOS_FORA_DO_CRON`. O `OnCalendar` versionado é `*:0/5`, embora o comentário diga 10 min. `contrato.txt:2,5`. [app/config/crons.py:1888](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:1888), [app/config/crons.py:760](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:760), [bin/hasner-esteira.timer:1](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hasner-esteira.timer:1), [bin/hasner-esteira.service:13](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hasner-esteira.service:13)
- **O que fica vermelho com só a entrada nova.** `test_contract_tabuleiro` cobra `vigia_tmux.py` em `PAPEL_DO_CRON`. `test_contract_crons` cobra o mesmo nome em `DUR_MAX_S`, por duração medida ou declarada. O gerador do diagrama consulta `papel()` e falha se faltar a chave. Com papel `vigia`, mudam a contagem do Tabuleiro e o bloco Vigias de `ARQUITETURA.mmd`. O selo compara `ARQUITETURA.mmd` e `MAPA.md` com o gerador. O bloco do pipeline diário pula `RAW`. `CRONS.md` pede atualização no mesmo commit como tabela de consulta. Um selo automático de paridade para `CRONS.md` [nao verificado]. [app/core/tests/test_contract_tabuleiro.py:15](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/tests/test_contract_tabuleiro.py:15), [app/chamados/tests/test_contract_crons.py:369](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/chamados/tests/test_contract_crons.py:369), [app/config/crons.py:2375](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:2375), [app/core/management/commands/gerar_diagrama.py:163](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:163), [app/core/management/commands/gerar_diagrama.py:242](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:242), [app/core/management/commands/gerar_diagrama.py:395](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:395), [app/core/management/commands/gerar_diagrama.py:921](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/gerar_diagrama.py:921), [app/docs/CRONS.md:7](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/CRONS.md:7)
- **Há uma trava de pouso separada.** O render integral da cópia, sem alteração, recusou `sombra.sh`: 87 min empurrariam o início para 03:56, antes do piso 04:05. A marca CRON-15 está documentada até **27/11/2026** e veda cron novo enquanto existir. O `install` pode tecnicamente preservar a linha existente da sombra sob condições estritas. O aval fornecido cita o descongelamento de `CONGELA-INSTRUMENTO`, e uma exceção à CRON-15 para esta obra fica [nao verificado]. `aval.txt:1`. [app/config/crons_duracao.json:78](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons_duracao.json:78), [app/config/crons.py:259](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/config/crons.py:259), [app/core/tests/marca_cron15.py:7](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/tests/marca_cron15.py:7), [app/core/management/commands/crons.py:91](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/management/commands/crons.py:91)

## 3. Como o repo reconhece turno ativo no pane?

- **O texto já usado é `esc to interrupt`.** `keepalive.sh` procura nas últimas 25 linhas e também reconhece `background agent`, `shell(s) still running`, `◯ ` e contagem de shells. `alarme_sessao_ociosa.py` pega o primeiro pane cujo comando aparece como `claude`, procura `esc to interrupt` e usa também a idade do transcript para decidir se avisa. São precedentes de heurística. O marcador da versão em produção e um reconhecedor confiável de caixa de entrada vazia ficam [nao verificado]. [bin/keepalive.sh:3](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/keepalive.sh:3), [bin/alarme_sessao_ociosa.py:48](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:48), [bin/alarme_sessao_ociosa.py:66](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:66), [bin/alarme_sessao_ociosa.py:144](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:144)
- **Falsos “ocioso” possíveis.** O texto pode ficar fora da janela, mudar ou faltar durante uma transição. Uma captura com erro pode parecer vazia se o leitor olhar só o `stdout`. O alvo `-t code` ainda pode casar por prefixo com `codex-hasner` quando `code` está ausente, caso reproduzido no selo com tmux falso. `gerar_estado.py` usa `=code:` para casar o nome exato da sessão. Os hooks existentes consultam transcript, processos ou arquivos para suas decisões, conforme a porta. [bin/keepalive.sh:5](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/keepalive.sh:5), [bin/alarme_sessao_ociosa.py:69](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/alarme_sessao_ociosa.py:69), [bin/gerar_estado.py:17](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/gerar_estado.py:17), [bin/tests/test_painel_e_do_code.sh:19](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/tests/test_painel_e_do_code.sh:19), [bin/hook_stop_fila1.py:382](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:382), [bin/hook_stop_fila1.py:936](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:936)

## 4. Quais falhas ficam abertas?

| Falha | O que fica sem regra |
|---|---|
| **Vigia e Code escrevem no RELATO ao mesmo tempo** | O vigia pode agir sobre uma autorização já retirada ou sobrescrever uma edição feita depois da leitura. A porta existente serializa os escritores que a usam com `flock` e `os.replace`. O próprio código registra que a edição manual e `gerar_cortes.py` ficam fora dessa trava. O contrato também omite trava entre dois ticks e o que fazer se a tecla sair antes de falhar a gravação do SHA. `contrato.txt:3–5`. [app/core/esteira_vigia.py:834](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:834), [app/core/esteira_vigia.py:915](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:915) |
| **Code declara PID de outro processo** | Se o PID errado estiver vivo, o empurrão pode ficar retido. Se estiver morto, o vigia pode anunciar que o produtor terminou sem prova. O Stop já compara executável e `argv` com o produtor declarado, verifica ancestrais e distingue leitura falha de PID morto. `contrato.txt:3–4`. [bin/hook_stop_fila1.py:577](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:577), [bin/hook_stop_fila1.py:599](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:599) |
| **PID reciclado** | Um novo processo no mesmo número pode segurar a retomada. O Stop confere `starttime` antes e depois de ler o processo, o que pega a troca durante a consulta. O contrato do vigia carece de uma identidade registrada na declaração para distinguir a instância original de outra que já ocupava o número antes da consulta. `contrato.txt:3–4`. [bin/hook_stop_fila1.py:570](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:570), [bin/hook_stop_fila1.py:610](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:610) |
| **`/clear` no meio de texto digitado** | A caixa pode estar vazia na captura e receber texto antes do `send-keys`. A checagem do item 3 cobre só o instante observado. O contrato deixa em aberto como tratar essa janela entre checar e digitar. `contrato.txt:3–4`. [bin/keepalive.sh:5](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/keepalive.sh:5) |
| **Retomada antes de o `/clear` terminar** | “Espera o prompt vazio” carece de um sinal que confirme a transição, de um timeout e de uma regra para o caso em que só o `/clear` saiu. Já existe `SessionStart` com matcher `clear`, que lê o handoff e grava hora, origem e sessão em `ultimo_clear.json`. É evidência de início da nova sessão. A prontidão do pane após o hook fica [nao verificado]. `contrato.txt:3–5`. [.claude/settings.json:48](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/.claude/settings.json:48), [bin/marco_de_clear.py:19](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/marco_de_clear.py:19), [bin/marco_de_clear.py:43](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/marco_de_clear.py:43) |

## 5. Quem escreve no topo e o que `VIGIA-TMUX` quebra?

- **O escritor instrumental do topo útil é o disjuntor.** `disjuntor._parei_censo_no_relato` usa `esteira_vigia.no_topo` com `hook_stop_fila1.painel_do_topo` e relê pela porta do Stop para provar a escrita. A faixa acima do painel só reconhece o literal `PAREI: censo-<ID> | espera Codex`. O Stop lê esse topo e grava sua própria trilha à parte. `pos_push.sh` grava o topo do **TICKETS**. `gerar_cortes.py` reescreve o bloco `SEUS-CORTES` entre marcadores e, se faltarem, o insere antes da primeira seção `## `. [bin/disjuntor.py:299](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/disjuntor.py:299), [app/core/esteira_vigia.py:865](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:865), [bin/hook_stop_fila1.py:39](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:39), [bin/hook_stop_fila1.py:419](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:419), [bin/pos_push.sh:18](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/pos_push.sh:18), [bin/gerar_cortes.py:114](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/gerar_cortes.py:114)
- **Sim, `VIGIA-TMUX ...` na primeira linha útil quebra a leitura do Stop e do B3.** Conferi em memória com o leitor da cópia. Antes ele devolve `[ESPERANDO, DESENHO]`. Depois da inserção, devolve `[VIGIA-TMUX, ESPERANDO]`. Assim o Stop perde o comando na primeira posição e o B3 perde a pergunta na segunda. Um `PAREI` ou `CLEAR` deslocado também deixa de ser visto naquela primeira posição. No handoff, a lista textual ainda encontra `PAREI`, `lei:` e `!:` até o primeiro `## `, mas os vereditos importados do Stop e a leitura da faixa do censo podem mudar. [bin/hook_stop_fila1.py:408](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:408), [bin/hook_stop_fila1.py:446](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:446), [bin/hook_stop_fila1.py:1058](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:1058), [bin/disjuntor.py:758](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/disjuntor.py:758), [bin/handoff_sessao.sh:85](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/handoff_sessao.sh:85), [bin/handoff_sessao.sh:114](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/handoff_sessao.sh:114)
- **O precedente compatível com a trilha do vigia é `no_relato` na seção `## ALARMES DA ESTEIRA`.** O aval e o contrato pedem uma linha no RELATO. Essa porta registra na seção pinada, abaixo do painel útil. Para decidir o que fazer, as 15 linhas do contrato também precisam respeitar o recorte do Stop. A própria cópia tem, nas linhas 13 e 14, `ESPERANDO` antigos citados em linhas `SUPERADA`. `aval.txt:1`, `contrato.txt:3,5`. [app/core/esteira_vigia.py:815](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/core/esteira_vigia.py:815), [bin/hook_stop_fila1.py:462](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/bin/hook_stop_fila1.py:462), [app/docs/RELATO.md:13](/home/ronald/codex_ws/eac7a67419eb54c3af01f061dde311b5bae06717/app/docs/RELATO.md:13)

## 23/11 13:08 · sessao 4b9ca280964f · A-TELA-DO-DIA-LE-UM-JUIZ · leitura · origem=Code

## 1. Quem produz o status, o texto e o botão?

O calendário do perfil e o drawer chamam `contexto_calendario` e renderizam o mesmo `_calendario_grade.html`. O detalhe do colaborador carrega esse calendário por HTMX. `app/colaboradores/views.py:1986`, `app/colaboradores/views.py:2987`, `app/colaboradores/services/drawer.py:122`, `app/templates/colaboradores/detalhe.html:215`, `app/templates/colaboradores/calendario_partial.html:67`, `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`

| Saída | Quem produz e o que lê |
|---|---|
| **Status, cor e ícone** | `contexto_calendario` lê `status_dos_dias_lavrados`, a forma em lote do juiz **status do dia** registrado em `JUIZES.md`. O D4 consulta o encerramento do turno, lê `CelulaDia.status_dia` e `status_rotulo` quando a lavra pode ser usada e, nos demais ramos, responde pelo relógio, pelos fatos do dia ou pela falta de certidão. O cartório passa os fatos da grade e o veredito à porta que grava ata, veredito e status. O template usa `dia.status` para pintar a célula. `app/docs/JUIZES.md:133`, `app/core/juizes.py:207`, `app/colaboradores/services/calendario.py:635`, `app/ponto/status_dia.py:272`, `app/ponto/status_dia.py:296`, `app/ponto/services/cartorio.py:1898`, `app/ponto/portas/celula.py:1045`, `app/ponto/portas/celula.py:1126`, `app/templates/colaboradores/partials/_calendario_grade.html:68` |
| **Texto** | `dia_decidido.do_dia` monta `palavra_dia` com a precedência do dia, a cobertura aprovada para folha, a decisão de ausência, a lista de datas em aberto, a ata sem turno pareado e os marcos faltantes. Para montar a lista, o calendário consulta `espelho.autoridade_do_periodo` para obter `resultado.datas_falta` do motor e usa os turnos carregados para obter turnos abertos e saídas órfãs. `folha_manda` publica a lista calculada por `datas_em_aberto_de`. As autoridades da tabela usadas aqui são **precedência**, **ausência cobre**, **geometria de turno** e **cálculo**. O juiz tabelado como **furo do dia**, `classificar_falta`, responde qual batida falta para a cobrança. `dias_em_aberto.py::passivo_da_competencia` consome a lista pronta para a pauta do DP. Na grade, “Em aberto” vai no `title` da célula. Para uma falta com chip de ausência, a frase completa vai no `title` do chip. `app/colaboradores/services/calendario.py:539`, `app/colaboradores/services/calendario.py:561`, `app/colaboradores/services/calendario.py:595`, `app/colaboradores/services/calendario.py:748`, `app/relatorios/cartao_pela_celula.py:419`, `app/docs/JUIZES.md:20`, `app/docs/JUIZES.md:29`, `app/docs/JUIZES.md:127`, `app/escala/servico_jornada.py:280`, `app/ponto/services/dias_em_aberto.py:77`, `app/templates/colaboradores/partials/_calendario_grade.html:68`, `app/templates/colaboradores/partials/_calendario_grade.html:97` |
| **Botão “Resolver dia”** | `calendario.resolver_dia_oferecido` lê o status D4 e a condição de último recurso. É o juiz `_TT11` no registro vivo `core/juizes.py`, indicado pelo anexo. A condição consulta a janela atual e os juízes de pergunta **viva** (`_C6`) e **respondida** (`_C21`). O template também exige `pode_veredito`, vindo de `tem_acao(user, 'regularizar_batida')`, o juiz **permissão de tela**. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:117`, `app/colaboradores/services/calendario.py:488`, `app/core/juizes.py:290`, `app/core/juizes.py:305`, `app/core/juizes.py:751`, `app/docs/JUIZES.md:10`, `app/docs/JUIZES.md:51`, `app/docs/JUIZES.md:102`, `app/colaboradores/views.py:1992`, `app/colaboradores/services/drawer.py:133`, `app/templates/colaboradores/partials/_calendario_grade.html:134` |

No card diário do **Espelho**, a faixa de navegação também lê o D4. O selo **EM ABERTO** do card tem seu próprio insumo, `dia.realizado_aberto`, vindo do pareamento em `realizado_do_dia`. O espelho também monta `palavra_dia` com `do_dia`, e o PDF imprime esse campo na coluna de observações. `app/ponto/views.py:161`, `app/ponto/views.py:201`, `app/ponto/services/espelho.py:167`, `app/ponto/services/espelho.py:689`, `app/ponto/services/espelho.py:812`, `app/ponto/services/espelho.py:1327`, `app/templates/ponto/espelho.html:187`, `app/relatorios/pdf_espelho.py:923`

## 2. Onde as respostas divergem?

**O botão usa o status D4. A palavra usa a lista `datas_em_aberto`.** A lista reúne as datas de turno aberto, as de saída órfã e os furos do motor até ontem para quem registra ponto. Dela saem as faltas já decididas e o dia do turno ainda vivo. `veredito_do_dia` usa essa lista depois de testar cobertura, rejeição, folga, feriado e ata sem turno pareado. A presença de uma data na lista, por si só, fica fora do predicado do botão. `app/ponto/services/dia_decidido.py:346`, `app/ponto/services/dia_decidido.py:397`, `app/relatorios/cartao_pela_celula.py:373`, `app/relatorios/cartao_pela_celula.py:395`, `app/relatorios/cartao_pela_celula.py:412`, `app/colaboradores/services/calendario.py:97`

- **E–S–E sem a última saída.** O teste da cópia usa `07:02 E`, `12:59 S`, `13:53 E` e espera um turno aberto. Tomando essa sequência num dia passado, se o turno já expirou pelo relógio, a célula tem `ok` lavrado e nenhum fato de maior precedência toma o lugar da frase, o D4 devolve `ok`, o `t.aberto` mantém a data na lista e a grade dá “Em aberto”, eventualmente com o marco faltante. `ok` fica fora da lista de status do botão. Enquanto o turno ainda está vivo, o D4 dá `aberto`. A sequência E–S–E, por si só, também pode resultar em `irregular` se a nova lavra identificar `FURO_PARCIAL`. Qual lavra produziu o `ok` observado em produção é [não verificado]. `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:131`, `app/ponto/turnos.py:2005`, `app/ponto/status_dia.py:280`, `app/colaboradores/services/calendario.py:607`, `app/ponto/services/dia_decidido.py:241`, `app/colaboradores/services/calendario.py:94`, `app/ponto/supra_juiz.py:429`, `app/escala/models.py:1622`, `app/ponto/status_dia.py:159`
- **`escala_suspeita`.** Exemplo pela regra: dia passado com célula de trabalho, veredito lavrado `indefinida`, sem ausência ou folga vencedora, e uma `S` às 17:00 que o pareador atribui à data como saída órfã. A lavra fornece `escala_suspeita`, a saída órfã põe a data na lista e a palavra sai “Em aberto”. `escala_suspeita` também fica fora da lista do botão. O motivo da escala suspeita e a parcela da lista que entrou nos dias observados são [não verificado]. `app/ponto/status_dia.py:139`, `app/ponto/status_dia.py:286`, `app/ponto/precedencia.py:347`, `app/ponto/precedencia.py:379`, `app/ponto/turnos.py:234`, `app/ponto/services/dia_decidido.py:346`, `app/ponto/services/dia_decidido.py:419`, `app/colaboradores/services/calendario.py:94`

## 3. De onde vêm as 32h?

**São 1.920 minutos do registro `Ausencia` usado no rótulo.** `rotulo_de` lê `ausencia.minutos`, e `palavra_do_dia` converte esse valor em “Falta (desconta 32h)”. Quando uma falta aprovada chega ao `save` com zero minutos, `Ausencia.save` soma a jornada líquida prevista de cada data de `data` a `data_fim`, inclusive. O juiz do previsto é `minutos_previstos_do_dia`, que usa os marcos e as pausas da escala daquele dia. Um valor já preenchido é preservado. `app/ponto/services/dia_decidido.py:162`, `app/ponto/services/dia_decidido.py:254`, `app/ponto/services/dia_decidido.py:533`, `app/ponto/models.py:887`, `app/docs/JUIZES.md:131`, `app/escala/utils.py:2276`, `app/escala/utils.py:2316`

**Sim, uma célula de jornada de 8h pode exibir 32h.** Uma única falta aprovada cobrindo quatro dias de 480 minutos grava 1.920 minutos. O mapa de cobertura devolve o mesmo objeto `Ausencia` nas quatro datas, e o rótulo repete o total **do lançamento** em cada uma. Para a apuração por dia, `minutos_de_falta_por_dia` rateia esse registro pela quantidade de datas da sua expansão. Nesse exemplo são 480 minutos por dia, e o fechamento soma o mapa rateado. Os registros e a origem das 32h dos dias relatados são [não verificado]. `app/ponto/models.py:887`, `app/ponto/turnos.py:2786`, `app/ponto/services/dia_decidido.py:473`, `app/ponto/services/dia_decidido.py:542`, `app/folha/export.py:113`, `app/folha/export.py:138`, `app/ponto/services/fechamento.py:873`

## 4. O `ok` chega a dinheiro?

**O `ok` da tela não credita jornada cheia.** O fechamento carrega batidas apuráveis, monta o motor pela régua da CCT e calcula por fatia de vínculo. O motor usa a célula e o papel que a ata atribui às batidas para o pareamento. Se a última saída continua faltando no pareamento, calcula o que os pares fechados provam, mantém o período aberto e deixa a ponta sem par sem crédito. O teste de `07:02 E / 12:59 S / 13:53 E` espera **357 minutos trabalhados, um turno aberto e zero horas de falta**. `app/ponto/services/fechamento.py:632`, `app/ponto/services/fechamento.py:729`, `app/ponto/services/fechamento.py:754`, `app/ponto/services/fechamento.py:286`, `app/ponto/motor_calculo_v2.py:494`, `app/ponto/motor_calculo_v2.py:2511`, `app/ponto/motor_calculo_v2.py:2541`, `app/ponto/tests/test_bob_dia_pago_se_contradiz.py:131`

- **Fechamento e valor diário.** `FechamentoMensal` recebe as horas e a quantidade de turnos abertos do resultado. `DiaPago` recebe os períodos do mesmo cálculo e soma os minutos trabalhados por dia. O calendário lê as rubricas dessa linha lavrada. `app/ponto/motor_calculo_v2.py:294`, `app/ponto/services/fechamento.py:898`, `app/ponto/services/fechamento.py:1001`, `app/ponto/services/fechamento.py:1200`, `app/ponto/services/dia_pago.py:173`, `app/colaboradores/services/calendario.py:319`, `app/colaboradores/services/calendario.py:688`
- **Export.** A geração do TXT chama o recálculo em modo leitura e passa os objetos de fechamento a `classificar_export` e `montar_linhas_export`. A classificação pode reter pelo **veredito e pelos protestos da célula**, que são insumos distintos da coluna `status_dia`. Também registra `turno_aberto` como trava de aprovação. Para quem entra, o TXT emite as rubricas do fechamento e as datas de falta decidida. `app/docs/JUIZES.md:119`, `app/folha/views.py:209`, `app/folha/views.py:267`, `app/folha/export.py:830`, `app/folha/export.py:968`, `app/folha/export.py:1273`, `app/folha/export.py:1293`, `app/folha/export.py:1301`, `app/folha/export.py:725`
- **Holerite.** O fluxo da aplicação publica e entrega o PDF recebido. O valor efetivamente pago, eventual desconto no salário e o conteúdo do holerite dos dias relatados são [não verificado]. `app/holerite/views.py:1161`, `app/holerite/views.py:1193`, `app/holerite/views.py:1445`

## 5. Quando aparece com pergunta aberta?

A regra vigente exige **permissão `regularizar_batida`**, data **anterior a hoje e ao início da competência atual da empresa**, status em **`falta`, `irregular`, `aberto`, `sem_certidao`** e **nenhuma pergunta simultaneamente viva e respondida naquela data**. Uma pergunta viva **sem resposta** não barra o botão por esse critério. Para ser respondida, precisa de `respondida_em` e texto não vazio após o trim. A consulta primeiro filtra essa dupla em SQL e depois chama `pergunta_viva`; o template exige `pode_veredito and dia.resolver_dia_oferecido`. `app/colaboradores/services/calendario.py:94`, `app/colaboradores/services/calendario.py:117`, `app/colaboradores/services/calendario.py:488`, `app/colaboradores/services/calendario.py:809`, `app/ponto/janelas.py:60`, `app/chamados/juizes.py:1544`, `app/chamados/juizes.py:1575`, `app/templates/colaboradores/partials/_calendario_grade.html:134`

## 6. Há contador ou vigia de status × “Em aberto”?

**Um contador ou alarme que cruze `dia.status` com `datas_em_aberto` por colaborador e data é [não verificado].** O código tem estes contadores relacionados:

- O rodapé do calendário conta os **status D4**, inclusive `aberto`. A pauta e o comando `cartao_x_txt_divergentes` contam **`dias_em_aberto` do resumo**. A divergência medida por esse comando é de rubricas entre cartão e TXT. `app/colaboradores/services/calendario.py:815`, `app/ponto/services/dias_em_aberto.py:112`, `app/pautas/management/commands/pauta_dia_em_aberto.py:98`, `app/relatorios/management/commands/cartao_x_txt_divergentes.py:63`, `app/config/crons.py:1281`
- `vigiar_fio_morto` mede dias sem **porta de ação** nos universos do fio. Ele lê a porta publicada por `tarefa_do_dia`, que consulta a oferta do calendário para dias sem rota elegível. O censo usa veredito acusador e disponibilidade de ação como insumos. `app/config/crons.py:1741`, `app/chamados/services/vigia_fio.py:331`, `app/chamados/services/vigia_fio.py:350`, `app/chamados/services/vigia_fio.py:594`, `app/chamados/services/tarefa_do_dia.py:695`

A leitura foi feita na cópia fornecida. A confirmação independente do hash ficou [não verificado].

## 23/11 11:46 · sessao b621f10444bc · E4C-F5 · verificador · origem=Code

## E4C-F5 · volta 5 · pré-verificação **REPROVADA**

As guardas ainda deixam a correção atravessar de um cartão para outro e a validação inline alcançar uma disputa que perdeu o vínculo com o cartão. O backfill acertou a ordem da aquisição inicial, mas ainda pode travar uma disputa fechada depois de um chamado. `app/chamados/services/acoes_disputa.py:377,398,416`, `app/chamados/views_cobrar.py:160,216`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:200,327`, `app/chamados/models.py:1508,1655`.

Os 22 blobs locais coincidem com os prefixos pós-diff do anexo. A cópia veio sem `.git`, então a identidade do commit completo `b240b2f52e6b9749753d48c14824911f12ee59b8` fica **[nao verificado]**. A verificação foi em leitura, incluindo sondas AST em memória. Suite ORM, SQL medido e efeitos em produção **[nao verificado]**.

## 1. ACHADO — Guardas por porta

`C` = Colaborador, `D` = DisputaSupervisao, `Ch` = ChamadoColaborador, `P` = PerguntaDisputa. A implementação das quatro aquisições está em `app/chamados/models.py:2939,2948,2961,2965`.

| Porta | Trava, guardas e veredito |
|---|---|
| **Reabrir pela supervisão, detalhe e fio** | **NADA nas duas guardas do questionário.** A seleção inicial é feita no detalhe e no fio. O helper trava C/D/Ch, resolve novamente a disputa aberta vinculada e cruza os corrigíveis com o resultado fresco do juiz. A permissão contextual permanece na borda pré-C. A rota de **recobrança** do detalhe segue separada, usa a seleção pré-C para renovar SLA e enviar push e não grava `DecisaoPergunta`. `app/chamados/views.py:1398,1405,1416,1421,1437`, `app/chamados/services/fio.py:612,615,617,708`, `app/chamados/services/acoes_disputa.py:109,110,113,125,135`, `app/chamados/juizes.py:1850`, `app/chamados/services/disputa_emissao.py:1832,1837,1847`. |
| **Reabrir pelo colaborador** | **NADA nas guardas da porta.** Aberta, teto de dois e conjunto corrigível têm recusa prévia e nova leitura após C/D/Ch. O contador só sobe se o lote efetivamente reabrir. `app/chamados/models.py:2454,2459,2469,2473,2475,2477,2481,2484,2487`. |
| **Corrigir resposta pelo admin, detalhe e fio** | **ACHADO.** Formato e motivo vêm do pedido. Validação vigente, D fechada e tipo são relidos após C/D/Ch. O cartão esperado, porém, é calculado pela própria ação após uma segunda consulta pré-C. As bordas conferem A e passam apenas `request` e `resp`. Se a costura mover P de A para B entre a conferência da borda e o `get` da ação, a ação compara B com B, grava a nota e corrige P. A validação subsequente pega outra trava e pode validar uma resposta concorrente, enquanto a mensagem final cita a hora do clique. `app/chamados/views.py:523,528,534,536`, `app/chamados/services/fio.py:696,699,703,872`, `app/chamados/services/acoes_disputa.py:370,377,382,398,413,416,418,428,432,444,456`, `app/chamados/services/validacao.py:733,735`. |
| **Resposta humana, core compartilhado** | **NADA nas guardas internas da RESPOSTA.** Tipo e opções, veto de absorção quando habilitado e C6 quando exigido são lidos após C e a releitura de P. O relógio da resposta também fica dentro. O resgate do chamado é uma etapa posterior, com o problema de ordem descrito na pergunta 2. `app/chamados/services/disputa_emissao.py:1477,1487,1490,1494,1500,1503,1509,1517,1529,1565,1592`. |
| **Validar no fio, aberta ou fechada** | **NADA nas guardas de seleção da pergunta.** Listagem, proprietário e aberta/fechada são refeitos após C. C6 é consultado de novo para a fechada. O validador trava o universo do plantio, reconfere o escopo e lê resposta, validação anterior, espera do par, plausibilidade e entrega antes do carimbo. A permissão contextual sobre Ch é anterior a C. `app/chamados/services/fio.py:612,615,647,649,655,663,666,671,673`, `app/chamados/services/validacao.py:733,735,739,744,749,754,758,763`, `app/chamados/services/materializacao.py:2184,2187,2191`. |
| **Validar inline** | **ACHADO.** A entrada exige D aberta vinculada a Ch, pelo pai ou por uma pergunta-destino do mesmo colaborador. Sob C, a porta relê D aberta pelo PK e confirma P viva e respondida, mas deixa de conferir o vínculo D–Ch. Se a única pergunta que liga D ao cartão A for reapontada para B, uma pergunta ainda viva e respondida de D pode ser validada pelo clique em A. A permissão de área também só roda antes de C. `app/chamados/views_cobrar.py:149,155,157,160,165,170,216,217,218,221`, `app/chamados/services/fio.py:872`. |
| **Validar lote classe A** | **NADA nas guardas do item.** Existência, estado já validado pela composta e classe A são checados na seleção e novamente após C, com as decisões recarregadas. A permissão da borda antecede o lote. `app/chamados/services/validacao.py:1093,1098,1101,1104,1119,1122,1125,1128`, `app/chamados/views_cobrar.py:262,274`. |
| **Upload de foto, RESPOSTA e VALIDAÇÃO** | **NADA na rechecagem do seletor dos dois atos.** C é tomado em cada etapa. D aberta, motivo presente, episódio, PK e via dentro do fato são reavaliados. A porta da resposta relê a voz do colaborador e o fechamento. **ACHADO na cauda.** O contador sobe na primeira seleção bem-sucedida. Se houver retratação antes da segunda, a validação pula P, mas a disputa ainda conta e chega ao aparo e à tentativa de fecho. O aparo lê e grava motivos sem C, risco preexistente colocado na fila pela triagem. `app/api/foto_recebida.py:28,38,64,73,76,83,90,91,114,119,123,125`, `app/chamados/services/disputa_emissao.py:1746,1748,1754`, `app/chamados/models.py:2189,2196`. |
| **Remover motivo / RETRATAÇÃO** | **NADA nas guardas do ato de retratar.** C antecede o relógio, a seleção de P e a aquisição completa. D aberta, motivo presente e P ainda não retratada são lidos após a trava. A decisão de chamar o fecho vem do conjunto restante calculado naquele ato, após sair do `atomic`. O fecho tem nova trava e relê fechamento, respostas sem veredito e C6. `app/chamados/models.py:2253,2257,2259,2262,2264,2267,2285,2287,2297,2303,2097,2123,2130,2138`. |
| **Declarar hora pelo colaborador** | **ACHADO.** Formato, opção e módulo são conferidos na entrada. Após C/D/Ch/P, o código relê Ch para o dia e repete o casamento e a leitura da ata, mas `_juizo` continua capturando o colaborador antigo. Se a admissão for movida para depois de um dia inteiramente faltante antes de C, a grade fresca o excluiria e a instância antiga pode seguir autorizando a resposta. A seleção da pergunta, a validação preexistente e a resposta idêntica são consultadas dentro do ato. `app/chamados/services/declaracao_texto.py:301,303,322,324,330,342,362,368,372,377,389,404,443,445`, `app/escala/services/leitor_celula.py:365,389`, `app/colaboradores/views.py:1336,1361`. |
| **Aplicar declaração em texto** | **ACHADO.** Sob C/D/Ch/P a porta recompõe a proposta no Ch relido e compara followup, motivo, dia e hora. A resposta já preenchida pode ser aproveitada mesmo se divergir da proposta. Além disso, o `atomic` da proposta termina antes da validação, que lê a resposta vigente sem amarrá-la à aceita. Exemplo: aplicar 07:18, entrar uma RESPOSTA de 07:41 no intervalo, e o clique de 07:18 validar 07:41. `app/chamados/services/declaracao_texto.py:676,683,686,689,696,702,721,727,730`, `app/chamados/services/validacao.py:714,733,735,763`. |
| **Absorver irmãs por ausência** | **ACHADO.** C por irmã antecede a releitura da irmã, do grupo, do dia cru do escape, da resposta posterior à passagem, do carimbo e das condições de C6, par e grupo recuperável. Tipo e resposta vigente do escape continuam vindo do ato anterior. A própria ausência e o relógio da passagem antecedem a releitura. Se `FALTA_DIA` for corrigida ou reaberta no mesmo dia depois da RESPOSTA e antes deste C, o código ainda cria a falta da resposta antiga e pode absorver as irmãs. `app/chamados/services/disputa_emissao.py:1565,1568,1593,1596`, `app/chamados/services/materializacao.py:1032,1037,1046,1054,1075,1083,1093,1096,1101,1108,1112,1115`. |
| **Devolver absorvidas por ausência rejeitada** | **NADA para a releitura da condição rejeitada, cobertura, grupo e foto do evento.** São refeitas após a trava completa e a D é recarregada antes de `reabrir_sistema`. Os IDs das D e o motivo textual ainda saem da foto anterior. Uma troca de D de P por escritor de produção ficou **[nao verificado]**. A identidade do grupo continua vulnerável à colisão de instante descrita na pergunta 4. `app/chamados/services/materializacao.py:1225,1227,1237,1243,1249,1253,1258,1262,1264,1269`, `app/chamados/models.py:2398,2408`. |
| **Encerrar órfãs pela lâmpada** | **ACHADO no insumo do juiz.** C por item antecede o filtro de D aberta, motivo, dia, materialização e validação, a resposta composta, a lâmpada, C6 e o teto. O mapa troca o colaborador fresco pelo `colab` recebido antes de C. Uma transferência de empresa entre a seleção e a trava deixa o juiz decidir pela empresa anterior. A cauda de fecho tem o problema de retry da pergunta 7. `app/chamados/reconciliador.py:1140,1177,1179,1181,1183,1235,1238,1240,1243,1254,1265`, `app/chamados/juizes.py:1275,1280`, `app/colaboradores/views.py:1415,1418`. |
| **Backfill do fio morto** | **NADA na nova checagem do plano.** Depois de adquirir o universo e reler C, reconfere competência entregue/indeterminada, replana o dia e recusa limbo e impedimentos antes de executar. Há **ACHADO de ordem** no universo alcançado pela execução, na pergunta 2. `app/chamados/management/commands/backfill_fio_morto_e4b.py:266,273,292,294,315,320,322,327`. |
| **SILÊNCIO do fio do dia** | **NADA quanto ao sítio da nova guarda.** C vem antes da seleção por dia de turno e `conversa=False`. C6 vem após C/D/Ch e antes do carimbo e da linha. Há **ACHADO de sequência** no chamador Resolver dia, que só grava o toque humano depois dessa guarda. `app/ponto/services/veredito_celula.py:176,180,185,193,195,200,352,357,361`. |

## 2. ACHADO — Ordem de trava

- As aquisições explícitas de C/D/Ch/P introduzidas pelo diff passam por `trava_na_ordem`. O atalho `trava_do_colaborador` delega ao protocolo. Cada chamada ordena por PK dentro de C→D→Ch→P. Há travas de outra natureza em caminhos alcançáveis, como o advisory lock do consumidor de órfãs e o da âncora de Pauta. São preexistentes e têm outros objetos. `app/chamados/models.py:2892,2898,2939,2948,2961,2965`, `app/chamados/reconciliador.py:1331,1342`, `app/chamados/services/fecho_runtime.py:86,92,131,133`.
- **Contraexemplo no backfill.** A aquisição inicial cobre D abertas e Ch vivos ou pais das abertas. Um credor terminal que não é pai de D aberta fica fora do conjunto de Ch. A execução reabre esse credor, grava seu status e então ressuscita perguntas. Uma irmã carimbada em D fechada força a primeira aquisição dessa D **depois** do Ch, ainda no `atomic` do lote. O caminho `materializar` também pode pedir uma D fechada depois dos Ch iniciais. `app/chamados/management/commands/backfill_fio_morto_e4b.py:192,200,139,154,291,327`, `app/chamados/services/disputa_emissao.py:1943,1946,1971`, `app/chamados/models.py:2946,2952,2956,302,308,1508,1517,1644,1655`, `app/chamados/services/materializacao.py:2184,2251`.
- **Alcance da pré-trava do resgate.** Na correção admin, o `atomic` da casca mantém Ch preso durante o resgate. Nas entradas avulsas web/API, o core sai do `atomic` que pré-travou Ch antes de chamar `_ressuscitar_por_resposta`. O resgate lê o véu e pode gravar Ch depois dessa liberação. `app/chamados/services/acoes_disputa.py:408,413,432`, `app/chamados/services/disputa_emissao.py:1487,1500,1568,1592`, `app/chamados/services/materializacao.py:1291,1298`, `app/chamados/services/arquivo.py:194,209,210`, `app/chamados/views.py:1278`, `app/api/views.py:2653`.
- **Mudança de conjunto e ordem.** Para os chamadores escalares, `= cid` vira `IN (cid)` com as mesmas condições e ordenação. A única coleção com `True` é o backfill. Para ele, o conjunto lógico inicial continua a união dos universos individuais e a ordem muda de `D1→Ch1→D2→Ch2` para todas as D e depois todos os Ch, globalmente por PK. O lote ainda consulta C duas vezes, de forma reentrante. `app/chamados/models.py:2925,2933,2942,2946,2948,2952,2956,2961`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:195,196,200`.

**22 chamadas de produção com `abertas_do_colaborador=True`.** Linhas da cópia:

- `app/chamados/models.py:302,777,2123,2259`, `app/chamados/reconciliador.py:547,662`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:200`.
- `app/chamados/services/becos.py:53`, `app/chamados/services/declaracao_texto.py:303`, `app/chamados/services/disputa_emissao.py:539,1345,2001`, `app/chamados/services/limbo_folgas.py:275`, `app/chamados/services/materializacao.py:1243,2184`, `app/chamados/services/validacao.py:284,549,581`.
- `app/ponto/services/justificativa.py:41,80,129`, `app/ponto/services/veredito_celula.py:185`.

## 3. ACHADO — C20 com entrada lateral

- **Escritor e vida do registro.** `trava_na_ordem` envia os IDs pedidos depois do `SELECT FOR UPDATE` de C. `_registrar_travados` põe um callback sem efeito na fila da conexão, uma vez por CID registrado. O leitor fica vazio fora de `atomic`. Commit ou rollback externo esvazia a fila. Liberar o savepoint conserva as entradas. Rollback de savepoint de `atomic` tira as entradas daquele escopo e conserva as anteriores. Savepoint **manual** é diferente, pois seu SID não entra na pilha que etiqueta callbacks. O backfill corta explicitamente a fila após a tentativa desfeita. `app/chamados/models.py:2939,2941`, `app/chamados/services/trava_do_colaborador.py:16,30,37,45,54,59,64`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:306,348,352,356`, `app/chamados/tests/test_e1_v13_ordem.py:120,132,135`.
- **`TestCase`.** Os `atomic` internos conservam as entradas na transação externa do teste, que termina em rollback. `fim_do_ato_anterior()` remove apenas sentinelas da fila para simular a troca de ato. As travas reais do banco continuam presas à transação do teste. `app/chamados/tests/test_f5_v3_guarda_sob_trava.py:27,33`, `app/chamados/tests/test_e1_v13_ordem.py:120,135`.
- **Mesmo banco, resultados diferentes.** Com FK em cache carregada com corte 21, banco já no corte 10 e novembro trancado, 14/10 pode dar `False` antes de C e `True` depois de C, sem escrita entre as duas chamadas. A presença de `c.pk` no registro, combinada com `hasattr(..., 'is_cached')`, muda a escolha do insumo. O A6 prova o lado sob C. Em um memo compartilhado, há outro contraexemplo: um par destravado com a FK antiga, seguido de um par travado da mesma empresa, faz o segundo pular a carga. Inverter os pares em memo frio pode mudar o resultado do par travado, com o banco e as travas iguais. `app/core/juizes.py:304`, `app/chamados/juizes.py:1045,1084,1091,1098,1107,1111,1117,1121,1188,1199,1238`, `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:370,373,374,376,377`.
- **A porta pode entregar a Empresa explicitamente.** Sob C, reler **C e Empresa** com `select_related('empresa')`, ou carregar o lote equivalente, e entregar a Empresa relida e um memo da operação ao mesmo C20. O E2 já carrega a relação após C. Isso também resolve o caso em que é o `empresa_id` do colaborador que mudou, pois o `in_bulk` atual só usa o ID recebido. A trava de C, por si, não serializa o escritor do corte de Empresa nem o de `PeriodoFechado`. `app/chamados/reconciliador.py:650,652,666,1238,1240`, `app/chamados/juizes.py:1091,1104,1121,1221`, `app/colaboradores/views.py:518,542,1415,1418`, `app/colaboradores/services/estrutura.py:18`, `app/colaboradores/services/colaborador.py:152,159,160`, `app/ponto/services/fechamento.py:1408,1419,1441`.

## 4. ACHADO — Escritores, leitores e significados

| Campo ou valor | Quem escreve e quem lê |
|---|---|
| **`PerguntaDisputa.materializada_em`** | `_escrever_carimbo` atribui. `carimbar`, `carimbar_identidade` e `descarimbar` persistem pela porta. `descarimbar_em_lote` zera por SQL. `fechar_por_admin` e `encerrar_fantasma` delegam. Leem estado e vigência a composta, C6, a validação/materialização, a lavra, o histórico, a auditoria e o backfill de SILÊNCIO. O homônimo `ItemRegularizacao.materializada_em` tem escritor próprio. `app/chamados/models.py:3333,3347,3387,3463,3493,3501,3519,3523,3602,3615,3623,3714,4457`, `app/chamados/juizes.py:1481,1595`, `app/chamados/services/validacao.py:789,809`, `app/chamados/services/materializacao.py:1080,1096,1187,1234,2238`, `app/chamados/veredito_lavrado.py:203`, `app/chamados/services/historico.py:104`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:359`, `app/chamados/services/regularizacao_ext.py:199,201,220`. |
| **`Batida.retratada_em`** | `retratar_batida` entrega o instante a `anotar`, que persiste. `retratar_humano` usa a mesma porta para zerar. Leem a apuração, o lastro da pergunta, o prazo de desfazer, a vigência da lavra e o recuperador de absorvidas que exclui batida plantada viva. O ato `RETRATACAO` de **P** grava linha e via de identidade próprias. Ele pode preservar o carimbo de materialização anterior. `app/ponto/models.py:67`, `app/ponto/registro_batida.py:264,273,274,291,294`, `app/ponto/services/retratar_humano.py:48,53,75,197,207`, `app/ponto/turnos.py:2897`, `app/chamados/juizes.py:807,862`, `app/chamados/veredito_lavrado.py:180`, `app/chamados/services/materializacao.py:1196`, `app/chamados/models.py:2287,2292,3493`. |
| **`DisputaSupervisao.fechada_em`** | `fechar` data. `reabrir_admin` e `reabrir_sistema` zeram. Leem as portas de ciclo, o universo da trava, seleção de foto, fio, inline, órfãs, emissão e validação. Fora do diff, os sinais, o app, o painel e os relatórios também usam o campo. `app/chamados/models.py:1931,2160,2164,2380,2392,2404,2408,2264,2475,2946`, `app/api/foto_recebida.py:33,120`, `app/chamados/services/fio.py:131,135,649,667`, `app/chamados/views_cobrar.py:161,165,217`, `app/chamados/reconciliador.py:1142,1267`, `app/chamados/services/disputa_emissao.py:541,1754,2223`, `app/chamados/services/validacao.py:320,323`, `app/chamados/signals.py:29,131,155`, `app/api/views.py:1018,1034,2704`, `app/chamados/services/pergunta_no_app.py:88`, `app/colaboradores/services/drawer.py:72`, `app/chamados/relatorios_disputa.py:48`. |
| **`ChamadoColaborador.status_local`** | `abrir` e `criar` escrevem o nascimento, `_gravar_estado` persiste a transição. `_vivas_entre` simula e restaura em memória. Há escritores históricos nas migrações 0038 e 0039. O diff lê o campo para o conjunto `VIVOS` da trava. O catálogo fornece `VIVOS` e a transição. Consumidores incluem C6/lastro, fio, sinais, filas, API, PDF, cartório e veredito. `app/chamados/models.py:64,240,308,375,390,1004,1037,1161,2956`, `app/chamados/migrations/0038_a3_registrado_backfill.py:10,15`, `app/chamados/migrations/0039_a3_via_backfill.py:24,33`, `app/chamados/catalogo/motor.py:24,125`, `app/chamados/juizes.py:835,906`, `app/chamados/services/fio.py:438,452,481,838`, `app/chamados/signals.py:34,186,287`, `app/chamados/services/painel.py:213,214,217`, `app/api/views.py:1037,1135,1139,1145`, `app/relatorios/pdf_chamados.py:116`, `app/ponto/services/cartorio.py:501,1412,1691,1710`, `app/ponto/services/veredito_celula.py:149,155`. |
| **Texto da declaração** | O PWA de declaração tipada envia hora e motivo. A declaração grava `HH:MM` pela porta da RESPOSTA e deixa nota `[Declaracao de horario]`. O texto livre do colaborador vem de PWA/API e persiste na mesma porta de `FollowupChamado.conteudo`. A proposta lê followups do colaborador, extrai uma hora inequívoca e devolve um recorte do texto. O fio mostra texto/hora e envia `followup_id`. Aplicar recompõe a proposta e entrega a hora ao escritor. A nota de correção do admin usa o texto da coluna relida. `app/chamados/views.py:1085,1090,1104,478,480`, `app/api/views.py:975,983`, `app/chamados/services/nota.py:88,103,107`, `app/chamados/services/declaracao_texto.py:29,40,445,449,637,662,676,689,727`, `app/chamados/services/disputa_emissao.py:1530,1565`, `app/chamados/services/fio.py:327,331`, `app/templates/chamados/partials/modal_fio.html:127,143`, `app/chamados/

[truncado em 20000 de 33737 caracteres]

## 23/11 11:42 · sessao 00860978c517 · - · - · origem=Ronald (tmux)

**Q4 — ACHADO.** A volta 5 substitui a igualdade de `materializada_em` por um conjunto de PKs para identificar os carimbos da passagem. O mesmo timestamp ainda identifica o grupo de devolução em `perguntas_absorvidas_por`. Uma irmã com via compatível, mesmo timestamp e sem batida plantada viva pode entrar no grupo sem ato `ABSORCAO`. O escritor já registra os vínculos explícitos do ato. [materializacao.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/materializacao.py:1063) [materializacao.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/materializacao.py:1115) [materializacao.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/materializacao.py:1185)

`data_evento` já guarda dia do turno na entrada de fato e dia civil na entrada de conversa. `SILENCIO` discrimina `conversa=False`. A absorção seleciona por `data_evento` e tipo `hora`, inclusive conversa de dia civil coincidente. A própria prova A9 usa conversa e exige que seja absorvida. [models.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/models.py:3049) [veredito_celula.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/ponto/services/veredito_celula.py:178) [materializacao.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/materializacao.py:1048) [test_f5_v5_guarda_sob_trava.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/tests/test_f5_v5_guarda_sob_trava.py:464)

O censo AST de `status_local` e `fechada_em` passou em memória para as três checagens de produção. `status_local` persiste em `_gravar_estado`, `abrir` e `criar`, e `_vivas_entre` o altera temporariamente. `fechada_em` persiste em `fechar`, `reabrir_admin` e `reabrir_sistema`. Há escritas de migração em `0038_a3_registrado_backfill.py:10,15` e `0039_a3_via_backfill.py:24,33`. [test_selo_status_so_pelas_portas.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/tests/test_selo_status_so_pelas_portas.py:340) [models.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/models.py:240)

**Q5 — ACHADO.** O teste de ordem do resgate passa com a nova trava do core removida, pela trava anterior da própria casca e pela asserção genérica de algum `FOR UPDATE` de `Ch` antes de `UPDATE P`. Os testes A3 substituem o escritor e cobrem a chamada no cenário simulado. A7 simula retratação dentro da transação do teste. [test_f5_v5_guarda_sob_trava.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/tests/test_f5_v5_guarda_sob_trava.py:170) [acoes_disputa.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/acoes_disputa.py:413) [disputa_emissao.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/disputa_emissao.py:1500)

Os quatro mutantes T5 dão erro no AST v5 e são aceitos com os hunks reversos em memória. O v5 ainda aceita a restauração das decisões por `_dec._result_cache.extend(p.decisoes.all())` ou por `_q._prefetched_objects_cache.update(...)` depois de instalar o cache. Q4(b) passa na árvore atual e segue limitado a um nível de chamada pelo nome curto e a `prefetch_related` com string. [test_f5_v5_guarda_sob_trava.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/tests/test_f5_v5_guarda_sob_trava.py:667) [test_e4c_f5_selo_par_ato_coluna.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:405) [test_f5_v4_guarda_sob_trava.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/tests/test_f5_v4_guarda_sob_trava.py:380)

**Q1 complementar — ACHADO.** Se a pergunta mudar de cartão entre o filtro da view e a consulta da ação de corrigir, a ação usa o cartão novo na própria referência e grava. Correção e aplicação da declaração liberam C antes de acionar a validação, que valida a resposta vigente. Uma RESPOSTA de 07:41 entre o commit da aplicação de 07:18 e a nova trava pode ser validada pelo clique anterior. [views.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/views.py:526) [acoes_disputa.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/acoes_disputa.py:377) [acoes_disputa.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/acoes_disputa.py:444) [declaracao_texto.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/declaracao_texto.py:721) [validacao.py](/home/ronald/codex_ws/b240b2f52e6b9749753d48c14824911f12ee59b8/app/chamados/services/validacao.py:733)

## 23/11 11:42 · sessao c25381410738 · - · - · origem=Ronald (tmux)

**1. ACHADO.** As duas etapas do upload conferem motivo, disputa aberta, episódio e via depois de C. O escritor automático relê a voz da pergunta e o fechamento. O contador sobe antes da segunda conferência, de modo que uma retratação entre resposta e validação ainda permite contar e seguir para o aparo do motivo. O aparo pede apenas o estado lido fora de C e grava a disputa sem essa trava. `app/api/foto_recebida.py:64–96`, `app/api/foto_recebida.py:114–129`, `app/chamados/services/disputa_emissao.py:1745–1756`, `app/chamados/models.py:2189–2196`.

**1. ACHADO.** A declaração do colaborador relê o dia e o casamento de marcos depois de C, porém o casamento ainda recebe o colaborador da instância anterior. Se uma edição deslocar a admissão para depois de um dia inteiramente faltante, a grade fresca o excluiria, enquanto a antiga pode autorizar RESPOSTA. A declaração por texto recalcula a proposta sobre chamado fresco, mas libera C antes da validação e o validador julga a resposta vigente sem vincular seu ato à proposta. Uma correção concorrente pode mudar a resposta antes dessa validação. `app/chamados/services/declaracao_texto.py:339–377`, `app/chamados/services/declaracao_texto.py:683–730`, `app/colaboradores/views.py:1333–1365`, `app/escala/services/leitor_celula.py:358–365`, `app/escala/services/leitor_celula.py:385–392`, `app/chamados/services/validacao.py:730–763`.

**1. ACHADO.** A absorção verifica, depois de C, o dia bruto do escape, a irmã, o grupo e o estado da irmã. Continua usando o tipo e a resposta que autorizaram o escape antes da trava. A própria ausência é criada antes dessa releitura. Uma resposta `FALTA_DIA` pode ser substituída no mesmo dia antes de entrar C e a passagem ainda criar a ausência antiga e tentar absorver. Na devolução, status rejeitado, cobertura concorrente, perguntas absorvidas e foto de auditoria são relidos sob C. A lista de disputas ainda deriva de `ps` pré-C. `app/chamados/services/disputa_emissao.py:1562–1568`, `app/chamados/services/disputa_emissao.py:1591–1596`, `app/chamados/services/materializacao.py:1032–1054`, `app/chamados/services/materializacao.py:1074–1118`, `app/chamados/services/materializacao.py:1243–1269`.

**1 e 3. ACHADO.** As órfãs reavaliam seleção, resposta, lâmpada, C6 e teto por item sob C. Entretanto substituem o colaborador relido pelo anterior. Depois de uma transferência entre empresas que comitou antes de C, `dias_trancados` busca pelo `empresa_id` antigo e pode usar a janela da empresa errada. A porta pode carregar `Colaborador` com `select_related('empresa')` depois de C e entregar explicitamente esse insumo ao juiz C20, inclusive para o memo. `app/chamados/reconciliador.py:1140–1146`, `app/chamados/reconciliador.py:1177–1185`, `app/chamados/reconciliador.py:1235–1243`, `app/chamados/juizes.py:1275–1281`, `app/chamados/juizes.py:1074–1125`, `app/colaboradores/views.py:1415–1418`, `app/colaboradores/services/colaborador.py:72–91`, `app/colaboradores/services/colaborador.py:152–161`.

**1 e 2. ACHADO.** O backfill relê entrega, plano e impedimentos depois de C, com as D abertas antes dos Ch do conjunto inicial. Um credor terminal pode não ser pai aberto e estar fora desse Ch inicial. Sua reabertura adquire o Ch e chega a `ressuscitar_perguntas`, que pede a D fechada de uma pergunta carimbada depois do Ch. O teste mede apenas a chamada isolada do helper inicial. `app/chamados/management/commands/backfill_fio_morto_e4b.py:192–200`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:291–327`, `app/chamados/services/disputa_emissao.py:1943–1949`, `app/chamados/services/disputa_emissao.py:1971`, `app/chamados/models.py:1508–1517`, `app/chamados/models.py:1643–1655`, `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:635–656`.

**4 e 5. ACHADO.** A nova comparação de dia bruto na absorção aceita o dia civil das conversas como entrada. Os significados de `data_evento` já coexistiam, discriminados por `conversa`. O RED A5 modifica diretamente uma conversa, embora a porta real de rechavear recuse conversa. A9 prova a colisão artificial de timestamp na ancoragem, enquanto o recuperador continua agrupando por `(disputa_id, materializada_em)`. T5 deixa passar `_q._prefetched_objects_cache.pop('decisoes')` após instalar o cache, pois reconhece alterações por atribuição. Q4(b) deixa passar `Prefetch('perguntas')`, nome importado por alias ou dois helpers em cadeia. `app/chamados/services/materializacao.py:1083–1089`, `app/chamados/services/materializacao.py:1185–1195`, `app/chamados/models.py:3031–3076`, `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:448–475`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:41–47`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:373–414`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:380–406`.

**7. ACHADO.** SILÊNCIO agora usa C6 sob C, porém o próprio Resolver dia só escreve as assinaturas que contam como toque humano depois de chamar o fechamento. Sem toque anterior, o C20 ainda dá trancada naquele momento. A pergunta pode passar a viva depois do log humano, sem SILÊNCIO no ato que decidiu o dia. A asserção nova chama o helper fora do fluxo de toque e não pega esse caso. A declaração com ata que some também ganha a mensagem “Sua declaracao foi registrada”, embora o ramo só emita aviso de operador e retorne antes do primeiro escritor. `app/ponto/services/veredito_celula.py:176–202`, `app/ponto/services/veredito_celula.py:351–363`, `app/chamados/juizes.py:1384–1396`, `app/chamados/juizes.py:1494–1503`, `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:336–355`, `app/chamados/services/declaracao_texto.py:64–73`, `app/chamados/services/declaracao_texto.py:372–386`.

Os 22 blobs locais conferem com os prefixos pós-diff. O hash do commit, execução de suite e efeitos em produção seguem [nao verificado]. [nao verificado]

## 23/11 11:40 · sessao a87007d25862 · - · - · origem=Ronald (tmux)

**2. ACHADO.** As novas aquisições explícitas do diff passam pelo protocolo C→D→Ch→P, por PK em cada grupo. O backfill corrige a ordem no topo, com duas chamadas a C, mas deixa disputas fechadas fora do conjunto. Exemplo reproduzível pela leitura: um credor terminal reabre seu chamado e depois ressuscita uma irmã carimbada em disputa fechada. A primeira trava dessa disputa vem depois da aquisição e do `UPDATE` do chamado. `app/chamados/models.py:2939,2948,2961,2965,1505,1517,1644,1655,1731`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:192,200,291,327`; `app/chamados/services/disputa_emissao.py:1943,1971`.

O conjunto lógico do topo do backfill continua sendo a união dos universos individuais. A ordem passa de D1→Ch1→D2→Ch2 para todas as D antes de todos os Ch. Para os demais chamadores escalares, `= cid` passa a `IN (cid)` com os mesmos filtros e a mesma ordenação. A única coleção com `True` é o backfill. Os 22 sítios de produção são:

- `app/chamados/models.py:302,777,2123,2259`; `app/chamados/reconciliador.py:547,662`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:200`.
- `app/chamados/services/becos.py:53`; `app/chamados/services/declaracao_texto.py:303`; `app/chamados/services/disputa_emissao.py:539,1345,2001`; `app/chamados/services/limbo_folgas.py:275`; `app/chamados/services/materializacao.py:1243,2184`; `app/chamados/services/validacao.py:284,549,581`.
- `app/ponto/services/justificativa.py:41,80,129`; `app/ponto/services/veredito_celula.py:185`.

No core de resposta, o resgate está depois do `atomic` em que Ch é pré-travado. No admin o `atomic` externo mantém a trava. Nas entradas avulsas web/API, o resgate ocorre depois de liberá-la. `app/chamados/services/disputa_emissao.py:1487,1500,1568,1592`; `app/chamados/services/acoes_disputa.py:407,430`; `app/chamados/views.py:1278`; `app/api/views.py:2653`.

**3. ACHADO.** O juiz C20 tem entrada lateral. Com Empresa carregada com corte 21 e o banco já com corte 10 e novembro trancado, 14/10 pode dar `False` antes da trava e `True` depois dela, com banco idêntico. No mesmo lote, se o primeiro par compartilhar o memo e a empresa e vier de outro colaborador destravado com FK velha, o par travado pula a carga e herda o corte velho. Inverter os pares pode mudar o resultado do par travado. A releitura ainda usa `c.empresa_id` antigo após transferência. `app/core/juizes.py:304`; `app/chamados/juizes.py:1045,1084,1091,1098,1107,1111,1125,1188,1199,1238`; `app/chamados/tests/test_f5_v5_guarda_sob_trava.py:370`; `app/colaboradores/views.py:1413,1417`.

O escritor é `trava_na_ordem`, que envia os IDs pedidos após o SELECT de C. O registro é um callback sem efeito. Sai no commit ou rollback externo. Liberar savepoint mantém o callback, e desfazer savepoint de `atomic` elimina os callbacks pendurados nele, conservando as aquisições anteriores. O savepoint manual não publica SID na pilha de `atomic`, daí o corte explícito de callbacks pelo backfill. No `TestCase` o registro das transações internas persiste no `atomic` externo até rollback. O helper de teste simula o commit limpando a fila, sem liberar a trava. `app/chamados/models.py:2939,2941`; `app/chamados/services/trava_do_colaborador.py:16,30,37,45,54`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:306,354`; `app/chamados/tests/test_f5_v3_guarda_sob_trava.py:27`.

A porta pode reler C e Empresa via `select_related('empresa')` depois da trava, ou em lote, e entregar explicitamente Empresa, dia e memo ao mesmo C20. Já há uma carga pós-C no E2. Isso permite escolher a Empresa a partir do vínculo relido e evita que o juiz consulte o registro. `app/chamados/reconciliador.py:650,652,666`; `app/chamados/juizes.py:1121,1221`.

**6. ACHADO.** O SELECT adicional possível é o `in_bulk` das Empresas faltantes, deduplicado dentro da chamada pela combinação de memo e empresa. No E2 a Empresa já chega num `select_related` executado após C, portanto a explicação de que essa FK veio de antes da trava falha nesse caminho. Não há garantia geral de três por transição. Com FK fria a base já carregava a Empresa. Com várias disputas que alcancem C20 na consulta reiniciada dentro do laço S143, podem existir novas cargas por disputa. As três pilhas escritas são compatíveis com chamadas alcançáveis, mas a medida das três queries, suas posições e a folga zero são [nao verificado]. Os dois selos exercitam um colaborador/dia e 1 ou 3 chamados e apenas exigem total menor ou igual ao teto. `app/chamados/juizes.py:1045,1091,1107,1494,1502`; `app/chamados/reconciliador.py:650,652,666`; `app/chamados/services/disputa_emissao.py:2054,2074,2206,2213`; `app/ponto/tests/test_selo_chokepoint_escrita.py:218,254,343,464,491`.

Outros achados enviados à principal incluem vínculo de disputa do inline não relido, cartão conferido pela borda não entregue à ação de correção, e falha de recuperação do fechamento da órfã após persistir o primeiro item e falhar no seguinte. `app/chamados/views_cobrar.py:160,216`; `app/chamados/services/fio.py:695,703`; `app/chamados/services/acoes_disputa.py:398,416`; `app/chamados/reconciliador.py:1141,1236,1263,1495`.

## 23/11 09:16 · sessao 991fd090b45f · E4C-F5 · censo · origem=Code

## Matriz de escritores

O censo AST da cópia encontrou **17 chamadas diretas em 16 funções**, mais o `bulk_create` dentro de `registrar`. A ressuscitação tem duas chamadas. As duas cópias locais estavam sem `.git`, então usei `git diff --no-index` entre os diretórios identificados pelos dois hashes. A classe I abaixo se refere às linhas alteradas nessa comparação. A identidade dos objetos Git e a execução em produção ficam [nao verificado]. `app/chamados/models.py:1681,1737,4537`

C, D, Ch e P indicam as linhas de colaborador, disputa, chamado e pergunta. Na ordem, uma nova aquisição conta mesmo quando é implícita no `UPDATE`. Um `UPDATE` em linha já travada aparece como escrita, sem contar como nova aquisição. `app/chamados/models.py:2896–2915,2932–2959`

| Função e chamada | Guardas e seleção que autorizam a escrita, inclusive na borda | Onde toma C | Reavaliação após C | Ordem de travas e escritas | Relógio anterior a C gravado no ato? |
|---|---|---|---|---|---|
| `ChamadoColaborador.ressuscitar_perguntas`, passivo — `app/chamados/models.py:1681` | Perguntas que o Ch resolve, materializadas, sem resposta pela face SQL, dentro do fato, sem legado de texto desacompanhado de instante e validação, marco apagado e C6 na cópia descarimbada. A borda de `reabrir` exige transição permitida, sem veto do dia, fechamento admin sem fato novo, premissa regularizada, agrupamento ou irmão vivo. Desarquivar exige permissão, área e via de tranca para a segunda passagem. Cartório exige Ch vivo, fio morto com perguntas, sem resposta do colaborador, e pula o ramo de duplicatas. `app/chamados/models.py:1403–1426,1456–1517,1549–1599,1636–1644`; `app/chamados/views_arquivar.py:51–86`; `app/ponto/services/cartorio.py:1690–1721` | `app/chamados/models.py:1655` | **Sim** para vínculo, materialização, resposta, via, legado, marco e C6. Refaz `_universo_da_ressuscitacao`, cruza com os PKs iniciais e consulta as assinaturas e atos que faltam. **Não** para os predicados do Ch e da rota que levaram à chamada. A cerca do lote roda sob C, julgando `alvo_lido`. `app/chamados/models.py:1676–1682,1738–1740,3582–3594,4364–4384` | Localmente C→D→Ch→P→insert. Em `reabrir`, se houver transação externa, a etapa anterior já pode ter travado Ch e relavrado P, e a ressuscitação pode pedir D fechada fora do conjunto anterior de abertas. `app/chamados/models.py:301–325,1505–1517,1643–1655` | **Não.** Preserva instantes históricos da projeção. O relógio implícito de C6 decide elegibilidade, sem virar `ato_em`. `app/chamados/models.py:1599,1681–1682,4373–4384`; `app/chamados/juizes.py:642,1555–1561` |
| `ChamadoColaborador.ressuscitar_perguntas`, `REABERTURA` — `app/chamados/models.py:1737` | Mesmas bordas e universo da linha anterior. A compensação usa o conjunto elegível relido e a sequência de atos para a foto. `app/chamados/models.py:1456–1517,1636–1682,1723–1737`; `app/chamados/views_arquivar.py:51–86`; `app/ponto/services/cartorio.py:1690–1721` | A mesma C de `app/chamados/models.py:1655` | **Sim** para elegibilidade da pergunta e foto composta sob C. **Não** para as guardas externas de reabertura, desarquivamento e cartório. `app/chamados/models.py:1676–1682,1723–1740` | C→D→Ch→P→insert→`UPDATE P`→eventual `UPDATE D` já travada. A ressalva sobre a transação externa é a da linha anterior. `app/chamados/models.py:1655,1737–1749,2395–2408,3593–3595` | **Não.** `now()` sob C, ajustado pelo último ato, inclusive o passivo recém-inserido. `app/chamados/models.py:1720,1732,4512–4522` |
| `DisputaSupervisao.fechar` — `app/chamados/models.py:2152` | Ator, motivo permitido, D aberta, ausência de respostas sem veredito, C6 por P. Só `desligamento` e `admin_encerra` inserem `SILENCIO` para as vivas cujo carimbo mudou. Desligamento seleciona D abertas e pula as que têm resposta sem veredito. `admin_encerra` exige justificativa. Borda produtiva desse wrapper [nao verificado]. `app/chamados/models.py:2076–2116,2145–2154,2171–2178`; `app/colaboradores/services/desligamento.py:164–167`; `app/colaboradores/views.py:1475`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57` | `app/chamados/models.py:2123–2125` | **Sim** para D aberta e execução de `_guardas` novamente. **Em parte** quanto ao frescor de `perguntas`, pois o refresh parcial não limpa esse prefetch explicitamente e a consulta encadeada é feita de novo. Reuso efetivo do cache antigo [nao verificado]. Ator e motivo são argumentos do ato. `app/chamados/models.py:2091–2115,2130–2148` | C→D→Ch, completa Ch dos destinos, depois `UPDATE P`→insert→`UPDATE D` já travada. O carimbo preserva a guarda de identidade da P. `app/chamados/models.py:2123–2154,2160–2164,3327–3341,3483–3496` | **Não.** Usa o `now()` sob C do carimbo, em `materializada_em`. `app/chamados/models.py:2148–2154,3341` |
| `DisputaSupervisao.remover_motivo` — `app/chamados/models.py:2284` | Ator obrigatório. Signal do posto exige registro existente ativo e seleciona D abertas de quem está no posto com o motivo. Signal da escala exige âncora de vazia para preenchida e seleciona D abertas do colaborador com o motivo. A P precisa ter o motivo e ainda não estar retratada. `app/colaboradores/signals.py:49–82`; `app/escala/signals.py:154–182`; `app/chamados/models.py:2243–2245,2282–2283` | `app/chamados/models.py:2254` | **Sim** para D aberta, motivo presente e P elegível. **Não** para o posto atual do colaborador, estado do posto ou transição da âncora que dispararam os signals. `app/chamados/models.py:2255–2266,2282–2284` | C→D→Ch→P→insert→`UPDATE P`→`UPDATE D` já travadas. Se não sobra motivo, chama a porta de fechamento depois do bloco local. `app/chamados/models.py:2254–2258,2284–2304` | **Sim.** `_agora_ret` antes de C vira `RETRATACAO.ato_em` e é oferecido ao carimbo, que pode conservar o anterior. `app/chamados/models.py:2252–2254,2284–2290,3487–3490` |
| `PerguntaDisputa._nascer` — `app/chamados/models.py:3291` | `abrir` exige dia do turno. `_nascer` exige destino do mesmo dono e par de resposta legal. Para inserir, julga episódio, dia, destino e ausência de canônica reutilizável, salvo conversa. Só há ato se a P nova recebe assinatura de resposta ou validação. As chamadas verificadas de emissão, repasse, âncora, ação admin e declarações passam campos de pergunta sem essas assinaturas. `app/chamados/models.py:3043–3069,3106–3112,3134–3198,3278–3291,4373–4384`; `app/chamados/services/disputa_emissao.py:448–455,851–855,1385`; `app/chamados/services/acoes_disputa.py:285`; `app/chamados/services/declaracao_texto.py:390–391,686–687` | `app/chamados/models.py:3113–3117` | **Sim** para a eleição do fato e reuso, feita sob C. **Em parte** para o destino, julgado sob C a partir do objeto recebido, sem rebusca explícita nesse ramo. O par de inserção volta à guarda no `save`. `app/chamados/models.py:3134–3198,3285–3291,3788–3792,3801–3814` | C→`INSERT P`→insert de atos. O ramo de `UPDATE` por reuso retorna antes de `registrar`. `app/chamados/models.py:3117,3278–3291` | **Não para `ato_em`.** Conserva assinaturas recebidas. A conversa lê `localdate()` antes da C local para `P.data_evento`. `app/chamados/models.py:3067–3069,3288–3291,4373–4384` |
| `responder_pergunta_disputa_core` — `app/chamados/services/disputa_emissao.py:1558` | Texto e tipo/opções. Web/API selecionam P do colaborador e exigem C6, com toque para hora na web. Correção admin exige cartão, D aberta, tipo hora, ausência de validação, formato e motivo. Declaração do colaborador exige chamado de batida, dia, hora válida, marco faltante e P sem validação compatível ou resposta igual. Aplicação admin depende de proposta do followup e só responde se falta resposta vigente. `app/chamados/views.py:523–536,1060–1091,1236–1278`; `app/api/views.py:2633–2653`; `app/chamados/services/fio.py:611–623,674–688`; `app/chamados/services/acoes_disputa.py:356–381`; `app/chamados/services/declaracao_texto.py:321–366,387–433,664–714` | Core `app/chamados/services/disputa_emissao.py:1490`. Correção já tomou C em `app/chamados/services/acoes_disputa.py:405`. Declarações em `app/chamados/services/declaracao_texto.py:301–305,362,671`. | **Sim** para tipo/opções, H4 nas entradas que o ligam, C6 na web/API e as três guardas internas da correção. **Não** para o vínculo esperado de dono/cartão da borda e o toque dependente do tipo. **Em parte** para declaração, pois o colaborador repete o casamento com dia antigo e o admin repete a proposta com a mesma instância Ch. Reuso e estado da P são julgados sob C. `app/chamados/services/disputa_emissao.py:1492–1515,1610–1618`; `app/chamados/services/acoes_disputa.py:405–411`; `app/chamados/services/declaracao_texto.py:332–365,387–433,664–714` | Core C→`UPDATE P`→insert. Declarações prefixam C→D→Ch→P. Na correção admin, o `atomic` externo mantém P enquanto o resgate pode pedir Ch. No caminho direto, resgate e absorção vêm depois do bloco local. `app/chamados/services/disputa_emissao.py:1487–1494,1555–1561,1584–1589`; `app/chamados/services/declaracao_texto.py:301–305`; `app/chamados/services/acoes_disputa.py:402–411`; `app/chamados/services/arquivo.py:194–211`; `app/chamados/models.py:495–506` | **Não.** `_tz.now()` sob C serve à coluna e à `RESPOSTA`. O passivo guarda os instantes anteriores. `app/chamados/services/disputa_emissao.py:1515–1525,1558–1560` |
| `reabrir_para_correcao` — `app/chamados/services/disputa_emissao.py:1688` | Ato de tipo permitido, donos do queryset, P dentro do fato com estado a limpar. Detalhe e fio exigem permissão, D aberta vinculada ao Ch e P corrigíveis. Web/API do colaborador exigem D própria aberta, teto de duas e corrigíveis. Recusa admin exige cartão, motivo, resposta vigente, sem validação e dentro do fato. Signal exige ausência rejeitada, sem outra cobertura e grupo absorvido. CLI de limbo exige estoque nominal e `--apply`. `app/chamados/services/disputa_emissao.py:1664–1705`; `app/chamados/views.py:505–518,1317–1329,1388–1432`; `app/api/views.py:2695–2712`; `app/chamados/services/fio.py:611–623,674–693`; `app/chamados/models.py:2448–2459`; `app/chamados/services/validacao.py:901–939`; `app/chamados/services/materializacao.py:1205–1223`; `app/ponto/signals.py:250–279`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:47–71` | Lote `app/chamados/services/disputa_emissao.py:1669–1671`. C externa em `app/chamados/models.py:2464`; `app/chamados/services/acoes_disputa.py:120`; `app/chamados/services/validacao.py:947`; `app/chamados/services/materializacao.py:1228–1230`. | **Sim** para via e estado a limpar, inclusive validação só no ato. Colaborador repete abertura, teto e corrigíveis. Fio repete corrigíveis, **não** D aberta/vinculada. Detalhe **não** repete ambos. Recusa repete guarda interna, **não** cartão e abertura exigida na borda. Signal repete ausência/cobertura/grupo, **não** refresca D usada depois. CLI preserva PKs de corte e retesta `respondida_em` nulo na consulta, **não** o estoque global. `app/chamados/services/disputa_emissao.py:1675–1707`; `app/chamados/models.py:2468–2479`; `app/chamados/services/acoes_disputa.py:117–124`; `app/chamados/views.py:1415–1432`; `app/chamados/services/validacao.py:946–963`; `app/chamados/services/materializacao.py:1233–1249`; `app/chamados/management/commands/curar_perguntas_em_limbo.py:47–71` | Lote C→insert→`UPDATE P`. Colaborador faz `UPDATE D` sem D pré-travada, após P. Recusa faz `UPDATE P` do motivo antes do insert. Signal prefixa C→D→Ch→P, então reabre D já travada quando a instância o permite. `app/chamados/services/disputa_emissao.py:1670,1688–1708`; `app/chamados/models.py:2463–2483`; `app/chamados/services/validacao.py:947–963`; `app/chamados/services/materializacao.py:1228–1246` | **Não.** `now()` do lote sob C, com ajuste para `REABERTURA` e `RECUSA`. `app/chamados/services/disputa_emissao.py:1670–1672,1691`; `app/chamados/models.py:4278,4512–4522` |
| `responder_automaticamente` — `app/chamados/services/disputa_emissao.py:1754` | Nas duas APIs, upload autenticado, foto e batida identificada do dono. Resolvedor escolhe D aberta do colaborador com `foto_ausente` e P do mesmo motivo/episódio. A porta exige par de argumentos legal e ausência de texto. `app/api/views.py:837–863`; `app/api/views_core.py:821–847`; `app/api/foto_recebida.py:28–40,89–95`; `app/chamados/services/disputa_emissao.py:1722–1731` | Upload `app/api/foto_recebida.py:65`, core `app/chamados/services/disputa_emissao.py:1739`. | **Sim** para D aberta, motivo presente, identidade por PK/episódio e texto ausente. **Não** para autorização da batida/upload, feita na API. A condição terminal da P fica fora de ambas as seleções. `app/api/foto_recebida.py:28–40,65–76`; `app/chamados/services/disputa_emissao.py:1741–1749` | C→`UPDATE P`→insert. A validação vem depois do bloco do upload, pela própria porta. O aparo de motivo grava D depois, fora de C nesse caminho. `app/api/foto_recebida.py:61–76,95–105`; `app/chamados/services/disputa_emissao.py:1750–1756`; `app/chamados/models.py:2189–2196` | **Sim.** `now()` do lote, antes de C e do laço, vira `respondida_em` e `RESPOSTA.ato_em`. `app/api/foto_recebida.py:59–73`; `app/chamados/services/disputa_emissao.py:1752–1756` |
| `carimbar_resposta_pela_trilha` — `app/chamados/services/disputa_emissao.py:1788` | CLI `--apply`, estoque nominal, nota mais antiga cujo conteúdo casa pergunta e resposta no Ch pai, texto sem carimbo. `app/chamados/management/commands/curar_perguntas_em_limbo.py:23–34,40–70`; `app/chamados/services/disputa_emissao.py:1769–1770` | `app/chamados/services/disputa_emissao.py:1777` | **Sim** para meio-par e igualdade do texto da resposta. **Não** para estoque global, nota, conteúdo referente à pergunta, Ch pai e `criado_em` da nota. `app/chamados/services/disputa_emissao.py:1779–1788` | C→`UPDATE P`→insert. `app/chamados/services/disputa_emissao.py:1777,1786–1790` | **Não há relógio próprio.** Grava o `criado_em` histórico selecionado antes de C. `app/chamados/management/commands/curar_perguntas_em_limbo.py:68–70`; `app/chamados/services/disputa_emissao.py:1786–1789` |
| `_materializar_ausencia_de_resposta` — `app/chamados/services/materializacao.py:1105` | Escape aceito na resposta humana, P com dia, ausência criada/obtida, irmãs de hora da D nesse dia e sem carimbo. Na escrita, exige carimbo aceito, par ato/coluna concordante e grupo recuperável quando ancorado. `app/chamados/services/disputa_emissao.py:1585–1589`; `app/chamados/services/materializacao.py:1032–1058,1072–1108` | C local por candidata em `app/chamados/services/materializacao.py:1071`. | **Em parte.** Relê cada candidata no recorte de dia/tipo, vigência temporal da resposta, carimbo e irmãs que ancoram. Aplica C6 e confere par/grupo antes do insert. A P de escape, o tipo recebido e o dia que determinou o recorte e a ausência vêm da foto anterior. `app/chamados/services/materializacao.py:1032–1056,1071–1108`; `app/chamados/models.py:3423–3449` | A criação/obtenção da ausência precede a primeira C local. No trecho da candidata, C→`UPDATE P`→insert. `app/chamados/services/materializacao.py:1037–1046,1070–1108`; `app/chamados/models.py:3457` | **Sim.** `agora` antes de C da passagem vira carimbo e `ABSORCAO.ato_em`. `app/chamados/services/materializacao.py:1046,1071,1091,1105` |
| `_carimbar_validacao` — `app/chamados/services/validacao.py:173` | Core seleciona PK e D esperada, principal e possíveis irmãs. Validação normal exige resposta completa ainda sem validação, espera declarado×gravado resolvida, implausibilidade confirmada quando aplicável e K6. Folga exige dia e lei da declaração. Par exige irmã elegível não validada e materialização das duas. Bordas incluem cartão no detalhe/fio, D aberta ou C6 para a fechada no fio, inline por D aberta e vivas respondidas, lotes web/copiloto por `LOTE_A`, CLI `validar_classe_a`, correção, proposta e upload. `app/chamados/services/validacao.py:534–587,665–677,714–763,1093–1115,1215–1251`; `app/chamados/views.py:482–501`; `app/chamados/services/fio.py:644–658`; `app/chamados/views_cobrar.py:157–170,203–210,263,383`; `app/core/views_ia.py:169`; `app/chamados/management/commands/validar_classe_a.py:54–70,88–103`; `app/chamados/services/acoes_disputa.py:420`; `app/chamados/services/declaracao_texto.py:716`; `app/api/foto_recebida.py:76` | Herdada de `app/chamados/services/validacao.py:730–734` → `app/chamados/services/materializacao.py:2164–2165`. | **Sim** para escopo da principal/irmãs, resposta vigente, não validada, demais crivos internos, folga/par. **Não** para cartão esperado, abertura/C6 exigidos na borda, classe A/`LOTE_A`, proposta/hora de referência e episódio/motivo do upload na segunda aquisição. A comparação de escopo inclui os Ch da foto do plantio, sem comparar com o cartão que autorizou a entrada. `app/chamados/services/materializacao.py:2135–2139,2167–2177`; `app/chamados/services/validacao.py:550–584,672–677,734–763`; `app/chamados/services/acoes_disputa.py:21–24` | Plantio C→D→Ch→P→`UPDATE P`→insert. Folga chama subordem adicional depois de o plantio ter travado P, e fechar o fio pode pedir D/Ch de outras perguntas do dia fora da foto do plantio. `app/chamados/services/materializacao.py:2154–2165`; `app/chamados/services/validacao.py:167–175,549–560,581–585`; `app/ponto/services/veredito_celula.py:180–188` | **Não.** `now()` do helper sob C para `VALIDACAO`; passivo histórico. `app/chamados/services/validacao.py:167–175,730–734` |
| `devolver_validacao_recusada` — `app/chamados/services/validacao.py:310` | Ramo de validação HORA que passa pela materialização e volta à verificação K6. A porta exige resposta HORA, sem materialização, motivo de recusa e validação na coluna ou no ato. As guardas de borda da validação são as do caminho anterior. Chamada externa do acervo citado no código [nao verificado]. `app/chamados/services/validacao.py:191–200,243–250,286–301,789–803` | Autocommit entra em `atomic`; a própria porta pede C→D→Ch→P em `app/chamados/services/validacao.py:271–285`. | **Sim** para as guardas próprias, com refresh dos campos e descarte do prefetch de atos. K6 é reconferido no caminho de `validar_pergunta`. **Não** para as autorizações de borda da validação já indicadas. Fechamento de D é relido para autorizar a eventual reabertura. `app/chamados/services/validacao.py:286–301,315–325,796–803` | C→D→Ch→P→`UPDATE P`→insert→eventual `UPDATE D` já travada e cascata ao pai. `app/chamados/services/validacao.py:284–285,307–325`; `app/chamados/models.py:2395–2408` | **Não.** `now()` depois de C, com ajuste de compensação e eventual passivo no lote. `app/chamados/services/validacao.py:303–313`; `app/chamados/models.py:4512–4522` |
| `devolver_paridade_bug_par_103` — `app/chamados/services/validacao.py:387` | CLI `--apply`, autor, filtro opcional de empresas e U103. Porta exige P existente, resposta completa de metade do intervalo/hora, via/carimbo anterior ao corte, marco sem prova de aceso, K6 apurável e não entregue, descarimbo efetivo e trilha. `app/chamados/management/commands/bug_par_103.py:32–67,92–112,130–142`; `app/chamados/services/validacao.py:359–399` | `app/chamados/services/validacao.py:365` | **Sim** para U103, paridade, K6, descarimbo e trilha. **Não** para escopo opcional de empresa do CLI, nem comparação do destino atual com o que foi travado. `app/chamados/services/validacao.py:365–401`; `app/chamados/management/commands/bug_par_103.py:45–54,110–112`; `app/chamados/services/materializacao.py:257–275` | C→D→Ch destino da foto→P→`UPDATE P`→insert. `app/chamados/services/validacao.py:360–388` | **Não.** `now()` sob C para `DEVOLUCAO`; hora do CLI no arquivo de reversão. `app/chamados/services/validacao.py:387–388`; `app/chamados/management/commands/bug_par_103.py:97–106` |
| `devolver_instante_sem_texto_l130` — `app/chamados/services/validacao.py:472` | CLI `--apply`, lista exata do corte e escopo de FKs. Porta exige instante sem texto, sem validação/materialização de coluna, no fato, C6 vivo e K6 apurável não entregue. Descarimbo e trilha precisam ocorrer. `app/chamados/management/commands/esto

[truncado em 20000 de 39807 caracteres]

## 23/11 08:54 · sessao d461044b5ef6 · E4C-F5 · verificador · origem=Code

| Achado do laudo / sítio da v4 | Veredito | Evidência |
|---|---|---|
| **R1a — reabertura pelo colaborador** | **Fecha a guarda apontada.** Reconfere disputa aberta, teto e perguntas corrigíveis sob C. A ordem da escrita ainda falha, conforme achado 2. | `app/chamados/models.py:2463–2483`, `app/chamados/juizes.py:1841–1846` |
| **R1b — reabertura pela supervisão** | **Fecha em parte.** A ação do fio repete o juiz das perguntas sob C. A seleção da disputa aberta segue anterior à trava e a rota `reabrir-disputa/` mantém a seleção antiga até a escrita. | `app/chamados/services/acoes_disputa.py:107–124`, `app/chamados/services/fio.py:617–623,690–693`, `app/chamados/urls.py:20`, `app/chamados/views.py:1404–1432` |
| **R2 — correção de hora pelo admin** | **Fecha em parte.** As três guardas do laudo passam pela mesma função antes e depois de C, sobre uma linha fresca. A autorização pelo chamado que lista a pergunta fica na borda. O novo `atomic` também muda a ordem das travas e o que sobrevive a uma exceção. | `app/chamados/services/acoes_disputa.py:368–411`, `app/chamados/views.py:523–536`, `app/chamados/services/fio.py:674–688`, `app/chamados/services/disputa_emissao.py:1555–1589` |
| **R3a — declaração do colaborador** | **Fecha em parte.** A grade e o casamento da hora são reavaliados após a trava. A segunda avaliação ainda recebe o dia do fato calculado antes dela. | `app/chamados/services/declaracao_texto.py:332–366`, `app/chamados/juizes.py:89–123` |
| **R3b — aplicação da declaração pelo admin** | **Fecha em parte.** Rebusca a proposta por followup, motivo, dia e hora sob a trava. Usa a mesma instância de chamado e libera a trava antes da validação. | `app/chamados/services/declaracao_texto.py:617–651,664–675,707–716` |
| **R4a — absorção** | **Fecha em parte.** Recalcula a ancoragem a partir das irmãs consultadas sob C. A pergunta de escape e o seu dia vêm da foto anterior; a ausência já foi criada ou reaberta quando chega à primeira C da passagem. | `app/chamados/services/disputa_emissao.py:1555–1560,1585–1589`, `app/chamados/services/materializacao.py:1032–1079,1097–1108` |
| **R4b — devolução da ausência rejeitada** | **Fecha em parte.** Relê a ausência, chama o mesmo juiz da cobertura e cruza os PKs com o grupo absorvido atual. A reabertura da disputa usa a instância antiga e a porta de reversão da ausência pode escrever durante a mesma aquisição de C. | `app/chamados/services/materializacao.py:1212–1250`, `app/chamados/models.py:2391–2408`, `app/ponto/services/ausencia.py:969–988,1275–1282,1320–1322` |
| **R5 — resposta automática da foto** | **Fecha em parte.** A retirada simples do motivo antes de C impede a `RESPOSTA`. A seleção ainda aceita a pergunta terminal se o motivo voltar. A `VALIDACAO` acontece em outro ato, com outra aquisição de C. | `app/api/foto_recebida.py:28–40,59–76`, `app/chamados/models.py:2282–2293,2984–2992`, `app/chamados/services/disputa_emissao.py:1741–1756`, `app/chamados/services/validacao.py:730–763` |
| **R5b — carimbo pela trilha** | **Não fecha.** O comando congela o conjunto nominal, a nota escolhida e `criado_em`. Sob C, o escritor confere o par da pergunta e a igualdade do texto. A nota, a pergunta nela citada e o chamado-pai ficam fora da releitura. Execução em produção [nao verificado]. | `app/chamados/management/commands/curar_perguntas_em_limbo.py:26–34,47–71`, `app/chamados/services/disputa_emissao.py:1774–1793` |
| **T2 — memo da tranca e C6, incluindo P8, P9 e S128** | **Fecha em parte.** Uma aquisição nova invalida o memo avulso por instância, o memo do request e os dias da consulta anterior. A estampa acompanha a aquisição de C. A FK de empresa já carregada e mudanças de K7 durante a mesma aquisição continuam escapando, conforme achado 6. | `app/chamados/services/trava_do_colaborador.py:24–28,45–64`, `app/chamados/juizes.py:899–907,939–951,1035–1042,1082–1120,1158–1196`, `app/chamados/models.py:1655–1678`, `app/chamados/services/disputa_emissao.py:1487–1513,2044–2067` |
| **T3 — devolver validação recusada** | **Fecha o caso apontado.** A própria porta abre `atomic` quando entra em autocommit, pede C → D → Ch → P, relê a pergunta e descarta o prefetch de decisões antes da guarda. O teste novo mede D antes do primeiro `UPDATE` de P. | `app/chamados/services/validacao.py:267–325`, `app/chamados/tests/test_f5_v3_guarda_sob_trava.py:97–120` |
| **T4 — recusa de resposta** | **Fecha a guarda interna, já na v3.** A mesma função confere validação vigente, resposta e identidade do fato antes e depois de C. A autorização pelo cartão, nas duas entradas, continua anterior à trava. | `app/chamados/services/validacao.py:920–963`, `app/chamados/views.py:505–518`, `app/chamados/services/fio.py:674–688` |
| **T5 — selo da sonda em cópia** | **Fecha em parte.** Os mutantes novos cobrem reatribuição direta, lista substituída antes da instalação do cache, ramo `if` e chamada direta prematura. Ainda há formas que o selo aceita com o cache velho ou a cópia errada, conforme achado 11. | `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:352–394`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:301–351` |
| **Q2 — `SILENCIO` do fechar e carimbo da absorção** | **Fecha em parte.** O memo aquecido antes de C deixa de servir à nova aquisição nos dois leitores. O fechamento ainda precisa da ressalva de Q4(b) e o carimbo pode reutilizar insumos dentro da mesma aquisição. | `app/chamados/models.py:2091–2109,2123–2154,3418–3457`, `app/chamados/juizes.py:1112–1120,1158–1196` |
| **Q2 — validação normal, folga e par** | **Fecha em parte.** As guardas internas da validação normal rodam após a trava e o refresh. `entrega_que_barra_a_validacao` usa K6, pela exportação vigente. O C6 que **autoriza** validar em disputa fechada está no fio e o core recebe apenas `sem_reabrir`. | `app/chamados/services/validacao.py:215–231,544–584,665–675,730–763,796–803`, `app/chamados/services/materializacao.py:802–841`, `app/chamados/services/fio.py:644–658` |
| **Q2 — demais chamadas que o laudo examinou ou deixou em aberto** | **Sem cura específica na v4.** As devoluções de paridade e L-130 refazem seus predicados sob a trava. O `SILENCIO` do veredito da célula seleciona após C, mas usa a face SQL abrangente como conjunto final. `remover_motivo` relê motivo e fechamento sob C. O nascimento registra no mesmo bloco. O backfill de decisões repete `admin_decidiu_sem_ato`; o backfill de fio morto mantém uma inversão entre colaboradores. | `app/chamados/services/validacao.py:356–389,457–473`, `app/ponto/services/veredito_celula.py:176–197`, `app/chamados/models.py:2252–2293,3113–3118,3285–3291,3689–3695`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:329–361`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:192–198,289–325` |
| **Q4(b) — prefetch de `perguntas` no fechamento** | **Não fecha no código.** O refresh parcial continua sem remover `perguntas` do cache. A leitura efetiva encadeia `.prefetch_related('decisoes')` e o laço usa `vivas` da segunda `_guardas`. Reuso efetivo da lista antiga por esse encadeamento [nao verificado]. O selo novo só varre formas literais no mesmo arquivo. | `app/chamados/models.py:2091–2107,2130–2148`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:362–386`, `app/chamados/services/becos.py:59–61,85–92`, `app/chamados/management/commands/fechar_becos_disputa.py:32–39,52–57` |
| **P3 — relógio da janela viva** | **Fecha a troca UTC → dia local.** O fallback ainda lê a hora dentro do juiz se não receber `hoje`. A ordem do lote fora da janela tem cobertura própria com `hoje` explícito. O total **186** e seu alcance em produção [nao verificado]. | `app/chamados/juizes.py:633–668,931–951,1554–1561`, `app/config/settings/base.py:93–95`, `app/chamados/tests/test_f5_contrato_v2_itens_3_5.py:197–230` |
| **H2 — ato e evento no mesmo `atomic`** | **Preservado no H2 identificado na cópia.** `bulk_create` e `acordar_celulas` continuam no mesmo bloco. O teste injeta falha do outbox e exige rollback dos dois no dublê. A correspondência desse H2 com um achado do laudo [nao verificado]. | `app/chamados/models.py:4534–4552`, `app/chamados/tests/e4_verificar.py:33–37`, `app/chamados/tests/test_e4_expand.py:389–421` |
| **Diff do reconciliador — órfãs pela lâmpada** | **Fecha em parte.** C passa a anteceder a seleção e o veredito. O novo `atomic` muda o alcance do rollback e o veredito e o carimbo ainda podem usar instantes diferentes. | `app/chamados/reconciliador.py:1173–1185,1221–1258`, `app/chamados/models.py:3418–3423,3609–3617` |
| **Diff dos apoios, mutante e contadores** | O mutante de tranca acompanha o mesmo retorno. C12 desloca os dois endereços das mesmas chamadas. O contador 65 → 66 corresponde ao novo `assert` dos dois nós de apoio. Os outros hunks carregam helpers ou adaptam dublês. O teste sem banco do upload substitui o validador e cobre a seleção do episódio. | `app/chamados/tests/e3_mutantes.py:66`, `app/chamados/tests/e4b_c12_ast.py:26–29`, `app/chamados/models.py:1793,1813`, `app/chamados/tests/test_e4b_c4v7_memoria.py:117–123`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:22,28–34`, `app/chamados/tests/e3_c8b_apoio.py:124`, `app/chamados/tests/e3_lu3_apoio.py:47`, `app/chamados/tests/test_cef_c1_sem_banco.py:233–238`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:166`, `app/chamados/tests/test_e4_expand.py:112,121`, `app/chamados/tests/test_e4b_c4_tranca_memoria.py:43`, `app/chamados/tests/test_e4b_classe_e_memoria.py:481` |

## ACHADOS NOVOS

1. **Bloqueia — uma rota de reabertura ainda passa PKs congelados ao escritor.** A rota de detalhe autoriza com `_disputa_do_chamado` e `perguntas_corrigiveis` antes de C. O lote só toma C depois de receber o queryset de PKs. No fio, a v4 cruza os corrigíveis, mas também precisa reconfirmar a disputa selecionada e aberta. O juiz dos corrigíveis filtra por vínculo e estado da pergunta. `app/chamados/urls.py:20`, `app/chamados/views.py:1404–1432`, `app/chamados/services/fio.py:120–137,617–623`, `app/chamados/services/acoes_disputa.py:117–124`, `app/chamados/juizes.py:1841–1846`, `app/chamados/services/disputa_emissao.py:1666–1675`.

2. **Bloqueia — ordem C → P → D/Ch.** Na reabertura pelo colaborador, o lote atualiza P e depois a porta atualiza o contador em D, que ficou fora do conjunto inicial de travas. Na correção administrativa, o novo `atomic` conserva a trava do `UPDATE` de P enquanto o core pode desarquivar um chamado. `arquivo.levantar` então pede Ch dentro da transação. `app/chamados/models.py:2463–2483,495–506`, `app/chamados/services/disputa_emissao.py:1688–1708,1555–1585`, `app/chamados/services/acoes_disputa.py:402–411`, `app/chamados/services/materializacao.py:1270–1282`, `app/chamados/services/arquivo.py:194–211`.

3. **Bloqueia — a autorização pelo cartão e pela fila pode vencer antes da gravação.** Correção, recusa e validação leem `chamado_que_lista` na entrada. O destino pode ser reapontado sob C e o chamado esperado fica fora da chamada ao escritor. Na disputa fechada, o C6 da borda também fica fora. O inline usa um conjunto anterior de vivas e o lote exige `LOTE_A` antes de chamar a validação, que sob C aplica outro crivo. `app/chamados/views.py:482–536`, `app/chamados/services/fio.py:97–117,644–688,933–942,978–981`, `app/chamados/views_cobrar.py:160–170,203–210`, `app/chamados/services/validacao.py:730–763,946–963,1093–1115,1215–1251`.

4. **Bloqueia — declaração relida com insumos antigos e validação de outra resposta.** O vínculo de célula e o contexto do chamado têm portas de escrita. R3a congela o dia antes delas poderem interpor-se; R3b pergunta novamente à mesma instância de chamado. Além disso, `aplicar_declaracao` aceita uma resposta vigente já existente sem comparar sua hora com a proposta e chama `validar_pergunta` depois de liberar C. A validação consome a resposta então vigente. **Fila:** perder a ata durante R3a devolve `nao_casa`; o mesmo estado na triagem devolve `indeterminado` e acende o aviso. `app/chamados/services/vinculos.py:93–101`, `app/chamados/services/contexto.py:82–104`, `app/chamados/services/declaracao_texto.py:332–366,618–629,664–675,707–716`, `app/chamados/services/validacao.py:730–741,763`.

5. **Bloqueia — absorção e devolução ainda podem escrever contra um fato vencido.** A resposta de escape sai do bloco do core antes da passagem. A passagem usa `p.data_evento` para criar a ausência antes de C, embora o dia da pergunta possa ser rechaveado sob C. Na devolução, `_reabrir` da ausência altera o status sem a C das perguntas. Outra janela está no próprio fechamento da disputa: D pode fechar entre as duas leituras, e `reabrir_sistema` recebe a D antiga de `ps`, vê `fechada_em=None` e retorna. Nesse caso, as perguntas são limpas e a disputa fica fechada. A trilha de `antes` também usa `_foto` anterior à trava. `app/chamados/services/disputa_emissao.py:1555–1560,1585–1589`, `app/chamados/services/fio.py:1059–1066,1084–1096`, `app/chamados/services/materializacao.py:1032–1079,1214–1250`, `app/ponto/services/ausencia.py:969–988,1320–1322`, `app/ponto/signals.py:250–272`, `app/chamados/models.py:2391–2408`.

6. **Bloqueia — C6 pode usar insumo antigo na mesma aquisição ou via FK já carregada.** `dias_trancados` pode repovoar um memo frio com a instância de `Empresa` que a triagem carregou. O dia de corte dessa empresa tem escritor próprio. Além disso, trancar ou reabrir `PeriodoFechado` grava antes da cascata e sem tomar C. O laço das órfãs mantém uma só estampa para todos os itens e pode conservar o resultado de K7 da primeira pergunta para as seguintes, após a mudança de período. `app/chamados/juizes.py:1045–1051,1082–1120`, `app/colaboradores/views.py:514–542`, `app/colaboradores/services/estrutura.py:18–21`, `app/ponto/janelas.py:36–63`, `app/ponto/services/fechamento.py:1400–1428,1432–1470`, `app/chamados/reconciliador.py:1233–1248`.

7. **Bloqueia — upload pode escrever sobre pergunta retratada ou perder motivo novo.** Se `foto_ausente` volta à D depois de uma retratação, o filtro por motivo, episódio e PK ainda aceita a pergunta velha. A emissão exclui a velha da identidade do fato; o escritor automático confere texto e fechamento. Se a retratação entrar entre a `RESPOSTA` e a validação, o texto permanece e a validação pode registrar `VALIDACAO` na pergunta terminal. A cauda do upload ainda chama `aparar_motivo_respondido` fora de C, e essa porta grava a lista e o contexto da instância, podendo sobrescrever uma agregação concorrente. `app/api/foto_recebida.py:38–40,61–76,97–105`, `app/chamados/models.py:2189–2196,2282–2293,2984–2992,4386–4392`, `app/chamados/services/disputa_emissao.py:326–337,538–551,1741–1756`, `app/chamados/services/validacao.py:730–763,789–806`.

8. **Bloqueia — a nova fronteira muda o que fica gravado em caso de exceção.** Em R2, `horario_corrigido` é anexada antes da nova guarda e segue dizendo que houve correção mesmo quando a guarda recusa. Se o resgate do chamado levantar depois do savepoint do core, agora voltam a resposta, o ato, o outbox e a trilha de banco do core. A nota prévia fica e o log administrativo posterior fica sem execução. Nas órfãs, uma exceção propagada em item posterior agora desfaz os carimbos e eventos dos itens anteriores do mesmo colaborador. A v4 fornecida acrescenta blocos transacionais e mantém os `except` de produção desses sítios. `app/chamados/services/acoes_disputa.py:382–426`, `app/chamados/services/disputa_emissao.py:1555–1607`, `app/chamados/models.py:4535–4551`, `app/chamados/reconciliador.py:1233–1277,1474–1496`, `app/core/observ.py:130–138`.

9. **Bloqueia — `materializada_em` passa a servir como identidade desta passagem.** O filtro novo trata igualdade de timestamp como prova de que esta passagem fez o carimbo. Outras portas escrevem o mesmo campo e podem produzir o mesmo instante. A ligação específica do ato já existe em `absorcao_escape` e `absorcao_ausencia`. **Fila:** o upload incrementa o contador por disputa percorrida, inclusive após a releitura pular todas as perguntas, e as duas APIs publicam esse valor como `disputas_fechadas`. `app/chamados/services/materializacao.py:1073–1076,1105–1108,1159–1184`, `app/chamados/models.py:3381–3385,3418–3423,3473–3495,4289–4293`, `app/api/foto_recebida.py:67–68,95–109`, `app/api/views.py:860–863`, `app/api/views_core.py:844–847`.

10. **Bloqueia — relógio do processamento da foto fica antes da trava.** O upload cria o próprio `agora` antes da seleção e o escritor o conserva no ato. Se uma `REABERTURA` posterior a esse instante limpar a resposta antes de o upload obter C, a nova `RESPOSTA` pode entrar com timestamp anterior à reabertura. A leitura por `(ato_em, pk)` apaga o ato e a coluna fica preenchida. **Fila de relógio:** a janela lê `localdate()` no meio de C6 sem `hoje`; P8, P9 e o reconciliador têm chamadas sem instante explícito. O reconciliador lê seu `agora` antes de C e o carimbo pode ler outro `now()`. A `RETRATACAO` de `remover_motivo` também captura `now()` antes de C. `app/api/foto_recebida.py:59–73`, `app/chamados/services/disputa_emissao.py:1741–1756,1510–1524`, `app/chamados/models.py:1595–1599,2252–2258,2284–2290,3418,4422–4451`, `app/chamados/juizes.py:642,939–951,1555–1561`, `app/chamados/reconciliador.py:1173–1185,1221–1236`.

11. **Fila — predicados duplicados e furos nos selos.** O fechamento normal recompõe C7 em `_guardas`, além de `pode_fechar`, que está registrado em `JUIZES.md`. A expressão da janela viva aparece também invertida em `expira_em`. No T5, a análise em memória aceitou uma chamada via alias antes do cache, a substituição de `_result_cache` depois de instalar o cache, a troca por subscript de `['decisoes']` e um bloco dentro de `for` vazio. Em Q4(b), o teste passou, mas a varredura literal encontrou a chamada de `becos.py`; a entrada do comando que chama esse helper ficou fora da detecção. A allowlist de vitalidade acompanha a mesma guarda R2, com chave pelo nome curto `_recusa`, que também dispensaria outro escopo homônimo no mesmo arquivo. `app/docs/JUIZES.md:63–66,97–105,120–121`, `app/chamados/models.py:2047–2053,2091–2115`, `app/chamados/juizes.py:662–668`, `app/chamados/veredito_lavrado.py:119–147`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:92–94,361–394`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:362–386`, `app/chamados/services/becos.py:61,87`, `app/chamados/management/commands/fechar_becos_disputa.py:33,57`, `app/chamados/tests/test_contract_leitores_vitalidade.py:91,311–318,337–354`.

12. **Bloqueia — dois sítios do censo continuam com desvios fora do diff.** No backfill de fio morto com mais de um colaborador, depois de todas as C o laço adquire D e Ch por colaborador, o que permite Ch₁ → D₂. O veredito da célula trata `q_vivas()` como conjunto final para `SILENCIO`, embora a face SQL deliberadamente inclua perguntas que o C6 considera mortas, inclusive por tranca. `app/chamados/management/commands/backfill_fio_morto_e4b.py:192–198,289–290`, `app/chamados/models.py:2936–2956,3327–3341,3689–3695`, `app/ponto/services/veredito_celula.py:176–197`.

## REDs da v4

Nos testes com `_interposto`, o ato entra na mesma conexão na primeira chamada da trava, antes de delegar à trava real. `fim_do_ato_anterior` só retira os callbacks do registro. A sequência dos testes é verificável no código. Concorrência com duas transações e commits reais [nao verificado]. `app/chamados/tests/test_f5_v3_guarda_sob_trava.py:27–45`.

| Teste | Interposição e o que a asserção mede | [nao verificado]
|---|---|
| **P3** | Congela 01h UTC e verifica o booleano da janela para 01/09. Sem interposição ou escrita. `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:28–36`. |
| **R1a** | O `UPDATE` de materialização entra após `_corrig` fixar os PKs e antes do lote. Mede materialização preservada e contador de reaberturas zerado. A existência de `REABERTURA` fica fora da asserção. `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:22–25,51–60`, `app/chamados/models.py:2456–2464`. |
| **R1b** | A mes

[truncado em 20000 de 29534 caracteres]

## 23/11 08:53 · sessao 2b8e8a94bbda · - · - · origem=Ronald (tmux)

O censo AST encontrou 17 chamadas ordinárias a `registrar`. O `bulk_create` está em `app/chamados/models.py:4535-4537`. O selo exclui migrações, e a migração histórica de `resposta_mantem_validacao` grava em `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:14`. Produção atual [nao verificado].

| Achado | Veredito | Evidência |
|---|---|---|
| R1 | Fecha em parte | A rota de supervisão ainda congela o juiz antes do lote. `app/chamados/urls.py:20`, `app/chamados/views.py:1404-1432`. A reabertura pelo colaborador também grava P antes de D no contador. `app/chamados/models.py:2463-2483`. |
| R2 | Fecha as três guardas e cria regressão transacional | `app/chamados/services/acoes_disputa.py:368-379,402-411` relê as guardas. O novo atomic prende P enquanto o resgate pega Ch. Se o resgate levantar, a resposta, o ato, o outbox e o log voltam, e a nota prévia permanece. `app/chamados/services/acoes_disputa.py:383-392`, `app/chamados/services/disputa_emissao.py:1555-1585`, `app/chamados/services/materializacao.py:1270-1282`, `app/chamados/services/arquivo.py:194-211`, `app/chamados/models.py:495-506`. |
| R5 | Fecha em parte | O predicado é relido antes da RESPOSTA. A VALIDACAO vem após a soltura de C e permite resposta já retratada. Aparar motivos também escreve sem C. `app/api/foto_recebida.py:61-76,97-105`, `app/chamados/services/validacao.py:730-763`, `app/chamados/models.py:2189-2196,2282-2293,4386-4392`. |
| R5b | Não fecha | O comando escolhe o conjunto e a nota pré-C, e a porta relê só o meio-par e o texto. `app/chamados/management/commands/curar_perguntas_em_limbo.py:26-34,47-71`, `app/chamados/services/disputa_emissao.py:1774-1793`. É cura nominal sob demanda conforme `app/config/crons.py:1475-1476`. Alcance em produção [nao verificado]. |
| T2, P8, P9 | Fecha a contaminação da aquisição anterior nos ramos exercidos | `app/chamados/juizes.py:899-908,1162-1196`, `app/chamados/models.py:1655-1678`, `app/chamados/services/disputa_emissao.py:1487-1513`. K7 muda sem C antes da cascata, e a mesma aquisição pode preservar memo anterior a essa mudança. `app/ponto/services/fechamento.py:1408-1448`, `app/chamados/juizes.py:1112-1120`. |
| T3 | Fecha o caminho direto | A função cria atomic se necessário e pega D/Ch/P antes da guarda e escrita. `app/chamados/services/validacao.py:271-310,315-325`. O sinal S88 retorna ao reabrir, pois `fechada_em` está nulo. `app/chamados/signals.py:19-30`. |
| T4 | Já fechado para as três guardas locais | `app/chamados/services/validacao.py:920-963`. A autorização `chamado_que_lista` continua na borda. `app/chamados/views.py:509-518`, `app/chamados/services/fio.py:674-688`. |
| T5 | Fecha em parte | Dois mutantes foram aceitos por `_censo` executado em memória: sobrescrever `_result_cache` depois de montar o cache e substituir por subscript a chave `'decisoes'` antes do juiz. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:370-394,465-469`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:306-317,336-341`. |
| Q2 validar | Fecha em parte | `entrega_que_barra_a_validacao` é K6, via `app/chamados/services/validacao.py:215-231` e `app/chamados/services/materializacao.py:825-842`. A borda de disputa fechada ainda lê C6, fechamento e listagem sem repeti-los sob C. `app/chamados/services/fio.py:644-658`, `app/chamados/services/validacao.py:730-763,801-803`. |
| Q4(b) | Não há cura no diff | A testemunha AST só cobre coincidência lexical no mesmo arquivo. O código atual encadeia `.prefetch_related('decisoes')` depois de `.all()`, detalhe ausente da hipótese do laudo. Reprodução de lista velha por prefetch simples [nao verificado]. `app/chamados/models.py:2097,2130-2148`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:362-386`. |
| P3 | Fecha a troca para dia local | `app/chamados/juizes.py:642`, `app/config/settings/base.py:93-95`. A leitura quando `hoje=None` continua dentro de C6 e seu memo continua chaveado por `None` nesse caminho. `app/chamados/juizes.py:743,843,939-951,1555`. |
| H2 | Ato/outbox seguem no mesmo atomic | `app/chamados/models.py:4535-4551`, `app/chamados/tests/e4_verificar.py:33-37`, `app/chamados/tests/test_e4_expand.py:389-421`. Identidade de H2 no anexo [nao verificado]. |

Outros pontos de bloqueio no censo: lote web/chat só verifica LOTE_A antes de C, e a validação sob C usa a régua diferente de implausibilidade (`app/chamados/services/validacao.py:1093-1115,1215-1251,749-752`). O backfill E4b pega Ch do primeiro colaborador antes de D do segundo (`app/chamados/management/commands/backfill_fio_morto_e4b.py:192-198`). `remover_motivo` lê o relógio do próprio ato antes de C (`app/chamados/models.py:2252-2258,2284-2290`). O veredito da célula autoriza SILENCIO pelo SQL declaradamente abrangente, sem C6 exato (`app/ponto/services/veredito_celula.py:176-197`, `app/chamados/models.py:3327-3341,3689-3695`).

Em R4b, a nova leitura não serializa com `mudar_estado` ou `criar_ausencia`, e a reabertura usa `Disputa` da foto velha. `app/chamados/services/materializacao.py:1217-1249`, `app/ponto/services/ausencia.py:549-562,955-988`, `app/ponto/signals.py:250-272`, `app/chamados/models.py:2391-2408`. Em R4a, `materializada_em` serve como instante de corte e identidade da passagem. `app/chamados/models.py:3381-3385`, `app/chamados/juizes.py:1469-1481`, `app/chamados/services/materializacao.py:1073-1076,1159-1182`.

No reconciliador, o novo atomic reúne os carimbos e logs do colaborador. Falha propagada em uma órfã posterior desfaz carimbos e logs anteriores da rodada. A admissão pela lâmpada passa para depois de C, mas recebe o relógio lido antes de C. `app/chamados/reconciliador.py:1221-1258,1489-1496`.

## 23/11 08:52 · sessao 06be39427f4c · - · - · origem=Ronald (tmux)

### Vereditos

| Achado | Veredito | Evidência |
|---|---|---|
| T2 / Q2, memo da tranca | **Fecha em parte.** A estampa separa triagem da aquisição de C no memo do request, do colaborador, da consulta e do C6. Preserva a `Empresa` já cacheada na instância e reutiliza `PeriodoFechado` na mesma aquisição. | `app/chamados/models.py:2932–2935`, `app/chamados/services/trava_do_colaborador.py:24–28,45–64`, `app/chamados/juizes.py:907,939–951,1045–1051,1091–1120,1158–1196` |
| Q2, `entrega_que_barra_a_validacao` | **O memo da tranca não é insumo dessa guarda.** Delega ao K6, que consulta `competencia_entregue` e só aplica a metades de intervalo. O painel pergunta C6 antes da trava para validar em disputa fechada, enquanto o núcleo só usa `sem_reabrir` na devolução. | `app/chamados/services/validacao.py:215–231,730–763,801–803`, `app/chamados/services/materializacao.py:808–841`, `app/chamados/services/fio.py:649–658` |
| P3 | **Fecha em parte.** Corrige o fuso do fallback. C6 ainda pode ler `localdate()` dentro do juiz, inclusive na sonda da ressuscitação, na resposta humana e nas órfãs, que já receberam outro relógio para o teto. A janela ainda usa o corte padrão quando não recebe empresa. | `app/chamados/juizes.py:633–668,1554–1575`, `app/config/settings/base.py:93–95`, `app/chamados/models.py:1557–1559,1595–1599,1720`, `app/chamados/services/disputa_emissao.py:1510–1524,2067–2072`, `app/chamados/reconciliador.py:1173–1185`, `app/ponto/janelas.py:36–51,60–72` |
| H2 | A definição de H2 neste laudo está **[nao verificado]**. O homônimo da cópia prova atomicidade entre ato e outbox, inclusive falha injetada, e não mede efeitos colaterais do novo atomic da correção. | `app/chamados/tests/test_e4_expand.py:389–421` |

### Achados adicionais

1. **Bloqueia — atomic novo muda a ordem e a persistência da correção R2.** A `RESPOSTA` e o `UPDATE` de P ocorrem no atomic interno, depois o núcleo pode desarquivar Ch, passando pela trava de Ch com P ainda presa. Antes a trava interna terminava antes do desarquivamento e a porta de campo pulava a trava em autocommit. Uma exceção propagada nessa fase agora desfaz resposta e logs do core. A nota anterior à trava sobrevive à exceção e também à recusa sob C. O `except` do followup e os outros `except` nesses ramos já existiam no diff fornecido. `app/chamados/services/acoes_disputa.py:382–392,402–426`, `app/chamados/services/disputa_emissao.py:1555–1607`, `app/chamados/services/materializacao.py:1270–1282`, `app/chamados/services/arquivo.py:205–210`, `app/chamados/models.py:495–506`.

2. **Bloqueia — R2 conserva autorização da tela lida antes de C.** As duas rotas exigem que a pergunta esteja listada no chamado. O serviço relê validação, fechamento e tipo, sem receber o chamado da tela ou retornar à guarda da listagem. A costura pode mudar o destino. `app/chamados/views.py:523–536`, `app/chamados/services/fio.py:97–117,669–688,857–867,960–978`, `app/chamados/services/acoes_disputa.py:368–392,405–410`.

3. **Bloqueia — R4b usa a foto velha da disputa para reabri-la.** Sob C, a releitura de absorvidas entrega só PKs. O laço chama `reabrir_sistema` nas disputas dos objetos antigos. Uma disputa aberta na foto e fechada na espera fica fechada, enquanto a reabertura de P grava. O evento também preserva os valores lidos antes da trava como `antes`. `app/chamados/services/materializacao.py:1214–1223,1234–1250`, `app/chamados/models.py:2391–2408`.

4. **Bloqueia — status da ausência e tranca têm escritores fora de C.** Reverter rejeição chama `mudar_estado`, que grava o status sem C. O consumidor que adquire C só roda no `on_commit`. O mesmo padrão ocorre no período, gravado antes do gancho da fila. Portanto a leitura sob C e a mesma estampa não excluem alteração depois da leitura, inclusive no laço de órfãs. A troca do corte também pode continuar invisível por cache de FK da Empresa. `app/ponto/services/ausencia.py:969–988,1275–1282,1320–1322`, `app/ponto/signals.py:250–272`, `app/ponto/services/fechamento.py:1400–1428,1432–1448,1464–1470`, `app/chamados/reconciliador.py:1233–1248`, `app/chamados/juizes.py:1045–1051,1091–1120`, `app/colaboradores/views.py:514–542`, `app/colaboradores/services/estrutura.py:18–21`.

5. **Bloqueia — R5 valida após perder a proteção do episódio.** A retirada do motivo entre o commit da RESPOSTA e a validação conserva o texto, registra RETRATACAO e põe `VIA_MOTIVO_RETRATADO`. A validação lê a resposta, não veda a via terminal e grava antes de decidir se precisa materializar. Se o motivo voltar, a seleção da foto também pode incluir pergunta anteriormente retratada no mesmo episódio. `app/api/foto_recebida.py:38–40,59–76`, `app/chamados/models.py:2274–2295,3886–3890`, `app/chamados/services/disputa_emissao.py:1741–1756`, `app/chamados/services/validacao.py:161–175,730–763,789–806`.

6. **Bloqueia — R3 e R4a relêem usando identidades antigas.** R3a fixa `data_iso` antes da trava. R3b passa o mesmo `ChamadoColaborador` sem refresh ao juiz, que lê célula e contexto da instância. R4a cria a ausência antes de C e continua usando `p.data_evento` e `p.tipo_resposta` antigos. A data pode ser regravada por `rechavear_data_evento`. `app/chamados/services/declaracao_texto.py:290–305,332–365,618–629,671–675`, `app/chamados/juizes.py:89–123`, `app/chamados/services/vinculos.py:93–101`, `app/chamados/services/contexto.py:82–104`, `app/chamados/services/materializacao.py:1032–1079`, `app/chamados/services/fio.py:1059–1066,1084–1096`.

7. **Bloqueia — R1 conserva lacunas de ordem e de guarda.** O colaborador grava P e depois atualiza o contador de D tendo adquirido somente C. Na supervisão, a entrada exige disputa aberta, mas a releitura sob C não pergunta se D continuou aberta. `app/chamados/models.py:2463–2483`, `app/chamados/services/disputa_emissao.py:1688–1707`, `app/chamados/services/fio.py:120–137,617–623,690–693`, `app/chamados/services/acoes_disputa.py:117–124`, `app/chamados/juizes.py:1582–1586,1841–1846`.

8. **Fila — órfãs passam a reverter o lote inteiro.** Uma falha tardia desfaz carimbos e eventos de iterações anteriores, que no antes consolidavam no atomic de cada pergunta. O consumidor captura a exceção e incrementa tentativas. `app/chamados/reconciliador.py:1233–1258,1489–1496`.

9. **Fila — campos com dois usos.** `materializada_em` é o relógio do carimbo e passa a servir de identidade da passagem corrente, embora a linha ABSORCAO tenha FKs próprias. `disputas_fechadas` nas APIs recebe a contagem de disputas selecionadas/tratadas, inclusive quando a releitura recusa todas as perguntas. `app/chamados/services/materializacao.py:1073–1076,1105–1108,1152–1185`, `app/api/foto_recebida.py:67–68,87,95–109`, `app/api/views.py:860–863`, `app/api/views_core.py:844–847`.

10. **Fila — predicados preexistentes copiados.** C7 normal compõe a ausência de resposta sem veredito e C6 morto tanto em `pode_fechar` como no núcleo de `fechar`. O calendário de expiração recompõe os limites da janela viva. `app/chamados/models.py:2047–2053,2091–2115`, `app/chamados/veredito_lavrado.py:119–147`, `app/chamados/juizes.py:633–668`.

## 23/11 08:52 · sessao d92384af5776 · - · - · origem=Ronald (tmux)

- **R1 fecha em parte.** A rota de supervisão efetiva ainda congela os IDs antes da trava e chama a porta de reabertura sem reavaliar o juiz. O RED do serviço não atravessa essa rota. O serviço do fio relê os IDs, mas não a guarda de disputa aberta e associada ao chamado. `app/chamados/urls.py:20`, `app/chamados/views.py:1404`, `app/chamados/views.py:1415`, `app/chamados/views.py:1432`, `app/chamados/services/fio.py:120`, `app/chamados/services/fio.py:617`, `app/chamados/services/acoes_disputa.py:120`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:62`.
- **R2 fecha as três guardas nomeadas.** A associação da pergunta ao chamado continua só antes da trava nas duas entradas. A nota do contraditório precede a nova recusa e pode afirmar que o admin corrigiu uma resposta que não foi alterada. O atomic novo também inclui a pós-escrita do core. Exceção propagada em `_ressuscitar_por_resposta` passa a reverter resposta, ato e log do core, preservando a nota prévia. `app/chamados/views.py:523`, `app/chamados/services/fio.py:674`, `app/chamados/services/fio.py:978`, `app/chamados/services/acoes_disputa.py:368`, `app/chamados/services/acoes_disputa.py:389`, `app/chamados/services/acoes_disputa.py:402`, `app/chamados/services/disputa_emissao.py:1555`, `app/chamados/services/disputa_emissao.py:1575`, `app/chamados/services/disputa_emissao.py:1585`.
- **R3 fecha em parte.** As consultas aos marcos rodam depois da trava, mas R3a congela `data_iso` e R3b reutiliza `ch` sem refresh. O dia e a FK têm escritores identificáveis. Perder a ata no meio gera `nao_casa` em R3a, embora a falta da ata na primeira leitura gere `indeterminado`. R3b valida após sair do atomic. `app/chamados/services/declaracao_texto.py:332`, `app/chamados/services/declaracao_texto.py:342`, `app/chamados/services/declaracao_texto.py:365`, `app/chamados/services/declaracao_texto.py:618`, `app/chamados/services/declaracao_texto.py:675`, `app/chamados/services/declaracao_texto.py:716`, `app/chamados/juizes.py:89`, `app/chamados/management/commands/carimbar_por_batida_coincidente.py:97`, `app/chamados/rejuncao.py:107`.
- **R4 fecha em parte.** A fonte da ausência e o tipo da pergunta original vêm da instância anterior à trava. O efeito da resposta ocorre após o atomic que gravou RESPOSTA. Timestamp do carimbo é usado como identidade da passagem, embora outras portas gravem o mesmo campo e ABSORCAO já tenha os dois vínculos. R4b relê o status com C, mas a transição de Ausência pode escrever sem esse C. `app/chamados/services/disputa_emissao.py:1558`, `app/chamados/services/disputa_emissao.py:1589`, `app/chamados/services/materializacao.py:1032`, `app/chamados/services/materializacao.py:1051`, `app/chamados/services/materializacao.py:1071`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1234`, `app/chamados/models.py:3383`, `app/ponto/services/ausencia.py:975`, `app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1281`, `app/ponto/services/ausencia.py:1320`.
- **R5 fecha a remoção isolada.** Retratar e reemitir o mesmo motivo deixa a linha antiga com o mesmo episódio, e o filtro relido aceita essa linha terminal. O core automático não verifica terminalidade. A validação e a aparagem do motivo continuam fora do atomic do novo predicado. `app/chamados/models.py:2282`, `app/chamados/models.py:2984`, `app/chamados/services/disputa_emissao.py:326`, `app/chamados/services/disputa_emissao.py:538`, `app/chamados/services/disputa_emissao.py:1741`, `app/api/foto_recebida.py:38`, `app/api/foto_recebida.py:65`, `app/api/foto_recebida.py:73`, `app/api/foto_recebida.py:102`, `app/chamados/models.py:2189`.
- **Q4(b) não fecha por alteração do código.** O refresh parcial não ganhou pop. O laço real usa `vivas` da segunda `_guardas`, que lê `self.perguntas.all().prefetch_related('decisoes')`. Stale efetivo dessa composição sem Django [nao verificado]. O teste estático só vê literal posicional e `.fechar` no mesmo arquivo. Encontra apenas `becos.py:61` e não testa `Prefetch`, wrapper `admin_encerra` ou chamada cruzada do comando. `app/chamados/models.py:2097`, `app/chamados/models.py:2130`, `app/chamados/models.py:2138`, `app/chamados/models.py:2146`, `app/chamados/models.py:2178`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:362`, `app/chamados/services/becos.py:61`, `app/chamados/management/commands/fechar_becos_disputa.py:33`, `app/chamados/management/commands/fechar_becos_disputa.py:57`.
- **T5 fecha em parte.** Os cinco testes existentes passam com o AST real extraído, sem banco. O mesmo censo aceita alias usado antes do cache, sobrescrita do conteúdo/cache após o cache inicial e antes do juiz e cache/lista dentro de laço que nunca roda. Aceita também a cópia condicional com original no outro caminho. `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:321`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:327`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:330`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:336`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:343`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:350`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:92`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:361`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:373`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:387`.
- **REDs de escrita.** R1a/R1b intercalam o UPDATE após o juiz eager e medem carimbo, R1a também contador. R2 intercala o próprio escritor de VALIDAÇÃO e mede resposta e validação composta. R3a/R3b intercalam mudança de lista simulada e medem coluna, não ato. R4a intercala depois da foto, mas o TestCase já reteve C do core que registrou a resposta-escape, e mede o carimbo e ato. R4b1 mede zero e carimbo, R4b2 só preservação da resposta nova. R5 mede só texto e intercala antes do C da resposta, sem exercer a fase posterior de validação. `_interposto` simula ordem na mesma conexão. `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:51`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:62`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:82`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:127`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:137`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:189`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:85`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:125`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:222`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:233`, `app/chamados/tests/test_f5_v4_guarda_sob_trava.py:273`, `app/chamados/tests/test_f5_v3_guarda_sob_trava.py:27`.
- **Selos e contadores.** `_recusa` migra a mesma guarda, com chave ampla por nome simples de função. O AST real identifica a linha nova `materializada_em__isnull` de R4a como permitida pela exceção já existente, e zero ofensores sem permissão. C12 conserva os dois escritores e o tamanho nove. Contagem de 66 asserts confere, com o novo assert dos dois nós de suporte. Mutante da consulta ainda altera o retorno dos mesmos dias. `app/chamados/tests/test_contract_leitores_vitalidade.py:91`, `app/chamados/tests/test_contract_leitores_vitalidade.py:135`, `app/chamados/tests/test_contract_leitores_vitalidade.py:311`, `app/chamados/tests/test_contract_leitores_vitalidade.py:352`, `app/chamados/services/materializacao.py:1076`, `app/chamados/tests/e4b_c12_ast.py:26`, `app/chamados/models.py:1793`, `app/chamados/models.py:1813`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:22`, `app/chamados/tests/test_e4b_c4v7_memoria.py:122`, `app/chamados/tests/e3_mutantes.py:66`.
- **H2, efeitos do atomic.** Universo e veredito passam a rodar sob C. Relógio do teto continua anterior a C, C6 é chamado sem o instante e o carimbo lê outro `now`. Se o segundo item falhar, o novo atomic desfaz o carimbo e a trilha do primeiro e o consumidor mantém o evento pendente. `app/chamados/reconciliador.py:1181`, `app/chamados/reconciliador.py:1221`, `app/chamados/reconciliador.py:1233`, `app/chamados/reconciliador.py:1247`, `app/chamados/reconciliador.py:1489`, `app/chamados/models.py:3418`, `app/chamados/models.py:3617`, `app/core/observ.py:133`.

## 23/11 08:03 · sessao c2f9772266f4 · E4D-CONTRATO · leitura · origem=Code

# E4D-CONTRATO

Inventário estático da cópia fornecida. A árvore não contém `.git`, então a correspondência com o HEAD `68eb689b54d98029243071e7943fd79cc8e718ca` fica [nao verificado]. Os resultados do ensaio vêm do `laudo.txt` transcrito.

## 1. Entradas do contrato

| Item e trecho do anexo | Campo e porta na cópia | Pergunta que ficou aberta |
|---|---|---|
| **Linha física de validação** — “todo escritor que carimba validar grava a linha VALIDACAO fisica”. `backlog_e4d.txt:2016` | `validada_em` e `validada_por`. `_carimbar_validacao` grava o ato. `carimbar(resposta='validar')` pode preencher os dois campos. `app/chamados/services/validacao.py:161–175`, `app/chamados/models.py:3324–3348` | Como representar a validação que nasce junto de `ABSORCAO`, dado que o ensaio preservou as absorvidas sem `VALIDACAO`? `backlog_e4d.txt:2087`, `laudo.txt:8–11` |
| **Ato de conclusão** — “choice de ato de conclusao” e “backfill marcado backfill com data do carimbo”. `backlog_e4d.txt:2085` | `materializada_em`, `materializada_por` e `via_resolucao`. O plantio chama `carimbar` e a enumeração atual de atos não contém conclusão. `app/chamados/services/materializacao.py:1981–1986`, `app/chamados/models.py:3293–3297,4152–4164` | Quais vias e autores entram no ato de conclusão, inclusive os carimbos de reconciliador e identidade? Como o lote identifica as exportadas para o `!`? `backlog_e4d.txt:2085`, `app/chamados/reconciliador.py:407,645`, `app/chamados/models.py:3364–3386` |
| **Um escritor, texto e raiz** — “unificar o TEXTO da resposta no ato”, “placar [...] le a projecao” e “DECIDE x GRAVA”. `backlog_e4d.txt:2086`, `backlog_mencoes.txt:2235` | `resposta_colab` e `respondida_em` são gravados com `RESPOSTA.ato_texto/ato_em`. O raio-x lê as duas colunas e `resposta_display` já reduz a sequência com fallback de legado. A disputa ainda decide e grava em `fechar` e `remover_motivo`. `app/chamados/services/disputa_emissao.py:1517–1534`, `app/chamados/services/raiox.py:135–147`, `app/chamados/models.py:2070–2099,2188–2225,3811–3818,4308–4313` | Qual é a autoridade do texto residual nas faces SQL e de exibição? Quem decide e quem persiste nascimento, filhos, par, fecho, reabertura, tranca e desligamento? `backlog_e4d.txt:2086`, `backlog_mencoes.txt:2235` |
| **D3 / L-130** — “ato SANEAMENTO generico [...] as 2 elegiveis [...] 109 entregues ficam”. `backlog_e4d.txt:2086` | `devolver_instante_sem_texto_l130` zera `respondida_em` e `resposta_colab` pela porta de descarimbo. Pode inserir a foto legada. O ato `SANEAMENTO` não consta dos choices. `app/chamados/services/validacao.py:428–446`, `app/chamados/models.py:3485–3487,4152–4164` | A escolha do ato, os dois elegíveis e o tratamento das 109 entregues estão respondidos. Qual campo identifica a lei no `SANEAMENTO` e qual efeito o redutor atribui a esse ato? `backlog_e4d.txt:2086`, `app/chamados/models.py:4272–4278,4314–4326` |
| **Ensaio para o contrato** — “2 latentes + 5 sem ato”, “emite ou nao?” e “promover a ABSORCAO = contrato”. `backlog_e4d.txt:2087`, `laudo.txt:38–43` | `validar` no plantio e no laço com destino, `responder` no laço sem credor, `ABSORCAO` nas irmãs, limpeza L-130, `reabrir_para_correcao` e FK `validada_por`. `app/chamados/services/materializacao.py:1055–1064,1981–1986`, `app/chamados/services/disputa_emissao.py:1629–1656,2029–2033,2100–2104`, `app/chamados/services/validacao.py:436–440`, `app/chamados/models.py:3631–3636` | O lote emite evento? Como promover `ABSORCAO` e dar cobertura aos carimbos, à reabertura de borda e ao `SET_NULL`? O anexo já responde que as 5.389 sem `ABSORCAO` ganham `VALIDACAO` e que as ~47 com `ABSORCAO` são puladas. `backlog_e4d.txt:2087`, `laudo.txt:8–11,26–33,38–43` |
| **Ato futuro** — “ato de fato pode nascer > agora?” e “brecha save sem pk”. `backlog_e4d.txt:2088` | `DecisaoPergunta.save()` barra instância com PK, e o `registrar` insere os valores recebidos. A tarefa corta atos em `agora`. A leitura usada por `resposta_display` reduz a sequência inteira. `app/chamados/models.py:3811–3818,4227–4233,4308–4326,4329–4347`, `app/chamados/services/tarefa_do_dia.py:390–427` | Pode nascer fato futuro? Qual horizonte vale para juiz, tarefa e modal, inclusive em inserção sem PK? O zero do ensaio mede `ato_em > agora + 1 min`. `backlog_e4d.txt:2088`, `laudo.txt:15` |
| **Pureza da constante** — “deixa de ler a classe” e “9 harnesses [...] mudam”. `backlog_e4d.txt:2089` | A constante de módulo já existe. A classe a reexporta, e o juiz ainda usa a classe para preservar a via de identidade ao cortar um carimbo futuro. `app/chamados/models.py:2580,2701`, `app/chamados/juizes.py:1382–1400` | A fonte da constante e a retirada da isenção já estão escritas. Não achei pergunta de lei adicional nessa entrada. `backlog_e4d.txt:2089` |
| **Corridas registradas** — “recusa entre irmas”, “responder_automaticamente em empate” e “elegibilidade antes da trava”. `backlog_e4d.txt:2098` | `materializada_em` agrupa a absorção por um relógio comum, com atomic por irmã. A resposta automática recebe o relógio do lote. Validar, recusar e corrigir leem elegibilidade antes das respectivas escritas. `app/chamados/services/materializacao.py:1028–1065,1121–1134`, `app/api/foto_recebida.py:39–51`, `app/chamados/services/validacao.py:677–685,729–743,894–926`, `app/chamados/services/acoes_disputa.py:344–384` | Qual é a unidade de decisão quando a recusa entra entre irmãs? Qual resultado vale no empate e quando a elegibilidade muda entre a leitura e a trava? `backlog_e4d.txt:2098` |
| **Âncora fora contada** — “CONTADA so na trilha [...] sem_grupo_recuperavel”. `backlog_e4d.txt:2099`, `backlog_mencoes.txt:2005` | O absorvedor devolve `recusadas`, registra `irma_viva_na_ausencia` e o chamador descarta o retorno. O grupo atual procura a âncora no texto de escape e no carimbo. `app/chamados/services/materializacao.py:1065–1071,1083–1089,1121–1134`, `app/chamados/services/disputa_emissao.py:1560–1563` | A contagem só na trilha está escrita. Como identificar grupo e dependente quando a âncora ficou fora e o grupo passa a ser lido por `ABSORCAO`? `backlog_e4d.txt:2099` |
| **H4** — “fantasma [...] e encerrar_por_fato_no_espelho” e “descarimbo sem REABERTURA/RECUSA”. `backlog_e4d.txt:2100` | Fantasma grava o carimbo e a via em `atomic` sem trava do dono nesse ramo. Encerrar por fato troca só a via. O descarimbo sem lastro limpa o trio e grava trilha de auditoria. `app/chamados/reconciliador.py:1185–1206`, `app/chamados/models.py:3399–3409,3489–3508`, `app/chamados/services/relavra_via.py:230–252` | Que trava cobre os escritores e que ato representa o descarimbo que deve alterar a vigência lida por H4? `backlog_e4d.txt:2100` |
| **Item 93** — “escolha sem ancora; ancora retirada”. `backlog_e4d.txt:2101` | A rejeição chama `perguntas_absorvidas_por`. A busca parte de perguntas `hora` e exige escape para formar o grupo. O retorno vazio impede chegar à reabertura. `app/ponto/signals.py:261–262`, `app/chamados/services/materializacao.py:1118–1134,1154–1185` | Qual vínculo autoriza reabrir a irmã de hora quando a origem foi escolha ou a âncora saiu? Como tratar o legado que não tem vínculo físico recuperável? `backlog_e4d.txt:2087,2101` |
| **Foto de lavra e instante de inserção** — “carimbo pela trilha FORA (morre com a E4d)” e “nao e portao de nada”. `backlog_mencoes.txt:2090,2139` | `marcos_do_lote` agrega `materializada_em` e `respondida_em`. A linha de `DecisaoPergunta` guarda `ato_em`, sem campo de inserção. `app/chamados/veredito_lavrado.py:151,203–208`, `app/chamados/models.py:4166–4179` | Que evidência preserva a foto lavrada quando a linha é inserida depois com `ato_em` anterior e o carimbo pela trilha FORA sai? A regra bitemporal já está registrada. `backlog_mencoes.txt:2090,2139` |
| **Exceções A03/A05/A06/A17/A18/A20** — “ate existir, a projecao segue fonte lida”. `backlog_mencoes.txt:2217–2222` | Reabertura admin na disputa, validação com assinatura, identidade de duplicata, pendência e recusa técnica na materialização, conferência de data. `app/chamados/models.py:2289–2324`, `app/chamados/services/validacao.py:161–175,994–998`, `app/chamados/services/fio.py:1488–1505`, `app/chamados/services/materializacao.py:1360–1366,1683–1703` | As exceções e a lei do par conferido estão escritas. Falta a fronteira por coluna e leitor no corte. O ramo exato que o anexo chama de A05 fica [nao verificado]. `backlog_mencoes.txt:2217–2222` |

## 2. Escritores das oito colunas

**Sim** indica linha de `DecisaoPergunta` da mesma pergunta no mesmo `atomic`. O tipo do ato importa. Por exemplo, `ABSORCAO` acompanha o carimbo inteiro e não é uma linha física de `VALIDACAO`. `app/chamados/services/materializacao.py:1055–1064`

| Coluna | Escritor e gravação de `DecisaoPergunta` |
|---|---|
| `validada_em`, `validada_por` | `_carimbar_validacao`, chamado por folga, par e validação comum — **sim**, `VALIDACAO`. `app/chamados/services/validacao.py:168–175,519,551,641,743` |
| `validada_em`, `validada_por` | `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` — **sim**, `DEVOLUCAO` quando há escrita. `app/chamados/services/validacao.py:269–282,331–358` |
| `validada_em`, `validada_por` | `responder_pergunta_disputa_core` limpa validação anterior — **sim**, `RESPOSTA`. `app/chamados/services/disputa_emissao.py:1484–1496,1517–1534` |
| `validada_em`, `validada_por` | `ressuscitar_perguntas` e `reabrir_para_correcao` limpam pelo lote — **sim**, `REABERTURA` ou `RECUSA`, conforme a porta. No segundo, o `UPDATE` alcança também a pendente já vazia, que fica sem linha. `app/chamados/models.py:1627–1655,1694–1703,3485–3487`, `app/chamados/services/disputa_emissao.py:1617,1629–1656` |
| `validada_em`, `validada_por` | `carimbar('validar')` preenche se há texto e falta validação. **Sim**, `ABSORCAO` no absorvedor. **Não**, linha nova para o carimbo no plantio e no laço do chamado-destino. A validação de entrada pode pertencer à transação externa do plantio. `app/chamados/models.py:3324–3348`, `app/chamados/services/materializacao.py:1055–1064,1981–1986`, `app/chamados/services/disputa_emissao.py:2029–2033`, `app/chamados/services/validacao.py:729–743,766–782` |
| `validada_por` | Exclusão do usuário via FK `SET_NULL` — **não** cria ato. `DecisaoPergunta.ato_por` e `validada_original_por` também têm `SET_NULL`. `app/chamados/models.py:3631–3636,4169–4175` |
| `respondida_em`, `resposta_colab` | Resposta humana, inclusive correção admin por delegação, e automática de foto — **sim**, `RESPOSTA`. `app/chamados/services/disputa_emissao.py:1481–1483,1517–1534,1682–1688`, `app/chamados/services/acoes_disputa.py:374–384`, `app/api/foto_recebida.py:39–51` |
| `respondida_em` | `carimbar_resposta_pela_trilha` preenche o instante do texto existente — **sim**, `RESPOSTA`. `app/chamados/services/disputa_emissao.py:1701–1710`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:68` |
| `respondida_em` | `carimbar` preenche quando há texto. **Sim**, `ABSORCAO` no absorvedor. **Não**, ato novo no plantio nem nos dois laços do resolvedor, com destino (`validar`) e sem credor (`responder`). `app/chamados/models.py:3324–3348`, `app/chamados/services/materializacao.py:1055–1064,1981–1986`, `app/chamados/services/disputa_emissao.py:2029–2033,2100–2104` |
| `respondida_em`, `resposta_colab` | Ressuscitação, reabertura e recusa limpam pelo lote — **sim** para a linha que perde estado, com a borda da pendente já vazia acima. L-130 — **só foto legada se faltava**, sem ato compensador da limpeza. `app/chamados/models.py:1694–1703`, `app/chamados/services/disputa_emissao.py:1639–1656`, `app/chamados/services/validacao.py:436–440` |
| `recusa_motivo` | Escape que exige admin — **sim**, `RESPOSTA`. Recusa do admin — **sim** se a reabertura encontra estado para retirar e a via admite a escrita, `RECUSA`. O motivo é salvo antes dessa seleção, no mesmo `atomic`. `app/chamados/services/disputa_emissao.py:1500–1534,1639–1656`, `app/chamados/services/validacao.py:916–926` |
| `recusa_motivo` | Pendência técnica do plantio, `_carimbar_recusa` simples ou com colisão, e retratação de duplicata — **não** gravam ato próprio. `app/chamados/services/materializacao.py:1360–1366,1683–1707`, `app/chamados/services/fio.py:1479–1484,1502–1505`, `app/chamados/models.py:3383–3386` |
| `materializada_em`, `via_resolucao` | `carimbar`, `carimbar_identidade`, `descarimbar` e `descarimbar_em_lote` são as portas compartilhadas. Os chamadores e seus atos estão na tabela abaixo. `app/chamados/models.py:3293–3348,3364–3386,3399–3409,3485–3487` |
| `tipo_resposta` | Nasce em `_nascer`, vindo da emissão normal/conversa, marcos faltantes, âncora ausente, ação de disputa e declaração em texto. **Não** há ato para o tipo. `app/chamados/models.py:2629,3206–3211`, `app/chamados/services/disputa_emissao.py:448–461,851–859,1385–1392`, `app/chamados/services/acoes_disputa.py:267–272`, `app/chamados/services/declaracao_texto.py:377,667` |
| Todas, no nascimento | `_nascer` persiste `**campos` e os defaults em `force_insert`. **Condicional**, no mesmo `atomic` insere foto `RESPOSTA`/`VALIDACAO` se nasce com os respectivos instantes. O helper não gera ato próprio para tipo, via, conclusão ou motivo. `app/chamados/models.py:2629–2636,2747–2754,3012–3033,3206–3211,4251–4270` |

| `materializada_em` / `via_resolucao` por chamador | Mesmo `atomic` |
|---|---|
| Absorvedor da ausência | **Sim**, `ABSORCAO` com escape e ausência. `app/chamados/services/materializacao.py:1055–1064` |
| Fecho da disputa por admin/desligamento e fecho do fio do dia pelo admin | **Sim**, `SILENCIO`. `app/chamados/models.py:2069–2089`, `app/ponto/services/veredito_celula.py:172–197` |
| Aceite de folga, dia já declarado ou declaração nova | **Sim**, `VALIDACAO` para a mesma pergunta no mesmo `atomic`. O `fechar_por_admin` não grava ato próprio de conclusão. `app/chamados/services/validacao.py:511–527,545–560`, `app/chamados/models.py:3246–3260` |
| `remover_motivo`, via `motivo_retratado` | **Sim**, `RETRATACAO`. Preserva o instante se já havia carimbo. `app/chamados/models.py:2187–2194,2217–2225,3378–3386` |
| Resposta nova desmonta carimbo sem lastro. Devolução BUG-PAR-103 limpa validação e carimbo | **Sim**, `RESPOSTA` ou `DEVOLUCAO`, respectivamente. `app/chamados/models.py:3514–3528`, `app/chamados/services/disputa_emissao.py:1499,1517–1534`, `app/chamados/services/validacao.py:331–358` |
| Ressuscitação e `reabrir_para_correcao`, inclusive via recusa, ausência rejeitada e reparo de limbo | **Sim**, `REABERTURA`/`RECUSA` para estado retirado. **Não** para pendente já vazia. `app/chamados/models.py:1694–1703`, `app/chamados/services/disputa_emissao.py:1639–1656`, `app/chamados/services/validacao.py:925–926`, `app/chamados/services/materializacao.py:1173–1181`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:70` |
| Plantio, paridade recusada, fato ocupado, S144 `juiz_aprovou` | **Não**, ato próprio desses carimbos. `app/chamados/services/materializacao.py:1474,1693–1703,1981–1986,2180–2182` |
| S143, laço com destino e laço sem credor | **Não**, ato próprio do carimbo. O segundo deixa auditoria `fecho_sem_credor`. `app/chamados/services/disputa_emissao.py:1911–1919,2029–2033,2100–2113` |
| Reconciliador, `sem_turno_aberto`, `celula_absolveu` e `fantasma` | **Não**. Fantasma deixa auditoria. `app/chamados/reconciliador.py:399–415,634–646,1197–1206` |
| Duplicata | **Não**, ato próprio da duplicata. O comando de backfill pode registrar um `SILENCIO` anterior e separado para uma linha elegível no mesmo lote. `app/chamados/services/fio.py:1479–1484,1502–1505`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:340–363` |
| Reabertura de competência, desfazer carimbo sem lastro e L-130 | **Não**, ato compensador do descarimbo. L-130 pode inserir a foto anterior no mesmo `atomic`. `app/chamados/models.py:3530–3536`, `app/chamados/services/competencia_trancada.py:275–283`, `app/chamados/services/relavra_via.py:236–252`, `app/chamados/services/validacao.py:436–446` |
| Só `via_resolucao`, re-lavra e encerramento por fato no espelho | **Não**. A re-lavra também salva a foto do veredito. `app/chamados/services/relavra_via.py:117–132`, `app/chamados/models.py:3489–3497`, `app/chamados/management/commands/encerrar_perguntas_com_fato.py:67–77` |

## 3. Leitores que ainda chegam à coluna

- **Tela/template.** `_pergunta_card` usa `validada_em` para estado, data e ações, e `respondida_em` para a data da fala. `app/templates/chamados/partials/_pergunta_card.html:20–42`
- **Tela/template.** Modal usa texto, instante de resposta, motivo de recusa, presença da validação e tipo. `app/templates/chamados/partials/modal_fio.html:219–228,327,345`
- **Tela/template.** Atendimento usa instante, texto como fallback, tipo, opções selecionadas, via e motivo. A prévia do lote imprime o texto cru. `app/templates/chamados/partials/_card_atendimento.html:106–167,185–195,393–394`, `app/templates/chamados/partials/_lote_preview.html:24`
- **Tela/template.** Tags, montagem do fio, explicação e histórico chegam ao texto, tipo, motivo, via e carimbos. O estado da pergunta e o fecho da disputa ainda olham `materializada_em`. `app/chamados/templatetags/chamados_tags.py:139,166`, `app/chamados/services/fio.py:178–188,371–378`, `app/chamados/services/explicador.py:55,102–118`, `app/chamados/services/historico.py:99–109`, `app/chamados/models.py:1998–2005,3226–3243`
- **Tela/template.** Prévia de folha consulta `pergunta_respondida` e `fio_vivo` com as instâncias ORM. Pendências de disputa chama `coletar`, que usa a disponibilidade de fecho e o predicado de espera do colaborador. `app/folha/previa.py:64–67,95,139–145`, `app/colaboradores/views.py:3043–3056`, `app/chamados/relatorios_disputa.py:23–28`, `app/chamados/services/fio.py:164–188`
- **API.** JSON do fio e das perguntas expõe `tipo_resposta`, `resposta_colab` e `respondida_em`. O POST devolve `respondida_em`. Mensageria expõe tipo, texto, instante e calcula dias de espera a partir dele. `app/api/views.py:1161–1165,2563,2577–2581,2657`, `app/api/views_mensageria.py:567–572`
- **PDF/TXT.** Não achei leitura direta das oito colunas nos geradores de PDF/TXT de folha ou no PDF de chamados. A saída textual de pendências usa indiretamente as colunas via `coletar`. `app/folha/validacao_pdf.py:58–65`, `app/folha/export.py:525–542`, `app/relatorios/pdf_chamados.py:158–169`, `app/chamados/management/commands/relatorio_pendencias_disputa.py:14–24,33–40`, `app/chamados/relatorios_disputa.py:23–28`
- **Placar/raio-x.** Crédito lê `materializada_em` no filtro e o par `respondida_em`/`resposta_colab` em `values_list`. O leitor de lavra testa presença de `materializada_em`. Painel e verbos agregam instantes e autoria. `app/chamados/services/raiox.py:135–147,182`, `app/chamados/services/painel.py:67,262–273`, `app/chamados/services/verbos.py:264–270,337–347`
- **Juiz em `chamados/juizes.py`.** A projeção temporal do C6 lê `validada_em`, `respondida_em`, `resposta_colab`, `materializada_em` e `via_resolucao`. A chave de memo inclui esses campos e `tipo_resposta`. `app/chamados/juizes.py:856–859,1382–1405`
- **Juiz em `chamados/juizes.py`.** Lastro, slot, concordância com a luz, declaração e plausibilidade usam tipo, texto, via ou carimbo. Os predicados de resposta e suas faces SQL usam o par físico. A fila ordena por `respondida_em`. `app/chamados/juizes.py:272–292,352–364,683–712,1346–1361,1502,1534–1591,1763–1766,1791,1934–1939,2041–2065`
- **SQL e leitores mistos.** `q_vivas` combina resposta, carimbo e via físicos com a validação pelo ato. `q_validada_pelo_ato` e `DecisaoPergunta.leitura` ainda sintetizam legado da projeção. A tarefa corta o horizonte e usa essa síntese. A foto de lavra ainda agrega os carimbos físicos. `app/chamados/models.py:3551–3561,3597–3612,4251–4270,4308–4313`, `app/chamados/services/tarefa_do_dia.py:262–263,398–432`, `app/chamados/veredito_lavrado.py:203–208`
- **Command/cron.** Backfill e auditoria leem o passivo, autoria, via e c

[truncado em 20000 de 30810 caracteres]

_7932 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
