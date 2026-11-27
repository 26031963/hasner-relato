# CODEX-ESTADO

> Ultima resposta processada: **27/11/2026 12:33** · publicado em **27/11/2026 12:33** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 27/11 12:33 · sessao f4c8c5fec76b · FERIADO-UM-JUIZ · construtor · origem=Code

A cura do **FERIADO-UM-JUIZ c1** está na cópia. O HEAD segue em `59600163ccc9cffb8f3ae148beede7cd7866ae86`. A proibição explícita de `git commit` prevaleceu sobre o item (iv). [Diff completo](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz.patch:1) · [Log:411](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:411)

### Cura

| Arquivo e funções | Mudança |
|---|---|
| `fechamento.py::apurar_por_fatia`, `_particionar_ft`, `recalcular_fechamento_mes` | Cada período usa o motor do próprio vínculo e o dia de início da jornada. A partição soma a HE100 de feriado uma vez, com o subconjunto noturno, e alimenta as duas lavras. [fechamento.py:312](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:312) · [fechamento.py:349](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:349) · [fechamento.py:988](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/services/fechamento.py:988) |
| `regras.py::_ContextoFeriado.__init__`, `dobra_de_feriado_do_dia`, `intermitente_do_tipo`, `rubrica_da_folga`, `do_dia` | A regra compartilhada exclui o intermitente e usa a escala certa para escolher a rubrica. A ampliação preserva o 12x36 e o tratamento BO-B(3) da escala certa. [regras.py:297](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:297) · [regras.py:366](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:366) · [regras.py:690](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/regras.py:690) |
| `alimentacao.py::_rubricas_da_particao_ft`, `rubricas_por_dia` | O oráculo projeta a partição do fechamento por dia e conserva a parcela dos períodos normais em dias com FT. [alimentacao.py:353](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/alimentacao.py:353) · [alimentacao.py:557](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/calculador/alimentacao.py:557) |

### REDs e mutantes

Na base, o 6x1 sem escala certa entregou **0 h de HE100 feriado** em vez de 4 h. O noturno entregou **0 h** em vez de 3 h. O intermitente recebeu **4 h de HE100 feriado** em vez de zero. Na cura, **16 testes em memória passaram**. O diff tem **0 cláusulas `except` adicionadas e 0 removidas**. [Log:19](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:19) · [Log:27](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:27) · [Log:37](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:37) · [Log:66](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:66) · [Log:410](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:410) [nao verificado]

Os **12 mutantes** falharam por asserção. Cada link abre o comando completo e o resultado correspondente. [Arquivo de mutantes](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_mutantes.py:29)

| O que o teste pega | Comandos e resultados | [nao verificado]
|---|---|
| Escala certa volta a impedir a dobra do 6x1 | [`exige_escala_certa`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:72) |
| Intermitente ganha HE100 feriado | [`intermitente_em_ft`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:99) · [`intermitente_no_oraculo`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:130) |
| Calendário nacional ou motor da última fatia toma o lugar do contexto do período | [`calendario_nacional`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:157) · [`aplica_motor_da_ultima_fatia`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:184) · [`perde_contexto_da_primeira_fatia`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:220) |
| Segundo par da véspera dobra pela data civil | [`aplica_dia_civil`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:246) |
| FT do 12x36 ganha HE100 feriado | [`promove_12x36`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:277) · [`promove_12x36_no_oraculo_avulso`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:308) |
| A promoção perde o noturno ou o oráculo perde uma parcela do pagamento | [`perde_noturno`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:333) · [`oraculo_copia_so_folga`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:358) · [`oraculo_perde_periodo_normal`](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/feriado_um_juiz_red.txt:383) |

### Fora desta volta

Os testes de persistência para LIMBO, A2, A8, noturno partido e véspera ficaram para o Code. Eles comparam o mensal com a soma das duas lavras e conferem o evento de folha e os leitores do dia. O teste legado manteve o controle 12x36 e ganhou o 6x1 com o aval citado. A execução desses testes com banco e a certificação Code/Opus ficam **[nao verificado]**. [Teste da porta:58](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_feriado_um_juiz_porta.py:58) · [Teste legado:227](/home/ronald/wt-codex-feriado-um-juiz-121621/app/ponto/tests/test_he100_dia_sem_previsto.py:227) [nao verificado]

O anexo registra **75 dias de 42 colaboradores** como hora comum e projeta **67 dias de 35 colaboradores**, somando **449,74 h**, para HE100 feriado. O apply da 11, a reversão, o antes × depois por colaborador e o diff do TXT ficam **[nao verificado]**. `antes_m1.md:3`

## 27/11 12:32 · sessao 19ca023221ab · FERIADO-BOTAO-CORTADO · construtor · origem=Code

**FERIADO-BOTAO-CORTADO c4.** A correção está na cópia. `posiciona()` mede o painel em `left/top = 0` e após um deslocamento de 64 px. Com essas leituras, converte a posição desejada na viewport para a origem e a escala reais do `fixed`, tanto sob o `backdrop-filter` da página quanto sob o `transform` do drawer. [Popover](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:81) · [`.hx-shell`](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/css/hasner-ponto.css:275) · [Drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:3)

O encaixe usa a interseção da viewport, com 8 px de margem, com a área útil dos ancestrais que recortam o painel. Se faltar altura, o painel fica no lado com mais espaço e rola por dentro. O formulário permanece no `details`. A mudança de produto está no JS compartilhado. [Área visível](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:48) · [Encaixe](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:109) · [Rolagem](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:169)

### Diff e smoke

- `red_painel_popover_feriado.py` foi removido após a incorporação das asserções ao smoke coletado. Ele mede margem, recorte por hit-test e distância de até 16 px sem sobreposição nos quatro cenários do feriado. O quinto cenário abre **Inverter tipo/Retratar** no drawer de `base_app.html`, limitado a 260 px no ensaio, e verifica também que a rolagem interna mantém o painel aberto. [Asserções](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:143) · [Cenários](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:302)
- O selo existente ficou como está. Ele exige posicionamento por medida e testa a geometria na última coluna em duas larguras. [Selo](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:104) · [Geometria](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:166)

```bash
git --no-optional-locks diff a2414b6605ff01965417728ccb271aa82a294f04 -- \
  static/js/popover-acao.js \
  core/tests/test_smoke_clique_feriado_vira_trabalho.py \
  core/tests/red_painel_popover_feriado.py
```

### Comandos para a sessão de Chromium

Na pista isolada, com `STATIC_ROOT` atualizado. Os mutantes de JS alteram somente a cópia do script carregada pela página temporária. [Carregamento de estáticos](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_chromium.py:80) · [Injeção](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:260)

```bash
smoke() {
  PYTHONDONTWRITEBYTECODE=1 python manage.py test \
    core.tests.test_smoke_clique_feriado_vira_trabalho "$@" \
    --settings=config.settings.ci --noinput
}

SMOKE_FERIADO_MUTANTE= smoke core.tests.test_selo_popover_acao
SMOKE_FERIADO_MUTANTE=gatilho-cortado smoke
SMOKE_FERIADO_MUTANTE=summary-display-none smoke
SMOKE_FERIADO_MUTANTE=porta-trocada smoke
SMOKE_FERIADO_MUTANTE=painel-recortado smoke
SMOKE_FERIADO_MUTANTE=painel-distante smoke
SMOKE_FERIADO_MUTANTE=painel-sem-compensacao smoke
SMOKE_FERIADO_MUTANTE=painel-sem-limite-drawer smoke
```

| Mutante | Asserção esperada no RED |
|---|---|
| `gatilho-cortado`, `summary-display-none` | `gatilhoVisivel`. [Sonda](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:115) |
| `porta-trocada` | `action`. [Troca da porta](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:291) |
| `painel-recortado` | `painelSemRecorte`. [Hit-test](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:148) |
| `painel-distante`, `painel-sem-compensacao` | `painelEncostado`. [Posição sem compensação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:46) · [Distância](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:143) |
| `painel-sem-limite-drawer` | `painelSemRecorte` e `painelRolavel` no cenário da marcação. [Mutante](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:53) · [Sonda](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:174) |

Validei a sintaxe Python e JS e executei isoladamente as quatro verificações estruturais do selo. Em DOM simulado, o mutante de compensação perdeu a ancoragem na página e no drawer, e o mutante do limite voltou a recortar o painel no drawer curto. [Selo estrutural](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_selo_popover_acao.py:74) · [Compensação](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:95) [nao verificado]

**[nao verificado]** Smoke e mutantes no Chromium, casos geométricos do selo, `bin/node_check.sh`, suite do Code, revisão final do Opus e smoke de tela do Ronald.

## 27/11 12:25 · sessao 85323386be77 · E4C-F5-INSTANTE · verificador · origem=Code

**Cura4 reprovada pelo aval literal.** Na resposta comum, o `post_save` emite e `DecisaoPergunta.registrar` suprime a própria emissão. A contagem fica em um, mas o aval atribui a emissão ao `registrar`. Há ainda um contraexemplo em que uma decisão nova pode gravar **zero eventos** ao consumir um recibo da transação anterior. `aval_item6.txt:1` (anexo), `app/chamados/services/disputa_emissao.py:1562–1567`, `app/chamados/signals.py:261–265`, `app/chamados/models.py:3653–3669,4833–4839`.

### 1. PROCEDENTE — o emissor do par está invertido

A tabela conta os inserts do outbox da pergunta por célula encontrada, em caminhos concluídos com sucesso. Todos passam pelo mesmo escritor físico, `emitir_evento_celula`. Sem célula pela FK ou pelo par colaborador/dia, são zero inserts. `app/chamados/models.py:3605–3636`, `app/ponto/portas/celula.py:981–991`.

| Caminho | Quem aciona o insert |
|---|---|
| `save` + `registrar`, mesma instância e mesmo atomic | **1, `post_save`**. O `registrar` consome o recibo pendente. Vale para a resposta humana, a automática, a resposta pela trilha e os pares de carimbo e validação. `app/chamados/signals.py:255–265`, `app/chamados/models.py:3520–3523,4832–4839`, `app/chamados/services/disputa_emissao.py:1562–1567,1760–1763,1794–1797`, `app/chamados/services/validacao.py:174–178,312–318,389–395`. |
| `save` sem `registrar` | **1, `post_save`**, se o save for inteiro ou nomear algum campo do conjunto. Save parcial fora do conjunto ou `raw` dá **0**. `app/chamados/signals.py:255–265`. |
| `registrar` sem `save` e sem recibo | **1, `registrar`**, para lote comum não vazio. Lote vazio dá **0**. `app/chamados/models.py:4796–4805,4824–4839`. |
| `created` | **0 pelo `post_save`**. `_nascer` chama `registrar` e dá **1** se houver assinatura para registrar, **0** se o lote vier vazio. `app/chamados/signals.py:255–257`, `app/chamados/models.py:3322–3327,4540–4559,4799–4803,4835–4839`. |
| `reabrir_para_correcao` | **2** na célula que entra tanto no lote de atos quanto nas linhas capturadas. O `registrar(pergunta_id=...)` emite primeiro e a porta emite de novo depois do `update`. Se só a captura contém a célula, **1**, pela porta. O recibo erra ao contar sempre um. A ressuscitação e a devolução L-130 também têm emissão explícita além do `registrar`. `app/chamados/services/disputa_emissao.py:1680–1682,1695–1716`, `app/chamados/models.py:1728–1733,3709–3711`, `app/chamados/services/validacao.py:481–494`. |
| `fechar_por_admin` | **1, `post_save`**. `carimbar_identidade` faz um save e retorna. Nos chamadores que registram `SILENCIO` logo depois com a mesma instância, esse `registrar` consome o recibo daquele save. O caminho com `SILENCIO` cai de **2 para 1**, não de 3 para 2. `app/chamados/models.py:2177–2184,3363–3377,3562–3571,4832–4839`, `app/ponto/services/veredito_celula.py:194–202`. |
| Retratação de motivo | **2** quando o carimbo grava. Primeiro emite `registrar(RETRATACAO)`, depois o `post_save` do carimbo de identidade. A ordem é inversa à do `SILENCIO`, e já eram dois inserts na base. `app/chamados/models.py:2315–2323,3562–3571`, `app/chamados/signals.py:255–265`. |
| Estoque (`entrada_origem=ESTOQUE`) | **0 pelo `registrar`**. A porta consome eventual recibo e pula a emissão. Um save anterior pode já ter emitido pelo signal. Os backfills antigos citados usam atos comuns e seguem o ramo emissor quando o lote contém atos. `app/chamados/models.py:4833–4860`, `app/chamados/management/commands/backfill_decisoes_pergunta.py:357–361`, `app/chamados/management/commands/backfill_fio_morto_e4b.py:161–164`. |

**Dá para cumprir o aval pela porta existente.** Uma opção é `registrar` receber a operação de salvar a projeção, executá-la sob a trava e no mesmo atomic, e sinalizar ao receiver de outbox, durante aquele save, que a emissão pertence ao `registrar`. O escopo da sinalização acaba em `finally`. Saves avulsos continuam pelo receiver e o receiver do beco continua recebendo o `post_save`. O custo é alterar a assinatura e migrar os pares de chamadas, preservar a ordem das travas, tratar explicitamente lotes vazios e cobrir rollback e falha de emissão. Se a porta também assumir as reaberturas, precisa receber as células capturadas antes do `update`, pois o filtro pode perder as perguntas depois. A intenção fica conhecida antes do save. `app/chamados/models.py:3605–3608,3891–3907,4796–4839`, `app/chamados/signals.py:229–240,254–265`, `app/chamados/services/disputa_emissao.py:1680–1682,1713–1716`.

### 2. PROCEDENTE — a marca mistura recibo de emissão com identidade da escrita

| Valor | Quem escreve e quem lê |
|---|---|
| `recibo` e `ja_acordadas` | O signal abre `{}`. `acordar_celulas` põe `celula.pk → evento.pk` após o insert. `guardar_recibo_da_escrita` copia o mapa para a instância. `ja_acordadas_na_escrita` produz o conjunto de células e o `registrar` o devolve a `acordar_celulas`, que o usa para iniciar `vistos`. `app/chamados/signals.py:261–265`, `app/chamados/models.py:3621–3636,3650–3669,4835–4839`. |
| `_acordou_na_escrita` | `guardar_recibo_da_escrita` mescla os IDs no `__dict__`. Outro save da mesma célula sobrescreve o ID anterior. `ja_acordadas_na_escrita` tira a marca inteira com `pop`, inclusive no caminho de estoque. O callback também faz `pop` da chave inteira. A marca registra **qual evento um save anterior emitiu** e depois serve de prova de **que o ato atual faz parte da mesma escrita**. Falta identidade do ato, da conexão ou do savepoint nessa prova. `app/chamados/models.py:3647–3669,4824–4839`. |
| `EventoCelula.processado_em` | O insert nasce com o campo nulo. O consumidor grava a baixa quando conclui e a retenção apaga linhas antigas. A cura lê `pk` e `processado_em IS NULL`. A fila viva lê também `tentativas < MAX_TENTATIVAS_EVENTO`. Assim, **sem baixa** e **apto a ser consumido** são condições distintas. A cura usa a primeira para decidir a supressão. `app/ponto/portas/celula.py:990–991`, `app/escala/models.py:1871–1877`, `app/chamados/reconciliador.py:1389–1404,1546–1563`, `app/chamados/models.py:3667–3669`. |
| `PerguntaDisputa.CAMPOS_QUE_ACORDAM` | O modelo declara o conjunto imutável. O receiver lê sua interseção com `update_fields`. A lista cobre resposta, validação, carimbo, dia e vínculos, com nomes de FK e seus `_id`. O receiver testa **campos nomeados pelo save**. A outra constante homônima pertence à porta da célula e cobre `veredito` e `ata`. `app/chamados/models.py:3595–3608`, `app/chamados/signals.py:255–260`, `app/ponto/portas/celula.py:975–1002`. |

### 3. PROCEDENTE para vazamento de estado, semântica e lacuna do RED

- **PROCEDENTE — estado em instância.** O rollback descarta callbacks da transação ou do savepoint, mas a marca foi escrita diretamente no objeto. O `pop` do `registrar` também ocorre antes do fim do atomic e sobrevive se esse `registrar` for desfeito. Se duas threads compartilham o objeto, leem e alteram a mesma marca, sem uma chave de conexão. Compartilhamento em produção [nao verificado]. `app/chamados/services/trava_do_colaborador.py:16–23`, `app/chamados/models.py:3650–3669,4807,4824–4839`.
- **PROCEDENTE — dois acionadores e uma derivação frágil.** Signal e `registrar` podem encomendar o mesmo evento. A cura usa a presença de uma linha sem baixa para inferir que ela pertence à escrita atual. Já `evento(unica=True)` procura o registro pelos atributos do próprio ato sob a serialização do chamador. `app/chamados/signals.py:261–265`, `app/chamados/models.py:3662–3669,4835–4839`, `app/core/observ.py:104–106,138–153`.
- **NAO PROCEDE — segundo juiz registrado, segundo insert fora da porta, relógio novo ou inversão de trava introduzida pelo diff.** C6 e C21 continuam nas autoridades registradas. O insert segue em `emitir_evento_celula`, e a sequência segue no `bulk_create` do `registrar`, depois da conferência do dono. A operação nova é um `SELECT` simples do recibo depois do insert, sem tomada explícita de trava nem leitura de relógio. `app/core/juizes.py:298,313`, `app/chamados/models.py:3632–3635,3667–3669,4807–4818`, `app/ponto/portas/celula.py:981–991`.
- **PROCEDENTE — lacuna do RED.** A resposta pública testa o ato e a quantidade de eventos. A autoria do evento exigida pelo aval fica sem asserção. `app/chamados/tests/test_e4c_f5_c4_red.py:634–646`.

### 4. PROCEDENTE — há duplicatas e há um caminho para zero

Contagem para a mesma célula, com os atos comuns aceitos e as travas exigidas satisfeitas. `app/chamados/models.py:3623–3636,4796–4818,4835–4839`.

| Contraexemplo | Resultado |
|---|---|
| Save desfeito por savepoint antes do `registrar` | **1 sobrevivente**, do `registrar`. O ID do evento revertido fica na memória, mas a consulta deixa de encontrá-lo. Se o save antecede o savepoint e sobrevive, fica **1**, o original. O caso testado desfaz o save. `app/chamados/models.py:3662–3669`, `app/chamados/tests/test_e4c_f5_c4_red.py:696–703`. |
| Dois `registrar` na mesma instância | Após um save elegível, **2 no total**. O signal emite, o primeiro `registrar` consome a marca e o segundo emite. Se o primeiro `registrar` for desfeito e repetido, a marca consumida continua ausente. A repetição também deixa **2** eventos, embora só um `registrar` sobreviva. `app/chamados/signals.py:261–265`, `app/chamados/models.py:3662–3669,4807,4824–4839`. |
| Instância recém-carregada do banco | **2**, mesmo no mesmo atomic. O recibo está no objeto salvo. Passar só `pergunta_id` também deixa o `registrar` sem acesso à marca. `app/chamados/models.py:3653–3656,4804,4824–4839`. |
| Save chamado fora de atomic externo | **1** do save avulso. O próprio `PerguntaDisputa.save` abre atomic e o commit executa a limpeza. Um `registrar` válido posterior dá **2 no total**. Se o helper for chamado genuinamente fora de atomic, retorna antes de agendar callback. `app/chamados/models.py:3650–3656,3891–3907,4813–4814`. |
| `on_commit` anterior à limpeza, reutilizando a mesma instância | **0 eventos novos para a decisão nova**, se o evento anterior ainda estiver sem baixa. Agende antes do save avulso um callback que, após o commit, abra outra transação, pegue a trava e chame `registrar` com o mesmo objeto. Esse callback pode consumir o recibo antes da limpeza agendada pelo save. A consulta encontra o evento da transação anterior e silencia a nova emissão. É um contraexemplo de código. Ocorrência em produção [nao verificado]. `app/chamados/models.py:3653–3669,4807–4818,4824–4839`, `app/chamados/services/trava_do_colaborador.py:58–68,76–84`. |
| Evento do recibo já processado | **2 inserts no total**, com **1 novo pendente**. A baixa invalida o recibo para fins de supressão. `app/chamados/models.py:3667–3669`, `app/chamados/tests/test_e4c_f5_c4_red.py:713–719`. |
| `bulk`/`update` sem `post_save` | **0** se a escrita vier sozinha, inclusive se ela alterar um campo que deveria acordar. Com `registrar` comum, **1** pela porta. Com `registrar` e emissão explícita, **2** na interseção, como na reabertura. `app/chamados/signals.py:254–265`, `app/chamados/models.py:3709–3711,4835–4839`, `app/chamados/services/disputa_emissao.py:1680–1682,1695–1716`. |

### 5. NAO PROCEDE para mutante vazio. PROCEDENTE para cobertura insuficiente do aval

O harness exige um caso executado, sem skip ou erro, com a falha na asserção de negócio nomeada, e controle verde antes e depois. Conferi as quatro reescritas dos seis mutantes na fonte. Cada âncora ocorre uma vez e a fonte resultante é sintaticamente válida. Resultado em execução e logs do recibo [nao verificado]. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:27–67,230–238,787–831`.

| Mutante | O que deve fazer falhar |
|---|---|
| M47 | Reemite na resposta pública. O teste espera **1**, o mutante produz **2**. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:787–789,809–811`, `app/chamados/tests/test_e4c_f5_c4_red.py:634–646`. |
| M48 | Reemite na célula já coberta do lote. **(1, 1) → (2, 1)**. O teste salva só `p1`. Duas perguntas salvas na mesma célula antes do lote já produziriam dois inserts pelo signal. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:812–815`, `app/chamados/tests/test_e4c_f5_c4_red.py:686–694`, `app/chamados/signals.py:261–265`. |
| M49 | Aceita o ID revertido como prova. **1 → 0** sobreviventes. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:792–794,816–819`, `app/chamados/tests/test_e4c_f5_c4_red.py:696–703`. |
| M50 | Aceita o ID já processado. **2 → 1** inserts. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:792–794,820–823`, `app/chamados/tests/test_e4c_f5_c4_red.py:713–719`. |
| M51 | Retira o callback de limpeza. **2 → 1**. O teste executa os callbacks capturados dentro de `TestCase`, simulando a limpeza. Um commit real e a ordem entre callbacks ficam sem cobertura. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:797–800,824–827`, `app/chamados/tests/test_e4c_f5_c4_red.py:705–711`, `app/chamados/tests/cortes_f2_f9_apoio.py:58`. |
| M52 | Cala **todo** `post_save` que passa `recibo`. O save avulso cai de **1 → 0**. Isso testa o silêncio indiscriminado. A coordenação prevista no aval, em que só o save já assumido pelo `registrar` fica calado, permanece sem teste de autoria. `app/chamados/tests/e4c_f5_c4red2_mutantes.py:803–806,828–831`, `app/chamados/tests/test_e4c_f5_c4_red.py:674–684`. |

### 6. NAO PROCEDE — os fakes preservam a prova existente de H2

O `save` falso já só anotava gravações. O fake novo devolve um conjunto vazio de recibos, e os dois emissores falsos passam a aceitar kwargs. H2 continua verificando a gravação e a chamada do outbox instrumentado, a propagação de `outbox indisponivel` e o rollback simulado da gravação. Sua cobertura é o caminho do `registrar` sem recibo. `app/chamados/tests/test_e4_expand.py:34–50,69–89,396–431`.

O selo antigo **M-H2-evento**, por sua vez, falha na âncora antes de executar o teste. Conferi **zero ocorrências** da âncora em `8ea46953e`, `8529b6003` e `59600163c`. O mesmo vale para **M-C11-reversao**. `app/chamados/tests/e4_verificar.py:21–24,33–37,58–64`.

## 27/11 12:21 · sessao 936455fd919d · FERIADO-BOTAO-CORTADO · construtor · origem=Code

A c3 atualiza o smoke de clique e guarda o RED de geometria do painel em um módulo fora da descoberta padrão do runner. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:217) · [RED do painel](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/red_painel_popover_feriado.py:1)

### Asserções por cenário

Os quatro cenários — página em `base.html`, página em `base_app.html`, drawer em `base.html` e drawer em `base_app.html` — executam as mesmas checagens no feriado de 20/11. [Fixture](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:217) · [Matriz de cenários](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:252)

- O `summary` nasce fechado, tem área positiva dentro da célula e da viewport, e o hit no centro retorna o próprio `summary` ou um filho. O clique no alvo abre o formulário. [Sonda de abertura](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:90)
- O centro do submit atinge o próprio botão, que está habilitado e pertence ao formulário. O `action` bate com `reverse('ponto:feriado_vira_trabalho')` e o método é `POST`. [Submit](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:121) · [Contrato do formulário](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:234)
- A região mensal contém `08:00`, `12:00`, `13:00` e `17:00`, nessa ordem. A sonda acrescenta espaço sob as três primeiras marcações após medir o submit, para exigir deslocamento. No fim da rolagem, a marcação de `17:00` fica visível e o `summary` continua visível e atingível pelo centro. [Rolagem](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:136)

### Mutantes e comandos para o Code

| `SMOKE_FERIADO_MUTANTE` | Asserção que deve cair |
|---|---|
| `gatilho-cortado` | `gatilhoVisivel`, pelo deslocamento para fora da célula. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:39) |
| `summary-display-none` | `gatilhoVisivel`, pela caixa sem área. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:41) |
| `porta-trocada` | `action`, pelo caminho `/porta-mutante/`. [Smoke](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:238) |
| `painel-recortado` | `painelSemRecorte`, no RED guardado. [RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/red_painel_popover_feriado.py:10) |
| `painel-distante` | `painelEncostado`, no RED guardado. [RED](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/red_painel_popover_feriado.py:22) |

```bash
rodar() {
  PYTHONDONTWRITEBYTECODE=1 python3 -B manage.py test \
    --settings=config.settings.ci --noinput "$1"
}
smoke=core.tests.test_smoke_clique_feriado_vira_trabalho
red=core.tests.red_painel_popover_feriado

SMOKE_FERIADO_MUTANTE='' rodar "$smoke"
SMOKE_FERIADO_MUTANTE=gatilho-cortado rodar "$smoke"
SMOKE_FERIADO_MUTANTE=summary-display-none rodar "$smoke"
SMOKE_FERIADO_MUTANTE=porta-trocada rodar "$smoke"

SMOKE_FERIADO_MUTANTE='' rodar "$red"
SMOKE_FERIADO_MUTANTE=painel-recortado rodar "$red"
SMOKE_FERIADO_MUTANTE=painel-distante rodar "$red"
```

### Diff

```bash
git --no-optional-locks diff 9bcb7447e -- core/tests/test_smoke_clique_feriado_vira_trabalho.py
git --no-optional-locks diff --no-index -- /dev/null core/tests/red_painel_popover_feriado.py
```

Conferi a sintaxe Python e JavaScript, o padrão de descoberta e os caminhos de falha com DOM simulado. A execução do smoke, do RED e dos mutantes em Chromium, assim como o calendário de produção, fica [nao verificado].

## 27/11 12:15 · sessao b30fa6c0a058 · FERIADO-UM-JUIZ · censo · origem=Code

# FERIADO-UM-JUIZ · Dossiê antes do construtor

Leitura estática da cópia. Hash Git, execução dos testes, impacto de frota e situação da competência 11 em produção [nao verificado]. [nao verificado]

## 1. Mapa

- **Hoje.** O fechamento monta o motor por fatia de vínculo com o calendário do colaborador. `eh_dia_trabalho(...) is False` manda o período para `periodos_ft`, com o dia da jornada no mapa. O fechamento filtra `_ft_certa`, procura `_ft_feriado` **dentro** desse grupo via `feriado_vira_folga` e põe o restante sem escala certa em `horas_trabalhadas` sem 100%. A partição é por identidade do período. `app/ponto/services/fechamento.py:668`, `:744`, `:953`, `:970`, `:1004` · `app/ponto/motor_calculo_v2.py:2560`, `:2573`, `:2605`.
- **Juiz.** `_feriado_dobra_no_dia` lê `feriado_em_dobra` e `eh_feriado` no dia de início da jornada. `get_motor` resolve o padrão por `tipo_ciclo`, com `12x36` em `CICLOS_FERIADO_SIMPLES`. Os quatro ramos de dobra usam o mesmo método. `app/ponto/motor_calculo_v2.py:941`, `:1000`, `:2925`, `:3208`, `:3385`, `:3467`, `:3740`, `:3810`.
- **Como perguntar.** Usar o dia de `dia_pago._dia_de(p, resultado.dia_da_jornada)`, os minutos apurados e o contexto **do motor da fatia daquele período**. `dobra_de_feriado_do_dia` já delega ao método do motor e devolve minutos. Hoje seu contexto permite fallback nacional, enquanto o calendário injetado no motor responde sozinho. A adaptação precisa preservar essa diferença ou consultar o próprio motor da fatia. `feriado_em_dobra_do_ciclo` expõe o padrão do ciclo, e `cct_do_motor` lê a configuração efetiva. O composto expõe o mapa de dias e só o último motor. Os totais de HE do resultado leem `periodos`, e o campo bruto do FT pode vir preenchido no comercial e vazio no partido. `app/ponto/calculador/regras.py:297`, `:312`, `:323`, `:358` · `app/ponto/calculador/nucleo.py:319` · `app/ponto/services/dia_pago.py:109` · `app/ponto/services/fechamento.py:331` · `app/ponto/motor_calculo_v2.py:328`, `:1000`, `:2993` · `app/ponto/tests/test_o280_bateria_da_lei.py:1810`.

## 2. Censo

- **Mensal.** `recalcular_fechamento_mes` deriva os seis campos. A promoção soma a HE a `horas_extras_100_feriado`, `horas_extras_100` e `horas_extras`, leva o subconjunto a `horas_extras_100_noturna` e retira o período de `horas_folga_trabalhada` ou `horas_trabalhadas`, conforme a origem. A porta persiste por `save` ou `update` do aprovado. `restaurar_fechamento` grava valores fornecidos para ajuste ou reversão. `app/ponto/services/fechamento.py:922`, `:939`, `:978`, `:1004`, `:1168`, `:1184`, `:1382`.
- **Lavra.** `DiaPago.motor` recebe as três listas prontas. `lavrar` soma as rubricas e grava `detalhe.folga_feriado_min` ou `detalhe.folga_sem_escala_min`. **O derivador paralelo é o oráculo:** `rubricas_por_dia` consulta a escala certa, `do_dia` decide com `folga_e_feriado and _certa`, e `lavrar(rubricas=...)` pode sobrescrever os valores. Mensal e oráculo precisam consumir o mesmo juízo de dobra e a mesma classificação. `app/ponto/services/fechamento.py:1225`, `:1289` · `app/ponto/services/dia_pago.py:234`, `:244`, `:259`, `:322`, `:359` · `app/ponto/calculador/alimentacao.py:520` · `app/ponto/calculador/regras.py:674`, `:694` · `CLAUDE.md:20`.
- **Leitores.** TXT e homologação leem os eventos do mensal. `_suprime_feriado` zera a parcela de 12x36 sem cláusula CCT. `hora_extra_feriado_diurna` usa o código cadastrado, separado de `folga_trabalhada_diurna`. HE100 noturna é subconjunto, sem linha própria. Conferir também o crivo `conta_vazia` quando o mês só tem horas promovidas. Outros leitores são ranking HE, cartão/resumo, telas mensais, Inteligência e a comparação de soma do dia com o mensal. `app/folha/export.py:420`, `:439`, `:739`, `:840`, `:911`, `:993`, `:1011` · `app/folha/services/ranking_he.py:33`, `:139`, `:224` · `app/relatorios/cartao_pela_celula.py:174` · `app/templates/ponto/_fech_linha.html:30` · `app/relatorios/views.py:98` · `app/inteligencia/views.py:118` · `app/folha/porta_export.py:457`.
- **Dia na tela, PDF e app.** `pago_do_dia` soma trabalhadas, folga e `folga_feriado_min`. O PDF usa `pago_h` para realizado. A faixa de rubricas da tela usa `rubricas_do_dia`, cujo `trabalhadas` lê só o campo homônimo e cujo `extras_100` lê o total. App e testemunha de export também leem a lavra. `app/ponto/services/dia_pago.py:897`, `:914` · `app/ponto/services/espelho.py:829` · `app/templates/ponto/espelho.html:275` · `app/relatorios/pdf_espelho.py:901`, `:984` · `app/api/views.py:1537` · `app/folha/porta_export.py:120`.

## 3. Leis e textos a atualizar

- **L-086:** corrigir o enunciado ainda abrangente. O rodapé já registra a revogação da cláusula de feriado em 27/09 03:xx. Domingo previsto segue a regra própria. Nos cortes, registrar o alcance novo sobre FT e remeter o CLASSE3 de 19/09 e o piso de 27/09 05:xx à **folga comum**. BO-B(3) mantém a rubrica de feriado no caso que nomeia. ESCALA-CERTA-DETERMINISTICA mantém a fonte por competência. BUG-FERIADO-DOIS-ESCRITORES mantém a partição disjunta. `app/docs/LEIS.md:118` · `app/docs/TICKETS.md:1134` · `app/ponto/services/fechamento.py:991` · `app/docs/PROMPTS.md:1370`, `:1418`, `:1424`.
- **Guardas herdadas:** L-085 fixa o início da jornada. O corte de 17/11 exclui o intermitente. A janela L-097 já pode zerar um par de folga antes do fechamento, com tratamento próprio de escala certa e autorização. A promoção recebe os minutos que sobreviveram a essa etapa. `app/docs/LEIS.md:117` · `app/docs/PROMPTS.md:2536` · `app/ponto/motor_calculo_v2.py:1562`, `:1569`, `:1585`, `:3694`.

## 4. Contrato e matriz RED

Unidade **colaborador × período FT**, com minutos pagáveis apurados, dia da jornada, motor da fatia, calendário daquele vínculo e resposta de escala certa para a rubrica. Validar mensal, `DiaPago.motor`, `DiaPago.oraculo` e evento TXT. `app/ponto/services/fechamento.py:299`, `:953`, `:1225`, `:1295` · `app/core/feriados.py:135` · `app/folha/export.py:439`.

| Caso | Esperado |
|---|---|
| 6x1, escala certa, vínculo folga por causa do feriado | Já promove toda a duração a HE100 feriado. Manter uma única rubrica. `app/ponto/services/fechamento.py:958`, `:970`, `:978` |
| 6x1, LIMBO / A2 / A8, feriado | Hora comum hoje. RED exige HE100 feriado pela duração positiva, sem repetir o período em trabalhadas ou FT. `app/escala/services/escala_certa.py:19` · `app/ponto/services/fechamento.py:958`, `:1004` |
| 12x36 de piso, certa e sem escala certa | HE100 **feriado** zero nas duas. A folga real certa tem 100% **de folga** hoje. A sem escala certa fica em trabalhadas. Há exceção CCT explícita no código, fora da mudança. `app/ponto/tests/test_o280_bateria_da_lei.py:1385` · `app/ponto/services/fechamento.py:989`, `:1004` · `app/core/regua_cct.py:473` |
| Intermitente em feriado | Trabalho convocado segue pelo caminho comum, fora de FT e da dobra automática. `app/ponto/motor_calculo_v2.py:2474`, `:2560`, `:3694` |
| Folga comum sem escala certa | Hora em trabalhadas, zero HE100, com noturno e intra quando devidos. `app/ponto/tests/test_dia_pago_soma.py:245` |
| FT noturno promovido | HE100 noturna acompanha a promoção. Cobrir também o partido, cujo FT não traz o split pronto, usando `he_noturna` com a régua e os intervalos do motor. `app/ponto/motor_calculo_v2.py:409`, `:1051`, `:2993`, `:3208` · `app/ponto/tests/test_o280_bateria_da_lei.py:1720` |
| Véspera cruza para feriado | Zero dobra de feriado, inclusive no segundo par da mesma jornada. `app/docs/LEIS.md:117` · `app/ponto/tests/test_o280_bateria_da_lei.py:1564`, `:1580` |

## 5. Testes existentes [nao verificado]

O teste de FT em feriado sem escala certa que já passa pela porta usa **12x36** e só exige pagamento e paridade entre lavras. Preservar esse controle e criar o gêmeo **6x1** com valores exatos. A asserção a inverter na porta do dinheiro é `HE100 feriado = 0; trabalhadas = duração` para `HE100 feriado = duração; trabalhadas = 0`, isolando o período. F3b fixa zero **no agregado do motor** e 480 no campo cru do FT. Esse selo fica válido com a promoção no fechamento. A bateria L-114 deve reunir F3/F4, F10/F10e, F11c/F11d, BO-B de contagem única e S6 intermitente. `app/ponto/tests/test_he100_dia_sem_previsto.py:40`, `:227` · `app/ponto/services/fechamento.py:958`, `:1004` · `app/ponto/tests/test_o280_bateria_da_lei.py:1385`, `:1479`, `:1580`, `:1720`, `:1775`, `:1810` · `app/ponto/tests/test_bob_b2_abono_feriado_guarda.py:127` · `app/ponto/tests/test_s6_intermitente_feriado.py:133` · `app/docs/LEIS.md:154`.

## 6. Padrão

`padrao=single source of truth` / **um juiz por pergunta**. A partição alimenta as projeções mensal e diária. A lavra principal já tem uma porta de escrita compartilhada. `app/docs/GLOSSARIO.md:11` · `app/docs/PLANO-PADROES.md:53`, `:61` · `app/ponto/services/fechamento.py:1225` · `app/ponto/services/dia_pago.py:359`.

## 7. Arquivos e pouso [nao verificado]

Superfície proposta: `fechamento.py`, `calculador/regras.py`, `calculador/alimentacao.py`, testes e textos do item 3. `dia_pago.py` já aceita a partição. `folha/export.py` entra como leitor a certificar, evitando a interseção informada com E4C. PAR-PELO-TURNO cruza pelo contrato de período e dia da jornada em `ponto/turnos.py`. Interseção exata entre árvores em voo [nao verificado]. `app/ponto/services/fechamento.py:1225`, `:1295` · `app/ponto/services/dia_pago.py:135` · `app/docs/RELATO.md:1057`, `:13678`.

Para o pouso, bateria verde e DIFF antes do apply de 11, com reversão e recorte por empresa pelo K6. O recálculo genérico pode gravar exportada fora da guarda cadastrada, inclusive por evento. A exclusão de exportadas exigida neste aval precisa valer também nesse caminho. `app/docs/LEIS.md:125`, `:142`, `:154`, `:156` · `app/folha/export.py:565` · `app/ponto/services/fechamento.py:559`, `:580`, `:1834`.

## 8. Pergunta de lei

1. **6x1 com escala certa, folga semanal que coincide com feriado e `feriado_vira_folga=False`** — o juiz confirma a dobra. A rubrica continua `folga_trabalhada_diurna` ou passa a `hora_extra_feriado_diurna`? O BO-B(3) nomeia expressamente o vínculo que folga por causa do feriado, e a partição atual conserva FT quando esse predicado é falso. `app/docs/PROMPTS.md:1370` · `app/escala/models.py:1384`, `:1400` · `app/ponto/services/fechamento.py:970`, `:989`.

## 27/11 12:14 · sessao bf3f4123005e · FERIADO-BOTAO-CORTADO · verificador · origem=Code

**FERIADO-BOTAO-CORTADO — PRONTO não demonstrado.** O predicado foi preservado. O teste novo tem mordida estrutural, mas a visibilidade com quatro batidas no navegador, a regressão visual da rolagem e o smoke do Ronald no calendário seguem [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/colaboradores/tests/test_feriado_botao_cortado.py:71`

**1. Predicado — descartada alteração.** Antes, `_calendario_grade.html:131` no blob `896fad490` do anexo. Depois, `app/templates/colaboradores/partials/_calendario_grade.html:96` no blob `019b86999` da cópia. A expressão é literalmente `pode_editar_dia and dia.feriado_vira_trabalho_oferecido` nos dois. Os três blobs novos conferem com o anexo, que contém dois templates e o teste. A oferta continua vindo de feriado do colaborador com célula existente e `trabalha is False`. O POST consulta o mesmo campo. `app/colaboradores/services/calendario.py:791` `app/ponto/views.py:3607` `app/ponto/views.py:3624`

**2. Botão com 0..N batidas — descartado bloqueio no HTML por quantidade. Procedente a lacuna de prova visual.**

- A ação é renderizada antes do bloco de batidas e retratadas. Vale para zero ou qualquer quantidade, dada a oferta. O rótulo efetivo é **“Tornar dia de trabalho”**. O perfil usa `mes` em `base.html` e o diagnóstico usa `fita`. As duas cascas incluem o drawer genérico, que injeta o parcial por `fetch`. Isso comprova o caminho compartilhado do drawer, com visibilidade nas duas cascas [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:95` `app/templates/colaboradores/partials/_calendario_grade.html:130` `app/templates/ponto/partials/_feriado_vira_trabalho.html:6` `app/templates/colaboradores/detalhe.html:1` `app/templates/colaboradores/detalhe.html:215` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/base.html:399` `app/templates/base_app.html:154` `app/templates/core/_drawer_generico.html:19`
- **Mutante que morde.** Voltar a ação para depois das marcações, mantendo o resto, derruba o `assertLess` nos dois layouts. Retirar `overflow-y:auto` derruba a asserção de `mes`. **Mutante que escapa.** Acrescentar `display:none` ao `<summary>` preserva todas as asserções do teste e esconde o gatilho. O teste injeta 0 e 4 batidas e renderiza o include isolado nos dois layouts. A execução do RED e do verde ficou [nao verificado], pois a importação local parou em `django.template`. `app/colaboradores/tests/test_feriado_botao_cortado.py:4` `app/colaboradores/tests/test_feriado_botao_cortado.py:35` `app/colaboradores/tests/test_feriado_botao_cortado.py:92` `app/colaboradores/tests/test_feriado_botao_cortado.py:109`

**3. Escritores e leitores — descartada mudança de contrato.**

| Variável | Quem escreve ou passa | Quem lê |
|---|---|---|
| `layout`, `dias`, `dia`, `cal_offset`, `resumo` | `contexto_calendario` entrega os dados. Perfil passa `mes`, drawer passa `fita` e `cal.dias`/`cal.resumo`, Furos passa só `dias`/`resumo`. | A grade escolhe o layout, itera `dias` como `dia` e usa o offset e os totais. `app/colaboradores/services/calendario.py:838` `app/templates/colaboradores/calendario_partial.html:67` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/relatorios/furos.html:42` `app/templates/colaboradores/partials/_calendario_grade.html:65` `app/templates/colaboradores/partials/_calendario_grade.html:155` |
| `pode_editar_dia`, `dia.feriado_vira_trabalho_oferecido` | View do perfil e `contexto_drawer` consultam `tem_acao(..., 'editar_escalas')`. `contexto_calendario` escreve a oferta do dia. | A grade lê ambos para o gatilho e o CSS. O POST consulta a mesma permissão e lê a oferta no contexto do dia. `app/colaboradores/views.py:2007` `app/colaboradores/services/drawer.py:159` `app/colaboradores/services/calendario.py:791` `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/ponto/views.py:3607` `app/ponto/views.py:3624` |
| `colaborador.pk`, `dia.data`, `volta_url` → `colab_pk`, `data`, `volta` | O serviço publica colaborador e data. O drawer passa `colab` como `colaborador`. Perfil e drawer montam a URL de volta. A grade passa os três aliases. | A grade usa a data também no `aria-label`. O parcial escreve `colab`, `dia` e `volta` no formulário, e a view os lê. `app/colaboradores/services/calendario.py:755` `app/colaboradores/services/calendario.py:839` `app/colaboradores/views.py:2009` `app/colaboradores/services/drawer.py:162` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/colaboradores/partials/_calendario_grade.html:97` `app/templates/ponto/partials/_feriado_vira_trabalho.html:9` `app/ponto/views.py:3614` |
| Campos da célula envolvidos pelo novo `<div>` | `contexto_calendario` publica status, palavra, ausência, feriado, batidas, retratadas, badges e `resolver_dia_oferecido`. Perfil e drawer fornecem `pode_veredito` via `regularizar_batida`. | A grade lê esses valores nos ícones, horários, ações de veredito e badges, agora dentro do mesmo bloco. `app/colaboradores/services/calendario.py:754` `app/colaboradores/services/calendario.py:811` `app/colaboradores/views.py:1992` `app/colaboradores/services/drawer.py:133` `app/colaboradores/services/drawer.py:157` `app/templates/colaboradores/partials/_calendario_grade.html:101` `app/templates/colaboradores/partials/_calendario_grade.html:130` `app/templates/colaboradores/partials/_calendario_grade.html:137` |

**4. Classes de reprovação — descartados derivação paralela, duplo sentido e juiz duplicado.** As três leituras da condição controlam o gatilho e dois estilos. A oferta continua sendo um fato do dia, e a permissão continua sendo do usuário. O juiz registrado `_TT11` segue respondendo só por “Resolver dia”. O cadastro da oferta de feriado no registro de juízes já está apontado em fila. **Selo sem mordida estrutural descartado. Lacuna de mordida visual procedente**, como mostra o mutante `display:none`. `app/docs/LEIS-UI.md:23` `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/core/juizes.py:745` `app/core/juizes.py:759` `app/docs/BACKLOG.md:2038` `app/colaboradores/tests/test_feriado_botao_cortado.py:95`

**5. Outros includes — descartada abertura da ação em Furos.** O `only` passa `dias` e `resumo`. Sem `pode_editar_dia` e `layout`, Furos usa a grade padrão e recebe o `<div>` extra sem os estilos de rolagem. O drawer recebe a ação antes das batidas e o rótulo com quebra de linha. A fita continua com rolagem horizontal. O efeito visual nos dois consumidores segue [nao verificado]. `app/templates/relatorios/furos.html:42` `app/templates/colaboradores/partials/_drawer_diagnostico.html:35` `app/templates/colaboradores/partials/_calendario_grade.html:65` `app/templates/colaboradores/partials/_calendario_grade.html:96` `app/templates/ponto/partials/_feriado_vira_trabalho.html:5`

**6. Rolagem separada — procedente a lacuna de verificação.** No mensal elegível, o botão fica fora da região rolável de uma célula de `132px`. A região tem `tabindex="0"` e engloba também ícones, chamado, “Resolver dia” e badges. O selo confere declarações de CSS e a estrutura do HTML. Alcance da última marcação, permanência do gatilho visível enquanto se rola e smoke do Ronald no calendário [nao verificado]. `app/templates/colaboradores/partials/_calendario_grade.html:68` `app/templates/colaboradores/partials/_calendario_grade.html:95` `app/templates/colaboradores/partials/_calendario_grade.html:101` `app/templates/colaboradores/partials/_calendario_grade.html:137` `app/colaboradores/tests/test_feriado_botao_cortado.py:99`

## 27/11 12:12 · sessao d4acb9eb4d2e · FERIADO-UM-JUIZ · sonda · origem=Code

## FERIADO-UM-JUIZ — competência 11/2026

**Confere: 75 dias de 42 colaboradores. Todos os 75 saíram como hora comum. Pela regra proposta, 67 dias de 35 colaboradores passariam a HE100 feriado, somando 449,74 h.** `extrato:3–78`

As referências `extrato:N` apontam para o dado raspado da mensagem, contando `sonda_leitura` como linha 1. A apuração usa a saída fornecida. Reexecução na sombra e conferência independente com a lista do admin [nao verificado].

### (a) e (b) — Universo e classificação atual

| Empresa | Dias | Colaboradores | Dias com `he100fer=0` e `sem_escala_h > 0` | Horas comuns | Fonte |
|---|---:|---:|---:|---:|---|
| 2 | 65 | 37 | 65 | 459,75 h | `extrato:3–67` |
| 3 | 10 | 5 | 10 | 74,20 h | `extrato:68–77` |
| **Total** | **75** | **42** | **75** | **533,95 h** | `extrato:3–78` |

**Nenhum dia foge do padrão.** Em todas as linhas `DIA`, `ht = sem_escala_h`, `he100 = he100fer = folga = 0` e `folga_feriado_min = None`. No código atual, a seleção para HE100 feriado parte de `_ft_certa`. Os períodos sem escala certa entram em `horas_trabalhadas` tanto no fechamento quanto na lavra diária. `extrato:3–77` · [fechamento.py:958](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:958) · [fechamento.py:970](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:970) · [fechamento.py:1004](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1004) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259)

### (c) — Impacto da regra proposta

| Empresa | Dias que mudariam | Colaboradores afetados | Horas que passariam a HE100 feriado | Dias que ficam | `12x36` | `intermitente` | Ciclo `None` | Fonte |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 57 | 30 | 375,54 h | 8 | 5 | 3 | 0 | `extrato:3–67` |
| 3 | 10 | 5 | 74,20 h | 0 | 0 | 0 | 0 | `extrato:68–77` |
| **Total** | **67** | **35** | **449,74 h** | **8** | **5** | **3** | **0** | `extrato:3–77` |

A projeção aplica `dobra_do_ciclo=True` e `ciclo != intermitente` e soma o `sem_escala_h` exibido, já arredondado por dia. Os 67 dias elegíveis são todos `6x1`. O total calculado a partir dos minutos originais e o novo fechamento mensal ficam [nao verificado]. `extrato:3–77` · [regras.py:312](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/calculador/regras.py:312) · [motor_calculo_v2.py:3740](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:3740)

Os únicos IDs sem dias que mudariam são da empresa 2. Em `12x36`, ficam **366, 415, 788, 1031 e 1035**, com um dia cada. Em `intermitente`, ficam **830** com dois dias e **975** com um dia, embora o extrato traga `dobra_do_ciclo=True` para os três dias. `extrato:14,20,29,36–37,52,64–65`

### (d) — FECH atual por colaborador

Valores mensais em horas. Os IDs **727** e **788** já têm `he100fer` positivo no mês, e os dias sondados desses dois IDs têm `he100fer=0`. `extrato:24,29,90,93`

| Empresa | ID | `ht` | `he100` | `he100fer` | `folga` | Fonte |
|---|---:|---:|---:|---:|---:|---|
| 2 | 43 | 184,06 | 0,00 | 0,00 | 0,00 | `extrato:79` |
| 2 | 203 | 182,34 | 0,00 | 0,00 | 0,00 | `extrato:80` |
| 2 | 206 | 170,16 | 0,00 | 0,00 | 0,00 | `extrato:81` |
| 2 | 207 | 169,23 | 0,00 | 0,00 | 0,00 | `extrato:82` |
| 2 | 278 | 178,12 | 0,00 | 0,00 | 0,00 | `extrato:83` |
| 2 | 366 | 65,85 | 0,00 | 0,00 | 0,00 | `extrato:84` |
| 2 | 369 | 179,55 | 0,00 | 0,00 | 0,00 | `extrato:85` |
| 2 | 384 | 192,04 | 0,00 | 0,00 | 0,00 | `extrato:86` |
| 2 | 415 | 179,90 | 0,00 | 0,00 | 0,00 | `extrato:87` |
| 2 | 583 | 153,08 | 0,00 | 0,00 | 0,00 | `extrato:88` |
| 2 | 622 | 143,48 | 0,00 | 0,00 | 0,00 | `extrato:89` |
| 2 | 727 | 167,77 | 7,05 | 7,05 | 0,00 | `extrato:90` |
| 2 | 735 | 164,36 | 0,00 | 0,00 | 0,00 | `extrato:91` |
| 2 | 784 | 173,02 | 0,00 | 0,00 | 0,00 | `extrato:92` |
| 2 | 788 | 153,65 | 14,23 | 14,23 | 0,00 | `extrato:93` |
| 2 | 789 | 161,32 | 0,00 | 0,00 | 0,00 | `extrato:94` |
| 2 | 822 | 170,30 | 0,00 | 0,00 | 0,00 | `extrato:95` |
| 2 | 830 | 117,08 | 0,00 | 0,00 | 0,00 | `extrato:96` |
| 2 | 899 | 169,79 | 0,00 | 0,00 | 0,00 | `extrato:97` |
| 2 | 903 | 184,17 | 0,00 | 0,00 | 0,00 | `extrato:98` |
| 2 | 924 | 120,57 | 0,00 | 0,00 | 0,00 | `extrato:99` |
| 2 | 944 | 158,02 | 0,00 | 0,00 | 0,00 | `extrato:100` |
| 2 | 961 | 184,11 | 0,00 | 0,00 | 0,00 | `extrato:101` |
| 2 | 962 | 179,47 | 0,00 | 0,00 | 0,00 | `extrato:102` |
| 2 | 967 | 81,98 | 0,00 | 0,00 | 0,00 | `extrato:103` |
| 2 | 968 | 172,80 | 0,00 | 0,00 | 0,00 | `extrato:104` |
| 2 | 975 | 32,87 | 0,00 | 0,00 | 0,00 | `extrato:105` |
| 2 | 988 | 213,94 | 3,04 | 0,00 | 0,00 | `extrato:106` |
| 2 | 990 | 179,52 | 0,00 | 0,00 | 0,00 | `extrato:107` |
| 2 | 991 | 167,69 | 0,00 | 0,00 | 0,00 | `extrato:108` |
| 2 | 1000 | 145,13 | 0,00 | 0,00 | 0,00 | `extrato:109` |
| 2 | 1023 | 62,94 | 0,00 | 0,00 | 0,00 | `extrato:110` |
| 2 | 1026 | 95,85 | 0,00 | 0,00 | 0,00 | `extrato:111` |
| 2 | 1031 | 71,52 | 0,00 | 0,00 | 0,00 | `extrato:112` |
| 2 | 1035 | 67,94 | 0,00 | 0,00 | 0,00 | `extrato:113` |
| 2 | 1036 | 73,09 | 0,00 | 0,00 | 0,00 | `extrato:114` |
| 2 | 1050 | 8,32 | 0,00 | 0,00 | 0,00 | `extrato:115` |
| 3 | 82 | 196,97 | 0,00 | 0,00 | 0,00 | `extrato:116` |
| 3 | 114 | 163,38 | 0,00 | 0,00 | 0,00 | `extrato:117` |
| 3 | 749 | 141,63 | 0,00 | 0,00 | 0,00 | `extrato:118` |
| 3 | 911 | 180,50 | 0,00 | 0,00 | 0,00 | `extrato:119` |
| 3 | 1024 | 103,97 | 0,00 | 0,00 | 0,00 | `extrato:120` |

## 27/11 12:08 · sessao 4f65f40651ed · FERIADO-BOTAO-CORTADO · leitura · origem=Code

**a. As quatro falhas têm a mesma causa:** o JS mede o gatilho na viewport, mas aplica `left/top` em um painel `fixed` cujo ancestral muda a referência dessas coordenadas. (`popover-acao.js:38,44,75`)

- **Página, nas duas cascas:** gatilho em `(788,97; 580)`, painel em `(802; 445)`. O painel cabe inteiro e **sobrepõe** o gatilho. A distância registrada é **0 px** e a sonda rejeita a sobreposição. O `backdrop-filter` de `.hx-shell` muda a referência do painel. (`smoke_falhas.txt:41,97`, `test_smoke_clique_feriado_vira_trabalho.py:119`, `base.html:335`, `base_app.html:93`, `hasner-ponto.css:283`)
- **Drawer, nas duas cascas:** gatilho em `(890,28; 576)`, painel em `(1156; 243)`. A distância é **197,82 px** e a direita do painel ultrapassa a viewport em **20 px**, o que também derruba o teste de recorte. O `translateX(0)` mantém o drawer como referência do painel mesmo depois de aberto. (`smoke_falhas.txt:74,130`, `_drawer_generico.html:3,15,16`) [nao verificado]

**b. O defeito de posicionamento antecede o c1.** O c1 mudou a posição e a quebra de linha do gatilho e separou a rolagem das marcações. A geometria pré-c1 em execução é [não verificado]. Os outros usos são **Resolver dia** e **Inverter tipo/Retratar batida**, na grade do perfil, no Espelho do drawer e na fita do diagnóstico. Eles têm o mesmo risco sob esses ancestrais. Ocorrência em produção [não verificado]. (`diff_c1.patch`, `_feriado_vira_trabalho.html:5`, `_calendario_grade.html:127,135`, `detalhe.html:215`, `_drawer_generico.html:140`, `_drawer_diagnostico.html:35`)

O smoke de **Excluir feriado** passa pelo `hxConfirm` do cadastro e verifica POST, exclusão e auditoria. Ele não mede proximidade. (`feriado_form.html:52`, `test_smoke_clique_excluir_feriado.py:54,110`)

**c. Cabem duas curas, com o mesmo predicado.**

- **Template/CSS, só no feriado:** botão `popovertarget` e formulário `popover` na camada superior, com âncora CSS e inversão de posição. Preserva os outros popovers. Exige adaptar a sonda do `summary`. Compatibilidade nos navegadores usados [não verificado]. (`_calendario_grade.html:131`, `_feriado_vira_trabalho.html:5`, `test_smoke_clique_feriado_vira_trabalho.py:228`)
- **JS:** compensar a origem do painel medida em `left:0; top:0`. É a menor mudança para os deslocamentos observados. O painel continua sujeito ao recorte do ancestral, e uma mudança compartilhada alcança os demais popovers. (`popover-acao.js:44,53,75`, `_drawer_generico.html:3`)

**d. Sim, 16 px é razoável para a abertura normal.** O JS prevê **2 px** abaixo ou acima do gatilho, com ajuste horizontal. O fallback que cola o painel no topo pode fugir desse limite. (`popover-acao.js:55,64,70`)

## 27/11 12:07 · sessao 914f194adfe1 · ORFAS-VOLTOU · sonda · origem=Code

- **Defeito.** A sonda mostrou dois `reg10`, das rubricas `0025` e `0243`, com `00000VIGIA` no campo `matric`. O layout exige que os dez caracteres desse campo passem em `isdigit()`. As letras explicam os dois erros `reg10 campo matric nao-numerico`. [app/folha/export.py:272](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:272)
- **Escritor e fonte.** `montar_linhas_export` lê `fech.colaborador.codigo_dominio` na linha 737. O `strip().zfill(10)` preenche com zeros e preserva as letras. No mapa exemplificado na cópia, `0025` é adicional noturno e `0243` é intrajornada, ambos emitidos pela chamada a `montar_linha_10` na linha 747. O serializador grava a matrícula na linha 45. O mapa ativo da empresa 2 na sondagem é [nao verificado]. [app/folha/export.py:737](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:737) [app/folha/export.py:747](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:747) [app/folha/export.py:45](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:45) [app/folha/tests/test_sm1_monster_export.py:75](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/tests/test_sm1_monster_export.py:75)
- **Dono pela L-099: CADASTRO.** O valor inválido vem do campo cadastral `Colaborador.codigo_dominio`. `classificar_export` só verifica se esse campo está vazio, então a exigência de matrícula numérica é aplicada na validação do TXT. `dono_da_divergencia` julga divergências de dia por colaborador e fica fora desse caminho. Quem gravou o valor é [nao verificado]. [CLAUDE.md:183](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/CLAUDE.md:183) [app/colaboradores/models.py:311](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/models.py:311) [app/folha/export.py:881](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:881) [app/ponto/management/commands/e6_oraculo.py:53](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/management/commands/e6_oraculo.py:53)
- **TXT real de 11/2026, empresa 2.** Com o `sem_medicao` visto na sombra, o clique solicita a conferência e a porta segura a geração até ficar `pronta`. Mesmo com a porta pronta, basta que a montagem real reproduza uma dessas linhas para `gerar_dominio_txt` recusar por falha de layout, antes de entregar ou registrar o arquivo. A porta recalcula os fechamentos antes de montar. O estado da porta real hoje, as linhas que esse recálculo produziria e o resultado efetivo da tentativa ficam [nao verificado]. [app/folha/porta_export.py:817](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:817) [app/folha/porta_export.py:1423](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1423) [app/folha/views.py:135](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:135) [app/folha/views.py:214](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:214) [app/folha/views.py:275](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:275) [app/folha/views.py:282](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/views.py:282)

## 27/11 12:03 · sessao 22ad6ec9cbe8 · ORFAS-VOLTOU · sonda · origem=Code

Na montagem em memória do TXT de **11/2026**, a sonda identificou **87 células com o colaborador fora**, **5 com o colaborador dentro e a data sem `reg11`** e **337 que acionaram `furo_espelho`**. A presença no TXT ficou **[nao verificado]** para as 561 células da empresa 2. `sonda.py`, saída fornecida, linhas 4–7.

| Empresa | Células | Colab. fora do TXT | Colab. dentro, data sem `reg11` na janela | Células que retêm | Efeito direto em rubrica/horas | Fonte |
|---|---:|---:|---:|---:|---:|---|
| 2 | 561 | [nao verificado] | [nao verificado] | 286 | 0 | `sonda.py`, saída linha 4 |
| 3 | 91 | 87 | 4 | 51 | 0 | `sonda.py`, saída linha 5 |
| 4 | 1 | 0 | 1 | 0 | 0 | `sonda.py`, saída linha 6 |
| **Total** | **653** | **87 verificados** | **5 verificados** | **337** | **0** | `sonda.py`, saída linha 7 |

**As somas fecham nas 653 células**, com zero fora das empresas 2–4. Por colaborador, são 5 dentro + 87 fora + 561 com presença indeterminada. Por data, são 137 fora da janela + 0 com `reg11` verificado + 5 sem `reg11` com colaborador dentro + 68 sem `reg11` com colaborador fora + 443 datas indeterminadas na janela. As 337 retenções se sobrepõem a essas contagens. A sonda encontrou 1.895 perguntas, 189 colaboradores no filtro, 179 com célula e 12 perguntas sem FK de célula. A referência do anexo traz 184 colaboradores e 658 células. A identidade entre os dois conjuntos de células é **[nao verificado]**. `sonda.py`, saída linhas 1–8. [app/folha/export.py:977](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:977)

A saída trouxe `layout_invalido` na empresa 2 e `sem_medicao` nas três empresas. A retenção vem da classificação, que exclui o colaborador inteiro da montagem quando o motivo é `furo_espelho`. O erro específico de layout da empresa 2 e a emissão efetiva do TXT são **[nao verificado]**. `sonda.py`, saída linhas 4–6. [app/folha/export.py:266](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:266) [app/folha/export.py:725](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:725) [app/folha/export.py:977](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:977) [app/folha/porta_export.py:1368](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/porta_export.py:1368)

O **zero direto** vale para fechar apenas a pergunta, mantendo os insumos da folha. O `reg11` registra data de falta. A contribuição de um dia sem `reg11` às rubricas mensais e o efeito de materializar uma resposta ou decidir o dia são **[nao verificado]**. [app/folha/export.py:215](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:215) [app/folha/export.py:734](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/folha/export.py:734) [app/chamados/services/validacao.py:777](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/chamados/services/validacao.py:777)

## 27/11 12:02 · sessao aac47beae035 · FERIADO-BOTAO-CORTADO · construtor · origem=Code

Adicionei um único arquivo, [test_smoke_clique_feriado_vira_trabalho.py](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:1). A fixture usa datas fixas e confere que a função real oferece a ação em 20/11/2026, com a célula em folga e quatro batidas. [Fixture e conferência](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:144)

- **Página × `base.html` e `base_app.html`** — renderiza o calendário mensal real no conteúdo de cada casca. [Renderização](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:209)
- **Drawer × `base.html` e `base_app.html`** — abre o drawer genérico e clica em Espelho, com as respostas das duas views fornecidas em memória. [Abertura do drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:60)

Nos quatro cenários, o Chromium confere o `summary` dentro da célula e da viewport, clica no elemento atingido no centro do botão e verifica abertura, painel inteiro na viewport, distância de até 16 px e `action` do formulário igual à porta real. A rolagem só registra deslocamento, evento e fechamento. [Medições](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:89)

O **RED esperado no drawer** vem de `popover-acao.js:44` e `:75–76`. O script aplica `position:fixed` e grava coordenadas da viewport, mas o `transform` do drawer muda a referência do `fixed`. O `backdrop-filter` de `.hx-shell` também merece leitura nas duas páginas. O efeito geométrico em ambos os casos segue [nao verificado]. [Posicionador](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/js/popover-acao.js:38) [Drawer](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/templates/core/_drawer_generico.html:16) [CSS da casca](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/static/css/hasner-ponto.css:283)

**Comandos para o Code**, a partir de `app/`. Os mutantes alteram o HTML do ensaio. [Mutantes CSS](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:38) [Mutante da porta](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/core/tests/test_smoke_clique_feriado_vira_trabalho.py:192)

```bash
alvo=core.tests.test_smoke_clique_feriado_vira_trabalho
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=gatilho-cortado PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=painel-recortado PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=painel-distante PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
SMOKE_FERIADO_MUTANTE=porta-trocada PYTHONDONTWRITEBYTECODE=1 python3 manage.py test "$alvo" --settings=config.settings.ci
```

A sintaxe Python passou. A execução na cópia parou no import de `django.core`, antes do banco de teste e do Chromium. Resultado dos quatro cenários, rolagem, mutantes, envio do POST e comportamento em produção seguem [nao verificado]. [Import do Django](/home/ronald/wt-codex-feriado-botao-cortado-112417/app/manage.py:11) [nao verificado]

## 27/11 11:59 · sessao ba05ad29f6be · ORFAS-VOLTOU · leitura · origem=Code

## 1. TXT do Domínio — competência 11/2026

**A pergunta órfã aberta, por si só, não tira colaborador ou dia, não altera rubrica ou horas e não bloqueia a geração. A célula pode tirar o colaborador inteiro.** O POST calcula `FechamentoMensal` em memória, classifica e monta com os mesmos objetos. A foto gravada ainda serve de guarda para abrir a geração e de alvo do carimbo de aprovação. O `DiaPago` gravado entra na conferência da porta, sem alimentar as linhas do TXT. `app/folha/views.py:132–173,214–219,249–288` `app/folha/porta_export.py:319–341,392–398,693–706,1364–1398`

- **Retenção.** `classificar_export → motivos_retencao_espelho → motivos_retencao_celula → _dias_sem_decisao` lê veredito, protestos, vínculo e, na exceção da pausa, ata/DNA. `indefinida`, `furo` e `nunca_bateu` acusam. Aplicados o recorte e as dispensas, um dia que retém põe o colaborador inteiro em `fora/furo_espelho`. A guarda de cadastro zero também consulta a existência de células para explicar a retenção. `app/escala/models.py:1780–1786` `app/folha/export.py:945–982,1157–1182,1214–1222,1244–1316`
- **Apuração e emissão.** O fechamento alimenta o motor com célula/ata, batidas apuráveis, escalas e ausências. Também lê a grade da célula para os totais de previsto, abono e realizado e chama o mesmo retentor. A montagem traduz os totais mensais pelo catálogo e lê ausências aprovadas para o bloco datado de faltas. Nenhum desses caminhos consulta `PerguntaDisputa` ou o estado da disputa. A conferência do POST lê o carimbo da porta. O caminho de badge que consulta chamados por célula é de modo gestor, enquanto o medidor chama o espelho no modo padrão. `app/ponto/services/fechamento.py:617–659,762–776,1037–1092` `app/ponto/motor_calculo_v2.py:638–675` `app/folha/export.py:71–87,169–185,402–461,720–755` `app/folha/porta_export.py:482–502,1423–1437` `app/ponto/services/espelho.py:613–621,1230`
- **Medida honesta.** Presença do colaborador é matrícula em `reg10` efetivamente emitido. Presença de data é `reg11` ligado ao `reg10` do bloco de faltas. Ausência de `reg11` deixa a contribuição daquele dia às rubricas mensais **[nao verificado]**. Para o contrafactual, fechar só a pergunta, mantendo os insumos, tem **efeito direto zero por leitura do código**. Validar uma resposta ou decidir o dia pode plantar batida, aprovar ausência ou mudar a célula. O efeito desse ato exige definir o desfecho e rodar o DIFF antes/depois em sombra com escrita. Função pura que reproduza esse ato até o TXT **[nao verificado]**. `app/folha/export.py:215–255,734–755,977–982` `app/chamados/services/validacao.py:777–805` `app/chamados/services/materializacao.py:1584–1592` `app/ponto/services/veredito_celula.py:324–357`

O anexo registra **1.895 perguntas, 184 colaboradores, 658 células e 12 perguntas sem FK de célula**. A sonda refaz o mesmo filtro e mede o estoque que encontrar na sombra. Identidade com as 658 originais, totais por empresa e resultado do TXT **[nao verificado]**. A coluna `celulas_retendo` conta causas lidas no retentor quando `furo_espelho` é o motivo efetivo. As colunas de presença contam células, inclusive quando várias pertencem ao mesmo colaborador. `autopsia_v2.md:3–4` `r2.md:3–17` `app/folha/export.py:977–982,1202–1204,1268–1271`

### Rascunho da sonda

Rodar via stdin no `manage.py tenant_command shell --schema=juliani --settings=config.settings.sombra`. O comentário inicial dá o arranque sem configurar os handlers de arquivo. O código lê o estado da porta sem dispará-la e mostra a montagem em memória mesmo se a porta estiver fechada. Execução da sonda **[nao verificado]**. `bin/sonda_leitura.sh:235–241` `app/config/settings/saas.py:118–143,160–184` `app/config/settings/sombra.py:27–50` `app/folha/porta_export.py:1360–1398,1423–1437`

```python
# Do diretório app/, com /app/logs já existente, passar este bloco por stdin:
# python -B -c 'import os,runpy; from django.conf import settings; os.path.isdir("/app/logs") or __import__("sys").exit("[nao verificado]"); os.environ["DJANGO_SETTINGS_MODULE"]="config.settings.sombra"; settings.LOGGING_CONFIG=None; runpy.run_path("manage.py",run_name="__main__")' tenant_command shell --schema=juliani --settings=config.settings.sombra

from collections import Counter, defaultdict
from datetime import date
import logging

from django.db import connection, transaction
from chamados.models import PerguntaDisputa
from colaboradores.models import Empresa
from core.observ import engolidos
from escala.models import CelulaDia
from folha.export import (
    STATUS_NO_TXT, classificar_export, codigo_no_sistema_dominio,
    conteudo_do_txt, janela_competencia, montar_linhas_export,
    motivos_retencao_celula, partes_da_linha, validar_layout_dominio,
)
from folha.porta_export import estado_da_porta
from ponto.models import FechamentoMensal
from ponto.services.fechamento import (
    recalcular_fechamento_mes, universo_da_competencia,
)

COMP = (11, 2026)
EMPRESAS = (2, 3, 4)
NV = "[nao verificado]"
CAMPOS = (
    "celulas", "colabs_com_celula", "colab_no_txt", "colab_fora_txt",
    "colab_nv", "fora_janela", "reg11", "sem_reg11_colab_no_txt",
    "sem_reg11_colab_fora_txt", "dia_nv", "celulas_retendo",
    "rubrica_horas_efeito_direto", "contrafactual_material_nv",
)


class ContadorDeAvisos(logging.Handler):
    def __init__(self):
        super().__init__(logging.WARNING)
        self.total = 0

    def emit(self, record):
        self.total += 1


def medir_empresa(empresa, celulas):
    ini, fim = janela_competencia(empresa, *COMP)
    n = Counter(celulas=len(celulas),
                colabs_com_celula=len({colab for _, colab, _ in celulas}),
                contrafactual_material_nv=len(celulas))
    porta = estado_da_porta(empresa, *COMP)["codigo"]
    tem_gravado = FechamentoMensal.objects.filter(
        colaborador__empresa=empresa, mes=COMP[0], ano=COMP[1],
        status__in=STATUS_NO_TXT,
    ).exists()

    matriculas, datas, retendo = {}, defaultdict(set), set()
    try:
        codigo_no_sistema_dominio(empresa)
    except ValueError:
        montagem = "sem_integracao"
    else:
        fechs = recalcular_fechamento_mes(
            *COMP, empresa_id=empresa.pk, somente_leitura=True,
            lavrar_dias_pagos=False, retornar_objeto=True,
        )
        esperados = universo_da_competencia(*COMP, empresa.pk).values_list("pk", flat=True)
        if fechs.keys() != set(esperados):
            raise RuntimeError("universo_fechamento_incompleto")

        itens = classificar_export(empresa, *COMP, fechamentos=list(fechs.values()))
        linhas, elegiveis = montar_linhas_export(
            empresa, *COMP, itens=itens, fechamentos=list(fechs.values()),
        )
        montagem = "layout_invalido" if validar_layout_dominio(linhas) else (
            "valido" if linhas else "sem_linhas"
        )
        if montagem != "layout_invalido":
            for colab in elegiveis:
                fech = fechs[colab]
                if fech.status in STATUS_NO_TXT:
                    matriculas[colab] = fech.colaborador.codigo_dominio.strip().zfill(10)
            atual = None
            for linha in conteudo_do_txt(linhas).splitlines():
                parte = partes_da_linha(linha)
                if parte is None:
                    raise RuntimeError("linha_ilegivel")
                if parte["reg"] == "10":
                    atual = parte["matricula"]
                    datas.setdefault(atual, set())
                elif atual is not None:
                    datas[atual].add(parte["data"])
                else:
                    raise RuntimeError("reg11_sem_reg10")

        alvos = {colab for _, colab, _ in celulas}
        for item in itens:
            colab = item["colaborador_id"]
            if colab in alvos and item["motivo"] == "furo_espelho":
                _, dias = motivos_retencao_celula(
                    fechs[colab].colaborador, ini, fim, com_dias=True,
                )
                retendo.update(d["celula_id"] for d in dias)

    donos = Counter(matriculas.values())
    for pk, colab, dia in celulas:
        mat = matriculas.get(colab)
        incerto = montagem in ("sem_integracao", "layout_invalido") or (
            mat in datas and donos[mat] > 1
        )
        presente = not incerto and mat in datas
        n["colab_nv" if incerto else "colab_no_txt" if presente else "colab_fora_txt"] += 1
        n["celulas_retendo"] += pk in retendo

        if not ini <= dia <= fim:
            n["fora_janela"] += 1
        elif incerto:
            n["dia_nv"] += 1
        elif not presente:
            n["sem_reg11_colab_fora_txt"] += 1
        elif dia.strftime("%Y%m%d") in datas[mat]:
            n["reg11"] += 1
        else:
            n["sem_reg11_colab_no_txt"] += 1

    if n["celulas"] != sum(n[k] for k in CAMPOS[2:5]):
        raise RuntimeError("soma_colab")
    if n["celulas"] != sum(n[k] for k in CAMPOS[5:10]):
        raise RuntimeError("soma_dia")
    return n, (porta, int(tem_gravado), montagem)


def sondar():
    cfg = connection.settings_dict
    if (cfg["NAME"], cfg["HOST"], connection.schema_name) != (
        "sombra", "sombra_db", "juliani",
    ) or not connection.get_autocommit() or connection.in_atomic_block:
        raise RuntimeError("envelope")

    with transaction.atomic():
        with connection.cursor() as cur:
            cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cur.execute(
                "SELECT current_database(), current_schema(), "
                "current_setting('transaction_isolation'), "
                "current_setting('transaction_read_only')"
            )
            if cur.fetchone() != ("sombra", "juliani", "repeatable read", "on"):
                raise RuntimeError("envelope_sql")

        perguntas = PerguntaDisputa.objects.filter(
            data_evento__gte=date(2026, 10, 21), chamado_destino__isnull=True,
            via_resolucao="", validada_em__isnull=True, materializada_em__isnull=True,
        ).order_by()
        com_fk = perguntas.filter(celula_id__isnull=False)
        por_empresa = defaultdict(list)
        for pk, emp, colab, dia in CelulaDia.objects.filter(
            pk__in=com_fk.values("celula_id"),
        ).values_list("pk", "colaborador__empresa_id", "colaborador_id", "data"):
            por_empresa[emp].append((pk, colab, dia))

        universo = (perguntas.count(),
                    perguntas.values("colaborador_id").distinct().count(),
                    sum(map(len, por_empresa.values())),
                    perguntas.filter(celula_id__isnull=True).count())
        saida = [
            ("universo_perguntas_colabs_celulas_sem_fk", universo),
            ("referencia_anexo", (1895, 184, 658, 12)),
            ("colunas", CAMPOS),
        ]
        total = Counter()
        for emp_id in EMPRESAS:
            n, estado = medir_empresa(Empresa.objects.get(pk=emp_id), por_empresa[emp_id])
            total.update(n)
            saida.append((emp_id, tuple(n[k] for k in CAMPOS), estado))
        fora = sum(len(v) for emp, v in por_empresa.items() if emp not in EMPRESAS)
        if total["celulas"] + fora != universo[2]:
            raise RuntimeError("soma_universo")
        return saida + [("total_234", tuple(total[k] for k in CAMPOS)),
                        ("celulas_fora_234", fora)]


raiz = logging.getLogger()
handlers, nivel = raiz.handlers[:], raiz.level
avisos = ContadorDeAvisos()
raiz.handlers = [avisos]
raiz.setLevel(logging.WARNING)
antes = engolidos()
try:
    saida = sondar()
    if avisos.total or engolidos() != antes:
        raise RuntimeError("leitura_incompleta")
except Exception:
    raise SystemExit(f"SONDA_INCOMPLETA {NV}") from None
finally:
    raiz.handlers, raiz.level = handlers, nivel

for registro in saida:
    print(*registro)
print("CONTRAFACTUAL_MATERIAL", NV)
```

## 2. Onde mora o número

- **Autoridade.** `perguntas_orfas_reatavies(colab)` define o universo elegível. O comando soma esse queryset por colaborador e publica a medida fora da soma que dá RC 2. Quem tenta o reate de fato é `reatar_pergunta_orfa`. O cron encaminha a saída a `logs/reconciliador.log` a cada 15 minutos. **3.735 às 11h** é a leitura do anexo. Valor atual **[nao verificado]**. `app/chamados/reconciliador.py:1640–1695` `app/chamados/management/commands/reconciliar_chamados.py:57–58,97–98,115–124` `app/config/crons.py:395,1913–1917` `autopsia_v2.md:17`
- **Publicação.** `app/core/placar_estrutural.py::placar()` lê `RESULTADOS` em memória. Para esta medida, hoje ele **nem chama a função real nem lê o log**. O placar estrutural que chega ao leitor é o do **`ESTADO.md`**, gerado em `bin/gerar_estado.py` e publicado junto com o RELATO por `bin/relato.sh`. `app/core/esteira_vigia.py` fornece o escritor e a trava do RELATO. `hook_stop_fila1` lê as duas linhas operacionais do topo. `handoff_sessao.sh` escreve o handoff e hoje exibe o R6 e a linha de disco. O `PLACAR.md` publicado à parte deriva da GRADE-MESTRA. `app/core/placar_estrutural.py:627–654` `bin/gerar_estado.py:60–104,408–414` `bin/relato.sh:48–49,112,125–130,147–158` `app/core/esteira_vigia.py:815–892` `bin/hook_stop_fila1.py:446–484` `bin/handoff_sessao.sh:85–123,195–218,328–337`
- **Encaixe mínimo — fila de `bin/`.** Ler uma vez a última linha válida emitida pelo vigia da frota e publicar a mesma frase no `ESTADO.md` e no topo visível do RELATO, abaixo das duas linhas operacionais, pelo mecanismo de publicação usado pelo disco. O handoff pode ler essa projeção. O comando continua sendo o único produtor do total. A linha atual do contador sai sem hora própria, então o leitor só deve publicar um horário se puder vinculá-lo à mesma rodada. Essa ligação no log fornecido **[nao verificado]**. `app/docs/LEIS.md:60–61` `app/chamados/management/commands/reconciliar_chamados.py:97–98,115` `bin/cron_run.sh:7–13` `bin/disco_estado.py:25–33,413–453` `bin/gerar_estado.py:320–326` `aval.txt:1`

## 27/11 11:52 · sessao a6f52086aa3d · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

**Os sete casos apontam para outra causa: a apuração do intermitente, sem dobra automática de feriado.** Com o cadastro da sonda, a cópia apura as horas em `resultado.periodos`, como trabalho realizado sem jornada prevista na escala. O caminho dos 75 passa por `periodos_ft` → `ft_sem_escala` e deixa `folga_sem_escala_min` na lavra. Os sete estão sem essa marca. `saida-sonda.tsv:88–89` · [motor_calculo_v2.py:2560](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2560) · [motor_calculo_v2.py:2605](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2605) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259)

### Os sete casos

Em todos, a sonda mostra município `4103`, a data nos dois conjuntos de feriados impressos, célula gerada com `trabalha=False` e `dna.marcos=None`, base e ciclo `intermitente`, `trabalha_em_feriado=True` e `feriado_vira_folga=False`. A janela de HE estava ativa desde `2026-08-21`, com `decisao_he_vigente=[]`. Os três campos `horas_extras_100`, `horas_extras_100_feriado` e `horas_folga_trabalhada` são `0.0` em cada lavra. As referências à saída contam `sonda_leitura` como linha 1. `Saída da sonda anexada:1–86`

| Empresa / colab | Data | Vínculo / tipo | Motivo na sonda | Trabalhadas (h) | Classificação pela cópia | Fonte |
|---|---|---|---|---:|---|---|
| 2 / 365 | 02/11/2026 | 318 / 470 | A2 | 12.0 | Outra causa, intermitente sem dobra | `Saída da sonda:3–14` · `saida-sonda.tsv:16` |
| 2 / 365 | 15/11/2026 | 318 / 470 | A2 | 11.036849166666666 | Outra causa, intermitente sem dobra | `Saída da sonda:15–26` · `saida-sonda.tsv:17` |
| 2 / 365 | 20/11/2026 | 318 / 470 | A2 | 11.008139166666666 | Outra causa, intermitente sem dobra | `Saída da sonda:27–38` · `saida-sonda.tsv:18` |
| 2 / 830 | 15/11/2026 | 1074 / 466 | A2 | 11.885994444444444 | Outra causa, intermitente sem dobra | `Saída da sonda:39–50` · `saida-sonda.tsv:42` |
| 3 / 85 | 02/11/2026 | 1140 / 483 | A2 | 11.897632222222223 | Outra causa, intermitente sem dobra | `Saída da sonda:51–62` · `saida-sonda.tsv:76` |
| 3 / 85 | 20/11/2026 | 1140 / 483 | A2 | 11.545001388888888 | Outra causa, intermitente sem dobra | `Saída da sonda:63–74` · `saida-sonda.tsv:77` |
| 3 / 146 | 02/11/2026 | 845 / 383 | A8 | 13.50255722222222 | Outra causa, intermitente sem dobra | `Saída da sonda:75–86` · `saida-sonda.tsv:81` |

### (a) Por onde entram as horas

- **Folga trabalhada.** O fechamento passa o vínculo da fatia a `calcular_mes`. Para ciclo `intermitente`, o motor pula `EC.eh_dia_trabalho` na classificação da folga do par. Ainda pode classificar como folga pelo dia da semana cadastrado no template. Na reconstrução destes sete, o caminho compatível com as horas positivas e a marca vazia é `resultado.periodos` → `DiaPago.horas_trabalhadas`. O filtro de escala certa só recebe `periodos_ft`. A2 e A8 fariam reprovar um período que chegasse ao filtro. [fechamento.py:306](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:306) · `motor_calculo_v2.py:2436–2450,2474–2478,2558–2566,2581–2607` · `fechamento.py:953–959,1225–1228` · `dia_pago.py:173–184,234–272` · `escala_certa.py:31–51`
- **Feriado e janela de HE.** O fechamento fornece os feriados do colaborador, e a fábrica escolhe `MotorIntermitente` pela base ou pelo ciclo. O construtor substitui o conjunto recebido por um conjunto vazio que o motor trata como injetado. `eh_feriado` responde `False`, então a dobra direta falha mesmo com `fd=1`, que carimba o flag `feriado_em_dobra`. Para o par apurado fora de folga, o bloqueio integral da janela de HE retorna antes de consultar a decisão. A promoção pelo fechamento exige `periodos_ft`, escala certa e `feriado_vira_folga=True`. [fechamento.py:756](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:756) · `regua_cct.py:421–425,457–479` · [motor_calculo_v2.py:3694](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:3694) · `motor_calculo_v2.py:961,1007–1011,1562–1571,2925–2929,3764–3767` · `fechamento.py:953–980`

### (b) De onde vem o previsto

**Na reconstrução, as sete datas ficam fora de `datas_previstas_trabalho` e têm zero minuto previsto na consulta do motor para a própria data.** O fechamento usa `EC.eh_dia_trabalho`, que devolve `celula.trabalha`. O juiz de minutos também consulta esse método e retorna `0` antes de ler os marcos quando a resposta é `False`. Já a janela de HE pode ler `EC.marcos_do_dia` para ajustar as pontas do par. Com `dna.marcos=None`, essa leitura cai em `TipoEscala.marcos_do_dia`, que fornece horários. Os `720` do DNA não são a jornada prevista usada nessa consulta. [fechamento.py:749](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:749) · `escala/models.py:326–345,1098–1107,1128–1144` · `escala/utils.py:2310–2325` · `motor_calculo_v2.py:1362–1367,1646–1660,2403–2404`

**[nao verificado] na lavra de 25/11:** se o cadastro era idêntico ao da sombra, quais pares formaram cada total, se houve algum período de zero minuto em `periodos_ft`, os horários efetivos do template e eventual recorte de entrada ou saída. A sonda mostra IDs de batidas por dia e totais lavrados. Também ficam **[nao verificado]** o motivo individual dos 75, o pagamento efetivo e a confirmação do hash via Git na cópia. `Saída da sonda:1–86` · `saida-sonda.tsv:88–89` · `motor_calculo_v2.py:2513–2521,2872–2878` · `dia_pago.py:173–184,259–272`

## 27/11 11:50 · sessao 2b67a6c991ff · FERIADO-BOTAO-CORTADO · verificador · origem=Code

## Veredito — REPROVADO

O diff preserva o predicado e dá ao botão um espaço próprio na célula mensal. O bloqueio está no painel da ação: no drawer, o JavaScript mede pela janela e posiciona dentro de um ancestral transformado. O verde informado cobre a estrutura do HTML, mas o clique no feriado de 20/11 dentro do drawer e nas duas cascas segue [não verificado]. [app/templates/colaboradores/partials/_calendario_grade.html:68 e :95–99, no diff] [app/templates/core/_drawer_generico.html:3 e :16] [app/static/js/popover-acao.js:38–45 e :75–76] [nao verificado]

1. **ALTA — O painel pode abrir deslocado e cortado no drawer.** Na ficha aberta como página, o `position:fixed` permite escapar do recorte da célula e da nova região rolável. No drawer, `transform:translateX(0)` faz do próprio drawer a referência do `fixed`, mas o script continua calculando `left` e `top` pela janela. O drawer também tem `overflow-y:auto`. Isso atinge a fita de diagnóstico e o calendário mensal aberto na aba Espelho ou pelo atalho da Folha. A incompatibilidade já existia no posicionador. Sem o JS, o painel usa `position:absolute` dentro da célula recortada. A geometria do clique real segue [não verificado]. [app/templates/core/_drawer_generico.html:3, :16 e :132–142] [app/folha/retrato_folha.py:1019] [app/templates/folha/_retrato_o_que_fazer.html:73] [app/static/js/popover-acao.js:38–76] [app/templates/ponto/partials/_feriado_vira_trabalho.html:7]

2. **MÉDIA — A região é focável e nomeada, mas o fluxo de teclado do popover tem lacunas.** `tabindex="0"`, `role="region"` e o `aria-label` com a data dão à região uma parada de Tab e um nome, inclusive em células sem conteúdo suficiente para rolar. O listener global em captura fecha todos os popovers em qualquer `scroll`, inclusive ao rolar a nova região ou o próprio painel quando o script lhe dá `overflow-y:auto`. `Escape` fecha o painel sem devolver explicitamente o foco ao `summary` e também dispara o fechamento do drawer. Foco visível, rolagem por teclado e anúncio em leitor de tela seguem [não verificado]. [app/templates/colaboradores/partials/_calendario_grade.html:99, no diff] [app/static/js/popover-acao.js:64–72 e :100–110] [app/templates/core/_drawer_generico.html:75–82]

3. **MÉDIA — O teste sela a estrutura e deixa de fora a interação que decide o aceite.** Os controles são localizados pelo destino do POST e pela data. O teste exige um bloco irmão anterior às quatro marcações, verifica o CSS e o `tabindex`, e também confere o rótulo literal. Usa datas e horários fixos e objetos em memória, sem relógio, banco ou CNPJ. Depende da ordem explícita das três células e das quatro marcações. O CNPJ `11222333…` pertence ao smoke de retratação, que usa uma casca HTML própria. O teste novo não mede visibilidade, clique, rolagem, impressão ou posicionamento do painel. RED, verde na pista e mutantes foram informados no pedido. O smoke específico de 20/11 pedido no aval e o clique nas duas cascas seguem [não verificado]. [app/colaboradores/tests/test_feriado_botao_cortado.py:10–15, :32–70 e :72–115, no diff] [app/core/tests/test_smoke_retratar_1366.py:94–96, :114–118 e :138–179] [aval.txt:1] [CLAUDE.md:207]

### Predicado, consumidores e efeitos

- **Predicado e esmeril.** A oferta continua sendo `pode_editar_dia and dia.feriado_vira_trabalho_oferecido`, com a mesma URL e os mesmos dados do formulário. Perfil, drawer e POST continuam usando `editar_escalas`. O POST consulta o campo do mesmo `contexto_calendario`, que decide pelo feriado do colaborador e pela célula existente em folga. O diff repete a condição para aplicar CSS e atributos de acessibilidade. Não introduz `except`, regra de negócio paralela ou mudança no motor/models. [app/templates/colaboradores/partials/_calendario_grade.html:131–133, cópia local, e :68 e :95–99, no diff] [app/colaboradores/views.py:2005–2010] [app/colaboradores/services/drawer.py:156–159] [app/colaboradores/services/calendario.py:785–793] [app/ponto/views.py:3607 e :3622–3633]

- **Consumidores.** `_feriado_vira_trabalho.html` tem um único include direto, na grade. A grade tem três: `calendario_partial.html` usa `mes` em Mês, Semana, Quinzena e Competência, na ficha, na aba Espelho do painel aberto por Colaboradores, Benefícios ou Retrato da Folha, e no atalho “Resolver dia no calendário” da Folha. O drawer de diagnóstico usa `fita` nas linhas e nas pendências do Painel Situacional, acessível pelo Painel Operacional, Central de Chamados e Vínculos. Auditoria de espelhos usa a grade padrão com `only`, sem permissão de edição no include. `base.html` e `base_app.html` incluem o mesmo drawer e carregam o mesmo script. Uma entrada concreta da grade a partir de uma página da casca app segue [não verificado]. [app/templates/colaboradores/partials/_calendario_grade.html:132] [app/templates/colaboradores/calendario_partial.html:50–67] [app/templates/colaboradores/detalhe.html:215] [app/templates/colaboradores/partials/_lista_tabela.html:16] [app/templates/core/_drawer_generico.html:104 e :140] [app/templates/colaboradores/beneficios_lista.html:86] [app/templates/folha/_retrato_lista.html:61] [app/templates/folha/_retrato_raiox.html:68–70] [app/templates/folha/_retrato_o_que_fazer.html:73] [app/templates/colaboradores/partials/_drawer_diagnostico.html:35] [app/templates/colaboradores/partials/_ps_linha.html:4] [app/templates/colaboradores/partials/pendencias_disputa.html:9] [app/templates/core/painel_operacional.html:139] [app/templates/chamados/painel_gestao.html:14] [app/templates/colaboradores/vincular.html:25] [app/templates/relatorios/furos.html:42] [app/templates/base.html:399 e :440] [app/templates/base_app.html:154 e :194]

- **Células e saídas.** Sem oferta, inclusive em mês sem feriados, a nova `div` fica sem flex, rolagem ou parada de Tab, e a ordem relativa do conteúdo é preservada. Com oferta, a fita muda a ordem. No mês, a ação fica fora do scroll e 6+ marcações permanecem no HTML dentro da região rolável. Em tela estreita, o rótulo quebra linha, mas a célula continua com 132 px e a ficha tem uma coluna lateral fixa de 420 px. Visibilidade nos extremos de largura e impressão pelo navegador seguem [não verificado]. A altura fixa e o recorte podem cortar conteúdo na impressão. O PDF de Auditoria segue pelo gerador ReportLab, que recebe os dados diretamente. [app/templates/colaboradores/partials/_calendario_grade.html:65–68 e :95–99, no diff] [app/templates/ponto/partials/_feriado_vira_trabalho.html:5–6, no diff] [app/templates/colaboradores/detalhe.html:87] [app/relatorios/views.py:1628–1634] [app/relatorios/furos_pdf.py:73–103]

- **LEIS-UI.** L2 e L4 são preservadas pelo predicado compartilhado e pela renderização condicional. O diff não introduz diálogo nativo, botão PDF ou mudanças nos ícones e tamanhos de estado, conforme L8–L11. O novo `aria-label` nomeia a região. Pela L7, a ajuda explicativa literal do formulário continua sendo uma pendência anterior ao diff. Pela L5, o formulário não tem o componente de espera e a duração da ação em produção segue [não verificado]. [app/docs/LEIS-UI.md:23, :39, :56, :77, :87, :95, :120 e :174] [app/templates/ponto/partials/_feriado_vira_trabalho.html:7–12] [app/templates/colaboradores/partials/_calendario_grade.html:95–103, no diff]

## 27/11 11:48 · sessao 30285ce09825 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

## FERIADO-NAO-PREVISTO-L086 — sonda na sombra

Apenas dias-colab com `horas_trabalhadas > 0`. As referências a `saida-sonda.tsv` seguem a numeração do anexo, com `sonda_leitura` na linha 1. `saida-sonda.tsv:1–4,87–90`

| Empresa | Dias no recorte | (a) `folga_sem_escala_min > 0` | (a) Soma dos minutos em horas | (b) Dias com rubrica alternativa positiva | (b) Soma das duas rubricas | (c) Dias no ramo não explicado | Fonte |
|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 69 | 65 | 459.7395 h (27584.37 min) | 0 | 0.0 h | 4 | `saida-sonda.tsv:5–73,88` |
| 3 | 13 | 10 | 74.201 h (4452.06 min) | 0 | 0.0 h | 3 | `saida-sonda.tsv:74–86,89` |
| 4 | 0 | 0 | 0 h (0 min) | 0 | 0.0 h | 0 | `saida-sonda.tsv:87–90` |

Na cópia, os períodos barrados pelo filtro de escala certa entram em `horas_trabalhadas`, com os minutos registrados em `detalhe.folga_sem_escala_min`. O item (b) soma `horas_folga_trabalhada` e `horas_extras_100_feriado` registradas na sonda. Pagamento efetivo [nao verificado]. [fechamento.py:959](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:959) · [fechamento.py:1004](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1004) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259) · `saida-sonda.tsv:4–86`

### (c) Ramo não explicado

| Empresa | Colab ID | Data | Horas trabalhadas | Fonte |
|---:|---:|---|---:|---|
| 2 | 365 | 2026-11-02 | 12.0 | `saida-sonda.tsv:16` |
| 2 | 365 | 2026-11-15 | 11.036849166666666 | `saida-sonda.tsv:17` |
| 2 | 365 | 2026-11-20 | 11.008139166666666 | `saida-sonda.tsv:18` |
| 2 | 830 | 2026-11-15 | 11.885994444444444 | `saida-sonda.tsv:42` |
| 3 | 85 | 2026-11-02 | 11.897632222222223 | `saida-sonda.tsv:76` |
| 3 | 85 | 2026-11-20 | 11.545001388888888 | `saida-sonda.tsv:77` |
| 3 | 146 | 2026-11-02 | 13.50255722222222 | `saida-sonda.tsv:81` |

### (d) Colab 1036 em 20/11

**Confere.** Na empresa 2, em 2026-11-20, constam **437.1 min** de `folga_sem_escala_min` (**7.285 h** pela conversão) e **7.28502 h** trabalhadas. As duas rubricas de (b) estão em `0.0 h` e `folga_feriado_min` está como `None`. O dia entra em (a). `saida-sonda.tsv:72`

## 27/11 11:48 · sessao 9781db85a25e · RC-SEM-PROXIMA · verificador · origem=Code

## REPROVADO — RC-SEM-PROXIMA, ver4, c4b `bd4f156cb`

P1 cura o `.rc` vencido ilegível, P2 entrega o diagnóstico no patch do leitor e P4 fixa os instantes dos dois testes. P3 corrige os três casos de Nova York do pedido, mas ainda pode listar após **10 s reais** ou ocultar uma parada de **20 min reais**. `bin/rc_sem_colher.py:94–102,126–162`, `bin/tests/test_rc_sem_colher.sh:179–216,282–321`, `vercodex.py:165–175` no anexo.

### Achados numerados

1. **ALTA — P3 ainda perde ocorrências reais.** `_instante_da_marca` tenta os dias `0,1,2` com `isdst=0,1`, confere o `HH:MM` retornado por `localtime` e escolhe o maior instante no intervalo inclusivo `[piso, agora]`. A regra de prazo funciona quando a ocorrência real entra no conjunto. Confirmei dois contraexemplos no `main`, com relógio e entrada injetados em memória. `bin/rc_sem_colher.py:126–140,157–162,184–198`.

   - **Listagem precoce, `America/Caracas`, 09/12/2007.** As duas `02:40` têm `isdst=0`. Com `.rc` às `02:20 −0400`, marca real às `02:40:10 −0430` e `agora` às `02:40:20 −0430`, o juiz escolheu `02:40 −0400`, imprimiu `RC SEM PROXIMA` e devolveu `3` após **10 s reais**. Com `.rc` às `02:35 −0430` e `agora` às `03:00:11 −0430`, a candidata antiga caiu abaixo do piso e a mesma marca sumiu das duas listas. `bin/rc_sem_colher.py:113–122,133–140,153–162,191–198`.
   - **Parada oculta no fim do DST, `America/Moncton`.** `.rc` em `29/10/2006 00:00:30 −0300`. O relógio voltou para o dia 28, a marca foi escrita às `23:10:10 −0400` e a consulta ocorreu às `23:30:11 −0400`. A marca sucedeu ao `.rc` em tempo real, mas sua data civil é `k=-1`. O juiz devolveu `0`, stdout vazio, após **20 min e 1 s**. Aqui as duas leituras têm `isdst` diferentes. `bin/rc_sem_colher.py:113–122,133–140,153–162,191–198`.

2. **MÉDIA — P1 cura o filtro de idade, com erro residual em candidato que perde a seleção.** `caso_vencido_ilegivel` exige a linha exata do órfão e rc `3`. Na sonda em memória, a c3 reconstruída e `abre-vencido` levantaram `PermissionError`. `caso_troca_stat_open` exige `10:10 rc=7`. `mtime-do-stat` produziu `10:00 rc=7`. A troca é uma guarda do novo `stat`. A c3 reconstruída respondeu `10:00 rc=0`, pois o gancho que efetua a troca dependia desse `stat`. As guardas de remoção e troca **depois** da visita continuam com `le-tarde-removido` e `le-tarde-trocado`. `bin/rc_sem_colher.py:94–107,115–122`, `bin/tests/test_rc_sem_colher.sh:173–178,225–240,266–272,295–301,438–447`.

   O pedido manda propagar o erro do **escolhido**, inclusive se estiver colhido. Isso cobre o exemplo do colhido ilegível do ver3 quando ele é o `.rc` escolhido. O código também abre um `.rc` dentro das 24 h que já perde para um mais novo. Confirmei `PermissionError` nas duas ordens da visita. O recibo descreve corretamente esse comportamento. O RED pedido e entregue só cobra o corte por idade. `pedido.md:6`, `recibo.md:40–43`, `bin/rc_sem_colher.py:94–102,184–190,201–202`, `bin/tests/test_rc_sem_colher.sh:295–297`.

3. **MÉDIA — O selo de P3 depende do comportamento da libc.** Em Nova York, confirmei os pares **10 s → vazio** e **prazo vencido → lista** para os dois dias civis e para a hora repetida. Na libc desta cópia, caíram `marca-dois-dias`, os três `marca-palpite-repetida*` e `marca-so-edt`. `marca-sem-conferir-hora` também caiu, em um dia comum de EST, ao aceitar um instante que se lia como `09:30` para a marca `10:30`. O selo dá `TZ` ao processo e prepara uma chamada anterior a `mktime` para enviesar `isdst=-1`. Cada mutante tem de cair individualmente. Uma libc que escolha sempre a primeira ocorrência faz o mutante sobreviver no caso `_1a`. Uma que escolha sempre a segunda faz o mutante sobreviver nos casos de segunda ocorrência. Outro host/libc e outra versão de tzdata ficam [nao verificado]. `bin/tests/test_rc_sem_colher.sh:131–134,189–216,303–329,389–403,448–454`, `recibo.md:23–26,49–50`.

4. **BAIXA — P4 curado na preparação dos casos, sem mutante da cura.** `caso_resumo` e `caso_resumo_vazio` usam `_cenario`, `agoras` fixos e o `main` real. Os quatro mutantes de resumo mordem a ordem das contas, a soma indevida, a perda de `min_idade` e o silêncio no zero. Eles não testam a volta do relógio de parede ao fixture. A c3 reconstruída passou com o fixture novo, portanto isso não demonstra um RED de P4. O teste sem `--resumo` só exige ausência da palavra e aceita saída vazia ou erro. `bin/tests/test_rc_sem_colher.sh:179–188,213–216,282–293,442–455`, `pedido.md:9,11`.

### P2, escritores e leitores

- **P2 curado no anexo, sem selo no repo conforme o pedido.** `_a` fornece exatamente os três argumentos impressos e enviados, `--min-idade 900 --resumo`. O cabeçalho mostra `bin/rc_sem_colher.py` como caminho relativo. A chamada envia também `python3` e o caminho montado com `R`. No bloco isolado, confirmei que rc `0` e `3` imprimem o último `RESUMO` recebido ou `SEM RESUMO do juiz`, com até 15 detalhes de 120 caracteres. Outro rc acrescenta a última linha de stderr ou `(stderr vazio)`. O bloco informa o rc, sem propagá-lo. `pedido.md:7`, `vercodex.py:165–177` no anexo.
- **Tupla e mtime.** `codex_pergunta.sh::_saida` escreve `rc=` no temporário e publica por `mv`. `_obras_vivas` monta `(ob, d, rc_m, rc_p, rc_txt, outro)`. O `getmtime` anterior ao `open` só decide se abre. O `fstat` do descritor fornece o mtime que passa pelo segundo corte e disputa a seleção, e `read(200)` lê o texto desse mesmo descritor. `main` materializa a tupla uma vez. Os leitores diretos são `sem_colher`, que usa os seis campos, e `sem_proxima`, que descarta o texto. Ambos usam o mtime para a folga, o `HH:MM` e a ordenação. `sem_colher` também o usa para a idade e extrai o `rc=` do texto. `sem_proxima` usa o mtime como base da inferência da marca. `bin/codex_pergunta.sh:565–574`, `bin/rc_sem_colher.py:67–70,77–123,133–164,184–198`.
- **Instante da marca.** A entrada do `RELATO` fornece o `HH:MM` da linha, descrito como vindo do `date` da sessão. O processo concreto que escreveu uma marca de produção fica [nao verificado]. `marcas_datadas` lê a linha. `sem_colher` usa a chave para excluir o `.rc`, mesmo quando falta um instante válido. `_instante_da_marca` produz o instante inferido e `sem_proxima` é seu leitor em produção. O `main` imprime o `HH:MM`, sem expor o instante inferido. `bin/rc_sem_colher.py:19–23,44–64,113–140,146–163,191–194`.
- **`ESPERA_UNICA`, sem `--resumo`.** Lê stdout e o rc do juiz, sem acessar a tupla. Com `0` entra no laço. Com `3` pula o laço. Em ambos os casos executa o `find` final e termina com sucesso. Outro rc resulta em falha da espera. A igualdade byte a byte é **falsa**. No caso `dst-dois-dias`, a c3 reconstruída imprimiu `RC SEM PROXIMA OBRA-P r.rc 23:50 colhido=00:10\n` e pulou o laço. A c4b teve stdout vazio e entrou no laço. Confirmei com o Bash da constante e dependências simuladas. `bin/disjuntor.py:1963–1969,2157–2174`, `bin/tests/test_rc_sem_colher.sh:189–193,303–309`, `bin/rc_sem_colher.py:191–198`.

### Bordas, escopo e esmeril

- **Hora pulada e virada comum.** Em Nova York, `02:10` na hora pulada dá `None`, e uma marca real às `03:10` lista depois do prazo. Um `.rc` de ontem às `23:50` aceita marca de hoje às `00:10`. O caso de março com dois dias civis ainda cabe em `84.020 s` reais. Acima de `86.400 s` reais, o `.rc` sai no filtro de idade. `bin/rc_sem_colher.py:40,81–101,133–140,157–162`, `bin/tests/test_rc_sem_colher.sh:104–110,189–193,303–309`.
- **O terceiro marcador do ver3 ficou fora de P3.** O `.rc` pode vencer antes de a marca amadurecer. Uma marca antiga pode ficar oculta quando o mesmo `HH:MM` volta a caber no teto, inclusive na segunda ocorrência da hora repetida. Isso segue a regra pedida da candidata mais recente. Um `rc_m` futuro pode ganhar a seleção e suprimir o `.rc` anterior. Com piso futuro a inferência dá `None`. No mesmo minuto, o prazo ainda não amadureceu. Esses três comportamentos são distintos dos contraexemplos do achado 1. `pedido.md:8`, `recibo.md:44–47`, `bin/rc_sem_colher.py:94–102,115–122,133–140,148–162`.
- **Esmeril.** `^\+.*except` dá **0** em cada anexo de diff. O `except Exception` do leitor é contexto. O juiz lê o relógio uma vez, no `main`, e não há derivação paralela das contas no leitor ou na espera. Dentro do juiz, as duas classificações ainda percorrem o parser comum e repetem a folga. Os casos antigos seguem lendo `date`. P4 nomeou os dois casos de resumo, ambos corrigidos. `recibo.md:37–38,51`, `vercodex.py:165–177` no anexo, `bin/rc_sem_colher.py:51–64,113–119,146–152,184–198`, `bin/tests/test_rc_sem_colher.sh:48–51,74–77,282–293`, `pedido.md:9,12`.

Conferi o código da cópia, os patches fornecidos, as sondas em memória e a sintaxe Bash. RED/GREEN integrais, execução de `test_disjuntor.sh`, Ruff, confirmação do hash por Git, instalação do vercodex e ocorrência dos cenários em produção ficam [nao verificado].

## 27/11 11:45 · sessao 9b2b58bedbd6 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

**(a) O zero veio de `periodos_ft` → `ft_sem_escala`. O motivo que a sonda encontrou foi `LIMBO`.** O fechamento testa `escala_certa_no_dia` antes de consultar `feriado_vira_folga`. A linha lavrada tem `folga_sem_escala_min=437,10`, que corresponde às `7,28502 h` trabalhadas na precisão do campo. Esse ramo grava as trabalhadas e preserva o noturno, sem acrescentar HE 100% ou folga trabalhada. A sonda leu `LIMBO` no cadastro da sombra. A lavra prova a reprovação no filtro, mas o motivo histórico exato é [nao verificado]. [fechamento.py:953](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:953) · [fechamento.py:1004](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:1004) · [escala_certa.py:31](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/services/escala_certa.py:31) · [dia_pago.py:259](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/dia_pago.py:259)

- `fd=1` carimba a dobra habilitada. A sonda obteve `feriado_vira_folga=True` para o vínculo 1552, `6x1`, `trabalha_em_feriado=False`. Os períodos barrados pelo filtro ficam fora da promoção. A janela de HE estava ativa e a consulta de decisões vigentes voltou vazia. Para um par em folga sem escala certa, a janela retorna antes do bloqueio integral, que zeraria também trabalhadas e noturnas. [regua_cct.py:421](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/regua_cct.py:421) · [models.py:1400](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1400) · [fechamento.py:958](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:958) · [motor_calculo_v2.py:1562](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:1562)
- Com a célula 143963 em `trabalha=False`, 20/11 fica fora de `datas_previstas_trabalho` na reconstrução. O conjunto é fornecido explicitamente, então `_previstas_fornecidas=True`. A dobra direta consulta o ciclo e o feriado no dia de início do turno. [models.py:1098](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1098) · [fechamento.py:749](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:749) · [motor_calculo_v2.py:826](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:826) · [motor_calculo_v2.py:941](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:941)

**(b) Sim, coincidem em 20/11 na reconstrução.** Os dois conjuntos impressos no recorte de 19 a 21/11 contêm apenas `{20/11/2026}`. O fechamento injeta `feriados_do_colab` e o calendário chama a mesma função. Para esse dia, o alcance usa o município 4103 do posto 460 do vínculo 1552. A referência à praça na docstring do motor ficou desatualizada. O método executável lê o conjunto injetado. [fechamento.py:668](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:668) · [fechamento.py:756](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:756) · [calendario.py:347](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/colaboradores/services/calendario.py:347) · [feriados.py:124](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:124) · [feriados.py:160](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:160) · [motor_calculo_v2.py:1000](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:1000)

**(c) Há um juiz para o alcance do feriado.** O registro aponta `core.feriados.eh_feriado`, que delega a `feriados_de`. `feriados_do_colab` resolve o município de cada dia e usa esse mesmo filtro. Nenhum dos dois sai. `escala_certa_no_dia` julga a confiabilidade da escala. O ponto que produz a divergência de pagamento é aplicar essa guarda à promoção do feriado, caso já registrado em `FERIADO-SO-NA-ESCALA-CERTA`. [juizes.py:196](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/juizes.py:196) · [feriados.py:101](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:101) · [feriados.py:135](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/feriados.py:135) · [escala_certa.py:49](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/services/escala_certa.py:49) · [BACKLOG.md:2164](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/BACKLOG.md:2164)

**(d) Sim, pela regra escrita de enquadramento, o trabalho de 20/11 cabe em HE 100% de feriado.** O BO-B (3) manda usar essa rubrica para feriado trabalhado em vínculo que folga no feriado. O cadastro fornecido satisfaz o predicado com `trabalha_em_feriado=False`, ciclo `6x1` e feriado no município do posto. O corte de 27/09 às 03h revogou a cláusula de feriado da L-086 e manteve a dobra pelo ciclo e pelo dia de início. `CICLOS_FERIADO_SIMPLES` contém só `12x36`. A régua CCT pode ligar a dobra desse ciclo pela cláusula cadastrada `feriado_12x36_em_dobra`. Para o `6x1`, vale a dobra pelo ciclo. O corte das 05h sobre escala certa trata do adicional de folga trabalhada. O BO-B (3) reserva essa rubrica aos dias de folga. [PROMPTS.md:1370](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/PROMPTS.md:1370) · [models.py:1384](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/escala/models.py:1384) · [LEIS.md:118](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/docs/LEIS.md:118) · [motor_calculo_v2.py:944](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:944) · [motor_calculo_v2.py:3740](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:3740) · [regua_cct.py:473](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/core/regua_cct.py:473) · [fechamento.py:991](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:991)

**(e) Ficam [nao verificado]** o conjunto integral efetivamente injetado na lavra de 25/11, a identidade entre o cadastro da sombra e o usado nessa lavra, a classe do motor, o pareamento e os valores intermediários por período, a fonte exata da régua CCT e o que saiu no mensal, no TXT ou no pagamento. O mapa impresso associa IDs de batidas a dias. A trilha por período e o carimbo da régua são produzidos em pontos próprios do cálculo. O impacto nas empresas 2, 3 e 4 também fica [nao verificado]. [turnos.py:2971](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/turnos.py:2971) · [motor_calculo_v2.py:2571](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/motor_calculo_v2.py:2571) · [fechamento.py:298](/home/ronald/codex_ws/59600163ccc9cffb8f3ae148beede7cd7866ae86/app/ponto/services/fechamento.py:298)

## 27/11 11:44 · sessao ed0d5daab456 · FERIADO-NAO-PREVISTO-L086 · sonda · origem=Code

# FERIADO-NAO-PREVISTO-L086 — medida na sombra

## Totais por empresa

O universo da competência **11/2026**, de **21/10 a 20/11**, reúne os dias em que `feriados_do_colab` inclui a data e a célula tem `trabalha=False`. Os totais de horas cobrem os registros diários `DiaPago` da versão `motor` com `horas_trabalhadas > 0`. `r.md:28–36` `r.md:41–89` `app/core/feriados.py:135–167`

Nas referências, `saida-sonda.tsv` é a saída raspada da mensagem, numerada a partir de `sonda_leitura` na linha 1. As colunas de noturnas são somas dos valores decimais impressos nos detalhes. O resumo da sonda imprime os totais de trabalhadas e de `extras_100`. `saida-sonda.tsv:1–91` `r.md:126–143`

| Empresa | Dias-colab no universo | Com trabalhadas > 0 | Com lavra e trabalhadas ≤ 0 | Sem lavra | Horas trabalhadas | Horas noturnas | Horas `extras_100` | Horas `extras_100_noturna` | Fonte |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---|
| 2 | 753 | 69 | 72 | 612 | 505.6704230627778 | 21.15548214285714177 | 0.0 | 0.0 | `saida-sonda.tsv:5–73,88` |
| 3 | 182 | 13 | 12 | 157 | 111.14582922305556 | 29.699891428571429 | 0.0 | 0.0 | `saida-sonda.tsv:74–86,89` |
| 4 | 39 | 0 | 9 | 30 | 0.0 | 0.0 | 0.0 | 0.0 | `saida-sonda.tsv:87–90` |

O universo total impresso é **974 dias-colab**. As **82 linhas** detalhadas têm `extras_100 = 0.0` e `extras_100_noturna = 0.0`. `saida-sonda.tsv:5–86,91`

## Colab 1036 em 20/11

**Sim, aparece na empresa 2.** A linha traz **7.28502 h trabalhadas**, **1.163055238095238 h noturnas**, **0.0 h de `extras_100`** e **0.0 h de `extras_100_noturna`**. Os valores conferem com **7,29 / 1,16 / 0,0** do aval, com as duas primeiras medidas arredondadas a duas casas. `saida-sonda.tsv:72` `aval.txt:1`

## Horário e limites da medida

- **`medido_em = 2026-11-27T11:40:09-03:00`**. A sonda registra esse horário antes de abrir a transação de leitura. A rotina da cópia aponta para o backup diário das **04:00**, então **27/11 às 04:00** é uma hipótese se a rotina seguiu o fluxo diário. O código também prevê `--dump-agora` e atualização incremental. A data e a hora efetivas da base lida estão **[nao verificado]**. `saida-sonda.tsv:2` `r.md:113–119` `app/config/crons.py:182` `bin/sombra.sh:194–215,236–253,299–305`
- **HE100 no restante do universo [nao verificado].** A saída só conta os dias com lavra e `horas_trabalhadas <= 0` e os dias sem lavra. O código pode lançar horas diretamente em HE100 de feriado sem somá-las a `horas_trabalhadas`, deixando o dia fora dos detalhes. O campo específico `horas_extras_100_feriado` também ficou fora da consulta. `saida-sonda.tsv:87–90` `r.md:66–98` `app/ponto/models.py:326–332` `app/ponto/services/dia_pago.py:241–253`
- **Causa do zero de 1036 [nao verificado].** A saída traz a lavra diária. O conjunto de feriados recebido na apuração original, `datas_previstas_trabalho`, as batidas, a régua aplicada e o resultado do crivo de escala certa para esse dia ficam fora da leitura. `r.md:55–76` `app/ponto/services/fechamento.py:953–974` `aval.txt:1`
- **Valores atuais em produção, resultado de recálculo, TXT e pagamento efetivo [nao verificado].** A sonda consulta o `DiaPago` gravado na sombra. A geração do TXT na cópia usa os objetos do fechamento mensal. `r.md:66–76,103–124` `app/folha/views.py:214` `app/folha/export.py:420`

## Lista completa com `horas_trabalhadas > 0`

A lista está ordenada por empresa, data e ID do colaborador. O negrito marca `extras_100 = 0.0`. A empresa 4 tem zero linhas nesse recorte. `saida-sonda.tsv:5–90`

| Empresa | Data | Colab ID | Trabalhadas (h) | Noturnas (h) | `extras_100` (h) | Fonte |
|---:|---|---:|---:|---:|---:|---|
| 2 | 2026-11-02 | 43 | 4.088383333333334 | 0.0 | **0.0** | `saida-sonda.tsv:5` |
| 2 | 2026-11-02 | 203 | 7.069715555555556 | 0.0 | **0.0** | `saida-sonda.tsv:6` |
| 2 | 2026-11-02 | 207 | 5.991126388888889 | 0.0 | **0.0** | `saida-sonda.tsv:10` |
| 2 | 2026-11-02 | 278 | 7.0710894444444445 | 0.0 | **0.0** | `saida-sonda.tsv:13` |
| 2 | 2026-11-02 | 365 | 12.0 | 0.0 | **0.0** | `saida-sonda.tsv:16` |
| 2 | 2026-11-02 | 366 | 10.998848333333335 | 5.997973333333333 | **0.0** | `saida-sonda.tsv:19` |
| 2 | 2026-11-02 | 369 | 7.0080025 | 0.0 | **0.0** | `saida-sonda.tsv:20` |
| 2 | 2026-11-02 | 384 | 7.31101361111111 | 0.0 | **0.0** | `saida-sonda.tsv:22` |
| 2 | 2026-11-02 | 583 | 8.078643055555556 | 0.0 | **0.0** | `saida-sonda.tsv:26` |
| 2 | 2026-11-02 | 622 | 6.974738382777777 | 0.0 | **0.0** | `saida-sonda.tsv:27` |
| 2 | 2026-11-02 | 784 | 4.133950277777777 | 0.0 | **0.0** | `saida-sonda.tsv:32` |
| 2 | 2026-11-02 | 788 | 16.0160475 | 1.1082016666666665 | **0.0** | `saida-sonda.tsv:34` |
| 2 | 2026-11-02 | 789 | 7.3611770241666665 | 0.0 | **0.0** | `saida-sonda.tsv:35` |
| 2 | 2026-11-02 | 822 | 7.177437222222222 | 0.9663374603174603 | **0.0** | `saida-sonda.tsv:38` |
| 2 | 2026-11-02 | 830 | 12.066969811944444 | 0.0 | **0.0** | `saida-sonda.tsv:41` |
| 2 | 2026-11-02 | 961 | 7.146785555555555 | 0.0 | **0.0** | `saida-sonda.tsv:51` |
| 2 | 2026-11-02 | 962 | 7.5270697222222225 | 0.0 | **0.0** | `saida-sonda.tsv:53` |
| 2 | 2026-11-02 | 967 | 7.394691666666666 | 0.0 | **0.0** | `saida-sonda.tsv:55` |
| 2 | 2026-11-02 | 968 | 6.9958141666666664 | 0.9541666666666667 | **0.0** | `saida-sonda.tsv:56` |
| 2 | 2026-11-02 | 975 | 3.9582752777777777 | 0.0 | **0.0** | `saida-sonda.tsv:58` |
| 2 | 2026-11-02 | 988 | 7.342193096111111 | 0.0 | **0.0** | `saida-sonda.tsv:59` |
| 2 | 2026-11-02 | 990 | 7.317387678888889 | 0.0 | **0.0** | `saida-sonda.tsv:61` |
| 2 | 2026-11-02 | 991 | 7.042927222222222 | 0.0 | **0.0** | `saida-sonda.tsv:62` |
| 2 | 2026-11-15 | 203 | 7.1011894444444446 | 0.0 | **0.0** | `saida-sonda.tsv:7` |
| 2 | 2026-11-15 | 207 | 6.187036434444445 | 0.0 | **0.0** | `saida-sonda.tsv:11` |
| 2 | 2026-11-15 | 278 | 7.403098888888889 | 0.0 | **0.0** | `saida-sonda.tsv:14` |
| 2 | 2026-11-15 | 365 | 11.036849166666666 | 0.0 | **0.0** | `saida-sonda.tsv:17` |
| 2 | 2026-11-15 | 369 | 6.9213000000000005 | 0.0 | **0.0** | `saida-sonda.tsv:21` |
| 2 | 2026-11-15 | 384 | 6.144064166666666 | 0.0 | **0.0** | `saida-sonda.tsv:23` |
| 2 | 2026-11-15 | 622 | 4.0098577777777775 | 0.0 | **0.0** | `saida-sonda.tsv:28` |
| 2 | 2026-11-15 | 735 | 3.9637504116666666 | 0.0 | **0.0** | `saida-sonda.tsv:30` |
| 2 | 2026-11-15 | 789 | 7.428601666666666 | 0.0 | **0.0** | `saida-sonda.tsv:36` |
| 2 | 2026-11-15 | 822 | 8.112541666666667 | 1.065664761904762 | **0.0** | `saida-sonda.tsv:39` |
| 2 | 2026-11-15 | 830 | 11.885994444444444 | 0.0 | **0.0** | `saida-sonda.tsv:42` |
| 2 | 2026-11-15 | 899 | 8.89184510277778 | 0.0 | **0.0** | `saida-sonda.tsv:44` |
| 2 | 2026-11-15 | 903 | 4.243828611111111 | 0.0 | **0.0** | `saida-sonda.tsv:46` |
| 2 | 2026-11-15 | 944 | 6.867059856944445 | 0.0 | **0.0** | `saida-sonda.tsv:49` |
| 2 | 2026-11-15 | 968 | 7.0078652777777775 | 0.9576526984126983 | **0.0** | `saida-sonda.tsv:57` |
| 2 | 2026-11-15 | 991 | 7.026201944444444 | 0.0 | **0.0** | `saida-sonda.tsv:63` |
| 2 | 2026-11-15 | 1000 | 3.9718216666666666 | 0.0 | **0.0** | `saida-sonda.tsv:65` |
| 2 | 2026-11-15 | 1023 | 8.376993055555555 | 0.0 | **0.0** | `saida-sonda.tsv:67` |
| 2 | 2026-11-15 | 1026 | 7.02381 | 0.956779365079365 | **0.0** | `saida-sonda.tsv:68` |
| 2 | 2026-11-20 | 203 | 7.032373333333334 | 0.0 | **0.0** | `saida-sonda.tsv:8` |
| 2 | 2026-11-20 | 206 | 4.199098888888889 | 0.0 | **0.0** | `saida-sonda.tsv:9` |
| 2 | 2026-11-20 | 207 | 6.012959166666666 | 0.0 | **0.0** | `saida-sonda.tsv:12` |
| 2 | 2026-11-20 | 278 | 7.165294444444444 | 0.0 | **0.0** | `saida-sonda.tsv:15` |
| 2 | 2026-11-20 | 365 | 11.008139166666666 | 0.0 | **0.0** | `saida-sonda.tsv:18` |
| 2 | 2026-11-20 | 384 | 7.568888055555556 | 0.0 | **0.0** | `saida-sonda.tsv:24` |
| 2 | 2026-11-20 | 415 | 6.995728888888889 | 0.0 | **0.0** | `saida-sonda.tsv:25` |
| 2 | 2026-11-20 | 727 | 6.987754444444445 | 0.0 | **0.0** | `saida-sonda.tsv:29` |
| 2 | 2026-11-20 | 735 | 4.312736620277778 | 0.0 | **0.0** | `saida-sonda.tsv:31` |
| 2 | 2026-11-20 | 784 | 3.956977222222222 | 0.0 | **0.0** | `saida-sonda.tsv:33` |
| 2 | 2026-11-20 | 789 | 3.544456188888889 | 0.0 | **0.0** | `saida-sonda.tsv:37` |
| 2 | 2026-11-20 | 822 | 7.05717611111111 | 0.9719514285714285 | **0.0** | `saida-sonda.tsv:40` |
| 2 | 2026-11-20 | 830 | 12.009573333333332 | 0.0 | **0.0** | `saida-sonda.tsv:43` |
| 2 | 2026-11-20 | 899 | 7.019682479444445 | 0.0 | **0.0** | `saida-sonda.tsv:45` |
| 2 | 2026-11-20 | 903 | 8.047641666666667 | 0.0 | **0.0** | `saida-sonda.tsv:47` |
| 2 | 2026-11-20 | 924 | 3.115772222222222 | 0.0 | **0.0** | `saida-sonda.tsv:48` |
| 2 | 2026-11-20 | 944 | 6.701955833333332 | 0.0 | **0.0** | `saida-sonda.tsv:50` |
| 2 | 2026-11-20 | 961 | 7.668902500000001 | 0.0 | **0.0** | `saida-sonda.tsv:52` |
| 2 | 2026-11-20 | 962 | 7.2847275 | 0.0 | **0.0** | `saida-sonda.tsv:54` |
| 2 | 2026-11-20 | 988 | 10.377472973611113 | 0.0 | **0.0** | `saida-sonda.tsv:60` |
| 2 | 2026-11-20 | 991 | 7.0027872222222225 | 0.0 | **0.0** | `saida-sonda.tsv:64` |
| 2 | 2026-11-20 | 1000 | 3.9380341666666667 | 0.0 | **0.0** | `saida-sonda.tsv:66` |
| 2 | 2026-11-20 | 1026 | 7.228945 | 0.9553495238095238 | **0.0** | `saida-sonda.tsv:69` |
| 2 | 2026-11-20 | 1031 | 10.85031111111111 | 6.014245833333333 | **0.0** | `saida-sonda.tsv:70` |
| 2 | 2026-11-20 | 1035 | 11.299347000833333 | 0.04410416666666667 | **0.0** | `saida-sonda.tsv:71` |
| 2 | 2026-11-20 | 1036 | 7.28502 | 1.163055238095238 | **0.0** | `saida-sonda.tsv:72` |
| 2 | 2026-11-20 | 1050 | 8.322671111111113 | 0.0 | **0.0** | `saida-sonda.tsv:73` |
| 3 | 2026-11-02 | 85 | 11.897632222222223 | 10.16872253968254 | **0.0** | `saida-sonda.tsv:76` |
| 3 | 2026-11-02 | 114 | 7.004955277777778 | 0.0 | **0.0** | `saida-sonda.tsv:78` |
| 3 | 2026-11-02 | 146 | 13.50255722222222 | 9.765453015873016 | **0.0** | `saida-sonda.tsv:81` |
| 3 | 2026-11-02 | 749 | 7.395433621944446 | 0.0 | **0.0** | `saida-sonda.tsv:82` |
| 3 | 2026-11-15 | 82 | 7.559408888888888 | 0.0 | **0.0** | `saida-sonda.tsv:74` |
| 3 | 2026-11-15 | 114 | 8.323086386944444 | 0.0 | **0.0** | `saida-sonda.tsv:79` |
| 3 | 2026-11-15 | 911 | 6.466329444444445 | 0.0 | **0.0** | `saida-sonda.tsv:83` |
| 3 | 2026-11-15 | 1024 | 7.3869547580555555 | 0.0 | **0.0** | `saida-sonda.tsv:85` |
| 3 | 2026-11-20 | 82 | 7.109137536666666 | 0.0 | **0.0** | `saida-sonda.tsv:75` |
| 3 | 2026-11-20 | 85 | 11.545001388888888 | 9.765715873015873 | **0.0** | `saida-sonda.tsv:77` |
| 3 | 2026-11-20 | 114 | 7.974111795277778 | 0.0 | **0.0** | `saida-sonda.tsv:80` |
| 3 | 2026-11-20 | 911 | 7.600584166666666 | 0.0 | **0.0** | `saida-sonda.tsv:84` |
| 3 | 2026-11-20 | 1024 | 7.3806365130555545 | 0.0 | **0.0** | `saida-sonda.tsv:86` |

## 27/11 11:41 · sessao 004a35688bb5 · FERIADO-NAO-PREVISTO-L086 · leitura · origem=Code

## (a) De onde vêm os feriados da competência

**Na cópia, o fechamento e o calendário chamam `feriados_do_colab`.** A chave territorial do conjunto é o **município do posto do vínculo de cada dia**. A referência à “praça do colab” em `MotorBase.eh_feriado` está na docstring. O corpo do método consulta o conjunto recebido. `app/ponto/services/fechamento.py:668`, `app/colaboradores/services/calendario.py:347`, `app/core/feriados.py:131`, `app/core/feriados.py:160`, `app/ponto/motor_calculo_v2.py:1003`, `app/ponto/motor_calculo_v2.py:1007`.

1. `recalcular_fechamento_mes` resolve a janela da competência pelo corte da **empresa**. Para os feriados, carrega um envelope que cobre os cortes 2 a 28, com um dia a mais em cada ponta. Para cada colaborador, chama `feriados_do_colab` com as linhas de feriado e as células carregadas em lote. `app/ponto/services/fechamento.py:468`, `app/ponto/services/fechamento.py:604`, `app/ponto/services/fechamento.py:615`, `app/ponto/services/fechamento.py:625`, `app/ponto/services/fechamento.py:641`, `app/ponto/services/fechamento.py:668`.
2. `feriados_do_colab` resolve `vinculo_do_dia` → `lugar_do_vinculo` → `vinculo.posto.municipio` → `feriados_de`, com cache por `Municipio.pk`. `Posto.praca` e `Posto.municipio` são FKs distintas. A praça entra na resolução da **CCT**, pelo vínculo na data de referência da régua, passada pelo fechamento como início da fatia. `app/core/feriados.py:124`, `app/core/feriados.py:160`, `app/core/feriados.py:164`, `app/colaboradores/models.py:230`, `app/colaboradores/models.py:231`, `app/ponto/services/fechamento.py:756`, `app/core/regua_cct.py:329`, `app/core/regua_cct.py:330`, `app/core/regua_cct.py:458`.
3. `apurar_por_fatia` chama `_montar_motor` e depois `motor.calcular_mes`. `_montar_motor` passa `feriados=` a `get_motor_cct`, que o repassa a `get_motor`, que o entrega ao construtor. `MotorBase` copia o conjunto para `self.feriados` e guarda o mesmo objeto em `_feriados_injetados`. Data presente retorna `True`. Data ausente com esse objeto injetado retorna `False` na linha 1010, inclusive se o conjunto for vazio. O `MotorIntermitente` substitui ambos por um conjunto vazio. `app/ponto/services/fechamento.py:298`, `app/ponto/services/fechamento.py:306`, `app/ponto/services/fechamento.py:755`, `app/ponto/services/fechamento.py:758`, `app/core/regua_cct.py:457`, `app/core/regua_cct.py:479`, `app/ponto/motor_calculo_v2.py:815`, `app/ponto/motor_calculo_v2.py:822`, `app/ponto/motor_calculo_v2.py:1007`, `app/ponto/motor_calculo_v2.py:1011`, `app/ponto/motor_calculo_v2.py:3698`, `app/ponto/motor_calculo_v2.py:3815`.
4. O resultado alimenta `FechamentoMensal` e a lavra diária `DiaPago(versao='motor')`. A geração do TXT chama a **mesma apuração** em `somente_leitura=True, retornar_objeto=True`, passa os objetos mensais a `montar_linhas_export` e lê deles as HE 100% para as rubricas. O calendário lê a linha diária já gravada. `app/ponto/services/fechamento.py:942`, `app/ponto/services/fechamento.py:1168`, `app/ponto/services/fechamento.py:1184`, `app/ponto/services/fechamento.py:1225`, `app/ponto/services/fechamento.py:1251`, `app/folha/views.py:215`, `app/folha/views.py:275`, `app/folha/views.py:288`, `app/folha/export.py:420`, `app/folha/export.py:739`, `app/colaboradores/services/calendario.py:319`, `app/colaboradores/services/calendario.py:688`.

## (b) O universo dos dois conjuntos

`feriados_do_colab` usa a geradora da célula se o vínculo for encontrado e a data estiver dentro da sua vigência. Se a célula nomeia uma geradora ausente ou fora das datas, o juiz devolve `None`. Sem geradora, escolhe entre os vínculos que cobrem a data, priorizando ativo e depois início mais recente. O município vem do posto desse vínculo. Sem município, só o nacional alcança o dia. `app/escala/alimentacao.py:214`, `app/escala/alimentacao.py:269`, `app/escala/alimentacao.py:279`, `app/escala/alimentacao.py:286`, `app/core/feriados.py:57`, `app/core/feriados.py:131`, `app/core/feriados.py:161`.

`feriados_de` aplica **um município fixo** à janela. O nacional vale para todos, o estadual exige a UF e o municipal exige UF e município normalizado. `feriados_do_colab` retém, para cada data, a resposta desse mesmo filtro no município daquele dia. Se o município permanece o mesmo, com a mesma janela e as mesmas linhas, os conjuntos coincidem. Numa troca de município, o conjunto fixo pode incluir datas do posto antigo após a troca e omitir datas do posto novo. Com `carregados=`, `feriados_de` confia no recorte entregue, enquanto `feriados_do_colab` só adiciona dias que percorre na janela pedida. `app/core/feriados.py:27`, `app/core/feriados.py:32`, `app/core/feriados.py:57`, `app/core/feriados.py:81`, `app/core/feriados.py:150`, `app/core/feriados.py:160`.

## (c) O que chega em `datas_previstas_trabalho`

O fechamento inicia um `set()` por colaborador. Dentro de cada fatia, `_montar_motor` só adiciona a data se `esc.eh_dia_trabalho(data)` for verdadeiro e ela estiver fora das datas suprimidas. `eh_dia_trabalho` retorna `cel.trabalha` quando há célula. Logo, **com a célula fornecida, 20/11 fica fora do conjunto e `_previstas_fornecidas=True`**, pois o construtor recebeu um conjunto, mesmo se vazio. O motor de dias órfãos também recebe um conjunto vazio explícito. A origem e o DNA concretos da célula do 1036 são [nao verificado]. `app/ponto/services/fechamento.py:726`, `app/ponto/services/fechamento.py:749`, `app/ponto/services/fechamento.py:760`, `app/ponto/services/fechamento.py:838`, `app/escala/models.py:1098`, `app/escala/models.py:1106`, `app/ponto/motor_calculo_v2.py:826`.

## (d) Onde o 100% pode ficar em zero

**O ramo que produziu o zero do 1036 é [nao verificado].** Os 7,29 h trabalhados e 1,16 h noturnos vêm do dado raspado. A lavra pode registrar ambos com HE 100% zero, inclusive quando o período vai para `ft_sem_escala`. Nesse caminho, a condição decisiva é `escala_certa_no_dia(...) == False` no filtro da linha **959**. O dado fornecido ainda permite outros caminhos. `app/ponto/services/fechamento.py:958`, `app/ponto/services/fechamento.py:1005`, `app/ponto/services/dia_pago.py:223`, `app/ponto/services/dia_pago.py:259`.

| Ordem da dobra direta | Valor no caso |
|---|---|
| 1. O cálculo usa o dia de início do turno. Nos cálculos por par, usa o mapa do juiz ou a data local da entrada crua quando falta a chave. Nos recálculos por dia, exige períodos no grupo. | Há batidas no recorte informado. O dia atribuído a cada par e a classe do motor são [nao verificado]. `app/ponto/motor_calculo_v2.py:976`, `app/ponto/motor_calculo_v2.py:3197`, `app/ponto/motor_calculo_v2.py:3442`, `app/ponto/motor_calculo_v2.py:3461`, `app/docs/LEIS.md:117`. |
| 2. O chamador exige `duracao > 0` ou `trab_dia > 0`. | A lavra mostra trabalhadas positivas. O valor na guarda do ramo que rodou é [nao verificado]. Os quatro chamadores são 12x36 por par, Turno Partido por dia, Comercial por par e Comercial por dia. `app/ponto/motor_calculo_v2.py:2925`, `app/ponto/motor_calculo_v2.py:3208`, `app/ponto/motor_calculo_v2.py:3385`, `app/ponto/motor_calculo_v2.py:3467`. |
| 3. `bool(self.feriado_em_dobra)`. Se falso, a expressão da linha 961 encerra a decisão. | [nao verificado]. O padrão vem do ciclo, com `12x36` no conjunto de feriado simples. A CCT ativa pode ligar a dobra para esse ciclo. `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:3740`, `app/ponto/motor_calculo_v2.py:3810`, `app/core/regua_cct.py:440`, `app/core/regua_cct.py:477`. |
| 4. `self.eh_feriado(dia)`. Data presente dá `True`. Data ausente no conjunto injetado dá `False` na linha 1010. | Para **20/11**, com os mesmos insumos do calendário e o conjunto preservado, dá `True` na linha 1008. O conjunto efetivo da lavra histórica e o dia consultado são [nao verificado]. O intermitente injeta vazio e retorna `False`. `app/ponto/services/fechamento.py:668`, `app/colaboradores/services/calendario.py:347`, `app/ponto/motor_calculo_v2.py:1007`, `app/ponto/motor_calculo_v2.py:1010`, `app/ponto/motor_calculo_v2.py:3698`. |

Se a dobra direta passa, os chamadores atribuem o trabalhado a `minutos_extra_100` e a `minutos_extra_100_feriado`. Para `resultado.periodos`, a lavra soma esses minutos divididos por 60 em `DiaPago.horas_extras_100` e no subconjunto de feriado. O leitor expõe `horas_extras_100` como `extras_100`. **A previsão fica fora da expressão da dobra**. A consulta a `datas_previstas_trabalho` na linha 996 pertence à folga semanal. `app/ponto/motor_calculo_v2.py:961`, `app/ponto/motor_calculo_v2.py:996`, `app/ponto/motor_calculo_v2.py:2927`, `app/ponto/motor_calculo_v2.py:3217`, `app/ponto/motor_calculo_v2.py:3387`, `app/ponto/motor_calculo_v2.py:3475`, `app/ponto/services/dia_pago.py:182`, `app/ponto/services/dia_pago.py:915`.

Para o caso com célula `False`, a sequência que pode mudar a rubrica depois da dobra direta é esta. `app/ponto/motor_calculo_v2.py:2558`, `app/ponto/services/fechamento.py:953`.

1. Com vínculo não intermitente e `data_jornada=20/11`, o `False` da célula faz `folga_do_vinculo=True` e o período vai para `periodos_ft`. Se foi esse o percurso do 1036, [nao verificado]. Antes da separação, a janela de HE pode zerar um par inteiro se houver folga ou feriado, janela de empresa vigente, previsto zero e ausência de autorização, com a exceção da folga sem escala certa. Um par zerado ali também perde seus minutos trabalhados e noturnos. Esses insumos por par são [nao verificado]. `app/ponto/motor_calculo_v2.py:1554`, `app/ponto/motor_calculo_v2.py:1563`, `app/ponto/motor_calculo_v2.py:1583`, `app/ponto/motor_calculo_v2.py:1587`, `app/ponto/motor_calculo_v2.py:2560`, `app/ponto/motor_calculo_v2.py:2574`, `app/ponto/motor_calculo_v2.py:2605`.
2. Em `periodos_ft`, **primeiro** passa pela guarda de escala certa. `LIMBO`, `A2` ou `A8` barram o período na linha 959. A lavra de `ft_sem_escala` conserva trabalhadas e noturnas e deixa o 100% desse período em zero. O motivo e a presença de `folga_sem_escala_min` na linha do 1036 são [nao verificado]. `app/ponto/services/fechamento.py:958`, `app/ponto/services/fechamento.py:1005`, `app/escala/services/escala_certa.py:31`, `app/escala/services/escala_certa.py:42`, `app/escala/services/escala_certa.py:51`, `app/ponto/services/dia_pago.py:259`.
3. **Depois**, se há vínculo e `feriado_vira_folga` dá `True`, o fechamento promove o período a HE 100% de feriado. O predicado exige `trabalha_em_feriado=False`, ciclo da lista `6x1`/`5x2`/`semanal` e feriado no município do posto. Se a promoção falha após passar pela escala certa, o período fica na rubrica de folga trabalhada. Vínculo, ciclo, chave e rubrica efetiva no caso são [nao verificado]. `app/ponto/services/fechamento.py:970`, `app/ponto/services/fechamento.py:979`, `app/escala/models.py:32`, `app/escala/models.py:1400`, `app/ponto/services/dia_pago.py:234`, `app/ponto/services/dia_pago.py:244`.

## (e) Autoridade e censo em `app/`

**São duas interfaces da mesma regra de alcance, com entradas diferentes.** `feriados_de` responde por município fixo. `feriados_do_colab` resolve o município da pessoa no dia e chama `feriados_de`. `JUIZES.md` e o registro vivo nomeiam `core.feriados.eh_feriado` para a existência do feriado. O próprio `eh_feriado` delega a `feriados_de`. A dobra é outra pergunta, registrada em `CICLOS_FERIADO_SIMPLES`. `app/core/feriados.py:70`, `app/core/feriados.py:101`, `app/core/feriados.py:135`, `app/core/feriados.py:164`, `app/docs/JUIZES.md:115`, `app/docs/JUIZES.md:130`, `app/core/juizes.py:196`, `app/core/juizes.py:320`.

| Função | Chamadas diretas na cópia |
|---|---|
| `feriados_de` | `eh_feriado` e `feriados_do_colab` em `app/core/feriados.py:105` e `app/core/feriados.py:164`. Fila de chamados em `app/chamados/services/fila_por_causa.py:134`. Dias úteis de VT/VA em `app/colaboradores/models.py:1003`. Consulta e presença por posto em `app/colaboradores/services/feriado_do_posto.py:57` e `app/colaboradores/services/feriado_do_posto.py:112`. |
| `feriados_do_colab` | Fechamento em `app/ponto/services/fechamento.py:668`, calendário em `app/colaboradores/services/calendario.py:347`, precedência em `app/ponto/precedencia.py:331`, espelho em `app/ponto/services/espelho.py:1065`, PDF em `app/relatorios/pdf_espelho.py:298`. Comandos em `app/ponto/management/commands/desvio_o68b.py:113`, `app/ponto/management/commands/diff_espelho_alimentacao.py:93`, `app/relatorios/management/commands/diff_extrato_parcial.py:58`. Testes em `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:84`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:93`, `app/chamados/tests/test_jv1_volta4_posto_do_dia.py:105`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:120`, `app/ponto/tests/test_caracterizacao_espelho_unit.py:133`. |

**Nenhuma das duas funções deve sair sob a L-002.** A derivação paralela que a lei mandaria retirar seria uma escolha independente de praça fixa para responder pelo colaborador. Nessa rota, a chamada executável já usa o vínculo do dia. A própria L-086 registra que a cláusula original de feriado foi superada pela regra de ciclo e dia de início. A promoção de folga-feriado tem a guarda adicional de escala certa, registrada no BACKLOG como `FERIADO-SO-NA-ESCALA-CERTA`. `app/docs/LEIS.md:60`, `app/docs/LEIS.md:118`, `app/ponto/services/fechamento.py:668`, `app/ponto/motor_calculo_v2.py:944`, `app/docs/BACKLOG.md:2164`.

O BACKLOG registra o pouso de `FERIADO-POR-POSTO` sem P4 e a porta única **da marca**. Essa porta propaga `Posto.opera_em_feriado` para `vinculo.trabalha_em_feriado` nos vínculos elegíveis. O corte da O275 restringe “trabalha em feriado” às escalas semanais e remete plantões à precedência existente. Esses registros tratam da escala, enquanto o alcance geográfico segue em `core.feriados`. `app/docs/BACKLOG.md:1984`, `app/docs/CORTES.md:616`, `app/colaboradores/services/feriado_do_posto.py:179`, `app/colaboradores/services/feriado_do_posto.py:368`, `app/core/feriados.py:160`.

## (f) Sonda de leitura

Para distinguir os ramos, a sonda mostra o conjunto reconstruído com o cadastro atual, a chave de feriado do vínculo, o motivo do juiz de escala certa, a decisão vigente de HE e o `DiaPago` gravado. Na linha lavrada, `fd=` em `versao_motor` é o carimbo da régua e `folga_sem_escala_min` identifica os minutos que passaram pelo ramo sem escala certa. `folga_feriado_min` identifica a promoção e `o85_dia_sem_vinculo_min` identifica trabalhadas órfãs. A coincidência entre o cadastro atual e o da lavra histórica é [nao verificado]. `app/ponto/services/fechamento.py:304`, `app/core/regua_cct.py:421`, `app/ponto/services/dia_pago.py:257`, `app/ponto/services/dia_pago.py:266`, `app/ponto/services/dia_pago.py:269`, `app/ponto/services/dia_pago.py:342`, `app/ponto/models.py:1265`.

```python
# Envie o bloco ao manage.py shell no tenant do caso.
from datetime import date, timedelta

from django.db import connection, transaction

from colaboradores.models import Colaborador
from core.feriados import carregar, feriados_de, feriados_do_colab, lugar_do_vinculo
from escala.alimentacao import celulas_do_periodo, escalas_do_periodo, vinculo_do_dia
from escala.services.escala_certa import motivo_escala_errada
from ponto.janelas import janela_fechamento, periodo_apuracao
from ponto.models import DecisaoHE, DiaPago
from ponto.turnos import dia_das_batidas

cid, mes, ano = 1036, 11, 2026
a, d, b = (date(ano, mes, n) for n in (19, 20, 21))
um = timedelta(days=1)

with transaction.atomic():
    with connection.cursor() as cur:
        cur.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")

    col = Colaborador.objects.select_related("empresa").get(pk=cid)
    ini, fim = periodo_apuracao(mes, ano, 2)[0], periodo_apuracao(mes, ano, 28)[1]
    celulas = celulas_do_periodo([cid], ini, fim)
    vinculos = escalas_do_periodo(
        cid, ini - um, fim + um, mais=[c.escala_geradora_id for c in celulas.values()]
    )
    v = vinculo_do_dia(cid, d, vinculos, celulas=celulas)
    posto, mun = getattr(v, "posto", None), lugar_do_vinculo(v)
    fer = feriados_do_colab(
        col, ini - um, fim + um, celulas=celulas, vinculos=vinculos,
        carregados=carregar(ini - um, fim + um),
    )

    print("colab, empresa, janela", cid, col.empresa_id, janela_fechamento(mes, ano, col.empresa))
    print("motor e calendario (vinculo, posto, praca, municipio usado)",
          getattr(v, "pk", None), getattr(posto, "pk", None),
          getattr(posto, "praca_id", None), getattr(mun, "pk", None))
    print("feriados_de_municipio_fixo", sorted(feriados_de(mun, a, b)))
    print("feriados_do_colab_injecao", sorted(x for x in fer if a <= x <= b))

    cel = celulas.get((cid, d))
    print("celula", None if cel is None else (cel.pk, cel.trabalha, cel.origem, cel.dna))
    print("vinculo (tipo_id, base, ciclo, trabalha_em_feriado, feriado_vira_folga)",
          None if v is None else (v.tipo_escala_id, v.tipo_escala.tipo_base,
                                  v.tipo_escala.tipo_ciclo, v.trabalha_em_feriado,
                                  v.feriado_vira_folga(d)))
    print("motivo_escala_errada", motivo_escala_errada(cid, d))
    print("dia_por_id_batida", sorted(dia_das_batidas(col, a, b).items()))
    print("janela_he (ativa, desde)", getattr(col.empresa, "janela_he_ativa", None),
          getattr(col.empresa, "janela_he_desde", None))
    print("decisao_he_vigente", list(DecisaoHE.objects.vigentes().filter(
        colaborador_id=cid, data=d).values_list("pk", "data", "estado")))

    dp = DiaPago.objects.filter(
        colaborador_id=cid, mes=mes, ano=ano, data=d, tipo=DiaPago.TIPO_DIA, versao="motor"
    ).values(
        "pk", "data", "lavrado_em", "versao_motor", "horas_trabalhadas",
        "horas_noturnas", "horas_extras_100", "horas_extras_100_feriado",
        "horas_folga_trabalhada", "detalhe",
    ).first()
    if dp is not None:
        detalhe = dp.pop("detalhe") or {}
        for chave in ("folga_sem_escala_min", "folga_feriado_min", "o85_dia_sem_vinculo_min"):
            if chave in detalhe:
                dp[chave] = detalhe[chave]
    print("dia_lavrado", dp)
```

## 27/11 11:39 · sessao b4b559a914e3 · FERIADO-NAO-PREVISTO-L086 · leitura · origem=Code

## 1. Sonda de medida

A tela usa `core.feriados.feriados_do_colab` e cruza a data retornada com `CelulaDia.trabalha is False`. A função resolve o município pelo posto do vínculo que responde pelo dia. `app/colaboradores/services/calendario.py:346` `app/colaboradores/services/calendario.py:785` `app/core/feriados.py:124` `app/core/feriados.py:135` `app/core/feriados.py:161`

A fonte diária gravada é `DiaPago`, com `tipo='dia'` e `versao='motor'`. O fechamento grava essas linhas a partir da mesma apuração que alimenta `FechamentoMensal`. A geração do TXT nesta cópia monta as rubricas a partir dos objetos mensais que recebe de `recalcular_fechamento_mes`; a sonda atende ao recorte de **lavra gravada** lendo `DiaPago`, sem chamar essa rotina. `app/ponto/models.py:311` `app/ponto/services/fechamento.py:1219` `app/ponto/services/fechamento.py:1251` `app/folha/views.py:214` `app/folha/views.py:275` `app/folha/export.py:420`

O resumo distingue dia com lavra e `horas_trabalhadas <= 0` de dia sem linha lavrada. O filtro usa o campo literal `horas_trabalhadas > 0`. Assim, um dia cuja hora foi registrada integralmente em HE100 de feriado pode ficar fora dos detalhes e ainda contar no universo. `app/colaboradores/services/calendario.py:777` `app/ponto/models.py:325` `app/ponto/services/dia_pago.py:244`

Bloco para a entrada padrão de `python -B manage.py shell --settings=config.settings.sombra`. A configuração aponta para `sombra_db`, banco `sombra`, onde o schema do tenant é `juliani`. Execução e totais da sombra **[nao verificado]**. `app/config/settings/sombra.py:7` `app/config/settings/sombra.py:29` `app/config/settings/sombra.py:47`

```python
from collections import defaultdict
from datetime import date
from math import fsum

from django.conf import settings
from django.db import connection, transaction
from django.utils import timezone
from django_tenants.utils import schema_context

from colaboradores.models import Empresa
from core.feriados import carregar, feriados_do_colab
from escala.models import CelulaDia
from ponto.janelas import janela_fechamento
from ponto.models import DiaPago


MES, ANO = 11, 2026
EMPRESAS = (2, 3, 4)
JANELA_PEDIDA = (date(2026, 10, 21), date(2026, 11, 20))


def medir_empresa(empresa):
    inicio, fim = janela_fechamento(MES, ANO, empresa)
    if (inicio, fim) != JANELA_PEDIDA:
        raise RuntimeError(
            f"Empresa {empresa.pk}: janela encontrada {inicio} a {fim}"
        )

    # O mapa entregue a feriados_do_colab cobre a janela inteira.
    # app/core/feriados.py:135
    celulas = {
        (celula.colaborador_id, celula.data): celula
        for celula in CelulaDia.objects.filter(
            colaborador__empresa_id=empresa.pk,
            data__range=(inicio, fim),
        ).only("colaborador", "data", "trabalha", "escala_geradora")
    }
    folgas_por_colab = defaultdict(set)
    for (colab_id, dia), celula in celulas.items():
        if celula.trabalha is False:
            folgas_por_colab[colab_id].add(dia)

    feriados_cadastrados = carregar(inicio, fim)
    alvos = set()
    for colab_id, folgas in folgas_por_colab.items():
        feriados = feriados_do_colab(
            colab_id, inicio, fim,
            carregados=feriados_cadastrados,
            celulas=celulas,
        )
        alvos.update((colab_id, dia) for dia in folgas & feriados)

    # DiaPago.data ja e uma data. A chave do dia vem da lavra.
    # app/ponto/models.py:315 e app/ponto/services/dia_pago.py:109
    linhas = DiaPago.objects.filter(
        colaborador_id__in={colab_id for colab_id, _ in alvos},
        mes=MES,
        ano=ANO,
        tipo=DiaPago.TIPO_DIA,
        versao="motor",
        data__range=(inicio, fim),
    ).values_list(
        "colaborador_id", "data", "horas_trabalhadas", "horas_noturnas",
        "horas_extras_100", "horas_extras_100_noturna",
    )
    lavra = {(colab_id, dia): tuple(horas) for colab_id, dia, *horas in linhas}

    detalhes = []
    sem_lavra = 0
    com_lavra_sem_horas_positivas = 0
    for colab_id, dia in sorted(alvos):
        horas = lavra.get((colab_id, dia))
        if horas is None:
            sem_lavra += 1
        elif horas[0] > 0:
            detalhes.append((colab_id, empresa.pk, dia.isoformat(), *horas))
        else:
            com_lavra_sem_horas_positivas += 1

    resumo = (
        empresa.pk,
        len(alvos),
        len(detalhes),
        com_lavra_sem_horas_positivas,
        sem_lavra,
        fsum(linha[3] for linha in detalhes),
        fsum(linha[5] for linha in detalhes),
    )
    return detalhes, resumo


def sondar():
    if (
        not getattr(settings, "HASNER_SOMBRA", False)
        or connection.settings_dict["NAME"] != "sombra"
        or connection.settings_dict["HOST"] != "sombra_db"
    ):
        raise RuntimeError("Use a configuracao do banco sombra.")
    if connection.in_atomic_block or not connection.get_autocommit():
        raise RuntimeError("A sonda exige uma transacao externa propria.")

    medido_em = timezone.localtime(timezone.now())
    with schema_context("juliani"), transaction.atomic(durable=True):
        with connection.cursor() as cursor:
            cursor.execute("SET TRANSACTION ISOLATION LEVEL REPEATABLE READ, READ ONLY")
            cursor.execute("SELECT current_database(), current_schema()")
            if cursor.fetchone() != ("sombra", "juliani"):
                raise RuntimeError("Banco ou schema diferente do solicitado.")

        resultados = []
        for empresa_id in EMPRESAS:
            empresa = Empresa.objects.only("dia_inicio_competencia").get(pk=empresa_id)
            resultados.append(medir_empresa(empresa))

    print("medido_em", medido_em.isoformat(timespec="seconds"), sep="\t")
    print(
        "colab_id", "empresa_id", "data", "horas_trabalhadas", "horas_noturnas",
        "horas_extras_100", "horas_extras_100_noturna", sep="\t",
    )
    for detalhes, _ in resultados:
        for linha in detalhes:
            print(*linha, sep="\t")

    print(
        "empresa_id", "universo", "com_horas_positivas",
        "com_lavra_sem_horas_positivas", "sem_lavra",
        "total_horas_trabalhadas", "total_horas_extras_100", sep="\t",
    )
    resumos = [resumo for _, resumo in resultados]
    for resumo in resumos:
        print(*resumo, sep="\t")
    print("universo_total", sum(resumo[1] for resumo in resumos), sep="\t")


sondar()
```

## 2. Dono e regra da dobra

**O78** é o registro explícito da L-086, **fechado como selo**. **FERIADO-POR-POSTO** é a obra da marca cadastral por posto, registrada **no ar em 22/11 às 13:34**, com o P4 destacado como `P4-SYNC-FERIADO`. Para a fatia de dinheiro, o antecedente direto é **ESPELHO-FERIADO-DOBRA**, fechado, e o ramo de pagamento do **BO-B (3)** que alimenta o fechamento e o `DiaPago`. `app/docs/BACKLOG.md:1501` `app/docs/BACKLOG.md:1984` `app/docs/BACKLOG.md:1985` `app/docs/BACKLOG.md:2044` `app/docs/BACKLOG.md:1667` `app/ponto/services/fechamento.py:960`

O grep também localiza **FERIADO-DIA-CHAVE**, registrado para a atribuição do dia na virada da meia-noite, e **O275/O277**, respectivamente interrompida e registrada, com O277 dependente da O275 no registro. O aval anexo põe a cura de 1036 na fila de dinheiro de `SO-DOIS-ALIVIOS-v2`. O registro formal dessa fatia pelo identificador `FERIADO-NAO-PREVISTO-L086` nos três arquivos é **[nao verificado]**. `app/docs/BACKLOG.md:1668` `app/docs/GRADE-MESTRA.md:91` `app/docs/GRADE-MESTRA.md:233` `app/docs/GRADE-MESTRA.md:234` `app/docs/CORTES.md:616` `aval.txt:1`

**Quem decide no motor.** `MotorBase._feriado_dobra_no_dia(dia)` retorna `bool(self.feriado_em_dobra) and self.eh_feriado(dia)`. No caminho normal do fechamento, **sim, é a mesma fonte que a tela usa**. O fechamento chama `feriados_do_colab` e injeta o conjunto no motor, cobrindo também as bordas da janela de batidas. Sem injeção, `eh_feriado` recorre a `is_feriado(data)` sem município, que enxerga só o nacional. `MotorIntermitente` injeta um conjunto vazio próprio. `app/ponto/motor_calculo_v2.py:941` `app/ponto/motor_calculo_v2.py:1000` `app/ponto/motor_calculo_v2.py:30` `app/ponto/motor_calculo_v2.py:3694` `app/ponto/services/fechamento.py:611` `app/ponto/services/fechamento.py:668` `app/ponto/services/fechamento.py:754`

| Campo ou insumo | Quem escreve e quem lê |
|---|---|
| `feriados` e `_feriados_injetados` | `feriados_do_colab` compõe as datas. `MotorBase.__init__` grava os atributos em memória e `MotorBase.eh_feriado` os lê. `MotorIntermitente.__init__` grava o vazio nos dois. `app/core/feriados.py:159` `app/ponto/motor_calculo_v2.py:817` `app/ponto/motor_calculo_v2.py:1007` `app/ponto/motor_calculo_v2.py:3698` |
| `Feriado.data`, `abrangencia`, `uf`, `municipio` e lugar do vínculo | `form_feriado` cria e edita o cadastro, e o importador insere nacionais. `_qs`, `alcanca` e `feriados_de` leem as datas e o alcance. `executar_vinculo` grava o posto no vínculo. A porta de estrutura grava `Posto.municipio`, e `lugar_do_vinculo` lê esse município do vínculo escolhido por `vinculo_do_dia`. `app/core/views_config.py:64` `app/core/views_config.py:83` `app/core/views_config.py:144` `app/core/feriados.py:27` `app/core/feriados.py:57` `app/core/feriados.py:70` `app/colaboradores/services/vinculo.py:275` `app/colaboradores/services/vinculo.py:287` `app/colaboradores/services/estrutura.py:67` `app/colaboradores/services/estrutura.py:99` `app/escala/alimentacao.py:269` `app/core/feriados.py:124` |
| `TipoEscala.tipo_ciclo` e `feriado_em_dobra` | `salvar_tipo` grava o ciclo. `get_motor` lê o ciclo e resolve o parâmetro por `CICLOS_FERIADO_SIMPLES`, hoje `{'12x36'}`. `MotorBase.__init__` guarda o booleano, e `_feriado_dobra_no_dia` o lê. Com a régua CCT ativa e cláusula aplicável, `get_motor_cct` fornece `True` para 12x36. O formulário de sindicato grava `feriado_12x36_em_dobra` e `regua_para` lê a cláusula da convenção aplicável. Escritor da chave global `regua_cct_ativa` **[nao verificado]**. `app/escala/services/cadastro_tipo.py:152` `app/escala/services/cadastro_tipo.py:307` `app/ponto/motor_calculo_v2.py:3740` `app/ponto/motor_calculo_v2.py:3765` `app/ponto/motor_calculo_v2.py:3810` `app/ponto/motor_calculo_v2.py:814` `app/ponto/motor_calculo_v2.py:961` `app/core/views_config.py:282` `app/core/views_config.py:312` `app/core/views_config.py:323` `app/core/regua_cct.py:366` `app/core/regua_cct.py:444` `app/core/regua_cct.py:473` |
| Dia do turno e minutos trabalhados | `_dia_do_turno` monta o mapa de instante da entrada para dia do turno. `calcular_mes` guarda o mapa e `_dia_do_turno_do_par` o lê, usando a data local da entrada crua se a chave faltar. A apuração do período escreve `minutos_trabalhados`, e os ramos de dobra exigem duração positiva antes de gravar HE100. `app/ponto/motor_calculo_v2.py:911` `app/ponto/motor_calculo_v2.py:2510` `app/ponto/motor_calculo_v2.py:963` `app/ponto/motor_calculo_v2.py:2898` `app/ponto/motor_calculo_v2.py:2925` `app/ponto/motor_calculo_v2.py:3208` `app/ponto/motor_calculo_v2.py:3372` `app/ponto/motor_calculo_v2.py:3385` `app/ponto/motor_calculo_v2.py:3467` |
| `CelulaDia.trabalha`, `escala_geradora`, `trabalha_em_feriado` e `opera_em_feriado` | A porta da célula grava `trabalha` e `escala_geradora` na geração, regeneração e edição. A tela lê `trabalha`, o juiz do vínculo lê `escala_geradora`, e o motor lê a resposta de `eh_dia_trabalho` para classificar folga. A porta da marca grava `opera_em_feriado` e propaga para vínculos elegíveis via `marcar_trabalha_em_feriado`. O nascimento do vínculo também preenche a chave. `feriado_vira_folga` lê essa chave, o ciclo e o feriado local. `app/ponto/portas/celula.py:289` `app/ponto/portas/celula.py:655` `app/colaboradores/services/calendario.py:791` `app/escala/alimentacao.py:269` `app/escala/models.py:1105` `app/ponto/motor_calculo_v2.py:2560` `app/colaboradores/services/estrutura.py:90` `app/colaboradores/services/feriado_do_posto.py:334` `app/colaboradores/services/feriado_do_posto.py:368` `app/colaboradores/services/vinculo.py:297` `app/colaboradores/services/vinculo.py:903` `app/escala/models.py:1400` |

**O ramo que importa quando a célula diz folga.** O motor pode encaminhar o período para `periodos_ft`. O fechamento filtra esses períodos por `escala_certa_no_dia` e promove a HE100 de feriado aqueles cujo vínculo do dia diz `feriado_vira_folga`. O fechamento grava os totais mensais e `dia_pago.lavrar` grava a mesma partição por dia. Nesse ramo, `feriado_vira_folga` consulta `core.feriados.eh_feriado` pelo lugar do vínculo, a mesma autoridade geográfica usada por `feriados_do_colab`. `app/ponto/motor_calculo_v2.py:2560` `app/ponto/motor_calculo_v2.py:2581` `app/ponto/motor_calculo_v2.py:2605` `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:968` `app/ponto/services/fechamento.py:978` `app/ponto/services/dia_pago.py:244` `app/escala/models.py:1400` `app/core/feriados.py:101`

### O que os registros mandam para feriado do colaborador, célula `False` e batida

| Lei, corte ou obra | Efeito para o caso |
|---|---|
| **L-086** | O texto original mandava dobrar o feriado trabalhado fora da escala. A própria linha registra a revogação da cláusula de feriado. A regra remanescente trata do domingo e preserva o dia de início da jornada. `app/docs/LEIS.md:117` `app/docs/LEIS.md:118` `app/ponto/motor_calculo_v2.py:980` |
| **Corte de 27/09 03:xx** | Para o feriado, valem o dia de início do turno e a régua de dobra do ciclo. `trabalha=False` por si só é insuficiente para concluir pela dobra. No 12x36, o padrão é simples e uma CCT aplicável pode autorizar a dobra. `app/ponto/motor_calculo_v2.py:941` `app/ponto/motor_calculo_v2.py:3740` `app/core/regua_cct.py:473` |
| **O78** | O registro histórico esperava dobra fora da escala. O selo atual testa início no feriado com dobra habilitada, véspera, ciclo sem dobra e a regra de domingo. O selo atual admite dobra mesmo com trabalho previsto quando a régua do ciclo permite. `app/docs/BACKLOG.md:1501` `app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py:43` `app/ponto/tests/test_selo_l086_feriado_previsto_nao_dobra.py:74` |
| **O277 — SDF-FERIADO-SEM-DOBRA** | O corte manda hora sem dobra para SDF e intermitente, e manda o cartão omitir a afirmação de pagamento em dobro. Para SDF, o registro vincula a aplicação ao cadastro da O275. Para intermitente, a cópia já mostra a neutralização do calendário de dobra no motor v2. `app/docs/BACKLOG.md:1698` `app/docs/BACKLOG.md:2245` `app/docs/GRADE-MESTRA.md:234` `app/ponto/motor_calculo_v2.py:3694` |
| **CORTE-CADERNO-1330-4-FERIADO-SO-SEMANAL** | Restringe a chave de previsão “trabalha em feriado” à escala semanal, citando 6x1 e 5x2. Plantões 12x36 e 24x48 seguem a precedência existente. O corte trata da previsão do dia e deixa a rubrica a cargo da regra de pagamento aplicável. `app/docs/CORTES.md:276` `app/docs/CORTES.md:616` |
| **BO-B (3), 23/10 15:0x** | Para trabalho em feriado numa escala que folga em feriado, o corte manda lançar HE100 de feriado. O ramo implementado exige um período em `periodos_ft` que passe por `escala_certa_no_dia` e pelo `feriado_vira_folga` do vínculo do dia. `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:960` `app/ponto/services/fechamento.py:973` `app/ponto/services/dia_pago.py:244` |

O aval relata para **1036 em 20/11** feriado do colaborador, célula `False`, batidas, **7,29 h trabalhadas, 1,16 h noturnas e 0,0 h de HE100**. O ciclo, a régua aplicável, a chave `trabalha_em_feriado` do vínculo e o resultado do crivo de escala certa para esse dia estão **[nao verificado]**. Esses insumos são necessários para atribuir o zero ao ramo exato. `aval.txt:1` `app/ponto/motor_calculo_v2.py:961` `app/ponto/services/fechamento.py:953` `app/ponto/services/fechamento.py:973`

_8658 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
