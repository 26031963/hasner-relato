# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 17:59** · publicado em **06/11/2026 18:04** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 17:59 · sessao 837afbd0fb35 · E-SEM-VIVA-B · verificador · origem=Code

**As curas de produção estão corretas para os alvos apontados; a certificação continua parcial.** O filtro exige `furo_espelho`, o positivo corrente cobre a competência antes ausente e o corpo da C5 continua lendo a lavra, sem consultar demissão, admissão ou retentor. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:185`, `app/escala/services/cadastro_realidade.py:189`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:217`.

Execução da suíte Django, sobrevivência dos mutantes à suíte integral e ocorrência dos cenários em produção: **[nao verificado]**. [nao verificado]

1. **MÉDIA — falta uma prova da fronteira inferior, inclusive para “ativo nunca-bateu igual”.**

   O mutante plausível que remove `data__gte=janelas[0][0]` deixa passar dias anteriores à janela. Por inspeção, os testes novos não o distinguem: suas células começam depois dessa fronteira, inclusive no corte 10. O teste do vigia com `hoje=01/09` continua excluindo 01/10 pelo limite **superior**. `app/escala/services/cadastro_realidade.py:176`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:33`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:250`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`, `app/folha/tests/test_vigia_retido_calado.py:133`.

   **Como provar:** ativo, corte 21, `hoje=25/10`, células `nunca_bateu` em 20/09 e 21/09; exigir somente 21/09. Remover o filtro inclui ambas; trocar `gte` por `gt` exclui ambas. Esse cenário distingue duas regressões na regra dos ativos. `app/ponto/janelas.py:10`, `app/ponto/janelas.py:66`, `app/escala/services/cadastro_realidade.py:175`.

2. **BAIXA — a cura do selo ORM é correta, mas não completa.**

   O detector agora encontra `filter(data_demissao__isnull=False)`. Ainda ignora `filter(**{'data_demissao__isnull': False})`, porque a expansão tem `k.arg=None`, descartado pelo próprio filtro do AST. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:327`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:357`.

   **Como provar:** inserir essa expansão na consulta de desligados e executar `_veredito`: o campo fica fora de `campos`, satisfazendo indevidamente o selo. A sonda AST em memória confirmou isso; não significa sobrevivência comportamental, pois o novo teste de cadastro torto combate essa alteração específica. `app/escala/services/cadastro_realidade.py:179`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:335`.

   A chamada descartada à porta também continua suficiente para a exigência positiva do AST. Os testes de ausência de retrato e divergência lavra/vivo combatem a recomposição simples, mas o selo não certifica fluxo de dados. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:154`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:317`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:332`.

3. **BAIXA — descarte 1 defensável como CADASTRO; documentação ainda contraditória.**

   Com fechamento preexistente, retirar `data_demissao` não impede a nova lavra: `_K2` lê os fechamentos existentes, rescisão responde falso para data nula e o retentor não recorta o fim. A C5 acompanha essa resposta. O teste novo reproduz corretamente o histórico e exige igualdade com a autoridade. Isso sustenta retirar o caso da correção local da C5, sem criar predicado paralelo. `app/folha/export.py:775`, `app/folha/export.py:60`, `app/folha/export.py:1200`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/services/cadastro_realidade.py:189`.

   Entretanto, “sem `data_demissao` = fora” permanece escrito antes da ressalva sobre fechamento antigo. O primeiro enunciado precisa da condição de ausência de fechamento preexistente. `app/escala/services/cadastro_realidade.py:148`, `app/escala/services/cadastro_realidade.py:151`.

   **Como provar:** `_fechar()`, apagar a demissão, `_lavrar()`, consultar C5 — exatamente o histórico agora exercitado pelo teste. Uma obrigação adicional de excluir data nula pelo aval literal fornecido é **[nao verificado]**; o código atual não faz essa exclusão. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:228`, `app/escala/services/cadastro_realidade.py:179`.

4. **MÉDIA — descarte 3 sustenta a separação de escopo, mas a perda de competência permanece.**

   O vigia declara unidade **pessoa** e transforma C5 em conjunto de IDs. Portanto, a triagem pode tratar isso como granularidade preexistente; não pode concluir que o roteamento respeita a competência de cada linha. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

   **Como provar:** hoje 25/10, corte 21, desligado com demissão 25/11, fechamentos 10 e 11, `nunca_bateu` em 05/10 e vínculo cobrindo 21–24/10 sem células. A anterior pode ser retida pelo `nunca_bateu`; a corrente, por `sem_celula`, sem porta e com fio `ninguem`. C5 contém somente 05/10, mas o vigia roteia ambas as linhas pelo mesmo ID. O aval por dias/competência não fica demonstrado por esse contador. `app/folha/export.py:1231`, `app/folha/export.py:1233`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:86`, `app/folha/retrato_folha.py:1186`, `app/escala/services/cadastro_realidade.py:175`, `app/folha/vigia_retido_calado.py:48`.

5. **MÉDIA — descarte 4 correto para empresa sem lavra na tela; insuficiente para retrato histórico no vigia.**

   A tela e o lavrador usam `empresas_visiveis`, sustentando o descarte no caminho apresentado. Mas “sem lavra não tem folha” não vale para a autoridade viva: `_K2` consulta fechamentos diretamente. `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/export.py:775`.

   **Como provar o caminho residual:** lavrar uma empresa visível com C5 positiva e depois alterar somente `Empresa.ativa=False`. O lavrador deixa de atualizá-la; `censo([id])` ainda lê seu retrato histórico, C5 ainda aceita empresa/retrato e o vigia pode declarar `roteados`. A tela já exclui essa empresa. Logo, pode haver “roteado para a lista” sem presença na lista acessível. `app/colaboradores/queries.py:85`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/vigia_retido_calado.py:35`, `app/folha/vigia_retido_calado.py:38`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:48`, `app/escala/views.py:487`.

6. **SEM DEFEITO NOVO IDENTIFICADO — filtro de motivo e sua origem.**

   O caminho normal atual sustenta a cura: somente IDs classificados como `furo_espelho` alimentam os dias de `dias_que_seguram`; férias terminam em outro ramo e fornecem outra lista. Não identifiquei, nesse caminho, outro motivo com datas dessa fonte nem furo com datas de férias. `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`, `app/folha/export.py:876`, `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1175`.

   | Quem escreve/lê | Caminho |
   |---|---|
   | Decide `motivo='furo_espelho'` | `classificar_export`. `app/folha/export.py:962`. |
   | Produz suas datas | Retentor → `dias_que_seguram` → conversão ISO em `fora_da_folha`. `app/folha/fora_da_folha.py:98`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:192`. |
   | Monta e persiste o JSON | `montar_linhas` copia motivo/datas; command grava. `app/folha/retrato_folha.py:1181`, `app/folha/retrato_folha.py:1183`, `app/folha/management/commands/lavrar_retrato_folha.py:74`. |
   | Compara literalmente furo no retrato | Nova C5; derivações de fio e tipo. `app/escala/services/cadastro_realidade.py:189`, `app/folha/retrato_folha.py:366`, `app/folha/retrato_folha.py:380`. |
   | Outros leitores de motivo | Balde, saldo, ação/drawer, apresentação e recorte temporal. `app/folha/retrato_folha.py:93`, `app/folha/retrato_folha.py:501`, `app/folha/retrato_folha.py:1062`, `app/folha/views_retrato.py:285`, `app/folha/retrato_folha.py:1240`. |
   | Vigia | Lê indiretamente pelo balde e fio. `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:47`. |

   Essa garantia não certifica qualquer JSON histórico: o modelo prevê retratos sem `datas` e declara `linhas` como JSON sem esquema de conteúdo. Conteúdo divergente em produção: **[nao verificado]**. `app/folha/models.py:332`, `app/folha/models.py:348`.

7. **SEM RELÓGIO SOLTO IDENTIFICADO nos testes novos/alterados; curas específicas com mordida adequada.** [nao verificado]

   Os comportamentais estão congelados por classe; os testes AST analisam texto. O positivo corrente distingue `janelas[:1]`, e o negativo de férias distingue a retirada do filtro de motivo. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:90`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:243`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:256`, `app/folha/tests/test_vigia_retido_calado.py:69`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:217`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:210`.

   O teste do command prova repasse de `hoje`, mas não unicidade da leitura do relógio: uma segunda chamada a `localdate()` não violaria suas asserções. O command atual contém uma leitura. `app/folha/tests/test_vigia_retido_calado.py:140`, `app/folha/tests/test_vigia_retido_calado.py:143`, `app/folha/management/commands/vigiar_retido_calado.py:31`.

   Não identifiquei push novo nos trechos alterados: C5 monta retorno/link; vigia conta; command imprime/alarma. `app/escala/services/cadastro_realidade.py:201`, `app/escala/services/cadastro_realidade.py:657`, `app/folha/vigia_retido_calado.py:44`, `app/folha/management/commands/vigiar_retido_calado.py:31`.

## 06/11 17:59 · sessao a3b5c798fdcd · PAUTA-08 · verificador · origem=Code

1. **Sim, fecha o achado específico da volta 1.** Toda criação de cabeça neste comando passa pelo `atomic`, pelo `select_for_update` do colaborador e pela comparação do hash antes de chamar o escritor. Não identifiquei caminho que grave uma cabeça nova sem essa conferência no mesmo `atomic`; o ramo `dry` só imprime, e o ramo de pauta existente não cria outra. ([comando:182](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:182), [comando:189](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189), [comando:195](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:195), [comando:198](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198), [comando:203](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:203)).

   **Isso prova a conferência, não o congelamento do fechamento até o commit.** O lock é sobre `Colaborador`; o hash lê `FechamentoMensal` sem bloqueá-lo explicitamente. A comparação final continua podendo lançar erro depois das gravações, mas essas cabeças agora tiveram a conferência anterior. ([comando:192](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:192), [regen_exportada.py:71](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:71), [comando:224](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224)).

2. **Banco e arquivo têm comportamentos diferentes.** O novo `raise` ocorre antes de criar qualquer pauta daquele colaborador e antes de acrescentar seus PKs à reversão. Portanto, nesse erro específico, não há pauta nova dele para desfazer nem entrada dele em `rev['pautas']`; seu plano já pode constar no JSON inicial. ([comando:163](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:163), [comando:166](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:166), [comando:195](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:195), [comando:212](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:212)).

   Os colaboradores anteriores têm transações próprias já encerradas e seus PKs já registrados: **o novo `raise` preserva essas pautas e mantém a lista de reversão coerente com elas**, supondo gravações e commits anteriores bem-sucedidos. ([comando:169](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:169), [comando:189](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189), [comando:212](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:212)).

   **O `atomic` não desfaz o arquivo.** Um erro após `_grava`, inclusive falha no commit, pode deixar no JSON PKs cujas pautas voltaram no banco. O próprio comentário reconhece isso; `_reverter` pula PK inexistente. Logo, a coerência nesse novo `raise` decorre da posição dele, não de rollback do JSON. ([comando:210](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:210), [comando:270](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:270), [comando:298](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:298)).

3. **O teste morde a ausência da conferência por colaborador, mas não fixa sua posição transacional.** O mock altera o retorno a partir da segunda chamada; as asserções exigem erro identificando o colaborador e ausência de suas pautas. Sem a conferência nova, sobra a comparação final: erro genérico e pautas já escritas, contrariando essas asserções. ([teste:204](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:211](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:211), [comando:224](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224)).

   Por análise estática, dois mutantes passariam pelas asserções desse teste: [nao verificado]

   - **Conferir depois de escrever, ainda dentro do `atomic`:** o erro reverte as pautas e satisfaz a ausência exigida. Se a conferência vier depois de `_grava`, pode deixar PKs mortos no JSON, que esse teste não inspeciona. ([comando:203](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:203), [comando:213](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:213), [teste:208](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:208)).
   - **Conferir antes de escrever, fora do `atomic`:** o mock provoca o erro antes das criações; o teste não observa a localização da transação nem o lock. ([teste:204](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:204), [teste:209](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:209)).

   Execução do teste e dos mutantes: **[nao verificado]**. [nao verificado]

4. **A falsidade geral apontada anteriormente foi corrigida.** A linha agora restringe a ausência ao comparador **genérico** e reconhece passivo validado e HE autorizada. Esses fluxos efetivamente escrevem pautas com os dois lados. ([LEIS.md:177](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/docs/LEIS.md:177), [passivo_validado.py:192](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/chamados/services/passivo_validado.py:192), [passivo_validado.py:229](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/chamados/services/passivo_validado.py:229), [autorizacao_he_periodo.py:184](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/autorizacao_he_periodo.py:184), [autorizacao_he_periodo.py:244](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/autorizacao_he_periodo.py:244)).

   Resta **ambiguidade** em “caminho AUTOMATICO segue sem codigo”: no escopo genérico da linha, não reproduz a falsidade anterior; como negação global, seria falsa, pois a autorização de HE chama automaticamente o emissor quando exportada e com autorizações. ([LEIS.md:177](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/docs/LEIS.md:177), [autorizacao_he_periodo.py:299](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/autorizacao_he_periodo.py:299)).

5. **Leitores e escritores no caminho tocado:**

   | Objeto | Quem produz/escreve | Quem lê/consome |
   |---|---|---|
   | `hash_do_gravado` | É função, não coluna: calcula MD5 truncado sobre campos do `FechamentoMensal`. ([regen_exportada.py:62](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:62)) | O comando guarda o inicial, compara por colaborador e compara no fim. ([comando:120](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:195](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:195), [comando:224](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:224)) |
   | Hash na reversão | O comando grava `hash_gravado` no plano JSON. ([comando:163](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:163)) | `_reverter` carrega o arquivo, mas usa marca, schema e PKs; não valida o hash guardado. ([comando:258](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:258)) |
   | `FechamentoMensal` no PAUTA-08 | O comando solicita recálculo somente leitura; recebe campos de objeto destacado. ([comando:135](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:1112](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:1112)) | Leem a linha: `hash_do_gravado`, `valores_do_gravado` para PAGO e o recálculo para semear o objeto destacado. ([regen_exportada.py:71](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:71), [regen_exportada.py:81](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:81), [fechamento.py:881](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:881)) |
   | `FechamentoMensal` no ramo escritor relacionado | O mesmo serviço usa `get_or_create`, `save` ou `update`; a porta de retificação chama esse ramo. ([fechamento.py:889](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:889), [fechamento.py:1128](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:1128), [fechamento.py:1144](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/services/fechamento.py:1144), [regen_exportada.py:179](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:179)) | A porta lê hash/valores antes e depois, envia hashes à auditoria e retorna `mudou`; o passivo consome `mudou` para decidir a pauta. ([regen_exportada.py:116](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:116), [regen_exportada.py:189](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:189), [regen_exportada.py:206](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:206), [regen_exportada.py:218](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/ponto/portas/regen_exportada.py:218), [passivo_validado.py:442](/home/ronald/codex_ws/fa493f80d4624801b03964a2f58d18ec56984656/app/chamados/services/passivo_validado.py:442)) |

Ocorrência desses cenários em produção: **[nao verificado]**.

## 06/11 17:58 · sessao bd1f88673f15 · - · - · origem=Ronald (tmux)

1. **MÉDIA — há mutante plausível sobrevivente na fronteira inferior da janela.** Trocar `data__gte=janelas[0][0]` por `data__gt=janelas[0][0]`, ou apagar esse filtro, preserva por inspeção os cenários novos: as células da fixture são posteriores ao início da anterior, inclusive no corte 10. Nenhum caso insere um `nunca_bateu` em 21/09 ou 20/09 para corte 21. `app/escala/services/cadastro_realidade.py:176`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:33`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:36`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:250`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:264`.

   O teste de `hoje=01/09` do vigia também não distingue a remoção: a célula de 01/10 continua excluída pelo limite superior de 31/08. O teste de `hoje=25/11` usa desligado, cujos dias ainda dependem dos retratos selecionados; não prova limite inferior dos ativos. `app/folha/tests/test_vigia_retido_calado.py:79`; `app/folha/tests/test_vigia_retido_calado.py:133`; `app/escala/services/cadastro_realidade.py:185`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:240`.

   **Como provar:** ativo com dias 20/09 e 21/09, `hoje=25/10`, corte 21; exigir somente 21/09. `janela_anterior` termina em 20/10 e inicia em 21/09 por composição das funções de janela. `app/ponto/janelas.py:10`; `app/ponto/janelas.py:60`; `app/ponto/janelas.py:66`. Suíte Django contra mutante: **[nao verificado]**. O mutante que apaga o filtro passou o selo AST executado em memória; esse selo não verifica os filtros de datas. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:330`.

2. **BAIXA — cura de kwargs correta, cobertura AST ainda parcial.** O delta detecta `filter(data_demissao__isnull=False)`, mas `filter(**{'data_demissao__isnull': False})` continua invisível: no AST, a expansão tem `k.arg=None`, que o detector descarta. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:327`.

   **Como provar:** substituir em memória a consulta de desligados por `filter(pk__in=..., situacao='desligado', **{'data_demissao__isnull': False})`; `_veredito` retorna `campos=set()` e o selo passa. A versão com kwarg direto retorna `{'data_demissao'}` e falha no selo. Executei essas sondas usando a classe AST extraída, com `unittest.TestCase`, sem Django nem escrita. O ponto de inserção é `app/escala/services/cadastro_realidade.py:179`; a exigência que passa indevidamente é `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:335`.

   Esse mutante comportamental específico seria combatido pelo novo teste de cadastro torto, pois o ID excluído de `segura` passa pelo ramo dos demais colaboradores e recebe todos os dias, inclusive os posteriores à rescisão. Portanto, não alego sobrevivência à suíte inteira. `app/escala/services/cadastro_realidade.py:192`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`. Execução Django: **[nao verificado]**.

3. **Curas dos testes: adequadas aos dois mutantes apontados na rodada anterior.** O novo positivo corrente exige `DIAS_A`, incluindo 21–23/10, e mata a seleção `janelas[:1]`; o novo negativo de férias mata a retirada do filtro `motivo == 'furo_espelho'`. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:217`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:221`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:210`; `app/escala/services/cadastro_realidade.py:189`. Execução Django desses casos: **[nao verificado]**.

4. **Relógio: não encontrei dependência solta nos testes novos/alterados.** Os comportamentais ESEM estão congelados por classe; os três testes alterados/adicionados do vigia pertencem à classe congelada. Os testes AST somente analisam texto. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:90`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:243`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:256`; `app/folha/tests/test_vigia_retido_calado.py:69`; `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:293`.

   O teste do command prova o repasse de `hoje`, mas não conta chamadas ao relógio: adicionar outra leitura de `timezone.localdate()` ao command não o faria falhar, pois verifica apenas chamadas ao censo e o argumento final. Isso é limite da asserção, não defeito observado no command atual. `app/folha/tests/test_vigia_retido_calado.py:140`; `app/folha/tests/test_vigia_retido_calado.py:142`; `app/folha/management/commands/vigiar_retido_calado.py:31`.

## 06/11 17:57 · sessao d31f241278af · - · - · origem=Ronald (tmux)

1. **Descarte 1 — sustenta-se como decisão de seguir o juiz; não como demonstração de exclusão de desligado sem data.** O universo não cria novo fechamento para desligado sem `data_demissao`, mas `_K2` consulta fechamentos existentes, `sai_pela_rescisao` devolve falso para data nula e o retentor não recorta o fim. Assim, um fechamento anterior permite nova lavra `furo_espelho`, aceita pela C5. O teste novo reproduz corretamente esse histórico e exige igualdade com a autoridade. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430`, `app/folha/export.py:775`, `app/folha/export.py:60`, `app/folha/export.py:1200`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:227`, `app/escala/services/cadastro_realidade.py:189`.

   **BAIXA — documentação contraditória:** a docstring ainda diz “sem `data_demissao` [...] = fora” e, duas linhas depois, admite que fechamento antigo segue o juiz. O primeiro enunciado precisa da condição “sem fechamento preexistente”. `app/escala/services/cadastro_realidade.py:148`, `app/escala/services/cadastro_realidade.py:151`.

   O aval literal não especifica expressamente o cadastro com data nula; inferir dele uma obrigação inequívoca de excluir esse caso é **[nao verificado]**. A solução não acrescentou predicado próprio de demissão: só consulta `situacao` e lê a lavra. `app/escala/services/cadastro_realidade.py:179`, `app/escala/services/cadastro_realidade.py:185`.

2. **Descarte 3 — sustenta-se para retirar da cura específica da C5; não elimina a divergência entre competências.** O vigia declara explicitamente unidade **PESSOA** e roteia pela presença do ID na lista. A C5 agora inclui desligados; portanto, o mesmo comportamento já existente para ativos passa a alcançá-los. `app/folha/vigia_retido_calado.py:12`, `app/folha/vigia_retido_calado.py:20`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`, `app/escala/services/cadastro_realidade.py:189`.

   **Caminho concreto:** hoje 25/10, corte 21, desligado com demissão 25/11, FM de competências 10 e 11, uma célula `nunca_bateu` em 05/10 e vínculo cobrindo 21–24/10 sem células. O juiz retém a anterior por `espelho_nunca_bateu` e a corrente por `sem_celula`; sem célula não há porta, produzindo grupo D e fio `ninguem`. A C5 contém apenas 05/10, mas o vigia roteia ambas as linhas porque descarta as datas e usa somente ID. `app/folha/export.py:960`, `app/folha/export.py:1227`, `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:65`, `app/folha/fora_da_folha.py:86`, `app/folha/fora_da_folha.py:175`, `app/folha/retrato_folha.py:1186`, `app/escala/services/cadastro_realidade.py:175`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:48`.

   A formulação estrita por competência não é garantida pelo vigia; ocorrência em produção e execução desse cenário: **[nao verificado]**. O aval fornecido limita a inclusão C5, enquanto o vigia documenta sua própria granularidade por pessoa. `app/escala/services/cadastro_realidade.py:138`, `app/folha/vigia_retido_calado.py:12`.

3. **Descarte 4 — sustenta-se no caminho normal da tela; a justificativa “sem lavra não tem folha” é ampla demais.** A tela passa somente `empresas_visiveis`, e o lavrador aplica a mesma seleção. Contudo, a autoridade viva lê FM diretamente, sem exigir empresa visível ou retrato; logo, ausência de lavra não prova ausência de retenção viva. `app/escala/views.py:487`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:51`, `app/folha/export.py:775`.

   **MÉDIA — caminho residual do vigia:** lavrar uma empresa visível e depois torná-la inativa. O lavrador deixa de atualizá-la; `censo([id])` continua lendo o retrato antigo, C5 aceita empresa e retrato, e o vigia pode marcar `roteados`, embora a tela exclua a empresa inteira. Esse problema já existia para ativos e agora alcança desligados aceitos pela lavra. `app/colaboradores/queries.py:85`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/vigia_retido_calado.py:33`, `app/folha/vigia_retido_calado.py:38`, `app/escala/services/cadastro_realidade.py:171`, `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:48`, `app/escala/views.py:487`.

   **Como provar:** conservar célula e retrato positivos, alterar somente `Empresa.ativa=False`, comparar `censo([id])` com o conjunto de empresas passado pela view à lista. Execução e ocorrência em produção: **[nao verificado]**. `app/colaboradores/queries.py:85`, `app/folha/vigia_retido_calado.py:35`, `app/escala/views.py:487`.

4. **Sem novo caminho de push identificado nesses trechos:** C5 monta assinaturas e sua ação é um link ao colaborador; o vigia conta, e o command imprime ou alarma. Os portões de emissão do cartório continuam excluindo desligados. `app/escala/services/cadastro_realidade.py:201`, `app/escala/services/cadastro_realidade.py:657`, `app/folha/vigia_retido_calado.py:44`, `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/ponto/services/cartorio.py:1696`, `app/ponto/services/cartorio.py:1762`.

## 06/11 17:57 · sessao e2e596e173d3 · - · - · origem=Ronald (tmux)

No caminho tocado, `hash_do_gravado` é **função que lê**, não campo persistido: consulta `FechamentoMensal`, serializa os campos selecionados e retorna MD5 truncado; exclusões em `_FORA_DO_HASH` (`app/ponto/portas/regen_exportada.py:37`, `:56`, `:62`, `:71`, `:74`).

| Objeto | Quem escreve/produz | Quem lê/consome |
|---|---|---|
| Hash no comando | `hash_do_gravado` calcula; `_medir` guarda o inicial em `hashes` e `linhas` (`app/ponto/portas/regen_exportada.py:70`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`, `:140`, `:148`). | Comparação por colab dentro do atomic (`comando:189`, `:195`); comparação final de todo universo (`comando:224`). |
| Hash no JSON | `_escrever` monta `plano[pk].hash_gravado`; `_grava` serializa e substitui o arquivo (`comando:163`, `:164`, `:166`, `:298`, `:299`, `:300`). | `_reverter` carrega o JSON, mas usa marca, schema e lista `pautas`; não confere o hash do plano (`comando:258`, `:262`, `:265`, `:269`, `:279`). |
| Hash na porta de retificação | `regenerar_em_exportada` calcula antes/depois e entrega os hashes à auditoria e ao retorno (`app/ponto/portas/regen_exportada.py:116`, `:189`, `:199`, `:206`, `:208`, `:217`). | A própria porta compara os hashes para `mudou` (`app/ponto/portas/regen_exportada.py:218`); `passivo_validado` usa `regen['mudou']` para decidir a pauta (`app/chamados/services/passivo_validado.py:438`, `:442`, `:443`). |
| `FechamentoMensal` no PAUTA-08 | O comando pede `somente_leitura=True`; o serviço instancia objeto destacado e devolve campos, sem entrar nos ramos de persistência (`comando:135`; `app/ponto/services/fechamento.py:867`, `:881`, `:882`, `:1112`, `:1119`). | `hash_do_gravado` lê a linha para o hash; `valores_do_gravado` lê o PAGO; o recálculo lê a linha para semear o objeto destacado (`app/ponto/portas/regen_exportada.py:71`, `:81`, `:84`; `app/ponto/services/fechamento.py:881`, `:883`). |
| `FechamentoMensal` no ramo de escrita do mesmo serviço | `recalcular_fechamento_mes` usa `get_or_create`, depois `save()` para aberto ou `update()` para o outro status (`app/ponto/services/fechamento.py:889`, `:1121`, `:1128`, `:1144`). A porta `regenerar_em_exportada` chama esse ramo sem `somente_leitura` (`app/ponto/portas/regen_exportada.py:179`). | Depois da retificação, a porta lê novamente hash e valores (`app/ponto/portas/regen_exportada.py:189`, `:190`). |

**Limite do lock:** PAUTA-08 bloqueia `Colaborador`, enquanto o hash consulta `FechamentoMensal` por SELECT comum (`comando:192`; `app/ponto/portas/regen_exportada.py:71`). O recálculo mostrado percorre colaboradores com `select_related` e escreve FM por `save/update`, sem adquirir explicitamente esse mesmo lock da pessoa (`app/ponto/services/fechamento.py:610`, `:1128`, `:1144`). Portanto, o delta prova a conferência no atomic, mas não prova congelamento do FM durante todo o atomic por esse lock (`comando:189`, `:192`, `:195`; `app/ponto/portas/regen_exportada.py:71`).

Outros escritores disponíveis no mesmo módulo, fora da execução normal do comando: aprovação/reabertura (`app/ponto/services/fechamento.py:1307`, `:1316`, `:1324`, `:1330`, `:1338`), restauração de campos (`:1376`), reabertura de período (`:1433`), invalidação de previsto/derivação (`:1594`, `:1597`, `:1636`, `:1640`).

## 06/11 17:57 · sessao 3074e80c895b · - · - · origem=Ronald (tmux)

Rastreio estático concluído; não executei Django nem consultei banco/log. **[nao verificado]** para estado de produção e acessos dinâmicos fora do código identificado. [nao verificado]

- **Quem decide e escreve:** `classificar_export` atribui `status='fora'`, `motivo='furo_espelho'` quando o retentor acusa; `montar_linhas` copia esse motivo para o JSON; o command persiste com `update_or_create`. `app/folha/export.py:960`, `app/folha/retrato_folha.py:1165`, `app/folha/retrato_folha.py:1181`, `app/folha/management/commands/lavrar_retrato_folha.py:74`.

- **Origem das datas:** `dias_que_seguram` inclui no mapa de dias somente IDs classificados como `furo_espelho`, chama `_dias_que_retem` para eles; esta delega a `motivos_retencao_celula(com_dias=True)`; `fora_da_folha` converte `data` para string ISO; `montar_linhas` copia a lista. `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:98`, `app/folha/fora_da_folha.py:192`, `app/folha/retrato_folha.py:1171`.

- **Outra forma de datas:** o ramo `ferias_com_batida` recebe `dias_ferias_com_batida`; o lavrador usa essa lista apenas quando não encontrou `dias` em `colabs`. Essa lista não vem de `dias_que_seguram`. `app/folha/export.py:868`, `app/folha/export.py:875`, `app/folha/retrato_folha.py:1172`, `app/folha/retrato_folha.py:1175`.

- **Resposta à equivalência:** no caminho normal atual do escritor, em estado estável, não identifiquei outro motivo com datas originadas em `dias_que_seguram`, nem `furo_espelho` com datas de férias: `_por_colab` só contém furo e a classificação de férias termina por `continue` antes de furo. `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:171`, `app/folha/retrato_folha.py:1158`, `app/folha/export.py:876`, `app/folha/export.py:960`.

- **Limite dessa equivalência:** `linhas` é JSON sem validação de esquema declarada; retratos antigos sem `datas` são explicitamente previstos, e a porta somente lê e recorta o conteúdo existente. Portanto, a garantia não vale para qualquer JSON persistido arbitrariamente nem certifica dados legados. `app/folha/models.py:348`, `app/folha/models.py:332`, `app/folha/retrato_folha.py:110`. A ocorrência de JSON divergente é **[nao verificado]**.

**Leitores do motivo gravado:**

| Leitor | Uso/evidência |
|---|---|
| C5 | Compara literalmente `motivo == 'furo_espelho'` antes de unir as datas. `app/escala/services/cadastro_realidade.py:189`. |
| `_balde` | Consulta motivo para retirar os módulos próprios; compõe a porta, contadores e filtros. `app/folha/retrato_folha.py:93`, `app/folha/retrato_folha.py:111`, `app/folha/retrato_folha.py:120`. |
| `_fio_cru` / `fio_da_linha` | Distingue literalmente furo dos demais motivos; leitura de legado quando não existe fio válido gravado. `app/folha/retrato_folha.py:366`, `app/folha/retrato_folha.py:441`. |
| `_tipo_cru` / `tipo_da_linha` | Distingue literalmente furo para escolher causa versus mapa de motivo; fallback legado. `app/folha/retrato_folha.py:380`, `app/folha/retrato_folha.py:491`. |
| `_saldo_cru` / `saldo_da_linha` / `no_periodo` | Consulta motivo para módulos/cadastro e recalcula faixa sobre o número de datas recortadas. `app/folha/retrato_folha.py:414`, `app/folha/retrato_folha.py:501`, `app/folha/retrato_folha.py:1240`, `app/folha/retrato_folha.py:1245`. |
| `o_que_fazer` | Apresenta o rótulo do motivo no drawer. `app/folha/retrato_folha.py:1062`, `app/folha/retrato_folha.py:1065`; chamado em `app/colaboradores/views.py:984`. |
| Tela, PDF e grupos do drawer do retrato | Apresentam rótulo de motivo. `app/folha/views_retrato.py:285`, `app/folha/views_retrato.py:417`, `app/folha/views_retrato.py:496`. |
| Vigia | Leitura indireta por porta/balde e fio; não compara diretamente o motivo. `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:47`. |
| Botões de ação | Leitura indireta por fio e tipo. `app/folha/retrato_folha.py:1015`. |

Os outros consumidores literais encontrados de `furo_espelho` — `dias_que_seguram`, views antigas de folha e `fechamento_lista` — leem **itens do juiz**, não a chave de linhas já gravadas do Retrato. `app/folha/fora_da_folha.py:119`, `app/folha/views.py:259`, `app/folha/views.py:591`, `app/ponto/services/fechamento_lista.py:34`.

Não encontrei defeito novo no filtro de motivo do delta: ele elimina precisamente a segunda forma legítima de datas, preservando o ramo do retentor. `app/escala/services/cadastro_realidade.py:189`, `app/folha/retrato_folha.py:1171`, `app/folha/retrato_folha.py:1175`.

## 06/11 17:57 · sessao 99686e6c7958 · - · - · origem=Ronald (tmux)

A generalização da volta 1 foi corrigida: `LEIS.md:177` agora delimita a ausência ao comparador **genérico** e reconhece os dois escritores anteriores. O passivo lê `antes/depois`, monta `pago -> devido` e escreve cabeça/filhas (`app/chamados/services/passivo_validado.py:178`, `:192`, `:229`, `:233`); seu chamador retifica pela porta e então gera a pauta (`app/chamados/services/passivo_validado.py:438`, `:443`). HE usa lavratura para pago, motor para devido e escreve a pauta (`app/ponto/services/autorizacao_he_periodo.py:174`, `:176`, `:184`, `:244`).

A cauda **“caminho AUTOMATICO segue sem codigo”** permanece ambígua: é compatível com a linha se conservar o escopo **genérico**; como negação de qualquer geração automática de Pauta DP com dois números, seria falsa, pois `_ato` chama `_linha_para_o_dp` quando a competência está exportada e houve autorização (`app/docs/LEIS.md:177`; `app/ponto/services/autorizacao_he_periodo.py:299`, `:300`). Eu não manteria o achado antigo como falsidade categórica sem explicitar essa interpretação.

O comando PAUTA-08 continua registrado como ato de uma vez por competência, lista fechada e escrita explícita (`app/config/crons.py:781`, `:784`, `:786`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:79`, `:160`). Existência histórica de outros caminhos ausentes desta cópia: **[nao verificado]**.

## 06/11 17:55 · sessao 9a0f294cef8e · ESPURIA · censo · origem=Code

HASH LIDO: 7c140052a45f90bc52eb5d68726782428fe9307c — identificador fornecido da cópia; correspondência com HEAD [nao verificado].

O quadro **477 = 175 + 262 + 40 + 0** é o declarado no dado raspado, sem linhas individuais para reclassificação ou conferência das amostras. A nova distribuição, os destinos dos 175 e a quantidade de dias com tipos divergentes nas lâmpadas são [nao verificado].

**1. “Dia com órfã na ata”: entidade, escritores, leitores e consulta**

A entidade é **`escala.models.CelulaDia`**, campo JSON **`ata`**, com unicidade por `(colaborador, data)`. Não é uma tabela separada de atas versionadas. (`app/escala/models.py:1774`, `app/escala/models.py:1778`, `app/escala/models.py:1833`)

O produtor é `ata_do_dia(dia, marcos=None, dna=None)`: grava `ata['orfas']` com as horas de `dia['orfas']` e `ata['n_orfas']` com sua quantidade. O alinhamento que produz essas órfãs é `_match_marcos(...)`: devolve `(celulas, orfas)`, sendo órfã a marca não atribuída a marco. (`app/ponto/services/cartorio.py:696`, `app/ponto/services/cartorio.py:708`, `app/escala/utils.py:585`, `app/escala/utils.py:621`, `app/escala/utils.py:662`)

Quem persiste é **`ponto.portas.celula.lavrar_veredito`**; ela pode atualizar a ata mesmo quando preserva um veredito administrativo. A chave `precedencia` tem outro escritor declarado, `carimbar_precedencia`, e atravessa a substituição da ata. (`app/ponto/portas/celula.py:85`, `app/ponto/portas/celula.py:104`, `app/ponto/portas/celula.py:995`, `app/ponto/portas/celula.py:1027`)

Leitores existentes:

| Leitor | O que devolve |
|---|---|
| `_orfas_da_ata(ata)` | Horas gravadas; em ata antiga contendo somente contagem, devolve `'?'` para cada órfã. (`app/escala/services/leitor_celula.py:175`) |
| `grade_da_celula(colab, ini, fim)` | Grade por período, transportando órfãs, lâmpadas, veredito e célula; lê a ata persistida. (`app/escala/services/leitor_celula.py:345`, `app/escala/services/leitor_celula.py:401`) |
| `periodos_do_dia(celula, tipo_escala)` | Interpretação do dia pela ata, incluindo órfãs e papéis das lâmpadas. (`app/ponto/juiz_batida.py:149`) |

A consulta abaixo seleciona **dias da última ata persistida com `n_orfas > 0`**, por empresa. A janela vem de `janela_fechamento`, que lê `Empresa.dia_inicio_competencia`; corte 21 produz 21/10–20/11/2026. (`app/ponto/janelas.py:10`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:54`, `app/colaboradores/models.py:46`)

```python
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento

ini, fim = janela_fechamento(11, 2026, empresa)

dias = (
    CelulaDia.objects
    .filter(
        colaborador__empresa=empresa,
        data__range=(ini, fim),
        ata__n_orfas__gt=0,
    )
    .order_by('colaborador_id', 'data')
)
```

Para a frota, aplicar a consulta às empresas do universo, respeitando o corte de cada uma. A célula não tem filtro próprio `vigente`/`retratada_em`; a leitura canônica consulta diretamente `(colaborador, intervalo)`, sem exigir vínculo atualmente ativo. (`app/escala/models.py:1681`, `app/escala/models.py:1833`, `app/escala/services/leitor_celula.py:350`)

**Retratação pertence ao universo das batidas:** `batidas_apuraveis(colab, ini=None, fim=None)` aplica `retratada_em__isnull=True`. Não se deve inventar esse filtro na célula nem juntar batidas por data civil para redefinir os dias da ata. (`app/ponto/turnos.py:2844`, `app/ponto/turnos.py:2863`, `app/ponto/services/cartorio.py:785`)

“Persistida” não garante “atualizada com todos os insumos atuais”: `grade_da_celula` não confere impressão; o cartório calcula `_em_dia` internamente. `houve_fato_novo(...)` exclui deliberadamente chamado/fio e não substitui essa conferência integral. (`app/escala/services/leitor_celula.py:362`, `app/ponto/services/cartorio.py:329`, `app/ponto/services/cartorio.py:1348`)

**2. Onde a tela escreve “espúria”**

O literal encontrado está no **calendário do colaborador**:

> “Batida fora do turno previsto (espúria) — não casa nenhum marco; conferir.”

Ele aparece quando `b.orfa` é verdadeiro. (`app/templates/colaboradores/partials/_calendario_grade.html:121`)

Quem atribui esse sinal é `contexto_calendario(...)`: obtém as órfãs pela grade lida da célula e marca `b.orfa=True` quando a hora local da batida pertence ao conjunto de órfãs daquele dia. Portanto, esse caminho **lê a órfã da ata**; não executa novamente o casamento batida–marco. (`app/colaboradores/services/calendario.py:332`, `app/colaboradores/services/calendario.py:381`, `app/colaboradores/services/calendario.py:621`, `app/escala/services/leitor_celula.py:403`)

No arquivo **`templates/ponto/espelho.html`**, a órfã aparece como **`• HH:MM`**, sem esse literal. `montar_realizado_grade(...)` transporta a lista de órfãs para a apresentação. (`app/templates/ponto/espelho.html:240`, `app/escala/utils.py:1099`, `app/escala/utils.py:1131`)

O serviço do espelho usa a célula na competência, mas também possui caminho pelo builder para o trecho anterior à competência e degradação expressa para ata agregada sem marcos. Assim, “todo espelho sempre lê somente a ata” não descreve todos os caminhos existentes. (`app/ponto/services/espelho.py:518`, `app/ponto/services/espelho.py:528`, `app/ponto/services/espelho.py:537`)

**3. Funções para classificar os dias e censo de casamento com escala**

`dia_sem_trabalho(cel) -> bool` recebe uma célula, não uma lista de batidas. Lê `trabalha` e `ata['tipo_dia']`: considera `trabalha is False` ou tipo preenchido diferente de `trabalho`; na ausência parcial, só `trabalha is False`. Está registrado como juiz. (`app/chamados/juizes.py:413`, `app/chamados/juizes.py:426`, `app/core/juizes.py:278`)

**`STATUS_QUE_PEDEM_DECISAO` não é classificador de folga trabalhada.** Seu conteúdo é `('falta', 'irregular', 'aberto', 'sem_certidao')`. A função que entrega o status é `status_do_dia_lavrado(colaborador, data, *, agora=None) -> {'status', 'rotulo', 'fonte'}`, registrada em `core/juizes.py`. (`app/colaboradores/services/calendario.py:94`, `app/ponto/status_dia.py:201`, `app/core/juizes.py:187`)

A forma de lote é `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None) -> {(colaborador_id, data): resposta}`; é o mesmo corpo do juiz singular. O calendário lê essa resposta. (`app/ponto/status_dia.py:218`, `app/ponto/status_dia.py:221`, `app/colaboradores/services/calendario.py:602`, `app/colaboradores/services/calendario.py:663`)

Para identificar **folga trabalhada**, ler o status `folga_trabalhada`. A regra de lavra existente o produz quando há batida e `tipo_dia == 'folga'` ou `veredito == 'fato_sem_previsao'`. Já `resolver_dia_oferecido(dia, hoje) -> bool` responde outra pergunta: último recurso disponível, dia passado e status pertencente à constante. (`app/ponto/status_dia.py:157`, `app/colaboradores/services/calendario.py:97`, `app/core/juizes.py:718`)

**`trabalha_em_feriado` pertence a `EscalaColaborador`, o vínculo.** Não pertence ao template `TipoEscala`. `ec.feriado_vira_folga(data, escala=None) -> bool` reúne flag desligada, ciclo elegível e feriado do município do posto daquele vínculo. (`app/escala/models.py:871`, `app/escala/models.py:896`, `app/escala/models.py:1373`)

Para perguntar o feriado: `eh_feriado(data, lugar_do_vinculo(ec)) -> bool`. Para uma janela pessoal, `feriados_do_colab(colaborador, ini, fim, *, carregados=None, vinculos=None, celulas=None) -> set[date]` resolve o vínculo **de cada dia**. `eh_feriado` está registrado. (`app/core/feriados.py:91`, `app/core/feriados.py:114`, `app/core/feriados.py:125`, `app/core/juizes.py:176`)

O censo de “qual escala casa” encontrou funções com perguntas diferentes:

| Função real | Entrada → saída; batidas e limite |
|---|---|
| `ponto.propositor.dia_degenerado(dia)` | Dict-dia → `(bool, motivo)`: cluster, contagem incompatível, órfã, tipo incoerente ou delta crítico; pode devolver `SEM_HORA_NA_ATA`. Lê marcas já presentes no dict; **não escolhe outra escala**. (`app/ponto/propositor.py:34`) |
| `atribuir_global(slots, marcas)` / `atribuir_global_turno(slots, marcas)` | Slots e horas → `atrib`, `furos`, `sobras`, `custo`, `desloc`, `ambiguo`. Funções puras; casam marcas aos slots recebidos. A variante turno trata meia-noite; não pesquisa catálogo. (`app/ponto/propositor.py:87`, `app/ponto/propositor.py:112`, `app/ponto/propositor.py:165`) |
| `template_suspeito(dias)` | Dict-dias → `(suspeito, mediana, n_dias)`. Lê deltas recebidos; não consulta batidas nem escolhe template. (`app/ponto/propositor.py:201`) |
| `escala.services.cadastro_tipo` | `descreve_turno_partido(payload, tipo=None, pai=None) -> bool` lê a descrição do turno; `tipo_em_memoria(**campos)` instancia template sem salvar. Não foi encontrado matcher batidas→escala nesse módulo [nao verificado]. **`salvar_tipo` e `derivar_tipo` escrevem**. (`app/escala/services/cadastro_tipo.py:86`, `app/escala/services/cadastro_tipo.py:140`, `app/escala/services/cadastro_tipo.py:396`, `app/escala/services/cadastro_tipo.py:401`) |
| `escala.services.propositor.propostas(hoje=None, janela=28, colaborador_id=None, so_existencia=False)` | Coleta → dict com propostas e universo. Lê batidas não retratadas, células e templates; propõe âncora, template ou grade por dia. A coleta não é `aplicar`/`recusar`, que escrevem. (`app/escala/services/propositor.py:38`, `app/escala/services/propositor.py:382`, `app/escala/services/propositor.py:477`, `app/escala/services/propositor.py:585`) |
| `propor(ec, dias_bat, dias_janela, templates, previsto_atual, forcar='', ritmo='', horario=(None,None), pausa=None)` | Insumos já medidos → proposta ou `None`. O casamento comum compara **datas**; há filtro de horário no caminho por ritmo. Não prova, em todo caminho, casamento horário de todos os marcos. (`app/escala/services/propositor.py:162`, `app/escala/services/propositor.py:235`, `app/escala/services/propositor.py:308`, `app/escala/services/propositor.py:526`) |
| `fase_12x36.ancora_sugerida(dias_batidos, ancora_atual, piso=6, ganho_min=40)` | Datas → dict com âncora ISO ou `None`, motivo e porcentagens. Pura; compara **paridade**, não horário nem catálogo de escalas. (`app/escala/services/fase_12x36.py:23`, `app/escala/services/fase_12x36.py:27`) |
| `fase_12x36.dias_de_fase_as_of(colab_id, empresa_id, ini, fim, ate=None)` | Identificação/janela → datas ISO da fase deslocada, pela lavra mais recente que cobriu cada dia. Lê snapshots, não recalcula batidas. O detector anterior foi removido; o censo é a autoridade de fase indicada pelo módulo. (`app/escala/services/fase_12x36.py:68`, `app/escala/services/fase_12x36.py:117`) |
| `censo_fase_12x36.Command` | Competências/vínculos → censo de paridade real; critério `<90%` com `>=6` plantões. Lê turnos; `--lavrar` e `--curar` possuem escrita. (`app/ponto/management/commands/censo_fase_12x36.py:36`, `app/ponto/management/commands/censo_fase_12x36.py:180`, `app/ponto/management/commands/censo_fase_12x36.py:256`, `app/ponto/management/commands/censo_fase_12x36.py:274`) |
| **`escala.detector_proposta.propor_escala(colab, ini, fim)`** | Pessoa/janela → `padrao`, `proposta`, `confianca`, `porque`. Lê `batidas_apuraveis`; procura template por entrada, saída e número de marcos. **Limite declarado: agrupa por LOCALDATE, não dia-turno; noturno pode distorcer a medida.** (`app/escala/detector_proposta.py:18`, `app/escala/detector_proposta.py:49`, `app/escala/detector_proposta.py:95`, `app/escala/detector_proposta.py:118`) |
| `regua_defesa.diagnostico_regua(ec, ref=None, dias=30)` + `sugerir_destino(ec, diag)` | Vínculo/janela → diagnóstico com classe, evidência e medianas; diagnóstico → destino `{tipo_escala_id, codigo, n_colabs, dist}` ou `None`. O primeiro lê batidas apuráveis; o segundo compara com templates usados na mesma empresa. (`app/escala/regua_defesa.py:118`, `app/escala/regua_defesa.py:153`, `app/escala/regua_defesa.py:246`) |
| `fase_na_tela.fase_sugerida(c, te, hoje=None, ec_atual=None)` | Pessoa/template → `(ancora, desde, motivo)`. Lê dias trabalhados e troca de ritmo; o `desde` refere-se à fase/ritmo, não à prova de todos os horários casarem com um novo template. (`app/escala/services/fase_na_tela.py:40`, `app/escala/services/fase_na_tela.py:50`) |
| `pausa_nao_cadastrada(te, batidas_por_dia)` | Template/timestamps → pausa sugerida com evidência ou `None`. Não escolhe a escala inteira. (`app/escala/services/pausa.py:29`) |

**Registro desses candidatos:** não localizei os nomes acima como entradas próprias em `core/juizes.py` [nao verificado quanto à ausência global]. Os registros encontrados respondem **ocupação de marco** por `_match_marcos`, **turno partido** por `eh_turno_partido` e **vínculo que vale no dia/cobertura da data** por `vinculo_do_dia`/`vinculo_cobre_a_data`; essas perguntas não equivalem a escolher “a escala que casa com as batidas”. (`app/core/juizes.py:200`, `app/core/juizes.py:208`, `app/core/juizes.py:1285`)

O destino já mostrado em Cadastro x Realidade vem da lavra do esmeril: `lista(...)` lê essa lavra; `anomalias_da_frota(...)` chama `propor_escala` sobre a janela de 60 dias. Chamar `lista` lê uma foto; chamar `gravar_lavra` escreve. (`app/escala/services/cadastro_realidade.py:710`, `app/escala/services/cadastro_realidade.py:802`, `app/ponto/services/esmeril_espelho.py:206`, `app/ponto/services/esmeril_espelho.py:223`, `app/ponto/services/esmeril_espelho.py:317`)

Para o **dono L-099**, já existe `dono_da_divergencia(...) -> 'CADASTRO'|'BATIDA'|'ESTRUTURA'`, com montagem centralizada em `InsumosDoDono(...).dono(data)`. O carregador pode consultar o espelho/motor para `dias_cadastro_x_realidade`; essa etapa deve seguir a restrição de sombra do dado fornecido [nao verificado no repositório]. (`app/ponto/management/commands/e6_oraculo.py:53`, `app/ponto/management/commands/e6_oraculo.py:98`, `app/ponto/management/commands/e6_oraculo.py:128`, `app/ponto/management/commands/e6_oraculo.py:154`, `app/ponto/management/commands/e6_oraculo.py:172`)

**4. “Célula refeita” e produção de órfã**

Os sinais de regeneração são **`regenerada_em`**, **`regeneracoes`** e **`dna_anterior`**. O último conserva apenas o DNA imediatamente anterior, não todo o histórico. `editada_em`/`origem='editada'` representam edição humana, outra operação. (`app/escala/models.py:1704`, `app/escala/models.py:1718`, `app/escala/models.py:1726`, `app/ponto/portas/celula.py:534`)

O escritor `regenerar_celulas_vinculo(...) -> número de células alteradas` troca trabalho/vínculo gerador/DNA quando há diferença, guarda o DNA velho e incrementa o contador. (`app/ponto/portas/celula.py:110`, `app/ponto/portas/celula.py:118`, `app/ponto/portas/celula.py:263`)

Há **duas órfãs diferentes**:

- **Batida órfã na ata:** não encontrou marco no alinhamento atual. Uma reescrita pode alterar os marcos comparados, mas o sinal de regeneração sozinho não prova essa causalidade. (`app/escala/utils.py:585`, `app/escala/utils.py:662`; causalidade dos 40 [nao verificado])
- **Cobrança órfã de regeneração:** cobrança emitida contra DNA anterior. A regeneração marca essas cobranças para novo julgamento; existe `cobranca_orfa(janelas, chamado, regenerada_em=None, editada_em=None) -> instante|None`. (`app/ponto/portas/celula.py:282`, `app/escala/services/regeneracao.py:141`)

Portanto, os **40** não podem ser certificados como “órfãs causadas pela célula refeita” apenas por `regenerada_em` preenchido [nao verificado].

**5. Marco de entrada aceso por batida gravada como saída**

Em **`ata['lampadas']`**, cada elemento guarda:

| Chave | Significado |
|---|---|
| `tipo`, `hora` | Tipo e hora do marco previsto. |
| `acesa`, `luz` | Estado da lâmpada e hora real atribuída. |
| `tipo_real` | Tipo gravado da batida atribuída. |

Essas chaves são produzidas em `lampadas_do_dia`; no casamento por proximidade sem coluna correspondente, `tipo_real=None` significa desconhecido. (`app/ponto/services/cartorio.py:638`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:688`)

**Já há comparação escrita:**

- `montar_realizado_grade(gd, marcos, chamados_dia=None) -> (real, orfas)` devolve **`real[].divergente`**, comparando literalmente `tipo_real` com o tipo do marco recebido. (`app/escala/utils.py:1099`, `app/escala/utils.py:1123`)
- O juiz registrado `periodos_do_dia(celula, tipo_escala) -> dict` devolve **`papeis[].tipo_divergente`**. Aqui o esperado vem do **DNA**; a conferência com a ata verifica horas e quantidade, podendo retornar `desalinhado` antes da comparação. (`app/ponto/juiz_batida.py:149`, `app/ponto/juiz_batida.py:195`, `app/ponto/juiz_batida.py:228`, `app/ponto/juiz_batida.py:246`, `app/core/juizes.py:1174`)

Para contar **a divergência literal da ata**, usar o adaptador `montar_realizado_grade` sobre a leitura das lâmpadas, contar uma vez cada `(colaborador,data)` com `divergente=True` entre lâmpadas efetivamente acesas e separar tipos desconhecidos. Para contar pela interpretação do juiz registrado, usar `periodos_do_dia` e publicar separadamente os dias `desalinhado`. As duas comparações têm referências esperadas diferentes; não devem receber o mesmo nome sem essa distinção. (`app/escala/services/leitor_celula.py:114`, `app/escala/services/leitor_celula.py:128`, `app/escala/utils.py:1130`, `app/ponto/juiz_batida.py:239`, `app/ponto/juiz_batida.py:246`)

O universo dessa pergunta é **todas as células da janela**, não apenas as que têm órfã: uma marca atribuída deixa de ser órfã mesmo podendo carregar tipo divergente. Assim, a contagem de órfãs não mede esse fenômeno. (`app/escala/utils.py:245`, `app/ponto/services/cartorio.py:677`, `app/ponto/services/cartorio.py:708`)

`dia_degenerado` também acusa `TIPO_INCOERENTE`, mas retorna antes para cluster/órfãs/contagem incompatível; não serve como contador completo das inversões. Quantidade real da frota [nao verificado]. (`app/ponto/propositor.py:56`, `app/ponto/propositor.py:65`)

**6. Dinheiro, decisão pendente e porta alcançável**

O dinheiro persistido é **`DiaPago`**, com versão escolhida explicitamente. `linhas_do_periodo(colab, ini, fim, *, versao='motor') -> {date: DiaPago}` lê as linhas diárias sem executar motor. A data da linha é a data da entrada do turno. (`app/ponto/models.py:303`, `app/ponto/models.py:319`, `app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:853`)

Para cada dia, os campos relevantes são `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, variantes noturnas e `horas_folga_trabalhada`; são **horas de rubricas**, não valores em reais. (`app/ponto/models.py:325`, `app/ponto/models.py:328`)

As funções de leitura prontas são:

| Função | Resposta |
|---|---|
| `rubricas_do_dia(linha)` | Dict com rubricas e `lavrada`. Linha ausente retorna zeros **com `lavrada=False`**. Variantes feriado/noturna são subconjuntos dos totais. (`app/ponto/services/dia_pago.py:899`) |
| `pago_do_dia(linha)` | Horas de trabalho pagas ou `None`; reúne trabalhadas, folga trabalhada e `detalhe['folga_feriado_min']`. Sem linha ou com todas as rubricas monetárias zeradas, retorna `None`. **HE zero isolada não prova ausência de dinheiro.** (`app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`) |
| `estado_por_dia(ids, ini, fim)` | Decisões de HE **vigentes**, por pessoa/data. Estado `'nao'` é decisão; ausência no mapa não prova, sozinha, existência de pendência. (`app/ponto/services/gestao_he.py:63`, `app/ponto/services/gestao_he.py:86`, `app/folha/porta_export.py:218`) |
| `he_pendente_lavrado.ler(empresa, mes, ano)` | `(detalhe, calculado_em)` ou `(None,None)`: foto de pendências, sem nova apuração. (`app/ponto/services/he_pendente_lavrado.py:193`) |
| `he_pendente_lavrado.apurar(empresa, mes, ano, *, decisoes=None, revogar=None)` | Mede a pendência atual pelo espelho/motor. Sem callback `revogar`, não revoga decisões; `lavrar` é a operação que grava a foto. (`app/ponto/services/he_pendente_lavrado.py:71`, `app/ponto/services/he_pendente_lavrado.py:94`, `a

[truncado em 20000 de 29100 caracteres]

## 06/11 17:51 · sessao 911185810e67 · - · - · origem=Ronald (tmux)

6. Dinheiro, decisão e porta

- O dado persistido por dia é `ponto.models.DiaPago`: chave colaborador/competência/data/versão, com `data` do dia da entrada; rubricas `horas_extras_50`, `horas_extras_100`, `horas_extras_100_feriado`, variantes noturnas e `horas_folga_trabalhada`. A restrição única inclui `versao`, portanto a leitura deve escolher a versão; a porta de leitura usa `versao='motor'` por padrão. (`app/ponto/models.py:315`, `app/ponto/models.py:328`, `app/ponto/models.py:386`; `app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:853`)
- Função pronta: `linhas_do_periodo(colab, ini, fim, *, versao='motor') -> {date: DiaPago}`. Filtra `tipo=TIPO_DIA`, versão e intervalo inclusivo; não roda motor. (`app/ponto/services/dia_pago.py:831`, `app/ponto/services/dia_pago.py:852`)
- `rubricas_do_dia(linha) -> dict`: retorna `lavrada`, `extras_50`, `extras_100`, `atraso`, `antecipada`, `noturnas`, `intra`, `trabalhadas`, `minutos_extra`; ausência de linha dá zeros **com `lavrada=False`**. As variantes feriado/noturna são subconjuntos e não devem ser somadas novamente aos totais. Para distinguir a rubrica de feriado e folga, ler também os campos específicos do `DiaPago`. (`app/ponto/services/dia_pago.py:899`, `app/ponto/services/dia_pago.py:908`, `app/ponto/services/dia_pago.py:912`)
- `pago_do_dia(linha) -> float | None`: `None` se não há linha ou se todas as rubricas de dinheiro estão zero; caso contrário soma `horas_trabalhadas + horas_folga_trabalhada + detalhe['folga_feriado_min']/60`. Logo, **HE zero isolada não prova dia sem dinheiro**. (`app/ponto/services/dia_pago.py:859`, `app/ponto/services/dia_pago.py:865`, `app/ponto/services/dia_pago.py:888`)
- A escrita da folga certa vai para `horas_folga_trabalhada`; a folga-feriado vai para HE100 e HE100-feriado, com `detalhe['folga_feriado_min']`; folga sem escala certa vai para `horas_trabalhadas`, com `detalhe['folga_sem_escala_min']`. (`app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`, `app/ponto/services/dia_pago.py:257`)
- A decisão de HE está em `DecisaoHE`, estados `'autorizado'` e `'nao'`; `'nao'` é decisão, não ausência de decisão. Só as decisões `objects.vigentes()` valem: filtro `revogada_em__isnull=True`. (`app/ponto/models.py:1255`, `app/ponto/models.py:1260`, `app/ponto/models.py:1273`, `app/ponto/models.py:1277`)
- Função pronta para decisão vigente: `estado_por_dia(ids, ini, fim) -> {(colaborador_id, 'YYYY-MM-DD'): estado}`; lê uma consulta em `DecisaoHE.objects.vigentes()`. **Ausência desse mapa só prova falta de decisão; a existência de pendência exige a fonte de pendência.** (`app/ponto/services/gestao_he.py:63`, `app/ponto/services/gestao_he.py:81`)
- Pendência de HE é `dia['he_fora_da_janela']`, montada pelo espelho; o caso de dia sem previsto é lido de `resultado_v2.dias_sem_previsto_fora_da_janela`, com `sentido='dia'`, `marco=None` e minutos publicados pelo motor. (`app/ponto/services/he_pendente_lavrado.py:19`; `app/ponto/services/espelho.py:743`)
- Para pendência já lavrada, `he_pendente_lavrado.ler(empresa, mes, ano) -> (detalhe, calculado_em) | (None, None)` lê `MetricaSnapshot`, sem motor, e informa a idade da foto; para medir atual, `apurar(empresa, mes, ano, *, decisoes=None, revogar=None)` chama `espelho_do_colab`, portanto passa pelo motor. Sem callback `revogar`, esse apurador declara-se leitor puro; não chamar `lavrar`, que grava. (`app/ponto/services/he_pendente_lavrado.py:71`, `app/ponto/services/he_pendente_lavrado.py:77`, `app/ponto/services/he_pendente_lavrado.py:94`, `app/ponto/services/he_pendente_lavrado.py:177`, `app/ponto/services/he_pendente_lavrado.py:193`)
- Para saber se pendência exige humano, já existe `acima_do_limite(minutos, limite) -> bool`, comparando o total do dia com `Empresa.limite_decisao_he_min`. O consumidor pronto `dias_acima_do_limite_sem_decisao(pendencias, limite, estados) -> [(colaborador_id, 'YYYY-MM-DD')]`, sem duplicatas, usa esse juiz e o mapa de decisão; `'nao'` também libera. (`app/ponto/portas/he.py:249`, `app/ponto/portas/he.py:265`; `app/folha/porta_export.py:199`, `app/folha/porta_export.py:223`)
- `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` está registrada em `core/juizes.py`. Sem ID ou sem verbo de porta alcançável retorna `None`; com porta retorna `{'rota', 'fila', 'verbos', 'fio'}`. (`app/chamados/juizes.py:1341`, `app/chamados/juizes.py:1373`, `app/chamados/juizes.py:1382`, `app/chamados/juizes.py:1386`; `app/core/juizes.py:277`)
- `rota='porta_fila'`: existe chamado da própria célula na fila; senão, `rota='porta_pergunta'`: pergunta viva da célula, presente no app, não respondida sem texto, cuja âncora está na fila. `fila` só enumera os chamados da célula visíveis na lista padrão; `verbos` pode conter também âncoras de pergunta. (`app/chamados/juizes.py:1344`, `app/chamados/juizes.py:1353`, `app/chamados/juizes.py:1375`)
- `fio`: qualquer verbo `cobrar` vence e dá `'colab'`; depois `validar` dá `'admin'`; os demais dão `'chamado'`. Para “há ato pendente do admin?”, consultar os verbos da porta contra **`VERBOS_DO_ADMIN=('validar','decidir')`**, porque `fio='chamado'` pode conter `decidir`. (`app/chamados/juizes.py:1327`, `app/chamados/juizes.py:1332`; `app/chamados/services/verbos.py:22`)
- A porta não precisa rodar o motor de dinheiro no seu corpo e alimentação; usa fila, partição e perguntas vivas. **Não certificaria ausência de escrita transitiva:** `alimentar_portas -> pergunta_viva -> dia_da_pergunta_trancado -> _admin_tocou_o_dia_depois -> _toque_humano_no_dia -> get_user_sistema`, e esse último usa `User.objects.get_or_create`. É escrita potencial se o usuário estiver ausente, apesar das docstrings de “SO LEITURA”; futura execução deve respeitar o envelope somente leitura. (`app/chamados/juizes.py:1282`, `app/chamados/juizes.py:1302`, `app/chamados/juizes.py:1140`, `app/chamados/juizes.py:954`, `app/chamados/juizes.py:1047`, `app/chamados/juizes.py:1069`; `app/core/utils.py:18`)
- Dinheiro atual sem lavra exige `autoridade_do_periodo(colaborador, ini, fim)`/`espelho_do_colab(...)`, que constroem e executam motor; pelo escopo do aval, essa etapa fica na sombra. A simples leitura de `DiaPago`, da decisão vigente, da foto de pendências e da porta não exige nova apuração monetária. (`app/ponto/services/espelho.py:971`, `app/ponto/services/espelho.py:1111`, `app/ponto/services/espelho.py:1164`, `app/ponto/services/espelho.py:1179`; `app/ponto/services/dia_pago.py:852`; restrição sombra: dado fornecido [nao verificado no repositório])

7. Cadastro para comparar os dois dias de 02/11

- O código de importação reconhece `(11,2)` como **Finados**, na allowlist de nacionais; isso não prova que a linha de 02/11/2026 está cadastrada na produção. (`app/core/views_config.py:111`, `app/core/views_config.py:117`; cadastro PROD [nao verificado])
- A autoridade runtime é `Feriado(data, abrangencia, uf, municipio)`, alcançada por `core.feriados.eh_feriado(data, municipio)` ou `feriados_do_colab(colaborador, ini, fim, ...)`. Nacional alcança todos; estadual exige UF; municipal exige UF e município normalizado. (`app/core/models.py:5`, `app/core/models.py:12`; `app/core/feriados.py:57`, `app/core/feriados.py:91`, `app/core/feriados.py:125`)
- O lugar do feriado vem do **`posto.municipio` do vínculo daquele dia**, não do texto livre da praça nem do posto atual de `Colaborador`. `lugar_do_vinculo(vinculo) -> Municipio|None`; `feriados_do_colab` consulta `vinculo_do_dia`. (`app/core/feriados.py:114`, `app/core/feriados.py:121`, `app/core/feriados.py:150`)
- O vínculo é `EscalaColaborador`, com `posto`, `tipo_escala`, datas início/fim, e `trabalha_em_feriado`. O campo só transforma feriado em folga nos ciclos 6x1, 5x2 e semanal; a função pronta é `ec.feriado_vira_folga(data, escala=None) -> bool`, que junta esse cadastro, ciclo e feriado pelo município do posto do vínculo. (`app/escala/models.py:871`, `app/escala/models.py:896`, `app/escala/models.py:32`, `app/escala/models.py:1373`, `app/escala/models.py:1389`)
- Para vínculo novo, `padrao_trabalha_em_feriado(tipo_escala, posto=None) -> bool` dá precedência à declaração `Posto.opera_em_feriado`; se não declarada, usa o tipo. Isso define o nascimento, não modifica vínculo existente. (`app/escala/models.py:905`, `app/escala/models.py:908`, `app/escala/models.py:911`)
- A dobra normal de feriado é decidida por `MotorBase._feriado_dobra_no_dia(dia) -> bool(self.feriado_em_dobra) and self.eh_feriado(dia)`, com dia de início do plantão. A política padrão sai de `TipoEscala.tipo_ciclo`, não de `tipo_base`: 12x36 está em `CICLOS_FERIADO_SIMPLES`; os demais ciclos recebem dobra por padrão. (`app/ponto/motor_calculo_v2.py:914`, `app/ponto/motor_calculo_v2.py:928`, `app/ponto/motor_calculo_v2.py:934`, `app/ponto/motor_calculo_v2.py:3584`, `app/ponto/motor_calculo_v2.py:3599`, `app/ponto/motor_calculo_v2.py:3669`)
- A CCT pode restaurar dobra do 12x36 via `Sindicato.feriado_12x36_em_dobra`; `get_motor_cct` aplica esse campo quando o switch global `ParametroSistema(chave='regua_cct_ativa', empresa=None)` está ligado. (`app/core/models.py:56`; `app/core/regua_cct.py:428`, `app/core/regua_cct.py:443`, `app/core/regua_cct.py:449`, `app/core/regua_cct.py:477`)
- `regua_para(colaborador, competencia, tipo_ciclo=None)` resolve o vínculo da data, sua praça e ciclo; lê `Empresa.regime_trabalhista`. Em `'clt'` devolve régua legal; em `'cct'` busca aplicação vigente. `AplicacaoConvencao(empresa, sindicato, praca opcional, ativo)` é resolvida por `_aplicacao_vigente`: praça específica vence empresa inteira; empate, falta ou sindicato fora da vigência resultam em piso legal com motivo. (`app/core/regua_cct.py:297`, `app/core/regua_cct.py:328`, `app/core/regua_cct.py:352`, `app/core/regua_cct.py:363`, `app/core/regua_cct.py:231`, `app/core/regua_cct.py:253`, `app/core/regua_cct.py:261`, `app/core/regua_cct.py:265`)
- Para folga trabalhada, a apuração do adicional passa por `escala_certa_no_dia(colab.id, data, cache)`. Com escala certa e `feriado_vira_folga=True`, o fechamento separa o período para HE100-feriado; sem escala certa, a hora vai a trabalhadas sem adicional. (`app/ponto/services/fechamento.py:919`, `app/ponto/services/fechamento.py:925`, `app/ponto/services/fechamento.py:935`, `app/ponto/services/fechamento.py:946`, `app/ponto/services/fechamento.py:951`)
- Há outro cadastro decisivo para HE zero: janela de HE da empresa, sua vigência e decisão autorizada. O motor bloqueia integralmente o par de folga/feriado sem previsto quando a janela se aplica, salvo autorização; folga sem escala certa fica fora desse bloqueio específico. A pendência recebe os minutos que o par pagaria. (`app/colaboradores/models.py:58`, `app/colaboradores/models.py:117`; `app/ponto/motor_calculo_v2.py:1422`, `app/ponto/motor_calculo_v2.py:1448`, `app/ponto/motor_calculo_v2.py:1451`, `app/ponto/motor_calculo_v2.py:1455`, `app/ponto/motor_calculo_v2.py:1465`, `app/ponto/motor_calculo_v2.py:1471`)
- O dado fornecido não contém os vínculos, municípios, ciclos, réguas, decisões ou linhas `DiaPago` dos dois dias; portanto qual desses cadastros efetivamente explica **0 versus 2,31** é [nao verificado].

Regras não encontradas, neste recorte de busca

- Não encontrei função única que, recebendo um dia órfão, devolva ao mesmo tempo dinheiro, pendência de HE e porta; as autoridades estão separadas acima. Busquei `DiaPago`, `he_pendente`, `decisao.*pendente`, `pendente.*decisao`, `porta_alcancavel_do_dia`, `VERBOS_DO_ADMIN`. [nao verificado: ausência global; arquivos principais inspecionados acima]
- Não encontrei regra runtime que faça “02/11 é feriado” sem consultar o cadastro; encontrei a allowlist de importação e o juiz que lê `Feriado`. Busquei `11, 2`, `11-02`, `Finados`, `finados`, `02/11`. (`app/core/views_config.py:117`; `app/core/feriados.py:27`, `app/core/feriados.py:91`; ausência global [nao verificado])
- Não encontrei regra de que `trabalha_em_feriado=True` sozinho garante pagamento: há seleção por ciclo/CCT, escala certa, previsto e janela/autorização. (`app/escala/models.py:1390`; `app/core/regua_cct.py:477`; `app/ponto/services/fechamento.py:925`; `app/ponto/motor_calculo_v2.py:1453`)
- Não encontrei garantia transitiva de somente leitura da porta; encontrei o `get_or_create` citado. (`app/core/utils.py:18`)

## 06/11 17:51 · sessao 851264ea97d5 · - · - · origem=Ronald (tmux)

Itens 3 e complemento 4, somente código lido; nenhuma função executada.

**Predicados e status**

| Função/campo real | Entrada e saída | Batidas/registro |
|---|---|---|
| `chamados.juizes.dia_sem_trabalho(cel)` | `CelulaDia` → `bool`: `cel.trabalha is False` ou `ata.tipo_dia` preenchido e diferente de `trabalho`; na ausência parcial, só `trabalha is False`. Não tratar `None` como folga. | Lê célula/ata, sem ler batidas diretamente; implementação `app/chamados/juizes.py:413`, retorno `:426-428`; registrado `app/core/juizes.py:278`. |
| `ponto.status_dia.status_do_dia_lavrado(colaborador, data, *, agora=None)` | → `{'status','rotulo','fonte'}`; fonte `lavrado`, `relogio`, `sem_celula` ou `nao_lavrado`. | Delegação ao lote em `app/ponto/status_dia.py:201-218`; registrado `app/core/juizes.py:187`. |
| `status_dos_dias_lavrados(pares, *, agora=None, carga=None, fatos=None)` | Pares `(colaborador,data)` → `{(colaborador_id,data): dict}`; é o mesmo corpo do juiz singular. | Carga dos turnos/células e fatos; contrato `app/ponto/status_dia.py:221-236`; calendário importa/chama `app/colaboradores/services/calendario.py:602-607` e lê `status` `:663-664`. |
| `resolver_dia_oferecido(dia, hoje)` | Item de `contexto_calendario()['dias']` → `bool`: `pode_ultimo_recurso` + dia passado + status membro da constante. | Não lê batidas diretamente; `app/colaboradores/services/calendario.py:97-104`; registrado `app/core/juizes.py:718`. |
| `STATUS_QUE_PEDEM_DECISAO` | `('falta','irregular','aberto','sem_certidao')`; **não contém `folga_trabalhada`**. | `app/colaboradores/services/calendario.py:94`; vocabulário próprio `folga_trabalhada` em `app/ponto/status_dia.py:36`. |
| `EscalaColaborador.trabalha_em_feriado` | Booleano do **vínculo**, default model `True`; help diz afetar ciclos weekday `6x1/5x2/semanal`. | `app/escala/models.py:871`, `:896-899`; não é um classificador de batida. |
| `EscalaColaborador.feriado_vira_folga(self,data,escala=None)` | → `bool`: `trabalha_em_feriado=False`, ciclo em `TipoEscala.CICLOS_FERIADO_VIRA_FOLGA` e feriado pelo município do posto **desse vínculo**. | Não lê batidas; `app/escala/models.py:1373-1395`; função não localizada como entrada própria no registro `[nao verificado]`; o registro aponta `fatos_do_dia` para “feriado suprime” (`app/core/juizes.py:281`). |
| `core.feriados.eh_feriado(data,municipio=None)` | → `bool`; sem município, só nacional. Para vínculo: `eh_feriado(data,lugar_do_vinculo(ec))`; para janela pessoal: `feriados_do_colab(colaborador,ini,fim,*,carregados=None,vinculos=None,celulas=None)` → `set[date]`, usando `vinculo_do_dia` por data. | Implementações `app/core/feriados.py:91-95`, `:114-157`; registrado `app/core/juizes.py:176`. |

`folga_trabalhada` é decidido pela lavra quando há batida e `tipo_dia=='folga'` ou `veredito=='fato_sem_previsao'`; `status_da_lavra(...):113` retorna `(status,rotulo)` em `app/ponto/status_dia.py:157-158`. Portanto a constante de “Resolver dia” não serve para identificar todos os dias de folga trabalhada (`app/colaboradores/services/calendario.py:94`).

**Censo de “qual escala casa com as batidas”**

| Sítio | Contrato real e limites | Leitura/escrita/registro |
|---|---|---|
| `ponto.propositor.dia_degenerado(dia)` | Dict-dia da grade → `(bool,motivo)`; motivos cluster, contagem, órfã, tipo incoerente, delta crítico; horas ilegíveis → `SEM_HORA_NA_ATA`. **Não escolhe outra escala.** | Lê dict, não consulta Batida nem grava; `app/ponto/propositor.py:34-73`. Registro próprio não encontrado `[nao verificado]`. |
| `ponto.propositor.atribuir_global(slots,marcas)` / `atribuir_global_turno(slots,marcas)` | Slots `(tipo,'HH:MM')` + horas das marcas → dict `atrib,furos,sobras,custo,desloc,ambiguo`; a variante turno normaliza travessia da meia-noite. **Casa marcas a slots fornecidos**, não busca catálogo. | Puras; `app/ponto/propositor.py:87-117`, `:153-168`. Registro próprio não encontrado `[nao verificado]`; juiz de ocupação registrado é `_match_marcos`, `app/core/juizes.py:200`. |
| `ponto.propositor.template_suspeito(dias)` | Janela de dict-dias → `(suspeito,mediana,n_dias)` pelos deltas absolutos; mínimo três dias; suspeita com mediana >45 min. Não escolhe template. | Pura; `app/ponto/propositor.py:197-221`. Registro próprio não encontrado `[nao verificado]`. |
| `escala.services.cadastro_tipo` | `descreve_turno_partido(payload,tipo=None,pai=None)` → bool por `eh_turno_partido`; `tipo_em_memoria(**campos)` → TipoEscala sem save. **Não encontrei nesse módulo um matcher batidas→escala** `[nao verificado]`. | `app/escala/services/cadastro_tipo.py:86-113`, `:396-398`; `salvar_tipo` cria/edita e pode regenerar vínculos, logo não chamar na sonda (`:140-142`, `:320`, `:327-359`); `derivar_tipo` grava cópia e blocos (`:401-425`). Juiz delegado `eh_turno_partido` registrado `app/core/juizes.py:208`. |
| `escala.services.propositor.propostas(hoje=None,janela=28,colaborador_id=None,so_existencia=False)` | → dict `fonte,n,propostas,universo,janela_dias,...`; uma proposta por vínculo candidato: âncora, template existente ou grade por dia. | Consulta Batida não retratada (`app/escala/services/propositor.py:477-483`), células e templates; dias reais por `turnos_de_batidas` (`:106-118`); saída `:585-589`. Não escreve na coleta (`:42`); **não chamar `aplicar`/`recusar`**, que escrevem (`:38`, `:42`). Registro próprio não encontrado `[nao verificado]`. |
| `escala.services.propositor.propor(ec,dias_bat,dias_janela,templates,previsto_atual,forcar='',ritmo='',horario=(None,None),pausa=None)` | → dict ou None; recebe dias/horários já medidos; cobertura mínima 90%, sobra máxima 30%; retorna `ancora/template/grade_por_dia`. | Sem consulta direta de Batida; `app/escala/services/propositor.py:308-375`, limites `:68-71`. Cuidado: caminho comum compara **datas**, por `_casamento` e `_sobra` (`:235-261`); filtro de horário existe no caminho `_propor_pelo_ritmo` (`:162-197`), e `_horario_mediano` só é passado quando há `_ritmo` (`:526-530`). Portanto não afirmar que toda proposta valida horário de todos os marcos. Registro próprio não encontrado `[nao verificado]`. |
| `escala.services.fase_12x36.ancora_sugerida(dias_batidos,ancora_atual,piso=6,ganho_min=40)` | → dict com `ancora` ISO ou None, motivo, batidas e porcentagens; sugere paridade, não horário/template. | Pura, recebe datas; `app/escala/services/fase_12x36.py:23-65`. O módulo declara quatro sítios de decisão de âncora ainda existentes (`:11-12`). Registro próprio não encontrado `[nao verificado]`. |
| `fase_12x36.dias_de_fase_as_of(colab_id,empresa_id,ini,fim,ate=None)` | → conjunto de datas ISO por lavra mais recente que cobriu cada dia; não recalcula batidas. | Consulta snapshots; `app/escala/services/fase_12x36.py:117-148`; `ultimo_lavrado(ate=None)` → dict ou `sem_medida=True`, `:197-214`. Detector anterior morreu, autoridade declarada é censo `:68-75`; registro próprio não encontrado `[nao verificado]`. |
| `censo_fase_12x36.Command.handle(...)` | Mede paridade real por competência/vínculo; desvio `<90%` com `>=6` plantões. Não há função curta nesse arquivo para devolver “escala X casa”. | Lê turnos reais `app/ponto/management/commands/censo_fase_12x36.py:180`, critério `:256`; opções `--lavrar` e `--curar` escrevem (`:36-42`, `:274-279`, `:339`). Não executar para mero censo sem controlar flags. |
| **Outro matcher:** `escala.detector_proposta.propor_escala(colab,ini,fim)` | → `{'padrao','proposta','confianca','porque'}`; `proposta` inclui template id/código/descrição/ciclo/horário. Casa entrada, saída e número de marcos, raio 30min; mínimo 5 dias; confiança alta exige 15 dias, coerência e estabilidade. | Lê `batidas_apuraveis` em `medir_padrao` (`app/escala/detector_proposta.py:49-58`); matcher `_casa_template` `:95-115`; principal `:118-156`; não escreve `:7`. **Limite declarado:** agrupa LOCALDATE, não dia-turno; noturno pode inflar dias/deslocar entrada (`:18-25`). Registro próprio não encontrado `[nao verificado]`. |
| **Outro:** `escala.regua_defesa.diagnostico_regua(ec,ref=None,dias=30)` + `sugerir_destino(ec,diag)` | Diagnóstico → dict `veredito,classe,evidencia,data_exemplo,declarado,real`, medianas em `med`; destino → `{tipo_escala_id,codigo,n_colabs,dist}` ou None, comparando entrada/intervalo/gap com templates usados por vínculos ativos da mesma empresa. | Diagnóstico lê batidas apuráveis `app/escala/regua_defesa.py:118-174`; destino consulta catálogo/vínculos `:246-288`, sem gravar. Registro próprio desses nomes não encontrado `[nao verificado]`. |
| `fase_na_tela.fase_sugerida(c,te,hoje=None,ec_atual=None)` | → `(ancora|None,desde|None,motivo)`; lê dias trabalhados e troca de ritmo/últimos plantões. | `_trabalhados` usa batidas apuráveis `app/escala/services/fase_na_tela.py:40-47`; principal `:50-95`; não grava. Registro próprio não encontrado `[nao verificado]`. |
| `pausa_nao_cadastrada(te,batidas_por_dia)` | → dict com pausa sugerida, evidência, segunda metade ou None; não escolhe toda escala. | Pura sobre timestamps recebidos; `app/escala/services/pausa.py:29-49`; coletor turnos→timestamps `:52-58`. Registro próprio não encontrado `[nao verificado]`. |

**Cadastro x realidade:** `lista(hoje=None,empresas=None,*,agora=None)` retorna dict ou None; lê a lavra do esmeril e `destino`, sem matcher próprio nesse ponto (`app/escala/services/cadastro_realidade.py:710-717`, `:802-817`). O destino foi calculado por `esmeril_espelho.anomalias_da_frota`, que chama `propor_escala(c,hoje-60d,hoje-1d)` (`app/ponto/services/esmeril_espelho.py:206-223`); `_destino` formata (`:234`), `gravar_lavra` escreve snapshot (`:317-330`) e `ler_lavra` lê banco (`:334-350`).

**Complemento regeneração:** campos `regenerada_em`, `regeneracoes`, `dna_anterior` em `app/escala/models.py:1718-1729`; `dna_anterior` só conserva imediatamente anterior (`:1714-1717`). Escritor canônico `ponto.portas.celula.regenerar_celulas_vinculo(...)`:110-112 → número de células alteradas (`:118`); salva DNA anterior, timestamp e incrementa contador (`:263-277`). A função antiga em `gerar_celulas.py` é import/reexport dessa porta (`app/ponto/management/commands/gerar_celulas.py:13`). Isso prova **reescrita**, não prova que qualquer órfã de ata foi causada pela reescrita; atribuição dos 40 dias requer detalhe por célula/DNA/ata `[nao verificado]`.

## 06/11 17:51 · sessao 67ab93825034 · - · - · origem=Ronald (tmux)

1. **Ata e consulta.** A ata é `escala.models.CelulaDia.ata`, JSON do julgamento (`app/escala/models.py:1774-1778`), com uma única célula por `(colaborador,data)` (`app/escala/models.py:1833`). `ata_do_dia(dia, marcos=None, dna=None)` copia `dia['orfas']` em `ata['orfas']` e grava a contagem em `ata['n_orfas']` (`app/ponto/services/cartorio.py:696-711`). A porta `lavrar_veredito` persiste a ata inclusive quando um veredito administrativo impede substituir o veredito (`app/ponto/portas/celula.py:995-1004`, `:1027-1035`).

   A consulta para a **última ata persistida**, por empresa, é:

   ```python
   ini, fim = janela_fechamento(11, 2026, empresa)
   qs = CelulaDia.objects.filter(
       colaborador__empresa=empresa,
       data__range=(ini, fim),
       ata__n_orfas__gt=0,
   )
   ```

   A janela vem de `janela_fechamento` e `Empresa.dia_inicio_competencia`, não de uma constante da sonda (`app/ponto/janelas.py:36-57`; `app/colaboradores/models.py:46`). Não há campo de vigência/retratação da ata nem versões concorrentes nessa entidade; a célula carrega a ata corrente (`app/escala/models.py:1681-1817`, `:1833`). A consulta **não certifica que ela já incorporou todos os insumos atuais**: o leitor `grade_da_celula(colab,ini,fim)` lê `cel.ata` diretamente (`app/escala/services/leitor_celula.py:345-362`). O cartório testa atualização internamente pela impressão dos insumos (`app/ponto/services/cartorio.py:1348-1358`); `houve_fato_novo(...)` deliberadamente exclui chamado/fio e não equivale a esse teste integral (`app/ponto/services/cartorio.py:329-367`).

   Retratação é filtro da **batida**, pela autoridade `batidas_apuraveis(colab,ini=None,fim=None)` — `retratada_em IS NULL` (`app/ponto/turnos.py:2844-2871`). Não deve ser inventado filtro homônimo na célula (`app/escala/models.py:1681-1817`).

   Leitores concretos: `_orfas_da_ata` repassa horas e usa `'?'` somente em ata antiga com contagem sem lista (`app/escala/services/leitor_celula.py:175-185`); `grade_da_celula` transporta isso ao dict-dia (`:401-411`); `periodos_do_dia` lê a lista da ata (`app/ponto/juiz_batida.py:149-164`).

2. **Texto “espúria”.** A busca em todos os templates encontrou o literal somente no calendário: `title="Batida fora do turno previsto (espúria) — não casa nenhum marco; conferir."`, condicionado a `b.orfa` (`app/templates/colaboradores/partials/_calendario_grade.html:121`). O calendário obtém órfãs por `grade_da_celula` (`app/colaboradores/services/calendario.py:332-381`) e marca `b.orfa=True` quando a hora local da batida pertence à lista lida (`:621-627`); esse caminho lê a ata (`app/escala/services/leitor_celula.py:401-403`).

   No template efetivamente chamado `espelho.html`, a órfã aparece como `• HH:MM`, sem a palavra “espúria” (`app/templates/ponto/espelho.html:240-241`). O serviço normalmente lê `grade_da_celula` na competência (`app/ponto/services/espelho.py:518-527`), mas possui caminho pelo builder antes da competência e degradação por ata agregada (`:528-550`). `montar_realizado_grade` apenas transporta a lista de órfãs (`app/escala/utils.py:1099-1103`, `:1131`). Logo, “o ESPELHO escreve literalmente espúria” não foi confirmado nesta cópia [nao verificado].

4. **Célula refeita.** Os campos são `regenerada_em`, `regeneracoes` e `dna_anterior`; este último guarda somente o DNA imediatamente anterior (`app/escala/models.py:1704-1729`). A regeneração reescreve `trabalha`, vínculo gerador e DNA quando há diferença, guarda o DNA antigo e incrementa o contador (`app/ponto/portas/celula.py:255-277`). Ela depois dispara o julgamento do chão alterado (`:352-374`).

   Há dois significados diferentes de órfã: batida sem marco na ata, produzida pelo alinhamento (`app/escala/utils.py:585-665`), e **cobrança órfã de regeneração**, cujo marco foi cobrado antes da reescrita (`app/ponto/portas/celula.py:282-289`). Para a segunda existe `cobranca_orfa(janelas,chamado,regenerada_em=None,editada_em=None) -> instante | None`, dependente de `marcos_faltantes` e reescrita posterior à cobrança (`app/escala/services/regeneracao.py:141-155`). Regeneração não implica automaticamente batida órfã; os “40” do dado não permitem comprovar essa causalidade individual [nao verificado].

5. **Marco versus tipo real.** A ata guarda em cada elemento de `ata['lampadas']` as chaves `tipo` (marco), `hora` (prevista), `acesa`, `luz` (hora real) e `tipo_real` (tipo gravado da batida) (`app/ponto/services/cartorio.py:677-690`). O casamento por proximidade no ramo sem coluna grava `tipo_real=None`, explicitamente desconhecido (`:624-640`).

   Funções existentes que comparam:

   - `periodos_do_dia(celula,tipo_escala) -> dict`: retorna `papeis[].tipo_divergente`, calculado para lâmpada acesa com tipo real conhecido diferente do tipo esperado no DNA (`app/ponto/juiz_batida.py:149-164`, `:238-247`). É juiz registrado (`app/core/juizes.py:1173-1175`), somente leitura, sem motor (`app/ponto/juiz_batida.py:153`). Para contar dias pela função real, contar uma vez cada célula com algum `p['tipo_divergente']` no retorno; separar `desalinhado` e tipos desconhecidos (`:195-204`, `:228-247`). Atenção: a comparação usa o tipo esperado do **DNA**; a validação com a ata confere hora, não explicitamente o tipo (`:228-247`).
   - `montar_realizado_grade(gd,marcos,chamados_dia=None) -> (real,orfas)`: devolve `real[].divergente` comparando tipo real ao tipo do marco; desconhecido não acusa (`app/escala/utils.py:1099-1103`, `:1123-1131`).
   - `dia_degenerado(dia) -> (bool,motivo|None)` acusa `TIPO_INCOERENTE`, mas sua precedência retorna antes para cluster, órfãs e contagem incompatível; portanto não serve para contar **todas** as divergências de tipo (`app/ponto/propositor.py:34-73`).

   Uma batida casada ocupa marco e deixa de ser órfã pelo próprio alinhamento (`app/escala/utils.py:245-247`, `:656-662`); assim o censo de órfãs, isoladamente, não mede tipos divergentes nas lâmpadas acesas (`app/ponto/services/cartorio.py:677-711`). Quantidade real de dias nessa condição não consta do dado [nao verificado].

**Não encontrei escrito:** função pública pura que certifique “ata vigente e atualizada” integralmente; o teste `_em_dia` está dentro do julgamento (`app/ponto/services/cartorio.py:1199`, `:1348-1358`). Também não encontrei o literal “espúria” no template do espelho; encontrei somente a apresentação com ponto e a frase do calendário (`app/templates/ponto/espelho.html:240-241`; `app/templates/colaboradores/partials/_calendario_grade.html:121`).

## 06/11 17:49 · sessao be1b981c644a · O10-C · leitura · origem=Code

HASH LIDO = 40019f43b68afe3e4270634d6f251eb3af9440f0 [nao verificado]

| id | regra de origem (L-NNN, corte literal do aval ou arquivo:linha da regra) | entrada (model e campos, cada campo com arquivo:linha da definicao) | esperado OBSERVAVEL (o que um teste ve: retorno, linha gravada, status, texto) | mutante que tem de morder (a alteracao de codigo que este caso pega) | tipo golden/borda/adversarial | [nao verificado]
|---|---|---|---|---|---|
| C01 | `app/ponto/management/commands/trocas_plantao_lote_o10c.py:37`: “DRY (sem flag): so le. A porta NAO e chamada”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`), `documento='cedente.pdf'` (`app/ponto/models.py:777`). Sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`) e sem `--aplicar` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`). | Zero escritas de células, trilhas e lista administrativa; nenhuma chamada a `trocar_plantao`. | Depositar caso administrativo durante o DRY ou chamar a porta para depois desfazer a transação. | golden |
| C02 | Aval fornecido: “cada um vai a lista do admin COM a porta da troca ja aberta no dia (quem cobriu + os 2 anexos obrigatorios), e o apply do lote aplica 0 pares sem erro”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`), `documento='cedente.pdf'` (`app/ponto/models.py:777`). Sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`); `--aplicar --por=ator` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:175`). | Zero pares; rc 0, sem erro; zero escritas de `CelulaDia` e zero trilhas de edição/troca; uma linha persistida na lista do admin por SOLO. Deve haver acesso à troca daquele dia, com indicação de quem cobriu e duas cartas. URL, status HTTP, controles e critério executável de “alcançável”: **regra nao decide**. | Apenas imprimir o SOLO no terminal; tratar ausência de substituto como erro; aplicar uma edição unilateral. | golden |
| C03 | Corte fornecido, mínimo (3): “rodar 2x -> ainda 1 linha por SOLO (idempotencia)”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`). Executar duas vezes `--aplicar --por=ator` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:175`), sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Após a segunda execução, ainda exatamente uma linha administrativa para esse SOLO; zero pares e zero escritas de células nas duas execuções. | Criar uma linha administrativa nova incondicionalmente em cada execução. | golden |
| C04 | Aval fornecido: “A comp 09 e exportada: nada se grava nela sem o ! proprio, depois do DRY publicado”. `app/ponto/portas/celula.py:548`: dia com TXT emitido não se edita. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-09-10` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); `ExportacaoDominio.empresa=E` (`app/folha/models.py:77`), `mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`). Apply sem par e sem `!`. | Zero pares; nenhuma edição de célula na competência 09. O alcance integral de “gravar na competência”, incluindo se o depósito administrativo é abrangido pela proibição: **regra nao decide**. O aval exige também uma linha por SOLO, mas não explicita essa conciliação. | Considerar o depósito SOLO autorização suficiente para editar o dia exportado ou abrir a guarda automaticamente. | borda |
| C05 | Aval fornecido: “o lado do substituto vem do ADMIN pela porta da troca (O10-B), nunca deduzido”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`). Existe somente um colega candidato: `EscalaColaborador.colaborador=col02` (`app/escala/models.py:872`), `posto=P` (`app/escala/models.py:873`), `data_inicio=2026-10-01` (`app/escala/models.py:875`), `data_fim=2026-10-31` (`app/escala/models.py:876`); o vínculo de col01 tem o mesmo `posto=P` (`app/escala/models.py:873`). Apply sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Ainda zero pares e zero escritas de células; uma linha administrativa para o SOLO. A unicidade do candidato não fornece o substituto. | Escolher automaticamente o único colega do mesmo posto/dia. | adversarial |
| C06 | `app/ponto/tests/test_o10c_lote_trocas.py:6`: aprovado, par declarado, duas cartas e dia fora de lavrada vira par pela porta. `app/ponto/portas/celula.py:635`: cada carta determina o dia de seu lado. | Fixture sintética: duas `Ausencia`, `colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento='cedente.pdf'/'substituto.pdf'` (`app/ponto/models.py:777`). Cedente: `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `status='aprovada'` (`app/ponto/models.py:799`). Vínculos válidos; competência não exportada. `--par=C:S` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`; identificadores: [nao verificado]), com apply. | Uma troca confirmada: retorno da porta `trocada`; dia do cedente com `trabalha=False`, dia do substituto com `trabalha=True`, ambos com origem de edição humana e trilha. Substituto recebe dia normal do próprio template; nenhum desconto novo pela troca. | Fazer o tratamento SOLO interceptar também o par declarado; inverter os lados; tornar o dia assumido folga trabalhada. | golden |
| C07 | `app/ponto/janelas.py:11`: competência termina no mês indicado; `app/ponto/janelas.py:13`: corte 21 abrange 21 do mês anterior até 20 do mês. Aval: “A comp 09 e exportada”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-08-21` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); exportação de setembro: `ExportacaoDominio.mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`), `empresa=E` (`app/folha/models.py:77`). Apply sem par. | O dia pertence à competência 09 e não pode ter sua célula editada sem o ato próprio. Depósito administrativo dentro do alcance de “nada se grava nela”: **regra nao decide**. | Consultar exportação pelo mês civil de agosto ou excluir o primeiro dia da janela exportada. | borda |
| C08 | `app/ponto/janelas.py:13`: competência com corte 21 termina no dia 20. Aval: “A comp 09 e exportada”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-09-20` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); `ExportacaoDominio.empresa=E` (`app/folha/models.py:77`), `mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`). Apply sem par. | O último dia ainda pertence à competência 09; zero edições de célula sem o ato próprio. Alcance da proibição sobre a linha administrativa: **regra nao decide**. | Usar limite final exclusivo e liberar indevidamente o dia 20. | borda |
| C09 | `app/ponto/janelas.py:12`: janela começa no corte do mês anterior. Aval: “cada um vai a lista do admin”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-09-21` (`app/ponto/models.py:750`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`); somente a competência 09 está exportada na fixture: `ExportacaoDominio.mes=9` (`app/folha/models.py:78`), `ano=2026` (`app/folha/models.py:79`). Apply sem par. | O dia pertence à competência 10; a exportação da 09 não o inclui. Zero pares, zero edições de células e uma linha administrativa. | Bloquear todo setembro por mês civil ou incluir o dia 21 na competência anterior. | borda |
| C10 | `app/ponto/tests/test_o10b_editar_dia_troca.py:252`: turno que cruza meia-noite fica no dia de início. `app/ponto/tests/test_o10b_editar_dia_troca.py:7`: dia assumido normal, sem HE100 por ter sido folga. | Fixture sintética com par declarado: duas `Ausencia.colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-20` (`app/ponto/models.py:750`), `documento='cedente.pdf'/'substituto.pdf'` (`app/ponto/models.py:777`). Template do substituto: `TipoEscala.hora_inicio=19:00` (`app/escala/models.py:50`), `hora_fim=07:00` (`app/escala/models.py:51`). Competências não exportadas; confirmação pela porta (`app/ponto/portas/celula.py:632`). | A troca edita os dias de 20/10 indicados nas cartas. O turno assumido permanece no dia 20, com marcos 19:00→07:00; terminar em 21/10 não cria outra troca nem converte o turno em folga trabalhada/HE100. | Deslocar o dia assumido para a data da saída ou criar uma segunda edição no dia 21. | borda |
| C11 | `app/ponto/janelas.py:11`: competência pelo mês/ano do fim. `app/ponto/portas/celula.py:635`: cada carta determina seu próprio dia. | Fixture sintética: `Ausencia` cedente com `colaborador=col01` (`app/ponto/models.py:743`), `data=2026-12-20` (`app/ponto/models.py:750`); substituto com `colaborador=col02` (`app/ponto/models.py:743`), `data=2026-12-21` (`app/ponto/models.py:750`); ambas com `documento` preenchido (`app/ponto/models.py:777`). `Empresa.dia_inicio_competencia=21` (`app/colaboradores/models.py:46`). Par declarado, vínculos válidos, competências não exportadas. | Cedente editado em 20/12/2026, competência 12/2026; substituto editado em 21/12/2026, competência 01/2027. Os dois lados mantêm suas datas distintas. | Copiar a data do cedente para o substituto ou calcular a competência seguinte sem mudar o ano. | borda |
| C12 | `app/ponto/portas/celula.py:510`: DNA e marcos vêm do vínculo que responde pela data. `app/escala/alimentacao.py:232`: a geradora da célula é a autoridade. | Fixture sintética com par confirmado em 15/10: `Ausencia.data=2026-10-15` (`app/ponto/models.py:750`). Substituto: `EscalaColaborador` antigo com `colaborador=col02` (`app/escala/models.py:872`), `data_inicio=2026-10-01` (`app/escala/models.py:875`), `data_fim=2026-10-20` (`app/escala/models.py:876`), `tipo_escala=T1` (`app/escala/models.py:874`); novo vínculo desde 21/10 com `tipo_escala=T2` nos mesmos campos. `CelulaDia.colaborador=col02` (`app/escala/models.py:1681`), `data=2026-10-15` (`app/escala/models.py:1685`), `escala_geradora=antigo` (`app/escala/models.py:1692`). | A célula assumida usa o vínculo antigo e os marcos de T1. A existência de T2 como vínculo atual não substitui a autoridade daquele dia. | Usar o vínculo mais recente/ativo no lugar do vínculo do dia. | borda |
| C13 | `app/ponto/management/commands/trocas_plantao_lote_o10c.py:8`: universo composto pelas `Ausencia` do tipo legado; `app/ponto/management/commands/trocas_plantao_lote_o10c.py:37`: DRY somente lê. | Fixture sintética: consulta sem nenhuma `Ausencia.tipo='troca_de_plantao'` (`app/ponto/models.py:749`). Execução DRY, sem `--aplicar` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`). | Universo vazio: zero pares, zero linhas administrativas novas e zero escritas de células/trilhas. Texto exato da saída e rc para universo vazio: **regra nao decide**. | Criar um caso administrativo genérico mesmo sem lançamento de origem. | borda |
| C14 | `app/ponto/portas/celula.py:639`: falta de qualquer carta exige recusa antes de escrever. | Fixture sintética: duas `Ausencia.colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`). Cedente com `documento='cedente.pdf'`; substituto com `documento=''` (`app/ponto/models.py:777`). Confirmar pela porta, com `autor` e `motivo` (`app/ponto/portas/celula.py:632`). | Retorno `recusada`; zero alterações nos dois dias e zero trilhas de edição/troca. A carta do cedente não supre a do substituto. | Validar apenas o documento do cedente ou aceitar uma única carta para o par. | borda |
| C15 | `app/ponto/tests/test_o10b_editar_dia_troca.py:315`: duas confirmações concorrentes; a segunda espera a primeira e lê seu estado, resultando em no-op. `app/ponto/portas/celula.py:642`: segunda confirmação retorna `igual`, sem escrita. | Fixture sintética: mesmas duas `Ausencia`, `colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento='cedente.pdf'/'substituto.pdf'` (`app/ponto/models.py:777`). Duas confirmações simultâneas do mesmo par pela porta (`app/ponto/portas/celula.py:632`), inicialmente ainda não aplicado. | Uma confirmação retorna `trocada`; a seguinte retorna `igual`. Estado final de um único par; a segunda não altera relógios nem acrescenta trilhas. | Ler o estado antes da sincronização e repetir a escrita/trilha com base nessa leitura antiga. | adversarial |
| C16 | Aval: “o apply do lote aplica 0 pares sem erro”. Corte fornecido, mínimo (3): “rodar 2x -> ainda 1 linha por SOLO (idempotencia)”. | Fixture sintética: uma `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`). Dois applies simultâneos (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`), ambos sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Ambos continuam sem autorização para formar par: zero pares e zero edições de células. O mínimo (3) exige uma linha na repetição, mas a semântica específica de depósito concorrente, espera, conflito e recuperação: **regra nao decide**. | Fazer a segunda execução transformar o SOLO já depositado em par automático. Para duplicação causada exclusivamente pela corrida, falta oráculo concorrente explícito. | adversarial |
| C17 | `app/ponto/portas/celula.py:633`: duas edições no mesmo atomic. `app/ponto/portas/celula.py:640`: “nunca meio par”. | Fixture sintética: duas `Ausencia.colaborador=col01/col02` (`app/ponto/models.py:743`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento` preenchido (`app/ponto/models.py:777`). Células iniciais: `CelulaDia.trabalha=True/False` (`app/escala/models.py:1686`), `origem='gerada'` (`app/escala/models.py:1691`). Provocar falha entre as duas edições durante a confirmação (`app/ponto/portas/celula.py:632`). | Nenhum meio par persistido: os dois estados iniciais permanecem e nenhuma trilha de edição/troca da tentativa fica gravada. Tipo exato do erro ou retorno dessa falha: **regra nao decide**. | Confirmar a transação do primeiro lado antes de editar o segundo. | adversarial |
| C18 | Aval fornecido: “admin so recebe caso com porta”. | Fixture sintética: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`). Interrupção entre depósito e disponibilização da porta. Se a lista usar a candidata `Pauta`: `ancora_tipo='dia'` (`app/pautas/models.py:83`), `ancora_id='col01:2026-10-15'` (`app/pautas/models.py:84`). Associação formal SOLO→Pauta: [nao verificado]. | O admin não pode receber como caso pronto uma linha cuja porta inexiste. Como verificar alcance, o que permanece após a interrupção e como a execução retoma: **regra nao decide**. | Expor primeiro uma linha administrativa sem porta e considerá-la concluída para fins de depósito. | adversarial |
| C19 | Aval fornecido: “Os 16 lancamentos SOLO nao viram par por lote: cada um vai a lista do admin”; “substitui qualquer versao anterior”. | Fixture sintética de legado: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='rejeitada'` (`app/ponto/models.py:799`). Apply sem par (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | Zero pares, zero escritas de células e uma linha administrativa para esse SOLO. O aval novo não estabelece exceção de depósito por rejeição. | Reutilizar a classificação antiga como filtro que descarta o SOLO rejeitado antes de depositá-lo. | adversarial |
| C20 | Aval: “cada um vai a lista do admin”. `app/ponto/portas/celula.py:635`: as duas cartas chegam com `documento`; `app/ponto/portas/celula.py:639`: faltar carta recusa a confirmação. | Fixture sintética de legado: `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `documento=''` (`app/ponto/models.py:777`), `documento_referencia=outra_ausencia_com_documento` (`app/ponto/models.py:791`). Apply sem par. | Uma linha administrativa e zero pares. Se depois houver confirmação com esse lado ainda sem seu `documento`, a porta recusa antes de escrever. Equivalência entre documento referenciado e carta obrigatória do lado: **regra nao decide**; não há autorização explícita para dispensar a carta própria. | Descartar o SOLO por não possuir carta; ou considerar a mera referência autorização para aplicar a troca sem exigir as duas cartas. | adversarial |
| C21 | Aval fornecido: “Os 16 lancamentos SOLO […] cada um vai a lista do admin”. Corte fornecido: “1 linha […] por SOLO”. | Fixture sintética com dois lançamentos distintos do mesmo dia: ambas `Ausencia.colaborador=col01` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`); uma com `status='rejeitada'`, outra com `status='aprovada'` (`app/ponto/models.py:799`). Apply sem pares. | Duas linhas administrativas, uma por lançamento SOLO; zero pares. A repetição mantém uma linha por lançamento. O precedente de dedup por dia não altera o quantificador “cada um” do aval. | Deduplicar somente por colaborador/data e fazer um lançamento ocultar o outro. | adversarial |
| C22 | Aval fornecido: “Os 16 lancamentos SOLO nao viram par por lote: cada um vai a lista do admin […] o apply do lote aplica 0 pares sem erro”. | Lote **sintético**, não reconstrução dos registros de produção: 16 `Ausencia`, `colaborador=col01…col16` (`app/ponto/models.py:743`), `tipo='troca_de_plantao'` (`app/ponto/models.py:749`), `data=2026-10-15` (`app/ponto/models.py:750`), `status='aprovada'` (`app/ponto/models.py:799`), `documento` preenchido (`app/ponto/models.py:777`). Apply sem `--par` (`app/ponto/management/commands/trocas_plantao_lote_o10c.py:174`, `app/ponto/management/commands/trocas_plantao_lote_o10c.py:172`). | rc 0; zero pares e zero escritas de células; 16 linhas administrativas, uma por SOLO. Na segunda execução, continuam 16. Critério executável para alcance das 16 portas: **regra nao decide**. | Criar uma única linha agregada para o lote; interromper no primeiro SOLO; formar pares entre os próprios lançamentos. | golden |

- **regras que voce NAO encontrou escritas — alcance da porta:** procurei URL, formulário, ação, permissões e resposta observável que comprovem “porta já aberta/alcançável”. O aval exige o resultado, mas não define esse protocolo; a assinatura está em `app/ponto/portas/celula.py:632`. [nao verificado]
- **regras que voce NAO encontrou escritas — escrita na competência:** procurei a enumeração das entidades abrangidas por “nada se grava nela” e a conciliação com o depósito administrativo de SOLO exportado. A guarda escrita trata da edição de células em `app/ponto/portas/celula.py:548`.

[truncado em 20000 de 21876 caracteres]

## 06/11 17:45 · sessao 17c10b2746e0 · - · - · origem=Ronald (tmux)

- **Porta e atomicidade:** `app/ponto/portas/celula.py:633` declara par no mesmo atomic e retornos `trocada|igual|recusada`. `:635` diz que cada carta determina o dia do próprio lado; `:636` determina cedente como folga sem desconto; `:637` determina substituto como dia normal do próprio template. `:639` exige recusa antes de escrever quando falta lado, são o mesmo colaborador ou falta carta. `:640` determina rollback do outro lado se houver competência lavrada ou ausência de vínculo, “nunca meio par”. `:641` determina trilha somente quando algum lado mudou; `:642` determina segunda confirmação `igual`, sem escrita.
- **Estado intermediário:** `app/ponto/portas/celula.py:540` exige trilha com antes real/depois e motivo; `:541` determina que falha de trilha desfaça a edição. Falha entre lados tem caso explícito em `app/ponto/tests/test_o10b_editar_dia_troca.py:325`, com preservação das células e ausência de trilhas observadas em `:338`.
- **Concorrência:** regra explícita em `app/ponto/tests/test_o10b_editar_dia_troca.py:315`: “Duas confirmacoes concorrentes: a segunda espera a primeira e le o estado DELA (no-op).” `app/ponto/portas/celula.py:670` documenta ordem global de trava `(colaborador_id,data)`, independente do papel; o cenário cruzado está descrito em `app/ponto/tests/test_o10b_editar_dia_troca.py:443`.
- **Vínculo:** `app/ponto/portas/celula.py:510` determina vínculo escolhido para a data e marcos do template desse vínculo; `:537` reafirma DNA do vínculo do dia. `:550` determina `sem_vinculo` quando nenhum responde pelo dia.
- **Exportada:** `app/ponto/portas/celula.py:548` determina que dia em competência com TXT emitido não se edita, retorna `barrada` com frase em `barrados`; `:549` permite somente `apesar_da_lavra=<motivo>`. O lote declara nunca passar essa abertura em `app/ponto/management/commands/trocas_plantao_lote_o10c.py:46`.
- **Pauta:** `app/pautas/models.py:9` declara pauta ancorada e abertura do objeto; `:33` define âncora dia `<colab_id>:<AAAA-MM-DD>`. `app/pautas/services.py:156` declara `escrever` como único caminho de escrita. Precedente de dedup por todo histórico, incluindo feitas/canceladas, é específico do limbo, em `app/chamados/services/fabrica_da_celula.py:228`; não constitui regra escrita de dedup de SOLO.
- **DRY:** `app/ponto/management/commands/trocas_plantao_lote_o10c.py:37` determina somente leitura e nenhuma chamada à porta. `app/ponto/tests/test_o10c_lote_trocas.py:14` também determina nenhuma escrita sem `--aplicar`.
- **Par declarado:** regra normativa anterior preservável em `app/ponto/tests/test_o10c_lote_trocas.py:6`: aprovado, colega declarado, duas cartas e fora de lavrada vira par pela porta; cedido folga e assumido normal. `:12` determina segunda rodada sem nova escrita; `:13` proíbe desconto novo.

Definições dos campos:

| Modelo | Campos com definição |
|---|---|
| `Ausencia` | `colaborador` `app/ponto/models.py:743`; `tipo` `:749`; `data` `:750`; `data_fim` `:751`; `hora_ini` `:768`; `hora_fim` `:770`; `descricao` `:776`; `documento` `:777`; `documento_referencia` `:791`; `status` `:799`; `registrado_por` `:801` |
| `CelulaDia` | `colaborador` `app/escala/models.py:1681`; `data` `:1685`; `trabalha` `:1686`; `origem` `:1691`; `escala_geradora` `:1692`; `dna` `:1697`; `editada_em` `:1732`; `editada_por` `:1733` |
| `Pauta` | `destinatario_departamento` `app/pautas/models.py:76`; `destinatario_user` `:78`; `ancora_tipo` `:83`; `ancora_id` `:84`; `texto` `:89`; `feito_em` `:93`; `cancelado_em` `:97`; `pai` `:101` |
| `LogAuditoria` | `usuario` `app/core/models.py:295`; `acao` `:296`; `modelo` `:297`; `objeto_id` `:298`; `descricao` `:299`; `valor_antes` `:300`; `valor_depois` `:301` |

Não encontrei regra escrita específica de SOLO para concorrência de depósito em Pauta, fechamento/cancelamento e reexecução, falha entre depósito e disponibilização da porta, chave por lançamento versus colaborador/dia, nem definição operacional de “porta já aberta/alcancável”; o aval fornecido exige o resultado, mas não detalha essas decisões. `[nao verificado]`

## 06/11 17:45 · sessao 9ad22bc8dbbb · - · - · origem=Ronald (tmux)

Regras escritas e campos conferidos, somente por leitura:

- Competência: `app/ponto/janelas.py:10` define competência pelo mês/ano do fim; `:12` manda começar no corte do mês anterior e terminar em corte−1; `:13` explicita 21→20. Campo `Empresa.dia_inicio_competencia`: `app/colaboradores/models.py:46`.
- Vínculo trocado: `app/ponto/portas/celula.py:507`–`:511` manda produzir DNA e marcos pelo vínculo que responde à data. `app/escala/alimentacao.py:231`–`:233` declara `CelulaDia.escala_geradora` como autoridade; `:244`–`:246` disciplina ausência de célula/geradora; `:249`–`:252` exclui vínculo desativado sem fim declarado.
- Exportada: `app/ponto/portas/celula.py:548`–`:550` estabelece que dia com TXT emitido não se edita, retorna `barrada`, registra frase e só abre por `apesar_da_lavra=<motivo>`. Isso descreve edição da célula; não define integralmente “nada se grava na competência”.
- Troca confirmada: `app/ponto/portas/celula.py:633`–`:642` exige duas edições no mesmo atomic; cada `Ausencia.data` é o dia daquele lado; cedente vira folga e substituto vira trabalho normal; recusa lado/carta ausente; nunca deixa meio par; repetição retorna `igual` sem escrita.
- Cross-meia-noite: caso normativo nomeado `test_B07_turno_que_cruza_a_meia_noite_fica_no_dia_em_que_comeca`, `app/ponto/tests/test_o10b_editar_dia_troca.py:252`; preservação dos marcos 19:00→07:00 em `:257`. O aval literal desse arquivo, `:4`–`:9`, exige dia assumido normal, sem HE100 por ter sido folga.
- Concorrência de confirmação: regra textual no docstring `app/ponto/tests/test_o10b_editar_dia_troca.py:315`: “Duas confirmacoes concorrentes: a segunda espera a primeira e le o estado DELA (no-op).”
- Estado intermediário: regra “nunca meio par” em `app/ponto/portas/celula.py:640`; caso de falha entre lados em `app/ponto/tests/test_o10b_editar_dia_troca.py:325`.
- Vazio: recusa lado ausente escrita em `app/ponto/portas/celula.py:639`; par inteiramente vazio exercitado em `app/ponto/tests/test_o10b_editar_dia_troca.py:304`.

Definições dos campos:

| Model | Campos e definição |
|---|---|
| `Ausencia` | `colaborador` `app/ponto/models.py:743`; `tipo` `:749`; `data` `:750`; `data_fim` `:751`; `documento` `:777` |
| `EscalaColaborador` | `colaborador` `app/escala/models.py:872`; `posto` `:873`; `tipo_escala` `:874`; `data_inicio` `:875`; `data_fim` `:876`; `ativa` `:877`; `data_ancora_colaborador` `:974` |
| `TipoEscala` | `hora_inicio` `app/escala/models.py:50`; `hora_fim` `:51`; `hora_inicio_intervalo` `:52`; `hora_fim_intervalo` `:53` |
| `CelulaDia` | `colaborador` `app/escala/models.py:1681`; `data` `:1685`; `trabalha` `:1686`; `origem` `:1691`; `escala_geradora` `:1692`; `dna` `:1697` |
| `ExportacaoDominio` | `empresa` `app/folha/models.py:77`; `mes` `:78`; `ano` `:79`; `gerado_por` `:81`; `nome_arquivo` `:82`; `hash_sha256` `:83` |

Não encontrei regra de atomicidade/idempotência concorrente do depósito SOLO em lista administrativa; a regra de concorrência localizada é da confirmação da troca. `[nao verificado]`

## 06/11 17:44 · sessao 99a3602610bb · PAUTA-08 · verificador · origem=Code

1. **O texto novo mede o estado atual, mas a afirmação “hash conferido intacto” pode ser persistida antes da conferência.** A cabeça é gravada dentro da transação por colaborador; a comparação final fica depois desses commits ([comando:198](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198), [comando:219](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219)).

| Afirmação de `_cabeca` | O que a sustenta / limite |
|---|---|
| Identidade `colNN` e competência | Montadas com PK, mês e ano recebidos ([comando:54](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54), [comando:67](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:67)). |
| “EXPORTADA (TXT vigente)” | Consulta `competencia_entregue` por empresa/competência; o juiz verifica registro vigente não invalidado ([comando:124](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:124), [export.py:581](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/folha/export.py:581)). |
| “PAGO = o gravado da folha de hoje” | Lê o `FechamentoMensal` atual, recortado pelos campos canônicos. Igualdade com o TXT exportado ou pagamento efetivo no Domínio: **não medida** ([comando:130](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130), [regen_exportada.py:77](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:77)). |
| “hash conferido intacto NESTE ato” | Compara hash inicial e final ([comando:120](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:219](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219)). Se divergir, lança erro **depois de gravar as pautas**; elas podem sobreviver com a afirmação não confirmada ([comando:189](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:189), [comando:221](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:221)). |
| “DEVIDO = o calculo de hoje, lido sem gravar” | Chama `somente_leitura=True`; o ramo retorna os campos calculados em vez de salvar o fechamento ([comando:135](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135), [fechamento.py:1112](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/fechamento.py:1112)). |
| “N campo(s) diferem” | Conta os campos selecionados por `_diferente`, excluindo mês/ano. Diferença numérica significa tolerância `> 0.015` ([comando:139](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139), [comando:174](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174), [regen_exportada.py:248](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:248)). |
| “os dois numeros vao abaixo” | Monta `campo: pago X -> devido Y`, distribui em filhas e as escreve ([comando:175](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175), [comando:202](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202)). |
| “A diferenca NAO e atribuida a causa neste ato” | Condiz com a saída: os itens contêm campo e valores, sem atribuição causal ([comando:66](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66), [comando:175](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175)). |
| “Conferir com o pago no Dominio” | Instrução humana; conferência **não medida** pelo comando ([comando:66](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66)). |
| “(L-092)” | Referência normativa, não medição ([comando:65](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:65)). |

“Intacto” significa igualdade do hash nos dois extremos, dentro dos campos cobertos; há exclusões explícitas, e o hash é MD5 truncado em 12 caracteres. Isso não demonstra história desde a exportação nem ausência de alterações intermediárias ([regen_exportada.py:37](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:37), [regen_exportada.py:56](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:56), [regen_exportada.py:74](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/portas/regen_exportada.py:74)).

2. **Sim, restam expressões equivalentes, com escopos distintos.**

- No comando: “sem tocar o gravado” na docstring; “antes e depois […] nao se moveu”; help “Nao toca o gravado”. Referem-se ao ato, sem demonstrar a história anterior ([comando:2](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:2), [comando:14](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:14), [comando:72](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:72)).
- Na docstring do teste: “PAGO […] L-092: nao se toca”; a propriedade descrita é hash antes/depois ([teste:2](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:2), [teste:13](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:13)).
- Em outro comando de `pautas/`: “O TXT exportado NAO foi tocado”, sobre **TXT**, além de “sem tocar o gravado” na docstring e no help ([pauta_folga_nao_definida_exportada.py:33](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:33), [mesmo arquivo:2](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:2), [mesmo arquivo:39](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:39)).
- O literal velho também permanece como texto rejeitado no comentário/assert do teste; não é uma afirmação emitida ao DP ([teste:123](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:123), [teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127)).

3. **O teste rejeita o texto velho literal, mas não qualquer variante histórica.** Ele apenas proíbe `NAO foi tocado` e exige `conferido intacto NESTE ato` ([teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127), [teste:128](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:128)).

Um mutante que passa por essas duas asserções: conservar o texto novo e acrescentar **“Desde a exportacao, o gravado permaneceu inalterado.”** Mantém a substring exigida, evita a proibida e volta a afirmar uma história que a comparação inicial/final não mede ([teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127), [teste:128](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:128), [comando:120](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120), [comando:219](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219)). Execução da suíte Django: **[nao verificado]**.

4. **Escritores e leitores localizados:**

| Quem | Operação sobre `Pauta.texto` |
|---|---|
| Comando → serviço | Cabeça e filhas passam por `_ps.escrever`; o serviço persiste `texto[:500]` ([comando:198](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:198), [comando:202](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:202), [services.py:185](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/services.py:185)). |
| Próprio comando | Localiza cabeça por prefixo; seleciona filhas por prefixo e compara textos completos; reversão verifica a marca inicial ([comando:234](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:234), [comando:241](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:241), [comando:268](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:268)). |
| Painel de pautas | Serializa cabeça/respostas; navegador mostra ambas por `textContent` ([views.py:40](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/views.py:40), [views.py:47](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/views.py:47), [_copiloto.html:405](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/templates/core/_copiloto.html:405), [_copiloto.html:424](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/templates/core/_copiloto.html:424)). |
| API/mensageria | API entrega texto literal; cliente busca o JSON e ferramenta repassa os itens ([views_mensageria.py:1334](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/api/views_mensageria.py:1334), [core_client.py:431](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/mensageria/nucleo/core_client.py:431), [ferramentas.py:2241](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/mensageria/nucleo/ferramentas.py:2241)). |
| Testes do comando | Inspecionam prefixo, valores das filhas e substrings da cabeça ([teste:105](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:105), [teste:108](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:108), [teste:127](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127)). |

**Não identifiquei leitor que quebre ao parsear a prosa alterada:** os reconhecedores desse comando usam marca/chave preservadas; UI e API exibem ou transportam o texto ([comando:41](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:41), [comando:54](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:54), [comando:234](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:234), [_copiloto.html:405](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/templates/core/_copiloto.html:405)). Existem parsers de outras famílias, protegidos por outros marcadores, como Esmeril e Discordantes ([pautas_do_esmeril.py:63](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/pautas_do_esmeril.py:63), [pautas_dos_discordantes.py:165](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/pautas_dos_discordantes.py:165)). Leitores externos: **[nao verificado]**.

**Cabeças antigas não são corrigidas pelo comando:** encontrar uma existente provoca `continue`; a comparação posterior considera apenas filhas. Assim, uma cabeça antiga pode conservar a frase velha; ocorrência real em produção: **[nao verificado]** ([comando:193](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193), [comando:241](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:241)).

5. **Sim: `LEIS.md:177` ainda contém uma generalização histórica contradita pelos docs.**

- **“Até 06/11 […] nenhum código criava”** conflita com o registro de **19/10** de Pauta DP com `pago -> devido` por campo. A cópia contém essa montagem e a escrita de cabeça/filhas em outro fluxo ([LEIS.md:177](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:177), [RELATO-ARQUIVO.md:43468](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/RELATO-ARQUIVO.md:43468), [RELATO-ARQUIVO.md:43472](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/RELATO-ARQUIVO.md:43472), [passivo_validado.py:192](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/chamados/services/passivo_validado.py:192), [passivo_validado.py:229](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/chamados/services/passivo_validado.py:229)). Esse fluxo retifica o gravado; é diferente do comparador somente leitura da PAUTA-08 ([passivo_validado.py:438](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/chamados/services/passivo_validado.py:438), [comando:135](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135)).
- **“Caminho AUTOMATICO segue sem codigo” precisa delimitar a família.** A autorização de HE já gera automaticamente Pauta DP em exportada após autorização, com pago da lavratura e devido do motor. Isso não implementa o comparador genérico da PAUTA-08 ([autorizacao_he_periodo.py:174](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/autorizacao_he_periodo.py:174), [mesmo arquivo:184](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/autorizacao_he_periodo.py:184), [mesmo arquivo:299](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/ponto/services/autorizacao_he_periodo.py:299)).
- **Há diferença de referência entre linhas da própria LEIS:** a linha 125 exige **exportado × motor de hoje**; a 177 descreve **gravado atual × recálculo**. O comando não demonstra equivalência entre gravado atual e exportado ([LEIS.md:125](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:125), [LEIS.md:177](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:177), [comando:130](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130)).
- A pauta manual é compatível com os cortes que encaminham diferenças ao DP e com a revogação da pauta como **pré-condição**: deixar de exigir previamente não impede sua emissão ([CORTES.md:353](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/CORTES.md:353), [LEIS.md:150](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:150), [LEIS.md:198](/home/ronald/codex_ws/2322d0801c7cecd6728a1bd6c242ca19bb3117e0/app/docs/LEIS.md:198)).

## 06/11 17:42 · sessao eb897eabe44c · - · - · origem=Ronald (tmux)

2. Restam expressões equivalentes, mas com escopos diferentes:

- `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:2`: “sem tocar o gravado”; `:14`: hash “antes e depois”, prova de que “nao se moveu”; `:72`: help “Nao toca o gravado”. São alegações sobre o ato, não prova da história desde o export (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:219`).
- `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:2`: docstring “PAGO (... L-092: nao se toca)”. A mesma docstring explicita hash antes/depois em `:13`.
- Outro comando conserva literalmente “O TXT exportado NAO foi tocado”, referente ao TXT: `app/pautas/management/commands/pauta_folga_nao_definida_exportada.py:33`. Também conserva “sem tocar o gravado” na docstring `:2`, exclusão de escritas em `:9` e help em `:39`.
- A ocorrência “nao tocada(s)” em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:277` refere-se às pautas alheias puladas na reversão (`:268`), não ao gravado.
- O literal velho aparece no comentário e na asserção negativa do teste, não como afirmação ao DP: `app/pautas/tests/test_pauta_pago_x_devido_exportada.py:123` e `:127`.

5. Há problemas de escopo na linha alterada:

- **Contradição documental concreta:** “até 06/11 [...] nenhum codigo criava” (`app/docs/LEIS.md:177`) colide com o marco de **19/10** que registra Pauta DP com `pago -> devido` por campo (`app/docs/RELATO-ARQUIVO.md:43468`, `:43472`). Outro marco de 19/10 registra cabeça e filhas escritas pela mesma família (`app/docs/RELATO-ARQUIVO.md:43412`, `:43419`). O código da cópia confirma os pares em `app/chamados/services/passivo_validado.py:192` e a escrita em `:229` e `:233`; o chamador associa regeneração e pauta em `:438` e `:443`. Esses caminhos retificam o gravado; distinguem-se do comando PAUTA-08 (`app/chamados/services/passivo_validado.py:423`).
- **“Caminho AUTOMATICO segue sem codigo” precisa restringir a família:** a autorização de HE em exportada já chama a pauta quando houve autorização (`app/ponto/services/autorizacao_he_periodo.py:299`). Ela lê pago da lavratura e devido do motor (`:174`–`:176`), escreve os dois números no texto (`:184`) e grava Pauta DP (`:244`). Essa classe já está descrita em `app/docs/DOSSIES.md:279` e `:287`; não é o comparador genérico FM atual × recálculo somente_leitura.
- **Diferença de referência:** `app/docs/LEIS.md:125` define os dois números como **exportado** × motor de hoje; `:177` descreve **gravado atual** × recálculo. O comando lê FM atual (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`), portanto não demonstra equivalência com o exportado.
- A existência da pauta manual não contradiz a revogação de sua exigência como **pré-condição** em `app/docs/LEIS.md:150` e `:198`. Os cortes que mandam encaminhar diferenças ao DP são compatíveis com essa saída (`app/docs/CORTES.md:353`, `:387`, `:402`).

## 06/11 17:41 · sessao f162274a47d4 · - · - · origem=Ronald (tmux)

Auditoria do contrato, por inspeção estática:

| Afirmação de `_cabeca` | Evidência e limite |
|---|---|
| Identidade `col<pk>` e competência | Montadas por `_chave`, sem medição histórica (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:53-54`, `:67`). |
| `EXPORTADA (TXT vigente)` | Consulta `competencia_entregue` por empresa e competência (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:123-125`); juiz verifica registro vigente e não invalidado (`app/folha/export.py:580-586`). Não verifica conteúdo físico do TXT nem pagamento efetivo no Domínio (`app/folha/export.py:580-586`). |
| `PAGO = o gravado da folha de hoje` | Lê `valores_do_gravado` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:130`), que consulta `FechamentoMensal` e recorta `CAMPOS` (`app/ponto/portas/regen_exportada.py:77-84`). Mede gravado atual; equivalência com dinheiro efetivamente pago é **não medida**. |
| `hash conferido intacto NESTE ato` | Compara hash inicial (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`) com final (`:219-222`). **Ressalva material:** persiste cabeça/filhas e encerra transação antes dessa conferência (`:189-209`); se hash divergir, lança erro depois dos commits, podendo sobreviver pauta com afirmação não confirmada (`:219-222`). |
| `DEVIDO = o calculo de hoje, lido sem gravar` | Chamada com `somente_leitura=True` (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:135`); ramo coleta dicionário em vez de salvar (`app/ponto/services/fechamento.py:1112-1128`), e `lavrar_dias_pagos` tem default falso (`:439-441`, `:1175`). |
| `N campo(s) diferem` | Lista por `_diferente`, excluindo `mes/ano`, e tamanho usado na cabeça (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:139`, `:174`). “Diferem” usa tolerância numérica `>0.015` (`app/ponto/portas/regen_exportada.py:248-252`). |
| `os dois numeros vao abaixo` | Gera itens pago/devido e filhas (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:175-177`); persiste filhas (`:201-204`). |
| `A diferenca NAO e atribuida a causa neste ato` | Texto não atribui causa; itens só contêm campo e valores (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66`, `:175-177`). Não existe medição causal nesse fluxo (`:120-140`). |
| `Conferir com o pago no Dominio` | Instrução humana, **não medida** pelo comando (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:66`). |
| `(L-092)` | Referência normativa, não medição (`app/pautas/management/commands/pauta_pago_x_devido_exportada.py:65`). |

O hash é MD5 truncado em 12 caracteres e cobre campos concretos menos exclusões explícitas, incluindo carimbos, identidade e aprovação; portanto “intacto” significa igualdade desse hash nos extremos, não ausência de qualquer escrita ou história desde a exportação (`app/ponto/portas/regen_exportada.py:35-38`, `:56-74`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`, `:219`).

O teste novo derruba o texto velho literal: proíbe `NAO foi tocado` e exige `conferido intacto NESTE ato`; não barra qualquer equivalente semântico (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127-128`). Mutante que passaria por essa lógica: conservar o cabeçalho atual e acrescentar **“O gravado permanece identico desde a exportacao.”** A nova frase evita o literal proibido e preserva o exigido, apesar de alegar história não medida (`app/pautas/tests/test_pauta_pago_x_devido_exportada.py:127-128`; `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:120`, `:219`). Execução do mutante: `[nao verificado]`.

## 06/11 17:41 · sessao a00820d944e2 · - · - · origem=Ronald (tmux)

**Quem escreve**

- O comando monta cabeça e filhas em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:174`, `:175` e `:177`; pede a escrita da cabeça com `texto=cab` em `:198`–`:199` e das filhas com `texto=f` em `:202`–`:204`.
- O autor vem de `autor_sistema(informado='ti')` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:188`. O service efetivamente cria a linha em `app/pautas/services.py:180`, gravando `texto=texto[:500]` em `:185`; o campo tem teto 500 em `app/pautas/models.py:89`.

**Quem lê**

- UI: `app/pautas/views.py:33` consulta as pautas; `:40` serializa a cabeça e `:46`–`:48` serializam as respostas. O navegador exibe os textos diretamente usando `textContent`, em `app/templates/core/_copiloto.html:405` e `:424`.
- API de mensageria: consulta a aba, normalmente DP, em `app/api/views_mensageria.py:1324`–`:1329` e entrega `p.texto` literalmente em `:1334`. `mensageria/nucleo/core_client.py:431`–`:438` obtém o JSON e `mensageria/nucleo/ferramentas.py:2241`–`:2248` repassa seus itens.
- O próprio comando lê o texto para localizar cabeça por `texto__startswith=chave + ' '` em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:233`–`:234`; seleciona filhas por prefixo e compara seus textos integralmente em `:241`–`:243`; na reversão exige `p.texto.startswith(MARCA)` em `:268`.

**Risco de parsing**

- Os leitores concretos acima não dependem da prosa alterada: os reconhecedores usam a marca e a chave definidas em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:41` e `:53`–`:54`, enquanto o delta muda o trecho posterior em `:64`–`:66`. UI e API transportam/exibem o texto diretamente (`app/pautas/views.py:40`; `app/templates/core/_copiloto.html:405`; `app/api/views_mensageria.py:1334`).
- Leitor externo ou em produção que aplique regex/split à prosa: **[nao verificado]**.

**Consequência adicional**

- A nova redação não corrige cabeças já existentes: a execução encontra a cabeça e continua sem reescrevê-la em `app/pautas/management/commands/pauta_pago_x_devido_exportada.py:193`–`:197`; a verificação de divergência compara somente filhas em `:241`–`:243`. Portanto uma cabeça antiga pode continuar exibindo a frase velha; existência efetiva em produção **[nao verificado]**.

## 06/11 17:35 · sessao d510127cbf40 · E-SEM-VIVA-B · verificador · origem=Code

**Reprovaria a volta 4.** O desligado sem demissão ainda pode entrar com fechamento preexistente; datas de férias passam como dias do retentor; e o vigia pode rotear outra competência apenas pela presença do colaborador na C5. `app/folha/export.py:775`, `app/escala/services/cadastro_realidade.py:184`, `app/folha/vigia_retido_calado.py:43`.

Os achados abaixo são de código. Ocorrência e quantidades em produção, execução da suíte Django e comparação integral com `24946096`: **[nao verificado]**.

1. **ALTA — a exclusão do desligado sem `data_demissao` depende de nunca ter existido fechamento.**

   O universo do fechamento exclui desligado sem data, mas `_K2` consulta os `FechamentoMensal` existentes diretamente, sem reaplicar esse universo. Com data nula, `sai_pela_rescisao` responde falso e o retentor não recorta o fim da janela. Portanto, um fechamento anterior à alteração cadastral pode produzir uma **lavra nova** com dias aceitos pela C5. `app/ponto/services/fechamento.py:423`, `app/ponto/services/fechamento.py:430`, `app/folha/export.py:775`, `app/folha/export.py:60`, `app/folha/export.py:1198`, `app/escala/services/cadastro_realidade.py:186`.

   O teste mascara esse histórico: apaga a data **antes** de criar fechamentos; `_fechar` só faz `get_or_create`. Ele prova ausência de fechamento naquela fixture, não exclusão de um desligado sem data que já possuía fechamento. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:140`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:60`.

   **Como provar:** no caso positivo existente, executar conceitualmente `self._fechar()`, depois retirar `data_demissao`, depois `self._lavrar()`. As células continuam acusadoras, o fechamento continua existente e a C5 aceita suas datas. Esse cenário repete a ampliação reprovada na volta 3; sua execução: **[nao verificado]**. `app/escala/models.py:1767`, `app/folha/export.py:1233`, `app/escala/services/cadastro_realidade.py:183`.

2. **MÉDIA — “datas de qualquer bloqueado” não equivale aos dias de `dias_que_seguram`.**

   A C5 aceita as datas de **todo** item do balde `bloqueado`; a autoridade `dias_que_seguram` só preenche seu mapa de dias para `furo_espelho`. `app/escala/services/cadastro_realidade.py:184`, `app/folha/fora_da_folha.py:128`, `app/folha/fora_da_folha.py:137`.

   O escritor legítimo também grava `datas` para `ferias_com_batida`. Logo, não é necessário fabricar JSON inválido para obter a divergência. `app/folha/export.py:868`, `app/folha/export.py:875`, `app/folha/retrato_folha.py:1172`, `app/folha/models.py:323`.

   **Como provar:** desligado com demissão depois do fim da competência, fechamento, férias com batida e uma célula ainda `nunca_bateu` naquele dia. O lavrador grava a data de férias; a C5 a aceita; `dias_que_seguram()['dias']` não contém o colaborador por esse motivo. A própria casa reconhece o intervalo entre primeira batida e rejulgamento de `nunca_bateu`; ocorrência desse caso específico: **[nao verificado]**. `app/folha/export.py:858`, `app/folha/retrato_folha.py:1175`, `app/escala/services/cadastro_realidade.py:188`, `app/chamados/services/adesao.py:59`.

3. **MÉDIA — o vigia perde competência e datas e pode reduzir `calados` além do aval.**

   `censo` transforma o retorno da C5 em um conjunto de IDs. Depois considera roteada qualquer linha bloqueada com fio `ninguem` daquele colaborador, em qualquer retrato selecionado da última lavra. `app/folha/vigia_retido_calado.py:38`, `app/folha/vigia_retido_calado.py:43`, `app/folha/vigia_retido_calado.py:46`, `app/folha/vigia_retido_calado.py:48`.

   **Como provar com as fontes reais:** colaborador desligado com demissão futura, um `nunca_bateu` retido na anterior e dias sem célula, mas cobertos por vínculo, na corrente. O retentor pode gerar `sem_celula` na corrente; sem célula, o fio resulta sem porta. A presença na C5 pela anterior roteia também a corrente, embora seus dias não estejam na C5. `app/folha/export.py:1227`, `app/folha/export.py:1231`, `app/folha/fora_da_folha.py:67`, `app/folha/fora_da_folha.py:78`, `app/folha/vigia_retido_calado.py:48`.

   O resultado condicional é `roteados=2` e `calados=0` para duas linhas antes caladas, apesar de só uma competência ter dias C5. Isso já era possível para ativos; o diff estende o alcance aos desligados. Ocorrência em produção: **[nao verificado]**. `app/folha/vigia_retido_calado.py:48`, `app/folha/vigia_retido_calado.py:53`, `app/escala/services/cadastro_realidade.py:188`.

4. **MÉDIA — o universo de empresas do leitor é maior que o do lavrador.**

   A C5 consulta todas as empresas ou os IDs recebidos. O lavrador parte de `empresas_visiveis()` inclusive quando recebe `--empresa`; essa fonte exclui empresas inativas e CNPJ começando por `11222333`. `app/escala/services/cadastro_realidade.py:169`, `app/folha/management/commands/lavrar_retrato_folha.py:49`, `app/folha/management/commands/lavrar_retrato_folha.py:51`, `app/colaboradores/queries.py:85`.

   **Como provar:** numa empresa excluída, manter fechamento e dia que a autoridade viva retém, sem retrato. A autoridade pode responder positivamente, mas C5 devolve ausência. Se houver retrato de quando a empresa era visível, C5 pode continuar usando uma lavra que o command já não atualiza. `app/folha/export.py:778`, `app/escala/services/cadastro_realidade.py:183`, `app/escala/services/cadastro_realidade.py:188`.

   Os CNPJs das fixtures novas não exercitam a exclusão `11222333*`; suas empresas são criadas ativas. A tela também usa `empresas_visiveis`, enquanto o vigia admite IDs diretamente. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:48`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:52`, `app/escala/views.py:487`, `app/folha/vigia_retido_calado.py:34`.

5. **MÉDIA — falta positivo de desligado retido na competência corrente.**

   O mutante plausível **M1** — trocar `for _ini, fim in janelas:` por `for _ini, fim in janelas[:1]:` — deixa de consultar o retrato corrente e preserva, por inspeção, os resultados esperados dos testes novos/alterados. Os positivos de desligados exigem somente dias da anterior; os ativos dispensam a lavra. Execução integral contra o mutante: **[nao verificado]**. `app/escala/services/cadastro_realidade.py:181`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:120`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:223`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:243`.

   **Como provar a lacuna:** desligado com demissão depois de 20/11, célula `nunca_bateu` em 23/10, fechamento e lavra da competência 11; exigir 23/10 na C5. Esse é o ramo que M1 elimina. `app/ponto/janelas.py:24`, `app/escala/services/cadastro_realidade.py:172`, `app/escala/services/cadastro_realidade.py:181`.

6. **MÉDIA — o selo não certifica a promessa que anuncia.**

   A C5 lê `situacao`, assim como `_K2` e o universo do fechamento; o campo foi omitido de `CAMPOS_DA_AUTORIDADE`. Portanto, “nenhum campo que o juiz lê é lido aqui” é falso pelo próprio código. Isso não significa que retirar a distinção ativo/desligado seria correto: ela sustenta a exceção aprovada para ativos. `app/escala/services/cadastro_realidade.py:151`, `app/escala/services/cadastro_realidade.py:178`, `app/folha/export.py:802`, `app/ponto/services/fechamento.py:431`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:260`.

   O detector só examina atributos e argumentos posicionais de algumas chamadas. Uma leitura por `filter(data_demissao__isnull=...)` não é detectada; helpers externos também ficam fora da função inspecionada. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:273`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:294`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:298`.

   Basta ainda existir uma chamada a `linhas_do_retrato`, mesmo com retorno descartado, para cumprir a exigência positiva. O selo não verifica que o resultado dessa chamada governa a seleção; o reconhecimento de código morto limita-se ao `if` com constante falsa. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:289`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:291`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:304`.

   **Como provar:** inserir uma consulta ORM ao campo proibido, ou manter uma chamada descartada à porta e selecionar por leitura paralela. Ambos satisfazem o detector atual; as sondas AST em memória confirmaram essas brechas. A suíte Django com esses mutantes: **[nao verificado]**. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:277`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:302`.

A equivalência universal perguntada no item 2 é, portanto, **não**. Além dos achados acima, há estes limites específicos:

| Caso | Resposta pelo código |
|---|---|
| Competência e corte da empresa | Leitor e lavrador usam mês/ano do **fim** de `janela_anterior` e `janela_atual`. Para o mesmo dia e empresa, não encontrei divergência de idioma, inclusive no dia do corte. `app/escala/services/cadastro_realidade.py:172`, `app/escala/services/cadastro_realidade.py:182`, `app/folha/management/commands/lavrar_retrato_folha.py:60`, `app/ponto/janelas.py:27`. |
| Lavra versus autoridade viva | A divergência é deliberadamente aceita pelo teste: mudar a demissão depois da lavra mantém os dias antigos até relavrar. Assim, a C5 não garante “antes da demissão atual”. `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:160`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:169`. |
| Readmissão | A reativação muda para ativo e limpa a demissão; C5 passa a devolver todos os candidatos, dispensando a lavra anterior. Preserva a exceção dos ativos, mas refuta uma descrição irrestrita de “somente a lavra”. `app/colaboradores/services/desligamento.py:210`, `app/colaboradores/services/desligamento.py:211`, `app/escala/services/cadastro_realidade.py:188`. |
| `colab` repetido | C5 une as datas de todas as linhas bloqueadas; `linha_do_colab` responde pela primeira linha encontrada. O JSON não impõe unicidade interna, mas o escritor normal deriva uma linha por fechamento/item. Duplicata real: **[nao verificado]**. `app/escala/services/cadastro_realidade.py:186`, `app/folha/retrato_folha.py:935`, `app/folha/models.py:348`, `app/folha/retrato_folha.py:1162`, `app/ponto/models.py:485`. |
| Datas não ISO | Não casam com `d.isoformat()`. O escritor normal produz `str(date)` e documenta ISO; portanto, não há evidência de formato alternativo legítimo no caminho examinado. Dados assim em produção: **[nao verificado]**. `app/escala/services/cadastro_realidade.py:188`, `app/folha/fora_da_folha.py:192`, `app/folha/retrato_folha.py:1175`, `app/folha/models.py:332`. |
| Bloqueado por outro motivo | Ausência de datas exclui; presença de datas inclui, independentemente de ser `furo_espelho`. É o contraexemplo de férias do achado 2. `app/escala/services/cadastro_realidade.py:184`, `app/escala/services/cadastro_realidade.py:186`. |

Quanto ao item 3, **não encontrei a recomposição paralela da volta 3 no novo corpo de produção**: ele lê `linhas_do_retrato`; não chama rescisão ou retentor. `_K2` continua registrado como `classificar_export`. O defeito está na equivalência assumida para o conteúdo lido e no estado atual usado para escolher quem passa por essa leitura. `app/escala/services/cadastro_realidade.py:162`, `app/escala/services/cadastro_realidade.py:184`, `app/core/juizes.py:299`, `app/docs/JUIZES.md:74`.

Também não há novo escritor persistente no corpo alterado de C5/vigia: são consultas, composição do retorno e contagens. As escritas diretas dos testes são fixtures. `app/escala/services/cadastro_realidade.py:173`, `app/folha/vigia_retido_calado.py:33`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:42`, `app/escala/tests/test_esemvivab_desligado_nunca_bateu.py:195`.

O command agora lê o dia na borda e o repassa. A assinatura permissiva de `censo` ainda permite omissão, que chega ao fallback de relógio de C5; esse fallback não nasceu neste diff. O lavrador e o retentor também conservam suas leituras próprias de relógio. `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/vigia_retido_calado.py:27`, `app/folha/vigia_retido_calado.py:43`, `app/escala/services/cadastro_realidade.py:167`, `app/folha/management/commands/lavrar_retrato_folha.py:48`, `app/folha/export.py:1197`.

**Rastreio dos escritores e leitores solicitados:**

| Campo/valor | Quem produz e escreve | Quem lê |
|---|---|---|
| `RetratoFolha.linhas` | `montar_linhas` monta; `lavrar_retrato_folha --apply` persiste por empresa/competência. `app/folha/retrato_folha.py:1156`, `app/folha/retrato_folha.py:1177`, `app/folha/management/commands/lavrar_retrato_folha.py:74`. | Porta `linhas_do_retrato`; `__str__` lê o tamanho. `app/folha/retrato_folha.py:110`, `app/folha/models.py:368`. |
| Chave `colab` | ID do item devolvido por `_K2`, gravado por `montar_linhas`. `app/folha/export.py:830`, `app/folha/retrato_folha.py:1163`, `app/folha/retrato_folha.py:1178`. | C5; vigia; ordenação; lookup do drawer; portas e tabela. `app/escala/services/cadastro_realidade.py:185`, `app/folha/vigia_retido_calado.py:48`, `app/folha/retrato_folha.py:756`, `app/folha/retrato_folha.py:936`, `app/folha/retrato_folha.py:1010`, `app/folha/views_retrato.py:272`. |
| Chaves `status`, `motivo` | `_K2` decide; `montar_linhas` copia. `app/folha/export.py:833`, `app/folha/export.py:859`, `app/folha/export.py:865`, `app/folha/export.py:870`, `app/folha/export.py:960`, `app/folha/retrato_folha.py:1180`. | Balde, fio, tipo, saldo, ação e apresentação. `app/folha/retrato_folha.py:91`, `app/folha/retrato_folha.py:441`, `app/folha/retrato_folha.py:491`, `app/folha/retrato_folha.py:501`, `app/folha/retrato_folha.py:1062`, `app/folha/views_retrato.py:284`. |
| Chave `datas` | Dias do retentor passam por `dias_que_seguram` e `fora_da_folha`; férias vêm do item de `_K2`; `montar_linhas` grava ambas as formas. `app/folha/export.py:1221`, `app/folha/fora_da_folha.py:137`, `app/folha/fora_da_folha.py:192`, `app/folha/export.py:875`, `app/folha/retrato_folha.py:1175`, `app/folha/retrato_folha.py:1183`. | C5; teto temporal de dono; recorte por período. `app/escala/services/cadastro_realidade.py:186`, `app/folha/retrato_folha.py:478`, `app/folha/retrato_folha.py:1233`. |
| `CelulaDia.veredito` | Método do modelo delega à porta; porta atribui e persiste. Chamadores: cartório e decisão administrativa. `app/escala/models.py:1819`, `app/ponto/portas/celula.py:814`, `app/ponto/portas/celula.py:819`, `app/ponto/services/cartorio.py:889`, `app/ponto/services/cartorio.py:1242`, `app/ponto/services/cartorio.py:1802`, `app/ponto/services/veredito_celula.py:330`. | Inventário abaixo. A cópia em memória também altera o valor, dentro da própria porta. `app/ponto/portas/celula.py:711`, `app/ponto/portas/celula.py:726`. |
| `Colaborador.situacao` | Default ativo; operação declarada na porta; desligamento, reativação, afastamento, reversão e admissão determinam os valores. `app/colaboradores/models.py:323`, `app/colaboradores/services/colaborador.py:32`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/services/desligamento.py:104`, `app/colaboradores/services/desligamento.py:210`, `app/ponto/views.py:2435`, `app/ponto/management/commands/reverter_situacao_afastado.py:73`, `app/colaboradores/views.py:1205`, `app/colaboradores/views.py:2610`. | Inventário abaixo; o admin exibe/filtra, mas declara o campo somente leitura. `app/colaboradores/admin.py:49`, `app/colaboradores/admin.py:71`. |

Os consumidores externos de `RetratoFolha.linhas` passam pela porta na tela do retrato, drawer do colaborador, vigia e nova C5. Internamente, a porta alimenta contadores, filtros, agrupamentos, ordenação, LEDs, ações e recorte temporal. `app/folha/views_retrato.py:196`, `app/colaboradores/views.py:982`, `app/folha/vigia_retido_calado.py:46`, `app/escala/services/cadastro_realidade.py:184`, `app/folha/retrato_folha.py:120`, `app/folha/retrato_folha.py:617`, `app/folha/retrato_folha.py:657`, `app/folha/retrato_folha.py:751`, `app/folha/retrato_folha.py:547`, `app/folha/retrato_folha.py:1028`, `app/folha/retrato_folha.py:1195`.

O retorno C5 tem este encadeamento de leitores, inclusive fora do diff:

| Retorno | Leitores |
|---|---|
| `colabs_nunca_bateu` | `_nunca_bateu` e `censo`. `app/escala/services/cadastro_realidade.py:197`, `app/folha/vigia_retido_calado.py:43`. |
| `_nunca_bateu` | `lista`. `app/escala/services/cadastro_realidade.py:829`. |
| `lista`, contendo C5 | Tela e PDF; command C7; command C8; `ListaDoAdmin` do vigia do fio. `app/escala/views.py:487`, `app/escala/views.py:489`, `app/escala/management/commands/fechar_avisos_regua_na_lista.py:27`, `app/ponto/management/commands/fechar_chamados_vinculo_na_lista.py:35`, `app/chamados/services/vigia_fio.py:216`. |
| Censo de retidos | Command imprime números e aplica o alarme. `app/folha/management/commands/vigiar_retido_calado.py:31`, `app/folha/management/commands/vigiar_retido_calado.py:35`. |

O inventário estático de acessos de produção a `CelulaDia.veredito`, incluindo leituras pelo próprio escritor e projeções do valor, é o seguinte; exaustividade de acessos dinâmicos: **[nao verificado]**.

```text
app/chamados/juizes.py:816,1132,1148,1181,1183
app/chamados/models.py:1161,1165,1166,1710,2790,2791,2792
app/chamados/reconciliador.py:627,629,631,997,1016
app/chamados/management/commands/auditar_invariantes_chamados.py:198,377,1435,1444,1594,1601,1602,1604,1711,1799
app/chamados/services/adesao.py:34
app/chamados/services/disputa_emissao.py:705
app/chamados/services/fabrica_da_celula.py:99
app/chamados/services/furo_sem_canal.py:72,73,97
app/chamados/services/limbo_folgas.py:194
app/chamados/services/resposta_tardia.py:45,56
app/chamados/services/silencio_ausencia.py:40
app/chamados/services/vigia_fio.py:379
app/colaboradores/management/commands/corrigir_escala_retroativa.py:123
app/colaboradores/services/calendario.py:413,414
app/colaboradores/services/pos_vinculo.py:61
app/escala/regua_defesa.py:688
app/escala/services/cadastro_realidade.py:173,445,581
app/escala/services/leitor_celula.py:407
app/escala/services/propositor.py:474
app/folha/export.py:1222,1233,1254,1255
app/folha/fora_da_folha.py:227
app/folha/previa.py:94
app/folha/services/vigia_pausa.py:157
app/pautas/services.py:422
app/ponto/marcos.py:155
app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58,62
app/ponto/management/commands/celula_veredito_velho.py:88,106,125
app/ponto/management/commands/recusar_ponta_pequena.py:145
app/ponto/management/commands/supra_juiz.py:166
app/ponto/portas/celula.py:397,701,741,766,783,797
app/ponto/services/cartorio.py:889,891,1199,1358
app/ponto/services/diagnostico_escala.py:177
app/ponto/services/furos_sem_cobranca.py:61
app/ponto/services/furos_vetados.py:61
app/ponto/services/pautas_dos_discordantes.py:71
app/ponto/services/veredito_celula.py:109,127
app/ponto/services/vigia_ausencia_veredito.py:42,56
app/relatorios/dia_da_frota.py:51,54,55,60,74
```

O inventário de acessos de produção a `Colaborador.situacao`, incluindo consultas por relações, metadados e projeções, é o seguinte; certificação absoluta de “todos”, incluindo acesso dinâmico, fixtures e migrações: **[nao verificado]**.

```text
app/api/login_helpers.py:141
app/api/views.py:116,355,415,420,421
app/api/views_core.py:173,344,402,407,408
app/api/views_mensageria.py:449,1058,1221,1236,1273
app/chamados/models.py:966
app/chamados/reconciliador.py:810
app/chamados/management/commands/auditar_invariantes_chamados.py:360,653,857,1473,1593,1655,1765,1777
app/chamados/management/commands/disparar_perguntas_competencia.py:169
app/chamados/services/adesao.py:33,66
app/chamados/services/desvio_escala.py:107
app/chamados/services/disputa_emissao.py:1241
app/chamados/services/fabrica_da_celula.py:91
app/chamados/services/pergunta_no_app.py:127
app/colaboradores/admin.py:49,50,71
app/colaboradores/models.py:280,

[truncado em 20000 de 30776 caracteres]

## 06/11 17:32 · sessao 1be5a3e989aa · OTIMIZA-3 · verificador · origem=Code

**REPROVA: o exemplo original foi corrigido, mas a classe “loader de string produz falso zero” continua aberta.** Há chamadas com literais completos que escapam da detecção, e o próprio selo exige zero para uma função chamada por loader. (`bin/quem_chama.py:515–520`, `bin/tests/test_quem_chama.sh:263–265`, `bin/tests/test_quem_chama.sh:508`)

A análise abaixo usa a cópia e reproduções em memória, sem criar arquivos. A cópia não contém `.git`: identidade do commit e execução integral do selo histórico **[não verificado]**. [nao verificado]

1. **BLOQUEIA — P1(a,c): loader obtido por `getattr` deixa falso zero, mesmo com módulo literal completo.**

   Contraexemplo sintético: alvo `app/pkg/mod.py`, contendo `def f(): return 1`; chamador:

   ```python
   import importlib
   m = getattr(importlib, 'import_module')('pkg.mod')
   m.f()
   ```

   A reprodução imprime `simbolo f [simbolo-de-topo]: 0 chamador(es)`. `_loaders_py` aceita somente função representada por `ast.Name` ou `ast.Attribute`; nessa chamada, a função é outro `ast.Call`. A string `'pkg.mod'` ainda conta como citação do arquivo, mas não rebaixa o símbolo. (`bin/quem_chama.py:515–520`, `bin/quem_chama.py:590–601`)

   O comportamento das formas pedidas é:

   | Forma | Resultado para o símbolo |
   |---|---|
   | `import_module('pkg.mod')`, `__import__('pkg.mod')`, `run_module('pkg.mod')` | Não varrido, com sítio. (`bin/quem_chama.py:99`, `bin/quem_chama.py:590–593`) |
   | `__import__('pkg', fromlist=['mod'])` | Não varrido, com sítio; combina constantes de pai e folha. (`bin/quem_chama.py:526–530`, `bin/quem_chama.py:550–552`) |
   | `import_module('.mod', package='pkg')` | Não varrido no AST, quando nome relativo e pacote são constantes. (`bin/quem_chama.py:531–536`) |
   | `from importlib import import_module as im`; `import importlib as il` | Cobertos: apelido do from-import ou último atributo da chamada. (`bin/quem_chama.py:506–520`) |
   | Tupla/lista/dict local, variável ou parâmetro com `'pkg.mod'` no mesmo arquivo | Cobertos lexicalmente; não há acompanhamento do valor até a chamada. (`bin/quem_chama.py:585–593`) |
   | Nome vindo de parâmetro, `getattr`, ambiente ou settings, sem literal completo no arquivo do loader | Pode permanecer zero; não há resolução entre arquivos nem desses valores. (`bin/quem_chama.py:523–525`, `bin/quem_chama.py:590–593`) |
   | `leaf=['mod']; __import__('pkg', fromlist=leaf)` | Pode dar zero nas duas facetas: `fromlist` variável não é combinado. (`bin/quem_chama.py:527–530`) |

2. **BLOQUEIA — P1(b), P2: execução por caminho e parâmetro entre arquivos continuam descobertas.**

   `spec_from_file_location`, `SourceFileLoader`, `SourcelessFileLoader`, `imp.load_source` e `run_path` são reconhecidos pelo nome. O alvo somente é associado quando alguma string do mesmo arquivo contém seu caminho ou basename. Portanto variável local, constante, `os.path.join`, `Path` e f-string funcionam **quando preservam esse literal**, como `'mod.py'`; não existe resolução dessas construções. (`bin/quem_chama.py:97–100`, `bin/quem_chama.py:433`, `bin/quem_chama.py:458–464`, `bin/quem_chama.py:586–589`)

   Contraexemplo sintético entre arquivos: `load.py` define `load(path)` usando `spec_from_file_location`; `caller.py` executa `load('app/pkg/mod.py').f()`. A reprodução mantém o arquivo com citações e o símbolo `f` com zero: nenhum arquivo reúne o loader reconhecido e o literal do alvo. (`bin/quem_chama.py:547`, `bin/quem_chama.py:585–593`)

   Também deixam o símbolo com zero, nos contraexemplos isolados, `exec(open('app/pkg/mod.py').read())`, `exec(compile(...))` e `subprocess.run(['python3', 'app/pkg/mod.py'])`: `exec`, `compile` e execução de Python por subprocesso não pertencem aos conjuntos de loaders. O caminho ainda pode contar na faceta arquivo. (`bin/quem_chama.py:97–99`, `bin/quem_chama.py:600–601`)

   Caminho montado como `'mod' + '.py'`, sem basename completo, pode deixar **arquivo e símbolo** com zero; o aviso FORA não altera os tipos das facetas. (`bin/quem_chama.py:90–92`, `bin/quem_chama.py:462`, `bin/quem_chama.py:911–915`)

3. **BLOQUEIA — P1(d,e): texto embutido tem cobertura diferente do AST e ainda produz zero.**

   Contraexemplo sintético reproduzido em `.sh`:

   ```sh
   python3 -c "import importlib;m=importlib.import_module('.mod',package='pkg');m.f()"
   ```

   Para `app/pkg/mod.py`, arquivo e símbolo imprimem zero. O reconhecimento textual procura nome completo entre aspas, `__import__` com `fromlist=` ou ponto de entrada; não resolve o relativo com `package=` que o AST resolve. (`bin/quem_chama.py:437–441`, `bin/quem_chama.py:467–483`, `bin/quem_chama.py:531–536`)

   Heredoc, `python3 -c`, string de código em `.py` e config são cobertos quando o texto contém um loader reconhecido e caminho/basename ou nome completo reconhecível. `exec(open(...))` embutido continua descoberto pelo mesmo motivo do achado 2. (`bin/quem_chama.py:472–483`, `bin/quem_chama.py:594–596`, `bin/quem_chama.py:685–689`)

   `pkg.mod:attr` simples é reconhecido em strings de `.py`, `.sh` e configurações aceitas, incluindo `.yml/.toml/.cfg/Dockerfile`. Em `.sh/config`, o nome pontilhado também pode causar EMBUTIDO, que prevalece sobre LOADER. Arquivo chamado apenas `gunicorn`, sem extensão reconhecida ou shebang, é ignorado; `gunicorn.conf.py` passa pelo AST. (`bin/quem_chama.py:119–120`, `bin/quem_chama.py:441`, `bin/quem_chama.py:777–792`, `bin/quem_chama.py:876–893`)

4. **BLOQUEIA — P5: `r9-montado-fora` certifica um falso zero concreto.**

   A fixture contém `X = "ld_mont"` e chama efetivamente `importlib.import_module("m.%s" % X).f_ld_mont()`, mas a asserção exige `simbolo f_ld_mont [simbolo-de-topo]: 0 chamador(es)`. Não é um contraexemplo vácuo: existe a ligação e existe a chamada. (`bin/tests/test_quem_chama.sh:263–265`, `bin/tests/test_quem_chama.sh:508`)

   A exclusão “nome montado fica FORA” está somente no texto; o símbolo continua em VARRIDOS e o resumo contabiliza seu zero como afirmação. Isso contraria o aval literal fornecido e a distinção operacional entre contado e não varrido. (`bin/quem_chama.py:10–13`, `bin/quem_chama.py:90–92`, `bin/quem_chama.py:590–593`, `bin/quem_chama.py:911–915`)

5. **FILA — P1/P6: casos reais corrigidos e casos reais ainda não associados ao loader.**

   O caso original está corrigido na cópia: `gerar_estado.py` conserva `'espelho_verdade.py'`, carrega o módulo e lê `mod.placar`; esse basename agora produz LOADER. O BLOQUEIO anterior foi resolvido nesse exemplo, mas não como classe, pelos achados anteriores. (`bin/gerar_estado.py:140–145`, `bin/quem_chama.py:586–589`)

   Permanecem estas formas reais sem reconhecimento específico do loader:

   | Caso real | Efeito demonstrável |
   |---|---|
   | `intake`: strings `modulo.simbolo`, separadas antes de `import_module` + `getattr` | Conta arquivo e símbolo pela regra antiga de citação; não produz LOADER. (`app/core/intake.py:34–35`, `app/core/intake.py:485–494`, `bin/quem_chama.py:606–608`) |
   | `test_marca_cron15`: nomes em tabela externa, seguidos de dois `getattr` | O arquivo do loader não contém os módulos completos; esse sítio não é associado. (`app/core/tests/test_marca_cron15.py:62–65`, `app/core/tests/marca_cron15.py:29–35`) |
   | `test_selo_estrutural`: dado externo, `hasattr(import_module(mod), cls)` | Mesmo problema; o alvo atual está na tabela externa. (`app/core/tests/test_selo_estrutural.py:23–31`, `app/core/estrutural.py:47`) |
   | Settings montado por perfil, depois `getattr(mod, 'MIDDLEWARE')` | Sem associação pelo loader. (`app/core/tests/test_trilha_com_contexto.py:103–108`, `app/core/tests/test_selo_log_nao_e_cego.py:120–125`, `app/core/tests/test_selo_erro_com_mensagem.py:80–85`) |
   | `exec(open(...).read(), ns)` e leitura de `ns['logconfig_dict']` | Execução real por caminho, sem detector correspondente. (`app/core/tests/test_access_log_persistido.py:73–78`, `bin/quem_chama.py:97–99`) |

   Esses exemplos não provam zeros globais atuais: testes e settings já recebem tipos fora de VARRIDOS, e citações existentes podem fornecer achados. A quantidade de falsos zeros atuais nesses alvos é **[não verificado]**. (`bin/quem_chama.py:348–349`, `bin/quem_chama.py:366–373`, `bin/quem_chama.py:606–608`)

   `include('x.urls')`, settings e `apps.get_model` não ganham reconhecimento de loader; `urlpatterns`, modelos e settings têm exclusões próprias, enquanto strings `app.mod.Classe` podem contar o símbolo diretamente. `call_command('nome')` conta o arquivo do command pelo token; `Command` é convencional, fora da contagem. (`app/config/urls.py:26`, `app/config/settings/base.py:53–68`, `bin/sombra_mascara_pessoal.py:19`, `bin/quem_chama.py:81–82`, `bin/quem_chama.py:352–355`, `bin/quem_chama.py:370–373`, `bin/quem_chama.py:817–819`, `bin/quem_chama.py:606–608`)

6. **FILA — P2: o caminho reconhecido protege a faceta arquivo, inclusive no próprio alvo.**

   Quando há marcador LOADER, o sítio é recolhido antes da exclusão de autocitação e depois acrescentado à faceta arquivo. Assim, loader reconhecido como único chamador, inclusive dentro do próprio alvo, não deixa arquivo com zero; os símbolos contáveis são rebaixados. (`bin/quem_chama.py:867–871`, `bin/quem_chama.py:885–894`)

   `app/X.py` corresponde a módulo `X`, sem `app.`; `app/pkg/__init__.py` corresponde a `pkg`. Ambos funcionam com esses nomes. Já `'app.pkg.mod'` não corresponde à chave `pkg.mod` usada pelo detector de nome e pode deixar o símbolo com zero. (`bin/quem_chama.py:171–180`, `bin/quem_chama.py:434–436`, `bin/quem_chama.py:590–593`)

   Basename coincidente associa todos os alvos correspondentes, inclusive pacotes com `__init__.py`; isso produz falsos positivos, não desambiguação. Os loaders não reconhecidos dos achados 1–3 continuam podendo deixar uma ou ambas as facetas com zero. (`bin/quem_chama.py:433`, `bin/quem_chama.py:461–464`)

7. **FILA — P3: falsos positivos podem esconder zero verdadeiro; há dois alvos falsamente associados comprovados, sem contagem comprovada de zeros ocultos.**

   Exemplos reais:

   - `bin/crons_duracao.py` cita `'crons.py'` e carrega `app/config/crons.py`; a regra por basename associa também `app/core/management/commands/crons.py`. Outro sítio equivalente copia `config.crons` para um temporário `crons.py`. **Um alvo falso, por dois sítios comprovados.** (`bin/crons_duracao.py:31–40`, `app/chamados/tests/test_cron_longo_sai_da_medida.py:268–279`, `bin/quem_chama.py:462`)
   - O logger `'core.erros'` é tratado como módulo carregado porque o arquivo contém `__import__`; esse loader percorre uma tupla que não contém `core.erros`. **Segundo alvo falso comprovado.** (`app/core/erros.py:30`, `app/core/erros.py:43–52`, `bin/quem_chama.py:590–593`)

   Um alvo sintético `app/a/common.py::morto`, sem leitores, seria rebaixado por um loader de `app/b/common.py`. Portanto o mecanismo pode retirar do relatório um zero verdadeiro utilizável pela L-111. Esses falsos positivos são expressamente admitidos pelo instrumento; não demonstram, sozinhos, violação do aval. (`bin/quem_chama.py:91–92`, `bin/quem_chama.py:462`, `bin/quem_chama.py:892–893`, `app/docs/LEIS.md:137`)

   **Nenhum zero verdadeiro oculto foi comprovado nesses dois alvos reais; total do repositório [não verificado].** Nos exemplos, há usos dos helpers e do logger. (`app/core/erros.py:71–73`, `app/colaboradores/views.py:437`, `app/core/management/commands/crons.py:57`, `app/core/management/commands/crons.py:99`)

   A regex não casa genericamente `http://` ou `C:\`: exige o módulo-alvo exato e `\w` após os dois-pontos. `10:30` exigiria módulo-alvo chamado `10`; existência desse alvo **[não verificado]**. `x.y:z` em string/config pode rebaixar `x.y`; `.md` e `.txt` comum são excluídos como chamadores. (`bin/quem_chama.py:441`, `bin/quem_chama.py:481–483`, `bin/quem_chama.py:780–792`)

8. **FILA — P4: os leitores de tipos e do resumo estão coerentes; a classificação incompleta é que alimenta zeros indevidos.**

   | Estado | Escritor | Leitor |
   |---|---|---|
   | `info['tipo']` | Classificador `:837`; EMBUTIDO `:883`; LOADER `:893` | Guardas `:882/:892`; render `:936` |
   | `a['tipo']` | Construção `:839`; EMBUTIDO `:881–883` | Render `:934` |
   | `a['embutido']` / `a['loader']` | `:880` / `:889` | Render `:933–936` |
   | Dicts `embutido` / `loader` | Inicialização `:847`; acumulação `:865/:868` | Consultas `:877/:886` |
   | `LOADER` / `EMBUTIDO` | Definições `:95/:94` | Emissão `:552/:588/:592/:595/:688` e `:598/:686`; recolha `:867/:864`; render `:907/:903` |
   | `LOADERS_CAMINHO` / `LOADERS_NOME` | `:97–99` | Regex `:100–101`; aliases `:509`; chamadas `:518/:520` |
   | `VARRIDOS` | `:56–73` | Guardas `:882/:892`; contagem `:911`; cabeçalho `:925` |
   | `FORA` | `:83–92` | Somente cabeçalho `:926` |
   | Parâmetros `emb` / `ldr` | Render `:933–936` | `_faceta`: `:902–909` |

   Todas as linhas da tabela pertencem a `bin/quem_chama.py`.

   EMBUTIDO e LOADER incrementam “não varridas”; somente o ramo VARRIDOS incrementa varridas e zeros. RESUMO e `main` usam esses acumuladores, sem semântica antiga observada. (`bin/quem_chama.py:903–915`, `bin/quem_chama.py:939–941`, `bin/quem_chama.py:968–969`)

   Não encontrei outro consumidor automatizado da saída além do selo; completude diante de referências construídas dinamicamente **[não verificado]**. O selo extrai facetas, e o uso humano pelo opus está documentado. (`bin/tests/test_quem_chama.sh:315–330`, `app/docs/RELATO.md:292`)

9. **FILA — P5: há asserções mascaradas, mas os grupos positivos representados mordem; três formas declaradas não têm fixture.**

   As asserções genéricas de não varrido/ausência de zero para `ld_cfg` e `ld_shn` sobrevivem à remoção do loader porque EMBUTIDO já as protege. Entretanto, as asserções de sítio específico falham: o grupo não passa inteiro por acaso. (`bin/tests/test_quem_chama.sh:499–506`, `bin/quem_chama.py:876–893`)

   Nas mutações em memória, remover os detectores representados altera os grupos correspondentes: spec → `dir/var/cte/par/sh`; SourceFileLoader → `sfl`; run_path → `rp/pyc`; import_module → `im/ima/rel/shn`; `__import__` → `imp`. Relativo, fromlist e apelidos também têm verificações específicas. (`bin/tests/test_quem_chama.sh:166–259`, `bin/tests/test_quem_chama.sh:485–505`)

   Já retirar `SourcelessFileLoader`, `load_source` ou `run_module` dos conjuntos e regex não altera as fixtures r9: faltam casos dessas formas. Execução do selo completo contra esses mutantes **[não verificado]**. (`bin/quem_chama.py:97–101`, `bin/tests/test_quem_chama.sh:164–259`)

   `r9-prosa-nao-carrega` não é vácuo: contém loaders completos em docstring e comentário shell; remover os respectivos filtros faz suas asserções falharem. O real histórico pode ser silenciosamente pulado pelo `if git cat-file`; sua execução nesta cópia é **[não verificado]**. (`bin/tests/test_quem_chama.sh:267–273`, `bin/tests/test_quem_chama.sh:509–510`, `bin/tests/test_quem_chama.sh:561–566`, `bin/quem_chama.py:585`, `bin/quem_chama.py:856`)

_2376 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
