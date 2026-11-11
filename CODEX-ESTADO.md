# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 07:35** · publicado em **11/11/2026 07:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 07:35 · sessao 9f734a1d238b · FIO-E0-V4-E3 · construtor · origem=Code

Discordo da solução proposta e **não apliquei alteração**, conforme sua instrução: `forcar=True` vence a guarda, mas o ramo NB retorna sem preencher `cods`; portanto, o primeiro acesso continuaria sujeito a `KeyError`. A escrita de `cods` fica no ramo normal (`ponto/services/cartorio.py:1228`, `:1282`, `:1441`; `ponto/tests/test_x1_folga_sem_adesao.py:132`).

O signal realmente chama o julgamento na criação da ausência. O receiver de `:275` é consumidor posterior; quem julga é o de `:143`, via `_julgar_dias` (`ponto/signals.py:93`, `:143`, `:275`).

**Diff vazio**; C2b e suas afirmações permanecem intactos (`ponto/tests/test_x1_folga_sem_adesao.py:131`).

Comandos executados:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest ponto.tests.test_e3_nb_impressao_memoria -v
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo green
```

Ambos: **rc=0, 19 testes** (`ponto/tests/test_e3_nb_impressao_memoria.py:88`; `ponto/tests/x1_mutantes.py:65`).

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante parcial_tira_marcador
```

Cada um: **rc=1, FAIL=1, ERROR=0**. Assertions atingidas, respectivamente: `ponto/tests/test_e3_nb_impressao_memoria.py:262`, `:264`, `:278`, `:318`. Isso comprova o mutante parcial somente em memória; resultado ORM: **[nao verificado]**.

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py ponto/tests/test_e3_nb_impressao_memoria.py ponto/tests/x1_mutantes.py
```

Ruff: rc=1, `No module named ruff`; Ruff limpo: **[nao verificado]**.

A cura exclusivamente no teste exige rever a primeira afirmação que depende de `out['cods']`, preservando a verificação do veredito por outra testemunha; isso ultrapassa a troca autorizada e a exigência de manter essa afirmação (`ponto/tests/test_x1_folga_sem_adesao.py:132`).

## 11/11 07:33 · sessao 010796d81078 · - · - · origem=Ronald (tmux)

Achados do backfill/foto, por severidade:

1. **Alta — relógio da leitura vira falsa escrita e bloqueia cura.** `fechamento_lido` usa `recalcular_fechamento_mes(... somente_leitura=True, retornar_objeto=True)`; a conta atribui `previsto_em = timezone.now()` antes do ramo de leitura devolver o objeto. A foto inclui esse campo transitório em `campos`, e `mudancas` compara todos os campos. Logo duas leituras sem alteração monetária produzem delta. Como a foto percorre competências de todo o histórico, um `dinheiro_online:10/2026` pode cancelar um ato da 11 por `fora_da_competencia_alvo`; se for exportada, também entra na cerca. Fontes: `app/ponto/services/fechamento.py:1086`, `:1128`, `:1135`, `:1759`; `app/chamados/services/foto_e4b.py:15`, `:108`, `:142`, `:160`, `:190`, `:194`, `:32`.
   
   Prova determinística sem Django/arquivos realizada com corpos reais extraídos por AST: antes/depois com horas_falta=8 e DSR=1 idênticos, só `previsto_em` t versus t+1μs, retornou `campos_alterados=['previsto_em']` e `cerca(...,'11/2026')=[('dinheiro_online:10/2026','fora_da_competencia_alvo')]`. Integração ORM com relógio solto: [nao verificado]. Os novos ensaios ORM congelam o relógio: `app/chamados/tests/test_e4b_c4_orm.py:24`, `:125`.

2. **Alta — o estado final de APPLY pode nascer depois da foto/cerca/DIFF.** O backfill materializa a disputa inteira; ao quitá-la fecha a disputa. O signal agenda `retomar_regularizacoes_confirmadas` em `on_commit`, que percorre todas as regularizações confirmadas do colaborador, sem filtro pela competência alvo, podendo criar/corrigir batidas nas datas dos itens e marcar regularizações materializadas. Isso roda depois de foto, cerca e DIFF final; DRY descarta os callbacks. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:145`, `:147`, `:297`, `:308`, `:314`, `:319`; `app/chamados/services/materializacao.py:1375`, `:1378`, `:1333`; `app/chamados/signals.py:129`, `:132`, `:137`, `:144`; `app/chamados/services/regularizacao_ext.py:218`, `:221`, `:224`, `:166`, `:188`, `:197`.
   
   Há ainda fechamento monetário agendado pela criação de batida, depois da foto do gravado: `app/ponto/registro_batida.py:157`, `:160`, `:175`; `app/ponto/services/fechamento.py:1807`, `:1810`, `:1824`. O dinheiro online tenta antecipar o cálculo, mas não observa essas futuras escritas reais nem seu resultado: `app/chamados/services/foto_e4b.py:188`, `:192`.

   A ordem `with transaction.atomic(), push_disputa_silenciado(...)` também restaura o silêncio antes de sair do atomic e disparar seus callbacks; previsão/silêncio desses efeitos posteriores não ficam cobertos pelo bloco. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:263`; `app/chamados/services/disputa_emissao.py:1749`, `:1753`. Push concreto nessa cascata: [nao verificado].

3. **Alta — “não entregue” substitui “aberta”, e comp10 não tem proteção própria.** Executáveis dependem de `competencia_do_dia_entregue(...) is False`; a cerca aceita a mesma competência alvo com `entregue=False`. Não achei guarda especial de 10/2026 nem exigência de `!`, nem pergunta a `periodo_trancado`/`competencia_fechada_para_celula` no comando/cerca. Assim `--competencia 10/2026 --aplicar` é aceito se K6 falso, exatamente a hipótese que o aval diz não bastar. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:210`, `:213`, `:240`, `:244`, `:246`, `:283`; `app/chamados/services/foto_e4b.py:30`, `:32`, `:34`; K6 só responde existência de TXT vigente: `app/folha/export.py:556`, `:580`, `:586`. O recibo admite que `!` ficou condição operacional externa: `app/docs/FIO-E0-V4-E4B-c4.txt:60`, `:65`, `:190`.

4. **Média/alta — foto/reversão “integral” omite escritas alcançadas.** Não captura `EventoCelula`, embora ressuscitar perguntas chame `acordar_celulas` e o save de pergunta também emita evento; portanto novas linhas da outbox não aparecem no delta/nascimentos/reversão. Fontes: enumeração da foto `app/chamados/services/foto_e4b.py:142` a `:187`; emissão `app/chamados/models.py:1566`, `:1568`, `:3413`; `app/chamados/signals.py:251`, `:257`; escritor `app/ponto/portas/celula.py:887`, `:896`; campos e consumidor `app/escala/models.py:1845`, `:1848`, `:1857`.
   
   Fábrica pode escrever pauta, cuja porta também cria/atualiza `ContadorPauta`; foto captura Pauta mas não contador. Auditoria da foto só inclui seis modelos, excluindo inclusive modelo Pauta. Fontes: `app/chamados/services/fabrica_da_celula.py:204`, `:214`, `:215`, `:354`, `:369`; `app/pautas/services.py:180`, `:186`, `:188`, `:351`, `:352`, `:354`; `app/chamados/services/foto_e4b.py:163`, `:173`, `:175`, `:186`. Regularizações e itens do achado 2 também não estão na enumeração da foto: `app/chamados/services/foto_e4b.py:84`, `:90`, `:142`.

5. **Média — rechecagem K6 não serializa a entrega.** Trava cobre C/D/Ch/P, mas o escritor `ExportacaoDominio.registrar` usa `exists/create` sem essa trava; `invalidar` também não trava C. A entrega pode ocorrer depois da última leitura K6 da foto posterior e antes do commit. Fontes: `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:283`, `:297`, `:308`; `app/folha/export.py:580`; `app/folha/models.py:151`, `:162`, `:174`, `:178`, `:182`. Corrida efetivamente reproduzida: [nao verificado]; recibo reconhece a pendência em `app/docs/FIO-E0-V4-E4B-c4.txt:190`.

Aspectos corretos demonstráveis por leitura:

- Ordem inicial C por PK de todo lote, depois universo aberto por colaborador; protocolo trava disputas abertas, pais delas e chamados vivos. Portas adicionais travam perguntas e disputas fechadas ao ressuscitar. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:183`, `:186`, `:187`, `:189`; `app/chamados/models.py:2640`, `:2675`, `:2688`, `:1562`. Universo inicial não é literalmente todas as linhas da foto nem todos os terminais/fechados que a porta pode escrever: mesmas fontes.
- Foto nasce sob atomic/C, após refresh de colaborador; reversão grava e relê JSON antes do primeiro ato. Reversão é fotografia para compensação, não achei executor de reversão neste comando. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:56`, `:59`, `:61`, `:63`, `:263`, `:264`, `:266`, `:267`, `:270`, `:275`, `:277`, `:295`.
- Reconfere entrega e plano sob trava; limbo/exportada abortam savepoint; cerca usa ambos lados do delta, recusando fora do alvo/indeterminado/exportado. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:283`, `:288`, `:290`, `:292`, `:302`, `:306`; `app/chamados/services/foto_e4b.py:22`, `:30`, `:32`, `:34`.
- DIFF por colaborador contém nascimentos por identidade, tentativas de push e campos numéricos de DiaPago, fechamento gravado e online; não refaz soma monetária. Fontes: `app/chamados/services/foto_e4b.py:39`, `:50`, `:60`, `:68`, `:71`, `:72`, `:192`; `app/chamados/management/commands/backfill_fio_morto_e4b.py:317`, `:351`, `:355`.
- Não achei mecanismo de comparar esse DIFF com expectativa de dinheiro, publicar previamente no RELATO ou exigir essa publicação antes de APPLY; execução operacional dessas condições: [nao verificado]. O DIFF impresso no mesmo APPLY vem depois da saída do atomic: `app/chamados/management/commands/backfill_fio_morto_e4b.py:263`, `:320`, `:351`, `:355`.
- Não introduziu exclusão por col950/col969/CADASTRO no comando; universo vem do vigia e lista complementar, e antes/depois continuam medidos pelo mesmo vigia. Fontes: `app/chamados/management/commands/backfill_fio_morto_e4b.py:76`, `:78`, `:153`, `:222`, `:223`, `:318`, `:365`.
- Provas de idempotência cobrem fixture específica de fábrica e classe e com mesma nota/âncora; não certificam todos os efeitos posteriores ou tabelas omitidas. Fontes: `app/chamados/tests/test_e4_backfill.py:141`, `:145`, `:154`, `:157`; `app/chamados/tests/test_e4_passivo.py:61`, `:75`, `:78`, `:80`.

Mapa do formato novo:

- `foto[chave].campos`: escritor é leitura dos campos concretos do ORM (`foto_e4b.py:124`), leitor `mudancas` (`:15`), `nascimentos` (`:44`), `relato` (`:69`), JSON reversão (`backfill:273`).
- `foto[chave].alcance[{dia,comp,entregue}]`: escritor `alcance/mensal` (`foto_e4b.py:102`, `:108`, `:111`, `:115`, `:118`); leitores cerca (`:26`, `:29`), relato (`:78`), BANG-LINHA (`backfill:345`, `:346`).
- `delta`: escritor `mudancas` (`foto_e4b.py:12`), leitores cerca (`:21`), nascimentos (`:43`), relato (`:73`) e backfill (`:300`, `:345`).
- `previstos`: alimentado por `_enviar_push_disputa_colab` sob contexto (`disputa_emissao.py:1760`, `:1763`); backfill corta tentativa desfeita (`backfill:305`, `:311`), filtra por colaborador (`:317`) e publica via relato (`foto_e4b.py:72`).
- Datas mantêm autoridades: batida pelo dia do turno (`foto_e4b.py:154`), ausência por `dias_da_ausencia` (`:158`), competência pelo fim da janela (`:108`, `:109`), instante de reversão pelo `agora` da borda (`backfill:215`, `:271`). O campo com dois sentidos encontrado é `previsto_em` do objeto online tratado como mudança persistida, achado 1.

## 11/11 07:33 · sessao 64dd4b872ec3 · - · - · origem=Ronald (tmux)

Mapa do memo/thread-local e conclusões:

- `_tl.colaborador` tem escritor em `app/ponto/services/cartorio.py:949`, restauração em `:1097` e leitor único produtivo `colaborador_deste_julgamento` em `:96-97`; S143 consome esse leitor em `app/chamados/services/disputa_emissao.py:1917`. O significado é instância do colaborador do ato; a igualdade de PK impede usar outro colaborador (`cartorio.py:97`).
- `_tl.ativo` também é escrito por `calado` (`cartorio.py:105-110`). Portanto “em julgamento” abrange supressão de signals; não significa necessariamente execução de `julgar_colab` (`cartorio.py:101-104`). Em execução normal, `julgar_colab` restaura o colaborador anterior, geralmente `None` (`cartorio.py:946-949`, `:1097`).
- `_memo_da_tranca` escolhe, nesta ordem: consulta ativa, lote quando permitido, request ou instância (`app/chamados/juizes.py:1060-1070`). Assim `lote=False` não ignora consulta ativa, e “memo do colaborador” pode efetivamente ser memo do request/consulta (`:1060-1069`).
- Empresa é escrita no memo por `dias_trancados` (`juizes.py:1015-1023`); tranca por empresa/competência é escrita e lida em `:1025-1032`; usuário sistema e toque positivo são escritos em `:1181-1190`, lidos em `:1173-1187`. Toque usa PK do colaborador/data/instante na chave; não achei colisão entre colaboradores (`:1174`).
- O `partial` recebe referência ao memo (`disputa_emissao.py:1919-1921`); a cópia efetiva acontece ao entrar no contexto (`juizes.py:1101-1104`). É cópia rasa: valores Empresa/PeriodoFechado continuam sendo referências aos objetos (`juizes.py:1018-1029`, `:1102`).
- `tranca_da_consulta` calcula pelo juiz existente `dias_trancados`, guarda resultado por identidade da pergunta e restaura o contexto com token no `finally` (`juizes.py:1106-1110`, `:948-954`). Não achei segundo juiz: C6/C20 estão registrados em `app/core/juizes.py:278`, `:292`; a função apenas alimenta C20 (`juizes.py:1107`). `colaborador_deste_julgamento` fornece metadado, não julgamento de domínio (`cartorio.py:90-97`).
- Fora de julgamento ou para outra PK, S143 recebe `None` e abre consulta fria; não lê colaborador errado por esse caminho (`cartorio.py:97`, `disputa_emissao.py:1917-1921`, `juizes.py:1097`). S143 também roda por receiver e por chamadores avulsos (`app/chamados/signals.py:111-115`, `disputa_emissao.py:1863-1879`).
- Não achei escritor de tranca/toque dentro da nova cura S143: ela chama `Chamado.reabrir`, que altera estado e ressuscita perguntas (`disputa_emissao.py:1849`, `app/chamados/models.py:1442`, `:1451`). Mudança concorrente da tranca continua possível pelo código: os escritores salvam `PeriodoFechado` antes de eventual trabalho de fila, sem trava C nessa escrita (`app/ponto/services/fechamento.py:1408-1428`, `:1437-1448`).
- Risco verificável de foto velha: uma entrada `('tranca', empresa, mês, ano): None` copiada evita consultar novamente `periodo_trancado`, mesmo se este já mudou (`juizes.py:1025-1030`, `:1102`). Probe somente em memória confirmou snapshot `False/0 consultas` contra consulta fresca `True/1 consulta`; ocorrência em produção [nao verificado]. Não achei invalidação produtiva de `_tranca_memo`; seu armazenamento é `juizes.py:1070`.
- Aninhamento de `julgar_colab`: restaura colaborador, mas força `_tl.ativo=False`; o julgamento exterior deixa de fornecer colaborador após o interior (`cartorio.py:946-949`, `:1097-1098`, `:97`). Esse defeito do ativo já aparece no trecho anterior do diff; caminho produtivo aninhado não achei. O teste novo exercita exceção, não julgamento aninhado (`app/chamados/tests/test_e4b_c4_tranca_memoria.py:119-144`).
- Semântica suspeita fora do memo: conversa escreve dia civil (`models.py:2794-2802`), e o juiz de dia do turno retorna `None` para ela (`juizes.py:332-336`); a nova foto usa `p.data_evento` indistintamente (`app/chamados/services/foto_e4b.py:128`). `_ancoradas` não exclui conversa (`models.py:344-346`), e a cerca da cura também usa data crua (`disputa_emissao.py:1835-1836`). Há atribuição de competência de fato a campo que também representa dia civil; impacto produtivo específico [nao verificado].
- Executei os cinco testes de memória: OK; os seis mutantes catalogados morreram por AssertionError, sem errors. Fontes dos alvos e enxertos: `test_e4b_c4_tranca_memoria.py:59-144`, `:147-166`. Mutação adicional `memo.update(memo_inicial) → pass` também morreu no assert `:81`; portanto “memo_inicial ignorado”, “memo não copiado” e “colaborador não restaurado” têm mordida de memória (`:81`, `:91-101`, `:143`, `:154-159`). Integração ORM dessas propriedades [nao verificado]. [nao verificado]

## 11/11 07:32 · sessao a5f07e75a644 · - · - · origem=Ronald (tmux)

- **Não dá para afirmar “cada RED morde a base”.** `--red-base` carrega somente `disputa_emissao.py` de b20 e executa somente D3/evento; RED da tranca usa outra origem (`ca42...`); a sonda de queries usa MAIN `fb926...`, conservando os demais arquivos HEAD. (`app/chamados/tests/e4b_c4_verificar.py:141`, `app/chamados/tests/test_e4b_c4_tranca_memoria.py:22`, `app/chamados/tests/test_e4b_c4_tranca_memoria.py:175`, `app/chamados/tests/e4b_c4_base.py:9`, `app/chamados/tests/e4b_c4_medir_queries.py:3`).

- **Os três mutantes óbvios citados são pegos.** Executei os seis mutantes de tranca e um extra `memo.update(memo_inicial) -> pass`: todos morreram com uma assertion, zero errors. `sem_copia` acusa aquecimento da fonte; `sem_restaurar_colab` acusa falta de restauração; ignorar `memo_inicial` acusa leitura avulsa extra. As assertions estão em `app/chamados/tests/test_e4b_c4_tranca_memoria.py:59`, `:83`, `:120`; catálogo em `:148`. Resultado da execução desta sessão, sem artefato persistido: [nao verificado].

- **Lacuna de aninhamento:** o teste de `julgar_colab` começa com `_tl.ativo=False`; verifica colaborador externo restaurado e leitura fora do ato, mas não entra em julgamento externo ativo nem prova que o retorno de julgamento interno preserva esse estado ativo. (`app/chamados/tests/test_e4b_c4_tranca_memoria.py:32`, `:120`, `:145`). Também não achei teste de alteração da fonte do memo no meio do ato; [nao verificado].

- **Teto global mede caminho real:** registra batida, exige 1/3 chamados fechados e captura SQL do registro real; não é proxy. O teste da tranca compara o mesmo caminho real HEAD × avulso, com rollback, métrica de `periodo_trancado`, SQL total e `on_commit`. (`app/ponto/tests/test_selo_chokepoint_escrita.py:406`, `:428`, `:435`, `app/chamados/tests/test_e4b_c4_orm.py:61`, `:105`).

- **RED do mapa é isolamento de componente:** explicitamente substitui `tranca_da_consulta` por `nullcontext`; prova economia do mapa nesse cenário modificado, não economia adicional do mapa com a tranca HEAD ativa. (`app/chamados/tests/test_e4b_c4_orm.py:259`, `:265`, `:297`, `:307`).

- **O anexo informado não prova morte de `tranca_avulsa`.** O executor atual a exige depois dos cinco mutantes ORM e do mapa; a ausência desse resultado não pode ser preenchida por `rc=0` de protocolo simulado. (`app/chamados/tests/e4b_c4_verificar.py:106`, `app/chamados/tests/test_e4b_c4_orm_protocolo.py:73`; anexo `c4v6_orm_resumo.txt`: [nao verificado]).

- **Foto/cerca ORM é adversarial declarado:** o hook chama a porta real de ressuscitação depois da fábrica; não certifica cascata natural completa. O teste exige que o hook realmente executou e que P90/D90 ressuscitaram antes do rollback. (`app/chamados/tests/test_e4b_c4_orm.py:3`, `:226`, `:237`).

- **Relógio:** não achei teste novo solto no escopo verificado. Classes ORM congeladas e B2d agora congelado inteiro; selo estático distingue relógio sem argumento e conversão `localtime(x)`. (`app/chamados/tests/test_e4b_c4_orm.py:24`, `:125`, `app/chamados/tests/test_b2d_resposta_visivel.py:25`, `app/core/tests/test_selo_teste_sem_relogio.py:32`, `:76`).

- Executei **71 testes de memória OK** e **40 selos OK**, com `PYTHONDONTWRITEBYTECODE=1`; nenhum ORM/banco foi executado. Resultado sem arquivo de saída persistido: [nao verificado]. O executor ORM é subconjunto explícito, portanto nem ele nem os 130 informados equivalem à suíte inteira. (`app/chamados/tests/e4b_c4_verificar.py:74`).

- Recibos não são prova atual: o primeiro declara ausência de certificação ORM; o v2 ainda fala de números/tetos e fixtures de voltas anteriores. (`app/docs/FIO-E0-V4-E4B-c4.txt:3`, `app/docs/FIO-E0-V4-E4B-c4-v2.txt:90`, `:183`).

## 11/11 07:27 · sessao 3b06430316b1 · FIO-E0-V4-E3 · construtor · origem=Code

Diff entregue:

- [Teste ORM C2b](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_x1_folga_sem_adesao.py:115): exige parcial real na grade, precedência, marcador e rejulgamento sem forçar após batida distante. [nao verificado]
- [Caso sem banco](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/test_e3_nb_impressao_memoria.py:309): assertion da impressão mata o novo mutante.
- [Executor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:36): mutante registrado; contagens green ajustadas para 19 em memória e 12 ORM.

Comandos executados e RC ([critério do executor](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/ponto/tests/x1_mutantes.py:62)):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo green
# rc=0; 19 testes, FAIL=0, ERROR=0 [nao verificado]

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante parcial_tira_marcador
# rc=1; FAIL=1, ERROR=0; assertion da impressão

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante literal_incondicional
# rc=1; FAIL=1, ERROR=0

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante sem_fonte_no_gancho
# rc=1; FAIL=1, ERROR=0

PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --sem-banco --modo mutante --mutante so_acusador
# rc=1; FAIL=1, ERROR=0
```

Ruff nos três arquivos: rc=0, `All checks passed!`:

```bash
/tmp/claude-1001/-home-ronald-saas-hasner-app/707e509b-29e4-4438-9018-16733980c39a/scratchpad/ruffpkg/bin/ruff check --no-cache ponto/tests/test_x1_folga_sem_adesao.py ponto/tests/test_e3_nb_impressao_memoria.py ponto/tests/x1_mutantes.py
```

[Catraca das linhas registradas](/home/ronald/wt-codex-fio-e0-v4-e3-032042/app/core/tests/test_medida_turno_do_par.py:539): rc=0 pelo executor em memória documentado no arquivo; registro mantido.

ORM green e morte do mutante pelo teste ORM: **[nao verificado]**, reservados à sessão com banco isolado: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --modo green
PYTHONDONTWRITEBYTECODE=1 python3 -B -m ponto.tests.x1_mutantes --modo mutante --mutante parcial_tira_marcador
```

## 11/11 07:25 · sessao f637d95c917c · TRAVA-SELO · verificador · origem=Code

**O fechamento integral NÃO PROCEDE.** Os escapes anteriores por caminhos/globs e por `--parallel '1 .'` foram tratados, mas permanecem lacunas nos selos, na documentação e na cobertura dos chamadores. A própria guarda declara não reconhecer uma enumeração de módulos equivalente à suíte inteira. (`bin/suite.sh:52`, `bin/suite.sh:67`, `bin/suite.sh:95`, `bin/suite.sh:131`; `bin/tests/test_suite_motivo.sh:66`; `CLAUDE.md:100`; `bin/regua.sh:165`)

1. **NÃO PROCEDE como fechamento de todas as classes; PROCEDE para os escapes sintáticos anteriores.**

   | Classe/entrada | Conferência na cópia |
   |---|---|
   | `--only '.'`, `/app`, pastas, glob, segmento vazio, app fora de LABELS | A forma ancorada e a pertinência a LABELS recusam essas palavras antes do custo. (`bin/suite.sh:95`, `bin/suite.sh:96`, `bin/suite.sh:98`) |
   | Inválido após label válida: `--only 'api .'` | Todas as palavras passam pela guarda; não valida apenas a primeira. (`bin/suite.sh:97`, `bin/suite.sh:98`) |
   | Glob produzido por `--volta` | Não expande no cwd: `set -f` antecede o filtro. Uma saída sem nenhuma label aproveitável termina com rc 1. (`bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:88`) |
   | `--only api -- .`, `/app`, outro app | A lista fechada recusa posicionais depois de `--`. (`bin/suite.sh:105`, `bin/suite.sh:111`, `bin/suite.sh:114`, `bin/suite.sh:118`) |
   | `--parallel '1 .'`; `--espera x` | Recusa rc 2 no parser. Paralelismo aceita inteiro positivo ou `auto`; espera aceita dígitos, incluindo zero. (`bin/suite.sh:52`, `bin/suite.sh:54`) |
   | `-- -v 2`, `--tag x`, `--tag core`, `--exclude-tag=x`, `-k padrao`; `--parallel auto` | Passam pelas guardas com recorte parcial; a coincidência do valor com um app deixou de causar recusa. (`bin/suite.sh:52`, `bin/suite.sh:112`, `bin/suite.sh:113`) |
   | `-- --failfast=x` | Recusado corretamente: a forma `--x=v` consulta apenas `_FLAG_VALOR`. (`bin/suite.sh:113`, `bin/suite.sh:114`) |
   | Todas as LABELS, inclusive repetidas ou como `<app>.tests`, em `--only` ou saída de `--volta` | A guarda exige motivo pela seleção efetiva. (`bin/suite.sh:80`, `bin/suite.sh:136`, `bin/suite.sh:139`, `bin/suite.sh:143`) |

   **Limites remanescentes:**

   - Uma lista de módulos que esgote a inteira continua fora da classificação. Isso também vale para módulos provenientes de `--volta`, pois ambos alimentam `ONLY`. Qual lista concreta esgota os testes desta cópia: **[não verificado]**. (`bin/suite.sh:80`, `bin/suite.sh:131`, `bin/suite.sh:139`)
   - `--only ''` ainda termina pelo `${2:?}` — observei rc 1 na chamada seca — sem a recusa falante rc 2. (`bin/suite.sh:51`)
   - A lista de flags usa busca por substring: um único argumento `'--failfast --reverse'` pode casar com `_FLAG_SO`. Ele permanece **um argumento** no executor; ampliação efetiva por essa forma: **[não verificado]**. (`bin/suite.sh:105`, `bin/suite.sh:111`, `bin/suite.sh:200`)
   - `-v2` e `-kpadrao` são recusados pela lista. Que sejam formas exigíveis no runner instalado, para classificá-las como recusas indevidas: **[não verificado]**. (`bin/suite.sh:111`, `bin/suite.sh:113`, `bin/suite.sh:114`)

   Quanto aos demais achados anteriores: `-z --no-renames`, import entre parênteses e relativo em subpacote foram implementados; URL, signal, transitividade e reexportação foram **declarados como limites**, sem implementação de seguimento. (`bin/vizinhos_da_volta.py:13`, `bin/vizinhos_da_volta.py:14`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`, `bin/vizinhos_da_volta.py:27`, `bin/vizinhos_da_volta.py:55`, `bin/vizinhos_da_volta.py:70`)

   **Ainda há dependência direta omitida fora do limite declarado:** para `core/sub/foo.py`, as formas Python válidas `import core . sub . foo` e `from core . sub import foo` não casam com os padrões atuais. Conferi a sintaxe e o não casamento em memória; ocorrência desses imports nesta obra: **[não verificado]**. (`bin/vizinhos_da_volta.py:64`, `bin/vizinhos_da_volta.py:66`, `bin/vizinhos_da_volta.py:70`)

2. **PROCEDE para anteceder splitting/glob de seleção; NÃO PROCEDE literalmente para “toda expansão”.**

   Mapa dos campos de `suite.sh`, incluindo os demais controles tocados pelo delta: [nao verificado]

   | Campo | QUEM ESCREVE | QUEM LÊ |
   |---|---|---|
   | `ONLY` | Inicialização, parser, helper e seleção filtrada: `bin/suite.sh:47`, `:51`, `:80`, `:87` | Conflito, filtro, vazio, validação, classificação, seco e executor: `bin/suite.sh:78`, `:84`, `:88`, `:97`, `:136`, `:155`, `:199` |
   | `EXTRA` | Inicialização e captura citada após `--`: `bin/suite.sh:47`, `:59` | `_extra_ok` e executor: `bin/suite.sh:109`, `:110`, `:112`, `:113`, `:200` |
   | `MOTIVO` | `bin/suite.sh:47`, `:56` | Guarda, diagnóstico e seco: `bin/suite.sh:143`, `:145`, `:154` |
   | `VOLTA` | `bin/suite.sh:47`, `:57` | Ativação, helper, diagnóstico e seco: `bin/suite.sh:77`, `:80`, `:81`, `:154` |
   | `LABELS` | Fonte literal: `bin/regua.sh:23`; derivação: `bin/suite.sh:72` | Filtro, pertinência, classificação e fallback: `bin/suite.sh:85`, `:96`, `:138`, `:139`, `:199` |
   | `_FORMA_LABEL` | `bin/suite.sh:95` | `_eh_label`, chamado na guarda: `bin/suite.sh:96`, `:98` |
   | `_FLAG_SO` | `bin/suite.sh:105` | Pertinência e diagnóstico: `bin/suite.sh:111`, `:114` |
   | `_FLAG_VALOR` | `bin/suite.sh:106` | Consumo, forma com `=` e diagnóstico: `bin/suite.sh:112`, `:113`, `:114` |
   | `PARALELO` | Inicialização e parser: `bin/suite.sh:47`, `:53`; validação em `:52` | Executor: `bin/suite.sh:200` |
   | `ESPERA` | Inicialização e parser: `bin/suite.sh:47`, `:55`; validação em `:54` | Chamada da trava: `bin/suite.sh:193`; parser/flock: `bin/trava_teste.sh:27`, `:36` |
   | `DIR` | `bin/suite.sh:47`, `:50` | Helper, seco e montagem: `bin/suite.sh:79`, `:154`, `:169`, `:170`, `:171`, `:174` |
   | `SECO` | `bin/suite.sh:47`, `:58` | Desvio seco: `bin/suite.sh:153` |

   Antes de `set -f; LC_ALL=C`, há expansões no parser e no subshell que calcula `RAIZ`; elas usam aspas, atribuições ou `[[ ]]`, sem splitting/glob das palavras de seleção. (`bin/suite.sh:35`, `bin/suite.sh:49`, `bin/suite.sh:51`, `bin/suite.sh:52`, `bin/suite.sh:59`, `bin/suite.sh:67`)

   O `source recursos.sh` também antecede a proteção, mas nesta cópia só faz atribuições/exports e define funções; o guard de arquivo executado impede chamar as funções operacionais durante o source. (`bin/suite.sh:39`; `bin/recursos.sh:40`, `bin/recursos.sh:81`, `bin/recursos.sh:105`, `bin/recursos.sh:140`, `bin/recursos.sh:149`, `bin/recursos.sh:178`, `bin/recursos.sh:191`)

   As expansões não citadas de `ONLY` começam depois da proteção. No executor, `${ONLY:-$LABELS}` e `$PARALELO` continuam sem aspas; `EXTRA` permanece array citado. A trava recebe o argv já expandido e executa `"$@"`. (`bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:97`, `bin/suite.sh:199`, `bin/suite.sh:200`; `bin/trava_teste.sh:42`)

   **O seco não mostra o comando completo:** passa pelas guardas e imprime motivo, volta, diretório e labels; omite EXTRA, paralelismo e espera. (`bin/suite.sh:118`, `bin/suite.sh:143`, `bin/suite.sh:153`, `bin/suite.sh:154`, `bin/suite.sh:155`, `bin/suite.sh:156`)

3. **PROCEDE que a flag consome label/caminho; NÃO SEI sobre ampliação efetiva no runner instalado.**

   `-- -k .` e `-- --tag /app` são aceitos porque o ramo exige apenas existir a próxima palavra. Esses valores não passam pela guarda de posicionais. (`bin/suite.sh:112`)

   Isso, sozinho, **não demonstra fuga**: o shell entrega `-k` e `.` como elementos distintos, preservados, e não reapresenta `.` como label por expansão. A descoberta efetiva é delegada ao `DiscoverRunner` externo; quantidade executada nesses exemplos: **[não verificado]**. (`bin/suite.sh:200`; `app/core/runner_quarentena.py:15`, `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`)

4. **NÃO PROCEDE que os selos mordam cada classe pela razão certa.**

   Há melhoria: caminhos/globs de `ONLY` exigem o diagnóstico específico; existem casos mistos, cwd preparado, âncora de motivo e rc 75. (`bin/tests/test_suite_motivo.sh:57`, `bin/tests/test_suite_motivo.sh:58`, `bin/tests/test_suite_motivo.sh:62`, `bin/tests/test_suite_motivo.sh:64`, `bin/tests/test_suite_motivo.sh:75`, `bin/tests/test_suite_motivo.sh:76`)

   Persistem estes sobreviventes plausíveis:

   | Mutante/lacuna | Por que o selo pode continuar verde | [nao verificado]
   |---|---|
   | Acrescentar `--pattern` a `_FLAG_SO` | O negativo testa `--pattern y`: passa a primeira palavra e recusa `y`, mantendo rc 2 e o diagnóstico genérico procurado. (`bin/tests/test_suite_motivo.sh:66`, `:67`; `bin/suite.sh:111`, `:114`) |
   | Tratar `-k` como flag simples, acrescentando-o a `_FLAG_SO` | O positivo não usa `-k`; cobre `-v`, `--tag`, `--exclude-tag=x` e `--failfast`. (`bin/suite.sh:111`, `:112`; `bin/tests/test_suite_motivo.sh:70`) |
   | Aceitar `--x=v` também para `_FLAG_SO` | Falta negativo como `--failfast=x`; `--exclude-tag=x` continua passando. (`bin/suite.sh:113`; `bin/tests/test_suite_motivo.sh:66`, `:70`, `:74`) |
   | Executor descartar EXTRA ou ignorar `--parallel auto` | Os positivos cobram chegada à trava, sem conferir esses argumentos; `_args` extrai apenas labels, e a trava falsa registra `$*`, perdendo fronteiras. (`bin/tests/test_suite_motivo.sh:19`, `:24`, `:70`, `:73`) |

   Os ramos dos três primeiros mutantes foram conferidos em memória; sobrevivência à execução **integral** do selo: **[não verificado]**. Os selos completos escrevem e removem arquivos temporários. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:27`)

   O caso “todas as LABELS depois de `--`” também recusa primeiro em EXTRA; não prova a classificação de inteira pela razão anunciada. (`bin/tests/test_suite_motivo.sh:43`; `bin/suite.sh:118`, `bin/suite.sh:143`)

   Em `test_suite_sh.sh`, a fixture nua agora é negativa, mas o juiz continua sendo dois greps. Ele aceita, por exemplo, `bash …/bin/suite.sh # --motivo "pre-opus X"` ou motivo sem obra; a guarda real recusaria ambos. (`bin/tests/test_suite_sh.sh:34`, `bin/tests/test_suite_sh.sh:35`, `bin/tests/test_suite_sh.sh:80`; `bin/suite.sh:133`, `bin/suite.sh:143`)

   Os selos próprios do wrapper e da descoberta real de vizinhos continuam ausentes deste teste: ele executa a suite copiada e substitui o helper. (`bin/tests/test_suite_motivo.sh:20`, `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:28`)

5. **NÃO PROCEDE como fidelidade integral da documentação.**

   O comando canônico ganhou motivo e a descrição de EXTRA deixou de ensinar repasse cru. Porém:

   - “Só número” contradiz `--parallel auto`. (`CLAUDE.md:96`, `CLAUDE.md:100`; `bin/suite.sh:52`)
   - A redação deve explicitar que `--only`/`--volta` cobrindo todas as LABELS também exigem motivo. (`CLAUDE.md:101`, `CLAUDE.md:102`; `bin/suite.sh:139`, `bin/suite.sh:143`)
   - “Volta do meio = `--volta`” omite ruff e disjuntor: quem os executa é `volta_intermediaria.sh`. (`CLAUDE.md:102`; `bin/volta_intermediaria.sh:23`, `bin/volta_intermediaria.sh:29`, `bin/volta_intermediaria.sh:32`)
   - A descrição de rc 75 corresponde à trava; a recusa de inteira é rc 2, mas não se pode generalizar esse rc a todo erro de argumento, como `--only ''`. (`CLAUDE.md:102`, `CLAUDE.md:103`; `bin/trava_teste.sh:36`, `bin/trava_teste.sh:38`; `bin/suite.sh:51`, `bin/suite.sh:150`)

   A busca em `app/docs`, `bin/` e `.claude/` encontrou **orientações vivas incompletas na própria ajuda**: `suite.sh --dir …`, `--parallel 2` e `--espera 1`, sem recorte/motivo, são recusadas se copiadas literalmente. A chamada nua do selo é fixture negativa; há também comandos antigos em relatos, cujo uso atual é **[não verificado]**. (`bin/suite.sh:29`, `bin/suite.sh:30`, `bin/suite.sh:31`, `bin/suite.sh:143`; `bin/tests/test_suite_sh.sh:80`, `bin/tests/test_suite_sh.sh:91`; `app/docs/RELATO.md:681`)

   Não encontrei outra orientação viva de EXTRA cru nessa busca; ausência fora dos arquivos pesquisados: **[não verificado]**.

6. **PROCEDE o erro de descarte de régua/pre-push; NÃO SEI quanto à obrigação de um selo próprio específico.** [nao verificado]

   A triagem restringe o aval a `bin/suite.sh`, mas o aval define a unidade como **“chamada de suite inteira pela trava”**. Régua e pre-push fazem precisamente essa chamada sem guarda de motivo; a trava apenas executa o comando recebido. Ser preexistente não constitui exceção expressa no aval fornecido. (`triagem_v5.txt:5`; `aval_trava_menos_fila_com_selo_1111.txt:1`; `bin/regua.sh:165`, `bin/regua.sh:168`; `bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/pre-push.sh:187`; `bin/trava_teste.sh:42`)

   Para o **selo próprio de `volta_intermediaria.sh`**, o aval não impõe literalmente esse formato de teste. O descarte para fila não está demonstrado errado apenas por essa exigência específica; a cobertura do fluxo continua pendente. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `triagem_v5.txt:5`; `bin/tests/test_suite_motivo.sh:28`; `bin/volta_intermediaria.sh:37`, `bin/volta_intermediaria.sh:43`, `bin/volta_intermediaria.sh:44`)

   Para **vizinhos**, declarar limites e invocar a inteira pre-opus não demonstra fechamento das classes já conhecidas: o aval exige vizinhos e transformação de escapes em regras de classe. A defesa pre-opus está escrita no helper/triagem, sem exceção correspondente no aval dado. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `triagem_v5.txt:5`; `bin/vizinhos_da_volta.py:12`, `bin/vizinhos_da_volta.py:15`, `bin/vizinhos_da_volta.py:16`, `bin/vizinhos_da_volta.py:18`)

## 11/11 07:22 · sessao 559aedadd68b · - · - · origem=Ronald (tmux)

**Q1 — NÃO PROCEDE como fechamento completo.** As formas caminho/glob e a injeção por `--parallel '1 .'` estão barradas; continua declarado que a enumeração de módulos cobrindo a inteira não é reconhecida, inclusive quando vem de `--volta`. (`bin/suite.sh:52`, `bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:95`, `bin/suite.sh:98`, `bin/suite.sh:130`, `bin/suite.sh:131`)

A whitelist usa pertinência por substring: um único argumento `'--failfast --reverse'` é aceito por conter trecho contíguo da lista. Isso não demonstra ampliação: o executor preserva esse elemento inteiro. (`bin/suite.sh:105`, `bin/suite.sh:111`, `bin/suite.sh:200`) O comportamento efetivo desse argumento no Django é **[não verificado]**.

**Q2 — PROCEDE para antecedência às expansões sujeitas a splitting/glob; não literalmente a toda expansão.** O parser já expande argumentos antes de `set -f`, mas usa aspas, atribuições e `[[ ]]`. A primeira expansão não citada da seleção acontece depois da proteção. (`bin/suite.sh:49`, `bin/suite.sh:67`, `bin/suite.sh:84`, `bin/suite.sh:97`)

O `source` anterior não chama as funções que contêm expansões não citadas: o guard de execução distingue arquivo executado de arquivo sourceado. O subshell anterior calcula a raiz usando caminho citado. (`bin/suite.sh:35`, `bin/suite.sh:39`; `bin/recursos.sh:140`, `bin/recursos.sh:149`, `bin/recursos.sh:178`, `bin/recursos.sh:191`)

O comentário “LC_ALL sem export […] não para os filhos” não é universal: se `LC_ALL` já foi importada com atributo export, a atribuição preserva esse atributo; efeito sobre esta seleção: **[não verificado]**. (`bin/suite.sh:66`, `bin/suite.sh:67`)

| Campo | Quem escreve | Quem lê |
|---|---|---|
| ONLY | `suite.sh:47,51,80,87` | `suite.sh:78,84,88,97,136,155,199` | [nao verificado]
| EXTRA | `suite.sh:47,59` | `suite.sh:109–113,200` | [nao verificado]
| MOTIVO | `suite.sh:47,56` | `suite.sh:143,145,154` | [nao verificado]
| VOLTA | `suite.sh:47,57` | `suite.sh:77,80–81,154` | [nao verificado]
| LABELS | Fonte `regua.sh:23`; derivação `suite.sh:72` | `suite.sh:85,96,138–139,199` | [nao verificado]
| _FORMA_LABEL | `suite.sh:95` | `_eh_label`, `suite.sh:96`, chamado em `suite.sh:98` | [nao verificado]
| _FLAG_SO | `suite.sh:105` | `suite.sh:111,114` | [nao verificado]
| _FLAG_VALOR | `suite.sh:106` | `suite.sh:112–114` | [nao verificado]
| PARALELO | `suite.sh:47,53`, validado em `suite.sh:52` | `suite.sh:200` | [nao verificado]
| ESPERA | `suite.sh:47,55`, validado em `suite.sh:54` | `suite.sh:193`; `trava_teste.sh:27,36` | [nao verificado]

O executor expande `${ONLY:-$LABELS}` e `$PARALELO` sem aspas; preserva `EXTRA` citado. A trava recebe esse argv já formado e executa `"$@"`. (`bin/suite.sh:199`, `bin/suite.sh:200`; `bin/trava_teste.sh:42`)

O modo seco passa pelas guardas, imprime motivo/volta/diretório e labels, mas não imprime `EXTRA`, `PARALELO`, `ESPERA` ou o argv completo. (`bin/suite.sh:143`, `bin/suite.sh:153`, `bin/suite.sh:154`, `bin/suite.sh:155`, `bin/suite.sh:156`)

**Q3 — PROCEDE que consome; NÃO SEI sobre ampliação efetiva no runner instalado.** `-- -k .` e `-- --tag /app` passam porque a guarda de flag de valor exige somente a existência da próxima palavra. Esses valores continuam elementos do array citado; não ganham condição de posicional por expansão do shell. (`bin/suite.sh:112`, `bin/suite.sh:200`)

A cópia delega a descoberta ao `DiscoverRunner` externo; não contém nesse trecho a implementação que permitiria medir a seleção efetiva. Portanto, os exemplos não sustentam afirmar fuga para a inteira. (`app/core/runner_quarentena.py:15`, `app/core/runner_quarentena.py:72`, `app/core/runner_quarentena.py:76`) Quantidade de testes executada: **[não verificado]**.

## 11/11 07:22 · sessao bec9acda611b · - · - · origem=Ronald (tmux)

**Q4 — NÃO PROCEDE como cobertura completa.** Há melhoria real, mas permanecem recusas pela razão errada e mutantes plausíveis sobreviventes. (`bin/tests/test_suite_motivo.sh:57–79`; `bin/tests/test_suite_sh.sh:31–35`)

- Caminhos/globs em `ONLY` agora exigem diagnóstico `nao e label`; há seleção mista inválida e cwd preparado para detectar ausência de `set -f`. (`bin/tests/test_suite_motivo.sh:57–64`)
- O caso `--only ' ' -- --failfast appA appB` recusa na guarda de EXTRA antes de alcançar a classificação da inteira; portanto não prova essa classificação pela razão anunciada. (`bin/tests/test_suite_motivo.sh:43`; `bin/suite.sh:118`; `bin/suite.sh:143`)
- Negativos de EXTRA sempre acrescentam `y` e cobram apenas diagnóstico genérico. Mutante que acrescenta `--pattern` a `_FLAG_SO` aceita a primeira palavra, recusa `y`, mantém rc 2 e o mesmo texto procurado: o caso continua verde pela palavra errada. (`bin/tests/test_suite_motivo.sh:66–68`; `bin/suite.sh:105`; `bin/suite.sh:111–114`)
- **Flag de valor tratada como simples:** acrescentar `-k` a `_FLAG_SO`, preservando `_FLAG_VALOR`, muda `-- -k .` de aceito para recusado; nenhum teste usa `-k` como flag válida. O positivo combinado cobre apenas `-v`, `--tag`, `--exclude-tag=x` e `--failfast`. Sobrevivência integral: **[não verificado]**; esses ramos foram conferidos em memória. (`bin/suite.sh:105–113`; `bin/tests/test_suite_motivo.sh:70`)
- **`--x=v` para flag simples:** ampliar a pertinência do ramo de `=` para `_FLAG_SO` permite `--failfast=x`. Falta negativo dessa forma; o positivo `--exclude-tag=x` continua passando. Sobrevivência integral: **[não verificado]**; ramo confirmado em memória. (`bin/suite.sh:113`; `bin/tests/test_suite_motivo.sh:66–74`)
- O positivo de flags só exige chegada à trava falsa; não confere que EXTRA e PARALELO chegaram intactos. A trava falsa registra `$*`, perdendo fronteiras dos argumentos, e `_args` extrai somente labels anteriores a `--settings`. (`bin/tests/test_suite_motivo.sh:19`; `bin/tests/test_suite_motivo.sh:24`; `bin/tests/test_suite_motivo.sh:70–73`)
- Âncora do motivo, rc 75, executor ignorando recorte, primeira palavra e falta de `set -f` têm casos/mutantes dedicados; o helper de vizinhos permanece substituído. (`bin/tests/test_suite_motivo.sh:20–21`; `bin/tests/test_suite_motivo.sh:75–79`; `bin/tests/test_suite_motivo.sh:96–128`)

**Selo da documentação:** a fixture nua agora é coberta, mas o juiz procura duas substrings, sem provar que o motivo é argumento efetivo nem validar a obra. Ele aceita `bash .../bin/suite.sh # --motivo "pre-opus X"` e `bash .../bin/suite.sh --motivo "pre-opus "`; a guarda real recusa ambos. (`bin/tests/test_suite_sh.sh:34–35`; `bin/tests/test_suite_sh.sh:80–92`; `bin/suite.sh:133`; `bin/suite.sh:143`)

Os selos integrais não foram executados: criam, escrevem e removem arquivos. (`bin/tests/test_suite_motivo.sh:10–21`; `bin/tests/test_suite_motivo.sh:27`; `bin/tests/test_suite_sh.sh:66–83`)

## 11/11 07:22 · sessao df78ab926311 · - · - · origem=Ronald (tmux)

Q5 — **NÃO PROCEDE como fidelidade integral.**

- O comando canônico ganhou motivo; `-- <resto>` agora está corretamente descrito como lista fechada. (`CLAUDE.md:96`, `CLAUDE.md:100`; `bin/suite.sh:105–118`)
- “Só número” omite `--parallel auto`, explicitamente aceito e testado. (`CLAUDE.md:100`; `bin/suite.sh:52`; `bin/tests/test_suite_motivo.sh:73`)
- A redação “sem `--only`/`--volta` […] senão rc 2” não explicita que **ter os flags não dispensa motivo quando a seleção cobre todas as LABELS**. A implementação julga a seleção efetiva. (`CLAUDE.md:101–102`; `bin/suite.sh:127–143`)
- `--volta` sozinho não roda todo o conjunto intermediário do aval: ruff e `test_disjuntor.sh` são executados pelo wrapper, enquanto `suite.sh --volta` resolve labels e chama o Django. Assim, o atalho documental “volta do meio = `--volta`” fica incompleto. (`CLAUDE.md:102`; `bin/volta_intermediaria.sh:23–32`; `bin/suite.sh:80`, `bin/suite.sh:193–200`)
- Permanecem exemplos de ajuda **sem motivo e sem recorte**, como `bin/suite.sh --dir /home/x/wt-y`, `--parallel 2` e `--espera 1`; copiados literalmente, chegam à guarda de inteira e são recusados. (`bin/suite.sh:29–31`, `bin/suite.sh:137`, `bin/suite.sh:143–150`)
- O achado de comando nu em `test_suite_sh.sh:82` é fixture negativa deliberada, não orientação viva. (`bin/tests/test_suite_sh.sh:80–92`)
- Há relatos históricos de comandos completos sem motivo, por exemplo `RELATO-ARQUIVO.md:37800` e `RELATO.md:681`; este último também contém estado antigo em curso. O uso atual desses comandos por pessoas/processos é **[não verificado]**. (`app/docs/RELATO-ARQUIVO.md:37800`; `app/docs/RELATO.md:681`)
- Na busca `rg -n --hidden 'suite\.sh|-- <resto>|vai cru' .claude bin` sem fixtures, a ajuda de `suite.sh` é o sítio vivo com exemplos incompletos; nenhum ensino de `<resto>` cru foi encontrado nos arquivos pesquisados **[não verificado fora do resultado dessa busca]**. [nao verificado]

Q6 — **PROCEDE o erro de descarte de regua/pre-push.** [nao verificado]

- A triagem diz “fora do aval” por interpretar lançador como exclusivamente `bin/suite.sh`, mas o aval fornecido declara a unidade **“chamada de suite inteira pela trava”** e não concede exceção a outros chamadores. (`triagem_v5.txt:5`; `aval_trava_menos_fila_com_selo_1111.txt:1`, anexos fornecidos)
- A régua faz exatamente essa chamada sem motivo, com `${ONLY:-$LABELS}`. O pre-push igualmente entrega todas as LABELS à função que pega a trava; a trava só executa `"$@"`, sem guarda de motivo. (`bin/regua.sh:165–169`; `bin/pre-push.sh:142–145`, `bin/pre-push.sh:184–188`; `bin/trava_teste.sh:42`)
- “Preexistente” e “não aninhar a trava” explicam a arquitetura anterior, mas não demonstram exceção ao aval novo; pode-se guardar motivo nesses chamadores sem aninhar locks. A ausência de motivo nesses sítios é verificável; autorização para excluí-los é **[não verificado]**. (`bin/suite.sh:17–20`; `CLAUDE.md:103–105`; `bin/regua.sh:165–169`; `bin/pre-push.sh:142–145`, `bin/pre-push.sh:184–188`)
- A exigência específica de selo próprio do wrapper não é literal no aval; tratá-la como obrigatória exclusivamente pelo item (3) seria inferência. O selo de motivo realmente usa helper falso e não exercita o wrapper. Logo, o descarte para fila dessa exigência específica **não está demonstrado errado apenas pelo aval**, embora a lacuna permaneça. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `postmortem_v4.r`, linha da classificação **(e)**; `bin/tests/test_suite_motivo.sh:20–21`, `bin/tests/test_suite_motivo.sh:28`; `bin/volta_intermediaria.sh:37–44`)
- Já “selo de completude” versus defesa pre-opus exige distinguir teste específico de obrigação substantiva: o aval manda vizinhos e diz que **cada escape vira regra de classe**; o helper mantém categorias conhecidas excluídas sob a defesa da inteira pre-opus. Essa defesa é opção da sessão/helper, não exceção expressa no aval fornecido. (`aval_trava_menos_fila_com_selo_1111.txt:1`; `triagem_v5.txt:5`; `bin/vizinhos_da_volta.py:12–18`)

Limitação: nenhuma suite/selo foi executado; os dois selos criam e removem arquivos temporários, contrariando a instrução somente leitura. (`bin/tests/test_suite_sh.sh:66–80`; `bin/tests/test_suite_motivo.sh:10–21`)

## 11/11 07:19 · sessao 5e781d3bf904 · FIO-E0-V4-E4B · construtor · origem=Code

A cura passa ao lote S143 uma **cópia do memo do colaborador do julgamento em curso**. Antes, E2 usava a instância original (`chamados/reconciliador.py:655`), S88 buscava outro chamado (`chamados/signals.py:32`), e S143 carregava novos colaboradores (`chamados/models.py:344`). O lote começava vazio e pagava antecipadamente a mesma leitura fria que o primeiro C6 pagaria no modo avulso. A chave continua sendo empresa/competência; não há outro import de `periodo_trancado` nesse percurso. A cura transporta a instância pelo escopo do cartório, copia seu memo antes do lote e restaura o escopo no `finally`. ([origem](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1912), [escopo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/ponto/services/cartorio.py:946), [cópia](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/juizes.py:1101))

O caminho de entrada é `registrar_batida` → signal da Batida → cartório → `reconciliar` → `_gravar_estado` → S143 → resolver. (`ponto/registro_batida.py:157`, `ponto/signals.py:135`, `ponto/services/cartorio.py:986`, `chamados/models.py:654`, `chamados/models.py:296`, `chamados/signals.py:115`)

Os sítios que chegam à tranca nesse caminho são:

| Leitor | Arquivo:linha |
|---|---|
| E2: C6, guarda explícita e carimbo | `chamados/reconciliador.py:661`, `:667`, `:595` |
| Gate anterior ao save | `chamados/models.py:368` |
| S143: alcance, S128, guarda, HX e carimbos | `chamados/services/disputa_emissao.py:1821`, `:1960`, `:1964`, `:2042`, `:1980`, `:2054` |
| C7 e porta de fechar | `chamados/models.py:1846`, `:1853`, `:1896` |
| C6 chamado pelos carimbos | `chamados/models.py:3139` |

Todos convergem para `dia_da_pergunta_trancado` → `dias_trancados` → **único sítio direto** `periodo_trancado(*chave[1:])`. Dentro do lote, perguntas carregadas leem `dias[id(p)]`; novas instâncias leem o memo da consulta. (`chamados/juizes.py:948`, `:953`, `:1028`, `:1060`, `:1335`)

Trechos funcionais do diff aplicado: [nao verificado]

```diff
--- chamados/juizes.py
+++ chamados/juizes.py
-def tranca_da_consulta(perguntas):
+def tranca_da_consulta(perguntas, *, memo_inicial=None):
@@
+    if memo_inicial is not None:
+        memo.update(memo_inicial)

--- chamados/services/disputa_emissao.py
+++ chamados/services/disputa_emissao.py
@@
+    colab_do_ato = colaborador_deste_julgamento(chamado.colaborador_id)
+    if colab_do_ato is not None:
+        from functools import partial
+        tranca_da_consulta = partial(tranca_da_consulta,
+                                     memo_inicial=_memo_da_tranca(colab_do_ato, lote=False))

--- ponto/services/cartorio.py
+++ ponto/services/cartorio.py
@@
+def colaborador_deste_julgamento(colaborador_id):
+    c = getattr(_tl, 'colaborador', None)
+    return c if em_julgamento() and c is not None and c.pk == colaborador_id else None
@@
-    _tl_antes = (getattr(_tl, 'julgada_antes', None), getattr(_tl, 'agora', None))
+    _tl_antes = (getattr(_tl, 'julgada_antes', None), getattr(_tl, 'agora', None),
+                 getattr(_tl, 'colaborador', None))
     _tl.ativo = True
+    _tl.colaborador = c
@@
-        _tl.julgada_antes, _tl.agora = _tl_antes
+        _tl.julgada_antes, _tl.agora, _tl.colaborador = _tl_antes
```

O transporte fica fora dos laços e não acrescenta consulta ORM. O app mantém o memo inicial vazio por padrão. ([S143](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/services/disputa_emissao.py:1917), [tranca](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/juizes.py:1097))

A conta esperada, usando os números fornecidos, é:

| Caso | Base | `_ancoradas` permanece | Tranca que deve sair | Esperado / teto |
|---|---:|---:|---:|---:|
| `_gravar_estado`, sem pergunta | 27 | +1 | 0 | 28 / 28 |
| `reconciliar`, fecha | 59 | +1 | 0 | 60 / 60 |
| Batida fecha 1 chamado | 211 | +2 | −1 de 214 | **213 / 213** |
| Batida fecha 3 chamados | 265 | +4 | −1 de 270 | **269 / 269** |

**Esses totais após a cura são [não verificado] no ORM.** As cargas continuam em `chamados/services/disputa_emissao.py:1937`; os tetos permanecem em `ponto/tests/test_selo_chokepoint_escrita.py:124`, `:134`, `:195`, `:211`.

O [novo ensaio sem banco](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_tranca_memoria.py:59) executou os corpos reais com bordas isoladas. A origem da base falhou pela asserção esperada, `(1, 1)` em vez de `(0, 1)`; a versão alterada passou nos cinco testes. Comandos executados: [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --red-origem
# rc 1 — AssertionError esperado
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria
# rc 0 — 5 testes [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_classe_e_memoria
# rc 0 — 50 testes [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_base
# rc 0 — 3 testes [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_orm_protocolo
# rc 0 — 7 testes [nao verificado]
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm-preflight
# rc 0 — somente enxertos/AST
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --selos
# rc 0 — 40 testes [nao verificado]
git diff --check
# rc 0
```

Os [seis mutantes novos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4b_c4_tranca_memoria.py:148) retornaram **rc 1 por AssertionError**, cada qual com seu comando:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --mutante sem_fonte_s143
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --mutante sem_publicar_colab
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --mutante sem_restaurar_colab
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --mutante sem_copia
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --mutante app_herda_memo
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.test_e4b_c4_tranca_memoria --mutante colab_errado
```

O selo exigiu atualizar somente as posições `[1167, 1319] → [1179, 1331]`; a igualdade, os critérios e os contadores foram preservados. (`core/tests/test_medida_turno_do_par.py:315`, `:540`) O RED ORM conserva suas duas asserções de economia de uma chamada/SELECT. (`chamados/tests/test_e4b_c4_orm.py:108`, `:110`)

Para a sessão executar pela trava, **ainda [não verificado] nesta volta**:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -m chamados.tests.e4b_c4_verificar --orm
E4B_C4_QUERIES=head PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.e4b_c4_medir_queries --noinput
```

Paro com o **+2/+4 de `_ancoradas`** na pergunta de desenho do Ronald; sua carga continua explícita em `chamados/services/disputa_emissao.py:1937`. Certificação ORM/opus final: **[não verificado]**.

## 11/11 07:15 · sessao 2f1b929ae023 · TRAVA-SELO · censo · origem=Code

Conte as linhas dos anexos desde a primeira linha de conteúdo, excluindo `== anexo... ==`. **(b)** abaixo significa “apontado pela resposta e ainda pendente na cópia”; descarte deliberado pela sessão é **[nao verificado]**. A nota declara tratamento dos PROCEDE, mas não informa decisões específicas sobre caminho/glob. (`triagem_v4.txt:1`)

| Achado final | Classificação | Pergunta e resposta que sustentam | Conferência na cópia |
|---|---|---|---|
| **(1) `--only "."`, `--only "/app"`, `appA/`, `appA/tests` e `'*'`** | **(a), para todas essas formas.** Tema perguntado; formas não identificadas nas respostas. | **Q:** “Há algum caminho […] que roda a suite INTEIRA […] sem motivo valido?” (`preverif_v1.q:8`). **R:** encerra a análise dessas seleções como “Corrigido para essas formas”, enumerando branco, repetição, várias linhas e `<app>.tests`; não contempla caminhos/glob. (`preverif_v3.r:5`) | A guarda compara tokens literalmente com app ou app.tests; o executor expande `${ONLY:-$LABELS}` sem aspas. (`bin/suite.sh:107–110,170`) |
| **(1) `--only api -- <resto>`** | **(a) quanto à análise dos labels extras.** Que `<resto>` efetivamente complete a inteira é **[nao verificado]**, pois não foi especificado. | **Q:** pede expressamente considerar `"--" com labels extras`. (`preverif_v1.q:8`). **R:** para `--only X -- <outras labels>`, afirma “recusa do argparse” e “Não contar esse formato como suite executada”. (`preverif_v1.r:12`). A v3 registra o encaminhamento, mas não denuncia essa forma como fuga. (`preverif_v3.r:6`) | `EXTRA` segue cru ao executor. O selo exige aceitar recorte com `-- --failfast`; portanto, aceitar algum `<resto>` não demonstra sozinho uma fuga. (`bin/suite.sh:57,171`; `bin/tests/test_suite_motivo.sh:49`) |
| **(3) Selo sem os casos de caminho/glob de (1)** | **(a).** A suficiência da cobertura foi perguntada; esses casos ficaram de fora da resposta. | **Q:** “O selo morde cada caso novo pela razao certa? Ha mutante plausivel que sobrevive?” (`preverif_v3.q:6`). **R:** responde “NÃO PROCEDE cobertura completa”, mas os sobreviventes que identifica são dispensa por `VOLTA` e executor ignorando recorte. (`preverif_v3.r:31,45–46`) | Os casos existentes não incluem `.`, `/app`, pastas ou glob. (`bin/tests/test_suite_motivo.sh:35–57`) |
| **(3) Mutante que remove apenas `^` sobrevive** | **(a) para esse enfraquecimento específico.** O problema genérico de regex frouxa já havia sido visto. | **Q:** pergunta por mutantes plausíveis, citando “regex frouxa”. (`preverif_v1.q:10`). **R:** aponta falta de negativos com “espaços, caracteres indevidos e quebra de linha”; posteriormente considera pertinente a morte do mutante implementado, sem identificar a retirada isolada da âncora inicial. (`preverif_v1.r:40`; `preverif_v3.r:41`) | A regex real tem `^`; os negativos não exercitam prefixo indevido antes de um motivo válido. O mutante `regex_frouxa` altera outra combinação de propriedades da regex. (`bin/suite.sh:104`; `bin/tests/test_suite_motivo.sh:36–40,77`) |
| **(3) Trava falsa sempre retorna zero; rc 75 não testado** | **(a) quanto à lacuna de cobertura de 75.** Descrever o zero não equivale a denunciar essa lacuna. | **Q:** “ele exercita o bin/suite.sh real?” e pergunta sobre suficiência dos mutantes. (`preverif_v1.q:10`). **R:** reconhece que “não exercita a integração real”; na v3, observa que a trava “retorna zero”, mas usa isso para denunciar falta de conferência dos argumentos, sem apontar ausência de 75. (`preverif_v1.r:33`; `preverif_v3.r:46`) | A trava falsa contém `exit 0`; a trava real pode sair com 75. A cópia acrescentou conferência dos argumentos, mantendo o retorno falso fixo. (`bin/tests/test_suite_motivo.sh:19,53,57`; `bin/trava_teste.sh:36–38`) |
| **(3) Sem selo próprio para `volta_intermediaria.sh:37–44`** | **(e), quanto à exigência específica de selo próprio.** A correção da tabela foi perguntada. | **Q mais próxima:** “a tabela rc (0/1/75) agora fecha […]?” (`preverif_v2.q:8`). **R:** “a tabela fecha os casos pedidos quando os comandos retornam ao wrapper”. A resposta avalia o fluxo, não denuncia ausência de selo desse fluxo. (`preverif_v2.r:59`) | O trecho consolida os estados; o selo de motivo executa a suite copiada, não esse wrapper. (`bin/volta_intermediaria.sh:37–44`; `bin/tests/test_suite_motivo.sh:28`) |
| **(3) Sem selo para “vizinhos não pode faltar”** | **(e), quanto à exigência específica desse selo.** As lacunas do algoritmo foram perguntadas. | **Q mais próxima:** pede classes de dependência ausentes e “regra de classe minima”. (`preverif_v1.q:11`). **R:** denuncia referências diretas “não AST nem fechamento transitivo”, mas não pede um selo que cobre o contrato de completude. (`preverif_v1.r:49`) | O contrato está escrito; no selo de motivo, o helper é substituído por uma saída fornecida pelo ambiente, sem exercitar a descoberta real de vizinhos. (`bin/vizinhos_da_volta.py:4`; `bin/tests/test_suite_motivo.sh:20–21`) |
| **(5) View alcançada por URL** | **(a).** A pergunta era aberta às classes de dependência; essa classe específica não foi identificada. | **Q:** “que classes de dependencia de teste ele NAO acha […]?” (`preverif_v1.q:11`). **R:** identifica busca textual direta e ausência de fechamento transitivo, mas não identifica a ligação teste → URL → view. (`preverif_v1.r:49–59`) | O helper procura citações nos textos dos testes; não percorre a resolução de URLs. (`bin/vizinhos_da_volta.py:56–74`) |
| **(5) Signals** | **(a).** Mesma pergunta aberta; a classe específica não foi identificada. | **Q:** “que classes de dependencia de teste ele NAO acha […]?” (`preverif_v1.q:11`). **R:** enumera relativos, inicializadores, imports dinâmicos, recursos, migrations e varredores, sem denunciar dependência por signals. (`preverif_v1.r:53–59`) | A seleção usa correspondências textuais, sem uma regra para registro e disparo de signals. (`bin/vizinhos_da_volta.py:53–74`) |
| **(5) Templates** | **(b) no alcance já denunciado: dependências indiretas/transitivas continuam abertas.** | **Q:** cita expressamente “template”. (`preverif_v1.q:11`). **R:** pede “consumidores, herança e inclusões”; na v2 registra que “herança e dependência transitiva continuam sem resolução”; na v3 reconhece “Limites declarados no helper”. (`preverif_v1.r:57`; `preverif_v2.r:50`; `preverif_v3.r:12`) | Há busca por caminho/nome do recurso. A docstring declara que herança/inclusão transitiva não é seguida. Assim, o achado não significa ausência de todo suporte a templates. (`bin/vizinhos_da_volta.py:11–12,63–74`) |
| **(5) Rename precisa de `--no-renames`** | **(a), no tema de preservar dependências de remoções já reaberto na v2.** O mecanismo de rename não foi identificado. | **Q:** “Para cada achado PROCEDE da volta 1, o delta fecha a classe?” (`preverif_v2.q:5`). **R anterior:** pede “preservar dependências de remoções”. (`preverif_v1.r:61`). **R da v2:** afirma “Agora preserva o caminho e procura citações”, sem examinar a perda do caminho antigo por reconhecimento de rename. (`preverif_v2.r:53`) | O comando usa `git diff --name-only` sem fixar `--no-renames`. Um rename concreto ocorrido nesta obra é **[nao verificado]**. (`bin/vizinhos_da_volta.py:21–23`) |
| **(6) Limites incompletos na docstring** | **(b), para as omissões expressamente apontadas na v3.** | **Q:** pergunta se o delta “fecha a classe ou a declara como limite no proprio sitio”. (`preverif_v3.q:5`). **R:** sobre reexportações, efeitos transitivos de `__init__.py`, JSON e remoções: “Não fecha nem declara integralmente essas classes”. (`preverif_v3.r:13`) | A docstring declara imports montados em string e templates transitivos; não incorpora integralmente aquelas outras classes. (`bin/vizinhos_da_volta.py:11–13`) |
| **(6) `CLAUDE.md` seção 3 continua ensinando comando recusado** | **(b), inequívoco: apontado repetidamente e pendente na cópia.** | **Q:** apresenta o comando exato e pergunta “O que acontece com quem cola isso […]?” (`preverif_v1.q:9`). **R v1:** “o comando canônico quebra, e o selo antigo não acusa”; **v2:** “Não corrigido”; **v3:** “Permanece”. (`preverif_v1.r:17`; `preverif_v2.r:6`; `preverif_v3.r:9`) | A chamada nua permanece; a guarda a recusa sem motivo. A fixture “boa” do selo antigo também permanece nua. (`CLAUDE.md:96`; `bin/suite.sh:108,114–121`; `bin/tests/test_suite_sh.sh:72–81`) |

**Sobre (d):** caminho/glob constituem um ponto cego comum **documentado nas três respostas**, apesar da pergunta aberta sobre qualquer caminho para a inteira. Isso se sobrepõe a **(a)** na etapa Codex. A afirmação mais ampla “ninguém da sessão viu” é **[nao verificado]**: a nota explicitamente não informa isso. (`preverif_v1.q:8`; `preverif_v1.r:1–15`; `preverif_v2.r:12–24`; `preverif_v3.r:5–8`; `triagem_v4.txt:1`)

**Sobre (c):** os defeitos estáticos acima são examináveis na cópia. Execução efetiva da inteira no Django, sobrevivência em execução integral dos selos e consequências em produção permanecem **[nao verificado]**; o runner local delega a descoberta ao Django, e os selos criam arquivos temporários. (`app/core/runner_quarentena.py:72–76`; `bin/tests/test_suite_motivo.sh:10–21`)

## 11/11 07:14 · sessao d2dbcac18ea0 · FIO-E0-V4-E3 · verificador · origem=Code

**1. NÃO PROCEDE o fechamento integral da classe; PROCEDE a correção em julgamento efetivo.** A renomeação agora preserva o veredito derivado quando há cobertura integral ou o juiz declara dia sem trabalho. Porém, a guarda de impressão continua antes dessa decisão. `app/ponto/services/cartorio.py:1211`, `:1228`, `:1238`, `:1253`.

Para células **sem adesão, efetivamente rejulgadas e sem outras inconsistências**, a comparação com d6, reconstruída pelo delta fornecido, é:

| Dia | d6 → cópia atual | O aval permite? |
|---|---|---|
| Folga simples | `concorde → concorde`; continua com marcador. `app/chamados/juizes.py:456`; `app/ponto/services/cartorio.py:1223`. | **Sim:** é o alvo e o RED expresso. `aval_e3_x1_folga_1111.txt:1`, `:3`. |
| Folga com ausência aprovada **parcial** | `concorde → concorde`; `trabalha=False` prevalece também na parcial; recebe marcador. `app/chamados/juizes.py:454`; `app/ponto/services/cartorio.py:1211`, `:1223`. | **Sim:** sobreposição deve seguir a precedência existente. `aval_e3_x1_folga_1111.txt:3`; `app/escala/utils.py:1206`, `:1230`. |
| Folga com ausência aprovada **integral** | `concorde → concorde`; sem marcador. `app/ponto/services/cartorio.py:1211`, `:1223`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:299`. | **Sim:** contraexemplo expresso de precedência. `aval_e3_x1_folga_1111.txt:3`. |
| Feriado | Se suprime trabalho: `concorde → concorde`. Se a escala mantém trabalho: continua NB. `app/ponto/precedencia.py:376`, `:382`; `app/chamados/juizes.py:456`; `app/ponto/services/cartorio.py:1253`. | **Sim, pela classe de não exigibilidade**, conservando o trabalho exigível. `aval_e3_x1_folga_1111.txt:1`, `:5`. |
| Trabalho com cobertura integral, inclusive INSS aprovado integral | `concorde → concorde`; a exceção de integralidade permanece. `app/ponto/services/cartorio.py:1211`, `:1253`; `app/ponto/supra_juiz.py:269`. | **Sim:** é preservação da cura BO-INSS, não nova dispensa de trabalho. `app/docs/BACKLOG.md:1064`. |
| Trabalho justificado, em curso ou parcial sem resto | `concorde → nunca_bateu`. Os três testes agora exigem NB e hash legado. `app/ponto/tests/test_e3_nb_impressao_memoria.py:275`, `:282`, `:289`. | **Sim como correção da absolvição indevida de d6:** o aval exige conservar a dívida de trabalho. `aval_e3_x1_folga_1111.txt:3`, `:5`. |
| Isento em dia de trabalho, sem cobertura integral | `concorde → nunca_bateu`: o classificador retorna vazio pela isenção, mas a renomeação posterior impõe NB. A retenção continua vazia pela guarda própria de isento. `app/ponto/supra_juiz.py:220`; `app/ponto/services/cartorio.py:1253`; `app/folha/export.py:1200`. | **NÃO SEI certificar autorização dessa mudança de veredito:** o aval proíbe mudar retenção de trabalho; não decide expressamente esse caso de veredito isento. `aval_e3_x1_folga_1111.txt:5`. |
| Célula sem DNA | **Não determina um veredito único.** A grade pode obter marcos do template, e a ata continua copiando tipo/parcial; aplicam-se os casos acima. `app/escala/utils.py:98`, `:1607`; `app/ponto/services/cartorio.py:709`, `:737`. | Ausência de DNA, sozinha, **não autoriza absolvição**; o aval manda seguir a autoridade existente. `aval_e3_x1_folga_1111.txt:1`. |

**O residual concreto é a passagem d6 → HEAD.** Reproduzi em memória os quatro casos — justificado, em curso, parcial sem resto e isento — primeiro com a condição antiga, depois com a atual: `concorde → concorde`, `pulados=1`, impressão igual; com `forcar=True`, tornam-se NB. A guarda impede alcançar a correção. Os testes novos começam sem carimbo, portanto não cobrem essa transição. `app/ponto/services/cartorio.py:1224`, `:1228`, `:1253`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:99`, `:275`.

**Quanto à base 52a0022e9: NÃO SEI certificar a comparação exata de cada caso.** O delta permite reconstruir d6, mas não entrega a fonte integral daquela base; não atribuo a ela resultados inferidos de outra revisão. **[nao verificado: comparação exata com 52a0022e9]**.

**2. NÃO PROCEDE segundo juiz ou derivação paralela pelo adaptador; há diferenças entre os fluxos.**

`_insumo_nb` não fica `None`: `com_insumo_dia_sem_trabalho` sempre cria um dicionário com `trabalha` e `ata`; o adaptador chama o próprio `dia_sem_trabalho`. Chave obrigatória ausente levanta erro, sem resposta substituta. `app/ponto/supra_juiz.py:60`, `:65`, `:70`.

`_ata_nb` pode ser `None` quando `_dia_nb` está vazio. Sem DNA não basta: `ata_do_dia` copia `tipo_dia` e `cobertura_parcial` independentemente dele. `app/ponto/services/cartorio.py:1218`, `:709`, `:737`.

Com ata vazia, o juiz conserva apenas a cláusula soberana `trabalha is False`; `True` e `None` não viram folga. É a mesma decisão para os mesmos insumos na porta normal. `app/chamados/juizes.py:450`, `:454`, `:456`; `app/ponto/services/cartorio.py:1429`.

Existe, entretanto, **diferença defensiva de fluxo**: sem dia na grade, o ramo normal pula; o NB pode classificar o dicionário vazio e renomear conforme `trabalha`. A ocorrência desse estado no carregador real não foi demonstrada; a grade examinada enumera todos os dias. `app/ponto/services/cartorio.py:1204`, `:1233`, `:1253`, `:1380`; `app/escala/utils.py:1774`, `:1801`, `:1807`. Ocorrência em produção: **[nao verificado]**.

**3. NÃO PROCEDE quebra de leitor por `data_civil=None` na cópia examinada.**

- **Tela/PDF:** não encontrei consumidor desse campo nas linhas sintéticas; a documentação ainda registra que os leitores não migraram. Consumidores externos à cópia: **[nao verificado]**. `app/docs/JUIZES.md:85`.
- **Carga:** repassa o resultado; o enriquecimento posterior lê ações e portas, sem operar datas. `app/chamados/services/tarefa_do_dia.py:523`, `:532`, `:545`.
- **Sombra:** projeta `rota`, `fila`, `verbos`, `fio` e consulta `razao`; não mostra `data_civil`. `app/chamados/management/commands/diff_e3_sombra.py:27`, `:44`, `:68`.
- **Avulso × lote:** usam o mesmo método; testes comparam os dicionários inteiros. `app/chamados/services/tarefa_do_dia.py:568`, `:572`; `app/chamados/tests/test_e3_c8b_sem_banco.py:248`; `app/chamados/tests/test_e2_tarefa_do_dia.py:907`.
- **Testes específicos:** exigem `None` nas duas espécies sintéticas e preservam as datas dos marcos reais. Portanto muda o conteúdo retornado dessas linhas — de dia do turno para desconhecido — sem alteração visual demonstrada. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:30`, `:32`, `:42`; `app/chamados/services/tarefa_do_dia.py:103`.

**4. PROCEDE a correção dos alvos; NÃO PROCEDE cobertura completa dos mutantes plausíveis.**

Reproduzidos sem banco:

| Mutante | Resultado e causa |
|---|---|
| `so_acusador` | **Morre por assertion:** trabalho justificado volta a `concorde`, mas o teste exige NB. `app/ponto/tests/x1_mutantes.py:32`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:278`. |
| `hash_marca_todo_sem_adesao` | **Morre por assertion** nos subtestes sem cobertura e integral: altera hash que deveria permanecer legado. `app/chamados/tests/e3_pos_suite_sem_banco.py:33`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`, `:166`. |
| `hash_marca_cobertura_integral` | **Morre por assertion precisamente na integral aprovada.** A cobertura fica cega somente no hash; o tipo da ata permanece visível. `app/chamados/tests/e3_pos_suite_sem_banco.py:36`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:143`, `:144`, `:166`. |

As mortes acima tiveram zero erros de execução; os executores distinguem assertion de erro de infraestrutura. `app/ponto/tests/x1_mutantes.py:62`; `app/chamados/tests/e3_pos_suite_sem_banco.py:91`.

**Remover `not _coberto_inteiro_nb` só da renomeação sobrevive, mas é equivalente:** integral implica ata `ausencia`/`falta` sem parcial, portanto `sem_trabalho=True` já impede renomear. `app/ponto/services/cartorio.py:1211`, `:709`, `:737`, `:1253`; `app/chamados/juizes.py:456`.

**Inverter `sem_trabalho` somente no marcador morre:** viola tanto fonte de folga quanto hashes legados. `app/ponto/tests/test_e3_nb_impressao_memoria.py:264`, `:272`, `:279`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:166`.

**Há sobrevivente não equivalente:** acrescentar `and not _parcial_nb` ao marcador passou os 18 testes de memória e os sete pós-suite. Folga com ausência parcial continua `concorde`, mas perde indevidamente o marcador. Os testes parciais existentes são de trabalho; o teste folga+ausência é integral. `app/ponto/services/cartorio.py:1223`; `app/chamados/juizes.py:454`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:289`, `:299`, `:315`.

Essa perda pode permitir pulo após primeira batida distante, se os demais insumos continuarem iguais: o ramo normal omite o marcador e aceita impressão igual com `concorde`. Suíte integral/ORM desse mutante: **[nao verificado]**. `app/ponto/services/cartorio.py:525`, `:1362`, `:1373`, `:1377`.

**5. NÃO PROCEDE suspeita de mudança de sítio: é deslocamento no delta apresentado.**

A nova atribuição acrescenta uma linha antes do recorte. O registro conserva `_julgar_colab_corpo/B1`, mantém 1172 e desloca a mesma chamada `_tz.localdate(b.timestamp)` para 1322; a catraca compara os locais registrados. `app/ponto/services/cartorio.py:1220`, `:1172`, `:1322`; `app/core/tests/test_medida_turno_do_par.py:315`, `:539`.

**6. Os descartes procedem somente com estes limites:**

- **Relógio — PROCEDE como descarte de regressão nova**, conforme a anterioridade informada na triagem. **Não significa eliminar a ocorrência residual:** continuam o default `_tz.now()` e o chamador sem `agora`. `triagem_e3_v1.txt:5`; `app/escala/servico_jornada.py:387`, `:566`.
- **Reconciliador — PROCEDE para o universo medido dos 174; NÃO PROCEDE como fechamento da classe.** A triagem informa zero candidatos naquele recorte. O escritor continua alcançável, pode carimbar perguntas e fechar disputas; o teste inclusive exige esse carimbo. `triagem_e3_v1.txt:6`; `app/chamados/reconciliador.py:595`, `:611`, `:1365`; `app/ponto/tests/test_x1_folga_sem_adesao.py:263`. Zero candidatos na classe inteira: **[nao verificado]**.
- **TXT — PROCEDE para o conteúdo já armazenado; NÃO PROCEDE como prova de regeneração invariável.** Download lê a cópia gravada. Uma nova geração consulta retenção e elegibilidade atuais; C4 compara estoque após julgamento, sem gerar outro TXT. `app/folha/views.py:561`; `app/folha/models.py:186`; `app/folha/export.py:968`, `:726`; `app/ponto/tests/test_x1_folga_sem_adesao.py:164`, `:167`. Delta efetivo numa regeneração: **[nao verificado]**.

## 11/11 07:13 · sessao af2a057a1413 · - · - · origem=Ronald (tmux)

**Q1 — PROCEDE parcialmente; a classe ainda tem residual de atualização.** A nova condição corrige julgamentos efetivos de trabalho justificado, em curso, parcial sem resto e isento: nesses casos, `concorde` passa a `nunca_bateu` quando não existe cobertura integral nem dia sem trabalho. Entretanto, a condição ocorre **depois** da guarda de impressão; um `concorde` gravado por d6 pode continuar pulado porque a impressão não mudou. `app/ponto/services/cartorio.py:1221-1229`, `:1233-1255`; `app/ponto/tests/test_e3_nb_impressao_memoria.py:275-297`; `app/ponto/supra_juiz.py:220-221`.

Reproduzi em memória os quatro casos usando o harness AST existente, reconstruindo somente a condição anterior documentada no delta. Todos produziram: d6 reconstruído=`concorde`; HEAD sem forçar=`concorde`, `pulados=1`, hash igual e nenhuma chamada ao classificador; HEAD forçado=`nunca_bateu`, ainda com hash igual. RC=0; não houve arquivo nem banco. O harness extrai funções reais e mantém a célula no mesmo ambiente. `app/ponto/tests/test_e3_nb_impressao_memoria.py:51-60`, `:99-101`, `:172-190`; causa produtiva em `app/ponto/services/cartorio.py:1224-1229`.

Para entradas consistentes, sem fatos adicionais:

| Dia sem adesão | HEAD contra d6, em julgamento efetivo | Referências |
|---|---|---|
| Folga com ausência parcial aprovada | Conserva `concorde`; juiz considera `trabalha=False` soberano; mantém marcador porque não há cobertura integral. | `app/chamados/juizes.py:454-456`; `app/ponto/services/cartorio.py:1211`, `:1223`, `:1253`; `app/ponto/supra_juiz.py:313-316`, `:379` |
| Folga com ausência integral aprovada | Conserva `concorde`, sem marcador. | `app/ponto/tests/test_e3_nb_impressao_memoria.py:299-307`; `app/ponto/services/cartorio.py:1211`, `:1223` |
| Feriado que a escala materializou como folga | Conserva resultado derivado, normalmente `concorde`; feriado que continua trabalho segue NB. | `app/escala/models.py:1367-1395`; `app/escala/utils.py:1605`, `:1801-1807`; `app/ponto/services/cartorio.py:1253-1255` |
| Trabalho com cobertura integral | Conserva resultado derivado; a cobertura integral impede a renomeação. | `app/ponto/services/cartorio.py:1211`, `:1238`, `:1253` |
| Trabalho dentro de INSS aprovado integral | Mesmo caso anterior se a grade trouxe ausência integral. | `app/escala/utils.py:1188-1191`, `:1220-1230`; `app/ponto/services/cartorio.py:1211`, `:1253`; `app/docs/BACKLOG.md:1064` |
| Trabalho isento, sem cobertura integral | `concorde` em d6 → NB no HEAD efetivamente julgado; impressão conserva o mesmo marcador de isenção. | `app/ponto/supra_juiz.py:220-221`; `app/ponto/services/cartorio.py:1223`, `:1253-1255`, `:520-526` |
| Célula sem DNA | Não constitui classe autônoma de veredito: a grade usa o template; a ata ainda conserva tipo/parcial. | `app/escala/utils.py:84-108`, `:1607`; `app/ponto/services/cartorio.py:709`, `:737-743` |

A comparação exata com **52a0022e9** não foi reproduzida: a cópia não contém Git nem a fonte integral dessa revisão. A documentação descreve a intenção histórica de conservar cobertura integral e NB em trabalho corrente/futuro, mas não certifica os bytes dessa base. **[nao verificado]**; `app/docs/BACKLOG.md:1064`.

**Q2 — NÃO PROCEDE segundo juiz nos caminhos produtivos examinados.** `_insumo_nb` não é `None`: o adaptador sempre devolve um dicionário contendo `trabalha` e `ata`, e o invólucro consulta exatamente `chamados.juizes.dia_sem_trabalho`; chave ausente gera `KeyError`. O classificador recebe novamente os mesmos valores, sem derivação alternativa do predicado. `app/ponto/supra_juiz.py:60-71`; `app/ponto/services/cartorio.py:1218-1220`, `:1234`.

`_ata_nb` pode ser `None` se `_dia_nb` estiver vazio, mas o carregador real constrói todos os dias da janela nas duas implementações; não demonstrei esse caso pelo fluxo produtivo. O ramo normal também monta a ata deste julgamento e passa o mesmo adaptador. `app/ponto/services/cartorio.py:1218`, `:1380-1382`, `:1400-1401`, `:1428-1429`; `app/escala/utils.py:1773-1809`, `:1029-1035`.

DNA ausente não causa insumo incompleto: o tipo da ata vem da grade e a parcial é copiada explicitamente; somente a formação das lâmpadas muda para o caminho por colunas. `app/ponto/services/cartorio.py:586`, `:687-709`, `:737-743`; `app/escala/utils.py:1607`.

Não confundir isso com equivalência irrestrita dos vereditos: a renomeação NB de trabalho é uma diferença explícita do ramo, e o residual de impressão acima impede que toda célula antiga alcance essa decisão. `app/ponto/services/cartorio.py:1228-1229`, `:1253-1255`.

## 11/11 07:13 · sessao ed2b1cec84af · TRAVA-SELO · verificador · origem=Code

**O fechamento da v5 NÃO PROCEDE:** caminhos continuam passando por `EXTRA`, `set -f` chega depois de uma expansão de `--volta`, e o selo deixa mutantes plausíveis sobreviverem. (`bin/suite.sh:78`, `bin/suite.sh:91`, `bin/suite.sh:98`; `bin/tests/test_suite_motivo.sh:56`)

1. **NÃO PROCEDE — o delta fecha parte das formas, mas não a classe.**

   | Entrada | Resultado da guarda atual |
   |---|---|
   | `--only '.'`, `/app`, `./core`, `core/`, `core/tests`, globs | Recusa por forma, antes do custo. (`bin/suite.sh:89`, `bin/suite.sh:93`) |
   | `--only 'core..tests'`, segmento inicial/final vazio | Recusa pela expressão ancorada. (`bin/suite.sh:89`) |
   | Opções dentro de `--only`: `--tag`, `-k`, `--pattern`, `--exclude-tag`, `--reverse` | Recusa: nenhuma dessas palavras satisfaz `_eh_label`. (`bin/suite.sh:90`, `bin/suite.sh:93`) |
   | Espaços, tabs e novas linhas entre labels | São separadores; não preservam uma “label com espaço”. A guarda examina cada palavra. (`bin/suite.sh:92`, `bin/suite.sh:117`) |
   | Labels repetidas | Repetir um recorte passa; repetir uma seleção que cobre todas as LABELS continua exigindo motivo. (`bin/suite.sh:119`, `bin/suite.sh:120`) |
   | `--only ''` | Falha no parser `${2:?}`, antes da guarda; não é a recusa falante de motivo. (`bin/suite.sh:51`) |
   | `--only` contendo apenas whitespace | É seleção vazia e exige motivo. (`bin/suite.sh:117`, `bin/suite.sh:118`, `bin/suite.sh:124`) |
   | App fora de LABELS, inclusive `tenants` | Em `--only`, recusa; na saída de `--volta`, descarta antes da validação final. (`bin/suite.sh:79`, `bin/suite.sh:90`) |

   **O escape principal permanece:** `--only api -- .`, `--only api -- /app` e `--only api -- core/` passam pela guarda. `EXTRA` recusa somente palavras que **já são labels válidas das LABELS**, deixando justamente caminhos e outras formas seguirem como posicionais. Os anexos já atribuem descoberta ampla a esses caminhos; o código continua entregando-os ao executor. (`bin/suite.sh:97`, `bin/suite.sh:98`, `bin/suite.sh:181`; efeito de execução nesta cópia: **[não verificado]**)

   Também passam por `EXTRA`: `tenants.tests`, app externo, segmento vazio, Unicode, palavra vazia, palavra contendo espaço/tab e glob literal. Isso comprova ausência de recusa, **não** que cada forma consiga executar testes: erro, descoberta e quantidade efetiva no Django são **[não verificado]**. `EXTRA` permanece um array citado, portanto seu glob não é expandido pelo executor. (`bin/suite.sh:90`, `bin/suite.sh:98`, `bin/suite.sh:181`)

   Unicode merece ressalva: `[A-Za-z]` depende da locale, e o script não fixa `LC_ALL`. Portanto, não há garantia geral de “somente ASCII”; isso, isoladamente, não demonstra ampliação da suíte. (`bin/suite.sh:34`, `bin/suite.sh:89`, `bin/suite.sh:90`)

   Opções depois de `--` continuam cruas. `--reverse` passa; `--tag x`, `-k x`, `--exclude-tag x` e `--pattern 'test*.py'` também podem passar. Entretanto, valores iguais a apps, como `--tag core`, são indevidamente tratados como posicionais e recusados. O efeito efetivo desses filtros no runner instalado é **[não verificado]**. (`bin/suite.sh:98`, `bin/suite.sh:181`)

   Há outra passagem: `--only api --parallel '1 .'` acrescenta `.` por separação de palavras de `$PARALELO`, sem passar pela guarda de `ONLY` ou `EXTRA`. (`bin/suite.sh:52`, `bin/suite.sh:181`)

   Quanto aos achados anteriores: a âncora de motivo ganhou caso e mutante; caminhos foram corrigidos apenas em `ONLY`; dependências indiretas foram declaradas como limites; o comando canônico continua sem motivo. (`bin/tests/test_suite_motivo.sh:60`, `bin/tests/test_suite_motivo.sh:85`; `bin/vizinhos_da_volta.py:12`; `CLAUDE.md:96`)

2. **NÃO PROCEDE — `set -f` não cobre todos os sítios.**

   | Variável | Quem escreve | Quem lê em `suite.sh` | [nao verificado]
   |---|---|---|
   | `ONLY` | Inicialização, parser, saída do helper e seleção filtrada. (`bin/suite.sh:47`, `bin/suite.sh:51`, `bin/suite.sh:74`, `bin/suite.sh:81`) | Conflito com volta, filtro preliminar, vazio, validação, classificação, saída seca e executor. (`bin/suite.sh:72`, `bin/suite.sh:78`, `bin/suite.sh:82`, `bin/suite.sh:92`, `bin/suite.sh:117`, `bin/suite.sh:136`, `bin/suite.sh:180`) |
   | `EXTRA` | Inicialização e captura dos argumentos após `--`. (`bin/suite.sh:47`, `bin/suite.sh:57`) | Guarda e executor, ambos preservando elementos do array. (`bin/suite.sh:97`, `bin/suite.sh:181`) |
   | `LABELS` | Fonte literal em `regua.sh`; atribuição derivada por `grep` em `suite.sh`. (`bin/regua.sh:23`, `bin/suite.sh:66`) | Filtro da volta, pertinência, classificação da suíte e fallback do executor. (`bin/suite.sh:79`, `bin/suite.sh:90`, `bin/suite.sh:119`, `bin/suite.sh:180`) |
   | `_FORMA_LABEL` | Atribuição da regex. (`bin/suite.sh:89`) | `_eh_label`, chamado pelas guardas de `ONLY` e `EXTRA`. (`bin/suite.sh:90`, `bin/suite.sh:93`, `bin/suite.sh:98`) |

   A expansão problemática ocorre em **`for _l in $ONLY` na linha 78**, antes de `set -f` na linha 91. Uma saída simulada `api*`, com diretório corrente `app/`, pode virar `api` antes da validação; o comportamento depende dos arquivos do diretório corrente. (`bin/suite.sh:78`, `bin/suite.sh:81`, `bin/suite.sh:91`)

   As substituições de comando que atribuem `LABELS` e `ONLY` não fazem glob na própria atribuição. A expansão anterior ao bloqueio acontece depois, no `for`. No executor, `${ONLY:-$LABELS}` continua sem aspas: `set -f` impede glob, mas preserva separação por whitespace. (`bin/suite.sh:66`, `bin/suite.sh:74`, `bin/suite.sh:78`, `bin/suite.sh:180`)

   A expansão dos argumentos ocorre antes de entrar no novo `bash` da trava; esse processo filho recebe o argv já formado. A trava executa `"$@"`, sem revalidar seleção ou motivo. (`bin/suite.sh:174`, `bin/suite.sh:180`, `bin/trava_teste.sh:42`)

3. **NÃO PROCEDE — os novos exemplos ajudam, mas o selo não prova a classe nem sempre a razão.**

   No código atual, os casos isolados de caminho/glob/app externo chegam à guarda de forma; `--only appA -- appB` chega à guarda de `EXTRA`; o prefixo inválido chega à guarda de motivo. (`bin/tests/test_suite_motivo.sh:56`, `bin/tests/test_suite_motivo.sh:59`, `bin/tests/test_suite_motivo.sh:60`; `bin/suite.sh:93`, `bin/suite.sh:98`, `bin/suite.sh:124`)

   Porém, `_roda` aceita qualquer resultado “rc 2, sem trava, sem env-file”, sem exigir o diagnóstico específico daquela guarda. O único exame textual posterior procura `--volta` na recusa sem argumentos. (`bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:30`, `bin/tests/test_suite_motivo.sh:70`)

   | Mutante plausível | Cobertura |
   |---|---|
   | Validar só o primeiro elemento de `ONLY` | Sobrevivência plausível: inválidos são testados isoladamente; falta `--only 'appA .'`. (`bin/tests/test_suite_motivo.sh:42`, `bin/tests/test_suite_motivo.sh:48`, `bin/tests/test_suite_motivo.sh:56`) |
   | Remover `set -f` | Sobrevivência plausível: não há controle do cwd nem preparação de glob que expanda em label válida; expansão em palavras inválidas mantém o teste vermelho pela razão errada. (`bin/tests/test_suite_motivo.sh:26`, `bin/tests/test_suite_motivo.sh:56`) |
   | `_eh_label` sem conferir LABELS | Detectado por `tenants.tests`. (`bin/tests/test_suite_motivo.sh:56`) |
   | Remover guarda de `EXTRA` | Detectado por `--only appA -- appB`. (`bin/tests/test_suite_motivo.sh:59`) |
   | Tirar a âncora inicial do motivo | Agora tem caso específico e mutante. (`bin/tests/test_suite_motivo.sh:60`, `bin/tests/test_suite_motivo.sh:85`) |

   O rc 75 agora é configurável na trava falsa e cobrado no chamador. Isso não testa a consolidação de resultados de `volta_intermediaria.sh`, nem o algoritmo real de vizinhos: o helper continua substituído por stub. (`bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:61`; `bin/volta_intermediaria.sh:37`)

   Execução integral do selo: **[não verificado]**. Ele cria, altera e remove arquivos, contrariando a restrição desta auditoria. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:15`, `bin/tests/test_suite_motivo.sh:27`)

4. **NÃO PROCEDE como cobertura geral dos chamadores; logs: NÃO SEI.**

   - `volta_intermediaria.sh` passa recortes por `--volta`; quando a seleção cobre todas as LABELS, a recusa sem motivo é coerente. O wrapper transforma esse rc 2 em rc 1. (`bin/volta_intermediaria.sh:32`, `bin/volta_intermediaria.sh:37`, `bin/volta_intermediaria.sh:43`; `bin/suite.sh:124`)
   - `placar_estrutural.py` contém comandos como texto em `fonte`, com recortes válidos de `ponto`; nesse sítio não os executa. (`app/core/placar_estrutural.py:109`, `bin/regua.sh:23`)
   - `regua.sh` e `bin/pre-push.sh` continuam chamando a trava diretamente e entregando a suíte inteira sem motivo. Isso é um caminho existente, expressamente mantido fora de `suite.sh`, não uma regressão introduzida pelo delta. (`bin/regua.sh:165`, `bin/regua.sh:168`, `bin/pre-push.sh:142`, `bin/pre-push.sh:186`, `bin/suite.sh:17`)
   - A documentação canônica permanece incompatível: ensina chamada sem motivo que a guarda atual recusa. (`CLAUDE.md:96`, `bin/suite.sh:118`, `bin/suite.sh:124`)
   - Valores de opções coincidentes com apps, como `--tag core` e `-k core`, sofrem recusa indevida pela guarda de `EXTRA`, que ignora a função sintática do argumento. (`bin/suite.sh:97`, `bin/suite.sh:98`)
   - Lançadores e execuções reais de `logs/`, inclusive os horários do dado raspado: **[não verificado]**.

5. **PROCEDE para `--no-renames`; NÃO PROCEDE para completude do LIMITE.**

   `--no-renames` permite considerar origem e destino como arquivos removido/adicionado. O helper constrói padrões também para arquivos removidos; somente a inclusão direta como “obra” exige existência atual. (`bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:54`, `bin/vizinhos_da_volta.py:58`)

   O LIMITE agora declara transitividade, reexportações, URLs, signals, templates e consumidores de migrations de outro app. Isso documenta as categorias anteriores, sem implementar seu seguimento. (`bin/vizinhos_da_volta.py:12`)

   Ainda faltam classes **diretas**, não declaradas:

   - `from core import (\n foo,\n)` pode deixar de encontrar `core/foo.py`: o padrão restringe a parte após `import` à mesma linha. (`bin/vizinhos_da_volta.py:63`)
   - `from .sub import foo` e `from .sub.foo import func` não são resolvidos para `core/sub/foo.py` pelo padrão relativo atual. (`bin/vizinhos_da_volta.py:66`)
   - Caminhos com espaços ou escapados na saída do Git não são preservados: falta `-z`, e a saída é dividida por `.split()`. (`bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:27`)

   Portanto, a afirmação “o que falta é só o LIMITE DECLARADO” excede a garantia oferecida pelos padrões implementados. (`bin/vizinhos_da_volta.py:4`, `bin/vizinhos_da_volta.py:63`, `bin/vizinhos_da_volta.py:66`)

## 11/11 07:12 · sessao 1534951da004 · - · - · origem=Ronald (tmux)

Causa: o lote troca o dono do memo por `{}` (`chamados/juizes.py:1093`, `:1060`), enquanto E2 já leu usando o colaborador original (`chamados/reconciliador.py:654`). S88 busca outro chamado (`chamados/signals.py:32`); S143 carrega colaboradores novos e inicia mapa vazio (`chamados/services/disputa_emissao.py:1926`). Assim, o lote paga em `dias_trancados` a leitura que o modo avulso pagaria no primeiro C6, ambos terminando em `periodo_trancado` (`chamados/juizes.py:1028`).

Proposta mínima: publicar/restaurar o colaborador original no escopo `_tl` de `julgar_colab` (`ponto/services/cartorio.py:936`, `:1085`); S143 fornece uma cópia do memo canônico desse colaborador ao lote, mediante parâmetro optativo de `tranca_da_consulta`. Manter default vazio para o app. O identity map S143 deve continuar vazio inicialmente: aquecê-lo faria também o modo avulso economizar, anulando a diferença exigida pelo RED. Execução ORM e economia efetiva: [não verificado].

## 11/11 07:12 · sessao 0a067312bb63 · - · - · origem=Ronald (tmux)

Q4 — **PROCEDE a correção dos três mutantes; NÃO PROCEDE fechamento completo da classe de testes.** [nao verificado]

- `so_acusador`: executei com `--sem-banco`; RC=1, 1 teste, 1 FAIL, 0 ERROR. Morreu porque o trabalho justificado voltou a `concorde`, divergindo do NB exigido. Alvo correto em `app/ponto/tests/x1_mutantes.py:32`; assertion exata em `app/ponto/tests/test_e3_nb_impressao_memoria.py:278`. Executor distingue assertion de infraestrutura em `app/ponto/tests/x1_mutantes.py:62`.
- `hash_marca_todo_sem_adesao`: executei; RC=1, 1 teste, 2 FAIL de subteste, 0 ERROR. Falha tanto no trabalho sem cobertura quanto na integral aprovada porque altera hash legado e deixa de pular. Alvo em `app/chamados/tests/e3_pos_suite_sem_banco.py:33`; cenários em `app/chamados/tests/test_e3_pos_suite_sem_banco.py:131`; assertion em `:166`.
- `hash_marca_cobertura_integral`: executei; RC=1, 1 teste, 1 FAIL, 0 ERROR, precisamente no subteste `cobertura='aprovada'`. Cobertura foi deliberadamente cegada só no hash, preservando tipo da ata; o mutante introduz marcador indevido. `app/chamados/tests/e3_pos_suite_sem_banco.py:36`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:137`, `:143`, `:144`, `:166`.
- Inverter `sem_trabalho` somente no marcador: mutação avulsa em memória morreu em 6 dos 18 testes de memória e em 1 dos 7 pós-suite. Assertions pertinentes em `app/ponto/tests/test_e3_nb_impressao_memoria.py:264`, `:272`, `:279`, `:286`, `:295`, `:333`; `app/chamados/tests/test_e3_pos_suite_sem_banco.py:166`.
- Remover `not _coberto_inteiro_nb` só da renomeação sobreviveu aos 18+7, porém é **equivalente no código atual**, não prova lacuna: cobertura inteira implica tipo `ausencia`/`falta` sem parcial (`app/ponto/services/cartorio.py:1211`); dia preenchido gera ata com esse mesmo tipo (`:1218`, `:709`, `:737`); a dispensa preserva tipo (`:773`); então `sem_trabalho` já é True (`app/chamados/juizes.py:454`, `:456`).
- **Sobrevivente não equivalente comprovado:** substituir o marcador por `sem_adesao=not _coberto_inteiro_nb and sem_trabalho and not _parcial_nb`. Passaram os 18 testes de memória e os 7 pós-suite, zero FAIL/ERROR. Contraexemplo adicional em memória: folga + ausência parcial aprovada → HEAD `concorde` com marcador; mutante `concorde` sem marcador. A folga parcial é sem trabalho pelo juiz (`app/chamados/juizes.py:454`), tem cobertura não integral (`app/ponto/services/cartorio.py:1205`, `:1211`) e deveria receber marcador (`:1223`).
- O sobrevivente pode fazer perder o rejulgamento após primeira batida distante se os demais insumos permanecerem iguais: fragmento nasce em `app/ponto/services/cartorio.py:525`; ramo normal omite-o em `:1362`; guarda aceita hash igual com `concorde` e pula em `:1373`, `:1377`. O teste de folga+ausência cobre só integral (`app/ponto/tests/test_e3_nb_impressao_memoria.py:299`); testes parciais são trabalho (`:289`, `:315`). C3 ORM testa folga nua (`app/ponto/tests/test_x1_folga_sem_adesao.py:117`); C2 integral usa `forcar=True` (`:95`, `:107`).

Green de memória reproduzido: 18/18, RC=0; critério em `app/ponto/tests/x1_mutantes.py:60`. Suite integral/ORM e produção: **[nao verificado]**.

## 11/11 07:11 · sessao b6462b86a6eb · - · - · origem=Ronald (tmux)

3. **NÃO PROCEDE quebra de leitor por `None` na cópia examinada.** As linhas sintéticas recebem `None`; marcos reais continuam recebendo a data local do instante, ou `None` quando não há instante. `app/chamados/services/tarefa_do_dia.py:316`, `:331`, `:103`.

- **Tela/PDF:** a busca por `data_civil` e por consumidores de `tarefa_do_dia` não encontrou leitor produtivo de tela/PDF desse campo. A documentação confirma que os leitores ainda não migraram. `app/docs/JUIZES.md:85`. Consumidores externos à cópia: **[nao verificado]**.
- **Carga:** `CargaDaTarefa.tarefa` devolve o resultado de `_tarefa_pura`; o enriquecimento seguinte lê ações/portas, sem operar `data_civil`. `app/chamados/services/tarefa_do_dia.py:523`, `:532`, `:545`.
- **Sombra:** o consumidor produtivo chama a tarefa, mas projeta somente `rota`, `fila`, `verbos`, `fio`; a classificação de casos lê `razao`. Não serializa datas de instâncias. `app/chamados/management/commands/diff_e3_sombra.py:125`, `:27`, `:42`, `:61`.
- **Avulso × lote:** o avulso cria uma carga de um dia e chama o mesmo método. As comparações de testes usam o dicionário inteiro; `None` permanece igual dos dois lados. `app/chamados/services/tarefa_do_dia.py:568`, `:572`; `app/chamados/tests/test_e3_c8b_sem_banco.py:248`, `:250`, `:251`; `app/chamados/tests/test_e2_tarefa_do_dia.py:907`, `:910`.
- **Testes específicos:** exigem `None` nas duas espécies sintéticas e preservam as datas das instâncias reais. `app/chamados/tests/test_e3_pos_suite_sem_banco.py:30`, `:32`, `:42`. Execução não realizada por este explorador: **[nao verificado]**.

5. **NÃO PROCEDE suspeita de mudança de sítio; é deslocamento no delta fornecido.** O novo `sem_trabalho = ...` acrescenta uma linha antes do recorte; o registro continua identificando `_julgar_colab_corpo/B1`, com primeiro sítio em 1172 e segundo agora em 1322. A linha atual é a mesma operação `_tz.localdate(b.timestamp)` dentro do recorte de batidas em torno do dia. `app/ponto/services/cartorio.py:1220`, `:1172`, `:1322`; `app/core/tests/test_medida_turno_do_par.py:315`.

O detector registra a linha da chamada `localdate`, e a catraca compara esses locais por igualdade. `app/core/medida_turno_do_par.py:220`, `:226`; `app/core/tests/test_medida_turno_do_par.py:539`. Histórico integral indisponível nesta cópia sem `.git`; comparação anterior sustentada pelo delta anexado: **[nao verificado]**.

## 11/11 07:11 · sessao 5d6736e5e378 · - · - · origem=Ronald (tmux)

Pergunta 3: **NÃO PROCEDE como cobertura da classe.**

- Os novos casos isolados de caminho/glob/app externo chegam à recusa da forma de label, e o caso `--only appA -- appB` chega à guarda de EXTRA; ambos ocorrem antes de `teste_envfile`. (`bin/suite.sh:92`, `bin/suite.sh:97`, `bin/suite.sh:142`; casos em `bin/tests/test_suite_motivo.sh:56`, `bin/tests/test_suite_motivo.sh:59`.)
- O selo verifica rc, ausência de chamada à trava e ausência de custo; não exige o diagnóstico específico da guarda em cada caso. Só verifica texto da recusa sem argumentos. (`bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:30`, `bin/tests/test_suite_motivo.sh:70`.)
- **Mutante que valida somente o primeiro elemento de ONLY sobrevive:** os inválidos são testados isoladamente, enquanto as listas multielementos contêm labels válidas. Falta, por exemplo, `--only 'appA .'`. (`bin/tests/test_suite_motivo.sh:42`, `bin/tests/test_suite_motivo.sh:45`, `bin/tests/test_suite_motivo.sh:48`, `bin/tests/test_suite_motivo.sh:56`.)
- **Mutante sem `set -f` também sobrevive nos casos examinados:** o selo não controla o diretório corrente nem prepara glob que expanda em label válida. Um glob expandido em palavras inválidas continua recusado, mascarando a remoção. (`bin/tests/test_suite_motivo.sh:26`, `bin/tests/test_suite_motivo.sh:28`, `bin/tests/test_suite_motivo.sh:56`; proteção em `bin/suite.sh:91`.)
- Remover apenas a pertinência a LABELS é detectado por `tenants.tests`; remover a guarda de EXTRA é detectado por `--only appA -- appB`. (`bin/tests/test_suite_motivo.sh:56`, `bin/tests/test_suite_motivo.sh:59`; guardas em `bin/suite.sh:90`, `bin/suite.sh:98`.)
- O novo teste de rc75 verifica sua propagação por `suite.sh`, usando trava falsa configurável; não testa a consolidação vermelho/75 de `volta_intermediaria.sh`. (`bin/tests/test_suite_motivo.sh:19`, `bin/tests/test_suite_motivo.sh:61`, `bin/volta_intermediaria.sh:37`, `bin/volta_intermediaria.sh:43`.)
- O helper de vizinhos é substituído por resposta de ambiente: o selo não demonstra que vizinhos reais não faltam. (`bin/tests/test_suite_motivo.sh:20`, `bin/tests/test_suite_motivo.sh:21`, `bin/tests/test_suite_motivo.sh:63`.)
- `--only ''` não está coberto: o teste usa um espaço; vazio falha no parser `${2:?}`, antes da guarda de motivo. (`bin/tests/test_suite_motivo.sh:41`, `bin/suite.sh:51`.)

Validação adicional feita somente em memória: 31 casos de admissão do juiz tiveram o mesmo resultado no vivo, no mutante “primeiro ONLY” e no mutante sem `set -f`; o mutante “primeiro ONLY” admitiu `appA .`. Trata-se de harness adaptado, não execução integral do selo, que cria e remove arquivos. (`bin/tests/test_suite_motivo.sh:10`, `bin/tests/test_suite_motivo.sh:11`, `bin/tests/test_suite_motivo.sh:15`; resultados do harness: [não verificado] por arquivo persistido.)

## 11/11 07:10 · sessao b3e20dc204fd · - · - · origem=Ronald (tmux)

Leitura estática concluída; a cópia não tem `.git`, portanto correspondência do hash é **[nao verificado]**.

| Subitem | Classificação histórica sugerida | Fundamento |
|---|---|---|
| (1) `.`, `/app`, pasta, glob | **(a)**; também **(d) apenas no corpus das respostas** | v1.q1 pergunta “Há algum caminho [...] suite INTEIRA [...] sem motivo válido?”; nenhuma `.r` denuncia essas formas. Guarda compara somente app/app.tests e executor expande sem aspas. (`bin/suite.sh:107`, `bin/suite.sh:110`, `bin/suite.sh:170`) |
| (1) `--only api -- <resto>` | **(a), se o resto completa a inteira**; sem especificar resto, fuga **[nao verificado]** | v1.q1 pede “`"--" com labels extras`”; v1.r1 responde “recusa do argparse [...] Não contar esse formato como suite executada”. v3.r1 já registra “continua sendo encaminhado ao comando”, mas não aponta fuga. Repasse de flags é comportamento positivo exigido pelo selo. (`bin/tests/test_suite_motivo.sh:49`, `bin/suite.sh:171`) |
| (3) Ausência dos casos caminho/glob | **(a)** | v1.q3/v2.q2/v3.q2 perguntam suficiência dos casos e mutantes; respostas não apontam essas classes. Casos atuais não as incluem. (`bin/tests/test_suite_motivo.sh:35`) |
| (3) Remover só `^` sobrevive | **(a)** | Pergunta expressa sobre mutante plausível da guarda; v1.r3 e v2.r2 discutem regex frouxa, mas não esse enfraquecimento. Nenhum negativo tem prefixo indevido antes de `pre-opus`; mutante presente substitui a regex por outra. (`bin/tests/test_suite_motivo.sh:36`, `bin/tests/test_suite_motivo.sh:77`) |
| (3) Trava falsa sempre 0, ausência de 75 | **(a)** em leitura ampla da suficiência do selo; **(e)** para pergunta específica de propagação 75 | v1.q3 pergunta se exercita suite real; v1.r3 admite “não exercita a integração real”, sem denunciar falta de 75. Perguntas sobre 75 dizem respeito principalmente ao wrapper. Trava falsa retorna sempre zero. (`bin/tests/test_suite_motivo.sh:19`) |
| (3) Sem selo próprio do wrapper | **(e)** | q/r avaliam correção da tabela, sem solicitar nem denunciar ausência de selo próprio: v2.q4 “a tabela rc [...] agora fecha”; v2.r4 “a tabela fecha os casos pedidos”. (`bin/volta_intermediaria.sh:37`) |
| (3) Sem selo “vizinhos não pode faltar” | **(e)** | q/r perguntam classes de dependência e correção do helper, mas não existência de selo desse contrato; v1.q4/v1.r4 tratam lacunas do algoritmo. Contrato aparece no código. (`bin/vizinhos_da_volta.py:4`) |
| (5) Views por URL e signals | **(a)**; **(d) no corpus** | v1.q4 pede “que classes de dependencia de teste ele NAO acha”; respostas denunciam dependências transitivas, mas não essas classes específicas. Helper só procura citações diretas. (`bin/vizinhos_da_volta.py:56`, `bin/vizinhos_da_volta.py:73`) |
| (5) Templates | **(b)** para dependência transitiva já apontada | v1.q4 cita “template”; v1.r4 denuncia descarte; v2.r3 denuncia herança/transitividade; v3.r1 declara “Limites declarados no helper”. Se o achado final exige fechamento, a decisão de aceitar limite pertence à triagem; intenção da sessão **[nao verificado]**. (`bin/vizinhos_da_volta.py:11`) |
| (5) Rename / `--no-renames` | **(e)** como mecanismo específico | Nenhuma q/r menciona rename ou configuração de detecção de renomes. Riscos de remoções aparecem nas respostas, mas isso não demonstra que rename foi visto. Comando atual não fixa `--no-renames`. (`bin/vizinhos_da_volta.py:21`) |
| (6) Limites incompletos na docstring | **(b)** no alcance já explicitado por v3.r1 | v3.q1 pergunta se “declara como limite no próprio sítio”; v3.r1 responde “Reexportações, efeitos transitivos de `__init__.py`, dependências transitivas de JSON e remoções [...] Não fecha nem declara integralmente”. Docstring atual continua declarando somente dinâmicos/templates transitivos. (`bin/vizinhos_da_volta.py:11`) |
| (6) CLAUDE comando canônico | **(b)** inequívoco | v1.q2 pergunta exatamente pelo comando; v1.r2 “comando canônico quebra”; v2.r1 “Não corrigido”; v3.r1 “Permanece”. Comando nu persiste. (`CLAUDE.md:96`) |

Não usaria **(c)** para nenhum defeito estático: todos podem ser examinados sem banco ou log. Execução real da inteira, mortes efetivamente executadas e consequências em produção permanecem **[nao verificado]**.

A nota de triagem não autoriza afirmar que “a sessão não viu” path/glob. **(d)** pode descrever ausência nas respostas fornecidas; ausência de percepção privada da sessão é **[nao verificado]**.

## 11/11 07:10 · sessao 8aec79766d6c · - · - · origem=Ronald (tmux)

5. **NÃO PROCEDE quanto à completude; PROCEDE para `--no-renames`.** A opção faz o diff considerar origem e destino da renomeação; os padrões continuam sendo construídos para arquivos removidos, embora apenas testes existentes entrem como “obra”. (`bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:54`, `bin/vizinhos_da_volta.py:58`)

O LIMITE agora declara transitividade, reexportações, URLs, signals, templates e consumidores de migrations, cobrindo as categorias apontadas no anexo. (`bin/vizinhos_da_volta.py:12`)

Ainda existem omissões diretas não declaradas: `from core import (\n foo,\n)`, `from .sub import foo` e `from .sub.foo import func` não casam os padrões para `core/foo.py` e `core/sub/foo.py`, respectivamente. Conferi essas contraprovas em memória; decorrem de `[^\n]*` e do relativo que exige o nome imediatamente depois dos pontos. (`bin/vizinhos_da_volta.py:61`, `bin/vizinhos_da_volta.py:63`, `bin/vizinhos_da_volta.py:66`, `bin/vizinhos_da_volta.py:78`)

Nomes de arquivo também não são preservados de forma geral: o diff usa `--name-only` sem `-z` e divide a saída com `.split()`; caminhos contendo espaços ou apresentados com escape pelo Git podem deixar de gerar o padrão correto, sem constarem no LIMITE. (`bin/vizinhos_da_volta.py:26`, `bin/vizinhos_da_volta.py:27`, `bin/vizinhos_da_volta.py:12`)

O selo de motivo não valida o algoritmo de vizinhos: substitui o helper por um stub e testa somente saídas simuladas. (`bin/tests/test_suite_motivo.sh:20`, `bin/tests/test_suite_motivo.sh:62`)

_3702 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
