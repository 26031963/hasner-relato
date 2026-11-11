# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 22:10** · publicado em **10/11/2026 22:12** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 22:10 · sessao 093c3c32dff7 · - · - · origem=Ronald (tmux)

Travas e efeitos:

- Núcleo genérico prende C antes de avaliar `donas`, depois D e Ch; conserva essas travas fora do savepoint desfeito e durante o replay. A implementação de `trava_na_ordem` adquire C/D/Ch/P por essa sequência. `app/chamados/services/disputa_emissao.py:623`, `:652`, `:701`; `app/chamados/models.py:2665`, `:2675`, `:2688`, `:2692`.
- Órfãos adquire o fecho antes de escrever; relê sob C e recusa crescimento. O atomic externo engloba ensaio, replay e análise posterior. `app/chamados/services/disputa_emissao.py:1156`, `:1166`, `:1173`, `:1175`, `:1216`, `:1223`, `:1450`.
- Declaração pelos dois callers conserva atomic externo e pré-trava do fato antes do novo savepoint. `_trava_do_ato` inclui donas abertas/fechadas, chamados e perguntas do fato. `app/chamados/services/declaracao_texto.py:288`, `:299`, `:301`, `:348`, `:352`, `:643`, `:645`; `app/chamados/services/disputa_emissao.py:763`.
- **Lacuna em pendentes da varredura:** o wrapper termina o atomic do núcleo antes de chamar `perguntar_entradas_pendentes`; esse helper agora agrega/salva DS depois da porta de P, sem atomic/trava próprios. Sem atomic externo, a pergunta pode estar commitada antes da escrita de motivos/contexto, e a DS é salva sem conservar C/D. Com atomic externo, as travas do núcleo continuam cobrindo DS do colaborador. `app/chamados/services/disputa_emissao.py:623`, `:650`, `:817`, `:830`, `:310`, `:313`, `:575`. API chama diretamente: `app/api/views.py:794`; cron: `app/ponto/management/commands/marcar_foto_ausente_retro.py:94`.

Replay:

- Os kwargs conservam dia/texto/destino/opções, mas **não congelam todo o fato na família BATIDA**: o coletor omite `batida_origem_id`, e `_nascer` deriva novamente o episódio do contexto da disputa recebida. Trocar a local pela dona pode mudar episódio e, pelo juiz, dia/canônica. `app/chamados/services/disputa_emissao.py:500`, `:503`, `:710`; `app/chamados/models.py:2866`, `:2875`, `:3452`, `:3463`.
- `criada=True` ou pk diferente é desfeito no núcleo/órfãos e levanta na declaração. Antes dessa guarda, insert/save e seus signals já executaram. `app/chamados/services/disputa_emissao.py:710`, `:711`, `:1453`, `:1454`, `:781`, `:782`; `app/chamados/models.py:3006`, `:3017`.
- Os motivos usados nos caminhos de declaração/órfãos são MARCO; o derivador de episódio devolve `None` para essa família. O risco de rederivação de episódio acima se aplica ao replay genérico BATIDA. `app/chamados/catalogo/motivos.py:34`, `:35`, `:48`, `:49`; `app/chamados/models.py:3461`.

Rollback e push: [nao verificado]

- Signals de P agendam beco com `on_commit` e escrevem outbox síncrono no banco. Não são, por si, envio externo de push. `app/chamados/signals.py:227`, `:238`, `:250`, `:257`.
- `_enviar_push_disputa_colab` é envio imediato, webpush e FCM; não usa `on_commit`. `app/chamados/services/disputa_emissao.py:1947`, `:1963`, `:1971`; `app/chamados/push_utils.py:40`; `app/chamados/fcm_utils.py:85`.
- Nos blocos explicitamente desfeitos por estrangeira, não encontrei chamada direta de `_enviar_push_disputa_colab` antes de `set_rollback`. Núcleo envia no wrapper; órfãos envia após sair do savepoint; declaração não envia diretamente. `app/chamados/services/disputa_emissao.py:681`, `:838`, `:1444`, `:1461`, `:1465`, `:1486`, `:758`.
- Há cascata síncrona real: `Chamado.abrir` → `get_or_create` → signal do cartório → possível emissor de ausência → push de supervisão e repasse. `app/chamados/models.py:971`; `app/chamados/signals.py:188`, `:214`, `:216`; `app/ponto/services/cartorio.py:1751`, `:1789`; `app/ponto/management/commands/detectar_ausencias.py:778`, `:802`. **Não reproduzi cenário completo admissível que prove push escapando especificamente do rollback por estrangeira [nao verificado].**
- Push de complementos ocorre ainda dentro do atomic externo e antes de `pode_fechar`; falha posterior pode desfazer banco após esse envio imediato. Não há teste/reprodução desse caso nesta auditoria. `app/chamados/services/disputa_emissao.py:1216`, `:1230`, `:1465`, `:1473` [nao verificado quanto à ocorrência].

Escritores/leitores e semântica:

| Campo | Escritores identificados | Leitores relevantes |
|---|---|---|
| DS.motivos | agregador; creates do núcleo/órfãos; **âncora mantém união/save próprios**; portas de remoção/aparo/reabertura admin | admissão/push; foto recebida; fechamento/materialização/listagem | [nao verificado]
| DS.contexto | agregador; creates; portas de anotar/renovar/aparar/remover | resolução dia/episódio, auditor I7a/b, derivação de destino |
| DS.fechada_em | `fechar`, `reabrir_admin`, `reabrir_sistema` | eleição da aberta, guardas agregador/push, três receivers de fechamento | [nao verificado]
| P.disputa | construtor `_nascer`; reuso devolve `ex` preservando FK | dona do agregado; eleição local/estrangeira; fallback de listagem |
| P.chamado_destino | `_nascer` e `fio.reapontar_resolvedora`; há costura em `ChamadoColaborador` | âncora que lista, juiz de resolvedora, vida do fio, S143 |

Referências da tabela: `app/chamados/services/disputa_emissao.py:558`, `:670`, `:1299`, `:1531`, `:1554`, `:1562`; `app/chamados/models.py:1961`, `:2063`, `:2100`, `:2118`, `:2149`, `:2155`, `:2173`, `:1932`, `:2982`, `:3008`, `:3014`, `:367`; `app/chamados/services/fio.py:925`, `:116`; `app/api/foto_recebida.py:63`; `app/chamados/signals.py:28`, `:129`, `:153`.

**A frase “agregador único” não vale literalmente nem dentro do emissor:** a âncora continua com o segundo predicado de união e o segundo save; o gate declara duas escritas dessa função. `app/chamados/services/disputa_emissao.py:1531`, `:1562`; `app/chamados/tests/test_porta_disputa_emissao.py:83`.

DS.motivos continua documentado como “cenários ativos”, enquanto o núcleo passa a admitir motivos somente com pergunta local; órfãos ainda conserva seed `orfao_14h`, e contexto pode guardar informação de motivo encaminhado para outra dona. Há diferença semântica entre essas populações; impacto completo em todos os leitores **[nao verificado]**. `app/chamados/models.py:1741`; `app/chamados/services/disputa_emissao.py:598`, `:638`, `:633`, `:1275`.

## 10/11 22:09 · sessao 5fbc4ce8791f · - · - · origem=Ronald (tmux)

Achados concretos:

1. **Motivo pode terminar nas duas disputas abertas.** No repasse, `motivos_atualizados` recebe `['orfao_14h']` também quando A já existe com `motivos=[]`; esse seed é salvo antes de perguntar. Se a pergunta `orfao_14h` pertence a B, o helper devolve `motivos_antes`, que já contém o seed, e agrega o motivo em B. Um motivo local de intervalo mantém A com pergunta, impedindo desfazer A. Resultado: A tem `intervalo_saida` e `orfao_14h`, B tem `orfao_14h`, mas a pergunta desse motivo continua só em B. Referências: `app/chamados/services/disputa_emissao.py:1274`, `:1307`, `:980`, `:1002`, `:1007`, `:1313`, `:1349`, `:1429`, `:1444`. Confirmei em sonda inline `python3 -B`, usando as funções produtivas extraídas pelo ambiente de memória; persistência ORM desse cenário [nao verificado].

2. **Replay conserva kwargs, mas não todos os insumos implícitos da identidade.** O gerador conserva motivo/texto/opções/destino/dia, omitindo `batida_origem_id`; a porta deriva essa origem do contexto da DS fornecida. No replay, a DS passa a ser a dona, cujo contexto pode conter outro episódio. Referências: `app/chamados/services/disputa_emissao.py:500`, `:504`, `:710`; `app/chamados/models.py:2866`, `:3452`, `:3463`. O resultado pode divergir: as guardas verificam `criada` ou PK diferente e desfazem o banco: emissor `:711`, órfãos `:1454`, declaração `:782`. Reprodução ORM com família BATIDA [nao verificado].

3. **Replay divergente dos órfãos ainda analisa os chamados.** Depois de desfazer o replay divergente, o agrupador executa toda a lista `analisar`, independentemente de qual replay teve sucesso. Referências: `app/chamados/services/disputa_emissao.py:1454`, `:1456`, `:1459`. A declaração levanta `ValueError`; o núcleo registra `replay-divergente`, portanto as três respostas diferem: `:783`, `:715`. Consequências completas do cartório nessa divergência [nao verificado].

4. **O agregador não é o único escritor em todo o módulo.** A âncora continua criando DS com motivos/contexto e salvando diretamente `disputa.motivos`; também ignora o retorno de `abrir_conversa`. Referências: `app/chamados/services/disputa_emissao.py:1554`, `:1562`, `:1567`. São sítios preservados no diff integral fornecido; atribuir essa duplicidade especificamente ao delta v11 seria [nao verificado].

5. **Pendentes do wrapper ficam fora do atomic próprio do núcleo.** O núcleo retorna dentro de seu atomic; o wrapper chama pendentes depois. O helper abre P pelo atomic da porta, depois salva D no agregador sem atomic próprio. Referências: `app/chamados/services/disputa_emissao.py:623`, `:650`, `:699`, `:830`, `:831`, `:310`, `:313`, `:575`; `app/chamados/models.py:2819`. Garantia de trava externa em todos os consumidores [nao verificado].

Mapa de push: [nao verificado]

- Wrapper: nascimento usa admitidos; agregação usa novos de A; verifica DS aberta e, se pendentes chamou o gerador, verifica novamente. Referências: `app/chamados/services/disputa_emissao.py:699`, `:647`, `:817`, `:827`, `:832`, `:835`.
- Admissões em B não produzem push próprio: `agregar_estrangeiras` descarta a lista de novos do agregador; helper de órfãos também não coleta B para notificação. Referências: `app/chamados/services/disputa_emissao.py:565`, `:576`, `:613`, `:1002`. Se isso viola o aval versus apenas expõe um caso não especificado: [nao verificado].
- Órfãos: principal recebe `motivos_atualizados`; complementadas recebem união `_mot`; evita repetir a principal no coletor. Referências: `app/chamados/services/disputa_emissao.py:1257`, `:1258`, `:1228`, `:1465`, `:1484`.
- Ensaio de órfãos desfeito não chega ao push da principal; só notifica complementos anteriores ao savepoint. Referências: `app/chamados/services/disputa_emissao.py:1278`, `:1447`, `:1461`, `:1462`.
- Declaração não chama push nesse helper; âncora chama uma vez. Referências: `app/chamados/services/disputa_emissao.py:758`, `:785`, `:1577`.
- O envio efetivo é síncrono, Web Push e FCM, sem `on_commit` nessa função. Referências: `app/chamados/services/disputa_emissao.py:1963`, `:1971`. Entrega externa ou duplicidade efetiva [nao verificado].

Retornos:

- Chamadas datadas do gerador, replay do núcleo, helper de órfãos e replay dos órfãos consomem P e criada; a primeira tentativa da declaração usa P e descarta criada. Referências: `app/chamados/services/disputa_emissao.py:502`, `:523`, `:710`, `:990`, `:1008`, `:1453`, `:773`.
- `abrir_conversa` do gerador consome ambos; âncora descarta ambos. Referências: `app/chamados/services/disputa_emissao.py:495`, `:523`, `:1567`.
- Os dois consumidores produtivos externos da declaração passam `pergunta`; a forma sem pergunta aparece internamente e em testes. Referências: `app/chamados/services/declaracao_texto.py:362`, `:650`; `app/chamados/services/disputa_emissao.py:764`; `app/chamados/tests/test_porta_disputa_emissao.py:175`.
- API `disputa_para_declaracao(pergunta=None)` ainda pode criar DS sem P; isso é exercitado intencionalmente por teste. Referências: `app/chamados/services/disputa_emissao.py:747`, `:754`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:405`.
- H2 sem estrangeira mantém fecho vazio; com estrangeira e sem P local desfaz o nascimento. Referências: `app/chamados/services/disputa_emissao.py:1444`, `:1473`, `:1479`.

Lacunas de testes pertinentes: [nao verificado]

- O lote misto existente usa estrangeira `intervalo_saida`, não `orfao_14h`, portanto não prova remoção do seed. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:22`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:209`.
- O double de identidade compara apenas motivo e dia, sem episódio; não prova a estabilidade implícita do replay BATIDA. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:109`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:101`.
- Nenhuma alteração de arquivo foi necessária para as sondas inline; efeitos ORM, PostgreSQL, banco/log de produção e suite inteira [nao verificado]. [nao verificado]

## 10/11 22:08 · sessao cabc2df259bf · - · - · origem=Ronald (tmux)

Achados prioritários:

- **Selo quebrado:** `nasce_sem_pergunta` e `replay_insere` varrem o arquivo inteiro, mas exigem exatamente uma alteração. Hoje encontram respectivamente cinco e dois sítios; abortam por montagem antes de demonstrar assertion RED. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:66`, `app/chamados/tests/test_bo_disputa_volta5_memoria.py:81`; sítios em `app/chamados/services/disputa_emissao.py:682`, `:776`, `:778`, `:1446`, `:1448` e `:711`, `:1454`.
- **Paridade sobre a própria versão:** o teste anuncia gerador BASE versus atual, mas instancia `AmbienteVolta3()` nas duas repetições; a comparação final demonstra repetibilidade da fonte atual. As assertions de dia esperado continuam úteis. `app/chamados/tests/test_bo_disputa_volta3_memoria.py:236`, `:245`, `:246`, `:289`, `:292`, `:300`.
- **5e testa o assert, não a integração:** chama diretamente `reabrir_sistema`, com `save`/`refresh_from_db` simulados; os mutantes removem assertions do teste. Não certifica que `analisar()`/cartório efetivamente reabre a dona. `app/chamados/tests/test_bo_disputa_volta5e_memoria.py:30`, `:46`, `:49`, `:59`, `:70`, `:74`.
- **Declaração sem cobertura do ramo candidata/costura:** `_candidata_reusavel` sempre retorna `None`, trava é `Mock` e `PerguntaDisputa.abrir` é double. A cobertura não prova `disputa_emissao.py:765-771`, ordem real de travas nem efeitos dos receivers. `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:77`, `:89`, `:95`, `:97`.
- **Divergência nova sem selo específico:** os testes 5c demonstram replay bem sucedido e rollback inicial, mas não forçam pergunta criada/PK diferente no replay de declaração ou órfãos; o teste adversarial existente exerce o replay do núcleo. `app/chamados/tests/test_bo_disputa_volta5_memoria.py:194`; `app/chamados/tests/test_bo_disputa_volta5c_memoria.py:118`, `:192`; portas `app/chamados/services/disputa_emissao.py:781`, `:1453`.
- **Possível vacuidade no ORM “depois da pergunta”:** chama o gerador e levanta sem afirmar que alguma pergunta nasceu antes. A irmã memória contém essa assertion positiva. `app/chamados/tests/test_bo_disputa_sem_pergunta.py:58`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:301`.

Cobertura útil existente:

| Família | O que morde | Limite |
|---|---|---|
| Sem pergunta | Exceção antes/depois da escrita, preservação da batida, separação I7a/I7b e pergunta morta como linha. `test_bo_disputa_sem_pergunta_memoria.py:286`, `:296`, `:329`, `:355`, `:376` | Atomic/ORM/signals simulados. `test_bo_disputa_sem_pergunta_memoria.py:50` |
| Volta2 | Consulta lazy após C, dona fechada antes de Ch e ausência de trava repetida. `test_bo_disputa_volta2_memoria.py:143` | Conta eventos dos doubles; não SQL/concorrência PostgreSQL. `test_bo_disputa_volta2_memoria.py:1` |
| Volta3/4 | Ordem inicial, dias mistos/indeterminados, ciclos sem reentrada e argumentos conservados. `test_bo_disputa_volta3_memoria.py:156`; `test_bo_disputa_volta4_memoria.py:20`, `:45`, `:106` | Porta de pergunta simulada. `test_bo_disputa_volta4_memoria.py:31`, `:63` |
| Volta5 | Motivo somente na dona, rollback por divergência, idempotência, push e AST sem reentrada. `test_bo_disputa_volta5_memoria.py:121`, `:194`, `:270`, `:334` | Dois mutantes quebrados acima; cenário de dona aberta escondida é artificial e explicitamente declarado. `test_bo_disputa_volta5_memoria.py:233` | [nao verificado]
| 5b | Reuso estrangeiro, push por critério base, sem ata, h2 desfeito, criadas reais/pendentes. `test_bo_disputa_volta5b_orfaos_memoria.py:152`, `:185`, `:223`, `:239`, `:258` | Double de `analisar()` só muda status; não executa cartório. `test_bo_disputa_volta5b_orfaos_memoria.py:120` | [nao verificado]
| 5c | Dois callers da declaração, saídas normais/recusa, rollback, pendentes estrangeiros, agregador e critérios opostos de push. `test_bo_disputa_volta5c_memoria.py:118`, `:149`, `:176`, `:237`, `:252`, `:262` | Limites da declaração acima. | [nao verificado]
| Gate | Censo real por função confrontado em igualdade exata com `ATOS_EMISSAO`. `test_porta_disputa_emissao.py:105`, `:118`, `:128`; reutilizado em `test_bo_disputa_volta5c_memoria.py:283` | Conta sítios/escritores; não prova semântica de motivos/contexto. `test_porta_disputa_emissao.py:122` |
| 5e | Assert rejeita dona fechada mesmo com nota antiga e reabertura sem nota. `test_bo_disputa_volta5e_memoria.py:50`, `:84`, `:88` | Não prova integração. |

Os protegidos contêm os asserts solicitados: sem ata continua sem pergunta/aberta; com ata completa fecha; muda não notifica; pergunta antiga aponta ao dia correto; duas passadas conservam uma pergunta. `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:63`; `app/chamados/tests/test_hx_disputa_muda.py:24`, `:44`; `app/chamados/tests/test_pergunta_no_app.py:89`, `:103`. Permanecerem **historicamente intocados**: [nao verificado], pois não há `.git`/base disponível.

Executei previamente 82 testes de memória em nove módulos, todos verdes, com `PYTHONDONTWRITEBYTECODE=1 python3 -B`; não executei ORM nem suite inteira. Evidência em arquivo/log da execução: [nao verificado]. Após receber a regra ` .claude/agents/verificador.md:7`, limitei os próximos comandos a leitura/AST; não executei os mutantes.

`opus5d.txt`, `preverif3.r`, `matriz2.r` e briefs não foram encontrados na cópia; conteúdo/achados individualizados: [nao verificado].

## 10/11 22:03 · sessao 38fa1f071ca0 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Arquivos tocados, somente testes: [nao verificado]

- [test_bo_disputa_volta5b_orfaos.py:102](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:102): E1/E2 exigem dona reaberta e nota quando houver chamado; preservam costura, push e ausência de nascimento local. [nao verificado]
- [test_bo_disputa_volta5e_memoria.py:1](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5e_memoria.py:1): prova sem banco, RED e mutantes dos novos asserts.

E3: não encontrei outro ORM com essa exigência depois de `analisar`. As expectativas restantes cobrem emissão/replay ([volta5.py:170](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:170), [sem_pergunta.py:83](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_sem_pergunta.py:83)).

Execução local: controle **4/4**, dois RED com **rc 1**, dois mutantes mortos por assertion; comandos do [runner:98](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5e_memoria.py:98):

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --red sem_reabertura
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --red sem_nota
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --mutante ignora_reabertura
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5e_memoria.py --mutante ignora_nota
```

**Motivo não chega à dona reaberta: sim, por leitura do código.** O agregador retorna `None` enquanto ela está fechada ([disputa_emissao.py:562](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:562)), antes de `analisar` ([disputa_emissao.py:1458](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1458)); a reabertura não agrega motivos ([models.py:2173](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/models.py:2173)).

Execução ORM/cartório e certificação pela suite/opus: [nao verificado]. [nao verificado]

perguntas de lei: 0

## 10/11 21:54 · sessao bae3df84aecf · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

D1/D2: cortei a reentrada de `analisar()` no cartório quando há pergunta ligada a dona fechada. [Emissor:1190](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:1190)

D3: âncora única, mantendo `assertRaisesRegex`. [Teste:211](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_e1_fila_v2_2_integracao.py:211) [nao verificado]

Resultados:

- Base original: **3 assertions RED**, zero errors. [Registro:56](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_red_base.txt:56)
- Memória: **85 + 35 testes verdes**. [Execuções:3230](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_execucoes.txt:3230), [medida do turno:49](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_turno.txt:49) [nao verificado]
- **35 mutantes** com controle GREEN e assertion RED, zero errors. [Registro:3230](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5d_execucoes.txt:3230)

Arquivos tocados, limites e comandos individuais estão no [relato da entrega](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/RELATO_BO_DISPUTA_VOLTA5D.txt:1). ORM/mod11 e certificação Code/opus final: [nao verificado].

perguntas de lei: 0

## 10/11 21:40 · sessao f71fee8a366a · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

C1–C6 implementados. Os arquivos tocados, as duas linhas `lei:` de C7 e os limites estão no [relato](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/RELATO_BO_DISPUTA_VOLTA5C.txt:1).

78 testes em memória e 35 da catraca verdes; 31 mutantes com RED por assertion, sem erros. [Comandos e resultados](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/docs/bo_disputa_volta5c_execucoes.txt:3). [nao verificado]

ORM, banco, produção e suite completa: [nao verificado]. Certificação reservada ao Code/suite e opus final. [nao verificado]

perguntas de lei: 0

## 10/11 21:33 · sessao 5c7f63d0e5be · FIO-E0-V4-E4B · censo · origem=Code

**Não identifiquei edição compartilhada obrigatória para a E4b; há dependências de leitura cujo comportamento precisa ser reconferido após BO-DISPUTA**, especialmente a fábrica chamando emissor e gerador. (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`)

Identidade Git do HEAD/base e atribuição cronológica dos hunks: **[nao verificado]**. O `diff_obra_v10.patch` não consta do texto recebido nem foi localizado na cópia; a interseção exaustiva contra esse patch é **[nao verificado]**. As referências abaixo são da cópia atual; “compatível” significa que o corpo sustenta a descrição fornecida, sem certificar identidade textual com a base **[nao verificado]**.

**1. CENSO do brief e 26 portas**

| Escritor/leitor do brief | Referência atual | Delta relevante |
|---|---|---|
| Comando novo E4b | **[nao verificado]** | Ainda não há implementação conferível do comando solicitado. |
| `DecisaoPergunta.registrar` | `app/chamados/models.py:3943` | **Brief correto**; censo antigo apontava `:3932`. Faz `bulk_create` e acorda células; não contém deduplicação própria. (`app/chamados/models.py:3953`, `app/chamados/models.py:3954`) |
| `desfazer_carimbo_sem_lastro` | `app/chamados/services/relavra_via.py:187` | Endereço continua válido; porta a chamar. |
| `perguntar_pela_celula` | `app/chamados/services/fabrica_da_celula.py:178` | **`:171` não é mais a definição**. Pode retornar por sem adesão, pauta de cadastro ou limbo exportado; chamada não prova pergunta criada. (`app/chamados/services/fabrica_da_celula.py:183`, `app/chamados/services/fabrica_da_celula.py:204`, `app/chamados/services/fabrica_da_celula.py:215`) |
| Materialização, citada sem função | `app/chamados/services/materializacao.py:1158` | Unidade canônica; ausência em `:462`; Batida delegada em `:927` e `:1007`. |
| Reconciliação, citada sem função | `app/chamados/reconciliador.py:761`; `app/chamados/reconciliador.py:1533` | Reemissão continua em `:761`; **reatar órfã passou de `:1529` para `:1533`**. |
| `censo` | `app/chamados/services/vigia_fio.py:318` | Endereço válido. **`limite=0` não significa ilimitado**: listas só recebem enquanto `len < limite`. (`app/chamados/services/vigia_fio.py:454`, `app/chamados/services/vigia_fio.py:475`) |
| `censo_alcance` | `app/chamados/services/vigia_fio.py:542` | Endereço válido. Todo `sem_porta` entra; demais linhas continuam limitadas. Para inventário integral, usar limite suficientemente grande, não zero. (`app/chamados/services/vigia_fio.py:628`) |
| `classificar`, mencionado como parte do vigia | `app/chamados/services/vigia_carimbo.py:65` | É gerador do universo de **perguntas validadas**, importado pelo vigia; não é o classificador a–e. As classes a–e permanecem em `censo_alcance`. (`app/chamados/services/vigia_fio.py:328`, `app/chamados/services/vigia_fio.py:667`) |
| `vigiar_fio_morto.handle` | `app/chamados/management/commands/vigiar_fio_morto.py:152` | **`:151 → :152`**. Corpo agora lê `contador_fio_mudo`, transportando também `nunca_bateu_sem_adesao` e seus esperados. (`app/chamados/management/commands/vigiar_fio_morto.py:181`, `app/chamados/management/commands/vigiar_fio_morto.py:233`) |
| `montar_contexto_painel` | `app/chamados/services/painel.py:198` | Endereço válido; contagens continuam vindo de `particionar_verbos` sobre o universo vivo filtrado. (`app/chamados/services/painel.py:230`) |
| `exige_o_admin` | `app/chamados/services/verbos.py:25` | Endereço válido; soma Validar + Decidir. (`app/chamados/services/verbos.py:22`, `app/chamados/services/verbos.py:27`) |
| Leitor do carimbo | `bin/fio_estado.py:98` | Continua lendo JSON e comparando **todos os esperados transportados**; portanto também alcança os novos contadores, sem edição necessária da E4b. (`bin/fio_estado.py:125`, `bin/fio_estado.py:147`) |

As vinte portas de alteração:

| Porta | Referência do censo → atual | Semântica frente à descrição fornecida |
|---|---|---|
| `_gravar_estado` | `app/chamados/models.py:201 → :201` | Compatível: estado canônico do chamado e consequências. |
| `fechar` | `app/chamados/models.py:1852 → :1855` | Compatível: F; exceções registram `SILENCIO`. (`app/chamados/models.py:1924`) |
| `reabrir_admin` | `app/chamados/models.py:2123 → :2126` | Compatível: F, contador e motivos. |
| `reabrir_sistema` | `app/chamados/models.py:2160 → :2163` | Compatível: limpeza de F. |
| `carimbar` | `app/chamados/models.py:3092 → :3103` | Compatível: C, assinaturas R/V condicionais e L opcional. |
| `carimbar_identidade` | `app/chamados/models.py:3157 → :3168` | Compatível: identidade; preservação opcional de instante/autor. |
| `descarimbar` | `app/chamados/models.py:3192 → :3203` | Compatível: limpeza C; memória quando `salvar=False`. |
| `descarimbar_em_lote` | `app/chamados/models.py:3241 → :3252` | Compatível: C e campos de `outros`, com donos travados. |
| `encerrar_por_fato_no_espelho` | `app/chamados/models.py:3257 → :3268` | Compatível: troca de via, sem fabricar efeito. |
| `_carimbar_validacao` | `app/chamados/services/validacao.py:127 → :127` | Compatível: V e DP `VALIDACAO`. |
| `devolver_validacao_recusada` | `app/chamados/services/validacao.py:163 → :163` | Compatível: limpa V, conserva resposta, DP `DEVOLUCAO`. |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1422 → :1425` | Compatível: R, invalidação V/C, DP `RESPOSTA`. |
| `reabrir_para_correcao` | `app/chamados/services/disputa_emissao.py:1559 → :1562` | Compatível: R/V/C e DP; continua sem escrever F. |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1635 → :1638` | Compatível: conserva texto existente; instante recebido. |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1658 → :1661` | Compatível: exige testemunha; instante histórico em R/DP. |
| `relavrar` | `app/chamados/services/relavra_via.py:106 → :106` | Compatível: via derivada e L. |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:784 → :784` | Compatível: pendência/recusa; efeitos delegados. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1083 → :1083` | Compatível: recusa técnica; não demonstra DP `RECUSA` humana. |
| `recusar_pergunta` | `app/chamados/services/validacao.py:533 → :533` | Compatível: recusa humana pela porta de correção. |
| `conferir_data_do_fato` | `app/chamados/services/validacao.py:625 → :625` | Compatível: sinalizadores e auditoria dos dois dias; sem DP próprio de conferência. |

Os seis nascimentos:

| Porta | Referência do censo → atual | Delta |
|---|---|---|
| `ChamadoColaborador.abrir` | `app/chamados/models.py:833 → :833` | Descrição compatível. |
| `ChamadoColaborador.criar` | `app/chamados/models.py:1054 → :1054` | Descrição compatível. |
| `_abrir_ou_agregar_disputa` | `app/chamados/services/disputa_emissao.py:474 → :474` | Continua podendo retornar DS antes da geração feita pelo wrapper. (`app/chamados/services/disputa_emissao.py:603`, `app/chamados/services/disputa_emissao.py:670`) |
| `criar_disputa_a_partir_de_chamados_orfaos` | `app/chamados/services/disputa_emissao.py:1024 → :1027` | Descrição de nascimento/contexto compatível. |
| `criar_disputa_ancora_de_chamado` | `app/chamados/services/disputa_emissao.py:1299 → :1302` | Descrição de âncora/nascimento compatível. |
| `PerguntaDisputa._nascer` | `app/chamados/models.py:2822 → :2825` | **Censo incompleto:** nascimento com assinaturas R/V também registra os atos correspondentes no mesmo atomic. Não reconstruir esses atos novamente. (`app/chamados/models.py:3018`, `app/chamados/models.py:3019`) |

Distinções semânticas que precisam acompanhar os endereços corrigidos:

| Distinção | Resultado atual e consequência |
|---|---|
| **D3 — “tem resposta”** | Continua diferente entre timestamp (`app/chamados/models.py:3497`), SQL com texto literal não vazio (`app/chamados/models.py:3348`), C6 com `.strip()` (`app/chamados/juizes.py:1341`) e sequência/passivo sem linha (`app/chamados/services/tarefa_do_dia.py:127`, `app/chamados/services/tarefa_do_dia.py:132`). Nenhum desses critérios substitui globalmente os demais. |
| **D4 — validação × efeito** | V ainda mata a vivacidade C6; a tarefa, pela DP `VALIDACAO`, exige lavra vigente e distingue materializar, rejulgar e encerrar. (`app/chamados/juizes.py:1325`, `app/chamados/services/tarefa_do_dia.py:140`) |
| **D5/D6 — dia × instante** | Continua fato=dia do turno e conversa=dia civil; plantio combina hora e cadeia, incluindo tentativa no dia seguinte e envelope inclusivo. (`app/chamados/juizes.py:332`, `app/chamados/juizes.py:324`, `app/chamados/juizes.py:326`) |
| **D9 — relógio** | **Atualizar a formulação absoluta do censo:** C6 já aceita `hoje`, inclui-o no memo e propaga-o; `date.today()` permanece fallback. Nem todo chamador injeta esse dia. (`app/chamados/juizes.py:577`, `app/chamados/juizes.py:839`, `app/chamados/juizes.py:852`, `app/chamados/services/vigia_fio.py:813`) |
| **Dia do carimbo** | `hoje` do carimbo é o dia civil local da medição, não o dia do turno de cada pergunta. A dívida de interpretação está registrada. (`app/chamados/management/commands/vigiar_fio_morto.py:167`, `app/chamados/management/commands/vigiar_fio_morto.py:230`, `app/docs/BACKLOG.md:1899`) |
| **Legado e executor** | `linhas_da_projecao_sem_linha` passou de `models.py:3903` para `app/chamados/models.py:3914`; correspondência segue pergunta+ato+instante. `PerguntaDisputa.save` está em `app/chamados/models.py:3413`; imutabilidade DP em `app/chamados/models.py:3903`. (`app/chamados/models.py:3927`, `app/chamados/models.py:3931`, `app/chamados/models.py:3935`) |
| **Exportada × indeterminada** | O juiz de entrega é `competencia_entregue`; a composição por dia está em `competencia_do_dia_entregue`. **Não copiar o balde de medida:** o vigia põe dia indeterminado em “aberta”; isso não autoriza APPLY. (`app/folha/export.py:556`, `app/chamados/services/vigia_fio.py:822`, `app/chamados/services/vigia_fio.py:831`) |

**2. Cruzamentos com BO-DISPUTA**

Aqui, **escrita significa edição do arquivo-fonte**. Chamar uma porta que produz efeitos no banco é dependência de leitura/uso, não edição daquele arquivo. A lista exata do patch permanece **[nao verificado]**; os cruzamentos abaixo seguem os arquivos de cura indicados nas matrizes fornecidas **[nao verificado quanto aos hunks futuros]**.

| Arquivo | E4b precisa fazer | Comportamento pressuposto que a BO pode mudar |
|---|---|---|
| `app/chamados/services/fabrica_da_celula.py:178` | **Ler/chamar; nenhuma edição obrigatória identificada.** | Hoje chama wrapper e depois gerador novamente. E4b não pode depender de duas tentativas nem tratar motivos retornados como perguntas efetivamente criadas. (`app/chamados/services/fabrica_da_celula.py:172`, `app/chamados/services/fabrica_da_celula.py:174`, `app/chamados/services/fabrica_da_celula.py:220`) |
| `app/chamados/services/disputa_emissao.py:474` | **Leitura transitiva obrigatória** pela fábrica. | Retorno de DS não prova pergunta própria: núcleo retorna antes da geração do wrapper. A cura de nascimento/dona precisa preservar os insumos efetivos do fato. (`app/chamados/services/disputa_emissao.py:603`, `app/chamados/services/disputa_emissao.py:670`, `app/chamados/services/disputa_emissao.py:682`) |
| `app/chamados/services/disputa_emissao.py:1027`; `app/chamados/services/disputa_emissao.py:1302` | **Leitura/chamada conforme o destino apurado**, sem edição demonstrada. | Não usar “DS retornada” como prova de nascimento local; conferir pergunta, dona e efeito após a operação. O nascimento pode preceder a pergunta. (`app/chamados/services/disputa_emissao.py:1120`, `app/chamados/services/disputa_emissao.py:1358`) |
| `app/chamados/services/disputa_emissao.py:1661` | **Leitura/chamada histórica, se houver testemunha.** | Reconstruir instante testemunhado; não usar resposta nova para inventar história. (`app/chamados/services/disputa_emissao.py:1670`, `app/chamados/services/disputa_emissao.py:1674`, `app/chamados/services/disputa_emissao.py:1677`) |
| `app/chamados/services/declaracao_texto.py:86` | **Leitura transitiva** de marcos/ata; não há necessidade demonstrada de chamar os consumidores humanos. | `_candidata_reusavel` não resolve dona global: pode recusar candidata estrangeira e o consumidor abrir depois da DS. Preservar dia/ata e conferir dona real. (`app/chamados/services/declaracao_texto.py:250`, `app/chamados/services/declaracao_texto.py:254`, `app/chamados/services/declaracao_texto.py:360`, `app/chamados/services/declaracao_texto.py:376`) |
| `app/ponto/management/commands/reconciliar_grade.py:412` | **Nenhuma leitura operacional, chamada ou edição exigida pelo contrato E4b.** | Seu helper passa `motivos=[]`; não serve como substituto da fábrica para criar pergunta faltante. (`app/ponto/management/commands/reconciliar_grade.py:424`, `app/ponto/management/commands/reconciliar_grade.py:425`) |
| `app/chamados/management/commands/smoke_chamados.py:82` | **Nenhuma chamada ou edição exigida.** | O precedente chama núcleo e gerador separadamente; não copiar esse mecanismo para o backfill. (`app/chamados/management/commands/smoke_chamados.py:99`, `app/chamados/management/commands/smoke_chamados.py:102`) |
| `app/chamados/management/commands/auditar_invariantes_chamados.py:14` | **Nenhuma edição exigida; diagnóstico eventual.** | O helper futuro das matrizes não é uma porta atual conferida para E4b **[nao verificado]**. O inventário contratado continua no vigia. (`app/chamados/services/vigia_fio.py:318`, `app/chamados/services/vigia_fio.py:542`) |

Dependências comuns que as matrizes também exigem preservar:

| Arquivo | Cruzamento e consequência |
|---|---|
| `app/chamados/models.py:3943` | **Ler/chamar**, mantendo seleção sob trava; `registrar` não deduplica o lote por pergunta+ato+instante. (`app/chamados/models.py:3953`) |
| `app/chamados/services/fio.py:1784`; `app/chamados/services/fio.py:1225` | **Ler identidade/canônica** quando necessário; eleição considera texto normalizado e PK, não apenas timestamp de resposta. (`app/chamados/services/fio.py:1231`, `app/chamados/services/fio.py:1233`) |
| `app/chamados/reconciliador.py:1533` | **Ler/chamar religação**; alcança derivador do emissor e possível reemissão. (`app/chamados/reconciliador.py:1556`, `app/chamados/reconciliador.py:1621`) |
| `app/chamados/services/materializacao.py:1158` | **Ler/chamar efeito canônico**; Batida continua por `registrar_batida`, com `fato_de_chao=False`. (`app/chamados/services/materializacao.py:927`, `app/chamados/services/materializacao.py:930`) |
| `app/chamados/signals.py:42`; `app/chamados/signals.py:144`; `app/chamados/signals.py:163` | **Ler efeitos transitivos**: pai, regularizações no commit e materialização síncrona. Save de motivos/contexto em DS fechada não é presumidamente inócuo. |

**DRY precisa conservar o silêncio de push:** rollback do banco não desfaz envio imediato. A fábrica já silencia o caminho; o wrapper envia diretamente. Callbacks de commit e efeitos síncronos também precisam ser distinguidos na conferência. (`app/chamados/services/fabrica_da_celula.py:218`, `app/chamados/services/disputa_emissao.py:671`, `app/chamados/services/disputa_emissao.py:687`, `app/chamados/signals.py:144`, `app/chamados/signals.py:163`)

Assim, **nenhuma edição compartilhada é necessária demonstrada pelo escopo do brief**; compatibilidade após a mudança da BO e interseção completa com o patch ausente permanecem **[nao verificado]**.

**3. Linhas do mapa/fila que alcançam o universo ou as portas**

O trecho recebido de `mapa.r` termina truncado durante LOTE-VALIDAR; conteúdo posterior é **[nao verificado]**. Os endereços conferíveis abaixo são do BACKLOG atual.

| Linha/item | Muda o que E4b faz? |
|---|---|
| **BO-0611-07-E4B-COBRE** — `app/docs/BACKLOG.md:1893` | **Sim, na entrega do DRY:** demonstrar se cobre o item 07; se não cobre, registrar residual O261. A linha não demonstra cobertura. |
| **BUG-PAR-FROTA-1110** — `app/docs/BACKLOG.md:1882` | Cruza validação/materialização; **não acrescenta cura conjunta ao backfill**. A aplicação atual ainda é individual. (`app/chamados/services/validacao.py:463`, `app/chamados/services/materializacao.py:1376`) |
| **DISPUTA-ZUMBI-1110** — `app/docs/BACKLOG.md:1886` | Exige distinguir carimbo, efeito e fechamento; não autoriza E4b a implementar fechamento por conta própria. |
| **PERGUNTA-DUPLICATA-1110** — `app/docs/BACKLOG.md:1887` | Pode afetar seleção/contagem; não autoriza acrescentar deduplicação: cura registrada depois da E4d. |
| **BUG-PLANTIO-NOTURNO-MAIS-1** — `app/docs/BACKLOG.md:1891` | Preservar dia do turno/instante; não incorporar migração de dia ao backfill. Autoridades atuais continuam em `app/chamados/juizes.py:332` e `app/chamados/juizes.py:296`. |
| **LOTE-VALIDAR-REGRESSAO / JOB** — `app/docs/BACKLOG.md:1892`; `app/docs/BACKLOG.md:1831` | Cruza validação, mas não muda a decisão nem autoriza criar job nesta obra; regressão ainda pede medida da causa. |
| **BO-0611-05-RESTO-DA-CLASSE** — `app/docs/BACKLOG.md:1889` | Não afrouxar dedup/colisão para “curar” passivo. As guardas continuam na porta. (`app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:132`, `app/ponto/registro_batida.py:150`) |
| **BO-0611-05-TEM-GPS-LEITORES** — `app/docs/BACKLOG.md:1890` | Não inventar coordenada nem presença GPS. **A pergunta antiga do mapa foi respondida:** zero e par parcial significam sem GPS. (`app/docs/CORTES.md:442`, `app/ponto/services/geofence.py:126`) |
| **BO-ROTULO-ENCERRADO** — `app/docs/BACKLOG.md:1884` | Não muda o efeito do backfill; cura expressamente de apresentação. |
| **SONDA3-METODO-CORRECAO** — `app/docs/BACKLOG.md:1885` | Não tratar transição aceita com superusuário como prova de ação do supervisor ou regularização do fato. |
| **BO-DISPUTA / matriz / RAIZ** — `app/docs/BACKLOG.md:1877`; `app/docs/BACKLOG.md:1878`; `app/docs/BACKLOG.md:1883` | Dependência das portas; não incorporar cura do agregado nem mexer nas sete por shell. |
| **CARIMBO-VELHO / CIVIL-X-TURNO** — `app/docs/BACKLOG.md:1898`; `app/docs/BACKLOG.md:1899` | Ausência de campo novo não prova zero; dia da medição não vira dia do turno. |
| **E4A1-C6-X-PASSADO-SEM-TURNO** — `app/docs/BACKLOG.md:1902` | Chamar fábrica não garante atravessar descarimbo: existe retorno antecipado para pauta. (`app/chamados/services/fabrica_da_celula.py:201`, `app/chamados/services/fabrica_da_celula.py:205`) |
| **E4A2-RELOGIO-FIO-VIVO / O169-JUIZES-760** — `app/docs/BACKLOG.md:1903`; `app/docs/BACKLOG.md:1905` | Não presumir relógio integralmente injetado nem destino encerrado como prova suficiente de efeito. |

O mapa **não redefine a–e nem permite classe→dono/cura fixa**: o classificador continua consultando célula, vivacidade, âncora e lista real do admin. (`app/chamados/services/vigia_fio.py:661`, `app/chamados/services/vigia_fio.py:667`, `app/chamados/services/vigia_fio.py:673`, `app/chamados/services/vigia_fio.py:675`, `app/chamados/services/vigia_fio.py:677`)

**4. NÃO TOCAR**

| Cerca | Delta recomendado para o brief |
|---|---|
| `disparar_perguntas_competencia.py` | **Manter.** Envelheceu apenas a justificativa “obra em curso”: agora é vigia, com `handle :145`; o reparo indicado é semear eventos para consumo posterior. (`app/chamados/management/commands/disparar_perguntas_competencia.py:1`, `app/chamados/management/commands/disparar_perguntas_competencia.py:11`, `app/chamados/management/commands/disparar_perguntas_competencia.py:145`) |
| `config/crons.py` e `vigiar_fio_morto.py` | **Manter.** Cron do disparar já está sem apply; carimbo já transporta os novos contadores. E4b DRY não precisa reeditar nenhum deles. (`app/config/crons.py:1521`, `app/chamados/management/commands/vigiar_fio_morto.py:233`) |
| Fábrica, materialização, reconciliador, relavra e modelos | Tornar explícito na lista final o **“só chamar” já existente no CENSO**; especialmente fábrica compartilhada com a cura BO. (`app/chamados/services/fabrica_da

[truncado em 20000 de 21988 caracteres]

## 10/11 21:31 · sessao c9f8f6b73718 · - · - · origem=Ronald (tmux)

Item 3 — linhas do mapa/fila que cruzam E4b:

| Item | Referência atual | Consequência para o backfill |
|---|---|---|
| **BO-0611-07-E4B-COBRE** | `app/docs/BACKLOG.md:1893` | Acrescenta obrigação explícita de **conferir pelo DRY** se item 07 é coberto; se não, residual O261. A linha não prova cobertura nem permite anunciar cura. (`app/docs/BACKLOG.md:1893`) |
| **BUG-PAR-FROTA-1110 / BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS** | `app/docs/BACKLOG.md:1882`; `app/docs/BACKLOG.md:1929` | Cruza as portas de validação/materialização, mas não autoriza E4b a aplicar metades isoladas nem implementar o novo par. A porta atual continua materializando unidade em `validacao.py:463` e lote em `materializacao.py:1376`; a cura conjunta permanece registrada. (`app/chamados/services/validacao.py:463`; `app/chamados/services/materializacao.py:1376`; `app/docs/BACKLOG.md:1929`) |
| **DISPUTA-ZUMBI-1110** | `app/docs/BACKLOG.md:1886` | Afeta interpretação de carimbo e fechamento; E4b precisa distinguir efeito e continente. Não transformar `pode_fechar` em autorização para fechar por conta própria. A linha manda separar premissa por sonda e declara carimbadores sem C7. (`app/docs/BACKLOG.md:1886`) |
| **PERGUNTA-DUPLICATA-1110** | `app/docs/BACKLOG.md:1887` | Duplicatas podem afetar seleção e contagem; não acrescentar deduplicação ao backfill. A cura está registrada **depois da E4d**. (`app/docs/BACKLOG.md:1887`) |
| **BUG-PLANTIO-NOTURNO-MAIS-1** | `app/docs/BACKLOG.md:1891`; ocorrência anterior `app/docs/BACKLOG.md:1817` | Não autoriza migrar dia no E4b. A leitura mantém fato = dia do turno e conversa = dia civil; plantio tenta dia e dia seguinte no envelope inclusivo. (`app/chamados/juizes.py:332`; `app/chamados/juizes.py:324`; `app/chamados/juizes.py:326`) |
| **LOTE-VALIDAR-REGRESSAO-0311 / LOTE-VALIDAR-VIRA-JOB** | `app/docs/BACKLOG.md:1892`; `app/docs/BACKLOG.md:1831` | Cruza porta de validação, mas acrescenta investigação de desempenho/job; não muda a autorização de E4b para criar decisões ou efeitos. A regressão ainda manda medir causa antes. (`app/docs/BACKLOG.md:1892`) |
| **BO-0611-05-RESTO-DA-CLASSE** | `app/docs/BACKLOG.md:1889` | Não permite afrouxar dedup/colisão para forçar o passivo. A porta conserva dedup de mesmo tipo, competência e recusa automática por oposta. (`app/ponto/registro_batida.py:113`; `app/ponto/registro_batida.py:132`; `app/ponto/registro_batida.py:145`; `app/ponto/registro_batida.py:150`) |
| **BO-0611-05-TEM-GPS-LEITORES** | `app/docs/BACKLOG.md:1890` | E4b não deve inventar presença GPS nem normalizar coordenadas no comando. A pergunta do mapa antigo foi respondida pelo corte vigente: zero/par parcial = sem GPS; isso é leitura, não veto de nascimento. (`app/docs/CORTES.md:442`; `app/ponto/services/geofence.py:126`; `app/ponto/services/geofence.py:136`) |
| **BO-ROTULO-ENCERRADO** | `app/docs/BACKLOG.md:1884` | Não altera o efeito do backfill; é cura de rótulo, expressamente sem mexer em estado. (`app/docs/BACKLOG.md:1884`) |
| **SONDA3-METODO-CORRECAO** | `app/docs/BACKLOG.md:1885` | Não usar “transição tecnicamente aceita com superusuário” como prova de ação do supervisor ou regularização do fato. A própria linha distingue esses resultados. (`app/docs/BACKLOG.md:1885`) |
| **BO-DISPUTA / matriz da porta / RAIZ-DA-DISPUTA** | `app/docs/BACKLOG.md:1877`; `app/docs/BACKLOG.md:1878`; `app/docs/BACKLOG.md:1883` | Dependência de coerência das portas, não autorização para E4b reparar as sete por shell nem incorporar raiz/agregado. O registro proíbe mexer nas sete por shell e preserva HX. (`app/docs/BACKLOG.md:1877`) |

As classes a–e continuam decididas em `censo_alcance`: sem célula=e; véu=a; respondida sem texto=b; respondida com âncora terminal=c; sem vivas, colaborador na lista mas dia fora=d; demais=e. Portanto o mapa não substitui esse classificador por “classe→cura” ou “classe→dono”. (`app/chamados/services/vigia_fio.py:667`; `app/chamados/services/vigia_fio.py:669`; `app/chamados/services/vigia_fio.py:673`; `app/chamados/services/vigia_fio.py:675`; `app/chamados/services/vigia_fio.py:677`; `app/chamados/services/vigia_fio.py:680`)

Outras linhas atuais que afetam a leitura do universo/portas:

| Linha | Consequência |
|---|---|
| `app/docs/BACKLOG.md:1898` FIO-ESTADO-CARIMBO-VELHO | Carimbo anterior à E4a2 pode não conter campos novos; ausência de chave não prova zero. (`app/docs/BACKLOG.md:1898`) |
| `app/docs/BACKLOG.md:1899` CARIMBO-DIA-CIVIL-X-TURNO | `hoje` do vigia é dia civil local da medição, não dia do turno da pergunta. O código usa `timezone.localdate(agora)` e grava esse valor. (`app/chamados/management/commands/vigiar_fio_morto.py:166`; `app/chamados/management/commands/vigiar_fio_morto.py:167`; `app/chamados/management/commands/vigiar_fio_morto.py:230`) |
| `app/docs/BACKLOG.md:1902` E4A1-C6-X-PASSADO-SEM-TURNO | Não presumir que chamar a fábrica sempre atravessa descarimbo: há retorno antecipado para passado sem turno previsto. (`app/chamados/services/fabrica_da_celula.py:201`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:205`) |
| `app/docs/BACKLOG.md:1903` E4A2-RELOGIO-FIO-VIVO | Não afirmar controle integral de C6 por relógio do comando; dívida registrada de injeção de relógio. (`app/docs/BACKLOG.md:1903`) |
| `app/docs/BACKLOG.md:1905` O169-JUIZES-760 | Não tomar destino encerrado como efeito materializado sem a conferência da autoridade; alcance produtivo segue dependente de medida. (`app/docs/BACKLOG.md:1905`) |

Item 4 — cerca e delta pós-11h:

| Arquivo/cerca | Resultado |
|---|---|
| `disparar_perguntas_competencia.py` | **Manter NÃO TOCAR**. O motivo “E4a2 em curso” envelheceu: agora é vigia; `handle` fica em `:145`, não `:65`; não aceita `--apply`; ajuda declara leitura; reparo indicado é `consumir_eventos_celula --semear`, cujo consumo fica para rodada seguinte. (`app/chamados/management/commands/disparar_perguntas_competencia.py:1`; `app/chamados/management/commands/disparar_perguntas_competencia.py:11`; `app/chamados/management/commands/disparar_perguntas_competencia.py:128`; `app/chamados/management/commands/disparar_perguntas_competencia.py:132`; `app/chamados/management/commands/disparar_perguntas_competencia.py:145`) |
| `config/crons.py` | Manter cerca: cron de disparar já usa `--comp-corrente --alarme` sem apply, papel vigia registrado; não há razão de E4b reeditar para reproduzir E4a2. (`app/config/crons.py:1521`; `app/config/crons.py:2044`; `app/config/crons.py:2168`) |
| `vigiar_fio_morto.py` | Manter cerca desta volta DRY. `handle` passou do endereço antigo `:151` para `:152`; agrega novos contadores do disparar e seus esperados no carimbo. A composição atual ainda avalia `d` antes de incorporar alcance/materializada, portanto etapa futura do alarme não se resume a acrescentar nomes à tupla. (`app/chamados/management/commands/vigiar_fio_morto.py:152`; `app/chamados/management/commands/vigiar_fio_morto.py:181`; `app/chamados/management/commands/vigiar_fio_morto.py:184`; `app/chamados/management/commands/vigiar_fio_morto.py:185`; `app/chamados/management/commands/vigiar_fio_morto.py:233`) |
| `ponto/services/geofence.py` | Acrescentar como dependência protegida explícita da obra já pousada: juiz `tem_gps` em `:126`; geofence chama em `:269`; sem GPS cria alerta com coordenadas nulas. Não editar essa regra no E4b. (`app/ponto/services/geofence.py:126`; `app/ponto/services/geofence.py:269`; `app/ponto/services/geofence.py:271`; `app/ponto/services/geofence.py:275`) |
| `core/templatetags/hasner_filters.py`, `ponto/views.py` e registro já cercado `core/juizes.py` | Complemento de cerca GPS: filtros delegam ao juiz; contador offline também; registro aponta juiz. Não reconstruir presença por nulidade no E4b. (`app/core/templatetags/hasner_filters.py:30`; `app/core/templatetags/hasner_filters.py:34`; `app/core/templatetags/hasner_filters.py:38`; `app/ponto/views.py:485`; `app/core/juizes.py:1201`) |
| `ponto/registro_batida.py` | Acrescentar explícito **só chamar pela materialização, não editar**. Conserva competência por `fato_de_chao=False`, dedup e colisão; o novo comando não pode abrir outro escritor para contorná-los. (`app/ponto/registro_batida.py:94`; `app/ponto/registro_batida.py:123`; `app/ponto/registro_batida.py:127`; `app/ponto/registro_batida.py:132`; `app/ponto/registro_batida.py:150`) |
| `fabrica_da_celula.py` | Tratar como porta protegida e dependência de BO-DISPUTA, não copiar lógica: `perguntar_pela_celula` atual em `:178`; pode retornar sem emitir por sem adesão, pauta de cadastro ou limbo exportado. Logo chamada não prova pergunta criada. (`app/chamados/services/fabrica_da_celula.py:178`; `app/chamados/services/fabrica_da_celula.py:183`; `app/chamados/services/fabrica_da_celula.py:204`; `app/chamados/services/fabrica_da_celula.py:214`; `app/chamados/services/fabrica_da_celula.py:215`) |

A pergunta GPS do mapa.r não deve ser reaberta: o corte atual é explícito, inclusive sobre zeros e par parcial. As duas perguntas sobre desligado/férias existem no backlog, mas pertencem à obra registrada correspondente e não são condição normativa nova para E4b cumprir seu DRY pelas portas existentes. (`app/docs/CORTES.md:442`; `app/docs/BACKLOG.md:1881`)

Identidade de commit e atribuição exata de cada hunk a pouso pós-11h: **[nao verificado]**, pois esta cópia não fornece metadados Git verificáveis. [nao verificado]

## 10/11 21:30 · sessao 724cd7299560 · - · - · origem=Ronald (tmux)

Conferência das **26 portas** abaixo. “Compatível” significa que o corpo atual sustenta a descrição do censo; identidade textual com a base e autoria/data dos deltas são **[nao verificado]** sem árvore-base/Git.

| Porta | Censo → HEAD | Semântica conferida |
|---|---|---|
| `ChamadoColaborador._gravar_estado` | `app/chamados/models.py:201` → **:201** | Compatível: estado/via/autor, urgência e consequências nas perguntas. `app/chamados/models.py:228`, `:245`, `:292`, `:309`. |
| `DisputaSupervisao.fechar` | `app/chamados/models.py:1852` → **:1855** | Compatível: fechamento F; exceções carimbam mudas e registram DP `SILENCIO`; zero perguntas usa `vazia`. `app/chamados/models.py:1903`, `:1921`, `:1924`, `:1932`. |
| `reabrir_admin` | `app/chamados/models.py:2123` → **:2126** | Compatível: limpa F/resolução/motivo; conta reabertura e agrega motivo opcional. `app/chamados/models.py:2149`, `:2153`, `:2161`. |
| `reabrir_sistema` | `app/chamados/models.py:2160` → **:2163** | Compatível: limpa F/resolução/motivo, idempotência por aberta; nota do sistema. `app/chamados/models.py:2167`, `:2173`, `:2180`. |
| `PerguntaDisputa.carimbar` | `app/chamados/models.py:3092` → **:3103** | Compatível: C; completa R/V condicionalmente; julga com C6, restaura memória se recusa; L opcional. **Não chama DP no ramo de assinatura**. `app/chamados/models.py:3130`, `:3139`, `:3149`, `:3152`. |
| `carimbar_identidade` | `app/chamados/models.py:3157` → **:3168** | Compatível: C sem C6, conserva instante/autor quando solicitado; recusa no mesmo save. `app/chamados/models.py:3182`, `:3187`, `:3190`. |
| `descarimbar` | `app/chamados/models.py:3192` → **:3203** | Compatível: limpa os três C; `salvar=False` apenas memória; fora do fato recusa. `app/chamados/models.py:3206`, `:3208`, `:3211`. |
| `descarimbar_em_lote` | `app/chamados/models.py:3241` → **:3252** | Compatível: exige donos travados, exclui fora do fato, C + `outros`. `app/chamados/models.py:3258`, `:3264`. |
| `encerrar_por_fato_no_espelho` | `app/chamados/models.py:3257` → **:3268** | Compatível: só troca via, sem fabricar efeito. `app/chamados/models.py:3273`, `:3275`. |
| `_carimbar_validacao` | `app/chamados/services/validacao.py:127` → **:127** | Compatível: V e DP `VALIDACAO` no mesmo instante/atomic; preserva projeção anterior sem linha. `app/chamados/services/validacao.py:133`, `:134`, `:139`. |
| `devolver_validacao_recusada` | `app/chamados/services/validacao.py:163` → **:163** | Compatível: limpa V, conserva resposta, DP `DEVOLUCAO`; contexto de continente recarregado. `app/chamados/services/validacao.py:188`, `:199`, `:205`, `:214`. |
| `responder_pergunta_disputa_core` | `app/chamados/services/disputa_emissao.py:1422` → **:1425** | Compatível: R, limpa V, delega C, advertência de escape, DP `RESPOSTA`; trava C antes de persistir. `app/chamados/services/disputa_emissao.py:1467`, `:1480`, `:1485`, `:1497`, `:1506`, `:1511`. |
| `reabrir_para_correcao` | `app/chamados/services/disputa_emissao.py:1559` → **:1562** | Compatível: limpa R/V; C opcional; foto em DP `REABERTURA`/`RECUSA`; **não escreve F**. `app/chamados/services/disputa_emissao.py:1593`, `:1615`, `:1629`, `:1632`. |
| `responder_automaticamente` | `app/chamados/services/disputa_emissao.py:1635` → **:1638** | Compatível: texto normalizado existente é no-op; R e DP no instante recebido. `app/chamados/services/disputa_emissao.py:1647`, `:1652`, `:1656`. |
| `carimbar_resposta_pela_trilha` | `app/chamados/services/disputa_emissao.py:1658` → **:1661** | Compatível: exige texto sem carimbo; conserva instante testemunhado em R/DP. `app/chamados/services/disputa_emissao.py:1670`, `:1674`, `:1677`. |
| `relavrar` | `app/chamados/services/relavra_via.py:106` → **:106** | Compatível: via derivada e L. `app/chamados/services/relavra_via.py:118`, `:126`. |
| `_materializar_resposta_hora` | `app/chamados/services/materializacao.py:784` → **:784** | Compatível: pendência/recusa continuam escrita semântica própria; efeito/carimbo delegados. `app/chamados/services/materializacao.py:850`, `:851`, `:924`, `:927`. |
| `_carimbar_recusa` | `app/chamados/services/materializacao.py:1083` → **:1083** | Compatível: recusa técnica; colisão delega identidade, demais salvam string; nota técnica, sem DP `RECUSA` humana. `app/chamados/services/materializacao.py:1093`, `:1099`, `:1102`, `:1107`. |
| `recusar_pergunta` | `app/chamados/services/validacao.py:533` → **:533** | Compatível: recusa humana exige motivo e R; limpa via reabertura com `ato='recusa'`. `app/chamados/services/validacao.py:561`, `:576`, `:590`, `:593`. |
| `conferir_data_do_fato` | `app/chamados/services/validacao.py:625` → **:625** | Compatível: juiz de divergência, sinalizadores de conferência e auditoria com dois dias/justificativa; não gera DP de conferência. `app/chamados/services/validacao.py:658`, `:664`, `:667`. |
| `ChamadoColaborador.abrir` | `app/chamados/models.py:833` → **:833** | Compatível: nascimento/reuso do chamado pela porta; estado canônico preservado. `app/chamados/models.py:833`. |
| `ChamadoColaborador.criar` | `app/chamados/models.py:1054` → **:1054** | Compatível: nascimento de chamado pela porta. `app/chamados/models.py:1054`. |
| `_abrir_ou_agregar_disputa` | `app/chamados/services/disputa_emissao.py:474` → **:474** | Corpo HEAD é anterior à cura BO anexada: nascimento/agregação; núcleo atual retorna DS antes da geração do wrapper. `app/chamados/services/disputa_emissao.py:603`, `:634`, `:665`. |
| `criar_disputa_a_partir_de_chamados_orfaos` | `app/chamados/services/disputa_emissao.py:1024` → **:1027** | Descrição de nascimento aberto/contexto continua aplicável; cura futura matriz não é HEAD. `app/chamados/services/disputa_emissao.py:1027`. |
| `criar_disputa_ancora_de_chamado` | `app/chamados/services/disputa_emissao.py:1299` → **:1302** | Nascimento/âncora continua nesta porta. `app/chamados/services/disputa_emissao.py:1302`. |
| `PerguntaDisputa._nascer` | `app/chamados/models.py:2822` → **:2825** | **Descrição do censo incompleta:** além de identidade/defaults/reuso/L, nascimento com R/V recebidos conserva assinaturas em DP no mesmo atomic: `DecisaoPergunta.registrar(linhas_da_projecao_sem_linha([p]))`. `app/chamados/models.py:3014`, `:3017`, **:3018–3019**. |

Delta dos executores e juízes que importa ao backfill:

| Item | HEAD / consequência |
|---|---|
| DP canônica | Censo `models.py:3932` → **`app/chamados/models.py:3943`**; **brief já aponta corretamente :3943**. Lote vazio retorna antes do atomic; lote não vazio faz `bulk_create` e acorda células. **Não deduplica por si**: seleção canônica antes continua necessária. `app/chamados/models.py:3947`, `:3951`, `:3953`, `:3954`. |
| Imutabilidade DP | `save` antigo :3892 → **`app/chamados/models.py:3903`**; reescrita recusa; delete também recusa. `app/chamados/models.py:3904`, `:3908`. |
| Correspondência legado | Antiga :3903 → **`app/chamados/models.py:3914`**; correspondência continua `(pergunta, ato, instante)`, inclui dedup no próprio lote, conserva V por autor. `app/chamados/models.py:3927`, `:3931`, `:3935`. |
| Pergunta save | Antiga :3402 → **`app/chamados/models.py:3413`**. |
| D3: “tem resposta” | Distinções permanecem: timestamp em **`app/chamados/models.py:3497`**; SQL timestamp + texto não vazio literal em **:3348**; C6 timestamp + `.strip()` em **`app/chamados/juizes.py:1341`**; tarefa usa DP e passivo sem linha em **`app/chamados/services/tarefa_do_dia.py:127`**, **:132**. |
| D4: validação ≠ efeito | Rótulo por V/M agora **`app/chamados/models.py:3034`**, condições **:3041**; C6 V mata em **`app/chamados/juizes.py:1325`**; DP `VALIDACAO` exige lavra vigente e distingue materializar/rejulgar/encerrar em **`app/chamados/services/tarefa_do_dia.py:140`**. |
| D5/D6 | Dia do turno para fato, civil para conversa continuam explícitos em **`app/chamados/juizes.py:332`**; instante de plantio com cadeia e envelope inclusivo continua em **`app/chamados/juizes.py:296`**, **:324**, **:326**. |
| **D9 precisa atualizar** | Já há injeção `hoje`: **`app/chamados/juizes.py:569`**, **:839**, **:1324**. Memo inclui `hoje` em **:852**; cálculo o propaga em **:856**, **:1389**, **:1406**. `date.today()` fica **fallback**, em **:577**. Portanto “C6 conserva date.today que --agora não controla” precisa qualificar o chamador: sem `hoje`, conserva; com `hoje`, usa o dia fornecido. |
| D9 — bordas preservadas | Janela inclusiva em **`app/chamados/juizes.py:603`**; lavra expira `<=` por dia local e julgamento local anterior à borda em **`app/chamados/veredito_lavrado.py:116`**. O instrumento ainda imprime literalmente o limite do fallback em **`app/chamados/management/commands/diff_e3_sombra.py:105`**. |

Não atribuir automaticamente `EXPAND` do nascimento ou injeção de `hoje` aos pousos posteriores às 11h: essa cronologia é **[nao verificado]**; a linha de DP do próprio brief já coincide com HEAD.

## 10/11 21:30 · sessao 8b2dc0981577 · - · - · origem=Ronald (tmux)

O patch `diff_obra_v10.patch` não está disponível na cópia nem no texto recebido; seus hunks e a lista exata de arquivos tocados permanecem **[nao verificado]**. As matrizes1/2 descrevem uma implementação não presente nesta árvore: aqui o núcleo está em `app/chamados/services/disputa_emissao.py:474`, a geração continua posterior ao núcleo no wrapper em `app/chamados/services/disputa_emissao.py:670`, e não há redirecionamento recursivo nesse núcleo (`app/chamados/services/disputa_emissao.py:521`, `app/chamados/services/disputa_emissao.py:603`).

**Item 2 — cruzamentos de código previstos pelas matrizes**

“Escrita” abaixo significa editar o arquivo-fonte. Chamar uma porta que escreve no banco é uso da porta, sem edição do arquivo que a implementa. O comando E4b ainda não existe nesta cópia **[nao verificado quanto à implementação futura]**.

| Arquivo previsto na cura BO-DISPUTA | Cruzamento necessário da E4b | Comportamento que a E4b precisa conferir/preservar |
|---|---|---|
| `app/chamados/services/fabrica_da_celula.py` | **LEITURA + chamada de porta**, sem edição necessária demonstrada: `perguntar_pela_celula` está em `:178`; chama `perguntar` em `:219`; esta chama wrapper e gerador em `:172` e `:174`. | A fábrica usa dia da célula, respeita teto pelo instante, pode abrir pauta sem pergunta e devolve **motivos**, não prova de perguntas criadas (`:201`, `:204`, `:207`, `:215`, `:220`). BO pretende retirar a segunda geração; E4b não pode depender de duas tentativas nem contar `ms` como efeito consumado (`:172`, `:174`, `:220`). |
| `app/chamados/services/disputa_emissao.py` | **LEITURA transitiva obrigatória** pela fábrica: núcleo `:474`, wrapper `:634`, gerador `:300`, pendentes `:257`. Outras portas dependem da classificação: órfãos `:1027`; âncora `:1302`. | Não supor que retornar DS significa pergunta própria criada: o núcleo cria DS em `:587` e retorna em `:603`, enquanto a geração ocorre depois, em `:670` ou `:682`. A geração admite ausência de dia e `continue` (`:425`, `:430`) e pode devolver reuso da porta (`:444`, `:470`). A E4b precisa observar resultado real e dona efetiva, sem implementar por conta própria o planejamento que BO pretende corrigir. |
| `app/chamados/services/disputa_emissao.py` | **LEITURA/chamada condicional de porta histórica**, sem edição necessária: `carimbar_resposta_pela_trilha :1661`; resposta `:1425`; correção `:1562`. | Resposta nova conserva legado antes de sobrescrever (`:1466`) e registra DP com o mesmo instante (`:1462`, `:1511`). A reconstrução da E4b deve continuar distinguindo ato testemunhado de resposta nova; não usar o core de resposta para inventar história (`:1462`, `:1466`, `:1511`). |
| `app/chamados/services/declaracao_texto.py` | **LEITURA transitiva**, sem necessidade demonstrada de chamar `declarar_hora_colab :306` ou `aplicar_declaracao :641` para o backfill. A fábrica lê mapeamento em `fabrica_da_celula.py:62`; o emissor lê marcos/abstenção em `disputa_emissao.py:859`, `:864`, `:895`. | `_marcos_faltantes :86` e a abstenção existente precisam conservar o significado do dia/ata. Não presumir que `_candidata_reusavel :230` resolve dona global: ela retorna `None` quando a canônica pertence a outra DS (`:250`, `:254`). Os consumidores ainda abrem depois do nascimento da DS (`:360`, `:376`, `:659`, `:666`); BO pretende corrigir justamente essa composição. |
| `app/ponto/management/commands/reconciliar_grade.py` | **Nenhuma edição nem chamada exigida pela E4b**; possível leitura de precedente **[nao verificado]**. Sítio previsto pela matriz: `_disputa :412`. | O helper chama núcleo com `motivos=[]` (`:424`, `:425`). Não é alternativa para a fábrica nem prova de pergunta faltante criada. |
| `app/chamados/management/commands/smoke_chamados.py` | **Nenhuma edição nem chamada exigida pela E4b**; possível leitura de precedente **[nao verificado]**. Sítio previsto: `_bloco_a_motivos :82`. | O smoke chama núcleo em `:99` e gerador novamente em `:102`; seu mecanismo não deve ser copiado pelo backfill. |
| `app/chamados/management/commands/auditar_invariantes_chamados.py` | **Nenhuma edição nem chamada exigida pela E4b**; possível leitura de diagnóstico **[nao verificado]**. | A cópia começa diretamente pelo piso I6 (`:14`, `:29`); o helper I7 `_dias_da_disputa_sem_pergunta` descrito nas matrizes não existe aqui **[nao verificado quanto ao futuro BO]**. O censo E4b deve usar o vigia contratado, não importar um helper futuro dessa auditoria. |

**Dependências comuns que as matrizes mandam preservar, sem demonstrar edição pela BO**

| Arquivo | E4b lê/chama e implicação |
|---|---|
| `app/chamados/models.py` | DP `registrar :3943` faz `bulk_create :3953`, sem deduplicação nessa operação; correspondência legada está em `:3927`, `:3931`, `:3935`. BO declara manter a porta da pergunta; edição por BO **[nao verificado]**. E4b deve selecionar sob trava e conservar a identidade completa, sem deduplicar só por existência de algum ato. |
| `app/chamados/services/fio.py` | Universo `:1784`, canônica `:1225`, dia da linha `:1608`, recusas `:1572` e `:1591`. A canônica prefere texto normalizado, depois menor PK (`:1231`, `:1232`, `:1233`); isso é distinto de `respondida_em`. Edição por BO **[nao verificado]**. |
| `app/chamados/reconciliador.py` | Religação `reatar_pergunta_orfa :1533` chama o derivador do emissor (`:1556`) e pode reemitir chamado (`:1621`). Usa data da pergunta (`:1557`), não simplesmente o último contexto da DS. Edição por BO **[nao verificado]**. |
| `app/chamados/services/materializacao.py` | Unidade `:1158`, ausência declarada `:462`, batida pela porta em `:927`/`:1007`. A unidade não possui barreira de exportada (`:1166`, `:1167`, `:1215`); a E4b deve fornecer sua própria seleção/revalidação contratada. Edição por BO **[nao verificado]**. |
| `app/chamados/signals.py` | Save de DS fechada pode reconciliar pai (`:28`, `:42`), agendar regularização no commit (`:129`, `:144`) e materializar pendentes sincronamente (`:153`, `:163`). Não supor que save só de motivos/contexto seja sem efeito: essas guardas não inspecionam `update_fields`. Edição por BO **[nao verificado]**. |

**DRY e integração:** rollback de banco não basta para impedir push disparado imediatamente pelo wrapper (`app/chamados/services/disputa_emissao.py:671`, `:687`, `:1767`). O caminho contratado da fábrica já silencia push (`app/chamados/services/fabrica_da_celula.py:218`); a guarda está em `app/chamados/services/disputa_emissao.py:1753`. A E4b deve preservar esse caminho e recontar os efeitos reais, incluindo cascatas síncronas; efeitos dependentes do acervo e execução dos callbacks permanecem **[nao verificado]**.

**Conclusão para consolidação:** não há necessidade demonstrada de a E4b **editar** qualquer dos arquivos previstos pelas matrizes BO; ela pode escrever somente seu comando novo e os dois selos pedidos **[nao verificado quanto à implementação futura e ao patch ausente]**. Há dependência de leitura/chamada da fábrica e do emissor alterados pela BO, portanto a compatibilidade precisa ser reconferida após seu pouso (`app/chamados/services/fabrica_da_celula.py:172`, `:174`, `:219`).

**cruzamento de escrita com BO-DISPUTA: 0**, no escopo proposto do brief; confirmação contra `diff_obra_v10.patch`: **[nao verificado]**.

## 10/11 21:02 · sessao 018ec434d395 · BO-DISPUTA-SEM-PERGUNTA · verificador · origem=Code

**NAO LIBERA.** Ainda há nascimento por declaração sem pergunta local, encaminhamento incompleto nos pendentes e uma catraca de escrita que reprova o novo helper. (`app/chamados/services/disputa_emissao.py:744`; `app/chamados/services/declaracao_texto.py:376`; `app/chamados/services/disputa_emissao.py:296`; `app/chamados/tests/test_porta_disputa_emissao.py:129`)

A identificação Git da cópia, a execução dos 68 labels, PostgreSQL, produção e o estado das sete disputas existentes são **[nao verificado]**.

**1. Achados anteriores e matriz**

Não encontrei o conteúdo de `preverif2.r`, `matriz2.r` nem `triagem_preverif2.txt` na cópia ou no dado transcrito; portanto, a correspondência **achado por achado e linha por linha** desses documentos é **[nao verificado]**. A tabela abaixo acompanha somente os estados a–j enumerados no aval fornecido.

| Estado | Situação atual e linha que decide |
|---|---|
| **a. Fato novo sem pergunta existente** | **Parcial.** O núcleo desfaz pai/DS quando não admite pergunta local; a declaração desliga essa proteção com `emitir_perguntas=False`. (`app/chamados/services/disputa_emissao.py:677`; `app/chamados/services/disputa_emissao.py:744`) |
| **b. Canônica em outra DS aberta** | **Parcial.** Núcleo e helper dos órfãos encaminham motivos; pendentes e declarações não fazem esse encaminhamento. (`app/chamados/services/disputa_emissao.py:644`; `app/chamados/services/disputa_emissao.py:960`; `app/chamados/services/disputa_emissao.py:296`; `app/chamados/services/declaracao_texto.py:666`) |
| **c. Canônica em DS fechada** | **Curado quanto ao nascimento no núcleo; pergunta de lei aberta quanto ao motivo ausente da dona.** Não nasce outra DS sem pergunta e a fechada não é salva; se ela não continha o motivo, continua sem contê-lo. O teste exige essa conservação. (`app/chamados/services/disputa_emissao.py:679`; `app/chamados/services/disputa_emissao.py:547`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:87`) |
| **d. Ciclo A↔B** | **Curado quanto à reentrada do núcleo.** Replay usa os argumentos conservados e desfaz retorno criado/PK divergente; não chama novamente o gerador. Isso não elimina motivos presentes em duas DS. (`app/chamados/services/disputa_emissao.py:701`; `app/chamados/services/disputa_emissao.py:707`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:37`) |
| **e. Exceção depois da batida gravada** | **Curado no limite transacional do núcleo.** O gerador está dentro do atomic; os testes verificam rollback preservando a batida anterior. Quem efetivamente reemite em produção é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:619`; `app/chamados/services/disputa_emissao.py:670`; `app/chamados/tests/test_bo_disputa_sem_pergunta.py:44`) |
| **f. HX sem ata** | **Igual na lei, com contador acrescentado.** Continua aberta sem pergunta; I7b publica número informativo. (`app/chamados/services/disputa_emissao.py:1400`; `app/chamados/management/commands/auditar_invariantes_chamados.py:74`; `app/chamados/management/commands/auditar_invariantes_chamados.py:81`) |
| **g. Grade** | **Curado o helper vazio removido.** O selo atual proíbe `_disputa` e a chamada direta ao núcleo na grade. Correspondência com o antigo endereço `:1452` da matriz ausente: **[nao verificado]**. (`app/ponto/management/commands/reconciliar_grade.py:412`; `app/chamados/tests/test_porta_chamado_e3.py:91`) |
| **h. Órfãos** | **Parcial.** O helper trata a dona estrangeira, mas há perda do coletor no complemento anterior ao filtro. HX e h2 continuam caminhos explícitos sem pergunta local. (`app/chamados/services/disputa_emissao.py:960`; `app/chamados/services/disputa_emissao.py:1214`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:137`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:158`) |
| **i. Motivo já em duas DS** | **Igual quanto à duplicação preexistente.** Os dois escritores fazem união; não removem o motivo da local. (`app/chamados/services/disputa_emissao.py:549`; `app/chamados/services/disputa_emissao.py:593`) |
| **j. Push e dona** | **Parcial.** Wrapper e agrupador filtram perguntas realmente criadas na DS escolhida; o complemento pode sair antes desse consumo. (`app/chamados/services/disputa_emissao.py:792`; `app/chamados/services/disputa_emissao.py:1410`; `app/chamados/services/disputa_emissao.py:1214`) |

**2. Consumo de `(p, criada)` e destino dos motivos**

| Caminho | Consumo efetivo |
|---|---|
| Gerador datado | Lê ambos, distingue criação/reuso e conserva argumentos estrangeiros quando recebe `_reusos`. (`app/chamados/services/disputa_emissao.py:488`; `app/chamados/services/disputa_emissao.py:509`) |
| Núcleo, DS existente ou nascimento | Divide locais/estrangeiras e encaminha os motivos à dona aberta. (`app/chamados/services/disputa_emissao.py:634`; `app/chamados/services/disputa_emissao.py:672`; `app/chamados/services/disputa_emissao.py:606`) |
| Replay | Lê ambos; exige reuso do mesmo PK e desfaz divergência. (`app/chamados/services/disputa_emissao.py:706`) |
| `_criar_pergunta_se_couber` | Lê ambos; estrangeira recebe encaminhamento e não acrescenta motivo novo à local. (`app/chamados/services/disputa_emissao.py:950`; `app/chamados/services/disputa_emissao.py:960`) |
| `_complementar_marcos_faltantes` | Delega ao helper anterior, passando o coletor. (`app/chamados/services/disputa_emissao.py:999`) |
| `perguntar_entradas_pendentes` | O gerador lê ambos, mas este consumidor descarta causas e não encaminha estrangeiras. (`app/chamados/services/disputa_emissao.py:296`) |
| Declaração do colaborador/admin | Usa `perg`, ignora `criada` e não compara a dona. (`app/chamados/services/declaracao_texto.py:376`; `app/chamados/services/declaracao_texto.py:666`) |
| Âncora e reabertura administrativa | Ignoram o retorno de `abrir_conversa`; a porta real de conversa nasce sem dedup estrangeiro. (`app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/acoes_disputa.py:263`; `app/chamados/models.py:2891`) |
| Gerador público, fábrica e smoke | O gerador público descarta causas/criadas; fábrica e smoke ignoram sua lista. (`app/chamados/services/disputa_emissao.py:518`; `app/chamados/services/fabrica_da_celula.py:174`; `app/chamados/management/commands/smoke_chamados.py:102`) |

**Há caminho fora HX/h2 que conserva DS local sem pergunta:** a declaração cria/agrega a local antes de perguntar; `_candidata_reusavel` recusa a candidata estrangeira, mas o `abrir` seguinte pode devolvê-la. Não há desfazimento da local por esse resultado, inclusive nos retornos normais de recusa. (`app/chamados/services/disputa_emissao.py:744`; `app/chamados/services/declaracao_texto.py:254`; `app/chamados/services/declaracao_texto.py:376`; `app/chamados/services/declaracao_texto.py:412`; `app/chamados/services/declaracao_texto.py:683`)

**Há motivo em nenhuma lista:** nos pendentes, o classificador pode escolher um motivo novo, a porta devolver pergunta estrangeira e nenhuma DS receber esse motivo. Reproduzi esse resultado com função produtiva extraída por AST e fronteiras em memória. (`app/chamados/services/disputa_emissao.py:288`; `app/chamados/services/disputa_emissao.py:296`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:31`)

**Há motivo em duas listas:** `locais()` verifica o motivo, sem restringir o dia; uma pergunta local antiga pode admitir o motivo em A enquanto a pergunta estrangeira do ato encaminha o mesmo motivo para B. O teste exige ambos. (`app/chamados/services/disputa_emissao.py:575`; `app/chamados/services/disputa_emissao.py:643`; `app/chamados/tests/test_bo_disputa_volta4_memoria.py:37`)

A dona fechada conserva seus campos; a pergunta pode ser reapontada sem transferir `P.disputa`. Se o motivo faltava na lista da fechada, permanece ausente. (`app/chamados/services/disputa_emissao.py:547`; `app/chamados/models.py:2982`; `app/chamados/models.py:3008`)

**3. Push ao colaborador** [nao verificado]

| Caminho | Quando sai e quais motivos leva |
|---|---|
| Wrapper da varredura | Depois do núcleo, somente para a DS retornada/selecionada, aberta na releitura, com motivos das perguntas locais realmente criadas. Releitura adicional ocorre se pendentes chamou o gerador, mesmo devolvendo vazio. (`app/chamados/services/disputa_emissao.py:783`; `app/chamados/services/disputa_emissao.py:787`; `app/chamados/services/disputa_emissao.py:792`; `app/chamados/services/disputa_emissao.py:795`) |
| Agrupador | Após perguntas e tentativa de fechamento, somente com `notificar`, motivos criados na DS principal e DS ainda aberta na consulta. (`app/chamados/services/disputa_emissao.py:1391`; `app/chamados/services/disputa_emissao.py:1410`) |
| Âncora | Envia `ancora_ausente` depois de criar conversa e analisar o chamado, sem releitura de fechamento nem usar `criada`. (`app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1502`; `app/chamados/services/disputa_emissao.py:1504`) |
| Fábrica da célula | O fluxo de julgamento silencia o push do emissor. (`app/chamados/services/fabrica_da_celula.py:218`) |

O helper de envio não verifica abertura nem existência de pergunta: só verifica o silenciador. Assim, a garantia depende de cada chamador; na âncora ela não inclui releitura após cascatas. Uma ocorrência concreta de push de âncora já fechada é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:1876`; `app/chamados/services/disputa_emissao.py:1504`)

Existe **pergunta local nova sem push** no complemento anterior ao filtro: reproduzi aumento de uma para duas perguntas locais e retorno `None`, antes do consumidor do coletor. (`app/chamados/services/disputa_emissao.py:1200`; `app/chamados/services/disputa_emissao.py:1214`; `app/chamados/services/disputa_emissao.py:1410`)

Há também envios que **não exigem pergunta criada no ato**, por finalidade própria: alerta diário, recobrança e reabertura do questionário. Não se pode estender a esses fluxos a condição do coletor de nascimento. (`app/ponto/management/commands/alertar_disputas_pendentes.py:59`; `app/chamados/services/disputa_emissao.py:1851`; `app/chamados/services/acoes_disputa.py:109`)

Cada chamada ao helper tenta **webpush e FCM**; isso são dois transportes explícitos. Entrega duplicada ao mesmo aparelho é **[nao verificado]**. Não identifiquei duas chamadas diretas ao helper no mesmo percurso normal do wrapper/agrupador. (`app/chamados/services/disputa_emissao.py:1890`; `app/chamados/services/disputa_emissao.py:1898`; `app/chamados/services/disputa_emissao.py:798`; `app/chamados/services/disputa_emissao.py:1413`)

**Mudança de critério desde a base:** o delta fornecido troca “motivo novo” por “pergunta local criada”. Hoje `_novos` é ignorado. Isso acrescenta push para novo dia de motivo repetido e suprime push quando o motivo é agregado com pergunta local já existente. A aprovação dessa alteração de comportamento é **[nao verificado]**; o comentário do helper ainda descreve abertura/agregação de motivo novo. (`app/chamados/services/disputa_emissao.py:777`; `app/chamados/services/disputa_emissao.py:792`; `app/chamados/services/disputa_emissao.py:1875`)

**4. Coletor `_ato` / `ato`**

| Campo | Quem escreve | Quem lê |
|---|---|---|
| `criadas` | Núcleo, helper dos órfãos e pendentes. (`app/chamados/services/disputa_emissao.py:640`; `app/chamados/services/disputa_emissao.py:682`; `app/chamados/services/disputa_emissao.py:964`; `app/chamados/services/disputa_emissao.py:299`) | Wrapper e final do agrupador, filtrando pela FK da DS principal. (`app/chamados/services/disputa_emissao.py:792`; `app/chamados/services/disputa_emissao.py:1410`) |
| `gerador_pendentes_chamado` | Wrapper inicializa falso; pendentes marca verdadeiro **antes** da chamada ao gerador. (`app/chamados/services/disputa_emissao.py:776`; `app/chamados/services/disputa_emissao.py:295`) | Wrapper decide se precisa reler o fechamento. (`app/chamados/services/disputa_emissao.py:796`) |

No caminho produtivo de criação, `criada=True` nasce com a DS passada; reuso retorna falso. Portanto, o gerador não registra pergunta estrangeira como criada pela porta real. (`app/chamados/models.py:3014`; `app/chamados/models.py:3031`; `app/chamados/models.py:3008`)

O problema identificado é **coletor registrado e abandonado**, ou filtrado para outra DS: o complemento anterior ao filtro pode retornar cedo; se continuar, perguntas complementadas numa DS diferente da principal também ficam fora do push final. (`app/chamados/services/disputa_emissao.py:1200`; `app/chamados/services/disputa_emissao.py:1214`; `app/chamados/services/disputa_emissao.py:1410`)

**5. Ordem de trava**

`_agregar_motivos_na_dona` **não adquire trava própria**: faz `get` simples e `save`. No núcleo, recebe proteção da trava anterior de todas as DS do colaborador; no agrupador, recebe proteção do fecho do repasse. (`app/chamados/services/disputa_emissao.py:546`; `app/chamados/services/disputa_emissao.py:552`; `app/chamados/services/disputa_emissao.py:621`; `app/chamados/services/disputa_emissao.py:1177`)

No núcleo, a consulta de IDs é lazy; a porta adquire C antes de avaliar IDs, depois D, Ch e P. O replay permanece no atomic externo. (`app/chamados/models.py:2666`; `app/chamados/models.py:2671`; `app/chamados/models.py:2688`; `app/chamados/models.py:2692`; `app/chamados/services/disputa_emissao.py:619`)

No repasse, a sequência é: leitura do fecho → trava de C → releitura → trava do conjunto → releitura e recusa de crescimento. A leitura intermediária ocorre antes de D/Ch/P; não acrescenta colaboradores novos após a primeira trava. (`app/chamados/services/disputa_emissao.py:1121`; `app/chamados/services/disputa_emissao.py:1124`; `app/chamados/services/disputa_emissao.py:1128`; `app/chamados/services/disputa_emissao.py:1133`)

**A proteção não cobre o ato inteiro dos pendentes no wrapper:** ele os chama após o atomic do núcleo terminar. Cada pergunta tem seu atomic, mas o `save(contexto)` posterior não tem atomic/trava própria nem guarda de fechada. A releitura final protege o push, não essa escrita. (`app/chamados/services/disputa_emissao.py:646`; `app/chamados/services/disputa_emissao.py:791`; `app/chamados/models.py:2819`; `app/chamados/services/disputa_emissao.py:301`; `app/chamados/services/disputa_emissao.py:795`)

Ausência de deadlock e comportamento concorrente em PostgreSQL: **[nao verificado]**.

**6. Testes: mordida e limites** [nao verificado]

Executei os quatro controles em memória: **48 testes verdes**. Dos **22 mutantes anunciados, 21 concluíram morte limpa**; isso não representa execução ORM nem suite verde. Os executores exigem controle verde e falha por assertion. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:459`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:392`; `app/chamados/tests/test_bo_disputa_volta8_memoria.py:264`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:251`)

| Testes | O que mordem | O que não provam | [nao verificado]
|---|---|---|
| **5b ORM** | Fixtures conferem ata/marco; casos verificam dona, FK, destino, chamado analisado, ausência de save na fechada, criação local, idempotência, HX e h2. As assertions são concretas. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:41`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:95`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:112`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:132`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:158`) | Execução desses testes: **[nao verificado]**. |
| **5b memória: 11 verdes, cinco mutantes mortos** | Ignorar dona estrangeira; salvar fechada; enviar sem pergunta, no reuso ou após fechamento. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:30`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:45`) | Trava, porta de identidade e fechamento são doubles. (`app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:68`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:87`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:96`) |
| **Volta5 memória: 15 verdes, oito mutantes mortos** | Nascimento vazio, save da fechada, reentrada, eleição antes da trava, replay que cala/re-resolve/insere, guarda de push. (`app/chamados/tests/test_bo_disputa_volta5_memoria.py:26`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:194`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:334`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:342`) | Selos AST não certificam concorrência; fronteiras continuam substituídas. (`app/chamados/tests/test_bo_disputa_volta5_memoria.py:5`; `app/chamados/tests/test_bo_disputa_volta5_memoria.py:94`) |
| **Volta8 memória: oito verdes, sete mutantes mortos** | Releituras por chamada ao gerador, inclusive retorno vazio; fechamento por outra instância; D2 estrangeira. (`app/chamados/tests/test_bo_disputa_volta8_memoria.py:35`; `app/chamados/tests/test_bo_disputa_volta8_memoria.py:182`; `app/chamados/tests/test_bo_disputa_volta8_memoria.py:207`) | Parte dos casos substitui núcleo e persistência para testar o wrapper. (`app/chamados/tests/test_bo_disputa_volta8_memoria.py:118`; `app/chamados/tests/test_bo_disputa_volta8_memoria.py:132`) |
| **Sem pergunta memória: 14 verdes** | Rollback e separação I7a/I7b; `contador_conta_tudo` morre limpidamente. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:278`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:322`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:349`) | `gerador_fora_atomic` termina com erros de montagem: troca três atomics por `nullcontext`, mas `set_rollback` acessa pilha vazia. O próprio executor rejeita essa morte. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:41`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:210`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:469`) |

**Buraco de cobertura demonstrado:** removi somente em memória o registro produtivo de `novas` nos pendentes; os 48 controles continuaram verdes. O teste positivo correspondente troca pendentes por uma função fake que ela própria escreve no coletor; o caso real de volta8 retorna sem perguntas novas. (`app/chamados/services/disputa_emissao.py:299`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:209`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:212`; `app/chamados/tests/test_bo_disputa_volta8_memoria.py:196`)

As funções produtivas são lidas e compiladas da cópia por AST, não reimplementadas manualmente; a exceção relevante acima é a fronteira fake de pendentes no teste positivo. (`app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:31`; `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:47`; `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:212`)

Os protegidos conservam assertions ativas: HX sem ata exige zero perguntas, aberta e luz; com ata completa exige fechamento; HX muda exige zero push; app exige pergunta correta visível e unicidade. Igualdade histórica byte a byte e execução ORM continuam **[nao verificado]**. (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:61`; `app/chamados/tests/test_hx_disputa_muda_sem_ata.py:77`; `app/chamados/tests/test_hx_disputa_muda.py:50`; `app/chamados/tests/test_pergunta_no_app.py:93`; `app/chamados/tests/test_pergunta_no_app.py:107`)

Há vacuidade nos testes condicionais de HX: suas assertions de continuidade aberta só executam **se** houver pergunta. Isso não afeta as assertions incondicionais protegidas citadas acima. (`app/chamados/tests/test_hx_disputa_muda.py:40`; `app/chamados/tests/test_hx_disputa_muda.py:66`)

**7. Catraca de medida do turno**

No delta fornecido mudam somente os números dos mesmos sítios: `809/810 → 826/827` e `1293 → 1319`; o registro atual corresponde às ocorrências da cópia. (`app/core/tests/test_medida_turno_do_par.py:268`; `app/core/tests/test_medida_turno_do_par.py:269`; `app/chamados/services/disputa_e

[truncado em 20000 de 27919 caracteres]

## 10/11 20:59 · sessao e9499ecd9a3b · - · - · origem=Ronald (tmux)

Achado confirmado: novo escritor fora do inventário de atos. `_agregar_motivos_na_dona` salva `DisputaSupervisao` em `app/chamados/services/disputa_emissao.py:552`, mas falta em `ATOS_EMISSAO`, `app/chamados/tests/test_porta_disputa_emissao.py:83`. A igualdade exata da catraca está em `app/chamados/tests/test_porta_disputa_emissao.py:129`. Executei `varrer_arvore`, `_qualificados` e `_dono_da_linha` reais, extraídos/carregados em memória: resultado tem os atos declarados **mais** `'_agregar_motivos_na_dona': 1`; portanto essa assertion reprova. São 11 sítios de escrita de DS no emissor, um deles não declarado. `app/core/censo_escritas.py:469`; `app/chamados/tests/test_porta_disputa_ciclo.py:117`.

Censo dos cinco campos:

| Campo | Escritores | Leitores e significado |
|---|---|---|
| DS.motivos | Nascimento/união no núcleo; encaminhamento à dona; agrupador; âncora. `app/chamados/services/disputa_emissao.py:551`, `:597`, `:666`, `:1203`, `:1243`, `:1251`, `:1280`, `:1376`, `:1481`, `:1489`. Remoção/foto/reabertura administrativa em `app/chamados/models.py:1966`, `:2063`, `:2159`. | Definição diz “cenários ativos”, `app/chamados/models.py:1743`; auditor lê como conjunto que deve corresponder às perguntas, `app/chamados/management/commands/auditar_disputas.py:169`; foto filtra motivo para resolver o fato, `app/api/foto_recebida.py:61`; relatórios exibem rótulos, `app/chamados/relatorios_disputa.py:52`. |
| DS.contexto | Datas pendentes, merge e nascimento, `app/chamados/services/disputa_emissao.py:293`, `:537`, `:600`, `:666`, `:1243`, `:1481`; remoção e anotações de ciclo em `app/chamados/models.py:1967`, `:2064`, `:2104`, `:2122`. | Resolvedor usa data por motivo/última batida, `app/chamados/services/disputa_emissao.py:330`, `:357`, `:372`; destino usa data explícita antes do contexto, `:194`; auditor usa origem/IDs/dia resolvido, `app/chamados/management/commands/auditar_invariantes_chamados.py:31`; materialização prioriza P.data_evento antes de contexto, `app/chamados/services/materializacao.py:343`, `:472`; juiz segue mesma prioridade, `app/chamados/juizes.py:375`; foto exige `ultima_batida.id`, `app/api/foto_recebida.py:67`. |
| DS.fechada_em | Fechamento em `app/chamados/models.py:1932`; reaberturas em `:2149`, `:2173`. Desligamento atual chama `fechar`, não faz bulk update: `app/colaboradores/services/desligamento.py:132`, `:142`. | App separa pendente/histórico, `app/chamados/services/pergunta_no_app.py:71`; regularização bloqueia com qualquer DS aberta, `app/chamados/services/regularizacao_ext.py:58`; três receivers reagem ao **valor**, sem exigir transição nem checar update_fields, `app/chamados/signals.py:28`, `:129`, `:153`; vigia lê perguntas vivas em DS fechada sem reabrir, `app/chamados/services/vigia_fio.py:737`. |
| P.disputa | Nascimento em `_nascer`, `app/chamados/models.py:3014`; FK definida em `:2416`. Reuso devolve a pergunta existente sem transferir a FK, `:3008`. Nenhuma atribuição produtiva `.disputa =`/`.disputa_id =` encontrada no censo textual; atualização indireta fora desse padrão [nao verificado]. | Define dona, colaborador e agrupamento: identidade exige colaborador da disputa, `app/chamados/models.py:3430`; app lê fechamento da dona, `app/chamados/services/pergunta_no_app.py:71`; fio procura dona por destino, `app/chamados/services/fio.py:131`; explicador separa por FK, `app/chamados/services/explicador.py:124`. |
| P.chamado_destino | Nascimento em `app/chamados/models.py:3014`; reuso costura em `:2982`; reapontamento em `app/chamados/services/fio.py:925`. `fio.py:779` é ensaio temporário restaurado em `:783`, não persistência. `models.py:367` reassocia a instância ao mesmo PK, não altera destino. | Contrato: chamado órfão resolvido pela materialização, `app/chamados/models.py:3385`; materialização lê destino em `app/chamados/services/materializacao.py:339`; S143 resolve perguntas do chamado, `app/chamados/signals.py:92`; fio/histórico usam essa ligação, `app/chamados/services/fio.py:135`, `app/chamados/services/historico.py:96`. |

ESMERIL: a dona aberta recebe apenas motivos, não recebe contexto, `app/chamados/services/disputa_emissao.py:549`. O auditor anterior exige contexto com data para motivos datados, `app/chamados/management/commands/auditar_disputas.py:89`; portanto o encaminhamento pode produzir `motivo_sem_contexto`, embora P.data_evento já permita aos consumidores principais trabalhar. Isso é divergência entre escritor e leitor; perda concreta em produção [nao verificado]. O leitor de foto exige a última batida **da dona**, `app/api/foto_recebida.py:67`, mas a identidade BATIDA de P vem da batida conservada na coluna, `app/chamados/models.py:3452`; uma conclusão de falha específica de upload requer cenário adicional [nao verificado].

Classes:

- Fonte da resolução por motivo é compartilhada: gerador chama helper, `app/chamados/services/disputa_emissao.py:458`; auditor chama o mesmo helper, `app/chamados/management/commands/auditar_invariantes_chamados.py:51`. Nesse trecho não há segunda derivação própria.
- Datas indeterminadas conservadas como `None` impedem falso I7b no conjunto conhecido, `app/chamados/management/commands/auditar_invariantes_chamados.py:52`, `:69`. Porém IDs não encontrados ou de outro colaborador desaparecem no filtro, `:36`: sonda com `[id_válido_sem_celula, 999]` resultou dias `{D}` e contadores `(0,1)`. Se IDs sem linha devem contar como indeterminação, essa cobertura permanece parcial; essa lei específica [nao verificado].
- O novo leitor do auditor chama `cobra_marcos_da_ata` sem passar hoje, `app/chamados/management/commands/auditar_invariantes_chamados.py:38`; helper lê relógio em `app/chamados/services/disputa_emissao.py:855`. A derivação de dia foi delegada, mas não ganhou instante único capturado na borda.
- Guardas evitam salvar dona fechada no helper e em `persistir`, `app/chamados/services/disputa_emissao.py:547`, `:589`; necessidade concreta é confirmada pelos receivers por valor, `app/chamados/signals.py:28`, `:129`, `:153`.
- Não encontrei `preverif2.r`, `matriz2.r` ou `triagem_preverif2.txt` na árvore; seus conteúdos e estados item a item [nao verificado]. A cópia não tem repositório Git acessível; identidade com o hash, base, arquivos protegidos intocados e alterações nas sete disputas produtivas [nao verificado].

## 10/11 20:59 · sessao d0543fe153c2 · - · - · origem=Ronald (tmux)

Achados para consolidar:

- **Coletor preenchido e abandonado no retorno precoce:** o complemento anterior ao filtro pode criar pergunta local em `_complementar_marcos_faltantes`, registrá-la em `ato['criadas']`, analisar o chamado e retornar `None` antes do único consumidor de push. Reproduzi com AST real + `AmbienteOrfaos`: pergunta inicial `cobertura_anterior`, chamado de intervalo; nasceu `intervalo_saida` local e houve zero push. Código: `app/chamados/services/disputa_emissao.py:1200`, `:1205`, `:1213`, `:1410`. O teste existente cobre apenas o complemento estrangeiro, sem criação local: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:195`.

- **Teste dos pendentes testa substituto no ponto crítico:** `test_pergunta_nascida_nos_pendentes_tambem_da_um_push` troca a função produtiva por fake que ele mesmo grava no coletor. Removi, somente em memória, o `_ato['criadas'].extend(novas)` produtivo; os quatro arquivos memória permaneceram verdes, 48 testes. Portanto não mordem essa perda de push. Escritor real: `app/chamados/services/disputa_emissao.py:300`; substituto: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:209`, `:212`; volta8 testa pendentes reais apenas com retorno sem criação: `app/chamados/tests/test_bo_disputa_volta8_memoria.py:196`.

- **Mutante anunciado não termina com RED limpa:** `sem_pergunta_memoria --mutante gerador_fora_atomic` dá controle verde, depois 3 failures e 2 errors; os errors são `IndexError` em `set_rollback`, pois os três `atomic` foram trocados por `nullcontext`, deixando a pilha vazia. O próprio executor recusa a morte, código 1. Mutação: `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:41`; pilha: `:210`; recusa: `:469`. As assertions de rollback mordem, mas a prova completa anunciada não foi validada limpidamente: `:284`, `:301`, `:387`.

Execuções sem banco e sem escrita, todas com `python3 -B`:

- `volta5b_orfaos_memoria`: 11 verdes; cinco mutantes mortos por assertions limpas. Cobrem retorno estrangeiro, dona fechada sem save, push sem pergunta, reuso local e fechamento por outra instância: `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:136`, `:148`, `:159`, `:169`, `:177`, `:187`, `:217`, `:227`, `:239`. Não certificam costura, trava ou fechamento reais: `:68`, `:87`, `:96`.
- `volta5_memoria`: 15 verdes; oito mutantes mortos limpidamente. Cobrem nascimento sem insumo, agregação estrangeira, save de fechada, replay com argumentos exatos, divergência criada/outro PK, lote misto, idempotência, fecho sem push e causas distintas: `app/chamados/tests/test_bo_disputa_volta5_memoria.py:108`, `:121`, `:136`, `:158`, `:194`, `:255`, `:270`, `:289`, `:303`. Os selos de reentrada e eleição são AST; não provam concorrência PostgreSQL: `:334`, `:342`, `:387`.
- `volta8_memoria`: oito verdes; sete mutantes mortos limpidamente. C9 positivo para gerador chamado com retorno vazio: `app/chamados/tests/test_bo_disputa_volta8_memoria.py:182`; sinal omitido/decisão pela lista são mutações reais: `:35`, `:57`. Parte dos testes troca núcleo e persistência por fronteiras explícitas, validando o wrapper: `:118`, `:132`.
- `sem_pergunta_memoria`: 14 verdes; `contador_conta_tudo` morto limpidamente. Fonte é lida da árvore e compilada por AST, sem cópia manual da implementação: `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:28`, `:31`, `:47`. ORM, atomic, catálogo e portas permanecem doubles: `:50`, `:70`, `:169`, `:217`.
- **Total dos quatro controles: 48 testes verdes; 21 dos 22 mutantes anunciados concluíram morte limpa.** Arquivos e executores: `app/chamados/tests/test_bo_disputa_sem_pergunta_memoria.py:459`, `app/chamados/tests/test_bo_disputa_volta5_memoria.py:392`, `app/chamados/tests/test_bo_disputa_volta8_memoria.py:264`, `app/chamados/tests/test_bo_disputa_volta5b_orfaos_memoria.py:251`. Execução ORM, signals e suite de 68 labels: **[nao verificado]**.

O gêmeo ORM 5b usa porta real, reobtém chamado por PK, confere motivo/destino/status e dona fechada sem save. Portanto seus casos não se limitam a contar mocks; execução não realizada. Referências: `app/chamados/tests/test_bo_disputa_volta5b_orfaos.py:70`, `:82`, `:95`, `:102`, `:112`, `:132`, `:155`, `:169`. O fixture prova que a ata realmente produz exatamente o marco esperado: `:41`, `:43`. Não há caso positivo do complemento anterior ao filtro nem pendentes reais nessa classe: lista de casos `:90`, `:106`, `:125`, `:137`, `:147`, `:158`, `:178`.

Os protegidos conservam assertions ativas na cópia: sem ata exige zero perguntas, aberta e luz (`app/chamados/tests/test_hx_disputa_muda_sem_ata.py:58`, `:59`, `:61`); com ata completa exige zero perguntas e fechada (`:77`, `:78`); HX muda exige zero perguntas/fechada e zero push (`app/chamados/tests/test_hx_disputa_muda.py:31`, `:32`, `:50`); app exige pergunta do dia correto visível e unicidade em duas passadas (`app/chamados/tests/test_pergunta_no_app.py:93`, `:94`, `:107`). Não aparecem nos patches fornecidos, mas igualdade histórica byte a byte com main é **[nao verificado]**, pois a cópia não tem `.git`. Há vacuidade já presente nos testes condicionais HX `test_com_pergunta_segue_aberta` e `test_pergunta_antes_de_fechar`: assertions só rodam se houver pergunta; `app/chamados/tests/test_hx_disputa_muda.py:40`, `:66`.

Catraca: executei os 35 testes pelo procedimento do próprio arquivo, substituindo apenas `SimpleTestCase` por `unittest.TestCase` em memória; todos verdes. A identidade é cobrada além dos totais (`app/core/tests/test_medida_turno_do_par.py:534`) e as linhas por igualdade exata (`:540`). No delta fornecido só mudam os números dos mesmos dois sítios: registro atual `:268`, `:269`; ocorrências reais em `app/chamados/services/disputa_emissao.py:826`, `:827`, `:1319`. Totais atuais permanecem 9/85 e A1=6/B1=82: `app/core/tests/test_medida_turno_do_par.py:100`, `:101`.

## 10/11 20:58 · sessao ad44b0f0cb17 · - · - · origem=Ronald (tmux)

Achados principais:

- **Declaração ainda permite DS local sem pergunta, fora HX/h2.** `disputa_para_declaracao` usa `emitir_perguntas=False` e conserva o nascimento antes da pergunta (`app/chamados/services/disputa_emissao.py:740`). Os consumidores chamam a porta, recebem `perg` e ignoram `criada`, sem comparar a dona (`app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`). `_candidata_reusavel` exclui justamente a canônica estrangeira (`app/chamados/services/declaracao_texto.py:254`); portanto a chamada seguinte pode devolver P estrangeira e deixar a nova D sem P. Há retornos normais dentro do atomic depois disso (`app/chamados/services/declaracao_texto.py:412`, `app/chamados/services/declaracao_texto.py:418`, `app/chamados/services/declaracao_texto.py:683`). O motivo entra na local; a dona estrangeira não recebe agregação nesses consumidores (`app/chamados/services/disputa_emissao.py:642`, `app/chamados/services/declaracao_texto.py:376`).

- **Pendentes não encaminha motivo estrangeiro.** Obtém `_motivo` do classificador, recebe `(perguntas, causas, novas)`, mas só acumula listas/coletor e salva contexto; não compara dona nem agrega motivos (`app/chamados/services/disputa_emissao.py:288`, `app/chamados/services/disputa_emissao.py:296`, `app/chamados/services/disputa_emissao.py:301`). Pode haver motivo ausente de ambas as listas se o motivo novo só aparece nesta passagem e estava ausente da dona (`app/chamados/services/disputa_emissao.py:292`, `app/chamados/services/disputa_emissao.py:299`).

- **Pendentes sai do atomic/travas do núcleo no wrapper.** O núcleo retorna sob seu atomic, encerrado ao devolver (`app/chamados/services/disputa_emissao.py:619`, `app/chamados/services/disputa_emissao.py:646`); o wrapper só depois chama pendentes (`app/chamados/services/disputa_emissao.py:791`). Pendentes salva contexto sem atomic próprio, releitura ou guarda de fechamento (`app/chamados/services/disputa_emissao.py:300`). Cada `P.abrir` possui atomic próprio (`app/chamados/models.py:2819`) e trava C (`app/chamados/models.py:2845`). A releitura final só protege push, não esse save nem os dias seguintes (`app/chamados/services/disputa_emissao.py:795`).

- **Complemento pode criar P local, registrar no coletor e sair sem push.** Complementa antes do filtro (`app/chamados/services/disputa_emissao.py:1200`), mas retorna quando todos os chamados ficaram cobertos (`app/chamados/services/disputa_emissao.py:1213`), antes de consumir o coletor (`app/chamados/services/disputa_emissao.py:1410`). Quando segue, só considera P da DS principal, perdendo complementos criados noutra D (`app/chamados/services/disputa_emissao.py:1410`).

- **Motivo em duas D continua possível e é intencional em teste.** `locais()` só recorta motivo, não o dia (`app/chamados/services/disputa_emissao.py:575`). Com P local antiga de mesmo motivo e P estrangeira do dia atual, persiste motivo em A e agrega em B (`app/chamados/services/disputa_emissao.py:643`). O teste exige motivo em ambas (`app/chamados/tests/test_bo_disputa_volta4_memoria.py:37`). Motivo previamente duplicado não é removido: ambos escritores fazem união (`app/chamados/services/disputa_emissao.py:549`, `app/chamados/services/disputa_emissao.py:593`).

Censo das portas:

| Caminho | Retorno e dona |
|---|---|
| Gerador conversa | Lê P/criada; registra criada e causa, mas não replay de conversa (`app/chamados/services/disputa_emissao.py:481`, `app/chamados/services/disputa_emissao.py:509`). A porta real de conversa sempre nasce, sem dedup estrangeiro (`app/chamados/models.py:2891`). |
| Gerador datado | Lê ambos, conserva argumentos de estrangeira quando `_reusos` existe (`app/chamados/services/disputa_emissao.py:488`). |
| Núcleo | Divide locais/estrangeiras; agrega dona aberta e não salva fechada (`app/chamados/services/disputa_emissao.py:634`, `app/chamados/services/disputa_emissao.py:544`). |
| Replay | Lê ambos; desfaz criada ou PK divergente (`app/chamados/services/disputa_emissao.py:706`). |
| `_criar_pergunta_se_couber` | Lê ambos; estrangeira retorna motivos anteriores e encaminha à dona aberta (`app/chamados/services/disputa_emissao.py:950`, `app/chamados/services/disputa_emissao.py:960`). |
| Complemento | Delega a esse helper com coletor (`app/chamados/services/disputa_emissao.py:999`). |
| Pendentes | Gerador lê ambos, mas consumidor não encaminha estrangeira (`app/chamados/services/disputa_emissao.py:296`). |
| Declaração colaborador/admin | Lê P, ignora criada e dona (`app/chamados/services/declaracao_texto.py:376`, `app/chamados/services/declaracao_texto.py:666`). |
| Âncora | Ignora retorno da conversa e envia push incondicional depois (`app/chamados/services/disputa_emissao.py:1494`, `app/chamados/services/disputa_emissao.py:1504`). Conversa real não devolve estrangeira (`app/chamados/models.py:2891`). |
| Reabertura administrativa | Ignora retorno de conversa; FCM próprio (`app/chamados/services/acoes_disputa.py:263`, `app/chamados/services/acoes_disputa.py:288`). |
| Fábrica | Wrapper e depois gerador público cujo retorno ignora (`app/chamados/services/fabrica_da_celula.py:172`). |
| Smoke | Núcleo e depois gerador público; pode usar `d=None` se nascimento recusado (`app/chamados/management/commands/smoke_chamados.py:99`, `app/chamados/management/commands/smoke_chamados.py:102`). |
| Grade | Helper `_disputa` removido; o comentário registra remoção (`app/ponto/management/commands/reconciliar_grade.py:412`). Ausência absoluta de chamadas por aliases: [nao verificado]. |

Travas:

- `_agregar_motivos_na_dona` não adquire trava; usa `get` simples e `save` (`app/chamados/services/disputa_emissao.py:546`). No núcleo, todas as DS do colaborador entram lazy antes da eleição (`app/chamados/services/disputa_emissao.py:621`), e a porta toma C antes de avaliar IDs e D antes de Ch/P (`app/chamados/models.py:2666`, `app/chamados/models.py:2671`, `app/chamados/models.py:2688`, `app/chamados/models.py:2692`).
- No agrupador, o fecho inclui D das perguntas e seus pais (`app/chamados/services/disputa_emissao.py:1085`, `app/chamados/services/disputa_emissao.py:1095`); lê antes, trava C, relê, trava conjunto inteiro, relê e recusa crescimento (`app/chamados/services/disputa_emissao.py:1121`). Assim a releitura intermediária antecede D/Ch/P, não inverte a ordem (`app/chamados/services/disputa_emissao.py:1123`).
- Declaração trava C, lê universo do fato e depois D/Ch/P (`app/chamados/services/declaracao_texto.py:299`). Isso protege ordem, mas não cura nascimento estrangeiro (`app/chamados/services/declaracao_texto.py:376`).
- `criada=True` estrangeira não ocorre na implementação real: True nasce com a D passada (`app/chamados/models.py:3014`, `app/chamados/models.py:3031`); reuso retorna False (`app/chamados/models.py:3008`). Portanto coletor do gerador não registra estrangeira como criada pela porta real (`app/chamados/services/disputa_emissao.py:509`).
- PostgreSQL, concorrência, signals completos e execução ORM: [nao verificado].

## 10/11 20:55 · sessao ce13aba8754d · BO-0611-05 · verificador · origem=Code

A retirada do bloqueio sem GPS está presente; os bloqueios por situação foram preservados antes da cobrança de afastamento e da porta de registro. Os dois kernels têm o mesmo código executável nesse trecho. (`app/api/views_core.py:402`, `app/api/views_core.py:435`, `app/api/views_core.py:658`; `app/api/views.py:415`, `app/api/views.py:448`, `app/api/views.py:666`)

A comparação com `6c0517eb9` abaixo usa o diff fornecido: esta cópia não contém histórico Git para conferir a base e a identidade do hash independentemente. **[nao verificado]**

1. **Recusas antes de `registrar_batida`**

   | Recusa atual | Core | UI | O que lê |
   |---|---|---|---|
   | 404, `Colaborador não encontrado.` | `app/api/views_core.py:391` | `app/api/views.py:404` | Acesso a `request.user.colaborador`; qualquer exceção nesse acesso cai no 404. |
   | 403, situação não permitida | `app/api/views_core.py:410` | `app/api/views.py:423` | `colab.situacao`; permite somente `ativo` e `afastado`. Escolhe mensagem/código para `desligado`, `ferias` ou outro valor. |
   | 400, `Tipo inválido.` | `app/api/views_core.py:426` | `app/api/views.py:439` | `request.data['tipo']`, obtido por `.get`; permite somente `E` e `S`. |

   Pelo diff da obra, os três retornos acima mantiveram posição, condição e texto da base. Saiu somente a guarda `if not latitude or not longitude`, com seu log `batida_recusada_sem_gps` e resposta `gps_obrigatorio`: na base fornecida, core condição em `app/api/views_core.py:440`, retorno em `:454`; UI condição em `app/api/views.py:449`, retorno em `:462`. O lugar passou a conter o comentário de encaminhamento à porta. (`app/api/views_core.py:435`; `app/api/views.py:448`; conferência independente da base **[nao verificado]**)

   Não houve deslocamento da guarda de situação em relação à cobrança: ela continua antes de `afastado_na_batida`. A volta 9 restaurou esse bloco em relação à volta 8; em relação à base fornecida, preservou-o. (`app/api/views_core.py:402`, `:411`; `app/api/views.py:415`, `:424`; comparação histórica independente **[nao verificado]**)

   Há também retorno antecipado `ja_registrada`, mas sem status de erro: ele não pertence ao conjunto solicitado de recusas ≥400. A colisão `ignorada_intervalo` acontece depois da chamada à porta. (`app/api/views_core.py:535`, `:658`, `:686`; `app/api/views.py:548`, `:666`)

   A proteção `IsAuthenticated` envolve as duas views; a tabela inventaria os retornos explícitos dentro delas, não as respostas produzidas pela autenticação do framework. (`app/api/views_core.py:385`; `app/api/views.py:398`)

2. **Ordem dos efeitos**

   | Ponto de saída | O que ocorreu antes |
   |---|---|
   | 404 | Tentativa de obter o colaborador; nenhum caminho de cobrança ou registro foi chamado pelo corpo da view. (`app/api/views_core.py:388`; `app/api/views.py:401`) |
   | 403 por situação | Obtenção do colaborador e leitura de situação; ainda não houve leitura do payload, cobrança de afastamento, detector de anomalias ou registro. (`app/api/views_core.py:402`, `:411`, `:419`; `app/api/views.py:415`, `:424`, `:432`) |
   | 400 por tipo | Já houve consulta ao juiz de afastamento e, quando positiva, tentativa de cobrar a supervisão; depois são lidos tipo, coordenadas, precisão e foto. (`app/api/views_core.py:411`, `:419`, `:425`; `app/api/views.py:424`, `:432`, `:438`) |

   A cobrança pode escrever **antes do 400**: procura Pauta viva; pode obter/criar o usuário sistema; cria Pauta, grava trilha e atualiza contador. Falha na cobrança é registrada e engolida. (`app/ponto/services/afastado_avisa.py:62`, `:71`, `:74`, `:80`; `app/core/utils.py:18`; `app/pautas/services.py:180`, `:186`, `:188`, `:352`; `app/core/observ.py:134`)

   Portanto, `situacao='ativo'` também pode gerar essa cobrança se uma ausência de afastamento cobrir o dia; `situacao='afastado'` sem ausência gera a divergência declarada. Coordenadas não participam dessa decisão. (`app/ponto/services/afastado_avisa.py:38`, `:41`; `app/ponto/turnos.py:2715`)

   Para tipo válido:

   | Situação | Com coordenadas | Sem coordenadas |
   |---|---|---|
   | `ativo` / `afastado` | Seguem cobrança, detector, demais preparações e porta. (`app/api/views_core.py:411`, `:433`, `:658`) | Agora seguem o mesmo percurso; o geofence é chamado depois da escrita. (`app/api/views_core.py:435`, `:658`, `:788`) |
   | `desligado` / `ferias` / outro | 403 antes desses efeitos. (`app/api/views_core.py:402`) | Mesmo 403; GPS nem é lido antes dele. (`app/api/views_core.py:410`, `:420`) |

   Pela base fornecida, a diferença está em `ativo`/`afastado` sem GPS: antes, cobrança e detector já tinham rodado; depois vinha o log da tentativa recusada e o 403. Agora esse log saiu e o fluxo continua. As demais ordens acima permaneceram. (`app/api/views_core.py:411`, `:433`, `:435`; `app/core/auditoria.py:22`; comparação histórica independente **[nao verificado]**)

3. **Quem escreve e quem lê**

   **Latitude/longitude da Batida:** as views capturam e encaminham; a porta sanitiza vazio/inválido para `None` e cria a Batida. Não inventa a coordenada faltante nem converte zero em uma coordenada alternativa. (`app/api/views_core.py:420`, `:664`; `app/api/views.py:433`, `:672`; `app/ponto/registro_batida.py:5`, `:105`, `:152`, `:157`)

   As escritas de coordenadas no geofence são de **AlertaGeofence**, não uma segunda escrita da Batida. O contexto do chamado também recebe cópias das coordenadas. (`app/ponto/services/geofence.py:271`, `:297`, `:304`, `:349`, `:391`)

   | Leitor afetado | Regra atual |
   |---|---|
   | Geofence | Consulta `tem_gps`, cujo resultado é `bool(latitude and longitude)`. (`app/ponto/services/geofence.py:145`, `:269`) |
   | Feed e offline | Filtro delega ao juiz. (`app/core/templatetags/hasner_filters.py:34`; `app/templates/core/_d2_feed.html:16`; `app/templates/ponto/batidas_offline_pendentes.html:96`) |
   | Relatório e modal | `par_tem_gps` delega ao mesmo juiz. (`app/core/templatetags/hasner_filters.py:41`; `app/templates/chamados/partials/_relatorio_geo.html:31`; `app/templates/chamados/partials/modal_fio.html:481`) |
   | Contador offline | Conta pares aprovados pelo juiz. (`app/ponto/views.py:485`) |
   | Três consultas de vínculo | Exigem não nulidade das duas coordenadas; **aceitam zero**, divergindo do juiz. (`app/colaboradores/views.py:337`, `:356`, `:395`) |
   | Detector de vínculo | Exige não nulidade e precisão; **aceita zero** como amostra. (`app/ponto/services/vinculo_na_lista.py:49`, `:56`; `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`) |

   Restam também regra própria no JS do dashboard, rejeição apenas de NaN no mapa e conversão individual de zero para `None` no emissor do contexto. São comportamentos diferentes entre si; não são consultas ao juiz. (`app/templates/core/dashboard2.html:242`, `:287`; `app/static/js/geo-colab-map.js:39`; `app/ponto/services/geofence.py:349`, `:391`)

   Esses remanescentes estão expressamente fora da migração desta volta no corte registrado; sua inscrição efetiva na fila externa não está disponível nesta cópia. (`app/docs/CORTES.md:442`; registro externo **[nao verificado]**)

   **Situação:** os kernels leem para barrar; o detector de vínculo exclui desligados e seleciona colegas ativos. Essas funções não escrevem situação. (`app/api/views_core.py:402`, `:407`; `app/api/views.py:415`, `:420`; `app/ponto/services/vinculo_na_lista.py:71`, `:83`, `:152`)

   Considerando os arquivos tocados inteiros, há escritores anteriores: cadastro cria `ativo`; lançamento de afastamento escreve `afastado`, ambos pela porta de Colaborador. O reversor escreve a volta para ativo pela mesma porta. (`app/colaboradores/views.py:1182`, `:1205`; `app/ponto/views.py:2435`; `app/colaboradores/services/colaborador.py:55`, `:91`; `app/ponto/management/commands/reverter_situacao_afastado.py:73`)

   Outros leitores nesses arquivos incluem refresh/me, universo de vínculo/geo e filtros de telas. (`app/api/views_core.py:173`, `:344`; `app/api/views.py:116`, `:355`; `app/ponto/services/geofence.py:46`; `app/ponto/views.py:376`, `:1119`, `:2225`, `:2373`; `app/colaboradores/views.py:206`, `:602`, `:1043`, `:1414`, `:1748`, `:2610`)

   Nos testes novos, a situação é preparada na criação do colaborador ou por UPDATE explícito para férias/desligado; isso é fixture, não escritor novo de produção. (`app/api/tests/test_l030_batida_pela_porta.py:33`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:518`, `:679`, `:756`)

4. **Mordida dos testes da porta** [nao verificado]

   O analisador AST reconhece somente `if` cuja condição leia os campos nominais procurados e contenha `return Response` com status literal 403 ou `HTTP_403_FORBIDDEN`, antes da primeira chamada nominal `registrar_batida`. Não acompanha aliases nem chamadas auxiliares. (`app/api/tests/test_l030_selo_porta.py:30`, `:50`, `:66`)

   | Teste | Pega | Não pega | [nao verificado]
   |---|---|---|
   | `PortaSemRecusaTest.test_MORDE_os_dois_kernels` | Guarda GPS reconhecida, inclusive `return` dentro de `try`. | Outro campo, status 400/422, helper, alias `sem_gps`, recusa por exceção; `raise` no `if` com resposta em `except` separado. (`app/api/tests/test_l030_selo_porta.py:104`, `:50`, `:66`) |
   | `PortaSemRecusaTest.test_mutantes_em_memoria_com_controle_verde` | Cinco formas: nomes diretos, `.get`, subscrito, `getattr`, condição mista situação/GPS. | Não executa os ramos mutados; não prova alcançabilidade nem formas não enumeradas. (`app/api/tests/test_l030_selo_porta.py:109`) |
   | `PortaSemRecusaTest.test_prosa_nao_e_barreira` | Evita falso positivo de comentário/docstring. | Não testa comportamento da produção; esse não é seu objeto. (`app/api/tests/test_l030_selo_porta.py:122`) |
   | `BarreiraDeSituacaoDeclaradaTest.test_MORDE_exatamente_uma_guarda_declarada_em_cada_kernel` | Guarda reconhecida removida/duplicada ou expressão/conjunto alterado. | Segunda recusa em helper, outro status ou exceção; ordem frente à cobrança; semântica do corpo. Mover a única guarda para helper reprova por ausência; acrescentar helper mantendo-a pode escapar. (`app/api/tests/test_l030_selo_porta.py:136`, `:160`) |
   | `…test_MORDE_os_codigos_da_barreira_sao_os_tres_da_base` | Falta de literal esperado ou presença de `colab_afastado`. | Associação código/situação trocada; mensagens erradas; código novo fora do conjunto comparado; literais preservados sem uso. (`app/api/tests/test_l030_selo_porta.py:165`) |
   | `BarreiraDeSituacaoDeclaradaTest.test_mutantes_em_memoria_com_controle_verde` | Quatro guardas adicionais, conjunto menor/maior e remoção. | Mesmos limites do analisador; não executa produção mutada. (`app/api/tests/test_l030_selo_porta.py:176`) |

   O teste dos códigos pode continuar verde com os literais numa docstring ou em código morto dentro da guarda: ele coleta constantes de string, não respostas efetivas. Já o selo GPS não fica verde simplesmente porque perdeu a porta: existe uma asserção exigindo encontrá-la. (`app/api/tests/test_l030_selo_porta.py:170`, `:70`)

   | Teste HTTP de `BatidaPelaPortaTest` | Pega | Não pega | [nao verificado]
   |---|---|---|
   | Sem coordenadas, ativo/afastado | Recusa que atinja essas fixtures, por return/helper/try/exceção; fallback; porta ausente/duplicada; persistência errada. | Recusa condicionada a campos, horários ou cadastros não exercitados; ordem de Pauta/log. (`app/api/tests/test_l030_batida_pela_porta.py:47`, `:59`) |
   | Desligado/férias/outro barrados | Status/código errado, porta chamada ou Batida gravada, com e sem GPS. | Helper equivalente; Pauta/log indevidamente anteriores ao 403; outras situações não ensaiadas. (`app/api/tests/test_l030_batida_pela_porta.py:65`) |
   | Zero numérico | Zero barrado/reescrito e juiz passando a aceitá-lo. | Zero textual ou somente uma coordenada zero. (`app/api/tests/test_l030_batida_pela_porta.py:78`) |
   | Campos vazios | Recusa, conversão antecipada ou persistência diferente de nulos. | Outros inválidos; apesar do nome “formulário”, envia JSON. (`app/api/tests/test_l030_batida_pela_porta.py:44`, `:88`) |
   | Coordenada parcial | Latitude isolada barrada/modificada ou longitude inventada. | Longitude isolada; resultado posterior do geofence. (`app/api/tests/test_l030_batida_pela_porta.py:93`) |

   Os caminhos aceitos usam `wraps` na função real e conferem chamada, banco e ID; não passam por um mock que fabrica sucesso. A autenticação é forçada, portanto login/refresh ficam fora. (`app/api/tests/test_l030_batida_pela_porta.py:40`, `:42`, `:49`)

   Nesta revisão, os seis testes AST passaram sem banco; os testes HTTP não foram reexecutados. O OK de `api ponto` foi informado pelo solicitante, sem log disponível para conferência independente. (`app/api/tests/test_l030_selo_porta.py:99`, `:133`; execução HTTP e log dos módulos **[nao verificado]**)

5. **Bateria férias/desligado**

   | Caso/teste | Caminho e afirmação atual | [nao verificado]
   |---|---|
   | Relógio congelado | Só afirma a data congelada. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:691`) |
   | “Controle paga…” | Só compara constantes `MEDIDO`; isoladamente não demonstra que produção alcança dinheiro. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:694`) |
   | C0 | API, tabela inteira: janela ligada, 8 h e 120 min pendentes; desligada, 10 h e HE50 de 2 h. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:704`, `:82`, `:118`, `:663`) |
   | F1 | Ausência de férias com situação **ativo**, pela API; tabela inteira. Ligada: 8 h; desligada: 10 h, HE50 de 2 h e reflexo DSR de 12 h; sete dias/56 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:711`, `:156`, `:193`, `:209`, `:216`) |
   | F2 | Ausência de férias com situação **ativo**, folga, pela API; tabela inteira. Ligada: pendência de 600 min e sem pagamento do dia; desligada: 10 h de folga trabalhada; oito dias/64 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:721`, `:239`, `:246`, `:276`, `:292`) |
   | F1s/F2s | Situação alterada para férias: primeiro 403/zero Batida nos dois kernels; depois porta real, comparando APURACAO de F1/F2 e idempotência. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:731`, `:740`, `:567`, `:665`) |
   | D1 | Célula preservada após desligamento por UPDATE; API barra, depois porta real. APURACAO espera 10 h, célula concorde/600 min, sem HE. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:749`, `:311`, `:322`) |
   | D2 | Desligado dentro da competência, sem célula; API barra, depois porta real. APURACAO espera 10 h em DiaPago/fechamento. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:762`, `:373`, `:384`) |
   | D3 | Demissão anterior à competência; API barra, depois porta real. Quatro batidas/600 min de relógio, sem DiaPago ou fechamento. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:769`, `:428`, `:434`, `:440`) |

   As quatro classes cobrem os dois kernels e janela ligada/desligada. O plantio usa `registrar_batida` real; a apuração chama cartório, fechamento e espelho reais. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:576`, `:593`, `:603`, `:778`)

   **Deixaram de ser confrontados com MEDIDO:** `porta` e valores de `admin` para D1/D2/D3; também não são confrontados para as variantes F1s/F2s. F1/F2 ativos continuam conferindo esses valores. O `admin` ainda participa da idempotência entre duas apurações, mas isso não afirma seus valores históricos. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`, `:656`, `:663`, `:665`, `:674`)

   O 403 da bateria exige código e zero Batida, mas não exige porta sem chamadas nem ausência de outros efeitos. `_numeros` omite zeros/nulos; o resumo omite valores falsy. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:567`, `:480`, `:635`)

   Não encontrei número **atualmente afirmado** demonstravelmente impossível por análise estática: D1/D2 têm percurso compatível nos dias órfãos do fechamento. Reproduzir cada número nesta árvore permanece **[nao verificado]**. Os antigos HTTP 200 dos desligados continuam na tabela, mas não são mais expectativas executadas nesses casos. (`app/ponto/services/fechamento.py:838`, `:902`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:328`, `:665`)

6. **Sobras e textos contraditórios**

   - “Única exceção… desligado” é falso: férias e outro valor também barram. É comentário restaurado da base fornecida, não prova de retirada esquecida. (`app/api/views_core.py:431`, `:402`; `app/api/views.py:444`, `:415`)
   - O cabeçalho de homologação diz afastado bloqueado, contrariando o próprio teste que exige 200; a descrição “férias bate” também é imprecisa para o caso que prepara situação férias e exige recusa. (`app/api/tests/test_api_bater_ponto.py:6`, `:28`, `:67`, `:84`, `:88`)
   - Textos amplos “férias nunca recusa” precisam distinguir **ausência de férias** de **situação férias**. Não demonstram ramo restante que libere a situação barrada: há testes de ausência com cadastro ativo e de chamada direta à porta. (`app/ponto/tests/test_batida_sideeffects.py:173`; `app/ponto/services/aviso_da_batida.py:7`; `app/ponto/tests/test_ferias_dia21.py:38`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:712`)
   - LEIS está coerente com os 403 mantidos. MAPA e MAPA-JUIZES registram `tem_gps`; o lookup JUIZES ainda não o inclui. (`app/docs/LEIS.md:78`, `:165`; `app/docs/MAPA.md:246`; `app/docs/MAPA-JUIZES.md:19`; `app/docs/JUIZES.md:19`)
   - Não encontrei sobra executável da liberação de desligado/férias nos kernels; ambos têm a guarda restaurada. Exaustividade semântica de todo texto da árvore **[nao verificado]**. (`app/api/views_core.py:402`; `app/api/views.py:415`)

7. **Catraca de linhas**

   Bate: UI registrada **728**, ocorrência real **728**; core registrada **723**, ocorrência real **723**. O teste específico de igualdade da catraca também passou em memória. (`app/core/tests/test_medida_turno_do_par.py:256`, `:257`, `:539`; `app/api/views.py:728`; `app/api/views_core.py:723`)

8. **Classes que já reprovaram obra**

   | Classe | Resultado |
   |---|---|
   | Segundo juiz | Os leitores migrados delegam; os leitores próprios remanescentes estão discriminados no item 3 e adiados pelo corte. (`app/core/templatetags/hasner_filters.py:34`, `:41`; `app/docs/CORTES.md:442`) |
   | Segundo escritor | O censo executado encontrou zero escritas **atribuíveis** da família Batida fora da porta; isso conserva os limites de atribuição do próprio censo. (`app/core/censo_escritas.py:6`, `:43`; `app/ponto/tests/test_chokepoint_batida_gate.py:26`) |
   | Relógio fora da borda | Ainda existem leituras internas: `date.today()` no afastamento e relógio no detector. Não são introduções mostradas pelo diff fornecido. (`app/ponto/turnos.py:2730`; `app/chamados/detector_anomalias.py:73`; `app/ponto/utils_turno_orfao.py:22`) |
   | Derivação paralela | Permanece nos leitores de coordenada discriminados; não apareceu nos novos filtros delegadores. (`app/colaboradores/views.py:337`; `app/core/templatetags/hasner_filters.py:34`) |
   | Selo/RED sem mordida | Há mordida para formas e fixtures enumeradas; não há cobertura universal de recusa. O “anti-vacuidade” da bateria só lê constantes. (`app/api/tests/test_l030_selo_porta.py:66`; `app/api/tests/test_l030_batida_pela_porta.py:59`; `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:694`) |
   | Ordem de trava | Situação continua antes da cobrança; os testes HTTP não selam ausência de Pauta/log antes do 403. (`app/api/views_core.py:402`, `:411`; `app/api/tests/test_l030_batida_pela_porta.py:73`) |
   | Pouso antes de pré-requisito | A regra exige suite inteira verde e depois verificação final. Só os módulos foram informados como OK; cumprimento final **[nao verificado]**. (`CLAUDE.md:374`) | [nao verificado]
   | Campo com dois significados | Ausência de férias e `situacao='ferias'` têm efeitos distintos; a bateria agora o

[truncado em 20000 de 21968 caracteres]

## 10/11 20:53 · sessao 892be02b5445 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Cura B1–B4 entregue, com comandos, limites e pergunta h2 sem resposta no [relato](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/RELATO_BO_DISPUTA_VOLTA5B.txt:1).

[Execuções locais registradas](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/RELATO_BO_DISPUTA_VOLTA5B_VERIFICACOES.txt:1): 66 testes em memória, 35 da catraca e 20 mutantes mortos. Execução ORM e tetos SQL: [nao verificado]. [nao verificado]

perguntas de lei: 1

## 10/11 20:52 · sessao 67fdd29f7bf8 · - · - · origem=Ronald (tmux)

Itens 3, 6, 8 e 9 — achados e censo:

- Juiz único implementado: `app/ponto/services/geofence.py:126` define `tem_gps`; `:145` devolve `bool(latitude and longitude)`; `:269` é o consumidor no geofence. O registro vivo contém a autoridade em `app/core/juizes.py:1201`, com corte em `app/docs/CORTES.md:442`.
- Leitores migrados delegam: `app/core/templatetags/hasner_filters.py:34` e `:41`; templates `app/templates/core/_d2_feed.html:16`, `app/templates/ponto/batidas_offline_pendentes.html:96`, `app/templates/chamados/partials/_relatorio_geo.html:31`, `app/templates/chamados/partials/modal_fio.html:481`; contador em `app/ponto/views.py:485`.
- Permanecem leitores com regra própria: nulidade em `app/colaboradores/views.py:337`, `:356`, `:395` e `app/ponto/services/vinculo_na_lista.py:49`; também `app/ponto/services/geofence.py:52`. Aceitam zero numérico onde `tem_gps` nega. A autorização de manter leitores do censo fora da migração está documentada em `app/docs/CORTES.md:442`; a presença efetiva deles no BACKLOG externo é `[nao verificado]`.
- `app/ponto/services/geofence.py:349`, `:350`, `:391`, `:392` transformam cada zero em `None` ao formar contexto. `app/templates/core/dashboard2.html:242` e `:287` repetem truthiness; `app/static/js/geo-colab-map.js:39` só rejeita NaN, aceitando zero. São dívidas existentes, não alterações demonstradas no diff fornecido.
- Um teste novo fixa explicitamente zero como amostra numérica no detector: `app/ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:135`–`:140`. Isso diverge do juiz, mas o ramo de produção correspondente já aceitava zero pelo filtro de nulidade; o diff fornecido acrescenta apenas a exigência de longitude. Não atribuir introdução dessa regra à obra.
- Escritor de `Batida.latitude/longitude`: `app/ponto/registro_batida.py:105` sanitiza e `:152`/`:157` criam com `**campos`; a sanitização converte vazio/inválido em `None`, preservando zero em `:5`–`:18`. Kernels só capturam e encaminham: `app/api/views_core.py:420`, `:421`, `:664`, `:665`; `app/api/views.py:433`, `:434`, `:672`, `:673`. Escritas do geofence em `:275`, `:297`, `:304`, `:315`, `:324` são de **AlertaGeofence**, não Batida.
- Situação, nas funções alteradas: kernels leem em `app/api/views_core.py:402`, `:407`, `:408` e `app/api/views.py:415`, `:420`, `:421`; detector lê em `app/ponto/services/vinculo_na_lista.py:71`, `:83`, `:152`. Nenhuma dessas funções escreve situação.
- Situação, considerando arquivos inteiros tocados: `app/colaboradores/views.py:1182` cria colaborador com `situacao='ativo'` em `:1205`, pela porta `app/colaboradores/services/colaborador.py:91`–`:93`; `app/ponto/views.py:2435` escreve afastado pela porta `app/colaboradores/services/colaborador.py:55`–`:61`. A bateria escreve férias diretamente por UPDATE em `app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:679` e desligado em `:758`; são preparação de teste.
- Leitores adicionais de situação nos arquivos tocados: refresh/me em `app/api/views_core.py:173`, `:344`, `app/api/views.py:116`, `:355`; geofence em `app/ponto/services/geofence.py:46`; filtros e contadores em `app/ponto/views.py:376`, `:1119`, `:1120`, `:1121`, `:2225`, `:2373`; lista/cadastro/vínculo em `app/colaboradores/views.py:33`, `:62`, `:66`, `:206`, `:602`, `:619`, `:884`, `:1043`, `:1045`, `:1414`, `:1448`, `:1540`, `:1689`, `:1748`, `:1770`, `:1831`, `:2610`, `:2846`, `:2856`, `:2896`, `:2912`.

Sobras:

- Comentário **falso, restaurado da base**: “Única exceção … desligado” em `app/api/views_core.py:431` e `app/api/views.py:444`; férias e outro valor também barram em `:402`/`:415`.
- Cabeçalhos antigos dizem afastado bloqueado em `app/api/tests/test_api_bater_ponto.py:6` e `:28`, contrariando o próprio teste `:67`–`:75`. `:84`–`:85` diz genericamente que férias bate, embora esse caso específico use situação férias e espere recusa em `:86`–`:90`.
- Textos amplos “nunca recusa” permanecem em `app/ponto/tests/test_batida_sideeffects.py:173`, `app/ponto/services/aviso_da_batida.py:7` e `app/ponto/tests/test_gate_ferias_vira_aviso.py:7`. Não demonstram remoção esquecida: férias como ausência com cadastro ativo continuam entrando (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:712`, `:722`); a porta direta também aceita (`app/ponto/tests/test_ferias_dia21.py:38`–`:42`).
- `app/docs/LEIS.md:78` e `:165` estão coerentes com as barreiras mantidas.
- `app/docs/JUIZES.md:19`–`:121` continua sem `tem_gps`, embora `app/docs/MAPA-JUIZES.md:19` e `app/docs/MAPA.md:246` já o registrem. O opus fornecido já condicionava corrigir esse lookup no pouso.
- MAPA-DA-FILA-BO e `logs/MAPA-FILA-BO/mapa.r` não estão disponíveis nesta cópia: registro do complemento `[nao verificado]`. `app/docs/BACKLOG.md:1873` ainda descreve a obra original em censo; isso não prova ausência do registro externo.

Proibidos: não observei introdução de fallback, nova recusa, mudança do classificador ou aceitação de zero no juiz; o comportamento visível é encaminhar coordenadas à porta e consultar o mesmo predicado (`app/api/views_core.py:664`, `app/api/views.py:672`, `app/ponto/services/geofence.py:145`, `:269`, `:292`). A integridade de `motor_calculo_v2.py` contra a base depende do diff fornecido, pois esta cópia não contém `.git`: comparação independente `[nao verificado]`.

## 10/11 20:52 · sessao 96e8efe466d2 · - · - · origem=Ronald (tmux)

Item 4 — leitura estática e execução sem banco:

| Teste | O que morde | O que não morde | [nao verificado]
|---|---|---|
| `PortaSemRecusaTest.test_MORDE_os_dois_kernels` | `if` anterior à porta que lê `latitude`/`longitude` e contém `return Response(...,status=403)`; inclusive dentro de `try`. | Outro campo, status400/422, status em variável, alias `sem_gps`, função auxiliar, `raise PermissionDenied`, ou return no `except` separado do `if`. O detector só reconhece essas formas nominais. (`app/api/tests/test_l030_selo_porta.py:50-79`, `:104-107`) |
| `PortaSemRecusaTest.test_mutantes_em_memoria_com_controle_verde` | Cinco mutantes AST por kernel: nomes diretos, `.get`, subscrito, `getattr`, condição mista férias/GPS. | Não executa a view mutada; prova a sensibilidade do analisador a essas cinco formas, sem provar alcançabilidade dos ramos. (`app/api/tests/test_l030_selo_porta.py:109-120`) |
| `PortaSemRecusaTest.test_prosa_nao_e_barreira` | Evita falso positivo em docstring/comentário do exemplo. | Não verifica produção nem comportamento de recusa. Não é vacuidade: seu objeto declarado é distinguir prosa de AST executável. (`app/api/tests/test_l030_selo_porta.py:122-130`) |
| `BarreiraDeSituacaoDeclaradaTest.test_MORDE_exatamente_uma_guarda_declarada_em_cada_kernel` | Remoção da guarda reconhecida, segunda guarda reconhecida ou mudança da expressão canônica/conjunto. | Segunda recusa escondida em helper, alias, exceção, status diferente; ordem relativa a cobrança/pauta; semântica do corpo. Mover toda a guarda para auxiliar faz faltar a guarda e reprova; acrescentar auxiliar mantendo a guarda pode escapar. (`app/api/tests/test_l030_selo_porta.py:66-90`, `:136-163`) |
| `...test_MORDE_os_codigos_da_barreira_sao_os_tres_da_base` | Ausência de qualquer um dos três literais esperados e presença de `colab_afastado` na guarda. | Troca da associação código/situação, mensagem errada, código novo fora do pequeno conjunto comparado, literais esperados conservados em código morto. Verifica conjunto de strings, não saída real. (`app/api/tests/test_l030_selo_porta.py:165-174`) |
| `BarreiraDeSituacaoDeclaradaTest.test_mutantes_em_memoria_com_controle_verde` | Quatro segundas guardas diretas, conjunto menor/maior, guarda removida. | Os mesmos escapes do detector; não executa os mutantes de produção. (`app/api/tests/test_l030_selo_porta.py:176-194`) |
| `BatidaPelaPortaTest.test_MORDE_sem_coordenadas_ativo_e_afastado_gravam_nos_dois_kernels` | Reintroduzir recusa que atinja ativo/afastado sem GPS, seja por return, helper, try ou exceção; fallback/coordenada alterada antes da porta; porta não chamada/duplicada; ausência ou duplicação da batida. | Condições de recusa que não incidam na fixture — aparelho, valores adicionais, foto presente, outro horário etc.; efeitos anteriores como Pauta/log. (`app/api/tests/test_l030_batida_pela_porta.py:31-63`) |
| `...test_MORDE_desligado_ferias_e_outro_seguem_barrados_antes_da_porta_nos_dois_kernels` | Remover barreira, trocar status/código, chamar porta antes de recusar, gravar Batida, com e sem GPS, nas três situações. | Mover a recusa para helper conservando resultados; criar Pauta/log antes do403; nova barreira para outra situação/fixture. (`app/api/tests/test_l030_batida_pela_porta.py:65-76`) |
| `...test_MORDE_zero_numerico_chega_intacto_ao_escritor` | Barrar/reescrever zero, perder persistência, juiz considerar zero válido. | Zero textual, só uma coordenada zero, outras combinações/condições não exercitadas. (`app/api/tests/test_l030_batida_pela_porta.py:78-86`) |
| `...test_MORDE_campos_vazios_do_formulario_sao_sanitizados_pela_porta` | Barrar vazios, convertê-los antes da porta, não gravá-los como `None`. | Outros valores inválidos e regras para outros formatos de request; envia JSON apesar do nome “formulário”. (`app/api/tests/test_l030_batida_pela_porta.py:44`, `:47-55`, `:88-91`) |
| `...test_caracteriza_coordenada_parcial_sem_inventar_a_outra` | Barrar latitude isolada ou inventar longitude; modificar latitude antes da porta. | Longitude isolada e geofence posterior; testa só ativo e uma orientação do par. (`app/api/tests/test_l030_batida_pela_porta.py:93-96`) |

Os cinco testes HTTP usam `wraps=registrar_batida`, POST real e checam persistência/ID nos caminhos aceitos; não passam por “mock que fabricou batida”. A autenticação é forçada, então login/refresh e middleware de autenticação não são cobertos. (`app/api/tests/test_l030_batida_pela_porta.py:39-56`)

O selo AST pode dar verde com barreira real escondida: demonstrei em memória os escapes acima. O próprio algoritmo explica a limitação: não acompanha fluxo entre funções nem aliases e só busca retorno403 dentro de guarda `if` cuja condição leia campo nominal. (`app/api/tests/test_l030_selo_porta.py:50-79`)

Executado: seis testes AST, `OK`, rc0; não executei os testes HTTP com banco nesta tarefa. `[nao verificado: execução HTTP nesta revisão]` (`app/api/tests/test_l030_selo_porta.py:99`, `:133`)

Item 7 — bate:

- `api/views.py`: registrada728, ocorrência real728. (`app/core/tests/test_medida_turno_do_par.py:256`; `app/api/views.py:728`)
- `api/views_core.py`: registrada723, ocorrência real723. (`app/core/tests/test_medida_turno_do_par.py:257`; `app/api/views_core.py:723`)
- Executei em memória o teste específico de igualdade de linhas da árvore inteira: um teste, `OK`, rc0. A asserção compara todo `LINHAS_REGISTRADAS` com o censo real. (`app/core/tests/test_medida_turno_do_par.py:20-28`, `:539-543`)

Item 8 — limite legado adicional: selo modal verifica apenas presença textual de `naoBloquear` quando encontra `hxConfirmar(`; comentário com a palavra já satisfaz essa parcela. Não demonstra que o fluxo efetivamente segue quando modal falha. (`app/core/tests/test_selo_modal_nao_nasce_vazio.py:135-143`)

## 10/11 20:52 · sessao efb82ef2f5e2 · - · - · origem=Ronald (tmux)

Item 5 — auditoria estática, sem banco e sem executar a bateria.

O arquivo declara caracterização do efeito de retirar as barreiras, não uma regra de pagamento; a tabela veio da volta 8 e os casos barrados agora usam a função real de registro. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:3`, `:11`, `:576`)

| Teste | Caminho e afirmações atuais | [nao verificado]
|---|---|
| `test_relogio_congelado` | Só verifica a data congelada; nenhuma batida ou cálculo. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:691`) |
| `test_MORDE_o_controle_paga_e_os_casos_nao_sao_o_controle` | Só compara constantes da própria tabela: C0 ligado = 8 h/120 min pendentes; desligado = 10 h/2 h HE50; casos diferentes do controle; flags históricas de idempotência verdadeiras. Não chama produção e sozinho não prova que a fixture alcança dinheiro. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:694`) |
| C0 | Ativo, sem ausência, célula de trabalho; quatro POSTs pela API e comparação integral com MEDIDO. Espera 8 h e pendência de 120 min com janela ligada; 10 h/HE50 2 h com janela desligada. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:704`, `:661`, `:82`, `:118`) |
| F1 | Ausência de férias lançada mantendo `situacao='ativo'`; célula existe; quatro POSTs pela API e tabela integral. Espera `fato_em_ausencia`, 8 h com janela ligada; 10 h/HE50 2 h e reflexo DSR 12 h com janela desligada; sete dias/56 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:711`, `:156`, `:193`, `:209`, `:216`) |
| F2 | Mesmo cadastro ativo com ausência de férias, mas célula de folga; API e tabela integral. Ligada: pagamento do dia ausente, pendência `dia=600`; desligada: 10 h de folga trabalhada; oito dias/64 h de abono. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:721`, `:239`, `:246`, `:276`, `:292`) |
| F1s | F1 com `situacao='ferias'` por UPDATE: primeiro quatro respostas 403/`colab_ferias` e zero batidas em **cada** kernel; depois quatro registros pela porta real; compara APURACAO de F1 e idempotência. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:731`, `:676`, `:567`, `:665`) |
| F2s | Idem para folga, comparando APURACAO de F2. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:740`, `:665`) |
| D1 | Gera célula enquanto ativo; encerra vínculo e muda situação por UPDATE, preservando célula; API 403/zero, depois porta real. Espera célula concorde/600 min, DiaPago e fechamento com 10 h, sem HE nem pendência. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:749`, `:311`, `:322`) |
| D2 | Já nasce desligado, demissão dentro da competência; não tem célula no dia; API 403/zero, depois porta real. Espera DiaPago e fechamento com 10 h, célula e retenção ausentes. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:762`, `:373`, `:384`) |
| D3 | Demissão anterior à competência; sem célula; API 403/zero, depois porta real. Espera quatro batidas, relógio de 600 min no espelho, mas sem DiaPago/fechamento e zero trabalhadas. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:769`, `:428`, `:434`, `:440`) |

As quatro classes executam esses testes nos dois kernels e nas duas configurações da janela; os casos barrados ainda percorrem os dois kernels internamente, independentemente da classe. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:778`, `:786`, `:567`)

A porta real recebe os quatro horários congelados, `origem='app'`, canal PWA, sequência real e coordenadas nulas, e o teste exige quatro registros sem conflito. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:576`) A apuração chama cartório, fechamento e espelho reais; não há mock do motor nesse percurso. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:593`, `:603`)

**O que deixou de ser afirmado:** para D1/D2/D3, nenhum teste compara com MEDIDO os valores históricos de `porta` e `admin`: HTTP 200, disputa, motivos, contagens de alertas/perguntas/chamados/Pauta etc. F1/F2 ainda comparam esses valores pela API; F1s/F2s não os comparam para a situação férias. A exclusão é explícita e delimitada por APURACAO. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:52`, `:661`, `:665`, `:674`) Entretanto, `admin` ainda participa da comparação **entre as duas apurações**, porque `idempotente = um == dois` compara todo o dicionário medido. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:656`, `:673`)

**Limites dos testes:** `force_authenticate` contorna o login; o 403 verifica código e zero Batida, mas não verifica que a porta ficou sem chamadas nem que outros efeitos ficaram ausentes. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:547`, `:567`) `_numeros` omite zeros e nulos, e `resumo` omite valores falsy; nessas saídas, não distinguem zero de `None`. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:480`, `:635`)

**Número impossível:** não encontrei contradição estática que demonstre número **afirmado** impossível. Os 10 h de D1/D2 têm percurso de produção compatível: dias órfãos fora da vigência são somados às trabalhadas, e D3 fica fora do universo pela demissão anterior à janela. (`app/ponto/services/fechamento.py:792`, `:838`, `:902`, `:431`) Reproduzir numericamente cada tabela nesta árvore permanece **[nao verificado]**. As antigas respostas 200 dos casos desligados continuam armazenadas em MEDIDO, mas já não são expectativas executadas nesses casos. (`app/ponto/tests/test_bateria_bo0611_05_ferias_desligado.py:328`, `:665`)

## 10/11 20:37 · sessao a613d79df0b4 · BO-DISPUTA-SEM-PERGUNTA · construtor · origem=Code

Delta aplicado em cinco arquivos: [nao verificado]

- **Item 1:** `core/tests/test_medida_turno_do_par.py::LINHAS_REGISTRADAS`: somente duas entradas mudaram: `_orfao_resolvido_cross_meianoite`, `[809, 810] → [815, 816]`; `criar_disputa_a_partir_de_chamados_orfaos`, `[1293] → [1299]`. [Registro](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:267)
- **Item 2:** `chamados/tests/test_bo_disputa_volta5.py::test_RED_d2_aberta_com_outros_motivos_e_canonica_em_fechada_duas_vezes`: `primeiro_estado` definido antes do laço; a comparação da segunda repetição permanece. [Código](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5.py:198)
- **Item 3:** `chamados/services/disputa_emissao.py::perguntar_entradas_pendentes` registra explicitamente a chamada ao gerador; `::criar_ou_atualizar_disputa` usa esse sinal para decidir a segunda releitura. [Escritor do sinal](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:294), [leitor e comentário corrigido](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:779)

Não provei que todas as cascatas sejam impossíveis quando o gerador devolve vazio. A cadeia concreta de fechamento passa por `reatar_pergunta_orfa` → `lavrar_sem_turno_aberto` → `p.disputa.fechar`; portanto, a dispensa agora depende da ausência de chamada, não do retorno vazio. [Gerador](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/disputa_emissao.py:497), [lavra](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/reconciliador.py:1619), [fechamento](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/reconciliador.py:411)

O RED novo executa pendentes real, simula fechamento por outra instância no gerador e retorno vazio, e exige **zero push**. O mutante que restaura a decisão da c8 falha justamente por emitir push. [Teste e mutantes](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta8_memoria.py:183) [nao verificado]

Mantidos **149/223**, com comentários atualizados em `ponto/tests/test_selo_chokepoint_escrita.py::TETOS`. Previsão: delta zero, pois a fábrica fornece data sem entradas pendentes. Nascimento pula pendentes; agregação sem chamada ao gerador conserva uma releitura. Medida ORM e identificação dos ramos efetivamente percorridos em cada cenário: **[nao verificado]**. [Fábrica](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/services/fabrica_da_celula.py:172), [tetos](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/ponto/tests/test_selo_chokepoint_escrita.py:186)

Controles locais passaram: volta8 **8**, volta5 **15**, sem_pergunta **14** e catraca **35** testes. Os sete mutantes da volta8, `sem_guarda_push` e o registro com linhas antigas morreram por asserção. [Runner c9](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta8_memoria.py:258), [runner volta5](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/chamados/tests/test_bo_disputa_volta5_memoria.py:382), [catraca](/home/ronald/wt-codex-bo-disputa-sem-pergunta-165646/app/core/tests/test_medida_turno_do_par.py:540) [nao verificado]

Comandos de memória e de cada mutante:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta8_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_volta5_memoria.py
PYTHONDONTWRITEBYTECODE=1 python3 -B chamados/tests/test_bo_disputa_sem_pergunta_memoria.py

for mutante_c9 in \
  reconsulta_sem_pendentes ignora_fecho_gerador ignora_fecho_pendentes \
  estrangeira_em_a salva_fechada_d2 omite_sinal_gerador_vazio \
  retorno_vazio_dispensa_releitura
do
  PYTHONDONTWRITEBYTECODE=1 python3 -B \
    chamados/tests/test_bo_disputa_volta8_memoria.py --mutante "$mutante_c9" || exit 1
done

PYTHONDONTWRITEBYTECODE=1 python3 -B \
  chamados/tests/test_bo_disputa_volta5_memoria.py --mutante sem_guarda_push
```

Catraca completa e mutante das linhas antigas, sem Django/banco:

```bash
PYTHONDONTWRITEBYTECODE=1 python3 -B - <<'PY'
from pathlib import Path
from types import ModuleType
import unittest

p = Path('core/tests/test_medida_turno_do_par.py').resolve()
suite = ModuleType('suite_turno') [nao verificado]
suite.__file__ = str(p) [nao verificado]
source = p.read_text().replace(
    'from django.test import SimpleTestCase',
    'from unittest import TestCase as SimpleTestCase')
exec(compile(source, str(p), 'exec'), suite.__dict__) [nao verificado]
runner = unittest.TextTestRunner(verbosity=2)
if not runner.run(unittest.defaultTestLoader.loadTestsFromModule(suite)).wasSuccessful(): [nao verificado]
    raise SystemExit(1)

linhas = suite.LINHAS_REGISTRADAS['derivacoes_de_dia'] [nao verificado]
linhas[('chamados/services/disputa_emissao.py::_orfao_resolvido_cross_meianoite', 'B1')] = [809, 810]
linhas[('chamados/services/disputa_emissao.py::criar_disputa_a_partir_de_chamados_orfaos', 'B1')] = [1293]
resultado = runner.run(unittest.TestSuite([
    suite.CatracaTurnoDoParTest( [nao verificado]
        'test_CATRACA_linhas_reais_congeladas_exigem_registro_no_mesmo_commit')]))
if resultado.errors or len(resultado.failures) != 1:
    raise SystemExit('mutante nao morreu por assertion')
print('MUTANTE MORTO: linhas antigas')
PY
```

Ruff indisponível no ambiente; lint, testes com banco, medidas ORM e certificação pela suite/opus ficam **[nao verificado]**. [nao verificado]

_3512 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
