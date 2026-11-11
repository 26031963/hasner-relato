# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 10:54** · publicado em **11/11/2026 10:53** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 10:54 · sessao 1623f482cb44 · - · - · origem=Ronald (tmux)

Fluxo após diff, com linhas **novas** de `bin/relato.sh`:

- `MAPA_RC=0; MAPA_PAR=""` em **94**; `cp` e validador em **96**; sucesso escreve `MAPA_PAR` em **97**; falha de qualquer um escreve `MAPA_RC=1` em **99**. Nenhuma outra atribuição a `MAPA_RC` no diff. Portanto validador executado vermelho não termina com rc 0; verde termina com rc 0 salvo outra falha. (`diff bin/relato.sh:94–99,155,161–162`.)
- Todos os `exit`: base **27,28,32,35,38,42,86**, todos `1`; novos **126** (`gerar_avais`, `1`), **141** (raspagem geral, `1`), **155** (`"$MAPA_RC"`: `0` ou `1`), **161** (falha do push/echo: `1`), **162** (`"$MAPA_RC"`: `0` ou `1`). A inserção de 24 linhas deixa as primeiras sete posições intactas. (`bin/relato.sh` base citada; `diff bin/relato.sh` linhas novas citadas.)
- Pode remover público **sem executar o validador**: fonte ausente/ilegível faz `cp` falhar, o `&&` pula Python; re-raspagem da cópia existente falha, `rm` apaga `MAPA-20.md`, `MAPA_RC` permanece `1`; etapa 4 inclui remoção rastreada. Isso não é caminho com **validador verde**. (`diff bin/relato.sh:96,99,102–109,148–152,159–162`.)
- Não encontrei caminho interno com validador verde que entre nesse `rm`: está exclusivamente no `else` do `cp && validar`. A raspagem geral ainda pode falhar por motivos próprios e sai `1`. (`diff bin/relato.sh:96–112,127–141`.)

QUEM ESCREVE / QUEM LÊ:

| Valor | Escritor | Leitor |
|---|---|---|
| `MAPA_RC` | shell: inicializa `0`, falha fixa `1` | saídas de “nada mudou” e final |
| `MAPA_PAR` | shell: vazio / par da foto | argumento opcional da raspagem geral |
| `MAPA_PATH` | shell: vazio / nome após `git add` bem sucedido | mensagem e pathspec do commit | [nao verificado]
| `$TMP/mapa20.md` | `cp` da fonte | validador; raspador pela lista de pares |
| `logs/MAPA-20/mapa.md` | sessão, **apenas declarado em comentário**, produtor real não fornecido | `cp` |
| `$DEST/MAPA-20.md` | `raspar_publico.py:40`, ou troca do arquivo re-raspado com `mv`; remoção com `rm` | raspador no ramo retido, teste `-f`, Git | [nao verificado]

Refs: `diff bin/relato.sh:89–112,141,148–162`; `bin/raspar_publico.py:23–40`. Escritor efetivo/eventos da fonte: **[nao verificado]**. O diff não cria gerador nem gatilho para regravar a fonte em pouso/reprovação/lei: o comentário delega à sessão. (`diff bin/relato.sh:89–90`.)

Classes:

- **Segundo juiz:** raspagem reaproveita o juiz único de privacidade: `raspar_publico.py:15–16,31,36`, `nome_no_texto.py:59,87`; não introduz uma segunda regex de nomes.
- **Segundo escritor:** duas rotas de atualização do público estão no mesmo publicador sob a trava já existente: `bin/relato.sh` base **27**, diff **96–112,141**; produtor da fonte fora desse bloco não fornecido.
- **Relógio:** nenhum relógio novo de decisão no diff; `date` continua só na mensagem do commit (`base bin/relato.sh:130`, diff **159**). Timer já existente em `bin/hasner-relato.timer:5–7`; chama script por `bin/hasner-relato.service:15`.
- **Fonte única / derivação:** `MAPA_PAR` faz publicador consumir a foto efetivamente validada (`diff bin/relato.sh:96–97,141`); censo/fonte/medidas de negócio não são computados pelo diff **[nao verificado]**.
- **Trava:** nenhuma trava nova; `codex_estado.sh --gerar` sai antes da aquisição de lock do ramo evento, logo não autoespera na trava que `relato.sh` já segura. (`bin/relato.sh:27`; `bin/codex_estado.sh:54–56,65–66`.)
- **Pouso/pré-requisito:** contrato MAPA-20 é apenas referenciado pelo novo comentário; não consta no texto físico desta base. Comprovação do contrato, fonte pronta, mapa publicado, selos executados e opus: **[nao verificado]**. A regra de contrato está em `CLAUDE.md:394`, suíte/opus em **377–381**. [nao verificado]
- **Campo com dois significados:** `MAPA_RC` é resultado acumulado da etapa MAPA (cópia **ou** validação), não rc puro do Python; `MAPA_PATH` identifica artefato rastreado inclusive para remoção, não prova de publicação nova. Isso está explícito no uso (`diff bin/relato.sh:94–99,148–152`). Confundir os nomes com “validador verde”/“mapa publicado” produziria conclusões erradas.

Limite preexistente útil: `set -uo pipefail` sem `-e`, `cd`, `git add` geral e `git commit` sem checagem continuam na base (`bin/relato.sh:21,120–132`), agora linhas novas **144–161**. Não são cura completa pelo novo `push || exit 1`; falha de commit seguida de push bem sucedido pode ainda terminar `0` quando mapa verde, embora exista outra falha.

## 11/11 10:52 · sessao 1239d5bfa276 · - · - · origem=Ronald (tmux)

Nenhum dos cinco selos vizinhos tem quebra estática causada pelo diff; execução completa dos selos ficou [nao verificado], pois eles criam arquivos e/ou acessam recursos externos. A busca equivalente a `git grep 'relato.sh' bin/tests` encontra referências nos cinco arquivos abaixo. (`bin/tests/test_codex_estado.sh:100`; `bin/tests/test_codex_estado_evento.sh:28`; `bin/tests/test_relato_publica_claude_md.sh:8`; `bin/tests/test_relato_afirma_com_prova.sh:44`; `bin/tests/test_publicacao_sem_nome.sh:6`.)

1. **`test_codex_estado.sh`: não quebra.** O recorte começa na primeira ocorrência literal de `CODEX_OK=""` e termina no marcador do passo 3; só busca `exit` nesse intervalo, excluindo comentários. O bloco 2f novo fica antes do bloco 2e e de `CODEX_OK=""`, portanto seus novos comandos não entram nessa região. (`bin/tests/test_codex_estado.sh:104-111`; base `bin/relato.sh:89-98`; diff `bin/relato.sh:89-122`, linhas novas.)

2. **`test_relato_publica_claude_md.sh`: não quebra.** A raspagem geral mantém o par CLAUDE; o novo raspador de MAPA retido não casa com a regex que exige continuação de linha imediatamente depois de `"$DEST"`. O teste conta apenas linhas começando na coluna 1 por `git add -A `; o novo `git add -A -- MAPA-20.md` tem indentação e não aumenta essa contagem. O pathspec do commit continua contendo CLAUDE. (`bin/tests/test_relato_publica_claude_md.sh:13-20`; diff `bin/relato.sh:104-105`, `141`, `145`, `151`, `160`, linhas novas.)

3. **`test_codex_estado_evento.sh`: não quebra na costura.** Continua sem `gerar_codex_estado.py` em `relato.sh`, mantendo chamadas de `codex_estado.sh` e `relato_nomes.sh`. (`bin/tests/test_codex_estado_evento.sh:27-30`; base `bin/relato.sh:32`, `97`; diff não altera essas chamadas.)

4. **`test_relato_afirma_com_prova.sh`: não quebra na costura.** Seu predicado apenas exige a presença de `relato_afirma_com_prova.py`, preservada pelo diff. (`bin/tests/test_relato_afirma_com_prova.sh:43-45`; base `bin/relato.sh:55`.)

5. **`test_publicacao_sem_nome.sh`: não recorta `relato.sh`.** As referências são comentários; o universo efetivo vem de `git ls-files` do público. MAPA-20 rastreado entra automaticamente nesse universo. Resultado de privacidade em produção permanece [nao verificado]. (`bin/tests/test_publicacao_sem_nome.sh:58-72`.)

## 11/11 10:48 · sessao ff7bc3086497 · UI-PAINEL-MOSTRA-O-APP · construtor · origem=Code

Cura aplicada na cópia: faixa, perguntas em leitura e contador no [template](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/templates/colaboradores/partials/painel_colaborador.html:39); projeção única em [pergunta_no_app.py](/home/ronald/wt-codex-ui-painel-mostra-o-app-100626/app/chamados/services/pergunta_no_app.py:78). `consulta_do_app` preserva seu contrato; os dois `/me/` mantêm a construção do JSON (`api/views.py:316`, `api/views_core.py:304`).

Não fiz commit: prevaleceu a proibição inicial. Encaixe, certificação e smoke permanecem com a sessão/Code. [nao verificado] [nao verificado]

Arquivos alterados:

- `templates/colaboradores/partials/painel_colaborador.html:39`: contador; faixa e lista em `:190`.
- `colaboradores/views.py:940`: painel recebe a projeção.
- `api/views.py:315` e `api/views_core.py:303`: substituição das regras inline; remoção dos imports tornados inúteis.
- `chamados/services/pergunta_no_app.py:78`: projeção ao lado da autoridade existente.
- `chamados/tests/e3_mutantes.py:23`: alvos `badge_disputas`, `kernel_core_disputas` e `vermelho_autor_errado` atualizados após a extração.
- `core/tests/test_tripwire_contagem_de_pendencia.py:83`: dois sítios antigos substituídos pelo único; teto reduzido para 25 em `:147`.
- `colaboradores/tests/test_ui_painel_mostra_o_app.py:1`: selo em memória e registro do RED.
- `colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:54`: selo painel × duas APIs.
- `colaboradores/tests/ui_painel_fontes.py:19`: quatro mutantes, exclusivamente em memória.

O RED anterior à cura registrou **cinco falhas por assertion**, sem erros. Depois, passaram 13 testes em memória; os quatro mutantes novos morreram por assertion. Tripwire e paridade passaram em 19 testes com adaptação em memória de `SimpleTestCase`. `py_compile` compilou os nove Python com escrita de bytecode interceptada; `git diff --check` passou. O registro inicial está em `colaboradores/tests/test_ui_painel_mostra_o_app.py:7`; resultados posteriores não foram gravados em arquivo. [nao verificado]

**ORM, RED visual e smoke não foram executados; faltam Django e Ruff neste ambiente.** O selo ORM cobre os três casos, a precedência com mensagem simultânea, identidade por campos e rótulo variável; sua execução continua pendente (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:135`). As duas cascas têm prova pelo grafo completo até o carregamento do painel (`colaboradores/tests/test_ui_painel_mostra_o_app.py:83`).

Quem escreve e quem lê cada campo usado?

| Campo | Escritores | Leitores |
|---|---|---|
| `pendentes` | Lista derivada por `consulta_do_app`, usando `respondida_em` e `fechada_em` (`chamados/services/pergunta_no_app.py:69`). Respostas/reaberturas: `chamados/services/disputa_emissao.py:1468`, `:1593`, `:1653`, `:1674`; `chamados/models.py:1566`, `:3132`. Fechamento/reabertura da disputa: `chamados/models.py:1932`, `:2149`, `:2173`. | Projeção `chamados/services/pergunta_no_app.py:89`; listagens `api/views.py:993`, `:2531`; dashboard `core/views.py:253`; aviso `chamados/services/aviso_pendencia.py:9`; cobrança `chamados/services/cobranca.py:48`. |
| `lido_colaborador_em` | Nasce nulo (`chamados/models.py:1673`, `chamados/services/nota.py:88`); `marcar_lidas` grava o carimbo (`chamados/services/nota.py:126`). | Projeção `chamados/services/pergunta_no_app.py:95`; serializador `api/views.py:1116`; web `chamados/views.py:603`; cartão `templates/chamados/partials/_card_atendimento.html:51`. |
| `status_local` | Nascimento pela porta `criar` (`chamados/models.py:1095`); transições persistentes por `_gravar_estado` (`chamados/models.py:201`, `:296`). | A projeção lê `VIVOS` (`chamados/services/pergunta_no_app.py:93`), cujo conjunto vem de `chamados/catalogo/motor.py:24`. |

Há outro leitor da regra de rótulo? **Sim:** `templates/core/dashboard_colaborador_v2.html:80`, alimentado por `core/views.py:253`. Permaneceu fora do diff e deve entrar na fila. Outros resultados do grep são textos de navegação/relatórios e expectativas/mutantes de testes (`templates/chamados/form_abertura.html:7`, `relatorios/views.py:1672`, `inteligencia/views.py:255`, `chamados/tests/test_e3_app_orm.py:44`, `colaboradores/tests/ui_painel_fontes.py:26`). Não houve ocorrência em JS. [nao verificado]

O que fazem os antigos leitores `api/views.py:1004/:2542`? Agora em `:993/:2531`, continuam usando `consulta_do_app`: listam perguntas/histórico e publicam perguntas da disputa; não calculam badge. Não foram alterados. O GET de chamados marca notas como lidas em `api/views.py:1038`; o selo usa exclusivamente `/me/` (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:93`).

Comandos para a sessão, no banco de ensaio; esperado **PASS**, ainda [nao verificado]:

```bash
export PYTHONDONTWRITEBYTECODE=1
export DJANGO_SETTINGS_MODULE=config.settings.ci

python3 manage.py test \
  colaboradores.tests.test_ui_painel_mostra_o_app \
  colaboradores.tests.test_ui_painel_mostra_o_app_orm \
  chamados.tests.test_e3_app_orm \
  chamados.tests.test_e3_v4_orm \
  colaboradores.tests.test_retrato_no_drawer \
  api.tests.test_contract_paridade_kernels \
  core.tests.test_tripwire_contagem_de_pendencia
```

Cada comando abaixo deve terminar com **FAIL por assertion**, sem `ERROR`; instalação dos mutantes em `colaboradores/tests/ui_painel_fontes.py:56`. Resultado ORM esperado [nao verificado]:

```bash
morder() {
  UI_PAINEL_MUTANTE="$1" python3 manage.py test \
    colaboradores.tests.test_ui_painel_mostra_o_app_orm
}
morder painel_query
morder template_rotulo
morder api_ui_precedencia
morder api_core_precedencia

python3 -m chamados.tests.e3_mutantes --mutante badge_disputas
python3 -m chamados.tests.e3_mutantes --mutante kernel_core_disputas
python3 -m chamados.tests.e3_mutantes --mutante vermelho_autor_errado
```

Os três alvos E3 ajustados também têm mordida por assertion em memória (`colaboradores/tests/test_ui_painel_mostra_o_app.py:154`). Reprodução local e Ruff pendente:

```bash
python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app \
  chamados.tests.test_e3_pendentes_app -v

UI_PAINEL_MUTANTE=painel_query python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app
UI_PAINEL_MUTANTE=template_rotulo python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app
UI_PAINEL_MUTANTE=api_ui_precedencia python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app
UI_PAINEL_MUTANTE=api_core_precedencia python3 -m unittest colaboradores.tests.test_ui_painel_mostra_o_app

ruff check --no-cache --config ruff.toml \
  api/views.py api/views_core.py colaboradores/views.py \
  chamados/services/pergunta_no_app.py chamados/tests/e3_mutantes.py \
  core/tests/test_tripwire_contagem_de_pendencia.py \
  colaboradores/tests/*ui_painel*.py
```

LEI-AKITA: origem=chamados.services.pergunta_no_app.projecao_do_app, testemunha=consulta_do_app, RED=colaboradores.tests.test_ui_painel_mostra_o_app_orm.PainelMostraOAppORMTest, quem-mais-le=api/views.py:993/2531+core/views.py:253+dashboard_colaborador_v2.html:80+aviso_pendencia.py:9+cobranca.py:48, juizes novos=0, padrao=projecao

## 11/11 10:48 · sessao 33adf0aa3653 · MAPA-ATE-O-20 · censo · origem=Code

MAPA-ATE-O-20 — cópia informada `9ca563bb3e0470eae1586b080303b2b026c85455`; identidade Git da cópia: [nao verificado].

Referências: **B:n** = `backlog_obras.txt:n`; **A:n** = `arquivos_obras.txt:n`, contando desde o primeiro cabeçalho do anexo; **L:n** = linha **n do RELATO**, transcrita em `leis_abertas.txt`; **R:n** = `app/docs/RELATO.md:n`. Os identificadores entre parênteses na primeira coluna são abreviações usadas nos cruzamentos. As referências de cada linha sustentam suas células; cruzamento significa arquivo compartilhado, sem presumir alteração no mesmo trecho. (`arquivos_obras.txt:1`; `backlog_obras.txt:2367`.)

Prefixos: **C/** = `app/chamados/`; **S/** = `app/chamados/services/`; **Q/** = `app/chamados/management/commands/`; **P/** = `app/ponto/services/`; **QP/** = `app/ponto/management/commands/`; **H/** = `app/templates/chamados/partials/`. Arquivos Python abreviados conservam `.py`. Essa convenção apenas encurta os caminhos citados nos censos. (`app/chamados/models.py:1853`; `app/chamados/services/materializacao.py:425`.)

**K34** é o perímetro compartilhado reaproveitado de F3/F4: `C/{models,juizes,reconciliador,signals,relatorios_disputa,views_cobrar}.py`; `S/{tarefa_do_dia,materializacao,validacao,disputa_emissao,fio,painel,verbos,explicador,declaracao_texto,becos,acoes_disputa,raiox}.py`; `app/api/views.py`; `P/cartorio.py`; `app/ponto/supra_juiz.py`; `Q/{auditar_invariantes_chamados,migrar_data_evento_dia_do_turno}.py`; `QP/reconciliar_grade.py`; `H/{modal_fio,_card_atendimento}.html`. São sítios do censo comum, **sem divisão comprovada de alterações entre F3 e F4**. (`app/chamados/models.py:1853`; `app/chamados/services/materializacao.py:425`; `app/chamados/services/validacao.py:461`; `app/chamados/services/disputa_emissao.py:1508`; `app/chamados/reconciliador.py:411`; `f3f4_censo.r` [nao verificado: numeração do anexo].)

**K4** reaproveita o censo E4C: inclui os arquivos de K34 também inventariados ali, mais `app/api/views_core.py`, `S/vigia_fio.py`, `app/colaboradores/services/calendario.py`, `P/dia_do_colab.py`, `C/templatetags/chamados_tags.py`, `app/core/views_ia.py`, `H/_lote_preview.html`, `app/templates/core/_copiloto.html` e `app/templates/chamados/{detalhe,detalhe_local,fio_colaborador}.html`. (`app/chamados/services/vigia_fio.py:675`; `app/colaboradores/services/calendario.py:474`; `app/ponto/services/dia_do_colab.py:70`; `app/api/views_core.py:313`; `app/chamados/templatetags/chamados_tags.py:128`; `censo.r` [nao verificado: numeração do anexo].)

**†** identifica obra sem censo integral entregue nestes anexos; não afirma inexistência de censo fora deles. `nao medido` não significa zero. VISIVEL descreve o resultado pretendido documentado, sem afirmar que já foi entregue. [nao verificado: cobertura integral fora dos anexos.]

| obra | arquivos que toca | cruza com | leis em aberto | depende de | move fio_morto_alcance | move a Central | VISIVEL | voltas ja gastas | etapa |
|---|---|---|---|---|---|---|---|---|---|
| **E3†** — alias de FIO-E0-V4-E3-LEITORES (B:1866) | `C/juizes.py`; `S/{carga_pergunta_viva,materializacao,tarefa_do_dia}.py`; `app/escala/servico_jornada.py`; `app/folha/export.py`; `P/cartorio.py`; `app/ponto/turnos.py` (A:2; A:3; A:4; A:5; A:18; A:19; A:20; A:25) | E4B, E4C, RAIZ F3/F4, O168, LASTRO, CAL-LE, LP2, LP2F, E2, A17, A18, J760, D5 | L:4, corte do pouso; L:554, unidade do app, trecho incompleto | Corte para os 35 dias com célula trabalha=True de desligados; sem ele não pousa (L:4) | **nao medido**. Impacto de frota: cura muda 206 dias contra 177 da base; 117 exclusivos em 12 colabs fora de X1, incluindo os 35 barrados (L:4; dado da sessão [nao verificado por arquivo:linha para 206/177]) | nao medido | Fila e verbos passam a consumir tarefa_do_dia (B:1866) | volta 7 (B:1866); X1 volta 3 (R:10) | **PAREI** (L:4) |
| **E4B†** — alias de FIO-E0-V4-E4B (B:1887) | `C/juizes.py`; `Q/backfill_fio_morto_e4b.py`; `S/{disputa_emissao,foto_e4b}.py`; `app/config/crons.py`; `P/cartorio.py` (A:27; A:28; A:29; A:30; A:47; A:58) | E3, E4C, RAIZ F1–F4, O168, LASTRO, LP2, LP2F, LP3, PRESC, F0F1, E4A2, BOD, BOM, J760, D5 | nenhuma; sem_porta foi respondida (R:82) | E3 antes do apply; E4a no ar antes do backfill; exportadas exigem o corte próprio (R:99; B:1878; B:1887) | nao medido | nao medido | O passivo volta às portas canônicas; credor com pergunta viva recupera alcance (B:1887; R:82) | 5 voltas Codex; c4 em preparo no registro (R:49; R:30) | construtor (A:26; R:30) |
| **E4C** — sem linha própria; fase de B:1878 | K4; migração de leitores, preservando exceções (`app/chamados/models.py:3034`; `app/chamados/services/tarefa_do_dia.py:398`) | E3, E4B, RAIZ F1–F4, O168, LASTRO, ROTULO, LOTE, CAL-LE, LP2, LP2F, LP3, JOB, E1, E2, ANCORA, F0F1, RET, A03/A05/A06/A17/A18/A20, BOD, BOM, F4C, J760, D5, RESTO, O137; também E4CAL-P3-ULTIMO-RECURSO-ENGOLE, E4CAL-P3-CENSO-HOJE-PORTA-CURTA e E4CAL-L1-P3 | **L:28 — D04 e D05** | E4B medida antes de trocar a fonte; respostas D04/D05 para os respectivos contratos; coordenação das testemunhas com F3/F4 (`app/docs/PROMPTS.md:2034`; `app/chamados/services/tarefa_do_dia.py:398`; L:28) | nao medido | nao medido | Fases, respostas, atividade e filas passam a ler decisões e efeitos; apresentação depende de D04/D05 (L:28) | nao medido | censo |
| **E4D†** — sem linha própria; fase de B:1878 | nao medido | nao medido | nenhuma própria | E4C retirar leitores migráveis antes de parar projeção; seis exceções permanecem (`app/docs/PROMPTS.md:2030`; `app/docs/PROMPTS.md:2034`) | nao medido | nao medido | nada na tela, como mudança própria documentada (B:1878) | nao medido | fila |
| **RAIZ F1†** — em F0F1, sem linha individual (B:1875) | `C/models.py:2982/:3014`; `S/fio.py:925`; `S/disputa_emissao.py:238/:1197`; `S/declaracao_texto.py:377` (R:19) | E4B, E4C, RAIZ F2–F4, LASTRO, LP3, E1, ANCORA, F0F1, RET, A03/A05/A06, BOD, BOM, F4C, PLANTIO | L:186, h2; eleição do destino consta RESPONDIDA em B:1875 | F0; entrada do c22 depois de E3 e E4B c4 (B:1875) | nao medido | nao medido | Destino da pergunta passa a ter uma eleição comum; mudança visual específica nao medido (R:19; B:1875) | nao medido | censo |
| **RAIZ F2†** — alias da BO-DISPUTA (B:1895) | `Q/auditar_invariantes_chamados.py`; `S/{declaracao_texto,descarte_disputa,disputa_emissao}.py`; `app/core/observ.py`; `QP/reconciliar_grade.py` (A:68; A:69; A:70; A:71; A:94; A:98) | E4B, E4C, RAIZ F1/F3/F4, LASTRO, LP3, F0F1, BOD, BOM | L:186, conflito h2 | F1 e c22; nascimento e destino pela raiz (B:1875; B:1895) | nao medido | nao medido | Evita disputa nascer sem pergunta; legado não é removido por shell (B:1889) | c21 existente; c22 ainda previsto (B:1875) | fila |
| **RAIZ F3** — sem linha própria (B:1895) | K34, perímetro comum do censo; divisão F3/F4 nao medido (`app/chamados/models.py:1853`; `app/chamados/services/materializacao.py:425`) | E3, E4B, E4C, RAIZ F1/F2/F4, O168, LASTRO, ROTULO, LOTE, CAL-LE, LP2, LP2F, LP3, JOB, E1, E2, ANCORA, F0F1, RET, A03/A05/A06/A17/A18/A20, BOD, BOM, F4C, J760, D5, PLANTIO, RESTO, O137 | nenhuma impeditiva; D8 não trava F3/F4 (L:31) | F2; aplicação e reação da raiz coordenadas; autor do fecho segue o último carimbo (B:1895; B:1896) | nao medido | nao medido | Fechamento e explicação passam a respeitar C7 e autoria dos efeitos (`app/chamados/models.py:1853`; `app/chamados/services/explicador.py:137`) | nao medido | censo |
| **RAIZ F4** — sem linha própria (B:1895) | K34; identidade do par em `C/models.py:3441`, fonte PARES_PAUSA_DNA (B:1896) | E3, E4B, E4C, RAIZ F1/F2/F3, O168, LASTRO, ROTULO, LOTE, CAL-LE, LP2, LP2F, LP3, JOB, E1, E2, ANCORA, F0F1, RET, A03/A05/A06/A17/A18/A20, BOD, BOM, F4C, J760, D5, PLANTIO, RESTO, O137 | nenhuma impeditiva; D8 não trava F3/F4 (L:31) | F3; par lido dos marcos da célula, sem novo juiz (B:1895; B:1896) | nao medido | nao medido | As duas metades da pausa são tratadas como par; metade aguarda a outra (`app/chamados/services/validacao.py:461`; `app/chamados/services/materializacao.py:1375`; censo F3/F4) | nao medido | censo |
| **RAIZ F5†** — sem linha própria (B:1895) | nao medido | nao medido | nenhuma própria | F4; coordenada com E4D (B:1895) | nao medido | nao medido | nao medido | nao medido | fila |
| **RAIZ F6†** — sem linha própria (B:1895) | nao medido | nao medido | nenhuma identificada | F5, pela sequência registrada (B:1895) | nao medido | nao medido | nao medido | nao medido | fila |
| **RAIZ F7†** — sem linha própria (B:1895) | nao medido | nao medido | nenhuma identificada | F6, pela sequência registrada (B:1895) | nao medido | nao medido | nao medido | nao medido | fila |
| **O168†** (B:1396) | `app/ponto/supra_juiz.py:539`; `P/cartorio.py:1091` | E3, E4B, E4C, RAIZ F3/F4, LASTRO, LP2, LP2F, D5 | nenhuma: guarda já é lei escrita (B:1396) | Guarda valer no evento antes de retirar --executar; CSF_RETRATADO medido em zero (B:1396) | nao medido | nao medido | Cobrança com conversa deixa de ser fechada pelo caminho rápido cego (B:1396) | nao medido | fila |
| **LASTRO†** — agregado nomeado na META; sem linha própria (B:2367) | Sítios dos filhos: `P/lastro.py:274/:340`; `P/cartorio.py:1374`; `P/dia_do_colab.py:38`; `P/diagnostico_escala.py:155`; `S/disputa_emissao.py`; `C/signals.py:158` (B:1732; B:1762; B:1763) | E3, E4B, E4C, RAIZ F1–F4, O168, LD2, LP2, LP2F, LP3, F0F1, BOD, BOM, D5 | nenhuma identificada | JUIZ-DO-PAR para degrau 2; JUIZ-1 e CRONS-INSTALL-SOB-MARCA para parte 2 (B:1732; B:1761) | nao medido | nao medido | Chamado fecha por lastro do marco e recebe reação por evento; parte 3 já pousada (B:1761; B:1763) | nao medido | fila; parte 3 NO AR (B:1763) |
| **O169-FECHADA†** (B:1940) | `QP/detectar_cluster_espurio.py`; `P/cluster_espurio.py` (A:61; A:62) | O169, L15, VOC | nenhuma de negócio; B3 DESENHO registrado (B:1940) | Volta 5 após E4A2; tratamento do legado pela porta (B:1940) | nao medido | nao medido | Legado não trancado reabre pela porta; trancado relavra a via com trilha (B:1940) | v5 (B:1940; A:60) | PAREI, conforme B3 registrado; liberação nao medido (B:1940) |
| **UI-PAINEL-MOSTRA-O-APP†** (B:1131) | **nenhum no delta da cópia**; planejados `app/api/views_core.py:313` e `app/api/views.py:324` (A:66; B:1131) | nenhum no delta; cruzamento do futuro patch nao medido (A:66) | nenhuma identificada | Mesmo lote de UI-ANEXO-AUSENCIA; projeção reconstruível e contagem única (B:1131) | nao medido | nao medido | Painel passa a mostrar o rótulo e os não lidos pela função comum ao app (B:1131) | nao medido | fila |
| **BO-ROTULO-ENCERRADO (ROTULO)** (B:1899) | `S/verbos.py:114`; `C/templatetags/chamados_tags.py:53`; `app/templates/chamados/{detalhe.html:132,detalhe_local.html:395,fio_colaborador.html:268}`; `H/{modal_fio.html:667,painel_chamado_turno.html:76}` — reuso mapa_3.r | E4C, RAIZ F3/F4, A20, O137 | nenhuma | Pouso O169-FECHADA na raia 2; centralização do vocabulário, pois o modal ainda usa literais (B:1897; `app/templates/chamados/partials/modal_fio.html:667`) | nao medido | nao medido | Seletor distingue encerrar de encerrar e arquivar nos três seletores (`app/templates/chamados/partials/modal_fio.html:668`; `app/templates/chamados/detalhe_local.html:396`) | nao medido | censo |
| **TRIAGEM-OFFLINE-SEM-CAMERA†** (B:1905) | `app/templates/ponto/triagem/apto.html:252`; `app/templates/ponto/bater_ponto_v2.html:287` — cobertura parcial reaproveitada de mapa_1.r/mapa_4.r | RESTO | nenhuma própria identificada | ROTULO na raia 2; juiz comum de admissibilidade e smoke de clique nas duas cascas (B:1897; B:1905) | nao medido | nao medido | Falha de câmera deixa de impedir a entrada da batida offline na fila (B:1905) | nao medido | fila |
| **LOTE-VALIDAR-REGRESSAO-0311 (LOTE)** (B:1911) | `S/validacao.py:695`; `S/acoes_disputa.py:18`; `C/views_cobrar.py:261/:381`; `app/core/views_ia.py:169`; `H/_lote_preview.html:4`; `app/templates/core/_copiloto.html:247` — reuso mapa_2.r | E4C, RAIZ F3/F4, JOB, A20 | nenhuma | TRIAGEM na raia 2; medir causa antes da cura (B:1897; B:1911) | nao medido | nao medido | Mudança visual da cura nao medido; a linha ainda pede investigação (B:1911) | nao medido | censo |
| **E4CAL-P1-REVERTER-AMPLO†** — extra B:852 | nao medido | nao medido | nenhuma identificada | Snapshot por campo antes de permitir reversão ampla (B:852) | nao medido | nao medido | nada na tela | nao medido | fila |
| **E4CAL-P1-PAREI-FM-CORRENTE†** — extra B:853 | nenhum sítio de produção identificado; referência é `logs/e4cal/r8_pouso/comum.py:15` (B:853) | nao medido | nenhuma identificada | Hash apenas de competência exportada (B:853) | nao medido | nao medido | nada na tela | nao medido | fila |
| **E4CAL-P1-SELO-CAMPOS-ACORDAM†** — extra B:854 | nao medido | nao medido | nenhuma identificada | Foto incluir outbox e auditoria da célula (B:854) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:854) | fila |
| **E4CAL-P1-ROTULO-SEM-CERTIDAO†** — extra B:855 | nao medido | nao medido | nenhuma identificada | Medir dono L-099 antes da cura (B:855) | nao medido | nao medido | Rótulo congelado sem certidão é o defeito registrado; resultado da cura nao medido (B:855) | nao medido | fila |
| **E4CAL-P3-ULTIMO-RECURSO-ENGOLE†** — extra B:856 | `app/colaboradores/services/calendario.py:436`, sítio registrado, sem censo próprio anexo (B:856) | E4C, CAL-LE, E2, E4CAL-L1-P3 | nenhuma identificada | Exceção após início da competência não deixar último recurso aberto (B:856) | nao medido | nao medido | nao medido | Codex v2 citado; total nao medido (B:856) | fila |
| **E4CAL-P3-CENSO-HOJE-PORTA-CURTA†** — extra B:857 | `S/vigia_fio.py:289/:400` (B:857) | E4C | nenhuma identificada | Injeção do mesmo relógio alcançar a porta curta (B:857) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:857) | fila |
| **E4CAL-P3-LEITURA-ESCREVE† (CAL-LE)** — extra B:858 | `app/colaboradores/services/calendario.py`; `S/materializacao.py`, sítios da cadeia citada (B:858; `app/colaboradores/services/calendario.py:474`; `app/chamados/services/materializacao.py:279`) | E3, E4C, RAIZ F3/F4, E4CAL-P3-ULTIMO-RECURSO-ENGOLE, E2, A17, A18, E4CAL-L1-P3 | nenhuma identificada | Separar leitura do efeito registrado como best-effort (B:858) | nao medido | nao medido | nao medido | nao medido | fila |
| **E4CAL-L1-POUSO-0411†** — extra B:859 | nao medido | nao medido | nenhuma identificada | Rebase e árvore certificada do pouso, já registrado (B:859) | nao medido | nao medido | nao medido | v2 citada; total nao medido (B:859) | **NO AR** (B:859) | [nao verificado]
| **E4CAL-P3-DOCS-VELHAS†** — extra B:860 | `app/core/juizes.py:717`, comentário; demais arquivos citados são documentação (B:860) | nao medido | nenhuma identificada | Marco da L1 (B:860) | nao medido | nao medido | nada na tela | nao medido | fila |
| **E4CAL-CARD-LE-NUCLEO†** — extra B:862 | `app/templates/colaboradores/partials/_calendario_grade.html:169` (B:862) | nao medido | nenhuma identificada | Retomada da L1; ler rótulos do núcleo (B:862) | nao medido | nao medido | Cards do rodapé recebem o vocabulário do núcleo (B:862) | nao medido | fila |
| **O169†** — extra B:1397 | `QP/detectar_cluster_espurio.py:61`; `P/cluster_espurio.py:55/:95` (B:1397; B:1940) | O169-FECHADA, L15, VOC | nenhuma: morte só por lastro consta decidida (B:1940) | Delta adicional acompanhado em O169-FECHADA (B:1940) | nao medido | nao medido | Chamado de cluster deixa de morrer apenas por idade (B:1940) | nao medido | **NO AR**, C20-O169; delta separado (B:1940) |
| **LASTRO-DEGRAU-2† (LD2)** — extra B:1732 | `P/lastro.py:274` (B:1732) | LASTRO, LP2, LP2F | nenhuma identificada | Pouso JUIZ-DO-PAR; pergunta ao papel do marco, não ao raio (B:1732) | nao medido | nao medido | nao medido | nao medido | fila; proposta anterior reprovada/superada (B:1732) |
| **LASTRO-PARTE-2† (LP2)** — extra B:1761 | `P/{lastro.py:340,cartorio.py:1374,dia_do_colab.py:38,diagnostico_escala.py:155}` (B:1762) | E3, E4B, E4C, RAIZ F3/F4, O168, LASTRO, LD2, LP2F, D5 | nenhuma identificada | JUIZ-1 e CRONS-INSTALL-SOB-MARCA antes do pouso (B:1761) | nao medido | nao medido | Fecho por marco passa a reconciliar; varredor vira alarme (B:1761) | nao medido | fila | [nao verificado]
| **LASTRO-PARTE-2-FILA-OPUS† (LP2F)** — extra B:1762 | Mesmos sítios de LP2; `C/signals.py:158` também citado (B:1762) | E3, E4B, E4C, RAIZ F3/F4, O168, LASTRO, LD2, LP2, LP3, BOD, RAIZ F2 | nenhuma identificada | Tratar achados do opus da parte 2; escopo de cada item ainda requer a medida indicada (B:1762) | nao medido | nao medido | nao medido | opus final citado; total nao medido (B:1762) | fila |
| **LASTRO-PARTE-3† (LP3)** — extra B:1763 | `S/disputa_emissao.py`; `C/signals.py`, reação ao estado da pergunta (B:1763) | E4B, E4C, RAIZ F1–F4, LASTRO, LP2F, F0F1, BOD, BOM | nenhuma identificada | Parte 2 e reação por evento; pouso registrado (B:1763) | nao medido | nao medido | Pergunta que morre aciona a reação do chamado (B:1763) | nao medido | **NO AR** (B:1763) |
| **O169-ATA-VAZIA-FOLGA†** — extra B:1795 | nenhum arquivo de produção identificado para o selo (B:1795) | nao medido | nenhuma identificada | Caso trabalha=False e ata vazia morder a ordem dos insumos (B:1795) | nao medido | nao medido | nada na tela | nao medido | fila |
| **O169-CONTADOR-NAO-MEDIDO†** — extra B:1796 | nao medido | nao medido | nenhuma identificada | Contabilizar fato None e provar vigia/caso (B:1796) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:1796) | fila |
| **O169-LIMIAR-15-DUPLO† (L15)** — extra B:1797 | `app/escala/orfa_classe.py`; `P/cluster_espurio.py`; linhas dos sítios nao medido (B:1797) | O169-FECHADA, O169, VOC | nenhuma identificada | Assinatura por juiz único (B:1797) | nao medido | nao medido | nada na tela | Codex v2 citado; total nao medido (B:1797) | fila |
| **LASTRO-FOLGA-FORA-DO-CATALOGO†** — extra B:1808 | nao medido | nao medido | nenhuma identificada | Medir escritor e frota antes da cura (B:1808) | nao medido | nao medido | nao medido | nao medido | censo |
| **LOTE-VALIDAR-VIRA-JOB (JOB)** — extra B:1832 | Mesmos caminhos de LOTE; resultado atual em `S/validacao.py:726/:762`, `C/views_cobrar.py:383`, `app/core/views_ia.py:173`, `H/_lote_preview.html:4` e `app/templates/core/_copiloto.html:247` — reuso mapa_2.r | E4C, RAIZ F3/F4, LOTE, A20 | nenhuma | Mecanismo existente de job; conservar confirmação, autoria e reclassificação pela porta (`app/chamados/services/validacao.py:698/:735/:746`; B:1832) | nao medido | nao medido | Requisição deixa de esperar o lote; tela lê resultado posterior (B:1832) | nao medido | censo |
| **FIO-E0-V4-E1-ENTRADAS† (E1)** — extra B:1852 | Recorte PLANTIO reaproveitado: `C/models.py:2776`; `S/fio.py:800/:979`; `Q/migrar_data_evento_dia_do_turno.py:179`; restante da obra nao medido | E4C, RAIZ F1/F3/F4, ANCORA, F0F1, RET, A03/A05/A06, F4C, PLANTIO | nenhuma identificada | Portas do fato e censo de todas as portas; apply em exportada exige corte (B:1852) | nao medido | nao medido | nao medido | voltas 5, 8 e 15g citadas; total consolidado nao medido (B:1852) | construtor, conforme registro anexado (B:1852) |
| **FIO-E0-V4-E1-FILA†** — extra B:1853 | nao medido | nao medido | nenhuma identificada | E1; V2-2 permanece em linha própria (B:1853) | nao medido | nao medido | nao medido | opus final volta 5; demais voltas nao medido (B:1853) | **NO AR** (B:1853) |
| **FIO-E0-V4-E2-FUNCAO† (E2)** — extra B:1865 | `S/tarefa_do_dia.py:562`; `app/colaboradores/services/calendario.py:163/:250` (B:1865; L:611) | E3, E4C, RAIZ F3/F4, E4CAL-P3-ULTIMO-RECURSO-ENGOLE, CAL-LE, E4CAL-L1-P3 | nenhuma nova: L:611 é encaixe com decisão já escrita | E1 antes do rebase; cura residual do relógio continua na fila E2 (B:1865; L:611) | nao medido | nao medido | Ação do dia é atribuída ao admin conforme decisão registrada (L:611) | 4 voltas Codex; v6 de suíte (B:1865) | **NO AR**, com residual em fila (B:1865; L:611) |
| **E4B-D2-ANCORA-RAIOX† (ANCORA)** — e

[truncado em 20000 de 39149 caracteres]

## 11/11 10:45 · sessao 599f583cc054 · - · - · origem=Ronald (tmux)

Não encontrei falha concreta de runtime nos três arquivos novos, por inspeção. ORM continua [nao verificado].

- As duas perguntas reutilizam o cenário E3 existente, com marcos `fim`/`int_ida`; a fixture cria identidades distintas. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:137`, `:138`; `chamados/tests/test_e3_app_orm.py:47`; `chamados/services/declaracao_texto.py:42`)
- O painel usa superusuário e passa o RBAC; o request não depende de sessão nos processadores examinados. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:70`, `:89`; `colaboradores/mixins.py:328`)
- Os mutantes Python recompilam a função decorada usando o namespace real do módulo; os testes invocam exatamente o atributo substituído. Não vi caminho para morrer por nome ausente. (`colaboradores/tests/ui_painel_fontes.py:75`, `:78`, `:79`; `colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:90`, `:98`)
- O mutante do template alcança `render_to_string` pelo `loader.get_template`, preservando os includes normais. (`colaboradores/tests/ui_painel_fontes.py:66`, `:68`, `:73`)
- O selo de origem variável exige três chamadas e o mesmo colaborador; mata a regra manual mesmo quando os três casos usuais coincidiriam. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:166`, `:175`, `:177`)
- As cascas são comprovadas pelo grafo, incluindo URL, view, partial e mecanismo de injeção; não há falsa prova por render duplicado do partial. (`colaboradores/tests/test_ui_painel_mostra_o_app.py:83`, `:95`, `:97`, `:101`, `:105`)

Há uma limitação concreta de cobertura: o selo novo de mensagem possui um chamado do colaborador e um do DP; sozinho, esse cenário não distingue o mutante E3 de autor trocado porque ambos contam `1`. A matriz E3 existente tem dois chamados do colaborador e mata esse mutante por diferença `2 × 1`; mantenha seu comando na entrega. (`colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:153`, `:157`; `chamados/tests/test_e3_app_orm.py:67`, `:69`, `:72`; `chamados/tests/e3_mutantes.py:87`)

O ajuste do tripwire é necessário para a extração: substitui duas declarações pelo sítio único e reduz o teto; não cria exceção nova em quantidade. A autorização depende da cláusula do usuário que permite adaptar testes existentes presos à regra inline. (`core/tests/test_tripwire_contagem_de_pendencia.py:83`, `:147`)

## 11/11 10:43 · sessao e683c1d88894 · MAPA-20 · verificador · origem=Code

REPROVADO para pouso. Há divergência do aval, falsos verdes no validador e mutantes do publicador que escapam ao selo. (Diff: `bin/mapa20_valida.py:18,33–59`; `bin/tests/test_mapa20_valida.sh:42–50`.)

Referências: **B** = base; **D** = arquivo resultante proposto pelo diff, com numeração pós-diff; **A** = anexo `aval_mapa_ate_o_20_1111.txt:1`.

1. **RC e todos os `exit`.** No fluxo implementado, validador vermelho **não termina em rc 0**: o `else` grava `MAPA_RC=1`, preservado até as duas saídas finais. Validador verde **não causa rc 1 sem outra falha**. (D `bin/relato.sh:94–103,142,148–149`.)

   | Linha de `relato.sh` após diff | Valor do `exit` | Gatilho |
   |---|---:|---|
   | 27 | 1 | Falha de lock |
   | 28 | 1 | Destino sem `.git` |
   | 32 | 1 | Falha na lista de nomes |
   | 35 | 1 | Falha em BACKLOG-TOPO |
   | 38 | 1 | Falha em ESTADO |
   | 42 | 1 | Falha em SESSAO |
   | 86 | 1 | Falha raspando RELATO retido |
   | 101 | 1 | Falha raspando MAPA retido |
   | 117 | 1 | Falha em gerar avais |
   | 132 | 1 | Falha na raspagem geral |
   | 142 | `$MAPA_RC` | Nada mudou |
   | 148 | 1 | Falha no `git push` ou no `echo` associado | [nao verificado]
   | 149 | `$MAPA_RC` | Final após publicação |

   Fontes da tabela: B `bin/relato.sh:27–42,86`; D `bin/relato.sh:101,117,132,140–149`.

   Ressalva: `cd`, `git add` e `git commit` continuam sem tratamento obrigatório de erro; não há `set -e`. Isso permite outros falsos sucessos, mas não zera o `MAPA_RC` vermelho. (B `bin/relato.sh:21`; D `bin/relato.sh:135–149`.)

2. **Ramo retido: escrita parcial possível e isolamento incompleto.** `DEST` recebe `DEST_RELATO` ou `/home/ronald/hasner-relato`. O ramo retido passa `$DEST/MAPA-20.md` como origem e, efetivamente, como destino do raspador. (B `bin/relato.sh:23`; D `bin/relato.sh:100–101`.)

   `raspar_publico.py` lê o conteúdo inteiro, mascara e confere; depois abre o destino com `'w'`. Falha antes dessa abertura preserva o arquivo; erro/interrupção durante a escrita pode deixá-lo truncado ou parcial no checkout público. Não há temporário nem substituição atômica. Ocorrência real: **[nao verificado]**. (B `bin/raspar_publico.py:28–40`.)

   O `|| exit 1` interrompe também a publicação do restante e não desfaz eventual truncamento. Portanto, “o resto segue” não vale quando essa raspagem falha. Num ciclo posterior, a cópia parcial pode ser re-raspada e incluída pelo teste de existência, sem validação estrutural da cópia pública. (D `bin/relato.sh:99–101,139,146–148`.)

   Comparação com 2e: CODEX-ESTADO gera e raspa em temporário antes de copiar, protegendo o destino de falhas dessas duas etapas. Entretanto, a cópia final é `cp` direto: também não garante atomicidade contra falha durante a cópia. O comentário “nunca arquivo pela metade” excede essa garantia. (B `bin/relato.sh:89–97`; B `bin/codex_estado.sh:25–38`.)

   Há ainda intervalo entre validar e publicar: o validador lê a origem, e a raspagem geral a lê novamente. Sem snapshot, alteração concorrente pode publicar bytes diferentes dos validados; adesão do escritor ao lock e ocorrência real: **[nao verificado]**. (D `bin/relato.sh:95–96,118–132`; D `bin/mapa20_valida.py:65`; B `bin/raspar_publico.py:28`; B `bin/relato.sh:27`.)

3. **Argumento `origem:destino`: o par novo está correto.** O raspador usa `par.rsplit(':', 1)`: o **último** `:` separa origem de destino. Assim, `:` na origem não quebra o argumento; `:` no destino muda a interpretação. (B `bin/raspar_publico.py:23–24`.)

   Os dois pares novos respeitam esse formato. `RAIZ` vem de `pwd`, sem restrição que proíba `:`; mesmo contendo esse caractere, ele permanece na parte da origem. Caminho real de produção: **[nao verificado]**. (B `bin/relato.sh:22`; D `bin/relato.sh:96,101`; B `bin/raspar_publico.py:24`.)

4. **Validador: falsos aceites, falsas recusas e universo divergente.** Para os exemplos expressamente perguntados:

   | Caso | Comportamento |
   |---|---|
   | Obra somente citada em prosa | Recusa: não satisfaz presença |
   | Obra somente na segunda tabela, após outra tabela | Recusa: examina apenas a primeira |
   | Célula de dados contendo somente espaços | Recusa |
   | Célula `-` | Aceita |
   | Linha inteira de separador | Ignora; não substitui uma obra |

   Fontes: D `bin/mapa20_valida.py:28–40,49–59`. Portanto, prosa, segunda tabela e espaços não são todos falsos aceites.

   **Contraexemplos aceitos indevidamente**, tomando uma tabela completa para `OBRAS + SEM DONO`:

   - Uma única coluna, contendo apenas as identidades: nenhuma coluna de conteúdo é exigida. (D `bin/mapa20_valida.py:48–59`; A.)
   - Cabeçalho com uma coluna e dados com três: a largura do cabeçalho é descartada antes da comparação. (D `bin/mapa20_valida.py:40,54–56`.)
   - Linhas `|E3|||`, e equivalentes para todas as identidades: `strip('|')` remove as colunas vazias finais. (D `bin/mapa20_valida.py:33–34,51–56`.)
   - Tabela inteira dentro de bloco cercado por crases; tabela sem separador; obras duplicadas. O parser não reconhece cercas, não exige separador e não acusa duplicação. (D `bin/mapa20_valida.py:30–40,49–50`.)
   - `-` em coluna que deveria trazer medição ou `nao medido`: só se testa vazio, não a declaração exigida pelo aval. (D `bin/mapa20_valida.py:51–59`; A.)

   **Recusas indevidas:** mapa completo na segunda tabela; tabela Markdown sem pipes externos; uma célula `x\|y.py:1`, cujo pipe escapado cria coluna artificial; mapa usando `UI-PAINEL`, literalmente autorizado. (D `bin/mapa20_valida.py:18,32–40,54–59`; A.)

   Comparação nome a nome:

   | Aval | `OBRAS` |
   |---|---|
   | E3 | E3 |
   | E4B | E4B |
   | E4C | E4C |
   | E4D | E4D |
   | RAIZ F1 | RAIZ F1 |
   | RAIZ F2 | RAIZ F2 |
   | RAIZ F3 | RAIZ F3 |
   | RAIZ F4 | RAIZ F4 |
   | RAIZ F5 | RAIZ F5 |
   | RAIZ F6 | RAIZ F6 |
   | RAIZ F7 | RAIZ F7 |
   | O168 | O168 |
   | LASTRO | LASTRO |
   | O169-FECHADA | O169-FECHADA |
   | **UI-PAINEL** | **UI-PAINEL-MOSTRA-O-APP — diverge** |
   | BO-ROTULO-ENCERRADO | BO-ROTULO-ENCERRADO |
   | TRIAGEM-OFFLINE-SEM-CAMERA | TRIAGEM-OFFLINE-SEM-CAMERA |
   | LOTE-VALIDAR-REGRESSAO-0311 | LOTE-VALIDAR-REGRESSAO-0311 |

   Fontes da comparação: A; D `bin/mapa20_valida.py:14–18`. Há 18 elementos, mas contar 18 não resolve a identidade errada.

5. **O selo tem mordida parcial; não certifica as garantias anunciadas.**

   | Caso MORDE | Mutante plausível que passa |
   |---|---|
   | `falta_obra` | Trocar `E3` por `INVENTADA`, mantendo 18: a fixture segue `M.OBRAS` |
   | `sem_dono` | Conferir apenas quantidade mínima de linhas, em lugar das identidades: retirar SEM DONO reduz a quantidade, mas uma duplicata poderia substituí-lo |
   | `celula_vazia` | Conferir vazios somente em E4C |
   | `sem_tabela` | Conferir presença somente de RAIZ F5 e SEM DONO, preservando a rejeição inicial de texto sem tabela; todas as outras obras deixam de ser protegidas |

   A fixture usa o próprio universo do validador; os únicos alvos pontuais são RAIZ F5 e E4C. Os mutantes de universo, presença restrita e vazio restrito passaram os casos em memória. (D `bin/tests/test_mapa20_valida.sh:19,22–28,36`; D `bin/mapa20_valida.py:45–59`.)

   Os checks de ligação também ficaram verdes, em memória, com estes mutantes de `relato.sh`:

   - `MAPA_RC=0` no `else`;
   - `if false; then # mapa20_valida.py`;
   - `MAPA_PAR` preenchido incondicionalmente;
   - retirada de MAPA do `git add` e do pathspec do commit; [nao verificado]
   - saída final `exit 0`, conservando `exit "$MAPA_RC"` dentro de `if false`.

   Esses checks apenas procuram texto/regex; não executam o publicador nem provam o fluxo das variáveis. (D `bin/tests/test_mapa20_valida.sh:40–50`.)

   **Comentário pode satisfazer a ligação:** só comentários de linha inteira são removidos; o nome em comentário inline satisfaz `find('mapa20_valida.py')`. A regex do ramo também ignora exatamente `|| exit 1`, incluindo a saída antecipada existente na raspagem retida. (D `bin/tests/test_mapa20_valida.sh:42–49`; D `bin/relato.sh:101`.)

6. **Selos vizinhos: nenhuma quebra textual identificada pelo diff.**

   - `test_codex_estado.sh`: o recorte começa em `CODEX_OK=""`, depois do bloco novo; permanece sem `exit`. (B `bin/tests/test_codex_estado.sh:104–113`; D `bin/relato.sh:111–113`.)
   - `test_relato_publica_claude_md.sh`: o par CLAUDE, o `git add` e o pathspec permanecem reconhecíveis. Seu mutante continua retirando CLAUDE e ficando vermelho. (B `bin/tests/test_relato_publica_claude_md.sh:15–20,29–30`; D `bin/relato.sh:132,136,147`.)
   - `test_codex_estado_evento.sh` e `test_relato_afirma_com_prova.sh`: os nomes procurados permanecem. Esses `grep` também aceitam comentários; `codex_estado.sh` já aparece em comentário da base. (B `bin/tests/test_codex_estado_evento.sh:28–30`; B `bin/tests/test_relato_afirma_com_prova.sh:44`; B `bin/relato.sh:93`.)
   - `test_publicacao_sem_nome.sh`: as menções ao publicador são comentários; o universo verificado vem de `git ls-files`, incluindo MAPA quando rastreado. Resultado sobre o repo público real: **[nao verificado]**. (B `bin/tests/test_publicacao_sem_nome.sh:58–72`.)

   Verde das suítes completas: **[nao verificado]**. [nao verificado]

7. **QUEM ESCREVE e QUEM LÊ.**

   | Valor | Quem escreve | Quem lê |
   |---|---|---|
   | `MAPA_RC` | Inicialização 0; `else` grava 1 | Saídas por nada alterado e final |
   | `MAPA_PAR` | Inicialização vazia; ramo verde grava o par | Expansão na raspagem geral | [nao verificado]
   | `MAPA_PATH` | Inicialização vazia; teste de existência grava o nome | Mensagem e pathspec do commit | [nao verificado]

   Fontes: D `bin/relato.sh:94,96,98,132,139,142,146–149`.

   `logs/MAPA-20/mapa.md`: o comentário atribui regravação à sessão por evento; não há produtor executável apresentado no diff. Escritor real: **[nao verificado]**. Leitores demonstrados: validador e, depois, raspador. (D `bin/relato.sh:89–96,132`; D `bin/mapa20_valida.py:63–65`; B `bin/raspar_publico.py:28`.)

   `$DEST/MAPA-20.md`: `raspar_publico.py` escreve tanto na publicação aprovada quanto na re-raspagem retida. Leem o arquivo o teste de existência, o próprio raspador no ramo retido e o Git para staging/commit. Leitura efetiva por usuários on-line: **[nao verificado]**. (D `bin/relato.sh:96,100–101,132,139,146–148`; B `bin/raspar_publico.py:28,40`.)

8. **Classes de reprovação, uma a uma.**

   | Classe | Conferência |
   |---|---|
   | **2º juiz duplicando registrado** | Duplicação de juiz de negócio não demonstrada. A privacidade continua consultando o módulo único `nome_no_texto`. (B `bin/raspar_publico.py:15–16,31–36`.) |
   | **2º escritor fora da porta** | As duas rotas MAPA usam o mesmo raspador; a falha demonstrada é escrita direta não atômica. Exclusividade e disciplina do produtor do mapa: **[nao verificado]**. (D `bin/relato.sh:96,101,132`; B `bin/raspar_publico.py:40`.) |
   | **Relógio fora da borda** | Nenhuma nova leitura temporal no validador. O `date` da mensagem de commit já existia na base. (D `bin/mapa20_valida.py:12–77`; B `bin/relato.sh:130`; D `bin/relato.sh:146`.) |
   | **Derivação paralela de fonte única** | O universo copiado diverge do aval em UI; o teste deriva sua expectativa da própria cópia. Não há derivação numérica de negócio nesse validador. (A; D `bin/mapa20_valida.py:14–18,43–60`; D `bin/tests/test_mapa20_valida.sh:22,36`.) |
   | **Selo sem mordida** | Procedente como cobertura insuficiente: mutantes de universo, validação e publicação sobrevivem, conforme item 5. (D `bin/tests/test_mapa20_valida.sh:19–36,42–50`.) |
   | **Ordem de trava** | O bloco MAPA executa sob o lock já adquirido. A chamada CODEX `--gerar` retorna antes do lock do caminho de evento; não nasce reaquisição nesse caminho. Trava do escritor do mapa: **[nao verificado]**. (B `bin/relato.sh:27`; D `bin/relato.sh:95–112`; B `bin/codex_estado.sh:54–66`.) |
   | **Pouso antes do pré-requisito** | Contrato efetivamente publicado, suite integral verde e opus final: **[nao verificado]**. São requisitos registrados, e a menção ao contrato no comentário não os demonstra. (B `CLAUDE.md:377–381,394`; D `bin/relato.sh:93`.) |
   | **Campo com dois significados** | `MAPA_RC` representa o resultado do validador; `MAPA_PAR`, a inclusão aprovada; `MAPA_PATH`, somente existência local. Este último não comprova validação nem atualização: inclui também a cópia retida. (D `bin/relato.sh:94–101,132,139,142–149`.) |

## 11/11 10:42 · sessao b9ef413725f4 · - · - · origem=Ronald (tmux)

1. **Divergência do universo:** o aval pede `UI-PAINEL`; o validador exige `UI-PAINEL-MOSTRA-O-APP`. Os demais nomes coincidem: `E3`, `E4B`, `E4C`, `E4D`; `RAIZ F1`, `RAIZ F2`, `RAIZ F3`, `RAIZ F4`, `RAIZ F5`, `RAIZ F6`, `RAIZ F7`; `O168`, `LASTRO`, `O169-FECHADA`; `BO-ROTULO-ENCERRADO`, `TRIAGEM-OFFLINE-SEM-CAMERA`, `LOTE-VALIDAR-REGRESSAO-0311`. Uma tabela com os nomes literais do aval recebe rc 1 pela divergência de UI. (`aval_mapa_ate_o_20_1111.txt:1`, anexo; diff `bin/mapa20_valida.py:15-18,57-59`.)

2. **Os exemplos sugeridos não são todos falsos aceites:** obra somente na prosa não satisfaz presença; obra somente na segunda tabela também não; célula contendo espaços é recusada; célula `-` passa; linha inteira de separador é ignorada e não substitui obra. Reproduzido em memória: respectivos rc `1,1,1,0`; inserir separador entre linhas válidas preserva rc 0. (`diff bin/mapa20_valida.py:30-40,49-59,73`.)

3. **Falsos aceites reproduzidos, rc 0:**
   - Tabela com todas as identidades e **uma coluna apenas**; nenhuma coluna exigida pelo aval é cobrada. (`diff bin/mapa20_valida.py:48-59`; aval anexo:1.)
   - Cabeçalho com uma coluna e dados com três: somente larguras das linhas de dados são comparadas. (`diff bin/mapa20_valida.py:40,54-56`.)
   - Linhas `|E3|||`, repetindo o padrão para todas as obras e `SEM DONO`: `strip('|')` elimina as colunas vazias finais. (`diff bin/mapa20_valida.py:34,51-56`.)
   - Conteúdo completo dentro de bloco de código cercado por crases; tabela sem separador; linhas duplicadas: o parser não distingue contexto de código, não exige separador e sobrescreve a identidade no dicionário. (`diff bin/mapa20_valida.py:30-40,49-50`.)
   - `-` nas colunas de medição também passa: o validador verifica presença textual, sem conferir medição ou declaração `nao medido`. (`diff bin/mapa20_valida.py:51-59`; aval anexo:1.)

4. **Falsas recusas reproduzidas, rc 1:** mapa completo na segunda tabela após tabela alheia; tabela Markdown sem pipes externos; uma célula com pipe escapado `x\|y.py:1`, por produzir largura adicional; mapa com `UI-PAINEL` literal do aval. (`diff bin/mapa20_valida.py:18,32-40,54-59`; aval anexo:1.)

5. **Mutantes executados em memória passam os cinco casos do selo novo**, com sequência `verde=0`, `MORDE_falta_obra=1`, `MORDE_sem_dono=1`, `MORDE_celula_vazia=1`, `MORDE_sem_tabela=1`: [nao verificado]
   - Trocar `E3` por `INVENTADA` em `OBRAS`, conservando tamanho 18: o teste fabrica sua expectativa a partir da própria lista mutada. (`diff bin/tests/test_mapa20_valida.sh:22-27,36`.)
   - Conservar `OBRAS`, mas conferir presença apenas de `RAIZ F5` e `SEM DONO`: a única obra retirada pelo teste é F5. (`diff bin/tests/test_mapa20_valida.sh:25-26`.)
   - Conferir células vazias somente em `E4C`: o único vazio testado está nessa obra. (`diff bin/tests/test_mapa20_valida.sh:19,27`.)

   A verificação de arquivo ausente continua verde nesses mutantes, porque a captura de `OSError` permanece intacta. (`diff bin/mapa20_valida.py:64-68`; diff `bin/tests/test_mapa20_valida.sh:35`.)

6. **Classes relativas ao validador:** selo sem mordida é demonstrado pelos mutantes acima; fonte única do universo não é certificada pelo teste, que deriva a fixture do objeto sob teste; o próprio validador declara usar o universo literal do aval e o desrespeita em UI. (`diff bin/tests/test_mapa20_valida.sh:22-27,36`; diff `bin/mapa20_valida.py:7-8,18`.) A regra de pré-verificação exige conferir essas classes e reserva o pouso à leitura final subsequente. (`CLAUDE.md:372-381`.)

## 11/11 10:41 · sessao 75140b8e444c · - · - · origem=Ronald (tmux)

5. O selo mata remoções completas de verificações, mas usa o próprio `M.OBRAS` para gerar quase todo o oráculo; trocar `E3` por `E3-ERRADA`, mantendo 18 elementos, passa nos casos e na contagem. Os únicos alvos específicos são ausência de `RAIZ F5`, ausência de `SEM DONO`, terceira célula vazia em `E4C` e texto sem tabela. [diff:bin/tests/test_mapa20_valida.sh:19,22,25-28,36]

Comprovação somente em memória: os seguintes mutantes do publicador passam os quatro checks textuais: [nao verificado]

- `MAPA_RC=0` no ramo vermelho.
- Trocar a chamada por `if false; then # mapa20_valida.py`.
- Inicializar `MAPA_PAR` já preenchido, incondicionalmente.
- Retirar MAPA do `git add` e do pathspec de commit. [nao verificado]
- Encerrar com `exit 0`, preservando `exit "$MAPA_RC"` dentro de `if false`.

Os predicados apenas buscam posição, substring, regex de ramo e alguma linha com `exit "$MAPA_RC"`; não executam o publicador nem exigem atribuição vermelha, caminho condicionado, staging ou saída final efetiva. [diff:bin/tests/test_mapa20_valida.sh:40-50] [nao verificado]

Comentários de linha inteira são removidos, mas comentários inline permanecem: o mutante `if false; then # mapa20_valida.py` fica verde. A regex `(?<!\|\| )\bexit\b` também ignora exatamente `|| exit 1`, portanto não acusa a saída antecipada já existente na raspagem retida. [diff:bin/tests/test_mapa20_valida.sh:42,44,48-49; diff:bin/relato.sh:100] [nao verificado]

6. Nenhum check textual vizinho quebra com esse diff:

- CODEX: o recorte começa em `CODEX_OK=""`, depois do bloco novo; continua sem `exit`. [base:bin/tests/test_codex_estado.sh:104-113]
- CLAUDE: regex da raspagem, `git add` e pathspec continuam satisfeitos; o mutante que remove CLAUDE continua vermelho. [base:bin/tests/test_relato_publica_claude_md.sh:15-20,29-30; diff:bin/relato.sh:132-149]
- Evento CODEX e afirmação com prova: os nomes buscados permanecem; os `grep` aceitam comentários também. [base:bin/tests/test_codex_estado_evento.sh:28-30; base:bin/tests/test_relato_afirma_com_prova.sh:44]
- Publicação sem nome não lê o publicador: as ocorrências de `relato.sh` são comentários; o universo vem de `git ls-files`, incluindo MAPA depois de rastreado. Verde real sem dados do repo público: [não verificado]. [base:bin/tests/test_publicacao_sem_nome.sh:58-72] [nao verificado]

## 11/11 10:40 · sessao 61a4a2c2a774 · - · - · origem=Ronald (tmux)

1. **Saídas/RC:** no fluxo normal de comandos do arquivo, não há caminho em que validador falhe e relato termine em 0: `MAPA_RC` começa em 0, recebe 1 no `else` e ambos os términos finais usam esse valor; os demais términos explícitos usam 1. Validador verde também não produz RC 1 sem outra falha. [diff `bin/relato.sh`:94–103,142,148–149]

   Todos os `exit` após diff:
   - RC **1**: 27 (lock), 28 (repo), 32 (nomes), 35 (backlog), 38 (estado), 42 (sessão), 86 (raspagem RELATO retido), 101 (raspagem MAPA retido), 117 (gerar avais), 132 (raspagem geral), 148 (push ou `echo` final falhando). [base `bin/relato.sh`:27–42,86; diff `bin/relato.sh`:101,117,132,148]
   - RC **`$MAPA_RC`**: 142 (nada mudou), 149 (após publicação): 0 se verde, 1 se vermelho. [diff `bin/relato.sh`:94–98,140–149]
   
   Há falhas não propagadas em `cd`, `git add` e `git commit`, pois não há `set -e`; isso é outro problema, mas não transforma MAPA vermelho em RC 0. [base `bin/relato.sh`:21; diff `bin/relato.sh`:135–149]

2. **Retido e parcial:** variável `DEST` recebe `DEST_RELATO` ou caminho padrão; o ramo retido passa o próprio arquivo público como origem e destino. [base `bin/relato.sh`:23; diff `bin/relato.sh`:100–101] O raspador primeiro lê tudo, mascara e confere; só então abre o destino em `'w'`, sem arquivo temporário ou rename. Erro anterior a essa abertura preserva arquivo; erro de escrita/interrupção após truncamento pode deixar arquivo vazio/parcial no checkout público; ocorrência real não demonstrada. [base `bin/raspar_publico.py`:28–40; ocorrência: não verificado] O `|| exit 1` impede o commit normal daquela execução, mas não desfaz a escrita. [diff `bin/relato.sh`:101]

   CODEX-ESTADO gera e raspa em temporário, protegendo o destino contra falhas do gerador/raspador; entretanto a cópia final usa `cp` direto, que também não é substituição atômica e pode falhar durante a escrita. Portanto o comentário “nunca arquivo pela metade” é mais forte que a implementação. [base `bin/codex_estado.sh`:25–38; base `bin/relato.sh`:89–95]

3. **Formato:** usa `src, dst = par.rsplit(':', 1)`: último `:` separa origem e destino. O par novo respeita o formato; `:` na origem **não quebra**. `:` no destino altera a separação. [base `bin/raspar_publico.py`:23–24; diff `bin/relato.sh`:96,101,132] `RAIZ` vem de `cd ... && pwd`, sem regra que proíba `:`; portanto o código não garante ausência desse caractere, e a construção nova continua funcionando com ele na origem. [base `bin/relato.sh`:22; base `bin/raspar_publico.py`:24]

7. **Produtores/consumidores:**
   
   | Valor | Escreve | Lê |
   |---|---|---|
   | `MAPA_RC` | inicialização 0 e ramo vermelho 1 | dois exits finais |
   | `MAPA_PAR` | inicialização vazia e ramo verde com par | expansão condicional na raspagem geral | [nao verificado]
   | `MAPA_PATH` | vazio; recebe nome se arquivo público existe | mensagem e pathspec do commit | [nao verificado]
   
   [diff `bin/relato.sh`:94,96,98,132,139,142,146–149]

   `logs/MAPA-20/mapa.md`: o comentário atribui regravação à sessão por evento, mas não há escritor executável no diff; a existência/produção real não é verificável nesta cópia, que não contém `logs`. O validador lê via `open(...).read()` e o raspador lê novamente na publicação. [diff `bin/relato.sh`:89–96; diff `bin/mapa20_valida.py`:63–65; base `bin/raspar_publico.py`:28; produção real: não verificado]

   `MAPA-20.md` público: escreve `raspar_publico.py` tanto pelo par aprovado na raspagem geral quanto pelo par do ramo retido; lê o teste `-f`, o próprio raspador no ramo retido e o Git ao adicionar/commitar/publicar. [diff `bin/relato.sh`:96,100–101,132,139,146–148; base `bin/raspar_publico.py`:28,40]

   **Janela entre validar e publicar:** lê o mapa para validar e depois torna a lê-lo para raspar; não há snapshot. O lock do publicador não demonstra que o escritor do mapa o obedece. Assim, mudança concorrente pode publicar bytes diferentes dos validados; ocorrência real não demonstrada. [base `bin/relato.sh`:27; diff `bin/relato.sh`:95–96,118–132; diff `bin/mapa20_valida.py`:65; base `bin/raspar_publico.py`:28; concorrência real: não verificado]

## 11/11 10:36 · sessao 1ca6effd2852 · - · - · origem=Ronald (tmux)

Há 48 IDs de primeira coluna adicionais aos 18 nomes pedidos; aliases E3/E4B/UI podem ser integrados às linhas obrigatórias. A lista completa foi enviada em duas mensagens, preservando os números de `backlog_obras.txt:852` a `backlog_obras.txt:2367`.

A instrução literal também alcança obras nomeadas dentro das descrições: UI-ANEXO-AUSENCIA (`backlog_obras.txt:1131`), JUIZ-DO-PAR (`backlog_obras.txt:1732`), JUIZ-1 e CRONS-INSTALL-SOB-MARCA (`backlog_obras.txt:1761`), PLANTIO (`backlog_obras.txt:1852`), E1-FILA-V2-2-TRAVA (`backlog_obras.txt:1853`), BO-0611-05 (`backlog_obras.txt:1889`), BUG-PAR (`backlog_obras.txt:1895`), O137/ESMERIL-NO-RASTRO (`backlog_obras.txt:1899`), BO-0611-05-RESTO-DA-CLASSE (`backlog_obras.txt:1905`), O284-A (`backlog_obras.txt:1911`) e ORDEM-ATE-O-ZERO (`backlog_obras.txt:2367`). A distinção entre obra própria, alias e referência histórica precisa ser declarada; omitir silenciosamente esses nomes contraria a leitura literal. [nao verificado: critério final de identidade]

E4C/E4D e RAIZ F2–F7 existem como etapas nomeadas, sem linha própria no BACKLOG anexado; não devem ser anunciadas como ausentes do BACKLOG inteiro (`backlog_obras.txt:1878`, `backlog_obras.txt:1895`). LASTRO genérico aparece na META, mas seus registros específicos são LASTRO-DEGRAU-2 e LASTRO-PARTE-2/3 (`backlog_obras.txt:1732`, `backlog_obras.txt:1761`, `backlog_obras.txt:1763`, `backlog_obras.txt:2367`).

Todos os impactos de extras em `fio_morto_alcance` e na Central devem permanecer `nao medido`; nenhuma medida desses efeitos foi dada para eles. [nao verificado]

## 11/11 10:36 · sessao 599f583cc054 · - · - · origem=Ronald (tmux)

Censo somente leitura, na BASE `9ca563bb3`; as linhas dos arquivos alterados precisarão ser atualizadas após a cura. [nao verificado]

**Quem escreve e quem lê**

- `pendentes` é uma lista derivada, não um campo persistido: `consulta_do_app` escreve a lista com perguntas vivas sem `respondida_em` e com disputa sem `fechada_em`; conserva `historico` separado. A autoridade da vitalidade é `perguntas_do_app`, que chama `pergunta_viva`. (`chamados/services/pergunta_no_app.py:45`, `:61`, `:69`, `:71`, `:75`)
- Escritores persistentes de `respondida_em`: resposta em `chamados/services/disputa_emissao.py:1468`; reabertura em `:1593`; resposta automática em `:1653`; reconstrução pela trilha em `:1674`; ressurreição em `chamados/models.py:1566`; `carimbar` em `:3132`. A atribuição `models.py:3163` desfaz estado apenas em memória. (`chamados/models.py:3159`)
- Escritores de `DisputaSupervisao.fechada_em`: fechamento em `chamados/models.py:1932`; reaberturas em `:2149` e `:2173`.
- Leitores diretos de `consulta_do_app` em produção: dois badges `/me/` (`api/views_core.py:313`, `api/views.py:324`), listagem de chamados (`api/views.py:1004`), perguntas da disputa (`api/views.py:2542`), dashboard (`core/views.py:252`), aviso de pendência (`chamados/services/aviso_pendencia.py:8`) e cobrança (`chamados/services/cobranca.py:47`).
- `FollowupChamado.lido_colaborador_em` nasce nulo: campo aceita nulo e a criação não fornece carimbo. (`chamados/models.py:1673`, `chamados/services/nota.py:88`) A escrita de leitura passa por `marcar_lidas`, que resolve o nome em `LADOS` e faz `update` com instante. (`chamados/services/nota.py:59`, `:126`, `:132`) Chamadores do lado colaborador: `api/views.py:1050`, `chamados/views.py:1135`, `:1150`.
- Leitores de `lido_colaborador_em`: badges dos kernels (`api/views_core.py:311`, `api/views.py:322`), serializador de followups (`api/views.py:1128`), contador web (`chamados/views.py:603`) e indicação “novo” do cartão (`templates/chamados/partials/_card_atendimento.html:51`, `:59`).
- `ChamadoColaborador.status_local`: escritor de transição persistente é `_gravar_estado`, atribui e salva. (`chamados/models.py:201`, `:228`, `:296`) Nascimento também fornece status no INSERT, pela porta `criar`. (`chamados/models.py:1095`) Atribuições `:356` e `:371` simulam estado em memória. A query do badge lê `status_local__in=VIVOS`; o conjunto inclui `aberto`, `em_analise` e `registrado`. (`api/views_core.py:309`, `api/views.py:320`, `chamados/catalogo/motor.py:24`)

**Outros leitores dos rótulos**

O grep no código de produção, incluindo templates e JS, encontrou a regra duplicada nos dois kernels e uma terceira regra manual no dashboard, além de usos textuais de “Chamados” sem regra de badge. (`api/views_core.py:316`, `:318`, `api/views.py:327`, `:329`, `templates/core/dashboard_colaborador_v2.html:80`)

A linha de fila deve apontar `templates/core/dashboard_colaborador_v2.html:80`: escolhe “Responder DP”/“Chamados” pelo número de perguntas, sem ramo vermelho; o número vem de `core/views.py:253`. Outros usos são rótulo de voltar (`templates/chamados/form_abertura.html:7`), cabeçalhos de relatório (`relatorios/views.py:1672`, `inteligencia/views.py:255`) e chave de comando (`ponto/management/commands/zerar_dados_operacionais.py:52`). O grep em `static` por ambos os literais não encontrou ocorrências. [nao verificado]

**Leitores `api/views.py:1004` e `:2542`**

- `api_chamados_v2`: consulta perguntas e histórico, indexa as perguntas por chamado vivo/destino e serializa a conversa. Não calcula badge. Seu GET marca followups do DP como lidos, portanto não serve para comparar o contador anterior. (`api/views.py:1003`, `:1040`, `:1044`, `:1050`)
- `api_disputa_perguntas`: publica pendentes, histórico e o subconjunto `respondidas`; mantém `em_disputa`, IDs e opções servidas. Não calcula badge. (`api/views.py:2531`, `:2543`, `:2571`, `:2573`, `:2581`, `:2582`) Ambos devem continuar usando `consulta_do_app`; nenhuma mudança nesses leitores é necessária. (`chamados/services/pergunta_no_app.py:61`)

**Contrato adicional a tratar**

O teste de contagem registra os dois sítios inline dos badges como exceções `MENSAGEM`. Ao mover a query, o contrato exige retirar os dois sítios antigos, declarar o sítio da projeção e reduzir o teto de acordo; sem isso haverá assertions por sítio novo e declarações obsoletas. (`core/tests/test_tripwire_contagem_de_pendencia.py:83`, `:85`, `:383`, `:390`, `:395`) Não editei arquivos nem executei banco. [nao verificado]

## 11/11 10:36 · sessao a41024c8adde · - · - · origem=Ronald (tmux)

- **Causas a–e:** E4B é a dona escrita de todas as cinco classes; não é necessário inventar dono por semelhança de código. O BACKLOG anexo diz literalmente “backfill do fio morto (matriz_e4 C3, classes a-e)”; a cópia confirma o escopo. Isso prova responsabilidade, não redução do baseline nem cobertura eficaz medida. (`backlog_obras.txt:1887`; `app/docs/BACKLOG.md:1875`.)
- **Significados:** a = chamado vivo fora da fila; b = resposta carimbada sem texto; c = respondida com destino terminal; d = colaborador na lista CXR e dia fora; e = restante discriminado. O subtipo de e `pergunta_viva_sob_chamado_terminal` tem ramo próprio. (`app/chamados/services/vigia_fio.py:519`; `app/chamados/services/vigia_fio.py:538`; `app/chamados/services/vigia_fio.py:667`; `app/chamados/services/vigia_fio.py:688`.)
- **SEM DONO:** os dados permitem dizer que nenhuma causa a–e ficou sem obra dona explícita, porque E4B nomeia todas. Não permitem dar como zero o resíduo depois das obras; redução efetiva não medida continua “nao medido”. A coluna de tamanho solicitada deve permanecer literalmente “a medir pela sessao”. (`backlog_obras.txt:1887`; impacto pós-obras: [nao verificado].)

Para as linhas RAIZ, a sequência escrita é F0→F1→F2=BO-DISPUTA→F3→F4=BUG-PAR→F5 com E4d→F6→F7. Essa ordem não define por si o inventário de arquivos de cada fase. (`backlog_obras.txt:1895`; `app/docs/BACKLOG.md:1883`.)

- **RAIZ F1:** sítios de destino em `app/chamados/models.py:2982`, `:3014`; `app/chamados/services/fio.py:925`; eleitores em `app/chamados/services/disputa_emissao.py:238`, `:1197`; `app/chamados/services/declaracao_texto.py:377`. F1 aparece respondida no anexo recente, precedendo c22 depois de E3/E4B; a lei antiga ainda aberta no RELATO da cópia é anterior e não deve prevalecer. (`backlog_obras.txt:1875`; `app/docs/RELATO.md:19`.)
- **RAIZ F2:** equivale a BO-DISPUTA e utiliza a cópia c21/c22 preservada: `app/chamados/management/commands/auditar_invariantes_chamados.py`, `app/chamados/services/declaracao_texto.py`, `app/chamados/services/descarte_disputa.py`, `app/chamados/services/disputa_emissao.py`, `app/core/observ.py`, `app/ponto/management/commands/reconciliar_grade.py`. (`arquivos_obras.txt`, bloco c21/c22: [nao verificado quanto à numeração do anexo]; `backlog_obras.txt:1875`; `backlog_obras.txt:1895`.)
- **RAIZ F3/F4:** reutilizar o mesmo censo, sem alegar divisão exaustiva entre as duas fases. Sítios centrais: `app/chamados/models.py:1853`, `:3103`; `app/chamados/services/materializacao.py:425`, `:875`, `:1158`; `app/chamados/services/validacao.py:134`, `:199`, `:461`; `app/chamados/services/disputa_emissao.py:1508`, `:1632`; `app/chamados/reconciliador.py:411`, `:611`, `:1170`; `app/chamados/services/becos.py:56`; `app/chamados/signals.py:238`; `app/chamados/services/explicador.py:137`; `app/chamados/relatorios_disputa.py:20`. Essas referências são as do `f3f4_censo.r` fornecido, não novo levantamento. (`f3f4_censo.r`: [nao verificado quanto à numeração do anexo].)
- **RAIZ F4 especificamente:** par da pausa pela célula, perda em `app/chamados/models.py:3441`, fonte `PARES_PAUSA_DNA`; não atribuir D8 como bloqueio de F3/F4. (`backlog_obras.txt:1896`; `leis_abertas.txt`, RELATO:31.)
- **RAIZ F5:** coordenada com E4d; o inventário fechado próprio dessa fase não foi fornecido. Pode citar a dependência da parada da projeção e preservar as seis exceções, sem inventar lista exaustiva de alterações. (`backlog_obras.txt:1895`; `app/docs/PROMPTS.md:2030`; `app/docs/PROMPTS.md:2034`.)
- **RAIZ F6/F7:** há nome e posição na sequência; escopo, arquivos, voltas e efeito visível específicos = “nao medido”. Não converter os lotes sugeridos do censo em fases oficiais. (`backlog_obras.txt:1895`; `app/docs/BACKLOG.md:1883`.)
- **O168:** `app/ponto/supra_juiz.py:539` e cartório do evento; retirar `--executar` depende da guarda funcionar no evento e de `CSF_RETRATADO` medido em zero. Nada disso é medida atual de fio_morto_alcance. (`backlog_obras.txt:1396`; `app/docs/BACKLOG.md:1395`; `app/ponto/services/cartorio.py:1091`.)
- **LASTRO:** o nome abreviado agrega obras distintas; não declarar uma única etapa para todo o agregado sem ressalva. Degrau 2 toca `app/ponto/services/lastro.py:274` e depende do juiz do par; parte 2 depende de JUIZ-1 e CRONS-INSTALL; parte 3 já pousou e toca emissão/reação. (`backlog_obras.txt:1732`; `backlog_obras.txt:1761`; `backlog_obras.txt:1763`.)

**C — cautela:** compartilhamento de arquivo prova cruzamento e necessidade de integração, não desmente sozinho uma ordem. O censo F3/F4 pede coordenação das testemunhas com E4c e parada com E4d, mas não fornece número que prove uma inversão obrigatória. (`f3f4_censo.r`, “Ordem sugerida”, item 5: [nao verificado quanto à numeração do anexo]; `app/docs/BACKLOG.md:1883`.)

A declaração META de O169-FECHADA “4 arquivos, 0 comuns com E3/E4B” diverge da contagem bruta da cópia anexa, que lista cinco arquivos, dos quais dois de produção; a ausência de arquivos comuns com E3/E4B continua confirmada pelo inventário fornecido. Isso é divergência de contagem, não premissa de ordem desmentida. (`meta18_linha.txt:1`; `arquivos_obras.txt`, blocos E3/E4B/O169-FECHADA: [nao verificado quanto à numeração do anexo].)

## 11/11 10:35 · sessao 5b642dda3528 · - · - · origem=Ronald (tmux)

Furos concretos para D:

- **QUEM CHAMA:** o caminho `logs/MAPA-20/mapa.md → MAPA-20.md` ainda não entra na raspagem, no staging nem no commit; acrescentar apenas o par não publica o arquivo. (`bin/relato.sh:103`, `bin/relato.sh:117`, `bin/relato.sh:121`, `bin/relato.sh:130`, `bin/relato.sh:131`)
- **QUEM ESPERA / LIMITE:** o publicador espera o lock por até 600 s e depende da obtenção da lista de nomes e dos geradores; o contrato não declara essas dependências nem como a atualização por evento trata falha/timeout. (`bin/relato.sh:27`, `bin/relato.sh:32`, `bin/relato.sh:35`, `bin/relato.sh:38`, `bin/relato.sh:42`, `bin/relato.sh:102`)
- **FALHA:** origem ausente é pulada com sucesso; não existe nesses dois programas validação de obras META-18 ou de células vazias. O `rc 1` prometido exige validação adicional antes da cópia. (`bin/raspar_publico.py:25`, `bin/raspar_publico.py:27`, `bin/raspar_publico.py:40`, `bin/raspar_publico.py:46`)
- **MEDIDA:** os programas contam substituições de nomes/CPF, não obras nem cobertura de `fio_morto_alcance`/Central; o contrato não define identificação única, tratamento das obras extras ou deduplicação de impactos. Implementação dessa conferência: [nao verificado]. (`bin/raspar_publico.py:41`, `bin/raspar_publico.py:45`)
- **FALHA DE PUBLICAÇÃO:** a atualização da linha local não prova publicação; o push pode falhar e não há retry por evento declarado no contrato. (`bin/relato.sh:132`)
- **QUEM DESLIGA:** falta definir a operação de desligamento e o destino da página já publicada; o script não possui condição de 20/20 e o próprio precedente documenta remoção do par **e** remoção do arquivo público. (`bin/relato.sh:101`, `bin/relato.sh:103`, `bin/relato.sh:132`)

Não executei os publicadores; execução, chamadas externas, estado público e comportamento da esteira em produção: [nao verificado]. [nao verificado]

## 11/11 10:34 · sessao 8051d24e8124 · MAPA-ATE-O-20 · sonda · origem=Code

```text
sonda_leitura: sonda_central.py [3912ec00c599cf5c] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

central_qs_na_fila n 3634
por_status_local 'aberto' n 2026
por_status_local 'em_analise' n 1608
por_categoria 'Ausencia' n 2710
por_categoria 'disputa_supervisao' n 323
por_categoria 'turno_aberto' n 171
por_categoria 'auditoria_clt' n 128
por_categoria 'Ponto' n 97
por_categoria 'sem_previsao_escala' n 63
por_categoria 'Outros' n 50
por_categoria 'batida_espuria' n 25
por_categoria 'turno_aberto_24h' n 18
por_categoria 'ausencia' n 14
por_categoria 'cadastro' n 12
por_categoria 'Escala' n 5
por_categoria 'App' n 3
por_categoria 'Documentos' n 3
por_categoria 'Ferias' n 3
por_categoria 'acesso_bloqueado' n 3
por_categoria 'Holerite' n 2
por_categoria 'agenda_aceite' n 2
por_categoria 'batida_fora_horario' n 1
por_categoria 'geofence' n 1
por_urgencia 'normal' n 1861
por_urgencia 'urgente' n 1295
por_urgencia 'emergencia' n 322
por_urgencia 'critica' n 140
por_urgencia 'consulta' n 16
agora 2026-11-11 10:34:04
```

## 11/11 10:28 · sessao 8a5868b1f67a · RAIA-2-BO-VISIVEL · sonda · origem=Code

```text
sonda_leitura: sonda_lote0311.py [63cd70bd29f499d7] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

LOTE 2026-10-28 09:33:35 pedidas=47 validadas=29 recusadas=18
LOTE 2026-10-28 09:33:57 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:34:20 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:34:38 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:35:34 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 09:40:34 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 14:19:49 pedidas=26 validadas=11 recusadas=15
LOTE 2026-10-28 14:20:09 pedidas=15 validadas=0 recusadas=15
LOTE 2026-10-28 15:41:19 pedidas=29 validadas=12 recusadas=17
LOTE 2026-10-28 16:24:03 pedidas=16 validadas=0 recusadas=16
LOTE 2026-10-28 16:24:24 pedidas=16 validadas=0 recusadas=16
LOTE 2026-10-29 13:27:43 pedidas=24 validadas=10 recusadas=14
LOTE 2026-10-29 14:19:11 pedidas=31 validadas=18 recusadas=13
LOTE 2026-10-29 14:42:19 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-29 14:42:38 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-29 14:43:01 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-29 14:43:19 pedidas=12 validadas=0 recusadas=12
LOTE 2026-10-30 08:20:15 pedidas=33 validadas=4 recusadas=29
LOTE 2026-10-30 08:22:45 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 08:24:19 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 08:42:57 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 09:48:15 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:12:51 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:30:44 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:33:23 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 12:35:49 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 13:04:01 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 15:28:31 pedidas=29 validadas=0 recusadas=29
LOTE 2026-10-30 15:29:07 pedidas=29 validadas=0 recusadas=29
LOTE 2026-11-03 08:06:21 pedidas=87 validadas=45 recusadas=42
LOTE 2026-11-03 16:26:49 pedidas=47 validadas=10 recusadas=37
LOTE 2026-11-05 08:02:56 pedidas=57 validadas=17 recusadas=40
LOTE 2026-11-05 16:11:28 pedidas=46 validadas=6 recusadas=40
LOTE 2026-11-06 07:59:36 pedidas=56 validadas=16 recusadas=40
LOTE 2026-11-06 10:21:21 pedidas=45 validadas=5 recusadas=40
LOTE 2026-11-11 08:56:00 pedidas=119 validadas=55 recusadas=64
LOTE 2026-11-11 09:20:37 pedidas=59 validadas=0 recusadas=59
linhas n 37
```

## 11/11 10:25 · sessao 8fe05992e025 · FIO-E0-V4-E4B · construtor · origem=Code

Adicionado spy no módulo vivo e assertion que detecta a entrada de `(colab, DIA)` no ato, mesmo após rollback: [teste:217](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/test_e4_backfill.py:217). [nao verificado]

Memória: 13 testes OK; `entrega_cerca_equivalente` falha pela nova assertion, com `bang:exportada` preservado. [Resultados:70](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v13.txt:70). [nao verificado]

[Causa, RED, mutantes e comandos ORM no recibo v13](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c4-v13.txt:1). ORM desta volta [nao verificado]; certificação fica com Code/suite e opus final. [nao verificado]

## 11/11 10:21 · sessao ffcb7cf7e4ff · LOTE-VALIDAR-REGRESSAO-0311 · leitura · origem=Code

**Classificação: (c), indeterminado sem sonda.** Há multiplicações de trabalho comprováveis no caminho atual, mas faltam quantidade e composição dos lotes, execução concorrente e comparação das revisões efetivamente implantadas para atribuir a regressão a uma causa pontual. A primeira sonda pode aproveitar a auditoria que a própria view grava com as contagens do lote. (`app/chamados/views_cobrar.py:383`; `app/chamados/services/validacao.py:727`; implantação e medições faltantes: [nao verificado])

Uso “código atual” para a cópia fornecida. Ela contém alterações identificadas como posteriores ao primeiro POST lento, incluindo 05/11 e 06/11; portanto, seu custo inteiro não pode ser atribuído ao intervalo 30/10–03/11. (`app/ponto/portas/celula.py:891`; `app/chamados/signals.py:242`) Os diffs históricos por hash não puderam ser conferidos nesta cópia: [nao verificado].

**1. Caminho do POST e custo visível**

A rota chega a `validar_lote_tela`, verifica permissão e método, recebe `refs`, executa o lote, registra auditoria e renderiza o resultado. O HTTP 200 também pode apresentar recusa; não comprova materialização de todos os itens. (`app/chamados/urls.py:37`; `app/chamados/views_cobrar.py:376`; `app/chamados/views_cobrar.py:380`; `app/chamados/views_cobrar.py:386`; `app/templates/chamados/partials/_lote_preview.html:7`)

| Etapa | Chamada e trabalho |
|---|---|
| Seleção do lote | Converte referências para inteiros, deduplica em uma lista, faz **um SELECT inicial** das perguntas com suas disputas e percorre as referências **serialmente**. A deduplicação usa busca na própria lista, podendo exigir trabalho quadrático na quantidade de referências. (`app/chamados/services/validacao.py:713`; `app/chamados/services/validacao.py:719`; `app/chamados/services/validacao.py:724`; `app/chamados/services/validacao.py:727`) |
| Reclassificação por item | `classe_do_lote` resolve delta, resolve slot novamente e consulta existência de batida apurável em ±60 minutos. Só a classe A segue para validação. (`app/chamados/services/validacao.py:735`; `app/chamados/services/validacao.py:864`; `app/chamados/services/validacao.py:872`; `app/chamados/services/validacao.py:875`) |
| Entrada na validação | `acao_validar_pergunta` chama `validar_pergunta`, que busca novamente a pergunta, verifica resposta, validação anterior e implausibilidade. (`app/chamados/services/validacao.py:746`; `app/chamados/services/acoes_disputa.py:19`; `app/chamados/services/validacao.py:389`; `app/chamados/services/validacao.py:404`) |
| Carimbo | Dentro de transação, trava colaborador/disputas/chamados, grava `validada_em/por` e registra a decisão. A gravação da pergunta dispara seus receivers. (`app/chamados/services/validacao.py:425`; `app/chamados/services/validacao.py:431`; `app/chamados/services/validacao.py:435`; `app/chamados/services/validacao.py:137`; `app/chamados/services/validacao.py:139`) |
| Recálculo por validação | Agenda `recalcular_por_evento`, que chama o recálculo da **competência daquele colaborador**. (`app/chamados/services/validacao.py:446`; `app/ponto/services/fechamento.py:1809`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`) |
| Materialização da pergunta | Para resposta de hora ainda não materializada, chama `_materializar_uma`; depois verifica se precisa desfazer a validação recusada. (`app/chamados/services/validacao.py:461`; `app/chamados/services/validacao.py:463`; `app/chamados/services/validacao.py:464`) |
| Trabalho sobre irmãs | **A cada item**, chama a materialização da disputa: consulta todas as irmãs respondidas, validadas e não materializadas; tenta cada uma e verifica fechamento da disputa. (`app/chamados/services/validacao.py:469`; `app/chamados/services/materializacao.py:1369`; `app/chamados/services/materializacao.py:1375`; `app/chamados/services/materializacao.py:1378`) |
| Retorno | Atualiza a pergunta do banco, registra resultado e atualiza novamente a instância mantida pelo lote. (`app/chamados/services/validacao.py:472`; `app/chamados/services/validacao.py:479`; `app/chamados/services/validacao.py:752`) |

O POST recebe referências explícitas; a classificação da **fila global** pertence à contagem/prévia. Não há chamada dessa classificação global no corpo da view do POST. (`app/chamados/views_cobrar.py:348`; `app/chamados/views_cobrar.py:361`; `app/chamados/views_cobrar.py:380`)

No ramo de hora, `_materializar_uma` verifica a data do fato, abre transação, calcula o plantio, busca batida existente, verifica paridade e pode criar a batida. Depois reconcilia destino/clones, verifica lastro e carimba a pergunta pela porta que consulta o C6 e relavra seu veredito. (`app/chamados/services/materializacao.py:1192`; `app/chamados/services/materializacao.py:1220`; `app/chamados/services/materializacao.py:846`; `app/chamados/services/materializacao.py:859`; `app/chamados/services/materializacao.py:875`; `app/chamados/services/materializacao.py:927`; `app/chamados/services/materializacao.py:974`; `app/chamados/services/materializacao.py:1233`; `app/chamados/services/materializacao.py:1277`; `app/chamados/models.py:3139`; `app/chamados/models.py:3150`)

**Multiplicação importante:** quando cria uma batida e precisa ligar sua origem, há um INSERT de `Batida` e depois outro `save()` da mesma batida para preencher `pergunta_origem`. Esse segundo save dispara novamente os receivers que não filtram os campos alterados. (`app/ponto/registro_batida.py:157`; `app/chamados/services/materializacao.py:946`; `app/chamados/services/materializacao.py:948`; `app/ponto/registro_batida.py:254`; `app/ponto/signals.py:34`; `app/ponto/signals.py:131`; `app/ponto/signals.py:206`)

Os receivers relevantes são:

| Objeto salvo | Receiver → chamadas e efeitos |
|---|---|
| `Batida`, em cada save | `_nucleo_post_save_batida` → recomposição de turnos entre D−2 e D+1 → reconstrução das linhas, DELETE do recorte e `bulk_create`. (`app/ponto/signals.py:34`; `app/ponto/signals.py:26`; `app/ponto/nucleo.py:47`; `app/ponto/nucleo.py:58`; `app/ponto/nucleo.py:59`) |
| `Batida`, somente criação | Publicação de tempo real; onboarding pode ser marcado se ainda incompleto e houver login. (`app/ponto/signals.py:39`; `app/ponto/signals.py:42`; `app/ponto/signals.py:45`; `app/colaboradores/signals.py:94`; `app/colaboradores/signals.py:102`) |
| `Batida`, em cada save | `_cartorio_post_save_batida` → julgamento de D−1 e D, salvo guarda de reentrada. (`app/ponto/signals.py:131`; `app/ponto/signals.py:135`; `app/ponto/signals.py:112`; `app/ponto/signals.py:90`; `app/ponto/signals.py:93`) |
| `Batida`, em cada save | Reconciliação dos chamados do colaborador e tentativa de fechar adesão. (`app/ponto/signals.py:206`; `app/ponto/signals.py:215`; `app/ponto/signals.py:234`) |
| `PerguntaDisputa` | Beco em `on_commit` → relê disputa e perguntas, classifica e pode resolver perguntas/fechar disputa. (`app/chamados/signals.py:227`; `app/chamados/signals.py:238`; `app/chamados/services/becos.py:72`; `app/chamados/services/becos.py:29`; `app/chamados/services/becos.py:54`; `app/chamados/services/becos.py:57`) |
| `PerguntaDisputa`, código atual | Outbox síncrono se o save toca campos relevantes → consulta linhas/células → INSERT de evento. (`app/chamados/signals.py:250`; `app/chamados/signals.py:255`; `app/chamados/signals.py:257`; `app/chamados/models.py:3228`; `app/chamados/models.py:3248`) |
| `ChamadoColaborador`, pre-save | Consulta o estado anterior. (`app/chamados/signals.py:178`; `app/chamados/signals.py:184`) |
| `ChamadoColaborador`, post-save | Rebaixa urgência; resolve perguntas se o estado foi gravado como encerrado; julga a célula se houve mudança de estado e não estiver em julgamento. (`app/chamados/signals.py:53`; `app/chamados/signals.py:72`; `app/chamados/signals.py:90`; `app/chamados/signals.py:104`; `app/chamados/signals.py:115`; `app/chamados/signals.py:187`; `app/chamados/signals.py:216`) |
| `ChamadoColaborador`, condicionado | Silêncio de isento ao nascer/voltar a vivo; reconciliação do pai quando um filho deixa de ser vivo. (`app/chamados/signals.py:282`; `app/chamados/signals.py:293`; `app/chamados/signals.py:305`; `app/chamados/signals.py:349`; `app/chamados/signals.py:357`; `app/chamados/signals.py:368`) |
| `DisputaSupervisao` fechada | S88 reconcilia o pai e anexa nota; S144 percorre perguntas não materializadas e carimba as aprovadas; callback retoma regularizações confirmadas do colaborador. (`app/chamados/signals.py:18`; `app/chamados/signals.py:42`; `app/chamados/signals.py:45`; `app/chamados/signals.py:147`; `app/chamados/signals.py:163`; `app/chamados/services/materializacao.py:1418`; `app/chamados/signals.py:137`; `app/chamados/services/regularizacao_ext.py:218`; `app/chamados/services/regularizacao_ext.py:224`) |

Portanto, **no ramo de plantio novo + vínculo de origem**, o código solicita dois recomputes de turnos, quatro julgamentos de célula antes das guardas, duas reconciliações do colaborador e duas verificações de adesão. Agenda ainda dois recálculos mensais: um pela validação e outro pela criação da batida. Isso não significa quatro lavras efetivas: guardas e impressão podem pular trabalho. (`app/ponto/signals.py:26`; `app/ponto/signals.py:112`; `app/ponto/signals.py:135`; `app/ponto/signals.py:215`; `app/ponto/signals.py:234`; `app/chamados/services/validacao.py:446`; `app/ponto/registro_batida.py:160`; `app/ponto/services/cartorio.py:1369`)

O `on_commit` usado aqui registra callbacks que chamam diretamente recálculo/fechamento; essas chamadas não demonstram envio a um job. Tempo efetivamente gasto nesses callbacks nos POSTs históricos: [nao verificado]. (`app/ponto/services/fechamento.py:1807`; `app/ponto/services/fechamento.py:1810`; `app/ponto/services/fechamento.py:1824`; `app/chamados/signals.py:132`; `app/chamados/signals.py:144`)

Dentro de **cada julgamento**:

- `julgar_celula` consulta a célula, aplica limites temporais e chama `julgar_colab` com uma célula; depois consulta A2 e pode gravar `ata.precedencia`. (`app/ponto/services/cartorio.py:1987`; `app/ponto/services/cartorio.py:1993`; `app/ponto/services/cartorio.py:2000`; `app/ponto/services/cartorio.py:1959`; `app/ponto/services/cartorio.py:1960`; `app/ponto/services/cartorio.py:2008`)
- O corpo carrega batidas da janela, monta grade, consulta a primeira batida histórica e carrega chamados dos módulos pertinentes **com suas perguntas, sem filtro temporal ou de estado nesse carregador**. O teste de impressão vem depois. (`app/ponto/services/cartorio.py:1118`; `app/ponto/services/cartorio.py:1134`; `app/ponto/services/cartorio.py:1137`; `app/ponto/services/cartorio.py:1165`; `app/ponto/services/cartorio.py:117`; `app/ponto/services/cartorio.py:118`; `app/ponto/services/cartorio.py:1280`; `app/ponto/services/cartorio.py:1369`)
- Havendo adiados, executa E2 do colaborador, reconcilia chamados e pode fazer segunda passada forçada; emissão também pode provocar releitura. (`app/ponto/services/cartorio.py:962`; `app/ponto/services/cartorio.py:970`; `app/ponto/services/cartorio.py:974`; `app/ponto/services/cartorio.py:984`; `app/ponto/services/cartorio.py:1036`; `app/ponto/services/cartorio.py:1049`)
- Havendo células julgadas, executa cluster do colaborador e fábrica por célula; a fábrica pode alcançar pautas. (`app/ponto/services/cartorio.py:1001`; `app/ponto/services/cartorio.py:1005`; `app/ponto/services/cartorio.py:1008`; `app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:279`)
- Status e rótulo entram no mesmo save da lavra. A consulta adicional a A2 em `fatos_do_status` é condicionada ao veredito `indefinida`; não cabe atribuir uma nova query ou save a todo item apenas pela existência das colunas. (`app/ponto/status_dia.py:177`; `app/ponto/status_dia.py:181`; `app/ponto/services/cartorio.py:1813`; `app/ponto/portas/celula.py:1038`; `app/ponto/portas/celula.py:1072`)

**Total exato de SQL, julgamentos concluídos, lavras e segundos por item em produção: [nao verificado].** Os números históricos escritos em comentários, como “122 ms”, não medem estes lotes. (`app/chamados/services/validacao.py:439`)

**2. Commits do anexo que alcançam arquivos desse caminho** [nao verificado]

A tabela cruza os arquivos declarados no anexo com o caminho acima. **“Tocou arquivo” não comprova “mudou função executada”. Para cada hash abaixo, o acréscimo histórico exato por item/POST permanece [nao verificado] sem diff.** A última coluna identifica o trabalho atual a conferir, sem tratá-lo como acréscimo comprovado daquele hash.

| Hashes do anexo | Arquivo alcançado / trabalho atual |
|---|---|
| `f059f0521` | Validação e cartório; encadeamento lavra → E2 → reconciliação → segunda passada condicionada. (`app/chamados/services/validacao.py:469`; `app/ponto/services/cartorio.py:962`; `app/ponto/services/cartorio.py:984`) |
| `22c2c2890`, `479b3aeeb`, `b3fc3919d`, `4f0476aa5`, `734dafb5a`, `38c502149`, `4150b482e` | Modelos/fio: fechamento consulta perguntas ancoradas, julga vivas, pode reapontar e relavrar perguntas. Custo por chamado afetado e respectivas perguntas. (`app/chamados/models.py:344`; `app/chamados/models.py:390`; `app/chamados/models.py:437`; `app/chamados/models.py:309`; `app/chamados/services/fio.py:909`) |
| `e046540e5`, `c1d9ee7c4`, `eb768cf2c`, `7b76efa7d`, `69ea3f8fe`, `3737d1027` | Cartório/modelos/reconciliador: absolvição, gates, projeção da célula deste julgamento e passadas condicionais. (`app/ponto/services/cartorio.py:959`; `app/ponto/services/cartorio.py:970`; `app/ponto/services/cartorio.py:984`; `app/ponto/portas/celula.py:932`) |
| `2692b5c50`, `587e5f175`, `d8be2e408`, `424ba425b`, `9e053c0e6` | Modelos e portas de pergunta; carimbo consulta C6 e pode relavrar antes do save. Alteração de comentário/assinatura versus execução por hash: [nao verificado]. (`app/chamados/models.py:3103`; `app/chamados/models.py:3139`; `app/chamados/models.py:3150`; `app/chamados/models.py:3152`) |
| `4ea6ffd2b`, `364ceb42e`, `ae0502e59` | Status na lavra; processamento do status e A2 condicionado a `indefinida`, no save combinado. (`app/ponto/services/cartorio.py:1813`; `app/ponto/services/cartorio.py:1817`; `app/ponto/status_dia.py:177`; `app/ponto/portas/celula.py:1072`) |
| `9d34ce228`, `8cea1769a`, `3a1e285aa`, `8b3858097`, `69d8b7d89`, `a531c8d1b`, `90a1249a0`, `5ec1220ad`, `f408ab32a` | Tocam `status_dia.py`, também usado pelo escritor; títulos sobre D4/calendário não bastam para excluir o arquivo. Hunk executado e custo novo: [nao verificado]. (`app/ponto/status_dia.py:113`; `app/ponto/status_dia.py:166`; `app/ponto/services/cartorio.py:1817`) |
| `15cb377f5` | Modelo de célula no caminho. A migration não é chamada pelo POST; efeito de índice/esquema instalado: [nao verificado]. (`app/ponto/services/cartorio.py:1974`; `app/ponto/services/cartorio.py:1987`) |
| `8ec526f23`, `9f0e75426`, `60258b809`, `2204220e4` | Cartório/grade/precedência: consulta ao juiz da folga e impressão/versionamento. (`app/escala/utils.py:1140`; `app/ponto/precedencia.py:194`; `app/ponto/services/cartorio.py:471`; `app/ponto/services/cartorio.py:1369`) |
| `36fbac826`, `f408ab32a`, `fe9736178`, `5ec1220ad`, `399565438`, `389775e00`, `d03410912`, `356a30b62` | Cobertura parcial, dispensa por marco, instante anterior e portões de emissão/lista. Podem alterar quais ramos e lavras acontecem; quantidade real afetada: [nao verificado]. (`app/ponto/services/cartorio.py:370`; `app/ponto/services/cartorio.py:427`; `app/ponto/services/cartorio.py:951`; `app/ponto/services/cartorio.py:1717`; `app/ponto/services/cartorio.py:1801`) |
| `4ef817f17`, `b89f191ff` | Versão de regra/impressão: mudança de impressão pode impedir o pulo e causar rejulgamento. Delta de cada hash: [nao verificado]. (`app/ponto/services/cartorio.py:132`; `app/ponto/services/cartorio.py:471`; `app/ponto/services/cartorio.py:1369`) |
| `59de66664`, `1222ab98c` | Isento na impressão, com horizonte/versionamento. Pode invalidar impressão para os casos elegíveis; incidência no lote: [nao verificado]. (`app/ponto/services/cartorio.py:520`; `app/ponto/services/cartorio.py:1362`; `app/ponto/services/cartorio.py:1369`) |
| `6995671cb` | `cods` próprio do ato, usado para decidir fábrica/cluster. O código atual restringe esses consumidores ao resultado do ato; não demonstra acréscimo universal. (`app/ponto/services/cartorio.py:944`; `app/ponto/services/cartorio.py:997`; `app/ponto/services/cartorio.py:1001`; `app/ponto/services/cartorio.py:1008`) |
| `d0d75b8f4`, `c5c433608`, `4236474ca`, `17e595092`, `399565438` | Fábrica da célula: decisão de perguntar, parcial só lista e encaminhamento de limbo exportado para pauta. (`app/chamados/services/fabrica_da_celula.py:112`; `app/chamados/services/fabrica_da_celula.py:123`; `app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:275`) |
| `40e1bac20`, `9e9590eee`, `d0d75b8f4` | Cluster chamado pelo julgamento; leitura de células/achados e filtro de parcial. (`app/ponto/services/cartorio.py:1005`; `app/ponto/services/cluster_espurio.py:46`; `app/ponto/services/cluster_espurio.py:102`) |
| `387558b71` | `disputa_emissao.py`, alcançado pelos encerramentos e respectivos receivers; mudança específica executada neste ramo: [nao verificado]. (`app/chamados/signals.py:115`; `app/chamados/services/disputa_emissao.py:925`) |
| `72eb11be1`, `d03410912`, `b2fba08fd`, `5ec1220ad`, `fe9736178`, `36fbac826` | Leitor da célula/grade, chamado também pela materialização. (`app/chamados/services/materializacao.py:232`; `app/chamados/services/materializacao.py:281`; `app/escala/services/leitor_celula.py:350`) |
| `b1e3f259e`, `c1c6f565e`, `5ec1220ad`, `36fbac826`, `8ec526f23` | Builders/overlays da grade. (`app/escala/utils.py:1130`; `app/escala/utils.py:1156`; `app/escala/utils.py:1231`; `app/escala/utils.py:2084`) |
| `d117b1201`, `b3bef4032`, `36fbac826`, `f408ab32a`, `fe9736178` | Classificador em `supra_juiz.py`, chamado pelo cartório. (`app/ponto/services/cartorio.py:1237`; `app/ponto/services/cartorio.py:1424`) |
| `f9be08e08`, `de1206907`, `d871bcca8`, `41abcd1f6`, `8cb8a4c55`, `8b3858097`, `36fbac826`, `5ec1220ad`, `b2fba08fd`, `90a1249a0` | `turnos.py` no caminho de encerramento/grade/parcial. Hunk efetivamente alcançado por cada alteração: [nao verificado]. (`app/ponto/services/cartorio.py:1116`; `app/ponto/turnos.py:2127`; `app/ponto/turnos.py:2616`) |
| `03c8d106e`, `5394309c0` | Motor: processamento de intervalos/noturnas por período no recálculo. (`app/ponto/motor_calculo_v2.py:121`; `app/ponto/motor_calculo_v2.py:162`; `app/ponto/motor_calculo_v2.py:173`) |
| `bd9fb40be`, `f9a65d662` | Calculador: dobra/noturnas/pontualidade e intervalos, alcançados por dia da competência recalculada. (`app/ponto/services/fechamento.py:1267`; `app/ponto/services/fechamento.py:1271`; `app/ponto/calculador/alimentacao.py:472`; `app/ponto/calculador/alimentacao.py:527`; `app/ponto/calculador/regras.py:783`; `app/ponto/calculador/regras.py:824`) |
| `eaa414cf7`, `50d1c15ce`, `17e595092` | Pautas, alcançadas condicionalmente pela fábrica; montagem de contexto/link. (`app/chamados/services/fabrica_da_celula.py:279`; `app/pautas/services.py:153`; `app/pautas/services.py:400`) |
| `fc9beecea`, `74123bb1d`, `971374506`, `b621e0c7b`, `794ee000b`, `7f3cc8757`, `68821bbd7`, `01ae542e2`, `1ea81aebc`, `cc62d1683` | `vigia_fio.py` é alcançado condicionalmente pela consulta de competência na costura. Isso **não demonstra execução do censo F5** alterado nesses commits. (`app/chamados/services/fio.py:774`; `app/chamados/services/fio.py:776`) |
| `543d2e524`, `68821bbd7`, `366e07d4f`, `a625b893e`, `1ea81aebc`, `cc62d1683`, `8fef4d216`, `a30153c7f`, `9651ef38e` | Tocam `chamados/juizes.py`, arquivo executado pelo POST. Custo adicional das funções especificamente alteradas: [nao verificado]. (`app/chamados/services/validacao.py:864`; `app/chamados/models.py:3139`) |
| `15136c126` | Toca `fio.py`, mas alteração de proposta/modal não prova 

[truncado em 20000 de 38148 caracteres]

## 11/11 10:19 · sessao 1aa4f63738d0 · O169-FECHADA · verificador · origem=Code

**O delta cura a instância anterior à trava e a reabertura sem trilha do legado. Não identifiquei defeito novo demonstrável nos quatro pontos pedidos.** (`app/ponto/services/cluster_espurio.py:278`; `app/ponto/services/cluster_espurio.py:294`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`)

Da rodada 1, mantendo a numeração:

1. **Escritores/leitores — já regular; preservado.** Continua renascimento → `reabrir` → `_gravar_estado`; a auditoria continua pelo escritor `evento`. Nenhuma escrita direta nova dos três campos. (`app/ponto/services/cluster_espurio.py:284`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/core/observ.py:134`)

2. **Juízes — sem defeito novo; concordo com a triagem.** Relatório e renascimento consultam a mesma casca de tranca. A diferença entre dia contado e competência permanece preexistente. (`app/ponto/services/cluster_espurio.py:251`; `app/chamados/models.py:1009`; `app/chamados/models.py:1043`; `app/chamados/juizes.py:2275`)

3. **Relógio — não curado, mas descartado como defeito deste delta.** Concordo: permanece dentro do caminho preexistente competência → perguntas vivas → janela. (`app/chamados/juizes.py:2286`; `app/chamados/juizes.py:2267`; `app/chamados/juizes.py:577`)

4. **Trava/TOCTOU — curado o achado material, severidade alta.** Chamado e colaborador agora nascem depois da trava C; o contador é reconsultado sob ela. `esperados` continua validando a fotografia inicial; concordo em não exigir atomicidade global das linhas. (`app/ponto/services/cluster_espurio.py:261`; `app/ponto/services/cluster_espurio.py:275`; `app/ponto/services/cluster_espurio.py:276`; `app/ponto/services/cluster_espurio.py:278`; `app/ponto/services/cluster_espurio.py:280`)

5. **Campos do relatório — parcialmente curado, baixa.** Agora há `parado` para saída do universo e recusa. Porém `recusa`, `ativo` e classificação continuam sendo a fotografia anterior à trava; não são atualizados pelo apply. `aplicadas` continua contando sucessos, não todas as escritas possíveis de uma recusa. (`app/ponto/services/cluster_espurio.py:256`; `app/ponto/services/cluster_espurio.py:257`; `app/ponto/services/cluster_espurio.py:281`; `app/ponto/services/cluster_espurio.py:285`; `app/chamados/models.py:1427`; `app/chamados/models.py:1436`)

6. **Testes/mutantes — parcialmente curado, média.** A falha de trilha ganhou cenário e mutante; a correção da releitura após trava continua sem cenário concorrente correspondente nos testes do legado. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:138`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:246`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:167`)

7. **Porta de relavra — curada a afirmação excessiva.** O texto agora distingue transição via `admin` de relavra para `competencia_trancada`; o gate dedicado continua recusando encerrados. Concordo com manter esse ramo PARADO nesta triagem. (`app/ponto/services/cluster_espurio.py:220`; `app/ponto/services/cluster_espurio.py:269`; `app/chamados/models.py:1630`; `app/chamados/models.py:1639`)

8. **Outros materiais — parcialmente curado.**
   - **Alta, curado:** ausência da trilha específica agora provoca rollback. **Média, residual:** falha seletiva de `estado_chamado` continua ignorada pelo núcleo. (`app/ponto/services/cluster_espurio.py:293`; `app/ponto/services/cluster_espurio.py:294`; `app/chamados/models.py:292`; `app/core/observ.py:139`)
   - **Baixa, parcialmente curado:** vazio/todas trancadas evitam criar usuário; existindo `REABRIR`, `get_or_create` ainda ocorre fora do atomic, mesmo que a linha depois recue. (`app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:272`; `app/ponto/services/cluster_espurio.py:275`; `app/core/utils.py:18`)
   - **Proveniência parcialmente curada:** a auditoria passou a declarar RECONCILIAR, coerente com o contador; a docstring ainda equipara esse universo a fechamento por idade. (`app/ponto/services/cluster_espurio.py:93`; `app/ponto/services/cluster_espurio.py:228`; `app/ponto/services/cluster_espurio.py:289`)
   - **Relavra não implementada:** permanece a diferença para o aval integral, aceita como parada nesta triagem. (`app/docs/CORTES.md:435`; `app/ponto/services/cluster_espurio.py:234`; `app/ponto/services/cluster_espurio.py:269`)

Quanto às perguntas 2–5:

- **2 — Quem escreve e lê.** `_gravar_estado` escreve `status_local`, limpa `via_encerramento`/`resolvido_em` e salva; `evento` escreve `LogAuditoria`. Antes da trava, contador e relatório leem estado/via, e `recusa_da_reabertura` pode ler `resolvido_em`. Depois da trava, o apply relê chamado/colaborador e universo. Histórico/vigia consomem `estado_chamado`; o teste consulta a ação do legado. Portanto **há leituras anteriores à trava**, mas a instância usada pela porta foi corrigida. (`app/chamados/models.py:228`; `app/chamados/models.py:248`; `app/chamados/models.py:296`; `app/core/observ.py:134`; `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:253`; `app/chamados/models.py:1342`; `app/ponto/services/cluster_espurio.py:278`; `app/chamados/services/historico.py:44`; `app/chamados/services/vigia_loop.py:114`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:115`)

- **3 — Mesma transação: sim, por linha.** Reabertura e ação `reabrir_legado_cluster_o169` estão dentro do mesmo atomic. `None` levanta `_SemTrilha` dentro dele e é capturado fora; outra exceção propaga. Não há separação de commit demonstrada entre essas duas escritas. Pode persistir reabertura **sem `estado_chamado`**, se somente essa emissão falhar e a emissão do legado funcionar. Recusas também podem gravar outras auditorias sem reabrir, comportamento preexistente. (`app/ponto/services/cluster_espurio.py:275`; `app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:288`; `app/ponto/services/cluster_espurio.py:294`; `app/ponto/services/cluster_espurio.py:295`; `app/chamados/models.py:292`; `app/chamados/models.py:1396`)

- **4 — Teste e mutante mordem, por inspeção.** O patch alcança o import local; a foto inclui estado/via/carimbo/notas/logs. Porém o mutante morre primeiro em `aplicadas == 0`, antes da foto: sua morte isolada não comprova rollback. Falta falha seletiva apenas da ação legado, pois o patch desliga todas as emissões. Mutante plausível sem cenário correspondente: remover a trava explícita anterior à releitura; os casos sequenciais não exercitam a espera que causava o defeito. Sobrevivência executada: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:244`; `app/ponto/services/cluster_espurio.py:276`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:141`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:143`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:145`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:246`)

- **5 — Sem defeito novo demonstrado.** Só `_SemTrilha` é capturada; trancadas pulam antes de qualquer sobrescrita de `parado`; recusas/falhas continuam antes do incremento. Sucesso sai do universo de encerrados; rollback permite tentar novamente. (`app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:282`; `app/ponto/services/cluster_espurio.py:286`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`; `app/ponto/services/cluster_espurio.py:107`; `app/chamados/models.py:248`)

Execução Django e ocorrência desses cenários em produção: **[nao verificado]**.

## 11/11 10:18 · sessao a8715dbb8cc7 · - · - · origem=Ronald (tmux)

- **Curado — alto:** instância e colaborador usados pelo apply agora são carregados após trava C; leitura anterior conserva somente `cid`, usado também para revalidar pertencimento ao universo. O relatório continua sendo fotografia anterior à trava. (`app/ponto/services/cluster_espurio.py:250`; `app/ponto/services/cluster_espurio.py:273`; `app/ponto/services/cluster_espurio.py:276`; `app/ponto/services/cluster_espurio.py:278`; `app/ponto/services/cluster_espurio.py:280`)

- **Curado — alto, para a ação legado:** reabertura, nota, ressuscitação e `reabrir_legado_cluster_o169` ficam dentro do mesmo atomic; `evento=None` lança `_SemTrilha` dentro e só é capturado após sair dele, desfazendo a linha. Falha de commit ou outra exceção propaga; não chega ao incremento. (`app/ponto/services/cluster_espurio.py:275`; `app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:288`; `app/ponto/services/cluster_espurio.py:294`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`; `app/chamados/models.py:1022`; `app/chamados/models.py:1451`)

- **Residual — médio:** `estado_chamado` permanece best-effort com retorno ignorado; falha seletiva nessa ação pode deixar reabertura e ação legado gravadas sem a linha de estado. Não é defeito novo. (`app/chamados/models.py:292`; `app/chamados/models.py:296`; `app/core/observ.py:105`; `app/core/observ.py:133`; `app/core/observ.py:139`)

- **Escritor canônico preservado:** tratamento chama renascimento → `reabrir` → `_gravar_estado`, que escreve `status_local`, limpa `resolvido_em`/via e salva; ambas auditorias usam `evento` → `LogAuditoria.objects.create`. Não há escrita nova direta desses campos. (`app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:288`; `app/chamados/models.py:1019`; `app/chamados/models.py:1442`; `app/chamados/models.py:228`; `app/chamados/models.py:248`; `app/chamados/models.py:249`; `app/chamados/models.py:296`; `app/core/observ.py:134`)

- **Leitores:** contador lê estado/via antes e depois da trava; relatório lê estado/via e executa `recusa_da_reabertura` antes dela; `recusa_da_reabertura` inclui leitura de `resolvido_em` no ramo administrativo. Histórico/vigia leem `estado_chamado`, não a ação legado. (`app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:110`; `app/ponto/services/cluster_espurio.py:253`; `app/ponto/services/cluster_espurio.py:256`; `app/ponto/services/cluster_espurio.py:280`; `app/chamados/models.py:1342`; `app/chamados/services/historico.py:44`; `app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:114`)

- **Sem defeito novo demonstrado de exceção, `parado`, contador ou idempotência:** somente `_SemTrilha` é capturada e ganha motivo explícito; linhas trancadas são puladas, portanto seu `parado` não é sobrescrito; recusas/saída do universo/trilha ausente fazem `continue` antes do incremento. Reabertura bem-sucedida sai do filtro de encerrados; rollback conserva elegibilidade para nova tentativa. (`app/ponto/services/cluster_espurio.py:258`; `app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:280`; `app/ponto/services/cluster_espurio.py:284`; `app/ponto/services/cluster_espurio.py:295`; `app/ponto/services/cluster_espurio.py:298`; `app/ponto/services/cluster_espurio.py:107`; `app/chamados/models.py:1363`)

- **Parcialmente curado — baixo:** vazio/todas trancadas agora evitam criar usuário sistema; havendo linha `REABRIR`, `get_user_sistema` permanece fora do atomic e pode criar usuário mesmo quando a porta recusa ou a trilha causa rollback. (`app/ponto/services/cluster_espurio.py:269`; `app/ponto/services/cluster_espurio.py:271`; `app/ponto/services/cluster_espurio.py:275`; `app/core/utils.py:18`)

## 11/11 10:17 · sessao 44d9bf1f107a · - · - · origem=Ronald (tmux)

- **Curado por inspeção; execução [nao verificado].** O teste compara estado, via, `resolvido_em`, followups e quantidade de auditorias antes/depois; detecta persistência da reabertura quando a trilha retorna `None`. O import local permite o patch alcançar o tratamento e o núcleo. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:75`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:138`; `app/ponto/services/cluster_espurio.py:244`; `app/chamados/models.py:255`)

- **Baixa — mordida do mutante menos específica que o nome.** `reabre_sem_trilha` elimina o `raise`; o teste falha primeiro em `aplicadas == 0`, antes de verificar a foto persistida. O harness aceita qualquer falha de asserção do caso, embora rejeite erros e tenha controle intacto. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:143`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:145`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:246`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:299`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:308`)

- **Média — cobertura parcial da falha de auditoria.** O patch retorna `None` para todas as ações, inclusive `estado_chamado`; falta cenário seletivo em que o núcleo grava a auditoria e somente `reabrir_legado_cluster_o169` falha. A foto atual verifica a contagem total após rollback, mas a execução com auditoria real já inserida não é exercitada nesse caso. (`app/ponto/tests/test_o169_legado_encerrado_por_idade.py:78`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:141`; `app/chamados/models.py:292`; `app/ponto/services/cluster_espurio.py:288`)

- **Média — mutante plausível sem cenário correspondente:** remover a trava explícita anterior à releitura preserva os casos sequenciais existentes, mas desfaz a correção do objeto lido depois da trava. A lista de mutantes não altera essa trava; os testes de apply usam fixtures estáveis e não instrumentam espera ou mudança de situação durante a aquisição. Sobrevivência executada: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:276`; `app/ponto/services/cluster_espurio.py:278`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:100`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:167`; `app/ponto/tests/test_o169_legado_encerrado_por_idade.py:220`)

_3794 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
