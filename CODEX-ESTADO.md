# CODEX-ESTADO

> Ultima resposta processada: **06/11/2026 21:31** · publicado em **06/11/2026 21:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 06/11 21:31 · sessao af15cd47f7f8 · FIO-COM-ASSENTO-E0-V3 · leitura · origem=Code

| Letra | Resultado da conferência |
|---|---|
| **A** | A impressão mistura FATO, AVALIAÇÃO e TAREFA. **Não é canônica nem independente da ordem recebida.** `app/ponto/services/cartorio.py:452`, `app/ponto/services/cartorio.py:522`. |
| **B** | O teto já participa da avaliação e da impressão: vencimento por instante, corte das lâmpadas pendentes e indicador de turno em curso. Precisa sobreviver na **versão da avaliação**. `app/ponto/services/cartorio.py:831`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:1378`. |
| **C** | Intervalo e saída final distinguem-se por **motivos diferentes**; a identidade geral não contém MARCO nem instância. Não achei unicidade parcial de tarefa aberta; **há locks**, mas a proteção é incompleta. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2392`, `app/chamados/models.py:863`. |
| **D** | Hoje, `validada_em` já mata a pergunta para o juiz. O caminho de HORA acrescenta verificação do efeito e devolução da recusa ao admin. `materializada_em` também identifica absorções e encerramentos sem batida; não pode virar simplesmente “tarefa concluída”. `app/chamados/juizes.py:1135`, `app/chamados/services/validacao.py:422`, `app/chamados/services/materializacao.py:585`. |
| **E** | Há cobertura parcial dos dez casos; faltam provas específicas para a sequência noturna completa e duas transições simultâneas de pergunta/disputa. Os testes próximos estão discriminados abaixo. `app/chamados/tests/test_fio_intervalo.py:62`, `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_c2_corrida_do_abrir.py:94`; cobertura exata faltante `[nao verificado]`. |

Vínculo Git da cópia ao hash informado, resultados de execução dos testes e estado operacional de produção: **[nao verificado]**. A cobertura abaixo significa teste encontrado e lido. [nao verificado]

**A — Insumos, mistura e consumidores.** A classificação abaixo aplica as três naturezas propostas aos argumentos efetivamente serializados; a função atual produz uma única impressão para todos eles. `app/ponto/services/cartorio.py:443`, `app/ponto/services/cartorio.py:522`.

| Campo serializado | Natureza no desenho proposto | O que entra hoje |
|---|---|---|
| `bs_janela[].pk` | FATO: identidade do registro | PK da batida. `app/ponto/services/cartorio.py:453`. |
| `bs_janela[].tipo` | FATO | Tipo gravado. `app/ponto/services/cartorio.py:453`. |
| `bs_janela[].timestamp` | FATO | `isoformat()` do instante recebido. `app/ponto/services/cartorio.py:453`. |
| `cob_status` | **AVALIAÇÃO da cobertura escolhida**; o status da ausência de origem é um dado dessa fonte | Apenas o status retornado por `ausencia_cobre(..., para='cobranca')`, sem identidade da ausência nesse componente. `app/ponto/services/cartorio.py:1306`, `app/ponto/services/cartorio.py:1348`, `app/ponto/services/cartorio.py:454`. |
| `chamados[].pk` | TAREFA/identidade do chamado | PK de cada chamado selecionado. `app/ponto/services/cartorio.py:455`. |
| `chamados[].status_local` | TAREFA | Estado do chamado. `app/ponto/services/cartorio.py:455`. |
| `dna.dna_versao` | FATO de cadastro/revisão do DNA | Não é a versão da regra do cartório. `app/ponto/services/cartorio.py:456`, `app/ponto/services/cartorio.py:471`. |
| `dna.marcos` | FATO de cadastro usado no julgamento | Representação textual do objeto; não é o DNA inteiro. `app/ponto/services/cartorio.py:456`. |
| `veto` | AVALIAÇÃO | Motivo do veto, quando verdadeiro. `app/ponto/services/cartorio.py:458`. |
| `teto` | AVALIAÇÃO temporal | Quantidade de marcos vencidos passada pelo chamador; não o instante completo. `app/ponto/services/cartorio.py:463`, `app/ponto/services/cartorio.py:1350`. |
| `regra` | AVALIAÇÃO | Versão aplicável à competência. `app/ponto/services/cartorio.py:263`, `app/ponto/services/cartorio.py:471`. |
| `fio` | TAREFA, agregado pelo juiz do fio | Quantidade de chamados vivos com fio vivo; `None` quando não há chamados vivos. `app/ponto/services/cartorio.py:1334`, `app/ponto/services/cartorio.py:1352`. |
| `cob_parcial` | AVALIAÇÃO | Estado do período parcial e índices dos marcos dispensados, derivados da grade. `app/ponto/services/cartorio.py:435`, `app/ponto/services/cartorio.py:508`. |
| `enc` | AVALIAÇÃO temporal | `enc:0` para dia passado ainda em curso; nos demais casos esse componente fica ausente. `app/ponto/services/cartorio.py:481`, `app/ponto/services/cartorio.py:1347`. |
| `isento` | AVALIAÇÃO | `isento:1` somente com isenção e `regra >= 4`. `app/ponto/services/cartorio.py:520`. |

A mistura ocorre literalmente na concatenação: batidas/DNA, cobertura/veto/regra/relógio e chamado/fio produzem o mesmo hash. O próprio `houve_fato_novo` já distingue isso: exclui estado de chamados e fio da definição de fato do dia, porque mudam pelo próprio fechamento. `app/ponto/services/cartorio.py:452`, `app/ponto/services/cartorio.py:329`.

Os consumidores encontrados são:

- **Cartório:** compara impressão atual com `CelulaDia.impressao`, junto de carimbo/veredito, para pular ou rejulgar; `forcar` atravessa o pulo. Há um ramo para nunca-bateu e outro normal. `app/ponto/services/cartorio.py:1199`, `app/ponto/services/cartorio.py:1203`, `app/ponto/services/cartorio.py:1358`, `app/ponto/services/cartorio.py:1362`.
- **Porta `carimbar`:** compara impressão e demais insumos para evitar novo save; depois persiste impressão e carimbos junto da lavra. `app/ponto/portas/celula.py:1047`, `app/ponto/portas/celula.py:1056`, `app/ponto/portas/celula.py:1060`.
- **Releitura após emissão:** usa novamente a impressão para rejulgar células cujo chamado/fio acabou de mudar. Portanto, mudança de tarefa pode disparar novo julgamento sem mudar batida ou DNA. `app/ponto/services/cartorio.py:1015`, `app/ponto/services/cartorio.py:1038`.
- **Comando:** reexporta a função importada; não contém outra implementação. O instrumento `celula_veredito_velho` também lê a impressão para fotografar o estado anterior. `app/ponto/management/commands/processar_cartorio.py:20`, `app/ponto/management/commands/celula_veredito_velho.py:110`.

**Canonicidade: não.** A função não ordena batidas/chamados e usa `str(dna.marcos)`. A carga normal ordena batidas somente por timestamp, sem desempate por PK; chamados usam `-criado_em`, também sem desempate. Inverter as listas ou a inserção das chaves de um `marcos` equivalente altera os bytes serializados — confirmado também por avaliação isolada da função em memória. `app/ponto/services/cartorio.py:453`, `app/ponto/services/cartorio.py:455`, `app/ponto/services/cartorio.py:456`, `app/ponto/turnos.py:2871`, `app/chamados/models.py:100`.

O teste chamado determinístico repete a mesma entrada na mesma ordem. Não achei nele teste de permutação ou de dicionários equivalentes com chaves reordenadas. `app/ponto/tests/test_cartorio.py:14`.

**B — Onde mora o teto e o que depende dele.**

A regra declarada exige **comparar instante ou exigir fato encerrado**; data civil não basta. O selo procura mecanismos no código, mas é uma verificação estrutural por vocabulário, não uma prova completa de todas as combinações temporais. `CLAUDE.md:185`, `app/ponto/tests/test_contract_teto_temporal.py:82`, `app/ponto/tests/test_contract_teto_temporal.py:108`.

Hoje ele aparece em quatro pontos:

1. **Entrada do julgamento:** recusa data futura/célula inexistente; para hoje, sem `teto_de_hoje`, espera o fim do turno. Essa guarda usa `_tz.now()`, enquanto o corpo recebe `agora`. `app/ponto/services/cartorio.py:1974`, `app/ponto/services/cartorio.py:1982`, `app/ponto/services/cartorio.py:1989`.
2. **Vencimento por marco:** `marcos_turno` encadeia os instantes e aplica `TETO_MARCO_MIN = 30`. `app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:840`.
3. **Conteúdo julgado/lavrado:** marco ainda não vencido sai da grade julgada e sua lâmpada passa a `acesa=None, pendente=True`; não vira falta. `app/ponto/services/cartorio.py:851`, `app/ponto/services/cartorio.py:862`, `app/ponto/services/cartorio.py:1378`.
4. **Atualidade e emissão de perguntas:** `teto`/`enc` alteram a impressão; a fábrica consulta os vencidos para selecionar motivos. `app/ponto/services/cartorio.py:1347`, `app/chamados/services/fabrica_da_celula.py:15`, `app/chamados/services/fabrica_da_celula.py:59`.

O encerramento do turno também considera aberta ainda viva, fim previsto e ausência de previsão; não equivale a “virou o dia”. O teste encontrado mantém a pergunta da saída ausente enquanto o noturno está aberto vivo e a faz nascer após sua expiração. `app/ponto/turnos.py:2114`, `app/ponto/turnos.py:2119`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:351`.

**Consequência para o desenho:** por inferência dessas dependências, o instante de referência, os marcos vencidos/pendentes, a condição de turno encerrado e a política temporal precisam compor a **versão da avaliação**. Se restar apenas a versão dos fatos, o relógio avança sem necessariamente mudar batidas/DNA, e o pulo pode preservar avaliação vencida. O teste cartório 06:27 → vigia 06:40 cobre justamente a mudança temporal que precisa provocar nova pergunta. `app/ponto/services/cartorio.py:1358`, `app/chamados/tests/test_pergunta_do_dia_em_curso.py:361`.

Não confundir esse teto com **horizonte de regra** ou **piso de emissão**: a versão é escolhida por competência; o piso limita emissão automática e deliberadamente fica fora da impressão. `app/ponto/services/cartorio.py:263`, `app/ponto/services/cartorio.py:270`, `app/ponto/services/cartorio.py:285`, `app/ponto/services/cartorio.py:1676`.

**C — Identidade real e concorrência.**

| Objeto | Chave/agrupamento efetivo |
|---|---|
| **Pergunta** | `(colaborador da disputa, motivo, data_evento)`, entre todas as disputas/estados; reutiliza a última encontrada. Sem data na chamada, trata como conversa e nasce sempre. `app/chamados/models.py:2392`, `app/chamados/models.py:2398`, `app/chamados/models.py:2424`, `app/chamados/models.py:2431`. |
| **Disputa** | Procura aberta por colaborador, agrega motivos e contexto. Não representa uma tarefa individual por marco. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:472`, `app/chamados/services/disputa_emissao.py:498`. |
| **Chamado** | `abrir` deduplica pelos filtros recebidos. No emissor de ausência: colaborador + módulo + `contexto_json.data_turno`. O pai de disputa usa colaborador + categoria + módulo + estados vivos. `app/chamados/models.py:867`, `app/ponto/management/commands/detectar_ausencias.py:693`, `app/chamados/services/disputa_emissao.py:504`. |

**O exemplo intervalo versus saída final não se funde necessariamente:** `intervalo_saida` e `orfao_14h` são motivos distintos. Entretanto, dois marcos com o mesmo motivo e data atingem a mesma chave; hora, posição e instância da tarefa não a integram. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2392`.

A data também não é uniformemente jornada: o emissor de intervalo pode usar o dia civil do marco, e seu teste noturno espera D+1 nas perguntas. `app/chamados/services/disputa_emissao.py:982`, `app/chamados/services/disputa_emissao.py:991`, `app/chamados/tests/test_fio_intervalo.py:77`.

**Banco:** não achei índice único parcial de tarefa aberta nos três modelos nem nas migrations de chamados consultadas. Chamado tem `unique_together=[]`; disputa tem índice comum `(colaborador, fechada_em)`; pergunta tem apenas ordenação no `Meta`. O unique parcial encontrado é de **Followup.evento_chave**, não da identidade da obrigação. `app/chamados/models.py:97`, `app/chamados/models.py:1742`, `app/chamados/models.py:2857`, `app/chamados/migrations/0047_evento_chave_do_fio.py:20`.

**Há `select_for_update`:**

- Abertura de chamado trava o colaborador antes do dedup; transição protegida recarrega/trava o chamado; costura trava o destino quando está em transação. `app/chamados/models.py:863`, `app/chamados/models.py:668`, `app/chamados/services/fio.py:758`.
- Emissão de disputa trava a disputa aberta **existente**. A consulta vazia não oferece linha para travar. `app/chamados/services/disputa_emissao.py:466`.

**Corridas possíveis por inspeção, sem ocorrência de produção demonstrada:**

- Pergunta: consulta `.last()` → `create`, sem lock no próprio gate. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`.
- Disputa nova: consulta vazia → abre/reusa pai → cria disputa, sem repetir a consulta após a trava do colaborador. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`.
- Validação: lê/testa `validada_em` antes do `atomic`, sem travar a pergunta nesse fluxo. `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:395`.
- Fechamento de disputa: confere estado/perguntas antes do `atomic` e depois grava, sem releitura travada nesse método. `app/chamados/models.py:1820`, `app/chamados/models.py:1827`, `app/chamados/models.py:1846`, `app/chamados/models.py:1861`.

**D — O que fecha, recusa e preservação da absorção.**

Para a **pergunta**, o primeiro ramo do juiz considera `validada_em` suficiente para deixá-la morta; `estado` distingue `validada` de `resolvida` pela presença adicional de `materializada_em`. Portanto, não existe hoje uma exigência universal de “efeito efetivado + rejulgamento consistente”. `app/chamados/juizes.py:1135`, `app/chamados/models.py:2499`.

O plantio normal de HORA/data-hora tem defesa adicional: executa em transação, exige sucesso e `_pergunta_materializada_de_fato`, desfaz a batida se reprovada e só então carimba. Contudo, consultar C6 após `carimbar(resposta='validar')` não é prova independente: esse carimbo pode preencher validação antes da consulta ao juiz que mata pela validação. `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1252`, `app/chamados/models.py:2591`, `app/chamados/models.py:2596`.

Para a **disputa**, o fechamento normal exige ausência de respostas sem veredito e de fio vivo. O materializador chama esse juiz; não fecha simplesmente pelo número de batidas criadas. Há portas excepcionais, inclusive encerramento pelo admin/desligamento. `app/chamados/models.py:1783`, `app/chamados/models.py:1789`, `app/chamados/models.py:1852`, `app/chamados/services/materializacao.py:1288`.

Para o **chamado**, o materializador passa pela retenção de irmãs e por `reconciliar()`; fechar a disputa também aciona o tratamento do pai. Não são o mesmo estado nem uma única condição de fechamento. `app/chamados/services/materializacao.py:736`, `app/chamados/services/materializacao.py:746`, `app/chamados/models.py:1812`.

**Validada com materialização recusada tem respostas diferentes:**

- **HORA declarada, sem materialização e com recusa registrada:** desfaz `validada_em/por`, preserva resposta, reabre a disputa por padrão e retorna `nao_gravou/devolvida`. Fica respondida e viva para o **admin**. O teste exige exatamente isso. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:187`, `app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:460`, `app/chamados/tests/test_o261_recusa_fala.py:95`.
- **Escape, outro tipo ou encerramento já carimbado por identidade:** essa devolução não se aplica; a validação pode permanecer e C6 a considera morta. Os testes preservam explicitamente os casos de escape e colisão já carimbada. `app/chamados/services/validacao.py:143`, `app/chamados/services/validacao.py:182`, `app/chamados/tests/test_o261_recusa_fala.py:190`, `app/chamados/tests/test_o261_recusa_fala.py:203`.
- **Alcançabilidade:** resposta não validada atribui ação ao admin, mas o crédito depende do destino/pai vivo e a lista padrão exclui arquivado/histórico. Logo, “viva” sozinha não prova porta alcançável. `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:60`, `app/chamados/services/verbos.py:18`.

Há uma divergência relevante entre comentário e código: o comentário da devolução de ausência diz que o app só lista disputa aberta; o seletor atual de perguntas do app consulta `q_vivas()` e depois o juiz, sem aquele filtro explícito. Não usaria o comentário como prova da seleção atual. `app/chamados/services/materializacao.py:632`, `app/chamados/services/pergunta_no_app.py:42`, `app/chamados/models.py:2793`.

**Compatibilidade com pós-condição:** separar conclusão da tarefa do registro do efeito é compatível como direção, mas não basta mudar o predicado de fechamento. Hoje `materializada_em` identifica também o **ato de absorção** e encerramentos sem batida, inclusive paridade recusada. `app/chamados/services/materializacao.py:433`, `app/chamados/services/materializacao.py:585`.

A recuperação de absorvidas depende de:

- `(disputa_id, data_evento, materializada_em)` compartilhado pelas irmãs;
- escape correspondente ao tipo da ausência dentro desse grupo;
- exclusão das perguntas que já possuem batida viva originada nelas. `app/chamados/services/materializacao.py:605`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:614`, `app/chamados/services/materializacao.py:616`.

**Quebraria essa recuperação**, por consequência direta dos filtros: deixar `materializada_em` nulo até a aprovação final do atestado, dar timestamps distintos às irmãs ou mover apenas parte delas para outra disputa. Hoje a absorção usa o mesmo instante mesmo com ausência aberta; nessa fase o chamado permanece vivo/silenciado. Seria necessário preservar esse identificador de ato ou substituí-lo com ligação equivalente antes de retirar a autoridade desses campos. `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:607`.

**E — Dez casos de laboratório, condicionados aos respectivos insumos.**

| Caso | Comportamento hoje / quem age | Cobertura encontrada |
|---|---|---|
| **1. Dois marcos no dia** | Motivos diferentes podem gerar perguntas distintas sob a mesma disputa/chamado; silêncio pede resposta do colaborador, resposta não validada pede admin. Resolver uma pergunta não basta para fechar disputa com outra viva. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/catalogo/natureza.py:48`, `app/chamados/models.py:1789`. | Dois marcos de intervalo: `app/chamados/tests/test_fio_intervalo.py:62`. **Não achei o caso exato intervalo-saída + saída-final**, nas buscas em `app/chamados/tests` e `app/ponto/tests`; cobertura específica `[nao verificado]`. |
| **2. Noturno 21→22, sem 23** | O plantio toma `data_evento`, combina a hora e ajusta usando os marcos dessa data, tentando D e D+1. Como uma fábrica já pode gravar D+1, **não está demonstrada a garantia contra deslocar novamente**. Recusa de HORA volta ao admin pelo fluxo descrito em D. `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:414`, `app/chamados/services/materializacao.py:420`, `app/chamados/juizes.py:303`, `app/chamados/juizes.py:310`. | Ajuste isolado para D+1 e entrada sem deslocamento: `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_plantio_intervalo_overnight.py:35`; reuso em D+1: `app/chamados/tests/test_materializar_overnight.py:33`. **Não achei teste integrado 21→22 sem 23** nesses testes e nas buscas de noturno; `[nao verificado]`. |
| **3. Validada/materialização recusada** | HORA sem carimbo e com motivo: validação desfeita, resposta preservada, admin recebe novamente; não exige nova resposta do colaborador. Fora desse escopo, pode permanecer validada/morta. `app/chamados/services/validacao.py:182`, `app/chamados/services/validacao.py:187`, `app/chamados/services/validacao.py:460`, `app/chamados/juizes.py:1135`. | Devolução, irmãs preservadas e exceções: `app/chamados/tests/test_o261_

[truncado em 20000 de 26519 caracteres]

## 06/11 21:30 · sessao 62c88a377430 · DUAS-TELAS-E-PWA-OFFLINE · censo · origem=Code

**Censo estático: 40 views POST examinadas; 39 com escrita, sendo 32 no recorte C. Normalizei 25 famílias de ação; 12 têm múltiplas telas comprovadas. Resultado: 23 pares divergentes, discriminados abaixo.** A contagem considera funções distintas, sem duplicar aliases de rota; apresentações equivalentes ficam agrupadas. As referências do inventário sustentam essa contagem. Ocorrência, frequência e valores afetados em produção: **[não verificado]**.

A referência adotada foi célula como fonte e projeções como leitores; escritor único conforme o glossário e autoridades do código vivo. (`app/docs/GLOSSARIO.md:11`; `app/docs/GLOSSARIO.md:12`; `app/docs/PLANO-PADROES.md:11`; `app/core/juizes.py:288`)

| Universo examinado | Views POST com escrita | Evidência |
|---|---:|---|
| Escala | 9 | Remover vínculo, editar calendário, aplicar calendário, corroborar e realocar régua; salvar tipo, criar calendário pelo wizard; toggle e lote de folgas. (`app/escala/views.py:256`; `app/escala/views.py:316`; `app/escala/views.py:376`; `app/escala/views.py:456`; `app/escala/views.py:463`; `app/escala/views_wizard.py:98`; `app/escala/views_wizard.py:140`; `app/escala/views_plano_folgas.py:98`; `app/escala/views_plano_folgas.py:144`) |
| Folha | 5; 1 no recorte | TXT, rubricas e três operações de integração. (`app/folha/views.py:102`; `app/folha/views.py:27`; `app/folha/views.py:466`; `app/folha/views.py:487`; `app/folha/views.py:501`) |
| Relatórios | 3 | Espelho anexado, avulso e lote. (`app/relatorios/views.py:971`; `app/relatorios/views.py:1050`; `app/relatorios/views.py:1152`) |
| Holerite | 6; 3 no recorte | Três uploads/publicação; enviar, reenviar e avisar ficaram fora da comparação de entrega. (`app/holerite/views.py:1145`; `app/holerite/views.py:1217`; `app/holerite/views.py:1721`; `app/holerite/views.py:605`; `app/holerite/views.py:734`; `app/holerite/views.py:1794`) |
| Fronteiras dos mesmos domínios | 16 | Sete em ponto, três em feriados, duas em vínculos e quatro portas de aceite de folga. (`app/ponto/views.py:1226`; `app/ponto/views.py:1257`; `app/ponto/views.py:1383`; `app/ponto/views.py:1434`; `app/ponto/views.py:1461`; `app/ponto/views.py:1520`; `app/ponto/views.py:3312`; `app/core/views_config.py:39`; `app/core/views_config.py:153`; `app/core/views_config.py:182`; `app/colaboradores/views.py:212`; `app/colaboradores/views.py:373`; `app/chamados/views.py:359`; `app/chamados/views.py:659`; `app/chamados/views.py:869`; `app/chamados/views_cobrar.py:145`) |

O quadragésimo POST é o preview do wizard, sem persistência. Copiar/excluir tipo também foram examinados: escrevem por GET e não entram no total POST. (`app/escala/views_wizard.py:226`; `app/escala/services/cadastro_tipo.py:396`; `app/escala/views.py:141`; `app/escala/views.py:156`; `app/templates/escala/tipos_lista.html:90`; `app/templates/escala/tipos_lista.html:92`)

As 25 famílias são: treze de escala/folga — criar, editar, copiar e excluir tipo; encerrar e vincular/realocar vínculo; corroborar régua; criar, editar e atribuir calendário; lançar, alterar e remover folga —, cinco de fechamento/célula, TXT, publicação e análise de holerite, espelho PDF e três de feriado. (`app/escala/urls.py:8`; `app/escala/urls.py:15`; `app/escala/urls.py:19`; `app/escala/urls.py:31`; `app/escala/urls.py:34`; `app/ponto/urls.py:31`; `app/ponto/urls.py:58`; `app/folha/urls.py:14`; `app/holerite/urls.py:8`; `app/holerite/urls.py:37`; `app/relatorios/urls.py:24`; `app/core/urls.py:42`)

**1. Criar calendário de folgas — 1 par divergente.**

| Tela A → escritor | Tela B → escritor |
|---|---|
| `/escala/folgas/criar/` → `editar_folga_calendario`, `app/escala/views.py:316`; template `app/templates/escala/folga_calendario_editar.html:9` → **`salvar_calendario_de_folga`**, `app/escala/folgas.py:548`. | Wizard `/escala/tipos/wizard/`, botão em `app/templates/escala/wizard_tipo_escala.html:160`; POST `/escala/tipos/wizard/calendario/criar/` → `wizard_calendario_criar`, `app/escala/views_wizard.py:140` → **`criar_calendario_de_folga`**, `app/escala/folgas.py:577`. |

**Difere:** editor recebe listas independentes para meses de 28/30/31 dias; wizard replica uma lista com descarte dos dias acima do tamanho. Pares semana/dia malformados são ignorados pelo editor e recusados pelo wizard. Convergem apenas no veto de nome repetido. (`app/escala/views.py:333`; `app/escala/views.py:340`; `app/escala/views_wizard.py:163`; `app/escala/folgas.py:539`; `app/escala/folgas.py:570`; `app/escala/folgas.py:590`)

**Leitor / gravidade:** `datas_que_o_calendario_manda` alimenta `gerar_foto_folgas`, que grava `FolgaDia` e relavra células. **Muda dia: sim, potencialmente após materialização; o cadastro isolado só grava o calendário.** (`app/escala/folgas.py:43`; `app/escala/folgas.py:150`; `app/escala/folgas.py:184`; `app/escala/folgas.py:203`)

**2. Editar folga semanal do tipo — 1 par divergente, convergindo no escritor.**

| Tela A → escritor | Tela B → escritor |
|---|---|
| `/escala/tipos/<tid>/wizard/`; POST `/escala/tipos/<tid>/wizard/salvar/` → `wizard_salvar`, `app/escala/views_wizard.py:98`; template `app/templates/escala/wizard_tipo_escala.html:128` → **`salvar_tipo`**, `app/escala/services/cadastro_tipo.py:140`. | Modal `/chamados/modal/<colaborador>/`; template `app/templates/chamados/partials/modal_fio.html:545` envia `aceitar_folga_limbo` para `/chamados/local/<id>/` → `detalhe_chamado_local`, `app/chamados/views.py:431` → `aceitar_sugestao`, `app/chamados/services/limbo_folgas.py:73` → **mesmo `salvar_tipo`**, chamado em `app/chamados/services/limbo_folgas.py:91`. |

**Difere:** wizard recebe os dias escolhidos; aceite exige sugestão fixa do contexto. Só o aceite acrescenta `LogAuditoria` específico e percorre chamados LIMBO do tipo, reconciliando e anexando ocorrência quando o fechamento passa. (`app/escala/views_wizard.py:44`; `app/chamados/services/limbo_folgas.py:81`; `app/chamados/services/limbo_folgas.py:94`; `app/chamados/services/limbo_folgas.py:98`)

**Leitor / gravidade:** campo e regeneração são comuns; o fio lê as ocorrências adicionais. **Dinheiro/dia: não há diferença comprovada para os mesmos dias; trilha e chamados diferem.** (`app/escala/services/cadastro_tipo.py:259`; `app/escala/services/cadastro_tipo.py:320`; `app/chamados/views.py:372`)

**3. Confirmar cadastro de escala errado — 1 par divergente, convergindo no escritor.**

| Tela A → escritor | Tela B → escritor |
|---|---|
| `/escala/cadastro-x-realidade/`; template `app/templates/escala/cadastro_x_realidade.html:28`; POST `.../regua/<ec_pk>/corroborar/` → `corroborar_regua_da_lista`, `app/escala/views.py:456` → **`corroborar_regua`**, `app/escala/regua_defesa.py:647`. | Modal `/chamados/modal/<colaborador>/`; template `app/templates/chamados/partials/modal_fio.html:559`; POST `/chamados/local/<id>/`, ação `corroborar_regua` → `app/chamados/views.py:438` → **mesmo `corroborar_regua`**. |

**Difere:** lista endereça o vínculo pela PK; chamado procura vínculo ativo por pessoa e tipo guardado no contexto. Só o caminho do chamado passa `chamado=` e anexa ocorrência. Ambos gravam régua pela mesma porta. (`app/escala/views.py:449`; `app/chamados/views.py:441`; `app/chamados/views.py:443`; `app/escala/regua_defesa.py:662`; `app/escala/regua_defesa.py:670`)

**Leitor / gravidade:** cobrança lê a régua; fio lê a ocorrência. **Dinheiro/dia: não para o mesmo vínculo; alvo diferente entre as telas é possível, mas sua ocorrência em produção está [não verificado].** (`app/escala/regua_defesa.py:641`; `app/escala/regua_defesa.py:662`; `app/chamados/views.py:372`)

**4. Vincular/realocar escala — 3 pares divergentes, convergindo nas portas.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — manual | `/colaboradores/vincular/<cid>/painel/`; POST `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`, `app/colaboradores/views.py:373`; template `app/templates/colaboradores/partials/painel_vinculo.html:136` → **`julgar_vinculo` / `executar_vinculo`**, chamadas em `app/colaboradores/views.py:411` e `app/colaboradores/views.py:423`. |
| B — lista sugerida | `/escala/cadastro-x-realidade/`; POST `.../regua/<ec_pk>/realocar/` → `realocar_regua_da_lista`, `app/escala/views.py:463`; template `app/templates/escala/cadastro_x_realidade.html:28` → `realocar_vinculo_para_destino`, `app/escala/regua_defesa.py:291` → **mesmas portas**, `app/escala/regua_defesa.py:316`. |
| C — chamado sugerido | Modal `/chamados/modal/<colaborador>/`; template `app/templates/chamados/partials/modal_fio.html:568`; POST `/chamados/local/<id>/`, ação `realocar_regua` → `app/chamados/views.py:449` → `realocar_para_destino`, `app/escala/regua_defesa.py:326` → **mesmas portas**, `app/escala/regua_defesa.py:329`. |

- **A×B e A×C:** manual recebe destino, início e âncora humanos; sugeridos fixam início em hoje e recusam ciclos antes do juiz. Manual pode alcançar a janela anterior e criar ciclo com âncora declarada. (`app/templates/colaboradores/partials/painel_vinculo.html:155`; `app/templates/colaboradores/partials/painel_vinculo.html:159`; `app/templates/colaboradores/partials/painel_vinculo.html:167`; `app/escala/regua_defesa.py:310`; `app/escala/regua_defesa.py:313`; `app/colaboradores/services/vinculo.py:69`; `app/colaboradores/services/vinculo.py:126`)
- **B×C:** lista usa destino da última lavra; chamado usa destino do seu contexto. Só C acrescenta ocorrência de realocação. (`app/escala/views.py:469`; `app/escala/regua_defesa.py:328`; `app/escala/regua_defesa.py:333`)

**Leitor / gravidade:** `eh_dia_trabalho` lê a célula; executar vínculo regenera o trecho escolhido, e a porta recalcula competências tocadas. **Muda dia/dinheiro: sim, potencialmente, pelo destino e período distintos.** Igualdade dos destinos em produção: **[não verificado]**. (`app/escala/models.py:1094`; `app/colaboradores/services/vinculo.py:320`; `app/ponto/portas/celula.py:112`; `app/ponto/portas/celula.py:445`; `app/ponto/portas/celula.py:458`)

**5. Lançar folga manual/aceitar “era folga” — 6 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| P — Plano | `/escala/plano-folgas/`; POST `/escala/plano-folgas/aplicar/` → `app/escala/views_plano_folgas.py:144`; template `app/templates/escala/plano_folgas.html:171` → **`aplicar_folgas_em_lote`**, `app/escala/folgas.py:362` → `_foto_do_dia`, `app/escala/folgas.py:320`. |
| D — detalhe | `/chamados/local/<id>/` → `app/chamados/views.py:359`, ramo em `app/chamados/views.py:483`; template `app/templates/chamados/detalhe_local.html:310`, formulário `app/templates/chamados/partials/_pergunta_card.html:92` → **`validar_pergunta`**, `app/chamados/services/validacao.py:382`. |
| M — modal | `/chamados/modal/<colaborador>/` → `app/chamados/views.py:869`, delegação em `app/chamados/views.py:954`; template `app/templates/chamados/partials/modal_fio.html:274` → **mesmo `validar_pergunta`**, por `app/chamados/services/fio.py:605`. |
| I — inline | Painel `/chamados/painel/`; botão `app/templates/chamados/partials/_lista_chamados.html:82`; POST `/chamados/validar-inline/<cid>/` → `app/chamados/views_cobrar.py:145` → **mesmo `validar_pergunta`**, chamada em `app/chamados/views_cobrar.py:208`. |

Nos aceites de folga, a cadeia termina em `declarar_dia` → **`lancar_folga`**; o Plano usa outro escritor da foto. (`app/chamados/services/validacao.py:291`; `app/ponto/services/declarar_dia.py:189`; `app/escala/folgas.py:280`; `app/escala/folgas.py:337`)

| Pares | Diferença |
|---|---|
| P×D, P×M, P×I | Plano recusa dia anterior à competência atual; aceite usa teto de antiguidade de 120 dias. Plano recebe F1…F9/FD; aceite fixa FD. Aceite carimba pergunta, fecha perguntas e tenta reconciliar chamados; Plano solicita relavra sem passar por esse encerramento explícito. (`app/escala/folgas.py:379`; `app/escala/folgas.py:385`; `app/escala/views_plano_folgas.py:173`; `app/ponto/services/declarar_dia.py:101`; `app/ponto/services/declarar_dia.py:190`; `app/ponto/services/declarar_dia.py:204`; `app/chamados/services/validacao.py:299`; `app/chamados/services/validacao.py:300`) |
| D×M, I×M | Detalhe e inline exigem disputa aberta; modal admite pergunta viva em disputa fechada e acrescenta trilha desse ato. **Convergem no escritor, divergem na validação anterior.** (`app/chamados/views.py:495`; `app/chamados/views_cobrar.py:160`; `app/chamados/services/fio.py:596`; `app/chamados/services/fio.py:601`; `app/chamados/services/fio.py:611`) |
| D×I | Detalhe valida a pergunta endereçada; inline percorre todas as perguntas vivas respondidas da disputa encontrada. **Convergem no escritor, diferem na seleção do ato.** (`app/chamados/views.py:493`; `app/chamados/views.py:501`; `app/chamados/views_cobrar.py:169`; `app/chamados/views_cobrar.py:201`) |

**Leitor / gravidade:** precedência lê a folga; relavra chega à célula. **Muda dia: sim, potencialmente**, sobretudo no passado que P recusa e os aceites podem alcançar. Dinheiro efetivamente alterado: **[não verificado]**. (`app/ponto/precedencia.py:356`; `app/ponto/precedencia.py:384`; `app/escala/folgas.py:272`)

**6. Aprovar fechamento — 3 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — competência/selecionados | `/ponto/fechamento/`; POST `/ponto/fechamento/aprovar/` → `app/ponto/views.py:1257`; disparador `app/templates/ponto/fechamento.html:143` → **`aprovar_lote`**, `app/ponto/services/fechamento.py:1316`. |
| B — individual | `/ponto/fechamento/painel/` → `app/ponto/views.py:966`; template `app/templates/ponto/partials/painel_fechamento.html:86`; POST `/ponto/fechamento/aprovar-colaborador/` → `app/ponto/views.py:1461` → **`aprovar`**, `app/ponto/services/fechamento.py:1328`. |
| C — ao gerar TXT | `/folha/`, template `app/templates/folha/index.html:63`, ou `/folha/exportar/`, template `app/templates/folha/exportar.html:26`; POST `/folha/exportar/gerar/` → `app/folha/views.py:102` → **mesmo `aprovar_lote` de A**, chamado em `app/folha/views.py:252`. |

- **A×B:** A fornece linhas gravadas ao juiz; B fornece `fechamento_lido`. A grava observação por update; B faz save inteiro. A pode trancar o período; B apenas aprova. (`app/ponto/views.py:1290`; `app/ponto/views.py:1480`; `app/ponto/views.py:1362`; `app/ponto/views.py:1367`; `app/ponto/services/fechamento.py:1333`)
- **A×C:** convergem em `aprovar_lote`; C calcula em leitura, carimba apenas elegíveis para o TXT, mantém observação, registra auditoria própria e exportação. A pode trancar; C não. (`app/folha/views.py:208`; `app/folha/views.py:246`; `app/folha/views.py:252`; `app/folha/views.py:254`; `app/folha/views.py:297`; `app/ponto/services/fechamento.py:1323`)
- **B×C:** leitura viva em ambos, mas escritores/save diferentes e universo de aprovação diferente; C acrescenta TXT e sua trilha. (`app/ponto/views.py:1480`; `app/ponto/views.py:1515`; `app/folha/views.py:246`; `app/folha/views.py:252`; `app/folha/views.py:297`)

O modo cego de A considera conta vazia; selecionados/B dispensam essa trava. Seu formulário existe **dentro de `display:none`**, portanto não presumi botão visível. (`app/ponto/views.py:1304`; `app/ponto/views.py:1509`; `app/templates/ponto/fechamento.html:152`; `app/templates/ponto/fechamento.html:209`)

**Leitor / gravidade:** tranca aciona encerramento da fila; status aprovado é lido por `competencia_fechada`, podendo impedir ajuste posterior. **Dinheiro/dia: não diretamente no carimbo; muda controle de escrita e fila.** A regra “TXT não tranca” está declarada como intencional no registro vivo. (`app/ponto/services/fechamento.py:1322`; `app/ponto/services/fechamento.py:1413`; `app/ponto/janelas.py:87`; `app/ponto/services/flip_batida.py:21`; `app/core/juizes.py:510`)

**7. Publicar/substituir holerite da competência — 3 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — avulso | `/holerite/upload/` → `app/holerite/views.py:1145`; template `app/templates/holerite/upload.html:9` → **delete/create na view**, `app/holerite/views.py:1191`, `app/holerite/views.py:1193`. |
| B — ZIP | `/holerite/upload-lote/` → `app/holerite/views.py:1217`; template `app/templates/holerite/upload_lote.html:12` → **delete/create na view**, `app/holerite/views.py:1255`, `app/holerite/views.py:1259`. |
| C — PDF processado | `/holerite/lote/` → `app/holerite/views.py:1721`; confirmação `app/templates/holerite/_lote_progresso.html:119` → job `app/holerite/management/commands/processar_lote_holerite.py:89` → **`motor._gravar`**, `app/holerite/motor.py:390`, documento em `app/holerite/motor.py:480`. |

| Par | Diferença e gravidade |
|---|---|
| A×B | A recusa substituir documento com ciência e audita substituição; B apaga sem essa guarda. A preenche empresa da folha; B omite. A assinatura possui CASCADE. **Dinheiro/dia: não diretamente; pode apagar ciência e mudar a identificação na matriz.** (`app/holerite/views.py:1176`; `app/holerite/views.py:1184`; `app/holerite/views.py:1202`; `app/holerite/views.py:1255`; `app/holerite/views.py:1259`; `app/holerite/models.py:193`; `app/holerite/views.py:295`) |
| A×C | A substitui documentos sem ciência e cria sem lote; C pula quem já tem documento, grava empresa do CNPJ e vínculo ao lote publicado. **Muda dia/dinheiro: sim, potencialmente pela guarda da competência**, além da substituição diferente. (`app/holerite/views.py:1191`; `app/holerite/views.py:1193`; `app/holerite/motor.py:323`; `app/holerite/motor.py:344`; `app/holerite/motor.py:479`; `app/holerite/management/commands/processar_lote_holerite.py:129`) |
| B×C | B apaga e recria sem empresa/lote; C publica incrementalmente, preservando documentos existentes e vinculando ao lote. **Muda dia/dinheiro: sim, potencialmente pela mesma guarda; também há diferença de preservação da ciência.** (`app/holerite/views.py:1255`; `app/holerite/views.py:1259`; `app/holerite/motor.py:344`; `app/holerite/motor.py:480`; `app/holerite/models.py:193`) |

**Leitor que torna essa diferença relevante:** `competencia_fechada_para_celula` só considera holerite com `lote.publicado_em` preenchido. A guarda de regeneração enumera esses documentos; edição do dia também consulta essa proteção. Assim, “publicado” pelos uploads A/B não produz a mesma proteção de C. Ocorrência concreta em produção: **[não verificado]**. (`app/folha/export.py:614`; `app/ponto/management/commands/gerar_celulas.py:169`; `app/ponto/portas/celula.py:568`)

**8. Analisar lote de holerite — 1 par divergente, convergindo no escritor.**

A página `/holerite/lote/`, `upload_lote_v2`, template `app/templates/holerite/upload_lote_v2.html:23`, e o drawer `/holerite/celula/<empresa>/<ano>/<mes>/modo/?modo=lote`, `drawer_modo`, template `app/templates/holerite/_drawer_modo_lote.html:13`, POSTam para a mesma view e **convergem na criação de `LoteHolerite` e no job**. (`app/holerite/views.py:340`; `app/holerite/views.py:357`; `app/holerite/views.py:1721`; `app/holerite/views.py:1773`)

**Difere:** drawer informa empresa esperada; página genérica não. O job verifica divergência de CNPJ na análise quando esse contexto existe. **Dinheiro/dia: não neste ato; muda a validação que antecede a publicação e o estado/erro mostrado no lote.** (`app/templates/holerite/_drawer_modo_lote.html:19`; `app/holerite/views.py:1771`; `app/holerite/views.py:1779`; `app/holerite/management/commands/processar_lote_holerite.py:107`; `app/holerite/management/commands/processar_lote_holerite.py:137`)

**9. Emitir/exportar espelho PDF — 3 pares divergentes.**

| Caminho | Tela, view, template → escritor |
|---|---|
| A — anexar | `/chamados/local/<id>/`, formulário `app/templates/chamados/detalhe_local.html:348`; POST `/relatorios/espelho/chamado/<id>/anexar/` → `app/relatorios/views.py:971` → **`gerar_pdf_espelho`**, `app/relatorios/services.py:49`. |
| B — avulso | `/relatorios/espelho/avulso/` → `app/relatorios/views.py:1050`; template `app/templates/relatorios/espelho_pdf_avulso.html:18` → **mesmo `gerar_pdf_espelho`**, chamada em `app/relatorios/views.py:1127`. |
| C — lote 

[truncado em 20000 de 26787 caracteres]

## 06/11 21:28 · sessao 3ef57447b4ee · - · - · origem=Ronald (tmux)

E — achados de leitura; testes encontrados, não executados. [nao verificado]

| Caso | Comportamento demonstrado hoje | Testes/limite | [nao verificado]
|---|---|---|
| **1. Dois marcos no dia** | Há perguntas distintas para motivos distintos; o dedup datado usa `(disputa,motivo,data_evento)`, portanto `intervalo_saida` e outro motivo não necessariamente se fundem. Isso ainda não é identidade explícita por MARCO/instância: `app/chamados/services/disputa_emissao.py:368`, `app/chamados/services/disputa_emissao.py:387`, `app/chamados/services/disputa_emissao.py:389`. | Dois marcos de intervalo no mesmo chamado geram duas perguntas: `app/chamados/tests/test_fio_intervalo.py:62`. **Não achei teste exato “falta saída intervalo + falta saída final”** nas buscas por marcos/motivos em `app/chamados/tests` e `app/ponto/tests` [nao verificado]. |
| **2. Noturno 21→22 sem 23** | O plantio parte de `p.data_evento`, combina esse dia com a hora e depois ajusta overnight: `app/chamados/services/materializacao.py:344`, `app/chamados/services/materializacao.py:414`, `app/chamados/services/materializacao.py:420`. O ajuste consulta a escala desse dia e tenta somente D e D+1 dentro do envelope dos marcos: `app/chamados/juizes.py:302`, `app/chamados/juizes.py:310`. **Não há garantia demonstrada contra revirada quando `data_evento` já contém o dia civil do marco**: o teste legado grava D+1 nas perguntas de intervalo (`app/chamados/tests/test_fio_intervalo.py:77`), enquanto o ajuste toma esse dia como base (`app/chamados/juizes.py:303`). | Próximos: volta 00:00 vai a D+1, entrada 19:00 não desloca: `app/chamados/tests/test_plantio_intervalo_overnight.py:27`, `app/chamados/tests/test_plantio_intervalo_overnight.py:35`; saída existente D+1 é reutilizada: `app/chamados/tests/test_materializar_overnight.py:33`. **Não achei teste exato 21→22 sem 23** nesses arquivos e na busca de datas/noturno em `app/chamados/tests` [nao verificado]. |
| **4. Atestado pendente/aprovado/rejeitado** | Escape ATESTADO cria ausência com estado decidido por `estado_inicial`; sem documento, `aguardando_documento`. A absorção tenta carimbar perguntas-hora do mesmo dia, mas o C6 pode recusar e deixá-las vivas; ausência aberta mantém chamado vivo e o silencia: `app/chamados/services/materializacao.py:495`, `app/chamados/services/materializacao.py:531`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:562`. Rejeição devolve perguntas absorvidas antes de levantar silêncio: `app/ponto/signals.py:254`. | Rejeitada: perguntas pendentes/vivas e disputa aberta, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`. Aprovada não devolve, `:129`. Outra ausência viva segura absorção, `:119`; pergunta com batida não volta, `:141`; nova resposta reabre a MESMA ausência, `:154`; reabertura só pelo DP não reabsorve, `:172`. |
| **5. Troca de plantão nos dois colabs** | Exige par de ausências/cartas, pessoas distintas e documentos nos dois lados. Trava cartas, edita os dois dias no mesmo atomic; cedente vira folga, substituto dia normal pelo template próprio; recusa do segundo lado desfaz primeiro: `app/ponto/portas/celula.py:632`, `:652`, `:657`, `:662`, `:673`. A mudança marca cobranças órfãs para rejulgamento e agenda juiz antes do recalculo, por competência dos dias: `app/ponto/portas/celula.py:622`, `:627`, `:422`, `:445`. Falta em folga tocada é reavaliada no pós-commit: `app/ponto/portas/celula.py:490`. | Cedente sem desconto `app/ponto/tests/test_o10b_editar_dia_troca.py:159`; substituto sem HE100/folga trabalhada e mantendo noturno `:169`; competências distintas `:260`; falha entre lados rollback `:325`; idempotência `:238`; sem par/documentos/vínculo recusa `:213`, `:222`, `:350`. O teste de concorrência existente verifica AST/locks, não duas transações reais: `:314`. |
| **8. Passivo sem inventar** | O plantio recusa texto vazio e grava recusa com dono; não toma validação/admin ou carimbo de resposta como anuência: `app/chamados/services/materializacao.py:804`. Lote de passivo usa mesma unidade oficial, classificação do vigia; exclui slot não derivável e plantio já fora do slot: `app/chamados/services/passivo_validado.py:39`, `:60`. O dry não chama unidade: `:360`; aplica por `(colab,competência)` atomic e fecha por disputa uma vez ao final: `:371`, `:403`. | Recusa sem resposta: `app/chamados/tests/test_a27_recusa_com_dono.py:33`; lote encontra carimbo falso invisível à fila admin `app/chamados/tests/test_o317_passivo_pela_porta.py:188`; dry `:256`; fora do slot `:383`; slot não derivável `:406`; outra pendência da disputa impede fechamento `:502`. |
| **9. Desligamento com histórico pendente** | Bloqueia login e fecha vínculos: `app/colaboradores/services/desligamento.py:109`, `:122`. **Resposta sem veredito preserva disputa e chamado vivo para admin julgar**; mudas podem morrer por `admin_decidiu`: `:139`, `:142`, `:155`. Cria chamado auditoria `dp_rh`: `:159`. Cartório continua julgando histórico, mas emissão exclui desligado: `app/ponto/services/cartorio.py:1696`. | Preservação resposta sem veredito e fechamento muda: `app/colaboradores/tests/test_desligar_guards.py:93`; desligado julgado sem cobrança `app/ponto/tests/test_hx_cartorio_universo.py:34`. |
| **10. DiaPago/export** | Dinheiro é atualizado pelo recalculo da competência do fato em `on_commit`: `app/ponto/services/fechamento.py:1791`, `:1800`; recalculo lavra DiaPago: `:1184`, `:1194`. Alterar só ata/veredito não equivale necessariamente a atualizar snapshot/TXT: teste exige rejulgar competência exportada deixando fechamento e TXT intactos, `app/ponto/tests/test_grade_v5_cartorio_rejulga_a_exportada.py:15`. Export retém por veredito/protestos da célula, com exceção preadoção e pausa exclusiva: `app/folha/export.py:1233`, `:1245`, `:1253`. **Comparações DiaPago×fechamento/espelho são publicadas, mas ficam FORA de `falhas`; não travam sozinhas o TXT**: `app/folha/porta_export.py:687`, `:696`. | `app/folha/tests/test_porta_do_export.py:135` sela contadores sem reprovação; `:216` acusa DiaPago incorreto; `:495` não lavrado distinto de zero; `app/folha/tests/test_borda_celula.py:49` sela furo retentor e `:90` preadoção sem retenção. |

Detalhes para consolidação:

- **No noturno, distinguir jornada de data civil é requisito concreto**, porque `data_evento` tanto recebe D+1 no teste do emissor de intervalo (`app/chamados/tests/test_fio_intervalo.py:77`) quanto serve de entrada para buscar os marcos e possivelmente acrescentar outro dia (`app/chamados/juizes.py:303`, `:310`). Não afirme que o fluxo atual garante “nunca 23”; a prova exata não foi encontrada [nao verificado].
- **Atestado aberto não deve ser descrito como chamado fechado**: o código explicitamente pula `reconciliar()` enquanto a ausência está em ABERTAS (`app/chamados/services/materializacao.py:562`). A absorção de cada irmã também pode ser recusada pelo C6, com trilha própria e retorno da lista de recusadas (`:549`, `:553`, `:569`).
- **Desligamento não apaga a obrigação histórica respondida**: a exceção preserva resposta sem veredito e a deixa ao admin, apesar de remover o acesso do colaborador (`app/colaboradores/services/desligamento.py:111`, `:139`, `:155`). Não substitua isso pela regra antiga “cascade fecha tudo”.
- **Export de desligado**: data de demissão até o fim da janela direciona para rescisão; demissão depois do fim não exclui aquela competência mensal (`app/folha/export.py:854`, `:858`); retenção de célula recorta universo por admissão/demissão (`app/folha/export.py:1198`).
- **Passivo validado exportado**: plantio, regeneração pela porta declarada e pauta DP são um par transacional; recusa da regeneração desfaz plantio/carimbos (`app/chamados/services/passivo_validado.py:371`, `:374`, `:419`, `:438`); pauta só quando o gravado mudou (`:442`), com tratamento próprio quando não mudou (`:445`).

## 06/11 21:28 · sessao 7e4c6131adfe · DUAS-TELAS-E-PWA-OFFLINE · censo · origem=Code

**Fechei 32 views POST próprias no recorte: 18 de ponto e 14 de férias. Elas expõem 33 rotas; colaboradores fornece duas telas que reutilizam o POST de ponto.** A contagem exclui os caminhos herdados do Django Admin, cuja implementação não está nesta cópia. `app/ponto/urls.py:37`, `app/ponto/urls.py:53`, `app/ponto/urls.py:83`, `app/ferias/urls.py:9`, `app/colaboradores/views.py:1935`, `app/colaboradores/views.py:2938`, `app/ponto/admin.py:18`; implementação herdada: **[não verificado]**.

**Censo consolidado: 10 grupos divergentes fechados e 1 par de endpoints com segundo disparador visual não confirmado.** Conto cada grupo uma vez, sem multiplicar suas combinações; os caminhos e as diferenças estão discriminados abaixo. As bases dessa contagem são as views citadas em D1–D11.

**Ocorrência, frequência e valores afetados em produção: [não verificado].** “Muda dinheiro/dia: sim” abaixo significa capacidade demonstrada pelo caminho do código, não prejuízo observado.

Todos os arquivos abreviados abaixo são relativos a `app/`; por exemplo, `ponto/views.py:1911` significa `app/ponto/views.py:1911`.

O padrão utilizado foi escritor único por estado, com célula como fonte e espelho/DiaPago/fechamento como projeções. As autoridades vivas de cobertura, efeito, saldo e fracionamento estão registradas no código. `docs/GLOSSARIO.md:11`, `docs/GLOSSARIO.md:12`, `docs/PLANO-PADROES.md:11`, `core/juizes.py:239`.

**Universo de ações e telas.**

São **25 ações pela seguinte convenção**, separando materialização retroativa, resolução do dia e registro histórico, mas agrupando operações individuais e em lote:

| App | Ações inventariadas | Views |
|---|---|---|
| Ponto | Ausência: lançar, aprovar, rejeitar, cancelar, editar registro, declarar período parcial, reverter rejeição, anexar documento. Justificativa: aprovar, rejeitar, materializar saída. Resolver dia. Criar e editar tipo. | 18 funções; referências em `ponto/views.py:1763`, `ponto/views.py:1859`, `ponto/views.py:1911`, `ponto/views.py:1923`, `ponto/views.py:2133`, `ponto/views.py:2270`, `ponto/views.py:2304`, `ponto/views.py:2333`, `ponto/views.py:2368`, `ponto/views.py:2569`, `ponto/views.py:2599`, `ponto/views.py:2662`, `ponto/views.py:2680`, `ponto/views.py:2720`, `ponto/views.py:2751`, `ponto/views.py:3106`, `ponto/views.py:3312`, `ponto/views_tipos_ausencia.py:57`. |
| Férias | Sincronizar/criar período, registrar gozo histórico, agendar, cancelar, alterar datas, aprovar solicitação, rejeitar solicitação, editar período, excluir período, enviar confirmação, vender dias. | 14 funções; `ferias/views.py:123`, `ferias/views.py:269`, `ferias/views.py:321`, `ferias/views.py:341`, `ferias/views.py:378`, `ferias/views.py:421`, `ferias/views.py:519`, `ferias/views.py:569`, `ferias/views.py:633`, `ferias/views.py:711`, `ferias/views.py:777`, `ferias/views.py:808`, `ferias/views.py:830`, `ferias/views.py:906`. |
| Colaboradores | Calendário e diagnóstico disparam “Resolver dia”; não acrescentam escritor POST local dessas entidades. | `colaboradores/views.py:1935`, `colaboradores/views.py:2938`, `templates/colaboradores/calendario_partial.html:63`, `templates/colaboradores/partials/_drawer_diagnostico.html:36`. |

As rotas dos três apps vêm de `config/urls.py:26`, `config/urls.py:29`, `config/urls.py:32`; CI repete esses includes, e o URLconf de teste herda as rotas da UI. `config/urls_ci.py:7`, `config/urls_test.py:16`.

**Leitores usados para avaliar as diferenças.**

| Leitor | O que observa |
|---|---|
| Espelho | Cobertura para cobrança e cobertura aprovada para folha; justificativas aprovadas também justificam o dia. `ponto/services/espelho.py:582`, `ponto/services/espelho.py:588`, `ponto/services/efeito_ausencia.py:40`. |
| Fechamento / DiaPago | Justificativas aprovadas, efeito da ausência e abono extraído da grade. `ponto/services/fechamento.py:660`, `ponto/services/fechamento.py:693`, `ponto/services/dia_pago.py:385`. |
| Folha | Ausências aprovadas que descontam/abonam; férias com batida são consultadas pelo **agendamento**. `folha/export.py:87`, `folha/export.py:140`, `folha/export.py:201`, `folha/export.py:491`. |
| Saldo / gozo de férias | Saldo deriva de direito−concedidos−vendidos; gozo consulta agendamento não cancelado. `ferias/models.py:216`, `ferias/services.py:47`, `ferias/services.py:57`. |

**D1 — Aprovar justificativa: individual × lote × materialização retroativa.**

| Caminho | Tela, rota, view → escritor |
|---|---|
| Individual | Painel `/ponto/justificativa/<jid>/painel/`, `ponto/views.py:1662`; `templates/ponto/partials/painel_justificativa.html:51` → POST `/ponto/justificativa/<jid>/aprovar/`, `ponto/views.py:1911` → `aprovar_justificativa_dp`, `ponto/services/justificativa.py:117`. Rotas: `ponto/urls.py:39`. |
| Lote | Fila `/ponto/justificativa/fila/`, `ponto/views.py:1695`; `templates/ponto/fila_justificativas.html:176` → POST `/ponto/justificativa/aprovar-lote/` → **escrita na própria view**, `ponto/views.py:1763`, `ponto/views.py:1796`. |
| Retroativa | `/ponto/justificativa/<jid>/materializar-saida/`, `ponto/views.py:3106`; `templates/ponto/materializar_saida_retroativa.html:38` → `materializar_saida_dp`, `ponto/services/justificativa.py:316`. Rota: `ponto/urls.py:44`. |

O individual valida saída posterior à entrada, gera saída somente quando há entrada e saída informada, fecha origem e bloqueio de entrada, chama auditoria e followup. O lote grava aprovação diretamente, pode gerar saída sem entrada e não executa aquela validação temporal nem os mesmos efeitos de bloqueio/auditoria/followup. Ambos invalidam fechamento e fecham origem. `ponto/services/justificativa.py:137`, `ponto/services/justificativa.py:190`, `ponto/services/justificativa.py:199`, `ponto/services/justificativa.py:202`, `ponto/services/justificativa.py:214`, `ponto/views.py:1796`, `ponto/views.py:1804`, `ponto/views.py:1808`, `ponto/views.py:1837`.

A retroativa aceita justificativa sem filtro de status, recebe novo horário e motivo, grava `saida_informada` e aprovação, e fecha uma cascata de chamados. Tem auditoria própria; não usa o escritor individual de aprovação. `ponto/views.py:3130`, `ponto/services/justificativa.py:329`, `ponto/services/justificativa.py:340`, `ponto/services/justificativa.py:365`, `ponto/services/justificativa.py:378`, `ponto/services/justificativa.py:398`.

**Muda dinheiro/dia: sim, potencialmente.** Os caminhos podem produzir batida e estado diferentes; aprovação é insumo do fechamento e do espelho. `ponto/services/fechamento.py:660`, `ponto/services/efeito_ausencia.py:40`.

**D2 — Rejeitar justificativa: individual × lote.**

| Tela A | Tela B |
|---|---|
| Painel, `templates/ponto/partials/painel_justificativa.html:58` → `/ponto/justificativa/<jid>/rejeitar/`, `ponto/views.py:1923` → `rejeitar_justificativa_dp`, `ponto/services/justificativa.py:247`. | Fila, `templates/ponto/fila_justificativas.html:188` → `/ponto/justificativa/rejeitar-lote/` → view/escritor `ponto/views.py:1859`. Rotas: `ponto/urls.py:42`. |

Individual usa transação/trava, cria chamado de rejeição e followup. Lote faz `get`/`save` diretamente e envia push, sem esses mesmos efeitos. O receiver comum rejulga o dia nos dois caminhos: **não há ausência de rejulgamento exclusiva do lote**. `ponto/services/justificativa.py:258`, `ponto/services/justificativa.py:275`, `ponto/services/justificativa.py:309`, `ponto/views.py:1874`, `ponto/views.py:1882`, `ponto/signals.py:161`.

**Muda dinheiro/dia: sim em concorrência, por inferência estática.** A leitura/escrita sem trava do lote pode disputar com a aprovação; o status resultante alimenta a cobertura do dia. No processamento sequencial da mesma pendência, a divergência demonstrada é chamado/followup. `ponto/views.py:1874`, `ponto/services/justificativa.py:127`, `ponto/services/efeito_ausencia.py:40`.

**D3 — Lançar ausência/falta/abono: formulário × Resolver dia.**

| Tela A | Tela B |
|---|---|
| `/ponto/ausencias/lancar/`, view `ponto/views.py:2368`; `templates/ponto/lancar_ausencia.html:12` → `criar_ausencia`, `ponto/services/ausencia.py:130`, chamada em `ponto/views.py:2415`. | Calendário `/colaboradores/<cid>/calendario/`, `colaboradores/views.py:1935`, e diagnóstico `/colaboradores/colab/<cid>/diagnostico-drawer/`, `colaboradores/views.py:2938` → formulários `templates/ponto/partials/_veredito_dia.html:17` e `_veredito_lote_barra.html:2` → `/ponto/celula/veredito/`, `ponto/views.py:3312` → `veredito_celula`, `ponto/services/veredito_celula.py:191` → `criar_ausencia`, `ponto/services/veredito_celula.py:315`. Rotas: `ponto/urls.py:57`, `colaboradores/urls.py:24`, `colaboradores/urls.py:86`. |

**Converge na criação de Ausencia**, mas o formulário permite range/minutos/documento/HE100; Resolver dia passa um dia, fecha perguntas/chamados e lavra veredito humano na célula. Para INSS, somente o lançamento chama a alteração de `Colaborador.situacao` para afastado. `ponto/views.py:2415`, `ponto/views.py:2433`, `ponto/services/veredito_celula.py:315`, `ponto/services/veredito_celula.py:325`, `ponto/services/veredito_celula.py:330`.

**Muda dinheiro/dia: sim pelo alcance/minutos; não está demonstrada diferença monetária para entradas equivalentes.** Calendário lê o carimbo humano exclusivo; DiaPago lê o abono da grade. `colaboradores/services/calendario.py:411`, `ponto/services/dia_pago.py:391`.

**D4 — Aprovar ausência: painel × lote.**

| Tela A | Tela B |
|---|---|
| `/ponto/ausencias/<aid>/painel/`, `ponto/views.py:2067`; `templates/ponto/partials/painel_ausencia.html:309` → `/ponto/ausencias/<aid>/aprovar/`, `ponto/views.py:2599`. | `/ponto/ausencias/`, `ponto/views.py:2155`; `templates/ponto/ausencias.html:117`, `templates/ponto/ausencias.html:308` → `/ponto/ausencias/lote/aprovar/`, `ponto/views.py:2270`. Rotas: `ponto/urls.py:54`, `ponto/urls.py:65`. |

Ambos convergem em `_a4_decidir` → `decidir` → `mudar_estado`. Individual expande raiz/irmãos, admite período parcial e confirmação de desconto, usa transação para os irmãos e envia push. Lote processa somente IDs selecionados, sem período e com `confirmado=False`. `ponto/views.py:82`, `ponto/services/ausencia.py:897`, `ponto/services/ausencia.py:808`, `ponto/views.py:2612`, `ponto/views.py:2615`, `ponto/views.py:2629`, `ponto/views.py:2641`, `ponto/views.py:2646`, `ponto/views.py:2282`.

**Muda dinheiro/dia: sim.** Podem ser aprovados conjuntos diferentes de registros; somente o individual declara o intervalo parcial nesse ato. Cobertura de folha aceita apenas aprovadas, e há autoridade específica para o período parcial. `ponto/turnos.py:2514`, `ponto/turnos.py:2614`.

**D5 — Rejeitar ausência: painel × endpoint de lote — segunda tela incompleta.**

Painel: `templates/ponto/partials/painel_ausencia.html:330` → `/ponto/ausencias/<aid>/rejeitar/`, `ponto/views.py:2680`. Lote: `/ponto/ausencias/lote/rejeitar/`, `ponto/views.py:2304`; existe emissor JS em `templates/ponto/ausencias.html:316`, mas botão que o invoque: **[não verificado]**. Rotas: `ponto/urls.py:55`, `ponto/urls.py:74`.

Ambos convergem em `decidir`; individual expande irmãos e envia push, enquanto lote usa IDs e persiste motivo em `descricao`. `ponto/views.py:2688`, `ponto/views.py:2701`, `ponto/views.py:2704`, `ponto/views.py:2311`, `ponto/views.py:2319`.

**Muda dinheiro/dia: sim pelo conjunto rejeitado**, pois rejeitada deixa de cobrir folha/cobrança; o par de telas permanece incompleto. `ponto/turnos.py:2514`, `ponto/turnos.py:2516`.

**D6 — Conceder férias: agendamento × solicitação × histórico.**

| Caminho | Tela, rota, view → escritor |
|---|---|
| Agendamento | `templates/ferias/agendar.html:13` ou `_secao_colab.html:60` → `/ferias/periodo/<id>/agendar/`, `ferias/views.py:269`; lote `templates/ferias/agendar_lote.html:24` → `/ferias/agendar-lote/`, `ferias/views.py:569`. Ambos → `executar_agendamento`, `ferias/services.py:306`. |
| Solicitação | `templates/ferias/solicitacoes.html:26` → `/ferias/solicitacoes/<sid>/avaliar/`, `ferias/views.py:711` → `aprovar_solicitacao`, `ferias/services.py:410`. |
| Histórico | Drawer `templates/ferias/_secao_colab.html:22` → `/ferias/<cid>/novo-periodo/`, `ferias/views.py:123`, ramo `ferias/views.py:197` → `registrar_gozo_historico`, `ferias/services.py:270`. |

Rotas acima: `ferias/urls.py:10`, `ferias/urls.py:12`, `ferias/urls.py:15`, `ferias/urls.py:16`.

Agendamento padrão usa validação comum de saldo/fracionamento/duplicata e unidade atômica. Solicitação grava aprovação antes de verificar disponibilidade e pode terminar aprovada **sem agendamento**, com aviso. Histórico possui escritor próprio, não chama o `clean` de fracionamento e limita concedidos por `min(direito, concedidos+dias)`. `ferias/views.py:226`, `ferias/services.py:315`, `ferias/services.py:416`, `ferias/services.py:424`, `ferias/services.py:428`, `ferias/services.py:443`, `ferias/services.py:286`, `ferias/services.py:299`.

Os três salvam `AgendamentoFerias`, passando pelo veto do modelo e pelo espelhamento de ausências: o histórico **não está sem veto de colisão**. `ferias/models.py:387`, `ferias/models.py:434`.

**Muda dinheiro/dia: sim, potencialmente**, pela existência do agendamento, saldo consumido e cobertura materializada; folha consulta agendamento para férias com batida. `ferias/models.py:216`, `ponto/services/ausencia.py:1202`, `folha/export.py:491`.

**D7 — Conceder férias: ausência direta × caminhos de agendamento de D6.**

A tela de lançamento de D3 aceita o tipo recebido; Resolver dia também o encaminha. Ambos criam **somente Ausencia**. Agendamento padrão, solicitação materializada e histórico criam Agendamento e alteram concedidos. `ponto/views.py:2415`, `ponto/services/veredito_celula.py:315`, `ponto/services/ausencia.py:341`, `ferias/services.py:321`, `ferias/services.py:434`, `ferias/services.py:298`.

O catálogo inclui `ferias`, e os choices não o excluem; o calendário exclui somente `falta`. A oferta efetiva desse tipo no cadastro de produção é **[não verificado]**. `ponto/catalogo/ausencias.py:101`, `ponto/catalogo/ausencias.py:303`, `ponto/catalogo/ausencias.py:313`, `colaboradores/views.py:1959`.

**Muda dinheiro/dia: sim, se férias estiver disponível.** Pode existir cobertura abonada sem consumo do saldo nem participação no universo de agendamentos que a folha usa para retenção. Não há sincronização inversa nesses caminhos; os receivers de ausência cuidam de célula/perguntas/chamados. `ferias/models.py:216`, `ponto/signals.py:143`, `ponto/signals.py:250`, `folha/export.py:491`, `folha/export.py:868`.

**D8 — Cancelar férias: normal × admin × massa × concluída.**

| Caminho | Tela, rota, view → escritor |
|---|---|
| Normal | Saldo `templates/ferias/saldo.html:71` ou drawer `_secao_colab.html:160` → `/ferias/agendamento/<id>/cancelar/`, `ferias/views.py:321` → `cancelar_agendamento_dp`, `ferias/services.py:330`. |
| Admin | Drawer `_secao_colab.html:162` → `/ferias/agendamento/<id>/cancelar-admin/`, `ferias/views.py:341` → `executar_cancelamento_admin`, `ferias/services.py:341`. |
| Massa | `templates/ferias/cancelar_admin_massa.html:27` → `/ferias/cancelar-admin-massa/`, `ferias/views.py:519` → mesmo escritor admin, chamada `ferias/views.py:559`. |
| Concluída | Drawer `_secao_colab.html:166` → `/ferias/agendamento/<id>/corrigir-concluida/`, `ferias/views.py:378` → mesmo escritor admin, chamada `ferias/views.py:407`. |

Rotas: `ferias/urls.py:9`, `ferias/urls.py:18`, `ferias/urls.py:19`, `ferias/urls.py:20`.

Normal bloqueia somente `concluido`; portanto uma repetição com status cancelado chega novamente ao decremento de concedidos. Admin recusa cancelado; concluída exige estado próprio e justificativa. Normal não envolve suas duas gravações no `atomic` que existe no escritor admin. `ferias/views.py:325`, `ferias/services.py:333`, `ferias/models.py:335`, `ferias/views.py:396`, `ferias/views.py:399`, `ferias/services.py:352`.

Admin carimba `obs` e rejeita ausências antes de removê-las; individual admin e concluída chamam LogAuditoria, massa não faz a mesma chamada. **Todos removem as ausências no resultado final do cancelamento**, pelo `save` do agendamento. `ferias/services.py:356`, `ferias/services.py:358`, `ferias/views.py:367`, `ferias/views.py:409`, `ferias/views.py:551`, `ferias/models.py:434`, `ponto/services/ausencia.py:1188`.

**Muda dinheiro/dia: sim, sobretudo saldo/idempotência.** O segundo POST normal volta a tentar devolver dias; o saldo depende diretamente desse contador. A disponibilidade visual também diverge: saldo exige apenas status agendado, drawer exige `pode_cancelar`, incluindo ainda não ter iniciado. `ferias/services.py:333`, `ferias/models.py:216`, `templates/ferias/saldo.html:70`, `templates/ferias/_secao_colab.html:159`, `ferias/models.py:330`.

**D9 — Alterar datas de férias: edição normal × correção admin.**

| Tela A | Tela B |
|---|---|
| Drawer `_secao_colab.html:130` → `/ferias/agendamento/<id>/editar/`, `ferias/views.py:633` → `remarcar_agendamento`, `ferias/services.py:403`. | Drawer `_secao_colab.html:146` → `/ferias/agendamento/<id>/editar-datas-admin/`, `ferias/views.py:421` → `corrigir_datas_agendamento`, `ferias/services.py:398`. Rotas: `ferias/urls.py:21`, `ferias/urls.py:22`. |

**Converge em `_trocar_janela`**, que altera datas/status/aviso, limpa ausências antigas e atualiza concedidos atomicamente. Normal exige status agendado; admin admite qualquer não cancelado, exige motivo e registra LogAuditoria. `ferias/services.py:366`, `ferias/services.py:377`, `ferias/views.py:649`, `ferias/views.py:452`, `ferias/views.py:454`, `ferias/views.py:497`.

**Muda dinheiro/dia: sim pelo universo permitido**, pois admin pode mover férias iniciadas/concluídas. Para a mesma alteração aceita, o escritor das datas está unificado. `ferias/services.py:398`, `ferias/services.py:403`, `ferias/views.py:452`.

**D10 — Alterar férias: editar Ausencia × editar Agendamento.**

Ausência: painel `/ponto/ausencias/<aid>/painel/`, `ponto/views.py:2067`, formulário `templates/ponto/partials/painel_ausencia.html:134` → `/ponto/ausencias/<aid>/editar/`, `ponto/views.py:2751` → `editar_ausencia_registro`, `ponto/services/ausencia.py:465`. Compare com os dois caminhos de D9. Rota: `ponto/urls.py:76`.

Editar Ausencia altera tipo/range/minutos e invalida fechamento; não altera Agendamento/concedidos. Editar Agendamento usa `_trocar_janela`, alterando os dois lados e limpando a janela antiga. `ponto/services/ausencia.py:498`, `ponto/services/ausencia.py:509`, `ponto/services/ausencia.py:524`, `ferias/services.py:378`, `ferias/services.py:387`.

**Muda dinheiro/dia: sim.** A cobertura do espelho pode mudar enquanto saldo e intervalo consultado pela folha permanecem no agendamento antigo. `ponto/services/espelho.py:588`, `ferias/models.py:216`, `folha/export.py:491`.

**D11 — Desfazer férias: cancelar Ausencia × cancelar Agendamento.**

Lista de ausências `templates/ponto/ausencias.html:120`, `templates/ponto/ausencias.html:278` → `/ponto/ausencias/lote/cancelar/`, `ponto/views.py:2333` → `decidir`, `ponto/services/ausencia.py:897`. Compare com os quatro caminhos de D8. Rota: `ponto/urls.py:56`.

O lote aceita qualquer ausência não rejeitada, inclusive férias, e a rejeita; não devolve concedidos nem cancela Agendamento. O cancelamento de Agendamento altera o saldo/status e remove os espelhos. `ponto/views.py:2345`, `ponto/views.py:2351`, `ferias/services.py:333`, `ferias/services.py:353`, `ponto/services/ausencia.py:1188`.

**Muda dinheiro/dia: sim.** Cancelar somente Ausencia retira cobertura, mas o Agendamento continua no universo de gozo/retenção e o saldo não recebe devolução. `ponto/turnos.py:2514`, `ferias/services.py:51`, `ferias/models.py:216`, `folha/export.py:868`.

**Pares que convergem no mesmo escritor — separados para fechar o censo.**

| Par/grupo | Convergência e diferenças restantes |
|---|---|
| Resolver dia no calendário × diagnóstico; individual × seleção em lote | **Converge em `veredito_celula`**, pelo mesmo POST. Mesmos efeitos de ausência/fio/célula

[truncado em 20000 de 24964 caracteres]

## 06/11 21:27 · sessao 6868c22bd168 · - · - · origem=Ronald (tmux)

**C — identidade, banco e concorrência**

- **Pergunta:** chave de dedup real = `(disputa.colaborador_id, motivo, data_evento)`, atravessando todas as disputas e estados; lê em ordem de PK e reutiliza a última linha. Não inclui posição/hora do marco nem instância da tarefa. Perguntas chamadas sem data são tratadas como conversa e nascem sempre. `app/chamados/models.py:2381`, `app/chamados/models.py:2392`, `app/chamados/models.py:2398`, `app/chamados/models.py:2424`, `app/chamados/models.py:2431`.
- **Dois marcos usuais podem distinguir-se hoje:** fim/S vira `orfao_14h`; intervalo/S vira `intervalo_saida`; início/E e retorno/E também têm motivos diferentes. A distinção é indireta pelo motivo, não uma chave explícita de MARCO. Dois marcos com o mesmo motivo e data caem no mesmo dedup. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/models.py:2392`.
- **Data não é uniformemente jornada:** fábrica de intervalo toma a data civil fornecida no marco; teste noturno espera as duas perguntas no dia seguinte ao `data_turno`. `app/chamados/services/disputa_emissao.py:982`, `app/chamados/services/disputa_emissao.py:987`, `app/chamados/services/disputa_emissao.py:991`, `app/chamados/tests/test_fio_intervalo.py:62`, `app/chamados/tests/test_fio_intervalo.py:77`.
- **Disputa:** busca a aberta por colaborador, agrega motivos e contexto; não constitui tarefa individual por marco. Meta contém índice comum `(colaborador, fechada_em)`, sem unicidade parcial. `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:472`, `app/chamados/services/disputa_emissao.py:498`, `app/chamados/models.py:1742`.
- **Chamado:** não possui chave única universal; `abrir` usa os `**lookup` recebidos de cada emissor. `unique_together=[]`; os índices declarados são comuns, de estado/urgência/criação. Exemplo do pai de disputa: colaborador + categoria + módulo + estados vivos. `app/chamados/models.py:97`, `app/chamados/models.py:796`, `app/chamados/models.py:867`, `app/chamados/services/disputa_emissao.py:504`.
- **Não achei índice único parcial de tarefa aberta** em `ChamadoColaborador.Meta`, `DisputaSupervisao.Meta`, `PerguntaDisputa.Meta` nem no censo de constraints das migrations de chamados. Existe unique parcial em **Followup**, sobre `evento_chave` não nulo; não é identidade da tarefa. `app/chamados/models.py:97`, `app/chamados/models.py:1742`, `app/chamados/models.py:2857`, `app/chamados/migrations/0047_evento_chave_do_fio.py:20`.
- **Há locks hoje:** `ChamadoColaborador.abrir` abre atomic e trava a linha do colaborador antes do dedup; encerramento recarrega/trava a linha do chamado dentro de atomic; costura trava o destino se o chamador está em atomic. A emissão trava disputa aberta existente. Portanto seria incorreto dizer “não há select_for_update”. `app/chamados/models.py:863`, `app/chamados/models.py:866`, `app/chamados/models.py:668`, `app/chamados/services/fio.py:758`, `app/chamados/services/disputa_emissao.py:466`.
- **Corridas possíveis, por inspeção:** pergunta faz `.last()` e depois `create()` sem atomic/lock no próprio gate; disputa inexistente não oferece linha ao `select_for_update`, e o caminho cria depois de chamar `Chamado.abrir`, sem repetir a busca da disputa após adquirir o lock do colaborador; validação lê e testa `validada_em` antes do atomic, sem lock da pergunta; fechamento da disputa verifica estado/perguntas antes do atomic e depois salva sem lock. São riscos estruturais, não ocorrências demonstradas pelo anexo. `app/chamados/models.py:2431`, `app/chamados/models.py:2476`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`, `app/chamados/services/validacao.py:359`, `app/chamados/services/validacao.py:366`, `app/chamados/services/validacao.py:395`, `app/chamados/models.py:1820`, `app/chamados/models.py:1827`, `app/chamados/models.py:1846`.
- **Teste de concorrência encontrado:** duas threads no `Chamado.abrir`, espera exatamente um chamado e apenas uma criação. Não achei teste equivalente para duas validações/fechamentos simultâneos na busca por concorrência/race e locks desses serviços. `app/chamados/tests/test_c2_corrida_do_abrir.py:94`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/tests/test_c2_corrida_do_abrir.py:140`; equivalência para pergunta/disputa `[nao verificado]`.

**D — fechamento e recusa**

- **Hoje validação sozinha já mata a pergunta para o C6:** primeiro ramo de `_pergunta_viva_calc` retorna falso se `validada_em` existe; `estado` mostra `validada` sem materialização e `resolvida` com ambos os carimbos. `app/chamados/juizes.py:1135`, `app/chamados/models.py:2499`.
- **O caminho normal de hora tem salvaguarda concreta adicional:** materializa em atomic, exige ramo bem-sucedido e `_pergunta_materializada_de_fato`; reprovação lança exceção para rollback; depois aplica carimbo e pergunta ao C6, também revertendo a batida se recusado. `app/chamados/services/materializacao.py:1195`, `app/chamados/services/materializacao.py:1207`, `app/chamados/services/materializacao.py:1214`, `app/chamados/services/materializacao.py:1252`.
- **Essa consulta ao C6 não prova, isoladamente, pós-condição independente da validação:** `carimbar(resposta='validar')` pode preencher validação antes de perguntar ao juiz, cujo primeiro ramo já retorna morta pela validação. A checagem de fato anterior no materializador é que acrescenta a testemunha nesse caminho. `app/chamados/models.py:2591`, `app/chamados/models.py:2596`, `app/chamados/juizes.py:1135`, `app/chamados/services/materializacao.py:1208`.
- **Recusa de hora sem carimbo:** `validar_pergunta` carimba e executa a unidade dentro do mesmo atomic; `devolver_validacao_recusada` limpa `validada_em/por`, preserva a resposta, registra motivo e reabre disputa fechada por padrão. Resultado normal = `respondida`, viva para o **admin**, não nova cobrança de resposta ao colaborador. `app/chamados/services/validacao.py:180`, `app/chamados/services/validacao.py:187`, `app/chamados/services/validacao.py:199`, `app/chamados/services/validacao.py:395`, `app/chamados/services/validacao.py:422`, `app/chamados/services/validacao.py:460`; prova em `app/chamados/tests/test_o261_recusa_fala.py:95`.
- **Não generalizar essa devolução:** só vale para resposta de hora, com `materializada_em` nulo e recusa preenchida. Colisão/paridade já carimbadas e escapes não entram; `sem_reabrir=True` também permite devolver ao admin sem reabrir a disputa. `app/chamados/services/validacao.py:182`, `app/chamados/services/validacao.py:184`, `app/chamados/services/validacao.py:197`, `app/chamados/services/validacao.py:427`, `app/chamados/tests/test_o261_recusa_fala.py:190`, `app/chamados/tests/test_o261_recusa_fala.py:203`.
- **Alcance não é garantido apenas por “viva”:** resposta não validada dá dono `admin_validar`; crédito vai ao destino vivo ou ao pai, mas lista padrão exclui arquivado/histórico. Testes cobrem pergunta respondida sob destino fechado e pergunta fora do app, portanto estado e porta alcançável precisam ser conferidos separadamente. `app/chamados/catalogo/natureza.py:48`, `app/chamados/services/raiox.py:54`, `app/chamados/services/raiox.py:60`, `app/chamados/services/verbos.py:18`, `app/chamados/services/verbos.py:48`, `app/chamados/tests/test_o261e_f5_alcance.py:169`, `app/chamados/tests/test_o261e_f5_alcance.py:205`.
- **Disputa fecha normalmente** quando não existe resposta sem veredito nem fio vivo; materializador lê esse juiz e chama `fechar`. Há exceções explícitas: desligamento, admin com motivo, competência trancada; não são todas “efeito materializado + rejulgamento”. `app/chamados/models.py:1783`, `app/chamados/models.py:1789`, `app/chamados/models.py:1722`, `app/chamados/models.py:1852`, `app/chamados/services/materializacao.py:1288`.
- **Chamado não fecha automaticamente só pelo carimbo:** destino/clones passam por retenção de irmãs respondidas sem consumo e `reconciliar()`, que consulta premissa do furo. `app/chamados/services/materializacao.py:736`, `app/chamados/services/materializacao.py:746`, `app/chamados/models.py:680`.

**Compatibilidade do desenho com absorção de ausência**

- Pode separar fechamento de tarefa e avaliação, mas precisa preservar o identificador do ato de absorção ou substituí-lo explicitamente: a recuperação atual agrupa `(disputa_id, data_evento, materializada_em)`, procura um escape do tipo da ausência no grupo e devolve as irmãs sem batida viva originada nelas. `app/chamados/services/materializacao.py:585`, `app/chamados/services/materializacao.py:605`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:616`.
- **Quebraria:** deixar `materializada_em` nulo até aprovação/rejulgamento final de um atestado aberto, alterar cada irmã para timestamp diferente, mover parte do grupo para outra disputa ou apagar o escape do grupo: o filtro/grupo deixa de encontrar as absorvidas. Atualmente todas recebem o mesmo `agora`, inclusive com ausência ainda aberta; o chamado segue vivo/silenciado nessa fase. `app/chamados/services/materializacao.py:529`, `app/chamados/services/materializacao.py:549`, `app/chamados/services/materializacao.py:560`, `app/chamados/services/materializacao.py:607`, `app/chamados/services/materializacao.py:609`, `app/chamados/services/materializacao.py:614`.
- **Rejeição:** sem outra ausência cobrindo, desfaz absorção e reabre as disputas para devolver perguntas ao colaborador. Teste cobre pendência, vivacidade, disputa aberta, idempotência, outra ausência, aprovação sem devolução, batida já plantada e reabertura da mesma ausência. `app/chamados/services/materializacao.py:638`, `app/chamados/services/materializacao.py:645`, `app/chamados/services/materializacao.py:657`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:106`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:119`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:129`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:141`, `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:154`.

Testes foram somente lidos; resultados de execução `[nao verificado]`. [nao verificado]

## 06/11 21:27 · sessao d20cb5a00918 · - · - · origem=Ronald (tmux)

E6 — concorrência:

- O chamado já tem proteção parcial: `abrir` trava `Colaborador` antes do dedup; fechamento recarrega e trava o chamado dentro de transação; costura trava a mesma linha do destino. Isso serializa abertura e disputa entre costura e encerramento pelos caminhos protegidos (`app/chamados/models.py:863`, `app/chamados/models.py:661`, `app/chamados/services/fio.py:752`).
- A disputa ainda tem janela: `fechar` testa estado, respostas e perguntas antes de entrar no `atomic`; grava sem `select_for_update` ou recarga. Duas decisões sobre instâncias antigas podem divergir — inferência do fluxo (`app/chamados/models.py:1820`, `app/chamados/models.py:1827`, `app/chamados/models.py:1836`, `app/chamados/models.py:1846`, `app/chamados/models.py:1861`).
- A emissão trava somente uma disputa aberta já existente; na ausência dela, abre/reusa o pai e cria disputa sem repetir a consulta. Duas consultas inicialmente vazias podem criar duas disputas após o reuso do mesmo pai — inferência, sem reprodução (`app/chamados/services/disputa_emissao.py:466`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:523`). O modelo declara índice comum `(colaborador, fechada_em)`, sem unicidade nesse `Meta` (`app/chamados/models.py:1742`).
- Há teste real com duas threads para **abertura de chamado**, exigindo um único chamado e resultados `[False, True]`; não equivale a testar duas validações/fechamentos simultâneos (`app/chamados/tests/test_c2_corrida_do_abrir.py:94`, `app/chamados/tests/test_c2_corrida_do_abrir.py:127`, `app/chamados/tests/test_c2_corrida_do_abrir.py:140`).
- Há teste de SQL/ordem de lock para gates e costura, e teste com instância velha, mas sem duas threads nesses testes (`app/chamados/tests/test_o261e_f1_codex_v5.py:24`, `app/chamados/tests/test_o261e_f1_codex_v5.py:49`, `app/chamados/tests/test_o261e_f1_codex_v5.py:65`).
- Não achei teste real de duas transições simultâneas de pergunta/disputa em `test_porta_disputa_emissao.py`, `test_o261e_f1_codex_v5.py` e `test_pvmuda_carimbo_em_voo.py`; este último exercita sequências por máquina de estados, não threads (`app/chamados/tests/test_porta_disputa_emissao.py:185`, `app/chamados/tests/test_o261e_f1_codex_v5.py:24`, `app/chamados/tests/test_pvmuda_carimbo_em_voo.py:178`).

E7 — falha entre evento e entrega:

- Já existe outbox durável `EventoCelula`; registra identificadores/data e releitura do estado atual. Os índices parciais são **não únicos**, para pendentes; não é índice único de tarefa aberta (`app/escala/models.py:1845`, `app/escala/models.py:1857`, `app/escala/models.py:1865`).
- Célula salva veredito/ata e insere evento no mesmo `atomic`; falha do INSERT desfaz a escrita (`app/ponto/portas/celula.py:894`). `PerguntaDisputa.save` também envolve o `post_save` emissor em `atomic`, com savepoint; nascimento não emite, alterações relevantes emitem (`app/chamados/models.py:2847`, `app/chamados/signals.py:214`).
- Cobertura explícita: rollback célula/evento; rollback pergunta/evento; reconsumo idempotente e falha parcial que deixa pendência para a próxima execução (`app/ponto/tests/test_o_outbox_celula.py:142`, `app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:300`, `app/ponto/tests/test_o_outbox_celula.py:163`, `app/ponto/tests/test_o_outbox_celula.py:209`).
- Consumidor processa o estado atual; falha incrementa tentativa/erro e mantém pendente, sucesso marca `processado_em`; após cinco tentativas o evento sai do lote vivo e passa a “morto”. Adiamento exclusivamente pelo teto mantém pendente sem consumir tentativa (`app/chamados/reconciliador.py:1196`, `app/chamados/reconciliador.py:1235`, `app/chamados/reconciliador.py:1304`, `app/chamados/reconciliador.py:1377`).
- O comando usa advisory lock por schema; chamar diretamente `consumir_eventos_celula` não adquire esse lock no corpo da função (`app/chamados/management/commands/consumir_eventos_celula.py:17`, `app/chamados/reconciliador.py:1213`, `app/chamados/reconciliador.py:1304`).
- Nem todo efeito secundário está no outbox: devolução de perguntas absorvidas por ausência e reconciliação são `on_commit` com blocos `engolir`; falha não desfaz ausência já gravada nem há reentrega durável nesses blocos — inferência local (`app/ponto/signals.py:250`, `app/ponto/signals.py:260`, `app/ponto/signals.py:272`). Recalcular fechamento na validação captura exceção e segue (`app/chamados/services/validacao.py:405`).
- Cuidado com o teste chamado `test_a_trava_real_exclui_a_segunda_sessao...`: ele entra duas vezes **na mesma sessão**, espera sucesso reentrante, e deixa a exclusão entre sessões como comentário; não demonstra duas sessões (`app/ponto/tests/test_o_outbox_celula.py:497`).

Sobre a ordem proposta:

- Já há vigia lendo pergunta × chamado × célula e donos de ação; ele não é um process manager de tarefa novo (`app/chamados/services/vigia_fio.py:4`, `app/chamados/services/vigia_fio.py:16`, `app/chamados/services/vigia_fio.py:169`).
- O universo do vigia preserva passivo por partir dos chamados vivos, sem janela temporal; isso é comportamento declarado do módulo, não prova dos números históricos citados em comentários (`app/chamados/services/vigia_fio.py:35`).
- O fluxo atual já põe julgamento antes do consumidor da batida e outbox antes dos antigos crons de cura; o cron `reconciliar_chamados` tem teste impedindo voltar a drenar o outbox (`app/ponto/signals.py:169`, `app/chamados/reconciliador.py:1347`, `app/ponto/tests/test_o_outbox_celula.py:482`).
- Não achei implementação ou teste da sequência integral “nova tarefa → sombra → app/admin no mesmo pouso → retirada dos ponteiros” nas áreas inspecionadas; tratar aprovação dessa ordem como recomendação do desenho, `[nao verificado]`. [nao verificado]

Testes existentes foram lidos, não executados; resultados nesta cópia `[nao verificado]`. [nao verificado]

## 06/11 21:27 · sessao 10c9fd12abfa · DUAS-TELAS-E-PWA-OFFLINE · censo · origem=Code

**Resultado: 33 views com escrita POST administrativa no recorte; 36 ações semânticas; 10 grupos divergentes com telas identificadas.** Há ainda dois cruzamentos entre escritores de Batida e nove ações que convergem, relacionados abaixo. A contagem distingue ação de endpoint: aprovação individual/lote conta uma ação; assumir/liberar contam duas; criar regularização em colaboradores repete a ação existente em chamados. Fontes do inventário: `app/chamados/urls.py:8`, `app/ponto/urls.py:8`, `app/colaboradores/urls.py:87`, `app/api/urls.py:9`.

**Ocorrências, pessoas afetadas, valores e execução em produção: [não verificado].** Os impactos abaixo são possibilidades demonstradas pelos caminhos do código. A referência adotada é escritor único, conforme `app/docs/GLOSSARIO.md:12`, `app/docs/PLANO-PADROES.md:46` e `app/docs/JUIZES.md:37`.

**Universo varrido**

| App | Views POST administrativas do recorte |
|---|---|
| chamados — **22** | `views.py`: abrir:147, followup:274, detalhe local:359, fio:659, modal:869, mensagem rápida:961, atribuir:1008, conferir data:1292, reabrir questionário:1372. `views_cobrar.py`: cobrar:24, massa:61, validar inline:145, validar lote:232, cobrar dia:306, validar lote tela:372. `views_arquivar.py`: arquivar:24, desarquivar:52, arquivar lote:93. `views_regularizacao.py`: criar:42, adicionar item:74, excluir item:137, enviar:154. Todos sob `app/chamados/`. |
| ponto — **10** | Offline pendentes:434; aprovar lote:1763; rejeitar lote:1859; aprovar individual:1911; rejeitar individual:1923; materializar saída:3106; flip:3260; retratar:3274; desfazer:3286; resolver dia:3312. Todas em `app/ponto/views.py`. |
| colaboradores — **1** | `colab_regularizar`, `app/colaboradores/views.py:2948`. |
| api — **0** | As escritas pertinentes operam sobre o próprio colaborador: `app/api/views.py:2608`, `app/api/views.py:2853`, `app/api/views.py:2998`, `app/api/views_core.py:1171`, `app/api/views_core.py:1316`. Os endpoints mensageria “validar”, “decidir” e “resolver-dia-recusado” são leitores: `app/api/views_mensageria.py:523`, `app/api/views_mensageria.py:1134`, `app/api/views_mensageria.py:1847`. |

**Endereços das telas usados nos pares**

| Sigla | Rota → view → template |
|---|---|
| **D** | `/chamados/local/<id>/` → `detalhe_chamado_local`, `app/chamados/views.py:359` → `chamados/detalhe_local.html`, render em `app/chamados/views.py:550`. |
| **M** | `/chamados/modal/<colab>/` → `modal_fio_colaborador`, `app/chamados/views.py:869` → `chamados/partials/modal_fio.html`, render em `app/chamados/views.py:949`; rota em `app/chamados/urls.py:39`. |
| **F** | `/chamados/colaborador/<colab>/` → `fio_colaborador`, `app/chamados/views.py:659` → `chamados/fio_colaborador.html`, render em `app/chamados/views.py:697`. |
| **T** | `/chamados/local/<id>/painel/` → `painel_chamado_turno`, `app/chamados/views.py:347` → `chamados/partials/painel_chamado_turno.html`, render em `app/chamados/views.py:353`. |
| **C** | `/colaboradores/<cid>/calendario/` → `calendario_ponto`, `app/colaboradores/views.py:1935` → `colaboradores/calendario_partial.html`, render em `app/colaboradores/views.py:1962`. |
| **G** | `/colaboradores/colab/<cid>/diagnostico-drawer/` → `colab_diagnostico_drawer`, `app/colaboradores/views.py:2938` → `colaboradores/partials/_drawer_diagnostico.html`, render em `app/colaboradores/views.py:2942`. |
| **J** | `/ponto/justificativa/<jid>/painel/` → `painel_justificativa`, `app/ponto/views.py:1662` → `ponto/partials/painel_justificativa.html`; formulários em `app/templates/ponto/partials/painel_justificativa.html:51`. |
| **L** | `/ponto/justificativa/fila/` → `fila_justificativas`, `app/ponto/views.py:1695` → `ponto/fila_justificativas.html`, render em `app/ponto/views.py:1749`. |

**Grupos divergentes**

1. **Aprovar justificativa / materializar sua saída — três caminhos.**

   **A: J**, formulário `app/templates/ponto/partials/painel_justificativa.html:51` → POST `/ponto/justificativa/<jid>/aprovar/`, view `app/ponto/views.py:1911` → escritor `aprovar_justificativa_dp`, `app/ponto/services/justificativa.py:117`.

   **B: L**, formulário de lote `app/templates/ponto/fila_justificativas.html:176` → POST `/ponto/justificativa/aprovar-lote/`, view/escritor `app/ponto/views.py:1763`; grava diretamente em `app/ponto/views.py:1796`.

   **C:** `/ponto/justificativa/<jid>/materializar-saida/`, view `app/ponto/views.py:3106`, template `app/templates/ponto/materializar_saida_retroativa.html:38` → escritor `materializar_saida_dp`, `app/ponto/services/justificativa.py:316`.

   **Difere:** A exige saída posterior à entrada e só cria S quando existe entrada; B não faz essa comparação e cria S tendo saída informada, mesmo sem entrada. A retorna antes de aprovar quando já existe batida gerada; B aprova e pula apenas a criação. Fontes: `app/ponto/services/justificativa.py:137`, `app/ponto/services/justificativa.py:169`, `app/ponto/views.py:1796`, `app/ponto/views.py:1808`.

   A fecha também bloqueio de entrada e acrescenta LogAuditoria/followup; B encerra a origem sem esses atos explícitos. C exige motivo de dez caracteres, não exige status pendente, grava a saída informada e executa cascata dos chamados do turno. Fontes: `app/ponto/services/justificativa.py:203`, `app/ponto/services/justificativa.py:214`, `app/ponto/views.py:1837`, `app/ponto/views.py:3135`, `app/ponto/services/justificativa.py:329`, `app/ponto/services/justificativa.py:365`, `app/ponto/services/justificativa.py:378`.

   **Leitor/gravidade: sim, pode mudar dia e dinheiro.** O fechamento inclui justificativas aprovadas em `datas_justificadas`; o motor usa isso para falta, abono e DSR. Fontes: `app/ponto/services/fechamento.py:660`, `app/ponto/motor_calculo_v2.py:2600`, `app/ponto/motor_calculo_v2.py:2605`, `app/ponto/motor_calculo_v2.py:2620`.

2. **Rejeitar justificativa — individual × lote.**

   **A: J**, formulário `app/templates/ponto/partials/painel_justificativa.html:58` → POST `/ponto/justificativa/<jid>/rejeitar/`, view `app/ponto/views.py:1923` → escritor `app/ponto/services/justificativa.py:247`.

   **B: L**, formulário `app/templates/ponto/fila_justificativas.html:188` → POST `/ponto/justificativa/rejeitar-lote/`, view/escritor `app/ponto/views.py:1859`.

   **Difere:** A usa transação, trava a origem e a justificativa, abre chamado de rejeição e acrescenta followup. B faz leitura/save diretamente e envia push, sem esses mesmos atos. Fontes: `app/ponto/services/justificativa.py:258`, `app/ponto/services/justificativa.py:262`, `app/ponto/services/justificativa.py:276`, `app/ponto/services/justificativa.py:309`, `app/ponto/views.py:1874`, `app/ponto/views.py:1880`.

   **Leitor/gravidade:** o fio recebe chamado/followup somente pelo caminho individual; todo save também rejulga o dia. Na execução isolada sobre o mesmo pendente, ambos gravam rejeitada; **mudança adicional de dinheiro por concorrência é [não verificado]**, embora o lote não tenha a mesma proteção contra decisão concorrente. Fontes: `app/ponto/services/justificativa.py:268`, `app/ponto/views.py:1875`, `app/ponto/signals.py:161`.

3. **Validar pergunta — detalhe × modal × inline × lote.**

   **A: D**, formulário `app/templates/chamados/partials/_pergunta_card.html:92` → ramo `app/chamados/views.py:483`.

   **B: M**, formulário `app/templates/chamados/partials/modal_fio.html:274` → `processar_post_fio`, `app/chamados/services/fio.py:581`.

   **C: Central `/chamados/painel/`**, view `app/chamados/views.py:721`, botão `app/templates/chamados/partials/_lista_chamados.html:82` → `/chamados/validar-inline/<cid>/`, view `app/chamados/views_cobrar.py:145`.

   **D: prévia de lote**, template `app/templates/chamados/partials/_lote_preview.html:21` → `/chamados/validar-lote/tela/`, view `app/chamados/views_cobrar.py:372`; copiloto também chama `/chamados/validar-lote/`, view `app/chamados/views_cobrar.py:232`, por `app/templates/core/_copiloto.html:208`.

   **Converge em `validar_pergunta`, `app/chamados/services/validacao.py:343`, mas difere antes do escritor:** detalhe exige disputa aberta; modal aceita pergunta viva em disputa fechada, passa `sem_reabrir=True` e acrescenta trilha específica. Inline percorre as respondidas vivas da disputa; lote só aceita classe A e reclassifica cada item. Fontes: `app/chamados/views.py:493`, `app/chamados/services/fio.py:596`, `app/chamados/services/fio.py:605`, `app/chamados/services/fio.py:611`, `app/chamados/views_cobrar.py:169`, `app/chamados/services/validacao.py:688`.

   **Leitor/gravidade: sim, pode mudar dia e dinheiro**, porque a diferença de alcance decide se a resposta chega à materialização e ao recálculo. Fontes: `app/chamados/services/validacao.py:406`, `app/chamados/services/validacao.py:423`, `app/ponto/services/espelho.py:1022`, `app/ponto/services/fechamento.py:1193`.

4. **Regularizar/fechar disputa — detalhe × modal.**

   **A: D**, formulário `app/templates/chamados/detalhe_local.html:315` → `app/chamados/views.py:467` → orquestrador `app/chamados/services/acoes_disputa.py:118`.

   **B: M**, formulário `app/templates/chamados/partials/modal_fio.html:348` → orquestrador duplicado `app/chamados/services/fio.py:642`.

   **Difere:** A procura disputa aberta somente por `chamado=chamado`; B usa `_disputa_do_chamado`, que também encontra a disputa pela pergunta cujo destino é aquele chamado. Fontes: `app/chamados/views.py:470`, `app/chamados/services/fio.py:564`, `app/chamados/services/fio.py:134`.

   **Converge em `materializar_perguntas_validadas_da_disputa`, `app/chamados/services/materializacao.py:1313`**, chamado pelos dois em `app/chamados/services/acoes_disputa.py:139` e `app/chamados/services/fio.py:666`.

   **Leitor/gravidade: sim, pode mudar dia/dinheiro.** Um caminho pode recusar o chamado do furo que o outro alcança; o escritor materializa batidas/ausências, fecha destinos e fecha a disputa quando quitada. Fontes: `app/chamados/services/materializacao.py:1319`, `app/chamados/services/materializacao.py:1326`, `app/chamados/services/materializacao.py:1308`.

5. **Responder e mudar status/resolver chamado — detalhe/painel × fio/modal × GLPI.**

   **A: D e T**, formulários `app/templates/chamados/detalhe_local.html:385` e `app/templates/chamados/partials/painel_chamado_turno.html:68` → detalhe local → `acao_resposta_livre`, `app/chamados/services/acoes_chamado.py:283`.

   **B: F e M**, formulários `app/templates/chamados/fio_colaborador.html:257` e `app/templates/chamados/partials/modal_fio.html:649` → `processar_post_fio`, `app/chamados/services/fio.py:704`.

   **Difere:** A registra nota `resposta_livre` e logs adicionais `responder`/`status`; B registra `mensagem_dp`. A oferece “Reabrir” como `aberto`; F/M não oferecem essa opção. No HTTP, A aceita status sem texto, B exige texto; os formulários HTML exigem texto nos dois lados. Fontes: `app/chamados/services/acoes_chamado.py:292`, `app/chamados/services/acoes_chamado.py:295`, `app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:705`, `app/chamados/services/fio.py:714`, `app/templates/chamados/detalhe_local.html:398`, `app/templates/chamados/fio_colaborador.html:267`, `app/templates/chamados/detalhe_local.html:387`.

   **O estado converge em `ChamadoColaborador.transicionar`, `app/chamados/models.py:122`, com auditoria canônica comum** em `app/chamados/models.py:274`. Portanto, o fio tem auditoria de estado; faltam nele os logs adicionais citados.

   **C: `/chamados/<ticket_id>/`**, view `app/chamados/views.py:219`, template `app/templates/chamados/detalhe.html:122` → `/chamados/<ticket_id>/followup/`, view `app/chamados/views.py:274` → escritor externo `GLPIClient.adicionar_followup`, `app/chamados/glpi_client.py:219`. Essa tela oferece `novo_status`, mas o handler só envia comentário e não consome esse campo. Fontes: `app/templates/chamados/detalhe.html:130`, `app/chamados/views.py:292`, `app/chamados/views.py:303`.

   **Leitor/gravidade:** timeline e estado/perguntas do chamado veem a diferença; os sinais comuns estão em `app/chamados/signals.py:76`. **Não há escrita direta de dinheiro nesses handlers**; efeito financeiro decorrente dessa diferença: **[não verificado]**. Fontes dos atos: `app/chamados/services/acoes_chamado.py:283`, `app/chamados/services/fio.py:704`, `app/chamados/views.py:303`.

6. **Abrir mensagem para colaborador — formulário completo × mensagem rápida.**

   **A:** `/chamados/abrir/`, view `app/chamados/views.py:147`, template `app/templates/chamados/form_abertura.html:127` → escritor `abrir_chamado_para_colaborador`, `app/chamados/services/abertura.py:122`.

   **B:** `/colaboradores/<cid>/painel/`, view `app/colaboradores/views.py:915`, template `app/templates/colaboradores/partials/painel_colaborador.html:188` → POST `/chamados/colaborador/<cid>/mensagem-rapida/`, view `app/chamados/views.py:961` → `ChamadoColaborador.abrir`, chamado em `app/chamados/views.py:973`.

   **Difere, comparando `mensagem_dp`:** A deduplica pela entrada completa, grava descrição, prazo, autor/contexto e log. B reutiliza mensagem DP viva e acrescenta ocorrência. Fontes: `app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:194`, `app/chamados/views.py:973`, `app/chamados/views.py:986`.

   **Leitor/gravidade: não muda dia/dinheiro diretamente.** Muda a composição do fio: chamado próprio com descrição versus ocorrência em chamado reutilizado. Fontes: `app/chamados/services/abertura.py:177`, `app/chamados/views.py:986`, `app/chamados/views.py:997`.

7. **Criar regularização externa — painel de lotes × drawer.**

   **A:** `/chamados/regularizacao/colab/<cid>/`, view `app/chamados/views_regularizacao.py:26`, template `app/templates/chamados/regularizacao_painel.html:14` → POST `.../criar/`, view `app/chamados/views_regularizacao.py:42` → `abrir_lote`, `app/chamados/services/regularizacao_ext.py:309`.

   **B: G**, formulário `app/templates/colaboradores/partials/_form_regularizar.html:2` → `/colaboradores/colab/<cid>/regularizar/`, view `app/colaboradores/views.py:2948` → `criar_regularizacao_externa`, `app/chamados/services/regularizacao_ext.py:230`.

   **Converge em `criar_regularizacao_externa`, mas difere:** A exige motivo textual, abre lote vazio e deduplica esse lote; B exige categoria e itens, permite observação vazia e chama diretamente o criador. A usa gate gestor/departamento supervisão; B usa `tem_acao('regularizar_batida')`. Fontes: `app/chamados/views_regularizacao.py:19`, `app/chamados/views_regularizacao.py:48`, `app/chamados/services/regularizacao_ext.py:312`, `app/colaboradores/views.py:2957`, `app/colaboradores/views.py:2961`, `app/colaboradores/views.py:2981`.

   **Leitor/gravidade:** a categoria altera o agrupamento de métricas; os itens alimentam a materialização confirmada. **Sim, potencialmente muda dia/dinheiro**, porque B aceita itens sem os mesmos controles posteriores de A, incluindo a recusa de entrada=saída. Fontes: `app/chamados/services/regularizacao_ext.py:277`, `app/chamados/views_regularizacao.py:120`, `app/colaboradores/views.py:2976`, `app/chamados/services/regularizacao_ext.py:179`.

8. **Retratar batida — calendário × drawer.**

   **A: C; B: G.** Ambos incluem o formulário em `app/templates/colaboradores/partials/_calendario_grade.html:121` → POST `/ponto/batida/<id>/retratar/`, view `app/ponto/views.py:3274` → **converge em `retratar`, `app/ponto/services/retratar_humano.py:150`**.

   **Difere:** C fornece `motivos_retratacao`; o retorno do contexto de G não fornece essa chave. O formulário exige motivo e o escritor exige cadastro ativo. Fontes: `app/colaboradores/views.py:1958`, `app/colaboradores/services/drawer.py:139`, `app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/ponto/services/retratar_humano.py:166`.

   **Leitor/gravidade: sim, pode mudar dia/dinheiro.** A retratação tira a marcação do universo apurável usado pelo espelho. O impedimento do formulário no drawer é inferido do código; reprodução em navegador: **[não verificado]**. Fontes: `app/ponto/turnos.py:2864`, `app/ponto/services/espelho.py:1022`.

9. **Inverter E/S — fila de flip × calendário/drawer.**

   **A:** `/ponto/flip-fila/`, view `app/ponto/views.py:3299`, template `app/templates/ponto/flip_fila.html:16`.

   **B: C e G**, formulário comum em `app/templates/colaboradores/partials/_calendario_grade.html:121`.

   Todos → POST `/ponto/batida/<id>/flip/`, view `app/ponto/views.py:3260` → **converge em `flip_tipo`, `app/ponto/services/flip_batida.py:10`**.

   **Difere:** a fila envia motivo descritivo; calendário/drawer omitem o campo, aceito pelo escritor. A descrição da trilha fica diferente e o leitor `flips_de` recupera esse motivo. Fontes: `app/templates/ponto/flip_fila.html:16`, `app/templates/colaboradores/partials/_calendario_grade.html:121`, `app/ponto/services/flip_batida.py:31`, `app/ponto/services/flip_batida.py:75`.

   **Gravidade da diferença: não muda dinheiro/dia; muda a explicação auditável.** A troca de tipo e o recompute usam o mesmo caminho. Fontes: `app/ponto/services/flip_batida.py:30`, `app/ponto/services/flip_batida.py:39`.

10. **Cobrar por push — individual × massa.** [nao verificado]

    **A: M e Central**, botões `app/templates/chamados/partials/modal_fio.html:345` e `app/templates/chamados/partials/_lista_chamados.html:84` → `/chamados/cobrar/<cid>/`, view/escrita `app/chamados/views_cobrar.py:24`.

    **B: Central**, template `app/templates/chamados/painel_gestao.html:59` → `/chamados/cobrar/massa/`, view/escrita `app/chamados/views_cobrar.py:61`.

    **Difere:** massa restringe aos vivos da fila “cobrar”, exige `muda_a_folha` e escolhe um chamado por colaborador. Individual parte do chamado solicitado sem esses filtros. Ambos aplicam limite diário por colaborador e gravam ocorrência após envio, com tipos diferentes. Fontes: `app/chamados/views_cobrar.py:29`, `app/chamados/views_cobrar.py:36`, `app/chamados/views_cobrar.py:76`, `app/chamados/views_cobrar.py:95`, `app/chamados/views_cobrar.py:97`, `app/chamados/views_cobrar.py:117`, `app/chamados/views_cobrar.py:132`.

    **Leitor/gravidade: não muda dinheiro/dia diretamente; muda quem recebe cobrança e a nota no fio.** O filtro exclusivo da massa exclui chamados sem data e competência exportada. Fontes: `app/chamados/services/cobranca_folha.py:54`, `app/chamados/services/cobranca_folha.py:69`.

**Dois cruzamentos adicionais de escritores de Batida — separados da contagem dos dez grupos**

| Ações existentes | Caminhos e diferença | Leitor / gravidade |
|---|---|---|
| **Corrigir tipo na regularização × inverter E/S** | A tela `/chamados/regularizacao/<reg>/editar/`, view `app/chamados/views_regularizacao.py:60`, formulário `app/templates/chamados/regularizacao_editar.html:45`, prepara `corrigir`; após confirmação, o escritor altera `tipo` e `timestamp` diretamente em `app/chamados/services/regularizacao_ext.py:153`. Flip, nas telas C/G/fila, passa por `app/ponto/services/flip_batida.py:10` → `anotar`, com guarda de retratada/competência e trilha de flip. Fontes: `app/ponto/services/flip_batida.py:15`, `app/ponto/services/flip_batida.py:21`, `app/ponto/services/flip_batida.py:30`. | **Sim, pode mudar dia/dinheiro:** tipo/hora alteram o pareamento; a trilha específica de flip também deixa de ser a mesma. Leitores: `app/ponto/services/espelho.py:1022`, `app/ponto/services/flip_batida.py:66`. |
| **Remover na regularização × retratar marcação** | A mesma tela oferece `remover`, `app/templates/chamados/regularizacao_editar.html:51`; após confirmação, executa `alvo.delete()`, `app/chamados/services/regularizacao_ext.py:136`. C/G retratam preservando a Batida, por `app/ponto/registro_batida.py:267`. O desfazer humano só reconhece retratação da sua própria porta, `app/ponto/services/retratar_humano.py:199`. | **Sim, muda o fato disponível e a reversibilidade:** exclusão versus registro preservado fora da apuração. O universo apurável filtra retratação em `app/ponto/t

[truncado em 20000 de 24883 caracteres]

## 06/11 21:26 · sessao 82a55b25c563 · FIO-COM-ASSENTO-E0-V3 · leitura · origem=Code

| Pergunta | Resultado da conferência |
|---|---|
| **(1) Juízes** | Os endereços existem, mas respondem perguntas diferentes: acusação do **dia**, sustentação de **classe de chamado**, cobertura de **ausência**, geometria de **turno** e oferta de **ação na tela**. Não formam, por si, uma autoridade de exigibilidade por motivo. `app/escala/models.py:1767`; `app/chamados/catalogo/premissa.py:144`; `app/colaboradores/services/calendario.py:97`. |
| **(2) Censo** | Há portas de escrita já centralizadas por campo/entidade, mas leitores têm universos diferentes. Pergunta não grava `status`; disputa também não: usam, respectivamente, estado derivado e `fechada_em`. `app/chamados/models.py:2492`; `app/chamados/models.py:1763`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:36`. |
| **(3) Contradições** | A1.2 reaproveita qualquer estado; identidade não inclui episódio; correção de dia preenchido não é re-chaveamento; há ressuscitação explícita. Outbox e preservação de respostas já existem, com contratos diferentes do v3. `app/chamados/models.py:2398`; `app/chamados/services/fio.py:832`; `app/chamados/models.py:1461`; `app/chamados/services/disputa_emissao.py:1393`. |
| **(4) Exigibilidade** | **Não achei** função equivalente à pergunta completa. As aproximações trabalham sobre pergunta existente, chamado existente ou célula, sem o mesmo universo. `app/chamados/juizes.py:822`; `app/chamados/juizes.py:1886`; `app/chamados/juizes.py:1341`. |
| **(5) Cortes e faltas** | Retiraria a apresentação de mecanismos existentes como novidade; reformularia “PM não julga” e S4. Faltam contrato de universo, desconhecido, veto, horizonte, colisão de identidade e distinção entre porta, permissão e entrega. `CLAUDE.md:189`; `CLAUDE.md:275`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:145`; `app/ponto/services/furos_vetados.py:27`. |

[Não verificado] Incidência e contagens em produção, execução dos selos e autenticação do HASH como HEAD: não houve acesso a banco/log; o anexo contém o lookup, sem ocorrências; `git rev-parse HEAD` falhou nesta cópia por ausência de repositório Git utilizável. A análise abaixo é estática.

**(1) Endereço, assinatura, pergunta e universo dos juízes**

| Item citado | Assinatura e endereço | O que responde de fato; limite para o v3 |
|---|---|---|
| `VEREDITOS_ACUSAM` | **Constante**, sem assinatura: `CelulaDia.VEREDITOS_ACUSAM`. `app/escala/models.py:1767`. | Pertencimento do veredito do **dia** a `furo`, `discordante`, `indefinida`, `cobranca_indevida`, `cobrado`, `nunca_bateu`, `fato_em_ausencia`. Não discrimina motivo/marco: a pergunta pode morrer pelo seu marco aceso enquanto outro marco mantém o dia acusando. `app/escala/models.py:1767`; `app/chamados/juizes.py:1173`. |
| `premissa_morta` | `(modulo_origem, subcategoria=None, fatos: Optional[Mapping]=None) -> bool`. `app/chamados/catalogo/premissa.py:144`. | Classe conhecida cujo fato sustentador foi informado como falso. Classe desconhecida ou fato não coletado **não** produz premissa morta. Recebe fatos do coletor; não consulta colaborador, dia ou banco. Seu complemento lógico não prova exigibilidade. `app/chamados/catalogo/premissa.py:115`; `app/chamados/catalogo/premissa.py:124`; `app/chamados/catalogo/premissa.py:149`. |
| `ausencia_cobre` | `(colab, data_ref, *, para)`. `app/ponto/turnos.py:2536`. | Retorna **uma ausência vencedora**, por maior data/PK, que cobre a data. Para cobrança exclui rejeitadas; para folha aceita somente aprovadas. Não responde sozinho se cobre o dia inteiro ou o marco cobrado. `app/ponto/turnos.py:2509`; `app/ponto/turnos.py:2542`; `app/ponto/turnos.py:2552`. |
| `dia_sem_trabalho` | `(cel) -> bool`. `app/chamados/juizes.py:413`. | Cala por `trabalha is False` ou `ata.tipo_dia` preenchido e diferente de trabalho. Na ausência parcial, só `trabalha=False` cala. `None` não equivale a folga. Não é o previsto calculado da escala. `app/chamados/juizes.py:418`; `app/chamados/juizes.py:422`. |
| `STATUS_QUE_PEDEM_DECISAO` | **Tupla**: `('falta', 'irregular', 'aberto', 'sem_certidao')`. O predicado é `resolver_dia_oferecido(dia, hoje)`. `app/colaboradores/services/calendario.py:94`; `app/colaboradores/services/calendario.py:97`. | Oferta do botão “Resolver dia”: **último recurso + passado + status**. A permissão do usuário fica fora. Usar apenas a tupla perde duas condições; usar o resultado como prova de responsável alcançável acrescenta uma condição que ele não verifica. `app/colaboradores/services/calendario.py:100`. |
| “Cobertura parcial” | Família: `cobre_parte_do_dia(a, parciais=None)`; `ausencia_explica_o_dia_inteiro(a, parciais=None)`; `periodo_da_parcial_no_dia(a, horas)`; `ausencia_parcial_cobra_o_resto(registro)`. `app/ponto/turnos.py:2583`; `app/ponto/turnos.py:2668`; `app/ponto/turnos.py:2614`; `app/chamados/juizes.py:400`. | São perguntas distintas: parcialidade do registro; explicação integral; relação com período declarado; leitura do carimbo da grade/ata. A parcialidade considera tipo que aceita horas, um dia e minutos abaixo de 480; não basta a flag do tipo. `app/ponto/turnos.py:2565`; `app/ponto/turnos.py:2575`; `app/chamados/juizes.py:410`. |
| “Juiz do turno” | Autoridade registrada: `turnos_do_colab(colab, ini, fim, celulas=None)`. Corpo do dia: `_data_do_turno(entrada, vespera, saida=None, cont_max_s=14*3600)`. `app/core/juizes.py:196`; `app/ponto/turnos.py:1848`; `app/ponto/turnos.py:246`. | Geometria de turnos a partir de batidas/marcos, com tratamento da véspera e cruzamento de meia-noite. Não oferece sozinho dia canônico para toda ocorrência sem batida/turno. `app/ponto/turnos.py:247`; `app/ponto/turnos.py:1860`. |

Há formas de cobertura com universos diferentes: `cobertura_ausencia_periodo(colab, ini, fim, *, para)` responde o período; `ausencias_do_periodo(...)` entrega **todas** as ausências que o tocam, sem o desempate da ausência vencedora. Não são duplicatas intercambiáveis de `ausencia_cobre`. `app/ponto/turnos.py:2733`; `app/ponto/turnos.py:2521`.

O dia do **chamado** tem outras autoridades: `data_do_chamado(modulo_origem, contexto_json)` lê a chave declarada do catálogo; `dia_do_fato(chamado, fortes=False)` prioriza célula, catálogo e `data_turno`, acrescentando fontes fracas quando permitido. Nenhuma dessas funções canonicaliza o chamado executando o pareador. `app/chamados/catalogo/modulos.py:219`; `app/chamados/juizes.py:80`; `app/chamados/juizes.py:120`.

A comparação com o anexo confirma a separação entre geometria, cobertura e dia do chamado. Confirma também a exceção parcial e a divergência declarada de `_protesta`: ele continua lendo somente `trabalha is not False` e marcos acesos, sem a segunda cláusula de `dia_sem_trabalho`. `app/docs/JUIZES.md:20`; `app/docs/JUIZES.md:50`; `app/docs/JUIZES.md:56`; `app/docs/JUIZES.md:59`; `app/docs/JUIZES.md:70`; `app/ponto/services/cartorio.py:1407`.

Não achei `STATUS_QUE_PEDEM_DECISAO` ou `resolver_dia_oferecido` na tabela de `app/docs/JUIZES.md:20` até seu encerramento em `app/docs/JUIZES.md:107`. O próprio documento se declara lookup, subordinado ao registro vivo e ao código. `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:14`.

**(2) Censo por fronteira de decisão**

O censo abaixo distingue portas que atribuem os campos, operações que as acionam e consumidores. A exclusão absoluta de escritores dinâmicos e o resultado atual dos testes ficam [não verificado]; o selo AST declara limites para receptores genéricos e argumentos não resolvidos. `app/chamados/tests/test_selo_status_so_pelas_portas.py:15`.

| Fronteira | Objeto | Escritores, operações e leitores |
|---|---|---|
| **Escrita** | Identidade, `data_evento`, `chamado_destino` | `PerguntaDisputa.abrir`: nascimento, dedup e reapontamento; `fio.reapontar_resolvedora`: destino e preenchimento de data ausente, com relavra no mesmo save. O contrato enumera **dois módulos** para esses campos. `app/chamados/models.py:2398`; `app/chamados/services/fio.py:784`; `app/core/tests/test_contract_escritores_estado.py:48`. |
| **Escrita** | Resposta e validação da pergunta | Resposta humana em `responder_pergunta_disputa_core`; resposta automática e reconstrução do instante em portas próprias. Validação/devolução em `validacao`; reabertura para correção preserva resposta anterior e limpa os campos atuais. `app/chamados/services/disputa_emissao.py:1282`; `app/chamados/services/disputa_emissao.py:1422`; `app/chamados/services/disputa_emissao.py:1439`; `app/chamados/services/validacao.py:131`; `app/chamados/services/validacao.py:187`; `app/chamados/services/disputa_emissao.py:1393`. |
| **Escrita** | Materialização/via da pergunta | Núcleo `_escrever_carimbo`; portas `carimbar`, `carimbar_identidade`, `descarimbar`, `descarimbar_em_lote`; operações próprias de encerramento/reabertura por via. `app/chamados/models.py:2556`; `app/chamados/models.py:2562`; `app/chamados/models.py:2625`; `app/chamados/models.py:2646`; `app/chamados/models.py:2692`; `app/chamados/models.py:2702`. |
| **Escrita** | Estado do chamado | `_gravar_estado` centraliza mudança persistida; `abrir` e `criar` escrevem nascimento. `_vivas_entre` altera somente memória para julgamento do pós-estado. A dispersão dos chamadores não significa escritores livres de `status_local`. `app/chamados/models.py:195`; `app/chamados/models.py:796`; `app/chamados/models.py:1015`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:36`. |
| **Escrita** | Abertura/fechamento da disputa | `fechar`, `reabrir_admin`, `reabrir_sistema`; fechamento grava `fechada_em`, autor, resolução e motivo em transação. Não há coluna `status` equivalente à do chamado. `app/chamados/models.py:1796`; `app/chamados/models.py:1845`; `app/chamados/models.py:2040`; `app/chamados/models.py:2077`; `app/chamados/tests/test_selo_status_so_pelas_portas.py:47`. |
| **Escrita** | Célula consumida pela cobrança | Geração, regeneração, edição/reversão do DNA; `lavrar_veredito` para ata/veredito/via/instante/protestos/status; `carimbar` para impressão, ausência e presença de entrada/saída. `app/ponto/portas/celula.py:110`; `app/ponto/portas/celula.py:534`; `app/ponto/portas/celula.py:740`; `app/ponto/portas/celula.py:811`; `app/ponto/portas/celula.py:969`; `app/ponto/portas/celula.py:1047`. |
| **Materialização** | Pergunta → fato e fechamento | `materializar_perguntas_validadas_da_disputa` seleciona respondidas, validadas e não materializadas; interpreta hora/data-hora/escolha, usa destino e fecha chamados/clones. Divergência de data bloqueia até conferência. `app/chamados/services/materializacao.py:1313`; `app/chamados/services/materializacao.py:1344`; `app/chamados/services/materializacao.py:339`; `app/chamados/services/materializacao.py:1156`. |
| **Materialização** | Cache do julgamento | `veredito_lavrado`, `julgado_em`, `janela_expira_em` são escritos por `lavrar`; existe também comando de backfill declarado como escritor. `app/chamados/veredito_lavrado.py:211`; `app/core/tests/test_contract_escritores_estado.py:63`. |
| **Consulta** | Pergunta, identidade e disputa | `do_fato`; C6 `pergunta_viva`; `.estado` e `.viva`; `q_vivas` como pré-filtro generoso; `DisputaSupervisao.pode_fechar`. O SQL de `q_vivas` não substitui C6. `app/chamados/models.py:2381`; `app/chamados/juizes.py:822`; `app/chamados/models.py:2492`; `app/chamados/models.py:2770`; `app/chamados/models.py:2798`; `app/chamados/models.py:1767`. |
| **Consulta** | Cobrança | `aviso_pendencia` seleciona C6 vivas sem resposta; `cobranca` lê dias, disputas e prazo do pai; fábrica lê célula/ata, situação, teto e histórico; reconciliação lê premissa, célula e lastro. `app/chamados/services/aviso_pendencia.py:7`; `app/chamados/services/cobranca.py:45`; `app/chamados/services/cobranca.py:75`; `app/chamados/services/fabrica_da_celula.py:74`; `app/chamados/reconciliador.py:1353`. |
| **Consulta** | Marcos/cobertura da célula | Leitores incluem `_marcos_faltantes`, `_coberto_por_ausencia`, `pergunta_viva`, cartório e vigia. Leem `trabalha`, data, ata/tipo do dia, cobertura/período parcial, lâmpadas, veredito/via e carimbos. `app/chamados/services/declaracao_texto.py:65`; `app/chamados/services/materializacao.py:55`; `app/chamados/juizes.py:1135`; `app/ponto/services/cartorio.py:1413`; `app/chamados/services/vigia_fio.py:378`. |
| **Tela admin** | Pergunta/destino/disputa | Fio ancora no destino; inclui perguntas C6 vivas em disputa fechada. Modal mostra dias divergentes, conferência, resposta e carimbos. Relatório de disputas restringe-se às abertas. `app/chamados/services/fio.py:97`; `app/chamados/services/fio.py:209`; `app/templates/chamados/partials/modal_fio.html:156`; `app/templates/chamados/partials/modal_fio.html:212`; `app/chamados/relatorios_disputa.py:11`. |
| **Tela app** | Pergunta/destino/estado | API usa `perguntas_do_app`, sem filtro de disputa aberta; publica estado, espera pelo admin e destino. POST exige C6 viva e chama a porta de resposta. `app/chamados/services/pergunta_no_app.py:42`; `app/api/views.py:2551`; `app/api/views.py:2568`; `app/api/views.py:2627`. |
| **Tela PWA/badge** | Pergunta/disputa | Fio do colaborador com `so_janela=True` segue pelas disputas abertas; badge também exige disputa aberta. Card apresenta data/resposta e condiciona formulário por `.viva`. `app/chamados/services/fio.py:209`; `app/chamados/models.py:1729`; `app/templates/chamados/partials/_card_atendimento.html:77`. |

**Os significados não são uniformes em todos os leitores:**

| Campo/conceito | Unidade e divergência |
|---|---|
| `PerguntaDisputa.estado` | É derivado. Pode devolver pendente/respondida mesmo quando C6 considera a pergunta morta sem carimbo. Portanto, “pendente” não prova exigibilidade. `app/chamados/models.py:2492`; `app/chamados/models.py:2502`; `app/chamados/models.py:2802`. |
| `data_evento` | É `DateField`, mas pode nascer do contexto/grade, da **data civil local da batida** ou do **dia civil do nascimento da conversa**. Não é universalmente turno canônico. `app/chamados/models.py:2218`; `app/chamados/services/disputa_emissao.py:319`; `app/chamados/services/disputa_emissao.py:350`; `app/chamados/models.py:2422`. |
| `chamado_destino` | É resolvedor e âncora de apresentação; o prazo da cobrança fica no **pai da disputa**. Destino não equivale a responsável nem a portador do SLA. `app/chamados/services/fio.py:97`; `app/chamados/services/cobranca.py:51`. |
| Célula | `trabalha` pertence ao previsto; ata/veredito ao julgamento; pergunta cobra um marco. A acusação do dia não identifica automaticamente o motivo exigível. `CLAUDE.md:167`; `app/chamados/juizes.py:1173`. |
| Carimbos | `respondida_em`, `validada_em`, `materializada_em` são instantes; `data_evento` é dia. O contrato exige timestamps em UTC e apresentação local. `app/chamados/models.py:2218`; `app/chamados/models.py:2228`; `app/chamados/models.py:2827`; `CLAUDE.md:143`. |
| Abertura da disputa | Significa `fechada_em is None`; não determina sozinha a vida da pergunta. API/admin podem ler viva em disputa fechada, enquanto badge/PWA restringem as abertas. `app/chamados/models.py:1763`; `app/chamados/services/pergunta_no_app.py:46`; `app/chamados/services/fio.py:209`; `app/chamados/models.py:1736`. |

O `vigia_fio` também tem fronteiras próprias:

- **Consulta de domínio:** `censo` parte de chamados `VIVOS` com célula; `censo_alcance` parte de dias que seguram a folha, por empresas visíveis e competências a lavrar. `dono_fora_do_fio` compartilha a composição dos baldes legais. `app/chamados/services/vigia_fio.py:334`; `app/chamados/services/vigia_fio.py:542`; `app/chamados/services/vigia_fio.py:577`; `app/chamados/services/vigia_fio.py:274`.
- **Materialização de medição:** o command escreve carimbo de frota com censo/alcance/passivos; recorte por empresa não substitui esse carimbo. Isso é escrita de instrumento, não transição de obrigação. `app/chamados/management/commands/vigiar_fio_morto.py:115`; `app/chamados/management/commands/vigiar_fio_morto.py:210`.
- **Execução/leitura:** cron chama o command; leitor host consome o carimbo e recusa universo vazio como verde. `app/config/crons.py:1703`; `bin/fio_estado.py:65`; `bin/fio_estado.py:74`.

**(3) Onde o comportamento atual difere do v3**

| Ponto | Conferência |
|---|---|
| **A1.2 em qualquer estado** | **Confirmado.** `do_fato` filtra somente colaborador, motivo e data; `abrir` pega a última linha e a devolve. Pode trocar célula/destino e relavrar, mas o gate não limpa resposta/carimbos nem reabre por esse reaproveitamento. `app/chamados/models.py:2392`; `app/chamados/models.py:2431`; `app/chamados/models.py:2465`. |
| **Identidade + episódio** | A identidade atual não contém episódio. “Episódio” já designa agrupamento pela mesma batida-origem e clones relacionados, outro significado. `app/chamados/models.py:2400`; `app/chamados/services/disputa_emissao.py:1018`; `app/chamados/models.py:2838`. |
| **Correção do dia re-chaveia a mesma ocorrência** | A porta de costura só preenche data antes vazia. Data diferente entregue ao nascimento procura outra chave. **Não achei** protocolo de re-chaveamento/colisão nesses dois caminhos. `app/chamados/services/fio.py:831`; `app/chamados/models.py:2431`. |
| **Extinção/encerramento** | Hoje há encerrados reabríveis, vias de resolução reversíveis e descarimbo. Não existe equivalência geral entre “resolvida”, “materializada”, “disputa fechada” e extinção do v3. `app/chamados/catalogo/motor.py:30`; `app/chamados/models.py:1323`; `app/chamados/models.py:2723`. |
| **Quem ressuscita** | `reabrir` chama `ressuscitar_perguntas`; cartório também a chama. A porta testa marco e C6 sobre cópia no pós-estado. Existem ainda inversas por fantasma, resposta e competência, além de relavra sem lastro. `app/chamados/models.py:1402`; `app/ponto/services/cartorio.py:1622`; `app/chamados/models.py:1420`; `app/chamados/models.py:2723`; `app/chamados/services/relavra_via.py:217`. |
| **Guarda contra renascimento** | Já existe proteção para fechamento administrativo sem fato novo, premissa regularizada, agrupamento e irmão vivo. Ela reutiliza a linha atual; não cria episódio pós-extinção. `app/chamados/models.py:1296`; `app/chamados/models.py:1309`; `app/chamados/models.py:1314`; `app/chamados/models.py:1390`. |
| **Suspensa sem cobrança** | Silêncio atual é véu, sem mudar status/apuração. Fila admin exclui silêncio futuro; seleção do app não consulta esse véu. `recobrar_questionario` conta perguntas cruas sem resposta e envia push sem guarda de silêncio nesse corpo. Ocorrência real de recobrança durante suspensão: **[não verificado]**. `app/chamados/services/arquivo.py:10`; `app/chamados/services/painel.py:186`; `app/chamados/services/pergunta_no_app.py:42`; `app/chamados/services/disputa_emissao.py:1485`. |
| **Responsável inalcançável** | Há juiz de canal de push, mas isso não prova acesso humano. A cobrança abre prazo mesmo se entrega falhar e orienta o admin a alcançar pelo supervisor; outro emissor ignora os retornos dos drivers. **Não achei** conversão automática para `incident` nesses caminhos. `app/core/canal.py:19`; `app/chamados/services/cobranca.py:173`; `app/chamados/services/cobranca.py:206`; `app/chamados/services/disputa_emissao.py:1540`. |
| **Exatamente um encaminhamento** | A autoridade de alcance admite várias portas/verbos simultâneos e consolida um `fio` por precedência. Unicidade de responsabilidade consolidada e unicidade de porta são propriedades diferentes. `app/chamados/juizes.py:1323`; `app/chamados/juizes.py:1350`; `app/chamados/juizes.py:1386`. |
| **Único módulo de transições** | Hoje há portas distintas para chamado, carimbo, resposta, validação, silêncio e costura. Isso difere de um PM único, mas não prova escrita descontrolada: vários campos já têm escritor canônico. `app/chamados/tests/test_selo_status_so_pelas_portas.py:36`; `app/core/tests/test_contract_escritores_estado.py:35`; `app/core/tests/test_contract_escritores_estado.py:83`. |

O outbox já cobre lavra da célula e alterações relevantes da pergunta, incluindo decisão administrativa pelos campos o

[truncado em 20000 de 29988 caracteres]

## 06/11 21:26 · sessao d5f797084ef3 · - · - · origem=Ronald (tmux)

Ampliação fechada: grupo **trocar vínculo de escala** passa a ter 3 telas e 3 pares: manual–lista, manual–modal e lista–modal; todas convergem em `julgar_vinculo` / `executar_vinculo` (`app/colaboradores/views.py:411`, `app/colaboradores/views.py:423`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:321`).

Tela manual: GET `/colaboradores/vincular/<cid>/painel/` → `painel_vinculo`, POST `/colaboradores/<cid>/vincular/escala/` → `vincular_escala`; formulário `colaboradores/partials/painel_vinculo.html` (`app/colaboradores/urls.py:12`, `app/colaboradores/urls.py:13`, `app/colaboradores/views.py:262`, `app/colaboradores/views.py:373`, `app/templates/colaboradores/partials/painel_vinculo.html:136`).

Os dois novos pares diferem porque manual recebe template, início e âncora humanos; as duas telas sugeridas fixam hoje e recusam ciclo antes de chamar o juiz. Manual permite retroagir até a janela anterior e criar ciclo com âncora declarada (`app/templates/colaboradores/partials/painel_vinculo.html:155`, `app/templates/colaboradores/partials/painel_vinculo.html:159`, `app/templates/colaboradores/partials/painel_vinculo.html:167`, `app/escala/regua_defesa.py:310`, `app/escala/regua_defesa.py:313`, `app/colaboradores/services/vinculo.py:69`, `app/colaboradores/services/vinculo.py:126`).

**Muda dia/dinheiro? Sim, potencial:** o início escolhido determina o trecho regenerado até hoje; regeneração padrão recalcula por evento as competências tocadas. Ocorrência efetiva em produção [não verificado] (`app/colaboradores/services/vinculo.py:320`, `app/ponto/portas/celula.py:110`, `app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:445`, `app/ponto/portas/celula.py:458`). Leitor do dia: `eh_dia_trabalho` retorna `CelulaDia.trabalha` (`app/escala/models.py:1094`).

Copiar tipo GET versus criar pelo wizard **não recomendo contar como par**: os comandos de tela são explicitamente “Copiar esta escala” e “Nova escala”, e não há segunda tela comprovada para o mesmo comando copiar (`app/templates/escala/tipos_lista.html:90`, `app/escala/views.py:108`). A cópia parcial existe: `copiar_tipo` cria com subconjunto explícito e não inclui calendário padrão, override por dia, HE fixa e duração do intervalo; isso é diferença entre comandos distintos (`app/escala/services/cadastro_tipo.py:367`, `app/escala/models.py:89`, `app/escala/models.py:203`, `app/escala/models.py:78`, `app/escala/models.py:110`).

## 06/11 21:26 · sessao 2f360630892b · - · - · origem=Ronald (tmux)

Confirmado: **não contar “recalcular” × “gerar TXT” como par da mesma ação**. Recalcular persiste o fechamento (`app/ponto/views.py:1252`); gerar TXT solicita `somente_leitura=True` (`app/folha/views.py:208`), cujo ramo entrega objetos sem salvar (`app/ponto/services/fechamento.py:1121`), depois aprova e registra exportação (`app/folha/views.py:252`, `:297`).

Correção ao inventário de telas: aprovação **selecionada** está acessível pelo JavaScript em `app/templates/ponto/fechamento.html:143`. Aprovação **cega**, recalcular antigo e reabrir competência estão dentro do bloco `display:none` aberto em `:152`, com formulários em `:209`, `:189`, `:223`, fechado em `:234`. O recalcular visível está em `:87`. Portanto, apresentar o cego como **endpoint existente com formulário oculto**, sem presumir botão acessível.

Reabrir individual e reavaliar vazios provêm da **mesma tela de fechamento**: `app/templates/ponto/_fech_linha.html:42` e `app/templates/ponto/fechamento.html:76`. A linha é incluída tanto na página (`:262`) quanto no fragmento de paginação (`app/templates/ponto/_fech_lote.html:8`), sem segunda experiência de tela independente comprovada.

Leitor relevante da diferença de carimbo: `competencia_fechada` consulta fechamento da pessoa e recusa `aprovado/exportado` (`app/ponto/janelas.py:86`, `:87`). O registro institucional de batida recusa antes de criar (`app/ponto/registro_batida.py:132`, `:137`); flip recusa antes de alterar (`app/ponto/services/flip_batida.py:21`, `:24`). Isso altera a possibilidade de regularizar posteriormente; mudança efetiva de dinheiro/dia por tentativa posterior é **[não verificado]**. O ato de aprovação isolado só grava carimbo/observação (`app/ponto/services/fechamento.py:1322`, `:1330`).

## 06/11 21:25 · sessao 4ab21ee7a095 · BUG-PLANTIO-NOTURNO-MAIS-1 · censo · origem=Code

**Não atribuo descarte à triagem sem evidência da decisão.** A regra exige registrar o motivo do descarte; “foi visto” e “foi descartado” precisam ser demonstrados separadamente. (`CLAUDE.md:368`, `CLAUDE.md:377`)

Os trechos abaixo são transcrições dos anexos fornecidos; sua numeração física em disco é [nao verificado].

1. **(a) — falha do Codex no núcleo “troca de significado sem backfill”, com percepção parcial do passivo.**

   **Pergunta exata — v1.q, item 6:**
   > pouso antes do pre-requisito; campo com dois significados. [nao verificado]

   **Pergunta exata — f1.q:**
   > quantas perguntas VIVAS (aberta/respondida) com D_e_dia_da_saida -- essas sao as que mudam de leitura quando o leitor migrar

   **Resposta exata — v1.r, item 6:**
   > Suite, pouso e saneamento do passivo: **[nao verificado]**. A costura não converte datas já preenchidas, pois só data perguntas sem dia. [nao verificado]

   **Resposta exata — f1.r:**
   > **(d) VIVAS em `D_e_dia_da_saida`: 81**

   O tema estava perguntado, mas a resposta não denunciou a incompatibilidade entre migrar os leitores e conservar as datas antigas. A costura realmente só preenche perguntas sem data. (`app/chamados/services/fio.py:832`)

   **Triagem:** existe a decisão “passivo so depois da cura no ar”, mas ela não prova descarte de um alerta explícito do Codex sobre essa incompatibilidade. (`app/docs/RELATO.md:41`) Os recortes “sem marcos” e “plantio em D−1” não foram medidos pela saída fornecida. [nao verificado]

2. **(a) — falha do Codex: reconheceu a retirada do recuo, mas não a regressão sobre perguntas antigas.**

   **Pergunta exata — v1.q, item 1:**
   > Ha algum par escritor/leitor em que os significados divergem (ESMERIL-SEMANTICA: unidade, fuso, dia do turno x dia civil x instante)?

   **Resposta exata — v1.r, item 6:**
   > Há asserts que atingem o defeito principal: escritor retorna D, saída planta em D+1, detector grava D e sem marcos não recua.

   A resposta tratou “sem marcos não recua” como comportamento correto, sem separar perguntas novas das antigas, cujo D ainda significava dia da saída. Na cópia, o ramo antigo recua a entrada quando ela ultrapassa a hora da saída. (`app/chamados/juizes.py:315`, `app/chamados/juizes.py:320`)

   **Triagem específica desse risco:** [nao verificado]. As 21 vivas são um teto do recorte SSE fornecido; quantas efetivamente estão sem marcos permanece [nao verificado].

3. **(a) — falha do Codex: inventariou os leitores, mas perdeu o efeito da retirada de D−1 sobre plantios existentes.**

   **Pergunta exata — v1.q, item 1:**
   > Para cada leitor: com que significado ele le (dia do turno, dia civil da batida, dia da saida, dia em que a pergunta foi feita).

   **Resposta exata — v1.r, inventário:**
   > `_plantio_na_hora_do_slot` usa D/D+1

   > `_pergunta_materializada_de_fato`, `_pergunta_viva_calc`

   A resposta mencionou os componentes, mas não apontou a cadeia de perda de reconhecimento → pergunta viva → descarimbo. A cópia mostra a busca antiga incluindo D−1, seu uso como prova de materialização e o descarimbo condicionado a `pergunta_viva`. (`app/chamados/juizes.py:265`, `app/chamados/juizes.py:724`, `app/chamados/services/relavra_via.py:227`, `app/chamados/services/relavra_via.py:231`)

   A fábrica chama esse descarimbo durante a emissão pelo dia. (`app/chamados/services/fabrica_da_celula.py:136`) **Triagem específica:** [nao verificado].

4. **(c) — RED histórico fora do alcance demonstrado; não há certificação do defeito no main.**

   **Pergunta exata — v3.q, item 4:**
   > O RED test_MORDE_saida_sem_dia_do_turno_nao_vira_500 morde o pai (39324b76)?

   **Resposta exata — v3.r, item 4:**
   > **Há mordida dedutiva contra o pai descrito no anexo; execução contra `39324b76`: [nao verificado].**

   **Resposta exata — v4.r, item 3:**
   > objeto e execução contra `39324b76`: **[nao verificado]**.

   O alvo explícito dessas perguntas era o pai intermediário. A resposta não afirmou execução nem prova contra o main original. O relato registra a correção dos “500 nos gemeos” como procedente, mas não demonstra sua existência no main. (`app/docs/RELATO.md:11`) A atribuição histórica aos commits indicados pelo opus permanece [nao verificado].

5. **VISTO; (b) comprovado apenas parcialmente, sem fechar a imputação do núcleo inteiro.**

   **Pergunta exata — v2.q, item 1:**
   > agora deixa de nascer pergunta e some sem rastro alem do log?

   **Pergunta exata — v2.q, item 2:**
   > Ha outro emissor (varredura, cron) que a pega depois, ou ela fica invisivel?

   **Resposta exata — v2.r, item 1:**
   > A recusa específica, entretanto, fica apenas no `logger.info`, sem pergunta ou pendência própria.

   **Resposta exata — v2.r, item 2:**
   > A guarda então pula a disputa e a triagem pode chegar à indicação de sucesso, sem registrar pendência própria nesse ramo.

   Portanto, o silêncio foi explicitamente visto. [nao verificado quanto às linhas físicas dos anexos]

   **Triagem localizada:** o efeito relacionado “push sem pergunta” foi colocado em fila. (`app/docs/BACKLOG.md:1798`) A linha que descarta especificamente a ausência de pendência, prazo, tripwire ou contador é [nao verificado]. Não há base para chamar esse núcleo de falha do Codex nem para fechar integralmente **(b)**.

6. **(a) parcial — o lote `dias_do_fato` não foi apontado; a derivação civil singular foi parcialmente vista.**

   **Pergunta exata — v1.q, item 7:**
   > Ha fallback disfarcado ("se nao achar, usa o dia do timestamp", `or data_entrada`, default de dia) restante em algum escritor tocado ou vizinho?

   **Resposta exata — v1.r, item 7:**
   > O derivador aceita o dia do chamado; esse juiz tem degraus de dia civil da batida e timestamp cru do contexto.

   A resposta reconheceu a família “dia civil da batida”, mas não identificou a duplicação no lote. A cópia confirma ambas as derivações por `localdate(timestamp)`. (`app/chamados/juizes.py:50`, `app/chamados/juizes.py:193`)

   **Triagem exata — v2.q:**
   > FILA: _dia_do_contexto_fraco (juizes.py)

   Essa linha trata do contexto fraco; não demonstra descarte específico de `_dia_da_batida` e `dias_do_fato`. Esse descarte é [nao verificado].

7. **Núcleo VISTO; (b) não comprovado. (a) parcial quanto ao comentário antigo.**

   **Pergunta exata — v1.q, item 6:**
   > derivacao paralela de fonte unica (ex.: dia do turno derivado sem _T1); selo/RED sem mordida

   **Resposta exata — v1.r, item 6:**
   > o construtor de célula apenas devolve a data recebida.

   > `NewType` não estabelece uma validação semântica nessa porta.

   > Os testes novos verificam ausência/`None`, não proveniência do dia. [nao verificado]

   O núcleo do achado — cast sem validação de significado e selo insuficiente para provar proveniência — já estava explicitamente na resposta. [nao verificado quanto às linhas físicas dos anexos]

   **Triagem exata — v2.q:**
   > mypy so checa chamador anotado, selo nao segue **dict/setattr por variavel/SQL cru

   Essa fila cobre limitações relacionadas, mas não comprova descarte do alerta semântico específico. **(b) integral:** [nao verificado]. O comentário que ainda promete “data correta da SAIDA” permanece na cópia e não foi denunciado especificamente nas respostas. (`app/escala/utils.py:913`)

Regras preventivas propostas para as classes identificadas:

- **(a):** Toda troca semântica deve percorrer uma pergunta antiga até seus leitores e efeitos de escrita, incluindo reconhecimento e descarimbo. [nao verificado]
- **(b), parcial:** Cada alerta deve receber triagem própria com decisão, motivo e cura ou adiamento identificável; uma fila vizinha não substitui essa linha. (`CLAUDE.md:368`)
- **(c):** Toda alegação de RED histórico deve identificar o hash original e evidenciar falha pelo motivo correto nesse hash. [nao verificado]

**Não atribuo (d):** a regra local reserva MODO-COMUM ao que Codex **e** opus deixaram passar; os sete achados foram apresentados pelo opus. (`CLAUDE.md:385`; anexo `opus_final.txt` [nao verificado quanto às linhas físicas])

## 06/11 21:24 · sessao 53ecda1d422f · - · - · origem=Ronald (tmux)

Complemento folga manual, com correção do disparador:

- **Plano → aplicar lote:** `/escala/plano-folgas/` → `/escala/plano-folgas/aplicar/`, `app/escala/urls.py:8`, `:10`; view `app/escala/views_plano_folgas.py:144` chama `aplicar_folgas_em_lote` em `:185`; template dispara POST em `app/templates/escala/plano_folgas.html:171`; escritor `app/escala/folgas.py:362` → `_foto_do_dia` cria em `:337`, altera em `:342` ou apaga em `:333`.
- **Detalhe → aceitar `FOLGA_CONTESTA`:** `/chamados/local/<id>/`, `app/chamados/urls.py:14`; view `app/chamados/views.py:359`, ramo `:483`, chamada `:501`; template `app/templates/chamados/detalhe_local.html:310` inclui `app/templates/chamados/partials/_pergunta_card.html:92`, POST com ação em `:95`.
- **Modal → aceitar:** `/chamados/modal/<colaborador>/`, `app/chamados/urls.py:39`; view `app/chamados/views.py:869` delega em `:954`; template `app/templates/chamados/partials/modal_fio.html:274`, ação `:276`.
- **Inline → aceitar:** `/chamados/validar-inline/<cid>/`, `app/chamados/urls.py:33`; view `app/chamados/views_cobrar.py:145` chama em `:208`; botão `app/templates/chamados/partials/_lista_chamados.html:82`.

Os três aceites convergem em `app/chamados/services/acoes_disputa.py:19` → `validar_pergunta` reconhece folga em `app/chamados/services/validacao.py:382` → `_aceitar_a_folga` chama `declarar_dia` em `:291` → `app/ponto/services/declarar_dia.py:189` → escritor `lancar_folga`, `app/escala/folgas.py:280`, `:312`.

**Um grupo Plano × aceites, três pares atômicos divergentes:**

1. Plano recusa qualquer dia anterior ao início da competência atual: `app/escala/folgas.py:379`, `:385`. Aceite permite dia com até 120 dias de antiguidade: constante `app/ponto/services/declarar_dia.py:53`, verificação `:101`.
2. Plano aceita tipo escolhido `F1…F9/FD`, `app/escala/views_plano_folgas.py:173`; aceite escreve `FD`, `app/ponto/services/declarar_dia.py:190`.
3. Plano só pede relavra em `app/escala/folgas.py:395`; aceite também fecha perguntas vivas e tenta reconciliar chamados: `app/ponto/services/declarar_dia.py:204` → `app/ponto/services/veredito_celula.py:174`, `:177`, `:182`, `:183`.
4. Aceite carimba pergunta, fecha sua via administrativa e registra `val_perg_folga`: `app/chamados/services/validacao.py:290`, `:299`, `:300`; Plano não passa por esse escritor, conforme cadeia `app/escala/views_plano_folgas.py:185` → `app/escala/folgas.py:362`.

**Leitor e gravidade:** `app/ponto/precedencia.py:356` lê a folga e `:384` a transforma em fato `folga`. **Muda dia: sim, potencialmente**, sobretudo para dia anterior à competência atual que o Plano recusa e o aceite pode gravar; ocorrência e dinheiro efetivo em produção [não verificado]. Ambas as rotas relavram por `app/escala/folgas.py:272` e preservam a guarda de competência entregue em `app/ponto/portas/celula.py:214`.

**Convergência dos aceites com validação divergente:** detalhe só admite disputa aberta (`app/chamados/views.py:495`); inline também procura aberta (`app/chamados/views_cobrar.py:160`). Modal admite pergunta viva em disputa fechada (`app/chamados/services/fio.py:596`, `:601`, `:605`) e acrescenta trilha em `:611`. Portanto detalhe × modal e inline × modal são **mais dois pares divergentes**, embora compartilhem escritor; detalhe × inline converge no mesmo escritor, sem divergência adicional comprovada para folga.

**Fronteira varrida:** quatro views aptas ao aceite — detalhe, fio, modal e inline, nas referências acima e `app/chamados/views.py:659`, `:674`. O template do fio completo só tem formulário de mensagem/status (`app/templates/chamados/fio_colaborador.html:257`); disparador visual `validar_pergunta` nessa tela [não verificado], então não o contei nos três pares Plano × telas.

`validar_lote` e `validar_lote_tela` **não escrevem folga**: `FOLGA_CONTESTA` vira classe C por substring `folga`, `app/chamados/services/validacao.py:755`, `:806`, `:808`; lote recusa qualquer classe diferente de A em `:688`. Toggle tem view escritora em `app/escala/views_plano_folgas.py:98`, mas disparador em template [não verificado].

## 06/11 21:24 · sessao f30767be6376 · - · - · origem=Ronald (tmux)

Fechamento cruzado: **há duas representações de férias que as telas podem alterar separadamente**; não encontrei sincronização inversa `Ausencia → AgendamentoFerias` nos escritores/modelo/signals examinados. Evidências: `app/ponto/models.py:868` até o `super().save()` em :908; receivers de Ausencia em `app/ponto/signals.py:143`, :150, :275 e :282; consumidor limita-se a perguntas/chamados em signals:250. Sincronização existente tem sentido `AgendamentoFerias → Ausencia`, `app/ferias/models.py:434` → `_sync_ausencia`, models:379 → `app/ponto/services/ausencia.py:1160`.

| Ação cruzada | Escritores/efeitos diferentes | Leitor e gravidade |
|---|---|---|
| Conceder férias via `/ponto/ausencias/lancar/` × agendar férias | `lancar_ausencia`, `app/ponto/views.py:2368`, chama `criar_ausencia` com tipo recebido e status aprovada em :2415. Produz apenas Ausencia; montagem dos campos em `app/ponto/services/ausencia.py:341`. Já agendamento escreve Agendamento e `periodo.dias_concedidos`, `app/ferias/services.py:321`, e espelha Ausencia por model save, models:434. | Espelho vê cobertura aprovada, `app/ponto/services/espelho.py:588`; saldo depende de concedidos/vendidos, `app/ferias/models.py:216`; folha procura **Agendamento** em `app/folha/export.py:491`, retenção :868. **Sim, muda dia/saldo/retenção potencialmente**: conceder só Ausencia abona cobertura sem consumir saldo nem compor o mesmo universo de férias com batida. Ocorrência real **[não verificado]**. |
| Justificar dia com tipo férias × agendar férias | `/ponto/celula/veredito/`, `app/ponto/urls.py:58`, view `veredito_celula_view`, `app/ponto/views.py:3312`, passa tipo da tela em :3341. Escritor `veredito_celula`, `app/ponto/services/veredito_celula.py:191`, cria apenas Ausencia aprovada em :315 e ainda fecha fio em :325, lavra célula :330 e registra log :334. Não cria Agendamento/saldo; contraste com services férias:321. | Mesmos leitores/impacto acima; efeito adicional no fio/célula só nesse caminho, veredito_celula:324. |
| Editar Ausencia de férias × editar datas do Agendamento | `editar_ausencia`, chamada em `app/ponto/views.py:2794` → `editar_ausencia_registro`, `app/ponto/services/ausencia.py:465`, altera só tipo/range/minutos da Ausencia em :498, salva :509 e invalida fechamento :524. Editar Agendamento usa `_trocar_janela`, `app/ferias/services.py:366`, muda também Agendamento/concedidos e remove Ausencias antigas em :378. | **Sim, muda dia**: a cobertura pode sair da janela original sem mover a janela do Agendamento, preservando saldo e retenção calculada sobre o intervalo antigo; leitores espelho:588 e export:491. Existência desse estado em produção **[não verificado]**. |
| Cancelar Ausencia de férias × cancelar Agendamento | Cancelamento lote não exclui tipo férias: busca qualquer Ausencia não rejeitada, `app/ponto/views.py:2345`, decide rejeitada em :2351. `decidir`, `app/ponto/services/ausencia.py:897`, muda estado e salva campos da Ausencia, :911/:927. Cancelar Agendamento atualiza concedidos/status/obs, `app/ferias/services.py:352`, e save remove espelhos, `app/ponto/services/ausencia.py:1188`. | **Sim, muda dia**: cancelamento somente da Ausencia retira cobertura do espelho, mas Agendamento continua no universo `ferias_na_janela` enquanto não cancelado, `app/ferias/services.py:51`; folha ainda pode reter por férias com batida, export:868; saldo não recebe devolução. Ocorrência real **[não verificado]**. |

**Condição de disponibilidade do tipo:** catálogo congelado inclui `ferias`, `app/ponto/catalogo/ausencias.py:101`; `vigente()` usa banco ou catálogo congelado, :278; choices agrupadas incluem todos os códigos, :313; painel do colaborador exclui só `falta`, `app/colaboradores/views.py:1959`. Portanto os caminhos aceitam férias se o catálogo vigente a contiver; disponibilidade efetiva de férias no cadastro de produção **[não verificado]**. `Ausencia.exige_documento` declara exceção de férias no fluxo próprio, `app/ponto/models.py:727`, reforçada por `FORA_DO_A4 = ('ferias',)`, `app/ponto/services/ausencia.py:751`; não é veto de criação pelo admin.

**Venda histórica:** `criar_periodo_aquisitivo` lê `dias_vendidos`, `app/ferias/views.py:194`, e passa para `registrar_gozo_historico`, :210; escritor incrementa vendidos junto do gozo, `app/ferias/services.py:300`, enquanto venda separada usa `vender_dias_do_periodo`, services:507. Porém os templates examinados não oferecem campo de vendidos no formulário histórico: `app/templates/ferias/_secao_colab.html:6`–:33 e `criar_periodo.html:9`–:30. Assim o segundo **escritor** está comprovado, mas o segundo **disparo de tela de venda** continua **[não verificado]**; não contar como par de ação visual fechado sem achar formulário adicional.

## 06/11 21:23 · sessao d5f797084ef3 · - · - · origem=Ronald (tmux)

Escala fechada em leitura estática. Caminho base `/escala/`: `app/config/urls.py:27`.

**Universo:** 9 funções com escrita POST, em 11 rotas; 1 função POST adicional sem persistência (`wizard_preview`); 2 escritores disparados por GET (`copiar_tipo_escala`, `excluir_tipo_escala`). As duas rotas extras de POST são criar/editar calendário e criar/editar tipo; preview declara e implementa objeto em memória (`app/escala/urls.py:24`, `app/escala/urls.py:25`, `app/escala/urls.py:35`, `app/escala/urls.py:36`, `app/escala/views_wizard.py:226`, `app/templates/escala/tipos_lista.html:90`, `app/templates/escala/tipos_lista.html:92`).

Lista das 9 funções escritoras POST: `escala_remove_colab` (`app/escala/views.py:256`), `editar_folga_calendario` (`app/escala/views.py:316`), `aplicar_folga_calendario` (`app/escala/views.py:376`), `corroborar_regua_da_lista` (`app/escala/views.py:456`), `realocar_regua_da_lista` (`app/escala/views.py:463`), `wizard_salvar` (`app/escala/views_wizard.py:98`), `wizard_calendario_criar` (`app/escala/views_wizard.py:140`), `plano_folgas_toggle` (`app/escala/views_plano_folgas.py:98`) e `plano_folgas_aplicar` (`app/escala/views_plano_folgas.py:144`).

**Par divergente inteiramente dentro de escala:**

| Ação | Tela A → escritor | Tela B → escritor | Diferença / leitor / gravidade |
|---|---|---|---|
| Criar calendário de folgas | `/escala/folgas/criar/`, `editar_folga_calendario` (`app/escala/urls.py:35`, `app/escala/views.py:316`), formulário `escala/folga_calendario_editar.html` (`app/templates/escala/folga_calendario_editar.html:9`) → `salvar_calendario_de_folga` (`app/escala/views.py:356`, `app/escala/folgas.py:548`) | `/escala/tipos/wizard/calendario/criar/`, `wizard_calendario_criar` (`app/escala/urls.py:20`, `app/escala/views_wizard.py:140`), botão `escala/wizard_tipo_escala.html` (`app/templates/escala/wizard_tipo_escala.html:160`) → `criar_calendario_de_folga` (`app/escala/views_wizard.py:171`, `app/escala/views_wizard.py:192`, `app/escala/folgas.py:577`) | Escritores diferentes. Editor recebe dias independentes para 28/30/31; wizard replica uma lista aos três tamanhos com clamp (`app/escala/views.py:340`, `app/escala/folgas.py:570`, `app/escala/folgas.py:590`). Pares semana/dia malformados são descartados pelo editor, mas recusados com 400 pelo wizard (`app/escala/views.py:333`, `app/escala/views_wizard.py:163`). Ambos convergem apenas no veto de nome repetido (`app/escala/folgas.py:539`, `app/escala/folgas.py:554`, `app/escala/folgas.py:584`). Leitor: `datas_que_o_calendario_manda` → `gerar_foto_folgas` → `FolgaDia` e relavra (`app/escala/folgas.py:43`, `app/escala/folgas.py:150`, `app/escala/folgas.py:180`, `app/escala/folgas.py:203`). **Não muda dia/dinheiro imediatamente:** criar grava só calendário/tamanhos; há potencial de dias diferentes depois da materialização (`app/escala/folgas.py:548`, `app/escala/folgas.py:577`, `app/escala/folgas.py:150`). |

**Pares de fronteira comprovados; segunda tela em chamados:**

1. **Confirmar cadastro de escala errado.** Tela A `/escala/cadastro-x-realidade/` (`app/escala/views.py:476`, render `app/escala/views.py:492`), formulário em `app/templates/escala/cadastro_x_realidade.html:28`, POST `/escala/cadastro-x-realidade/regua/<ec_pk>/corroborar/` → `corroborar_regua_da_lista` → **converge em `corroborar_regua`** (`app/escala/urls.py:15`, `app/escala/views.py:456`, `app/escala/views.py:459`, `app/escala/regua_defesa.py:647`). Tela B modal `/chamados/modal/<colaborador_id>/` (`app/chamados/urls.py:39`, `app/chamados/views.py:869`, render `app/chamados/views.py:927`), template `chamados/partials/modal_fio.html`, formulário POST `/chamados/local/<chamado_id>/` com `acao=corroborar_regua` (`app/templates/chamados/partials/modal_fio.html:559`, `app/templates/chamados/partials/modal_fio.html:561`, `app/chamados/urls.py:14`, `app/chamados/views.py:359`, `app/chamados/views.py:438`). **Difere:** lista escolhe EC pela PK; modal escolhe vínculo ativo pelo colaborador e tipo do contexto; só modal passa `chamado` e anexa ocorrência (`app/escala/views.py:449`, `app/chamados/views.py:441`, `app/chamados/views.py:443`, `app/escala/regua_defesa.py:670`). **Não há diferença de dia/dinheiro comprovada para o mesmo EC:** campos são gravados na mesma chamada; diferença comprovada é ocorrência e seleção do alvo (`app/escala/regua_defesa.py:662`). Leitor de cobrança comum: `vetar_cobranca_se_regua_suspeita`, detector (`app/escala/regua_defesa.py:554`, `app/ponto/management/commands/detectar_ausencias.py:603`). Leitor da diferença: followups do chamado (`app/chamados/views.py:372`).

2. **Realocar escala para destino sugerido.** Mesmas telas A e B acima; A POST `/escala/cadastro-x-realidade/regua/<ec_pk>/realocar/` → `realocar_regua_da_lista` → `realocar_vinculo_para_destino` (`app/escala/urls.py:17`, `app/escala/views.py:463`, `app/escala/views.py:471`, formulário `app/templates/escala/cadastro_x_realidade.html:28`). B POST `/chamados/local/<chamado_id>/`, `acao=realocar_regua` → `realocar_para_destino` → **converge em `realocar_vinculo_para_destino`** → `julgar_vinculo` / `executar_vinculo` (`app/templates/chamados/partials/modal_fio.html:568`, `app/templates/chamados/partials/modal_fio.html:570`, `app/chamados/views.py:449`, `app/escala/regua_defesa.py:326`, `app/escala/regua_defesa.py:329`, `app/escala/regua_defesa.py:316`, `app/escala/regua_defesa.py:321`). **Difere:** lista usa destino da última lavra; modal usa destino persistido no contexto do chamado e só ele anexa ocorrência de realocação (`app/escala/views.py:469`, `app/escala/regua_defesa.py:328`, `app/escala/regua_defesa.py:333`). **Pode mudar dia/dinheiro se os destinos divergirem:** destinos efetivamente recebidos iguais em produção [não verificado]; o escritor cria vínculo desde hoje e regenera células (`app/escala/regua_defesa.py:313`, `app/colaboradores/services/vinculo.py:321`). Leitor de dia: `EscalaColaborador.eh_dia_trabalho` lê `CelulaDia.trabalha` (`app/escala/models.py:1069`, `app/escala/models.py:1094`).

3. **Gravar folga semanal no template.** A wizard editar tipo `/escala/tipos/<tid>/wizard/`, POST `/escala/tipos/<tid>/wizard/salvar/`, template `escala/wizard_tipo_escala.html` (`app/escala/urls.py:23`, `app/escala/urls.py:25`, `app/templates/escala/wizard_tipo_escala.html:15`, `app/templates/escala/wizard_tipo_escala.html:128`), view `wizard_salvar` → **converge em `salvar_tipo`** (`app/escala/views_wizard.py:98`, `app/escala/views_wizard.py:108`, `app/escala/services/cadastro_tipo.py:140`). B modal citado acima, `acao=aceitar_folga_limbo` → `detalhe_chamado_local` → `aceitar_sugestao` → mesmo `salvar_tipo` (`app/templates/chamados/partials/modal_fio.html:545`, `app/chamados/views.py:431`, `app/chamados/services/limbo_folgas.py:73`, `app/chamados/services/limbo_folgas.py:91`). **Difere:** B exige sugestão fixa do contexto, grava LogAuditoria e reconcilia/anexa nota nos chamados limbo do template; A recebe dias do formulário e não executa esse bloco (`app/chamados/services/limbo_folgas.py:81`, `app/chamados/services/limbo_folgas.py:94`, `app/chamados/services/limbo_folgas.py:98`, `app/escala/views_wizard.py:108`). **Para mesmos dias, diferença comprovada em dinheiro/dia: não; trilha e chamados: sim.** Escritor do campo comum `folga_dia_semana` e regeneração comuns (`app/escala/services/cadastro_tipo.py:259`, `app/escala/services/cadastro_tipo.py:320`). Eventual efeito financeiro da reconciliação extra [não verificado].

**Candidatos que não devem entrar na contagem de pares fechados:**

- Aplicar calendário coletivo vs individual: escritor coletivo e rota existem (`app/escala/urls.py:37`, `app/escala/views.py:376`), mas disparador em tela coletiva [não verificado]. Tela individual comprovada: POST `/colaboradores/<cid>/vincular/folga/`, view `vincular_folga`, template `colaboradores/partials/painel_vinculo.html` (`app/colaboradores/urls.py:17`, `app/colaboradores/views.py:212`, `app/templates/colaboradores/partials/painel_vinculo.html:191`). Ambos convergem em `trocar_calendario_de_folga` e `gerar_foto_folgas` (`app/escala/views.py:409`, `app/escala/views.py:411`, `app/colaboradores/views.py:239`, `app/colaboradores/views.py:245`). Só individual registra `HistoricoVinculo`; coletivo faz relavra externa extra com autor e retorna barrados (`app/colaboradores/views.py:241`, `app/escala/views.py:416`, `app/escala/views.py:424`). **Não afirmar que individual deixa célula velha:** gerar foto relavra internamente (`app/escala/folgas.py:203`). Leitor do histórico: `app/colaboradores/views.py:306`, `app/templates/colaboradores/partials/painel_vinculo.html:53`.
- Toggle vs lote: endpoints diferentes, mas só lote tem disparador encontrado na mesma tela `plano_folgas.html`; segunda tela do toggle [não verificado] (`app/escala/urls.py:9`, `app/escala/urls.py:10`, `app/templates/escala/plano_folgas.html:171`). Diferença real do código: lote recusa datas anteriores à competência corrente; caminhos individuais não têm essa guarda antes da foto (`app/escala/folgas.py:379`, `app/escala/folgas.py:385`, `app/escala/folgas.py:309`, `app/escala/folgas.py:354`).
- Django admin de TipoEscala: registrado como `admin.ModelAdmin` puro; EscalaColaborador está protegido por `SoLeitura` (`app/escala/admin.py:7`, `app/escala/admin.py:8`, `app/escala/admin.py:13`). Rota exata, template e implementação da view/escritor herdado não constam da cópia examinada [não verificado]. Não contar como par escritor fechado.

## 06/11 21:23 · sessao 1cdcaaaf806b · - · - · origem=Ronald (tmux)

Censo `chamados`: **22 views POST com ramo administrativo**, incluindo quatro de regularização externa; **27 verbos semânticos**, ou **25 famílias** juntando assumir/liberar e autorizar geofence com/sem advertência. Inventário abaixo; a contagem inclui escritores HTTP existentes mesmo quando o formulário atual não foi encontrado. (`app/chamados/urls.py:10`, `app/chamados/urls.py:25`, `app/chamados/urls.py:48`)

Views:

- `views.py`: `abrir_chamado:147`, `adicionar_followup:274`, `detalhe_chamado_local:359`, `fio_colaborador:659`, `modal_fio_colaborador:869`, `abrir_chamado_rapido:961`, `atribuir_chamado:1008`, `conferir_data_pergunta:1292`, `reabrir_disputa_supervisao:1372`.
- `views_cobrar.py`: `cobrar_chamado:24`, `cobrar_massa:61`, `cobrar_dia_view:306`, `validar_inline:145`, `validar_lote:232`, `validar_lote_tela:372`.
- `views_arquivar.py`: `arquivar_chamado:24`, `desarquivar_chamado:52`, `arquivar_lote:93`.
- `views_regularizacao.py`: `regularizacao_criar:42`, `regularizacao_add_item:74`, `regularizacao_del_item:137`, `regularizacao_enviar:154`.

**Pares divergentes confirmados**

1. **Validar pergunta: detalhe × modal.** Detalhe `/chamados/local/<id>/`, view `app/chamados/views.py:359`, formulário `app/templates/chamados/partials/_pergunta_card.html:92`, restringe a disputa aberta (`app/chamados/views.py:493`). Modal `/chamados/modal/<colab>/`, view `app/chamados/views.py:869`, formulário `app/templates/chamados/partials/modal_fio.html:274`, aceita pergunta viva em disputa fechada e passa `sem_reabrir=True` (`app/chamados/services/fio.py:591`, `app/chamados/services/fio.py:605`). **Convergem no escritor** `app/chamados/services/validacao.py:343`, mas o modal possui gate diferente e log adicional `validar_viva_em_disputa_fechada` (`app/chamados/services/fio.py:611`). **Pode mudar dia/dinheiro:** a validação materializa e recalcula o fechamento (`app/chamados/services/validacao.py:406`, `app/chamados/services/validacao.py:423`).

2. **Regularizar/fechar disputa: detalhe × modal.** Detalhe: formulário `app/templates/chamados/detalhe_local.html:315` → `app/chamados/views.py:470` → `app/chamados/services/acoes_disputa.py:118`. Modal: formulário `app/templates/chamados/partials/modal_fio.html:348` → `app/chamados/services/fio.py:642`. Detalhe procura somente disputa cujo **container é o chamado**; modal usa `_disputa_do_chamado`, que também encontra disputa pela pergunta cujo destino é o chamado (`app/chamados/services/fio.py:564`, `app/chamados/services/fio.py:134`). **Convergem no materializador**, chamado em `app/chamados/services/acoes_disputa.py:139` e `app/chamados/services/fio.py:666`; os blocos de orquestração permanecem duplicados. **Pode mudar dia/dinheiro**, pois esse escritor materializa respostas e fecha a disputa; a diferença de alcance pode impedir o detalhe de executar o que o modal executa (`app/chamados/services/acoes_disputa.py:139`).

3. **Responder/mudar status/resolver chamado: detalhe/painel de turno × fio/modal.** Detalhe e painel de turno postam `/chamados/local/<id>/` (`app/templates/chamados/detalhe_local.html:385`, `app/templates/chamados/partials/painel_chamado_turno.html:68`) → `app/chamados/services/acoes_chamado.py:283`. Fio `/chamados/colaborador/<id>/` e modal usam `processar_post_fio` (`app/chamados/views.py:674`, `app/chamados/views.py:954`) → `app/chamados/services/fio.py:704`; formulários em `app/templates/chamados/fio_colaborador.html:257` e `app/templates/chamados/partials/modal_fio.html:660`. Detalhe anexa nota `resposta_livre`, logs adicionais `responder` e `status`; fio/modal anexam `mensagem_dp` e não acrescentam esses dois logs (`app/chamados/services/acoes_chamado.py:292`, `app/chamados/services/acoes_chamado.py:295`, `app/chamados/services/acoes_chamado.py:318`, `app/chamados/services/fio.py:714`). **Ambos têm auditoria canônica do estado**, via `transicionar` → `_gravar_estado` → `evento('estado_chamado')` (`app/chamados/models.py:122`, `app/chamados/models.py:274`). Gate HTTP: detalhe aceita mudar status sem conteúdo; fio exige conteúdo (`app/chamados/services/acoes_chamado.py:313`, `app/chamados/services/fio.py:705`), embora todos os formulários tornem o texto obrigatório (`app/templates/chamados/detalhe_local.html:387`, `app/templates/chamados/fio_colaborador.html:261`). **Diferença visível adicional:** detalhe oferece reabrir como `aberto`; fio/modal não (`app/templates/chamados/detalhe_local.html:398`, `app/templates/chamados/fio_colaborador.html:267`, `app/templates/chamados/partials/modal_fio.html:662`). Leitores de estado/perguntas recebem o mesmo mecanismo comum: `app/chamados/signals.py:76`, `app/chamados/services/disputa_emissao.py:1555`. Impacto financeiro específico da diferença de logs: **não**; diferença de alcance HTTP/UI pode alterar o estado do chamado e suas perguntas (`app/chamados/models.py:250`).

4. **Abrir mensagem para colaborador: formulário completo × mensagem rápida.** `/chamados/abrir/`, view `app/chamados/views.py:147`, formulário `app/templates/chamados/form_abertura.html:127` → escritor `app/chamados/services/abertura.py:122`. `/chamados/colaborador/<id>/mensagem-rapida/`, view/escrita `app/chamados/views.py:961`, formulário `app/templates/colaboradores/partials/painel_colaborador.html:188` → `ChamadoColaborador.abrir` (`app/chamados/views.py:973`). Completo cria chamado com assunto, descrição, prazo, autor/contexto e log próprio; deduplica pela entrada completa (`app/chamados/services/abertura.py:165`, `app/chamados/services/abertura.py:172`, `app/chamados/services/abertura.py:194`). Rápido reaproveita mensagem DP viva e acrescenta ocorrência (`app/chamados/views.py:973`, `app/chamados/views.py:986`). **Não muda dia/dinheiro diretamente:** o rápido escreve `mensagem_dp`; o completo permite outros tipos, que devem ser separados ao comparar (`app/chamados/views.py:975`, `app/chamados/services/abertura.py:149`).

**Convergências limpas comprovadas**

- **Autorizar geofence com/sem advertência e recusar geofence:** detalhe, formulários `app/templates/chamados/detalhe_local.html:285`, e modal, formulários `app/templates/chamados/partials/modal_fio.html:498`, convergem em `app/chamados/services/acoes_chamado.py:165` e `app/chamados/services/acoes_chamado.py:243`; roteamento modal em `app/chamados/services/fio.py:529`. Recusar converge depois em `ponto.registro_batida.retratar_batida` (`app/chamados/services/acoes_chamado.py:269`).
- **Aprovar exceção de batida:** detalhe `app/templates/chamados/detalhe_local.html:254` e modal `app/templates/chamados/partials/modal_fio.html:591` postam a mesma view e convergem em `app/chamados/services/acoes_chamado.py:21` (`app/chamados/views.py:426`).
- **Reabrir questionário:** detalhe `app/templates/chamados/detalhe_local.html:9` e modal `app/templates/chamados/partials/modal_fio.html:363` convergem na mesma view `app/chamados/views.py:1372`, escritor de reabertura chamado em `app/chamados/views.py:1435`.
- **Recusar pergunta:** detalhe `app/templates/chamados/partials/_pergunta_card.html:109` e modal `app/templates/chamados/partials/modal_fio.html:285` convergem em `app/chamados/services/validacao.py:494`, via casca `app/chamados/services/acoes_disputa.py:65`.

**Cuidados para a consolidação**

- Full `fio_colaborador.html` tem formulário de mensagem/status; não encontrei formulários de ações de disputa nele. Para disputa, usar **modal × detalhe**, evitando atribuir tela disparadora ao fio completo. [não verificado para ocorrência fora do template inspecionado]
- `corrigir_resposta` tem ramo no detalhe (`app/chamados/views.py:523`), mas o único formulário encontrado está no modal (`app/templates/chamados/partials/modal_fio.html:296`); não fechar par visual.
- `aplicar_declaracao_texto` tem escritor no detalhe (`app/chamados/views.py:478`), porém seu formulário encontrado posta ao **modal** (`app/templates/chamados/partials/modal_fio.html:137`), cujo roteador não tem esse ramo e termina em “Nada a fazer” (`app/chamados/services/fio.py:738`). Não há segunda tela comprovada: registrar como lacuna separada.
- O modal marca notas lidas inclusive no GET (`app/chamados/views.py:877`); isso fica fora da contagem solicitada de views POST.
- Não usar comentários históricos de produção como dado atual: nenhuma ocorrência de produção foi verificada nesta investigação. [não verificado]

## 06/11 21:23 · sessao 2f360630892b · - · - · origem=Ronald (tmux)

Censo fechado deste lote: **8 views POST com escrita em folha/relatórios**, sendo **4 no recorte direto** (TXT + três PDFs); mais **6 views POST de fechamento em ponto** consultadas por serem do domínio pedido. As oito são folha `lista_rubricas`, `gerar_dominio_txt`, `nova_integracao`, `editar_integracao`, `excluir_integracao`; relatórios `anexar_espelho_chamado`, `espelho_pdf_avulso`, `espelho_lote`. Referências: `app/folha/views.py:27`, `:102`, `:466`, `:487`, `:501`; `app/relatorios/views.py:971`, `:1050`, `:1152`. As seis adicionais: `app/ponto/views.py:1226`, `:1257`, `:1383`, `:1434`, `:1461`, `:1520`.

**Dois grupos repetidos, seis pares divergentes**, mais **um par de telas convergente puro**. Cadastro de rubricas/integração é auxiliar, com quatro views POST sem outra tela equivalente encontrada nesta varredura (`app/folha/urls.py:8`, `:10`, `:11`, `:12`).

1. **Aprovar fechamento — três portas, três pares divergentes.**

   - **A, competência:** UI `/ponto/fechamento/`, view `fechamento_mensal`, template `ponto/fechamento.html`; formulário de todos em `app/templates/ponto/fechamento.html:209`, seleção em `:143`. POST `/ponto/fechamento/aprovar/` → `aprovar_fechamento`, `app/ponto/views.py:1257` → **`aprovar_lote`**, `app/ponto/services/fechamento.py:1316`; rotas `app/ponto/urls.py:28`, `:32`.
   - **B, painel individual:** `/ponto/fechamento/painel/` → `painel_fechamento`, `app/ponto/views.py:966`, render em `:1006`, template `ponto/partials/painel_fechamento.html`, formulário `app/templates/ponto/partials/painel_fechamento.html:86`. POST `/ponto/fechamento/aprovar-colaborador/` → `aprovar_colaborador`, `app/ponto/views.py:1461` → **`aprovar`**, `app/ponto/services/fechamento.py:1328`; rotas `app/ponto/urls.py:30`, `:33`.
   - **C, exportação:** UI `/folha/` → `index`, `app/folha/views.py:436`, template `folha/index.html`, formulário `app/templates/folha/index.html:63`; e `/folha/exportar/` → `tela_exportar_dominio`, `app/folha/views.py:83`, template `folha/exportar.html`, preview em `app/templates/folha/exportar.html:26`, formulário gerado `app/folha/views.py:422`. POST `/folha/exportar/gerar/` → `gerar_dominio_txt`, `app/folha/views.py:102` → **mesmo `aprovar_lote` de A**, chamado em `:252`; rotas `app/folha/urls.py:7`, `:13`, `:14`.

   **Diferenças:**

   | Par | Diferença comprovada |
   |---|---|
   | A × B | A decide pelas linhas gravadas (`app/ponto/views.py:1290`); B deriva `fechamento_lido` (`:1480`). A cego considera conta vazia; A selecionado/B dispensam esse veto (`:1304`, `:1509`). A usa update com observação (`:1362`; `app/ponto/services/fechamento.py:1323`); B faz save inteiro (`:1333`). A pode trancar (`app/ponto/views.py:1367`); B só aprova (`:1515`). |
   | A × C | **Convergem em `aprovar_lote`**, mas A lê gravado (`app/ponto/views.py:1290`), C calcula em leitura (`app/folha/views.py:207`), limita carimbo a quem entra no TXT e dispensa conta vazia (`:243`–`:248`; constante `app/folha/export.py:754`). C não passa observação (`app/folha/views.py:252`), escreve `LogAuditoria` `aprovacao_por_geracao` (`:254`) e registra TXT (`:297` → `app/folha/models.py:174`). A pode trancar (`app/ponto/views.py:1367`). |
   | B × C | Ambos consultam cálculo vivo, mas B usa escritor `aprovar`/save inteiro (`app/ponto/views.py:1480`, `:1515`; `app/ponto/services/fechamento.py:1333`); C usa `aprovar_lote`/update e só subconjunto elegível para exportar (`app/folha/views.py:246`, `:252`), além de auditoria e histórico TXT (`:254`, `:297`). |

   **Leitores:** lista lê fechamento gravado (`app/ponto/views.py:1135`); painel lê `fechamento_lido` (`:993`); ambos leem a tranca viva (`:1027`, `:1126`). Trancar também dispara encerramento da fila de chamados/perguntas (`app/ponto/services/fechamento.py:1413`, `:1421`).

   **Gravidade:** **não muda dinheiro/dia diretamente neste carimbo**, que grava status, aprovador, data e eventualmente observação (`app/ponto/services/fechamento.py:1322`, `:1330`). A tranca muda fila e controles; “TXT não tranca” está declarado **conforme/intencional** no registro vivo (`app/core/juizes.py:510`, `:518`). Não inferir bloqueio financeiro: aprovado não decide entrada no TXT (`app/folha/export.py:843`), e recalcular continua gravando os valores calculados de aprovado (`app/ponto/services/fechamento.py:1139`, `:1153`). Ocorrência financeira em produção: **[não verificado]**.

2. **Emitir espelho PDF — três telas, três pares divergentes.**

   - **A, anexar ao chamado:** UI `/chamados/local/<id>/`, rota `app/chamados/urls.py:14`, view `detalhe_chamado_local`, `app/chamados/views.py:359`, render `:550`, template `chamados/detalhe_local.html`, formulário `app/templates/chamados/detalhe_local.html:348`. POST `/relatorios/espelho/chamado/<id>/anexar/`, rota `app/relatorios/urls.py:24`, view `app/relatorios/views.py:971` → **`services.gerar_pdf_espelho`**, `app/relatorios/services.py:49`, chamada `app/relatorios/views.py:1003`.
   - **B, avulso:** `/relatorios/espelho/avulso/`, rota `app/relatorios/urls.py:28`, view `app/relatorios/views.py:1050`, template `relatorios/espelho_pdf_avulso.html`, formulário `app/templates/relatorios/espelho_pdf_avulso.html:18` → **mesmo service**, chamada `app/relatorios/views.py:1127`, com `chamado=None`.
   - **C, lote:** `/relatorios/espelho/lote/`, rota `app/relatorios/urls.py:29`, view `app/relatorios/views.py:1152`, template `relatorios/espelho_lote.html`, formulário `app/templates/relatorios/espelho_lote.html:17` → **`lote_espelho.pedir`**, `app/relatorios/lote_espelho.py:83`, cria registro em `:103`; execução `:187` → `services.gerar_pdf_espelho_lote_bytes`, `app/relatorios/services.py:165` → mesmo service individual em `:237`.

   **Escritor comum do registro auditável:** `app/relatorios/pdf_espelho.py:1074` → `app/relatorios/pdf_base.py:152`, que cria `RegistroPDFAuditavel`.

   | Par | Diferença comprovada |
   |---|---|
   | A × B | Convergem no registro PDF, mas **só A** cria nota e anexo no chamado (`app/relatorios/services.py:75`, `:88`, `:90`); B passa `chamado=None` (`app/relatorios/views.py:1129`). |
   | A × C | Só A anexa ao chamado (`app/relatorios/services.py:88`); C cria lote e trilha do pedido (`app/relatorios/views.py:1227`, `:1238`) e pode substituir PDF auditável recusado por informacional (`app/relatorios/services.py:244`, `:250`). |
   | B × C | B mensal recusa `ValueError` do auditável (`app/relatorios/views.py:1131`); C captura qualquer exceção e tenta informacional sem hash/registro CLT (`app/relatorios/services.py:244`, `:250`; `app/relatorios/pdf_espelho.py:1038`). B livre limita 180 dias (`app/relatorios/views.py:1085`); validação de C passa das datas à seleção sem teto equivalente (`:1176`–`:1202`). B livre omite `por` (`:1098`), C informa `por=user` (`app/relatorios/services.py:250`), portanto o rodapé do B livre fica sem autor (`app/relatorios/pdf_espelho.py:1067`). |

   C chama a geração com `request=None` (`app/relatorios/lote_espelho.py:205`), enquanto A/B passam request (`app/relatorios/views.py:1005`, `:1129`); o registro usa `get_ip_origem(request)` (`app/relatorios/pdf_base.py:163`). C também mantém eventos de resultado (`app/relatorios/lote_espelho.py:254`). A/B convergem no bloqueio por disputa (`app/relatorios/pdf_espelho.py:1084`), mas C oferece fallback (`app/relatorios/services.py:244`).

   **Leitores:** anexo em `baixar_espelho_followup`, `app/relatorios/views.py:1020`; painel do lote em `:1284`; arquivo pronto em `:1295`.

   **Gravidade:** **não muda dinheiro/dia**: os escritores aqui criam registro de PDF, nota/anexo e lote (`app/relatorios/pdf_base.py:152`; `app/relatorios/services.py:88`; `app/relatorios/lote_espelho.py:103`). Divergem documento, autoria, IP e trilha; efeito financeiro em produção **[não verificado]**.

**Convergência pura para o fechamento do censo:** gerar TXT por `/folha/` e `/folha/exportar/` converge no **mesmo endpoint e escritor**, `gerar_dominio_txt` (`app/templates/folha/index.html:63`; `app/folha/views.py:422`; `app/folha/urls.py:14`), com registro final único em `ExportacaoDominio.registrar` (`app/folha/views.py:297`; `app/folha/models.py:174`). A montagem usa `montar_linhas_export`, `app/folha/export.py:682`, chamada `app/folha/views.py:268`.

Não contei como “duas telas” reabrir individual/reavaliar vazios/reabrir competência: suas origens encontradas estão na mesma tela de fechamento (`app/templates/ponto/_fech_linha.html:42`; `app/templates/ponto/fechamento.html:76`, `:223`); endpoints e escritores foram varridos (`app/ponto/views.py:1383`, `:1434`, `:1520`).

## 06/11 21:23 · sessao 596a49a8fba7 · - · - · origem=Ronald (tmux)

Censo de `ponto`: **18 funções de view com escrita administrativa**, expostas em **19 rotas**; a diferença é `salvar_tipo_ausencia`, compartilhada entre criação e edição. A superfície herdada do Django Admin fica separada, porque sua implementação não está nesta cópia. Evidências: `app/ponto/urls.py:37`, `app/ponto/urls.py:54`, `app/ponto/urls.py:58`, `app/ponto/urls.py:65`, `app/ponto/urls.py:83`.

Inventário nominal:

| Views | Referência |
|---|---|
| Aprovar/rejeitar justificativa individual | `app/ponto/views.py:1911`, `app/ponto/views.py:1923` |
| Aprovar/rejeitar justificativa em lote | `app/ponto/views.py:1763`, `app/ponto/views.py:1859` |
| Materializar saída retroativa | `app/ponto/views.py:3106` |
| Anexar documento de ausência | `app/ponto/views.py:2133` |
| Aprovar/rejeitar/cancelar ausência em lote | `app/ponto/views.py:2270`, `app/ponto/views.py:2304`, `app/ponto/views.py:2333` |
| Lançar ausência | `app/ponto/views.py:2368` |
| Reverter rejeição | `app/ponto/views.py:2569` |
| Aprovar ausência / editar período / rejeitar / cancelar / editar registro | `app/ponto/views.py:2599`, `app/ponto/views.py:2662`, `app/ponto/views.py:2680`, `app/ponto/views.py:2720`, `app/ponto/views.py:2751` |
| Resolver dia | `app/ponto/views.py:3312` |
| Criar/editar tipo de ausência | `app/ponto/views_tipos_ausencia.py:57` |

São **14 ações**, caso se contem separadamente: oito verbos de ausência, três de justificativa, resolver dia e criar/editar tipo. Se resolver dia for normalizado como lançamento de ausência, são 13; se materializar saída for agrupado com aprovação de justificativa, são 12. A enumeração acima permite fixar a convenção sem confundir views, rotas e verbos. Evidências: referências da tabela.

**Grupo divergente: aprovar justificativa — três caminhos explícitos.**

- Individual: `/ponto/justificativa/<jid>/aprovar/`, view `app/ponto/views.py:1911`, template `app/templates/ponto/partials/painel_justificativa.html:51` → escritor `aprovar_justificativa_dp`, `app/ponto/services/justificativa.py:117`. Valida saída posterior à entrada (`:137`), só gera saída quando existem saída informada **e entrada** (`:137`), grava estado (`:190`), invalida fechamento (`:199`), fecha origem e bloqueio de entrada (`:202`), registra LogAuditoria e followup (`:214`).
- Lote: `/ponto/justificativa/aprovar-lote/`, view e escritor `app/ponto/views.py:1763`, template `app/templates/ponto/fila_justificativas.html:176`. Grava status diretamente (`views.py:1796`), gera saída com saída informada mesmo sem entrada (`:1808`), não executa a validação saída>entrada presente na porta individual; fecha apenas origem (`:1837`). Não chama o fechamento específico de bloqueio, LogAuditoria da aprovação ou followup que existem na porta individual (`services/justificativa.py:203`, `:215`, `:219`).
- Emergência: `/ponto/justificativa/<jid>/materializar-saida/`, view `app/ponto/views.py:3106`, template `app/templates/ponto/materializar_saida_retroativa.html:38` → `materializar_saida_dp`, `app/ponto/services/justificativa.py:316`. Recebe novo horário e exige motivo com dez caracteres (`views.py:3130`); carrega qualquer status (`services/justificativa.py:329`), grava `saida_informada` e status aprovado (`:367`), fecha cascata de chamados (`:378`) e registra outra ação de auditoria (`:398`). Valida saída>entrada quando há entrada (`:340`).

Esse grupo contém três pares combinatórios; os escritores de aprovação são distintos. A diferença pode alterar a saída registrada e o resultado do dia; leitor financeiro específico fica com a análise do pai, mas a própria porta identifica a aprovação como insumo do fechamento e invalida sua projeção. `app/ponto/services/justificativa.py:194`, `app/ponto/views.py:1804`.

**Par divergente: rejeitar justificativa individual × lote.**

Individual: `/ponto/justificativa/<jid>/rejeitar/`, `app/ponto/views.py:1923`, template `app/templates/ponto/partials/painel_justificativa.html:58` → `app/ponto/services/justificativa.py:247`: decisão em atomic com trava (`:258`), chamado de rejeição (`:275`) e followup (`:309`). Lote: `/ponto/justificativa/rejeitar-lote/`, view/escritor `app/ponto/views.py:1859`, template `app/templates/ponto/fila_justificativas.html:188`: get e save direto (`views.py:1874`), push (`:1882`), sem os chamados/followup da porta individual. O evento comum rejulga o dia nos dois saves: `app/ponto/signals.py:161`. Portanto, não atribuir ao lote ausência de rejulgamento.

**Par que converge no escritor, mas diverge no alcance/validação: aprovar ausência individual × lote.**

Individual: `/ponto/ausencias/<aid>/aprovar/`, `app/ponto/views.py:2599`, template `app/templates/ponto/partials/painel_ausencia.html:309`. Expande raiz e irmãos (`views.py:2615`), permite confirmação de desconto com auditoria extra (`:2612`), recebe período coberto (`:2629`) e envolve os irmãos num atomic (`:2641`); envia push (`:2646`).

Lote: `/ponto/ausencias/lote/aprovar/`, `app/ponto/views.py:2270`, template `app/templates/ponto/ausencias.html:308`. Só processa os IDs enviados (`views.py:2280`); gate com `confirmado=False` (`:2284`); não recebe período. Ambos convergem em `_a4_decidir`, `app/ponto/views.py:82` → `decidir`, `app/ponto/services/ausencia.py:897` → `mudar_estado`, `app/ponto/services/ausencia.py:808`.

**Par que converge no escritor, mas diverge no alcance/campo: rejeitar ausência individual × lote.**

Individual: `/ponto/ausencias/<aid>/rejeitar/`, `app/ponto/views.py:2680`, template `app/templates/ponto/partials/painel_ausencia.html:330`: expande raiz/irmãos (`views.py:2688`), não recebe motivo e envia push (`:2704`). Lote: `/ponto/ausencias/lote/rejeitar/`, `app/ponto/views.py:2304`: só IDs selecionados, recebe motivo e acrescenta em `descricao` (`:2311`). Ambos passam por `_a4_decidir` (`:2701`, `:2319`). Existe função JS emissora em `app/templates/ponto/ausencias.html:316`, mas botão que a invoque no template atual **[não verificado]**; a barra visível contém Aprovar e Cancelar (`:117`, `:120`).

**Convergências fechadas e limites.**

- Cancelar ausência individual e lote convergem no mesmo escritor e nos mesmos campos/auditoria: `app/ponto/views.py:2734`, `app/ponto/views.py:2351`. O botão atual do lote está em `app/templates/ponto/ausencias.html:120`; disparador de tela do cancelamento individual **[não verificado]**.
- Resolver falta/justificado no calendário e diagnóstico converge na mesma view `/ponto/celula/veredito/`, `app/ponto/views.py:3312` → `app/ponto/services/veredito_celula.py:191`. Calendário inclui formulário em lote (`app/templates/colaboradores/calendario_partial.html:63`); diagnóstico também (`app/templates/colaboradores/partials/_drawer_diagnostico.html:36`); ação unitária usa o mesmo endpoint (`app/templates/colaboradores/partials/_calendario_grade.html:126`, `app/templates/ponto/partials/_veredito_dia.html:17`). O ramo falta/justificado chama `criar_ausencia` (`services/veredito_celula.py:315`), fecha fio (`:325`) e lavra o dia (`:330`).
- **Não afirmar Resolver dia no espelho ou fechamento:** existe comentário na view do espelho (`app/ponto/views.py:163`), mas disparador nos templates atuais **[não verificado]**.
- Anexar documento tem view administrativa e chama a porta única: `app/ponto/views.py:2133` → `app/ponto/services/ausencia.py:846`; disparador no template atual **[não verificado]**.
- Django Admin: registro `JustificativaAdmin(admin.ModelAdmin)` aberto em `app/ponto/admin.py:18`; `Justificativa` não sobrescreve `save` no corpo da classe (`app/ponto/models.py:115` até `:197`) e declara status/decisor/data (`:159`). `AusenciaAdmin` usa SoLeitura (`app/ponto/admin.py:38`), cujo bloqueio está em `app/core/admin.py:59`. Django aparece como dependência em `app/requirements.txt:13`; URL detalhada, view, template e escritor herdados **[não verificado]**, pois o código do framework não está na cópia nem instalado no Python disponível.

## 06/11 21:23 · sessao f2a149a1cf55 · - · - · origem=Ronald (tmux)

Censo Q2 — somente leitura, sem contagens de produção.

**Distinção dos estados.** `PerguntaDisputa.estado` é propriedade derivada; combina `validada_em`, C6 e `materializada_em`, não há coluna `status` da pergunta (`app/chamados/models.py:2491-2506`). `DisputaSupervisao` abre/fecha por `fechada_em`; `.aberta` significa `fechada_em is None` (`app/chamados/models.py:1686-1687,1763-1765`); `motivo_fechamento` é coluna própria (`app/chamados/models.py:1708-1725`). `ChamadoColaborador.status_local` é outro estado; o selo distingue explicitamente os dois modelos/campos (`app/chamados/tests/test_selo_status_so_pelas_portas.py:28-53`).

| Fronteira | Escritores e leitores conferidos |
|---|---|
| Escrita — identidade/data/destino | `PerguntaDisputa.abrir` nasce/reutiliza/reaponta (`app/chamados/models.py:2398-2477`); `fio.reapontar_resolvedora` altera destino, só preenche `data_evento` quando antes vazio e relavra no mesmo save (`app/chamados/services/fio.py:829-848`). Dois módulos permitidos pelo contrato, não módulo único (`app/core/tests/test_contract_escritores_estado.py:48-62`). |
| Escrita — resposta/validação | Resposta humana: `responder_pergunta_disputa_core` escreve texto+instante, desfaz validação anterior e pode descarimbar resolução sem lastro (`app/chamados/services/disputa_emissao.py:1282-1301`). Resposta automática sem sobrescrever texto: `responder_automaticamente` (`app/chamados/services/disputa_emissao.py:1422-1435`). Reconstrução de instante com lastro da nota: `carimbar_resposta_pela_trilha` (`app/chamados/services/disputa_emissao.py:1439-1453`). Validação administrativa escreve em `validacao.py:131-133`; devolução desfaz em `validacao.py:187-190`. |
| Escrita — carimbo/resolução | `_escrever_carimbo` centraliza materializada_em/por/via (`app/chamados/models.py:2556-2560`); portas `carimbar`, `carimbar_identidade`, `descarimbar`, `descarimbar_em_lote` (`app/chamados/models.py:2562,2625,2646,2692`). Via isolada também possui portas próprias; selo diferencia isso (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:35-38`; `encerrar_por_fato_no_espelho`, `app/chamados/models.py:2702-2710`). |
| Escrita — disputa/chamado | Selo AST declara portas de Chamado `_gravar_estado`, `criar`, `abrir`, `_vivas_entre` somente em memória; Disputa `fechar`, `reabrir_admin`, `reabrir_sistema` (`app/chamados/tests/test_selo_status_so_pelas_portas.py:36-53`). Implementações: chamado `models.py:195,332,796,1015`; disputa `models.py:1796,2040,2077`. Fechamento disputa grava fechada_em/por/resolucao/motivo em atomic (`app/chamados/models.py:1845-1869`). |
| Escrita — célula | Portas `regenerar_celulas_vinculo`, `editar_dia`, `gerar_celulas_periodo`, `lavrar_veredito`, `carimbar`, `carimbar_precedencia` (`app/ponto/portas/celula.py:110,534,811,969,1047,1071`). `lavrar_veredito` escreve ata, veredito/via/em/protestos e status_dia/status_rotulo (`app/ponto/portas/celula.py:969-1040`). `gerar_celulas_periodo` cria/atualiza trabalha, geradora e DNA; preserva passado salvo enriquecimento e edição humana (`app/ponto/portas/celula.py:847-871`). |
| Materialização | Seleciona perguntas respondidas+validadas e ainda não materializadas (`app/chamados/services/materializacao.py:1344-1354`); hora gera batida a partir de data_evento+hora, data_hora ancora ciclo, escolha pode declarar ausência (`app/chamados/services/materializacao.py:1313-1328`). Data pergunta divergente do chamado bloqueia até conferência humana (`app/chamados/services/materializacao.py:1156-1173`). Lê `chamado_destino` para operação/fecho (`app/chamados/services/materializacao.py:339-345,1049-1052`). |
| Consulta — pergunta | Identidade de leitura `do_fato(colab,motivo,data_evento)` (`app/chamados/models.py:2381-2395`); C6 exato por `.viva` (`app/chamados/models.py:2798-2807`); `q_vivas` é **superset**, não equivalente SQL completo (`app/chamados/models.py:2770-2777`). `pode_fechar` lê C6 e respostas sem veredito (`app/chamados/models.py:1767-1794`). |
| Consulta — cobrança | `aviso_pendencia.perguntas_pendentes_do_colab` = q_vivas + C6 exato + respondida_em nulo (`app/chamados/services/aviso_pendencia.py:7-13`). `cobranca._pendentes` delega; `checar_cobranca` lê data_evento, disputa e SLA do chamado **pai** (`app/chamados/services/cobranca.py:45-67,75-94`). |
| Consulta — célula/fábrica | `motivos_a_perguntar` lê trabalha/ata por juiz, situação ativa, data, teto por instante, veredito e parcial_so_lista; também deduplica qualquer pergunta histórica do dia exceto via duplicata (`app/chamados/services/fabrica_da_celula.py:74-126`). `_marcos_faltantes` lê grade de d-1 e d, tipo_dia/cobertura parcial, marcos/células missing (`app/chamados/services/declaracao_texto.py:65-123`). |
| Tela admin | `fio.montar_chamados_fio` inclui perguntas C6 vivas de disputa fechada, sem reabrir, e ancora por destino (`app/chamados/services/fio.py:209-239`). Modal lê datas via filtros, mostra ambos os dias divergentes e conferência (`app/templates/chamados/partials/modal_fio.html:156-179`), resposta/carimbos/pendência/admin (`app/templates/chamados/partials/modal_fio.html:212-245`). Relatório lê somente disputas abertas e classifica perguntas vivas (`app/chamados/relatorios_disputa.py:11-23,40-47`). |
| Tela app/API | `perguntas_do_app` busca todas as perguntas do colab, q_vivas e C6 exato, sem filtro de disputa aberta (`app/chamados/services/pergunta_no_app.py:42-53`). API delega essa seleção, entrega estado explícito, separa respondidas/aguarda_admin e destino (`app/api/views.py:2551-2594`); POST aceita somente pergunta C6 viva e chama core (`app/api/views.py:2627-2643`). |
| Tela PWA colab | O fio com `so_janela=True` segue pelas disputas abertas, diferente do admin (`app/chamados/services/fio.py:209-221`); card condiciona formulário por `.viva`, apresenta data_evento, resposta e estado (`app/templates/chamados/partials/_card_atendimento.html:77-100,183`). |

**Universos e significados que divergem.**

- `data_evento` é DateField, descrito como snapshot do serviço de jornada (`app/chamados/models.py:2218-2220`), mas nasce de três espécies distintas: dia do contexto/grade (`app/chamados/services/disputa_emissao.py:319-347`), **data civil local da batida** para anomalia (`app/chamados/services/disputa_emissao.py:350-364`) e **localdate do nascimento** para conversa, que nasce sempre sem dedup (`app/chamados/models.py:2422-2427`). Não cabe assumir que toda data_evento já é turno canônico.
- A porta trata identidade pelo valor cru colab+motivo+data_evento e pega a última linha sem filtro de estado (`app/chamados/models.py:2392-2395,2430-2432`); o gate de compatibilidade com destino, porém, declara unidade **turno**, não igualdade aritmética de datas (`app/chamados/models.py:2408-2421`).
- `fio.data_do_fato_de` lê p.data_evento e compara com `dia_do_fato(ch, fortes=True)`; devolve os dois e divergência, não canonicaliza nem corrige data (`app/chamados/services/fio.py:40-60,77-88`). `reapontar_resolvedora` não re-chaveia data existente, apenas completa a ausente (`app/chamados/services/fio.py:831-835`).
- `estado` pode rotular PENDENTE/RESPONDIDA mesmo quando C6 julga morta sem carimbo; a própria propriedade `.viva` documenta essa diferença (`app/chamados/models.py:2502-2506,2802-2807`). Não usar `.estado` como sinônimo de exigibilidade.
- A API serve vivas em disputa fechada (`app/chamados/services/pergunta_no_app.py:46-53`), mas badge do colab exige disputa aberta (`app/chamados/models.py:1729-1740`) e PWA fio declara restrição pelas abertas (`app/chamados/services/fio.py:209-221`). São leitores com universos diferentes.
- SLA da cobrança fica no chamado **pai** (`app/chamados/services/cobranca.py:51-66`); apresentação/resolução usa chamado **destino**, com pai apenas como fallback (`app/chamados/services/fio.py:97-117`). Destino não significa a mesma coisa que responsável/SLA.
- Carimbos respondida_em/validada_em/materializada_em são instantes; `data_evento` é dia; vencimento de janela é dia (`app/chamados/models.py:2218-2229,2356-2361,2827-2829`). Não agrupar pelo mesmo relógio sem contrato explícito.
- `_coberto_por_ausencia` possui docstring antiga “aprovada não-falta”, mas implementação usa ausência vencedora para cobrança, explica dia inteiro **ou parcial sem período**, preserva resto de parcial com período (`app/chamados/services/materializacao.py:55-82`). A documentação não descreve integralmente o universo atual.

**Vigia fio — escritores/leitores.**

- `vigia_fio` é leitura e composição, não escreve estado de domínio (`app/chamados/services/vigia_fio.py:318-323`). `fio_do_dia` delega à redução `fio_vivo` (`app/chamados/services/vigia_fio.py:156-166`).
- `censo`: universo chamado vivo com célula, recorte empresa, módulos FURO_MODULES; depois células VEREDITOS_ACUSAM; perguntas agrupadas por célula; hoje/futuro fora do alarme (`app/chamados/services/vigia_fio.py:334-379,398-424`).
- `censo_alcance`: outro universo, empresas visíveis × competências a lavrar × dias_que_seguram folha; alimenta portas uma vez e lê alcance real (`app/chamados/services/vigia_fio.py:555-618`).
- `dono_fora_do_fio` lê Resolver dia oferecido, ausência aberta, aceite de folga e lista do admin, nesta ordem (`app/chamados/services/vigia_fio.py:274-315`); não exige exatamente um responsável: retorna o primeiro balde.
- `ListaDoAdmin` lê lista Cadastro x Realidade no universo empresas visíveis e dono BATIDA/CADASTRO; falha preserva o alarme (`app/chamados/services/vigia_fio.py:188-228`).
- Contadores adicionais: disputa vazia (`app/chamados/services/vigia_fio.py:727-734`), viva em fechada (`:737-759`), respondida sem texto depois de marco temporal (`:774-786`), carimbo sem lastro dividido por competência entregue (`:789-844`). São universos distintos.
- Command consumidor lê todos na mesma rodada/relógio (`app/chamados/management/commands/vigiar_fio_morto.py:159-176`) e **escreve arquivo de medição**, não domínio, `_carimbar` (`:115`) com censo+alcance+passivos (`:211-225`), somente frota, não recorte empresa (`:228-230`).
- Cron chama o command (`app/config/crons.py:1703`). Leitor host usa `app/logs/fio_morto.json`, valida alcance e recusa universo vazio como verde (`bin/fio_estado.py:61-88`); `bin/gerar_estado.py:255` referencia a mesma fonte.

**Já existe outbox parcial, relevante ao ADR.**

- Em célula, os campos que acordam são somente veredito e ata; save+evento no mesmo atomic (`app/ponto/portas/celula.py:878-902`).
- Pergunta possui conjunto próprio amplo de campos que acordam (`app/chamados/models.py:2655-2662`); `PerguntaDisputa.save` e signal outbox são atômicos (`app/chamados/models.py:2847-2855`, `app/chamados/signals.py:205-221`). Nascimento pergunta não emite por esse receiver (`app/chamados/signals.py:211-217`).
- Reabertura por update salva `RespostaAnterior` e emite célula explicitamente no mesmo atomic (`app/chamados/services/disputa_emissao.py:1393-1417`).
- Já há reaberturas reais: ressuscitar perguntas pelo C6 sobre cópia pós-estado (`app/chamados/models.py:1420-1458,1461`); sistema reabre disputa sem exigir user/teto (`app/chamados/models.py:2077-2095`); nova resposta derruba validação e carimbo sem lastro (`app/chamados/services/disputa_emissao.py:1282-1301`). Isso não equivale ao protocolo de episódio com extinção provada do ADR.

**Limite do censo:** conferi os pontos de produção e os contratos AST acima; não executei os selos nem consultei dados. “Não existem outros escritores fora das portas” é a intenção verificável do selo, mas passagem atual dos testes está [não verificado]. Comentários de testes antigos também envelhecem: `test_porta_disputa_ciclo.py:50-56` ainda descreve escrita em lote no desligamento, enquanto o selo mais específico só permite as portas (`test_selo_status_so_pelas_portas.py:36-53`) e o código de desligamento registra a retirada desse update (`app/colaboradores/services/desligamento.py:133`).

## 06/11 21:23 · sessao 8a3204923ac2 · - · - · origem=Ronald (tmux)

Censo de `ponto`: **10 views POST administrativas; 9 ações semânticas**, contando aprovação/rejeição de justificativa separadamente, materialização, flip, retratação, desfazer, resolver dia e aprovação/rejeição offline. As rotas estão em `app/ponto/urls.py:37`, `:40`, `:41`, `:42`, `:43`, `:44`, `:58`, `:59`, `:61`, `:62`, `:77`; views em `app/ponto/views.py:434`, `:1763`, `:1859`, `:1911`, `:1923`, `:3106`, `:3260`, `:3274`, `:3286`, `:3312`.

1. **Aprovar justificativa: unitário × lote — divergente, muda dia/dinheiro.**
   - Tela A: `/ponto/justificativa/<jid>/painel/`, view `app/ponto/views.py:1662`, template `app/templates/ponto/partials/painel_justificativa.html:51`, POST `/ponto/justificativa/<jid>/aprovar/`, view `app/ponto/views.py:1911` → escritor `app/ponto/services/justificativa.py:117`.
   - Tela B: `/ponto/justificativa/fila/`, view `app/ponto/views.py:1695`, template `app/templates/ponto/fila_justificativas.html:176`, POST `/ponto/justificativa/aprovar-lote/`, view/escritor `app/ponto/views.py:1763`, escrita direta `:1796`.
   - Unitário valida saída posterior à entrada (`app/ponto/services/justificativa.py:137`), exige entrada para criar S (`:137`), retorna sem aprovar quando já há `batida_gerada` (`:169`). Lote aprova antes (`app/ponto/views.py:1796`), cria S apenas exigindo saída informada e ausência de gerada (`:1808`), sem a validação saída>entrada.
   - Unitário fecha também bloqueio de entrada (`app/ponto/services/justificativa.py:203`) e registra LogAuditoria/followup (`:214`, `:218`). Lote chama encerramento da origem (`app/ponto/views.py:1837`), sem esses atos explícitos.
   - Leitor: `app/ponto/services/fechamento.py:660` inclui justificativas aprovadas em `datas_justificadas`; motor usa isso para faltas, abono e DSR em `app/ponto/motor_calculo_v2.py:2600`, `:2605`, `:2618`, `:2620`.

2. **Aprovar/materializar saída da justificativa: três caminhos — grupo divergente.**
   - A/B anteriores; C `/ponto/justificativa/<jid>/materializar-saida/`, view `app/ponto/views.py:3106`, template `app/templates/ponto/materializar_saida_retroativa.html:38` → escritor `app/ponto/services/justificativa.py:316`.
   - C exige horário e motivo ≥10 caracteres (`app/ponto/views.py:3133`, `:3135`), aceita justificativa por PK sem exigir status pendente (`app/ponto/services/justificativa.py:329`), grava saída informada, gerada e aprovada (`:365`), fecha chamados de turno por cascata (`:378`) e origem (`:393`), registra `mat_saida_retro` (`:398`). A/B exigem pendente (`app/ponto/services/justificativa.py:127`; `app/ponto/views.py:1794`).
   - C pode aprovar justificativa previamente rejeitada sem batida vinculada; A/B não selecionam esse estado. Impacto em dia/dinheiro pelo mesmo leitor de justificativas aprovadas, `app/ponto/services/fechamento.py:660`.

3. **Rejeitar justificativa: unitário × lote — divergente.**
   - Tela A painel, formulário `app/templates/ponto/partials/painel_justificativa.html:58`, view `app/ponto/views.py:1923` → `app/ponto/services/justificativa.py:247`.
   - Tela B fila, formulário `app/templates/ponto/fila_justificativas.html:188`, view/escritor `app/ponto/views.py:1859`.
   - Unitário usa atomic, trava origem e `select_for_update` (`app/ponto/services/justificativa.py:258`, `:261`, `:262`), abre chamado de rejeição (`:276`) e followup (`:309`). Lote lê e salva diretamente (`app/ponto/views.py:1874`), fazendo push (`:1883`), sem os mesmos atos.
   - Rejeitar o mesmo pendente mantém o mesmo status nominal, mas divergência de concorrência permite sobrescrever decisão intermediária no lote; ocorrência em produção [não verificado]. O chamado/fio diferem pelo escritor `app/ponto/services/justificativa.py:276`; todo save rejulga o dia em `app/ponto/signals.py:161`.

4. **Retratar marcação: calendário × drawer — divergente de formulário, mesmo escritor.**
   - Calendário: rota `app/colaboradores/urls.py:24`, view `app/colaboradores/views.py:1935`, template `app/templates/colaboradores/calendario_partial.html:62`.
   - Drawer: rota `app/colaboradores/urls.py:86`, view `app/colaboradores/views.py:2938`, template `app/templates/colaboradores/partials/_drawer_diagnostico.html:35`.
   - Ambos incluem formulário de retratação `app/templates/colaboradores/partials/_calendario_grade.html:121` → view `app/ponto/views.py:3274` → **converge em** `app/ponto/services/retratar_humano.py:150`.
   - Calendário fornece `motivos_retratacao` (`app/colaboradores/views.py:1956`, `:1958`); contexto do drawer não fornece essa chave (`app/colaboradores/services/drawer.py:139`). O select obrigatório fica sem opções válidas no drawer; o escritor exige motivo cadastrado/ativo (`app/ponto/services/retratar_humano.py:166`). Resultado visual inferido do código; navegador/produção [não verificado].
   - Muda dia/dinheiro: retratação tira marcação do universo apurável (`app/ponto/turnos.py:2864`), lido pelo espelho (`app/ponto/services/espelho.py:1022`, `:1179`).

Convergências para o fim:

- **Flip**: calendário + drawer acima, formulário comum `app/templates/colaboradores/partials/_calendario_grade.html:121`; terceira tela `/ponto/flip-fila/`, view `app/ponto/views.py:3299`, template `app/templates/ponto/flip_fila.html:16`. Todos → view `app/ponto/views.py:3260` → **converge em** `app/ponto/services/flip_batida.py:10`. Fila fornece motivo; calendário/drawer omitem, aceito pelo argumento padrão do escritor (`:10`); trilha/recompute comuns (`:30`, `:39`).
- **Desfazer retratação**: calendário + drawer, formulário comum `app/templates/colaboradores/partials/_calendario_grade.html:121` → view `app/ponto/views.py:3286` → **converge em** `app/ponto/services/retratar_humano.py:190`; prazo e origem conferidos igualmente (`:199`, `:203`).
- **Resolver dia**: calendário + drawer e formulários individual/lote `app/templates/ponto/partials/_veredito_dia.html:17`, `app/templates/ponto/partials/_veredito_lote_barra.html:2` → mesma view `app/ponto/views.py:3312` → **converge em** `app/ponto/services/veredito_celula.py:191`. Completa batidas (`:301`), cria ausência (`:315`), fecha perguntas/chamados (`:325` → `:160`) e lavra célula/trilha (`:330`, `:334`).

Fora dos pares: aprovação/rejeição offline possuem uma tela local, `app/ponto/views.py:434`, template `app/templates/ponto/batidas_offline_pendentes.html:108`, `:114`; escrevem via `anotar`/`apagar` em `app/ponto/views.py:472`, `:479`. `abrir_justificativa` e `regularizar_retorno` são selfserve do usuário-colaborador (`app/ponto/views.py:1558`, `:3168`); painel de justificativa e espelho não possuem ramo de escrita (`app/ponto/views.py:1662`, `:94`).

## 06/11 21:22 · sessao 53ecda1d422f · - · - · origem=Ronald (tmux)

Censo `holerite`: **6 views POST administrativas escritoras**, distribuídas em publicar documento, enviar aviso e reenviar aviso; `upload_lote_v2` também cria a prévia/lote. Rotas: `app/holerite/urls.py:8`, `:9`, `:14`, `:16`, `:37`, `:39`. Escritas: `app/holerite/views.py:618`, `:745`, `:1193`, `:1255`, `:1259`, `:1758`, `:1773`, `:1823`, `:1828`.

**Publicar holerite: grupo com três pares divergentes.**

| Tela / rota | View → escritor | Template |
|---|---|---|
| `/holerite/upload/` | `app/holerite/views.py:1145` → delete dos antigos `:1191`, create `:1193` | `app/templates/holerite/upload.html:9`; outro disparador: `app/templates/holerite/_drawer_modo_avulso.html:11`, renderizado por `app/holerite/views.py:363` |
| `/holerite/upload-lote/` | `app/holerite/views.py:1217` → delete `:1255`, create `:1259` | `app/templates/holerite/upload_lote.html:12` |
| `/holerite/lote/` | `app/holerite/views.py:1721` → job `:1759` → `app/holerite/management/commands/processar_lote_holerite.py:89` → `app/holerite/motor.py:390`, grava documento `:480` | `app/templates/holerite/upload_lote_v2.html:23`; drawer `app/templates/holerite/_lote_drawer.html:17`; drawer empresa `app/templates/holerite/_drawer_modo_lote.html:13`; confirmar publicação `app/templates/holerite/_lote_progresso.html:119` |

1. **Avulso × ZIP:** avulso barra substituição se há ciência, registra trilha antes de apagar e preenche `empresa_folha_id`; ZIP apaga todos da pessoa/competência sem essas verificações nem preencher empresa. `app/holerite/views.py:1174`, `:1176`, `:1184`, `:1191`, `:1202`, `:1255`, `:1259`. Ciência é `CASCADE`, portanto o delete ZIP apaga a assinatura vinculada: `app/holerite/models.py:193`. Leitor: matriz separa documentos por `empresa_folha_id` em `app/holerite/views.py:295`; ciência determina estado “assinado” em `app/holerite/matriz.py:476`. **Dinheiro/dia: não diretamente; perde prova de ciência e altera identificação da folha na matriz** — campos gravados em `app/holerite/views.py:1193`, `:1259`; efeito financeiro real [não verificado].

2. **Avulso × lote PDF:** avulso substitui documento sem ciência e cria sem lote; PDF pula pessoa já publicada (`ja_publicado`), mantém assinatura/documento, grava empresa do CNPJ e vínculo ao lote. `app/holerite/views.py:1174`, `:1191`, `:1193`; `app/holerite/motor.py:323`, `:344`, `:479`, `:480`. PDF roda gravação em transação, `app/holerite/motor.py:385`; carimba publicação do lote em `app/holerite/management/commands/processar_lote_holerite.py:129`. **Dinheiro/dia: sim, potencialmente**, pois só documento com lote publicado fecha competência para célula: `app/folha/export.py:614`. Avulso cria sem esse lote em `app/holerite/views.py:1193`. Guarda de regeneração enumera apenas holerite com lote publicado em `app/ponto/management/commands/gerar_celulas.py:169` e pergunta ao juiz em `:173`; edição do dia também consulta essa guarda em `app/ponto/portas/celula.py:568`. Alteração concreta em produção [não verificado].

3. **ZIP × lote PDF:** além da mesma divergência de fechamento, ZIP deleta inclusive documento com ciência e recria sem empresa/lote; PDF faz publicação incremental, com empresa do documento/lote e preservação dos existentes. `app/holerite/views.py:1255`, `:1259`; `app/holerite/motor.py:323`, `:344`, `:415`, `:479`, `:480`; fechamento exige lote publicado em `app/folha/export.py:614`. **Dinheiro/dia: sim, potencialmente**, pela guarda diferente da célula; perda de ciência também comprovada pelo `CASCADE` de `app/holerite/models.py:193`. Ocorrência em produção [não verificado].

**Avisar/reenviar holerite: dois pares divergentes com o histórico.** Entrega pode ficar fora da interpretação estrita de C; os caminhos estão fechados abaixo para decisão do consolidado. Escopo desses caminhos: `app/holerite/views.py:605`, `:734`, `:1794`.

| Tela / rota | View → escritor | Template |
|---|---|---|
| Drawer da competência, `/holerite/celula/<empresa>/<ano>/<mes>/enviar/` | `app/holerite/views.py:605` → `app/holerite/servicos.py:145` → `:76` | `app/templates/holerite/_celula_lista.html:26` |
| Mesmo drawer, `/holerite/celula/<empresa>/<ano>/<mes>/reenviar/` | `app/holerite/views.py:734` → `app/holerite/servicos.py:76`, com não lidos e trava | `app/templates/holerite/_celula_lista.html:34` |
| Histórico, `/holerite/lote/<id>/avisar/` | `app/holerite/views.py:1794` → push direto `:1813`, update `:1821`, save lote `:1828` | `app/templates/holerite/historico_lotes.html:36`, `:38`, `:57` |

- **Enviar drawer × avisar histórico:** drawer seleciona pelo filtro e pessoa (`app/holerite/views.py:565`), usa FCM com fallback Web Push (`app/holerite/servicos.py:38`, `:42`), só novos (`:154`), limpa/grava falha (`:113`, `:129`) e audita envio (`:134`). Histórico seleciona itens `ok` do lote, usa só FCM e marca todos os documentos da pessoa/competência, sem atualizar falha; também carimba o lote (`app/holerite/views.py:1808`, `:1813`, `:1821`, `:1826`). Leitor: `app/holerite/matriz.py:480`, `:493` usa aviso/falha; histórico lê aviso do lote em `app/templates/holerite/historico_lotes.html:34`. **Dinheiro/dia: não**, escritas atingem aviso/falha/lote, conforme `app/holerite/servicos.py:113`, `:129` e `app/holerite/views.py:1821`.

- **Reenviar drawer × reenviar histórico:** drawer pula já lidos e respeita trava horária (`app/holerite/servicos.py:103`, `:106`; parâmetros em `app/holerite/views.py:745`); histórico manda `forcar=1` e repete para todos os itens `ok` sem essas guardas (`app/templates/holerite/historico_lotes.html:55`; `app/holerite/views.py:1805`, `:1808`, `:1813`). Mesmos leitores e gravidade do par anterior, pelas escritas de `app/holerite/views.py:1821` e estados de `app/holerite/matriz.py:476`.

**Convergências separadas.**

- Avulso em página inteira e drawer **converge em `upload_holerite`**, mesmo escritor: `app/templates/holerite/upload.html:9`, `app/templates/holerite/_drawer_modo_avulso.html:11`, `app/holerite/views.py:1193`.
- Lote PDF em página inteira, drawer genérico e drawer da empresa **converge em `upload_lote_v2` → job → `motor._gravar`**: templates em `app/templates/holerite/upload_lote_v2.html:23`, `app/templates/holerite/_lote_drawer.html:17`, `app/templates/holerite/_drawer_modo_lote.html:13`; cadeia em `app/holerite/views.py:1759`, `app/holerite/management/commands/processar_lote_holerite.py:89`, `app/holerite/motor.py:390`. Drawer empresa acrescenta `empresa` (`app/templates/holerite/_drawer_modo_lote.html:19`), que gera validação de CNPJ esperado na análise (`app/holerite/management/commands/processar_lote_holerite.py:107`), ausente quando não fornecido — portanto convergência com validação contextual diferente.
- Enviar/reenviar, execução curta e job **convergem em `servicos.enviar`**: `app/holerite/views.py:618`, `:745`; `app/holerite/servicos.py:155`; job em `app/holerite/management/commands/enviar_holerites_em_lote.py:66`. São ramificações de execução, sem segunda tela própria comprovada [não verificado].
- Lista de entrega e relatório PDF convergem em `_relatorio_da_selecao`, mas **sem escritor de estado**, excluídos do censo POST: `app/holerite/views.py:963`, `:971`, `:977`.

Exportação CSV é GET com escrita apenas de trilha (`app/holerite/views.py:1071`, `:1097`), assim como maço PDF (`:917`, `:943`); não achei outro caminho da mesma ação entre rotas/templates de holerite (`app/holerite/urls.py:18`, `:26`; segundo disparador [não verificado]).

_2581 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
