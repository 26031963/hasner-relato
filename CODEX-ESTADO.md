# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 12:38** · publicado em **20/11/2026 12:40** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 12:38 · sessao 4cd48316710c · E4C-F5-CONSTROI · construtor · origem=Code

**Pendências da sessão.** Suíte, mutantes, corrida em duas conexões e censo A/B no SHA final [nao verificado]. Fiz a checagem estática de sintaxe dos dois arquivos e `git diff --check`. O selo fora do escopo continua chamando `carimbar` diretamente. [Selo direto](chamados/tests/test_e4c_f5_selo_par_ato_coluna.py#L515)

**Empate.** `projecao_temporal_da_pergunta` corta somente `respondida_em > T1`. A resposta completa em T1 participa da passagem. [Leitor](chamados/juizes.py#L1394) · [RED do empate](chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py#L247)

**Âncora única fora.** No cenário R5, a âncora fica `RESPONDIDA` e a dependente fica `PENDENTE`. Ambas ficam sem carimbo e sem `ABSORCAO`, com a validação ausente na composta e na coluna. O caminho posterior da dependente fica em aberto com Ronald. [RED de estado gravado](chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py#L201)

**LEI-AKITA** `origem=materializador da ausência; testemunha=composta × coluna e grupo da rejeição; RED=T2, empate e DEVOLUCAO posterior; padrão=Parallel Change / Expand-Contract; juízes novos=0`. `chamados/services/materializacao.py:1052`

### Construção

- `_materializar_ausencia_de_resposta` mantém T1 comum e relê por PK sob a trava de cada tentativa. A resposta completa posterior entra em `recusadas` antes de `carimbar`. A trilha usa o escritor best-effort do C6, com razão própria e foto dos instantes. `chamados/services/materializacao.py:1036`, `chamados/services/materializacao.py:1052`, `chamados/services/materializacao.py:1088`, `core/observ.py:105`
- O `atomic` da irmã conserva `ABSORCAO` junto do carimbo depois de conferir o par e, para grupo ancorado, a recuperação pelo leitor da rejeição. Divergência desfaz a tentativa local. A entrada `escolha` que já chega sem âncora de hora segue o fluxo anterior. `chamados/services/materializacao.py:1041`, `chamados/services/materializacao.py:1051`, `chamados/services/materializacao.py:1070`
- `_perguntas_em_voo` usa o contexto existente durante os fechamentos síncronos. Isso protege as candidatas de um segundo carimbo via S143 ou S144 durante a própria passagem. `chamados/services/materializacao.py:1101`, `chamados/services/materializacao.py:1834`, `chamados/services/disputa_emissao.py:2016`, `chamados/services/materializacao.py:2211`

**SELECTs por irmã, por leitura estática.** A releitura custa `+1` SELECT da pergunta com a célula. O prefetch fresco custa um SELECT de decisões e serve também ao C6 e à conferência do par, substituindo a leitura avulsa do C6 nas tentativas que chegam a ele. A conferência de grupo custa até `+3` SELECTs quando chamada. Contagem executada [nao verificado]. `chamados/services/materializacao.py:1045`, `chamados/services/materializacao.py:1053`, `chamados/models.py:4333`, `chamados/services/materializacao.py:1150`

### Testes e comandos [nao verificado]

```bash
modulo=chamados.tests.test_e4c_f5_a1c_passagem_da_ausencia
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$modulo"
a1c() {
  PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$modulo.PassagemDaAusenciaTest.$1"
}
```

Os testes abaixo releem a pergunta e comparam a presença e o instante da validação nos dois lados. Os resultados são esperados [nao verificado]. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:104`

| Teste | Regra | Esperado | [nao verificado]
|---|---|---|
| `test_RED_resposta_t2_concordante_fica_fora_da_passagem` | R1, R2, R4, R7 | `RESPONDIDA`, em `recusadas`, zero chamadas a `carimbar`, trilha temporal. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:122` |
| `test_resposta_automatica_t2_tambem_fica_fora` | R1, R2 | A resposta automática em T2 recebe a mesma proteção. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:150` |
| `test_controle_ancorado_em_t1_volta_na_rejeicao` | R2, R5, R7 | Dois carimbos em T1, assinatura anterior preservada e devolução real do grupo. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:169` |
| `test_RED_ancora_unica_fora_deixa_dependente_sem_carimbo` | R5 | Âncora `RESPONDIDA`, dependente `PENDENTE`, ambas sem carimbo e sem `ABSORCAO`. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:201` |
| `test_RED_resposta_t2_ja_validada_conserva_resposta_e_assinatura` | R1, R2 | Resposta T2 e validação T3 preservadas, inclusive no fechamento com lastro para S144. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:219` |
| `test_RED_empate_com_t1_fica_na_passagem` | R1, R2 | Carimbo e `ABSORCAO` em T1. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:247` |
| `test_RED_recusa_antes_da_trava_muda_a_via` | R1, R2, R7 | A releitura vê a recusa e carimba `VIA_AUSENCIA_DIA`, com o par vazio. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:259` |
| `test_RED_devolucao_t2_desfaz_carimbo_aceito_pelo_c6` | R3, R6, R7 | C6 aceita a tentativa. O rollback descarta carimbo e `EventoCelula`, conserva a resposta T0 e põe a irmã em `recusadas`. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:276` |
| `test_RED_falha_da_trilha_temporal_deixa_a_passagem_seguir` | R4, R7 | Erro registrado, irmã preservada e controle seguinte absorvido em T1. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:300` |
| `test_RED_falha_no_insert_da_absorcao_desfaz_o_carimbo` | R2, R3, R7 | Falha no INSERT desfaz também o carimbo. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:327` |

### Mutantes

Uma troca temporária por linha, com o comando do teste que deve morrer [nao verificado]. [nao verificado]

| Sítio | Troca exata | Comando |
|---|---|---|
| `chamados/services/materializacao.py:1052` | Mover `q = irmas_frescas.filter(pk=candidata.pk).first()` para imediatamente antes de `trava_do_colaborador`. | `a1c test_RED_resposta_t2_concordante_fica_fora_da_passagem` |
| `chamados/services/materializacao.py:1065` | Trocar `declarou_hora(q)` por `declarou_hora(candidata)`. | `a1c test_RED_recusa_antes_da_trava_muda_a_via` |
| `chamados/services/materializacao.py:1059` | Trocar a condição da recusa temporal por `if False:`. | `a1c test_RED_resposta_t2_concordante_fica_fora_da_passagem` |
| `chamados/services/materializacao.py:1059` | Trocar `respondida_em_t1 is None` por `(respondida_em_t1 is None or leitura.respondida_em == agora)`, exercitando `>=` no consumidor. | `a1c test_RED_empate_com_t1_fica_na_passagem` |
| `chamados/services/materializacao.py:1060` | Inserir na recusa temporal `q.validada_em = q.validada_por = None` seguido de `q.save(update_fields=['validada_em', 'validada_por'])`. | `a1c test_RED_resposta_t2_ja_validada_conserva_resposta_e_assinatura` |
| `chamados/services/materializacao.py:1070` | Remover a atribuição `validada_pelo_ato` e o `if` que levanta `_Reprovada('par_ato_coluna_divergente')`. | `a1c test_RED_devolucao_t2_desfaz_carimbo_aceito_pelo_c6` |
| `chamados/services/materializacao.py:1077` | Mover a chamada inteira a `DecisaoPergunta.registrar` para depois do `with _tx_abs.atomic()`, ainda no `try`, sob `if motivo is None:`. | `a1c test_RED_falha_no_insert_da_absorcao_desfaz_o_carimbo` |
| `chamados/services/materializacao.py:1074` | Remover o `if` do grupo e o `raise _Reprovada('sem_grupo_recuperavel')`. | `a1c test_RED_ancora_unica_fora_deixa_dependente_sem_carimbo` |
| `chamados/services/materializacao.py:1088` | Remover a chamada inteira a `_evento`, preservando `recusadas.append`. | `a1c test_RED_resposta_t2_concordante_fica_fora_da_passagem` |
| `chamados/services/materializacao.py:1047` | Trocar `sorted(candidatas, key=lambda q: q.pk not in ancoras)` por `candidatas`. | `a1c test_controle_ancorado_em_t1_volta_na_rejeicao` |
| `chamados/services/materializacao.py:1108` | Retirar `with _perguntas_em_voo(pks):` do fechamento da disputa e dedentar `disputa.fechar(...)`. | `a1c test_RED_resposta_t2_ja_validada_conserva_resposta_e_assinatura` |

### Quem mais lê

- **C6.** `carimbar` chama o juiz com a tentativa aplicada. O C6 lê a composta e usa a concordância com a luz quando cabível. `chamados/models.py:3358` · `chamados/juizes.py:1406` · `chamados/juizes.py:1428`
- **Consumidor da rejeição.** O `on_commit` chama a devolução, que reencontra o grupo pela âncora, disputa, dia e relógio. `ponto/signals.py:254` · `chamados/services/materializacao.py:1146` · `chamados/services/materializacao.py:1188`
- **Vigia A/B.** Compara a presença da validação composta com a coluna e publica as duas contagens. `chamados/services/vigia_ato_x_coluna.py:8` · `chamados/management/commands/vigiar_fio_morto.py:198`

## 20/11 12:23 · sessao e087ded65b96 · E4C-F5 · verificador · origem=Code

## P1. As premissas conferem na árvore?

**1. Resposta posterior a T1.** O materializador lê T1 depois de criar ou obter a ausência e antes de selecionar as irmãs. Hoje ele calcula a via antes da trava e chama `carimbar` com a instância da seleção, sem reler a irmã nem testar T2. Já existem as peças da leitura. `DecisaoPergunta.composta` entrega `respondida_em` da sequência e `resposta_colab` **da coluna**. A sequência ordena por instante, põe atos reais em ordem de PK e a linha sintética depois dos reais no empate. `RECUSA` e `REABERTURA` limpam a resposta. C21, `pergunta_respondida`, exige instante e texto após trim, qualquer que seja a origem. Sobre a composta **fresca e sem corte em T1**, C21 mais `respondida_em > T1` responde ao item. A função `projecao_temporal_da_pergunta(..., T1)` oculta justamente a resposta posterior. O C6 usa essa função para julgar vitalidade e pode dar a pergunta por encerrada se a resposta concordar com a luz. O materializador ainda carece da composição desses leitores. `app/chamados/services/materializacao.py:1024–1069` · `app/chamados/models.py:4283–4317,4333–4362` · `app/chamados/juizes.py:1383–1409,1426–1440,1533–1535,1551–1560`

**2. Falha da auditoria.** Confere para falha no `INSERT` da trilha. Quando C6 mantém a irmã viva, `carimbar` restaura a foto em memória e retorna `False` antes do `save`. O materializador pula `ABSORCAO`, põe o PK em `recusadas` e chama `core.observ.evento` fora do bloco da irmã. O `INSERT` tem `atomic` próprio. Na falha, o escritor retorna `None`, incrementa o contador de engolidos em memória e emite `ERROR`. A linha de `LogAuditoria` fica ausente. A passagem ainda tenta reconciliar o destino e fechar a disputa pelo C7. A porta de resposta descarta a lista e, se o restante concluir, retorna `(True, None)`. A premissa de que a irmã conserva a foto vale para **a tentativa de carimbo**. O teste existente verifica a trilha com o escritor disponível. A entrega do `ERROR` ao suporte em produção está [nao verificado]. `app/chamados/models.py:3346–3386,2029–2035` · `app/chamados/services/materializacao.py:1060–1090` · `app/core/observ.py:73–105,124–139` · `app/chamados/services/disputa_emissao.py:1557–1578` · `app/chamados/tests/test_o261e_f2_fila.py:241–258`

**3. Âncora de hora.** Hoje nenhuma etapa do escritor elege uma âncora ou ordena o laço por dependência. `p` é o escape que disparou a passagem. O escritor percorre as perguntas de hora do mesmo dia e disputa com carimbo nulo, e grava cada carimbo aceito e seu `ABSORCAO` em T1. Quem identifica a âncora **depois** é `perguntas_absorvidas_por`. Para a ausência, ele procura uma pergunta de hora com carimbo, via aceita e texto de escape compatível com o tipo, e usa seu par `(disputa_id, materializada_em)` no recorte de colaborador e dia. Exclui origens de batida viva. Se a única âncora ficar fora, as outras ainda podem ser gravadas no código atual. O leitor devolve `[]` e a rejeição devolve `0` para esse grupo. O vínculo físico em `ABSORCAO` existe, mas esse leitor usa o carimbo. `app/chamados/services/disputa_emissao.py:1557–1560` · `app/chamados/services/materializacao.py:1033–1069,1097–1103,1123–1139,1159–1170`

**4. Tranca, TXT e vigia.** C20 é `dia_da_pergunta_trancado`, cuja forma em lote `dias_trancados` é consumida pela tarefa. Pela janela K8, busca em K7 um `PeriodoFechado` ainda sem `reaberto_em` e procura toque humano registrado para o mesmo colaborador-dia depois do fechamento. Valem desarquivar, Resolver dia com assinatura humana e **Cobrar este dia**. Cobrar resposta grava uma ocorrência no chamado. O TXT é uma pergunta separada, K6, que lê `ExportacaoDominio` **vigente** por empresa, mês e ano, inclusive para distinguir revogação sem substituta. Hoje C20 faz a tarefa devolver `competencia_trancada`, sem PK da pergunta nem ação na instância. A carga seletiva de K6 pula pares trancados e pode pular marcos ordinários. O vigia indicado pelo rascunho é `vigiar_fio_morto` → `vigia_fio.censo_alcance`. Ele conta **colaborador-dias que seguram a folha**, por rota e papel, com `sem_porta` como um dos resultados. Seu universo vem de `dias_que_seguram`, nos períodos a lavrar. O código atual carece do censo por pergunta marcada em C20 verdadeiro e K6 falso. `app/core/juizes.py:303–305,342–344` · `app/chamados/juizes.py:979–1013,1067–1098,1242–1256` · `app/ponto/services/fechamento.py:1464–1470` · `app/folha/export.py:556–588` · `app/chamados/views_cobrar.py:34–57` · `app/chamados/services/tarefa_do_dia.py:227–235,550–579,589–619` · `app/chamados/management/commands/vigiar_fio_morto.py:188–198,213–216` · `app/chamados/services/vigia_fio.py:529–545,582–621,658–670`

**5. Cobrar por pergunta.** O modal agrupa por `chamado_que_lista` e escolhe a disputa do cartão. O botão depende de acesso ao cartão, disputa aberta, ramo fora de `ancora_ausente` e pelo menos uma pergunta agrupada que espere texto e esteja viva pelo C6. O produtor reduz isso a `todas_respondidas`. A Central usa outro caminho: `creditos_perguntas` reduz as perguntas a conjuntos de IDs de **chamado**, `particionar_verbos` publica o verbo por chamado e a linha detalhada oferece o botão para `cobrar` sem cobrança registrada hoje. Nas duas superfícies, o POST recebe `cid`, checa permissão e departamento, limita por colaborador-dia e envia um push genérico. Há insumos para identificar a pergunta que motivou a oferta no modal. A atribuição por PK compartilhada pelas duas superfícies e pelo C19 está [nao verificado]. A obra `PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA` está registrada para **2.333 colaborador-dias** com pergunta devida vencida pelo C9 e sem ação alcançável para o admin, na sombra v10, competências 9–11. O registro espera decisão sobre o que o admin pode fazer. A medição bruta está [nao verificado]. `app/chamados/services/fio.py:97–117,164–190,216–233,260–294,388–395` · `app/templates/chamados/partials/modal_fio.html:123,354–380` · `app/chamados/services/raiox.py:51–74,135–158` · `app/chamados/services/verbos.py:30–52` · `app/templates/chamados/partials/_lista_chamados.html:83–84` · `app/chamados/views_cobrar.py:24–57` · `app/docs/CORTES.md:490` · `app/docs/BACKLOG.md:771` · `app/core/juizes.py:293`

**Construção.** A árvore contém o contador A/B, o selo, a devolução que aceita validação só no ato e o teste da classe vii com controle que morde. A reabertura em lote também já trata validação só no ato, corrigindo a suspeita do rascunho nesse sítio. O caso direto de `carimbar` com resposta mais recente segue no selo e fora do laço que A1 altera. O wrapper veta `models.py`, `core/juizes.py` e `bin/`. A cópia veio sem metadados Git. O HEAD `604505878403ca9e69e68ebf4650b35be3d16958`, a execução dos testes e os números da sonda citados no rascunho estão [nao verificado]. `app/chamados/services/vigia_ato_x_coluna.py:8–25` · `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:454–465,515–558` · `app/chamados/services/validacao.py:273–296` · `app/chamados/services/disputa_emissao.py:1636–1668` · `app/chamados/tests/test_reconciliar_pergunta_a_premissa.py:212–219` · `bin/codex_pergunta.sh:121–135`

## P2. Colisões com os cortes e avais vigentes

**1.** Preservar toda resposta vigente completa, inclusive automática, cabe na L-130. A formulação anterior «fica pendente», atribuída ao 1135 no rascunho, conflita se for tomada como rótulo da pergunta. Com resposta completa e sem validação nem carimbo, o código devolve `RESPONDIDA`. Também seria incorreto impor coluna vazia quando já houve validação legítima posterior. O item 1 e a minuta A1 permitem conservar esse par. O literal integral do 1135 está [nao verificado]. `app/docs/LEIS.md:151` · `app/chamados/models.py:3259–3268` · `app/chamados/services/validacao.py:161–175`

**2.** «Segue com o erro registrado» cabe na garantia da minuta aprovada e no contrato do escritor. Já «trilha gravada» do 1135, tal como citada pelo rascunho, seria incompatível como garantia incondicional de `LogAuditoria`. A L-007 exige trilha para ação crítica. A exceção operacional do item 2 trata a tentativa recusada sem escrita de carimbo. Ela não dá licença para dispensar trilha de uma transição que efetivamente grave estado. O literal integral do 1135 está [nao verificado]. `app/docs/LEIS.md:65` · `app/chamados/models.py:3358–3377` · `app/chamados/services/materializacao.py:1064–1089` · `app/core/observ.py:94–105,124–139`

**3.** Adiar as dependentes preserva o relógio comum e o leitor de devolução exigido na ficha. A colisão aparece se «esperam nova passagem» for cumprido gravando dependentes sem âncora, trocando a chave da devolução ou criando ato de reparo. O primeiro caminho quebra a recuperação na rejeição. Os outros saem do desenho aprovado e contrariam os limites da ficha. `fichas_2011_1150.txt:7` · `app/chamados/services/materializacao.py:1060–1069,1113–1139,1166–1194`

**4.** A exceção pré-toque cabe no literal de `CORTE-RESSUSCITAR-TEM-DONO`, registrado em `CORTES.md:249` e publicado integralmente na linha 545. Suspende «não fica sem dono» e o destino ADMIN em aberta enquanto C20 arquiva o dia. A regra por estado que substituiu o item 1 está registrada no RELATO, assim como a decisão do 1150 de cobrar ADMIN no caso SILÊNCIO em F5. O trecho «aí vale o dono ADMIN» só é compatível com o item 5 e com E3-V10 se, após o toque, a mesma pergunta ainda qualificar e tiver porta. A exceção também precisa respeitar a cláusula de **paga fora** encaminhada ao DP. K6 falso, sozinho, não identifica esse caso. A autoridade para «paga fora» está [nao verificado]. `app/docs/CORTES.md:249,490,545` · `app/docs/RELATO.md:768,785–787` · `app/chamados/services/tarefa_do_dia.py:227–235,748–778` · `app/folha/export.py:556–578`

**5.** Sem porta para aquela pergunta cabe como a segunda exceção nominal ao dono obrigatório de `CORTES.md:545`, inclusive perante a regra de SILÊNCIO aberto registrada para F5/F6. E3-V10 decide o **fio do colaborador-dia**. Se outra pergunta tem resposta alcançável no app, o dia continua do colaborador e a instância silenciada pode ficar sem porta. A outra cláusula em jogo é L-002/L-011. Reusar os fatos que realmente produziram o botão cabe em S1. Criar uma regra que atribua um botão genérico a uma pergunta quando a oferta existente não responde por ela exige o corte da autoridade. A mesma obra pode publicar o total de PKs e a interseção deduplicada de colaborador-dias sob C9. Somar PKs aos 2.333 mistura as unidades. `app/docs/CORTES.md:490,545` · `app/docs/LEIS.md:60,69,149` · `app/docs/RELATO.md:768` · `app/docs/BACKLOG.md:771` · `app/chamados/services/tarefa_do_dia.py:734–778`

**Construção.** A ordem A1 → S1 e a exigência das seis provas antes do PRONTO cabem na ficha. O PRONTO integral exige resolver o selo direto de `carimbar`, que A1 deixa fora. Para S1, a opção A do 1150 autoriza mudar a expectativa-alvo de SILÊNCIO e mantém o controle de entregue. O golden compara o dicionário inteiro e o CSF. Colocar o diagnóstico DP nesse dicionário para o caso entregue quebra o contrato, inclusive se a ação operacional ficar vazia. `fichas_2011_1150.txt:1,7–8` · `app/docs/RELATO.md:768,785` · `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:515–532` · `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:32–46,138–163`

## P3. Contraexemplos ao literal

**1.** Não achei contraexemplo à regra combinada com A1 partindo de um par íntegro. Conferi T2 humano, automático e sintético, empate, validação legítima após T2 e uma devolução posterior. O limite decisivo é **resposta em T0 < T1, `DEVOLUCAO` em T2 > T1**. A devolução conserva a resposta e deixa as duas validações vazias. O filtro do item 1 deixa passar, e se C6 aceitar pelo fato, `carimbar` pode gravar coluna em T1 enquanto a devolução posterior mantém a composta nula. O `ABSORCAO` é neutro. Cumprir só o filtro produz B. A conferência do par e o rollback local, já exigidos por A1, cobrem esse caso sem ato de reparo. `app/chamados/services/validacao.py:283–296` · `app/chamados/models.py:3349–3372,4290–4303,4333–4351` · `app/chamados/juizes.py:1426–1440`

**2.** Não achei contraexemplo no ramo de falha autorizado. Entrem uma irmã muda com ambas as validações nulas e uma ausência já obtida. C6 recusa o carimbo, o PK entra em `recusadas` e o `INSERT` da auditoria falha. A irmã fica sem carimbo e sem `ABSORCAO`, o par fica nulo dos dois lados e sobra o erro operacional. Conferi o rollback do `INSERT`, a ordem do laço e as caudas de reconciliação. Forçar rollback da resposta ou engolir falha de `DecisaoPergunta.registrar` seria outro comportamento. Esse registro de ato está dentro do bloco da irmã. `app/chamados/models.py:3346–3386` · `app/chamados/services/materializacao.py:1024–1034,1060–1090` · `app/core/observ.py:73–91,124–139` · `app/chamados/services/disputa_emissao.py:1526–1532,1557–1560`

**3.** «Não vira beco» **não é demonstrável pelo código atual**. Exemplo para o RED: resposta `FALTA_DIA` numa âncora de hora, falta aprovada criada, T1 capturado, correção da âncora para `19:02` em T2. Pelo aval, a âncora fica fora e a dependente muda espera. Em T3, a validação ordinária da âncora carimba o par e roda `materializar_perguntas_validadas_da_disputa`. Esse laço pula a dependente sem resposta e sem validação. Enquanto o dia estiver coberto pela ausência aprovada, a tarefa dá o marco por `ausencia_aprovada`, sem ação. Ficam gravadas a ausência, a resposta e a validação da âncora, com a dependente sem `ABSORCAO` e sem cobrança naquele estado. Se depois a ausência for rejeitada, a devolução só alcança o grupo carimbado. **Quem chama a mesma passagem de novo:** a porta de resposta, quando recebe outro escape da tabela. Validação ordinária, Resolver dia, o `on_commit` da ausência e o fechamento da disputa chamam outras portas. A reaparição no app exige C6 vivo, pergunta sem resposta e disputa aberta. O RED de ponta a ponta está [nao verificado]. `app/chamados/services/disputa_emissao.py:1481–1508,1526–1531,1557–1560` · `app/chamados/services/materializacao.py:1097–1103,1123–1139,1166–1170,2109–2135,2173–2189` · `app/chamados/services/validacao.py:757–760,801–806` · `app/chamados/services/tarefa_do_dia.py:312–324` · `app/ponto/services/veredito_celula.py:176–203` · `app/ponto/signals.py:250–272` · `app/chamados/services/pergunta_no_app.py:64–92`

**4.** Entrem uma instância com SILÊNCIO efetivo e última linha marcada, C20 verdadeiro, K6 falso e nenhum botão atribuível à pergunta. O admin desarquiva o chamado desse dia. O toque fica na trilha e C20 libera o julgamento. A pergunta segue sem botão. Aplicar «aí vale o dono ADMIN» de forma incondicional viola o item 5, mesmo se o único efeito novo for na saída da tarefa. Há outro contraexemplo para o contador. Se esse colaborador-dia não estiver entre os que retêm por `furo_espelho`, o censo atual nem o visita, apesar de C20 verdadeiro e K6 falso. `app/chamados/juizes.py:1076–1098,1312–1331` · `app/chamados/services/tarefa_do_dia.py:182–183,227–235,748–756` · `app/folha/fora_da_folha.py:125–157` · `app/chamados/services/vigia_fio.py:529–545`

**5.** No mesmo chamado, P é a silenciada marcada e só Q satisfaz o recorte que motiva Cobrar. A oferta existente enxerga o chamado. Atribuí-la a P viola o item 5. Se o admin clicar, grava-se uma ocorrência no chamado por colaborador-dia, sem vínculo com P. Há ainda o caso em que P de fato motiva a oferta, mas o colaborador já recebeu cobrança hoje por outro chamado. A Central mostra a hora, o modal ainda pode renderizar o botão e o POST recusa sem novo push ou ocorrência. O literal precisa decidir se esse caso conta como `sem porta` ou como cobrança já efetuada. O invariante A/B fica intacto nesses dois exemplos. `app/chamados/services/fio.py:164–190,388–395` · `app/templates/chamados/partials/_lista_chamados.html:83–84` · `app/templates/chamados/partials/modal_fio.html:363–380` · `app/chamados/views_cobrar.py:34–57`

**Construção.** O próprio selo fornece um contraexemplo fora de A1. Ele chama diretamente `p.carimbar(..., quando=T1, resposta='validar')`, com `RESPOSTA(False)` em T2 e concordância com a luz. `carimbar` preenche a coluna em T1, a composta perde a validação para T2 e o teste intercepta a chamada a `save` e compara os dois lados. Alterar só o laço A1 não alcança essa chamada. Com as condições do selo, a leitura estática dá B. O resultado executado está [nao verificado]. Liberar PRONTO deixando a expectativa cair ou excluindo esse controle reduziria a prova da ficha. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:515–532` · `app/chamados/models.py:3349–3372,4333–4351` · `app/chamados/juizes.py:1426–1440` · `fichas_2011_1150.txt:7`

## P4. Competência exportada, dinheiro e sítios vedados

**1.** A1 muda quais carimbos, atos de absorção e eventos da célula saem de uma porta que cria ou obtém a ausência **antes** do laço. A criação pode invalidar o carimbo da projeção mensal quando muda o insumo. A cerca genérica só julga escrita em ato cercado. As portas de correção e declaração podem gravar resposta antes de chegar à validação, que tem sua própria guarda de entrega e chama recálculo. Há alcance indireto de fechamento nessas fronteiras. O delta em exportada, TXT, batida e dinheiro está [nao verificado]. `app/chamados/services/materializacao.py:1024–1036,1060–1069` · `app/chamados/signals.py:243–259` · `app/ponto/services/ausencia.py:154–164,581–599` · `app/ponto/services/fechamento.py:1648–1656` · `app/chamados/services/fio.py:1394–1409` · `app/chamados/services/acoes_disputa.py:375–385` · `app/chamados/services/declaracao_texto.py:689–698` · `app/chamados/services/validacao.py:746–772`

**2.** Reusar o escritor da recusa conserva seu `atomic` e seu alcance. O caminho posterior pode reconciliar destino e fechar disputa. A transição com pauta também pode entregar recusas ao DP pela porta existente. Isso é efeito de negócio da passagem, além da foto da irmã recusada. Não encontrei edição de arquivo vedado nem nova trava exigida para o tratamento da falha em si. `app/core/observ.py:124–139` · `app/chamados/services/materializacao.py:1070–1090` · `app/chamados/services/fecho_runtime.py:95–135,146–168`

**3.** Processar a âncora primeiro muda a ordem **das perguntas processadas**, mantendo a trava existente do colaborador por tentativa. A ordem global continua Colaborador → Disputa → Chamado → Pergunta. Os blocos separados deixam intervalo para perda da âncora entre tentativas. Uma correção que alargue a transação para reter a trava por todo o grupo, ou introduza nova aquisição, sai da fronteira aprovada de A1. A mesma passagem reexecutada ainda chamaria a porta de ausência e a reconciliação. `app/chamados/models.py:2830–2854` · `app/chamados/services/materializacao.py:1024–1036,1060–1089,1123–1139` · `fichas_2011_1150.txt:7`

**4.** S1 precisa ler mais pares por K8/K6, inclusive os trancados para o censo e os ordinários para proteger entregue. Essa leitura passa por `folha.models` e `folha.export`. C20 já responde pela tranca e pode ser lido sem editar seu juiz. O alcance aprovado em entregue é diagnóstico DP, com tarefa operacional e CSF preservados. O efeito de uma implementação em competência entregue está [nao verificado]. `app/chamados/services/tarefa_do_dia.py:550–579,589–619` · `app/folha/export.py:556–588` · `app/chamados/juizes.py:1009–1013` · `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:138–163`

**5.** A decisão por instância pode mudar o fio e a ação do dia que a Folha consome. `fora_da_folha.py` lê a porta publicada pela tarefa. O retrato tem tradução para `validar`, `decidir`, `resolver_dia` e `decidir_ausencia`. Para um `cobrar` sem tradução e fio `admin`, pode cair no link legado «Validar no fio». Corrigir esse rótulo ali exigiria edição de `folha/`, vedada pela ficha. O botão existente, se acionado, faz push e ocorrência. Contar e sinalizar são leituras. `app/folha/fora_da_folha.py:70–79` · `app/folha/retrato_folha.py:989–992,1015–1021,1042–1075` · `app/chamados/services/tarefa_do_dia.py:791–822,830–840` · `app/chamados/views_cobrar.py:24–57` · 

[truncado em 20000 de 27919 caracteres]

## 20/11 12:10 · sessao fbf66ab55a5a · FERIADO-POR-POSTO · verificador · origem=Code

**FERIADO-POR-POSTO — BLOQUEIA.** A marca pode ser desfeita por uma edição comum do posto, a chave pode voltar a divergir por caminhos normais de cadastro, e o `--apply` pode confirmar só parte da operação. A prévia usa a porta real e desfaz as escritas transacionais, mas deixa de fora o recálculo e outros efeitos que só ocorrem após o commit. Ela também pode anunciar pagamento para uma batida que pertence à jornada da véspera. `app/colaboradores/services/estrutura.py:66`, `app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/feriado_do_posto.py:286`, `app/colaboradores/services/feriado_do_posto.py:371`, `app/ponto/turnos.py:2902`

## Porta, cadastro e contador

1. **BLOQUEIA — A1, há outro escritor efetivo da marca.** O aval pede «uma função de serviço» e «`marcar_opera_em_feriado` passa a ter UM chamador». O gravador explícito tem esse chamador. O formulário, porém, carrega a linha inteira e `atualizar_posto` faz `posto.save()` sem restringir as colunas. O mesmo vale para `gravar_posto_ativo`. **Contraexemplo** → o formulário carrega `None`, o comando grava OPERA e propaga `True`, o formulário salva uma alteração de endereço. A marca volta a `None`, a chave permanece `True`, e o A4 deixa de acusar porque ignora posto sem marca. O dicionário do formulário pode omitir a marca e ainda assim ocorrer a sobrescrita. `app/colaboradores/views.py:733`, `app/colaboradores/views.py:855`, `app/colaboradores/views.py:879`, `app/colaboradores/views.py:899`, `app/colaboradores/services/estrutura.py:66`, `app/colaboradores/services/estrutura.py:76`, `app/colaboradores/services/estrutura.py:83`, `app/colaboradores/services/feriado_do_posto.py:218`

2. **BLOQUEIA — A1/C4, confirmação parcial, trilha facultativa e corrida.** O aval pede recusa de FECHA sem município, evento antes/depois, retratação pelo reconciliador, e propagação pela porta do vínculo com «UMA linha» da troca. A ordem está implementada. O plano é lido antes das escritas, a marca confirma num `atomic`, cada chamado em outro, e cada chave no `atomic` da porta do vínculo. **Contraexemplo** → OPERA em dois vínculos, o segundo falha durante a regeneração. Marca, primeira chave e efeitos do primeiro commit já podem ter ficado. Dois `--apply` do mesmo OPERA que leram `None` e chave `False` antes de qualquer escrita podem gravar dois eventos de posto e duas trilhas `False → True`. Um segundo ato sequencial com objetos frescos, chave já convergente e sem novos chamados elegíveis pula essas escritas. Há ainda o caso de `evento()` devolver `None` e o ato confirmar sem trilha. Um `--por` inexistente passa `autor=None` e também confirma. `app/colaboradores/services/feriado_do_posto.py:235`, `app/colaboradores/services/feriado_do_posto.py:263`, `app/colaboradores/services/feriado_do_posto.py:268`, `app/colaboradores/services/vinculo.py:868`, `app/colaboradores/services/vinculo.py:894`, `app/core/auditoria.py:17`, `app/core/observ.py:94`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139`

3. **CORRIGE — A2 e R3, reaplicar FECHA executa retratação.** A2 diz «marca não muda; só propaga aos vínculos divergentes». `reaplicar_marca` delega ao ato completo. **Contraexemplo** → posto já FECHA, zero divergentes, chamado vivo de feriado com célula já em folga. A reaplicação pode fechar o chamado e criar nota, embora a propagação seja zero. Ao fechar um posto cuja célula ainda diz trabalho, o gate pode recusar a retratação direta. Se a regeneração conseguir virar o dia, o fechamento pode vir pelo cartório/E2, sem nota da marca e com `retratados=0`. Se a célula de trabalho estiver editada, a regeneração a preserva e a retratação pode continuar recusada. O teste antigo do comando só confere o universo impresso. O teste novo confirma a distinção entre retratação da marca e fechamento pela célula. `app/colaboradores/services/feriado_do_posto.py:260`, `app/colaboradores/services/feriado_do_posto.py:295`, `app/colaboradores/services/feriado_do_posto.py:313`, `app/chamados/models.py:676`, `app/ponto/portas/celula.py:297`, `app/ponto/portas/celula.py:531`, `app/chamados/tests/test_jv1_volta5_posto_do_dia_nos_vetos.py:83`, `app/colaboradores/tests/test_feriado_por_posto.py:216`

4. **BLOQUEIA — A3, a fonte única termina na criação por cópia.** O item 4 do 1530 diz «os dois nascem pela marca», pela «MESMA função que a porta da marca e o contador A4 usam», com herança para posto sem marca e cargo administrativo. Os três sítios mantêm o posto da origem. O antigo `:974` continua copiando em `criar_trecho_retroativo`. O ajuste e o ramo que cria no desfazer chamam `chave_ao_nascer`. Porta e contador compartilham a seleção de elegíveis e o predicado de ciclo/cargo, mas decidem a chave esperada em expressões próprias. **Contraexemplos** → (a) o cadastro normal reaproveita um vínculo com chave `False`, troca seu posto por um OPERA e faz save mantendo `False`; (b) antes de um ajuste futuro entrar em vigor, a marca muda, e o desfazer apaga o novo e reabre o antigo sem reler a marca. A cláusula de nascimento foi aplicada ao ramo de criação, mas uma reabertura ainda pode deixar o A4 acima de zero. `app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:976`, `app/colaboradores/services/vinculo.py:1108`, `app/colaboradores/services/vinculo.py:1122`, `app/colaboradores/services/feriado_do_posto.py:174`, `app/colaboradores/services/feriado_do_posto.py:186`, `app/colaboradores/services/feriado_do_posto.py:218`, `app/colaboradores/services/feriado_do_posto.py:263`

5. **BLOQUEIA — A1/A3/A4, outro decisor manda na chave.** A exceção do aval é «cargo administrativo». O comando antigo decide por cargo para **todos** os `ativa=True` da empresa e chama o mesmo gravador. **Contraexemplo** → vigilante em 6×1 de posto FECHA, chave `False`. `sincronizar_feriado_admin --apply` decide `True`, grava e regenera. Há um gravador explícito para a troca, com dois decisores que produzem respostas opostas. Na criação normal, outro predicado lê a marca declarada sem receber cargo nem restringir a vitória da marca a weekday. Assim, um supervisor que nasce em OPERA pode ganhar `True` pela criação normal e ficar invisível para o A4. `app/colaboradores/management/commands/sincronizar_feriado_admin.py:19`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:38`, `app/colaboradores/services/vinculo.py:287`, `app/escala/models.py:905`, `app/colaboradores/services/feriado_do_posto.py:174`, `app/colaboradores/services/feriado_do_posto.py:213`

6. **CORRIGE — A4 e cron, vigia implementado, publicação ausente no caminho declarado.** O aval pede «esperado 0», estreia `11 == J`, dono **CADASTRO** e entrada no **ESTADO**. O comando conta OPERA×chave falsa e FECHA×chave verdadeira, imprime total e até 50 PKs, e é idempotente dado o mesmo dia e banco. Está em `FORA_DE_PIPELINE`, com papel de contador e dono **DP**. **Contraexemplo** → rodar o placar atual chama o contador antigo de 5×2/6×1 e deixa o A4 sem publicação por essa via. Como ambos os comandos da marca estão fora do agendamento, não há cron novo nem interruptor para desligá-lo. A vigência é inclusiva nas duas pontas e admite `ativa=False` com fim declarado que ainda cobre o dia. O A4 também admite colaborador inativo e posto inativo. A equivalência desse universo com `sonda_1300` está [nao verificado]. `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:1`, `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:14`, `app/config/crons.py:769`, `app/config/crons.py:1246`, `app/config/crons.py:1350`, `bin/placar_code.sh:296`, `app/colaboradores/services/feriado_do_posto.py:195`, `app/escala/alimentacao.py:199`

## Prévia, emissão e relógio

7. **CORRIGE — A5 e RESPOSTAS-2011-1100 item 3, a recusa padrão funciona.** O literal é «fora da sombra o comando RECUSA a prévia antes de chamar a porta». Com posto existente e argumentos válidos, `HASNER_SOMBRA=False` ou chave ausente → chamada direta levanta `PreviaForaDaSombra` antes do plano. O comando converte em `CommandError`, com saída não zero no CLI, depois de buscar o posto e imprimir o cabeçalho. `--sugestoes` sozinho continua sendo leitura de sugestões. Posto inexistente, combinação inválida de flags e FECHA sem município seguem outras saídas, com mensagem e retorno normal. **Contraexemplo à garantia de isolamento** → settings comuns com `HASNER_SOMBRA='false'` passam pelo teste de veracidade e chegam à porta. O guard verifica o sinalizador, enquanto banco lateral, credenciais, máscara e proxy dependem da configuração e do lançador. `--apply --motivo ...` pula a prévia e executa fora da sombra por desenho. O requisito do `!` até a tela é operacional. O comando não vincula a autorização nem exige que se tenha mostrado a prévia. `app/colaboradores/services/feriado_do_posto.py:348`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:39`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:99`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:109`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139`, `app/config/settings/sombra.py:47`, `bin/sombra.sh:182`, `bin/sombra_mascara.py:68`, `bin/sombra_mascara.py:84`

8. **CORRIGE — A5 item 1 e 1100 item 3, há emissão síncrona e efeito fora do banco.** A regeneração chama `julgar_celula(apply_=True, forcar=True)`. O cartório pode chamar `_abrir_chamado`, que tenta push para supervisão e pode repassar órfãos para disputa, com webpush e FCM ao colaborador antes do commit. **Contraexemplo** → feriado passado vira trabalho, há histórico de batidas, o dia tem furo emissível e o destinatário tem canal habilitado. O push pode sair e o rollback apaga o chamado que o motivou. A sombra configurada desativa os canais conhecidos. O lançador usa proxy morto sobre uma rede Docker e o guard da função não prova isolamento de rede. Também escapam logs e o contador em memória de exceções engolidas. Os INSERTs com IDs automáticos podem consumir valores de sequência PostgreSQL, algo que a foto das linhas não mede. A classe de push pré-commit está descrita na docstring. O registro na O-OUTBOX exigido pelo aval está [nao verificado]. `app/ponto/portas/celula.py:460`, `app/ponto/services/cartorio.py:1838`, `app/ponto/services/cartorio.py:2059`, `app/ponto/management/commands/detectar_ausencias.py:775`, `app/ponto/management/commands/detectar_ausencias.py:788`, `app/chamados/services/disputa_emissao.py:1303`, `app/chamados/services/disputa_emissao.py:1789`, `app/chamados/push_utils.py:24`, `app/chamados/fcm_utils.py:85`, `app/core/observ.py:73`, `app/config/settings/base.py:104`, `app/config/settings/saas.py:83`, `app/colaboradores/services/feriado_do_posto.py:329`

9. **BLOQUEIA — A5/C3, o rollback mede só parte do ato real.** A prévia faz duas fotos de `CelulaDia.trabalha` para os colaboradores do plano externo e chama a porta num `atomic` desfeito. Caem junto os callbacks de recálculo, rejunção, falta espúria, geofence, beco da disputa e reconciliação de pai. A fila `EventoCelula` é escrita na mesma transação e o consumidor agendado lê as linhas confirmadas. **Contraexemplo** → uma célula muda DNA, ata ou geradora e conserva `trabalha=True`. A resposta diz zero «outras células», enquanto o apply pode rejulgar, emitir e recalcular a competência. Outro caso → `Posto` ativo com disputa aberta de motivo `posto_desativado`. Salvar a marca dispara a revalidação por `post_save`, mesmo sem reativação, e pode alterar disputa e chamado fora dos totais exibidos. O plano da resposta ainda pode divergir do plano usado no ato, pois é calculado antes do `atomic` com o objeto original e, no ato, recalculado com um posto relido. `app/colaboradores/services/feriado_do_posto.py:360`, `app/colaboradores/services/feriado_do_posto.py:371`, `app/colaboradores/services/feriado_do_posto.py:383`, `app/ponto/portas/celula.py:297`, `app/ponto/portas/celula.py:483`, `app/ponto/portas/celula.py:528`, `app/ponto/portas/celula.py:945`, `app/ponto/signals.py:291`, `app/colaboradores/signals.py:49`, `app/chamados/signals.py:229`, `app/chamados/signals.py:351`, `app/config/crons.py:393`

10. **BLOQUEIA — A5 item 2 e 1530 item 2, a prévia confunde dia civil e dia da jornada.** O aval posterior permite «batidas cruas + nome da regra + “sem apuração ainda”». O rótulo existe. A escolha do grupo `(a)` usa qualquer batida apurável no intervalo civil inclusivo `00:00:00` a `23:59:59.999999` e depois imprime todas as cruas ali, inclusive retratadas. **Contraexemplo** → turno iniciado às 22h da véspera, com saída às 06h no feriado, sem turno iniciado no feriado. A prévia põe a saída em «PASSAM A PAGAR» no feriado. O juiz atribui a saída à jornada da véspera e a dobra segue o início do plantão. Uma batida órfã isolada também basta para entrar em `(a)`. Hoje sem batida é sempre «futuro», mesmo com a jornada já vencida. O fuso é o corrente do Django, cujo padrão é São Paulo, e o corte de competência é o da empresa. São três referências distintas. `app/colaboradores/services/feriado_do_posto.py:381`, `app/colaboradores/services/feriado_do_posto.py:391`, `app/ponto/turnos.py:2872`, `app/ponto/turnos.py:2902`, `app/core/juizes.py:215`, `app/ponto/motor_calculo_v2.py:914`, `app/config/settings/base.py:93`, `app/ponto/janelas.py:36`

11. **CORRIGE — relógio e ordem de trava.** O comando passa um `hoje` à porta. `qs_na_fila()` recebe só os defaults e lê o instante por dentro, a porta de vínculo conserva `localdate()` para o chamador legado, e o recálculo da célula e o cartório relêem o dia. **Contraexemplo** → começar pouco antes da meia-noite e cruzá-la entre plano e aplicação. O recorte de vínculo continua no dia injetado enquanto o universo de silenciados e o julgamento podem mudar. O diff não acrescenta `select_for_update` explícito. Na prévia, o `atomic` externo retém as travas existentes dos chamados e das células futuras até o rollback. O laço percorre vínculos por PK do vínculo e não adquire previamente todos os colaboradores por PK. Dois lotes sobrepostos podem então chegar a colaboradores em ordens opostas. A catraca já documenta a aquisição do conjunto no topo para transação com mais de um colaborador. `app/colaboradores/management/commands/marcar_feriado_do_posto.py:109`, `app/colaboradores/services/feriado_do_posto.py:209`, `app/colaboradores/services/feriado_do_posto.py:251`, `app/colaboradores/services/feriado_do_posto.py:371`, `app/chamados/services/painel.py:158`, `app/colaboradores/services/vinculo.py:903`, `app/ponto/portas/celula.py:460`, `app/ponto/services/cartorio.py:2064`, `app/ponto/services/cadastro_futuro.py:8`, `app/chamados/models.py:2816`, `app/chamados/models.py:2841`

## Dinheiro e lavra

12. **BLOQUEIA — C2, 1530 item 3 e 1100 item 4, o carimbo muda em competência protegida e a guarda tem alcance específico.** O literal é manter `competencia_fechada_para_celula` como autoridade e «registrar sem curar» a invalidação geral. A regeneração consulta TXT vigente ou holerite publicado e barra o dia da janela protegida. O save da chave dispara `invalidar_previsto` antes da guarda, em **todas** as competências carimbadas do colaborador. **Contraexemplo** → 10/2026 com TXT vigente e `previsto_em` preenchido. A troca em novembro mantém a célula barrada, mas grava `previsto_em=NULL` e outro `atualizado_em` em outubro. A prontidão passa a ler a grade viva. Para 10/2026 paga fora do sistema, sem TXT vigente e sem holerite publicado, a guarda da cópia não vê a quitação descrita no corte. Com corte 21 e ato em 20/11, 12/10 ainda entra na janela anterior. Em 21/11, a regeneração ordinária salta para a anterior de novembro nessa empresa. O signal continua podendo atingir outubro. `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1607`, `app/folha/export.py:597`, `app/folha/export.py:1045`, `app/ponto/management/commands/gerar_celulas.py:135`, `app/ponto/portas/celula.py:218`, `app/ponto/janelas.py:66`, `app/docs/CORTES.md:529`

13. **BLOQUEIA — R5/C2 e R7, o gravado também pode mudar, e o ato não tem reversão integral.** **Contraexemplos** → (a) competência aprovada ou paga fora do sistema, sem TXT/holerite na guarda, com feriado que vira trabalho. O callback recalcula `FechamentoMensal`, inclusive números de linhas aprovadas, e substitui `DiaPago`; (b) o TXT é registrado entre a leitura da guarda de células e o recálculo do ato. A guarda já foi lida, e o escritor do fechamento avisa sobre exportação e segue, salvo a proteção nominal de 09/2026; (c) FECHA retrata um chamado vivo de feriado cuja célula exportada já é folga. O `post_save` do chamado pode chamar o cartório e relavrar a ata por um caminho distinto da guarda de regeneração. As portas existentes de reversão restauram o DNA anterior de um dia ou campos conhecidos de um fechamento. A porta da marca não salva uma foto que permita repor em conjunto chaves, dias pagos, chamados e efeitos externos. Inverter a marca é outro ato e pode gerar novos efeitos. `app/colaboradores/services/feriado_do_posto.py:295`, `app/chamados/signals.py:189`, `app/ponto/services/cartorio.py:2059`, `app/ponto/portas/celula.py:221`, `app/folha/views.py:299`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:539`, `app/ponto/services/fechamento.py:1160`, `app/ponto/services/fechamento.py:1358`, `app/ponto/services/fechamento.py:1798`, `app/ponto/services/dia_pago.py:359`, `app/ponto/portas/celula.py:779`

14. **BLOQUEIA — A1/C3, a regeneração passada pode tomar uma célula de outro vínculo.** A1 seleciona por «vínculos weekday vigentes» e a juíza do vínculo do dia dá precedência à geradora válida. **Contraexemplo** → dois vínculos cobrem D, o vínculo A é do posto marcado e a célula de D tem geradora B, de outro posto. Na regeneração passada basta A cobrir a data. A diferença de geradora é, ela própria, motivo para regravar a célula com A. O suplemento futuro consulta a juíza e exclui dia que tem outro dono. A prévia agrupa pelo primeiro vínculo registrado por colaborador para rotular a regra, sem consultar a geradora da célula alterada. `app/colaboradores/services/feriado_do_posto.py:195`, `app/colaboradores/services/feriado_do_posto.py:360`, `app/colaboradores/services/feriado_do_posto.py:391`, `app/escala/alimentacao.py:228`, `app/ponto/portas/celula.py:225`, `app/ponto/portas/celula.py:289`, `app/ponto/services/cadastro_futuro.py:43`

## Mapa dos escritores e leitores

| Dado | Quem escreve | Quem lê e para quê |
|---|---|---|
| `Posto.opera_em_feriado` | Gravador da marca, chamado por `aplicar_marca`. Saves inteiros de editar/ativar também persistem a coluna. O criador genérico aceita `**dados`, e o formulário atual não inclui essa chave. `app/colaboradores/services/estrutura.py:43`, `app/colaboradores/services/estrutura.py:66`, `app/colaboradores/services/estrutura.py:76`, `app/colaboradores/services/estrutura.py:83`, `app/colaboradores/services/feriado_do_posto.py:290`, `app/colaboradores/views.py:855` | Veto de cobrança, sugestões e pauta, padrão de criação, contador antigo, plano/A2/A4 e saída do comando. `None` significa sem declaração. `app/colaboradores/models.py:256`, `app/colaboradores/services/feriado_do_posto.py:60`, `app/colaboradores/services/feriado_do_posto.py:125`, `app/colaboradores/services/feriado_do_posto.py:136`, `app/colaboradores/services/feriado_do_posto.py:186`, `app/colaboradores/services/feriado_do_posto.py:218`, `app/colaboradores/services/feriado_do_posto.py:263`, `app/colaboradores/services/feriado_do_posto.py:313`, `app/escala/models.py:905`, `app/escala/models.py:919`, `app/ponto/management/commands/detectar_ausencias.py:616`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:106` |
| `EscalaColaborador.trabalha_em_feriado` | Troca pela porta, com chamadores marca e sincronizador. Nascimento normal, trecho retroativo, ajuste e desfazer, além do default do campo. Reaproveitamento por save inteiro preserva e pode repersistir valor velho. `app/col

[truncado em 20000 de 32779 caracteres]

## 20/11 12:04 · sessao 4d65a86aeb77 · E4C-F5-CONSTROI · construtor · origem=Code

O K47 conserva a medida de 3×6. As escolhas nascem pela porta de conversa e recebem resposta e validação pelos escritores reais. A guarda lê a composta em lote antes de excluir as validadas, e `self._comparar` continua exigindo a leitura de todos os PKs e o mesmo número positivo de SELECTs de decisões nas duas rodadas. [test_e4c_f5_voltaB_lotes.py:197](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197) [test_e4c_f5_voltaB_lotes.py:40](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaB_lotes.py:40) [disputa_emissao.py:2091](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:2091)

### Mutações de produção para a sessão

- **V — validação.** Em `_carimbar_validacao`, trocar `perg.save(update_fields=['validada_em', 'validada_por'])` por `pass`. A linha `VALIDACAO` é gravada e a coluna fica vazia. [validacao.py:171](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:171)
- **R — nova resposta.** Em `responder_pergunta_disputa_core`, trocar `resposta_mantem_validacao=False` por `resposta_mantem_validacao=True`. A coluna é limpa e a composta conserva a validação anterior. [disputa_emissao.py:1506](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1506) [disputa_emissao.py:1531](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:1531)
- **D — devolução.** Em `devolver_validacao_recusada`, trocar `perg.save(update_fields=['validada_em', 'validada_por'])` por `pass`. A linha `DEVOLUCAO` limpa a validação composta e a coluna continua preenchida. [validacao.py:290](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/validacao.py:290) [models.py:4347](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/models.py:4347)
- **K47 sem lote.** O seletor existente retira a segunda chamada a `.prefetch_related('decisoes')` de `_resolver_perguntas_por_chamado_resolvido`. [e4c_f5_voltaB_mutantes.py:23](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaB_mutantes.py:23) [disputa_emissao.py:2091](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/disputa_emissao.py:2091)

### Esperados por teste [nao verificado]

Na volta D, a asserção comum relê a linha e compara a presença da validação composta com a presença de `validada_em`. A coluna também é conferida contra o instante fixo esperado. [test_e4c_f5_voltaD_portas.py:45](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:45)

| Teste | Esperado antigo, no literal anterior à edição | Esperado novo | Mutante | [nao verificado]
|---|---|---|---|
| `test_RED_k47_escolhas_sem_credor` | `self.assertEqual(set(carimbadas.values_list('pk', flat=True)), pks)` · volta B:213 anterior, também em `k47_falha.log:102-117` | Par preenchido. A disputa fecha e nenhuma escolha já validada recebe carimbo de materialização. A medida de lote continua em `self._comparar`. [B:197](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197) | V e K47 sem lote |
| `test_RED_corrigir` | `self.assertEqual(tipo, 'info' if ato_validado else 'sucesso')` · D:79 anterior | Com validação vigente, preserva `07:13` e o par original. Após nova resposta, corrige para `07:18` e valida com o par preenchido em `AGORA`. [D:84](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:84) | R |
| `test_RED_declaracao` | `self.assertEqual(resultado['estado'], 'validada_sem_registro')` e `self.assertTrue(resultado['ok'], resultado)` · D:102 e 106 anteriores | A validação vigente preserva `07:13`. A declaração aceita após a nova resposta grava `07:18` e mantém o par vazio. [D:103](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:103) | R |
| `test_RED_aplicar_declaracao` | `self.assertEqual(resultado['estado'], 'validada_sem_registro' if ato_validado else 'validada')` · D:127 anterior | Preserva a validação vigente ou aplica e valida pelo caminho real, com o par preenchido e o histórico esperado. [D:122](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:122) | V |
| `test_RED_fecho` | `self.assertIsNone(retida.materializada_em)` e `self.assertEqual(aberta.materializada_em, AGORA)` · D:147-148 anteriores | Retém a pergunta com o par preenchido. O carimbo da outra pergunta preserva o par vazio que a nova resposta produziu. [D:142](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:142) | R |
| `test_RED_fecho_3x6` | `self.assertEqual(set(disputa.perguntas.filter(materializada_em__isnull=True).values_list('pk', flat=True)), {p.pk for p in perguntas})` e `self.assertEqual(medidas, [1, 1])` · D:162-164 anteriores | Todas as perguntas têm o par preenchido e ficam sem carimbo de materialização. Mantém `[1, 1]`. [D:155](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:155) | V e `fecho_sem_lote` |
| `test_RED_propostas_3x6` | `self.assertTrue(all(not p['aceita'] for p in propostas))` e `self.assertEqual(medidas, [1, 1])` · D:179-181 anteriores | O par preenchido vem da validação real. Todas as propostas continuam recusadas, com `[1, 1]`. [D:172](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:172) | V |
| `test_RED_tela_3x6` | `self.assertEqual(resultado, esperados)` e `self.assertEqual(medidas, [1, 1])` · D:203-205 anteriores | Alterna validação vigente e validação desfeita por nova resposta. A tela mantém a aceitação esperada por dia e `[1, 1]`. [D:189](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:189) | R e `tela_sem_lote` |
| `test_RED_irma_plantio` | `self.assertEqual((irma, par, impedimento), (q, (AGORA + timedelta(hours=1), 'E'), None))` ou `self.assertEqual((irma, par), (None, None))` com `self.assertIn('respondida, sem validacao', impedimento or '')` · D:226-229 anteriores | A irmã com o par preenchido é aceita. Após a nova resposta, o par fica vazio e a guarda informa falta de validação. [D:228](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:228) | R |
| `test_RED_irma_implausivel` | `self.assertEqual((irma, par, impedimento), (q, (AGORA + timedelta(hours=1), 'E'), None))` ou `self.assertEqual((irma, par), (None, None))` com `self.assertIn('hora implausivel', impedimento or '')` · D:236-239 anteriores | A irmã com validação vigente é aceita. Com o par vazio após a nova resposta, a hora implausível impede o par. [D:238](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:238) | R |
| `test_RED_materializar_ato_mudou_sob_trava` | `self.assertEqual(resultado['materializadas'], int(ato_validado))` e `self.assertEqual(p.materializada_em, AGORA if ato_validado else None)` · D:269-270 anteriores | Sob a trava real, outra instância responde e limpa o par. Só o ramo que revalida materializa. [D:248](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:248) | R |
| `test_RED_validar_par` | `self.assertEqual(self._validacoes(q), [VALIDACAO_EM] if ato_validado else [VALIDACAO_EM, AGORA])` e `self.assertEqual(set(disputa.perguntas.filter(materializada_em__isnull=False).values_list('pk', flat=True)), {p.pk, q.pk})` · D:287-289 anteriores | As duas metades materializam pelo caminho real e terminam com o par preenchido. A irmã ganha nova validação apenas quando a resposta anterior desfez a original. [D:272](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:272) | R |
| `test_RED_validar` | `self.assertEqual(resultado['estado'], 'ja_validada' if ato_validado else 'validada')` · D:304 anterior | `ja_validada` preserva o par original. `ja_no_espelho` é o resultado esperado no ramo que revalida, reutiliza a batida da fixture e preenche o par em `AGORA`. [D:300](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:300) | V |
| `test_RED_recusar` | `self.assertEqual(resultado['estado'], 'ja_validada' if ato_validado else 'recusada')` e `self.assertEqual(D.objects.filter(pergunta=p, ato=D.RECUSA).count(), int(not ato_validado))` · D:315-316 anteriores | A recusa preserva a validação vigente ou, depois de uma nova resposta, mantém o par vazio e grava a recusa. [D:312](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:312) | R |
| `test_RED_lote_entrada` | `self.assertEqual(resultado['recusados'], recusados)` e `self.assertEqual(resultado['validadas'], int(not ato_validado))` · D:329-330 anteriores | O lote recusa `Ja validada.` quando há par preenchido e valida de novo quando a resposta anterior limpou o par. [D:324](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:324) | V |
| `test_RED_lote_entrada_3x6` | `self.assertEqual(resultado['recusados'], [{'ref': ref, 'motivo': 'Ja validada.'} for ref in refs])` e `self.assertEqual(medidas, [1, 1])` · D:341-343 anteriores | Todos os pares preenchidos vêm do escritor real. Mantém as recusas por PK e `[1, 1]`. [D:338](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:338) | V e `lote_entrada_sem_prefetch` |
| `test_RED_lote_rele_ato_escrito_em_outra_instancia` | `self.assertEqual(D.composta(p).validada_em, AGORA if ato_validado else None)` e `self.assertEqual(resultado, {'validadas': int(ato_validado), 'erros': [], 'nao_gravados': [{'ref': p.pk, 'motivo': 'Sem plantio'}] if ato_validado else [], 'recusados': [] if ato_validado else [{'ref': p.pk, 'motivo': 'Passou pela porta'}]})` · D:364-369 anteriores | O lote relê a ação real. Conta a validação mantida sem plantio e informa o aviso em `nao_gravados`. Quando a recusa devolve a validação, o par termina vazio e o aviso entra em `recusados`. [D:350](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:350) | D | [nao verificado]
| `test_devolucao_e_o_escritor_da_coluna` | `self.assertIs(resultado, True)`, `self.assertIsNone(p.validada_em)` e `self.assertEqual(D.objects.filter(pergunta=p, ato=D.DEVOLUCAO).count(), 1)` · D:390-392 anteriores | Começa com o par preenchido. A devolução real deixa as duas leituras vazias e grava uma linha `DEVOLUCAO`. [D:388](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:388) | D |

`test_diagnostico_refresh_parcial_e_integral` ficou como estava. Seu `update` permite distinguir o cache preservado pela releitura parcial do cache descartado pela releitura integral. As asserções consultam o cache e a composta. [D:40](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:40) [D:376](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_voltaD_portas.py:376)

### Comandos para a sessão

O bloco usa `manage.py` e o executor de mutações em memória da volta D. Cada chamada de `mutar` roda os testes indicados com uma única troca de produção. [manage.py:7](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/manage.py:7) [e4c_f5_voltaD_mutantes.py:49](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaD_mutantes.py:49) [nao verificado]

```bash
b=chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest
d=chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest
k47="$b.test_RED_k47_escolhas_sem_credor"

PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "${b%.*}" "${d%.*}"

PYTHONDONTWRITEBYTECODE=1 F5_VOLTA_B_MUTANTE=k47 \
  python3 manage.py test "$k47"
PYTHONDONTWRITEBYTECODE=1 F5_VOLTA_D_MUTANTE=fecho_sem_lote \
  python3 manage.py test "$d.test_RED_fecho_3x6"
PYTHONDONTWRITEBYTECODE=1 F5_VOLTA_D_MUTANTE=tela_sem_lote \
  python3 manage.py test "$d.test_RED_tela_3x6"
PYTHONDONTWRITEBYTECODE=1 F5_VOLTA_D_MUTANTE=lote_entrada_sem_prefetch \
  python3 manage.py test "$d.test_RED_lote_entrada_3x6"

mutar() {
  PYTHONDONTWRITEBYTECODE=1 F5_VOLTA_D_MUTANTE="$1" python3 - "$@" <<'PY'
import os
import sys

import django
from django.core.management import execute_from_command_line

from chamados.tests.e4c_f5_voltaD_mutantes import SITIOS, mutar_sitio

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "config.settings.saas")
django.setup()
nome, *testes = sys.argv[1:] [nao verificado]
save = "perg.save(update_fields=['validada_em', 'validada_por'])"
SITIOS.update({
    "V": ("chamados.services.validacao", "_carimbar_validacao", save, "pass"),
    "R": ("chamados.services.disputa_emissao", "responder_pergunta_disputa_core",
          "resposta_mantem_validacao=False", "resposta_mantem_validacao=True"),
    "D": ("chamados.services.validacao", "devolver_validacao_recusada", save, "pass"),
})
with mutar_sitio(nome):
    execute_from_command_line(["manage.py", "test", *testes]) [nao verificado]
PY
}

mutar V "$k47" "$d.test_RED_aplicar_declaracao" "$d.test_RED_fecho_3x6" \
  "$d.test_RED_propostas_3x6" "$d.test_RED_validar" \
  "$d.test_RED_lote_entrada" "$d.test_RED_lote_entrada_3x6"

mutar R "$d.test_RED_corrigir" "$d.test_RED_declaracao" "$d.test_RED_fecho" \
  "$d.test_RED_tela_3x6" "$d.test_RED_irma_plantio" "$d.test_RED_irma_implausivel" \
  "$d.test_RED_materializar_ato_mudou_sob_trava" "$d.test_RED_validar_par" \
  "$d.test_RED_recusar"

mutar D "$d.test_RED_lote_rele_ato_escrito_em_outra_instancia" \
  "$d.test_devolucao_e_o_escritor_da_coluna"
```

**PAREI da classe vii.** Nenhum impedimento de forma identificado nos dois arquivos. Compilei a sintaxe e conferi as três trocas em memória. O verde da suíte, as falhas dos mutantes nas asserções indicadas e os valores medidos dos SELECTs ficam para a sessão e o opus final. [nao verificado] [nao verificado]

## 20/11 11:59 · sessao ff2371be9831 · TRAVAS-SEM-MEMORIA · verificador · origem=Code

## Achados

1. **BLOQUEIA — M3 e M9a, o hook existe, mas o registro falta na cópia.** A V4 manda registrar um `PreToolUse` de Bash para recusar a inteira sem PRONTO e a ficha sem leitura antes da execução. O script implementa os dois ramos, mas o único `PreToolUse` entregue chama `disjuntor.py pretool`. Uma chamada Bash canônica com `FICHA X` e `DECISAO usar A` não recebe a recusa de M9a pelo settings entregue. Já a inteira canônica sem PRONTO continua barrada pelo executor. A instalação local e a carga na sessão viva ficam [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47,58,88–91`, `.claude/settings.json:62–73`, `bin/disjuntor.py:1837–1863`, `bin/hook_pista_pronto.py:198–222`, `bin/suite.sh:300–303`.

2. **BLOQUEIA — M2 e M3, a rota decide se é inteira olhando uma árvore diferente da que a pista recebe.** A V4 exige seleção efetiva e árvore commitada. O código descobre testes no disco de `--dir`, usa `git status` sem consultar ignorados e manda o SHA para a pista. Contraexemplo de fixture: em H, todos os testes commitados estão em `appA`. No disco, `appB` tem um `test_local.py` ignorado pelo Git. Com `PISTA_LATERAL=O`, `--only appA` e `--motivo 'pre-opus O'`, o executor vê um recorte, pula o PRONTO e envia `pede H --only appA`. Em H, isso cobre todos os testes. Também basta `--dir` conter `app/` e o Git encontrar um repositório ancestral. A porta não confirma que o diretório é a raiz da cópia cujo HEAD foi lido. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45–47,65`, `bin/suite.sh:237–255,290–305,315–318`.

3. **BLOQUEIA — M4, na contingência VM o SHA conferido pode deixar de ser o SHA executado.** Com `PISTA_CONTINGENCIA='pista fora'`, cópia limpa em H e PRONTO de H, a suite confere H, monta o caminho vivo e espera a trava. Se a cópia avança para J durante a espera, o Docker recebe a árvore de J sem nova conferência de HEAD ou PRONTO. O modo `--na-vez` limpa montagens e executa o comando recebido. O RELATO já descrevia essa janela. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47–48`, `bin/suite.sh:296–303,312–328,361–374,394–403,108–120`, `bin/trava_teste.sh:35–42`, `app/docs/RELATO.md:1209`.

4. **BLOQUEIA — M4, a espera pode devolver verde sem provar que rodou o job pedido.** O contrato diz que `ssh pista espera <id>` sai com o rc da suite. O executor descarta o status desse processo, usa o primeiro `rc=` no texto e trata `Ran` como opcional. Uma espera de fixture que imprime `rc=0`, sem `Ran`, e sai `255` faz a suite sair `0`. O selo já aceita `sha=x` na resposta para qualquer H, pois não compara o SHA devolvido. Se a espera falhar sem `rc=`, sai `71` também para recorte, sem a contingência que existe quando `pede` falha. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:48,64,73–75`, `app/docs/BACKLOG.md:2191`, `bin/suite.sh:317–339`, `bin/tests/test_pista_lateral.sh:24–33,93–102,120–121`.

5. **CORRIGE — M2, a rota nominal funciona pelo executor, com lacunas no despacho.** Para lateral e espinha, a inteira canônica exige a chave de mesma obra, a cópia e o marcador. A volta herda a chave, mantém ruff e selo e passa `--volta` à suite. Pouso sem a chave segue pela VM, e régua e pre-push têm runners próprios. Divergências: o comando canônico ainda ensinado em `CLAUDE.md` omite chave e `--dir`, então a chamada copiada sai `2`. Na pista, `--parallel auto` é encaminhado embora o contrato documente `N`, e `--espera 1` não limita `pede` nem `espera`. Aceitação de `auto` pelo servidor [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45–46`, `respostas_f5_travas_2011_1025.txt:12`, `CLAUDE.md:95–106`, `bin/suite.sh:140–145,282–305,317–331,394–403`, `bin/volta_intermediaria.sh:18–38`, `bin/regua.sh:165–169`, `bin/pre-push.sh:185–199`, `app/docs/BACKLOG.md:2191`.

6. **CORRIGE — M3, o hook e o executor interpretam argv de formas diferentes.** A V4 pede recusa antecipada da inteira efetiva. Para `--only 'appA appB'` cobrindo tudo, o hook passa só pela presença do flag, enquanto o executor exige PRONTO se o motivo e a chave forem válidos. Com `--motivo 'pre-opus X' --motivo 'pre-opus O'` e chave `O`, o hook pode negar pelo primeiro valor, embora o executor use o último. Com `cd /outra-raiz && ... --dir wt`, o hook resolve `wt` pelo `cwd` inicial do payload. E `CODEX_PAPEL=leitura CODEX_OBRA=X-RASCUNHO echo bin/codex_pergunta.sh` recebe recusa de ficha se o novo hook estiver carregado, porque o nome aparece entre as quatro primeiras palavras. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47,58,98–100`, `bin/hook_pista_pronto.py:45–95,108–133,163–176,198–208`, `bin/suite.sh:135–151,237–264,282–303`.

7. **CORRIGE — M4, o marcador é verificado, mas a contingência e a suspensão continuam a cargo da sessão.** O juiz aceita o cabeçalho em qualquer dos dois RELATOs e confere obra e SHA. Itens, medidas, pendências e subida após mais de três voltas são obrigações da sessão. Um PRONTO arquivado com pendência passa, como pedido. Já `PISTA_CONTINGENCIA=qualquer-texto` com PRONTO válido manda à VM mesmo com a pista saudável. Após uma divergência no mesmo SHA, repetir o pedido ainda pode enfileirar novo job. O código imprime causa e grava telemetria, mas não lê suspensão nem verifica ocorrência anterior de falha. O registro e a suspensão pela sessão ficam [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45,48,65,73,75,107`, `bin/pronto_item_a_item.py:8–11,30–46`, `bin/suite.sh:288,300–339`, `CLAUDE.md:372–381`, `app/docs/BACKLOG.md:2191`.

8. **CORRIGE — M5, o medidor publica HEAD, mas a identidade da cópia pode estar errada.** A V4 pede `git rev-parse HEAD` em cada worktree da união, inclusive a só suja. O medidor reutiliza o HEAD de `worktree list --porcelain` e verifica apenas que o caminho é diretório e o valor tem 40 caracteres diferentes do caso todo zero. Se uma worktree externa foi removida e seu diretório recriado como pasta comum antes de `prune`, o metadado antigo pode render `copia=...@H12` embora ali não haja HEAD resolvível. Duas só sujas em `/x/a/wt` e `/y/a/wt` também recebem o mesmo nome `a/wt`. A composição dobra ambas por esse nome e pode imprimir duas vezes o HEAD de uma delas. Uma declaração `wt.agente` é atribuída a todas as worktrees de basename `wt`, inclusive uma limpa sem relação demonstrada com a obra. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:49,81`, `bin/raias_estado.py:73–133,146–172,194–199`, `bin/raia_agente.sh:40–59`.

9. **CORRIGE — M5, a publicação pode trocar o identificador que o medidor produziu.** O gerador usa o compositor comum para `copia=<nome>@<HEAD12>` e `raia=<nome> SEM WORKTREE`, e `relato.sh` publica após raspagem. Com o nome sintético `wt-ZEBRANTE` e a lista de nomes da própria fixture, a saída gerada tem `copia=wt-ZEBRANTE@H12`, e a raspada tem `copia=wt-[nome]@H12`. O selo usa a lista sintética, mas seus nomes de worktree não colidem com ela. Colisão na publicação real [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:49,81`, `bin/raias_estado.py:169–199`, `bin/gerar_estado.py:211–224,398`, `bin/relato.sh:114–147`, `bin/raspar_publico.py:24–40`, `bin/nome_no_texto.py:56–84,114–121`, `bin/tests/test_travas_sem_memoria.sh:40–63,87–96`.

10. **BLOQUEIA — M9a, a existência do `.id` vale como procedência, e a alternativa pelo RELATO falta.** A V4 aceita entrega Codex identificável pela resposta e sidecar ou pelo registro de entrega no RELATO. `r.txt` com uma linha e `r.txt.id` vazio passa, exatamente como na fixture. O mesmo `r.txt`, com entrega registrada no RELATO e sem `.id`, é recusado. Na fonte de `main`, `DECISAO usar A | bin:1` pode passar pela listagem da árvore de `bin`, tratada como se fosse arquivo. `arquivo:1-999` confere só a linha `1`. A ficha também é reaberta pelo wrapper. Se o conteúdo mudar após a passagem pelo hook, o wrapper consome a versão nova. A V4 atribui à sessão fechar a versão, conferir o trecho, registrar `PAREI RASCUNHO` e seguir a fila. A execução desse rito fica [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:57–58,82–83`, `bin/hook_pista_pronto.py:41–42,136–195`, `bin/codex_pergunta.sh:99–100,323–335,464–469`, `bin/tests/test_pista_lateral.sh:144–155`.

## Escritores, leitores e significados

| Estado, ambiente ou ref | Quem escreve e quem lê |
|---|---|
| `app/docs/RELATO.md` e `RELATO-ARQUIVO.md` | A sessão publica o PRONTO e arquiva pelo rito. `tem_pronto` lê os dois. No RELATO vivo, `esteira_vigia` escreve pela porta com trava e o disjuntor a usa. `gerar_cortes.py` e `deploy_agendado.sh` reescrevem o mesmo arquivo fora dessa porta, condição anterior ao diff. O Stop lê o topo e `relato.sh` lê o vivo para publicar outra cópia. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47–48`, `CLAUDE.md:439–442`, `bin/pronto_item_a_item.py:20,30–46`, `app/core/esteira_vigia.py:834–862,865–892,915–927`, `bin/disjuntor.py:293–328`, `bin/gerar_cortes.py:114–136`, `bin/deploy_agendado.sh:31–45`, `bin/hook_stop_fila1.py:461–488`, `bin/relato.sh:54–56,114–119`. |
| Ficha, `r.txt` e `<resposta>.id` | A sessão entrega a ficha, e hook e wrapper a abrem separadamente. O wrapper põe a resposta no caminho indicado e escreve o `.id` quando há resposta nova com rc `0`. Para o disjuntor, o sidecar identifica a chamada e o SHA-256 da resposta. Para M9a, basta existir como arquivo. Esse é um significado divergente do mesmo estado. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:58`, `bin/codex_pergunta.sh:57–67,323–335,402–430,464–469`, `bin/disjuntor.py:855–862,896–904`, `bin/hook_pista_pronto.py:136–148,171–192`. |
| `logs/raias/*.agente`, HEADs e `main` | `raia_agente.sh` abre, altera e fecha declarações, além de manter `historico.log`. O medidor lê `.agente` pelo nome do arquivo e não consulta o histórico. As operações Git da sessão dão os HEADs e atualizam `main`. Suite e hook consultam `HEAD^{commit}`, o medidor usa o inventário de worktrees e `main..HEAD`, e M9a lê `main:<path>` a cada referência. O selo cria seus próprios refs locais. `bin/raia_agente.sh:30–31,40–81`, `bin/raias_estado.py:42–69,73–97,118–133`, `CLAUDE.md:91`, `bin/suite.sh:296–297`, `bin/hook_pista_pronto.py:98–105,149–159`, `bin/tests/test_travas_sem_memoria.sh:30–63,112–128`. |
| `ESTADO.md`, `DEST_RELATO` e `logs/pista_lateral.log` | O gerador e `raia_agente.sh listar` leem o medidor. `relato.sh` gera o temporário, pode antepor aviso de retenção, raspa para `DEST_RELATO` e publica em `origin HEAD`. O selo põe `DEST_RELATO` no ambiente e lê `HEAD:ESTADO.md` do seu origin. A suite acrescenta a telemetria da pista, lida pelo selo. Um leitor operacional desse novo log fica [nao verificado]. `bin/gerar_estado.py:215–224,398`, `bin/raia_agente.sh:80–81`, `bin/relato.sh:23,37–38,54–75,114–147`, `bin/raspar_publico.py:24–40`, `bin/suite.sh:288,301–302,323–338`, `bin/tests/test_travas_sem_memoria.sh:70–83`, `bin/tests/test_pista_lateral.sh:97`. |
| `refs/pista/<SHA>` | A suite escreve a ref. A rota manual documentada também manda a sessão ou raia criá-la diretamente. Esse é um segundo caminho de escrita fora da porta. O selo verifica e apaga refs da fixture. A leitura pela pista está documentada, e o executor remoto fica [nao verificado]. `bin/suite.sh:315–318`, `app/docs/BACKLOG.md:2191`, `app/docs/RELATO.md:201`, `bin/tests/test_pista_lateral.sh:60,96`. |
| `PISTA_LATERAL`, `PISTA_CONTINGENCIA`, `HOOK_PISTA_RAIZ`, `CODEX_OBRA` e `CODEX_PAPEL` | A sessão fornece obra e rota. A suite lê o ambiente efetivo, enquanto o hook extrai atribuições do texto Bash. `PISTA_CONTINGENCIA` só é lida pela suite. `HOOK_PISTA_RAIZ` redireciona o leitor do hook e é preenchida pelo selo. Os papéis e a obra Codex também são lidos pelo wrapper e disjuntor, e o Stop consulta o ambiente de processos Codex. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45–46,58,63`, `bin/suite.sh:282–303,312–325`, `bin/hook_pista_pronto.py:45–75,108–130,163–171,215`, `bin/tests/test_pista_lateral.sh:62–74,127–130`, `bin/codex_pergunta.sh:80–84`, `bin/disjuntor.py:1448–1463`, `bin/hook_stop_fila1.py:726–770`. |

**Há um só leitor do marcador de PRONTO**, `tem_pronto`, chamado pelo hook e pelo executor. As duas portas duplicam a interpretação de obra, motivo e seleção, e é nessa interpretação que divergem. As entradas herdadas da suite continuam vindo de `LABELS` em `regua.sh`, de `VOLTA_PORQUE` e `VOLTA_ESPERA` na volta e de `recursos.sh` para o runner VM. Os novos selos ainda injetam `SELO_SSH`, `SELO_SSH_LOG`, `SELO_VOLTA_SAIDA` e os caminhos de publicação isolados. `bin/pronto_item_a_item.py:30–60`, `bin/hook_pista_pronto.py:108–133`, `bin/suite.sh:156–181,282–305,352–355,379–403`, `bin/regua.sh:23`, `bin/volta_intermediaria.sh:31–34`, `bin/recursos.sh:99–137`, `bin/tests/test_pista_lateral.sh:14–33,46–48,62–74,115–121`, `bin/tests/test_travas_sem_memoria.sh:70–77`, `bin/codex_estado.sh:13–19`.

## Contornos e falhas

| Entrada | Veredito por leitura |
|---|---|
| Inteira canônica sem chave ou com chave de outra obra | **Fecha.** `rc 2` antes de SSH e VM. Com `--only` que cobre tudo na árvore examinada, fecha no executor e passa pelo hook. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45–47`, `bin/hook_pista_pronto.py:112–123`, `bin/suite.sh:237–264,282–303`. |
| `--dir` ausente, sem `app/` ou sem HEAD legível | **Fecha** na rota com chave. **Não fecha** a identidade do diretório quando o Git resolve um ancestral, nem a troca da árvore durante a espera da VM. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45,47,65`, `bin/suite.sh:294–303,361–374,394–403`. |
| `PISTA_CONTINGENCIA` sem PRONTO, ou hook ausente, não carregado ou com erro | **Fecha** na inteira canônica pelo executor. **Não fecha** M9a, pois o wrapper só verifica se o anexo é arquivo e depois o lê. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47–48,58`, `bin/suite.sh:300–315`, `bin/hook_pista_pronto.py:212–222`, `bin/codex_pergunta.sh:99–100,323–335`. |
| Inteira com `--motivo 'pouso O'` sem chave, régua ou pre-push | **Fora do desenho** como portas legítimas. Se o resultado de uma delas for usado para pre-opus, **não fecha** essa troca de finalidade. A decisão usa a porta e o motivo declarados. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:45–46,76–77`, `bin/suite.sh:237,282–293,394–403`, `bin/regua.sh:165–169`, `bin/pre-push.sh:185–199`. |
| Chamada direta a `trava_teste.sh`, `suite.sh --na-vez` ou `ssh pista pede` | **Fora da porta canônica da V4 e não fecha nesses pontos.** Por exemplo, `bash bin/suite.sh --na-vez '' '' -- printf 'chegou\n'` executa o comando sem consultar PRONTO ou adquirir a trava. `trava_teste.sh` adquire a trava e executa o argv recebido. A interface manual da pista recebe SHA e opções, sem obra ou PRONTO no contrato fornecido. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:47,63`, `bin/suite.sh:108–120,394–403`, `bin/trava_teste.sh:25–42`, `app/docs/BACKLOG.md:2191`. |
| RELATO ausente, juiz levanta exceção, JSON inválido ou ferramenta diferente de Bash | Um RELATO ausente é pulado, e o outro ainda pode valer. Sem marcador legível, o CLI sai `3` e a suite sai `2`. Exceção no juiz durante a decisão do hook ou JSON como `{` gera stderr, rc `0` e nenhuma negativa. No executor, erro do juiz dá rc `2`, sem SSH ou VM, mas a telemetria o rotula `SEM-PRONTO`. `tool_name=Read` passa sem I/O pelo ramo novo. Ficha ou referência ausente em chamada reconhecida recebe a recusa de M9a. `bin/pronto_item_a_item.py:30–60`, `bin/suite.sh:300–303`, `bin/hook_pista_pronto.py:136–160,171–194,198–222`. |

## Selos, trava e escopo

- **BLOQUEIA — P1 e FD não provam o registro carregado.** `_hook` executa o Python diretamente, sem ler evento ou matcher do settings. Retirar o registro inteiro deixa H1–H4 e FD1–FD8 com as mesmas entradas. FD2, FD4 e FD5 também passam no selo com um segundo anexo `/outro` inexistente na fixture e sem executar o wrapper. Na chamada real, esse anexo já impediria a entrega. O mutante de decisão exige `FD1|FD3`, logo basta um deles morder, embora a V4 exija os dois e a contagem de chegadas. V1–V3 chamam a suite diretamente. P4 não executa subida ao opus, L1c não injeta divergência e o selo não executa R1/G1/G2. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:73–77,87–91,109–111`, `bin/tests/test_pista_lateral.sh:68–79,98–102,113–155,163–175,194–199`, `bin/codex_pergunta.sh:99–100`.

- **BLOQUEIA — E1 tem oráculo, mas perdeu o mutante exigido.** E1 e E2 comparam tokens exatos no gerado e no origin da fixture. E1 compara também o SHA completo do medidor. Trocar todos os HEADs pelo da raiz ou omitir os tokens faria essas comparações falharem por leitura, mas o selo não aplica o mutante E1. O selo anterior `test_raia_declarada_no_estado.sh` continua cobrando declaração e escritor, sem conferir tokens ou publicação. O item 7 preserva expressamente E1. `respostas_f5_travas_2011_1025.txt:13`, `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:87–88,109`, `bin/tests/test_travas_sem_memoria.sh:68–138`, `bin/tests/test_raia_declarada_no_estado.sh:28–41,60–87`.

- **CORRIGE — Há guardas sem mutante e mutantes que sobreviveriam por leitura.** O selo da pista planta mutantes para SHA, obra, guardas de chave e PRONTO, sujeira, contingência automática, rc do job e leitura da decisão. P2 também tem casos de indentação, citação e limite do SHA. Sobreviveriam retirar só o teste de `DIR/app`, a validação da forma da chave para recortes, a recusa de `EXTRA` ou o repasse de `--parallel` à pista. No hook, sobreviveriam aceitar cabeçalho com qualquer ID, reconhecer só `DECISAO ` e trocar a leitura de `main` pela do disco da fixture, onde as duas coincidem. Também ficam sem caso a falha do juiz, o JSON inválido, a árvore ignorada e a troca de HEAD na contingência. O selo do motivo recompõe a mordida de `regex_frouxa` com pousos malformados. Em execução como root, se o arquivo `chmod 000` continuar legível, o caso é pulado e o mutante `busca_falha_vira_recorte` fica sem sua entrada de prova. `bin/suite.sh:289–309`, `bin/hook_pista_pronto.py:41–42,149–195,212–218`, `bin/tests/test_pista_lateral.sh:87–155,178–199`, `bin/tests/test_suite_motivo.sh:61–70,134–138,175,214`.

- **CORRIGE — Os selos individuais têm âncora de caminho. A régua ainda pode pular uma pasta inteira.** Os três selos tocados derivam a raiz pelo caminho do script, então podem ser chamados de outro diretório. O selo M5 usa os `bin/` da árvore em teste e exige Git. Nesta cópia sem `.git`, sua entrada sai vermelha por construção. Se a pasta `bin/tests` faltar na árvore usada pela régua, o glob não produz arquivo e a régua passa pelo laço sem executar selo algum. Execução em worktree [nao verificado]. `rascunho_TRAVAS-SEM-MEMORIA-V4.txt:30,87`, `bin/tests/test_pista_lateral.sh:10–14`, `bin/tests/test_suite_motivo.sh:7–10`, `bin/tests/test_travas_sem_memoria.sh:16–38`, `bin/regua.sh:134–145`.

- **CORRIGE — A ordem da trava existente permanece, e a rota nova lê relógio para telemetria.** Pista retorna antes do `flock` da VM. Pouso e contingência ainda chamam `trava_teste.sh` antes do modo `--na-vez`. O diff não adiciona uma chamada a `flock`. Os novos `date` ficam no executor, antes do pedido, no append e no fim. Se o relógio civil recuar durante o job, `tempo_s` pode sair negativo. O `time.time()` do medidor já existia e continua servindo à idade da raia. `bin/suite.sh:288,311,315–341,394–403`, `bin/trava_teste.sh:35–42`, `bin/raias_estado.py:121,126–132`.

- **FILA — Escopo do diff e registro pendente.** Os seis arquivos do diff fornecido ficam em `bin/` e `bin/tests/`. Produto, `app/`, crons, wrapper e configuração do Code não foram alterados por esse diff. T2 está fora do aval de construção. O bloco C também manda registrar `TRAVAS-T2-AVAL-LE-A-COPIA` no BACKLOG com zero devoluções demonstráveis. Esse registro não consta no diff. Publicação fora da cópia [nao verificado]. `bin/hook_pista_pronto.py:1`, `bin/pronto_item_a_item.py:1`, `bin/raias_estado.py:73`, `bin/suite.sh:266`, `bin/tests/test_pista_lateral.sh:1`, `bin/tests/test_suite_motivo.sh:61`, `bin/tests/test_travas_sem_memoria.sh:1`, `retoma_2011_0945.txt:20–21`, `respostas_f5_travas_2011_1025.txt:12–13`.

## O que ficou [nao verificado]

Identidade Git do `HEAD` e da base da cópia entregue, instalação e carga real do hook, RED na base, resultados executados dos selos e mutantes, smoke, chamadas

[truncado em 20000 de 20274 caracteres]

## 20/11 11:57 · sessao a017e52fb649 · F5-A1C-E-SILENCIO-V1-RASCUNHO · leitura · origem=Code

**PAREI RASCUNHO — A1(c) e SILÊNCIO.** A1(c) não fecha com «fica pendente» como estado da pergunta e preservação da resposta vigente. SILÊNCIO não fecha com dono obrigatório em toda aberta, tranca sem toque respeitada e ADMIN somente com Cobrar alcançável. Os desenhos abaixo dependem dos cortes de Q4. **O PRONTO integral da F5 continua retido.** `app/chamados/models.py:3259–3268` · `app/chamados/services/tarefa_do_dia.py:227–235,748–778` · `app/docs/CORTES.md:490,545` · `respostas_2011_1135.txt:4,6–11`

**F5-A1C-E-SILENCIO-V1 — rascunho para aprovação.** Cópia identificada pelo caminho fornecido como `8bb563c5b34e0afdbecd0220ef73c71610dbcc21`. O diretório veio sem metadados Git, portanto o SHA está [nao verificado]. A entrega é este texto, em leitura. Execução de testes, publicação no RELATO e remedição na sombra [nao verificado].

## 1 — Q1. Matrizes de transições

### A1(c) — Absorção da ausência

**A** é validação vigente na composta com coluna vazia. **B** é coluna preenchida com validação nula na composta. A leitura ordena por instante, põe as linhas reais em ordem de PK e, no empate, põe a sintética depois das reais. `ABSORCAO` guarda o vínculo e é neutra para a redução da validação. `q_validada_pelo_ato` tem o ramo da linha real e o da validação sintética. O contador já existe nesta cópia e lê presença nos dois lados em lotes. `app/chamados/models.py:3563–3585,4283–4317,4333–4362` · `app/chamados/services/vigia_ato_x_coluna.py:8–25` · `app/chamados/management/commands/vigiar_fio_morto.py:198,234,283`

| Estado de entrada | Juiz → operação | Persistência e fronteira | Retorno → prova |
|---|---|---|---|
| Escape sem dia do fato | Guarda de entrada → termina antes de chamar a porta da ausência. `app/chamados/services/materializacao.py:1019–1024` | Zero escrita por esse materializador. `app/chamados/services/materializacao.py:1021–1024` | `[]`. Teste nominal deste ramo [nao verificado]. `app/chamados/services/materializacao.py:1021–1022` |
| Irmã muda, elegível, C6 aceita | C6 com o carimbo em memória → `carimbar`. `app/chamados/models.py:3346–3365` | Carimbo, autor e via de ausência, sem inventar instante de resposta ou validação. No mesmo `atomic` da irmã, `ABSORCAO` em T1 com escape e ausência. A ausência nasce ou é obtida antes desse bloco. `app/chamados/models.py:3348–3357,3372` · `app/chamados/services/materializacao.py:1024–1036,1052–1069` | PK ausente de `recusadas`. O261 cobre a irmã muda e o outro dia. E1 cobra linha, vínculos e igualdade de relógio. `app/chamados/services/materializacao.py:1070–1090` · `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88–104` · `app/chamados/tests/test_e1_entradas.py:239–251` |
| Irmã com resposta anterior a T1, C6 aceita | `declarou_hora` escolhe a via e C6 julga o carimbo. `app/chamados/juizes.py:1785–1803` · `app/chamados/models.py:3349–3365` | Com texto, `carimbar` preenche os instantes faltantes conforme o ramo. Se preencher validação, a composta pode lê-la sinteticamente. Carimbo e `ABSORCAO` ficam juntos. Um escape `ATESTADO` de tipo hora usa a via de ausência. `app/chamados/models.py:3349–3357,4290–4295` · `app/chamados/services/materializacao.py:1052–1069` | PK fora de `recusadas`. E1 cobre o escape, o texto e os vínculos. Falta controle de hora anterior a T1 com comparação A/B após a porta real [nao verificado]. `app/chamados/tests/test_e1_entradas.py:239–251` |
| C6 mantém a irmã viva | `carimbar` restaura a foto em memória e retorna `False`. `app/chamados/models.py:3358–3365,3379–3386` | Sem carimbo ou `ABSORCAO` daquela tentativa. O evento é chamado após sair do `atomic` da irmã e tem savepoint próprio, best-effort. O reconciliador e C7 ainda decidem o destino e a disputa. `app/chamados/services/materializacao.py:1060–1090` · `app/core/observ.py:94–105,124–139` | Materializador retorna o PK em `recusadas`. A porta de resposta descarta a lista e, no fluxo de sucesso, devolve `(True, None)`. Prova existente `test_MORDE_irma_viva_sai_com_trilha`. `app/chamados/services/disputa_emissao.py:1557–1560,1574–1578` · `app/chamados/tests/test_o261e_f2_fila.py:241–258` |
| Recusa humana entre a seleção e a trava | C21 habilita a recusa. A porta de correção grava `RECUSA` sob a trava. A absorção atual ainda usa `q` e via da foto anterior. `app/chamados/services/validacao.py:915–944` · `app/chamados/services/disputa_emissao.py:1626–1655` · `app/chamados/services/materializacao.py:1033–1064` | Recusa conserva a foto anterior no ato e limpa par e carimbo. No rascunho, a absorção relê a irmã, a elegibilidade e a via. Uma irmã ainda elegível volta ao C6 com a foto fresca. `app/chamados/models.py:3508–3511,4297–4303` · `app/chamados/services/materializacao.py:1052–1069` | Falta RED com duas portas reais que distinga via velha de via relida e confira par, trilha e grupo [nao verificado]. `app/chamados/services/materializacao.py:1052–1076` |
| Resposta humana T2 > T1, lida sob a trava | Porta humana grava `RESPOSTA(False)`. A projeção temporal reconhece resposta posterior ao horizonte. O C6 pode aceitar o carimbo anterior se a resposta concordar com a luz. `app/chamados/services/disputa_emissao.py:1481–1508,1526–1531` · `app/chamados/juizes.py:1394–1403,1426–1440` | A sonda gravou coluna em T1, composta nula e resposta em T2. No rascunho, pula-se o carimbo e `ABSORCAO`, conserva-se a resposta, e a trilha explica o motivo temporal. No caso da sonda, o rótulo da pergunta é **RESPONDIDA** e C6 pode encerrá-la pelo fato. `medida4.log:16` · `app/chamados/models.py:3259–3268` · `app/chamados/juizes.py:1335–1380,1426–1440` | `recusadas` contém o PK no rascunho. O anexo 10 exige uma chamada a `carimbar` e seu assert final só procura B. Falta provar carimbo ausente, ato ausente, resposta intacta, trilha e grupo. `test_e4c_f5_item4_medida_relogio_comum.py:53–72` · `app/chamados/services/materializacao.py:1060–1090` |
| Resposta T2 seguida de validação administrativa legítima, ou irmã materializada enquanto esperava | Composta e colunas da irmã relida → respeitar o ato já consumado e reavaliar a seleção. A validação administrativa escreve coluna e `VALIDACAO` juntas. `app/chamados/services/validacao.py:161–175,746–760` · `app/chamados/services/materializacao.py:1033–1064` | Se já materializada, não recebe segundo carimbo. Se T2 ainda é a resposta vigente, fica fora desta absorção com a validação legítima preservada, inclusive quando os dois lados estão preenchidos. `app/chamados/models.py:3349–3357,4342–4351` | A recusa temporal não implica coluna vazia em todas as entradas. Falta RED com validação posterior e controle de irmã já materializada [nao verificado]. `app/chamados/services/validacao.py:161–175` · `app/chamados/services/materializacao.py:1033–1076` |
| **NOVO.** Resposta vigente anterior a T1, `DEVOLUCAO` posterior a T1 | Filtro só de resposta posterior deixa passar. `DEVOLUCAO` limpa a validação e conserva a resposta. Se o C6 aceitar pelo fato, a sintética em T1 também pode perder para esse ato mais novo. `app/chamados/models.py:4297–4303,4333–4351` · `app/chamados/services/validacao.py:283–296` · `app/chamados/juizes.py:1426–1440` | Rascunho testa a equivalência do carimbo tentado ainda no `atomic`. Se quebrar, a tentativa e seu `EventoCelula` voltam juntos, antes de registrar `ABSORCAO`. `app/chamados/services/materializacao.py:1060–1069` · `app/chamados/signals.py:243–259` | Falta RED para resposta antiga, devolução nova e C6 favorável ao carimbo [nao verificado]. O selo atual ensaia diretamente o caso da **resposta** mais recente. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:512–530` |
| **NOVO.** Empate de relógio ou resposta automática/sintética posterior | Comparação temporal estrita `>` e redução canônica. A leitura composta não expõe origem. Os escritores automático e pela trilha gravam `RESPOSTA(True)`, que preserva a validação. `app/chamados/juizes.py:1394–1403` · `app/chamados/models.py:4305–4317,4333–4362` · `app/chamados/services/disputa_emissao.py:1680–1707` | O empate não cai na recusa temporal. O desenho escolhido preserva qualquer resposta vigente completa posterior, com o alcance declarado em Q4. `app/chamados/juizes.py:1394–1403,1551–1560` | Faltam controles de empate, automático posterior e passivo sintético [nao verificado]. `app/chamados/models.py:4283–4295,4333–4351` |
| Irmã comum excluída, com âncora de escape de hora preservada | `perguntas_absorvidas_por` acha o grupo da âncora por disputa, dia e T1 e exclui origem com batida viva. `app/chamados/services/materializacao.py:1123–1139` | A irmã excluída não ganha o carimbo do grupo. As demais continuam recuperáveis pelo leitor existente. Na rejeição sem outra cobertura, o consumidor `on_commit` chama a devolução, que reabre as perguntas e a disputa sob sua ordem de trava e pede trilha. `app/ponto/signals.py:250–272` · `app/chamados/services/materializacao.py:1159–1194` | O261 prova devolução, idempotência, outra cobertura e batida viva. Falta exercitar o mesmo leitor após recusa por T2 no mesmo grupo [nao verificado]. `app/chamados/tests/test_o261_e1_ausencia_rejeitada_devolve.py:88–150` |
| Única âncora de hora fica fora, ou a entrada é `escolha` sem outra âncora de hora | O leitor exige escape carimbado no universo `tipo_resposta='hora'`. A sonda usou entrada `escolha`. `app/chamados/services/materializacao.py:1123–1136` · `test_e4c_f5_item4_medida_relogio_comum.py:29–33` | Com relógio comum mas sem âncora, outras irmãs podem ter `ABSORCAO` e carimbo, invisíveis à devolução. O rascunho segura o novo grupo que perderia sua âncora, condicionado a Q4. `app/chamados/services/materializacao.py:1064–1069,1126–1139` | Leitor retorna `[]` e a devolução retorna `0`. Falta RED de âncora única e comparação com o caso já sem âncora na entrada [nao verificado]. `app/chamados/services/materializacao.py:1130–1132,1166–1170` |
| Falha na gravação da trilha da recusa | `evento` tenta o escritor único da auditoria e captura a exceção. `app/core/observ.py:85–105,124–139` | Sem linha de auditoria nessa falha. O erro vai ao log/contador de engolidos. A resposta do escape já saiu de seu próprio bloco atômico quando chama o materializador. `app/core/observ.py:76–91,133–139` · `app/chamados/services/disputa_emissao.py:1478–1532,1557–1560` | A lista local continua contendo o PK. O teste existente prova o caso de auditoria disponível. Falta teste de falha deliberada [nao verificado]. `app/chamados/services/materializacao.py:1070–1090` · `app/chamados/tests/test_o261e_f2_fila.py:241–258` |
| A da reabertura e B por foto velha da resposta, nas revisões anteriores | A1(a) agora lê relógio após a trava. A1(b) relê a pergunta após a trava antes de decidir limpar a coluna. `app/chamados/services/disputa_emissao.py:1478–1508,1626–1642` | `REABERTURA`/`RECUSA` e limpeza do lote juntas. `RESPOSTA(False)` e limpeza quando necessária juntas. Coluna e ato podem ficar ambos vazios. `app/chamados/services/disputa_emissao.py:1526–1531,1636–1655` | `reabrir_para_correcao` devolve contagem e resposta devolve `(True, None)` no sucesso. Testes `test_RED_A_reabertura_posterior_a_validacao_limpa_coluna_e_composta` e `test_RED_B_resposta_posterior_a_validacao_limpa_coluna_e_preserva_trilha`. `app/chamados/services/disputa_emissao.py:1578,1655` · `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:38,62` |
| **NOVO na cópia.** Reabertura com validação só no ato e foto das colunas toda vazia | A composta lê `VALIDACAO`, mas a compreensão que monta `REABERTURA` exige texto, resposta, validação na coluna ou materialização. `app/chamados/models.py:4342–4351` · `app/chamados/services/disputa_emissao.py:1636–1653` | A leitura estática aponta que o lote pode atualizar a linha sem gravar compensadora, deixando A. A correção local de A1(c) não passa por esse ponto. `app/chamados/services/disputa_emissao.py:1640–1655` · `app/chamados/models.py:3508–3511` | O próprio selo já contém `test_reabertura_em_lote_compensa_validacao_so_no_ato`, com essa entrada. Resultado executado [nao verificado]. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:532–567` |
| Devolução de validação recusada, A ou B | A guarda atual aceita coluna ou ato, derruba o prefetch antigo, e a foto lê a validação vigente quando a coluna está vazia. `app/chamados/services/validacao.py:178–188,267–297` | Coluna limpa e `DEVOLUCAO` no mesmo bloco. Reabertura e `validacao_devolvida` seguem a comparação das fotos após esse bloco. Em chamada da validação ainda há transação externa. `app/chamados/services/validacao.py:289–321,746–760,786–808` | `True` se mudou, `None` na repetição, `False` fora do caso. O achado de refresh/foto antiga do 1025 está tratado no código desta cópia. `test_devolucao_e_o_escritor_da_coluna` é o controle a conferir. `app/chamados/services/validacao.py:243–246,271–284,309–321` · `app/chamados/tests/test_e4c_f5_voltaD_portas.py:383–392` |

As revisões de 0545 e 1025 também apontam o alcance da divergência antes da guarda de entrega. Corrigir e aplicar declaração podem chamar a porta de resposta antes da validação. A validação de hora tem sua guarda K6 e pode chamar recálculo. A absorção cria ou obtém a ausência antes do laço e a cerca genérica só atua em ato cercado. A criação de ausência pode invalidar uma projeção se mudou o insumo. O efeito líquido em entregue, batida ou valor está [nao verificado]. `app/chamados/services/acoes_disputa.py:375–385` · `app/chamados/services/declaracao_texto.py:695–699` · `app/chamados/services/validacao.py:746–772` · `app/chamados/services/materializacao.py:1024–1036,1083–1089` · `app/chamados/services/fio.py:1394–1409` · `app/ponto/services/ausencia.py:154–164` · `app/ponto/services/fechamento.py:1648–1656`

### SILÊNCIO — Dono e Cobrar

A escrita F2 é comum às entradas que passam pela ressuscitação. Sob a ordem existente, a porta relê o universo gravável, promove o passivo, grava a `REABERTURA` marcada com a foto, limpa materialização, assinatura e o par instante/texto, e reabre a disputa se necessário. `perguntas_ressuscitadas` é uma trilha best-effort após o bloco local. A porta retorna o número de perguntas atualizadas. A tarefa é leitura. `app/chamados/models.py:1646–1674,1701–1759` · `app/chamados/services/tarefa_do_dia.py:830–840`

| Estado de entrada | Juiz → operação | Persistência e fronteira | Retorno → prova |
|---|---|---|---|
| Sem ato ordinário, ressuscitada | C19 ignora a compensadora na sequência de decisões. C6 e partição do app dizem se a ação chega ao colaborador. `app/chamados/services/tarefa_do_dia.py:154–157,393–403,625–637` | Escrita F2 já descrita. Leitura posterior sem escrita. `app/chamados/models.py:1713–1731` · `app/chamados/services/tarefa_do_dia.py:830–840` | `aguarda_resposta` com `responder`. Fio `colab` se alcançável no app. Controle `test_RED_ressuscitar_sem_decisao_nao_muda_a_tarefa_do_dia`. `app/chamados/services/tarefa_do_dia.py:748–776` · `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:328–344` |
| SILÊNCIO comum, sem compensadora vigente | C19 lê o ato e conserva a guarda de pergunta encerrada. `app/chamados/services/tarefa_do_dia.py:158–183,744–746` | Zero escrita na leitura. `app/chamados/services/tarefa_do_dia.py:830–840` | `silencio_decidido`, `[]`. Uma porta residual pode pertencer a outro chamado. Controles E2, E3 e API F6. `app/chamados/services/tarefa_do_dia.py:734–767` · `app/chamados/tests/test_e2_tarefa_do_dia.py:277–289` · `app/chamados/tests/test_e3_portas_v6_sem_banco.py:98–106` · `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:59–65` |
| SILÊNCIO efetivo, última linha marcada, K6 falso, C20 falso, botão alcançável | C19 mais os fatos de oferta da tela. K8 identifica a competência e K6 diz se há TXT vigente. `app/core/juizes.py:303–305,342–344` · `app/chamados/services/tarefa_do_dia.py:563–579` · `app/templates/chamados/partials/modal_fio.html:123,354–380` | Tarefa só lê. O clique existente faz POST, verifica acesso e limite diário, envia push e anexa ocorrência se o envio passar. `app/chamados/views_cobrar.py:24–57` | Hoje `silencio_decidido`, `[]` no marco ordinário. Rascunho publica `cobrar` de ADMIN naquela instância com a porta existente. O teste-alvo F1/B1 muda e precisa provar HTML, acesso e destino do POST. `app/chamados/services/tarefa_do_dia.py:182–183,338–347,751–756` · `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320–344` |
| Mesmo caso, com outra pergunta alcançável no app no mesmo dia | C19 compõe ação por instância e escolhe o fio do dia por E3-V10. `app/chamados/services/tarefa_do_dia.py:734–776` · `app/docs/CORTES.md:490` | Zero escrita de dono, fila ou prazo. `app/chamados/services/tarefa_do_dia.py:791–806,830–840` | No rascunho, a silenciada conserva sua ação ADMIN e o **dia** sai `colab`/`porta_app`. E3 tem controle de precedência, falta combinação com ressuscitada [nao verificado]. `app/chamados/tests/test_e3_degrau_dono_e_porta.py:107–120` |
| K6 falso, C20 verdadeiro, nenhum toque posterior | C20 vence antes de julgar o marco. Na própria ressuscitação, a sondagem por C6 também pode barrar a entrada já trancada. Uma marca anterior pode ser lida sob uma tranca atual. `app/chamados/models.py:1543–1555,1581–1595` · `app/chamados/juizes.py:1067–1085,1412–1415` | A tranca é leitura, sem novo estado persistido na pergunta. `app/chamados/juizes.py:979–997` | `competencia_trancada`, instância sem PK e sem ações. Uma rota residual do dia não é dona da pergunta. Existe `test_dia_trancado_nao_tem_tarefa`. Falta caso nominal com marca e K6 falso [nao verificado]. `app/chamados/services/tarefa_do_dia.py:227–235,678–691,734–767` · `app/chamados/tests/test_e2_tarefa_do_dia.py:1088–1091` |
| Trancada sem TXT, depois de toque humano aceito | C20 lê desarquivar, Resolver dia ou **Cobrar este dia** depois da tranca. **Cobrar resposta** grava ocorrência no chamado. `app/chamados/juizes.py:1088–1098,1242–1256,1271–1332` · `app/chamados/services/cobrar_dia.py:27–45` · `app/chamados/views_cobrar.py:44–57` | Trilha existente do ato humano. Nenhum carimbo novo de desbloqueio pela tarefa. `app/chamados/juizes.py:1009–1013,1076–1085` | Volta ao julgamento normal. Só recebe Cobrar se o silêncio ainda for efetivo e a ação estiver disponível. E2 cobre toque e controles do sistema, de outro dia e anteriores à tranca. `app/chamados/services/tarefa_do_dia.py:227–235,338–347` · `app/chamados/tests/test_e2_tarefa_do_dia.py:1093–1122` |
| Aberta julgável, Cobrar fora de alcance | A âncora, a disputa escolhida, as perguntas listadas, `todas_respondidas`, o ramo de âncora de ciclo, a permissão e a oferta da Central são fatos diferentes. A presença na fila sozinha não responde à pergunta. `app/chamados/services/fio.py:97–117,260–294,388–395` · `app/templates/chamados/partials/modal_fio.html:123,354–380` · `app/templates/chamados/partials/_lista_chamados.html:83–84` | Zero escrita. O rascunho devolve diagnóstico explícito por pergunta e conta uma vez no vigia existente. `app/chamados/services/tarefa_do_dia.py:42–54,809–822` · `app/chamados/services/vigia_fio.py:582–595,658–662` | Sem ADMIN nessa instância até haver porta ou corte. O selo da cobrança prova o botão e a rota estáticos. Falta prova da combinação em tela real, inclusive âncora que só oferece ação por outra irmã [nao verificado]. `app/chamados/tests/test_selo_painel_oferece_cobrar.py:173–193` |
| **NOVO.** Botão do modal renderizado, mas cobrança do colaborador já enviada hoje | O modal pode renderizar o botão. A Central mostra a hora no lugar dele. O POST do modal ainda pode responder «Já cobrado». `app/templates/chamados/partials/modal_fio.html:363–380` · `app/templates/chamados/partials/_lista_chamados.html:83–84` · `app/chamados/views_cobrar.py:34–43` | Sem segundo push ou segunda ocorrência nesse ramo do POST. `app/chamados/views_cobrar.py:41–57` | Exibir o estado já cobrado no diagnóstico. Separar no RED a existência do botão da possibilidade de executar nova cobrança hoje. A disponibilidade futura do push é [nao verificado]. `app/chamados/services/painel.py:349–358,395–396` · `ap

[truncado em 20000 de 56339 caracteres]

## 20/11 11:56 · sessao f9a1536028af · CADERNO-DE-CORTES-2011 · leitura · origem=Code

Cópia indicada conferida. A identidade Git de HEAD/main e os logs originais citados nos documentos ficam [nao verificado].

## FAIXA-NO-CADASTRO -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta o aval para executar depois do contrato 20. O campo e a migração a partir do mapa atual já estão descritos. app/docs/BACKLOG.md:2334
Q2: Quando chegar a vez dessa mudança, você autoriza que o administrador cadastre qual ausência prevalece quando duas coincidem? app/docs/BACKLOG.md:2334
Q3 (a): Autorizar na vez registrada. Tela ganha o campo de prioridade. Na folha, a migração reproduz a prioridade atual e alterações posteriores podem mudar qual ausência prevalece. Alcance hoje: nao medido. Construir campo, migração e leitor único. app/docs/BACKLOG.md:2334, app/ponto/precedencia.py:52,381
Q3 (b): Adiar o aval. Tela com o formulário atual e folha pela prioridade vigente. Alcance hoje: nao medido. Nenhuma construção nesta etapa. app/templates/ponto/partials/painel_tipo_ausencia.html:28, app/ponto/precedencia.py:76
Q4: nao respondida quanto à execução. MANHA-1211 item 7 autorizou o registro do desenho. app/docs/BACKLOG.md:2334, app/docs/RELATO.md:2197
Q5: vira complemento de EFEITO-NONE-UM-LEITOR, na posição registrada após o contrato 20 e com aval próprio. app/docs/GRADE-MESTRA.md:1325,1328, app/docs/BACKLOG.md:2334

## DISPUTA-FECHADA-SEM-RESOLUCAO -- classe: resto -- tipo: corte
Q1: "sem pergunta". O registro pedia um corte para garantir que o encerramento deixasse o motivo na trilha. app/docs/BACKLOG.md:1222, app/docs/GRADE-MESTRA.md:1049
Q2: Como o administrador deve saber por que uma disputa foi encerrada quando a explicação está em branco? app/docs/BACKLOG.md:1222
Q3 (a): Usar o motivo que já fica na trilha dos novos encerramentos. Tela atual e efeito direto zero na folha. Alcance hoje: nao medido. Nenhuma nova regra de encerramento. app/chamados/models.py:2059,2095,2106
Q3 (b): Exibir no detalhe dos registros antigos “motivo não registrado” quando faltar a informação. Efeito zero na folha. Alcance hoje: nao medido. Em 30/09, 30 de 4.682 fechadas tinham o texto vazio. Construir o aviso. app/docs/BACKLOG.md:1222, app/chamados/migrations/0049_disputa_motivo_fechamento.py:15
Q4: ja respondida pelo corte C7-DISPUTA-FECHA-POR-UM-JUIZ, com L-007, para a trilha dos novos encerramentos. O código grava o motivo estruturado e exige texto na porta de encerramento manual. app/docs/BACKLOG.md:1618, app/docs/LEIS.md:65, app/chamados/models.py:2095,2106
Q5: pode morrer como obra avulsa de encerramento novo. A migração admite motivo vazio no legado, e o registro original preserva o histórico. app/chamados/migrations/0049_disputa_motivo_fechamento.py:15, app/docs/BACKLOG.md:1222

## VIGENCIA-SOBREPOSTA-COL515 -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta decidir como tratar dois vínculos históricos inativos que cobrem os mesmos dias com horários opostos. app/docs/BACKLOG.md:1223
Q2: Quando o histórico de uma pessoa mostra dois horários para os mesmos dias, o administrador deve preservar os dois registros ou corrigir as datas quando houver prova? app/docs/BACKLOG.md:1223
Q3 (a): Preservar as datas e avisar ou recusar se uma reativação criar conflito. Admin vê o motivo ao tentar reativar. Folha sem recálculo por essa opção. Alcance hoje: nao medido. O registro de 30/09 traz 1 pessoa, 2 vínculos e 11 dias. Construir a guarda na reativação. app/docs/BACKLOG.md:1223, app/colaboradores/services/vinculo.py:505,523
Q3 (b): Corrigir o histórico com prova e trilha. Admin vê o período corrigido. Efeito na folha: nao medido. Alcance hoje: nao medido, com o mesmo caso registrado em 30/09. Construir a correção pela porta existente, respeitando a regra da folha já entregue. app/docs/BACKLOG.md:1223,1248, app/docs/LEIS.md:125,155
Q4: nao respondida para o histórico inativo. VINCULO-LINHA-DO-TEMPO trata a linha ativa, que é o universo do censo atual do código. app/docs/BACKLOG.md:1248, app/colaboradores/services/vinculo.py:505,523
Q5: vira complemento de VINCULO-LINHA-DO-TEMPO. O corte sobre o histórico inativo segue pendente. app/docs/GRADE-MESTRA.md:1078,1082, app/docs/BACKLOG.md:1223

## O62 -- classe: espurias/HE -- tipo: corte
Q1: "sem pergunta". Falta corrigir a resposta que diz “escala sem intervalo” quando a duração está cadastrada e faltam os horários da pausa no registro do dia. app/docs/BACKLOG.md:1469, app/ponto/juiz_batida.py:294,304
Q2: Quando a escala prevê uma pausa e o registro do dia não traz os horários dela, como a pausa deve aparecer para o administrador e contar na folha? app/docs/BACKLOG.md:1469, app/docs/LEIS.md:80
Q3 (a): Corrigir primeiro o que se informa. Admin vê a duração prevista e que faltam horários. Efeito imediato zero na apuração. Alcance hoje: nao medido. Na amostra original, 3 de 5 pessoas tinham dias assim, incluindo 17 de 20 dias de col87. Construir a correção da resposta e da exibição. app/docs/BACKLOG.md:1469, app/ponto/juiz_batida.py:304,309
Q3 (b): Corrigir a exibição e aplicar a duração cadastrada onde a apuração ainda a omite. Admin distingue pausa prevista de pausa registrada. A folha desconta a pausa nesses casos. Alcance hoje: nao medido. Construir a correção na fonte e medir por rubrica. app/docs/LEIS.md:80,193, app/docs/RELATO.md:496
Q4: ja respondida pelo corte INTERVALO-CADASTRADO-NAO-VIRA-HE, L-032. A regra escrita manda descontar da jornada a duração cadastrada. app/docs/LEIS.md:80
Q5: segue para execução. A proteção anterior do motor cobriu o caso documentado, e o retorno incorreto ainda existe na cópia. app/docs/RELATO-ARQUIVO.md:6405,6409,6412, app/ponto/juiz_batida.py:304

## O310-RAIOX -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta o corte específico para guardar o histórico, com modelo e migração. A lente está registrada como publicada. app/docs/BACKLOG.md:1727, app/folha/retrato_folha.py:723
Q2: Para comparar a situação de ontem com a de hoje, o administrador precisa só da foto anterior ou também de um histórico por data? app/docs/CORTES.md:357, app/folha/retrato_folha.py:723
Q3 (a): Guardar a foto anterior. Admin vê o comparativo de entradas e bloqueios por responsável. Efeito zero na folha. Alcance hoje: nao medido. Construir armazenamento da foto anterior, migração e comparação. app/docs/CORTES.md:357, app/folha/retrato_folha.py:723
Q3 (b): Guardar fotos datadas. Admin vê o mesmo comparativo, com consulta às datas guardadas. Efeito zero na folha. Alcance hoje: nao medido. Construir histórico por data, migração e consulta. app/docs/CORTES.md:357, app/folha/retrato_folha.py:725
Q4: nao respondida quanto ao armazenamento. O256-RAIO-X-DO-RETRATO já manda ler duas fotos gravadas e informar quando falta a anterior. app/docs/CORTES.md:357, app/folha/retrato_folha.py:723
Q5: vira complemento de O310. A lente foi publicada com diferença de folha zero registrada. Resta o histórico. app/docs/BACKLOG.md:1727

## O34 -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta escolher o tratamento dos ajustes que deixam de encaixar quando o código muda. O registro traz três propostas de desenho. app/docs/BACKLOG.md:1436, app/docs/GRADE-MESTRA.md:1238
Q2: Quando uma correção pronta deixa de encaixar porque o sistema mudou, ela deve se refazer sozinha, mostrar o que mudou ou ter prazo para ser refeita ou descartada? app/docs/BACKLOG.md:1436
Q3 (a): Refazer o ajuste automaticamente. Efeito direto zero na tela do admin e na folha. Alcance hoje: nao medido. Construir a reconstrução automática e sua conferência. app/docs/BACKLOG.md:1436
Q3 (b): Parar a aplicação e mostrar ao operador o trecho que mudou. Efeito direto zero na tela do admin e na folha. Alcance hoje: nao medido. Construir o diagnóstico com a comparação. app/docs/BACKLOG.md:1436
Q3 (c): Dar prazo e registrar o ato de refazer ou descartar. Efeito direto zero na tela do admin e na folha. Alcance hoje: nao medido. A passada registrada achou 11 de 14 falhas dessa família e 17 ajustes em espera. Construir prazo e destinos na fila. app/docs/BACKLOG.md:1436
Q4: nao respondida. ZUMBIDO trata a segunda falha pelo mesmo motivo e veta a terceira tentativa. A escolha entre as três propostas segue aberta. app/docs/CORTES.md:307, app/docs/BACKLOG.md:1436
Q5: vira complemento de O31, como já agrupado na Grade. app/docs/GRADE-MESTRA.md:1234,1238

## O37 -- classe: resto -- tipo: !
Q1: "o gravado de competencia exportada e reescrito, ou fica e a diferenca vira Pauta DP?" app/docs/RELATO-ARQUIVO.md:24120
Q2: A folha de agosto deve ser refeita por causa dos dias de plantão registrados errado ou a diferença deve ir ao departamento pessoal? app/docs/RELATO-ARQUIVO.md:24098,24119
Q3 (a): Preservar a folha e encaminhar a diferença ao DP. Admin sem tarefa nova na tela. Alcance hoje: nao medido. A medida registrou 22 dias de 2 pessoas e 161 h de diferença de jornada prevista em agosto. Depois, preparar a linha para o DP. app/docs/RELATO-ARQUIVO.md:24089,24100, app/docs/RELATO.md:1012
Q3 (b): Reescrever pela porta de retificação. Admin veria os dias corrigidos e a folha seria recalculada. Alcance hoje: nao medido, com os mesmos 22 dias na medida histórica. Efeito por rubrica: nao medido. Depois, conferir e comparar os valores antes e depois pela porta existente. app/docs/RELATO-ARQUIVO.md:24091,24119, app/ponto/portas/regen_exportada.py:87
Q4: ja respondida pelo corte COMPLEMENTO-1 RESPOSTAS-R4-E1PORTA-1611 item 3, de 16/11. Passivo exclusivamente anterior a setembro vai à lista do DP. app/docs/RELATO.md:1012, app/docs/BACKLOG.md:1439
Q5: vira complemento de O54, sem espera avulsa pelo Ronald para os casos medidos de agosto. Entrada efetiva na lista do DP [nao verificado]. app/docs/BACKLOG.md:1460, app/docs/RELATO.md:1012

## O296 -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o aval nominal para trocar a identificação pessoal no comentário do arquivo de cálculo protegido. app/docs/BACKLOG.md:1704, CLAUDE.md:142
Q2: Você autoriza retirar a identificação pessoal também do comentário dentro do cálculo da folha? app/docs/BACKLOG.md:1704
Q3 (a): Substituir a identificação somente fora do arquivo protegido. Efeito zero na tela e na folha. Alcance hoje: nao medido. A medição de 16/10 registrou 9 comentários fora. Depois, fazer as substituições de texto. app/docs/BACKLOG.md:1704
Q3 (b): Incluir o comentário do arquivo protegido com aval nominal. Efeito zero na tela e na folha. Alcance hoje: nao medido. A medição registrou 10 comentários ao todo. Depois, fazer as substituições de texto nos sítios conferidos. app/docs/BACKLOG.md:1704, CLAUDE.md:142
Q4: nao respondida. A O295 tratou a publicação, e o registro da O296 mantém o aval específico para o arquivo protegido. app/docs/BACKLOG.md:1703,1704
Q5: segue pelo `!` nominal do décimo comentário. app/docs/BACKLOG.md:1704, app/docs/GRADE-MESTRA.md:410

## REJULGAR-61-COBPARC -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` próprio, com a lista das 55 atas de competências entregues. app/docs/BACKLOG.md:906, app/docs/GRADE-MESTRA.md:657
Q2: Você autoriza revisar os registros antigos de ausências parciais dos meses que já foram entregues? app/docs/BACKLOG.md:906
Q3 (a): Preservar os registros e levar diferenças comprovadas ao DP. Admin vê os registros atuais, e a folha entregue fica preservada. Alcance hoje: nao medido. Em 08/11, eram 61 atas, 55 entregues e 6 trancadas. Depois, preparar a lista e os comparativos. app/docs/BACKLOG.md:906, app/docs/LEIS.md:125,145
Q3 (b): Autorizar revisão nominal. Admin vê os registros revistos com motivo e trilha. Eventual retificação da folha entregue passa pela porta própria. Alcance hoje: nao medido, com 55 atas no lote documentado do `!`. Depois, conferir o lote e reutilizar a porta. app/docs/BACKLOG.md:906, app/ponto/portas/regen_exportada.py:87,111,116
Q4: nao respondida. TRES-DO-TOPO-1109 item 2 decidiu a regra da COBPARC e reservou o `!` próprio do acervo. app/docs/RELATO.md:2920, app/docs/BACKLOG.md:906
Q5: segue. O lote inclui setembro e foi registrado como obra separada da COBPARC. app/docs/BACKLOG.md:906, app/docs/RELATO.md:1012

## RESIDUO-PREMIG-NO-BIND-MOUNT -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para apagar os arquivos antigos da instalação em uso. app/docs/BACKLOG.md:1761, app/docs/GRADE-MESTRA.md:421
Q2: Você autoriza remover os arquivos antigos da instalação ou quer mantê-los onde estão? app/docs/BACKLOG.md:1761
Q3 (a): Manter os arquivos. Efeito zero na tela e na folha. Alcance hoje: nao medido. Em 19/10 havia 64 arquivos, 28 fora das pastas de estáticos, logs e mídia. Depois, usar o censo já corrigido. app/docs/BACKLOG.md:1761, app/docs/RELATO-ARQUIVO.md:43426
Q3 (b): Conferir a lista e remover os arquivos autorizados. Efeito previsto zero na tela e na folha. Alcance hoje: nao medido, com 64 arquivos na medição histórica. Depois, executar a limpeza sobre o inventário conferido. app/docs/BACKLOG.md:1761, app/docs/LEIS.md:67
Q4: nao respondida quanto a apagar. L-009 exige autorização para a remoção. app/docs/LEIS.md:67
Q5: pode morrer como bloqueio do censo. A correção passou a classificar os ignorados e registrou `EXTRA-VIVO=0`. Remoção efetiva [nao verificado]. app/docs/RELATO-ARQUIVO.md:43426

## JANELA-DO-NOTURNO-CRAVADA -- classe: espurias/HE -- tipo: !
Q1: "sem pergunta". Falta o aval para tocar o cálculo protegido e permitir cadastrar os horários noturnos da convenção. app/docs/BACKLOG.md:1769, CLAUDE.md:142
Q2: Você autoriza que o administrador cadastre o horário noturno de cada convenção? app/docs/BACKLOG.md:1769
Q3 (a): Usar a faixa fixa de 22h a 05h. Tela com o cadastro atual e folha pela faixa fixa. Casos com horário diferente hoje: nao medido. Nenhuma construção. app/docs/BACKLOG.md:1769, app/ponto/motor_calculo_v2.py:99,103
Q3 (b): Cadastrar início e fim na tela. A folha pode mudar o adicional e a parte noturna da hora extra onde a convenção usar outra faixa. Alcance hoje: nao medido. Construir os campos, a leitura comum e os testes de cálculo. app/docs/LEIS.md:64, app/ponto/motor_calculo_v2.py:983,1014
Q4: nao respondida quanto ao `!`. L-006 já define o cadastro pela tela para regras que variam. app/docs/LEIS.md:64, CLAUDE.md:142
Q5: segue. O cálculo ainda fixa os horários e a alteração exige aval explícito. app/ponto/motor_calculo_v2.py:101,103, app/docs/BACKLOG.md:1769

## PROTEGE-VAZIA-EM-55-LEIS -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para alterar linhas de leis já escritas. app/docs/BACKLOG.md:1755, app/docs/RELATO-ARQUIVO.md:299
Q2: Você autoriza revisar as regras publicadas e marcar os trechos de código que elas protegem expressamente? app/docs/LEIS.md:43, app/docs/RELATO-ARQUIVO.md:299
Q3 (a): Deixar a coluna como está. Efeito zero na tela e na folha. Alcance na cópia: 55 linhas com sítio na coluna de quem aplica e proteção vazia. Total elegível: nao medido. Nenhuma construção. app/docs/LEIS.md:57, bin/tests/test_lei_protege_sitio.sh:66,77
Q3 (b): Autorizar a revisão linha a linha. Efeito zero na tela e na folha. Alcance na cópia: as mesmas 55 linhas candidatas. Total elegível: nao medido. Depois, preencher as proteções explícitas e conferir a trava em ato próprio. app/docs/LEIS.md:43,57,131, bin/tests/test_lei_protege_sitio.sh:77
Q4: nao respondida quanto ao `!`. LEI-PROTEGE-SITIO já define o critério: preencher quando o texto da lei protege expressamente o sítio. app/docs/LEIS.md:43,48, app/docs/BACKLOG.md:1755
Q5: segue. A L-097 exemplifica proteção explícita com a coluna vazia. app/docs/RELATO-ARQUIVO.md:780, app/docs/LEIS.md:115

RESUMO: 1 sobe ao Ronald como lei, sobre vigências históricas inativas (app/docs/BACKLOG.md:1223). 3 já respondidas, DISPUTA-FECHADA-SEM-RESOLUCAO, O62 e O37 (app/docs/BACKLOG.md:1618, app/docs/LEIS.md:80, app/docs/RELATO.md:1012). 7 podem morrer ou virar complemento (app/docs/GRADE-MESTRA.md:1049,1082,1238,1328, app/docs/BACKLOG.md:1460,1727, app/docs/RELATO-ARQUIVO.md:43426). 0 fecham por evidência de uso pelo corte de 16/11, que tratou de outras obras (app/docs/BACKLOG.md:2367, app/docs/RELATO.md:1037).

## 20/11 11:56 · sessao a4b0bc5e8dec · CADERNO-DE-CORTES-2011 · leitura · origem=Code

Li a cópia fornecida em somente leitura. A confirmação do hash como HEAD de `main` fica [nao verificado], pois a cópia não tem metadados Git.

## O275 -- classe: espurias/HE -- tipo: corte
Q1: “O FLAG NOVO DA O275 PODE SER LIGADO NUM TIPO DE PLANTAO (`12x36`/`24x48`), OU O PLANTAO RESPONDE SO PELA PRECEDENCIA QUE JA EXISTE?” app/docs/RELATO-ARQUIVO.md:746
Q2: O administrador pode tornar um feriado dia de trabalho quando ele cai na folga de um plantão? app/docs/RELATO-ARQUIVO.md:748
Q3 A: Admin pode usar a opção também nos plantões, salvo intermitente. A folha passa a contar o feriado como dia previsto onde a opção estiver ligada. Alcance hoje nao medido. O censo registrou 337 vínculos 12x36 potencialmente expostos. Atualiza e confere a implementação guardada. app/docs/RELATO-ARQUIVO.md:749–753
Q3 B: Admin usa a opção em escalas semanais, 6x1 e 5x2. A folha dos plantões mantém a previsão atual. Alcance hoje nao medido. O censo apontou 1 vínculo no alvo e 4 tipos cadastrados como 12x36 fora do alcance. Acrescenta a restrição por ciclo e ajusta os testes. app/docs/RELATO-ARQUIVO.md:749–753
Q4: nao respondida. SDF-FERIADO-E-DIA autoriza o cadastro. A escolha entre A e B permanece aberta. app/docs/BACKLOG.md:1682 e app/docs/RELATO.md:417
Q5: segue. A publicação espera A ou B, e a O277 depende desse cadastro para tratar a dobra. app/docs/BACKLOG.md:1682 e app/docs/BACKLOG.md:1684

## O10-D-PERMUTA-RECIPROCA -- classe: resto -- tipo: corte
Q1: “(a) DNA carrega marcos do cedente ou (b) d1=d2 fora da porta”. O RELATO também lista tipo e aprovação da carta, falha de gravação, “cede so plantao”, descanso e o ! de escala. O literal desses cortes e do ! fica [nao verificado]. app/docs/RELATO.md:886–887
Q2: Numa troca no mesmo dia, quem assume recebe os horários de quem cede ou o sistema recusa a troca? app/docs/RELATO.md:887
Q3 A: A operação aceita a troca e mostra os horários assumidos ao admin. A folha trata o plantão assumido como dia normal com os adicionais cabíveis. Alcance hoje nao medido. Implementa o transporte dos horários do cedente. app/docs/RELATO.md:887 e app/docs/CORTES.md:286
Q3 B: A operação mostra a recusa ao admin. A folha conserva os plantões originais. Alcance hoje nao medido. Implementa a recusa, condicionada à revisão do corte que já permite a mesma data. app/docs/RELATO.md:887 e app/docs/RELATO.md:2719
Q4: ja respondida pelo corte LEI-O10-D-PERMUTA quanto a permitir a mesma data, reafirmado no preparo de 16/11. O desenho dos horários e os demais itens listados na revisão seguem abertos. app/docs/RELATO.md:2719 e app/docs/RELATO.md:1055 e app/docs/RELATO.md:886–887
Q5: segue. A cópia aprovada ainda aguarda os cortes residuais e o ! de escala. app/docs/RELATO.md:879 e app/docs/RELATO.md:886

## O14 -- classe: resto -- tipo: smoke
Q1: “sem pergunta”. Falta “SMOKE de olho no app de um noturno 12x36 (ex.: col174 ou col788) depois do deploy.” app/docs/PENDENTES_RONALD.json:1587
Q2: No celular do trabalhador noturno, a saída da madrugada e as horas aparecem junto com o restante do plantão? app/docs/PENDENTES_RONALD.json:1587
Q3 A: Se a conferência passar, o admin conserva a tela atual, o colaborador vê o turno no dia correto e a folha conserva os valores. Alcance hoje nao medido. Registra a conferência do aparelho. app/docs/BACKLOG.md:1414 e app/api/views.py:1440–1453 e app/api/views.py:1475–1485
Q3 B: Se falhar, o admin recebe o dia e a divergência vistos no celular. A conferência é uma leitura da folha. Alcance hoje nao medido. A correção depende do erro observado [nao verificado]. app/docs/PENDENTES_RONALD.json:1587 e app/docs/RELATO-ARQUIVO.md:27292–27293
Q4: nao respondida quanto à conferência no aparelho. app/docs/BACKLOG.md:1414
Q5: segue. Os 126 de 368 colaboradores com aviso diferente são da medição do servidor. O fechamento por uso de 16/11 registra outras operações. app/docs/RELATO-ARQUIVO.md:27288–27293 e app/docs/RELATO.md:1037

## O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL -- classe: fio morto -- tipo: corte
Q1: “sem pergunta” de mérito pendente. “esta PERGUNTA ja foi materializada de fato (clique morto)?” é o que a função já responde. A declaração separada dessa autoridade exige a frase de corte. app/docs/BACKLOG.md:1002 e app/docs/LEIS.md:69
Q2: Como o sistema sabe se ainda há algo que o administrador pode fazer naquela pergunta? app/chamados/services/raiox.py:160–194
Q3 A: Registra a regra existente por pergunta. O admin só vê a ação enquanto ela puder produzir efeito. A folha segue consultando as ações disponíveis para o dia. Alcance hoje nao medido. Formaliza o registro com teste do comportamento atual. app/docs/BACKLOG.md:1002 e app/chamados/services/fio.py:310–319 e app/folha/fora_da_folha.py:70–83
Q3 B: O botão do admin passa a ler a mesma tarefa do dia que a folha já usa. Alcance hoje nao medido. Migra o leitor restante e confere que o botão continua disponível quando ainda há ação. app/docs/BACKLOG.md:2112 e app/chamados/services/tarefa_do_dia.py:142–147 e app/chamados/services/fio.py:310–319
Q4: ja respondida pelo corte CLIQUE MORTO de 02/09. O efeito comprovado retira a ação, e o registro sozinho deixa a pergunta disponível. LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN responde por qual caminho se chega ao dia. app/chamados/services/raiox.py:160–194 e app/docs/CORTES.md:457 e app/docs/BACKLOG.md:1002
Q5: vira complemento de FIO-E0-V4-E3-LEITORES. A migração já abrange a tela e a folha, e a tela ainda consulta a regra pelo caminho antigo. app/docs/BACKLOG.md:2112 e app/chamados/services/fio.py:310–319

## O176 -- classe: resto -- tipo: corte
Q1: “o que o pacote declara tocar (`declarados()`) ja esta na arvore?” app/docs/BACKLOG.md:1553
Q2: Para liberar um novo trabalho, basta encontrar o título do anterior ou é preciso conferir que suas mudanças entraram? app/docs/BACKLOG.md:1553
Q3 A: A fila interna continua liberando pelo título, sem efeito direto na tela do admin ou na folha. Alcance hoje nao medido. Em 04/10 houve 0 liberações erradas entre 47 rótulos examinados. Mantém o mecanismo atual. app/docs/BACKLOG.md:1553 e app/core/esteira_vigia.py:269–314
Q3 B: A fila interna só libera após comprovar a entrega, sem efeito direto na tela do admin ou na folha. Alcance hoje nao medido. Constrói o verificador compartilhado de conteúdo e testa dois pacotes com o mesmo título. app/docs/BACKLOG.md:1553 e app/docs/CORTES.md:317
Q4: nao respondida. ESTEIRA-SECA-1-E-2-AGORA exige uma fonte comum. L-011 exige a assinatura da nova autoridade. O critério de conteúdo ainda espera corte. app/docs/CORTES.md:317 e app/docs/LEIS.md:69 e app/docs/BACKLOG.md:1553
Q5: segue. A cópia ainda usa o título para decidir se o pacote já entrou. app/core/esteira_vigia.py:269–314 e app/docs/BACKLOG.md:1553

## O94 -- classe: resto -- tipo: !
Q1: “o gravado de 09 passa a nao pagar essas 283 h (e elas viram cobranca a resolver com o DP/supervisao), ou ficam como estao ate a cobranca ser respondida?” app/docs/PENDENTES_RONALD.json:369
Q2: As horas de setembro dos turnos sem saída devem ser retiradas do cálculo registrado agora ou mantidas até o DP resolver a pendência? app/docs/PENDENTES_RONALD.json:369
Q3 A: Admin vê a cobrança da saída faltante. O cálculo registrado perde as horas indevidas, com eventual retificação pelo DP. Alcance hoje nao medido. A medida inicial foi 14 pessoas e 283,01 h, depois revista para 12 e 219,02 h. Prepara nova comparação e ato nominal com trilha e reversão. app/docs/BACKLOG.md:1495 e app/docs/RELATO-ARQUIVO.md:6233–6234 e app/docs/RELATO.md:5054
Q3 B: Admin e DP recebem os dois valores e tratam a cobrança. A folha emitida conserva o valor enquanto o DP decide. Alcance hoje nao medido. Prepara a pauta com a diferença atualizada, sem executar recálculo por este ato. app/docs/BACKLOG.md:1676 e app/docs/RELATO.md:5054
Q4: nao respondida quanto ao ! nominal. L-113 e RELAVRA-09-MOTOR-DE-HOJE separam o cálculo atual da folha já emitida. O registro posterior autoriza guarda e medição da 09 e deixa a aplicação para autorização própria. app/docs/LEIS.md:155 e app/docs/BACKLOG.md:1676 e app/docs/RELATO.md:5054
Q5: segue como ! de dinheiro. O JSON ainda tem resposta nula e a última medida do alcance deste ato [nao verificado]. app/docs/PENDENTES_RONALD.json:366–373

## HIGIENE-DE-CONTEXTO -- classe: resto -- tipo: !
Q1: “pode QUEBRAR o paragrafo em dois -- regra num, historia no outro -- sem mudar uma palavra”. Era o ! do item 5. app/docs/RELATO-ARQUIVO.md:27476–27490
Q2: Pode separar a regra da história que a explica, preservando todas as palavras da regra? app/docs/RELATO-ARQUIVO.md:27488–27490
Q3 A: Tela do admin e folha ficam iguais. Alcance documental atual nao medido. A leitura histórica marcou 5 trechos misturados em 27 parágrafos. Move a história com ponteiro e confere a regra palavra por palavra. app/docs/RELATO-ARQUIVO.md:27481–27489 e app/docs/BACKLOG.md:1321
Q3 B: Tela do admin e folha ficam iguais. Alcance documental atual nao medido. Na leitura histórica, 0 dos 27 parágrafos continham só história. Mantém os trechos juntos, sem trabalho novo. app/docs/RELATO-ARQUIVO.md:27481–27490
Q4: ja respondida pelo corte DIETA-DO-CLAUDE-MD (O224), que aplica L-109 e preserva a regra palavra por palavra. app/docs/BACKLOG.md:1321 e app/docs/LEIS.md:135
Q5: pode morrer como pendência independente. Os itens 1–4 e 6 foram fechados, e O220/O224 registram a transferência da história e sua conclusão. app/docs/BACKLOG.md:1388 e app/docs/BACKLOG.md:1337 e app/docs/BACKLOG.md:1321

## O148-EDITAR -- classe: resto -- tipo: corte
Q1: “sem pergunta” em forma interrogativa. A proposta literal que espera corte é “calendario com uso edita por versao com vigencia, nunca no lugar”. app/docs/BACKLOG.md:819 e app/docs/CORTES.md:390
Q2: Quando um calendário de folgas já é usado, o administrador salva no mesmo cadastro ou cria uma versão que começa a valer numa data escolhida? app/docs/CORTES.md:390
Q3 A: Admin escolhe a data da nova versão e pode consultar a anterior. A folha passa a usar as novas folgas a partir da vigência. Alcance hoje nao medido. Em 16/11 havia 72 calendários do tipo dias do mês em uso. Constrói versões, vigência, trilha e edição. app/docs/CORTES.md:390 e app/docs/RELATO.md:1058
Q3 B: Admin edita o cadastro existente. A porta atual sincroniza as folgas e células futuras, sem recalcular a folha no clique. Alcance hoje nao medido. Mantém a porta atual. app/escala/folgas.py:571–601 e app/escala/folgas.py:605–640
Q4: nao respondida. O148-EDITAR registra a proposta e pede corte de lei. CALENDARIO-DE-FOLGAS-ARQUIVA-NAO-APAGA decide o arquivamento. app/docs/CORTES.md:390 e app/docs/BACKLOG.md:2751
Q5: vira complemento de O148, como já consta na grade, com o corte de versão pendente. O smoke de 16/11 usou calendário sem vínculo nem folga materializada e saiu sem salvar. app/docs/GRADE-MESTRA.md:1523–1526 e app/docs/RELATO.md:983

## DIA-CONVOCADO-PELO-ADMIN -- classe: resto -- tipo: !
Q1: “sem pergunta” pendente. Falta o aval de construção. O literal do posto já respondido foi “o ato de convocar vence a marca do posto para aquele colab-dia, ou convocacao em posto fechado exige reabrir o posto?” app/docs/BACKLOG.md:2290 e app/docs/RELATO.md:115
Q2: O administrador pode convocar alguém para trabalhar no feriado mesmo quando o posto está marcado como fechado? app/docs/RELATO.md:115
Q3 A: Admin convoca com motivo e recebe aviso de posto fechado. A folha passa a tratar aquele dia como previsto, admite a cobrança de falta e aplica a regra de feriado no pagamento. Alcance hoje nao medido. A leitura registrou 0 dias já editados, 12 postos e 26 vínculos potencialmente expostos. Constrói a tela e adapta os leitores do dia e da cobrança. app/docs/RELATO.md:114–118 e app/docs/BACKLOG.md:2290
Q3 B: Admin teria de reabrir o posto antes de convocar. A folha manteria o dia como feriado sem falta até a convocação válida. Alcance hoje nao medido. Constrói a conferência do posto e a recusa explicada na tela. app/docs/RELATO.md:115 e app/docs/BACKLOG.md:2290
Q4: ja respondida pelo corte FILA-PARALELA-1911-1150 item A, que permite a convocação individual com aviso, e pelo corte RESPOSTA-DESENHO-DIA-CONVOCADO-1911-1145, que a torna dia previsto. app/docs/RELATO.md:115 e app/docs/RELATO.md:118
Q5: vira complemento de O10-TROCA-DE-PLANTAO-ENTRA. O vínculo já foi registrado e a construção aguarda aval. app/docs/RELATO.md:132 e app/docs/BACKLOG.md:2290

## LAUDO-col33-0310 -- classe: contrato 20 -- tipo: corte
Q1: “sem pergunta” pendente. O literal já respondido começa em “(a) rejulgar o resto pela 7b (comp 10 aberta: reversao antes, prova de frota depois), por grupo com DIFF publicado” e oferece também só o grupo de ata movida ou esperar o ciclo natural. app/docs/RELATO.md:4876
Q2: Os registros antigos de outubro devem ser revistos em lote, só onde mudam as horas registradas, ou à medida que cada dia voltar a ser analisado? app/docs/RELATO.md:4876
Q3 A: Admin vê os chamados dos dois grupos atualizados. A folha muda no grupo de horas, e o segundo grupo teve 0 linhas de DiaPago alteradas na comparação. Alcance hoje nao medido. Foram medidos 11 e 7 colaboradores nos dois grupos. Executa em lotes pela porta existente, com comparação e reversão. app/docs/RELATO.md:5071–5077
Q3 B: Admin vê a atualização do primeiro grupo e encaminha os demais aos responsáveis. A folha muda só no primeiro grupo. Alcance hoje nao medido. O primeiro grupo medido tinha 11 colaboradores. Executa um lote restrito. app/docs/RELATO.md:4876 e app/docs/RELATO.md:5071
Q3 C: Admin e folha recebem a atualização quando cada dia for revisto pelo fluxo regular. Alcance hoje nao medido. Dispensa lote específico. app/docs/RELATO.md:4876
Q4: ja respondida pelo corte REJULGAR-COMP10-RESTO-DA-FROTA, opção A, com inclusão posterior de col444 por REJULGAR-COMP10-COL444-E-DERIVA. app/docs/CORTES.md:392 e app/docs/CORTES.md:395
Q5: pode morrer como pendência de corte. Col33 foi resolvido, os grupos foram aplicados e os quatro revertidos foram reaplicados. A divergência de col988 tem registro próprio. O rejulgamento era pré-requisito do trabalho de dia da batida no contrato 20. app/docs/RELATO.md:5263 e app/docs/RELATO.md:5076–5081 e app/docs/RELATO.md:4794 e app/docs/BACKLOG.md:849 e app/docs/BACKLOG.md:1849

## BO-ROTULO-REABRIR-SO-NO-DETALHE -- classe: resto -- tipo: corte
Q1: “sem pergunta”. Falta aval para construir. SMOKES-1211 item 2 já reservou “Reabrir” ao detalhe completo. app/docs/BACKLOG.md:2337
Q2: Como o administrador deve ver a opção “Reabrir” somente quando o chamado puder ser reaberto? app/docs/BACKLOG.md:2337 e app/docs/LEIS-UI.md:39–49
Q3 A: Admin vê no detalhe local só as ações permitidas ao seu perfil. O detalhe integrado oferece comentário. Sem efeito direto na folha. Casos hoje nao medidos. Constrói o filtro por estado e perfil e retira o seletor integrado sem efeito. app/docs/BACKLOG.md:2337 e app/chamados/views.py:292–310 e app/chamados/catalogo/motor.py:139–156
Q3 B: Admin vê ações permitidas nos dois detalhes quando a mudança puder ser gravada. Sem mudança na regra da folha. Casos hoje nao medidos. Constrói a ligação do detalhe integrado às transições autorizadas e confere as duas telas. app/docs/BACKLOG.md:2219–2220 e app/docs/BACKLOG.md:2337
Q4: ja respondida pelo corte SMOKES-1211 item 2 quanto ao lugar e pela LEIS-UI L4 quanto à exibição da ação. O aval de construção continua pendente. app/docs/BACKLOG.md:2337 e app/docs/LEIS-UI.md:39–49
Q5: segue. O smoke de rótulos de 12/11 cobriu 3 de 5 telas e saiu sem salvar. O fechamento por uso de 16/11 é de REABRIR-NAO-DESTROI. app/docs/RELATO.md:2198 e app/docs/RELATO.md:1037 e app/docs/BACKLOG.md:2337

RESUMO: 3 sobem ao Ronald como lei (O275, O176 e O148-EDITAR), 6 já têm resposta de regra no escopo indicado em Q4, e 5 podem morrer ou ficar como complemento. O148-EDITAR conta também como complemento porque o vínculo com O148 já está escrito e a lei segue pendente. app/docs/RELATO.md:417 e app/docs/BACKLOG.md:1553 e app/docs/GRADE-MESTRA.md:1523–1526; demais fontes em Q4 e Q5 de cada ficha.

## 20/11 11:55 · sessao bbbe83f93203 · CADERNO-DE-CORTES-2011 · leitura · origem=Code

Usei a cópia local e os anexos da mensagem. A confirmação do hash pelo Git ficou [nao verificado]. A entrega está no chat, em modo somente leitura.

```text
## DST-L1 -- classe: espurias/HE -- tipo: corte
Q1: “nasce o juiz `decisao_admin_dia_sem_trabalho`?” app/docs/RELATO.md:542. A revisão da alternativa pela hora extra foi marcada como respondida. Restam o registro da divisão dos casos e a medição de como aparecem as correções de escala e as invalidações de batida. app/docs/RELATO.md:451; app/docs/BACKLOG.md:2827
Q2: Quando alguém bate ponto num dia em que não estava escalado, como o administrador registra o que aconteceu? app/docs/RELATO.md:542
Q3: A) Usar a decisão de hora extra. O admin chega a Autorizar ou Não pelo dia; a decisão dispara recálculo e pode liberar a pendência de exportação. A porta existente atendia 55 de 65 dias com duas ou mais batidas na sombra de 18/11, dentro de 113 dias observados; hoje, nao medido. Depois, ligar as telas à porta e dar destino aos casos que ela não recebe. app/docs/BACKLOG.md:1976; app/docs/RELATO.md:461; app/ponto/portas/he.py:175,190,229; app/folha/porta_export.py:676
Q3: B) Registrar o reconhecimento do trabalho numa ação própria do dia. O admin vê esse reconhecimento separado da autorização de hora extra; o efeito financeiro desse reconhecimento teria de ser definido, com alcance nao medido. Depois, construir o registro e a leitura da decisão por pessoa e dia. app/docs/RELATO.md:506,522,542
Q4: ja respondida pelo corte DST-L1-1800 quanto a mandar o dia à decisão do admin. O desenho pela hora extra e a resposta à revisão constam de DSTL1-PELA-PORTA-DA-HE e RESPOSTAS-1811-1312, com registro e medição ainda na fila. app/docs/CORTES.md:549; app/docs/RELATO.md:451; app/docs/BACKLOG.md:1976,2827
Q5: vira complemento de DIA-SEM-TRABALHO-PORTA. O backlog já registra a ação pela hora extra como parte L1 dessa obra. app/docs/BACKLOG.md:1975,1976

## O-JANELA-HE -- classe: espurias/HE -- tipo: smoke
Q1: “sem pergunta”. Falta o retorno do teste de clique dos 13 passos, com filtros, recusa em lote, autorização com motivo, PDF e atalho na Central. app/docs/BACKLOG.md:1255; app/docs/RELATO-ARQUIVO.md:28550,28554,28561,28573
Q2: Os filtros, as decisões de hora extra, o PDF e o atalho da Central funcionam como esperado quando o administrador os usa? app/docs/RELATO-ARQUIVO.md:28550,28573
Q3: A) Roteiro passa. O admin confirma o que viu na tela; autorizar põe o fechamento na fila de recálculo. Cobertura do roteiro inteiro: nao medido. Depois, registrar o resultado e encerrar o teste de clique. app/docs/RELATO-ARQUIVO.md:28554,28565,28573; app/docs/RELATO.md:1037
Q3: B) Algum passo falha. O admin registra o clique e o que a tela mostrou; efeito da falha na folha e alcance: nao medido. Depois, reproduzir o passo e delimitar a correção. app/docs/BACKLOG.md:1255; app/docs/RELATO-ARQUIVO.md:28550,28573
Q4: nao respondida. L-097 define o que deve acontecer com as horas extras; o corte de uso deixou esta confirmação de tela aberta. app/docs/LEIS.md:115; app/docs/RELATO.md:1037
Q5: segue. PRIORIDADE-ESPELHO-VERDADE-E-TRES-CORTES, item 4 de 16/11, fechou BO-HE-RECUSAR-LOTE-500 com 71 atos e zero erro após a cura; O-JANELA-HE ficou aberta com 72 atos desde 01/10 e dois erros anteriores à cura. app/docs/BACKLOG.md:2367; app/docs/RELATO.md:1037

## FRAC5-MINIMO-5 -- classe: resto -- tipo: corte
Q1: “sem pergunta”. Falta o novo corte de pouso da correção guardada, com as pendências da revisão. app/docs/BACKLOG.md:788; app/docs/GRADE-MESTRA.md:1785
Q2: O sistema deve impedir que o administrador agende uma parte das férias com menos de cinco dias? app/docs/BACKLOG.md:788
Q3: A) Manter a correção guardada. O admin vê o aviso atual de até três partes, uma com pelo menos 14 dias; a validação e o efeito na folha seguem os caminhos atuais. O corte registrou 5 agendamentos e 0 com parte menor que 5; alcance atual nao medido. Depois, nenhuma construção nessa opção. app/templates/ferias/agendar.html:10; app/ferias/models.py:264; app/docs/CORTES.md:484
Q3: B) Autorizar a correção guardada. O admin recebe recusa ao agendar ou remarcar fora da regra; ao aprovar, pode ver aviso de aprovação sem agendamento. Sem agendamento, a ausência de férias correspondente não é criada; impacto na folha e alcance atual nao medido. Depois, retomar a cópia guardada, tratar os achados da revisão e medir o histórico. app/docs/BACKLOG.md:788; app/docs/RELATO.md:1626; app/ferias/models.py:388; app/ferias/services.py:774,790
Q4: nao respondida para o novo pouso. FRAC5-SO-O-QUE-O-CORTE-MANDOU escolheu a opção (b) anterior, que liberou o texto e mandou guardar a correção. app/docs/CORTES.md:476,484
Q5: segue. O backlog exige novo corte de pouso e registra que falta medir o histórico. app/docs/BACKLOG.md:788

## ESCALA-NAO-DESCREVE C8 -- classe: espurias/HE -- tipo: !
Q1: “sem pergunta” para o aval atual. O fechamento está suspenso à espera do corte sobre a tabela 2d. A pergunta anterior foi “a amostra fica minutos_do_oraculo apesar da concatenacao, vai a outra fonte, ou o detector exclui (contado) o dia com >1 bloco?” app/docs/RELATO.md:48,122; app/docs/BACKLOG.md:2377
Q2: De quais registros devem sair as sugestões de horário que o administrador vê quando as batidas e a escala cadastrada divergem? app/docs/RELATO.md:48,61,122
Q3: A) Liberar a fonte cortada às 13h. O admin vê a sugestão calculada por essa fonte; o cálculo da sugestão não grava folha. A sombra registrou 233 pessoas e 15 dias com mais de um bloco; alcance atual nao medido. Depois, implementar o brief com o limite conhecido e testar os dias partidos. app/docs/RELATO.md:74,110,122; app/ponto/services/esmeril_espelho.py:239
Q3: B) Usar os turnos fechados do sistema como fonte. As sugestões podem mudar; o efeito na folha dependeria de aplicar a escala pela ação da tela. Na sombra, essa fonte coincidiu com a proposta nova em 139 de 233 e com a antiga em 4; alcance atual nao medido. Depois, rever o brief e testar a dependência do cadastro nessa fonte. app/docs/RELATO.md:48,61; app/templates/escala/cadastro_x_realidade.html:32
Q4: ja respondida pelo corte ORDEM-E-RESPOSTAS-1911-1300, item 4, quanto à pergunta anterior. O fechamento com a tabela posterior continua suspenso. app/docs/RELATO.md:48,74,122
Q5: segue. A obra está sem construtor e espera o aval sobre a tabela 2d. app/docs/BACKLOG.md:2377; app/docs/RELATO.md:48

## JUIZ-DE-DONO-DO-DIA -- classe: contrato 20 -- tipo: corte
Q1: “sem pergunta”. Falta autorizar o critério que atribui a causa ao dia de cada pergunta para decidir entre a cobrança ao colaborador e a lista do admin, ligada ao R2 do placar. app/docs/GRADE-MESTRA.md:59; app/docs/CORTES.md:425,426; app/docs/BACKLOG.md:1201; app/core/placar_estrutural.py:135
Q2: Como saber se o problema daquele dia deve continuar com o funcionário, ir ao administrador ou ser corrigido no sistema? app/docs/BACKLOG.md:1201; app/docs/LEIS.md:122
Q3: A) Manter a cobrança atual. O admin não recebe uma nova classificação na lista; a folha segue pelas regras atuais. A medida de 27/10 registrou 787 perguntas em 394 dias de colaboradores; hoje, nao medido. Depois, nenhuma construção nessa opção. app/docs/CORTES.md:426; app/docs/RELATO.md:4840
Q3: B) Classificar pelo cadastro e pelas batidas do próprio dia. O admin recebe na lista os problemas de cadastro ou de batida; a classificação direciona a ação, e o efeito de eventual correção na folha é nao medido. Casos que trocariam de destino: nao medido. Depois, ligar a classificação aos dados do dia e testar os destinos. app/docs/CORTES.md:425; app/docs/LEIS.md:122; app/ponto/management/commands/e6_oraculo.py:53,85
Q4: nao respondida. L-099 define as categorias e FIO-EXPORTADA-X-O44 mantém a cobrança provisória; o critério para esses dias ficou pendente. app/docs/LEIS.md:122; app/docs/CORTES.md:426; app/docs/GRADE-MESTRA.md:59
Q5: segue. A Grade Mestra ainda registra a falta de critério autorizado para o dia. app/docs/GRADE-MESTRA.md:59

## RAIZ-DA-DISPUTA -- classe: fio morto -- tipo: corte
Q1: F1 — “QUEM FECHA o orfao?” e “reabrir a dona, nascer outra ou Pauta DP?” app/docs/RELATO.md:328
Q2: Quem encerra um chamado que ficou sem pergunta ao funcionário, e para onde vai um motivo novo quando a discussão original já foi encerrada? app/docs/RELATO.md:328
Q3: Chamado sem pergunta, A) Encerrar pelo evento que resolve o caso. O admin vê o encerramento com trilha; efeito sobre retenção na folha e casos que podem fechar: nao medido. Depois, ligar o evento ao fechamento. app/docs/RELATO.md:245; app/folha/fora_da_folha.py:17
Q3: Chamado sem pergunta, B) Mandar à fila de quem pode resolver. O admin vê responsável e ação; o encaminhamento não decide a folha. Alcance efetivo nao medido; o recorte de 19/11 achou 84 chamados vivos sem pergunta de destino, entre 5.504. Depois, construir o encaminhamento com responsável. app/docs/RELATO.md:245; app/folha/fora_da_folha.py:46
Q3: Motivo novo, A) Reabrir a discussão original. O admin vê o motivo no histórico reaberto; efeito na folha depende da resolução e é nao medido. Depois, construir a reabertura com trilha. app/docs/RELATO.md:245,328; app/docs/CORTES.md:515
Q3: Motivo novo, B) Abrir outra discussão. O admin vê uma nova pendência; efeito na folha e alcance nao medido. Depois, unificar a emissão e evitar duplicatas. app/docs/RELATO.md:245,328; app/chamados/services/disputa_emissao.py:540,1110
Q3: Motivo novo, C) Encaminhar ao DP. O admin vê a pauta com a origem; o encaminhamento não altera o gravado. Alcance nao medido nas três opções: 24.399 é o estoque de perguntas em discussões fechadas, e o instante de chegada do motivo não é registrado. Depois, construir a pauta com vínculo à origem. app/docs/RELATO.md:245,328; app/docs/LEIS.md:125
Q4: nao respondida para as duas perguntas da F1. RAIZ-F1-UM-ELEITOR-DO-DESTINO resolveu a escolha anterior do chamado de destino; BO-DISPUTA-DESENHO-RAIZ registrou a questão do motivo novo na F1. app/docs/RELATO.md:2453,328; app/docs/CORTES.md:515
Q5: segue. A leitura de 19/11 confirmou as duas perguntas abertas e mediu o estoque, sem identificar quantos motivos chegaram depois do encerramento. app/docs/RELATO.md:245,328

RESUMO: 2 obras sobem ao Ronald como lei, JUIZ-DE-DONO-DO-DIA e RAIZ-DA-DISPUTA F1, esta com duas perguntas (app/docs/GRADE-MESTRA.md:59; app/docs/RELATO.md:328). 2 têm corte para a pergunta de desenho já registrada, DST-L1 e C8 (app/docs/CORTES.md:549; app/docs/RELATO.md:74,122). 1 pode virar complemento, DST-L1 (app/docs/BACKLOG.md:1975,1976).
```

## 20/11 11:54 · sessao 76801b9e1966 · - · - · origem=Ronald (tmux)

## FAIXA-NO-CADASTRO -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta autorizar a execução após o contrato 20. O desenho registrado prevê campo no cadastro e migração a partir do mapa atual. app/docs/BACKLOG.md:2334
Q2: Quando chegar a vez dessa mudança, o administrador poderá cadastrar qual ausência prevalece quando duas regras caem no mesmo dia? app/docs/BACKLOG.md:2334
Q3: A) Autorizar na vez registrada. Tela ganha o campo. Na folha, mudanças posteriores na prioridade podem alterar qual ausência vale. Alcance atual não medido. Construir campo, migração e leitura única. app/docs/BACKLOG.md:2334; app/ponto/precedencia.py:45
Q3: B) Adiar o aval. Tela mantém as escolhas atuais e a folha segue a prioridade vigente. Alcance atual não medido. Nenhuma construção neste item. app/templates/ponto/partials/painel_tipo_ausencia.html:28; app/ponto/precedencia.py:76
Q4: A direção já está registrada pelo MANHA-1211, item 7. O aval para executar segue pendente. app/docs/BACKLOG.md:2334; app/docs/RELATO.md:2197
Q5: vira complemento de EFEITO-NONE-UM-LEITOR, na posição POS-20 e com seu aval próprio. app/docs/GRADE-MESTRA.md:1325; app/docs/GRADE-MESTRA.md:1328; app/docs/BACKLOG.md:2334

## DISPUTA-FECHADA-SEM-RESOLUCAO -- classe: resto -- tipo: corte
Q1: "sem pergunta". O registro pede corte para exigir resolução ao fechar e identifica falta de trilha do motivo. app/docs/BACKLOG.md:1222
Q2: Como o administrador deve saber por que uma disputa foi encerrada quando a explicação está em branco? app/docs/BACKLOG.md:1222
Q3: A) Usar o motivo já gravado nos fechamentos novos. A tela de regularização já pede texto. Folha segue pela mesma apuração. Alcance atual não medido. Nenhuma nova regra de fechamento. app/chamados/models.py:2059; app/chamados/models.py:2095; app/chamados/services/acoes_disputa.py:121
Q3: B) Mostrar no detalhe quais encerramentos antigos ficaram sem motivo registrado. Sem alteração de folha. Aferição de 30/09, 30 de 4.682 com resolução vazia, estoque atual não medido. Construir o aviso a partir do registro existente. app/docs/BACKLOG.md:1222; app/chamados/migrations/0049_disputa_motivo_fechamento.py:11
Q4: ja respondida pelo corte C7-DISPUTA-FECHA-POR-UM-JUIZ quanto à trilha dos fechamentos novos. O campo de texto pode ficar vazio com motivo estruturado gravado. app/docs/BACKLOG.md:1618; app/docs/LEIS.md:65; app/chamados/models.py:2040; app/chamados/models.py:2095
Q5: pode morrer como obra avulsa de fechamento novo. O estoque antigo segue sem aferição atual e a migração preserva o legado em branco. app/chamados/models.py:2095; app/chamados/migrations/0049_disputa_motivo_fechamento.py:11; app/docs/BACKLOG.md:1222

## VIGENCIA-SOBREPOSTA-COL515 -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta decidir o tratamento de dois vínculos históricos inativos com horários opostos no mesmo período. app/docs/BACKLOG.md:1223
Q2: O que fazer quando o histórico de uma pessoa mostra dois horários de trabalho diferentes para os mesmos dias? app/docs/BACKLOG.md:1223
Q3: A) Preservar histórico e avisar ou recusar eventual reativação conflitante. Administrador recebe a explicação ao tentar reativar, folha sem recálculo por essa opção. Medida histórica de 1 pessoa, 2 vínculos e 11 dias, frota atual não medida. Construir a guarda pela linha do tempo. app/docs/BACKLOG.md:1223; app/colaboradores/services/vinculo.py:505
Q3: B) Corrigir as datas históricas com evidência e trilha. O admin vê o período corrigido. Efeito na folha não medido. Mesmo alcance histórico. Construir tratamento do passivo com as regras de competência. app/docs/BACKLOG.md:1223; app/docs/BACKLOG.md:1248; app/docs/LEIS.md:155
Q4: nao respondida para o histórico inativo. VINCULO-LINHA-DO-TEMPO já trata a linha ativa e seu censo filtra `ativa=True`. app/docs/BACKLOG.md:1248; app/colaboradores/services/vinculo.py:505; app/colaboradores/services/vinculo.py:523
Q5: vira complemento de VINCULO-LINHA-DO-TEMPO. app/docs/GRADE-MESTRA.md:1078; app/docs/GRADE-MESTRA.md:1082; app/docs/RELATO.md:1191

## O62 -- classe: espurias/HE -- tipo: corte
Q1: "sem pergunta". Falta corrigir a resposta que diz “escala sem intervalo” quando há duração cadastrada e faltam os horários da pausa no registro do dia. app/docs/BACKLOG.md:1469; app/ponto/juiz_batida.py:294
Q2: Quando a escala prevê uma pausa e o dia não tem os horários dela, como a pausa deve aparecer e contar na folha? app/docs/BACKLOG.md:1469; app/docs/LEIS.md:80
Q3: A) Mostrar duração prevista e falta dos horários. Folha segue a apuração atual. Construir correção de resposta e leitores de tela. Amostra antiga de 3 de 5 pessoas, 17 de 20 dias no col87. Frota atual não medida. app/docs/BACKLOG.md:1469; app/ponto/motor_calculo_v2.py:669
Q3: B) Levar também a duração aos leitores de cálculo que ainda a omitem. Tela distingue pausa prevista de pausa registrada. Folha aplica a duração onde a regra ainda falta. Efeito atual não medido. Construir a correção na fonte e medir por rubrica antes do pouso. app/docs/LEIS.md:80; app/docs/LEIS.md:193; app/docs/LEIS.md:142
Q4: ja respondida pelo corte INTERVALO-CADASTRADO-NAO-VIRA-HE, L-032. app/docs/LEIS.md:80
Q5: segue para execução. A proteção no motor já existe e o retorno incorreto ainda está na cópia. app/docs/RELATO-ARQUIVO.md:6403; app/docs/RELATO-ARQUIVO.md:6412; app/ponto/juiz_batida.py:304

## 20/11 11:54 · sessao 73b72598e64b · - · - · origem=Ronald (tmux)

## O176 -- classe: resto -- tipo: corte
Q1: "o que o pacote declara tocar (`declarados()`) ja esta na arvore?" app/docs/BACKLOG.md:1553
Q2: O sistema pode liberar o trabalho pelo título ou precisa conferir se as mudanças daquele pacote já entraram? app/docs/BACKLOG.md:1553
Q3: (a) Usar título. Tela do admin e folha sem efeito direto. Na fila de trabalho, nomes iguais podem liberar um alvo ainda ocupado. Manter o verificador atual. app/core/esteira_vigia.py:264,302-314; bin/fabricante_alvo.py:128-137.
Q3: (b) Conferir o conteúdo entregue. Tela do admin e folha sem efeito direto. A fila só libera com prova de entrega. Construir a comparação compartilhada e teste de dois pacotes com o mesmo título. app/docs/BACKLOG.md:1553; app/docs/CORTES.md:317.
Q3: Alcance hoje nao medido nas duas. Em 04/10, 0 liberações erradas entre 47 rótulos de pacote. app/docs/BACKLOG.md:1553
Q4: nao respondida. ESTEIRA-SECA-1-E-2-AGORA já manda usar uma fonte. L-011 exige o corte para a autoridade nova. app/docs/CORTES.md:317; app/docs/LEIS.md:69.
Q5: segue. A guarda ainda usa o título e a mudança de critério espera corte. app/core/esteira_vigia.py:302-314; app/docs/BACKLOG.md:1553.

## O94 -- classe: resto -- tipo: !
Q1: "o gravado de 09 passa a nao pagar essas 283 h (e elas viram cobranca a resolver com o DP/supervisao), ou ficam como estao ate a cobranca ser respondida?" app/docs/PENDENTES_RONALD.json:369
Q2: As horas de setembro com saída faltando devem sair do fechamento antes que a supervisão esclareça os casos? app/docs/PENDENTES_RONALD.json:369
Q3: (a) Admin recebe cobrança pelas saídas. A retificação retiraria 283,01 h do fechamento dos 14 da medição de 27/09. Preparar nova medição e retificação com trilha, sujeita à guarda da 09 e ao aval. app/docs/BACKLOG.md:1495; app/ponto/services/fechamento.py:19-41.
Q3: (b) Admin/DP confere pago e calculado. Folha emitida preservada durante a apuração. Os mesmos 14 da medição histórica. Preparar a pauta com os dois valores pela porta existente. app/docs/BACKLOG.md:1495; app/pautas/management/commands/pauta_pago_x_devido_exportada.py:71.
Q3: Alcance hoje nao medido. A medida publicada é de horas, pela soma de trabalho e folga trabalhada. app/docs/BACKLOG.md:1495.
Q4: nao respondida quanto ao ! nominal. L-113 altera L-092. O corte BO-B de 23/10 deu ! para a guarda e a medição da 09, com o apply pendente. app/docs/LEIS.md:125,155; app/docs/RELATO.md:5054.
Q5: segue como ! de dinheiro. Código da cura no ar e decisão sobre o gravado pendente. app/docs/BACKLOG.md:1495; app/docs/PENDENTES_RONALD.json:366-373.

## HIGIENE-DE-CONTEXTO -- classe: resto -- tipo: !
Q1: "pode QUEBRAR o paragrafo em dois -- regra num, historia no outro -- sem mudar uma palavra". app/docs/RELATO-ARQUIVO.md:27488-27489
Q2: Podemos separar o texto que manda fazer algo da história que explica por quê, preservando as palavras? app/docs/RELATO-ARQUIVO.md:27481-27490
Q3: (a) Separar e mover a história com referência para consulta. Tela e folha sem efeito. Hoje nao medido. A O224 já fez a passagem e conferiu o texto movido. app/docs/BACKLOG.md:1321; app/docs/LAPIDES.md:23-34.
Q3: (b) Conservar os trechos juntos. Tela e folha sem efeito. Hoje nao medido. Nenhuma construção documental para esses trechos. Em 01/10, 5 trechos de história estavam misturados com regra, em 27 parágrafos com zero inteiramente históricos. app/docs/RELATO-ARQUIVO.md:27476-27490.
Q4: ja respondida pelo corte L-109, aplicado com o aval da O224, DIETA-DO-CLAUDE-MD, para mover narrativa e preservar a regra palavra por palavra. app/docs/LEIS.md:135; app/docs/BACKLOG.md:1321.
Q5: pode morrer como pendência independente. Itens 1–4 e 6 fechados, passagem do item 5 comprovada na O220/O224. A meta posterior de tamanho está em CLAUDE-MD-ENXUTO. app/docs/BACKLOG.md:1388,1337,1321,1081.

## O148-EDITAR -- classe: resto -- tipo: corte
Q1: "sem pergunta". Falta cortar a regra proposta neste literal: "calendario com uso edita por versao com vigencia, nunca no lugar". app/docs/BACKLOG.md:819; app/docs/CORTES.md:390
Q2: Quando um calendário de folgas já é usado, o administrador deve salvar na mesma ficha ou criar uma nova versão que comece a valer em uma data escolhida? app/docs/CORTES.md:390
Q3: (a) Admin escolhe a data de início e consulta versões. Na folha, a regra do calendário novo vale dali em diante. Construir versão, vigência, trilha e editor. app/docs/CORTES.md:390.
Q3: (b) Admin continua salvando na ficha existente. As folgas e dias futuros do vínculo podem mudar, e o clique salva com recálculo da folha desligado. Preservar a porta atual. app/escala/views.py:319,354-357; app/escala/folgas.py:571-601,605-640.
Q3: Alcance hoje nao medido nas duas. A leitura de 16/11 encontrou 72 calendários em uso, sem cobrir todos os casos de folga materializada. app/docs/BACKLOG.md:842; app/escala/models.py:1582-1592.
Q4: nao respondida. O148-EDITAR é registro de ato e pede corte para a versão. O corte sobre arquivar responde à exclusão. app/docs/CORTES.md:390; app/docs/LEIS.md:206,317; app/docs/BACKLOG.md:2751.
Q5: vira complemento de O148, como já registrado. O smoke de 16/11 cobriu o editor de três grades e nada gravou. app/docs/GRADE-MESTRA.md:1523-1527; app/docs/BACKLOG.md:819-820,1524.

Nas quatro fichas, 2 sobem como lei, 1 já respondida e 2 podem morrer ou virar complemento. O148-EDITAR conta como lei pendente e como complemento. O94 segue como !. app/docs/BACKLOG.md:819,1321,1495,1553.

## 20/11 11:54 · sessao 921ff40c3d99 · - · - · origem=Ronald (tmux)

## DIA-CONVOCADO-PELO-ADMIN -- classe: resto -- tipo: !
Q1: “sem pergunta” pendente. Falta aval de construção. As duas decisões de desenho constam como respondidas. app/docs/BACKLOG.md:2290, app/docs/RELATO.md:115, app/docs/RELATO.md:118
Q2: Pode liberar a tela para convocar uma pessoa para trabalhar no feriado? app/docs/BACKLOG.md:2290
Q3: (a) Autorizar a tela com motivo e aviso de posto fechado. A convocação torna o dia previsto, permite cobrança de falta e mantém a dobra conforme o ciclo. Última leitura registrou 0 dias editados e 26 vínculos potenciais em 12 postos fechados. Alcance atual não medido. Construir tela e ajustar os leitores listados no desenho. app/docs/RELATO.md:114–118, app/ponto/portas/celula.py:572–591, app/ponto/motor_calculo_v2.py:914–934
Q3: (b) Aguardar autorização. A tela continua sem convocação e a folha continua com o tratamento atual do feriado. Alcance atual não medido. O desenho aguarda aval. app/docs/BACKLOG.md:2290, app/docs/RELATO.md:114–118
Q4: ja respondida pelo corte RESPOSTA-DESENHO-DIA-CONVOCADO-1911-1145 e pelo corte FILA-PARALELA-1911-1150 item A. app/docs/RELATO.md:115, app/docs/RELATO.md:118
Q5: vira complemento de O10-TROCA-DE-PLANTAO-ENTRA, onde já está registrada. app/docs/RELATO.md:132, app/docs/BACKLOG.md:2290

## LAUDO-col33-0310 -- classe: resto -- tipo: corte
Q1: “sem pergunta” pendente. A pergunta histórica começava “(a) rejulgar o resto pela 7b (comp 10 aberta: reversao antes, prova de frota depois), por grupo com DIFF publicado”; o corte escolheu essa opção e os atos estão registrados. app/docs/RELATO.md:4876, app/docs/CORTES.md:392, app/docs/RELATO.md:5076–5077
Q2: Os registros antigos de outubro devem ser atualizados agora em todos os casos, só onde mudam as horas, ou conforme cada dia for revisto? app/docs/RELATO.md:4876
Q3: (a) Atualizar os chamados de ambos os grupos e as horas onde mudarem. A leitura de 27/10 registrou 11 e 7 colaboradores nos grupos, com 0 DiaPago no segundo. Alcance atual não medido. Executar lotes pela rotina existente com comparação e reversão. app/docs/RELATO.md:5069–5077
Q3: (b) Atualizar chamados e folha só no grupo que muda as horas, encaminhando os outros casos aos responsáveis. A leitura de 27/10 registrou 11 colaboradores no grupo. Alcance atual não medido. Executar o lote restrito e fazer a triagem à parte. app/docs/RELATO.md:4876, app/docs/RELATO.md:5071–5072
Q3: (c) Atualizar tela e folha por revisão regular, sem lote. Alcance atual não medido. app/docs/RELATO.md:4876
Q4: ja respondida pelo corte REJULGAR-COMP10-RESTO-DA-FROTA, com o complemento REJULGAR-COMP10-COL444-E-DERIVA. app/docs/CORTES.md:392, app/docs/CORTES.md:395
Q5: pode morrer como pendência de corte. O caso nominal está resolvido, os lotes foram aplicados e a divergência de col988 tem item próprio. app/docs/RELATO.md:5263, app/docs/RELATO.md:5076–5077, app/docs/RELATO.md:4794, app/docs/BACKLOG.md:849

## BO-ROTULO-REABRIR-SO-NO-DETALHE -- classe: resto -- tipo: corte
Q1: “sem pergunta”. Falta aval de construção. O lugar de “Reabrir” já foi registrado no SMOKES-1211 item 2. app/docs/BACKLOG.md:2337
Q2: Pode corrigir o menu para o admin só ver “Reabrir” quando o chamado puder ser reaberto? app/docs/BACKLOG.md:2337
Q3: (a) Mostrar no detalhe local apenas ações aceitas pelo sistema e manter comentário no detalhe integrado. A correção da oferta não grava folha. Casos atuais não medidos. Construir filtro por estado e perfil e retirar o seletor ignorado. app/chamados/catalogo/motor.py:139–156, app/chamados/views.py:292–310, app/docs/BACKLOG.md:2337
Q3: (b) Mostrar no detalhe integrado ações com gravação comprovada e aplicar o mesmo filtro no local. A regra da folha permanece, efeitos indiretos não medidos. Casos atuais não medidos. Construir a ligação às transições autorizadas com aval próprio. app/docs/BACKLOG.md:2219–2220, app/chamados/services/fecho_runtime.py:146–175
Q4: ja respondida pelo corte SMOKES-1211 item 2 quanto ao lugar, e pela LEIS-UI L4 quanto a oferecer apenas ação executável. app/docs/BACKLOG.md:2337, app/docs/LEIS-UI.md:39–49
Q5: segue pelo aval de construção. O smoke de 12/11 verificou os rótulos da obra anterior, com três telas cobertas e nenhuma gravação. app/docs/BACKLOG.md:2337, app/docs/RELATO.md:2198

## 20/11 11:54 · sessao 9d752332afc2 · - · - · origem=Ronald (tmux)

O310-RAIOX — resto, corte. Sem pergunta literal para a persistência do histórico. O corte O256-RAIO-X-DO-RETRATO já manda comparar dois retratos gravados e informar quando falta o anterior. O aval específico para modelo e migração permanece pendente. A lente publicada não moveu a folha e a obra já é complemento de O310. Opções para o histórico são guardar só a foto anterior ou guardar fotos datadas, ambas sem efeito de folha e com alcance atual não medido. app/docs/BACKLOG.md:1727, app/docs/CORTES.md:357, app/folha/models.py:366, app/folha/retrato_folha.py:723.

O34 — resto, corte de desenho. Sem pergunta literal. A linha traz as três opções para o Ronald: reconstruir o ajuste sozinho, falhar exibindo o que mudou ou dar prazo para recriar ou descartar. Nenhum efeito direto na tela do admin ou na folha. Alcance atual não medido. Na passada registrada foram 11 de 14 falhas, 8 de âncora e 3 de padrão, com 17 fatias em espera. ZUMBIDO já proíbe o terceiro relance pelo mesmo motivo e exige registro da causa, mas não escolhe uma das três opções. Vira complemento de O31, como já consta da Grade. app/docs/BACKLOG.md:1436, app/docs/CORTES.md:307, app/docs/GRADE-MESTRA.md:1234, app/docs/GRADE-MESTRA.md:1238.

O37 — resto, !. Literal: “o gravado de competencia exportada e reescrito, ou fica e a diferenca vira Pauta DP?” O residual histórico medido foi de 22 dias em dois colaboradores, 161 h na competência 08, sem edição humana. As opções históricas eram corrigir o gravado pela porta, com a tela refletindo os dias corrigidos e impacto efetivo por rubrica ainda não medido, ou preservar a folha e destinar o comparativo ao DP. Já respondida pelo corte COMPLEMENTO-1 RESPOSTAS-R4-E1PORTA-1611 item 3, que manda passivos exclusivamente anteriores à 09 à lista do DP. Pode morrer como espera autônoma e já é complemento de O54. Entrada efetiva na lista do DP [nao verificado]. app/docs/RELATO-ARQUIVO.md:24091, app/docs/RELATO-ARQUIVO.md:24120, app/docs/RELATO.md:1012, app/docs/BACKLOG.md:1460, app/docs/GRADE-MESTRA.md:881.

O296 — resto, !. Sem pergunta literal. Falta autorizar a troca no comentário do motor. As opções são trocar os nove comentários fora da zona ou incluir o décimo com autorização nominal. Ambas alteram só comentários, com efeito nulo na tela do admin e na folha. São 10 locais na medição histórica, alcance atual não medido. A resposta da O295 mantém o histórico público e não concede a autorização nominal do motor. Segue pelo !. O comentário público registrado separadamente integra um complemento, com contagem própria. app/docs/BACKLOG.md:1703, app/docs/BACKLOG.md:1704, app/docs/BACKLOG.md:1732, app/docs/PENDENTES_RONALD.json:2671, CLAUDE.md:142.

## 20/11 11:54 · sessao 5466f272498e · - · - · origem=Ronald (tmux)

## REJULGAR-61-COBPARC -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` próprio com a lista das 55 atas de competências entregues. app/docs/BACKLOG.md:906; app/docs/GRADE-MESTRA.md:657
Q2: Você autoriza revisar a lista de registros antigos de faltas parciais de meses já entregues? app/docs/BACKLOG.md:906
Q3 (a): Preservar as atas, levar as diferenças provadas para conferência do DP e preservar a folha entregue. Alcance atual não medido, 55 entregues e 6 trancadas na medida de 08/11. Depois, preparar a lista e a comparação. app/docs/BACKLOG.md:906; app/docs/LEIS.md:125
Q3 (b): Autorizar revisão nominal, com registro do motivo e comparação para o DP. A folha entregue só pode ser retificada pela exceção autorizada. Alcance atual não medido, 55 no lote que espera `!`. Depois, conferir o lote e reutilizar a porta existente. app/docs/BACKLOG.md:906; app/ponto/portas/regen_exportada.py:87,111-117
Q4: nao respondida. TRES-DO-TOPO-1109 item 2 resolve a regra da COBPARC e exige aval separado para o acervo. app/docs/RELATO.md:2920
Q5: segue. O registro separa a revisão histórica da obra da regra atual. app/docs/BACKLOG.md:897,906

## RESIDUO-PREMIG-NO-BIND-MOUNT -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para apagar os arquivos da instalação em uso. app/docs/BACKLOG.md:1761; app/docs/GRADE-MESTRA.md:421
Q2: Você autoriza remover os arquivos antigos da instalação ou quer deixá-los onde estão? app/docs/BACKLOG.md:1761
Q3 (a): Manter os arquivos. Tela e folha iguais, inventário atual não medido, 64 arquivos registrados em 19/10. Depois, usar a classificação já corrigida. app/docs/BACKLOG.md:1761; app/docs/RELATO-ARQUIVO.md:43426
Q3 (b): Conferir a lista e remover os arquivos autorizados. Tela e folha sem mudança prevista, alcance atual não medido, 64 registrados em 19/10. Depois, fazer a limpeza com o inventário conferido. app/docs/BACKLOG.md:1761
Q4: nao respondida. L-009 exige autorização para apagar. app/docs/LEIS.md:67
Q5: pode morrer como bloqueio do censo. Já se distinguem os ignorados e o registro da correção mostra `EXTRA-VIVO=0`. Delete efetivo [nao verificado]. app/docs/RELATO-ARQUIVO.md:43426

## JANELA-DO-NOTURNO-CRAVADA -- classe: espurias/HE -- tipo: !
Q1: "sem pergunta". Falta o `!` para alterar o cálculo protegido e permitir cadastrar os horários noturnos da convenção. app/docs/BACKLOG.md:1769; CLAUDE.md:142
Q2: Você autoriza que cada convenção tenha seu próprio horário de trabalho noturno no sistema? app/docs/BACKLOG.md:1769
Q3 (a): Manter 22h–05h. A tela continua sem campos de horário, a apuração conserva essa faixa, casos com outra faixa não medidos. Depois, nenhuma construção. app/docs/BACKLOG.md:1769; app/ponto/motor_calculo_v2.py:101,103
Q3 (b): Cadastrar início e fim pela tela. Adicional e parcela noturna da hora extra podem mudar onde a convenção estabelecer faixa diferente, casos não medidos. Depois, construir campos, leitura comum e testes de cálculo. app/docs/LEIS.md:64; app/ponto/motor_calculo_v2.py:983-987,1014-1022
Q4: nao respondida quanto ao `!`. L-006 já define que uma regra variável nasce no cadastro pela tela. app/docs/LEIS.md:64; CLAUDE.md:142
Q5: segue. O motor ainda fixa os horários e o aval segue exigido. app/ponto/motor_calculo_v2.py:101,103; app/docs/BACKLOG.md:1769

## PROTEGE-VAZIA-EM-55-LEIS -- classe: resto -- tipo: !
Q1: "sem pergunta". Falta o `!` para alterar as linhas de leis já escritas. app/docs/BACKLOG.md:1755; app/docs/RELATO-ARQUIVO.md:299
Q2: Você autoriza conferir as regras já publicadas e marcar os trechos de código que elas protegem expressamente? app/docs/LEIS.md:43-48; app/docs/RELATO-ARQUIVO.md:299
Q3 (a): Manter a coluna. Tela e folha iguais, 55 candidatas na triagem desta cópia e total elegível não medido. Depois, nenhuma construção. app/docs/LEIS.md:57; bin/tests/test_lei_protege_sitio.sh:49-91
Q3 (b): Autorizar a conferência. Tela e folha iguais, 55 candidatas e total elegível não medido. Depois, preencher só as proteções explícitas e conferir a trava em ato próprio. app/docs/LEIS.md:43-48,57,131; bin/tests/test_lei_protege_sitio.sh:49-91
Q4: nao respondida quanto ao `!`. LEI-PROTEGE-SITIO já resolve o critério de preenchimento. app/docs/RELATO-ARQUIVO.md:37575-37589; app/docs/LEIS.md:43-48
Q5: segue. A L-097 ainda exemplifica proteção explícita com coluna vazia. app/docs/RELATO-ARQUIVO.md:780; app/docs/LEIS.md:115

## 20/11 11:54 · sessao 86af01f9c4a6 · - · - · origem=Ronald (tmux)

## O275 -- classe: espurias/HE -- tipo: corte
Q1: “O FLAG NOVO DA O275 PODE SER LIGADO NUM TIPO DE PLANTAO (`12x36`/`24x48`), OU O PLANTAO RESPONDE SO PELA PRECEDENCIA QUE JA EXISTE?” app/docs/RELATO-ARQUIVO.md:746
Q2: O administrador pode transformar um feriado em dia de trabalho mesmo quando ele cai na folga do plantão? app/docs/RELATO-ARQUIVO.md:748
Q3: A) Opção de cadastro também nos plantões, exceto intermitente. A folha passa a receber o feriado como dia previsto onde a opção se aplicar e a regra da dobra cabe à O277. Alcance atual não medido, censo histórico identificou 337 vínculos 12x36 expostos e 0 tipos ligados. Atualiza e confere a implementação guardada. app/docs/RELATO-ARQUIVO.md:749-753; app/docs/BACKLOG.md:1682-1684; app/docs/RELATO.md:1771
Q3: B) Opção limitada a escalas que seguem dias da semana. Os plantões mantêm o previsto atual e a folha recebe o feriado previsto nas escalas alcançadas. Alcance atual não medido, censo anterior achou 1 vínculo no alvo e 4 tipos cadastrados como 12x36 que exigiriam recadastro. Acrescenta a restrição e ajusta os testes. app/docs/RELATO-ARQUIVO.md:749-753
Q4: não respondida. A precedência atual preserva a folga do plantão, e o alcance da opção nova continua em corte. app/ponto/precedencia.py:13-18; app/docs/RELATO.md:1771
Q5: segue. Implementação guardada e publicação retida pela escolha. app/docs/BACKLOG.md:1682

## O10-D-PERMUTA-RECIPROCA -- classe: contrato 20 -- tipo: corte
Q1: “(a) DNA carrega marcos do cedente ou (b) d1=d2 fora da porta”. Demais literais sobre carta, falha de gravação, “cede so plantao”, descansos e `!` de escala [nao verificado]. app/docs/RELATO.md:886-887; app/core/contratos_estruturais.py:113-125
Q2: Na troca de plantão no mesmo dia, quem assume recebe os horários de quem cede ou a operação recusa a troca? app/docs/RELATO.md:887
Q3: A) Tela permite a troca e mostra os horários cedidos. A folha usa o plantão assumido como dia normal, com adicionais cabíveis pelo contrato da troca. Alcance hoje não medido. Constrói a preservação dos horários do cedente. app/docs/RELATO.md:887; app/docs/CORTES.md:286
Q3: B) Tela recusa esse uso da troca e a folha conserva a previsão anterior. Alcance hoje não medido. Constrói a recusa explícita, se houver novo corte que permita essa restrição. app/docs/RELATO.md:883-887; app/docs/RELATO.md:2719
Q4: ja respondida pelo corte LEI-O10-D-PERMUTA quanto a permitir a mesma data, reafirmado na preparação de 16/11. Não respondida quanto a como levar os horários a quem assume e às demais pendências listadas na revisão. app/docs/RELATO.md:2719; app/docs/RELATO.md:1055; app/docs/CORTES.md:528; app/docs/RELATO.md:886-887
Q5: segue. A cópia aprovada ainda espera os cortes residuais e o `!` de escala. app/docs/RELATO.md:879; app/docs/RELATO.md:886

## O14 -- classe: resto -- tipo: smoke
Q1: “sem pergunta”. Falta “SMOKE de olho no app de um noturno 12x36 (ex.: col174 ou col788) depois do deploy.” app/docs/PENDENTES_RONALD.json:1587; app/docs/BACKLOG.md:1414
Q2: No celular de quem faz plantão à noite, a saída da madrugada aparece no mesmo dia que as outras marcações do plantão? app/docs/PENDENTES_RONALD.json:1587
Q3: A) Conferência passa. Admin já vê o dia pela mesma origem. O app alinha marcações e turno ao dia certo e lê o valor de folha já calculado. Alcance atual não medido. Registra a conferência. app/api/views.py:1440-1453; app/api/views.py:1475-1485; app/docs/RELATO-ARQUIVO.md:27288-27293
Q3: B) Conferência falha. Registra o dia divergente visto no app e confronta com o dia da tela do admin. O teste por si só não altera folha. Alcance atual não medido. A correção depende da falha observada [nao verificado]. app/docs/PENDENTES_RONALD.json:1587; app/docs/RELATO-ARQUIVO.md:27292-27293
Q4: não respondida quanto à conferência no aparelho. app/docs/BACKLOG.md:1414
Q5: segue. O registro da redução de avisos em 126 dos 368 colaboradores é da medição anterior do servidor, e o celular continua pendente. app/docs/RELATO-ARQUIVO.md:27288-27293

## O261-E-F5C-PERGUNTA-SEM-ACAO-POSSIVEL -- classe: fio morto -- tipo: corte
Q1: “esta PERGUNTA ja foi materializada de fato (clique morto)?” é a pergunta que o código já responde. O que ficou pendente foi o corte formal para registrar uma autoridade própria, se essa forma for mantida. app/docs/BACKLOG.md:1002; app/chamados/services/raiox.py:160-194
Q2: Antes de mostrar um botão ao administrador, o sistema consegue saber se a ação já fez efeito? app/chamados/services/fio.py:310-319
Q3: A) Registra o critério por pergunta separadamente. Tela mantém botão apenas quando há ação. Folha segue consultando a rota do dia. Alcance atual não medido. Formaliza o registro e conserva o critério atual. app/docs/BACKLOG.md:1002; app/chamados/services/fio.py:310-319; app/folha/fora_da_folha.py:69-73
Q3: B) Leva o leitor residual da tela à migração da tarefa do dia, que já consulta a prova do efeito. Folha já lê essa tarefa. Alcance atual não medido. Migra o leitor residual com prova de comportamento. app/docs/BACKLOG.md:2112; app/chamados/services/tarefa_do_dia.py:142-147; app/folha/fora_da_folha.py:69-73
Q4: ja respondida pelo corte CLIQUE MORTO de 02/09. O efeito efetivo tira a ação. O registro sem efeito não basta. O corte LAUDO-3010-JUIZ-DA-ACAO-DO-ADMIN trata da rota do dia. app/chamados/services/raiox.py:74-95; app/docs/CORTES.md:457; app/docs/BACKLOG.md:1002
Q5: vira complemento de FIO-E0-V4-E3-LEITORES. A tela ainda tem leitor residual, e a regra do efeito já é lida pela tarefa. app/docs/BACKLOG.md:2112; app/templates/chamados/partials/_card_atendimento.html:214; app/chamados/services/tarefa_do_dia.py:142-147

## 20/11 11:31 · sessao 31268814822e · E4C-F5 · verificador · origem=Code

Revisei os dois itens por leitura estática da cópia fornecida. O hash informado, a execução dos controles e os totais de frota ficaram [nao verificado]. O único resultado de execução usado abaixo é o log anexado. `medida4.log:9–16`

## P1 — As premissas conferem?

### Item 1 — A1(c), saída (a)

- **(a) Relógio e trava.** `_materializar_ausencia_de_resposta` cria ou obtém a ausência, lê `agora` uma vez, materializa a lista de perguntas-hora do mesmo dia e calcula `_via`. Tudo isso antecede a trava do colaborador, adquirida dentro do `atomic` de cada irmã. O mesmo `agora` vai para `carimbar` e para a linha `ABSORCAO`. A resposta humana faz o inverso na sua própria transação: trava, relê e só então lê seu relógio. `app/chamados/services/materializacao.py:999,1024–1069` · `app/chamados/services/disputa_emissao.py:1478–1495`
- **(b) Recusa e trilha.** `carimbar` consulta C6 com o carimbo aplicado em memória. Se C6 devolver viva, restaura a instância e retorna `False`. O materializador acrescenta o PK a `recusadas`, chama `evento` com `ACAO_IRMA_VIVA_NA_AUSENCIA` e devolve a lista. Há teste que exige o PK, carimbo vazio e uma linha de auditoria. O chamador do escape descarta o retorno da lista. `app/chamados/models.py:3346–3365` · `app/chamados/services/materializacao.py:1070–1094` · `app/chamados/tests/test_o261e_f2_fila.py:241–258` · `app/chamados/services/disputa_emissao.py:1557–1560`
- **(c) Dado e leitor.** A porta humana grava `PerguntaDisputa.respondida_em` e uma `DecisaoPergunta.RESPOSTA` com o mesmo `ato_em`, texto, autor e `resposta_mantem_validacao=False`. `DecisaoPergunta.leitura` ordena a sequência real e sintética e entrega o instante da resposta vigente. `composta` leva esse instante e o texto da projeção. Já existe o corte estrito de horizonte em `projecao_temporal_da_pergunta`: uma resposta vigente some da vista se seu instante for maior que o horizonte. Esse leitor entrega a distinção temporal sobre a composta, sem uma segunda ordenação. A composta não expõe a origem humana da linha e também pode trazer instante de resposta automática ou sintética. `app/chamados/services/disputa_emissao.py:1493–1508,1526–1531` · `app/chamados/models.py:4284–4289,4306–4317,4333–4362` · `app/chamados/juizes.py:1383–1403`
- **(d) B foi reproduzido.** O escape `ATESTADO` entra pela porta real. O teste interpõe a resposta `19:02` em T2 na chamada da trava e força a releitura antes do `carimbar` real com T1. Com texto e validação vazia, `carimbar` põe T1 na coluna. A leitura insere a validação sintética em T1 e a `RESPOSTA` humana de T2 a desfaz. `ABSORCAO` não repõe a validação lida. O log mostra coluna `12:00:00`, composta `None` e resposta `12:00:01`. É prova do cenário interposto com releitura. A incidência em produção ficou [nao verificado]. `medida4.log:9–16` · `app/chamados/services/disputa_emissao.py:1557–1560` · `app/chamados/models.py:3349–3372,4290–4303,4333–4351` · `app/chamados/services/materializacao.py:1063–1069`
- **(e) Grupo.** `perguntas_absorvidas_por` restringe a hora, colaborador, dia, via e carimbo, acha o grupo `(disputa_id, materializada_em)` por uma **pergunta-hora de escape carimbada** e exclui as que têm batida plantada viva. O consumidor encontrado é a devolução na rejeição da ausência. Deixar de fora uma irmã comum sem carimbo preserva o grupo quando a âncora permanece. Se a recusada for a única âncora de escape, o leitor devolve `[]`, inclusive com outras irmãs carimbadas. O escape do anexo é do tipo `escolha`, fora desse índice de busca. A sonda, portanto, não prova a conservação do grupo. `app/chamados/services/materializacao.py:1033–1036,1123–1139,1166–1169` · `app/ponto/signals.py:254–262` · `test_e4c_f5_item4_medida_relogio_comum.py:29–31`
- **(f) Releitura, sim. Prefetch antigo nesse caminho, não.** As instâncias `q` saem da consulta filtrada antes da trava. `_declarou_hora` lê o texto dessa foto, e o caminho segue direto para `carimbar` depois de travar. Uma leitura nova sob a trava é necessária para obter as colunas atuais. Não há `Prefetch('decisoes')` nessa consulta de irmãs. Fio e tarefa têm prefetch em consultas próprias. A invalidação feita por `DecisaoPergunta.registrar` vale para a instância recebida pelo escritor, não para a outra instância já materializada pela absorção. `app/chamados/services/materializacao.py:1033–1064` · `app/chamados/juizes.py:1785–1803` · `app/chamados/models.py:2841–2848,4333–4339,4376–4387` · `app/chamados/services/fio.py:260–267` · `app/chamados/services/tarefa_do_dia.py:532–538`

### Item 2 — SILÊNCIO

- **(g) Tranca e toque.** A tarefa **já lê** `dias_trancados` em lote e sai cedo com `competencia_trancada`. C6 usa C20 pelo invólucro `dia_da_pergunta_trancado`, com o mesmo corpo em lote. O juiz usa K8/K7 e consulta `LogAuditoria` depois de `Period​oFechado.fechado_em`, por colaborador-dia, exigindo usuário humano. As assinaturas são `cobrar_dia_manual` no colaborador, com `dia=...`, `veredito_celula` na célula, com `via=admin` ou `acoes`, e `desarquivar_chamado` no chamado ligado à célula. O push de **Cobrar resposta** registra uma ocorrência de cobrança, enquanto **Cobrar este dia** grava a assinatura que C20 lê. `app/core/juizes.py:304,342–344` · `app/chamados/juizes.py:979–1009,1067–1098,1242–1332,1412–1415` · `app/chamados/services/tarefa_do_dia.py:227–235,550–555` · `app/chamados/services/cobrar_dia.py:31–45` · `app/chamados/views_cobrar.py:34–57`
- **(h) E3-V10.** Confere para o **fio do colaborador-dia**. O corte manda escolher a primeira ação alcançável na ordem colab > admin e contar explicitamente o dia sem porta. Para vencimento, manda usar C9. O corte tem unidade diferente da instância da pergunta. `app/docs/CORTES.md:490` · `app/core/juizes.py:293` · `app/chamados/services/tarefa_do_dia.py:748–778`
- **(i) Botão e contador.** No modal, **Cobrar resposta** fica no bloco de disputa com permissão contextual, aberta, fora do ramo `ancora_ausente`, quando `todas_respondidas` é falso. O fio calcula esse sinal por `aguarda_o_colaborador`, que reduz as perguntas listadas que esperam texto e estão vivas pelo C6. Na Central, a lista detalhada oferece o mesmo POST na linha com `item.verbo == 'cobrar'` sem `cobrado_hoje`. O POST exige `gerir_chamados` e acesso à área. Hoje o compositor exclui o verbo residual `cobrar` e não inclui essa ação no conjunto administrativo alcançável. Já há rótulo `SEM PORTA` na Folha e contagem por **dia** no vigia. Um contador dessa classe por instância ficou [nao verificado]. `app/templates/chamados/partials/modal_fio.html:123,354–380` · `app/chamados/services/fio.py:164–190,388–395` · `app/templates/chamados/partials/_lista_chamados.html:83–84` · `app/chamados/views_cobrar.py:24–43` · `app/chamados/services/tarefa_do_dia.py:751–766` · `app/folha/fora_da_folha.py:56` · `app/chamados/services/vigia_fio.py:582–595,658–662`
- **(j) Obra registrada.** `PORTA-DO-ADMIN-NA-PERGUNTA-VENCIDA` consta no backlog como registrada, à espera de corte, com base de 2.333 **colaborador-dias** com pergunta devida vencida por C9 e sem ação administrativa alcançável. Somar perguntas ressuscitadas diretamente a esse número mistura unidades e pode incluir dias fora do recorte C9 ou contar o mesmo dia duas vezes. O cruzamento ficou [nao verificado]. `app/docs/BACKLOG.md:771` · `app/docs/CORTES.md:490` · `respostas_2011_1135.txt:7`
- **(k) Compensadora como último ato.** A fonte existente é a sequência integral já ordenada por `(ato_em, pk)` na carga, com a marca reconhecida por `DecisaoPergunta.es_da_ressuscitacao`. Para a tarefa, vale o último ato real até o `agora` da carga. Hoje `_fatos_das_perguntas` descarta a compensadora **antes** de aplicar esse horizonte. O `decisoes[-1]` de `_pela_decisao` já vê a lista filtrada, e `leitura`/`composta` não devolvem o último ato nem a marca. `app/chamados/models.py:19–25,1713–1719,4225,4246–4250` · `app/chamados/services/tarefa_do_dia.py:158–164,387–403,532–545`

**(l) Teste-alvo e controles.** O teste-alvo existe em `app/chamados/tests/test_f1nova_b1_juiz_respondida.py::test_RED_ressuscitar_com_SILENCIO_nao_devolve_ao_colaborador`. Ele compara a razão e os trios `(acao, de, porta)` antes e depois. Não verifica o botão, o acesso do admin ou o fio publicado. Os sete módulos de controle identificáveis pelo v05 e o golden estão abaixo. “Preservável” é juízo estático sobre a expectativa, execução [nao verificado]. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:320–344`

| Arquivo::teste | Expectativa sem alteração | [nao verificado]
|---|---|
| `app/chamados/tests/test_e2_tarefa_do_dia.py::test_ato_duplicado_e_a_mesma_tarefa` e `::test_a_pendencia_historica_e_do_admin_ate_a_decisao` | Preservável. Ambos usam silêncio comum, sem compensadora. `app/chamados/tests/test_e2_tarefa_do_dia.py:277–289,499–508` |
| `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py::test_RED_reabertura_marcada_nao_vira_devolvida_ao_colaborador` e `::test_MORDE_reabertura_comum_segue_devolvida_ao_colaborador` | Preservável. A base é uma resposta, e a reabertura comum continua tendo efeito próprio. `app/chamados/tests/test_f2_p1_tarefa_ignora_reabertura_marcada.py:54–69,78–95` |
| `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py::test_RED_b_mesmo_ressuscitar_mesma_tarefa_e_mesmo_csf` | Preservável **se K6 for aplicado também à pergunta no marco ordinário**. O teste usa uma exportada com `SILENCIO` real e compara tarefa inteira e CSF. A seleção atual de K6 pode pular os marcos ordinários. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:106–120,138–161` · `app/chamados/services/tarefa_do_dia.py:598–617` |
| `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py::test_RED_silencio_decidido_nao_tem_dono_admin` e `::test_dono_admin_True_colaborador_False` | Preservável. Exercitam o silêncio comum e sua projeção sem ação administrativa. `app/api/tests/test_f6_u4_aguarda_admin_pela_tarefa.py:59–65,92–96` |
| `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py::test_sequencias` | Preservável. `rm` é uma resposta que mantém a validação, sem compensadora. O teste conserva o recuo até o silêncio efetivo. `app/chamados/tests/test_e4c_f6f7_c14_lei3_pela_leitura.py:26–53` |
| `app/chamados/tests/test_e3_portas_tarefa.py::test_v8_R15_produto_decisoes_residual_A` | Preservável. Q12 fornece só `SILENCIO` e exige que a rota residual siga `porta_fila`. `app/chamados/tests/test_e3_portas_tarefa.py:721–759` |
| `app/chamados/tests/test_e3_portas_v6_sem_banco.py::test_encerrada_externa_tem_porta_do_chamado_e_passivo_preserva_pergunta` | Preservável se `silencio_decidido` continuar na guarda de encerradas. Remover essa razão muda a rota esperada. O mutante `encerrada_rota_pergunta` cobre a guarda. `app/chamados/tests/test_e3_portas_v6_sem_banco.py:98–106` · `app/chamados/tests/e3_portas_v6_mutantes.py:67–70` |
| `app/chamados/tests/f2_ressuscitar_tarefa_main.json`, consumido pelo teste F2 acima | Preservável sob a mesma condição de K6. P2 tem `silencio_decidido` com ações vazias antes e depois. `app/chamados/tests/f2_ressuscitar_tarefa_main.json:39–46,114–121` · `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:155–161` |

Os outros dois controles do próprio módulo-alvo também são preserváveis: ressuscitação sem decisão e reabertura comum com o mesmo motivo textual. O código sustenta a possibilidade de mudar só a expectativa-alvo. O item escrito e os anexos ainda não demonstram esse resultado. `app/chamados/tests/test_f1nova_b1_juiz_respondida.py:340–362` · `respostas_2011_1135.txt:11`

## P2 — Avais, leis, registro e selos

### Item 1

A saída (a) é compatível com a equivalência do aval das 11h00 no caso medido e com a validação sintética já lida pela sequência. O selo F5 exige a composta nas novas chamadas ao juiz temporal ou ao juiz de resposta, e a catraca nominal de colunas cruas não inclui `_materializar_ausencia_de_resposta`. **`Juízes novos=0` fecha para a comparação temporal da resposta vigente** usando os leitores existentes. O mesmo não está demonstrado para um predicado que exija reconhecer especificamente a **origem humana** do ato. A composta não retorna essa origem, e o registro C21 responde se há instante e texto. Uma regra paralela de sequência ou uma autoridade nova ficaria sob L-002 e L-011, ainda que o teste nominal não a detectasse. `respostas_2011_1100.txt:4` · `app/chamados/models.py:4306–4317,4354–4362` · `app/chamados/juizes.py:1383–1403` · `app/core/juizes.py:305` · `app/chamados/tests/test_e4c_f5_selo_catraca.py:15–25,298–307` · `app/chamados/tests/test_e4c_f5_selo_chamada.py:147–149,188–193` · `app/docs/LEIS.md:60,69` · `CLAUDE.md:314–315`

### Item 2

**`Juízes novos=0` fecha para “trancada sem TXT que o admin não tocou”** pela combinação de C20 e K6/K8, e é compatível com **“ressuscitada em silêncio”** como composição de C19 sobre a sequência ordenada, o horizonte e a marca existente. Para **“cobrar alcançável”**, o registro tem o juiz do dia e o código tem os insumos das telas. Uma resposta compartilhada que cubra a oferta real por chamado, a permissão e a sua atribuição à instância ficou [nao verificado]. A presença de um PK na fila é insuficiente. O selo E3 veda repetir o predicado do app, e o selo da cobrança guarda a rota existente e o ramo do modal. Contar só a partir da tarefa exige a mesma autoridade que a tela usa. `app/core/juizes.py:301–305,342–344` · `app/chamados/services/tarefa_do_dia.py:387–403,734–766` · `app/templates/chamados/partials/modal_fio.html:123,354–380` · `app/chamados/views_cobrar.py:24–43` · `app/chamados/tests/test_e3_degrau_dono_e_porta.py:380–385` · `app/chamados/tests/test_selo_painel_oferece_cobrar.py:173–193` · `app/docs/LEIS.md:60,69,149`

O aval de 11h35 conserva a precedência do dia de E3-V10/L-129 e a compensadora da F2. A colisão é de **cobertura de dono**: o aval de 11h00 manda ADMIN em aberta e o corte da ressuscitação exige destino para a pergunta. O item novo contém dois destinos sem um dono nomeado, detalhados em P3. `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:6–10` · `app/docs/CORTES.md:490,543–545` · `app/docs/LEIS.md:65,149`

## P3 — O literal fecha?

### Item 1

**A parte de dados é alcançável no caso medido** com a trava e a ordem atuais. A resposta humana deixa `validada_em` nulo e grava a linha que desfaz a validação. Sem aplicar o carimbo da absorção nessa irmã, coluna e composta podem ficar nulas, sem ato de reparo e com T1 preservado nas demais absorções. O log prova B no caminho antigo, e o verde desse contrafactual ficou [nao verificado]. O teste anexado ainda exige que `carimbar` seja chamado uma vez e, no fim, só asserta ausência de B. A expectativa atual, sozinha, não prova a recusa, a trilha ou o grupo. `app/chamados/services/disputa_emissao.py:1481–1508,1526–1531` · `app/chamados/services/materializacao.py:1033–1069` · `app/chamados/models.py:4333–4351` · `test_e4c_f5_item4_medida_relogio_comum.py:53–61` · `medida4.log:9–16`

**O conjunto literal não fecha.** Na própria sonda `19:02` concorda com a luz `19:00`. C6 pode dar a pergunta por encerrada pelo fato mesmo sem carimbo, e `PerguntaDisputa.estado` com resposta vigente e sem carimbo devolve `RESPONDIDA`. Assim, **“fora da absorção”** é demonstrável como efeito da recusa, enquanto **“pendente/viva”** como estado não decorre dela. A trilha reaproveitada também é best-effort, fora do `atomic` da irmã, e a descrição atual atribui a recusa a C6. Por fim, deixar a irmã intocada não garante coluna vazia se houver uma validação legítima posterior já gravada sob a mesma ordem de trava. `test_e4c_f5_item4_medida_relogio_comum.py:24–33,42–44` · `app/chamados/juizes.py:1335–1380,1426–1440` · `app/chamados/models.py:3249–3268` · `app/chamados/services/materializacao.py:1060–1076` · `app/core/observ.py:94–105,133–139` · `app/chamados/services/validacao.py:163–174,752`

### Item 2

1. **Trancada sem TXT.** K6 falso e C20 verdadeiro podem coexistir. Pela definição do aval de 11h00, esse caso é “em aberta” e iria a ADMIN. Pela regra de 11h35, a instância fica arquivada, sem pergunta e sem ação no resultado da tarefa. O toque devolve o dia ao julgamento, mas **Cobrar resposta** não é o toque de C20. A saída antecipada também não impede, por si, que a composição publique uma porta residual de outro chamado do dia. Logo, arquivar a instância não dá destino à pergunta, e eventual dono do dia não supre essa lacuna. Quantidade e casos nominais [nao verificado]. `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:6` · `app/core/juizes.py:342–344` · `app/chamados/services/tarefa_do_dia.py:227–235,678–691,734–767` · `app/chamados/juizes.py:1088–1098` · `app/chamados/views_cobrar.py:44–57`
2. **“Sem porta” por instância.** O contrato geral da tarefa permite `acoes=[]` e fio `nada`. A obrigação específica do corte é que a **pergunta ressuscitada** tenha destino. Uma ressuscitada em aberta, sem botão alcançável, termina no literal de 11h35 com rótulo e contador, sem ADMIN nessa instância e sem DP, reservado ao caso entregue. A precedência colab > admin continua válida no fio do dia, mas não resolve essa falta de dono na pergunta. `app/chamados/services/tarefa_do_dia.py:42–54,766–774` · `app/docs/CORTES.md:490,545` · `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:7–10`

## P4 — Exportada, dinheiro, `folha/`, escala/vínculo e `!`

### Item 1

A decisão de não carimbar a irmã e registrar a recusa fica em Chamados. O caminho real, porém, já cria ou reutiliza uma ausência **antes** da absorção e pode reconciliar o destino depois da recusa. A criação pode acionar o cartório e, se o insumo da Folha mudar, invalidar `FechamentoMensal.derivado_em`. O `UPDATE` da invalidação não filtra por K6. Isso comprova alcance indireto sobre o gravado no caminho existente, não um delta imputável à saída (a). Alteração efetiva em exportada, valores, escala/vínculo ou necessidade de `!` para esse delta ficaram [nao verificado]. Escrita no gravado exportado ou em dado de escala/vínculo tem a cerca expressa no `CLAUDE.md`. `app/chamados/services/materializacao.py:1024–1033,1070–1089` · `app/ponto/signals.py:143–147` · `app/ponto/services/ausencia.py:154–164,195,594–599` · `app/ponto/services/fechamento.py:1648–1656` · `CLAUDE.md:265–268,404–415`

### Item 2

A publicação de tarefa e o destino textual do DP são de leitura no aval. K6 cobre a entrada da exportada, mas a carga atual consulta K6 para um recorte que pode deixar de fora o marco ordinário. A Folha já consome o fio publicado, e o retrato não tem tradução própria para `cobrar`, podendo cair no link legado. Há, portanto, alcance sobre **a leitura** da Folha, que o aval de 11h00 manda registrar. Edição de `folha/`, execução de toque administrativo, escrita em exportada, dinheiro ou dado de escala/vínculo fora do declarado ficaram [nao verificado]. `respostas_2011_1100.txt:5–6` · `respostas_2011_1135.txt:6–10` · `app/docs/CORTES.md:545` · `app/chamados/services/tarefa_do_dia.py:563–578,598–619` · `app/folha/fora_da_folha.py:71–73,275–280` · `app/folha/retrato_folha.py:1015–1021,1044–1048,1066–1071` · `CLAUDE.md:265–268`

**Item 1 — PROCEDE (AVAL-DEVOLVIDO).** A recusa evita o carimbo que produz B na sonda, mas não entrega o estado pendente exigido, e a trilha reaproveitada é best-effort. `medida4.log:16` · `app/chamados/models.py:3259–3268` · `app/core/observ.py:94–105,133–139`

**Item 2 — PROCEDE (AVAL-DEVOLVIDO).** Tranca sem TXT e ausência de botão deixam classes de ressuscitadas em aberta sem o dono exigido pelo corte da pergunta. E3-V10 autoriza contar o **dia** sem porta. `respostas_2011_1100.txt:5` · `respostas_2011_1135.txt:6–8` · `app/docs/CORTES.md:490,545`

## 20/11 11:24 · sessao 0014d54c272b · CODEX-FECHA-A-VOLTA-V1-RASCUNHO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 20/11 11:20 · sessao fd2a82382307 · E4C-F5-CONSTROI · construtor · origem=Code

## Achados

1. **`PerguntaDisputa.carimbar` pode produzir B** — coluna preenchida, composta sem validação. Se `quando` antecede uma `RESPOSTA` que desfaz validação, a leitura aplica a validação sintética e depois a apaga. A absorção abre essa janela ao capturar `agora` e carregar as perguntas antes da trava, passando o instante anterior para `carimbar`. O C6 pode aceitar o carimbo pela concordância com o marco. [chamados/models.py:3344](chamados/models.py#L3344) · `chamados/models.py:3355`, `:4339–4348` · `chamados/services/materializacao.py:1033–1036`, `:1060–1064` · `chamados/juizes.py:1428–1440`
2. **`reabrir_para_correcao` pode preservar A preexistente** — composta validada, coluna vazia. Se só existe a `VALIDACAO` física e a projeção já está sem texto, resposta, validação e materialização, o filtro exclui a pergunta da compensação. O lote grava a coluna nula e a validação física segue vigente. [chamados/services/disputa_emissao.py:1636](chamados/services/disputa_emissao.py#L1636) · `chamados/services/disputa_emissao.py:1650–1653` · `chamados/models.py:4342–4348`

Os dois caminhos têm REDs dedicados e ficaram sem cura de produção nesta volta. [chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:512](</home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:512>) · `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:532`

## Censo

HEAD lido por `git rev-parse HEAD`: `8f84213791cde624afb4c4d1c2d496c730fae0df`. Os caminhos abaixo são relativos a `app/`. A varredura encontrou nove sítios de atribuição ou envio ao lote e cinco `save` associados. `composta` usa a validação de `leitura`, que ordena atos por instante, dá às linhas sintéticas a última posição no empate e aplica os atos que desfazem a validação. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:31`, `:452` · `chamados/models.py:4333–4351`, `:4354–4362`

| Escrita | Função | Efeito | Evidência na mesma função ou transação |
|---|---|---|---|
| `chamados/services/validacao.py:168`, save `:171` | `_carimbar_validacao` | PREENCHE | `registrar` grava `VALIDACAO` com o instante da coluna no mesmo `atomic`. `chamados/services/validacao.py:170–175` |
| `chamados/services/validacao.py:287`, save `:290` | `devolver_validacao_recusada` | LIMPA | A guarda entra se coluna ou composta estiver validada. O `atomic` salva a coluna nula e registra `DEVOLUCAO`, que apaga a validação vigente. Cobre A e B separadamente. `chamados/services/validacao.py:277–296` · `chamados/models.py:4302` |
| `chamados/services/validacao.py:365`, save `:367` | `devolver_paridade_bug_par_103` | LIMPA | Sob a trava, salva a coluna nula e registra `DEVOLUCAO` com a foto anterior no mesmo `atomic`. `chamados/services/validacao.py:347–348`, `:361–374` |
| `chamados/services/disputa_emissao.py:1506`, save `:1526` | `responder_pergunta_disputa_core` | LIMPA | Depois da trava, a `RESPOSTA` recebe o instante da resposta e `resposta_mantem_validacao=False`. O registro e o `save` ficam no mesmo `atomic`. `chamados/services/disputa_emissao.py:1478–1493`, `:1526–1531` · `chamados/models.py:4317` |
| `chamados/services/disputa_emissao.py:1614`, lote `:1653` | `reabrir_para_correcao` | LIMPA | Registra `REABERTURA` ou `RECUSA` antes do lote, no mesmo `atomic`, para as linhas que passam no filtro da foto. A isolado fica de fora conforme o achado 2. `chamados/services/disputa_emissao.py:1624–1626`, `:1636–1653` · `chamados/models.py:4189`, `:4302` |
| `chamados/models.py:1582` | `ChamadoColaborador._universo_da_ressuscitacao` | LIMPA em cópia | A sonda limpa a cópia e troca seu cache pela sequência posterior ao instante simulado da reabertura. O C6 recebe essa cópia. `chamados/models.py:1558`, `:1569`, `:1581–1595` |
| `chamados/models.py:1721` | `ChamadoColaborador.ressuscitar_perguntas` | LIMPA | `registrar` preserva a foto e grava a `REABERTURA` compensadora no universo relido sob trava. `descarimbar_em_lote` encaminha os campos ao `update` no mesmo `atomic`. `chamados/models.py:1646–1651`, `:1669–1674`, `:1702–1722`, `:3508–3511` |
| `chamados/models.py:3355`, save `:3372` | `PerguntaDisputa.carimbar` | PREENCHE | A validação sintética vem da coluna quando falta linha física correspondente. A equivalência fecha quando o carimbo prevalece na sequência. Um ato posterior pode produzir B conforme o achado 1. `chamados/models.py:3354–3377`, `:4290–4294`, `:4333–4348` |
| `chamados/models.py:3384` | `PerguntaDisputa._desfazer_em_memoria` | RESTAURA em memória | Repõe o valor que `carimbar` capturou antes da tentativa. Os caminhos de recusa ou exceção chamam essa restauração. `chamados/models.py:3346–3347`, `:3358–3365`, `:3373–3386` |

## Arquivos e verificação

- [test_e4c_f5_selo_par_ato_coluna.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452) — `SimpleTestCase` com descoberta por AST, resolução de aliases e regras por classe. O censo só alimenta a anti-vacuidade. O selo segue um nível para um auxiliar local que chame `registrar`, confere a foto de `carimbar` usada por `_desfazer_em_memoria` e confere o repasse do lote ao `update`. Os escritores persistentes do censo chamam `registrar` diretamente quando gravam o ato. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:31`, `:260`, `:318`, `:452`, `:499`
- [e4c_f5_voltaE_mutantes.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/e4c_f5_voltaE_mutantes.py:6) — três patches de fonte em memória, cada um com `assert fonte.count(antes) == 1`. A checagem estática confirmou um diagnóstico por mutante no teste `test_escritores_fecham_o_par`. `limpa_sem_ato` acusa `ressuscitar_perguntas`; `preenche_e_desfaz` acusa `_carimbar_validacao`; `escritor_novo` acusa `_limpar_por_fora`. `chamados/tests/e4c_f5_voltaE_mutantes.py:6–38` · `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452–462`

## Comandos para o Code

O primeiro comando é o **GREEN esperado da varredura AST na base**. O segundo executa o módulo inteiro, com os dois REDs dos achados esperados no HEAD. Os três últimos isolam os mutantes no teste AST. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:452`, `:512`, `:532` · `chamados/tests/e4c_f5_voltaE_mutantes.py:5`

```bash
selo_e=chamados.tests.test_e4c_f5_selo_par_ato_coluna
teste_ast="$selo_e.SeloParAtoColunaTest.test_escritores_fecham_o_par"

env -u F5_VOLTA_E_MUTANTE PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
env -u F5_VOLTA_E_MUTANTE PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$selo_e" --settings=config.settings.ci

F5_VOLTA_E_MUTANTE=limpa_sem_ato PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
F5_VOLTA_E_MUTANTE=preenche_e_desfaz PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
F5_VOLTA_E_MUTANTE=escritor_novo PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$teste_ast" --settings=config.settings.ci
```

**[nao verificado]** Execução pelo runner Django, suíte, banco e ocorrência dos dois caminhos em produção. A checagem local executou somente a varredura AST, os patches em memória e trechos isolados do leitor e do filtro de reabertura. `chamados/models.py:4320–4362` · `chamados/services/disputa_emissao.py:1636–1651`

## 20/11 11:17 · sessao e099536c8d72 · CODEX-FECHA-A-VOLTA-V1-RASCUNHO · leitura · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 20/11 10:59 · sessao f9ea3a01eb41 · E4C-F5-CONSTROI · construtor · origem=Code

Implementei a volta **E-CONTADOR** na base `48be81433dab4bb4ee9641e2daeedd22acf1cbff`. O contador lê `DecisaoPergunta.composta(p).validada_em` e `p.validada_em` uma vez por pergunta, com `decisoes` carregadas em lotes de 2.000 por padrão. A e B têm esperado `0` e dono `ESTRUTURA`. [vigia_ato_x_coluna.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/vigia_ato_x_coluna.py:8)

O `vigiar_fio_morto` passa o mesmo queryset do contador de texto dentro de `tranca_em_lote()`, imprime `VFM|ATO_X_COLUNA|A=%d B=%d (esperado 0 e 0)|universo=%d` e publica `ato_x_coluna_a` e `ato_x_coluna_b` no `censo` e no `esperado` do carimbo. As duas chaves ficam fora de `ALARMA_SEMPRE` e `ALARMA_EM_0`. O encaixe usa o comando e o cron existentes. `app/chamados/management/commands/vigiar_fio_morto.py:79`, `:104`, `:188`, `:233`, `:274`, `:283`, `app/config/crons.py:1737`.

### Leitura e escritores

O passivo com coluna preenchida e zero decisões cai em **`(True, True)`**. `leitura(p)` acrescenta a validação sintética no instante da coluna e `composta(p)` devolve essa validação. A pergunta soma um ao universo e zero a A e B. `app/chamados/models.py:4290`, `:4333`, `:4345`, `:4360`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:63`.

| Campo ou caminho | Escrita na cópia |
|---|---|
| `PerguntaDisputa.validada_em` na validação e nas devoluções | `_carimbar_validacao` grava a coluna e `VALIDACAO` na mesma transação. `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` limpam a coluna e registram `DEVOLUCAO` na mesma transação. `app/chamados/services/validacao.py:168`, `:287`, `:347`, `:365`. |
| `PerguntaDisputa.validada_em` na resposta, reabertura, recusa e ressuscitação | A resposta humana limpa a coluna e registra `RESPOSTA` com `resposta_mantem_validacao=False` no mesmo `atomic`. Reabertura e recusa limpam pelo lote e registram o ato no mesmo `atomic`. A ressuscitação registra `REABERTURA` e limpa pelo lote no mesmo `atomic`. `app/chamados/services/disputa_emissao.py:1478`, `:1506`, `:1529`, `:1626`, `:1636`, `:1653`, `app/chamados/services/validacao.py:943`, `app/chamados/models.py:1646`, `:1719`, `:3509`. |
| `PerguntaDisputa.validada_em` no carimbo com `resposta='validar'` | `carimbar` persiste a coluna. A validação sintética vem da leitura, conforme o aval das 11:00. A sonda da ressuscitação e `_desfazer_em_memoria` atribuem o campo em memória. `app/chamados/models.py:1582`, `:3354`, `:3372`, `:3384`, `:4290`. |
| `DecisaoPergunta.composta(p).validada_em` | `composta` produz o valor em memória a partir de `leitura`. `DecisaoPergunta.registrar` persiste as linhas de ato que alimentam a leitura. `app/chamados/models.py:4333`, `:4345`, `:4360`, `:4365`. |

### Testes e leitores [nao verificado]

- O [teste novo](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:19) cobre porta real, A, B, passivo, recorte inclusive vazio, carimbo com `--alarme` e custo. O teste do comando captura o payload de `_carimbar` em memória, com A=1 e B=2 para detectar troca de chaves. O teste O261 confere o JSON em disco. `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:47`, `:78`, `:99`, `app/chamados/tests/test_o261_vigia_fio.py:826`.
- Os dois inventários de dublês ganharam a chamada nova. `test_e4a2_c5_memoria` executa o `handle` extraído por AST e `test_fio_mudo_placar` substitui as derivações do vigia. O contrato O261 ganhou as duas chaves nas comparações integrais de `censo` e `esperado`, com os valores anteriores preservados. `app/chamados/tests/test_e4a2_c5_memoria.py:132`, `app/core/tests/test_fio_mudo_placar.py:128`, `app/chamados/tests/test_o261_vigia_fio.py:816`, `:837`, `:851`, `:881`.
- O selo de chamadas F5 varre fontes por chamadas a dois juízes. O contrato do dono de `validada_em` varre escritas. O contrato do relógio do vigia seleciona `censo` e `censo_alcance`. A inclusão dispensa alterações nesses contratos. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126`, `:152`, `app/chamados/tests/test_contract_dono_validada.py:47`, `app/chamados/tests/test_e3_degrau_dono_e_porta.py:389`.
- Fora de `app/`, `bin/fio_estado.py` lê o JSON, percorre todas as chaves de `esperado` e publica as divergências. `bin/gerar_estado.py` usa esse leitor. Ambos já aceitam A e B sem edição. O teste de `bin/` verifica o caminho e campos específicos do alcance. Dentro de `app/`, o `pipeline_placar` lê as chaves específicas `fio_mudo_alvos` e `nunca_bateu_sem_adesao`. `bin/fio_estado.py:98`, `:121`, `:191`, `bin/gerar_estado.py:260`, `bin/tests/test_fio_estado.sh:117`, `:203`, `app/core/management/commands/pipeline_placar.py:85`, `:135`.

### Comandos para o Code

**GREEN e vizinhos**, a partir de `app/`. Os casos novos estão em `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:47`.

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4c_f5_contador_ato_x_coluna

PYTHONDONTWRITEBYTECODE=1 python manage.py test --settings=config.settings.ci \
  chamados.tests.test_e4c_f5_contador_texto \
  chamados.tests.test_f7_vigia_validada_sem_linha \
  chamados.tests.test_o261_vigia_fio \
  chamados.tests.test_e4a2_c5_memoria \
  core.tests.test_fio_mudo_placar \
  chamados.tests.test_e4c_f5_selo_chamada \
  chamados.tests.test_contract_dono_validada
```

**RED por mutante.** A primeira troca faz A sair `0` no caso `(True, False)`. A segunda faz B sair `0` no caso `(False, True)`. Cada comando exige uma âncora de linha única e aplica a troca em memória. A falha esperada é na asserção de contagem. `app/chamados/services/vigia_ato_x_coluna.py:17`, `:19`, `app/chamados/tests/test_e4c_f5_contador_ato_x_coluna.py:55`, `:59`.

```bash
mutar_contador() {
  PYTHONDONTWRITEBYTECODE=1 DJANGO_SETTINGS_MODULE=config.settings.ci \
    python - "$@" <<'PY'
import sys
from pathlib import Path

import django
from django.conf import settings
from django.test.utils import get_runner

django.setup()
from chamados.services import vigia_ato_x_coluna

antes, depois, teste = sys.argv[1:] [nao verificado]
fonte = Path(vigia_ato_x_coluna.__file__)
texto = fonte.read_text(encoding='utf-8')
linhas = [linha for linha in texto.splitlines(keepends=True) if linha.strip() == antes]
if len(linhas) != 1:
    raise SystemExit(f'Âncora inválida: {antes!r}, ocorrências: {len(linhas)}')
(linha,) = linhas
mutado = texto.replace(linha, linha.replace(antes, depois, 1), 1)
escopo = vars(vigia_ato_x_coluna).copy()
exec(compile(mutado, str(fonte), 'exec'), escopo)
vigia_ato_x_coluna.contar.__code__ = escopo['contar'].__code__
prefixo = 'chamados.tests.test_e4c_f5_contador_ato_x_coluna.ContadorAtoXColunaTest.'
sys.exit(get_runner(settings)(interactive=False).run_tests([prefixo + teste])) [nao verificado]
PY
}

mutar_contador 'if celula == (True, False):' 'if celula == (False, True):' \
  test_RED_a_ato_validado_coluna_vazia

mutar_contador 'elif celula == (False, True):' 'elif celula == (True, False):' \
  test_RED_b_coluna_preenchida_ato_devolvido
```

**Mutante `retry_reativa` do aval**, pelo executor existente. `app/chamados/tests/bug_par_mutantes.py:68`, `:189`, `app/chamados/services/validacao.py:702`.

```bash
PYTHONDONTWRITEBYTECODE=1 python -m chamados.tests.bug_par_mutantes \
  --django --mutante retry_reativa
```

Conferi a sintaxe dos seis arquivos e a unicidade das duas âncoras sem carregar Django. Banco, suíte, execução dos mutantes, custo observado e nova medição de frota **[nao verificado]**. O `A=0`, `B=0` em 51.953 perguntas é o dado do anexo fornecido. A remedição e a certificação ficam com Code e Opus. [nao verificado]

_7514 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
