# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 08:40** · publicado em **06/11/2026 08:44** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 08:40 · sessao bf99151fb9fa · LASTRO-PARTE-3 · verificador · origem=Code

**O achado 3 está fechado nos caminhos encontrados que passam por `PerguntaDisputa.save`, no banco `default`. O RED tem mordida por inspeção; sua execução contra o mutante não foi verificada. O selo melhorou, mas ainda tem lacunas de cobertura.** `app/chamados/models.py:2847`; `app/chamados/signals.py:215`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:249`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`.

**1. Atomicidade e caminhos de escrita**

**Achado 3 anterior: corrigido; severidade alta eliminada nos caminhos abaixo.** O `atomic` envolve todo o `super().save()`, incluindo o receiver síncrono que chama a captura e a emissão. Não existe captura da exceção dentro desse receiver. `app/chamados/models.py:2854`; `app/chamados/signals.py:215`; `app/chamados/signals.py:221`.

| Caminho | Conferência |
|---|---|
| `save()` direto e `save(update_fields=...)` | Ambos passam pelo override. O receiver filtra `update_fields`, quando fornecido. `app/chamados/models.py:2847`; `app/chamados/signals.py:219`. |
| `carimbar` | Persiste por `self.save`; restaura carimbo e lavra em memória quando há exceção. `app/chamados/models.py:2609`; `app/chamados/models.py:2610`. |
| `carimbar_identidade` e `descarimbar` | Persistem pelo mesmo `save`. `app/chamados/models.py:2644`; `app/chamados/models.py:2652`. |
| `_escrever_carimbo` | Só atribui em memória; a persistência pertence às portas acima. `app/chamados/models.py:2556`. |
| `abrir` | Reutilização salva pelo override; nascimento usa `objects.create`, mas o receiver exclui deliberadamente `created=True`. `app/chamados/models.py:2474`; `app/chamados/models.py:2476`; `app/chamados/signals.py:216`. |
| Materialização | O carimbo passa por `save`; plantio e carimbo continuam dentro do `atomic` abrangente da materialização. `app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1202`. |

**Existem bypasses; não equivalem a um defeito novo comprovado.** `app/chamados/models.py:2699`; `app/chamados/veredito_lavrado.py:207`.

- **Reabertura:** usa `update`, mas captura as linhas antes e emite no mesmo `atomic`, inclusive com `desfazer_absorcao=True`. **Não procedente como quebra de atomicidade.** `app/chamados/services/disputa_emissao.py:1401`; `app/chamados/services/disputa_emissao.py:1405`; `app/chamados/services/disputa_emissao.py:1414`; `app/chamados/services/disputa_emissao.py:1417`.
- **Ressuscitação:** chama `descarimbar_em_lote`, cujo `update` não emite evento. **Bypass técnico procedente; impacto funcional dos antigos achados 2/7 não procedente nos chamadores encontrados.** `app/chamados/models.py:1504`; `app/chamados/models.py:2699`.
- **O descarte de 2/7 procede:** `reabrir` grava o estado antes da ressuscitação; o cartório exige chamado em `VIVOS`; o desarquivamento exige sucesso de `reabrir` antes da ressuscitação adicional. `app/chamados/models.py:1393`; `app/chamados/models.py:1402`; `app/ponto/services/cartorio.py:1592`; `app/ponto/services/cartorio.py:1622`; `app/chamados/views_arquivar.py:70`; `app/chamados/views_arquivar.py:86`.
- **Backfill da lavra:** `bulk_update` escreve somente `veredito_lavrado`, `julgado_em` e `janela_expira_em`, excluídos dos campos que acordam. **Não procedente como omissão de evento desta obra.** `app/chamados/veredito_lavrado.py:194`; `app/chamados/veredito_lavrado.py:207`; `app/chamados/models.py:2660`.

A garantia tem limites explícitos: pergunta sem célula resolvível não emite; `raw` e nascimento são excluídos; o `atomic` não encaminha `using`. A configuração SaaS mostra somente `default`; caminho produtivo da pergunta em outro alias: **[nao verificado]**. `app/chamados/models.py:2685`; `app/chamados/signals.py:216`; `app/chamados/models.py:2854`; `app/config/settings/saas.py:83`.

As FKs `celula` e `chamado_destino` também permitem `SET_NULL`, uma via indireta de alteração fora desse `save`. Contraexemplo funcional por deleção no escopo da obra: **[nao verificado]**. `app/chamados/models.py:2210`; `app/chamados/models.py:2821`.

**2. Efeito do savepoint nos chamadores**

**Mudança de comportamento procedente; regressão não demonstrada.** O caso concreto é S144: o fechamento da disputa abre uma transação, dispara o receiver de materialização e depois escreve a trilha. Esse receiver captura `Exception`; o novo savepoint permite reverter a pergunta/evento sem inutilizar a transação externa. Isso corresponde ao best-effort declarado ali. `app/chamados/models.py:1846`; `app/chamados/models.py:1865`; `app/chamados/models.py:1866`; `app/chamados/signals.py:125`; `app/chamados/signals.py:140`; `app/chamados/signals.py:141`; `app/chamados/services/materializacao.py:1349`; `app/chamados/models.py:2854`.

Os demais casos relevantes são:

- **Reconciliador:** captura a exceção de `carimbar` e segue; agora a pergunta volta no banco, e `carimbar` restaura também a memória. `app/chamados/reconciliador.py:583`; `app/chamados/reconciliador.py:591`; `app/chamados/models.py:2610`; `app/chamados/models.py:2854`.
- **Materialização:** a captura ocorre depois de sair do `atomic` abrangente; o savepoint novo não conserva uma batida cujo carimbo falhou. `app/chamados/services/materializacao.py:1145`; `app/chamados/services/materializacao.py:1202`; `app/chamados/services/materializacao.py:1208`; `app/chamados/services/materializacao.py:1210`.
- **S143:** já tinha savepoint próprio e captura fora dele. `app/chamados/signals.py:88`; `app/chamados/signals.py:91`; `app/chamados/signals.py:93`.
- **`select_for_update`:** o desfazimento de carimbo mantém sua transação e suas travas anteriores; o `save` fica aninhado nela. `app/chamados/services/relavra_via.py:217`; `app/chamados/services/relavra_via.py:221`; `app/chamados/services/relavra_via.py:231`.
- **Callback do beco:** em autocommit, passa a aguardar a conclusão do `atomic` do save; uma falha na emissão impede esse sucesso. `app/chamados/signals.py:202`; `app/chamados/signals.py:221`; `app/chamados/models.py:2854`.

Captura explícita de `IntegrityError` ou `get_or_create` **da pergunta**, capaz de produzir outra mudança concreta: **[nao verificado]**. O `get_or_create` do gate examinado pertence a `ChamadoColaborador`. `app/chamados/models.py:932`.

**3. Mordida do RED de atomicidade**

**Por inspeção, morde o mutante “retirar o `atomic`”; hipótese de teste vacuamente verde não procedente.** O teste salva `validada_em`, exige a exceção e consulta novamente o banco antes do encerramento do teste. A transação externa do `TestCase`, sozinha, não desfaz a escrita naquele ponto apenas porque `assertRaises` capturou um `RuntimeError`. A fixture confirma previamente que `validada_em` era nulo. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:255`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:261`.

O patch atinge o nome correto: `acordar_celulas` importa `emitir_evento_celula` **dentro da função**, do mesmo módulo alterado pelo teste; não usa um alias previamente capturado no receiver. `app/chamados/models.py:2677`; `app/chamados/models.py:2688`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`.

Não localizei nesse teste outro rollback que mascarasse a retirada do override. Execução real do RED/GREEN e do mutante com Django: **[nao verificado]**. O teste cobre uma falha síncrona simulada na emissão, pelo caminho `save(update_fields=['validada_em'])`; não constitui uma execução de todas as portas. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:258`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:260`.

O antigo achado 9 também está corrigido no teste: agora exige pelo menos uma pergunta exercitada, mantém a igualdade exata e exige ausência de eventos adicionais na repetição. Resultado executado: **[nao verificado]**. `app/ponto/tests/test_o_outbox_celula.py:118`; `app/ponto/tests/test_o_outbox_celula.py:124`; `app/ponto/tests/test_o_outbox_celula.py:130`.

**4. Cobertura do selo**

**Cobertura dos campos diretos atuais: procedente. Cobertura universal: não.** O conjunto extraído atualmente contém `celula`, `chamado_destino`, `chamado_destino_id`, `data_evento`, `disputa`, `materializada_em`, `respondida_em`, `resposta_colab`, `validada_em` e `via_resolucao`; todos estão em `CAMPOS_QUE_ACORDAM`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:198`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:221`; `app/chamados/models.py:2660`.

**Lacuna residual do selo — procedente, severidade baixa; sem campo mutável atual descoberto faltando no conjunto.**

- `_ancoradas` não é inspecionada. Seus filtros por string e `Q` escapam como sítios; os campos atuais da pergunta nesses filtros já estão representados nas funções varridas. `app/chamados/models.py:327`; `app/chamados/models.py:328`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:221`.
- O scanner só reconhece atributos e `getattr` literal sobre `p`/`_p`. Apelidos diferentes, `values_list`, `F`, `Q` e strings de filtro escapam. A execução em memória do scanner real confirmou esses escapes com leituras mutantes de `recusa_motivo`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:196`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:212`.
- Retirar `celula_id` e `disputa_id` do conjunto não seria detectado: o resultado extraído contém `celula`/`disputa`, e o selo não exige os pares nome/attname. Hoje os pares estão presentes. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:204`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:227`; `app/chamados/models.py:2662`.
- Helpers continuam fora, expressamente declarado. Eles leem `motivo` e `tipo_resposta`, ausentes do conjunto; o nascimento os escreve, mas alteração produtiva posterior não foi demonstrada. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:193`; `app/chamados/juizes.py:886`; `app/chamados/juizes.py:1099`; `app/chamados/juizes.py:1103`; `app/chamados/models.py:2476`; `app/chamados/models.py:2477`.

As costuras de `validada_em` e da âncora agora existem; portanto “selo inteiramente sem mordida” não procede. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:229`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:235`.

**5. Quem escreve e quem lê**

O inventário abaixo identifica os escritores encontrados e os leitores **nesta cadeia**; não declara um inventário de todas as telas e relatórios. Os pontos de transporte são o receiver e a reabertura. `app/chamados/signals.py:221`; `app/chamados/services/disputa_emissao.py:1405`; `app/chamados/services/disputa_emissao.py:1417`.

| Elemento | Escritor/definição e leitores/chamadores |
|---|---|
| `CAMPOS_QUE_ACORDAM` da pergunta | Definido em `app/chamados/models.py:2660`; lido produtivamente pelo filtro em `app/chamados/signals.py:219`. |
| Conjunto homônimo da célula | Contém `veredito/ata`; lido por `salvar_e_emitir`. São conjuntos distintos. `app/ponto/portas/celula.py:663`; `app/ponto/portas/celula.py:682`. |
| `linhas_do_evento` | Lê `celula_id`, `disputa__colaborador_id`, `data_evento`; chamado pelo receiver e pela reabertura. `app/chamados/models.py:2668`; `app/chamados/signals.py:221`; `app/chamados/services/disputa_emissao.py:1405`. |
| `acordar_celulas` | Resolve célula por FK/fallback, deduplica localmente e chama a porta de emissão; seus chamadores são receiver e reabertura. `app/chamados/models.py:2682`; `app/chamados/models.py:2684`; `app/chamados/models.py:2688`; `app/chamados/signals.py:221`; `app/chamados/services/disputa_emissao.py:1417`. |
| `PerguntaDisputa.save` | Persiste por `super().save` dentro do novo `atomic`; portas e bypasses discriminados na resposta 1. `app/chamados/models.py:2854`; `app/chamados/models.py:2855`. |

| Campo da pergunta | Quem escreve | Quem lê nesta cadeia |
|---|---|---|
| `resposta_colab` | Resposta, automática e reabertura. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1433`; `app/chamados/services/disputa_emissao.py:1393`. | C6 e comparação com luz. `app/chamados/juizes.py:1150`; `app/chamados/juizes.py:1113`. |
| `respondida_em` | Resposta, automática, trilha, carimbo, reabertura e ressuscitação. `app/chamados/services/disputa_emissao.py:1283`; `app/chamados/services/disputa_emissao.py:1434`; `app/chamados/services/disputa_emissao.py:1450`; `app/chamados/models.py:2589`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1504`. | C6. `app/chamados/juizes.py:1150`. |
| `validada_em` | Validação, devolução, resposta nova, carimbo, reabertura e ressuscitação. `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:188`; `app/chamados/services/disputa_emissao.py:1295`; `app/chamados/models.py:2592`; `app/chamados/services/disputa_emissao.py:1393`; `app/chamados/models.py:1504`. | C6 e coletor. `app/chamados/juizes.py:1136`; `app/chamados/models.py:327`. |
| `materializada_em` | Porta do carimbo e descarimbo coletivo. `app/chamados/models.py:2558`; `app/chamados/models.py:2699`. | C6. `app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1177`; `app/chamados/juizes.py:1207`. |
| `via_resolucao` | Carimbo, lote, fato no espelho e relavra de via. `app/chamados/models.py:2560`; `app/chamados/models.py:2700`; `app/chamados/models.py:2709`; `app/chamados/services/relavra_via.py:123`. | C6. `app/chamados/juizes.py:1145`; `app/chamados/juizes.py:1171`. |
| `data_evento` | Nascimento e preenchimento de data ausente no reaponte. `app/chamados/models.py:2476`; `app/chamados/services/fio.py:834`. | C6, tranca, gate e captura. `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:931`; `app/chamados/models.py:347`; `app/chamados/models.py:2668`. |
| `celula`/`celula_id` | Nascimento, reuso de `abrir`, além da política `SET_NULL`. `app/chamados/models.py:2477`; `app/chamados/models.py:2436`; `app/chamados/models.py:2210`. | C6 e captura/resolução do evento. `app/chamados/juizes.py:1147`; `app/chamados/models.py:2668`; `app/chamados/models.py:2682`. |
| `disputa`/`disputa_id` | Nascimento; mudança posterior: **[nao verificado]**. `app/chamados/models.py:2476`. | C6, gate e captura do colaborador. `app/chamados/juizes.py:1200`; `app/chamados/models.py:345`; `app/chamados/models.py:2668`. |
| `chamado_destino`/`_id` | Nascimento, reuso, reaponte e política `SET_NULL`. `app/chamados/models.py:2477`; `app/chamados/models.py:2450`; `app/chamados/services/fio.py:845`; `app/chamados/models.py:2821`. | Coletor, gate e helper de materialização. `app/chamados/models.py:328`; `app/chamados/models.py:344`; `app/chamados/juizes.py:743`. |

A captura lê ainda o colaborador da disputa; a emissão lê `pk/colaborador_id/data` da célula encontrada. O nascimento da disputa atribui seu colaborador. `app/chamados/models.py:2668`; `app/ponto/portas/celula.py:675`; `app/chamados/services/disputa_emissao.py:1182`.

| Campo de `EventoCelula` | Quem escreve | Quem lê |
|---|---|---|
| `pk` | Identidade da linha inserida pela porta. `app/ponto/portas/celula.py:675`. | Exclusão dos lidos e atualização do lote. `app/chamados/reconciliador.py:1335`; `app/chamados/reconciliador.py:1378`. |
| `celula_id` | Porta de emissão. `app/ponto/portas/celula.py:675`. | Vigia e semeadura. `app/chamados/reconciliador.py:1262`; `app/chamados/reconciliador.py:1284`. |
| `colaborador_id` | Mesma porta. `app/ponto/portas/celula.py:675`. | Agrupamento, carregamento e vigia de lastro. `app/chamados/reconciliador.py:1339`; `app/chamados/reconciliador.py:1345`; `app/ponto/services/lastro.py:421`. |
| `data` | Mesma porta. `app/ponto/portas/celula.py:675`. | Não delimita o consumo, agrupado por colaborador; outro leitor produtivo: **[nao verificado]**. `app/chamados/reconciliador.py:1339`. |
| `criado_em` | `auto_now_add`. `app/escala/models.py:1860`. | Ordenação e idade. `app/chamados/reconciliador.py:1332`; `app/chamados/reconciliador.py:1250`. |
| `processado_em` | Inicialmente nulo; consumidor marca sucesso. `app/escala/models.py:1861`; `app/chamados/reconciliador.py:1386`. | Pendentes, mortos e retenção. `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`; `app/chamados/reconciliador.py:1393`. |
| `tentativas` | Default zero; consumidor incrementa. `app/escala/models.py:1862`; `app/chamados/reconciliador.py:1380`. | Seleção de pendentes/mortos e incremento. `app/chamados/reconciliador.py:1238`; `app/chamados/reconciliador.py:1244`; `app/chamados/reconciliador.py:1380`. |
| `erro` | Default vazio; consumidor grava falha. `app/escala/models.py:1863`; `app/chamados/reconciliador.py:1380`. | Leitor produtivo adicional: **[nao verificado]**. |

**Segundo escritor de INSERT fora da porta: não procedente.** O novo helper chama `emitir_evento_celula`; a criação continua naquela porta. O consumidor já escreve os metadados de processamento, sem introduzir outro produtor. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`; `app/chamados/reconciliador.py:1380`; `app/chamados/reconciliador.py:1386`.

**6. Classes de reprovação**

| Classe | Veredito e severidade |
|---|---|
| Segundo juiz duplicando autoridade | **Não procedente no acréscimo; sem severidade.** Signal transporta, consumidor chama as autoridades existentes e C6 permanece registrado. `app/chamados/signals.py:221`; `app/chamados/reconciliador.py:1372`; `app/core/juizes.py:266`. |
| Segundo escritor fora da porta | **Não procedente; sem severidade.** `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`. |
| Relógio fora da borda | **Procedente na cadeia herdada; média, não introduzida nesta obra.** C6 chama helper que lê `date.today()`. `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:562`. |
| Derivação paralela de fonte única | **Não procedente como novo julgamento; sem severidade.** Há repetição logística de seleção FK/fallback, sem cálculo próprio de vitalidade. `app/chamados/models.py:2681`; `app/chamados/reconciliador.py:1295`. |
| Selo/RED sem mordida | **RED de atomicidade: hipótese não procedente por inspeção. Selo: lacuna parcial procedente, baixa**, discriminada na resposta 4. Execução Django: **[nao verificado]**. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:249`. |
| Inversão de trava | **Não demonstrada; severidade não atribuída.** Helper acrescenta SELECT simples e INSERT em evento sem FK; a marcação do consumidor ocorre depois, em transação curta. `app/chamados/models.py:2682`; `app/chamados/models.py:2688`; `app/escala/models.py:1856`; `app/chamados/reconciliador.py:1376`. |
| Pouso antes do pré-requisito | APIs necessárias presentes na cópia; ordem efetiva de commits/deploy: **[nao verificado]**. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304`. |

A lavra não produz a realimentação apontada na triagem: seus três campos ficam fora do conjunto. Carimbos feitos pelo próprio consumidor podem gerar outra passagem, com convergência nos filtros encontrados; a deduplicação por colaborador vale por lote, e o laço tem teto de tempo. **Laço infinito não demonstrado.** `app/chamados/veredito_lavrado.py:231`; `app/chamados/models.py:2660`; `app/chamados/reconciliador.py:619`; `app/chamados/reconciliador.py:1339`; `app/chamados/reconciliador.py:1391`.

## 06/11 08:38 · sessao b5cf1af1f565 · - · - · origem=Ronald (tmux)

Q4 — **cobertura atual suficiente para os campos diretos; cobertura universal não procede.** O selo extrai atualmente `celula`, `chamado_destino`, `chamado_destino_id`, `data_evento`, `disputa`, `materializada_em`, `respondida_em`, `resposta_colab`, `validada_em`, `via_resolucao`; todos constam no conjunto. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:198`; `app/chamados/models.py:2660`.

- **Lacuna do selo — média, procedente quanto à promessa universal.** `_ancoradas` não integra as funções inspecionadas; seus filtros por strings e `Q()` escapam. Hoje seus campos de Pergunta relevantes já aparecem nas funções varridas, portanto não há omissão funcional atual demonstrada nessa coleta. `app/chamados/models.py:327`; `app/chamados/models.py:328`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:221`.
- **Apelidos e ORM — lacuna demonstrada em memória.** Executei `_lidos` real com `_meta` simulado a partir dos campos do modelo e funções reais compiladas preservando arquivo/linha. Mutantes acrescentando leitura de `recusa_motivo` por `alias = p; alias.recusa_motivo`, `values_list('recusa_motivo')`, `Q(recusa_motivo='x')` e `F('recusa_motivo')` produziram o mesmo conjunto; as asserções atuais continuariam passando. O scanner reconhece apenas atributos e `getattr` literais sobre `p`/`_p`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:196`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:212`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:227`.
- **Helpers — exclusão expressamente declarada.** `motivo` e `tipo_resposta` continuam lidos transitivamente e ausentes de `CAMPOS_QUE_ACORDAM`; não localizei escrita produtiva posterior ao nascimento na busca dirigida. Alterações dinâmicas: [nao verificado]. `app/chamados/juizes.py:886`; `app/chamados/juizes.py:1099`; `app/chamados/juizes.py:1103`; `app/chamados/juizes.py:672`; `app/chamados/models.py:2476`; `app/chamados/models.py:2660`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:193`.
- **Pares FK — lacuna adicional baixa.** Retirar `celula_id` e `disputa_id` do conjunto não seria detectado: o conjunto extraído contém somente `celula` e `disputa`. O scanner aceita nomes/attnames, mas não exige sua equivalência. Ele ainda conta atribuição como leitura, incluindo `_p.chamado_destino = self`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:204`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:209`; `app/chamados/models.py:350`.

Q6:

| Classe | Resultado |
|---|---|
| Segundo juiz | **Não procedente no acréscimo.** Signal transporta; consumidor usa as autoridades existentes; C6 permanece registrado. `app/chamados/signals.py:215`; `app/chamados/reconciliador.py:1354`; `app/chamados/reconciliador.py:1372`; `app/core/juizes.py:266`. |
| Segundo escritor do evento | **Não procedente.** Helper chama a porta; INSERT continua nela. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`. |
| Relógio fora da borda | **Procedente na cadeia herdada**, sem nova leitura nos helpers adicionados: C6 chama função que lê `date.today()`. `app/chamados/juizes.py:1197`; `app/chamados/juizes.py:562`; `app/chamados/models.py:2665`; `app/chamados/models.py:2671`. |
| Derivação paralela de julgamento | **Não procedente no acréscimo.** A repetição é logística, escolhendo célula por FK/fallback, sem julgar vitalidade. `app/chamados/models.py:2681`; `app/chamados/reconciliador.py:1295`. |
| Selo sem mordida universal | **Procedente, média**, com limites acima; a correção específica de `getattr` e da âncora está presente. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:212`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:235`. |
| Inversão de trava nova | **Não demonstrada.** Helper acrescenta SELECT simples e INSERT em evento sem FK; consumidor marca depois da reconciliação em atomic curto. `app/chamados/models.py:2682`; `app/chamados/models.py:2688`; `app/escala/models.py:1856`; `app/chamados/reconciliador.py:1376`. |
| Pouso antes do pré-requisito | APIs necessárias estão na cópia; ordem efetiva de deploy/commits: **[nao verificado]**. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304`. |

Sonda apenas em memória, sem Django/banco e sem alterações de arquivos; resultado da suíte Django/RED histórico: **[nao verificado]**. [nao verificado]

## 06/11 08:37 · sessao fe5ab7bf9462 · - · - · origem=Ronald (tmux)

**Inventário e bypasses — conferência estática, somente leitura.**

| Elemento | Escritor / chamadores | Leitor |
|---|---|---|
| `PerguntaDisputa.CAMPOS_QUE_ACORDAM` | Declaração única em `app/chamados/models.py:2660` | Filtro produtivo do receiver em `app/chamados/signals.py:219`; testes em `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:225` |
| `linhas_do_evento` | Definição em `app/chamados/models.py:2665` | Receiver em `app/chamados/signals.py:221`; reabertura em `app/chamados/services/disputa_emissao.py:1405` |
| `acordar_celulas` | Definição em `app/chamados/models.py:2671` | Receiver em `app/chamados/signals.py:221`; reabertura em `app/chamados/services/disputa_emissao.py:1417` |
| `PerguntaDisputa.save` | Override envolve `super().save` em `transaction.atomic()` em `app/chamados/models.py:2853` | Carimbo comum em `app/chamados/models.py:2609`; identidade em `:2644`; descarimbo em `:2652`; reuso de `abrir` em `:2474`; respostas em `app/chamados/services/disputa_emissao.py:1301`, `:1435`, `:1451`; validação em `app/chamados/services/validacao.py:133`, `:190`; reaponte em `app/chamados/services/fio.py:847`; relavra de via em `app/chamados/services/relavra_via.py:125` |

| Campo da pergunta | Quem grava | Quem lê na cadeia |
|---|---|---|
| `resposta_colab` | Resposta em `app/chamados/services/disputa_emissao.py:1282`, automática em `:1433`; reabertura em `:1393`, persistida em `:1416` | C6 em `app/chamados/juizes.py:1150`; comparação com luz em `:1113` |
| `respondida_em` | Resposta em `app/chamados/services/disputa_emissao.py:1283`, automática em `:1434`, trilha em `:1450`; carimbo em `app/chamados/models.py:2589`; reabertura em `app/chamados/services/disputa_emissao.py:1393`; ressuscitação em `app/chamados/models.py:1504` | C6 em `app/chamados/juizes.py:1150` |
| `validada_em` | Validação em `app/chamados/services/validacao.py:131`, devolução em `:188`; resposta nova em `app/chamados/services/disputa_emissao.py:1295`; carimbo em `app/chamados/models.py:2592`; reabertura em `app/chamados/services/disputa_emissao.py:1393`; ressuscitação em `app/chamados/models.py:1504` | C6 em `app/chamados/juizes.py:1136`; coletor em `app/chamados/models.py:327` |
| `materializada_em` | Atribuição canônica em `app/chamados/models.py:2558`; persistência em `:2609`, `:2644`, `:2652`; lote em `:2699` | C6 em `app/chamados/juizes.py:1145`, `:1171`, `:1177`, `:1207` |
| `via_resolucao` | Carimbo em `app/chamados/models.py:2560`; lote em `:2700`; fato no espelho em `:2709`; relavra em `app/chamados/services/relavra_via.py:123` | C6 em `app/chamados/juizes.py:1145`, `:1171` |
| `data_evento` | Nascimento em `app/chamados/models.py:2476`; preenchimento ausente em `app/chamados/services/fio.py:834`, persistido em `:847` | C6 em `app/chamados/juizes.py:1197`; tranca em `:931`; gate em `app/chamados/models.py:347`; captura em `:2668` |
| `celula` / `_id` | Nascimento em `app/chamados/models.py:2477`; reuso em `:2436`, persistido em `:2474`; FK `SET_NULL` em `:2209` | C6 em `app/chamados/juizes.py:1147`; captura em `app/chamados/models.py:2668`; resolução em `:2682` |
| `disputa` / `_id` | Nascimento em `app/chamados/models.py:2476`; alteração posterior não localizada [nao verificado] | C6 em `app/chamados/juizes.py:1200`; gate em `app/chamados/models.py:345`; captura em `:2668` |
| `chamado_destino` / `_id` | Nascimento em `app/chamados/models.py:2477`; reuso em `:2450`, persistido em `:2474`; reaponte em `app/chamados/services/fio.py:845`, persistido em `:847`; FK `SET_NULL` em `app/chamados/models.py:2821` | Coletor em `app/chamados/models.py:328`; gate em `:344`; helper de materialização em `app/chamados/juizes.py:743` |

A captura lê `disputa__colaborador_id`; a emissão lê `pk`, `colaborador_id` e `data` da célula encontrada. `app/chamados/models.py:2668`, `:2682`, `:2684`; `app/ponto/portas/celula.py:675`.

| Campo do evento | Escritor | Leitor |
|---|---|---|
| `celula_id` | INSERT da porta em `app/ponto/portas/celula.py:675` | Vigia em `app/chamados/reconciliador.py:1262`; semeadura em `:1284`, `:1297` |
| `colaborador_id` | Mesma porta em `app/ponto/portas/celula.py:675` | Agrupamento em `app/chamados/reconciliador.py:1339`; carregamento em `:1345`; vigia de lastro em `app/ponto/services/lastro.py:421` |
| `data` | Mesma porta em `app/ponto/portas/celula.py:675` | Consumidor não usa para limitar o recorte, agrupando pelo colaborador em `app/chamados/reconciliador.py:1337`; outro leitor produtivo [nao verificado] |
| `criado_em` | `auto_now_add` em `app/escala/models.py:1860` | Ordenação em `app/chamados/reconciliador.py:1332`; idade em `:1250` |
| `processado_em` | Inicialmente nulo em `app/escala/models.py:1861`; consumidor em `app/chamados/reconciliador.py:1386` | Pendentes em `:1238`; mortos em `:1244`; retenção em `:1393` |
| `tentativas` | Default zero em `app/escala/models.py:1862`; incremento em `app/chamados/reconciliador.py:1380` | Filtros em `:1238`, `:1244`; incremento em `:1380` |
| `erro` | Default vazio em `app/escala/models.py:1863`; consumidor em `app/chamados/reconciliador.py:1380` | Outro leitor produtivo [nao verificado] |

**Segundo escritor fora da porta: não procedente no acréscimo, sem severidade.** O helper novo chama `emitir_evento_celula`; o INSERT permanece na porta. As atualizações de processamento continuam no consumidor, sem novo INSERT lateral. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`; `app/chamados/reconciliador.py:1380`, `:1386`.

**Bypass da ressuscitação: existe tecnicamente; achado funcional de ausência de reabertura não procedente nos chamadores encontrados, sem severidade demonstrada.** `ressuscitar_perguntas` chama o lote sem evento próprio. Entretanto, `reabrir` grava o estado antes de ressuscitar; o cartório exige `VIVOS`; desarquivar reabre antes da segunda ressuscitação. `app/chamados/models.py:1504`, `:2699`, `:1391`, `:1393`, `:1402`; `app/ponto/services/cartorio.py:1591`, `:1592`, `:1622`; `app/chamados/views_arquivar.py:70`, `:86`.

**Outros bypasses explícitos encontrados:** a reabertura usa `update`, com captura e evento no mesmo `atomic`; o backfill usa `bulk_update` somente dos campos de lavra, excluídos do conjunto que acorda. `app/chamados/services/disputa_emissao.py:1401`, `:1405`, `:1414`, `:1416`, `:1417`; `app/chamados/veredito_lavrado.py:194`, `:207`; `app/chamados/models.py:2660`.

**Limite adicional:** as FKs `celula` e `chamado_destino` declaram `SET_NULL`, uma possibilidade de atualização indireta pelo coletor de deleção. Contraexemplo funcional dessa via no escopo do diff: [nao verificado]. `app/chamados/models.py:2209`, `:2821`.

**Multi-DB:** o novo `atomic` não recebe `using`; o `save` aceita `**kwargs`. Caminho produtivo usando outro alias não localizado [nao verificado]; a configuração SaaS declara `default` e `TenantSyncRouter`. Não há base para reportar defeito alcançável por esse ponto. `app/chamados/models.py:2847`, `:2854`; `app/config/settings/saas.py:83`, `:93`.

## 06/11 08:37 · sessao fb07214db4c7 · - · - · origem=Ronald (tmux)

Q2 — **mudança de comportamento em falhas confirmada; regressão não demonstrada**:

- **S144 é o caso concreto mais relevante.** `DisputaSupervisao.fechar()` abre `atomic`, salva a disputa e escreve sua trilha; esse save dispara o receiver que chama `materializar_pendentes_aprovadas()` e captura `Exception` sem savepoint próprio. Essa função carimba perguntas. O novo `PerguntaDisputa.save()` passa a isolar a falha da pergunta/evento, permitindo que o fechamento externo prossiga; antes, uma exceção de banco podia deixar a transação externa marcada para rollback. É compatível com o best-effort declarado pelo receiver, e não prova regressão. `app/chamados/models.py:1846`, `app/chamados/models.py:1865`, `app/chamados/models.py:1866`; `app/chamados/signals.py:125`, `app/chamados/signals.py:132`, `app/chamados/signals.py:140`, `app/chamados/signals.py:141`; `app/chamados/services/materializacao.py:1343`, `app/chamados/services/materializacao.py:1349`; `app/chamados/models.py:2854`.

- **Reconciliador engole a exceção e continua**, mas agora a pergunta não fica gravada sem evento: chama `carimbar()` dentro do try e captura `Exception`; `carimbar()` restaura também carimbo e lavra em memória e relança. Sem transação externa, o novo `atomic` reverte pergunta/evento; se houver transação externa, fornece o savepoint local. A existência dessa transação externa em cada execução efetiva é [nao verificado]. `app/chamados/reconciliador.py:583`, `app/chamados/reconciliador.py:586`, `app/chamados/reconciliador.py:591`; `app/chamados/models.py:2605`, `app/chamados/models.py:2609`, `app/chamados/models.py:2610`, `app/chamados/models.py:2854`.

- **Materialização preserva o rollback abrangente.** `_materializar_uma()` já envolve plantio e carimbo num `atomic`, e captura a exceção depois de sair dele; portanto o savepoint da pergunta não permite conservar a batida quando seu carimbo/evento falha. O `finally` continua registrando a recusa. `app/chamados/services/materializacao.py:1144`, `app/chamados/services/materializacao.py:1145`, `app/chamados/services/materializacao.py:1202`, `app/chamados/services/materializacao.py:1208`, `app/chamados/services/materializacao.py:1210`, `app/chamados/services/materializacao.py:1221`.

- **S143 já isolava as falhas.** O receiver que resolve perguntas ao fechar chamado já tinha seu próprio `atomic`, com captura fora dele; não há redução nova do escopo externo de rollback nesse caminho. `app/chamados/signals.py:83`, `app/chamados/signals.py:88`, `app/chamados/signals.py:91`, `app/chamados/signals.py:93`.

- **`select_for_update` permanece nos mesmos chamadores.** O desfazimento de carimbo abre atomic, trava pergunta/disputa, verifica C6 e chama `descarimbar()`; não captura exceção localmente. O savepoint novo fica dentro dessa transação e não substitui a trava já adquirida. `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`.

- **Callbacks mudam de momento no save em autocommit:** `_becos_post_save_pergunta` registra `on_commit`; com o novo atomic, a callback só fica liberada após a sequência do save/receiver de outbox concluir. A emissão síncrona que falha impede o sucesso desse atomic. `app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/signals.py:215`, `app/chamados/signals.py:221`; `app/chamados/models.py:2854`.

- **Captura explícita de `IntegrityError` do save de PerguntaDisputa ou `get_or_create` dessa entidade:** não localizada; ausência universal [nao verificado]. O `get_or_create` do gate mostrado é de `ChamadoColaborador`, sem relação com o override novo de save da pergunta. `app/chamados/models.py:932`.

- **Multi-DB:** o override usa `transaction.atomic()` sem `using`, e encaminha os argumentos a `super().save`; portanto a garantia não é genericamente estendida a outro alias. Contudo, a configuração saas apresentada só declara `default`, e nenhum chamador produtivo de PerguntaDisputa com outro alias foi localizado; defeito atual por multi-DB [nao verificado]. `app/chamados/models.py:2847`, `app/chamados/models.py:2854`, `app/chamados/models.py:2855`; `app/config/settings/saas.py:83`, `app/config/settings/saas.py:93`.

## 06/11 08:32 · sessao 44cd8afa75f9 · FIO-MORTO-POR-DONO-0611 · sonda · origem=Code

**PROD**

A conferência integral dos scripts ficou **[nao verificado]**: `conta_prod.py`, `sonda_dono.py` e `logs/a683/sonda_sem_fio.py` não foram encontrados na cópia. Os números abaixo são os dados raspados fornecidos, com as somas recalculadas; equivalência das sondas e hashes contra seus conteúdos: **[nao verificado]**.

O código permite conferir o contrato: universo deduplicado por colaborador/data nas competências consideradas; `limite=0` preserva todos os dias `sem_porta`; hoje/futuro é separado antes desse contador. Portanto, o balde `de_hoje_pra_frente=10` informado em PROD não integra seus 319 `sem_porta`. ([vigia_fio.py:581](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:581), [vigia_fio.py:625](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:625), [vigia_fio.py:648](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:648); valores PROD: [nao verificado])

| Medida de 06/11 | Universo | Sem porta | Fonte |
|---|---:|---:|---|
| PROD, contador vivo às 08:27:18 | 2.144 | **319** | Dado fornecido [nao verificado] |
| SOMBRA, dump das 05:02, consultado às 08:28:22 | 2.127 | **322** | Dado fornecido [nao verificado] |
| PROD − SOMBRA | +17 | −3 | Cálculo sobre o dado [nao verificado] |

O dado compara o estado do dump das 05:02 com uma leitura posterior de PROD. Mostra universos diferentes e três dias sem porta a menos em PROD; quais mudanças produziram essa diferença e qualquer atribuição a pouso: **[nao verificado]**. [nao verificado]

**SOMBRA**

O método `InsumosDoDono.dono` chama o juiz que retorna ESTRUTURA, CADASTRO ou BATIDA; a classe do censo não substitui essa medição. A implementação da convenção “não medido → INDETERMINADO” na sonda ausente: **[nao verificado]**. ([e6_oraculo.py:172](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:172), [e6_oraculo.py:98](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/ponto/management/commands/e6_oraculo.py:98))

Há uma inconsistência no pedido: **247/159/12 não corresponde ao `TAB` nem ao `TRANS` de 05/11 fornecidos**, que reconstituem **244/162/12**. A tabela abaixo usa essa decomposição, também registrada na cópia; reconciliar os três dias entre essas duas distribuições: **[nao verificado]**. ([RELATO.md:38](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:38), [RELATO.md:40](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/docs/RELATO.md:40))

As classes `e-terminal` e `e-sem-viva` correspondem, respectivamente, aos detalhes `pergunta_viva_sob_chamado_terminal` e `sem_pergunta_viva` do censo. ([vigia_fio.py:685](/home/ronald/codex_ws/185c878317e21679038d2274c24f4d017998ee4b/app/chamados/services/vigia_fio.py:685))

“Novos” significa **fora dos 418 sem porta de 05/11**; não demonstra entrada nova no universo. O motivo das quatro saídas do universo, a identidade das diferenças entre PROD e SOMBRA e a causa operacional das mudanças de rota ficam **[nao verificado]**.

| SOMBRA | Universo | Sem porta | ESTRUTURA | CADASTRO | BATIDA | INDETERMINADO | Fonte |
|---|---:|---:|---:|---:|---:|---:|---|
| 05/11, `TAB`/`TRANS` | 1.987 | 418 | 244 | 162 | 12 | 0 | Anexo [nao verificado] |
| 06/11 | 2.127 | 322 | 278 | 31 | 13 | 0 | Anexo [nao verificado] |

| HOJE, por dono | Passado | Hoje pra frente | Total | Fonte |
|---|---:|---:|---:|---|
| ESTRUTURA | 278 | 0 | **278** | Anexo [nao verificado] |
| CADASTRO | 31 | 0 | **31** | Anexo [nao verificado] |
| BATIDA | 13 | 0 | **13** | Anexo [nao verificado] |
| INDETERMINADO | 0 | 0 | **0** | Anexo [nao verificado] |
| **Total** | **322** | **0** | **322** | Anexo [nao verificado] |

| Classe — valores **05/11 → 06/11** | ESTRUTURA | CADASTRO | BATIDA | INDETERMINADO | Total | Fonte |
|---|---:|---:|---:|---:|---:|---|
| a | 0 → 0 | 0 → 0 | 2 → 2 | 0 → 0 | **2 → 2** | Anexo [nao verificado] |
| b | 0 → 0 | 1 → 0 | 0 → 0 | 0 → 0 | **1 → 0** | Anexo [nao verificado] |
| c | 8 → 8 | 20 → 5 | 2 → 3 | 0 → 0 | **30 → 16** | Anexo [nao verificado] |
| d | 185 → 200 | 9 → 14 | 3 → 3 | 0 → 0 | **197 → 217** | Anexo [nao verificado] |
| e-terminal | 33 → 31 | 132 → 12 | 5 → 5 | 0 → 0 | **170 → 48** | Anexo [nao verificado] |
| e-sem-viva | 18 → 39 | 0 → 0 | 0 → 0 | 0 → 0 | **18 → 39** | Anexo [nao verificado] |
| **Total** | **244 → 278** | **162 → 31** | **12 → 13** | **0 → 0** | **418 → 322** | Anexo [nao verificado] |

| Destino dos 418 de 05/11 | Dias | Detalhamento | Fonte |
|---|---:|---|---|
| Saíram do universo porque a competência saiu | **0** | Nenhuma ocorrência informada | `TRANS` [nao verificado] |
| Saíram do universo com competência ainda presente | **4** | c/CADASTRO: 1; d/BATIDA: 1; e-terminal/ESTRUTURA: 2 | `TRANS` [nao verificado] |
| Seguem no universo → `fio_do_admin` | **135** | b/CADASTRO: 1; c/CADASTRO: 14; e-terminal/CADASTRO: 120 | `TRANS` [nao verificado] |
| Seguem no universo → `porta_pergunta` | **1** | e-terminal/CADASTRO: 1 | `TRANS` [nao verificado] |
| Seguem sem porta, mesma classe e dono | **278** | Decomposição abaixo | `TRANS` [nao verificado] |
| Seguem sem porta, classe ou dono mudou | **0** | Nenhuma ocorrência informada | `TRANS` [nao verificado] |
| **TRANS_SOMA** | **418** | **0 + 4 + 135 + 1 + 278 + 0 = 418** | Cálculo sobre o anexo [nao verificado] |

| Classe/dono | Seguem sem porta, mesma classe/dono | Novos sem porta | HOJE | Fonte |
|---|---:|---:|---:|---|
| a / BATIDA | 2 | 0 | 2 | Anexo [nao verificado] |
| b / CADASTRO | 0 | 0 | 0 | Anexo [nao verificado] |
| c / ESTRUTURA | 8 | 0 | 8 | Anexo [nao verificado] |
| c / CADASTRO | 5 | 0 | 5 | Anexo [nao verificado] |
| c / BATIDA | 2 | 1 | 3 | Anexo [nao verificado] |
| d / ESTRUTURA | 185 | 15 | 200 | Anexo [nao verificado] |
| d / CADASTRO | 9 | 5 | 14 | Anexo [nao verificado] |
| d / BATIDA | 2 | 1 | 3 | Anexo [nao verificado] |
| e-terminal / ESTRUTURA | 31 | 0 | 31 | Anexo [nao verificado] |
| e-terminal / CADASTRO | 11 | 1 | 12 | Anexo [nao verificado] |
| e-terminal / BATIDA | 5 | 0 | 5 | Anexo [nao verificado] |
| e-sem-viva / ESTRUTURA | 18 | 21 | 39 | Anexo [nao verificado] |
| **Total** | **278** | **44** | **322** | Anexo [nao verificado] |

| Conferência | Soma recalculada | Resultado informado | Fonte |
|---|---|---|---|
| Universo PROD | 1.038 + 330 + 10 + 288 + 14 + 2 + 143 + 319 = **2.144** | Confere | Anexo [nao verificado] |
| Universo SOMBRA 05/11 | 1.000 + 312 + 0 + 238 + 17 + 0 + 2 + 418 = **1.987** | Confere | Anexo [nao verificado] |
| Universo SOMBRA 06/11 | 1.035 + 323 + 0 + 288 + 15 + 0 + 144 + 322 = **2.127** | Confere | Anexo [nao verificado] |
| `SOMA_CONFERE dono` | 278 + 31 + 13 + 0 = **322** | True | Anexo [nao verificado] |
| `SOMA_CONFERE classe` | 2 + 0 + 16 + 217 + 48 + 39 = **322** | True | Anexo [nao verificado] |
| `SOMA_CONFERE tempo` | 322 + 0 = **322** | True | Anexo [nao verificado] |
| `TRANS_SOMA` | 4 + 136 + 278 = **418** | True | Anexo [nao verificado] |
| `HOJE = SEGUE + NOVO` | 278 + 44 = **322** | True | Anexo [nao verificado] |

`rc_prod=0; rc_sombra=0; hashes impressos: conta_prod.py=d1c151d5c135c518, sonda_dono.py=454e4be486841a45, sonda_sem_fio.py=24efa49ef6157099; somas: universos 2144/1987/2127, dono=classe=tempo=322, TRANS=418, SEGUE+NOVO=278+44=322.` — dado fornecido [nao verificado].

## 06/11 08:30 · sessao f8387567b3d9 · - · - · origem=Ronald (tmux)

Auditoria somente leitura do censo:

- Universo = dias retornados por `dias_que_seguram`, nas competências de `competencias_a_lavrar`, para empresas visíveis; chave única `(colaborador_id, data)` e contador `len(dias)`. [app/chamados/services/vigia_fio.py:566](app/chamados/services/vigia_fio.py:566), [app/chamados/services/vigia_fio.py:581](app/chamados/services/vigia_fio.py:581)
- `dias_que_seguram` seleciona colaboradores retidos por `furo_espelho` e lê seus dias de retenção. [app/folha/fora_da_folha.py:118](app/folha/fora_da_folha.py:118)
- Competências = atual e anterior enquanto esta não estiver entregue; empresas visíveis = ativas, excluído prefixo de diagnóstico. [app/ponto/services/he_pendente_lavrado.py:220](app/ponto/services/he_pendente_lavrado.py:220), [app/colaboradores/queries.py:85](app/colaboradores/queries.py:85)
- Ordem da classificação: porta alcançável → hoje/futuro → baldes legais → sem porta. Consequentemente, `sem_porta` nesta implementação só contém passado; `de_hoje_pra_frente=10` de PROD pertence a outro balde, não aos 319 sem porta. [app/chamados/services/vigia_fio.py:642](app/chamados/services/vigia_fio.py:642), [app/chamados/services/vigia_fio.py:648](app/chamados/services/vigia_fio.py:648), [app/chamados/services/vigia_fio.py:654](app/chamados/services/vigia_fio.py:654)
- `limite=0` não afeta contador nem omite dia sem porta; afeta somente a listagem das outras rotas. [app/chamados/services/vigia_fio.py:625](app/chamados/services/vigia_fio.py:625)
- Classes: a = véu vivo; b = resposta vazia; c = respondida com destino terminal; d = sem pergunta viva, colaborador na lista e dia fora dela; e = demais detalhes. `e-sem-viva` corresponde a `sem_pergunta_viva`; `e-terminal` a `pergunta_viva_sob_chamado_terminal`. A classe d não mede dono. [app/chamados/services/vigia_fio.py:667](app/chamados/services/vigia_fio.py:667), [app/chamados/services/vigia_fio.py:677](app/chamados/services/vigia_fio.py:677), [app/chamados/services/vigia_fio.py:685](app/chamados/services/vigia_fio.py:685)
- `fio_do_admin` é balde distinto, aplicado após as três primeiras leis quando `lista.dia_do_admin` responde verdadeiro. [app/chamados/services/vigia_fio.py:306](app/chamados/services/vigia_fio.py:306)
- Porta é chamada da célula na fila ou pergunta viva no app com âncora na fila; a rota prefere `porta_fila` se ambas existirem. [app/chamados/juizes.py:1373](app/chamados/juizes.py:1373)
- Formato anexado coincide com `linhas_alcance` da cópia. [app/chamados/services/vigia_fio.py:705](app/chamados/services/vigia_fio.py:705)

Não localizei `logs/a683/sonda_sem_fio.py`, `sonda_dono.py` ou `conta_prod.py` na cópia; conferir os scripts, sua equivalência ou recalcular seus hashes permanece **[não verificado]**. `.gitignore` ignora `logs/`, mas isso não prova por que os arquivos não estão presentes. [.gitignore:53](.gitignore:53)

O valor entre colchetes em `sonda_leitura:` é o prefixo de 16 caracteres do SHA-256 da cópia do script efetivamente executada, não hash da árvore. [bin/sonda_leitura.sh:219](bin/sonda_leitura.sh:219), [bin/sonda_leitura.sh:221](bin/sonda_leitura.sh:221)

## 06/11 08:30 · sessao 34229530d650 · - · - · origem=Ronald (tmux)

- `InsumosDoDono.dono` reúne cadastro, turno e fase e chama o juiz único; consulta CxR quando necessário: `app/ponto/management/commands/e6_oraculo.py:172-183`.
- A precedência executável é: folga indefinida/sem vínculo → CADASTRO; batida fora do turno ou contagem ímpar → BATIDA; fase deslocada → CADASTRO; CxR → CADASTRO; restante → ESTRUTURA. `app/ponto/management/commands/e6_oraculo.py:98-110`.
- O carregador de CxR usa `dias_cadastro_x_realidade` do resultado do motor. `app/ponto/management/commands/e6_oraculo.py:154-162`.
- O juiz retorna somente esses três donos; INDETERMINADO deve indicar ausência de medição, conforme a convenção do dado fornecido. `app/ponto/management/commands/e6_oraculo.py:98-110`; implementação dessa convenção na sonda: [não verificado].
- Os scripts `sonda_sem_fio.py` e `sonda_dono.py` não foram encontrados na cópia pesquisada; equivalência e hashes das sondas: [não verificado].
- Há divergência no próprio pedido: o cabeçalho informa 05/11 = ESTRUTURA 247 / CADASTRO 159 / BATIDA 12, enquanto `TAB` e `TRANS` fornecidos somam **244 / 162 / 12**, total 418. Valores documentados também em `app/docs/RELATO.md:40-41`; prevalência do cabeçalho sobre os anexos: [não verificado].

## 06/11 08:27 · sessao 9de3fb453a9a · LASTRO-PARTE-3 · verificador · origem=Code

1. **PROCEDENTE — inventário de escritores e leitores; sem gravidade própria.** O conjunto da pergunta é declarado em `app/chamados/models.py:2660`; quem o lê em produção é o filtro do receiver, em `app/chamados/signals.py:219`. O conjunto homônimo da **célula** contém `veredito/ata` e é consumido por `salvar_e_emitir`; são conjuntos distintos. `app/ponto/portas/celula.py:663`; `app/ponto/portas/celula.py:682`.

   | Campo da PerguntaDisputa | Quem escreve | Quem lê nesta cadeia |
   |---|---|---|
   | `resposta_colab` | Resposta normal/automática e reabertura. `app/chamados/services/disputa_emissao.py:1282`, `:1433`, `:1393` | C6 decide pela resposta preenchida; helper compara com a luz. `app/chamados/juizes.py:1150`, `:1113` |
   | `respondida_em` | Resposta normal/automática/pela trilha; carimbo com resposta; reabertura e ressuscitação. `app/chamados/services/disputa_emissao.py:1283`, `:1434`, `:1450`; `app/chamados/models.py:2589`, `:1504`; `app/chamados/services/disputa_emissao.py:1393` | C6. `app/chamados/juizes.py:1150` |
   | `validada_em` | Validação, devolução, correção da resposta, carimbo com validação, reabertura e ressuscitação. `app/chamados/services/validacao.py:131`, `:188`; `app/chamados/services/disputa_emissao.py:1295`, `:1393`; `app/chamados/models.py:2592`, `:1504` | C6 e coletor do gate. `app/chamados/juizes.py:1136`; `app/chamados/models.py:327` |
   | `materializada_em` | `_escrever_carimbo`, persistido por `carimbar`, `carimbar_identidade`, `descarimbar`; lote por `descarimbar_em_lote`. `app/chamados/models.py:2558`, `:2609`, `:2644`, `:2652`, `:2699` | C6. `app/chamados/juizes.py:1145`, `:1171`, `:1177`, `:1207` |
   | `via_resolucao` | Portas do carimbo; também `encerrar_por_fato_no_espelho` e `relavrar`. `app/chamados/models.py:2560`, `:2709`; `app/chamados/services/relavra_via.py:123` | C6. `app/chamados/juizes.py:1145`, `:1171` |
   | `data_evento` | Nascimento e preenchimento de data ausente em `reapontar_resolvedora`. `app/chamados/models.py:2476`; `app/chamados/services/fio.py:832–847` | C6, tranca, gate e captura do evento. `app/chamados/juizes.py:1197`, `:931`; `app/chamados/models.py:347`, `:2668` |
   | `celula` / `celula_id` | Nascimento e reutilização em `abrir`; a FK também declara `SET_NULL`. `app/chamados/models.py:2477`, `:2435–2474`, `:2209` | C6 lê a relação; emissor captura e resolve a FK. `app/chamados/juizes.py:1147`; `app/chamados/models.py:2668`, `:2681` |
   | `disputa` / `disputa_id` | Nascimento; alteração posterior não demonstrada. `app/chamados/models.py:2476`; [nao verificado] | C6 obtém colaborador; captura obtém `disputa__colaborador_id`; gate obtém o chamado pai. `app/chamados/juizes.py:1200`; `app/chamados/models.py:2668`, `:328` |
   | `chamado_destino` / `_id` | Nascimento, reutilização em `abrir` e `reapontar_resolvedora`; FK declara `SET_NULL`. `app/chamados/models.py:2477`, `:2450–2474`, `:2821`; `app/chamados/services/fio.py:845–847` | Coletor/gate e helper de materialização efetiva. `app/chamados/models.py:328`, `:344–350`; `app/chamados/juizes.py:743` |
   | `motivo`, `tipo_resposta` | Nascimento pela porta `abrir`; alteração posterior não localizada. `app/chamados/models.py:2476–2477`; [nao verificado] | **Leitura transitiva do C6**, pelos helpers de luz, marco e materialização. `app/chamados/juizes.py:1099–1103`, `:886`, `:672–674` |

   A captura lê ainda o colaborador da disputa; a emissão usa `pk/colaborador_id/data` da célula encontrada. Há criação desses identificadores nas portas de disputa e célula. `app/chamados/models.py:2668`, `:2682–2688`; `app/chamados/services/disputa_emissao.py:1182–1186`; `app/ponto/portas/celula.py:253`.

   | Campo de EventoCelula | Quem escreve | Quem lê |
   |---|---|---|
   | `id` | Identidade da linha criada pela porta. `app/ponto/portas/celula.py:675` | Exclusão dos já lidos e atualização do lote. `app/chamados/reconciliador.py:1332–1341`, `:1378` |
   | `celula_id` | `emitir_evento_celula`. `app/ponto/portas/celula.py:675` | Vigia e deduplicação da semeadura. `app/chamados/reconciliador.py:1262`, `:1284`, `:1297` |
   | `colaborador_id` | Mesma porta. `app/ponto/portas/celula.py:675` | Agrupamento e carregamento do colaborador; também vigia de lastro. `app/chamados/reconciliador.py:1339–1345`; `app/ponto/services/lastro.py:421` |
   | `data` | Mesma porta. `app/ponto/portas/celula.py:675` | Não delimita o consumo: este varre o colaborador inteiro. Outro leitor produtivo não localizado. `app/chamados/reconciliador.py:1339–1372`; [nao verificado] |
   | `criado_em` | `auto_now_add`. `app/escala/models.py:1860` | Ordenação e idade do pendente. `app/chamados/reconciliador.py:1332`, `:1250–1251` |
   | `processado_em` | Inicialmente nulo; consumidor marca sucesso. `app/escala/models.py:1861`; `app/chamados/reconciliador.py:1386` | Seleção de pendentes, mortos e retenção. `app/chamados/reconciliador.py:1238`, `:1244`, `:1393` |
   | `tentativas` | Inicialmente zero; consumidor incrementa em falha. `app/escala/models.py:1862`; `app/chamados/reconciliador.py:1380` | Seleção de pendentes/mortos e incremento. `app/chamados/reconciliador.py:1238`, `:1244`, `:1380` |
   | `erro` | Inicialmente vazio; consumidor grava as falhas. `app/escala/models.py:1863`; `app/chamados/reconciliador.py:1380` | Leitor produtivo não localizado. [nao verificado] |

2. **PROCEDENTE — bypass existente; gravidade baixa nos chamadores encontrados.** O escritor que atende à pergunta é `ressuscitar_perguntas → descarimbar_em_lote`: altera `respondida_em`, `validada_em`, `materializada_em` e `via_resolucao` por `update`, sem `post_save`, sem `reabrir_para_correcao` e sem emissão própria. `app/chamados/models.py:1504`; `app/chamados/models.py:2699–2700`.

   O outro caminho coletivo, `reabrir_para_correcao`, está coberto pela emissão explícita. O `bulk_update` da lavra escreve somente `veredito_lavrado/julgado_em/janela_expira_em`, não os insumos diretos do C6. `app/chamados/services/disputa_emissao.py:1413–1417`; `app/chamados/veredito_lavrado.py:194–207`.

   Outros bypasses por raw SQL, escritores dinâmicos ou alteração produtiva de `motivo/tipo_resposta`: **não demonstrados**. [nao verificado]

3. **PROCEDENTE — alta: falta atomicidade entre pergunta e evento em caminhos de produção.** O receiver não abre transação; `carimbar()` também não envolve seu `save` em `atomic`. O consumidor chama `carimbar()` sem transação abrangente e engole sua exceção. Assim, **em autocommit, falha no INSERT do evento pode deixar a pergunta gravada sem evento**, embora o método tenha lançado exceção. `app/chamados/signals.py:215–221`; `app/chamados/models.py:2605–2613`; `app/chamados/reconciliador.py:582–592`, `:1354`.

   Nesse caminho, a falha não entra na lista `falhas` do consumidor, que pode marcar o evento anterior como processado. A pergunta carimbada também deixa o filtro da próxima tentativa. `app/chamados/reconciliador.py:591–592`, `:619`, `:1354`, `:1386`. Ocorrência efetiva dessa falha em produção: [nao verificado].

   **Evento sobrevivendo ao rollback: ausente nos caminhos demonstrados.** A reabertura insere no mesmo `atomic` do update; o aceite de folga pode emitir e depois pedir rollback, mas evento e validação pertencem à mesma transação. `app/chamados/services/disputa_emissao.py:1401–1417`; `app/chamados/services/validacao.py:289–313`.

   **Leitura externa antes do commit: não demonstrada.** O signal apenas insere; quem consome faz outra consulta à fila. A validação de hora protege carimbo e eventual devolução no mesmo `atomic`. `app/chamados/signals.py:221`; `app/chamados/reconciliador.py:1332`; `app/chamados/services/validacao.py:395–427`. A sincronização do signal, isoladamente, não prova publicação antecipada; o defeito comprovável é a ausência de transação comum quando o escritor está em autocommit. `app/chamados/models.py:2609`; `app/chamados/signals.py:221`.

4. **Hipótese afastada — sem gravidade: a reabertura não muda a identidade capturada.** Seu update limpa resposta e validação; `desfazer_absorcao` acrescenta apenas limpeza do carimbo. Não altera `celula`, `data_evento`, `disputa` nem colaborador. Portanto, pelo código dessa porta, a célula de antes é a mesma de depois. `app/chamados/services/disputa_emissao.py:1393`, `:1405`, `:1413–1417`; `app/chamados/models.py:2699–2700`.

   Mudança concorrente desses identificadores por outro escritor durante a captura: [nao verificado].

5. **PROCEDENTE — realimentação presente, com convergência nos caminhos examinados; sem laço infinito demonstrado.** O consumidor carimba perguntas, esses carimbos acordam o outbox e o laço pode consumir os novos IDs na mesma execução. `app/chamados/reconciliador.py:1354`, `:1364`, `:1331–1335`; `app/chamados/models.py:2585–2609`.

   Os pontos de convergência são concretos: perguntas absolvidas e órfãs deixam os universos que exigem `materializada_em IS NULL`; `encerrar_fantasma` possui ainda guarda de no-op. `app/chamados/reconciliador.py:617–620`, `:1037–1040`; `app/chamados/models.py:2718–2721`.

   `_gravar_estado` relavra perguntas, mas essa lavra salva apenas os três campos de cache, excluídos do conjunto que acorda; portanto essa escrita não gera outro evento da pergunta. `app/chamados/models.py:291–295`; `app/chamados/veredito_lavrado.py:227–231`; `app/chamados/models.py:2660–2662`; `app/chamados/signals.py:219–220`.

   Convergência universal sob alterações concorrentes: [nao verificado].

6. **Hipótese de rajada por `save()` sem `update_fields` — não demonstrada.** Os escritores identificados usam listas explícitas, inclusive resposta, validação, carimbo, reaponte e relavra; o lote de backfill usa `bulk_update` só dos campos da lavra. `app/chamados/services/disputa_emissao.py:1301`; `app/chamados/services/validacao.py:133`; `app/chamados/models.py:2609`, `:2644`, `:2474`; `app/chamados/services/fio.py:847`; `app/chamados/services/relavra_via.py:125`; `app/chamados/veredito_lavrado.py:207`.

   **Há amplificação prevista mesmo com `update_fields`:** N saves que toquem campos relevantes, todos com célula, podem produzir N eventos, inclusive para a mesma célula, porque `vistos` é local a cada chamada. Na reabertura coletiva, a deduplicação vale para todas as linhas daquela chamada. `app/chamados/signals.py:219–221`; `app/chamados/models.py:2678–2688`; `app/chamados/services/disputa_emissao.py:1417`.

   Volume real, frequência e custo em produção: [nao verificado].

7. **PROCEDENTE — omissão de `ressuscitar_perguntas`, mas necessidade de REABRIR afastada nos chamadores atuais.** Ela pode tornar uma pergunta viva: simula a remoção dos carimbos, exige C6 viva e depois faz o update correspondente. `app/chamados/models.py:1455–1458`, `:1504`.

   Porém, nos chamadores encontrados, o chamado já está vivo: `reabrir()` grava seu novo estado **antes** de ressuscitar; o cartório pula chamados fora de `VIVOS`; o desarquivamento reabre antes da ressuscitação adicional. `app/chamados/models.py:1390–1402`; `app/ponto/services/cartorio.py:1591–1593`, `:1622`; `app/chamados/views_arquivar.py:70`, `:86`.

   Portanto, não há contraexemplo demonstrado de chamado terminal precisando renascer por essa omissão. Isso sustenta a exceção nos chamadores atuais, não a frase geral “tornar viva nunca exige evento”. `app/chamados/models.py:1390–1402`; `app/ponto/services/cartorio.py:1591–1622`. Outros chamadores em produção: [nao verificado].

8. **PROCEDENTE — média: o selo promete cobertura maior que a implementada; demais classes discriminadas abaixo.**

   | Classe | Veredito e prova |
   |---|---|
   | Segundo juiz duplicando autoridade | **Ausente no acréscimo.** O signal transporta o evento; o consumidor chama as autoridades existentes. C6 continua registrado. `app/chamados/signals.py:215–221`; `app/chamados/reconciliador.py:1353–1372`; `app/core/juizes.py:266` |
   | Segundo escritor de EventoCelula fora da porta | **Ausente no acréscimo.** O helper chama `emitir_evento_celula`, que faz o INSERT. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675` |
   | Relógio fora da borda | **Presente na cadeia herdada:** `_dentro_da_janela_viva` lê `date.today()`, chamado pelo C6. Os novos helpers não acrescentam relógio. `app/chamados/juizes.py:562`, `:1197`; `app/chamados/models.py:2664–2689` |
   | Derivação paralela de julgamento | **Ausente no acréscimo.** Há repetição logística da escolha FK/fallback usada na semeadura, sem novo cálculo de vitalidade. `app/chamados/models.py:2681–2688`; `app/chamados/reconciliador.py:1295–1299` |
   | Selo sem mordida suficiente | **Presente.** O AST só percorre `_pergunta_viva_calc` e atributos diretos `p.campo`; não visita helpers, `getattr` nem o coletor do gate. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:200–209` |
   | Inversão de trava introduzida | **Não demonstrada.** A emissão acrescenta SELECT simples de célula e INSERT; o consumidor marca eventos num atomic posterior à reconciliação. `app/chamados/models.py:2682–2688`; `app/chamados/reconciliador.py:1376–1386` |
   | Pouso antes do pré-requisito | As APIs de porta/consumo estão na cópia. Ordem efetiva de commits, deploy e pré-requisitos operacionais: [nao verificado]. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304` |

   **O selo não cobre o coletor que lê `chamado_destino`.** Retirar `chamado_destino/chamado_destino_id` do conjunto não seria detectado por ele, embora o gate use esses campos; hoje ambos estão incluídos. Também ignora `getattr(p, 'celula')` e as leituras transitivas de `motivo/tipo_resposta`, estes ausentes do conjunto. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:200–203`; `app/chamados/models.py:328`, `:344–350`, `:2662`; `app/chamados/juizes.py:1147`, `:1099–1103`.

   Os testes comportamentais **mordem os três caminhos representados por construção**: esvaziam a fila, comprovam retenção inicial, executam atos reais, exigem evento e verificam estado/segunda rodada. Não exercitam falha do INSERT da pergunta em autocommit; a base é `TestCase`. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:50`, `:89–125`, `:134–185`. RED/GREEN executado: [nao verificado].

9. **Hipótese de maquiagem nas contagens — não demonstrada; os ajustes descrevem comportamento compatível com o código.**

   Em `test_o_outbox_celula`, a igualdade continua exata e a repetição continua exigindo zero eventos adicionais. Somar os saves de perguntas relevantes corresponde ao novo produtor. Porém, o teste não exige `len(perguntas) > 0`, portanto isoladamente não garante que esse terceiro produtor foi exercitado. `app/ponto/tests/test_o_outbox_celula.py:104–129`.

   Em `test_orfas...`, um carimbo pode gerar evento novo e provocar outra passagem do colaborador; o consumidor exclui os **IDs já lidos**, não o colaborador inteiro. A asserção adicional mantém todos os eventos pendentes com uma tentativa, distinguindo nova passagem de releitura do mesmo evento. `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:144–152`; `app/chamados/reconciliador.py:1332–1339`.

   Esses ajustes não corrigem nem detectam a quebra de atomicidade do achado 3. Resultado executado dos selos e contagem efetiva de duas falhas: [nao verificado].

## 06/11 08:25 · sessao 2cec561183f3 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — bypass existente, sem dano demonstrado nos chamadores atuais.** `ressuscitar_perguntas` chama `descarimbar_em_lote(..., validada_em=None, respondida_em=None)`; a porta faz `QuerySet.update` de `materializada_em`, `via_resolucao` e demais campos sem emitir evento. São insumos do C6 capazes de mudar morta→viva. (`app/chamados/models.py:1504`, `app/chamados/models.py:2699`, `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1207`)

   Contudo, o chamado da própria porta já está vivo nos três chamadores encontrados: `reabrir` grava o estado antes de ressuscitar; cartório pula chamados fora de `VIVOS`; desarquivar chama `reabrir` antes de repetir a ressuscitação. Assim, **não demonstrei necessidade de reabrir esse chamado por evento**. (`app/chamados/models.py:1390`, `app/chamados/models.py:1402`, `app/ponto/services/cartorio.py:1591`, `app/ponto/services/cartorio.py:1622`, `app/chamados/views_arquivar.py:70`, `app/chamados/views_arquivar.py:86`)

   O cartório pede novo julgamento depois de ressuscitar; o universo avalia previamente a pergunta com os campos limpos e a célula do julgamento em curso. (`app/ponto/services/cartorio.py:1641`, `app/ponto/services/cartorio.py:1647`, `app/chamados/models.py:1448`, `app/chamados/models.py:1458`)

2. **Escritores por campo do conjunto:**

   | Campo | Escritores |
   |---|---|
   | `resposta_colab` | Resposta normal, automática e reabertura. (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1416`, `app/chamados/services/disputa_emissao.py:1433`) |
   | `respondida_em` | Resposta normal/automática/pela trilha; `carimbar(resposta=...)`; reabertura; ressuscitação. (`app/chamados/services/disputa_emissao.py:1283`, `app/chamados/services/disputa_emissao.py:1434`, `app/chamados/services/disputa_emissao.py:1450`, `app/chamados/models.py:2589`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1504`) |
   | `validada_em` | Validação, devolução da validação, correção da resposta, `carimbar(resposta='validar')`, reabertura e ressuscitação. (`app/chamados/services/validacao.py:131`, `app/chamados/services/validacao.py:188`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/models.py:2592`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/models.py:1504`) |
   | `materializada_em` | Atribuição central `_escrever_carimbo`; persistência por `carimbar`, `carimbar_identidade`, `descarimbar`; atualização coletiva por `descarimbar_em_lote`. (`app/chamados/models.py:2558`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`, `app/chamados/models.py:2652`, `app/chamados/models.py:2699`) |
   | `via_resolucao` | Mesmas portas de carimbo; adicionalmente `encerrar_por_fato_no_espelho` e `relavrar`. (`app/chamados/models.py:2560`, `app/chamados/models.py:2709`, `app/chamados/models.py:2710`, `app/chamados/services/relavra_via.py:123`, `app/chamados/services/relavra_via.py:125`) |
   | `data_evento` | Nascimento e preenchimento de data ausente em `reapontar_resolvedora`. (`app/chamados/models.py:2476`, `app/chamados/services/fio.py:832`, `app/chamados/services/fio.py:834`, `app/chamados/services/fio.py:847`) |
   | `celula`/`celula_id` | Nascimento; reapontamento da pergunta existente em `abrir`. A FK declara `SET_NULL` na exclusão da célula, mas não localizei caminho produtivo que exclua `CelulaDia`: alcance desse escritor implícito [nao verificado]. (`app/chamados/models.py:2477`, `app/chamados/models.py:2435`, `app/chamados/models.py:2474`, `app/chamados/models.py:2209`) |
   | `chamado_destino`/`_id` | Nascimento, reapontamento em `abrir` e `reapontar_resolvedora`. A FK declara `SET_NULL` na exclusão do chamado; `apagar_em_ensaio` exclui todos os chamados, explicitamente limitado a comandos de ensaio. (`app/chamados/models.py:2477`, `app/chamados/models.py:2450`, `app/chamados/models.py:2474`, `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:2821`, `app/chamados/models.py:531`, `app/chamados/models.py:539`) |
   | `disputa`/`disputa_id` | Nascimento em `abrir`; escritor posterior não demonstrado. (`app/chamados/models.py:2476`; [nao verificado]) |

3. **PROCEDENTE — cobertura declarada do selo é estreita.** Além das leituras diretas, C6 chama helpers que leem `tipo_resposta`, `motivo` e `chamado_destino`; os dois primeiros não estão em `CAMPOS_QUE_ACORDAM`. Não encontrei escritor produtivo posterior ao nascimento que altere `motivo`/`tipo_resposta`; defeito ativo por esses campos permanece hipótese. (`app/chamados/juizes.py:1162`, `app/chamados/juizes.py:1214`, `app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1103`, `app/chamados/juizes.py:886`, `app/chamados/juizes.py:743`, `app/chamados/models.py:2660`; [nao verificado] quanto ao escritor ausente)

4. **Rajada por `save()` sem `update_fields`: não demonstrada.** Todos os sítios atribuídos a PerguntaDisputa pelo censo de produção usam `update_fields`; o lote de lavra usa somente `veredito_lavrado`, `julgado_em`, `janela_expira_em`, fora do conjunto que acorda. Não há evidência de cron genérico salvando perguntas integralmente. (`app/chamados/models.py:2474`, `app/chamados/models.py:2609`, `app/chamados/models.py:2644`, `app/chamados/models.py:2652`, `app/chamados/models.py:2710`, `app/chamados/services/fio.py:847`, `app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1435`, `app/chamados/services/disputa_emissao.py:1451`, `app/chamados/services/validacao.py:133`, `app/chamados/services/validacao.py:190`, `app/chamados/services/relavra_via.py:125`, `app/chamados/veredito_lavrado.py:194`, `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:231`; ausência universal [nao verificado])

5. **Reabertura e captura anterior: sem mudança de identidade na porta atual.** O dicionário atualizado limpa resposta e validação; a opção `desfazer_absorcao` acrescenta apenas limpeza do carimbo. Não altera `celula`, `data_evento` nem `disputa`/colaborador; logo capturar as linhas antes corresponde à mesma identidade depois, sem considerar concorrência externa. (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1405`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/services/disputa_emissao.py:1417`, `app/chamados/models.py:2699`)

## 06/11 08:25 · sessao 87ea1e057353 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — média — selo incompleto para o gate.** `CamposQueAcordamCobremOC6Test` analisa apenas `_pergunta_viva_calc` e apenas atributos diretos `p.campo`; não percorre `_ancoradas`, `_vivas_entre`, helpers nem `getattr`. Portanto remover `chamado_destino` e `chamado_destino_id` de `CAMPOS_QUE_ACORDAM` passaria esse selo, embora o gate leia ambos; hoje estão incluídos corretamente. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:200-209`; `app/chamados/models.py:327-350`; `app/chamados/models.py:2662`. O próprio `getattr(p, 'celula', None)` fica fora da varredura. `app/chamados/juizes.py:1147`.

2. **PROCEDENTE — testes novos mordem os três caminhos representados, por inspeção estática.** A fixture esvazia a fila, exige C6 viva e gate retendo; o teste exige evento depois do ato, saída/permanência correta e segunda rodada sem eventos nem mudança de estado. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:89-105`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:111-125`. Os atos usam as funções reais de resposta, recusa e carimbo administrativo. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:134-137`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:153-156`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:172-175`. RED/GREEN executado: **[nao verificado]**.

3. **PROCEDENTE — baixa — falta teste da atomicidade da nova emissão.** Os testes novos derivam de `TestCase` e exercitam o caminho feliz; não há neles falha injetada em `emitir_evento_celula` para testar rollback da pergunta. `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:50`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:134-185`. O vizinho possui essa injeção para a célula, que é outra porta. `app/ponto/tests/test_o_outbox_celula.py:141-145`.

4. **Ajuste `test_o_outbox_celula`: compatível com a nova produção de eventos.** O teste continua comparando contagem exata e exigindo ausência de eventos na repetição; adiciona ao esperado os saves de perguntas existentes que acordam o consumidor. `app/ponto/tests/test_o_outbox_celula.py:104-129`. A comparação não assegura que houve qualquer pergunta salva, pois exige `len(salvos) >= 1`, mas não exige `len(perguntas) >= 1`; essa cobertura fica nos testes novos. `app/ponto/tests/test_o_outbox_celula.py:117-123`; `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:114`.

5. **Ajuste `test_orfas...`: não concluir que “2 falhas” está errado apenas porque o carimbo converge.** O carimbo tira a muda do universo de órfãs, que exige `materializada_em IS NULL`; a segunda volta não a carimba novamente. `app/chamados/reconciliador.py:1037-1040`; `app/chamados/reconciliador.py:1135-1146`. Entretanto há outro caminho para o mesmo `fechar` mockado: a retratação da premissa chama `_retratar`, cujo resolver varre disputas abertas do colaborador e chama `d.fechar` quando `pode_fechar` permite. `app/chamados/reconciliador.py:1357-1364`; `app/chamados/reconciliador.py:522-536`; `app/chamados/services/disputa_emissao.py:1648-1650`; `app/chamados/services/disputa_emissao.py:1694-1701`. O teste preserva eventos falhos pendentes e uma tentativa por evento. `app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:147-152`. Resultado executado: **[nao verificado]**.

6. **Classes no código acrescentado:** segundo juiz **ausente** — o signal transporta evento e o consumidor delega aos caminhos existentes; C6 continua registrado como `pergunta_viva`. `app/chamados/signals.py:215-221`; `app/chamados/reconciliador.py:1353-1372`; `app/core/juizes.py:266`. Segundo escritor de `EventoCelula` fora da porta **ausente** — o modelo chama a porta, que faz o INSERT. `app/chamados/models.py:2688`; `app/ponto/portas/celula.py:675`. Novo relógio fora da borda **ausente nos trechos novos** — os helpers e o receiver não consultam relógio. `app/chamados/models.py:2664-2689`; `app/chamados/signals.py:215-221`. Derivação paralela de julgamento **ausente nesses trechos**; há repetição da seleção logística FK/fallback já usada pela semeadura. `app/chamados/models.py:2681-2684`; `app/chamados/reconciliador.py:1295-1296`. Pré-requisitos de API existem na cópia; sequência efetiva do pouso **[nao verificado]**. `app/ponto/portas/celula.py:666`; `app/chamados/reconciliador.py:1304`.

## 06/11 08:24 · sessao 11aa941cbe04 · - · - · origem=Ronald (tmux)

1. **PROCEDENTE — média/alta condicional: a pergunta e o outbox não são atômicos em todos os escritores.** `salvar_e_emitir` abre `atomic` para célula+evento (`app/ponto/portas/celula.py:683`); o receiver da pergunta não abre transação (`app/chamados/signals.py:215-221`), tampouco `carimbar` em torno de seu save (`app/chamados/models.py:2605-2613`). Há chamada de produção sem atomic local: `retratar_perguntas_absolvidas_por_celula` chama `p.carimbar` e engole exceção (`app/chamados/reconciliador.py:582-592`). Em autocommit, hipótese de falha do INSERT deixa pergunta alterada sem evento; o próprio consumidor chama essa função sem receber `falhas` (`app/chamados/reconciliador.py:1354`) e pode marcar o evento antigo processado (`app/chamados/reconciliador.py:1386`). Falha efetiva em produção: **[nao verificado]**.

2. **Ausente no caminho examinado: evento sobrevivente ao rollback apenas porque emissão é síncrona.** Reabertura engloba captura/update/emissão no mesmo atomic (`app/chamados/services/disputa_emissao.py:1401-1417`). Há rollback explícito depois de carimbar validação no aceite de folga (`app/chamados/services/validacao.py:289-313`), mas o receiver insere no atomic existente (`app/chamados/signals.py:215-221`), logo a inferência no mesmo banco é rollback conjunto; divergência de aliases/conexões: **[nao verificado]**. Consumidor não roda inline no receiver: ele busca a fila por SELECT (`app/chamados/reconciliador.py:1332`). Em autocommit a pergunta já foi salva antes do INSERT, por exemplo resposta em `app/chamados/services/disputa_emissao.py:1301`; não foi demonstrada leitura externa de dado não commitado **[nao verificado]**.

3. **Realimentação presente, com convergência para dados estáveis nos caminhos examinados.** Consumidor carimba perguntas (`app/chamados/reconciliador.py:1354` e `:1364`), e carimbo salva campos que acordam (`app/chamados/models.py:2585-2609`); nova volta é possível pelo laço de busca de novos IDs (`app/chamados/reconciliador.py:1331-1335`). Após carimbo, o candidato absolvido deixa o universo `materializada_em__isnull=True` (`app/chamados/reconciliador.py:617-620`), e a órfã deixa universo equivalente (`app/chamados/reconciliador.py:1037-1040`); `encerrar_fantasma` ainda tem no-op explícito (`app/chamados/models.py:2718-2721`). `_gravar_estado` relavra as perguntas (`app/chamados/models.py:291-295`), mas a lavra salva somente `veredito_lavrado`, `julgado_em`, `janela_expira_em` (`app/chamados/veredito_lavrado.py:227-231`), disjuntos do conjunto (`app/chamados/models.py:2660-2662`), portanto receiver os ignora (`app/chamados/signals.py:219-220`). Garantia global com concorrência ou mudanças externas: **[nao verificado]**.

4. **Mapa EventoCelula.** Inserção de `celula_id`, `colaborador_id`, `data` é da porta única (`app/ponto/portas/celula.py:675`); `criado_em` é `auto_now_add`, `tentativas=0`, `erro=''`, `processado_em=NULL` pelos campos (`app/escala/models.py:1857-1863`). Consumidor lê PK e `criado_em` para ordenação/exclusão (`app/chamados/reconciliador.py:1332-1335`), `colaborador_id` para agrupar e carregar colaborador (`:1339-1345`); lê `processado_em`/`tentativas` na seleção (`:1238`), grava `tentativas`/`erro` (`:1380`), grava `processado_em` (`:1386`) e remove por retenção (`:1393`). `criado_em` alimenta também idade (`:1250-1251`), `celula_id` alimenta vigia e dedup da semeadura (`:1262`, `:1284`, `:1297`). O consumidor varre colaborador inteiro e não usa `data` nem `celula_id` para delimitar a reconciliação (`:1339-1372`); leitor de produção de `erro` ou `data` do evento não localizado na busca de referências **[nao verificado]**.

5. **Ordem de trava: nova emissão não demonstrou inversão.** Consumidor não segura atomic abrangendo a reconciliação, apenas marca os eventos num atomic posterior (`app/chamados/reconciliador.py:1344-1386`); exclusividade do comando usa advisory try-lock (`app/chamados/management/commands/consumir_eventos_celula.py:19-23`, `app/chamados/reconciliador.py:1223-1231`). `reconciliar` trava chamado antes de `_gravar_estado` (`app/chamados/models.py:616-620`, `:673-675`); relavra pergunta só depois do save do chamado (`:279-295`). Nova porta busca célula com SELECT simples e insere evento (`app/chamados/models.py:2681-2688`); não há `select_for_update` nela. Ausência de qualquer deadlock em produção: **[nao verificado]**.

6. **Sobre teste de órfãs: ajuste admite nova volta, não leitura repetida do mesmo evento.** Teste espera duas falhas, mantém evento pendente e verifica todas as tentativas iguais a 1 (`app/chamados/tests/test_orfas_nao_morre_na_guarda_da_porta.py:144-152`); exclusão por IDs lidos garante isso no código (`app/chamados/reconciliador.py:1332-1335`). A contagem não é prova suficiente de convergência universal; o ponto que limita as emissões de carimbo é o filtro de materialização acima (`app/chamados/reconciliador.py:619`, `:1038`). Teste não executado por restrição de somente leitura **[nao verificado]**.

## 06/11 08:06 · sessao f4de5888dd8a · LASTRO-PARTE-3 · censo · origem=Code

**1. Censo dos escritores**

Os caminhos abaixo distinguem **a instrução que persiste** da função que a chama. `_escrever_carimbo` e `_desfazer_em_memoria` atribuem valores em memória; a persistência acontece nas portas que executam `save`/`update`. (`app/chamados/models.py:2556`, `app/chamados/models.py:2609`, `app/chamados/models.py:2616`)

Na coluna de transação, “sem atomic próprio” significa que a função não abre um `atomic` explícito; ela pode herdar a transação do chamador. Os exemplos de herança estão discriminados depois das tabelas. Os ganchos da coluna final são adicionais ao receiver comum descrito na resposta 2. (`app/chamados/models.py:2562`, `app/chamados/services/disputa_emissao.py:1247`, `app/chamados/signals.py:191`)

| Função | Forma e campos persistidos | Atomic explícito | EventoCelula / `reconciliar()` depois |
|---|---|---|---|
| `PerguntaDisputa.abrir`, nascimento | `cls.objects.create`: fornece `disputa`, `data_evento`, `celula` e demais argumentos; os outros campos pedidos nascem pelos defaults ou por `**campos`. (`app/chamados/models.py:2476`, `:2227`, `:2322`, `:2791`) | Sem próprio. (`app/chamados/models.py:2398`) | O corpo termina com auditoria e retorno. O chamador `gerar_perguntas_disputa` pode, **depois**, chamar `reatar_pergunta_orfa`, condicionalmente. (`app/chamados/models.py:2478`, `:2489`; `app/chamados/services/disputa_emissao.py:404`, `:408`) |
| `PerguntaDisputa.abrir`, reaproveitamento | `ex.save(update_fields=upd)`: `celula` quando encontrada e diferente; `chamado_destino` quando reapontado e permitido; acrescenta `veredito_lavrado`, `julgado_em`, `janela_expira_em`. (`app/chamados/models.py:2433`, `:2450`, `:2471`, `:2474`) | Sem próprio. (`app/chamados/models.py:2398`) | Retorna imediatamente após esse bloco; sem emissão/reconciliação explícita no ramo. (`app/chamados/models.py:2474`, `:2475`) |
| `PerguntaDisputa.carimbar` | `self.save(update_fields=campos)`: base `materializada_em`, `materializada_por`, `via_resolucao`; acrescenta `respondida_em` se solicitado, houver texto e faltar carimbo; `validada_em`, `validada_por` com `resposta='validar'` e validação ausente; os três campos de lavra se `lavrar_veredito=True`. (`app/chamados/models.py:2585`, `:2587`, `:2591`, `:2606`, `:2609`) | Sem próprio; contextos abaixo. (`app/chamados/models.py:2562`) | Depende do chamador: há fechamento de disputa e reconciliação de destinos depois, discriminados abaixo. (`app/chamados/reconciliador.py:586`, `:602`; `app/chamados/services/materializacao.py:549`, `:564`) |
| `PerguntaDisputa.carimbar_identidade` | `self.save`: `['via_resolucao','materializada_em','materializada_por']`, mais `recusa_motivo` quando fornecido. (`app/chamados/models.py:2640`, `:2644`) | Sem próprio; contextos abaixo. (`app/chamados/models.py:2625`) | Depende do chamador; `fechar_por_admin` integra caminhos que reconciliam chamados depois. (`app/chamados/models.py:2522`; `app/ponto/services/veredito_celula.py:177`, `:183`) |
| `PerguntaDisputa.descarimbar` | Com `salvar=True`: `self.save(update_fields=['via_resolucao','materializada_em','materializada_por'])`; zera o trio. Com `False`, somente memória. (`app/chamados/models.py:2646`, `:2652`) | Sem próprio; herdado nos caminhos abaixo. (`app/chamados/models.py:2646`) | Sem emissão/reconciliação própria; devolve a lista de campos. (`app/chamados/models.py:2650`, `:2653`) |
| `PerguntaDisputa.descarimbar_em_lote` | `PerguntaDisputa.objects.filter(...).update(materializada_em=None, materializada_por=None, via_resolucao='', **outros)`. (`app/chamados/models.py:2663`) | Sem próprio; herda, por exemplo, o atomic de `reabrir_para_correcao`. (`app/chamados/services/disputa_emissao.py:1401`, `:1412`) | Retorna o resultado do `update`; sem gancho próprio. (`app/chamados/models.py:2663`) |
| `PerguntaDisputa.encerrar_por_fato_no_espelho` | `self.save(update_fields=['via_resolucao'])`, escrevendo `VIA_FATO_MATERIALIZADO`; já nessa via é no-op. (`app/chamados/models.py:2671`, `:2674`) | O comando chamador abre atomic por pergunta. (`app/chamados/management/commands/encerrar_perguntas_com_fato.py:67`, `:68`) | A porta retorna após salvar; sem emissão/reconciliação própria. (`app/chamados/models.py:2674`, `:2675`) |
| `responder_pergunta_disputa_core` | `p.save(update_fields=_campos)`: sempre `resposta_colab`, `respondida_em`; acrescenta `validada_em`, `validada_por` para desfazer validação anterior; acrescenta o trio do descarimbo quando `reabrir_por_resposta()` o devolve. (`app/chamados/services/disputa_emissao.py:1282`, `:1295`, `:1300`, `:1301`; `app/chamados/models.py:2711`) | Sem próprio; há chamadores com e sem atomic local. (`app/chamados/services/disputa_emissao.py:1247`; `app/chamados/services/declaracao_texto.py:302`, `:368`; `app/chamados/views.py:1283`) | Chama `_ressuscitar_por_resposta`; para escapes de ausência chama materialização, que pode reconciliar destinos. Resposta HORA simples não tem chamada explícita ao emissor/reconciliação nesse corpo. (`app/chamados/services/disputa_emissao.py:1326`, `:1330`, `:1359`; `app/chamados/services/materializacao.py:563`, `:564`) |
| `reabrir_para_correcao` | `qs.update(**campos)`: `respondida_em=None`, `validada_em=None`, `validada_por=None`, `resposta_colab=''`. Com `desfazer_absorcao=True`, delega esses campos a `descarimbar_em_lote`, limpando também o trio de materialização. (`app/chamados/services/disputa_emissao.py:1393`, `:1412`, `:1413`) | **Próprio**, envolvendo histórico e alteração. (`app/chamados/services/disputa_emissao.py:1401`, `:1403`) | Não emite nem chama `reconciliar()` no corpo; termina nos retornos das escritas. (`app/chamados/services/disputa_emissao.py:1412`, `:1413`) |
| `responder_automaticamente` | `p.save(update_fields=['resposta_colab','respondida_em'])`. (`app/chamados/services/disputa_emissao.py:1428`, `:1430`) | Sem próprio. (`app/chamados/services/disputa_emissao.py:1417`) | Retorna após salvar; o chamador da foto chama `validar_pergunta` depois. (`app/chamados/services/disputa_emissao.py:1431`; `app/api/foto_recebida.py:48`, `:51`) |
| `carimbar_resposta_pela_trilha` | `p.save(update_fields=['respondida_em'])`. (`app/chamados/services/disputa_emissao.py:1445`, `:1446`) | Sem próprio. (`app/chamados/services/disputa_emissao.py:1434`) | Depois registra evento de **observabilidade**, não `EventoCelula`, e retorna. (`app/chamados/services/disputa_emissao.py:1447`, `:1450`) |
| `reapontar_resolvedora` | `p.save`: `['chamado_destino']`, mais `data_evento` **somente se estava ausente**, mais `['veredito_lavrado','julgado_em','janela_expira_em']`. (`app/chamados/services/fio.py:831`, `:832`, `:834`, `:847`) | **Próprio**, `atomic(savepoint=False)`. (`app/chamados/services/fio.py:837`) | Sem emissão/reconciliação interna. Há chamador que reconcilia depois: `limbo_folgas.agrupar_furo`. (`app/chamados/services/fio.py:849`; `app/chamados/services/limbo_folgas.py:268`, `:276`, `:277`) |
| `relavrar` | `p.save(update_fields=['via_resolucao','veredito_lavrado','julgado_em','janela_expira_em'])`. (`app/chamados/services/relavra_via.py:123`, `:125`) | Sem próprio; o comando `relavrar_via_i6` envolve a chamada em atomic. (`app/chamados/services/relavra_via.py:104`; `app/chamados/management/commands/relavrar_via_i6.py:101`, `:102`) | Depois registra observabilidade e retorna; sem emissão/reconciliação explícita. (`app/chamados/services/relavra_via.py:130`, `:137`) |
| `_carimbar_validacao` | `perg.save(update_fields=['validada_em','validada_por'])`. (`app/chamados/services/validacao.py:131`, `:133`) | Sem próprio, mas os três sítios de chamada estão nos atomics de `_aceitar_a_folga` ou `validar_pergunta`. (`app/chamados/services/validacao.py:262`, `:263`, `:289`, `:290`, `:395`, `:396`) | O aceite de folga fecha o fio ou declara o dia; a validação normal chama materialização, cujo fechamento de destinos usa `reconciliar()`. (`app/chamados/services/validacao.py:270`, `:291`, `:430`; `app/chamados/services/materializacao.py:741`) |
| `devolver_validacao_recusada` | `perg.save(update_fields=['validada_em','validada_por'])`, zerando ambos. (`app/chamados/services/validacao.py:188`, `:190`) | Sem próprio; seu chamador `validar_pergunta` a executa dentro do atomic. (`app/chamados/services/validacao.py:395`, `:425`) | Pode reabrir a disputa por `reabrir_sistema`; não chama diretamente emissor/reconciliação. (`app/chamados/services/validacao.py:199`, `:200`, `:205`) |

Os contextos das portas do modelo completam a distinção entre escritor e chamador:

- **Nascimento/reaproveitamento:** `gerar_perguntas_disputa` chama `abrir`; os caminhos `criar_disputa_a_partir_de_chamados_orfaos` e `criar_disputa_ancora_de_chamado` possuem atomic exterior. (`app/chamados/services/disputa_emissao.py:389`, `:898`, `:1148`, `:1195`)
- **`carimbar`:** recebe chamadas de `lavrar_sem_turno`, absolvição pela célula, encerramento de fantasma, resolução por chamado encerrado, absorção por ausência e materialização. Há atomic próprio do chamador em `_retratar` e no encerramento de órfãs; a absolvição pela célula não recebe um atomic exterior do consumidor. (`app/chamados/reconciliador.py:405`, `:522`, `:586`, `:1135`, `:1136`, `:1314`; `app/chamados/services/disputa_emissao.py:1603`, `:1675`; `app/chamados/services/materializacao.py:549`, `:1202`, `:1349`)
- **`carimbar_identidade`:** os quatro chamadores são `fechar_por_admin`, `retratar_duplicata`, `_materializar_resposta_hora` e `_carimbar_recusa`. O fechamento administrativo da disputa envolve o primeiro em atomic; o fechamento do fio do dia salva perguntas e depois reconcilia chamados. (`app/chamados/models.py:2522`, `:1846`, `:1855`; `app/chamados/services/fio.py:891`; `app/chamados/services/materializacao.py:894`, `:1069`; `app/ponto/services/veredito_celula.py:177`, `:183`)
- **`descarimbar`:** é usado por `reabrir_fantasma`, `reabrir_por_competencia`, `reabrir_por_resposta(salvar=False)` e `desfazer_carimbo_sem_lastro`. Os três caminhos que persistem têm atomic nos respectivos chamadores mostrados. (`app/chamados/models.py:2696`, `:2713`, `:2720`; `app/chamados/services/relavra_via.py:217`, `:231`; `app/chamados/services/competencia_trancada.py:253`, `:254`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:146`, `:147`)
- **`descarimbar_em_lote`:** além da correção de questionário, recebe `validada_em=None`, `validada_por=None`, `respondida_em=None` de `ressuscitar_perguntas`; essa função não abre atomic próprio. (`app/chamados/models.py:1461`, `:1504`; `app/chamados/services/disputa_emissao.py:1401`, `:1412`)

Não apareceu no censo outro `save` dos oito campos sem `update_fields`, nem `bulk_create`/`bulk_update` deles. O `bulk_update` de `PerguntaDisputa` existente grava exclusivamente os três campos de lavra. A inexistência universal de outra forma dinâmica não reconhecida: **[nao verificado]**. (`app/chamados/veredito_lavrado.py:194`, `:207`)

**2. Receivers de PerguntaDisputa**

O receiver explícito encontrado é **`_becos_post_save_pergunta`**, de `post_save`. Ele ignora `raw` e pergunta sem `disputa_id`, captura o ID da disputa e agenda `fechar_beco_da_disputa` com `transaction.on_commit`, sob `engolir`. Não filtra por `created` nem pelos campos alterados. (`app/chamados/signals.py:191`)

A porta do beco relê a disputa, verifica se é fechável, protege reentrada por `_EM_CURSO` e, em atomic, resolve perguntas dos chamados encerrados e fecha a disputa quando permitido. O fechamento da disputa pode chamar `reconciliar()` do chamado-pai pelo outro receiver. Portanto um `save` de pergunta **já pode chegar indiretamente à reconciliação**, mas isso é condicional ao beco e às guardas de fechamento. (`app/chamados/services/becos.py:25`, `:41`, `:49`, `:52`, `:65`; `app/chamados/signals.py:18`, `:41`, `:42`)

Não localizei `pre_save` de `PerguntaDisputa` nem outra conexão explícita na cópia; registro dinâmico em execução: **[nao verificado]**. O carregamento dos sinais do app acontece em `ready()`. (`app/chamados/apps.py:8`)

**3. Selos e impacto de acrescentar chamadas ao emissor**

| Selo | Enumeração/exigência |
|---|---|
| `test_porta_pergunta_identidade` | Declara sete atos e exige sete escritas censadas no módulo do modelo; também exige a tupla exata de portas. **A contagem inclui a exclusão de perguntas por `remover_motivo`.** (`app/chamados/tests/test_porta_pergunta_identidade.py:60`, `:77`, `:102`, `:111`) |
| `test_porta_resposta_pergunta` | Exige quatro sítios: dois em `responder_pergunta_disputa_core`, um em `carimbar_resposta_pela_trilha`, um em `responder_automaticamente`; vincula contagem à função por AST. (`app/chamados/tests/test_porta_resposta_pergunta.py:66`, `:97`, `:107`) |
| `test_porta_desfecho_pergunta` | Exige um sítio em `_materializar_resposta_hora`, um em `_carimbar_recusa` e **zero escrita direta de pergunta em signals**. (`app/chamados/tests/test_porta_desfecho_pergunta.py:72`, `:102`, `:138`) |
| `test_porta_reapontamento_pergunta` | Exige uma escrita em `fio.reapontar_resolvedora`, zero no reconciliador; proíbe atribuições de `data_evento`/`chamado_destino` no reconciliador e escrita direta no comando de dedup. (`app/chamados/tests/test_porta_reapontamento_pergunta.py:77`, `:113`, `:147`, `:163`) |
| `test_porta_veredito_pergunta` | Exige uma escrita em cada função: `lavrar`, `gravar_lavras_em_lote`, `relavrar`, com conferência por função. (`app/chamados/tests/test_porta_veredito_pergunta.py:94`, `:123`) |
| `test_porta_validacao_pergunta` | Enumera `_carimbar_validacao`, `recusar_pergunta`, `conferir_data_do_fato`, `devolver_validacao_recusada`; exige uma escrita em cada e zero nas cascas views/ações. (`app/chamados/tests/test_porta_validacao_pergunta.py:72`, `:95`, `:122`) |
| `test_o261e_f2_porta_carimbo` | Allowlist por arquivo/função para atribuição de materialização e via; exige nenhum escritor fora e nenhum declarado fantasma. Enumera também referências a `_escrever_carimbo`, `_desfazer_em_memoria`, `carimbar_identidade`, incluindo as vias permitidas dos quatro chamadores de identidade. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35`, `:44`, `:224`, `:373`, `:398`, `:485`) |
| `test_contract_escritores_estado` | Enumera arquivos escritores de `data_evento`, materialização, validação e resposta; usa regex sobre código sem comentários e rejeita novos escritores e declarações fantasmas. (`app/core/tests/test_contract_escritores_estado.py:61`, `:83`, `:133`, `:177`, `:200`) |
| `test_contract_dono_validada` | Dono é `validacao.py`; allowlist efetiva contém `disputa_emissao.py`, `materializacao.py`, `models.py`. Procura atribuição/`update` de validação e exige zero fora. (`app/chamados/tests/test_contract_dono_validada.py:26`, `:30`, `:47`, `:65`) |
| `test_chokepoint_chamado_gate` | Exige toda escrita censada da família dentro das portas declaradas, sem pendências/exceções. As oito portas de pergunta estão registradas em `core/portas.py`. (`app/chamados/tests/test_chokepoint_chamado_gate.py:316`, `:347`, `:391`; `app/core/portas.py:392`) |
| `test_reabertura_fantasma` | Enumera três origens antigas, proíbe nelas a string de reabertura crua e exige os quatro campos zerados na fonte da porta canônica. (`app/chamados/tests/test_reabertura_fantasma.py:43`, `:53`, `:68`) |
| `test_porta_remocao_motivo` | Exige `remover_motivo` entre os escritores censados de pergunta e zero escrita de disputa/pergunta nas duas origens antigas. (`app/chamados/tests/test_porta_remocao_motivo.py:140`, `:156`) |

Há ainda verificadores sintáticos da forma das escritas: texto acompanhado de instante na mesma função/chamada; materialização acompanhada de autor; retorno de `carimbar` não ignorado. Eles não enumeram chamadores do emissor. (`app/chamados/tests/test_contract_par_resposta.py:65`, `:87`; `app/chamados/tests/test_contract_texto_com_carimbo.py:60`, `:148`; `app/chamados/tests/test_contract_autor_muda.py:29`; `app/chamados/tests/test_o261e_f2_fila.py:40`)

**Não encontrei allowlist/contagem de chamadores de `emitir_evento_celula`.** Os selos próximos verificam outras coisas: `SeloSaveDaPortaTest` inspeciona saves de veredito/ata **dentro do arquivo da porta**; o gate da célula verifica funções que contêm escrita, não seus chamadores. Inexistência universal de outro selo: **[nao verificado]**. (`app/ponto/tests/test_o_outbox_celula.py:632`; `app/ponto/tests/test_chokepoint_celula_gate.py:62`)

Consequência estática para as duas alterações hipotéticas:

- **Receiver somente chamando `emitir_evento_celula`:** os enumeradores acima não o tratam como nova escrita direta de pergunta. O censo não segue chamadas de serviço interproceduralmente. Se o receiver passasse a salvar pergunta diretamente, o selo de signals e o gate da família acusariam. (`app/core/censo_escritas.py:364`; `app/chamados/tests/test_porta_desfecho_pergunta.py:138`; `app/chamados/tests/test_chokepoint_chamado_gate.py:347`)
- **`reabrir_para_correcao` somente chamando o emissor:** também não acrescenta escrita direta de pergunta para esses enumeradores. Se acrescentasse nova escrita reconhecida pelo censo, a contagem/vinculação de funções da porta da resposta acusaria. (`app/core/censo_escritas.py:364`; `app/chamados/tests/test_porta_resposta_pergunta.py:66`, `:97`, `:107`)

Resultado comportamental da suíte após qualquer dessas mudanças: **[nao verificado]**. O teste de reentrega existente exige uma retratação e nenhum followup duplicado, mas aceita eventos adicionais gerados pela própria retratação. (`app/ponto/tests/test_o_outbox_celula.py:149`, `:161`, `:170`)

**4. Forma usada para encontrar a CelulaDia e nulabilidade**

No nascimento de pergunta de fato, a busca é pelo par **`(disputa.colaborador_id, data_evento)`**; o resultado é gravado em `p.celula`. No C6, a decisão sobre veredito usa diretamente **`p.celula`**. A semeadura do outbox já usa a forma “FK quando existe; senão busca pelo par”. (`app/chamados/models.py:2406`, `:2430`, `:2476`; `app/chamados/juizes.py:1147`, `:1180`; `app/chamados/reconciliador.py:1295`)

Essas formas não têm equivalência garantida: `_celulas_da_pergunta` só reaproveita a FK quando colaborador e data conferem; caso contrário devolve `None` para a lei consultar por seus próprios meios. (`app/chamados/juizes.py:621`)

- **`celula=NULL`, `data_evento` preenchida: permitido e alcançável.** Na conversa, `abrir` troca data ausente por `localdate()`, mas mantém `celula=None`, pois pula a busca reservada à pergunta de fato. Na pergunta de fato, `.first()` também pode não encontrar célula. (`app/chamados/models.py:2407`, `:2424`, `:2428`, `:2430`, `:2476`)
- **`data_evento=NULL`: permitido pelo modelo.** A porta `abrir` preenche a ausência; `reapontar_resolvedora` contém tratamento para pergunta já existente sem data. Existência dessas linhas no PROD fornecido: **[nao verificado]**. (`app/chamados/models.py:2218`, `:2424`; `app/chamados/services/fio.py:832`)
- A FK aceita `NULL`, usa `SET_NULL`, e a `Meta` de pergunta não declara constraint conjugando FK e data. (`app/chamados/models.py:2209`, `:2811`)

**5. Contraexemplo de realimentação pelo consumidor**

**Existe caminho `consumidor → save de pergunta → novo evento → consumidor`. Isso demonstra realimentação; laço infinito não fica demonstrado.** O emissor insere um evento novo e o consumidor percorre novos pendentes em `while True`, excluindo somente os PKs já lidos naquela execução. (`app/ponto/portas/celula.py:673`; `app/chamados/reconciliador.py:1329`)

| Caminho alcançado pelo consumidor | Persistência de PerguntaDisputa |
|---|---|
| Absolvição pela célula | Consumidor chama `retratar_perguntas_absolvidas_por_celula`, que chama `p.carimbar`; o save real está no modelo. (`app/chamados/reconciliador.py:1353`, `:586`; `app/chamados/models.py:2609`) |
| Órfã encerrada pela lâmpada | `encerrar_orfas_pela_lampada → encerrar_fantasma → carimbar → save`. (`app/chamados/reconciliador.py:1363`, `:1136`; `app/chamados/models.py:2685`, `:2609`) |
| Retratação por célula ou premissa | `_retratar → resolver_pergunta

[truncado em 20000 de 26767 caracteres]

## 06/11 08:03 · sessao f857db3e22f3 · - · - · origem=Ronald (tmux)

Complementos do inventário Q3:

- **`test_contract_dono_validada.py`** enumera dono (`validacao.py`) e três exceções (`disputa_emissao.py`, `materializacao.py`, `models.py`), detectando atribuição ou `update(validada_em=...)`; exige nenhum escritor fora deles. O comentário “allowlist vazia” diverge do dict existente (`app/chamados/tests/test_contract_dono_validada.py:26-43`, `:47-71`).
- **`test_reabertura_fantasma.py`** enumera três origens onde proíbe a string `update(respondida_em=None`; exige também import canônico e quatro campos zerados na fonte de `reabrir_para_correcao` (`app/chamados/tests/test_reabertura_fantasma.py:43-48`, `:53-81`).
- **`test_porta_remocao_motivo.py`** exige `DisputaSupervisao.remover_motivo` entre os escritores censados de pergunta em models e zero escrita de disputa/pergunta nas duas origens antigas (`app/chamados/tests/test_porta_remocao_motivo.py:140-171`; origens em `:79-81`).

Outros testes inspecionados varrem formas de escrita, sem enumerar conjunto exato de escritores/chamadores: [nao verificado]

- `test_contract_par_resposta.py`: atribuição de texto ou `update(resposta_colab=...)` exige escrita de `respondida_em` na mesma função; allowlist vazia (`:36-45`, `:65-94`).
- `test_contract_texto_com_carimbo.py`: texto por atribuição ou kwargs de `update/create/get_or_create/update_or_create`; exige carimbo na mesma chamada ou nome presente no escopo AST da atribuição (`:60-108`, `:148-172`).
- `test_contract_autor_muda.py`: atribuição de `materializada_em` exige `materializada_por`/`_id` na janela até save ou quarenta linhas; exceção para variável `reg` (`:21-57`).
- `test_o261e_f2_fila.py`: proíbe chamada `p.carimbar(...)` como statement solto em produção; não possui lista exata de chamadores (`:40-60`).
- `test_porta_unica.py`: enumera três arquivos para regras de fechamento de **DisputaSupervisao**, sem enumerar escritores de PerguntaDisputa (`:10-38`).

Uma mera chamada a `emitir_evento_celula` não corresponde aos critérios sintáticos desses testes; resultado da suíte modificada: [não verificado]. [nao verificado]

## 06/11 08:01 · sessao 7c10cf86c128 · - · - · origem=Ronald (tmux)

Oito sítios externos a `app/chamados/models.py` escrevem algum dos oito campos pedidos:

| Função / sítio | Forma e campos | Atomic | Gancho posterior |
|---|---|---|---|
| `responder_pergunta_disputa_core` | `save(update_fields=_campos)`: sempre `resposta_colab`, `respondida_em`; adiciona `validada_em`, `validada_por` se havia validação; adiciona os campos devolvidos por `p.reabrir_por_resposta()` (`app/chamados/services/disputa_emissao.py:1282`, `:1295`, `:1300`, `:1301`). | Sem atomic próprio na função (`app/chamados/services/disputa_emissao.py:1247`). Os callers `declarar_hora_colab` e `aplicar_declaracao` têm atomic (`app/chamados/services/declaracao_texto.py:302`, `:368`, `:605`, `:641`); PWA e API chamam sem atomic local (`app/chamados/views.py:1283`, `app/api/views.py:2643`). | Chama `_ressuscitar_por_resposta` e, para escapes de ausência, `_materializar_ausencia_de_resposta` (`app/chamados/services/disputa_emissao.py:1326`, `:1330`). Este último carimba irmãs e chama `q.chamado_destino.reconciliar()` quando a ausência não está aberta (`app/chamados/services/materializacao.py:549`, `:563`, `:564`). Resposta HORA simples chega ao retorno sem chamada direta de emissão/reconciliação (`app/chamados/services/disputa_emissao.py:1359`). |
| `reabrir_para_correcao` | `qs.update(**campos)` com `respondida_em=None`, `validada_em=None`, `validada_por=None`, `resposta_colab=''`; com `desfazer_absorcao=True`, delega a `PerguntaDisputa.descarimbar_em_lote(qs, **campos)` (`app/chamados/services/disputa_emissao.py:1393`, `:1412`, `:1413`). | Atomic próprio, envolvendo histórico e alteração (`app/chamados/services/disputa_emissao.py:1401`, `:1403`). | Não chama diretamente emissão/reconciliação; termina nos dois retornos da escrita (`app/chamados/services/disputa_emissao.py:1412`, `:1413`). Caller de rejeição da ausência também possui atomic e reabre disputas depois (`app/chamados/services/materializacao.py:657`, `:658`, `:661`). |
| `responder_automaticamente` | `save(update_fields=['resposta_colab', 'respondida_em'])` (`app/chamados/services/disputa_emissao.py:1428`, `:1430`). | Sem atomic próprio (`app/chamados/services/disputa_emissao.py:1417`). | Retorna imediatamente após save (`app/chamados/services/disputa_emissao.py:1431`). Caller da foto chama `validar_pergunta` depois (`app/api/foto_recebida.py:48`, `:51`). |
| `carimbar_resposta_pela_trilha` | `save(update_fields=['respondida_em'])` (`app/chamados/services/disputa_emissao.py:1445`, `:1446`). | Sem atomic próprio (`app/chamados/services/disputa_emissao.py:1434`). | Só registra evento de observabilidade e retorna (`app/chamados/services/disputa_emissao.py:1447`, `:1450`). Caller `curar_perguntas_em_limbo.handle` não abre atomic local; chama esta porta e depois `reabrir_para_correcao` para outro conjunto (`app/chamados/management/commands/curar_perguntas_em_limbo.py:43`, `:67`, `:69`). |
| `reapontar_resolvedora` | `save(update_fields=campos + ['veredito_lavrado','julgado_em','janela_expira_em'])`; `campos=['chamado_destino']` e adiciona `data_evento` somente quando recebeu data e a pergunta ainda não tinha data (`app/chamados/services/fio.py:831`, `:832`, `:834`, `:835`, `:847`). | Atomic próprio `savepoint=False` (`app/chamados/services/fio.py:837`). | Sem emissão/reconciliação interna; retorna após o atomic (`app/chamados/services/fio.py:849`). Há caller que reconcilia depois: `limbo_folgas.agrupar_furo`, com atomic exterior, reaponta e chama `furo.reconciliar()` (`app/chamados/services/limbo_folgas.py:268`, `:276`, `:277`, `:280`). Ramo histórico do reatador chama `resolver_perguntas_por_chamado_resolvido` depois (`app/chamados/reconciliador.py:1625`, `:1631`). |
| `relavrar` | `save(update_fields=['via_resolucao','veredito_lavrado','julgado_em','janela_expira_em'])` (`app/chamados/services/relavra_via.py:123`, `:125`). | Sem atomic próprio (`app/chamados/services/relavra_via.py:104`). O comando `relavrar_via_i6` envolve a chamada em atomic (`app/chamados/management/commands/relavrar_via_i6.py:101`, `:102`). | Depois escreve evento de observabilidade e retorna; sem emissão/reconciliação direta (`app/chamados/services/relavra_via.py:130`, `:137`). `relavrar_dia` apenas delega (`app/chamados/services/relavra_via.py:155`); o censo de funções mortas documenta ausência de caller desde 05/09 (`bin/esmeril_chamado_censo.py:141`). |
| `_carimbar_validacao` | `save(update_fields=['validada_em','validada_por'])` (`app/chamados/services/validacao.py:131`, `:133`). | Todos os três sítios de chamada no app estão sob atomic: `_aceitar_a_folga` nos ramos “já folga” e declaração; `validar_pergunta` (`app/chamados/services/validacao.py:262`, `:263`, `:289`, `:290`, `:395`, `:396`). | Validação normal chama materialização depois (`app/chamados/services/validacao.py:430`), cujos ramos chamam `_reconciliar_destino_e_clones`, que chama `.reconciliar()` (`app/chamados/services/materializacao.py:944`, `:1029`, `:1049`, `:741`). Aceite de folga chama o fechamento do fio ou declara o dia (`app/chamados/services/validacao.py:270`, `:291`). |
| `devolver_validacao_recusada` | `save(update_fields=['validada_em','validada_por'])`, zerando ambos (`app/chamados/services/validacao.py:188`, `:190`). | Sem atomic próprio (`app/chamados/services/validacao.py:155`); seu caller no app é `validar_pergunta`, dentro do atomic de linha 395 (`app/chamados/services/validacao.py:425`). | Pode chamar `d.reabrir_sistema()`; depois escreve evento de observabilidade (`app/chamados/services/validacao.py:199`, `:200`, `:205`). Não há chamada direta de emissão/reconciliação nesse corpo (`app/chamados/services/validacao.py:155`). |

`materializacao.py` salva perguntas nas linhas 821 e 1073, mas somente `pendencia_admin`/`recusa_motivo`; portanto esses dois saves não escrevem os oito campos deste censo (`app/chamados/services/materializacao.py:821`, `:1073`). O mesmo vale para `validacao.py:546` e `:619`, que escrevem somente `recusa_motivo` e o par de conferência da data (`app/chamados/services/validacao.py:546`, `:619`).

Verificação adicional em memória: além do censo existente, varri via AST todos os `.py` da cópia, retirando testes e migrations, procurando chamadas dos métodos pedidos com kwargs dos oito campos, incluindo aliases `celula_id` e `disputa_id`. Encontrou apenas `PerguntaDisputa.abrir` criando (`app/chamados/models.py:2476`), `descarimbar_em_lote` atualizando (`app/chamados/models.py:2663`) e a criação de **EventoCelula**, que é outro modelo (`app/ponto/portas/celula.py:673`). A escrita dinâmica `qs.update(**campos)` de reabertura está explicitamente inventariada acima (`app/chamados/services/disputa_emissao.py:1413`). Não executei testes nem importei Django; usei `python3 -B` e leitura/AST em memória [não verificado quanto a evidência persistida].

## 06/11 08:00 · sessao c33a4501a9fe · - · - · origem=Ronald (tmux)

**Q4 — FK e par colaborador/data coexistem; não há equivalência garantida.**

- `celula` aceita `NULL` e usa `SET_NULL`; `data_evento` também aceita `NULL` (`app/chamados/models.py:2208`, `app/chamados/models.py:2218`). A `Meta` de PerguntaDisputa não declara constraint conjugando os dois campos (`app/chamados/models.py:2811`).
- O nascimento canônico é `PerguntaDisputa.abrir`: parte com `celula = None`; para pergunta de fato procura `CelulaDia.objects.filter(colaborador_id=disputa.colaborador_id, data=data_evento).first()`; cria com essa FK, mesmo se a busca não achar célula (`app/chamados/models.py:2398`, `app/chamados/models.py:2406`, `app/chamados/models.py:2430`, `app/chamados/models.py:2476`).
- Há caminho explícito **celula NULL e data preenchida**: pergunta de conversa chega sem data, recebe `localdate()`, mas pula a busca da célula, reservada a `not _conversa` (`app/chamados/models.py:2407`, `app/chamados/models.py:2424`, `app/chamados/models.py:2428`).
- O modelo permite data NULL; `abrir` preenche a data ausente. Existência de linhas com data NULL no dado fornecido: **[não verificado]** (`app/chamados/models.py:2218`, `app/chamados/models.py:2424`).
- C6 lê diretamente `p.celula` para veredito/via e para a decisão sobre o dia (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1180`). Sua consulta alternativa da lei usa `p.disputa.colaborador` e `p.data_evento` (`app/chamados/juizes.py:1197`).
- `_celulas_da_pergunta` reaproveita a FK apenas se `c.data == p.data_evento` e `c.colaborador_id == colab.pk`; caso contrário entrega `None`, permitindo a consulta própria da lei. A própria docstring avisa que a FK nem sempre corresponde ao dia de `data_evento` (`app/chamados/juizes.py:612`, `app/chamados/juizes.py:621`).
- Existe leitor nomeado para buscar célula por par: `celula_do_dia_agora(colaborador, d)` consulta por colaborador/data e aplica a projeção do julgamento em curso; não recebe uma pergunta diretamente (`app/chamados/juizes.py:375`).

**Q5 — Há realimentação no grafo; laço infinito não está demonstrado.**

Se um receiver emitisse evento em **todo** save de pergunta, seriam alcançáveis caminhos `consumidor → save de pergunta → novo EventoCelula → consumidor`: o emissor faz `objects.create`, e o consumidor busca novos pendentes em `while True`, excluindo somente PKs que já leu (`app/ponto/portas/celula.py:666`, `app/ponto/portas/celula.py:673`, `app/chamados/reconciliador.py:1329`).

Os sítios de escrita alcançáveis pelo consumidor são:

| Caminho | Save real de PerguntaDisputa |
|---|---|
| `consumir_eventos_celula` → `retratar_perguntas_absolvidas_por_celula` → `p.carimbar` | Chamada em `app/chamados/reconciliador.py:1353` e `:586`; save em `app/chamados/models.py:2609`. |
| Consumidor → `encerrar_orfas_pela_lampada` → `encerrar_fantasma` → `carimbar` | Chamadas em `app/chamados/reconciliador.py:1363`, `:1136`, `app/chamados/models.py:2685`; mesmo save em `app/chamados/models.py:2609`. |
| Consumidor → retratação por célula ou premissa → `_retratar` → `resolver_perguntas_por_chamado_resolvido` | Chamadas em `app/chamados/reconciliador.py:1356`, `:1360`, `:993`, `:1448`, `:536`; resolver chama `carimbar(..., resposta='validar', lavrar_veredito=True)` em `app/chamados/services/disputa_emissao.py:1603`, e `q.carimbar(..., resposta='responder')` em `:1675`; save em `app/chamados/models.py:2609`. |
| Qualquer `ch.reconciliar()` desses caminhos → gate da pergunta viva → `reapontar_resolvedora` | `app/chamados/models.py:617`, `:651`, `:420`; save em `app/chamados/services/fio.py:847`, com `chamado_destino`, três campos de lavra, e `data_evento` somente quando faltava (`:831`). |
| Qualquer `ch.reconciliar()` → `_gravar_estado` → relavra das perguntas do destino | `app/chamados/models.py:620`, `:288`, `:295`; save em `app/chamados/veredito_lavrado.py:231`, update_fields `['veredito_lavrado', 'julgado_em', 'janela_expira_em']`. **Esse save não muda os campos da pergunta enumerados no enunciado, mas acionaria um receiver sem filtro.** |
| Consumidor → `fechar_pelo_marco_colab` → `ch.reconciliar()` | `app/chamados/reconciliador.py:1371`, `app/ponto/services/lastro.py:400`; pode alcançar gate, relavra e receiver S143 acima/abaixo. |
| Fechamento de chamado → receiver S143 → resolver | `app/chamados/signals.py:75`, `:91`, `:92`; mesmos `carimbar` do resolver em `app/chamados/services/disputa_emissao.py:1603`, `:1675`. |
| Fechamento de disputa → receiver S144 → `materializar_pendentes_aprovadas` → `p.carimbar` | `app/chamados/signals.py:124`, `:139`; chamada em `app/chamados/services/materializacao.py:1349`; save em `app/chamados/models.py:2609`. |
| Save de pergunta → receiver já existente → beco → resolver/fechar disputa | `app/chamados/signals.py:191`, `:201`; `app/chamados/services/becos.py:49`, `:52`; mesmos saves dos ramos anteriores. |

As guardas visíveis impedem afirmar que esse ciclo seria infinito:

- Absolvidas só seleciona `materializada_em NULL` e `validada_em NULL`; depois do carimbo sai desse universo (`app/chamados/reconciliador.py:617`).
- Órfãs só seleciona perguntas sem carimbo/validação, em disputa aberta; `encerrar_fantasma` tem no-op para fantasma já carimbada (`app/chamados/reconciliador.py:1037`, `app/chamados/models.py:2682`).
- Resolver filtra `materializada_em NULL`; seu ramo sem credor restringe `_abertas` a materializada e validada nulas (`app/chamados/services/disputa_emissao.py:1572`, `:1656`).
- S144 também filtra `materializada_em NULL` (`app/chamados/services/materializacao.py:1343`).
- Relavra por estado só entra ao cruzar a fronteira de `ENCERRADOS`, não em cada consumo (`app/chamados/models.py:267`, `:286`).
- O receiver de beco já contém proteção contra reentrada `_EM_CURSO` por disputa (`app/chamados/services/becos.py:57`, `:65`).

Portanto: **novo evento causado por save feito no consumidor é um contraexemplo comprovável de realimentação; consumo/escrita infinitamente repetidos, sem outra mudança, ficam [não verificado]** pelas guardas acima.

**Leituras de C6, por campo pedido.**

| Campo | Quem lê |
|---|---|
| `validada_em` | `_pergunta_viva_calc`, mata no primeiro degrau (`app/chamados/juizes.py:1136`). |
| `respondida_em`, `resposta_colab` | `_pergunta_viva_calc`, ramo da resposta (`app/chamados/juizes.py:1150`); `_resposta_concorda_com_a_luz` lê texto (`:1113`). |
| `via_resolucao`, `materializada_em` | Degrau da tranca (`app/chamados/juizes.py:1145`), identidade (`:1171`), marco (`:1177`) e fallback (`:1207`). |
| `celula` | C6 captura FK (`app/chamados/juizes.py:1147`); `_celulas_da_pergunta` valida identidade (`:621`). |
| `data_evento`, `disputa` | C6 consulta lei por dia/colaborador (`app/chamados/juizes.py:1197`); fallback de lastro também lê os dois (`:658`, `:664`, `:749`, `:752`). |
| `CelulaDia.veredito`, `veredito_via` | C6 lê decisão humana (`app/chamados/juizes.py:1148`) e veredito que acusa (`:1181`); escritor persistente é `lavrar_veredito`, atribuições em `app/ponto/portas/celula.py:812`, salvamento via `salvar_e_emitir` em `:817`. |
| `CelulaDia.ata` | Ramo de concordância lê lâmpadas (`app/chamados/juizes.py:1111`); C6 chama `marcos_pendentes` (`:1194`), cuja leitura da ata está em `app/ponto/services/lastro.py:138`. Escritores na porta: `lavrar_veredito` (`app/ponto/portas/celula.py:783`, `:810`) e `carimbar_precedencia` (`:862`, `:864`). |

Os escritores dos oito campos PerguntaDisputa estão no censo do agente principal; acima estão as leituras e os saves adicionais necessários para o grafo do consumidor.

## 06/11 08:00 · sessao f857db3e22f3 · - · - · origem=Ronald (tmux)

Q2: há um receiver explícito de `PerguntaDisputa`, `_becos_post_save_pergunta`, registrado em `post_save`; ignora `raw` e ausência de `disputa_id`, captura esse ID e agenda `fechar_beco_da_disputa` por `transaction.on_commit`, sob `engolir`. Não chama diretamente `emitir_evento_celula` nem `reconciliar` (`app/chamados/signals.py:191-202`). O app carrega os sinais em `ready()` (`app/chamados/apps.py:8-10`). Nenhum `pre_save` de pergunta apareceu na busca global; verificação em runtime: [não verificado].

A porta do beco relê a disputa, classifica, usa `_EM_CURSO` contra reentrada e executa `fechar_beco`; este resolve perguntas dos chamados encerrados e fecha a disputa dentro de `atomic` (`app/chamados/services/becos.py:25-33`, `:36-54`, `:57-75`).

Q3, selos enumeradores:

| Selo | O que enumera/exige |
|---|---|
| `test_porta_pergunta_identidade.py` | Sete sítios censados em models: abrir, encerrar_por_fato_no_espelho, carimbar, carimbar_identidade, descarimbar, descarimbar_em_lote e remover_motivo; igualdade de contagem (`:60-77`, `:111-124`). |
| `test_porta_resposta_pergunta.py` | responder_core=2, carimbar_resposta_pela_trilha=1, responder_automaticamente=1; quatro escritas, vinculadas à função por AST (`:66-73`, `:97-126`). |
| `test_porta_desfecho_pergunta.py` | _materializar_resposta_hora=1 e _carimbar_recusa=1; AST/contagem; `signals.py` deve ter zero escrita direta de pergunta (`:72-74`, `:102-131`, `:138-154`). |
| `test_porta_reapontamento_pergunta.py` | fio.reapontar_resolvedora=1, reconciliador=0; comparação por função; nenhuma atribuição de chamado_destino/data_evento no reconciliador; command dedup com zero escrita (`:77-88`, `:113-137`, `:147-180`). |
| `test_porta_veredito_pergunta.py` | lavrar=1, gravar_lavras_em_lote=1, relavrar=1; comparação por função (`:94-98`, `:123-147`). |
| `test_porta_validacao_pergunta.py` | _carimbar_validacao=1, recusar_pergunta=1, conferir_data_do_fato=1, devolver_validacao_recusada=1; AST; views/acoes_disputa com zero escrita (`:72-79`, `:95-115`, `:122-134`). |
| `test_chokepoint_chamado_gate.py` | Toda escrita censada da família deve estar em porta; teto/total/fora zero e allowlist vazia (`:316-328`, `:347-377`, `:391-412`). |
| `test_contract_escritores_estado.py` | Arquivos escritores de data_evento/chamado_destino (`:61-62`), materializada_em/por (`:83-104`), validada_em/por/respondida_em (`:133-173`); regex de atribuição/update após tirar comentários; proíbe novos e fantasmas (`:177-217`). |
| `test_o261e_f2_porta_carimbo.py` | Escritores de materializada_em/por e via_resolucao por `(arquivo, função)`; nenhum fora e todos declarados vivos (`:35-58`, `:224-248`). Referências/chamadores somente de `_escrever_carimbo`, `_desfazer_em_memoria`, `carimbar_identidade` (`:373-391`, `:485-494`); vias exatas de quatro juízes de identidade (`:398-403`, `:496-501`). |

Os seis selos Q1–Q6 também exigem tupla exata das oito portas, registrada em `app/core/portas.py:392-399` (asserções: identidade `:102-109`, resposta `:90-95`, desfecho `:95-100`, reapontamento `:106-111`, veredito `:118-121`, validação `:90-93`).

Não apareceu na busca global, inclusive ocultos, selo que enumere chamadores de `emitir_evento_celula`; afirmação universal de inexistência: [não verificado]. Os selos de outbox encontrados fazem outra coisa:

- `SeloSaveDaPortaTest` inspeciona exclusivamente o arquivo `ponto.portas.celula`: nenhum `save(update_fields)` de veredito/ata fora de `salvar_e_emitir`; lista não literal também acusa (`app/ponto/tests/test_o_outbox_celula.py:632-668`; campos em `:19`).
- O selo de veredito/ata varre escritas da `CelulaDia`, não chamadores do emissor (`app/ponto/tests/test_selo_veredito_so_pela_porta.py:179-194`).
- O gate da célula verifica funções que **contêm** escrita na porta, usando `OPERACOES`; não quem as chama (`app/ponto/tests/test_chokepoint_celula_gate.py:62-84`). `emitir_evento_celula` consta na operação `lavrar_veredito` (`app/ponto/portas/celula.py:64-72`).

Consequência estrutural limitada: acrescentar apenas chamada ao emissor em receiver ou `reabrir_para_correcao` não acrescenta escrita direta de `PerguntaDisputa` para esses censos. O visitante reconhece construtor e verbos ORM/save; não segue chamadas de serviço (`app/core/censo_escritas.py:214-223`, `:364-399`). Isso vale tanto para import direto quanto `modulo.emitir_evento_celula(...)`: o nome não é verbo ORM (`:375-398`). O registro da família célula contém CelulaDia/FuroDiario, sem EventoCelula (`app/core/portas.py:40-85`).

Se o receiver passasse a **escrever diretamente pergunta**, ficariam vermelhos o selo específico de signals e o gate da família (`app/chamados/tests/test_porta_desfecho_pergunta.py:138-154`; `app/chamados/tests/test_chokepoint_chamado_gate.py:347-377`). Se `reabrir_para_correcao` ganhasse escrita direta censável de pergunta, a contagem/AST da porta da resposta acusaria função fora de ATOS (`app/chamados/tests/test_porta_resposta_pergunta.py:66-73`, `:97-126`). “Todos os testes permaneceriam verdes” após apenas chamar o emissor: [não verificado], pois não executei testes.

O teste de reentrega do outbox exige uma retratação e nenhum followup duplicado, mas aceita eventos que a própria retratação gere (`app/ponto/tests/test_o_outbox_celula.py:150-173`, especialmente `:161`).

## 06/11 07:49 · sessao ef717d1946a3 · LASTRO-PARTE-3 · leitura · origem=Code

**Os cinco atos admitem montagem em TestCase, mas nem todo ramo deve fechar o chamado.** Em particular, uma resposta discordante continua prevalecendo sobre `carimbar_identidade`; esse é um controle legítimo. (`app/chamados/models.py:2625`, `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`)

Os vermelhos abaixo são **previstos pela leitura da cópia**; execução dos novos casos e confirmação independente do hash: **[nao verificado]**. Ocorrência ou quantidade desses estados em produção: **[nao verificado]**.

**Peças de fixture já existentes**

Nenhum dos helpers abaixo, isoladamente, entrega toda a combinação pedida; é necessário compor chamado, âncora e célula. O A5 de LASTRO-PARTE-2 simula a recusa com mock, portanto não demonstra retenção por C6 real. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`)

| Peça | Helper existente | Teste que o usa | [nao verificado]
|---|---|---|
| Cadastro com colaborador, usuário e admin; chamados e disputa com pai/destino separados | `_Base.setUpTestData`, `_ch`, `_disputa`, `_pergunta`, em `app/chamados/tests/test_o261e_f1_porta_encerramento.py:49`, `:59`, `:65`, `:69` | Retenção real por destino: `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82` |
| Pergunta de hora já ancorada; aceita sobrescrever motivo, resposta, data e célula por `**kw` | `PortaValidacaoPerguntaTest._pergunta`, `app/chamados/tests/test_porta_validacao_pergunta.py:175` | Recusa pública: `app/chamados/tests/test_porta_validacao_pergunta.py:233` |
| Pergunta discordante, DNA, lâmpada e dia coerente | `PerguntaRespondidaEMarcoAcesoTest._caso('19:06')`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36` | Confirma C6 viva aos seis minutos: `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59` |
| Célula acusando outro marco; pergunta muda com materialização opcional | `_Base._celula` e `_pergunta`, `app/chamados/tests/test_bug117_marco_no_espelho.py:55`, `:64` | Marco aceso sem materialização ainda deixa pergunta viva: `app/chamados/tests/test_bug117_marco_no_espelho.py:137` |
| Chamado cobrando marcos nomeados e ata plantada depois dos sinais | `_Base._caso`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63` | `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:97` |
| Vínculo válido no dia | `dar_vinculo`, `app/chamados/tests/_vinculo_do_dia.py:15` | `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:61` |

Para os casos, a composição concreta é esta:

- Colaborador ativo, com vínculo, usuário e autor administrativo; competência sem entrega/tranca. Usar os cadastros F1 e o vínculo acima; o gate só considera perguntas da competência aberta. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:49`, `app/chamados/tests/_vinculo_do_dia.py:15`, `app/chamados/models.py:347`)
- Alvo `batida_ausente`, `status_local='em_analise'`, sem silêncio; dia do chamado igual ao da célula e da pergunta; `contexto_json.marcos_faltantes` contendo **somente S/19:00**. A receita Q6 entrega chamado e âncora; a LP2 mostra como nomear o marco cobrado. (`app/chamados/tests/test_porta_validacao_pergunta.py:179`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:68`)
- Pergunta `orfao_14h`, `tipo_resposta='hora'`, `chamado_destino=alvo`, `celula=cel`; DNA com `hf='19:00'`, lâmpada S/19:00 acesa e `luz='19:00'`. Esse motivo cobra precisamente S/`hf`. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:21`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/juizes.py:869`)
- Plantar a ata final depois das criações que acionam o cartório, seguindo a ordem LP2; não deixar outra cobrança viva apta a receber reata do mesmo colaborador/dia. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:64`, `app/chamados/models.py:373`)
- Antes do ato, exigir **C6 viva**, `marcos_acesos(alvo, cel) is True`, alvo na fila e `alvo.reconciliaria() is False`. Assim o caso prova retenção real, não apenas a presença de uma pergunta. (`app/chamados/juizes.py:1135`, `app/ponto/services/lastro.py:153`, `app/chamados/services/painel.py:186`, `app/chamados/models.py:623`)

Em cada ato com `save`, executar os callbacks com `captureOnCommitCallbacks(execute=True)`, padrão já usado na suíte; o evento da pergunta é registrado em `on_commit`. Relê-se pergunta, célula e chamado antes das asserções. (`app/chamados/tests/test_varredura_becos_evento_pergunta.py:55`, `app/chamados/signals.py:191`, `app/chamados/signals.py:202`)

A ordem abaixo considera primeiro o aproveitamento direto da fixture discordante, depois os acréscimos necessários. **[nao verificado]**

1. **Corrigir a resposta para a hora da luz — RED previsto**

   **Fixture:** composição acima, pai da disputa igual ao alvo vivo; resposta anterior `19:06`, `respondida_em` preenchido, `validada_em=None`, `materializada_em=None`; célula `concorde`, via `cartorio`, demais lâmpadas acesas. A discordância mantém C6 viva mesmo com o marco aceso. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`)

   **Ato exato:** `responder_pergunta_disputa_core(p, '19:00', colab.usuario, True)`. A função grava texto e instante da nova resposta. (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1282`)

   **Pela regra:** C6 morre pela concordância com a luz em dia coerente; o alvo passa a `reconciliaria()=True` e deve sair da fila por `reconciliar()`, cujo destino é `resolvido`, via sistema. (`app/chamados/juizes.py:1130`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:623`, `app/chamados/catalogo/motor.py:95`, `app/chamados/catalogo/motor.py:111`)

   **Asserção vermelha:** exigir que o `status_local` relido esteja fora de `VIVOS`, ao final do ato e dos callbacks. A leitura prevê que continue `em_analise`: o ramo HH:MM salva a resposta e acrescenta nota, sem reconciliar o destino; o resgate não atua sem silêncio, e o beco não atua com pai vivo. (`app/chamados/services/disputa_emissao.py:1301`, `app/chamados/services/disputa_emissao.py:1326`, `app/chamados/services/disputa_emissao.py:1355`, `app/chamados/services/materializacao.py:687`, `app/chamados/services/becos.py:31`)

   **Controle:** trocar o veredito para `furo` faz a concordância com a luz deixar de absolver a pergunta; o chamado deve continuar vivo. (`app/chamados/juizes.py:1132`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:62`)

2. **Recusar a resposta / `reabrir_para_correcao` — RED previsto**

   **Fixture:** a mesma do caso 1, ainda com resposta discordante `19:06` e célula coerente. A receita da recusa pública já existe na suíte. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`, `app/chamados/tests/test_porta_validacao_pergunta.py:224`)

   **Ato público exato:** `recusar_pergunta(p.pk, admin, 'hora incorreta')`. O ato interno relevante é `reabrir_para_correcao(PerguntaDisputa.objects.filter(pk=p.pk))`, com `desfazer_absorcao=False`, o padrão. (`app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:547`, `app/chamados/services/disputa_emissao.py:1362`)

   **Pela regra:** apagada a resposta discordante, C6 morre pelo veredito coerente da célula; o alvo torna-se reconciliável e deve sair da fila pelo mesmo gate. (`app/chamados/services/disputa_emissao.py:1393`, `app/chamados/juizes.py:1195`, `app/chamados/models.py:623`)

   **Asserção vermelha:** novamente, `status_local` relido fora de `VIVOS`. A recusa salva primeiro o motivo, enquanto a resposta ainda retém; depois o `qs.update` remove resposta e validação, sem `post_save` dessa mudança e sem reconciliação na função. A nota posterior também não chama o gate nesse caminho. (`app/chamados/services/validacao.py:545`, `app/chamados/services/disputa_emissao.py:1413`, `app/chamados/services/validacao.py:551`)

   **Controle:** com célula `furo`, pergunta muda e sem materialização continua viva pelo veredito acusador; nesse ramo, reabrir não autoriza fechar. (`app/chamados/juizes.py:1183`, `app/chamados/tests/test_bug117_marco_no_espelho.py:137`)

3. **`carimbar_identidade` — um RED e um controle obrigatório**

   **Fixture do RED:** pai vivo; pergunta **muda**, sem materialização ou validação; célula `furo`, via `cartorio`, porque S/12:00 está apagada; o alvo cobra exclusivamente S/19:00, que está acesa. A receita BUG117 já demonstra pergunta viva com outro marco apagado e o próprio marco aceso, desde que ainda sem materialização. (`app/chamados/tests/test_bug117_marco_no_espelho.py:77`, `app/chamados/tests/test_bug117_marco_no_espelho.py:137`)

   **Ato exato:** `p.carimbar_identidade(PerguntaDisputa.VIA_ADMIN_DECIDIU, admin)`. Essa é uma via usada pelo próprio encerramento administrativo da pergunta. (`app/chamados/models.py:2522`, `app/chamados/models.py:2625`)

   **Pela regra:** C6 morre pela identidade; o furo vizinho não impede regularizar a premissa do chamado que cobra somente o marco aceso. Portanto, `reconciliaria()=True` e o alvo deve sair por `reconciliar()`. (`app/chamados/juizes.py:1171`, `app/chamados/models.py:1247`, `app/chamados/models.py:623`)

   **Asserção vermelha:** `status_local` relido fora de `VIVOS`. O carimbador termina salvando somente a pergunta; seu callback não encontra beco com pai vivo. (`app/chamados/models.py:2640`, `app/chamados/models.py:2644`, `app/chamados/services/becos.py:31`)

   **Controle obrigatório:** aplicar o mesmo carimbo à fixture discordante do caso 1. A pergunta continua viva porque a resposta é julgada **antes** da identidade; sem reata, `reconciliaria()` continua falso e o chamado deve permanecer vivo. Exigir fechamento aqui produziria um teste contrário à regra. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:2630`, `app/chamados/models.py:405`)

4. **Encerrar administrativamente a disputa com pai terminal e destino distinto — RED previsto**

   **Fixture:** a pergunta muda e a célula acusadora do caso 3. Acrescentar um pai `disputa_supervisao` já `resolvido`, criado **antes das perguntas**, e manter `chamado_destino=alvo` vivo, diferente do pai. Os helpers F1 permitem essa separação; o teste administrativo existente cobre o outro ramo, com pai vivo. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:69`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:220`)

   **Ato exato:** `d.admin_encerra(admin, 'decisao no teste')`, que chama `d.fechar(admin, resolucao='decisao no teste', motivo=d.FECHOU_ADMIN_ENCERRA)`. (`app/chamados/models.py:1872`, `app/chamados/models.py:1879`)

   **Pela regra:** a porta carimba a muda antes de fechar a disputa; C6 morre e o destino com marco aceso torna-se reconciliável. É o **destino** que deve sair da fila pelo gate. (`app/chamados/models.py:1852`, `app/chamados/juizes.py:1171`, `app/chamados/models.py:1249`, `app/chamados/models.py:623`)

   **Asserção vermelha:** `status_local` relido do **alvo**, fora de `VIVOS`. O receiver consulta somente o pai e retorna porque ele já está encerrado; o materializador posterior exclui a pergunta recém-carimbada. O beco também retorna quando a disputa já está fechada. (`app/chamados/signals.py:32`, `app/chamados/signals.py:33`, `app/chamados/services/materializacao.py:1343`, `app/chamados/services/becos.py:27`)

5. **Validar pergunta já materializada — RED previsto, montagem mais exigente**

   **Fixture:** resposta discordante `19:06`, célula coerente e luz `19:00`, como no caso 1; acrescentar `materializada_em` preenchido e `validada_em=None`. O helper BUG117 já monta materialização, e seu teste demonstra a precedência da resposta sobre ela. (`app/chamados/tests/test_bug117_marco_no_espelho.py:64`, `app/chamados/tests/test_bug117_marco_no_espelho.py:145`)

   Usar pai vivo **diferente do alvo**, com módulo `disputa_supervisao`; acrescentar uma irmã muda, não materializada, de outro dia, ancorada no pai e sem célula. A irmã permanece viva e impede quitar a disputa, sem reter o alvo; esse pai também não pertence aos módulos usados para procurar uma cobrança sobrevivente. (`app/chamados/juizes.py:1207`, `app/chamados/models.py:328`, `app/chamados/models.py:1794`, `app/chamados/catalogo/modulos.py:235`, `app/chamados/models.py:373`)

   **Ato exato:** `validar_pergunta(p.pk, admin, confirmar_implausivel=True)`, executando também os callbacks. O argumento explícito evita que a guarda de plausibilidade impeça alcançar o ramo examinado. (`app/chamados/services/validacao.py:343`, `app/chamados/services/validacao.py:374`)

   **Pela regra:** `validada_em` mata C6 imediatamente; o alvo passa a reconciliável e deve sair da fila pelo gate, mesmo que a disputa continue aberta por outra pergunta. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:327`, `app/chamados/models.py:623`)

   **Asserção vermelha:** `status_local` relido do alvo fora de `VIVOS`. A validação carimba, mas pula `_materializar_uma` porque a pergunta já está materializada; o lote também a exclui. A irmã impede fechar a disputa, e o pai vivo impede o beco. (`app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1297`, `app/chamados/services/materializacao.py:1241`, `app/chamados/services/becos.py:31`)

   O recálculo mensal agendado precisa ser executado no teste; sua chamada não equivale, por si, a uma chamada ao gate do chamado ou à emissão de `EventoCelula`. (`app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1794`, `app/ponto/services/fechamento.py:1808`)

**Quem escreve e quem lê os campos usados nessas verificações**

A tabela delimita os escritores da fixture/atos e os leitores relevantes aos casos; não é um censo de todos os leitores do projeto. **[nao verificado]**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `Chamado.status_local` — campo da asserção RED | Fixture `_ch`; em produção, `reconciliar()` chama `_gravar_estado`, que atribui e salva. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/chamados/models.py:620`, `app/chamados/models.py:222`, `app/chamados/models.py:279`) | Asserção sobre objeto relido; motor do fechamento; fila. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:77`, `app/chamados/models.py:639`, `app/chamados/services/painel.py:187`) |
| `silenciado_ate` — pertinência à fila | Nasce nulo; portas de arquivo atribuem ou limpam. (`app/chamados/models.py:84`, `app/chamados/services/arquivo.py:166`, `app/chamados/services/arquivo.py:201`) | Fila e resgate por resposta. (`app/chamados/services/painel.py:188`, `app/chamados/services/materializacao.py:687`) |
| `contexto_json.data_turno`, `marcos_faltantes`, `modulo_origem` | Helpers de chamado e fixture LP2. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:68`) | Seleção de `batida_ausente`, extração dos marcos e premissa regularizada. (`app/ponto/services/lastro.py:240`, `app/ponto/services/lastro.py:74`, `app/chamados/models.py:1247`) |
| `Pergunta.chamado_destino`, `disputa`; `Disputa.chamado` | Helpers de âncora Q6/F1. (`app/chamados/tests/test_porta_validacao_pergunta.py:184`, `app/chamados/tests/test_porta_validacao_pergunta.py:186`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`) | Coletor de ancoradas e julgamento no pós-estado; receiver do pai. (`app/chamados/models.py:328`, `app/chamados/models.py:344`, `app/chamados/signals.py:32`) |
| `motivo`, `tipo_resposta`, `data_evento`, `celula` da pergunta | Helper Q6 aceita esses dados; PRAM/BUG117 montam motivo, dia e célula. (`app/chamados/tests/test_porta_validacao_pergunta.py:186`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:47`, `app/chamados/tests/test_bug117_marco_no_espelho.py:68`) | Mapeamento do marco, comparação com a luz e cerca da competência. (`app/chamados/juizes.py:886`, `app/chamados/juizes.py:1099`, `app/chamados/models.py:347`) |
| `resposta_colab`, `respondida_em` | Core da resposta; reabertura zera ambos por `update`. (`app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`) | C6 dá precedência à resposta e compara seu texto com a luz. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1113`) |
| `validada_em` | `_carimbar_validacao`; nova resposta e reabertura podem limpar. (`app/chamados/services/validacao.py:131`, `app/chamados/services/disputa_emissao.py:1295`, `app/chamados/services/disputa_emissao.py:1393`) | Primeiro degrau de C6 e filtro das ancoradas. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:327`) |
| `materializada_em`, `via_resolucao` | `_escrever_carimbo`, persistido pelo carimbador de identidade; BUG117 também os prepara como fixture. (`app/chamados/models.py:2556`, `app/chamados/models.py:2644`, `app/chamados/tests/test_bug117_marco_no_espelho.py:71`) | C6; validação e materializadores leem a presença da materialização. (`app/chamados/juizes.py:1171`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1297`) |
| `Celula.colaborador`, `data`, `trabalha`, `dna.marcos` | Fixture `_celula`; porta de célula também grava DNA/trabalho. (`app/chamados/tests/test_bug117_marco_no_espelho.py:55`, `app/ponto/portas/celula.py:650`) | Localização da célula do chamado, premissa e identificação do marco da pergunta. (`app/ponto/services/lastro.py:393`, `app/chamados/models.py:1245`, `app/chamados/juizes.py:893`) |
| `ata.lampadas.{tipo,hora,acesa,luz}` | Fixtures PRAM/LP2; cartório constrói tipo, hora, acendimento e luz. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:39`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:72`, `app/ponto/services/cartorio.py:677`) | `marcos_acesos`, fato do marco e concordância resposta/luz. (`app/ponto/services/lastro.py:174`, `app/chamados/juizes.py:910`, `app/chamados/juizes.py:1111`) |
| `veredito`, `veredito_via` | Fixture `_celula`; produção pela lavra da célula. (`app/chamados/tests/test_bug117_marco_no_espelho.py:60`, `app/ponto/portas/celula.py:812`) | C6 distingue decisão administrativa, dia coerente e dia acusador. (`app/chamados/juizes.py:1147`, `app/chamados/juizes.py:1132`, `app/chamados/juizes.py:1183`) |
| `Disputa.fechada_em` | `Disputa.fechar`. (`app/chamados/models.py:1861`, `app/chamados/models.py:1865`) | Receiver do pai, classificador de beco e cauda da materialização. (`app/chamados/signals.py:28`, `app/chamados/services/becos.py:27`, `app/chamados/services/materializacao.py:1241`) |

Para reproduzir especificamente o escape do evento, a fixture deve começar sem evento pendente do colaborador; há helper de limpeza já existente. Após o ato, o diagnóstico esperado por leitura é alvo ainda vivo, agora em `com_lastro`, e contável em `com_lastro_sem_evento`. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:75`, `app/ponto/services/lastro.py:318`, `app/ponto/services/lastro.py:421`)

**Não chamar `reconciliar()` antes da asserção vermelha:** isso executaria justamente o fechamento cuja reação está faltando. `reconciliaria()` é a consulta adequada para demonstrar que o juiz já permitiria a saída. (`app/chamados/models.py:617`, `app/chamados/models.py:620`, `app/chamados/models.py:623`)

## 06/11 07:45 · sessao 66bbc807ce84 · - · - · origem=Ronald (tmux)

**Resposta HH:MM — montável e candidato RED.** Base concreta: pergunta `orfao_14h`, `tipo_resposta='hora'`, resposta anterior `19:06`, `respondida_em` preenchido, célula com DNA `hf='19:00'`, lâmpada S/19:00 acesa com `luz='19:00'`, veredito `concorde`, via `cartorio`; esse conjunto já existe em `PerguntaRespondidaEMarcoAcesoTest._caso` e seu teste discordante (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`). Falta complementá-lo com chamado `batida_ausente` aberto, cobrando exclusivamente S/19:00, `chamado_destino` da pergunta igual a esse chamado, pai da disputa vivo e ausência de outro chamado apto à reata; o helper LP2 `_caso` fornece chamado+ata e o helper F1 `_disputa`/`_pergunta` fornece a montagem da âncora, mas nenhum desses helpers isoladamente entrega todo o estado (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:69`).

Ato: `responder_pergunta_disputa_core(p, '19:00', colab.usuario, True)`. O core grava resposta e carimbo, só materializa escapes de ausência, acrescenta followup e retorna; HH:MM não chama reconciliação nem emite evento nesse corpo (`app/chamados/services/disputa_emissao.py:1247`, `app/chamados/services/disputa_emissao.py:1282`, `app/chamados/services/disputa_emissao.py:1327`, `app/chamados/services/disputa_emissao.py:1343`, `app/chamados/services/disputa_emissao.py:1359`). C6 passa a morta porque resposta concorda com a luz em dia coerente (`app/chamados/juizes.py:1099`, `app/chamados/juizes.py:1130`, `app/chamados/juizes.py:1158`). O gate, antes retido por pergunta sem reata, passa a permitir o fechamento (`app/chamados/models.py:373`, `app/chamados/models.py:405`, `app/chamados/models.py:623`). O RED esperado é a permanência em `qs_na_fila` depois do ato; execução efetiva desse teste: **[nao verificado]**. O contrato existente considera defeito lastro elegível sem evento pendente (`app/ponto/services/lastro.py:411`).

**Recusa/reabertura — montável e candidato RED no mesmo estado coerente.** Ato público: `recusar_pergunta(p.pk, admin, 'hora incorreta')`; ato interno equivalente relevante: `reabrir_para_correcao(PerguntaDisputa.objects.filter(pk=p.pk))` (`app/chamados/services/validacao.py:494`, `app/chamados/services/validacao.py:545`, `app/chamados/services/validacao.py:547`). Há helper/teste real dessa recusa, `_pergunta` e `test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha`, mas ele precisa ganhar célula, DNA, lâmpada e marco cobrado para este caso (`app/chamados/tests/test_porta_validacao_pergunta.py:175`, `app/chamados/tests/test_porta_validacao_pergunta.py:224`).

A recusa primeiro salva o motivo enquanto a resposta discordante ainda retém; depois `qs.update` apaga resposta e carimbos sem novo `post_save` da pergunta (`app/chamados/services/validacao.py:545`, `app/chamados/services/disputa_emissao.py:1393`, `app/chamados/services/disputa_emissao.py:1413`). Sem resposta e com veredito coerente, C6 morre, permitindo `reconciliaria=True`, mas nenhum fechamento aparece nesse caminho; a mesma asserção de saída da fila é o RED previsto, ainda **[nao verificado]** por execução (`app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1195`, `app/chamados/models.py:623`, `app/chamados/services/validacao.py:548`).

**Controle necessário:** se a célula ainda acusa outro marco (`veredito='furo'`), recusar/apagar a resposta deixa a pergunta viva; nesse cenário não cabe exigir fechamento só porque o marco cobrado está aceso (`app/chamados/juizes.py:1181`, `app/chamados/juizes.py:1183`). Concordar com a luz também não mata a pergunta enquanto o dia acusa (`app/chamados/juizes.py:1132`, `app/chamados/juizes.py:1162`; teste existente `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:62`).

**Não deixar o TestCase esconder callbacks:** o `post_save` de pergunta agenda o beco via `on_commit`; convém executar os callbacks capturados ao redor do ato. Mantendo o pai vivo, o beco continua sem agir; mantendo `silenciado_ate=None`, o resgate também não interfere (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/services/becos.py:31`, `app/chamados/services/materializacao.py:687`).

**Limite do helper LP2:** A5 anuncia retenção por pergunta viva, mas efetivamente usa mock de `reconciliar=False`; não serve como prova de fixture com C6 real (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:137`, `app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:144`).

## 06/11 07:45 · sessao 5408b8736f70 · - · - · origem=Ronald (tmux)

Os dois atos são montáveis; `carimbar_identidade` exige dois casos, pois resposta discordante continua viva por precedência. (`app/chamados/models.py:2625-2632`; `app/chamados/juizes.py:1150-1172`)

1. **Controle: identidade sobre resposta discordante — permanece vivo, sem RED legítimo.** Partindo da pergunta ancorada respondida e discordante, executar `p.carimbar_identidade(PerguntaDisputa.VIA_DUPLICATA, admin)`. O carimbo grava `materializada_em`, autor e via, mas C6 decide pela resposta antes de consultar identidade; consequentemente `pergunta_viva(p)=True`, `ch.reconciliaria()=False` sem sobrevivente, chamado permanece vivo. (`app/chamados/models.py:2633-2644`; `app/chamados/juizes.py:1150-1172`; `app/chamados/models.py:393-414`)

2. **Identidade sobre pergunta muda — RED possível.** Montar célula `veredito='furo', veredito_via='cartorio'`, lâmpada cobrada pelo chamado acesa e outro marco apagado; pergunta muda, sem materialização, ancorada no chamado. Antes, C6 viva pelo veredito acusador; depois de `p.carimbar_identidade(PerguntaDisputa.VIA_DUPLICATA, admin)`, C6 morta pela via. `marcos_acesos=True` regulariza a premissa independentemente do outro furo; portanto `ch.reconciliaria()=True`. O ato somente salva a pergunta e seu callback trata beco, sem reconciliar destino; com pai vivo, nem beco existe. RED: chamado deve sair da fila ao final do ato/callbacks, mas permanece. (`app/chamados/juizes.py:1181-1184`; `app/chamados/juizes.py:1170-1172`; `app/chamados/models.py:1247-1250`; `app/chamados/models.py:2644`; `app/chamados/signals.py:191-202`; `app/chamados/services/becos.py:31`)

3. **Admin encerra disputa, pai terminal e destino distinto — RED possível.** Usar estado anterior de pergunta muda acima; criar pai terminal **antes** das perguntas e apontar a pergunta para outro chamado vivo. Executar `d.admin_encerra(admin, 'decisao no teste')`, equivalente a `d.fechar(admin, resolucao='decisao no teste', motivo=d.FECHOU_ADMIN_ENCERRA)`. A porta carimba a muda antes de fechar disputa, C6 morre e destino fica reconciliável; receiver ignora pai terminal e nunca percorre destinos. Materializador posterior exclui pergunta já carimbada. RED: destino deve sair da fila, mas permanece vivo. (`app/chamados/models.py:1872-1879`; `app/chamados/models.py:1852-1865`; `app/chamados/signals.py:28-42`; `app/chamados/services/materializacao.py:1343-1344`)

**Helpers existentes para composição:** `_Base._ch(modulo, status, dia)`, `_disputa(pai)` e `_pergunta(d, destino, **campos)` permitem criar pai terminal, destino distinto e pergunta ligada à célula; uso conjunto no teste de retenção por destino e no teste administrativo de pai vivo. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59-71`; usos `:82-87`, `:220-226`) O helper `_Base._caso(marcos,lampadas)` planta a célula e o chamado com ata acesa depois dos signals; teste A1 o usa e confirma fecho pelo evento. Ele **não** cria pergunta viva, e A5 apenas simula recusa com mock: não apresentá-lo como fixture pronta de retenção real. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:63-73`; `:97-109`; `:137-150`)

**Cuidados da montagem:** pergunta muda com célula `concorde` já está morta, logo não demonstra desbloqueio por identidade. (`app/chamados/juizes.py:1195-1196`) Executar os callbacks com `captureOnCommitCallbacks(execute=True)` evita atribuir ao produto um efeito da transação do TestCase; o callback é registrado por `transaction.on_commit`. (`app/chamados/signals.py:202`) A execução efetiva dos novos casos e o resultado observado do runner: [nao verificado].

**Campos lidos pelas asserções:**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `status_local` | `_gravar_estado`, atribuição e save. (`app/chamados/models.py:222`; `:279`) | `reconciliaria` → `_decisao_do_fechamento`; fila. (`app/chamados/models.py:623-651`; `app/chamados/services/painel.py:186-188`) |
| `materializada_em`, `materializada_por_id`, `via_resolucao` | `_escrever_carimbo`, persistido por `carimbar_identidade`. (`app/chamados/models.py:2556-2560`; `:2644`) | C6 lê instante/via; materializador lê instante. (`app/chamados/juizes.py:1170-1172`; `app/chamados/services/materializacao.py:1343-1344`) |
| `respondida_em`, `resposta_colab` | Fixture existente `_pergunta`; os atos de identidade/admin não alteram resposta. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:629-637`; `app/chamados/models.py:2640-2644`) | C6 lê antes da identidade. (`app/chamados/juizes.py:1150-1162`) |
| Âncora `chamado_destino`, `disputa.chamado`, `validada_em` | Helpers `_pergunta` e `_disputa`. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:65-71`) | `_ancoradas` e `_vivas_entre`. (`app/chamados/models.py:327-351`) |
| `contexto_json.marcos_faltantes`, `ata.lampadas` | Helper `_caso`. (`app/ponto/tests/test_lastro_parte2_fecho_no_evento.py:68-72`) | `_marcos_cobrados`, `_lampada`, premissa. (`app/ponto/services/lastro.py:62-94`; `app/chamados/models.py:1247-1250`) |
| `veredito`, `veredito_via` | Produção: `lavrar_veredito`; fixture pode usar padrão de `update` já existente. (`app/ponto/portas/celula.py:751`; `:812-815`; `app/chamados/tests/test_o261e_f1_porta_encerramento.py:181-186`) | C6. (`app/chamados/juizes.py:1147-1149`; `:1181-1184`) |
| `Disputa.fechada_em` | `Disputa.fechar`. (`app/chamados/models.py:1861-1865`) | Receiver do pai e classificador de beco. (`app/chamados/signals.py:28`; `app/chamados/services/becos.py:27-28`) |

## 06/11 07:44 · sessao 7bdef93e5b55 · - · - · origem=Ronald (tmux)

`validar_pergunta` com pergunta já materializada é montável; o RED exige impedir que o fechamento da disputa reconcilie o pai incidentalmente. (`app/chamados/services/validacao.py:419`, `app/chamados/services/materializacao.py:1241`, `app/chamados/signals.py:42`)

- **Helpers existentes:** `_Base._celula(lampadas, veredito='furo')`, `_Base._pergunta(motivo, cel, materializada=True)` montam célula com DNA/ata e pergunta materializada; usados conjuntamente no teste da precedência da resposta. (`app/chamados/tests/test_bug117_marco_no_espelho.py:55`, `app/chamados/tests/test_bug117_marco_no_espelho.py:64`, `app/chamados/tests/test_bug117_marco_no_espelho.py:145`)
- **Helper para pergunta respondida discordante e dia coerente:** `PerguntaRespondidaEMarcoAcesoTest._caso('19:06')` produz pergunta `orfao_14h`, luz `19:00`, veredito `concorde`; o teste confirma C6 viva aos seis minutos. Ele ainda não cria chamado/âncora. (`app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:36`, `app/chamados/tests/test_pergunta_respondida_e_marco_aceso.py:59`)
- **Helpers para âncora:** `_Base._ch`, `_disputa(pai=...)`, `_pergunta(d, destino=...)` e teste que confirma recusa real de `reconciliar` por pergunta viva, sem sobrevivente. (`app/chamados/tests/test_o261e_f1_porta_encerramento.py:59`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:65`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:69`, `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82`)

**Caso concreto:** alvo `batida_ausente` aberto, cobrando exclusivamente `S 19:00`, célula com DNA `hf=19:00`, lâmpada acesa com luz `19:00`, veredito `concorde` via cartório; pergunta `orfao_14h` ancorada no alvo, resposta `19:06`, `respondida_em` preenchido, `materializada_em` preenchido e `validada_em=None`. C6 permanece viva pela discordância antes de ler o carimbo. Acrescentar pai vivo da disputa e uma segunda pergunta viva de **outro dia**, ancorada no pai, para não reter o alvo nem oferecer sobrevivente do mesmo dia. (`app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1162`, `app/chamados/models.py:373`)

**Ato:** `validar_pergunta(p.pk, admin, confirmar_implausivel=True)`, dentro de `captureOnCommitCallbacks(execute=True)`; esse mecanismo já é usado para testar os eventos da pergunta. (`app/chamados/services/validacao.py:343`, `app/chamados/tests/test_varredura_becos_evento_pergunta.py:55`)

**Antes:** confirmar `pergunta_viva(p)=True`, `alvo.reconciliaria()=False` e alvo na fila; ausência de sobrevivente é necessária para que não ocorra reata legítima. (`app/chamados/models.py:357`, `app/chamados/models.py:375`, `app/chamados/models.py:405`)

**Depois pela regra:** C6 morta por `validada_em`; o chamado agora é reconciliável, e deveria sair da fila pelo gate `reconciliar()`, não por escrita direta de estado. (`app/chamados/juizes.py:1136`, `app/chamados/models.py:607`, `app/chamados/models.py:623`)

**Asserção RED proposta:** após ato e callbacks, `assertFalse(qs_fila(_base=ChamadoColaborador.objects.filter(pk=alvo.pk)).exists())`, ou equivalente `assertNotIn(status_recarregado, VIVOS)`. O hash mantém o alvo aberto: carimba validação, pula `_materializar_uma` por já materializada, exclui a pergunta do lote, e a irmã impede fechar a disputa; beco tampouco atua com pai vivo. (`app/chamados/services/validacao.py:396`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1294`, `app/chamados/services/materializacao.py:1241`, `app/chamados/services/becos.py:31`, `app/chamados/services/painel.py:186`)

**Não suprimir callbacks:** `recalcular_por_evento` agenda o cálculo mensal em `on_commit`; o corpo chama `recalcular_fechamento_mes`, sem chamada explícita a `reconciliar` ou emissão de `EventoCelula` nesse trecho. (`app/ponto/services/fechamento.py:1782`, `app/ponto/services/fechamento.py:1791`, `app/ponto/services/fechamento.py:1808`)

**Escritores/leitores principais:** validação escreve `validada_em/por` em `_carimbar_validacao`; C6 lê `validada_em`, e o coletor de âncoras filtra esse campo. O carimbo `materializada_em/por/via` é atribuído por `_escrever_carimbo`; validação/lote leem `materializada_em`. `status_local` é escrito por `_gravar_estado`, através de `reconciliar`, e lido pela fila. (`app/chamados/services/validacao.py:127`, `app/chamados/juizes.py:1136`, `app/chamados/models.py:327`, `app/chamados/models.py:2556`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:1297`, `app/chamados/models.py:222`, `app/chamados/models.py:620`, `app/chamados/services/painel.py:187`)

Execução deste caso no hash: **[nao verificado]**.

_2139 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
