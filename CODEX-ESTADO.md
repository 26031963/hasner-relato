# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 08:00** · publicado em **29/10/2026 10:42** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 08:00 · sessao 1ee182181f58

HEAD: `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. Sem escrita, testes ou banco. **Hipótese** abaixo indica caminho possível por leitura, não ocorrência demonstrada.

## T12 — Entradas do julgamento fora da impressão

A chave contém Batidas `(pk,tipo,timestamp)`, cobertura escolhida `.status`, Chamados `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto de régua, quantidade de marcos vencidos, versão da regra, encerramento, quantidade de chamados com fio e classificação da cobertura parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O pulo normal ocorre em `:1092`; o ramo sem adesão tem chave/pulo próprios em `:941`, `:943`.

| Entrada efetivamente lida | Está na chave? |
|---|---|
| Batidas apuráveis da janela (`app/ponto/services/cartorio.py:871`, `:1042`) | **Sim**, identidade, tipo e instante (`:324`). Retratação muda o conjunto apurável. |
| `Batida.is_intervalo`, consumida pela grade/pareador (`app/escala/utils.py:1267`; `app/ponto/turnos.py:1261`) | **Não** na tupla serializada (`app/ponto/services/cartorio.py:324`). |
| Primeira batida apurável **da vida**, que define adesão (`app/ponto/services/cartorio.py:899`, `:901`) | **Não diretamente**: a chave usa a janela local `bj` (`:1042`). `nunca_bateu` tem exceção ao pulo normal (`:1092`), mas isso não serializa a data de adesão. |
| Chamados e estados usados no julgamento (`app/ponto/services/cartorio.py:1040`, `:1069`) | **Sim**, identidade/estado (`:326`). |
| Fio vivo (`app/ponto/services/cartorio.py:1074`, `:1110`) | **Sim como quantidade**, não identidade/conteúdo das perguntas (`:1087`, `:367`). |
| Bordas/pausas do DNA da célula corrente (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, em `dna.marcos` (`app/ponto/services/cartorio.py:327`). |
| `CelulaDia.trabalha`, geradora e vínculo escolhido (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: o hash da célula contém apenas versão/marcos do DNA (`app/ponto/services/cartorio.py:327`). |
| Vigência, atividade, âncora, ciclo, intervalo livre, defeito de folga e template (`app/escala/utils.py:1240`, `:1300`, `:1313`; `app/ponto/supra_juiz.py:175`) | **Parcial**: marcos congelados entram; os demais campos não são serializados (`app/ponto/services/cartorio.py:327`). |
| FolgaDia/foto, que sobrepõe tipo e remove faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`app/ponto/services/cartorio.py:323`). |
| Justificativa aprovada, limpeza de faltantes e `dia.justificado` (`app/escala/utils.py:1157`, `:1200`, `:1203`) | **Não**. A ata lê essa marca para dispensar lâmpadas (`app/ponto/services/cartorio.py:602`, `:610`). |
| Ausência escolhida para cobrança, inclusive não aprovada (`app/ponto/services/cartorio.py:1041`) | **Parcial**: somente o status escolhido (`:325`), não identidade, tipo, datas, minutos ou horas. |
| Cobertura aprovada usada pela grade para produzir tipo do dia, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: o hash recebe status da cobertura de cobrança e classificação parcial, não a linha completa da cobertura de folha (`app/ponto/services/cartorio.py:1083`, `:1088`). |
| Integralidade e posição das batidas perante ausência parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: parcial/`sem_periodo`/`dentro`/`fora` (`app/ponto/services/cartorio.py:375`, `:1088`); horas/minutos exatos não entram. |
| Tipo usado em `lampada.dispensada` (`app/ponto/services/cartorio.py:604`, `:610`) | **Não**: duas ausências integrais aprovadas de tipos diferentes podem preservar os agregados da chave (`:325`, `:375`). |
| Catálogo TipoAusencia, incluindo efeito e parcialidade (`app/ponto/catalogo/ausencias.py:249`, `:250`; `app/escala/utils.py:1147`) | **Não diretamente**; somente resultados derivados de cobertura entram (`app/ponto/services/cartorio.py:325`, `:375`). |
| Isenção do colaborador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `app/ponto/services/cartorio.py:1113`) | **Não diretamente**. O veto serializado trata apenas régua degenerada/suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Demissão usada no cadastro do dia (`app/ponto/services/cartorio.py:844`; `app/ponto/supra_juiz.py:169`) | **Não** (`app/ponto/services/cartorio.py:323`). |
| Situação e afastamento atual, controlando emissão (`app/ponto/services/cartorio.py:1033`, `:1340`, `:1381`) | **Não**; são entradas dos efeitos de emissão, não necessariamente do veredito/ata (`:323`). |
| Encerramento e teto por instante (`app/ponto/services/cartorio.py:1049`, `:1056`, `:1117`) | **Sim como agregados**: encerramento e quantidade de vencidos (`:1084`, `:1085`; serialização em `:334`, `:352`). |
| Versão declarada da regra (`app/ponto/services/cartorio.py:890`) | **Sim** (`:342`, `:1086`); isso não equivale automaticamente à versão de qualquer código alterado. |
| Ata/DNA de outras células da janela, usados pelo pré-juiz (`app/escala/utils.py:1252`, `:1519`, `:1524`) | **Não** na chave da célula corrente (`app/ponto/services/cartorio.py:323`, `:1083`). |
| Feriado/localização e precedência | **Não serializados diretamente**. Há, porém, uma diferença importante: `julgar_celula` recalcula a chave `ata.precedencia` **depois** de `julgar_colab`, mesmo que este pule (`app/ponto/services/cartorio.py:1519`, `:1560`, `:1568`). O batch chama `julgar_colab` diretamente (`app/ponto/management/commands/processar_cartorio.py:65`). |
| `TurnoMaterializado` | **Não é entrada direta encontrada**: o carregador de encerramento usa `turnos_do_colab` (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). |

**Escritas e invalidação por `forcar=True`:**

| Escrita da entrada ausente | Força outro julgamento? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | Signal chama cartório **sem force** (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir/apagar (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`, `:1174`) | Signals de save/delete chamam **sem force** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | Receiver encontrado invalida o catálogo, não força julgamento (`app/ponto/models.py:1233`, `:1236`). |
| Registrar/retratar/apagar Batida (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals sem force (`app/ponto/signals.py:131`, `:138`, `:93`), mas presença/tipo/instante normalmente mudam a chave. Escritor produtivo que altere **somente** `is_intervalo`: **nao li**. |
| Flags/demissão/posto de colaborador (`app/colaboradores/services/colaborador.py:32`, `:53`, `:61`, `:68`, `:75`) | Nenhum force nesses setters. O signal de isenção silencia chamados (`app/chamados/signals.py:207`, `:217`); isso pode alterar indiretamente outro componente da chave. |
| Vigência/âncora e FolgaDia (`app/colaboradores/services/vinculo.py:799`, `:845`; `app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional:** regeneração força somente datas tocadas, com `recalculo=True` e filtros de data/via humana (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| Editar TipoEscala (`app/escala/services/cadastro_tipo.py:306`) | Regenera com `recalculo=False` (`:348`, `:350`); não alcança aquele force (`app/ponto/portas/celula.py:359`). |
| Reverter regeneração (`app/ponto/portas/celula.py:537`) | Sem chamada equivalente de force no corpo até `:551`. |
| Gravar ata/veredito (`app/ponto/portas/celula.py:650`, `:671`) | Não chama julgamento forçado nessa operação; decisão administrativa tem preservação própria (`:645`, `:646`). |
| Feriado criar/editar/excluir (`app/core/views_config.py:81`, `:68`, `:198`) | Save agenda reavaliador de chamados, não force do cartório (`app/chamados/signals.py:259`, `:269`); ausência de post_delete declarada em `:255`. |
| Alterar município/localização (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Nenhum force encontrado nessas portas. Outros chamadores: **nao li**. |

Existem forças adicionais no pós-vínculo, segunda passada enfileirada e ação explícita de cobrança; não demonstram cobertura universal das escritas acima (`app/colaboradores/services/pos_vinculo.py:72`; `app/ponto/services/cartorio.py:746`; `app/chamados/services/cobrar_dia.py:29`).

**Podem deixar célula velha — hipóteses quando os demais componentes permanecem iguais:** justificativa decidida; alteração de tipo/conteúdo de ausência sem mudar status/parcial; catálogo de ausência; isenção/demissão; foto de folga; vínculo/template/geradora fora das condições de force; ata de outra célula da janela; primeira batida global fora de `bj`. As omissões e gatilhos estão nas linhas das duas tabelas. Para Feriado/localização, não confundir veredito/lâmpadas com `ata.precedencia`, que tem atualização separada em `julgar_celula` (`app/ponto/services/cartorio.py:1568`).

Completude recursiva de todos os métodos de modelo e chamadores de cadastro: **nao li integralmente**. Ocorrências reais de célula velha: não verificadas.

## T13 — Outros caches com chave incompleta

| Sítio / proteção | Chave e pulo | Entrada ausente / suspeita |
|---|---|---|
| **Memo de `pergunta_viva`** (`app/chamados/juizes.py:778`) | Campos da pergunta, veredito/via da célula e estado do destino (`:767`); igualdade retorna memo (`:791`, `:794`). | O cálculo lê motivo, DNA, lâmpadas e pendências (`:842`, `:849`, `:1051`, `:1064`), ausentes da chave. **Hipótese:** mesma instância reutilizada após alteração de ATA/DNA/motivo mantém resposta antiga. Cache limitado à instância/request (`:788`). |
| **Lavra S2 / seleção de perguntas stale** (`app/chamados/veredito_lavrado.py:173`; `app/chamados/services/raiox.py:31`) | Nunca lavrado, prazo expirado ou timestamps de insumos posteriores ao julgamento (`veredito_lavrado.py:137`, `:179`; `raiox.py:43`). | **Versão do código não entra.** O backfill declara que mudança de regra/código não é detectada por `esta_stale` (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`). Há reparo separado por comparação com juiz atual (`:11`). |
| **`houve_fato_novo`**, gate de renascimento (`app/ponto/services/cartorio.py:249`; `app/chamados/models.py:956`) | Timestamps de Batida, Ausência, Justificativa, célula e marco vencido (`cartorio.py:272`, `:275`, `:279`, `:285`, `:287`). | **Exclusão física não deixa linha consultável. Hipótese:** exclusão posterior à decisão admin, sem outro carimbo observado, pode não ser reconhecida. Feriado/localização/vínculo também dependem de efeitos indiretos nesses carimbos. Não é cache de cálculo; pula renascimento. |

**Reaproveitamentos adjacentes, distintos de pular o cálculo do cartório:**

| Sítio | Chave | Entrada ausente / limite |
|---|---|---|
| Frescor do RetratoFolha (`app/folha/retrato_folha.py:1268`) | Compara `FechamentoMensal.atualizado_em` com `RetratoFolha.lavrado_em` (`:1296`, `:1306`). | Retrato usa retenções dependentes de células/vereditos (`:1157`; `app/folha/export.py:1188`, `:1215`). **Hipótese:** célula muda sem atualizar fechamento e o rótulo continua atualizado. O lavrador não pula montagem por isso (`app/folha/management/commands/lavrar_retrato_folha.py:65`, `:74`). |
| ETag do placar (`app/chamados/services/painel.py:79`) | Máximos de Chamado/Pergunta, filtros, usuário/schema e quantum de120s (`:64`, `:91`); igualdade permite304 (`app/chamados/views.py:822`). | Resposta usa PlacarNavbar e particionamento dependente do fio (`views.py:844`; `painel.py:227`). Célula/ATA e `PlacarNavbar.lavrado_em` não entram diretamente. **Hipótese limitada ao quantum:** fonte muda sem mover os máximos e há304. |

**Procurados sem outro caso de chave incompleta demonstrado:**

- **DiaPago:** monta linhas e faz `delete/bulk_create`; não compara hash de entrada para pular (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`). Hash no comando é prova antes/depois (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:132`).
- **FechamentoMensal:** invalidação por `derivado_em=None`; leitor chama derivação online (`app/ponto/services/fechamento.py:1611`, `:1655`, `:1718`).
- **Lavra de ata/veredito:** evita gravação quando a **saída** já é igual; não evita produzir o resultado (`app/ponto/portas/celula.py:643`, `:661`).
- **Espelho em lote:** hash identifica pedido e só reutiliza job em curso (`app/relatorios/lote_espelho.py:49`, `:98`).
- **Retrato de HE/teto:** apuração chama espelho; lavra usa `update_or_create` (`app/ponto/services/he_pendente_lavrado.py:92`, `:180`).

Foram buscados hashes, impressões, fingerprints, carimbos e condições de reaproveitamento em `app/`. Caches de sessão, snapshots de inteligência e demais memos fora dos sítios acima: **nao li**. Nenhuma hipótese desta seção foi reproduzida.

## T14 — Caminhos para fio morto O261

O contador exige chamado vivo com célula, módulo de furo, célula acusadora e ausência de pergunta viva; ainda exclui hoje/futuro, decisão oferecida ao admin, ausência aberta e aceite de folga validado (`app/chamados/services/vigia_fio.py:184`, `:199`, `:227`, `:258`, `:261`, `:266`, `:273`, `:279`). Só então conta em `:286`.

**L** marca escrita direta na pergunta/vitalidade/veredito equivalente à lâmpada; não significa, por si, violação da regra (`CLAUDE.md:273`, `:276`).

| Caminho | Como pode produzir/conservar fio morto |
|---|---|
| **L — validar pergunta** (`app/chamados/services/validacao.py:131`, `:133`) | `validada_em` mata antes de consultar a célula acusadora (`app/chamados/juizes.py:1006`). **Hipótese:** chamado/célula permanecem e não há outro fio. A porta tenta devolver validação de resposta-hora recusada; não é falha inevitável (`validacao.py:415`, `:418`). |
| **L — carimbar `fato_ocupado`** (`app/chamados/services/materializacao.py:1042`, `:1053`) | Via de identidade materializada mata mesmo com célula acusadora (`app/chamados/juizes.py:1039`, `:1044`), sem fechar chamado/absolver célula nessa operação. **Hipótese**, condicionada aos filtros do contador. |
| **L — `paridade_recusada`** (`app/chamados/services/materializacao.py:858`, `:868`) | Também encerra por identidade (`app/chamados/juizes.py:1042`). Porém exige marco já existente no espelho antes do carimbo (`materializacao.py:840`, `:841`); não prova morte cega. Pergunta previamente validada já estava morta (`:848`). |
| **L — encerrar administrativamente disputa/perguntas** (`app/chamados/models.py:1404`, `:2036`, `:2039`) | A identidade administrativa vence a acusação (`app/chamados/juizes.py:1039`). Signal tenta fechar o pai da disputa, não incondicionalmente todos os chamados-destino (`app/chamados/signals.py:19`, `:42`). **Hipótese:** destino de furo sobrevive. |
| **L — remover motivo apagando perguntas** (`app/chamados/models.py:1519`) | Com motivos restantes, retorna sem fechar disputa/pai nem absolver célula (`:1522`, `:1524`). **Hipótese:** perguntas apagadas eram o único fio daquela célula. O evento de pergunta é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — marcar duplicata** (`app/chamados/services/fio.py:844`, `:848`) | Mata a excedente por identidade (`app/chamados/juizes.py:1040`). **Não produz sozinho** se a canônica permanece viva na mesma célula; a migração escolhe canônica entre vivas (`fio.py:882`, `:890`). |
| **Abrir `batida_ausente` antes de produzir perguntas** (`app/ponto/management/commands/detectar_ausencias.py:688`) | Etapa de fio é posterior e best-effort (`:777`, `:791`, `:792`). Falha ou ausência de órfãos elegíveis não desfaz nascimento. **Hipótese de persistência**, pois cartório/outros eventos ainda podem reparar. |
| **Abrir/reusar `turno_aberto`** (`app/ponto/management/commands/processar_alertas_turno.py:318`) | Perguntas só são tentadas se `criado`, em `try/except` (`:355`, `:367`, `:368`). Reuso não executa essa etapa. **Hipótese**, não consequência inevitável. |
| **Grade emite sem pergunta** (`app/ponto/management/commands/reconciliar_grade.py:498`) | Abre/reabre e retorna sem gerar perguntas (`:519`, `:520`). Caminho manual executável; o cron não passa `--apply` (`app/config/crons.py:436`). |
| **View cria bloqueio de turno** (`app/ponto/views.py:290`) | Ramo cria `entrada_bloqueada_turno_aberto` sem pergunta (`:298`, `:301`). O módulo pertence ao vocabulário (`app/chamados/catalogo/furo_modulos.py:15`), mas O261 ainda exige célula associada e acusadora; ocorrência dessas condições não provada. |
| **Pergunta recusada por ausência de ata legível** (`app/chamados/services/disputa_emissao.py:833`, `:842`) | Chamado pode existir antes; posterior célula acusadora pode encontrá-lo sem fio. **Hipótese**, sujeita à fábrica do julgamento seguinte. |
| **L — célula volta a acusar após pergunta morta** (`app/ponto/services/cartorio.py:1409`; `app/ponto/portas/celula.py:652`, `:671`) | Validação e identidades terminadoras continuam vencendo a acusação (`app/chamados/juizes.py:1006`, `:1044`). Para outras mortes, acusação posterior pode tornar pergunta viva novamente (`:1055`, `:1057`). |
| **Fábrica preserva identidade morta** (`app/chamados/services/fabrica_da_celula.py:93`, `:95`) | Dedup considera perguntas existentes não duplicatas, não apenas vivas. O gate reencontra o fato sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1988`). Pode conservar a falta de fio após relavra acusadora. |

**Reparos existentes têm recortes:**

- Beco por evento exige pai já resolvido, não o chamado vivo do O261 (`app/chamados/services/becos.py:31`).
- Ressuscitação do cartório ignora chamado sem perguntas e perguntas com texto (`app/ponto/services/cartorio.py:1261`, `:1263`).
- `ressuscitar_perguntas` seleciona materializadas sem texto; validada sem materialização não entra (`app/chamados/models.py:1028`, `:1031`).
- Fábrica por evento só alcança célula julgada; impressão igual pode impedir esse caminho (`app/ponto/services/cartorio.py:1092`, `:769`, `:770`).

Não tratar como produtor automático a absolvição por célula: ela exige não acusação/ausência de pendências e pode ser desfeita pela acusação posterior (`app/chamados/reconciliador.py:529`, `:538`; `app/chamados/juizes.py:1055`). Tranca também tenta encerrar chamados, além de matar perguntas (`app/chamados/services/competencia_trancada.py:174`, `:177`).

Todos os escritores fora das portas e caminhos citados: **nao li integralmente**. Ocorrências/quantidades de produção: **nao li**.

## T15 — Emissor de `batida_ausente` pergunta à precedência?

Os nascimentos diretos encontrados são o funil de ausência, o emissor da grade e o reemissor C2 (`app/ponto/management/commands/detectar_ausencias.py:688`; `app/ponto/management/commands/reconciliar_grade.py:498`; `app/chamados/reconciliador.py:710`).

| Caminho | Consulta `fatos_do_dia` antes de criar? | O que efetivamente barra |
|---|---|---|
| `detectar_ausencias.handle` → `_abrir_chamado` (`app/ponto/management/commands/detectar_ausencias.py:439`, `:688`) | **Sim**, mas o resultado só veta `faixa == 'feriado'` (`:615`, `:616`). | Folga depende de `eh_dia_trabalho`/célula (`:143`, `:305`, `:626`); ausência é consultada no chamador (`:346`, `:805`). Não há veto geral de todas as faixas da precedência no funil. |
| Cartório → mesmo funil (`app/ponto/services/cartorio.py:1358`, `:1394`) | **Sim indiretamente**, pelo mesmo `detectar_ausencias.py:616`. | Antes disso usa protestos e ata do julgamento (`cartorio.py:1339`, `:1381`, `:1387`); não substitui o veto por feriado do funil. |
| `emitir_furo_retroativo` → mesmo funil (`app/ponto/management/commands/emitir_furo_retroativo.py:142`) | **Sim indiretamente**. | Exige `FURO_SEM_COBRANCA`, vínculo e trabalho previsto (`:109`, `:118`, `:127`), depois executa os vetos do funil. |
| `reconciliar_grade._emitir_furo` (`app/ponto/management/commands/reconciliar_grade.py:498`) | **Não nesse emissor.** | Régua e veto parcial 

[truncado em 20000 de 22228 caracteres]

## 29/10 07:58 · sessao 6da5510d7ea7

T12 — leitura do commit `b3bf529e` por `git show`, sem execução/testes/banco.

**Chave e pulo.** `impressao_insumos` serializa Batida `(pk,tipo,timestamp)`, `cob.status`, Chamado `(pk,status_local)`, DNA `(dna_versao,marcos)`, veto, número de vencidos, versão da regra, encerramento, quantidade de chamados com fio e estado da parcial (`app/ponto/services/cartorio.py:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). O ramo normal passa esses agregados em `:1083`; pula em `:1092`. O ramo sem adesão passa menos entradas em `:941` e pula em `:943`.

| Entrada lida para veredito/ata | Presença na impressão |
|---|---|
| Batidas apuráveis da janela: tipo, instante, presença/ausência da linha (`cartorio.py:871`; grade `app/escala/utils.py:1267`) | **Sim** para `(pk,tipo,timestamp)` de `bj`, a janela por célula (`cartorio.py:1042`, `:324`). Retratação altera o universo apurável (`:871`). |
| `Batida.is_intervalo` usado na grade (`app/escala/utils.py:1267`) e pareador (`app/ponto/turnos.py:1261`) | **Não**: a tupla da chave não contém essa flag (`cartorio.py:324`). |
| Primeira batida apurável da vida define `adesao` (`cartorio.py:899`, `:901`) | **Não diretamente**. O hash usa `bj`, não a primeira global (`:1042`, `:1083`). Há exceção explícita: veredito `nunca_bateu` não é pulado no ramo normal (`:1092`), mas o ramo sem adesão tem pulo próprio (`:943`). |
| Chamados de batida e seus estados (`cartorio.py:1004`, `:1040`, `:1069`) | **Sim**, identidade/estado de `lista` (`:326`, `:1083`). |
| Perguntas vivas que formam `_com_fio` (`cartorio.py:1074`) | **Sim como quantidade** de chamados com fio (`:1087`, `:367`), não conteúdo/identidade das perguntas. O classificador recebe essa quantidade (`:1110`). |
| DNA.marcos e dna_versao; pausas e bordas extraídas desses marcos (`app/escala/utils.py:53`, `:104`, `:1547`) | **Sim**, para o DNA da célula corrente (`cartorio.py:327`). DNA/ata de outras células da janela não são serializados nessa chave (`utils.py:1252`, `:1524`; `cartorio.py:1083`). |
| Célula.trabalha e escala_geradora/vínculo do dia, que decidem o dia previsto (`app/escala/utils.py:1252`, `:1255`, `:1545`; `app/ponto/supra_juiz.py:136`, `:171`) | **Não diretamente**: `cartorio.py:327` pega somente versão/marcos do DNA. |
| Demais dados de vínculo/template: vigência, ativa, âncora, tipo_ciclo, intervalo livre, régua-união e defeito de folga (`app/escala/utils.py:1240`, `:1300`, `:1313`, `:1315`; `app/ponto/supra_juiz.py:175`) | **Parcial**: bordas/pausas congeladas entram pelo DNA; vínculo/geradora/flags/template vivo e defeito não são serializados (`cartorio.py:327`). |
| Foto FolgaDia sobrepõe tipo e apaga faltantes/órfãs (`app/escala/utils.py:1109`, `:1118`, `:1119`) | **Não diretamente** (`cartorio.py:323`). Pode coincidir com mudança de DNA/regeneração, mas isso não integra a FolgaDia à chave. |
| Justificativa aprovada: consulta por data, limpa faltantes/órfãs e marca `justificado` (`app/escala/utils.py:1157`, `:1200`, `:1202`, `:1203`) | **Não** (`cartorio.py:323`). A dispensa lê esse flag para mudar a ata (`:602`, `:608`, `:610`). |
| Ausência da cobertura de **cobrança**, inclusive estados não aprovados (`cartorio.py:1041`) | **Parcial**: só `status` da escolhida (`:325`). Identidade, tipo, datas, minutos e horas não entram diretamente. |
| Ausência aprovada da cobertura de **folha** define tipo `falta/ausencia`, faltantes e órfãs (`app/escala/utils.py:1148`, `:1173`, `:1180`, `:1195`) | **Parcial**: chave recebe status da cobertura de cobrança e parcial, não a linha de folha nem seu tipo (`cartorio.py:1083`, `:1088`). |
| Integralidade da cobertura e relação das batidas com período parcial (`app/escala/utils.py:1190`, `:1194`) | **Sim como resultado comprimido**: bool/`sem_periodo`/`dentro`/`fora` (`cartorio.py:1079`, `:1088`, `:375`). Horas/minutos exatos não entram. |
| Tipo da ausência usado para `dispensada` na ata (`cartorio.py:604`, `:610`) | **Não**: aprovado tipo A→tipo B pode manter status/parcial iguais (`:325`, `:375`). |
| Catálogo TipoAusencia: ativo, efeito, parcial, fim etc. (`app/ponto/catalogo/ausencias.py:249`, `:250`; uso do conjunto parcial em `app/escala/utils.py:1147`) | **Não diretamente**; só os resultados status/parcial mencionados (`cartorio.py:325`, `:375`). |
| Isenção da pessoa muda grade e classificador (`app/escala/utils.py:865`; `app/ponto/supra_juiz.py:195`; `cartorio.py:1113`) | **Não diretamente** (`cartorio.py:323`). O veto da chave não cobre isenção: só degeneração/régua suspeita (`app/ponto/services/furos_vetados.py:19`, `:28`, `:30`). |
| Colaborador.data_demissao entra na classificação de vínculo ausente (`cartorio.py:844`; `app/ponto/supra_juiz.py:169`, `:171`) | **Não** (`cartorio.py:323`). |
| Colaborador.situacao e afastamento atual controlam emissão, não a assinatura do classificador (`cartorio.py:1033`, `:1340`, `:1381`) | **Não** (`cartorio.py:323`); são entradas dos efeitos do julgamento, não comprovadas como produtoras diretas de veredito/ata. |
| Encerramento e instante/teto (`cartorio.py:1049`, `:1056`, `:1082`, `:1117`) | **Sim como agregados:** `enc=0` quando em curso e quantidade de vencidos (`:1084`, `:1085`; serialização `:334`, `:352`). |
| Versão da regra (`cartorio.py:890`) | **Sim**, resultado de `versao_para` (`:1086`, `:342`), não automaticamente qualquer mudança de código. |
| Ata prévia da janela fornece papel ao pré-juiz (`app/escala/utils.py:1519`, `:1524`) | **Não** (`cartorio.py:323`). O builder depois também usa atas em memória da nova montagem (`utils.py:1824`, `:1835`); isso não elimina a leitura inicial. |
| `TurnoMaterializado` | **Não é entrada direta encontrada nesse julgamento.** `dias_encerrados` carrega `turnos_do_colab`, não TurnoMaterializado (`app/ponto/turnos.py:2011`, `:2013`, `:2018`). Não atribuir a esse modelo uma omissão da chave sem outro caminho. |
| Feriado/folga por calendário/local | Folga e previsto chegam por `eh_dia_trabalho`/foto (`app/escala/utils.py:1545`, `:1109`); **não há feriado/local serializado** (`cartorio.py:323`). Predicado completo dentro de todos os métodos de `eh_dia_trabalho`: **nao li integralmente**. |

**Escritas das entradas ausentes e força do julgamento:**

| Escrita | Dispara `forcar=True`? |
|---|---|
| Justificativa aprovar/rejeitar/materializar (`app/ponto/services/justificativa.py:88`, `:170`, `:265`) | **Não no signal:** `post_save` → `_julgar_dias` → `julgar_celula` sem force (`app/ponto/signals.py:161`, `:165`, `:93`). |
| Ausência criar/editar/decidir (`app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`) | **Não no signal save/delete** (`app/ponto/signals.py:143`, `:150`, `:93`). |
| TipoAusencia salvar/criar (`app/ponto/services/ausencia.py:1292`, `:1294`) | **Não no receiver encontrado:** apenas invalida catálogo (`app/ponto/models.py:1233`, `:1236`). |
| Batida registrar/retratar/apagar (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Signals julgam **sem force** (`app/ponto/signals.py:131`, `:138`, `:93`). Criação/retratação normalmente mudam a tupla/conjunto da chave; não são por si prova de cache velho. Escritor que altere apenas `is_intervalo` em linha existente: **nao li**. |
| Flags/demissão/posto de Colaborador, portas `gravar/salvar/update` (`app/colaboradores/services/colaborador.py:32`, `:34`, `:53`, `:61`, `:68`, `:75`) | **Nenhum force encontrado nesses setters.** Signal de isento apenas silencia chamados (`app/chamados/signals.py:207`, `:217`), não força cartório. Fluxos completos de todos os chamadores: **nao li**. |
| Vínculo vigência/âncora (`app/colaboradores/services/vinculo.py:799`, `:845`) | **Condicional:** signals regeneram (`app/escala/signals.py:71`, `:117`); regeneração força só `_tocadas`, com `recalculo=True`, dias não futuros/não humanos (`app/ponto/portas/celula.py:359`, `:368`, `:371`). |
| FolgaDia materializar/lancar/remover (`app/escala/folgas.py:172`, `:259`, `:289`) | **Condicional pela regeneração**, mesma restrição acima (`folgas.py:192`, `:261`, `:303`; `celula.py:359`, `:371`). |
| TipoEscala editar (`app/escala/services/cadastro_tipo.py:306`) | Regenera com **`recalculo=False`** (`:348`, `:350`), portanto não alcança o force de `celula.py:359`. |
| CelulaDia regenerar/metadados; reverter (`app/ponto/portas/celula.py:232`, `:246`, `:263`, `:537`) | Regeneração tem force condicional (`:359`, `:371`); reversão não tem chamada equivalente no corpo até `:551`. |
| Ata/veredito manual (`app/ponto/portas/celula.py:621`, `:650`, `:671`) | Não chama cartório com force no corpo; via admin inclusive prevalece sobre veredito do cartório (`:645`, `:646`). |
| Feriado criar/editar/delete (`app/core/views_config.py:81`, `:68`, `:198`) | `post_save` agenda reavaliador de chamados, **não julgamento forçado** (`app/chamados/signals.py:259`, `:269`); ausência de post_delete é declarada (`:255`). Eventual fechamento muda status e acorda cartório sem force (`chamados/signals.py:174`). |
| Município/posto altera alcance (`app/colaboradores/services/estrutura.py:50`, `:70`; `app/colaboradores/management/commands/seed_municipios.py:38`) | Force de cartório não encontrado nessas portas; outros chamadores: **nao li**. |

**Forças que existem, mas não invalidam universalmente cada entrada:** pós-vínculo (`app/colaboradores/services/pos_vinculo.py:72`), segunda passada apenas em células enfileiradas para rejulgar (`cartorio.py:745`, `:746`), ação explícita cobrar dia (`app/chamados/services/cobrar_dia.py:29`) e reparo por porta de furo sem cobrança (`app/ponto/services/furos_sem_cobranca.py:128`). Não provar cobertura de uma escrita apenas porque existe um force em outro caminho.

**Podem deixar célula velha — hipóteses estáticas, quando os agregados da chave permanecem iguais:**

- Justificativa aprovada/desaprovada, sem mudar os outros componentes: muda limpeza/dispensa mas signal não força (`utils.py:1157`, `:1203`; `cartorio.py:602`; `signals.py:93`).
- Ausência aprovada de dia inteiro trocada, por exemplo `atestado→falta`: muda tipo do dia/`dispensada`, mantendo `status='aprovada'` e parcial falsa (`ausencia.py:497`, `:508`; `utils.py:1173`, `:1180`; `cartorio.py:604`, `:325`).
- Isenção/demissão, foto de folga, metadados da célula/geradora, template/vínculo fora de force condicional, catálogo de ausência e ata da janela: são lidos e não têm representação completa na chave nas linhas acima.
- Primeira batida global fora de `bj` muda adesão sem mover a tupla local (`cartorio.py:899`, `:1042`); a exceção `nunca_bateu` reduz uma faixa, não codifica a data de adesão (`:1092`).
- Alteração isolada de `is_intervalo` seria candidata, mas porta produtiva dessa alteração isolada: **nao li**.

Ocorrências/quantidades reais dessas hipóteses: **não provado**, sem banco/teste. Completude recursiva de todos os métodos de modelo e todos os chamadores de cadastro: **nao li integralmente**. [nao verificado]

## 29/10 07:58 · sessao 318ef9b17cef

T13 — fonte `git show b3bf529e:app/...`; somente leitura.

**Candidatos encontrados; “hipótese” abaixo significa chave incompleta demonstrada por leitura, sem demonstrar ocorrência em produção.**

| Sítio / o que protege | Chave ou condição de reaproveitamento | Entrada lida que não entra / conclusão |
|---|---|---|
| **Memo de `pergunta_viva`**, `app/chamados/juizes.py:778` | `_chave_memo_pv` contém campos da pergunta, `cel.veredito`, `cel.veredito_via` e estado do destino (`:767`–`:775`); chave igual retorna o resultado anterior (`:791`–`:794`). | O cálculo lê **ATA/DNA/motivo**: `fato_do_marco_no_espelho` usa motivo (`:842`), DNA (`:849`) e lâmpadas; `_pergunta_viva_calc` o chama (`:1051`) e também lê marcos pendentes da ata (`:1064`). ATA/DNA/motivo não estão na chave. **Hipótese:** mesma instância reutilizada após mudar lâmpada/pendência, mantendo veredito/via, reutiliza resposta velha. Limitado à vida da instância/request, não cache persistente (`:788`). |
| **Lavra S2 de materialização**, `app/chamados/veredito_lavrado.py:173`; seleção do cron em `app/chamados/services/raiox.py:31`–`:45` | Nunca lavrado, prazo expirado, ou máximo de timestamps de Batida, Célula e Pergunta posterior a `julgado_em` (`veredito_lavrado.py:137`–`:169`, `:179`–`:190`). Quem não satisfaz isso não entra no lote (`raiox.py:43`–`:46`). | **Versão do código do juiz não entra.** O próprio backfill declara que mudança de lei/código deixa campo velho sem `esta_stale` perceber (`app/chamados/management/commands/backfill_veredito_lavrado.py:5`–`:9`). Há reparo separado por comparação com juiz atual (`:11`–`:13`), não invalidação automática por versão. Lacuna declarada no código, não ocorrência nova medida. |
| **`houve_fato_novo`**, `app/ponto/services/cartorio.py:249`; impede reabertura administrativa em `app/chamados/models.py:956` | Procura timestamps em linhas **ainda existentes** de Batida (`cartorio.py:272`), Ausência (`:275`), Justificativa (`:279`), regeneração/edição de Célula (`:285`) e marco recém-vencido (`:287`). | **Exclusão física** de uma linha não deixa timestamp nessa consulta. **Hipótese:** apagar testemunha após decisão admin, sem outro fato/carimbo novo, pode ser mudança real não reconhecida. A mesma função também não consulta Feriado/localização/vínculo diretamente; exige que outra escrita deixe um dos carimbos observados (`:272`–`:287`). Não é hash de recálculo: é gate que pula renascimento e também alimenta o vigia de loop (`app/chamados/services/vigia_loop.py:50`). |
| **Frescor do RetratoFolha**, `app/folha/retrato_folha.py:1268` | “Atualizado” se não houver `FechamentoMensal.atualizado_em` maior que `RetratoFolha.lavrado_em` (`:1296`–`:1306`). | Retrato deriva de `fora_da_folha(...com_itens=True)` (`:1157`), cuja retenção usa células e seus vereditos (`app/folha/export.py:1188`, `:1215`). **Celula.veredito/ata não entram diretamente no comparador de frescor. Hipótese:** célula muda sem atualizar fechamento e retrato segue rotulado atualizado. **Não é pulo do lavrador:** o comando sempre monta linhas (`app/folha/management/commands/lavrar_retrato_folha.py:65`) e, com apply, regrava (`:74`–`:76`). |
| **ETag do placar**, `app/chamados/services/painel.py:79`; retorno304 em `app/chamados/views.py:822`–`:826` | Máximos de timestamps de Chamado/Pergunta (`painel.py:64`–`:68`), filtros, quantum120s e extra schema/usuário (`:91`; `views.py:820`). | Não contém `Celula.veredito/ata` nem `PlacarNavbar.lavrado_em`. Entretanto a resposta sem filtro lê o payload desse PlacarNavbar (`views.py:844`–`:850`); com filtro chama o particionamento (`painel.py:227`–`:228`), que depende da pergunta viva. **Hipótese limitada ao mesmo quantum:** fonte muda sem alterar máximos da chave, mas o cliente recebe304. O quantum de120s é deliberado (`painel.py:47`, `:76`, `:87`). |

**Lugares procurados em que não encontrei outro cache de entrada que pule cálculo:**

- **DiaPago:** `lavrar` monta linhas e faz `delete + bulk_create`, não compara hash de insumos (`app/ponto/services/dia_pago.py:333`, `:343`, `:357`–`:358`). O hash de Fechamento no comando é **prova antes/depois**, não decisão para pular cálculo (`app/ponto/management/commands/lavrar_dias_pagos.py:91`, `:106`, `:132`–`:139`).
- **FechamentoMensal:** há invalidação por `derivado_em=None` (`app/ponto/services/fechamento.py:1611`–`:1615`) e vigia desse campo (`:1655`), mas `fechamento_lido` chama derivação online (`:1718`). Não identifiquei nesses trechos uma chave de hash que evite o recálculo.
- **Lavra de veredito/ata:** a porta só evita **gravação** quando a saída inteira é igual, incluindo ata (`app/ponto/portas/celula.py:643`, `:661`–`:662`); não pula a produção da saída. Não classificar esse no-op como outro cache de insumos.
- **Espelho em lote:** hash é identidade do **pedido** (`app/relatorios/lote_espelho.py:49`–`:63`); só reutiliza job **em curso** (`:98`–`:100`), não PDF concluído por chave de dados.
- **Retrato de HE/teto:** `apurar` chama o espelho por colaborador (`app/ponto/services/he_pendente_lavrado.py:92`); `lavrar` usa `update_or_create` (`:180`); leitura devolve foto e horário (`:201`–`:204`). Não encontrei ali hash que pule apuração.
- **Impressão do cartório:** `app/ponto/services/cartorio.py:943`, `:1092`; pertence ao T12, não duplicado aqui.

Buscas feitas em Python de `app/` por `hashlib`, `fingerprint`, `impressao`, `houve_fato_novo`, `julgada_em`, `lavrada_em`, `derivado_em`, `cache.get/set` e condições de reaproveitamento. Não li integralmente todos os módulos encontrados; caches de sessão, snapshots de inteligência e demais memos locais fora dos sítios acima: **nao li**.

## 29/10 07:57 · sessao 47e48220e190

T14 — fonte `b3bf529e`, lida por `git show`; sem testes ou banco.

**O estado contado exige todas as condições**, não apenas ausência de pergunta: chamado `VIVOS` com célula (`app/chamados/services/vigia_fio.py:184`), módulo de furo (`:199`), veredito acusador (`:226`, `:227`), nenhuma pergunta viva (`:258`), dia passado (`:261`), sem ação do dia oferecida ao admin (`:266`), sem Ausência aberta (`:273`) e sem aceite de folga validado (`:279`). Só então incrementa `fio_morto` (`:286`).

`L` abaixo significa que o caminho mexe diretamente no fio equivalente à lâmpada — pergunta, sua vitalidade ou veredito da célula — à luz de `CLAUDE.md:273`, `:274`, `:276`. Não significa que todo ato listado seja violação.

| Caminho | Por que pode deixar fio morto / guarda existente |
|---|---|
| **L — validação da pergunta:** `app/chamados/services/validacao.py:131`, `:133`, chamada em `:389` | `validada_em != None` mata a pergunta **antes de consultar a célula acusadora** (`app/chamados/juizes.py:1006`, `:1007`). O código reconhece que uma validada sem gravação não volta ao contador apenas por retirar uma via (`app/chamados/services/materializacao.py:848`, `:852`). **Condicional:** se o chamado continua vivo, célula acusa e não há outra pergunta/ação válida, satisfaz O261. A porta atual tenta devolver a validação de resposta-hora recusada (`validacao.py:415`, `:418`; devolução em `:182`, `:188`, `:190`); não afirmar que toda recusa deixa fio morto. |
| **L — recusa terminal `fato_ocupado`:** `app/chamados/services/materializacao.py:1042`, `:1044`, `:1053` | Carimba `materializada_em` e via de identidade, sem fechar chamado ou alterar célula nessa função. A identidade vence célula acusadora (`app/chamados/juizes.py:1039`, `:1044`). **Hipótese de O261**, condicionada aos demais filtros e à inexistência de outra pergunta viva. |
| **L — recusa `paridade_recusada`:** `app/chamados/services/materializacao.py:858`, `:859`, `:868` | Também via de identidade (`app/chamados/juizes.py:1042`, `:1044`). **Não é prova de morte cega no HEAD:** antes de carimbar, exige que o marco esteja no espelho (`materializacao.py:840`, `:841`); caso contrário registra recusa sem essa via (`:853`). A própria função ressalta que pergunta validada já estava morta pelo degrau 0 (`:848`, `:850`). |
| **L — fechamento administrativo da disputa:** `app/chamados/models.py:1404`, `:1407` → `fechar_por_admin:2036`, `:2039` | A via administrativa mata por identidade, mesmo que célula ainda acuse (`app/chamados/juizes.py:1039`, `:1044`). O fechamento da disputa tenta fechar apenas o **chamado-pai**, pelo signal (`app/chamados/signals.py:19`, `:42`); não é fechamento incondicional de todos os chamados de furo apontados pelas perguntas. **Hipótese de sobrevivência de chamado-destino**; o gate pode recusar fechamento de dia ainda furado (`app/chamados/models.py:470`, `:495`). |
| **L — apagar perguntas ao remover motivo:** `app/chamados/models.py:1519` | Se restam motivos, salva e retorna sem fechar a disputa (`:1522`, `:1523`, `:1524`), sem absolver célula nesse ato. Chamadores lidos: `app/colaboradores/signals.py:77`, `app/escala/signals.py:161`. **Hipótese:** se eram o único fio da célula com chamado de furo sobrevivente, produz ausência de perguntas. O evento de beco é `post_save`, não esse delete (`app/chamados/signals.py:185`). |
| **L — dedup marca pergunta `duplicata`:** `app/chamados/services/fio.py:844`, `:848` | Via de identidade mata a excedente (`app/chamados/juizes.py:1040`, `:1044`). **Não produz fio morto sozinho quando a canônica permanece viva na mesma célula:** migração escolhe canônica entre vivas (`fio.py:882`, `:884`, `:890`). O261 requer que a canônica também não satisfaça o fio daquela célula (`vigia_fio.py:242`, `:258`). |
| **Nascimento antes da pergunta:** `app/ponto/management/commands/detectar_ausencias.py:688` | Persiste chamado antes da etapa de fio, que é best-effort (`:777`, `:780`, `:791`, `:792`). Falha ou conjunto `_orfaos` vazio não desfaz nascimento. **Hipótese de O261 persistente**, se o signal/cartório não o resolver e célula acusadora continuar. |
| **Nascimento `turno_aberto`:** `app/ponto/management/commands/processar_alertas_turno.py:318` | Perguntas vêm depois, somente se `criado`, também em `try/except` (`:355`, `:367`, `:368`). Reuso/renascimento não executa essa etapa do emissor. **Hipótese:** chamado sobrevivente pode estar sem pergunta; não é consequência inevitável porque `reabrir` e cartório têm reparadores. |
| **Emissão da grade sem pergunta:** `app/ponto/management/commands/reconciliar_grade.py:498` | Depois de abrir/reabrir, retorna sem gerar perguntas (`:518`, `:519`, `:520`, `:521`). O cron deste HEAD não passa `--apply` (`app/config/crons.py:436`); trata-se do caminho executável manual, não prova de ocorrência pelo cron. |
| **View cria bloqueio de turno:** `app/ponto/views.py:290` | Cria `entrada_bloqueada_turno_aberto` (`:298`) sem criação de pergunta naquele ramo (`:274`, `:301`). Esse módulo pertence ao vocabulário de furo (`app/chamados/catalogo/furo_modulos.py:15`). Sem célula associada não entra no vigia (`vigia_fio.py:184`); associação e célula acusadora são condições adicionais, **não provadas como ocorrência**. |
| **Pergunta não nasce porque falta ata:** `app/chamados/services/disputa_emissao.py:833`, `:842`, `:843` | Com vínculo e sem célula/ata legível, `_ata_do_dia` retorna impedimento. O chamado já pode existir; a fábrica recusa perguntar. Posterior julgamento acusador pode encontrar esse chamado ainda sem fio. **Hipótese**, sujeita ao reparo no julgamento seguinte. |
| **L — célula volta a acusar depois de pergunta morta:** `app/ponto/services/cartorio.py:1409` → `app/ponto/portas/celula.py:652`, `:666`, `:671` | A relavra pode gravar veredito acusador enquanto pergunta continua validada ou encerrada por identidade; esses dois degraus vencem a acusação (`app/chamados/juizes.py:1006`, `:1044`). **Não vale para toda via morta:** sem os degraus anteriores, célula acusadora devolve `True` à pergunta (`:1055`, `:1057`). |
| **Rejulgamento não fabrica substituta para identidade morta:** `app/chamados/services/fabrica_da_celula.py:93`, `:95` | Dedup da fábrica considera qualquer pergunta existente não duplicata, não apenas vivas. `PerguntaDisputa.abrir` reencontra o fato e retorna sem limpar validação/materialização (`app/chamados/models.py:1966`, `:1967`, `:1988`, `:1989`). Isso pode conservar o estado anterior após a célula voltar a acusar. |

**Por que os reparadores existentes não provam ausência de fio morto:**

- O evento de pergunta tenta fechar **beco cujo pai já está resolvido** (`app/chamados/services/becos.py:31`, `:69`); ele não fecha o chamado vivo que caracteriza O261.
- A ressuscitação do cartório ignora chamados sem perguntas e os com fio já vivo (`app/ponto/services/cartorio.py:1260`, `:1261`), e ignora perguntas com texto (`:1263`). Não é reparador geral de todos os mortos.
- `ressuscitar_perguntas` só alcança materializadas sem texto (`app/chamados/models.py:1028`, `:1029`, `:1031`) e poupa marco já aceso (`:1058`); depois limpa validação/materialização (`:1067`). Pergunta validada sem materialização não entra nesse conjunto.
- Fábrica por evento só pergunta após célula efetivamente julgada (`app/ponto/services/cartorio.py:759`, `:769`, `:770`); pulo por impressão igual impede alcançar esse ponto (`:1092`).

**Caminhos lidos que não devem ser vendidos como produtores automáticos:**

- Absolvição por célula já exige veredito não acusador e ausência de marcos pendentes (`app/chamados/reconciliador.py:529`, `:538`); grava `celula_absolveu` (`:550`), mas essa via não é identidade irrevogável em `pergunta_viva` (`app/chamados/juizes.py:1039`). Acusação posterior pode reativar a pergunta (`:1055`).
- Decisão humana pela célula fecha perguntas **e tenta fechar chamados**, e grava veredito administrativo não acusador (`app/ponto/services/veredito_celula.py:177`, `:182`, `:183`, `:329`; `app/escala/models.py:1727`). Não prova O261 residual.
- Tranca mata a pergunta por leitura (`app/chamados/juizes.py:1010`), mas o fluxo de tranca também encerra chamados (`app/chamados/services/competencia_trancada.py:174`, `:177`); só a leitura não prova sobrevivência.

Completude de todos os escritores de perguntas fora das portas e caminhos citados: **nao li integralmente**. Ocorrências e quantidades de produção: **nao li**.

## 29/10 07:55 · sessao 47e48220e190

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 07:55 · sessao 47e48220e190

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 07:55 · sessao 47e48220e190

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 07:55 · sessao 47e48220e190

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 07:55 · sessao 318ef9b17cef

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 07:55 · sessao 318ef9b17cef

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 07:55 · sessao 318ef9b17cef

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 07:55 · sessao 318ef9b17cef

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 07:55 · sessao 6da5510d7ea7

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 07:55 · sessao 6da5510d7ea7

## T6 — Quem abre ou reabre chamado de turno

Referências desta seção: árvore `app/`, HEAD informado `1974dbc0`. **Consultar a célula não significa exigir veredito acusador:** `celula_veta_cobranca` lê o veredito, mas sua lista de vetos não contém `concorde` (`app/ponto/marcos.py:119`, `:155`).

| Arquivo:linha | Gatilho / quem chama | Premissa testada | Consulta veredito/ata antes de abrir? |
|---|---|---|---|
| `app/ponto/management/commands/processar_alertas_turno.py:318` | Cron (`app/config/crons.py:338`); chamada em `:144` | Turno materializado aberto, entrada existente, ausência não cobrindo, alerta `pos_20`, HE não permitida (`:88`, `:92`, `:97`, `:135`, `:143`). | **Não** como condição de absolvição; usa turno/fim previsto (`:100`, `:118`). Abre `turno_aberto` (`:320`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Cron (`app/config/crons.py:339`) | Recomputação, horas sem saída, nível N5, nenhum chamado reutilizável, ausência não cobrindo (`:57`, `:63`, `:94`, `:114`). | **Não** ao veredito/ata como veto; abre `turno_aberto_24h` (`:123`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138` | Comando manual | Turno aberto, idade mínima, vínculo, ausência não cobrindo, nenhum irmão (`:92`, `:101`, `:104`, `:108`, `:118`). | **Sim, indiretamente:** alimenta `turnos_do_colab` com células (`:88`); não exige veredito acusador. |
| `app/ponto/management/commands/detectar_ausencias.py:688` | Cron (`app/config/crons.py:340`), `handle:439`, cartório e comando retroativo | Ausência de entrada/chamado (`:423`, `:424`); vetos de régua, feriado, célula e existência de célula (`:600`, `:604`, `:616`, `:623`, `:626`, `:655`). | **Sim, veto parcial:** `:626` → `app/ponto/marcos.py:155`. **`concorde` passa pelo veto** (`marcos.py:119`). |
| `app/ponto/services/cartorio.py:1351`, `:1387` | Serviço: signals (`app/ponto/signals.py:93`, `app/chamados/signals.py:174`) e cron (`app/config/crons.py:583`) | Protestos cobradores, horizonte, ausência de chamado vivo; ou furo parcial (`:1312`, `:1320`, `:1332`, `:1374`, `:1386`). | **Sim:** ata deste julgamento (`:1141`), marcos parciais (`:1380`); delega ao funil de `detectar_ausencias`. |
| `app/ponto/management/commands/emitir_furo_retroativo.py:142` | Comando manual | Alvos selecionados como furos; só executa com `apply_` (`:139`, `:141`). | **Sim pelo funil delegado**, em `detectar_ausencias.py:626`; mesma lacuna para `concorde`. |
| `app/ponto/management/commands/reconciliar_grade.py:498` | Comando, chamado em `:253`; cron configurado **sem `--apply`** (`app/config/crons.py:436`) | Marcos faltantes da grade, veto de régua e de célula (`:490`, `:493`). | **Sim, veto parcial** de `app/ponto/marcos.py:119`. Se encontra encerrado, também chama `reabrir` (`:519`). |
| `app/chamados/reconciliador.py:710` | `reemitir_chamado_do_dia`, chamado em `:984` | Motivo C1, ativo, horizonte, isenção, ausência, protestos cobradores e veto da célula (`:643`, `:654`, `:674`, `:684`, `:686`, `:699`, `:703`). | **Sim:** protestos e veredito; exige protesto cobrador para nascer/renascer (`:701`). |
| `app/ponto/views.py:290` | View `bater_ponto` | POST de entrada, bloqueio por turno, nenhuma regularização pendente, nenhum chamado vivo (`:274`, `:280`). | **Não** como veto de absolvição; contexto deriva do pareamento/idade em `app/ponto/services/triagem_batida.py:178`, `:181`, `:206`. |
| `app/ponto/services/cluster_espurio.py:84` | Evento do cartório (`app/ponto/services/cartorio.py:760`) e comando detector | Assinatura de cluster: ≥2 órfãs próximas e ≥1 lâmpada apagada (`:26`, `:27`, `:28`, `:30`). | **Sim, ata:** leitura em `:22`, `:23`. |
| `app/ponto/management/commands/detectar_par_relampago.py:367` | Cron (`app/config/crons.py:572`) | Par relâmpago completo, não retratado, execução não limitada a retratar (`:351`, `:352`, `:361`, `:365`). | **Não como veto de absolvição**; a premissa é o par. |
| `app/ponto/services/triagem_batida.py:336` | View de solicitação de anuência (`app/ponto/views.py:341`) | Motivo informado e nenhum chamado vivo (`:323`, `:325`). | **Não**; abre `batida_fora_horario` (`:344`). |
| `app/ponto/services/triagem_batida.py:459` | Serviço de batida, chamado em `:663` | Fora da janela prevista e dedup de 14h (`:421`, `:428`, `:433`, `:452`). | **Não como veto de absolvição**; janela prevista não é veredito. |
| `app/api/views.py:746`; `app/api/views_core.py:746` | API de batida | Detector retorna desvio leve (`:730`, `:734`). | **Não como veto de absolvição**; cria auditoria `batida_fora_escala_leve` (`:754`). |
| `app/api/views.py:2033` | API de justificativas | Dados obrigatórios e nenhum chamado de turno vivo (`:2011`, `:2022`, `:2032`). | **Não**; cria wrapper `justificativa_colab` (`:2036`). |
| `app/ponto/services/justificativa.py:174` | Rejeição pelo DP, chamada em `app/ponto/views.py:1911` | Justificativa pendente encontrada e rejeitada (`:160`, `:166`). | **Não**; chamado de rejeição, módulo `justificativa` (`:182`). |
| `app/chamados/services/regularizacao_ext.py:246` | View (`app/colaboradores/views.py:2983`) e serviço (`regularizacao_ext.py:318`) | Pedido de regularização com itens (`:236`, `:240`). | **Não**; cria container de confirmação (`:250`). |
| `app/chamados/services/disputa_emissao.py:504` | `_abrir_ou_agregar_disputa`; wrappers em `:563`, `:598` | Nenhuma disputa aberta encontrada (`:467`, `:502`). | **Não no nascimento do pai**; recebe a premissa do chamador. O caminho da fábrica consulta veredito/ata antes (`app/chamados/services/fabrica_da_celula.py:81`, `:95`). |
| `app/chamados/services/disputa_emissao.py:908` | Serviço de chamados órfãos; chamado, entre outros, pelo cron em `app/ponto/management/commands/processar_alertas_turno.py:367` | Chamados regularizáveis e nenhuma disputa existente (`:891`, `:907`). | **Sim no filtro de marcos:** `_ata_do_dia` em `:880`; não exige veredito acusador do pai. |
| `app/chamados/services/disputa_emissao.py:1128` | Confirmação de âncora, chamada em `app/chamados/services/acoes_disputa.py:189` | `ancora_limbo` não encerrado, sem pergunta correspondente e sem disputa (`:1109`, `:1111`, `:1114`, `:1127`). | **Não no nascimento do pai**. |

**Portas e reaberturas:**

| Arquivo:linha | Caminho / premissa | Consulta antes? |
|---|---|---|
| `app/chamados/models.py:723`, `:813` | Nascimento por `abrir`/`criar`; `_celula_de` apenas resolve a associação (`:833`). | **Não há guarda geral de absolvição no nascimento**; associar célula em `:804` não testa seu veredito. |
| `app/chamados/models.py:685` → `:756` | Dedup encontra encerrado; `_renascer_por_premissa_viva` exige ativo, não cascade e ausência de tranca (`:750`, `:752`, `:754`). | **Sim**, delegando a `reabrir`. |
| `app/chamados/models.py:962` | `reabrir` recusa furo cuja premissa está regularizada. | **Sim:** célula atual, decisão administrativa, marcos acesos/inexistentes e furo da ata (`:889`, `:891`, `:897`, `:906`, `:908`). |
| `app/chamados/views_arquivar.py:68`; `app/chamados/services/competencia_trancada.py:258` | Desarquivamento/reabertura de competência (`views_arquivar.py:64`; `competencia_trancada.py:252`, `:255`). | **Sim pela mesma guarda de `reabrir`**. |
| `app/chamados/services/acoes_disputa.py:266`; `app/chamados/services/disputa_emissao.py:1606` | Reabertura administrativa da disputa; ou resposta aguardando consumo (`acoes_disputa.py:256`, `:265`; `disputa_emissao.py:1604`). | **Sim pela mesma guarda de `reabrir`**. |
| **`app/chamados/services/acoes_chamado.py:315`** | View chama resposta livre (`app/chamados/views.py:547`); gestor envia `novo_status` válido (`acoes_chamado.py:313`). | **Não para renascimento:** chama `transicionar`, não `reabrir`. O motor permite `resolvido → em_analise` e saída de `fechado` para [nome] (`app/chamados/catalogo/motor.py:102`, `:150`, `:153`). A guarda de premissa em `transicionar` só roda para destino `resolvido` (`app/chamados/models.py:165`). |
| `app/ponto/management/commands/detectar_ausencias.py:301`; `app/chamados/services/adesao.py:92`; `app/ponto/management/commands/reabrir_chamados_ausencia_aberta.py:49` | Reabrem, respectivamente, âncora, adesão e ausência ainda aberta (`detectar_ausencias.py:273`; `adesao.py:74`; `reabrir_chamados_ausencia_aberta.py:29`, `:36`). | Delegam a `reabrir`; são caminhos de cadastro/ausência, não detectores de marco faltante. |

Formulários genéricos também chegam a `criar` (`app/chamados/services/abertura.py:47`, `:172`), chamados pelas views em `app/chamados/views.py:197`, `:136`. A API genérica grava módulo `app_colaborador` (`app/api/views.py:2237`, `:2246`). Permissões completas do classificador para produzir categorias de turno: **nao li**.

`intervalo_pendente` não tem emissor ativo naquele comando: o fluxo termina em `continue` (`app/ponto/management/commands/detectar_intervalo_ausente.py:145`, `:152`). O emissor `sem_batidas_mes` está removido do fechamento (`app/ponto/services/fechamento.py:1075`).

**Sobre célula absolvida:** há nascimento sem consulta e nascimento com veto que deixa passar `concorde`; isso está demonstrado acima. Já uma ata lavrada, sem lâmpada apagada nem órfã, bloqueia a reabertura canônica de furo (`app/chamados/juizes.py:433`, `:436`, `:438`, `:446`; `app/chamados/models.py:962`). **A transição humana é uma via adicional que não passa nessa guarda** (`acoes_chamado.py:315`).

**Caso de referência:** o chamado pode nascer antes da consulta do cartório; o `post_save` chama `julgar_celula` (`app/chamados/models.py:723`; `app/chamados/signals.py:158`, `:174`). Chamados entram na impressão (`app/ponto/services/cartorio.py:1076`), e o julgamento pode lavrar/carimbar novamente (`:1402`, `:1417`). Que um caso concreto termine **somente** com impressão alterada, mantendo chamado e ata: **não provado sem execução**.

## T7 — `disparar_perguntas_competencia`

Comando: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`. Na tabela, **CMD** refere-se a esse arquivo; **FAB** a `app/chamados/services/fabrica_da_celula.py`.

| Decisão | Pergunta respondida | Juiz existente / diferença de universo |
|---|---|---|
| CMD`:155` | Quais chamados vivos pertencem à janela? | `VIVOS`, registrado em `app/core/juizes.py:227`. Seleciona pela data da célula, sem exigir módulo, trabalho ou veredito acusador. FAB`:73` exige trabalho. |
| CMD`:156` | Pertence ao colaborador solicitado? | Recorte do operador; também aplicado ao primeiro universo em CMD`:77`. | [nao verificado]
| CMD`:163` | Há alguma resolvedora viva? | Repete a redução de `fio_vivo` (`app/chamados/juizes.py:1111`, registro `app/core/juizes.py:241`), chamando o juiz individual `pergunta_viva` (`juizes.py:778`). |
| CMD`:165` | Qual é o dia do fato? | Usa `ch.celula.data`; `dia_do_fato` já prioriza célula (`app/chamados/juizes.py:60`). |
| CMD`:167` | Há motivo vencido da ata ainda não selecionado? | Já chama FAB`::motivos_pela_ata:34` e `vencidos_no_instante:15`. FAB`::motivos_a_perguntar:68` compõe essas perguntas com trabalho, ativo, futuro, veredito e dedup. |
| CMD`:168`, `:172` | Esse fato já entrou nesta rodada? | `_vistos` é memória da seleção, inicialmente formada pelo primeiro universo (CMD`:154`). O filtro de `:168` já exclui a chave testada em `:172`. |
| CMD`:178` | Pode perguntar ao colaborador? | Exige ativo, assim como FAB`:73` e `_ja_perguntou` (`app/chamados/services/disputa_emissao.py:1206`). Juiz registrado específico para esse predicado: não localizado. |
| CMD`:181` | Já há pergunta viva desse motivo/dia? | `_ja_perguntou` consulta perguntas da **primeira disputa obtida** (`disputa_emissao.py:1202`) e delega a `fio_vivo` (`:1209`). |
| CMD`:220` | Já existe pergunta do mesmo fato? | Identidade já expressa por `PerguntaDisputa.do_fato` (`app/chamados/models.py:1916`, `:1927`), usada pelo gate em `:1966`. O `exists()` inclui qualquer estado e duplicatas. |
| CMD`:225`, `:226` | O alvo entra no lote? | Acrescenta o primeiro motivo ainda não visto por chamado (CMD`:167`). FAB`:95` devolve todos os motivos ainda não perguntados da célula. |

**A existência em CMD`:220` não prova decisão humana:** mesmo assim a saída rotula esse conjunto como “FIO DECIDIDO” (CMD`:232`). A consulta não testa validação, resposta ou via de resolução (CMD`:220`).

**Universos temporais:**

- **Não existe exclusão geral `data >= hoje` neste ramo.** Ambos usam vencimento por instante; hoje pode perguntar marco vencido. Sem marcos, só passado estrito recebe `None`; hoje/futuro recebe conjunto vazio (FAB`:28`, `:30`, `:45`; CMD`:167`).
- O evento exclui explicitamente futuro (FAB`:75`); o comando usa os mesmos vencidos (CMD`:111`, `:167`). Isso não equivale a `dia_encerrado`, registrado em `app/core/juizes.py:189`: a fábrica admite emissão parcial antes do fim (FAB`:15`).
- Competência é janela de seleção do comando (CMD`:61`, `:76`, `:155`); não é fronteira da fábrica. A guarda de competência exportada está retirada dos dois caminhos (CMD`:115`; FAB`:83`).

**O que impede afirmar que o evento substitui o cron:**

1. **Fio mudo sem novo julgamento:** evento só percorre células efetivamente julgadas (`app/ponto/services/cartorio.py:752`, `:762`); impressão igual pula julgamento (`:1085`). CMD`:155` varre chamados independentemente disso.
2. **Predicados diferentes:** evento exige trabalho e aceita somente veredito compatível (FAB`:73`, `:81`); fio mudo não aplica esses dois filtros (CMD`:155`, `:167`).
3. **Dedup diferente:** evento exclui duplicatas da identidade existente (FAB`:93`); fio mudo combina viva na primeira disputa com existência global incluindo duplicatas (CMD`:181`, `:220`).
4. **Escritor já é compartilhado:** comando e evento chegam a FAB`::perguntar:98` (CMD`:288`; FAB`:123`). O impedimento demonstrado está na seleção e no gatilho.
5. **Configuração ainda emissora:** o encadeamento usa `--comp-corrente --apply` (`app/config/crons.py:1435`). O modo somente alarme existe, mas não é o configurado (CMD`:261`).
6. Equivalência e quantidade de casos na frota: **não provado**; logs e banco, **nao li**.

## T8 — Contra-exemplos da JUIZ-DO-PAR

Referências abaixo em `_juizpar/app/`; o patch altera o matcher em `_juizpar/DIFF_JUIZPAR.patch:421`. Resultados deduzidos por leitura, sem testes ou banco.

| Caso / entradas | Ata nova | Outro leitor | Resultado |
|---|---|---|---|
| **Avulsa após fechamento.** Marcos `08/12/13/18`; batidas `E08 S12 E13 S17 S18`; ata anterior muda. | O pré-juiz produz turno `08→17` e uma saída órfã. Dois turnos desativam o papel (`escala/utils.py:1535`). A DP escolhe `S18` para `hf18` e deixa `S17` órfã (`:230`, `:596`). | O pareador absorve intervalo12/13, fecha em17 e produz órfã18 (`ponto/turnos.py:1291`, `:1417`, `:1422`). Na releitura da ata nova,17 fica longe das luzes para cluster e a4h da luz de intervalo13; o mapa de papel abstém-se (`:1668`, `:1689`, `:1693`; teto3h em `:317`). | **Divergência estável por leitura:** `S18` é saída na ata e órfã no juiz. A reconciliação não troca `hf` já ocupado (`escala/utils.py:1937`). |
| **Dois turnos.** Marcos `08/12/13/18`; `E08 S10 E16 S18`. | Dois turnos mantêm DP (`escala/utils.py:1535`, `:1642`). O cluster retira16 de `hfi13`, pois18 já ocupa o marco mais próximo (`:611`). | S10 está120min longe do intervalo12 e não é absorvida; juiz fecha `08→10` e `16→18` (`ponto/turnos.py:1293`, `:1417`). | **Divergência de papel:** ata trata16 como órfã; geometria como entrada do segundo turno. Reconciliação cobre bordas do dia, não essa volta (`escala/utils.py:1913`). A Zona5 também pode eliminá-la pelo cluster (`escala/utils.py:545`). |
| **24x48, início=fim08.** `E D08`, pausa12–16, `S D+1 07:30`. | Encadeamento coloca `hf08` emD+1; delta da saída é−30min (`escala/utils.py:346`). | A referência da Zona5 só avança hora **menor** que início; `hf==hi` fica emD08 (`ponto/motor_calculo_v2.py:2981`). | Diferença de referência de1440min **se alcançar Zona5**. Plantão normal herda `MotorBase`, não o motor da Zona5 (`motor_calculo_v2.py:3425`, `:3511`, `:3606`). Cadastro24x48 que alcance essa rota: **não provado**. |
| **Noturno regular.** Marcos19/01/02/07; `E D19:02 S D+1 01:01 E02:00 S07:03`. | Cadeia avança os marcos; único turno fechado fornece papéis (`escala/utils.py:346`, `:1535`, `:1642`, `:525`). | Zona5 também avança01/02/07 por serem menores que19 (`ponto/motor_calculo_v2.py:2981`). | **Sem divergência identificada neste caso.** Caso correspondente lido, não executado (`escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:360`). |
| **Tipo gravado trocado.** Marcos11/14/15/19:30; `E11 E13:02 S15:39 S16:49`. | Papéis vêm dos pares do juiz, não dos botões (`escala/utils.py:421`); cenário no teste em `escala/tests/test_juiz_do_par_ata_casa_pelo_turno.py:316`. | Pareador aceita papel da ata e pares com tipos trocados (`ponto/turnos.py:1317`, `:815`). | **Contra-exemplo novo não provado.** O teste lido compara segunda e terceira lavra (`test_juiz_do_par_ata_casa_pelo_turno.py:338`). |
| **Intervalo livre.** Bordas08/18; `E08 S12 E13 S18`. | Sem `hii/hfi`, não recebe o turno no matcher; usa caminho de pausas livres (`escala/utils.py:1581`, `:1586`, `:1643`). | Zona5 exige quatro marcos e não participa (`ponto/motor_calculo_v2.py:2972`). | Divergência com Zona5 não demonstrada. Caminho completo de inferência das pausas livres: **nao li integralmente**. |

Os dois primeiros são divergências que **persistem no código novo**, não regressões atribuídas ao patch: nesses casos o builder conserva a DP ao desativar o papel (`_juizpar/app/escala/utils.py:1535`, `:1642`).

## 29/10 07:55 · sessao 6da5510d7ea7

Hash: `1974dbc089102d3f937990e9afe0b1282880f082` — 3.919 arquivos rastreados conferidos contra o HEAD, zero divergências.

**T4 — escritor de estado**

**(a) Fora da porta**

- `app/chamados/models.py:192` — Escrita de `status_local` fora das portas: nenhuma encontrada.
  Por quê: a atribuição encontrada pertence a `_gravar_estado`, persistida em `:231`; nenhum `.update(status_local=...)` ou `bulk_update` de estado encontrado em produção.
- `app/chamados/models.py:1395` — Escrita de `fechada_em` fora das portas: nenhuma encontrada.
  Por quê: as três atribuições de produção pertencem a `fechar`, `reabrir_admin` e `reabrir_sistema`.
- `app/chamados/models.py:756` — Renascimento de chamado fora de `reabrir`: nenhum encontrado.
  Por quê: `_renascer_por_premissa_viva` delega para `reabrir`; ausência absoluta de caminhos dinâmicos: **nao provado**.

**(b) Pela porta**

- `app/chamados/models.py:231` — `_gravar_estado` persiste as transições.
  Por quê: `transicionar`, `reconciliar`, `cancelar`, `superar`, `analisar`, `reabrir`, `encerrar` e `encerrar_por_competencia_trancada` convergem nesse escritor.
- `app/chamados/models.py:723` — `abrir` nasce por `get_or_create`.
  Por quê: a criação está dentro da porta; o dedup de encerrado chama `_renascer_por_premissa_viva` em `:685`.
- `app/chamados/models.py:813` — `criar` nasce por `create(status_local=status)`.
  Por quê: o `create` pertence à própria porta de nascimento.
- `app/chamados/models.py:1399` — `DisputaSupervisao.fechar` persiste o fechamento.
  Por quê: grava `fechada_em`, autor, resolução e motivo pelo método canônico.
- `app/chamados/models.py:1618` — `reabrir_admin` persiste a reabertura administrativa.
  Por quê: limpa o fechamento e grava o contador administrativo pela porta.
- `app/chamados/models.py:1634` — `reabrir_sistema` persiste a reabertura automática.
  Por quê: limpa os campos de fechamento pela porta separada.
- `app/chamados/services/disputa_emissao.py:523`, `:927`, `:1147` — Três `create()` diretos fazem nascer disputas abertas.
  Por quê: são **pela porta de emissão**, expressamente declarada em `app/core/portas.py:438`; classificá-los como escrita externa seria incorreto.
- `app/chamados/models.py:2067` — `encerrar_fantasma` pertence a `PerguntaDisputa`.
  Por quê: não é escritor do estado dos dois modelos pedidos.

**(c) Testes e migrations — contagem estática** [nao verificado]

- `app/chamados/tests/` e demais testes de `app/` — 690 chamadas de nascimento identificadas: Chamado 387 `create` + 2 `get_or_create`; Disputa 300 `create` + 1 `get_or_create`. Também 51 atribuições a `status_local`/`fechada_em` e 36 `update` desses campos. [nao verificado]
  Por quê: contagem de ocorrências AST; atribuição em memória não prova persistência. Total semântico incluindo construções dinâmicas: **nao provado**.
- `app/chamados/migrations/0038_a3_registrado_backfill.py:10`, `:15`; `0039_a3_via_backfill.py:24`, `:33` — Quatro `update` de estado.
  Por quê: são migrações de dados; não entram no censo de produção acima.

**T5 — ordem de trava**

- `app/chamados/models.py:669` — `ChamadoColaborador.abrir`: trava **Colaborador**.
  Por quê: `select_for_update().filter(...).first()` executa antes do dedup; não há trava explícita de Chamado ou CelulaDia nessa função.
- `app/chamados/services/abertura.py:164` — `abrir_chamado_para_colaborador`: trava **Colaborador**; chamado por `app/chamados/views.py:136`.
  Por quê: depois da trava chama `ChamadoColaborador.criar`, que não acrescenta outro `select_for_update`.
- `app/colaboradores/models.py:431` — `Colaborador.save`: trava o **Colaborador com maior matrícula elegível**, quando falta matrícula.
  Por quê: a consulta ordenada é avaliada por `.first()`; não trava necessariamente a própria pessoa salva.
- `app/chamados/services/disputa_emissao.py:467` — `_abrir_ou_agregar_disputa`: sequência de consultas **Disputa → Colaborador**, esta última via `ChamadoColaborador.abrir` em `:503`.
  Por quê: **os ramos são exclusivos**: encontrando disputa, agrega e retorna em `:502`; sem disputa, a consulta não travou linha de disputa e então chama `abrir`.
- `app/chamados/services/disputa_emissao.py:563`, `:598` — Chamadores da função anterior: `disputa_para_declaracao` e `criar_ou_atualizar_disputa`.
  Por quê: delegam ao mesmo núcleo; também há chamadas diretas em `app/ponto/management/commands/reconciliar_grade.py:415` e `app/chamados/management/commands/smoke_chamados.py:99`.
- `app/chamados/services/declaracao_texto.py:307`, `:605` — `declarar_hora_colab` e `aplicar_declaracao` chegam à trava por `disputa_para_declaracao`.
  Por quê: não possuem `select_for_update` próprio; a ordem vem do núcleo delegado.
- `app/chamados/services/disputa_emissao.py:1121` — `criar_disputa_ancora_de_chamado`: sequência de consultas **Disputa → Colaborador**, via `abrir` em `:1129`; chamado por `app/chamados/services/acoes_disputa.py:189`.
  Por quê: novamente só chama `abrir` quando **não encontrou disputa**; aquisição simultânea dessas duas travas de linha nesse trecho: **nao provado**.
- `app/chamados/services/disputa_emissao.py:847` — `criar_disputa_a_partir_de_chamados_orfaos` não executa `select_for_update` na disputa.
  Por quê: apesar da docstring em `:853`, a consulta em `:901` é comum; o nascimento chama `abrir` e trava Colaborador.
- `app/ponto/services/vigia_de_hora.py:98` — `rodar`: **advisory por empresa → Colaborador**, quando o julgamento emite chamado por `abrir`.
  Por quê: a chave é `7180000 + empresa`, mantida até o `unlock` em `:135`; não é trava de linha de CelulaDia. Chamadores: comando `vigia_de_hora.py:24` e registro de cron em `app/config/crons.py:2009`.
- `app/ponto/portas/celula.py:99` — Nenhum `select_for_update` ou `pg_advisory*` encontrado no módulo.
  Por quê: suas operações escrevem células, mas não declaram essas travas explícitas.
- `app/chamados/services/disputa_emissao.py:467`, `:1121`; `app/chamados/models.py:669` — **Par inverso entre os quatro modelos: nenhum provado.**
  Por quê: não apareceu caminho explícito Colaborador → disputa existente com `select_for_update`; os dois caminhos aparentes Disputa → Colaborador têm a exclusão de ramos descrita acima. Deadlock por travas implícitas de escrita, FK ou reentrada de signals: **nao provado**.

## 29/10 07:55 · sessao 6da5510d7ea7

**Não gravado:** o sandbox desta sessão é somente leitura, sem possibilidade de aprovação. O registro precisa ser aplicado pelo Claude escritor. [nao verificado]

Texto preparado para `CLAUDE.md`, seção 7b, sem número de lei:

> **REGISTRAR-GPT6-CODEX (aval Ronald, 28/10).** Desde 28/10, GPT-6/Codex (Codex CLI 0.165.0, tmux `codex-hasner` na VM) é auxiliar de LEITURA do Claude do chat: nunca escreve. Opera em `~/codex_ws`, cópia do HEAD por `git archive`, com `--sandbox read-only --ask-for-approval never`. Recebe tarefas somente por delegação do Claude: censo pela regra, contra-exemplo para o verificador e triagem de laudo. A resposta é lida por `~/codex_prototipo/gerar_codex_estado.py` e conferida por amostra no clone antes de virar aval.

Texto preparado para seção nova do RELATO, abaixo do teto de 25 linhas:

> **REGISTRAR-GPT6-CODEX — 28/10**
> LEI-AKITA: origem=CLAUDE.md::7b, testemunha=clone do HEAD, RED=n/a (registro), quem-mais-le=Claude do chat, juizes novos=0, padrao=n/a (instrumento).
> Aval Ronald: Codex auxiliar de leitura, somente por delegação do Claude; condições operacionais na seção 7b.
> Primeiras entregas, conforme este aval: CENSO-DE-LEITORES da JUIZ-DO-PAR (~20 sítios fora de `_match_marcos`) e CENSO-DO-HEAD do O-OUTBOX; ambos informados como registrados no BACKLOG.
> Conferência desta cópia: os nomes AGENTE-NAO-ESPERA e AGENTE-NO-MODELO-CERTO não aparecem no CLAUDE.md.
> INCOMPLETO: gravação pelo escritor; texto original dos dois avais de agente não localizado, conteúdo **nao provado**.
> Commit: não realizado. [nao verificado]

Ressalva para o registro do O-OUTBOX: a leitura anterior encontrou consultas na sequência Disputa → Colaborador, mas em ramos exclusivos; aquisição conjunta dessas duas travas e ausência geral de deadlock: **nao provado**.

## 29/10 07:47 · sessao 1ee182181f58

Fonte: commit `b3bf529ef8097638c70b6913649912a68021e2c9`, lido por `git show`. `_h2910` não estava disponível; os caminhos abaixo pertencem ao commit pedido. Sem escrita, testes ou banco.

## T9 — Quem produz a classe do cron do feriado

**Premissa exata:**

1. Chamado `batida_ausente` em `VIVOS`, opcionalmente da empresa selecionada (`app/chamados/services/feriado_reavaliar.py:17`).
2. `dia_do_fato(ch)` retorna uma data (`:30`).
3. `fatos_do_dia` retorna previsão conhecida: `era_dia_previsto is not None` (`:33`, `:34`).
4. A faixa vencedora é `feriado` ou `folga`, e o catálogo considera morta a premissa com `FATO_FALTA_DESCOBERTA=False` (`:36`, `:38`).
5. **Para fechar efetivamente**, `ch.reconciliar()` precisa aceitar (`:47`): motor permite a transição e o gate aceita premissa regularizada, agrupamento ou cobrança irmã sobrevivente (`app/chamados/models.py:467`, `:470`, `:495`, `:498`). Selecionar como espúrio não prova fechamento.

As entradas do seletor são Ausências não rejeitadas, FolgaDia, feriados aplicáveis, vínculo/geradora, `CelulaDia.trabalha` e `trabalha_em_feriado` (`app/ponto/precedencia.py:211`, `:220`, `:229`, `:291`, `:300`, `:314`). O lugar vem de **`Colaborador.posto.municipio`**, não de `Praca.cidade/estado` (`app/core/feriados.py:46`, `:113`).

Na tabela, **direto** significa chamar `feriado_reavaliar`; evento do cartório está identificado separadamente.

| Ato / porta de escrita | Reavaliação já disparada? |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar: `:141` | **Sim, direto:** `post_save` agenda `reavaliar_dia` em `on_commit` (`app/chamados/signals.py:259`, `:269`, `:270`). Mudança de data agenda somente a **nova** (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não.** Ausência de `post_delete` é deliberada: esse consumidor fecha, não reabre (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill: `:70` | **Não, direto.** O evento de Posto encontrado trata geofence (`app/ponto/signals.py:291`, `:306`); o backfill usa `update`, sem `save`. |
| Alterar Município.nome/UF: `app/colaboradores/management/commands/seed_municipios.py:38` | **Não nessa porta.** Esses campos determinam alcance estadual/municipal (`app/core/feriados.py:52`, `:53`, `:63`, `:66`). |
| Mover/desvincular colaborador de posto: portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75`; chamadas em `app/colaboradores/views.py:329`, `:645`, `:670` | **Não, direto.** Muda o lugar usado pelo juiz (`app/core/feriados.py:113`). |
| Criar/editar calendário e tamanhos: `app/escala/folgas.py:479`, `:483`, `:490`, `:504`, `:508` | **Não nessa gravação.** A view salva e redireciona sem materializar fotos (`app/escala/views.py:362`, `:364`). |
| Materializar calendário; lançar, alterar/remover ou aplicar lote de folgas: `app/escala/folgas.py:172`, `:173`, `:259`, `:280`, `:284`, `:289`, `:334` | **Sim, condicional:** esses fluxos relavram células (`:192`, `:261`, `:303`, `:340`); o evento exige `recalculo=True` e dias efetivamente tocados (`app/ponto/portas/celula.py:420`, `:431`, `:432`). |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Condicional:** relavra da competência corrente em diante (`:441`, `:442`), depois vale o gate anterior. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho:** o contrato exige materialização pelo chamador (`:434`). As telas lidas materializam (`app/colaboradores/views.py:245`; `app/escala/views.py:414`). |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Condicional:** chama regeneração em `:320`, alcançando o evento se houver dias tocados. |
| Alterar vigência/encerrar vínculo: `app/colaboradores/services/vinculo.py:799`; alterar âncora: `:845` | **Condicional:** signals comparam vigência/âncora e regeneram (`app/escala/signals.py:56`, `:71`, `:104`, `:117`); depois vale o gate E2. |
| Alterar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada:** os signals citados comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:107`). |
| Updates de vigência, absorção, restauração ou desfazimento: `app/colaboradores/services/vinculo.py:615`, `:691`, `:753`, `:864`, `:874`, `:940`, `:941` | **Sem evento próprio nessas portas.** Regeneração posterior depende do chamador; o wizard a faz em `:320`. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota com **`recalculo=False`** (`:348`, `:350`), portanto não dispara E2 (`app/ponto/portas/celula.py:420`). |
| Regenerar células: `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**, somente para datas adicionadas a `_tocadas` (`:234`, `:249`, `:267`) e com `recalculo` (`:420`). |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam o reavaliador nessas funções**; terminam em `:551` e `:616`. |
| Criar/editar/decidir Ausência: `app/ponto/services/ausencia.py:418`, `:508`, `:819`, `:911`; apagar espelhos/intervalos de férias: `:1174`, `:1197`, `:1254` | **Não, direto; têm eventos:** `save/delete` julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`), além do consumidor de silêncio/fechamento (`:263`, `:269`). |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | O `update` não emite signal. Porém o cancelamento completo salva o agendamento depois (`app/ferias/services.py:356`, `:362`), cuja sincronização remove/espelha Ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote: `app/ponto/views.py:1790`, `:1862` | **Não, direto; sim cartório por `post_save`** (`app/ponto/signals.py:161`, `:165`). Justificativa não entra no seletor de precedência; entra no gate final de fechamento (`app/ponto/precedencia.py:211`; `app/chamados/models.py:875`). |
| Registrar/retratar/apagar Batida: `app/ponto/registro_batida.py:94`, `:267`, `:285` | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório**, não do reavaliador específico (`app/ponto/signals.py:131`, `:138`). |
| Ata/veredito, associação do chamado e estado: `app/ponto/portas/celula.py:650`, `:671`; `app/chamados/services/vinculos.py:88`, `:99`; `app/chamados/models.py:231` | Não chamam diretamente o reavaliador. O evento de chamado rejulga em criação/mudança de status, não na simples associação (`app/chamados/signals.py:163`, `:174`). |

**Ficam sem o evento específico:** exclusão/data anterior de Feriado; mudança de localização; calendário não materializado; mudança isolada de `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; regeneração de frota com `recalculo=False`; reversão, geração ordinária e mudança sem `_tocadas`. O próprio código declara os casos célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`).

Ausência, Justificativa e Batida **têm outros eventos**; equivalência integral desses consumidores com a classe do cron não está provada pelas chamadas acima. Outros chamadores das portas de restauração e operações de admin Django: **nao li**.

## T10 — Célula × justificativa aprovada × turno aberto

### (a) Onde a justificativa entra

**Já existe leitura neste HEAD, pela grade:**

- O cartório monta a grade (`app/ponto/services/cartorio.py:874`); o builder aplica `_marcar_ausencias` (`app/escala/utils.py:1785`).
- `_marcar_ausencias` consulta Justificativa `aprovada` pelo dia (`app/escala/utils.py:1157`), troca colunas `missing` por `None`, limpa órfãs e marca `justificado=True` (`:1200`, `:1202`, `:1203`).
- `dispensar_por_ausencia` lê esse flag (`app/ponto/services/cartorio.py:602`), acende marcos, deixa `luz=None`, grava `dispensada='justificativa'` e recalcula faltantes (`:608`, `:609`, `:610`, `:612`). É aplicada antes da ata final (`:1407`).

Portanto, o ponto pedido por ATA-NAO-DISPENSA-JUSTIFICADA **já é `dispensar_por_ausencia`**; a leitura direta de Justificativa em `cartorio.py:279` serve a `houve_fato_novo`, não à fabricação das lâmpadas.

**Há uma condição para o efeito chegar à ata:** o signal da Justificativa chama julgamento (`app/ponto/signals.py:161`, `:165`), mas justificativa/`justificado` não compõem explicitamente a impressão (`app/ponto/services/cartorio.py:314`, `:323`). Se todos os componentes da impressão permanecerem iguais, o julgamento pula (`:1092`). Ocorrência concreta desse caso: não verificada.

### (b) Como uma entrada sem saída pode resultar em `concorde`

**Uma entrada sem saída, sozinha, não implica `concorde` neste HEAD.**

| Etapa | Caminho |
|---|---|
| Batida e encerramento | O cartório conta a entrada casada e passa o encerramento decidido pelo juiz (`app/ponto/services/cartorio.py:1101`, `:1117`). |
| Sem par fechado | Realizado zero não satisfaz `REALIZADO_ZERO_COM_TURNO` com uma única célula OK: o mínimo é dois (`app/ponto/supra_juiz.py:48`, `:256`). Sem realizado nem órfã, `_tem_fato_dia=False` (`:348`). |
| **Caso normal sem justificativa** | O ramo específico exige batida, realizado zero, encerramento, trabalho previsto, saída `missing`, sem cobertura aprovada e sem defeito de âncora; produz **`FURO_PARCIAL`** (`app/ponto/supra_juiz.py:394`, `:399`). |
| **Com justificativa aprovada** | A grade remove os `missing` (`app/escala/utils.py:1200`); `_n_missing` fica zero e esse ramo não entra (`app/ponto/supra_juiz.py:347`, `:395`). |
| Decisão final | Sem outros protestos e sem fio vivo, códigos vazios derivam **`concorde`** (`app/ponto/services/cartorio.py:1121`, `:1141`; `app/escala/models.py:1595`). Com fio vivo, o cartório força `cobrado` (`cartorio.py:1141`, `:1142`). |

`marcos_pendentes` **não lista saídas faltantes**: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo significa marco ainda não vencido, escrito pelo teto do cartório (`app/ponto/services/cartorio.py:703`, `:704`). Pode estar vazio num dia vencido cuja saída continua `missing`; nesse caso, a acusação vem do ramo `FURO_PARCIAL` (`supra_juiz.py:394`).

### (c) Qual classe o O261 conta

| Filtro | Evidência |
|---|---|
| Chamado vivo com célula associada | `app/chamados/services/vigia_fio.py:184` |
| Módulo pertencente a `FURO_MODULES` | `vigia_fio.py:199`; vocabulário em `app/chamados/catalogo/furo_modulos.py:15` |
| **Célula com veredito acusador** | `vigia_fio.py:226`, `:227`; `concorde` não pertence a `VEREDITOS_ACUSAM` (`app/escala/models.py:1735`) |
| Sem pergunta viva | `vigia_fio.py:258` |
| Data anterior a hoje | `vigia_fio.py:261` |
| Sem decisão do dia oferecida ao admin, ausência aberta ou aceite de folga validado | `vigia_fio.py:266`, `:273`, `:279` |
| Restante conta uma célula em `fio_morto` | `vigia_fio.py:286` |
| Também conta em `fio_morto_cobrando` se houver chamado correspondente em `COBRANCA` | `vigia_fio.py:200`, `:288`; estados em `app/chamados/catalogo/motor.py:29` |

**Não é a classe `concorde` de (b): ela fica fora antes de examinar o fio** (`app/chamados/services/vigia_fio.py:227`). A mesma geometria poderá entrar se resultar em `furo` e satisfizer os demais filtros (`app/ponto/supra_juiz.py:399`).

## T11 — Plano do grupo (1) do censo de leitores

O grupo declarado está em `app/docs/BACKLOG.md:756`; o juiz registrado é `escala/utils.py::_match_marcos` (`app/core/juizes.py:166`).

A chamada existente é `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`: recebe turno, deriva seus papéis e devolve células/órfãs (`app/escala/utils.py:551`, `:562`, `:590`). **Mudar a chamada não prova equivalência de saída.**

| Sítio | Pergunta atual | Consulta ao juiz que substitui a atribuição | Muda saída? |
|---|---|---|---|
| `app/ponto/services/lastro.py:274` | Sem lâmpada, há batida do **tipo gravado** perto do marco? Escolhe a mais próxima (`:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler ocupação do marco cobrado. Preservar o degrau da lâmpada, que já é autoridade (`:265`). | **Não sei**: tipo/proximidade não equivale a papel/exclusividade (`:283`; `app/escala/utils.py:579`). |
| `app/ponto/propositor.py:112` | Atribuição global com teto, custo, furos, sobras e ambiguidade (`:133`, `:165`). | Matcher sobre instantes/turno; projetar ocupação, faltantes e órfãs. | **Não sei**: `custo`, `desloc` e `ambiguo` fazem parte do contrato antigo (`:166`) e não da saída do matcher (`utils.py:590`). |
| `app/ponto/propositor.py:87` | Como encadear slots/marcas noturnos antes da atribuição (`:99`, `:107`)? | `encadear_marcos` (`app/escala/utils.py:330`) e matcher. | **Não sei**: wrapper avança com `<`; cadeia canônica com `<=` (`propositor.py:99`; `utils.py:346`). |
| `app/ponto/services/flip_auto.py:233` | Qual tipo a batida deveria ter pelo slot recebido (`:235`, `:240`)? | Ler papel/tipo do marco ocupado pelo matcher, mantendo elegibilidade (`:226`, `:228`). | **Não sei**: altera o insumo que produz flips (`:241`). |
| `app/chamados/services/materializacao.py:165` → `app/ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar existentes e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Matcher antes/depois da candidata; comparar ocupações das mesmas batidas. | **Não sei**: a paridade autoriza plantio (`materializacao.py:436`, `:816`). |
| `app/ponto/propositor.py:315` | Qual o primeiro furo posicional (`:320`)? | Primeiro marco `missing` do matcher sobre o turno completo. | **Não sei**: há inferência própria de pausa e recusa de noturno antes da atribuição (`:276`, `:295`, `:311`). |
| `app/ponto/propositor.py:351` | O dia está completo posicionalmente? | Projetar ausência de furos do mesmo resultado canônico. Hoje coleta por dia civil (`:337`, `:345`). | **Não sei**. Chamadores: `app/chamados/reconciliador.py:112`; primeiro-furo também em `app/escala/servico_jornada.py:442`. |
| `app/ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização, custo, sobras e furos? | Consumir atribuição canônica; hoje remonta slots/marcas (`:98`, `:101`). | **Não sei**: imprime também métricas do contrato antigo (`:111`). |
| `app/ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender com hora livre a≤30min? | Matcher com todos os marcos/batidas; ler ocupação exclusiva. | **Não sei**: hoje acende diretamente `acesa/luz`, sem tipo real (`:486`, `:491`). |
| `app/chamados/services/desvio_escala.py:74` | Qual batida pertence a cada marco para calcular medianas? Dia cheio usa posição; incompleto, proximidade (`:78`, `:83`, `:86`). | Matcher por dia/turno, seguido da agregação dos deltas ocupados. | **Não sei**: altera potencialmente amostras e classificação `deslocado` (`:89`, `:90`). |
| `app/chamados/services/declaracao_texto.py:131` | Hora declarada corresponde a único marco faltante≤240min, sem quase empate (`:134`, `:137`)? | Matcher incluindo candidata e concorrentes; localizar a candidata no resultado. | **Não sei**: escolha determina motivo/tipo (`:292`, `:296`, `:297`); outro chamador em `:573`. |
| `app/chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da hora respondida (`:277`, `:280`)? | Matcher com candidata/turno; projetar tipo ocupado. | **Não sei**: hoje marco explicitamente cobrado prevalece (`:257`, `:291`). |
| `app/chamados/detector_anomalias.py:210` | Qual marco/categoria de desvio recebe a batida? | Já chama matcher, mas com `derivar=False`; passar o turno disponível no coletor (`:202`). | **Não sei**: afeta classificação de desvio (`:214`, `:223`, `:226`). Segunda resposta declarada em `:206`. |

**Transcritores e consumidores que já delegam:**

- `app/escala/utils.py::marco_ocupado_pela_batida:353` chama o matcher (`:393`) e transcreve rótulo/delta (`:399`). Trocar a transcrição: **não**; alimentar turno completo exige ampliar a alimentação atual (`:353`).
- `app/escala/regua_defesa.py:492` usa esse transcritor, porém passa só as pontas de **um período** (`:491`). Efeito de fornecer o turno completo: **não sei**; resíduo registrado em `app/docs/BACKLOG.md:757`.
- `app/ponto/calculador/regras.py:251` delega a pergunta da volta; a saída de intervalo ainda usa proximidade em `:259`. Substituição desse último critério pelo papel ocupado: **não sei**.

**Demais sítios encontrados:**

| Sítio | Pergunta / substituição | Muda saída? |
|---|---|---|
| `app/ponto/turnos.py:2248` | Tipo oposto perto de marco autoriza flip (`:2261`, `:2267`)? Matcher responde ocupação; política de flip continua sendo outra pergunta. Chamadores em `app/ponto/services/flip_auto.py:300`, `app/ponto/services/veredito_celula.py:242`, `app/chamados/management/commands/auditar_disputas.py:119`. | **Não sei**. |
| `app/ponto/turnos.py:2275` | Tipo retroativo pela proximidade de exatamente um tipo de marco (`:2289`, `:2294`). Consultar papel ocupado do matcher. | **Não sei**. Chamadores: **nao li**. |
| `app/ponto/services/veredito_celula.py:266` | Vizinha≤3h permite fabricar candidata de intervalo (`:263`, `:267`)? É geração de proposta, depois simulada na grade (`:273`), não apenas atribuição. | **Não sei**. |
| `app/ponto/turnos.py:2339` | Qual próximo tipo **ainda não batido**, pelo marco próximo (`:2330`, `:2341`)? Não é ocupação de batida existente. | Substituição direta: **não sei**. |

**Motor — somente lista:**

- **!** `app/ponto/motor_calculo_v2.py:2016`
- **!** `app/ponto/motor_calculo_v2.py:2028`
- **!** `app/ponto/motor_calculo_v2.py:2055`
- **!** `app/ponto/motor_calculo_v2.py:2113`
- **!** `app/ponto/motor_calculo_v2.py:3000`

Fora dos sítios nomeados, delegações e buscas descritas, não li integralmente todos os módulos de `app/`; completude absoluta do censo: não provada.

## 29/10 07:46 · sessao f89af232ead9

T9 — fonte lida por `git show b3bf529e:app/...`, commit exato `b3bf529ef8097638c70b6913649912a68021e2c9`; `_h2910/app` não existe nesta visão. Nenhuma escrita, teste ou banco.

**Premissa exata do cron**

1. Universo: `modulo_origem='batida_ausente'`, `status_local in VIVOS`, opcionalmente empresa (`app/chamados/services/feriado_reavaliar.py:14`, `:17`, `:18`).
2. Obtém `d=dia_do_fato(ch)`; sem dia, ignora (`feriado_reavaliar.py:30`, `:31`).
3. Lê `fatos_do_dia(colaborador,d)`; se `era_dia_previsto is None`, ignora (`:33`, `:34`).
4. Só seleciona se `faixa in ('feriado','folga')` **e** `premissa_morta(... fatos={FATO_FALTA_DESCOBERTA:False})` (`:36`, `:38`).
5. Para efetivamente fechar, `encerrar` ainda exige `ch.reconciliar()` retornar True (`:47`). Este pergunta motor e `_gate_premissa_do_fechamento` (`app/chamados/models.py:467`, `:470`): premissa regularizada para folha, agrupado sob pai vivo ou outra cobrança viva do dia permitem; caso contrário recusa (`:495`, `:498`, `:525`). Portanto **o conjunto “espúrios” impresso não é prova de que todos fecharão** (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:24`, `:32`).

**Entradas reais do juiz**

- Ausências de qualquer status exceto rejeitada, incluindo períodos abertos (`app/ponto/precedencia.py:211`, `:212`, `:215`); tipo decide faixa, e ausência pode vencer feriado/folga (`:278`, `:318`, `:333`).
- FolgaDia e calendário de origem; folga de calendário de outro vínculo é descartada (`:220`, `:297`).
- Lugar = `Colaborador.posto.municipio`, **não Praca.cidade/estado** (`app/core/feriados.py:40`, `:46`, `:106`, `:113`).
- Feriado.data/abrangencia/UF/município (`core/feriados.py:60`, `:62`, `:64`, `:83`).
- Vínculo do dia/geradora, célula.trabalha; na ausência de resposta da célula, previsão calculada (`precedencia.py:289`, `:291`, `:300`, `:309`).
- `ec.trabalha_em_feriado` impede que feriado suprima trabalho previsto (`precedencia.py:314`, `:319`).
- Justificativa aprovada **não entra nesse seletor**; entra no gate final de fechamento (`app/chamados/models.py:875`). Ata/veredito, marcos, batidas/turnos e cobrança irmã também entram nesse gate (`:889`, `:891`, `:897`, `:906`, `:908`, `:920`, `:921`; `:498`).

**Portas e evento — distinguir reavaliador exato de outro consumidor existente**

| Ato / porta de escrita | Já chama o reavaliador exato? / outro evento |
|---|---|
| Criar/editar Feriado: `app/core/views_config.py:81`, `:68`; importar nacional: `:141` | **Sim quando há save/criação**: post_save agenda `reavaliar_dia(instance.data)` no on_commit (`app/chamados/signals.py:259`, `:263`, `:269`, `:270`). Edição da data só agenda a **data nova**, não a anterior (`:263`). |
| Excluir Feriado: `app/core/views_config.py:198` | **Não**; ausência de post_delete é explicitamente declarada porque esse consumidor apenas fecha (`app/chamados/signals.py:255`, `:256`). |
| Alterar município do posto: `app/colaboradores/services/estrutura.py:50`; backfill por update: `:70` | **Não** para feriado/folga. Posto tem consumidor de **geofence**, não desse predicado (`app/ponto/signals.py:291`, `:303`, `:306`). O update de `:70` tampouco emite save. |
| Alterar nome/UF do Município: `app/colaboradores/management/commands/seed_municipios.py:38`, `:40` | Não há chamada ao reavaliador nessa porta. O nome/UF alteram o alcance em `app/core/feriados.py:52`, `:53`, `:63`, `:66`. |
| Mover/desvincular colaborador de posto: views `app/colaboradores/views.py:329`, `:645`, `:670`; portas `app/colaboradores/services/colaborador.py:53`, `:61`, `:68`, `:75` | **Não** para o reavaliador exato; altera o lugar consultado por `core/feriados.py:113`. |
| Criar/editar calendário de folgas: `app/escala/folgas.py:479`, `:483`, `:484`, `:488`, `:490`, `:504`, `:506`, `:508` | **Não nesta porta**. A edição da view salva e redireciona, sem materializar fotos (`app/escala/views.py:362`, `:364`). O calendário só altera a foto/previsão quando materializado. |
| Materializar foto de calendário: `app/escala/folgas.py:172`, `:173`; lançar folga: `:259`; alterar/remover folga: `:302` → `_foto_do_dia` (`:280`, `:284`, `:289`); lote: `:334` | **Sim condicional via E2**: relavram células (`:192`, `:220`, `:261`, `:303`, `:340`) e chegam ao on_commit de `app/ponto/portas/celula.py:420`. Só dispara quando `recalculo` e `_tocadas` não vazio. |
| Reparentar FolgaDia: `app/escala/folgas.py:436` | **Sim condicional**: somente da competência corrente em diante relavra (`:441`, `:442`), depois vale o mesmo gate E2. |
| Trocar calendário do vínculo: `app/colaboradores/services/vinculo.py:445` | **Não sozinho**: contrato manda chamador materializar (`:434`). As telas o fazem (`app/colaboradores/views.py:245`, `:246`; `app/escala/views.py:414`, `:415`), alcançando E2 condicional. |
| Criar/editar vínculo pelo wizard: `app/colaboradores/services/vinculo.py:274`, `:278` | **Sim condicional**: o fluxo chama regeneração explicitamente (`:320`), com E2 se houver dias tocados. |
| Alterar vigência por save; encerrar vínculo: `app/colaboradores/services/vinculo.py:799` | **Sim condicional**: signal detecta alteração de início/fim e regenera (`app/escala/signals.py:56`, `:58`, `:71`), depois E2. |
| Alterar âncora por save (`app/colaboradores/services/vinculo.py:845`) | **Sim condicional**: signal compara âncora e regenera (`app/escala/signals.py:104`, `:108`, `:117`), depois E2. |
| Marcar `trabalha_em_feriado`: `app/colaboradores/services/vinculo.py:825` | **Não por essa alteração isolada**: os signals de regeneração comparam vigência ou âncora, não essa flag (`app/escala/signals.py:57`, `:58`, `:107`, `:108`). |
| Fechar/reabrir vigência por update: `app/colaboradores/services/vinculo.py:615`, `:691`; absorver posteriores `:753`; restaurar snapshot `:864`, `:874`; desfazer ajuste `:940`, `:941` | **Sem evento próprio de save/reavaliador nessas portas.** Alguns fluxos chamadores podem regenerar depois; o wizard faz isso em `:320`. Não presumir cobertura de cada chamador. |
| Editar TipoEscala: `app/escala/services/cadastro_tipo.py:306` | Regenera frota, **mas sem E2**: passa `recalculo=False` (`:348`, `:350`), excluído pelo gate `app/ponto/portas/celula.py:420`. |
| Regenerar célula por vínculo: escrita em `app/ponto/portas/celula.py:232`, `:246`, `:263` | **Sim**: só dias realmente criados/alterados entram `_tocadas` (`:234`, `:249`, `:267`); on_commit chama `reavaliar_dias` (`:420`, `:431`, `:432`). Sem mudança, sem evento. |
| Reverter regeneração: `app/ponto/portas/celula.py:537`; geração ordinária: `:592`, `:604`, `:614` | **Não chamam E2/reavaliador nessas funções**; reversão termina em `:551`, gerador em `:616`. |
| Criar/editar Ausência: `app/ponto/services/ausencia.py:418`, `:508`; mudar estado/decidir: `:819`, `:911`; remover espelho férias: `:1174`, `:1197`; apagar intervalo férias: `:1254` | **Não chamam `feriado_reavaliar`**, mas **há eventos**: save/delete julgam cartório (`app/ponto/signals.py:143`, `:147`, `:150`, `:152`); on_commit executa silêncio e fechamento da ausência (`:263`, `:269`, `:271`, `:272`). Equivalência com este seletor não provada. |
| Rejeitar Ausências de férias em massa: `app/ponto/services/ausencia.py:1267` | Update não emite signal. **Não chamar o fluxo todo de sem evento**: cancelamento admin depois salva agendamento (`app/ferias/services.py:356`, `:362`), cuja sincronização espelha/remove ausências (`app/ferias/models.py:384`; `ausencia.py:1174`). |
| Aprovar/rejeitar/materializar Justificativa: `app/ponto/services/justificativa.py:88`, `:170`, `:265`; lote direto: `app/ponto/views.py:1790`, `:1862`; criação: `app/api/views.py:2059`, `app/ponto/views.py:1635`, regularização `app/ponto/services/regularizacao.py:91`, alerta `app/ponto/management/commands/processar_alertas_turno.py:374` | **Não reavaliador exato; sim cartório por post_save** (`app/ponto/signals.py:161`, `:165`). Nenhum receiver post_delete de Justificativa foi encontrado; único apagamento lido é ensaio (`zerar_dados_operacionais.py:95`). |
| Escrita de ata/veredito: `app/ponto/portas/celula.py:621`, `:650`, `:671`; vínculo chamado→justificativa/célula: `app/chamados/services/vinculos.py:88`, `:99`; status de chamado: `app/chamados/models.py:231` | Mudam gate/universo; **não chamam o reavaliador exato**. O evento de chamado só rejulga ao criar/mudar status (`app/chamados/signals.py:163`, `:174`), não na simples associação. |
| Batida: registrar/retratar/apagar (`app/ponto/registro_batida.py:94`, `:267`, `:285`) | Dependência indireta do gate (`app/chamados/models.py:920`, `:921`); **há eventos de cartório** para save/delete (`app/ponto/signals.py:131`, `:138`), não chamada direta ao reavaliador de feriado. |

**Atos sem o evento exato:** exclusão/antiga data de Feriado; mudança de localização; edição de calendário sem materialização; `trabalha_em_feriado`; updates/restauros de vínculo sem regeneração posterior; edição de template com `recalculo=False`; reversão e gerador ordinário de células; qualquer alteração que deixe `_tocadas` vazio. O próprio código declara essas lacunas de célula inalterada/frota/reversão/acervo (`app/chamados/signals.py:254`, `:255`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:21`, `:22`).

Ausência, Justificativa e Batida **não estão sem evento em geral**: têm os consumidores acima. Que esses consumidores cubram integralmente a classe do cron: **não provado**. Outros chamadores das portas de vínculo/restauração e operações de admin Django não detalhados acima: **nao li**.

## 29/10 07:45 · sessao d0f9ca89765c

Fonte lida: objetos de `b3bf529ef8097638c70b6913649912a68021e2c9` via `git show`; `_h2910` não existe nesta sessão. Caminhos abaixo relativos a `app/` desse commit. Sem execução/testes/banco.

O universo **declarado** do grupo (1) está em `docs/BACKLOG.md:756`; a autoridade registrada para “que marco previsto a batida ocupa?” é `escala/utils.py::_match_marcos` (`core/juizes.py:166`). O plano declarado exige migração um a um, RED e DIFF (`docs/BACKLOG.md:756`).

**Chamada de substituição já disponível:** `_match_marcos(abs_marcos, locs, consumed, tol_min, turno=turno_do_juiz)`; o matcher aceita turno explícito (`escala/utils.py:551`), deriva papéis desse turno (`:562`) e devolve células/órfãs (`:590`). Para resposta hipotética de materialização, o universo precisa incluir a candidata e as batidas concorrentes; equivalência com algoritmo antigo: **não provado**.

| Sítio | Pergunta atual | Chamada ao juiz / projeção necessária | Muda saída? |
|---|---|---|---|
| `ponto/services/lastro.py:274` | Sem lâmpada, existe batida **do tipo gravado** dentro da tolerância linear do marco? Escolhe a mais próxima (`:279`, `:282`, `:287`). | Matcher com marcos encadeados e turno completo; ler se o **marco cobrado** ficou ocupado. A lâmpada já é autoridade no degrau1 (`:265`). `marco_ocupado_pela_batida` é alternativa de projeção, mas hoje não recebe turno (`escala/utils.py:353`). | **Não sei** sem DIFF; existem critérios diferentes: tipo/proximidade versus papel/exclusividade (`lastro.py:283`; `utils.py:562`, `:579`). O plano anterior por raio foi explicitamente reprovado (`docs/BACKLOG.md:1432`). |
| `ponto/propositor.py:112` | Que marca ocupa cada slot, minimizando custo com teto4h, furos, sobras e ambiguidade (`:78`, `:133`, `:165`)? | Matcher sobre instantes completos e turno; projetar ocupação/furos/órfãs. O juiz não fornece diretamente o contrato `custo`, `ambiguo`, `desloc` retornado em `:166`. | **Não sei**; não é troca de função com contrato idêntico (`propositor.py:166`; `utils.py:590`). |
| `ponto/propositor.py:87` | Como adaptar a atribuição ao noturno? Faz encadeamento próprio e delega (`:99`, `:107`, `:109`). | `escala/utils.py::encadear_marcos:330` + matcher com turno. | **Não sei**; próprio wrapper usa `<` no slot (`propositor.py:99`), enquanto cadeia canônica usa `<=` (`utils.py:346`). |
| `ponto/services/flip_auto.py:233` | Qual tipo cada batida deveria ter pelo slot que recebeu (`:235`, `:240`)? | Consumir papel/tipo do marco ocupado no resultado do matcher, mantendo os gates próprios de elegibilidade (`:226`, `:228`). | **Não sei**; flips são derivados da atribuição (`:241`). |
| `chamados/services/materializacao.py:165` → `ponto/propositor.py:249`, `:250` | Plantio ocupa furo sem deslocar marcas existentes, e sem ambiguidade (`propositor.py:251`, `:254`, `:258`)? | Duas consultas ao matcher, antes/depois da candidata; comparar a identidade da ocupação de cada marca. | **Não sei**; guarda controla se a paridade permite plantar (`materializacao.py:436`, `:816`). |
| `ponto/propositor.py:315` | Qual primeiro furo posicional, ignorando tipo gravado (`:261`, `:320`)? | Ler primeiro marco `missing` do matcher sobre o turno completo. Há ainda uma inferência própria de pausa60±5min antes da atribuição (`:295`, `:311`). | **Não sei**; inferência de pausa e recusa de noturno também fazem parte da saída (`:276`, `:295`). |
| `ponto/propositor.py:351` | Dia está completo posicionalmente? | Mesmo juiz, projetando ausência de furos; hoje coleta template e batidas por dia civil (`:337`, `:345`) e chama `primeiro_furo_posicional`. | **Não sei**. Chamador: `chamados/reconciliador.py:112`; outro consumidor do primeiro-furo: `escala/servico_jornada.py:442`. |
| `ponto/management/commands/propor_reorganizacao.py:107` | Qual reorganização/custo/sobras/furos propõe para o dia? | Consumir resultado/projeção canônica; hoje remonta slots e marcas da célula (`:98`, `:101`) e chama DP paralela. | **Não sei**; imprime custo/sobras/furos (`:111`). |
| `ponto/services/cartorio.py:485` | Marco do DNA ausente da régua pode acender por uma hora real livre a≤30min? | Matcher com todos os marcos do DNA e batidas; ler ocupação exclusiva. Hoje escolhe `_perto` e acende sem tipo real (`:486`, `:491`). | **Não sei**; afeta diretamente `acesa/luz`, não só apresentação (`:491`). |
| `chamados/services/desvio_escala.py:74` | Dia cheio casa por posição; incompleto pelo marco mais próximo≤4h; quais medianas por marco (`:74`, `:78`, `:83`, `:86`)? | Matcher por dia/turno, depois agregar deltas dos marcos ocupados. | **Não sei**; muda universo de amostras e potencialmente `deslocado` (`:89`, `:90`). |
| `chamados/services/declaracao_texto.py:131` | Hora declarada casa com único marco **faltante**≤240min, sem quase empate<30min (`:134`, `:135`, `:137`)? | Matcher com candidata e concorrentes; localizar marco da candidata e preservar resposta de indeterminação quando o juiz não atribuir. | **Não sei**; hoje deriva motivo/tipo da escolha (`:292`, `:296`, `:297`). Outro chamador: `:573`. |
| `chamados/services/materializacao.py:274` | Qual tipo do marco mais próximo da resposta, circular≤240min (`:277`, `:280`)? | Matcher com resposta candidata e turno; projetar tipo do marco ocupado. Hoje o marco explicitamente cobrado prevalece sobre essa aproximação (`:257`, `:291`). | **Não sei**; pode mudar tipo e a indicação de divergência (`:283`, `:291`). |
| `chamados/detector_anomalias.py:210` | Qual slot/categoria de desvio recebe a batida? Já chama matcher, mas **desliga papel** com `derivar=False`. | Passar `_turno_da_batida` em `turno=`; ele já existe no coletor (`:202`). | **Não sei**; pode trocar `informativa`, `ok` ou desvio de borda (`:214`, `:223`, `:226`). Está declarado como segunda resposta pendente (`:206`). |

**Já delegam ao juiz; não são DP paralela:**

- `escala/utils.py::marco_ocupado_pela_batida:353` chama `_match_marcos` em `:393` e transcreve `(rótulo, delta)` em `:399`. Substituir a transcrição: **não**; alimentar turno completo exige trabalho adicional, pois assinatura atual não recebe turno (`:353`).
- `escala/regua_defesa.py::cumpriu_a_volta_do_intervalo:492` usa essa transcrição, mas alimenta só entrada/saída de **um período** (`:491`). Saída atual é rótulo `hfi` e tolerância (`:493`, `:495`); passar turno completo: **não sei** sem DIFF. O resíduo está registrado separadamente em `docs/BACKLOG.md:757`.
- `ponto/calculador/regras.py:251` delega a mesma pergunta de volta; o ramo de **saída de intervalo** ainda usa proximidade em `:259`. Substituição dessa decisão por papel do matcher: saída **não sei** sem DIFF.

**Outros sítios encontrados, fora da lista nominal de `BACKLOG:756`:**

- `ponto/turnos.py::decidir_tipo_estrito:2248`: escolhe melhor marco de tipo oposto≤10min e próprio>30min (`:2261`, `:2267`, `:2270`). Chamadores: `ponto/services/flip_auto.py:300`, `ponto/services/veredito_celula.py:242`, `chamados/management/commands/auditar_disputas.py:119`. Matcher com turno responde **ocupação**; manter/trocar política de flip não é automaticamente equivalente. Saída: **não sei**.
- `ponto/turnos.py::tipo_por_marcos:2275`: muda tipo retroativo por proximidade a exatamente um tipo de marco (`:2289`, `:2294`, `:2296`). Chamadores desse sítio: **nao li**. Substituição por papel ocupado: **não sei**.
- `ponto/services/veredito_celula.py:266`: escolhe batida vizinha≤3h para gerar horário proposto por duração de intervalo (`:263`, `:267`). É geração de candidata; a proposta depois passa por simulação de grade (`:273`). Equivalência com escolha pelo juiz: **não sei**.
- `ponto/turnos.py:2339`: usa marco mais próximo para sugerir o **próximo tipo ainda não batido** (`:2330`, `:2341`). Não é atribuição de batida existente; substituição direta pelo matcher: **não provado**.

**Motor — somente lista, “!”:**

- **!** `ponto/motor_calculo_v2.py:2016`, `:2028`: helper de proximidade.
- **!** `ponto/motor_calculo_v2.py:2055`: pergunta de volta do intervalo por período.
- **!** `ponto/motor_calculo_v2.py:2113`: saída de intervalo por proximidade.
- **!** `ponto/motor_calculo_v2.py:3000`: atribuição da Zona5 pelo matcher sem turno explícito.

Censo completo de código fora dos sítios nomeados, suas delegações e buscas descritas acima: **não provado**; não li integralmente todos os módulos de `app/`.

## 29/10 07:44 · sessao f9ba1355efad

T10 — fonte lida por `git show b3bf529e:app/...`, sem execução de código, testes ou banco.

**(a) A justificativa aprovada já entra na ata neste commit.** [nao verificado]

- `app/ponto/services/cartorio.py:874` monta a grade; `app/escala/utils.py:1785` aplica `_marcar_ausencias`.
- `app/escala/utils.py:1157` consulta `Justificativa(status='aprovada')` pelo dia; `:1200` troca colunas `missing` por `None`, `:1202` limpa órfãs e `:1203` grava `dia['justificado']=True`.
- `app/ponto/services/cartorio.py:602` lê **esse flag**, sem nova consulta de justificativa; `:608` acende os marcos não acesos, `:609` deixa `luz=None`, `:610` grava `dispensada='justificativa'` e `:612` recalcula `n_missing`. O ponto de entrada da ATA-NAO-DISPENSA-JUSTIFICADA é, portanto, **já existente**, em `dispensar_por_ausencia`, aplicado à ata em `:1149` e `:1407`.
- A leitura direta de `Justificativa` em `cartorio.py:279` pertence a **houve_fato_novo**, não à produção das lâmpadas.
- **Gatilho não garante relavra:** o `post_save` de justificativa chama julgamento em `app/ponto/signals.py:161`, `:165`, mas `impressao_insumos` não contém justificativa/`justificado` em seus componentes (`cartorio.py:314`, `:323`, `:329`, `:334`, `:342`, `:352`, `:367`, `:375`). Se os demais componentes não mudarem, o julgamento pula em `:1092`. Ausência de efeito em produção por esse caminho: **não provado sem execução**.

**(b) Uma entrada, nenhuma saída, dia encerrado: não basta para concluir `concorde`.**

| Passo | Evidência |
|---|---|
| Cartório conta a entrada casada como batida do dia | `app/ponto/services/cartorio.py:1101` |
| Passa o encerramento decidido pelo juiz | `cartorio.py:1117`; carregador em `:850` → `app/ponto/turnos.py:1978` |
| Uma entrada sem par tem realizado zero; `REALIZADO_ZERO_COM_TURNO` exige pelo menos duas células OK | `app/ponto/supra_juiz.py:256`; constante `MIN_CELULAS_OK=2` em `:48` |
| Sem realizado nem órfã, `_tem_fato_dia` é falso | `supra_juiz.py:348` |
| **Mas o ramo específico já cobre esse caso:** batida presente + realizado zero + encerrado + trabalho + previsto + sem cobertura aprovada + missing + sem defeito de âncora produz `FURO_PARCIAL` | `supra_juiz.py:394`, `:399` |
| Com justificativa aprovada, a grade remove os `missing`; `_n_missing` passa a zero e esse ramo não entra | `app/escala/utils.py:1200`; `supra_juiz.py:347`, `:395` |
| Sem outros protestos e sem fio vivo, `_vd` permanece `None`; `[]` deriva `concorde` | `cartorio.py:1121`, `:1141`; `app/ponto/portas/celula.py:652`; `app/escala/models.py:1595` |
| Se houver fio vivo, o cartório força `cobrado` mesmo com `cods=[]` | `cartorio.py:1141`, `:1142` |

Logo: **o caminho normal sem justificativa, com saída missing e dia efetivamente encerrado, acusa `FURO_PARCIAL`; `concorde` é alcançável com justificativa aprovada limpando o furo, desde que não existam outros protestos nem fio vivo** (`supra_juiz.py:394`; `utils.py:1200`; `cartorio.py:1141`).

`marcos_pendentes` não significa “batida faltante”: lê somente `lampada['pendente']` (`app/ponto/services/lastro.py:138`, `:148`). Esse carimbo nasce quando o cartório troca marco ainda não vencido de `acesa=False` para `acesa=None, pendente=True` (`cartorio.py:703`, `:704`). No dia totalmente vencido, essa proteção pode estar vazia mesmo com saída ausente; a saída ausente permanece `missing` e é o ramo `FURO_PARCIAL` acima que a acusa (`supra_juiz.py:394`).

**(c) O261 não conta célula `concorde`, mesmo com chamado vivo.**

- `app/chamados/services/vigia_fio.py:184` parte de chamados `VIVOS` com célula.
- `:199` exige módulo de `FURO_MODULES`; a lista é `batida_ausente`, `turno_aberto`, `intervalo_pendente`, `entrada_bloqueada_turno_aberto` (`app/chamados/catalogo/furo_modulos.py:15`).
- `vigia_fio.py:226`–`:227` admite **somente células cujo veredito pertence a `VEREDITOS_ACUSAM`**. `concorde` não pertence à tupla (`app/escala/models.py:1735`).
- Exclui dia com pergunta viva (`vigia_fio.py:258`), hoje/futuro (`:261`), decisão do dia oferecida ao admin (`:266`), ausência aberta (`:273`) e aceite de folga validado (`:279`).
- O restante soma **uma célula/dia** em `fio_morto` (`:286`). Soma também em `fio_morto_cobrando` se houver chamado do vocabulário em `COBRANCA` (`:200`, `:288`); `COBRANCA=('aberto','em_analise')`, enquanto `registrado` é apenas vivo (`app/chamados/catalogo/motor.py:24`, `:29`).

**Não é a mesma classe do `concorde` descrito em (b): ele é excluído antes da avaliação do fio** (`vigia_fio.py:227`). Se a mesma geometria de uma entrada/saída faltante resultar em `furo`, poderá entrar, mas só satisfazendo todos os demais filtros acima (`supra_juiz.py:399`; `vigia_fio.py:258`–`:286`).

_33 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
