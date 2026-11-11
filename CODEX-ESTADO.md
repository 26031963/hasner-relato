# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 02:32** · publicado em **11/11/2026 02:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 02:32 · sessao 79a6f5449c81 · FIO-E0-V4-E3 · verificador · origem=Code

**PARE na pré-verificação: c8b tem bloqueios de execução, horizonte temporal, paridade da alimentação e eficácia do selo B5-j.** As formas em memória passam, mas não cobrem esses caminhos. (app/chamados/services/tarefa_do_dia.py:245,313,467; app/chamados/juizes.py:1346; app/escala/servico_jornada.py:376; app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:118)

1. **BLOQUEIA — há leitor de `viva_c6` sem alimentação correspondente.**

   Em dia de trabalho com algumas instâncias, uma pergunta de motivo conhecido cujo marco desapareceu entra na exposição administrativa em `_tarefa_pura`. Porém `_ler_vida` só seleciona perguntas de dia sem trabalho, posição acesa ou motivo fora do mapa. A posição ausente não entra; o atributo nunca é atribuído, e `expor_admin` o acessa diretamente. Reproduzi em memória: `AttributeError: ... has no attribute 'viva_c6'`. (app/chamados/services/tarefa_do_dia.py:245,313,465,467,480)

   **RED necessário:** grade atual sem intervalo, pergunta antiga de intervalo e demais marcos presentes; chamar tarefa avulsa e lote pelo construtor real. O teste atual de integração contorna esse construtor com `__new__`. (app/chamados/tests/test_e3_c8b_sem_banco.py:227)

2. **BLOQUEIA — o C6 atual pode apagar uma decisão exigível no `agora` passado.**

   A carga corta decisões e passivo por `ato_em <= agora`, mas passa ao C6 a pergunta com os carimbos atuais. A primeira guarda do C6 mata por qualquer `validada_em`, inclusive posterior ao instante consultado. (app/chamados/services/tarefa_do_dia.py:385,387,394,478; app/chamados/juizes.py:1346)

   Contraexemplo: RESPOSTA textual em T1; VALIDACAO em T2; consulta com `agora=T1`. A sequência carregada ainda exige `validar/admin`, mas `viva_c6=False` impede a exposição. A porta real grava carimbo e decisão VALIDACAO no mesmo instante, tornando o cenário alcançável pelos escritores existentes. Reproduzi a supressão em memória. (app/chamados/services/tarefa_do_dia.py:133,136,245; app/chamados/services/validacao.py:134,139)

   **RED necessário:** consultar o dict inteiro em T1 antes e depois da validação real em T2, mantendo o mesmo `agora`. O contrato já declara o corte temporal dos fatos datados da pergunta. (app/chamados/services/tarefa_do_dia.py:348)

3. **BLOQUEIA — relógio fora da borda no caminho novo.**

   A tarefa injeta `hoje=localdate(self.agora)`, mas CF compara o fim do turno com `_tz.now()`. Portanto a alimentação nova não depende exclusivamente do relógio recebido pela tarefa. Esse comportamento já existia no CF avulso; c8b passa a alcançá-lo pela nova leitura do C6. (app/chamados/services/tarefa_do_dia.py:478,480; app/chamados/juizes.py:1414; app/escala/servico_jornada.py:375,376)

   Com CF real, vínculo diurno 08–17 e nenhuma ausência/batida, mudar somente o relógio ambiente de 12h para 20h mudou o resultado de ausência de falta para `saida_sem_entrada`. O apoio fixa `now` em `AGORA`, ocultando essa diferença; impacto nos dias produtivos anexados: **[nao verificado]**. (app/chamados/tests/e3_c8b_apoio.py:89,91; app/escala/servico_jornada.py:376,454)

   **FILA adicional:** esse teto chama `fim_turno` sem `ec/celulas`, lendo o template; a contagem posterior lê os marcos congelados da célula. É uma divergência antiga de fonte dentro do CF, preservada pelos dois caminhos. (app/escala/servico_jornada.py:375,427; app/escala/servico_jornada.py:68,71,89)

4. **BLOQUEIA a afirmação irrestrita “carga = avulso”: existe contraexemplo de vínculo.**

   Se a célula de um colaborador aponta para uma geradora de outro colaborador, com vigência cobrindo o dia, `escala_vigente` entrega essa geradora diretamente ao juiz. A carga usa somente os vínculos do colaborador solicitado e devolve `None`. Reproduzi os corpos reais em memória; a FK não impõe igualdade dos colaboradores. Ocorrência produtiva e escritor que tenha produzido essa inconsistência: **[nao verificado]**. (app/escala/servico_jornada.py:55,57; app/chamados/services/carga_pergunta_viva.py:157; app/ponto/turnos.py:2990; app/escala/alimentacao.py:279; app/escala/models.py:1692)

   O teste de paridade substitui justamente `escala_vigente` por um alimentador que chama diretamente `vinculo_do_dia`; assim, não confronta os dois carregadores reais nesse caso. A correção do cadastro inconsistente pode ter fila própria, mas a igualdade universal não está demonstrada. (app/chamados/tests/e3_c8b_apoio.py:97,101)

   Nos demais recortes, a inspeção encontrou reutilização dos corpos e filtros abaixo; paridade ORM integral: **[nao verificado]**.

   | Insumo | Comparação carga × avulso |
   |---|---|
   | Turnos | Mesmo `_turnos_do_colab_puro`, executado novamente para D..D ou D−1..D+1; não recorta turnos previamente pareados na janela larga. (app/chamados/services/carga_pergunta_viva.py:163,180; app/ponto/turnos.py:1918) |
   | Janela | Mesmo pad `[ini−2, fim+1]`; carga recorta batidas por intervalo civil `[00:00 pad_ini, 00:00 pad_fim+1)`. O pareador continua decidindo `data_turno`. (app/ponto/turnos.py:1834,1847; app/chamados/services/carga_pergunta_viva.py:172,175) |
   | Universo de batidas | Carga usa `batidas_apuraveis`; avulso filtra `retratada_em__isnull=True`. A última batida também é apurável. (app/ponto/turnos.py:1863,1894,2999,3017) |
   | Ausências | Mesmo efeito `cobranca`, excluindo rejeitadas; mesmo recorte inclusivo de cobertura, preservando sobrepostas e a data inicial de range malformado. Não escolhe apenas a vencedora. (app/chamados/services/carga_pergunta_viva.py:98,105; app/ponto/turnos.py:2511,2531,2757) |
   | Células/grade/DNA | Células vêm da porta comum; grade e DNA recebem os mapas carregados. Grade exige a lista inteira de vínculos. (app/ponto/turnos.py:2980; app/chamados/services/carga_pergunta_viva.py:149,178; app/escala/services/leitor_celula.py:350,355) |
   | Testemunhas | Plantadas continuam filtradas por origem e retratação; prova sem origem reutiliza a janela inclusiva de ±20 minutos e a ordenação por timestamp. (app/chamados/services/carga_pergunta_viva.py:140,184,187; app/chamados/juizes.py:235,244,744) |
   | Ordenação | Não encontrei outro contraexemplo determinístico por empate: há unicidade de batida por colaborador/timestamp e de vínculo por colaborador/data inicial. (app/ponto/models.py:105; app/escala/models.py:1003) |
   | Exportações | O chamador novo anota a vigência do conjunto e separa cada tripla antes de chamar K6. (app/chamados/services/tarefa_do_dia.py:434,437,438) |

5. **BLOQUEIA — B5-j perdeu a mordida após o split.**

   O universo produtivo continua pela porta, mas o detector procura consultas somente na função que chama `parear_turnos` ou `_turnos_do_colab_puro`. Agora `turnos` chama o corpo puro, enquanto a consulta mora em `_carregar_batidas`. O comentário afirmando que o tripwire ainda vê esse universo ficou incorreto. (app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63,118,125; app/ponto/turnos.py:3011,3015,3027,3036)

   Mutação somente em memória: substituir a porta desse helper por `Batida.objects`, sem filtro de retratação. O detector retornou exatamente os mesmos universos para original e mutante, ambos com `crus=[]`. O selo precisa observar a chamada ao carregador extraído e matar essa mutação. (app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:103,118; app/ponto/turnos.py:3017)

6. **FILA — K6 aceita alimentação capaz de mudar o veredito para outra tripla.**

   `exportacoes` pula a seleção por empresa/mês/ano e a anotação. Uma linha vigente de outra tripla pode fazer K6 devolver `True` — reproduzido em memória — ou devolver o registro alheio com `registro=True`. Uma lista incompleta também pode produzir falso negativo. (app/folha/export.py:583,586,587)

   Linhas ORM não anotadas consultam a propriedade `e_vigente` individualmente; isso pode reintroduzir N consultas. Não implica automaticamente veredito errado quando a tripla e o conjunto estão corretos. (app/folha/models.py:121,123,134,200)

   O único chamador produtivo novo encontrado fornece linhas anotadas e da tripla correta. Portanto há fragilidade do contrato do juiz; uso produtivo incorreto desse kwarg: **[nao verificado]**. (app/chamados/services/tarefa_do_dia.py:437,438)

7. **Cache, residual A/S e classes de reprovação.**

   `viva_c6` é um cache transitório do retorno do C6, com escritor produtivo em `_ler_vida` e leitores em `expor_admin` e na guarda residual; não constitui um segundo predicado de vida. O caminho com carga também evita ler/escrever o memo avulso. (app/chamados/services/tarefa_do_dia.py:480,245,326; app/chamados/juizes.py:867)

   A carga não tem invalidação depois de `_vida_carregada=True`. No consumidor produtivo literal encontrado, o command de sombra apenas percorre dias e lê resultados; não identifiquei escrita entre a alimentação e esses leitores. Reutilização após escrita ou por leitor dinâmico: **[nao verificado]**. (app/chamados/services/tarefa_do_dia.py:445,481; app/chamados/management/commands/diff_e3_sombra.py:115,125)

   O residual A/S lê literalmente o veredito lavrado `fato_em_ausencia`, exige dia sem trabalho, competência não entregue e nenhuma pergunta viva elegível. Emite `validar/admin` do dia, sem criar nem ressuscitar pergunta. Esse ramo não alcança `nunca_bateu`, `indefinida` ou dia de trabalho que deva continuar com o colaborador. (app/chamados/services/tarefa_do_dia.py:230,234,325,330)

   A classificação da contradição continua no supra-juiz: fato em dia encerrado com cobertura aprovada; parcial sem período recebe outro código. O mapa converte `FATO_EM_AUSENCIA` em `fato_em_ausencia`, e a porta lavra o resultado. Não há segundo classificador da contradição na tarefa. (app/ponto/supra_juiz.py:379,382,400,404; app/escala/models.py:1615,1619,1624; app/ponto/portas/celula.py:923,1035)

   **ESMERIL:** há relógio fora da borda e selo sem mordida, conforme os bloqueios acima. Não identifiquei segundo escritor persistente novo nem novo significado de `data_evento`: a tarefa mantém a identidade pelo dia do turno e escreve as novas ações em dicionários. O teto pelo template permanece uma fonte paralela antiga. (app/chamados/services/tarefa_do_dia.py:218,236,252,331; app/escala/servico_jornada.py:375,427)

8. **Os GREEN/mutantes não certificam os escritores e leitores ORM reais.**

   Reexecutei os **32 testes em memória: RC=0, OK**; o agregado dos **14 mutantes: RC=0**, exigindo FAIL por assertion e nenhum ERROR. Os executores são os indicados na entrega. (app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:115; app/chamados/tests/test_e3_c8b_sem_banco.py:47; app/chamados/tests/e3_c8b_mutantes.py:17,87,105)

   C6, CF e derivação do slot executam corpos extraídos do fonte. Porém são substituídos: `CargaDaJanela`, pareamento avulso e puro, grade, managers/Q, tranca e relógio. A integração da tarefa usa `__new__`; AS/B puros recebem vida e complementos prontos. Assim, não exercitam os escritores reais formando os fatos que a cura consome. (app/chamados/tests/e3_c8b_apoio.py:63,89,102,112,121,123,132; app/chamados/tests/test_e3_c8b_sem_banco.py:41,227; app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105,119)

   O mutante de janela larga morre pelo stub `Janela.turnos`, que levanta `AssertionError`; ele protege o caminho escolhido, sem provar a geometria real do pareamento. (app/chamados/tests/e3_c8b_apoio.py:75; app/chamados/tests/e3_c8b_mutantes.py:65)

   **RED ORM ainda necessários para certificar esta cura:**

   - **Execução/horizonte:** marco removido sem `viva_c6`; mesma consulta T1 antes/depois da validação em T2; mesmo `agora` com relógio ambiente diferente. (app/chamados/services/tarefa_do_dia.py:313,467; app/chamados/juizes.py:1346; app/escala/servico_jornada.py:376)
   - **B/A/S pelas portas reais:** resposta e decisão E1 reais; ausência, batidas, julgamento e lavra reais; pergunta morta ou inexistente; competência exportada com nenhuma ação nova. Os testes atuais recebem essas condições prontas. (app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105,119; app/chamados/tests/test_e3_c8b_sem_banco.py:35,119)
   - **Paridade completa:** construtor real, dict inteiro avulso/lote, N=1/N=4 e contagem SQL incluindo relações lazy; o apoio atual substitui o pai e contorna o construtor. (app/chamados/tests/e3_c8b_apoio.py:63; app/chamados/tests/test_e3_c8b_sem_banco.py:227,257)
   - **Geometria/fontes:** D..D e D−1..D+1, bordas do pad, noturno, troca de vínculo, retratadas e geradora de outro colaborador; ambos os pareadores atuais devolvem a mesma lista sintética. (app/chamados/tests/e3_c8b_apoio.py:121,123; app/escala/servico_jornada.py:55)
   - **Ausências reais:** estados de cobrança, catálogo de parcialidade, sobreposição e range malformado; hoje Q/managers e parcialidade são doubles. (app/chamados/tests/e3_c8b_apoio.py:17,39,120)
   - **K6 carregado:** outra tripla, lista incompleta/não anotada, empate, substituição e vigente invalidada sem substituta; o teste novo usa `Registro/Query` sintéticos. (app/chamados/tests/test_e3_c8b_sem_banco.py:437,459,473)
   - **Frescor após escrita:** resposta, validação, materialização/retratação, lavra, tranca/toque e exportação; comparar carga nova, carga antiga e memo avulso. O teste novo altera apenas insumo em memória. (app/chamados/tests/test_e3_c8b_sem_banco.py:290,298)

**Quem escreve e quem lê no caminho tocado:**

| Campo/valor | Escritor/autoridade | Leitor |
|---|---|---|
| `veredito`, `veredito_via`, `ata` | Julgamento e porta de lavra. (app/ponto/supra_juiz.py:404; app/ponto/portas/celula.py:1033,1035,1036) | Grade, C6 e residual. (app/escala/services/leitor_celula.py:424; app/chamados/juizes.py:1359; app/chamados/services/tarefa_do_dia.py:325) |
| `trabalha`, `dna`, `escala_geradora` | Geração/edição pela porta da célula. (app/ponto/portas/celula.py:259,272,603) | Vínculo, marcos, CF e insumo de dia sem trabalho. (app/escala/alimentacao.py:269; app/escala/servico_jornada.py:297,427; app/escala/services/leitor_celula.py:428) |
| Identidade: `colaborador`, `instancia_marco`, `conversa`, `motivo`, `data_evento` | Nascimento; mudança de dia pela porta declarada de rechaveamento. (app/chamados/models.py:2427,2825; app/chamados/veredito_lavrado.py:47) | Seleção da carga, posição e identidade da tarefa/slot. (app/chamados/services/tarefa_do_dia.py:107,230,236,371; app/chamados/services/materializacao.py:280) |
| `resposta_colab`, `respondida_em` | Escritores de resposta e carimbo com resposta. (app/chamados/services/disputa_emissao.py:1467,1652; app/chamados/models.py:3132) | C6, slot e projeção E1. (app/chamados/juizes.py:1362; app/chamados/services/materializacao.py:279; app/chamados/services/tarefa_do_dia.py:394) |
| `validada_em` | Porta de validação. (app/chamados/services/validacao.py:134,137) | Primeira guarda C6 e passivo E1. (app/chamados/juizes.py:1346; app/chamados/services/tarefa_do_dia.py:394) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, persistido pelas portas de carimbo. (app/chamados/models.py:3097,3152,3190) | C6, identidade e testemunho de materialização. (app/chamados/juizes.py:1357,1383,1421; app/chamados/services/tarefa_do_dia.py:230) |
| `DecisaoPergunta`: ato, instante, texto e absorção | `registrar`; validação registra no mesmo ato do carimbo. (app/chamados/models.py:3943; app/chamados/services/validacao.py:139) | Carga ordena/corta; `_pela_decisao` usa a última e lê o estado da absorção. (app/chamados/services/tarefa_do_dia.py:132,153,385) |
| Batida: colaborador, timestamp, tipo, origem da pergunta e retratação | Registro/anotação e retratação pela porta. (app/ponto/registro_batida.py:94,205,267,274) | Pareador, plantadas, janela do slot e efeitos. (app/chamados/services/carga_pergunta_viva.py:140,175,184; app/chamados/services/tarefa_do_dia.py:406) |
| Ausência: tipo, período, minutos e status | Criação, edição e decisão da ausência. (app/ponto/services/ausencia.py:130,465,897) | Filtro de efeito/cobertura, CF e E1. (app/ponto/turnos.py:2511,2531; app/escala/servico_jornada.py:338; app/chamados/services/tarefa_do_dia.py:153) |
| Vínculo: proprietário, atividade, vigência e tipo | Escritores completos de cadastro: **[nao verificado]**. | Carregador e juiz único de vínculo; marcos. (app/ponto/turnos.py:2990; app/escala/alimentacao.py:269,285; app/escala/servico_jornada.py:69) |
| `veredito_lavrado`, `julgado_em`, `janela_expira_em` | `veredito_lavrado.lavrar`; backfill declarado como exceção. (app/chamados/veredito_lavrado.py:58,254) | `esta_stale` e carga da tarefa. (app/chamados/veredito_lavrado.py:213; app/chamados/services/tarefa_do_dia.py:399) |
| Tranca/toque do admin | Escritores completos: **[nao verificado]**. | Autoridade `dias_trancados`; carga repassa o resultado ao C6 e à tarefa. (app/chamados/juizes.py:987; app/chamados/services/tarefa_do_dia.py:418; app/chamados/services/carga_pergunta_viva.py:143) |
| Exportação, tripla e geração | `ExportacaoDominio.registrar`. (app/folha/models.py:174,182) | Anotação de vigência e K6. (app/folha/models.py:209,220; app/folha/export.py:583) |
| `invalidada_em` | `invalidar`, com persistência e trilha. (app/folha/models.py:143,158,162) | K6. (app/folha/export.py:586) |
| `vigente`, `_vigente_anotado` | `anotar_vigencia`, somente em memória. (app/folha/models.py:224,225) | `e_vigente`, `so_vigentes`, K6. (app/folha/models.py:123,200; app/folha/export.py:583) |
| Trilha `retratar_pergunta_obsoleta` | Escritor atual correspondente: **[nao verificado]**. | Carga e `_retratada_com_trilha`. (app/chamados/services/carga_pergunta_viva.py:107; app/chamados/juizes.py:282,292) |
| `viva_c6`, `_vida_carregada`, `_exportadas` | Alimentação transitória pelo C6/K6. (app/chamados/services/tarefa_do_dia.py:472,480,481) | Guardas de alimentação, exposição e residual. (app/chamados/services/tarefa_do_dia.py:445,245,325) |
| Caches de células, vínculos, ausências, slots, testemunhas e turnos | Carregadores e auxiliares reais, em memória. (app/ponto/turnos.py:2980,2990,3016; app/chamados/services/carga_pergunta_viva.py:88,105,115,140,180) | Métodos `ausencias_do_dia/grade/vinculo/turnos/plantadas/batida_na_janela/retratada`. (app/chamados/services/carga_pergunta_viva.py:146,149,157,163,184,187,191) |
| Saída: instâncias, ações, pergunta, razão, incerteza, rota, fila, verbos e fio | `_tarefa_pura` e `_portas_da_tarefa`, em memória. (app/chamados/services/tarefa_do_dia.py:182,249,252,331,534,542) | Projeção externa de sombra; descarta saída com `rota=None`. (app/chamados/management/commands/diff_e3_sombra.py:27,44,125) |

O command atual não mede o diff integral base × cura e torna invisível nessa projeção o novo `admin/porta=None`. Quantidade de mudanças reais entre os 232 dias, K6/E1/FK dos casos e compatibilidade ORM integral permanecem **[nao verificado]**. (app/chamados/management/commands/diff_e3_sombra.py:27,44,124)

## 11/11 02:31 · sessao bb320e41c3f5 · RAIZ-DA-DISPUTA · leitura · origem=Code

**O main tem três sítios persistentes de destino, distribuídos por duas funções; a eleição do destino ocorre também nos chamadores.** Nascimento e reuso escrevem em `_nascer`; reapontamento escreve em `reapontar_resolvedora`. O selo existente permite os dois **arquivos**, portanto não demonstra função escritora única. (`app/chamados/models.py:3014`, `:3017`, `:2982`, `:3006`; `app/chamados/services/fio.py:925`, `:927`; `app/core/tests/test_contract_escritores_estado.py:61`, `:182`.)

**Há uma distinção essencial entre main e c21:** no main, o núcleo pode confirmar a DS antes da geração das perguntas; o protocolo de ensaio, rollback e replay conservado pertence ao delta fornecido. (`app/chamados/services/disputa_emissao.py:587`, `:603`, `:670`, `:682`; materialização da raia anexada: [nao verificado].)

Uso estas abreviações de arquivo nas tabelas:

| Sigla | Arquivo |
|---|---|
| M | [app/chamados/models.py](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/models.py) |
| E | [app/chamados/services/disputa_emissao.py](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/disputa_emissao.py) |
| F | [app/chamados/services/fio.py](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/fio.py) |
| D | `app/chamados/services/declaracao_texto.py` |
| J | `app/chamados/juizes.py` |
| V | `app/chamados/services/validacao.py` |
| R | `app/chamados/reconciliador.py` |
| A | `app/chamados/services/acoes_disputa.py` |
| L | `app/chamados/services/limbo_folgas.py` |
| MT | `app/chamados/services/materializacao.py` |
| T/nome | `app/chamados/tests/nome` |

As provas abaixo são referências a testes inspecionados; resultado de execução nesta leitura: **[nao verificado]**. Identidade criptográfica da cópia com HEAD/PROD, frequência diária e quantidade atual de casos: **[nao verificado]**. [nao verificado]

**F0 — nomes reais e unidade do censo**

Não há `DisputaSupervisao.status`: aberta significa `fechada_em is None`. Na pergunta, `estado` também é propriedade calculada, não coluna de estado. (`M:1757`, `M:1823`, `M:3033`.)

Há três relações distintas:

| Relação | Significado e política |
|---|---|
| `DisputaSupervisao.chamado` | Chamado-pai; FK nullable, `SET_NULL`. (`M:1749`.) |
| `PerguntaDisputa.disputa` | Dona da pergunta; FK com `CASCADE`. (`M:2416`.) |
| `PerguntaDisputa.chamado_destino` | Destino/resolvedora; FK nullable, `SET_NULL`. **É o campo real de destino da pergunta; não há outro campo `destino`.** (`M:3385`.) |

**Script Python de leitura**

Este enumerador produz posições candidatas, delegações e uma fila explícita de revisão de operações genéricas. A classificação persistência/memória, as eleições e as cadeias estão nas tabelas seguintes; não são inferidas apenas pela presença de um nome no AST. A limitação de inferência está também documentada no censo existente. (`app/core/censo_escritas.py:15`, `:38`, `:43`.)

```python
import ast
from pathlib import Path

FIELDS = {
    "chamado_destino", "chamado_destino_id",
    "disputa", "disputa_id", "fechada_em", "motivo_fechamento",
}
OPS = {
    "create", "get_or_create", "update_or_create",
    "update", "bulk_create", "bulk_update", "save",
}

for path in sorted(Path("app").rglob("*.py")):
    if any(p in {"tests", "__pycache__", "node_modules", ".git"}
           for p in path.parts):
        continue
    if path.name == "tests.py" or path.name.startswith(("test_", "tests_")):
        continue

    source = path.read_text(encoding="utf8")
    tree = ast.parse(source, filename=str(path))
    parents = {
        child: node
        for node in ast.walk(tree)
        for child in ast.iter_child_nodes(node)
    }
    cache = {}

    def owners(node):
        result = []
        while node in parents:
            node = parents[node]
            if isinstance(node, (ast.ClassDef, ast.FunctionDef,
                                 ast.AsyncFunctionDef)):
                result.append(node)
        return result[::-1]

    def terminal(node):
        return node.attr if isinstance(node, ast.Attribute) \
            else getattr(node, "id", "")

    def strings(node):
        return {
            x.value for x in ast.walk(node)
            if isinstance(x, ast.Constant) and isinstance(x.value, str)
        }

    def emit(node, kind):
        scope = ".".join(x.name for x in owners(node))
        print(f"{path}:{node.lineno}|{scope}|{kind}")

    nodes = sorted(ast.walk(tree), key=lambda n: (
        getattr(n, "lineno", 0), getattr(n, "col_offset", 0)))

    for node in nodes:
        if (isinstance(node, ast.Attribute)
                and isinstance(node.ctx, ast.Store)
                and node.attr in FIELDS):
            emit(node, "ATR " + node.attr)

        if not isinstance(node, ast.Call):
            continue

        end = terminal(node.func)
        direct = {k.arg for k in node.keywords} & FIELDS
        own = owners(node)
        fn = next((x for x in reversed(own)
                   if isinstance(x, (ast.FunctionDef,
                                     ast.AsyncFunctionDef))), None)

        if fn not in cache:
            subtree = list(ast.walk(fn or tree))
            aliases = {}
            for x in subtree:
                if isinstance(x, ast.ImportFrom):
                    aliases.update({
                        a.asname or a.name: a.name for a in x.names
                    })
            touched = {
                ast.unparse(x.value) for x in subtree
                if isinstance(x, ast.Attribute)
                and isinstance(x.ctx, ast.Store) and x.attr in FIELDS
            }
            constructed = {
                y.id
                for x in subtree
                if isinstance(x, ast.Assign)
                and isinstance(x.value, ast.Call)
                and {k.arg for k in x.value.keywords} & FIELDS
                for y in x.targets if isinstance(y, ast.Name)
            }
            cache[fn] = aliases, touched, constructed

        aliases, touched, constructed = cache[fn]
        root = ast.unparse(node.func).split(".")[0]
        root = aliases.get(root, root)

        if direct and end in OPS | {
            "abrir", "abrir_conversa", "_sob_contrato", "cls"
        }:
            emit(node, "KW " + ",".join(sorted(direct)))

        if (end in {"create", "get_or_create", "update_or_create"}
                and root == "DisputaSupervisao"):
            emit(node, "DEFAULT DS")

        if (end == "reapontar_resolvedora"
                or aliases.get(end) == "reapontar_resolvedora"):
            emit(node, "DELEGA DESTINO")

        if end == "save" and isinstance(node.func, ast.Attribute):
            receiver = ast.unparse(node.func.value)
            if receiver in touched | constructed or strings(node) & FIELDS:
                emit(node, "SAVE alvo/candidato")

        target_class = any(
            isinstance(x, ast.ClassDef) and x.name == "PerguntaDisputa"
            for x in own
        )
        if (end in OPS and any(k.arg is None for k in node.keywords)
                and (target_class or root == "PerguntaDisputa")):
            emit(node, "REVER **kwargs")

        if end == "setattr":
            emit(node, "REVER setattr")
        if end in {"execute", "executemany", "raw", "RawSQL", "RunSQL"}:
            emit(node, "REVER SQL")
        if end in {"bulk_update", "update"} and strings(node) & FIELDS:
            emit(node, "REVER expressao/lista")
```

O script é um inventário de candidatos, não um analisador interprocedural completo: não resolve universalmente reexports, reflexão, campos montados fora da função, receivers desconhecidos ou SQL dinâmico. Esses casos precisam da revisão adicional abaixo; ausência universal por essas formas: **[nao verificado]**.

**F0 — saída pertinente, classificada após abertura dos arquivos**

Atribuição e seu `save` aparecem reunidos na mesma linha de operação.

| Sítio / função | O que efetivamente escreve | Quem elege | Atomic e `select_for_update` |
|---|---|---|---|
| `M:3014`, `M:3017`, `PerguntaDisputa._nascer` | **Insere `P.disputa` e `P.chamado_destino`.** | Dona e destino recebidos; dia/célula são resolvidos antes. (`M:2875`, `M:2896`.) | `_sob_contrato` abre atomic; `_nascer` trava C. Não há trava autônoma de D/Ch/P em todo nascimento. (`M:2819`, `M:2841`, `M:2845`.) |
| `M:2982`, `M:3006`, `_nascer` | **Troca destino da canônica; não troca dona.** | Canônica do universo do fato; aceita destino solicitado não nulo/diferente, após os dois juízes. (`M:2907`, `M:2921`, `M:2968`, `M:2972`, `M:2980`, `M:3008`.) | Mesmo atomic/C; a costura trava Ch; D/Ch/P são explicitamente pré-travados no ramo de rechaveamento. (`F:768`, `M:2947`.) |
| `F:925`, `F:927`, `reapontar_resolvedora` | **Troca destino, inclusive para `None`.** | Destino do chamador; `resolvedora_valida` e `_costura_nao_mata_o_fio` podem recusar. (`F:907`, `F:909`; `J:2176`.) | Atomic próprio; C; Ch na costura; D/Ch/P adicionais quando há colisão a retratar. (`F:881`, `F:889`, `F:904`, `F:768`.) |
| `M:367`, `_vivas_entre` | Só memória: reassocia instância do **mesmo PK**. | Guarda de identidade; consulta posterior do juiz. (`M:366`, `M:368`.) | Não salva P nessa operação. (`M:349`.) |
| `F:779`, `F:783`, `_costura_nao_mata_o_fio` | Só memória: simula destino e restaura no `finally`. | Julga vitalidade no pós-estado. (`F:778`, `F:781`.) | Não persiste essas atribuições. (`F:783`.) |
| `M:2876`, `_nascer` | Só memória: constrói proxy com `disputa`. | Insumo para `dia_do_turno_da_linha`. (`M:2875`.) | Proxy não é inserido; inserção real é `M:3014`. |
| `M:3387` | Exclusão de chamado pode tornar destino `NULL`. | Política `SET_NULL`; não elege substituto. (`M:3385`.) | Efeito da FK; transação concreta de uma exclusão em produção: [nao verificado]. |
| `M:2418` | Exclusão da dona elimina P. | `CASCADE`; não transfere a pergunta. (`M:2416`.) | Transação concreta de exclusão em produção: [nao verificado]. |

A implementação efetiva de `trava_na_ordem` usa `select_for_update` para C, D, Ch e P, nessa ordem; P só entra quando fornecida em `perguntas=`. (`M:2666`, `M:2675`, `M:2688`, `M:2691`, `M:2692`.)

**Pedidos que chegam ao nascimento/reuso**

| Sítio AST / função | Dona pedida e eleição do destino | Cadeia / fronteira |
|---|---|---|
| `M:2789`, `abrir`; `M:2801`, `abrir_conversa` | Encaminham parâmetros a `_sob_contrato`; não são novas eleições. | `_sob_contrato` → `_nascer`. (`M:2820`.) |
| `E:439`, `E:444`, `gerar_perguntas_disputa` | Dona recebida; destino de `_derivar_chamado_destino`. | Wrapper `E:670`, `E:682`; pendentes `E:294`; fábrica `app/chamados/services/fabrica_da_celula.py:172`. |
| `E:837`, `_criar_pergunta_se_couber` | Dona/destino recebidos. B pede `_chi`; grupo pede primeiro chamado na ordem recebida; ata/complemento pede chamado corrente. | `E:1154`, `E:1197`, `E:1176`, `E:1247`; agrupador HTTP/cron abaixo. |
| `D:376`, `declarar_hora_colab` | Pede a DS obtida pelo núcleo e **o chamado do ato**. | HTTP `app/chamados/views.py:1090`; atomic `D:348`, trava `D:352`, núcleo `D:360`, destino `D:377`. |
| `D:666`, `aplicar_declaracao` | Mesmo padrão, chamado da aplicação administrativa. | HTTP detalhe `app/chamados/views.py:480`; atomic `D:654`, trava `D:656`, núcleo `D:659`, destino `D:667`. |
| `E:1371`, `criar_disputa_ancora_de_chamado` | DS aberta selecionada/criada; destino é `chamado_limbo`. | Atomic/trava `E:1322`, `E:1331`; pedido `E:1377`; ação HTTP `A:189`, despacho `F:524`. |
| `A:263`, `acao_reabrir_disputa_admin` | Conversa na DS reaberta; destino omitido → default `None`. | Despacho HTTP `F:581`; reabertura `A:259`; default `M:2794`. |

A eleição do derivador **não é menor PK**: filtra chamados VIVOS do colaborador, motivos atendidos, exclui candidato já com P do motivo não materializada, compara o dia e desempata pelo turno/estágio; só devolve destino com resultado único. (`E:198`, `E:212`, `E:226`, `E:231`, `E:237`, `E:238`, `E:245`; desempate `E:153`, `E:161`.)

A canônica da **pergunta** é outra eleição: resposta não vazia vence; empate pelo menor PK. Isso não estabelece menor PK para o **chamado-destino**. (`F:1225`, `F:1231`, `F:1232`, `F:1233`.)

**Todos os pedidos nominais a `reapontar_resolvedora` no main**

| Sítio / função | Regra do destino | Cadeia e atomic externo relevante |
|---|---|---|
| `M:437`, `_gate_pergunta_viva` | Sobrevivente do C8: mais novo por criação/PK. (`M:390`; `J:2101`.) | Portas `reconciliar`, `cancelar`, `encerrar`, competência. (`M:685`, `M:786`, `M:1617`, `M:1637`.) Todos os caminhos dessas portas até HTTP/cron: [nao verificado]. |
| `R:954`, `reemitir_chamado_do_dia` | Chamado obtido por `abrir`, exigido VIVO. (`R:910`, `R:936`.) | Chamado por reata `R:1621`; esta chega pela emissão `E:464` ou reconciliação `R:1433`, `R:1526`. |
| `R:1642`, `reatar_pergunta_orfa` | Histórico somente quando há **um** candidato. (`R:1623`.) | Mesmas portas; signal Batida `app/ponto/signals.py:207`, `:215`; signal filho `app/chamados/signals.py:350`, `:368`. |
| `R:1655`, `reatar_pergunta_orfa` | Resultado do derivador. (`R:1556`.) | Mesmas portas; atomic abrangente depende do chamador. [nao verificado] para todos os percursos. |
| `D:370`, `declarar_hora_colab` | Chamado recebido; candidata local sem destino. (`D:363`, `D:364`.) | HTTP `app/chamados/views.py:1090`; atomic/trava `D:348`, `D:352`. |
| `L:244`, `reatar_perguntas_ao_limbo` | Pai LIMBO recebido; seleção mais recente. (`L:220`, `L:221`.) | Command `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:110`; atomic `:99`, trava `:104`. |
| `L:282`, `agrupar_furo` | Pai recebido; no lote, último PK da ordenação crescente. (`L:308`, `L:309`.) | Atomic/trava `L:270`, `L:274`; detector `app/ponto/management/commands/detectar_ausencias.py:761`, cron `app/config/crons.py:364`; lote `L:331`, encadeamento `app/config/crons.py:1509`. |
| `F:2174`, `_migrar_sob_trava` | Novo chamado recebido; só migra a P canônica. (`F:2155`, `F:2165`.) | Atomic/trava `F:2083`, `F:2095`; `Chamado.reabrir` `M:1415`; irmão eleito por estágio/PK `J:2145`. |
| `app/chamados/management/commands/reatar_beco_folga_ao_limbo.py:127`, `_reverter` | Destino anterior do JSON, inclusive `None`. (`:118`, `:127`.) | Porta management command; sem atomic externo próprio nesse método; reapontar abre o seu. (`F:881`.) |

A prosa histórica do reconciliador não basta para atribuir essa reata a um cron atual: o corpo declara a migração para evento e o cron como vigia. (`R:1539`, `R:1540`.)

**F0 — estado persistido da DS: todos os valores**

| Sítio / função | Valores gravados | Eleição, atomic e trava |
|---|---|---|
| `E:587`, `_abrir_ou_agregar_disputa` | Nascimento: `fechada_em=NULL`, `fechada_por=NULL`, `resolucao=''`, `motivo_fechamento=''`. Defaults `M:1757`, `M:1766`, `M:1773`, `M:1794`. | Atomic `E:521`; universo travado `E:530`; escolhe aberta por `.first()` e ordering de abertura decrescente. (`E:531`; `M:1804`.) |
| `E:1123`, agrupador de órfãos | Mesmos defaults de nascimento. | Atomic `E:1052`; fecho/travas `E:1059`, `E:1010`; seleção aberta `E:1096`. |
| `E:1358`, âncora | Mesmos defaults de nascimento. | Atomic `E:1322`; universo `E:1331`; seleção aberta `E:1332`. |
| `M:1932`, `M:1936`, `fechar` | `fechada_em=now`, autor, resolução; motivo **`vazia`**, **`veredito`**, **`desligamento`**, **`admin_encerra`** ou **`competencia_trancada`**. (`M:1780`, `M:1792`, `M:1903`, `M:1935`.) | Atomic/trava `M:1905`, `M:1910`. Atenção: respostas/vivas são examinadas **antes** desse atomic interno. (`M:1886`, `M:1895`.) |
| `M:2149`, `M:2152`, save `M:2161`, `reabrir_admin` | `fechada_em=None`, autor `None`, resolução `''`, motivo `''`; contador admin +1, eventual motivo novo. (`M:2150`, `M:2153`, `M:2159`.) | Guardas fechada/limite/usuário `M:2138`; sem atomic/trava próprios. |
| `M:2173`, `M:2176`, save `M:2177`, `reabrir_sistema` | Mesmo reset dos quatro campos, sem consumir contador admin. | Aberta → `False`; fechada → salva e `True`. Atomic/trava pertencem ao chamador. (`M:2167`, `M:2181`.) |

As migrations também entram no levantamento, mas como definição/alteração de schema: criação de `P.disputa`, adição nullable do destino, criação do fechamento nullable e adição do motivo com default vazio. Não são novos escritores de eleição de runtime. (`app/chamados/migrations/0014_perguntadisputa.py:27`; `0016_s84_pergunta_chamado_destino.py:16`; `0013_disputasupervisao.py:24`; `0049_disputa_motivo_fechamento.py:12`.)

**Cadeias dos escritores de fechamento/reabertura**

Todos os pedidos de fechar abaixo chegam a `M:1936`; não são outros escritores da coluna. (`M:1855`, `M:1936`.)

| Chamador direto de `fechar` | Fronteira confirmada / contexto |
|---|---|
| `app/api/foto_recebida.py:80` | Upload HTTP `app/api/views.py:869`, cópia `app/api/views_core.py:854`; atomic externo: [nao verificado]. |
| `R:412` | Reata `R:1619` → emissão ou reconciliação por signal; atomic abrangente de todos os percursos: [nao verificado]. |
| `R:612` | Signal/reconciliação `R:1432` ou cartório `app/ponto/services/cartorio.py:970`; atomic local anterior termina antes desse fechamento. (`R:592`, `R:612`.) |
| `R:1171` | Consumidor `R:1375` → command `app/chamados/management/commands/consumir_eventos_celula.py:31` → cron `app/config/crons.py:394`. |
| `M:1950` | `admin_encerra`; chamador produtivo externo dessa porta: [nao verificado]. |
| `M:2072` | Remoção de motivo por signals Posto/Escala: `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`; atomic local `M:2025` termina antes do fechar. |
| `app/colaboradores/services/desligamento.py:142` | HTTP `app/colaboradores/views.py:1434` ou command agendado `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; atomic `desligamento.py:103`. |
| `app/ponto/services/entrada_sincronizada.py:59` | Batida HTTP `app/api/views.py:803`, cópia `views_core.py:798`; atomic externo no ponto da chamada: [nao verificado]. |
| `MT:586` | Resposta HTML/REST → `E:1541`; atomic da resposta `E:1503` já terminou nessa chamada. |
| `MT:1333` | Materialização `MT:1378`, validação `V:469`, ação HTTP `A:139` ou passivo `app/chamados/services/passivo_validado.py:405`. |
| `E:1284` | Agrupador HTTP `A:222` ou comandos de alerta/detecção; atomic `E:1052`; crons `app/config/crons.py:362`, `:363`, `:364`. |
| `E:1964` | Resolver por chamado resolvido; wrapper transacional e trava em `E:1796`, `E:1797`; receiver `app/chamados/signals.py:92`. |
| `app/chamados/services/competencia_trancada.py:204` | Trancar período `app/ponto/services/fechamento.py:1428` ou command `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`; atomic por DS `competencia_trancada.py:197`. |
| `app/chamados/services/becos.py:57` | Signal P após commit `app/chamados/signals.py:238` ou cron `app/config/crons.py:663`; atomic/trava `becos.py:41`, `:51`. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88` | Management command; atomic `:87`; registro de cron: [nao verificado]. |

| Chamador de reabertura | Cadeia e proteção |
|---|---|
| `M:1575`, sistema | Ressuscitação; `Chamado.reabrir` `M:1451`, HTTP `app/chamados/views_arquivar.py:86` ou cartório `app/ponto/services/cartorio.py:1633`; atomic e ordem completa `M:1557`, `M:1562`. |
| `MT:686`, sistema | Ausência rejeitada por signal `app/ponto/signals.py:276`; atomic/trava `MT:675`, `MT:679`. |
| `V:218`, sistema | Validação recusada; no fluxo de validar, atomic/trava `V:425`, `V:431`. O helper isolado não envolve toda a reabertura em atomic. (`V:201`, `V:217`.) |
| `app/chamados/services/relavra_via.py:261`, sistema | Desfaz carimbo sem lastro; atomic por P `:219`, trava `:230`; chamada específica da fábrica pode proibir reabrir fechada. (`app/chamados/services/fabrica_da_celula.py:209`.) |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:165`, sistema | Management command; atomic `:146`, releitura `:164`; cron automático: [nao verificado]. |
| `A:259

[truncado em 20000 de 47820 caracteres]

## 11/11 02:29 · sessao 0eb5a9c564d8 · - · - · origem=Ronald (tmux)

**BLOQUEIA — B5-j perdeu a mordida no universo extraído.** A produção ainda chama `batidas_apuraveis` em `_carregar_batidas`, mas o tripwire só vigia chamadas a `parear_turnos`/`_turnos_do_colab_puro` e procura queries dentro da função chamadora; a query passou para outro método. O comentário afirmando que está “no escopo [...] para o tripwire [...] ver” ficou falso. `app/ponto/turnos.py:3011`, `app/ponto/turnos.py:3015`, `app/ponto/turnos.py:3017`, `app/ponto/turnos.py:3027`, `app/ponto/turnos.py:3036`; `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:63`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:118`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:125`.

Reproduzi sem escrever: carreguei as funções AST do tripwire e substituí **em memória**, somente no helper, `batidas_apuraveis(None)` por `Batida.objects`. `universos(original) == universos(mutante)` deu `True`; ambos retornaram `crus=[]`. A lacuna interprocedural já é descrita pelo próprio teste, mas o split introduziu justamente essa forma no novo caminho. `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:27`, `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:103`.

**BLOQUEIA — relógio da consulta não atravessa CF.** A nova vida é lida com `hoje=localdate(self.agora)`, mas CF usa `_tz.now()` no teto e não recebe `agora`; assim o resultado novo da tarefa pode depender do relógio externo, mesmo conservando o mesmo `agora`. `app/chamados/services/tarefa_do_dia.py:478`, `app/chamados/services/tarefa_do_dia.py:480`; `app/chamados/juizes.py:1414`; `app/escala/servico_jornada.py:375`, `app/escala/servico_jornada.py:376`.

Reproduzi com CF real via apoio, vínculo diurno 08–17, sem ausências/batidas e a mesma carga: só mudar `tz.now` de 12h para 20h mudou `(None,None,None)` para `('E','saida_sem_entrada','ENTROU')`, sem erro registrado. Os testes existentes fixam `now=lambda: AGORA`; portanto não confrontam relógio real versus borda injetada. O comportamento já existe em CF avulso, mas c8b passa a consumi-lo na tarefa. `app/chamados/tests/e3_c8b_apoio.py:89`, `app/chamados/tests/e3_c8b_apoio.py:91`; `app/escala/servico_jornada.py:351`, `app/escala/servico_jornada.py:376`, `app/escala/servico_jornada.py:471`.

**FILA — outra alimentação temporal antiga continua pelo template.** CF chama `fim_turno(ec.tipo_escala,data_evento)` sem `ec`/`celulas`; `fim_turno` encaminha isso a `marcos_turno`, que lê o template quando `ec=None`, embora a contagem posterior de CF leia os marcos congelados do vínculo/célula. Não é divergência nova carga versus avulso; é uma segunda fonte já existente no teto, agora alcançável pela tarefa. `app/escala/servico_jornada.py:375`, `app/escala/servico_jornada.py:89`, `app/escala/servico_jornada.py:68`, `app/escala/servico_jornada.py:71`, `app/escala/servico_jornada.py:427`.

**O que os testes realmente exercitam.** [nao verificado]

- Corpos C6, CF, slot/materialização são extraídos do fonte via AST; não são simples lambdas devolvendo veredito. O extrator remove decorators e executa a função isolada, portanto não equivale a importar a aplicação com o ORM. `app/chamados/tests/e3_c8b_apoio.py:93`, `app/chamados/tests/e3_c8b_apoio.py:102`, `app/chamados/tests/e3_c8b_apoio.py:129`; `app/chamados/tests/e4_apoio.py:8`, `app/chamados/tests/e4_apoio.py:13`.
- São doubles: `CargaDaJanela` inteira, carregamento de batidas, turnos avulsos **e** corpo puro do pareamento (ambos devolvem a mesma lista `turnos` recebida), grade, tranca em lote, modelos/managers, timezone e catálogo de parcialidade (`lambda a:a.parcial`). Essa paridade de turnos é estabelecida pela infraestrutura, não pelo pareador real. `app/chamados/tests/e3_c8b_apoio.py:63`, `app/chamados/tests/e3_c8b_apoio.py:72`, `app/chamados/tests/e3_c8b_apoio.py:112`, `app/chamados/tests/e3_c8b_apoio.py:119`, `app/chamados/tests/e3_c8b_apoio.py:123`, `app/chamados/tests/e3_c8b_apoio.py:132`.
- O teste anunciado “carga da tarefa avulso/lote” evita o `__init__` produtivo usando `cls.__new__`, injeta grade/perguntas/decisões/efeitos/fila e substitui K6/`alimentar_fila`. Ele prova ligação dos corpos nessa fixture, não leitura ORM desses insumos. `app/chamados/tests/test_e3_c8b_sem_banco.py:218`, `app/chamados/tests/test_e3_c8b_sem_banco.py:221`, `app/chamados/tests/test_e3_c8b_sem_banco.py:227`, `app/chamados/tests/test_e3_c8b_sem_banco.py:232`.
- O teste básico de batidas usa `_turnos` previamente preenchido: nunca executa o pareador e só verifica uma leitura. `app/chamados/tests/test_e3_c8b_sem_banco.py:505`, `app/chamados/tests/test_e3_c8b_sem_banco.py:507`, `app/chamados/tests/test_e3_c8b_sem_banco.py:512`, `app/chamados/tests/test_e3_c8b_sem_banco.py:514`.
- Os testes AS/B puros injetam o bool `viva_c6`; o juiz “dia sem trabalho” é substituído por `lambda d:True`, e B declara FK/texto/E1 hipotéticos. Eles não provam escritores reais produzindo os dados usados pela cura. `app/chamados/tests/test_e3_c8b_sem_banco.py:41`, `app/chamados/tests/test_e3_c8b_sem_banco.py:53`; `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:91`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:119`.

**Execução segura realizada:** `PYTHONDONTWRITEBYTECODE=1 python3 -m unittest -q chamados.tests.test_e3_tarefa_admin_sem_banco chamados.tests.test_e3_c8b_sem_banco`: RC0, 32 testes, OK. Também executei o agregado dos 14 mutantes: RC0; o runner exige falha por assertion, sem `ERROR`. Resultados de execução não têm arquivo de saída por exigência de só leitura; executores em `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:115`, `app/chamados/tests/test_e3_c8b_sem_banco.py:47`, `app/chamados/tests/e3_c8b_mutantes.py:17`, `app/chamados/tests/e3_c8b_mutantes.py:87`, `app/chamados/tests/e3_c8b_mutantes.py:105`.

**Limite dos mutantes:** `C6_pareia_janela_larga` morre porque o stub `Janela.turnos` levanta `AssertionError`, capturado pelo C6 e denunciado por `env.erros`; isso protege o caminho escolhido, sem provar a geometria das janelas. `app/chamados/tests/e3_c8b_apoio.py:75`, `app/chamados/tests/e3_c8b_mutantes.py:65`, `app/chamados/tests/test_e3_c8b_sem_banco.py:417`, `app/chamados/juizes.py:1430`.

**RED ORM que faltam para certificar c8b** — são lacunas dos novos testes indicados, não alegação de inexistência de toda cobertura antiga: [nao verificado]

1. B com pergunta/decisões/FK reais, resposta registrada pelo escritor real, e tarefa completa mostrando `validar/admin`; A/S gerado por ausência+batidas e lavra real, incluindo ausência de perguntas e perguntas mortas. Os atuais recebem essas condições prontas. `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:105`, `app/chamados/tests/test_e3_tarefa_admin_sem_banco.py:119`, `app/chamados/tests/test_e3_c8b_sem_banco.py:35`.
2. Igualdade do dict inteiro avulso/lote passando por `CargaDaTarefa.__init__` real, com N=1/N=4 e contagem SQL incluindo relações lazy, grade, cobertura, tranca e K6; atualmente o construtor é contornado e o pai substituído. `app/chamados/tests/test_e3_c8b_sem_banco.py:227`, `app/chamados/tests/e3_c8b_apoio.py:63`.
3. Turnos reais D..D e D−1..D+1, nas bordas do pad, overnight, troca de escala, retratadas, geradora fora da vigência e ordenação empatada. Atualmente avulso/puro devolvem a mesma lista sintética. `app/chamados/tests/e3_c8b_apoio.py:121`, `app/chamados/tests/e3_c8b_apoio.py:123`.
4. Ausências SQL com estados de cobrança, parciais reais/catálogo, sobreposição e range malformado, confrontadas com `ausencias_do_dia`; atualmente managers/Q são simulados e parcialidade é uma flag sintética. `app/chamados/tests/e3_c8b_apoio.py:17`, `app/chamados/tests/e3_c8b_apoio.py:39`, `app/chamados/tests/e3_c8b_apoio.py:120`.
5. K6 carregado com objetos ORM: revogada sem substituta, substituição, empate `gerado_em/pk`, lista parcial/não anotada/outra tripla; teste novo usa objetos `Registro` e `Query` sintéticos. A suíte antiga tem boas formas ORM de revogação/substituição/empate, mas avulsas. `app/chamados/tests/test_e3_c8b_sem_banco.py:437`, `app/chamados/tests/test_e3_c8b_sem_banco.py:459`; `app/folha/tests/test_k6_competencia_entregue.py:72`, `app/folha/tests/test_k6_competencia_entregue.py:78`, `app/folha/tests/test_k6_competencia_entregue.py:85`.
6. Frescor depois dos escritores reais — resposta, validação, materialização/retratação de batida, lavra, tranca/toque e exportação — comparando carga nova/antiga e memo avulso. O teste atual modifica só dicionário de ausências em memória e compara `_pv_memo`; a suíte ORM anterior testa frescor da consulta do app, não `p.viva_c6` da nova carga. `app/chamados/tests/test_e3_c8b_sem_banco.py:290`, `app/chamados/tests/test_e3_c8b_sem_banco.py:298`; `app/chamados/tests/test_e3_v4_orm.py:101`.
7. `agora` fixo antes/depois do fim do turno com relógio ambiente variando, incluindo DNA diferente do template. A infraestrutura atual iguala os relógios. `app/chamados/tests/e3_c8b_apoio.py:89`, `app/escala/servico_jornada.py:375`.

Paridade ORM completa e mudanças reais dos 232 dias: **[não verificado]**.

## 11/11 02:29 · sessao 0a0b269ffb28 · - · - · origem=Ronald (tmux)

Auditoria da carga concluída, somente leitura.

**BLOQUEIA — existe `carga.vinculo != avulso`.** Se a célula do colab 7 aponta para um vínculo do colab 8 cuja vigência cobre o dia, `escala_vigente` passa a geradora diretamente ao juiz e devolve esse vínculo; a carga consulta apenas o balde do colab 7 e devolve `None`. Reproduzi os corpos reais por AST, sem Django/DB/arquivos. A FK não declara igualdade entre os colaboradores. Ocorrência em produção: [não verificado]. Fontes: `app/escala/servico_jornada.py:55`, `app/chamados/services/carga_pergunta_viva.py:157`, `app/ponto/turnos.py:2990`, `app/escala/alimentacao.py:279`, `app/escala/models.py:1692`.

**BLOQUEIA — o teste de paridade substitui justamente esse carregador avulso.** `ambiente_c6` troca `escala_vigente` por `vinculo_avulso`, que chama diretamente `vinculo_do_dia` sobre a lista fornecida. Logo, não prova paridade com a carga real de `escala_vigente`. Fonte: `app/chamados/tests/e3_c8b_apoio.py:97`.

**BLOQUEIA — B5-j ficou cego ao carregador extraído.** O detector examina o escopo que chama `_turnos_do_colab_puro`; a query agora fica em `_carregar_batidas`. Reproduzi uma mutação textual em memória substituindo a porta por `Batida.objects` sem filtro de retratação: original e mutante retornaram os mesmos universos, com `crus=[]`. A produção continua usando a porta. Fontes: `app/ponto/tests/test_tripwire_b5j_universo_de_apuracao.py:118`, `app/ponto/turnos.py:3011`, `app/ponto/turnos.py:3023`.

**BLOQUEIA — relógio fora da borda.** A tarefa passa `hoje=localdate(agora)` ao C6, mas `classificar_falta` continua comparando o teto com `_tz.now()`. A consulta nova não depende exclusivamente do `agora` recebido. Fontes: `app/chamados/services/tarefa_do_dia.py:478`, `app/escala/servico_jornada.py:374`.

Paridade estática restante:

- **Ausências:** mesmo efeito `cobranca`, exclusão de `rejeitada`, mesmo recorte inclusivo de cobertura e preservação de todas as sobrepostas; o resultado usado é existência de ausência inteira, sem desempate por ordem. Fontes: `app/chamados/services/carga_pergunta_viva.py:98`, `app/ponto/turnos.py:2511`, `app/ponto/turnos.py:2531`, `app/ponto/turnos.py:2757`, `app/escala/servico_jornada.py:338`.
- **Turnos:** reutiliza `_turnos_do_colab_puro`, reaplicado à janela exata; pad de dois dias anteriores e um seguinte; batidas recortadas por `[00:00 pad_ini, 00:00 pad_fim+1)`; universo vivo vem da porta; cópias evitam compartilhar carimbos de pareamento. Fontes: `app/chamados/services/carga_pergunta_viva.py:163`, `app/ponto/turnos.py:1834`, `app/ponto/turnos.py:1918`, `app/ponto/turnos.py:3017`.
- **Células/grade/DNA:** carregadas pela porta comum e repassadas ao leitor e ao mesmo derivador de DNA; a grade exige a lista inteira de vínculos. Fontes: `app/ponto/turnos.py:2980`, `app/chamados/services/carga_pergunta_viva.py:149`, `app/chamados/services/carga_pergunta_viva.py:178`, `app/escala/services/leitor_celula.py:350`, `app/escala/utils.py:84`.
- **Ordenação:** batidas têm unicidade `(colaborador,timestamp)`; vínculos têm unicidade `(colaborador,data_inicio)`, eliminando empate legítimo da chave `(ativa,data_inicio)`. Não encontrei outro contraexemplo determinístico por ordenação. Fontes: `app/ponto/models.py:105`, `app/escala/models.py:1003`. Paridade ORM integral: [não verificado].

RED ORM necessário neste recorte: geradora de outro colaborador confrontando `escala_vigente`, `carga.vinculo`, slot e C6; janelas D..D e D−1..D+1 com noturno/troca de vínculo/batida retratada; ausências sobrepostas/rejeitadas/range malformado; execução em dois relógios reais com o mesmo `agora`. Ausência dessa cobertura integral: `app/chamados/tests/e3_c8b_apoio.py:61`, `app/chamados/tests/e3_c8b_apoio.py:121`, `app/chamados/tests/e3_c8b_apoio.py:138`.

## 11/11 02:28 · sessao c70532bdb5bc · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **BLOQUEIA — consumidor de `viva_c6` fora do universo carregado.** `_tarefa_pura` expõe também pergunta de motivo do mapa cuja posição não existe nas instâncias; `_ler_vida` só seleciona dia sem trabalho, posição acesa ou motivo fora do mapa. Com dia de trabalho, instâncias existentes e pergunta de marco ausente, falta a única atribuição de `viva_c6`; o acesso direto gera `AttributeError`. Reproduzi sem arquivos usando o corpo real, infraestrutura existente, `sem_trabalho=False`, apenas instância início e pergunta fim sem atributo. Referências: `app/chamados/services/tarefa_do_dia.py:245`, `:313`, `:467`, `:480`; infraestrutura usada: `app/chamados/tests/test_e3_c8b_sem_banco.py:19`. **RED ORM faltante:** dia trabalho com grade atual início/fim e pergunta antiga de intervalo removido; tarefa avulsa/lote não pode lançar nem fabricar decisão.

2. **BLOQUEIA — nova exposição desfaz o horizonte da decisão.** `_dec` e passivo obedecem `ato_em <= agora`; C6 novo recebe apenas a data de `agora`, porém mata imediatamente por `validada_em` atual, independentemente de ser posterior. Se RESPOSTA existia em T1 e VALIDACAO nasceu em T2, consulta em T1 preserva RESPOSTA em `_dec`, mas o C6 atual suprime `validar/admin`. A porta real escreve validação e linha E1 com mesmo instante, portanto o cenário é alcançável. Referências: `app/chamados/services/tarefa_do_dia.py:385`, `:394`, `:478`, `:245`, `:133`; `app/chamados/juizes.py:1346`; `app/chamados/services/validacao.py:134`. **RED ORM faltante:** consultar o mesmo `agora=T1` antes/depois de registrar validação em T2; comparar dict inteiro.

3. **FILA/contrato K6 — alimentação não é validada pelo juiz.** `exportacoes` fornecido pula seleção por empresa/mês/ano e anotação; uma linha vigente de outra tripla pode devolver `True`/registro para tripla pedida, e uma lista incompleta pode devolver `False`. Linhas reais não anotadas usam `e_vigente`, que consulta a vigência individual e introduz N consultas; não tornam automaticamente o resultado incorreto quando a tripla é correta. Referências: `app/folha/export.py:583`, `:586`; `app/folha/models.py:121`, `:134`, `:200`. O único caller produtivo novo encontrado anota e filtra a tripla corretamente: `app/chamados/services/tarefa_do_dia.py:437`, `:438`. **RED ORM faltante:** outra tripla, mistura de triplas, linha antiga sem anotação, empate `(gerado_em,pk)`, vigente invalidada sem substituta; o teste atual só fornece listas corretas anotadas em `app/chamados/tests/test_e3_c8b_sem_banco.py:473`.

4. **Cache C6: não achei segundo juiz nem leitor produtivo fora da carga.** Único escritor produtivo é o retorno de `pergunta_viva`; leitores produtivos são `expor_admin` e a guarda residual. A carga não relê após `_vida_carregada`; não possui invalidação. O único consumidor produtivo externo encontrado é o command de sombra, que percorre dias e lê saídas, sem mutação nesse percurso. Referências: `app/chamados/services/tarefa_do_dia.py:480`, `:245`, `:326`, `:445`; `app/chamados/management/commands/diff_e3_sombra.py:115`, `:125`. Uso após escrita/outro lote: **[não verificado]**; não confundir com o bug de atributo não carregado acima.

5. **Residual A/S não deriva contradição nem alcança `nunca_bateu`/`indefinida` por esse ramo.** Exige literalmente `sem_trabalho`, `fato_em_ausencia`, ausência de pergunta viva elegível e competência não entregue; cria ação administrativa do dia sem pergunta. Pergunta fora do fato e última retração saem antes. Em dia de trabalho o residual não executa. Referências: `app/chamados/services/tarefa_do_dia.py:230`, `:234`, `:325`, `:330`. Isso não prova resultado real dos dias anexados: K6, E1 e demais complementos produtivos **[não verificado]**.

Cadeia real do valor A/S e dos campos principais:

| Campo/valor | Quem escreve/decide | Quem lê no caminho novo |
|---|---|---|
| `FATO_EM_AUSENCIA` | Supra-juiz: fato em dia encerrado + cobertura aprovada; parcial sem período sai como cadastro; `app/ponto/supra_juiz.py:379`, `:382`, `:400` | `derivar_veredito`, mapa em `app/escala/models.py:1619`, função `:1624` |
| `CelulaDia.veredito/via/ata` | Cartório chama porta `app/ponto/services/cartorio.py:1813`; atribuição real `app/ponto/portas/celula.py:1033`; admin prevalece em `:1003` | Grade transporta `app/escala/services/leitor_celula.py:424`; tarefa lê `app/chamados/services/tarefa_do_dia.py:325`; C6 lê `app/chamados/juizes.py:1359` |
| `trabalha/dna/escala_geradora` | Operação gerar/editar declaradas na porta `app/ponto/portas/celula.py:7`, `:14` | Grade entrega insumo para juiz de dia sem trabalho `app/escala/services/leitor_celula.py:428`; tarefa consulta `app/chamados/services/tarefa_do_dia.py:207` |
| `materializada_em/via_resolucao` | Atribuição única em memória `_escrever_carimbo`, `app/chamados/models.py:3097`; portas persistem `:3152`, `:3190` | C6 `app/chamados/juizes.py:1357`, `:1383`, `:1421`; identidade tarefa `app/chamados/services/tarefa_do_dia.py:230` |
| `respondida_em/validada_em` | Carimbo com resposta `app/chamados/models.py:3130`; validação especializada `app/chamados/services/validacao.py:134`; desfazer em memória `app/chamados/models.py:3163` | C6 `app/chamados/juizes.py:1346`, `:1362`; tarefa lê decisão E1 em `app/chamados/services/tarefa_do_dia.py:133` |
| `DecisaoPergunta.ato/ato_em` | Exemplo escritor real validação registra em `app/chamados/services/validacao.py:139` | Carga ordena por pergunta/instante/pk e corta horizonte `app/chamados/services/tarefa_do_dia.py:385`; `_pela_decisao` usa última `:132` |
| `ExportacaoDominio` | Registro real `app/folha/models.py:174`, `:182` | K6 `app/folha/export.py:583` |
| `_vigente_anotado/vigente` | Anotador compara `(gerado_em,pk)` e atribui em memória `app/folha/models.py:220`, `:224` | `e_vigente` `app/folha/models.py:123`; `so_vigentes` `:200`; K6 `app/folha/export.py:583` |
| `invalidada_em` | Porta `invalidar` `app/folha/models.py:143`, atribuição `:158`, save `:162` | K6 `app/folha/export.py:586` |
| `viva_c6` | Somente resultado C6 `app/chamados/services/tarefa_do_dia.py:480` | `:245`, `:326` |
| `exportada/_exportadas` | Alimentação pelo K6 `app/chamados/services/tarefa_do_dia.py:438`, `:472` | Exclusão de candidatas `:475`; guardas puras `:245`, `:325` |
| `acoes/pergunta_id/razao` novos | Somente dict em memória `app/chamados/services/tarefa_do_dia.py:249`, `:252`, `:331` | Projeção de portas `:534`; command sombra `app/chamados/management/commands/diff_e3_sombra.py:125` |

Censo completo de todos os escritores/leitores dinâmicos desses campos: **[não verificado]**.

## 11/11 02:28 · sessao a614d13f920e · FIO-E0-V4-E3 · construtor · origem=Code

**lei: PARE — encontrei a condição de parada do C6.**

A cura exigida altera os dois leitores citados:

- `universo_adesao` seleciona exclusivamente células `nunca_bateu` e incrementa `dias` para cada uma. Num contraexemplo com um trabalho NB e uma folga NB, preservar o trabalho e absolver a folga reduz `dias` de **2 para 1**, independentemente dos marcos. [adesao.py:34](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:34), [adesao.py:40](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:40).
- `dia_nunca_bateu_sem_adesao` compara somente o literal: para a folga curada, passa necessariamente de **True para False**. [fabrica_da_celula.py:89](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/fabrica_da_celula.py:89).

A contagem alimenta a descrição usada na abertura do chamado. Isso demonstra mudança no universo e no texto calculado; **não demonstra atualização de um chamado já persistido**. [adesao.py:17](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:17), [adesao.py:77](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/chamados/services/adesao.py:77). Fechamento, duplicação ou alteração efetiva dos chamados existentes: [nao verificado].

O retentor solicitado é `folha.export.motivos_retencao_celula`: consulta `CelulaDia.VEREDITOS_ACUSAM` e aplica as ressalvas dos protestos e da pausa. [export.py:1236](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/folha/export.py:1236), [export.py:1256](/home/ronald/wt-codex-fio-e0-v4-e3-022700/app/folha/export.py:1256).

Parei antes de editar, conforme a ordem expressa do C6. Cura, RED/GREEN ORM, mutantes e ruff não foram executados; não há certificação desta volta. [nao verificado — execução suspensa pela condição de parada].

## 11/11 02:28 · sessao 61a58b3a3472 · FIO-E0-V4-E4B · sonda · origem=Code

Usei a saída raspada fornecida, sem nova execução. Os números abaixo são transcrições dessa saída, marcados **[nao verificado]** por não terem sido medidos nesta sessão. [nao verificado]

**1. FOTO_SOMBRA**

`2026-11-11 02:03:21.722000-03:00`, com `hoje=2026-11-11`: **posterior** ao pouso informado da E4a-2 em 10/11, 14:5x local. Pelo critério temporal solicitado, a foto serve como pós E4a-2. [nao verificado] [nao verificado]

A saída termina com `rc_sombra=0` e imprime ambos os censos. Há mensagens `ERROR tipo_marco_divergente`, com divergências `marcos_faltantes=S x ata=E` e `marcos_faltantes=E x ata=S`. [nao verificado] Nesse ramo, o código registra a divergência e retorna, sem lançar a exceção construída; portanto, essas mensagens não indicam, por si, abortamento da sonda. [materializacao.py:299](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/materializacao.py:299)

**2. Contadores impressos**

| Censo | Chave | Valor | Evidência |
|---|---|---:|---|
| Alcance | `fio_morto_alcance` | 579 | [nao verificado] |
| Alcance | `alcance_universo` | 2733 | [nao verificado] |
| Alcance | `alcance_de_hoje_pra_frente` | 0 | [nao verificado] |
| Vivo | `fio_morto` | 4 | [nao verificado] |
| Vivo | `fio_morto_de_hoje_pra_frente` | 0 | [nao verificado] |

**3. Decomposição e conferência**

| Rota\|papel | Quantidade | Evidência |
|---|---:|---|
| aguarda_decisao_de_ausencia\|Aguardando decisao do DP | 2 | [nao verificado] |
| aguarda_decisao_de_ausencia\|Aguardando documento do colaborador | 1 | [nao verificado] |
| decisao_do_dia_pendente\|regularizar_batida | 266 | [nao verificado] |
| fio_do_admin\|admin-batida | 3 | [nao verificado] |
| fio_do_admin\|admin-cadastro | 176 | [nao verificado] |
| porta_fila\|cobrar | 1062 | [nao verificado] |
| porta_fila\|cobrar,decidir | 33 | [nao verificado] |
| porta_fila\|cobrar,validar | 3 | [nao verificado] |
| porta_fila\|decidir | 70 | [nao verificado] |
| porta_fila\|decidir,validar | 15 | [nao verificado] |
| porta_fila\|validar | 144 | [nao verificado] |
| porta_pergunta\|cobrar | 259 | [nao verificado] |
| porta_pergunta\|cobrar,validar | 1 | [nao verificado] |
| porta_pergunta\|historico | 10 | [nao verificado] |
| porta_pergunta\|validar | 109 | [nao verificado] |
| sem_porta\|nenhuma porta | 579 | [nao verificado] |

A soma de **todas as rotas confere com `alcance_universo=2733`**; o componente `sem_porta|nenhuma porta=579` confere com `fio_morto_alcance=579`. [nao verificado] Esse é o significado do mapa no código: ele cobre o universo inteiro. [vigia_fio.py:551](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:551)

| Competência — somente `sem_porta` | Total | Causas impressas | Evidência |
|---|---:|---|---|
| 10/2026 | 132 | c=12; d=115; e=5 | [nao verificado] |
| 11/2026 | 447 | a=2; c=4; d=339; e=102 | [nao verificado] |

**Confere:** `132 + 447 = 579`; os subtotais de causas também conferem com cada competência. A saída imprime `SEM_PORTA_LINHAS=579` e `OMITIDOS=2121`. [nao verificado] O código preserva todas as linhas `sem_porta`; a omissão limita outras rotas. [vigia_fio.py:628](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:628)

**4. Comparação com o DRY de 10/11**

| Classe do DRY | DRY | Saída atual | Movimento demonstrável | Evidência |
|---|---:|---|---|---|
| Total | 385 | 579 | Subiu | [nao verificado] |
| E3 | 232 | nao separado pela saida | Indeterminável | [nao verificado] |
| segue_sem_porta | 82 | nao separado pela saida | Indeterminável | [nao verificado] |
| classe e | 45 | 107 | Subiu | [nao verificado] |
| nao_sei | 20 | nao separado pela saida | Indeterminável | [nao verificado] |
| outros | 6 | nao separado pela saida | Indeterminável | [nao verificado] |

A classificação atual imprime `alcance_a=2`, `alcance_b=0`, `alcance_c=16`, `alcance_d=454`, `alcance_e=107`. Dentro de `e`: `pergunta_viva_sob_chamado_terminal=53` e `sem_pergunta_viva=54`. [nao verificado] Não há, na saída fornecida, correspondência individual com E3, `segue_sem_porta`, `nao_sei` e `outros`. [nao verificado]

**5. “Hoje pra frente” > 0?**

**Não:** `alcance_de_hoje_pra_frente=0`, `fio_morto_de_hoje_pra_frente=0` e `SEM_PORTA_HOJE_PRA_FRENTE=0`. Nenhuma célula/linha individual de hoje+ é mostrada no trecho fornecido. [nao verificado]

As chaves que contam hoje+ são `alc['alcance_de_hoje_pra_frente']` e `viv['fio_morto_de_hoje_pra_frente']`, incrementadas nos respectivos ramos de data maior ou igual a hoje. [vigia_fio.py:648](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:648) [vigia_fio.py:420](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:420)

## 11/11 02:27 · sessao b9969e85d8f2 · VERIFICADOR-EM-FABLE · verificador · origem=Code

**BLOQUEIA a promessa de “contador 0 a partir daqui”: o contador continua acumulando histórico, sem marco da cura. A negativa está implementada para o pedido canônico `Agent/verificador` reconhecido como final.** (`bin/disjuntor.py:990`, `bin/disjuntor.py:2173`, `bin/disjuntor.py:1844`)

1. **BLOQUEIA — contador histórico não mede “a partir daqui”.** O escritor acrescenta IDs à lista sem remover os anteriores; o leitor retorna seu comprimento, sem janela temporal ou marco da alteração. Portanto, histórico já persistido permanece positivo depois da cura. É preciso distinguir histórico e ocorrências posteriores à cura para sustentar essa promessa. (`bin/disjuntor.py:990`, `bin/disjuntor.py:993`, `bin/disjuntor.py:2173`, `bin/disjuntor.py:2239`) Os seis mencionados no anexo estarem efetivamente nessa lista: **[nao verificado]**.

2. **FILA — quem escreve, quem lê e alcançabilidade do contador.** O escritor lógico é `decide_b3._fn`, alimentado por `_reprovacoes_finais`; a persistência passa por `_com_estado`. O leitor direto é `_b3_sem_obra`; `ler` e `linha` propagam o valor até `gerar_estado.py`. O teste B3 também lê esse campo. (`bin/disjuntor.py:976`, `bin/disjuntor.py:990`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:372`, `bin/disjuntor.py:2173`, `bin/disjuntor.py:2047`, `bin/disjuntor.py:2242`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:430`)

   A chamada negada não alcança esse escritor, mas outro final **com obra** chega à varredura por `_nega_b3`; o Stop também a aciona. Assim, o zero não fica impossível de falsificar por construção. Há controle anterior que injeta um transcript final sem obra e exige contador 1, além do mutante `B-sem-contador`. (`bin/disjuntor.py:1847`, `bin/disjuntor.py:1849`, `bin/disjuntor.py:1780`, `bin/disjuntor.py:1087`, `bin/hook_stop_fila1.py:1204`, `bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:431`, `bin/tests/test_disjuntor.sh:1597`)

   A cobertura, porém, é dos transcripts da sessão fornecida, com metadata `agentType == 'verificador'`; não constitui censo global de aberturas. Sem obra, conta mesmo sem veredito concluído. (`bin/disjuntor.py:433`, `bin/disjuntor.py:836`, `bin/disjuntor.py:840`, `bin/disjuntor.py:844`)

3. **FILA — existem saídas da guarda; abertura real por todas elas não está demonstrada.** Com a chave desligada, `main_pretool` pula a decisão: é uma exceção explícita do instrumento. Outra grafia de `subagent_type` e ausência de `tool_input` também não entram na negativa nova. O lançador aceitar esses payloads e abrir o agente é **[nao verificado]**. (`bin/disjuntor.py:35`, `bin/disjuntor.py:1899`, `bin/disjuntor.py:1902`, `bin/disjuntor.py:1842`, `bin/disjuntor.py:1844`)

   Prompt não textual não escapa automaticamente: há conversão por `str`; se o resultado for reconhecido como final sem obra, recebe a negativa. A aceitação desse formato pela ferramenta real é **[nao verificado]**. (`bin/disjuntor.py:1843`, `bin/disjuntor.py:597`, `bin/disjuntor.py:1847`)

   `_pretool`/`pretool` não engolem suas exceções; `main_pretool` nega quando `_ramo_b3` reconhece o ramo. Já a guarda anterior aos imports protege apenas Agent detectado como abertura com cópia; um verificador sem `wt-` não recebe essa mesma proteção. Falha concreta de import e comportamento do lançador diante dela: **[nao verificado]**. (`bin/disjuntor.py:1807`, `bin/disjuntor.py:1924`, `bin/disjuntor.py:1931`, `bin/disjuntor.py:82`, `bin/disjuntor.py:97`)

   Uma abertura que não atravesse esse hook não encontra essa negativa. A configuração copiada aponta para `/home/ronald/saas-hasner/bin/disjuntor.py`; configuração ativa e correspondência desse caminho com o hash entregue: **[nao verificado]**. (`.claude/settings.json:62`, `.claude/settings.json:68`, `bin/disjuntor.py:1847`)

4. **FILA — não aparece falsa negativa nova dentro do contrato dos parsers.** Pedido não final pula a guarda; final com `_obra_de(prompt)` válido segue para a decisão B3 existente. Ambos têm controles positivos no caso novo. “Obra legível” aqui exige as âncoras de `_obra_de`: apenas colocar um ID válido nu, como `LEITURA FINAL OBRA-N`, não basta. (`bin/disjuntor.py:169`, `bin/disjuntor.py:170`, `bin/disjuntor.py:602`, `bin/disjuntor.py:1844`, `bin/disjuntor.py:1849`, `bin/tests/test_disjuntor.sh:1473`, `bin/tests/test_disjuntor.sh:1474`)

5. **FILA — ordem preservada; há efeitos posteriores deliberadamente pulados.** B1 continua antes. O ramo `codex_decide` também permanece antes textualmente, mas é separado: para `Agent`, `cmd` é vazio, portanto essa mesma chamada não executaria a decisão Codex. (`bin/disjuntor.py:1818`, `bin/disjuntor.py:1823`, `bin/disjuntor.py:1828`, `bin/disjuntor.py:1836`)

   A negativa nova pula B4 e suas atualizações/disparos. Não pula uma varredura B3 anteriormente executada nessa chamada: sem obra, o código anterior já evitava `_nega_b3`. Também não perde aprovação do CENSO, pois `verificador` pertence a `SO_LEITURA`. O registro externo `NEGOU`, resposta `deny` e tentativa de registro no RELATO continuam acontecendo. Isso altera qual motivo aparece e adia a manutenção do B4, sem liberar o pedido negado. (`bin/disjuntor.py:1847`, `bin/disjuntor.py:1849`, `bin/disjuntor.py:151`, `bin/disjuntor.py:1853`, `bin/disjuntor.py:1860`, `bin/disjuntor.py:1862`, `bin/disjuntor.py:1868`, `bin/disjuntor.py:1906`, `bin/disjuntor.py:1909`)

6. **FILA — o mutante morde a guarda; `O9a` não cobre cauda inválida.** A mutação retira exatamente a nova negativa, fazendo o primeiro pedido alcançar a passagem pelo B4 e derrubar a expectativa do caso. Há controle da cópia sem mutação e controles positivos que exigem stdout vazio e nenhuma `FALHA` nova. (`bin/tests/test_disjuntor.sh:1518`, `bin/tests/test_disjuntor.sh:1471`, `bin/disjuntor.py:1849`, `bin/disjuntor.py:1860`, `bin/tests/test_disjuntor.sh:1491`, `bin/tests/test_disjuntor.sh:1517`, `bin/tests/test_disjuntor.sh:98`)

   `O9a` é capturado como **ID** inválido, com cauda ausente; falha no primeiro `fullmatch`. Para cobrir a segunda condição, falta no caso novo um par como `O9 noturno-foo` negado e `O9 NOTURNO-FOO` aceito. O par aparece no teste anterior de concordância entre leitores. (`bin/disjuntor.py:157`, `bin/disjuntor.py:164`, `bin/disjuntor.py:619`, `bin/tests/test_disjuntor.sh:1472`, `bin/tests/test_disjuntor.sh:743`, `bin/tests/test_disjuntor.sh:744`)

7. **FILA — as demais classes citadas não reaparecem no trecho novo.** Não nasce segundo juiz B3: a guarda reutiliza `_obra_de` → `_obra_lida`, compartilhado com `obras_da_linha`. Existe validação distinta do CENSO para pasta/trilha, fora dessa decisão. (`bin/disjuntor.py:1845`, `bin/disjuntor.py:607`, `bin/disjuntor.py:638`, `bin/disjuntor.py:158`, `bin/disjuntor.py:1711`)

   Não há relógio novo na guarda; o instante continua vindo da borda. `'*'` significa “pertence ao ramo B3, sem obra” em `_ramo_b3`; `None` significa ausência de obra em `_obra_de` ou ausência do ramo em `_ramo_b3`. O consumidor atual respeita essa diferença e não aceita `'*'` como ID. (`bin/disjuntor.py:1897`, `bin/disjuntor.py:1900`, `bin/disjuntor.py:1844`, `bin/disjuntor.py:1789`, `bin/disjuntor.py:1794`, `bin/disjuntor.py:1799`, `bin/disjuntor.py:1925`)

A suíte shell integral permanece **[nao verificado]**: seus helpers criam e removem arquivos, incompatíveis com somente leitura. As sondas foram feitas apenas em memória, com I/O substituído; não comprovam operação em produção. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

## 11/11 02:25 · sessao 4fb254e5076a · - · - · origem=Ronald (tmux)

- **FILA — alcance depende da porta e da classificação.** A negativa nova só roda para `tool_name == 'Agent'`, `subagent_type == 'verificador'` literal e prompt reconhecido como final; outra grafia ou `tool_input` ausente não entra nesse ramo (`bin/disjuntor.py:1841–1847`). Isso demonstra ausência da negativa específica; o lançador aceitar tais payloads e abrir o agente é **[nao verificado]**. A chave desligada pula `pretool` por desenho (`bin/disjuntor.py:1898–1902`); a chave é definida como desligamento geral (`bin/disjuntor.py:34–38`, `209–211`).

- **FILA — exceção dentro do hook reconhecido fecha; falha de import tem outra cobertura.** `_pretool` e `pretool` não engolem exceção (`bin/disjuntor.py:1804–1811`, `1814–1875`); `main_pretool` usa `_ramo_b3` e responde `deny` no ramo reconhecido, inclusive sem obra via `'*'` (`bin/disjuntor.py:1788–1801`, `1924–1932`). Antes dos imports, entretanto, `_guarda_import` protege apenas Agent com cópia detectada; Agent verificador com prompt sem `wt-` não satisfaz o detector (`bin/disjuntor.py:65–84`, `87–105`). Falha concreta de import e comportamento do lançador perante essa falha: **[nao verificado]**.

- **FILA — prompt não textual não é escape automático.** Ambos os classificadores convertem `prompt` por `str(...)` (`bin/disjuntor.py:1793–1794`, `1843–1845`); se o resultado contém final reconhecido sem obra, a negativa continua funcionando (`bin/disjuntor.py:597–599`, `1844–1847`). A aceitação desses valores pela ferramenta real é **[nao verificado]**. Há diferenças possíveis de representação na leitura posterior: `_veredito_e_obra` usa `json.dumps` quando o conteúdo não é string (`bin/disjuntor.py:651–655`), portanto uniformidade completa entre representações não está selada pelo caso novo (`bin/tests/test_disjuntor.sh:1471–1474`).

- **FILA — cobertura efetiva de todas as aberturas não demonstrada.** A cópia configura `PreToolUse` com matcher vazio e comando absoluto `/home/ronald/saas-hasner/bin/disjuntor.py pretool` (`.claude/settings.json:62–68`). O código só chama `main_pretool` na opção `pretool` (`bin/disjuntor.py:2309–2311`); uma abertura que não passe por esse hook não encontra essa negativa. Configuração ativa, correspondência do caminho absoluto ao hash entregue e existência de aberturas alternativas reais: **[nao verificado]**.

- **FILA — não há segundo juiz novo de obra nesse diff.** A porta chama `_obra_de`, que consulta `_obra_lida`; a leitura de obras nas linhas usa o mesmo validador (`bin/disjuntor.py:602–620`, `631–640`, `1845–1847`). Existe validação distinta para IDs do CENSO, explicitamente voltada a pasta/trilha e ao wrapper (`bin/disjuntor.py:158–162`, `1602–1607`, `1711–1714`), além do token da célula com fallback ao reconhecedor compartilhado (`bin/disjuntor.py:1656–1671`). Isso não equivale a novo predicado B3 duplicado (`bin/disjuntor.py:1845–1847`).

- **FILA — relógio e troca de modelo ficam fora do item (1).** A negativa nova não testa horário (`bin/disjuntor.py:1844–1847`); o instante do hook é lido em `main_pretool` e passado adiante (`bin/disjuntor.py:1897–1900`). O agente segue com `model: inherit` na cópia (`.claude/agents/verificador.md:5`). Reset efetivamente conferido, troca posterior para Fable, fallback por limite e medição de três dias: **[nao verificado]**, pois o dado fornecido contém instruções para essas etapas, sem evidência de execução.

Não executei a suíte que cria árvores e mutantes: os próprios helpers escrevem arquivos temporários (`bin/tests/test_disjuntor.sh:69–79`, `1488–1497`).

## 11/11 02:25 · sessao b70755730f06 · - · - · origem=Ronald (tmux)

1. **BLOQUEIA — “esperado 0 a partir daqui” não corresponde ao contador existente.** O escritor mantém uma lista acumulada de IDs, acrescentando somente os ainda ausentes; o leitor devolve `len(lista)`, sem corte temporal, marco da mudança ou expiração. Portanto um histórico já persistido permanece positivo após a cura. Os “6 em 24 h” estarem nessa lista é **[nao verificado]**. (`bin/disjuntor.py:990`, `bin/disjuntor.py:993`, `bin/disjuntor.py:2173`, `bin/disjuntor.py:2239`)

2. **FILA — mapa completo de escrita/leitura.** Escritor lógico: `decide_b3._fn`, que incorpora `sem_obra`; persistência por `_com_estado`, sob trava e substituição do arquivo. Leitor direto: `_b3_sem_obra`; propagação por `ler` e `linha`; apresentação por `gerar_estado.py`. (`bin/disjuntor.py:976`, `bin/disjuntor.py:990`, `bin/disjuntor.py:1057`, `bin/disjuntor.py:335`, `bin/disjuntor.py:369`, `bin/disjuntor.py:372`, `bin/disjuntor.py:2171`, `bin/disjuntor.py:2047`, `bin/disjuntor.py:2242`, `bin/gerar_estado.py:280`, `bin/gerar_estado.py:281`)

3. **FILA — o escritor permanece alcançável; zero não fica impossível de falsificar por construção.** A chamada negada retorna antes da varredura, mas outra chamada final com obra legível alcança `_nega_b3` → `estado_b3` → `julga_b3` → `decide_b3`; o Stop também chama `julga_b3`. Assim transcripts históricos ou de chamadas que escaparam ainda podem aumentar a lista. (`bin/disjuntor.py:1846`, `bin/disjuntor.py:1849`, `bin/disjuntor.py:1780`, `bin/disjuntor.py:1087`, `bin/disjuntor.py:1078`, `bin/hook_stop_fila1.py:1204`)

4. **FILA — cobertura do contador tem limites próprios.** A fonte examina apenas `subagents` do `transcript_path` fornecido, exige `agentType == 'verificador'` e conta final sem obra mesmo sem veredito concluído. Não prova universo global de chamadas nem janela de 24 h. Há teste anterior que injeta transcript sem obra, aciona Stop e exige contador 1. (`bin/disjuntor.py:433`, `bin/disjuntor.py:834`, `bin/disjuntor.py:840`, `bin/disjuntor.py:844`, `bin/disjuntor.py:655`, `bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:431`)

5. **FILA — `'*'` e `None` têm contratos diferentes, respeitados pelos consumidores atuais.** `_obra_de(None reconhecido)` retorna `None`; `_ramo_b3` retorna `'*'` para chamada pertencente ao ramo B3 sem obra, reservando `None` para fora do ramo. Seu consumidor testa a presença do ramo e nega na exceção; a nova regra consulta diretamente `_obra_de`, portanto não aceita `'*'` como obra. (`bin/disjuntor.py:602`, `bin/disjuntor.py:610`, `bin/disjuntor.py:1789`, `bin/disjuntor.py:1794`, `bin/disjuntor.py:1799`, `bin/disjuntor.py:1924`, `bin/disjuntor.py:1925`, `bin/disjuntor.py:1845`)

Sonda executada somente em memória confirmou: após acumular `s1`, varredura posterior vazia com instante futuro mantém contador 1; `_obra_de` devolve `None` e `_ramo_b3` devolve `'*'` para o mesmo final sem obra. A semântica exercitada está em `bin/disjuntor.py:990`, `bin/disjuntor.py:2173` e `bin/disjuntor.py:1794`; suíte integral **[nao verificado]**.

## 11/11 02:25 · sessao c781d947d859 · - · - · origem=Ronald (tmux)

Correção das contagens preliminares: são **15 chamadores diretos de `DisputaSupervisao.fechar`, 5 de `reabrir_sistema` e 1 de `reabrir_admin`** na varredura AST de `.método(...)` em produção; chamadas construídas dinamicamente fora dessa forma: [nao verificado]. Os endereços conferidos seguem abaixo.

| Chamada direta de `fechar` | Cadeia até a porta | Atomic externo ao método `fechar` |
|---|---|---|
| `app/api/foto_recebida.py:80`, `resolver_disputas_de_foto` | HTTP `api_upload_foto` em `api/views.py:847`→`:869`; cópia `api/views_core.py:832`→`:854`. | Helper não abre atomic (`foto_recebida.py:57`); atomic do HTTP: [nao verificado]. |
| `app/chamados/reconciliador.py:412`, `lavrar_sem_turno_aberto` | `reatar_pergunta_orfa` chama `lavrar` em `reconciliador.py:1619`; `reconciliar_perguntas_orfas_colab`→`reconciliar_chamados_colab:1433`; signal Batida `app/ponto/signals.py:207`→`:215`; signal filho `app/chamados/signals.py:350`→`:368`. | `lavrar_sem_turno_aberto` não abre atomic (`reconciliador.py:340`); depende da porta que disparou signal; [nao verificado] para todos os percursos. |
| `reconciliador.py:612`, `retratar_perguntas_absolvidas_por_celula` | Mesmo signal Batida→`reconciliar_chamados_colab:1432`; signal filho→mesma função; consumidor de evento `reconciliador.py:1365`; cartório `app/ponto/services/cartorio.py:969`→`:970`. | Atomic `reconciliador.py:592` acaba antes do fechamento `:612`; consumidor declara ausência de atomic envolvendo o colaborador em `:1366`. O método `fechar` abre o seu próprio atomic (`models.py:1905`). |
| `reconciliador.py:1171`, `encerrar_orfas_pela_lampada` | `consumir_eventos_celula:1375`→comando `app/chamados/management/commands/consumir_eventos_celula.py:31`→cron `app/config/crons.py:394`. | Atomic de cada P em `reconciliador.py:1146` termina antes do fechamento `:1171`; sem atomic geral do consumidor (`:1366`). |
| `app/chamados/models.py:1950`, `admin_encerra` | Não encontrei chamador produtivo de `admin_encerra`; só a definição em `models.py:1943`; cadeia HTTP/cron/signal: [nao verificado]. | Sem atomic próprio em `admin_encerra`; delega a `fechar` (`models.py:1950`). |
| `models.py:2072`, `remover_motivo` | Signal Posto `app/colaboradores/signals.py:77`; signal Escala `app/escala/signals.py:161`. | Atomic próprio `models.py:2025` **termina antes** de `self.fechar` em `:2072`; locks desse atomic só continuam se havia atomic externo. Atomic externo dos dois signals: [nao verificado]. |
| `app/colaboradores/services/desligamento.py:142`, `executar_desligamento` | HTTP `app/colaboradores/views.py:1434`; comando `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`; cron `app/config/crons.py:440`. | Dentro do atomic do ato `desligamento.py:103`. |
| `app/ponto/services/entrada_sincronizada.py:59`, `resolver_cobranca_da_entrada` | HTTP `api_bater_ponto` em `app/api/views.py:400`→`:803`; cópia `views_core.py:387`→`:798`. | Helper não abre atomic (`entrada_sincronizada.py:19`); atomic do HTTP no ponto da chamada: [nao verificado]. |
| `app/chamados/services/materializacao.py:586`, `_materializar_ausencia_de_resposta` | Resposta HTTP HTML `app/chamados/views.py:1283` e REST `app/api/views.py:2632`→`disputa_emissao.responder_pergunta_disputa_core:1541`→helper. | Atomic do núcleo da resposta `disputa_emissao.py:1503` termina antes de chamar o helper em `:1541`; helper não abre atomic abrangente em `materializacao.py:496`. Atomic maior da casca: [nao verificado]. |
| `materializacao.py:1333`, `_fechar_disputa_se_quitada` | `materializar_perguntas_validadas_da_disputa:1378`→HTTP `acoes_disputa.acao_regularizar_disputa:139`, `fio.processar_post_fio:670`, ou `validacao.validar_pergunta:469`; também passivo `passivo_validado.py:405`. | `_fechar_disputa_se_quitada` não abre atomic (`materializacao.py:1302`). Validação chama o materializador **depois** do atomic da validação (`validacao.py:425`, `:469`); passivo envolve a cauda em atomic (`passivo_validado.py:404`). |
| `app/chamados/services/disputa_emissao.py:1284`, `criar_disputa_a_partir_de_chamados_orfaos` | HTTP `acoes_disputa.acao_iniciar_disputa_orfaos:222`; comandos `app/ponto/management/commands/processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:802`; registros de cron `app/config/crons.py:362`, `:363`, `:364`. | Dentro do atomic do agrupador (`disputa_emissao.py:1052`) após `_trava_do_repasse` (`:1060`). |
| `disputa_emissao.py:1964`, `_resolver_perguntas_por_chamado_resolvido` | Wrapper `resolver_perguntas_por_chamado_resolvido:1798`; receiver S143, `becos.fechar_beco`, reconciliador, comando `reconciliar_fantasmas`, portas admin são declarados no corpo do wrapper (`disputa_emissao.py:1786`); execução de todos esses percursos: [nao verificado]. | Wrapper usa atomic se não havia um; se já havia, conserva o externo; trava universo antes do corpo (`disputa_emissao.py:1796`, `:1797`). |
| `app/chamados/services/competencia_trancada.py:204`, `encerrar` | `disparar_no_ato:282` ou processo do comando `:290`; ato de trancar em `app/ponto/services/fechamento.py:1428`; comando manual `app/chamados/management/commands/encerrar_por_competencia_trancada.py:20`. | Atomic por DS em `competencia_trancada.py:197`. |
| `app/chamados/services/becos.py:57`, `fechar_beco` | Signal post_save P `app/chamados/signals.py:228`→`on_commit:238`→`fechar_beco_da_disputa:237`→`becos.py:78`; também comando `fechar_becos_disputa.py:57`, cron `app/config/crons.py:663`. | Atomic próprio em `becos.py:41`, ordem universo C→D→Ch em `:51`, antes dos resolvers e do fechamento. |
| `app/chamados/management/commands/encerrar_perguntas_com_fato.py:88`, `Command.handle` | Porta management command `handle:27`; registro de cron desse comando: [nao verificado]. | Atomic explícito em `:87`. |

Todos chegam ao mesmo escritor persistente: `fechar` atribui/salva em `app/chamados/models.py:1932`/`:1936`; o atomic interno e sua chamada à ordem são `:1905`/`:1910`.

| Chamada direta de `reabrir_sistema` | Cadeia até a porta | Atomic externo ao escritor |
|---|---|---|
| `app/chamados/models.py:1575` | `ChamadoColaborador.ressuscitar_perguntas:1512`; chamadores diretos: `ChamadoColaborador.reabrir:1451`, HTTP `app/chamados/views_arquivar.py:86`, cartório `app/ponto/services/cartorio.py:1633`. | Atomic `models.py:1557`, ordem completa `:1562`, incluindo DS/P envolvidas. |
| `app/chamados/services/materializacao.py:686` | `devolver_absorvidas_da_ausencia_rejeitada:639`; signal `app/ponto/signals.py:276`→`_consumidor_ausencia:279`→`on_commit:272`→`:262`. | Atomic próprio `materializacao.py:675`; ordem C→D→Ch→P `:679`. |
| `app/chamados/services/validacao.py:218` | `devolver_validacao_recusada:163` chamado por `validar_pergunta:464`→cascas HTTP de validação, incluindo fio `app/chamados/services/fio.py:609`. | No percurso `validar_pergunta`, chamada ocorre dentro do atomic `validacao.py:425`, ordem `:431`. O helper isolado não possui atomic abrangendo a reabertura; atomic `:201` cobre só descarimbo da validação. |
| `app/chamados/services/relavra_via.py:261` | `desfazer_carimbo_sem_lastro:187`; fábrica `app/chamados/services/fabrica_da_celula.py:169`, `:209`; cartório chama fábrica em `app/ponto/services/cartorio.py:1008`; signal Chamado chama cartório em `app/chamados/signals.py:216`; cron cartório registrado `app/config/crons.py:616`. | Atomic por P em `relavra_via.py:219`; ordem C→D→P `:230`. A fábrica do julgamento passa `reabrir_fechada=False` em `fabrica_da_celula.py:209`, portanto esse percurso específico não reabre fechada. |
| `app/chamados/management/commands/reabrir_orfas_enterradas.py:165` | Management command `handle:53`; cron automático desse comando: [nao verificado]. | Atomic em `:146`; chamada condicional após reler fechamento em `:164`. |

O escritor `reabrir_sistema` não toma trava/atomic próprios, salva o reset em `models.py:2177` e retorna True em `:2181`; a garantia transacional é a dos chamadores acima.

O único chamador direto de `reabrir_admin` é `app/chamados/services/acoes_disputa.py:259`: HTTP fio/modal→`processar_post_fio:581`→`acao_reabrir_disputa_admin:235`; a casca não abre atomic envolvendo reabrir+pergunta. Depois da gravação em `models.py:2161`, chama `PerguntaDisputa.abrir_conversa` em `acoes_disputa.py:263`. Atomic externo de todas as portas HTTP que chegam nessa casca: [nao verificado].

## 11/11 02:25 · sessao 166caaf2105c · - · - · origem=Ronald (tmux)

- **FILA — O9a cobre ID inválido, não cauda inválida.** O regex extrai `oid='O9a', cauda=None`; `_obra_lida` rejeita no primeiro `fullmatch`, sem avaliar o ramo de cauda. Para exercitar esse ramo, usar ID válido com cauda capturada inválida, como `obra O9 noturno-foo`, mais controle `obra O9 NOTURNO-FOO`. A comparação entre contador e topo já inclui ambas, mas a nova unidade não inclui esse par. (`bin/disjuntor.py:157`, `bin/disjuntor.py:163`, `bin/disjuntor.py:169`, `bin/disjuntor.py:619`; `bin/tests/test_disjuntor.sh:1472`, `bin/tests/test_disjuntor.sh:743`)

- **FILA — o mutante morde no fluxo isolado; resultado integral não verificado.** A mutação remove exatamente a guarda; sem ela, `ob=None` pula `_nega_b3` e segue ao B4. Em execução em memória com dependências de I/O substituídas, os dois primeiros prompts mudaram de negativa para passagem, derrubando a primeira expectativa do caso. A unidade shell devolve `1` se faltar a mensagem; o harness acusa falha se o caso mutado passar. (`bin/disjuntor.py:1845`, `bin/disjuntor.py:1849`, `bin/disjuntor.py:1860`; `bin/tests/test_disjuntor.sh:1471`, `bin/tests/test_disjuntor.sh:1499`, `bin/tests/test_disjuntor.sh:1518`)

- **FILA — controles presentes e positivos.** Há passagem com obra legível e passagem não final; `_passa` exige stdout vazio e nenhuma `FALHA` nova. Há também controle da cópia sem mutação, incluindo explicitamente a nova unidade. (`bin/tests/test_disjuntor.sh:98`, `bin/tests/test_disjuntor.sh:1473`, `bin/tests/test_disjuntor.sh:1474`, `bin/tests/test_disjuntor.sh:1491`, `bin/tests/test_disjuntor.sh:1517`)

- **FILA — alcance do selo é restrito.** As negativas verificam texto com `grep`, não decodificam `permissionDecision`; o emissor real escreve `deny`. A nova unidade cobre o payload canônico `Agent/verificador`, com prompt string e `tool_input`, sem ensaiar as outras portas listadas na pergunta. (`bin/tests/test_disjuntor.sh:322`, `bin/tests/test_disjuntor.sh:323`, `bin/tests/test_disjuntor.sh:1471`; `bin/disjuntor.py:1774`)

- **FILA — escritor do contador tem controle anterior de alcançabilidade.** O caso B3 injeta diretamente um transcript de verificador final sem obra, roda Stop e exige `b3_sem_obra=1`; portanto a suíte conserva uma prova de leitura de ocorrência externa à nova porta. Isso não prova sua frequência ou presença em produção. (`bin/tests/test_disjuntor.sh:427`, `bin/tests/test_disjuntor.sh:428`, `bin/tests/test_disjuntor.sh:429`, `bin/tests/test_disjuntor.sh:431`)

A suíte shell integral não foi executada: ela cria e remove arquivos temporários, incompatível com somente leitura. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

## 11/11 02:24 · sessao b0a72e4a8b1a · FIO-E0-V4-E3 · construtor · origem=Code

Diff B+A/S e entrega: [E3_TAREFA_ENTREGA.txt](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:1).

32 testes em memória verdes e 14 mutantes mortos por assertion. [Evidências](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:257). [nao verificado]

Suíte ORM bloqueada por ausência de `django`; certificação e medição dos 232 dias seguem **[nao verificado]**. [Limites](/home/ronald/wt-codex-fio-e0-v4-e3-010719/app/E3_TAREFA_ENTREGA.txt:284). [nao verificado]

## 11/11 02:22 · sessao 764012eeb554 · - · - · origem=Ronald (tmux)

F0 de destino/dona — conferido em leitura.

| Sítio main | Natureza | Valor/regra | Transação/travas |
|---|---|---|---|
| `models.py:3014`, persistência `:3017`, `PerguntaDisputa._nascer` | Nascimento de `P.disputa` e `P.chamado_destino` | Disputa e destino recebidos pelo chamador; dia/célula resolvidos previamente. | `_sob_contrato`, atomic `:2819`; `_nascer`, atomic `:2841`, trava C `:2845`; implementação `select_for_update` C/D/Ch/P `:2666/:2675/:2688/:2692`. |
| `models.py:2982`, save `:3006`, `_nascer` | Reuso: troca destino; preserva dona, retorno `:3008` | Canônica do universo do fato `:2907/:2921`; destino solicitado, se não nulo/diferente `:2968`, aceito por `resolvedora_valida` `:2972` e `_costura_nao_mata_o_fio` `:2980`. | Mesmo atomic; Ch travado pela costura `fio.py:768`; D/Ch/P explicitamente pré-travados no rechaveamento `models.py:2947`. |
| `fio.py:925`, save `:927`, `reapontar_resolvedora` | Troca destino, inclusive `None` | Destino do chamador; juízes `:907/:909`; `None` permitido por `juizes.py:2176`. | Atomic `fio.py:881`, C `:889`, Ch via `:768`; D/Ch/P adicionais quando colisão `:904`. |
| `fio.py:779/:783`, `_costura_nao_mata_o_fio` | Só memória | Destino pós-estado para consultar `pergunta_viva`; restauração no `finally`. | Não persiste essas atribuições. `fio.py:778`. |
| `models.py:367`, `_vivas_entre` | Só memória | Reassocia a mesma PK, guarda `:366`; consulta o juiz `:368`. | Não salva a pergunta nesse método. `models.py:349`. |
| `models.py:2876`, `_nascer` | Proxy de identidade, só memória | `cls(disputa=...)` passado a `dia_do_turno_da_linha`. | Não salva o proxy. `models.py:2875`. |
| `models.py:3387` | Efeito de exclusão | `SET_NULL` em `P.chamado_destino`; não elege substituto. | Política ORM da FK, definida em `:3385`. |
| `models.py:2418` | Efeito de exclusão | Excluir dona elimina P por `CASCADE`; não muda sua dona. | FK definida em `:2416`. |

O nome real do destino é **`PerguntaDisputa.chamado_destino`**; não existe um segundo campo de destino distinto dessa FK na definição inspecionada. Dona é `PerguntaDisputa.disputa`, campo separado. `models.py:2416`, `models.py:3385`.

Há dois sumidouros genéricos a declarar:

- `PerguntaDisputa.descarimbar_em_lote`, `models.py:3252`, aceita `**outros`, proíbe apenas campos do carimbo em `:3255` e expande em `update` `:3266`. **Os chamadores atuais não passam destino/dona**: ressuscitação passa apenas validação/resposta em `models.py:1566`; correção passa o dicionário de resposta/validação de `disputa_emissao.py:1593` em `:1630`.
- `PerguntaDisputa.save`, `models.py:3413`, encaminha `*args, **kwargs` a `super().save` `:3423`, dentro de atomic `:3422`. O full save produtivo identificado da P é a inserção `models.py:3017`; demais saves inspecionados restringem campos. Generalização para chamadas não identificadas: [nao verificado].

`abrir(**campos)` não tem allowlist: `models.py:2776` → `:2789` → `_sob_contrato(**kw)` `:2806/:2820` → expansão no construtor `:3014`. `_sob_contrato` cobra o contrato de saída `:2821`, não os nomes dos kwargs. Não encontrei chamador produtivo passando `disputa_id` ou `chamado_destino_id` nessa expansão; efeito de fornecer simultaneamente FK objeto e `_id`, dependente da implementação Django ausente da cópia: [nao verificado].

Chamadores completos de `reapontar_resolvedora`:

| Chamador | Eleição e cadeia confirmada |
|---|---|
| `models.py:437`, `_gate_pergunta_viva` | Sobrevivente eleito por C8 `models.py:390`; C8 prefere `-criado_em,-pk`, `juizes.py:2101`. Chega por `reconciliar` `models.py:650/:685`, `cancelar` `:786`, `encerrar` `:1617`, `encerrar_por_competencia_trancada` `:1637`. Cadeia de **todos** consumidores dessas portas até cada HTTP/cron: [nao verificado]. |
| `reconciliador.py:954`, `reemitir_chamado_do_dia` | Resultado de `ChamadoColaborador.abrir`, `:910`; exige VIVOS `:936`. Chamado por `reatar_pergunta_orfa`, `:1621`. |
| `reconciliador.py:1642`, `reatar_pergunta_orfa` | Histórico único, `len(hist)==1` em `:1623`. |
| `reconciliador.py:1655`, `reatar_pergunta_orfa` | Candidato de `_derivar_chamado_destino`, `:1556`. |
| `declaracao_texto.py:370`, `declarar_hora_colab` | `ch` recebido, apenas candidata local órfã `:363/:364`; atomic `:348`, trava `:352`, conjunto C/D/Ch/P `:299/:301`. HTTP `chamados/views.py:1090`. |
| `limbo_folgas.py:244`, `reatar_perguntas_ao_limbo` | Pai recebido; origem command `reatar_beco_folga_ao_limbo.py:110`, atomic `:99`, C+pai `:104`. Pai LIMBO mais recente: `limbo_folgas.py:220/:221`, filtrado pela fila no command `:59/:105`. |
| `limbo_folgas.py:282`, `agrupar_furo` | Pai recebido; atomic `:270`, trava C/abertas/furo/pai `:274`. Consumidores `detectar_ausencias.py:761`, cron `config/crons.py:364`, e `agrupar_furos_sob_limbo` `:331` ← `apurar_furos_diarios.py:19`, encadeado `config/crons.py:1509`. Pai do lote: último PK crescente, `limbo_folgas.py:308/:309`. |
| `fio.py:2174`, `_migrar_sob_trava` | `novo` recebido, só quando P é canônica `:2165`; universo/canônica/plano `:2151/:2155/:2158`. `migrar_resolvedoras_vivas`, atomic `:2083`, C/D/Ch/P `:2095`; chamado por `Chamado.reabrir`, `models.py:1415`, irmão eleito em `:1354`; juiz maximiza estágio e PK `juizes.py:2145/:2147`. |
| `reatar_beco_folga_ao_limbo.py:127`, `_reverter` | `destino_antes` lido do JSON `:118/:127`, podendo resultar `None`; sem atomic externo próprio nesse método. A porta abre atomic `fio.py:881`. |

Os três sítios do reconciliador chegam pela emissão `disputa_emissao.py:464` ou `reconciliar_chamados_colab` `reconciliador.py:1433/:1526`: batida via signal `ponto/signals.py:207/:215`; filho morto via signal `chamados/signals.py:350/:368`, `on_commit` `:369`. O cron `reconciliar_chamados` não deve ser apresentado como escritor atual dessa reata só pela prosa antiga: o código/documentação viva declara-o vigia, `reconciliador.py:1539/:1540`.

Pedidos de nascimento/reuso com destino:

| Sítio | Eleição |
|---|---|
| `disputa_emissao.py:439/:444` | Gerador recebe destino de `_derivar_chamado_destino`, `:409/:411`; chamada conversa `:441`, datada `:450`. Derivador filtra VIVOS `:198`, motivos `:212`, exclui candidato já com P não materializada `:226`; empate pelo turno `:237`, entrada única `:153` ou estágio mais avançado `:161`; exige resultado único `:238/:245`, senão `None` `:244`. |
| `disputa_emissao.py:837`, `_criar_pergunta_se_couber` | Destino do chamador `:843`; B oferece o próprio intervalo `:1154`; grupo oferece primeiro da ordem recebida `:1197`; complemento/ata oferecem chamado recebido `:1176`. |
| `declaracao_texto.py:376/:666` | Próprio chamado do colaborador/admin `:377/:667`. |
| `disputa_emissao.py:1371` | Próprio chamado LIMBO `:1377`. |
| `acoes_disputa.py:263` | Conversa administrativa, sem destino explícito; default `None`, `models.py:2794`. |

**Diferença relevante para F1:** pedido `None` no reuso de `_nascer` preserva destino atual, `models.py:2968`; em `reapontar_resolvedora`, pode descosturar gravando `None`, `fio.py:925`, com permissão explícita do juiz `juizes.py:2176/:2177`.

Os nomes importados de `chamados.models` que parecem escritores locais são reexports do emissor, enumerados em `models.py:4091/:4094`; devem ser resolvidos pelo censo. `reapontar_resolvedora` tem os nove sítios acima, importados diretamente de `fio` ou chamada interna no próprio módulo. `models.py:433`, `reconciliador.py:791/:1550`, `declaracao_texto.py:369`, `limbo_folgas.py:236/:268`, command `:117`.

O selo existente não prova função única: allowlist por **arquivo** em `core/tests/test_contract_escritores_estado.py:61`, lista parcial de apps `:13`, regex limitada a atribuição/update `:182/:183`, exclusão de migrations/testes `:191`.

No contrato futuro, desancorar/reversão, nascimento, reuso e reata precisam continuar distinguíveis; uma função única de atribuição pode recebê-los, sem impor silenciosamente uma eleição de intervalo sobre LIMBO ou reversão. Referências das operações: `models.py:3014/:2982`, `fio.py:925`, command de reversão `:127`. A função proposta `fio.gravar_destino_da_pergunta` **não existe hoje**: proposta [nao verificado]; há precedente explícito para separar atribuição da persistência em `fio.gravar_dia_do_fato`, `fio.py:790/:800`.

## 11/11 02:22 · sessao c5bbaf0c48c1 · - · - · origem=Ronald (tmux)

Leitura concluída nos arquivos indicados; nenhum teste foi executado e efeitos ORM concorrentes/produção continuam [nao verificado]. [nao verificado]

**Ponto crítico para a consolidação:** os pareceres c20/c21 descrevem uma árvore diferente do main. No main, a DS nasce no atomic do núcleo, mas a geração das perguntas ocorre depois de seu retorno; o delta coloca emissão, ensaio e replay dentro dele. `app/chamados/services/disputa_emissao.py:521`, `:587`, `:603`, `:662`, `:670`, `:682`.

### F1 — emissão/agregação no main

Abreviações abaixo: `DE=app/chamados/services/disputa_emissao.py`; `DT=app/chamados/services/declaracao_texto.py`. Os números são da cópia aberta.

| Partida | Juiz | Operação/persistência | Retorno | Prova aberta |
|---|---|---|---|---|
| Sem DS aberta; sem P local | **SEM JUIZ** de “precisa nascer com P”: o núcleo só procura DS aberta. `DE:531` | `_abrir_ou_agregar_disputa`, `DE:474`: cria/reusa pai `:568`, cria DS com motivos/contexto/pai `:587`; atomic `:521`, trava universo por `trava_na_ordem` `:530`. | `(disputa, True, motivos_lista)`, `DE:603`. A geração não participa desse atomic autônomo: `:670`. **Pode chegar aberta sem P viva**, porque gerador pode recusar `:315`, `:430` e wrapper retorna `:672`. | `app/chamados/tests/test_porta_disputa_emissao.py:185`; o teste da declaração chama só o núcleo, sem gerar P `:174`, `:223`. |
| DS aberta; motivo novo/repetido; P qualquer | **SEM JUIZ** de admissão via P: união dos motivos do pedido, `DE:536` | Núcleo une motivos `:541`; merge contexto e contador `:548`, `:557`, `:562`; save `:565`. | `(mesma DS, False, novos)`, `:566`. Wrapper depois gera P `:682`, pendentes `:685`, push só novos `:686`. **Continua aberta sem P viva se nada nascer**. | `app/chamados/tests/test_porta_disputa_emissao.py:199`, `:211`. |
| DS aberta/recém-criada; pedido datado; sem canônica | Catálogo `DE:314`, resolução dia `:345` etc.; para destino `_derivar_chamado_destino`, `:166`. | Gerador pede `PerguntaDisputa.abrir(disputa=..., chamado_destino=_ch_dest, data_evento=...)`, `DE:444`. Novo destino vem do derivador, `:409`, `:411`. | Lista contém P nova **ou reusada**; booleano não altera `criadas.append(p)`, `:470`. | `app/chamados/tests/test_emissao_costura_intervalo.py:50`, `:58`, `:65`. |
| DS aberta; pedido não datado sem dia | Dedup do gerador `DE:325`, `:436`; hora sem dia recusa `:425`. | Para tipo não hora abre conversa `:439`, destino `_ch_dest`, `:441`. | Lista P ou vazia, `:471`. | Prova específica dessa combinação: **sem teste** nesta leitura. | [nao verificado]
| DS A sem P do fato; canônica do fato pertence a B | A eleição de P é delegada à porta `abrir`, `DE:444`/`:837`; emissão não decide dona nem examina retorno da passagem comum. | `_criar_pergunta_se_couber` soma motivo em A **antes** das guardas `:828`, ignora `(p, criada)` de `abrir` `:837`, devolve `True` `:847`; agrupador pode analisar chamado `:1159`, `:1223`, `:1252` e salvar motivos em A `:1255`. **Não há replay explícito nem rollback do nascimento A no main; chega aberta sem P viva se `pode_fechar` não fechar ou veto HX atuar.** | `(motivos, True)`, `DE:847`, depois DS `:1291`. Encaminhamento de motivo para B nesta função: ausente no corpo `:823–847`. | Teste dessa transição estrangeira ponta a ponta: **sem teste** nesta leitura. | [nao verificado]
| DS A aberta; entrada pendente é de outro dia | `dia_do_turno_da_batida`, `DE:283`; classificador canônico `_cf`, `:288`. | `perguntar_entradas_pendentes` injeta data em `DS.contexto`, `:293`, gera P `:294`; salva contexto se lista retornada não vazia `:296`. Não tem atomic/trava próprios no corpo `:257–297`. | Lista de P, `:297`; wrapper chama depois do núcleo `:685`. | `app/chamados/tests/test_hx_entradas_pendentes.py:33`, `:43`, `:50`, `:64`. |
| Sem DS aberta; órfãos elegíveis; P não nasce por **ata ausente** | `_ata_do_dia`, `DE:887`: com escala vigente e `sem_celula`, False `:902`, `:908`, `:912`; autofecho tem veto local adicional `:1279`. | Agrupador atomic `:1052`, `_trava_do_repasse` `:1059`; cria DS `:1123`; seed `orfao_14h` `:1101`; marca abstenção `:1173`/`:1211`/`:1242`; mantém DS e acende WARNING `:1281`. **Chegada aberta sem pergunta viva.** | DS, `:1291`. | `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:51`, asserts zero P e aberta `:58–61`. |
| Sem DS aberta; órfãos elegíveis; ata disponível e dia completo | Classificador `DE:1200`/`:1237`; `DS.pode_fechar`, `:1278`. | DS nasce `:1123`, loops podem dar continue `:1202`, `:1239`; pendentes tentadas antes `:1270`; `fechar` `:1284`. | DS fechada, `:1288`; não envia push principal. | `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:63`, asserts `:77–79`; `test_hx_disputa_muda.py:24`, `:44`. |
| Sem DS aberta; órfão sem data/base ou classificador None antes de ler ata | Guarda local `DE:1233`, `:1235`, `:1238` / `:1201`; **SEM JUIZ de ata consultado nesse ramo**. | Continue antes de `_ata_do_dia` `:1240` / `:1209`; DS já existe `:1123`; se sem P e `pode_fechar=True`, fecha `:1284`. | DS fechada `:1288`. Afirmação de ata lida nesse caso é **[nao verificado]**. | `app/chamados/tests/test_hx_disputa_muda.py:24` exercita classificador None; não prova universalmente ata. Sem teste específico falta data/base nesta leitura. |
| Órfãos só já cobertos por P/complemento, ou fora de elegibilidade | `chamado_orfao_e_regularizavel`, `DE:1066`, filtro por carimbo `:1089`; `_ata_do_dia`, `:1077`. | Complementa dona da primeira P por destino, `:1070`, `:1073`; salva seus motivos diretamente `:1083`, `:1084`; sem sobreviventes não nasce principal. | `None`, `:1094`. | Teste específico dessa combinação: **sem teste** nesta leitura. | [nao verificado]
| Conjunto muda sob trava do repasse | `_trava_do_repasse`, `DE:993`: trava C primeiro `:1004`, relê `:1005`, trava C→D→Ch→P `:1010`, recusa crescimento `:1015`. | Atomic do repasse desfaz; fora dele grava evento recusa `:1293`, `:1296`. | `REPASSE_RECUSADO`, `:1299`; não confundir com None. | `app/chamados/tests/test_e1_fila_v2_2_integracao.py:269`, `:272`, `:275`; asserts sem D/P novas `:266`. |
| Âncora viva sem P da âncora | Guardas categoria/ENCERRADOS, `DE:1316`, `:1318`; dedup por motivo/destino/materializada, `:1323`. | Atomic `:1322`; trava universo `:1331`; cria DS `:1358` ou une motivos `:1366`; abre conversa destino `chamado_limbo`, `:1371`, `:1377`; analisa `:1379`. | DS `:1382`; guardas/dedup retornam None `:1317`, `:1319`, `:1328`. | `app/chamados/tests/test_fio_ancora.py:69`, `:88`, destino assert `:80`. |

**Duas eleições no main:** o derivador exige único candidato após filtro de dia e desempate por turno/estágio (`DE:198`, `:226`, `:231`, `:237`, `:238`), enquanto o agrupador por episódio pede o **primeiro na ordem herdada do chamador** (`DE:1197`); B pede cada intervalo `_chi` (`:1154`) e sem origem pede o `ch` corrente (`:1247`). A declaração pede o chamado do ato (`DT:377`, `:667`). São decisões de insumo anteriores aos escritores, e não uma eleição única do destino. `DE:166`, `DT:376`.

### Declaração: fronteira e retorno no main

- Colaborador: HTTP `app/chamados/urls.py:42` → `views.responder_chamado_colaborador` `app/chamados/views.py:1041` → `declarar_hora_colab` `:1090` → atomic `DT:348` → `_trava_do_ato` `:352` → núcleo `:360` → candidata `:363` → reata órfã com **ch do ato** `:370`, ou abre com **ch do ato** `:376`, `:377`. Retorno final `{'ok': True, 'estado':'declarada', 'pergunta_id':...}`, `DT:426`; pode retornar por carimbo/erro depois de emitir DS/P, `:418`, `:421`.
- Admin: HTTP detalhe local `app/chamados/urls.py:14` → `views.detalhe_chamado_local` `app/chamados/views.py:359`, ação apply `:478–480` → `aplicar_declaracao`, atomic `DT:654`, trava `:656`, núcleo `:659`, candidata `:662`, abrir `:666–667`; validação ocorre **depois do atomic local**, `:696`; retorno mapeia resultado de validar `:697`.
- A candidata é a canônica do universo do fato, mas só retorna se pertence à DS recém-obtida e é reusável. Não move `P.disputa`; estrangeira vira `None` e chama `abrir`. `DT:250`, `:253`, `:254`, `:376`, `:666`.
- `_trava_do_ato` trava C primeiro e universo do fato, depois DS abertas/fechadas do fato, chamado do ato/destinos e P. `DT:299–303`.

### Cadeias produtivas verificadas

- API: `app/api/urls.py:67` → `app/api/views.py:400`, nascimento da anomalia/contexto última batida `:770` → helper importado por reexport de models `:775–778`; clone HTTP core `app/api/urls_core.py:5`, `:14` → `app/api/views_core.py:387`, `:771`. Reexport comum: `app/chamados/models.py:4093`.
- PWA: `app/ponto/urls.py:11` → `app/ponto/views.py:259–260` → `app/ponto/services/triagem_batida.py:34`, `:734`.
- Signal/célula: `Chamado.analisar` `app/chamados/models.py:800` → `post_save` `app/chamados/signals.py:187–188`, `:216` → cartório chama fábrica `app/ponto/services/cartorio.py:996`, `:1008` → `perguntar_pela_celula`, `app/chamados/services/fabrica_da_celula.py:178`, `:219` → `perguntar` `:160`, `:172`, `:174`. Execução real da cascata nesta leitura: [nao verificado].
- Cron foto: `app/config/crons.py:686` → `app/ponto/management/commands/marcar_foto_ausente_retro.py:96`.
- Cron repasse: `app/config/crons.py:362`, `:363`, `:364` → `processar_alertas_turno.py:385`, `processar_alertas_avancados.py:159`, `detectar_ausencias.py:802`, todos em `app/ponto/management/commands/`.
- Manual fio/modal: `app/chamados/urls.py:21`, `:39` → ação processada em `app/chamados/services/fio.py:549–551` → `acoes_disputa.acao_iniciar_disputa_orfaos`, `app/chamados/services/acoes_disputa.py:210`, `:222`; confirmação âncora delega em `fio.py:521–524` → `acoes_disputa.py:189`.
- Cobrar dia: `app/chamados/urls.py:32` → serviço `app/chamados/services/cobrar_dia.py:16`, `:49`; passivo `app/chamados/services/pergunta_no_app.py:197`, `:212`.
- Helper grade sem perguntas: `app/ponto/management/commands/reconciliar_grade.py:412`, `:424` pede núcleo com motivos `[]`; nenhum chamador desse helper foi encontrado na busca, mas alcançabilidade completa por reflexão é [nao verificado].

### Delta c21 — somente o que muda, conferido contra main e texto do diff

As referências **c21** abaixo são endereços virtuais apresentados pelo anexo; não existem como árvore raia nesta cópia. Identidade Git da raia é [nao verificado].

| Mudança | Main aberto | Delta fornecido |
|---|---|---|
| Gerador move a preparação da pergunta para `_resolver_pergunta_por_motivo` e `_gerar_perguntas_com_causas`; conserva args, inclusive origem `None`; pedido de destino passa por novo `_destino_do_intervalo`. | `DE:300`, `:345`, `:444` | `raia_c21_delta.diff`, hunk `@@ -326...`, novo `DE c21:542–546`. |
| Nova eleição avulsa para intervalo: VIVOS, regularizáveis, marco explícito do mesmo motivo/dia, **menor PK**; fallback destino recebido. | Não existe entre `DE:254` e `:257`. | `raia_c21_delta.diff`, hunk `@@ -254...`, `_destino_do_intervalo`; c21 `:291–294`. |
| Nova eleição no agrupador: fotografia por chamado do lote; `setdefault` guarda **primeiro do lote**; `_criar_pergunta_se_couber` sobrescreve pedido somente havendo chave. | B escolhe `_chi` `DE:1154`, grupo primeiro `:1197`; sem índice. | Mesmo anexo, hunks `@@ -1057...` e `@@ -821...`; c21 `:1296–1299`, `:1044`. |
| Chamadas abrir/reapontar das duas declarações saem de DT e passam a `_pergunta_para_declaracao`; colab mantém costurar=True, admin default False. | `DT:370`, `:376`, `:666`. | Hunks `@@ -357...`, `@@ -656...`, novo helper `DE c21:819–849`. |
| Nascimento DS do núcleo deixa de confirmar vazio: geração entra no savepoint; sem motivos locais admitidos desfaz nascimento; kwargs canônicos são repetidos na dona. | DS `DE:587` e perguntas só `:670`; sem replay explícito. | Hunk `@@ -467...`, `_abrir_ou_agregar_disputa`; c21 `:679`, `:709`, `:737`, `:766`. |
| Repasse recebe savepoint do nascimento, coleta estrangeiras, adia analisar, desfaz principal nova sem nenhuma P e repete porta na dona; analisa só destinos confirmados e não recusados. | `DE:1123`, retorno `abrir` ignorado `:837`, análise imediata `:1159`, `:1223`, `:1252`. | Hunk `@@ -1098...`; c21 `:1524`, `:1534`, `:1545–1547`. |
| Agregação DS motivos/contexto passa ao helper `_agregar_motivos_na_dona`; fechada não é salva, agenda trilha após commit; nascimentos permanecem três. | saves `DE:565`, `:1084`, `:1132`, `:1161`, `:1256`, `:1367`; context pendentes `:296`. | Hunk `@@ -467...`, helper; novo `descarte_disputa.py:4`, `:37`; hunk âncora `@@ -1363...`. | [nao verificado]
| Amplia veto local de autofecho além de `_absteve_por_ata`. | `DE:1279`. | Hunk `@@ -1276...`: `_insumo_incompleto`/`ato['insumo_incompleto']`; c21 `:1562`. Não é escritor persistente, mas altera juiz da transição. |
| Remove helper grade que pedia núcleo com `[]`. | `app/ponto/management/commands/reconciliar_grade.py:412–428`. | Hunk `@@ -409...`. |
| Não altera os escritores persistentes de destino/canônica em models/fio, nem `P.disputa`, nem seus juízes de costura. | `app/chamados/models.py:2982`, `:3006`, `:3014`; `app/chamados/services/fio.py:925`, `:927`. | Esses arquivos não aparecem como alvos do diff fornecido. Aplicação efetiva em Git: [nao verificado]. |

**Replay explícito é delta, não main:** não atribuir `DE c21:766`/`:1534` a F1 main. O defeito da origem `None` vem de código comum em `app/chamados/models.py:2866`, mas o ensaio/rollback/replay que o amplifica é acrescentado no diff; sem executar ORM, o efeito concreto continua [nao verificado].

### Leitores/escritores auxiliares necessários a esta parte

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `DS.motivos` | `DE:541`, `:589`, `:1083`, `:1125`, `:1131`, `:1160`, `:1255`, `:1360`, `:1366`. | Núcleo união `:536`; seed `:1100`; complementos `:1081`; push `:1290`; pendentes `:1270`. | [nao verificado]
| `DS.contexto` | `DE:293` + save `:296`; `:562` + save `:565`; nascimentos `:590`, `:1126`, `:1361`. | Gerador `:345`, `:372`; derivador `:191`; pendentes `:273`; merge `:544`. |
| `P.chamado_destino` | Pedidos `DE:441`, `:450`, `:843`, `:1377`; `DT:370`, `:377`, `:667`; persistência pertence aos models/fio acima. | Derivação dedup `DE:227`; repasse complemento/filtro `:1070`, `:1090`; âncora dedup `:1324`; declaração órfã `DT:364`. |
| `P.disputa` | Pedidos de nascimento `DE:440`, `:445`, `:838`, `:1372`, `DT:376`, `:666`; main não reatribui a dona nesses corpos. | `_candidata_reusavel`, `DT:254`; complemento lê dona `DE:1073`. |
| `P.data_evento` | Pedido `DE:451`, `:844`, `DT:377`, `:667`; atribuidor persistente fora deste escopo. | Dedup `DE:398`, `:418`, `:1415`; complemento fallback `:1074`; eleição candidata `DT:250`. |
| `DS.fechada_em` | Fecha por delegação `DE:1284`; default/nascimento pelo model fora desta parte. | Seleção de aberta `DE:41`, `:532`, `:1097`, `:1333`. |
| `Ch.contexto_json` | Nascimento pai `DE:580`, `:1116`, `:1351`. | Origem/dia `DE:145`, `:1232`; B marcos explícitos `:1143`; filtro partial-admin `:930`. |
| `Ch.categoria`/`status_local` | Escritores pertencem às portas de Chamado, fora desta parte. | Derivador `DE:200`, `:203`; B `:1138`; âncora `:1316`, `:1318`. |
| `P.materializada_em` | Escritores fora desta parte. | Filtros de derivação/repasse/âncora `DE:228`, `:1070`, `:1090`, `:1326`. |


## 11/11 02:21 · sessao c781d947d859 · - · - · origem=Ronald (tmux)

Conferi os trechos abaixo em leitura; testes citados foram abertos, não executados. Resultado de execução: [nao verificado]. [nao verificado]

**F0 — estado real de DisputaSupervisao**

Não existe coluna `status` em `DisputaSupervisao`: o estado aberto é `fechada_em is None`, exposto por `aberta`; o motivo do fechamento é outra coluna. (`app/chamados/models.py:1757`, `:1793`, `:1823`.)

| Sítio | Valor escrito/persistência | Atomic/travas e eleição |
|---|---|---|
| `_abrir_ou_agregar_disputa` | `create` deixa `fechada_em=NULL`, `fechada_por=NULL`, `resolucao=''`, `motivo_fechamento=''`. (`app/chamados/services/disputa_emissao.py:587`; defaults em `app/chamados/models.py:1757`, `:1766`, `:1773`, `:1794`.) | Atomic próprio `:521`; `trava_na_ordem` antes da seleção da aberta; seleção `.first()` herda ordering `-aberta_em`, portanto aberta mais recente. (`disputa_emissao.py:529`, `:531`; `models.py:1804`.) |
| `criar_disputa_a_partir_de_chamados_orfaos` | Segundo nascimento, mesmos defaults de estado. (`disputa_emissao.py:1123`.) | Atomic do agrupador e `_trava_do_repasse`; seleciona aberta `.first()` herdando `-aberta_em`. (`disputa_emissao.py:1059`, `:1060`, `:1096`; `models.py:1804`.) |
| `criar_disputa_ancora_de_chamado` | Terceiro nascimento, mesmos defaults de estado. (`disputa_emissao.py:1358`.) | Atomic e trava do universo antes da seleção da aberta. (`disputa_emissao.py:1323`, `:1331`, `:1332`.) |
| `DisputaSupervisao.fechar` | `fechada_em=now`, `fechada_por=user`, `resolucao=argumento or ''`, `motivo_fechamento=motivo`. (`models.py:1932`, `:1936`.) | Atomic `:1905`; ordem C→D→Ch via `trava_na_ordem` `:1910`. Julgamento **antes** desse atomic: resposta sem veredito `:1886`; vivas `:1895`; motivo normal=`vazia` se zero P, senão `veredito` `:1903`. Exceções admitidas: `desligamento`, `admin_encerra`, `competencia_trancada`. (`models.py:1780`, `:1792`, `:1876`.) |
| `DisputaSupervisao.reabrir_admin` | `fechada_em=None`, `fechada_por=None`, `resolucao=''`, `motivo_fechamento=''`, contador admin +1; motivo novo append se pedido. (`models.py:2149`, `:2153`, `:2159`, `:2161`.) | Sem atomic/trava próprios; guarda fechada, limite 2, user. (`models.py:2138`.) Chamador HTTP direto: `acoes_disputa.acao_reabrir_disputa_admin:259`; corpo não abre atomic. (`app/chamados/services/acoes_disputa.py:235`, `:259`.) |
| `DisputaSupervisao.reabrir_sistema` | Mesmo reset dos quatro campos, sem contador. (`models.py:2173`, `:2177`.) | Sem atomic/trava próprios; aberta retorna False; fechada salva e retorna True. (`models.py:2167`, `:2181`.) Atomic/trava depende do chamador: ressurreição `models.py:1557`, `:1562`, `:1575`; rejeição de ausência `materializacao.py:675`, `:679`, `:686`; validação `validacao.py:425`, `:431`, `:218`. |

Defaults de schema, não atos de runtime: migration cria `fechada_em NULL` e `resolucao=''`; migration adiciona `motivo_fechamento=''` com os cinco valores. (`app/chamados/migrations/0013_disputasupervisao.py:24`, `:25`; `app/chamados/migrations/0049_disputa_motivo_fechamento.py:15`.)

Os leitores decisórios desses campos incluem `aberta`, seleção da aberta no emissor, recusa da fechada, receivers de fechamento e contador de vazias. (`models.py:1824`; `disputa_emissao.py:532`; `models.py:1879`; `app/chamados/signals.py:28`, `:129`, `:153`; `app/chamados/services/vigia_fio.py:734`.)

**F1 — transições confirmadas no main**

`P.status` também é derivado: validada com materialização→resolvida; validada sem materialização→validada; viva com/sem resposta→respondida/pendente; materializada→resolvida. Vitalidade é decidida por `pergunta_viva`, não pela FK da disputa fechada. (`app/chamados/models.py:3033`, `:3041`, `:3044`, `:3046`.)

| Partida | Juiz → operação | Persistido / retorno | Prova aberta |
|---|---|---|---|
| DS aberta × zero P × destino qualquer | `pode_fechar:1848`/`fechar:1897` → `fechar:1855` | DS fechada, motivo `vazia`; `True`. (`models.py:1903`, `:1932`, `:1941`.) | `app/chamados/tests/test_c7_red_minimos.py:174` |
| DS aberta × P sem vivas e sem resposta sem veredito | Mesmo juiz → mesmo escritor | DS fechada, motivo `veredito`; `True`. (`models.py:1853`, `:1903`, `:1935`.) | `test_c7_red_minimos.py:147` |
| DS aberta × P muda viva | `fechar:1896` → recusa normal | Nada escrito; `ValueError`. (`models.py:1898`.) | `test_c7_red_minimos.py:132` |
| DS aberta × resposta sem veredito | `respostas_sem_veredito:1826` → recusa de `fechar` | Nada escrito, mesmo em exceção de desligamento; `ValueError`. (`models.py:1886`.) | `test_c7_red_minimos.py:160`, `:234` |
| DS aberta × muda viva × admin com motivo | `admin_encerra:1947` → `fechar:1950` | P carimbada `admin_decidiu`, DS fechada `admin_encerra`; True. (`models.py:1918`, `:1921`, `:1935`.) | `test_c7_red_minimos.py:250`, `:261` |
| DS aberta × muda viva × desligamento | Chamador declara exceção → `fechar` | P `admin_decidiu`, DS `desligamento`. (`app/colaboradores/services/desligamento.py:142`; `models.py:1918`.) | `test_c7_red_minimos.py:209`, `:221` |
| DS aberta × P morta por tranca | `pode_fechar` em `competencia_trancada.encerrar:202` → `fechar:204` | DS `competencia_trancada`; P não carimbada. (`app/chamados/services/competencia_trancada.py:198`, `:204`.) | `test_c7_red_minimos.py:329` |
| DS já fechada × mesma entrada | Guardas `fechar:1879` | Sem escrita, False. (`models.py:1882`.) | `test_c7_red_minimos.py:428` |
| DS já fechada × outra entrada | Guardas `fechar:1880` | Sem escrita, ValueError. (`models.py:1883`.) | `test_c7_red_minimos.py:439` |
| DS fechada × qualquer P | Guardas admin `models.py:2138` → `reabrir_admin:2149` | **ABERTA SEM PERGUNTA VIVA possível na porta isolada**; reset quatro campos, contador +1, None implícito. Não exige P viva. (`models.py:2138`, `:2161`.) | `app/chamados/tests/test_disputa_transicoes.py:131`; integração casca `test_reabrir_disputa_admin_sem_aberta.py:22` |
| DS fechada × qualquer P | `SEM JUIZ` de vitalidade em `reabrir_sistema:2163` → mesmo | **ABERTA SEM PERGUNTA VIVA possível na porta isolada**; reset campos e True; chamadores normalmente a invocam por P religada. (`models.py:2167`, `:2173`.) | Teste isolado exato: sem teste localizado. | [nao verificado]
| DS aberta × P respondida corrigível | `perguntas_corrigiveis`, limite em `models.py:2223` → `reabrir_para_correcao:1562` | Zera resposta, respondida/validada/autor; contador colab+1; `{ok:True,estado:'reaberta',usadas}`. (`disputa_emissao.py:1593`, `:1632`; `models.py:2237`, `:2239`, `:2248`.) | `test_porta_disputa_ciclo.py:283`, repetição `:297`, limite `:311`, fechada `:322` |
| DS qualquer × P respondida não validada × admin recusa com motivo | Guardas `validacao.recusar_pergunta:561` → `reabrir_para_correcao:593` | Recusa escrita; resposta/assinaturas zeradas; `{ok:True,estado:'recusada'}`. DS não reabre por esse método. (`validacao.py:584`, `:588`, `:621`.) | `test_porta_validacao_pergunta.py:224`, `:258`, `:274` |
| DS qualquer × P respondida × nova resposta | Validação tipo `disputa_emissao.py:1433` → `responder_pergunta_disputa_core:1467` | Nova resposta e timestamp; desfaz validação prévia; se via sem lastro, limpa carimbo; `(True,None)`. DS não é diretamente reaberta. (`disputa_emissao.py:1480`, `:1485`, `:1508`, `:1559`; `models.py:3313`.) | Prova específica resposta→DS fechada mantida: sem teste localizado. | [nao verificado]
| DS qualquer × P respondida válida | `motivo_implausivel_validacao:404` → `_carimbar_validacao:435` e materialização | Carimba validação/autor; sucesso `validada` ou `ja_no_espelho`; falha pode devolver validação. (`validacao.py:134`, `:137`, `:461`, `:515`, `:517`.) | Cobertura específica de cada ramo: [nao verificado]. |
| DS fechada × P HORA sem materialização e recusa escrita | `_resposta_de_hora:190` → `devolver_validacao_recusada:199` | Validação/autor NULL, resposta preservada; DS reabre se `reabrir=True`; True/None/False conforme mudança/caso. (`validacao.py:199`, `:218`, `:221`, `:231`.) | Sem teste aberto específico. | [nao verificado]
| DS fechada × P carimbada, sem texto, marco não provado aceso e candidata C6 | `_universo_da_ressuscitacao` → `ressuscitar_perguntas:1543` | Descarimba trio + validação/autor/respondida; reabre DS; retorna n. FK `disputa` e destino preservados. (`models.py:1566`, `:1575`.) | `test_porta_pergunta_identidade.py:301`, `:319`, exclui com texto `:331` |
| DS aberta/fechada × P via fantasma | Chamador decide lampada/C6/competência → `reabrir_fantasma:3289` | Trio NULL/NULL/''; True/False. (`models.py:3296`, `:3298`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:165` reabre DS.) | `test_porta_pergunta_identidade.py:242`, `:257` |
| DS qualquer × P via competência antiga | `SEM JUIZ` local, via específica → `reabrir_por_competencia:3317` | Limpa trio; mantém resposta; True/False. (`models.py:3320`, `:3322`.) | `test_porta_pergunta_identidade.py:223`, `:266` |
| DS aberta × zero P × sem ata no agrupador | `_ata_do_dia` + `_absteve_por_ata` → branch `disputa_emissao.py:1279` | **ABERTA SEM PERGUNTA VIVA**, WARNING; retorna DS. (`disputa_emissao.py:1279`, `:1281`, `:1299`.) | `test_hx_disputa_muda_sem_ata.py:51`, asserts `:58`, `:59` |
| DS aberta × zero P × ata completa | `pode_fechar:1278` → `fechar:1284` | Fecha vazia, retorna DS. (`disputa_emissao.py:1284`, `:1290`.) | `test_hx_disputa_muda_sem_ata.py:63`, asserts `:77`, `:78` |

**Reuso/ressurreição de identidade e origem None**

O main não possui o protocolo novo de ensaio→rollback→replay do emissor c21; as funções atuais são `_abrir_ou_agregar_disputa:474`, `disputa_para_declaracao:606`, `gerar_perguntas_disputa:300`. O protocolo apresentado está no delta recebido, não nesses corpos do main. (`app/chamados/services/disputa_emissao.py:300`, `:474`, `:606`.)

No main, repetição de `abrir` recalcula `_bo = campos.get('batida_origem_id') or derivador(disputa,motivo)`; portanto `None` explícito e ausência da chave têm o mesmo efeito. Com dona diferente, o contexto consultado pode ser diferente; a própria docstring declara que contexto aponta outra anomalia depois de merge. (`models.py:2866`, `:3456`, `:3463`.)

O lookup elege P canônica por `universo_do_fato`/`canonica_do_grupo`; reuso conserva `P.disputa`, pode alterar `P.chamado_destino`, devolve `(ex,False,dia)`. (`models.py:2907`, `:2921`, `:2982`, `:3008`.) Não há transferência de dona nesse reuso: a FK escrita na inserção é a recebida e a devolução estrangeira não a sobrescreve. (`models.py:3014`, `:3008`.)

Na inserção o save ainda deriva batida se `None`; a divergência entre chave procurada e chave efetivamente inserida com insumo None deve ser tratada separadamente do replay futuro. (`models.py:3448`, `:3449`, `:2866`.) Contraexemplo ORM exato com replay estrangeiro: [nao verificado].

**Travas e cadeias úteis**

`trava_na_ordem` realiza de verdade `select_for_update` C `models.py:2666`, D `:2675`, Ch `:2688`, P `:2692`; só trava P explicitamente quando há `perguntas=`. (`models.py:2691`.)

A reabertura admin vem de HTTP `processar_post_fio:581`→`acoes_disputa.acao_reabrir_disputa_admin:259`→model, seguida de `abrir_conversa:263`; a conversa tem destino default None e pode falhar depois de a DS já ter sido reaberta porque a casca não envolve o conjunto em atomic. (`app/chamados/services/fio.py:581`; `app/chamados/services/acoes_disputa.py:259`, `:263`; atomic externo HTTP: [nao verificado].)

Reabertura colab tem duas cascas: HTML `views.py:1334`, REST `api/views.py:2691`; ambas filtram DS aberta antes e delegam ao método. (`app/chamados/views.py:1328`; `app/api/views.py:2682`.)

Validação de P viva em DS fechada possui porta deliberada com `sem_reabrir=True`: `processar_post_fio` verifica vitalidade e chama a casca passando esse valor. Assim a devolução recusada pode manter DS fechada sem mudar a pergunta de dona. (`app/chamados/services/fio.py:600`, `:605`, `:609`; `app/chamados/services/validacao.py:215`.)

O selo existente de DS é mais fraco que o censo solicitado: considera atributo e kwargs diretos em update/create/get_or_create/update_or_create/bulk_create; exclui migrations e não cobre `**kwargs`, `bulk_update`, SQL, setattr ou listas dinâmicas. (`app/chamados/tests/test_c7_red_minimos.py:579`, `:581`, `:583`, `:597`.)

## 11/11 02:13 · sessao abd8f65a40e6 · FIO-E0-V4-E4B · sonda · origem=Code

1. **FOTO_SOMBRA:** `2026-11-11 02:03:21.722000-03:00`, com `hoje=2026-11-11`: é posterior ao pouso informado da E4a-2, em 10/11 às 14:5x local. [nao verificado] [nao verificado]

**Porém, a sonda apresentou erro; não concluo os números da remedida**, conforme sua regra. A saída fornecida registra `SemLavra: ENSAIO SEM A LAVRA DE PROD`, com marcador `esmeril_lavra_de_prod` **AUSENTE** e lavra carregada `78b94bfb197d278e3f538ab46c3a747a`. [nao verificado]

O código confirma que essa guarda levanta `SemLavra` quando o marcador não corresponde à lavra carregada. `ListaDoAdmin` captura a exceção e continua sem resposta da lista; portanto, `rc_sombra=0` não basta para validar a medição. [esmeril_espelho.py:266](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/ponto/services/esmeril_espelho.py:266), [vigia_fio.py:219](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:219)

2–5. **Contadores, decomposições, conferência da soma, comparação com o DRY e conclusão sobre “hoje pra frente”: não concluídos devido ao erro.** A condição de remedida válida pós E4a-2 permanece [nao verificado].

As chaves que contam “hoje pra frente” são `CENSO_ALCANCE['alcance_de_hoje_pra_frente']` e `CENSO_VIVO['fio_morto_de_hoje_pra_frente']`. [vigia_fio.py:648](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:648), [vigia_fio.py:420](/home/ronald/codex_ws/52a0022e962812517db4e305d09ac6668ebbf4fc/app/chamados/services/vigia_fio.py:420)

## 11/11 02:11 · sessao bcaec9e8255c · FIO-E0-V4-E4B · leitura · origem=Code

**lei:** identidade histórica não basta para provar porta alcançável; a lei exige pergunta viva ou ação nomeada do admin quando a pendência permanece. As ações exigíveis devem vir de C19, por instância e após decisões/efeitos. (`app/docs/CORTES.md:253`; `app/docs/CORTES.md:263`; `app/docs/CORTES.md:426`; `app/docs/JUIZES.md:78–85`)

**O retorno atual não permite construir essa certidão.** `perguntar_pela_celula` devolve motivos; `perguntar` devolve quantidade de carimbos desfeitos; o gerador mistura perguntas inseridas e reusadas. O backfill descarta esses retornos e decide `segue_sem_porta` pelo recenso. (`app/chamados/services/fabrica_da_celula.py:175`; `app/chamados/services/fabrica_da_celula.py:220`; `app/chamados/services/disputa_emissao.py:470–471`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:137–143`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:343–348`)

Os 82 dias e sua distribuição são o dado fornecido. **A associação de qualquer desses dias aos ramos abaixo é [nao verificado].** Os testes foram inspecionados; execução e resultado atual da suíte são **[nao verificado]**. [nao verificado]

Para tornar a matriz legível, as referências abreviadas abaixo significam estes arquivos:

| Sigla | Arquivo |
|---|---|
| F | `app/chamados/services/fabrica_da_celula.py` |
| E | `app/chamados/services/disputa_emissao.py` |
| PD | `app/chamados/models.py` |
| R | `app/chamados/reconciliador.py` |
| M | `app/chamados/services/materializacao.py` |
| RV | `app/chamados/services/relavra_via.py` |
| T | `app/chamados/services/tarefa_do_dia.py` |
| B | `app/chamados/management/commands/backfill_fio_morto_e4b.py` |
| DT | `app/chamados/services/declaracao_texto.py` |
| C | `app/docs/CORTES.md` |
| J | `app/docs/JUIZES.md` |
| L | `app/docs/LEIS.md` |

Na coluna PROVA, nomes de testes são relativos a `app/chamados/tests/`. **“Sem teste” significa sem teste específico localizado nesta leitura; a inexistência absoluta é [nao verificado].** [nao verificado]

As portas legais usadas nas células são:

| Código | Porta devida |
|---|---|
| **P** | Pergunta viva e alcançável ao colaborador, quando C19 exige responder/perguntar. A identidade histórica sozinha não basta. (`C:253`; `C:426`; `J:78–85`) |
| **A** | Ação nomeada do admin, incluindo validação, decisão ou correção de cadastro. (`C:253`; `J:78–82`; `L:122`) |
| **N** | Nenhuma pergunta/decisão exigível para aquele marco naquele instante: dispensa, marco ainda não vencido ou efeito/decisão encerrando a obrigação. (`J:64–84`; `L:100`; `T:180–210`; `T:241–260`; `T:140–154`) |
| **U** | Porta devida indeterminada: deve permanecer `nao_sei`, sem transformar ausência de informação em dispensa. (`J:83–84`; `C:426`) |

**Matriz da seleção e das saídas da fábrica.** Cada retorno abaixo é o retorno da função indicada; a coluna de distinção informa também o que chega a `perguntar_pela_celula`. (`F:35–220`)

| Ramo — condição | Entrada que o alcança | Retorno exato | Estado gravado / nascimento | PORTA DEVIDA | Retorno distingue? | PROVA |
|---|---|---|---|---|---|---|
| V1 — `F:47–48` | Sem marcos vencidos **nem pendentes**; dia passado | `vencidos_no_instante → None` | Leitura; nada nasce | **A: CADASTRO/DP**, sem pergunta ao colab. (`C:428`; `J:91`) | Distingue falta de turno de conjunto vazio; a saída externa será `[]` | `test_e4_c20.py:20` |
| V2 — `F:47–48` | Sem marcos; hoje ou futuro | `set()` | Nada nasce | **N**, nenhum marco exigível. (`L:100`; `F:41–43`) | Não distingue ausência de turno de nenhum vencimento | `test_e4_c20.py:20` cobre o passado; hoje/futuro: sem teste específico | [nao verificado]
| V3 — `F:49` | Há vencidos ou pendentes | `set(venc)` | Nada nasce | **P/A/N**, segundo marco, decisão e efeito. (`J:78–84`) | Expõe os vencidos; não expõe causa dos excluídos | `test_fabrica_por_evento.py:45`; `test_pergunta_do_dia_em_curso.py:504` |
| F01 — `F:183–184` | `cel.veredito == 'nunca_bateu'` | `[]` | Não cria pergunta nem pauta de falta de turno | **A: lista C5**, zero pergunta. (`C:433`) | **Não** | `test_e4a2_nunca_bateu.py:185–192`; `:289` |
| F02 — `F:201–205`, escritor `F:367–373` | Passado, ativo, não dispensado; V1; nenhuma pauta reconhecida | `pauta_cadastro_sem_turno → True`; externa `[]` | Cria uma cabeça de pauta DP por colaborador | **A: DP/CADASTRO**. (`C:428`; `J:91`) | Booleano distingue criação; fábrica o descarta | `test_e4_c20.py:64`; `test_e4a_pauta_cadastro_contador.py:51` |
| F03 — `F:327–328`, `F:367–368` | Mesmo caso; pauta de cadastro viva | Juiz `True`; escritor `False`; externa `[]` | Não repete pauta | **A já entregue ao DP**; leitura efetiva pelo DP não é provada. (`C:428`; `F:321–323`) | Externa **não** distingue de F02 | `test_e4a_pauta_cadastro_contador.py:51` |
| F04 — `F:331–335`, `F:367–368` | Pauta feita/cancelada em data local ≥ dia consultado | Juiz `True`; escritor `False`; externa `[]` | Não reabre pauta do dia velho | Encaminhamento anterior satisfaz a regra de entrega. (`C:428`) | **Não** na externa | `test_e4a_pauta_cadastro_contador.py:80`; `:90` |
| F05 — `F:334–336`, `F:369–373` | Só há pauta encerrada antes do novo dia sem turno | Juiz `False`; escritor `True`; externa `[]` | Cria pauta nova para a persistência posterior do problema | **A: DP/CADASTRO**. (`C:428`) | **Não** na externa | `test_e4a_pauta_cadastro_contador.py:58`; `:67`; `:88` |
| F06 — `F:105–106` | Chamada de `motivos_a_perguntar` sobre `nunca_bateu` | `[]` | Leitura; nada nasce | **A: C5**, sem pergunta. (`C:433`) | **Não** | `test_e4a2_nunca_bateu.py:185–192` |
| F07 — `F:112–113` | Juiz `dia_sem_trabalho(cel)` verdadeiro | `[]` | Não emite | **N** para cobrança do marco; parcial com resto tem exceção própria. (`J:64–72`) | **Não** | `test_f1c4_3_emissores.py:83`; `:89` |
| F08 — `F:112–113` | Situação atual do colab diferente de `ativo` | `[]` | Não emite | Não estabelece dispensa de toda decisão histórica; C19 pesa vínculo/fato e pode exigir ação admin. (`T:168–182`; `J:78–84`) Lei específica que dispense tudo só pela situação atual: **[nao verificado]** | **Não** | Sem teste específico desta guarda da fábrica | [nao verificado]
| F09 — `F:114–115` | Dia futuro | `[]` | Não emite | **N** por teto temporal. (`L:100`; `F:8–11`) | **Não** | `test_pergunta_do_dia_em_curso.py:95` |
| F10 — `F:120–121` | Veredito preenchido fora de `VEREDITOS_ACUSAM` | `[]` | Não emite | **N** para nova pergunta; guarda inclusive admin deve ficar. (`C:438`) | **Não** | `test_fabrica_por_evento.py:52`; `test_pergunta_do_dia_em_curso.py:479` |
| F11 — `F:123–125`; cartório `:398–399` | Parcial com `periodo_parcial='sem_periodo'` | `[]` | Não cria pergunta/pai/push | **A: CADASTRO**, lista do admin. (`C:319`) | **Não** | `test_cobparc5_parcial_sem_periodo_so_lista.py:157` | [nao verificado]
| F12 — `F:123–125`; cartório `:400` | Parcial, julgamento anterior e nenhum fato novo | `[]` | Não cria pergunta/pai/push | **A: lista admin** pela mudança de regra sem fato novo. (`C:323`) | **Não** | `test_cobparc5_parcial_sem_periodo_so_lista.py:160`; `:222` | [nao verificado]
| F13 — `F:58–61` | `motivos_pela_ata` recebe `None` | `[]` | Não consulta ata nem emite | Falta de turno passado vai ao DP; não autoriza pergunta por data. (`C:428`; `F:55–60`) | **Não** na externa | `test_e4_c20.py:20` |
| F14 — `F:58–61` | Recebe conjunto vazio: nenhum marco venceu | `[]` | Não consulta ata nem emite | **N agora**, teto por instante. (`L:100`) | Não distingue de F13 | `test_fabrica_por_evento.py:45` |
| F15 — `E:897–901` → `F:65–66` | Exceção lendo marcos/ata | `_ata_do_dia → (True,None)`; motivos `[]` | Nenhuma pergunta; registra falha engolida | **U**, não prova dispensa. (`J:83–84`) | **Não**; vira o mesmo `[]` | Sem teste | [nao verificado]
| F16 — `E:902–910` → `F:65–66` | Leitor diz `sem_celula`; sem escala vigente | `(True,None)`; motivos `[]` | Nenhuma pergunta | **U**; se o teto já mostrou falta de turno passado, F02–F05 interceptam antes. (`J:83–84`; `F:201–205`) | **Não** | Sem teste específico da fábrica | [nao verificado]
| F17 — `E:902–912` → `F:65–66` | `sem_celula`, escala vigente | `(False,None)`; motivos `[]` | Não cria pergunta; acende aviso de ata | **U**, aguarda ata/lâmpadas. (`J:83–84`; `E:887–890`) | **Não** | `test_fabrica_pela_ata.py:62` usa stub; não prova o corpo de `_ata_do_dia` |
| F18 — `DT:104–105` | Leitor declara isento | `(fora_vinculo,[])`; `_ata_do_dia → (True,[])`; motivos `[]` | Nada nasce | **N**. (`T:183–189`; `J:78–84`) | **Não** | Sem teste específico desse percurso | [nao verificado]
| F19 — `DT:112–115` | Dia fora do vínculo, sem contribuição elegível da véspera | Faltantes `[]`; motivos `[]` | Nada nasce | **N** pelo vínculo/fato lido. (`T:168–182`; `J:78–84`) | **Não** | Sem teste específico desse percurso | [nao verificado]
| F20 — `DT:119–120` | Dia conhecido sem trabalho e sem parcial cobrando resto | Faltantes `[]`; motivos `[]` | Nada nasce | **N**. (`J:64–72`) | **Não** | `test_f1c4_3_emissores.py:89` caracteriza dispensa |
| F21 — `DT:121–127` | Ata agregada ou sem pares válidos E/S com hora | Estado `sem_celula`; cai em F16/F17 | Nenhuma pergunta | **U**, ata sem lâmpadas não equivale a dia quitado. (`J:83–84`) | **Não** | `test_fabrica_pela_ata.py:62` é prova do bloqueio externo |
| F22 — `DT:131–132` | Nenhuma célula do leitor tem `missing` verdadeiro | Faltantes `[]`; motivos `[]` | Nada nasce | **N** se lâmpadas provam cumprimento; **U** se indeterminadas. (`J:78–84`; `DT:91–94`) | Não separa cumprido de desconhecido | `test_fabrica_pela_ata.py:49` cobre sem faltantes |
| F23 — `F:69–70` | Faltante existe, mas seu tipo/hora não está nos vencidos | Não acrescenta motivo; pode resultar `[]` | Nada nasce para esse marco | **N agora** se ainda não venceu; **U** se incompatibilidade de insumos. (`L:100`; `J:83–84`) | **Não** | `test_fabrica_por_evento.py:45` cobre não vencido |
| F24 — `F:71` | Faltante da véspera, `f[0] != dia` | Não acrescenta motivo; pode resultar `[]` | Não emite por esta célula | Porta do **dia do turno correspondente**, não determinada por esta saída. (`T:212–215`; `J:78–84`) | **Não** | `test_fabrica_pela_ata.py:53` |
| F25 — `F:71–73` | Posição/tipo sem mapeamento | Não acrescenta motivo; pode resultar `[]` | Nada nasce para o marco | **U** se há marco devido sem identidade; C19 não deve inventá-la. (`T:269–275`; `J:83–84`) | **Não** | Sem teste específico | [nao verificado]
| F26 — `F:72–73` | Dois faltantes mapeiam para o mesmo motivo | Acrescenta o motivo uma vez | Ainda só leitura | Exigibilidade é por **instância**, inclusive segunda pausa. (`C:426`; `T:269–275`) | Lista perde multiplicidade de instâncias | `test_fabrica_pela_ata.py:53` cobre mapa; segunda instância: sem teste específico da emissão | [nao verificado]
| F27 — `F:145–157` | Todos os motivos têm linha no domínio do fato | `motivos_a_perguntar → []`; externa `[]` | Não chama `perguntar`; não exige C6 viva, fila ou disputa aberta para dedup | **P/A/N segundo decisão/efeito**, nunca decidido só pela existência histórica. (`C:253`; `C:426`; `PD:2753–2755`) | **Não** | `test_fabrica_por_evento.py:34` cobre repetição normal; dedup morto sem porta: sem teste | [nao verificado]
| F28 — `F:155–156`, `F:208–209` | Linha deduplicada tem carimbo elegível | Externa continua `[]` se nenhum motivo novo | Pode desfazer carimbo; detalhado abaixo | **P/A** se a obrigação voltou; preservar fechamento quando exigido. (`C:253`; `F:195–198`) | **Não**, quantidade é descartada | `test_o261_julgamento_desfaz_carimbo.py:92` |
| F29 — `F:214–216`, `F:277–285` | Limbo `trabalha=None`, motivos presentes, competência entregue, motivos ainda não pautados | Escritor retorna lista `novos`; externa `[]` | Cria pauta DP ancorada no dia; zero pergunta ao colab | **A: DP**, encaminhamento específico do limbo entregue. (`F:210–213`; `L:122`) Corte correspondente em LEIS/CORTES/JUIZES: **[nao verificado]** | **Não** na externa | `test_f1c4_3_emissores.py:199` |
| F30 — `F:262–265`, `F:278–285` | Mesmo caso; todos os motivos em pauta viva/feita/cancelada | Escritor `[]`; externa `[]` | Não repete pauta | Encaminhamento DP anterior; não reabre fechamento humano. (`F:259–261`; `L:122`) | Não distingue de F29 | `test_f1c4_3_emissores.py:218`; `:227`; `:243` |
| F31 — `F:217–220` | Há motivos, limbo não encaminhado ao DP | `ms`, lista de motivos solicitados | Chama escritor; pode criar, reusar, recusar costura ou não gerar pergunta nos ramos internos | **P/A** conforme C19; a lista não prova cumprimento. (`C:426`; `J:78–84`) | Distingue somente motivos pedidos | `test_fabrica_por_evento.py:34`; `test_f1c4_3_emissores.py:207`; `:212` |
| F32 — `F:228–229` | `limbo_vai_a_pauta`: sem motivos | `False` | Leitura | Não determina porta. (`J:78–84`) | Não distingue dos demais `False` | `test_f1c4_3_emissores.py:218` cobre segunda passada |
| F33 — `F:228–229` | `trabalha is not None` | `False` | Leitura | Trabalho entregue segue podendo perguntar. (`C:438`; `F:210–213`) | **Não** | `test_f1c4_3_emissores.py:212` |
| F34 — `F:230–231` | Limbo com motivos; entrega falsa ou indeterminada | `bool(entrega)`, portanto `False` nos dois | Segue para emissão | Falsa e desconhecida não são a mesma autorização no backfill. (`B:286–287`; `B:318–322`) | **Não** | Aberta: `test_f1c4_3_emissores.py:207`; entrega indeterminada: sem teste | [nao verificado]
| F35 — `F:242–245` | Vigia; limbo entregue | `motivos_sem_pauta_do_limbo(cel,devidos)` | Só lê; não cria pauta | **A: DP** para motivos ausentes. (`F:235–241`; `L:122`) | Lista não distingue dispensa de pauta já existente | `test_f1c4_3_emissores.py:218` |
| F36 — `F:242–245` | Vigia; fora do limbo entregue | `devidos` | Só lê | Exigibilidade pelos mesmos guardas da fábrica. (`F:238–241`; `C:426`) | Mesmas perdas de `motivos_a_perguntar` | `test_fabrica_por_evento.py:76` |
| F37 — `F:299–302` | Eventos sem célula atual para as datas | `0` | Nada emite | **U** quanto ao dia sem célula. (`J:83–84`) | Não distingue de células que devolveram listas vazias | Sem teste específico | [nao verificado]
| F38 — `F:299–302` | Células atuais encontradas | `sum(len(perguntar_pela_celula(...)))` | Efeitos dos ramos anteriores; soma **motivos**, não inserções | Não certifica P/A/N. (`F:179`; `F:220`) | **Não** | `test_e4_c20.py:42` |

Os auxiliares de cadastro também têm saídas próprias, consumidas pelo juiz/contador; não são prova de que uma pergunta nasceu. (`F:308–351`)

| Ramo — condição | Entrada | Retorno exato | Estado gravado | PORTA DEVIDA | Distingue? | PROVA |
|---|---|---|---|---|---|---|
| F39 — `F:312` | `_pautas_cadastro(vivas=True)` | QuerySet de cabeças DP com marcador, usando `Pauta.objects.vivas()` | Nenhum | Seleção para **A/DP**. (`C:428`) | Distingue pelo argumento, não por resultado certificado | `test_juiz_pauta_cadastro_sem_turno.py:241` |
| F40 — `F:312` | `vivas=False` | QuerySet do histórico de cabeças com marcador | Nenhum | Permite reconhecer entrega anterior. (`C:428`) | Sim, consulta histórico | `test_e4a_pauta_cadastro_contador.py:80` |
| F41 — `F:329–330` | Sem pauta viva, consulta com `dia=None` | `False` | Nenhum | Não prova ausência de encaminhamento histórico para um dia. (`F:321–325`) | Distingue do `True`; não informa motivo | `test_e4a_pauta_cadastro_contador.py:58` |
| F42 — `F:346–351` | Contador com ou sem filtro de empresas | `{'nome':'cadastro_sem_turno','numero':<colabs distintos com pauta viva>,'esperado':0,'dono':'CADASTRO'}` | Nenhum nesta função | Lâmpada de **A/CADASTRO**, não juiz de pendência. (`F:340–344`; `L:122`) | Número agregado, sem ramo/dia | `test_e4a_pauta_cadastro_contador.py:74` |

**Matriz do descarimbo chamado pela fábrica.** O retorno de `perguntar` é exclusivamente a soma dos resultados desse descarimbo; emissão não incrementa esse inteiro. (`F:167–175`)

| Ramo — condição | Entrada | Retorno exato | Estado gravado | PORTA DEVIDA | Distingue? | PROVA |
|---|---|---|---|---|---|---|
| RV1 — `RV:238–241` | Pergunta não encontrada/elegível após releitura travada | Não incrementa `n`; função retorna `n` | Nada nessa pergunta | Decisão/identidade preservada; exigibilidade por C19. (`J:78–84`; `RV:177–178`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:117`; `:152` |
| RV2 — `RV:238–241` | Julgamento passou `reabrir_fechada=False`; disputa fechou | Não incrementa `n` | Não reabre disputa nem limpa carimbo | A viva em disputa fechada mantém tratamento do admin, sem reabrir por esta porta. (`F:195–198`; `C:253`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:139`; `:165` | [nao verificado]
| RV3 — `RV:242–243` | C6 diz morta: carimbo tem lastro | Não incrementa `n` | Preserva carimbo | **N** quanto a ressuscitar essa pergunta. (`RV:65–68`; `J:78–84`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:106` |
| RV4 — `RV:246–247` | `descarimbar()` recusa | Não incrementa `n` | Preserva carimbo | Não ressuscita identidade fora do fato. (`RV:163–168`; `PD:2753–2755`) | **Não** | `test_o261_julgamento_desfaz_carimbo.py:117` caracteriza identidade |
| RV5 — `RV:244–262` | Elegível, C6 viva, porta aceita | Incrementa `n`; retorno `int n` | Limpa materialização/via; preserva resposta/validação; grava evento | **P/A** segundo resposta/decisão vigente. (`C:253`; `J:78–84`; `RV:206–207`) | Conta mudanças, não identifica perguntas/portas | `test_o261_julgamento_desfaz_carimbo.py:92` |
| RV6 — `RV:260–261` | Mesmo sucesso, disputa fechada e chamada permite reabri-la | Mesmo incremento | Também chama `reabrir_sistema` | **P/A**; continente acompanha a pergunta ressuscitada. (`RV:69–71`; `C:253`) | Não separa de RV5 | Sem teste específico desta entrada pela fábrica | [nao verificado]

**Matriz da emissão interna.** A fábrica chama o gerador dentro de `criar_ou_atualizar_disputa` e depois novamente em `perguntar`. Uma lista `[p]` pode conter pergunta reusada. (`F:172–174`; `E:669–688`; `E:444–471`)

| Ramo — condição | Entrada | Retorno exato | Estado gravado / nascimento | PORTA DEVIDA | Distingue? | PROVA |
|---|---|---|---|---|---|---|
| E01 — `E:535–566` | Disputa aberta; motivo novo | Núcleo `(d,False,sorted(novos))`; público `d` | Agrega motivos/contexto; tenta gerar perguntas | **P/A** conforme dia/decisão. (`C:426`; `J:78–84`) | Público não distingue nascimento/agregação | `test_regressao_etapa7.py:254` caracteriza núcleo com outro motivo |
| E02 — `E:535–566` | Disputa aberta; motivo já presente | Núcleo `(d,False,[])`; público `d` | Atualiza contexto, inclusive contador/data; tenta emissão outra vez | **P/A/N** por fato do dia, não por motivo já agregado. (`E:674–681`; `C:426`) | **Não** no público | `test_regressao_etapa7.py:236` |
| E03 — `E:568–603` | Sem disputa aberta; pai novo | `(d,True,list(motivos))`; público `d` | Nasce chamado-pai e disputa; tenta perguntas | **P/A** quando devida. (`C:253`; `C:426`) | **Não** no público | `test_regressao_etapa7.py:221` |
| E04 — `E:584–592` | Sem disputa aberta; chamado-pai vivo reusável | Mesma tripla de E03 | Reusa pai, anexa ocorrência, cria disputa | **P/A** quando devida. (`C:253`; `C:426`) | Não distingue pai novo de reusado | Sem teste | [nao verificado]
| E05 — `E:595–603` | Falha de auditoria após nascimento da disputa | Mesma tripla; exceção engolida | Disputa/pai permanecem | Porta humana continua exigível conforme dia. (`C:253`; `J:78–84`) | **Não** | Sem teste | [nao verificado]
| E06 — `E:398–401` | Intervalo com pergunta mesma disputa/motivo/dia, fora vias excluídas | Gerador singleton `[]` | Não insere nem reata | **P/A/N** conforme a pergunta existente; dedup não prova vida. (`C:253`; `C:426`) | **Não** | `test_emissao_costura_intervalo.py:65` |
| E07 — `E:417–420` | Entrada/saída com pergunta na mesma disputa e `_data_ev` derivada | Gerador singleton `[]` | Não insere nem reata | **P/A/N** conforme decisão/efeito. (`J:78–84`) | **Não** | `test_regressao_etapa7.py:236` cobre idempotência; ramo específico: sem teste | [nao verificado]
| E08 — `E:238–244` | Derivador termina com zero candidatos | `(None,None)` | Não esc

[truncado em 20000 de 66776 caracteres]

## 11/11 02:05 · sessao 168b267eaf81 · - · - · origem=Ronald (tmux)

Revisão somente leitura: dois pontos concretos merecem correção.

1. **Residual A/S alcança P com `trabalha=False`.** `dia_sem_trabalho` retorna verdadeiro nesse caso, mesmo com cobertura parcial; o residual aceita `indefinida` e gera `validar/admin`. Confirmei essa saída em memória. O teste P novo fixa `dia_sem_trabalho=False` e não cobre essa variação. A exclusão já possui autoridade em `cartorio.py::parcial_so_lista`. (`chamados/juizes.py:445`, `chamados/services/tarefa_do_dia.py:319`, `chamados/tests/test_e3_c8b_sem_banco.py:129`, `ponto/services/cartorio.py:398`)

2. **Guarda exportada pode suprimir tarefas anteriores à cura.** Qualquer pergunta em marco aceso entra em `pares_admin`, inclusive morta pelo C6. Se K6 disser exportada, o retorno antecipado elimina todas as instâncias, inclusive outro marco apagado que antes exigia `perguntar/sistema`. Isso excede “guardar somente as exposições novas”. (`chamados/services/tarefa_do_dia.py:457`, `chamados/services/tarefa_do_dia.py:464`, `chamados/services/tarefa_do_dia.py:203`)

K6 mantém seu predicado: a carga usa `anotar_vigencia` e a mesma combinação `so_vigentes`/`invalidada_em`. A nova entrada é opcional e preserva as chamadas anteriores. (`folha/export.py:556`, `folha/export.py:583`, `chamados/services/tarefa_do_dia.py:429`)

Os dois alertas anteriores sobre `instancia_marco` precisam desta ressalva:

- Nascimento regular recusa instância diferente da derivada; portanto a forma não-marco com instância explícita divergente depende de acervo ou escrita que contorne a porta [nao verificado]. (`chamados/models.py:3437`)
- Duas perguntas da mesma identidade são impedidas para marcos desde 21/09; antes desse horizonte, escolher apenas `ps[-1]` pode ocultar uma viva anterior. Ocorrência nos dados anexados: [nao verificado]. (`chamados/models.py:2403`, `chamados/models.py:3482`, `chamados/services/tarefa_do_dia.py:263`)

Nenhuma quebra adicional observada nos campos do dict: as chaves permanecem existentes e o agregador continua lendo `acoes`, `de`, `porta`, `celula_id`, `nao_sei`. Resultado dos testes completos dos leitores: [nao verificado]. (`chamados/services/tarefa_do_dia.py:425`)

## 11/11 02:04 · sessao 035cb0e9da4c · - · - · origem=Ronald (tmux)

Dois pontos concretos para corrigir:

1. `CargaDaVida.turnos` executa o pareamento da janela larga apenas para carregar `_bats`. Uma falha em DNA de outro dia pode interromper a subjanela válida que o avulso julgaria. Carregar as batidas diretamente com os filtros da base evita essa mudança de entrada e o processamento descartado. (`chamados/services/carga_pergunta_viva.py:163`, `ponto/turnos.py:3032`, `escala/utils.py:19`, `escala/utils.py:107`)

2. A leitura das testemunhas constrói até três ramos SQL por pergunta. A quantidade de queries é constante, mas a árvore SQL cresce linearmente; o limite concreto do banco não foi executado. [nao verificado] Um envelope mínimo/máximo de timestamps mais `pergunta_origem_id__in` preserva os filtros posteriores e mantém a consulta pequena. (`chamados/services/carga_pergunta_viva.py:107`, `chamados/services/carga_pergunta_viva.py:129`, `chamados/services/carga_pergunta_viva.py:133`)

Nos trechos revistos, a passagem de carga conserva as comparações e a ordem dos degraus do C6; ela substitui fontes de insumos. (`chamados/juizes.py:1338`, `chamados/juizes.py:1408`, `chamados/juizes.py:1421`)

Não identifiquei N+1 residual adicional no caminho alimentado. A confirmação por SQL permanece [nao verificado].

_3627 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
