# CODEX-ESTADO

> Ultima resposta processada: **05/11/2026 23:22** · publicado em **05/11/2026 23:22** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 05/11 23:22 · sessao 299368d3907f · - · - · origem=Ronald (tmux)

- **Dia D e vínculo:** `app/escala/alimentacao.py:228` define o juiz por dia; a geradora da célula é autoridade, sem escolher pelo vínculo ativo ou mais recente (`:231`). A geradora vale dentro da vigência, inclusive vínculo hoje inativo (`:272`). As comparações incluem os extremos (`:282`).

- **Competência 21→20:** competência 10/2026 = 21/09/2026–20/10/2026, por contrato escrito em `app/ponto/janelas.py:10`. A competência consultada corresponde à data do fato, sem usar mês civil (`app/ponto/janelas.py:75`).

- **Cross-meia-noite:** a retenção lê a célula soberana e seu dia; proíbe remontar o turno por janela de timestamps. Universo `[ini,fim]`, sem véspera (`app/folha/export.py:1168`). Isso fixa a identidade de D, mas não escreve se a retirada do passe no meio do turno fraciona a isenção ou preserva o turno inteiro: **[nao verificado]**.

- **Sem trilha:** a regra manda isento sem trilha desde a admissão, com texto exato **“desde a admissao -- flag sem trilha”**, e futuras trocas abrem/fecham a janela pela porta da quarentena (`app/docs/CORTES.md:352`). Campo da admissão: `app/colaboradores/models.py:324`.

- **Legado textual:** há registro explícito de trilha somente na descrição, anterior à estruturação dos JSON (`app/colaboradores/views.py:178`; `app/colaboradores/tests/test_passe_livre_trilha.py:6`). Portanto JSON vazio não prova ausência de trilha. A gramática completa de formatos antigos aceitos, ou precedência em contradição entre texto e JSON: **[nao verificado]**.

- **Concorrência e intermediário:** escritor salva flag em `app/colaboradores/views.py:176`, depois chama auditoria em `:183`. A porta apenas atribui e salva em `app/colaboradores/services/colaborador.py:55`. O comentário existente admite que falha da trilha não desfaz a decisão (`app/colaboradores/views.py:188`). O corte posterior exige trilha em toda troca (`app/docs/CORTES.md:352`). Regra de visibilidade intermediária, isolamento, desempate entre mudanças simultâneas e estado vencedor: **[nao verificado]**. Não transformar o comentário best-effort em esperado normativo da nova obra.

- **Horizonte/exportada:** L-124 preserva foto anterior ao horizonte até fato novo, com backfill único separado (`app/docs/LEIS.md:145`; `app/docs/CORTES.md:419`). O aval fornecido é mais específico ao proibir “tocar exportada”; não inferir dessa lei geral autorização para alterar a exportada nesta obra.

Regras procuradas e não encontradas escritas:

- Inclusão/exclusão do dia de ligar/desligar às 11:32; evento exatamente à meia-noite; remoção e religação no mesmo dia: **[nao verificado]**. Procurado em `app/core/constants.py:13`, `app/docs/CORTES.md:348`, `app/docs/CORTES.md:352`, `app/colaboradores/views.py:145`, `app/colaboradores/tests/test_passe_livre_trilha.py:64`.
- Ordenação/desempate de `LogAuditoria.criado_em` iguais, trilhas contraditórias, incompletas ou fora de ordem: **[nao verificado]**. Modelo declara somente ordenação por `-criado_em` (`app/core/models.py:307`, `app/core/models.py:312`).
- Conversão do timestamp da troca para um dia efetivo de isenção: **[nao verificado]**. Fuso configurado São Paulo e `USE_TZ=True` não definem essa política (`app/config/settings/base.py:93`, `app/config/settings/base.py:95`).
- Sem trilha e admissão nula; não isento hoje com trilha ausente; comportamento para colaborador inexistente na pergunta histórica: **[nao verificado]**. Regra localizada restringe-se a “isento sem trilha” (`app/docs/CORTES.md:352`).
- Contrato de erro/retorno para concorrência e leitura entre salvar flag e gravar trilha: **[nao verificado]**. Sítios examinados: `app/colaboradores/views.py:173`, `app/colaboradores/services/colaborador.py:55`, `app/colaboradores/tests/test_passe_livre_trilha.py:96`.

## 05/11 23:22 · sessao fa42c7ac23d1 · - · - · origem=Ronald (tmux)

Sítio e contrato da porta:

- `PerguntaDisputa.carimbar`: `app/chamados/models.py:2562`. Comentário normativo em `app/chamados/models.py:2532`: `materializada_em`, `materializada_por`, `via_resolucao` são UM ato; carimbo não identidade só grava se o C6 dá a pergunta por morta; viva restaura tudo e retorna `False`. Docstring em `app/chamados/models.py:2563` determina retorno booleano e restauração em exceções do C6, lavra ou gravação.
- A candidata escolhida aparece literalmente em `app/docs/PROMPTS.md:1882`: “O261 item 3 PEP = candidata 1 (dentro de carimbar, atomic, rele a linha travada, C6 rejulga; ordem de trava = relavra_via.py:221 medida antes do verde; conferir chave de _pv_memo; RED intercalacao decisao humana x resposta nova)”.
- O registro anterior apresentava DUAS candidatas: releitura travada dentro da porta versus chamador trava e porta compara/recusa, `app/docs/RELATO.md:131`. Nesse registro, metade memória do item 3 já estava curada, metade trava permanecia parada.
- Outro apontamento ao item 3: `app/docs/BACKLOG.md:1154`, O261-E-F2-FILA, “carimbar sem select_for_update, carimbo em memoria apos erro pos-C6”.
- O antecedente concreto da concorrência está em `app/chamados/services/relavra_via.py:214`: instância lida como `sem_turno_aberto`, decisão humana `admin_decidiu` no meio; decisão deve usar a linha gravada relida travada. Trava existente em `app/chamados/services/relavra_via.py:221`.

Definições de campos para a tabela:

| Model | Campo | Definição |
|---|---|---|
| PerguntaDisputa | classe | `app/chamados/models.py:2201` |
| PerguntaDisputa | celula | `app/chamados/models.py:2209` |
| PerguntaDisputa | disputa | `app/chamados/models.py:2211` |
| PerguntaDisputa | motivo | `app/chamados/models.py:2216` |
| PerguntaDisputa | pergunta | `app/chamados/models.py:2217` |
| PerguntaDisputa | data_evento | `app/chamados/models.py:2218` |
| PerguntaDisputa | tipo_resposta | `app/chamados/models.py:2222` |
| PerguntaDisputa | resposta_colab | `app/chamados/models.py:2227` |
| PerguntaDisputa | respondida_em | `app/chamados/models.py:2228` |
| PerguntaDisputa | validada_em | `app/chamados/models.py:2229` |
| PerguntaDisputa | data_conferida_em | `app/chamados/models.py:2238` |
| PerguntaDisputa | recusa_motivo | `app/chamados/models.py:2321` |
| PerguntaDisputa | via_resolucao | `app/chamados/models.py:2322` |
| PerguntaDisputa | veredito_lavrado | `app/chamados/models.py:2346` |
| PerguntaDisputa | julgado_em | `app/chamados/models.py:2351` |
| PerguntaDisputa | janela_expira_em | `app/chamados/models.py:2357` |
| PerguntaDisputa | validada_por | `app/chamados/models.py:2775` |
| PerguntaDisputa | chamado_destino | `app/chamados/models.py:2783` |
| PerguntaDisputa | materializada_em | `app/chamados/models.py:2791` |
| PerguntaDisputa | materializada_por | `app/chamados/models.py:2795` |
| CelulaDia | classe | `app/escala/models.py:1635` |
| CelulaDia | colaborador | `app/escala/models.py:1681` |
| CelulaDia | data | `app/escala/models.py:1685` |
| CelulaDia | trabalha | `app/escala/models.py:1686` |
| CelulaDia | escala_geradora | `app/escala/models.py:1692` |
| CelulaDia | dna | `app/escala/models.py:1697` |
| CelulaDia | veredito | `app/escala/models.py:1770` |
| CelulaDia | veredito_via | `app/escala/models.py:1771` |
| CelulaDia | veredito_em | `app/escala/models.py:1772` |
| CelulaDia | ata | `app/escala/models.py:1778` |
| DisputaSupervisao | classe | `app/chamados/models.py:1640` |
| DisputaSupervisao | colaborador | `app/chamados/models.py:1666` |
| DisputaSupervisao | chamado | `app/chamados/models.py:1679` |
| EscalaColaborador | classe | `app/escala/models.py:871` |
| EscalaColaborador | colaborador | `app/escala/models.py:872` |
| EscalaColaborador | posto | `app/escala/models.py:873` |
| EscalaColaborador | tipo_escala | `app/escala/models.py:874` |
| EscalaColaborador | data_inicio | `app/escala/models.py:875` |
| EscalaColaborador | data_fim | `app/escala/models.py:876` |
| EscalaColaborador | ativa | `app/escala/models.py:877` |
| Empresa | dia_inicio_competencia | `app/colaboradores/models.py:46` |

Regras escritas que sustentam casos:

- Data: `app/chamados/models.py:2231`–`:2237` dizem que `data_evento` é a verdade; discordância com data no chamado bloqueia materialização até conferir.
- Vazio: docstring `app/chamados/models.py:2565`–`:2567` determina carimbar resposta/validação somente com texto do colaborador. `app/chamados/juizes.py:1249` define texto vazio como `(t or '').strip()`.
- Estado intermediário: `app/chamados/services/disputa_emissao.py:1435`–`:1440` registra corte PERGUNTA-EM-LIMBO: texto existente sem `respondida_em` só recebe instante testemunhado pela nota da trilha; já carimbada ou sem texto fica como está. É OUTRA porta: `carimbar_resposta_pela_trilha`, `:1434`.
- Concorrência: `app/docs/PROMPTS.md:1882`; contrato de linha relida travada + C6 rejulga. O retorno exato do ato obsoleto não é decidido ali.
- Prioridade humana: `app/chamados/juizes.py:1151`–`:1157` registra HX-ADMIN-DECIDE-O-DIA, corte 26/08: resposta vence tudo menos decisão explícita do admin sobre o MESMO dia na célula; resposta vira histórico com trilha, não some.
- Resposta/marco: `app/chamados/juizes.py:1159`–`:1161` registra lei 25/10: marco aceso, resposta e luz concordantes pela régua e dia que não acusa dispensam validação; discordância ou impossibilidade de dizer segue viva.
- Teto: L-073, `app/docs/LEIS.md:100`, data não basta no cross meia noite; comparar instante ou exigir fato encerrado. Corte PERGUNTA-VIVA-TETO e complemento CLASSE-4 em `app/chamados/juizes.py:1185`–`:1193`: veredito até teto com marco pendente não absolve; o fato do próprio marco permite absolver mesmo com restante pendente.
- Competência: L-026 em `app/docs/LEIS.md:77`: 21→20 pelo campo da empresa, nunca mês civil nem 21 cravado. Corte K8 em `app/docs/CORTES.md:216`: tranca pergunta competência da DATA DO FATO. O próprio corte distingue `PeriodoFechado` da empresa e `FechamentoMensal` do colaborador.
- Vínculo: corte de nascimento do juiz em `app/docs/CORTES.md:284`, pergunta “qual vínculo vale neste dia?”. Proveniência da célula em `app/escala/models.py:1692`.
- Legado: `app/chamados/models.py:2277` registra B2-LEGADO-SEM-LASTRO: carimbo histórico de via vazia sem testemunha nem destino não absolve e não se refaz.
- Memo: contrato auto invalidante em `app/chamados/juizes.py:781`–`:787`: chave inclui campos mutáveis lidos e percebe alteração em memória. Chave atual em `:811`–`:819`, memo em `:822`. Para esperado, usar contrato, não composição atual da chave.
- Exceção/lavra/save: docstring da porta `app/chamados/models.py:2571`–`:2574` exige restaurar instância e veredito lavrado; exceção sobe.

Lacunas documentadas:

- `app/docs/RELATO.md:6492` lista divergências de desenho: ato obsoleto `False` versus exceção, identidade preexistente sobrescrita, alcance de atomic/restauração, geradora fora da vigência; e explicita ausência de lei para `ata={}`, `trabalha=True`, veredito não acusador (`_celula_furada=None` versus C6 `False`).
- Não encontrei expansão textual de “PEP” nos cinco documentos requeridos; as ocorrências `PEP 562` em `app/docs/RELATO-ARQUIVO.md:17996` e `:28267` são outro contexto (compatibilidade de importação de módulo).

## 05/11 23:22 · sessao 55764b590581 · - · - · origem=Ronald (tmux)

- **Campos da entrada**: `Colaborador.isento_ponto` — `app/colaboradores/models.py:294`; `data_admissao` — `:324`. `LogAuditoria.usuario` — `app/core/models.py:295`; `acao` — `:296`; `modelo` — `:297`; `objeto_id` — `:298`; `descricao` — `:299`; `valor_antes` — `:300`; `valor_depois` — `:301`; `criado_em` — `:307`. Batidas: `colaborador`, `timestamp`, `tipo` — `app/ponto/models.py:22`, `:23`, `:24`.

- **Escritor**: `passe_livre` carrega o colaborador em `app/colaboradores/views.py:156`, define `novo_valor = (acao == 'add')` em `:157` e grava a flag pela porta em `:176`. A porta salva apenas os campos informados em `app/colaboradores/services/colaborador.py:55`–`:62`. Motivo é obrigatório nas duas direções, com mínimo de 10 caracteres, em `app/colaboradores/views.py:158`–`:172`.

- **Formato da trilha**: `acao='editar'`, `modelo='Colaborador'`, `objeto_id=colab.pk`; descrição `Passe livre (isencao Art.62): {antes} -> {novo_valor}. Motivo: {_motivo}`; JSON anterior `{'isento_ponto': antes}` e posterior `{'isento_ponto': novo_valor, 'motivo': _motivo}` — `app/colaboradores/views.py:183`–`:186`. `criado_em` é horário de criação do registro, `auto_now_add=True` — `app/core/models.py:307`.

- **Legado textual**: a própria cópia explica que o antes/depois anteriormente existia apenas na descrição; procurar somente `valor_depois['isento_ponto']` perde essa trilha — `app/colaboradores/views.py:178`–`:182`; `app/colaboradores/tests/test_passe_livre_trilha.py:6`–`:14`. Caso adversarial pertinente: JSON nulo, descrição histórica válida, dia estritamente entre ligação e remoção; o esperado deve vir do aval “vigência lida da trilha”, sem assumir que JSON nulo significa ausência de trilha.

- **Sem trilha**: regra escrita em `app/docs/CORTES.md:352`: isento sem trilha = desde a admissão; texto exigido na lista: `"desde a admissao -- flag sem trilha"`. Toda troca futura deve gravar trilha e abrir/fechar janela pela porta. A distinção entre janela aberta e fechada para item sem dia também está nessa linha. A regra de quarentena distingue cobrança de cálculo em `app/docs/CORTES.md:348`.

- **Dois períodos**: a regra de reentrada é o literal do aval fornecido: `"quem re-entra no passe (col41) respeita os dois periodos"`. A cópia prova que ligar e desligar deixam duas linhas no teste `app/colaboradores/tests/test_passe_livre_trilha.py:96`–`:101`. Datas efetivas completas de col41 no dado atual: **[nao verificado]**.

- **Idempotência**: mesma decisão não deve produzir nova linha; regra explicitada em `app/colaboradores/tests/test_passe_livre_trilha.py:103`–`:107`, condicionada à mudança de valor em `app/colaboradores/views.py:173`.

- **Concorrência/intermediário**: gravação da flag ocorre antes da tentativa de trilha — `app/colaboradores/views.py:176`, `:183`. A função e a porta lidas não apresentam transação conjunta nem `select_for_update` — `app/colaboradores/views.py:144`–`:203`; `app/colaboradores/services/colaborador.py:55`–`:62`. `core.observ.evento` declara best effort em `app/core/observ.py:96`–`:102` e usa savepoint próprio da trilha em `:124`–`:138`. O comentário do escritor determina que falha de log não desfaz a isenção — `app/colaboradores/views.py:188`–`:194`. Para leitura entre gravação da flag e da trilha, desempate de trocas simultâneas e ordem de `criado_em` empatado: **regra não decide [nao verificado]**. L-007 exige usuário, antes/depois e motivo — `app/docs/LEIS.md:65`.

- **Lacunas procuradas**: não encontrei implementação de `colab_em_quarentena`/`inicio_da_isencao` no código desta cópia **[nao verificado]**; o relato aponta implementação em outra cópia e reprovação — `app/docs/RELATO.md:1204`. Não encontrei regra para admissão nula sem trilha, fronteira inclusiva/exclusiva na data da troca, remoção e religação no mesmo dia, prioridade entre texto e JSON contraditórios, ou horário efetivo distinto do horário registrado **[nao verificado]**.

- **Atenção ao censo**: o evento atual de flag ligada ainda chama o silenciador depois do commit — `app/chamados/signals.py:213`–`:224`; o silenciador consulta o flag atual — `app/chamados/services/silencio_isentos.py:24`. Isso não fornece uma regra histórica por D; a fonte de esperado permanece o aval e o corte posterior.

## 05/11 23:22 · sessao f8f2fcc97bf1 · - · - · origem=Ronald (tmux)

Regras localizadas:

- Contrato do instrumento: CLI `bin/quem_chama.py <rev-range> --obra <id>`; saída `logs/<obra>/QUEM_CHAMA.txt`; por arquivo tocado, AST de imports e símbolo mudado, citações em código shell/Python/cron/template e leitores de artefatos escritos por shell; lista é fato, sem autoridade de juiz; leitura concluída retorna `rc 0`; revisão inválida ou árvore ilegível retorna `rc 2`, sem arquivo; nomes dinâmicos ficam fora com declaração explícita; duas rodadas produzem mesmo SHA. [app/docs/RELATO.md:178]
- Golden escrito: intervalo do commit `af710af2` precisa identificar `core/esteira_vigia.py`, `core/integrador_lote.py` e `bin/vigia_logins.sh`. [app/docs/RELATO.md:178]
- Lista positiva de tipos varridos; toda faceta fora dela precisa aparecer como `nao varrido`, nunca como zero chamador. [app/docs/RELATO.md:114]
- Python embutido em `.sh` não deve ser varrido; teto de duas voltas opus. [app/docs/PROMPTS.md:1853]
- Classes de shell que motivaram reprovação: heredoc, `python3 -c`, `manage.py shell --command`. [app/docs/RELATO.md:126]
- Cabeçalhos `---`/`+++` dentro de hunk não são cabeçalhos de arquivo; chamador no mesmo arquivo conta quando está fora da definição. [app/docs/RELATO.md:146]
- Símbolos/módulos apagados ou renomeados exigem consideração do lado velho; management command por nome precisa alcançar `config/crons.py` e `call_command`. [app/docs/RELATO.md:147]
- Um achado “importa de” não pode satisfazer indevidamente a expectativa da faceta de símbolo; essa sobrevivência de mutante está registrada na reprovação. [app/docs/RELATO.md:126]
- L-111 manda apagar sítio sem chamador de produção, testes exclusivos e pendente no mesmo commit; o contrato QUEM-CHAMA continua sendo somente lista de fatos. [app/docs/LEIS.md:137; app/docs/RELATO.md:178]

Limitação escrita: ferramenta está somente na raia `wt-qc-224741`, fora do HEAD; censos anteriores a situaram em `d1354f77`. [app/docs/RELATO.md:6428; app/docs/RELATO.md:6454]

Não encontrei regras específicas do QUEM-CHAMA para data, competência 21→20, passagem da meia-noite, troca de vínculo, intervalo vazio, concorrência sobre o mesmo arquivo de saída ou atomicidade durante estado intermediário. A fonte de escopo consultada define censo por revisão, AST e citações, sem decidir essas condições. [app/docs/RELATO.md:178; nao verificado]

Não encontrei `CODEX-RED-POR-EXEMPLO`, `Specification by Example`, os arquivos `bin/quem_chama.py`/`bin/tests/test_quem_chama.sh`, cópia da raia ou laudos round3/round4 disponíveis para leitura nesta cópia. Suas definições internas de modelos/campos permanecem `[nao verificado]`; a ausência da ferramenta no HEAD também está registrada. [app/docs/RELATO.md:6428; nao verificado]

## 05/11 23:22 · sessao b84a151c131f · - · - · origem=Ronald (tmux)

Referências centrais:

- `app/docs/PROMPTS.md:1853`: aval MANHA-0511, literal: **“7 O261 item 3 PEP no carimbar”**.
- `app/docs/RELATO.md:131`: O261-E-F2-FILA item 3 tem **metade-memória** (desfazer a lavra na porta `veredito_lavrado`) e **metade-trava**, com duas candidatas registradas: **reler linha travada e C6 rejulgar dentro de `carimbar`**; ou **chamador trava e porta compara e recusa**.
- `app/docs/PROMPTS.md:1882`: complemento PD-TRIAGEM-CHAT 05/11 17:1x explicita: **“O261 item 3 PEP = candidata 1 (dentro de carimbar, atomic, rele a linha travada, C6 rejulga; ordem de trava = relavra_via.py:221 medida antes do verde; conferir chave de _pv_memo; RED intercalacao decisao humana x resposta nova)”**.
- `app/docs/BACKLOG.md:1747`: resumo confirma **“PEP=candidata 1”**.
- A expansão da sigla **PEP** não foi encontrada nos cinco documentos, nem em `CLAUDE.md`, `app/docs/LEIS.md` e `app/docs/GLOSSARIO.md`. [não verificado]

Outras leituras escritas de “item 3”, sem escolher:

- `app/docs/CORTES.md:64` e `:253`: item 3 de FIO-MORTO-E-ESTRUTURAL designa **três leitores**: `disputa_emissao`, `supra_juiz`, `_C1` de `core/juizes`.
- `app/docs/BACKLOG.md:1450`: item (3) do aval 18/10 é **contador “fio morto” no ESTADO, esperado 0 fora hoje**.
- `app/docs/PROMPTS.md:1280`: item 3 referido no aval 19/10 é **zero na classe ALVO (56); 45 para GRUPO-D; seis `plantio_fora_do_slot` como BATIDA**.
- `app/docs/RELATO.md:630`: usa “O261 item 3” ao referir a lâmpada **`cobrado`**.
- `app/docs/RELATO-ARQUIVO.md:42924` e `:42937`: item 3 do corte = **três leitores não migrados**, reiterando CORTES.
- `app/docs/RELATO-ARQUIVO.md:17996` e `:28267`: **PEP 562** é outro uso escrito do termo, ligado ao `__getattr__` de módulo; não define o rótulo da obra.

Sítio e contrato escrito do carimbo:

- `app/chamados/models.py:2532`: “PORTA UNICA DO CARIMBO”.
- `app/chamados/models.py:2533`: `materializada_em`, `materializada_por`, `via_resolucao` constituem **um ato**.
- `app/chamados/models.py:2536`: `carimbar` aplica em memória, pergunta ao C6 e grava somente se a pergunta ficar morta; viva restaura e retorna `False`.
- `app/chamados/models.py:2562`: definição de `PerguntaDisputa.carimbar`.
- `app/chamados/models.py:2565`: `resposta` admite `None`, `responder`, `validar`; somente texto do colaborador autoriza completar os campos de resposta/validação vazios.
- `app/chamados/models.py:2570`: C6 julga **com carimbo e resposta aplicados**.
- `app/chamados/models.py:2571`: exceção do C6 ou posterior na lavra/save desfaz memória; exceção sobe.
- `app/chamados/services/relavra_via.py:214`: decisão sobre **linha gravada**, não instância recebida; registra a corrida de `admin_decidiu` sobrescrito.
- `app/chamados/services/relavra_via.py:221`: `select_for_update(of=('self', 'disputa'))`.

Lacunas expressamente registradas:

- `app/docs/RELATO.md:6492`: comparação PEP lista divergências sem decisão normativa: ato obsoleto `return False` versus `raise`; identidade preexistente sobrescrita; alcance do `atomic`/restauração; **`ata={}`, `trabalha=True`, veredito não acusador — `_celula_furada=None` versus C6 `False`, “sem lei que resolva”**; geradora fora da vigência. Esse documento manda o caso da ata vazia ao construtor como pergunta de lei, caso precise decidir.

## 05/11 23:22 · sessao 905bdd82c713 · - · - · origem=Ronald (tmux)

Campos e regras lidos, sem executar aplicação/banco nem alterar arquivos.

- `ChamadoColaborador`: `colaborador` — `app/chamados/models.py:32`; `modulo_origem` — `:40`; `contexto_json` — `:41`; `celula` — `:44`; `status_local` — `:46`; `via_encerramento` — `:48`; `resolvido_em` — `:54`; `criado_em` — `:61`.
- `DisputaSupervisao`: `colaborador` — `app/chamados/models.py:1666`; `contexto` — `:1675`; `chamado` — `:1679`, relação reversa `disputa_origem` — `:1684`; `fechada_em` — `:1687`.
- `PerguntaDisputa`: `celula` — `app/chamados/models.py:2209`; `disputa` — `:2211`; `motivo` — `:2216`; `pergunta` — `:2217`; `data_evento` — `:2218`; `tipo_resposta` — `:2222`; `resposta_colab` — `:2227`; `respondida_em` — `:2228`; `validada_em` — `:2229`; `data_conferida_em` — `:2238`; `chamado_destino` — `:2783`; `materializada_em` — `:2791`; `chamados_relacionados_ids` — `:2805`.
- `FollowupChamado`: `chamado` — `app/chamados/models.py:1598`; `autor_tipo` — `:1599`; `conteudo` — `:1601`; `criado_em` — `:1604`.
- `EscalaColaborador`: `colaborador` — `app/escala/models.py:872`; `posto` — `:873`; `tipo_escala` — `:874`; `data_inicio` — `:875`; `data_fim` — `:876`; `ativa` — `:877`.
- `CelulaDia`: `colaborador` — `app/escala/models.py:1681`; `data` — `:1685`; `trabalha` — `:1686`; `escala_geradora` — `:1692`; `dna` — `:1697`; `regenerada_em` — `:1718`; `dna_anterior` — `:1726`.
- `TurnoMaterializado`: `colaborador` — `app/ponto/models.py:1203`; `data_turno` — `:1205`; `entrada` — `:1206`; `saida` — `:1208`; `aberto` — `:1210`; `cross_meianoite` — `:1211`; `dt_fim_previsto` — `:1212`.
- `FechamentoMensal`: `colaborador` — `app/ponto/models.py:405`; `mes` — `:408`; `ano` — `:409`; `status` — `:410`.
- `Empresa.dia_inicio_competencia` — `app/colaboradores/models.py:46`.
- `Batida.timestamp` — `app/ponto/models.py:23`.

Regras para origem dos casos:

- **L-026**, `app/docs/LEIS.md:77`: “COMPETENCIA CORRE DO DIA 21 AO 20 (`Empresa.dia_inicio_competencia`), nunca mes civil; nenhum juiz usa 21 cravado”. Para corte 21, `20/10/2026` pertence a `21/09–20/10`; `21/10/2026` pertence a `21/10–20/11`. A janela está definida em `app/ponto/janelas.py:10`.
- **L-073**, `app/docs/LEIS.md:100` e `CLAUDE.md:185`: “teto por DATA nao basta”; só comparar instante ou exigir fato encerrado. Permite caso noturno D 19:00–D+1 07:00 avaliado D+1 00:01: passar de data não basta para tratar o turno como encerrado.
- **L-085**, `app/docs/LEIS.md:117`: “A JORNADA PERTENCE AO DIA DE INICIO”; a cauda após meia-noite herda o dia inicial.
- Vínculo do dia, regra escrita em `app/escala/alimentacao.py:231`: a resposta é `CelulaDia.escala_geradora`; não se decide por ativo ou mais recente (`:233`).
- Corte E1 literal em `app/escala/alimentacao.py:272`: “a geradora da celula so vale DENTRO da vigencia (data_inicio..data_fim) do vinculo que a gerou; fora dela a juiza devolve None e o dia e dono CADASTRO”. Inativo cuja vigência cobre D continua valendo (`:276`); outro vínculo não pode assumir silenciosamente (`:278`).
- **L-021**, `app/docs/LEIS.md:72`: `data_fim` nunca anterior a `data_inicio`; o resíduo é recusado com PK no log.
- Data divergente da pergunta, comentário normativo em `app/chamados/models.py:2231`: verdade da pergunta é `data_evento`, não `contexto_json['data_turno']` do destino; enquanto ninguém conferir, materialização bloqueada (`:2236`).
- Legado sem célula é explicitamente admitido no campo: `app/chamados/models.py:42`–`:44` (“dia sem CelulaDia (legado <21/07)”). Isso não escreve uma exceção de O168 para legado.
- Legado de `turno_aberto_24h` sem data: `app/chamados/catalogo/modulos.py:18`–`:20` declara acervo anterior à cura; emissor atual grava `data_turno`. Não determina como O168 resolve data ausente.
- Guarda tem enunciado escrito em `app/ponto/supra_juiz.py:540`: cobrança sem furo só pode morrer no ato se não há conversa; pergunta respondida na origem ou destino é prova/trilha CLT e exige decisão humana. Esse enunciado não define conversa livre em `FollowupChamado`.

## 05/11 23:22 · sessao af99158c10c5 · - · - · origem=Ronald (tmux)

- Campos: `Batida` em `app/ponto/models.py:8`; `colaborador:22`, `timestamp:23`, `tipo:24`, `sequencia:25`, `escala:29`, `origem:45`, `canal:55`, `latitude:58`, `longitude:59`, `gps_accuracy:60`, `aprovada:63`, `retratada_em:67`, `timestamp_dispositivo:75`, `timestamp_gap_ms:76`, `sincronizado_em:77`, `foto:78`. Constraint de unicidade `(colaborador, timestamp)` em `app/ponto/models.py:106`.
- Identidade: `Colaborador.usuario` em `app/colaboradores/models.py:351`; API exige autenticação e toma `request.user.colaborador` em `app/api/views.py:398-402`. Identificador de colaborador no payload da fila: [não verificado].
- Entrada API: campos `tipo`, `latitude`, `longitude`, `gps_accuracy`/`accuracy`, `foto` em `app/api/views.py:432-436`; `timestamp_dispositivo` em `app/api/views.py:538`. Rota em `app/api/urls.py:67`. View HTML `/ponto/bater/` exige login e recupera colaborador em `app/ponto/views.py:241-249`; rota em `app/ponto/urls.py:11`.
- Horário: regra escrita diz que atraso de zero a 48 horas preserva a hora do aparelho (`app/api/hora_do_aparelho.py:62-67`, `:141-146`). Contrato de saída `(ts_efetivo, ts_aparelho, motivo)` em `:124-133`. Vazio devolve `(agora, None, None)` (`:132-139`); data impossível não deve produzir 500, usa `agora` (`:85-90`); qualquer entrada deve ser total, pior resultado `None`, significando usar `agora` (`:98-105`). Fora da janela retorna `agora`, preservando a hora lida e motivo (`:148-153`). O aval da obra exige hora do aparelho; se pretender aceitação irrestrita além de 48h, o conflito não é decidido pelo dado fornecido [não verificado].
- Competência: L-026 escrita em `app/docs/LEIS.md:77`: período 21–20 via cadastro da empresa, nenhum juiz fixa 21. Campo `Empresa.dia_inicio_competencia` em `app/colaboradores/models.py:46`; contrato da janela em `app/ponto/janelas.py:10-14`. Exemplos: corte 21, `20/11/2026` pertence a `21/10–20/11`; `21/11/2026` pertence a `21/11–20/12` conforme contrato `app/ponto/janelas.py:24-33`.
- Cross-meia-noite: sequência é posição na jornada do turno, sem retratadas, não contagem do dia do relógio; corte literal registrado em `app/api/views.py:582-583`. O comentário anterior explicita sincronização após meia-noite e erro de sequência do dia do servidor em `:578-581`.
- Vínculo trocado: `EscalaColaborador` em `app/escala/models.py:871`; `colaborador:872`, `posto:873`, `tipo_escala:874`, `data_inicio:875`, `data_fim:876`, `ativa:877`. `CelulaDia.colaborador` em `app/escala/models.py:1681`, `data:1685`, `trabalha:1686`, `escala_geradora:1692`. Regra escrita manda ler a geradora da célula, nunca escolher ativa ou mais recente (`app/escala/alimentacao.py:229-247`); geradora vale somente dentro da vigência, inclusive hoje inativa (`:272-278`). API documenta aplicação ao lote retroativo em `app/api/views.py:610-619`.
- Idempotência: contrato do chokepoint em `app/ponto/registro_batida.py:95-102`: mesmo tipo dentro de 60s retorna existente; sem colisão `(batida,None)`. Corte BATER-DUPLICATA-IDEMPOTENTE determina devolver a mesma chave já gravada inclusive outro tipo ou retratada (`:116-122`). Caso concorrente pode exigir no máximo uma linha pela constraint `app/ponto/models.py:106`; status de cada resposta ou tratamento da corrida: regra não decide.
- Foto/GPS: GPS obrigatório, ausência retorna 403 e código `gps_obrigatorio` conforme trecho escrito `app/api/views.py:448-466`. PWA sem selfie é cobrado no ato; APK aguarda 90 minutos, e canal legado NULL mantém prazo APK (`app/ponto/registro_batida.py:64-76`). Definições de entrada do juiz `origem`, `canal`, `tem_foto`, `decorrido_min` em `:80-84`.
- Dado legado: campo `Batida.canal=NULL` não é retropreenchido e pode representar batida anterior a 27/10/2026 (`app/ponto/models.py:46-51`). Destino de fila offline antiga sem identidade não consta nas regras examinadas: regra não decide.
- Estado intermediário: perda da resposta após gravação pode ser caso de reenvio da mesma chave, com devolução da batida existente pelo corte `app/ponto/registro_batida.py:116-122`; requisitos de reter/remover item da fila dependem do aval e do frontend, não deste trecho [não verificado].
- Não executei testes nem consultei banco/log [não verificado]. [nao verificado]

## 05/11 23:21 · sessao aecc38db7896 · - · - · origem=Ronald (tmux)

Fontes de regra confirmadas:

- `app/docs/PROMPTS.md:830-832`: literal O168 — “o cartorio nao fecha cobranca sem furo de chamado que tem conversa”; `:835-836`: retirar `--executar` somente depois da guarda no evento e `CSF_RETRATADO` medido em zero.
- `app/docs/PROMPTS.md:1875`: “pode_executar_csf no nucleo, DIFF sombra, >10% fila viva = PAREI, padrao=juiz unico”.
- `app/docs/PROMPTS.md:1882`: complemento O168 define denominador `painel.qs_na_fila`; mensagem livre é `FollowupChamado autor_tipo=colaborador`; DIFF separa resposta validada/materializada de resposta sem decisão humana; primeira classe sozinha `>10%` exige PAREI; guarda grava motivo na trilha; `CSF_RETRATADO=0` sozinho não prova.
- `CLAUDE.md:175`: idempotência observável — “2x = mesmo estado, 1 linha de trilha, 0 duplicata”.
- `CLAUDE.md:176`: coerência, determinismo e idempotência com o cadastro existente.
- `CLAUDE.md:185`: teto temporal por instante ou fato encerrado; data sozinha insuficiente no cross-meia-noite.
- `CLAUDE.md:20-22`: leitores consultam mesma autoridade; regra própria de derivação reprova.
- `app/docs/LEIS.md:136`: L-110 exige casos respondidos pela regra; onde ela não responde, registrar lacuna.

Lacunas relevantes:

- Ordem de serialização da concorrência e instante exato em que a conversa precisa ser relida: regra O168 específica não encontrada nas fontes pesquisadas; o trecho `atomic`/intercalação de `PROMPTS.md:1882` pertence ao item O261 PEP.
- Política de fechamento para conversa já validada/materializada: `PROMPTS.md:1882` define classe de medição e limiar, sem autorizar seu fechamento.
- Texto literal/formato do motivo da guarda: `PROMPTS.md:1882` exige motivo, mas não fixa conteúdo.
- Semântica de `respondida_em` preenchido com texto vazio para O168: não encontrada; `app/docs/CORTES.md:380` trata o legado na O261 e não define o conceito de conversa da O168.

## 05/11 23:21 · sessao 1b69673e0479 · - · - · origem=Ronald (tmux)

- Fila: IndexedDB `juliani-offline`, versão 2 (`app/static/pwa.js:50`); store `batidas_pendentes`, chave `id` incremental (`app/static/pwa.js:54`).
- Campos: `tipo`, `timestamp` (`app/static/pwa.js:114`); `timestamp_dispositivo` (`app/static/pwa.js:115`); `timestamp_gap_ms`, `latitude`, `longitude` (`app/static/pwa.js:116`); `gps_accuracy`, `gps_captured_at` (`app/static/pwa.js:117`); `foto` (`app/static/pwa.js:118`). Campo identificador do colaborador na fila: [não verificado].
- Sync envia `tipo` (`app/static/pwa.js:144`), coordenadas (`app/static/pwa.js:145`), `timestamp_dispositivo` (`app/static/pwa.js:147`) e foto (`app/static/pwa.js:152`). Preservação de GPS completo no envio não está demonstrada: precisão e instante existem na fila, mas não aparecem no FormData (`app/static/pwa.js:143`).
- Concorrência tem entradas observáveis pelo evento SW (`app/static/pwa.js:16`), evento online (`app/static/pwa.js:320`) e carga inicial (`app/static/pwa.js:328`). A função lê toda a fila antes dos POSTs (`app/static/pwa.js:130`); não encontrei trava de exclusão nessa função (`app/static/pwa.js:126`).
- Estado intermediário: resposta HTTP `ok` precede a exclusão local (`app/static/pwa.js:158`); a exclusão usa transação separada (`app/static/pwa.js:161`). Resposta `400` também exclui (`app/static/pwa.js:166`). Outros erros preservam a fila segundo comentário e tratamento (`app/static/pwa.js:177`).
- Sessão expirada: o sync usa cookie CSRF atual (`app/static/pwa.js:140`); resposta `ok` basta para tentar JSON e excluir (`app/static/pwa.js:158`). Destino da fila após expiração de sessão: regra não decide nos arquivos examinados [não verificado].
- Troca de usuário: as duas cascas fornecem somente meta `user-perfil=colaborador`, condicionada a `not is_staff`, sem ID (`app/templates/base.html:16`; `app/templates/base_app.html:16`). Destino da fila antiga: regra não decide nos arquivos examinados [não verificado].
- SW novo com fila antiga: nova versão diferencia CacheStorage por `CACHE_NAME` (`app/static/sw.js:1`); ativação apaga caches de outras versões (`app/static/sw.js:20`); fila permanece em IndexedDB com nome próprio (`app/static/pwa.js:50`). Política para migrar fila sem identidade: regra não decide nos arquivos examinados [não verificado].
- Duas cascas carregam o mesmo `pwa.js` (`app/templates/base.html:20`; `app/templates/base_app.html:20`). A escolha central usa `modo_app` e devolve `base_app.html` ou `base.html` (`app/core/context_processors.py:205`).
- Recarga de cinco minutos ocorre ao voltar à visibilidade após mais de `300000` ms, sem condição de conectividade (`app/templates/ponto/bater_ponto_v2.html:305`). Há outra recarga em bfcache (`app/templates/ponto/bater_ponto_v2.html:306`; `app/static/pwa.js:431`) e em troca de controller sem formulário sujo (`app/static/pwa.js:41`).
- Regra existente Offline Fallback: sem rede, cópia do cache; sem cópia, erro de rede, nunca `undefined` (`app/docs/GLOSSARIO.md:29`; `app/core/tests/test_sw_offline_fallback.py:8`). O teste usa o SW real (`app/core/tests/test_sw_offline_fallback.py:11`).
- Regra duas cascas: smoke de clique nas duas cascas quando toca SW/base (`app/docs/LEIS.md:99`, L-072).
- Navegação retorna antes do trecho que grava `/` e `/ponto/bater/` (`app/static/sw.js:45`; `app/static/sw.js:50`; `app/static/sw.js:68`; `app/static/sw.js:74`). `STATIC_CACHE` só lista pwa, manifesto e ícones (`app/static/sw.js:3`).
- App Shell no GLOSSARIO, regra explícita de migração da fila legada, texto/status de bloqueio por identidade divergente e comportamento de sessão expirada offline não foram encontrados nos arquivos examinados [não verificado].

## 05/11 23:21 · sessao 564d0876fc39 · - · - · origem=Ronald (tmux)

Regras escritas encontradas:

- `app/docs/PROMPTS.md:1870`: aval integral da obra; “casca offline sem dado pessoal, fila com identidade, sem recarga offline, envio unico com hora do aparelho, foto/GPS intactos, Safari iOS medido”; também exige contador PWA com hora do aparelho.
- `app/docs/PROMPTS.md:1876`: corte integral: “casca guardada no aparelho, sem dado pessoal; dado da rede ou ausente”; armadilha HTML de usuário em cache; identidade da fila na mesma obra.
- `app/docs/BACKLOG.md:1745`: PRONTO inclui “smoke 2 cascas (L-072), iPhone modo aviao”.
- L-072, `app/docs/LEIS.md:99`: smoke de clique Ronald nas DUAS cascas para alterações em SW/base.
- `app/docs/GLOSSARIO.md:29`: Offline Fallback: network-first, cache na falha, erro de rede sem cópia; nunca `undefined` no `respondWith`.
- `CLAUDE.md:143`: timestamps em UTC no banco; apresentação UTC-3.
- L-026, `app/docs/LEIS.md:77`, e L-090, `app/docs/LEIS.md:123`: competência pelo corte cadastrado da empresa, 21→20 por padrão; nunca mês civil ou 21 fixo.
- L-085, `app/docs/LEIS.md:117`: jornada pertence ao dia de início, inclusive cauda após meia-noite. Não equivale a deslocar o timestamp da batida.
- `app/docs/CORTES.md:284`: autoridade `vinculo_do_dia` responde qual vínculo vale no dia.
- `app/docs/CORTES.md:364`: corte exige leitor de data de saída usar vínculo do dia; RED troca no meio da competência.

Campos úteis encontrados:

- Fila IndexedDB: `app/static/pwa.js:50` versão 2; `:54` `id` autoincremental; `:114` `tipo,timestamp`; `:115` `timestamp_dispositivo`; `:116` `timestamp_gap_ms,latitude,longitude`; `:117` `gps_accuracy,gps_captured_at`; `:118` `foto`.
- `Batida`: `app/ponto/models.py:22` colaborador; `:23` timestamp; `:24` tipo; `:58` latitude; `:59` longitude; `:60` gps_accuracy; `:75` timestamp_dispositivo; `:76` timestamp_gap_ms; `:77` sincronizado_em; `:78` foto.
- `app/ponto/models.py:106`: unicidade declarada `(colaborador,timestamp)`.

Não encontrei regras específicas escritas para migração de fila antiga sem identidade, política exata na sessão expirada offline, chave/janela do “envio único”, recuperação após aceite com resposta perdida, descarte/retenção da fila na troca de usuário, versão mínima de Safari/iOS ou texto/status obrigatório dessas situações. Busquei os termos em `PROMPTS.md`, `LEIS.md`, `CORTES.md`, `GLOSSARIO.md`, `PLANO-PADROES.md` e `CLAUDE.md`. Para essas escolhas: “regra não decide”; só identidade, ausência de dados pessoais e envio único têm contrato explícito nas duas linhas do aval.

## 05/11 23:12 · sessao 7fcfc9f0d68a · C20-FIO-MUDO · verificador · origem=Code

**Não certificaria a obra como completa.** P3 continua parcial; a reabertura da disputa acrescenta risco de ordem de travas e efeitos sobre outros leitores; os cinco atos de perda ainda não têm cobertura demonstrada. (`app/chamados/models.py:461`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:513`, `app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:845`)

Execução dos testes, eficácia dos mutantes, reprodução das corridas e resultado em produção: **[não verificado]**. [nao verificado]

**1. P1–P6 e os descartes da triagem**

| Achado | Avaliação do delta |
|---|---|
| **P1 — eleição paralela da canônica** | **Resolve a eleição no grupo misto.** Agora reúne vivas locais e órfãs por motivo e consulta `canonica_do_grupo`; desapareceu a precedência automática da local. O ramo “todas daqui” ainda pula qualquer normalização, relevante na ressuscitação posterior. (`app/chamados/models.py:491`, `app/chamados/models.py:496`, `app/chamados/models.py:498`; `app/chamados/services/fio.py:873`) |
| **P2 — retratação antes da costura aceita** | **Resolve dentro da adoção.** Canônica externa só permite retratação após retorno positivo da porta; recusa dá `continue`. Não resolve o mesmo padrão no migrador, que continua retratando antes de reapontar. (`app/chamados/models.py:501`, `app/chamados/models.py:505`, `app/chamados/models.py:514`; `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`) |
| **P3 — transação da reabertura direta** | **Resolve em parte.** Serializa a adoção do mesmo chamado e relê seu estado, mas a leitura inicial, a transição e a ressuscitação de `reabrir()` continuam anteriores a essa unidade. (`app/chamados/models.py:462`, `app/chamados/models.py:1424`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`) |
| **P4 — duplicata respondida ainda viva** | **Resolve a seleção indevida e corrige a documentação; não resolve suas consequências nos demais leitores.** A adoção exclui `VIA_DUPLICATA`, enquanto C6 continua podendo considerá-la viva pela resposta. (`app/chamados/models.py:446`, `app/chamados/models.py:477`; `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`) |
| **P5 — continente fechado** | **Resolve em parte.** Reabre a disputa da canônica efetivamente movida. Não executa esse efeito quando a canônica já está aqui; tampouco demonstra acesso real pelo colaborador ou coerência dos outros chamados/perguntas do continente. (`app/chamados/models.py:499`, `app/chamados/models.py:512`, `app/chamados/models.py:514`; `app/chamados/tests/test_c20_fio_mudo.py:229`) |
| **P6 — R19a vacuamente verde sem cobrança** | **Resolve essa vacuidade.** Agora exige cobrança existente antes de conferir contagem e destino da validada. (`app/chamados/tests/test_c20_fio_mudo.py:272`, `app/chamados/tests/test_c20_fio_mudo.py:273`, `app/chamados/tests/test_c20_fio_mudo.py:275`) |

Os motivos dos descartes:

- **R19c:** sustenta apenas adiamento, não cobertura. O teste continua executando hoje e consultando amanhã; não tenta adotar órfã futura. C5 determina a data do fato, sem impor por si só “não futuro”. (`app/chamados/tests/test_c20_fio_mudo.py:301`; `app/chamados/juizes.py:80`, `app/chamados/juizes.py:91`)
- **Tranca:** a delegação ao C6 procede; a alegação de mordida por R19a não basta. R19a prepara validação, que retorna antes do degrau de tranca. Matar um mutante que remove C6 inteiro não demonstra cobertura da retirada isolada da tranca. (`app/chamados/tests/test_c20_fio_mudo.py:268`; `app/chamados/juizes.py:1136`, `app/chamados/juizes.py:1140`)
- **Queries:** sustenta “sem medição”, não “sem risco”. A busca continua carregando apenas `disputa`, sem o mapa de identidade empregado pelo migrador. O selo de corrida usa outro módulo; a captura do chokepoint começa depois do `abrir` de preparação. Estouro efetivo: **[não verificado]**. (`app/chamados/models.py:478`; `app/chamados/services/fio.py:929`; `app/chamados/tests/test_c2_corrida_do_abrir.py:56`; `app/ponto/tests/test_selo_chokepoint_escrita.py:312`, `app/ponto/tests/test_selo_chokepoint_escrita.py:325`)
- **D±1:** “ampliar seria juiz novo” não se sustenta como necessidade técnica: E4-c já decide compatibilidade entre datas adjacentes na mesma jornada. A seleção atual não chega a essas candidatas. Obrigatoriedade dessa ampliação pelo aval literal: **[não verificado]**. (`app/chamados/models.py:475`; `app/chamados/juizes.py:1957`, `app/chamados/juizes.py:1991`)
- **L-113:** copiar a cerca existente não concilia a lei posterior. A adoção sai quando a competência foi entregue; L-113 permite nascimento em exportada e registra a queda da Pauta DP como pré-condição. Não considero a divergência resolvida pelo descarte. (`app/chamados/models.py:472`; `app/docs/LEIS.md:150`)
- **Nunca-bateu:** a dispensa de questionário tem fundamento; a exclusão integral sem conferir pai/agrupamento não está provada por R19d. A própria adesão prevê agrupar os furos no pai, e o teste só exige cobrança, veredito e ausência de perguntas. (`app/chamados/services/adesao.py:19`, `app/chamados/services/adesao.py:102`, `app/chamados/services/adesao.py:112`; `app/chamados/tests/test_c20_fio_mudo.py:118`)
- **Gancho no perdedor:** permanece incompleto. As portas gerais de retratação e reapontamento não reparam o chamado que perde sua pergunta. (`app/chamados/services/fio.py:891`, `app/chamados/services/fio.py:845`)
- **Universo:** os números reproduzidos somam 495; retirados três, restam 492, enquanto o aval escrito continua 494. Reconciliação individual: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `app/chamados/tests/test_c20_fio_mudo.py:13`)

O achado de relógio também permanece: a adoção consulta C6, cujo ramo de janela pode ler `date.today()` fora de parâmetro recebido da borda. É leitura preexistente ampliada pela composição, não corrigida pelo delta. (`app/chamados/models.py:489`; `app/chamados/juizes.py:1197`, `app/chamados/juizes.py:562`)

**2. Quem escreve e quem lê**

| Campo/ato | Escritores e leitores relevantes |
|---|---|
| **`Disputa.fechada_em`** | `fechar` preenche; `reabrir_admin` e `reabrir_sistema` limpam. Leem `aberta`, lookup de disputa, fio/API do colaborador e reconciliador. (`app/chamados/models.py:1952`, `app/chamados/models.py:2154`, `app/chamados/models.py:2178`, `app/chamados/models.py:1856`; `app/chamados/services/fio.py:131`; `app/api/views.py:1008`; `app/chamados/reconciliador.py:1149`) |
| **`fechada_por`** | Os mesmos três caminhos escrevem/limpam; os receivers de fechamento usam o autor para retomada/materialização. (`app/chamados/models.py:1953`, `app/chamados/models.py:2155`, `app/chamados/models.py:2179`; `app/chamados/signals.py:113`, `app/chamados/signals.py:140`) |
| **`resolucao`** | Fechamento escreve; as duas reaberturas limpam; `limpar_resolucao_residual` é outra porta preexistente específica. Leem a nota de fechamento, a cauda da materialização e a guarda de idempotência do fechamento. (`app/chamados/models.py:1954`, `app/chamados/models.py:2156`, `app/chamados/models.py:2180`, `app/chamados/models.py:2264`, `app/chamados/models.py:1912`; `app/chamados/signals.py:45`; `app/chamados/services/materializacao.py:1250`) |
| **`motivo_fechamento`** | Fechamento preenche; reaberturas limpam. Leem a idempotência do fechamento e o contador de disputa vazia. (`app/chamados/models.py:1955`, `app/chamados/models.py:2157`, `app/chamados/models.py:2181`, `app/chamados/models.py:1913`; `app/chamados/services/vigia_fio.py:734`) |
| **`Pergunta.via_resolucao`** | O delta usa `retratar_duplicata → carimbar_identidade → _escrever_carimbo`. Existem ainda os escritores preexistentes de relavra e encerramento por fato. Leem a nova seleção, C6, estado e ressuscitação. (`app/chamados/services/fio.py:891`; `app/chamados/models.py:2651`, `app/chamados/models.py:2735`, `app/chamados/models.py:2764`, `app/chamados/models.py:477`, `app/chamados/models.py:2593`, `app/chamados/models.py:1546`; `app/chamados/services/relavra_via.py:123`; `app/chamados/juizes.py:1171`) |
| **`Pergunta.chamado_destino`** | Nascimento/identidade em `Pergunta.abrir` e costura em `reapontar_resolvedora`; o delta chama a costura. Leem âncora, montagem do fio, lookup de disputa, gate e migrador. (`app/chamados/models.py:2541`, `app/chamados/models.py:2567`, `app/chamados/models.py:501`; `app/chamados/services/fio.py:845`, `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:135`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:931`; `app/chamados/models.py:344`) |
| **`Chamado.status_local` sob trava** | A adoção somente lê; a escrita continua em `_gravar_estado` e no nascimento. A leitura travada não atualiza `self`; a porta de costura relê seu destino. (`app/chamados/models.py:462`, `app/chamados/models.py:222`, `app/chamados/models.py:1021`; `app/chamados/services/fio.py:763`) |
| **`pergunta_reatada`** | Gate e adoção chamam `core.observ.evento`, escritor da auditoria. No gate, `objeto_id` é o chamado que fecha; na adoção, é o que abre. Há leitura pela auditoria genérica; consumidor operacional específico: **[não verificado]**. (`app/chamados/models.py:423`, `app/chamados/models.py:506`; `app/core/observ.py:94`; `app/core/views.py:705`) |
| **Ocorrência de `reabrir_sistema`** | Passa por `nota.anexar_ocorrencia` e vai ao **chamado original da disputa**, não ao destino adotante. Sem `objeto_ref`, não recebe deduplicação própria. O fio carrega esses followups. (`app/chamados/models.py:2183`, `app/chamados/models.py:2185`; `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:103`; `app/chamados/services/fio.py:172`) |

**Não identifiquei segundo escritor direto novo fora dessas portas.** O delta acrescenta chamadas; os donos do estado fechado já incluem `reabrir_sistema`. Isso não transforma `via_resolucao` em campo com um único sítio de atribuição em toda a cópia. (`app/chamados/models.py:501`, `app/chamados/models.py:513`, `app/chamados/models.py:516`; `app/chamados/tests/test_selo_resolucao_um_escritor.py:32`; `app/chamados/services/relavra_via.py:123`)

Os leitores, porém, passam a enxergar combinações distintas:

- **Disputa aberta com pai terminal é possível.** `reabrir_sistema` não reabre o pai; o receiver do pai só atua no fechamento. Isso não invalida automaticamente a pergunta, cuja âncora pode ser o novo destino vivo, mas precisa ser distinguido da acessibilidade do continente. (`app/chamados/models.py:2178`, `app/chamados/models.py:2185`; `app/chamados/signals.py:28`; `app/chamados/services/fio.py:117`)
- Reabrir a disputa torna **outras perguntas dela** elegíveis aos recortes de fio/reconciliação; não somente a adotada. Também pode fazer a disputa voltar a bloquear seu chamado original vivo. (`app/chamados/services/fio.py:214`; `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:1482`; `app/chamados/juizes.py:2080`, `app/chamados/juizes.py:2084`)
- `fio_vivo` e vigia continuam reduzindo C6, independentemente de a disputa estar aberta. Já a adoção exclui duplicatas antes de C6; o migrador não faz essa exclusão. Uma duplicata respondida pode entrar na eleição do migrador e ser ignorada pela adoção. (`app/chamados/juizes.py:1242`; `app/chamados/services/vigia_fio.py:165`; `app/chamados/models.py:477`; `app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`)
- Para o fechamento citado como `fechar_se_todas_respondidas`, o caminho equivalente conferido é `_fechar_disputa_se_quitada`: consulta C7, que exige ausência de respostas sem veredito e de fio vivo. Não fecha apenas porque todas responderam. Correspondência com outro símbolo exato: **[não verificado]**. (`app/chamados/services/materializacao.py:1224`, `app/chamados/services/materializacao.py:1238`; `app/chamados/models.py:1885`)

**3. Ordem de trava e transação**

**`abrir()`** chama a adoção dentro de seu `atomic`, já segurando o colaborador; nascimento e adoção ficam na transação externa. O `atomic(savepoint=False)` interno não libera essas travas nem cria confirmação independente. (`app/chamados/models.py:952`, `app/chamados/models.py:955`, `app/chamados/models.py:1024`, `app/chamados/models.py:461`)

**`reabrir()` direto** não envolve todo o ato: consulta estado sem trava, grava a transição em seu núcleo transacional, ressuscita e depois adota. A trava nova impede duas adoções simultâneas do mesmo destino, mas não impede duas execuções de aprovarem anteriormente a reabertura. (`app/chamados/models.py:1424`, `app/chamados/models.py:274`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`)

**A afirmação “não trava Disputa, ordem intacta” é insuficiente.** A adoção segura Chamado, salva Pergunta e chama um `save` da Disputa; esse `UPDATE` participa das travas de escrita. No caminho de `abrir`, isso ocorre também sob a trava do Colaborador. (`app/chamados/models.py:462`, `app/chamados/models.py:955`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`; `app/chamados/services/fio.py:847`)

Há um **ciclo estático possível**:

1. T1 segura o chamado A e carrega uma candidata cuja disputa D aparece fechada. Outro ato reabre D e confirma. (`app/chamados/models.py:462`, `app/chamados/models.py:478`, `app/chamados/models.py:2154`, `app/chamados/models.py:2166`)
2. T2 declara hora em A; sua transação obtém D agora aberta e mantém sua trava. Reutilizando uma pergunta sem destino, tenta costurá-la em A e espera T1. (`app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/declaracao_texto.py:314`, `app/chamados/services/declaracao_texto.py:320`; `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:502`; `app/chamados/services/fio.py:762`)
3. T1 usa seu objeto antigo de D fechada e tenta `reabrir_sistema`, esperando D: **Chamado→Disputa contra Disputa→Chamado**. Reprodução do deadlock: **[não verificado]**. (`app/chamados/models.py:512`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`)

Não usei o comentário do selo como prova automática de Disputa→Colaborador: no emissor, encontrar a disputa retorna antes do ramo que chama `abrir`. O selo é intraprocedural, considera apenas dois modelos e não cobre essa composição nem travas por escrita. (`app/chamados/services/disputa_emissao.py:502`, `app/chamados/services/disputa_emissao.py:504`; `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:23`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:85`)

Também existe trava explícita conjunta de Pergunta e Disputa na relavra, inclusive fechada no comportamento padrão; deve entrar no censo de ordem. (`app/chamados/services/relavra_via.py:185`, `app/chamados/services/relavra_via.py:221`)

**Corrida com fechamento permanece:** a adoção decide sobre D carregada anteriormente; `reabrir_sistema` não relê seu estado. `fechar` também decide antes de seu bloco de escrita e grava os mesmos quatro campos. Portanto, não há decisão mutuamente protegida entre reabrir e fechar; qual escrita prevaleceria numa execução concreta: **[não verificado]**. (`app/chamados/models.py:478`, `app/chamados/models.py:512`, `app/chamados/models.py:2172`, `app/chamados/models.py:2182`, `app/chamados/models.py:1911`, `app/chamados/models.py:1937`, `app/chamados/models.py:1956`)

**4. Contraexemplos e idempotência**

- **Canônica daqui + órfã respondida:** se ambas têm resposta e a local é mais antiga, a local vence. Não há costura, evento ou reabertura; a órfã vira duplicata, pode continuar viva por C6 e fica na âncora terminal, excluída das adoções seguintes. (`app/chamados/services/fio.py:873`; `app/chamados/models.py:477`, `app/chamados/models.py:499`, `app/chamados/models.py:514`; `app/chamados/juizes.py:1150`)
- **Canônica órfã em D1, locais em D2:** só D1 reabre. D2 recebe retratações sem atualização do continente; se a canônica já é local, nem a disputa dela passa pela reabertura nova. (`app/chamados/models.py:499`, `app/chamados/models.py:512`, `app/chamados/models.py:514`)
- **Retratar local já respondida:** o carimbo não garante sua morte. A porta documenta que a resposta pode mantê-la viva; o gate continua consultando C6 sobre ancoradas sem excluir duplicata. Logo, carimbar as irmãs não demonstra “uma única viva”. (`app/chamados/models.py:2716`, `app/chamados/models.py:327`, `app/chamados/models.py:351`; `app/chamados/juizes.py:1150`)
- **Disputa de outro chamado vivo:** a proteção examina a âncora efetiva da pergunta. Destino explícito terminal permite candidatura mesmo com `disputa.chamado` vivo diferente; reabrir esse continente pode alterar o bloqueio daquele outro chamado. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:487`, `app/chamados/models.py:513`; `app/chamados/juizes.py:2080`)
- **Segunda adoção simples:** para no grupo “todas daqui”, sem repetir costura/evento. Um segundo `reabrir` após o primeiro terminar normalmente também é recusado pela transição já viva. Isso não cobre concorrência nem ciclo posterior de fechamento/reabertura. (`app/chamados/models.py:496`, `app/chamados/models.py:1408`, `app/chamados/models.py:1478`)
- **Ressuscitação posterior:** uma duplicata local sem resposta pode entrar no universo das materializadas sem texto, ser descarimbada e voltar viva quando o fato acusa novamente. Se a canônica também estiver local, a adoção pula o grupo inteiro e não refaz o dedup. Contraexemplo condicionado a reabertura admitida; reprodução: **[não verificado]**. (`app/chamados/models.py:1525`, `app/chamados/models.py:1546`, `app/chamados/models.py:1549`, `app/chamados/models.py:1595`, `app/chamados/models.py:2754`, `app/chamados/models.py:496`)

A trilha também tem limites: `evento` é best effort; a ocorrência de reabertura não tem chave própria. Ausência de repetição de evento/nota não é demonstrada apenas pelo retorno `[]`. (`app/core/observ.py:98`, `app/core/observ.py:139`; `app/chamados/models.py:2185`; `app/chamados/services/nota.py:103`)

**5. Mordida dos testes novos** [nao verificado]

| Teste | Avaliação estática | [nao verificado]
|---|---|
| **R08d** | Tem mordida para P1: prova órfã viva respondida, cobrança e local existentes, destino da canônica e carimbo da local. Não cobre duas respondidas, canônica local ou estado das duas disputas. (`app/chamados/tests/test_c20_fio_mudo.py:180`, `app/chamados/tests/test_c20_fio_mudo.py:186`, `app/chamados/tests/test_c20_fio_mudo.py:191`) |
| **R08e** | Tem mordida para seleção da duplicata: exige que ela continue viva, permaneça no terminal e que a canônica seja adotada. Também evidencia que a duplicata viva permanece fora da cura. (`app/chamados/tests/test_c20_fio_mudo.py:208`, `app/chamados/tests/test_c20_fio_mudo.py:214`, `app/chamados/tests/test_c20_fio_mudo.py:215`) |
| **R08f** | Morde a ausência de reabertura de `fechada_em`, mas não a promessa completa de acesso. A fixture cria disputa sem chamado; não exercita ocorrência, pai terminal/vivo ou limpeza dos outros campos. O montador HTML exclui disputas sem chamado. (`app/chamados/tests/test_c20_fio_mudo.py:54`, `app/chamados/tests/test_c20_fio_mudo.py:220`, `app/chamados/tests/test_c20_fio_mudo.py:229`; `app/chamados/services/fio.py:181`) |
| **R08g** | **O mock intercepta a função realmente chamada:** o import é local ao método e busca o símbolo no módulo patchado. Porém, não comprova chamada com candidata/destino esperados; uma saída antecipada pode passar sem testar recusa. Também não confere preservação dos destinos. (`app/chamados/tests/test_c20_fio_mudo.py:240`, `app/chamados/tests/test_c20_fio_mudo.py:245`; `app/chamados/models.py:474`, `app/chamados/models.py:501`) |
| **R08i** | Prova primeira adoção e segunda chamada retornando `[]`, com contagem estável. Não confere eventos, notas, carimbos, continentes ou ciclo de reabertura. (`app/chamados/tests/test_c20_

[truncado em 20000 de 22873 caracteres]

## 05/11 23:11 · sessao 1af9bb312b57 · F-346 · verificador · origem=Code

**R07/R08 fecham as lacunas anunciadas dos oito campos, por análise estática. O selo AST melhorou, mas continua permitindo falsos negativos e falsos positivos.** (`app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_remarcar_limpa_janela_f346.py:129`, `app/ferias/test_remarcar_limpa_janela_f346.py:153`, `app/ferias/test_remarcar_limpa_janela_f346.py:175`)

Executei o helper, o método sintético e o detector de relógio por extração AST em memória, sem importar os módulos Django. Execução Django, mutantes em banco e comportamento em produção: **[nao verificado]**; `django` e `freezegun` não estão disponíveis neste ambiente. Conferência independente de hash/pai: **[nao verificado]**. [nao verificado]

1. **Sim: os oito campos agora têm valores discriminantes.**

R07 carrega o objeto **antes** do UPDATE concorrente simulado. Os três valores novos diferem dos carregados; depois da remarcação, recarrega e compara os três. Portanto, incluir isoladamente `confirmacao`, `confirmado_em` ou `obs` em `update_fields`, ou voltar ao save completo, sobrescreveria uma sentinela e falharia na comparação. Isso é dedução estática, sem execução dos mutantes. (`app/ferias/models.py:313`, `app/ferias/models.py:314`, `app/ferias/models.py:318`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`)

R08 exige estas mudanças **no objeto recarregado do banco**; retirar isoladamente qualquer um dos cinco campos da persistência deixa um valor antigo discriminante. (`app/ferias/test_remarcar_limpa_janela_f346.py:31`, `app/ferias/test_remarcar_limpa_janela_f346.py:124`, `app/ferias/test_remarcar_limpa_janela_f346.py:128`)

| Campo | Antes → exigido por R08 |
|---|---|
| `data_inicio` | `ini` → `ini + 30 dias` |
| `data_fim` | `ini + 9 dias` → `ini + 36 dias` |
| `dias` | `10` → `7` |
| `aviso_emitido` | `False` → `True` |
| `status` | `concluido` → `agendado` |

Valores e comparação: `app/ferias/test_remarcar_limpa_janela_f346.py:31`, `app/ferias/test_remarcar_limpa_janela_f346.py:124`, `app/ferias/test_remarcar_limpa_janela_f346.py:129`.

**Ainda faltam sentinelas explícitas de preservação para `periodo_aquisitivo_id`, `agendado_por_id` e `criado_em`.** Esses campos existem, ficam fora do save da troca e não são comparados após a remarcação em R01–R08. A PK participa das seleções/recargas, mas também não recebe uma comparação independente de preservação. (`app/ferias/models.py:299`, `app/ferias/models.py:310`, `app/ferias/models.py:319`, `app/ferias/services.py:392`, `app/ferias/test_remarcar_limpa_janela_f346.py:43`, `app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_remarcar_limpa_janela_f346.py:129`)

R08 testa diretamente o serviço: seu estado inicial `concluido` seria recusado pela porta HTTP de remarcação. Isso não prejudica a discriminação da persistência, mas limita o que o caso demonstra sobre a porta. (`app/ferias/test_remarcar_limpa_janela_f346.py:124`, `app/ferias/test_remarcar_limpa_janela_f346.py:127`, `app/ferias/views.py:649`)

2. **Resultado em memória sobre `services.py`: `['_trocar_janela']`.**

Esse resultado corresponde à atribuição existente na troca; o helper examina somente o arquivo indicado pelo teste e suas funções síncronas. Não constitui prova de ausência de escritores nas formas que ele ignora. (`app/ferias/test_remarcar_limpa_janela_f346.py:133`, `app/ferias/test_remarcar_limpa_janela_f346.py:153`, `app/ferias/test_remarcar_limpa_janela_f346.py:189`, `app/ferias/services.py:379`)

Confirmei estas classes de **falsos negativos** em memória:

| Forma sintética | Resultado e motivo |
|---|---|
| `[ag.data_inicio, ag.data_fim] = [i, f]`; tupla aninhada | `[]`: só desempacota uma camada de `ast.Tuple`; não percorre listas nem alvos aninhados. (`app/ferias/test_remarcar_limpa_janela_f346.py:159`) |
| `async def z(...): ag.data_inicio = i` | `[]`: exige `ast.FunctionDef`. Escritas no nível do módulo também ficam fora desse universo. (`app/ferias/test_remarcar_limpa_janela_f346.py:153`) |
| `self.data_inicio = i`, sendo `self` um AG | `[]`: exclusão textual de `self`. (`app/ferias/test_remarcar_limpa_janela_f346.py:161`) |
| `setattr(self.ag, "data_inicio", i)`; `self.agendamentos.update(data_inicio=i)`; `periodo.agendamentos.update(data_inicio=i)` | `[]`: a raiz `self`/`periodo` é excluída mesmo quando o destinatário final é AG. (`app/ferias/test_remarcar_limpa_janela_f346.py:137`, `app/ferias/test_remarcar_limpa_janela_f346.py:143`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:173`) |
| `qs.bulk_update(xs, campos)` ou `fields=campos` | `[]`: não resolve a variável dos campos. (`app/ferias/test_remarcar_limpa_janela_f346.py:174`) |
| Alias de `setattr`, `builtins.setattr(...)`, `ag.__setattr__(...)`, `ag.__dict__["data_inicio"] = i`, `getattr(qs, "update")(...)` | `[]`: não correspondem às formas literais reconhecidas. (`app/ferias/test_remarcar_limpa_janela_f346.py:160`, `app/ferias/test_remarcar_limpa_janela_f346.py:167`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:173`) |

Confirmei também estes **falsos positivos** em memória:

- `pa.data_inicio = i`, a variante anotada com valor e `setattr(pa, "data_inicio", i)` contam mesmo se `pa` for `PeriodoAquisitivo`; outro modelo ou um dicionário com `.update(dias=i)` também conta. Não há resolução de tipo/alias, e as exclusões diferem entre atribuição e chamadas. (`app/ferias/test_remarcar_limpa_janela_f346.py:137`, `app/ferias/test_remarcar_limpa_janela_f346.py:160`, `app/ferias/test_remarcar_limpa_janela_f346.py:163`, `app/ferias/test_remarcar_limpa_janela_f346.py:173`)
- `qs.update(obs="dias")` e `qs.filter(obs="dias").update(status="concluido")` contam: a constante `"dias"` pode aparecer em **qualquer parte da chamada**, inclusive filtro ou valor de outro campo. (`app/ferias/test_remarcar_limpa_janela_f346.py:175`)
- `qs.update(**{"obs": "x"})` e `campo="obs"; setattr(ag, campo, "x")` contam. Esse excesso é deliberado e documentado como conservador. (`app/ferias/test_remarcar_limpa_janela_f346.py:168`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:174`)
- Uma função interna que atribui a data faz contar **a interna e a externa**, pois `ast.walk(fn)` atravessa definições aninhadas. (`app/ferias/test_remarcar_limpa_janela_f346.py:152`, `app/ferias/test_remarcar_limpa_janela_f346.py:155`)

3. **Não cobrem cada ramo: falta uma sentinela de `AugAssign`.**

Todas as asserções existentes do método sintético passaram na execução extraída em memória. A cobertura declarada está assim: (`app/ferias/test_remarcar_limpa_janela_f346.py:201`)

| Forma | Caso existente |
|---|---|
| `Assign` simples | Sim, `a/b`. (`app/ferias/test_remarcar_limpa_janela_f346.py:202`) |
| `AugAssign` | **Não**; o helper aceita, mas o método não contém exemplo. (`app/ferias/test_remarcar_limpa_janela_f346.py:156`, `app/ferias/test_remarcar_limpa_janela_f346.py:201`) |
| `AnnAssign` sem/com valor | Sim, `n/o`. (`app/ferias/test_remarcar_limpa_janela_f346.py:216`, `app/ferias/test_remarcar_limpa_janela_f346.py:217`) |
| `setattr` constante/variável | Sim, `d/m`. (`app/ferias/test_remarcar_limpa_janela_f346.py:206`, `app/ferias/test_remarcar_limpa_janela_f346.py:215`) |
| `update` kwargs/`**` | Sim, `e/k`; `g` protege o filtro `periodo_aquisitivo`. (`app/ferias/test_remarcar_limpa_janela_f346.py:207`, `app/ferias/test_remarcar_limpa_janela_f346.py:211`, `app/ferias/test_remarcar_limpa_janela_f346.py:214`) |
| `bulk_update` | Sim, lista literal em `f`. (`app/ferias/test_remarcar_limpa_janela_f346.py:208`) |
| Alheio | Sim, atribuição em `periodo` e UPDATE em `PeriodoAquisitivo`. (`app/ferias/test_remarcar_limpa_janela_f346.py:205`, `app/ferias/test_remarcar_limpa_janela_f346.py:213`) |

Isso não cobre todas as condições internas: faltam, por exemplo, `_raiz` com `Subscript`/retorno `None`, exclusões de chamadas por `self`/`periodo` e `setattr` constante em campo alheio. (`app/ferias/test_remarcar_limpa_janela_f346.py:143`, `app/ferias/test_remarcar_limpa_janela_f346.py:170`, `app/ferias/test_remarcar_limpa_janela_f346.py:201`)

4. **Não identifiquei leitura nova de relógio fora do freeze; UTC é compatível com a configuração copiada.**

O detector executado em memória retornou **`0`**. O arquivo não contém chamadas diretas a `today/now/localdate/localtime`; R07 constrói um instante literal, e R07/R08 pertencem à classe congelada. As leituras do serviço e de `status_por_data` ficam dentro da chamada feita pelos testes congelados. (`app/core/tests/test_selo_teste_sem_relogio.py:32`, `app/ferias/test_remarcar_limpa_janela_f346.py:23`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/services.py:380`, `app/ferias/models.py:280`)

O selo, sozinho, é menos forte: encontrar o texto `freeze_time` já faz retornar zero para o arquivo inteiro; ele não verifica o alcance do congelamento. Aqui há também o decorador efetivo na classe. (`app/core/tests/test_selo_teste_sem_relogio.py:28`, `app/core/tests/test_selo_teste_sem_relogio.py:34`, `app/ferias/test_remarcar_limpa_janela_f346.py:23`)

`confirmado_em` é `DateTimeField`, o literal é aware em UTC, e a configuração declara `USE_TZ=True` e `America/Sao_Paulo`. A comparação é entre instantes, sem exigir identidade do objeto `tzinfo`; UTC versus outro offset equivalente não fragiliza essa asserção. O teste pressupõe timestamps aware: portabilidade para `USE_TZ=False`/outro backend permanece **[nao verificado]**. (`app/ferias/models.py:318`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/config/settings/base.py:93`, `app/config/settings/base.py:95`)

5. **Escritores e leitores dos cinco campos semeados por UPDATE direto:**

| Campo | Escritores no produto | Leitores específicos localizados |
|---|---|---|
| `aviso_emitido` | Default; execução; aprovação; troca. (`app/ferias/models.py:305`, `app/ferias/services.py:323`, `app/ferias/services.py:439`, `app/ferias/services.py:380`) | KPI/filtro, aviso da situação, mensagem da remarcação e template. (`app/ferias/views.py:26`, `app/ferias/views.py:77`, `app/ferias/situacao.py:257`, `app/ferias/views.py:671`, `app/templates/ferias/_secao_colab.html:121`) |
| `status` | Default; histórico; cancelamentos DP/admin; troca; alinhamento do cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:518`) | Guardas do modelo/HTTP, consultas, materialização e template. (`app/ferias/models.py:324`, `app/ferias/views.py:649`, `app/ferias/services.py:51`, `app/ponto/services/ausencia.py:1164`, `app/templates/ferias/_secao_colab.html:120`) |
| `confirmacao` | Default; pedido; resposta confirmar/recusar. (`app/ferias/models.py:314`, `app/ferias/services.py:486`, `app/ferias/services.py:496`, `app/ferias/services.py:499`) | Guarda de envio, seleção da resposta, atendimento e template. (`app/ferias/views.py:838`, `app/ferias/views.py:880`, `app/chamados/views.py:628`, `app/templates/ferias/_secao_colab.html:122`) |
| `confirmado_em` | Pedido limpa; confirmação carimba. (`app/ferias/services.py:487`, `app/ferias/services.py:500`) | R07 e teste de confirmação. Leitor específico de runtime: **[nao verificado]**. (`app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/ferias/test_confirmacao_agendamento.py:76`) |
| `obs` | Histórico, execução, aprovação e cancelamento admin. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:441`, `app/ferias/services.py:360`) | Cancelamento admin lê para concatenar o carimbo; R07 confere preservação. (`app/ferias/services.py:360`, `app/ferias/test_remarcar_limpa_janela_f346.py:117`) |

**O `.update()` direto não executa o override `AgendamentoFerias.save()` nem emite os sinais de save; portanto não dispara a guarda/sincronização por esse caminho e não invalida a premissa das sentinelas.** O override concentra esses efeitos, e a própria cópia documenta o UPDATE sem signal. (`app/ferias/models.py:434`, `app/chamados/signals.py:80`, `app/colaboradores/services/colaborador.py:81`)

Já o `save(update_fields=...)` posterior, dentro da troca, executa guarda e sincronização normalmente. Triggers ou customizações presentes somente no banco implantado: **[nao verificado]**. (`app/ferias/services.py:392`, `app/ferias/models.py:435`, `app/ferias/models.py:437`)

## 05/11 23:09 · sessao 37fe8504fe3c · - · - · origem=Ronald (tmux)

**P3 está parcialmente resolvido:** a adoção da mesma linha passa a serializar, mas a reabertura inteira continua fora dessa unidade. `reabrir()` lê o estado sem trava, grava o novo estado em outro `atomic`, ressuscita perguntas e somente depois entra no `atomic(savepoint=False)` da adoção. Portanto, duas reaberturas ainda podem aprovar as guardas e registrar a transição antes da serialização nova. (`app/chamados/models.py:1424`, `app/chamados/models.py:1483`, `app/chamados/models.py:274`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`, `app/chamados/models.py:461`)

**Estado transacional dos chamadores:**

- `abrir()` chama a adoção dentro de seu `atomic`, já segurando o colaborador; o nascimento e a adoção pertencem à transação externa. O `savepoint=False` não solta essa trava nem cria uma fronteira independente. (`app/chamados/models.py:952`, `app/chamados/models.py:955`, `app/chamados/models.py:1021`, `app/chamados/models.py:1024`, `app/chamados/models.py:461`)
- `reabrir()` direto não estabelece transação sobre o ato inteiro. Sem transação externa, `_gravar_estado`, ressuscitação e adoção são etapas distintas; com chamador externo transacional, herdam a unidade dele. (`app/chamados/models.py:1413`, `app/chamados/models.py:1483`, `app/chamados/models.py:1492`, `app/chamados/models.py:1493`)
- A nova leitura travada confirma apenas o estado do banco; não atualiza a instância `self`. A costura consulta novamente o destino por sua própria porta. (`app/chamados/models.py:462`, `app/chamados/models.py:464`, `app/chamados/services/fio.py:758`, `app/chamados/services/fio.py:763`)

**“Não trava Disputa” não demonstra preservação da ordem:** `reabrir_sistema()` executa um `UPDATE` da disputa enquanto a adoção segura o chamado e, no caminho de `abrir`, também o colaborador. A sequência nova inclui, portanto, chamado→pergunta→disputa, embora não exista `select_for_update` explícito de disputa no helper. (`app/chamados/models.py:462`, `app/chamados/models.py:501`, `app/chamados/services/fio.py:847`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`, `app/chamados/models.py:955`)

**Ciclo estático factível, sem depender dos ramos excludentes do emissor:**

1. T1 adota para `ChA`, segura sua linha e carrega uma candidata `P1` cuja disputa `D` aparece fechada no snapshot. (`app/chamados/models.py:462`, `app/chamados/models.py:475`, `app/chamados/models.py:478`)
2. Outro ato reabre `D` e confirma essa alteração; seu chamado pai pode ser `ChB`, distinto de `ChA`. (`app/chamados/models.py:2154`, `app/chamados/models.py:2166`)
3. T2 declara hora em `ChA`. Seu `atomic` externo chama `disputa_para_declaracao`, que trava agora `D` aberta e retorna pelo ramo existente. A trava permanece até terminar o `atomic` da declaração. (`app/chamados/services/declaracao_texto.py:302`, `app/chamados/services/declaracao_texto.py:310`, `app/chamados/services/disputa_emissao.py:563`, `app/chamados/services/disputa_emissao.py:467`, `app/chamados/services/disputa_emissao.py:502`)
4. Uma candidata reutilizável sem destino — pode ser `P2`, de outro marco, deixando `P1` livre — leva T2 à costura de `ChA`, que espera sua linha travada por T1. (`app/chamados/services/declaracao_texto.py:226`, `app/chamados/services/declaracao_texto.py:313`, `app/chamados/services/declaracao_texto.py:320`, `app/chamados/services/fio.py:762`)
5. T1 usa seu snapshot fechado e tenta atualizar `D`, esperando a trava de T2: chamado→disputa contra disputa→chamado. Reprodução do deadlock: **[não verificado]**. (`app/chamados/models.py:512`, `app/chamados/models.py:513`, `app/chamados/models.py:2182`)

Não se deve usar isoladamente o comentário “Disputa→Colaborador” como prova desse ciclo: no `_abrir_ou_agregar_disputa`, encontrar a disputa retorna antes do `ChamadoColaborador.abrir`; no caminho da âncora, encontrar a disputa segue para pergunta e análise do chamado. (`app/chamados/services/disputa_emissao.py:502`, `app/chamados/services/disputa_emissao.py:504`, `app/chamados/services/disputa_emissao.py:1195`, `app/chamados/services/disputa_emissao.py:1203`)

**A trava de disputa também existe por JOIN:** `desfazer_carimbo_sem_lastro` trava pergunta e disputa com `select_for_update(of=('self', 'disputa'))`, inclusive disputa fechada no parâmetro padrão. Esse sítio precisa entrar no censo real de ordem. (`app/chamados/services/relavra_via.py:185`, `app/chamados/services/relavra_via.py:217`, `app/chamados/services/relavra_via.py:221`, `app/chamados/services/relavra_via.py:245`)

**Corrida com fechamento permanece:** a adoção decide reabrir pelo objeto de disputa carregado anteriormente; `reabrir_sistema` também decide pelo objeto recebido, sem reler ou comparar o estado gravado. `fechar()` igualmente decide pelas informações da instância antes de seu `atomic`, e grava os mesmos quatro campos. As portas não fornecem exclusão mútua entre essas decisões; prevalência concreta de um fechamento concorrente: **[não verificado]**. (`app/chamados/models.py:478`, `app/chamados/models.py:512`, `app/chamados/models.py:2172`, `app/chamados/models.py:2182`, `app/chamados/models.py:1911`, `app/chamados/models.py:1937`, `app/chamados/models.py:1952`, `app/chamados/models.py:1956`)

A ocorrência de reabertura é inserida no chamado **da disputa**, não necessariamente no novo destino; não recebe `objeto_ref`, portanto não obtém a chave de deduplicação da porta de notas. Duas instâncias antigas fechadas podem repetir essa ocorrência. (`app/chamados/models.py:2183`, `app/chamados/models.py:2185`, `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:102`, `app/chamados/services/nota.py:103`)

Os receivers de pergunta agendam o julgamento de becos em `on_commit`; portanto não fecham o beco no meio da adoção transacional. Os receivers de disputa fechada, por sua vez, podem reconciliar o pai e materializar perguntas durante o fechamento. (`app/chamados/signals.py:191`, `app/chamados/signals.py:202`, `app/chamados/signals.py:41`, `app/chamados/signals.py:42`, `app/chamados/signals.py:124`, `app/chamados/signals.py:140`)

**O selo não prova essa composição:** ele examina `select_for_update` somente para raízes `Colaborador`/`DisputaSupervisao` e somente dentro da mesma função; não contempla os `UPDATE`s descritos, o JOIN com raiz `PerguntaDisputa` nem o encadeamento entre métodos. (`app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:14`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:23`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:85`, `app/chamados/tests/test_selo_ordem_de_trava_disputa_colaborador.py:105`)

## 05/11 23:08 · sessao 1219f33ddbbf · - · - · origem=Ronald (tmux)

**Testes — análise estática; execução, RED, mutantes e corridas: [não verificado].** [nao verificado]

| Caso | O que comprova e o que falta |
|---|---|
| **R08d** | Exercita órfã respondida versus local sem resposta; exige cobrança, pergunta local e vivacidade da órfã; verifica reapontamento da órfã e carimbo da local. Tem mordida para P1, mas não cobre local canônica respondida, duas respondidas, disputa local fechada ou acesso posterior. (`app/chamados/tests/test_c20_fio_mudo.py:173`, `:180`, `:186`, `:191`) |
| **R08e** | A preparação carimba uma duplicata respondida e exige que continue viva por C6; ao final exige duplicata imóvel e canônica reapontada. Morde a seleção indevida da duplicata respondida. Não prova que todos os leitores deixam de contá-la como fio vivo: C6 continua priorizando resposta. (`app/chamados/tests/test_c20_fio_mudo.py:204`, `:208`, `:214`; `app/chamados/juizes.py:1150`, `:1171`) |
| **R08f** | Morde a ausência do `reabrir_sistema`: exige reapontamento e `fechada_em=None`. A disputa de teste nasce **sem chamado**, logo não exercita ocorrência, chamado terminal/vivo da disputa, limpeza dos outros três campos ou acesso real do colaborador. (`app/chamados/tests/test_c20_fio_mudo.py:54`, `:220`, `:227`, `:229`) |
| **R08g** | **O mock intercepta a função certa**: patch em `chamados.services.fio.reapontar_resolvedora`; o helper faz import local desse módulo e chama o nome importado dentro do contexto do patch. Entretanto, não usa `assert_called`, não exige vivacidade das duas candidatas nem preservação dos destinos; uma saída antecipada do helper permitiria aprovação vacuosa. No fluxo pretendido, morde retratação anterior à recusa. (`app/chamados/tests/test_c20_fio_mudo.py:238`, `:240`, `:245`; `app/chamados/models.py:474`, `:489`, `:501`) |
| **R08i** | A primeira adoção é comprovada pelo destino; a segunda chamada exige `[]` e contagem estável. Cobre a segunda adoção sequencial simples. Não conta eventos, ocorrências, carimbos ou reaberturas, nem cobre concorrência ou ciclo fechar/reabrir. (`app/chamados/tests/test_c20_fio_mudo.py:248`, `:256`, `:259`, `:261`) |
| **R19a** | O assert novo elimina a vacuidade apontada em P6: agora exige cobrança e verifica que a validada não mudou de destino nem ganhou irmã. Continua sendo controle de **validação**, não de tranca. (`app/chamados/tests/test_c20_fio_mudo.py:268`, `:272`, `:275`) |

**Descartes da triagem**

- **R19c fraco: descarte apenas como adiamento.** O teste continua rodando o vigia de hoje e apenas consultando ausência de pergunta amanhã; não semeia órfã futura nem tenta abertura futura. C5 determina o dia, mas seu primeiro degrau não impõe “dia ≤ hoje”. (`app/chamados/tests/test_c20_fio_mudo.py:301`; `app/chamados/juizes.py:80`, `:91`)
- **Tranca: justificativa insuficiente.** R19a prepara `validada_em`; C6 retorna morto por validação antes de alcançar tranca. Um mutante que removesse somente o degrau de tranca não seria morto por esse cenário. Remover C6 inteiro e falhar em R19a não prova cobertura individual de tranca. (`app/chamados/tests/test_c20_fio_mudo.py:268`; `app/chamados/juizes.py:1136`, `:1140`)
- **Queries: risco permanece sem medição.** O selo de corrida usa módulo `sem_previsao_escala`, que sai na primeira guarda C20; o chokepoint chama `abrir` na preparação, antes da captura de queries. Estouro efetivo: **[não verificado]**. (`app/chamados/tests/test_c2_corrida_do_abrir.py:56`; `app/chamados/models.py:459`; `app/ponto/tests/test_selo_chokepoint_escrita.py:312`, `:325`)
- **D±1: argumento “ampliar seria juiz novo” não demonstrado.** C5 apenas entrega uma data; E4-c existente já decide a compatibilidade de jornadas entre D e D±1. Buscar candidatas adjacentes e consultar E4-c não exige necessariamente outro juiz. A busca atual mantém igualdade estrita; obrigação de ampliar perante o aval literal: **[não verificado]**. (`app/chamados/juizes.py:80`, `:1957`, `:1991`; `app/chamados/models.py:475`)
- **L-113: descarte não resolve a divergência.** A guarda segue exigindo competência não entregue; L-113 revoga pauta DP como pré-condição e permite nascimento em competência exportada. Copiar a cerca antiga demonstra precedente técnico, não conciliação com a lei posterior. (`app/chamados/models.py:472`; `app/docs/LEIS.md:150`)
- **Nunca-bateu: fila procede, exclusão integral não provada.** A lei/código dispensa questionário e prevê agrupamento dos furos no pai de adesão; R19d só exige cobrança, veredito e ausência de perguntas, sem pai/agrupamento. (`app/chamados/services/adesao.py:19`, `:102`, `:112`; `app/ponto/services/cartorio.py:1157`, `:1268`; `app/chamados/tests/test_c20_fio_mudo.py:118`)
- **Universo: permanece inconsistente.** Os números escritos somam 495; retirar os três nunca-bateu dá 492, enquanto o contrato continua 494. Reconciliação individual: **[não verificado]**. (`app/chamados/tests/test_c20_fio_mudo.py:1`, `:13`)

**Idempotência e reabertura**

- A segunda adoção sequencial simples para no grupo composto só por perguntas locais; não repete costura ou evento nesse ramo. (`app/chamados/models.py:496`)
- Um segundo `reabrir` após o primeiro ter deixado o chamado vivo é recusado pela transição, antes de ressuscitar/adotar. Isso difere de duas execuções concorrentes ou de fechar e reabrir novamente. (`app/chamados/models.py:1408`, `:1478`, `:1492`)
- **Contraexemplo adicional de ciclo:** uma duplicata local sem resposta pode ser ressuscitada num fechamento/reabertura posterior: o universo inclui materializadas sem texto, descarimba a cópia para consultar C6 e depois limpa a via no banco. Se a canônica também estiver local, a adoção pula o grupo “só daqui”, preservando as duas novamente vivas quando o dia acusa. É contraexemplo estático condicionado a uma reabertura admitida; reprodução: **[não verificado]**. (`app/chamados/models.py:1525`, `:1546`, `:1549`, `:1595`, `:2754`, `:496`; `app/chamados/juizes.py:1181`)

**Cinco atos de perda**

| Ato | Estado da cobertura |
|---|---|
| **Reabrir** | Melhorou a adoção de órfã e a disputa fechada da canônica movida; o atomic novo envolve só adoção, posterior a `_gravar_estado` e ressuscitação. R08r cobre execução simples. (`app/chamados/models.py:461`, `:1483`, `:1492`; `app/chamados/tests/test_c20_fio_mudo.py:150`) |
| **Retratar duplicata** | P2 foi curado dentro da adoção: move antes de retratar. A porta geral continua apenas carimbando identidade, sem reparar o chamado perdedor; o migrador existente ainda retrata antes de tentar costura. (`app/chamados/models.py:501`, `:516`; `app/chamados/services/fio.py:891`, `:943`, `:947`) |
| **Validação** | Nenhum gancho C20 novo; o caminho existente mantém atomic do carimbo/desfecho e recalcula por evento. Validação mata legitimamente no primeiro degrau de C6. (`app/chamados/services/validacao.py:395`, `:405`; `app/chamados/juizes.py:1136`) |
| **Morte por leitura** | Nenhum gancho C20 novo; C6 continua decidindo tranca, identidade, marco/célula; reconciliação consulta C6 e separa tranca antes de carimbar. (`app/chamados/juizes.py:1140`, `:1171`, `:1177`; `app/chamados/reconciliador.py:651`, `:657`) |
| **Reapontar** | A adoção usa a porta correta e a porta protege o destino; continua ausente guarda/reparação geral para o chamado que perde a pergunta. (`app/chamados/models.py:501`; `app/chamados/services/fio.py:837`, `:845`) |

A classe de testes usa `TestCase`; os novos cenários não executam explicitamente callbacks de commit, embora o save da pergunta agende fechamento de becos por `on_commit`. Efeitos posteriores desse fechamento: **[não verificado]** pelos testes novos. (`app/chamados/tests/test_c20_fio_mudo.py:22`, `:32`; `app/chamados/signals.py:191`, `:202`)

## 05/11 23:08 · sessao 790368a804d1 · - · - · origem=Ronald (tmux)

Achados estáticos, sem execução de testes ou acesso a produção **[não verificado]**. [nao verificado]

- **Não surgiu escritor direto novo dos campos auditados.** A adoção chama `reapontar_resolvedora`, `retratar_duplicata → carimbar_identidade`, `reabrir_sistema` e `evento`; não atribui diretamente esses estados. (`app/chamados/models.py:501`, `app/chamados/models.py:506`, `app/chamados/models.py:513`, `app/chamados/models.py:516`; `app/chamados/services/fio.py:891`)

- **Estado fechado da disputa:** `fechar` grava `fechada_em`, `fechada_por`, `resolucao`, `motivo_fechamento`; `reabrir_admin` e `reabrir_sistema` limpam os quatro juntos; `limpar_resolucao_residual` também escreve `resolucao`, pela porta específica do passivo. (`app/chamados/models.py:1952`, `app/chamados/models.py:2154`, `app/chamados/models.py:2178`, `app/chamados/models.py:2264`) A cascata de desligamento viva já usa `_d.fechar`; os comentários de testes que a descrevem como escritor por `.update()` estão defasados. (`app/colaboradores/services/desligamento.py:133`, `app/colaboradores/services/desligamento.py:142`; `app/chamados/tests/test_porta_disputa_ciclo.py:51`)

- **Leitores desse estado:** `aberta` deriva de `fechada_em`; `_disputa_do_chamado` exige disputa aberta; fio do colaborador carrega perguntas das abertas, e API usa o mesmo recorte; reconciliador seleciona/reconfere `fechada_em` antes de fechar. (`app/chamados/models.py:1855`; `app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:135`, `app/chamados/services/fio.py:211`; `app/api/views.py:1008`; `app/chamados/reconciliador.py:1149`, `app/chamados/reconciliador.py:1482`) `fechada_por` alimenta retomada/materialização nos signals; `resolucao` alimenta a nota do fechamento e `_fechar_disputa_se_quitada`; `motivo_fechamento` alimenta o contador `disputa_vazia`. (`app/chamados/signals.py:45`, `app/chamados/signals.py:113`, `app/chamados/signals.py:140`; `app/chamados/services/materializacao.py:1250`; `app/chamados/services/vigia_fio.py:734`)

- **A ocorrência de reabertura vai ao chamado original da disputa**, não ao novo destino da pergunta. `reabrir_sistema` não reabre esse chamado, não muda `disputa.chamado` e não restringe a reabertura às perguntas adotadas. Pode, portanto, deixar disputa aberta cujo pai continua terminal e tornar elegíveis outras perguntas daquele continente. (`app/chamados/models.py:2178`, `app/chamados/models.py:2183`, `app/chamados/models.py:2185`; `app/chamados/services/fio.py:181`, `app/chamados/services/fio.py:214`) O signal de pai tem apenas ramo de fechamento e retorna quando `fechada_em` está vazio. (`app/chamados/signals.py:28`)

- **Nota e evento têm garantias diferentes.** A nota usa a porta `anexar_ocorrencia`; sem `objeto_ref`, é `create` sem deduplicação própria. Sua existência depende da guarda de reabertura. (`app/chamados/models.py:2172`, `app/chamados/models.py:2185`; `app/chamados/services/nota.py:95`, `app/chamados/services/nota.py:103`) `pergunta_reatada` é escrito pela porta genérica `evento`, que é best effort e pode retornar `None`. Os dois chamadores usam significados distintos para `objeto_id`: chamado que fecha no gate, chamado que abre na adoção. (`app/chamados/models.py:423`, `app/chamados/models.py:506`; `app/core/observ.py:98`, `app/core/observ.py:139`) Fio lê `followups`; consumidor de produção específico de `pergunta_reatada`: **[não verificado]**. (`app/chamados/services/fio.py:172`, `app/chamados/services/fio.py:176`)

- **`chamado_destino` tem dois caminhos autorizados já existentes:** nascimento/identidade em `PerguntaDisputa.abrir` e costura em `fio.reapontar_resolvedora`. A porta de identidade pode reapontar a última pergunta do fato sem passar pela função mockada em R08g. (`app/chamados/models.py:2522`, `app/chamados/models.py:2541`, `app/chamados/models.py:2567`; `app/chamados/services/fio.py:845`; `app/core/tests/test_contract_escritores_estado.py:60`) Leitores incluem âncora, montagem do fio, lookup de disputa e migrador. (`app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:135`, `app/chamados/services/fio.py:236`, `app/chamados/services/fio.py:931`)

- **`via_resolucao` não possui só `_escrever_carimbo` como sítio em toda a cópia:** há escrita na porta de encerramento por fato e em `relavra_via` preexistentes. O delta passa pela porta de identidade. (`app/chamados/models.py:2651`, `app/chamados/models.py:2735`, `app/chamados/models.py:2764`; `app/chamados/services/relavra_via.py:123`; `app/chamados/services/fio.py:891`)

Contraexemplos relevantes:

1. **Canônica DAQUI + órfã respondida:** é possível se a daqui também tem resposta e menor PK. `canonica_do_grupo` escolhe a daqui; a adoção pula costura, evento e reabertura e retrata a órfã. A órfã respondida pode continuar viva pelo C6 e fica presa na âncora terminal, agora excluída das próximas adoções. (`app/chamados/services/fio.py:873`; `app/chamados/models.py:477`, `app/chamados/models.py:499`, `app/chamados/models.py:514`; `app/chamados/juizes.py:1150`, `app/chamados/juizes.py:1171`)

2. **Retratar pergunta daqui já respondida não garante uma só viva.** A irmã local respondida pode continuar viva mesmo após `VIA_DUPLICATA`; a própria porta documenta isso. O delta não relê C6 depois da retratação. (`app/chamados/models.py:514`, `app/chamados/models.py:2716`; `app/chamados/juizes.py:1150`) Isso pode conservar duas vivas no destino e fazer a duplicata respondida segurar o gate, pois `_ancoradas` filtra validação, não via. (`app/chamados/models.py:327`, `app/chamados/models.py:351`, `app/chamados/models.py:402`)

3. **Disputas diferentes:** quando a canônica órfã pertence a D1 e a local a D2, apenas D1 reabre; a irmã de D2 pode virar duplicata sem atualização do continente D2. Quando a canônica já é local, nem a disputa dela é reaberta. (`app/chamados/models.py:498`, `app/chamados/models.py:512`, `app/chamados/models.py:514`)

4. **Disputa de outro chamado vivo:** a guarda protege a âncora efetiva da pergunta, não `disputa.chamado`. Com destino explícito terminal e container vivo diferente, a pergunta é candidata e o delta pode reabrir a disputa desse outro container. (`app/chamados/services/fio.py:117`; `app/chamados/models.py:481`, `app/chamados/models.py:487`, `app/chamados/models.py:513`) `disputa_viva_do_chamado` passa a enxergar a disputa aberta e suas perguntas independentemente do destino delas, podendo alterar o bloqueio de fechamento do container vivo. (`app/chamados/juizes.py:2080`, `app/chamados/juizes.py:2084`)

5. **Migrador continua com universo diferente:** ele não exclui `VIA_DUPLICATA`; chama C6 sobre todas as resolvedoras. Uma duplicata respondida excluída pela adoção pode entrar na eleição do migrador. Além disso, o migrador continua retratando antes da costura, diferentemente da correção local P2. (`app/chamados/services/fio.py:931`, `app/chamados/services/fio.py:935`, `app/chamados/services/fio.py:941`, `app/chamados/services/fio.py:943`, `app/chamados/services/fio.py:947`; `app/chamados/models.py:477`)

6. **Leitores não compartilham um universo único:** `fio_vivo` reduz C6 sem olhar estado da disputa; vigia delega ao mesmo juiz; reabrir a disputa muda os recortes SQL do reconciliador e do fio, sem mudar o resultado de C6 por si só. (`app/chamados/juizes.py:1242`; `app/chamados/services/vigia_fio.py:165`; `app/chamados/reconciliador.py:1038`, `app/chamados/reconciliador.py:1482`; `app/chamados/services/fio.py:214`) O fechamento equivalente localizado é `_fechar_disputa_se_quitada`, que usa C7 (`pode_fechar`); a resposta duplicada viva continua segurando esse caminho. (`app/chamados/services/materializacao.py:1224`, `app/chamados/services/materializacao.py:1238`; `app/chamados/models.py:1885`)

**R08f é especialmente estreito:** `_disputa()` cria continente sem chamado; o teste só exige destino correto e `fechada_em=None`, sem campos restantes, ocorrência, acesso por tela/API ou perguntas colaterais. (`app/chamados/tests/test_c20_fio_mudo.py:54`, `app/chamados/tests/test_c20_fio_mudo.py:220`, `app/chamados/tests/test_c20_fio_mudo.py:227`, `app/chamados/tests/test_c20_fio_mudo.py:229`) O leitor HTML `montar_chamados_fio` exclui disputas sem chamado, logo o próprio cenário não prova a promessa “colaborador alcança a pergunta” por esse leitor. (`app/chamados/services/fio.py:181`, `app/chamados/services/fio.py:183`)

## 05/11 22:48 · sessao e720195d15d6 · F-346 · verificador · origem=Code

**O delta cura estaticamente o achado (1): a troca persiste exatamente os cinco campos que altera, preservando os demais. R06/R07 têm mordida para os mutantes anunciados, mas deixam lacunas específicas.** (`app/ferias/services.py:379`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:392`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`)

Executei somente a extração do AST em memória. Execução Django, originais/mutantes em banco, incidência em produção e conferência independente de hash/pai/base: **[nao verificado]**. [nao verificado]

**1. Cura, campos e chamadores**

Não ficou fora nenhum campo de AG atribuído pela troca. O ajuste de `PA.dias_concedidos/status` continua persistido separadamente. O override continua chamando guarda, persistência e sincronização; incluir as duas datas mantém habilitada a guarda de sobreposição. (`app/ferias/services.py:378`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/models.py:397`, `app/ferias/models.py:434`)

| Campo persistido | Quem escreve | Quem lê |
|---|---|---|
| `data_inicio` | Criações histórico/execução/aprovação; alteração pela troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:436`, `app/ferias/services.py:379`) | Guarda de edição, coerência, colisão, materialização, consultas por janela e apresentação. (`app/ferias/models.py:324`, `app/ferias/models.py:373`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1168`, `app/ferias/services.py:51`, `app/templates/ferias/_secao_colab.html:117`) |
| `data_fim` | Mesmas criações e troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:437`, `app/ferias/services.py:379`) | Coerência, colisão, materialização, consultas por janela e apresentação. (`app/ferias/models.py:370`, `app/ferias/models.py:429`, `app/ponto/services/ausencia.py:1168`, `app/ferias/services.py:51`, `app/templates/ferias/_secao_colab.html:118`) |
| `dias` | Mesmas criações e troca. (`app/ferias/services.py:281`, `app/ferias/services.py:323`, `app/ferias/services.py:438`, `app/ferias/services.py:379`) | Saldo para remarcar, cancelamentos, ajuste do PA, coerência, fracionamento e apresentação. (`app/ferias/models.py:222`, `app/ferias/services.py:333`, `app/ferias/services.py:353`, `app/ferias/services.py:378`, `app/ferias/models.py:374`, `app/ferias/situacao.py:122`, `app/templates/ferias/_secao_colab.html:119`) |
| `aviso_emitido` | Default; execução; aprovação; troca. (`app/ferias/models.py:305`, `app/ferias/services.py:323`, `app/ferias/services.py:439`, `app/ferias/services.py:380`) | KPI/filtro, situação, mensagem da remarcação e apresentação. (`app/ferias/views.py:26`, `app/ferias/views.py:77`, `app/ferias/situacao.py:257`, `app/ferias/views.py:671`, `app/templates/ferias/_secao_colab.html:121`) |
| `status` | Default; histórico; cancelamentos; troca; alinhamento do cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:518`) | Guardas, consultas por janela, fracionamento, ativação da materialização e apresentação. (`app/ferias/views.py:452`, `app/ferias/views.py:649`, `app/ferias/services.py:51`, `app/ferias/situacao.py:119`, `app/ponto/services/ausencia.py:1164`, `app/templates/ferias/_secao_colab.html:120`) |

| Campo excluído do UPDATE | Quem escreve e quem lê |
|---|---|
| `periodo_aquisitivo_id` | Preenchido nas criações; lido pela troca, guarda, saldo e materialização. A troca não o altera. (`app/ferias/services.py:279`, `app/ferias/services.py:322`, `app/ferias/services.py:435`, `app/ferias/services.py:375`, `app/ferias/models.py:354`, `app/ponto/services/ausencia.py:1163`) |
| `agendado_por_id` | Preenchido nas criações; lido como autor das ausências e no filtro do lote. (`app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:440`, `app/ponto/services/ausencia.py:1189`, `app/ferias/views.py:536`) |
| `obs` | Preenchido nas criações; cancelamento admin lê, concatena e persiste o carimbo. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:441`, `app/ferias/services.py:360`, `app/ferias/services.py:362`) |
| `confirmacao` | Pedido e resposta escrevem; envio, resposta, atendimento e template leem. (`app/ferias/services.py:486`, `app/ferias/services.py:496`, `app/ferias/services.py:499`, `app/ferias/views.py:838`, `app/ferias/views.py:880`, `app/chamados/views.py:628`, `app/templates/ferias/_secao_colab.html:122`) |
| `confirmado_em` | Pedido limpa e confirmação carimba; teste lê o carimbo. Leitor específico de runtime: **[nao verificado]**. (`app/ferias/services.py:487`, `app/ferias/services.py:500`, `app/ferias/test_confirmacao_agendamento.py:76`) |
| `criado_em` | Nasce por `auto_now_add`; leitor específico de runtime: **[nao verificado]**. (`app/ferias/models.py:319`) |
| PK | Identifica o agendamento carregado pelas views e apresentado pela API; a troca não lhe atribui novo valor. (`app/ferias/views.py:439`, `app/ferias/views.py:638`, `app/api/views.py:2258`, `app/ferias/services.py:379`) |

Nos dois chamadores de runtime localizados, **não há dependência do save completo**: nenhum altera os campos excluídos antes da chamada. A correção prepara auditoria e depois recarrega os objetos; a remarcação lê `aviso_emitido`, ainda atribuído e persistido pela troca. (`app/ferias/views.py:439`, `app/ferias/views.py:470`, `app/ferias/views.py:490`, `app/ferias/views.py:496`, `app/ferias/views.py:638`, `app/ferias/views.py:668`, `app/ferias/views.py:671`, `app/ferias/services.py:392`)

**2. Mordida dos testes** [nao verificado]

**R06 morde a retirada isolada do `atomic`.** O DELETE precede o veto; sem o bloco do serviço, a exceção da guarda não desfaz aquele DELETE. O `TestCase` externo não mascara isso: a comparação ocorre depois de `assertRaises`, antes do encerramento do teste. A comparação de ausências discrimina o mutante; a de saldo acrescenta pouco nesse cenário, pois os dias permanecem 10 e o save do PA viria depois do veto. Essa é uma dedução estática. (`app/ferias/tests.py:19`, `app/ferias/services.py:377`, `app/ferias/services.py:378`, `app/ferias/services.py:387`, `app/ferias/services.py:394`, `app/ferias/models.py:432`, `app/ferias/test_remarcar_limpa_janela_f346.py:100`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`)

**R07 morde a volta ao save completo**, ou a inclusão indevida de `confirmacao` na lista: carrega antes, altera a coluna diretamente, remarca com o objeto antigo e recarrega. Contudo, acrescentar somente `confirmado_em` ou somente `obs` ao UPDATE passaria: o caso não modifica nem confere esses campos. (`app/ferias/test_remarcar_limpa_janela_f346.py:110`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/services.py:392`)

| Mutante | Resultado deduzido dos casos |
|---|---|
| **Save antes do DELETE**, mantendo `exceto` | **R01–R07 deixam passar.** A guarda exclui as ausências antigas; sync e limpeza posterior chegam aos mesmos resultados observados. R06 veta antes da limpeza. Os casos não certificam a ordem nem os efeitos intermediários. (`app/ferias/models.py:420`, `app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1252`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`) |
| **Omitir `exceto`** | R06/R07 isolados deixam passar; **R02/R03 mordem**, pois exigem preservar as PKs da interseção/borda. (`app/ferias/test_remarcar_limpa_janela_f346.py:64`, `app/ferias/test_remarcar_limpa_janela_f346.py:72`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`) |
| **Ler janela antiga depois da atribuição** | R06/R07 isolados deixam passar; **R01 morde**, pois a janela antiga permanece materializada. (`app/ferias/services.py:379`, `app/ponto/services/ausencia.py:1250`, `app/ferias/test_remarcar_limpa_janela_f346.py:56`) |
| Omitir **`data_inicio` ou `data_fim`** do UPDATE | **R01 morde**, recarregando e exigindo ambas as datas novas. (`app/ferias/test_remarcar_limpa_janela_f346.py:47`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`) |
| Omitir **`dias`** | **R01–R07 deixam passar**: R04 confere ausências e saldo do PA, sem conferir `AG.dias`. Há asserção discriminante nos testes anteriores de remarcação/admin. (`app/ferias/test_remarcar_limpa_janela_f346.py:74`, `app/ferias/test_remarcar_limpa_janela_f346.py:78`, `app/ferias/test_editar_agendamento.py:59`, `app/ferias/test_editar_datas_admin.py:93`) |
| Omitir **`aviso_emitido`** | **R01–R07 deixam passar**; a mensagem usa o objeto em memória, que recebeu a atribuição mesmo sem persistência. (`app/ferias/services.py:380`, `app/ferias/views.py:671`, `app/ferias/test_remarcar_limpa_janela_f346.py:46`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`) |
| Omitir **`status`** | **R01–R07 deixam passar**: partem de `agendado` e mantêm datas futuras. O teste anterior de concluído → futuro tem asserção discriminante. (`app/ferias/test_remarcar_limpa_janela_f346.py:29`, `app/ferias/test_remarcar_limpa_janela_f346.py:32`, `app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_a3_situacao_de_ferias.py:578`, `app/ferias/test_a3_situacao_de_ferias.py:581`) |

**3. AST ampliado**

Executando o helper em memória sobre o arquivo atual, obtive **`['_trocar_janela']`**, sem falso positivo atual. O universo examinado continua restrito a `services.py`. (`app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/ferias/test_remarcar_limpa_janela_f346.py:121`, `app/ferias/services.py:379`)

Há um **falso negativo bastante plausível**, confirmado em memória:

```python
def novo(pa, i):
    AgendamentoFerias.objects.filter(periodo_aquisitivo=pa).update(data_inicio=i)
```

O detector retorna `[]`: a substring `periodo` aparece no texto da chamada por causa do filtro, e o selo descarta o escritor inteiro, embora o modelo seja AG. (`app/ferias/test_remarcar_limpa_janela_f346.py:144`)

Também escapam `qs.update(**campos)` com dicionário preparado antes e `setattr(ag, campo, valor)` com nome de campo variável: o detector exige nomes/constantes presentes na própria chamada. (`app/ferias/test_remarcar_limpa_janela_f346.py:141`, `app/ferias/test_remarcar_limpa_janela_f346.py:145`)

Como falso positivo sintético, `ag.data_inicio: int`, sem atribuição, é contado como escritor. O ramo `AnnAssign` não exige valor e não recebeu uma sentinela sintética correspondente no teste de mordida. (`app/ferias/test_remarcar_limpa_janela_f346.py:136`, `app/ferias/test_remarcar_limpa_janela_f346.py:172`)

Portanto, o selo ampliou a cobertura sintática, mas continua verificando contagem e presença nominal de chamadas; não prova transação, ordem, argumentos da limpeza ou persistência dos cinco campos. (`app/ferias/test_remarcar_limpa_janela_f346.py:162`, `app/ferias/test_remarcar_limpa_janela_f346.py:167`)

**4. Triagem do achado (2)**

**Na cópia atual, o padrão é compartilhado pelas duas portas:** ambas recebem objeto carregado na view e delegam à função que captura a janela antiga antes do `atomic`, sem recarga protegida. (`app/ferias/views.py:439`, `app/ferias/views.py:638`, `app/ferias/services.py:376`, `app/ferias/services.py:377`, `app/ferias/services.py:400`, `app/ferias/services.py:407`)

A docstring declara que a ordem já existia na correção anterior; o anexo v1 também sustenta essa origem. **O corpo anterior à obra, no commit base, não pôde ser conferido independentemente: [nao verificado].** O comentário atual é evidência documental, não substitui a leitura histórica. (`app/ferias/services.py:369`)

Recarregar simplesmente dentro do `atomic` **não elimina a corrida**: ainda permite duas leituras da mesma janela antes das escritas. A leitura feita pela guarda tampouco atualiza `_ini_ant/_fim_ant`, usados na limpeza. (`app/ferias/services.py:376`, `app/ferias/services.py:387`, `app/ferias/models.py:400`, `app/ferias/models.py:420`)

Não sustentaria a formulação absoluta “exige `select_for_update`”. Uma alternativa de desenho seria UPDATE condicionado à janela/versão esperada, com aborto de todo o `atomic` se houver conflito, preservando guarda e sync. Isso evita necessariamente acrescentar um SELECT com trava, mas continua envolvendo a trava inerente ao UPDATE e exige resolver sua integração com o chokepoint atual. **Alternativa não implementada nem validada nesta cópia: [nao verificado].** (`app/ferias/models.py:434`, `app/ferias/services.py:377`, `app/ferias/services.py:392`)

Assim, **manter a concorrência em fila de desenho é defensável**, mas a exclusividade de `select_for_update` como cura não está demonstrada. Já há ordens opostas no código: troca faz Ausência → AG → PA; cancelamento admin começa PA → Ausência → AG. (`app/ferias/services.py:387`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/services.py:355`, `app/ferias/services.py:356`, `app/ferias/services.py:362`)

**5. Classes novas no delta**

| Classe | Avaliação |
|---|---|
| **2º juiz** | Não identificado: permanecem os juízes de status, aviso e colisão. (`app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:393`, `app/ferias/models.py:428`) |
| **2º escritor fora da porta** | Não identificado no produto; o delta restringe a escrita dentro da porta registrada. (`app/ferias/services.py:392`, `app/core/portas.py:488`) |
| **Relógio fora da borda** | Permanece, sem leitura nova neste delta: serviço e juízes consultam o relógio separadamente. (`app/ferias/services.py:380`, `app/ferias/models.py:280`, `app/ferias/models.py:94`) |
| **Derivação paralela** | Não identificada: continuam as mesmas derivações e autoridades. (`app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/models.py:212`) |
| **Selo sem mordida** | Os novos casos mordem seus alvos anunciados; permanecem as lacunas de campos e do AST descritas acima. (`app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/test_remarcar_limpa_janela_f346.py:144`) |
| **Ordem de trava** | Nenhuma nova ordem explícita introduzida neste delta: limpeza → save/sync → PA permanece. A questão concorrente continua aberta. (`app/ferias/services.py:387`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/models.py:434`) |
| **Pouso antes de pré-requisito** | Não aparece nova dependência de schema: os cinco campos já existem. Migrações aplicadas, suite e condições de deploy: **[nao verificado]**. (`app/ferias/models.py:302`, `app/ferias/models.py:305`, `app/ferias/models.py:309`) |

## 05/11 22:46 · sessao 49ee82a1baf3 · - · - · origem=Ronald (tmux)

**R06/R07 têm mordida estática para os mutantes anunciados; execução Django/banco dos originais e mutantes: [nao verificado].** R06 chama o serviço diretamente, provoca colisão materializada e compara ausências/saldo/janela após a exceção; o veto levanta antes do `super().save()`. Sem o `atomic` do serviço, o DELETE anterior ao veto não é desfeito apenas por `assertRaises`; o `TestCase` externo permanece aberto durante a comparação. (`app/ferias/test_remarcar_limpa_janela_f346.py:87`, `app/ferias/test_remarcar_limpa_janela_f346.py:98`, `app/ferias/test_remarcar_limpa_janela_f346.py:100`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`, `app/ferias/tests.py:19`, `app/ferias/services.py:377`, `app/ferias/services.py:387`, `app/ferias/models.py:428`, `app/ferias/models.py:434`)

R07 carrega o objeto com confirmação antiga, faz `QuerySet.update(confirmacao='confirmado')`, remarca com o objeto antigo e recarrega a linha. Voltar ao save completo, ou recolocar `confirmacao` no `update_fields`, regrava o valor antigo e contraria a asserção. (`app/ferias/test_remarcar_limpa_janela_f346.py:107`, `app/ferias/test_remarcar_limpa_janela_f346.py:110`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/models.py:314`, `app/ferias/services.py:392`)

R07 **não** verifica concorrência sobre `confirmado_em` ou `obs`: esses campos não são alterados entre carga e remarcação nem conferidos depois. Um mutante que acrescentasse somente `confirmado_em` ou somente `obs` ao `update_fields` preservaria a asserção atual. (`app/ferias/test_remarcar_limpa_janela_f346.py:110`, `app/ferias/test_remarcar_limpa_janela_f346.py:111`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/services.py:392`)

| Mutante | Mordida estática dos casos F-346 |
|---|---|
| Save antes do DELETE, mantendo `exceto` e `atomic` | **Passa pelos resultados observados.** A guarda exclui as ausências da janela antiga; o sync preserva interseção/cria dias novos; o DELETE posterior com `exceto` retira apenas antiga − nova. R06 veta antes de alterar e igualmente preserva o estado. Isso muda ordem/efeitos intermediários, mas esses casos conferem o resultado final, não a ordem. (`app/ferias/models.py:420`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1182`, `app/ponto/services/ausencia.py:1252`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`, `app/ferias/test_remarcar_limpa_janela_f346.py:103`) |
| Omitir `exceto` | **R02/R03 mordem:** interseção e dia de borda são apagados e recriados com outra PK. (`app/ferias/test_remarcar_limpa_janela_f346.py:61`, `app/ferias/test_remarcar_limpa_janela_f346.py:64`, `app/ferias/test_remarcar_limpa_janela_f346.py:69`, `app/ferias/test_remarcar_limpa_janela_f346.py:72`, `app/ponto/services/ausencia.py:1252`) |
| Capturar antiga depois da atribuição | **R01 morde:** o helper passa a receber a própria janela nova, inteiramente excluída por `exceto`, deixando a antiga materializada. (`app/ferias/services.py:376`, `app/ferias/services.py:379`, `app/ferias/services.py:387`, `app/ponto/services/ausencia.py:1250`, `app/ferias/test_remarcar_limpa_janela_f346.py:56`) |
| Omitir `data_inicio` ou `data_fim` do save | **R01 morde:** recarrega e exige ambas as datas novas. (`app/ferias/test_remarcar_limpa_janela_f346.py:47`, `app/ferias/test_remarcar_limpa_janela_f346.py:55`) |
| Omitir `dias` | **R01–R07 deixam passar:** R04 muda 10→5, mas só confere ausências e `PA.dias_concedidos`; sync usa datas, sem ler `ag.dias`. Existe mordida fora desse arquivo: `test_editar_reajusta_saldo_do_periodo` e teste admin conferem `ag.dias` após redução. (`app/ferias/test_remarcar_limpa_janela_f346.py:74`, `app/ferias/test_remarcar_limpa_janela_f346.py:78`, `app/ponto/services/ausencia.py:1178`, `app/ferias/test_editar_agendamento.py:52`, `app/ferias/test_editar_agendamento.py:59`, `app/ferias/test_editar_datas_admin.py:88`, `app/ferias/test_editar_datas_admin.py:93`) |
| Omitir `aviso_emitido` | **R01–R07 deixam passar:** não o conferem; a view usa o objeto em memória, já atribuído pelo serviço. (`app/ferias/test_remarcar_limpa_janela_f346.py:49`, `app/ferias/test_remarcar_limpa_janela_f346.py:114`, `app/ferias/services.py:380`, `app/ferias/views.py:671`) |
| Omitir `status` | **R01–R07 deixam passar:** todos partem de `agendado` e movem para futuro. Há mordida fora deles na correção de concluído para futuro. (`app/ferias/test_remarcar_limpa_janela_f346.py:23`, `app/ferias/test_remarcar_limpa_janela_f346.py:29`, `app/ferias/test_remarcar_limpa_janela_f346.py:32`, `app/ferias/test_remarcar_limpa_janela_f346.py:112`, `app/ferias/test_a3_situacao_de_ferias.py:571`, `app/ferias/test_a3_situacao_de_ferias.py:581`) |

**AST:** extraí `trocadores_de_janela`/`chamadas` e executei exclusivamente em memória com `python3 -B`, sem importar Django nem escrever arquivos: resultado atual `['_trocar_janela']`; portanto, não houve falso positivo sobre o `services.py` atual. O detector olha apenas esse arquivo e exclui literalmente `self`/`periodo`; a única atribuição de AG encontrada pelo detector é a de `_trocar_janela`. (`app/ferias/test_remarcar_limpa_janela_f346.py:117`, `app/ferias/test_remarcar_limpa_janela_f346.py:121`, `app/ferias/test_remarcar_limpa_janela_f346.py:134`, `app/ferias/services.py:379`)

Contraexemplos sintéticos também executados em memória:

```python
def x(qs, i):
    campos = {"data_inicio": i}
    qs.update(**campos)            # resultado []

def x(ag, i):
    campo = "data_inicio"
    setattr(ag, campo, i)          # resultado []

def x(ag_por_periodo, i):
    ag_por_periodo.update(data_inicio=i)  # resultado []
```

Os dois primeiros escapam porque o selo exige keyword explícita ou constante dentro da própria chamada; o terceiro escapa porque a exclusão testa a substring `periodo` em todo o nome da função chamada, sem identificar o modelo. Alias de `setattr`, atribuição a lista (`[ag.data_inicio, ag.data_fim] = ...`), alvo `self` e `AsyncFunctionDef` também escaparam nos exemplos executados. (`app/ferias/test_remarcar_limpa_janela_f346.py:126`, `app/ferias/test_remarcar_limpa_janela_f346.py:132`, `app/ferias/test_remarcar_limpa_janela_f346.py:134`, `app/ferias/test_remarcar_limpa_janela_f346.py:141`, `app/ferias/test_remarcar_limpa_janela_f346.py:144`)

Há falsos positivos **plausíveis**, não presentes no arquivo atual: `ag.data_inicio: int` sem valor é contado como escritor embora não atribua; qualquer outro tipo com `.data_inicio` também é contado, pois o selo não resolve o tipo do receptor. Ambos foram confirmados nos exemplos em memória. (`app/ferias/test_remarcar_limpa_janela_f346.py:133`, `app/ferias/test_remarcar_limpa_janela_f346.py:136`)

O selo estrutural tampouco prova `atomic`, ordem, parâmetros de DELETE ou persistência: exige um escritor, presença nominal da chamada de limpeza e delegação das duas portas. Sua ampliação tem mordida para as formas adicionadas, mas não constitui garantia semântica. (`app/ferias/test_remarcar_limpa_janela_f346.py:162`, `app/ferias/test_remarcar_limpa_janela_f346.py:165`, `app/ferias/test_remarcar_limpa_janela_f346.py:167`, `app/ferias/test_remarcar_limpa_janela_f346.py:177`)

## 05/11 22:45 · sessao a914ed5cd748 · - · - · origem=Ronald (tmux)

**O delta cura estaticamente o achado (1): os cinco campos de AG atribuídos pela troca são exatamente os cinco de `update_fields`; o PA continua salvo separadamente. A guarda e a sincronização continuam executadas pelo override.** (`app/ferias/services.py:378`, `app/ferias/services.py:379`, `app/ferias/services.py:380`, `app/ferias/services.py:384`, `app/ferias/services.py:392`, `app/ferias/services.py:394`, `app/ferias/models.py:434`)

| Campo AG | Quem escreve | Quem lê |
|---|---|---|
| `data_inicio` | Criações histórico/execução/aprovação; alteração existente pela troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:436`, `app/ferias/services.py:379`) | Guarda de edição; coerência; colisão; materialização; API/tabela. (`app/ferias/models.py:324`, `app/ferias/models.py:373`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1168`, `app/api/views.py:2259`, `app/templates/ferias/_tabela_ferias.html:39`) |
| `data_fim` | Mesmas criações e troca. (`app/ferias/services.py:280`, `app/ferias/services.py:322`, `app/ferias/services.py:437`, `app/ferias/services.py:379`) | Coerência; colisão; materialização; API/tabela. (`app/ferias/models.py:370`, `app/ferias/models.py:429`, `app/ponto/services/ausencia.py:1168`, `app/api/views.py:2260`, `app/templates/ferias/_tabela_ferias.html:40`) |
| `dias` | Mesmas criações e troca. (`app/ferias/services.py:281`, `app/ferias/services.py:323`, `app/ferias/services.py:438`, `app/ferias/services.py:379`) | Saldo de remarcação; cancelamento; ajuste do PA; fracionamento; coerência; API. (`app/ferias/models.py:222`, `app/ferias/services.py:333`, `app/ferias/services.py:378`, `app/ferias/situacao.py:122`, `app/ferias/models.py:374`, `app/api/views.py:2261`) |
| `aviso_emitido` | Default; execução; aprovação; troca. (`app/ferias/models.py:305`, `app/ferias/services.py:323`, `app/ferias/services.py:439`, `app/ferias/services.py:380`) | KPI/filtro; situação; resposta da remarcação; tabela. (`app/ferias/views.py:26`, `app/ferias/views.py:77`, `app/ferias/situacao.py:257`, `app/ferias/views.py:671`, `app/templates/ferias/_tabela_ferias.html:43`) |
| `status` | Default; histórico; cancelamentos; troca; cron. (`app/ferias/models.py:309`, `app/ferias/services.py:282`, `app/ferias/services.py:336`, `app/ferias/services.py:361`, `app/ferias/services.py:384`, `app/ferias/services.py:518`) | Guardas de portas; frações; guarda de sobreposição; ativação da materialização; API. (`app/ferias/views.py:452`, `app/ferias/views.py:649`, `app/ferias/situacao.py:119`, `app/ferias/models.py:394`, `app/ponto/services/ausencia.py:1164`, `app/api/views.py:2262`) |

| Campo fora do UPDATE | Quem escreve e quem lê |
|---|---|
| `periodo_aquisitivo_id` | Criações preenchem; troca não altera. Guarda, saldo, sync e tabela leem a relação. (`app/ferias/services.py:279`, `app/ferias/services.py:322`, `app/ferias/services.py:435`, `app/ferias/services.py:375`, `app/ferias/models.py:354`, `app/ferias/models.py:428`, `app/ponto/services/ausencia.py:1163`, `app/templates/ferias/_tabela_ferias.html:36`) |
| `agendado_por_id` | Criações preenchem; sync usa como autor da ausência e lote filtra por ele. (`app/ferias/services.py:283`, `app/ferias/services.py:323`, `app/ferias/services.py:440`, `app/ponto/services/ausencia.py:1189`, `app/ferias/views.py:536`) |
| `obs` | Criações preenchem; cancelamento admin lê, concatena carimbo e salva. (`app/ferias/services.py:284`, `app/ferias/services.py:323`, `app/ferias/services.py:441`, `app/ferias/services.py:360`, `app/ferias/services.py:362`) |
| `confirmacao` | Pedido e resposta escrevem; envio, resposta, atendimento e template leem. (`app/ferias/services.py:486`, `app/ferias/services.py:496`, `app/ferias/services.py:499`, `app/ferias/views.py:838`, `app/ferias/views.py:880`, `app/chamados/views.py:628`, `app/templates/ferias/_secao_colab.html:122`) |
| `confirmado_em` | Pedido limpa; confirmação carimba. Leitor de runtime específico não localizado: **[nao verificado]**; teste lê o carimbo. (`app/ferias/services.py:487`, `app/ferias/services.py:500`, `app/ferias/test_confirmacao_agendamento.py:76`) |
| `criado_em` | `auto_now_add`; leitor específico de runtime para AG não localizado: **[nao verificado]**. (`app/ferias/models.py:319`) |
| PK | Identifica os objetos usados pelas portas/views e aparece na API; necessidade de regravá-la pela troca não demonstrada. (`app/ferias/views.py:439`, `app/ferias/views.py:638`, `app/api/views.py:2258`) |

**Nenhum dos dois chamadores de runtime localizados depende de persistir outros campos de AG.** A correção carrega o objeto, valida/consulta saldo, prepara auditoria e chama o serviço; depois recarrega os objetos. A remarcação carrega, valida datas/saldo e chama o serviço; depois lê `aviso_emitido`, que continua atribuído e persistido. (`app/ferias/views.py:439`, `app/ferias/views.py:465`, `app/ferias/views.py:470`, `app/ferias/views.py:490`, `app/ferias/views.py:496`, `app/ferias/views.py:638`, `app/ferias/views.py:661`, `app/ferias/views.py:668`, `app/ferias/views.py:671`)

O save limitado ainda inclui ambas as datas; portanto não aciona o retorno antecipado reservado a updates sem datas. O override chama guarda, `super().save` e sync na mesma sequência. (`app/ferias/services.py:392`, `app/ferias/models.py:397`, `app/ferias/models.py:434`)

Execução de testes, SQL efetivo e cobertura de caminhos dinâmicos/externos: **[nao verificado]**. [nao verificado]

## 05/11 22:27 · sessao e05481d49698 · H12H06-B-SIMPLES · sonda · origem=Code

Saída fornecida, transcrita literalmente; não houve nova execução da sonda. Execução e hash informados: [não verificado].

```text
sonda_leitura: sonda_h12_comp10.py [8d165ce05418159e] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

emp2 janela=2026-09-21..2026-10-20 entregue=False exp=None hash=None fm=477 demitidos=71 demissao_apos_fim=12 no_txt=0 ids=col343,col574,col878,col981,col518,col318,col912,col663,col973,col270,col699,col907
emp3 janela=2026-09-21..2026-10-20 entregue=False exp=None hash=None fm=119 demitidos=18 demissao_apos_fim=7 no_txt=0 ids=col761,col79,col90,col752,col984,col755,col641
emp4 janela=2026-09-21..2026-10-20 entregue=False exp=None hash=None fm=24 demitidos=0 demissao_apos_fim=0 no_txt=0 ids=
```

Conforme exclusivamente o dado raspado fornecido, sem validação independente: [não verificado].

| Empresa | 10/2026 entregue, vigente não invalidada? | Colaboradores do FM com demissão após 20/10 | Destes, com linha no TXT vigente | Fonte |
|---|---|---:|---:|---|
| 2 | Não (`entregue=False`, `exp=None`) | 12 | 0 | Dado fornecido [não verificado] |
| 3 | Não (`entregue=False`, `exp=None`) | 7 | 0 | Dado fornecido [não verificado] |
| 4 | Não (`entregue=False`, `exp=None`) | 0 | 0 | Dado fornecido [não verificado] |

O código define entrega pela existência de exportação vigente com `invalidada_em is None`. [app/folha/export.py:580](app/folha/export.py:580) [app/folha/export.py:583](app/folha/export.py:583)

Na comparação solicitada:

- **Antes do POUSO A:** o registro da alteração descreve o predicado anterior como `if colab.data_demissao is not None:`, sem consultar a janela; esses colaboradores seriam excluídos como rescisão. [app/docs/RELATO.md:90](app/docs/RELATO.md:90) [app/folha/tests/test_rescisao_modulo_proprio.py:44](app/folha/tests/test_rescisao_modulo_proprio.py:44)
- **Atual, na cópia:** `sai_pela_rescisao` exige demissão até o fim da janela, inclusive. Demissão posterior deixa de causar `fora / rescisao_modulo_proprio` e segue para os demais filtros. [app/folha/export.py:60](app/folha/export.py:60) [app/folha/export.py:858](app/folha/export.py:858) [app/folha/export.py:864](app/folha/export.py:864)

Portanto, numa nova geração, **os 12 da empresa 2 e os 7 da empresa 3 deixariam de ser excluídos por esse motivo**; na empresa 4, o grupo informado é vazio. Quantidades: dado fornecido [não verificado]; efeito do predicado: [app/folha/export.py:60](app/folha/export.py:60) [app/folha/export.py:858](app/folha/export.py:858).

Isso não demonstra que ganhariam linhas: ainda existem filtros de código, férias, cadastro, espelho e homologação, além das condições da montagem. **Quantos efetivamente entrariam, com quais rubricas e valores: [não verificado].** [app/folha/export.py:760](app/folha/export.py:760) [app/folha/export.py:960](app/folha/export.py:960) [app/folha/export.py:708](app/folha/export.py:708) [app/folha/export.py:725](app/folha/export.py:725)

Identidade da cópia com o HEAD solicitado: [não verificado].

## 05/11 22:20 · sessao 1640fe6c71e4 · PD-H12H06-B · censo · origem=Code

**O ponto decisivo: reexecutar `save()` sobre `minutos=720` preserva os 720. A derivação automática existe apenas para `falta`, aprovada, com minutos zero; não existe para atestado.** `app/ponto/models.py:877-887`.

**Também há impacto além da duração:** atestado de um dia passando de 720 para menos de 480 pode virar cobertura parcial, conforme o cadastro vivo do tipo. `app/ponto/turnos.py:2580-2600`.

**1. Escritores e possibilidade de substituir o positivo**

| Escritor | Decisão relevante |
|---|---|
| **`Ausencia.save()`** | Aceita um novo positivo atribuído pelo chamador e o grava; não recalcula positivo nem deriva atestado. Só calcula falta aprovada com minutos falsy. `app/ponto/models.py:877-887,908`. |
| **`criar_ausencia()`** | Valida minutos contra `parciais_vigentes()`, salvo `checar_parcial=False`; cria e chama `save`. Nos modos de devolver existente, retorna a linha sem corrigir minutos. `app/ponto/services/ausencia.py:206-209,246-252,294-314,385,417-418`. |
| **`garantir_ausencia()`** | Usa `se_existir='retornar'`: existente fica intacta. `app/ponto/services/ausencia.py:453-462,311-314`. |
| **`editar_ausencia_registro()`** | Substitui positivo, mas recusa minutos truthy se o tipo não estiver em `parciais_vigentes()`. Com `None/0`, zera; somente falta aprovada será derivada pelo modelo. Também revalida sobreposição e reescreve datas/range. `app/ponto/services/ausencia.py:473-508`. |
| **`mudar_estado()`** | Mesmo estado retorna sem salvar. Mudando estado, inclui `minutos` em `update_fields`, mas não recebe um novo valor nem zera o anterior; portanto conserva 720. `app/ponto/services/ausencia.py:801-819`. |
| **`decidir()`** | Chama `mudar_estado`, altera carimbos, descrição e período; não recebe minutos. Não é uma porta de recomposição da duração. `app/ponto/services/ausencia.py:881-911`. |
| **Materialização** | Hoje omite minutos: falta aprovada deriva; atestado nasce com zero, documentado como dia inteiro. Existente pode voltar sem atualização. `app/chamados/services/materializacao.py:484-492,519-528`. |
| **`rederivar_fim()`** | Apenas `save()` e `refresh_from_db()`; não corrige 720 positivo. `app/ponto/services/ausencia.py:1277-1282`; `app/ponto/models.py:877`. |
| **Espelhamento de férias** | Cria com minutos zero; ignora dia existente. `app/ponto/services/ausencia.py:1182-1191`. |
| **Django admin** | `AusenciaAdmin` é somente leitura; não permite adicionar, alterar ou excluir. `app/ponto/admin.py:37-48`; `app/core/admin.py:59-75`. |

Os trechos que decidem são:

```python
if self.status == 'aprovada' and self.tipo == 'falta' and not self.minutos:
    ...
    self.minutos = _tot
...
super().save(*args, **kwargs)
```

`app/ponto/models.py:877-908`.

```python
if minutos and tipo not in _CATV.parciais_vigentes():
    raise AusenciaInvalida(..., codigo='tipo_nao_parcial')
...
ausencia.minutos = int(minutos or 0)
ausencia.save()
```

`app/ponto/services/ausencia.py:480-508`.

Portanto, **o modelo aceita gravar o previsto calculado externamente sobre um positivo**, desde que `minutos` entre nos campos efetivamente salvos; a edição de serviço só aceita esse positivo conforme o cadastro parcial do tipo e ainda altera/revalida outros campos. `app/ponto/models.py:877-908`; `app/ponto/services/ausencia.py:473-508`.

A permissão para lançar por minutos vem do cadastro vivo; sua situação nas 30 linhas não foi consultada. `app/ponto/catalogo/ausencias.py:336-358`; **[não verificado]**.

**2. Efeitos, gravações e travas**

A sequência verificável do caminho direto é esta; ela contém ramos condicionais, não uma lista fixa de entidades sempre escritas. `app/ponto/signals.py:85-93,143-147`; `app/ponto/services/cartorio.py:1358-1367,1488-1522,1799-1823`.

1. **Modelo → Ausencia:** carimba decisão sem `aprovada_em`, eventualmente deriva minutos, normaliza o range e chama `super().save()`. Não há `select_for_update` explícito nesse método. `app/ponto/models.py:868-908`.
2. **`post_save` → cartório, imediatamente:** salvo `raw`, chama `_julgar_dias`; o antirrecursão pode interromper. Percorre os dias da ausência, podendo alcançar várias células de um range mesmo que o comando salve uma única ausência. `app/ponto/signals.py:85-93,115-128,143-147`.
3. **Cartório → leitura da célula:** usa `.filter(...).first()`, sem trava explícita nessa busca; célula inexistente/futura ou turno corrente ainda não encerrado pode impedir o julgamento. `app/ponto/services/cartorio.py:1974-1987`.
4. **Cartório → efeitos condicionais:** pode reconciliar chamados antes de lavrar a célula, emitir cobrança e ressuscitar perguntas; depois pode reconciliar novamente e fabricar perguntas/disputas. Logo, “Ausencia → Celula e acabou” seria uma descrição incompleta. `app/ponto/services/cartorio.py:1488-1522,1600-1622,1777-1789,958-973,981-999`.
5. **Lavra → LogAuditoria → CelulaDia → EventoCelula:** no ramo normal, `lavrar_veredito(save=False)` prepara a célula e registra a trilha; `carimbar` efetiva o save. `salvar_e_emitir` grava primeiro a célula e depois insere o evento, no mesmo `atomic`. `app/ponto/services/cartorio.py:1800-1823`; `app/ponto/portas/celula.py:814-849,676-684`; `app/core/observ.py:133-138`.
6. **Precedência:** depois do julgamento, pode salvar novamente a ata da célula e emitir outro evento; igualdade evita essa escrita. `app/ponto/services/cartorio.py:1995-1997`; `app/ponto/portas/celula.py:853-864`.
7. **Consumidor, após commit:** agenda a devolução de perguntas absorvidas quando rejeitada; depois reconcilia chamados do colaborador e fecha chamados da ausência decidida. Com `atomic` externo, isso ocorre ao sair dele; não é um job enfileirado por esse código. `app/ponto/signals.py:250-279`; `app/chamados/services/silencio_ausencia.py:119-129`.

Nos ramos de chamado, **reconciliar abre `atomic`, trava o chamado com `select_for_update`, decide e grava**; a atualização do chamado vem antes da resolução/relavra das perguntas pelos receivers. Já o nascimento por `abrir` trava o **Colaborador** antes do dedup/criação. `app/chamados/models.py:616-620,638,668-675,261-279,863-867`; `app/chamados/signals.py:75-94`.

**`invalidar_fechamento` não é efeito automático de todo `Ausencia.save()`.** É uma chamada explícita de `decidir`, depois dos saves da decisão; ela percorre as competências afetadas e atualiza `FechamentoMensal.derivado_em=None`, com savepoint por competência e sem recalcular o motor. `app/ponto/services/ausencia.py:895-932`; `app/ponto/services/fechamento.py:1632-1640`.

Assim, um save direto dos minutos não garante atualização da projeção mensal; também não garante regravação da célula, pois o cartório pode pular por impressão igual, que não codifica o valor exato dos minutos. `app/ponto/signals.py:143-147,275-279`; `app/ponto/services/cartorio.py:452-456,498-508,1348-1364`.

**Sobre um `atomic` por linha:** ele não inverte, por si, a sequência interna acima nem acumula as 30 ausências numa transação. Entretanto, amplia a duração das travas até o commit daquela linha e adia os consumidores; portanto não equivale a dizer que todo `save()` em autocommit já possui esse mesmo escopo transacional. Essa é uma inferência da posição dos saves, atomics e callbacks. `app/ponto/models.py:908`; `app/ponto/portas/celula.py:681-684`; `app/ponto/signals.py:272`; `app/chamados/models.py:673-674`. Ausência de deadlock em concorrência: **[não verificado]**.

**3. Autoridade de competência entregue/exportada**

A autoridade é **`folha.export.competencia_entregue(empresa_id, mes, ano)`**: existe `ExportacaoDominio` vigente e esse vigente não está invalidado. Não basta existir qualquer exportação histórica. `app/folha/export.py:556-586`.

Para `(colaborador, data)`, já existe a composição:

```python
competencia_do_dia_entregue(data, colaborador.empresa)
```

Ela usa `janela_atual(data, empresa)` e consulta a competência do **mês/ano do fim da janela**, não o mês civil da data; sem data retorna `None`. `app/chamados/services/vigia_fio.py:822-844`.

As distinções são:

| Pergunta | Autoridade |
|---|---|
| Foi entregue? | `competencia_entregue`, sobre exportação vigente válida. `app/folha/export.py:580-586`. |
| Está aprovada/exportada para a guarda de batida? | `competencia_fechada`, sobre `FechamentoMensal.status`. `app/ponto/janelas.py:75-89`; `app/ponto/registro_batida.py:133-135`. |
| Está fechada para regenerar célula? | `competencia_fechada_para_celula`: entregue **ou** holerite do colaborador em lote publicado. `app/folha/export.py:589-616`. |
| O export pode passar pela conferência atual? | `folha/porta_export.py::conferir`, que verifica medição/carimbo/falhas; não responde se já foi entregue. `app/folha/porta_export.py:800-821`. |

Outros escritores já usam esse juiz: `competencias_a_lavrar` consulta `competencia_entregue`; o fechamento o consulta via `marcos_da_competencia`; a regeneração de células usa a composição mais restritiva própria da célula. `app/ponto/services/he_pendente_lavrado.py:209-225`; `app/ponto/services/fechamento.py:1513-1524`; `app/ponto/management/commands/gerar_celulas.py:126-174`.

Criar/editar ausência hoje não barra competência fechada; portanto o recorte “somente competência aberta” precisaria estar explícito no comando. `app/ponto/services/ausencia.py:226-233,485`. A abertura efetiva da competência informada: **[não verificado]**.

**4. Leitores e efeito da troca de 720 pelo previsto**

| Leitor | Regra e consequência |
|---|---|
| **Fechamento: faltas** | Ausências aprovadas dos tipos que descontam têm minutos rateados por todos os dias do registro; o fechamento soma e divide por 60. **Muda horas de falta** quando o total muda; num dia, 720→420 significa 12h→7h. `app/folha/export.py:87-92,113-122,138-143`; `app/ponto/services/fechamento.py:864-866,987`. |
| **Fechamento: pagas** | Usa `tipo in TIPOS_PAGOS and minutos`, sem teto de 480: soma crédito e pula a aplicação do abono integral por cobertura. Positivo→positivo mantém esse ramo; zero muda o ramo. `app/ponto/services/fechamento.py:665-669,684-692`. |
| **`efeitos_da_ausencia`** | Mesmo predicado pago+minutos truthy: soma `min_parcial_pagos` e não justifica/suprime integralmente por esse ramo. **Muda o crédito bruto**; positivo→positivo mantém o `continue`. `app/ponto/services/efeito_ausencia.py:43-61`. |
| **`credito_parcial`** | Crédito inteiro na data inicial; abatimento `min(atraso_do_dia, credito_do_dia)`, sem transportar sobra. **Muda crédito**, mas o atraso líquido só muda quando o novo crédito deixa de cobri-lo. `app/ponto/services/credito_parcial.py:23-25,47-58`; `app/ponto/services/fechamento.py:982-985`. |
| **`turnos.cobre_parte_do_dia`** | Exige tipo parcial vivo, registro de um dia e **`0 < minutos < 480`**. 720→420 pode virar parcial; 720→480/660 continua inteiro; 720→0 também continua inteiro; range maior que um dia não vira parcial. `app/ponto/turnos.py:2580-2600`. |
| **Dia inteiro/pontualidade** | Exclui falta e exige `not cobre_parte_do_dia`; o mapa de dias abonados inteiros alimenta o motor. **Pode mudar** ao cruzar 480, alterando a cobertura da pontualidade. `app/ponto/turnos.py:2668-2690`; `app/ponto/services/fechamento.py:680-683,742`. |
| **Grade/cobrança** | Inteira dispensa o dia; parcial preserva a cobrança restante e dispensa marcos conforme o período. **Pode reativar cobrança** ao passar para 1..479; sem horas declaradas, resulta `sem_periodo`. `app/escala/utils.py:1195-1212,1251-1274`; `app/ponto/turnos.py:2632-2641`; `app/escala/servico_jornada.py:340-345`. |
| **Abono materializado** | `por_dia_da_grade` usa previsto e realizado da grade, não diretamente `Ausencia.minutos`. **Não substitui diretamente o abono pelo novo campo**; efeitos indiretos dependem da classificação. `app/ponto/services/dia_pago.py:384-400`. |
| **TXT, falta 8792** | Expande datas e emite `len(datas)`, mais registro 11 por data. **Não muda diretamente** mantendo tipo/status/datas: conta dias, não as horas gravadas. `app/folha/export.py:157-185,215-221,732-736`. |
| **TXT, atraso 8069** | Usa atraso/saída do fechamento. **Pode mudar indiretamente** após apuração, pelo crédito ou pela cobertura. `app/folha/export.py:449-452`; `app/ponto/services/fechamento.py:982-986`. |
| **Datas abonadas/8932** | A contagem crua expande datas sem ler minutos; **não muda só pela duração**. `app/folha/export.py:193-206`. |
| **Espelho, rótulos** | Rótulo aprovado de desconto/abono apresenta horas derivadas de minutos; **muda o texto numérico**. Zero elimina a duração no rótulo. `app/ponto/services/dia_decidido.py:148-155,217-235,484-489`. |
| **Espelho, totais** | Tela/PDF recebem resumo do fechamento/lavratura; **dependem da reapuração**, não apenas do novo valor na ausência. Contagem de dias de falta continua pelas datas. `app/ponto/services/espelho.py:1283-1284`; `app/relatorios/pdf_espelho.py:628`; `app/relatorios/cartao_pela_celula.py:320-346`. |
| **API** | GET serializa `a.minutos` diretamente: **muda o JSON**. `app/api/views.py:1869-1880`. |
| **Lista web** | Mostra dias primeiro, depois minutos se `cobre_parte`, senão “1 dia”/fim aberto. **Pode mudar de “1 dia” para minutos** ao cruzar o juiz de 480. `app/ponto/views.py:2233-2243`; `app/templates/ponto/ausencias.html:159-165`. |
| **Drawer/formulário/admin** | Exibem/preenchem minutos; propriedade `horas` divide por 60. **Mudam os números apresentados**. `app/templates/ponto/partials/painel_ausencia.html:125,149,296-313`; `app/ponto/admin.py:44`; `app/ponto/models.py:864-866`. |

**Há dois conceitos distintos de parcial:** crédito/efeito usam pago+minutos truthy; cobertura usa tipo parcial+um dia+`0<minutos<480`. Não se pode interpretar “previsto correto gravado” como “mesmo efeito funcional garantido”. `app/ponto/services/credito_parcial.py:24-25`; `app/ponto/services/efeito_ausencia.py:44-52`; `app/ponto/turnos.py:2598-2600`.

A antiga comparação de apresentação `>=720` foi retirada; a view atual delega ao juiz de cobertura. `app/ponto/views.py:2233-2240`.

**5. Moldes de backfill, snapshot e reversão**

- **Molde mais próximo:** `curar_ausencia_sem_fim` identifica anomalias, oferece dry-run, passa cada linha pelo serviço e registra antes/depois. Não corrige positivo porque `rederivar_fim` só chama o save existente. `app/ponto/management/commands/curar_ausencia_sem_fim.py:29-53`; `app/ponto/services/ausencia.py:1277-1282`.
- **Backfill histórico de Ausencia:** migration 0057 muda estados e registra uma trilha por linha; possui reversa, mas ela volta estados para `pendente`, não restaura minutos nem uma fotografia exata. `app/ponto/migrations/0057_a4_backfill_pendente.py:35-65`.
- **Backfill de célula:** `backfill_b5_insumos` usa o juiz e carimba insumos na célula; não altera `Ausencia.minutos`. `app/ponto/management/commands/backfill_b5_insumos.py:51-84`.
- **Snapshot/restore reutilizável como formato:** scripts existentes fotografam/restauram `FechamentoMensal` e `DiaPago`, não Ausencia. `bin/snapshot_relavratura_10_2026.py:20-64`; `bin/restore_relavratura_10_2026.py:69-86`.
- **Trilha não equivale a snapshot garantido:** `evento` é best-effort e pode retornar `None`; inferência: não deve ser tratado sozinho como garantia de reversão completa. `app/core/observ.py:94-105,124-139`.

Não foi localizado um comando que já substitua esses 720 positivos pelo previsto, nem snapshot/restore específico de Ausencia; inexistência absoluta: **[não verificado]**.

**6. Contraexemplos e resultados zero/None**

| Caso | Consequência |
|---|---|
| **Salvar mesmo valor novamente** | Save completo continua chegando ao banco, move `atualizado_em` e dispara os receivers. A segunda execução precisa de comparação explícita para ser sem escrita; inferência desses trechos. `app/ponto/models.py:813-814,908`; `app/ponto/signals.py:143-147`. |
| **Previsto legítimo de 720** | O registro continua no filtro `minutos=720`; esse filtro sozinho não identifica pendência. Comparar atual e previsto evita regravação; inferência do cálculo. `app/escala/utils.py:2293-2341`. |
| **Insumos alterados entre execuções** | Célula, DNA, vínculo, template e folgas podem produzir outro previsto na segunda execução. `app/escala/utils.py:2280-2290`; `app/escala/models.py:1094-1096,1124-1133,1280-1288`. |
| **Zero atribuído a falta aprovada** | O save chama novamente o helper; não preserva necessariamente o zero calculado antes, se os insumos mudarem nesse intervalo. `app/ponto/models.py:877-887`. |
| **Range legado incoerente** | Calcula minutos antes de derivar `data_fim` por `dias_corridos`. Primeira passada pode calcular um dia e criar o range; uma nova derivação pode então somar vários dias. `app/ponto/models.py:882-887,893-907`. |
| **Sem escala/tipo/marcos completos** | Helper devolve **0** quando não encontra vínculo/tipo ou falta entrada/saída. `app/escala/utils.py:2277-2292`. |
| **Folga ou dia indefinido** | `eh_dia_trabalho(...) is not True` devolve **0**, inclusive quando a resposta é `None`; célula existente manda por `trabalha`. `app/escala/utils.py:2287-2288`; `app/escala/models.py:1087-1096`. |
| **Feriado** | Não é zero universal: vira folga conforme ciclo autorizado, configuração e feriado no lugar do vínculo. Célula existente continua tendo precedência. `app/escala/models.py:1094-1096,1367-1395`. |
| **12x36** | Sem foto do mês, âncora individual válida decide pela paridade; folga retorna zero. Template 12x36 sozinho pode responder `None`, que o helper converte em zero. Foto mensal pode substituir a fase da âncora. `app/escala/models.py:824-829,1272-1292,1309-1317,1351-1364`; `app/escala/utils.py:2287-2288`. |
| **Turno noturno/pausas** | Calcula span com virada de meia-noite e desconta pausas; não usa simplesmente 720 nem `minutos_jornada`. Entrada igual à saída produz 1440 antes das pausas, pela aritmética existente. `app/escala/utils.py:2293-2295,2312,2326-2341`. |

**O helper normalmente retorna inteiro, inclusive zero; não retorna `None` nos caminhos normais examinados.** O zero mistura “folga” com “não há informação suficiente de escala/marcos”, portanto não prova sozinho que o dia deveria ter duração zero. `app/escala/utils.py:2277-2292,2341`.

_2031 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
