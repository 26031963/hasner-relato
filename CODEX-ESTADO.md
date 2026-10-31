# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 12:49** · publicado em **31/10/2026 12:49** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 12:49 · sessao 423848803f88 · V3-RETOMA · sonda · origem=Code

**Não integralmente.** A sonda consulta corretamente o vínculo diário e o calendário, mas mede batidas no dia civil, usa outro universo e reconstrói incorretamente o “chamador” do fechamento. Referências: anexo `r2_sonda_interm_feriado.py:13–43`; `app/escala/alimentacao.py:297–301`; `app/ponto/services/fechamento.py:122–129,412–428`.

Nas referências ao anexo, conto a primeira linha de comentário como linha 1.

1. **Universo:** `universo_colabs_com_vinculo_intermitente` conta colaboradores distintos com algum vínculo de `tipo_ciclo='intermitente'` sobreposto à janela fixa. Não exige que esse vínculo seja escolhido em algum dia, nem que haja feriado ou batida. O rótulo descreve esse conjunto cadastral, mas ele não equivale ao universo do fechamento, que considera situação e demissão na janela da empresa. A candidatura normal dos vínculos também exige ativo ou fim declarado, condição ausente nessa pré-seleção. Referências: anexo `r2_sonda_interm_feriado.py:13–16`; `app/ponto/services/fechamento.py:412–428`; `app/escala/alimentacao.py:116–121`.

   Se “intermitente” significar todos os processados por `MotorIntermitente`, há outra diferença: a fábrica aceita `tipo_ciclo` **ou** `tipo_base` intermitente; a sonda verifica somente ciclo. Existência de casos adicionais no dado: [nao verificado]. Referências: anexo `r2_sonda_interm_feriado.py:13,32`; `app/ponto/motor_calculo_v2.py:3561–3564`.

2. **Competência:** a sonda fixa 21/08–20/09 e 21/09–20/10 de 2026. O fechamento obtém a janela por empresa, usando `dia_inicio_competencia`, com padrão 21 e limite entre 2 e 28. Portanto, essas janelas só equivalem às do sistema para empresas com corte efetivo 21; os cortes das empresas envolvidas são [nao verificado]. Referências: anexo `r2_sonda_interm_feriado.py:10`; `app/ponto/services/fechamento.py:598–602`; `app/ponto/janelas.py:36–57`.

3. **Vínculo do dia: alinhado.** `vinculo_do_dia_carregado` carrega células e vínculos, acrescenta as geradoras e chama a mesma juíza. A geradora nomeada pela célula prevalece, respeitadas suas datas; sem geradora, a escolha considera cobertura e precedência por ativo/data de início. A sonda não substitui isso por “último vínculo”. Referências: anexo `r2_sonda_interm_feriado.py:29–33`; `app/escala/alimentacao.py:237–254,284–301`.

4. **Dia e fuso:** `dia-colab feriado` significa, na sonda, pessoa/data com vínculo diário intermitente e **ao menos uma batida apurável entre 00:00:00 e 23:59:59**. Isso não comprova jornada trabalhada naquele feriado: uma saída de jornada da véspera pode entrar, e uma saída no dia seguinte fica fora. O sistema distingue batidas por instante de batidas pertencentes à jornada e pode atribuir o turno à véspera. Referências: anexo `r2_sonda_interm_feriado.py:27–40`; `app/ponto/turnos.py:283–307,2844–2852,2897–2925`.

   A sonda usa `make_aware` sem explicitar o fuso. A cópia configura `America/Sao_Paulo`; o motor converte por `settings.TIME_ZONE`. O fuso efetivamente ativo no processo da sonda e eventual deslocamento dos resultados são [nao verificado]. Referências: anexo `r2_sonda_interm_feriado.py:34–35`; `app/config/settings/base.py:93–95`; `app/ponto/motor_calculo_v2.py:28,73–79`.

5. **Calendário territorial: mesma regra, classificação restrita.** Sonda e fechamento usam o município do posto do colaborador. Nacional alcança todos; estadual exige UF; municipal exige UF e município normalizado. Trata-se do posto consultado no cadastro, sem resolução histórica por data. Referências: anexo `r2_sonda_interm_feriado.py:19–22`; `app/core/feriados.py:32–37,57–83,106–114`; `app/ponto/services/fechamento.py:621–623`.

   Entretanto, `feriados_de` devolve **datas**, e a sonda dá prioridade a nacional: havendo nacional e territorial na mesma data, conta somente nacional. Assim, `TERRITORIAL=0` significa nenhum dia-colaborador selecionado em data territorial **sem nacional coincidente**; não demonstra ausência de feriados territoriais cadastrados. Referências: anexo `r2_sonda_interm_feriado.py:15,39–40`; `app/core/feriados.py:83`; `app/core/models.py:28`.

   `sem_municipio` conta `lugar is None` no universo inteiro, antes dos filtros diários. Para essas pessoas, o juiz só reconhece nacionais. Se a pessoa sem município participou dos dias contados: [nao verificado]. Referências: anexo `r2_sonda_interm_feriado.py:18–22`; `app/core/feriados.py:50–65,113–114`.

6. **Horas e colaboradores:** `horas_bruto(max-min)` mede literalmente a soma das amplitudes entre primeira e última batida de cada dia selecionado, sem descontar pausas nem exigir pares; uma batida conta o dia e acrescenta zero horas. `colabs` conta pessoas distintas com algum desses dias. Esses rótulos são compatíveis com a operação, mas as horas não são horas apuradas ou dobra de feriado do fechamento, que recebe resultados do motor. Referências: anexo `r2_sonda_interm_feriado.py:36–41,46`; `app/ponto/services/fechamento.py:877–898`.

7. **“Chamador da última fatia”: divergência comprovada no código.** A sonda escolhe o vínculo de maior `data_inicio` sobreposto à janela. O fechamento efetivo usa `vinculo_de_referencia`: percorre os dias do fim para o início e retorna o primeiro vínculo respondido por `vinculo_do_dia`, ou `None`. Logo, o zero impresso não comprova zero divergências com o chamador real. Referências: anexo `r2_sonda_interm_feriado.py:23–26,42–43`; `app/ponto/services/fechamento.py:122–129,826–827`.

   O comentário “ULTIMA fatia” ainda aparece no fechamento, mas o executável passa a referência acima. Além disso, a chamada das rubricas a partir de 10/2026 resolve novamente template e intermitência por dia. Referências: `app/ponto/services/fechamento.py:1229–1243`; `app/ponto/calculador/alimentacao.py:487–510`.

Transcrição dos números fornecidos; a coluna de verificação indica ausência de conferência independente em produção. [nao verificado]

| Medida impressa | 09/2026 | 10/2026 | Verificação independente |
|---|---:|---:|---|
| Universo com vínculo intermitente | 8 | 9 | [nao verificado] |
| Sem município | 1 | 1 | [nao verificado] |
| Dia-colaborador nacional | 3 | 6 | [nao verificado] |
| Dia-colaborador territorial | 0 | 0 | [nao verificado] |
| Colaboradores com dias contados | 3 | 6 | [nao verificado] |
| Horas brutas nacionais | 23.87 | 36.27 | [nao verificado] |
| Horas brutas territoriais | 0.00 | 0.00 | [nao verificado] |
| Dia-colaborador com “chamador” da sonda não intermitente | 0 | 0 | [nao verificado] |

`ERROS 0` no dado fornecido. Esse contador cobre as exceções capturadas na consulta do vínculo diário; não valida equivalência com o fechamento. Referência: anexo `r2_sonda_interm_feriado.py:28–31,48–50`; execução em produção [nao verificado].

## 31/10 12:49 · sessao 8d89b05bec75 · - · - · origem=Ronald (tmux)

Achados de vínculo/dia, todos em leitura:

- A seleção diária da sonda está alinhada à autoridade: `vinculo_do_dia_carregado` delega à versão plural, que carrega células, inclui as geradoras e chama `vinculo_do_dia` (`app/escala/alimentacao.py:284`, `app/escala/alimentacao.py:297`).
- A autoridade primeiro lê `CelulaDia.escala_geradora_id`, encontra esse vínculo e verifica as datas; geradora ausente da alimentação ou fora da vigência resulta em `None`, sem escolher outro vínculo (`app/escala/alimentacao.py:237`, `app/escala/alimentacao.py:247`). Sem geradora, filtra cobertura e desempata por `(ativa, data_inicio)` (`app/escala/alimentacao.py:253`).
- O universo preliminar da sonda por mera sobreposição pode incluir vínculo inativo sem `data_fim`; a candidatura normal do sistema exige `ativa=True` ou fim declarado (`app/escala/alimentacao.py:116`). A cobertura diária também exige essa vigência declarada (`app/escala/alimentacao.py:164`, `app/escala/alimentacao.py:182`). **Nuance:** o ramo de geradora nomeada verifica datas e não verifica `ativa`; não afirmar rejeição universal de todo vínculo inativo sem fim (`app/escala/alimentacao.py:247`).
- O filtro `batidas_apuraveis` é adequado ao universo de batidas não retratadas, mas recorta **instantes**, usando `timestamp >= ini` e `<= fim`; não forma jornadas (`app/ponto/turnos.py:2844`). O sistema tem função distinta para as batidas de uma jornada, selecionando `data_turno == dia` (`app/ponto/turnos.py:2897`, `app/ponto/turnos.py:2916`).
- O dia civil da batida pode diferir do dia da jornada: o juiz converte para data local e, conforme os marcos noturnos, atribui à véspera (`app/ponto/turnos.py:283`, `app/ponto/turnos.py:295`, `app/ponto/turnos.py:297`). O motor também constrói mapa de entrada para `data_turno` (`app/ponto/motor_calculo_v2.py:829`).
- O fuso base da cópia é `America/Sao_Paulo`, com `USE_TZ=True` (`app/config/settings/base.py:93`). O motor converte usando `settings.TIME_ZONE` (`app/ponto/motor_calculo_v2.py:28`, `app/ponto/motor_calculo_v2.py:73`). A sonda usa `make_aware` sem fuso explícito; o fuso efetivamente ativo durante sua execução é [nao verificado]. Não concluir que o dado sofreu deslocamento UTC sem evidência.
- A fábrica considera intermitente tanto `tipo_ciclo == 'intermitente'` quanto `tipo_base == 'intermitente'`; o script verifica somente o primeiro (`app/ponto/motor_calculo_v2.py:3561`). Consequência concreta na população fornecida: [nao verificado].

## 31/10 12:48 · sessao 6c278f8056cc · - · - · origem=Ronald (tmux)

- **Calendário territorial alinhado:** a sonda chama os mesmos `lugar_do_colab` e `feriados_de` usados no fechamento. Este passa o município do posto atual; nacional vale para todos, estadual exige UF, municipal exige UF + município normalizado sem acento/caixa. Referências: `app/core/feriados.py:32-37`, `:57-67`, `:70-83`, `:106-114`; fechamento em `app/ponto/services/fechamento.py:621-623`.
- **Territorial zero tem significado restrito:** `feriados_de` devolve **datas**, perdendo identidade/abrangência de cada registro (`app/core/feriados.py:83`); o modelo admite registros de abrangências diferentes na mesma data (`app/core/models.py:28`). A sonda prioriza `nac` quando a data pertence a `nac_set`, então um feriado territorial coincidente com nacional entra só em nacional (anexo, linha `k = 'nac' if f in nac_set else 'terr'`). Não demonstra ausência de calendário territorial ou de feriados territoriais coincidentes [nao verificado].
- **Sem município:** ausência do município do posto leva a `None` (`app/core/feriados.py:113-114`); esse contexto só alcança nacional (`:50-65`, `:91-95`). O contador da sonda mede `lugar is None` para todos os colaboradores do seu universo, antes de selecionar dia/intermitência/batidas (anexo). Não permite concluir se a pessoa sem município teve algum dos dias contados [nao verificado].
- **Local histórico:** o juiz consultado lê o posto atual do colaborador, sem argumento de data (`app/core/feriados.py:106-114`), e o fechamento também (`app/ponto/services/fechamento.py:621-623`). A sonda está alinhada a isso; município/posto histórico e mudanças reais no período [nao verificado].
- **Competência:** sonda fixa corte 21. Fechamento resolve janela por empresa em `app/ponto/services/fechamento.py:599-602`; `app/ponto/janelas.py:36-57` usa `dia_inicio_competencia`, default 21, limitado a 2–28. Se todos os envolvidos têm corte 21 [nao verificado].
- **Dia do feriado:** rubrica usa dia de início do plantão, não data civil da batida; regra expressa em `app/ponto/calculador/regras.py:309-347`, particularmente `:334-337`. Fechamento amplia busca antes e depois da janela para turnos cruzando bordas (`app/ponto/services/fechamento.py:603-612`). A sonda conta qualquer batida no próprio dia civil do feriado (anexo), portanto não mede diretamente plantões atribuídos ao feriado.
- **Fuso configurado na cópia:** `TIME_ZONE='America/Sao_Paulo'`, `USE_TZ=True` (`app/config/settings/base.py:93-95`). Fuso efetivamente ativo no processo da sonda/produção [nao verificado].
- **Observação de caminho:** a cópia contém o fechamento em `app/ponto/services/fechamento.py`; o caminho citado pelo usuário `folha/fechamento.py` não foi localizado [nao verificado].

Nenhum arquivo foi criado, editado ou removido [nao verificado].

## 31/10 12:48 · sessao 0227bff3335b · - · - · origem=Ronald (tmux)

Achados em modo somente leitura:

- O arquivo citado como `folha/fechamento.py` corresponde, nesta cópia, ao fechamento em `app/ponto/services/fechamento.py`; a função de recálculo está em `app/ponto/services/fechamento.py:432`. Não encontrei `app/folha/fechamento.py` [nao verificado].
- **Universo:** o fechamento chama `universo_da_competencia` (`app/ponto/services/fechamento.py:538`), cujo predicado inclui não desligados ou desligados com demissão a partir do início da janela de sua empresa, com filtro opcional de empresa (`app/ponto/services/fechamento.py:412`). O `cids` do anexo seleciona vínculos intermitentes por interseção simples e não aplica esse universo [nao verificado: anexo sem arquivo local].
- **Competência:** a sonda fixa 21→20; o fechamento resolve a janela de cada colaborador pela empresa (`app/ponto/services/fechamento.py:598`). O corte vem de `dia_inicio_competencia`, com default 21 e clamp 2–28 (`app/ponto/janelas.py:36`, `app/ponto/janelas.py:50`, `app/ponto/janelas.py:54`). Se as empresas do dado usam 21 [nao verificado].
- **Chamador:** o executável não escolhe `order_by('-data_inicio').first()`. Ele carrega vínculos por `escalas_do_periodo` e acrescenta geradoras das células (`app/ponto/services/fechamento.py:628`), então resolve `esc = vinculo_de_referencia(...)` (`app/ponto/services/fechamento.py:826`). Essa função percorre dias do fim para o início e devolve o primeiro vínculo respondido por `vinculo_do_dia`, ou `None` (`app/ponto/services/fechamento.py:122`). Portanto, o `cham_dif` da sonda mede um proxy diferente [nao verificado: anexo sem arquivo local].
- O comentário “ULTIMA fatia” ainda aparece (`app/ponto/services/fechamento.py:1229`), mas o valor passado efetivamente é aquele resolvido em `:826–827`, na chamada de `rubricas_por_dia` (`app/ponto/services/fechamento.py:1239`). Além disso, essa porta sobrepõe template/intermitência pelo vínculo diário (`app/ponto/calculador/alimentacao.py:487`) e consulta a CCT do template diário (`app/ponto/calculador/alimentacao.py:507`).
- **Dia e horas:** o fechamento busca batidas com um dia adicional nas duas extremidades (`app/ponto/services/fechamento.py:603`), e o motor atribui o período à `data_jornada`, inclusive herdando jornada anterior quando pertinente (`app/ponto/motor_calculo_v2.py:2342`, `app/ponto/motor_calculo_v2.py:2367`). As horas gravadas vêm do resultado do motor (`app/ponto/services/fechamento.py:877`, `app/ponto/services/fechamento.py:898`). O max−min do anexo é amplitude de timestamps no dia civil, não horas de fechamento [nao verificado: anexo sem arquivo local].
- O código configura `America/Sao_Paulo` e `USE_TZ=True` (`app/config/settings/base.py:93`). O fuso efetivamente ativo na execução da sonda [nao verificado].
- Não devem ser usados como dados desta produção os números históricos presentes nos comentários do código; o dado fornecido não confirma que descrevam a execução anexada [nao verificado].

## 31/10 12:46 · sessao 75b1e64329fa · REPROVA-E-DESENHO-B3 · verificador · origem=Code

**Reprovaria a v2 principalmente pela prova de idade da linha e pela falta de garantia de consumo persistido.** Os pontos são estes. (bin/disjuntor.py:206–212, 247–260, 983–988)

1. **O próprio disjuntor pode tornar “nova” uma linha velha.** Depois de negar, ele escreve um alarme no RELATO, preservando o topo. Um DESENHO antigo, nunca consumido, pode ganhar `mtime >= disparado_em` nessa escrita automática e liberar a tentativa seguinte. SHA consumido não impede essa primeira aceitação indevida. É preciso identificar também a linha presente **no disparo**, ou registrar a escrita individual da autorização. (bin/disjuntor.py:161–164, 206–212, 983–988)

2. **“Consumida uma vez” não acompanha a persistência atual.** `_com_estado` executa a decisão antes de salvar; se a gravação falha, registra FALHA e devolve a decisão mesmo assim. Uma futura liberação pode passar sem persistir o SHA e ser repetida na próxima chamada. A v2 precisa declarar essa exceção ou condicionar a confirmação do consumo à gravação. Além disso, “exceção = passa, como hoje” é impreciso: o selo atual exige conservar a negativa quando a gravação falha. (bin/disjuntor.py:247–260; bin/tests/test_disjuntor.sh:369–373)

3. **A migração não pode olhar somente o estado já acumulado.** A varredura lê todos os transcripts da sessão, sem corte temporal, e incorpora os agentes encontrados. Uma reprovação antiga ainda não varrida pode entrar depois da migração e disparar retroativamente. Falta definir a fronteira do histórico e a ordem entre migrar e incorporar transcripts; `b3_limiar` sozinho não resolve isso. (bin/disjuntor.py:545–557, 565–575)

4. **Mover para `liberado` exige impedir a recontagem.** Hoje a varredura ignora somente IDs em `rearmado`; os demais voltam para `reprovados`. Criar `liberado` e esvaziar a lista, sem alterar esse filtro, ressuscita as mesmas reprovações no próximo Stop ou PreToolUse. (bin/disjuntor.py:567–570, 573–580)

5. **“Só Ronald rearma” não é garantido.** A guarda protege a chave e a retirada do hook, mas não `--rearma-b3`. Um Bash do principal sem `agent_id` passa diretamente; a CLI executa o rearme sem autenticar o chamador. O próprio construtor consegue usar o comando ensinado pela negativa para contornar DESENHO/ERRO-BOBO. `USER` aparece apenas no registro. (bin/disjuntor.py:136–158, 590–594, 929–930, 951–954, 1170–1173)

6. **Reconhecedor único não basta: falta uma transição única de liberação.** Hoje o Stop reconhece DESENHO apenas para calar o recado; o PreToolUse continua negando pela presença do disparo. Ambos precisam usar a mesma operação que valida, consome, move os IDs e desarma sob a trava do estado. Apenas mover `_pergunta_b3_escrita` deixaria essa divergência intacta; seu predicado atual também aceita somente “DESENHO + obra”, sem validar lei, pergunta ou referência. (bin/hook_stop_fila1.py:411–417, 1048–1054; bin/disjuntor.py:219–247, 947–954)

7. **A “trava dura” depende de reconhecer o prompt.** O PreToolUse só consulta B3 para `subagent_type=verificador`, prompt reconhecido como FINAL e obra legível. Sem obra reconhecida, a chamada passa por essa guarda; o contador `b3_sem_obra` observa transcripts posteriormente, mas não fecha essa passagem. “Corte novo” também precisa de definição operacional: o estado atual é indexado pela obra e o rearme recebe apenas obra/motivo. (bin/disjuntor.py:473–489, 554–566, 585–595, 947–954)

8. **O ESTADO ainda não mede o desfecho prometido.** O leitor não processa eventos `LIBERA`, e `linha()` não apresenta liberações por desenho/erro-bobo. A trilha também trunca detalhes em 300 caracteres; portanto, `LIBERA …: <linha>` não preservará necessariamente a autorização inteira. (bin/disjuntor.py:117–118, 1035–1059, 1147–1161)

9. **O selo atual contradiz a v2, e o selo proposto precisa provar o ciclo completo.** Hoje duas reprovações devem permitir o Agent e somente a terceira dispara; DESENHO é testado para calar o Stop, enquanto a autorização do Agent só é demonstrada após rearme manual. Os testes e mutantes também chamam diretamente o reconhecedor que sairia do hook. É necessário substituir essas expectativas e provar: primeira reprovação → negativa **sem Stop** → linha válida → autorização sem FALHA → nova reprovação → mesma linha negada → outra linha libera; incluindo migração, recontagem e falha de persistência. (bin/tests/test_disjuntor.sh:320–348, 363–373, 638–655, 786–812)

Análise estática; não executei o selo, pois ele cria e remove arquivos temporários. (bin/tests/test_disjuntor.sh:37–49)

## 31/10 12:46 · sessao 263a6b853e68 · V3-RETOMA · leitura · origem=Code

**Na regra de dobra, a v2 paga o nacional e perde o municipal; a v3 por `rubricas_por_dia` paga ambos, inclusive com chamador 12x36, porque usa o ciclo do vínculo do dia e preserva o calendário recebido do chamador.** A diferença está no conjunto de feriados efetivamente consultado, não no predicado de dobra, que é compartilhado. (`app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:879`, `app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:290`.)

Uso **W₂/W₃** para as horas efetivamente apuradas em cada caminho, após seus recortes; registro de batida sozinho não garante esse total, pois há retorno para turno aberto e bloqueio posterior de HE. (`app/ponto/motor_calculo_v2.py:2690`, `app/ponto/motor_calculo_v2.py:2697`, `app/ponto/motor_calculo_v2.py:2712`, `app/ponto/motor_calculo_v2.py:2399`.)

| Caminho, com vínculo do dia intermitente e trabalho apurado positivo | Nacional cadastrado e consultável | Municipal isolado, presente no calendário territorial recebido |
|---|---|---|
| **A — v2 por fatia** | **W₂ em HE100 e W₂ no subconjunto feriado.** (`app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:2750`.) | **0 de dobra de feriado.** O conjunto é apagado e o fallback sem município não recupera municipal. (`app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:905`, `app/core/feriados.py:64`.) |
| **B — v3; chamador 12x36** | **W₃ em `horas_extras_100` e W₃ em `horas_extras_100_feriado`.** (`app/ponto/calculador/alimentacao.py:494`, `app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/regras.py:780`, `app/ponto/calculador/regras.py:785`.) | **W₃ nas mesmas duas rubricas.** (`app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:294`, `app/ponto/calculador/regras.py:347`.) |
| **B — v3; chamador intermitente** | **W₃ nas mesmas duas rubricas.** (`app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/regras.py:780`, `app/ponto/calculador/regras.py:785`.) | **W₃ nas mesmas duas rubricas.** (`app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:347`, `app/ponto/calculador/regras.py:785`.) |

As duas rubricas são total e subconjunto: não se somam para obter outra dobra. Na v2 isso está declarado no campo e na propriedade de soma; no export a parcela normal é calculada subtraindo o subconjunto feriado do total. (`app/ponto/motor_calculo_v2.py:169`, `app/ponto/motor_calculo_v2.py:282`, `app/folha/export.py:409`, `app/folha/export.py:410`.)

**A — por que a v2 chega nisso**

1. O fechamento efetivo está em `app/ponto/services/fechamento.py`: por fatia, toma `esc.tipo_escala`, monta o motor e chama `calcular_mes` com aquele tipo e vínculo. Portanto, nesse caminho, a escolha do motor acompanha a fatia intermitente. (`app/ponto/services/fechamento.py:262`, `app/ponto/services/fechamento.py:263`, `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:273`.)

2. O construtor chama `get_motor_cct` com `tipo_escala=esc.tipo_escala` e `feriados=feriados`. O calendário foi carregado territorialmente; concretamente, o lugar usado é `colaborador.posto.municipio`, não uma nova resolução do município por fatia. (`app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:622`, `app/ponto/services/fechamento.py:709`, `app/ponto/services/fechamento.py:711`, `app/ponto/services/fechamento.py:713`, `app/core/feriados.py:113`, `app/core/feriados.py:114`.)

3. `get_motor` escolhe `MotorIntermitente` prioritariamente quando ciclo **ou** base é intermitente. A política padrão continua sendo `ciclo not in CICLOS_FERIADO_SIMPLES`; esse conjunto contém apenas `'12x36'`. Logo, intermitente recebe `feriado_em_dobra=True`. (`app/ponto/motor_calculo_v2.py:3563`, `app/ponto/motor_calculo_v2.py:3564`, `app/ponto/motor_calculo_v2.py:3537`, `app/ponto/motor_calculo_v2.py:3607`, `app/ponto/motor_calculo_v2.py:3608`.)

4. `MotorIntermitente` herda `Motor12x36` e seu único método próprio é o construtor: aumenta `jornada_minutos` e sobrescreve `self.feriados` com conjunto vazio. Não desliga `feriado_em_dobra`. (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:3491`, `app/ponto/motor_calculo_v2.py:3494`, `app/ponto/motor_calculo_v2.py:3495`.)

5. `eh_feriado` consulta o conjunto e, se não encontrar a data, chama `is_feriado(data)` **sem município**. Esse wrapper chama o juiz territorial com `None`; nacional alcança todos, municipal exige UF e município correspondentes. Portanto, apagar o conjunto perde o municipal, mas não impede recuperar nacional pelo banco. (`app/ponto/motor_calculo_v2.py:903`, `app/ponto/motor_calculo_v2.py:905`, `app/ponto/motor_calculo_v2.py:30`, `app/ponto/motor_calculo_v2.py:41`, `app/core/feriados.py:60`, `app/core/feriados.py:64`, `app/core/feriados.py:95`.)

6. O predicado real é somente `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. No `calcular_periodo` herdado, duração positiva e predicado verdadeiro escrevem **toda a duração**, não apenas excedente, em HE100 e HE100 feriado. (`app/ponto/motor_calculo_v2.py:879`, `app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:2748`, `app/ponto/motor_calculo_v2.py:2749`, `app/ponto/motor_calculo_v2.py:2750`.)

O comentário “feriado em dia não previsto” não corresponde ao predicado executado: este não consulta previsão. O dia passado pelo cálculo é `e.date()`, e a docstring do helper declara a regra pelo início do plantão. (`app/ponto/motor_calculo_v2.py:2745`, `app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:873`, `app/ponto/motor_calculo_v2.py:879`.)

**Zero municipal significa zero de dobra**, não garantia universal de HE100 total zero: o ramo normal pode produzir HE100 por excedente acima de 120 minutos, se houver previsto positivo; sem previsto, usa a própria duração como limite e não gera esse excedente. (`app/ponto/motor_calculo_v2.py:2773`, `app/ponto/motor_calculo_v2.py:2778`, `app/ponto/motor_calculo_v2.py:2791`, `app/ponto/motor_calculo_v2.py:2792`.)

**B — por que as duas variantes da v3 dão o mesmo resultado**

1. `_te_dia` começa com o tipo do chamador, mas o vínculo do dia conhecido o substitui; também substitui `_interm_dia`. Depois, a chamada é `_cct_de(_te_dia)`. Portanto, com o vínculo intermitente resolvido, o chamador 12x36 não fornece a lei do feriado desse dia. (`app/ponto/calculador/alimentacao.py:487`, `app/ponto/calculador/alimentacao.py:490`, `app/ponto/calculador/alimentacao.py:494`, `app/ponto/calculador/alimentacao.py:495`, `app/ponto/calculador/alimentacao.py:507`.)

2. `_cct_de` cria motor pela fábrica da casa, com aquele template; consulta a lei da família e copia os quatro campos pela fronteira `cct_do_motor`. A consulta usa `ini` como competência e cache por template, não `k` como competência diária. (`app/ponto/calculador/alimentacao.py:389`, `app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:404`.)

3. Esse motor intermitente tem **flag de dobra verdadeiro e calendário vazio**. A fronteira copia ambos. Entretanto, a alimentação usa apenas `_cct.feriado_em_dobra` para essa decisão; o calendário enviado a `do_dia` é `_feriados`, construído diretamente do argumento `feriados` do chamador. **O vazio de `_cct.feriados` não é utilizado nessa chamada.** (`app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:3608`, `app/ponto/calculador/nucleo.py:350`, `app/ponto/calculador/nucleo.py:351`, `app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:510`, `app/ponto/calculador/alimentacao.py:524`.)

4. `_ContextoFeriado` recebe esse calendário preservado e o flag verdadeiro; empresta exatamente os métodos `MotorBase._feriado_dobra_no_dia` e `MotorBase.eh_feriado`. Assim, o municipal presente no conjunto responde verdadeiro antes de precisar do fallback. (`app/ponto/calculador/regras.py:290`, `app/ponto/calculador/regras.py:291`, `app/ponto/calculador/regras.py:294`, `app/ponto/calculador/regras.py:295`, `app/ponto/motor_calculo_v2.py:903`.)

5. A função de dobra devolve todo o trabalhado positivo; `do_dia` escreve esse total em HE100 e HE100 feriado, zera HE50 e retorna antes do ramo de excedente. Mesmo com `dia_de_trabalho=False`, intermitente segue pelo caminho normal, sem o retorno da folga. (`app/ponto/calculador/regras.py:345`, `app/ponto/calculador/regras.py:347`, `app/ponto/calculador/regras.py:780`, `app/ponto/calculador/regras.py:785`, `app/ponto/calculador/regras.py:786`, `app/ponto/calculador/regras.py:789`, `app/ponto/calculador/regras.py:736`, `app/ponto/calculador/regras.py:738`.)

Se o municipal não vier no conjunto do chamador, a v3 também não o recupera pelo fallback sem município. No fechamento, o mesmo calendário territorial é efetivamente passado à porta v3. (`app/ponto/motor_calculo_v2.py:905`, `app/core/feriados.py:64`, `app/ponto/services/fechamento.py:622`, `app/ponto/services/fechamento.py:1242`.)

**Ressalva para o fechamento completo: “trabalho registrado” não basta**

Após `calcular_periodo`, `calcular_mes` chama `_dia_sem_previsto_fora_da_janela`. Havendo feriado ou folga, empresa na janela de HE, previsto exatamente zero e ausência de autorização, o bloqueio pode zerar trabalho e todas as extras, inclusive feriado; a exceção de folga sem escala certa está no mesmo portão. (`app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2399`, `app/ponto/motor_calculo_v2.py:1386`, `app/ponto/motor_calculo_v2.py:1389`, `app/ponto/motor_calculo_v2.py:1391`, `app/ponto/motor_calculo_v2.py:1394`, `app/ponto/motor_calculo_v2.py:1403`, `app/ponto/motor_calculo_v2.py:1411`.)

Esse bloqueio também colapsa o segmento efetivo; a alimentação v3 descarta o par colapsado e inclui o dia bloqueado com zero minutos. Logo, **nacional pode terminar em zero nos dois caminhos por esse portão**, sem que isso implemente “intermitente não dobra”. (`app/ponto/motor_calculo_v2.py:1420`, `app/ponto/motor_calculo_v2.py:1421`, `app/ponto/calculador/alimentacao.py:102`, `app/ponto/calculador/alimentacao.py:111`, `app/ponto/calculador/alimentacao.py:467`, `app/ponto/calculador/alimentacao.py:469`.)

**Quem escreve e quem lê os cinco insumos**

| Campo | Escritores/resolução na v2 | Leitores na v2 |
|---|---|---|
| `feriados` | Fechamento carrega e injeta; base grava `set`; intermitente sobrescreve vazio. (`app/ponto/services/fechamento.py:622`, `app/ponto/services/fechamento.py:713`, `app/ponto/motor_calculo_v2.py:740`, `app/ponto/motor_calculo_v2.py:3495`.) | `eh_feriado` testa pertinência e chama fallback quando ausente. (`app/ponto/motor_calculo_v2.py:903`, `app/ponto/motor_calculo_v2.py:905`.) |
| `feriado_em_dobra` | Fábrica resolve pelo ciclo; CCT pode conceder `True` ao 12x36; base grava booleano. (`app/ponto/motor_calculo_v2.py:3608`, `app/core/regua_cct.py:477`, `app/core/regua_cct.py:478`, `app/ponto/motor_calculo_v2.py:737`.) | Helper decide; ramo adicional lê para alerta. (`app/ponto/motor_calculo_v2.py:879`, `app/ponto/motor_calculo_v2.py:2756`.) |
| `prorrogacao_noturna_pos5h` | CCT injeta; sem valor, base consulta `prorrogacao_pos5h_legal` e grava. Essa função responde `tipo_ciclo != '12x36'`. (`app/core/regua_cct.py:454`, `app/core/regua_cct.py:466`, `app/ponto/motor_calculo_v2.py:708`, `app/ponto/motor_calculo_v2.py:712`, `app/core/regua_cct.py:182`.) | Janela noturna, desconto noturno dos intervalos e split de HE noturna. (`app/ponto/motor_calculo_v2.py:922`, `app/ponto/motor_calculo_v2.py:1866`, `app/ponto/motor_calculo_v2.py:955`.) |
| `hora_reduzida_afastada` | Fábrica tem padrão falso; CCT injeta somente com cláusula, ciclo 12x36 e trava liberada; base grava. (`app/ponto/motor_calculo_v2.py:3540`, `app/core/regua_cct.py:470`, `app/core/regua_cct.py:471`, `app/core/regua_cct.py:472`, `app/ponto/motor_calculo_v2.py:719`.) | `fator_hora_noturna`: `1.0` se afastada, senão fator reduzido. (`app/ponto/motor_calculo_v2.py:930`.) |
| Lei da família | A declaração está em `NAO_JULGA_PONTUALIDADE`, incluindo a identidade `MotorIntermitente`. (`app/ponto/calculador/regras.py:487`, `app/ponto/calculador/regras.py:494`.) | No cálculo v2 analisado, a ausência de julgamento decorre do método herdado: intermitente herda o cálculo cru; a chamada a `_julgar_pontualidade` pertence à filha `Motor12x36ComEscala`. (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:2688`, `app/ponto/motor_calculo_v2.py:2794`, `app/ponto/motor_calculo_v2.py:3461`, `app/ponto/motor_calculo_v2.py:3471`.) |

| Campo | Escritores/transporte na v3 | Leitores na v3 |
|---|---|---|
| `feriados` | `cct_do_motor` copia o conjunto do motor; **alimentação usa separadamente o argumento do chamador**; `_ContextoFeriado` grava esse conjunto. (`app/ponto/calculador/nucleo.py:350`, `app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:294`.) | `do_dia` exige calendário e chama dobra; método emprestado testa pertinência. Já `periodo.reler` usa `cct.feriados`. (`app/ponto/calculador/regras.py:633`, `app/ponto/calculador/regras.py:775`, `app/ponto/motor_calculo_v2.py:903`, `app/ponto/calculador/periodo.py:830`.) |
| `feriado_em_dobra` | Fronteira copia; alimentação lê e passa; contexto grava. (`app/ponto/calculador/nucleo.py:351`, `app/ponto/calculador/alimentacao.py:510`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:295`.) | Guarda de entrada, helper emprestado e releitura por período. (`app/ponto/calculador/regras.py:633`, `app/ponto/calculador/regras.py:290`, `app/ponto/motor_calculo_v2.py:879`, `app/ponto/calculador/periodo.py:831`.) |
| `prorrogacao_noturna_pos5h` | Fronteira renomeia para `prorrogacao_pos5h`; alimentação transporta; contexto noturno regrava o nome esperado pelo método v2. (`app/ponto/calculador/nucleo.py:348`, `app/ponto/calculador/alimentacao.py:508`, `app/ponto/calculador/alimentacao.py:530`, `app/ponto/calculador/regras.py:61`.) | Guarda noturna, cálculo de janela/intervalos, split HE e `periodo.noturno_no_par`. (`app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:85`, `app/ponto/calculador/regras.py:86`, `app/ponto/calculador/regras.py:824`, `app/ponto/calculador/periodo.py:508`.) |
| `hora_reduzida_afastada` | Fronteira copia; alimentação lê e passa. (`app/ponto/calculador/nucleo.py:349`, `app/ponto/calculador/alimentacao.py:509`, `app/ponto/calculador/alimentacao.py:530`.) | Guarda de entrada e fator noturno; releitura por período. (`app/ponto/calculador/regras.py:609`, `app/ponto/calculador/regras.py:88`, `app/ponto/calculador/periodo.py:509`.) |
| Lei da família | Tabela declara a causa; `motivo_nao_julga_pontualidade` lê por `type(motor)` exato; alimentação consulta e passa a causa. (`app/ponto/calculador/regras.py:494`, `app/ponto/calculador/regras.py:518`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:531`.) | `do_dia` declara atraso/antecipada e descarta pontualidade recebida; releitura também recebe `fora_por_lei`. Há consumidor lateral em `chamados/juizes.py`. (`app/ponto/calculador/regras.py:618`, `app/ponto/calculador/regras.py:620`, `app/ponto/calculador/regras.py:622`, `app/ponto/calculador/periodo.py:787`, `app/chamados/juizes.py:1551`.) |

Os três flags também são lidos pelo carimbo da régua, sem refazer a decisão. (`app/core/regua_cct.py:423`, `app/core/regua_cct.py:424`, `app/core/regua_cct.py:425`.)

Na origem cadastral da CCT, a tela escreve hora reduzida, concessão de dobra 12x36 e prorrogação noturna; a régua lê os campos do sindicato. (`app/core/views_config.py:278`, `app/core/views_config.py:279`, `app/core/views_config.py:297`, `app/core/views_config.py:309`, `app/core/regua_cct.py:374`.)

**Inventário dos sítios que decidem ou materializam a resposta**

| Sítio | Predicado/consequência |
|---|---|
| Calendário territorial | Nacional sempre alcança; estadual exige UF; municipal exige UF e município normalizado. Sem município, somente nacional. (`app/core/feriados.py:57`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:91`.) |
| Fábrica v2 | Sem override: `ciclo not in {'12x36'}`. (`app/ponto/motor_calculo_v2.py:3537`, `app/ponto/motor_calculo_v2.py:3607`.) |
| `get_motor_cct` | Switch desligado delega; ligado, ciclo literal 12x36 e cláusula verdadeira concedem flag de dobra por `setdefault`. (`app/core/regua_cct.py:449`, `app/core/regua_cct.py:457`, `app/core/regua_cct.py:477`, `app/core/regua_cct.py:478`.) |
| `MotorBase._feriado_dobra_no_dia` | `bool(flag) and eh_feriado(dia)`. (`app/ponto/motor_calculo_v2.py:879`.) |
| `Motor12x36.calcular_periodo` | Duração positiva + helper: duração inteira em HE100 feriado. (`app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:2750`.) |
| Herdeiros/delegadores desse cálculo | Intermitente herda; 12x36 com escala chama `super`; plantão delega explicitamente. Intermitente ainda modifica o calendário. (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:3461`, `app/ponto/motor_calculo_v2.py:3449`.) |
| TurnoPartido, consolidação diária | Trabalho positivo + helper: total diário no último período, seguido de `continue`. A variante noturna herda sem alteração. (`app/ponto/motor_calculo_v2.py:3030`, `app/ponto/motor_calculo_v2.py:3033`, `app/ponto/motor_calculo_v2.py:3039`, `app/ponto/motor_calculo_v2.py:3474`.) |
| Comercial por período | Duração positiva + helper: duração inteira em HE100 feriado. (`app/ponto/motor_calculo_v2.py:3193`, `app/ponto/motor_calculo_v2.py:3196`.) |
| Comercial, consolidação diária | Trabalho positivo + helper: total diário em HE100 feriado e `continue`. (`app/ponto/motor_calculo_v2.py:3275`, `app/ponto/motor_calculo_v2.py:3279`, `app/ponto/motor_calculo_v2.py:3290`.) |
| `regras.feriado_em_dobra_do_ciclo` | Consulta a mesma constante e retorna “ciclo fora dela”. (`app/ponto/calculador/regras.py:305`, `app/ponto/calculador/regras.py:306`.) |
| `regras.dobra_de_feriado_do_dia` / `_ContextoFeriado` | Trabalho não positivo → zero; senão todo o trabalho quando o helper emprestado responde verdadeiro. (`app/ponto/calculador/regras.py:290`, `app/ponto/calculador/regras.py:344`, `app/ponto/calculador/regras.py:345`, `app/ponto/calculador/regras.py:347`.) |
| `alimentacao.rubricas_por_dia` → `regras.do_dia` | Combina flag do template diário com calendário do chamador; dobra positiva substitui HE50/excedente. (`app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/alimentacao.py:524`, `app/ponto/calculador/regras.py:775`, `app/ponto/calculador/regras.py:786`, `app/ponto/calculador/regras.py:789`.) |
| `periodo.reler` | Soma trabalho do dia da jornada e chama dobra com **`cct.feriados`** e `cct.feriado_em_dobra`; dobra positiva é escrita no último período, senão usa excedente. (`app/ponto/calculador/periodo.py:817`, `app/ponto/calculador/periodo.py:830`, `app/ponto/calculador/periodo.py:831`, `app/ponto/calculador/periodo.py:838`, `app/ponto/calculador/periodo.py:841`, `app/ponto/calculador/periodo.py:844`.) |
| Export | Suprime a parcela feriado se `tipo_ciclo == '12x36' and not feriado_12x36_em_dobra`. Ciclo e cláusula são consultados no fim da janela; é decisão mensal de emissão. (`app/folha/export.py:348`, `app/folha/export.py:359`, `app/folha/export.py:369`, `app/folha/export.py:411`, `app/folha/export.py:412`.) |
| Bloqueio posterior de HE | Feriado/folga, empresa na janela, previsto zero e sem autorização podem zerar inclusive dobra já calculada. (`app/ponto/motor_calculo_v2.py:1387`, `app/ponto/motor_calculo_v2.py:1389`, `app/ponto/motor_calculo_v2.py:1391`, `app/ponto/motor_calculo_v2.py:1394`, `app/ponto/motor_calculo_v2.py:1412`.) |

**`periodo.reler` é uma diferença adicional relevante:** alimentado por `cct_do_motor(MotorIntermitente)`, recebe calendário vazio; porta

[truncado em 20000 de 24300 caracteres]

## 31/10 12:46 · sessao f49bbf6a890c · - · - · origem=Ronald (tmux)

Os furos residuais mais fortes são:

1. **A negativa pode “rejuvenescer” uma linha antiga.** O PreToolUse escreve no RELATO depois de negar; o escritor preserva o topo, mas regrava o arquivo. Portanto, um DESENHO válido antigo e nunca consumido pode falhar pela idade na primeira tentativa e passar na seguinte apenas porque a negativa atualizou o `mtime`. Guardar SHA consumido não resolve a primeira utilização dessa linha antiga. É necessário comparar também com o conteúdo observado no disparo. Evidência: `bin/disjuntor.py:161-164`, `bin/disjuntor.py:209-212`, `bin/disjuntor.py:979-988`; escrita efetiva: `app/core/esteira_vigia.py:829-831`, `app/core/esteira_vigia.py:879-884`.

2. **“Só Ronald rearma” não é uma propriedade atual do mecanismo.** A guarda protege a chave e a remoção do hook, mas não `--rearma-b3`; um Bash do principal sem `agent_id` passa imediatamente. A CLI executa o rearme diretamente e usa `USER` apenas para registrar quem chamou. O próprio motivo da negativa ensina esse comando. Assim, o construtor pode contornar integralmente DESENHO/ERRO-BOBO. Evidência: `bin/disjuntor.py:136-156`, `bin/disjuntor.py:924-930`, `bin/disjuntor.py:951-954`, `bin/disjuntor.py:590-594`, `bin/disjuntor.py:1170-1173`.

3. **Centralizar o reconhecedor não centraliza a liberação.** Hoje o Stop reconhece texto apenas para calar o recado; o PreToolUse consulta o disparo e continua negando. A v2 precisa definir uma transição compartilhada, sob a mesma trava do estado, que consuma a linha, mova os reprovados e desarme. Caso o hook apenas chame um reconhecedor que consome SHA, pode consumir a autorização sem desarmar a trava; se ambos implementarem a transição, haverá duas autoridades e possibilidade de consumo duplicado. Evidência do fluxo atual e da trava disponível: `bin/hook_stop_fila1.py:1048-1054`, `bin/disjuntor.py:947-954`, `bin/disjuntor.py:219-247`. O efeito da implementação futura permanece `[não verificado]`.

4. **“Corte novo” não identifica um novo ciclo no estado.** O registro é indexado somente pela obra; o rearme recebe somente obra e motivo. Não há identificação de corte nessas transições, logo a v2 precisa dizer como um corte novo chega ao mecanismo e quais memórias ele encerra — inclusive SHA consumidos. Evidência: `bin/disjuntor.py:566-580`, `bin/disjuntor.py:585-595`, `bin/disjuntor.py:1170-1173`.

## 31/10 12:45 · sessao 0d8883ba45c4 · - · - · origem=Ronald (tmux)

Achados em `bin/disjuntor.py`, priorizados:

1. **`mtime` do RELATO permite liberar com linha velha.** Cada negativa chama `relato_negacao` depois da decisão (`bin/disjuntor.py:983-988`), que escreve no próprio RELATO (`bin/disjuntor.py:206-212`). O escritor mantém o topo byte a byte (`app/core/esteira_vigia.py:829-831`), mas regrava todo o arquivo via temporário e `os.replace` (`app/core/esteira_vigia.py:879-884`). Portanto um DESENHO antigo, ainda não consumido, ganha `mtime >= disparado_em` pela própria negativa seguinte. Guardar somente SHA consumido não impede a primeira aceitação indevida. A regra precisa comparar também a linha presente no disparo ou registrar sua escrita individualmente.

2. **Mover `reprovados` para `liberado` não basta: a varredura os ressuscita.** Hoje só `rearmado` exclui IDs históricos (`bin/disjuntor.py:567-568`); qualquer outro ID encontrado volta a `reprovados` (`bin/disjuntor.py:569-570`) e o limiar dispara (`bin/disjuntor.py:573-580`). `liberado` precisa participar dessa exclusão na mesma operação de estado.

3. **Migração somente das listas já guardadas não cobre reprovação antiga ainda não varrida.** A coleta lê todos os transcripts da sessão sem filtro temporal (`bin/disjuntor.py:545-557`), depois incorpora os IDs ao estado (`bin/disjuntor.py:565-570`). É necessário especificar a ordem da migração e a fronteira de IDs/transcripts antigos; caso contrário, um REPROVADO anterior à troca do limiar entra depois como novo.

4. **Consumo único não é durável sob a falha de gravação hoje tolerada.** `_com_estado` executa a decisão primeiro (`bin/disjuntor.py:247`), tenta persistir depois (`bin/disjuntor.py:249-253`) e, se falhar, registra FALHA mas devolve a decisão (`bin/disjuntor.py:254-260`). Uma liberação implementada nesse callback pode passar sem persistir o SHA, permitindo reutilizar a linha no processo seguinte. O contrato precisa declarar essa exceção de fail-open ou exigir persistência antes de confirmar LIBERA.

5. **O PreToolUse atual nunca consulta a linha de liberação.** A decisão depende somente da presença da obra em `varrer_b3`; se disparada, nega incondicionalmente e recomenda rearme manual (`bin/disjuntor.py:947-954`). A integração precisa liberar, consumir e recontar dentro do disjuntor, inclusive quando nenhum Stop tiver rodado.

6. **`corte novo` não tem representação operacional.** A chave é apenas a identidade da obra e os elementos são IDs dos agentes (`bin/disjuntor.py:565-570`); só o rearme explícito move esses IDs e apaga o disparo (`bin/disjuntor.py:585-595`). Se “corte novo” deve desligar B3 mantendo a obra, falta definir o sinal e como ele muda o estado.

7. **A medição de LIBERA não existe no leitor do ESTADO.** `ler` percorre eventos NEGOU, FALHA, DISJUNTOR e ATENDIDO (`bin/disjuntor.py:1035-1037`); conta disparos pelo token B3 (`bin/disjuntor.py:1054-1055`), mas não liberações por tipo. `linha` só apresenta disparos e outros contadores atuais (`bin/disjuntor.py:1147-1161`). Além disso, a trilha reduz qualquer detalhe a 300 caracteres (`bin/disjuntor.py:117-118`), portanto `LIBERA ...: <linha>` não preservará integralmente linhas longas.

8. **“Exceção = passa, como hoje” precisa ser qualificado.** O tratamento externo realmente abre na exceção (`bin/disjuntor.py:989-990`), mas a falha de persistência deliberadamente conserva uma negativa já tomada (`bin/disjuntor.py:254-260`). O selo atual exige isso (`bin/tests/test_disjuntor.sh:369-373`).

O limiar ainda é 3 (`bin/disjuntor.py:68`), e o disparo não registra `disparado_em` nem a geração do limiar: apenas marca `disparado=True` (`bin/disjuntor.py:574-578`). A chave desligada com guarda ativa já está na ordem pedida (`bin/disjuntor.py:979-981`).

## 31/10 12:45 · sessao ec5780f53744 · - · - · origem=Ronald (tmux)

1. **O selo atual contradiz o limiar v2.** `b3()` exige que duas reprovações não disparem e que o próximo Agent passe; só a terceira dispara. Assim, mudar para `1` derruba casos atuais, enquanto `3` é o comportamento esperado hoje. Os casos e os controles de cópia precisam ser atualizados antes de o mutante `=3` provar algo. (bin/tests/test_disjuntor.sh:320–329; bin/tests/test_disjuntor.sh:722)

2. **Os testes de DESENHO atuais provam apenas silêncio do Stop.** `_b3_cala()` verifica `siga`, ausência de recado B3 e ausência de FALHA; não chama PreToolUse para demonstrar que a linha libera um Agent. A autorização positiva do Agent só aparece após `--rearma-b3`. (bin/tests/test_disjuntor.sh:335–348; bin/tests/test_disjuntor.sh:363–365)

3. **Os testes estão acoplados ao reconhecedor que a proposta remove.** `b3_topo()` importa `hook_stop_fila1` e captura diretamente `H._pergunta_b3_escrita`; vários mutantes dependem do texto dessa função no hook. Mover a função exige migrar testes e âncoras para `disjuntor.linha_libera_b3`, preservando os controles de identidade da obra. (bin/tests/test_disjuntor.sh:638–655; bin/tests/test_disjuntor.sh:786–809)

4. **O selo v2 descrito não cobre a transição mais importante: liberar e disparar novamente.** A cobertura atual verifica persistência do *rearme*, mas não existe ciclo de liberação por linha. O novo selo deveria executar: primeira reprovação → Pre nega sem Stop → linha válida → Pre passa sem FALHA → nova reprovação → mesma linha nega → linha diferente libera; deve verificar também que os agentes antigos saíram de `reprovados`, entraram em `liberado` e não reaparecem na varredura. O scanner atual relê todos os transcripts e só exclui agentes em `rearmado`. (bin/tests/test_disjuntor.sh:363–368; bin/disjuntor.py:550–570)

5. **Migração precisa de teste para histórico ainda não contado.** Testar somente estado com duas reprovações não detecta transcript antigo nunca escaneado: o scanner atual encontra todos os `agent-*.jsonl` e adiciona seus IDs ao estado. Um teste precisa manter transcript antigo fora do estado, executar migração e repetir varredura; caso contrário, `b3_limiar` pode estar correto no JSON e ainda disparar retroativamente. (bin/disjuntor.py:550–557; bin/disjuntor.py:565–575)

6. **O selo proposto não verifica a medição prometida.** A rotina atual de ESTADO percorre eventos `NEGOU`, `FALHA`, `DISJUNTOR` e `ATENDIDO`; não lê `LIBERA`, e `linha()` só exibe disparos e negativas. Convém incluir eventos dentro/fora de 24 horas e duas categorias de liberação, além de não contar `MIGRA` ou negativas repetidas como novos disparos. (bin/disjuntor.py:1036–1059; bin/disjuntor.py:1147–1161)

7. **Teste adversarial necessário para “linha velha”.** Uma negativa atual escreve no RELATO depois de responder, mesmo que preserve o topo. Portanto, basta comparar o mtime global do RELATO ao disparo para uma linha antiga poder parecer nova após a escrita automática de alarmes; o selo precisa tentar essa sequência específica. (bin/disjuntor.py:983–988; bin/tests/test_disjuntor.sh:574–586)

Análise estática; os testes não foram executados, pois criam e removem arquivos temporários. (bin/tests/test_disjuntor.sh:37–49)

## 31/10 12:44 · sessao 03ec68bba43d · - · - · origem=Ronald (tmux)

Leitura sem alterações. O arquivo do fechamento encontrado nesta árvore é `app/ponto/services/fechamento.py`; ele declara o recalculo da folha e implementa o laço por fatias (`app/ponto/services/fechamento.py:2`, `app/ponto/services/fechamento.py:132`, `app/ponto/services/fechamento.py:203`).

**A: resultado da regra de feriado do v2**

- `get_motor` escolhe `MotorIntermitente` quando `tipo_ciclo == 'intermitente'` ou `tipo_base == 'intermitente'`; `CICLOS_FERIADO_SIMPLES` contém apenas `12x36`, portanto o flag calculado do intermitente é `feriado_em_dobra=True` (`app/ponto/motor_calculo_v2.py:3563`, `app/ponto/motor_calculo_v2.py:3537`, `app/ponto/motor_calculo_v2.py:3607`).
- `MotorIntermitente` herda de `Motor12x36`, define somente `__init__`, aumenta `jornada_minutos` e apaga o conjunto de feriados recebido; não sobrescreve `calcular_periodo` nem define flag contrário à dobra (`app/ponto/motor_calculo_v2.py:3483`, `app/ponto/motor_calculo_v2.py:3491`, `app/ponto/motor_calculo_v2.py:3494`, `app/ponto/motor_calculo_v2.py:3495`).
- `MotorBase.eh_feriado` primeiro consulta `self.feriados`; ausente a data, chama `is_feriado(data)` sem passar local (`app/ponto/motor_calculo_v2.py:901`, `app/ponto/motor_calculo_v2.py:903`, `app/ponto/motor_calculo_v2.py:905`).
- `is_feriado(data, praca=None)` chama `core.feriados.eh_feriado(data, praca)` e devolve falso se ocorrer exceção (`app/ponto/motor_calculo_v2.py:30`, `app/ponto/motor_calculo_v2.py:40`, `app/ponto/motor_calculo_v2.py:42`).
- O juiz territorial admite nacional para todos, estadual somente com UF, municipal somente com UF e município correspondentes; sem município o fallback só reconhece nacional (`app/core/feriados.py:57`, `app/core/feriados.py:60`, `app/core/feriados.py:62`, `app/core/feriados.py:64`, `app/core/feriados.py:91`).
- Assim, **nacional cadastrado e consultável: reconhecido**; `_feriado_dobra_no_dia` resulta verdadeiro pelo flag e calendário e o `calcular_periodo` herdado escreve toda a duração líquida efetiva em `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, marcando `hora_extra_100=True` (`app/ponto/motor_calculo_v2.py:879`, `app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:2748`, `app/ponto/motor_calculo_v2.py:2749`, `app/ponto/motor_calculo_v2.py:2750`, `app/ponto/motor_calculo_v2.py:2751`).
- **Municipal isolado: não reconhecido**, pois o intermitente apagou o conjunto e o fallback não tem município; consequentemente a dobra de feriado é zero (`app/ponto/motor_calculo_v2.py:3495`, `app/ponto/motor_calculo_v2.py:905`, `app/core/feriados.py:64`, `app/core/feriados.py:91`, `app/ponto/motor_calculo_v2.py:879`).
- Não confundir “dobra municipal zero” com uma garantia absoluta de “HE100 total zero”: se existir jornada prevista positiva e excedente superior a duas horas, o ramo normal pode produzir HE100 pelo excedente (`app/ponto/motor_calculo_v2.py:2773`, `app/ponto/motor_calculo_v2.py:2778`, `app/ponto/motor_calculo_v2.py:2785`, `app/ponto/motor_calculo_v2.py:2791`, `app/ponto/motor_calculo_v2.py:2792`).
- No caso comum de intermitente sem previsão, `_prev_dia` falso faz o limite virar a própria duração e não há extra; o aumento de `jornada_minutos` do intermitente só é lido quando `_previsto_do_dia` devolve `None` (`app/ponto/motor_calculo_v2.py:2773`, `app/ponto/motor_calculo_v2.py:2778`, `app/ponto/motor_calculo_v2.py:1200`, `app/ponto/motor_calculo_v2.py:1211`, `app/ponto/motor_calculo_v2.py:1214`).
- O número é duração efetiva depois da janela e dos intervalos, não necessariamente a distância bruta entre batidas; o plantão aberto sem saída retorna antecipadamente (`app/ponto/motor_calculo_v2.py:2690`, `app/ponto/motor_calculo_v2.py:2697`, `app/ponto/motor_calculo_v2.py:2699`, `app/ponto/motor_calculo_v2.py:2702`, `app/ponto/motor_calculo_v2.py:2712`, `app/ponto/motor_calculo_v2.py:2723`).
- O dia consultado no `calcular_periodo` é `e.date()`; a política não exige “dia não previsto”, apesar do comentário envelhecido da linha 2745: o corpo efetivo é somente flag e calendário (`app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:879`).

**Ressalva indispensável se a resposta fala do fechamento completo**

Não se pode afirmar que “trabalho registrado em feriado nacional sempre resulta em W horas pagas” com os insumos fornecidos: após `calcular_periodo`, `calcular_mes` chama `_dia_sem_previsto_fora_da_janela` (`app/ponto/motor_calculo_v2.py:2396`, `app/ponto/motor_calculo_v2.py:2399`).

Essa função:
- passa quando é folga do par **ou** `self.eh_feriado(dia)`; exige empresa com janela ativa e data alcançada por `janela_he_desde`, previsto exatamente zero e ausência de decisão vigente `autorizado` (`app/ponto/motor_calculo_v2.py:1386`, `app/ponto/motor_calculo_v2.py:1387`, `app/ponto/motor_calculo_v2.py:1389`, `app/ponto/motor_calculo_v2.py:1391`, `app/ponto/motor_calculo_v2.py:1394`, `app/ponto/motor_calculo_v2.py:1327`, `app/ponto/motor_calculo_v2.py:1330`).
- Nesse caso transfere os minutos para fora da janela, zera trabalhadas, extras, inclusive `minutos_extra_100_feriado`, e colapsa o segmento efetivo; gera pendência até autorização (`app/ponto/motor_calculo_v2.py:1409`, `app/ponto/motor_calculo_v2.py:1410`, `app/ponto/motor_calculo_v2.py:1411`, `app/ponto/motor_calculo_v2.py:1414`, `app/ponto/motor_calculo_v2.py:1420`, `app/ponto/motor_calculo_v2.py:1423`).

Portanto a resposta precisa distinguir **regra bruta de dobra nacional = W** de **resultado final eventualmente zero por L-097**, que não implementa exclusão por ciclo intermitente (`app/ponto/motor_calculo_v2.py:2747`, `app/ponto/motor_calculo_v2.py:1387`, `app/ponto/motor_calculo_v2.py:1411`).

**Por que a fatia está no vínculo certo**

O fechamento usa `vinculo_do_dia` para atribuir o dono e percorre cada escala/fatia; nela usa `esc.tipo_escala`, monta seu motor e chama `calcular_mes` com aquele tipo e vínculo (`app/ponto/services/fechamento.py:188`, `app/ponto/services/fechamento.py:191`, `app/ponto/services/fechamento.py:203`, `app/ponto/services/fechamento.py:262`, `app/ponto/services/fechamento.py:263`, `app/ponto/services/fechamento.py:270`, `app/ponto/services/fechamento.py:272`, `app/ponto/services/fechamento.py:273`).

O construtor local do motor chama `get_motor_cct(colab, fatia_ini, tipo_escala=esc.tipo_escala, feriados=feriados, ...)`; o conjunto territorial foi montado com `feriados_de(lugar_do_colab(colab), ...)`, mas o intermitente o apaga depois (`app/ponto/services/fechamento.py:621`, `app/ponto/services/fechamento.py:622`, `app/ponto/services/fechamento.py:709`, `app/ponto/services/fechamento.py:711`, `app/ponto/services/fechamento.py:713`, `app/ponto/motor_calculo_v2.py:3495`).

O município usado nesse carregamento é o do `colaborador.posto`, não uma consulta direta ao posto do vínculo da fatia (`app/core/feriados.py:106`, `app/core/feriados.py:113`, `app/core/feriados.py:114`, `app/ponto/services/fechamento.py:622`).

**Escritores/leitores v2**

| Campo | Quem escreve/resolve | Quem lê |
|---|---|---|
| `feriados` | fechamento carrega território e injeta: `app/ponto/services/fechamento.py:622`, `:713`; base converte em `set`: `app/ponto/motor_calculo_v2.py:740`; intermitente sobrescreve vazio: `:3495` | `MotorBase.eh_feriado`: `app/ponto/motor_calculo_v2.py:903`; fallback `:905` |
| `feriado_em_dobra` | `get_motor` deriva do ciclo: `app/ponto/motor_calculo_v2.py:3607`; constante só 12x36: `:3537`; base grava booleano: `:737`; CCT pode conceder de volta para 12x36: `app/core/regua_cct.py:477`, `:478` | predicado efetivo: `app/ponto/motor_calculo_v2.py:879`; ramo de alerta: `:2756`; carimbo: `app/core/regua_cct.py:425`; fronteira v3: `app/ponto/calculador/nucleo.py:351` |
| `prorrogacao_noturna_pos5h` | `get_motor_cct` com switch desligado/CLT: `app/core/regua_cct.py:453`, `:454`; ligado: `:466`; base resolve ausência pela lei do ciclo e grava: `app/ponto/motor_calculo_v2.py:708`, `:710`, `:712`; lei retorna ciclo diferente de 12x36: `app/core/regua_cct.py:182` | noturnas: `app/ponto/motor_calculo_v2.py:922`; desconto de intervalo noturno: `:1866`; split HE noturna: `:955`; carimbo: `app/core/regua_cct.py:423`; fronteira v3 `app/ponto/calculador/nucleo.py:348` |
| `hora_reduzida_afastada` | default da fábrica falso: `app/ponto/motor_calculo_v2.py:3540`; CCT injeta só se cláusula, ciclo 12x36 e trava liberada: `app/core/regua_cct.py:470`; base grava: `app/ponto/motor_calculo_v2.py:719` | fator noturno: `app/ponto/motor_calculo_v2.py:930`; carimbo: `app/core/regua_cct.py:424`; fronteira v3: `app/ponto/calculador/nucleo.py:349` |
| lei da família | tabela nomeia `MotorIntermitente` e `Motor12x36` cru: `app/ponto/calculador/regras.py:487`, `:494`; função lê por identidade exata de classe: `:518` | v2 não chama essa função [não verificado como afirmação negativa absoluta]; concretamente intermitente herda `Motor12x36.calcular_periodo` (`app/ponto/motor_calculo_v2.py:3483`), que não chama `_julgar_pontualidade`; filha com escala chama: `:3461`, `:3471`; escritor comum: `:1950` |

**Dado útil sobre a v3 atual**

A árvore já contém a correção V3-RETOMA: `_cct_de(_te_dia)` usa template do dia, enquanto `_feriados` continua vindo diretamente do chamador; esse arranjo torna esperado que ambas variantes do chamador deem a mesma dobra nacional **e municipal**, porque o motor usado somente para flags do intermitente tem `feriado_em_dobra=True`, e o calendário efetivo da v3 não é o conjunto apagado nele (`app/ponto/calculador/alimentacao.py:392`, `app/ponto/calculador/alimentacao.py:403`, `app/ponto/calculador/alimentacao.py:407`, `app/ponto/calculador/alimentacao.py:507`, `app/ponto/calculador/alimentacao.py:510`, `app/ponto/calculador/alimentacao.py:524`).

## 31/10 12:44 · sessao c04dc1b063c1 · - · - · origem=Ronald (tmux)

Inventário de decisões de produção sobre dobra, com referências atuais da árvore:

| Sítio | Predicado / consequência |
|---|---|
| `app/ponto/motor_calculo_v2.py:3537` e `:3607-3608` | `CICLOS_FERIADO_SIMPLES={'12x36'}`; se não veio override, `feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`. Intermitente recebe `True`. |
| `app/core/regua_cct.py:440`, `:449-458`, `:477-479` | Com switch CCT desligado delega diretamente; ligado, somente ciclo literal `'12x36'` + cláusula `feriado_12x36_em_dobra` faz `setdefault('feriado_em_dobra', True)`. |
| `app/ponto/motor_calculo_v2.py:859-879` | `_feriado_dobra_no_dia(dia)` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`; não testa “previsto”. |
| `app/ponto/motor_calculo_v2.py:2747-2754` | `Motor12x36.calcular_periodo`: duração positiva + helper verdadeiro põe toda duração em `minutos_extra`, `minutos_extra_100`, `minutos_extra_100_feriado`. |
| `app/ponto/motor_calculo_v2.py:3030-3039` | Consolidação diária do TurnoPartido: trabalho positivo + helper verdadeiro põe o total diário em HE100 feriado e dá `continue`. |
| `app/ponto/motor_calculo_v2.py:3193-3200` | `MotorComercial.calcular_periodo`: duração positiva + helper verdadeiro põe toda duração em HE100 feriado. |
| `app/ponto/motor_calculo_v2.py:3275-3290` | Consolidação diária Comercial: trabalho positivo + helper verdadeiro põe total diário em HE100 feriado e dá `continue`. |
| `app/ponto/motor_calculo_v2.py:3483-3495` | `MotorIntermitente(Motor12x36)` conserva decisão herdada; `__init__` só amplia jornada e zera `self.feriados`, embora a docstring afirme “sem feriado-100 automático”. |
| `app/ponto/calculador/regras.py:287-295`, `:344-347` | `_ContextoFeriado` empresta exatamente `MotorBase._feriado_dobra_no_dia` e `eh_feriado`; `dobra_de_feriado_do_dia` retorna zero se trabalho não positivo, senão todo trabalho quando helper verdadeiro. |
| `app/ponto/calculador/regras.py:298-306` | `feriado_em_dobra_do_ciclo` responde `tipo_ciclo not in CICLOS_FERIADO_SIMPLES`. Busca de produção encontrou somente definição; a própria documentação do repo o registra órfão em `app/docs/BACKLOG.md:1018`. |
| `app/ponto/calculador/regras.py:775-789` | `do_dia`: havendo calendário, flag e dia, chama dobra; se positiva, HE100 total e feriado = dobra/60, HE50=0 e retorna antes do excedente. |
| `app/ponto/calculador/periodo.py:817-847` | Usa dia da jornada, soma trabalho dos pares, chama dobra com `cct.feriados` e `cct.feriado_em_dobra`; se positiva grava total em HE100 feriado no último período e HE50=0; caso contrário usa excedente. |
| `app/folha/export.py:330-349`, `:352-369`, `:409-432` | Export usa ciclo do vínculo no fim da janela e cláusula CCT no fim da janela; suprime parcela de feriado se `tipo_ciclo=='12x36' and not feriado_12x36_em_dobra`. Outros ciclos preservam parcela apurada. É decisão mensal de emissão, não consulta diária de calendário. |

Há também decisões sobre **folga causada pelo feriado**, que pagam HE100 feriado por outro caminho:

- `app/escala/models.py:32`, `:1357-1363`: `feriado_vira_folga` só aceita ciclos 6x1/5x2/semanal, vínculo que não trabalha em feriado, e calendário válido na praça do colaborador. Intermitente está fora do conjunto.
- `app/ponto/services/fechamento.py:909-931`: primeiro exige `_ft_certa`; identifica vínculo diário e pergunta `feriado_vira_folga`; soma horas desses períodos a HE100 total e feriado.
- `app/ponto/services/dia_pago.py:244-251`: grava a partição `ft_feriado` em HE100 total/feriado.
- `app/ponto/calculador/alimentacao.py:523`: forma `folga_e_feriado` exigindo vínculo diário, presença no calendário recebido e `feriado_vira_folga`.
- `app/ponto/calculador/regras.py:660-683`: somente `not intermitente`, com escala certa e dia, passa à decisão de folga; `_hora_feriado = bool(folga_e_feriado and _certa)` põe horas em HE100 total/feriado.

Calendário usado nessas decisões:

- `app/core/feriados.py:57-67`: nacional sempre alcança; estadual exige UF; municipal exige UF e município normalizado iguais.
- `app/core/feriados.py:91-95`: sem município, somente nacional.
- `app/core/feriados.py:106-114`: lugar vem de `colaborador.posto.municipio`.

Outro portão pode cancelar o pagamento apurado:

- `app/ponto/motor_calculo_v2.py:1386-1414`: `_dia_sem_previsto_fora_da_janela` exige folga ou feriado, empresa na janela, previsto exatamente zero e ausência de autorização; folga sem escala certa escapa; ao bloquear zera trabalho, noturno e extras inclusive `minutos_extra_100_feriado`. Por isso “há registro” sozinho não garante minutos positivos pagos.

`core/juizes.py`:

- Pergunta F3 é “feriado trabalhado paga dobra ou hora simples?” (`app/core/juizes.py:138`); juiz declarado é `ponto/motor_calculo_v2.py::CICLOS_FERIADO_SIMPLES` (`:260-262`).
- Pendências de F3 são seis entradas (`app/core/juizes.py:437-442`): intermitente zerando calendário e dobrando nacional pelo fallback; TurnoPartido/Comercial supostamente sem olhar flag; Comercial por período supostamente sem flag; supressão literal 12x36 no export; concessão literal 12x36 em CCT; campo manual `Ausencia.hora_extra_100`, descrito como sem leitor.
- As duas descrições de Comercial/TurnoPartido permanecem no catálogo, mas os corpos atuais já consultam `_feriado_dobra_no_dia` em `app/ponto/motor_calculo_v2.py:3030`, `:3193`, `:3275`. Não tratar a descrição histórica como comportamento atual.
- Campo manual recebe checkbox em `app/ponto/views.py:2381`, repassa ao criador em `:2405-2410`; o catálogo diz “gravado e lido por ninguém” (`app/core/juizes.py:442`). Esse campo não é a mesma variável que `PeriodoCalculo.hora_extra_100`.

O277:

- `app/docs/BACKLOG.md:1390` diz literalmente que o corte DP de 14/10 é **“SDF/intermitente em feriado NAO recebe dobra; o cartao nao afirma pagamento em dobro”**; revoga direção anterior da O264, passa a considerar código errado e docstring certa.
- Mesma linha estabelece orientação de 15/10: SDF depende cadastro da O275; intermitente segue docstring **sem entrada nova no frozenset**; cura deve incidir em `feriado_em_dobra`, não retirar `set()` nem alterar `eh_feriado`, e não usar “dia previsto não dobra”; registra “0 prod tocado”.
- O comportamento ainda contradiz essa regra nos caminhos que pagam: constante só exclui 12x36 (`app/ponto/motor_calculo_v2.py:3537`), fábrica dá `True` a intermitente (`:3607-3608`), classe não sobrescreve flag (`:3491-3495`), helper paga quando flag+calendário (`:879`). A v3 empresta esse helper e preserva a decisão (`app/ponto/calculador/regras.py:290-295`, `:344-347`). Zeros acidentais por calendário perdido não constituem implementação de O277.

## 31/10 12:44 · sessao 886c35c942b4 · GRADE-FOLGA · verificador · origem=Code

O diff está **incompleto**: pode esconder uma folga válida ao escolher a candidata, muda a ATA pela ordem dos overlays e não garante corrigir atas já julgadas. Além disso, o teste de calendário alheio não demonstra a regressão específica no legacy. As bases estão em `app/ponto/precedencia.py:234`, `app/escala/utils.py:1198`, `app/ponto/services/cartorio.py:1161` e `app/escala/utils.py:993`.

As referências abaixo são da árvore **ANTES**; referências ao teste novo indicam explicitamente o arquivo proposto no anexo. [nao verificado]

1. **A menor PK pode esconder a folga válida — e o diff leva essa lacuna à grade.**

   A precedência escolhe primeiro **uma única** FolgaDia por pessoa/data, pela menor PK; somente depois descarta calendário de outro vínculo. Entretanto, a unicidade é por **vínculo/data**, permitindo várias candidatas da mesma pessoa no dia. (`app/ponto/precedencia.py:234`, `:298`, `:313`; `app/escala/models.py:1571`.)

   Contraexemplo: dia cuja célula diz trabalho, calendário inválido do vínculo velho com PK menor e folga manual válida com PK maior. O carregador proposto guarda o calendário; o predicado o rejeita; a manual nunca é examinada. Antes, o overlay podia encontrar essa manual; depois, pode deixar o dia em trabalho. A mesma possibilidade existe com calendário válido do vínculo do dia ocultado pela candidata inválida. (`app/ponto/precedencia.py:234`, `:313`, `:316`; `app/escala/models.py:1071`; `app/escala/utils.py:1109`.)

   Portanto, compartilhar o carregador **não basta**: é necessário resolver o conjunto das candidatas válidas, com regra explícita para coexistência. A existência desse contraexemplo em produção é **[nao verificado]**.

2. **A troca de ordem muda resultado observável, inclusive lâmpadas da ATA.**

   Antes há `folga(e1) → ausência(e1) → folga(e2) → ausência(e2)`. A ausência consulta a mesma pessoa, mas pode deixar marcas que uma folga posterior não remove. Os vínculos são percorridos por `-data_inicio`. (`app/escala/utils.py:1785`, `:1128`; `app/escala/models.py:1003`.)

   Exemplo: dia inicialmente de trabalho, justificativa aprovada e folga somente no segundo vínculo percorrido. A primeira ausência marca `justificado=True`; a folga posterior limpa células/órfãs, preservando essa marca. Com todas as folgas primeiro, nenhuma passagem marca a justificativa, pois o dia já é folga. (`app/escala/utils.py:1198`, `:1117`.)

   | Resultado desse cenário | Antes | Depois |
   |---|---|---|
   | `tipo_dia/celulas/orfas` | `folga/[None,None]/[]` | Igual |
   | `justificado` | `True` | Ausente |
   | Duas lâmpadas reconstruídas do DNA | Dispensadas por justificativa | Apagadas |
   | `n_missing` da ATA | 0 | 2 |

   A reconstrução das lâmpadas consulta o DNA mesmo com células vazias; `dispensar_por_ausencia` lê `justificado` para dispensá-las. Esse cenário foi reproduzido em memória com as funções da cópia e consultas dubladas. (`app/ponto/services/cartorio.py:470`, `:509`, `:559`, `:612`.)

   Com falta aprovada também há diferença de dicionário: antes podem sobrar `falta_aprovada=True` e `ausencia_id` após uma folga posterior; depois, a falta já encontra folga e não escreve essas chaves. (`app/escala/utils.py:1170`, `:1174`, `:1117`.)

3. **O vínculo coincide nos dias devolvidos pelos dois builders, com duas ressalvas.**

   No caminho padrão, a precedência carrega células, vínculos da janela e geradoras faltantes; os dois builders carregam todos os vínculos da pessoa e perguntam à mesma `vinculo_do_dia`. Sem geradora, o desempate usa `(ativa, data_inicio)`; não há empate de início para a mesma pessoa pela restrição do modelo. (`app/ponto/precedencia.py:271`, `:275`, `:283`, `:307`; `app/escala/utils.py:964`, `:968`, `:970`, `:1240`, `:1252`, `:1254`; `app/escala/alimentacao.py:253`; `app/escala/models.py:1004`.)

   - **Por turno:** `esc_por_dia[d]` é diretamente a resposta da juíza; inclusive `None`. Geradora fora da vigência produz `None`, sem escolher outro vínculo como reserva. (`app/escala/utils.py:1260`; `app/escala/alimentacao.py:247`.)
   - **Legacy, dias retidos:** o segmento é formado pela juíza; a `lambda: esc` proposta entrega esse mesmo vínculo nos dias conservados. (`app/escala/utils.py:974`, `:983`, `:997`.)
   - **Legacy, dia adicional:** o single recebe também `d0−1`. Nesse dia, a lambda pode responder outro vínculo: no exemplo do anexo, o segmento novo passa **novo em 09/10**, enquanto a juíza responde velho. Esse resultado é descartado pelo recorte. Logo a afirmação literal “mesmo vínculo em todo dia montado” é falsa, embora os dias retidos coincidam. (`app/escala/utils.py:993`, `:995`, `:997`.)
   - **Legacy, vínculo `None`:** fabrica diretamente uma linha folga e não chama o predicado. O builder por turno passa `None` ao predicado proposto. Portanto, não são duas consultas equivalentes nesse ramo; o rótulo legacy já nasce folga. (`app/escala/utils.py:985`, `:987`, `:1260`.)

4. **A mudança de universo atua nas duas direções; o teste não cobre todas.** [nao verificado]

   Antes, por turno, entram as FolgaDia dos vínculos pertencentes a `escs_janela_ids`, aplicadas pela janela inteira. O helper proposto passa a consultar qualquer vínculo da pessoa, reproduzindo o universo atual da precedência. (`app/escala/utils.py:1785`, `:1109`; `app/ponto/precedencia.py:236`.)

   | Situação, mantendo os demais fatos | Direção possível |
   |---|---|
   | Manual de vínculo ausente de `escs_janela_ids` | Trabalho → folga: passa a ser encontrada. |
   | Calendário de outro vínculo presente na janela | Folga → trabalho: deixa de valer pelo filtro. |
   | Calendário do vínculo do dia | Continua elegível. |
   | Candidata inválida de PK menor escondendo manual/calendário válido | Pode perder folga válida, conforme achado 1. |

   Essas direções decorrem do filtro anterior por vínculo, do universo por pessoa e do descarte posterior da candidata; não demonstram quantidade de dias reais atingidos. (`app/escala/utils.py:1109`, `:1116`, `:1786`; `app/ponto/precedencia.py:234`, `:313`.) Quantidades em produção: **[nao verificado]**.

   O teste chamado `test_MORDE_folga_de_calendario_de_outro_vinculo_e_trabalho_na_grade` **já passa no legacy ANTES**: em 15/10, o single do segmento novo consulta somente FolgaDia do novo. É o teste da **manual presa ao velho** que detecta mudança no legacy. Assim, a bateria tem mordida nesse builder, mas não comprova a regressão específica anunciada pelo primeiro teste nos dois caminhos. (`app/escala/utils.py:993`, `:997`, `:1109`; teste proposto `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:62`, `:67`.)

   Faltam casos para candidatas coexistentes, vínculo antigo fora da janela, ausência/justificativa na troca de ordem e ATA/chamado resultantes; os três testes propostos só verificam `tipo_dia` e faixa da precedência. (Teste proposto `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:48`, `:62`, `:67`, `:72`.) RED/verde da suíte nova executada: **[nao verificado]**.

5. **Quem escreve e quem lê os campos envolvidos.**

   | Campo/valor | Escritores | Leitores relevantes |
   |---|---|---|
   | `FolgaDia.calendario_origem_id` não nulo | Gerador grava `ec.folga_calendario`. (`app/escala/folgas.py:157`, `:174`.) | Reconciliação/proteção das manuais, guardas das portas, reparentamento e precedência. (`app/escala/folgas.py:101`, `:165`, `:255`, `:275`, `:405`; `app/ponto/precedencia.py:313`.) |
   | Origem nula/manual | `lancar_folga` e `_foto_do_dia` criam sem calendário. A FK também declara `SET_NULL` na exclusão do calendário. (`app/escala/folgas.py:259`, `:284`; `app/escala/models.py:1557`.) | Os mesmos leitores de proveniência, além do toggle e declaração de dia. (`app/escala/views_plano_folgas.py:112`; `app/ponto/services/declarar_dia.py:108`.) |
   | `FolgaDia.escala_colaborador_id` | Gerador e portas manuais gravam `ec`; reparentamento troca a FK da manual, recusando calendário. (`app/escala/folgas.py:174`, `:259`, `:284`, `:405`, `:435`.) | Alimentação, cálculo da foto, overlay, precedência, plano, diagnóstico e conflito de fase. (`app/escala/alimentacao.py:66`; `app/escala/models.py:1251`, `:1280`, `:1327`; `app/escala/utils.py:1109`; `app/ponto/precedencia.py:236`, `:314`; `app/escala/views_plano_folgas.py:60`; `app/ponto/services/diagnostico_escala.py:102`; `app/escala/services/fase_conflitante.py:74`.) |
   | Vínculo usado no dia | Juíza seleciona; por turno materializa `esc_por_dia`; legacy forma segmentos e passa `esc` ao single. (`app/escala/alimentacao.py:237`; `app/escala/utils.py:1260`, `:974`, `:993`.) | Montagem de marcos/trabalho e, no diff, o predicado de folga. A precedência lê sua própria chamada à mesma juíza. (`app/escala/utils.py:1297`, `:1471`; `app/ponto/precedencia.py:307`.) |
   | `tipo_dia/celulas/orfas` | Builders escrevem a base; folga limpa os três; falta/ausência inteira também; parcial preserva células/órfãs; justificativa limpa missing/órfãs; isenção altera células. (`app/escala/utils.py:727`, `:752`, `:1743`, `:1749`, `:1117`, `:1174`, `:1180`, `:1195`, `:1198`, `:809`.) | Legacy remapeia; conservação e reconciliação alteram células/órfãs; adaptador, cartório e leitor da ATA transportam o resultado. (`app/escala/utils.py:1014`, `:1766`, `:1979`, `:1059`; `app/ponto/services/cartorio.py:564`; `app/escala/services/leitor_celula.py:344`, `:359`.) |

   A porta registrada de FolgaDia é `escala/folgas.py`; o diff altera leitura e dicionários, sem acrescentar escritor persistente dessa entidade. (`app/core/portas.py:466`; trechos propostos sobre `app/escala/utils.py:1101` e `app/ponto/precedencia.py:234`.)

   Para os leitores de `tipo_dia`, abaixo **F→T** significa folga antes e trabalho depois; **T→F** inverte a elegibilidade indicada. Leitores da ATA só recebem a mudança após nova lavratura, pois o leitor recupera o tipo gravado. (`app/escala/services/leitor_celula.py:344`.)

   | Leitor | Comportamento condicionado à mudança |
   |---|---|
   | `app/escala/utils.py:850` | F→T permite marcar isenção do dia, havendo células. |
   | `app/escala/utils.py:1170`, `:1198` | F→T permite falta aprovada e justificativa; ausência não-falta continua podendo cobrir ambos. |
   | `app/escala/utils.py:1016` | Apenas transporta o tipo do segmento. |
   | `app/ponto/services/cartorio.py:567`, `:997` | Grava o tipo na ATA; folga↔trabalho isoladamente não altera o teste de cobertura integral por ausência/falta. |
   | `app/escala/services/leitor_celula.py:344`, `:384` | Transporta o tipo gravado; folga↔trabalho não altera a seleção específica de ausência parcial. |
   | `app/chamados/juizes.py:408` | F→T deixa de declarar “dia sem trabalho” pela ATA; continua declarando se `cel.trabalha is False`. |
   | `app/ponto/supra_juiz.py:258`, `:281`, `:379`, `:396`, `:420`, `:443`, `:448`, `:463` | F→T habilita ramos de cobrança e inconsistência de trabalho; pode desabilitar cobrança em dia sem trabalho. Os demais requisitos permanecem. |
   | `app/ponto/status_dia.py:96`, `:106`, `:115`, `:146` | Muda os fatos usados para lavrar status e os ramos folga/folga trabalhada, conforme batida, previsto e veredito. |
   | `app/ponto/services/espelho.py:690` | F→T desliga `_folga`. |
   | `app/relatorios/furos.py:147`, `:151`, `:218`, `:237` | F→T inclui o dia na apuração de furos, conforme células/âncora; sentinelas não mudam. |
   | `app/inteligencia/calculadores.py:46`, `:141` | F→T inclui o dia nas métricas de trabalho. |
   | `app/escala/services/furos_diarios.py:39` | F→T permite coletar furos desse dia. |
   | `app/folha/causas.py:40`, `:41`, `:100` | F→T inclui em dias previstos e retira de folga com órfãs. |
   | `app/folha/export.py:1071`, `:1075`, `:1080` | F→T inclui no universo de previsto/realizado limitado por dia; não cria abono, que exige ausência. |
   | `app/ponto/services/dia_pago.py:386`, `:393`, `:398` | F→T inclui nos mapas previsto/realizado. T→F exclui mesmo que `minutos_previstos` permaneça positivo. |
   | `app/ponto/services/fechamento.py:1011`, `:1015` | F→T inclui na contagem de dias previstos; não muda a detecção de ausência de células. |
   | `app/escala/orfa_classe.py:65` | F→T deixa a classificação automática de folga e segue a classificação de trabalho, se ainda houver órfãs. |
   | `app/ponto/propositor.py:36`, `:210` | F→T permite avaliar degeneração/deslocamento. |
   | `app/ponto/services/simular_dia.py:12` | F→T permite considerar o dia perfeito, conforme células/órfãs. |
   | `app/ponto/worklist.py:19` | F→T deixa a saída “dia de folga” e permite examinar lâmpadas. |
   | `app/chamados/services/declaracao_texto.py:91`, `:95` | F→T permite oferecer marcos faltantes; sentinelas não mudam. |
   | `app/chamados/services/disputa_emissao.py:68` | F→T permite devolver a data de trabalho para emissão. |
   | `app/chamados/services/materializacao.py:223` | O teste de tipo aceita tanto folga quanto trabalho: não muda por essa troca. |
   | `app/ponto/services/veredito_celula.py:221` | F→T permite materializar trabalho, sujeitos os demais requisitos. |
   | `app/ponto/services/bordas_realizado.py:74` | F→T inclui na auditoria das bordas do realizado. |
   | `app/ponto/management/commands/tripwire_celulas.py:342` | F→T retira da checagem específica de ATA folga sem lâmpadas. |
   | `app/ponto/management/commands/mapa_divergencia.py:131`, `:143`, `:155`, `:228` | A assinatura/comparação passa a refletir o novo tipo. |

6. **Classes de reprovação e outros sítios que ainda discordam.**

   - **Segundo juiz registrado:** a extração proposta do predicado não cria outro vencedor da precedência nem outro seletor de vínculo: essas autoridades continuam sendo `fatos_do_dia` e `vinculo_do_dia`. Há, porém, outros leitores com regras diferentes, descritos abaixo. (`app/core/juizes.py:160`, `:1255`.)
   - **Segundo escritor fora da porta, relógio novo ou inversão de travas:** não aparecem nos trechos propostos; eles consultam dados e modificam a grade em memória. A porta persistente de FolgaDia continua registrada em `escala/folgas.py`. (`app/core/portas.py:466`; alterações propostas sobre `app/escala/utils.py:1101`, `:1785` e `app/ponto/precedencia.py:234`.)
   - **Selo sem mordida suficiente:** ocorre a limitação do teste legacy já descrita; também falta testar a ATA afetada pela ordem. (Teste proposto `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:62`; `app/escala/utils.py:993`; `app/ponto/services/cartorio.py:612`.)
   - **Ativação incompleta:** o diff não altera `REGRA_VERSOES`, e o hash não inclui `tipo_dia/celulas/orfas`. Dia já julgado com impressão igual pode ser pulado, preservando a ATA antiga. É necessário um caminho explícito de atualização — versão/horizonte ou rejulgamento forçado apropriado. (`app/ponto/services/cartorio.py:110`, `:160`, `:324`, `:1148`, `:1157`, `:1161`.) Sua execução ou pouso consta como **[nao verificado]**.

   **Há outro sítio alimentando diretamente o mesmo chamado:** `o_dia_ja_e_folga` busca somente FolgaDia do vínculo do dia. Uma manual presa ao velho vale pelo predicado proposto, mas não é reconhecida ali. Esse booleano decide o caminho que carimba/fecha a pergunta e o fio. (`app/ponto/services/declarar_dia.py:61`, `:90`; `app/chamados/services/validacao.py:261`, `:271`.)

   Se esse reconhecimento falha, a validação vai para `checar_declaracao`; o veto consulta **qualquer FolgaDia da pessoa/data**, sem validar proveniência/vínculo. Assim, calendário antigo que o novo predicado rejeita ainda pode impedir um lançamento por “já tem FOLGA”. É divergência preexistente deixada aberta. (`app/chamados/services/validacao.py:280`; `app/ponto/services/declarar_dia.py:114`; `app/ponto/precedencia.py:161`, `:458`.)

   **Também permanece uma alimentação paralela no calendário:** passa `{data: True}` somente da foto do vínculo carregado. Isso elimina proveniência e vínculo; o novo predicado trata `True` como manual. Se a juíza escolhe outra geradora, esse calendário não é descartado; uma manual em outro vínculo também fica fora dessa alimentação. (`app/colaboradores/services/calendario.py:592`, `:604`, `:607`; `app/ponto/precedencia.py:283`, `:313`.) Esse caminho demonstra divergência no calendário; por si só não demonstra escrita da mesma ATA/chamado.

   A foto por vínculo ainda alimenta o cálculo que gera `trabalha`/DNA; ela não equivale ao universo “manual da pessoa” do predicado proposto. A mesma ATA recebe depois a precedência por outra leitura. Portanto, igualar apenas o overlay não prova convergência de toda a cadeia. (`app/escala/models.py:1251`, `:1280`; `app/ponto/portas/celula.py:243`, `:590`; `app/ponto/services/cartorio.py:1673`.)

Os **11 casos**, sua distribuição e seu desaparecimento após a cura não são comprováveis pelo material fornecido: **[nao verificado]**.

## 31/10 12:43 · sessao 47ffd0481ef0 · - · - · origem=Ronald (tmux)

**Resultado B:** com vínculo do dia resolvido como intermitente, **ambos os chamadores — 12x36 e intermitente — produzem `W/60` em `horas_extras_100` e `horas_extras_100_feriado`**, tanto no nacional quanto no municipal da praça presente no conjunto recebido; `W` são os minutos trabalhados usados pela v3. A escolha do template do dia está em `app/ponto/calculador/alimentacao.py:490–507`; o flag de dobra vem desse template em `:510`; o conjunto de feriados, separadamente, vem do argumento do chamador em `:407/:524`; a escrita das duas rubricas está em `app/ponto/calculador/regras.py:775–789`.

- `_te_dia` começa como template do chamador (`alimentacao.py:487`), mas o vínculo do dia o substitui, junto com `_interm_dia`, em `:490–497`; `_cct_de(_te_dia)` roda em `:507`. Portanto, identificado o vínculo intermitente, a variante do chamador não altera esses flags (`app/ponto/calculador/alimentacao.py:490–510`).
- `_cct_de` constrói motor com `get_motor_cct(c, ini, tipo_escala=_te)` (`app/ponto/calculador/alimentacao.py:387–404`); o ciclo intermitente seleciona `MotorIntermitente` (`app/ponto/motor_calculo_v2.py:3563–3564`); só `12x36` pertence a `CICLOS_FERIADO_SIMPLES` (`:3537`), então o flag é `True` (`:3607–3608`).
- **A diferença decisiva de calendário:** `MotorIntermitente` zera `self.feriados` (`app/ponto/motor_calculo_v2.py:3495`), e `cct_do_motor` copia esse vazio (`app/ponto/calculador/nucleo.py:350`), **mas `rubricas_por_dia` não usa `_cct.feriados` na dobra**: usa `_feriados = set(feriados or ())` do chamador (`app/ponto/calculador/alimentacao.py:407`) e o passa em `:524`.
- `_ContextoFeriado` volta a escrever esse conjunto e o flag em seu próprio contexto (`app/ponto/calculador/regras.py:293–295`), emprestando `MotorBase.eh_feriado` e `_feriado_dobra_no_dia` (`:290–291`). A regra retorna todo `W` se `W>0` e o predicado for verdadeiro (`:344–347`).
- O predicado emprestado é `bool(self.feriado_em_dobra) and self.eh_feriado(dia)` (`app/ponto/motor_calculo_v2.py:879`); `eh_feriado` aceita a data injetada ou consulta `is_feriado(data)` (`:903–905`). O fallback, sem município, só alcança nacional (`app/ponto/motor_calculo_v2.py:30–43`; `app/core/feriados.py:91–95/:57–67`).
- Se o municipal **não** veio no conjunto recebido, não é recuperado pelo fallback sem município (`app/ponto/motor_calculo_v2.py:905/:30–41`; `app/core/feriados.py:57–67/:91–95`). No fechamento o conjunto é montado pelo município do posto e entregue à v3 (`app/ponto/services/fechamento.py:621–623/:1239–1243`; `app/core/feriados.py:106–114`).
- Intermitente não é desviado pelo ramo da folga: mesmo quando `dia_de_trabalho=False`, passa adiante (`app/ponto/calculador/regras.py:646–660/:736–740`); portanto não cai na outra conversão `folga_e_feriado` (`:676–735`).

**Escritores/leitores na v3:**

| Campo | Quem escreve / fornece | Quem lê |
|---|---|---|
| `feriados` | Motor escreve conjunto em `app/ponto/motor_calculo_v2.py:740`, intermitente zera em `:3495`; fronteira copia em `app/ponto/calculador/nucleo.py:350`. Na porta por dia, entretanto, `app/ponto/calculador/alimentacao.py:407/:524` usa o argumento do chamador; contexto o grava em `app/ponto/calculador/regras.py:294`. | Fronteira verifica existência/None em `app/ponto/calculador/nucleo.py:344–350`; regra chama a dobra em `app/ponto/calculador/regras.py:775–777`; método emprestado lê conjunto em `app/ponto/motor_calculo_v2.py:903`. A releitura alternativa usa **`cct.feriados`** em `app/ponto/calculador/periodo.py:830`. |
| `feriado_em_dobra` | Fábrica resolve pelo ciclo em `app/ponto/motor_calculo_v2.py:3607–3608`, motor grava em `:737`; fronteira copia em `app/ponto/calculador/nucleo.py:351`; alimentação lê em `app/ponto/calculador/alimentacao.py:510` e passa em `:524`; contexto grava em `app/ponto/calculador/regras.py:295`. | `app/ponto/motor_calculo_v2.py:879`, emprestado pela v3 em `app/ponto/calculador/regras.py:290`; `do_dia` exige ambos os dados em `:633–638/:775`; releitura lê em `app/ponto/calculador/periodo.py:831`. |
| `prorrogacao_noturna_pos5h` | `get_motor_cct`: `app/core/regua_cct.py:454/:466`; ausência de valor é resolvida no motor em `app/ponto/motor_calculo_v2.py:708–712`. Fronteira renomeia para `prorrogacao_pos5h` em `app/ponto/calculador/nucleo.py:348`; alimentação lê/passa em `app/ponto/calculador/alimentacao.py:508/:530`; contexto noturno grava em `app/ponto/calculador/regras.py:61`. | `app/ponto/calculador/regras.py:609/:627/:85–86/:815–824`; contexto empresta método em `:63`; releitura passa `cct.prorrogacao_pos5h` em `app/ponto/calculador/periodo.py:508`. |
| `hora_reduzida_afastada` | CCT resolve apenas para ciclo 12x36 em `app/core/regua_cct.py:470–472`; motor grava em `app/ponto/motor_calculo_v2.py:719`; fronteira copia em `app/ponto/calculador/nucleo.py:349`; alimentação lê/passa em `app/ponto/calculador/alimentacao.py:509/:530`. | Guarda em `app/ponto/calculador/regras.py:609`; fator noturno em `:88`; releitura passa o flag em `app/ponto/calculador/periodo.py:509`. |
| Lei da família | Declaração nominal das identidades de classe em `app/ponto/calculador/regras.py:487–498`, incluindo `MotorIntermitente` em `:494–497`; leitor único faz lookup exato por `type(motor)` em `:516–518`; alimentação consulta em `app/ponto/calculador/alimentacao.py:403` e passa causa em `:531`. | `do_dia` lê causa e declara atraso/antecipada, descartando pontualidade recebida, em `app/ponto/calculador/regras.py:618–625`; releitura recebe causa e a passa em `app/ponto/calculador/periodo.py:783–789`; outro consumidor é `folha_julga_pontualidade` em `app/chamados/juizes.py:1529/:1545–1551`. |

**Outro sítio relevante:** `periodo.reler` usa `cct.feriados`, e não o calendário do chamador da alimentação (`app/ponto/calculador/periodo.py:829–831`). Com `cct_do_motor(MotorIntermitente)`, recebe o conjunto zerado (`app/ponto/motor_calculo_v2.py:3495`; `app/ponto/calculador/nucleo.py:350`): nacional pode dobrar pelo fallback, municipal sozinho não; quando dobra, grava toda a jornada no último período em `app/ponto/calculador/periodo.py:833–842`, pelo mesmo predicado `app/ponto/motor_calculo_v2.py:879/:903–905`.

## 31/10 12:41 · sessao 8e16097b8d6d · - · - · origem=Ronald (tmux)

**Troca de ordem muda resultado, inclusive ATA.** Provei em memória com `python3 -B`, extraindo por AST os overlays e funções de ATA reais, dublando somente consultas/imports. Nenhum arquivo ou banco foi escrito. Fontes executadas: `app/escala/utils.py:1101`, `app/escala/utils.py:1123`, `app/ponto/services/cartorio.py:428`, `app/ponto/services/cartorio.py:559`, `app/ponto/services/cartorio.py:594`.

Caso: dois vínculos em `rows`, folga somente no segundo, justificativa aprovada para um dia inicialmente de trabalho. Antes: `A1 → B1 → A2 → B2`; B1 marca `justificado=True`, A2 limpa células/órfãs e põe folga, mas preserva a marca; B2 não remove. Depois: folgas primeiro, e nenhuma ausência marca justificativa porque o dia já é folga. Fontes: `app/escala/utils.py:1785`, `app/escala/utils.py:1117`, `app/escala/utils.py:1198`.

Resultado medido no cenário sintético:

| Resultado | Antes | Depois |
|---|---|---|
| `tipo_dia/celulas/orfas` | `folga/[None,None]/[]` | igual |
| `justificado` | `True` | ausente |
| ATA, duas lâmpadas do DNA | acesas, `dispensada='justificativa'` | apagadas |
| `n_missing` da ATA | 0 | 2 |

As lâmpadas são reconstruídas do DNA mesmo com células `None`; depois o cartório lê `justificado` para dispensá-las. Portanto, há efeito além de metadado sem leitor: `app/ponto/services/cartorio.py:448`, `app/ponto/services/cartorio.py:470`, `app/ponto/services/cartorio.py:509`, `app/ponto/services/cartorio.py:612`.

Com falta aprovada também muda o dict: antes B1 grava `falta_aprovada=True/ausencia_id`, A2 mantém essas chaves ao trocar para folga; depois todos os B pulam a falta em folga e as chaves não aparecem. Não localizei leitor vivo de `dia['falta_aprovada']`; a mudança de dict está demonstrada pelo código. Fontes: `app/escala/utils.py:1170`, `app/escala/utils.py:1174`, `app/escala/utils.py:1117`.

**Escritores e transporte de `dias_out`:**

- Builder simples escreve folga/trabalho, células e órfãs; depois conserva sobras e aplica overlays: `app/escala/utils.py:725`, `app/escala/utils.py:727`, `app/escala/utils.py:741`, `app/escala/utils.py:752`, `app/escala/utils.py:763`, `app/escala/utils.py:768`.
- Legacy fabrica folga para segmento sem vínculo; incorpora o builder simples e remapeia os três campos à régua final: `app/escala/utils.py:985`, `app/escala/utils.py:993`, `app/escala/utils.py:1014`, `app/escala/utils.py:1016`.
- Por turno escreve folga/trabalho, células, órfãs, recolhe sobras/intervalos; depois overlays e reconciliação de pontas alteram células/órfãs: `app/escala/utils.py:1743`, `app/escala/utils.py:1749`, `app/escala/utils.py:1766`, `app/escala/utils.py:1785`, `app/escala/utils.py:1979`.
- Folga escreve os três; ausência inteira/falta também; ausência parcial mantém células/órfãs; justificativa limpa missing/órfãs sem mudar tipo; isenção modifica células e usa trabalho para marcar o dia: `app/escala/utils.py:1117`, `app/escala/utils.py:1174`, `app/escala/utils.py:1180`, `app/escala/utils.py:1195`, `app/escala/utils.py:1198`, `app/escala/utils.py:809`, `app/escala/utils.py:850`.
- Cartório lê os três e grava tipo, lâmpadas, contagens e órfãs na ATA; leitor da célula recupera a ATA e entrega nova grade aos consumidores: `app/ponto/services/cartorio.py:564`, `app/ponto/services/cartorio.py:567`, `app/ponto/services/cartorio.py:574`, `app/escala/services/leitor_celula.py:344`, `app/escala/services/leitor_celula.py:359`.

**Censo dos leitores de `tipo_dia` e efeito condicional do diff.** Considere F→T = folga antes, trabalho depois; T→F = inverso. Nos leitores da ATA, o efeito depende de nova lavratura; a leitura não remonta automaticamente o dia: `app/escala/services/leitor_celula.py:331`, `app/escala/services/leitor_celula.py:344`.

| Leitor | Efeito de F→T; T→F faz o inverso salvo observação |
|---|---|
| `app/escala/utils.py:850` | Passa a permitir marca de isenção do dia, havendo células dict. |
| `app/escala/utils.py:1170` | Falta aprovada passa a poder virar `falta`; não-falta vence ambas. |
| `app/escala/utils.py:1198` | Justificativa passa a ser aplicada. |
| `app/escala/utils.py:1016` | Só propaga tipo do segmento legacy. |
| `app/ponto/services/cartorio.py:567` | Lavra o novo tipo. |
| `app/ponto/services/cartorio.py:997` | F→T isolado não muda o teste ausência/falta; eventual conversão subsequente em falta muda cobertura integral. |
| `app/escala/services/leitor_celula.py:344` | Só propaga tipo da ATA; fallback `cel.trabalha` fica igual. |
| `app/escala/services/leitor_celula.py:384` | Folga↔trabalho isoladamente não altera alvo de cobertura parcial, que exige ausência. |
| `app/chamados/juizes.py:408` | Deixa de declarar dia sem trabalho **se** `cel.trabalha is not False`; se for False continua sem trabalho independentemente da ATA. |
| `app/ponto/supra_juiz.py:258`, `:281`, `:379`, `:396`, `:420`, `:443`, `:448`, `:463` | Reabre ramos de cobrança/realizado/orfandade de trabalho conforme demais gates; deixa o ramo de cobrança em dia sem trabalho, que também exige previsto zero. |
| `app/ponto/status_dia.py:96`, `:106`, `:115`, `:146` | Deixa os rótulos folga/folga trabalhada condicionados ao tipo, com demais gates preservados; tipo vai aos fatos do status. |
| `app/ponto/services/espelho.py:690` | Flag visual `_folga` fica False. |
| `app/relatorios/furos.py:147`, `:151`, `:218`, `:237` | F→T entra na apuração de furos, sujeito a sem_ancora e células; sentinelas fora_vinculo/sem_celula não mudam. |
| `app/inteligencia/calculadores.py:46`, `:141` | Entra em numerador/denominador das métricas conforme células/limbo. |
| `app/escala/services/furos_diarios.py:39` | Entra na coleta de furos diários, sujeito a previsto e janela. |
| `app/folha/causas.py:40`, `:41`, `:100` | Entra em dias previstos/furados e deixa conjunto folga com órfãs; diagnóstico pode mudar. |
| `app/folha/export.py:1071`, `:1075`, `:1080` | Entra no universo trabalho+ausência, previsto e realizado com cap passam a contribuir; não vira abono porque exige ausência. |
| `app/ponto/services/dia_pago.py:386`, `:393`, `:398` | Entra nos mapas previsto/realizado com cap; não vira abono porque exige ausência. **T→F exclui o dia mesmo se `minutos_previstos` continuar positivo.** |
| `app/ponto/services/fechamento.py:1011`, `:1015` | Folga↔trabalho não altera abstinência sem_celula; F→T entra na contagem/soma dos dias previstos. |
| `app/escala/orfa_classe.py:65` | Órfãs deixam a classificação automática `FOLGA_DESMENTIDA` e seguem heurística de trabalho. Overlay que apague órfãs torna retorno vazio antes deste teste. |
| `app/ponto/propositor.py:36`, `:210` | Dia passa a poder ser avaliado como degenerado e a contribuir para suspeita de deslocamento. |
| `app/ponto/services/simular_dia.py:12` | Dia passa a poder ser considerado perfeito, respeitando órfãs/missing/tipos. |
| `app/ponto/worklist.py:19` | Sai de “dia de folga”/sem classificação e passa a verificar lâmpadas. |
| `app/chamados/services/declaracao_texto.py:91`, `:95` | Passa a oferecer marcos missing cobrados; sentinelas não mudam. |
| `app/chamados/services/disputa_emissao.py:68` | Passa a devolver a data de trabalho para emissão da disputa. |
| `app/chamados/services/materializacao.py:223` | Teste do tipo não muda: aceita tanto folga quanto trabalho; somente sentinelas são barradas. Marcos podem mudar via células/ATA. |
| `app/ponto/services/veredito_celula.py:221` | Passa a permitir materializar trabalho, sujeito aos demais requisitos. |
| `app/ponto/services/bordas_realizado.py:74` | Entra na auditoria dos limites do realizado. |
| `app/ponto/management/commands/tripwire_celulas.py:342` | Sai da checagem específica de ATA folga sem lâmpadas. |
| `app/ponto/management/commands/mapa_divergencia.py:131`, `:143`, `:155`, `:228` | Assinatura e causa da divergência passam a refletir novo tipo. |

`app/colaboradores/services/calendario.py:794` lê `tipo_do_dia` da precedência, não `tipo_dia` da grade; `e6_piso_autopsia` apenas menciona o campo em comentário e usa a autoridade do previsto: `app/ponto/management/commands/e6_piso_autopsia.py:99`, `app/ponto/management/commands/e6_piso_autopsia.py:104`.

## 31/10 12:41 · sessao 516c9bd1170f · REPROVA-E-DESENHO-B3 · verificador · origem=Code

**REPROVA como “muda só o limiar”.** Trocar `3 → 1` antecipa o disparo, mas não implementa liberação pela linha, exceção por erro bobo ou a medição prometida. O Stop também tem implementação própria fora de `disjuntor.py`. (`bin/disjuntor.py:574`, `bin/disjuntor.py:950`, `bin/hook_stop_fila1.py:1050`)

1. **Escrever a linha não libera o próximo verificador.** O Stop deixa de cobrar quando reconhece a pergunta no topo; o PreToolUse não consulta o RELATO: continua negando enquanto `disparado` estiver ativo. Quem limpa esse estado hoje é `rearma_b3`. Portanto “até a linha existir” muda o mecanismo de destravamento. (`bin/hook_stop_fila1.py:1051`, `bin/disjuntor.py:947–954`, `bin/disjuntor.py:592–593`)

2. **O reconhecedor não exige o conteúdo proposto.** Basta `DESENHO` em maiúsculas e a identidade da obra na mesma linha. Não verifica juiz único, escritor único, célula soberana, lâmpada 4a, pergunta sem juiz ou `arquivo:linha`. Só considera as duas primeiras linhas úteis; o selo aceita até `DESENHO OBRA-X (C)`. (`bin/hook_stop_fila1.py:417`, `bin/hook_stop_fila1.py:420–440`, `bin/tests/test_disjuntor.sh:654–655`)
   
   Também não há vínculo com o disparo atual, data ou consumo da declaração. Uma linha pode nomear várias obras; o problema de citar outra obra na mesma linha e calá-la já está registrado. (`bin/hook_stop_fila1.py:417`, `bin/disjuntor.py:507–517`, `app/docs/BACKLOG.md:811`)

3. **“Erro bobo, declara por quê e libera” não existe.** A máquina atual acumula agentes reprovados e mantém o disparo até rearme; não tem transição de dispensa por declaração nem registro específico dessa classificação. O rearme registra usuário e motivo genéricos. Usá-lo como dispensa confundiria dois atos diferentes. (`bin/disjuntor.py:565–580`, `bin/disjuntor.py:585–595`)

4. **O Stop não garante pedir isso na primeira reprovação.** A varredura B3 ocorre no final do caminho normal. Antes dela há saídas por orçamento, PAREI, ESPERANDO provado, fila vazia, teto e B2. O próprio selo comenta que o teto libera antes da varredura e cala o recado. O PreToolUse varre independentemente e pode negar sem esse aviso anterior. (`bin/hook_stop_fila1.py:1219–1248`, `bin/hook_stop_fila1.py:1286–1302`, `bin/tests/test_disjuntor.sh:330–332`, `bin/disjuntor.py:949–950`)

5. **Os contadores têm unidades diferentes das prometidas.**
   
   - B3 conta **IDs distintos de agentes reprovados por obra**, acumulados no JSON, sem janela de 24 horas. Não conta cada mensagem de reprovação. (`bin/disjuntor.py:556–570`)
   - O log recebe **um disparo por ciclo armado**, não uma linha por Stop ou por tentativa negada. (`bin/disjuntor.py:574–579`)
   - O ESTADO conta eventos `DISJUNTOR` das últimas 24 horas pelo identificador `B3`; não lê obra, classificação ou motivo. `REARMA` não entra nessa leitura. Logo não entrega “liberadas por erro bobo, com o porquê”. (`bin/disjuntor.py:1032–1055`)
   - `gerar_estado.py` delega a `disjuntor.ler/linha`; a apresentação atual diz “disparos 24 h” e soma negações gerais. Não há outro limiar B3 nesse gerador. (`bin/gerar_estado.py:277–283`, `bin/disjuntor.py:1147–1160`)

6. **A troca alcança contagem antiga e não garante log literal com `1`.** A varredura reaplica o limiar a todas as obras persistidas: uma obra antiga com uma ou duas reprovações dispara na próxima avaliação. O log usa o `n` real, que pode ser maior que um; as mensagens de Stop/PreToolUse usam a constante. Assim podem anunciar `1` enquanto o disparo registra `2` ou mais. (`bin/disjuntor.py:559–578`, `bin/disjuntor.py:954`, `bin/hook_stop_fila1.py:1054`)

7. **“Rearme zera, como hoje” precisa delimitar o que zera.** Ele esvazia `reprovados`, baixa `disparado` e guarda os IDs anteriores em `rearmado`, para não recontá-los. Não apaga disparos do log nem zera o placar das últimas 24 horas. (`bin/disjuntor.py:567–568`, `bin/disjuntor.py:590–593`, `bin/disjuntor.py:1036–1055`)
   
   “Do Ronald” também não é autenticação implementada: a CLI aceita obra/motivo e registra `$USER`; o selo a executa diretamente. A guarda atual protege a chave/settings, não esse comando de rearme. (`bin/disjuntor.py:136–158`, `bin/disjuntor.py:590–591`, `bin/disjuntor.py:1170–1173`, `bin/tests/test_disjuntor.sh:363–365`)

8. **Há mensagens e selos presos ao desenho antigo.**
   
   | Sítio | Suposição atual |
   |---|---|
   | `bin/disjuntor.py:11–12`, `:68`, `:94`, `:949` | Terceira reprovação/quarto verificador, em documentação e comentários. |
   | `bin/disjuntor.py:576–578` | Ação do log cravada em **“4o verificador final negado”**; plural fixo `reprovacoes`. |
   | `bin/hook_stop_fila1.py:1052–1054` | Stop também crava **“4o”**; com constante `1`, fica “1 reprovacoes” e “falhou 1 vezes”. |
   | `bin/disjuntor.py:951–954` | PreToolUse mantém “QUAL PADRAO”/“POR QUE falhou”, sem os campos novos. |
   | `bin/tests/test_disjuntor.sh:320–329`, `:376–382` | Duas reprovações **devem passar**; terceira dispara; mensagens/log exigem literalmente `3`. |
   | `bin/tests/test_disjuntor.sh:366–370`, `:384–387` | Casos sem Stop, falha de gravação e identidade ainda semeiam três reprovações. |
   | `bin/tests/test_disjuntor.sh:759–760` | Mutantes ancoram frases antigas, inclusive **“o 4o”**; âncora ausente já causa falha. (`:712–717`) |

   Os selos de topo provam que o recado do Stop cala, mantendo `decision=block` pelo `siga`; não provam que a linha libera PreToolUse. (`bin/tests/test_disjuntor.sh:335–348`)

9. **Preservar “o mesmo de hoje” exige preservar o alcance exato.** A leitura conta apenas agentes com metadado `verificador`, prompt reconhecido como FINAL e veredito no `SubagentHandback`; reconhece o prefixo da primeira linha, não qualquer texto de reprovação. A negativa B3 intercepta lançamento `Agent/verificador` FINAL, não todo caminho possível de revisão. (`bin/disjuntor.py:529–541`, `bin/disjuntor.py:550–557`, `bin/disjuntor.py:944–950`)
   
   A chave pula os disjuntores no Stop e no PreToolUse, mas a guarda da própria chave/settings continua ativa mesmo desligada: essa é uma exceção atual ao “desliga tudo”. (`bin/hook_stop_fila1.py:1040–1041`, `bin/disjuntor.py:979–981`)

Os registros anteriores também dizem terceira reprovação: devem ser distinguidos do novo corte, preservando sua natureza histórica. O `3` do B2 pertence a outro limiar. (`app/docs/CORTES.md:413`, `app/docs/BACKLOG.md:876`, `app/docs/PROMPTS.md:1695`, `bin/disjuntor.py:62`)

Análise estática; não executei o selo porque ele cria arquivos temporários e remove árvores. (`bin/tests/test_disjuntor.sh:37–49`)

## 31/10 12:40 · sessao a3f1a051fdbb · - · - · origem=Ronald (tmux)

O vínculo coincide nos dias efetivamente devolvidos, no caminho padrão de `fatos_do_periodo`: ambos os builders usam `vinculo_do_dia` com células completas; a precedência carrega os candidatos da janela e acrescenta as geradoras faltantes. A ausência de empate por `data_inicio` é garantida por unicidade por colaborador. Referências: `app/escala/utils.py:964`, `:968`, `:971`, `:1240`, `:1252`, `:1255`, `:1260`; `app/ponto/precedencia.py:271`, `:275`, `:283`, `:307`; `app/escala/models.py:1004`.

Há duas ressalvas:

- O legacy chama o single com um dia adicional `d0−1`, portanto a `lambda: esc` proposta pode devolver outro vínculo nesse padding: no exemplo, novo em 09/10, quando a juíza devolveria velho. O resultado desse dia é descartado, sem alterar o dia retido. `app/escala/utils.py:993`, `:995`, `:997`.
- Quando a juíza devolve `None`, o legacy cria diretamente o dia folga e não chama o predicado; por turno, o mapa contém `None`. Geradora fora da vigência resulta em `None` nas duas consultas à juíza. `app/escala/utils.py:985`, `:987`, `:1260`; `app/escala/alimentacao.py:247`, `:250`.

O teste chamado `test_MORDE_folga_de_calendario_de_outro_vinculo_e_trabalho_na_grade` **já passa no legacy antes do diff**: a FolgaDia velha de 15/10 não é consultada pelo single do segmento novo. A mordida legacy do conjunto é o caso manual preso ao vínculo velho, que antes não aparece no segmento novo. Logo o nome/comentário promete uma regressão específica nos dois builders que o teste não demonstra. `app/escala/utils.py:993`, `:997`, `:1109`; anexo `app/escala/tests/test_grade_folga_de_calendario_e_do_vinculo.py:63`, `:68`.

O atalho do calendário continua escolhendo outra representação: passa `{data: True}` somente da foto do vínculo carregado, e não a candidata por menor pk da pessoa; o predicado proposto aceita `True` como manual e perde a origem de calendário. A precedência pode completar outra geradora, sem reparar esse booleano. `app/colaboradores/services/calendario.py:593`, `:604`, `:607`; `app/ponto/precedencia.py:283`.

O registro não aponta um segundo juiz de vínculo introduzido pelo diff: a autoridade é `escala/alimentacao.py::vinculo_do_dia`; o novo predicado apenas recebe sua resposta. O registro também distingue a pergunta de vínculo da pergunta de foto/fase. `app/core/juizes.py:1255`, `:1299`.

A ativação está incompleta para atas existentes com impressão idêntica: o cartório pula o dia já julgado se `forcar=False`; o diff não altera a versão e a grade não integra essa impressão. Uma nova versão com horizonte ou rejulgamento forçado resolveria essa perna; sua execução não consta do dado `[não verificado]`. Evitar afirmar “nenhuma ata existente muda”, porque mudanças concomitantes de insumos também invalidam a impressão. `app/ponto/services/cartorio.py:110`, `:160`, `:1148`, `:1157`, `:1161`.

## 31/10 12:40 · sessao e5bf9903abb4 · - · - · origem=Ronald (tmux)

Achados para consolidação:

- **Candidata inválida pode esconder folga válida.** A seleção atual guarda somente a menor PK por pessoa/data (`app/ponto/precedencia.py:234-238`), embora o banco permita uma FolgaDia por vínculo/data (`app/escala/models.py:1571`). Havendo calendário antigo inválido de PK menor e manual válida de PK maior, o predicado descarta a primeira e nunca vê a segunda; a extração proposta preserva essa lacuna e a leva à grade. Ocorrência concreta no dado: **[não verificado]**.

- **Calendário segue com alimentação incompatível com o predicado.** Passa `{data: True}` derivado apenas das folgas do vínculo ativo (`app/colaboradores/services/calendario.py:592-608`). Isso perde `calendario_origem_id`/`escala_colaborador_id`; o predicado proposto aceita `True` como manual, inclusive quando a geradora do dia é outro vínculo. A precedência já completa os vínculos faltantes das geradoras (`app/ponto/precedencia.py:279-287`), mas não recupera a proveniência desse booleano. Manual de outro vínculo também fica fora dessa alimentação (`app/colaboradores/services/calendario.py:607`).

- **Outra resposta viva alimenta o mesmo chamado:** `o_dia_ja_e_folga` procura somente FolgaDia do vínculo do dia (`app/ponto/services/declarar_dia.py:61-70`, `:90-91`); o aceite do admin usa esse resultado para carimbar/fechar pergunta e fio (`app/chamados/services/validacao.py:247-271`). Manual presa em vínculo velho vale pelo predicado proposto, mas aqui não é reconhecida como já declarada. A alternativa passa por `checar_declaracao` (`app/chamados/services/validacao.py:280`), cujo veto encontra qualquer FolgaDia da pessoa/data sem conferir validade (`app/ponto/precedencia.py:161-166`, `:458-466`). Assim tanto a manual velha válida quanto o calendário velho inválido podem gerar recusa por folga existente. É lacuna preexistente deixada fora do diff, não regressão criada nele.

Escritores e portas de FolgaDia:

| Campo/valor | Escritor |
|---|---|
| FK vínculo + calendário não nulo | `gerar_foto_folgas`: `app/escala/folgas.py:157-175` |
| FK vínculo + calendário nulo/manual | `lancar_folga`: `app/escala/folgas.py:254-260`; `_foto_do_dia`: `:274-284`; nulabilidade/default: `app/escala/models.py:1557-1559` |
| Troca FK vínculo da manual | `reparentar_folga`: `app/escala/folgas.py:435-436`; calendário é excluído desse ato em `:405-406` |
| Exclusão indireta do calendário pode nulificar origem | `SET_NULL`: `app/escala/models.py:1557-1559`; rota concreta de exclusão: **[não verificado]** |
| Exclusão indireta do vínculo apaga FolgaDia | `CASCADE`: `app/escala/models.py:1553-1554` |

A porta declarada é exclusivamente `escala/folgas.py` (`app/core/portas.py:466`). O diff fornecido não escreve FolgaDia, não lê relógio novo nem adiciona trava: modifica leitura e dicionários da grade (`cura.diff`, trechos de `_aplicar_folgas_foto`; linhas posteriores propostas **[não verificado]**).

Leitores adicionais dos campos:

- Proveniência calendário/manual: divergência da foto (`app/escala/folgas.py:101-104`), proteção/reconciliação da foto (`:165-175`), guardas das portas manuais (`:254-256`, `:274-276`), reparentamento (`:405-418`, `:453-457`), toggle (`app/escala/views_plano_folgas.py:112-114`), declaração de dia (`app/ponto/services/declarar_dia.py:108-112`), filtro atual da precedência (`app/ponto/precedencia.py:313-315`).
- FK vínculo como universo de folga: alimentação (`app/escala/alimentacao.py:66-69`), cálculo aritmético (`app/escala/models.py:1251-1259`, `:1280-1282`, `:1327-1334`), defeito mensal (`:1220-1221`), plano (`app/escala/views_plano_folgas.py:60-70`), diagnóstico (`app/ponto/services/diagnostico_escala.py:102-104`), conflito de fase (`app/escala/services/fase_conflitante.py:74-80`), overlay atual (`app/escala/utils.py:1109-1111`).

Essas consultas mensais/fase não são todas a pergunta de validade da folga lançada: o gerador lê a foto por vínculo e grava `trabalha`/DNA (`app/ponto/portas/celula.py:243-248`, `:590-596`); o cartório grava separadamente a precedência na mesma ata (`app/ponto/services/cartorio.py:1673-1682`). A auditoria C5 também aceita folga pela cobertura de algum vínculo sem provenance (`app/chamados/management/commands/auditar_invariantes_chamados.py:1530-1549`), mas é alarme, não escritor de ata/chamado.

O comando de rejulgamento também seleciona qualquer FolgaDia pessoa/data (`app/ponto/management/commands/rejulgar_ausencia_em_folga.py:55-60`), porém o resultado efetivo é delegado a `julgar_celula` (`:69-71`); não impõe por si só a validade final.

## 31/10 12:40 · sessao b8196e590e3e · - · - · origem=Ronald (tmux)

O contrato vai além de mudar a constante: o destravamento por linha escrita e o desvio “erro bobo” não existem na máquina atual. `bin/disjuntor.py:947`, `bin/disjuntor.py:585`.

1. **Linha no RELATO cala o Stop, mas não libera verificador.** O Stop consulta `_pergunta_b3_escrita`; o PreToolUse consulta somente `varrer_b3` e nega enquanto `disparado` estiver ativo. Apenas `rearma_b3` limpa esse estado. `bin/hook_stop_fila1.py:1050`, `bin/disjuntor.py:950`, `bin/disjuntor.py:593`.

2. **O conteúdo exigido não é validado.** O reconhecedor atual aceita qualquer linha que contenha `DESENHO` e a identidade da obra; não exige uma das quatro leis, pergunta sem juiz, `arquivo:linha` ou motivo. Ele considera somente as duas primeiras linhas úteis. `bin/hook_stop_fila1.py:411`, `bin/hook_stop_fila1.py:417`, `bin/hook_stop_fila1.py:420`, `bin/hook_stop_fila1.py:440`.

3. **O Stop não garante pedir isso na primeira reprovação.** A varredura/recado B3 acontece no final do caminho normal; antes existem saídas por B4, orçamento, PAREI, ESPERANDO provado, fila vazia, teto e B2. O teste reconhece explicitamente que o teto pode calar o recado. `bin/hook_stop_fila1.py:1208`, `bin/hook_stop_fila1.py:1219`, `bin/hook_stop_fila1.py:1228`, `bin/hook_stop_fila1.py:1233`, `bin/hook_stop_fila1.py:1247`, `bin/hook_stop_fila1.py:1286`, `bin/hook_stop_fila1.py:1297`, `bin/hook_stop_fila1.py:1302`, `bin/tests/test_disjuntor.sh:330`.

4. **“Erro bobo” não tem transição nem registro.** O estado B3 guarda `reprovados`, `disparado` e `rearmado`; o rearme registra motivo genérico e usuário, sem classificação de liberação. A CLI não autentica Ronald: apenas chama a função com os argumentos. `bin/disjuntor.py:566`, `bin/disjuntor.py:585`, `bin/disjuntor.py:590`, `bin/disjuntor.py:1170`.

5. **A medição prometida não corresponde ao ESTADO atual.** O leitor conta eventos `DISJUNTOR` nas últimas 24 h por identificador (`B3`), sem ler motivo, conteúdo da pergunta ou liberação. `REARMA` não participa dos eventos lidos. A frase é “disparos 24 h”, não “paradas de desenho” nem liberações por erro bobo. `bin/disjuntor.py:1032`, `bin/disjuntor.py:1036`, `bin/disjuntor.py:1054`, `bin/disjuntor.py:1147`, `bin/disjuntor.py:1156`.

6. **O gerador ESTADO delega a esse leitor; não possui outro limiar.** Ele importa `disjuntor.ler/linha` e insere a frase na seção Disjuntores. Alterar apenas a seção visual não criaria a métrica ausente. `bin/gerar_estado.py:277`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:336`.

7. **Baixar o limiar também alcança reprovações antigas.** O contador persiste IDs por obra, sem janela temporal; a varredura verifica todas as obras guardadas. Obras antigas com uma ou duas reprovações passam a disparar no próximo scan. O log usa `n` real, que pode ser maior que um, e o texto mantém `reprovacoes` no plural. Rearmar zera a contagem da obra, não apaga disparos anteriores do log/ESTADO 24 h. `bin/disjuntor.py:559`, `bin/disjuntor.py:565`, `bin/disjuntor.py:572`, `bin/disjuntor.py:576`, `bin/disjuntor.py:590`, `bin/disjuntor.py:1036`.

8. **Sítios que conservam a regra antiga:** docstring 3ª/4º; comentário do limiar e do reconhecedor FINAL; ação de log “4o verificador”; comentário PreToolUse “3a”; mensagem Stop “4o verificador”. As mensagens parametrizadas passam a dizer “1 reprovacoes”/“falhou 1 vezes”. `bin/disjuntor.py:11`, `bin/disjuntor.py:68`, `bin/disjuntor.py:94`, `bin/disjuntor.py:578`, `bin/disjuntor.py:949`, `bin/disjuntor.py:951`, `bin/hook_stop_fila1.py:1052`.

Referências documentais ao contrato antigo aparecem em `app/docs/CORTES.md:413`, `app/docs/BACKLOG.md:876`, `app/docs/PROMPTS.md:1695`, `app/docs/PROMPTS.md:1700` e `app/docs/PROMPTS.md:1707`; são registros de avais anteriores, cuja relação com o novo corte precisa ficar explícita, sem tratá-los automaticamente como código a substituir. `app/docs/CORTES.md:413`, `app/docs/PROMPTS.md:1707`.

A chave realmente pula os disjuntores no Stop e no PreToolUse; a guarda da própria chave/settings continua ativa mesmo desligada, portanto “desliga tudo” tem essa exceção atual. `bin/hook_stop_fila1.py:1040`, `bin/disjuntor.py:979`.

## 31/10 12:39 · sessao 51f85d210cb4 · - · - · origem=Ronald (tmux)

Achados dos selos, por leitura estática:

- **Trocar só a constante quebra o selo atual.** Duas reprovações finais devem deixar Stop sem B3 e permitir PreToolUse; só a terceira dispara. Isso está codificado em `bin/tests/test_disjuntor.sh:320`, `bin/tests/test_disjuntor.sh:323`, `bin/tests/test_disjuntor.sh:324` e novamente em `bin/tests/test_disjuntor.sh:376`, `bin/tests/test_disjuntor.sh:380`.
- **Mensagens/log exigem literalmente 3.** Stop exige `3 reprovacoes` e `falhou 3 vezes`; PreToolUse exige `falhou 3 vezes`; trilha exige `3 reprovacoes`. Referências: `bin/tests/test_disjuntor.sh:327`, `bin/tests/test_disjuntor.sh:328`, `bin/tests/test_disjuntor.sh:329`. A descrição do selo ainda promete terceira reprovação/quarto verificador: `bin/tests/test_disjuntor.sh:13`.
- **Os casos de varredura sem Stop e de falha de gravação continuam semeando três reprovações**, portanto não certificam primeira reprovação: `bin/tests/test_disjuntor.sh:366`, `bin/tests/test_disjuntor.sh:367`, `bin/tests/test_disjuntor.sh:370`. A identidade O9 também só é exercitada com três: `bin/tests/test_disjuntor.sh:384`.
- **Linha no topo hoje só cala recado do Stop.** `_b3_cala` exige `decision=block` com o `siga` da fila, sem mensagem B3 e sem FALHA; não prova liberar PreToolUse. Referências: `bin/tests/test_disjuntor.sh:335`, `bin/tests/test_disjuntor.sh:337`, `bin/tests/test_disjuntor.sh:339`. As linhas aceitas não contêm lei concreta nem pergunta com arquivo:linha: `bin/tests/test_disjuntor.sh:343`, `bin/tests/test_disjuntor.sh:345`. O teste unitário aceita até `DESENHO OBRA-X (C)`: `bin/tests/test_disjuntor.sh:654`.
- **O selo atual exige topo entre as primeiras duas linhas úteis**, inclusive separadas por branco; terceira linha não cala. Referências: `bin/tests/test_disjuntor.sh:343`, `bin/tests/test_disjuntor.sh:347`, `bin/tests/test_disjuntor.sh:353`. Mutantes preservam essa política em `bin/tests/test_disjuntor.sh:786`, `bin/tests/test_disjuntor.sh:797`, `bin/tests/test_disjuntor.sh:798`, `bin/tests/test_disjuntor.sh:810`.
- **Rearme é testado como CLI invocável pelo próprio agente**, com motivo livre `Ronald respondeu o desenho`; a prova é somente liberação imediata e persistência após novo Stop. Referências: `bin/tests/test_disjuntor.sh:363`, `bin/tests/test_disjuntor.sh:364`, `bin/tests/test_disjuntor.sh:365`. O código correspondente só registra `$USER` e motivo, guarda os IDs antigos em `rearmado` e zera o ciclo: `bin/disjuntor.py:590`, `bin/disjuntor.py:592`, `bin/disjuntor.py:593`.
- **Mudar mensagens exige atualizar mutantes textuais.** Há âncoras literais para `QUAL PADRAO quebra` e `o 4o`: `bin/tests/test_disjuntor.sh:759`, `bin/tests/test_disjuntor.sh:760`. O executor acusa falha quando a âncora desaparece ou deixa de ser única: `bin/tests/test_disjuntor.sh:712`, `bin/tests/test_disjuntor.sh:715`.
- **Não há cobertura do novo contador de paradas/dispensas por erro bobo nos casos B3 existentes.** O bloco verifica a trilha antiga e somente `b3_sem_obra` na leitura do ESTADO; a outra asserção do leitor cobre negações gerais/RELATO ausente. Referências: `bin/tests/test_disjuntor.sh:329`, `bin/tests/test_disjuntor.sh:388`, `bin/tests/test_disjuntor.sh:391`, `bin/tests/test_disjuntor.sh:626`.
- **Rodar este selo violaria só leitura:** cria diretório temporário, arquivos e remove árvores. Referências: `bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:49`.

## 31/10 12:36 · sessao 0819a0bb99c3 · INTERMITENTE-EM-FERIADO · leitura · origem=Code

**1. Fontes existentes e limites da leitura**

**Texto integral da CLT, do art. 452-A/§6 e da Lei 605/49/art. 9:** sem fonte no repo. Foram encontradas referências a esses dispositivos, sem sua transcrição integral. [nao verificado]

**Conteúdo cadastrado de `Sindicato.obs`:** sem fonte no repo. [nao verificado] O modelo contém apenas a declaração `obs = models.TextField(blank=True)`. (`app/core/models.py:40`)

O repositório aponta onde estaria esse conteúdo:

> “A fundamentacao INTEIRA (clausulas literais + parecer) esta em Sindicato.obs e em core/regua_cct.py”  
> “A CCT em PDF **nao esta no sistema** […] Enquanto nao tiver, o lastro e o texto em `obs`.”

(`CLAUDE.md:442-443`, `CLAUDE.md:455-456`)

A CCT identificada é “Vigilantes de Londrina/PR”, registro “MTE PR000251/2026”, vigência “01/02/2026-31/01/2028”. (`CLAUDE.md:430-432`)

Existe esta transcrição literal da cláusula 38-d:

> “no regime de 12x36 - ainda que cumprido em horario noturno -, a hora sera considerada normal de 60 (sessenta) minutos, garantido, sempre, o adicional noturno respectivo.”

O trecho trata de hora noturna no 12x36; não menciona feriado ou intermitente. (`app/core/regua_cct.py:127-138`)

**2. O que está escrito sobre intermitente, dobra e §6**

| Fonte | Trecho literal e alcance |
|---|---|
| Docstring de `MotorIntermitente` | “Intermitente-horista (Art. 452-A) — sem escala, sem marcos, sem jornada prevista. Apura SOMENTE horas cumpridas do par E/S (+ noturno legal). **Sem HE por estouro de jornada, sem feriado-100 automatico, sem atraso.**” (`app/ponto/motor_calculo_v2.py:3485-3488`) |
| Comentário do cálculo geral de feriado | “Feriado: Sumula 146 TST paga **TODAS as horas em dobro (nao so excedente)**.” (`app/ponto/motor_calculo_v2.py:2731-2733`) |
| Docstring da regra compartilhada | “o dia de feriado que dobra poe a jornada **INTEIRA em HE 100** […] e nao so o excedente.” (`app/ponto/calculador/regras.py:330-332`) |
| Régua CCT: piso declarado para 12x36 | `'feriado_12x36_em_dobra': False, # BUG 91: piso legal = hora simples (Art.59-A par.2)` (`app/core/regua_cct.py:44`) |
| Régua CCT: exceção cadastrada | “a dobra do feriado no 12x36 so volta por **CLAUSULA CADASTRADA**”; “Fora do 12x36 […] a Sumula 146 e o piso”. (`app/core/regua_cct.py:473-476`) |
| Comentário do modelo `Sindicato` | “o piso LEGAL do feriado trabalhado em 12x36 e HORA SIMPLES”; “a convencao coletiva PODE ser mais benefica e devolver a dobra; nenhuma das nossas diz isso hoje”. (`app/core/models.py:49-52`) |
| Comentário do espelho | “100% = descanso trabalhado (Lei 605/49 art.9 + Sum.146 TST) + excedente por CCT.” Em seguida: “Quantidade CRUA: majoracao e' da folha, nunca do ponto.” (`app/relatorios/pdf_espelho.py:764-766`) |

Há **cortes internos em sentidos diferentes**, com a inversão expressamente registrada:

- **13/10, aproximadamente 17h:** “intermitente que trabalha em feriado recebe a dobra (Lei 605 art.9 / Sumula 146 / art.452-A par.6) -- a docstring do MotorIntermitente se corrige para o codigo”. (`app/docs/PROMPTS.md:1204`)
- **13/10, aproximadamente 18h:** “intermitente em feriado recebe dobra (452-A §6, Lei 605 art.9, Sum.146)”; acrescenta: “**leitura passa pelo DP antes de virar lei**”. (`app/docs/PROMPTS.md:1213`)
- **14/10, 21:4x:** “**SDF/intermitente em feriado NAO recebe dobra (DP 14/10); o cartao nao afirma pagamento em dobro**”. A própria linha identifica esse corte como inversão da O264. (`app/docs/PROMPTS.md:1231`)
- **15/10, 11:5x:** “**intermitente segue pela docstring do MotorIntermitente (sem feriado-100 automatico)**”. O registro complementar diz “sem entrada nova no frozenset”. (`app/docs/PROMPTS.md:1239`)

O BACKLOG também registra a inversão:

> “A LEITURA DO DP CHEGOU E INVERTEU ESTA OBRA”; “a docstring do `MotorIntermitente` **nao se corrige**”; a cura dos dois zeros de 12/10 está “**CANCELADA** -- aqueles zeros sao o numero CERTO”.

(`app/docs/BACKLOG.md:1377`; orientação complementar em `app/docs/BACKLOG.md:1390`)

**Art. 452-A, §6:** os registros de 13/10 acima citam o dispositivo para sustentar a dobra, mas não transcrevem seu conteúdo. (`app/docs/PROMPTS.md:1204`, `app/docs/PROMPTS.md:1213`) Para a enumeração literal solicitada — remuneração, férias + 1/3, 13º, RSR e adicionais legais, pagos ao fim de cada período — **sem fonte no repo**. [nao verificado]

**3. Tratamento atual no código**

**Motor v2**

A limpeza está em [app/ponto/motor_calculo_v2.py:3491](/home/ronald/codex_ws/c7ea207a3f1e8ea911a0265b8e88991b4d91c1e4/app/ponto/motor_calculo_v2.py:3491), com estas atribuições:

```python
super().__init__(*args, **kwargs)
# neutraliza gatilhos de extra: jornada "infinita" e sem feriados
self.jornada_minutos = 24 * 60 * 365
self.feriados = set()
```

(`app/ponto/motor_calculo_v2.py:3491-3495`)

**As linhas atuais 903–905 não limpam o calendário:** elas consultam o conjunto e depois chamam o fallback. (`app/ponto/motor_calculo_v2.py:901-905`)

| Decisão | Código literal / valor produzido |
|---|---|
| Escolha do motor | `if ciclo == 'intermitente' or tipo_base == 'intermitente': cls = MotorIntermitente`. (`app/ponto/motor_calculo_v2.py:3563-3564`) |
| Flag de dobra pelo ciclo | `CICLOS_FERIADO_SIMPLES = frozenset({'12x36'})`; se o argumento não veio: `feriado_em_dobra = ciclo not in CICLOS_FERIADO_SIMPLES`. Para o ciclo literal `intermitente`, esse ramo produz **`True`**. (`app/ponto/motor_calculo_v2.py:3537`, `:3607-3608`) |
| Limpeza no intermitente | `self.feriados = set()`: conjunto vazio. (`app/ponto/motor_calculo_v2.py:3495`) |
| Reconhecimento do feriado | `if data in self.feriados: return True`; caso contrário: `return is_feriado(data)`. O conjunto vazio mantém essa segunda chamada. (`app/ponto/motor_calculo_v2.py:901-905`) |
| Fallback | `return eh_feriado(data, praca)`; em exceção, `return False`. O argumento `praca` tem padrão `None`. (`app/ponto/motor_calculo_v2.py:30-43`) |
| Alcance sem município | “SEM MUNICIPIO […] nem municipal nem estadual valem, so o nacional.” (`app/core/feriados.py:91-95`) |
| Decisão de dobra | `return bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. (`app/ponto/motor_calculo_v2.py:879`) |

`MotorIntermitente` herda de `Motor12x36` e seu corpo contém somente o `__init__` mostrado acima. (`app/ponto/motor_calculo_v2.py:3483-3495`) O `calcular_periodo` herdado decide assim:

- **Sem saída:** marca `turno_aberto = True` e retorna; os campos de extra nascem em zero. (`app/ponto/motor_calculo_v2.py:2689-2693`, `:166-169`)
- **Com duração positiva e dobra reconhecida:** atribui a duração inteira a `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, e marca `hora_extra_100 = True`. (`app/ponto/motor_calculo_v2.py:2747-2754`)
- **Ramo seguinte:** `elif feriado and duracao > 0 and self.feriado_em_dobra` apenas acrescenta o alerta “PREVISTO na escala — horas SIMPLES”. O helper anterior já usa o mesmo flag e a consulta de feriado. (`app/ponto/motor_calculo_v2.py:2756-2760`, `:879`)
- **Demais situações:** calcula excedente contra `_jornada_do_dia(e)`; sem previsto, subtrai a própria duração, produzindo **extra zero**. Havendo extra, aplica a tolerância e reparte em até 120 minutos a 50%, com o restante a 100%. (`app/ponto/motor_calculo_v2.py:2773-2792`)

Assim, **o corpo não estabelece “HE100 feriado sempre zero para intermitente”**: o calendário injetado é apagado, mas permanecem o fallback e o flag derivado do ciclo. (`app/ponto/motor_calculo_v2.py:3495`, `:901-905`, `:3537`, `:3607-3608`)

Há teste que injeta `12/06/2026` como feriado e exige `minutos_extra_100 == 0` e `horas_extra == 0`, com as mensagens “Horista nao tem feriado-100 automatico” e “Horista nao tem HE por estouro de jornada”. (`app/ponto/tests/test_intermitente.py:54-68`)

**Calculador em `ponto/calculador/`**

Na alimentação de `regras.do_dia`, o calendário vem diretamente do argumento recebido:

```python
_feriados = set(feriados or ())
```

O flag de dobra vem do motor CCT; a identificação de intermitente é atualizada pelo vínculo do dia e enviada separadamente à regra. (`app/ponto/calculador/alimentacao.py:378-396`, `:479-485`, `:509-510`)

| Ramo | Valor produzido |
|---|---|
| Folga trabalhada do intermitente | `if _trab <= 0 or intermitente: return 0.0, 0.0`. (`app/ponto/calculador/regras.py:386-388`) |
| Dia sem trabalho previsto, mas intermitente | Não retorna pelo ramo dos demais regimes; grava `intermitente_segue_como_trabalho = True` e `horas_trabalhadas = trab / 60`. (`app/ponto/calculador/regras.py:736-740`) |
| Excedente sem previsto | `extra = trab - _limite if previsto_min else 0.0`: **zero** quando não há previsto. (`app/ponto/calculador/regras.py:767-771`) |
| Regra de feriado | Usa os mesmos métodos `MotorBase._feriado_dobra_no_dia` e `MotorBase.eh_feriado`. Retorna o trabalhado inteiro quando o juiz reconhece dobra; caso contrário, **`0.0`**. (`app/ponto/calculador/regras.py:287-295`, `:344-347`) |
| Dobra positiva | `horas_extras_100 = _dobra / 60`, `horas_extras_100_feriado = _dobra / 60`, `horas_extras_50 = 0.0`; retorna sem somar o excedente comum. (`app/ponto/calculador/regras.py:774-789`) |
| Calendário ou flag ausentes | Declara `horas_extras_100` e `horas_extras_100_feriado` em `SEM_ENTRADA`. (`app/ponto/calculador/regras.py:633-638`) |

Na releitura v3 de `periodo.py`, `reler` é descrito como “**LEITURA DE ENSAIO, nao caminho de producao**”. (`app/ponto/calculador/periodo.py:540-545`) Sem previsto, o excesso retorna com os campos numéricos zerados; a dobra é julgada depois, separadamente. (`app/ponto/calculador/periodo.py:367-371`, `:410-413`, `:819-831`) Quando positiva, grava toda a dobra em `minutos_extra`, `minutos_extra_100` e `minutos_extra_100_feriado`, com `minutos_extra_50 = 0.0`; caso contrário, grava os valores do excesso. (`app/ponto/calculador/periodo.py:833-847`)

A integração do fechamento chama `rubricas_por_dia` e grava `versao='oraculo'`; o comentário registra que essas linhas nascem ao lado de `versao='motor'` e que “Nenhum leitor le 'oraculo' hoje”. (`app/ponto/services/fechamento.py:1214-1217`, `:1233-1262`)

O relatório documenta um caso específico como “v2 paga 0 de HE100 feriado […] v3 paga em dobra”, mas o mantém como **pergunta**. Sua referência à limpeza em `903-905` não corresponde às linhas atuais do código. (`app/docs/RELATO.md:12`; `app/ponto/motor_calculo_v2.py:901-905`, `:3493-3495`)

**4. Leis L-NNN, cortes e casos E1**

| Registro | Trecho literal |
|---|---|
| **L-085** | “A JORNADA PERTENCE AO DIA DE INICIO, inclusive para domingo, feriado, contagem de semana e DSR”. (`app/docs/LEIS.md:117`) |
| **L-086** | Texto original: “domingo ou feriado so pagam dobra em dia NAO previsto”. A mesma linha registra: “a clausula de FERIADO do texto esta **REVOGADA** -- feriado previsto DOBRA conforme feriado_em_dobra do ciclo (12x36 nao dobra)”. (`app/docs/LEIS.md:118`) |
| Corte corretivo de 27/09 | “FERIADO NAO entra na L-086: segue a regra de feriado que ja existe no cadastro do ciclo”. (`app/ponto/motor_calculo_v2.py:862-865`) |
| **L-095** | Inclui “HE100 feriado” entre os valores lavrados uma vez pelo motor v2. (`app/docs/LEIS.md:113`) |
| Menção a intermitente na auditoria da **L-091** | “tipo_base ainda escolhe o motor […] 12x36 e intermitente pelo rotulo”. (`app/docs/LEIS.md:174`) |
| **C05** | Descrito como “lote 1 contra a v2, rubrica extra”, “v3 verde, v2 vermelha”. (`app/docs/RELATO.md:86`) |
| **C06** | “CCT Vigilantes x prorrogacao noturna”. (`app/docs/BACKLOG.md:782`) |
| **C16** | “intervalo 30 min batido: suprimido=30 pelo art.71 par.4 pos-2017?”. (`app/docs/BACKLOG.md:782`) |
| **C17** | “comercial 08-17 sem intervalo”; resposta registrada: “C17 HE 0 intra 60”. (`app/docs/RELATO.md:91`) |
| Corte **V3-E1-C17** | “L-031+L-032: sem intervalo batido indeniza so o suprimido, HE 0; C17/C16/C06 PENDENTE ate o DP/contador”. (`app/docs/CORTES.md:410`) |
| Corte **E1-VALIDACAO** | “lote1 todos menos C05/C17 (DP)”. (`app/docs/CORTES.md:411`) |
| Estado posteriormente relatado | “PENDENTE_DP C05/C06/C16/C17 em skip”. (`app/docs/RELATO.md:82`) |

**Arquivos `docs/E1-*` e texto integral desses quatro casos:** sem fonte no repo. [nao verificado] O relatório referencia `docs/E1-PERGUNTA-AO-DP-CONTADOR.md` expressamente como documento “da copia” e identifica outra cópia de trabalho para o E1. (`app/docs/RELATO.md:88`, `:92`)

**C05, C06, C16 ou C17 como caso específico de intermitente trabalhando em feriado:** sem fonte no repo. [nao verificado]

_1018 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
