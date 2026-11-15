# CODEX-ESTADO

> Ultima resposta processada: **15/11/2026 00:32** · publicado em **15/11/2026 00:33** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 15/11 00:32 · sessao 76caf82e94c3 · - · - · origem=Ronald (tmux)

(a) O par conferido é recuperável **quando existe a auditoria correspondente**, por dados estruturados; sua existência em produção é [nao verificado]. O escritor fornece ambos os dias ao log em `app/chamados/services/validacao.py:702-707`.

- Modelo: `PerguntaDisputa.data_conferida_em` é `DateTimeField(null=True, blank=True)` em `app/chamados/models.py:2536`; `data_conferida_por` é FK nullable para `auth.User`, com `SET_NULL`, em `app/chamados/models.py:2537-2540`.
- Único escritor explícito atual dos carimbos: `conferir_data_do_fato`, atribuindo `timezone.now()` e `autor`, e salvando somente esses dois campos, em `app/chamados/services/validacao.py:699-701`. O contrato P13 declara essa unicidade em `app/core/tests/test_contract_escritores_estado.py:113-117`.
- A view apenas verifica POST/permissão e delega; não constitui outro escritor dos campos: `app/chamados/views.py:1306-1314`.
- A porta exige justificativa, consulta `data_do_fato_de(p)`, recusa concordância e recusa pergunta já carimbada: `app/chamados/services/validacao.py:689-698`.
- Junto ao carimbo, solicita `LogAuditoria` com `acao='conferir_data'`, `modelo='PerguntaDisputa'`, `objeto_id=p.pk`; descrição contendo ambos os dias e justificativa; `valor_antes={'data_turno': turno.isoformat() if turno else None}` e `valor_depois={'data_evento': ev.isoformat() if ev else None, 'justificativa': just}`: `app/chamados/services/validacao.py:702-707`.
- Os JSONs residem em `LogAuditoria.valor_antes/valor_depois`, com `objeto_id`, `usuario` e `criado_em`: `app/core/models.py:295-307`. `registrar_log` encaminha os payloads sem reconstruí-los em `app/core/auditoria.py:22-24`; `evento` os grava em `app/core/observ.py:133-138`.
- **Carimbo não garante log:** auditoria é best-effort e engole falhas, em `app/core/auditoria.py:25-29` e `app/core/observ.py:105-139`; o save do carimbo ocorre antes da chamada de auditoria, em `app/chamados/services/validacao.py:701-702`.
- `DecisaoPergunta` não possui ato de conferência nem colunas para esse par: vocabulário em `app/chamados/models.py:4000-4011`, campos em `app/chamados/models.py:4014-4027`. A porta de conferência não chama `DecisaoPergunta.registrar`: seu corpo está em `app/chamados/services/validacao.py:683-710`.
- Os campos de conferência não entram em `CAMPOS_QUE_ACORDAM`, em `app/chamados/models.py:3303-3305`; por isso o receiver de outbox retorna nesse save específico, conforme `app/chamados/signals.py:257-259`.

Para a sonda, buscar os logs por `(acao='conferir_data', modelo='PerguntaDisputa', objeto_id=p.pk)`, recuperar o par ordenado `(valor_depois['data_evento'], valor_antes['data_turno'])` e comparar com o par vigente. Ausência, payload inválido ou pares conflitantes devem ficar em `NAO_RECUPERAVEL`; escolher arbitrariamente o log mais recente não resolve a ambiguidade [nao verificado]. Não é possível exigir igualdade entre `LogAuditoria.criado_em` e `data_conferida_em`: o primeiro é `auto_now_add` em `app/core/models.py:307`, enquanto o segundo é calculado antes, em `app/chamados/services/validacao.py:699-702`.

## 15/11 00:32 · sessao 831e9043d8e0 · - · - · origem=Ronald (tmux)

O par vigente é **`(ev, turno)` devolvido por `data_do_fato_de(p)`**, e não simplesmente duas colunas da pergunta: `ev = p.data_evento`; `turno = dia_do_fato(chamado_que_lista(p), fortes=True)`; a divergência é `bool(ev and turno and ev != turno)`. (`app/chamados/services/fio.py:77-94`)

- `PerguntaDisputa.data_evento` é `DateField` anulável. (`app/chamados/models.py:2501-2504`)
- O chamado usado é `p.chamado_destino`, com fallback para `p.disputa.chamado`. (`app/chamados/services/fio.py:116-117`)
- O dia forte desse chamado vem primeiro de **`chamado.celula.data`**; depois da chave declarada pelo catálogo, com fallback para `contexto_json['data_turno']`. Com `fortes=True`, ausência de dia forte retorna `None`, sem consultar batidas/carimbos. (`app/chamados/juizes.py:98-128`)
- O materializador lê esse mesmo par, mas libera a divergência verificando somente se `p.data_conferida_em` está preenchida. (`app/chamados/services/materializacao.py:1197-1199`)
- A porta de conferência lê exatamente esse par e o grava na auditoria: `valor_antes['data_turno'] = turno.isoformat()` e `valor_depois['data_evento'] = ev.isoformat()`, além da justificativa. Portanto existe uma fonte histórica possível; sua presença e integridade em produção estão **[nao verificado]**. (`app/chamados/services/validacao.py:693-707`)

A partição do anexo **cabe neste pedido**: chamar `competencia_do_dia_entregue(p.data_evento, p.colaborador.empresa_id, memo)`. A função retorna `None` sem dia; nos demais casos usa a janela da empresa e o mês/ano do **fim** da janela para consultar K6. (`app/chamados/services/vigia_fio.py:855-865`; `app/chamados/models.py:2511`)

- `True` → `exportada`; `False` → `aberta`; `None` → `sem_data`, preservando a parte própria do anexo. A função retorna esses três estados; o tratamento de `None` pertence ao chamador. (`app/chamados/services/vigia_fio.py:843-865`)
- `janela_atual` respeita o corte da empresa; aceita também seu PK, consultando `dia_inicio_competencia`. (`app/ponto/janelas.py:42-63`)
- K6 exige `ExportacaoDominio` vigente e não invalidada. “Aberta”, nessa medição, significa o resultado falso desse juiz, sem depender do estado da disputa. (`app/folha/export.py:580-594`)

## 15/11 00:32 · sessao ee221ef3acda · - · - · origem=Ronald (tmux)

Numeração do anexo, iniciada em `# E4c...` como linha 1:

| Referência | Linha de `preparo.md` |
|---|---:|
| F1 | 90 |
| F2 | 140 |
| F3 | 208 |
| F4 | 262 |
| F5 | 324 |
| F6 | 392 |
| F7 | 438 |
| F8 | 469 |
| F9 | 503 |
| `arquivos_em_voo.txt` | 93 |
| `atos_pergunta.py` | 144 |
| `E4A_PARQUE.txt` | 145 |
| `test_e3_pendentes_app.py` | 441 |
| `detalhe_local.html` | 443 |
| `meu_atendimento.html` | 444 |
| `efeito_ausencia.py`, `leitor_celula.py`, `motor_calculo_v2.py` | 472 |
| `CORTES.md` | 582 |
| `remedida.r` | 609 |
| Autorreferência `preparo.md` | 614 |
| TSV absolutos | 615–617 |

**Correção do censo anterior:** a leitura literal acrescenta `manage.py` (`preparo.md:4`) e `logs/FIO-E0-V4-E4B/remedida.r` (`preparo.md:609`). Portanto são **87 nomes de arquivos citados = 57 alvos + 30 adicionais**, incluindo a autorreferência; **86 sem a autorreferência**. O cruzamento permanece **23**: 17 alvos + seis apoios. `preparo.md:4`, `preparo.md:20`, `preparo.md:609`, `preparo.md:614`; `diff_nomes.txt:1`.

A identidade de `manage.py` é [nao verificado]: a cópia contém `app/manage.py` e `mensageria/manage.py`, e o preparo usa apenas o basename. `RELATO/TICKETS` em `preparo.md:564` não identifica caminhos inequivocamente; não somei arquivos adicionais por essa menção.

## 15/11 00:31 · sessao 14c61e9af45a · E3-RESTO-R3 · construtor · origem=Code

Entrega registrada com a tabela dos 42 casos, censos, RED/GREEN, comandos, mutantes e R5: [e3_r3_c5_entrega.txt](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:1).

Sem banco: **18 + 168 testes verdes; 32/32 mutantes mortos**. Permanecem **quatro falhas de cores**; ORM não foi executado. [Evidências](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:98). [nao verificado]

Autoriza acrescentar somente o token `laranja` no partial da barra? A proposta passou nos 121 testes dos leitores em memória. [Proposta concreta](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:137). [nao verificado]

A confirmação é necessária porque seu pedido proíbe editar templates. Mantive o template e os selos intactos; também preservei `config/crons.py` pela proibição inicial. [Pendências](/home/ronald/wt-codex-e3-resto-r3-231625/app/chamados/tests/e3_r3_c5_entrega.txt:140).

## 15/11 00:24 · sessao ee221ef3acda · - · - · origem=Ronald (tmux)

Censo normalizado do anexo: **57 arquivos nas F1–F9**, mais **28 arquivos de apoio/autorreferência = 85 arquivos citados**. Sem contar a autorreferência ao próprio `preparo.md`, são 84. As referências relativas/absolutas às mesmas saídas TSV foram unificadas. [nao verificado: anexo fornecido no texto, sem arquivo local numerado]

| Fatia | Quantidade |
|---|---:|
| F1 | 1 |
| F2 | 1 |
| F3 | 5 |
| F4 | 5 |
| F5 | 14 |
| F6 | 7 |
| F7 | 4 |
| F8 | 8 |
| F9 | 12 |
| Total | 57 |

[nao verificado: cálculo sobre o anexo textual]

Os **17 alvos cruzados** são exatamente os 17 da volta 1: `app/api/views.py`; `app/chamados/juizes.py`; `app/chamados/models.py`; `app/chamados/relatorios_disputa.py`; `app/chamados/services/acoes_disputa.py`; `cobranca.py`; `disputa_emissao.py`; `explicador.py`; `fio.py`; `furo_sem_canal.py`; `materializacao.py`; `pergunta_no_app.py`; `raiox.py`; `tarefa_do_dia.py`; `validacao.py`; `vigia_fio.py`; `app/templates/chamados/partials/_card_atendimento.html` (os basenames intermediários ficam em `app/chamados/services/`). [nao verificado: cruzamento dos anexos textuais]

Os **28 adicionais citados** são: [nao verificado: inventário do anexo textual]

1. `logs/AVAIS-1313/prepara_a_espinha_com_agentes.txt`
2. `logs/PREPARACAO/censos/E4c/fatias.md`
3. `logs/E4C/censo.r`
4. `logs/E4C/aval_expand_contract.txt`
5. `app/docs/LEIS.md`
6. `logs/AVAIS-1313/sexta_noite_1311.txt`
7. `app/docs/BACKLOG.md`
8. `censo_ast.py`
9. `e3/ast.tsv`
10. `main/ast.tsv`
11. `unidades.tsv`
12. `unid_dest.tsv`
13. `app/core/juizes.py`
14. `app/docs/JUIZES.md`
15. `logs/E4C/particao_volta1.txt`
16. `CLAUDE.md`
17. `arquivos_em_voo.txt`
18. `app/chamados/services/atos_pergunta.py`
19. `logs/FIO-E0-V4-E4-ESCRITORES/volta2/E4A_PARQUE.txt`
20. `app/chamados/tests/test_e4_escritores.py`
21. `app/chamados/tests/test_e3_pendentes_app.py`
22. `app/templates/chamados/detalhe_local.html`
23. `app/templates/chamados/meu_atendimento.html`
24. `app/ponto/services/efeito_ausencia.py`
25. `app/escala/services/leitor_celula.py`
26. `app/ponto/motor_calculo_v2.py`
27. `app/docs/CORTES.md`
28. `preparo.md`, autorreferência final.

Destes, **seis** também aparecem no diff: `app/core/juizes.py`, `app/docs/BACKLOG.md`, `app/docs/LEIS.md`, `app/docs/JUIZES.md`, `app/docs/CORTES.md`, `app/chamados/tests/test_e3_pendentes_app.py`. Portanto **o cruzamento literal completo é 23/85**, e **o cruzamento dos alvos das fatias é 17/57**. O “17 de 57” bate no seu escopo; para “todos os arquivos citados”, faltam os 28 de apoio no denominador e seis deles no numerador. [nao verificado: cruzamento dos anexos textuais]

F8 e F9: **nenhum dos seus 8+12 arquivos-alvo** está em `diff_nomes.txt`; os comandos alterados no diff possuem outros nomes e não devem ser assimilados aos comandos da F9. [nao verificado: comparação dos anexos textuais]

## 15/11 00:24 · sessao b51159c45936 · - · - · origem=Ronald (tmux)

| Fatia | Arquivo | Commit(s) no anexo por arquivo | Veredito | Prova em `4b75cba10` |
|---|---|---|---|---|
| Transversal; F3/F6 | `app/docs/LEIS.md` | `183d3d27a`, `a3145c9ba` — `diff_commits_por_arquivo.txt` [nao verificado quanto à linha específica] | **SEGUE VALIDA** L-127, Expand-Contract. **MUDA** o contexto do fio: L-129 já está registrada. | `app/docs/LEIS.md:148`; `app/docs/LEIS.md:149` |
| Transversal; F3/F6 | `app/docs/BACKLOG.md` | `183d3d27a`, `b12e86556` — anexo [nao verificado quanto à linha específica] | **SEGUE VALIDA** D04 = atos humanos; D05 conserva fases, exigibilidade aditiva e `aguarda_admin` até efeito. Referência corrigida para `:1999`. Ordem exige censo rerodado. | `app/docs/BACKLOG.md:1999`; `app/docs/BACKLOG.md:9` |
| F2/D01; F5 | `app/docs/JUIZES.md` | `0a567a225`, `be856fb8b`, `4112a571e` — anexo [nao verificado quanto à linha específica] | **SEGUE VALIDA** T como autoridade por instância e DP; sua porta é a fonte dos leitores migrados. O documento declara-se lookup e remete ao registro vivo. D01 não aparece na tabela lida, nem no registro de chamados. | `app/docs/JUIZES.md:7`; `app/docs/JUIZES.md:78`; `app/docs/JUIZES.md:80`; `app/docs/JUIZES.md:85`; `app/docs/JUIZES.md:87`; `app/core/juizes.py:280` |
| F2/D01; F5 | `app/core/juizes.py` | `0a567a225`, `992be9c60` — anexo [nao verificado quanto à linha específica] | **SEGUE VALIDA** a distinção C17/C19 já apresentada na volta 1. Registro conserva C17 e C19 separados; comentário fixa migração dos chamadores para `porta_da_tarefa`. D01 segue sem entrada no bloco de chamados. | `app/core/juizes.py:135`; `app/core/juizes.py:138`; `app/core/juizes.py:159`; `app/core/juizes.py:297`; `app/core/juizes.py:299`; `app/core/juizes.py:302` |
| F3/F4/F7, exceção A20; seção 10 | `app/docs/CORTES.md` | `b12e86556` — anexo [nao verificado quanto à linha específica] | **MUDA** a afirmação do preparo de que resta uma pergunta de lei A20 não respondida: LU-08 já escolhe “conferência vale só para o par conferido”. A regra não comprova implementação; esta permanece [nao verificado]. BACKLOG conserva a formulação antiga. | `app/docs/CORTES.md:440`; `app/docs/BACKLOG.md:1975` |
| F7, apoio | `app/chamados/tests/test_e3_pendentes_app.py` | `992be9c60` — anexo [nao verificado quanto à linha específica] | Mantém **MUDA** da volta 1: o preparo atribui E17 de templates a um teste que efetivamente limita chamadores C17. **SOME** esta referência como prova de catraca de templates. | `app/chamados/tests/test_e3_pendentes_app.py:21`; `app/chamados/tests/test_e3_pendentes_app.py:118`; `app/chamados/tests/test_e3_pendentes_app.py:145`; `app/chamados/tests/test_e3_pendentes_app.py:148` |

Nenhuma dessas referências permite atribuir um trecho individual ao commit informado: o anexo fornece histórico por arquivo, sem diff de conteúdo. [nao verificado] [nao verificado]

## 15/11 00:23 · sessao 1e80124d62b9 · - · - · origem=Ronald (tmux)

| Fatia | Arquivo | Manutenção da volta 1 | Evidência em `4b75cba10` |
|---|---|---|---|
| F1 | `app/chamados/juizes.py` | **Mantém SEGUE VALIDA**: predicado e faces SQL ainda leem projeção; fila usa a face SQL e ordena pelo carimbo. | `app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1548`, `app/chamados/juizes.py:1556`, `app/chamados/juizes.py:1722`, `app/chamados/juizes.py:1724` |
| F2 | `app/chamados/models.py` | **Mantém SEGUE VALIDA** para estado, queryset e apresentação; **mantém MUDA** para `_nascer`, cuja leitura é guarda de entrada do escritor. FE conserva ausência de V/C e **carimbo OU texto**, respeitando tranca; conservação E4b segue deliberadamente na projeção. | `app/chamados/models.py:3124`, `app/chamados/models.py:3454`, `app/chamados/models.py:3468`, `app/chamados/models.py:3641`, `app/chamados/models.py:3664`, `app/chamados/models.py:2922`, `app/chamados/models.py:1906`, `app/chamados/models.py:1907`, `app/chamados/models.py:1909`, `app/chamados/models.py:4067` |
| F3 | `app/chamados/services/validacao.py` | **Mantém SEGUE VALIDA** para guardas R/V; **mantém SOME** para o trabalho pendente de impedir recusa fora do fato: já retorna antes da escrita e chama a reabertura com `ato='recusa'`. | `app/chamados/services/validacao.py:602`, `app/chamados/services/validacao.py:605`, `app/chamados/services/validacao.py:609`, `app/chamados/services/validacao.py:613`, `app/chamados/services/validacao.py:628` |
| F3 | `app/chamados/services/acoes_disputa.py` | **Mantém SEGUE VALIDA** para migração da guarda crua de pendência; **mantém SOME** para tratamento pendente da recusa fora do fato: já traduz para informação. `recusa_motivo` continua sendo POST. | `app/chamados/services/acoes_disputa.py:95`, `app/chamados/services/acoes_disputa.py:81`, `app/chamados/services/acoes_disputa.py:85` |
| F4 | `app/chamados/services/disputa_emissao.py` | **Mantém SEGUE VALIDA** para leituras R/V/C; **mantém SOME** para unificação da reabertura, já na porta do lote; **mantém MUDA** para “reconferir depois da cura” do automático: a guarda já está implementada. | `app/chamados/services/disputa_emissao.py:1621`, `app/chamados/services/disputa_emissao.py:1623`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1592`, `app/chamados/services/disputa_emissao.py:1649`, `app/chamados/services/disputa_emissao.py:1657` |
| F4 | `app/chamados/services/materializacao.py` | **Mantém SEGUE VALIDA** para migração do texto; preservam-se os sítios técnicos A18/A06/A20 citados na volta 1. | `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:1095`, `app/chamados/services/materializacao.py:1099`, `app/chamados/services/materializacao.py:1105`, `app/chamados/services/materializacao.py:1199` |

## 15/11 00:21 · sessao 3a93feb266d8 · CAMINHO-ATE-E4C-RELIDO · leitura · origem=Code

# MAPA DOS RIOS — PERGUNTA E FIO (main 4b75cba10)

Referencias abaixo relativas a `app/`. Os rotulos de Pergunta sao derivados dos campos; `viva` e outro resultado de leitura, e pode ser falsa enquanto o rotulo continua `pendente` ou `respondida`. As setas abaixo respeitam essa diferenca. (`chamados/models.py:2694`, `chamados/models.py:3117`, `chamados/juizes.py:1399`)

## 1. RESUMO

1. Pergunta tem **4 rotulos derivados**, **17 vias de resolucao** e **8 atos de decisao**; nao tem coluna de status. (`chamados/models.py:2694`, `chamados/models.py:2603`, `chamados/models.py:4000`, `chamados/models.py:3117`)
2. Ha **2 portas de nascimento**: datada, com dedup do fato, e conversa, sem esse dedup; recebem respectivamente **4 e 3 chamadas diretas** de producao. (`chamados/models.py:2854`, `chamados/models.py:2872`; nascentes discriminadas abaixo)
3. A resposta tem **3 operacoes escritoras**: resposta normal, automatica e recuperada pela trilha. Reenviar a mesma resposta normal move o instante novamente. (`chamados/services/disputa_emissao.py:1425`, `chamados/services/disputa_emissao.py:1640`, `chamados/services/disputa_emissao.py:1668`, `chamados/tests/test_porta_resposta_pergunta.py:196`)
4. Validacao, devolucao ao admin e recusa ao colaborador sao atos diferentes, com linhas diferentes na sequencia. (`chamados/services/validacao.py:164`, `chamados/services/validacao.py:230`, `chamados/services/validacao.py:628`)
5. O carimbo comum tem **8 chamadas diretas** e consulta C6; identidade tem **5 chamadas**, correspondentes a **5 vias**, julgadas pelo chamador. (`chamados/models.py:3222`, `chamados/tests/test_o261e_f2_porta_carimbo.py:385`, `chamados/tests/test_o261e_f2_porta_carimbo.py:399`; sitios abaixo)
6. A via tem **4 funcoes escritoras declaradas**; a ancora tem **2 escritores declarados**, incluindo o reuso no nascimento. (`chamados/tests/test_o261e_f2_porta_carimbo.py:51`, `chamados/services/fio.py:846`)
7. Nao existe rotulo `expirada`: vence a janela da lavra. Silencio decidido deixa ato proprio; tranca tambem cala por leitura, sem carimbo novo. (`chamados/models.py:2694`, `chamados/veredito_lavrado.py:107`, `chamados/models.py:1988`, `chamados/juizes.py:1403`)
8. Fio/tarefa tem **0 escritores proprios de estado persistido**; coexistem C17, com **1 leitor direto**, e C19, com **6 modulos consumidores externos diretos**. (`chamados/services/tarefa_do_dia.py:8`, `chamados/juizes.py:1664`, `chamados/management/commands/diff_e3_sombra.py:127`, `chamados/services/tarefa_do_dia.py:638`; leitores discriminados abaixo)

## 2. TABELA DE TRANSICOES

“Carimbo” abaixo significa `materializada_em`, `materializada_por` e `via_resolucao`; a atribuicao conjunta mora em `_escrever_carimbo`, e a volta em lote tem escritor proprio. (`chamados/models.py:3180`, `chamados/models.py:3335`)

As provas sao testes/selos **lidos**; resultado de execucao nesta copia: **[nao verificado]**. [nao verificado]

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| 1 | inexistente -> pendente, pergunta datada | Dia do turno, universo/canonica do fato, destino e contrato de saida; recusa `respondida_em` com texto vazio antes da trava, inclusive no reuso/costura. (`chamados/models.py:2917`, `chamados/models.py:2958`, `chamados/models.py:2971`, `chamados/models.py:2988`, `chamados/models.py:2899`, `chamados/models.py:2922`) | `abrir -> _sob_contrato -> _nascer`; nascentes N1 | `chamados/models.py:3097`, `chamados/models.py:3100` | Identidade, dia, celula, destino, texto/tipo/opcoes; assinaturas recebidas viram decisoes; log `pergunta_emitida`. (`chamados/models.py:3097`, `chamados/models.py:3102`, `chamados/models.py:3108`) | `chamados/models.py:3117`; `chamados/services/tarefa_do_dia.py:145` | `chamados/tests/test_porta_pergunta_identidade.py:167::test_MORDE_abrir_2x_o_mesmo_fato_e_uma_pergunta_e_uma_trilha`; `chamados/tests/test_e3_item1b.py:218::test_RED_nascer_recusa_na_porta` |
| 2 | inexistente -> pendente, conversa | Dia civil; destino valido e contrato; porta nao faz dedup do fato; recusa `respondida_em` com texto vazio antes da trava. (`chamados/models.py:2872`, `chamados/models.py:2879`, `chamados/models.py:2917`, `chamados/models.py:2922`) | `abrir_conversa -> _sob_contrato -> _nascer`; nascentes N2 | `chamados/models.py:3097`, `chamados/models.py:3100` | Mesma insercao, com `conversa=True`. (`chamados/models.py:2879`, `chamados/models.py:3097`) | `chamados/models.py:3117`; `chamados/services/tarefa_do_dia.py:145` | `chamados/tests/test_e1_entradas.py:178::test_MORDE_conversa_e_fato_de_nascimento`; `chamados/tests/test_e3_10_cura_no_escritor.py:96::test_RED_nascer_com_respondida_em_e_texto_vazio_e_recusado` |
| 3 | pendente -> respondida; respondida -> respondida; validada/resolvida -> respondida quando reativada | Nas bordas, C6; no core, texto/tipo/opcoes; queda do carimbo depende da via sem lastro; o `save` recusa transicao para instante de resposta com texto vazio. (`chamados/views.py:1256`, `api/views.py:2607`, `chamados/services/disputa_emissao.py:1432`, `chamados/models.py:3412`, `chamados/models.py:3529`, `chamados/models.py:3559`) | `responder_pergunta_disputa_core`; nascentes R1 | `chamados/services/disputa_emissao.py:1467`, `chamados/services/disputa_emissao.py:1480`, `chamados/services/disputa_emissao.py:1508` | Texto/instante; limpa validacao anterior; eventualmente descarimba; Decisao `resposta`, evento da validacao desfeita e nota. O rotulo final continua sujeito a C6/carimbos preservados. (`chamados/services/disputa_emissao.py:1485`, `chamados/services/disputa_emissao.py:1511`, `chamados/services/disputa_emissao.py:1520`, `chamados/services/disputa_emissao.py:1555`, `chamados/models.py:3124`) | `chamados/models.py:3117`; `chamados/services/tarefa_do_dia.py:148`; `api/views.py:2546` | `chamados/tests/test_porta_resposta_pergunta.py:225::test_MORDE_resposta_NOVA_invalida_a_validacao_anterior`; `chamados/tests/test_porta_resposta_pergunta.py:196::test_CARACTERIZACAO_reenviar_a_MESMA_resposta_move_respondida_em` |
| 4 | pendente -> respondida automaticamente | Ja tem texto: recusa. O par solicitado passa por `recusa_respondida_sem_texto -> respondida_sem_texto`; instante com texto vazio/so brancos levanta `RespondidaSemTexto` antes de tocar a instancia. (`chamados/services/disputa_emissao.py:1649`, `chamados/services/disputa_emissao.py:1657`, `chamados/services/fio.py:1602`) | `responder_automaticamente`; chamada `api/foto_recebida.py:48` | `chamados/services/disputa_emissao.py:1659`, `chamados/services/disputa_emissao.py:1662` | Texto/instante e Decisao `resposta`, com motivo automatico. (`chamados/services/disputa_emissao.py:1663`) | `chamados/models.py:3117`; `chamados/services/tarefa_do_dia.py:148` | `chamados/tests/test_porta_resposta_pergunta.py:110::test_cada_escrita_mora_num_ato_declarado`; `chamados/tests/test_e3_10_cura_no_escritor.py:55::test_RED_texto_vazio_ou_so_espacos_e_recusado_sem_escrever` |
| 5 | pendente com texto sem assinatura -> respondida | Exige texto e `respondida_em` vazio; ao persistir, o `save` tambem consulta a recusa do par ilegal. (`chamados/services/disputa_emissao.py:1677`, `chamados/services/disputa_emissao.py:1683`, `chamados/models.py:3529`, `chamados/models.py:3566`) | `carimbar_resposta_pela_trilha`; command `chamados/management/commands/curar_perguntas_em_limbo.py:67` | `chamados/services/disputa_emissao.py:1681`, `chamados/services/disputa_emissao.py:1683` | Instante fornecido pela trilha; Decisao `resposta` e evento. (`chamados/services/disputa_emissao.py:1684`, `chamados/services/disputa_emissao.py:1686`) | `chamados/models.py:3117`; `chamados/services/tarefa_do_dia.py:148` | `chamados/tests/test_porta_resposta_pergunta.py:241::test_MORDE_carimbar_pela_trilha_2x_nao_mexe_no_instante` |
| 6 | respondida -> validada; depois pode seguir para resolvida ou devolucao | Existe, respondeu, ainda nao validada; plausibilidade; folga tem ramo proprio. (`chamados/services/validacao.py:410`, `chamados/services/validacao.py:418`, `chamados/services/validacao.py:421`, `chamados/services/validacao.py:429`, `chamados/services/validacao.py:437`) | `validar_pergunta -> _carimbar_validacao`; nascentes V1 | `chamados/services/validacao.py:159`, `chamados/services/validacao.py:162` | `validada_em/por` e Decisao `validacao`; chama materializacao. (`chamados/services/validacao.py:164`, `chamados/services/validacao.py:486`, `chamados/services/validacao.py:494`) | `chamados/models.py:3124`; `chamados/services/tarefa_do_dia.py:156` | `chamados/tests/test_e1_entradas.py:94::test_MORDE_validacao_e_linha_com_o_relogio_do_carimbo` |
| 7 | validada -> resolvida; pendente aprovada pelo juiz -> resolvida | Unidade de materializacao, juiz do lastro e C6 depois do carimbo. (`chamados/services/materializacao.py:1164`, `chamados/services/materializacao.py:1433`, `chamados/models.py:3222`) | `_materializar_uma`; `materializar_pendentes_aprovadas`; nascentes M1 | `chamados/services/materializacao.py:1283`, `chamados/services/materializacao.py:1437` -> `chamados/models.py:3235` | Carimbo; unidade pode completar assinatura e lavra. Efeitos externos sao feitos pelo materializador. (`chamados/models.py:3213`, `chamados/models.py:3232`, `chamados/services/materializacao.py:1283`) | `chamados/models.py:3124`; `chamados/services/tarefa_do_dia.py:156` | `chamados/tests/test_porta_desfecho_pergunta.py:256::test_MORDE_pendente_que_o_juiz_reprova_fica_pendente`; `chamados/tests/test_o261e_f2_porta_carimbo.py:687::test_MORDE_pendente_com_resposta_por_validar_nao_sai_pelo_fecho_da_disputa` |
| 8 | pergunta do escape/irma -> resolvida por absorcao, se C6 aceitar | Escape define ausencia; `declarou_hora` escolhe via; C6 aceita ou recusa cada irma. (`chamados/services/materializacao.py:603`, `chamados/services/materializacao.py:555`, `chamados/models.py:3222`) | `_materializar_ausencia_de_resposta`; chamada pelo fluxo de resposta/ausencia (`chamados/services/disputa_emissao.py:1525`, `chamados/services/materializacao.py:502`) | `chamados/services/materializacao.py:567` -> `chamados/models.py:3235` | Carimbo e eventual assinatura; Decisao `absorcao` liga pergunta, escape e ausencia. Recusada deixa evento `irma_viva_na_ausencia`. (`chamados/services/materializacao.py:569`, `chamados/services/materializacao.py:575`) | `chamados/services/tarefa_do_dia.py:167`; `chamados/services/materializacao.py:609` | `chamados/tests/test_e2_tarefa_do_dia.py:238::test_absorcao_segue_a_decisao_da_ausencia` |
| 9 | validada -> respondida, devolvida ao admin | Hora declarada, sem materializacao e com motivo escrito. (`chamados/services/validacao.py:176`, `chamados/services/validacao.py:215`, `chamados/services/validacao.py:217`) | `devolver_validacao_recusada`; chamada `chamados/services/validacao.py:489` | `chamados/services/validacao.py:224`, `chamados/services/validacao.py:227` | Limpa assinatura, preserva resposta; Decisao `devolucao`; evento quando a foto mudou. (`chamados/services/validacao.py:230`, `chamados/services/validacao.py:246`) | `chamados/services/tarefa_do_dia.py:154`; `chamados/models.py:3117` | `chamados/tests/test_o261_recusa_fala.py:148::test_idempotente_validar_2x_a_recusada_mesmo_estado_uma_trilha` |
| 10 | respondida -> pendente, recusa humana | Motivo obrigatorio; validada nao pode; precisa ter resposta; `fora_do_fato` devolve recusa explicita antes de escrever. (`chamados/services/validacao.py:590`, `chamados/services/validacao.py:602`, `chamados/services/validacao.py:605`, `chamados/services/validacao.py:609`) | `recusar_pergunta -> reabrir_para_correcao(ato='recusa')`; casca `chamados/services/acoes_disputa.py:80`, que trata `fora_do_fato` como informacao em `chamados/services/acoes_disputa.py:85` | Motivo: `chamados/services/validacao.py:625`; reset: `chamados/services/disputa_emissao.py:1634` -> `chamados/models.py:3363` | Motivo; limpa resposta/assinatura e carimbo; Decisao `recusa` com foto, autor e motivo; nota/evento. (`chamados/services/validacao.py:628`, `chamados/services/validacao.py:633`, `chamados/services/disputa_emissao.py:1617`, `chamados/models.py:3365`) | `chamados/services/tarefa_do_dia.py:152`; `chamados/models.py:3117` | `chamados/tests/test_porta_validacao_pergunta.py:224::test_MORDE_recusar_SEM_request_carimba_reabre_e_deixa_trilha`; `chamados/tests/test_e3_porta_A_reabrir.py:66::test_RED_recusa_de_linha_fora_do_fato_e_falante` |
| 11 | respondida/validada/resolvida -> pendente, quando o universo permite | `perguntas_corrigiveis -> pergunta_sem_lastro_ou_viva`; absorcao rejeitada usa grupo proprio; lote exclui vias fora do fato e verifica o par recebido. (`chamados/juizes.py:1713`, `chamados/juizes.py:1492`, `chamados/services/materializacao.py:609`, `chamados/models.py:3343`, `chamados/models.py:3363`) | `reabrir_para_correcao`; nascentes D1; sem parametro `desfazer_absorcao`. (`chamados/services/disputa_emissao.py:1562`) | `chamados/services/disputa_emissao.py:1595`, `chamados/services/disputa_emissao.py:1634` -> `chamados/models.py:3363` | Limpa texto/resposta/validacao e sempre retira o carimbo pela porta do lote. Decisao com foto anterior inclui carimbo retirado e exclui vias fora do fato; evento de celula. (`chamados/services/disputa_emissao.py:1617`, `chamados/services/disputa_emissao.py:1631`, `chamados/services/disputa_emissao.py:1634`, `chamados/models.py:3365`) | `chamados/services/tarefa_do_dia.py:152`; `chamados/models.py:3117` | `chamados/tests/test_reabertura_fantasma.py:57::test_reabertura_limpa_o_texto_junto_com_o_carimbo`; `chamados/tests/test_e3_porta_A_reabrir.py:32::test_RED_reabertura_comum_escreve_pela_porta_do_lote`; `chamados/tests/test_e3_porta_A_reabrir.py:55::test_RED_fora_do_fato_nao_ganha_trilha_sem_escrita` |
| 12 | pendente/respondida/validada -> resolvida por fato do destino ou fecho sem credor | C6, tranca, pergunta em voo e testemunha externa; fecho sem credor consulta C6 novamente. (`chamados/services/disputa_emissao.py:1967`, `chamados/services/disputa_emissao.py:1973`, `chamados/services/disputa_emissao.py:1977`, `chamados/services/disputa_emissao.py:1986`, `chamados/services/disputa_emissao.py:2055`) | `resolver_perguntas_por_chamado_resolvido`; nascentes F1 | `chamados/services/disputa_emissao.py:1993`, `chamados/services/disputa_emissao.py:2067` -> `chamados/models.py:3235` | Carimbo; completa assinatura apenas havendo texto; primeiro ramo relavra; segundo deixa `fecho_sem_credor`. (`chamados/models.py:3213`, `chamados/services/disputa_emissao.py:1994`, `chamados/services/disputa_emissao.py:2070`) | `chamados/models.py:3117`; `chamados/services/tarefa_do_dia.py:156` | `chamados/tests/test_porta_resposta_pergunta.py:288::test_MORDE_resolver_nao_enterra_resposta_do_colaborador` |
| 13 | pendente -> resolvida, sem turno aberto | Juizo de turno e C6 antes/depois do carimbo. (`chamados/reconciliador.py:341`, `chamados/reconciliador.py:399`, `chamados/models.py:3222`) | `lavrar_sem_turno_aberto` (`chamados/reconciliador.py:341`) | `chamados/reconciliador.py:406` -> `chamados/models.py:3235` | Carimbo `sem_turno_aberto`, se aceito. (`chamados/reconciliador.py:406`) | `chamados/models.py:3117`; `chamados/juizes.py:679` | `chamados/tests/test_o261e_f2_porta_carimbo.py:226::test_so_a_porta_escreve_o_carimbo` |
| 14 | pendente -> resolvida, celula absolveu | Selecao de absolvidas e C6 com o carimbo. (`chamados/reconciliador.py:550`, `chamados/reconciliador.py:621`, `chamados/models.py:3222`) | `retratar_perguntas_absolvidas_por_celula`; consumidor `chamados/reconciliador.py:1316` | `chamados/reconciliador.py:596` -> `chamados/models.py:3235` | Carimbo `celula_absolveu`. (`chamados/reconciliador.py:596`) | `chamados/models.py:3117`; `chamados/juizes.py:1448` | `chamados/tests/test_o261e_f2_porta_carimbo.py:226::test_so_a_porta_escreve_o_carimbo` |
| 15 | pendente -> resolvida, fantasma confirmado | `veredito_orfa`/lampada e C6. (`chamados/reconciliador.py:1136`, `chamados/models.py:3222`) | `encerrar_orfas_pela_lampada -> encerrar_fantasma`; consumidor `chamados/reconciliador.py:1376` | `chamados/reconciliador.py:1148` -> `chamados/models.py:3386` | Carimbo `fantasma` e evento do encerramento. (`chamados/reconciliador.py:1150`) | `chamados/models.py:3117`; `chamados/juizes.py:679` | `chamados/tests/test_o261e_f2_porta_carimbo.py:665::test_MORDE_encerrar_fantasma_nao_carimba_pergunta_viva` |
| 16 | pendente -> resolvida por silencio decidido; ou pergunta -> resolvida por decisao do admin sobre o dia | Silencio: motivo, respostas sem veredito e C6; decisao do dia pertence ao chamador. (`chamados/models.py:1950`, `chamados/models.py:1960`, `chamados/models.py:2011`, `chamados/models.py:3252`) | `fechar_por_admin -> carimbar_identidade`; nascentes I1 | `chamados/models.py:3147`, `chamados/models.py:3273` | Carimbo `admin_decidiu`; no silencio, Decisao `silencio` e evento com motivo. (`chamados/models.py:1988`, `chamados/models.py:1991`) | `chamados/services/tarefa_do_dia.py:165`; `chamados/models.py:3117` | `chamados/tests/test_porta_pergunta_identidade.py:194::test_MORDE_fechar_por_admin_2x_nao_mexe_no_carimbo` |
| 17 | validada -> resolvida por paridade recusada | Guarda do plantio; identidade aceita somente via permitida. (`chamados/services/materializacao.py:930`, `chamados/models.py:3261`) | `_materializar_resposta_hora -> carimbar_identidade` | `chamados/services/materializacao.py:930` -> `chamados/models.py:3273` | Carimbo `paridade_recusada`. (`chamados/services/materializacao.py:930`) | `chamados/juizes.py:1437`; `chamados/models.py:3124` | `chamados/tests/test_o261e_f2_porta_carimbo.py:505::test_MORDE_paridade_gravando_admin_decidiu` |
| 18 | validada -> resolvida por fato ocupado | Recusa/colisao do materializador e conjunto de identidade. (`chamados/services/materializacao.py:1089`, `chamados/models.py:3261`) | `_carimbar_recusa -> carimbar_identidade` | `chamados/services/materializacao.py:1105` -> `chamados/models.py:3273` | Carimbo `fato_ocupado` e motivo. (`chamados/services/materializacao.py:1105`) | `chamados/juizes.py:1437`; `chamados/models.py:3124` | `chamados/tests/test_o261e_f2_porta_carimbo.py:498::test_cada_juiz_de_identidade_grava_a_SUA_via` |
| 19 | pergunta excedente -> resolvida, fora do fato por duplicata | Canonica/plano de retratacao e guarda de identidade. (`chamados/services/fio.py:1264`, `chamados/services/fio.py:1379`, `chamados/models.py:3263`) | `retratar_para_canonica -> retratar_duplicata`; nascentes I2 | `chamados/services/fio.py:1398` -> `chamados/models.py:3273` | Via `duplicata`, referencia a canonica no motivo; conserva carimbo anterior quando existente. (`chamados/services/fio.py:1398`, `chamados/models.py:3265`) | `chamados/juizes.py:1412`; `chamados/models.py:3284` | `chamados/tests/test_leitores_ouvem_a_lei.py:28::test_duplicata_respondida_sai_da_fila` |
| 20 | pergunta do motivo removido -> resolvida, fora do fato | Remocao do motivo e guarda de identidade. (`chamados/models.py:2035`, `chamados/models.py:3263`) | `DisputaSupervisao.remover_motivo -> carimbar_identidade` | `chamados/models.py:2125` -> `chamados/models.py:3273` | Decisao `retratacao` e via `motivo_retratado`; preserva a pergunta. (`chamados/models.py:2120`, `chamados/models.py:2125`) | `chamados/juizes.py:1412`; `chamados/models.py:3284` | `chamados/tests/test_e1_entradas.py:142::test_MORDE_a_retratada_morre_e_tem_linha` |
| 21 | resolvida -> pendente/respondida/validada, conforme assinaturas preservadas | Via especifica nos atalhos; C6 no sem lastro; fora do fato recusa. (`chamados/models.py:3395`, `chamados/models.py:3412`, `chamados/models.py:3419`, `chamados/services/relavra_via.py:242`, `chamados/models.py:3289`) | `reabrir_fantasma`, `reabrir_por_competencia`, `reabrir_por_resposta`, `desfazer_carimbo_sem_lastro -> descarimbar

[truncado em 20000 de 76763 caracteres]

## 15/11 00:14 · sessao 63ff6ea79e59 · CAMINHO-ATE-E4C-RELIDO · leitura · origem=Code

# MAPA DOS RIOS — ChamadoColaborador ENCERRA — main 4b75cba10

Convencao: caminhos abaixo relativos a `app/`. Cada linha da matriz representa um par origem → destino; portas alternativas ficam na mesma linha. `ENCERRADOS` significa `resolvido/fechado`; `TERMINAIS` acrescenta `cancelado/superado`. (`chamados/catalogo/motor.py:30`, `chamados/catalogo/motor.py:31`)

## 1. RESUMO

1. Ha **7 estados declarados**, sem estado literal `encerrado`. (`chamados/catalogo/motor.py:19`)
2. O encerramento runtime admite **11 pares origem → destino**, realizados por **21 combinacoes de par/porta**: humana 8, reconciliar 3, encerrar 4, competencia trancada 3 e cancelar 3. (`chamados/catalogo/motor.py:99`, `chamados/catalogo/motor.py:111`, `chamados/catalogo/motor.py:114`, `chamados/catalogo/motor.py:116`, `chamados/catalogo/motor.py:153`, `chamados/models.py:1677`)
3. Sao **5 portas decisorias** e **1 atalho**, `fechar`; o escritor runtime da transicao e `_gravar_estado`. (`chamados/models.py:128`, `chamados/models.py:452`, `chamados/models.py:642`, `chamados/models.py:820`, `chamados/models.py:1652`, `chamados/models.py:1669`, `chamados/models.py:296`)
4. O censo abaixo enumera **50 sitios externos de encerramento**: R=34, E=4, T=1, C=2, H=8, atalho `fechar`=1; o encaminhamento interno de `fechar` acrescenta um sitio, sem acrescentar transicao. (`chamados/models.py:455`; sitios discriminados na secao 2)
5. A migration 0039 acrescenta **2 pares historicos**: `cancelado → superado` e a reversa. (`chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)
6. L2: **0 escritores externos encontrados no runtime; 4 sitios de estado em migracoes**, dos quais 2 pertencem ao encerramento. Os selos excluem migracoes. (`chamados/models.py:296`, `chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`, `chamados/tests/test_selo_status_so_pelas_portas.py:34`)
7. `cancelar` e `encerrar` tambem perguntam pela pergunta viva; a descricao “sem guard” de `encerrar` nao descreve seu corpo atual. (`chamados/models.py:833`, `chamados/models.py:1654`, `chamados/models.py:1664`)
8. L3: **2 terminais sem saida runtime; 0 sem saida considerando tambem ida/reversa das migracoes**. Ocorrencias e frequencias em producao: **[nao verificado]**. (`chamados/catalogo/motor.py:104`, `chamados/catalogo/motor.py:106`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

## 2. TABELA DE TRANSICOES

**Portas e juizes usados na matriz:**

- **H — `transicionar`**, tambem alcancada por `fechar`: `pode_transicionar` consulta o motor; verifica disputa viva para ambos os destinos, ancoradas quando a origem e viva, justificativa pendente para `fechado`, furo/premissa para `resolvido`. (`chamados/models.py:119`, `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:165`, `chamados/models.py:181`, `chamados/models.py:455`)
- **R — `reconciliar`**: `_decisao_do_fechamento` consulta motor, premissa, conversa do pai e pergunta viva, nessa ordem. (`chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689`)
- **E — `encerrar`**: motor `ENCERRAR` e pergunta viva. (`chamados/models.py:1661`, `chamados/models.py:1664`)
- **T — `encerrar_por_competencia_trancada`**: exige origem viva, motor `ENCERRAR` e pergunta viva. A verificacao da tranca esta no servico chamador. (`chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684`, `chamados/services/competencia_trancada.py:95`, `chamados/services/competencia_trancada.py:192`)
- **C — `cancelar`**: motor `CANCELAR` e pergunta viva. (`chamados/models.py:830`, `chamados/models.py:833`)
- **Nucleo comum**: antes da atribuicao, `_gravar_estado` ainda possui uma guarda que levanta erro se a travessia vivo → terminal deixar pergunta viva ancorada. (`chamados/models.py:219`, `chamados/models.py:224`, `chamados/models.py:225`, `chamados/models.py:228`)

**Persistencia comum P:** `status_local`, `resolvido_em`, `via_encerramento`, `atualizado_em`; rebaixamento de urgencia e preservacao da original quando aplicavel. H tambem escreve `respondido_por`; C escreve esse autor somente quando recebido. O carimbo existente e preservado entre encerrados. (`chamados/models.py:228`, `chamados/models.py:229`, `chamados/models.py:230`, `chamados/models.py:243`, `chamados/models.py:246`, `chamados/models.py:835`)

**Trilha comum:** tentativa de `LogAuditoria.acao='estado_chamado'`, com antes/depois, via e ator, antes do `save`, dentro do atomic. Essa trilha e best-effort. O wrapper coleta recusas S143 e escreve Pauta para destinos em `ENCERRADOS`; `cancelado` passa pelo ramo sem esse coletor. (`chamados/models.py:280`, `chamados/models.py:292`, `chamados/models.py:296`, `core/observ.py:98`, `core/observ.py:105`, `chamados/services/fecho_runtime.py:158`, `chamados/services/fecho_runtime.py:168`)

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| 1 | aberto → resolvido | H: `chamados/models.py:132`, `chamados/models.py:181`; R: `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689` | H `transicionar`/`fechar`; R `reconciliar`; nascentes H/R abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; H via `admin`, R via `sistema`; trilha comum (`chamados/models.py:450`, `chamados/models.py:655`) | `chamados/signals.py:97`; `chamados/juizes.py:779` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 2 | em_analise → resolvido | H: `chamados/models.py:132`, `chamados/models.py:181`; R: `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689` | H `transicionar`/`fechar`; R `reconciliar`; nascentes H/R abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`sistema`; trilha comum (`chamados/models.py:450`, `chamados/models.py:655`) | `chamados/signals.py:97`; `chamados/juizes.py:779` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 3 | registrado → resolvido | H somente `admin_ti`: `chamados/catalogo/motor.py:153`; R: `chamados/models.py:674`, `chamados/models.py:683`, `chamados/models.py:686`, `chamados/models.py:689`; guardas H acima | H `transicionar`/`fechar`; R `reconciliar`; nascentes parametrizadas H e nascentes R abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`sistema`; trilha comum (`chamados/models.py:450`, `chamados/models.py:655`) | `chamados/signals.py:97`; `chamados/juizes.py:779` | `chamados/tests/test_motor_invariantes.py::test_humana_espelha_pode_transicionar` (`:27`); `::test_selo_sistema` (`:107`) |
| 4 | fechado → resolvido | H somente `admin_ti`: `chamados/catalogo/motor.py:150`, `chamados/catalogo/motor.py:153`; disputa e premissa: `chamados/models.py:154`, `chamados/models.py:181` | H `transicionar`/`fechar`; entradas parametrizadas H abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; via `admin`; preserva carimbo existente; autor e trilha (`chamados/models.py:243`, `chamados/models.py:450`) | `chamados/signals.py:97`; `chamados/services/painel.py:221` | `chamados/tests/test_motor_invariantes.py::test_fechado_so_sai_por_reabrir_ou_admin_ti` (`:61`) |
| 5 | aberto → fechado | H: `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`; E: `chamados/models.py:1661`, `chamados/models.py:1664`; T: `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684` | H `transicionar`/`fechar`; E `encerrar`; T `encerrar_por_competencia_trancada`; nascentes H/E/T abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`cascade`/`competencia_trancada`; T acrescenta nota/evento no servico (`chamados/models.py:450`, `chamados/models.py:1666`, `chamados/models.py:1686`, `chamados/services/competencia_trancada.py:210`, `chamados/services/competencia_trancada.py:215`) | `chamados/signals.py:97`; `chamados/views_arquivar.py:64` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`); `chamados/tests/test_competencia_trancada.py::test_MORDE_chamado_de_dia_trancado_em_decidir_sai_pelo_ato_de_trancar` (`:68`) |
| 6 | em_analise → fechado | H: `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`; E: `chamados/models.py:1661`, `chamados/models.py:1664`; T: `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684` | H `transicionar`/`fechar`; E `encerrar`; T `encerrar_por_competencia_trancada`; nascentes H/E/T abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`cascade`/`competencia_trancada`; trilha comum e complemento T (`chamados/models.py:450`, `chamados/models.py:1666`, `chamados/models.py:1686`, `chamados/services/competencia_trancada.py:215`) | `chamados/signals.py:97`; `chamados/views_arquivar.py:164` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 7 | registrado → fechado | H: `chamados/catalogo/motor.py:105`, `chamados/models.py:136`, `chamados/models.py:154`, `chamados/models.py:166`; E: `chamados/models.py:1661`, `chamados/models.py:1664`; T: `chamados/models.py:1677`, `chamados/models.py:1679`, `chamados/models.py:1684` | H `transicionar`/`fechar`; E `encerrar`; T `encerrar_por_competencia_trancada`; nascentes H/E/T abaixo | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; vias `admin`/`cascade`/`competencia_trancada`; trilha comum e complemento T (`chamados/models.py:450`, `chamados/models.py:1666`, `chamados/models.py:1686`, `chamados/services/competencia_trancada.py:215`) | `chamados/signals.py:97`; `chamados/services/painel.py:222` | `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`); `::test_selo_sistema` (`:107`) |
| 8 | resolvido → fechado | H: `chamados/models.py:132`, `chamados/models.py:136`, `chamados/models.py:154`; E: `chamados/models.py:1661`; guarda de pergunta retorna cedo fora de VIVOS (`chamados/models.py:417`) | H `transicionar`/`fechar`; E `encerrar`; nascentes H/E abaixo; T recusa essa origem (`chamados/models.py:1677`) | `_gravar_estado`, `chamados/models.py:228`, `chamados/models.py:296` | P; preserva carimbo existente; via `admin` ou `cascade`; trilha comum (`chamados/models.py:243`, `chamados/models.py:450`, `chamados/models.py:1666`) | `chamados/signals.py:97`; `chamados/services/painel.py:222` | `chamados/tests/test_encerrar_gate.py::test_resolvido_para_fechado` (`:20`); `chamados/tests/test_motor_invariantes.py::test_selo_humana` (`:105`) |
| 9 | aberto → cancelado | C: `chamados/models.py:830`, `chamados/models.py:833` | `cancelar`; nascentes C abaixo | `_gravar_estado`, `chamados/models.py:835`, `chamados/models.py:296` | P; via `cancelado`; nota `cancelamento` se motivo; autor opcional (`chamados/models.py:835`, `chamados/models.py:836`, `chamados/models.py:838`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_motor_cancelado.py::test_cancela_vivos` (`:22`); `chamados/tests/test_o261e_f1_porta_encerramento.py::test_g_RED_cancelar_com_viva_recusa` (`:101`) |
| 10 | em_analise → cancelado | C: `chamados/models.py:830`, `chamados/models.py:833` | `cancelar`; nascentes C abaixo | `_gravar_estado`, `chamados/models.py:835`, `chamados/models.py:296` | P; via `cancelado`; nota se motivo; autor opcional (`chamados/models.py:835`, `chamados/models.py:838`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_motor_cancelado.py::test_cancela_vivos` (`:22`) |
| 11 | registrado → cancelado | C: `chamados/models.py:830`, `chamados/models.py:833` | `cancelar`; nascentes C abaixo | `_gravar_estado`, `chamados/models.py:835`, `chamados/models.py:296` | P; via `cancelado`; nota se motivo; autor opcional (`chamados/models.py:835`, `chamados/models.py:838`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_motor_cancelado.py::test_cancela_vivos` (`:22`) |
| 12 | cancelado → superado — migracao | Modulo duplicavel, data presente e irmao unico elegivel (`chamados/migrations/0039_a3_via_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:16`, `chamados/migrations/0039_a3_via_backfill.py:20`, `chamados/migrations/0039_a3_via_backfill.py:23`) | `backfill`, registrado em `RunPython` (`chamados/migrations/0039_a3_via_backfill.py:39`) | `queryset.update`, `chamados/migrations/0039_a3_via_backfill.py:24` | `status_local`, via `superado`, `superado_por_id`; essa escrita nao carimba `resolvido_em` nem chama a trilha runtime (`chamados/migrations/0039_a3_via_backfill.py:24`) | `chamados/services/historico.py:145`, `chamados/services/historico.py:178` | `chamados/tests/test_esmeril2_migracoes_reversas.py::test_0039_superado_ida_volta_idempotente` (`:33`); `::test_0039_irmao_alheio_nao_barra_nem_e_escolhido` (`:42`) |
| 13 | superado → cancelado — reversa | Filtro `status_local='superado'` (`chamados/migrations/0039_a3_via_backfill.py:33`) | `desfazer`, reversa do `RunPython` (`chamados/migrations/0039_a3_via_backfill.py:39`) | `queryset.update`, `chamados/migrations/0039_a3_via_backfill.py:33` | Estado `cancelado`, limpa `superado_por`; depois limpa via de todos os chamados (`chamados/migrations/0039_a3_via_backfill.py:33`, `chamados/migrations/0039_a3_via_backfill.py:34`) | `chamados/services/painel.py:223`; `chamados/services/historico.py:178` | `chamados/tests/test_esmeril2_migracoes_reversas.py::test_0039_superado_ida_volta_idempotente` (`:33`) |

### Nascentes diretas da matriz

**R — 34 sitios, todos pedindo `resolvido`:**

- `_retratar`: `chamados/reconciliador.py:524`; signal `disputa_fechada_fecha_chamado_pai`: `chamados/signals.py:43`.
- `fechar_adesao_com_batida`: `colaboradores/services/pos_vinculo.py:43`; comando, `_marcar`: `colaboradores/management/commands/marcar_feriado_do_posto.py:141`.
- `julgar_colab` e `_julgar_colab_corpo`, **6 sitios**: `ponto/services/cartorio.py:1011`, `ponto/services/cartorio.py:1539`, `ponto/services/cartorio.py:1552`, `ponto/services/cartorio.py:1573`, `ponto/services/cartorio.py:1651`, `ponto/services/cartorio.py:1666`.
- `detectar_ausencias`, **4 sitios**: `handle`, `_fechar_por_afastamento`, `_fechar_por_cobertura`, `_fechar_chamado`: `ponto/management/commands/detectar_ausencias.py:159`, `ponto/management/commands/detectar_ausencias.py:826`, `ponto/management/commands/detectar_ausencias.py:837`, `ponto/management/commands/detectar_ausencias.py:849`.
- `sincronizar_adesao`, **2 sitios**: `chamados/services/adesao.py:116`, `chamados/services/adesao.py:151`.
- `aceitar_sugestao` e duas tentativas de `agrupar_furo`, **3 sitios**: `chamados/services/limbo_folgas.py:102`, `chamados/services/limbo_folgas.py:284`, `chamados/services/limbo_folgas.py:287`.
- `_materializar_ausencia_de_resposta` e `_fecha`, **2 sitios**: `chamados/services/materializacao.py:588`, `chamados/services/materializacao.py:777`.
- `designar_cobertura` e `declarar_cobertura_passada`, **2 sitios**: `ponto/services/cobertura.py:123`, `ponto/services/cobertura.py:319`.
- `fechar_pelo_marco_colab`: `ponto/services/lastro.py:401`; `resolver_cobranca_da_entrada`: `ponto/services/entrada_sincronizada.py:32`.
- `reconciliar_colab`: `ponto/services/cluster_espurio.py:190`; `fechar_chamado_da_ausencia`: `ponto/services/ausencia.py:758`.
- `fechar_o_fio_do_dia`: `ponto/services/veredito_celula.py:203`; `_autoresolve_f2_f4`: `ponto/services/triagem_batida.py:598`.
- Comandos `_ata_sem_furo` e `handle`: `ponto/management/commands/curar_residuo_bug115.py:155`, `ponto/management/commands/detectar_intervalo_ausente.py:139`.
- `reconciliar_por_ausencia`: `chamados/services/silencio_ausencia.py:102`; `feriado_reavaliar.encerrar`: `chamados/services/feriado_reavaliar.py:48`; comando `handle`: `chamados/management/commands/auditar_chamados_orfaos.py:68`.

**E — 4 sitios, todos pedindo `fechado`:**

- `encerrar_com_retencao`: `chamados/reconciliador.py:497`.
- `_executar_aceite`: `colaboradores/services/vinculo.py:404`.
- `executar_desligamento`: `colaboradores/services/desligamento.py:187`.
- `materializar_regularizacao_externa`: `chamados/services/regularizacao_ext.py:206`.

**T — 1 sitio:** `competencia_trancada.encerrar`, pedindo `fechado`; acrescenta nota e evento especificos apos sucesso. (`chamados/services/competencia_trancada.py:209`, `chamados/services/competencia_trancada.py:210`, `chamados/services/competencia_trancada.py:215`)

**C — 2 sitios:** comandos `fechar_chamados_vinculo_na_lista` e `fechar_avisos_regua_na_lista`, ambos pedindo `cancelado` e passando motivo. (`ponto/management/commands/fechar_chamados_vinculo_na_lista.py:58`, `escala/management/commands/fechar_avisos_regua_na_lista.py:48`)

**H — 8 sitios externos sem `ato`:**

- `acao_aprovar_excecao`, `acao_responder_agenda`, `acao_responder_escala`, `acao_autorizar_geofence`, `acao_recusar_geofence`: pedem `resolvido`. (`chamados/services/acoes_chamado.py:71`, `chamados/services/acoes_chamado.py:110`, `chamados/services/acoes_chamado.py:155`, `chamados/services/acoes_chamado.py:229`, `chamados/services/acoes_chamado.py:286`)
- `acao_resposta_livre` e `processar_post_fio`: destino parametrizado, conferido contra `STATUS_CHOICES`, depois julgado pela porta. (`chamados/services/acoes_chamado.py:322`, `chamados/services/acoes_chamado.py:325`, `chamados/services/fio.py:757`, `chamados/services/fio.py:760`)
- `avaliar_solicitacao_beneficio`: `resolvido` ao aprovar, `fechado` no outro ramo. (`colaboradores/views.py:2072`)

**Atalho `fechar` — 1 sitio externo:** comando `fechar_chamados_meio_periodo`; chama `ch.fechar`, que encaminha internamente para H e acrescenta nota quando recebe resolucao. (`ponto/management/commands/fechar_chamados_meio_periodo.py:110`, `chamados/models.py:455`, `chamados/models.py:458`)

**Envelopes excluidos do encerramento:** `_chamado_para_decisao` pede somente `em_analise`; no smoke, uma chamada percorre a matriz, outra reabre e outra testa recusas. O smoke recusa banco sem `test` no nome e reverte sua transacao. (`chamados/services/acoes_chamado.py:241`, `chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`, `chamados/management/commands/smoke_chamados.py:135`, `chamados/management/commands/smoke_chamados.py:148`, `chamados/management/commands/smoke_chamados.py:164`)

### Nascentes anteriores: commands, crons e eventos

**Commands que chegam indiretamente a R:**

- Por `retratar → _retratar → reconciliar`: `retratar_chamados_lei_cala`, `reconciliar_grade`, `reconciliar_vinculo`, `reconciliar_fantasmas` e `_resolver_chamado_do_turno` de `processar_alertas_turno`. (`chamados/management/commands/retratar_chamados_lei_cala.py:59`, `ponto/management/commands/reconciliar_grade.py:309`, `ponto/management/commands/reconciliar_vinculo.py:68`, `ponto/management/commands/reconciliar_fantasmas.py:280`, `ponto/management/commands/processar_alertas_turno.py:216`, `chamados/reconciliador.py:547`, `chamados/reconciliador.py:524`)
- Por servicos especificos: `fechar_chamados_ausencia`, `reavaliar_ausencias_lancadas`, `detectar_cluster_espurio`, `apurar_furos_diarios` e `processar_cartorio`. (`ponto/management/commands/fechar_chamados_ausencia.py:44`, `ponto/management/commands/reavaliar_a

[truncado em 20000 de 37085 caracteres]

## 15/11 00:14 · sessao f2529de0f105 · CAMINHO-ATE-E4C-RELIDO · leitura · origem=Code

# MAPA DOS RIOS — ChamadoColaborador NASCE

Caminhos relativos a `app/`, exceto os iniciados por `bin/`. HASH informado: `4b75cba1018af37173b5da7aa15475c13468f559`.

## 1. RESUMO

1. **2 portas de nascimento:** `abrir`, com dedup, e `criar`, sem dedup interno. (`chamados/models.py:880`, `chamados/models.py:1101`)
2. **38 sitios operacionais diretos:** 19 chamam `abrir`, 19 chamam `criar`; inventario individual abaixo, com arquivo:linha.
3. **1 sitio adicional fora de `tests/`:** o smoke; seu `handle` exige banco com `test` no nome e desfaz a transacao. (`chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`, `chamados/management/commands/smoke_chamados.py:124`)
4. **2 chegadas de nascimento nos fluxos operacionais:** `aberto` e `registrado`; a natureza escolhe EMITIR ou REGISTRAR. (`chamados/models.py:1094`, `chamados/catalogo/motor.py:130`)
5. `abrir` serializa a busca e o nascimento sob trava; a identidade vem do lookup do chamador, sem unicidade declarada no modelo. (`chamados/models.py:107`, `chamados/models.py:949`, `chamados/models.py:953`)
6. **9 nascentes de `criar` fazem pre-dedup externo**, com criterios diferentes, discriminados no inventario abaixo. (`ponto/utils.py:31`, `ponto/utils.py:94`, `ponto/views.py:284`, `ponto/services/triagem_batida.py:102`, `ponto/services/triagem_batida.py:325`, `ponto/services/triagem_batida.py:452`, `ponto/services/geofence.py:353`, `ponto/management/commands/processar_alertas_avancados.py:100`, `chamados/services/abertura.py:165`)
7. **1 funcao escreve a trilha do nascimento**, com antes `None` e depois status/modulo/via; o registro e best-effort. (`chamados/models.py:1256`, `core/observ.py:105`)
8. **L2: 0 escritores de runtime encontrados fora das portas; 4 sitios historicos nas migrations**, excluidas pelo selo de runtime. (`chamados/tests/test_selo_status_so_pelas_portas.py:15`, `chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

## 2. TABELA DE TRANSICOES

A tabela conta o nascimento efetivo da entidade. Pegar existente e recusar nascimento nao criam uma transicao; `abrir` pode encaminhar um existente encerrado ao sitio de renascimento. (`chamados/models.py:953`, `chamados/models.py:969`, `chamados/models.py:1012`, `chamados/models.py:1017`)

| # | estado de partida -> estado de chegada | juiz (arquivo:linha) | operacao (funcao) | escritor (arquivo:linha) | o que persiste (campos/modelos/trilha) | leitores (arquivo:linha) | prova (teste ou selo que morde, arquivo::nome, ou "nenhum") | [nao verificado]
|---|---|---|---|---|---|---|---|
| N1 | inexistente -> aberto | Natureza acionavel: `chamados/catalogo/natureza.py:36`; escolha EMITIR: `chamados/models.py:1094`; motor: `chamados/catalogo/motor.py:130`; absolvimento pela celula: `chamados/models.py:1150`; veto da regua: `chamados/models.py:1173`. Dedup de `abrir`: `chamados/models.py:953`, `chamados/models.py:974`. | `abrir` nas 19 nascentes A; `criar` nas nascentes B, exceto B2/B8/B9; B15/B17 dependem do modulo. Chamadas e argumentos no inventario abaixo. | `abrir`: `chamados/models.py:1018`; `criar`: `chamados/models.py:1142`; trilha: `chamados/models.py:1265`; auditoria: `core/observ.py:134`. | `ChamadoColaborador`, `status_local=aberto`, campos recebidos e celula resolvida pela porta; datas automaticas: `chamados/models.py:67`; resolucao da celula: `chamados/models.py:1000`, `chamados/models.py:1113`. `LogAuditoria.nascimento_chamado`, antes `None`, depois status/modulo/via: `chamados/models.py:1265`. Para modulo de turno com celula, `EventoCelula`: `chamados/models.py:1024`, `chamados/models.py:1144`. | Painel: `chamados/services/painel.py:213`; receiver do cartorio: `chamados/signals.py:189`; receiver de isencao: `chamados/signals.py:284`; auditoria generica: `core/views.py:705`; contador por cron: `chamados/services/vigia_loop.py:86`. | `chamados/tests/test_a3_registrado.py::test_cobranca_continua_aberto` — `chamados/tests/test_a3_registrado.py:34`; `chamados/tests/test_chamado_nasce_com_trilha.py::test_abrir_deixa_uma_linha_so_no_nascimento_real` — `chamados/tests/test_chamado_nasce_com_trilha.py:38`. |
| N2 | inexistente -> registrado | Natureza de auditoria: `chamados/catalogo/natureza.py:18`; escolha REGISTRAR: `chamados/models.py:1094`; motor: `chamados/catalogo/motor.py:133`; mesmas guardas de celula/regua: `chamados/models.py:1118`, `chamados/models.py:1122`. | `criar`: B2, B8, B9, B15 quando `gps_ausente`, B17 quando recebe modulo de auditoria. `abrir` tambem comporta REGISTRAR, mas nenhuma das 19 nascentes A pede modulo de auditoria; catalogo A abaixo e `chamados/models.py:982`. | `criar`: `chamados/models.py:1142`; capacidade de `abrir`: `chamados/models.py:985`, `chamados/models.py:1018`; trilha: `chamados/models.py:1265`. | Mesmo modelo e trilha, com `status_local=registrado`: `chamados/models.py:1110`, `chamados/models.py:1142`, `chamados/models.py:1268`. `registrado` pertence a VIVOS, mas nao a COBRANCA: `chamados/catalogo/motor.py:24`, `chamados/catalogo/motor.py:29`. | Contador proprio de registrados: `chamados/services/fila.py:126`; natureza/dono: `chamados/catalogo/natureza.py:44`; receivers: `chamados/signals.py:189`, `chamados/signals.py:284`; auditoria: `core/views.py:705`. | `chamados/tests/test_a3_registrado.py::test_criar_auditoria_nasce_registrado` — `chamados/tests/test_a3_registrado.py:15`; `chamados/tests/test_a3_registrado.py::test_abrir_auditoria_nasce_registrado_e_dedupa_por_vivos` — `chamados/tests/test_a3_registrado.py:21`. |

### 2a. Nascentes diretas de `abrir` — 19

Em todas, o lookup inclui o colaborador. A porta consulta exatamente esse lookup; os filtros de status sao retirados apenas na segunda busca, destinada ao filho agrupado sob pai vivo. (`chamados/models.py:953`, `chamados/models.py:974`)

| ID | nascente / funcao | lookup alem do colaborador |
|---|---|---|
| A1 | `ponto/management/commands/detectar_par_relampago.py:368` — `handle` | `par_relampago` + `contexto_json__data_turno`; constante em `ponto/management/commands/detectar_par_relampago.py:17`. |
| A2 | `ponto/management/commands/reconciliar_grade.py:507` — `_emitir_furo` | `batida_ausente` + `contexto_json__data_turno`; constante em `ponto/management/commands/reconciliar_grade.py:54`. |
| A3 | `ponto/management/commands/detectar_ausencias.py:191` — `handle` | `sem_previsao_escala` + `status_local__in=VIVOS`. |
| A4 | `ponto/management/commands/detectar_ausencias.py:272` — `handle` | `ancora_limbo`. |
| A5 | `ponto/management/commands/detectar_ausencias.py:511` — `_chamado_dia_sem_grade` | `sem_previsao_escala` + subcategoria `dia_sem_grade` + VIVOS. |
| A6 | `ponto/management/commands/detectar_ausencias.py:694` — `_abrir_chamado` | `batida_ausente` + `contexto_json__data_turno`; constante em `ponto/management/commands/detectar_ausencias.py:29`. |
| A7 | `ponto/management/commands/emitir_turno_aberto_retroativo.py:151` — `handle` | `turno_aberto_24h` + `contexto_json__data_turno`. |
| A8 | `ponto/management/commands/processar_alertas_turno.py:335` — `_abrir_chamado_critico` | `turno_aberto` + `contexto_json__data_turno`. |
| A9 | `ponto/services/cluster_espurio.py:196` — `reconciliar_colab` | `cluster_batida_espuria` + `contexto_json__data_turno`; constante em `ponto/services/cluster_espurio.py:11`. |
| A10 | `ponto/services/ausencia.py:722` — `abrir_chamado_ausencia_colab` | `ausencia_colab` + `contexto_json__ausencia_id`. |
| A11 | `ponto/views.py:3052` — `ping_geo` | `ping_geo_fora` + `criado_em__date`. |
| A12 | `escala/regua_defesa.py:51` — `vetar_cobranca_se_degenerada` | `regua_degenerada` + `contexto_json__tipo_escala_id`. |
| A13 | `api/views.py:1969` — `api_justificativas` | `justificativa_colab` + categoria `justificativa` + VIVOS. |
| A14 | `chamados/reconciliador.py:911` — `reemitir_chamado_do_dia` | `batida_ausente` + `contexto_json__data_turno`; chave em `chamados/reconciliador.py:836`. |
| A15 | `chamados/services/disputa_emissao.py:568` — `_abrir_ou_agregar_disputa` | `disputa_supervisao` + categoria homonima + VIVOS. |
| A16 | `chamados/services/disputa_emissao.py:1104` — `criar_disputa_a_partir_de_chamados_orfaos` | `disputa_supervisao_manual` + categoria `disputa_supervisao` + subcategoria `orfao_14h` + VIVOS. |
| A17 | `chamados/services/disputa_emissao.py:1339` — `criar_disputa_ancora_de_chamado` | Mesmo modulo/categoria de A16, subcategoria `ancora_ausente` + VIVOS. |
| A18 | `chamados/services/adesao.py:75` — `sincronizar_adesao` | `sem_previsao_escala` + subcategoria `nunca_bateu`. |
| A19 | `chamados/views.py:973` — `abrir_chamado_rapido` | `mensagem_dp` + VIVOS. |

### 2b. Nascentes diretas de `criar` — 19 operacionais + 1 smoke

`criar` nao pesquisa chamado existente. O estado default e substituido pelo destino do motor; um `status` explicito diferente de `aberto` permanece como recebido. (`chamados/models.py:1101`, `chamados/models.py:1107`, `chamados/models.py:1110`, `chamados/models.py:1142`)

| ID | nascente / funcao | modulo, chegada e pre-dedup externo |
|---|---|---|
| B1 | `colaboradores/services/vinculo.py:410` — `_executar_aceite` | `escala_colaborador` -> aberto; encerra anteriores e cria novo: `colaboradores/services/vinculo.py:401`. |
| B2 | `colaboradores/services/desligamento.py:195` — `executar_desligamento` | `desligamento_revisao` -> registrado; modulo em `colaboradores/services/desligamento.py:208`. |
| B3 | `colaboradores/views.py:1981` — `solicitar_beneficio` | `beneficio` -> aberto. |
| B4 | `colaboradores/views.py:2367` — `agenda_enviar_aceite` | `agenda_dia` -> aberto. |
| B5 | `ponto/management/commands/processar_alertas_avancados.py:121` — `handle` | `turno_aberto_24h` -> aberto. Busca primeiro `turno_aberto` vivo da data; depois `turno_aberto_24h` em NAO_FECHADOS, sem data nessa segunda busca: `ponto/management/commands/processar_alertas_avancados.py:100`, `ponto/management/commands/processar_alertas_avancados.py:105`. |
| B6 | `ponto/utils.py:40` — `bloquear_se_sem_posto` | `colaborador_sem_posto` -> aberto; pre-dedup por colaborador/modulo/data de criacao: `ponto/utils.py:31`. |
| B7 | `ponto/utils.py:103` — `bloquear_se_em_ferias` | `colaborador_em_ferias` -> aberto; pre-dedup por colaborador/modulo/data de criacao: `ponto/utils.py:94`. |
| B8 | `api/views.py:717` — `api_bater_ponto` | `batida_fora_escala_leve` -> registrado; ramo de desvio leve: `api/views.py:705`, modulo em `api/views.py:725`. |
| B9 | `api/views_core.py:711` — `api_bater_ponto` | `batida_fora_escala_leve` -> registrado; ramo de desvio leve: `api/views_core.py:696`, modulo em `api/views_core.py:719`. |
| B10 | `api/views.py:2173` — `api_abrir_chamado` | `app_colaborador` -> aberto; modulo em `api/views.py:2182`. |
| B11 | `ponto/services/justificativa.py:291` — `rejeitar_justificativa_dp` | `justificativa` -> aberto; origem precisa estar pendente: `ponto/services/justificativa.py:277`. |
| B12 | `ponto/services/triagem_batida.py:107` — `triagem_bater_ponto` | `acesso_bloqueado` -> aberto; pre-dedup por colaborador/modulo/VIVOS: `ponto/services/triagem_batida.py:102`. |
| B13 | `ponto/services/triagem_batida.py:336` — `ramo_solicitar_anuencia` | `batida_fora_horario` -> aberto; pre-dedup por colaborador/modulo/VIVOS: `ponto/services/triagem_batida.py:325`. |
| B14 | `ponto/services/triagem_batida.py:459` — `_bloqueio_horario_clt` | `batida_fora_horario` -> aberto; pre-dedup por colaborador/modulo/janela de 14h, sem filtro de estado: `ponto/services/triagem_batida.py:452`. |
| B15 | `ponto/services/geofence.py:361` — `verificar_geofence` | `gps_ausente` -> registrado; `gps_fora_geofence` -> aberto; escolha em `ponto/services/geofence.py:344`. Pre-dedup por colaborador/subcategoria/VIVOS: `ponto/services/geofence.py:353`. |
| B16 | `chamados/services/regularizacao_ext.py:247` — `criar_regularizacao_externa` | `regularizacao_externa` -> aberto. |
| B17 | `chamados/services/abertura.py:47` — `abrir_chamado_local` | Modulo variavel -> aberto ou registrado; a tela aceita chaves de MOTIVOS: `chamados/views.py:170`, `chamados/views.py:174`; a categoria pode substituir o modulo: `chamados/services/abertura.py:30`. |
| B18 | `chamados/services/abertura.py:172` — `abrir_chamado_para_colaborador` | Modulo limitado aos tipos do catalogo -> aberto: `chamados/services/abertura.py:148`, `chamados/catalogo/modulos.py:306`. Pre-dedup sob trava por colaborador/modulo/VIVOS/assunto/descricao/prazo/autor: `chamados/services/abertura.py:164`, `chamados/services/abertura.py:165`. |
| B19 | `ponto/views.py:294` — `bater_ponto` | `entrada_bloqueada_turno_aberto` -> aberto; pre-dedup por colaborador/modulo/VIVOS: `ponto/views.py:284`. |
| B20 | `chamados/management/commands/smoke_chamados.py:124` — `_novo` | Passa `status` explicitamente. O laco cria origens aberto/em_analise/resolvido/registrado; fechado e criado separadamente: `chamados/management/commands/smoke_chamados.py:131`, `chamados/management/commands/smoke_chamados.py:147`, `chamados/catalogo/motor.py:99`. O `handle` restringe o banco e faz rollback: `chamados/management/commands/smoke_chamados.py:33`, `chamados/management/commands/smoke_chamados.py:44`. |

As chegadas dos sitios B decorrem da natureza do modulo e da conversao do default na porta. (`chamados/catalogo/natureza.py:18`, `chamados/catalogo/natureza.py:36`, `chamados/models.py:1110`)

### 2c. Entradas indiretas, commands e crons

Estes sitios alimentam as nascentes A/B; nao representam novos escritores de nascimento. As cadeias abaixo terminam nos escritores de `abrir`/`criar`. (`chamados/models.py:1018`, `chamados/models.py:1142`)

- **Formularios:** `chamados/views.py:197` -> B17; `chamados/views.py:136` -> B18; ausencia em `ponto/views.py:2579` e `api/views.py:1845` -> A10; regularizacao em `colaboradores/views.py:2987` e `chamados/services/regularizacao_ext.py:319` -> B16.
- **Batida:** `ponto/views.py:353` -> ramo que chama bloqueio horario em `ponto/services/triagem_batida.py:657` e geofence em `ponto/services/triagem_batida.py:753`; APIs chamam geofence em `api/views.py:781` e `api/views_core.py:775`.
- **Sem posto/ferias:** `ponto/services/triagem_batida.py:84` -> B6; `chamados/detector_anomalias.py:116` -> B7; detector chamado em `api/views.py:434` e `api/views_core.py:420`.
- **Regua degenerada:** `ponto/management/commands/detectar_ausencias.py:601` e `ponto/management/commands/reconciliar_grade.py:495` -> A12.
- **Cartorio/eventos:** cluster em `ponto/services/cartorio.py:1042` -> A9; emissor em `ponto/services/cartorio.py:1791` e `ponto/services/cartorio.py:1829` -> A6; fabrica em `ponto/services/cartorio.py:1045`, com chamada de emissao em `chamados/services/fabrica_da_celula.py:172`.
- **Reemissao:** `chamados/reconciliador.py:1622` -> A14, pelo reconciliador em `chamados/reconciliador.py:1434`, alimentado por eventos em `ponto/signals.py:215` e `chamados/signals.py:370`.
- **Cobrar dia:** `chamados/views_cobrar.py:325` -> `chamados/services/cobrar_dia.py:41`; repasse de orfaos em `chamados/services/cobrar_dia.py:49`.

| command ou script | caminho para nascimento / modo de escrita |
|---|---|
| `emitir_furo_retroativo` | Chama o emissor A6 em `ponto/management/commands/emitir_furo_retroativo.py:154`. |
| `processar_cartorio` | Chama julgamento com emissor em `ponto/management/commands/processar_cartorio.py:65`; chega a A6 pelos sitios do cartorio citados acima. |
| `vigia_de_hora` | `ponto/management/commands/vigia_de_hora.py:28` -> julgamento em `ponto/services/vigia_de_hora.py:131`. |
| `detectar_cluster_espurio --apply` | Guarda de escrita em `ponto/management/commands/detectar_cluster_espurio.py:69`; chama A9 em `ponto/management/commands/detectar_cluster_espurio.py:81`. |
| `apurar_furos_diarios` | Chama adesao/A18 em `escala/management/commands/apurar_furos_diarios.py:17`. |
| `efetivar_desligamentos_agendados --apply` | Chama desligamento/B2 em `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57`. |
| `cobrar_furos_sem_cobranca --apply` | Guarda em `ponto/management/commands/cobrar_furos_sem_cobranca.py:27`; chama servico em `ponto/management/commands/cobrar_furos_sem_cobranca.py:30`, que julga em `ponto/services/furos_sem_cobranca.py:128`. |
| `celula_veredito_velho --aplicar` | Julgamento em `ponto/management/commands/celula_veredito_velho.py:144`; sem aplicar, rollback em `ponto/management/commands/celula_veredito_velho.py:156`. |
| `curar_residuo_bug115 --apply` | Chama A5 em `ponto/management/commands/curar_residuo_bug115.py:106`; outro ramo julga em `ponto/management/commands/curar_residuo_bug115.py:146`; guarda em `ponto/management/commands/curar_residuo_bug115.py:91`. |
| `rejulgar_ausencia_em_folga --apply` | Guardas em `ponto/management/commands/rejulgar_ausencia_em_folga.py:70` e `ponto/management/commands/rejulgar_ausencia_em_folga.py:73`; julgamento em `ponto/management/commands/rejulgar_ausencia_em_folga.py:78`. |
| `marcar_foto_ausente_retro --apply` | Guarda em `ponto/management/commands/marcar_foto_ausente_retro.py:89`; chama emissao em `ponto/management/commands/marcar_foto_ausente_retro.py:96`, chegando a A15 por `chamados/services/disputa_emissao.py:662`. |
| `reconciliar_grade --apply` | Guarda em `ponto/management/commands/reconciliar_grade.py:250`; chama A2 em `ponto/management/commands/reconciliar_grade.py:254` e A15 pelo sitio `ponto/management/commands/reconciliar_grade.py:424`. |
| Commands diretos que tambem repassam orfaos | `detectar_ausencias`: `ponto/management/commands/detectar_ausencias.py:803`; `processar_alertas_turno`: `ponto/management/commands/processar_alertas_turno.py:385`; `processar_alertas_avancados`: `ponto/management/commands/processar_alertas_avancados.py:159` -> A16. |
| Smoke, entrada indireta | Chama A15 em `chamados/management/commands/smoke_chamados.py:100`, alem de B20; sujeito ao mesmo rollback em `chamados/management/commands/smoke_chamados.py:44`. |
| `bin/r5_idempotencia_frota.py` | Chama `processar_cartorio(apply=True)` em `bin/r5_idempotencia_frota.py:109`, nas rodadas de `bin/r5_idempotencia_frota.py:125` e `bin/r5_idempotencia_frota.py:127`; destino real da execucao: [nao verificado]. |

**Agendamento presente no codigo:** alertas de turno a cada 5 minutos, avancados a cada 30, ausencias a cada 5, vigia de hora a cada 10; desligamentos com `--apply`; par relampago com `--apply --retratar`; cartorio com `--apply`; foto retroativa com `--apply`; apuracao de furos encadeada ao cartorio. (`config/crons.py:362`, `config/crons.py:363`, `config/crons.py:364`, `config/crons.py:367`, `config/crons.py:440`, `config/crons.py:605`, `config/crons.py:616`, `config/crons.py:686`, `config/crons.py:1527`)

**Agendamentos que nao emitem nesse modo:** grade sem `--apply`; cluster com `--alarme`. Retroativo de turno e declarado como ato, sem horario. (`config/crons.py:462`, `config/crons.py:598`, `config/crons.py:952`, `config/crons.py:961`)

### 2d. Dedup, recusas e trilha

- **Encontrou pelo lookup:** devolve o existente com `False`; se esta em ENCERRADOS, chama o sitio de renascimento antes de devolver. Isso nao e um novo nascimento. (`chamados/models.py:953`, `chamados/models.py:968`, `chamados/models.py:971`)
- **[nome] agrupado sob pai vivo:** a busca sem filtros de status pode satisfazer o dedup; devolve o filho com `False`. (`chamados/models.py:974`, `chamados/models.py:978`, `chamados/models.py:979`)
- **Celula absolve:** nao cria chamado; `abrir` devolve `(None, False)`, `criar` devolve `None`. A trilha da recusa deduplica por celula/modulo/veredito, sem criterio temporal nessa consulta. (`chamados/models.py:1009`, `chamados/models.py:1118`, `chamados/models.py:1246`)
- **Regua veta:** nao cria chamado; registra `nascimento_recusado` na celula ou no colaborador. A dedup da trilha considera modulo/dia/veto/via e verifica se houve nascimento posterior. (`chamados/models.py:1014`, `cha

[truncado em 20000 de 31069 caracteres]

## 15/11 00:13 · sessao cbdef0a5c43a · CAMINHO-ATE-E4C-RELIDO · censo · origem=Code

**(a) O cruzamento histórico fica [nao verificado].** A cópia disponibilizada não tem `.git`; não foi possível executar `git diff --name-only a0bf6e78e 4b75cba10` nem identificar o commit de cada sítio. [nao verificado]

O anexo declara **57 arquivos-alvo nas F1–F9 e 17 arquivos diferentes entre E3 v10 e main**; essa contagem não comprova o diff entre os hashes solicitados. O total literal incluindo documentos, testes, logs e rascunhos citados também fica [nao verificado]. [nao verificado]

Há registro documental do merge da E3: **`90f3fa66b`, incorporando `1ee368261`**. Isso identifica o pouso, sem atribuir cada alteração individualmente. `app/docs/RELATO.md:15`.

A tabela relê os **17 arquivos assinalados no anexo**, condicionada ao cruzamento histórico ainda [nao verificado].

| Fatia / arquivos do anexo | Resultado da releitura em `4b75cba10` | Commit que mudou o sítio |
|---|---|---|
| **F1 — `app/chamados/juizes.py`** | **SEGUE VALIDA** a migração da fonte: `respondida_sem_texto` e suas faces SQL continuam lendo projeção; a fila já usa `q_respondida_com_texto`, mas ordena por `respondida_em`. A centralização SQL existe; a leitura por DP continua pendente nesses sítios. `app/chamados/juizes.py:1528`, `app/chamados/juizes.py:1548`, `app/chamados/juizes.py:1556`, `app/chamados/juizes.py:1722`, `app/chamados/juizes.py:1724`. | [nao verificado] |
| **F2 — `app/chamados/models.py`** | **SEGUE VALIDA** para `estado`, `q_vivas`, `respondida` e `resposta_display`, ainda apoiados na projeção. **MUDA** o enquadramento de `_nascer`: sua leitura do par em `campos` já é uma guarda de entrada do escritor. FE deve conservar **carimbo OU texto**, além de ausência de V/C e da condição de tranca. A conservação E4b permanece uma leitura deliberada da projeção. `app/chamados/models.py:3124`, `app/chamados/models.py:3455`, `app/chamados/models.py:3641`, `app/chamados/models.py:3664`, `app/chamados/models.py:2922`, `app/chamados/models.py:1906`, `app/chamados/models.py:4067`. | [nao verificado] |
| **F3 — `app/chamados/services/validacao.py`; `app/chamados/services/acoes_disputa.py`** | **SEGUE VALIDA** a migração das guardas R/V. **SOME do trabalho pendente** o tratamento da recusa fora do fato: a porta já recusa antes da escrita, chama a reabertura canônica com `ato='recusa'`, e a casca já trata `fora_do_fato` como informação. `recusa_motivo` da casca continua sendo POST. `app/chamados/services/validacao.py:602`, `app/chamados/services/validacao.py:605`, `app/chamados/services/validacao.py:609`, `app/chamados/services/validacao.py:628`, `app/chamados/services/acoes_disputa.py:81`, `app/chamados/services/acoes_disputa.py:85`. | [nao verificado] |
| **F4 — `app/chamados/services/disputa_emissao.py`; `app/chamados/services/materializacao.py`** | **SEGUE VALIDA** a migração do texto/projeção nos leitores. **SOME do trabalho pendente** a unificação da reabertura: os dois ramos já passam por `descarimbar_em_lote`; o parâmetro antigo tem lápide. **MUDA** “reconferir depois da cura” do automático: a recusa do par ilegal já está implementada. As exceções técnicas A18/A06/A20 continuam nos sítios indicados. `app/chamados/services/disputa_emissao.py:1592`, `app/chamados/services/disputa_emissao.py:1621`, `app/chamados/services/disputa_emissao.py:1634`, `app/chamados/services/disputa_emissao.py:1657`, `app/chamados/services/materializacao.py:835`, `app/chamados/services/materializacao.py:1095`, `app/chamados/services/materializacao.py:1199`. | [nao verificado] |
| **F5 — `app/chamados/services/fio.py`; `app/chamados/services/explicador.py`** | **SEGUE VALIDA**: espera do texto, guarda do POST, mérito da duplicata e explicação ainda leem R. As exceções de pendência, recusa técnica e identidade permanecem no explicador. `app/chamados/services/fio.py:187`, `app/chamados/services/fio.py:698`, `app/chamados/services/fio.py:1271`, `app/chamados/services/fio.py:1283`, `app/chamados/services/explicador.py:79`, `app/chamados/services/explicador.py:85`, `app/chamados/services/explicador.py:90`, `app/chamados/services/explicador.py:95`. | [nao verificado] |
| **F5 — `app/chamados/services/vigia_fio.py`** | **SEGUE VALIDA** a existência da leitura R em `censo_alcance`. **MUDA** a justificativa da migração de `respondida_sem_resposta`: seu contrato atual mede explicitamente o **par escrito**, desde um marco temporal. Trocar isso por contagem de atos vazios preservando a mesma pergunta é [nao verificado]. `app/chamados/services/vigia_fio.py:694`, `app/chamados/services/vigia_fio.py:795`, `app/chamados/services/vigia_fio.py:798`, `app/chamados/services/vigia_fio.py:804`, `app/chamados/services/vigia_fio.py:807`. | [nao verificado] |
| **F5 — `app/chamados/services/raiox.py`; `app/chamados/services/tarefa_do_dia.py`** | **SEGUE VALIDA** a migração do crédito: ainda lê projeção, já distinguindo carimbo **e** texto. **MUDA** a referência da matriz de T para `_pela_decisao:131`; as ações carregam porta calculada pela âncora na fila. A leitura A06 de `_tarefa_pura` está em `:245`. `app/chamados/services/raiox.py:135`, `app/chamados/services/raiox.py:138`, `app/chamados/services/raiox.py:143`, `app/chamados/services/tarefa_do_dia.py:131`, `app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:245`. | [nao verificado] |
| **F5 — `app/chamados/services/cobranca.py`** | **MUDA** o censo da leitura indireta: `_pendentes` chama `consulta_do_app`; `_calados` também depende de `espera_o_texto_do_colaborador` **e** P. A certidão registra a retirada do filtro cru de “calado”. `app/chamados/services/cobranca.py:47`, `app/chamados/services/cobranca.py:230`, `app/chamados/services/cobranca.py:241`. | [nao verificado] |
| **F5 — `app/chamados/services/pergunta_no_app.py`** | **SOME o predicado cru de `consulta_do_app`, não a função**: ela permanece delegando à partição. **SEGUE VALIDA** a migração em `particao_do_app`. **MUDA** o RED proposto: as chaves existentes são `pendentes` e `historico`; não existe a saída `"respondidas"` descrita no preparo. `app/chamados/services/pergunta_no_app.py:64`, `app/chamados/services/pergunta_no_app.py:73`, `app/chamados/services/pergunta_no_app.py:81`, `app/chamados/services/pergunta_no_app.py:85`. | [nao verificado] |
| **F5 — `app/chamados/services/furo_sem_canal.py`** | **SOME o sítio cru; a E3 já fez a delegação**. `medir` continua existindo e lê `carga.tarefa`/`porta_da_tarefa`; ausência de canal é calculada pela ausência da porta publicada. `app/chamados/services/furo_sem_canal.py:43`, `app/chamados/services/furo_sem_canal.py:89`, `app/chamados/services/furo_sem_canal.py:96`, `app/chamados/services/furo_sem_canal.py:104`. | [nao verificado] |
| **F6 — `app/api/views.py`** | **SEGUE VALIDA** a migração da apresentação e das guardas: texto/instante continuam na projeção; `aguarda_admin` ainda equivale a `estado == RESPONDIDA`. **SOME** a derivação crua da correção em `api_chamados_v2`, que já chama `aguarda_o_colaborador`. `app/api/views.py:1159`, `app/api/views.py:1161`, `app/api/views.py:1175`, `app/api/views.py:2549`, `app/api/views.py:2557`, `app/api/views.py:2573`, `app/api/views.py:2616`. | [nao verificado] |
| **F6 — `app/chamados/relatorios_disputa.py`** | **SOME o predicado próprio, não `classificar`**: a função permanece; a certidão registra a substituição das duas regras por `pode_fechar` e `aguarda_o_colaborador`. `app/chamados/relatorios_disputa.py:11`, `app/chamados/relatorios_disputa.py:15`, `app/chamados/relatorios_disputa.py:24`, `app/chamados/relatorios_disputa.py:26`. | [nao verificado] |
| **F7 — `app/templates/chamados/partials/_card_atendimento.html`** | **SEGUE VALIDA** a migração da apresentação R. **SOME** o julgamento cru de envio: o template já lê `resposta_enviada`. **MUDA** a referência da mistura pendência/via/recusa para `:189`/`:192`. `app/templates/chamados/partials/_card_atendimento.html:100`, `app/templates/chamados/partials/_card_atendimento.html:106`, `app/templates/chamados/partials/_card_atendimento.html:114`, `app/templates/chamados/partials/_card_atendimento.html:189`, `app/templates/chamados/partials/_card_atendimento.html:192`. | [nao verificado] |

Há outra correção em **F7**: `test_e3_pendentes_app.py:118` contém `CatracaC17Test`; o teste fixa **um chamador de C17**, permitido em `diff_e3_sombra.comparar`. Esse sítio não sustenta a catraca de templates E17 atribuída pelo preparo. `app/chamados/tests/test_e3_pendentes_app.py:118`, `app/chamados/tests/test_e3_pendentes_app.py:145`, `app/chamados/tests/test_e3_pendentes_app.py:148`, `app/chamados/tests/test_e3_pendentes_app.py:21`.

| Fatias restantes | Situação |
|---|---|
| **F8 e F9** | A inclusão de seus arquivos no diff solicitado fica [nao verificado]. A ausência deles entre os 17 do anexo não certifica ausência de alteração entre os hashes. [nao verificado] |

**C17×C19**

| Item | C17 | C19 |
|---|---|---|
| **Pergunta literal do registro** | “por onde alguem chega a este dia (porta alcancavel, com o verbo do painel)?” `app/core/juizes.py:138`. | “quais acoes ainda sao exigiveis para cada instancia de marco do dia, de quem e por quais portas?” `app/core/juizes.py:159`. |
| **Docstring** | “Por onde alguem CHEGA a este dia?” `app/chamados/juizes.py:1665`. | “para o colaborador, o dia do turno e cada INSTANCIA de marco, quais acoes ainda sao exigiveis, de quem e por qual porta.” `app/chamados/services/tarefa_do_dia.py:654`. |
| **Função registrada** | `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)`. `app/chamados/juizes.py:1664`; `app/core/juizes.py:297`. | `tarefa_do_dia(colab, dia_do_turno, agora, *, carga=None)`. `app/chamados/services/tarefa_do_dia.py:653`; `app/core/juizes.py:299`. |
| **Universo** | Uma célula: chamados da célula na fila e perguntas C6 vivas no app, sob âncora na fila, excluindo respondida sem texto. `app/chamados/juizes.py:1667`, `app/chamados/juizes.py:1701`, `app/chamados/juizes.py:1703`. | Colaborador × dia × `agora`, por instância de marco; perguntas do fato (`conversa=False`), decisões ordenadas por `ato_em,pk`, limitadas ao horizonte `agora`. `app/chamados/services/tarefa_do_dia.py:654`, `app/chamados/services/tarefa_do_dia.py:395`, `app/chamados/services/tarefa_do_dia.py:411`, `app/chamados/services/tarefa_do_dia.py:412`. |
| **Predicado / resultado** | Existe porta da fila **ou** porta de pergunta elegível? Sem portas retorna `None`; havendo, retorna rota, fila, verbos e fio pela escada dos verbos. `app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1703`, `app/chamados/juizes.py:1705`, `app/chamados/juizes.py:1709`. | Quais ações permanecem exigíveis após decisão e conferência do efeito? Retorna razão e ações com responsável/porta, podendo declarar desconhecimento. Uma ação pode ter `porta=None`. `app/chamados/services/tarefa_do_dia.py:131`, `app/chamados/services/tarefa_do_dia.py:142`, `app/chamados/services/tarefa_do_dia.py:151`, `app/chamados/services/tarefa_do_dia.py:159`. |

Os testes fixam esses contratos nas seguintes asserções: [nao verificado]

| Juiz / teste | Asserção | [nao verificado]
|---|---|
| **C17 — fila sem pergunta**, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:159` | `assertEqual({'rota':'porta_fila', 'fila':(a.pk,), 'verbos':{a.pk:'decidir'}, 'fio':'chamado'}, self._juiz())`. `app/chamados/tests/test_o261e_f5b_juiz_porta.py:161`. |
| **C17 — respondida sob âncora na fila**, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:193` | `assertEqual({'rota':'porta_pergunta', 'fila':(), 'verbos':{b.pk:'validar'}, 'fio':'admin'}, self._juiz())`. `app/chamados/tests/test_o261e_f5b_juiz_porta.py:196`. |
| **C17 — registrado**, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:200` | `assertIsNone(self._juiz())`. `app/chamados/tests/test_o261e_f5b_juiz_porta.py:203`. |
| **C17 — escada**, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:300` | `validar+cobrar → colab`; `validar+decidir → admin`; `decidir → chamado`; `None → chamado`. `app/chamados/tests/test_o261e_f5b_juiz_porta.py:304`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:305`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:306`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:307`. |
| **C19 — instâncias independentes**, `app/chamados/tests/test_e2_tarefa_do_dia.py:159` | Uma instância exige `validar/admin`; outra permanece exigindo `responder/colaborador`. `app/chamados/tests/test_e2_tarefa_do_dia.py:165`, `app/chamados/tests/test_e2_tarefa_do_dia.py:167`. |
| **C19 — teto por marco**, `app/chamados/tests/test_e2_tarefa_do_dia.py:170` | `assertEqual((razao, acoes), ('nao_venceu', []))` para o marco final. `app/chamados/tests/test_e2_tarefa_do_dia.py:174`. |
| **C19 — ação com e sem porta**, `app/chamados/tests/test_e2_tarefa_do_dia.py:396` | Primeiro exige `responder/colaborador/porta=ch.pk`; após silenciamento, conserva `responder/colaborador/porta=None`. `app/chamados/tests/test_e2_tarefa_do_dia.py:405`, `app/chamados/tests/test_e2_tarefa_do_dia.py:408`. |
| **C19 — ordem pelo ato**, `app/chamados/tests/test_e2_tarefa_do_dia.py:425` | VALIDACAO posterior no tempo vence RESPOSTA de pk maior retrodatada: `('validada_sem_efeito', [('materializar','sistema')])`. `app/chamados/tests/test_e2_tarefa_do_dia.py:428`, `app/chamados/tests/test_e2_tarefa_do_dia.py:429`, `app/chamados/tests/test_e2_tarefa_do_dia.py:432`. |

**duas** — C17 julga alcance de porta; C19 julga exigibilidade por instância, inclusive com porta ausente. `app/chamados/juizes.py:1705`; `app/chamados/services/tarefa_do_dia.py:654`; `app/chamados/tests/test_e2_tarefa_do_dia.py:408`.

## 15/11 00:11 · sessao 894ece4013e2 · - · - · origem=Ronald (tmux)

Auditoria de C17/C19 e referências destes arquivos concluída, somente leitura. Evidência do histórico: `git diff a0bf6e78e 4b75cba10 -- <arquivo>` e `git log a0bf6e78e..4b75cba10 -- <arquivo>`.

**Mapeamento de todas as referências do mapa no meu recorte**

| Arquivo | Antes → depois |
|---|---|
| `chamados/juizes.py` | 679→679; 693→693; 974→974; 1004→1004; 1029→1029; 1066→1066; 1076→1076; 1399→1399; 1403→1403; 1405→1405; 1407→1407; 1412→1412; 1437→1437; 1444→1444; 1448→1448; 1464→1464; 1483→1483; 1492→1492; 1504→1504; 1513→1513; **1622→1664; 1653→1695; 1671→1713** |
| `chamados/services/tarefa_do_dia.py` | 8→8; **116→131; 126→141; 130→145; 133→148; 137→152; 139→154; 141→156; 143→158; 150→165; 152→167; 538→574; 562→653** |
| `escala/services/cadastro_realidade.py` | **460→469**, com mudança do juiz consultado |
| `chamados/services/vigia_fio.py` | **640→646–649**, com mudança da carga/juiz |
| `folha/fora_da_folha.py` | **67→70**, com mudança de assinatura e juiz |
| `chamados/management/commands/backfill_fio_morto_e4b.py` | 87→87; 143→143; 147→147 |
| `chamados/management/commands/diff_e3_sombra.py` | **115→117; 125→128** |
| `chamados/services/raiox.py` | **175→179** |
| `chamados/tests/test_e3_portas_tarefa.py` | **75→85** |

Os trechos de C6 citados até `juizes.py:1513` não mudaram entre os hashes; preservar texto e referências dessas partes. Os deslocamentos posteriores vêm das inserções e mudanças de alimentação em `juizes.py:1531–1642`.

**Correções comportamentais obrigatórias**

1. **RESUMO item 8**: manter zero escritores; substituir os números antigos de consumidores. Antes C17 tinha 3 chamadas diretas; depois tem **1**, apenas o lado antigo do comparador em `chamados/management/commands/diff_e3_sombra.py:127`. C19 passou de 2 módulos consumidores externos para **6 módulos diretos**, com pauta indireta. Referências para texto: `chamados/services/tarefa_do_dia.py:638`, `chamados/management/commands/diff_e3_sombra.py:127`, `escala/services/cadastro_realidade.py:469`, `chamados/services/vigia_fio.py:649`, `folha/fora_da_folha.py:70`, `chamados/services/furo_sem_canal.py:97`, `chamados/management/commands/backfill_fio_morto_e4b.py:87`.

2. **“C17, porta alcancavel”** em Quem escreve/le:
   - Antes: “seus **3 consumidores diretos** sao cadastro/admin, vigia e folha.”
   - Depois sugerido: “seu **1 consumidor direto** e o lado antigo do comparador; cadastro/admin, vigia e folha passaram a ler a porta de C19.” (`chamados/juizes.py:1664`, `chamados/juizes.py:1695`, `chamados/management/commands/diff_e3_sombra.py:127`, `escala/services/cadastro_realidade.py:469`, `chamados/services/vigia_fio.py:649`, `folha/fora_da_folha.py:70`)
   - Commit da migração: **992be9c60**.

3. **“C19 tem 2 entradas externas”**:
   - Evitar confundir módulos consumidores com sítios. O wrapper `tarefa_do_dia(...)` continua tendo **2 chamadas diretas** externas: backfill `:87` e diff `:128`. Entretanto, os consumidores novos usam `carga.tarefa(...)` e `porta_da_tarefa(...)`; dizer apenas “2 entradas externas” omite os leitores agora em produção.
   - Texto sugerido: “**C19 tem 6 modulos consumidores diretos:** backfill avulso, diff em lote, cadastro/admin, vigia, folha e furo sem canal; a pauta dos discordantes chega indiretamente pela folha. Nao encontrei chamada de tela a essa porta nesta copia.”
   - Provas: `chamados/management/commands/backfill_fio_morto_e4b.py:87`; `chamados/management/commands/diff_e3_sombra.py:117`, `:128`, `:129`; `escala/services/cadastro_realidade.py:464`, `:469`; `chamados/services/vigia_fio.py:647`, `:648`, `:649`; `folha/fora_da_folha.py:70`, `:237`, `:238`; `chamados/services/furo_sem_canal.py:93`, `:96`, `:97`; pauta indireta `ponto/services/pautas_dos_discordantes.py:77`, `:80`.
   - `candidata = porta_da_tarefa` está em `diff_e3_sombra.py:43`; chamada em `:129`. São **6 sítios diretos da projeção**, contando esse alias e os dois sítios da folha.
   - Commit de migração/projeção: **992be9c60**; furo passou a consultar C19 em **ae9989de1**; inclusão de `nao_sei` na projeção em **3c55819f0**.

4. **L1, parágrafo Fio fora da contagem**:
   - Preservar distinção entre ação e verbo, provada por `chamados/tests/test_e3_portas_tarefa.py:85` (ação responder `:91`, verbo validar `:93`).
   - Atualizar “C17 e C19 sao 2 juizes de porta/tarefa, com consumidores e resultados distintos” para especificar que C17 permanece no comparador, enquanto os leitores de porta usam C19: `chamados/juizes.py:1664`, `chamados/services/tarefa_do_dia.py:574`, `:638`, `chamados/management/commands/diff_e3_sombra.py:127`.
   - O teste recebeu alteração interna em **b2dc7586a**; agora conserva porta de chamado residual (`test_e3_portas_tarefa.py:94–97`), mantendo a diferença ação/verbo.

5. **C19 — nova regra de porta/dono**: além da enumeração de decisões preservada em `tarefa_do_dia.py:131–176`, os leitores agora recebem:
   - união de portas das ações, chamados da célula, âncoras e chamados sem FK ligados ao dia forte (`:596–603`);
   - colaborador com responder e pergunta pendente no app (`:610–612`);
   - admin apenas pela fila (`:613`);
   - residual cobrar sem dono (`:618`);
   - precedência `colab`, `admin`, `chamado`, `sistema` (`:622`);
   - rota somente com dono; `porta_app` sem chamado quando colaborador (`:628–634`);
   - projeção `rota/fila/verbos/fio/nao_sei` (`:638–645`).
   - Commits: **cdf9ae7ee** união inicial; **3c55819f0** união/regras/`nao_sei`; **be856fb8b** dia forte; **52110c313** pendentes do app; **b4a87aecb** publicação somente com dono; **aa0e27c11** admin só na fila; **b1acb7985** e **1ee368261** carga do app em lote (`tarefa_do_dia.py:508–525`).

6. **Mesmo rio, sítios novos de alimentação**:
   - `juizes.py:1541` e `:1551`: faces SQL da mesma pergunta `respondida_sem_texto`; o conjunto de brancos segue `str.isspace()` (`:1536–1538`). `perguntas_respondidas_a_validar` usa a face com texto em `:1722`. Commit **0544c994d**.
   - `alimentar_fila` agora admite `colab_ids` (`juizes.py:1559`), preserva pertencimento à fila mesmo sem verbo (`:1573–1576`) e adiciona `fila_dia` pelo `dia_do_fato(..., fortes=True)` (`:1580–1589`). Commit principal **be856fb8b**.
   - Alimentação de C17 agora leva o mesmo `agora` para C6 (`juizes.py:1625`) e para consulta do app (`:1642`). Commit **ae9989de1**.

**Recontagens L1/L2/L3**

- Nenhuma mudança nos meus arquivos introduziu escritor persistente de Fio/tarefa; continua **0** (`tarefa_do_dia.py:8`, `:592`, `:655`).
- A mudança C17/C19 não acrescenta uma transição persistida à matriz: manter 30 linhas e contar as linhas existentes. (`tarefa_do_dia.py:592`, `:635`, `:645`)
- **Atenção L1**: a exceção #4 deixou de ter só guarda local: `responder_automaticamente` agora chama `recusa_respondida_sem_texto`, juiz externo, em `disputa_emissao.py:1653–1657`. #5 persiste por `save`, cujo tripwire novo também consulta o juiz externo em `models.py:3529` e `_recusa_par_ilegal`; pela definição ampla da lista (“composição de predicados em funções diferentes”), a recontagem indicada é **28→30**. Confirmar com os exploradores de modelos/serviços as linhas completas para a redação final.
- L2/L3 dependem principalmente dos arquivos dos outros exploradores; não houve nova escrita/terminalidade em C17/C19. (`tarefa_do_dia.py:592`, `:645`)

## 15/11 00:11 · sessao dcfb61b28f30 · - · - · origem=Ronald (tmux)

Nos serviços auditados, as contagens de nascentes permanecem; mudaram os juízes e a escrita da reabertura. (`chamados/services/disputa_emissao.py:1562`, `chamados/services/disputa_emissao.py:1634`, `chamados/services/disputa_emissao.py:1657`, `chamados/services/validacao.py:609`)

Correções de comportamento:

- **Matriz #4:** acrescentar a recusa de par com instante de resposta e texto vazio/espaços, por `recusa_respondida_sem_texto -> respondida_sem_texto`; levanta `RespondidaSemTexto` antes de modificar a instância. (`chamados/services/disputa_emissao.py:1657`, `chamados/services/fio.py:1595`, `chamados/services/fio.py:1602`)
- **Matriz #5:** a guarda local permanece; o `save` também consulta a recusa do par ilegal. Pelo critério original de composição de predicados entre funções, passa a integrar L1. (`chamados/services/disputa_emissao.py:1677`, `chamados/services/disputa_emissao.py:1683`, `chamados/models.py:3529`, `chamados/models.py:3559`, `chamados/models.py:3566`)
- **Matriz #10:** acrescentar recusa explícita de `fora_do_fato`, anterior à gravação do motivo, reabertura, nota e push. O reset agora também retira o carimbo pelo escritor do lote. (`chamados/services/validacao.py:609`, `chamados/services/validacao.py:625`, `chamados/services/validacao.py:628`, `chamados/services/disputa_emissao.py:1634`)
- **Matriz #11:** retirar `desfazer_absorcao`; o carimbo volta sempre junto com texto/resposta/validação. A operação passa exclusivamente por `descarimbar_em_lote`; a trilha exclui as vias fora do fato e inclui carimbadas sem texto quando algo é retirado. (`chamados/services/disputa_emissao.py:1562`, `chamados/services/disputa_emissao.py:1595`, `chamados/services/disputa_emissao.py:1617`, `chamados/services/disputa_emissao.py:1631`, `chamados/services/disputa_emissao.py:1634`)
- **L1:** **28 → 30**, com entrada de #4 e #5; ambas recebem o predicado compartilhado do par, ainda que #5 também mantenha guarda local redundante. (`chamados/services/disputa_emissao.py:1657`, `chamados/services/disputa_emissao.py:1683`, `chamados/models.py:3529`, `chamados/models.py:3566`, `chamados/services/fio.py:1602`)
- **Montagem do fio:** explicação usa universo por disputa e C7 completo; aviso/correção e indicação de resposta passam pelos leitores compartilhados de vitalidade/texto. (`chamados/services/fio.py:163`, `chamados/services/fio.py:177`, `chamados/services/fio.py:347`, `chamados/services/fio.py:357`, `chamados/services/fio.py:378`)

Commits identificados por `git log a0bf6e78e..4b75cba10 -- <arquivo>`:

| Mudança | Commit | Sítio novo | [nao verificado]
|---|---|---|
| Reabertura sempre pelo lote; parâmetro removido; recusa fora do fato | `f1fe1d367` | `chamados/services/disputa_emissao.py:1634`; `chamados/services/materializacao.py:688`; `chamados/services/validacao.py:609`; `chamados/services/acoes_disputa.py:85` |
| Guarda explícita da resposta automática e helper | `99ed67ff0` | `chamados/services/disputa_emissao.py:1657`; `chamados/services/fio.py:1595` |
| Helper usa sondador `SimpleNamespace` | `046d7c085` | `chamados/services/fio.py:1600` |
| Leitor por pergunta e indicador de resposta | `cb7064355` | `chamados/services/fio.py:177`; `chamados/services/fio.py:357` |
| Leitura do fio e agrupamento por disputa | `de125d5e1` | `chamados/services/fio.py:163`; `chamados/services/fio.py:240`; `chamados/services/fio.py:270` |
| Redução pelo juiz `fio_vivo` | `513ff8077` | `chamados/services/fio.py:174` |
| Explicador recebe universo completo para C7 | `ae66e55af` | `chamados/services/fio.py:347` |

Mapeamento completo de referências alteradas nos cinco serviços; referências não listadas conservam a linha:

```text
chamados/services/fio.py
216→246;224→254;235→265;446→484;609→647;670→708;
791→829;801→839;808→846;815→853;908→946;910→948;
922→960;926→964;927→965;928→966;934→972;
1019→1057;1029→1067;1033→1071;1036→1074;1038→1076;
1040→1078;1043→1081;1044→1082;1046→1084;1047→1085;
1075→1113;1079→1117;1226→1264;1341→1379;1342→1380;
1360→1398;1609→1660;2084→2135;2111→2162;2175→2226;2181→2232.

chamados/services/disputa_emissao.py
1593→1595;1603→1605;1615→1617;
1629/1630/1632→1634 [condicional/update substituídos pelo lote];
1638→1640;1647→1649;1652→1659;1655→1662;1656→1663;
1661→1668;1670→1677;1674→1681;1676→1683;1677→1684;1679→1686;
1960→1967;1966→1973;1970→1977;1979→1986;1986→1993;
1987→1994;2048→2055;2060→2067;2063→2070.

chamados/services/validacao.py
157→159;160→162;162→164;174→176;213→215;215→217;
222→224;225→227;228→230;244→246;311→313;312→314;
343→345;352→354;408→410;416→418;419→421;427→429;
435→437;458→460;484→486;487→489;492→494;
588→590;600→602;603→605;617→625;620→628;625→633;
656→664;681→689;685→693;689→697;691→699;693→701;694→702;773→781.

chamados/services/materializacao.py
688→688 [removeu desfazer_absorcao=True; chamada continua nesse sítio].

chamados/services/acoes_disputa.py
Referências citadas mantêm a linha; comportamento da casca mudou em :85,
incluindo fora_do_fato entre os resultados informativos.
```

## 15/11 00:10 · sessao 005a94fe3084 · CAMINHO-ATE-E4C-RELIDO · sonda · origem=Code

(a) A sonda chama a função real em `residuo_sem_linha.py:53` (anexo). A definição está em [app/chamados/models.py:4057](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/models.py:4057); os chamadores E4b e DRY estão, respectivamente, em [backfill_fio_morto_e4b.py:141](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/management/commands/backfill_fio_morto_e4b.py:141) e [backfill_decisoes_pergunta.py:67](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/management/commands/backfill_decisoes_pergunta.py:67).

O universo coincide com o filtro interno: `respondida_em` ou `validada_em` não nulos. O resíduo verifica ausência de correspondência por pergunta, ato e instante. `residuo_sem_linha.py:39` (anexo); [app/chamados/models.py:4067](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/models.py:4067), [app/chamados/models.py:4070](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/models.py:4070).

A partição chama a função real sobre `data_evento` e empresa: `True` → exportada; `False` → aberta; `None` → sem_data. A função compõe `janela_atual` e `competencia_entregue`. `residuo_sem_linha.py:56–57` (anexo); [vigia_fio.py:855](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/services/vigia_fio.py:855).

Divergências de escopo:

- O DRY acrescenta horizonte/conversa e separa conversas em balde próprio; a sonda não faz esses recortes. `residuo_sem_linha.py:39`, `residuo_sem_linha.py:56` (anexo); [backfill_decisoes_pergunta.py:59](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/management/commands/backfill_decisoes_pergunta.py:59), [backfill_decisoes_pergunta.py:73](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/management/commands/backfill_decisoes_pergunta.py:73).
- A E4b recebe os IDs selecionados como `passivo_sem_linha` pelo juiz do dia; compartilhar a função não implica compartilhar o universo global da sonda. [backfill_fio_morto_e4b.py:98](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/management/commands/backfill_fio_morto_e4b.py:98), [backfill_fio_morto_e4b.py:139](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/management/commands/backfill_fio_morto_e4b.py:139).
- `sem_data` é separado na sonda; o contador `carimbos_sem_lastro_por_dono` agrega esse caso em aberta. `residuo_sem_linha.py:57` (anexo); [vigia_fio.py:837](/home/ronald/codex_ws/4b75cba1018af37173b5da7aa15475c13468f559/app/chamados/services/vigia_fio.py:837).

(b) Contagens transcritas da saída fornecida; zeros inferidos pela ausência de entradas nos contadores. [nao verificado]

| Parte | Resposta | Validação | Linhas | Evidência |
|---|---:|---:|---:|---|
| exportada | 12248 | 11230 | 23478 | [nao verificado] |
| aberta | 4696 | 3885 | 8581 | [nao verificado] |
| sem_data | 0 | 0 | 0 | [nao verificado] |
| **Total** | **16944** | **15115** | **32059** | [nao verificado] |

| Empresa | Aberta | Exportada | Sem_data | Total | Evidência |
|---|---:|---:|---:|---:|---|
| emp1 | 89 | 0 | 0 | 89 | [nao verificado] |
| emp2 | 4431 | 19595 | 0 | 24026 | [nao verificado] |
| emp3 | 3229 | 2645 | 0 | 5874 | [nao verificado] |
| emp4 | 830 | 1238 | 0 | 2068 | [nao verificado] |
| emp29 | 2 | 0 | 0 | 2 | [nao verificado] |
| **Total** | **8581** | **23478** | **0** | **32059** | [nao verificado] |

- Soma das partes = total: **23478 + 8581 + 0 = 32059**; soma por empresa = **32059**; erros = **0**. [nao verificado]
- Universo: **17740 perguntas**. [nao verificado]
- Aberta: **4727 perguntas**; exemplos **#6,#6,#7,#7,#8,#8,#118,#118,#119,#119**. [nao verificado]
- Exportada: **12248 perguntas**; exemplos **#174,#174,#175,#175,#176,#176,#181,#181,#183,#183**. [nao verificado]

(c) A20: inclusão **[nao medido]**; contagem **[nao medido]**; **#pk [nao medido]**. A sonda não compara `data_conferida_em` com `respondida_em`/`validada_em`. `residuo_sem_linha.py:50–62` (anexo).

## 15/11 00:10 · sessao 625a04860cb6 · - · - · origem=Ronald (tmux)

| Fatia/sítio | Leitura após E3 e consequência para preparo | Commit | [nao verificado]
|---|---|---|
| F5 `fio.espera_o_texto_do_colaborador` | **SEGUE VALIDA**: existe; ainda lê `respondida_em` e `respondida_sem_texto`, portanto migração D01 continua pendente (`app/chamados/services/fio.py:177`, `:187`). | [nao verificado] |
| F5 `fio.processar_post_fio` | **SEGUE VALIDA**: guarda de regularização ainda pergunta `respondida_em is None`; migração T/D01 permanece (`app/chamados/services/fio.py:698`). | [nao verificado] |
| F5 `fio.canonica_do_grupo` / `merito_perdedor` | **SEGUE VALIDA**: texto ainda vem de `resposta_colab`; segunda função conserva gate de identidade `via_resolucao` (`app/chamados/services/fio.py:1271`, `:1282`, `:1283`). | [nao verificado] |
| F5 demais unidades de identidade do `fio` | **SEGUE VALIDA** a conservação A06: `reuso_sem_rechave` (`app/chamados/services/fio.py:1139`), `sem_dia_na_saida` (`:1206`), `retratar_duplicata` (`:1389`), `_no_fato` (`:1472`), `fecho_cruza_dias` (`:1905`), `perguntas_sobre_batida_retratada` (`:1941`), `universo_do_grupo` (`:1963`) e `grupos_de_colisao` (`:2008`). | [nao verificado] |
| F5 `explicador` | **SEGUE VALIDA**: `_explicar_validada` lê texto da projeção; `explicar_pergunta` também. Permanecem leituras de pendência, recusa e via (`app/chamados/services/explicador.py:32`, `:79`, `:80`, `:85`, `:90`, `:95`). | [nao verificado] |
| F5 `vigia_fio` | **SEGUE VALIDA** quanto à existência dos sítios: `censo_alcance` conserva leitura R (`app/chamados/services/vigia_fio.py:694`); `respondida_sem_resposta` filtra carimbo desde marco e conta textos vazios da projeção (`:804`, `:807`). A docstring deste último declara medir o par que a porta escreve; equivalência desse contador com contagem de atos vazios é **[nao verificado]** (`:798`). | [nao verificado] |
| F5 `raiox` | **SEGUE VALIDA**: crédito ainda usa C/R, mas a divisão já considera carimbo **e** texto; não deve perder essa distinção (`app/chamados/services/raiox.py:135`, `:138`, `:143`, `:144`). Clique morto conserva gate C (`:179`). | [nao verificado] |
| F5 `tarefa_do_dia` | **SEGUE VALIDA**, atualizando referências: A06 está em `app/chamados/services/tarefa_do_dia.py:245` e `:246`; matriz por decisão em `:131`–`:176`. **MUDA o detalhamento do contrato**: ações administrativas/colaborador carregam `porta_admin`, calculada pela âncora presente na fila (`:141`, `:142`, `:146`, `:151`, `:155`). | [nao verificado] |
| F5 `consulta_do_app` | **SOME o sítio do predicado**, não a função: ela permanece e delega à partição (`app/chamados/services/pergunta_no_app.py:64`, `:73`). A migração direta deve ocorrer na partição (`:81`). | [nao verificado] |
| F5 `particao_do_app` | **MUDA a formulação do RED**: saída literal é `pendentes`/`historico`, não `"respondidas"`; continua lendo carimbo R e fechamento F (`app/chamados/services/pergunta_no_app.py:81`, `:85`). | [nao verificado] |
| F5 `cobranca` | **MUDA a explicação da leitura indireta**: `_pendentes` chama `consulta_do_app` (`app/chamados/services/cobranca.py:47`, `:48`); `_calados` chama `espera_o_texto_do_colaborador` **e** P (`:227`, `:241`). Não é somente via `particao_do_app`. | [nao verificado] |
| F5 `furo_sem_canal.medir` | **SOME o sítio cru**, não a função: `medir` permanece (`app/chamados/services/furo_sem_canal.py:43`) e agora lê carga T/porta T (`:93`, `:96`, `:97`); sem canal é ausência de porta (`:104`). E3 já fez a delegação. | [nao verificado] |
| F6 `api_chamados_v2` | **SEGUE VALIDA** para texto/instante, ainda projeção (`app/api/views.py:1159`, `:1161`). **SOME a derivação crua da correção**: já chama `aguarda_o_colaborador` (`:1175`). | [nao verificado] |
| F6 `api_disputa_perguntas._item`, `api_disputa_perguntas`, `api_disputa_responder` | **SEGUE VALIDA**: estado da propriedade e `aguarda_admin = RESPONDIDA` (`app/api/views.py:2546`, `:2549`); texto/instante projeção (`:2557`, `:2559`); lista respondidas por carimbo (`:2573`); reenvio por carimbo (`:2616`); resposta de POST serve carimbo (`:2633`). | [nao verificado] |
| F6 `relatorios_disputa.classificar` | **SOME o predicado próprio**, não a função: permanece (`app/chamados/relatorios_disputa.py:11`); classificação já delega a C7 (`:24`) e `aguarda_o_colaborador` (`:26`). Certidão explica os dois predicados substituídos (`:15`–`:22`). | [nao verificado] |
| F7 `_card_atendimento.html` | **SEGUE VALIDA** migração da apresentação R: texto e instante permanecem (`app/templates/chamados/partials/_card_atendimento.html:106`, `:114`, `:141`, `:143`, `:148`, `:154`, `:182`, `:183`, `:391`). **SOME o julgamento cru de envio**: template lê `resposta_enviada` (`:100`), preparada por `fio` via `espera_o_texto_do_colaborador` (`app/chamados/services/fio.py:357`). Atualizar referência da mistura pendência/via/recusa para template `:189`–`:192`. | [nao verificado] |
| F7 suposta catraca E17 em `test_e3_pendentes_app.py:118` | **MUDA**: nessa linha está `CatracaC17Test`; teste vigente fixa um chamador C17 (`app/chamados/tests/test_e3_pendentes_app.py:118`, `:145`–`:148`), cujo permitido é `diff_e3_sombra.comparar` (`:21`). Não sustenta o fecho estrutural de templates que o preparo atribui a esse sítio. Local de eventual catraca E17: **[nao verificado]**. | [nao verificado] |

Sem histórico Git nesta cópia, o conjunto efetivo de arquivos mudados entre os dois commits e os commits que mudaram cada sítio são **[nao verificado]**. [nao verificado]

## 15/11 00:10 · sessao 1caa01de2959 · - · - · origem=Ronald (tmux)

Releitura F1–F4, **condicional ao cruzamento histórico**, que não pode ser confirmado sem Git nesta cópia; commit exato por sítio **[nao verificado]**. O pouso documental da E3 pode ser referido como `90f3fa66b`, conforme indicação do agente principal em `app/docs/RELATO.md:15`; atribuição individual **[nao verificado]**.

| Fatia / arquivo | Classificação atual e evidência |
|---|---|
| F1 — `app/chamados/juizes.py` | **SEGUE VALIDA** a migração dos 16 sítios: todos continuam lendo projeção diretamente, por `getattr`, ou por face SQL: `_retratada_com_trilha` :288; `_slot_esperado` :348/:360; `_pergunta_materializada_de_fato` :679/:707; `_chave_memo_pv` :852; `_resposta_concorda_com_a_luz` :1356; `projecao_temporal_da_pergunta` :1385; `pergunta_sem_lastro_ou_viva` :1496; `respondida_sem_texto` :1528; faces SQL :1548/:1556; `perguntas_corrigiveis` :1715; fila :1722/:1724; `declarou_hora` :1750; `delta_da_resposta` :1897; `classificar_resposta_validacao` :2001/:2006; `motivo_implausivel_validacao` :2023. |
| F1 — faces SQL / fila | **MUDA a referência anterior ao pouso**: `q_respondida_sem_texto` existe em :1541 e `q_respondida_com_texto` em :1551; suas expressões ainda são sobre R, :1548/:1556. A fila já chama a face com texto, :1722, e conserva ordenação pela projeção em :1724. Logo E3 fez a centralização/trim SQL; não fez a migração para DP. Não retirar essas unidades da E4c. | [nao verificado]
| F2 — `app/chamados/models.py`, leitores | **SEGUE VALIDA**: `_gravar_estado` lê V no filtro :310; `_ancoradas` :344; universo da ressuscitação C/R/VIA :1531; ressuscitação captura R/V :1611; `sem_veredito_entre` R/V/C :1906; `fechar` VIA/C/R :1984/:1989; `remover_motivo` F/R/VIA :2097/:2122/:2124; `estado` :3124; `fechar_por_admin` :3138; `q_vivas` :3455/:3468; `respondida` :3641; `resposta_display` :3664. |
| F2 — FE | **SEGUE VALIDA, preservando o predicado existente**: `respostas_sem_veredito` delega ao corpo único em :1889; o corpo exige V e C ausentes e **carimbo OU texto não vazio**, :1906–1909. `pode_fechar` combina ausência dessa lista com ausência de fio vivo, :1917. O comentário distingue expressamente esse julgamento do próximo ato do fio, :1900–1902. Migrar a fonte não autoriza trocar OR por AND. |
| F2 — A05/A06 | **SEGUE VALIDA a permanência** dos sítios: `do_fato` :2802; `q_dos_fatos` :2833; `carimbar` :3209; `carimbar_identidade` :3265; `fora_do_fato` :3284; gate de `descarimbar_em_lote` :3364; `encerrar_por_fato_no_espelho` :3372; `encerrar_fantasma` :3383; `reabrir_fantasma` :3395; `reabrir_por_resposta` :3412; `reabrir_por_competencia` :3419; índices de `Meta` :3628/:3634. |
| F2 — guardas E3 | **SEGUE VALIDA a exclusão da migração E4c** para `descarimbar_em_lote` :3343–3355; `save` chama a guarda em :3529 e atualiza o par carregado em :3534; `_par_lido` :3553/:3557; `_recusa_par_ilegal` :3559–3566. O comentário explicita que se impede nova transição ilegal, preservando o passivo, :3536–3540. |
| F2 — `_nascer` | **MUDA a descrição “nasceu na E3” para sítio já presente**: função :2903; recusa do par R ilegal antes da trava, :2919–2923. Essa leitura específica é guarda de entrada do escritor; a proposta D01/D03 não deve absorver inadvertidamente `campos.get('respondida_em')`/`campos.get('resposta_colab')` como leitura de apresentação. A guarda é efetiva em :2922, não trabalho pendente de E4c. |
| F2 — conservação | **SEGUE VALIDA** a porta `DecisaoPergunta.linhas_da_projecao_sem_linha`, :4057; ela lê a projeção para conservar RESPOSTA/VALIDACAO em :4067–4081. Não substituir esse caso por “sem ato”. |
| F3 — `app/chamados/services/validacao.py` | **SEGUE VALIDA** a migração de `_carimbar_validacao` :164/:166; `_foto_da_validacao` :172; `devolver_validacao_recusada` :213/:215/:220/:232; `_e_aceite_de_folga` :261; `_aceitar_a_folga` :370; `validar_pergunta` :418/:421/:486/:513; `recusar_pergunta` :602/:605/:615; lote :767/:788; `classe_do_lote` :886; fila :959. **SEGUE VALIDA** a exceção A20 em `conferir_data_do_fato`, :697. |
| F3 — recusa | **MUDA a descrição anterior ao pouso**: a porta já recusa `fora_do_fato` antes de escrever, :609–614, e chama a reabertura canônica com `ato='recusa'`, :628–629. Isso já foi feito; continua pendente migrar as guardas R/V em :602/:605 para a fonte prevista. | [nao verificado]
| F3 — `app/chamados/services/acoes_disputa.py` | **SEGUE VALIDA** a migração de `acao_reabrir_disputa` :95; `acao_regularizar_disputa` :130; `acao_corrigir_resposta` :345/:353. **SEGUE VALIDA a exclusão** do homônimo `recusa_motivo`: é `request.POST.get`, :81. **MUDA** o retorno documentado da casca: `fora_do_fato` já é tratado como informação, :85–86. |
| F4 — `app/chamados/services/disputa_emissao.py` | **SEGUE VALIDA** para `_derivar_chamado_destino` :228; `motivo_fora_da_fabrica` :762; órfãos :1070/:1090; âncora :1326; `responder_pergunta_disputa_core` :1470; `reabrir_para_correcao` :1621; automático :1649; trilha :1677/:1685; recobrança :1720; credor :1828/:1846; resolver :1986/:2047. **SEGUE VALIDA** a exceção A06 de `gerar_perguntas_disputa`, :328/:400/:419/:437. |
| F4 — reabertura | **MUDA o trabalho pendente**: E3 já unificou os dois ramos na porta do lote, :1634; a trilha já exclui linhas fora do fato, :1631; a lápide do parâmetro `desfazer_absorcao` está em :1592–1593. Não propor novamente essa unificação. A captura histórica da projeção continua em :1621–1623, e deve ser distinguida de leitura do estado corrente. |
| F4 — cura no automático | **MUDA de “reconferir depois da cura” para guarda já implementada**: `responder_automaticamente` chama `recusa_respondida_sem_texto` antes da alteração da instância, :1653–1657. Ainda lê R para proteger resposta existente, :1649; a migração dessa leitura permanece pendente. |
| F4 — `app/chamados/services/materializacao.py` | **SEGUE VALIDA** para `_tipo_pelo_marco` :280/:308; `_ts_tipo_do_plantio` :359/:405/:412; `estado_aplicacao_pergunta` :438/:440; `_declarar_ausencia_de_pergunta` :474; `_materializar_ausencia_de_resposta` :539/:571; `perguntas_absorvidas_por` :631–638; devolução :674–677; hora :835/:876/:977; data/hora :1001/:1010/:1062; seleção de validadas :1375–1379; pendentes aprovadas :1424–1425. **SEGUE VALIDA** a permanência de `_carimbar_recusa` A18/A06 :1095–1099 e `_materializar_uma` A20 :1199. |

Não encontrei unidade F1–F4 eliminada que justifique **SOME** integral nesses seis arquivos; a afirmação negativa decorre da presença das unidades acima. O que já saiu é o ramo alternativo/parâmetro da reabertura, cuja lápide está em `app/chamados/services/disputa_emissao.py:1592`. Não há prova de que a E3 tenha concluído a leitura pelo ato nesses sítios: R/V/C continuam nos locais citados.

## 15/11 00:10 · sessao bcc5a8d79bb7 · - · - · origem=Ronald (tmux)

L1 deve passar de **28 para 30** pelo criterio original de composicao de predicados em funcoes diferentes: #4 consulta explicitamente o juiz comum; #5 tambem passa pelo novo gate de `save`. (`chamados/services/disputa_emissao.py:1657`, `chamados/services/disputa_emissao.py:1677`, `chamados/services/disputa_emissao.py:1683`, `chamados/models.py:3529`, `chamados/models.py:3559-3566`)

L2 continua **1 escritor persistente declarado fora de `reapontar_resolvedora`**: o reuso em `_nascer`. L3 continua **0 dos 4 rotulos sem saida**, com o mesmo subuniverso de **2 vias terminais quanto ao retorno ao fato**. (`chamados/models.py:3065`, `chamados/models.py:3089`, `chamados/models.py:2694`, `chamados/models.py:3286-3295`, `chamados/models.py:1613`, `chamados/models.py:2471`, `chamados/models.py:3263`, `chamados/models.py:3363`)

Nenhuma referencia antiga citada de `models.py` perdeu seu sitio: as linhas citadas conservam o texto, com os deslocamentos enviados. As novas guardas relevantes sao:

- Nascimento recusa `respondida_em` sem texto, inclusive reuso/costura, antes da trava: commit `99ed67ff0`. (`chamados/models.py:2919-2923`)
- Lote verifica o par texto/instante resultante dos `outros`: commit `cb7064355`. (`chamados/models.py:3343-3355`)
- `save` recusa nascimento ou alteracao para o par ilegal; permite salvar outros campos do par preexistente intacto: commit `cb7064355`. (`chamados/models.py:3529`, `chamados/models.py:3559-3566`)

Deslocamentos dos sitios citados:

| Faixa antiga | Deslocamento |
|---|---:|
| ate 1872 | 0 |
| 1909–2371 | +8 |
| 2372–2896 | +22 |
| 2897–3312 | +27 |
| 3313–3388 | +43 |
| 3913 | 4000 |

A correspondencia detalhada e a atribuicao por commits foram enviadas anteriormente; a leitura de `git log --first-parent` por `models.py` identifica o pouso `90f3fa66b`, enquanto o log completo por caminho identifica os commits de implementacao acima. (`chamados/models.py:2919-2923`, `chamados/models.py:3340-3355`, `chamados/models.py:3529-3566`)

## 15/11 00:10 · sessao ed4db7c21f73 · - · - · origem=Ronald (tmux)

Nenhuma linha das seções 1, tabela N1/N2, L1/L2/L3 e 4 precisa de correção: `ChamadoColaborador` permanece nas linhas 27–1756; o primeiro hunk de `models.py` começa em 1868, em `DisputaSupervisao`. Os demais arquivos citados nessas seções não têm diff entre os hashes. (`chamados/models.py:27`, `chamados/models.py:1757`, `chamados/models.py:1873`)

Recontagem:

- **L1: 2 → 2.** As cadeias de decisão dos dois destinos permanecem iguais. (`chamados/models.py:1098`, `chamados/models.py:1150`, `chamados/models.py:1173`, `chamados/catalogo/motor.py:130`, `chamados/catalogo/motor.py:133`)
- **L2: 4 históricos + 0 runtime → 4 históricos + 0 runtime.** A varredura AST em memória encontrou zero violações; escritores persistentes e temporários continuam nos mesmos sítios. (`chamados/tests/test_selo_status_so_pelas_portas.py:340`, `chamados/models.py:228`, `chamados/models.py:296`, `chamados/models.py:356`, `chamados/models.py:371`, `chamados/models.py:1018`, `chamados/models.py:1142`, `chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)
- **L3: 0 incluindo migrations + 2 terminais runtime → 0 + 2.** Permanecem `cancelado` e `superado`, com saídas históricas pelas migrations. (`chamados/catalogo/motor.py:103`, `chamados/catalogo/motor.py:106`, `chamados/catalogo/motor.py:142`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

Não há alteração dessas seções a registrar em “MUDOU COM A E3”. (`chamados/models.py:880`, `chamados/models.py:1101`)

## 15/11 00:10 · sessao ed0f087cb41a · - · - · origem=Ronald (tmux)

| Item | C17 | C19 |
|---|---|---|
| Pergunta literal do registro | `por onde alguem chega a este dia (porta alcancavel, com o verbo do painel)?` — `app/core/juizes.py:138`. | `quais acoes ainda sao exigiveis para cada instancia de marco do dia, de quem e por quais portas?` — `app/core/juizes.py:159`. |
| Função | `porta_alcancavel_do_dia(celula_id, *, alimentacao=None)` — `app/chamados/juizes.py:1664`; registrada em `app/core/juizes.py:297`. | `tarefa_do_dia(colab, dia_do_turno, agora, *, carga=None)` — `app/chamados/services/tarefa_do_dia.py:653`; registrada em `app/core/juizes.py:299`. |
| Pergunta literal da docstring | `Por onde alguem CHEGA a este dia?` — `app/chamados/juizes.py:1665`. | `para o colaborador, o dia do turno e cada INSTANCIA de marco, quais acoes ainda sao exigiveis, de quem e por qual porta.` — `app/chamados/services/tarefa_do_dia.py:654`. |
| Universo | Uma célula: chamados da célula na fila; perguntas vivas C6 da célula, presentes no app, com âncora na fila e sem o par respondida/texto vazio — `app/chamados/juizes.py:1667`, `app/chamados/juizes.py:1699`, `app/chamados/juizes.py:1703`. | Colaborador × dia do turno × instante `agora`, por instância de marco; perguntas `conversa=False` no intervalo, atos ordenados por `pergunta_id, ato_em, pk` e limitados a `ato_em <= agora` — `app/chamados/services/tarefa_do_dia.py:653`, `app/chamados/services/tarefa_do_dia.py:107`, `app/chamados/services/tarefa_do_dia.py:395`, `app/chamados/services/tarefa_do_dia.py:410`. |
| Predicado/resultado | Existência de porta por chamado da célula na fila **ou** pergunta elegível sob âncora na fila. Sem ambos: `None`; com porta: rota, fila visível, verbos do painel e fio pela escada dos verbos — `app/chamados/juizes.py:1696`, `app/chamados/juizes.py:1703`, `app/chamados/juizes.py:1705`, `app/chamados/juizes.py:1709`, `app/chamados/juizes.py:1655`. | Exigibilidade por instância, considerando vínculo/isencão/tranca, lâmpada, teto, ausência e última decisão/efeito; devolve razões, ações com responsável/porta e desconhecimento. Pode haver ação exigível com `porta=None` — `app/chamados/services/tarefa_do_dia.py:200`, `app/chamados/services/tarefa_do_dia.py:210`, `app/chamados/services/tarefa_do_dia.py:277`, `app/chamados/services/tarefa_do_dia.py:295`, `app/chamados/services/tarefa_do_dia.py:298`, `app/chamados/services/tarefa_do_dia.py:323`, `app/chamados/services/tarefa_do_dia.py:142`. |

Testes que fixam C17: [nao verificado]

- Fila sem pergunta: `assertEqual({'rota': 'porta_fila', 'fila': (a.pk,), 'verbos': {a.pk: 'decidir'}, 'fio': 'chamado'}, self._juiz())` — `app/chamados/tests/test_o261e_f5b_juiz_porta.py:159`, asserção `:161`.
- Pergunta respondida sob âncora na fila: `assertEqual({'rota': 'porta_pergunta', 'fila': (), 'verbos': {b.pk: 'validar'}, 'fio': 'admin'}, self._juiz())` — `app/chamados/tests/test_o261e_f5b_juiz_porta.py:193`, asserção `:196`.
- Chamado `registrado`: `assertIsNone(self._juiz())` — `app/chamados/tests/test_o261e_f5b_juiz_porta.py:200`, asserção `:203`.
- Pergunta respondida com âncora arquivada: `assertIsNone(self._juiz(), ...)` — `app/chamados/tests/test_o261e_f5b_juiz_porta.py:261`, asserção `:275`.
- Escada dos verbos: `validar+cobrar → colab`, `validar+decidir → admin`, `decidir → chamado`, `None → chamado` — `app/chamados/tests/test_o261e_f5b_juiz_porta.py:300`, asserções `:304`, `:305`, `:306`, `:307`.
- Registro: `assertIn('chamados/juizes.py::porta_alcancavel_do_dia', set(juizes.JUIZES['chamado'].values()))` — `app/chamados/tests/test_o261e_f5b_juiz_porta.py:320`, asserção `:322`.

Testes que fixam C19: [nao verificado]

- Instâncias independentes: fixa `inicio/aceso`, `int_ida/sem_pergunta`, `int_volta/aceso`, `fim/sem_pergunta`; as duas apagadas exigem `perguntar/sistema` — `app/chamados/tests/test_e2_tarefa_do_dia.py:151`, asserções `:154`, `:156`, `:157`.
- Responder uma instância não fecha outra: fixa `int_ida → validar/admin` e `fim → responder/colaborador` — `app/chamados/tests/test_e2_tarefa_do_dia.py:159`, asserções `:165`, `:167`.
- Teto por marco: fixa `fim → ('nao_venceu', [])` — `app/chamados/tests/test_e2_tarefa_do_dia.py:170`, asserção `:174`.
- DEVOLUCAO: fixa `('materializacao_recusada', [('validar', 'admin')])` — `app/chamados/tests/test_e2_tarefa_do_dia.py:222`, asserção `:229`.
- Efeito/lâmpada: fixa `validada_sem_efeito → materializar/sistema`, `efeito_sem_lampada → rejulgar/sistema`, lavra vencida → `('veredito_sem_lavra_vigente', [], True)` — `app/chamados/tests/test_e2_tarefa_do_dia.py:293`, asserções `:301`, `:309`, `:314`.
- Ação e alcance são separáveis: fixa `responder/colaborador/porta=ch.pk` e, após silenciamento, **a mesma ação** com `porta=None` — `app/chamados/tests/test_e2_tarefa_do_dia.py:396`, asserções `:405`, `:408`.
- Ordem dos atos: VALIDACAO às 19:50 vence RESPOSTA criada depois, mas retrodatada às 19:10; fixa `validada_sem_efeito/materializar/sistema` — `app/chamados/tests/test_e2_tarefa_do_dia.py:425`, asserção `:432`.
- Passivo: fixa `('passivo_sem_linha', [], True)` — `app/chamados/tests/test_e2_tarefa_do_dia.py:488`, asserção `:495`.

**Conclusão: duas — C17 julga alcance da porta; C19 julga exigibilidade por instância, inclusive ação com porta ausente.** `app/chamados/juizes.py:1703`; `app/chamados/services/tarefa_do_dia.py:323`; `app/chamados/tests/test_e2_tarefa_do_dia.py:408`.

A documentação registra a migração dos leitores para `porta_da_tarefa` e a permanência de C17 no instrumento de comparação; isso não afirma igualdade dos dois predicados. `app/docs/JUIZES.md:85`; `app/core/juizes.py:135`.

## 15/11 00:10 · sessao 3a93ea19d374 · - · - · origem=Ronald (tmux)

Nenhum trecho das seções 1, portas/juízes/persistência/trilha comum ou 3 exige correção por alteração dos sítios citados. Os arquivos respectivos não têm diff no intervalo; `ChamadoColaborador` permanece igual até o fim da classe. (`chamados/models.py:128`, `chamados/models.py:201`, `chamados/models.py:642`, `chamados/models.py:820`, `chamados/models.py:1652`, `chamados/models.py:1669`)

Recontagem:

- **L1: 12 → 12 pares**, sendo 11 runtime e 1 histórico. As combinações continuam H=8, R=3, E=4, T=3, C=3, total 21; os filtros históricos permanecem iguais. (`chamados/catalogo/motor.py:99`, `chamados/catalogo/motor.py:111`, `chamados/catalogo/motor.py:114`, `chamados/catalogo/motor.py:116`, `chamados/catalogo/motor.py:153`, `chamados/models.py:1677`, `chamados/migrations/0039_a3_via_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:23`)
- **L2: 4 → 4 sítios persistentes de estado em migrações; 0 → 0 encontrados no runtime.** Nenhuma escrita adicional desses campos aparece nas adições runtime do diff; nascimento e núcleo permanecem iguais. (`chamados/migrations/0038_a3_registrado_backfill.py:10`, `chamados/migrations/0038_a3_registrado_backfill.py:15`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`, `chamados/models.py:296`, `chamados/models.py:1018`, `chamados/models.py:1142`)
- **L3: 2 → 2 no runtime; 0 → 0 incluindo migrações e reversas.** `cancelado` e `superado` continuam terminais absolutos no motor, ligados somente pela ida/reversa histórica. (`chamados/catalogo/motor.py:104`, `chamados/catalogo/motor.py:106`, `chamados/catalogo/motor.py:142`, `chamados/migrations/0039_a3_via_backfill.py:24`, `chamados/migrations/0039_a3_via_backfill.py:33`)

A mudança em `q_vivas` não modifica a descrição do gate de perguntas ancoradas: `_ancoradas` busca diretamente perguntas não validadas, sem `q_vivas`; `_vivas_entre` julga cada pergunta por `pergunta_viva`. O predicado de respostas sem veredito mantém o corpo anterior, embora ganhe comentários e alimentação opcional por `perguntas=`. (`chamados/models.py:344`, `chamados/models.py:368`, `chamados/models.py:1889`, `chamados/models.py:1905`, `chamados/models.py:3454`)

Metadado da seção 4: comparei, em memória, os hashes Git dos **4.352 blobs** de `4b75cba10` com a cópia atual: **0 ausentes e 0 divergentes**. A correspondência foi verificada por comando, sem arquivo de evidência; citação `arquivo:linha` desse resultado: **[nao verificado]**. A frase antiga sobre ausência de Git descreve a consulta anterior; a cópia atual não contém `.git`, mas o histórico está disponível em `/home/ronald/saas-hasner`. **[nao verificado]**

_5427 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
