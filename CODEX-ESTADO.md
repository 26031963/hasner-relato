# CODEX-ESTADO

> Ultima resposta processada: **20/11/2026 14:26** · publicado em **20/11/2026 14:28** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 20/11 14:26 · sessao eb93f9221b49 · E4C-F5-CONSTROI · construtor · origem=Code

A cura ficou na cópia `23e80acde0372b2c61e243f311e7d9d96c49e35d`. Conferi a sintaxe dos nove arquivos, as âncoras dos mutantes e o literal do S14. A execução dos REDs, dos mutantes e da suite fica com a sessão [nao verificado]. O diff está sem commit, conforme a proibição expressa da obra [nao verificado por arquivo:linha].

## Cura e decisões

- `_materializar_ausencia_de_resposta` registra `q.recusa_do_carimbo`, `fora_da_releitura` ou `ja_carimbada` na ação `irma_viva_na_ausencia`. Mantive a comparação após `True` usando `DecisaoPergunta.par_ato_coluna(q)`. O RED do par `(True, False)` cobre a irmã com validação no ato e sem texto, caso em que a porta mantém a coluna vazia. `chamados/services/materializacao.py:1059–1099`, `chamados/models.py:3366–3393`, `chamados/tests/test_e4c_f5_volta3_ausencia.py:192`.
- A passagem termina as tentativas das irmãs antes de chamar o S14. Se qualquer irmã do destino entrou em `recusadas`, o destino fica fora da chamada. O literal cadastrado e o único descarte autorizado foram preservados. Há REDs para luz sem batida e para um destino compartilhado entre uma irmã aceita e uma recusada. `chamados/services/materializacao.py:1100–1108`, `chamados/tests/e4b_c12_sitios.json:107–112`, `chamados/tests/test_selo_reconciliar_le_o_retorno.py:128–135`, `chamados/tests/test_e4c_f5_volta3_ausencia.py:100,126`.
- `_materializar_uma` usa a mensagem própria da recusa do par. `_balde_da_recusa` usa `recusa:PAR_ATO_COLUNA` e preserva a precedência da via e a tradução dos demais erros. `chamados/services/materializacao.py:2010–2037`, `chamados/services/passivo_validado.py:131–148`.
- B2 exercita uma nova resposta `FALTA_DIA` na âncora original e exige a absorção do mesmo PK dependente. B3 cobre a irmã assinada por ato real e ainda sem carimbo. B4 exige `ausencia_aprovada` no marco apagado e o PK da dependente na trilha. No ramo `ausencia_aprovada`, a tarefa publica `pergunta_id=None`; se B4 exigir o PK **nesse campo da tarefa**, a exigência segue aberta no leitor fora da escrita autorizada. `chamados/tests/test_e4c_f5_volta3_ausencia.py:46–98,175–190`, `chamados/services/tarefa_do_dia.py:290–324`.

## Sítio, origem e caminho posterior

Os nomes abaixo identificam os testes. O avanço descrito é o que cada teste executa e exige. Resultados em Django [nao verificado]. [nao verificado]

| Sítio da base | Origem | RED e caminho no mesmo PK |
|---|---|---|
| `reconciliador.py:408` | Instância velha, banco limpo | `test_RED_c1_prefetch_le_reabertura_apos_select_e_preserva_banco_limpo`. Batida real, com reabertura entre SELECT e prefetch. `SEM_PORTA` abaixo. `chamados/tests/test_e4c_f5_guarda_reconciliador.py:108–135` |
| `reconciliador.py:408` | Par persistido | `test_RED_c1_par_persistido_recusa_e_questionario_reabre_o_mesmo_pk`. Batida real, seguida de reabertura do questionário pelo titular. `chamados/tests/test_e4c_f5_guarda_reconciliador.py:137–157` |
| `disputa_emissao.py:2043` | Par persistido | `test_RED_destino_recusa_b_persistido_e_a_mesa_recusa_o_mesmo_pk`. Save do chamado, seguido de POST de recusa pelo destino. `chamados/tests/test_e4c_f5_guarda_par_s143.py:90–117` |
| `disputa_emissao.py:2117` | Par persistido | `test_RED_sem_credor_recusa_b_persistido_e_titular_reabre_o_mesmo_pk`. Save de outro chamado, seguido de reabertura do questionário pelo titular. `chamados/tests/test_e4c_f5_guarda_par_s143.py:119–136` |
| `materializacao.py:1067` | Banco limpo por `DEVOLUCAO` T2, tentativa com relógio T1 | `test_RED_devolucao_t2_desfaz_carimbo_aceito_pelo_c6`. Resposta de escape, seguida de recusa da resposta da irmã pela porta existente. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:429–476` |
| `materializacao.py:1067` | Par persistido | `test_RED_absorcao_recusa_coluna_persistida_e_a_mesa_recusa_mesmo_pk`. Resposta de escape, seguida de recusa da irmã pela porta existente. `chamados/tests/test_e4c_f5_volta3_ausencia.py:204–248` |
| `materializacao.py:2004` | Instância velha, banco limpo | `test_RED_passivo_foto_velha_recusa_par_e_mesa_valida_mesmo_pk`. Passivo com devolução entre classificação e trava, seguido de POST da mesa. `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:79–147` |
| `materializacao.py:2004` | Par persistido | `test_RED_passivo_b_persistido_recusa_par_e_mesa_valida_mesmo_pk`. Passivo, seguido de POST da mesa que valida e planta pelo mesmo PK. `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:125–153` |
| `materializacao.py:2204` | Par persistido | `test_RED_s144_b_persistido_fecha_disputa_sem_salvar_carimbo`. `Disputa.fechar()` aciona S144. `SEM_PORTA` abaixo. `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:155–181` |

`SEM_PORTA reconciliador.py:408 (a):` pergunta de texto, muda, sem validação, carimbo ou destino. C6 morto pela célula `concorde`. Disputa aberta, uma reabertura e nenhum chamado pai ou destino. Nova resposta exige C6 vivo. Mesa e recusa exigem chamado listante e resposta completa. Reabrir exige resposta nas colunas. A correção direta exige chamado e tipo hora. Devolução exige, conforme a porta, declaração de hora e recusa escrita, carimbo de paridade ou inclusão no grupo carimbado de absorção. `chamados/tests/test_e4c_f5_guarda_reconciliador.py:94–106,130–135`, `chamados/views.py:483–536,1238–1246`, `chamados/models.py:2414–2417`, `chamados/services/acoes_disputa.py:349–355`, `chamados/services/validacao.py:191–200,269–274,348–373,698–704,915–923`, `chamados/services/materializacao.py:1149–1159`.

`SEM_PORTA materializacao.py:2204 (b):` pergunta `PENDENTE`, muda, C6 morto, coluna antiga e ato devolvido, sem carimbo ou lavra nova. Disputa fechada sem chamado pai ou destino. Nova resposta exige C6 vivo. Mesa exige resposta e chamado. Recusa e correção exigem resposta elegível e disputa aberta. Reabrir administrativamente exige chamado. As devoluções exigem declaração de hora e motivo escrito, carimbo anterior de paridade ou participação no grupo carimbado de absorção. `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:155–181`, `chamados/views.py:1238–1246`, `chamados/services/fio.py:634–688`, `chamados/services/acoes_disputa.py:239–269`, `chamados/services/validacao.py:191–200,269–274,348–373,698–704,915–923`, `chamados/services/materializacao.py:1149–1159`.

- `INALCANCAVEL reconciliador.py:646 (a)/(b)` e `INALCANCAVEL models.py:3532 (a)/(b)` no anexo, hoje `models.py:3560`. Ambos os seletores entregam coluna nula e os gatilhos passam `resposta=None`. Os testes `test_A2_evento_*` acionam o consumidor e `test_A2_AST_*` verificam os filtros. `chamados/reconciliador.py:646,678–681,1096–1103`, `chamados/models.py:3552–3560`, `chamados/tests/test_e4c_f5_guarda_reconciliador.py:159–222`.
- `INALCANCAVEL disputa_emissao.py:2043 (a)` e `:2117 (a)` para o fio de mesmo dono. A trava de C antecede as duas cargas e é compartilhada com os escritores de limpeza. O laço direto admite por SQL uma FK cruzada preexistente, barrada pela costura atual. Existência desse passivo [nao verificado]. `chamados/services/disputa_emissao.py:1924–1932,1963–1998,2083–2097,1478–1532,1627–1669`, `chamados/services/validacao.py:746–800,938–944`, `chamados/models.py:3047–3050`.
- `INALCANCAVEL materializacao.py:1067 (a)` para **instância ou prefetch velho**. A releitura ocorre sob C. O RED T2 cobre a variante que chega de fato com banco limpo, pela validação sintética no relógio T1. `chamados/services/materializacao.py:1036–1077`.
- `INALCANCAVEL materializacao.py:2204 (a)` pelos fluxos de fechamento e repasse lidos. O receiver consulta a pergunta após a trava, sem prefetch de decisões. O controle `test_s144_rele_pergunta_limpa_entre_prefetch_do_fecho_e_trava` interpõe uma devolução real antes da trava. No laço normal do plantio, B fresco é pulado e a validação individual grava `VALIDACAO` sob a trava. `chamados/services/materializacao.py:2150–2158,2200–2213`, `chamados/models.py:2090–2097,2118–2122`, `chamados/services/disputa_emissao.py:1017–1026,1073,1098`, `chamados/services/validacao.py:161–175,752–800`, `chamados/tests/test_e4c_f5_guarda_plantio_s144.py:183–208`.

**A3.** O escritor identificado entre o prefetch e o primeiro carimbo é `ressuscitar_perguntas`, alcançado pela reabertura do credor e pelo cartório síncrono. A seleção e a releitura desse escritor exigem materialização já existente, enquanto a carga do primeiro laço exige materialização nula. A segunda carga vem depois das reaberturas. Nenhuma releitura adicional foi aplicada ao S143. Outra escrita interveniente de limpeza do mesmo PK [nao verificado]. `chamados/services/disputa_emissao.py:1963–1967,1996–2009,2060–2097`, `chamados/models.py:1507–1517,1549–1555,1669–1671,1713–1722`, `chamados/signals.py:189–218`, `ponto/services/cartorio.py:1690–1721`.

## Conflitos para a sessão

O teste indicado no brief mantém a asserção `[destino_recusada.pk, destino_aceita.pk]`. A cura B1 exclui `destino_recusada`. Aplicar B1 ao conjunto já retornado como `recusadas` também atinge os controles de irmã rechaveada, irmã já carimbada e ausência simulada como `None`, que exigem `s14.assert_called_once()`. O controle de rechaveamento ainda exige motivo nulo, em conflito separado com A4. As quatro expectativas permanecem no arquivo. `chamados/services/materializacao.py:1059–1067,1091–1108`, `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:355–356,385–390,410–412,418–427`.

A asserção do selo direto A5 permanece na forma original. S1, H4, `respondida_em`, a recuperação por `perguntas_absorvidas_por` e a linha documental E4D ficaram fora deste diff. H4 tem revisão prévia no aval 1400 item 2 e a linha E4D cabe no commit documental do marco, conforme os anexos. `chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:515–532`, `chamados/services/disputa_emissao.py:1478–1532,1556–1561`, `chamados/services/materializacao.py:1136–1165`. Incidência em produção, medições em duas conexões e certificação [nao verificado].

## Recorte e mutantes para a sessão

Os comandos recortam os REDs novos, o teste A4 alterado, os selos S14 e A5, K47 e a Volta D. O módulo inteiro de A1(c) contém as expectativas em conflito listadas acima. `chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:340–476`.

```sh
t=chamados.tests
a=$t.test_e4c_f5_volta3_ausencia.AusenciaVolta3Test
s=$t.test_e4c_f5_volta3_selos.SelosVolta3Test
r=$t.test_e4c_f5_guarda_reconciliador.GuardaReconciliadorTest
p=$t.test_e4c_f5_guarda_plantio_s144.GuardaPlantioS144Test
f=$t.test_e4c_f5_guarda_par_s143.GuardaParS143Test
d=$t.test_e4c_f5_voltaD_portas.PortasVoltaDTest
k=$t.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k47_escolhas_sem_credor
a4=$t.test_e4c_f5_a1c_passagem_da_ausencia.PassagemDaAusenciaTest.test_RED_devolucao_t2_desfaz_carimbo_aceito_pelo_c6
a5=$t.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest.test_carimbo_sintetico_preserva_equivalencia_com_resposta_mais_recente

PYTHONDONTWRITEBYTECODE=1 python manage.py test \
  "$a" "$s" "$p" "$f" "$a4" "$a5" "$k" "$d" \
  "$t.test_e4c_f5_guarda_reconciliador" \
  "$t.test_e4b_c12_selo" "$t.test_selo_reconciliar_le_o_retorno"
```

Cada comando abaixo indica o teste que deve matar o mutante. `tirar_guarda` exige `False` e o motivo próprio nos nove REDs de chamadores, além da equivalência no selo direto. O aplicador troca código em memória. Execução das mutações [nao verificado]. `chamados/tests/e4c_f5_volta3_mutantes.py:15–119`, `chamados/tests/test_e4c_f5_volta3_selos.py:33–70`, `chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222`, `chamados/tests/test_e4c_f5_voltaD_portas.py:45–67,312–323,388–395`.

```sh
m() { PYTHONDONTWRITEBYTECODE=1 python -m chamados.tests.e4c_f5_volta3_mutantes "$@"; }

m s14_fecha_recusada "$a.test_RED_luz_sem_batida_preserva_resposta_t2_e_destino_aberto" \
  "$a.test_RED_destino_comum_nao_fecha_quando_a_segunda_irma_recusa"
m m20_s14_perde_literal "$s.test_RED_s14_conserva_literal_do_cadastro_c12"
m a1c_sem_releitura "$a.test_RED_luz_sem_batida_preserva_resposta_t2_e_destino_aberto"
m a1c_ignora_ancora "$a.test_RED_ancora_decidida_reabre_o_caminho_da_dependente"
m absorcao_sem_comparador "$a.test_RED_coluna_nula_e_ato_validado_sem_texto_desfaz_a_absorcao"
m absorcao_motivo_c6 "$a4"
m absorcao_fora_da_releitura_motivo_nulo "$a.test_trilha_identifica_a_irma_que_saiu_da_releitura"
m absorcao_ja_carimbada_motivo_nulo "$a.test_trilha_identifica_a_irma_ja_carimbada"

plantio=$p.test_RED_passivo_foto_velha_recusa_par_e_mesa_valida_mesmo_pk
m plantio_motivo_c6 "$plantio"
m passivo_balde_generico "$plantio"
m a2_celula_sem_filtro_de_coluna "$r.test_A2_evento_celula_absolveu_entrega_coluna_nula_e_resposta_none"
m a2_orfa_sem_filtro_de_coluna "$r.test_A2_evento_orfa_entrega_coluna_nula_e_resposta_none"

m v_ato_sem_coluna "$k" "$d.test_RED_recusar" "$s.test_RED_validacao_iniciada_pelo_ato_tambem_grava_a_coluna"
m r_resposta_mantem_ato "$d.test_RED_recusar"
m d_devolucao_mantem_coluna "$d.test_devolucao_e_o_escritor_da_coluna"
F5_VOLTA_B_MUTANTE=k47 PYTHONDONTWRITEBYTECODE=1 python manage.py test "$k"

m tirar_guarda "$a5" "$a4" "$f" "$plantio" \
  "$p.test_RED_passivo_b_persistido_recusa_par_e_mesa_valida_mesmo_pk" \
  "$p.test_RED_s144_b_persistido_fecha_disputa_sem_salvar_carimbo" \
  "$a.test_RED_absorcao_recusa_coluna_persistida_e_a_mesa_recusa_mesmo_pk" \
  "$r.test_RED_c1_prefetch_le_reabertura_apos_select_e_preserva_banco_limpo" \
  "$r.test_RED_c1_par_persistido_recusa_e_questionario_reabre_o_mesmo_pk"
```

LEI-AKITA: origem=chamados/models.py::PerguntaDisputa.carimbar, testemunha=DecisaoPergunta.composta x coluna, RED=o teste direto do selo + um por chamador, quem-mais-le=S143, plantio, absorcao e as filas por coluna, juizes novos=0, padrao=Single Writer Principle [nao verificado]

## 20/11 14:20 · sessao 4ce9057c680a · - · - · origem=Ronald (tmux)

Implementei o balde `recusa:PAR_ATO_COLUNA` no passivo, preservando a precedência da via. [`passivo_validado.py:131`]( /home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/services/passivo_validado.py:131)

Os dois REDs do passivo cobrem instância antiga com banco limpo e divergência persistida. Ambos verificam a recusa, a reversão da batida e a validação pela mesa no mesmo PK. O RED de S144 cobre divergência persistida e o controle cobre a releitura após uma devolução anterior à trava. [`test_e4c_f5_guarda_plantio_s144.py:143`](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:143)

`SEM_PORTA materializacao.py:2204 persistido` — pergunta pendente, muda e morta pelo C6, sem carimbo, com coluna antiga e ato devolvido. A disputa fecha sem chamado pai ou destino. Nova resposta exige C6 vivo, validação exige resposta e chamado, recusa e correção exigem resposta e disputa aberta, e devolução exige o recorte de hora e recusa escrita. `views.py:1238–1246`, `validacao.py:698–701`, `fio.py:641–687`, `juizes.py:1762–1767`, `validacao.py:269–274`.

`INALCANCAVEL materializacao.py:2204 instância velha no caminho Disputa.fechar` — S144 consulta a pergunta após a trava e o save da disputa. `models.py:2090–2097`, `models.py:2118–2122`, `signals.py:149–165`, `materializacao.py:2191–2192` na base.

Recorte para a sessão. Os três REDs verificam o motivo da porta, portanto devem falhar com `tirar_guarda`. Os dois do passivo também verificam o texto e a chave usados pelos mutantes `plantio_motivo_c6` e `passivo_balde_generico`. Execução [nao verificado].

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_guarda_plantio_s144
```

## 20/11 14:19 · sessao 3c3207a587c4 · E4C-F5-CONSTROI · leitura · origem=Code

# PAREI RASCUNHO — E4C-F5-CONSTROI · H4

**Unidade de leitura: uma `PerguntaDisputa`, pelo próprio PK.** A cópia já tem a guarda do par ato × coluna em `carimbar`. A medida registra dois aceites do core depois da absorção. O rascunho abaixo deixa a escolha em aberto. `app/chamados/models.py:3330–3350,3375–3405` · `medida_h4.log:333–334`

## 1. Como provar absorção vigente da mesma pergunta

**O código tem os dados para provar o caso positivo, mas ainda não tem um predicado executável de vigência.** `DecisaoPergunta.composta` entrega resposta, validação, instante de materialização e via. A leitura não entrega o ato de absorção nem a sua posição em relação a uma reabertura. O leitor de grupo e a tarefa do dia respondem a perguntas diferentes. `app/chamados/models.py:4325–4390` · `app/chamados/services/materializacao.py:1123–1156` · `app/chamados/services/tarefa_do_dia.py:158–192`

| Evidência | O que o código faz hoje |
|---|---|
| **Carimbo da própria pergunta** | `carimbar` escreve `materializada_em`, `materializada_por_id` e `via_resolucao`. Na passagem, o relógio é comum às irmãs e a via é `declaracao` se declarou hora, `ausencia_dia` caso contrário. Com texto, `resposta='validar'` pode completar os instantes e o autor da validação. A porta recusa se o C6 mantiver a pergunta viva ou se o carimbo deixar `(ato validado=False, coluna preenchida=True)`. `app/chamados/models.py:3324–3328,3361–3405` · `app/chamados/services/materializacao.py:1036–1081` |
| **Ato da própria pergunta** | A passagem grava `DecisaoPergunta(ato=ABSORCAO, pergunta=q)` com `ato_em` igual ao carimbo, `ato_por`, `ato_texto`, `absorcao_escape_id` e `absorcao_ausencia_id`. O carimbo e a linha ficam no mesmo `atomic` da irmã, sob a trava do colaborador. `registrar` insere a linha e acorda a célula. A linha é histórica e as portas de decisão impedem sua edição e exclusão. `app/chamados/services/materializacao.py:1052–1084` · `app/chamados/models.py:4219–4232,4252–4259,4280–4286,4401–4427` |
| **Quem desfaz** | `reabrir_para_correcao` registra `REABERTURA`, ou `RECUSA` quando chamada por essa porta, e limpa resposta, validação e carimbo pelo lote. A ressuscitação também grava `REABERTURA`, marcada com `da_ressuscitacao`, e limpa o trio pelo lote. Há descarimbo sem linha `REABERTURA` em `desfazer_carimbo_sem_lastro`. Uma via também pode mudar com o relógio preservado, por relavra ou por `encerrar_por_fato_no_espelho`. `app/chamados/services/disputa_emissao.py:1615–1669` · `app/chamados/services/validacao.py:926–944` · `app/chamados/models.py:1690–1722,3451–3461,3525–3549` · `app/chamados/services/relavra_via.py:106–128,229–252` |
| **Quem lê o grupo** | `perguntas_absorvidas_por` usa colaborador, dia, tipo `hora`, as duas vias, e o par `(disputa_id, materializada_em)` encontrado pelo texto do escape. Exclui pergunta com batida de origem não retratada. Na rejeição, sem outra ausência cobrindo o dia, o consumidor chama a reabertura dos PKs que o grupo devolve. A tarefa do dia, por sua vez, lê o estado da ausência ligada quando a `ABSORCAO` é o ato que ela está interpretando. `app/chamados/services/materializacao.py:1140–1156,1176–1207` · `app/ponto/signals.py:250–279` · `app/chamados/services/tarefa_do_dia.py:158–192` |

**Definição candidata para o aval.** Sob a trava existente, a releitura do PK confirma a absorção se o carimbo atual estiver preenchido com via `ausencia_dia` ou `declaracao`, houver `ABSORCAO` **desse PK** no mesmo instante do carimbo, com os dois vínculos, e a sequência de gravação desse PK não tiver `REABERTURA` ou `RECUSA` posterior àquela absorção. Para essa última comparação, o rascunho usa a ordem de inserção por PK entre os atos serializados pela mesma trava. O relógio `ato_em` sozinho dá a ordem errada no controle em que a recusa de T2 já foi gravada quando a passagem insere a absorção datada de T1. Uma `RESPOSTA` posterior por si só não encerra a absorção nessa definição. `app/chamados/services/materializacao.py:1036,1052–1081` · `app/chamados/models.py:4252–4259,4410–4412` · `app/config/settings/base.py:104` · `app/chamados/services/disputa_emissao.py:1510–1532,1630–1668` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:324–338`

A ausência de carimbo ou a reabertura posterior afasta a vigência nessa definição. Já carimbo sem ato correspondente deixa a absorção **sem confirmação**. `declaracao` também sai do plantio, `ausencia_dia` também sai de `via_pelo_fato`, e o código prevê carimbo legado sem linha. `pergunta_viva=False` pode decorrer de validação, competência trancada, identidade, célula ou lastro. A rejeição da ausência pode deixar o carimbo de pé se outra ausência cobrir o dia. O rascunho usa a devolução efetiva da pergunta como limite da vigência. Fazer a rejeição, por si, encerrar o veto fica aberto. `app/chamados/services/materializacao.py:1176–1203,1988–2005` · `app/chamados/juizes.py:1406–1500,1842–1849` · `app/chamados/models.py:4347–4358` · `app/chamados/services/tarefa_do_dia.py:184–192`

## 2. O que cada chamador faz hoje

O core retorna `(True, None)` depois da escrita. Os retornos `(False, erro)` de hoje são de entrada: resposta vazia, opção inválida, `OUTRO_MOTIVO` no ramo `hora`, formato de hora ou data/hora inválido e seleção de posto ou escala inválida. Ele toma a trava depois dessas verificações, relê a pergunta inteira, colhe o passivo e a assinatura antiga, lê o relógio e grava. `ja_respondida` serve à auditoria e ao rótulo da nota. No caminho aceito, a lista de irmãs recusadas pelo materializador é descartada. `app/chamados/services/disputa_emissao.py:1439–1496,1527–1579` · `app/chamados/models.py:2830–2836`

| Chamador direto | Antes do core | `(True, None)` | `(False, erro)` |
|---|---|---|---|
| **Web/PWA** | Busca a pergunta do titular e aplica C6 antes da trava. Pergunta morta ou ausente recebe a recusa da view. Hora digitada também exige toque. | `resposta_acao(ok=True)`, “Resposta registrada.” | `resposta_acao(ok=False)` com a mensagem do core. O envelope chama o registrador de recusa e dá toast HTMX `204` ou mensagem e redirect. `app/chamados/views.py:1237–1246,1259–1282` · `app/core/respostas.py:47–71` |
| **API** | C6 antes da trava. Pergunta morta ou ausente dá `404`; resposta vazia dá `400` próprio. | Registra `resp_pergunta_ok` e devolve `200` com `status`, `pergunta_id` e `respondida_em`. | Registra `resp_pergunta_recusada` e devolve `400` com `erro`. `app/api/views.py:2624–2664` |
| **Correção administrativa** | Exige hora, motivo, disputa aberta, tipo `hora` e validação composta ausente. Com chamado, grava a nota `horario_corrigido` **antes** do core. Card e fio/modal chegam à mesma ação. | Registra `corrigir_resposta_admin` e chama `validar_pergunta`. A validação pode devolver erro mesmo com a resposta já escrita. | `resp('erro', ...)`, sem log de sucesso nem chamada à validação. A nota anterior pode persistir. No card HTMX, a resposta pode renderizar o card com toast; no fio/modal, renderiza a parcial. `app/chamados/services/acoes_disputa.py:335–388` · `app/chamados/services/nota.py:87–107` · `app/chamados/views.py:377–416,523–536` · `app/chamados/services/fio.py:534–547,674–688` |
| **Declaração de hora do colaborador** | Lê o marco faltante, toma a trava do ato, seleciona pela canônica e por `pergunta_sem_lastro_ou_viva` e pode receber a mesma canônica por `abrir`. A validação composta barra antes do core. Resposta completa igual pode retornar `ja_declarada`. | Grava a nota de declaração e retorna `declarada`. A view devolve JSON de sucesso, envia push e registra a declaração. | Retorna `resposta_invalida` e a view devolve JSON `400`, antes da nota, do push e do registro de sucesso. A abertura ou agregação da disputa, o nascimento ou o reapontamento anteriores podem persistir porque a saída do `atomic` é normal. `app/chamados/services/declaracao_texto.py:217–256,290–305,336–380,414–428` · `app/chamados/views.py:1089–1100` · `app/chamados/services/disputa_emissao.py:483–645` · `app/chamados/models.py:3177–3234` |
| **Aplicação administrativa de declaração em texto** | Seleciona a proposta, toma a trava do ato e passa pela canônica e pela validação composta. Chama o core só se faltar resposta completa. Com resposta completa, segue direto à validação. | Chama `validar_pergunta` e devolve `ok`, estado e mensagem dela, com `pergunta_id`. A view usa o envelope de detalhe. | Retorna `resposta_invalida` com `pergunta_id` e pula a validação. Escritas anteriores do bloco também podem persistir. `app/chamados/services/declaracao_texto.py:217–256,643–699` · `app/chamados/views.py:410–416,477–481` |

Esses são os cinco sítios diretos encontrados. O upload de foto usa `responder_automaticamente`, ignora o booleano dessa porta e segue para `validar_pergunta`. O comando de limbo usa `carimbar_resposta_pela_trilha`. A declaração “não trabalhei” por chamado escreve contexto e nota. Nenhum desses três caminhos consome a tupla do core. `app/chamados/models.py:4562–4565` · `app/chamados/views.py:1276–1278` · `app/api/views.py:2652–2653` · `app/chamados/services/acoes_disputa.py:374–376` · `app/chamados/services/declaracao_texto.py:190–213,318–319,421,693–695` · `app/api/foto_recebida.py:38–54` · `app/chamados/management/commands/curar_perguntas_em_limbo.py:45,68–69`

## 3. RED que distingue antes e depois da trava

Na medida, as duas irmãs acabaram com uma `ABSORCAO`, carimbo `ausencia_dia`, resposta `19:02` com instante e validação ausente nos dois lados do par. A **com luz** terminou `resolvida` e morta. A **muda** terminou `respondida` e viva. Os dois testes da sonda saíram `ok` e registraram o aceite. Os dois `FAIL` foram do teste de resposta **anterior à absorção, já validada**, repetido nas duas classes. A cópia agora espera também a tentativa `celula_absolveu` mostrada pelo traceback. `medida_h4.log:323–346` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:271–309`

Para cada irmã, o RED começa com uma instância anterior à passagem. A interposição na aquisição da trava da **resposta** executa a passagem real para o mesmo PK, confirma o carimbo e a linha de decisão, e só então deixa a resposta adquirir a trava. O mutante antecipa a leitura **e a decisão** da guarda, preservando o `refresh_from_db()` normal para o restante da escrita. Assim o teste mede a posição da guarda, sem confundi-la com a remoção da releitura do escritor. A1(b) já fornece a técnica de interposição. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94` · `app/chamados/services/disputa_emissao.py:1478–1496` · `app/chamados/services/materializacao.py:1052–1081`

| Caso | C6 antes → depois da passagem | Guarda de absorção antes → depois | Core com guarda antecipada → sob a trava |
|---|---|---|---|
| **Irmã com luz** | `False → False`. Uma repetição do C6 recusaria dos dois lados. | Sem absorção confirmada → com absorção confirmada. | `(True, None) → (False, erro H4)` no desenho. Hoje a medida deu `(True, None)`. `medida_h4.log:333` · `app/chamados/services/materializacao.py:1052–1081` · `app/chamados/services/disputa_emissao.py:1481–1532,1579` |
| **Irmã muda** | `True → False`. A repetição do C6 distinguiria a posição só aqui. | Sem absorção confirmada → com absorção confirmada. | `(True, None) → (False, erro H4)` no desenho. Hoje a medida deu `(True, None)`. `medida_h4.log:334` · `app/chamados/services/materializacao.py:1052–1081` · `app/chamados/services/disputa_emissao.py:1481–1532,1579` |

No lado recusado, o RED compara a foto imediatamente posterior à passagem com a foto final. Exige o mesmo texto, instante, assinatura, carimbo e ato de absorção, sem `RESPOSTA` nova nem efeitos de sucesso do core. O controle inverso começa com absorção confirmada e interpõe a reabertura antes da trava da resposta. A guarda antecipada recusaria usando o carimbo antigo. A releitura sob trava deve permitir `(True, None)` para a resposta elegível no mesmo PK. `app/chamados/services/disputa_emissao.py:1481–1579,1630–1669` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:189–220`

**Cobertura escrita que já existe.** A1(b) interpõe **validação** antes da trava da resposta e exige aceite, limpeza de coluna e composta e preservação da assinatura. A1(c) interpõe a **resposta** antes da trava da passagem e exige que a resposta completa de T2 fique fora da absorção, inclusive com luz e na porta automática. Também há controles de empate, recusa anterior à aquisição, devolução da ausência rejeitada e rollback quando falha a inserção do ato. Nenhum desses testes inverte as duas operações para fixar o RED H4 acima. A fonte das duas sondas H4 e a execução dessa intercalação pela sonda estão **[nao verificado]**. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:85–104,142–220,271–338,481–497` · `medida_h4.log:333–345`

## 4. Desenhos e custo

Os desenhos usam a evidência do item 1. A condição de escrita é nova. A leitura deve vir da fonte de decisões já existente, com dados frescos, sem acrescentar um juiz de vitalidade ou um atalho SQL no chamador. Repetir C6 depois do refresh não basta como prova de frescor: o memo é da instância, sua chave não inclui os atos e `registrar` invalida a instância que recebeu. `app/chamados/models.py:4347–4390,4411–4424` · `app/chamados/juizes.py:826–899`

| Desenho | Arquivos, sítios e testes | O que a ficha barra e o que fica aberto | [nao verificado]
|---|---|---|
| **A. Guarda estrita no core para os cinco sítios** | Dois arquivos de produção: `models.py` na leitura da decisão e `disputa_emissao.py` logo após o refresh. Acrescenta os dois REDs H4, o inverso de reabertura e controles da correção e das duas declarações. Mantém a expectativa do A1(b). `app/chamados/models.py:4347–4399` · `app/chamados/services/disputa_emissao.py:1478–1496` · `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94` · `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84–134` | **Barra:** muda a correção administrativa em qualquer caso em que ela passe suas guardas e encontre absorção confirmada no core. A nota prévia pode ficar gravada e a validação seguinte deixa de rodar. Uma guarda geral de C6 também quebraria A1(b) e a resposta durante a passagem no caso com luz. A variante estrita usa a trava existente e deixa a decisão de vigência apoiada no ato. `app/chamados/services/acoes_disputa.py:344–388` · `app/chamados/juizes.py:1406–1415` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:142–167` |
| **B. Guarda estrita no core, habilitada pelo reexport que as quatro entradas já usam** | Dois arquivos de produção: `models.py` na leitura e no reexport atual, e `disputa_emissao.py` para a opção explícita e a guarda após refresh. Web, API e ambas as declarações já importam de `chamados.models`. A correção importa diretamente do serviço. Um adaptador no reexport habilita a guarda e delega ao mesmo escritor. REDs H4 pela entrada com guarda, prova de que as quatro entradas chegam a ela e controle de que a correção segue pelo caminho atual. `app/chamados/models.py:4347–4399,4562–4565` · `app/chamados/services/disputa_emissao.py:1478–1496` · `app/chamados/views.py:1274–1282` · `app/api/views.py:2652–2664` · `app/chamados/services/declaracao_texto.py:318–319,419–423,689–699` · `app/chamados/services/acoes_disputa.py:374–384` | Preserva A1(b), o caminho administrativo atual e a ordem das travas. A diferença de comportamento para a corrida web/API aparece no envelope já existente, depois da guarda de entrada. O custo é fazer o reexport carregar uma diferença de contrato hoje inexistente. Chamadas diretas ao serviço continuam fora dessa guarda, inclusive a correção. Se “web e API não mudam” incluir o aceite na janela H4, este desenho excede a ficha. `app/chamados/models.py:4562–4565` · `app/chamados/views.py:1242–1246,1278–1282` · `app/api/views.py:2640–2644,2653–2664` · `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94` |
| **C. Guarda só nas duas declarações, sob a trava que elas já têm** | Dois arquivos de produção: `models.py` para a leitura e `declaracao_texto.py` para uma condição compartilhada nos dois pontos que chamariam o core. Casos novos nas duas declarações, com retorno `resposta_invalida`, além dos controles de reabertura e carimbo sem ato. A interposição deve ocorrer na **primeira** trava do ato da declaração. Interpor na chamada reentrante do core provaria outra ordem. `app/chamados/services/declaracao_texto.py:290–305,350–380,419–423,656–670,689–699` · `app/chamados/tests/test_e4c_f5_voltaD_portas.py:103–134` | Preserva o escritor e os retornos atuais do core, a correção, A1(b) e as entradas web/API. Usa as travas atuais. Deixa aberta a janela H4 entre C6 e o core em web/API e preserva o aceite nas duas chamadas diretas medidas. No apply com resposta já completa, a validação direta continua fora do recorte. `app/chamados/views.py:1237–1246,1274–1282` · `app/api/views.py:2634–2656` · `app/chamados/services/declaracao_texto.py:689–699` · `medida_h4.log:333–334` |

**Alcance a declarar no aval.** A1(b) prova o aceite após validação isolada. Há também absorção de irmã com validação anterior preservada e absorção que completa a coluna. A ficha precisa dizer se o veto alcança a coexistência de validação vigente e absorção confirmada. O controle ancorado fornece o caso real de teste. Quanto a ponto e folha, a recusa de um escape pode suprimir a criação de falta aprovada, e a recusa de aplicação de declaração pode suprimir a validação, o plantio e o recálculo subsequentes. Os dois H4 medidos usaram hora, sem medida de efeito financeiro ou de competência exportada **[nao verificado]**. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:189–209` · `app/chamados/models.py:3366–3374` · `app/chamados/services/disputa_emissao.py:1556–1561` · `app/chamados/services/materializacao.py:1027–1035,1117–1120` · `app/chamados/services/declaracao_texto.py:695–699` · `app/chamados/services/validacao.py:752–779` · `medida_h4.log:333–334`

### Texto de aval para o desenho B

**PAREI RASCUNHO — F5 H4 · unidade: pergunta.**  
**PROBLEMA** — Depois de a passagem carimbar e registrar a absorção da mesma pergunta, o core ainda aceita a resposta nas duas ordens medidas. `medida_h4.log:333–334` · `app/chamados/services/materializacao.py:1052–1081` · `app/chamados/services/disputa_emissao.py:1481–1532,1579`

**LEIS** — L-130 e equivalência entre validação composta e presença da coluna. `app/docs/LEIS.md:151` · `app/chamados/models.py:4392–4399`

**CONDIÇÃO PROPOSTA** — Pelas quatro entradas que usam o reexport, reler sob a trava existente e, antes de qualquer escrita da resposta, devolver `(False, "A pergunta já foi resolvida pela ausência declarada.")` se estiver confirmada a absorção vigente do próprio PK pela via, carimbo e sequência descritos no item 1. Preservar o envelope de cada chamador. No escopo medido, as duas irmãs estão sem validação vigente. A extensão do veto à coexistência com validação fica para a escolha no chat. `app/chamados/models.py:4219–4259,4562–4565` · `app/chamados/services/disputa_emissao.py:1478–1532` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:189–209` · `medida_h4.log:333–334`

**RED** — Interpor a passagem real entre a leitura antecipada e a aquisição da trava da resposta, nos dois casos H4. O mutante que antecipa a guarda deve aceitar ambos. A leitura sob trava deve devolver o erro H4 e preservar a foto da passagem. Controles: reabertura interposta, A1(b), resposta durante a passagem, plantio com a mesma via sem ato de absorção, grupo recuperável e caminhos da correção e das declarações. `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94` · `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:142–220,324–338` · `app/chamados/services/materializacao.py:1140–1156,1988–2005` · `app/chamados/tests/test_e4c_f5_voltaD_portas.py:84–134`

**PROIBIDO** — Recusar a resposta posterior à

[truncado em 20000 de 22224 caracteres]

## 20/11 14:19 · sessao e9dc6eb0e8f1 · - · - · origem=Ronald (tmux)

A1 e A2 estão em [test_e4c_f5_guarda_reconciliador.py](/home/ronald/wt-codex-e4c-f5-constroi-184647/app/chamados/tests/test_e4c_f5_guarda_reconciliador.py:108). A execução de Django e dos mutantes fica com a sessão [nao verificado].

| Sítio e origem | Teste e caminho posterior | [nao verificado]
|---|---|
| `reconciliador.py:408`, instância velha e banco limpo | `test_RED_c1_prefetch_le_reabertura_apos_select_e_preserva_banco_limpo`. Interpõe a reabertura real entre o SELECT e o prefetch. A nova tentativa de reabrir retorna `sem_resposta_para_corrigir`. `test_e4c_f5_guarda_reconciliador.py:108–135` |
| `reconciliador.py:408`, par persistido | `test_RED_c1_par_persistido_recusa_e_questionario_reabre_o_mesmo_pk`. O titular reabre o questionário e limpa resposta e validação no mesmo PK. `test_e4c_f5_guarda_reconciliador.py:137–157` |
| `reconciliador.py:646`, ambas as origens | `INALCANCAVEL`. `test_A2_evento_celula_absolveu_entrega_coluna_nula_e_resposta_none` e `test_A2_AST_seleciona_celula_absolvida_com_coluna_nula`. `reconciliador.py:646,678–681`, `test_e4c_f5_guarda_reconciliador.py:184,219` |
| `models.py:3560`, ambas as origens | `INALCANCAVEL`. `test_A2_evento_orfa_entrega_coluna_nula_e_resposta_none` e `test_A2_AST_seleciona_orfa_com_coluna_nula`. `reconciliador.py:1096–1103`, `models.py:3552–3560`, `test_e4c_f5_guarda_reconciliador.py:187,222` |

`SEM_PORTA reconciliador.py:408 (a):` pergunta de texto, muda, sem validação, carimbo ou destino, C6 morto pela célula `concorde`. Disputa aberta, uma reabertura e nenhum chamado pai ou destino. Nova resposta exige C6 vivo. Mesa e recusa exigem chamado listante e resposta completa. Reabertura exige resposta nas colunas e a correção direta exige chamado e tipo hora. Devolução exige declaração de hora com motivo de recusa, carimbo de paridade ou participação no grupo carimbado de absorção, conforme a porta. `test_e4c_f5_guarda_reconciliador.py:94–106,130–135`, `views.py:483–536,1238–1246`, `juizes.py:1458–1474,1583–1588,1762–1767`, `models.py:2414–2417`, `services/acoes_disputa.py:349–355`, `services/validacao.py:191–200,269–274,348–351,698–704,915–923`, `services/materializacao.py:1149–1152`.

Remover a guarda deixa o C6 aceitar o carimbo nos dois REDs do C1. Ambos exigem `False`, a causa `RECUSA_PAR_ATO_COLUNA` e a foto preservada. Remover o filtro de `validada_em` de cada seletor derruba o AST correspondente e permite que o candidato divergente alcance o gatilho coberto pelo teste de integração. `models.py:3375–3405`, `test_e4c_f5_guarda_reconciliador.py:94–106,159–181,205–222`. [nao verificado]

```sh
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test chamados.tests.test_e4c_f5_guarda_reconciliador
```

## 20/11 14:18 · sessao c18cc3fee7d3 · - · - · origem=Ronald (tmux)

Criei dois REDs S143 para B persistido. Ambos acionam o `post_save` do chamado, observam a recusa real e comparam carimbo, coluna, lavra e atos gravados antes e depois. [test_e4c_f5_guarda_par_s143.py](chamados/tests/test_e4c_f5_guarda_par_s143.py:52)

| Sítio | RED | Porta posterior executada |
|---|---|---|
| `disputa_emissao.py:2043` | `test_RED_destino_recusa_b_persistido_e_a_mesa_recusa_o_mesmo_pk` | POST de recusa pelo chamado-destino. O mesmo PK vira pendente, recebe `RECUSA` e fica com par `(False, False)` (`test_e4c_f5_guarda_par_s143.py:90`). |
| `disputa_emissao.py:2117` | `test_RED_sem_credor_recusa_b_persistido_e_titular_reabre_o_mesmo_pk` | O titular reabre o questionário. O mesmo PK vira pendente, recebe `REABERTURA` e fica com par `(False, False)` (`test_e4c_f5_guarda_par_s143.py:119`). |

`INALCANCAVEL disputa_emissao.py:2043 origem a` e `INALCANCAVEL disputa_emissao.py:2117 origem a` para o fio do mesmo dono. A trava do colaborador antecede ambas as cargas e é compartilhada com as portas canônicas de resposta, correção, recusa e validação (`disputa_emissao.py:1924–1932`, `:1963–1998`, `:2083–2097`, `:1478–1485`, `:1627–1633`, `validacao.py:746–800`, `:938–944`). O filtro do laço direto admite uma FK cruzada preexistente. A costura atual barra esse vínculo (`models.py:3047–3050`, `fio.py:1741–1757`). Existência desse passivo [nao verificado].

A3. A reabertura pode gravar `REABERTURA` ao ressuscitar perguntas. O escritor exige materialização na seleção e na releitura, enquanto a carga do laço direto exige materialização nula (`disputa_emissao.py:1963–1967`, `:2000–2004`, `models.py:1549–1555`, `:1669–1671`, `:1713–1722`). O cartório síncrono usa a mesma porta (`signals.py:189–218`, `ponto/services/cartorio.py:1690–1721`). A segunda carga vem depois das reaberturas (`disputa_emissao.py:2060–2097`). Com isso, nenhuma releitura adicional foi aplicada ao escritor identificado.

O mutante que tira a guarda é coberto pelos dois testes. Ambos exigem `False` e `RECUSA_PAR_ATO_COLUNA` no retorno real (`test_e4c_f5_guarda_par_s143.py:77–84`). AST verificada, execução e mutante [nao verificado]. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python manage.py test chamados.tests.test_e4c_f5_guarda_par_s143
```

## 20/11 14:11 · sessao 999def1cb322 · E4C-F5-CONSTROI · leitura · origem=Code

N_DECIDE = 42
N_EXIBE = 1

Contei uma vez cada sítio final que consome o instante devolvido por `composta`. Chamadas em linhas distintas contam separadamente, mesmo quando chegam ao mesmo juiz. A projeção temporal e o desempacotamento contam junto com a guarda que usa o valor. Há ainda **2 EXIBE** que chamam `leitura` diretamente. Incluindo essas duas rotas, o inventário do instante do ato soma **42 DECIDE e 3 EXIBE**. `app/chamados/models.py:4360`, `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1428`, `app/chamados/models.py:1715`, `app/chamados/services/historico.py:106`

Pelo item 1 do aval, o resultado devolve a **PERGUNTA DE DESENHO** ao topo e a pre-opus fica aguardando. `respostas_2011_1410.txt:4` do anexo raspado, contando da linha `aval Ronald`.

## Instante devolvido por `composta`

O acesso ao atributo ocorre em `projecao_temporal_da_pergunta`, `respondida_sem_texto`, `pergunta_respondida`, `espera_o_texto_do_colaborador`, no serializador do APK e no evento de auditoria da absorção. A tabela rastreia esses seis acessos até os usos finais, inclusive os aliases `_pergunta_respondida` e as tuplas recortadas pelo horizonte. `app/chamados/juizes.py:1391`, `app/chamados/juizes.py:1541`, `app/chamados/juizes.py:1560`, `app/chamados/services/fio.py:190`, `app/api/views.py:2581`, `app/chamados/services/materializacao.py:1091`

| Classe | Sítio | Função e uso |
|---|---|---|
| DECIDE | `app/api/views.py:2581` | `api_disputa_perguntas._item` recebe a composta por parâmetro e serializa o instante para o APK, tanto no histórico quanto nas pendentes. **DECIDE por dúvida**. O uso desse campo pelo APK está [nao verificado]. `app/api/views.py:2542`, `app/api/views.py:2587`, `app/api/views.py:2601`, `app/api/urls.py:92` |
| DECIDE | `app/api/views.py:2590` | `api_disputa_perguntas` usa o juiz para incluir o item em `respondidas`. |
| DECIDE | `app/api/views.py:2646` | `api_disputa_responder` determina `ja_respondida` e passa o resultado ao core, que o usa para rotular a trilha. `app/api/views.py:2653`, `app/chamados/services/disputa_emissao.py:1552`, `app/chamados/services/disputa_emissao.py:1567` |
| DECIDE | `app/chamados/juizes.py:353` | `_slot_esperado` desempacota o instante projetado e pode suprimir o slot no horizonte. A mesma guarda também lê a coluna crua. `app/chamados/juizes.py:351` |
| DECIDE | `app/chamados/juizes.py:1428` | `_pergunta_viva_calc` desempacota, recompõe o par com `_replace` e entrega ao juiz de resposta no C6. `app/chamados/juizes.py:1409` |
| DECIDE | `app/chamados/models.py:2026` | `DisputaSupervisao.sem_veredito_entre` seleciona respostas que ainda seguram o fechamento. `app/chamados/models.py:2024`, `app/chamados/models.py:2029` |
| DECIDE | `app/chamados/models.py:3265`, `app/chamados/models.py:3268` | `PerguntaDisputa.estado` usa o juiz em dois ramos para distinguir `RESPONDIDA` de `PENDENTE`. |
| DECIDE | `app/chamados/models.py:3816` | `PerguntaDisputa.respondida` entrega a composta ao juiz da propriedade. |
| DECIDE | `app/chamados/reconciliador.py:367` | `lavrar_sem_turno_aberto` usa a resposta como guarda da lavra. |
| DECIDE | `app/chamados/reconciliador.py:1136` | `veredito_orfa` usa a resposta como guarda do veredito. |
| DECIDE | `app/chamados/views.py:1275` | `responder_pergunta_disputa` determina `ja_respondida` para o core rotular a trilha. `app/chamados/services/disputa_emissao.py:1552`, `app/chamados/services/disputa_emissao.py:1567` |
| DECIDE | `app/chamados/views_cobrar.py:170`, `app/chamados/views_cobrar.py:178` | `validar_inline` seleciona as respondidas a validar e, no outro ramo, as vivas que aguardam o colaborador. |
| DECIDE | `app/chamados/services/acoes_disputa.py:134` | `acao_regularizar_disputa` barra regularização sem resposta de pergunta viva. |
| DECIDE | `app/chamados/services/becos.py:32` | `classificar` usa a resposta para decidir se há pergunta visível antes de classificar o beco. |
| DECIDE | `app/chamados/services/declaracao_texto.py:419` | `declarar_hora_colab` testa resposta vigente com a mesma hora. |
| DECIDE | `app/chamados/services/declaracao_texto.py:689` | `aplicar_declaracao` decide se precisa registrar a resposta pela porta. |
| DECIDE | `app/chamados/services/disputa_emissao.py:1866` | `reabrir_credor_de_pergunta_viva` barra reabertura do credor de pergunta respondida. |
| DECIDE | `app/chamados/services/disputa_emissao.py:2037` | `_resolver_perguntas_por_chamado_resolvido` protege a resposta que ainda não foi materializada de fato. |
| DECIDE | `app/chamados/services/fio.py:190` | `espera_o_texto_do_colaborador` testa a ausência do instante e consulta `respondida_sem_texto` na mesma expressão. O resultado recorta cobrança, cartão e porta de correção. `app/chamados/services/cobranca.py:242`, `app/chamados/services/fio.py:362`, `app/api/views.py:1179` |
| DECIDE | `app/chamados/services/fio.py:712` | `processar_post_fio` barra fechamento manual quando falta resposta de pergunta viva. |
| DECIDE | `app/chamados/services/fio.py:1288` | `canonica_do_grupo` dá preferência à pergunta respondida. |
| DECIDE | `app/chamados/services/fio.py:1301` | `merito_perdedor` identifica respostas com decisão de mérito que podem vetar a retratação. |
| DECIDE | `app/chamados/services/materializacao.py:193` | `par_do_intervalo` exige resposta da metade antes de examinar o par. |
| DECIDE | `app/chamados/services/materializacao.py:262` | `paridade_absolvida_bug_par_103` verifica elegibilidade para devolução. |
| DECIDE | `app/chamados/services/materializacao.py:294` | `_situacao_da_irma` consulta o juiz para classificar a irmã sem carimbo. |
| DECIDE | `app/chamados/services/materializacao.py:890` | `estado_aplicacao_pergunta` verifica se há resposta de hora a aplicar. |
| DECIDE | `app/chamados/services/materializacao.py:1060` | `_materializar_ausencia_de_resposta` combina a resposta vigente com o instante desempacotado da projeção. A guarda preserva a resposta posterior ao relógio da absorção. `app/chamados/services/materializacao.py:1057`, `app/chamados/services/materializacao.py:1062` |
| DECIDE | `app/chamados/services/materializacao.py:1323` | `_recusa_sem_anuencia` decide se recusa aplicar o fato por falta de resposta. |
| DECIDE | `app/chamados/services/pergunta_no_app.py:88` | `particao_do_app` decide a separação entre pendentes e histórico. |
| DECIDE | `app/chamados/services/validacao.py:699` | `validar_pergunta` barra a validação sem resposta. |
| DECIDE | `app/chamados/services/validacao.py:920` | `recusar_pergunta` verifica se ainda existe resposta a devolver. |
| DECIDE | `app/chamados/services/vigia_fio.py:637` | `censo_alcance` consulta o juiz para classificar a causa `c`. |
| DECIDE | `app/chamados/management/commands/auditar_invariantes_chamados.py:179` | `_contraprova_i21a` consulta o juiz para verificar a contraprova do I21. |
| DECIDE | `app/chamados/management/commands/auditar_invariantes_chamados.py:598`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1672` | `Command.handle` usa o juiz para separar o piso do I2 e para excluir respondidas do universo do F1, respectivamente. |
| DECIDE | `app/chamados/management/commands/dedup_perguntas_fato.py:76` | `Command.handle` separa as perguntas com resposta no grupo de deduplicação. |
| DECIDE | `app/colaboradores/services/ficha.py:173` | `ficha_do_colab` seleciona as perguntas sem resposta. |
| DECIDE | `app/escala/services/cadastro_realidade.py:500` | `beco_em_lote.respondida_no_horizonte` desempacota a projeção, repõe o instante com `_replace` e consulta o juiz para o critério M1. `app/escala/services/cadastro_realidade.py:499`, `app/escala/services/cadastro_realidade.py:508` |
| DECIDE | `app/folha/previa.py:143` | `_material` consulta o juiz para classificar o material disponível para o dia na prévia. `app/folha/previa.py:95` |
| DECIDE | `app/ponto/services/cartorio.py:1689` | `_julgar_colab_corpo._tem_resposta_do_colab` protege da reconciliação o chamado que tem resposta. `app/ponto/services/cartorio.py:1696` |
| EXIBE | `app/chamados/services/materializacao.py:1091` | `_materializar_ausencia_de_resposta` serializa o instante no detalhe da auditoria. A guarda de absorção já foi avaliada em `app/chamados/services/materializacao.py:1060`. O destino do campo é `LogAuditoria.valor_depois`. `app/chamados/services/materializacao.py:1087`, `app/core/observ.py:134`, `app/core/observ.py:136` |

As chamadas ao juiz nos censos, nas auditorias e na prévia entram como **DECIDE** porque o instante participa do predicado antes da saída de apresentação. `app/chamados/services/vigia_fio.py:637`, `app/chamados/management/commands/auditar_invariantes_chamados.py:179`, `app/folha/previa.py:143`

## Instante de `leitura` direta

| Classe | Sítio | Função e uso |
|---|---|---|
| EXIBE | `app/chamados/models.py:1715` | `ChamadoColaborador.ressuscitar_perguntas` copia o instante para `respondida_original_em` na foto da reabertura. A foto é gravada para os alvos já selecionados, e o instante é exibido em “Respostas anteriores”. `app/chamados/models.py:1705`, `app/chamados/models.py:1719`, `app/chamados/models.py:3818`, `app/templates/chamados/partials/modal_fio.html:212` |
| EXIBE | `app/chamados/services/historico.py:106` | `_eventos_dos_chamados` lê o campo por `getattr` e usa o instante para datar, recortar e ordenar a cronologia exibida pela mensageria. `app/chamados/services/historico.py:98`, `app/chamados/services/historico.py:100`, `app/chamados/services/historico.py:185`, `app/chamados/services/historico.py:208`, `app/api/views_mensageria.py:2177`, `app/api/views_mensageria.py:2199` |

`composta` repassa o instante em `app/chamados/models.py:4361`. A leitura entregue a `_pela_decisao` fornece `validada_em` a esse consumidor. O vigia de texto acessa `resposta_colab`, e o comparador ato–coluna acessa `validada_em`. `app/chamados/services/tarefa_do_dia.py:163`, `app/chamados/services/tarefa_do_dia.py:429`, `app/chamados/services/tarefa_do_dia.py:474`, `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/models.py:4371`

## Coluna crua `PerguntaDisputa.respondida_em`

**59 sítios de leitura da coluna, 35 DECIDE e 24 EXIBE.** A unidade aqui é a linha do acesso, projeção ou expressão SQL que lê a coluna, incluindo os acessos indiretos comprovados na tabela. As aplicações das faces SQL aparecem como rastreio das três expressões em `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588` e `app/chamados/juizes.py:1599`.

| Classe | Sítio | Função e uso |
|---|---|---|
| DECIDE | `app/chamados/models.py:1567` | `_universo_da_ressuscitacao` protege a pergunta com texto sem carimbo. |
| DECIDE | `app/chamados/models.py:3347`, `app/chamados/models.py:3351` | `carimbar` guarda o valor para desfazer a tentativa e verifica se precisa preenchê-lo. `app/chamados/models.py:3381` |
| DECIDE | `app/chamados/models.py:3495`, `app/chamados/models.py:3730`, `app/chamados/models.py:3731` | `descarimbar_em_lote` e `_recusa_par_ilegal` usam a coluna na guarda do par. |
| DECIDE | `app/chamados/models.py:4276`, `app/chamados/models.py:4285`, `app/chamados/models.py:4286`, `app/chamados/models.py:4287` | `linhas_da_projecao_sem_linha` seleciona, pareia, deduplica e preenche fotos do legado. Também tem caminhos diretos para escrita de atos e backfill. `app/chamados/models.py:1673`, `app/chamados/services/validacao.py:452`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:150` |
| DECIDE | `app/chamados/juizes.py:353`, `app/chamados/juizes.py:857` | `_slot_esperado` consulta a coluna na guarda do horizonte. `_chave_memo_pv` inclui a coluna na chave de vitalidade. |
| DECIDE | `app/chamados/juizes.py:1541`, `app/chamados/juizes.py:1548`, `app/chamados/juizes.py:1560` | `respondida_sem_texto`, `texto_sem_carimbo` e `pergunta_respondida` recebem coluna crua por ORM ou por objeto intermediário. `app/chamados/juizes.py:1752`, `app/chamados/services/validacao.py:406`, `app/chamados/services/vigia_fio.py:635`, `app/chamados/models.py:3731`, `app/chamados/services/fio.py:1714`, `app/chamados/services/fio.py:1719`, `app/chamados/services/raiox.py:147` |
| DECIDE | `app/chamados/juizes.py:1580`, `app/chamados/juizes.py:1588`, `app/chamados/juizes.py:1599`, `app/chamados/juizes.py:1774` | `q_respondida_sem_texto`, `q_respondida_com_texto`, `q_nao_respondida` e a ordenação de `perguntas_respondidas_a_validar`. |
| DECIDE | `app/chamados/veredito_lavrado.py:207`, `app/chamados/veredito_lavrado.py:208` | `marcos_do_lote` filtra pelo horizonte e agrega o máximo para verificar a vigência da lavra. `app/chamados/veredito_lavrado.py:230` |
| DECIDE | `app/chamados/services/disputa_emissao.py:1642`, `app/chamados/services/disputa_emissao.py:1654` | `reabrir_para_correcao` desempacota a coluna para selecionar as vazias e decidir quais ganham ato com foto. `app/chamados/services/disputa_emissao.py:1644`, `app/chamados/services/disputa_emissao.py:1650`, `app/chamados/services/disputa_emissao.py:1665` |
| DECIDE | `app/chamados/services/disputa_emissao.py:1713`, `app/chamados/services/disputa_emissao.py:1884` | `carimbar_resposta_pela_trilha` guarda o carimbo existente. `reabrir_credor_de_pergunta_viva` separa o limbo. |
| EXIBE | `app/chamados/services/disputa_emissao.py:2109`, `app/chamados/services/disputa_emissao.py:2126` | `_resolver_perguntas_por_chamado_resolvido` serializa antes e depois na auditoria. `app/chamados/services/disputa_emissao.py:2120` |
| EXIBE | `app/chamados/services/validacao.py:450`, `app/chamados/services/validacao.py:931`, `app/chamados/services/materializacao.py:1189` | `devolver_instante_sem_texto_l130`, `recusar_pergunta` e `devolver_absorvidas_da_ausencia_rejeitada` incluem a coluna na foto de auditoria. `app/chamados/services/validacao.py:465`, `app/chamados/services/validacao.py:952`, `app/chamados/services/materializacao.py:1208` |
| EXIBE | `app/chamados/services/validacao.py:1275` | `fila_validar_classificada` calcula a idade e as estatísticas. A classe e as referências do lote saem de cálculos separados. `app/chamados/services/validacao.py:1273`, `app/chamados/services/validacao.py:1279`, `app/chamados/services/validacao.py:1284` |
| DECIDE | `app/api/views.py:1165`, `app/api/views.py:2663` | `api_chamados_v2` e `api_disputa_responder` serializam o valor cru. **DECIDE por dúvida**, pois o uso do campo no APK está [nao verificado]. `app/api/urls.py:73`, `app/api/urls.py:93` |
| EXIBE | `app/api/views_mensageria.py:571` | `_item_validar` serializa o instante no JSON. O adaptador da fila seleciona outros campos, incluindo a idade. `mensageria/nucleo/ferramentas.py:116`, `mensageria/nucleo/ferramentas.py:128` |
| DECIDE | `app/api/views_mensageria.py:572` | `_item_validar` entrega `dias_esperando` à fila do copiloto. **DECIDE por dúvida** sobre como o campo entra nas propostas. `app/api/views_mensageria.py:527`, `mensageria/nucleo/ferramentas.py:128` |
| EXIBE | `app/templates/chamados/partials/modal_fio.html:220`, `app/templates/chamados/partials/_pergunta_card.html:32`, `app/templates/chamados/partials/_card_atendimento.html:106`, `app/templates/chamados/partials/_card_atendimento.html:143`, `app/templates/chamados/partials/_card_atendimento.html:394` | Os três templates formatam a data de resposta para exibição. |
| DECIDE | `app/chamados/services/foto_e4b.py:251` | `foto_do_colaborador.guardar` lê campos concretos, inclusive os da pergunta. A diferença das fotos pode vetar a tentativa. `app/chamados/services/foto_e4b.py:121`, `app/chamados/services/foto_e4b.py:272`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:330`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:339` |
| EXIBE | `app/chamados/services/foto_e4b.py:180`, `app/chamados/services/passivo_validado.py:83` | `nascimentos` usa a coluna fotografada no relato. `_reversao_de` serializa o campo no arquivo de reversão. `app/chamados/services/foto_e4b.py:199`, `app/chamados/services/passivo_validado.py:66`, `app/chamados/services/passivo_validado.py:370` |
| DECIDE | `app/chamados/services/painel.py:67` | `marcador_do_placar` agrega o instante para o ETag, que controla a resposta HTTP. `app/chamados/services/painel.py:91`, `app/chamados/views.py:822` |
| DECIDE | `app/chamados/services/raiox.py:138` | `creditos_perguntas` põe a coluna na tupla e passa `p[3]` ao juiz para separar os créditos. `app/chamados/services/raiox.py:147` |
| DECIDE | `app/chamados/services/tarefa_do_dia.py:419`, `app/chamados/services/tarefa_do_dia.py:434` | `VistaNoHorizonte.__getattr__` recorta a coluna no fallback. `_fatos_das_perguntas` usa o `ato_em` derivado da coluna para decidir o passivo no horizonte. `app/chamados/services/tarefa_do_dia.py:420`, `app/chamados/services/tarefa_do_dia.py:429`, `app/chamados/services/tarefa_do_dia.py:433`, `app/chamados/models.py:4287` |
| EXIBE | `app/chamados/services/verbos.py:337`, `app/chamados/services/verbos.py:338`, `app/chamados/services/verbos.py:344`, `app/chamados/services/verbos.py:347` | `placar_aceite` lê o instante para os totais de respostas após cobrança. `app/chamados/services/verbos.py:350`, `app/chamados/services/painel.py:298` |
| EXIBE | `app/chamados/services/vigia_fio.py:749` | `respondida_sem_resposta` usa o instante como corte do contador exibido e registrado pelo vigia. `app/chamados/management/commands/vigiar_fio_morto.py:224`, `app/chamados/management/commands/vigiar_fio_morto.py:280` |
| EXIBE | `app/chamados/management/commands/auditar_invariantes_chamados.py:1116`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:57` | `Command.handle` filtra o relatório I20. `linhas_a_criar` seleciona a entrada do plano de backfill exibido pelo comando. `app/chamados/management/commands/backfill_decisoes_pergunta.py:482` |
| DECIDE | `app/chamados/management/commands/curar_perguntas_em_limbo.py:47`, `app/chamados/management/commands/curar_perguntas_em_limbo.py:70` | `Command.handle` seleciona o limbo e recorta as perguntas a reabrir. |
| EXIBE | `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:58`, `bin/censo_folha.py:28`, `bin/censo_folha.py:29` | `Command.handle` serializa a foto para reversão. O script de folha calcula os agravantes do relatório e do CSV. `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:66`, `bin/censo_folha.py:37`, `bin/censo_folha.py:47` |

As faces SQL alimentam corrigíveis, fila e vitalidade em `app/chamados/juizes.py:1765`, `app/chamados/juizes.py:1772`, `app/chamados/models.py:1552` e `app/chamados/models.py:3622`. Também entram nas portas de canal, reabertura, materialização e calendário em `app/chamados/canal.py:66`, `app/chamados/views.py:1412`, `app/chamados/services/acoes_disputa.py:98`, `app/chamados/services/disputa_emissao.py:1757`, `app/chamados/services/materializacao.py:146`, `app/chamados/services/materializacao.py:2129`, `app/colaboradores/services/calendario.py:505` e `app/ponto/management/commands/reconciliar_grade.py:461`. As aplicações no estoque, nas auditorias e nos relatórios estão em `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:37`, `app/chamados/management/commands/estoque_l130_instante_sem_texto.py:97`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1587`, `app/chamados/management/commands/auditar_invariantes_chamados.py:1618`, `app/chamados/management/commands/bug_par_103.py:73` e `app/chamados/services/verbos.py:335`.

## [nao verificado]

- A identidade Git da cópia em relação ao hash informado [nao verificado].
- O uso dos campos serializados por clientes fora da cópia, inclusive o APK, e por consumidores externos da auditoria [nao verificado].
- A incidência em produção, os resultados das três formas da sonda e caminhos montados em tempo de execução que escapem à leitura estática [nao verificado].

## 20/11 14:09 · sessao 446c25c36903 · RAIZ-DA-DISPUTA-F1-RASCUNHO · sonda · origem=Code

**Nenhuma das cinco leituras responde ao item 2 do aval.** A sonda conta disputas encerradas vinculadas ao chamado. Para saber se falta uma pergunta canônica, o código precisa da chave do fato que chegou, incluindo o motivo. O total e os cortes **2, 3 e 4+**, gerais e por `empresa_pk`, ficam **[nao verificado]**. (`sonda.py:32`, `sonda.py:81`, `sonda.py:107`, `app/chamados/models.py:3099`, `app/chamados/models.py:3113`)

### 1. Quem escolhe a pergunta e a disputa

- **Pergunta canônica do fato.** `PerguntaDisputa._nascer` consulta `fio.universo_do_fato` pela chave de colaborador, motivo, dia do turno e, quando aplicável, batida de origem. `fio.canonica_do_grupo` prefere uma pergunta respondida segundo a L-130, que exige instante e texto não vazio. Entre as respondidas escolhe o menor PK. Se nenhuma foi respondida, escolhe o menor PK do grupo. A pergunta eleita pode pertencer a outra disputa, inclusive encerrada. Com universo vazio, `ex` fica `None` e, se o pedido passar pelas guardas, a pergunta nasce na disputa fornecida pelo chamador. (`app/chamados/services/fio.py:1274`, `app/chamados/juizes.py:1544`, `app/chamados/services/fio.py:1948`, `app/chamados/models.py:2963`, `app/chamados/models.py:3099`, `app/chamados/models.py:3113`, `app/chamados/models.py:3200`, `app/chamados/models.py:3206`)
- **Disputa usada no repasse.** Para complementar marcos, o repasse pega a primeira pergunta com `chamado_destino=ch` e `materializada_em` nulo. A ordem padrão é `id` crescente. Ele usa a disputa dona dessa pergunta, sem filtro de fechamento. A emissão comum procura uma disputa aberta do colaborador e, se não acha, cria uma. A regra escrita para escolher **a disputa encerrada original quando chega um motivo novo sem canônica** está ausente nesses caminhos. `reabrir_sistema` recebe a disputa já selecionada e reabre essa linha. O aval do menor PK em `PROMPTS.md` trata da eleição do **chamado de destino**. (`app/chamados/services/disputa_emissao.py:1084`, `app/chamados/services/disputa_emissao.py:1095`, `app/chamados/models.py:3761`, `app/chamados/services/disputa_emissao.py:540`, `app/chamados/services/disputa_emissao.py:601`, `app/chamados/models.py:2326`, `app/docs/PROMPTS.md:2085`)
- **Ações do fio.** Na validação, o `pergunta_id` enviado determina a disputa, após a conferência de que a pergunta pertence ao fio do chamado. O resolvedor de disputa aberta tenta primeiro o chamado-pai, pela abertura mais recente, e depois o destino, pelo maior PK de pergunta. A reabertura administrativa tem sua própria escolha, pela `fechada_em` mais recente entre as disputas cujo pai é o chamado. (`app/chamados/services/fio.py:131`, `app/chamados/services/fio.py:638`, `app/chamados/models.py:1941`, `app/chamados/services/acoes_disputa.py:259`)

### 2. O que as cinco leituras mediram

| Leitura | Vínculo contado | Chamados com 2+ encerradas |
|---|---|---:|
| `destino` | Chamado de destino da pergunta. `sonda.py:113` | 648 |
| `destino_sem_materializacao` | Mesmo destino, com `materializada_em` nulo. `sonda.py:115` | 11 |
| `pai` | Chamado-pai da disputa, inclusive sem pergunta. `sonda.py:107` | 2 |
| `pai_ou_destino` | União dos vínculos de pai e destino, deduplicada por disputa. `sonda.py:110`, `sonda.py:114` | 650 |
| `ancora_da_pergunta_no_fio` | Destino da pergunta, com recurso ao pai quando o destino está vazio. `sonda.py:117`, `app/chamados/services/fio.py:116` | 648 |

Os números são dos resumos do dado raspado. Todas as linhas da tabela usam o mesmo corte de fechamento, `fechada_em` preenchido. O filtro de materialização e a âncora do fio também deixam de fora o teste do universo do fato. (`sonda.py:32`, `sonda.py:54`, `sonda.py:101`, `sonda.py:115`, `app/chamados/services/fio.py:1948`)

### 3. Sonda corrigida

Usei `destino` para medir o vínculo do repasse. A entrada `SONDA_PEDIDOS` é um JSONL já disponível ao leitor, com `[chamado_pk, motivo, dia_do_turno_apurado, batida_origem_pk_ou_null]` por pedido novo que chega à eleição. A sonda conta cada chamado uma vez se algum desses pedidos tem universo vazio. A busca da canônica abrange o fato inteiro, inclusive perguntas em outras disputas ou destinos. Para publicar a incidência do aval, a entrada precisa cobrir os pedidos do recorte no estado medido. Essa entrada está **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1084`, `app/chamados/models.py:3058`, `app/chamados/models.py:3067`, `app/chamados/models.py:3099`, `app/chamados/models.py:3113`)

```python
import json
import os
from collections import Counter
from datetime import date

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Count, Q

from chamados.models import PerguntaDisputa
from chamados.services.fio import universo_do_fato


def emitir(contagem, **grupo):
    cortes = {c: contagem[c] for c in ("2", "3", "4+")}
    print(json.dumps(dict(grupo, dois_ou_mais=sum(cortes.values()), **cortes)))


with transaction.atomic():
    with connection.cursor() as cursor:
        cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
        cursor.execute("SELECT current_database(), current_schema()")
        if cursor.fetchone() != ("saas_hasner", "juliani") or getattr(settings, "HASNER_SOMBRA", False):
            raise SystemExit("INCONCLUSIVA: banco ou schema fora do envelope")

    linhas = (
        PerguntaDisputa.objects.filter(chamado_destino__isnull=False).order_by()
        .values_list("chamado_destino_id", "chamado_destino__colaborador_id",
                     "chamado_destino__colaborador__empresa_id")
        .annotate(n=Count("disputa_id", distinct=True,
                          filter=Q(disputa__fechada_em__isnull=False)))
    )
    candidatos, por_empresa = {}, {}
    for chamado, colaborador, empresa, n in linhas.iterator(chunk_size=4000):
        por_empresa.setdefault(empresa, Counter())
        if n >= 2:
            candidatos[chamado] = colaborador, empresa, n

    contados = set()
    with open(os.environ["SONDA_PEDIDOS"], encoding="utf-8") as pedidos:
        for linha in pedidos:
            chamado, motivo, dia, batida = json.loads(linha)
            if chamado not in candidatos or chamado in contados:
                continue
            colaborador, empresa, n = candidatos[chamado]
            if universo_do_fato(colaborador, motivo, date.fromisoformat(dia),
                                batida_origem_id=batida):
                continue
            contados.add(chamado)
            por_empresa[empresa][str(n) if n < 4 else "4+"] += 1

    total = Counter()
    for contagem in por_empresa.values():
        total.update(contagem)
    emitir(total)
    for empresa, contagem in sorted(por_empresa.items()):
        emitir(contagem, empresa_pk=empresa)
```

### 4. O que ficou sem verificação

- Os pedidos novos efetivamente recebidos, a chave de cada fato, a canônica que o código encontraria naquele estado e a incidência resultante, total e por empresa. **[nao verificado]**
- A causa dos vínculos múltiplos e qual disputa anterior deveria receber o motivo novo. **[nao verificado]**
- A execução da sonda corrigida em produção. Conferi a sintaxe e a agregação com dados simulados em memória. A cópia foi identificada pelo diretório do hash, conforme o mecanismo de exportação. A identidade do commit e a revisão em execução em produção ficaram **[nao verificado]**. (`bin/codex_pergunta.sh:304`, `bin/codex_pergunta.sh:315`)

## 20/11 14:08 · sessao 38319cb10f70 · - · - · origem=Ronald (tmux)

Na faixa que examinei, há **31 linhas que leem a coluna crua**. A unidade é a linha de código que lê o campo, inclusive por ORM ou por um objeto intermediário. Chamadores dos predicados e `.only()` que prepara a instância aparecem como contexto da trilha.

- **DECIDE** — `_universo_da_ressuscitacao` protege texto sem foto possível em `app/chamados/models.py:1567`. `carimbar` guarda o carimbo para desfazer a tentativa e testa se precisa preenchê-lo em `app/chamados/models.py:3347,3351`. `descarimbar_em_lote` e `_recusa_par_ilegal` usam a coluna na guarda do par em `app/chamados/models.py:3495,3730,3731`. `linhas_da_projecao_sem_linha` usa a coluna para selecionar, parear, deduplicar e preencher atos do legado em `app/chamados/models.py:4276,4285,4286,4287`.
- **DECIDE** — `_slot_esperado`, `_chave_memo_pv`, `respondida_sem_texto`, `texto_sem_carimbo`, `pergunta_respondida`, as três faces SQL e `perguntas_respondidas_a_validar` em `app/chamados/juizes.py:353,857,1541,1548,1560,1580,1588,1599,1774`. Os predicados de `:1541` e `:1560` também recebem o instante do ato em rotas próprias. A origem crua de `:1560` vem de `app/chamados/services/raiox.py:138,147`, e a de `:1541` vem, entre outros, de `app/chamados/juizes.py:1752` e `app/chamados/services/validacao.py:406`. A origem crua de `:1548` pode chegar por `app/chamados/models.py:3495,3731` e `app/chamados/services/fio.py:1714,1719`.
- **DECIDE** — `marcos_do_lote` filtra o carimbo pelo horizonte e pega o máximo para invalidar lavra em `app/chamados/veredito_lavrado.py:207,208,230`.
- **DECIDE** — `reabrir_para_correcao`, `carimbar_resposta_pela_trilha`, `reabrir_credor_de_pergunta_viva` e `fila_validar_classificada` em `app/chamados/services/disputa_emissao.py:1642,1654,1713,1884` e `app/chamados/services/validacao.py:1275`. Este último é um caso conservador. A idade integra as estatísticas da fila, enquanto a classificação e as referências do lote são calculadas separadamente. `app/chamados/services/validacao.py:1273,1278,1284`.
- **EXIBE** — `_resolver_perguntas_por_chamado_resolvido`, `devolver_instante_sem_texto_l130`, `recusar_pergunta` e `devolver_absorvidas_da_ausencia_rejeitada` serializam o carimbo cru apenas nas fotos de auditoria. `app/chamados/services/disputa_emissao.py:2109,2126`, `app/chamados/services/validacao.py:450,465,931,951`, `app/chamados/services/materializacao.py:1189,1208`.

Entre os acessos indiretos do ato, `app/chamados/services/historico.py:106` lê o atributo por `getattr`, a partir do nome em `:100`, e usa o instante na cronologia enviada como texto. **EXIBE**. `app/api/views_mensageria.py:2177,2199`, `mensageria/nucleo/ferramentas.py:992,1005,1008`.

`app/chamados/models.py:1715` preserva o instante da `leitura` como `respondida_original_em` num ato de reabertura. O destino nominal é a exibição de respostas anteriores. O gravador inclui a reabertura para cada alvo já selecionado, independentemente desse valor. `app/chamados/models.py:1705,1719,3818,3834`, `app/templates/chamados/partials/modal_fio.html:205,212`.

`app/chamados/services/tarefa_do_dia.py:419,420` lê e recorta a coluna crua em `VistaNoHorizonte`. O resultado de `leitura` nessa trilha chega a `_pela_decisao`, que usa `validada_em`. `app/chamados/services/tarefa_do_dia.py:163,429,474`.

## 20/11 14:08 · sessao 2f91232fad22 · E4C-F5-CONSTROI · sonda · origem=Code

```python
# Corpo para --command, a partir de app/:
# python -B manage.py tenant_command shell --schema=juliani \
#     --settings=config.settings.sombra --verbosity=0 --command '<corpo>'
# bin/sonda_leitura.sh:94  bin/sonda_leitura.sh:239
#
# A guarda abaixo roda dentro do shell. O arranque padrão dos settings
# prepara /app/logs e configura handlers de arquivo.
# app/config/settings/sombra.py:27  app/config/settings/saas.py:118
# app/config/settings/saas.py:136  app/config/settings/saas.py:160

import json
from collections import Counter, defaultdict
from contextlib import closing

from django.conf import settings
from django.db import connection, transaction
from django.db.models import F, Prefetch

FORMAS = ("forma_1", "forma_2", "forma_3")
CAMPOS = ("lidas", "coerentes", *FORMAS, "excecoes")


def emitir(**dados):
    print(json.dumps(dados, sort_keys=True), flush=True)


def contar(empresas, exemplos, erros):
    from chamados.models import DecisaoPergunta, PerguntaDisputa

    # Campos usados pela leitura real e pelo prefetch reverso.
    # app/chamados/models.py:4191  app/chamados/models.py:4276
    # app/chamados/models.py:4317  app/chamados/models.py:4333
    decisoes = DecisaoPergunta.objects.using(connection.alias).only(
        "pergunta_id", "ato", "ato_em", "ato_texto", "resposta_mantem_validacao",
    )
    perguntas = (
        PerguntaDisputa.objects.using(connection.alias)
        .annotate(empresa_pk=F("colaborador__empresa_id"))
        .only("respondida_em", "validada_em", "validada_por", "resposta_colab")
        .order_by("pk")
        .prefetch_related(Prefetch("decisoes", queryset=decisoes))
    )

    # O iterator carrega as decisões por lote.
    # app/chamados/services/vigia_texto_resposta.py:14
    with closing(perguntas.iterator(chunk_size=1000)) as linhas:
        for pergunta in linhas:
            contagem = empresas[pergunta.empresa_pk]
            contagem["lidas"] += 1
            try:
                ato = DecisaoPergunta.leitura(pergunta).respondida_em
                coluna = pergunta.respondida_em
                if ato == coluna:
                    classe = "coerentes"
                elif coluna is None:
                    classe = "forma_1"
                elif ato is None:
                    classe = "forma_2"
                else:
                    classe = "forma_3"
            except Exception as erro:
                contagem["excecoes"] += 1
                erros[type(erro).__name__] += 1
                continue

            contagem[classe] += 1
            if classe in exemplos and len(exemplos[classe]) < 20:
                exemplos[classe].append(pergunta.pk)


def principal():
    # app/config/settings/sombra.py:13  app/config/settings/sombra.py:49
    # app/config/settings/sombra.py:60
    if (
        getattr(settings, "HASNER_SOMBRA", False) is not True
        or connection.settings_dict["NAME"] != "sombra"
        or getattr(connection, "schema_name", None) != "juliani"
        or connection.in_atomic_block
    ):
        emitir(status="INCONCLUSIVA")
        return 1

    empresas = defaultdict(Counter)
    exemplos = {forma: [] for forma in FORMAS}
    erros = Counter()
    guarda_ok = False
    erro_varredura = None
    alias = connection.alias

    try:
        if not connection.get_autocommit():
            emitir(status="INCONCLUSIVA")
            return 1

        with transaction.atomic(using=alias):
            with connection.cursor() as cursor:
                cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
                cursor.execute("SELECT pg_catalog.current_database(), pg_catalog.current_schema()")
                guarda_ok = cursor.fetchone() == ("sombra", "juliani")
            if guarda_ok:
                contar(empresas, exemplos, erros)
            transaction.set_rollback(True, using=alias)
    except Exception as erro:
        if not guarda_ok:
            emitir(status="INCONCLUSIVA", tipo_excecao=type(erro).__name__)
            return 1
        erro_varredura = type(erro).__name__

    if not guarda_ok:
        emitir(status="INCONCLUSIVA")
        return 1

    status = "INCONCLUSIVO" if erros or erro_varredura else "CONCLUSIVO"
    frota = Counter()
    for empresa_pk, contagem in sorted(empresas.items()):
        frota.update(contagem)
        emitir(
            escopo="empresa", empresa_pk=empresa_pk, status=status,
            **{campo: contagem[campo] for campo in CAMPOS},
        )

    emitir(
        escopo="frota", status=status,
        **{campo: frota[campo] for campo in CAMPOS},
        exemplos_pks=exemplos, excecoes_por_tipo=dict(erros),
        tipo_excecao_varredura=erro_varredura,
    )
    return int(status == "INCONCLUSIVO")


raise SystemExit(principal())
```

**Dono** vem de `PerguntaDisputa.colaborador_id`, derivado do colaborador da disputa no nascimento e preenchido pela mesma relação no legado. [app/chamados/models.py:2643] [app/chamados/models.py:3738] [app/chamados/migrations/0051_decisao_pergunta.py:22]  
**Empresa** vem de `Colaborador.empresa_id`, o PK da FK `Colaborador.empresa` do dono. [app/colaboradores/models.py:321]  
**[nao verificado]** Execução no shell da sombra, contagens reais na sombra ou em produção e identidade Git da cópia.

## 20/11 14:08 · sessao 38774a51bf64 · - · - · origem=Ronald (tmux)

Encontrei 37 sítios externos de leitura da coluna crua, contando dois predicados SQL distintos em `app/chamados/services/verbos.py:335,337` e agrupando os repasses com seu uso. O inventário com linhas e classificação foi enviado ao agente principal.

Para o instante do ato, confirmei três cadeias de exibição ou auditoria:

- `app/chamados/services/historico.py:106` lê por `getattr` e monta a cronologia serializada em `app/api/views_mensageria.py:2177,2199`.
- `app/chamados/models.py:1715` conserva o instante em `respondida_original_em`. A leitura de `app/chamados/models.py:3818–3834` entrega a foto ao template `app/templates/chamados/partials/modal_fio.html:212`.
- `app/chamados/services/materializacao.py:1091` serializa o instante na trilha de auditoria iniciada em `app/chamados/services/materializacao.py:1087`. Outros usos dessa entrada de auditoria ficaram [nao verificado].

A serialização de `app/api/views.py:2581` entrega o campo na rota do APK. O uso exato do campo pelo APK ficou [nao verificado], portanto indiquei DECIDE pela regra de dúvida. A seleção do subconjunto `respondidas` ocorre em `app/api/views.py:2590`.

## 20/11 14:08 · sessao 36e82553f207 · - · - · origem=Ronald (tmux)

N_DECIDE = 5
N_EXIBE = 3

Contagem por sítio distinto que lê o instante do ato. As 39 chamadas ao juiz `pergunta_respondida` e as quatro chamadas à projeção temporal foram rastreadas e encaminhadas ao agente principal. [app/chamados/juizes.py:1391] [app/chamados/juizes.py:1560]

| Classe | Função e sítio | Uso |
|---|---|---|
| DECIDE | `projecao_temporal_da_pergunta` [app/chamados/juizes.py:1391] | Corta pelo horizonte. A tupla alimenta a decisão sobre slot, vitalidade, absorção e o critério M1. [app/chamados/juizes.py:351] [app/chamados/juizes.py:353] [app/chamados/juizes.py:1409] [app/chamados/juizes.py:1428] [app/chamados/services/materializacao.py:1057] [app/chamados/services/materializacao.py:1060] [app/escala/services/cadastro_realidade.py:499] [app/escala/services/cadastro_realidade.py:500] |
| DECIDE | `respondida_sem_texto` [app/chamados/juizes.py:1541] | Recebe a composta por `espera_o_texto_do_colaborador`. [app/chamados/services/fio.py:189] [app/chamados/services/fio.py:190] |
| DECIDE | `pergunta_respondida` [app/chamados/juizes.py:1560] | Testa presença do instante com o texto, inclusive via alias, propriedade, tupla e `_replace`. [app/chamados/models.py:2024] [app/chamados/models.py:2026] [app/chamados/models.py:3816] [app/chamados/juizes.py:1428] [app/api/views.py:2590] |
| DECIDE | `espera_o_texto_do_colaborador` [app/chamados/services/fio.py:190] | Testa diretamente a ausência do instante antes de chamar o predicado de integridade. [app/chamados/services/fio.py:189] [app/chamados/services/fio.py:190] |
| DECIDE | `api_disputa_perguntas._item` [app/api/views.py:2581] | Serializa a composta recebida por parâmetro para o APK. Classificação conservadora. O uso efetivo do campo pelo APK está [nao verificado]. [app/api/views.py:2542] [app/api/views.py:2587] [app/api/views.py:2601] |
| EXIBE | `_eventos_dos_chamados` [app/chamados/services/historico.py:100] [app/chamados/services/historico.py:106] | Usa `getattr` para datar a linha do tempo exibida pela mensageria. [app/chamados/services/historico.py:114] [app/api/views_mensageria.py:2177] [app/api/views_mensageria.py:2199] |
| EXIBE | `ChamadoColaborador.ressuscitar_perguntas` [app/chamados/models.py:1715] | Preserva a data anterior na foto da reabertura, exibida no modal. [app/chamados/models.py:3818] [app/templates/chamados/partials/modal_fio.html:212] |
| EXIBE | `_materializar_ausencia_de_resposta` [app/chamados/services/materializacao.py:1091] | Registra o instante como detalhe da auditoria da tentativa. [app/chamados/services/materializacao.py:1087] [app/core/observ.py:134] [app/core/observ.py:136] |

`composta` copia o instante de `leitura`, ponto de repasse que antecede os usos acima. `_pela_decisao` recebe `LeituraDoAto` e consulta apenas `validada_em`. [app/chamados/models.py:4360] [app/chamados/models.py:4361] [app/chamados/services/tarefa_do_dia.py:429] [app/chamados/services/tarefa_do_dia.py:163]

## 20/11 14:06 · sessao d1ca218ae110 · E4C-F5-CONSTROI · leitura · origem=Code

# E4C-F5-CONSTROI — leitura dos oito sítios

A guarda descrita ainda está ausente. Hoje, `carimbar` pode preencher `validada_em` em memória, consultar o C6 e salvar sem comparar o par ato × coluna. A comparação que já existe na passagem da ausência acontece **depois** de `carimbar` devolver `True`, dentro do savepoint da irmã. `chamados/models.py:3323–3377`, `chamados/services/materializacao.py:1052–1084`.

A leitura nova cobriu o que os mapas deixavam em aberto: relógio e argumentos de cada chamada, ordem entre carga e trava, tratamento do `False`, entradas do passivo, elegibilidade do mesmo PK depois da recusa e selos do S14. As referências abaixo são relativas a `app/`, com o nome do arquivo abreviado nas tabelas. A identidade Git da cópia, as ocorrências em produção e a execução dos testes nesta sessão estão **[nao verificado]**. [nao verificado]

## P1–P3 — chamadas, alcance da guarda e continuação

**Como a guarda seria atingida.** `resposta='validar'` só preenche a coluna quando há texto e a coluna está vazia. A composta inclui uma validação sintética no instante da coluna quando falta uma linha correspondente. Um ato posterior que limpa a validação pode apagar essa validação na composta. Por isso, ausência de uma linha `VALIDACAO` por si só é insuficiente para disparar a guarda. Com `None` ou `'responder'`, a coluna só pode chegar preenchida de antes. O par visado é `(False, True)`, na ordem ato, coluna. `models.py:3344–3357`, `models.py:4276–4294`, `models.py:4302–4351`, `models.py:4364–4371`.

Nos sítios S143, o wrapper toma `SELECT FOR UPDATE` do **Colaborador**, das **disputas abertas** dele e dos **chamados vivos**, pais dessas disputas e chamado recebido. O wrapper não passa PKs de **Pergunta** à trava. A carga das duas listas de perguntas começa depois dessa trava. `disputa_emissao.py:1924–1932`, `disputa_emissao.py:1963–1999`, `models.py:2874–2904`.

| Sítio | P1 — gatilho e argumentos | P1 — instância no ponto da chamada | P1 — `False` hoje e o que fica | P2 — estado que chega à guarda | P3 — continuação no mesmo PK |
|---|---|---|---|---|---|
| `models.py:3532` | Cron `consumir_eventos_celula` → comando → consumidor → `encerrar_orfas_pela_lampada` → `encerrar_fantasma`. Via `fantasma`, usuário do sistema, `quando` omitido → `now()` na porta, `resposta=None`. `config/crons.py:394`, `management/commands/consumir_eventos_celula.py:17–35`, `reconciliador.py:1427–1432`, `reconciliador.py:1188–1203`, `models.py:3524–3532`, `models.py:3344`. | Vem de `_orfas_abertas`, com `select_related` e prefetch de decisões. A carga antecede o `atomic` do carimbo, que não relê nem toma `SELECT FOR UPDATE` da pergunta, disputa ou colaborador. O comando usa advisory lock por schema. `reconciliador.py:1096–1106`, `reconciliador.py:1188–1203`, `reconciliador.py:1278–1297`. | Pula a órfã sem trilha de fantasma, sem incremento e sem colocá-la entre as disputas a fechar. O `False` não adia nem conta como falha. Sem outra falha ou outro caso de teto no lote, os eventos recebem `processado_em`. `reconciliador.py:1190–1212`, `reconciliador.py:1427–1459`. | **Não.** A instância selecionada tem `validada_em=NULL` e nada no caminho atribui o campo. `None` também não o preenche. Isso vale mesmo que a coluna no banco possa mudar depois do `SELECT`. `reconciliador.py:1100–1106`, `reconciliador.py:1188–1202`, `models.py:3532`, `models.py:3350–3357`. | Guarda inalcançável nesse sítio. Para o `False` atual, só outro evento volta a julgar a mesma pergunta se ela ainda passar pelos filtros. A repetição do cron sobre aquele evento processado não a recupera. `reconciliador.py:1096–1106`, `reconciliador.py:1396–1399`, `reconciliador.py:1446–1455`. |
| `reconciliador.py:408` | `post_save` de `Batida`, ou filho sai de `VIVOS` e acorda pai no `on_commit` → `reconciliar_chamados_colab` → órfãs reatáveis → `reatar_pergunta_orfa` → sem destino derivado e sem histórico elegível → `lavrar_sem_turno_aberto`. A emissão também chama `reatar` para pergunta recém-criada sem destino. Via `sem_turno_aberto`, usuário do sistema, `quando` omitido → `now()`, `resposta=None`. `ponto/signals.py:206–215`, `chamados/signals.py:351–371`, `reconciliador.py:1486–1487`, `reconciliador.py:1573–1581`, `reconciliador.py:1610–1677`, `disputa_emissao.py:453–474`, `reconciliador.py:403–409`, `models.py:3344`. | No ramo de reata em lote, é a instância do queryset sem destino e sem materialização, com prefetch de decisões, sem filtro de coluna validada e sem releitura ou trava própria antes do carimbo. Na emissão é a instância devolvida pelo nascimento. A trava herdada pode variar com o gatilho. `reconciliador.py:1553–1581`, `reconciliador.py:363–408`, `disputa_emissao.py:453–474`, `models.py:3229–3246`. | `lavrar_sem_turno_aberto` devolve `False` sem fechar a disputa. `reatar` tenta o c2 e pode gravar a reemissão e a costura desse PK, se o c2 passar. Senão devolve `False`, sem incremento de reatadas. `reconciliador.py:408–417`, `reconciliador.py:1673–1678`, `reconciliador.py:985–1010`, `reconciliador.py:1578–1584`. | **Só divergência de entrada.** C1 exige motivo do grupo, sem resposta composta, sem carimbo, sem turno aberto e C6 morto. Uma leitura concorrente é possível por inspeção: se o queryset ler uma órfã ainda validada e sem carimbo e o prefetch ler a `REABERTURA` que acabou de limpar resposta e validação, a instância pode ter coluna velha e composta vazia. Uma nova `RESPOSTA` completa faria o c1 pular. A ocorrência dessa corrida e a origem de um B já persistido nesse recorte estão **[nao verificado]**. `reconciliador.py:363–408`, `reconciliador.py:1553–1581`, `models.py:2421–2424`, `disputa_emissao.py:1615–1633`, `disputa_emissao.py:1649–1669`, `models.py:4333–4351`. | No caso da releitura separada acima, a próxima batida ou saída de filho lê a coluna já limpa e pode lavrar o mesmo PK, se continuar sem destino, sem histórico elegível, sem resposta, sem turno aberto e morto pelo C6. O c2 pode agir já nesta passada se houver destino vivo elegível ou se puder emitir e costurar. Para pergunta muda que continua morta, **sem porta humana direta** de resposta, validação, recusa ou correção. `reconciliador.py:363–408`, `reconciliador.py:862–1010`, `reconciliador.py:1673–1678`, `views.py:1238–1246`, `validacao.py:698–704`, `validacao.py:915–923`, `juizes.py:1762–1767`. |
| `reconciliador.py:646` | `post_save` de `Batida` ou `on_commit` do filho → `reconciliar_chamados_colab`, e cron do outbox → `consumir_eventos_celula`; ambos chamam `retratar_perguntas_absolvidas_por_celula`. Via `celula_absolveu`, sistema, `quando` omitido → `now()`, `resposta=None`. `ponto/signals.py:206–215`, `chamados/signals.py:351–371`, `config/crons.py:394`, `reconciliador.py:1417–1419`, `reconciliador.py:1486`, `reconciliador.py:635–646`, `models.py:3344`. | Queryset de célula julgada que não acusa, sem materialização e com coluna de validação nula. Instância e prefetch passam pelo C6 e pela tranca do dia **antes da trava local do Colaborador**. O laço toma essa trava e usa a mesma instância, sem `SELECT FOR UPDATE` da Pergunta. `reconciliador.py:643–646`, `reconciliador.py:678–724`, `models.py:2830–2836`. | `continue`, sem registro da recusa, sem contagem e sem incluir essa disputa por essa pergunta. No outbox o `False` não alimenta `falhas` nem `adiar`, e o lote pode ser marcado processado. `reconciliador.py:646–668`, `reconciliador.py:1419–1432`, `reconciliador.py:1446–1459`. | **Não.** O filtro SQL dá coluna nula à instância e o chamador não a repõe. A decisão prefetched pode envelhecer, mas `resposta=None` preserva a coluna nula da instância. `reconciliador.py:678–724`, `reconciliador.py:643–646`, `models.py:3350–3357`. | Guarda inalcançável nesse sítio. Um novo gatilho pode alcançar um `False` atual se o PK continuar sem coluna e carimbo e a célula e os juízes voltarem a autorizá-lo. `reconciliador.py:678–724`. |
| `disputa_emissao.py:2043` | S143: save de status de chamado encerrado, inclusive via S14 → receiver → resolver → laço direto. O mesmo resolver recebe retratação, beco, reata com histórico e comando `reconciliar_fantasmas --apply`. O cron de fantasmas está configurado com `--alarme`. Passa via do fato, autor do resolver, `_tz.now()` por pergunta, `'validar'` e lavra. `signals.py:91–119`, `materializacao.py:1100–1102`, `reconciliador.py:573–587`, `reconciliador.py:1699–1705`, `becos.py:53–61`, `reconciliar_fantasmas.py:148–153`, `reconciliar_fantasmas.py:281–291`, `config/crons.py:392`, `disputa_emissao.py:2043–2047`. | Carga fresca sob as travas S143, com destino igual ao chamado, sem materialização, dona aberta e prefetch de decisões. A lista é materializada **antes** do primeiro laço que pode reabrir credores, e o laço de carimbo não relê a instância. `disputa_emissao.py:1924–1926`, `disputa_emissao.py:1963–1967`, `disputa_emissao.py:1996–2012`. | Pula sem carimbo, lavra ou incremento. O chamado já salvo não é desfeito pelo `False`. O HX ainda pode reabrir pai para resposta sem consumo, e o laço sem credor e a cauda de fechamento continuam. `models.py:292–308`, `signals.py:112–119`, `disputa_emissao.py:2043–2070`, `disputa_emissao.py:2083–2151`. | **Coluna nova:** exige texto e um ato de limpeza que vença a validação sintética datada pelo `now()` do laço. Esse relógio vem depois da carga sob a trava. Um escritor que produza esse ordenamento no gatilho real está **[nao verificado]**. **Coluna anterior:** a seleção admite B, mas o PK tem de estar fora de voo, morto pelo C6, fora da tranca do dia e, se respondido pela composta, aprovado pelo lastro. Origem de B que chegue ali **[nao verificado]**. `disputa_emissao.py:1963–1967`, `disputa_emissao.py:2012–2045`, `models.py:4333–4351`. | Se permanecer sem carimbo e com resposta composta sem validação, validar ou recusar pelo chamado que lista são portas existentes com dona aberta. Correção de questionário também alcança resposta elegível. HX só reabre pai existente, fora de `VIVOS`, se houver resposta sem consumo e `reabrir()` aceitar. O segundo laço pode pegar o PK sem credor se passar pelos filtros dele. Nenhuma nova passagem é enfileirada pelo `False`. `views.py:483–518`, `juizes.py:1762–1767`, `disputa_emissao.py:2060–2070`, `disputa_emissao.py:2083–2118`. |
| `disputa_emissao.py:2117` | Mesmos gatilhos do S143. Segundo laço varre também outras disputas abertas do colaborador. Usa via do pai ou `chamado_encerrado`, um `_tz.now()` por disputa, `resposta='responder'`. `disputa_emissao.py:1910–1932`, `disputa_emissao.py:2083–2117`. | Carrega as perguntas da disputa **depois** do primeiro laço e do HX, ainda sob a trava do Colaborador. Materializa novo `list(...prefetch_related('decisoes'))`, sem trava específica da Pergunta. Usa a lista no restante desse laço. `disputa_emissao.py:2060–2070`, `disputa_emissao.py:2083–2105`. | `continue`, sem `fecho_sem_credor` e sem carimbo. A cauda relê a disputa e ainda pode fechá-la se não houver pergunta em voo e `pode_fechar()` liberar. Esse laço também não incrementa `n_pq` quando tem sucesso. `disputa_emissao.py:2046–2047`, `disputa_emissao.py:2117–2151`. | **Só divergência de entrada.** A guarda de composta nula em `_abertas` **admite** `(False, True)`. Exige também pai e destinos fora de `VIVOS`, ausência de carimbo e de voo, e C6 morto. Escritor que entregue B a essa carga **[nao verificado]**. `disputa_emissao.py:2083–2106`, `disputa_emissao.py:2117`, `models.py:3350–3357`. | Com resposta composta e dona aberta, validar, recusar ou corrigir pelo chamado que efetivamente liste. Sem pai nem destino, essas rotas da mesa não têm chamado de entrada. C6 morto também barra nova resposta do titular. Nesse estado, **sem porta humana direta dessas**. Outro fechamento pode refazer a varredura se a dona continuar aberta, mas a mesma divergência volta a recusar. `fio.py:97–117`, `fio.py:634–693`, `views.py:1238–1246`, `disputa_emissao.py:2083–2118`. |
| `materializacao.py:1067` | POST de resposta no PWA/API → `responder_pergunta_disputa_core` → `FALTA_DIA` ou `ATESTADO` → `_materializar_ausencia_de_resposta`. Passa o usuário da resposta, `quando=agora` da passagem, comum ao grupo e colhido antes da trava por irmã, `'validar'`, via `declaracao` ou `ausencia_dia`. `views.py:1228–1278`, `api/views.py:2609–2653`, `disputa_emissao.py:1556–1561`, `materializacao.py:1036–1067`, `materializacao.py:1117–1120`. | A lista inicial só ordena as candidatas e identifica âncoras. Por irmã, novo `atomic`, trava da linha do **Colaborador**, nova busca por PK no mesmo dia/disputa/tipo e prefetch de decisões **executado depois da trava**. Sem `SELECT FOR UPDATE` de Pergunta. S14 volta à instância da foto inicial para o destino. `materializacao.py:1037–1056`, `materializacao.py:1100–1102`, `models.py:2830–2836`. | Acrescenta PK a `recusadas`, tenta gravar `irma_viva_na_ausencia` com `motivo='c6'` e não registra `ABSORCAO` daquela tentativa. A ausência já foi criada ou retornada e a resposta do escape já foi gravada. S14 ainda pode fechar o destino e acordar S143 no mesmo PK. O core descarta a lista devolvida. `disputa_emissao.py:1527–1532`, `disputa_emissao.py:1556–1561`, `materializacao.py:1027–1036`, `materializacao.py:1067–1107`, `signals.py:91–119`. | **Sim, coluna nova.** Resposta anterior a T1 preservada por `DEVOLUCAO` em T2, com T2 entre o relógio da passagem e a trava da irmã. A devolução limpa a validação e conserva a resposta. A tentativa em T1 repõe a coluna, e a composta lê a devolução posterior. O teste `test_RED_devolucao_t2_desfaz_carimbo_aceito_pelo_c6` fixa esse caso e hoje espera rollback pela comparação local. Uma **resposta completa** posterior a T1 é barrada *antes* da porta. **Coluna anterior:** o ramo também admite B se passar pela releitura, pela guarda temporal e pela ausência de carimbo. Origem de B já persistido **[nao verificado]**. `validacao.py:269–296`, `materializacao.py:1053–1073`, `test_e4c_f5_a1c_passagem_da_ausencia.py:85–104`, `test_e4c_f5_a1c_passagem_da_ausencia.py:429–451`. | Para a irmã do caso T2, validação real da resposta, recusa ou correção podem agir se a dona estiver aberta e o caminho escolhido tiver acesso ao PK. Nova resposta do titular exige C6 vivo. Outra resposta de escape no grupo pode refazer a passagem com relógio novo se a irmã ainda estiver sem carimbo. S14 pode alcançar o S143 imediatamente. A rejeição da ausência só devolve o PK que o leitor reencontra como **absorvido**, com carimbo e grupo, de modo que a irmã recusada e sem carimbo fica fora. `views.py:483–518`, `views.py:1238–1246`, `validacao.py:698–704`, `materializacao.py:1037–1067`, `materializacao.py:1100–1107`, `materializacao.py:1140–1156`, `materializacao.py:1176–1203`. |
| `materializacao.py:2004` | Validação individual → `_validar_o_par` ou chamada direta; regularização na view e no fio → laço das validadas; backfill → mesmo laço; plantio da irmã de intervalo → chamada interna; comando do passivo → unidade. Passa `quando=now()` **depois** do plantio/reuso e do lastro, `'validar'`, lavra e via por declaração/fato. `validacao.py:645–660`, `validacao.py:760–803`, `views.py:466–475`, `acoes_disputa.py:143`, `fio.py:714–728`, `backfill_fio_morto_e4b.py:153–156`, `materializacao.py:1754–1767`, `passivo_validado.py:529–537`, `materializacao.py:1945–2008`. | Laço normal: trava C → D → Ch → P, incluindo PKs das metades conhecidas, faz novo `get` e exige composta validada. Validação direta: `get` e checagem antes da trava C/D/Ch, usa a mesma instância, mas grava coluna e linha `VALIDACAO` do alvo no ato. A irmã é buscada separadamente e exigida validada pela composta ou validada pelo par. Passivo: instância anterior à trava C/D/Ch/P, com refresh só dos três campos de materialização. Nenhuma dessas cargas do alvo usa prefetch de decisões. `materializacao.py:2071–2089`, `materializacao.py:2128–2149`, `validacao.py:693–704`, `validacao.py:752–792`, `validacao.py:161–175`, `validacao.py:648–658`, `materializacao.py:139–150`, `materializacao.py:193–202`, `vigia_carimbo.py:97–105`, `passivo_validado.py:373–381`, `passivo_validado.py:529–537`. | Levanta `_Reprovada` dentro do savepoint. Desfaz batida nova, vínculo de reuso e demais escritas desse bloco. Fora dele acrescenta erro e tenta persistir `recusa_motivo` e nota no destino. O par tem rollback maior que pode desfazer também a recusa da meia tentativa. Na validação individual de hora, o chamador pode devolver a validação e gravar `DEVOLUCAO`. No passivo, a unidade falsa entra no balde de recusa. `materializacao.py:1560–1594`, `materializacao.py:2004–2032`, `materializacao.py:1706–1730`, `materializacao.py:1754–1780`, `validacao.py:653–671`, `validacao.py:786–803`, `passivo_validado.py:537–545`. | **Sim, coluna anterior na instância do passivo.** O classificador lê coluna preenchida. Se `devolver_validacao_recusada` limpar a coluna e gravar `DEVOLUCAO` antes da trava do lote, o refresh parcial conserva a coluna velha em memória e a leitura dos atos pode ver a devolução. Ela conserva a resposta, então a anuência pode passar. Ainda são necessários slot, plantio/reuso e lastro. No laço normal, B encontrado na releitura é pulado. Na validação direta, a linha recém-gravada valida o alvo. **Coluna nova que resulte em B**, inclusive numa irmã validada só pelo ato, exige limpeza que vença o novo relógio. Caminho real desse ordenamento **[nao verificado]**. `vigia_carimbo.py:97–116`, `passivo_validado.py:327–379`, `passivo_validado.py:529–537`, `validacao.py:269–296`, `materializacao.py:1321–1330`, `materializacao.py:1959–1971`, `materializacao.py:2141–2149`, `validacao.py:168–175`. | No caso do passivo com devolução concorrente, o banco já está sem validação. Com resposta composta, a validação individual pode assinar de novo e plantar/reusar se os vetos passarem. Recusa ou correção também podem devolver o PK quando elegível. Repetir o passivo só seleciona o PK se houver coluna preenchida e classe elegível de novo. A devolução automática pertence ao caminho de validação individual de hora, e exige recusa escrita e ausência de carimbo. `validacao.py:269–296`, `validacao.py:698–728`, `validacao.py:752–803`, `validacao.py:903–944`, `juizes.py:1762–1767`, `vigia_carimbo.py:97–116`. |
| `materializacao.py:2204` | Save de disputa com `fechada_em` → receiver S144 → `materializar_pendentes_aprovadas`. `fechar()` é um gatilho. O receiver também aceita save de campo de uma dona já fechada, como complemento de motivos no repasse. Passa `fechada_por` ou sistema, `quando=now()` do lote, `resposta=None`, via `juiz_aprovou`. `models.py:2095–2122`, `signals.py:149–167`, `disputa_emissao.py:1065–1099`, `materializacao.py:2186–2205`. | Query nova da Pergunta sem materialização, sem prefetch de decisões e sem `SELECT FOR UPDATE` próprio. No `fechar()` herda Colaborador, disputas e chamados travados pelo fechamento. No repasse, herda as linhas do conjunto C/D/Ch/P que o repasse travou. O lastro lê no laço antes do carimbo. `models.py:2090–2097`, `disputa_emissao.py:1017–1026`, `disputa_emissao.py:1073`, `materializacao.py:2191–2204`. | Não acrescenta PK a `feitas`, segue o laço. Nenhuma batida é plantada por S144. O `False` não desfaz a disputa fechada, e o receiver descarta a lista. O fechamento do pai passa por receiver próprio. `materializacao.py:2190–2206`, `signals.py:19–47`, `signals.py:149–167`, `models.py:2118–2127`. | **Só divergência de entrada.** Filtra materialização, voo e lastro. Nenhum desses filtros exige igualdade do par. `escolha` sem carimbo não passa no lastro. Para hora/data_hora há ramos de batida e de destino encerrado. Escritor que entregue B ao S144 real **[nao verificado]**. `materializacao.py:2191–2204`, `juizes.py:711–766`, `juizes.py:784–802`, `models.py:3350–3357`. | Dona fechada: validar pelo fio exige PK listado, C6 vivo, resposta composta e ausência de validação composta. Nova resposta também exige C6 vivo. Recusa e correção pelo fio exigem dona aberta. Reabertura admin exige pai selecionável, perfil, motivo, justificativa e teto, e só então uma ação posterior

[truncado em 20000 de 34009 caracteres]

## 20/11 14:05 · sessao 6d8585e3c201 · CADEIA-20-PASSO-1 · leitura · origem=Code

MAPAS JA FEITOS CONSULTADOS: 03_chamado_encerra.md, 06_pergunta_fio.md

- `03_chamado_encerra.md` localizou a porta canônica, a guarda que devolve `False` e o teste da trilha de recusa. `app/chamados/models.py:422–443,654–705` `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82–93`
- `06_pergunta_fio.md` localizou a leitura de vitalidade por C6, a porta de reapontamento para H4/H09 e o descarimbo da fábrica que pode ocorrer com lista de motivos vazia. `app/chamados/juizes.py:866–898,1510–1519` `app/chamados/services/fio.py:956–984` `app/chamados/services/fabrica_da_celula.py:206–220`

## PAPEL E ESCOPO DE ESCRITA

Construtor da próxima volta de CADEIA-20-PASSO-1, na cópia indicada pelo hash `a4a9b6bfe5f7234deeb14ce42f4b1d5535a211a6`. Execute A1 e B1 do CADEIA-20-COBERTURA-V1, conforme RESPOSTAS-2011-1135 item 3 e RESPOSTAS-2011-1355 item 1. A F5 tem prioridade na vaga. Antes da suíte, a sessão refaz a cópia sobre o main com F5 e o verificador compara lado a lado as duas funções compartilhadas. Conflito de texto ou de sentido dá `PAREI` com o trecho. Pouso da CADEIA só depois da F5 no ar. `app/docs/RELATO.md:100` `app/docs/LEIS.md:147` `app/ponto/services/cartorio.py:131–158,1264–1267,1798–1808`

- `app/ponto/services/cartorio.py` é o único arquivo de produção autorizado. O escopo é a projeção comum, A1, B1 e as guardas necessárias nas portas atingidas dentro desse arquivo. `app/ponto/services/cartorio.py:493–578,1059–1060,1123–1233,1527–1661,1726–1738,2012–2037`
- `app/escala/tests/test_cadeia20_passo1_cobertura.py` é o único arquivo de teste autorizado. Crie-o no app `escala`, seguindo a convenção `app/escala/tests/test_cadeia20_*.py` dos testes da obra. Registre caso, resposta da regra e mutante antes das asserções. `app/escala/tests/test_cadeia20_volta3.py:89–106,142–158` `app/escala/tests/test_cadeia20_lavra.py:308–321` `app/escala/tests/test_cadeia20_volta3_mutantes.py:101–119`
- Ficam fora da lista de escrita `app/core/tests/test_medida_turno_do_par.py`, `app/chamados/juizes.py`, `app/core/juizes.py`, todos os `models.py`, os testes existentes, as portas externas ao cartório e qualquer outro arquivo tocado pela F5. Portar para o arquivo novo somente o que for próprio da CADEIA. Cabe à sessão excluir da cópia refeita o delta anterior da CADEIA na catraca de `core`. O diff anexado identifica somente `cartorio.py`. A lista integral dos arquivos da F5 e o delta histórico de `core` ficam [nao verificado]. `app/core/tests/test_medida_turno_do_par.py:314–319,558–562` `app/chamados/juizes.py:1510–1519` `app/core/juizes.py:333–342` `f5_cartorio.diff:1–5`

## PRONTO 1 ANTES DO CÓDIGO

A matriz aceita por RESPOSTAS-2011-1355 fixa **18 recusas**, todas `pergunta_viva_na_competencia_aberta`. Os pares célula/pergunta são **138359/54568, 129943/53186 e 128355/58553**. Cada par aparece em três etapas de P1, no corpo inicial (`:1734`), no fecho adiado (`:1141`) e na segunda passada (`:1695`). As três etapas se repetem em `encerrado` às `06:27−03:00` e no grupo `cruza_e_em_curso` às `00:30−03:00`, em `2026-11-20`. Os três números de linha ainda identificam as chamadas na cópia. O motivo literal é registrado quando há viva ancorada sem reata possível. A decisão devolve `None` e `reconciliar()` devolve `False`. A condição de entrada do item 3 está satisfeita para essas recusas. `cadeia20_matriz_p1_clausula.out:3–20` `app/ponto/services/cartorio.py:1141,1152–1157,1695,1734` `app/chamados/models.py:386–405,422–443,654–668,684–705`

Use a matriz como prova dessas recusas nos pontos e instantes registrados. Continue exigindo, para as nove células, escritor, etapa, cinco presenças antes e depois, primeira leitura que cobre o delta, via efetiva, fotos P1–P3 por braço e descartes. A matriz de 18 linhas não mede fechamento bem-sucedido, perda efetiva de fio, efeito de A1/B1, projeção B1, P2/P3, descarte zero ou o percurso separado de `cruza`. O C1 do 0215, conforme relatado no V1, atribui 138541 à fábrica e 142995/143945 ao repasse de órfãos do emissor. As linhas da matriz de recusas não demonstram essas três atribuições. Se uma escrita relevante ficar fora das duas vias, ou se aparecer divergência entre a projeção e o ato real, aplique C3 e publique `PAREI` desta obra com célula, etapa e números. `cadeia20_matriz_p1_clausula.out:3–20` `app/ponto/services/cartorio.py:1123–1184,1209–1233,1952–1955` `app/ponto/management/commands/detectar_ausencias.py:788–807` `app/chamados/services/disputa_emissao.py:1258–1284`

## PROJEÇÃO COMUM

Desde a competência **2026-10**, use `versao_para` e a janela da empresa. Na impressão, represente as cinco presenças da mesma leitura que alimenta o julgamento: chamado de batida total, vivo, vivo que protesta, vivo com fio C6 e vivo com fio C6 que protesta. Total inclui terminais. Fio é presença em um chamado vivo segundo C6. A interseção é sobre o mesmo chamado. Conserve a diferença entre nenhum vivo e vivo sem fio, a regra de emissão por presença de vivo, os demais insumos da impressão, v5 e a lavra de `razoes_orfas`, o ramo `nunca_bateu`/sem adesão e a prioridade do admin. Até 09, conserve os bytes legados para os mesmos insumos e o caminho legado diante de fato novo em aberta. A cerca `_K6` de MUDA 4 vale antes das portas atingidas, inclusive em cascata, com a janela original de leitura preservada. `app/docs/LEIS.md:145` `app/ponto/services/cartorio.py:131–164,280–299,502–578,1268–1273,1334–1337,1399–1415,1527–1558,1605–1661,1889–1897,2012–2021` `app/ponto/portas/celula.py:970–987` `app/chamados/services/vigia_fio.py:786–808` `app/folha/export.py:556–594`

## CLASSE 1 — A1

Em `_julgar_colab_corpo`, o ponto de recomposição fica **depois do sucesso em `_rec` no corpo inicial e antes de seguir para ressuscitação e lavra**. O bloco de entrada é `:1726–1738`, o retorno confirmado da porta é `:1734–1735` e a ressuscitação começa em `:1739`. A ata candidata já está em `:1578–1583`, a classificação e o par em `:1605–1661`, o lote sem protesto em `:1683–1707` e a lavra/carimbo em `:2012–2037`. Esses pontos ainda correspondem às linhas citadas no V1. `app/ponto/services/cartorio.py:1264–1267,1578–1583,1605–1707,1726–1739,2012–2037`

Quando o fecho confirmado retirar a cobrança que motivou `COBRANCA_SEM_FURO` e restar candidato sem protesto, recarregue lista e fio atingidos para a mesma célula e janela. Recomponha impressão, códigos e par da primeira lavra de forma coerente, pelo classificador e pela porta existentes. Consulte C6 com o `agora` do ato sobre a cópia em memória da **célula referenciada pela pergunta**, com a ata e o par candidatos. Preserve a instância original e evite reutilizar relações ou resultado memoizado da foto anterior. Se a lavra candidata extinguir o fio do remanescente, encaminhe-o pelo lote adiado existente. `app/ponto/services/cartorio.py:1121–1127,1527–1547,1578–1583,1605–1661,1683–1686,1726–1738,2012–2037` `app/ponto/portas/celula.py:970–1006` `app/chamados/juizes.py:825–898,934–952,1418–1420,1450–1469` `app/chamados/models.py:1560–1576`

A ordem física é primeira lavra e carimbo no mesmo save, E2, fecho canônico do restante e segunda passada com o estado gravado. Preserve o `EventoCelula` desse save, o gate real, a trilha de cada recusa, a transação de fecho e rejeição de justificativas, a ordenação da fila e sua janela original. Use a segunda passada que já existe. A2, save adicional, carimbo movido para o fim e terceira passada ficam fora do aval. `app/ponto/services/cartorio.py:1123–1164,1691–1707,2012–2037` `app/ponto/portas/celula.py:957–966,1181–1202` `app/chamados/models.py:422–443,654–668`

## CLASSE 2 — B1

Em `julgar_colab`, tire as fotos por PK imediatamente antes e depois do laço de fábrica em `:1179–1184`. O ponto que recebe o gatilho adicional é a condição de `:1209`. A única releitura existente está em `:1220–1233`. As referências do V1 a esses blocos, à janela em `:1268–1273` e à contagem de emissão em `:2136–2146` conferem na cópia. `app/ponto/services/cartorio.py:1059–1060,1165–1184,1209–1233,1268–1273,2136–2146`

Em ato elegível à fábrica, compare as cinco presenças pelas mesmas leituras do julgamento e com dados frescos na segunda foto. Inclua células elegíveis da janela que podem ser atingidas por efeito em outro dia, inclusive célula pulada inicialmente. Conserve o gatilho por subida de `emitidos`. Um delta de projeção aciona a mesma releitura, uma vez, com `forcar=False`, janela original e guarda da impressão. Preserve a execução única da fábrica, o significado de `emitidos`, o acerto dos contadores e os vigias de descarte. Lista de motivos, contagem de perguntas, mudança de rótulo e ocorrência de escrita são insuficientes para decidir se a projeção mudou. Uma escrita anterior à primeira foto ou posterior à última leitura ainda precisa de via demonstrada pelo C3. `app/ponto/services/cartorio.py:1110–1123,1152–1184,1209–1233,1462–1466,1527–1558,1605–1617,1652–1653,1952–1955,2136–2146` `app/chamados/services/fabrica_da_celula.py:206–220` `app/ponto/management/commands/detectar_ausencias.py:788–807`

## RED DA VOLTA

**Classe 1.** Monte célula aberta no horizonte com os dois vivos da classe medida. Prove na fixture que um motiva `COBRANCA_SEM_FURO` e fecha no corpo inicial e que o outro fica sem protesto, com pergunta que C6 ainda lê viva sobre a foto inicial. Afirme a resposta de C6 na candidata e após o primeiro carimbo, a sequência carimbo → E2 → fecho → segunda passada, os dois terminais recarregados e a impressão final em dia sem rodada externa extra. Mate os mutantes que mantêm lista/fio antigos, deixam E2 antes do primeiro carimbo ou omitem a fila do fecho. No controle, use pergunta que C6 ainda dá por viva, preserve a resposta ou ato humano, verifique a recusa com trilha e o remanescente vivo. Esse controle mata o mutante que contorna o gate ou apaga o ato. `app/ponto/services/cartorio.py:1123–1164,1578–1583,1683–1707,1726–1738,2012–2037` `app/chamados/models.py:422–443` `app/ponto/portas/celula.py:1181–1202`

Já existem controles de dois candidatos com fio/sem fio e precedência C8, leitura da ata durante o ato, recusa com trilha e janela da segunda passada. Falta o caso integrado A1 com fecho confirmado em `COBRANCA_SEM_FURO`, recomposição e dois terminais, acompanhado do controle de recusa no mesmo caminho. `app/ponto/tests/test_fechar3_troca_escala_gate.py:168–208,216–250,382–396` `app/chamados/tests/test_o261e_f1_porta_encerramento.py:82–93` `app/escala/tests/test_cadeia20_lavra.py:308–349`

**Classe 2.** Em todos os casos prove a elegibilidade da fábrica, registre sua execução real e afirme que `emitidos` ficou constante.

- **F:0→1.** Comece com vivo sem fio e faça a fábrica estabelecer o primeiro fio C6, forma observada em 138541 segundo o V1. Exija uma releitura e impressão e palavra persistidas de acordo com a nova projeção ainda em P1. Faça o caso distinguir o delta de uma lista de motivos retornada, por exemplo com o caminho de descarimbo quando a fixture o sustentar. Mate o gatilho só por emissão e o gatilho só pela lista da fábrica. `app/ponto/services/cartorio.py:1179–1184,1209–1228,1527–1558` `app/chamados/services/fabrica_da_celula.py:206–220`
- **FP:0→1 com F já presente.** Use dois vivos, só um protesta. O primeiro fio fica no que não protesta e a etapa faz o que protesta ganhar fio. Prove que F continua presente, FP muda e a célula é relida uma vez no mesmo ato. Inclua a variante em que o PK atingido é de outro dia elegível da janela. Mate o mutante que perde a interseção ou fotografa só os PKs que chamaram a fábrica. O valor real de FP em 129476 fica [nao verificado]. `app/ponto/services/cartorio.py:1179–1225,1268–1273,1496–1501,1605–1617,1652–1653`
- **Dois controles de projeção igual.** Em um, outro chamado ganha fio e FP permanece igual. No outro, nasce pergunta adicional no mesmo chamado que já tem fio. Prove a escrita e as cinco presenças iguais, com delta zero de impressão e zero chamada à releitura pelo novo gatilho. Mate os mutantes que usam contagem exata de chamados com fio, cardinalidade de perguntas ou ocorrência de escrita como gatilho. 129490 é referência de investigação do V1 e a igualdade real de FP fica [nao verificado]. `app/ponto/services/cartorio.py:1179–1184,1209–1224,1527–1547,1605–1617,1652–1653` `app/chamados/services/fabrica_da_celula.py:206–220`

Já existem o controle O222 por emissão, com releitura e contadores, o teste de descarimbo apesar de lista vazia, os controles de fio vivo/morto e a relavra sem `forcar` quando o fio morre. Faltam os dois positivos B1 sem emissão e os dois negativos que afirmam projeção igual e zero releitura nova. `app/ponto/tests/test_bateria_lavratura.py:1181–1237` `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:92–115` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:132–164,315–354`

**R3 H1–H4, comuns à projeção.** Acrescente os quatro casos completos no arquivo novo.

- **H1.** Em célula passada, aberta e carimbada no horizonte, use único chamado `em_analise` com premissa regularizada e sem pergunta viva impeditiva. Faça POST real do gestor com `novo_status=resolvido`. Afirme chamada pelo sinal com `forcar=False` efetivo, perda do último vivo, impressão mudada antes do retorno e P2 em dia. Mata o mutante que retira a chamada do sinal. Há fixture de POST, sem a prova síncrona do carimbo. `app/chamados/views.py:547` `app/chamados/services/acoes_chamado.py:292–297,322–331` `app/chamados/signals.py:189–218` `app/ponto/services/cartorio.py:1552–1558,2174–2205` `app/chamados/tests/test_selo_acoes_orfas.py:45–48,85–92`
- **H2.** Provoque `analisar()` real na fábrica, `aberto → em_analise`, com as cinco presenças iguais. Afirme a transição, delta zero de impressão e zero reentrada pelo sinal. Mata a volta de `pk:status_local` na impressão do horizonte e a perda da guarda de reentrada. O teste unitário da impressão atual caracteriza a sensibilidade legada ao estado. `app/chamados/services/disputa_emissao.py:1180–1192,1258–1266` `app/chamados/signals.py:189–218` `app/ponto/services/cartorio.py:502–507,1097–1123` `app/ponto/tests/test_cartorio.py:26–32`
- **H3.** Valide pela porta humana a única pergunta viva, mantendo vivo o chamado. Afirme evento da pergunta, zero chamada direta ao cartório pelo sinal dela e, no próximo passe sem `forcar`, fio `1→0` com impressão e palavra atualizadas conforme os códigos. Mata a omissão do evento ou do fio na impressão. Há POST de validação e há relavra sem `forcar` por morte do fio em testes separados. `app/chamados/tests/test_guard_s116_detalhe_local.py:63–73` `app/ponto/tests/test_o261_cobrado_le_o_fio.py:164–202` `app/chamados/services/validacao.py:161–174` `app/chamados/signals.py:229–259` `app/ponto/services/cartorio.py:1541–1558`
- **H4.** Em dia sem códigos, fixe dois vivos e um só protestante. Reaponte a única pergunta viva do protestante para o outro pela porta real, sem outro fato. Afirme que F continua presente, FP cai, a impressão muda e `cobrado → concorde`. Mata a projeção que guarda fio global e perde a interseção. Já há teste da porta de reapontamento e da emissão de evento. Falta a combinação com julgamento, impressão e palavra. `app/chamados/services/fio.py:956–984` `app/chamados/tests/test_e4_c20.py:117–125` `app/ponto/services/cartorio.py:1605–1653`

**Controles preservados.** H09 e H10 usam o reapontamento de H4 em aberta e já carimbada. H09 prova pergunta ainda viva, `pk:status_local` e `fio:1` iguais, interseção alterada e foto persistida estável. H10 prova a mudança pelo formato novo e a estabilidade após P1. H09b usa em ambos os braços a mesma entrada, PKs, relógio e sinal real, `_K6=False` e `forcar=False`. Com `aberto → em_analise` e fio constante, demonstre que os bytes de entrada e o SHA-256 legado mudam dentro de cada braço. Compare **entre braços**, após transição, julgamento e fotos seguintes, os quatro campos persistidos `impressao`, `ata`, `veredito` e `julgada_em`, em UTF-8 canônico, com chaves da ata ordenadas, listas em ordem, `null` distinto de `[]` e timestamps no mesmo fuso e precisão. Preserve o golden isolado de 09 `c07c4d180f94f73a14c6139b181946fb50c1d19a08e4a069f9f91a9c0a30f6df`, o controle de regra 4 e a integração v5. O teste existente de volta 3 cobre v4→v5 e foto de 09 sem essa mudança derivada. Faltam H09/H10 e H09b completos, que devem falhar se o formato novo vazar para 09. `app/escala/tests/test_cadeia20_volta3.py:109–119,142–158` `app/ponto/tests/test_e3_isento_na_impressao.py:59–106` `app/ponto/services/cartorio.py:502–507,543–578,1541–1558,2174–2217`

E10 exige TXT vigente, insumo capaz de relavrar, foto intacta P1–P3 e zero chamadas às portas de escrita. I10 usa TXT invalidado sem substituta e deve alcançar o caminho de aberta. O mutante é ignorar `_K6` ou tratar TXT invalidado como entrega. As fixtures do K6 já existem. Falta o controle de três rodadas do cartório e de seus efeitos em cascata. Para 12711/08, registre a resposta de `_K6` e, se entregue, prove zero toque já em P1. R5 do relógio fica como requisito do V3, com a mesma pergunta viva às 10:00, validada às 10:01 e morta às 10:02, relógio ambiente às 10:02. O mutante ignora `agora`. A prova pela API `fio_vivo(..., agora=...)` depende de MUDA 2, fora desta escrita. `app/folha/tests/test_k6_competencia_entregue.py:30–85` `app/chamados/juizes.py:886–898,1382–1406,1510–1519` `app/ponto/services/cartorio.py:1123–1233` `app/ponto/portas/celula.py:1181–1202`

## CRUZAMENTO COM F5

- **`chamados_que_falam_de_batida`.** O hunk F5 é `−141,10 +141,9`. A troca do prefetch fica na antiga `:147`, nova `:146`, com comentário antigo em `:144–145`. O texto antigo confere na cópia. O ponto de contato é **a mesma leitura**, porém as chamadas novas de A1/B1 ficam longe dessas linhas, nos pontos de recomposição e de foto. Reuse o carregador para as leituras frescas e preserve `perguntas_que_resolvem__decisoes` na integração. Se o recorte exigir editar o carregador, revise o trecho como vizinho imediato do prefetch, com sobreposição caso a própria query seja alterada. `f5_cartorio.diff:5–17` `app/ponto/services/cartorio.py:131–158,1179–1209,1462–1466,1496–1501,1726–1739`
- **`_julgar_colab_corpo`.** O hunk F5 `−1686,7 +1685,8` casa pelo texto em `:1800–1806` na cópia. A troca do `return` antigo da F5 em `:1689` corresponde a `:1803` local. A F5 introduz a importação em sua `:1688` nova e consulta `DecisaoPergunta.composta(q)` em `:1689` nova. A1 fica em **outro trecho da mesma função**, antes da ressuscitação, cerca de 65–70 linhas acima, em `:1726–1739`. B1 fica longe, em `julgar_colab`. Preserve a leitura composta para os dois usos do helper de resposta e confira se a recomposição altera quais chamados chegam a esses usos. O número `:1686` do diff, nesta cópia, pertence ao enfileiramento de A1. `f5_cartorio.diff:18–27` `app/ponto/services/cartorio.py:1179–1233,1683–1686,1726–1739,1798–1828`

## SELO E LIMITES DA LEITURA

O aceite futuro conserva os seis itens do PRONTO, sem declarar pronto pela matriz de cláusulas. Para R1/R2, rode três passadas sem `--forcar` externo, empresas 2/3/4 nessa ordem. Exija oito impressões e palavras estáveis em P1=P2=P3 e 129134 estável em cada braço fixo. Em `cruza`, use 00:30/00:30/06:27 no dia proposto e confira a mudança do juiz e a cobrança no mesmo P3. Já há controle vizinho do turno em curso, da virada e do segundo julgamento pulado. Publique duração, consultas, releituras e os dois descartes. Preserve a prova integral FM/ED por empresa e competência 08–11, dinheiro/retensão/`DiaPago` nas abertas, (A) em P2/P3, 12711 em separado, contador de sobras e R5 da casa. Alcance de entregue ou movimento financeiro dá `PAREI`. `app/ponto/tests/test_celula_concorde_meio_turno.py:44–79` `app/ponto/services/cartorio.py:1038–1056,1152–1164,1209–1233,1244–1249` `app/ponto/management/commands/processar_cartorio.py:40–74` `app/ponto/turnos.py:2124–2134` `app/folha/models.py:75–104` `app/ponto/models.py:398–485` `app/ponto/services/fechamento.py:1046–1068,1191–1202` `bin/r5_idempotencia_frota.py:104–130` `CLAUDE.md:401–417`

[truncado em 20000 de 22725 caracteres]

## 20/11 14:04 · sessao 796da950f995 · E4C-F5-CONSTROI · leitura · origem=Code

## P1 — O que a sonda contou

Os **18 + 18 + 45 = 81** são chamadas a `core.observ.registrar_engolido()` durante três leituras da sonda. `ler()` tira uma fotografia do contador cumulativo do processo antes da leitura e outra depois. Cada chamada ao registrador soma **1**, inclusive quando recebe `exc=None` ou uma `Exception` construída para registrar uma divergência. O registrador chama `logger.error()` **depois** de incrementar o contador. A sonda mantém `logging.disable(logging.CRITICAL)`, e um `logger.error()` ou `logger.exception()` direto fica fora dessa contagem. [app/core/observ.py:57](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/core/observ.py:57) · `app/core/observ.py:73–91` · `app/chamados/services/materializacao.py:580–588` · `r.txt, bloco Python:71–90, 475–477` · `saida_sem_P.txt:28,36,38,53`

| Etapa | O que `qtd` mede |
|---|---|
| `vigia_ESTOQUE_engolidos` | **18** registros durante uma chamada ao censo da foto de estoque. `r.txt, bloco Python:177–185,249–254,439–440` · `saida_sem_P.txt:28–29` |
| `vigia_SIMULACAO_engolidos` | **18** registros durante outra chamada ao censo, depois da escrita simulada. A identidade das operações entre as duas chamadas é `[nao verificado]`. `r.txt, bloco Python:177–185,249–254,441–447` · `saida_sem_P.txt:35–37` |
| `modal_SIMULACAO_engolidos` | **45** registros durante um GET do modal de `col723`, incluindo a montagem e a renderização. A sonda usa cache por colaborador dentro de cada foto e desfaz o savepoint desse GET. `r.txt, bloco Python:71–90,188–194,220–225,261–264` · `saida_sem_P.txt:38` |

O `tipo: null` vem de `falha(etapa + "_engolidos", qtd=delta, **ids)`, que usa o `exc=None` padrão. Se uma exceção chega ao `except` de `ler()`, a sonda passa a exceção a `falha()` e emite outra linha, com o nome da etapa **sem** o sufixo `_engolidos`. O `Desfaz` intencional do modal tem tratamento próprio. As três linhas anexadas são do ramo do delta. `r.txt, bloco Python:65–90` · `saida_sem_P.txt:28,36,38,53`

## P2 — Chamadas e candidatos na árvore

**Os dois vigias percorrem a mesma entrada.** A sonda abre `tranca_em_lote()` e chama `censo_alcance()`. O contexto de tranca restaura o memo em `finally`. O censo chama `empresas_visiveis()`, `competencias_a_lavrar()` e `dias_que_seguram()` para formar o universo. Depois lê `alimentar_portas()`, `carga_da_tarefa()` e `porta_da_tarefa()`. Conforme o dia, também passa por `dono_fora_do_fio()`, `ListaDoAdmin` e `_no_app()`. `r.txt, bloco Python:177–185` · `app/chamados/juizes.py:1159–1170` · [app/chamados/services/vigia_fio.py:487](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/chamados/services/vigia_fio.py:487) · `app/chamados/services/vigia_fio.py:517–544,559–579,592–594,615–652`

- **Lista do admin.** `ListaDoAdmin._carregar()` captura falha na leitura da lista Cadastro × Realidade ou no preparo dos seus mapas e registra `fio_do_admin_lista`. O carregador marca a instância como carregada antes do `try`, de modo que esse ponto contribui com até **1** registro por censo da sonda. Dentro de `cadastro_realidade.lista()`, há também `logger.exception()` com continuação nas linhas **883, 893, 900, 907, 914, 921, 928, 935, 942, 949 e 956**. Esses logs diretos ficam fora do delta. [app/chamados/services/vigia_fio.py:183](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/chamados/services/vigia_fio.py:183) · `app/chamados/services/vigia_fio.py:579` · `app/escala/services/cadastro_realidade.py:878–956` · `app/core/observ.py:73–82`
- **C6 e seus leitores, candidatos nos dois vigias e no modal.** No vigia, `alimentar_portas()`, a carga da tarefa e o caminho do app chegam a `pergunta_viva()`. No modal, `montar_chamados_fio()` lê C6 nas disputas fechadas e em `p.viva`. Os pontos que registram e seguem são `lei_indisponivel` e `juiz_indisponivel` em `_pergunta_viva_calc()` e, nos auxiliares, `janela_viva_indisponivel`, `lei_indisponivel_na_via`, `batida_origem_indisponivel` e `lei_indisponivel_no_fallback`. Em `classificar_falta()`, os candidatos são `folga_nao_cobra`, `ausencia_cala` e `nucleo_indisponivel_na_lei`. `app/chamados/juizes.py:1656–1677` · `app/chamados/services/tarefa_do_dia.py:621–632` · `app/chamados/services/pergunta_no_app.py:45–65` · `app/chamados/services/fio.py:268–272,313–319` · `app/chamados/models.py:3619–3628` · `app/chamados/juizes.py:618–621,706–709,766–768,798–801,1479–1495` · `app/escala/servico_jornada.py:312–314,354–357,406–414`
- **Derivação de marco, candidata nos dois vigias e no modal.** O C6 pode chegar a `_slot_esperado()` e ao derivador `_ts_tipo_do_plantio()`. O modal também chega ao derivador pela leitura de `estado_aplicacao_pergunta()`. O derivador registra `marcos_da_regua` e `tipo_pelo_marco_ata` em `except`, e `tipo_marco_divergente` quando as duas fontes discordam. Esse último ramo incrementa o mesmo contador sem precisar que uma exceção seja levantada. `app/chamados/juizes.py:345–378,756–757` · `app/chamados/services/carga_pergunta_viva.py:113–125` · `app/chamados/services/fio.py:375–385` · `app/chamados/services/materializacao.py:514–518,564–588,651–669,884–911`

**O modal** chama `modal_fio_colaborador()` com um GET. A view verifica o gestor, marca notas como lidas, chama `montar_chamados_fio(..., com_explicacao=True)`, monta ficha, proposta, permissões e contadores, e renderiza `modal_fio.html` via `resposta_render()`. A sonda lê as URLs dos botões do HTML devolvido. `r.txt, bloco Python:188–194,220–225,261–264` · [app/chamados/views.py:869](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/chamados/views.py:869) · `app/chamados/views.py:877–914,917–956` · `app/core/respostas.py:74–86`

- **Três candidatos próprios de `montar_chamados_fio()`.** O `except` de `propostas_do_chamado()` registra `propostas_texto` **por card** e segue com lista vazia. O bloco de regularização O318 registra `o318_em_regularizacao` **por lote**. O bloco de composição dos rótulos e ações registra `desenlace` **por montagem** e segue com os rótulos de origem. Os três passam `None` ao registrador, dentro de um `except` ativo. [app/chamados/services/fio.py:325](/home/ronald/codex_ws/eb1e49c1ec1055e6bf43d7f67df63b17054b474e/app/chamados/services/fio.py:325) · `app/chamados/services/fio.py:285,406–430,432–459`
- **Outros pontos que seguem com fallback.** Os `except` do explicador, do marco previsto e do estado de aplicação em `fio.py` têm fallback próprio. O filtro `leitura_da_resposta` e `folha_julga_pontualidade()` também têm fallback. Esses `except` ficam fora do delta por si mesmos. A leitura do switch em `get_motor_cct()` tem `logger.exception()` direto, também fora do delta. No vigia, os avisos da oferta de **Resolver dia** são `logger.warning()` diretos. `app/chamados/services/fio.py:336–385` · `app/chamados/templatetags/chamados_tags.py:144–178` · `app/chamados/juizes.py:2004–2031` · `app/core/regua_cct.py:441–449` · `app/chamados/services/tarefa_do_dia.py:697–706` · `app/colaboradores/services/calendario.py:142–165` · `app/core/observ.py:73–82`

A saída traz o delta total por leitura. A origem efetiva, a distribuição por operação e o tipo e a mensagem das ocorrências dos **18, 18 e 45** ficam `[nao verificado]`. `r.txt, bloco Python:65–90` · `saida_sem_P.txt:28,36,38`

## P3 — Números que a rodada mediu

`S`, `S_EMP` e `T` usam a exigência por PK, K6, C20 e o par `(colaborador, dia do turno)` lidos pela própria sonda. A classe já está calculada quando o modal é consultado. `resumo()` agrega as classes e os pares sem ler os campos `vigia` e `botao`. `r.txt, bloco Python:311–377,380–408`

| Saída | Leitura independente dos três deltas |
|---|---|
| `FROTA` e `JANELA_SIMULACAO` | **4** empresas ativas. De **21/10 a 20/11**, as empresas **1, 2, 3 e 4** tinham respectivamente **69, 8.912, 1.953 e 338** perguntas na janela inicial. `r.txt, bloco Python:127–138,427–431` · `saida_sem_P.txt:27,30–33` |
| `UNIVERSO_SIMULACAO` | **115 chamados** com retorno elegível da porta e **336 PKs distintos** de perguntas elegíveis. A sonda consulta `perguntas_a_ressuscitar()` e valida os PKs contra a base da frota. `r.txt, bloco Python:140–151` · `app/chamados/models.py:1513–1519` · `saida_sem_P.txt:34` |
| `ESCRITA_SIMULADA` | **336** na soma dos retornos da porta, **336** marcas novas no instante fixado e **336** PKs distintos nessas marcas. A sonda confere se o retorno bate com o número de marcas e se os PKs marcados pertencem ao conjunto elegível. `r.txt, bloco Python:154–174` · `app/chamados/models.py:1694–1703` · `saida_sem_P.txt:35` |
| `S` e `T` estoque | **81 marcadas**, **81 fora de `silencio_decidido`**, **0** no universo de `T` e **0** no contador. As quatro classes têm **0 perguntas e 0 colaborador-dias** cada. `trancada_sem_txt` e `trancada_sem_txt_confirmada` são **0**. A igualdade `0 = 0` é a conferência interna da soma das classes. `r.txt, bloco Python:115–124,318–345,380–408` · `saida_sem_P.txt:40–44` |
| `S`, `S_EMP` e `T` simulação | **336 marcadas**, **136 fora de `silencio_decidido`**, **200** no universo e **200** no contador. `SEM_TXT_SEM_TRANCA_C20` tem **200 perguntas em 64 colaborador-dias**, com **132, 16 e 52** perguntas nas empresas **2, 3 e 4**. Cada uma das outras três classes tem **0 perguntas e 0 colaborador-dias**. `trancada_sem_txt` e `trancada_sem_txt_confirmada` são **0**. A igualdade `200 = 200` é a conferência interna da soma das classes. `r.txt, bloco Python:318–356,380–408,443–447` · `saida_sem_P.txt:45–52` |
| `ROLLBACK_MARCAS` | **`true`**. Depois de sair da transação, a sonda releu as marcas de ressurreição no instante fixado e obteve o mesmo mapa de PK do ato para PK da pergunta que tinha antes. A verificação de outros efeitos da transação fica `[nao verificado]`. `r.txt, bloco Python:154–156,438–461` · `saida_sem_P.txt:39` |

**Dependem das etapas com erro.** O universo e o contador dos dois `VIGIA`, assim como a pertinência e a rota das perguntas no censo, ficam `[nao verificado]`. Os **64 colaborador-dias** de `S` vêm da deduplicação por classe. O resultado de `botao.modal` para os chamados que consultaram o GET de `col723` fica `ERRO`. Quantos `P` foram afetados por esse GET é `[nao verificado]`. `por_pergunta` e `lista_dp_silencio` recebem `[nao verificado]` por construção. `r.txt, bloco Python:249–254,288–310,347–372,384–396` · `saida_sem_P.txt:29,37–38,54–56`

## P4 — Trecho para a próxima rodada

A alteração intercepta `registrar_engolido()` durante o savepoint de `ler()`, guarda a primeira ocorrência e chama o registrador original. Quando o chamador passa `None` dentro de um `except`, `sys.exc_info()` recupera a exceção ativa. A mensagem publicada é fixa e só inclui o identificador da operação se ele estiver na lista de literais conhecidos. O texto bruto da exceção, seus argumentos, o contexto do registrador e o traceback ficam fora da saída. A rodada na sombra com esse trecho fica `[nao verificado]`. `r.txt, bloco Python:65–90,475–477,499` · `app/core/observ.py:73–91` · `app/chamados/services/fio.py:330–332,426–430,455–457`

Ao lado de `from core.observ import engolidos`, acrescente `from core import observ`. Em `falha()`, troque `if exc else None` por `if exc is not None else None`. Substitua `ler()` pelo trecho abaixo. `r.txt, bloco Python:65–90,499`

```python
ETAPAS = {"vigia_ESTOQUE", "vigia_SIMULACAO", "modal_SIMULACAO"}
amostradas = set()

OPERACOES = {
    "chamados.vigia_fio.fio_do_admin_lista",
    "chamados.pergunta_viva.lei_indisponivel",
    "chamados.pergunta_viva.juiz_indisponivel",
    "chamados.juiz.janela_viva_indisponivel",
    "chamados.juiz.lei_indisponivel_na_via",
    "chamados.juiz.batida_origem_indisponivel",
    "chamados.juiz.lei_indisponivel_no_fallback",
    "escala.folga_nao_cobra",
    "escala.ausencia_cala",
    "escala.nucleo_indisponivel_na_lei",
    "chamados.materializacao.marcos_da_regua",
    "chamados.materializacao.tipo_pelo_marco_ata",
    "chamados.materializacao.tipo_marco_divergente",
    "chamados.fio.propostas_texto",
    "chamados.fio.o318_em_regularizacao",
    "chamados.fio.desenlace",
}


def ler(etapa, fn, *, desfazer=False, **ids):
    antes = engolidos()["total"]
    registrar = observ.registrar_engolido
    primeira = None

    def guardar(logger_nome, operacao, exc, **contexto):
        nonlocal primeira
        if primeira is None:
            causa = exc if exc is not None else sys.exc_info()[1]
            origem = f"{logger_nome}.{operacao}"
            mensagem = ("Registro best-effort em " + origem
                        if origem in OPERACOES else "Registro best-effort")
            primeira = causa, mensagem
        return registrar(logger_nome, operacao, exc, **contexto)

    try:
        try:
            with patch.object(observ, "registrar_engolido", guardar), transaction.atomic():
                valor = fn()
                if desfazer:
                    raise Desfaz()
        except Desfaz:
            if not desfazer:
                raise
    except Exception as exc:
        falha(etapa, exc, **ids)
        valor = ERRO

    delta = engolidos()["total"] - antes
    if delta:
        causa = None
        detalhe = {}
        if etapa in ETAPAS and etapa not in amostradas and primeira is not None:
            causa, detalhe["mensagem"] = primeira
            amostradas.add(etapa)
        falha(etapa + "_engolidos", causa, qtd=delta, **ids, **detalhe)
        return ERRO
    return valor
```

## O QUE NAO LI

- Banco, logs, tracebacks e HTML da rodada. Causas efetivas, tipos, mensagens e divisão dos **18, 18 e 45** por operação `[nao verificado]`.
- Linhas `P` omitidas, inclusive os PKs cujo campo `botao.modal` recebeu `ERRO` por `col723`. `saida_sem_P.txt:38,52,54–56` · `[nao verificado]`.
- Metadados Git da cópia. Os **23 SHA-256** dos arquivos listados conferem com o anexo. A confirmação do `HEAD` pelo Git fica `[nao verificado]`. `saida_sem_P.txt:3–26`.
- Rodada na sombra com o trecho proposto e verificação de rollback de efeitos além do mapa de marcas no instante fixado. `r.txt, bloco Python:454–461` · `[nao verificado]`.

## 20/11 14:02 · sessao c4207efb99e4 · RAIZ-DA-DISPUTA-F1-RASCUNHO · sonda · origem=Code

```python
# Envie este bloco pelo stdin, a partir de app/ (bin/sombra.sh:378, app/config/settings/sombra.py:49).
# DJANGO_SETTINGS_MODULE=config.settings.sombra python manage.py tenant_command shell --schema=juliani --verbosity=0

import json
from collections import Counter, defaultdict

from django.conf import settings
from django.db import connection, transaction
from django.db.models import Exists, OuterRef

from chamados.models import DisputaSupervisao, FollowupChamado


CONTAGENS = (
    "universo", "dois_ou_mais", "2", "3", "4+",
    "motivos_diferentes", "motivos_fechamento_diferentes",
    "com_motivo_fechamento_ausente", "reabertura_admin",
    "nota_sistema_no_pai", "reabertura_admin_ou_nota_no_pai",
)


def emitir(**linha):
    print(json.dumps(linha, separators=(",", ":")), flush=True)


def medir(leitura, vinculos, empresas, disputas):
    total = Counter(dict.fromkeys(CONTAGENS, 0))
    por_empresa = defaultdict(Counter)
    casos = {}

    for chamado_pk, disputa_pks in sorted(vinculos.items()):
        encerradas = sorted(pk for pk in disputa_pks if disputas[pk]["fechada"])
        qtd = len(encerradas)
        medidas = Counter(universo=1)
        if qtd >= 2:
            casos[chamado_pk] = encerradas
            dados = [disputas[pk] for pk in encerradas]
            reabertura_admin = any(d["reabertura_admin"] for d in dados)
            nota_no_pai = any(d["nota_no_pai"] for d in dados)
            motivos_fechamento = {d["motivo_fechamento"] for d in dados}
            medidas.update({
                "dois_ou_mais": 1,
                str(qtd) if qtd < 4 else "4+": 1,
                "motivos_diferentes": int(len({d["motivos"] for d in dados}) > 1),
                "motivos_fechamento_diferentes": int(len(motivos_fechamento - {"", None}) > 1),
                "com_motivo_fechamento_ausente": int(bool(motivos_fechamento & {"", None})),
                "reabertura_admin": int(reabertura_admin),
                "nota_sistema_no_pai": int(nota_no_pai),
                "reabertura_admin_ou_nota_no_pai": int(reabertura_admin or nota_no_pai),
            })
        total.update(medidas)
        por_empresa[empresas[chamado_pk]].update(medidas)

    emitir(leitura=leitura, **total)
    for empresa_pk, contagens in sorted(por_empresa.items()):
        emitir(leitura=leitura, empresa_pk=empresa_pk, **{k: contagens[k] for k in CONTAGENS})
    for chamado_pk, disputa_pks in casos.items():
        emitir(leitura=leitura, chamado_pk=chamado_pk, disputa_pks=disputa_pks)


def sondar():
    sombra = getattr(settings, "HASNER_SOMBRA", False)
    with connection.cursor() as cursor:
        cursor.execute("SELECT current_database(), current_schema()")
        banco, schema = cursor.fetchone()
    emitir(banco=banco, schema=schema, HASNER_SOMBRA=sombra)
    if sombra is not True or banco != "sombra" or schema != "juliani":
        print("INCONCLUSIVA", flush=True)
        raise SystemExit(1)

    alias = connection.alias
    nota_sistema = FollowupChamado.objects.using(alias).filter(
        chamado_id=OuterRef("chamado_id"),
        autor_tipo="sistema",
        conteudo__startswith="[sistema] disputa reaberta: ",
    )
    linhas = (
        DisputaSupervisao.objects.using(alias).order_by()
        .annotate(nota_sistema_pai=Exists(nota_sistema))
        .values_list(
            "pk", "chamado_id", "chamado__colaborador__empresa_id",
            "fechada_em", "motivos", "motivo_fechamento", "reaberturas_admin",
            "nota_sistema_pai", "perguntas__pk", "perguntas__chamado_destino_id",
            "perguntas__chamado_destino__colaborador__empresa_id",
            "perguntas__materializada_em",
        )
    )

    vinculos = {
        leitura: defaultdict(set)
        for leitura in (
            "destino", "destino_sem_materializacao", "pai",
            "pai_ou_destino", "ancora_da_pergunta_no_fio",
        )
    }
    empresas, disputas = {}, {}
    for (pk, pai, empresa_pai, fechada_em, motivos, motivo_fechamento,
         reaberturas_admin, nota_no_pai, pergunta_pk, destino, empresa_destino,
         materializada_em) in linhas.iterator(chunk_size=4000):
        disputas[pk] = {
            "fechada": fechada_em is not None,
            "motivos": frozenset(motivos or []),
            "motivo_fechamento": motivo_fechamento,
            "reabertura_admin": reaberturas_admin > 0,
            "nota_no_pai": nota_no_pai,
        }
        if pai is not None:
            empresas[pai] = empresa_pai
            vinculos["pai"][pai].add(pk)
            vinculos["pai_ou_destino"][pai].add(pk)
        if destino is not None:
            empresas[destino] = empresa_destino
            vinculos["destino"][destino].add(pk)
            vinculos["pai_ou_destino"][destino].add(pk)
            if materializada_em is None:
                vinculos["destino_sem_materializacao"][destino].add(pk)
        ancora = destino if destino is not None else pai
        if pergunta_pk is not None and ancora is not None:
            vinculos["ancora_da_pergunta_no_fio"][ancora].add(pk)

    for leitura, pares in vinculos.items():
        medir(leitura, pares, empresas, disputas)


with transaction.atomic(using=connection.alias):
    try:
        sondar()
    finally:
        transaction.set_rollback(True, using=connection.alias)
```

- `destino` liga `PerguntaDisputa.chamado_destino` à disputa dona. `destino_sem_materializacao` conta as donas distintas das perguntas com `materializada_em` nulo, antes do `.first()` do repasse, que escolhe a pergunta de menor PK. `app/chamados/models.py:2603`, `app/chamados/models.py:3639`, `app/chamados/models.py:3761`, `app/chamados/services/disputa_emissao.py:1084`
- `pai` usa `DisputaSupervisao.chamado`, também lido pela reabertura por sistema. `pai_ou_destino` une os dois vínculos por par de PKs. `ancora_da_pergunta_no_fio` usa o destino com fallback para o pai, antes dos filtros de exibição. `universo_do_fato` recebe colaborador, motivo, dia e episódio opcional para a eleição da canônica. `app/chamados/models.py:1886`, `app/chamados/models.py:2341`, `app/chamados/services/fio.py:117`, `app/chamados/services/fio.py:268`, `app/chamados/services/fio.py:1948`
- O universo inclui chamados com qualquer disputa vinculada. Os cortes `2`, `3` e `4+` usam `fechada_em` preenchido e agrupam pela empresa do colaborador do chamado. Entre as encerradas, a sonda compara os conjuntos de `motivos` e, separadamente, os códigos preenchidos de `motivo_fechamento`, com uma contagem própria para ausências. `app/chamados/models.py:44`, `app/colaboradores/models.py:321`, `app/chamados/models.py:1878`, `app/chamados/models.py:1930`, `app/chamados/models.py:2095`
- Nos casos com duas ou mais encerradas, `reabertura_admin` usa `reaberturas_admin > 0` em alguma delas. `nota_sistema_no_pai` conta a presença da nota em algum pai dessas disputas. A união conta cada chamado uma vez. A nota guarda o chamado-pai e o texto, então a atribuição da reabertura de sistema a um PK de disputa fica [nao verificado]. `app/chamados/models.py:2316`, `app/chamados/models.py:2343`, `app/chamados/services/nota.py:88`
- **O QUE NAO LI.** Registros de banco, logs reais e metadados Git do hash informado. Execução da consulta na sombra e incidência na frota [nao verificado].

## 20/11 13:54 · sessao da91ef7c26e7 · E4C-F5-CONSTROI · verificador · origem=Code

### P1 — Premissas

- [nao verificado] | O diretório da cópia tem o nome do hash informado, mas não contém metadados Git que permitam confirmar o HEAD. A revisão usa os arquivos da cópia e os oito anexos fornecidos.
- PROCEDE | O core já toma a trava da linha do **Colaborador dono** dentro de `atomic`, depois da validação de formato. Sob a trava, faz `p.refresh_from_db()` da linha inteira, lê o passivo de resposta e validação para preservar a trilha, captura `validada_em` e `validada_por_id` e só então lê o relógio da nova resposta. `ja_respondida` vem do chamador e serve à trilha e ao rótulo da nota. O core hoje não chama `pergunta_viva`. `app/chamados/services/disputa_emissao.py:1439–1496,1547–1579`; `app/chamados/models.py:2830–2836,2874–2878,4276–4294`.
- PROCEDE | A H4 é da mesma classe de corrida que o RETOMA A1 descreve: **“leitura colhida antes da trava”**. O A1 mandou reler instância e instante depois da trava existente nos três sítios do censo. A cura A1(b) fica no core e seu RED espera que uma resposta posterior à **validação** seja aceita, limpe ato e coluna e preserve a assinatura na trilha. O A1 não decidiu o que fazer com uma resposta posterior à **absorção**. Anexo 2, A1; `app/chamados/services/disputa_emissao.py:1481–1532,1630–1636`; `app/chamados/services/materializacao.py:1036–1057`; `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94`.
- PROCEDE | Web e API são os dois chamadores que aplicam diretamente `pergunta_viva` como condição de entrada, antes da trava do core. A correção administrativa verifica a validação composta, disputa aberta e tipo `hora`, depois chama o core. As duas declarações leem o marco faltante, tomam a trava do ato e usam `pergunta_sem_lastro_ou_viva` na **seleção** da candidata. Se a seleção não a reaproveita, `abrir` ainda pode devolver a mesma canônica. As duas verificam validação composta antes do core. `app/chamados/views.py:1237–1246,1274–1282`; `app/api/views.py:2634–2656`; `app/chamados/services/acoes_disputa.py:344–378`; `app/chamados/services/declaracao_texto.py:217–256,336–380,419–423,651–670,689–698`; `app/chamados/juizes.py:1503–1507`.
- NAO PROCEDE | **“Como ja e hoje quando as duas coisas acontecem em sequencia”** só vale para a entrada web/API que lê a pergunta já morta depois da passagem. O core chamado depois do carimbo continua com a escrita. Na medida, ambas as irmãs tinham `viva_depois_de_P=False` e ambas retornaram `(True, None)`, com `resposta='19:02'`, `respondida_em=True`, `carimbo=True`, `via='ausencia_dia'`, `absorcoes=1` e validação ausente na coluna e na composta. `irma_com_luz` já tinha `guarda_viva_antes=False` e terminou `resolvida`, morta. `irma_muda` tinha `guarda_viva_antes=True` e terminou `respondida`, viva. Anexo 1, item 2; anexo 4, linhas `MEDIDA-H4`; `app/chamados/views.py:1237–1246`; `app/api/views.py:2634–2644`; `app/chamados/services/disputa_emissao.py:1481–1532,1579`.
- PROCEDE | O 1230 item 1 protege a resposta vigente completa que chega **durante** a declaração. A passagem fixa T1, relê cada irmã sob a trava e, se a resposta completa é posterior a T1, exclui a irmã **antes** de tentar o carimbo. Isso vale para resposta humana e automática. O automático usa `responder_automaticamente` e não passa pelo core revisado. Anexo 3, item 1; `app/chamados/services/materializacao.py:1036–1081`; `app/chamados/juizes.py:1383–1403`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:142–187`; `app/api/foto_recebida.py:38–51`.
- NAO PROCEDE | **“Quem grava primeiro sob a trava vale”** é mais amplo que o que o 1230 decidiu. Hoje, a passagem protege a resposta completa posterior a T1 que já encontra na releitura. Se a passagem carimba primeiro, os dois casos H4 mostram que o core ainda aceita a resposta depois. A trava da passagem é por irmã e o carimbo e o ato `ABSORCAO` ficam no mesmo bloco. Anexo 3, item 1; anexo 4, linhas `MEDIDA-H4`; `app/chamados/services/materializacao.py:1036–1081`.

### P2 — Colisão

- PROCEDE | Na leitura geral da frase do 1400 **“O core repete a guarda da pergunta viva SOB a trava do dono”**, a guarda também recusa uma irmã que já estava morta **antes** da absorção. Isso alcança o caso do 1230 **“resposta que chega durante a declaracao da ausencia: CONTINUA VALENDO e fica fora da passagem”**. O teste da irmã com luz chama o core antes da trava da passagem e exige que a resposta seja registrada em T2 e fique fora da absorção. O caso com luz da própria H4 já é falso antes de P. Uma recusa restrita à absorção vigente já confirmada preserva a ordem autorizada pelo 1230. Anexo 1, item 2; anexo 3, item 1; anexo 4, `MEDIDA-H4 irma_com_luz`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:80–104,142–167`.
- PROCEDE | A aplicação geral também muda a elegibilidade da correção administrativa, embora o 1025 item 5 diga **“janelas de elegibilidade antes da trava em validar, recusar e corrigir) ficam registrados na E4D”**. A correção hoje chega ao core com guardas próprias. A consequência concreta da guarda geral aparece também no RED A1(b): o C6 devolve falso diante da validação que o teste exige que a resposta posterior limpe. Anexo 1, item 2, **“O core repete a guarda da pergunta viva SOB a trava do dono”**; anexo 7, item 5; `app/chamados/services/acoes_disputa.py:344–378`; `app/chamados/juizes.py:1406–1415`; `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–94`.
- NAO PROCEDE | A inserção de uma leitura sob a trava existente, por si, não infringe **“trava nova; mudar ordem de trava; ato de reparo”** nem **“quebrar perguntas_absorvidas_por”**. A obrigação de preservar o grupo continua relevante: seu leitor usa o texto do escape, a via e o relógio comum, e a rejeição da ausência usa esse grupo para devolver as perguntas. Anexo 1, item 2, **“Sem trava nova”**; anexo 7, item 4 e `PROIBIDO`; anexo 8, `PROIBIDO`; `app/chamados/services/materializacao.py:1074–1081,1123–1156,1183–1207`.

### P3 — Literal

- PROCEDE | Falta definir **“absorção confirmada”** como condição sobre a mesma pergunta. `pergunta_viva=False` também pode vir de validação, tranca de competência, identidade, decisão da célula ou lastro. `materializada_em` é um carimbo compartilhado, `declaracao` também é via de plantio, uma linha `ABSORCAO` pode ficar no histórico depois da reabertura e `perguntas_absorvidas_por` busca o grupo pela projeção e pela âncora, não pela linha do ato. Além disso, repetir a chamada ao C6 após `refresh_from_db()` não demonstra por si uma leitura nova: a chave do memo não inclui as linhas de decisão, e `registrar` invalida o memo da instância que recebeu. `app/chamados/juizes.py:826–899,1406–1500`; `app/chamados/services/materializacao.py:1066–1081,1143–1156,1183–1207,1511–1533`; `app/chamados/models.py:4382–4398`.
- PROCEDE | **“RED = os 2 casos”** aponta para sondas que saíram `ok` registrando o aceite indevido. Os dois `FAIL` do anexo são do teste de **resposta anterior à absorção, já validada**, executado nas duas classes. O teste da cópia já espera a tentativa adicional `celula_absolveu` que o traceback mostrou. Execução dessa versão e fonte dos dois testes H4 na cópia [nao verificado]. Anexo 4, linhas `test_medida_h4_*`, `FAIL` e `FAILED (failures=2)`; `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:271–309`.
- PROCEDE | O mutante **“guarda antes da trava”** é exprimível depois que a guarda for definida. Dos dois valores medidos, só `irma_muda` distingue a posição da leitura: `True` antes de P e `False` depois. `irma_com_luz` dá `False` dos dois lados. Para distinguir as posições, o carimbo precisa ocorrer entre a leitura antecipada e a aquisição da trava da resposta. A execução dessa intercalação pelos testes H4 [nao verificado]. Anexo 1, item 2; anexo 4, linhas `MEDIDA-H4`; `app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:62–77` mostra a técnica de interposição na trava.
- PROCEDE | **“RECUSADA”** tem um envelope disponível, `(False, erro)`. Falta ao item dizer qual é o erro H4 e quais efeitos anteriores à chamada podem persistir. Hoje o core usa esse envelope para erros de entrada, antes da primeira escrita. `app/chamados/services/disputa_emissao.py:1439–1478,1527–1532`; anexo 1, item 2.
- PROCEDE | **Web/PWA:** se o core devolver falso, a view devolve `resposta_acao(ok=False)` com a mensagem do core. Esse envelope registra a recusa e devolve toast HTMX com 204 ou redireciona. **API:** se o core devolver falso, registra `resp_pergunta_recusada` e devolve JSON 400. O veto anterior da API por pergunta morta devolve 404. `app/chamados/views.py:1278–1282`; `app/core/respostas.py:47–71`; `app/api/views.py:2624–2644,2652–2656`.
- PROCEDE | **Correção administrativa:** devolve `resp('erro', ...)` e pula o log de sucesso e `validar_pergunta`. A nota `horario_corrigido` é inserida **antes** do core quando há chamado e pode ficar gravada, inclusive com o texto “O admin corrigiu para”. A mesma ação atende ao card e ao modal do fio. `app/chamados/services/acoes_disputa.py:356–385`; `app/chamados/services/nota.py:87–107`; `app/chamados/views.py:523–536`; `app/chamados/services/fio.py:674–688`.
- PROCEDE | **Declaração de hora do colaborador:** devolve `resposta_invalida` e a view responde JSON 400, antes da nota de declaração e do push de sucesso. **Aplicação de declaração pelo admin:** devolve `resposta_invalida` com `pergunta_id` e pula `validar_pergunta`. Nos dois caminhos, o retorno normal de dentro do `atomic` permite confirmar as escritas anteriores, como abertura ou agregação da disputa e nascimento ou reapontamento da pergunta. `app/chamados/services/declaracao_texto.py:350–379,419–428,656–669,689–699`; `app/chamados/views.py:477–481,1089–1099`; `app/chamados/services/disputa_emissao.py:483–617,620–645`; `app/chamados/models.py:3177–3234`.

### P4 — Alcance

- PROCEDE | Uma chamada geral ao C6 sob a trava acrescenta, conforme o ramo, leitura de **competência trancada**, célula, batidas, ausência e **escala/vínculo** dentro da seção travada. A leitura de jornada pode chegar à constante `MotorBase.CADASTRO_NAO_DESCREVE_MIN` em `ponto/motor_calculo_v2.py`. A trava do dono já existe. O C6 usa K7 para a tranca, enquanto a validação administrativa tem a consulta K6 à entrega do TXT em `folha/export.py`. `app/chamados/services/disputa_emissao.py:1478–1485`; `app/chamados/juizes.py:979–997,1067–1084,1406–1496`; `app/escala/servico_jornada.py:347–405`; `app/ponto/turnos.py:1440–1444,1850–1920,1997`; `app/chamados/services/materializacao.py:792–831`.
- PROCEDE | Uma recusa geral também pode impedir atos com efeito no ponto e no cálculo: `FALTA_DIA` cria falta aprovada, cujos minutos alimentam o fechamento, e as duas portas administrativas que validam depois do core podem deixar de plantar batida e de agendar recálculo. O recálculo existente pode alcançar competência já exportada e lê funções de `folha/export.py`. Uma guarda estrita de H4 no core cabe sem edição direta em escala/vínculo, `ponto/motor_calculo_v2.py` ou `folha/`, e sem nova trava ou mudança de ordem. Incidência em competência exportada e diferença monetária nos dois casos H4 [nao verificado]. `app/chamados/services/disputa_emissao.py:1556–1561`; `app/chamados/services/materializacao.py:1022–1035,1117–1120,1511,1628`; `app/ponto/models.py:878–897`; `app/chamados/services/acoes_disputa.py:375–384`; `app/chamados/services/declaracao_texto.py:695–699`; `app/chamados/services/validacao.py:746–772`; `app/ponto/services/fechamento.py:485–498,873–875,1798–1825`; anexo 4.

**Campos lidos pela guarda e gravações que a recusa pode suprimir**

- PROCEDE | **`resposta_colab`, `respondida_em`, `validada_em`, `validada_por`**. Escrevem o core, a resposta automática e o carimbo pela trilha, a porta `carimbar`, a validação e suas devoluções, e os lotes de reabertura e ressuscitação, conforme o campo. Leem `linhas_da_projecao_sem_linha`, `DecisaoPergunta.composta` e o C6, as filas de validação/materialização, o vigia do par, a passagem e o leitor de absorvidas. Os autores são lidos nas fotos e na trilha de validação e devolução. `app/chamados/services/disputa_emissao.py:1487–1532,1615–1668,1674–1722`; `app/chamados/models.py:1721–1722,3346–3357,4276–4294,4333–4371`; `app/chamados/services/validacao.py:161–188,269–296,324,697–704`; `app/chamados/services/materializacao.py:1041–1075,1143–1156,1188–1192,2092`; `app/chamados/services/vigia_ato_x_coluna.py:8–20`.
- PROCEDE | **`materializada_em`, `materializada_por`, `via_resolucao`**. Escrevem as portas de carimbo/descarimbo do modelo. A via também pode mudar em `encerrar_por_fato_no_espelho` e `relavra_via`. Leem C6 e lastro, a passagem, `perguntas_absorvidas_por` e a devolução da ausência rejeitada, que também lê o autor. O core só deixa de limpar esse trio quando a via é sem lastro. A via `ausencia_dia` da H4 não está nesse conjunto. `app/chamados/models.py:3317–3377,3402–3433,3508–3522,3538–3552,4088–4089`; `app/chamados/services/relavra_via.py:106–125`; `app/chamados/juizes.py:1406–1500`; `app/chamados/services/materializacao.py:1037–1081,1143–1156,1188–1192`.
- PROCEDE | **Atos `DecisaoPergunta`**. `registrar` grava as linhas. O core fornece a nova `RESPOSTA` e, quando necessário, as fotos do passivo, com pergunta, tipo de ato, instante, texto, autor, motivo e `resposta_mantem_validacao` conforme a linha. A passagem fornece `ABSORCAO` com os vínculos da ausência e do escape. `leitura/composta` e C6 leem a sequência para a resposta e a validação vigentes. A tarefa do dia lê a sequência e o estado da ausência ligada. O modal lê texto, instante, autor e motivo da trilha. `app/chamados/services/disputa_emissao.py:1527–1532`; `app/chamados/services/materializacao.py:1077–1081`; `app/chamados/models.py:4276–4294,4333–4362,4374–4398`; `app/chamados/services/tarefa_do_dia.py:157–192`; `app/templates/chamados/partials/modal_fio.html:212`.
- PROCEDE | **Identidade e lastro**. `abrir/_nascer` escreve disputa, motivo, tipo, opções e `conversa` no nascimento. `gravar_dia_do_fato` escreve `data_evento`. Nascimento, costura e rechaveamento escrevem destino/célula. A porta do chamado escreve `status_local`. A porta da célula escreve `veredito`, `veredito_via`, `ata`, `dna` e `trabalha` nos atos correspondentes. As portas de batida e de ausência escrevem as testemunhas que C6, lastro e passagem consultam. As portas de fechamento escrevem `PeriodoFechado`, e os atos humanos escrevem a trilha de toque que o C6 lê. Nesse último ramo, o helper do usuário `sistema` ainda pode criá-lo se faltar. `app/chamados/models.py:213–240,3177–3234`; `app/chamados/services/fio.py:843–867,1032–1091`; `app/ponto/portas/celula.py:306,642,1090–1098`; `app/ponto/registro_batida.py:118,264–287`; `app/ponto/services/ausencia.py:262,1057,1285`; `app/ponto/services/fechamento.py:1400–1441,1464–1470`; `app/chamados/juizes.py:667–823,935–976,1088–1098,1228–1234,1242–1314`; `app/core/utils.py:6–18`.
- PROCEDE | **Outras gravações da resposta**. O core pode escrever `recusa_motivo`, também escrito na recusa administrativa e na materialização, e lido na devolução e no modal. A resposta aceita pode produzir auditoria, followup, `EventoCelula` e resgate do chamado. Escrevem esses efeitos `core.observ.evento`, `nota.anexar_ocorrencia`, `emitir_evento_celula` e `arquivo.levantar`, respectivamente. Leem a trilha e o fio, o reconciliador consome o evento, e o cron de prescrição lê `resgatado_em`. A nota prévia da correção continua sujeita ao comportamento descrito em P3. `app/chamados/services/disputa_emissao.py:1524–1576`; `app/chamados/services/validacao.py:269–296,934`; `app/chamados/services/materializacao.py:1214–1242,1388,1725`; `app/templates/chamados/partials/modal_fio.html:226`; `app/core/observ.py:94–137`; `app/chamados/services/nota.py:75–109`; `app/chamados/views.py:372`; `app/chamados/signals.py:244–259`; `app/ponto/portas/celula.py:945–954`; `app/chamados/reconciliador.py:1369`; `app/chamados/services/arquivo.py:194–211`; `app/chamados/management/commands/arquivar_prescritos.py:54`.

VEREDITO ITEM 2: objecao: a premissa do aceite em sequência está invertida no core, e a guarda geral de vitalidade excede H4. O aval precisa definir absorção vigente já confirmada, o retorno e as gravações por chamador, e o RED que distingue a leitura sob a trava.

## 20/11 13:49 · sessao e26af89327c3 · E4C-F5-CONSTROI · leitura · origem=Code

## Q0 EM ATE 10 LINHAS

1. **Sim — PERGUNTA DE DESENHO da F5.** D3 limpa instante e texto sem ato compensador. A05 pode gravar o instante sem uma `RESPOSTA` correspondente. [escritores.md:16] [matriz.md:20] [app/chamados/services/validacao.py:452] [app/chamados/models.py:3350]
2. A12 grava as duas colunas e `RESPOSTA` no mesmo `atomic`, depois de travar o colaborador, reler a pergunta e obter o instante. [matriz.md:27] [app/chamados/services/disputa_emissao.py:1478] [app/chamados/services/disputa_emissao.py:1527]
3. A13, inclusive por A19 e A23, e D2 limpam por A08 e registram os atos no mesmo `atomic`. Há seleção ou decisão anterior à trava em A19, A23 e D2. [matriz.md:28] [matriz.md:34] [matriz.md:38] [escritores.md:71] [app/chamados/services/validacao.py:915] [app/chamados/models.py:2414] [app/chamados/models.py:1632]
4. A14 e A15 gravam coluna e `RESPOSTA` no mesmo `atomic`, com guardas sobre a instância anterior ao bloco e sem trava própria. A ordem de gravação pode divergir da ordem por `ato_em`. [matriz.md:29] [matriz.md:30] [app/chamados/services/disputa_emissao.py:1683] [app/chamados/services/disputa_emissao.py:1713] [app/chamados/models.py:4339]
5. Além da enumeração do mapa, há o `save` genérico e a máscara de `resposta_colab` por `bulk_update`, restrita ao banco `sombra`. O nascimento A22 aparece no mapa com campos efetivos `[nv]`. [matriz.md:37] [app/chamados/models.py:3691] [bin/sombra_regras_pessoais.py:85] [bin/sombra_mascara_pessoal.py:10] [bin/sombra_mascara_pessoal.py:62]
6. `composta` toma o instante da sequência de atos, com complemento do legado, e o texto da coluna. API, templates e consultas SQL ainda leem o instante cru. [app/chamados/models.py:4333] [app/chamados/models.py:4360] [app/api/views.py:1165] [app/templates/chamados/partials/modal_fio.html:220] [app/chamados/juizes.py:1588]
7. O comparador, o vigia A/B e o selo de pareamento têm por alvo `validada_em`. Existe vigia separado da igualdade do texto. Equivalente para o instante da resposta `[nao verificado]`. [app/chamados/models.py:4371] [app/chamados/services/vigia_ato_x_coluna.py:16] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:22] [app/chamados/services/vigia_texto_resposta.py:23]

### 1. O que o mapa atribui a cada escritor

As linhas dos dois mapas são contadas a partir do título de cada anexo raspado.

| Escritor e porta | Campos e ato segundo o mapa |
|---|---|
| **A05** `PerguntaDisputa.carimbar` | `respondida_em` condicional. A síntese agrupa os dois campos, mas a linha específica de A05 só atribui o instante. Ato próprio de decisão `[nv]`. [escritores.md:16] [escritores.md:42] [matriz.md:20] |
| **A08** `PerguntaDisputa.descarimbar_em_lote` | Recebe os dois campos por `**outros`. Ato próprio `[nv]`. [escritores.md:45] [matriz.md:23] |
| **A12** `responder_pergunta_disputa_core` | Ambos. `RESPOSTA` e linhas legadas faltantes. [escritores.md:49] [matriz.md:27] |
| **A13** `reabrir_para_correcao` → A08 | Limpa ambos. `REABERTURA` por padrão, ou o ato recebido, para pergunta da qual algo é retirado. [escritores.md:50] [matriz.md:28] |
| **A14** `responder_automaticamente` | Ambos. `RESPOSTA` e linhas legadas faltantes. [escritores.md:51] [matriz.md:29] |
| **A15** `carimbar_resposta_pela_trilha` | Só o instante. `RESPOSTA` e evento `carimbar_resposta_pela_trilha`. [escritores.md:52] [matriz.md:30] |
| **A19** `recusar_pergunta` → A13 → A08 | Limpeza delegada de ambos. `RECUSA` via A13, mais ocorrência e auditoria. [escritores.md:56] [matriz.md:34] |
| **A23** `reabrir_por_colab` → A13 → A08 | Limpeza delegada de ambos. `REABERTURA` via A13 e ocorrência `pedido_de_correcao`. [escritores.md:60] [matriz.md:38] |
| **D2** `ChamadoColaborador.ressuscitar_perguntas` → A08 | Limpa ambos. Registra `REABERTURA` marcada. Substituição `[nao verificado]` e exceção FATO `[nao verificado]` na tabela detalhada. A síntese marca a exceção `[nv]`. [escritores.md:15] [escritores.md:16] [escritores.md:71] [escritores.md:101] [escritores.md:102] |
| **D3** `devolver_instante_sem_texto_l130` → A08 | Limpa ambos. A síntese diz **nenhum** ato substituto. A tabela detalhada diz nenhum ato de reabertura/devolução, substituição `[nao verificado]` e exceção FATO `[nao verificado]`. A síntese marca a exceção `[nv]`. [escritores.md:15] [escritores.md:16] [escritores.md:72] [escritores.md:101] [escritores.md:102] |
| **A22** `_nascer` | O mapa registra `**campos`, defaults e linhas de projeção sem linha quando houver, mais `pergunta_emitida`. Os campos efetivos e defaults ficam `[nv]` na matriz. A enumeração dos argumentos dos chamadores fica `[nao verificado]` em escritores. [escritores.md:59] [matriz.md:37] |

### 2. Escrita, transação e leitura anterior à trava nesta cópia

`models.py` abaixo é `app/chamados/models.py`. `disputa_emissao.py`, `validacao.py` e `materializacao.py` ficam em `app/chamados/services/`.

| Sítio atual | Como escreve e o que fica no mesmo `atomic` | Leitura anterior à trava e efeito possível |
|---|---|---|
| **A05** `carimbar` | Porta de carimbo com `save` direto. Preenche só `respondida_em`, quando há texto e falta instante. O corpo pode gravar a coluna sem `RESPOSTA`. O caminho de absorção grava `ABSORCAO` no mesmo bloco, cujo efeito sobre a resposta vigente é neutro. [models.py:3350] [models.py:3372] [materializacao.py:1067] [materializacao.py:1078] [models.py:4301] | A porta usa a instância e o relógio recebido, sem travar nem reler por conta própria. O resolvedor trava antes de carregar as perguntas. A absorção fixa relógio e candidatas antes da trava e relê a pergunta sob ela. [models.py:3344] [disputa_emissao.py:1925] [disputa_emissao.py:1963] [materializacao.py:1036] [materializacao.py:1052] |
| **A08** `descarimbar_em_lote` | Porta comum do lote. Faz `UPDATE` direto com `**outros`. Exige colaborador já travado e depende do chamador para o `atomic` e o ato. [models.py:3472] [models.py:3497] [models.py:3509] | Nos três chamadores que passam o par inteiro, a validação do par usa os valores recebidos. Se receber só um campo, consulta a outra coluna antes de conferir a trava já adquirida pelo chamador. [models.py:3483] [models.py:3499] |
| **A12** `responder_pergunta_disputa_core` | Porta da resposta humana com `save` direto. Colunas, fotos legadas faltantes e `RESPOSTA` compartilham o `atomic` e o relógio da resposta. [disputa_emissao.py:1478] [disputa_emissao.py:1494] [disputa_emissao.py:1527] | Sim, tipo e opções são avaliados antes. Web e API também calculam `ja_respondida` antes, usado para rotular a trilha. Para gravar a resposta e fotografar o que substitui, o core relê sob a trava e só então pega o relógio. [disputa_emissao.py:1449] [app/chamados/views.py:1275] [app/api/views.py:2646] [disputa_emissao.py:1481] [disputa_emissao.py:1552] |
| **A13** `reabrir_para_correcao` | Delega a A08. Foto, ato e limpeza ficam no mesmo `atomic`. A linha depende de haver estado a retirar. Se ambas as colunas já estão vazias e só há resposta no ato, a checagem extra de estado só no ato procura `validada_em`. [disputa_emissao.py:1627] [disputa_emissao.py:1640] [disputa_emissao.py:1664] | Lê os donos antes da trava e fotografa sob a trava. Pode receber PKs cuja elegibilidade já foi decidida fora dela, como em A23. [disputa_emissao.py:1630] [disputa_emissao.py:1654] [app/chamados/juizes.py:1765] [models.py:2414] |
| **A14** `responder_automaticamente` | Porta da resposta automática com `save` direto. Colunas, fotos faltantes e `RESPOSTA` ficam no mesmo `atomic`. [disputa_emissao.py:1693] [disputa_emissao.py:1700] | Sim. Guarda do texto e foto anterior ao `atomic`, sem trava nem releitura. O upload escolhe o instante antes do laço. Duas execuções podem passar pela mesma guarda e a que gravar por último pode carregar o menor `ato_em`. A coluna então fica com esse instante e a sequência lê o maior. [disputa_emissao.py:1683] [disputa_emissao.py:1693] [app/api/foto_recebida.py:42] [models.py:4339] |
| **A15** `carimbar_resposta_pela_trilha` | Porta de recuperação do instante com `save` direto. Instante e `RESPOSTA` ficam no mesmo `atomic`. O evento vem depois do bloco. [disputa_emissao.py:1717] [disputa_emissao.py:1723] | Sim. Guarda e texto vêm da instância recebida, sem trava nem releitura. O comando escolhe a nota antes de chamar a porta. Uma resposta concorrente pode mudar o texto depois dessa seleção, e a `RESPOSTA` da trilha ainda levar o texto antigo. [disputa_emissao.py:1713] [disputa_emissao.py:1721] [app/chamados/management/commands/curar_perguntas_em_limbo.py:55] |
| **A19** `recusar_pergunta` | Delega a A13 e A08. Motivo, limpeza e `RECUSA`, quando gerada por A13, compartilham a transação externa. Ocorrência e auditoria vêm depois. [validacao.py:938] [validacao.py:945] [validacao.py:958] | Sim. As guardas de validação, resposta e fora do fato vêm antes da trava. Uma validação ou resposta concorrente pode mudar o estado entre a guarda e a foto que A13 efetivamente limpa. [validacao.py:911] [validacao.py:924] [validacao.py:938] [disputa_emissao.py:1654] |
| **A23** `reabrir_por_colab` | Delega a A13 e A08. Limpeza, `REABERTURA` quando há estado a retirar, e contador ficam na transação externa. A ocorrência vem depois. [models.py:2421] [models.py:2430] | Sim. Lê contador, fechamento e corrigíveis antes da trava. O juiz materializa os PKs elegíveis e A23 passa esse conjunto a A13. Outra execução pode mudar a elegibilidade ou o contador entre seleção e escrita. [models.py:2409] [models.py:2423] [app/chamados/juizes.py:1765] |
| **D2** `ressuscitar_perguntas` | Delega a A08. Fotos legadas, `REABERTURA` com `da_ressuscitacao=True` e limpeza ficam no mesmo `atomic`, sobre os IDs que passaram pela releitura. [models.py:1646] [models.py:1669] [models.py:1673] [models.py:1713] [models.py:1721] | Sim. O universo vem de antes da trava local. A releitura exige materialização e via elegível. Ela pode conservar um alvo cujo texto chegou entre a seleção e a trava, fotografar a resposta nova e depois zerá-la. [models.py:1632] [models.py:1651] [models.py:1669] [models.py:1682] |
| **D3** `devolver_instante_sem_texto_l130` | Delega a A08. Registra no mesmo `atomic` a foto legada `RESPOSTA`, se faltava, e o evento de antes/depois. A limpeza fica sem ato compensador. Assim, um instante antes registrado pode continuar vigente no ato depois de zerar a coluna. [validacao.py:444] [validacao.py:452] [validacao.py:465] [models.py:4303] [models.py:4342] | Antes do bloco lê IDs para travar. Relê a pergunta, reavalia o impedimento e tira a foto sob a trava. [validacao.py:440] [validacao.py:450] |
| **A22** `_nascer` | Porta de inserção por `save(force_insert=True)`. Se `**campos` trouxer resposta com instante, a foto `RESPOSTA` sai no mesmo `atomic`. Nos chamadores localizados, os campos nascem com os defaults `''` e `None`. [models.py:2657] [models.py:3029] [models.py:3229] [models.py:4285] [disputa_emissao.py:448] [disputa_emissao.py:848] [disputa_emissao.py:1385] [app/chamados/services/declaracao_texto.py:378] [app/chamados/services/declaracao_texto.py:668] [app/chamados/services/acoes_disputa.py:267] | A validação do par recebido precede a trava, mas usa argumentos. A inserção e a foto ficam depois da trava do colaborador. [models.py:3054] [models.py:3060] [models.py:3229] |

### 3. Escritores adicionais

Busquei nomes literais em código e templates, depois segui os `**campos` e `**outros` e rodei em memória o extrator AST do selo de escritores. A varredura do selo retornou zero ocorrências fora dos sítios que ele declara. Seu escopo é `app/` e exclui SQL cru, `__dict__` e a máscara de sombra. [app/chamados/tests/test_e3_porta_A_escritor_do_par.py:19] [app/chamados/tests/test_e3_porta_A_escritor_do_par.py:92] [app/chamados/tests/test_e3_porta_A_escritor_do_par.py:166] [app/chamados/tests/test_e3_porta_A_escritor_do_par.py:203] [nao verificado]

```bash
rg -n --hidden -g '*.py' -g '*.html' -g '!**/.git/**' \
  -g '!**/tests/**' -g '!**/migrations/**' -g '!**/docs/**' \
  -g '!**/test_*.py' 'respondida_em|resposta_colab' .
```

- **`PerguntaDisputa.save`** é um ponto genérico adicional. Repassa ao ORM os campos escolhidos pelo chamador, testa a legalidade das duas colunas e pode persistir o par sem registrar `DecisaoPergunta`. A leitura da metade que fica no banco é feita sem `select_for_update`. Entre os fluxos de negócio encontrados, as mutações explícitas chegam pelos sítios do item 2. [app/chamados/models.py:3691] [app/chamados/models.py:3720] [app/chamados/tests/test_e3_porta_A_escritor_do_par.py:42]
- **Máscara de sombra.** `resposta_colab` entra no inventário `TEXTO` e é alterada por `setattr` e `_base_manager.bulk_update`. O script também mascara `ato_texto` de linhas existentes, no mesmo `atomic` da máscara, sem criar ato de resposta. [bin/sombra_regras_pessoais.py:85] [bin/sombra_regras_pessoais.py:142] [bin/sombra_mascara_pessoal.py:10] [bin/sombra_mascara_pessoal.py:16] [bin/sombra_mascara_pessoal.py:44] [bin/sombra_mascara_pessoal.py:57] [bin/sombra_mascara_pessoal.py:64]
- **Chamadores adicionais de A13.** Reabertura pela supervisão, devolução de absorvidas após rejeição de ausência e cura do limbo chegam à mesma escrita de A08. As atribuições na cópia da ressuscitação e no desfazer do carimbo ficam em memória. [app/chamados/services/acoes_disputa.py:106] [app/chamados/views.py:1432] [app/chamados/services/materializacao.py:1202] [app/chamados/management/commands/curar_perguntas_em_limbo.py:70] [app/chamados/models.py:1569] [app/chamados/models.py:1583] [app/chamados/models.py:3383]

### 4. Como `composta` lê e onde a coluna crua ainda é lida

`leitura(p)` reúne atos reais e fotos sintéticas de projeção sem linha correspondente. A foto de `RESPOSTA` usa o instante e o texto da coluna quando falta uma linha do mesmo tipo e instante. A sequência é ordenada por `ato_em`, com reais em ordem de PK antes das sintéticas no mesmo instante. `RESPOSTA` põe instante e texto vigentes. `RECUSA` e `REABERTURA` limpam ambos. `composta(p)` conserva o instante desse resultado e pega o texto diretamente de `p.resposta_colab`. [app/chamados/models.py:4276] [app/chamados/models.py:4285] [app/chamados/models.py:4303] [app/chamados/models.py:4333] [app/chamados/models.py:4360]

Leituras cruas localizadas no código de produção, com caminhos relativos a `app/chamados/` quando abreviados:

| Uso | Sítios atuais |
|---|---|
| **API e tela** | Ambos em `app/api/views.py:1163` e `app/api/views_mensageria.py:569`. Instante no retorno do POST em `app/api/views.py:2663`. Texto em `views_cobrar.py:293` e `templatetags/chamados_tags.py:139,166`. Templates em `app/templates/chamados/partials/modal_fio.html:219`, `_pergunta_card.html:32`, `_card_atendimento.html:106,114,141,143,151,157,185,394` e `_lote_preview.html:24`, todos no mesmo diretório. |
| **Juízes e filtros ORM** | Instante e texto no slot, na chave de memo e nas faces SQL em `juizes.py:353,365,857,1580,1588,1599`. Ambos para a porta do painel e para corrigíveis/fila de validação em `juizes.py:1752,1765,1772`. Texto em `juizes.py:1361,1800,1947,2051,2073`. Consumidores das faces SQL incluem `models.py:1552,3622`, `canal.py:66`, `views.py:1412`, `services/acoes_disputa.py:98`, `services/disputa_emissao.py:1757`, `services/materializacao.py:146,2129` e `app/ponto/management/commands/reconciliar_grade.py:461`. |
| **Painel, métricas e histórico** | Ambos para crédito do painel em `services/raiox.py:138,147`. Instante em `services/painel.py:67`, `services/verbos.py:335,344` e `veredito_lavrado.py:207`. Texto em `services/historico.py:111`, `services/fio.py:380`, `services/explicador.py:55,102`, `app/ponto/services/dia_do_colab.py:70` e `app/ponto/services/veredito_celula.py:197`. |
| **Escritores, validação e materialização** | Ambos nas guardas e fotos em `models.py:1567,3346,3492,3730,4276`, `services/disputa_emissao.py:1641,1654,1713,1884,2109`, `services/validacao.py:406,450,930` e `services/materializacao.py:1188`. Texto em `services/disputa_emissao.py:1683,1721,2114`, `services/acoes_disputa.py:357`, `services/declaracao_texto.py:419`, `services/validacao.py:175,295,372,483,1202` e `services/materializacao.py:562,590,641,687,694,734,749,786,971,1041,1080,1147,1408,1590,1616,1625,1677`. Instante na idade da fila em `services/validacao.py:1275`. |
| **Vigias, fotos e comandos operacionais** | `services/vigia_texto_resposta.py:23`, `services/vigia_fio.py:635,749,752`, `services/foto_e4b.py:180,251,272`, `services/passivo_validado.py:64,83`, `management/commands/backfill_decisoes_pergunta.py:57,353`, `management/commands/estoque_l130_instante_sem_texto.py:37,58,97`, `management/commands/curar_perguntas_em_limbo.py:32,47,70`, `management/commands/auditar_invariantes_chamados.py:1116,1587`, `management/commands/bug_par_103.py:73`, `management/commands/validar_classe_a.py:63` e `bin/censo_folha.py:28`. |

### 5. Instrumentos de comparação e selos

| Instrumento | Cobertura |
|---|---|
| **`DecisaoPergunta.par_ato_coluna`** | Compara dois booleanos de presença de `validada_em`, um da composta e outro da coluna. Seu retorno deixa de fora valores exatos, `respondida_em` e `resposta_colab`. [app/chamados/models.py:4365] [app/chamados/models.py:4371] |
| **`vigia_ato_x_coluna.contar`** | Conta as duas direções da divergência de presença da validação, com esperado zero. Sua entrada de comparação é exclusivamente o par acima. [app/chamados/services/vigia_ato_x_coluna.py:8] [app/chamados/services/vigia_ato_x_coluna.py:16] |
| **Selo do par ato–coluna** | O campo-alvo do AST é `validada_em`. Confere compensador para limpeza e, para preenchimento, ato físico pareado ou guarda do carimbo sintético. Considera `RESPOSTA` pelo efeito sobre a validação. A escrita do instante e do texto da resposta fica fora desse pareamento. [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:22] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:397] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:455] |

Para **texto**, `vigia_texto_resposta.contar` compara literalmente `leitura(p).resposta_colab` com `p.resposta_colab or ''`. O resultado vai ao carimbo de `vigiar_fio_morto` com esperado zero. A comparação deixa de fora o instante e pode dar igualdade com uma linha sintética ou com texto vazio dos dois lados, como no caso D3 com texto `''`. [app/chamados/services/vigia_texto_resposta.py:11] [app/chamados/services/vigia_texto_resposta.py:23] [app/chamados/management/commands/vigiar_fio_morto.py:105] [app/chamados/management/commands/vigiar_fio_morto.py:282] [app/chamados/models.py:4285] [app/chamados/services/validacao.py:452]

Para **as duas colunas**, há selo AST dos sítios de escrita, guarda de legalidade do par e vigia de instante com texto vazio. Eles cobrem localização da escrita ou coerência entre as colunas. Um comparador, contador ou selo equivalente para o **instante do ato versus o instante da coluna** ficou `[nao verificado]`. [app/chamados/tests/test_e3_porta_A_escritor_do_par.py:234] [app/chamados/services/fio.py:1704] [app/chamados/services/vigia_fio.py:740]

### NAO VERIFICADO

- Identidade Git da cópia. O diretório se apresenta pelo hash informado, mas `git rev-parse HEAD` não encontrou metadados Git. Numeração dos mapas conferida sobre o texto raspado, sem os arquivos originais. `[nao verificado]`
- Execução, frequência ou ocorrência dessas corridas em produção, valores atuais das colunas e resultados dos vigias. As corridas descritas são inferências da ordem do código. `[nao verificado]`
- Exaustividade para SQL dinâmico, nomes montados em tempo de execução, gravações externas à árvore e leitores por reflexão que escapem à varredura. Execução da suíte e testes com banco. `[nao verificado]` [nao verificado]

## 20/11 13:48 · sessao d761a5768498 · CENSO-DE-ESCRITORES-E4C · censo · origem=Code

### B2b F0

| Q | Censo |
|---|---|
| **Q1** | **Nenhuma troca de coluna por ato identificada.** O recorte registrado é de instrumentação da cerca. Hoje a guarda retorna fora de cerca e, dentro dela, lê `conversa` e `data_evento` gravados, o dia da instância e a empresa da disputa. O registro da F0 descreve mover a coleta dos dias, preparar coletor e `RETIDA` e estender a trilha de retenção. A redação final do corte 13:35 [nao verificado]. `app/docs/BACKLOG.md:2033`, `app/docs/RELATO.md:66`, `app/chamados/services/fio.py:1402`, `app/chamados/services/fio.py:1409` |
| **Q2** | Para os **insumos** identificados, `data_evento` tem uma atribuição em `gravar_dia_do_fato` e três caminhos de persistência, nascimento, costura e rechaveamento. Nenhum grava `DecisaoPergunta` pelo dia. O rechaveamento tenta `LogAuditoria` na mesma transação. `conversa` é gravada no nascimento, sem ato próprio. A migration 0051 adiciona `conversa` com `False` e atualiza `colaborador_id` e `instancia_marco` do modelo histórico. `app/chamados/services/fio.py:839`, `app/chamados/services/fio.py:928`, `app/chamados/services/fio.py:976`, `app/chamados/services/fio.py:1086`, `app/chamados/services/fio.py:1095`, `app/chamados/models.py:3206`, `app/chamados/migrations/0051_decisao_pergunta.py:25`, `app/chamados/migrations/0051_decisao_pergunta.py:98` |
| **Q3** | O par da F5 mede presença de validação. A coleta dos dias, a cerca e a trilha de retenção ficam fora desse par. `app/chamados/models.py:4295`, `app/chamados/services/fio.py:1389`, `app/chamados/reconciliador.py:465`. Definição do par no patch fornecido, presença na árvore [nao verificado]. |
| **Q4** | Para `data_evento`, a atribuição já é única. A F0 descrita extrai a leitura da guarda. Uma eventual consolidação dos três pontos de persistência exige um destino de leitura que o recorte disponível não define [nao verificado]. A classe `OsTresCaminhosPassamPeloPontoTest` já prende nascimento, reuso e costura. `app/chamados/services/fio.py:839`, `app/docs/RELATO.md:66`, `app/chamados/tests/test_e1_v15d_um_ponto_de_escrita.py:182` |

### B2b F1

| Q | Censo |
|---|---|
| **Q1** | [nao verificado]. O registro confirma dez sítios de F1, mas remete a `F1/linhas_1911.md` para identificá-los. Faltam a lista e a fonte de destino de cada leitor. `app/docs/RELATO.md:89`, `app/docs/BACKLOG.md:827` |
| **Q2** | [nao verificado] por campo da fatia. A guarda de `save` e a porta de lote são verificáveis. A atribuição dos dez sítios depende da lista citada. `app/chamados/models.py:3448`, `app/chamados/models.py:3674`, `app/docs/RELATO.md:89` |
| **Q3** | [nao verificado] por campo. Falta o cruzamento dos dez sítios com os campos do par. `app/docs/RELATO.md:89`, `app/chamados/models.py:4295` |
| **Q4** | [nao verificado]. Faltam os sítios de F1 e os testes associados a cada um para delimitar arquivos e custo. `app/docs/RELATO.md:89` |

### B2b F2

| Q | Censo |
|---|---|
| **Q1** | [nao verificado]. A leitura registra A8 como dependência da **F2-P2 da espinha** e registra que a B2b precisa decidir a recusa pela cerca. Falta o recorte que atribui A8 ou outros sítios à **B2b F2** e nomeia uma troca de leitor. `app/docs/RELATO.md:251`, `app/docs/RELATO.md:559` |
| **Q2** | **A8, confirmado como sítio da B2b.** Escreve `validada_em/por`, `respondida_em`, `resposta_colab`, `materializada_em/por` e `via_resolucao` por `descarimbar_em_lote`. **Pareado** com `DecisaoPergunta.REABERTURA(da_ressuscitacao=True)` no mesmo `atomic`, para o conjunto relido sob trava. A atribuição à F2 da B2b [nao verificado]. `app/chamados/models.py:1627`, `app/chamados/models.py:1650`, `app/chamados/models.py:1694`, `app/chamados/models.py:1702`, `app/chamados/models.py:3485` |
| **Q3** | O selo fornecido inclui A8 para `validada_em`. A sonda da F5 recorta a sequência na **cópia em memória**. O pareamento persistido de A8 já existe na árvore. A execução do selo da cópia [nao verificado]. `app/chamados/models.py:1572`, `app/chamados/models.py:1688`, `app/chamados/models.py:1700` |
| **Q4** | A8 é um dos encaminhamentos a preservar se os campos forem centralizados. Seus testes prendem a reabertura por pergunta, a segunda execução e a releitura sob trava. Custo específico de B2b F2 [nao verificado] sem o recorte. `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:165`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:192`, `app/chamados/tests/test_f2_ressuscitar_tarefa_igual.py:215` |

### B2b F3

| Q | Censo |
|---|---|
| **Q1** | [nao verificado]. Falta a correspondência entre os sítios enumerados no registro da B2b e a F3. A menção à F3 da espinha em `marcos_do_lote` tem escopo próprio. `app/docs/RELATO.md:251`, `app/chamados/veredito_lavrado.py:151` |
| **Q2** | O B6 do inventário geral faz `bulk_update` sem passar por `PerguntaDisputa.save`. Escreve a lavra, pelo comando de backfill, e o lote não registra `DecisaoPergunta` correspondente. **Não pareado** para a lavra. Atribuição de B6 à B2b F3 [nao verificado]. `app/chamados/veredito_lavrado.py:250`, `app/chamados/management/commands/backfill_veredito_lavrado.py:143`, `app/docs/RELATO.md:567` |
| **Q3** | A lavra do B6 fica fora do selo de presença de validação. A cobertura dos campos efetivamente migrados pela B2b F3 [nao verificado]. `app/chamados/veredito_lavrado.py:250`, `app/chamados/models.py:4295`, `app/docs/RELATO.md:251` |
| **Q4** | [nao verificado]. A lista da fatia é necessária para saber se o backfill entra no corte e quais outros escritores da lavra entram com ele. `app/docs/RELATO.md:251`, `app/chamados/veredito_lavrado.py:250` |

### B2b F4

| Q | Censo |
|---|---|
| **Q1** | O vínculo verificável é com `registrar_retencao` e o dedup da **trilha**. Troca de leitor de coluna da pergunta [nao verificado]. Falta o recorte da F4 que diga se há algum leitor além dessa trilha. `app/docs/RELATO.md:66`, `app/chamados/reconciliador.py:465` |
| **Q2** | `registrar_retencao` escreve trilha de retenção via `evento`, cujo modelo é `LogAuditoria`. O escritor de trilha é best-effort. Campo de projeção atribuído à F4 [nao verificado]. `app/chamados/reconciliador.py:465`, `app/core/observ.py:94`, `app/core/observ.py:133` |
| **Q3** | A trilha de retenção fica fora do par A/B e da escrita de `DecisaoPergunta`. `app/chamados/reconciliador.py:465`, `app/chamados/models.py:4329` |
| **Q4** | [nao verificado] para coluna de projeção. O registro da F0 cita seis chamadores vivos da trilha, o que dimensiona o contrato de `registrar_retencao`, sem definir uma migração de leitores na F4. `app/docs/RELATO.md:66`, `app/chamados/reconciliador.py:465` |

### B2b F5

| Q | Censo |
|---|---|
| **Q1** | [nao verificado]. Falta o trecho de `BRIEF-FATIAS.md` que associa sítios e fontes à **B2b F5**. A obra `E4C-F5-CONSTROI` tem registro próprio. `app/docs/BACKLOG.md:827`, `app/docs/BACKLOG.md:2027` |
| **Q2** | [nao verificado] quais escritores do inventário abaixo pertencem à fatia. A matriz geral reúne sítios da B2b sem esse recorte. `app/docs/RELATO.md:251`, `app/docs/RELATO.md:567` |
| **Q3** | [nao verificado] o cruzamento com a B2b F5. O patch de `E4C-F5-CONSTROI` tem cobertura de `validada_em` descrita na tabela F8. `app/docs/BACKLOG.md:2027`, `app/chamados/models.py:4295` |
| **Q4** | [nao verificado]. Faltam campos, sítios e respectivos testes da B2b F5. `app/docs/BACKLOG.md:827` |

### B2b F6

| Q | Censo |
|---|---|
| **Q1** | **Leitura identificada:** `conversa` e `data_evento` da linha gravada, mais o dia de turno da instância, hoje lidos inline pela guarda → coleta extraída para uma função chamada pela porta, conforme o registro da F0. Migração desses campos para ato e função correspondente ao sítio X15 [nao verificado]. `app/chamados/services/fio.py:1409`, `app/chamados/juizes.py:338`, `app/docs/RELATO.md:66`, `app/docs/RELATO.md:251` |
| **Q2** | Para `data_evento` e `conversa`, valem os pontos da F0. `abrir` fornece dia de turno, `abrir_conversa` fornece dia civil e o juiz devolve `None` para conversa. `app/chamados/models.py:2963`, `app/chamados/models.py:2981`, `app/chamados/services/fio.py:839`, `app/chamados/juizes.py:338` |
| **Q3** | A leitura e a persistência desses dois campos ficam fora do par `validada_em` da F5. `app/chamados/services/fio.py:1409`, `app/chamados/models.py:4295` |
| **Q4** | Os três caminhos de dia já convergem na atribuição de `gravar_dia_do_fato`. A menor extração descrita mexe no bloco da guarda em `fio.py`, **1 bloco e 1 arquivo de produção**. Classe de teste que depende do bloco inline e escopo adicional de X15 [nao verificado]. `app/chamados/services/fio.py:839`, `app/chamados/services/fio.py:1409`, `app/docs/RELATO.md:66`, `app/docs/RELATO.md:251` |

### B2b F7

| Q | Censo |
|---|---|
| **Q1** | O recorte verificável é de **guarda nas portas humanas**, incluindo recusar, conferir data e aceitar folga. Conferir consulta `data_do_fato_de`, recusa sem dia segundo o corte registrado e grava `data_conferida_em/por`. A materialização e o modal leem esses carimbos. O leitor de destino por campo, inclusive eventual troca por ato, [nao verificado]. `app/docs/RELATO.md:231`, `app/docs/RELATO.md:330`, `app/docs/RELATO.md:358`, `app/chamados/services/validacao.py:990`, `app/chamados/services/materializacao.py:169`, `app/templates/chamados/partials/modal_fio.html:170` |
| **Q2** | **Conferência:** um `save` explícito para os dois campos em `conferir_data_do_fato`, **não pareado**. `LogAuditoria.conferir_data` é chamado depois do `save`, sem `atomic` abrangente na porta e com falha tolerada. O autor ainda tem `SET_NULL` na exclusão do usuário, sem ato. **Recusa, folga e demais portas humanas:** os escritores de resposta, validação e encerramento estão no inventário abaixo. A lista final das portas da F7 [nao verificado]. `app/chamados/services/validacao.py:996`, `app/chamados/services/validacao.py:999`, `app/core/auditoria.py:17`, `app/chamados/models.py:2645`, `app/chamados/services/validacao.py:519`, `app/chamados/services/validacao.py:560`, `app/chamados/services/validacao.py:922`, `app/docs/RELATO.md:421` |
| **Q3** | A F5 observa `validada_em` dos caminhos de validação e recusa. Ficam fora os dois carimbos de conferência, a atomicidade do `LogAuditoria` e a regra de competência dessas portas. `app/chamados/services/validacao.py:173`, `app/chamados/services/validacao.py:925`, `app/chamados/services/validacao.py:998`, `app/chamados/services/fio.py:1424` |
| **Q4** | Para a conferência já há **um escritor explícito**. Se o destino da leitura for a auditoria, o menor corte é garantir carimbo e trilha no mesmo `atomic` em `conferir_data_do_fato`, **1 sítio, 1 arquivo de produção e 1 classe de teste identificada**, `PortaValidacaoPerguntaTest`. A política para o `SET_NULL` do autor e a fonte de destino [nao verificado]. `app/chamados/services/validacao.py:983`, `app/chamados/services/validacao.py:994`, `app/chamados/models.py:2646`, `app/chamados/tests/test_porta_validacao_pergunta.py:185`, `app/chamados/tests/test_porta_validacao_pergunta.py:309` |

### E4C-F8

| Campo | Q1 · Leitor atual → destino descrito | Q2 · Escritores | Q3 · Cobertura do patch F5 | Q4 · Menor corte de escrita antes da troca |
|---|---|---|---|---|
| `resposta_colab` | U5 copia a coluna para o texto do `SILENCIO` e U9 a exibe. U3, U6, U7 e U8 hoje passam pelo juiz Python ou SQL, que ainda lê a coluna nesses caminhos. U7 também usa `respostas_sem_veredito`, cuja seleção já combina resposta e validação da sequência com `materializada_em` da coluna. Destino do brief, resposta vigente da sequência com conservação. A face pública e o contrato final para esses consumidores [nao verificado]. `app/ponto/services/veredito_celula.py:195`, `app/ponto/services/dia_do_colab.py:70`, `app/ponto/services/cartorio.py:1688`, `app/colaboradores/services/calendario.py:499`, `app/colaboradores/services/ficha.py:169`, `app/folha/previa.py:142`, `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1580`, `app/chamados/models.py:2003`, `app/chamados/models.py:4295` | Inventário R1–R6 abaixo, com R6 escrevendo só o instante. `app/chamados/services/disputa_emissao.py:1529`, `app/chamados/models.py:1702` | `composta` lê o texto da coluna. O patch liga um contador de divergência de texto ao vigia, cujo corpo não veio no recorte [nao verificado]. O selo A/B testa apenas `validada_em`. `app/chamados/models.py:4295`, `app/chamados/management/commands/vigiar_fio_morto.py:190` | Centralizar texto **junto com o instante** numa porta transacional, com releitura sob trava e registro do ato correspondente. Encaminhar resposta humana, foto, cura pela trilha, reabertura, ressuscitação, L-130 e o preenchimento sintético de `carimbar`. Estimativa **7 encaminhamentos + 1 porta, 3 arquivos de produção, pelo menos 7 classes**. Os contratos que prendem os caminhos avulsos estão abaixo. `app/chamados/services/disputa_emissao.py:1529`, `app/chamados/services/disputa_emissao.py:1656`, `app/chamados/services/disputa_emissao.py:1685`, `app/chamados/services/disputa_emissao.py:1707`, `app/chamados/models.py:1702`, `app/chamados/models.py:3328`, `app/chamados/services/validacao.py:440` |
| `respondida_em` | Dependência do juiz em U3, U6, U7, U8 e do filtro `q_vivas` em U5. Hoje a face SQL ainda usa a coluna. Para os caminhos de julgamento que migrarem, o destino correspondente é o instante da resposta vigente. O brief antigo enumera principalmente texto. Seu recorte final para o instante e para `q_vivas` [nao verificado]. `app/chamados/juizes.py:1552`, `app/chamados/juizes.py:1580`, `app/chamados/models.py:3597`, `app/ponto/services/veredito_celula.py:181`, `app/chamados/models.py:4318` | R1–R6 e C4–C7 quando `carimbar` recebe `resposta` e completa um instante faltante. `app/chamados/models.py:3326`, `app/chamados/services/disputa_emissao.py:1707` | `composta` entrega o instante da sequência aos pontos alterados. A/B não avalia o par texto/instante nem detecta ordem incorreta entre duas respostas. `app/chamados/models.py:4277`, `app/chamados/models.py:4295`, `app/chamados/models.py:4314`. Cobertura de outros hunks da F5 [nao verificado]. | Mesmo corte do texto. O ramo `resposta='responder'` também precisa passar pela porta. `app/chamados/models.py:3326`, `app/chamados/services/disputa_emissao.py:2103`, `app/chamados/tests/test_o261_resposta_vazia.py:78` |
| `validada_em` | U1 lê o instante para procedência. U2 usa `validada_em__gte` no SQL. Destino descrito, instante da validação vigente pela sequência conservada. `q_validada_pelo_ato` já responde **presença**, enquanto U2 precisa comparar **instante** com o corte. `app/ponto/services/procedencia_batida.py:74`, `app/ponto/services/flip_auto.py:130`, `app/chamados/models.py:3539`, `app/chamados/models.py:4318` | Nove caminhos de estado, V1–V3, R1, R4, R5, C4, C6 e C7. `app/chamados/services/validacao.py:168`, `app/chamados/models.py:3330` | Cobertura direta de A/B, **presença** do resultado de `leitura` contra presença da coluna. Cobre o efeito lido de A8 e a inclusão de `validada_em` no AST dos outros caminhos. Ficam fora a igualdade de instantes e a existência de linha física em carimbo sintético conservado. A guarda de `carimbar` e a correção da reabertura comum têm as lacunas descritas abaixo. `app/chamados/models.py:3330`, `app/chamados/models.py:4251`, `app/chamados/models.py:4312` | Uma porta para escrever o par de validação, recebendo preenchimento e limpeza sob a mesma trava do ato. Redirecionar **6 pontos de decisão persistente + 1 porta em 3 arquivos**, `models.py`, `validacao.py` e `disputa_emissao.py`. Os três chamadores sintéticos convergem em `carimbar`. Piso **6 classes** identificadas abaixo. `app/chamados/models.py:1702`, `app/chamados/models.py:3348`, `app/chamados/services/validacao.py:171`, `app/chamados/services/validacao.py:276`, `app/chamados/services/validacao.py:351`, `app/chamados/services/disputa_emissao.py:1529`, `app/chamados/services/disputa_emissao.py:1656` |
| `validada_por` | U1 lê o autor da coluna. O destino descrito é o autor da validação vigente. Há exemplo local de pegar `ato_por_id` da última `VALIDACAO` do instante, na foto de A8. `LeituraDoAto` não expõe o autor, e o contrato público para U1 [nao verificado]. `app/ponto/services/procedencia_batida.py:71`, `app/chamados/models.py:19`, `app/chamados/models.py:1688`, `app/chamados/models.py:4169` | Mesmos nove caminhos do instante e o `SET_NULL` da FK. `app/chamados/models.py:3332`, `app/chamados/models.py:3631`, `app/chamados/services/validacao.py:169` | Fora do A/B. A foto feita em A8 é local à reabertura, e o tipo de leitura da árvore não devolve o autor. `app/chamados/models.py:19`, `app/chamados/models.py:1689` | Mesmo corte de `validada_em`, preservando o autor do passivo e tratando o `SET_NULL` como operação do ORM. A procedência tem teste com teto de uma consulta para cinco batidas. A solução e o custo para impor um único ponto inclusive na exclusão de `User` [nao verificado]. `app/chamados/models.py:3631`, `app/chamados/models.py:4265`, `app/ponto/tests/test_procedencia_batida.py:117` |
| `materializada_em`, `materializada_por` | U1 usa o par da coluna como alternativa ao par de validação. U5 usa o instante acabado de gravar para datar o `SILENCIO`. O brief conserva C em U1 por falta de fonte para o par completo. U5 cruza a escrita do ato no mesmo relógio. Destino adicional para C [nao verificado]. `app/ponto/services/procedencia_batida.py:71`, `app/ponto/services/veredito_celula.py:190`, `app/ponto/services/veredito_celula.py:196`, `app/chamados/models.py:19` | C1–C23 e `SET_NULL` para o autor. `app/chamados/models.py:3293`, `app/chamados/models.py:3485`, `app/chamados/models.py:3651` | `composta` transporta `materializada_em` da coluna. O autor não entra no tipo nem no selo. O patch preserva a foto de C nesses leitores. `app/chamados/models.py:19`, `app/chamados/models.py:3236`, `app/chamados/models.py:3647`. Definição de `composta` no patch, presença na árvore [nao verificado]. | Se C entrar em migração, o menor corte é concentrar a persistência do trio, preservando as decisões dos chamadores. **6 pontos persistentes + 1 porta, 3 arquivos de produção** para instante/autor. Com atualização avulsa de via, **8 + 1, 4 arquivos**. Piso de **7 classes** abaixo. A fonte integral para reconstruir C e o tratamento da FK [nao verificado]. `app/chamados/models.py:3348`, `app/chamados/models.py:3386`, `app/chamados/models.py:3408`, `app/chamados/models.py:3485`, `app/chamados/models.py:3497`, `app/chamados/services/disputa_emissao.py:1529`, `app/chamados/services/validacao.py:351`, `app/chamados/services/relavra_via.py:127` |
| `via_resolucao`, `recusa_motivo` | U2 filtra a via e extrai a batida vizinha do texto técnico. U4 verifica a via `duplicata`. **Nenhuma troca desses leitores está descrita no brief.** A `RECUSA` do admin tem outra origem. `app/ponto/services/flip_auto.py:129`, `app/ponto/services/flip_auto.py:132`, `app/ponto/services/cartorio.py:1698`, `app/chamados/services/validacao.py:916` | Via, C1–C25. Motivo, M1–M6. `app/chamados/models.py:3382`, `app/chamados/models.py:3496`, `app/chamados/services/materializacao.py:1699` | Via segue da coluna em `composta`. Motivo técnico fica fora do selo e do contador A/B. `app/chamados/models.py:2746`, `app/chamados/models.py:4295`, `app/ponto/services/flip_auto.py:132`. Cobertura da cópia além do recorte [nao verificado]. | Sem troca definida na F8. Se entrar em outra migração, via cabe no corte do trio acima. Para motivo, **6 origens → 1 porta, 4 arquivos de produção**, preservando a origem humana, técnica, de duplicata e de escape. Piso **3 classes** abaixo. Fonte comum dos motivos [nao verificado]. `app/chamados/services/validacao.py:916`, `app/chamados/services/materializacao.py:1365`, `app/chamados/services/materializacao.py:1699`, `app/chamados/services/fio.py:1502`, `app/chamados/services/disputa_emissao.py:1511` |

### E4C-F5-SILENCIO-ADMIN

| Q | Censo |
|---|---|
| **Q1** | O ponto verificado é **decisão de dono**. A tarefa hoje remo

[truncado em 20000 de 47997 caracteres]

## 20/11 13:47 · sessao 3f633be2914e · CRUZA-O-PRE-MAPA · leitura · origem=Code

## Q1 — O que os mapas já registravam

### `validada_em`

| Escritor e porta | Par com o ato | Linha do mapa |
|---|---|---|
| A10 `_carimbar_validacao`, chamada por `validar_pergunta`, preenche a validação. | Pareado com `VALIDACAO`. | `escritores.md:47`, `matriz.md:24`, `06_pergunta_fio.md:29` |
| A05 `PerguntaDisputa.carimbar(resposta='validar')` também preenche a validação. | O mapa registra carimbo de sistema sem ato próprio na porta. O rio da disputa chama esse sítio de segundo escritor positivo, fora de `_carimbar_validacao`. | `escritores.md:42`, `matriz.md:19`, `01_disputa.md:175`, `logs/PREPARA-ESPINHA/E4d/preparo.md:42,98` |
| A11 `devolver_validacao_recusada` limpa a validação e preserva a resposta. | Pareado com `DEVOLUCAO` quando havia validação. | `escritores.md:48`, `matriz.md:25`, `06_pergunta_fio.md:32` |
| A12 `responder_pergunta_disputa_core` pode limpar a validação anterior. | Pareado com `RESPOSTA`. O rio também registra evento da validação desfeita. | `escritores.md:49`, `matriz.md:26`, `06_pergunta_fio.md:26` |
| A13 `reabrir_para_correcao` limpa a validação por A08. A19 passa por A13 no fluxo de recusa. | `REABERTURA` por padrão, ou o ato recebido. Na recusa, `RECUSA` com foto. A síntese de escritores omite A13 para esse campo, mas a tabela de portas a registra. | `escritores.md:17,50`, `matriz.md:27,33`, `06_pergunta_fio.md:33–34` |
| D1 `devolver_paridade_bug_par_103`, em `validacao.py:349`, grava a limpeza diretamente. | Fora das portas da síntese para validação, com `DEVOLUCAO` no próprio sítio. | `escritores.md:17,70,116` |
| D2 `ressuscitar_perguntas`, em `models.py:1702`, passa a limpeza a A08 por `**outros`. | Fora das portas da síntese para validação, com `REABERTURA` marcada no PRE-MAPA. O rio já registrava a limpeza em lote e a trilha. | `escritores.md:17,72,117`, `06_pergunta_fio.md:45` |

### `respondida_em`

| Escritor e porta | Par com o ato | Linha do mapa |
|---|---|---|
| A12 `responder_pergunta_disputa_core` grava texto e instante. | Pareado com `RESPOSTA`. | `escritores.md:49`, `matriz.md:26`, `06_pergunta_fio.md:26` |
| A14 `responder_automaticamente` grava texto e instante. | Pareado com `RESPOSTA`, com motivo automático. | `escritores.md:51`, `matriz.md:28`, `06_pergunta_fio.md:27` |
| A15 `carimbar_resposta_pela_trilha` recupera o instante para texto já escrito. | Pareado com `RESPOSTA` e evento da recuperação. | `escritores.md:52`, `matriz.md:29`, `06_pergunta_fio.md:28` |
| A05 `carimbar` pode preencher o instante quando vazio. | O preparo registra que esse instante pode ser o do carimbo de sistema, sem `RESPOSTA` na porta. O rio registra a assinatura complementar e, no fecho pelo destino, a condição de haver texto. | `escritores.md:42`, `06_pergunta_fio.md:30,35,120`, `logs/PREPARA-ESPINHA/E4d/preparo.md:42,96` |
| A13 `reabrir_para_correcao` limpa instante e texto por A08. A19 delega a recusa a A13. | Pareado com `REABERTURA` ou o ato recebido, como `RECUSA`. | `escritores.md:50`, `matriz.md:27,33`, `06_pergunta_fio.md:33–34` |
| D2 `ressuscitar_perguntas`, em `models.py:1702`, limpa instante e texto por A08. | Fora das portas da síntese para resposta, com `REABERTURA` marcada no PRE-MAPA. | `escritores.md:16,72`, `06_pergunta_fio.md:45` |
| D3 `devolver_instante_sem_texto_l130`, em `validacao.py:440`, limpa o par e o carimbo por A08. | Sem ato de devolução ou reabertura no fluxo mapeado. | `escritores.md:16,73,118`, `perguntas.md:86` |

O nascimento A22 aceita `**campos`. O rio registra que assinaturas recebidas viram decisões. O mapa não diz quais campos de resposta ou validação cada chamador fornece. A contagem L2 de zero fugas nos sítios auditados do rio refere-se ao conjunto de portas de carimbo e via. (`escritores.md:59`, `matriz.md:36`, `06_pergunta_fio.md:24–25,115–120`)

**O achado da F5 já estava escrito quanto à existência de vários escritores de `validada_em`, inclusive o segundo preenchimento positivo em `carimbar`; faltavam a prova de equivalência de presença ao fim da escrita e os casos concretos do carimbo sintético diante de `RESPOSTA` posterior e da reabertura de pergunta validada só pelo ato.** (`ficha_cruza_o_pre_mapa_2011_1400.txt:3`, `escritores.md:17`, `01_disputa.md:175`, `f5_diff_porta_selo_contador.patch:821–864`)

## Q2 — Campos por fatia

**B2b F0**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| Sem campo com troca de leitor descrita. Os quatro instrumentos da cerca entram desligados. | `backlog_linhas.txt:827,2055` | — | — |

**B2b F1**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material | `backlog_linhas.txt:827` aponta o brief. `app/docs/RELATO.md:89` registra dez sítios, sem enumerar seus campos. | [nao verificado] | — |

**B2b F2**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material | `backlog_linhas.txt:827` | [nao verificado] | — |

**B2b F3**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material | `backlog_linhas.txt:827` | [nao verificado] | — |

**B2b F4**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material | `backlog_linhas.txt:827,2055` cita a trilha no contexto dos instrumentos da F0. | [nao verificado] | — |

**B2b F5**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material | `backlog_linhas.txt:827` | [nao verificado] | — |

**B2b F6**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material | `backlog_linhas.txt:827,2055` cita dias e guarda no contexto da F0. | [nao verificado] | — |

**B2b F7**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material para troca de leitor | `backlog_linhas.txt:827`, `app/docs/RELATO.md:330,358` registram o corte das portas humanas e a discussão das guardas. | [nao verificado] | — |

**E4C-F8**

O BACKLOG menciona nove unidades. Os recortes permitem identificar os campos abaixo, com ressalva sobre a origem do texto e as exceções de materialização. (`backlog_linhas.txt:2056`, `logs/PREPARA-ESPINHA/E4c/preparo.md:469,477–478,492,496–498`, `app/docs/RELATO.md:819`)

| Campo | Fonte que identifica a troca | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| `respondida_em` | O teste da F8 troca a entrada de `_tem_resposta_do_colab` pela resposta do ato. Na cópia, o cartório passa a pergunta ORM ao juiz, que lê o instante e o texto. **[LEITURA NOVA: motivo (ii)]** `cartorio.py:1684–1689`. **[LEITURA NOVA: motivo (i)]** `juizes.py:1544–1552`. | Pareado com o ato em A12/A13/A14/A15. Fora da porta em D2, com `REABERTURA`. Sem ato em D3 e sem ato próprio no preenchimento de sistema pela A05. | `logs/PREPARA-ESPINHA/E4c/preparo.md:496`, `escritores.md:16,42,49–52,72–73`, `matriz.md:26–29`, `logs/PREPARA-ESPINHA/E4d/preparo.md:96` |
| `resposta_colab` | As unidades migram o leitor da resposta. O preparo testa texto vindo do ato, inclusive no cartório. O corte registrado conserva o texto corrente para leitores e SILENCIO. O mapa não diz como conciliar esse teste com o corte em cada uma das nove unidades. | Pareado com o ato em A12/A13/A14. Fora da porta em D2, com `REABERTURA`. Sem ato em D3. | `logs/PREPARA-ESPINHA/E4c/preparo.md:492,496,498`, `app/docs/RELATO.md:819`, `escritores.md:16,49–51,72–73`, `matriz.md:26–28` |
| `validada_em` | `procedencia` passa a DP + efeito Batida. `fila_flip_recusas` indica DP/T para a leitura de validação. | Pareado com o ato em A10/A11/A12/A13. Fora da porta em D1/D2, ambos com ato. A05 preenche em ramo sem ato próprio. | `logs/PREPARA-ESPINHA/E4c/preparo.md:477–478`, `escritores.md:17,42,47–50,70,72`, `matriz.md:19,24–27` |
| `validada_por` | `procedencia` lê o autor do ato, com teste em que o autor da coluna difere do autor de `VALIDACAO`. | Mesmos sítios do instante de validação, com os mesmos pares e a mesma exceção A05. | `logs/PREPARA-ESPINHA/E4c/preparo.md:477,497`, `escritores.md:17,42,47–50,70,72`, `matriz.md:19,24–27` |
| `materializada_em`, `materializada_por` | `procedencia` lista ambos na entrada da leitura com destino DP + efeito Batida e referências a D03, A05 e A06. O mapa não diz qual ramo abandona cada coluna. | Fora da porta na síntese em D1–D4, por A07/A08. D1 e D2 têm ato. D3 é sem ato. A equivalência de D4 com um ato é [nao verificado]. A05/A06 escrevem o carimbo, com ato próprio [nao verificado] na matriz. | `logs/PREPARA-ESPINHA/E4c/preparo.md:477`, `escritores.md:15,116–119`, `matriz.md:19–22,26–27` |

`via_resolucao` e `recusa_motivo` aparecem na leitura de `fila_flip_recusas`, junto das referências A06/A18. O mapa não diz que a F8 troca a fonte desses dois campos. (`logs/PREPARA-ESPINHA/E4c/preparo.md:478`, `matriz.md:20,32`)

**E4C-F5-SILENCIO-ADMIN**

| Campo | Fonte do escopo | Classe dos escritores | Linha do mapa |
|---|---|---|---|
| escopo nao definido no material para troca de leitor | `backlog_linhas.txt:1966` registra a separação do SILENCIO com dono admin e o recorte das ressuscitadas. O rio descreve o carimbo `admin_decidiu` e o ato `SILENCIO` da operação existente. | [nao verificado] para os campos que a fatia migra | `06_pergunta_fio.md:39`, `matriz.md:16,20` |

## Q3 — Cobertura pelo que a F5 deixa na cópia dela

O **selo do par** examina por AST as formas de escrita que reconhece para `validada_em`. O **contador A/B** compara dois booleanos depois da leitura composta. A conta A é ato validado com coluna vazia. B é coluna preenchida com ato sem validação vigente. O patch põe ambos no carimbo de estado com esperado zero. A guarda em `carimbar` segue por construir, e o patch traz o teste que exige preservar a equivalência no carimbo sintético diante de `RESPOSTA` posterior. (`f5_diff_porta_selo_contador.patch:22–28,173–199,223–248,307–349,821–838`, `backlog_linhas.txt:2046`) [nao verificado]

| Campo da Q2 | Selo do par | Contador A/B | Guarda em `carimbar`, quando existir |
|---|---|---|---|
| `validada_em` | Vê preenchimentos, limpezas e formas em lote reconhecidas. O selo exige o pareamento aplicável à escrita e aceita a exceção sintética identificada no teste. A cobertura de formas dinâmicas além das reconhecidas é [nao verificado]. | Vê divergência de presença nos dois sentidos. Igualdade de instante, autor e identificação do escritor ficam fora do resultado booleano. | O teste especifica a equivalência de presença para o ramo sintético `resposta='validar'` diante de resposta posterior. A implementação e seu alcance além desse cenário são [nao verificado]. | [nao verificado]
| `validada_por` | O campo-alvo do teste é `validada_em`. O autor fica fora da verificação. | A comparação usa só presença de `validada_em`. | O teste da guarda verifica presença da validação. O mapa não diz qual contrato a guarda terá para o autor. | [nao verificado]
| `respondida_em` | A sequência de resposta pode afetar a validação checada pelo selo. O instante da coluna de resposta fica fora das escritas verificadas. | A leitura composta carrega o instante do ato. A/B compara somente presença da validação. | A `RESPOSTA` posterior é a condição do teste de validação sintética. O teste deixa sem verificação o pareamento do instante de resposta com sua coluna. | [nao verificado]
| `resposta_colab` | O texto fica fora do campo-alvo. | A leitura composta carrega o texto da projeção. A/B deixa o texto fora da comparação. | O cenário usa resposta com texto. O teste deixa a equivalência de texto fora da asserção. | [nao verificado]
| `materializada_em`, `materializada_por` | As escritas de materialização ficam fora do campo-alvo. | `composta` carrega `materializada_em` da coluna. A/B deixa ambos os campos fora da comparação e `materializada_por` fica fora de `LeituraComposta`. | O cenário exercita um carimbo. A asserção verifica o par da validação. O mapa não diz qual contrato a guarda terá para os campos de materialização. |

As distinções da tabela vêm do campo-alvo e da lista de sítios do teste, dos atributos retornados por `composta`, da comparação de presença e da asserção do carimbo sintético. O patch imprime a divergência de texto em métrica separada de A/B. (`f5_diff_porta_selo_contador.patch:38–48,62–74,173–199,223–248,307–349,821–838`) [nao verificado]

A cobertura direta é parcial e só para `validada_em`. O selo e o contador admitem vários escritores, desde que as escritas cobertas fechem o par. O teste da guarda trata um ramo de A05. A passagem a um escritor de projeção é o trabalho descrito na E4d. (`f5_diff_porta_selo_contador.patch:173–199,307–349,821–838`, `perguntas.md:9–13`, `backlog_linhas.txt:2041`) [nao verificado]

## Q4 — Recorte de escrita por campo

O desenho nomeado é **E4D-PROJECAO-UM-ESCRITOR**. Para os campos desta leitura, o mapa diz que a porta para de gravar a projeção e o projetor grava a coluna a partir dos atos. A tabela delimita os sítios que precisariam entrar em cada recorte de escrita antes de trocar leitores. O mapa não diz se cada recorte constitui, por si só, uma fatia independente e suficiente. (`perguntas.md:9–13,15`, `backlog_linhas.txt:2041`)

| Campo | Arquivos e sítios do recorte de escrita | Fatia ou pergunta já nomeada |
|---|---|---|
| `validada_em`, `validada_por` | A05 `models.py:3299,3324,3345`; A10/A11 `services/validacao.py:168,273`; A12/A13 `services/disputa_emissao.py:1494,1617,1639`, com persistência do lote `models.py:3485`; D1 `services/validacao.py:349`; D2 `models.py:1694,1702`. O recorte abrange o preenchimento humano e o sintético de A05, as limpezas com ato compensador e as delegações de A19/A23 registradas na matriz. | E4d, perguntas 17–18 e D1/D2 nas perguntas 30–31. O mapa não nomeia uma fatia autônoma só para o par. (`escritores.md:17,42,45,47–50,70,72`, `matriz.md:19,24–27,33,37`, `perguntas.md:54–56,80–82`) |
| `respondida_em`, `resposta_colab` | A12 `services/disputa_emissao.py:1481`; A13 no mesmo arquivo `:1617,1639` e A08 em `models.py:3485`; A14 `services/disputa_emissao.py:1682`; A05 `models.py:3299,3324` e A15 `services/disputa_emissao.py:1705` entram pelo instante. D2 `models.py:1694,1702` e D3 `services/validacao.py:414,440` entram pela limpeza. O recorte inclui a origem do instante complementar de A05 e a unificação do texto no ato registrada para a E4d. | E4d, perguntas 15–16, D2 na 31 e D3 na 33. **D3** tem brief próprio registrado e `SANEAMENTO` previsto para a limpeza L-130. O mapa não diz que D3, isolada, fecha os dois campos. (`escritores.md:16,42,49–52,72–73`, `matriz.md:19,26–29`, `perguntas.md:50–52,82,86`, `backlog_linhas.txt:2041`) |
| `materializada_em`, `materializada_por` | A05/A06 `models.py:3324,3378` para carimbo. A07/A08 `models.py:3399,3448,3485` para retirada. Os sítios D1 `services/validacao.py:346`, D2 `models.py:1702`, D3 `services/validacao.py:440` e D4 `services/relavra_via.py:246` entram pelas devoluções e retiradas que o mapa associa ao trio do carimbo. O recorte inclui os chamadores que delegam o carimbo ou a retirada e as exceções A05/A06. | E4d, perguntas 12–13 e D1–D4 nas perguntas 30–34. D3 é nomeada para seu fluxo. A equivalência do evento de D4 com um ato projetável permanece [nao verificado]. (`escritores.md:15,42–45,116–119`, `matriz.md:16,19–22,26–27,32–33,35,37`, `perguntas.md:44–46,80–88`, `backlog_linhas.txt:2041`) |

## Q5 — Mapas para os briefs

| Caminho | Material comprovado no recorte |
|---|---|
| `logs/PREPARA-ESPINHA/E4c/preparo.md` | Seção F8 com nove unidades de ponto/colaboradores/folha, contrato de saída e provas de cartório, autoria da validação e paridade do texto entre calendário, ficha, prévia e dia do colaborador. (`citacoes_b2b_f8.txt:1–7`, `logs/PREPARA-ESPINHA/E4c/preparo.md:469,492,496–498`) |

As duas ocorrências em `logs/PREPARA-ESPINHA-2/mapa-04/inventario.md` se referem a **F8.8**, no dedup de chamado noturno. Essas linhas não sustentam uma citação no brief da E4C-F8. O inventário registra três menções em `logs/PREPARACAO/censos/E4c/fatias.md`, mas o anexo de citações não traz essas linhas. O mapa não diz, no recorte entregue, o que esse arquivo oferece aos briefs de B2b ou E4C-F8. Não há citação positiva para B2b nos quatro diretórios. (`citacoes_b2b_f8.txt:1–9`, `logs/PREPARA-ESPINHA-2/mapa-04/inventario.md:22,53`, `inventario_e_delta.txt:186–189`)

### nao lido — arquivos integrais do inventário nos quatro diretórios da Q5

Os agrupamentos abaixo usam caminhos relativos ao prefixo da linha. Onde há um excerto no anexo, a íntegra do arquivo continua não lida. (`inventario_e_delta.txt:2–9,57–184`, `citacoes_b2b_f8.txt:1–9`)

| Prefixo | Arquivos |
|---|---|
| `logs/MADRUGADA-PREPARA/` | `aval.txt`, `cef.md`, `m1.r` a `m5.r`, `o50_backlog.txt`. (`inventario_e_delta.txt:2–9`) |
| `logs/PREPARA-ESPINHA/` | `CONTRATO-DE-ENTRADA-DO-FIO/preparo.md`, `E3-V10-ATAQUE-DONO/ataque.md`, `E3-V10-ATAQUE-ESCRITOR/ataque.md`, `E4c/preparo.md`, `E4d/preparo.md`, `LISTA-UNICA-DE-LEI.md`, `MAPA-DOS-RIOS/mapa.md`, `O50/preparo.md`. (`inventario_e_delta.txt:177–184`) |
| `logs/PREPARA-ESPINHA-2/` | `ataque-r3-c5/ataque.md`, `c6-sondas/k36_c6.txt`, `c6-sondas/k36_main.txt`, `c6-sondas/k36.md`, `c6-sondas/k36.r`, `c6-suite/fails.txt`, `c6-suite/t1.r`, `lista-unica-2/LISTA-UNICA-2.md` e `inventario.md` em cada pasta de `mapa-01/` a `mapa-14/`. (`inventario_e_delta.txt:155–176`) |
| `logs/PREPARACAO/censos/` | `arquivos_em_voo.txt`, `ATA-X-TURNO-PAUSA/censo.md`, `backlog_4obras.txt`, `BUG-ENC-CELULAS-FORMA/censo.md`, `COBPARC-FILA-RELOGIO/censo.md`, `CODEX-CONTEXTO-DA-CASA/censo.md`, `CONTRATO-DE-ENTRADA-DO-FIO.md`, `CONTRATO-DE-ENTRADA-DO-FIO/matriz_transicoes.md`, `CONTRATO-DE-ENTRADA-DO-FIO/r.r`, `DECLARADO-X-GRAVADO/censo.md`, `DECLARADO-X-GRAVADO/fechamento.md`. (`inventario_e_delta.txt:57–67`) |
| `logs/PREPARACAO/censos/E4c/` | `agg.py.txt`, `censo_ast.py.txt`, `censo.md`, `fatias.md`, `git_lines.txt`, `str_outro.txt`. (`inventario_e_delta.txt:68–73`) |
| `logs/PREPARACAO/censos/E4d/` | `censo_ast.py.txt`, `censo.md`, `w_ast.txt`, `w_git.txt`. (`inventario_e_delta.txt:74–77`) |
| `logs/PREPARACAO/censos/` | `HJ-091.md`, `HJ-091/r.r`, `HJ-091/wt.txt`, `JUIZES.md`, `JUSTIFICATIVA-PAGA-O-PERIODO.md`, `JUSTIFICATIVA-PAGA-O-PERIODO/matriz_transicoes.md`, `JUSTIFICATIVA-PAGA-O-PERIODO/r.r`, `LASTRO-DEGRAU-2/censo.md`, `O10-C/censo.md`, `O275/censo.md`, `OTIMIZA-1.md`, `OTIMIZA-1/r.r`, `PADRAO-DE-CIMA/censo.md`, `QUARENTENA-DO-COLAB/censo.md`, `RAIZ-DA-DISPUTA/censo_ast.py.txt`, `RAIZ-DA-DISPUTA/censo.md`, `SILENCIO-VENCIDO-VOLTA/censo.md`, `SUITE-POR-IMPACTO/censo.md`, `TIPO-NA-FONTE-ITEM3/censo_ast.py.txt`, `TIPO-NA-FONTE-ITEM3/censo.md`, `V3-E1/censo.md`, `V3-RETOMA-FILA/censo.md`, `V3-RETOMA-PERIODOS-DA-CELULA/censo.md`. (`inventario_e_delta.txt:78–100`) |
| `logs/PREPARACAO/` | `contratos/r_chamados.r`, `contratos/r_escala.r`, `contratos/r_ponto.r`, `lastro/backlog_lastro.txt`, `lastro/JUIZES.md`, `lastro/PRONTO.md`, `lastro/raias_lastro.txt`, `lastro/r.r`, `LEIS-ABERTAS/conferencia_E.md`, `LEIS-ABERTAS/lista.md`, `LEIS-ABERTAS/para_o_corte.md`, `LEIS-ABERTAS/proposta_linhas_893_959.md`, `O169-CELULA/segura.md`, `PARADAS-65/lista.md`, `perguntas_de_lei_1311.md`. (`inventario_e_delta.txt:101–115`) |
| `logs/PREPARACAO/triagem/anexos/` | `ATA-X-TURNO-PAUSA.txt`, `BUG-ENC-CELULAS-FORMA.txt`, `COBPARC-FILA-RELOGIO.txt`, `CODEX-CONTEXTO-DA-CASA.txt`, `JUSTIFICATIVA-PAGA-O-PERIODO.txt`, `LASTRO-DEGRAU-2.txt`, `O10-C.txt`, `O261-PAI-TERMINAL.txt`, `O275.txt`, `OTIMIZA-1.txt`, `PADRAO-DE-CIMA.txt`, `QUARENTENA-DO-COLAB.txt`, `SILENCIO-VENCIDO-VOLTA.txt`, `SUITE-POR-IMPACTO.txt`, `V3-E1.txt`, `V3-RETOMA-FILA.txt`, `V3-RETOMA-PERIODOS-DA-CELULA.txt`. (`inventario_e_delta.txt:116–132`) |
| `logs/PREPARACAO/triagem/g1/` | `OBRA-BUG-ENC-CELULAS-FORMA.txt`, `OBRA-CODEX-CONTEXTO-DA-CASA.txt`, `OBRA-PADRAO-DE-CIMA.txt`, `OBRA-QUARENTE

[truncado em 20000 de 23662 caracteres]

## 20/11 13:47 · sessao 2200198165d8 · RAIZ-DA-DISPUTA · leitura · origem=Code

MAPAS JA FEITOS CONSULTADOS: 01_disputa.md
**PAREI — RAIZ-DA-DISPUTA F1. O corte 2 precisa de uma regra escrita para identificar a discussão original quando chega um motivo novo, sem pergunta canônica, e o mesmo chamado aponta para mais de uma disputa encerrada.** `app/chamados/services/disputa_emissao.py:1084` · `app/chamados/services/fio.py:1948` · `app/chamados/models.py:2326`

A porta `reabrir_sistema` recebe a disputa já escolhida, reabre a mesma linha e deixa uma ocorrência quando há chamado-pai. A emissão comum procura uma disputa **aberta** do colaborador. Já o repasse pega a primeira pergunta não materializada do chamado e usa a dona dela, mesmo fechada. O teste `test_validar_pergunta_listada.py` monta duas discussões distintas ligadas ao mesmo chamado de furo. A FK de destino permite esse vínculo múltiplo. `app/chamados/models.py:2330` · `app/chamados/models.py:2340` · `app/chamados/services/disputa_emissao.py:540` · `app/chamados/services/disputa_emissao.py:1084` · `app/chamados/tests/test_validar_pergunta_listada.py:106` · `app/chamados/models.py:3639`

A eleição escrita com desempate pelo menor PK escolhe o **chamado de destino**. A canônica da pergunta é eleita dentro do **mesmo fato**, cujo universo inclui o motivo. Nenhuma dessas autoridades identifica qual discussão anterior é a original para um motivo novo sem canônica. Fazer o `.first()` decidir a reabertura daria força de regra à ordem de PK das perguntas. Regra escrita para esse caso e incidência em produção: [nao verificado]. `app/docs/PROMPTS.md:2085` · `app/chamados/services/fio.py:1274` · `app/chamados/services/fio.py:1957` · `app/chamados/models.py:3761`

Para o corte 1, há uma via de evento já existente: o consumidor da célula reavalia a premissa do chamado vivo e, quando ela morreu, usa a porta de reconciliação, que registra a retratação com o motivo. `app/chamados/reconciliador.py:1419` · `app/chamados/reconciliador.py:1504` · `app/chamados/reconciliador.py:572`

_7556 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
