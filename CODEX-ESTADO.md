# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 03:47** · publicado em **27/11/2026 03:47** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 03:47 · sessao 501893001208 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

O diff da fix5 está nos dois arquivos autorizados. A compilação e o `git diff --check` passaram. Em ensaios isolados, o selo AST rejeitou a escrita injetada do caso 07 e o teste da fixture rejeitou a omissão de `entrada_em`. A suíte Django e as mordidas em execução ficam **[nao verificado]**. [REDs](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1) [Mutantes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:1) [nao verificado]

## Diff e decisões

```bash
GIT_OPTIONAL_LOCKS=0 git diff -- \
  chamados/tests/test_e4d_matriz_reds.py \
  chamados/tests/e4d_matriz_mutantes.py
```

- O caso 14 e os casos 01/02/42/44/45 ficaram idênticos ao HEAD. `marco_do_estoque` também ficou intacto. Um assert incondicional ali anteciparia a falha dos casos 04/07/08/18 para a ausência da 0057 e esconderia os contratos desta volta. [Fixture de estoque](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:238) [Caso 14](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:644)
- Os REDs 04 e 08 propõem as interfaces `--dir-reversao`/`--reverter` e `--so-trilha-exportada`/`--aval`. O 04 usa um JSON por competência com foto e SHA-256. Essas interfaces e o formato do JSON são decisões do teste, com encaixe em produção **[nao verificado]**. O parser da base declara apenas `--aplicar`. [Comando atual](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/backfill_decisoes_pergunta.py:455) [Reversão](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:459) [Exportada](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:540) [nao verificado]
- Os nomes `pedido_id`, `autoria_desconhecida`, `pergunta_sem_absorcao_encaminhada_admin` e `perguntas_sem_absorcao_na_lista_do_admin` são contratos propostos pelos REDs. O selo lê `PORTAS` e exige uma lápide com entidade, via, arquivo, função e motivo no próprio registro. O scanner cobre escritas com via literal e a observação SQL cobre as rotas exercitadas. A cobertura exaustiva de escritores **[nao verificado]**. [Pedido](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:570) [Autoria](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:628) [Admin](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) [Selo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:305)
- O ensaio isolado revelou um `TypeError` ao desmontar a troca de `__code__` no runner. Corrigi a restauração em `finally` e conferi que a função original volta após o mutante. [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:15)

## Caso → o que exige → mutante → morde?

`NV` significa **[nao verificado] na suíte**. A coluna registra o resultado observado, e os comandos abaixo identificam cada variante. [nao verificado]

| Caso | O que exige | Mutante | Morde? |
|---|---|---|---|
| 04 | Foto e hash próprios. Reverter B primeiro preserva A, o ato preexistente e a vizinha. [RED:459](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:459) | Reverte a outra competência junto | NV |
| 05 | `ABSORCAO` e `VALIDACAO` físicas antes de V/M, no mesmo atomic, pelas duas entradas. [RED:1395](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1395) | Segundo escritor de M no módulo e no alias | NV |
| 06/07 | Carimbo de célula pela exceção com lápide. Backfill preserva a foto e mantém zero ato novo e zero evento. [06:898](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:898) [07:526](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:526) | Reescreve carimbo, injeta escritor AST, fabrica conclusão | AST sim, com lápide sintética em memória. Suíte NV | [nao verificado]
| 08 | Via autorizada insere R e V na trilha. Hashes do TXT e do gravado iguais, zero escrita de coluna e zero evento. [RED:540](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:540) | Escreve uma coluna da exportada | NV |
| 09/10 | Ato físico precede a escrita de V/R. Resposta com texto, M pela exceção com lápide. [09:918](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:918) [10:959](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:959) | Segundo escritor de V/R ou M fora da porta | NV |
| 11/12 | Mesmo pedido preserva as linhas, pedido distinto grava por pergunta. FK protege e autoria histórica desconhecida tem marca explícita. [11:570](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:570) [12:608](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:608) | Descarta vazia, dá chave nova ao retry, volta `SET_NULL` ou omite a marca | NV |
| 18 | Compensador futuro restaurado, horizonte comum e recusa de ato futuro no `registrar`. Controles do I23 incluem `REABERTURA` e `RECUSA` com recuo do relógio. [Restore:701](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:701) [Registrar:766](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:766) | C6 antecipa, I23 zera fixo ou `registrar` aceita futuro | NV |
| 20 | Recusa para só Q, preserva irmãs e distingue dois pedidos na mesma ausência. [RED:1440](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1440) | Ignora empate ou reutiliza pedido da ausência | NV |
| 26 | Preserva R/V/M e DP. Registra Q e a apresenta na lista do admin. [RED:1579](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) | Devolve sem vínculo ou preserva sem encaminhar | NV |
| 27/28 | Releitura sob trava, com dia e célula atuais no fantasma. No espelho, resultado revogado preserva a pergunta, e H4 derrubado ganha `REABERTURA`. M exige lápide. [27:1126](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1126) [28:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167) | Instância antiga, só FK fresca, primeiro espelho, omissão da compensação ou escrita fora da porta | NV |
| 30 | Texto `09:00` em ambas as fontes. Instante do ato na ordem SQL, API, modal e crédito do raio-x. [RED:1617](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1617) | Um leitor por vez usa o instante cru | NV |
| 31/32 | Nascimento da raiz e do chamado pelas portas declaradas. Filha vazia na abertura direta e na emissão. [31:1683](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1683) [32:1717](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1717) | Escrita de mesmo valor fora da porta ou nascimento com V | NV |
| 36/37 | Consulta fresca vê a tranca e preserva a irmã de outra competência. Destranca preserva R e aplica M pela exceção com lápide. [36:1793](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1793) [37:1829](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1829) | Ignora tranca, regrava R ou destranca fora da porta | NV |
| Item 9 | `fato()` explicita `entrada_em` pelo relógio congelado quando o campo existe e preserva o instante dado pelo caller. [RED:377](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:377) | Omite a entrada e deixa o default agir | Sim, com relógio simulado em memória. Suíte NV | [nao verificado]

## Item 10 — conferência do verificador v2

| Caso | Veredito e motivo |
|---|---|
| 09 | **PROCEDENTE.** A rota carimba V sem registrar `VALIDACAO`. O recorte anterior só conferia o ato no primeiro ramo e deixava M sem selo. [Resolvedor:2118](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:2118) [RED:918](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:918) |
| 10 | **PROCEDENTE.** O ramo carimba R e emite `fecho_sem_credor`; esse log não é uma `RESPOSTA` física. A via de M também precisava de selo. [Resolvedor:2192](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:2192) [RED:959](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:959) |
| 11 | **PROCEDENTE.** A assinatura atual não recebe identidade do pedido e o filtro do lote exclui vazias. Uma chamada isolada não prova idempotência. [Porta:1628](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1628) [Filtro:1695](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/disputa_emissao.py:1695) |
| 12 | **PROCEDENTE.** As quatro FKs já eram isoladas no teste. O modelo ainda usa `SET_NULL` e o teste anterior deixava passar autoria histórica nula sem marca. [Projeção:3804](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/models.py:3804) [Trilha:4347](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/models.py:4347) [RED:628](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:628) | [nao verificado]
| 20 | **PROCEDENTE.** A materialização reaproveita a ausência do mesmo tipo. Conferir só a FK da ausência deixa passar uma chave fixa para pedidos distintos. [Materialização:1037](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1037) [RED:1451](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1451) |
| 26 | **PROCEDENTE.** A devolução retorna cedo sem itens e reabre os selecionados pelo caminho atual. Só conferir a foto deixava passar a omissão do registro e da lista. [Materialização:1239](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1239) [RED:1579](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1579) |
| 27 | **PROCEDENTE como lacuna do teste.** O reconciliador relê a pergunta sob trava. A fixture anterior distinguia apenas a FK, enquanto o juiz também usa o dia. [Releitura:1265](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/reconciliador.py:1265) [Juiz:1182](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/reconciliador.py:1182) | [nao verificado]
| 28 | **PROCEDENTE.** `declaracao` fica fora de `SEM_HISTORIA`. O novo controle elegível com H4 chega à etapa que precisa de trava e compensação, e o espelho que muda de resposta exige releitura efetiva. [Seletor:31](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/encerrar_perguntas_com_fato.py:31) [RED:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167) |
| 36 | **PROCEDENTE como lacuna do teste.** `_pv_memo` depende da instância e do instante. A consulta nova isola a regra da tranca. A causa dinâmica da falha da pista antiga fica **[nao verificado]**. [Memo:931](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:931) [RED:1793](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1793) | [nao verificado]

## Comandos para a pista do Code

O runner recebe a variante por `E4D_MUTANTE`. Os comandos usam a configuração CI de PostgreSQL e os seletores dos REDs. [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:547) [Configuração CI](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/config/settings/ci.py:6)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4d_matriz_reds

m() {
  PYTHONDONTWRITEBYTECODE=1 E4D_MUTANTE="$1" python3 manage.py test \
    --settings=config.settings.ci \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D \
    "chamados.tests.test_e4d_matriz_reds.$2"
}

h() { m "$1" "MatrizHistoricoTest.$2"; }
p() { m "$1" "MatrizPortasTest.$2"; }
a() { m "$1" "MatrizAusenciaTest.$2"; }
r() { m "$1" "MatrizRaizELeitoresTest.$2"; }

h 04_reverte_a_outra_competencia test_caso_04_aplica_fotos_separadas_por_competencia
a 05_segundo_escritor_m test_caso_05_absorcao_e_validacao_antes_de_v_m_no_mesmo_atomic
p 06_carimbo_reescrito test_caso_06_celula_absolve_so_com_carimbo
h 07_escritor_ast_fora_da_porta test_caso_07_backfill_celula_absolveu_fica_como_estava
h 07_fabrica_conclusao test_caso_07_backfill_celula_absolveu_fica_como_estava
h 08_regrava_exportada test_caso_08_exportada_veta_apply_comum_e_preserva_txt_e_gravado
p 09_segundo_escritor_v test_caso_09_resolvedor_validar_registra_antes_de_v
p 09_m_fora_da_porta test_caso_09_resolvedor_validar_registra_antes_de_v
p 10_segundo_escritor_r test_caso_10_resolvedor_responder_registra_so_com_texto
p 10_m_fora_da_porta test_caso_10_resolvedor_responder_registra_so_com_texto
h 11_descarta_reabertura_vazia test_caso_11_reabertura_vazia_grava_ato_sem_projetar
h 11_retry_ganha_novo_pedido test_caso_11_reabertura_vazia_grava_ato_sem_projetar
h 12_autor_da_foto_set_null test_caso_12_proteger_cada_referencia_de_autoria
h 12_oculta_autoria_desconhecida test_caso_12_backfill_marca_autoria_desconhecida
h 18_c6_julga_compensacao_cedo test_caso_18_estoque_futuro_compensa_no_horizonte_comum
h 18_i23_zero_fixo test_caso_18_estoque_futuro_compensa_no_horizonte_comum
h 18_registrar_aceita_compensador_futuro test_caso_18_registrar_recusa_compensador_futuro
a 20_ignora_recusa_no_empate test_caso_20_recusa_para_so_q_e_preserva_irma_ja_absorvida
a 20_reusa_pedido_da_ausencia test_caso_20_pedidos_distintos_na_mesma_ausencia
a 26_devolve_sem_vinculo test_caso_26_sem_absorcao_preserva_legado
a 26_preserva_sem_encaminhar test_caso_26_sem_absorcao_preserva_legado
p 27_julga_instancia_antiga test_caso_27_fantasma_trava_rele_julga_e_escreve
p 27_rele_so_a_celula test_caso_27_fantasma_trava_rele_julga_e_escreve
p 27_fantasma_fora_da_porta test_caso_27_fantasma_trava_rele_julga_e_escreve
p 28_via_sem_compensacao test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
p 28_usa_o_primeiro_espelho test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
p 28_via_fora_da_porta test_caso_28_espelho_trava_rele_e_reabre_ato_que_perde_h4
r 30_fila_pelo_carimbo_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_api_pelo_par_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_modal_pelo_par_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 30_raiox_pelo_carimbo_cru test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta
r 31_raiz_fora_da_porta test_caso_31_nascimento_da_raiz_e_do_chamado
r 31_chamado_fora_da_porta test_caso_31_nascimento_da_raiz_e_do_chamado
r 32_raiz_fora_da_porta test_caso_32_nascimento_da_filha_com_rvm_vazios
r 32_filha_nasce_com_v test_caso_32_nascimento_da_filha_com_rvm_vazios
r 36_ignora_tranca test_caso_36_tranca_retirada_da_fila_por_leitura
r 37_destranque_fora_da_porta test_caso_37_destranca_passivo_preserva_resposta
r 37_regrava_r_no_destranque test_caso_37_destranca_passivo_preserva_resposta
m fixture_omite_entrada_congelada FixtureMatrizTest.test_fato_explica_entrada_quando_o_campo_existe
```

## Perguntas de lei

1. **Caso 14.** Para o meio-par histórico de L-130, qual ato prevalece entre `SANEAMENTO` e `REABERTURA`? O teste continua congelado. `opus.md:19` · [RED:644](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:644) [nao verificado]
2. **Caso 18.** O zero para compensadores de restore deve sair no próprio I23 ou em um contador separado? O RED usa I23 e preserva o controle da `REABERTURA` legítima que fica adiantada após recuo do relógio. [I23 atual](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/auditar_invariantes_chamados.py:1122) [Caracterização F5](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319)
3. **Caso 28.** A presença de H4 vigente e de fato novo confirmado sob trava torna elegível uma pergunta com via `declaracao`? O RED preserva o cenário original e também exercita `legado_sem_lastro`, que já entra no seletor. [Seletor:36](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/management/commands/encerrar_perguntas_com_fato.py:36) [RED:1167](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1167)

## 27/11 03:46 · sessao d6cd1e928312 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei o diff nos dois arquivos autorizados. A compilação estática passou, e a comparação com o conteúdo inicial preservou os REDs nomeados e os skips. A suíte e as mordidas em PostgreSQL ficam para o Code. [test_e4c_f5_c4_red.py:492](chamados/tests/test_e4c_f5_c4_red.py:492), [e4c_f5_c4red2_mutantes.py:490](chamados/tests/e4c_f5_c4red2_mutantes.py:490). Execução em PostgreSQL [nao verificado].

1. **Escritores.** O campo declara `db_default=Now()`. O controle lê `statement_timestamp()` antes e depois de cada escritor, exige que `entrada_em` caia no intervalo inclusivo e mantém `entrada_em != MARCO`. M33 grava no marco e M34 grava um instante fixo anterior. A fonte instalada de `django/db/models/functions/datetime.py::Now.as_postgresql` estava indisponível nesta cópia, então a emissão SQL dessa instalação fica [nao verificado]. O comando abaixo imprime a fonte e a linha no ambiente do Code. [chamados/models.py:4406](chamados/models.py:4406), [requirements.txt:13](requirements.txt:13), [test_e4c_f5_c4_red.py:499](chamados/tests/test_e4c_f5_c4_red.py:499), [e4c_f5_c4red2_mutantes.py:623](chamados/tests/e4c_f5_c4red2_mutantes.py:623).
2. **Item 13.** `competencia_fechada` barra `aprovado` e `exportado`. A testemunha da F6 mostra `bloqueada_competencia`. A porta `validar_pergunta` devolve `(False, 'nao_gravou')`, conforme o teste já existente da F6, que também exige a trilha de devolução. O controle mantém o sucesso em FM aberto. Em FM exportado, exige a tupla exata, o motivo `COMPETENCIA_APROVADA` e a preservação das respostas, validações e carimbos do par, das batidas e do TXT. M32 injeta o retorno K6 `bloqueada_competencia_entregue`, com mordida esperada só em `fm='exportado'`. [ponto/janelas.py:87](ponto/janelas.py:87), [materializacao.py:943](chamados/services/materializacao.py:943), [validacao.py:869](chamados/services/validacao.py:869), [validacao.py:243](chamados/services/validacao.py:243), [test_bug_par_volta.py:240](chamados/tests/test_bug_par_volta.py:240), [test_e4c_f5_c4_red.py:902](chamados/tests/test_e4c_f5_c4_red.py:902), [e4c_f5_c4red2_mutantes.py:603](chamados/tests/e4c_f5_c4red2_mutantes.py:603).
3. **M21.** O anexo mostra as duas rodadas no conjunto observado e só o teste-pai no esperado. A cópia confirma que `vistos` é comparado dentro de `subTest`. Essa é a asserção certa, pois trocar `<=` por `<` deixa a linha empatada como `ato` nas duas rodadas. O conjunto esperado agora contém exatamente `rodada=1` e `rodada=2`. [0058_decisao_entrada_do_ato_historico.py:20](chamados/migrations/0058_decisao_entrada_do_ato_historico.py:20), [test_e4c_f5_c4_red.py:1062](chamados/tests/test_e4c_f5_c4_red.py:1062), [e4c_f5_c4red2_mutantes.py:490](chamados/tests/e4c_f5_c4red2_mutantes.py:490).

### Comandos para o Code

```bash
PYTHONDONTWRITEBYTECODE=1 python -c 'import inspect; from django.db.models.functions.datetime import Now; f = Now.as_postgresql; print(inspect.getsourcefile(f), inspect.getsourcelines(f)[1]); print(inspect.getsource(f))'

PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_c4_red

mut=chamados.tests.e4c_f5_c4red2_mutantes
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M21Empate0058.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M32FMExportadoBarra.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M33EntradaNoMarco.test_mordida"
PYTHONDONTWRITEBYTECODE=1 python manage.py test "$mut.M34EntradaFixa.test_mordida"
```

### Diff

```diff
--- a/chamados/tests/test_e4c_f5_c4_red.py
+++ b/chamados/tests/test_e4c_f5_c4_red.py
@@ -500,7 +500,9 @@
                 p = self._historica(respondida_em=RESPOSTA_EM, resposta_colab='07:13')
                 with connection.cursor() as cursor:
-                    cursor.execute('SELECT transaction_timestamp()')
-                    instante_db, = cursor.fetchone()
-                self._escrever(p, escritor)
+                    cursor.execute('SELECT statement_timestamp()')
+                    antes_db, = cursor.fetchone()
+                    self._escrever(p, escritor)
+                    cursor.execute('SELECT statement_timestamp()')
+                    depois_db, = cursor.fetchone()
                 p.refresh_from_db()
                 atos = list(p.decisoes.order_by('ato_em', 'pk'))
@@ -517,5 +519,6 @@
                 self.assertEqual(D.divergencia_ato_x_coluna(p, MARCO), [])
                 self.assertNotEqual(novo.entrada_em, MARCO, 'ato novo entra fora do marco')
-                self.assertEqual(novo.entrada_em, instante_db, 'ato novo entra no instante da transação')
+                self.assertGreaterEqual(novo.entrada_em, antes_db, 'entrada do ato respeita o limite anterior à escrita')
+                self.assertLessEqual(novo.entrada_em, depois_db, 'entrada do ato respeita o limite posterior à escrita')
                 transaction.set_rollback(True)
 
@@ -898,20 +901,36 @@
 
     def test_item13_validacao_livre_com_txt_invalidado(self):
-        """O TXT invalidado sem substituto libera o par em FM aberto e exportado."""
+        """Sem TXT vigente, FM aberto libera o par e FM exportado aciona a F6."""
+        from chamados.catalogo.recusa import traduzir_recusa
+
         txt = self._entregar()
         with freeze_time(ITEM13 + HORA):
             txt.invalidar(motivo='Entrega cancelada na fixture', por=self.admin)
         self.assertFalse(competencia_entregue(self.empresa.pk, self.mes, self.ano))
-        batidas, arquivo_anterior, _ = self._gravado()
+        perguntas = (self.saida, self.volta)
+        campos_do_par = PAR + CARIMBO
+        par_antes = [valores(p, campos_do_par) for p in perguntas]
         for status in ('aberto', 'exportado'):
             with self.subTest(fm=status), transaction.atomic():
                 FechamentoMensal.objects.filter(pk=self.fm.pk).update(status=status)
+                gravado = self._gravado()
                 with freeze_time(ITEM13 + 2 * HORA):
                     r = validacao.validar_pergunta(self.saida.pk, self.admin, confirmar_implausivel=True)
-                self.assertTrue(r['ok'], 'TXT invalidado libera a validação')
-                self._assert_par_gravado()
-                self.assertEqual(self._gravado()[1], arquivo_anterior)
-                self.assertEqual(list(Batida.objects.filter(pk__in=self.chao).order_by('pk').values()), batidas)
-                for p in (self.saida, self.volta):
+                if status == 'aberto':
+                    batidas, arquivo_anterior, _ = gravado
+                    self.assertTrue(r['ok'], 'TXT invalidado libera a validação')
+                    self._assert_par_gravado()
+                    self.assertEqual(self._gravado()[1], arquivo_anterior)
+                    self.assertEqual(list(Batida.objects.filter(pk__in=self.chao).order_by('pk').values()), batidas)
+                else:
+                    self.assertEqual((r['ok'], r['estado']), (False, 'nao_gravou'),
+                                     'FM exportado sem TXT vigente recusa pela F6')
+                    for p in perguntas:
+                        p.refresh_from_db()
+                    self.assertEqual(traduzir_recusa(self.saida.recusa_motivo)[0], 'COMPETENCIA_APROVADA')
+                    self.assertEqual([valores(p, campos_do_par) for p in perguntas], par_antes,
+                                     'F6 preserva respostas, validações e carimbos do par')
+                    self.assertEqual(self._gravado(), gravado, 'F6 preserva batidas e TXT')
+                for p in perguntas:
                     self.assertEqual(D.divergencia_ato_x_coluna(p, None), [])
                 transaction.set_rollback(True)
--- a/chamados/tests/e4c_f5_c4red2_mutantes.py
+++ b/chamados/tests/e4c_f5_c4red2_mutantes.py
@@ -490,5 +490,6 @@
 M21Empate0058 = contraprova(
     'M21Empate0058', red.MarcoDa0058Test, 'test_0058_empate_e_reexecucao_sobre_update_ja_aplicado',
-    '0058 inclui a entrada no instante do marco', recorte_0058_estrito)
+    '0058 inclui a entrada no instante do marco', recorte_0058_estrito,
+    subcasos=(dict(rodada=1), dict(rodada=2)))
 
 M22SoValidacaoNaIrma = contraprova(
@@ -616,3 +617,27 @@
 M32FMExportadoBarra = contraprova(
     'M32FMExportadoBarra', red.Item13PorPKTest, 'test_item13_validacao_livre_com_txt_invalidado',
-    'TXT invalidado libera a validação', fm_exportado_barra_validacao, subcasos=(dict(fm='exportado'),))
+    'FM exportado sem TXT vigente recusa pela F6', fm_exportado_barra_validacao,
+    subcasos=(dict(fm='exportado'),))
+
+
+def entrada_em_fixa(instante):
+    bulk_create = D.objects.bulk_create
+
+    def gravar(linhas, *args, **kwargs):
+        linhas = list(linhas)
+        for linha in linhas:
+            if linha.entrada_origem == D.ATO_NOVO:
+                linha.entrada_em = instante
+        return bulk_create(linhas, *args, **kwargs)
+
+    return patch.object(D.objects, 'bulk_create', new=gravar)
+
+
+M33EntradaNoMarco = contraprova(
+    'M33EntradaNoMarco', red.EstoqueDoParTest, 'test_escritores_gravam_o_novo_ato_e_a_coluna_no_mesmo_instante',
+    'ato novo entra fora do marco', lambda: entrada_em_fixa(red.MARCO),
+    subcasos=tuple(dict(escritor=e) for e in ('carimbar', 'validar', 'responder')))
+M34EntradaFixa = contraprova(
+    'M34EntradaFixa', red.EstoqueDoParTest, 'test_escritores_gravam_o_novo_ato_e_a_coluna_no_mesmo_instante',
+    'entrada do ato respeita o limite anterior à escrita', lambda: entrada_em_fixa(red.MARCO - red.HORA),
+    subcasos=tuple(dict(escritor=e) for e in ('carimbar', 'validar', 'responder')))
```

## 27/11 03:44 · sessao aacf2bb78d5c · - · - · origem=Ronald (tmux)

```text
/bin/bash: line 4: /home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/INTRUSO_COPIA.txt: Read-only file system
/bin/bash: line 4: /home/ronald/saas-hasner/INTRUSO_VIVA.txt: Read-only file system
/bin/bash: line 4: /tmp/INTRUSO_TMP.txt: Read-only file system
```

## 27/11 03:44 · sessao 9dfe567edf08 · - · - · origem=Ronald (tmux)

ok

## 27/11 03:37 · sessao 55eda9ce28c0 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei a c4red5 nos dois arquivos de teste. O diff passa em `git diff --check`, os dois módulos compilam em memória e as asserções do item 10 são idênticas às do HEAD. O ensaio isolado do arnês aceitou o conjunto exato e rejeitou a falta de um subcaso. [REDs](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:301) · [Arnês](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) [nao verificado]

**PRONTO 9(a–c) [nao verificado].** Com a restrição de acesso ao banco, ficam pendentes o log dos REDs no HEAD, o verde da c1 e os ciclos verde → mordida → verde na pista do Code. [Configuração da pista](/home/ronald/wt-codex-e4c-f5-instante-012038/app/config/settings/ci.py:6) [nao verificado]

| Item | Teste | Mutante | Morde? | [nao verificado]
|---|---|---|---|
| 1 | Foto da recusa dentro das transações do [estoque](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:338) e do [worker](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:801) | [M30 × 2, M27](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:569) | [nao verificado] |
| 2 | PK e ato nos REDs de [fronteira, reprojeção e conversa](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:355). Mensagens integrais na [elegibilidade](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:434). | [M07, M09, M11](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:440) | [nao verificado] |
| 3 | Três escritores sem trava prévia da Empresa. O ato novo confere autor, motivo e instante da transação. [Controle vivo e skips A/B](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:464) | [M26](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:503) | Estacionado pela lei A |
| 4 | Dois PKs com B novo, um PK de estoque e skip da lei 1. [Comparador e leitores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:613) | [M12a, M12b, M13](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:460) | [nao verificado] |
| 5 | Alarme e placar em subcasos próprios. [Asserções](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:673) | [M12a no contador, M12b nas duas saídas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:460) | [nao verificado] |
| 6 | Identidade da candidata e instante futuro na coluna em memória. [Horizonte](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:233) | [M31](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:597) | [nao verificado] |
| 7 | [TXT invalidado com FM aberto e exportado](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:899) e [devolução via `validar_pergunta` após recusa por paridade](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:986) | [M32](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:616), [M29](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:535) | [nao verificado] |
| 8 | [Conjunto exato por `str(teste)`](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:51). A ordem das travas fica como [pré-condição da fixture](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:790). | [M07 exige dois subcasos, M11 exige sete](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:440). M14 e M25 removidos. | Conjunto exato conferido no ensaio isolado. Mordidas [nao verificado]. | [nao verificado]
| 9 | REDs, c1 e controles antes e depois de cada mutação. [Ciclo do arnês](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:46) | Comandos abaixo | [nao verificado] |
| 10 | Validação futura semeada como estoque restaurado. [Asserções da tarefa](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:301) | [M28](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:517) | [nao verificado] |

**Diff e comandos para a pista do Code**

```bash
git --no-optional-locks --no-pager diff -- \
  chamados/tests/test_e4c_f5_c4_red.py \
  chamados/tests/e4c_f5_c4red2_mutantes.py

export PYTHONDONTWRITEBYTECODE=1
python3 manage.py test --settings=config.settings.ci -v 2 \
  chamados.tests.test_e4c_f5_c4_red
python3 manage.py test --settings=config.settings.ci -v 2 \
  chamados.tests.test_e4c_f5_instante_c1

mordida() {
  python3 manage.py test --settings=config.settings.ci -v 2 \
    "chamados.tests.e4c_f5_c4red2_mutantes.$1.test_mordida"
}

# PRONTO 9(c), mais M32 no controle de TXT invalidado
for mutante in \
  M08QualquerResposta M11Elegibilidade M12aVigia M12bAlarme \
  M13Dry M21Empate0058 M24RelogioAposEspera M27K6LidoAntesDaTrava \
  M28CompensadorCru M29DevolucaoApagaResposta \
  M30EstoqueSemAtomic M30WorkerSemAtomic M31FotoRelida M32FMExportadoBarra
do
  mordida "$mutante"
done
```

Na cura, a mesma função recebe `M01DuasValidacoes`, `M02IgnoraResposta`, `M03FotoDaLavra`, `M04ValidacaoFutura`, `M07ValidacaoNoMarco`, `M09Conversa`, `M10Proveniencia`, `M15SoFM`, `M16TrilhaVazia`, `M17AlteraTXT`, `M18AlteraBatida`, `M19RecusaFutura`, `M20EmissaoDupla`, `M22SoValidacaoNaIrma` e `M23SoCandidata`. `mordida M26FotoViraAto` fica para depois da decisão da lei A. [Labels das contraprovas](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:425)

## 27/11 03:16 · sessao 196363bf4456 · E4D-CONTRATO-ROTA-B · verificador · origem=Code

A volta 2 ainda deixa passar uma implementação sem reversão por competência, sem a via autorizada da exportada e sem lista de exceções com lápides. Também aceita `REABERTURA` onde a regra exige `SANEAMENTO`. `app/chamados/tests/test_e4d_matriz_reds.py:328–400,438–455,655–671,1324–1362,1453–1474` · `app/docs/PROMPTS.md:2873,2950`

**Referências.** `T` = `app/chamados/tests/test_e4d_matriz_reds.py`. `M` = `app/chamados/tests/e4d_matriz_mutantes.py`. `C4` = `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c`. `J` = `app/docs/JUIZES.md`. O cotejo das cópias mostrou diferenças somente em `T` e `M`. A cópia E4D é uma exportação sem `.git`, portanto a identidade do HEAD via Git fica [nao verificado]. Reexecução da pista, execução dos mutantes e dados de produção [nao verificado].

## 1. Procedentes da volta 1

### Os 14 motivos errados, um por um

| Caso | Situação e contraexemplo |
|---|---|
| **04** | **Curado mudando o que o caso pede.** Saiu o símbolo inventado e entrou o comando real, mas agora só se testa aplicação. Um apply que agrupe a prévia por competência e grave tudo sem foto, hash ou reversão por competência passa. `T:328–360` · `app/docs/PROMPTS.md:2873,2950` |
| **06** | **Curado mudando o que o caso pede.** Cobra M e zero ato. Uma escrita direta de M, uma vez por campo, sem entrada na lista e sem lápide passa. `T:655–671,232–236` · `app/docs/PROMPTS.md:2873` |
| **07** | **Curado mudando o que o caso pede.** Agora chama o comando efetivo e observa a ausência de ato, escrita e evento. O comando pode simplesmente ignorar a via e passar com a exceção ainda sem lápide. `T:362–373` · `app/docs/PROMPTS.md:2873` |
| **08** | **Curado mudando o que o caso pede.** A foto e os hashes vêm dos registros reais, mas o teste exige zero DP pela via comum. Uma implementação sem via autorizada passa, embora o caso peça deixar R/V/M, TXT e gravado intactos **e inserir a trilha** sob a autorização específica. `T:185–190,375–400` · `app/docs/PROMPTS.md:2873,2950` |
| **18 · contador** | **Curado quanto ao medidor.** A leitura agora é do I23 real, com controle positivo. Ainda passa uma implementação que isenta toda `REABERTURA` futura e conta `RECUSA` futura. A caracterização F5 exige contar uma `REABERTURA` futura após recuo do relógio. `T:499–503,536–543` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325` |
| **21** | **Curado quanto à fixture.** O carimbo interposto passa pela trava real. O controle de sucesso ainda muda o motivo para `intervalo_volta`. Uma recusa que devolve sempre `ja_validada` para `orfao_14h`, após adquirir a trava, passa. `T:123,650–653,800–815` · `app/chamados/tests/_ato_da_fixture.py:16–27,47–51` |
| **23** | **Curado quanto à fixture e ao alvo do patch.** O alvo agora é o módulo usado pelo import local. Uma correção que recusa sempre `orfao_14h` depois da trava e aceita `orfao_22h` passa. A validação do controle positivo é simulada. `T:123,843–863` · `app/chamados/services/acoes_disputa.py:373–376` |
| **28** | **Não curado.** O controle `legado_sem_lastro` entra. A pergunta principal em `declaracao` segue excluída pelo seletor, antes da releitura e da compensação. Corrigir trava e releitura para todo item admitido ainda deixa o RED cair em `fato_materializado`. Alargar o seletor pode deixar passar uma decisão tomada pelo primeiro espelho, pois o mock responde `True` antes e depois da trava. `T:902–943` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44,66–77` |
| **30 · SQL** | **Curado mudando o que o caso pede.** Foi retirada a consulta artificial, mas a fila real só é conferida por PK. Usar o ato para admitir P e devolver ou ordenar pelas colunas cruas passa. A fixture também mudou de dois pares completos divergentes para `NULL/08:00` contra `10:05/09:00`. `T:1288–1300` · `app/chamados/juizes.py:1863–1868` |
| **30 · modal** | **Curado quanto à fixture.** O modal real chega ao par cru. Ainda passa um leitor que usa a DP somente quando a coluna de instante está nula. Ele falharia no par completo divergente pedido pelo caso. `T:208–220,1288–1293,1310–1311` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/docs/PROMPTS.md:2873` |
| **31** | **Curado mudando o que o caso pede.** Cobre os nascimentos automático e manual, mas perdeu o selo. Um segundo escritor de D ou do chamado que grave os mesmos valores, fora da lista, passa. `T:1324–1351` · `app/docs/PROMPTS.md:2873,2950` |
| **32** | **Curado mudando o que o caso pede.** A cobrança indevida de `tipo_resposta` na lista saiu. A abertura direta prova R/V/M vazios e zero DP nessa porta. A emissão pode contornar `P.abrir` e nascer com V sem derrubar o 32. O selo dos escritores de D e chamado também saiu do 31. `T:1324–1362` · `app/docs/PROMPTS.md:2873,2950` |
| **37** | **Curado mudando o que o caso pede.** Preserva R, observa M e protege a vizinha. Um descarimbo que faça as mesmas alterações fora da exceção listada passa. `T:1453–1474` · `app/docs/PROMPTS.md:2873,2950` |
| **42** | **Curado mudando o que o caso pede.** Saiu `marcar_estoque` e entrou uma resposta real que muda a projeção. Ficaram de fora a migração, a marca, a hora real da entrada nova e a reconstrução do que se sabia em T1. Congelar o marco no valor da primeira resposta passa sem guardar instante de entrada algum. `T:545–564` · `C4:948–979` · `app/docs/PROMPTS.md:2974` |

### Mordida, guardas e caso 20

- **Escritor único e lápides — não curado.** O vínculo entre `INSERT`, ato e PK melhorou. O 05 também compara o savepoint de V/M com o dos atos. A presença de `registrar` na pilha ainda aceita dois escritores chamados por ele, e o observador perde um `UPDATE` que alcance P por outro predicado que não `id =/IN`. No sentido inverso, um projetor único chamado depois de `registrar`, dentro do mesmo atomic, falha pela forma da pilha. O censo estático devolveu um `bulk_create` atribuído a DP, mas descarta os achados sem entidade. Por exemplo, a chamada genérica `Model.save(pergunta.decisoes.model(...))` escapa desse filtro. Nenhum desses mecanismos recupera as lápides retiradas. `T:72–109,222–236,488–493,1123–1134` · `app/core/censo_escritas.py:364–430,469–491` · `app/chamados/models.py:4609`
- **Mutantes — não curado.** No 05, `M` troca o atributo de `materializacao`, enquanto a resposta chama o alias já importado em `disputa_emissao`. No 30/modal, `M` instala uma função de dois parâmetros e `T` chama com `request=req`. O primeiro pode deixar a escrita extra de fora. O segundo produz `TypeError` antes de comparar o par. No 31, fechar D após nascer prova a asserção de estado, mas nada diz sobre um escritor fora da lista que preserve esse estado. Mordida em execução [nao verificado]. `M:89–99,124–143,303–315,328–332` · `app/chamados/services/disputa_emissao.py:12,1596` · `T:215,1075–1078,1119–1120,1327–1334`
- **20 — curado quanto à referência e ao empate.** A docstring nomeia a caracterização invertida, o teste antigo permanece e a nova matriz verifica empate e posterioridade. A identidade do pedido saiu. Reusar uma chave fixa por ausência passa mesmo que dois pedidos distintos devam ser separados. `T:1144–1189` · `app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364–380` · `app/docs/PROMPTS.md:2950`

| Guarda | Situação da lacuna da volta 1 |
|---|---|
| **13** | **Curada.** O snapshot integral de DP pega a alteração de texto ou motivo que escapava ao teste por tipo e autor. `T:181–183,731–748` | [nao verificado]
| **15** | **Parcial.** O controle aberto elimina a recusa incondicional. A entrega é interposta antes da aquisição da trava, então uma releitura feita após a interposição e ainda fora da trava pode passar. `T:239–252,750–776` |
| **19** | **Curada para o contraexemplo anterior.** A automática que sempre recusa agora falha no controle em branco. Uma guarda especializada no motivo do caso principal ainda passa, pois o controle usa outro motivo. `T:123,778–798` |
| **21, 23** | **Parciais.** Fixtures reparadas e caminhos positivos adicionados, com os escapes por motivo descritos acima. `T:650–653,800–815,843–863` |
| **22** | **Curada para o contraexemplo anterior.** O controle do mesmo motivo precisa validar e plantar. Como ele usa outro dia, uma recusa fixa para o dia inicial ainda passa. `T:123,817–841` |
| **24** | **Curada quanto à ordem e ao carimbo de E.** A ordem literal saiu e a foto de E passou a ser exigida. Ainda se exige exatamente o conjunto `{E,Q}`. Uma passagem que preserve E e produza os mesmos efeitos sobre Q, mas reporte só Q na lista, falha. `T:1191–1218` | [nao verificado]
| **27** | **Curada para a instância antiga.** A FK muda antes da trava, o juiz simulado decide por essa FK e o novo mutante conserva a releitura real, mas entrega a instância antiga ao juiz. Ainda se pode usar uma releitura atualizada só da FK e julgar outros campos obsoletos. A lápide de `fantasma` segue sem selo. `T:865–900` · `M:65–69` · `app/chamados/reconciliador.py:1265–1298` |
| **36** | **Parcial.** A irmã viva em outra competência e o pai aberto entraram. A asserção que cai reutiliza a instância e o mesmo `agora`. A chave de `_pv_memo` não contém a tranca. Uma consulta fresca pode dar `False` enquanto a instância anterior devolve `True`. A causa dinâmica exata da ocorrência [nao verificado]. `T:1424–1451` · `app/chamados/juizes.py:889–908,931–952` · `app/ponto/services/fechamento.py:1431–1453` |
| **43** | **Curada para a reescrita anterior.** O retry avança o relógio e acusa `UPDATE` de P, DP ou D mesmo com os mesmos valores. A contagem de eventos ainda deixa passar a troca de um evento por outro com o mesmo total. `T:173–174,1507–1527` |
| **46** | **Curada para a ata de fixture.** Depois do plantio, a ata recarregada precisa trazer 480 realizados. Plantar sem relavrar deixa a ata inicial em zero e falha. A mordida do mutante que corta o signal [nao verificado]. `T:1545–1570` · `M:79–85` · `app/ponto/signals.py:85–93,136–140` |

## 2. As 51 ocorrências da pista de 03:06

A lista anexa contém **51 falhas em 29 métodos**, com `errors=0` no placar fornecido. `T` declara 45 métodos numerados. Resultado individual dos 16 métodos sem ocorrência e placar dos mutantes [nao verificado]. A tabela classifica a primeira asserção que caiu, inclusive quando ela só cobre uma parte do caso.

| Caso · ocorrências | A asserção caiu pelo motivo certo? |
|---|---|
| **05 · 1** | **Sim.** Falta `VALIDACAO` física de Q. O caminho carimba V/M e registra `ABSORCAO`. `T:1123–1124` · `app/chamados/services/materializacao.py:1107–1132` |
| **20 · 2** | **Sim.** Tanto no empate quanto em T2, a recusa humana foi aceita, mas Q ficou fora de `recusadas`. Identidade do pedido segue sem teste. `T:1150–1153,1162–1189` | [nao verificado]
| **25 · 1** | **Sim.** M fica de pé apesar da `ABSORCAO` vigente. O seletor atual exige a âncora que essa entrada dispensa. `T:1223–1247` · `app/chamados/services/materializacao.py:1195–1212` |
| **26 · 1** | **Sim, para preservar o legado.** A rejeição muda R/V/M. Uma correção que deixe tudo intacto e nunca encaminhe Q ao admin passa. `T:1254–1272` · `app/chamados/services/materializacao.py:1235–1279` · `app/docs/PROMPTS.md:2873` |
| **01, 02 · 2** | **Sim, pela falta do ato.** O comando calcula a prévia de R/V, mas seu apply chama a porta de colisões e silêncio. As ocorrências ainda não chegam ao zero evento, e ambas aceitam ato novo com proveniência errada. `T:264–295` · `app/chamados/management/commands/backfill_decisoes_pergunta.py:486–516,628–631` |
| **03 · 2** | **Sim.** No vigente, a foto oferece retroativamente uma validação. No controle compensado, falta a validação nova de `09:00` após o apply. A segunda ocorrência prova a ausência do apply nesse controle. `T:311–326` · `app/chamados/models.py:4437–4447` |
| **04 · 1** | **Não para a reversão.** Falta `VALIDACAO` no apply. Uma aplicação irreversível pode ficar verde. `T:328–360` | [nao verificado]
| **11 · 1** | **Sim, pelo ato vazio ausente.** A assinatura atual aceita a chamada e a porta não gera linha. Uma porta que insira outra linha a cada repetição passa, pois há uma única chamada. `T:402–416` · `app/chamados/services/disputa_emissao.py:1695–1715` |
| **12 · 4** | **Sim, uma por FK.** `validada_por`, `materializada_por`, `ato_por` e `validada_original_por` estão isoladas e cada exclusão deveria ser protegida. Manter autoria histórica nula sem marca ainda passa. `T:418–436` · `app/chamados/models.py:3804–3829,4347–4353` |
| **14 · 1** | **Não.** Faltou a `REABERTURA` que o teste passou a exigir. A implementação correta com `SANEAMENTO`, lei em campo próprio e referência à resposta antiga também falha nessa asserção. A resposta mais nova da fixture só é criada depois do saneamento. `T:438–455` · `app/chamados/services/validacao.py:450–455,472–489` · `app/docs/PROMPTS.md:2873,2950` |
| **16 · 1** | **Sim.** A porta aceita a resposta futura. Se a exceção viesse depois de um `INSERT` bem-sucedido e revertido, o observador ainda o registraria. A ordem da projeção durante uma tentativa revertida fica sem verificação equivalente. `T:53–65,457–470` · `app/chamados/models.py:4598–4623` |
| **17 · 3** | **Sim.** `save` aceita DP futura com e sem `force_insert`, e a terceira falha mostra os dois `INSERT`s. O selo AST vem depois e essa pista não o alcança. `T:472–493` · `app/chamados/models.py:4405–4408` |
| **18 · C6 e modal · 2** | **Sim.** O C6 dá `False` em T1 e o modal não apresenta o par que ainda deveria valer. A tarefa, que devolve uma razão, não é a asserção `False != True`. `T:522–534` · `app/chamados/juizes.py:1499–1521` · `app/chamados/services/tarefa_do_dia.py:389–429` |
| **18 · I23 · 1** | **Depende da identidade do contador.** A pista mede a inclusão do compensador futuro pelo I23 real. O I23 vigente foi definido para incluí-lo. O brief exige `ato_futuro=0` no restore, mas atribuir esse contrato novo ao I23 [nao verificado]. Isentar toda `REABERTURA` passa no novo teste e quebra a caracterização F5. `T:536–543` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325` |
| **42 · 1** | **Sim, pelo marco.** O leitor troca `10:00` por `10:30` depois da resposta retrodatada. A asserção de lavra vem depois e a ocorrência não prova entrada ou migração. `T:545–564` · `app/chamados/veredito_lavrado.py:197–209` |
| **44 · 1** | **Não pelo retry.** A primeira aplicação já deixou zero DP. A segunda nem foi alcançada. O teste repete o comando que recalcula o acervo e pode passar com uma porta que duplique o mesmo payload quando o recebe novamente. `T:199–200,566–587` | [nao verificado]
| **45 · autor, lastro, texto novo e FK nula · 4** | **Sim.** Erros de contagem A/B, coerentes com o vigia atual de presença. Um vigia que só compare diferenças observadas de autor e texto ainda passa sem detectar uma diferença exclusivamente de instante. `T:593–632` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` |
| **45 · texto de estoque, absorvida histórica e quatro reconciliadas · 6** | **Interface C4.** As seis comparam `None` à chave `estoque` exigida. Elas não demonstram o valor histórico medido. Devolver zeros fixos na chave elimina cinco ocorrências e ainda perde a divergência do texto histórico. Alimentar um PK de cada vez também deixa escapar erros de precedência em acervo misto. `T:613–640` · `app/chamados/services/vigia_ato_x_coluna.py:21–25` · `C4:559–572,603–607` |
| **09, 10, 29 · 3** | **Sim.** Faltam `VALIDACAO`, `RESPOSTA` e `REABERTURA` físicas, respectivamente. As asserções posteriores de autoria, via e ordem ainda ficam atrás desses REDs. `T:683–700,712–723,957–964` · `app/chamados/services/disputa_emissao.py:2112–2122,2189–2202` · `app/chamados/services/relavra_via.py:230–261` |
| **28 · 1** | **Não para a trava ou compensação.** A pergunta ficou em `declaracao` porque nem entrou no seletor. A exigência de elegibilidade pode justificar um RED próprio. `T:906–943` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44` |
| **39, 40, 41 · 4** | **Sim.** Caem pela ordem física de V, `recusa_motivo` e R, esta última nos dois retornos de plantio recusado. Um segundo escritor de `validada_por` ou `resposta_colab` em ramo não observado ainda escapa, e `RESULTADO` fica depois do RED do 41. `T:974–1035` · `app/chamados/services/validacao.py:164–178,995–1000` · `app/chamados/services/disputa_emissao.py:1559–1567` |
| **30 · SQL, API, modal, raio-x · 4** | **Sim para os observáveis exercitados.** SQL perde o PK, API e modal expõem a coluna e raio-x põe o chamado no grupo pendente. Só API e modal comparam o par exibido. SQL pode conservar a ordenação crua, e raio-x pode creditar por qualquer resposta histórica, inclusive uma já compensada. `T:1298–1314` · `app/chamados/juizes.py:1863–1868` · `app/api/views.py:1161–1165` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/chamados/services/raiox.py:135–158` |
| **34, 38 · 2** | **Sim.** `SILENCIO` entra depois de M. A verificação da cascata do pai está depois da primeira asserção. Um pai fechado por `UPDATE` direto ainda satisfaria a checagem de status. `T:1364–1381,1476–1505` · `app/chamados/models.py:2184–2207` · `app/chamados/signals.py:20–46` |
| **35 · 1** | **Sim.** O PK eleito difere do esperado. A vizinha de outro chamado e o empate já estão no cenário. Como o primeiro assert cai, as preservações posteriores ficam [nao verificado]. `T:1383–1418` · `app/chamados/services/disputa_emissao.py:539–542,582–605` |
| **36 · 1** | **Inconclusiva como RED da regra.** A ocorrência é no C6 após trancar, mas o teste admite o contraexemplo do memo descrito acima. A pista não mostra o resultado em P recarregada em nova instância. `T:1434–1447` · `app/chamados/juizes.py:889–908,931–952` |

## 3. Fix4 e o cartão

O **fix4 em si** preserva o que 18 e 30 pedem. O 18 passou a ligar a disputa e a pergunta ao cartão que o montador real usa. O 30 já tinha esses vínculos. `contexto_parcial_fio` monta chamados, disputa e permissões que o template consulta. A mudança de regra do 30 veio no recorte do teste, que injeta `NULL/08:00` e tira a asserção C6. `T:208–220,505–515,1283–1314` · `app/chamados/services/fio.py:251–254,305–362,573–603` · `app/templates/chamados/partials/modal_fio.html:123,157–160`

**Cartão ausente não passa em nenhum dos dois casos.** O `partition` produz `''`, mas a pergunta tem texto e `assertIn` falha antes que o helper devolva `(None, None)`. A lacuna é **dentro do cartão**. Uma irmã usa o mesmo texto, o helper pega a primeira `Resposta:` e não identifica a pergunta pelo PK. Em T2 do 18, a pergunta alvo pode sumir e uma irmã muda de mesmo título satisfazer o teste. Em T1 ou no 30, a resposta esperada pode vir da irmã errada. `T:123–127,216–220,532–534,1310–1311` · `app/templates/chamados/partials/modal_fio.html:157–160,217–220`

O 18 também perdeu a prova de que os três leitores enxergam o **mesmo par**. O C6 compara só um booleano e a tarefa só a razão. Trocar `08:00` por `09:00` ainda diverge da luz `19:00` e pode conservar o booleano, enquanto a tarefa pode manter `aguarda_validacao` com ação ou dono errado. `T:507–509,522–534` · `app/chamados/juizes.py:1428–1454,1521–1533` · `app/chamados/services/tarefa_do_dia.py:165–170` · `J:81–95`

## 4. Conflitos com c4red4

| Recorte da volta 1 | Situação na volta 2 |
|---|---|
| **01/02/44 · entrada e evento** | **Curado mudando o que os casos cobrem.** O backfill pelo comando não se opõe ao evento da resposta pública. As asserções de origem e entrada inicial sumiram. Gravar `entrada_em=ato_em` no primeiro apply e conservá-lo no retry passa em E4D. C4 exige `ESTOQUE/MARCO` na foto histórica e `ATO_NOVO` com um evento para a resposta pública. `T:264–295,566–587` · `C4:478–504` |
| **02/04/08/44 · admissibilidade** | **Curado mudando o que os casos cobrem.** 02 ficou antes do marco. 04 usa resposta vigente correspondente, sem a divergência que C4 manda reprojetar. 08 te

[truncado em 20000 de 31735 caracteres]

## 27/11 03:15 · sessao fc98a769dbc1 · E4C-F5-INSTANTE · verificador · origem=Code

Os 28 registros de `falhas.txt` chegam a asserções de comportamento. Os contraexemplos que reprovam a c4red4 estão na prova de atomicidade, na checagem das mensagens de estoque, no alcance dos três escritores e no arnês. Em particular, o teste de estoque pode obter a foto intacta porque **a transação da fixture** desfez a escrita que a porta deveria ter desfeito. `falhas.txt:1` `falhas.txt:55` `test_e4c_f5_c4_red.py:326` `test_e4c_f5_c4_red.py:332` `e4c_f5_c4red2_mutantes.py:48` [nao verificado]

Referências dos testes da cópia: [REDs c4red4](/home/ronald/codex_ws/48b24c39c6bd6686d0e0f066a77187d4caddd34c/app/chamados/tests/test_e4c_f5_c4_red.py:1), [mutantes](/home/ronald/codex_ws/48b24c39c6bd6686d0e0f066a77187d4caddd34c/app/chamados/tests/e4c_f5_c4red2_mutantes.py:1) e [c1](/home/ronald/codex_ws/48b24c39c6bd6686d0e0f066a77187d4caddd34c/app/chamados/tests/test_e4c_f5_instante_c1.py:1). Os dois blobs de teste conferem com `diff.txt`. O SHA da árvore inteira, a identidade da c1 entre commits e o verde integral da c1 ficam [nao verificado]. O anexo lista as falhas, sem o resumo completo da pista. `diff.txt:1` `falhas.txt:1` `falhas.txt:55`

## 1. Os sete itens do opus 5d

| Item | Estado e contraexemplo |
|---|---|
| **1. Falso zero** | **Fechado** para os três controles históricos e a separação da escrita retroativa. O teste vivo de precedência só fornece A novo e B de estoque. A presença de B novo no alarme e no DRY ficou no skip, embora seja possível produzi-la sem a lei. A razão do skip cita “item 3”; o estacionamento é o item 1. `opus.md:19` `test_e4c_f5_c4_red.py:551` `test_e4c_f5_c4_red.py:559` `test_e4c_f5_c4_red.py:574` `test_e4c_f5_c4_red.py:616` | [nao verificado]
| **2. Nascimento** | **Fechado.** Saiu `b_nascida`. A c1 ainda exige B de estoque quando a pergunta nasce em 13/11 com instante de resposta anterior ao marco de 01/11. Reintroduzir “nasceu após o marco ⇒ novo” volta a quebrar esse caso. `test_e4c_f5_c4_red.py:579` `test_e4c_f5_c4_red.py:592` `test_e4c_f5_instante_c1.py:49` `test_e4c_f5_instante_c1.py:83` |
| **3. Concorrência** | **Aberto na prova de “não grava”.** O mesmo worker espera no PostgreSQL, segue após o commit e exige a mensagem exata de `ENTREGUE`. A ordem Empresa → Colaborador está no chamador da fixture, sem troca da guarda e sem `lock_timeout`. Entretanto, `assertRaisesRegex` envolve `_registrar_estoque` por fora de seu `atomic`. Uma porta que insira e só depois lance o `ValueError` correto passa com a foto intacta graças ao rollback do chamador. `test_e4c_f5_c4_red.py:650` `test_e4c_f5_c4_red.py:671` `test_e4c_f5_c4_red.py:689` `test_e4c_f5_c4_red.py:737` `app/folha/export.py:556` |
| **4. Resposta futura** | **Fechado.** A comparação usa a razão e as ações da face pública por pergunta, e M05 saiu. O estímulo ainda depende de `foto_da_lavra`. Uma cura que mova a foto para antes da montagem da candidata pode preservar a face pública e quebrar o ponto de observação. `test_e4c_f5_c4_red.py:226` `test_e4c_f5_c4_red.py:276` `app/chamados/services/tarefa_do_dia.py:461` `e4c_f5_c4red2_mutantes.py:430` |
| **5. Carimbo** | **Fechado mudando o que a regra pede.** O teste exige uma única saída e o de competência entregue exige `ENTREGUE`, foto integral persistida e `validada_em` em memória. A exigência nova é que **cada uma das três portas**, já com Empresa travada pela fixture, promova o passivo a estoque na primeira escrita. Q0.2 amarra a linha da coluna que **o carimbo escreve** à mesma transação e ao mesmo instante. O próprio código reserva o ato de estoque a aval específico e rejeita lote misto. A autorização para essa promoção automática pelas três portas fica [nao verificado]. Contraexemplo de cobertura: uma porta que só consiga promovê-lo com a Empresa pré-travada passa nesse teste e pode falhar na chamada normal. O novo ato também pode ter `entrada_em` ou autor incorretos e passar. `test_e4c_f5_c4_red.py:444` `test_e4c_f5_c4_red.py:467` `test_e4c_f5_c4_red.py:478` `app/chamados/models.py:3521` `app/chamados/models.py:4392` `app/chamados/models.py:4761` |
| **6. `suppress` e mensagens** | **Fechado mudando o que a regra pede.** Sai o `suppress`, e o caso de proveniência exige mensagem exata. O teste troca o critério anterior de fidelidade da foto por rejeição obrigatória. Um registrador que descarte o motivo falso e grave a foto canônica satisfaz a fidelidade da L-007, mas falha no teste novo. Além disso, os outros subcasos de elegibilidade continuam aceitando fragmentos como `duplicada`, `reproduz a coluna` e `ENTREGUE`. `opus.md:11` `opus.md:28` `app/docs/LEIS.md:65` `test_e4c_f5_c4_red.py:379` `test_e4c_f5_c4_red.py:409` `test_e4c_f5_c4_red.py:426` `app/chamados/models.py:4803` |
| **7. Arnês** | **Aberto.** Há instâncias novas, controle inicial, mutação, controle no `finally`, e savepoints nos subcasos de M11. M25/M26 têm contextos delimitados e M12/M13 usam patch de atributo. Ainda há mutantes que atingem só a fixture ou cujo veredito termina antes do efeito anunciado. A prova diferencial dos 27 mutantes sobre a cura fica [nao verificado]. `e4c_f5_c4red2_mutantes.py:26` `e4c_f5_c4red2_mutantes.py:200` `e4c_f5_c4red2_mutantes.py:230` `e4c_f5_c4red2_mutantes.py:371` `e4c_f5_c4red2_mutantes.py:395` `test_e4c_f5_c4_red.py:433` |

### Procedentes da c4red3, um a um

| Achado anterior | Estado na c4red4 |
|---|---|
| Par do carimbo | **Fechado.** Os quatro subcasos chegam à recusa nomeada, incluindo só uma validação e resposta divergente sem validação. `test_e4c_f5_c4_red.py:93` `test_e4c_f5_c4_red.py:116` |
| Duas falhas de foto | **Fechado.** A exceção injetada é esperada e o vermelho vem da foto e da leitura que vazaram. `test_e4c_f5_c4_red.py:126` `test_e4c_f5_c4_red.py:137` `falhas.txt:53` |
| Absorção | **Fechado** quanto ao par antes e depois da escrita. Há dois vermelhos por PK e um controle separado para T1 com o relógio movido a T2. Os motivos agora são literais distintos para a recusa da porta e para a reconferência depois do carimbo. `test_e4c_f5_c4_red.py:169` `test_e4c_f5_c4_red.py:188` `test_e4c_f5_c4_red.py:200` `test_e4c_f5_c4_red.py:214` |
| Horizonte | **Fechado.** Cai a decisão pública da validação candidata. A resposta futura passa a controle público, como no item 4. `test_e4c_f5_c4_red.py:259` `test_e4c_f5_c4_red.py:290` `falhas.txt:31` |
| Falso zero e colisão por nascimento | **Fechado** para a colisão. **Aberto** para a prova viva de B novo no alarme e DRY. `test_e4c_f5_instante_c1.py:83` `test_e4c_f5_c4_red.py:559` `test_e4c_f5_c4_red.py:592` |
| Fronteira e classe A | **Fechado** para recusar os quatro payloads e admitir a resposta compensada de outro instante. **Aberto** se “encaminha à reprojeção” significa entregá-la a um consumidor: o teste aceita o texto `reproje` e a foto intacta. A c1 prova a listagem do DRY com outra fixture. `test_e4c_f5_c4_red.py:345` `test_e4c_f5_c4_red.py:369` `test_e4c_f5_c4_red.py:379` `test_e4c_f5_instante_c1.py:142` | [nao verificado]
| Conversa e proveniência | Conversa **fechado**. Proveniência **fechado mudando o que a regra pede**, pela recusa obrigatória descrita no item 6. Os dois motivos são subcasos de uma única linha, de modo que o teste também não demonstra rejeição de lote com uma linha válida antes da inválida. `test_e4c_f5_c4_red.py:393` `test_e4c_f5_c4_red.py:409` | [nao verificado]
| Competência entregue, origem e emissão | **Fechado** para a exigência explícita de `ENTREGUE`, preservação do carimbo e emissão observada. **Aberto** para a entrada e a autoria do ato novo. A linha nova é fatiada nos quatro primeiros campos, e a autoria nem é selecionada na consulta. `test_e4c_f5_c4_red.py:444` `test_e4c_f5_c4_red.py:478` `test_e4c_f5_c4_red.py:504` |
| Colisão c1 × travas | **Fechado.** A c1 chama `registrar` diretamente e exige `ColaboradorNaoTravado`. A c4red4 fornece as duas travas no chamador. A consequência é que a c4red4 prova esse chamador de teste. `test_e4c_f5_instante_c1.py:173` `test_e4c_f5_c4_red.py:650` `app/chamados/models.py:4737` |
| Colisão c1 × recusa íntegra e segunda tentativa | **Fechado** para o conflito anterior. Ambos exigem sucesso de primeira no cenário aberto. Resta a diferença de pré-condições: a c1 fornece só a trava do dono e seu marco de fixture de janeiro faz a resposta de outubro cair em **B novo** pela regra atual. A c4 fornece Empresa e um passivo anterior ao marco. O verde desses dois caminhos sobre a mesma cura fica [nao verificado]. `test_e4c_f5_instante_c1.py:317` `cortes_f2_f9_apoio.py:31` `cortes_f2_f9_apoio.py:40` `cortes_f2_f9_apoio.py:61` `test_e4c_f5_c4_red.py:44` `test_e4c_f5_c4_red.py:462` `app/chamados/models.py:4615` |
| K6 antes da trava | **Fechado** para o defeito de testar duas tentativas diferentes. M27 agora captura K6 antes da espera e devolve a foto antiga à consulta posterior. A ausência de escrita própria da porta continua **aberta** pelo rollback da fixture. `e4c_f5_c4red2_mutantes.py:402` `test_e4c_f5_c4_red.py:742` `app/chamados/models.py:4799` |
| Item 13 | **Aberto** nos dois pontos antigos. O inverso usa TXT invalidado com **FM ainda aberto**. A devolução ainda começa com validação pela fixture e chama o helper diretamente. Um defeito na ligação `validar_pergunta` → devolução passa pelo controle M29. Outro contraexemplo é alterar `aprovado_em` do FM: esse campo fica fora do hash que a foto compara. `test_e4c_f5_c4_red.py:775` `test_e4c_f5_c4_red.py:790` `test_e4c_f5_c4_red.py:837` `test_e4c_f5_c4_red.py:918` `app/chamados/services/validacao.py:835` `app/ponto/portas/regen_exportada.py:37` |
| 0058 e L-130 | **Fechado** para empate, `+1 µs`, reexecução do recorte correto e a REABERTURA marcada da c1. **Aberto** para reparar uma marcação ampla já gravada: uma linha pós-marco previamente sobrescrita com `entrada_em=MARCO` é indistinguível, para a 0058, de uma linha corretamente marcada. `test_e4c_f5_c4_red.py:953` `test_e4c_f5_c4_red.py:969` `test_e4c_f5_instante_c1.py:234` `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16` |

## 2. As 28 falhas do anexo

A numeração segue a ordem de `falhas.txt`. Nos casos `ValueError not raised`, o vermelho é a aceitação indevida pela porta. A asserção posterior de foto intacta ainda está por executar nesses subcasos. `test_e4c_f5_c4_red.py:332` `test_e4c_f5_c4_red.py:451` `falhas.txt:5`

| Nº | Efeito que derrubou a asserção |
|---|---|
| 1–2 | **1** Par adulterado depois do carimbo: irmã ausente das recusadas. **2** Só a resposta diverge: irmã também ausente. `falhas.txt:1` `test_e4c_f5_c4_red.py:171` `test_e4c_f5_c4_red.py:200` |
| 3 | Carimbo com competência entregue deixa de emitir a recusa. A fixture registra o TXT para a competência retornada pelo dia do fato. `falhas.txt:5` `test_e4c_f5_c4_red.py:338` `test_e4c_f5_c4_red.py:451` |
| 4–6 | **4** Carimbar, **5** validar, **6** responder: cada caminho chega com duas linhas e falha na tupla histórica. Em todos os três, o código monta a foto sem marcar `ESTOQUE` e sem fornecer o marco. `falhas.txt:7` `test_e4c_f5_c4_red.py:480` `app/chamados/models.py:3473` `app/chamados/models.py:3526` `app/chamados/services/validacao.py:170` `app/chamados/services/disputa_emissao.py:1522` `app/chamados/services/disputa_emissao.py:1565` |
| 7 | Classe A com ato vigente divergente entra sem a recusa de reprojeção. `falhas.txt:13` `test_e4c_f5_c4_red.py:373` |
| 8–9 | **8** Motivo falsamente atribuído ao carimbo e **9** motivo atribuído ao DP entram sem recusa. A chave da comparação omite `ato_motivo`. É efeito da política de recusa escolhida pelo teste. `falhas.txt:15` `test_e4c_f5_c4_red.py:411` `app/chamados/models.py:4785` |
| 10 | Conversa com dia civil entra sem recusa: a consulta de competência usa `data_evento` preenchido. `falhas.txt:19` `test_e4c_f5_c4_red.py:400` `app/chamados/services/materializacao.py:803` |
| 11–14 | **11** Resposta no marco, **12** validação no marco, **13** resposta depois, **14** validação depois. Os quatro chegam à ausência da recusa depois de o comparador apontar B novo. A ordem dos atos vem do laço; o identificador literal de cada ato foi truncado no anexo. `falhas.txt:21` `falhas.txt:27` `test_e4c_f5_c4_red.py:349` `test_e4c_f5_c4_red.py:364` |
| 15 | Uma linha de resposta e dois eventos da célula. O `save` dispara o outbox e `registrar` também acorda a célula. `falhas.txt:29` `test_e4c_f5_c4_red.py:502` `app/chamados/signals.py:252` `app/chamados/models.py:4755` |
| 16 | Validação futura candidata muda a decisão pública de `aguarda_validacao` para `devolvida_ao_colaborador`. A vista corta a coluna futura, mas delega `_atos_do_carimbo` à candidata; `leitura` anexa esse pendente sem teto. Isso ativa a leitura da recusa anterior pela cauda de respostas que mantém validação. `falhas.txt:31` `test_e4c_f5_c4_red.py:263` `app/chamados/services/tarefa_do_dia.py:160` `app/chamados/services/tarefa_do_dia.py:416` `app/chamados/models.py:4549` |
| 17–20 | **17** PK 37 na materialização, **18** PK 38 na materialização, **19** PK 37 no passivo, **20** PK 38 no passivo. As recusas e a preservação passam antes da asserção que encontra a trilha vazia. `falhas.txt:33` `falhas.txt:39` `test_e4c_f5_c4_red.py:863` `test_e4c_f5_c4_red.py:877` |
| 21–22 | **21** PK 39, validação vetada no cenário da recusa, **22** PK 41, TXT vigente com FM aberto. Falta foto de auditoria após o veto. O nº 21 diz respeito à **validação vetada**; a asserção do instante da `RECUSA` fica mais adiante. `falhas.txt:41` `test_e4c_f5_c4_red.py:829` `test_e4c_f5_c4_red.py:893` `test_e4c_f5_c4_red.py:906` |
| 23–26 | **23** Validação única, **24** duas validações, **25** resposta divergente com validação, **26** resposta divergente sem validação. Todos obtêm `(True, None)`; a guarda vigente compara presença de validação. `falhas.txt:45` `falhas.txt:51` `test_e4c_f5_c4_red.py:93` `app/chamados/models.py:3499` `app/chamados/models.py:4646` |
| 27–28 | **27** Falha na foto da projeção, **28** falha na foto da lavra. Ambas chegam à comparação da instância e da leitura após a exceção esperada. As duas chamadas de foto ficam fora do bloco que desfaz a escrita em memória. `falhas.txt:53` `test_e4c_f5_c4_red.py:126` `test_e4c_f5_c4_red.py:141` `app/chamados/models.py:3473` `app/chamados/models.py:3510` |

**O instante do run é efeito do código.** A primeira data mostrada na tupla truncada é `ato_em=18/10 09:00`, igual dos dois lados. O marco esperado é `entrada_em=20/10 11:00`, no último campo. A linha histórica entra pelo caminho padrão `origem=ato`, com `entrada_em=Now()` do banco. O `freeze_time` controla o relógio Python. A representação `FakeDatetime` no diff não explica a desigualdade. O valor literal completo de `entrada_em` nas três falhas fica [nao verificado]. `falhas.txt:7` `test_e4c_f5_c4_red.py:44` `test_e4c_f5_c4_red.py:69` `test_e4c_f5_c4_red.py:478` `cortes_f2_f9_apoio.py:31` `app/chamados/models.py:4405` `app/chamados/models.py:4495` `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py:16`

## 3. Arnês

- **M14 e M25 mordem a fixture.** M14 inverte duas chamadas escritas no teste. M25 antepõe a trava do dono à entrega da fixture. Um chamador real que adquira as travas em outra ordem fica fora da prova. M27, por sua vez, atinge o K6 atual porque `_conferir_estoque` importa `competencia_do_fato` na hora da chamada. Uma cura que passe a usar um alias já importado pode deixar o patch de M27 sem efeito. `e4c_f5_c4red2_mutantes.py:230` `e4c_f5_c4red2_mutantes.py:371` `e4c_f5_c4red2_mutantes.py:402` `test_e4c_f5_c4_red.py:650` `app/chamados/models.py:4799`
- **M12 não chega ao alarme sob mutação.** O patch também troca o alias do comando, mas a primeira comparação de `contar` derruba o método antes de executar `vigiar_fio_morto`. M13 chega à comparação do quadro. Ambos dependem de um trecho literal de fonte para montar; uma refatoração equivalente pode produzir erro de montagem antes da comparação de negócio. `e4c_f5_c4red2_mutantes.py:200` `e4c_f5_c4red2_mutantes.py:214` `e4c_f5_c4red2_mutantes.py:221` `test_e4c_f5_c4_red.py:605` `test_e4c_f5_c4_red.py:611` `test_e4c_f5_c4_red.py:627`
- **M23 tem `finally`, com risco de reaplicar a adulteração.** A lista guarda o instante lido *depois* da escrita adulterada. Se um savepoint interno já reverteu aquela tentativa, o `finally` pode escrever esse instante de volta sobre a linha que sobreviveu. O rollback do `TestCase` envolve o ensaio inteiro, e o controle posterior começa com outra fixture. Isso não demonstra a restauração da mesma linha. M11 e M26 têm isolamento por subteste; M25/M26 têm desmontagem dos patches sob contexto. `e4c_f5_c4red2_mutantes.py:35` `e4c_f5_c4red2_mutantes.py:56` `e4c_f5_c4red2_mutantes.py:333` `e4c_f5_c4red2_mutantes.py:346` `e4c_f5_c4red2_mutantes.py:371` `e4c_f5_c4red2_mutantes.py:395` `test_e4c_f5_c4_red.py:433` `test_e4c_f5_c4_red.py:463` `app/chamados/services/materializacao.py:1074`
- **O veredito identifica texto de asserção, sem identificar o subcaso ou a gravação.** M07 afeta as duas validações, mas um único vermelho com a mensagem compartilhada basta. M11 aceita qualquer subconjunto dos casos com o mesmo rótulo. M20 conta eventos, sem fixar se a duplicata vem do `post_save` ou de `registrar`. E qualquer RED vermelho na base para no controle inicial, antes de montar o mutante. A pista fornecida não evidencia nenhum ciclo verde → mordida → verde. `e4c_f5_c4red2_mutantes.py:48` `e4c_f5_c4red2_mutantes.py:146` `e4c_f5_c4red2_mutantes.py:289` `test_e4c_f5_c4_red.py:365` `test_e4c_f5_c4_red.py:433` `test_e4c_f5_c4_red.py:504` `falhas.txt:1` [nao verificado]

## 4. O que pode sair do skip

**B novo no alarme e no DRY independe da lei.** Contraexemplo de cobertura: resposta válida sem linha com `respondida_em=MARCO`, ao lado de outra anterior ao marco. O comparador já separa os dois, a c1 já prova B novo no contador e o comando inclui B positivo no alarme. Outra fixture independente pode combinar, no mesmo PK, resposta anterior ao marco e validação no marco sem linhas, cobrando que B novo prevaleça uma vez. Pela inspeção, esses são controles verdes esperados. Falha atual dessa publicação [nao verificado]. O RED independente de **aceitar** o B novo como estoque já está vivo nos quatro casos de fronteira. `test_e4c_f5_c4_red.py:345` `test_e4c_f5_c4_red.py:572` `test_e4c_f5_c4_red.py:616` `test_e4c_f5_instante_c1.py:102` `app/chamados/models.py:4615` `app/chamados/services/vigia_ato_x_coluna.py:24` `app/chamados/management/commands/vigiar_fio_morto.py:213` `app/chamados/management/commands/estoque_ato_x_coluna.py:44`

O que depende da lei é marcar como nova a **escrita** retroativa ou a limpeza sem ato. `b_antiga`, depois da escrita, e `estoque` preservam os mesmos insumos relevantes para o comparador, embora o skip espere `True` e `False`. A data de abertura é da disputa, e a coluna não guarda sua entrada. O texto da linha `lei:` mencionada no enunciado não consta no topo do `RELATO.md` da cópia; o registro externo fica [nao verificado]. `test_e4c_f5_c4_red.py:516` `test_e4c_f5_c4_red.py:583` `test_e4c_f5_c4_red.py:597` `app/chamados/models.py:1921` `app/chamados/models.py:4600` `app/docs/RELATO.md:1`

## 5. Quem escreve e quem lê

| Campos e saídas | Escrita e leitura |
|---|---|
| `respondida_em`, `resposta_colab`, `validada_em`, `validada_por` | Resposta humana, automática e pela trilha escrevem a resposta. Validação e carimbo escrevem a assinatura; nova resposta, devolução, recusa/reabertura e L-130 limpam os pares conforme o ato. Leem o comparador real, `leitura` com passivo, C6, C21, a face SQL de validação e `verbos` pelo dia civil. `app/chamados/services/disputa_emissao.py:1529` `app/chamados/services/disputa_emissao.py:1661` `app/chamados/services/disputa_emissao.py:1759` `app/chamados/services/disputa_emissao.py:1793` `app/chamados/services/validacao.py:171` `app/chamados/services/validacao.py:307` `app/chamados/services/validacao.py:485` `app/chamados/models.py:3474` `app/chamados/models.py:3726` `app/chamados/models.py:4530` `app/chamados/models.py:4589` `app/chamados/juize

[truncado em 20000 de 25432 caracteres]

## 27/11 03:15 · sessao f80556b9c6d3 · - · - · origem=Ronald (tmux)

As 51 falhas anexadas correspondem a 29 métodos. Classifiquei 39 como asserções pertinentes ao contrato ou a uma de suas partes, 11 como falhas por outro motivo ou sem evidência da condição alegada e uma dependente de o I23 ser o contador que a regra pretende mudar. Resultados individuais dos outros 16 métodos e execução dos mutantes [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:263–1570`

- **39 pertinentes.** 01, 02 e 03×2 exercitam criação ou supressão esperada de atos, embora os erros de 01/02 e do controle 03 antecedam a verificação de eventos. 11 demonstra falta da reabertura inicial. 12×4 isola cada FK. 16 e 17×3 demonstram falta de veto à inserção. 18/C6 e 18/modal divergem do horizonte em T1. 42 isola a mudança da lavra em sua leitura de marcos, com a projeção já alterada. 45×4 demonstra divergência de autor, lastro, texto novo e FK nula. Também pertinentes 05, 09, 10, 20×2, 25, 26, 29, 30×4, 34, 35, 38, 39, 40 e 41×2. `app/chamados/tests/test_e4d_matriz_reds.py:264–326,402–436,457–564,589–632,673–723,945–1036,1114–1189,1220–1314,1364–1418,1476–1505`
- **11 falhas por outro motivo ou insuficientes para o contrato alegado.** 04 mede aplicação e deixou reversão/foto/hash por competência de fora. 14 exige `REABERTURA` no lugar de `SANEAMENTO`. 44 falha na primeira chamada e a segunda enumeraria um novo acervo, condição que permite passar com a porta ainda duplicando o mesmo payload. 28 cai no seletor que exclui `declaracao` antes da releitura. 36 reaproveita a mesma instância e o mesmo `agora` antes e depois de trancar. `_pv_memo` pode retornar o `True` anterior mesmo com uma consulta nova funcionando. A causa dinâmica fica [nao verificado]. As seis falhas restantes do 45 param na chave `estoque` ausente, incluindo quatro zeros reconciliados. Elas evidenciam a forma de retorno incompatível com c4. O valor histórico do texto, quando a chave vier, continua por demonstrar. `app/chamados/tests/test_e4d_matriz_reds.py:328–360,438–455,566–587,613–640,903–943,1434–1447` · `app/chamados/management/commands/encerrar_perguntas_com_fato.py:31–44` · `app/chamados/juizes.py:899–952` · `app/chamados/services/vigia_ato_x_coluna.py:21–25` · `e4c_c4_red_48b24c39c.py:603–608`
- **18/I23.** O contador vigente conta explicitamente compensadores adiantados pelo último ato. A falha `1 != 0` mede esse comportamento. Que o I23 seja a interface do `ato_futuro` da E4D [nao verificado]. Isentar toda `REABERTURA` e contar só `RECUSA` passa no teste com seu controle atual. `app/chamados/management/commands/auditar_invariantes_chamados.py:1122–1134` · `app/chamados/tests/test_e4d_matriz_reds.py:495–520,536–543`
- **Modal.** O cartão ausente produz `''`, mas o teste exige o texto não vazio da pergunta antes de aceitar ausência de resposta. O recorte é do cartão inteiro e a busca pega a primeira resposta. Uma pergunta homônima no mesmo cartão pode satisfazer a presença e esconder a falta da pergunta observada em T2. No 30, a pista mostra o bloco real exibindo `08:00` sem data, consistente com a leitura crua. O mutante do modal recebe `request=req` e tem assinatura de dois argumentos, portanto pode morrer com `TypeError` antes da asserção. `app/chamados/tests/test_e4d_matriz_reds.py:123–127,208–220,522–534,1288–1314` · `app/templates/chamados/partials/modal_fio.html:157–160,217–220` · `app/chamados/tests/e4d_matriz_mutantes.py:303–315`
- **Mordida.** Em análise AST pura e sem escrita, o censo atribuiu o `bulk_create` com alias `DP` e deixou `Model.save(pergunta.decisoes.model(...))` sem entidade. O teste filtra fora essa segunda forma. O observador SQL só reconhece o PK em predicados específicos de `id`; uma regravação pelos campos de disputa ou célula pode escapar. A pilha contendo `registrar` também admite dois escritores executados sob a mesma chamada. `app/core/censo_escritas.py:364–430,469–484` · `app/chamados/tests/test_e4d_matriz_reds.py:92–109,222–236,488–493,1129–1137`

## 27/11 03:14 · sessao a22e65db9a41 · - · - · origem=Ronald (tmux)

Censo estático entregue ao agente principal. O registro vivo identifica C6, C7, C19, C20, C21, K3, K6 e K7. A tabela de consulta explicita que validação e coluna `validada_em` têm atribuições diferentes. O código atual grava V diretamente tanto na validação quanto no carimbo chamado pelo resolvedor. A porta `registrar` grava DP e acorda a célula. `app/docs/JUIZES.md:7–15,38–43,81–109` · `app/core/juizes.py:293–313,336–352` · `app/chamados/services/validacao.py:164–178` · `app/chamados/services/disputa_emissao.py:2112–2122` · `app/chamados/models.py:3473–3514,4590–4623`

O selo AST aponta uma escrita de DP em `models.py:4609` e deixa escapar chamadas sem modelo atribuído. A própria fixture demonstra outro caminho físico, `Model.save(instancia, force_insert=True)`. O selo SQL depende do formato do filtro por PK e da presença de `registrar` na pilha. Uma atualização da mesma linha selecionada por outra chave, ou dois escritores chamados por `registrar`, pode escapar. `app/core/censo_escritas.py:364–430,469–491` · `app/chamados/tests/test_e4d_matriz_reds.py:92–109,137–141,222–236,488–493`

Dois mutantes têm defeito verificável por leitura. O do 05 troca um atributo de `materializacao` que a emissão já importou por nome. O do modal 30 recebe dois argumentos, enquanto o teste passou a mandar `request=req`. A mordida em execução fica [nao verificado]. `app/chamados/services/disputa_emissao.py:12,1596` · `app/chamados/tests/e4d_matriz_mutantes.py:125–126,141–143,303–315` · `app/chamados/tests/test_e4d_matriz_reds.py:208–215,1075–1078,1114–1120`

## 27/11 03:12 · sessao d01a7dafcf06 · - · - · origem=Ronald (tmux)

- **Fix4 e modal.** O contexto real restaura seleção, vínculo com a disputa e permissão. O caso 18 passa a usar uma disputa com container, como o 30 já usava. A mudança monta a fixture necessária para ler o modal nos dois casos. O cartão ausente produz `''`, mas a pergunta tem texto e o `assertIn` falha antes da comparação. Ainda há seleção ambígua dentro do cartão. Duas perguntas usam `A que horas?` e o regex pega a primeira resposta, sem conferir o PK. Em T2, a pergunta alvo pode sumir e uma irmã muda com o mesmo título fazer o teste passar. `app/chamados/tests/test_e4d_matriz_reds.py:123–127,208–220,505–515,1286–1289` · `app/chamados/services/fio.py:251–254,305–323,334–362,573–603` · `app/templates/chamados/partials/modal_fio.html:76,123,157–160,217–220`.
- **Mutante do modal.** O fix4 passou `request=req` e o mutante substitui a função por `renderizar(template, contexto)`. Ele quebra com `TypeError` antes de testar o par. Execução do mutante [nao verificado]. `app/chamados/tests/test_e4d_matriz_reds.py:215` · `app/chamados/tests/e4d_matriz_mutantes.py:303–315`.
- **Caso 18.** As três falhas apontam para contrato após o fix4. C6 decide `False` cedo, o modal perde a resposta em T1 e I23 inclui o compensador adiantado por estoque. A cobertura ainda aceita C6 usando texto incorreto, pois 08:00 e 09:00 discordam da luz 19:00 e produzem o mesmo booleano. A tarefa pode devolver a razão certa com a ação errada. I23 pode excluir toda `REABERTURA` e passar, pois o controle positivo é `RECUSA`. Essa cura derruba a caracterização F5 que exige contar `REABERTURA` por recuo do relógio após uma validação nova. A distinção por proveniência permite coexistência. `app/chamados/tests/test_e4d_matriz_reds.py:505–543` · `app/chamados/juizes.py:1428–1454,1521–1533` · `app/chamados/services/tarefa_do_dia.py:165–170` · `app/chamados/management/commands/auditar_invariantes_chamados.py:1123–1134` · `app/chamados/tests/test_e4c_f5_4c1_instante_unico.py:319–325`.
- **Caso 30.** API e modal caem ao exibir valores crus na face real. SQL e raio-x agora exercitam consumidores reais, mas cobram somente elegibilidade e classificação. O SQL pode retornar o PK certo ainda ordenando e entregando o par da coluna. O raio-x pode creditar por qualquer `RESPOSTA` histórica, mesmo superada. O cenário substituiu o par cru válido 10:00/08:00 por `NULL`/08:00 e tirou a asserção C6. Um leitor que só usa o ato quando a coluna de instante é nula passaria. `app/chamados/tests/test_e4d_matriz_reds.py:1288–1314` · `app/chamados/juizes.py:1863–1868` · `app/api/views.py:1163–1165` · `app/templates/chamados/partials/modal_fio.html:217–220` · `app/chamados/services/raiox.py:135–158` · `diff.txt` [nao verificado quanto à numeração absoluta do anexo].
- **Conflitos e lacunas C4.** O C4 anexado concorda com o horizonte da tarefa do 18, mas grava `VALIDACAO` futura como ato novo pelo helper no relógio anterior. A lei A7 recusa fato futuro e o RED16 só exercita `RESPOSTA`. No 14, o RED atual exige `REABERTURA` como a caracterização C1 da mesma revisão, em contradição com a lei de `SANEAMENTO` em choice próprio. No 30, retirar C6 removeu o conflito direto entre asserções com a caracterização da composta. Uma troca global da fonte do texto ainda derruba essa caracterização. `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c:297–314` · `app/chamados/tests/test_e4d_matriz_reds.py:438–470,1298–1314` · `app/chamados/tests/test_e4c_f5_instante_c1.py@48b24c39c:234–248` · `app/chamados/tests/test_e4c_f5_opus_models.py:59–69,77–90` · `app/docs/PROMPTS.md:2873,2950`.
- **Estoque C4.** Os 01/02/04/44 atuais não exigem o instante de entrada e a marca do backfill. Antecipar os carimbos ao marco elimina a oposição à recusa do estoque novo, e retentar o comando é operação distinta de ter duplicata no mesmo payload. O 08 coincide com o veto comum C4 depois de suprimir a via autorizada. O 42 compara marcos e lavra, deixando migração e entrada sem prova, embora C4 prove a fronteira inclusiva e preserve `marco+1µs`. Se “hora da migração” for a execução da 0058, C4 guarda a hora da 0057 e preserva uma linha que já existia quando a 0058 executou. A identidade dessas duas horas [nao verificado]. Acrescentar SELECT direto em DP a `marcos_do_lote` quebra o selo F3. `app/chamados/tests/test_e4d_matriz_reds.py:264–295,328–359,375–399,545–587` · `app/chamados/tests/test_e4c_f5_c4_red.py@48b24c39c:345–376,436–442,478–504,953–978` · `app/chamados/migrations/0058_decisao_entrada_do_ato_historico.py@48b24c39c:9–20` · `app/chamados/tests/test_f3_lavra_foto_sem_decisao.py:312–322` · `app/docs/PROMPTS.md:2974`.

## 27/11 03:04 · sessao 5e9d4a43e848 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

O helper agora renderiza o modal com `contexto_parcial_fio`, um `RequestFactory` e `self.admin`. A leitura fica restrita ao cartão `fio-ticket-<pk>`. O caso 18, único outro consumidor do helper, ganhou o vínculo entre disputa e chamado exigido pelo montador. As asserções continuam iguais. [test_e4d_matriz_reds.py:208](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:208) [test_e4d_matriz_reds.py:495](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:495) [test_e4d_matriz_reds.py:534](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:534)

A abertura do modal usa `modal_fio_colaborador` e `_resp_modal_full`. O re-render após reabrir usa `contexto_parcial_fio`, caminho escolhido para o helper. Ambos chamam `montar_chamados_fio(..., com_explicacao=True)` e renderizam o mesmo template. O montador ancora a pergunta do caso 30 pelo `chamado_destino`. [views.py:869](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:869) [views.py:890](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:890) [views.py:928](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:928) [views.py:1394](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/views.py:1394) [fio.py:117](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/fio.py:117) [fio.py:582](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/fio.py:582)

**RED do caso 30.** Pela leitura do código, a asserção do modal deve receber `('08:00', '·')` e esperar `('09:00', '· 20/10 10:05')`. O contexto real fornece `folha_julga_pontualidade`, e o template ainda imprime o texto e o instante das colunas alteradas pela fixture. A sintaxe e os três mutantes foram checados em memória. Resultado na suite serial [nao verificado]. [fio.py:599](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/fio.py:599) [modal_fio.html:217](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/templates/chamados/partials/modal_fio.html:217) [test_e4d_matriz_reds.py:1293](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1293) [test_e4d_matriz_reds.py:1311](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1311) [nao verificado]

### Diff

```diff
--- a/app/chamados/tests/test_e4d_matriz_reds.py
+++ b/app/chamados/tests/test_e4d_matriz_reds.py
@@ -208,10 +208,13 @@
     def resposta_no_modal(self, ch, p):
-        ch.disputa_obj = p.disputa
-        ch.perguntas_disputa = [p]
+        from chamados.services.fio import contexto_parcial_fio
+
+        req = RequestFactory().get('/')
+        req.user = self.admin
         with timezone.override('UTC'):
-            html = render_to_string('chamados/partials/modal_fio.html', {
-                'chamados': [ch], 'colaborador': self.colab, 'pode_agir_map': {ch.pk: True},
-            })
+            contexto = contexto_parcial_fio(req, self.colab)
+            html = render_to_string('chamados/partials/modal_fio.html', contexto, request=req)
+        inicio_cartao = '<div id="fio-ticket-'
+        html = html.partition(f'{inicio_cartao}{ch.pk}"')[2].partition(inicio_cartao)[0]
         self.assertIn(p.pergunta, html)
         bloco = re.search(r'<strong>Resposta:</strong>\s*([^<]*)\s*<span[^>]+>([^<]*)</span>', html)
         return (bloco[1].strip(), bloco[2].strip()) if bloco else (None, None)
@@ -502,9 +505,10 @@
         marco = self.marco_do_estoque()
         ch = self.chamado()
         cel = self.celula(trabalha=True, veredito='concorde', veredito_via='cartorio',
                           dna={'marcos': {'hf': '19:00'}},
                           ata={'lampadas': [{'tipo': 'S', 'hora': '19:00', 'acesa': True, 'luz': '19:00'}]})
-        p = self.cartao(destino=ch, respondida_em=T0, resposta_colab='08:00',
+        raiz = self.disputa(chamado=ch, motivos=['orfao_14h'])
+        p = self.cartao(disputa=raiz, destino=ch, respondida_em=T0, resposta_colab='08:00',
                         validada_em=T2, validada_por=self.admin, celula=cel)
         self.fato_restaurado(p, DP.RESPOSTA, marco, texto='08:00')
         estoque = self.fato_restaurado(p, DP.VALIDACAO, marco, em=T2, texto='08:00', autor=self.admin)
```

### Suite serial para o Code [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4d-contrato-rota-b-174640 --only "chamados.tests.test_e4d_matriz_reds" --parallel 1
```

### Mutantes

Da raiz da cópia, aplique cada comando separadamente sobre o diff limpo. Na checagem em memória, M1 voltou a perder a chave, M2 leu a resposta do cartão vizinho e M3 perdeu o vínculo da fixture com o chamado. [test_e4d_matriz_reds.py:214](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:214) [test_e4d_matriz_reds.py:216](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:216) [test_e4d_matriz_reds.py:510](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:510)

```bash
f=app/chamados/tests/test_e4d_matriz_reds.py

# M1 — retirar a chave antes de renderizar
sed -i '214a\            del contexto["folha_julga_pontualidade"]' "$f"

# M2 — ler o HTML inteiro do fio
sed -i '216,217d' "$f"

# M3 — desvincular a disputa do caso 18
sed -i '510s/chamado=ch, //' "$f"
```

## 27/11 03:02 · sessao 6aa002de3fc5 · E4C-F5-INSTANTE · construtor · origem=Code

Apliquei o BRIEF c4red4 nos dois arquivos de teste. A conferência estática encontrou 15 REDs ativos, um skip e 27 seletores de mutantes. O diff passou em `git diff --check`, tem zero `except` e zero `suppress` adicionados e deixa o c1 intocado. As falhas no HEAD, o verde do c1 e as mordidas com PostgreSQL seguem **[nao verificado]**. [RED](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1) · [Arnés](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) · [c1](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_instante_c1.py:83) [nao verificado]

1. Estacionei o falso zero com o skip `lei: registro da escrita da coluna -- E4C c4red3 opus5d item 3`. Os casos históricos e a precedência de ato novo no vigia e no DRY ficaram em controles próprios. [Controles e skip](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:551)
2. Removi `b_nascida` e retirei M06 da bateria ativa pela mesma dependência da lei. [Acervo estacionado](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:575) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:421)
3. A concorrência ficou como controle do chamador, na ordem `travar_entrega → trava_na_ordem → registrar`. A fixture observa a espera com `Event` e `pg_locks`, sob prazo monotônico. M27 injeta a leitura de K6 antes da trava. [Fixture](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:650) · [M27](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:402)
4. A resposta futura virou controle da razão e da ação públicas da tarefa. M05 saiu. [Face pública](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:276) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:430)
5. Os escritores exigem uma foto `ESTOQUE` com `entrada_em=MARCO` e um ato novo. O carimbo em competência entregue exige `ValueError` com `ENTREGUE`, foto integral preservada, `validada_em` anterior e zero linhas gravadas. [Carimbo e escritores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:444)
6. A foto após resposta compensada é aplicada diretamente. Para o motivo adulterado, usei a mensagem integral de `_conferir_estoque` para linha que não reproduz a coluna. O HEAD ainda exclui `ato_motivo` da chave comparada. [Testes](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:379) · [Chave e mensagem](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/models.py:4784) [nao verificado]
7. O arnés roda controle, mutação e controle após restaurar, com fixtures independentes. Só conta a falha na asserção nomeada. Os patches são locais ao caso, M23 restaura em `finally` e os cenários de inserção ficam em transações isoladas. Exercitei o arnés com `unittest` sintético, incluindo sobrevivente, asserção errada e restauração. [Arnés](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:26) · [M23](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:326) · [Isolamento por subteste](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:420)

**Comandos para o Code**, a partir de `app/`. O laço roda cada mutante por seu seletor, sobre a cura, com os dois controles. [Configuração de CI](/home/ronald/wt-codex-e4c-f5-instante-012038/app/config/settings/ci.py:6) · [Seletores](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/e4c_f5_c4red2_mutantes.py:421)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_c4_red --settings=config.settings.ci --verbosity=2

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4c_f5_instante_c1 --settings=config.settings.ci --verbosity=2

for classe in $(sed -nE 's/^(M[0-9]+[[:alnum:]]+) = contraprova\($/\1/p' \
  chamados/tests/e4c_f5_c4red2_mutantes.py); do
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    "chamados.tests.e4c_f5_c4red2_mutantes.${classe}.test_mordida" \
    --settings=config.settings.ci --verbosity=2
done
```

## 27/11 02:58 · sessao 99acdd48db17 · RAMO2-B-EM-TURNO-QUE-CRUZA · construtor · origem=Code

**RAMO2-B c2n — três ajustes na cópia**

1. **Pausa na entrada efetiva.** `segmentos_do_motor` agora usa `_ini <= _a <= s`, como `segmentos_do_par`. No RED sobre `045d308c0`, 12x36 e Comercial deram 800 min trabalhados, 720 min de base, pausa 0 e divergência 1. Após a cura, os dois deram **800 − 20 = 780 min trabalhados**, **720 − 20 = 700 min de base**, pausa de 20 min e divergência 0. O teste também morde se o segmento de duração zero for removido, pois o núcleo usa sua ponta para medir a pausa. `app/ponto/calculador/alimentacao.py:143` · `app/ponto/calculador/periodo.py:310` · `app/ponto/calculador/nucleo.py:145–153` · `app/ponto/tests/test_ramo2b_c2h_saida.py:266–284`
2. **Porta da HE.** `_acusa` passa `{id(p): date(2026, 11, 5)}` para o próprio período da fixture. As três asserções existentes passaram no ensaio local. O mutante que omite o mapa reproduziu os três `TypeError`. `app/ponto/tests/test_ramo2b_c2_base_da_he.py:723–750` · `app/folha/porta_export.py:319,347`
3. **Catraca.** O censo de `045d308c0` mediu **9 pareadores, 80 derivações de dia, A1=6, A2=15, B1=77, B2=191 e 3 sítios nomeados**. Registrei a saída de `he_acima_do_teto_por_dia` do B1 e as linhas medidas do motor. Em `unittest` local, o registro anterior teve 5 falhas em 35 testes e o atualizado passou os 35. `test_MORDE_dia_novo_sobe` continua exigindo `SUBIU` para a derivação nova. `app/core/medida_turno_do_par.py:392–452` · `app/core/tests/test_medida_turno_do_par.py:110–111,161,253–350,847–853`

**Escritores e leitores.** O motor grava as pontas e a pausa em `_carimbar_segmento`, o desconto nos dois `calcular_periodo` e o marco da base em `_escrever_base_da_he`. A alimentação publica os pares e o contador. O núcleo usa a ponta de duração zero para medir o vão, o cálculo noturno ignora esse segmento e o DIFF soma o contador. O mapa da jornada da fixture é escrito em `_acusa` e lido pela porta via `id(p)`. `app/ponto/motor_calculo_v2.py:1957,2048–2050,3015,3501` · `app/ponto/calculador/alimentacao.py:143–178,469–475,545–560` · `app/ponto/calculador/nucleo.py:145–153` · `app/ponto/calculador/regras.py:82–86` · `app/ponto/management/commands/diff_calculador.py:323–324` · `app/ponto/tests/test_ramo2b_c2_base_da_he.py:735–737` · `app/folha/porta_export.py:347`

**Quem mais chama `segmentos_do_motor`.** Em produção, `app/ponto/calculador/alimentacao.py:334`, via DIFF em `app/ponto/management/commands/diff_calculador.py:312` e lavratura em `app/ponto/services/fechamento.py:1293–1312`. Nos testes, `app/ponto/tests/test_ramo2b_c2h_saida.py:91,277`, `app/ponto/tests/test_ramo2b_c2_base_da_he.py:462,767` e `app/ponto/tests/test_v3_dobra_split_noturno_porta.py:172`.

`LEI-AKITA origem=app/ponto/motor_calculo_v2.py:2048 testemunha=app/ponto/calculador/alimentacao.py:143 RED=app/ponto/tests/test_ramo2b_c2h_saida.py:266 quem-mais-le=app/ponto/calculador/alimentacao.py:334 juizes novos=0 padrao=fonte unica (app/docs/GLOSSARIO.md:11)`

**Mutantes** — resultados observados em memória com `unittest`. Os primeiros dois falharam em ambos os motores, a omissão do mapa gerou três erros e os dois registros antigos foram rejeitados. `app/ponto/tests/ramo2b_c2n_mutantes.py:14–60`

```bash
export PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci
python3 -B -m ponto.tests.ramo2b_c2n_mutantes ignora_pausa_na_entrada  # rc 1
python3 -B -m ponto.tests.ramo2b_c2n_mutantes apaga_segmento_vazio     # rc 1
python3 -B -m ponto.tests.ramo2b_c2n_mutantes omite_dia_da_jornada    # rc 2
python3 -B -m ponto.tests.ramo2b_c2n_mutantes contagem_anterior      # rc 1
python3 -B -m ponto.tests.ramo2b_c2n_mutantes linha_anterior         # rc 1
```

**Recorte para o Code.** Os ensaios locais usaram `unittest` com dependências Django substituídas em memória. Recorte Django e impacto em produção [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test --settings=config.settings.ci --noinput \
  ponto.tests.test_o280_bateria_da_lei \
  ponto.tests.test_ramo2b_c2l_red_motor \
  ponto.tests.test_ramo2b_c2h_saida \
  ponto.tests.test_ramo2b_c2_base_da_he \
  folha.tests.test_porta_do_export \
  core.tests.test_medida_turno_do_par \
  ponto.tests.test_s5b_split_noturno_tem_um_dono \
  ponto.tests.test_calculador_regras
```

```text
$ GIT_OPTIONAL_LOCKS=0 git -C /home/ronald/wt-codex-ramo2-b-em-turno-que-cruza-221946 status --porcelain
 M app/core/tests/test_medida_turno_do_par.py
 M app/ponto/calculador/alimentacao.py
 M app/ponto/tests/test_ramo2b_c2_base_da_he.py
 M app/ponto/tests/test_ramo2b_c2h_saida.py
?? app/ponto/tests/ramo2b_c2n_mutantes.py
```

## 27/11 02:56 · sessao 96e5d25defc9 · E4D-CONTRATO-ROTA-B · construtor · origem=Code

A fix3 troca de `orfao_14h` para `intervalo_volta` apenas o motivo de `vizinha` no caso 26 e de `outra` no caso 30. As datas foram preservadas. O motivo distingue as chaves no índice `pergunta_unica_por_marco`. Comparei a AST com `774860a13` e confirmei que as asserções e o restante do módulo são idênticos. [Caso 26](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1261) · [Caso 30](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1292) · [Índice](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/migrations/0053_pergunta_unica_por_familia.py:59)

- **26.** A preservação dos campos e a ausência de novos atos independem do motivo trocado. O mesmo dia mantém `vizinha` no recorte da rejeição. Pela leitura da base, espero `FAIL` na comparação dos campos, pois a rejeição ainda seleciona `e` e `q` pelo grupo legado de disputa e carimbo. Execução [nao verificado]. [Asserções](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1267) · [Seleção legada](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/services/materializacao.py:1199)
- **30.** As asserções independem do motivo trocado. `outra` continua pendente no chamado vizinho, e o cartão cuja resposta os leitores comparam mantém o motivo e o dia originais. Pela leitura da base, espero `FAIL` já no subteste SQL, pois o filtro exige `respondida_em` preenchido e a fixture o deixa nulo. Execução [nao verificado]. [Fixture e asserções](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1285) · [Filtro SQL](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/juizes.py:1676)

**Recorte para o Code** [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 bin/suite.sh --dir /home/ronald/wt-codex-e4d-contrato-rota-b-174640 --only "chamados.tests.test_e4d_matriz_reds"
```

**Mutantes existentes**, para o ambiente de testes do Code. A mordida de cada um fica [nao verificado]. [Mutante do 26](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:275) · [Mutantes do 30](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:288) · [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:393) [nao verificado]

```bash
mutante() {
  E4D_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$2" \
    --settings=config.settings_test \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D
}
caso26=chamados.tests.test_e4d_matriz_reds.MatrizAusenciaTest.test_caso_26_sem_absorcao_preserva_legado
caso30=chamados.tests.test_e4d_matriz_reds.MatrizRaizELeitoresTest.test_caso_30_sql_api_modal_e_raiox_leem_a_mesma_resposta

mutante 26_devolve_sem_vinculo "$caso26"
mutante 30_fila_pelo_carimbo_cru "$caso30"
mutante 30_api_pelo_par_cru "$caso30"
mutante 30_modal_pelo_par_cru "$caso30"
mutante 30_raiox_pelo_carimbo_cru "$caso30"
```

## 27/11 02:52 · sessao 6e9234b9033d · CODEX-CONSTROI-MOTOR · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 27/11 02:50 · sessao da26229ad312 · - · - · origem=Ronald (tmux)

Achados do mapa de agendamento e dos portões de deploy. `NENHUM` abaixo significa `TOCADO` pelo inventário atual para um arquivo regular, com lease válida e sem outros motivos de recusa. A resposta de recusa esperada é `PROIBIDO` para o caminho, seguida de `RECUSA rc 3 (inventario rc 3): wt DESCARTADA`. REDs descritos são casos propostos, execução [nao verificado]. `bin/codex_pergunta.sh:162–175,220–233,805–823,830`.

| Caminho | Leitor e efeito | Wrapper hoje | Caso de RED de recusa |
|---|---|---|---|
| `app/config/crons.py` | Define a grade, empresas, argumentos e dependências. `bin/crons.sh:23–40` renderiza e instala o crontab do host. Elos existentes são resolvidos a cada execução em `bin/encadeado.sh:40–67`, sem reinstalar. A mesma fonte fornece o bloco do ensaio em `bin/sombra.sh:346–351`. `app/config/crons.py:281,326–327,1847–1890,1900–1981`. | `#G:p_crons`, `bin/codex_pergunta.sh:167`. | Alterar entrada da grade. RED atual `bin/tests/test_codex_pergunta.sh:417`, `PROIBIDO`, motivo `crons`, rc 3. |
| `app/config/crons_duracao.json` | Define durações de `sombra.sh` e `noturno.py`, podendo mover o minuto derivado ou recusar render. `app/config/crons.py:146–160,245–278,350–352,594–595`. O medidor também lê o JSON anterior e conserva valores sem medição nova. `bin/crons_duracao.py:77–99`. | NENHUM. | Alterar uma duração. Hoje `TOCADO`; c11 deve recusar rc 3. |
| `app/logs/placar.jsonl` | `check --medir` chama o leitor que calcula p95 em sete dias, fator 1,5 e piso de 30 s, regravando o JSON da grade. `bin/crons.sh:19–21`, `bin/crons_duracao.py:25–31,54–76,93–99`, `bin/cron_run.sh:6–13`. | `#G:p_ignorado`, `app/.gitignore:3`, `.gitignore:53–54`, `bin/codex_pergunta.sh:225`. | Acrescentar linha a fixture ignorada. `PROIBIDO`, motivo `ignorado (fora do diff que pousa)`, rc 3. |
| `app/core/management/commands/crons.py` | `render` governa texto instalado, `corrente` governa execução dos elos e `sombra` governa cobertura do ensaio. `app/core/management/commands/crons.py:23,36–60`, `bin/crons.sh:23–40`, `bin/encadeado.sh:40–67`, `bin/sombra.sh:341–386,400–433`, `bin/deploy.sh:151–167`. | NENHUM. | Alterar o arquivo em cada um dos três verbos, em três casos. Hoje `TOCADO`; c11 deve recusar rc 3 em cada caso. |
| `app/core/tests/marca_cron15.py` e `app/core/tests/__init__.py` | Importados pelo render de install. A marca governa se a recusa `CronNaoCabe` da sombra pode conservar a linha que já está no host e também marca exatamente 15 testes na suite. `app/core/management/commands/crons.py:91–119`, `app/core/tests/marca_cron15.py:27,32–65`, `app/chamados/tests/test_contract_crons.py:16`, `bin/crons.sh:23–27,35–40`. | NENHUM. | Alterar prazo ou inicializador, separadamente. Hoje `TOCADO`. Uma regra que aceite toda a pasta de testes de `core` aceita também esses arquivos e precisa de um caso de portão independente. |
| `app/manage.py`, settings SaaS e módulos transitivos, comandos dos jobs e dependências | `manage.py` escolhe settings e despacha os comandos de cerca, migração, `collectstatic`, `crons` e `tenant_command`. O código executado pelos jobs pode mudar o rc e o carimbo que libera a execução do elo seguinte. `app/manage.py:7–18`, `bin/deploy.sh:194–217,296–303,338–339`, `bin/crons.sh:23–27`, `app/config/crons.py:21,1908–1928`, `bin/cron_run.sh:11–25,29–38`, `bin/encadeado.sh:71–104`. | NENHUM para módulos `.py` regulares comuns. Formas de autoload têm `#G:p_autoload`, `bin/codex_pergunta.sh:175`. | Alterar entrypoint, configuração ou command agendado, um por vez. Hoje `TOCADO`; c11 deve recusar rc 3 para os caminhos fora do brief. Os REDs atuais de autoload estão em `bin/tests/test_codex_pergunta.sh:469–472`. |
| `app/gunicorn.conf.py`, `app/gunicorn.core.conf.py` | O selo executa as configurações do disco e julga `max_requests`. Deploy exige verde antes de migrar e o repete após o reload. `bin/selo_reciclagem.sh:34–49`, `bin/deploy.sh:324–339,443–448`. O teste de suite também lê as mesmas configurações e a declaração das 03:30. `app/core/tests/test_contract_reciclagem.py:26–68,70–80`. | NENHUM. | Alterar cada arquivo separadamente. Hoje `TOCADO`; c11 deve recusar rc 3. O mutante de valor já está no teste em `:57–62`. |
| `app/templates/**`, estático, configuração de storage, urlconfs, views, templatetags e dependências | O deploy decide `collectstatic` lendo a existência/mtime do manifest e arquivos fonte. A prova seguinte carrega Django, resolve estáticos, compila cinco templates e importa os dois urlconfs e includes. Falha antes do apply. `bin/deploy.sh:296–303,315–339`, `bin/prova_de_casca.sh:30–47`, `bin/prova_de_casca.py:9–16,28–53,64–99`, `app/config/settings/saas.py:109–112`. | NENHUM para fontes. Manifest `app/staticfiles/staticfiles.json` tem `#G:p_ignorado`, `.gitignore:13`, `bin/codex_pergunta.sh:225`. | Alterar um arquivo de cada fonte e incluir manifest sintético como caso separado. Fontes abrem hoje como `TOCADO`; c11 deve recusar rc 3. Manifest ignorado já deve recusar rc 3. |
| `app/**/*.py` e `app/**/*.html`; arquivos de auth declarados | Marcador de conflito muda a guarda antes de migrar. Alteração em caminho listado de auth afeta a janela, inclusive no reload agendado, pelo diff contra `logs/deploy.stamp`. `bin/arvore_sem_conflito.sh:19–33`, `bin/auth_sitios.txt:13–22`, `bin/janela_auth.sh:39–45,85–115`, `bin/deploy.sh:101,126–149`. `app/core/models.py` consta na lista de auth. `bin/auth_sitios.txt:17`. | NENHUM nos arquivos App, `#G:p_bin/p_fora` na lista host e `#G:p_fora/p_ignorado` no carimbo. `bin/codex_pergunta.sh:168,174,225`; `.gitignore:53`. | Para caminho fora da lista positiva, rc 3. Caso de modelo aceito tem teste de portão com relógio da janela, porque o nome permitido ainda participa da decisão. |
| `app/config/settings/ci.py`, `app/core/runner_quarentena.py`, `app/core/quarentena_lista.py`, `app/core/testes_em_quarentena.txt` e selos Django dos apps | Settings selecionam runner, que poda testes pela lista. Régua e hook de push executam a suite, e a régua deriva `.regua_stamp` da saída. `app/config/settings/ci.py:38–40`, `app/core/runner_quarentena.py:36–37,49–69,72–96`, `app/core/quarentena_lista.py:9,25–29`, `bin/regua.sh:165–179,196–203`, `bin/pre-push.sh:45–73,184–199`. Selos crons em `app/chamados/tests/test_contract_crons.py:13–16,250–282`, `app/core/tests/test_contract_crons_duracao_horario.py:9–24,43–56`. | NENHUM nos arquivos App. `.regua_stamp` tem `#G:p_fora/p_ignorado`, `.gitignore:60`, `bin/codex_pergunta.sh:174,225`. | Alterar runner/lista/setting, um por vez. Hoje `TOCADO`; c11 deve recusar rc 3. Selo de autoridade dentro de pasta elegível requer teste de independência em vez de prometer recusa pelo nome. |
| `app/core/tests/test_ambiente_do_run.py`, runner e entrada da suite | As linhas de ambiente e poda são dados para certificação de pouso, junto com retorno do processo, OOM e contagens. `app/core/tests/test_ambiente_do_run.py:43–60`, `app/core/runner_quarentena.py:86–96`, `bin/suite.sh:547–569`, `bin/pista_certifica.sh:41–60,67–100`. | NENHUM. | Alterar escritor do ambiente ou runner. Rc 3 pela c11 se forem fora do brief. Caso o teste seja elegível, testar a autoridade do certificado separadamente. |
| `app/core/integrador_lote.py`, `app/core/esteira_vigia.py`, `app/core/management/commands/gerar_diagrama.py` | Python da árvore é executado no host pelo integrador e vigia, governa lote e chama deploy/auto-revert. O gerador e o selo textual entram no portão do molde antes de recarregar. `bin/hasner-integrador.timer:5–7`, `bin/hasner-integrador.service:11–13`, `bin/integrador.sh:18–24,51–52`, `app/core/integrador_lote.py:194–200,369–381,638–639,704–753`, `bin/hasner-esteira.timer:6–8`, `bin/hasner-esteira.service:13–17`, `bin/vigia_esteira.sh:11`, `app/core/esteira_vigia.py:1297–1317`, `bin/molde_fatia/cadeia.sh:36–55`, `bin/gerar_diagrama.py:15`. | NENHUM. | Alterar cada arquivo. Hoje `TOCADO`; c11 deve recusar rc 3. |
| `app/docs/LEIS.md`, `app/docs/CORTES.json`, `app/docs/CORTES.md` | Leitores de índice de leis e juiz novo na régua, e de sítios protegidos no push. `bin/regua.sh:138–144`, `bin/pre-push.sh:12–16`, `bin/tests/test_leis_indice.sh:17–39`, `bin/tests/test_juiz_novo_tem_corte.sh:24–44`, `bin/tests/test_lei_protege_sitio.sh:26,52–92,165–203`. | NENHUM. | Alterar cada fonte versionada. Hoje `TOCADO`; c11 deve recusar rc 3. |
| `app/requirements.txt`, `app/Dockerfile`, `app/.dockerignore`; compose e ambiente de host | Build resolve Django, django-tenants e Gunicorn e copia contexto. Efeito por imagem nova depende de rebuild/recreate. O deploy informa que HUP e restart não reconstroem a imagem. O compose do mensageria determina `migrate` de boot e é lido pelo selo. `app/requirements.txt:12–21`, `app/Dockerfile:8,37–44`, `app/.dockerignore:3–13`, `bin/deploy.sh:20–22,213–224`, `docker-compose.mensageria.yml:3,13–20`, `bin/selo_reciclagem.sh:52–75`. Compose SaaS vivo [nao verificado]. | NENHUM nos três arquivos de `app`. `#G:p_compose/p_fora` e `#G:p_env/p_fora` nos nomes do host. `bin/codex_pergunta.sh:172–174`. | Alterar cada insumo do build e cada insumo host separadamente. Os três do `app` aparecem como `TOCADO` hoje, c11 deve recusar rc 3. Host deve recusar hoje. |
| Scripts e artefatos de host `bin/deploy.sh`, `bin/crons.sh`, selos, `/etc/cron.d`, timers, `.sombra_stamp`, `logs/fim/**` | Fronteira física da cópia. `bin/deploy_agendado.sh:49–60,63–81` programa cron de um disparo com diretórios recebidos por argv. Carimbo da sombra lido pelo deploy e pela raia dinheiro. O carimbo de fim é lido pela cadeia. `bin/sombra.sh:82,410–433`, `bin/deploy.sh:163–167`, `bin/trava.sh:40–52`, `bin/encadeado.sh:81–96`. | `#G:p_bin/p_fora` para scripts versionados, `#G:p_fora/p_ignorado` nos carimbos conforme `.gitignore:53,60–61`; `/etc/cron.d` está fora do universo inventariado `bin/codex_pergunta.sh:143–149,202–205`. | Usar equivalentes sintéticos relativos dentro da fixture para os caminhos de host no repo. Arquivos reais externos [nao verificado]. |

`bin/crons.sh:40` devolve `0` depois da pipeline de `crontab -` até quando esta falha, pela sequência `&& echo …; exit 0`. A igualdade do check e a presença da linha no selo são perguntas distintas. O selo de reciclagem confere só o texto `deploy.sh --reload-agendado`, e os minutos 03:30 vêm da declaração e do teste de suite. `bin/crons.sh:24–40`, `bin/selo_reciclagem.sh:70–78`, `app/config/crons.py:326–327`, `app/core/tests/test_contract_reciclagem.py:70–80`. Resultado em host [nao verificado].

O motor aceito é lido pelo cron `vigiar_motor_autoridade` e pode alterar seu resultado de alarme. O comando o importa e calcula o mês, acusa pelo rc 2, e o envelope carimba tanto rc 0 como rc 2. `app/config/crons.py:596`, `app/ponto/management/commands/vigiar_motor_autoridade.py:34–39,54–65,97–108`, `bin/cron_run.sh:19–25`. A alteração de comportamento do motor pode também influenciar duração observada de código que o executa, via `bin/cron_run.sh:11–13` e `bin/crons_duracao.py:54–76`; mudança real de horário por esse caminho [nao verificado].

C inclui ainda `bin/disjuntor.py:2025` além dos sítios do anexo, pois o docstring também referencia `:644`. D exige revisar as asserções de cardinalidade fixa em `bin/tests/test_agente_no_modelo.sh:142,287`, além de `OPUS_CONSTROI_TAGS` e do texto de despacho. Locais atuais `bin/codex_pergunta.sh:644–645,166`, `bin/disjuntor.py:1959–1961,2024–2039,2111`, `CLAUDE.md:403–405`, `bin/tests/test_agente_no_modelo.sh:13,142,287,355`, `bin/tests/test_censo_antes_da_vaga.sh:203`.

## 27/11 02:50 · sessao a7b4cf266d17 · - · - · origem=Ronald (tmux)

**Mapa de schema e portões do deploy** [nao verificado]

| Caminho | Leitor e efeito | Wrapper c10 e RED de c11 |
|---|---|---|
| `app/config/settings/{saas,base}.py` e inicializadores dos pacotes | `app/manage.py:9-12`, `app/config/wsgi.py:14-15`, `app/config/settings/saas.py:10,22-53,83-95`. Escolhe apps shared/tenant, router, backend e os modelos de tenant. `BASE_DIR` também determina que labels a cerca pergunta. `app/core/tests/test_model_sem_migration_pendente.py:24-48`. | NENHUM. Um caso por arquivo de settings e por `__init__.py`; exigir recusa do caminho exato. `bin/codex_pergunta.sh:162-176,223-233`. |
| `app/config/settings/saas.py` + pacote de migrations com outro nome | Anexo `opus.md`, B; `app/core/tests/test_model_sem_migration_pendente.py:24-48` e `bin/deploy.sh:194-207,256,338-340`. Altera a origem do grafo que a cerca e o apply leem. | NENHUM para settings e pacote alternativo. Testar cada arquivo isolado, além da combinação B. Usar mudança inerte de fixture. `bin/codex_pergunta.sh:165,162-176`. |
| `app/<app>/migrations/**`, inclusive `__init__.py` | A mesma cerca, `migrate_schemas --tenant --check` e apply; `bin/deploy.sh:194-207,338-340`. Exemplo de DDL real em `app/colaboradores/migrations/0025_onboarding_db_defaults.py:12-17`. | `#G:p_migr`, `bin/codex_pergunta.sh:165`; REDs existentes `bin/tests/test_codex_pergunta.sh:415-416`. Acrescentar caso isolado de `migrations/__init__.py`. |
| `app/<app>/models.py` existentes e novos, inclusive `api` e `tenants`; pacote `models/` | Registro SaaS e cerca consultam classes, campos e labels. Alteração declarativa sem migration deve produzir drift e impedir publicação, sem DDL no gate. `app/config/settings/saas.py:22-53`; `app/core/tests/test_model_sem_migration_pendente.py:24-48`; `app/tenants/test_model_sem_migration_pendente.py:25-42`; `bin/deploy.sh:194-205`. | `models.py`: NENHUM, inclusive fora do brief e novos; exigir recusa de ambos. `models/`: `#G:p_models_pkg`, `bin/codex_pergunta.sh:166`; RED `bin/tests/test_codex_pergunta.sh:412`. |
| Dependências Python de migration já versionada | `app/colaboradores/models.py` é importado pela migration `app/colaboradores/migrations/0004_colaborador_data_demissao_alter_colaborador_cpf_and_more.py:3,25` e fornece o validador. `app/core/crypto_fields.py` fornece o campo usado pela migration `app/colaboradores/migrations/0032_alter_contabancaria_agencia_and_more.py:3,14-17` e pelo model atual `app/colaboradores/models.py:2`. Mudar a implementação alcança o carregamento de migrations sem editar a migration; efeito de um mutante específico no DDL [nao verificado]. | NENHUM. Recusar `crypto_fields.py`; se o model for nominalmente permitido, preservar esta linha como caminho residual e exigir teste de efeito independente do wrapper. `bin/codex_pergunta.sh:162-176`. |
| `app/tenants/{models,services}.py`, view e command de provisionamento | `app/tenants/management/commands/criar_cliente.py:3,23` e `app/tenants/views.py:19,52` chamam o serviço, que grava `Cliente` em `app/tenants/services.py:1,5-13`. O model habilita criação automática de schema em `app/tenants/models.py:14-21`. Está catalogado como setup de tenant em `app/config/crons.py:1450`. | NENHUM. Casos isolados por arquivo; se o model estiver no brief, o caminho de provisionamento subsiste. `bin/codex_pergunta.sh:162-176`. |
| `app/*/{__init__,apps,signals}.py`, dependências de `ready()` | Inicialização do Django usada pela cerca, migrate e prova; `app/manage.py:9-12`, `bin/prova_de_casca.py:9-16`, `bin/deploy.sh:196,207,339`. Exemplos de imports efetivos em `app/core/apps.py:8-10`, `app/ponto/apps.py:7-8`, `app/escala/apps.py:8-9`, `app/chamados/apps.py:8-10`, `app/colaboradores/apps.py:7-9` e `app/inteligencia/apps.py:7-10`. Sinais atuais exemplificados por `post_save/post_delete` em `app/ponto/signals.py:34,48`; receptor de migrate atual [nao verificado]. | NENHUM. Um caso de `apps.py`, `signals.py`, inicializador e helper direto de `ready()`; exigir recusa por arquivo. `bin/codex_pergunta.sh:162-176`. |
| Arquivos que mudam o resolvedor/commands `shell`, `makemigrations`, `migrate_schemas`, `collectstatic` | `app/manage.py:9-12`; `app/core/tests/test_model_sem_migration_pendente.py:48`; `bin/deploy.sh:194-207,296-300,338-340`. Alteram a pergunta, o apply ou um insumo da prova de casca. A troca via registro de comando Django depende da precedência dos apps e [nao verificado] em execução. | NENHUM para `manage.py`, `management/__init__.py`, `management/commands/__init__.py` e arquivos regulares dos comandos em app instalado. Testar cada slot como escrita fora da lista. `bin/codex_pergunta.sh:162-176`. |
| `app/core/tests/test_model_sem_migration_pendente.py`, seus REDs e `app/tenants/test_model_sem_migration_pendente.py` | O deploy importa a pergunta de dentro do teste. Cerca usa labels locais e `--check`; REDs cobrem campo folha, app sem pacote de migrations e campo tenants. `bin/deploy.sh:194-205`; `app/core/tests/test_model_sem_migration_pendente.py:24-48,56-80`; `app/tenants/test_model_sem_migration_pendente.py:14-42`. `app/core/tests/__init__.py` também integra essa cadeia de import. | NENHUM. RED por arquivo de pergunta e RED, mais inicializador e pacote homônimo candidato. Mudança c11 deve bloquear todas as origens da pergunta e dos REDs. Host test protegido `#G:p_bin`/`#G:p_fora` cobre respostas falsas em `bin/tests/test_deploy_portoes_antes_do_migrate.sh:91-100,245-259`, sem testar o conteúdo carregado no container. |
| Settings de CI, runner, quarentena e testes de ambiente | `app/config/settings/ci.py:40`; `app/core/quarentena_lista.py:9,25-28`; `app/core/runner_quarentena.py:75-96`; `bin/suite.sh:568`; `bin/pre-push.sh:184-199`. A lista elimina casos do run com trilha. `app/core/tests/test_ambiente_do_run.py:43-56` escreve testemunho que `bin/pista_certifica.sh:41-70,85-99` usa para certificação do pouso. | NENHUM. Exigir recusa dos settings, runner, lista e selo de ambiente quando fora da lista positiva. Caso especial para o selo se uma regra ampla de testes o incluir. `bin/codex_pergunta.sh:162-176`. |
| `app/**.py` e `app/**.html`; sítios de auth | `bin/arvore_sem_conflito.sh:19,29-35` verifica índice e marcadores. `bin/auth_sitios.txt:13-22` inclui `app/core/models.py` e `app/config/settings/base.py`, consultados no diff e no relógio por `bin/janela_auth.sh:56-61,85-115`. Ambos podem impedir o apply. `bin/deploy.sh:101,126-149`. | NENHUM para os caminhos normais de app. RED de recusa para caminho fora da lista. Para arquivo permitido, é RED de efeito do portão no fixture, pois wrapper deve aceitar o arquivo. |
| `app/templates/**`, `app/static/**`, `app/<app>/static/**`, `templatetags`, `app/config/urls*.py` e views alcançadas | `bin/deploy.sh:296-320` detecta arquivos mais novos e chama coleta/prova. `bin/prova_de_casca.sh:30-34,42-47`; `bin/prova_de_casca.py:30-53,68-98` abre HTML para referências estáticas, compila cinco páginas e importa os urlconfs/rotas. `app/config/settings/base.py:70-87,97-100`. Um erro pode impedir migrate. A prova de rota muda o resultado depois do HUP em `bin/deploy.sh:417-448`. | NENHUM para fontes comuns. `app/staticfiles/staticfiles.json` normalmente `#G:p_ignorado`, `.gitignore:13` e wrapper `:225`. Testar uma fonte de cada leitor e o manifest. |
| `app/gunicorn.conf.py`, `app/gunicorn.core.conf.py`, testes e helper do selo | `bin/selo_reciclagem.sh:34-49` lê config de disco via Gunicorn, `bin/deploy.sh:328,448` usa rc antes do apply e após HUP. `app/core/tests/test_contract_reciclagem.py:23-26,45-80` lê config via `core/censo_fonte.py` e cobra exatamente `30 3` no catálogo. O host procura somente a substring de reload no instalado em `bin/selo_reciclagem.sh:70-76`. | NENHUM para `.conf.py`, teste e helper. Um RED para cada configuração e para o teste se fora da lista. Selo de host `#G:p_bin`/`#G:p_fora`. |
| `app/requirements.txt`, `app/Dockerfile`, `app/.dockerignore` e compose | Pins Django/django-tenants em `app/requirements.txt:13,16`; build instala e copia em `app/Dockerfile:37-44`; `app/.dockerignore:8-13` distingue camada e bind-mount. Rebuild/recreate é ato separado do deploy, `bin/deploy.sh:20-22`. Mensageria tem compose próprio, mount separado e migrate no boot `docker-compose.mensageria.yml:13-20`. Compose SaaS ignorado em `.gitignore:40-41`, conteúdo atual [nao verificado]. | NENHUM nos três arquivos de app. Compose `#G:p_compose`, `#G:p_fora` para raiz, e `#G:p_ignorado` quando aplicável. `bin/codex_pergunta.sh:172,174,225`; RED compose `bin/tests/test_codex_pergunta.sh:422`. |
| Config, renderer e commands do cron como insumo da sombra | Render do app em `bin/sombra.sh:346-349`; roda a lista e agrega rc em `:351-391`; carimbo em `:400-406`, gate hoje/OK e DB de ensaio com migrations em `:410-433`; invocação antes de migrate em `bin/deploy.sh:151-167`. Alteração de app pode mudar o veredito do próximo ensaio. | `config/crons.py`: `#G:p_crons`, wrapper `:167`, RED `bin/tests/test_codex_pergunta.sh:417`. NENHUM para renderer `app/core/management/commands/crons.py:23,36-60`, helpers/commands e pacote homônimo candidato `config/crons/__init__.py`. Carimbo de host fora do app `#G:p_fora`. |

Nos casos de recusa pós-executor, exigir `PROIBIDO` com arquivo exato em `.recusa`, `rc=3`, `motivo=INVENTARIO` e `.id` ausente. O helper atual confirma motivo/rc/sidecar; para cada caminho deve confirmar também a coluna do arquivo. `bin/codex_pergunta.sh:223-233,799-823`; `bin/tests/test_codex_pergunta.sh:153-169`.

**Positivos com leitor ativo**

- `app/ponto/motor_calculo_v2.py` consta nominalmente em `CLAUDE.md:146` e no positivo `bin/tests/test_codex_pergunta.sh:410`. O cron de `app/config/crons.py:596` chama `app/ponto/management/commands/vigiar_motor_autoridade.py:34-38,57-65,102-108`. Também passa pelo elo `app/config/crons.py:1808,1818`, `app/ponto/management/commands/lavrar_cxr_do_motor.py:17,32,40-43`, `app/ponto/services/cxr_na_lista.py:58,78`, `app/ponto/services/espelho.py:1029,1147,1262` e `app/core/regua_cct.py:434`. O resultado/rc pode afetar carimbo de conclusão e dependente, pois `bin/cron_run.sh:11-26` só carimba 0/2.
- `models.py` regulares já existentes, exatos e nomeados no brief. O brief nominal está [nao verificado]. `app/folha/models.py` no positivo `bin/tests/test_codex_pergunta.sh:411` e `app/ponto/models.py` em `bin/tests/test_agente_no_modelo.sh:138` são fixtures, não prova do brief. Se nomeados, os caminhos de import e drift são inerentes; `colaboradores/models.py` já integra uma migration histórica; `tenants/models.py` já governa auto-criação; `core/models.py` já está no gate de auth. Referências na matriz.
- Testes do próprio app. A cerca de `core` é importada em produção pelo deploy, os REDs tenants são consumidos pelo pre-push e o teste de ambiente produz a prova do pouso. Todos os testes `.py` entram também na varredura de conflitos. O aval anexo item 2 manda retirar pergunta/REDs da cerca do alcance; não fornece regra para os demais selos consumidos por portão. `bin/deploy.sh:194-205`; `bin/pre-push.sh:198-199`; `bin/pista_certifica.sh:9,41-70,85-99`; `bin/arvore_sem_conflito.sh:29-35`.

**C e D.** Linha do cabeçalho é realmente `bin/codex_pergunta.sh:645`, após `REGRAS` em `:644`. Sítios anotados pelo opus: `CLAUDE.md:403`, `bin/disjuntor.py:1959,2111`, `bin/tests/test_agente_no_modelo.sh:13,287,355`, `bin/tests/test_censo_antes_da_vaga.sh:203`. A documentação interna do leitor também repete `:644` em `bin/disjuntor.py:2025,2038`. D: `#G:p_models_pkg` está em wrapper `:166`; falta em `bin/disjuntor.py:1961`; leitor seleciona somente aquelas tags em `:2036-2039`, negativa no caso de brief só com `models/` em `:2105-2112`; `CLAUDE.md:403` também omite.

**lei:** A L-126 veta gerar migration, e o anexo manda tirar a cerca e seus REDs do alcance. Falta resposta escrita encontrada para o predicado sobre efeitos de Python ainda permitido em `models.py`, motor e testes quando os leitores de produção ou portões já executam esses arquivos, e para a autoridade sobre outros selos de suite consumidos por portões. O detalhe dos nomes por brief e o estado efetivo da produção estão [nao verificado]. `app/docs/LEIS.md:147`; `app/docs/CORTES.md:651`; anexo CCM item 2; leitores na matriz.

## 27/11 02:50 · sessao 6262cbfd9789 · - · - · origem=Ronald (tmux)

- O wrapper inventaria alterações de conteúdo, modo, tipo, criação e remoção contra a árvore-base. O RED de recusa após a chamada deve exigir `rc=3`, caminho e motivo na linha `PROIBIDO`, ausência de `.id` e `.rc` com `rc=3`. O controle para uma alteração permitida exige `TOCADO`, `rc=0`, `.id` e ausência de `.recusa`. `bin/codex_pergunta.sh:187–233,805–830` e `bin/tests/test_codex_pergunta.sh:148–170`.

- A prova de casca é pré-migrate. Entram `app/templates/**/*.html`, os cinco templates compilados, estáticos e manifest, settings, storage, templatetags e o grafo transitivo de imports dos dois URLconfs. Qualquer falha impede o migrate e reload. **NENHUM** para arquivos regulares, com `p_ignorado` para um manifest que o Git reporte como ignorado. `bin/deploy.sh:285–339`, `bin/prova_de_casca.sh:30–47`, `bin/prova_de_casca.py:9–53,68–99`, `.gitignore:13` e `bin/codex_pergunta.sh:162–175,223–231`.

- Marcadores de conflito em qualquer `.py`/`.html`, inclusive arquivos permitidos, fecham o deploy antes de migrar. Uma mudança em qualquer sítio de auth, que inclui `app/core/models.py`, pode fechar tanto o deploy normal como o reload agendado entre 23:20 e 06:00. **NENHUM** por nome no wrapper. `bin/arvore_sem_conflito.sh:19–33`, `bin/deploy.sh:101,127–149`, `bin/auth_sitios.txt:13–22`, `bin/janela_auth.sh:85–115` e `bin/codex_pergunta.sh:162–175`.

- O selo de reciclagem pré-migrate lê a configuração em disco `app/gunicorn.conf.py` e `app/gunicorn.core.conf.py` via `gunicorn --print-config`. **NENHUM** para as duas. O compose de mensageria é `p_compose` e `p_fora`. O selo lê também o PID 1 e o crontab instalado, procurado por presença de `deploy.sh --reload-agendado`, sem validação de hora naquela leitura. O selo da suite exige exatamente um reload declarado às 03:30 e `max_requests=0`. `bin/selo_reciclagem.sh:34–79`, `bin/deploy.sh:324–339,448`, `app/core/tests/test_contract_reciclagem.py:48–62,70–80` e `bin/codex_pergunta.sh:162–175`.

- `app/core/tests/marca_cron15.py` afeta tanto a suite quanto o install. O command consulta seu prazo quando a sombra não cabe, para manter a linha elegível já instalada do host ou recusar. `check` não passa a linha do host. **NENHUM** para marca, command e REDs da suite. `app/core/management/commands/crons.py:49–60,91–119`, `app/core/tests/marca_cron15.py:27,32–65`, `bin/crons.sh:23–40`, `app/core/tests/test_crons_marca_cobre_install.py:94–147` e `bin/codex_pergunta.sh:162–175`.

- `app/config/settings/ci.py`, `app/core/runner_quarentena.py`, `app/core/quarentena_lista.py` e `app/core/testes_em_quarentena.txt` influenciam seleção e resultado da suite. `app/core/tests/test_ambiente_do_run.py` escreve SHA, camadas, job e argv que a certificação lê. Os testes descobertos e o `tests/__init__.py` importado também integram a execução. **NENHUM** por nome. A régua também lê selos em `bin/tests/` (`p_bin` e `p_fora`), analisa JS do app e grava `.regua_stamp` na raiz (`p_fora`). `app/config/settings/ci.py:40`, `app/core/runner_quarentena.py:75–105`, `app/core/quarentena_lista.py:9,27`, `app/core/tests/test_ambiente_do_run.py:44–57`, `bin/pista_certifica.sh:41–100`, `bin/suite.sh:198–216,568,611–616`, `bin/regua.sh:138–148,165–202`, `bin/node_check.sh:30–47`, `bin/pre-push.sh:45–69,186–199` e `bin/codex_pergunta.sh:162–175`.

- O carimbo `.sombra_stamp` está fora de `app/` (`p_fora`). `crons sombra` e os comandos do bloco determinam o status que o deploy lê junto com a confirmação das migrations no banco de ensaio. **NENHUM** para o command de render e os comandos do bloco pelo nome, `p_crons` para a fonte declarativa. `bin/sombra.sh:81–83,182–189,346–406,410–433`, `bin/deploy.sh:151–168`, `app/core/management/commands/crons.py:23,43–48` e `bin/codex_pergunta.sh:162–175`.

- `app/core/integrador_lote.py` e `app/core/esteira_vigia.py` são executados no host por unidades versionadas. O primeiro copia lotes, chama reload antes da suite e publica depois do push. O segundo relança unidades e pode chamar deploy de auto-revert,-testando o resultado. **NENHUM**. Timer ativo em produção [nao verificado]. `bin/integrador.sh:24,52`, `bin/hasner-integrador.service:13`, `bin/hasner-integrador.timer:6`, `app/core/integrador_lote.py:367–381,614–640,739–761`, `bin/vigia_esteira.sh:11`, `bin/hasner-esteira.service:15`, `bin/hasner-esteira.timer:7` e `app/core/esteira_vigia.py:951–961,1284–1317,1591–1594`.

- A versão **viva** de `app/docs/RELATO.md` e os arquivos citados no reconhecimento de `arquivo:linha` integram a decisão B3 que barra merge. A leitura da própria cópia diretamente por B3 [nao verificado]. `app/docs/LEIS.md` e `app/docs/CORTES.json` são dados do selo de índice, e `LEIS.md` inclui a coluna que determina os sítios cobrados no pre-push. **NENHUM**. `bin/hook_stop_fila1.py:32`, `bin/disjuntor.py:679–750,765–792,1030–1043,1289–1305`, `bin/commit_msg.sh:29–39`, `bin/tests/test_leis_indice.sh:16–36`, `bin/tests/test_lei_protege_sitio.sh:26–49,170–208` e `bin/pre-push.sh:12–16`.

- **C.** `bin/codex_pergunta.sh:644` é `REGRAS`, `:645` é `PROIBIDO`, `:646` é `ENTREGA`. Sítios do opus `CLAUDE.md:403`, `bin/disjuntor.py:1959,2111`, `bin/tests/test_agente_no_modelo.sh:13,287,355`, `bin/tests/test_censo_antes_da_vaga.sh:203`. Referências abreviadas adicionais `bin/disjuntor.py:2025,2038`, `bin/tests/test_agente_no_modelo.sh:140–143`; `app/docs/LEIS.md:147` e `app/docs/CORTES.md:651` citam `:646`. O selo verifica a string, a regex e a contagem, sem resolver a linha de destino. `bin/tests/test_agente_no_modelo.sh:286–288`.

- **D.** `p_models_pkg` recusa ao Codex e tem RED em `bin/tests/test_codex_pergunta.sh:412`. `OPUS_CONSTROI_TAGS` omite esse tag, logo um brief só com `models/` não passa no disjuntor. Omissões textuais em `CLAUDE.md:329,403` e `bin/disjuntor.py:2110–2112`. RED proposto do lado Opus é brief sintético só com `models/` admitido após a correção D, com mutante que omite o tag voltando a recusar. `bin/codex_pergunta.sh:166`, `bin/disjuntor.py:1961,2023–2039,2102–2112` e `bin/tests/test_agente_no_modelo.sh:134–143,286–288,355–356`.

- **lei:** O aval c11 manda corrigir D e voltar a cerca de schema e seus REDs para fora do alcance do Codex. A cobertura do restante das testemunhas consumidas por produção, se elas pertencem ao app dos testes autorizados, e o despacho dos demais caminhos agora recusados pela lista positiva não aparecem no texto do aval nem na lista de Opus existente. Não há nomes dos `models.py` nem do app do brief no material dado [nao verificado]. `app/docs/LEIS.md:147`, `CLAUDE.md:398–405`, `bin/disjuntor.py:1961` e anexo `CCM-C11-LISTA-POSITIVA-v2-2711-0250.txt`, item 2 [nao verificado na cópia].

## 27/11 02:50 · sessao 5a9c1213012a · E4D-CONTRATO-ROTA-B · construtor · origem=Code

A fix2 altera apenas os dois arquivos de teste autorizados. A checagem estática encontrou **45 casos numerados**, com o 33 fora do recorte, **53 mutantes nomeados** e **zero cláusulas `except`**. Conferi também os seletores SQL com `VALUES`, `UNNEST`, PK direto e `IN`. Execução Django, resultado individual e mordida dos mutantes ficam **[nao verificado]**. [T — matriz](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1) · [M — mutantes](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:1) [nao verificado]

O selo de escrita associa o `INSERT` ao par **pergunta e ato** e confere a origem de cada atualização da mesma pergunta. Assim, antecipar o ato e conservar uma escrita direta ainda é uma falha esperada. As guardas 21 e 23 usam a fixture de assinatura com trava real. O 27 faz o juiz distinguir a instância antiga da relida. [Selo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:219) · [Fixture com trava](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/_ato_da_fixture.py:47) · [Caso 27](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:861)

### Matriz

`T` e `M` são os arquivos acima. `C4` referencia a cópia `c154a8d4e`. A coluna “Hoje” é uma previsão por leitura. **Guarda** indica o comportamento que se espera preservar. [C4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_c4_red.py:1) · [C1 da c4](/home/ronald/wt-codex-e4c-f5-instante-012038/app/chamados/tests/test_e4c_f5_instante_c1.py:234)

| Caso | Regra e teste | Hoje | Mutante | [nao verificado]
|---|---|---|---|
| 01 | Apply grava R sem evento. `T:261`, `docs/PROMPTS.md:2873`, `C4:401,495` | O comando enumera R e aplica apenas colisões e silêncios. `backfill_decisoes_pergunta.py:489,631` | `01_backfill_emite_evento` |
| 02 | Aplica V histórico, preserva R por inteiro e não infere ABSORÇÃO. `T:275`, `docs/PROMPTS.md:2806`, `C4:336,401` | V fica só na projeção. `backfill_decisoes_pergunta.py:489,631` | `02_reescreve_resposta_antiga` |
| 03 | ABSORÇÃO vigente dispensa V retroativa. A compensada tem controle próprio. `T:294`, `docs/PROMPTS.md:2806` | O seletor oferece V na absorvida. `chamados/models.py:4437` | `03_foto_sem_respeitar_absorcao` |
| 04 | Aplica R/V em duas competências e preserva atos anteriores. `T:325`, `docs/PROMPTS.md:2950` | O comando lista as duas competências e não insere os atos. `backfill_decisoes_pergunta.py:489,631` | `04_descarta_segunda_competencia` |
| 05 | V e ABSORÇÃO físicas precedem V/M da mesma pergunta no mesmo bloco. `T:1110`, `docs/PROMPTS.md:2873` | Há carimbo e ABSORÇÃO, mas falta V física. `materializacao.py:1107` | `05_segundo_escritor_m` |
| 06 | Absolvição da célula escreve uma vez os três campos M e deixa zero atos. `T:651`, `docs/RELATO.md:292` | Guarda. `reconciliador.py:695` | `06_carimbo_reescrito` |
| 07 | Apply preserva `celula_absolveu`, com zero atos, eventos e reescritas de R/V/M. `T:359`, `docs/RELATO.md:292` | Guarda. `backfill_decisoes_pergunta.py:121,631` | `07_fabrica_conclusao` |
| 08 | Via comum preserva pergunta, TXT e gravado da exportada. `T:372`, `C4:419` | Guarda da via comum. `backfill_decisoes_pergunta.py:505` | `08_regrava_exportada` |
| 09 | V física precede a coluna. A via de M segue a testemunha do dia. `T:669`, `docs/RELATO.md:292` | O resolvedor carimba V sem linha física. `disputa_emissao.py:2112` | `09_segundo_escritor_v` |
| 10 | O resolvedor grava R física para texto em escolha e hora. Muda recebe só M. `T:698`, `docs/RELATO.md:292` | O ramo carimba R sem ato. `disputa_emissao.py:2189` | `10_segundo_escritor_r` |
| 11 | Reabertura vazia deixa um ato por pergunta, com projeção e eventos preservados. `T:399`, `docs/PROMPTS.md:2873` | O filtro exclui a pergunta vazia do lote de atos. `disputa_emissao.py:1695,1703` | `11_descarta_reabertura_vazia` |
| 12 | Cada uma das quatro referências de autoria protege seu próprio usuário. `T:415`, `docs/PROMPTS.md:2873` | As FKs admitem a exclusão com `SET_NULL`. `chamados/models.py:3804,3824,4347,4352` | `12_autor_da_foto_set_null` |
| 13 | Autor inativo conserva a foto integral dos atos e perde o refresh. `T:721`, `api/views.py:76` | Guarda. `api/views.py:96` | `13_refresh_inativo` |
| 14 | L-130 conserva o histórico e aceita nova resposta após a REABERTURA. `T:435`, `docs/LEIS.md:151`, `C1:234` | A porta atual limpa o par sem gravar REABERTURA. `validacao.py:444,475` | `14_apaga_resposta_antiga` |
| 15 | Competência aberta saneia. Entrega durante a espera impede o saneamento. `T:746`, `docs/LEIS.md:151` | Guarda. `validacao.py:417,465` | `15_k6_antigo` |
| 16 | A porta recusa R futura antes de inserir ato ou evento. `T:454`, `docs/PROMPTS.md:2873` | `registrar` aceita a linha futura. `chamados/models.py:4598` | `16_recusa_depois_do_insert` |
| 17 | `save` sem PK recusa a inserção e o censo cobra um escritor mesmo com alias. `T:469`, `docs/PROMPTS.md:2873` | `save` delega ao Django quando o PK está vazio. `chamados/models.py:4405` | `17_atalho_com_alias` |
| 18 | Tarefa, C6 e modal respeitam o horizonte. I23 exclui o caso de restore e mantém controle positivo. `T:492`, `docs/PROMPTS.md:2950`, `C4:297` | C6 reduz a sequência inteira, o modal lê o par atual e I23 conta a compensação adiantada. `chamados/juizes.py:1499`, `modal_fio.html:217`, `auditar_invariantes_chamados.py:1122` | `18_c6_julga_compensacao_cedo`, `18_i23_zero_fixo` |
| 19 | A humana vence no empate e a automática ainda escreve quando elegível. `T:774`, `docs/PROMPTS.md:2873` | Guarda. `disputa_emissao.py:1741` | `19_automatica_sobrescreve` |
| 20 | RECUSA no empate ou posterior interrompe só Q. As irmãs anterior e seguinte conservam os efeitos da passagem. `T:1140`, `docs/PROMPTS.md:2950` | A passagem não veta Q pela RECUSA relida. `materializacao.py:1089` | `20_ignora_recusa_no_empate` |
| 21 | RECUSA relê a validação sob trava e continua possível para uma elegível. `T:796`, `validacao.py:974` | Guarda. `validacao.py:989` | `21_recusa_sem_reler` |
| 22 | Validação observa a reabertura concorrente. O controle elegível valida e planta. `T:813`, `validacao.py:742` | Guarda. `validacao.py:752` | `22_valida_foto_antiga` |
| 23 | Correção relê V e uma elegível chega à validação com R corrigida. `T:839`, `acoes_disputa.py:418` | Guarda. `acoes_disputa.py:429` | `23_corrige_sem_reler` |
| 24 | Sem grupo recuperável, Q fica intacta e E conserva a nova resposta. `T:1187`, `materializacao.py:1119` | Guarda. `materializacao.py:1136` | `24_sem_veto_de_grupo` |
| 25 | Rejeição devolve irmã pela ABSORÇÃO vigente e preserva a compensada. `T:1216`, `docs/PROMPTS.md:2873` | O seletor por coluna exige âncora de hora e omite Q. `materializacao.py:1195` | `25_absorcao_so_por_coluna` |
| 26 | Sem ABSORÇÃO, preserva o legado e a vizinha de outra passagem. `T:1250`, `docs/PROMPTS.md:2873` | O agrupamento por coluna devolve o legado. `materializacao.py:1195,1267` | `26_devolve_sem_vinculo` |
| 27 | O fantasma só recebe M depois da trava, releitura e julgamento do valor fresco. `T:861`, `reconciliador.py:1265` | Guarda. `reconciliador.py:1268` | `27_julga_instancia_antiga` |
| 28 | O espelho relê sob trava e compensa H4 vigente antes de trocar a via. `T:898`, `docs/PROMPTS.md:2873` | O legado de controle entra no comando. A declaração com ABSORÇÃO fica fora do seletor. `encerrar_perguntas_com_fato.py:31,66` | `28_via_sem_compensacao` |
| 29 | Perda de lastro grava REABERTURA antes de limpar M. `T:941`, `docs/PROMPTS.md:2873` | O descarimbo direto não grava REABERTURA. `relavra_via.py:230` | `29_segundo_descarimbo` |
| 30 | Fila SQL, API, modal e raio-x observam a mesma resposta. `T:1272`, `docs/PROMPTS.md:2873` | Fila e raio-x classificam pelo par cru, que ainda aparece na API e no modal. `chamados/juizes.py:1863`, `api/views.py:1161`, `modal_fio.html:217`, `raiox.py:135` | `30_fila_pelo_carimbo_cru`, `30_api_pelo_par_cru`, `30_modal_pelo_par_cru`, `30_raiox_pelo_carimbo_cru` |
| 31 | Emissão automática e manual nascem com raiz, pai e filha coerentes. `T:1320`, `docs/PROMPTS.md:2873` | Guarda dos efeitos do nascimento. `disputa_emissao.py:582,1351` | `31_raiz_nasce_fechada` |
| 32 | `PerguntaDisputa.abrir` nasce com identidade, R/V/M vazios e zero DP. `T:1349`, `docs/PROMPTS.md:2950` | Guarda do nascimento sem assinatura. `chamados/models.py:3088,3330` | `32_filha_nasce_com_v` |
| 34 | SILÊNCIO físico precede M e o fecho chega ao pai. `T:1360`, `docs/PROMPTS.md:2873` | M é carimbado antes de SILÊNCIO. `chamados/models.py:2184` | `34_segundo_escritor_m` |
| 35 | Reabre a mais recente do chamado, com maior PK no empate. `T:1379`, `docs/CORTES.md:618` | A emissão abre outra disputa. `disputa_emissao.py:582` | `35_elege_de_outro_chamado` |
| 36 | Tranca retira por leitura e preserva a irmã viva de outra competência e o pai. `T:1416`, `chamados/juizes.py:1505` | Guarda. `competencia_trancada.py:162` | `36_ignora_tranca` |
| 37 | Destranca libera M, preserva R e poupa a via vizinha. `T:1449`, `chamados/models.py:3703` | Guarda dos efeitos. `competencia_trancada.py:279` | `37_regrava_r_no_destranque` |
| 38 | Desligamento grava SILÊNCIO antes de M e a cascata fecha o pai da muda. `T:1472`, `chamados/signals.py:20` | M é carimbado antes de SILÊNCIO. `chamados/models.py:2184` | `38_perde_cascata_do_pai` |
| 39 | VALIDACÃO precede V/M e sustenta a batida e a via. `T:962`, `docs/PROMPTS.md:2873` | V é escrita antes da linha física. `validacao.py:164` | `39_segundo_escritor_v` |
| 40 | RECUSA precede motivo e limpeza da resposta. `T:984`, `docs/PROMPTS.md:2873` | `recusa_motivo` é salvo antes da RECUSA. `validacao.py:995` | `40_segundo_escritor_motivo` |
| 41 | Correção sobrevive à recusa do plantio e deixa RESULTADO nos dois retornos. `T:1003`, `docs/PROMPTS.md:2950` | R precede o ato e os caminhos de recusa ainda carecem da linha RESULTADO. `disputa_emissao.py:1562`, `acoes_disputa.py:452` | `41_segundo_escritor_r` |
| 42 | Resposta retrodatada muda a projeção presente e preserva marco e lavra históricos. `T:541`, `docs/CORTES.md:609`, `C4:914` | O `Max(respondida_em)` cru move o marco passado. `veredito_lavrado.py:205` | `42_marco_pelo_par_cru` |
| 43 | Retry de fecho em T2 conserva PKs, trilha e eventos, com zero UPDATE. `T:1503`, `chamados/models.py:2094` | Guarda. `chamados/models.py:2100` | `43_retry_regrava_fecho` |
| 44 | Segundo apply preserva o ato inteiro. Nova pergunta elegível ainda entra. `T:562`, `docs/PROMPTS.md:2805`, `C4:419` | Falta a primeira inserção de R na via pública do apply. `backfill_decisoes_pergunta.py:631` | `44_duplica_retry` |
| 45 | Compara autor, texto e lastro por classe A/B e por novo/estoque. `T:585`, `docs/PROMPTS.md:2950`, `C4:574` | O vigia atual compara presença de V e perde essas diferenças. `vigia_ato_x_coluna.py:8` | `45_vigia_so_instante`, `45_ignora_fk_nula`, `45_estoque_vira_novo`, `45_conta_absorcao_legada` |
| 46 | Plantio atualiza a ata de `480/0` para `480/480` e prontidão de 0 para 100. `T:1525`, `folha/export.py:1104` | Guarda proposta com cartório real e sinal da batida. Execução [nao verificado]. `ponto/signals.py:137` | `46_denominador_realizado`, `46_omite_relavra` |

O caso 20 nomeia a caracterização que a cura inverte, `test_RED_recusa_datada_depois_mas_inserida_antes_da_absorcao_recusa`. Ela cobra `recusadas == []` e bloqueio H4 da próxima resposta. [Caso antigo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4c_f5_h4_resposta_apos_absorcao.py:364) · [Caso novo](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:1140)

### Lei e desenho

- **lei: 14.** A E4D pede SANEAMENTO próprio e alvo seletivo. A c4 manda REABERTURA marcada pela L-130. O teste segue a c4 e cobre a resposta escrita depois do saneamento. Falta decidir o que vale para uma resposta que já estava gravada no empate ou depois da histórica. `docs/PROMPTS.md:2950` · `docs/LEIS.md:151` · `C1:234` · `T:435`
- **lei: 30.** A E4D diz que o ato manda no par. A c4 manda que `composta` entregue o texto da projeção. A fix2 segue a c4 para `composta` e cobra o efeito do par nos quatro leitores públicos do caso. `docs/PROMPTS.md:2873` · `chamados/models.py@c154a8d4e:4635` · `T:1272`
- **lei: 18.** O zero foi aplicado à compensação do restore. Se o zero for universal, conflita com o I23, cuja caracterização conta uma compensação futura normal. `docs/PROMPTS.md:2950` · `auditar_invariantes_chamados.py:1122` · `test_e4c_f5_4c1_instante_unico.py:319`
- **lei/desenho: 04, 08 e 44.** Qual porta reverte apenas as linhas do lote de uma competência? Qual entrada transporta o `!` da exportada e publica os hashes reais? O retry do 44 mede reaplicação pública do comando. Qual entrada permite reenviar exatamente o mesmo lote, preservando a distinção da c4 para duplicata dentro de um payload? `backfill_decisoes_pergunta.py:455,514,631` · `ponto/portas/regen_exportada.py:62` · `C4:419`
- **lei/desenho: 11, 12, 20 e 26.** Qual entrada transporta a identidade do pedido de reabertura e de passagem? Qual leitura distingue autoria histórica desconhecida de autor nulo de sistema? Qual leitor público expõe o encaminhamento ao admin por pergunta e ausência? `disputa_emissao.py:1628` · `materializacao.py:1010,1215` · `chamados/models.py:4347` · `T:1250`
- **lei/desenho: lápides.** Onde fica a lista nominal de escritores, campos e lápides que o selo deve ler? Os testes medem os efeitos e os campos das vias exercitadas. A declaração de produção ainda não tem superfície definida nos arquivos consultados. `docs/PROMPTS.md:2873,2950` · `docs/RELATO.md:292` · `core/portas.py:396` · `T:229`
- **lei/desenho: 42.** Qual testemunha preserva o marco histórico quando a projeção muda? O F3 sela o leitor com zero consultas à tabela DP. A marca, a fronteira e a reexecução do estoque ficam na migração real coberta pela c4. `docs/CORTES.md:609,610` · `test_f3_lavra_foto_sem_decisao.py:312` · `0058_decisao_entrada_do_ato.py@c154a8d4e:9` · `C4:914`

### Comandos para o Code

Recorte da matriz e comando de cada mutante pelo nome da tabela. O prefixo do nome seleciona o caso correspondente. [Matriz](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/test_e4d_matriz_reds.py:260) · [Runner](/home/ronald/wt-codex-e4d-contrato-rota-b-174640/app/chamados/tests/e4d_matriz_mutantes.py:393)

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  chamados.tests.test_e4d_matriz_reds --verbosity 2

mutante_e4d() {
  E4D_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
    chamados.tests.test_e4d_matriz_reds \
    --testrunner=chamados.tests.e4d_matriz_mutantes.RunnerMutanteE4D \
    -k "test_caso_${1%%_*}_" --verbosity 2
}

mutante_e4d 05_segundo_escritor_m
mutante_e4d 27_julga_instancia_antiga
```

## 27/11 02:37 · sessao a1bc05e461b2 · CODEX-CONSTROI-MOTOR · censo · origem=Code

### Auditoria de quem perguntou

A volta 10 perguntou expressamente quem escreve e quem lê a cerca e os REDs. Também pediu três mutações isoladas da pergunta. Para a admissão do Opus, o pedido apresentou a triagem do achado 4 da v9 e perguntou se bastava tocar **um** caminho da lista. A citação usada nessa pergunta ainda incluía `models` e `motor`, que já tinham saído da redação vigente. `q.txt:2–4` · `CLAUDE.md:325–330`

Os pares `q.txt` e `r.txt` de `verif1` a `verif10` estão indisponíveis no caminho indicado da cópia. A tabela classifica **a volta 10 pelos anexos raspados**, com linhas contadas a partir do início de cada anexo. As classes para a obra inteira e a triagem das voltas anteriores ficam [nao verificado]. `CLAUDE.md:434–438`

| Achado | Classe na volta 10 | Evidência |
|---|---|---|
| **A — Construtor pode alterar a cerca e os REDs** | **(a) Tema presente, achado não visto** | A pergunta pediu **“QUEM ESCREVE e QUEM LE”** a função e os REDs. A resposta apontou `labels_locais()` como produtora e descreveu as mutações em memória dos testes. Ficou sem resposta quem pode editar os arquivos. O deploy importa a função diretamente, e os caminhos da função e dos REDs ficam fora dos padrões de `PROIBIDOS`. `q.txt:4` · `r.txt:25–27` · `bin/deploy.sh:194–196` · `bin/codex_pergunta.sh:162–176` |
| **B — `MIGRATION_MODULES` fora da guarda de caminho** | **(e) Não perguntado na v10** | **“Ha outro chamador de `call_command`”** pediu o censo de chamadas. A resposta identificou **“a única chamada Python”**. O pedido ficou sem a pergunta sobre quem controla a configuração que o Django usa para localizar migrations. `#G:p_migr` só reconhece o componente `migrations`, e a escrita em settings fica fora dessa guarda. O efeito completo no deploy está relatado no anexo. `q.txt:2–5` · `r.txt:29` · `bin/codex_pergunta.sh:162–176,223–233` · `opus.md:4` |
| **C — Referência `:644` errada** | **(e) Não perguntado na v10** | **“O descarte procede pela letra?”** tratava do requisito de tocar um caminho. **“O `any(...)` implementa essa condição”** foi a resposta. O checklist também citou **“selo sem mordida”**, limitado ao delta, e a resposta examinou os REDs da cerca. A referência foi mantida em `:644`, onde está `REGRAS`. `PROIBIDO` está em `:645`, e o selo exige a presença da string `:644`. `q.txt:3,5` · `r.txt:17,33` · `CLAUDE.md:403` · `bin/codex_pergunta.sh:644–645` · `bin/tests/test_agente_no_modelo.sh:286–288` |
| **D — `models/` vedado ao Codex e insuficiente para admitir Opus** | **(e) Não perguntado na v10** | **“O texto exige que toque UM”** perguntou quantos caminhos bastavam. A resposta confirmou **“pelo menos um caminho”** da lista vigente. Ficou sem pergunta a cobertura dos dois lados para `models/`. `p_models_pkg` barra o Codex e está ausente de `OPUS_CONSTROI_TAGS`. Assim, um pedido Opus cujo `ARQUIVOS:` só toque `models/` é recusado. `q.txt:3` · `r.txt:17–19` · `bin/codex_pergunta.sh:166` · `bin/disjuntor.py:1961,2036–2039,2105–2112` |

O descarte documentado na v10 é o da exigência de tocar todos os caminhos. Ele não sustenta **(b)** para A–D. Os quatro achados têm evidência legível no código, então a ausência de banco ou logs de produção não os põe em **(c)**. `q.txt:3,6` · `r.txt:17–19` · `opus.md:3–6` · `CLAUDE.md:431–438`

**MODO-COMUM [nao verificado].** O Opus final registrou A–D. Para propor a linha de classe **(d)**, falta evidência de que Codex e Opus já tinham deixado passar o mesmo achado. `opus.md:1–6` · `CLAUDE.md:434–442`

_8568 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
