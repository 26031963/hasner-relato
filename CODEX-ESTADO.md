# CODEX-ESTADO

> Ultima resposta processada: **21/11/2026 12:09** · publicado em **21/11/2026 12:11** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 21/11 12:09 · sessao a161069b9565 · E4C-F5 · verificador · origem=Code

**E4C-F5 `003abf218` — BLOQUEIA o pouso.** Os cinco testes D e o teste de texto atingem os sítios pedidos. Permanecem divergências entre seleção SQL e leitura composta, decisões sobre instâncias carregadas antes de uma escrita, dois relógios na ressuscitação e evidência incompleta no PRONTO. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:60–208`, `app/chamados/juizes.py:1583–1599`, `app/chamados/services/validacao.py:1074–1104`, `app/chamados/models.py:1558,1702`, `app/docs/RELATO.md:92–94`.

Conferi a cópia e os snapshots locais das bases fornecidas em leitura. O inventário `ebfdcc4df` → cópia bate com `stat.txt` e `testes_numstat.txt`, e `b0935` → cópia difere apenas nos dois arquivos de teste do item 1. A cópia não contém metadados Git para autenticar o SHA. Suite, banco, logs integrais e implantação [nao verificado].

## 1. Escopo — BLOQUEIA

- **Leitura mista introduzida no slot.** `if respondida is None and p.respondida_em is not None:` combina a projeção temporal da composta com a coluna. Com ato vigente posterior ao horizonte e coluna nula, a guarda deixa calcular um slot que deveria ficar fora daquela consulta. A interpretação de `respondida is None` também inclui ato já desfeito. O recorte do invariante de escrita para `validada_em` consta do 1440, mas uma exceção nominal para essa leitura mista [nao verificado]. `app/chamados/juizes.py:344–378`, `app/docs/RELATO.md:148–157`.
- **Seletores que ainda decidem antes do compositor.** `return Q(respondida_em__isnull=False) & Q(resposta_colab__regex=_RE_TEM_TEXTO)` é a face da coluna. Ela seleciona a irmã do intervalo, a fila para validar, as corrigíveis e a metade que entra no materializador. A chamada posterior à composta não recupera um PK já excluído. As chamadas são preexistentes ou preservadas nos hunks migrados. O 0545 mantém filas de **validação** na coluna, e o 1440 adia o invariante do **instante**. Uma exceção específica para a seleção da irmã e para as portas de correção e materialização [nao verificado]. `app/chamados/juizes.py:1583–1599,1762–1776`, `app/chamados/services/materializacao.py:139–150,2140–2163`, `app/chamados/models.py:1549–1555`.
- **Outros leitores de instante/carimbo que encontrei fora do compositor.** `respondida_sem_texto(p)` em porta e vigia, o filtro de recuperação do legado, o limbo do credor, `Max('respondida_em')` da lavra, a idade da fila e a serialização das APIs. Há também leitura direta no modal e no cartão da pergunta. Os papéis são distintos, mas a autorização nominal de cada leitor [nao verificado]. `app/chamados/juizes.py:1752`, `app/chamados/services/vigia_fio.py:635,749`, `app/chamados/services/validacao.py:406–408,1275`, `app/chamados/services/disputa_emissao.py:1899`, `app/chamados/veredito_lavrado.py:206–209`, `app/api/views.py:1163–1165,2663`, `app/api/views_mensageria.py:569–572`, `app/templates/chamados/partials/modal_fio.html:219–228,345`, `app/templates/chamados/partials/_pergunta_card.html:20–42`.
- **Exceções e leituras de outra finalidade.** O placar lê a tupla da projeção e tem exceção nominal no selo. `resposta_display` lê o texto do ato. As fotos de ressuscitação, silêncio, retratação e histórico leem o ato para trilha ou exibição. O vigia compara as duas fontes. O C19 lê a sequência até seu horizonte, e H4 lê o vínculo de absorção e a ordem de inserção. A face SQL de validação já existia na F2. `app/chamados/services/raiox.py:135–147`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:23–28`, `app/chamados/models.py:1705–1718,2110–2112,2242–2246,3591–3618,3854–3868,4402–4425`, `app/chamados/services/historico.py:97–111`, `app/chamados/services/tarefa_do_dia.py:387–429`, `app/chamados/services/vigia_texto_resposta.py:17–24`, `app/docs/RELATO.md:131–136,1486`.
- O cache do modal tem registro no RELATO e delega à mesma contagem. H4, a guarda do par e a saída de SILÊNCIO têm os despachos na cópia. A correspondência nominal de **cada** carregador com os 59 pontos de toque permanece [nao verificado]. O mapa publicado na cópia explicita lacunas. `app/chamados/templatetags/chamados_tags.py:221–238`, `app/templates/chamados/partials/modal_fio.html:396`, `app/docs/RELATO.md:94,131–136,155–156,274,798`.

## 2. Escritores, leitores e significado — BLOQUEIA

| Campo ou valor | Escritores e leitores conferidos |
|---|---|
| `validada_em` | Validação e devoluções gravam coluna e ato. A resposta humana limpa e grava `RESPOSTA`. Reabertura e ressuscitação limpam pelo lote com ato compensador. `carimbar` pode preencher a coluna, e a leitura pode interpretá-la como passivo, sem `VALIDACAO` física. Leem a composta a correção, declaração, par, materializador, lote, estado, C6 e veredito. Filas, vigias e procedência também leem a coluna. `app/chamados/services/validacao.py:161–175,264–296,365–374,656,702,916,1082,1104`, `app/chamados/services/disputa_emissao.py:1503–1537,1664–1684`, `app/chamados/models.py:1702–1722,2024–2026,3259–3262,3371–3393,4318–4322`, `app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/declaracao_texto.py:229`, `app/chamados/services/materializacao.py:201–203,2144,2161`, `app/chamados/services/vigia_carimbo.py:98`, `app/ponto/services/procedencia_batida.py:74`. |
| `respondida_em` e `resposta_colab` | Resposta humana, automática, carimbo da trilha, nascimento, carimbo e portas de reabertura/ressuscitação. A composta lê instante vigente da sequência e texto corrente da projeção. Histórico migra o instante e mantém o texto corrente. O app serializa ambos e também inclui `resposta_display`, que tem fonte distinta. `app/chamados/services/disputa_emissao.py:1495–1537,1664–1684,1689–1737`, `app/chamados/models.py:3053–3055,3234,1721–1722,3367–3370,3844,3868,4388–4390`, `app/chamados/services/validacao.py:450–456`, `app/chamados/services/historico.py:97–111`, `app/api/views.py:2579–2590`. |
| `materializada_em` e `via_resolucao` | `_escrever_carimbo` aplica o par usado por carimbo, identidade e descarimbo. O descarimbo em lote e as portas de alteração exclusiva da via também escrevem. Leem C6, estado, materializador, grupo de ausência, H4 e procedência de batida. `app/chamados/models.py:3324–3328,3366,3416–3460,3500–3550,4388–4425`, `app/chamados/services/relavra_via.py:118–126`, `app/chamados/juizes.py:1393–1403`, `app/chamados/services/materializacao.py:879–896,1154–1167,2143–2161`, `app/ponto/services/procedencia_batida.py:74`. |
| `decisoes` e `LeituraComposta` | A inserção passa por `registrar`. `leitura` ordena por `(ato_em, pk)` e acrescenta o passivo sem correspondência após as linhas reais empatadas. `composta` monta cinco valores. Consumidores centrais são C6/C21, estado, sem veredito, app, histórico e portas da volta D. H4 ordena pelo PK de inserção para responder se uma absorção foi revogada. `app/chamados/models.py:4252–4254,4348–4390,4402–4449`, `app/chamados/juizes.py:1406–1428,1550–1560`, `app/api/views.py:2579–2602`, `app/chamados/services/historico.py:97–111`, `app/chamados/services/validacao.py:1082,1104`. |
| Universo de `fechar` | Todas as perguntas da relação inversa da disputa, com atos. A lista passa a `respostas_sem_veredito` e ao C6 e fornece as vivas, os destinos a travar e os PKs da trilha. A porta de fechar grava a disputa e dispara a cascata do pai. `app/chamados/models.py:1982–2027,2071–2126`, `app/chamados/signals.py:43`. |
| Contador de texto e retorno do lote | O vigia compara literalmente ato e `(p.resposta_colab or '')`, sem normalizar caixa ou espaços. A entrada humana aplica `strip`, e o C21 usa o vazio após trim. O lote soma PKs validados e devolve recusas por PK. JSON, HTMX e copiloto consomem o retorno. `app/chamados/services/vigia_texto_resposta.py:17–25`, `app/chamados/services/disputa_emissao.py:1446–1448,1689–1714`, `app/chamados/juizes.py:1534–1560`, `app/chamados/services/validacao.py:1063–1114`, `app/chamados/views_cobrar.py:263–269,381–388`, `app/core/views_ia.py:169–191`, `app/templates/chamados/partials/_lote_preview.html:6–9`. |

**Trecho que bloqueia.** A minuta aprovada diz “Se a coluna estava vazia, a passagem não a preenche”. A passagem chama `q.carimbar(..., resposta='validar')`. Para uma irmã com texto anterior ou empatado com T1 e sem validação, `if resposta == 'validar' and self.validada_em is None: self.validada_em = quando` abre o caminho de escrita. O passivo sintético pode fazer o par retornar `(True, True)` e deixar o carimbo passar. O teste do grupo chega a exigir `validada_em=T1` para o escape de hora. O literal aprovado e o contrato exercitado divergem. `app/docs/RELATO.md:413–421`, `app/chamados/services/materializacao.py:1065–1087`, `app/chamados/models.py:3367–3393,4318–4322`, `app/chamados/tests/test_e4c_f5_a1c_passagem_da_ausencia.py:189–209`.

Os carimbos são instantes com `USE_TZ=True`. O C6 e a tarefa incluem o empate com `agora`. `data_evento` representa dia do turno para pergunta de fato e dia civil para conversa, que a função do dia do turno exclui. O memo C6 é de instância, e o contador do modal é de render por disputa. O censo completo de acessos dinâmicos, templates indiretos e consumidores fora das famílias acima [nao verificado]. `app/config/settings/base.py:93–95`, `app/chamados/models.py:2633–2659,3699`, `app/chamados/juizes.py:337–341,857–907,1394–1402`, `app/chamados/services/tarefa_do_dia.py:391–404`, `app/chamados/templatetags/chamados_tags.py:231–238`.

## 3. Cache depois da escrita — BLOQUEIA

- **Lote, antes do próximo item.** `achadas = {...prefetch_related('decisoes')}` é avaliado antes de todo o laço. Cada volta faz `perg = achadas.get(ref)`. Validar uma metade pode validar a outra em `_validar_o_par`; a outra instância já carregada no lote conserva a coluna e os atos anteriores e passa pela guarda de entrada com essa foto. A releitura de 1103 só vale para o item depois de chamar a ação. `app/chamados/services/validacao.py:648–659,1074–1086,1094–1104`.
- **Disputa fechada com universo anterior à trava.** `perguntas = list(...)`, `_pend = self.respostas_sem_veredito(...)` e `vivas = [...]` antecedem `trava_na_ordem`. A porta grava usando as mesmas instâncias. Uma resposta pode entrar antes de o fecho adquirir a trava, e o fecho continua com a decisão anterior. A posição da trava é preexistente, e o hunk elimina a segunda carga de perguntas que havia entre as guardas. `app/chamados/models.py:2071–2106,2118–2126`, `app/chamados/services/disputa_emissao.py:1478–1537`, `prod.diff`, hunk de `DisputaSupervisao.fechar`.
- **Absolvição pela célula.** O queryset passa a carregar `decisoes` antes da trava. Depois de `trava_do_colaborador(_cid)`, a porta chama `_gravou = p.carimbar(...)` na mesma instância. Uma resposta ou validação interposta por outra instância não invalida aquele prefetch. A separação entre seleção e trava já existia para as colunas, e o diff estende a foto velha aos atos. `app/chamados/reconciliador.py:635–646,696–724`, `app/chamados/models.py:3375–3386,4361,4443–4449`.
- **Pontos que passam.** A releitura do item em `1103–1104` usa refresh integral e depois a composta. A corrida nova intercala o escritor real justamente entre essas duas linhas. O core relê integralmente sob a trava, antes do veto e da escrita, e `registrar` invalida prefetch e memo na instância recebida. A devolução limpa o prefetch após refresh parcial e relê o estado final e a disputa. O plantio relê cada PK sob trava, foto recebida atualiza `d`, e o backfill de fio morto recarrega os restantes após reabrir. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:113–147`, `app/chamados/services/disputa_emissao.py:1478–1537`, `app/chamados/models.py:4443–4449`, `app/chamados/services/validacao.py:267–309,1103–1104`, `app/chamados/services/materializacao.py:2149–2163`, `app/api/foto_recebida.py:70–84`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:132–145`.

## 4. Classes de reprovação — BLOQUEIA

| Classe | Veredito |
|---|---|
| **a. Segundo juiz** | **NAO BLOQUEIA.** C7, C21 e respostas sem veredito conservam os registros. O helper de horizonte em cadastro chama os dois juízes. H4 verifica carimbo e vínculo de absorção para um veto próprio. `app/core/juizes.py:291,305–307`, `app/escala/services/cadastro_realidade.py:497–507`, `app/chamados/models.py:4402–4425`, `app/chamados/services/disputa_emissao.py:1485–1490`. |
| **b. Segundo escritor** | **NAO BLOQUEIA pelo diff lido.** O adaptador H4 chama o mesmo core. Validação, devolução, carimbo e registro usam as portas existentes. Escrita de `respondida_em` sem linha física e escritores de via fora da trava estão registrados no recorte 1440/H4. `app/chamados/services/disputa_emissao.py:1532–1537,1587–1594`, `app/chamados/models.py:3367–3374,4428–4449`, `app/docs/RELATO.md:144,148–157`, `app/docs/BACKLOG.md:2056–2059`. |
| **c. Relógio fora da borda** | **BLOQUEIA.** `_agora_sonda = _tz_s.now()` corta `d.ato_em > _agora_sonda` antes da trava. A reabertura real lê novamente `_agora_r = _tz_r.now()` depois da trava. Uma linha no intervalo fica vigente na simulação e é anterior à reabertura gravada. O comentário “a mesma borda” descreve dois instantes distintos. `app/chamados/models.py:1558,1591–1595,1632,1651,1702–1718`. |
| **d. Derivação paralela** | **BLOQUEIA.** A combinação `respondida is None and p.respondida_em is not None` e a seleção da irmã pela face SQL mantêm decisões fora da fonte escolhida. `app/chamados/juizes.py:350–354,1588`, `app/chamados/services/materializacao.py:145–150,192–203`. |
| **e. Selo ou RED sem mordida** | **BLOQUEIA para a cobertura declarada.** `CAMPOS = frozenset({'respondida_em', 'validada_em'})` e `FUNCOES = {...}` limitam a catraca. `_slot_esperado` e a seleção da irmã ficam de fora. O selo de chamada aceita a chamada à composta no slot sem verificar a leitura crua imediatamente seguinte. A bateria do item 1 passa na revisão estática, conforme §5. `app/chamados/tests/test_e4c_f5_selo_catraca.py:12–28,338–349`, `app/chamados/tests/test_e4c_f5_selo_chamada.py:70–77,190–193`, `app/chamados/juizes.py:350–354`. |
| **f. Ordem de trava** | **NAO BLOQUEIA quanto à ordem de aquisição.** Os SELECTs de prefetch são leituras simples. S14 mudou de irmãs intercaladas com os fechos para todas as irmãs antes dos fechos. Cada irmã tem seu atomic, e o core já saiu do atomic da resposta. O uso de dados anteriores à trava é o bloqueio distinto do §3. `app/chamados/services/materializacao.py:1055–1058,1105–1114`, `app/chamados/services/disputa_emissao.py:1532–1566`, `app/docs/RELATO.md:134–139`. |
| **g. Campo com dois significados** | **BLOQUEIA nos consumidores dos §§1–2.** Os tipos distinguem corretamente texto do ato e texto corrente. O slot ainda alterna instante vigente e carimbo físico dentro da mesma guarda. `app/chamados/models.py:17–25,4382–4390`, `app/chamados/juizes.py:350–354`. |
| **h. Manager/anotação** | **NAO BLOQUEIA.** O manager próprio é o de imutabilidade do ato. Os prefetches do diff são explícitos nos carregadores. Os `Exists` da face da F2 já existiam e a leitura de H4 é do vínculo, sem introduzir atalho SQL para o O261. `app/chamados/models.py:3591–3618,4250,4402–4425`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:203–207`. |
| **i. Pré-requisito** | **NAO BLOQUEIA a construção.** A tabela, a marca de ressuscitação, a coluna de preservação e o backfill já estão no código das migrations 0051, 0054, 0055 e 0056. Aplicação no destino e ordem efetiva F6F7/F9/F5 [nao verificado]. `app/chamados/migrations/0051_decisao_pergunta.py:41–75`, `app/chamados/migrations/0054_decisao_da_ressuscitacao.py:15–21`, `app/chamados/migrations/0055_decisao_resposta_mantem_validacao.py:12–22`, `app/chamados/migrations/0056_decisao_resposta_mantem_validacao_historico.py:17–26`. |

## 5. Testes novos do item 1 — NAO BLOQUEIA [nao verificado]

Cada âncora ocorre uma vez no corpo de produção indicado. O seletor recompila o corpo verdadeiro com uma substituição, troca seu `__code__` e restaura no `finally`. Os pontos de interposição delegam à carga, ao refresh ou ao escritor real. Os seis casos, por leitura, não têm `expectedFailure`, `skip` nem mock do predicado alvo. `app/chamados/tests/e4c_f5_voltaD_mutantes.py:13–50,54–71`, `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:22–36,50–208`.

| Mutante | Sítio e fato discriminado |
|---|---|
| `corrigir` | `app/chamados/services/acoes_disputa.py:349`. Validação real entre carga e guarda. O resultado deve ser `info`, texto `15:08` preservado e validação presente. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:50–71`. |
| `declaracao` | `app/chamados/services/declaracao_texto.py:229`. Validação real entre carga do fato e prefetch. O marco fica com `aceita=False`. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:73–88`. |
| `irma_implausivel` | `app/chamados/services/materializacao.py:203`. A irmã implausível é validada com confirmação pela porta. O teste exige que o fluxo prossiga até identificar a irmã já gravada no espelho. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:153–182`. |
| `lote_entrada` | `app/chamados/services/validacao.py:1082`. Validação real interposta antes do prefetch. Exige PK recusado como já validado e zero em `validadas`. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:90–100`. |
| `lote_releitura` | `app/chamados/services/validacao.py:1104`. Classe A real, POST web, entrada real e core real depois do refresh do lote. O assert final é exatamente `([pergunta.pk], 0, None)` e só extrai `ref` das recusas. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:102–147`, `app/chamados/views.py:1274–1278`, `app/chamados/services/disputa_emissao.py:1587–1594`. |
| `casefold_projecao`, `strip_ato` | `app/chamados/services/vigia_texto_resposta.py:23`. Primeiro seletor usa `.lower()` na projeção e o segundo `.strip()` no ato. As portas automáticas e de reabertura criam a diferença observada. Cada caso exige `(1, 1)`. `app/chamados/tests/e4c_f5_voltaD_mutantes.py:46–50`, `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:185–208`. |

O trecho fornecido de `MUT_FIM` registra seis verdes e sete falhas de asserção, zero erros. Os asserts indicados no topo batem com a árvore. Reprodução independente e tracebacks integrais [nao verificado]. `app/chamados/tests/test_e4c_f5_voltaD_corridas.py:70,88,99,144,182,208`.

## 6. Adaptações preexistentes — BLOQUEIA

**Trecho que bloqueia.** `composta=lambda p: N(validada_em=p.validada_em)` é um segundo dublê de `composta`, com um campo, fora do helper único. O 1050 item 2 nomeia os hunks de `test_e4_expand.py:101–104,180–182`, enquanto esse dublê está em `:82`. A ligação de `_foto_da_validacao` ao corpo real está em outro hunk, `:153`. O RELATO registra as adaptações desse harness, e um aval nominal para o dublê local [nao verificado]. `app/chamados/tests/test_e4_expand.py:80–83,101–104,153,180–191`, `app/chamados/tests/duble_pergunta.py:6–41`, `app/docs/RELATO.md:294,303`.

Dos 35 arquivos preexistentes alterados, eis o enquadramento por sítio. A contagem estática de `assert` e `self.assert*` não caiu em nenhum deles. Ela aumentou no selo F2, no teste B2 de tipo e no teste do vigia. Equivalência de todos os efeitos dos harnesses em execução [nao verificado]. `app/chamados/tests/test_e4c_f2_leitura_do_ato.py:519–544,584–593`, `app/chamados/tests/test_f1nova_b2_leitores.py:95–97`, `app/chamados/tests/test_o261_vigia_fio.py:816–852`.

- **Âncoras i.** `app/chamados/tests/bug_par_mutantes.py:69`, `app/chamados/tests/e3_mutantes.py:30`, `app/chamados/tests/e3_r3_c6_mutantes.py:49–51`, `app/chamados/tests/e4b_c12_ast.py:19–27`, `app/chamados/tests/test_cef_c1_sem_banco.py:309–312`, `app/chamados/tests/test_e4b_classe_e_memoria.py:40,51`, `app/chamados/tests/test_e4c_f6f7_c7_sem_banco.py:113`, `app/core/tests/test_medida_turno_do_par.py:258–274,319`.
- **Dublês e ORM ii/v.** `app/chamados/tests/e3_v8_apoio.py:12,77,167`, `app/chamados/tests/e3_volta7_apoio.py:12,33,79,111`, `app/chamados/tests/test_bug_par_c2_sem_banco.py:13,32,80–83`, `app/chamados/tests/test_bug_par_sem_banco.py:14,170,193`, `app/chamados/tests/test_cef_c1_sem_ban

[truncado em 20000 de 34867 caracteres]

## 21/11 12:03 · sessao 55480df206d3 · PISTA-ESCOLHE-SOZINHA · verificador · origem=Code

[resposta retida: caminho_de_segredo -- nao se publica credencial nem caminho de segredo]

## 21/11 12:02 · sessao 6e0323cce2c0 · FERIADO-POR-POSTO · construtor · origem=Code

**PAREI no achado 31.** A cópia em `df9df24c4c63a0cea98eca40890853e1d97a22e4` tem dois cálculos efetivos de `trabalha_em_feriado` no nascimento do vínculo. [Nascimento normal em vinculo.py:296](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/vinculo.py:296) · [Nascimento por cópia em feriado_do_posto.py:186](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:186)

| Caminho | Valor gravado |
|---|---|
| `executar_vinculo` | Chama `EscalaColaborador.padrao_trabalha_em_feriado`, que usa a marca declarada sem consultar o cargo. [vinculo.py:296](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/vinculo.py:296) · [escala/models.py:905](/home/ronald/wt-codex-feriado-por-posto-120132/app/escala/models.py:905) |
| `ajustar_vinculo_pelo_sistema` e `desfazer_ajuste_do_sistema` | Chamam `chave_ao_nascer`, que só usa a marca declarada para ciclos elegíveis e cargos não administrativos. Nos demais casos, grava a chave herdada. [vinculo.py:1118](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/vinculo.py:1118) · [vinculo.py:1142](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/vinculo.py:1142) · [feriado_do_posto.py:174](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:174) |

Para um cargo administrativo em posto `OPERA`, o nascimento normal grava `True`. Uma cópia que herda `False` grava `False`. A escolha da regra comum fica devolvida ao opus pelos dois caminhos acima. [escala/models.py:911](/home/ronald/wt-codex-feriado-por-posto-120132/app/escala/models.py:911) · [feriado_do_posto.py:182](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:182) · [feriado_do_posto.py:190](/home/ronald/wt-codex-feriado-por-posto-120132/app/colaboradores/services/feriado_do_posto.py:190)

A volta ficou em leitura, sem alterações. REDs, mutantes, Ruff e suíte [nao verificado]. [nao verificado]

## 21/11 11:38 · sessao b90a3599ab67 · WRAPPER-N-VAGAS · verificador · origem=Code

# AVAL-DEVOLVIDO — itens 2, 3 e 4

Revisão estática da cópia fornecida. A confirmação independente do hash por Git, a ocupação atual das vagas e o estado das worktrees no host estão [nao verificado].

## Item 2 — Wrapper com N vagas

**1. Premissas e leitores — PROCEDE.** O comentário de vaga única confere. O wrapper deriva um caminho por repositório a partir do `git-common-dir` de `CODEX_VIVO`, abre o arquivo com `exec 6>>` na linha 252 e tenta `flock -n 6` na linha 253. Quando a vaga está ocupada, `_recusa2` devolve **rc 2**. O rc 75 já tem usos distintos para pausa do construtor e limite ou indisponibilidade do Codex. `bin/codex_pergunta.sh:42–45,54–56,69–78,196,203–204,248–259,447–459`.

| Quem usa a vaga | O que lê ou testa | Efeito de trocar apenas o nome para `construtor.vaga.<k>` |
|---|---|---|
| `#G:vaga_reservada` | Em todos os papéis, compara o `realpath -m` da resposta com o caminho exato em `$VAGA`. | Se continuar protegendo só o caminho antigo, uma resposta pode substituir o inode de uma vaga numerada por `mv -f`, enquanto o lock permanece no inode anterior. `bin/codex_pergunta.sh:69–78,405–406,428–430`. |
| `#G:vaga_topologia` | No ramo construtor, exige `$VAGA` não vazia. Isso representa um `git-common-dir` cujo basename é `.git`. | A guarda continua válida se o sítio continuar vazio para topologia sem `.git`. Gerar caminhos numerados a partir de um sítio vazio invalida a recusa. `bin/codex_pergunta.sh:72–78,195–204`; `bin/tests/test_codex_pergunta.sh:405–420`. |
| Guarda de symlink, `#G:vaga_abre` e `#G:vaga` | Conferem o caminho e a pasta, abrem o arquivo e testam o lock pelo FD 6. | Cada candidato precisa passar pelas mesmas verificações. A exclusão que hoje cerca a validação da própria worktree também se perde se duas chamadas da mesma worktree puderem adquirir vagas distintas. `bin/codex_pergunta.sh:216–238,248–262,407–422`. |
| Selos de ocupação `c_vaga`, `c_vaga_outra_wt` e `c_vaga_copia` | Prendem o caminho literal antigo e exigem rc 2. Cobrem outro `LOGD`, outra worktree do mesmo repositório e outra cópia do wrapper. | O lock do selo deixa de disputar a vaga real. Os casos perdem a prova de exclusão e a expectativa de rc ainda diverge do rc 75 proposto. `bin/tests/test_codex_pergunta.sh:216–220,399–403,422–427,553,583–584`. |
| Selos `c_censo`, `c_vaga_reservada` e `c_vaga_abre` | Prendem a vaga para provar a precedência do censo, usam o caminho como resposta e põem um diretório no caminho para forçar erro de abertura, respectivamente. | O censo ainda pode recusar, mas o caso perde a prova de que veio antes de uma vaga realmente ocupada. Os outros dois casos passam a mirar o caminho antigo. O controle de resposta legítima em outra pasta deve continuar passando. `bin/tests/test_codex_pergunta.sh:222–236,262–264,274–282,539–540,591`. |
| `bin/raias_estado.py`, `raia_agente.sh` e `ESTADO` | O medidor lê `logs/raias/*.agente` e Git. `raia_agente.sh listar` e o `ESTADO` usam esse medidor. O painel Codex do `ESTADO` lê `chamadas.log` e `limite.log`. | A troca do arquivo de lock não quebra esses leitores. O placar de ocupação alegado no aval não aparece nesses caminhos. `bin/raias_estado.py:51–102,125–135`; `bin/raia_agente.sh:80–81`; `bin/gerar_estado.py:211–229,275–289`; `bin/codex_limite_estado.py:20–22,45–67,82–89`. |
| Hooks em `bin/` | O `PreToolUse` usa `disjuntor.py`; a via Codex faz o censo no wrapper antes do lock. O placar do censo casa `ini` com obra, PID e hash. O Stop procura o wrapper em `/proc` para provar espera de censo. | Esses caminhos não dependem do nome do arquivo de lock. Mudar as posições dos campos de `ini` pode quebrar o casamento. Outros leitores exclusivos do host estão [nao verificado]. `.claude/settings.json:62–72`; `bin/disjuntor.py:1851–1863,2096–2108,2148–2158`; `bin/hook_stop_fila1.py:730–766`; `bin/codex_pergunta.sh:241–253`. |

**2. Colisão com aval ou corte — PROCEDE.** O anexo autoriza a emenda para **1 opus + 3 Codex**. A cópia ainda prescreve **1 opus + 1 Codex** na L-126 e **1 Codex por repositório** na 7b CODEX item 1. O registro em `CORTES.md` passa pela fonte `CORTES.json`. Há também uma colisão a resolver com o desenho anterior `CODEX-2-RAIAS-PORTAO-CPU`, ainda marcado ABERTA, que condiciona a segunda vaga Codex a load, horário e desligamento. O item 2 permite ocupar a segunda vaga pelo primeiro `flock` livre. A vigência operacional daquele portão no host está [nao verificado]. Como a obra toca `bin/`, sua construção cabe à sessão/opus pela cerca atual. `aval.txt:1,4`; `app/docs/LEIS.md:147`; `CLAUDE.md:353–356`; `app/docs/CORTES.md:3–6`; `bin/gerar_cortes.py:23–24`; `app/docs/BACKLOG.md:2317`; `app/docs/RELATO.md:4104`; `bin/codex_pergunta.sh:121–128,343–347`.

**3. Condições no literal — PROCEDE.** Há três incompatibilidades. `aval.txt:4,9`.

- **Rc e mordida.** `CODEX_VAGAS=1` com vaga ocupada não pode preservar o rc 2 de hoje e exigir rc 75 ao mesmo tempo. O selo atual já neutraliza `#G:vaga` e cobra a recusa pelo motivo certo. Para provar saturação com N vagas, os N falsos precisam estar simultaneamente admitidos no mesmo repositório e usar cópias e respostas distintas. O N+1 precisa falhar pela ocupação, pois a pausa também devolve 75 e há outro gate de slots no wrapper. `bin/codex_pergunta.sh:196,204,248–259,364–380,383–392`; `bin/tests/test_codex_pergunta.sh:216–220,553,605–625`.
- **Guardas e transição.** Preservar literalmente a reserva de um só caminho deixa os demais desprotegidos. O aval também não define como uma chamada nova respeita um lock ainda preso no nome antigo, nem como `CODEX_VAGAS` fica limitado a três vagas Codex. O anexo mostra o arquivo antigo. Um detentor atual desse lock está [nao verificado]. `aval.txt:1,4`; `bin/codex_pergunta.sh:69–78,248–259,428–430`; `app/docs/LEIS.md:147`.
- **`chamadas.log`.** O escritor de produção na cópia é o próprio wrapper. O destino padrão é `logs/codex/chamadas.log`, substituível por `CODEX_LOGD`. `ini` e `fim` ainda não levam `k`, e a recusa por ocupação ocorre antes de `ini`. O log pode receber a vaga adquirida, mas o contrato precisa dizer em qual registro e preservar os campos posicionais. A presença do log no host está [nao verificado]. `bin/codex_pergunta.sh:101,248–273,304–309`; `bin/codex_limite_estado.py:49–68`; `bin/disjuntor.py:2096–2108`.

**4. Exportada, dinheiro ou `!` — NAO PROCEDE.** O escopo declarado é admissão, log e selo de host, com pouso próprio de instrumento. A cerca vigente continua aplicável a competência exportada, arquivos protegidos e itens que pedem `!`. `aval.txt:4`; `app/docs/LEIS.md:131,147`; `CLAUDE.md:265–268`; `bin/codex_pergunta.sh:343–347`.

## Item 3 — Teto 4 na conduta

**1. Premissas — NAO PROCEDE.** A 7b CODEX item 2 exige linha `DESPACHO` no RELATO e cita CPU/load da VM e `/health/` do `saas_core` na rajada. A regra de raias considera teste um arquivo para o cruzamento. O teto 6 da cláusula de DESPACHO é o de itens independentes em paralelo. O teto específico de construtores está na L-126. `CLAUDE.md:309–311,357–365`; `app/docs/LEIS.md:147`.

**2. Colisão com aval ou corte — PROCEDE.** Além da atualização da L-126, há uma reserva nominal anterior: vaga 3 para TRAVAS-V2 e vaga 4 para LASTRO-REDESENHO, com a quarta reservada até a liberação dos arquivos em comum. O teto 4 já apareceu na abertura de laterais de 20/11. O novo literal condiciona a abertura à medida e à disjunção, mas não diz se a quarta vaga passa a ser de uso geral. Revogação dessa reserva em registro posterior está [nao verificado]. `aval.txt:1,5–6`; `app/docs/PROMPTS.md:2671,2675`; `app/docs/RELATO.md:1290–1291`; `app/docs/LEIS.md:147`.

**3. Condições no literal — PROCEDE.** O item exige registrar medidas, mas não diz qual resultado impede a abertura, qual janela de load vale ou como tratar `/health/` indisponível. A 7b cita medição **na rajada**, e a prova citada no wrapper foi de seis chamadas **curtas**. A condição também precisa distinguir a terceira ou quarta abertura **total**, que inclui opus, do índice `k` das três vagas Codex. Para a regra de arquivos valer antes da abertura, é preciso conhecer os arquivos previstos na próxima volta, inclusive testes, além do diff já produzido. Medidas atuais e listas das obras em voo estão [nao verificado]. `aval.txt:1,4–5,9`; `CLAUDE.md:309–311,355–365`; `bin/codex_pergunta.sh:10–13,248–259`; `app/docs/RELATO.md:4103,4107`.

**4. Exportada, dinheiro ou `!` — NAO PROCEDE.** O item regula quantas obras podem abrir e o que medir. A autorização de cada obra continua sujeita aos limites da L-126 e da seção 7b. `aval.txt:1,5`; `app/docs/LEIS.md:147`; `CLAUDE.md:265–268,357–365`.

## Item 4 — FERIADO-A5 na vaga Codex

**1. Premissas — PROCEDE.** O RELATO confirma o registro do verificador de escopo inteiro sobre `6e36bd3b0`, com 10 BLOQUEIA, 9 CORRIGE e 1 FILA. O anexo informa FERIADO em `df9df24c4` e F5 em `003abf218`, sem as listas de arquivos. Os achados que ainda se aplicam à cópia indicada e a interseção atual estão [nao verificado]. Há ainda uma incompatibilidade concreta de entrada: `wt-feriado-posto-135549` é a cópia referida, e o wrapper exige `wt-codex-<obra>-<6 dígitos>`, registrada no mesmo repositório, com HEAD igual ao hash informado e inventário limpo. `aval.txt:6`; `app/docs/RELATO.md:151,175,179,284`; `bin/codex_pergunta.sh:216–238,254–259`.

**2. Colisão com aval ou corte — PROCEDE.** O novo aval altera expressamente a espera pela F5 quando a interseção for zero. A colisão restante é tratar interseção zero como condição suficiente para abrir: o BACKLOG também põe FERIADO atrás da fila 1 e sujeito a vaga no teto, e há a reserva documentada da quarta vaga. Se a L-126 for a via, a abertura também passa pelo wrapper vigente. Para o pouso, a condição registrada é **a partir de 21/11 com `sombra_c2b = 0`**. O anexo não identifica qual seria a outra “condição já escrita”. `aval.txt:1,5–6`; `app/docs/BACKLOG.md:1965–1966`; `app/docs/RELATO.md:284,764,1291,1366`; `app/docs/LEIS.md:147`; `bin/codex_pergunta.sh:241–259`.

**3. Condições no literal — PROCEDE.** `git diff --name-only` sem base não é o mesmo que comparar a cópia com `main`. O aval nomeia `main` para FERIADO, mas deixa sem base a lista da F5. Para que o zero tenha significado, o registro precisa identificar a referência imutável usada nos dois lados, os HEADs comparados e se a lista cobre alterações locais, arquivos novos e os arquivos previstos para a volta. O RELATO já distingue diff de commits e estado local em uma regra de cruzamento. O `r.txt` está ausente da cópia examinada, `logs/` é ignorado e o RELATO documenta o caminho; a presença atual e o conteúdo do arquivo no host estão [nao verificado]. `aval.txt:6`; `app/docs/RELATO.md:284,3026,4107`; `CLAUDE.md:309–311,344–346`; `.gitignore:53–54`.

**4. Exportada, dinheiro ou `!` — PROCEDE.** A abertura de uma volta de construção não é um apply. O defeito está no escopo aberto de “volta dos achados”: há achado do próprio verificador já remetido à fila por atingir `ponto/portas/celula.py`, proibido ao Codex. Também está registrada, sem autorização de cura nessa volta, a invalidação de `FechamentoMensal.previsto_em` sem filtro de exportada. O RELATO registra efeito em dinheiro na sombra, que deu origem ao portão `sombra_c2b = 0`. O aval precisa limitar a volta aos achados triados dentro do escopo autorizado, com a prévia só na sombra e a prova por chave e valor. Efeito da volta proposta sobre exportada ou dinheiro em produção está [nao verificado]. `aval.txt:6`; `app/docs/BACKLOG.md:1966,1981–1982`; `app/docs/RELATO.md:763,1366`; `app/escala/signals.py:137–146`; `app/ponto/services/fechamento.py:1607–1613`; `app/docs/LEIS.md:147`; `CLAUDE.md:265–268,372–380`.

## Colisões com os itens 5 e 6

- **Item 5.** Alterar a fila do V2 deixa duas cláusulas do rascunho aprovado em colisão com o item 2: a vaga única por repositório e o rc 2 quando a retomada encontra a vaga tomada. O contrato do lançador já manda encerrar a automação nessa situação. `aval.txt:4,7`; `app/docs/BACKLOG.md:1984`; `app/docs/RELATO.md:185,199,219,247,254–256`.
- **Item 6.** A autoria dos próximos testes passa à sessão pelo novo aval. Usar isso como prova de que uma vaga já foi liberada colide com o ciclo do lock, que é adquirido pelo wrapper e mantido durante a chamada. O último despacho de construção da F5 encontrado na cópia registra Codex para testes, e o anexo não informa detentor atual. Liberação efetiva da vaga está [nao verificado]. `aval.txt:4,8`; `app/docs/RELATO.md:18,21`; `bin/codex_pergunta.sh:248–259,417–430`.

| Item | Pergunta | PROCEDE/NAO PROCEDE | Arquivo:linha |
|---|---|---|---|
| 2 | 1. Premissas e leitores | **PROCEDE** | `bin/codex_pergunta.sh:69–78,196,202,248–259`; `bin/tests/test_codex_pergunta.sh:216–226,274–282,399–427`; `bin/gerar_estado.py:211–229,275–289` |
| 2 | 2. Aval ou corte | **PROCEDE** | `app/docs/LEIS.md:147`; `CLAUDE.md:353–356`; `app/docs/CORTES.md:3–6`; `app/docs/BACKLOG.md:2317` |
| 2 | 3. Literal | **PROCEDE** | `aval.txt:4`; `bin/codex_pergunta.sh:204,248–273,304–309`; `bin/tests/test_codex_pergunta.sh:553,605–625` |
| 2 | 4. Exportada, dinheiro ou `!` | **NAO PROCEDE** | `aval.txt:4`; `app/docs/LEIS.md:131,147` |
| 3 | 1. Premissas | **NAO PROCEDE** | `CLAUDE.md:309–311,357–365`; `app/docs/LEIS.md:147` |
| 3 | 2. Aval ou corte | **PROCEDE** | `app/docs/PROMPTS.md:2671,2675`; `app/docs/RELATO.md:1290–1291`; `app/docs/LEIS.md:147` |
| 3 | 3. Literal | **PROCEDE** | `aval.txt:1,4–5,9`; `CLAUDE.md:355–365`; `bin/codex_pergunta.sh:10–13`; `app/docs/RELATO.md:4103,4107` |
| 3 | 4. Exportada, dinheiro ou `!` | **NAO PROCEDE** | `aval.txt:5`; `CLAUDE.md:265–268`; `app/docs/LEIS.md:147` |
| 4 | 1. Premissas | **PROCEDE** | `app/docs/RELATO.md:284`; `bin/codex_pergunta.sh:216–238,254–259` |
| 4 | 2. Aval ou corte | **PROCEDE** | `app/docs/BACKLOG.md:1965–1966`; `app/docs/RELATO.md:764,1291,1366`; `app/docs/LEIS.md:147` |
| 4 | 3. Literal | **PROCEDE** | `aval.txt:6`; `app/docs/RELATO.md:284,3026,4107`; `.gitignore:53–54` |
| 4 | 4. Exportada, dinheiro ou `!` | **PROCEDE** | `app/docs/BACKLOG.md:1966,1981–1982`; `app/docs/RELATO.md:1366`; `app/ponto/services/fechamento.py:1607–1613`; `app/docs/LEIS.md:147` |

## 21/11 11:22 · sessao 04a8e3820a94 · E4C-F5-CONSTROI · construtor · origem=Code

Entreguei os seis testes do item 1 e os dois seletores de texto. O inventário da volta é [test_e4c_f5_voltaD_corridas.py:1](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:1), criado, e [e4c_f5_voltaD_mutantes.py:46](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:46), ampliado. [nao verificado]

Conferi a sintaxe dos dois arquivos e a unicidade das sete âncoras. Em memória, os dois mutantes de texto mudaram o contador de `(1, 1)` para `(0, 1)` com seus respectivos pares. A execução dos seis testes com ORM e a certificação pelo Code e pelo Opus final seguem [nao verificado]. [vigia_texto_resposta.py:23](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/vigia_texto_resposta.py:23), [e4c_f5_voltaD_mutantes.py:55](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:55) [nao verificado]

| Teste | Mutante e sítio | Resultado exigido na original | [nao verificado]
|---|---|---|
| [test_RED_corrigir:60](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:60) | [`corrigir`:14](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:14), [acoes_disputa.py:349](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/acoes_disputa.py:349) | Retorna `info` e preserva `15:08` e a validação. |
| [test_RED_declaracao:73](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:73) | [`declaracao`:16](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:16), [declaracao_texto.py:229](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/declaracao_texto.py:229) | A projeção devolve `aceita=False`. |
| [test_RED_irma_implausivel:153](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:153) | [`irma_implausivel`:23](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:23), [materializacao.py:203](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/materializacao.py:203) | Após a confirmação administrativa, a tela chega à batida da irmã que o escritor acabou de gravar. |
| [test_RED_lote_entrada:90](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:90) | [`lote_entrada`:32](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:32), [validacao.py:1082](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/validacao.py:1082) | Recusa o PK com `Ja validada.` e devolve `validadas=0`. |
| [test_RED_lote_releitura:102](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:102) | [`lote_releitura`:35](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:35), [validacao.py:1104](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/validacao.py:1104) | PK em `recusados`, `validadas=0` e `validada_em` nulo no banco. |
| [test_RED_casefold:187](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:187) | [`casefold_projecao`:46](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:46), [`strip_ato`:48](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:48), [vigia_texto_resposta.py:23](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/vigia_texto_resposta.py:23) | `(1, 1)` para ato `'ato'` × projeção `'Ato'` e ato `'ato '` × projeção `'ato'`. |

As corridas intercalam leitor e escritor na transação do teste, com instâncias ORM distintas do mesmo PK. Em `corrigir`, `declaracao` e `lote_entrada`, a pausa fica após carregar a linha da pergunta e antes da leitura das decisões. Em `irma_implausivel`, fica após a busca real da irmã, antes da composta. As quatro pausas ficam antes das guardas que os mutantes trocam. [test_e4c_f5_voltaD_corridas.py:23](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:23), [test_e4c_f5_voltaD_corridas.py:50](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:50), [test_e4c_f5_voltaD_corridas.py:160](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:160) [nao verificado]

Em `lote_releitura`, o POST passa pela guarda web antes do lote. A resposta real retoma logo após o `refresh_from_db()` integral da instância do lote, que continua no caminho mutado. Prevaleceu a opção (a) do aval das 11h15 sobre o `PAREI` do anexo `r.txt`. A asserção final cobre somente o resultado por PK e a coluna persistida. O motivo copiado do sucesso é o ponto já identificado como `LOTE-MOTIVO-DO-SUCESSO`. [views.py:1239](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/views.py:1239), [test_e4c_f5_voltaD_corridas.py:117](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:117), [test_e4c_f5_voltaD_corridas.py:144](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/test_e4c_f5_voltaD_corridas.py:144), [validacao.py:1103](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/validacao.py:1103), [validacao.py:1112](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/services/validacao.py:1112)

**Comandos para o Code**, a partir de `app/`, com os vereditos esperados indicados em cada linha. [config/settings_test.py:1](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/config/settings_test.py:1), [e4c_f5_voltaD_mutantes.py:55](/home/ronald/wt-codex-e4c-f5-constroi-111415/app/chamados/tests/e4c_f5_voltaD_mutantes.py:55)

```bash
f5_mod=chamados.tests.test_e4c_f5_voltaD_corridas
f5() {
  F5_VOLTA_D_MUTANTE="$1" PYTHONDONTWRITEBYTECODE=1 \
    python3 -B manage.py test --settings=config.settings_test --noinput "$2"
}

f5 ''                 "$f5_mod"                                            # OK, 6 testes [nao verificado]
f5 corrigir           "$f5_mod.CorridasValidacaoVoltaDTest.test_RED_corrigir"       # FAILED
f5 declaracao         "$f5_mod.CorridasValidacaoVoltaDTest.test_RED_declaracao"     # FAILED
f5 irma_implausivel   "$f5_mod.CorridaIrmaVoltaDTest.test_RED_irma_implausivel"     # FAILED
f5 lote_entrada       "$f5_mod.CorridasValidacaoVoltaDTest.test_RED_lote_entrada"   # FAILED
f5 lote_releitura     "$f5_mod.CorridasValidacaoVoltaDTest.test_RED_lote_releitura" # FAILED
f5 casefold_projecao  "$f5_mod.TextoLiteralVoltaDTest.test_RED_casefold"            # FAILED
f5 strip_ato          "$f5_mod.TextoLiteralVoltaDTest.test_RED_casefold"            # FAILED
```

## 21/11 11:12 · sessao 1c6fa39f4814 · FERIADO-POR-POSTO · verificador · origem=Code

**BLOQUEIA o pouso.** A prévia pode mostrar uma troca diferente da executada, duas aplicações concorrentes podem deixar a marca e a chave em sentidos opostos, e a condição `sombra_c2b = 0` no dia do pouso está [nao verificado]. `app/colaboradores/services/feriado_do_posto.py:285`, `app/colaboradores/services/feriado_do_posto.py:373`, `app/colaboradores/services/feriado_do_posto.py:406`, `app/docs/BACKLOG.md:1966`

## Achados

1. **BLOQUEIA — A1 e C4. A marca e a chave podem divergir após duas aplicações bem-sucedidas.** O aval pede “a marca do posto manda” e “1 linha de trilha DA TROCA DA CHAVE”. A porta monta o plano com a instância recebida, grava marca e evento num `atomic`, retrata um chamado por `atomic` e propaga um vínculo por `atomic`. Exemplo: posto sem marca e vínculo `False`. A planeja OPERA e para após gravar a marca. B lê OPERA, aplica FECHA e encontra a chave já `False`, portanto não a propaga. A termina e grava a chave `True`. Resultado: posto FECHA, vínculo `True`, ambas as chamadas retornam sucesso. Duas chamadas OPERA com planos velhos também podem gravar duas trilhas da mesma troca. Falta releitura protegida da marca e da chave antes de decidir cada escrita. `app/colaboradores/services/feriado_do_posto.py:263`, `app/colaboradores/services/feriado_do_posto.py:285`, `app/colaboradores/services/feriado_do_posto.py:306`, `app/colaboradores/services/vinculo.py:894`

2. **BLOQUEIA — C4 e L-007. Uma falha de auditoria pode consolidar uma troca sem trilha.** Falha no `INSERT` de `LogAuditoria` → `evento` desfaz seu savepoint, registra o erro e devolve `None`. A porta ignora esse retorno, confirma a marca ou a chave e segue. A próxima chamada com o mesmo valor pula a trilha, portanto o buraco fica permanente. Há ainda um caminho independente para trilha sem autor humano: `--por` com usuário inexistente resolve para `None` e o comando aplica normalmente. `app/docs/LEIS.md:65`, `app/core/observ.py:94`, `app/core/observ.py:124`, `app/core/auditoria.py:17`, `app/colaboradores/services/feriado_do_posto.py:289`, `app/colaboradores/services/vinculo.py:894`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139`

3. **BLOQUEIA — A5 e 1530 item 2. “Passam a pagar” pode ser falso.** Uma única entrada num feriado passado, sem saída, basta para `turnos_do_colab` devolver um turno aberto. A prévia põe o dia em `(a) PASSAM A PAGAR`, embora a prova do valor dependa do par. A função também chama o pareador **depois** do rollback. Como o pareador lê célula, geradora e DNA, ela pode classificar pelo turno formado com o cadastro de antes. No lado `(b)`, hoje à noite sem batida sai como `futuro` mesmo após o fim da jornada, e ontem sai como “falta a cobrar” sem consultar se o dia encerrou ou se há veto de cobrança. O conteúdo autorizado em 1035 item 2 — batidas, nome da regra e `sem apuracao ainda` — aparece, mas o rótulo da lista promete um resultado que a classificação não verificou. `app/colaboradores/services/feriado_do_posto.py:378`, `app/colaboradores/services/feriado_do_posto.py:401`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:72`, `app/ponto/turnos.py:1850`, `app/ponto/turnos.py:1889`, `app/ponto/turnos.py:2038`, `app/ponto/calculador/regras.py:267`

4. **BLOQUEIA — C2 e 1530 item 3. O zero exigido no pouso não está demonstrado.** O último contraexemplo documentado é vínculo `1355`, 12/10, empresa 2. A guarda devolveu aberta, a célula virou de folga para trabalho, o fechamento ganhou 7 h de HE 100 de feriado e 26 linhas de `DiaPago` foram regravadas. O corte registra 10/2026 das empresas 2, 3 e 4 como paga fora do sistema. O código segue o aval ao usar K9, sem criar outra consulta de “paga”. K9 considera TXT vigente ou holerite publicado, e a virada da janela depende do corte cadastrado da empresa. A saída de 12/10 da janela no calendário de corte 21 explica uma forma de chegar a zero. A remedição de todo o universo em 21/11 ou no efetivo dia do pouso está [nao verificado]. `app/docs/CORTES.md:529`, `app/docs/RELATO.md:1366`, `app/folha/export.py:597`, `app/ponto/management/commands/gerar_celulas.py:170`, `app/ponto/janelas.py:36`, `app/ponto/janelas.py:66`

5. **CORRIGE — A2 e R3. Reaplicar FECHA tem duas semânticas.** O literal de A2 é “marca não muda; só propaga aos vínculos divergentes”. `reaplicar_marca` cumpre isso passando `_so_propaga=True`. Já `--fecha` e a prévia chamam `aplicar_marca` diretamente. Posto já FECHA, chave convergente e chamado vivo com célula de folga → `reaplicar_marca` mantém o chamado aberto, mas `--fecha --apply` pode fechá-lo e anexar nota. Essa segunda semântica permite retomar uma retratação interrompida, como o teste comprova. A CLI e a prévia não distinguem essa retomada da reaplicação de A2. Também usam `qs_na_fila` para o literal “chamados vivos”, deixando de fora registrados e silenciados, e o calendário carregado só cobre `[hoje − 180, hoje]`. Um chamado silenciado hoje ou um feriado de 181 dias atrás fica de fora. `app/colaboradores/services/feriado_do_posto.py:246`, `app/colaboradores/services/feriado_do_posto.py:296`, `app/colaboradores/services/feriado_do_posto.py:313`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:144`, `app/chamados/services/painel.py:158`, `app/colaboradores/tests/test_feriado_por_posto.py:333`, `app/colaboradores/tests/test_feriado_por_posto.py:532`

6. **CORRIGE — Retomada cobre exceção propagada e perde efeitos que falham após o commit.** A marca permanece após queda na retratação ou na segunda propagação. Status e nota do chamado ficam no mesmo `atomic`, e cada troca de chave fica com a regeneração síncrona no seu `atomic`. Uma nova chamada com instâncias relidas refaz o que ainda diverge. Mas chave confirmada + queda antes do callback de folha, ou falha interna desse callback → a próxima chamada vê a chave igual e não agenda o recálculo novamente. O A4 pode ficar em zero enquanto a folha continua velha. O mesmo teste de chave igual impede retentar a invalidação de `previsto_em` quando o signal falha. `app/colaboradores/services/feriado_do_posto.py:288`, `app/colaboradores/tests/test_feriado_por_posto.py:502`, `app/colaboradores/tests/test_feriado_por_posto.py:532`, `app/colaboradores/services/vinculo.py:894`, `app/ponto/services/fechamento.py:1798`, `app/escala/signals.py:144`

7. **CORRIGE — Travas e relógio. A prévia muda a duração e a composição das travas.** A obra não acrescenta trava explícita de posto. A propagação alcança a trava preexistente das células futuras, ordenada por colaborador e data. A retratação chama a ordem preexistente `Colaborador → Disputa → Chamado → Pergunta` e pode, depois dela, travar o contador DP e as âncoras. Na aplicação comum as fases confirmam separadamente. Na prévia, o `atomic` externo conserva as travas bem-sucedidas até o rollback e passa por vários colaboradores sem pré-travar o conjunto por PK. Duas prévias sobre pessoas em comum podem pedir essas travas em ordens diferentes. Deadlock observado [nao verificado]. A CLI injeta um `hoje`, mas `qs_na_fila` lê o instante novamente, o sincronizador deixa a porta ler `localdate()` por vínculo e o signal do núcleo, o cartório e o E2 voltam a ler o relógio. Um lote que cruza meia-noite pode usar um dia para escolher os vínculos e outro para julgar ou materializar turnos. `app/ponto/services/cadastro_futuro.py:21`, `app/ponto/services/cadastro_futuro.py:43`, `app/chamados/models.py:770`, `app/chamados/models.py:2816`, `app/chamados/services/fecho_runtime.py:130`, `app/colaboradores/services/feriado_do_posto.py:370`, `app/chamados/services/painel.py:186`, `app/colaboradores/services/vinculo.py:905`, `app/ponto/signals.py:63`, `app/ponto/services/cartorio.py:2064`, `app/chamados/services/feriado_reavaliar.py:86`

8. **CORRIGE — A5. A saída usa objetos já mudados em memória e perde parte do efeito.** Vínculo `False` → a porta o coloca em `plano['propagar']`, altera a própria instância para `True` e a devolve após o rollback. A CLI pode imprimir `True -> True`. A fotografia e o ato montam planos separados, sem trava, então um vínculo que entre entre as consultas pode ficar fora da foto. A classificação volta a usar o município da instância original do chamador, mesmo que a porta tenha usado um posto relido. Com município velho em memória, o feriado municipal afetado pode cair em `outras`. No FECHA, trabalho → folga cai em `outras`, cujo detalhe é limitado a dez linhas na CLI. Uma mudança de ata, veredito ou carimbo sem alteração em `trabalha`, `dna` ou geradora também fica invisível à fotografia. `app/colaboradores/services/vinculo.py:897`, `app/colaboradores/services/feriado_do_posto.py:358`, `app/colaboradores/services/feriado_do_posto.py:373`, `app/colaboradores/services/feriado_do_posto.py:389`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:83`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:131`, `app/ponto/portas/celula.py:460`

9. **CORRIGE — RESPOSTAS-2011-1100 itens 3 e 4. A recusa morde, mas a prova dos efeitos é parcial.** Para posto existente com uma única ação, `HASNER_SOMBRA` ausente, `False`, `1` ou string → chamada direta levanta `PreviaForaDaSombra`; a CLI converte em `CommandError` antes do plano e da porta, após ler e imprimir o posto. `--sugestoes` sem posto é outra leitura e roda fora da sombra. Posto inexistente, flags incompatíveis, FECHA sem município na sombra e `--apply` sem motivo apenas imprimem erro e retornam normalmente. A prova (b) compara chave e valor, inclui `Posto`, `EscalaColaborador` e `FechamentoMensal.previsto_em`, e tem controles positivos de OPERA e FECHA. A foto omite `TurnoMaterializado`, `DisputaSupervisao`, `Pauta` e `ContadorPauta`, atingíveis pelos signals ou pelo coletor de recusas. O controle de `PerguntaDisputa`/`EventoCelula` aceita uma tabela apenas povoada, sem exigir mudança. E o controle de `on_commit` exige qualquer callback, então o geofence do simples `Posto.save()` já pode fazê-lo passar. `app/colaboradores/services/feriado_do_posto.py:351`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:39`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:99`, `app/colaboradores/management/commands/marcar_feriado_do_posto.py:139`, `app/colaboradores/tests/test_feriado_previa_da_marca.py:36`, `app/colaboradores/tests/test_feriado_previa_da_marca.py:154`, `app/colaboradores/tests/test_feriado_previa_da_marca.py:265`, `app/ponto/signals.py:291`, `app/ponto/nucleo.py:57`, `app/chamados/services/fecho_runtime.py:130`

10. **CORRIGE — A prévia ainda alcança emissores síncronos.** OPERA num feriado passado com falta → regeneração → cartório → `detectar_ausencias` → push da supervisão e, pela disputa, Web Push e FCM do colaborador. FECHA também pode chegar ao cartório pelo `post_save` do chamado. O rollback recolhe as linhas de banco e os `on_commit` capturados, incluindo recálculo, E2, geofence, fechamento de beco e o sentinela da trava. Ele não recolhe push já enviado, linha de log nem incremento de `_ENGOLIDOS`. A guarda só exige o booleano `True`. O perfil da sombra usa banco/host próprios, VAPID vazio e máscara de endereços, mas o runner mostrado usa rede Docker e proxy morto. Assim, um processo com `HASNER_SOMBRA=True` e perfil efetivo diferente passa pela guarda. As provas não interceptam o transporte nem medem emissão com destinatário alcançável. Envio externo na sombra fornecida [nao verificado]. A classe foi registrada para O-OUTBOX conforme 1100 item 3. `app/colaboradores/services/feriado_do_posto.py:351`, `app/config/settings/sombra.py:47`, `bin/sombra.sh:182`, `bin/sombra_mascara.py:68`, `app/ponto/services/cartorio.py:1839`, `app/ponto/management/commands/detectar_ausencias.py:777`, `app/chamados/services/disputa_emissao.py:1303`, `app/chamados/services/disputa_emissao.py:1809`, `app/chamados/signals.py:210`, `app/chamados/services/trava_do_colaborador.py:29`, `app/core/observ.py:73`, `app/docs/BACKLOG.md:1980`

11. **CORRIGE — 1530 item 4 e Single Source of Truth. O predicado é compartilhado, o valor ainda tem caminhos paralelos.** O literal manda os dois copiadores gravarem pela “MESMA função que a porta da marca e o contador A4 usam”. Ajustar conserva o posto e troca o template. Desfazer, no ramo que cria, usa o posto e o template do vínculo antigo. Ambos chamam `chave_ao_nascer` e registram a mudança em relação à chave herdada. O terceiro copiador, `criar_trecho_retroativo`, conserva `ec.posto`, cria inativo e copia a chave, como autorizado. Já a porta compara `!= novo` e o A4 compara diretamente com a marca gravada. Os três compartilham a elegibilidade, mas apenas os copiadores chamam `chave_ao_nascer`. Há um segundo cálculo efetivo no nascimento normal: administrativo em posto OPERA → `executar_vinculo` nasce com `True`; o copiador, com herdada `False`, mantém `False`. Reaproveitar vínculo na mesma data ainda troca posto e template com `save()` inteiro e conserva a chave velha. O ramo `volta == novo.data_inicio` também reabre o PK antigo sem reler a marca, situação já registrada em fila. `app/colaboradores/services/feriado_do_posto.py:174`, `app/colaboradores/services/feriado_do_posto.py:186`, `app/colaboradores/services/feriado_do_posto.py:213`, `app/colaboradores/services/feriado_do_posto.py:263`, `app/colaboradores/services/vinculo.py:259`, `app/colaboradores/services/vinculo.py:976`, `app/colaboradores/services/vinculo.py:1093`, `app/colaboradores/services/vinculo.py:1108`, `app/colaboradores/services/vinculo.py:1128`, `app/escala/models.py:905`, `app/docs/BACKLOG.md:1985`, `app/docs/BACKLOG.md:1986`

12. **CORRIGE — A4 existe como comando; falta o vigia no ESTADO.** O literal é “entra no ESTADO”, esperado zero, dono CADASTRO. O comando conta os dois sentidos, ordena os PKs, imprime até 50 e é somente leitura. Está em `FORA_DE_PIPELINE`, sem horário e sem papel de linha de cron. O trecho do placar publica o contador antigo de 5x2/6x1, que mede outra coisa. Exemplo: novo divergente `FECHA × True` → o comando novo conta, mas a execução normal do placar não publica A4. Não há cron novo para desligar. A ação que zera a divergência é a reaplicação por CADASTRO. O valor histórico `J=11` está registrado, não certificado na frota desta árvore. `app/config/crons.py:1242`, `app/config/crons.py:2375`, `bin/placar_code.sh:296`, `app/colaboradores/management/commands/vinculos_divergentes_da_marca_do_posto.py:14`, `app/colaboradores/services/feriado_do_posto.py:213`, `app/docs/RELATO.md:1450`

13. **CORRIGE — R8. O selo do escritor único deixa passar escritas reais.** Em memória, o censo devolveu zero para `posto.save()` inteiro, `qs.update(**dados)` com a marca no dicionário, `bulk_update(..., CAMPOS)`, construtor `Posto` apelidado, `setattr` com campo em variável e `getattr` com nome da porta em variável. Ele detecta os equivalentes literais e os apelidos testados da função. Duas chamadas dentro de `aplicar_marca` viram um único sítio pela deduplicação `(arquivo, função)`. Já existe uma entrada genérica que ele não vê: `criar_posto(**dados)` aceita `opera_em_feriado` e gravaria a marca sem passar pela porta; a tela atual monta um dicionário explícito sem a marca. `atualizar_posto` e liga/desliga agora limitam as colunas, e o Django admin usa `SoLeitura`. `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:86`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:125`, `app/colaboradores/tests/test_selo_marca_do_posto_um_escritor.py:137`, `app/colaboradores/services/estrutura.py:43`, `app/colaboradores/services/estrutura.py:67`, `app/colaboradores/views.py:851`, `app/colaboradores/admin.py:32`

14. **CORRIGE — Merge e selos c12. O modo base ORM quebra antes de provar o desligamento.** S06 declara a função antiga, S19 declara arquivo e função antigos, e o enxerto usa o corpo atual revertido para ambos. Os testes estáticos passaram e `--mutante S19` deu a falha esperada no sítio da porta. Para S20, o enxerto ainda seleciona a função congelada `(request, colaborador, v)`. O teste atual chama `executar_desligamento(..., hoje=...)`, como exige a assinatura atual. Entrada `E4B_C12_MUTANTE=base` no desligamento → `TypeError` antes do gate. É uma interação concreta entre o código atual e o verificador tocado pela obra. O RELATO registra três erros dessa classe. Também falta prova ORM específica de S19 que exercite recusa e Pauta DP, além do selo AST. `app/chamados/tests/e4b_c12_sitios.json:43`, `app/chamados/tests/e4b_c12_sitios.json:148`, `app/chamados/tests/e4b_c12_base.json`, `app/chamados/tests/e4b_c12_verificar.py:111`, `app/chamados/tests/e4b_c12_verificar.py:172`, `app/chamados/tests/test_e4b_c12_orm.py:110`, `app/colaboradores/services/desligamento.py:82`, `app/docs/RELATO.md:50`

15. **FILA — 1100 item 4. A exportada preserva a célula sob K9, mas perde o carimbo do previsto.** Vínculo divergente com `previsto_em` preenchido em competência exportada → o `post_save` zera esse campo e atualiza `atualizado_em` em todas as competências carimbadas do colaborador. O leitor de prontidão deixa de usar o snapshot e passa à leitura viva. A regeneração consulta K9 na janela anterior e barra as células lavradas, tanto no passado quanto no suplemento das futuras existentes. O fechamento mensal e `DiaPago` são recalculados por competência tocada após commit. R5 preserva a célula, um campo de `FechamentoMensal` e os valores de `DiaPago` na sua fixture. O aval manda registrar a invalidação sem curar, e o registro existe. Inverter a marca é outro ato e pode recalcular de novo. O reversor de célula restaura o DNA imediatamente anterior e a obra não liga a ele uma reversão integral de folha, chamados e carimbos. `app/escala/signals.py:137`, `app/ponto/services/fechamento.py:1607`, `app/folha/export.py:1045`, `app/ponto/portas/celula.py:218`, `app/ponto/portas/celula.py:284`, `app/ponto/portas/celula.py:483`, `app/ponto/portas/celula.py:779`, `app/ponto/services/dia_pago.py:359`, `app/colaboradores/tests/test_feriado_por_posto.py:367`, `app/docs/BACKLOG.md:1982`

## Escritores, leitores e significado

| Campo ou valor | Caminhos conferidos |
|---|---|
| `Posto.opera_em_feriado` | A porta chama o `save(update_fields=...)` de `estrutura`. Leem a marca o veto de emissão, as sugestões e a pauta, o nascimento normal, os copiadores, o sincronizador, o A4, o plano e a CLI. `None` significa sem declaração, `False` veta emissão no feriado do posto, `True` deixa seguir os demais gates. **Segundo juiz de efeito, preexistente:** 12x36 com plantão previsto em posto FECHA → `fatos_do_dia` preserva o trabalho, enquanto o veto direto da marca pode bloquear o furo. A diferença também importa para células preservadas de vínculos weekday. `app/colaboradores/services/estrutura.py:90`, `app/colaboradores/services/feriado_do_posto.py:60`, `app/colaboradores/services/feriado_do_posto.py:127`, `app/colaboradores/services/feriado_do_posto.py:136`, `app/escala/models.py:905`, `app/escala/models.py:929`, `app/ponto/precedencia.py:376`, `app/ponto/management/commands/detectar_ausencias.py:616`, `app/core/juizes.py:310`, `app/docs/JUIZES.md:114` |
| `EscalaColaborador.trabalha_em_feriado` | Escrita explícita pela porta do vínculo, chamada pela marca e pelo sincronizador. Escrita no nascimento normal, nos três copiadores e por `save()` inteiro no reaproveitamento. O gerador lê `feriado_vira_folga`; precedência, fechamento e calculador também o chamam. A4 e propagação usam vigência com início e fim inclusivos, inclusive inativo com fim declarado que cobre o dia. O sincronizador começa de `ativa=True`. Vínculo futuro já criado fica fora do A4 e da propagação de hoje. `app/colaboradores/services/vinculo.py:283`, `app/colaboradores/services/vinculo.py:296`, `app/colaboradores/services/vinculo.py:898`, `app/colaboradores/services/vinculo.py:983`, `app/colaboradores/services/vinculo.py:1118`, `app/colaboradores/services/vinculo.py:1142`, `app/colaboradores/management/commands/sincronizar_feriado_admin.py:42`, `app/colaborad

[truncado em 20000 de 28633 caracteres]

## 21/11 10:54 · sessao 681cb0feb709 · E4C-F5 · verificador · origem=Code

**AVAL-DEVOLVIDO — RESPOSTAS-2111-1050, item 1, Q3.** Incide o **PAREI com a lista** pelo motivo de recusa produzido em `validacao.py:1112` na corrida de `lote_releitura`. `aval.txt:3–4`, `validacao.py:1090–1114`, `_lote_preview.html:6–8`.

### 1. Premissas — NAO PROCEDE

As cinco referências conferem. Em cada função, a âncora ocorre uma vez e a substituição muda só a linha indicada. Os cinco testes cabem em `.py` de teste, com sincronização em torno das leituras reais e o escritor gravando sobre outra instância do mesmo PK. `e4c_f5_voltaD_mutantes.py:14–37`, `e4c_f5_voltaD_mutantes.py:49–66`, `test_e1_identidade_corrida.py:25–71`. [nao verificado]

| Mutante | Arquivo, função e troca exata | Escritor real e resultado que discrimina |
|---|---|---|
| `corrigir` | `acoes_disputa.py:349`, `acao_corrigir_resposta`. A guarda de resposta já validada troca `DecisaoPergunta.composta(perg).validada_em` por `perg.validada_em`. `e4c_f5_voltaD_mutantes.py:14–15`. | `validacao.py:161`, `_carimbar_validacao`, pela `validar_pergunta`. Assinar depois do `get` da correção. A original retorna `info` e preserva a resposta. O mutante atravessa a guarda e pode sobrescrevê-la pela porta de resposta. `validacao.py:760`, `acoes_disputa.py:345–350`, `acoes_disputa.py:374–388`. |
| `declaracao` | `declaracao_texto.py:229`, `_resposta_validada`. O `is not None` passa da composta para `perg.validada_em`. `e4c_f5_voltaD_mutantes.py:16–17`. | `validacao.py:161`, `_carimbar_validacao`. Assinar entre a carga da pergunta e o prefetch das decisões. A projeção por marco ou a proposta dá `aceita=False` na original e `True` no mutante. A aplicação por `followup_id` refaz a guarda sob trava, então o teste deve observar `aceita`. `declaracao_texto.py:280–287`, `declaracao_texto.py:475–480`, `declaracao_texto.py:634–639`, `declaracao_texto.py:651–670`. | [nao verificado]
| `irma_implausivel` | `materializacao.py:203`, `par_do_intervalo`. Troca `leitura_da_irma.validada_em is None` por `q.validada_em is None` na guarda que chama o juiz. `e4c_f5_voltaD_mutantes.py:23–24`. | `validacao.py:161`, `_carimbar_validacao`, após a confirmação administrativa com `confirmar_implausivel=True` e com a guarda do U163 satisfeita. Assinar depois da busca da irmã, antes da composta, no caminho da tela. A original pula o juiz da irmã. O mutante o chama e pode devolver a recusa por hora implausível. `materializacao.py:195–208`, `materializacao.py:925–928`, `validacao.py:714–723`, `validacao.py:760`. |
| `lote_entrada` | `validacao.py:1082`, `validar_lote_classe_a`. A guarda de `Ja validada.` troca a composta por `perg.validada_em`. `e4c_f5_voltaD_mutantes.py:32–34`. | `validacao.py:161`, `_carimbar_validacao`. Assinar entre a carga principal e o prefetch. A original recusa com `Ja validada.`. O mutante chega à reclassificação. `validacao.py:1074–1087`. |
| `lote_releitura` | `validacao.py:1104`, `validar_lote_classe_a`. Após o refresh integral, a guarda que incrementa `validadas` troca a composta por `perg.validada_em`. `e4c_f5_voltaD_mutantes.py:35–37`. | `disputa_emissao.py:1439`, `responder_pergunta_disputa_core`. Depois do refresh do lote, o escritor relê sob trava, limpa a coluna e grava `RESPOSTA` com `resposta_mantem_validacao=False`. A original põe o PK em `recusados`, com `validadas=0`. O mutante conta `1` pelo carimbo que ficou na instância. `disputa_emissao.py:1478–1489`, `disputa_emissao.py:1503–1537`, `models.py:4370–4376`, `validacao.py:1103–1114`. |

**Classe A real.** `FilaValidarLoteTest._p('15:08')` cria uma pergunta `hora`, motivo `orfao_14h`, com marco de saída às `15:00`. A fixture já tem asserções de `LOTE_A` e de assinatura persistida pelo botão do lote. O escritor da assinatura é `_carimbar_validacao`, chamado via `acao_validar_pergunta`. Para a corrida, a segunda resposta passa pela guarda web enquanto a pergunta está viva, com `resposta_tocou=1`, e espera na entrada do escritor até o lote concluir o `refresh_from_db()`. A sincronização no refresh funciona também sob o mutante, que elimina a consulta à composta em `:1104`. A execução dessa corrida com a fixture é [nao verificado]. `test_fila_validar_lote.py:16–44`, `test_fila_validar_lote.py:103–110`, `acoes_disputa.py:13–22`, `validacao.py:760`, `validacao.py:1096–1104`, `views.py:1239–1278`, `disputa_emissao.py:1587–1594`.

**Casefold, item 32.** O mutante principal aplica `.lower()` só à projeção em `vigia_texto_resposta.py:23`. Com ato `'ato'` e projeção `'Ato'`, `contar()` dá `(1, 1)` na original e `(0, 1)` no mutante. O teste atual põe a maiúscula no ato. O mesmo método de teste pode cobrir o outro mutante assimétrico da tabela, `.strip()` só no ato, com ato `'ato '` e projeção `'ato'`. Cada variante pode ser ativada em memória pelo teste, no molde do seletor existente. A execução dos mutantes assimétricos é [nao verificado]. `tabela_item5.md:39`, `vigia_texto_resposta.py:19–25`, `test_e4c_f5_contador_texto.py:21–24`, `test_e4c_f5_contador_texto.py:40–53`, `e4c_f5_voltaD_mutantes.py:55–66`. [nao verificado]

### 2. Colisão — NAO PROCEDE

O 1050 substitui expressamente o item 2 do 1010 e inclui o teste do item 32 que a tabela apontava como fora do escopo anterior. Os testes de corrida são compatíveis com o selo da classe vii. O escritor mantém ato e coluna pareados, e a diferença aparece entre as leituras do leitor. A instrumentação cabe em `.py` de teste e respeita o `PROIBIDO` do aval. O teor integral dos avais citados apenas por caminho é [nao verificado]. `aval.txt:1–4`, `aval.txt:8–9`, `r_1010.txt:17–19`, `tabela_item5.md:39`, `RELATO.md:283`, `validacao.py:167–175`, `models.py:4361–4376`, `e4c_f5_voltaD_mutantes.py:49–66`. [nao verificado]

### 3. Literal — PROCEDE

**PAREI com a lista — `validacao.py:1112`, `validar_lote_classe_a`.** Se a ação individual confirma a validação e a resposta concorrente a desfaz na janela pedida, a releitura põe corretamente o PK em `recusados`. O motivo, porém, é copiado do sucesso anterior. A tela pode mostrar **“Pergunta validada e ponto gravado no espelho.”** sob **“Ficaram de fora”**, sem explicar que outra resposta desfez a validação. A inconsistência decorre dos ramos do código. Sua reprodução com banco é [nao verificado]. `aval.txt:4`, `validacao.py:853–854`, `acoes_disputa.py:57–64`, `validacao.py:1090–1114`, `_lote_preview.html:6–8`.

Um teste por mutante é viável. **Nenhum dos cinco é equivalente.** Nos quatro primeiros, a instância ainda tem coluna nula e a composta já vê a assinatura. Na releitura, a instância ainda tem o carimbo e a composta já vê a resposta que o desfez. Para provar a mordida, o teste precisa entrar em `mutar_sitio` com o seletor correspondente e rodar a mesma asserção contra a original e o mutante. Os cinco resultados de execução são [nao verificado] nesta revisão. `models.py:4361–4376`, `validacao.py:161–175`, `disputa_emissao.py:1503–1537`, `e4c_f5_voltaD_mutantes.py:49–66`, `tabela_item5.md:34`, `tabela_item5.md:40`. [nao verificado]

### 4. Alcance — NAO PROCEDE

Os cenários cabem em competência aberta no banco de teste. A porta real pode materializar ponto e disparar recálculo na fixture, e a classe A lê a tolerância do motor. O item dispensa escrita em competência exportada, alteração de dinheiro em produção e qualquer `!` adicional. Efeitos em produção e a identidade do hash por metadados Git são [nao verificado]. `aval.txt:4`, `aval.txt:9`, `test_fila_validar_lote.py:19–40`, `validacao.py:757–772`, `validacao.py:786–803`, `validacao.py:1147–1150`, `CLAUDE.md:265–268`, `CLAUDE.md:401–417`. [nao verificado]

## 21/11 10:38 · sessao b97c9e232059 · E4C-F5 · leitura · origem=Code

**E4C-F5 — publicação para a cópia identificada como `b0935c1f106d232dc39b263676a588d66eddff54`.** O `git log -p` retornou `fatal: not a git repository`. A confirmação do hash pelo Git, os diffs históricos e os resultados nominais de execução no integrado ficam [nao verificado].

## 32 — `casefold` no contador de texto

O contador compara literalmente o texto vigente do ato com o texto da coluna, usando `or ''` na coluna. `DecisaoPergunta.leitura` usa o `ato_texto` da sequência e pode sintetizar a resposta do passivo com o texto da própria coluna. `DecisaoPergunta.composta` entrega o texto da coluna aos juízes. O juiz L-130 testa presença de instante e texto não vazio após `strip()`. [vigia_texto_resposta.py:23](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/vigia_texto_resposta.py:23), `app/chamados/models.py:4313–4322,4361–4379,4388–4390`, `app/chamados/juizes.py:1533–1535,1551–1560`.

Os leitores de **resposta** que usam `lower()` são `classificar_resposta_validacao`, para reconhecer escolhas afirmativas, e `classe_do_lote`, para reconhecer escapes e escolhas após também retirar acentos. Retirar `lower()` do primeiro mudaria o caso `Confirma` em `test_confirma_e_classe_a_diferente_e_b_vazio_none`. Retirá-lo do segundo mudaria `ATESTADO` e `Confirma` nos testes de classe do lote. O `casefold()` localizado na busca de turno compara o termo com o nome canônico do turno. `app/chamados/juizes.py:2043–2056`, `app/chamados/services/validacao.py:1153–1154,1181–1183,1196–1212`, `app/chamados/models.py:4159`, `app/chamados/tests/test_a27_recusa_com_dono.py:110–114`, `app/chamados/tests/test_fila_validar_lote.py:42–59`, `app/escala/views.py:227–228`.

`test_diferenca_so_de_caixa_conta` registra uma `RESPOSTA` com `ato_texto='Ato'`, atualiza diretamente a projeção para `resposta_colab='ato'` no mesmo instante e exige `(1, 1)`, uma divergência em uma pergunta. A prova é da comparação do contador diante desse par construído pela fixture. A ocorrência desse par pelo fluxo normal ou em produção fica [nao verificado]. [test_e4c_f5_contador_texto.py:50](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_contador_texto.py:50), `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,50–53`.

| Parecer | Registro em uma linha |
|---|---|
| Terra | **NAO BLOQUEIA**, com fundamento integral [nao verificado]. `app/docs/RELATO.md:1374` |
| Sol 6.1 | **BLOQUEIA**, com fundamento integral [nao verificado]. `app/docs/RELATO.md:1374,1377` |
| Sol 6.2 | **BLOQUEIA** e registra, fora do Opus, que `strip()` só no lado do ato sobrevive ao RED de espaços. `app/docs/RELATO.md:1377`, `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–48` |

O trecho raspado de `relato_trechos.txt` traz 6-A1, H4 e CADEIA-20. Para os pareceres de `casefold`, as âncoras disponíveis são as do RELATO. A transformação exata do sobrevivente histórico e seu recibo ficam [nao verificado]. `app/docs/RELATO.md:94,1374,1377,1439`.

| Mutação no contador | Efeito por leitura do teste | [nao verificado]
|---|---|
| Inserir `casefold()` ou `lower()` no ato ou nos dois lados | `Ato` × `ato` vira `ato` × `ato`. `test_diferenca_so_de_caixa_conta` receberia `(0, 1)` e falharia. `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53` |
| Inserir `casefold()` ou `lower()` só na projeção | Sobrevive ao teste de caixa, pois a projeção já contém `ato`. Os demais asserts do módulo também são compatíveis com a variante. O teste de carimbo vizinho calcula a expectativa chamando o próprio contador. `app/chamados/tests/test_e4c_f5_contador_texto.py:21–53,56–70,72–95`, `app/chamados/tests/test_o261_vigia_fio.py:842–850` |
| Retirar `casefold()` do contador atual | A expressão de comparação oferece zero ocorrências para a remoção. Uma rodada que deixe a expressão intacta mede o controle. `app/chamados/services/vigia_texto_resposta.py:21–25` |
| Inserir `strip()` só no ato | Sobrevive ao RED de espaços. As duas variações plantam espaço na coluna e deixam o ato em `ato`. Esta é a variante registrada por Sol 6.2. `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–48`, `app/docs/RELATO.md:1377` |

**Destinos possíveis**

1. **Equivalente sob um invariante demonstrado.** Para normalização **simétrica**, uma prova de igualdade literal em todo o universo do contador bastaria. A porta humana trava o colaborador, relê e grava a mesma resposta na coluna e no ato. A automática também grava o mesmo valor, e a porta pela trilha copia a coluna. A guarda de `carimbar` verifica presença de **validação**, e a guarda do `save` verifica presença conjunta de **instante e texto na projeção**. Uma trava que garanta igualdade textual para todos os pares, inclusive passivo, fica [nao verificado]. Para normalização unilateral, até um par idêntico `Ato` × `Ato` produziria um falso positivo. `app/chamados/services/disputa_emissao.py:1478–1502,1532–1537,1707–1715,1728–1737`, `app/chamados/models.py:3384–3393,3731–3764,4392–4399`, `app/chamados/services/vigia_texto_resposta.py:17–25`.
2. **Teste faltando.** Para normalização só na coluna, `ato` no ato e `Ato` na coluna demonstrariam o falso zero. `Ato` nos dois lados demonstraria o falso positivo. Para `strip()` só no ato, o caso discriminante põe o espaço no ato. Esses casos fixariam a comparação literal também na orientação inversa à das fixtures atuais. `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–53`, `app/docs/RELATO.md:1377`.
3. **Outro destino.** Publicar **sem alvo** para remoção do contador atual, **falha prevista pelo RED** para inserção no ato ou nos dois lados, ou **pendente de identificação** para o sobrevivente histórico. O RELATO também registra `M_strip_velho` verde e `M_strip_novo` vermelho com o caso só de espaço, em rodada histórica distinta. `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:40–53`, `app/docs/RELATO.md:94,1439–1440`.

**Escrita e leitura.** A porta humana e a automática escrevem `resposta_colab` e fornecem `ato_texto` a `DecisaoPergunta.registrar`, que persiste a linha. `leitura` lê `ato_texto`, `resposta_display` o usa na apresentação e compara o valor da opção literalmente, e o contador confronta ato e coluna. Os dois classificadores de resposta leem `resposta_colab`. Seus resultados são usados pelo comando `validar_classe_a`, pelas tags de chamados, pela entrada do lote e pela API de mensageria. O comando `vigiar_fio_morto` chama o contador, imprime divergência e universo e escreve `censo.divergencia_texto_resposta` com esperado zero. `fio_estado` lê o par medido/esperado e exibe o desvio. `app/chamados/services/disputa_emissao.py:1500–1502,1532–1537,1709–1715`, `app/chamados/models.py:3865–3891,4361–4379,4428–4438`, `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/management/commands/validar_classe_a.py:66`, `app/chamados/templatetags/chamados_tags.py:171,198`, `app/chamados/services/validacao.py:1085,1273`, `app/api/views_mensageria.py:545–546`, `app/chamados/management/commands/vigiar_fio_morto.py:104–110,143–150,194–197,231–232,269–283`, `bin/fio_estado.py:121–126,191–194`.

## 80 — K47 e volta D

O RELATO atribui a adaptação da classe vii a `604505878` e remete ao corpo desse commit para os esperados anteriores. Relata que K47 e os REDs fabricavam divergência por `update` direto da coluna, e que K47 estava vermelho em `76ce6695c`. Para K47 em `8938552e1`, registra uma mordida de custo `8!=14`, que é evidência de execução relatada. O asserto substituído, o hunk e sua linha anterior ficam [nao verificado] para cada método. `app/docs/RELATO.md:94,283,752,786,790`.

Na tabela, `B` é [test_e4c_f5_voltaB_lotes.py](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197), `D` é [test_e4c_f5_voltaD_portas.py](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:25) e `M` é [e4c_f5_voltaD_mutantes.py](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:13), todos em `app/chamados/tests/`. `V` representa `VALIDACAO_EM` e `A`, `AGORA`. O helper `_afirmar_par` cobra igualdade de **presença** entre ato e coluna e confere o instante exato **da coluna**. `D:35–67`.

| Método | O que asserta hoje | Esperado anterior, commit e linha | Seletor | [nao verificado]
|---|---|---|---|
| `test_RED_k47_escolhas_sem_credor` | Escolha aberta pela porta, resposta `Sim`, validação pareada em `A`. Fecha uma disputa, zero perguntas resolvidas ou materializadas. Em 3 e 6, julga todos os PKs e os SELECTs de decisões são iguais e positivos. `B:40–52,197–222` | [nao verificado] | `F5_VOLTA_B_MUTANTE=k47`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:11,23,83–115`. Na D, `fecho` e `fecho_sem_lote` apontam para a mesma função de produção. `M:18–19,38–39` |
| `test_RED_corrigir` | Com validação, `info`, preserva `07:13`, instante original e V. Sem validação vigente, `sucesso`, grava `07:18`, instante A e validação A. `D:84–95` | [nao verificado] | `corrigir`. `M:14–15` |
| `test_RED_declaracao` | Mesmo PK. Com V, `validada_sem_registro`, resposta e instante preservados. Sem V, `ok`, `07:18` em A, validação ausente. `D:103–120` | [nao verificado] | `declaracao`. `M:16–17` |
| `test_RED_aplicar_declaracao` | Mesmo PK. `validada_sem_registro` com V e trilha `[V]`, ou `validada` com A e trilha `[V, A]`. `D:122–134` | [nao verificado] | `declaracao`. `M:16–17`, `app/chamados/services/declaracao_texto.py:670` |
| `test_RED_fecho` | Resumo `0/0`. A validada mantém V e fica sem materialização. A aberta fica sem validação e materializa em A pela via `VIA_CHAMADO_ENCERRADO`. `D:142–153` | [nao verificado] | `fecho`. `M:18–19` |
| `test_RED_fecho_3x6` | Preserva os PKs sem materialização e o par em V. Um SELECT de decisões em cada tamanho. `D:155–170` | [nao verificado] | `fecho_sem_lote`, acionado em `D:163`. `M:38–39` |
| `test_RED_propostas_3x6` | Mesmos IDs dos followups, todas as propostas com `aceita=False`, um SELECT de decisões em cada tamanho. `D:172–187` | [nao verificado] | `declaracao` cobre a guarda do percurso. A tabela D tem zero seletores para retirar o prefetch próprio de propostas. `M:13–46`, `app/chamados/services/declaracao_texto.py:284–287,632–640` |
| `test_RED_tela_3x6` | Pares `(dia, aceita)` alternam conforme a fixture, com um SELECT de decisões em cada tamanho. `D:189–211` | [nao verificado] | `tela_sem_lote`, acionado em `D:199`. `declaracao` cobre a guarda da projeção. `M:16–17,40–42`, `app/chamados/services/declaracao_texto.py:479–480` |
| `test_RED_irma_plantio` | Par da irmã em V ou vazio, trilha `[V]`. Com V, retorna irmã e plantio `(A+1h, E)`. Sem V, retorna impedimento de irmã respondida sem validação. `D:213–236` | [nao verificado] | `irma_plantio`. `M:20–22` |
| `test_RED_irma_implausivel` | Mesmo controle do par e da trilha. Com V, aceita o plantio. Sem V, retorna impedimento de hora implausível. `D:213–226,238–246` | [nao verificado] | `irma_implausivel`. `M:23–24` |
| `test_RED_materializar_ato_mudou_sob_trava` | Confere o PK carregado. Resposta sob a trava desfaz V. Com revalidação em A, materializa uma em A. Sem revalidação, materializa zero. `D:248–270` | [nao verificado] | `materializar`. `M:25–26` |
| `test_RED_validar_par` | Retorna a irmã. Primeira metade em V, irmã em V ou A com atos `[V]` ou `[V, A]`, e os dois PKs materializados. `D:272–298` | [nao verificado] | `validar_par`. `M:27–28` |
| `test_RED_validar` | `ja_validada`, V, `[V]` e sem materialização, ou `ja_no_espelho`, A, `[V, A]` e materialização em A. `D:300–310` | [nao verificado] | `validar`. `M:29` |
| `test_RED_recusar` | `ja_validada`, V, zero RECUSAs e `07:13` preservado, ou `recusada`, par vazio, uma RECUSA, texto limpo e motivo registrado. `D:312–322` | [nao verificado] | `recusar`. `M:30–31` |
| `test_RED_lote_entrada` | Com V, recusa `Ja validada.`, zero validadas e `[V]`. Sem V vigente, uma validada em A, lista de recusas vazia e `[V, A]`. `D:324–336` | [nao verificado] | `lote_entrada`. `M:32–34` |
| `test_RED_lote_entrada_3x6` | Todas as refs recusadas como `Ja validada.`, um SELECT de decisões em cada tamanho. `D:338–348` | [nao verificado] | `lote_entrada_sem_prefetch`, acionado em `D:344`. `M:43–45` |
| `test_RED_lote_rele_ato_escrito_em_outra_instancia` | Texto mantém validação em A e entra em `nao_gravados`, com uma validada. Hora perde a validação e entra em `recusados`, com zero validadas. Ambos sem materialização, com prefixos dos avisos e envelope integral conferidos. `D:350–374` | [nao verificado] | `lote_releitura`. `M:35–37` |
| `test_diagnostico_refresh_parcial_e_integral` | Após apagar a coluna na fixture, refresh parcial conserva cache e não vê a validação. Refresh integral limpa cache e a composta volta a ler V. `D:376–386` | [nao verificado] | Sem seletor próprio. `M:13–46` |
| `test_devolucao_e_o_escritor_da_coluna` | Retorna `True`, limpa o par, deixa `validada_em` nula e grava uma `DEVOLUCAO`. `D:388–396` | [nao verificado] | Sem seletor próprio na D. `M:13–46` |

| Método auxiliar da D | Asserto ou papel atual | Esperado anterior | Seletor próprio |
|---|---|---|---|
| `_cenario` | Isola a fixture em transação com rollback. `D:27–33` | [nao verificado] | Nenhum. `M:13–46` |
| `_validacao_original` | Chama o escritor real sob relógio V. `D:35–38` | [nao verificado] | Nenhum. `M:13–46` |
| `_perder_coluna` | Apaga `validada_em` e `validada_por` para o diagnóstico. `D:40–43` | [nao verificado] | Nenhum. `M:13–46` |
| `_afirmar_par` | Compara presença no ato e na coluna e confere instante bruto. `D:45–49` | [nao verificado] | Nenhum. `M:13–46` |
| `_responder` | Exige `ok` da porta de resposta real. `D:51–54` | [nao verificado] | Nenhum. `M:13–46` |
| `_resposta_desfaz_validacao` | Responde pela porta e exige par sem validação. `D:56–58` | [nao verificado] | Nenhum. `M:13–46` |
| `_estado_da_validacao` | Monta V, confere o par e, no ramo sem validação vigente, responde para desfazê-la. `D:60–67` | [nao verificado] | Nenhum. `M:13–46` |
| `_batidas_do_dia` | Insere as batidas da fixture. `D:69–74` | [nao verificado] | Nenhum. `M:13–46` |
| `_post` | Monta POST com o usuário admin da fixture. `D:76–79` | [nao verificado] | Nenhum. `M:13–46` |
| `_validacoes` | Lê `ato_em` das linhas físicas `VALIDACAO`. `D:81–82` | [nao verificado] | Nenhum. `M:13–46` |
| `_chamado_de_entrada` | Cria chamado com data de turno e marcos da fixture. `D:97–101` | [nao verificado] | Nenhum. `M:13–46` |
| `_resolver_sem_credor` | Chama o serviço com o C6 e o fechamento isolados por mocks. `D:136–140` | [nao verificado] | Nenhum. `M:13–46` |
| `_julgar_par` | Executa o juízo de intervalo, confere o par da irmã em V ou vazio e preserva a trilha `[V]`. `D:213–226` | [nao verificado] | Nenhum. `M:13–46` |

**FALTA-EXECUCAO.** `M:49–66` só instala a mutação se o seletor estiver entre os nomes passados ao context manager. Neste módulo, os três ganchos são `fecho_sem_lote`, `tela_sem_lote` e `lote_entrada_sem_prefetch`. Os outros 11 são vínculos ao sítio mutado. Rodar o módulo com a variável, por si só, deixa esses 11 sítios com o código de controle. `D:163,199,344`, `M:13–46,49–66`.

O RELATO registra 14 mutantes D mortos por asserção em `76ce6695c`. Em `c8eb8eddf`, registra 145 mordidas em 155 e oito sobreviventes D, nominalmente `corrigir`, `declaracao`, `irma_plantio`, `irma_implausivel`, `materializar`, `validar_par`, `lote_entrada` e `lote_releitura`. Registra também que `fecho`, `validar` e `recusar` mordem por outro teste. O teste nominal dessas três mordidas e o placar final da bateria de 163, inclusive K47 e os oito, ficam [nao verificado]. `app/docs/RELATO.md:62,786`, `execucoes.txt:30` conforme o anexo fornecido.

- **Publicar os assertos atuais e o histórico relatado** entrega a tabela com o esperado anterior em aberto por método. Recuperar `604505878`, seu pai e os recibos nominais permitiria preencher a coluna histórica e distinguir falha de asserção, sobrevivência e falha de aplicação. `app/docs/RELATO.md:62,94,283`, `M:49–66`.
- **Aceitar equivalência sob a classe vii** exige demonstrar igualdade nas guardas e fotos relevantes. Com a mesma presença de validação nos dois lados, trocar composta por coluna deixa essas guardas com o mesmo resultado. Para `materializar` e `lote_releitura`, importa também qual foto cada leitor vê depois da escrita ou da trava. Um teste que construa divergência ou uma foto defasada distinguiria a fonte e ampliaria o que se cobra desses REDs. `D:35–67,248–270,350–386`, `M:14–37`, `app/chamados/services/materializacao.py:2142–2164`, `app/chamados/services/validacao.py:1096–1114`, `app/docs/RELATO.md:62,283,752`.

**Escrita e leitura no percurso.** `_carimbar_validacao` escreve `validada_em` e `validada_por` e registra `VALIDACAO`; resposta nova, devolução e reabertura podem limpar o par. A D relê a coluna e a composta e consulta a trilha física. `_carimbar_recusa` e `recusar_pergunta` escrevem `recusa_motivo`, que a devolução testa e fotografa. A nota das propostas é escrita por `nota.anexar_ocorrencia`, lida por `propostas_do_chamado`, que produz `aceita`; o fio lê a lista. `validar_lote_classe_a` produz os totais e listas lidos pelas views do lote. `app/chamados/services/validacao.py:161–175,269–320,930–944,1074–1114`, `app/chamados/services/disputa_emissao.py:1500–1537,1630,1664–1683`, `D:45–49,81–82,320–322,396`, `app/chamados/services/materializacao.py:1717–1741`, `app/chamados/services/nota.py:75–103`, `app/chamados/services/declaracao_texto.py:613–639`, `app/chamados/services/fio.py:331`, `app/chamados/views_cobrar.py:263,383`.

## 102 — censo de `.composta()` em `app/`

A varredura sintática encontrou **53 chamadas em 21 arquivos** fora de `tests`. A tabela usa **DECIDE** para resultado que governa seleção, veredito ou gravação, e **EXIBE** para resultado usado exclusivamente em serialização, legenda ou diagnóstico apresentado. São **49 DECIDE e 4 EXIBE** por esse critério. Se o critério for contar também todo ramo que escolhe uma legenda ou categoria de relatório, as três linhas EXIBE marcadas com `*` passam a DECIDE, dando **52 DECIDE e 1 EXIBE**. As duas opções contam uma vez cada chamada. `app/api/views.py:2579–2603`, `app/chamados/management/commands/dedup_perguntas_fato.py:76,95–106`, `app/chamados/services/materializacao.py:294–295,345,364–369`, `app/folha/previa.py:95,139–147`, `app/folha/views.py:760,803`, demais sítios na tabela.

| Chamada | Classe | Onde o resultado é usado |
|---|---|---|
| `app/api/views.py:2587` | DECIDE e também exibe | `:2588–2591` monta o item e decide entrada em `respondidas`. `:2579–2581` serializa texto e instante. |
| `app/api/views.py:2601` | EXIBE | `_item` só usa a leitura passada para os campos de `:2579–2581`. `p.estado` em `:2567` tem sua própria chamada no modelo. |
| `app/api/views.py:2646` | DECIDE | `:2653` passa `ja_respondida` à porta. A porta usa o valor na trilha e na nota persistida de correção ou resposta. `app/chamados/services/disputa_emissao.py:1552–1557,1569–1581` |
| `app/chamados/juizes.py:352` | DECIDE | `:351–354` usa a projeção temporal para retirar o slot. |
| `app/chamados/juizes.py:1408` | DECIDE | `:1409–1411,1421–1428` governa o juízo de vitalidade. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:179` | DECIDE | `:178–183` determina se a contraprova do I21 dispara alarme. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:598` | DECIDE | `:597–603` divide piso e violações do I2. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:1672` | DECIDE | `:1672–1673` exclui o caso da auditoria. |
| `app/chamados/management/commands/dedup_perguntas_fato.py:76` | EXIBE* | `:95–99` usa `com_resp` só na legenda. A escolha da canônica e o `apply` vêm de `:77,8

[truncado em 20000 de 32374 caracteres]

## 21/11 10:36 · sessao 26db093179a0 · - · - · origem=Ronald (tmux)

O `git log -p` falhou porque a cópia veio sem `.git`. O [RELATO.md](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/docs/RELATO.md:283) atribui a troca a `604505878` e remete os esperados anteriores ao corpo do commit. O esperado anterior por método e a respectiva linha do diff ficam [nao verificado]. O [RELATO.md](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/docs/RELATO.md:752) registra a fixture divergente e a falha antiga de K47. A [linha 790](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/docs/RELATO.md:790) registra a mordida de lote `8 != 14` em `8938552e1`.

A tabela K47 + 18 métodos D, os 13 auxiliares e os vínculos de escrita/leitura foram encaminhados ao agente principal. Apenas `fecho_sem_lote`, `tela_sem_lote` e `lote_entrada_sem_prefetch` têm `mutar_sitio` dentro de `test_e4c_f5_voltaD_portas.py:163,199,344`. Os outros 11 seletores da D constam do mapa de alterações e dependem de outro acionamento, pois o contexto exige que o nome lhe seja passado. A verificação estática confirmou cada âncora uma vez no respectivo alvo. K47 possui seletor próprio da B, `F5_VOLTA_B_MUTANTE=k47`, ligado por `test_e4c_f5_voltaB_lotes.py:40–52,197–222` e `e4c_f5_voltaB_mutantes.py:11,23,83–115`. O resultado nominal no integrado permanece [nao verificado].

## 21/11 10:34 · sessao 2a38b67b8ab5 · - · - · origem=Ronald (tmux)

- **Censo atual** por AST de `app/**/*.py`, excluindo diretórios `tests`: 53 ocorrências, das quais 52 DECIDE e 1 EXIBE exclusivo. A única EXIBE exclusiva é `app/api/views.py:2601`, cujo objeto só alimenta os campos serializados em `:2579–2581`. A leitura do histórico `:2587` alimenta a mesma serialização e o `if` em `:2590–2591`. A chamada de passagem da ausência também tem decisão e trilha serializada. `app/chamados/services/materializacao.py:1060–1067,1098–1104`
- **Comparação histórica**: o `42/1` de `85af6ab26` conta consumidores de **`respondida_em`**, incluindo indiretos. O parágrafo publica 41 endereços no trecho de decisões, equivalentes a 40 chamadas atuais porque `models.py:3265` e `:3268` compartilham a leitura em `:3259`. Publica ainda a serialização da API em `:2581` como a âncora nominal de “por dúvida” e duas leituras diretas fora do recorte de `.composta()`. Uso no APK, classificação nominal completa dos três “por dúvida” e identidade inequívoca do EXIBE histórico [nao verificado]. `app/docs/RELATO.md:92,149,160`; `app/docs/BACKLOG.md:2054`; `app/chamados/models.py:1707–1719`; `app/chamados/services/historico.py:98–114`
- **Complemento nominal atual** frente aos 41 endereços listados: `app/api/views.py:2601`, exposição do instante; as 12 demais ocorrências só consomem `validada_em`: `app/chamados/models.py:4399`, `app/chamados/services/acoes_disputa.py:349`, `app/chamados/services/declaracao_texto.py:229`, `app/chamados/services/disputa_emissao.py:1661,2112`, `app/chamados/services/materializacao.py:200,2161`, `app/chamados/services/validacao.py:184,279,656,1082,1104`. A atribuição de entrada ou saída entre commits está [nao verificado]. O diretório é uma cópia sem repositório Git [nao verificado por arquivo:linha].
- **Opções**: publicar `52/1` por ocorrência, documentando os usos duplos, ou publicar também os consumidores do instante como quadro próprio para preservar a unidade histórica de `42/1`. O primeiro quadro responde ao censo da árvore; o segundo exige o levantamento nominal de `85af6ab26` para afirmar o delta histórico completo. `app/docs/RELATO.md:149,160`; `app/api/views.py:2579–2603`.

## 21/11 10:33 · sessao 798b6f213fa2 · - · - · origem=Ronald (tmux)

### Item 32

O vigia compara literalmente o texto vigente retornado por `DecisaoPergunta.leitura` com a projeção `p.resposta_colab`, tratando vazio com `or ''`. A leitura usa `ato_texto` da linha vigente e sintetiza do passivo a partir da projeção quando falta a linha correspondente. O compositor entrega o texto da projeção aos juízes. `app/chamados/services/vigia_texto_resposta.py:17–25`, `app/chamados/models.py:4313–4322,4361–4379,4388–4390`.

`test_diferenca_so_de_caixa_conta` grava a linha RESPOSTA com `Ato` e atualiza diretamente a coluna para `ato`, no mesmo instante, e exige `(1,1)`. Inserir `casefold()` ou `lower()` no lado do ato ou nos dois lados produziria `(0,1)`. Inserir só na coluna manteria `(1,1)`, pois a coluna já é minúscula. Os demais cenários do módulo também manteriam a expectativa para essa variante, por leitura. O teste de carimbo em `test_o261_vigia_fio.py` deriva a expectativa do próprio contador. Execução nominal [nao verificado]. `app/chamados/tests/test_e4c_f5_contador_texto.py:21–53,56–70,87–95`, `app/chamados/tests/test_o261_vigia_fio.py:842–850`.

Há `.lower()` em dois classificadores distintos, `classificar_resposta_validacao` e `classe_do_lote`. A remoção mudaria a classificação de `Confirma` que ambos os testes fixam, e a do escape `ATESTADO` no lote. A busca de nome de turno usa `casefold()` nos dois operandos. `app/chamados/juizes.py:2043–2056`, `app/chamados/services/validacao.py:1153–1154,1196–1212`, `app/chamados/models.py:4159`, `app/chamados/tests/test_a27_recusa_com_dono.py:110–114`, `app/chamados/tests/test_validar_classe_a.py:75–80`, `app/chamados/tests/test_fila_validar_lote.py:42–59`, `app/escala/views.py:227–228`.

- Terra julgou `NAO BLOQUEIA`. O fundamento integral [nao verificado]. `app/docs/RELATO.md:1374`.
- Sol 6.1 julgou `BLOQUEIA`. O fundamento integral [nao verificado]. `app/docs/RELATO.md:1374–1377`.
- Sol 6.2 julgou `BLOQUEIA` e apontou que `strip()` aplicado só ao texto lido do ato sobreviveria ao RED de espaços, que planta o espaço só na coluna. `app/docs/RELATO.md:1377`, `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–48`.

Opções de publicação:

1. **Equivalente sob igualdade demonstrada.** A comparação literal e a simétrica seriam equivalentes num domínio em que o par de textos sempre fosse igual. A porta humana serializa pelo colaborador e grava o mesmo texto nos dois lados. A automática grava os mesmos valores e a resposta pela trilha copia o texto da coluna. A guarda de `carimbar` verifica presença de validação, e a do `save` verifica a presença conjunta de instante e texto da projeção. Uma garantia geral de igualdade [nao verificado]. Igualdade por si só falha para transformação unilateral, pois `Ato` em ambos os lados produziria divergência espúria. `app/chamados/services/disputa_emissao.py:1478–1502,1532–1537,1709–1715,1728–1737`, `app/chamados/models.py:3384–3393,3731–3764,4392–4399`, `app/chamados/services/vigia_texto_resposta.py:23`, `app/docs/RELATO.md:148`.
2. **Teste faltando.** Se a transformação é normalizar só a coluna, `ato` no ato e `Ato` na coluna perderiam uma divergência. `Ato` idêntico nos dois lados ganharia uma divergência. Ambos complementam o cenário atual. Se a transformação é `strip()` só no ato, o caso correspondente põe o espaço no ato. `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:21–24,40–53`, `app/docs/RELATO.md:1377`.
3. **Outro destino.** Registrar remoção como variante sem ponto de aplicação no contador, ou registrar provável morte estática para a inserção simétrica ou só no ato. Manter o caso histórico em anexo até identificar a transformação e obter o recibo. O RELATO registra sobrevivência plausível e sem execução verificável. Também registra em separado `M_strip_velho` verde e `M_strip_novo` vermelho com o caso só-espaço. `app/chamados/services/vigia_texto_resposta.py:23`, `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/docs/RELATO.md:94,1439–1440`.

`DecisaoPergunta.registrar` persiste `ato_texto` via `bulk_create`; `leitura` o lê e `resposta_display` o expõe, formatando dropdowns e comparando o valor de opção literalmente. O comando lê o contador, publica divergência e universo na saída, escreve o censo com esperado zero no carimbo, e o ESTADO lê e exibe desvios genericamente. `app/chamados/models.py:3865–3891,4223,4361–4379,4428–4438`, `app/chamados/management/commands/vigiar_fio_morto.py:104–110,194–197,231–232,269–283`, `bin/fio_estado.py:121–126,191–194`.

`git log` e `git rev-parse` retornaram `fatal: not a git repository`. O relato aponta os commits D `497c896bd` e `76ce6695c` e as guardas migradas, mas os hunks dessa cópia e o SHA por Git estão [nao verificado]. `app/docs/RELATO.md:786`.

## 21/11 10:24 · sessao 915e6f0db115 · E4C-F5 · leitura · origem=Code

## 1. Contagem e lista de 31

O vPRONTO6 marca **35 itens** como `SO POR EXECUCAO` na tabela de 01–113 e **6** nos avais posteriores, totalizando **41**. A conta chega a **36** ao descontar os cinco que a execução 4 registra como fechados — **11, 12, 57, 59 e 73**. Os cinco continuam com a marca no vPRONTO6. A tabela abaixo lista os **41 números e nomes**, mais 32, 80 e 102. `vPRONTO6_r.txt:5–143,240`, `execucoes.txt:29`, `app/docs/RELATO.md:12–13`

**Existe uma lista de 31 no anexo.** É a seção `FALTA-EXECUCAO` do vPRONTO6 — **10, 13, 20, 22, 27, 28, 39, 43, 44, 47, 48, 50, 52, 53, 54, 60, 79, 80, 83, 84, 87, 88, 90, 91, 96, 98, 108, 109, 117, 120 e 128**. O aval usa “os 31 de execucao” sem enumerá-los. A lista do anexo inclui o **80**, que o aval já cita separadamente, o **128**, marcado `FALTA`, e o **120**, identificado como fora da F5. A identidade entre essa lista e os “31” do aval fica **[nao verificado]**. `aval.txt:7,9`, `vPRONTO6_r.txt:84,135,143,185–217`

## 2. Onde roda e o que a prova cobre

Os rótulos Django começam em `chamados`, `ponto` ou `escala`, com `app/` como raiz montada. **NAO EXISTE na cópia** indica que falta o instrumento específico para a medida pedida. Quando o RELATO nomeia um instrumento externo, deixo o nome registrado e o conteúdo **[nao verificado]**. `bin/suite.sh:316–325`, `bin/sombra.sh:13–14`, `app/docs/RELATO.md:45,70,90`

A prova disponível da bateria de **163** termina em **EM CURSO**. O resultado de `bateria.out` e a conciliação **163 → 124 por nome** estão **[nao verificado]**. A suíte de `b0935c1f1` tem `rc=0` agregado. As provas de sombra citadas abaixo são de `9e90d`, com identidade de produção declarada no anexo. A confirmação independente do hash por Git está **[nao verificado]**. `execucoes.txt:1–2,29–31`, `aval.txt:6`, `vPRONTO6_r.txt:247`

| Item | O que precisa ser medido — trecho literal do PRONTO | Onde roda | Bateria de 163 ou suíte `rc=0` basta? | [nao verificado]
|---|---|---|---|
| **10. Mutantes tira-lote** | “colher a asserção de cada mordida” dos 33 seletores, 9 A + 14 B + 10 C. `vPRONTO6_r.txt:14,187` | **M** `F5_VOLTA_A_MUTANTE`, `F5_VOLTA_B_MUTANTE` e `F5_VOLTA_C_MUTANTE`, com os 33 valores nas tabelas. **T** módulos `chamados.tests.test_e4c_f5_voltaA_lotes`, `chamados.tests.test_e4c_f5_voltaB_lotes` e `chamados.tests.test_e4c_f5_voltaC_lotes`. Os dois casos destacados são `chamados.tests.test_e4c_f5_voltaA_lotes.CarregadoresEmLoteTest.test_RED_k16_pendentes_s128` e `chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k47_escolhas_sem_credor`. `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19,58–71`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:11–27,83`, `app/chamados/tests/e4c_f5_voltaC_mutantes.py:11–31`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:112–146`, `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222` | **Não.** Falta o placar nominal dos 33, inclusive K16 e K47. `execucoes.txt:2,30`, `vPRONTO6_r.txt:14` |
| **11. Sombra dos juízes e do passivo** | “51.953 julgadas de cada lado, 82 chaves comparadas e zero blocos diferentes” e “série completa por volta”. `vPRONTO6_r.txt:15` | **S** `bin/sombra.sh --rodar e4c_f5_pronto_item11.py` em MAIN e F5. **NAO EXISTE na cópia** o script externo nomeado. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:33,35,45` | **Não.** A prova é a execução 4, que registra o recorte como fechado em `9e90d`. Série completa por volta **[nao verificado]**. `execucoes.txt:29`, `app/docs/RELATO.md:12,33` |
| **12. Sombra das decisões por porta** | “141 tentativas, 152 decisões iguais e rollback conferido em 141”, em amostra. `vPRONTO6_r.txt:16` | **S** `bin/sombra.sh --rodar e4c_f5_pronto_item12.py`. **NAO EXISTE na cópia** o script externo nomeado. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:29,35,45` | **Não.** A execução 4 fecha a amostra em `9e90d`. O RELATO limita a medida a até 20 por porta e empresa. `execucoes.txt:29`, `app/docs/RELATO.md:12,29` |
| **13. Entregues e exportadas** | “comparar por PK as decisões de validar, recusar e materializar com a guarda de competência entregue”. `vPRONTO6_r.txt:17,188` | **S** `bin/sombra.sh --rodar e4c_f5_ab_entregue.py` e `bin/sombra.sh --rodar e4c_f5_6a1_v2.py` são as sondas externas registradas. **NAO EXISTE na cópia** a sonda dedicada à lista geral das decisões alteradas em exportadas. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:66,78,90`, `vPRONTO6_r.txt:17,188` | **Não.** A execução 4 cobre A/B entregue e 6-A1 nos recortes medidos. Lista geral por PK **[nao verificado]**. `execucoes.txt:29`, `app/docs/RELATO.md:27,66,78` |
| **16. Suíte inteira pré-Opus** | “Placar nominal da execução 1, recibo da Pista 2 e precedência da publicação completa”. `vPRONTO6_r.txt:20` | **T**, suíte inteira `bin/suite.sh --dir <cópia> --motivo "pre-opus E4C-F5"`. O executor chama `manage.py test` com os rótulos da régua. `bin/suite.sh:23–33,159,316–325`, `bin/regua.sh:23` | **Sim** para a suíte inteira verde no integrado. **Não** para os três recibos adicionais. `execucoes.txt:2`, `vPRONTO6_r.txt:20` |
| **20. RED da partição do app** | “Mordidas nominais no hash final” de `a1_app_coluna` e `a1_app_texto_do_ato`. `vPRONTO6_r.txt:24,189` | **T** `chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_GUARDA_particao_do_app_instante_do_ato_texto_da_projecao`. **M** `CORTES_F2_MUTANTE=a1_app_coluna` e `a1_app_texto_do_ato`, um por rodada, definidos no próprio teste. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:45–53`, `app/chamados/tests/cortes_f2_f9_apoio.py:77–87` | **Não.** Falta o resultado com cada seletor ativo. `execucoes.txt:2,30` |
| **22. RED do fio** | “três variantes dirigidas, coluna, texto do ato e composição local”. `vPRONTO6_r.txt:26,190` | **T** `chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_GUARDA_fio_instante_do_ato_texto_da_projecao` e `chamados.tests.test_e4c_f5_voltaA_sitios.SitiosVoltaATest.test_MORDE_fio_composicao_local`. **M** `CORTES_F2_MUTANTE=a1_fio_coluna`, `a1_fio_texto_do_ato` e `a1_fio_composicao_local`, definidos no teste. `app/chamados/tests/test_e4c_f5_voltaA_sitios.py:55–71`, `app/chamados/tests/cortes_f2_f9_apoio.py:77–87` | **Não.** Faltam as três mordidas nominais. `execucoes.txt:2,30` |
| **27. Contagem preservada no vigia** | “Comparação antes × depois com a mesma fixture”, com `(0,2)`, `(1,2)`, `(1,1)` e `(1,3)`. `vPRONTO6_r.txt:31,191` | **T** `chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_divergencia_e_universo`. `app/chamados/tests/test_e4c_f5_contador_texto.py:26–38` | **Não.** O `rc=0` é do integrado e falta a execução da mesma fixture contra a base. `execucoes.txt:2`, `vPRONTO6_r.txt:31` |
| **28. RED de consultas do vigia** | “3 SELECTs de decisões para 5 perguntas em lotes de 2” e “RED nominal contra a carga anterior”. `vPRONTO6_r.txt:32,192` | **T** `chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_RED_decisoes_em_lotes_de_chunk`. `app/chamados/tests/test_e4c_f5_contador_texto.py:87–95` | **Não.** Falta o RED da carga anterior e o recibo nominal do controle final. `execucoes.txt:2`, `vPRONTO6_r.txt:32` |
| **32. Triagem de `casefold`** | “Falta decisão motivada para o Opus” e “o destino do sobrevivente”. `vPRONTO6_r.txt:36,171` | **D** decidir e publicar o destino. **T** `chamados.tests.test_e4c_f5_contador_texto.ContadorTextoTest.test_diferenca_so_de_caixa_conta`. **M** seletor de `casefold` **NAO EXISTE na cópia**. `app/chamados/tests/test_e4c_f5_contador_texto.py:50–53`, `app/chamados/services/vigia_texto_resposta.py:19–25`, `app/docs/RELATO.md:94,1374,1377` | **Não.** O verde agregado não resolve a divergência de pareceres. A mordida específica pedida pelo aval está **[nao verificado]**. `execucoes.txt:2,30`, `aval.txt:7`, `app/docs/RELATO.md:94` |
| **39. REDs 3 × 6 da A** | “11 comparativos de lote sobre 9 K e um O261 por candidata” e “Placar nominal e RED da passagem”. `vPRONTO6_r.txt:43,193` | **T** módulo `chamados.tests.test_e4c_f5_voltaA_lotes`. Rótulos por classe incluem `chamados.tests.test_e4c_f5_voltaA_lotes.TarefaEmLoteTest.test_RED_k1_tarefa`, `chamados.tests.test_e4c_f5_voltaA_lotes.CarregadoresEmLoteTest.test_RED_k16_pendentes_s128`, `chamados.tests.test_e4c_f5_voltaA_lotes.O261PorCandidataTest.test_RED_o261_selects_de_decisoes_por_candidata` e `chamados.tests.test_e4c_f5_voltaA_lotes.BecoC11EmLoteTest.test_RED_k2_c11_vivas_do_beco`. **M** seletores A do 10. `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:23–240`, `app/chamados/tests/e4c_f5_voltaA_mutantes.py:9–19,58–71` | **Não.** Faltam placar por método e RED da passagem. `execucoes.txt:2,30`, `vPRONTO6_r.txt:43` |
| **43. Tetos existentes E2 e C11** | “Diff de preservação e placar nominal”. `vPRONTO6_r.txt:47,194` | **T** `chamados.tests.test_e2_tarefa_do_dia.SeloDeterminismoERelogio.test_lote_tem_queries_constantes` e `escala.tests.test_f3_beco1_folga_na_lista.F3Beco1NaListaTest.test_MORDE_F5_queries_nao_crescem_com_os_dias`. **D** conferir o diff base → final. `app/chamados/tests/test_e2_tarefa_do_dia.py:876,913–938`, `app/escala/tests/test_f3_beco1_folga_na_lista.py:41,378–402` | **Não.** O verde agregado não fornece diff nem placar nominal. `execucoes.txt:2`, `vPRONTO6_r.txt:47` |
| **44. O261 por candidata, teto 9** | “colher o teto 9 e as contagens de SELECT para 3 e 6 candidatas”. `vPRONTO6_r.txt:48,195` | **T** `chamados.tests.test_o261_julgamento_desfaz_carimbo.JulgamentoDesfazCarimboSemLastroTest.test_custo_por_candidata_e_CONSTANTE` e `chamados.tests.test_e4c_f5_voltaA_lotes.O261PorCandidataTest.test_RED_o261_selects_de_decisoes_por_candidata`. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:90,187–207`, `app/chamados/tests/test_e4c_f5_voltaA_lotes.py:188–204` | **Não.** Falta a remedição nominal final dos dois testes. `execucoes.txt:2`, `vPRONTO6_r.txt:48` |
| **47. Quatro casos verdes do chokepoint** | “1 e 3 furos × fecha e fica” e “colher as contagens por cenário”. `vPRONTO6_r.txt:51,196` | **T** `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_ela_fecha`, `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ela_fecha`, `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_1_furo_vivo_que_fica` e `ponto.tests.test_selo_chokepoint_escrita.SeloChokepointEscritaTest.test_registrar_batida_com_3_furos_vivos_que_ficam`. `app/ponto/tests/test_selo_chokepoint_escrita.py:298,467–475,502–510` | **Não.** Falta o número da rodada final por cenário. `execucoes.txt:2`, `vPRONTO6_r.txt:51` |
| **48. Atribuição SQL e escritores novos** | “Captura final por SQL, verbo e sítio”, com zero `INSERT/UPDATE/DELETE/SAVEPOINT` a mais. `vPRONTO6_r.txt:52,197` | **T** quatro rótulos do 47, cuja captura alimenta a contagem. **NAO EXISTE na cópia** comando dedicado ao relatório comparativo completo por SQL, verbo e sítio. As anotações apontam a captura anterior. `app/ponto/tests/test_selo_chokepoint_escrita.py:196–267,438–510` | **Não.** O verde verifica os tetos. A atribuição final da diferença permanece **[nao verificado]**. `execucoes.txt:2`, `vPRONTO6_r.txt:52` |
| **50. Onze famílias verdes** | “saída nominal cruzada com a partição do 49”. `vPRONTO6_r.txt:54,198` | **T** módulos `chamados.tests.test_pergunta_viva` e `chamados.tests.test_e4c_f5_duble_conforme`. Rótulos existentes incluem `chamados.tests.test_pergunta_viva.PerguntaVivaOrdemTest.test_0_validada_morre` e `chamados.tests.test_e4c_f5_duble_conforme.DublePerguntaConformeTest.test_contagem_dos_asserts_igual_ao_main`. **D** cruzar os 11 e os 6 métodos com o inventário do 49. `app/chamados/tests/test_pergunta_viva.py:15–76`, `app/chamados/tests/test_e4c_f5_duble_conforme.py:38–87`, `aval.txt:7` | **Não.** Faltam saída nominal e vínculo por família. `execucoes.txt:2`, `vPRONTO6_r.txt:54,198` |
| **52. Medida de `respondida`** | “MAIN × F5 na property `respondida`, pelo mesmo PK, nos dois sentidos da divergência”. `vPRONTO6_r.txt:56,199` | **S** envelope `bin/sombra.sh --rodar`. Sonda específica final **NAO EXISTE na cópia**. O teste `chamados.tests.test_e4c_f5_opus_models.SitiosPeloCompositorTest.test_RED_3791_respondida_texto_da_projecao` cobre a fixture. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:92,782`, `app/chamados/models.py:3838–3844`, `app/chamados/tests/test_e4c_f5_opus_models.py:75,87–89` | **Não.** A comparação de frota registrada é anterior. `execucoes.txt:2,29`, `app/docs/RELATO.md:782` |
| **53. Medida de `sem_veredito_entre`** | “comparando os PKs devolvidos” por `DisputaSupervisao.sem_veredito_entre`. `vPRONTO6_r.txt:57,200` | **S** envelope `bin/sombra.sh --rodar`. Sonda específica final **NAO EXISTE na cópia**. A fixture tem o rótulo `chamados.tests.test_e4c_f5_opus_models.SitiosPeloCompositorTest.test_RED_2004_sem_veredito_entre_texto_da_projecao`. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:92`, `app/chamados/models.py:2002–2027`, `app/chamados/tests/test_e4c_f5_opus_models.py:75–81` | **Não.** Falta o comparativo final dos PKs. `execucoes.txt:2,29`, `vPRONTO6_r.txt:57` |
| **54. Medida de `estado`** | “comparando o rótulo de cada PK”. `vPRONTO6_r.txt:58,201` | **S** envelope `bin/sombra.sh --rodar`. Sonda específica final **NAO EXISTE na cópia**. A fixture tem o rótulo `chamados.tests.test_e4c_f5_opus_models.SitiosPeloCompositorTest.test_RED_3235_estado_texto_da_projecao`. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:92`, `app/chamados/models.py:3249–3268`, `app/chamados/tests/test_e4c_f5_opus_models.py:75,83–85` | **Não.** Falta o comparativo final por rótulo e PK. `execucoes.txt:2,29`, `vPRONTO6_r.txt:58` |
| **57. Catraca publicada por volta** | “36 fora das permitidas na base, 24 na A, 10 na B, 0 na C e 0 em `9e90d`, com 44 reconhecidas no final”. `vPRONTO6_r.txt:61` | **T** `chamados.tests.test_e4c_f5_selo_chamada.SeloChamadaTest.test_producao_respeita_catraca` para o estado atual. Para a série, o RELATO registra o scanner do selo sobre cada árvore obtida por `git archive`. Comando dedicado à série **NAO EXISTE na cópia**. `app/chamados/tests/test_e4c_f5_selo_chamada.py:126–160,188–193`, `app/docs/RELATO.md:13,37` | **Não para a série.** A execução 4 registra o item fechado. A origem do 19 histórico está **[nao verificado]**. `execucoes.txt:29`, `app/docs/RELATO.md:12–13,37` |
| **59. Limite da exceção de folha** | “diff restrito ao prefetch e ao argumento composto do juiz”. `vPRONTO6_r.txt:63` | **D** conferência do diff registrado de `app/folha` entre MAIN e F5. O RELATO identifica `59_diff_folha_9e90d.txt` e as duas âncoras. Teste dedicado ao diff **NAO EXISTE na cópia**. `app/docs/RELATO.md:12,35,58`, `app/folha/previa.py:64–67,139–146` | **Não.** A prova do fechamento é o diff da execução 4. `execucoes.txt:29`, `app/docs/RELATO.md:12` |
| **60. Prévia da competência 11** | “Q1 `montar_previa` na competência 11, com a mesma foto e relógio, por colaborador e dia”. `vPRONTO6_r.txt:64,202` | **S** envelope `bin/sombra.sh --rodar`, chamando `montar_previa`. Sonda Q1 específica **NAO EXISTE na cópia**. `bin/sombra.sh:13–14`, `app/folha/previa.py:28,64–67,135–146`, `app/docs/RELATO.md:782` | **Não.** Os 4/450/5.861 e o diff zero são da cópia anterior. `app/docs/RELATO.md:782`, `execucoes.txt:2,29` |
| **73. Medida vigente de display** | “universo 51.953, erros 0, literal 16, LIKE 0 e CRU 0”. `vPRONTO6_r.txt:77` | **S** `bin/sombra.sh --rodar`. O RELATO identifica `f5_property_e_display.py` como sonda externa e `item73_display.out` como a saída válida. A versão corrigida do script **NAO EXISTE na cópia**. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:12–13,58,749` | **Não.** A prova válida é `item73_display.out` na execução 4. A primeira rodada teve 36.212 erros de instrumento. `execucoes.txt:29`, `app/docs/RELATO.md:12` |
| **79. Selo do par inteiro verde e mutantes** | “controle verde e mordidas nominais de `limpa_sem_ato`, `preenche_e_desfaz` e `escritor_novo` em ambos os modos”, mais `v_ato_sem_coluna`. `vPRONTO6_r.txt:83,203` | **T** `chamados.tests.test_e4c_f5_selo_par_ato_coluna.SeloParAtoColunaTest.test_escritores_fecham_o_par` e `chamados.tests.test_e4c_f5_volta3_selos.SelosVolta3Test.test_RED_validacao_iniciada_pelo_ato_tambem_grava_a_coluna`. **M** `F5_VOLTA_E_MUTANTE` com os três primeiros nomes e `F5_VOLTA3_MUTANTE=v_ato_sem_coluna` para o último. `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:535–546`, `app/chamados/tests/test_e4c_f5_volta3_selos.py:41–59`, `app/chamados/tests/e4c_f5_voltaE_mutantes.py:5–25`, `app/chamados/tests/e4c_f5_volta3_mutantes.py:10,53–56` | **Não.** Os três da E têm mordida nos dois modos em `9e90d`. A mordida de `v_ato_sem_coluna` e o placar nominal integrado seguem **[nao verificado]**. `execucoes.txt:4–11,30` |
| **80. K47 e REDs da D na classe vii** | “esperados anteriores por adaptação e o resultado nominal final da D”. `vPRONTO6_r.txt:84,182,204` | **D** tabela antes → depois. **T** `chamados.tests.test_e4c_f5_voltaB_lotes.CarregadoresVoltaBTest.test_RED_k47_escolhas_sem_credor` e `chamados.tests.test_e4c_f5_voltaD_portas.PortasVoltaDTest`, com 18 métodos. **M** `F5_VOLTA_B_MUTANTE=k47` e os 14 valores de `F5_VOLTA_D_MUTANTE`. A classe D aciona diretamente três seletores — `fecho_sem_lote`, `tela_sem_lote` e `lote_entrada_sem_prefetch`, nos métodos `test_RED_fecho_3x6`, `test_RED_tela_3x6` e `test_RED_lote_entrada_3x6`. Executor local dos demais **NAO EXISTE na cópia**. `app/chamados/tests/test_e4c_f5_voltaB_lotes.py:197–222`, `app/chamados/tests/e4c_f5_voltaB_mutantes.py:11–23`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:25,84–398`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:8–66` | **Não.** Faltam a tabela e o placar nominal final. O placar anterior registra oito sobreviventes D. `app/docs/RELATO.md:62,94`, `execucoes.txt:30`, `aval.txt:4–5` |
| **82. Sombra A/B e censo de frota** | “Frota 51.953, A=0 e B=0. Entregue 25.766, A=0 e B=0, zero sem dia apurável”. `vPRONTO6_r.txt:86` | **S** `manage.py vigiar_fio_morto` imprime A/B e universo da frota. Para o entregue, `bin/sombra.sh --rodar e4c_f5_ab_entregue.py` é a chamada do script externo registrado. O script **NAO EXISTE na cópia**. `app/chamados/management/commands/vigiar_fio_morto.py:194–199,231–234`, `bin/sombra.sh:13–14`, `app/docs/RELATO.md:27,35,90` | **Não.** A execução 4 registra ambos os recortes em `9e90d`, iguais aos de `99a74`. `execucoes.txt:29`, `app/docs/RELATO.md:12,27` |
| **83. Mutante `retry_reativa`** | “Mordida nominal final”. `vPRONTO6_r.txt:87,205` | **T** `chamados.tests.test_bug_par_volta.BugParVoltaTest.test_saida_primeiro_par_inteiro_retry_idempotente`. **M** `python -m chamados.tests.bug_par_mutantes --django --mutante retry_reativa`. `app/chamados/tests/test_bug_par_volta.py:40,61–63`, `app/chamados/tests/bug_par_mutantes.py:68–71,189–209,245` | **Não.** A mordida por asserção registrada é de `c8eb8eddf`. `app/docs/RELATO.md:98`, `execucoes.txt:30` |
| **84. Texto efetivamente mostrado nos 16** | “Renderização nominal MAIN × cópia final”. `vPRONTO6_r.txt:88,206` | **S** `bin/sombra.sh --rodar f5_display_16.py` em ambas as árvores. **NAO EXISTE na cópia** o script externo registrado. `bin/sombra.sh:13–14`, `app/docs/RELATO.md:769–780`, `app/templates/chamados/partials/_pergunta_card.html:31`, `app/templates/chamados/partials/_card_atendimento.html:106,143,186,394` | **Não.** O 16/16 é da comparação anterior. A execução 4 traz o placar do 73. `app/docs/RELATO.md:12,769`, `execucoes.txt:29` |
| **87. REDs e mutantes da devolução** | “Três REDs, um controle e quatro variantes de guarda, foto e cache”. `vPRONTO6_r.txt:91,207` | **T** `chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna.DevolucaoAtoOuColunaTest.test_RED_A_devolve_ato_e_deixa_trilha`, `chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna.DevolucaoAtoOuColunaTest.test_RED_B_limpa_coluna_mesmo_sem_validacao_vigente`, `chamados.tests.test_e4c_f5_item1_devolucao_ato_ou_coluna.DevolucaoAtoOuColunaTest.test_RED_PREFETCH_enxerga_validacao_posterior

[truncado em 20000 de 33739 caracteres]

## 21/11 10:19 · sessao 1706a82be043 · E4C-F5 · leitura · origem=Code

## A. Logs citados no RELATO

Os cinco itens têm uma referência comum em `c8eb8eddf`: “tabelas nominais inteiras em logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt, montadas pelo Codex sobre c8eb8eddf, amostra da sessao 9 de 9”. A mesma linha registra as lacunas de cada tabela. Conteúdo dos logs brutos [nao verificado]. `app/docs/RELATO.md:94`

| Item | Caminho, hash e trecho literal |
|---|---|
| **01 — Matriz e censo de entrada** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:43-59`: “[01 06] 15 sitios x selo: tabela nominal em ra.txt:43-59”. **`3308f7599`**, `logs/ESPINHA-BRIEFS/F5/brief_c3.md`: “Etapa A fechou -- matriz 15 sitios (logs/ESPINHA-BRIEFS/F5/brief_c3.md, amostra 9/9 arquivo:linha conferidos no 3308f7599), perguntas de lei 0, CENSO fechado pelo wrapper (disjuntor --censo, brief canonico logs/E4C-F5-CONSTROI/brief.md)”. O despacho do censo cita `logs/ESPINHA-BRIEFS/F5/c3/etapaA2`: “F5 Etapa A relancada sobre 3308f7599 (main POUSADO, F6F7+F9 no ar): Codex censo logs/ESPINHA-BRIEFS/F5/c3/etapaA2”. `app/docs/RELATO.md:94,1503,1511` |
| **40 — REDs e mutantes da B** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:84-121`: “[35 36 40 41] mapa K: 33 K com carregador e arquivo:linha + K40 (ra.txt:84-121)”. A rodada cita `logs/ESPINHA-BRIEFS/F5/vB/alvo_B.log` e registra “RED dos 30 testes novos: base ec7134368 FAILED 28/30 (vB/base_novos.log), B com o import de DIA OK 30/30 (vB/B_novos.log)” e “Volta B COMMITADA na copia: d0210a400”. Os caminhos relativos dos mutantes e da passagem aparecem como “Em curso: 15 mutantes (vB/mut/”, “Mutantes da B: 15/15 MORTOS” e “VOLTA B VERDE (00:5x, vB3/volta.log, worktree d9319a215 contra ec7134368)”. A conciliação 29×27 segue listada como falta. `app/docs/RELATO.md:94,1296` |
| **41 — REDs e mutantes da C** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:84-121`, pelo mesmo trecho “[35 36 40 41] mapa K”. Na volta C, o RELATO registra “57582ee14: 25 K + sobra dedup” e “triagem C no Codex leitura (logs/ESPINHA-BRIEFS/F5/vC/triagem)”. Para C+D, cita “F5 VOLTA C+D VERMELHA 01:32 (logs/ESPINHA-BRIEFS/F5/vC/volta.log, copia wt-f5-vC-copia-011631 = 9d386d1b1)”. Para o placar agregado, registra “mutantes tira-lote A+B+C 33 de 33 mordem pela medida” na **`a3c23d10b`**, cuja passagem cita `logs/ESPINHA-BRIEFS/F5/vC5/volta.log`. Na **`8938552e1`**, repete “33 de 33 mutantes MORDEM” com o caminho relativo `vC5/green_c6.log`. O vínculo nominal das três M segue listado como falta. `app/docs/RELATO.md:94,790,793,1283,1289` |
| **49 — Harness nas exceções** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt:161-171`: “[49 51] 9 modulos de harness com asserts MAIN = copia (ra.txt:161-171)”. A volta A cita o recorte `logs/ESPINHA-BRIEFS/F5/vA/prova/` sobre o **MAIN `eb1e49c1e`** e registra a migração em **`cda246732`**: “helper + migracao + selos (a)(b) -> Codex construtor 20:09 (logs/ESPINHA-BRIEFS/F5/vA/duble/; base (b) medida no MAIN em base_asserts_eb1e49c1e.txt)”. A partição nominal das 11 famílias segue listada como falta. `app/docs/RELATO.md:94,1312,1314` |
| **51 — Décima segunda família** | **`c8eb8eddf`**, `logs/ESPINHA-BRIEFS/F5/vPRONTO5/cruza/ra.txt`, no bloco “[49 51]”, que nomeia “os dois RED-8 nomeados (test_red_8_custo_pergunta_viva_calc_prefetch_uma_e_cinco, test_red_8_custo_montar_chamados_fio_uma_e_cinco)” e registra “FALTA: a particao nominal das 11 familias e o ordinal da 12a.” Para os RED-8, há registros históricos em `logs/ESPINHA-BRIEFS/F5/v2/prova/RED_base.log`, **`9fa5ad533`**, com “red_8 (custo) ERROR na fixture”, e `logs/ESPINHA-BRIEFS/F5/opus_models/`, **`18bf86f01`**, com “GREEN Ran 54, 2 ERROR test_red_8_* da base”. Para o RED do fio sem prefetch, a **`d40f1c9dc`** cita o caminho relativo `quatro/red_sem_lote.log`: “3 not less than or equal to 1”. `app/docs/RELATO.md:94,1309,1320,1372` |

## B. Item 68

O **“contrato de fechamento”** é `test_porta_humana_de_disputa_presta_contas`, em `app/chamados/tests/test_contract_dedup_status.py:135–151`. O RELATO usa o rótulo “contrato de fechamento” no item 68 e nomeia o módulo ao registrar “FAIL test_contract_dedup_status (ancora de TEXTO 'self.respostas_sem_veredito()' deslocada pelo item D, classe i)”. `app/docs/RELATO.md:94,1283`

A tabela usa os blocos localizáveis na árvore final e o antes registrado no RELATO ou em comentários. O conteúdo exato da base `ebfdcc4dff23c46983c63e5a62e08cf671246a52`, os limites dos hunks e a completude do diff [nao verificado].

| Uso e bloco | Antes | Depois | Efeito | Só ato × coluna? |
|---|---|---|---|---|
| **Contrato — guarda de fechamento** | O RELATO registra a âncora antiga `self.respostas_sem_veredito()`. A expressão completa do assert antigo [nao verificado]. `app/docs/RELATO.md:1283` | O `assertIn` exige `_pend = self.respostas_sem_veredito(perguntas=perguntas)` entre as atribuições do corpo de `fechar`, via AST. `app/chamados/tests/test_contract_dedup_status.py:14–16,135–144` | O teste passa a cobrar a chamada com o lote já carregado. `fechar` carrega perguntas e decisões uma vez e entrega essas instâncias à guarda. `app/chamados/models.py:2068–2073`, `app/docs/RELATO.md:1289` | **Outro efeito.** Reuso do lote no fechamento e troca do esperado estrutural. `app/chamados/models.py:2068–2072`, `app/chamados/tests/test_contract_dedup_status.py:142–144` |
| **Expand — dublês da devolução** | A devolução passou a guardar por ato ou coluna e a fotografar a composta. A forma anterior dos dublês na base pedida [nao verificado]. `app/docs/RELATO.md:303` | `Pergunta.refresh_from_db` é um no-op. O dublê de `composta` devolve `validada_em` da própria coluna. A11 executa a devolução duas vezes e cobra `True`, depois `None`, resposta preservada, um ato e uma gravação. `app/chamados/tests/test_e4_expand.py:72–83,148–161` | Dá suporte à releitura e à foto da devolução na fixture. O dublê representa a mesma validação pelos dois lados. `app/chamados/tests/test_e4_expand.py:72–83,148–161`, `app/chamados/services/validacao.py:267–296` | **Só invariante**, no efeito identificado desse suporte à devolução. `app/chamados/services/validacao.py:267–296`, `app/chamados/models.py:4393–4399` |
| **Expand — A13, `values_list`** | “A13 por flat=” era um dos vermelhos do dublê. A assinatura anterior exata [nao verificado]. `app/docs/RELATO.md:303` | `values_list(*campos, flat=False)` devolve lista plana com `flat=True` e lista de tuplas no outro ramo. `app/chamados/tests/test_e4_expand.py:178–183` | Acomoda a consulta que monta o conjunto de donos para a trava do lote. `app/chamados/services/disputa_emissao.py:1642–1647` | **Outro efeito.** Compatibilidade do dublê com a consulta da trava. `app/chamados/tests/test_e4_expand.py:178–183`, `app/chamados/services/disputa_emissao.py:1642–1647` |
| **Expand — A13, releitura da reabertura** | “Pergunta validada so no ATO era limpa sem linha REABERTURA”. A forma anterior do dublê de `objects` [nao verificado]. `app/docs/RELATO.md:294` | `objects.filter(pk__in=...).prefetch_related(...)` devolve a pergunta se o PK estiver no lote. `app/chamados/tests/test_e4_expand.py:189–192` | Dá suporte à releitura pela composta quando as colunas estão vazias, para registrar o ato compensador quando ainda há validação pelo ato. `app/chamados/services/disputa_emissao.py:1652–1683` | **Só invariante**, no efeito identificado da releitura. `app/chamados/services/disputa_emissao.py:1652–1683`, `app/chamados/models.py:4393–4399` |
| **Expand — A14, import de `fio`** | A14 isolado dava `ImportError`. O comentário diz que o import dependia de outro módulo já ter carregado `fio`. `app/docs/RELATO.md:303`, `app/chamados/tests/test_e4_expand.py:101–104` | O harness disponibiliza `chamados.services.fio` com o corpo de `recusa_par_ilegal` lido da fonte e os dois predicados de resposta no dublê de `juizes`. `app/chamados/tests/test_e4_expand.py:92–104` | Permite executar A14 isoladamente pela guarda do par **instante de resposta × texto**. `app/chamados/tests/test_e4_expand.py:101–104,218–225`, `app/chamados/services/disputa_emissao.py:1689–1707`, `app/chamados/services/fio.py:1704–1722` | **Outro efeito.** Independência do import e exercício da guarda de resposta da L-130. `app/chamados/tests/test_e4_expand.py:97–104`, `app/chamados/services/fio.py:1704–1722` |

**B.3 — Mudança de assert.** Sim, no contrato. A âncora antiga registrada era `self.respostas_sem_veredito()`. O esperado final é a atribuição com `perguntas=perguntas`, procurada no AST do corpo de `fechar`. No Expand, os blocos identificados adaptam a instrumentação. Remoção, relaxamento ou troca de esperado no diff do Expand [nao verificado]. `app/docs/RELATO.md:1283`, `app/chamados/tests/test_contract_dedup_status.py:135–144`, `app/chamados/tests/test_e4_expand.py:148–161,178–225`

- **A 01** | **log citado**. `app/docs/RELATO.md:94,1503,1511`
- **A 40** | **log citado**. `app/docs/RELATO.md:94,1296`
- **A 41** | **log citado**. `app/docs/RELATO.md:94,790,793,1283,1289`
- **A 49** | **log citado**. `app/docs/RELATO.md:94,1312,1314`
- **A 51** | **log citado**. `app/docs/RELATO.md:94,1309,1320,1372`
- **B — Contrato de fechamento** | **outro efeito: reuso do lote de perguntas e decisões no fechamento, com troca do esperado estrutural. PAREI.** `aval.txt:7`, `app/chamados/models.py:2068–2072`, `app/chamados/tests/test_contract_dedup_status.py:142–144`
- **B — `test_e4_expand.py`** | **outro efeito: compatibilidade de `values_list(flat=True)` com a consulta da trava e execução isolada de A14 pela guarda de instante × texto. PAREI.** `aval.txt:7`, `app/chamados/tests/test_e4_expand.py:92–104,178–183`, `app/chamados/services/disputa_emissao.py:1642–1647,1689–1707`

## 21/11 10:18 · sessao 7556fe64fda6 · E4C-F5 · verificador · origem=Code

**AVAL-DEVOLVIDO — RESPOSTAS-2111-1010, item 2.** As sete referências precisam ser corrigidas antes da construção. A unidade declarada é o mutante, e o código aponta cinco comparações alteradas. `aval.txt:1`, `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:14`, `e4c_f5_voltaD_mutantes.py:23`, `e4c_f5_voltaD_mutantes.py:32`.

## 1. As premissas conferem? NAO

Nenhuma das sete linhas citadas contém a comparação mutada. Nos cinco pontos efetivos, há uma sequência permitida pelo código que produz a divergência na instância. O escritor grava a coluna e o ato no mesmo `atomic`. O leitor conserva a instância do primeiro `SELECT` e recebe as decisões do segundo, diretamente ou via prefetch. A composta usa essas decisões para calcular a validação vigente. `validacao.py:161`, `disputa_emissao.py:1478`, `disputa_emissao.py:1532`, `models.py:4361`, `models.py:4370`.

| Linha do aval | Comparação efetiva e sequência alcançável |
|---|---|
| `acoes_disputa.py:344` | **SIM em `:349`.** O `get` de `:345` carrega `perg` com a coluna nula. `validar_pergunta` confirma a validação pelo `_carimbar_validacao` antes da consulta às decisões. A composta vê a assinatura e devolve `info`. A troca pela coluna passa pela guarda. `acoes_disputa.py:345`, `acoes_disputa.py:349`, `validacao.py:746`, `validacao.py:760`. |
| `declaracao_texto.py:475` | **SIM em `:229`, pela projeção.** `:475` escolhe o caminho que carrega as perguntas com prefetch em `:476`. `_carimbar_validacao`, via `validar_pergunta`, confirma entre o `SELECT` das perguntas e o prefetch das decisões. Com a coluna ainda nula na instância, a composta produz `aceita=False` e o mutante produz `True`. `declaracao_texto.py:229`, `declaracao_texto.py:280`, `declaracao_texto.py:475`, `declaracao_texto.py:480`, `validacao.py:760`. |
| `declaracao_texto.py:634` | **SIM na mesma `:229`, pelas propostas.** `:634` consulta o memo por fato e `:636` carrega as perguntas e as decisões. O mesmo escritor confirma entre esses dois `SELECTs`, com efeito em `aceita`. `aplicar_declaracao` seleciona pelo `followup_id` e refaz a guarda sob trava, então o resultado da aplicação por si só deixa passar essa diferença. `declaracao_texto.py:634`, `declaracao_texto.py:639`, `declaracao_texto.py:651`, `declaracao_texto.py:658`, `declaracao_texto.py:670`, `validacao.py:760`. |
| `materializacao.py:195` | **SIM em `:203`.** `:195` busca a irmã `q`. No caminho da tela, `validar_pergunta(q.pk, ..., confirmar_implausivel=True)` pode confirmar a assinatura pelo `_carimbar_validacao` entre o `SELECT` de `q` e a leitura das decisões em `:200`. A composta pula o juiz da irmã. A coluna antiga, nula, faz o mutante chamar o juiz. A confirmação precisa passar pelo veto do U163, que antecede o carimbo. `materializacao.py:145`, `materializacao.py:195`, `materializacao.py:200`, `materializacao.py:203`, `validacao.py:714`, `validacao.py:720`, `validacao.py:760`. |
| `materializacao.py:925` | **SIM na mesma `:203`.** `:925` abre o ramo da testemunha da tela e `:928` chama `par_do_intervalo`. É a mesma sequência, com a confirmação da irmã entre o `SELECT` de `q` e o das decisões. O efeito verificável é passar ou cair na recusa por implausibilidade da irmã. A original ainda pode cair em outra guarda do par. `materializacao.py:925`, `materializacao.py:928`, `materializacao.py:195`, `materializacao.py:200`, `materializacao.py:203`, `materializacao.py:231`, `validacao.py:760`. |
| `validacao.py:1074` | **SIM em `:1082`.** `:1074` inicia a carga do lote. `_carimbar_validacao`, via `validar_pergunta`, confirma entre o `SELECT` das perguntas e o prefetch das decisões. A original devolve `Ja validada.` na entrada. A coluna antiga, nula, leva o mutante à reclassificação. `validacao.py:760`, `validacao.py:1074`, `validacao.py:1082`, `validacao.py:1085`. |
| `validacao.py:1102` | **SIM em `:1104`.** Uma resposta humana passa pela guarda pública enquanto a pergunta ainda está viva. O lote valida a pergunta e o `refresh_from_db()` de `:1103` lê a coluna preenchida. Antes da consulta fresca às decisões, `responder_pergunta_disputa_core` relê sob trava, zera a coluna e grava `RESPOSTA` que desfaz a validação. A original põe o item em `recusados`. O mutante conta a validação lida no refresh. `views.py:1239`, `views.py:1278`, `disputa_emissao.py:1481`, `disputa_emissao.py:1503`, `disputa_emissao.py:1535`, `validacao.py:1096`, `validacao.py:1103`, `validacao.py:1110`. |

## 2. Colide com aval vigente? NAO

Acrescentar os testes de intercalação é compatível com os controles da classe vii, que afirmam o par ato × coluna após a escrita. Os testes atuais recarregam a pergunta para afirmar o par, enquanto as sequências acima observam uma instância já carregada durante uma segunda leitura. A fonte da decisão continua sendo a composta. `RELATO.md:283`, `PROMPTS.md:2688`, `test_e4c_f5_voltaD_portas.py:45`, `test_e4c_f5_voltaD_portas.py:60`, `models.py:4382`, `models.py:4393`. [nao verificado]

## 3. Alguma condição não fecha no literal? SIM

- **A unidade é o mutante, com um teste que o mate.** As sete referências descrevem caminhos de cinco trocas. Projeção e propostas chegam à mesma guarda de `declaracao`. A busca da irmã e a chamada da tela chegam à mesma guarda de `irma_implausivel`. `aval.txt:1`, `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:13`, `declaracao_texto.py:229`, `declaracao_texto.py:480`, `declaracao_texto.py:635`, `materializacao.py:195`, `materializacao.py:203`, `materializacao.py:928`. [nao verificado]
- **Nenhuma das cinco trocas exige forjar a divergência.** Para `lote_releitura`, o caso positivo atual usa texto e força `LOTE_A`. Esse arranjo não demonstra a entrada real no lote, pois o classificador exige `hora` ou `data_hora` com hora legível. A sequência da Q1 cabe numa pergunta de hora que entre pela classe A real e cuja assinatura fique até o refresh. O teste que apaga só a coluna é um diagnóstico separado. `test_e4c_f5_voltaD_portas.py:40`, `test_e4c_f5_voltaD_portas.py:350`, `test_e4c_f5_voltaD_portas.py:376`, `validacao.py:1213`, `test_fila_validar_lote.py:103`. [nao verificado]
- **O seletor atual precisa ser acionado pelo teste ou pela bateria.** Definir `F5_VOLTA_D_MUTANTE` só aplica a troca dentro de `mutar_sitio` com o nome correspondente. Os três usos atuais nesse módulo selecionam mutantes de desempenho. A morte dos cinco no hash informado é [nao verificado]. `e4c_f5_voltaD_mutantes.py:49`, `test_e4c_f5_voltaD_portas.py:163`, `test_e4c_f5_voltaD_portas.py:199`, `test_e4c_f5_voltaD_portas.py:344`. [nao verificado]

## 4. Toca exportada, dinheiro ou `!` fora do declarado? NAO

O item autoriza testes e o seletor troca a função em memória, restaurando-a ao sair. Os cenários cabem em competência de teste aberta. O fluxo existente pode conferir a entrega e recalcular por evento, e a classificação do lote lê a tolerância do motor. O item não exige escrita em competência exportada, alteração no motor ou em escala/vínculo de produção, nem exclusão de arquivo. `aval.txt:4`, `aval.txt:9`, `e4c_f5_voltaD_mutantes.py:55`, `validacao.py:757`, `validacao.py:769`, `validacao.py:1147`, `CLAUDE.md:265`. [nao verificado]

Identidade do hash por metadados Git e ocorrência das intercalações em produção [nao verificado].

- **Q1 — PROCEDE (aval devolvido, item 2).** `acoes_disputa.py:344`, `acoes_disputa.py:349`.
- **Q2 — NAO PROCEDE.** `RELATO.md:283`, `models.py:4393`.
- **Q3 — PROCEDE (aval devolvido, item 2).** `aval.txt:1`, `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:13`.
- **Q4 — NAO PROCEDE.** `aval.txt:4`, `e4c_f5_voltaD_mutantes.py:55`.

## 21/11 06:09 · sessao f238c58b558f · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

**A rejeição tira a FALTA da cobertura lida pelo E6 e aciona o cartório no próprio `save`. A retenção do export vem do veredito da célula.** O commit libera um consumidor de chamados e perguntas. A saída fornecida mede a foto B antes desse consumidor e deixa o resultado pós-commit em produção [nao verificado]. `app/ponto/turnos.py:2523`, `app/ponto/signals.py:143`, `app/ponto/signals.py:250`, `app/folha/export.py:1273`, `sonda_ab4.py:354`, `sonda_ab4.py:378`.

Analisei a cópia em modo somente leitura. A cópia está sem metadados Git, então a correspondência com o hash informado fica [nao verificado]. As referências aos anexos contam as linhas a partir do início de cada trecho colado.

### 1. Condição exata de `sem_lavratura`

A linha do dia precisa existir, `isencao` precisa ser falsa, `ausencia` precisa ser `None` e **tanto `minutos_realizados` quanto `pago_h` precisam ser `None`**. Zero numérico passa pelo leitor. O E6 incrementa `dia_sem_lavratura_no_espelho` e pula a comparação. A sonda dá a esse ramo o rótulo `sem_lavratura`. `app/ponto/management/commands/e6_oraculo.py:263`, `app/ponto/management/commands/e6_oraculo.py:336`, `sonda_ab4.py:167`.

- **`minutos_realizados`** é uma chave do dicionário escrita pelo montador do espelho a partir de `realizado_do_dia`. O cálculo retorna `None` quando nenhum turno pareado tem `data_turno` igual à data. O pareador lê batidas não retratadas e usa a ata e a escala como insumos. `app/ponto/services/espelho.py:267`, `app/ponto/services/espelho.py:628`, `app/ponto/services/espelho.py:689`, `app/ponto/services/espelho.py:797`, `app/ponto/turnos.py:719`, `app/ponto/turnos.py:795`, `app/ponto/turnos.py:1867`, `app/ponto/turnos.py:1944`.
- **`pago_h`** é outra chave escrita pelo montador. Ela lê `DiaPago` (`ponto_diapago`), com `versao='motor'` e `tipo='dia'`. `pago_do_dia` devolve `None` se faltar a linha ou se todas as rubricas que ele testa forem zero. Se passar pelo teste, soma `horas_trabalhadas`, `horas_folga_trabalhada` e `detalhe['folga_feriado_min'] / 60`. Uma falha na consulta também deixa o mapa vazio no espelho. `app/ponto/services/espelho.py:420`, `app/ponto/services/espelho.py:825`, `app/ponto/services/dia_pago.py:833`, `app/ponto/services/dia_pago.py:854`, `app/ponto/services/dia_pago.py:861`, `app/ponto/services/dia_pago.py:890`, `app/ponto/management/commands/apurar_furo_da_exportada.py:179`.
- **Escritas em `DiaPago` na cópia.** `dia_pago.lavrar` substitui as linhas no recálculo gravável ou no backfill. `retratar_exportada` atualiza os minutos da grade e pode criar linhas com rubricas zeradas. `apurar_furo_da_exportada` só atualiza `furo_apurado` e `lavrado_em`. Há também três scripts de restauração que repõem linhas. `app/ponto/services/fechamento.py:1191`, `app/ponto/services/fechamento.py:1200`, `app/ponto/services/dia_pago.py:345`, `app/ponto/services/dia_pago.py:359`, `app/ponto/services/dia_pago.py:472`, `app/ponto/services/dia_pago.py:538`, `app/ponto/services/dia_pago.py:671`, `bin/restore_relavratura_10_2026.py:85`, `bin/restore_o263_comp10.py:54`, `bin/restore_badge_linha_09_10.py:61`.

### 2. O que a rejeição escreve e quando

`decidir` abre um `atomic`. `mudar_estado(..., _acorda=False)` grava `Ausencia.status='rejeitada'`, e o `save` dos carimbos pode disparar os receivers de novo. A cobertura do espelho vem de uma consulta a `Ausencia` para cobrança, que já exclui a rejeitada na leitura dentro da transação. No fim do ato, `_acordar_projecao` invalida `FechamentoMensal.derivado_em` nas competências afetadas. Essa atualização só escreve os carimbos do fechamento. `app/ponto/services/ausencia.py:975`, `app/ponto/services/ausencia.py:988`, `app/ponto/services/ausencia.py:1079`, `app/ponto/services/ausencia.py:1104`, `app/ponto/services/ausencia.py:1123`, `app/ponto/services/ausencia.py:195`, `app/ponto/services/fechamento.py:1652`, `app/ponto/services/espelho.py:596`, `app/ponto/services/espelho.py:859`, `app/ponto/turnos.py:2530`, `app/ponto/turnos.py:2768`.

**Cartório, síncrono.** O `post_save` de `Ausencia` chama `_julgar_dias`, que calcula as datas via `_dias_ausencia` e chama `cartorio.julgar_celula` dentro de `engolir`. `engolir` conta e registra a exceção sem propagá-la. `em_julgamento()` consulta uma flag da thread e corta a reentrada. `julgar_colab` liga essa flag e a limpa no `finally`. Para uma célula elegível, o cartório relê a cobertura, considera-a na impressão e grava `veredito`, `protestos`, `ata` e `ausencia` na célula quando há lavra. Dia futuro, célula inexistente, turno de hoje ainda em curso e impressão já em dia têm guardas próprias. `app/ponto/signals.py:85`, `app/ponto/signals.py:115`, `app/ponto/signals.py:143`, `app/core/observ.py:73`, `app/core/observ.py:86`, `app/ponto/services/cartorio.py:59`, `app/ponto/services/cartorio.py:993`, `app/ponto/services/cartorio.py:1151`, `app/ponto/services/cartorio.py:1385`, `app/ponto/services/cartorio.py:1427`, `app/ponto/services/cartorio.py:1898`, `app/ponto/services/cartorio.py:2073`, `app/ponto/portas/celula.py:1090`, `app/ponto/portas/celula.py:1194`.

**Consumidor, após o commit.** Outro `post_save` registra `transaction.on_commit(_depois)`. Ele devolve perguntas absorvidas quando cabível, reconcilia cobranças e fecha chamados de ausência decidida. Uma mudança de estado de chamado com dia identificável pode acionar o cartório de novo. O callback da ausência não chama diretamente o recálculo que grava `DiaPago`. `app/ponto/signals.py:250`, `app/ponto/signals.py:275`, `app/chamados/services/materializacao.py:1154`, `app/chamados/services/materializacao.py:1180`, `app/chamados/signals.py:189`, `app/chamados/signals.py:210`, `app/ponto/services/fechamento.py:1807`.

### 3. O que a sonda impede

- **O rollback corta o `on_commit`.** A foto B chama `decidir`, relê o status gravado e mede export e E6 antes de reverter o savepoint. O `atomic` externo também termina em rollback. O cartório síncrono pode gravar e ser lido dentro de B, antes de suas escritas serem desfeitas. `sonda_ab4.py:224`, `sonda_ab4.py:289`, `sonda_ab4.py:354`, `sonda_ab4.py:378`, `sonda_ab4.py:470`, `app/ponto/signals.py:143`, `app/ponto/signals.py:272`.
- **Os patches não acionam `em_julgamento()`.** A sonda silencia push, bloqueia as operações de socket e storage listadas e intercepta `Logger.handle`. O wrapper de `linhas_do_periodo` chama o leitor original e registra qualquer exceção antes de relançá-la. O JSON informa 19 conclusivos e zero falhas reportadas. `sonda_ab4.py:409`, `sonda_ab4.py:420`, `sonda_ab4.py:429`, `sonda_ab4.py:440`, `sonda_ab4.py:459`, `app/chamados/services/disputa_emissao.py:1777`, `app/ponto/services/cartorio.py:59`, `saida_sonda4.json:735`.

### 4. Pós-commit em produção e duração [nao verificado]

**O mesmo estado é possível após um commit real.** Com a cobertura retirada, o E6 devolve `sem_lavratura` enquanto os dois valores continuarem `None`. A rejeição não agenda uma relavratura de `DiaPago` pelo seu caminho direto. O estado pode acabar quando o pareamento passar a devolver um turno, quando uma relavratura fornecer uma rubrica que sustente `pago_h` ou quando o dia voltar a ter cobertura ou isenção. Um evento de batida pode agendar recálculo após commit. A cópia também prevê recálculo pelo DP ou por comando e guarda de escrita para 09/2026 já exportada. `app/ponto/management/commands/e6_oraculo.py:263`, `app/ponto/management/commands/e6_oraculo.py:336`, `app/ponto/services/ausencia.py:1123`, `app/ponto/services/fechamento.py:1652`, `app/ponto/registro_batida.py:176`, `app/ponto/registro_batida.py:195`, `app/ponto/services/fechamento.py:1807`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:558`, `app/config/crons.py:1148`.

**Não há prazo fixo pelo cron.** `processar_cartorio --apply` está configurado a partir das **06:27**, por empresa, e revisita células passadas. Esse caminho pode atualizar a célula que o export lê. Ele não executa a lavratura do `DiaPago`. O recálculo gravável é quem chama essa lavratura. Para os 30 dias da foto B, o estado depois dos callbacks, a retenção numa nova geração e a duração efetiva ficam [nao verificado]. `app/config/crons.py:616`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1898`, `app/ponto/services/fechamento.py:1191`, `sonda_ab4.py:354`, `sonda_ab4.py:470`.

### 5. O que o export retém

O export usa **outro predicado**. Para quem passa pelas guardas de isenção e desligamento, ele pode reter dia passado com vínculo sem célula, célula sem veredito nas condições do laço ou célula cujo `veredito` esteja em `VEREDITOS_ACUSAM`. Nesse último ramo, dispensa o dia se todos os protestos forem não cobráveis ou se o único protesto for `FURO_PARCIAL` e a ata confirmar furo só de pausa. Um motivo de espelho torna o colaborador `fora / furo_espelho`, e a montagem do TXT só inclui quem ficou `entra`. O teste de minutos pertence ao E6. `app/folha/export.py:1207`, `app/folha/export.py:1247`, `app/folha/export.py:1267`, `app/folha/export.py:1293`, `app/folha/export.py:1173`, `app/folha/export.py:1149`, `app/escala/models.py:1767`, `app/folha/export.py:968`, `app/folha/export.py:716`, `app/folha/export.py:725`, `app/ponto/management/commands/e6_oraculo.py:342`.

A sonda monta o TXT em memória com o fechamento gravado que carregou. A geração real passa pelo crivo da porta, classifica com fechamentos recalculados em leitura e cria um novo `ExportacaoDominio` com o conteúdo gerado. A foto B demonstra mudança na montagem da sonda. Uma mudança no TXT efetivamente entregue em produção fica [nao verificado]. `sonda_ab4.py:131`, `sonda_ab4.py:358`, `app/folha/views.py:130`, `app/folha/views.py:209`, `app/folha/views.py:267`, `app/folha/views.py:297`, `app/folha/models.py:174`.

**nao sei** — para esses dias em produção, o elo decisivo é a leitura pós-commit dos turnos e do `DiaPago` para o E6, e da `CelulaDia` para a retenção. A sonda leu os dois antes de executar o `on_commit`. `app/ponto/management/commands/e6_oraculo.py:263`, `app/folha/export.py:1273`, `app/ponto/signals.py:272`, `sonda_ab4.py:354`, `sonda_ab4.py:378`.

## 21/11 05:59 · sessao c2ebda6cddfe · E4C-F5-CONSTROI · leitura · origem=Code

As oito trocas comparam **a validação vigente pela leitura composta** com **a coluna `PerguntaDisputa.validada_em` da instância ORM**. A composta lê as decisões por `ato_em` e PK, incorpora a validação da coluna quando falta a linha correspondente e aplica os atos que desfazem a validação. A coluna é um `DateTimeField` anulável. Os oito sítios decidem pela presença do instante. [models.py:2659](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:2659) [models.py:4318](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4318) [models.py:4361](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4361) [models.py:4382](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4382)

O **sim** abaixo indica uma sequência de execução permitida pelo código da cópia. Uma gravação pareada pode terminar entre o SELECT da pergunta e o SELECT das decisões, deixando a coluna antiga na instância do leitor. A ocorrência dessas sequências e a existência de registros já divergentes em produção são [nao verificado]. [config/settings/saas.py:83](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/config/settings/saas.py:83) [models.py:911](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:911) [models.py:4361](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4361)

## Escritores e selos comuns aos oito sítios

| Escritor | Lado da coluna | Lado do ato |
|---|---|---|
| `_carimbar_validacao` | Preenche a coluna | Grava `VALIDACAO` para o mesmo PK, com o mesmo instante e no mesmo `atomic`. [validacao.py:161](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:161) |
| `PerguntaDisputa.carimbar` com `resposta='validar'` | Preenche a coluna vazia quando há texto | A composta pode incorporar o carimbo como validação sintética. A porta restaura a instância e recusa pelo par se a coluna ficou preenchida e a composta ficou vazia. [models.py:3367](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:3367) [models.py:3384](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:3384) [models.py:4318](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4318) |
| `responder_pergunta_disputa_core` | Limpa a coluna sob a trava do colaborador | Grava `RESPOSTA` com `resposta_mantem_validacao=False` no mesmo ato. [disputa_emissao.py:1478](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1478) [disputa_emissao.py:1503](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1503) [disputa_emissao.py:1532](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1532) |
| `devolver_validacao_recusada` e `devolver_paridade_bug_par_103` | Limpam a coluna | Gravam `DEVOLUCAO` no mesmo bloco da escrita. [validacao.py:284](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:284) [validacao.py:347](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:347) [validacao.py:365](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:365) |
| `reabrir_para_correcao`, incluindo a chamada de `recusar_pergunta`, e `ressuscitar_perguntas` | Limpam pelo `descarimbar_em_lote` | Gravam `REABERTURA` ou `RECUSA` no mesmo `atomic`. [disputa_emissao.py:1630](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1630) [disputa_emissao.py:1664](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1664) [validacao.py:938](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:938) [models.py:1646](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:1646) [models.py:1713](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:1713) |
| Nascimento em `PerguntaDisputa.abrir` e backfill E4b | O nascimento salva os campos recebidos. O backfill lê a projeção existente | Ambos registram as linhas faltantes com o instante da projeção. Resposta automática e resposta pela trilha preservam a validação vigente. [models.py:3229](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:3229) [backfill_fio_morto_e4b.py:147](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/management/commands/backfill_fio_morto_e4b.py:147) [disputa_emissao.py:1711](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1711) [disputa_emissao.py:1733](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/disputa_emissao.py:1733) |

O invariante é **validação vigente pelo ato ⇔ coluna preenchida**, definido em `par_ato_coluna`. `test_escritores_fecham_o_par` e `test_atos_que_desfazem_gravam_a_coluna` verificam o pareamento das escritas. O selo da Volta 3 verifica o sentido `VALIDACAO` física → coluna com o mesmo instante. O teste do carimbo sintético cobre uma resposta posterior que faria a composta perder a validação. Esses selos verificam as escritas. Nos caminhos travados abaixo, a trava do colaborador também protege o intervalo entre as leituras. [models.py:4393](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:4393) [models.py:2875](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/models.py:2875) [test_e4c_f5_selo_par_ato_coluna.py:536](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:536) [test_e4c_f5_selo_par_ato_coluna.py:548](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:548) [test_e4c_f5_selo_par_ato_coluna.py:715](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:715) [test_e4c_f5_volta3_selos.py:12](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_volta3_selos.py:12)

## Os oito sítios

**1. `corrigir`.** A original testa `DecisaoPergunta.composta(perg).validada_em`. A mutante testa `perg.validada_em`. **Sim.** A correção carrega `perg` antes de consultar as decisões, sem trava nessa guarda. Se `validar_pergunta` confirmar a assinatura entre as consultas, a composta já vê o ato e devolve `info`, enquanto a coluna da instância continua nula. [e4c_f5_voltaD_mutantes.py:14](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:14) [acoes_disputa.py:344](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/acoes_disputa.py:344) [validacao.py:746](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:746) [validacao.py:760](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:760)

**Teste e fixture mínima.** `RED_corrigir` usa o auxiliar que carimba os dois lados ou limpa os dois por uma nova resposta. Para morder, usar uma pergunta de hora respondida e ainda sem validação, pausar a correção depois do `get`, validar o mesmo PK em outra transação e retomar. Exigir `info` e preservação da resposta e da assinatura. [test_e4c_f5_voltaD_portas.py:60](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:60) [test_e4c_f5_voltaD_portas.py:84](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:84) [acoes_disputa.py:349](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/acoes_disputa.py:349)

**2. `declaracao`.** `_resposta_validada` testa `DecisaoPergunta.composta(perg).validada_em is not None`. A mutante testa `perg.validada_em is not None`. **Sim, na projeção e nas propostas.** Esses chamadores chegam ao mesmo sítio via `_porta_recusaria_por_carimbo`. Uma validação confirmada entre o SELECT da pergunta e o prefetch das decisões faz a original devolver `aceita=False` e a mutante, `True`. As guardas das duas portas de escrita leem a pergunta depois de adquirir a trava do colaborador. [e4c_f5_voltaD_mutantes.py:16](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:16) [declaracao_texto.py:217](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:217) [declaracao_texto.py:280](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:280) [declaracao_texto.py:350](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:350) [declaracao_texto.py:475](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:475) [declaracao_texto.py:634](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:634) [declaracao_texto.py:655](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:655)

**Teste e fixture mínima.** `RED_declaracao` e `RED_aplicar_declaracao` montam pares alinhados. Usar um marco faltante, uma proposta e uma pergunta respondida. Confirmar a validação entre as duas consultas da proposta e verificar `aceita=False`. É preciso acrescentar essa asserção ou testar a projeção diretamente. `aplicar_declaracao` seleciona a proposta pelo `followup_id`, sem usar `aceita`, e refaz a guarda sob trava. [test_e4c_f5_voltaD_portas.py:103](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:103) [test_e4c_f5_voltaD_portas.py:122](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:122) [declaracao_texto.py:634](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:634) [declaracao_texto.py:651](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:651) [declaracao_texto.py:664](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/declaracao_texto.py:664)

**3. `irma_plantio`.** Com `irma_validada=True`, a original rejeita a irmã se `leitura_da_irma.validada_em is None`. A mutante usa `q.validada_em is None`. **Não pelos escritores censados, partindo de um par íntegro.** `_gravar_irma_do_par` chega a essa guarda com o colaborador travado. A busca carrega uma instância fresca da irmã dentro do ato. Vale também para o plantio pelo reparador do passivo. A fixture de `RED_irma_plantio` testa irmã com ambos os lados preenchidos ou ambos nulos. [e4c_f5_voltaD_mutantes.py:20](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:20) [materializacao.py:139](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:139) [materializacao.py:195](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:195) [materializacao.py:1766](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:1766) [materializacao.py:2153](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2153) [validacao.py:746](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:746) [passivo_validado.py:377](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/passivo_validado.py:377) [test_e4c_f5_voltaD_portas.py:213](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:213) [test_e4c_f5_voltaD_portas.py:228](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:228)

**4. `irma_implausivel`.** A original consulta o juiz de implausibilidade quando `leitura_da_irma.validada_em is None`. A mutante usa `q.validada_em is None`. **Sim, no caminho da tela.** A tela chama `par_do_intervalo` sem a trava do plantio. O admin pode confirmar a hora implausível da irmã depois do SELECT de `q`, antes do SELECT das decisões. A composta vê a assinatura e pula o juiz. A mutante vê a coluna antiga nula e chama o juiz. [e4c_f5_voltaD_mutantes.py:23](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:23) [materializacao.py:145](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:145) [materializacao.py:200](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:200) [materializacao.py:925](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:925) [validacao.py:720](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:720) [validacao.py:760](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:760)

**Teste e fixture mínima.** `RED_irma_implausivel` usa os mesmos pares alinhados de `RED_irma_plantio`, com `exigir_validada=False` e o juiz de implausibilidade substituído. Manter esse juiz e os demais controles da fixture. Começar com a irmã respondida e sem validação, gravar a assinatura pelo escritor real em outra instância depois da busca de `q` e antes da composta, e exigir que o par passe sem chamar o juiz da irmã. [test_e4c_f5_voltaD_portas.py:213](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:213) [test_e4c_f5_voltaD_portas.py:238](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:238) [validacao.py:161](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:161)

**5. `materializar`.** A original testa `DecisaoPergunta.composta(p).validada_em is None`. A mutante testa `p.validada_em is None`. **Não pelos escritores censados, partindo de um par íntegro.** A primeira seleção exige coluna preenchida. O sítio mutado vem depois da trava e de um novo `get` por PK. `RED_materializar_ato_mudou_sob_trava` grava uma nova resposta em outra instância e, num subcaso, volta a validar. O `get` posterior faz o sítio receber `None` nos dois lados ou `AGORA` nos dois lados. [e4c_f5_voltaD_mutantes.py:25](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:25) [materializacao.py:2085](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2085) [materializacao.py:2142](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2142) [materializacao.py:2153](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/materializacao.py:2153) [test_e4c_f5_voltaD_portas.py:248](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:248) [test_e4c_f5_voltaD_portas.py:256](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:256)

**6. `validar_par`.** A original decide se precisa carimbar a irmã com `DecisaoPergunta.composta(q).validada_em is None`. A mutante usa `q.validada_em is None`. **Não pelos escritores censados, partindo de um par íntegro.** `validar_pergunta` já mantém a trava quando `_validar_o_par` busca a irmã e decide se chama `_carimbar_validacao`. `RED_validar_par` valida as duas metades e, no subcaso negativo, responde de novo à irmã, limpando ambas as faces. A própria fixture trava antes da chamada. [e4c_f5_voltaD_mutantes.py:27](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:27) [validacao.py:648](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:648) [validacao.py:655](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:655) [validacao.py:746](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:746) [validacao.py:786](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:786) [test_e4c_f5_voltaD_portas.py:272](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:272) [test_e4c_f5_voltaD_portas.py:284](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:284)

**7. `lote_entrada`.** A original testa `DecisaoPergunta.composta(perg).validada_em` para devolver `Ja validada.`. A mutante testa `perg.validada_em`. **Sim.** O lote carrega a coluna e depois faz o prefetch. Uma validação confirmada entre as consultas deixa a coluna nula na instância e a assinatura presente no prefetch. [e4c_f5_voltaD_mutantes.py:32](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:32) [validacao.py:1074](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1074) [validacao.py:1082](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1082) [validacao.py:161](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:161)

**Teste e fixture mínima.** `RED_lote_entrada` monta os dois estados alinhados e força classe A. Usar uma única pergunta respondida e sem assinatura. Confirmar a validação por outra transação entre o SELECT principal e o prefetch. Exigir `Ja validada.` na recusa de entrada e `validadas=0`. Com a classe A forçada, a mutante chega à ação individual, que relê `ja_validada`, e o lote acaba contando a assinatura. [test_e4c_f5_voltaD_portas.py:324](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/test_e4c_f5_voltaD_portas.py:324) [validacao.py:693](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:693) [validacao.py:1082](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1082) [validacao.py:1094](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1094)

**8. `lote_releitura`.** Depois da ação e do `refresh_from_db()` integral, a original testa `DecisaoPergunta.composta(perg).validada_em` para contar a validação. A mutante testa `perg.validada_em`. **Sim.** O refresh descarta o prefetch anterior. Uma nova resposta humana, cujo pedido passou pela guarda pública enquanto a pergunta ainda estava viva, pode terminar depois do refresh e antes da consulta às decisões. O core relê sob trava, limpa a coluna e grava a `RESPOSTA` compensadora. A original vê a validação desfeita e a mutante conserva o instante lido no refresh. [e4c_f5_voltaD_mutantes.py:35](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/tests/e4c_f5_voltaD_mutantes.py:35) [validacao.py:1096](/home/ronald/codex_ws/b0935c1f106d232dc39b263676a588d66eddff54/app/chamados/services/validacao.py:1096) [views.py:1239](/home/ronald/codex_ws/b0935c1f106d232dc

[truncado em 20000 de 23913 caracteres]

## 21/11 05:57 · sessao baf7883bee21 · - · - · origem=Ronald (tmux)

- **`corrigir` — sim, por intercalamento.** O original lê a validação vigente pela sequência. O mutante lê a coluna da instância carregada. O `GET` e a consulta às decisões acontecem separadamente, sem trava no leitor. Uma validação entre as duas consultas faz o original devolver `info` e o mutante passar pela guarda. Uma nova resposta após uma validação física permite o sentido inverso. A fixture atual valida antes de chamar a correção e exercita somente pares alinhados. Fixture mínima: carregar pergunta de hora respondida e ainda sem validação, intercalar outra transação que valide a pergunta, retomar a correção e exigir `info`, resposta e assinatura preservadas. [app/chamados/services/acoes_disputa.py:344] [app/chamados/models.py:2659] [app/chamados/models.py:4361] [app/chamados/models.py:4382] [app/chamados/services/validacao.py:161] [app/chamados/services/validacao.py:746] [app/chamados/services/disputa_emissao.py:1485] [app/chamados/services/disputa_emissao.py:1503] [app/chamados/services/disputa_emissao.py:1532] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:60] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:84]

- **`declaracao` — sim no leitor compartilhado de tela/propostas.** `_resposta_validada` testa a presença da validação composta, o mutante testa presença do campo. A projeção e as propostas carregam coluna e `decisoes` por consultas separadas sem trava. A validação canônica entre as duas pode alterar `aceita`. As duas guardas de escrita dos REDs consultam o alvo sob trava do colaborador. O `aplicar_declaracao` usa propostas antes da trava, mas filtra apenas por `followup_id`, então a divergência de `aceita` não muda os asserts atuais. A fixture comum entra com par alinhado. Fixture mínima: um marco faltante e uma pergunta respondida ainda sem validação quando sua coluna é carregada, intercalar o escritor de validação antes do prefetch das decisões e verificar `aceita=False` em `projecao_por_marco` ou `propostas_do_chamado`. [app/chamados/services/declaracao_texto.py:217] [app/chamados/services/declaracao_texto.py:280] [app/chamados/services/declaracao_texto.py:290] [app/chamados/services/declaracao_texto.py:350] [app/chamados/services/declaracao_texto.py:380] [app/chamados/services/declaracao_texto.py:471] [app/chamados/services/declaracao_texto.py:634] [app/chamados/services/declaracao_texto.py:650] [app/chamados/services/declaracao_texto.py:670] [app/chamados/services/validacao.py:161] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:60] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:103] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:122]

- **`irma_plantio` — não pelos escritores ordinários no trecho efetivo.** A guarda original testa a presença da validação composta da irmã `q`, o mutante sua coluna. A única passagem `irma_validada=True` vem de `_gravar_irma_do_par`. A irmã é buscada novamente dentro do ato sob trava do colaborador, nas entradas da validação, do materializador e do passivo. Os escritores pareados passam pela mesma trava de colaborador. A fixture constrói duas metades complementares e fixa as outras guardas, mas alterna entre `(VALIDACAO_EM, VALIDACAO_EM)` e `(None, None)`. Uma divergência persistida no banco de produção é [nao verificado]. [app/chamados/services/materializacao.py:139] [app/chamados/services/materializacao.py:195] [app/chamados/services/materializacao.py:1435] [app/chamados/services/materializacao.py:1758] [app/chamados/services/materializacao.py:2153] [app/chamados/services/validacao.py:746] [app/chamados/services/validacao.py:786] [app/chamados/services/passivo_validado.py:377] [app/chamados/services/passivo_validado.py:528] [app/chamados/models.py:2874] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:213] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:228]

- **Escritores e selos.** `_carimbar_validacao` escreve coluna e `VALIDACAO` com o mesmo instante no mesmo `atomic`. `carimbar` pode preencher a coluna e a leitura sintetiza `VALIDACAO` quando falta a linha correspondente, com recusa de `(False, True)` antes de salvar. Devoluções, resposta humana, reabertura/recusa e ressuscitação limpam a coluna e escrevem o ato compensador. `responder_automaticamente` e `carimbar_resposta_pela_trilha` escrevem RESPOSTA marcada para manter a validação e não alteram a coluna. O selo AST compara escrituras e atos nos dois sentidos, e há testes específicos para a recusa do carimbo sintético e os horários sob trava na reabertura/resposta. [app/chamados/services/validacao.py:161] [app/chamados/models.py:3367] [app/chamados/models.py:3384] [app/chamados/models.py:4318] [app/chamados/services/validacao.py:277] [app/chamados/services/validacao.py:347] [app/chamados/services/disputa_emissao.py:1503] [app/chamados/services/disputa_emissao.py:1532] [app/chamados/services/disputa_emissao.py:1642] [app/chamados/services/disputa_emissao.py:1664] [app/chamados/models.py:1702] [app/chamados/services/disputa_emissao.py:1708] [app/chamados/services/disputa_emissao.py:1728] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:536] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:548] [app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:715] [app/chamados/tests/test_e4c_f5_a1_leitura_sob_trava.py:38] [nao verificado]

A aplicação das trocas da bateria externa é [nao verificado]. O helper local só aplica a troca quando o teste entra no seu contexto com o nome do mutante. As chamadas da classe estão nos três testes de consultas. [app/chamados/tests/e4c_f5_voltaD_mutantes.py:49] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:163] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:199] [app/chamados/tests/test_e4c_f5_voltaD_portas.py:344] [nao verificado]

## 21/11 05:56 · sessao 60b01236ac3f · - · - · origem=Ronald (tmux)

**`irma_implausivel`: sim, por uma corrida de leitura.** A busca da irmã carrega `q` sem prefetch de decisões. A tela chega a `par_do_intervalo` sem a trava do plantio e o ato é consultado depois da leitura da linha ORM. Uma irmã respondida e ainda não validada pode ser carregada com `q.validada_em = None`, e então o admin confirma sua hora implausível e grava a validação antes da consulta às decisões. O registro físico e a coluna são gravados juntos pelo escritor, mas o leitor retém a coluna da instância anterior. A original vê a validação e pula o juízo. A mutante vê `None` e consulta o implausível. A ocorrência efetiva em produção é [nao verificado]. `app/chamados/services/materializacao.py:139-150,195-208,925-928`, `app/chamados/templatetags/chamados_tags.py:131-136`, `app/templates/chamados/partials/_pergunta_card.html:42-43,75-81`, `app/chamados/services/validacao.py:161-175,720-723,746-760`, `app/chamados/models.py:4361-4379,4382-4390,2659`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:23-24`.

A fixture mapeada alterna validação pareada e resposta humana que limpa ambas as faces. Para fazê-la morder, conservar o mock de implausibilidade, começar com a irmã respondida e não validada, interpor `_carimbar_validacao` em outra instância depois que a busca real retorna a irmã e antes da consulta à sequência, e esperar que o par seja aceito e que o juiz de implausibilidade da irmã não seja chamado. `app/chamados/tests/test_e4c_f5_voltaD_portas.py:35-67,213-226,238-246`, `app/chamados/services/validacao.py:161-175`.

**`materializar`: não pelos escritores atuais censados, partindo de par coerente.** O queryset inicial filtra a coluna não nula. O sítio alterado vem depois de adquirir a trava e reler a pergunta por PK. A versão original lê a validação vigente composta da instância fresca, a mutante lê a coluna da mesma instância. A fixture intercala resposta de outra instância e opcional nova validação, mas o `GET` posterior faz ambas lerem `None` ou `AGORA`. Uma linha de passivo divergente real que atravesse o filtro é [nao verificado]. `app/chamados/services/materializacao.py:2085-2103,2142-2164`, `app/chamados/models.py:2659,2875-2878,2901-2903,4361-4390`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:25-26`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:56-58,248-270`.

**`validar_par`: não pelos escritores atuais censados, partindo de par coerente.** A original lê a validação composta da irmã selecionada por uma consulta fresca e a mutante lê a coluna dessa irmã. Ambas controlam a decisão de chamar `_carimbar_validacao` dentro do savepoint que planta o par. A entrada real de `_validar_o_par` já tem a trava do colaborador. A fixture cria duas respostas, carimba ambas e só no subcaso negativo responde de novo à irmã, apagando a validação nos dois lados. A própria fixture trava antes de chamar. Passivo divergente real naquela irmã é [nao verificado]. `app/chamados/services/validacao.py:648-660,746-760,786-788`, `app/chamados/services/materializacao.py:139-150,195-200`, `app/chamados/models.py:2659,2875-2878,4361-4390`, `app/chamados/tests/e4c_f5_voltaD_mutantes.py:27-28`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:272-298`.

**Escritores comuns.** `_carimbar_validacao` grava a coluna e `VALIDACAO` da mesma pergunta, com a mesma data e no mesmo `atomic`. `PerguntaDisputa.carimbar` pode preencher a coluna para validação virtual e recusa se a composta ficar sem validação. A resposta humana baixa a coluna e grava `RESPOSTA` com `resposta_mantem_validacao=False`. Devolução ordinária, BUG-PAR, reabertura/recusa e ressuscitação baixam a coluna e emitem o respectivo compensador. Respostas automáticas e promovidas da trilha declaram preservação. O selo AST verifica o pareamento e a validação sintética. Seu escopo exclui migrações e testes. Um teste vizinho simula passivo B registrando `DEVOLUCAO` sozinho. `app/chamados/services/validacao.py:161-175,284-296,347-374`, `app/chamados/models.py:1701-1722,3367-3374,3384-3400,4313-4322,4330-4345`, `app/chamados/services/disputa_emissao.py:1499-1537,1630-1648,1664-1683,1714-1715,1735-1737`, `app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:428-491,524-556,715-732`, `app/chamados/tests/test_e4c_f5_guarda_plantio_s144.py:56-61`.

A ativação do mutante pelo mecanismo local exige entrar em `mutar_sitio`. O módulo dos testes mapeados chama esse contexto explicitamente só para desempenho. Uma ativação externa para os oito sítios é [nao verificado]. `app/chamados/tests/e4c_f5_voltaD_mutantes.py:49-66`, `app/chamados/tests/test_e4c_f5_voltaD_portas.py:163,199,344`.

## 21/11 05:55 · sessao 40ff2e051b8f · - · - · origem=Ronald (tmux)

`lote_entrada` e `lote_releitura` podem observar divergência durante uma escrita concorrente, inclusive quando o escritor mantém a paridade persistida. Os testes mapeados não montam essa leitura intercalada. A configuração usa PostgreSQL, os leitores não abrangem as duas consultas com uma trava e o código descreve `READ COMMITTED`. Ocorrência no ambiente real [nao verificado]. (app/config/settings/saas.py:83–92, app/chamados/models.py:904–912, app/chamados/services/validacao.py:1074–1084, 1094–1104)

Nos dois sítios, a original usa o instante da validação vigente no histórico `DecisaoPergunta`, ordenado por instante e PK, com validação sintética da projeção para o passivo sem linha e com os atos que desfazem a validação. A mutante lê a coluna `DateTimeField` anulável da instância. Ambas decidem por presença do instante. (app/chamados/models.py:2659, 4313–4322, 4330–4379, 4382–4390, app/chamados/tests/e4c_f5_voltaD_mutantes.py:32–37)

- **`lote_entrada`** lê primeiro as perguntas com as colunas, depois as decisões pelo prefetch. A validação real pode gravar a coluna e o ato com o mesmo instante e fazer commit entre as consultas. A instância do lote conserva a coluna nula, mas a composta lê o ato novo. A fixture mapeada parte de validação pareada ou de nova resposta que limpa as duas leituras. Fixture mínima com uma pergunta respondida e ainda sem validação, barreira após o SELECT principal, validação real em outra conexão antes do prefetch. Com a classe A forçada como no teste mapeado, o original recusa com `Ja validada.` e conta zero. O mutante chama a ação que relê `ja_validada` e depois conta uma validação em `nao_gravados`. (app/chamados/services/validacao.py:161–175, 693–704, 746–760, 1074–1109, app/chamados/services/acoes_disputa.py:18–29, app/chamados/tests/test_e4c_f5_voltaD_portas.py:35–67, 324–336)
- **`lote_releitura`** executa a ação em outra instância e recarrega integralmente a pergunta, descartando o cache das decisões. A coluna e as novas decisões são consultadas em etapas distintas. Uma resposta cujo pedido passou pela guarda pública enquanto a pergunta estava viva pode terminar depois que o lote vê a coluna preenchida. O core relê sob trava, limpa a coluna e grava a resposta que desfaz a validação. A composta consulta a nova sequência e devolve nulo, e a mutante fica com o instante carregado no refresh. A fixture existente usa um texto que conserva a validação e uma hora devolvida pela porta, com par concordante em ambos. Fixture mínima com a mesma pergunta respondida do ramo positivo, resposta humana iniciada antes da validação e liberada com instante posterior depois do SELECT do refresh e antes do SELECT de decisões. Original conta zero e põe em `recusados`. Mutante conta uma e, com o ramo de texto do teste, põe em `nao_gravados`. (app/chamados/views.py:1239–1246, 1274–1278, app/chamados/services/disputa_emissao.py:1478–1517, 1532–1537, app/chamados/services/validacao.py:1094–1114, app/chamados/tests/test_e4c_f5_voltaD_portas.py:350–386)

A coluna ganha preenchimento físico em `_carimbar_validacao` ou sintético em `carimbar`, com guarda para `(False, True)`. A sequência ganha a validação em `_carimbar_validacao` ou preserva o passivo ao materializar a linha faltante. A nova resposta humana, as duas devoluções, a reabertura/recusa e a ressuscitação limpam a coluna e registram o compensador. A paridade de presença consta em `par_ato_coluna` e é guardada pelos selos de escritores, de atos compensadores, de validação no sentido ato → coluna e de classificação da resposta. Esses selos verificam escritas, enquanto os dois sítios do lote consultam snapshots que podem ser distintos. (app/chamados/models.py:1646, 1702–1722, 3367–3400, 4313–4322, 4392–4399, 4428–4448, app/chamados/services/validacao.py:161–175, 284–296, 347–374, 938–944, 1074–1084, 1094–1104, app/chamados/services/disputa_emissao.py:1512–1514, 1532–1537, 1630–1683, app/chamados/tests/test_e4c_f5_selo_par_ato_coluna.py:428–491, 524–556, app/chamados/tests/test_e4c_f5_volta3_selos.py:12–29, 41–59, app/chamados/tests/test_f2_resposta_mantem_validacao.py:18–25, 74–79)

## 21/11 05:40 · sessao 23ac1ea19728 · FALTA-APROVADA-MOVE-O-TXT-ENTREGUE · leitura · origem=Code

A sonda abaixo seleciona os 19 casos a partir do JSON caso a caso da volta 3. O critério é `CONCLUSIVO`, `mudou_txt=True` e transição `entra ↔ fora/furo_espelho`. A cópia confirma 17 saídas em 16 PKs distintos e duas entradas, nos PKs 161 e 504. As 19 chaves completas estão [nao verificado]. Com o anexo resumido, a seleção falha antes de abrir a transação. `app/docs/RELATO.md:7`, `app/folha/export.py:837–842,968–981`.

A captura observa a própria execução de `classificar_export`. Guarda em memória `motivos_espelho`, os dias devolvidos pelo retentor, o PK e o veredito da célula, seus protestos e a ata quando o leitor da pausa a consulta. `sem_celula` pode sair com PK nulo ou com uma célula sem veredito. Os protestos participam das exceções do crivo como conjunto. A assinatura ainda recebe `inconsistencias`, mas o corpo atual ignora esse argumento. `app/folha/export.py:968–973,1148–1155,1173,1212–1213,1247–1298,1301–1307`, `app/ponto/juiz_batida.py:149–161`.

| Insumo do juiz | Caminho reutilizado |
|---|---|
| Colaborador, janela, `hoje` e `agora` | O registro do colaborador no fechamento, `janelas.janela_fechamento` e a data local do instante do ato. `app/ponto/management/commands/e6_oraculo.py:285–316`, `app/ponto/janelas.py:54–57`. |
| `horas_do_dia` | `InsumosDoDono` lê `batidas_apuraveis`, monta `minutos_do_oraculo` e passa `horas` do dia. `app/ponto/management/commands/e6_oraculo.py:128–154,182–195`, `app/ponto/turnos.py:2891–2899`. |
| `em_cadastro_x_realidade` | `espelho_do_colab` fornece `resultado`. `InsumosDoDono.cxr()` lê `resultado.dias_cadastro_x_realidade`. `app/ponto/management/commands/e6_oraculo.py:162–172,195,316–328`, `app/ponto/services/espelho.py:1241–1245,1310–1312`, `app/ponto/motor_calculo_v2.py:2719`. |
| `folga_nao_definida`, `sem_vinculo_no_dia`, `sem_vinculo_em_dia_de_trabalho` | Os três vêm de `cadastro_do_dia_carregado`. `app/ponto/management/commands/e6_oraculo.py:182–188`, `app/ponto/supra_juiz.py:179–214`. |
| `batida_fora_de_turno` | `InsumosDoDono.turnos()` carrega a janela com bordas e `batida_fora_de_turno()` consulta o realizado e a batida solta apurável. `app/ponto/management/commands/e6_oraculo.py:156–160,185,198–212`. |
| `fase_deslocada` | `InsumosDoDono.fase()` lê `dias_de_fase_as_of` até `hoje`. `app/ponto/management/commands/e6_oraculo.py:174–187`, `app/escala/services/fase_12x36.py:117–148`. |

Todos os argumentos do juiz estão disponíveis pelo carregador compartilhado. O índice diário `dias_esp` e a sequência de cortes que decide **se o E6 registra uma divergência** são locais de `Command.handle`, pois pertencem ao laço de comparação do comando. A sonda reproduz esses cortes para o placar e chama `InsumosDoDono.dono()` em todos os dias da união A/B, nas duas fotos. `retornos_do_juiz` conta todas as chamadas. `divergencias_e6_por_dono` conta as que passam pelos cortes do comando. `app/ponto/management/commands/e6_oraculo.py:182–195,215–269,320–320,328–381`.

### `sonda_ab4.py`

```python
import io
import json
import logging
import socket
import sys
from collections import Counter
from contextlib import ExitStack, closing, redirect_stderr, redirect_stdout
from copy import deepcopy
from unittest.mock import patch

from django.conf import settings
from django.contrib.auth import get_user_model
from django.core.files.storage import FileSystemStorage
from django.db import connection, transaction
from django.utils import timezone

from chamados import fcm_utils, push_utils [nao verificado]
from chamados.services.disputa_emissao import push_disputa_silenciado [nao verificado]
from chamados.services.materializacao import ESCAPES_QUE_VIRAM_AUSENCIA
from colaboradores.mixins import tem_acao
from folha import export as E
from ponto import janelas
from ponto.juiz_batida import periodos_do_dia
from ponto.management.commands import e6_oraculo as E6
from ponto.models import Ausencia, FechamentoMensal
from ponto.services import dia_pago
from ponto.services.ausencia import decidir
from ponto.services.espelho import espelho_do_colab


ENTRA, FURO = ("entra", None), ("fora", "furo_espelho")
CAMINHOS = {(ENTRA, FURO): "sai", (FURO, ENTRA): "entra"}
TIPO, ESTADO = ESCAPES_QUE_VIRAM_AUSENCIA["FALTA_DIA"]
MEDICOES = {}


class SondaFalhou(RuntimeError):
    pass


def exigir(condicao, erro):
    if not condicao:
        raise SondaFalhou(erro)


def caminho(a, b):
    return CAMINHOS.get(((a["status"], a["motivo"]), (b["status"], b["motivo"])))


def selecionar(origem):
    exigir(
        origem["universo"]["percorrido_igual_ao_listado"] is True
        and origem["placar"]["status"] == "CONCLUSIVO",
        "selecao_inconclusiva",
    )
    casos = {}
    por_caminho = {"sai": [], "entra": []}
    for c in origem["casos"]:
        if c["status"] != "CONCLUSIVO" or c["mudou_txt"] is not True:
            continue
        grupo = caminho(c["A"]["classificador"], c["B"]["classificador"])
        if grupo is None:
            continue
        chave = tuple(c[k] for k in ("empresa_pk", "mes", "ano", "colaborador_pk"))
        exigir(all(type(n) is int and n > 0 for n in chave), "chave_invalida")
        exigir(chave not in casos, "caso_repetido")
        faltas = tuple(sorted(c["ausencia_pks"]))
        exigir(
            faltas and len(set(faltas)) == len(faltas)
            and all(type(p) is int and p > 0 for p in faltas),
            "faltas_invalidas",
        )
        casos[chave] = grupo, faltas
        por_caminho[grupo].append(chave[-1])
    exigir(
        len(por_caminho["sai"]) == 17
        and len(set(por_caminho["sai"])) == 16
        and sorted(por_caminho["entra"]) == [161, 504],
        "lote_diferente_de_19",
    )
    return casos


def classificar_com_prova(empresa, mes, ano, fech):
    # app/folha/export.py:1212-1213,1247,1259-1262,1298
    laco = E._dias_sem_decisao.__code__
    pausa = E._furo_so_da_pausa.__code__
    periodos = periodos_do_dia.__code__
    por_motivo, atas_lidas = {}, {}

    def observar(frame, evento, retorno):
        if evento != "return" or retorno is None:
            return
        codigo = frame.f_code
        if codigo is periodos and frame.f_back.f_code is pausa:
            cel = frame.f_locals["celula"]
            atas_lidas[cel.data] = {
                "celula_pk": cel.pk,
                "ata": deepcopy(cel.ata),
                "periodos": deepcopy(retorno),
            }
        elif codigo is laco:
            _, dias = retorno
            celulas = frame.f_locals.get("cels", {})
            for dia in dias:
                data = dia["data"]
                cel = celulas.get(data)
                por_motivo.setdefault(dia["motivo"], []).append({
                    **dia,
                    "protestos": deepcopy(cel.protestos) if cel is not None else None,
                    "leitura_ata": atas_lidas.get(data),
                })

    exigir(sys.getprofile() is None, "perfilador_ja_ativo")
    sys.setprofile(observar)
    try:
        item, = E.classificar_export(empresa, mes, ano, fechamentos=[fech])
    finally:
        sys.setprofile(None)

    motivos = deepcopy(item.get("motivos_espelho"))
    exigir(
        item["colaborador_id"] == fech.colaborador_id
        and (motivos is not None) == (item["motivo"] == "furo_espelho")
        and (motivos or []) == sorted(por_motivo),
        "rastro_do_espelho_incompleto",
    )
    return item, motivos, por_motivo, atas_lidas


def fotografar(empresa, mes, ano, colab_pk):
    fech = FechamentoMensal.objects.select_related("colaborador__empresa").get(
        colaborador_id=colab_pk, colaborador__empresa=empresa, mes=mes, ano=ano,
    )
    item, motivos, dias, atas = classificar_com_prova(empresa, mes, ano, fech)
    linhas, _ = E.montar_linhas_export(
        empresa, mes, ano, itens=[item], fechamentos=[fech],
    )

    matricula = item["codigo_dominio"].zfill(10)
    pai = None
    tipos = {"10": Counter(), "11": Counter()}
    for linha in linhas:
        partes = E.partes_da_linha(linha)
        exigir(partes is not None, "layout_txt")
        reg = partes["reg"]
        if reg == "10":
            pai = partes["matricula"]
            exigir(pai == matricula, "matricula_txt")
        else:
            exigir(reg == "11" and pai is not None, "layout_txt")
        tipos[reg][linha] += 1

    return {
        "status": item["status"],
        "motivo": item["motivo"],
        "motivos_espelho": motivos,
        "dias_por_motivo": dias,
        "atas_lidas": atas,
        "fechamento_pk": fech.pk,
        "colaborador": fech.colaborador,
        "linhas": linhas,
        "tipos": tipos,
    }


def situacao_e6(ins, linha, chave):
    # app/ponto/management/commands/e6_oraculo.py:336-369
    if linha is None:
        return "sem_linha"
    if linha.get("isencao") or linha.get("ausencia") is not None:
        return "isento_ou_ausencia"
    m_esp, _ = E6.minutos_do_espelho(linha)
    if m_esp is None:
        return "sem_lavratura"
    m_e6 = ins.oraculo.minutos.get(chave, 0.0)
    if m_esp == 0 and m_e6 == 0:
        return "ambos_zero"
    diferenca = m_e6 - m_esp
    if E6.diferenca_e_a_janela_declarada(diferenca, linha):
        return "desconto_janela_declarada"
    return "ate_10_min" if abs(diferenca) <= 10 else "divergencia"


def julgar(foto, empresa, mes, ano, dias, agora):
    # app/ponto/management/commands/e6_oraculo.py:285,297,304,316,320,328,378
    hoje = timezone.localtime(agora).date()
    ini, fim = janelas.janela_fechamento(mes, ano, empresa)
    c = foto["colaborador"]
    esp = espelho_do_colab(c, hoje, ini, fim, agora=agora)
    ins = E6.InsumosDoDono(c, ini, fim, hoje, espelho=esp, agora=agora)
    linhas = {str(l.get("data"))[:10]: l for l in (esp.get("dias") or [])}
    foto["e6"] = {}
    for data in sorted(dias):
        exigir(ini <= data <= fim, "dia_fora_da_janela_e6")
        dono = ins.dono(data)
        exigir(dono in ("CADASTRO", "BATIDA", "ESTRUTURA"), "juiz_sem_resposta")
        chave = str(data)
        foto["e6"][data] = (dono, situacao_e6(ins, linhas.get(chave), chave))
    exigir(timezone.localdate() == hoje, "mudou_dia_do_ato")


class Descartar(io.TextIOBase):
    def __init__(self, falhas=None):
        self.falhas = falhas

    def write(self, texto):
        erro = sys.exc_info()[1]
        if self.falhas is not None and erro is not None:
            self.falhas.append(erro)
        return len(texto)


def contar_erros(falhas):
    objetos = {id(e): e for e in falhas if isinstance(e, BaseException)}
    totais = Counter(
        str(e) if isinstance(e, SondaFalhou) else type(e).__name__
        for e in objetos.values()
    )
    totais.update(e for e in falhas if isinstance(e, str))
    return dict(totais)


def etapa(falhas, acao):
    inicio = len(falhas)
    valor = None
    try:
        with transaction.atomic():
            try:
                valor = acao()
                exigir(not transaction.get_rollback(), "transacao_marcada_para_rollback")
            finally:
                transaction.set_rollback(True)
    except Exception as erro:
        falhas.append(erro)
    erros = contar_erros(falhas[inicio:])
    del falhas[inicio:]
    return valor, erros


def preparar_b():
    usuarios = get_user_model().objects.filter(is_active=True).order_by("-is_superuser", "pk")
    with closing(usuarios.iterator(chunk_size=200)) as ativos:
        dp = next((u for u in ativos if tem_acao(u, "editar_ausencias")), None)
    exigir(dp is not None, "sem_decisor_dp")

    with connection.cursor() as cursor:
        cursor.execute("""
            DO $sonda$
            DECLARE
                s record;
                qualificada text;
                ultimo bigint;
                usada boolean;
                proximo numeric;
            BEGIN
                FOR s IN
                    SELECT n.nspname, c.relname, p.seqcache, p.seqincrement,
                           p.seqmin, p.seqmax, p.seqcycle
                    FROM pg_catalog.pg_sequence p
                    JOIN pg_catalog.pg_class c ON c.oid = p.seqrelid
                    JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace
                    WHERE n.nspname IN ('juliani', 'public')
                    ORDER BY n.nspname, c.relname
                LOOP
                    qualificada := format('%I.%I', s.nspname, s.relname);
                    EXECUTE format('ALTER SEQUENCE %s CACHE %s', qualificada, s.seqcache);
                    EXECUTE 'SELECT last_value, is_called FROM ' || qualificada
                        INTO ultimo, usada;
                    proximo := ultimo::numeric;
                    IF usada THEN
                        proximo := proximo + s.seqincrement;
                    END IF;
                    IF proximo NOT BETWEEN s.seqmin AND s.seqmax THEN
                        IF NOT s.seqcycle THEN
                            CONTINUE;
                        END IF;
                        proximo := CASE WHEN s.seqincrement > 0
                                       THEN s.seqmin ELSE s.seqmax END;
                    END IF;
                    EXECUTE format('ALTER SEQUENCE %s RESTART WITH %s', qualificada, proximo);
                END LOOP;
            END
            $sonda$;
        """)
    return dp


def rejeitar(colab_pk, pks, dp):
    # app/ponto/services/ausencia.py:1057-1088,1090-1104
    ausencias = list(
        Ausencia.objects.filter(pk__in=pks, colaborador_id=colab_pk)
        .select_related("colaborador__empresa").order_by("pk")
    )
    exigir(
        tuple(a.pk for a in ausencias) == pks
        and all((a.tipo, a.status) == (TIPO, ESTADO) for a in ausencias),
        "faltas_mudaram",
    )
    agora = timezone.now()
    for aus in ausencias:
        decidir(
            aus, "rejeitada", por=dp, decisor=dp, aprovada_em=agora,
            motivo="sonda de falta aprovada",
        )
    gravadas = list(
        Ausencia.objects.filter(pk__in=pks).order_by("pk").values_list("pk", "status")
    )
    exigir(gravadas == [(pk, "rejeitada") for pk in pks], "rejeicao_incompleta")


def publicar_foto(foto):
    dias_retidos = [d for ds in foto["dias_por_motivo"].values() for d in ds]
    return {
        "fechamento_pk": foto["fechamento_pk"],
        "motivos_espelho": len(foto["motivos_espelho"] or []),
        "dias_retidos": len(dias_retidos),
        "celula_pks_retidas": sorted({
            d["celula_id"] for d in dias_retidos if d["celula_id"] is not None
        }),
        "dias_por_situacao_e6": dict(Counter(s for _, s in foto["e6"].values())),
        "divergencias_e6_por_dono": dict(Counter(
            d for d, s in foto["e6"].values() if s == "divergencia"
        )),
        "retornos_do_juiz": dict(Counter(d for d, _ in foto["e6"].values())),
    }


def medir(falhas, chave, esperado, dp):
    emp_pk, mes, ano, colab_pk = chave
    grupo, faltas = esperado
    fotos = MEDICOES[chave] = {}
    erros = {}
    pub = {
        "empresa_pk": emp_pk, "colaborador_pk": colab_pk,
        "ausencia_pks": faltas, "erros": erros,
    }
    hoje_ato = timezone.localdate()

    def foto_a():
        vig = E.competencia_entregue(emp_pk, mes, ano, registro=True)
        exigir(vig is not None, "competencia_sem_entrega")
        empresa = vig.empresa
        pub["exportacao_pk"] = vig.pk
        ini, fim = E.janela_competencia(empresa, mes, ano)
        atuais = tuple(
            E.ausencias_na_janela(colab_pk, TIPO, ini, fim).filter(status=ESTADO)
            .order_by("pk").values_list("pk", flat=True)
        )
        exigir(atuais == faltas, "faltas_diferentes_da_sonda3")
        fotos["A"] = fotografar(empresa, mes, ano, colab_pk)
        return empresa

    def foto_b():
        rejeitar(colab_pk, faltas, dp)
        fotos["B"] = fotografar(empresa, mes, ano, colab_pk)
        a, b = fotos["A"], fotos["B"]
        exigir(
            caminho(a, b) == grupo and a["linhas"] != b["linhas"],
            "delta_diferente_da_sonda3",
        )
        dias = {
            d["data"] for foto in (a, b)
            for registros in foto["dias_por_motivo"].values() for d in registros
        }
        exigir(dias, "nenhum_dia_retido")
        agora = timezone.now()
        exigir(timezone.localtime(agora).date() == hoje_ato, "mudou_dia_do_ato")
        _, falhas_e6 = etapa(falhas, lambda: julgar(b, empresa, mes, ano, dias, agora))
        if falhas_e6:
            erros["E6_B"] = falhas_e6
        return dias, agora

    empresa, falhas_a = etapa(falhas, foto_a)
    if falhas_a:
        erros["A"] = falhas_a
        return pub
    contexto, falhas_b = etapa(falhas, foto_b)
    if falhas_b:
        erros["B"] = falhas_b
        return pub

    dias, agora = contexto
    _, falhas_e6 = etapa(
        falhas, lambda: julgar(fotos["A"], empresa, mes, ano, dias, agora),
    )
    if falhas_e6:
        erros["E6_A"] = falhas_e6

    if not erros:
        pub.update({estado: publicar_foto(foto) for estado, foto in fotos.items()})
        a, b = (fotos[estado]["tipos"] for estado in ("A", "B"))
        pub["delta_txt"] = {
            reg: {
                "A": sum(a[reg].values()),
                "B": sum(b[reg].values()),
                "adicionadas": sum((b[reg] - a[reg]).values()),
                "removidas": sum((a[reg] - b[reg]).values()),
            }
            for reg in a
        }
    return pub


def principal(origem=None):
    falhas = []
    saida = {"esperados": 19, "selecionados": 0, "casos": []}

    def observar_log(_logger, registro):
        erro = registro.exc_info[1] if registro.exc_info else None
        args = registro.args if isinstance(registro.args, tuple) else ()
        excecoes = [a for a in args if isinstance(a, BaseException)]
        if erro is not None:
            falhas.append(erro)
        elif excecoes:
            falhas.extend(excecoes)
        elif registro.levelno >= logging.ERROR:
            falhas.append("erro_registrado_sem_tipo")

    def bloquear(*args, **kwargs):
        erro = SondaFalhou("efeito_externo_bloqueado")
        falhas.append(erro)
        raise erro

    def push_suprimido(*args, **kwargs): [nao verificado]
        return {"ok": False, "motivo": "sonda", "token_invalido": False}

    ler_dia_pago = dia_pago.linhas_do_periodo

    def observar_dia_pago(*args, **kwargs):
        # app/ponto/services/espelho.py:420-426
        try:
            return ler_dia_pago(*args, **kwargs)
        except Exception as erro:
            falhas.append(erro)
            raise

    try:
        with ExitStack() as pilha:
            pilha.enter_context(redirect_stdout(Descartar()))
            pilha.enter_context(redirect_stderr(Descartar(falhas)))
            pilha.enter_context(patch.object(logging.Logger, "handle", observar_log))
            pilha.enter_context(timezone.override(settings.TIME_ZONE))

            casos = selecionar(json.load(sys.stdin) if origem is None else origem)
            saida["selecionados"] = len(casos)
            exigir(
                settings.SETTINGS_MODULE == "config.settings.sombra"
                and getattr(settings, "HASNER_SOMBRA", False) is True
                and set(settings.DATABASES) == {"default"}
                and connection.vendor == "postgresql"
                and connection.settings_dict["NAME"] == "sombra"
                and connection.settings_dict["HOST"] == "sombra_db"
                and getattr(connection, "schema_name", None) == "juliani"
                and not connection.in_atomic_block
                and connection.get_autocommit(),
                "guarda_sombra",
            )
            pilha.enter_context(push_disputa_silenciado()) [nao verificado]
            pilha.enter_context(patch.object(push_utils, "_enviar_push_usuario", push_suprimido)) [nao verificado]
            pilha.enter_context(patch.object(fcm_utils, "push_fcm", push_suprimido)) [nao verificado]
            pilha.enter_context(patch.object(dia_pago, "linhas_do_periodo", observar_dia_pago))
            pilha.enter_context(patch.object(socket, "getaddrin

[truncado em 20000 de 23631 caracteres]

_7648 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
