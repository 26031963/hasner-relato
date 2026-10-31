# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 15:53** · publicado em **31/10/2026 15:56** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 15:53 · sessao 099205dd302f · TRAVA-MENOS-FILA · leitura · origem=Code

**(a) O histórico deve nascer dentro de `bin/trava_teste.sh`, em torno da aquisição e da execução.** Instrumentar apenas `suite.sh` perderia régua, pre-push e demais entradas diretas (`bin/regua.sh:165`, `bin/pre-push.sh:142`, `bin/vigia_arvore.sh:83`).

Os pontos são:

- **Pedido:** depois de validar argumentos e imediatamente antes de abrir/esperar o lock; nunca depois do `flock`, pois apagaria a espera (`bin/trava_teste.sh:33`, `bin/trava_teste.sh:35-36`).
- **Início:** imediatamente após o `flock` conseguir a vez, antes de escrever `.quem` e executar o comando. Assim mede espera pela trava, separada do trabalho protegido (`bin/trava_teste.sh:36-42`).
- **Fim e rc:** após `"$@"`, capturando `$?` antes de qualquer escrita. Hoje esse comando é a última instrução: acrescentar um append sem preservar o status pode trocar o resultado do teste pelo resultado do histórico (`bin/trava_teste.sh:42`).
- **Sem vez:** registrar tentativa encerrada com `inicio=null`, fim e `rc=75`, preservando a saída atual. O finalizador precisa cobrir esse caminho e conservar a limpeza de `.quem` somente quando houve aquisição; hoje o `EXIT` é instalado depois do caminho que sai 75 (`bin/trava_teste.sh:36-41`).
- **Lock do histórico:** usar outro descritor, em escopo curto e com espera limitada; substituir o FD 9 ou manter o lock do histórico durante o run alteraria o dispositivo atual. O FD 9 pertence à trava principal (`bin/trava_teste.sh:35-42`).

**Uma linha final por tentativa serve para histórico concluído; não serve, sozinha, para publicar a fila corrente.** Pedido, início e término precisam de eventos correlacionados ou de estado corrente separado: antes da aquisição ainda não existe início, e antes de terminar não existem fim/rc (`bin/trava_teste.sh:36-42`).

**(b) Chamadores diretos encontrados e identificação disponível:**

| Chamador | O que transmite à trava |
|---|---|
| Régua | `ESTEIRA_QUEM=regua` (`bin/regua.sh:165`). |
| Pre-push | `pre-push`; a função atende arsenal e control-plane, portanto um push pode produzir duas tentativas (`bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198`). |
| `suite.sh` | Identidade herdada ou `suite` (`bin/suite.sh:96`). |
| Vigia da árvore | `vigia_arvore`, em dois pontos de execução (`bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:131`). |
| Régua do relógio | `regua_relogio` (`bin/regua_relogio.sh:51`). |
| Molde das fatias | `fatia`, nos RED/GREEN parciais (`bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46-47`). |

**Todos têm ferramenta nomeável; não têm obra obrigatoriamente identificada.** A trava constrói `quem` como ferramenta mais PID; o molde conhece `__FATIA__`, mas transmite somente `fatia` (`bin/trava_teste.sh:23`, `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`).

O “label de `suite.sh`” não resolve: `--only` e `LABELS` são alvos de testes, não identificadores da obra; o parser não oferece argumento de obra (`bin/suite.sh:43-53`, `bin/suite.sh:59`, `bin/suite.sh:102`). Uso padronizado de `SUITE_OBRA` pelos chamadores: [nao verificado].

O instalador de hooks gera delegação a `bin/pre-push.sh`; há também uma cópia em `app/core/hooks/pre-push.sh` que executa Docker diretamente. Qual hook está efetivamente instalado: [nao verificado] (`bin/hooks.sh:51-56`, `app/core/hooks/pre-push.sh:54`, `app/core/hooks/pre-push.sh:82`, `app/core/hooks/pre-push.sh:87`).

**(c) O contrato precisa corrigir estes pontos:**

1. **“Único portão do `juliani_db_test`” excede a cobertura existente.** Calendário e molde de autocura executam suites completas diretamente; isolamento, placar, selo da fatia e vigia Python também executam testes sem passar pela trava (`bin/regua_calendario.sh:41-45`, `bin/molde_autocura/rodar.sh:20-26`, `bin/isolamento.sh:31-33`, `bin/placar_code.sh:122-126`, `bin/molde_fatia/rodar.sh:28-30`, `app/core/esteira_vigia.py:751-760`). `REGUA_DB` muda o banco de teste, enquanto o host padrão continua `juliani_db_test` (`app/config/settings/ci.py:12`, `app/config/settings/ci.py:17`). O selo não garante universalidade: varre `bin/*.sh`, tem exceções por `REGUA_DB` e aceita presença textual da trava no arquivo (`bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:59-62`).

2. **“Pedidos sem início” inclui tentativas já encerradas.** Um timeout sai sem executar o comando; contá-lo como pendente produziria fila permanente. É necessário distinguir aguardando, admitido e encerrado sem admissão (`bin/trava_teste.sh:36-42`). Além disso, `rc=75` isolado não prova timeout: o comando protegido também pode devolver 75; o registro precisa indicar se houve aquisição (`bin/trava_teste.sh:38`, `bin/trava_teste.sh:42`).

3. **`--quem` não prova posse nem mede fila.** Ele lê apenas um arquivo lateral; as atualizações acontecem depois da aquisição e no `EXIT`, permitindo divergência entre texto e lock, inclusive após morte abrupta (`bin/trava_teste.sh:28`, `bin/trava_teste.sh:40-41`).

4. **Continuar o run após append falho é coerente com “hist é testemunha”, mas deixa cobertura incompleta.** “Conta FALHA” precisa de canal independente: tentar registrar a falha somente no histórico que falhou pode torná-la invisível. Esse canal não existe na trava atual (`bin/trava_teste.sh:20-42`). O contrato também precisa impedir espera indefinida pelo lock do histórico; o mecanismo proposto ainda está [nao verificado].

5. **A média pode esconder justamente as piores esperas.** Excluir tentativas sem início retira os timeouts; atribuir-lhes zero também falseia a espera. Publicaria média dos admitidos com quantidade de amostras, negados separados e idade dos pendentes, pois o caminho 75 efetivamente esperou e não começou (`bin/trava_teste.sh:36-38`). Janela por pedido/início/fim, fuso, dias civis versus janelas móveis, gatilho por obra versus agregado e fonte da conta da VM não estão definidos no contrato enviado: [nao verificado].

6. **O destino do ESTADO precisa ser corrigido.** O fluxo encontrado gera `$TMP/ESTADO.md` e publica no destino padrão `/home/ronald/hasner-relato`, não em `app/docs/ESTADO.md` (`bin/relato.sh:23`, `bin/relato.sh:38`, `bin/relato.sh:100-102`, `bin/gerar_estado.py:376`). O template atual não contém bloco TRAVA (`bin/gerar_estado.py:295-369`). O novo leitor deve conter falhas de importação, leitura e composição para publicar `nao sei`; sem contenção, erro do gerador encerra o ciclo de publicação (`bin/gerar_estado.py:289-293`, `bin/relato.sh:38`).

7. **A instrumentação precisa preservar o isolamento dos testes da própria trava.** O selo copia o script e altera apenas o caminho do lock; acrescentar histórico com caminho real pode fazer suas execuções sintéticas contaminarem a medida publicada (`bin/tests/test_trava_teste.sh:101-103`, `bin/tests/test_trava_teste.sh:119-137`).

## 31/10 15:53 · sessao 1d030a5d304b · V3-RETOMA · leitura · origem=Code

**Os textos examinados usam a duração diária para as extras e a faixa horária para o trabalho noturno. Não estabelecem expressamente que as extras sejam sempre os últimos minutos trabalhados, nem disciplinam sua distribuição entre vários períodos intradiários.** Essa constatação limita-se aos dispositivos e verbetes abaixo. [CLT, normaalizada-pe.html:300–307, 360–368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

As citações entre aspas são **trechos literais selecionados**; as explicações restantes são paráfrases. As linhas indicam a representação textual dos documentos externos consultados.

**1. CLT: dispositivo, literalidade e situação**

O art. 59, caput e §§ 1º, 2º, 3º, 5º e 6º, consta do texto atualizado; o § 4º está revogado. [normaatualizada-pe.html:300–307](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

| Dispositivo | Trecho literal selecionado | Conteúdo, em paráfrase |
|---|---|---|
| Art. 59, caput | “A duração diária do trabalho poderá ser acrescida de horas extras” | Refere-se à duração **diária**; prevê até duas extras mediante os instrumentos indicados. [l13467.htm:105](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 1º | “50% (cinquenta por cento)” | Acréscimo mínimo sobre a hora normal. [l13467.htm:107](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 2º | “em um dia” | Permite compensar excesso diário pela diminuição em outro dia, mediante acordo ou convenção coletiva, observados o período máximo anual e o limite de dez horas diárias. [normaatualizada-pe.html:302](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 3º | “horas extras não compensadas” | Determina seu pagamento na rescisão, segundo a remuneração dessa data. [l13467.htm:111](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 4º | — | **Revogado** pela Lei 13.467/2017. [l13467.htm:113, 987–992](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 5º | “seis meses” | Banco de horas por acordo individual escrito, com compensação nesse prazo máximo. [l13467.htm:115](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |
| § 6º | “no mesmo mês” | Compensação por acordo individual tácito ou escrito. [l13467.htm:117](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2017/lei/l13467.htm) |

Os quatro parágrafos solicitados do art. 73 constam do texto atualizado, sem indicação de revogação. [normaatualizada-pe.html:361–368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

| Dispositivo | Trecho literal selecionado | Conteúdo, em paráfrase |
|---|---|---|
| Art. 73, § 1º | “30 (trinta) segundos” | Hora noturna computada com 52 minutos e 30 segundos. [normaatualizada-pe.html:361](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 2º | “Considera-se noturno” | Define o trabalho entre 22h de um dia e 5h do seguinte. [normaatualizada-pe.html:363](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 4º | “Nos horários mistos” | Aplica as disposições do artigo às horas noturnas dos horários que abrangem períodos diurnos e noturnos. [normaatualizada-pe.html:366](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |
| § 5º — íntegra | “Às prorrogações do trabalho noturno aplica-se o disposto neste capítulo.” | Estende as disposições do capítulo à prorrogação. [normaatualizada-pe.html:368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html) |

Portanto, **quantificação diária**, **localização no relógio** e **prorrogação noturna** aparecem nesses dispositivos; uma regra expressa de seleção cronológica dos minutos extraordinários não aparece. [normaatualizada-pe.html:300–307, 360–368](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html)

**2. Súmulas e OJs indicadas**

| Verbete | Texto literal relevante | O que o trecho efetivamente disciplina |
|---|---|---|
| **Súmula 60, II** | “Cumprida integralmente a jornada no período noturno e prorrogada esta, devido é também o adicional quanto às horas prorrogadas.” | Adicional na prorrogação noturna. Transcrição do verbete em documento oficial: [acordao.do, E-ED-RR-825-88.2010.5.03.0064:171–176](https://consultadocumento.tst.jus.br/consultaDocumento/acordao.do?anoProcInt=2013&dtaPublicacaoStr=28%2F05%2F2021+07%3A00%3A00&nia=7637301&numProcInt=170108). |
| **Súmula 85, III e IV** | “horas excedentes à jornada normal diária” | Compensação e consequências de sua invalidade, com referências aos módulos diário e semanal. [documento d4a380524f995d9b0bb461bf24733a13:89–91](https://jurisprudencia-backend.tst.jus.br/rest/documentos/d4a380524f995d9b0bb461bf24733a13). |
| **Súmula 366** | “Se ultrapassado esse limite, será considerada como extra a totalidade do tempo que exceder a jornada normal.” | Variações de registro e tolerância; o trecho não identifica necessariamente o excesso com o final do dia. [documento 8e88a25e9f40b1e5e2199ac0e3dd8c43:147–149](https://jurisprudencia-backend.tst.jus.br/rest/documentos/8e88a25e9f40b1e5e2199ac0e3dd8c43). |
| **Súmula 437, I** | “sem prejuízo do cômputo da efetiva jornada de labor para efeito de remuneração” | Pagamento relacionado à supressão do intervalo e preservação do cômputo do trabalho efetivo. [documento f22a2b6f90b469ac17ad6ba9b3c9e847:51–54](https://jurisprudencia-backend.tst.jus.br/rest/documentos/f22a2b6f90b469ac17ad6ba9b3c9e847). |
| **OJ SDI-1 97 — íntegra** | “O adicional noturno integra a base de cálculo das horas extras prestadas no período noturno.” | **Base de cálculo** das extras noturnas. [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:8318–8320](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1). |
| **OJ SDI-1 388** | “após as 5 horas da manhã” | Adicional nas horas posteriores às 5h, em jornada 12×36 que compreenda todo o período noturno. [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:10520–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1). |

**Situação dos verbetes:**

- **366 e 437: canceladas.** A Resolução 225/2025 registra, respectivamente nos incisos XV e XXII, cancelamento por perda de eficácia desde 11/11/2017 pela Lei 13.467/2017. Trecho literal comum: “cancelada por perda de eficácia a partir de 11/11/2017”. [2025_res0225.pdf:83–84, 99–100](https://juslaboris.tst.jus.br/bitstream/handle/20.500.12178/251919/2025_res0225.pdf?isAllowed=y&sequence=1)
- **60, 85, OJ 97 e OJ 388:** constam sem cancelamento na edição oficial de 2024 e não figuram nos cancelamentos da Resolução 225/2025. **Vigência atual em 31/10/2026: [NAO VERIFICADO].** [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:3007–3014, 3212–3236, 8318–8320, 10520–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1); [2025_res0225.pdf:51–136](https://juslaboris.tst.jus.br/bitstream/handle/20.500.12178/251919/2025_res0225.pdf?isAllowed=y&sequence=1)

**3. Resposta específica sobre quais minutos são extras**

Nos seis verbetes examinados, **não há comando literal que reserve as extras aos últimos minutos cronológicos do dia**, nem regra específica para atribuir extras noturnas quando a prorrogação ocorre em período anterior ao último. [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf:3007–3014, 3212–3236, 6280–6288, 7293–7312, 8318–8320, 10520–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1)

A OJ 97 pressupõe horas extras prestadas em período noturno e determina sua composição remuneratória; ela não fornece o critério anterior para escolher esses minutos. A OJ 388, por sua vez, fala em adicional após as 5h sem qualificar essas horas como extraordinárias. **Essa distinção é leitura delimitada dos textos.** [Mesmo PDF:8320, 10526–10528](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1)

**[NAO VERIFICADO]** Não consegui fundamentar literalmente uma regra geral que imponha: **“extras = últimos minutos trabalhados”**, **“excesso apurado separadamente por período”** ou uma solução específica para o período anterior ao último. Também permanece **[NAO VERIFICADO]** afirmar que inexista qualquer outra Súmula ou OJ do TST sobre essa seleção temporal.

## 31/10 15:52 · sessao 2e56327f1075 · - · - · origem=Ronald (tmux)

O ESTADO publicado nasce em temporário, não em `app/docs/ESTADO.md`: `bin/relato.sh:38` chama o gerador com `$TMP/ESTADO.md`; o gerador escreve o destino recebido em `bin/gerar_estado.py:376`; o publicador envia o temporário ao raspador em `bin/relato.sh:100-102`, com destino padrão `/home/ronald/hasner-relato` (`bin/relato.sh:23`), e o raspador grava em `dest/dst` (`bin/raspar_publico.py:40`). O template atual não contém TRAVA, e suas métricas vêm dos leitores importados em `bin/gerar_estado.py:198-293` e do template em `bin/gerar_estado.py:295-369`.

Lacunas do contrato:

- **Uma linha final por run é insuficiente para fila corrente.** O início só fica conhecido depois de `flock` (`bin/trava_teste.sh:36-40`) e fim/rc depois do comando (`bin/trava_teste.sh:42`); se a linha nascer ao terminar, os pedidos ainda aguardando não aparecem. Se nascer antes, faltam início/fim/rc e são necessárias atualizações ou eventos correlacionados por ID. É consequência do desenho existente; o protocolo adicional está [nao verificado].
- **Pedidos sem início incluem encerrados.** Timeout sai por `75` antes do comando (`bin/trava_teste.sh:36-38`); o contrato precisa distinguir aguardando, negado e cancelado para não transformar timeout em fila eterna. Cancelamento/sinais não têm protocolo atual além do `EXIT` do detentor (`bin/trava_teste.sh:41`).
- **`--quem` é testemunho lateral.** Ele somente lê `$TRAVA.quem` ou imprime `livre` (`bin/trava_teste.sh:28`); o arquivo é atualizado depois da aquisição (`bin/trava_teste.sh:40`) e no `EXIT` (`bin/trava_teste.sh:41`). Logo há janelas de divergência entre o arquivo e o lock; o contrato não deve tratar essa leitura como prova suficiente da posse.
- **A observação não pode segurar a pista indefinidamente.** O lock atual ocupa FD 9 antes da espera e permanece durante o comando (`bin/trava_teste.sh:35-42`); usar FD próprio e espera limitada para o histórico evita substituir FD 9 ou aumentar a ocupação da pista. O prazo e o canal durável de FALHA do histórico estão [nao verificado].
- **Falha do histórico pode fabricar média boa.** Se append falhar e o run prosseguir, os runs omitidos não aparecem na média; “conta FALHA” precisa de canal independente e de indicação de cobertura incompleta. O mecanismo ainda está [nao verificado]; a trava atual não tem histórico nem contador (`bin/trava_teste.sh:20-42`).
- **`nao sei` precisa cobrir toda a seção.** O gerador já protege importação, leitura e composição de outro medidor conjuntamente (`bin/gerar_estado.py:287-293`); sem isso, uma exceção do novo leitor impede a geração e `relato.sh` encerra o ciclo (`bin/relato.sh:38`). Arquivo vazio, registros truncados, falha de permissões e ausência de amostras precisam de semântica explícita [nao verificado].
- **A média tem viés sem universo definido.** Excluir os `75` omite justamente quem esperou e não conseguiu iniciar (`bin/trava_teste.sh:36-38`); incluí-los como espera zero também falseia a medida. Devem aparecer separados como negados, com tempo observado como limite inferior, e pendentes com idade corrente [nao verificado].
- **“24 h” e “2 dias seguidos” são insuficientes para o gatilho.** O contrato não fixa inclusão por pedido/início/fim, fuso, dias civis versus janelas móveis, mínimo de amostras, agregado global versus por obra nem origem da conta da VM [nao verificado]. Datas com ano/fuso e duração monotônica são recomendáveis; o carimbo atual de `--quem` contém somente dia/mês/hora (`bin/trava_teste.sh:40-41`).

Há precedente útil de observabilidade na sombra: ela mede antes/depois do `flock`, registra inclusive negação (`bin/sombra_trava.sh:27-35`) e conta negados e linhas ilegíveis como problemas (`bin/sombra_trava.sh:55-60`), embora esse formato também não registre a fila enquanto espera (`bin/sombra_trava.sh:28-31`).

O selo atual só verifica serialização e timeout `75`, sem história ou medição de fila (`bin/tests/test_trava_teste.sh:117-137`).

## 31/10 15:52 · sessao ee9b1d9ee7f7 · - · - · origem=Ronald (tmux)

Chamadores diretos encontrados, todos com identidade de ferramenta, sem obra obrigatória:

| Chamador | Identidade transmitida | Evidência |
|---|---|---|
| Régua | `ESTEIRA_QUEM=regua` | `bin/regua.sh:165` |
| Pre-push | `ESTEIRA_QUEM=pre-push`, na função compartilhada pelo arsenal e pelo control-plane | `bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:198` |
| Suite | `ESTEIRA_QUEM` herdado ou `suite` | `bin/suite.sh:96` |
| Vigia da árvore | `ESTEIRA_QUEM=vigia_arvore`, para suite/reteste e confirmação viva | `bin/vigia_arvore.sh:83`, `bin/vigia_arvore.sh:88`, `bin/vigia_arvore.sh:106`, `bin/vigia_arvore.sh:131` |
| Régua do relógio | `ESTEIRA_QUEM=regua_relogio` | `bin/regua_relogio.sh:51` |
| Molde das fatias | `ESTEIRA_QUEM=fatia`, usado nos RED/GREEN parciais | `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`, `bin/molde_fatia/rodar.sh:47` |

`QUEM` acrescenta PID à identidade, portanto identifica uma execução da ferramenta, sem identificar necessariamente a obra (`bin/trava_teste.sh:23`). O molde dispõe de `__FATIA__` e bancos específicos, mas envia apenas `fatia` à trava (`bin/molde_fatia/rodar.sh:29`, `bin/molde_fatia/rodar.sh:40`, `bin/molde_fatia/rodar.sh:46`). O parser de `suite.sh` aceita `--dir`, `--only`, `--parallel`, `--espera` e argumentos extras; não oferece argumento de obra (`bin/suite.sh:43`, `bin/suite.sh:46`, `bin/suite.sh:50`, `bin/suite.sh:52`). `LABELS` é lista de apps de teste lida da régua, utilizada como alvo do `manage.py test`, não identificador de obra (`bin/suite.sh:59`, `bin/suite.sh:102`). Não encontrei uso de `SUITE_OBRA` na busca do repositório [nao verificado].

O hook que o instalador gera delega a `bin/pre-push.sh`; a instalação efetiva nesta máquina não foi conferida (`bin/hooks.sh:51`, `bin/hooks.sh:56`; [nao verificado]). Existe uma cópia antiga em `app/core/hooks/pre-push.sh` que executa Docker diretamente, sem passar por `trava_teste.sh`; não há prova de que esteja instalada (`app/core/hooks/pre-push.sh:54`, `app/core/hooks/pre-push.sh:82`, `app/core/hooks/pre-push.sh:87`; [nao verificado]).

A premissa “único portão do juliani_db_test” não é verdadeira para todo executor existente: o CI aponta por default ao mesmo container e apenas varia o nome do banco quando recebe `REGUA_DB` (`app/config/settings/ci.py:12`, `app/config/settings/ci.py:17`).

- `regua_calendario.sh` executa a suite inteira diretamente três vezes, com bancos próprios no container default (`bin/regua_calendario.sh:41`, `bin/regua_calendario.sh:43`, `bin/regua_calendario.sh:44`, `bin/regua_calendario.sh:45`).
- O molde de autocura executa Docker diretamente e inclui um GREEN completo (`bin/molde_autocura/rodar.sh:20`, `bin/molde_autocura/rodar.sh:25`, `bin/molde_autocura/rodar.sh:26`).
- Há recortes diretos no isolamento, placar e selo do molde de fatia (`bin/isolamento.sh:31`, `bin/isolamento.sh:33`; `bin/placar_code.sh:121`, `bin/placar_code.sh:125`; `bin/molde_fatia/rodar.sh:28`, `bin/molde_fatia/rodar.sh:30`).
- O vigia Python também monta e executa Docker diretamente (`app/core/esteira_vigia.py:751`, `app/core/esteira_vigia.py:757`, `app/core/esteira_vigia.py:760`).

O selo não prova cobertura universal: percorre somente `bin/*.sh`, aceita presença textual de `trava_teste.sh` em qualquer parte do código e isenta `REGUA_DB` fora de dois padrões limitados de suite inteira (`bin/tests/test_trava_teste.sh:40`, `bin/tests/test_trava_teste.sh:59`, `bin/tests/test_trava_teste.sh:60`, `bin/tests/test_trava_teste.sh:62`). Seus exercícios da trava usam uma cópia isolada, não o dispositivo global; instrumentação nova não deve contaminar o histórico real com esses runs (`bin/tests/test_trava_teste.sh:101`, `bin/tests/test_trava_teste.sh:103`, `bin/tests/test_trava_teste.sh:119`, `bin/tests/test_trava_teste.sh:134`).

## 31/10 15:50 · sessao b06ea5b91449 · - · - · origem=Ronald (tmux)

Não localizei fonte primária que formule literalmente a regra “as horas extras são necessariamente os últimos minutos trabalhados cronologicamente”, nem regra específica de imputação quando o excesso ocorre em período anterior ao último. A **inexistência universal** desse verbete permanece **[NAO VERIFICADO]**; não recomendo transformar o resultado negativo da busca em afirmação de inexistência.

Achados úteis para a consolidação:

- **Súmula 264** trata de composição remuneratória, não de seleção temporal dos minutos. Sua reafirmação no **Tema 280**, processo **RR-0000254-24.2023.5.09.0411**, consta de acórdão primário do TST, datado de Brasília, **25/08/2025**, reproduzido no [PDF oficial hospedado pelo TRT3](https://portal.trt3.jus.br/internet/jurisprudencia/incidentes-suscitados-irr-iac-arginc-tst/downloads/acordao-de-merito-tema-280.pdf), **ID acd3354, pág. 7**. Trecho literal curto: “A remuneração das horas extraordinárias é composta do valor da hora normal”. A numeração das linhas não pôde ser obtida: **[NAO VERIFICADO: arquivo:linha]**. O resultado indexado contém o dispositivo integral.
- **OJ SDI-1 127**: refere-se à subsistência da hora reduzida após a Constituição; não oferece regra de distribuição cronológica. **OJ SDI-1 395**: refere-se à compatibilidade entre hora reduzida e turnos ininterruptos de revezamento; também não seleciona minutos extraordinários. Ambas aparecem no [Livro oficial do TST](https://www.tst.jus.br/documents/10157/63003/Livro-Internet.pdf), em resultados indexados; a OJ395 aparece na página interna **C-97** da edição indexada. Vigência atual e linhas: **[NAO VERIFICADO]**, pois o documento aberto retornou 403.
- A redação da **OJ388** sobre empregado submetido a jornada **12x36** ajuda a distinguir **prorrogação do período noturno** de **excesso da jornada contratual**: o texto trata de adicional após 5h dentro desse regime, sem identificar tais horas como extras. Essa distinção é uma **inferência do texto**, não transcrição de regra geral. O outro agente deve fornecer o texto e a situação confirmados.
- As buscas pelos termos “últimos minutos”, “ordem cronológica”, “primeiro período”, “último período” e “dois períodos”, associados a horas extras/noturnas no domínio TST, não produziram enunciado específico. Essa é uma limitação da pesquisa, não prova jurídica de inexistência. **[NAO VERIFICADO]**.

O endereço aparentemente atual do livro é [livrointernet-12-pdf](https://www.tst.jus.br/documents/d/guest/livrointernet-12-pdf), mas também retornou 403. A rede no shell não resolveu DNS; nenhum arquivo foi criado, editado ou apagado.

## 31/10 15:50 · sessao 85b784e75287 · - · - · origem=Ronald (tmux)

Fontes oficiais acessíveis:

- **Livro do TST reproduzido pelo TRT3**, atualizado em 28/06/2024: [JURISPRUDÊNCIA CONSOLIDADA_VERSÃO_COMPLETA_28_6_2024.pdf](https://as1.trt3.jus.br/bd-trt3/bitstream/handle/11103/17962/JURISPRUD%C3%8ANCIA%20CONSOLIDADA_VERS%C3%83O_COMPLETA_28_6_2024.pdf?isAllowed=y&sequence=1). A própria obra informa que incorporou o livro do TST extraído do sítio oficial, linhas146–148. Permite reprodução de qualquer parte com citação da fonte, linhas42–43.
- **Resolução TST225/2025**: [2025_res0225.pdf](https://juslaboris.tst.jus.br/bitstream/handle/20.500.12178/251919/2025_res0225.pdf?isAllowed=y&sequence=1),141 linhas.

**Súmula60,II**, livro TRT3:3012–3014,página73:

> Cumprida integralmente a jornada no período noturno e prorrogada esta, devido é também o adicional quanto às horas prorrogadas. Exegese do art.73,§5º,da CLT.

ItemI,livro TRT3:3010–3011:

> O adicional noturno, pago com habitualidade, integra o salário do empregado para todos os efeitos.

**Súmula85,III**,livro TRT3:3221–3225,página77:

> O mero não atendimento das exigências legais para a compensação de jornada, inclusive quando encetada mediante acordo tácito, não implica a repetição do pagamento das horas excedentes à jornada normal diária, se não dilatada a jornada máxima semanal, sendo devido apenas o respectivo adicional.

ItemIV,livro TRT3:3226–3230:

> A prestação de horas extras habituais descaracteriza o acordo de compensação de jornada. Nesta hipótese, as horas que ultrapassarem a jornada semanal normal deverão ser pagas como horas extraordinárias e, quanto àquelas destinadas à compensação, deverá ser pago a mais apenas o adicional por trabalho extraordinário.

Inteiro teor dos itensI–VI:3212–3236. Não contém regra que selecione cronologicamente os últimos minutos como extras; trata de compensação de jornada, conforme esse trecho delimitado.

**Súmula366**,livro TRT3:6280–6288,página129:

> Não serão descontadas nem computadas como jornada extraordinária as variações de horário do registro de ponto não excedentes de cinco minutos, observado o limite máximo de dez minutos diários. Se ultrapassado esse limite, será considerada como extra a totalidade do tempo que exceder a jornada normal, pois configurado tempo à disposição do empregador, não importando as atividades desenvolvidas pelo empregado ao longo do tempo residual (troca de uniforme, lanche, higiene pessoal, etc).

**Cancelada**. Resolução225/2025,art.1º,XV,2025_res0225.pdf:83–84:

> XV - Súmula nº366 (cancelada por perda de eficácia a partir de11/11/2017,pela Lei13.467/2017);

**Súmula437,I**,livro TRT3:7296–7300,página147:

> Após a edição da Lei nº8.923/94,a não concessão ou a concessão parcial do intervalo intrajornada mínimo, para repouso e alimentação, a empregados urbanos e rurais, implica o pagamento total do período correspondente, e não apenas daquele suprimido, com acréscimo de, no mínimo,50% sobre o valor da remuneração da hora normal de trabalho (art.71 da CLT),sem prejuízo do cômputo da efetiva jornada de labor para efeito de remuneração.

ItemIV,livro TRT3:7309–7312:

> Ultrapassada habitualmente a jornada de seis horas de trabalho, é devido o gozo do intervalo intrajornada mínimo de uma hora, obrigando o empregador a remunerar o período para descanso e alimentação não usufruído como extra, acrescido do respectivo adicional, na forma prevista no art.71,caput e§4º da CLT.

**Cancelada**. Resolução225/2025,art.1º,XXII,2025_res0225.pdf:99–100:

> XXII - Súmula nº437 (cancelada por perda de eficácia a partir de11/11/2017,pela Lei13.467/2017);

**OJSDI-197**,livro TRT3:8318–8320,página168:

> O adicional noturno integra a base de cálculo das horas extras prestadas no período noturno.

**OJSDI-1388**,livro TRT3:10520–10528,páginas206–207:

> O empregado submetido à jornada de12 horas de trabalho por36 de descanso, que compreenda a totalidade do período noturno, tem direito ao adicional noturno, relativo às horas trabalhadas após as5 horas da manhã.

**Vigência formal:** Súmulas60 e85 e OJs97 e388 constam sem cancelamento na edição oficial de2024 citada. Não estão na relação de cancelamentos da Resolução225/2025,artigos1º–6º,2025_res0225.pdf:51–136. Sua manutenção até31/10/2026 está **[NAO VERIFICADO]**, pois o livro atual do TST localizado em [livrointernet-12-pdf](https://www.tst.jus.br/documents/d/guest/livrointernet-12-pdf) bloqueou a leitura integral com403.

**Resultado delimitado da leitura:** nenhum dos seis verbetes acima afirma literalmente que as horas extras correspondem necessariamente aos últimos minutos trabalhados no dia, nem apresenta algoritmo para distribuir extras noturnas entre vários períodos intradiários; as referências são as linhas de cada verbete acima. A inexistência de qualquer outra Súmula ou OJ do TST com essa regra permanece **[NAO VERIFICADO]**.

## 31/10 15:49 · sessao 004f7cf3dbc6 · - · - · origem=Ronald (tmux)

Fonte primária oficial: [CLT atualizada — Câmara dos Deputados](https://www2.camara.leg.br/legin/fed/declei/1940-1949/decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html). As linhas abaixo são da representação textual recuperada pela ferramenta web, sem arquivo local criado.

| Dispositivo | Arquivo:linha | Situação registrada |
|---|---|---|
| Art. 59, caput | `decreto-lei-5452-1-maio-1943-415500-normaatualizada-pe.html:300` | Vigente; redação Lei 13.467/2017 |
| § 1º | mesmo arquivo:301 | Vigente; redação Lei 13.467/2017 |
| § 2º | mesmo arquivo:302 | Vigente; redação MP 2.164-41/2001 |
| § 3º | mesmo arquivo:303 | Vigente; redação Lei 13.467/2017 |
| § 4º | mesmo arquivo:304 | **Revogado pela Lei 13.467/2017** |
| § 5º | mesmo arquivo:306 | Vigente; incluído Lei 13.467/2017 |
| § 6º | mesmo arquivo:307 | Vigente; incluído Lei 13.467/2017 |
| Art. 73, caput | mesmo arquivo:360 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 1º | mesmo arquivo:361 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 2º | mesmo arquivo:363 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 4º | mesmo arquivo:366 | Texto vigente apresentado; redação DL 9.666/1946 |
| § 5º | mesmo arquivo:368 | Texto vigente apresentado; redação DL 9.666/1946 |

Dois trechos literais centrais, totalizando 25 palavras da página:

- Art. 59, caput: “A duração diária do trabalho poderá ser acrescida de horas extras” — arquivo:300.
- Art. 73, § 5º: “Às prorrogações do trabalho noturno aplica-se o disposto neste capítulo.” — arquivo:368.

Conteúdo confirmado dos demais dispositivos, em paráfrase:

- Art. 59, § 1º: remuneração da extra pelo menos 50% superior à normal; § 2º: compensação do excesso de um dia pela diminuição em outro, com limites anual e diário; § 3º: extras não compensadas pagas na rescisão; §§ 5º e 6º: compensação semestral por acordo individual escrito e mensal por acordo individual tácito ou escrito. — arquivo:301–307.
- Art. 73, § 1º: hora noturna de 52 minutos e 30 segundos; § 2º: trabalho entre 22h de um dia e 5h do seguinte; § 4º: aplicação das disposições às horas noturnas nos horários mistos. — arquivo:361,363,366.
- Os dispositivos consultados **não contêm regra expressa identificando as extras como os últimos minutos cronológicos trabalhados**, nem regra específica para prorrogação situada em período anterior ao último. Essa constatação limita-se ao texto desses artigos. — arquivo:300–307,360–368.
- Apoio contextual: art. 58 estabelece duração normal diária; art. 71, § 2º exclui os intervalos de descanso da duração do trabalho. — arquivo:285,350.

Limitação operacional: tentativa de recuperação direta pelo shell falhou por resolução DNS; Planalto retornou bloqueio. A Câmara forneceu o texto oficial acessível. Nenhum arquivo foi criado, alterado ou apagado. [NAO VERIFICADO por arquivo:linha; evidenciado apenas pelas saídas das ferramentas]

## 31/10 15:48 · sessao 161e3a1c4479 · REPROVA-E-DESENHO-CODEX · verificador · origem=Code

**REPROVADO — PROCEDENTE, BLOQUEIA.** #8 e #9 continuam parcialmente abertos na atualização do estado antigo; também há caminhos que autorizam Codex sem leitura válida da triagem ou depois de a obra parar. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:883`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:166`)

“Morde” abaixo significa que o caso distingue estaticamente o comportamento de `8609b12c`. Execução integral do selo: **[não verificado]**; ele cria e remove arquivos. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

| Achado v2 | Está curado? | O selo morde a volta ao comportamento antigo? |
|---|---|---|
| **#1** | **Sim, no cenário relatado. Não PROCEDENTE, não-bloqueia.** A porta executa a decisão, incluindo triagens pendentes. (`bin/disjuntor.py:982`) | **Sim.** A quarta chamada passa pelo wrapper sem Stop/PreToolUse intermediário; exige rc 3 e ausência de resposta. Mutante restaura consulta apenas ao estado. (`bin/tests/test_disjuntor.sh:1216`, `bin/tests/test_disjuntor.sh:1224`, `bin/tests/test_disjuntor.sh:1457`) |
| **#2** | **Sim. Não PROCEDENTE, não-bloqueia.** A mesma decisão lê e aplica a liberação válida. (`bin/disjuntor.py:906`, `bin/disjuntor.py:982`) | **Sim.** Escreve DESENHO após disparar e chama diretamente o wrapper; a base continuaria negando. (`bin/tests/test_disjuntor.sh:1229`, `bin/tests/test_disjuntor.sh:1235`, `bin/tests/test_disjuntor.sh:1458`) |
| **#3** | **Sim, para os casos relatados. Não PROCEDENTE, não-bloqueia.** `shlex` preserva valores completos e a última atribuição prevalece. (`bin/disjuntor.py:1254`, `bin/disjuntor.py:1261`) | **Sim.** Exercita atribuição repetida, texto dentro de `DUMMY` e `O9 (C)`. (`bin/tests/test_disjuntor.sh:1261`, `bin/tests/test_disjuntor.sh:1262`, `bin/tests/test_disjuntor.sh:1263`, `bin/tests/test_disjuntor.sh:1464`) |
| **#6** | **Sim, com fonte íntegra. Não PROCEDENTE nesse cenário, não-bloqueia.** Liberação e rearme arquivam todas as identidades triadas, incluindo descartadas. (`bin/disjuntor.py:913`, `bin/disjuntor.py:958`) | **Sim.** Reclassifica antes do reset e depois acrescenta duas novas voltas; a base recontaria a antiga. (`bin/tests/test_disjuntor.sh:1273`, `bin/tests/test_disjuntor.sh:1277`, `bin/tests/test_disjuntor.sh:1281`, `bin/tests/test_disjuntor.sh:1466`) |
| **#7** | **Sim. Não PROCEDENTE, não-bloqueia.** O rearme alcança obra que só possui triagens. (`bin/disjuntor.py:951`, `bin/disjuntor.py:952`) | **Sim.** Rearma antes de qualquer decisão B3; a base recusaria. (`bin/tests/test_disjuntor.sh:1287`, `bin/tests/test_disjuntor.sh:1291`, `bin/tests/test_disjuntor.sh:1469`) |
| **#8** | **Parcial. PROCEDENTE, BLOQUEIA.** Ausência só é reconhecida como perda quando o contador novo já é positivo. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`) | **Sim no percurso novo; falta o estado herdado.** O caso cria o contador pela versão nova antes de retirar a fonte. (`bin/tests/test_disjuntor.sh:1299`, `bin/tests/test_disjuntor.sh:1301`, `bin/tests/test_disjuntor.sh:1470`) |
| **#9** | **Parcial. PROCEDENTE, BLOQUEIA.** Rejeita corrupção estrutural e truncamento, mas a perda de linhas completas antes do primeiro contador novo passa. (`bin/disjuntor.py:819`, `bin/disjuntor.py:822`, `bin/disjuntor.py:828`) | **Sim para as variantes cobertas; falta a redução na migração.** O teste semeia o contador antes de remover linhas. (`bin/tests/test_disjuntor.sh:1314`, `bin/tests/test_disjuntor.sh:1324`, `bin/tests/test_disjuntor.sh:1326`, `bin/tests/test_disjuntor.sh:1472`) |
| **#10** | **Sim. Não PROCEDENTE, não-bloqueia.** Leitura, idempotência e append participam da trava do estado. (`bin/disjuntor.py:786`, `bin/disjuntor.py:800`) | **Sim.** Mantém a trava ocupada e exige recusa sem append; depois exige exatamente uma linha. (`bin/tests/test_disjuntor.sh:1344`, `bin/tests/test_disjuntor.sh:1346`, `bin/tests/test_disjuntor.sh:1350`, `bin/tests/test_disjuntor.sh:1475`) |
| **#12** | **Sim, nos cenários relatados. Não PROCEDENTE, não-bloqueia.** A guarda antecede a remoção; sidecar restante impede a chamada. (`bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:165`) | **Sim.** Confere preservação após negativa e recusa quando o sidecar não pode ser removido. (`bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1246`, `bin/tests/test_disjuntor.sh:1249`, `bin/tests/test_disjuntor.sh:1459`, `bin/tests/test_disjuntor.sh:1463`) |

O resíduo de **#8** é o próprio cenário antigo na atualização: JSON com `codex=[a,b,c]`, obra disparada e sem `b3_triagem_linhas`; a fonte ausente vira `{}`, apaga `codex`, permite liberar arquivando uma lista vazia e deixa as mesmas três voltas dispararem quando a fonte retorna. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:883`, `bin/disjuntor.py:913`, `bin/disjuntor.py:888`)

O resíduo de **#9**: nesse estado herdado sem contador, uma fonte reduzida de três para duas linhas completas é aceita e substitui a contagem conhecida sem FALHA. (`bin/disjuntor.py:811`, `bin/disjuntor.py:822`, `bin/disjuntor.py:830`, `bin/disjuntor.py:883`)

Há ainda uma limitação dos negativos do selo: `_xp` procura o recado de bloqueio; sua ausência também pode resultar de falha aberta. Isso afeta os negativos de reset/rearme e parte dos casos de corrupção. (`bin/tests/test_disjuntor.sh:1203`, `bin/tests/test_disjuntor.sh:1277`, `bin/tests/test_disjuntor.sh:1284`, `bin/tests/test_disjuntor.sh:1292`, `bin/tests/test_disjuntor.sh:1315`, `bin/tests/test_disjuntor.sh:1321`)

**(a)** O escritor físico do JSON é `_com_estado`: toma `flock`, chama o mutador e substitui o arquivo ainda sob a trava. As leituras gerais ocorrem nele e em `ler_estado`. (`bin/disjuntor.py:249`, `bin/disjuntor.py:253`, `bin/disjuntor.py:268`, `bin/disjuntor.py:274`, `bin/disjuntor.py:288`, `bin/disjuntor.py:294`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `b1[agente].checado/disparado/motivo` | `b1_do_agente`. (`bin/disjuntor.py:481`) | Cache e decisão; `b1_disparados` lê `disparado`. (`bin/disjuntor.py:476`, `bin/disjuntor.py:478`, `bin/disjuntor.py:479`, `bin/disjuntor.py:490`) |
| `b2.ultimo` | `b2_decide`. (`bin/disjuntor.py:1134`) | `b2_decide`. (`bin/disjuntor.py:1133`) |
| `b4_para.desde` | `b4_stop`, incluindo remoção. (`bin/disjuntor.py:1149`, `bin/disjuntor.py:1155`) | `b4_stop`. (`bin/disjuntor.py:1157`) |
| `b4.negou_desde` | `pretool`, incluindo remoção. (`bin/disjuntor.py:1374`, `bin/disjuntor.py:1379`) | `pretool`. (`bin/disjuntor.py:1373`, `bin/disjuntor.py:1378`) |
| `b3_limiar` | `_migra_limiar`. (`bin/disjuntor.py:710`) | `_migra_limiar`. (`bin/disjuntor.py:699`, `bin/disjuntor.py:707`) |
| `b3_sem_obra` | `decide_b3`. (`bin/disjuntor.py:871`) | Deduplicação e `_b3_sem_obra`. (`bin/disjuntor.py:870`, `bin/disjuntor.py:1518`) |
| `b3[obra].reprovados` | Migração, decisão e rearme. (`bin/disjuntor.py:709`, `bin/disjuntor.py:877`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Os mesmos percursos. (`bin/disjuntor.py:705`, `bin/disjuntor.py:886`, `bin/disjuntor.py:912`, `bin/disjuntor.py:957`) |
| `b3[obra].disparado` | Decisão e rearme. (`bin/disjuntor.py:889`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Migração e decisão. (`bin/disjuntor.py:702`, `bin/disjuntor.py:888`, `bin/disjuntor.py:898`) |
| `b3[obra].codex` | Decisão e rearme. (`bin/disjuntor.py:883`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Decisão e rearme. (`bin/disjuntor.py:887`, `bin/disjuntor.py:913`, `bin/disjuntor.py:955`) |
| `b3[obra].rearmado` | Migração e rearme. (`bin/disjuntor.py:708`, `bin/disjuntor.py:957`) | Decisão. (`bin/disjuntor.py:874`, `bin/disjuntor.py:881`) |
| `b3[obra].liberado` | Decisão. (`bin/disjuntor.py:912`) | Decisão. (`bin/disjuntor.py:874`, `bin/disjuntor.py:881`, `bin/disjuntor.py:912`) |
| `b3[obra].consumidas` | Decisão. (`bin/disjuntor.py:915`) | Decisão. (`bin/disjuntor.py:908`) |
| `b3[obra].disparado_em` | `_fotografa`. (`bin/disjuntor.py:728`, `bin/disjuntor.py:730`) | Migração, fotografia e liberação. (`bin/disjuntor.py:703`, `bin/disjuntor.py:728`, `bin/disjuntor.py:910`) |
| `b3[obra].topo_no_disparo` | `_fotografa`. (`bin/disjuntor.py:727`, `bin/disjuntor.py:730`) | Decisão. (`bin/disjuntor.py:903`, `bin/disjuntor.py:908`) |
| `b3_triagem_linhas` | **`_triagens` e `triagem`.** (`bin/disjuntor.py:830`, `bin/disjuntor.py:797`) | Ambos. (`bin/disjuntor.py:811`, `bin/disjuntor.py:797`) |

Portanto, **há múltiplos escritores lógicos**, inclusive do contador novo; nesses percursos, todos passam pela mesma trava. Escrita fora da trava do JSON: **não PROCEDENTE, não-bloqueia**. (`bin/disjuntor.py:486`, `bin/disjuntor.py:800`, `bin/disjuntor.py:935`, `bin/disjuntor.py:971`, `bin/disjuntor.py:1136`, `bin/disjuntor.py:1162`, `bin/disjuntor.py:1382`)

Na trilha `triagem.log`, os quatro campos têm um escritor: `triagem`, sob essa trava. O leitor é `_triagens`; timestamp é separado, mas não validado nem usado; obra e volta são as chaves, e o último veredito prevalece. Idempotência, decisão e rearme consomem esse mapa. **Escritor fora da trava: não PROCEDENTE, não-bloqueia.** (`bin/disjuntor.py:794`, `bin/disjuntor.py:795`, `bin/disjuntor.py:800`, `bin/disjuntor.py:826`, `bin/disjuntor.py:829`, `bin/disjuntor.py:790`, `bin/disjuntor.py:883`, `bin/disjuntor.py:958`)

O sidecar fornece `obra/papel/hash/pid/fim/sha`: o wrapper escreve; `_le_id` lê; `triagem` verifica papel/SHA e compõe a identidade. A validação continua estrutural, preservando #14 sobre autoria. (`bin/codex_pergunta.sh:188`; `bin/disjuntor.py:757`, `bin/disjuntor.py:760`, `bin/disjuntor.py:780`, `bin/disjuntor.py:782`, `bin/disjuntor.py:784`)

**(b)** A liberação normal exige linha reconhecida, não consumida, fora da fotografia e posterior ao disparo; falha de persistência invalida a liberação. O rearme administrativo explicitamente zera a parada sem DESENHO/ERRO-BOBO. (`bin/disjuntor.py:906`, `bin/disjuntor.py:908`, `bin/disjuntor.py:910`, `bin/disjuntor.py:936`, `bin/disjuntor.py:959`)

Entretanto, **há passagem indevida — PROCEDENTE, BLOQUEIA**:

- **Codex depois de a obra parar:** a guarda ocorre antes da espera por slot. Outra decisão pode parar a obra durante essa espera; a chamada não repete a consulta. (`bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:146`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:166`)
- **Opus final com obra parada:** exceção no PreToolUse libera a ferramenta; classificação textual ou obra não reconhecida pode evitar a consulta B3. Persistem #4 e #5. (`bin/disjuntor.py:501`, `bin/disjuntor.py:513`, `bin/disjuntor.py:1367`, `bin/disjuntor.py:1370`, `bin/disjuntor.py:1406`)
- **Identidade incorreta alimentando a decisão:** chamadas concorrentes compartilham resposta/sidecar sem trava por caminho; `mtime` continua sendo o critério de resposta nova. Persistem #11 e #13. (`bin/codex_pergunta.sh:165`, `bin/codex_pergunta.sh:166`, `bin/codex_pergunta.sh:187`, `bin/codex_pergunta.sh:188`)

**(c) Sim — PROCEDENTE, BLOQUEIA.**

- **Autorização sem ler a triagem:** `_triagens=None` registra FALHA e preserva o cache, mas não torna a porta “não sei”. Sem disparo anterior, com cache vazio ou inferior a três, a decisão retorna livre; `b3_parada` devolve zero e o wrapper prossegue. (`bin/disjuntor.py:861`, `bin/disjuntor.py:864`, `bin/disjuntor.py:882`, `bin/disjuntor.py:898`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`)
- **Fonte ausente tomada como vazia:** sem contador novo, ausência vira `{}`, mesmo com contagem herdada conhecida; estado ausente também vira `{}`. (`bin/disjuntor.py:263`, `bin/disjuntor.py:297`, `bin/disjuntor.py:811`, `bin/disjuntor.py:816`)
- **Painel com zeros sem leitura:** disparos, negações e liberações começam zerados; falha de abertura e linhas não parseadas são ignoradas; a apresentação imprime os zeros. Persiste #15. (`bin/disjuntor.py:1450`, `bin/disjuntor.py:1452`, `bin/disjuntor.py:1460`, `bin/disjuntor.py:1464`, `bin/disjuntor.py:1482`, `bin/disjuntor.py:1582`)
- **Rc mascarado pelo hook:** a exceção é registrada, mas `main_pretool` retorna zero sem emitir negativa. No wrapper, especificamente, rc não zero de `--b3-parada` é recusado; o falso verde acima chega como rc zero. (`bin/disjuntor.py:1406`, `bin/disjuntor.py:1408`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`)

## 31/10 15:47 · sessao 0f4e6dd6efbc · - · - · origem=Ronald (tmux)

**#8 e #9: cura parcial; PROCEDENTE, BLOQUEIA na migração de 8609b12c.** O estado antigo não possui `b3_triagem_linhas`; o novo leitor assume zero, aceita trilha ausente como `{}` e recalcula `codex=[]`. Com obra disparada, isso permite liberar arquivando lista vazia; restaurar as mesmas triagens dispara novamente. Uma trilha que perdeu linhas completas também passa nessa primeira leitura sem watermark. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:822`, `bin/disjuntor.py:883`, `bin/disjuntor.py:913`, `bin/disjuntor.py:888`)

Reproduzi o primeiro cenário exclusivamente em memória: estado herdado disparado com `[a,b,c]`, fonte ausente e ERRO-BOBO válido resultaram em `{}` e `liberado=[]`; restaurar `[a,b,c]` resultou em nova parada. O selo existente estabelece o watermark por `_xt` antes de retirar/corromper a fonte; não cobre essa migração. (`bin/tests/test_disjuntor.sh:1299`, `bin/tests/test_disjuntor.sh:1313`, `bin/tests/test_disjuntor.sh:1324`)

**VERDE sem leitura: PROCEDENTE, BLOQUEIA.** Mesmo quando `_triagens` devolve `None`, `decide_b3` preserva o cache e somente impede liberações de obras já disparadas. Sem disparo anterior e com cache inferior a três, devolve `{}`; `b3_parada` retorna zero e o wrapper segue para Codex. (`bin/disjuntor.py:860`, `bin/disjuntor.py:882`, `bin/disjuntor.py:888`, `bin/disjuntor.py:898`, `bin/disjuntor.py:989`, `bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:166`)

| Campo do estado | Escritor(es) lógico(s) | Leitor(es) |
|---|---|---|
| `b1[agente].checado/disparado/motivo` | `b1_do_agente` (`bin/disjuntor.py:481`) | Cache/decisão (`bin/disjuntor.py:476`); disparados (`bin/disjuntor.py:490`) |
| `b2.ultimo` | `b2_decide` (`bin/disjuntor.py:1134`) | Mesmo (`bin/disjuntor.py:1133`) |
| `b4_para.desde` | `b4_stop`, incluindo remoção (`bin/disjuntor.py:1155`, `bin/disjuntor.py:1149`) | Mesmo (`bin/disjuntor.py:1157`) |
| `b4.negou_desde` | `pretool`, incluindo remoção (`bin/disjuntor.py:1379`, `bin/disjuntor.py:1374`) | Mesmo (`bin/disjuntor.py:1373`) |
| `b3_limiar` | `_migra_limiar` (`bin/disjuntor.py:710`) | Mesmo (`bin/disjuntor.py:699`) |
| `b3_sem_obra` | `decide_b3` (`bin/disjuntor.py:871`) | `_b3_sem_obra` (`bin/disjuntor.py:1518`) |
| `b3[obra].reprovados` | Migração, decisão e rearme (`bin/disjuntor.py:709`, `bin/disjuntor.py:877`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Mesmos (`bin/disjuntor.py:705`, `bin/disjuntor.py:886`, `bin/disjuntor.py:955`) |
| `b3[obra].disparado` | Decisão e rearme (`bin/disjuntor.py:889`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Migração/decisão (`bin/disjuntor.py:702`, `bin/disjuntor.py:898`) |
| `b3[obra].codex` | Decisão e rearme (`bin/disjuntor.py:883`, `bin/disjuntor.py:914`, `bin/disjuntor.py:959`) | Decisão/rearme (`bin/disjuntor.py:887`, `bin/disjuntor.py:955`) |
| `b3[obra].rearmado` | Migração e rearme (`bin/disjuntor.py:708`, `bin/disjuntor.py:957`) | Decisão (`bin/disjuntor.py:874`, `bin/disjuntor.py:881`) |
| `b3[obra].liberado` | Decisão (`bin/disjuntor.py:912`) | Decisão (`bin/disjuntor.py:874`, `bin/disjuntor.py:881`) |
| `b3[obra].consumidas` | Decisão (`bin/disjuntor.py:915`) | Decisão (`bin/disjuntor.py:908`) |
| `b3[obra].disparado_em/topo_no_disparo` | `_fotografa` (`bin/disjuntor.py:727`, `bin/disjuntor.py:730`) | Migração/fotografia/decisão (`bin/disjuntor.py:703`, `bin/disjuntor.py:728`, `bin/disjuntor.py:903`, `bin/disjuntor.py:910`) |
| `b3_triagem_linhas` | `_triagens` **e** `triagem` (`bin/disjuntor.py:830`, `bin/disjuntor.py:797`) | Ambos (`bin/disjuntor.py:811`, `bin/disjuntor.py:797`) |

**Há múltiplos escritores lógicos; não há escritor físico adicional identificado nesses caminhos. Não-bloqueia quanto à trava:** todo JSON passa por `_com_estado`, que toma `flock`, grava temporário e substitui o arquivo; migração/fotografia/rearme permanecem nessa trava. (`bin/disjuntor.py:249`, `bin/disjuntor.py:253`, `bin/disjuntor.py:274`, `bin/disjuntor.py:866`, `bin/disjuntor.py:921`, `bin/disjuntor.py:935`, `bin/disjuntor.py:971`)

**Trilha de triagem:** timestamp, obra, volta e veredito têm escritor físico único `triagem`, agora sob a mesma trava. `_triagens` lê os quatro campos; timestamp é apenas separado, sem validação/uso; obra e volta formam o mapa e o último veredito prevalece. `_procedentes`, decisão, rearme e idempotência consomem esse mapa. (`bin/disjuntor.py:786`, `bin/disjuntor.py:794`, `bin/disjuntor.py:800`, `bin/disjuntor.py:813`, `bin/disjuntor.py:826`, `bin/disjuntor.py:829`, `bin/disjuntor.py:836`, `bin/disjuntor.py:883`, `bin/disjuntor.py:958`, `bin/disjuntor.py:790`)

**Sidecar de identidade:** `obra/papel/hash/pid/fim/sha` são escritos pelo wrapper e lidos por `_le_id`/`triagem`; a validação estrutural não comprova autoria, preservando o achado #14. (`bin/codex_pergunta.sh:188`, `bin/disjuntor.py:757`, `bin/disjuntor.py:760`, `bin/disjuntor.py:780`, `bin/disjuntor.py:782`, `bin/disjuntor.py:784`)

**Outros caminhos persistentes:** exceção no PreToolUse libera ferramenta; classificação textual pode pular a guarda Opus; o painel apresenta zeros quando não consegue ler a trilha. (`bin/disjuntor.py:1406`, `bin/disjuntor.py:501`, `bin/disjuntor.py:1367`, `bin/disjuntor.py:1450`, `bin/disjuntor.py:1482`, `bin/disjuntor.py:1578`)

## 31/10 15:47 · sessao 6dcf031be48f · - · - · origem=Ronald (tmux)

Conclusão: os nove casos novos mordem estaticamente os respectivos percursos da base, mas #8 e #9 continuam **PROCEDENTES, BLOQUEIA** na passagem de estado legítimo de `8609b12c` para a versão nova; os testes sempre criam o novo contador antes de provocar a perda da fonte. (`bin/tests/test_disjuntor.sh:1299`, `bin/tests/test_disjuntor.sh:1324`, `bin/disjuntor.py:811`)

| Achado | Cura nominal e selo |
|---|---|
| #1 | Curado: porta chama decisão; caso usa wrapper real como primeira operação após terceira triagem. (`bin/disjuntor.py:982`, `bin/tests/test_disjuntor.sh:1224`) |
| #2 | Curado: mesma decisão lê liberação; caso chama wrapper após DESENHO sem decisão intermediária. (`bin/disjuntor.py:906`, `bin/tests/test_disjuntor.sh:1235`) |
| #3 | Curados os cenários relatados: `shlex`, última atribuição, valor de outra variável e obra com espaços; selo os distingue. (`bin/disjuntor.py:1254`, `bin/disjuntor.py:1261`, `bin/tests/test_disjuntor.sh:1261`) |
| #6 | Curado com trilha íntegra: liberação/rearme arquivam todas as identidades triadas; caso reclassifica antes do reset e depois adiciona duas novas. (`bin/disjuntor.py:913`, `bin/disjuntor.py:958`, `bin/tests/test_disjuntor.sh:1273`) |
| #7 | Curado: rearme cria registro para obra só com triagens; selo chama rearme antes de qualquer decisão. (`bin/disjuntor.py:951`, `bin/tests/test_disjuntor.sh:1291`) |
| #8 | Parcial: ausência preserva estado somente quando novo contador já é positivo. Caso cobre essa condição. (`bin/disjuntor.py:816`, `bin/tests/test_disjuntor.sh:1302`) |
| #9 | Parcial: rejeita truncamento, corrupção estrutural e redução abaixo do contador novo. Caso cobre essas condições. (`bin/disjuntor.py:819`, `bin/disjuntor.py:822`, `bin/disjuntor.py:828`, `bin/tests/test_disjuntor.sh:1314`) |
| #10 | Curado: leitura/idempotência/append sob `_com_estado`; teste mantém a trava tomada e exige recusa sem append. (`bin/disjuntor.py:786`, `bin/disjuntor.py:800`, `bin/tests/test_disjuntor.sh:1344`) |
| #12 | Curado: remoção após porta, imediatamente antes do Codex, e existência restante recusa chamada; selo testa preservação e sidecar preso. (`bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:165`, `bin/tests/test_disjuntor.sh:1245`, `bin/tests/test_disjuntor.sh:1249`) |

Resíduo #8: estado herdado com `codex=[a,b,c]`, obra disparada e sem `b3_triagem_linhas`; trilha ausente é interpretada como `{}`. A lista conhecida é apagada, liberação válida arquiva lista vazia e as três voltas podem reaparecer quando a trilha retorna. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:883`, `bin/disjuntor.py:913`)

Resíduo #9: sem o novo contador, uma trilha herdada reduzida de três para duas linhas é aceita, substitui a lista conhecida e não gera FALHA; o teste semeia contador pela implementação nova antes de reduzir. (`bin/disjuntor.py:822`, `bin/disjuntor.py:830`, `bin/disjuntor.py:883`, `bin/tests/test_disjuntor.sh:1324`)

Esses dois resíduos foram reproduzidos apenas em memória, substituindo acesso a arquivos e persistência; nenhuma execução integral do selo foi feita, pois ele cria e remove arquivos. Resultado integral do selo: **[não verificado]**. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`)

Limite adicional do selo: `_xp` detecta somente presença do recado; os negativos de reset/rearme e corrupção podem passar por falha aberta. Os subcasos de corrupção só exigem FALHA explicitamente no último cenário. (`bin/tests/test_disjuntor.sh:1203`, `bin/tests/test_disjuntor.sh:1277`, `bin/tests/test_disjuntor.sh:1292`, `bin/tests/test_disjuntor.sh:1315`, `bin/tests/test_disjuntor.sh:1338`)

## 31/10 15:46 · sessao 528ea250a67d · - · - · origem=Ronald (tmux)

- **#1, #2, #3, #12: curados nos cenários originais; não PROCEDENTE, não-bloqueia.** A porta chama a decisão atual, o parser usa `shlex` e a última atribuição, e o sidecar só é removido depois da guarda, com conferência da remoção. Os casos e mutantes correspondentes mordem por inspeção; execução **[não verificado]**. (`bin/disjuntor.py:982`, `bin/disjuntor.py:1254`, `bin/disjuntor.py:1261`; `bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:165`; `bin/tests/test_disjuntor.sh:1216`, `bin/tests/test_disjuntor.sh:1229`, `bin/tests/test_disjuntor.sh:1239`, `bin/tests/test_disjuntor.sh:1254`, `bin/tests/test_disjuntor.sh:1457`)

- **Trilha desconhecida ainda permite chamada: PROCEDENTE, BLOQUEIA.** Três triagens podem existir sem decisão B3 anterior. Se a trilha ficar ilegível/truncada, `_triagens` devolve `None`; a decisão registra FALHA, usa `{}` para descobrir obras e mantém somente o estado anteriormente conhecido. Sem registro da obra, retorna vazio; `b3_parada` retorna zero e o wrapper chama Codex. (`bin/disjuntor.py:817`, `bin/disjuntor.py:820`, `bin/disjuntor.py:861`, `bin/disjuntor.py:864`, `bin/disjuntor.py:878`, `bin/disjuntor.py:898`, `bin/disjuntor.py:982`, `bin/disjuntor.py:989`; `bin/codex_pergunta.sh:54`, `bin/codex_pergunta.sh:166`)

- **#8 incompleto na atualização do estado antigo: PROCEDENTE, BLOQUEIA.** O contador novo ausente vale zero; trilha ausente então vira `{}`, ainda que o JSON contenha `b3.codex` conhecido. A decisão pode apagar essa lista e liberar sem arquivar suas identidades. O caso novo cria suas triagens na versão nova e não cobre estado herdado sem contador. (`bin/disjuntor.py:811`, `bin/disjuntor.py:816`, `bin/disjuntor.py:883`, `bin/disjuntor.py:912`; `bin/tests/test_disjuntor.sh:1299`)

- **Concorrência entre guarda e chamada: PROCEDENTE, BLOQUEIA.** O wrapper consulta B3 antes de copiar dados e esperar pelos slots. Outra decisão pode parar a obra durante essa espera; não existe rechecagem antes de `codex exec`. (`bin/codex_pergunta.sh:53`, `bin/codex_pergunta.sh:146`, `bin/codex_pergunta.sh:158`, `bin/codex_pergunta.sh:166`)

- **Persistem os caminhos de falha aberta e classificação textual do Opus: PROCEDENTE, BLOQUEIA.** Exceção no PreToolUse libera a ferramenta; a expressão “nao e a leitura final”, mesmo citada, faz `_eh_final` ser falso e pula a guarda. (`bin/disjuntor.py:114`, `bin/disjuntor.py:501`, `bin/disjuntor.py:1367`, `bin/disjuntor.py:1406`)

- **Persistem identidade concorrente, `mtime` e autoria apenas estrutural: PROCEDENTE, BLOQUEIA para o contrato literal da identidade.** Resposta e sidecar compartilhados não têm trava por caminho; a identidade depende de `-nt`; os campos são aceitos com SHA correto, sem comprovação de emissão pelo wrapper. (`bin/codex_pergunta.sh:166`, `bin/codex_pergunta.sh:187`, `bin/codex_pergunta.sh:188`; `bin/disjuntor.py:757`, `bin/disjuntor.py:760`, `bin/disjuntor.py:782`)

- **Painel verde sem leitura: PROCEDENTE, BLOQUEIA.** Os contadores começam zerados, `OSError` da trilha é ignorado e a apresentação imprime esses zeros; estado ausente também produz zero em `b3_sem_obra`. (`bin/disjuntor.py:1450`, `bin/disjuntor.py:1452`, `bin/disjuntor.py:1482`, `bin/disjuntor.py:297`, `bin/disjuntor.py:1518`, `bin/disjuntor.py:1582`)

## 31/10 15:35 · sessao 3e6557bd95cb · ODOMETRO-DE-REGRAS · leitura · origem=Code

**(a) Não nos dois selos lidos.** Eles cobrem outras propriedades:

- `test_lei_akita.sh`: presença/contagem da LEI-AKITA, teto de `fallback`, exigência no molde e `padrao=` nos commits; lê o CLAUDE.md do disco. (`bin/tests/test_lei_akita.sh:28`, `bin/tests/test_lei_akita.sh:35`, `bin/tests/test_lei_akita.sh:45`, `bin/tests/test_lei_akita.sh:70`)
- O arquivo encontrado é `test_claude_md_regras_preservadas.sh`: preserva nomes, ponteiros e blocos literais de PISTA; também lê o disco. Não mede bytes, emendas com alvo vivo ou predicados repetidos. (`bin/tests/test_claude_md_regras_preservadas.sh:55`, `bin/tests/test_claude_md_regras_preservadas.sh:83`)
- A régua já descobre `test_*.sh` e barra por retorno diferente de zero; isso fornece a chamada, mas não as novas verificações. (`bin/regua.sh:138`, `bin/regua.sh:140`)
- Não localizei outro selo com essas três cobranças; ausência global não demonstrada por execução. **[nao verificado]**

**(b) Bytes e delimitadores são determinísticos; “alvo de pé” e “mesmo predicado” ainda exigem julgamento.** Medindo os bytes brutos desde o título da 7b, incluindo-o, até antes do título da 8, obtive **22.645 B: VERDE por tamanho**. (`CLAUDE.md:448`, `CLAUDE.md:669`) [nao verificado]

Três exemplos, como classificação de leitura — não veredito de um selo existente:

| Bloco/caso | Cor pela interpretação indicada | Ambiguidade que precisa desaparecer |
|---|---|---|
| **AVAL-DE-CRITERIO — “ALTERA a L-009”** | **VERMELHO**, se L-009 resolve para seu bloco PRE-APROVADO x NUNCA PRE-APROVADO, ainda presente. | Essa associação exige consultar o índice: o bloco original não traz `L-009` no cabeçalho. Uma busca literal apenas na 7b poderia dar VERDE indevido. (`CLAUDE.md:467`, `CLAUDE.md:576`, `app/docs/LEIS.md:67`) |
| **PRE-APROVADO + DEPLOY JA** | **VERMELHO**, interpretando “dinheiro exige `!`” como o mesmo predicado nos dois blocos. | DINHEIRO-EM-COMPETENCIA-ABERTA depois restringe essa exigência. É preciso definir se texto superado ainda conta e distinguir repetição de contradição. (`CLAUDE.md:473`, `CLAUDE.md:591`, `CLAUDE.md:558`, `CLAUDE.md:573`) | [nao verificado]
| **PROMPT-NAO-SE-REPETE** | **VERDE** quanto a emenda com alvo vivo e duplicação de parada/dinheiro. | “NAO refazer” e “NO MESMO turno” tratam de recepção de prompt; palavras isoladas não identificam predicado de parada. (`CLAUDE.md:602`) | [nao verificado]

Também falta delimitar o universo: PAREI-DE-LEI declara alterar PAREI-SO-LEI, cujo bloco permanece, mas ambos estão **fora da 7b**. Um examinador restrito ao recorte não os alcança. (`CLAUDE.md:72`, `CLAUDE.md:79`, `CLAUDE.md:448`)

**(c) O contrato precisa fechar estes pontos antes da construção:**

- **Quem chama:** definir como o selo recebe o SHA e qual commit a régua usa quando não há push. Hoje o loop não passa argumentos; o hook já calcula `PUSH_SHA`. Definir também posição diante dos atalhos: a régua retorna “JA VERDE” antes dos selos e `SKIP_TESTS=1` encerra o hook na entrada. (`bin/regua.sh:84`, `bin/regua.sh:138`, `bin/pre-push.sh:5`, `bin/pre-push.sh:33`)
- **Quem espera / placar:** especificar onde aparecem os três números e como uma falha invalida o verde anterior. Hoje a régua descarta a saída dos selos aprovados e, na falha de selo, sai antes de atualizar o carimbo. (`bin/regua.sh:140`, `bin/regua.sh:143`, `bin/regua.sh:196`)
- **Falha / limite:** definir formalmente início/fim de bloco, identidade/aliases de X, significado de “de pé”, referências versus regras operativas e equivalência dos predicados. A redação atual contém alteração parcial e regra explicitamente remetida à história; presença textual não resolve vigência. Também falta declarar tratamento de falha do `git show`, títulos duplicados e recorte malformado. (`CLAUDE.md:537`, `CLAUDE.md:557`, `CLAUDE.md:576`; tratamento proposto **[nao verificado]**)
- **Medida:** definir se N conta blocos, pares ou predicados distintos, imprimindo os envolvidos e suas linhas. Nos mutantes, exigir recusa pela causa esperada, usando o mesmo juiz, mais uma base válida que passe. Acrescentaria prova de commit diferente do disco, recorte ausente e fronteira 25.000/25.001 B. O selo de preservação já reutiliza `_julgar`, mas isso não prova o novo odômetro. (`bin/tests/test_claude_md_regras_preservadas.sh:83`, `bin/tests/test_claude_md_regras_preservadas.sh:94`; novos casos **[nao verificado]**)
- **Quem desliga:** “remover a chamada da régua” precisa ser concretizado, pois a descoberta ocorre por glob. Para pausa, nomear arquivo, consumidor, escopo, retorno durante pausa e condição de reativação; `pausar.sh` registra os campos, mas o selo precisa lê-los. (`bin/regua.sh:138`, `bin/pausar.sh:31`, `bin/pausar.sh:35`, `bin/pausar.sh:39`)

O pouso próprio do instrumento está previsto na L-105; o SHA que comprovará o pouso anterior da CLAUDE-MD-RACIONAL ainda precisa ser identificado. (`CLAUDE.md:623`; pouso da CLAUDE-MD-RACIONAL **[nao verificado]**) [nao verificado]

## 31/10 15:34 · sessao 3c92a2b0ed4d · - · - · origem=Ronald (tmux)

- Nos dois selos examinados, não há odômetro: `test_lei_akita.sh` cobra seção/13 linhas, teto de `fallback`, molde e `padrao=` dos commits; `test_claude_md_regras_preservadas.sh` cobra conjuntos de nomes, ponteiros e hashes de blocos de PISTA. Ambos leem arquivos do disco; o `git show` comentado no segundo descreve geração histórica das referências, não leitura do SHA do push. (`bin/tests/test_lei_akita.sh:28`, `bin/tests/test_lei_akita.sh:35`, `bin/tests/test_lei_akita.sh:45`, `bin/tests/test_lei_akita.sh:70`, `bin/tests/test_claude_md_regras_preservadas.sh:9`, `bin/tests/test_claude_md_regras_preservadas.sh:55`, `bin/tests/test_claude_md_regras_preservadas.sh:83`)
- A busca em `bin/tests` não localizou cobrança de teto de bytes da 7b, emenda com alvo vivo ou repetição semântica de parada/dinheiro; ausência global é evidência de busca, não prova executada. `[nao verificado]`
- **Chamada:** o loop real da régua é `tests/test_*.sh`, sem argumento de SHA; o hook determina `PUSH_SHA` na linha 33. Falta definir interface comum (`--sha`/variável), SHA adotado pela régua sem push e política para ausência de SHA/deleção. (`bin/regua.sh:138`, `bin/regua.sh:140`, `bin/pre-push.sh:33`, `bin/pre-push.sh:34`)
- **Atalhos:** a régua retorna “JA VERDE” antes do loop de selos; o pre-push tem `SKIP_TESTS=1` antes de qualquer cobrança e pula arsenal com carimbo. O contrato precisa posicionar a cobrança antes dos atalhos relevantes ou explicitar seu efeito. (`bin/regua.sh:84`, `bin/regua.sh:93`, `bin/regua.sh:138`, `bin/pre-push.sh:5`, `bin/pre-push.sh:62`, `bin/pre-push.sh:65`)
- **Espera/placar:** falha de selo faz a régua imprimir vermelho e sair, mas o ramo não grava `REGUA_STATUS=FALHOU`; o carimbo só é gravado depois da suite. Saída verde do selo também é capturada e descartada pelo loop. Para o odômetro aparecer no placar, faltam propagação de medidas e atualização/invalidação do carimbo na falha. (`bin/regua.sh:140`, `bin/regua.sh:143`, `bin/regua.sh:145`, `bin/regua.sh:196`)
- **Pausa/desliga:** `pausar.sh` exige caminho, dono, motivo e condição de saída; `COBRE` é opcional e quem lê a pausa são os selos. Falta nomear caminho da pausa do odômetro, consumidor, escopo e veredito durante pausa; executar `pausar.sh` sozinho não conecta a pausa à cobrança. (`bin/pausar.sh:31`, `bin/pausar.sh:32`, `bin/pausar.sh:33`, `bin/pausar.sh:34`, `bin/pausar.sh:35`, `bin/pausar.sh:37`)
- **Remoção:** não existe chamada individual do novo selo na régua para “remover”: ela o descobriria pelo glob. Remover somente uma chamada explícita do hook deixaria a cobrança na régua; o contrato deve definir remoção do arquivo/registro no loop sem allowlist. (`bin/regua.sh:138`, `bin/regua.sh:140`)

## 31/10 15:10 · sessao f841df73dc38 · LEI-AKITA-UNIVERSO · verificador · origem=Code

**a) IMPROCEDENTE — não encontrei leitura de conteúdo julgado no worktree.** CLAUDE, fabricante, molde e vocabulário vêm de `git show ALVO:path`; relatórios vêm de `git grep ALVO`. Os `open` restantes **escrevem fixtures**, e `os.path` apenas compõe seus caminhos. Sem contraexemplo de leitura residual de dados do disco. (`bin/tests/test_lei_akita.sh:22`, `:24`, `:42`, `:73`, `:92`, `:99`, `:194`, `:197`, `:238`, `:239`, `:247`)

**b) PROCEDENTE quanto à falta de isolamento geral; a herança específica do SHA foi corrigida.** A chamada sobrescreve `LEI_AKITA_HEAD`, e MORDE força `ALVO=HEAD`; porém `R` continua vindo do chamador e os subprocessos herdam o restante do ambiente, sem `env=` isolado. (`bin/tests/test_lei_akita.sh:17`, `:71`, `:322`, `:192`, `:232`)

Contraexemplo: exportar `GIT_DIR` apontando outro repositório permite que os comandos sintéticos operem nesse repositório apesar de `git -C <temporário>`. **Não executei esse cenário**, pois envolveria escritas. [não verificado] [nao verificado]

Não encontrei aprovação acidental por MORDE vazio: há verificações de vocabulário, resultados esperados, contagens e sentinela. O caso sincronizado aceita vazio intencionalmente. (`bin/tests/test_lei_akita.sh:185`, `:199`, `:224`, `:264`, `:284`, `:289`, `:301`, `:323`)

**c) PROCEDENTE — o corte ainda depende do HEAD do checkout.** O `git log -S` não recebe `ALVO`; seu resultado determina uma exclusão adicional no `rev-list`. (`bin/tests/test_lei_akita.sh:158`, `:163`, `:167`)

Contraexemplo concreto: histórico linear `O(origin/main) → B(padrão inventado) → T(ALVO) → C(HEAD, primeira inserção do marcador)`. O corte será `C`, seu pai será `T`, e a enumeração será `rev-list T ^O ^T`: **universo vazio**, omitindo `B`. (`bin/tests/test_lei_akita.sh:158`, `:163`, `:167`, `:313`)

Mesmo com HEAD correto, esse piso contradiz o contrato literal `origin/main..<commit>`: o próprio MORDE exige excluir um commit ruim anterior ao corte ainda não publicado. (`bin/tests/test_lei_akita.sh:244`, `:297`, `:299`, `:301`)

**d) PROCEDENTE — existem erros Git silenciados.** `_show` transforma falha em `OSError`, mas `vocabulario` a descarta nos dois documentos. (`bin/tests/test_lei_akita.sh:74`, `:96`, `:106`)

Contraexemplo: ALVO remove ambos os documentos de vocabulário num commit apenas de documentação, com `padrao=n/a (instrumento)`; HEAD os restaura. Os dois `git show ALVO:...` falham, mas `n/a` passa sem consultar o vocabulário, enquanto MORDE consulta o HEAD restaurado. Mantidas as demais condições, existe caminho até `OK`. (`bin/tests/test_lei_akita.sh:71`, `:117`, `:130`, `:132`, `:314`, `:323`, `:326`, `:328`)

Também há sondagens de pai que ignoram qualquer rc, podendo trocar a comparação contra o primeiro pai por `git show` sem diagnóstico. (`bin/tests/test_lei_akita.sh:163`, `:175`, `:177`, `:178`)

**SHA totalmente inexistente, entretanto, não passa silenciosamente**: a enumeração é estrita e a ausência de `PADRAO_FIM` reprova. (`bin/tests/test_lei_akita.sh:152`, `:167`, `:319`)

## 31/10 15:10 · sessao 0bd03fc57462 · - · - · origem=Ronald (tmux)

- **a) IMPROCEDENTE quanto a dados do worktree.** Leituras de conteúdo passam por `git show ALVO:path` (22–24, 73, 92, 99) ou `git grep ALVO` (42, 45). `open` nas linhas 194, 197, 239 e 247 escreve fixtures; `os.path` nas linhas 238–239 compõe caminhos. Referências: `bin/tests/test_lei_akita.sh`.

- **c) PROCEDENTE.** `bin/tests/test_lei_akita.sh:158` procura o corte com `git log` sem `head`/ALVO, portanto parte do HEAD do checkout. O corte contamina `pisos` na linha 163 e a enumeração na linha 167. Contraexemplo: histórico linear `O(origin/main) → B(padrão inventado) → T(ALVO) → C(HEAD, primeira inserção PADRAO-NA-CURA)`. O selo calcula `corte=C`, `pisos=['^T']` e executa `rev-list T ^O ^T`: universo vazio, `PADRAO_JULGADOS 0 de 0` e `PADRAO_FIM` (310–314). Contraprova adicional: HEAD numa história sem marcador causa erro de corte mesmo que ALVO tenha corte e universo válidos (158–160).

- **Contrato do universo: PROCEDENTE independentemente do HEAD.** Mesmo com HEAD=ALVO, `bin/tests/test_lei_akita.sh:163` e `:167` acrescentam exclusão além de `^origin/main`. O próprio MORDE exige excluir commit ruim anterior ao corte (`:244`, `:297`–`:302`). Isso contradiz o universo literal informado pelo usuário, `origin/main..<commit julgado>`.

- **d) PROCEDENTE.** `_show` converte rc!=0 em `OSError` (`bin/tests/test_lei_akita.sh:73`–`:75`), mas `vocabulario` engole esse erro (`:96`–`:97`, `:106`–`:107`). Contraexemplo concreto dedutível: ALVO=T remove ambos os documentos de vocabulário em commit só de `app/docs/`, com `padrao=n/a (instrumento)`; HEAD=U restaura os documentos. Os dois `git show T:app/docs/...` falham; VOCAB fica vazio, mas `n/a` é aceito sem consultar VOCAB (`:130`–`:132`), e o juiz emite FIM (`:314`). O MORDE lê HEAD=U (`:71`) e pode passar. Mantidos CLAUDE/molde/fallback válidos, o selo completo pode retornar OK apesar desses erros Git (`:326`–`:328`).

O repositório real `/home/ronald/saas-hasner` contém o objeto 43ce4ca4; confirmei por leitura que o commit inteiro altera somente `bin/tests/test_lei_akita.sh`. [não verificado por arquivo:linha — evidência obtida em `git show --stat` e `git diff`, sem execução do selo]

## 31/10 15:09 · sessao e739a561fc30 · - · - · origem=Ronald (tmux)

No snapshot auditado:

- **MORDE não herda `LEI_AKITA_HEAD` efetivo:** chamada sobrescreve com SHA inválido em `bin/tests/test_lei_akita.sh:322`; Python usa `HEAD` quando MORDE em `bin/tests/test_lei_akita.sh:71`.
- **Não existe isolamento geral do ambiente:** `subprocess.run` não recebe `env=` em `bin/tests/test_lei_akita.sh:73`, `:151`, `:192`, `:232`, `:292`. Contraexemplo: `GIT_DIR`/`GIT_WORK_TREE` exportados continuam selecionando o repositório real mesmo com `git -C <temporário>`; os casos sintéticos podem operar fora do temporário. Execução desse contraexemplo não realizada, pois escreveria arquivos. [não verificado]
- **Não identifiquei aprovação por MORDE vazio:** exige vocabulário essencial (`bin/tests/test_lei_akita.sh:185`), termo commitado (`:199`), divergência dos casos (`:224`), contagens de universo (`:264`, `:284`, `:289`, `:301`) e sentinela sem falhas (`:323`).
- **Falhas Git são abafadas no vocabulário:** `_show` converte rc não zero em `OSError` (`bin/tests/test_lei_akita.sh:74`); `vocabulario` engole em `:96` e `:106`. Contraexemplo: ALVO sem PLANO-PADROES, mas com GLOSSARIO contendo `juiz unico`; a leitura ausente pode ser ignorada e esse padrão aprovado (`:135`), enquanto MORDE lê o HEAD completo (`:71`, `:117`).
- **Outros rc ignorados:** sondagens de pai com `estrito=False` em `bin/tests/test_lei_akita.sh:163` e `:175` tratam qualquer falha como ausência de pai; isso altera piso ou comparação sem diagnóstico (`:177`).
- **SHA completamente inexistente não fica verde:** falhas dos estáticos em `bin/tests/test_lei_akita.sh:35`, `:37`, `:38`, `:52`, `:53`; enumeração estrita em `:152`, `:167`; sentinela obrigatória em `:319`.
- **Universo adicionalmente restrito:** piso em `bin/tests/test_lei_akita.sh:163` acrescenta exclusão ao `origin/main..<head>` em `:167`; o próprio MORDE exige deixar de fora um commit anterior ao corte ainda não publicado (`:297`–`:302`).

## 31/10 15:01 · sessao b7922e87983c · E4-D5-PROD · sonda · origem=Code

| Empresa | Competência | Janela | entregue | celulas | lavradas | ate_ontem | Distribuição de status | Referência |
|---|---|---|---|---:|---:|---:|---|---|
| 1 | 10/2026 | 2026-09-21..2026-10-20 | False | 90 | 0 | 90 | `{None: 90}` | [nao verificado] |
| 1 | 11/2026 | 2026-10-21..2026-11-20 | False | 62 | 0 | 20 | `{None: 20}` | [nao verificado] |
| 2 | 10/2026 | 2026-09-21..2026-10-20 | False | 13340 | 0 | 13340 | `{None: 13340}` | [nao verificado] |
| 2 | 11/2026 | 2026-10-21..2026-11-20 | False | 13575 | 143 | 4303 | `{None: 4219, 'ausencia': 3, 'ok': 41, 'folga': 30, 'irregular': 9, 'escala_suspeita': 1}` | [nao verificado] |
| 3 | 10/2026 | 2026-09-21..2026-10-20 | False | 3538 | 0 | 3538 | `{None: 3538}` | [nao verificado] |
| 3 | 11/2026 | 2026-10-21..2026-11-20 | False | 3622 | 21 | 1165 | `{None: 1156, 'folga': 3, 'irregular': 3, 'ok': 3}` | [nao verificado] |
| 4 | 10/2026 | 2026-09-21..2026-10-20 | False | 666 | 0 | 666 | `{None: 666}` | [nao verificado] |
| 4 | 11/2026 | 2026-10-21..2026-11-20 | False | 713 | 4 | 230 | `{None: 228, 'folga': 2}` | [nao verificado] |

DRY:

| Empresa | Competência | ja_lavrado | completaveis | completados | fila_cartorio | Referência |
|---|---|---:|---:|---:|---:|---|
| 1 | 10/2026 | 0 | 0 | 0 | 90 | [nao verificado] |
| 1 | 11/2026 | 0 | 0 | 0 | 20 | [nao verificado] |
| 2 | 10/2026 | 0 | 13340 | 0 | 0 | [nao verificado] |
| 2 | 11/2026 | 84 | 4145 | 0 | 74 | [nao verificado] |
| 3 | 10/2026 | 0 | 3538 | 0 | 0 | [nao verificado] |
| 3 | 11/2026 | 9 | 1129 | 0 | 27 | [nao verificado] |
| 4 | 10/2026 | 0 | 666 | 0 | 0 | [nao verificado] |
| 4 | 11/2026 | 2 | 221 | 0 | 7 | [nao verificado] |

**ERROS=0** [nao verificado]

## 31/10 15:00 · sessao e828f50b15de · V3-RETOMA · verificador · origem=Code

**BLOQUEIA — a HE principal e seu split noturno podem usar CCTs de datas diferentes.** A principal recebe a CCT diária, mas o split conserva as extras calculadas pelo motor da fatia (`app/ponto/calculador/alimentacao.py:514`, `:535`; `app/ponto/services/fechamento.py:711`).

1. **BLOQUEIA — HE100 noturna positiva com HE100 total zero.** O fechamento escolhe a CCT em `fatia_ini`; o motor grava a dobra nos períodos; a alimentação copia esses minutos para `he_pontas`, sem reconciliá-los com a nova CCT diária (`app/ponto/services/fechamento.py:711`; `app/ponto/motor_calculo_v2.py:2747`; `app/ponto/calculador/alimentacao.py:92–96`).

   Se a CCT de dobra vencer dentro da mesma fatia, um feriado posterior pode perder a HE100 principal e conservar sua parcela noturna. Contraprova em memória com as funções da cópia: plantão 23h–05h, trabalhado=previsto=360, `feriado_em_dobra=False`, `he_pontas=[(entrada, saída, 0, 360)]` resulta em **HE100=0 e HE100 noturna=6** (`app/ponto/calculador/regras.py:769`, `:775–810`, `:822–834`). Ocorrência em produção: **[nao verificado]**.

   Os testes novos não exercitam essa composição: praça/vigência usam períodos falsos com extras zeradas e verificam somente HE100 de feriado; o teste noturno injeta os 360 minutos diretamente (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:41`, `:69–82`; `app/ponto/tests/test_v3_dobra_do_ciclo_do_dia.py:44–48`).

2. **NAO-BLOQUEIA — aumento de consultas sem cache equivalente nesse caminho.** Agora são **D chamadas de `get_motor_cct` por colaborador/janela**, sendo D os dias com minutos válidos, unidos aos dias bloqueados, dentro da janela. Não são automaticamente todos os dias civis do mês (`app/ponto/calculador/alimentacao.py:472–480`, `:514`). Antes eram **T chamadas**, pelos templates distintos, conforme o trecho removido do anexo (`delta_7e0f.diff`, hunk de `alimentacao.py:382`).

   Assim, com um template e 30 dias visitados, passa de **1 para 30** chamadas nessa porta. No fechamento completo, a contagem passa de **F+O+T para F+O+D**: F motores das fatias e O processamento adicional de órfãos (`app/ponto/services/fechamento.py:263`, `:711`, `:793`, `:1239`). Contagens reais por colaborador/mês e impacto de latência: **[nao verificado]**.

   Cada chamada relê o switch; com ele ativo, recarrega vínculo e, para empresa CCT, consulta aplicações. As escalas/células já disponíveis na porta não são repassadas à fábrica (`app/core/regua_cct.py:444–458`, `:329`, `:366`; `app/escala/alimentacao.py:298–300`; `app/ponto/calculador/alimentacao.py:400`, `:426–438`). `_FONTE_DO_MOTOR` guarda somente metadados por instância; não evita essa resolução (`app/core/regua_cct.py:488–506`).

3. **NAO-BLOQUEIA — quatro testes passariam sem a cura, por inspeção estática:** [nao verificado]

   - `test_a_dobra_continua_paga`: verifica a atribuição que já ocorria antes do retorno removido (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45–46`; `app/ponto/calculador/regras.py:780–785`).
   - `test_MORDE_NACIONAL_dobra_no_intermitente`: o nacional já estava no conjunto entregue ao código antigo (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:120–122`).
   - `test_MORDE_dia_FORA_da_praca_nao_dobra`: o antigo `ini=21/09` antecede ambos os vínculos, iniciados em outubro; sem vínculo, não encontra a aplicação específica da praça e já produz zero (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:72`, `:171–185`; `app/core/regua_cct.py:329–330`, `:256–257`).
   - `test_MORDE_dia_DENTRO_da_vigencia_dobra`: a convenção da empresa já estava vigente em 21/09; a leitura antiga já concedia a dobra (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:72`, `:199–214`; `app/core/regua_cct.py:261–264`).

   Execução da suíte contra o código anterior: **[nao verificado]**. [nao verificado]

4. **NAO-BLOQUEIA — cobertura incompleta dos demais efeitos.** O caso novo trabalha 360 minutos contra 420 previstos; portanto não protege o `elif` contra soma indevida de HE50 quando houver excedente positivo. Também não cobre saída antecipada, HE50 noturna ou parâmetros noturnos mudando com praça/vigência (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:38`, `:69–82`; `app/ponto/calculador/regras.py:794`, `:831`, `:851–861`; `app/ponto/calculador/alimentacao.py:515–537`).

Os **chamadores de produção** são: `regras.do_dia` ← `rubricas_por_dia`, em `app/ponto/calculador/alimentacao.py:523`; `rubricas_por_dia` ← fechamento, em `app/ponto/services/fechamento.py:1239`, e comparador, em `app/ponto/management/commands/diff_calculador.py:417`. O fechamento entrega os valores à lavratura como versão **`oraculo`**, que substitui os campos correspondentes; os filtros de exportação examinados continuam selecionando `motor` (`app/ponto/services/fechamento.py:1256–1262`; `app/ponto/services/dia_pago.py:320–328`; `app/folha/porta_export.py:284`).

Quanto ao comportamento além dos asserts, retirar o retorno mantém a dobra substituindo o excedente e permite executar **ambos os splits noturnos, adicional noturno, atraso, saída antecipada e detalhes de cadastro incompatível** (`app/ponto/calculador/regras.py:779–861`). A seleção diária altera dobra, prorrogação pós-05h e hora reduzida conforme praça/vigência; a exclusão de pontualidade continua sendo lida da família do motor (`app/ponto/calculador/alimentacao.py:411–412`, `:514–538`; `app/core/regua_cct.py:261–262`, `:329–330`).

O calendário agora é o conjunto devolvido pelo motor: os motores comuns conservam o conjunto recebido; o intermitente o esvazia. **Uma data ausente do conjunto ainda consulta o calendário nacional**, mesmo quando o conjunto não está vazio (`app/ponto/motor_calculo_v2.py:740`, `:901–905`, `:3495`; `app/core/feriados.py:91–95`). Isso não recalcula feriados municipais pela praça histórica do dia: o fechamento continua montando o conjunto pelo posto do colaborador, e `folga_e_feriado` continua usando esse conjunto original (`app/ponto/services/fechamento.py:621–623`; `app/core/feriados.py:113–114`; `app/ponto/calculador/alimentacao.py:530–531`).

## 31/10 14:59 · sessao f17443fe0e70 · REPROVA-E-DESENHO-CODEX · verificador · origem=Code

**REPROVADO: o código cobre o fluxo sequencial básico, mas não fecha o contrato.** Há chamadas que passam sem atualizar o B3, bloqueios indevidos e voltas antigas que reaparecem após reset. (`bin/disjuntor.py:940`, `bin/disjuntor.py:1226`, `bin/disjuntor.py:852`, `bin/disjuntor.py:883`)

Execução do selo e ocorrência dos cenários em produção: **[nao verificado]**. Não executei o selo porque ele cria, altera e apaga arquivos. (`bin/tests/test_disjuntor.sh:39`, `bin/tests/test_disjuntor.sh:41`, `bin/tests/test_disjuntor.sh:51`)

| Item do contrato | Veredito sobre o código |
|---|---|
| **1. Três chamadas distintas PROCEDENTES por obra** | **Parcial.** O leitor deduplica por identidade, separa obras e usa limiar três; entretanto, o disparo depende de alguém executar `decide_b3`, e a porta direta do wrapper não executa essa decisão. (`bin/disjuntor.py:783`, `bin/disjuntor.py:805`, `bin/disjuntor.py:859`, `bin/disjuntor.py:940`) |
| **2. Não triada ou DESCARTADO não conta** | **Cumpre com a fonte íntegra.** Vale o último veredito; somente PROCEDENTES entram na lista recalculada. Falta ou corrupção da fonte tem os defeitos descritos abaixo. (`bin/disjuntor.py:794`, `bin/disjuntor.py:805`, `bin/disjuntor.py:811`, `bin/disjuntor.py:854`) |
| **3. Negar próximo Opus final e Codex verificador até liberação válida; outros papéis passam** | **Não cumpre integralmente.** Existem as duas guardas e o reconhecedor compartilhado, mas a porta direta consulta estado atrasado, o parser pode negar outro papel e o PreToolUse falha aberto. (`bin/disjuntor.py:625`, `bin/disjuntor.py:940`, `bin/disjuntor.py:1310`, `bin/disjuntor.py:1320`, `bin/disjuntor.py:1359`) |
| **4. Escritor único; identidade do wrapper; somente verificador** | **Parcial.** Há uma implementação de escrita da triagem e recusa estrutural de identidade ausente, papel diferente e SHA divergente. A escrita não participa da trava do estado, e os campos não comprovam autoria pelo wrapper. (`bin/disjuntor.py:752`, `bin/disjuntor.py:776`, `bin/disjuntor.py:788`) |
| **5. Liberação/rearme zeram e impedem recontagem das antigas** | **Não cumpre integralmente.** Arquiva as PROCEDENTES atuais; perde a identidade de voltas anteriormente contadas que estejam descartadas ou ausentes da fonte durante o reset. (`bin/disjuntor.py:854`, `bin/disjuntor.py:883`, `bin/disjuntor.py:921`) |

Os caminhos de falha encontrados são estes; os cenários descrevem consequências do código, sem afirmar ocorrência em produção. (`bin/disjuntor.py:770`, `bin/disjuntor.py:814`, `bin/disjuntor.py:940`)

1. **A quarta chamada direta pode passar após três PROCEDENTES.** Três `--triagem` gravam o resultado, mas não disparam o B3. Se a operação seguinte chamar o wrapper diretamente, com ambiente herdado ou por uma forma não reconhecida pelo parser, `--b3-parada` encontra o estado anterior livre e retorna zero. Isso também acontece se o estado estiver ausente, mesmo existindo triagens. (`bin/disjuntor.py:788`, `bin/disjuntor.py:791`, `bin/disjuntor.py:947`, `bin/disjuntor.py:956`; `bin/codex_pergunta.sh:54`)

2. **A chamada direta continua negada após uma linha válida de liberação.** Com a obra disparada, escrever um DESENHO/ERRO-BOBO novo e válido não basta para essa porta: ela não lê o RELATO. Sem `decide_b3` intermediário, o wrapper continua recusando. (`bin/disjuntor.py:877`, `bin/disjuntor.py:940`, `bin/disjuntor.py:953`; `bin/codex_pergunta.sh:54`)

3. **O parser pode negar outro papel ou outra obra.** Com OBRA-X parada, o comando abaixo executaria com papel `leitura`, mas o parser pega a primeira atribuição e o B3 o nega. Uma ocorrência dentro de `DUMMY="CODEX_PAPEL=verificador"` também pode prevalecer. Para `CODEX_OBRA="O9 (C)"`, o parser lê somente `O9`, podendo negar pela obra errada. (`bin/disjuntor.py:1226`, `bin/disjuntor.py:1228`, `bin/disjuntor.py:1310`)

   ```bash
   CODEX_OBRA=OBRA-X CODEX_PAPEL=verificador CODEX_PAPEL=leitura bin/codex_pergunta.sh h q r
   ```

4. **O próximo Opus final passa quando a decisão levanta exceção.** Estado corrompido, trava ocupada ou triagem sem permissão de leitura impedem a decisão; `main_pretool` registra FALHA e termina sem negar. O Stop também omite o recado B3 nesse caminho. A porta do wrapper, especificamente para estado ilegível, recusa: são comportamentos diferentes. (`bin/disjuntor.py:257`, `bin/disjuntor.py:264`, `bin/disjuntor.py:798`, `bin/disjuntor.py:1359`, `bin/disjuntor.py:949`; `bin/hook_stop_fila1.py:1056`)

5. **A classificação textual pode pular a guarda do Opus.** Um prompt de leitura final da OBRA-X que inclua a citação antiga “nao e a leitura final” faz `_eh_final` retornar falso. Uma obra não reconhecida também pula a consulta B3. (`bin/disjuntor.py:113`, `bin/disjuntor.py:498`, `bin/disjuntor.py:511`, `bin/disjuntor.py:1320`)

6. **Uma volta já contada pode contar novamente após liberação ou rearme.** Cenário: `a,b,c` PROCEDENTES disparam; `a` vira DESCARTADO; uma decisão recalcula `[b,c]`; a liberação arquiva somente `[b,c]`. Depois, `a` volta a PROCEDENTE e, com apenas duas novas `d,e`, dispara novamente como `[a,d,e]`. O rearme sofre o mesmo problema porque suas pendências também contêm somente PROCEDENTES atuais. (`bin/disjuntor.py:851`, `bin/disjuntor.py:854`, `bin/disjuntor.py:883`, `bin/disjuntor.py:921`)

7. **O rearme não alcança triagens ainda sem registro B3 da obra.** Três PROCEDENTES gravadas antes de qualquer varredura, seguidas de `--rearma-b3`, encontram `r` ausente: o comando recusa o rearme e não arquiva aquelas voltas. Elas podem disparar na primeira decisão posterior. (`bin/disjuntor.py:917`, `bin/disjuntor.py:919`, `bin/disjuntor.py:859`, `bin/disjuntor.py:1551`)

8. **Fonte ausente apaga contagem conhecida e pode permitir recontagem.** Com duas voltas conhecidas, perder `triagem.log` faz a decisão substituir a lista por vazia. Se a obra já estava disparada, liberar durante essa ausência arquiva uma lista vazia; restaurar a trilha permite que as mesmas três voltas antigas disparem novamente. (`bin/disjuntor.py:800`, `bin/disjuntor.py:854`, `bin/disjuntor.py:869`, `bin/disjuntor.py:883`)

9. **Fonte truncada ou corrompida produz contagem silenciosamente errada.** Linhas inválidas são ignoradas. Uma última linha DESCARTADO truncada deixa prevalecer o PROCEDENTE anterior, podendo disparar indevidamente; perder linhas PROCEDENTE subconta. (`bin/disjuntor.py:798`, `bin/disjuntor.py:803`, `bin/disjuntor.py:805`)

10. **A triagem é gravada fora da trava do estado.** Duas triagens iguais concorrentes podem ambas observar ausência e acrescentar duas linhas. Isso quebra a idempotência da escrita, mas **não duplica sozinho a contagem**, porque o leitor deduplica pela identidade. O JSON B3 é gravado sob `flock`; a escrita desprotegida identificada é `triagem.log`. (`bin/disjuntor.py:252`, `bin/disjuntor.py:273`, `bin/disjuntor.py:784`, `bin/disjuntor.py:788`, `bin/disjuntor.py:805`)

11. **Chamadas concorrentes no mesmo caminho podem misturar resposta e identidade.** A termina como verificador; B, papel leitura, sobrescreve o mesmo arquivo; A então calcula o SHA do conteúdo de B e grava seu próprio sidecar. A triagem aceita a resposta de leitura com identidade de verificador. Não há exclusão por caminho envolvendo resposta e sidecar. (`bin/codex_pergunta.sh:163`, `bin/codex_pergunta.sh:184`, `bin/codex_pergunta.sh:186`, `bin/codex_pergunta.sh:187`; `bin/disjuntor.py:779`, `bin/disjuntor.py:781`)

12. **Uma tentativa negada pode destruir a identidade da resposta anterior.** Reutilizar o caminho numa obra parada apaga `.id` antes de consultar B3; a tentativa é negada e a resposta anterior, ainda existente, deixa de ser triável. Além disso, a falha do `rm` não interrompe o script, podendo deixar um sidecar antigo. (`bin/codex_pergunta.sh:27`, `bin/codex_pergunta.sh:52`, `bin/codex_pergunta.sh:54`; `bin/disjuntor.py:776`)

13. **`mtime` não garante que a resposta pertence à chamada.** Uma resposta legítima com `mtime` igual ao cabeçalho não recebe identidade, apesar de rc zero. Condicionalmente, uma resposta antiga com timestamp futuro passa em `-nt` se a execução retornar zero sem substituí-la, recebendo identidade nova. Ocorrência dessas condições: **[nao verificado]**. (`bin/codex_pergunta.sh:125`, `bin/codex_pergunta.sh:141`, `bin/codex_pergunta.sh:184`, `bin/codex_pergunta.sh:190`)

14. **Os campos do sidecar não atestam sua origem.** Fabricar os campos exigidos com SHA correto satisfaz a validação; alterar `papel` de leitura para verificador mantém o SHA da resposta válido. O próprio selo fabrica identidades dessa forma. Portanto, a exigência literal de identidade *gravada pelo wrapper* excede o que a checagem demonstra. (`bin/disjuntor.py:752`, `bin/disjuntor.py:759`, `bin/disjuntor.py:779`, `bin/disjuntor.py:781`; `bin/tests/test_disjuntor.sh:1129`)

15. **A leitura apresenta zeros compatíveis com “verde” sem ter lido a trilha.** `ler` inicia disparos, negações e liberações zerados; ignora `OSError` na trilha; `linha` apresenta `B3=0` e os demais zeros. Estado ausente também resulta em zero para `b3_sem_obra`; estado ilegível, nesse campo específico, preserva “nao sei”. (`bin/disjuntor.py:1403`, `bin/disjuntor.py:1435`, `bin/disjuntor.py:1523`, `bin/disjuntor.py:1531`, `bin/disjuntor.py:295`, `bin/disjuntor.py:1469`)

A cobertura de `b3_codex` e dos mutantes é parcial; os resultados de execução permanecem **[nao verificado]**. (`bin/tests/test_disjuntor.sh:1125`, `bin/tests/test_disjuntor.sh:1272`)

| Item | Casos e mutantes existentes | O que fica sem caso que morde |
|---|---|---|
| **1** | Duas passam, terceira dispara; obras separadas; mesma identidade triada cinco vezes. Mutantes `sem-codex`, `limiar-2`, `por-achado`. (`bin/tests/test_disjuntor.sh:1147`, `bin/tests/test_disjuntor.sh:1151`, `bin/tests/test_disjuntor.sh:1156`, `bin/tests/test_disjuntor.sh:1272`, `bin/tests/test_disjuntor.sh:1282`) | Uma chamada real produzindo exatamente uma identidade: as respostas e sidecars são fabricados pelo teste. (`bin/tests/test_disjuntor.sh:1129`) |
| **2** | DESCARTADO, não triado e PROCEDENTE→DESCARTADO; mutantes `conta-descartado` e `acumula`. (`bin/tests/test_disjuntor.sh:1140`, `bin/tests/test_disjuntor.sh:1144`, `bin/tests/test_disjuntor.sh:1174`, `bin/tests/test_disjuntor.sh:1273`, `bin/tests/test_disjuntor.sh:1284`) | Fonte ausente, truncada ou ilegível. Os negativos usam ausência do recado B3, podendo aceitar uma falha aberta. (`bin/tests/test_disjuntor.sh:1135`, `bin/tests/test_disjuntor.sh:1142`, `bin/tests/test_disjuntor.sh:1180`) |
| **3** | Negação de Opus/Codex, leitura permitida, Stop e DESENHO liberando; mutantes `volta-nao-nega` e `porta-sempre-livre`. (`bin/tests/test_disjuntor.sh:1157`, `bin/tests/test_disjuntor.sh:1159`, `bin/tests/test_disjuntor.sh:1164`, `bin/tests/test_disjuntor.sh:1275`, `bin/tests/test_disjuntor.sh:1287`) | **A porta real do wrapper.** O teste chama somente Python, depois de `_parada` já materializar o disparo; não testa wrapper como primeira operação após terceira triagem ou após liberação. (`bin/tests/test_disjuntor.sh:1191`, `bin/tests/test_disjuntor.sh:1192`) |
| **4** | Recusa sem sidecar, outro papel, veredito inválido e SHA divergente; mutantes `sem-sidecar` e `sem-sha`. (`bin/tests/test_disjuntor.sh:1137`, `bin/tests/test_disjuntor.sh:1173`, `bin/tests/test_disjuntor.sh:1276`, `bin/tests/test_disjuntor.sh:1286`) | Produção/autoria da identidade pelo wrapper e concorrência da escrita. Os mutantes copiam apenas `.py`, excluindo o shell. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1207`) |
| **5** | Liberação seguida de nova volta isolada; reclassificação depois da liberação; rearme com PROCEDENTES pendentes. Mutantes retiram arquivamento na liberação e pendências no rearme. (`bin/tests/test_disjuntor.sh:1167`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1184`, `bin/tests/test_disjuntor.sh:1281`, `bin/tests/test_disjuntor.sh:1285`) | Reclassificação **antes** do reset, fonte ausente durante reset e rearme sem registro B3 prévio. (`bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:1183`, `bin/tests/test_disjuntor.sh:1186`) |

**O buraco decisivo do selo é o item 3 na integração com o wrapper:** retirar `codex_pergunta.sh:54` fica fora do percurso exercitado. O item 4 também não tem prova da identidade realmente emitida pelo wrapper, e o item 5 não tem o ordenamento que ressuscita voltas antigas. ERRO-BOBO e validação detalhada do topo têm casos gerais em `b3`/`b3_topo`, mas isso não cobre essas lacunas de integração e reset. (`bin/tests/test_disjuntor.sh:1129`, `bin/tests/test_disjuntor.sh:1192`, `bin/tests/test_disjuntor.sh:1207`, `bin/tests/test_disjuntor.sh:1170`, `bin/tests/test_disjuntor.sh:386`, `bin/tests/test_disjuntor.sh:700`)

## 31/10 14:59 · sessao e59b691f8ed7 · - · - · origem=Ronald (tmux)

- **BLOQUEIA — HE noturna usa CCT de outra data.** O fechamento monta o motor em `fatia_ini` (`app/ponto/services/fechamento.py:711`); esse motor grava a dobra no período (`app/ponto/motor_calculo_v2.py:2747`). A alimentação copia suas HE50/100 (`app/ponto/calculador/alimentacao.py:92`) e as entrega intactas (`app/ponto/calculador/alimentacao.py:535`). Entretanto, os totais passam a usar a CCT diária (`app/ponto/calculador/alimentacao.py:514`), enquanto os splits continuam usando as extras antigas (`app/ponto/calculador/regras.py:822`). Reprodução em memória com funções extraídas por AST: plantão 23h–05h, trabalhado=previsto=360, CCT diária sem dobra, período antigo com 360 minutos de HE100 ⇒ `HE100=0`, `HE100_noturna=6`; os ramos responsáveis estão em `app/ponto/calculador/regras.py:775` e `app/ponto/calculador/regras.py:834`. Ocorrência em produção: **[nao verificado]**.

- **NAO-BLOQUEIA — cobertura incompleta da retirada do `return`.** Além dos três efeitos exercitados, o fluxo agora calcula saída antecipada e publica detalhes de cadastro incompatível (`app/ponto/calculador/regras.py:851`, `app/ponto/calculador/regras.py:858`), além de HE50 noturna (`app/ponto/calculador/regras.py:831`). O cenário novo tem trabalhado=360 e previsto=420 (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:38`), portanto não testa dobra com excedente positivo nem protege a exclusão da HE50 pelo `elif` (`app/ponto/calculador/regras.py:794`).

- **NAO-BLOQUEIA — teste que não morde.** `test_a_dobra_continua_paga` verifica apenas a dobra, já atribuída antes do antigo retorno; passaria sem retirar esse retorno (`app/ponto/tests/test_v3_dobra_noturna_e_cct_do_dia.py:45`; `app/ponto/calculador/regras.py:780`).

O chamador de produção de `regras.do_dia` é `app/ponto/calculador/alimentacao.py:523`; `rubricas_por_dia` é chamado pela lavratura do fechamento (`app/ponto/services/fechamento.py:1239`, versão `oraculo` em `:1262`) e pelo comando comparador (`app/ponto/management/commands/diff_calculador.py:417`).

_1111 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
