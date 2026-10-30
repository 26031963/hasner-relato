# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 21:34** · publicado em **29/10/2026 21:35** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 21:34 · sessao 45d6d74dee1e · o261 · sonda · origem=Code

[nao verificado] — Transcrição do dado fornecido:

```text
MODO MEDIR | universo (respondida sem resposta, VIVA, nao validada) 263, esperado 263 | competencia ABERTA 115 | EXPORTADA -> Pauta DP 148 em 2 empresa x competencia | sem data_evento 0 | colabs 23
ABERTA por competencia: {'2:10/2026': 88, '3:10/2026': 5, '2:11/2026': 22}
EXPORTADA por competencia: {'2:09/2026': 147, '3:09/2026': 1}
ABERTA por via_resolucao: {'chamado_encerrado': 113, 'ausencia_dia': 1, 'completude_turno': 1}
ABERTA com disputa fechada (badge do colab nao alcanca: cura (4) alcancabilidade): 0
porta reabrir_para_correcao alcanca (perguntas_corrigiveis): 115 de 115; fora: []
VIGIA desde 17:20: respondida sem resposta 0 | com respondida==materializada (ramo sem credor) 0 | vias {}
PROVA universo depois 263 (esperado = exportadas 148 se APLICAR, = 263 se MEDIR) | competencia ABERTA ainda no universo 115 (esperado 0 depois do APLICAR: 2a passada = 0)
PROVA estado antes->depois das ABERTA: {('respondida', 'respondida'): 115} | vivas depois 115 de 115
PROVA exportadas vigentes intactas: True (7)
ERROS=0
FIM-DA-SONDA
```

## 29/10 21:15 · sessao c435b9a9bb3f · o261 · verificador · origem=Code

**A v3 corrige a reprovação da volta 2:** o contrato do carimbo agora inclui o contador e seu esperado. Sem achado bloqueante por inspeção; permanece o achado baixo do seletor da auditoria. (`app/chamados/tests/test_o261_vigia_fio.py:701,706`, pós-diff; `app/core/models.py:286`.)

As referências “pós-diff” usam a numeração resultante do patch fornecido. Execução dos testes, REDs e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **Sítios criados ou tocados — cumprimento do aval**

   - **`disputa_emissao.py` — sem achado na cura.** A guarda exige texto com `strip()`, preserva materialização, autor e via, mantém a consulta ao C6 e acrescenta trilha com antes/depois. Não acrescenta escrita de `validada_em`. (`app/chamados/services/disputa_emissao.py:1641,1643,1655,1659,1661`, pós-diff; `app/docs/CORTES.md:383`.)
   - **`vigia_fio.py` — sem achado.** O contador lê o par persistido, aplica o mesmo `strip()`, aceita recorte por empresa e restringe-se ao carimbo posterior ao marco. (`app/chamados/services/vigia_fio.py:369,372,381,383,385`, pós-diff.)
   - **`vigiar_fio_morto.py` — sem achado.** Imprime o contador, inclui medido/esperado no carimbo e conserva os grupos de alarme. (`app/chamados/management/commands/vigiar_fio_morto.py:79,89,172,193,198`, pós-diff.)
   - **Novo `test_o261_resposta_vazia.py` — sem achado por inspeção.** Cobra vazio, espaços, texto, conteúdo da trilha, repetição sequencial, corte histórico e integração com impressão/carimbo. (`app/chamados/tests/test_o261_resposta_vazia.py:77,89,97,106,124,134,145`, pós-diff.)
   - **`test_o261_vigia_fio.py` — sem achado remanescente.** As duas igualdades agora conferem a composição nova sem retirar a comparação dos contadores anteriores. (`app/chamados/tests/test_o261_vigia_fio.py:701,706`, pós-diff.)

   A reparação das 263 vivas, reversão/prova e encaminhamento das exportadas são exigências do aval cuja realização permanece **[nao verificado]**; este diff não demonstra essa etapa. (`app/docs/CORTES.md:383`.)

2. **Classes de reprovação**

   - **Segundo juiz — sem achado. Derivação paralela — sem achado.** O escritor mantém `pergunta_viva`; o contador verifica a inconsistência dos campos, sem decidir vitalidade ou via. (`app/chamados/services/disputa_emissao.py:1641`; `app/chamados/services/vigia_fio.py:372`, pós-diff.)
   - **Segundo escritor fora da porta — sem achado.** Permanecem os dois sítios declarados da função; a auditoria passa pelo escritor único `evento`. (`app/chamados/tests/test_porta_resposta_pergunta.py:68,95`; `app/chamados/services/disputa_emissao.py:1659,1661`, pós-diff; `app/core/observ.py:94`.)
   - **Relógio fora da borda — sem achado novo.** `_tz.now()` já existia; o contador acrescenta uma constante temporal e o teste novo congela o relógio. (`app/chamados/services/disputa_emissao.py:1638`, cópia; `app/chamados/services/vigia_fio.py:369`; `app/chamados/tests/test_o261_resposta_vazia.py:32`, pós-diff.)
   - **Ordem de trava — sem achado novo.** O diff não acrescenta bloqueio de negócio; a auditoria usa seu `atomic` existente. (`app/chamados/services/disputa_emissao.py:1659,1661`, pós-diff; `app/core/observ.py:133`.)
   - **Idempotência sequencial — sem achado.** A pergunta materializada sai de `_abertas`; o teste exige estado igual e uma única linha após duas chamadas. Garantia concorrente: **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1634`, cópia; `app/chamados/tests/test_o261_resposta_vazia.py:106`, pós-diff.)
   - **Selo sem mordida — sem achado por inspeção.** Há casos positivos e negativos para a guarda, a trilha e o contador; execução efetiva: **[nao verificado]**. (`app/chamados/tests/test_o261_resposta_vazia.py:77,97,106,134,145`, pós-diff.)

3. **Leitores, contratos e testes existentes** [nao verificado]

   - **Baixa — ação ausente do seletor da auditoria.** `fecho_sem_credor` não integra `ACOES`, usada pelo dropdown. A gravação aceita verbo livre; listagem geral e filtro GET explícito continuam disponíveis. (`app/chamados/services/disputa_emissao.py:1661`, pós-diff; `app/core/models.py:286,296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:25`.)
   - **Carimbo, painel e saída `VFM|` — sem achado remanescente.** O painel compara genericamente `esperado` contra `censo`; os testes shell usam carimbos sintéticos compatíveis; a linha antiga `VFM|ESPERADO|` permanece baseada no mesmo dicionário. (`bin/fio_estado.py:69,105`; `bin/tests/test_fio_estado.sh:40,93`; `app/chamados/management/commands/vigiar_fio_morto.py:175`, pós-diff.)
   - **Leitores dos campos — sem achado funcional.** Os leitores de pendência/vitalidade continuam usando o juiz; o teste existente da órfã exige materialização, via e fecho, sem exigir carimbo de resposta. (`app/chamados/models.py:1381,2111`; `app/folha/fora_da_folha.py:66`; `app/chamados/tests/test_b2d_resposta_visivel.py:76,97`.)
   - **Baixa, preexistente e fora do diff — contrato textual desatualizado.** O teste do par ainda chama carimbo sem texto de “inofensivo” e “legítimo”. Seu detector unidirecional não quebra com a v3, mas essa explicação contraria o aval novo. (`app/chamados/tests/test_contract_par_resposta.py:28,81,113`; `app/docs/CORTES.md:383`.)

4. **Mensagem do commit** [nao verificado]

   **Sem achado funcional contra o que ela descreve.** Os irmãos conferidos só criam o carimbo com texto; ausência pode regravar o valor existente. (`app/chamados/services/disputa_emissao.py:1570`; `app/chamados/services/materializacao.py:511,528,1190`.)

   Duas qualificações: “instante do aval” é precisão não demonstrada por `17:2x`; e “cada fecho deixa UMA linha” descreve o caminho normal, pois `evento` é *best-effort*. O instante exato e a gravação efetiva de cada trilha são **[nao verificado]**. (`app/docs/CORTES.md:383`; `app/chamados/services/vigia_fio.py:369`, pós-diff; `app/core/observ.py:98,105,139`.)

## 29/10 21:14 · sessao f85af26a11ef · - · - · origem=Ronald (tmux)

Sem quebra executável identificada nos testes existentes do escritor: o caso sem credor exige materialização/via/fecho, sem exigir `respondida_em`; os casos que exigem carimbo têm texto. (`app/chamados/tests/test_b2d_resposta_visivel.py:76`; `app/chamados/tests/test_materializar_carimba_respondida.py:62`; `app/chamados/tests/test_materializar_carimba_respondida.py:76`.)

**Baixa, documentação de contrato desatualizada:** o contrato existente ainda declara carimbo sem texto “inofensivo” e “legítimo”, contrário ao novo aval; o detector permanece unidirecional e não falha com a v3. Não é quebra funcional. (`app/chamados/tests/test_contract_par_resposta.py:28`; `app/chamados/tests/test_contract_par_resposta.py:81`; `app/chamados/tests/test_contract_par_resposta.py:113`; `app/docs/CORTES.md:383`.)

**Censo de escritores — sem achado contra a mensagem:** os irmãos de materialização e S128 só criam o carimbo com texto; ausência inclui o campo no `save` mesmo sem atribuir valor novo. A resposta automática recebe texto constante no único chamador localizado. (`app/chamados/services/materializacao.py:511`; `app/chamados/services/materializacao.py:528`; `app/chamados/services/materializacao.py:1190`; `app/chamados/services/disputa_emissao.py:1570`; `app/api/foto_recebida.py:25`; `app/api/foto_recebida.py:48`.)

**Relógio/contrato estrutural — sem achado:** o teste novo congela o relógio e conserva os dois sítios de escrita já declarados; não acrescenta atribuição isolada de texto. (`diff_v3.patch`, `app/chamados/tests/test_o261_resposta_vazia.py:32`; `app/chamados/tests/test_porta_resposta_pergunta.py:68`; `app/chamados/tests/test_contract_par_resposta.py:81`; `app/core/tests/test_selo_teste_sem_relogio.py:34`.)

Execução dos testes e efeitos reais: [nao verificado]. [nao verificado]

## 29/10 21:14 · sessao d9a6926f4fa7 · - · - · origem=Ronald (tmux)

- **Baixa — ação nova ausente do seletor de auditoria.** `fecho_sem_credor` não integra `ACOES`; a tela alimenta o dropdown por essa lista. A gravação aceita verbos livres, e a listagem geral e o filtro GET explícito permanecem disponíveis. (`app/core/models.py:286,296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:25`; ação nova: `app/chamados/services/disputa_emissao.py:1661`, diff.)

- **Outros testes de auditoria e portas — sem achado.** O novo evento continua usando o escritor único; a função conserva seus dois sítios de escrita de pergunta. (`app/core/observ.py:94,134`; `app/core/tests/test_contract_trilha.py:57`; `app/chamados/tests/test_porta_resposta_pergunta.py:66,95`; `app/chamados/services/disputa_emissao.py:1659,1661`, diff.)

- **Contratos dos campos — sem achado.** O teste existente da órfã sem resposta exige materialização, via e fechamento, sem exigir `respondida_em`; os casos existentes de carimbo com texto pertencem ao ramo S128 preservado. (`app/chamados/tests/test_b2d_resposta_visivel.py:76,97`; `app/chamados/tests/test_materializar_carimba_respondida.py:66,81`; `app/chamados/services/disputa_emissao.py:1570`.)

- **Leitores da pergunta — sem achado.** Pendências consultam vitalidade; `.estado` mantém `RESOLVIDA` para a morta materializada; fora da folha considera somente vivas. (`app/chamados/models.py:1381,2111,2113`; `app/chamados/services/aviso_pendencia.py:13`; `app/folha/fora_da_folha.py:66`.)

Execução dos testes e efeitos em produção: **[nao verificado]**. [nao verificado]

## 29/10 21:13 · sessao 2e2b8e5f00e7 · - · - · origem=Ronald (tmux)

- **Contrato do carimbo — sem achado remanescente.** O v3 atualiza a igualdade integral de `censo` e o conjunto de `esperado` para conter `respondida_sem_resposta`, corrigindo as duas incompatibilidades apontadas na v2. (`app/chamados/tests/test_o261_vigia_fio.py:701,706-709`, diff pós-aplicação.)

- **Leitor do painel — sem achado.** A comparação percorre genericamente todas as chaves de `esperado`; a chave nova divergente aparecerá em `FORA DO ESPERADO`, mesmo sem constar de `gritou`. (`bin/fio_estado.py:69-73,105-108,118-119`.) O ESTADO usa esse leitor e compositor. (`bin/gerar_estado.py:256-260`.)

- **Outros testes do carimbo — sem achado.** O selo shell usa carimbos sintéticos e não exige um conjunto fechado de chaves do comando real; as verificações existentes de caminho, recorte e alarme permanecem compatíveis. (`bin/tests/test_fio_estado.sh:40-47,93-102`; `app/chamados/tests/test_o261_vigia_fio.py:717-750`, cópia.)

- **Saída `VFM|` — sem achado.** A nova linha é adicional; a linha `VFM|ESPERADO|` continua renderizando o mesmo `ESPERADO`, e os grupos de alarme não mudam. (`app/chamados/management/commands/vigiar_fio_morto.py:79-89,168-183`, diff pós-aplicação.)

- **Novo teste do comando — sem achado de isolamento sob settings CI.** Embora use o caminho padrão do comando, CI já redireciona o carimbo para um diretório temporário. (`app/chamados/tests/test_o261_resposta_vazia.py:154-160`, diff; `app/config/settings/ci.py:30-33`; `app/chamados/management/commands/vigiar_fio_morto.py:100-101`, cópia.)

Execução dos testes: **[nao verificado]**. [nao verificado]

## 29/10 21:09 · sessao c7d5d7152cde · o261 · verificador · origem=Code

**Ainda não aprovaria a v2: ela quebra um teste existente do carimbo em duas asserções.** (`app/chamados/tests/test_o261_vigia_fio.py:699,704`.)

“Diff” abaixo indica numeração pós-aplicação; demais referências são da cópia. Execução dos testes e efeitos em produção: **[nao verificado]**. [nao verificado]

1. **Cumprimento do aval**

   **Sem achado no escritor:** a guarda usa texto com `strip()`, conserva materialização, autor e via, e mantém a consulta ao C6. A trilha agora captura o estado anterior e descreve a presença de resposta independentemente de acrescentar um carimbo nesta chamada. (`app/docs/CORTES.md:383`; `app/chamados/services/disputa_emissao.py:1641,1643,1649,1655,1661`, diff.)

   O contador passa a aplicar o mesmo teste Python e recebe o recorte de empresas. **Sem achado nesses ajustes.** A reparação das 263, com reversão/prova e encaminhamento do exportado, permanece **[nao verificado]**. (`app/chamados/services/vigia_fio.py:372,381,383,384`, diff; `app/chamados/management/commands/vigiar_fio_morto.py:172`, diff; `app/docs/CORTES.md:383`.)

2. **Classes de reprovação**

   - **Segundo juiz / derivação paralela — sem achado.** O contador verifica a inconsistência dos campos, usando o mesmo `strip()`; não decide vitalidade, via ou destino da pergunta. (`app/chamados/services/vigia_fio.py:372,384`, diff; `app/chamados/juizes.py:1020`.)
   - **Segundo escritor fora da porta — sem achado.** A alteração permanece no ato já declarado; a trilha usa `core.observ.evento`. (`app/chamados/tests/test_porta_resposta_pergunta.py:66,68,95`; `app/chamados/services/disputa_emissao.py:1659,1661`, diff; `app/core/observ.py:94`.)
   - **Relógio fora da borda — sem achado novo.** O `_tz.now()` deste ramo já existia; o contador acrescenta uma constante temporal, sem nova leitura do relógio. (`app/chamados/services/disputa_emissao.py:1638`, cópia; `app/chamados/services/vigia_fio.py:369`, diff; `app/docs/CORTES.md:369`.)
   - **Ordem de trava — sem achado novo.** O diff não acrescenta trava de negócio; o evento usa o `atomic` da porta de auditoria. (`app/chamados/services/disputa_emissao.py:1659,1661`, diff; `app/core/observ.py:133`.)
   - **Idempotência sequencial — sem achado.** A segunda chamada encontra a pergunta materializada, fora de `_abertas`; o teste novo também exige estado igual e uma única linha de trilha. (`app/chamados/services/disputa_emissao.py:1634`, cópia; `app/chamados/tests/test_o261_resposta_vazia.py:106`, diff.)
   - **Selo sem mordida — sem achado nos casos novos por inspeção.** Há asserções para vazio, espaços, texto, conteúdo da trilha e impressão/carimbo do comando. Isso não resolve a incompatibilidade com o selo existente, descrita abaixo. RED e execução efetivos: **[nao verificado]**. (`app/chamados/tests/test_o261_resposta_vazia.py:77,89,97,106,124,145`, diff; `app/chamados/tests/test_o261_vigia_fio.py:699,704`.)

3. **Leitores, contratos e testes existentes** [nao verificado]

   - **Média — contrato do carimbo quebrado.** O comando acrescenta `respondida_sem_resposta` a `reg['censo']`, mas `censo()` continua sem essa chave. Portanto, a igualdade integral exigida pelo teste existente falhará **mesmo quando o contador for zero**. A segunda incompatibilidade é `esperado`: o teste exige exatamente as quatro chaves antigas. É necessário atualizar o contrato e sua prova para a composição nova, preservando a conferência dos contadores. (`app/chamados/management/commands/vigiar_fio_morto.py:193,198`, diff; `app/chamados/services/vigia_fio.py:205,326`, cópia; `app/chamados/tests/test_o261_vigia_fio.py:699,704`.)

   - **Baixa — permanece a ausência da ação no seletor da auditoria.** `fecho_sem_credor` não está em `ACOES`, que alimenta o dropdown. A linha continua acessível pela listagem geral e pelo filtro GET explícito; a gravação admite o verbo livre. (`app/core/models.py:286,296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:23`.)

   - **`bin/fio_estado.py` — sem achado.** O leitor compara genericamente `esperado` com `censo` e publica a divergência com valor medido e esperado. Estar fora de `gritou` não impede a exibição. (`bin/fio_estado.py:69,72,105,118`.)

   - **Leitores da pergunta — sem achado.** O badge exige disputa aberta e pergunta viva; `.estado` conserva `RESOLVIDA` para a pergunta morta materializada; `_fio_do_dia` considera somente perguntas vivas. (`app/chamados/models.py:1381,2111,2113`; `app/folha/fora_da_folha.py:66`.)

4. **Triagem dos descartados**

   - **Marco 17:20 — descarte como bloqueio procede.** É o início conservador da janela `17:2x`: pode contar ocorrências anteriores ao instante efetivo, sem excluir as posteriores. Não demonstra o instante exato do aval; esse instante e a existência de ocorrências nesse intervalo são **[nao verificado]**. (`app/chamados/services/vigia_fio.py:369,381`, diff; `app/docs/CORTES.md:383`.)
   - **Concorrência — descarte procede no contrato sequencial informado.** A leitura sem bloqueio e o `save` já existiam. A trilha nova também pode duplicar sob concorrência, mas isso não demonstra quebra das duas chamadas sequenciais exigidas. (`app/chamados/services/disputa_emissao.py:1630,1634,1651`, cópia; `:1661`, diff; `app/chamados/tests/test_fio_disputa_resolvido.py:82`.)

## 29/10 21:08 · sessao 1fff0cdcd0a0 · - · - · origem=Ronald (tmux)

- **Baixa — permanece a limitação do filtro da auditoria:** `fecho_sem_credor` não aparece no seletor de ações, pois `ACOES` enumera somente sete verbos e a tela usa essa lista. A gravação aceita o verbo livre; listagem geral e filtro GET continuam alcançando o evento. (`app/core/models.py:286-296`; `app/core/views.py:705,709,738`; `app/templates/core/log_auditoria.html:23-27`.)

- **Trilha v2 — sem achado:** `_antes` captura via e carimbo anteriores; `materializada_em=None` corresponde ao filtro de entrada. A descrição passa a distinguir presença de texto, independentemente de acrescentar carimbo nesta chamada. (`app/chamados/services/disputa_emissao.py:1634-1635`, cópia; `:1643,1649-1656,1661-1668`, diff.)

- **Segundo escritor / juiz / trava / relógio — sem achado novo:** permanecem os dois saves declarados do ato, a consulta a C6 e o relógio já existente; a auditoria usa a porta canônica `evento`, que abre seu próprio `atomic`. (`app/chamados/tests/test_porta_resposta_pergunta.py:66-68,95-103`; `app/chamados/services/disputa_emissao.py:1641,1638`, cópia; `app/core/observ.py:94,133-138`.)

- **Idempotência — descarte da concorrência procede no contrato informado:** leitura sem bloqueio e save já existiam; os testes e a definição da casa exigem duas chamadas sequenciais. A auditoria duplicada sob concorrência é possível, mas sua ocorrência e exigência contratual são **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1630-1651`, cópia; `app/chamados/tests/test_fio_disputa_resolvido.py:82-89`; `app/chamados/models.py:2124-2128`.)

- **Leitores da pergunta — sem regressão identificada:** o badge exige disputa aberta e C6 vivo; `.estado` continua retornando resolvida para pergunta morta materializada mesmo com `respondida_em=None`. (`app/chamados/models.py:1381-1385,2111-2114`.)

- **Selos do escritor — sem achado por leitura:** os testes novos cobrem vazio, espaços, controle com texto, estado e trilha sequenciais. Execução e RED efetivo: **[nao verificado]**. (`app/chamados/tests/test_o261_resposta_vazia.py:77-130`, diff.)

## 29/10 21:08 · sessao b7b7ed5f0e24 · - · - · origem=Ronald (tmux)

**Média — contrato existente do carimbo quebrado.** O v2 acrescenta a chave `respondida_sem_resposta` em `reg['censo']`, mas deixa `censo()` sem essa chave. O teste existente exige igualdade integral entre ambos e falhará nessa comparação; também exige o dicionário `esperado` antigo, sem a chave nova. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:193,198` (**diff**, conforme hunk `+190`); `app/chamados/services/vigia_fio.py:205,325,326` (**cópia**); `app/chamados/tests/test_o261_vigia_fio.py:699,704-706` (**cópia**). Execução: **[nao verificado]**.

**`bin/fio_estado.py` — sem achado.** O leitor compara genericamente todas as chaves de `esperado` contra `censo` e renderiza as divergências com medição e esperado; portanto exibirá o contador novo quando diferente de zero. O fato de não estar em `gritou` não impede essa exibição. (`bin/fio_estado.py:69-73,105-108,118-119`.)

**Outros contratos do comando — sem achado.** O v2 conserva o recorte sem carimbo e as constantes de alarme, compatíveis com os respectivos testes existentes. (`app/chamados/management/commands/vigiar_fio_morto.py:79-80,177-197`, cópia; `app/chamados/tests/test_o261_vigia_fio.py:647-657,717-750`.)

**Teste shell do leitor — sem achado identificado por inspeção.** Ele usa carimbos sintéticos e verifica a comparação genérica, sem exigir o conjunto completo das chaves produzidas pelo comando. Execução: **[nao verificado]**. (`bin/tests/test_fio_estado.sh:36-51,82-91`.)

## 29/10 21:04 · sessao 17fc8315b18b · o261 · verificador · origem=Code

**Não aprovaria a v1 ainda:** a guarda corrige o carimbo sem texto, mas há falhas na trilha, na idempotência concorrente e no recorte do vigia. (`app/chamados/services/disputa_emissao.py:1652,1660`; `app/chamados/management/commands/vigiar_fio_morto.py:166`, diff.)

Referências marcadas **“diff”** usam a numeração após aplicação; as demais são da cópia. Execução dos testes, comportamento do PostgreSQL real e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **Cumprimento do aval**

   A cura principal está no lugar autorizado: condiciona `respondida_em` ao texto, conserva materialização, autor e via, consulta `pergunta_viva` e mantém o fechamento da disputa por `d.fechar`. Não acrescenta reparação do passivo das 263. **Sem achado nessa delimitação.** (`app/docs/CORTES.md:383`; `app/chamados/services/disputa_emissao.py:1641,1643,1652,1673`, diff.)

   - **Média — trilha pode afirmar um estado falso.** A descrição distingue os casos por `'respondida_em' in _c`, que significa **carimbo acrescentado nesta chamada**, não existência de resposta. Com carimbo anterior preservado, escreve “respondida_em fica nulo”; com texto anterior, pode ainda declarar “sem resposta” enquanto `tem_resposta=True`. `_abertas` não exclui perguntas já respondidas. (`app/chamados/services/disputa_emissao.py:1634,1653,1660,1663`, diff.)
   - **Média — falta o “antes” da trilha.** O evento registra somente `depois`, embora a regra exija usuário, antes/depois e motivo para ação crítica; `antes` ficará `None`. (`app/chamados/services/disputa_emissao.py:1658,1662`, diff; `app/core/observ.py:94`; `app/docs/LEIS.md:65`.)
   - **Lacuna de evidência — marco exato.** O código fixa **17:20**, mas o aval fornecido registra **17:2x**. Não é possível certificar esse instante como a fronteira autorizada entre histórico e novas ocorrências. (`app/chamados/services/vigia_fio.py:369`, diff; `app/docs/CORTES.md:383`.) Instante efetivo: **[nao verificado]**.

2. **Classes de reprovação**

   - **Segundo juiz / derivação paralela — Média.** O critério de texto já participa do juiz registrado C6 como `(resposta_colab or '').strip()`. O vigia cria outra implementação, por regex, sem compartilhar o predicado nem demonstrar equivalência. Deve compartilhar o critério de presença de texto; chamar `pergunta_viva` inteira também confundiria vazio com vitalidade. (`app/core/juizes.py:232`; `app/chamados/juizes.py:1005,1020`; `app/chamados/services/vigia_fio.py:375,381`, diff; `CLAUDE.md:20`.)
   - **Segundo escritor fora da porta — sem achado.** A alteração permanece no ato já declarado de resolução da pergunta; a auditoria nova passa por `core.observ.evento`, sem criação direta de `LogAuditoria`. (`app/chamados/tests/test_porta_resposta_pergunta.py:68,88`; `app/chamados/services/disputa_emissao.py:1657`, diff; `app/core/observ.py:94,134`.)
   - **Relógio — sem leitura nova identificada.** Permanecem dois `_tz.now()` internos para os carimbos. O complemento de injeção nomeia juiz/grade/cartório/reconciliador/fábrica; não basta, isoladamente, para concluir que esta porta de escrita está proibida de obter o instante. A constante nova delimita observação histórica, sem alterar uma decisão de negócio; sua precisão continua sem prova, conforme acima. (`app/chamados/services/disputa_emissao.py:1561,1638`; `app/docs/CORTES.md:369`; `app/chamados/services/vigia_fio.py:365,369`, diff.)
   - **Ordem de trava — sem inversão nova identificada.** O ramo não acrescenta trava explícita; `evento` abre um `atomic` para a trilha. Isso deixa a falha de serialização abaixo. (`app/chamados/services/disputa_emissao.py:1630,1656`, diff; `app/core/observ.py:133`.)
   - **Idempotência concorrente — Média.** Duas execuções podem ler a mesma pergunta aberta antes dos respectivos saves, sobrescrever a materialização e emitir duas linhas. Não há bloqueio da leitura nem atualização condicional; o `atomic` do chamador não reivindica a pergunta. O teste novo cobre apenas chamadas sequenciais. (`app/chamados/services/disputa_emissao.py:1630,1634,1656,1658`, diff; `app/chamados/services/becos.py:41,49`; `app/chamados/tests/test_o261_resposta_vazia.py:97`, diff.)

   **Selos/RED:**

   - **Sem achado de passagem vazia nos casos principais:** restaurar o carimbo indiscriminado contraria `assertIsNone`; retirar a materialização ou o evento contraria as asserções de materialização e contagem. O caso com texto é controle positivo, já atendido pelo código antigo. (`app/chamados/tests/test_o261_resposta_vazia.py:80,84,92,102`, diff; `app/chamados/services/disputa_emissao.py:1648`, cópia.)
   - **Baixa — cobertura incompleta do escritor:** substituir a guarda por `bool(q.resposta_colab)` deixaria passar resposta só com espaços; os testes do escritor usam apenas `''` e `'07:55'`. O teste com espaços chama somente o contador. (`app/chamados/tests/test_o261_resposta_vazia.py:77,88,115`, diff.)
   - **Média — integração do vigia sem mordida nova:** retirar do comando a impressão, o carimbo e o alarme novos não afeta o teste acrescentado, que chama diretamente a função contadora. Também faltam asserções sobre fechamento da disputa, via específica e conteúdo da trilha. (`app/chamados/tests/test_o261_resposta_vazia.py:77,97,106`, diff.)

3. **Leitores e contratos**

   - **Média — `--empresa` mistura recorte e frota.** Os contadores existentes recebem `empresas`; o novo é chamado sem recorte e consulta todas as perguntas. Uma infração de outra empresa pode aparecer no resultado selecionado e provocar saída 2 com `--alarme`. (`app/chamados/management/commands/vigiar_fio_morto.py:143,154,161,166,204`, diff; `app/chamados/services/vigia_fio.py:380`, diff.)
   - **Baixa — ESTADO perde o número e seu esperado.** O contador fica no topo do JSON, fora de `censo` e `ESPERADO`. O leitor compara somente esses dois conjuntos; mostrará o nome em `gritou`, mas não a medição nova nem seu esperado zero. (`app/chamados/management/commands/vigiar_fio_morto.py:86,195,197`, diff; `bin/fio_estado.py:69,82,118`.)
   - **Baixa — ação ausente no filtro enumerado da auditoria.** `fecho_sem_credor` não entra no dropdown alimentado por `ACOES`. A listagem geral e o filtro GET livre continuam alcançando a linha. (`app/core/models.py:286`; `app/core/views.py:705,738`; `app/templates/core/log_auditoria.html:23`.)
   - **Gravação da ação e código de saída — sem achado.** O campo admite verbos livres na gravação usada por `evento`; o comando conserva o carimbo antes da saída 2, e o placar já interpreta esse código como alarme. (`app/core/models.py:296`; `app/core/observ.py:134`; `app/chamados/management/commands/vigiar_fio_morto.py:184,204`, diff; `app/core/management/commands/pipeline_placar.py:75,98`.)

4. **Query, schema e PostgreSQL**

   - **`''`, NULL e fronteira inclusiva — sem achado.** `TextField(blank=True, default='')` não declara `null=True`; portanto o ramo `isnull` é redundante. O `*` contempla a string vazia, e `gte` inclui exatamente o marco. (`app/chamados/models.py:1865`; `app/chamados/services/vigia_fio.py:380,381`, diff.)
   - **Paridade com `strip()` — não demonstrada.** O teste cobre espaços ASCII, mas não tabulações, quebras de linha ou Unicode. A equivalência de `\s` do PostgreSQL com o critério Python, para a configuração real, é **[nao verificado]**; permanece o achado de definição paralela. (`app/chamados/juizes.py:1020`; `app/chamados/tests/test_o261_resposta_vazia.py:115`, diff.)
   - **“Novas” significa carimbo posterior ao marco.** A query não mede quando o estado ilegal surgiu. Por exemplo, apagar posteriormente o texto de uma resposta cujo carimbo é anterior continuaria fora da contagem; a existência desse caminho em produção é **[nao verificado]**. (`app/chamados/services/vigia_fio.py:380,381`, diff.)

## 29/10 21:03 · sessao e8be9c30180a · - · - · origem=Ronald (tmux)

Achados da parte juiz/vigia:

- **Média — derivação paralela do critério de resposta vazia.** O juiz registrado é `pergunta_viva` (`app/core/juizes.py:232`), cujo corpo exige texto por `(p.resposta_colab or '').strip()` (`app/chamados/juizes.py:1020`). O vigia novo implementa outra definição por regex (`app/chamados/services/vigia_fio.py:381`, pós-diff), contrariando o princípio de contador ler a mesma autoridade (`app/docs/LEIS.md:60`). Deve compartilhar o predicado de texto com o juiz; chamar `pergunta_viva` inteira também seria incorreto, porque vida e vazio são perguntas diferentes (`app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1020`).

- **Média — o marco exato não está provado.** A constante fixa 17:20 (`app/chamados/services/vigia_fio.py:369`, pós-diff), mas o aval registra apenas `17:2x` (`app/docs/CORTES.md:383`). A equivalência entre esse instante e o aval/deploy é **[nao verificado]**. É corte histórico pedido pelo aval, não relógio de negócio por si só; o problema comprovável é a precisão inventada (`app/docs/CORTES.md:383`).

- **Baixa — falta mordida do escritor para resposta só com espaços.** O novo teste do escritor usa apenas `''` e `'07:55'`; remover `.strip()` do guard novo e deixar `bool(q.resposta_colab)` sobreviveria a esses casos (`app/chamados/tests/test_o261_resposta_vazia.py`, anexo, testes `test_sem_texto...` e `test_MORDE_com_texto...`). O teste do vigia com `'   '` não exercita esse escritor (mesmo arquivo, `test_vigia_conta_so_as_novas`). O juiz existente já distingue espaços como vazio (`app/chamados/tests/test_pergunta_viva.py:26`).

Schema/query:

- `resposta_colab` não aceita NULL no schema: `TextField(blank=True, default='')`; logo `isnull=True` é redundante, sem perda funcional por si só (`app/chamados/models.py:1865`).
- A regex com `*` contempla `''`; a filtragem `respondida_em__gte` exclui carimbo NULL e inclui exatamente o marco (`app/chamados/services/vigia_fio.py:380`, pós-diff).
- PostgreSQL é o backend declarado (`app/config/settings/prod.py:11`). A equivalência completa entre `strip()` e `\s` do PostgreSQL/locale não está demonstrada pela cópia; diferenças de Unicode/locale e ocorrência em produção ficam **[nao verificado]**. Não há teste de paridade, tabulação, newline, Unicode ou igualdade ao marco no teste novo (anexo, `test_vigia_conta_so_as_novas`).
- Não achei escritor concreto remanescente que legitimamente gere novo carimbo vazio: irmãos têm guardas por `strip()` (`app/chamados/services/disputa_emissao.py:1219`, `app/chamados/services/disputa_emissao.py:1570`, `app/chamados/services/materializacao.py:511`, `app/chamados/services/materializacao.py:1190`). A prosa antiga que chama carimbo sem texto legítimo não basta para alegar falso alarme novo (`app/chamados/tests/test_contract_par_resposta.py:28`).

Mordida:

- **Sem achado de teste vazio nos três casos de cura/trilha:** o primeiro exige fechamento e ausência do carimbo; o segundo exige fechamento e carimbo; o terceiro exige estado idêntico e uma linha após duas chamadas (anexo `app/chamados/tests/test_o261_resposta_vazia.py`, três testes respectivos).
- O teste do vigia injeta o estado inválido diretamente: certifica o contador, não deve ser apresentado como prova da cura (mesmo arquivo, `test_vigia_conta_so_as_novas`). [nao verificado]
- Execução real dos testes e PostgreSQL: **[nao verificado]**. [nao verificado]

## 29/10 21:02 · sessao 27725cac3b86 · - · - · origem=Ronald (tmux)

Linhas do diff abaixo são pós-aplicação; demais referências são da cópia.

- **P2 — trilha contradiz o estado.** A descrição do novo evento decide se havia resposta por `'respondida_em' in _c`, que identifica apenas carimbo recém-escrito. Se `q.resposta_colab='07:55'` e `q.respondida_em` já existe, o ramo preserva ambos e registra **“sem resposta do colaborador, respondida_em fica nulo”**, enquanto `depois.tem_resposta=True`. A entrada é possível porque `_abertas` só exclui materializadas e validadas. Referências: `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1653` (diff), `app/chamados/services/disputa_emissao.py:1660` (diff).

- **P2 — a garantia nova de uma linha não cobre concorrência.** O ramo lê `_todas`/`_abertas` sem bloqueio ou atualização condicional; duas execuções podem capturar a mesma pergunta aberta, salvar e emitir `fecho_sem_credor` duas vezes. `atomic` dos chamadores não serializa essa leitura; há chamador com `atomic` e nenhuma trava (`fechar_beco`). O novo teste verifica duas chamadas sequenciais apenas. Referências: `app/chamados/services/disputa_emissao.py:1630`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1656` (diff), `app/chamados/services/disputa_emissao.py:1658` (diff), `app/chamados/services/becos.py:41`, `app/chamados/services/becos.py:49`, `app/chamados/tests/test_o261_resposta_vazia.py:97` (diff). Ocorrência concorrente real: **[nao verificado]**.

- **Aval:** a guarda `bool(...strip())` cura a escrita sem texto; mantém `materializada_em`, autor e via, usa o juiz existente `pergunta_viva`, e deixa o fecho da disputa na porta `d.fechar`. O passivo não é alterado por rotina adicionada neste diff. Referências: `app/docs/CORTES.md:383`, `app/chamados/services/disputa_emissao.py:1641`, `app/chamados/services/disputa_emissao.py:1643`, `app/chamados/services/disputa_emissao.py:1652` (diff), `app/chamados/services/disputa_emissao.py:1673` (diff).

- **Segundo escritor de LogAuditoria: sem achado.** O evento entra por `core.observ.evento`, declarado escritor único e implementado por `objects.create`; o ramo novo não escreve o modelo diretamente. Referências: `app/chamados/services/disputa_emissao.py:1657` (diff), `app/core/observ.py:94`, `app/core/observ.py:96`, `app/core/observ.py:134`.

- **Ordem de trava: sem inversão nova identificada; falta serialização descrita acima.** O resolver não acrescenta `select_for_update`; o `evento` abre somente `transaction.atomic` para a criação da trilha. Referências: `app/chamados/services/disputa_emissao.py:1514`, `app/chamados/services/disputa_emissao.py:1630`, `app/core/observ.py:133`.

- **Selos:** não passam integralmente vazios: o caso sem texto exige materialização, autor, via e ausência de carimbo; remover a guarda e voltar ao original viola `assertIsNone`. Remover o evento viola a contagem 1. O caso `MORDE_com_texto` preserva a simetria, mas já seria verde no código anterior, que carimba indiscriminadamente; isso é contraprova útil, não RED da cura. Não há teste novo que exija `d.fechada_em`, via específica, descrição correta da trilha ou concorrência. Referências: `app/chamados/tests/test_o261_resposta_vazia.py:77` (diff), `app/chamados/tests/test_o261_resposta_vazia.py:88` (diff), `app/chamados/tests/test_o261_resposta_vazia.py:97` (diff), `app/chamados/services/disputa_emissao.py:1648` (base). Execução/RED real: **[nao verificado]**.

- **Relógio:** não há leitura nova de `now()` neste ramo; os dois `_tz.now()` já existem no resolver. A regra registrada de injeção nomeia juiz/grade/cartório/reconciliador/fábrica e bordas, então não afirmo automaticamente que este service de escrita viola aquele complemento. Referências: `app/chamados/services/disputa_emissao.py:1561`, `app/chamados/services/disputa_emissao.py:1638`, `app/docs/CORTES.md:369`.

## 29/10 21:01 · sessao 32c02b5a9280 · - · - · origem=Ronald (tmux)

Achados:

- **P2 — recorte `--empresa` contaminado pela frota.** O comando entrega `empresas` ao censo e ao contador irmão, mas a nova chamada `respondida_sem_resposta()` não recebe recorte; a nova função consulta todas as perguntas. Assim uma empresa sem infração pode imprimir contagem de outra e sair com código 2. Referências: `app/chamados/management/commands/vigiar_fio_morto.py:142-160`; `diff_v1.patch`, nova linha 166 do comando e novas linhas 372-382 do serviço. A ocorrência concreta em produção é [nao verificado].

- **P2/P3 — contador novo incompleto no contrato do carimbo/ESTADO.** Ele é guardado no topo do JSON, mas permanece fora de `censo`, `ESPERADO`, `ALARMA_SEMPRE` e `ALARMA_EM_0`. O leitor calcula `fora_do_esperado` exclusivamente de `censo` × `esperado` e não extrai a chave nova: `bin/fio_estado.py:69-92`. O ESTADO mostrará o nome em “a rodada ALARMOU” via `gritou`, mas não o valor medido nem seu esperado: `bin/fio_estado.py:101-119`; `bin/gerar_estado.py:260-261`. Portanto não alegar que o alarme fica totalmente oculto.

- **P3 — ação nova ausente no filtro enumerado da auditoria.** `fecho_sem_credor` não consta em `LogAuditoria.ACOES`: `app/core/models.py:286-296`. O dropdown usa essa lista: `app/core/views.py:738`; `app/templates/core/log_auditoria.html:23-27`. A linha continua na listagem geral, e GET manual com `acao=fecho_sem_credor` funciona pelo filtro livre: `app/core/views.py:705-710`. Não há falha de gravação por `choices`: o campo suporta verbos livres de até 64 caracteres, conforme o próprio comentário em `app/core/models.py:296`, e o escritor chama `objects.create` sem `full_clean`: `app/core/observ.py:133-138`.

Sem achado:

- **Código de saída e consumidor do pipeline:** o diff mantém carimbo antes do `SystemExit(2)` (`app/chamados/management/commands/vigiar_fio_morto.py:174-197`, linhas-base), e o placar já trata exit 2 como ALARME: `app/core/management/commands/pipeline_placar.py:75-79,98-99`.
- **Outros leitores por ações específicas:** histórico consulta apenas transições de Chamado, portanto a ação de PerguntaDisputa não entra por engano: `app/chamados/services/historico.py:44,86-88`; vigia de loop filtra `estado_chamado`: `app/chamados/services/vigia_loop.py:28-32`.
- **Segundo escritor de LogAuditoria:** a chamada nova usa a casa única `evento`, que efetivamente grava em `app/core/observ.py:94-105,133-138`.

Cobertura insuficiente: os testes existentes verificam os grupos declarados e esperado antigos, não a nova integração (`app/chamados/tests/test_o261_vigia_fio.py:643-657,703-707`); o teste novo do anexo chama só a função contadora, nunca o comando (`app/chamados/tests/test_o261_resposta_vazia.py:106-119`, arquivo novo do diff). Remover do comando a impressão, o carimbo e o alarme novos não é detectado por esse teste novo. Execução dos testes e consumidores externos à cópia: [nao verificado].

## 29/10 20:57 · sessao 0d415d34eeb7 · o261 · leitura · origem=Code

O **segundo ramo** de `resolver_perguntas_por_chamado_resolvido` pode preencher `respondida_em` sem resposta e salva sem chamar `lavrar`; o primeiro condiciona esse carimbo ao texto após `strip()` e grava a lavra junto (`chamados/services/disputa_emissao.py:1570`, `chamados/services/disputa_emissao.py:1580`, `chamados/services/disputa_emissao.py:1648`).

Referências abaixo relativas a `app/`, exceto `bin/...`.

**Pergunta 1 — escritores de `respondida_em`**

No esquema declarado, `resposta_colab` aceita vazio, mas não SQL `NULL`: é `TextField(blank=True, default='')`, sem `null=True`. Portanto, os “sim” abaixo já se sustentam com `''` ou espaços; a condição do banco instalado é [nao verificado] (`chamados/models.py:1865`, `chamados/migrations/0014_perguntadisputa.py:24`).

| Função/sítio | Escrita e persistência | Pode gravar carimbo não nulo com texto vazio/espaços? |
|---|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | `update(respondida_em=None, ...)` (`chamados/models.py:1162`). | **Não:** escreve `None` (`chamados/models.py:1163`). |
| `responder_pergunta_disputa_core` | Atribui `now()` e salva o campo (`chamados/services/disputa_emissao.py:1248`, `chamados/services/disputa_emissao.py:1266`). | **Não:** normaliza a resposta com `strip()` e rejeita vazio antes de escrever texto e carimbo (`chamados/services/disputa_emissao.py:1219`, `chamados/services/disputa_emissao.py:1247`). |
| `reabrir_para_correcao` | Monta `campos` e executa `qs.update(**campos)` (`chamados/services/disputa_emissao.py:1358`, `chamados/services/disputa_emissao.py:1377`). | **Não:** grava `respondida_em=None` e `resposta_colab=''` (`chamados/services/disputa_emissao.py:1358`). |
| `responder_automaticamente` | Atribui os argumentos `texto` e `instante`; salva ambos (`chamados/services/disputa_emissao.py:1392`). | **Sim na função:** verifica apenas o texto **anterior**, sem validar o novo `texto`. O caller da foto passa uma constante não vazia e `now()` (`chamados/services/disputa_emissao.py:1390`, `api/foto_recebida.py:25`, `api/foto_recebida.py:42`, `api/foto_recebida.py:48`). |
| `carimbar_resposta_pela_trilha` | Atribui `instante` e salva `respondida_em` (`chamados/services/disputa_emissao.py:1409`). | **Não:** retorna sem escrever quando o texto não passa em `strip()` ou o carimbo já existe (`chamados/services/disputa_emissao.py:1407`). |
| `resolver_perguntas_por_chamado_resolvido`, primeiro ramo | Atribui `agora`, inclui o campo e salva (`chamados/services/disputa_emissao.py:1572`, `chamados/services/disputa_emissao.py:1583`). | **Não:** tanto a atribuição quanto a inclusão em `update_fields` exigem `_tem_resp`, calculado por `strip()` (`chamados/services/disputa_emissao.py:1570`). |
| Mesma função, segundo ramo | Atribui `_agora`, inclui o campo e salva (`chamados/services/disputa_emissao.py:1648`). | **Sim, caminho concreto:** exige somente carimbo nulo nesse teste; não exige conteúdo da resposta. O universo tem validação/materialização nulas e passa por `pergunta_viva(q) == False` (`chamados/services/disputa_emissao.py:1634`, `chamados/services/disputa_emissao.py:1641`, `chamados/services/disputa_emissao.py:1648`). |
| `_materializar_ausencia_de_resposta` | Atribui `agora` sob condição; o `save` inclui `respondida_em` **incondicionalmente** (`chamados/services/materializacao.py:511`, `chamados/services/materializacao.py:528`). | **Não cria** carimbo sem texto, mas **pode regravar um carimbo não nulo preexistente** numa irmã vazia/espaços, pois o campo sempre integra `update_fields` (`chamados/services/materializacao.py:512`, `chamados/services/materializacao.py:529`). |
| `_materializar_uma` | Atribui `agora`, acrescenta o campo e salva (`chamados/services/materializacao.py:1192`, `chamados/services/materializacao.py:1198`). | **Não:** atribuição e inclusão do campo exigem texto após `strip()` (`chamados/services/materializacao.py:1190`). |

Há também a **criação genérica**: `PerguntaDisputa.abrir(..., **campos)` repassa os campos diretamente a `objects.create`. Assim, a função admite `respondida_em=<datetime>` com resposta omitida, cujo padrão é `''` (`chamados/models.py:2028`, `chamados/models.py:2085`, `chamados/models.py:1865`). Os seis callers atuais não fornecem esse carimbo (`chamados/services/acoes_disputa.py:259`, `chamados/services/disputa_emissao.py:389`, `chamados/services/disputa_emissao.py:770`, `chamados/services/disputa_emissao.py:1160`, `chamados/services/declaracao_texto.py:322`, `chamados/services/declaracao_texto.py:611`).

Nos management commands, `curar_perguntas_em_limbo.Command.handle` escreve **por delegação**: com `--apply`, chama `carimbar_resposta_pela_trilha` usando `nota.criado_em` e chama `reabrir_para_correcao` para limpar as demais. A primeira conserva a guarda de texto descrita acima (`chamados/management/commands/curar_perguntas_em_limbo.py:64`, `chamados/management/commands/curar_perguntas_em_limbo.py:67`, `chamados/management/commands/curar_perguntas_em_limbo.py:69`, `chamados/services/disputa_emissao.py:1407`).

Nas migrations de `chamados`, `0014` cria o esquema; as migrations de dados inspecionadas operam sobre `ChamadoColaborador`, não sobre esse par de campos (`chamados/migrations/0014_perguntadisputa.py:16`, `chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4`, `chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5`, `chamados/migrations/0038_a3_registrado_backfill.py:9`, `chamados/migrations/0039_a3_via_backfill.py:12`).

O `bulk_update` de perguntas em `gravar_lavras_em_lote` grava somente `veredito_lavrado`, `julgado_em` e `janela_expira_em`, sem `respondida_em` (`chamados/veredito_lavrado.py:194`, `chamados/veredito_lavrado.py:207`). Outras escritas por `F()`, SQL ou `bulk_update` desse campo não apareceram na busca estática [nao verificado].

**Pergunta 2 — leitores que decidem sem exigir conteúdo da resposta**

Aqui, “sem olhar `resposta_colab`” significa **sem exigir texto na decisão local indicada**. Alguns sítios também consultam vitalidade ou delegam a etapas posteriores; por exemplo, selecionar pelos carimbos não prova que o plantio aceitará uma resposta vazia (`chamados/services/materializacao.py:1286`, `chamados/services/materializacao.py:763`).

| Função | Decisão e referência |
|---|---|
| `PerguntaDisputa.estado` | Após as precedências de validação/materialização, distingue `PENDENTE` de `RESPONDIDA` pela nulidade do carimbo (`chamados/models.py:2108`, `chamados/models.py:2112`, `chamados/models.py:2115`). |
| `PerguntaDisputa.respondida` | Retorna exclusivamente `respondida_em is not None` (`chamados/models.py:2317`). |
| `DisputaSupervisao.pendentes_do_colaborador` | Seleciona perguntas sem carimbo para compor o badge, além de consultar vitalidade (`chamados/models.py:1381`). |
| `perguntas_corrigiveis` | Só carimbadas entram nas candidatas à correção; depois consulta lastro/vitalidade (`chamados/juizes.py:1116`). |
| `relatorios_disputa.classificar` | Entre as vivas, algum carimbo nulo significa `AGUARDA_COLAB`; caso contrário, `AGUARDA_SUPERVISAO` (`chamados/relatorios_disputa.py:17`). |
| `responder_pergunta_disputa` | Define `ja_respondida` pelo carimbo e passa ao core (`chamados/views.py:1280`). |
| `api_disputa_responder` | Mesma decisão sobre `ja_respondida`; a nova entrada é validada separadamente (`api/views.py:2688`). |
| `declarar_hora_colab` | Passa `bool(perg.respondida_em)` como `ja_respondida`; o teste anterior de idempotência também olha o texto (`chamados/services/declaracao_texto.py:363`, `chamados/services/declaracao_texto.py:365`). |
| `api_chamados_v2` | Libera `pode_solicitar_correcao` quando todas têm carimbo, além das demais condições (`api/views.py:1184`). |
| `api_disputa_perguntas` | Usa `.estado == RESPONDIDA` para separar listas e definir `aguarda_admin`; também usa o estado para escolher `chamado_id`. O texto é apenas serializado nessa decisão (`api/views.py:2621`, `api/views.py:2632`, `api/views.py:2642`). |
| `explicar_pergunta` | `.estado` determina dono colaborador/admin e ação cobrar/validar; o texto apenas compõe a frase depois (`chamados/services/explicador.py:72`). |
| `folha._fio_do_dia` | Entre vivas, estado `PENDENTE` atribui o dia ao colaborador; as demais atribuem ao admin (`folha/fora_da_folha.py:66`). |
| `reabrir_disputa_supervisao` | Forma `_sem_resp` pelo carimbo nulo; decide recobrança ou aviso considerando também vitalidade (`chamados/views.py:1416`, `chamados/views.py:1421`, `chamados/views.py:1431`). |
| `validar_inline` | Carimbadas vivas vão para validação; as sem carimbo compõem “aguardando colaborador” (`chamados/views_cobrar.py:169`, `chamados/views_cobrar.py:175`, `chamados/views_cobrar.py:192`). |
| `validar_pergunta` | Gate inicial retorna `sem_resposta` quando falta carimbo (`chamados/services/validacao.py:356`). |
| `recusar_pergunta` | Sem carimbo retorna `ja_pendente`; com carimbo prossegue para recusa/reabertura (`chamados/services/validacao.py:530`, `chamados/services/validacao.py:540`). |
| `materializar_perguntas_validadas_da_disputa` | Seleciona candidatas pelos carimbos respondida/validada/materializada, sem filtro de conteúdo nessa seleção (`chamados/services/materializacao.py:1286`). |
| `aplicar_declaracao` | Decide chamar o core de resposta somente quando falta carimbo; depois chama validação (`chamados/services/declaracao_texto.py:632`, `chamados/services/declaracao_texto.py:641`). |
| `resolver_perguntas_por_chamado_resolvido`, segundo ramo | Decide preencher o carimbo ausente sem verificar texto (`chamados/services/disputa_emissao.py:1648`). |
| `acao_reabrir_disputa` | Conta carimbos nulos para responder que o questionário já aguarda resposta (`chamados/services/acoes_disputa.py:95`). |
| `acao_regularizar_disputa` | Bloqueia regularização se alguma viva não tem carimbo (`chamados/services/acoes_disputa.py:130`). |
| `processar_post_fio` | Repete esse bloqueio de regularização (`chamados/services/fio.py:654`). |
| `montar_chamados_fio` | Calcula `todas_respondidas` e disponibilidade de solicitar correção por `all(p.respondida_em ...)` (`chamados/services/fio.py:336`). |
| `perguntas_pendentes_do_colab` | Seleciona vivas sem carimbo, cuja presença dispara o aviso de questionário pendente (`chamados/services/aviso_pendencia.py:13`, `chamados/services/aviso_pendencia.py:17`). |
| `becos.classificar` | Carimbo nulo, combinado com destino não resolvido, determina pergunta visível e participa da classificação de beco (`chamados/services/becos.py:29`). |
| `cobranca._calados` | Recorta perguntas sem carimbo para contar pessoas vencidas/sem canal (`chamados/services/cobranca.py:229`, `chamados/services/cobranca.py:237`). |
| `furo_sem_canal.medir` | Trata carimbo nulo como pergunta aberta na cobertura da célula (`chamados/services/furo_sem_canal.py:80`, `chamados/services/furo_sem_canal.py:85`). |
| `creditos_perguntas` | Separa respondidas/não respondidas pelo carimbo e atribui créditos aos chamados (`chamados/services/raiox.py:133`, `chamados/services/raiox.py:137`, `chamados/services/raiox.py:149`). |
| `pode_executar_csf` | Qualquer pergunta carimbada na disputa ou nas resolvedoras impede execução automática (`ponto/supra_juiz.py:483`, `ponto/supra_juiz.py:487`). |
| `reconciliar_grade.Command._relavrar_orfa` | Pergunta carimbada e viva bloqueia a relavra com `ORFA_FIO_PRESO` (`ponto/management/commands/reconciliar_grade.py:452`). |
| `auditar_invariantes_chamados.Command.handle` | Carimbo presente e disputa aberta formam o piso I2, excluído da violação; carimbo nulo integra a seleção C2 (`chamados/management/commands/auditar_invariantes_chamados.py:590`, `chamados/management/commands/auditar_invariantes_chamados.py:1597`). |
| `_contraprova_i21a` | Ausência do carimbo pode gerar alarme; neste caller, a entrada já excluiu texto vazio (`chamados/management/commands/auditar_invariantes_chamados.py:177`, `chamados/management/commands/auditar_invariantes_chamados.py:1571`). |
| `curar_perguntas_em_limbo.Command.handle` | Na limpeza, seleciona por PK e carimbo nulo antes de delegar à reabertura; a medição inicial do comando também examina texto (`chamados/management/commands/curar_perguntas_em_limbo.py:46`, `chamados/management/commands/curar_perguntas_em_limbo.py:69`). |

Também existem decisões de métricas, histórico e cache:

| Função | Uso do carimbo sem exigir texto |
|---|---|
| `recobrar_questionario` | Conta pendentes pelo carimbo nulo e usa a contagem na trilha/retorno (`chamados/services/disputa_emissao.py:1444`, `chamados/services/disputa_emissao.py:1453`, `chamados/services/disputa_emissao.py:1470`). |
| `placar_aceite` | Conta respostas posteriores à cobrança e respostas de hoje pela data do carimbo (`chamados/services/verbos.py:306`, `chamados/services/verbos.py:315`). |
| `marcador_do_placar` | `Max('respondida_em')` integra o marcador usado no ETag (`chamados/services/painel.py:66`, `chamados/services/painel.py:91`). |
| `marcos_do_lote` / `esta_stale` | O maior carimbo por colaborador integra o marco comparado com `julgado_em` (`chamados/veredito_lavrado.py:166`, `chamados/veredito_lavrado.py:187`). |
| `_eventos_dos_chamados` | Presença/data do carimbo decide emitir evento `pergunta_respondida`; o texto só compõe a legenda, inclusive “sem resposta” (`chamados/services/historico.py:99`, `chamados/services/historico.py:105`, `chamados/services/historico.py:109`). |
| `fila_validar_classificada` | Calcula idade e estatísticas pelo carimbo; a seleção padrão vem de outra função (`chamados/services/validacao.py:867`, `chamados/services/validacao.py:870`, `chamados/services/validacao.py:873`). |
| `_item_validar` | Calcula `dias_esperando` diretamente pelo carimbo (`api/views_mensageria.py:570`). |
| Script operacional `bin/censo_folha.py` | Conta nulas como `PERG_VIVAS` e carimbadas não validadas como `AGUARDA_ADMIN` (`bin/censo_folha.py:28`, `bin/censo_folha.py:37`). |

Nos templates:

| Template | Decisão sem guarda de conteúdo |
|---|---|
| `templates/chamados/partials/modal_fio.html` | Ausência do carimbo permite corrigir/apagar pergunta cega; presença abre o bloco de resposta e ações de validação; `todas_respondidas` decide cobrar versus regularizar (`templates/chamados/partials/modal_fio.html:182`, `templates/chamados/partials/modal_fio.html:210`, `templates/chamados/partials/modal_fio.html:272`, `templates/chamados/partials/modal_fio.html:327`). |
| `templates/chamados/partials/_pergunta_card.html` | Carimbo define “aguardando validação” e habilita o bloco de resposta/ações, com guardas adicionais de validação/permissão (`templates/chamados/partials/_pergunta_card.html:24`, `templates/chamados/partials/_pergunta_card.html:29`, `templates/chamados/partials/_pergunta_card.html:42`). |
| `templates/chamados/partials/_card_atendimento.html` | Carimbo mostra “Resposta enviada”, escolhe “Reenviar”/“Enviar” e mostra selo “Enviado” nas perguntas de hora (`templates/chamados/partials/_card_atendimento.html:94`, `templates/chamados/partials/_card_atendimento.html:155`, `templates/chamados/partials/_card_atendimento.html:384`). |

Outros consumidores de `.estado`, incluídos pelo escopo literal da pergunta, consultam somente estados finais: `processar_post_fio` usa `VALIDADA/RESOLVIDA` para registrar trilha, e o card usa `RESOLVIDA` para o rótulo “Encerrado”. Essas saídas vêm dos marcos finais e da vitalidade, sem usar a distinção `PENDENTE/RESPONDIDA` (`chamados/services/fio.py:608`, `templates/chamados/partials/_card_atendimento.html:183`, `chamados/models.py:2108`).

Não entram como leitores sem conteúdo: `_pergunta_viva_calc` exige `strip()`; `q_vivas` examina texto vazio/nulo; `perguntas_respondidas_a_validar` exclui `''`; `irmas_respondidas_sem_consumo` exige `strip()` no resultado. Os testes SQL de `''` não equivalem a rejeitar espaços (`chamados/juizes.py:1020`, `chamados/models.py:2246`, `chamados/juizes.py:1123`, `chamados/juizes.py:1477`). `respostas_sem_veredito` também olha o texto, embora aceite **carimbo OU texto** (`chamados/models.py:1430`).

**Pergunta 3 — diferença entre os dois ramos e dependência da lavra**

| Campo/ato | Primeiro ramo: perguntas apontadas ao chamado | Segundo ramo: disputa sem credor vivo |
|---|---|---|
| Seleção | Destino igual ao chamado, materialização nula e disputa aberta; pula pergunta viva e dia trancado (`chamados/services/disputa_emissao.py:1536`, `chamados/services/disputa_emissao.py:1547`, `chamados/services/disputa_emissao.py:1551`). | Pai e destinos sem credor vivo; escolhe materialização/validação nulas e pula pergunta viva (`chamados/services/disputa_emissao.py:1627`, `chamados/services/disputa_emissao.py:1634`, `chamados/services/disputa_emissao.py:1641`). |
| `materializada_em`, `materializada_por`, `via_resolucao` | Grava os três (`chamados/services/disputa_emissao.py:1561`). | Grava os três (`chamados/services/disputa_emissao.py:1643`). |
| `respondida_em` | Preenche se há texto após `strip()` **e** o campo está nulo (`chamados/services/disputa_emissao.py:1570`). | Preenche se está nulo, **independentemente do texto** (`chamados/services/disputa_emissao.py:1648`). |
| `validada_em`, `validada_por` | Preenche se há texto após `strip()` e validação nula (`chamados/services/disputa_emissao.py:1574`). | Não os inclui na escrita local; as candidatas têm `validada_em=None` (`chamados/services/disputa_emissao.py:1635`, `chamados/services/disputa_emissao.py:1647`). |
| Lavra | Chama `lavrar(p, salvar=False)` e salva os três campos da lavra junto (`chamados/services/disputa_emissao.py:1580`). | Salva apenas os campos listados nesse ramo, sem chamar `lavrar` (`chamados/services/disputa_emissao.py:1647`). |
| `resposta_colab` | Não integra os campos gravados (`chamados/services/disputa_emissao.py:1565`, `chamados/services/disputa_emissao.py:1582`). | Não integra os campos gravados (`chamados/services/disputa_emissao.py:1647`). |

Os três campos da lavra **não exigem `respondida_em` preenchido**:

- `veredito_lavrado` recebe `SIM/NÃO` conforme `_pergunta_materializada_de_fato`; numa exceção, recebe `INDETERMINADO` (`chamados/veredito_lavrado.py:219`, `chamados/veredito_lavrado.py:226`). Esse juiz consulta materialização, via, tipo, fatos externos e, conforme o caminho, o texto/slot; não usa o carimbo de resposta como requisito (`chamados/juizes.py:603`, `chamados/juizes.py:628`, `chamados/juizes.py:674`, `chamados/juizes.py:733`).
- `julgado_em` recebe `timezone.now()` na lavratura (`chamados/veredito_lavrado.py:228`).
- `janela_expira_em` recebe `expira_em(p.data_evento)`, calculado pela data do evento, relógio e janelas de competência (`chamados/veredito_lavrado.py:229`, `chamados/veredito_lavrado.py:109`).

Existe uma dependência **de atualização do cache**: `respondida_em` participa do máximo por colaborador em `marcos_do_lote`; quando esse marco supera `julgado_em`, `esta_stale` considera a lavra desatualizada. Isso é diferente de exigir resposta carimbada para calcular ou gravar os três campos (`chamados/veredito_lavrado.py:166`, `chamados/veredito_lavrado.py:187`).

## 29/10 20:54 · sessao 470cff31a54a · - · - · origem=Ronald (tmux)

Inventário Python de leitores. “Sem olhar texto” abaixo significa que **a decisão local** usa carimbo/estado sem exigir `resposta_colab` preenchida; onde há juiz posterior, isso não prova que a operação inteira aceita resposta vazia.

| Arquivo:linha | Função | Decisão local |
|---|---|---|
| `app/chamados/models.py:2112`, `:2115` | `PerguntaDisputa.estado` | Distingue `PENDENTE`/`RESPONDIDA` pela nulidade do carimbo; prioridade de validação/materialização está em `:2108-2115`. |
| `app/chamados/models.py:2317-2318` | `PerguntaDisputa.respondida` | Booleano exclusivamente `respondida_em is not None`. |
| `app/chamados/models.py:1381-1385` | `DisputaSupervisao.pendentes_do_colaborador` | Seleciona disputas para badge usando carimbo nulo, prefilter e `pergunta_viva`. |
| `app/chamados/juizes.py:1114-1118` | `perguntas_corrigiveis` | Só carimbadas entram no conjunto corrigível; depois verifica lastro/vitalidade. |
| `app/chamados/relatorios_disputa.py:17-22` | `classificar` | Entre vivas, alguma sem carimbo → `AGUARDA_COLAB`; demais → `AGUARDA_SUPERVISAO`. |
| `app/chamados/views.py:1280-1283` | `responder_pergunta_disputa` | Define `ja_respondida` só pelo carimbo e passa ao core. |
| `app/chamados/views.py:1416-1433` | `reabrir_disputa_supervisao` | Carimbo nulo forma `_sem_resp`; vivas geram recobrança, não vivas geram aviso. |
| `app/api/views.py:1184-1188` | `api_chamados_v2` | Libera `pode_solicitar_correcao` quando todas as perguntas têm carimbo, além das demais guardas. |
| `app/api/views.py:2621-2624`, `:2642-2644` | `api_disputa_perguntas` | `.estado == RESPONDIDA` separa listas, marca `aguarda_admin` e influencia chamado escolhido. |
| `app/api/views.py:2688-2695` | `api_disputa_responder` | Define `ja_respondida` só pelo carimbo; valida a **nova entrada** separadamente. |
| `app/chamados/views_cobrar.py:169-176`, `:192-199` | `validar_inline` | Carimbadas vivas vão à validação; carimbo nulo conta como aguardando colaborador. |
| `app/chamados/services/validacao.py:356-358` | `validar_pergunta` | Gate inicial devolve `sem_resposta` se não há carimbo; validação/materialização posterior pode examinar texto (`:367`, `:375`). |
| `app/chamados/services/validacao.py:530-540` | `recusar_pergunta` | Sem carimbo → `ja_pendente`; com carimbo executa recusa/reabertura, sem guarda de conteúdo. |
| `app/chamados/services/materializacao.py:1286-1293` | `materializar_perguntas_validadas_da_disputa` | Seleciona candidatas pelos carimbos respondida/validada/materializada; delega unidade depois. |
| `app/chamados/services/declaracao_texto.py:365` | `declarar_hora_colab` | Passa `bool(perg.respondida_em)` como `ja_respondida` ao core; o gate de idempotência imediatamente anterior **olha texto** (`:363`). |
| `app/chamados/services/declaracao_texto.py:632-641` | `aplicar_declaracao` | Só chama resposta do core se carimbo ainda não existe; depois chama validação. |
| `app/chamados/services/disputa_emissao.py:1648-1650` | `resolver_perguntas_por_chamado_resolvido` | No segundo ramo, decide preencher carimbo ausente sem guarda de texto. |
| `app/chamados/services/acoes_disputa.py:95-101` | `acao_reabrir_disputa` | Conta nulas para devolver “aguardando resposta” quando não há corrigíveis. |
| `app/chamados/services/acoes_disputa.py:130-131` | `acao_regularizar_disputa` | Bloqueia regularização quando alguma viva tem carimbo nulo. |
| `app/chamados/services/fio.py:336-338` | `montar_chamados_fio` | Define todas respondidas e disponibilidade de solicitar correção só pelos carimbos. |
| `app/chamados/services/fio.py:608-611` | `processar_post_fio` | `.estado` VALIDADA/RESOLVIDA autoriza registro de trilha após validação. Esse ramo não depende especificamente de `respondida_em`; deriva dos carimbos finais (`models.py:2108-2109`). |
| `app/chamados/services/fio.py:654-655` | `processar_post_fio` | Bloqueia regularização quando alguma viva não tem carimbo. |
| `app/chamados/services/aviso_pendencia.py:7-13` | `perguntas_pendentes_do_colab` | Seleciona vivas sem carimbo; presença dispara aviso em `:16-24`. |
| `app/chamados/services/becos.py:29-33` | `classificar` | Carimbo nulo e destino não resolvido determinam pergunta visível; isso participa da classificação de beco. |
| `app/chamados/services/cobranca.py:229-248` | `_calados` | Recorta carimbo nulo e conta pessoas vencidas/sem canal. |
| `app/chamados/services/furo_sem_canal.py:80-86` | `medir` | Carimbo nulo significa pergunta aberta; células sem essa cobertura e sem chamado vivo são “sem canal”. |
| `app/chamados/services/raiox.py:133-149` | `creditos_perguntas` | Divide vivas em respondidas/não respondidas pelo índice contendo carimbo e retorna créditos de chamados distintos. |
| `app/chamados/services/verbos.py:306-319` | `placar_aceite` | Conta respostas depois da cobrança e no dia usando só datas do carimbo. |
| `app/chamados/services/painel.py:64-68` | `marcador_do_placar` | `Max(respondida_em)` integra marcador de mudança/ETag. |
| `app/chamados/veredito_lavrado.py:166-169` | `marcos_do_lote` | `Max(respondida_em)` por colaborador integra marcos; marco maior que `julgado_em` torna lavra stale em `:187-190`. |
| `app/chamados/services/historico.py:99-114` | `_eventos_dos_chamados` | Presença/data do carimbo decide emitir evento “pergunta_respondida”; texto só compõe legenda e pode virar “sem resposta”. |
| `app/ponto/supra_juiz.py:479-490` | `pode_executar_csf` | Qualquer pergunta carimbada no pai ou nas resolvedoras impede execução automática. |
| `app/ponto/management/commands/reconciliar_grade.py:452-454` | `Command._relavrar_orfa` | Pergunta carimbada e viva impede relavra → `ORFA_FIO_PRESO`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:176-181` | `_contraprova_i21a` | Falta de carimbo ou de ausência aprovada gera alarme. O conjunto de entrada I21 **já excluiu texto vazio** em `:1568-1574`. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:590-598` | `Command.handle` | Carimbo presente + disputa aberta define piso I2 e exclui da violação I2. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1597-1601` | `Command.handle` | C2 exige carimbo nulo para contar passivo invisível morto pela célula. |
| `app/folha/fora_da_folha.py:59-73` | `_fio_do_dia` | `.estado == PENDENTE` atribui dono `colab`; demais vivas atribuem `admin`. |
| `bin/censo_folha.py:28-29`, `:37-38` | módulo operacional | Conta nulas como `PERG_VIVAS` e carimbadas não validadas como `AGUARDA_ADMIN`. |

Fronteiras importantes:

- `app/api/views.py:1007-1010` apenas atribui queryset lazy `_pend`; nenhum uso de `_pend` existe no arquivo [não verificado para caminhos de execução externos]. Não incluir como decisão efetiva.
- `_pergunta_viva_calc` exige carimbo **e texto strip** (`app/chamados/juizes.py:1020`); `q_vivas` exige carimbo e texto não vazio/nulo (`app/chamados/models.py:2246-2247`); `perguntas_respondidas_a_validar` exclui `resposta_colab=''` (`app/chamados/juizes.py:1123-1127`); `irmas_respondidas_sem_consumo` exige strip no resultado (`:1471-1477`).
- `respostas_sem_veredito` verifica **carimbo OU texto strip**, portanto não é leitor exclusivamente de carimbo (`app/chamados/models.py:1428-1432`).
- `_chave_memo_pv` usa carimbo **e texto** (`app/chamados/juizes.py:768-769`).
- `explicar_pergunta` decide `sem_resposta`/`aguarda_validacao` por `.estado`; usa texto apenas na frase, sem guarda de conteúdo (`app/chamados/services/explicador.py:72-81`). Acrescentar ao inventário acima.
- `vigia_fio.py:279` compara `.estado == VALIDADA`, mas `_e_aceite_de_folga` examina resposta; excluir do recorte estrito de decisões sem olhar texto.
- `_item_validar` apenas serializa carimbo e calcula idade (`app/api/views_mensageria.py:569-570`), e `fila_validar_classificada` apenas calcula idade/estatísticas pelo carimbo (`app/chamados/services/validacao.py:870-874`); podem ser listados separadamente como apresentação/métrica, sem gate de negócio.

## 29/10 20:54 · sessao 97455eae5552 · - · - · origem=Ronald (tmux)

Inventário consolidado dos indiretos/templates:

| Arquivo:linha | Decisão sem validar `resposta_colab` |
|---|---|
| `app/chamados/models.py:2108-2115` — `PerguntaDisputa.estado` | Após precedência de validação/materialização, distingue `PENDENTE`/`RESPONDIDA` apenas pela nulidade de `respondida_em`. |
| `app/chamados/models.py:2317-2318` — `respondida` | Retorna apenas `respondida_em is not None`. Consumidores desta propriedade não identificados [não verificado]. |
| `app/api/views.py:2621-2624` — `api_disputa_perguntas` | Separa listas pendentes/respondidas e define `aguarda_admin` por `.estado == RESPONDIDA`; texto é somente serializado em `:2632`. |
| `app/api/views.py:2642-2644` — mesma função | Escolhe `chamado_id` priorizando pergunta cujo estado difere de `RESPONDIDA`. |
| `app/chamados/services/explicador.py:72-81` — `explicar_pergunta` | Decide dono colaborador/admin por `PENDENTE`/`RESPONDIDA`; texto apenas compõe a mensagem depois da decisão em `:79-80`. |
| `app/folha/fora_da_folha.py:66-70` — `_fio_do_dia` | Pergunta viva com `.estado == PENDENTE` atribui dia ao colaborador; demais vivas atribuem ao admin. |
| `app/templates/chamados/partials/modal_fio.html:182` | `pergunta_cega` sem timestamp oferece corrigir/apagar (`:185-197`). |
| `app/templates/chamados/partials/modal_fio.html:210` | Timestamp permite bloco de resposta/status e ações; ausência mostra “aguardando resposta” (`:311-312`). Botões de validar/rejeitar/corrigir ficam dentro desse ramo (`:272-305`). |
| `app/templates/chamados/partials/modal_fio.html:327` | Derivado `chamado.todas_respondidas` decide cobrar versus regularizar (`:333-352`); origem é `all(p.respondida_em ...)` em `app/chamados/services/fio.py:336-338`. |
| `app/templates/chamados/partials/_pergunta_card.html:24` | Define “aguardando validação” versus “aguardando colaborador”. |
| `app/templates/chamados/partials/_pergunta_card.html:29` | Timestamp habilita bloco resposta e, adicionalmente a `not validada_em and pode_agir` em `:42`, botões validar/rejeitar (`:92-117`). |
| `app/templates/chamados/partials/_card_atendimento.html:94` | Mostra confirmação “Resposta enviada” (`:99-101`). |
| `app/templates/chamados/partials/_card_atendimento.html:155` | Escolhe “Reenviar resposta” versus “Enviar resposta”. |
| `app/templates/chamados/partials/_card_atendimento.html:384` | Pergunta de hora com timestamp mostra selo “Enviado” (`:385`). |
| `bin/censo_folha.py:28-29` | Conta perguntas pendentes/respondidas sem texto; usa as contagens para rotular `PERG_VIVAS`/`AGUARDA_ADMIN` em `:37-38`. Incluir se “produção” abranger scripts fora de `app/`. |

Leitores de `.estado` cujo resultado consultado não depende de `respondida_em`:

- `app/chamados/services/fio.py:608-611`, `processar_post_fio`: registra log quando estado é `VALIDADA`/`RESOLVIDA`; estas saídas derivam dos marcos de validação/materialização em `app/chamados/models.py:2108-2114`.
- `app/templates/chamados/partials/_card_atendimento.html:183`: estado `resolvida` escolhe rótulo “Encerrado”, pelos mesmos marcos (`app/chamados/models.py:2108-2114`).

Exclusões relevantes:

- `_card_atendimento.html:137` já está sob guarda de conteúdo `if p.resposta_colab` em `:135`.
- `app/chamados/services/vigia_fio.py:279` combina `.estado == VALIDADA` com `_e_aceite_de_folga`; esse helper testa `resposta_colab.strip()` em `app/chamados/services/validacao.py:216-218`.
- Nenhum consumidor JS local de `estado='respondida'`, `aguarda_admin` ou lista `respondidas` foi identificado [não verificado].

## 29/10 20:54 · sessao 0a5bcdeadbf5 · - · - · origem=Ronald (tmux)

P1 — inventário dos **9 sítios explícitos** de escrita, mais criação genérica:

| Função | Escrita e persistência | Carimbo não nulo com texto vazio/espaços? |
|---|---|---|
| `ChamadoColaborador.ressuscitar_perguntas` | `alvo.update(... respondida_em=None, ...)`: `app/chamados/models.py:1162–1163`; função em `:1122` | **Não:** grava `None`, sem criar carimbo (`app/chamados/models.py:1163`). |
| `responder_pergunta_disputa_core` | Atribuição `p.respondida_em = _tz.now()` em `app/chamados/services/disputa_emissao.py:1248`; texto em `:1247`, `save(update_fields=...)` em `:1266` | **Não:** normaliza `(resposta or '').strip()` e recusa vazio antes da escrita (`app/chamados/services/disputa_emissao.py:1219–1221`). |
| `reabrir_para_correcao` | Dicionário `respondida_em=None` em `app/chamados/services/disputa_emissao.py:1358`, usado por `qs.update(**campos)` em `:1377`; função em `:1327` | **Não:** zera carimbo e texto juntos (`app/chamados/services/disputa_emissao.py:1358`). |
| `responder_automaticamente` | Texto e instante recebidos são atribuídos em `app/chamados/services/disputa_emissao.py:1392–1393`, salvos em `:1394`; função em `:1381` | **Sim como API da função:** só verifica texto **anterior**, em `:1390`; não valida `texto` novo (`:1392`). **Único caller encontrado passa texto constante não vazio**, `_TEXTO` em `app/api/foto_recebida.py:25`, chamada em `:48`, instante `now()` em `:42`. |
| `carimbar_resposta_pela_trilha` | Atribui instante em `app/chamados/services/disputa_emissao.py:1409`, salva em `:1410`; função em `:1398` | **Não:** retorna se carimbo já existe **ou** texto não passa `.strip()` (`app/chamados/services/disputa_emissao.py:1407–1408`). |
| `resolver_perguntas_por_chamado_resolvido`, primeiro ramo | `p.respondida_em = agora` em `app/chamados/services/disputa_emissao.py:1572`, campo incluído em `:1573`, salva em `:1583`; função em `:1514` | **Não cria carimbo sem texto:** `_tem_resp = bool((p.resposta_colab or '').strip())` em `:1570`, condição em `:1571`. |
| Mesma função, segundo ramo | `q.respondida_em = _agora` em `app/chamados/services/disputa_emissao.py:1649`, campo em `:1650`, salva em `:1651` | **Sim, caminho concreto:** exige apenas `respondida_em is None`, **não** consulta `resposta_colab` (`app/chamados/services/disputa_emissao.py:1648–1651`). Seleção exige materialização e validação nulas (`:1634–1635`), sem credor vivo (`:1627–1633`) e `pergunta_viva(q)` falso (`:1641–1642`). |
| `_materializar_ausencia_de_resposta` | Atribui `q.respondida_em = agora` em `app/chamados/services/materializacao.py:513`, salva em `:528–530`; função em `:478` | **Não cria** carimbo se texto vazio/espaços: teste `.strip()` e condição em `:511–512`. **Pode regravar carimbo não nulo preexistente com texto vazio**, porque `'respondida_em'` entra **incondicionalmente** em `update_fields` (`app/chamados/services/materializacao.py:528–530`). |
| `_materializar_uma` | Atribui `p.respondida_em = agora` em `app/chamados/services/materializacao.py:1192`, inclui campo em `:1193`, salva em `:1198`; função em `:1063` | **Não:** inclusão depende do texto com `.strip()` e carimbo nulo (`app/chamados/services/materializacao.py:1190–1193`). |

Criação genérica: `PerguntaDisputa.abrir(..., **campos)` (`app/chamados/models.py:2028`) repassa `**campos` diretamente a `objects.create` (`app/chamados/models.py:2085–2086`). Logo, **a API admite** `respondida_em=<datetime>` com `resposta_colab` omitido/padrão `''` (`app/chamados/models.py:1865`); não há validação desse par na função (`app/chamados/models.py:2036–2086`). Nos seis callers atuais não se passa timestamp: `app/chamados/services/acoes_disputa.py:259–264`; `app/chamados/services/disputa_emissao.py:389–397`, `:770–778`, `:1160–1167`; `app/chamados/services/declaracao_texto.py:322–323`, `:611–612`. O único `**kw` desses callers contém somente `chamados_relacionados_ids` (`app/chamados/services/disputa_emissao.py:767–769`).

Management command: `curar_perguntas_em_limbo.Command.handle` delega a `carimbar_resposta_pela_trilha`, passando `nota.criado_em` (`app/chamados/management/commands/curar_perguntas_em_limbo.py:67–68`); delega limpeza a `reabrir_para_correcao` (`:69`); só executa com `--apply` (`:64–66`). Não é escritor independente, e a guarda de `.strip()` permanece na porta (`app/chamados/services/disputa_emissao.py:1407`).

Migration: `0014_perguntadisputa` declara campo nullable, **não** executa escrita de dados (`app/chamados/migrations/0014_perguntadisputa.py:16–25`). As quatro migrations de dados da pasta operam sobre `ChamadoColaborador`, não `PerguntaDisputa`: `app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4–10`, `:13–17`; `0024_rebaixar_urgencia_resolvidos_regressao.py:5–11`, `:15–19`; `0038_a3_registrado_backfill.py:9–15`; `0039_a3_via_backfill.py:12–28`, `:32–34`.

O `bulk_update` de perguntas encontrado grava apenas `CAMPOS_DA_LAVRA = ['veredito_lavrado', 'julgado_em', 'janela_expira_em']`, portanto **não** escreve `respondida_em` (`app/chamados/veredito_lavrado.py:194`, `:207`). Nenhum `F()` ou outro `bulk_update` de `respondida_em` apareceu na varredura [não verificado por execução; auditoria estática].

`resposta_colab` não admite SQL NULL no esquema declarado: `TextField(blank=True, default='')`, sem `null=True` (`app/chamados/models.py:1865`, migration `app/chamados/migrations/0014_perguntadisputa.py:24`). Assim, os riscos concretos descritos acima se sustentam com `''` ou espaços; persistência efetiva de `None` depende do banco real [não verificado].

## 29/10 19:02 · sessao 80de480f740f · espera · verificador · origem=Code

**Reprovo: há caminhos que aceitam identidade falsa de produtor e liberam o Stop.** A liberação ocorre quando a prova retorna motivo vazio (`bin/hook_stop_fila1.py:775`).

1. **Opção do interpretador aceita como produtor.** Entrada: `ESPERANDO: -c | pid=N`, com N executando `python3 -c 'import time; time.sleep(30)'`. O código compara o produtor com `argv[1]` sem distinguir script de opção; aceita `-c` e libera um interpretador apenas esperando. Reproduzido sem escrita. O caso `valor-de-opcao` declara `produtor_espera.sh`, portanto não morde esta variante (`bin/hook_stop_fila1.py:487`, `bin/hook_stop_fila1.py:490`, `bin/tests/test_hook_esperando_com_prova.sh:155`).

2. **Alias de caminho contorna a rejeição de produtor genérico.** Entrada: `ESPERANDO: /usr/bin/sleep/ | pid=N`, com N executando `/usr/bin/sleep 30`. A rejeição usa basename lexical; a identidade usa `realpath`. `/usr/bin/sleep` bloqueia, mas `/usr/bin/sleep/`, `/usr/bin/sleep/.` e `/proc/N/exe` passam. Reproduzido sem escrita: um temporizador vira prova de produtor. O selo não exercita esses aliases (`bin/hook_stop_fila1.py:444`, `bin/hook_stop_fila1.py:492`, `bin/hook_stop_fila1.py:498`, `bin/tests/test_hook_esperando_com_prova.sh:157`).

3. **Mudança de diretório atribui o script ao caminho errado.** Entrada: iniciar `python3 produtor.py` em `/a`; o script muda para `/b`; declarar `ESPERANDO: /b/produtor.py | pid=N`. O hook resolve o argumento relativo contra o diretório **atual**, aceitando `/b/produtor.py`; a declaração verdadeira `/a/produtor.py` pode bloquear. Reproduzido com `/bin/bash dev/stdin`, iniciado em `/` e depois mudando para `/tmp`: `/tmp/dev/stdin` inexistente foi aceito. O selo verifica caminho diferente, mas não mudança de diretório (`bin/hook_stop_fila1.py:489`, `bin/tests/test_hook_esperando_com_prova.sh:149`).

4. **Inode reciclado transforma primeira observação em avanço.** Entrada: memória `{"1:123":10}` de arquivo removido; outro arquivo estático recebe dispositivo/inode `1:123` e `mtime_ns=20`; seu criador já terminou. `ESPERANDO: agente-x | arquivo=/tmp/sinal` libera na primeira observação desse novo arquivo. A memória identifica somente dispositivo/inode e conserva entradas antigas. Retorno vazio confirmado com E/S simulada; reciclagem real neste host: **[nao verificado]**. É troca de identidade, distinta do residual aceito de avanço antigo do mesmo arquivo (`bin/hook_stop_fila1.py:565`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:588`, `bin/hook_stop_fila1.py:605`).

5. **Selo dá falso vermelho quando PID 1 é interpretador.** Com `/proc/1/exe=/usr/bin/python3.12`, o caso monta `ESPERANDO: python3.12 | pid=1` e exige `ancestral`. O hook retorna antes `produtor generico`, bloqueando corretamente, mas reprovando o selo. Confirmado chamando a função em memória (`bin/tests/test_hook_esperando_com_prova.sh:158`, `bin/hook_stop_fila1.py:498`, `bin/hook_stop_fila1.py:505`).

6. **Caso de memória ocupada depende de duração, sem sincronização.** A trava dura três segundos; o teste espera meio segundo e inicia o hook. Se o checador anterior demorar até a trava expirar, a prova encontra memória livre e pode liberar corretamente, fazendo o selo reprovar. Também falta confirmação de que o `flock` adquiriu a trava antes da avaliação. Contraexemplo por análise; execução com atraso: **[nao verificado]** (`bin/tests/test_hook_esperando_com_prova.sh:170`, `bin/tests/test_hook_esperando_com_prova.sh:171`, `bin/tests/test_hook_esperando_com_prova.sh:172`, `bin/hook_stop_fila1.py:745`, `bin/hook_stop_fila1.py:572`).

Nas classes solicitadas:

- **Segundo juiz que duplica juiz registrado:** nada; o leitor do topo foi compartilhado (`bin/hook_stop_fila1.py:400`, `bin/hook_stop_fila1.py:678`).
- **Segundo escritor fora da porta:** nada identificado; a memória nova é gravada na própria prova (`bin/hook_stop_fila1.py:589`).
- **Relógio lido fora da borda:** nada novo identificado; a prova lê o mtime do sinal (`bin/hook_stop_fila1.py:566`).
- **Derivação paralela de fonte única:** nada adicional; o checador do RELATO continua sendo chamado (`bin/hook_stop_fila1.py:745`).
- **Selo/RED sem mordida por ausência de sinal:** nada nos verificadores existentes; positivos exigem liberação atribuída ao ESPERANDO e negativos exigem bloqueio mais motivo. Faltam os contraexemplos 1–4 (`bin/tests/test_hook_esperando_com_prova.sh:74`, `bin/tests/test_hook_esperando_com_prova.sh:84`, `bin/tests/test_hook_esperando_com_prova.sh:87`).
- **Ordem de trava:** nada adicional; memória sob `flock` e atualização com `max` (`bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:588`).
- **Pouso antes do pré-requisito:** nada no fluxo examinado; prova do RELATO antecede a liberação nova (`bin/hook_stop_fila1.py:763`, `bin/hook_stop_fila1.py:774`).

**PAREI, teto e fila vazia:** nenhuma regressão adicional identificada na integração (`bin/hook_stop_fila1.py:769`, `bin/hook_stop_fila1.py:788`, `bin/hook_stop_fila1.py:827`).

Execução integral do selo: **[nao verificado]**; ele cria e remove arquivos, contrariando o modo solicitado (`bin/tests/test_hook_esperando_com_prova.sh:28`, `bin/tests/test_hook_esperando_com_prova.sh:30`).

## 29/10 19:00 · sessao c57a7aa93193 · - · - · origem=Ronald (tmux)

Encontrei três defeitos na prova PID, reproduzidos sem escrever arquivos:

1. **Opção aceita como produtor.** [bin/hook_stop_fila1.py:487](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:487) aceita `argv[1]` sem verificar se é opção. Entrada: `ESPERANDO: -c | pid=N`, sendo N um `python3 -c 'import time; time.sleep(30)'`. `_prova_pid('-c', N)` retornou `''`; o parser retornou `(True, '', linha)`. Assim, um interpretador genérico parado passa como produtor `-c` e libera pelo ramo de [bin/hook_stop_fila1.py:775](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:775).

2. **Caminho contorna rejeição de produtor genérico.** [bin/hook_stop_fila1.py:498](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:498) verifica o basename lexical; [bin/hook_stop_fila1.py:492](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:492) compara caminhos resolvidos. Para um `/usr/bin/sleep 30` vivo, `/usr/bin/sleep` bloqueou como genérico, mas `/usr/bin/sleep/`, `/usr/bin/sleep/.` e `/proc/N/exe` retornaram prova `''`. Entrada: `ESPERANDO: /usr/bin/sleep/ | pid=N`. Libera usando somente o temporizador que a própria regra pretende rejeitar.

3. **`cwd` atual atribui script ao caminho errado.** [bin/hook_stop_fila1.py:489](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:489) resolve script relativo contra o diretório atual do processo, que pode ter mudado após abrir o script. Entrada convencional: iniciar `python3 produtor.py` em `/a`, o script mudar para `/b`; declarar `/b/produtor.py` aceita o produtor errado, enquanto `/a/produtor.py` pode bloquear. Reproduzi com `/bin/bash dev/stdin`, iniciado em `/`, recebendo pelo pipe `cd /tmp; while :; do /usr/bin/sleep 30; done`: `/tmp/dev/stdin` não existia, mas `_prova_pid('/tmp/dev/stdin', N)` retornou `''`.

Ancestrais e rejeição de PID morto/reciclado: **nada adicional**, pelos ramos de [bin/hook_stop_fila1.py:502](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:502), [bin/hook_stop_fila1.py:517](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:517) e [bin/hook_stop_fila1.py:519](/home/ronald/codex_ws/512e6c938dbf0fbf0c39e7c7caff081f12f237f0/bin/hook_stop_fila1.py:519).

## 29/10 19:00 · sessao fe8a22260b54 · - · - · origem=Ronald (tmux)

Achei um contraexemplo novo na identidade do sinal: a memória usa apenas `st_dev:st_ino`, sem geração do arquivo nem descarte de entradas antigas (`bin/hook_stop_fila1.py:565`, `bin/hook_stop_fila1.py:585`, `bin/hook_stop_fila1.py:588`).

Entrada concreta: `ESPERANDO: agente-x | arquivo=/tmp/sinal`; a memória contém `{"1:123":10}` referente a um arquivo removido. Outro arquivo estático nasce com o inode reciclado `1:123` e `mtime_ns=20`, e seu criador termina antes da avaliação. A primeira observação desse novo arquivo retorna prova porque `20 > 10`; não há produtor vivo, mas o Stop libera (`bin/hook_stop_fila1.py:605`, `bin/hook_stop_fila1.py:607`, `bin/hook_stop_fila1.py:775`). Reproduzi o retorno `''` executando a função real com E/S simulada em memória; reciclagem efetiva neste host: [nao verificado]. É identidade entre arquivos diferentes, não o residual de avanço antigo do mesmo arquivo (`bin/hook_stop_fila1.py:565`).

Para ordem de trava/regressão da memória: nada novo; observação anterior ao lock que tenha sido ultrapassada acaba bloqueada pelo `max` e pela comparação final (`bin/hook_stop_fila1.py:551`, `bin/hook_stop_fila1.py:572`, `bin/hook_stop_fila1.py:588`, `bin/hook_stop_fila1.py:605`).

Para symlink/FIFO da memória e trava, temporário, permissões capturadas e falhas de gravação: nada adicional encontrado no fluxo analisado (`bin/hook_stop_fila1.py:526`, `bin/hook_stop_fila1.py:571`, `bin/hook_stop_fila1.py:578`, `bin/hook_stop_fila1.py:589`, `bin/hook_stop_fila1.py:600`).

## 29/10 19:00 · sessao e9b065734b1c · - · - · origem=Ronald (tmux)

Achado concreto no selo:

- **Falso vermelho quando PID 1 é interpretador/embrulho.** Em `bin/tests/test_hook_esperando_com_prova.sh:158`, `/proc/1/exe=/usr/bin/python3.12` produz a entrada `ESPERANDO: python3.12 | pid=1`, mas o teste exige `ancestral`. `_prova_pid` retorna antes `produtor generico (python3.12)` em `bin/hook_stop_fila1.py:498`; a verificação ancestral está em `bin/hook_stop_fila1.py:505`. O bloqueio correto reprova o selo, abortando a régua em `bin/regua.sh:140`. Confirmado chamando a função em memória, sem escrita; a mesma incompatibilidade ocorre com `bash` e `node` pelas listas em `bin/hook_stop_fila1.py:439`.

Demais verificações:

- **Contagem falsa:** nada; contei 33 chamadas de bloqueio e quatro de liberação, compatíveis com `bin/tests/test_hook_esperando_com_prova.sh:185`.
- **Selo sem mordida por liberação alternativa:** nada; exige saída vazia e trilha `ALLOW ESPERANDO com prova` em `bin/tests/test_hook_esperando_com_prova.sh:71` e `bin/tests/test_hook_esperando_com_prova.sh:74`; negativos exigem `siga: ITEM-VIVO` e motivo em `bin/tests/test_hook_esperando_com_prova.sh:84` e `bin/tests/test_hook_esperando_com_prova.sh:87`.
- **Prova do RELATO pulada:** nada no fluxo estático; executa antes das portas em `bin/hook_stop_fila1.py:763`, com caso adversarial em `bin/tests/test_hook_esperando_com_prova.sh:175`.
- **Regressão PAREI, teto, fila vazia:** nada identificado na integração: PAREI conserva precedência em `bin/hook_stop_fila1.py:769`, fila vazia em `bin/hook_stop_fila1.py:788`, teto em `bin/hook_stop_fila1.py:815`.
- **Segundo juiz, segundo escritor, relógio fora da borda, derivação paralela ou pouso antes do pré-requisito:** nada identificado neste recorte; topo compartilhado em `bin/hook_stop_fila1.py:400`, prova antes da liberação em `bin/hook_stop_fila1.py:763`.

Observação para consolidação: o caso `pid-ok-arq-parado` ocorre depois da remoção da memória em `bin/tests/test_hook_esperando_com_prova.sh:140`; portanto verifica primeira observação do arquivo em `bin/tests/test_hook_esperando_com_prova.sh:142`, não comparação contra baseline conhecido. Ainda exercita a conjunção, e não classifico isoladamente como defeito.

Não executei o selo, pois cria e remove arquivos em `bin/tests/test_hook_esperando_com_prova.sh:28` e `bin/tests/test_hook_esperando_com_prova.sh:30`.

_247 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
