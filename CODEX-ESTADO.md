# CODEX-ESTADO

> Ultima resposta processada: **09/11/2026 17:35** · publicado em **09/11/2026 17:36** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 09/11 17:35 · sessao 98f00f1fa6ec · FIO-E0-V4-E4-ESCRITORES · construtor · origem=Code

**E4a não está pronta:** o RED final mantém 9 falhas, incluindo identidade, fatos ausentes e registro C20. [Resultado](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_TESTES.txt:784).

- [Entrega: diff, escritores/leitores, lacunas e pendências](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_ENTREGA.txt:1).
- [Hunks textuais para a sessão](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_HUNKS_SESSAO.txt:1).
- [Provas: 24 testes verdes e comandos dos 18 mutantes que falharam e foram restaurados](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_TESTES.txt:1). [nao verificado]

Sem commit, pela proibição de escrever em `.git*`. [Registro](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/E4A_ENTREGA.txt:292). [nao verificado]

## 09/11 17:13 · sessao af1c0c27e636 · - · - · origem=Ronald (tmux)

C20 precisa destes quatro movimentos coordenados:

1. O comando vira vigia puro: retirar `_rodar` e seus blocos de escrita, hoje em [disparar_perguntas_competencia.py:266](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/management/commands/disparar_perguntas_competencia.py:266), e fazer a leitura usar `motivos_que_o_evento_deixou`, cuja composição já reúne os gates da fábrica e a Pauta do limbo. [fabrica_da_celula.py:201](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:201)

2. O consumidor passa a chamar `perguntar_pela_celula` depois do lastro. O `lote` já oferece as datas; carregar as células atuais por colaborador e `data__in={e.data for e in lote}` evita depender do identificador da célula anterior à regeneração. Reutilizar `agora` da rodada e manter as exceções dentro do bloco que deixa eventos pendentes. [reconciliador.py:1337](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1337), [reconciliador.py:1348](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1348), [reconciliador.py:1383](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1383)

3. As fontes precisam permanecer completas após cessar os saves das projeções: retorno do chamado a VIVOS, registro de DP e reapontamento com origem e destino devem emitir o evento no mesmo atomic. Hoje o signal de pergunta depende do save e observa somente a situação posterior; `_gravar_estado` não emite explicitamente o evento. [signals.py:237](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/signals.py:237), [models.py:294](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/models.py:294), [fio.py:922](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fio.py:922)

4. Passado sem marcos já tem corte: não pergunta ao colaborador; cria pendência CADASTRO, uma por colaborador, com contador. O emissor antigo ainda retorna `None` por comparação de datas e alcança `classificar_falta`. [PROMPTS.md:2004](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/PROMPTS.md:2004), [fabrica_da_celula.py:29](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:29), [fabrica_da_celula.py:55](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:55)

A Pauta cabe pela porta existente `pautas.services.escrever`, usando `ANCORA_COLAB`, destinatário DP e marcador estável. Sob atomic e trava do colaborador, consultar toda a história das cabeças e escrever somente se nenhuma existir; o contador deve contar colaboradores, não dias. Essa implementação é proposta, ainda [nao verificado]. A porta e a âncora existem em [services.py:152](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/pautas/services.py:152) e [models.py:30](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/pautas/models.py:30).

O risco de loop vem do consumidor reler eventos novos na mesma execução. A fábrica deve escrever apenas diante de motivos efetivamente ausentes; sua seleção já exclui identidades existentes, e o nascimento da pergunta não aciona o outbox. Uma segunda passagem deve resultar em `motivos=[]`, sem save nem relavra. [reconciliador.py:1343](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/reconciliador.py:1343), [fabrica_da_celula.py:124](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/services/fabrica_da_celula.py:124), [signals.py:239](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/chamados/signals.py:239)

Hunk proibido necessário em `config/crons.py`:

```diff
@@ ENC('disparar_perguntas_competencia', ...)
-    ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --apply',
+    ENC('disparar_perguntas_competencia', gatilho='apurar_furos_diarios', args='--comp-corrente --alarme',
@@ JUIZES_POR_VARREDURA = (
-    # 'disparar_perguntas_competencia' FICA, e so pelo FIO MUDO: no JUIZ-VARREDURA-4 parte A o caminho da CELULA virou
-    # VIGIA (conta o que o evento `fabrica_da_celula.perguntar_pela_celula` deixou, esperado 0, SystemExit(2)); o fio
-    # mudo (chamado vivo sem pergunta viva) segue ESCREVENDO -- nao ha evento dele e a lei esta aberta (CORTES.md,
-    # PAPEL-PRAZO-DISPARAR-PERGUNTAS 27/10).
-    'disparar_perguntas_competencia',
@@ PAPEL_DO_CRON = dict(
     **{n: 'vigia' for n in (
+                            'disparar_perguntas_competencia',
```

Âncoras reais: [crons.py:1521](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:1521), [crons.py:2044](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:2044), [crons.py:2169](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/config/crons.py:2169). Não encontrei registro desse comando em `core/juizes.py`; alteração ali para C20: [nao verificado].

A condição de saída permanece literalmente “fio mudo medido + C11 pousado”. Esta exploração não comprova nenhuma dessas duas condições, nem `20/20`; o contrato estrutural consulta as exceções reais e não deve ganhar `verde=True` artificial. [CORTES.md:305](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/docs/CORTES.md:305), [contratos_estruturais.py:264](/home/ronald/wt-codex-fio-e0-v4-e4-escritores-171001/app/core/contratos_estruturais.py:264) [nao verificado]

## 09/11 17:13 · sessao e00ecb618eb2 · - · - · origem=Ronald (tmux)

Leitura concluída sem editar arquivos. O corte completo de identidade e continente permanece dependente de representação factual ausente; hunks apenas de leitura R/V podem ser aplicados separadamente. (`chamados/models.py:2752`, `chamados/models.py:3474`, `chamados/models.py:3480`, `chamados/models.py:3860`)

**Hunk de apresentação**, arquivo `chamados/models.py`, âncora imediatamente anterior a `PerguntaDisputa.estado` (`chamados/models.py:3022`):

```diff
@@
-    @property
-    def estado(self):
+    @property
+    def atos(self):
+        """Leitura mecanica da sequencia; nao julga tarefa nem efeito."""
+        from chamados.services.atos_pergunta import leitura
+        return leitura(self)
+
+    @property
+    def resposta_do_ato(self):
+        return self.atos.resposta
+
+    @property
+    def respondida_no_ato(self):
+        return self.atos.respondida_em
+
+    @property
+    def validada_no_ato(self):
+        return self.atos.validada_em
+
+    @property
+    def validada_pelo_ato(self):
+        return self.atos.validada_por
+
+    @property
+    def recusa_humana_do_ato(self):
+        return self.atos.recusa_humana
+
+    @property
+    def estado(self):
```

Esse bloco não corrige `estado`: a propriedade ainda contém julgamento por V/C/R e precisa ser substituída por apresentação da autoridade da tarefa. Não propor atribuições aos campos antigos para alimentar o C6: isso perpetuaria a projeção em memória. (`chamados/models.py:3030`, `chamados/models.py:3034`, `chamados/models.py:3035`; `chamados/juizes.py:1323`, `chamados/juizes.py:1339`)

**Hunk de presença da resposta**, arquivo `chamados/models.py`, âncora `PerguntaDisputa.respondida` (`chamados/models.py:3486`):

```diff
@@
     @property
     def respondida(self):
-        return self.respondida_em is not None
+        return self.atos.respondida_em is not None
```

**Hunk de conteúdo discriminado**, arquivo `chamados/models.py`, âncora `PerguntaDisputa.resposta_display` (`chamados/models.py:3508`):

```diff
@@
     def resposta_display(self):
         # S80 - resolve ID para nome legivel quando tipo_resposta e dropdown
-        if not self.resposta_colab:
+        resposta = self.atos.resposta
+        if not resposta:
             return ''
         if self.tipo_resposta == 'posto_dropdown':
             try:
                 from colaboradores.models import Posto
-                p = Posto.objects.filter(pk=int(self.resposta_colab)).first()
+                p = Posto.objects.filter(pk=int(resposta)).first()
@@
-                logger.warning('resposta_display: pk de posto invalido (%r)', self.resposta_colab)
+                logger.warning('resposta_display: pk de posto invalido (%r)', resposta)
@@
-                e = EscalaColaborador.objects.filter(pk=int(self.resposta_colab)).select_related('tipo_escala').first()
+                e = EscalaColaborador.objects.filter(pk=int(resposta)).select_related('tipo_escala').first()
@@
-                logger.warning('resposta_display: pk de escala invalido (%r)', self.resposta_colab)
+                logger.warning('resposta_display: pk de escala invalido (%r)', resposta)
@@
-                if _o.get('valor') == self.resposta_colab:
-                    return _o.get('rotulo', self.resposta_colab)
-        return self.resposta_colab
+                if _o.get('valor') == resposta:
+                    return _o.get('rotulo', resposta)
+        return resposta
```

**Falta fato — A03.** Falta o ato de reabertura do continente, com disputa, autor, instante, motivo e identidade da operação. `REABERTURA` atual representa correção da resposta; DP exige uma pergunta, portanto uma disputa vazia não pode receber esse ato sem inventar pergunta. Acrescentar uma opção em `ATOS` sozinho não resolve. (`chamados/models.py:2123`, `chamados/models.py:2193`, `chamados/models.py:3860`)

Proposta textual **somente para decisão da sessão**, arquivo `chamados/models.py`, âncora `DecisaoPergunta`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    REABRIR_DISPUTA = 'reabrir_disputa'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (REABRIR_DISPUTA, 'Continente reaberto pelo admin'),
```

Para esse ato ser utilizável em continente vazio ainda falta decidir o sujeito factual: tornar `pergunta` opcional e acrescentar `disputa`, com constraint “exatamente um sujeito”, implicaria schema. Não construir essa mudança silenciosamente nesta E4a. (`chamados/models.py:3860`, `chamados/models.py:3877`)

**Falta fato — A17/A18.** Faltam resultado técnico, código, motivo completo, pergunta, instante e autor operacional. Pendência nasce em campos; recusa técnica nasce em campos e nota do chamado. Nenhuma é DP `RECUSA` humana. (`chamados/services/materializacao.py:850`, `chamados/services/materializacao.py:1089`, `chamados/services/materializacao.py:1107`)

Proposta textual, arquivo `chamados/models.py`, âncora `DecisaoPergunta.ATOS`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    PENDENCIA_TECNICA, RECUSA_TECNICA = 'pendencia', 'recusa_tecnica'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (PENDENCIA_TECNICA, 'Materializacao exige atendimento tecnico'),
+        (RECUSA_TECNICA, 'Materializacao recusada tecnicamente'),
```

Só o enum não preserva todo o resultado técnico: `ato_motivo` admite 240 caracteres e `ato_texto` está documentado como resposta. A sessão precisa decidir representação dos dados técnicos; reutilizar texto da resposta para payload quebraria o significado declarado. (`chamados/models.py:3837`, `chamados/models.py:3864`, `chamados/models.py:3865`)

**Falta fato — A20.** Faltam fato durável da conferência, par dos dias, autor, instante e justificativa integral. O log não garante existência: `registrar_log` captura qualquer exceção. (`chamados/services/validacao.py:664`, `chamados/services/validacao.py:667`, `core/auditoria.py:17`, `core/auditoria.py:25`)

Proposta textual, arquivo `chamados/models.py`, âncora `DecisaoPergunta.ATOS`:

```diff
@@
     REABERTURA, SILENCIO, RETRATACAO, ABSORCAO = 'reabertura', 'silencio', 'retratacao', 'absorcao'
+    CONFERENCIA_DATA = 'conferencia_data'
@@
         (ABSORCAO, 'Absorvida pela ausencia declarada'),
+        (CONFERENCIA_DATA, 'Par de dias conferido pelo admin'),
```

A opção tem 16 caracteres e cabe no campo atual, mas não dá campos próprios ao par e à justificativa. Proposta adicional para a sessão decidir, sem aplicação: `ato_dados = models.JSONField(default=dict, blank=True)`, com validação obrigatória específica por ato; isso exige schema e não é hunk executável isoladamente nesta entrega. (`chamados/models.py:3861`, `chamados/models.py:3865`)

**Identidade — impedimento material para N=0 seguro.** As duas constraints parciais excluem linhas através de `via_resolucao`; a porta de retratação escreve essa via e a duplicata nem possui DP correspondente. Cessar a via deixa a linha retratada dentro do índice: um nascimento legítimo posterior colide com ela. Não remover a constraint, substituir por dedup apenas em Python ou fingir que subquery DP cabe na condição de índice. (`chamados/models.py:2053`, `chamados/models.py:2058`, `chamados/models.py:2752`, `chamados/models.py:3471`, `chamados/models.py:3477`)

O teste E5 deve permanecer RED enquanto não existir representação factual de terminalidade consultável pelo banco e tratamento das constraints pela sessão; retirar fisicamente campo não é necessário para demonstrar esse impedimento. (`chamados/models.py:3474`, `chamados/models.py:3480`)

**A23 — idempotência sob trava.** Além de substituir o filtro R em `perguntas_corrigiveis`, a sessão deve mover a seleção corrigível e leitura do contador para dentro do atomic, após trava C/D e releitura. Hoje a guarda e `usadas` são calculados antes da trava: duas chamadas concorrentes podem selecionar a mesma resposta antes de qualquer DP de correção. (`chamados/models.py:2220`, `chamados/models.py:2225`, `chamados/models.py:2232`, `chamados/models.py:2236`; `chamados/juizes.py:1592`)

**A05 — assinatura.** Não reduzir `carimbar` a “não salva”: os ramos `responder` e `validar` hoje completam assinaturas sem DP. O hunk deve registrar DP `RESPOSTA`/`VALIDACAO` no mesmo atomic e no instante `quando`, antes de produzir a consequência, conservando o gate da autoridade e a lavra L. Os testes E4/E9 precisam morder perda desse ato e troca de `quando` pelo relógio da reparação. (`chamados/models.py:3113`, `chamados/models.py:3119`, `chamados/models.py:3123`, `chamados/models.py:3138`)

Não produzi hunk integral de A01–A09/A21–A23 que alegasse fechar o corte: A03 e identidade acima impedem um patch seguro e aplicável isoladamente sem decisão de representação da sessão. [nao verificado]

## 09/11 17:12 · sessao 02db88593fc2 · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; nenhuma edição.

- Vizinho executado: `PYTHONDONTWRITEBYTECODE=1 python3 -B -m unittest chamados.tests.test_e3_pendentes_app` → rc 0, `Ran 6 tests`, `OK`. O módulo usa `unittest`, AST e stubs de módulos; o relógio é explícito. (`chamados/tests/test_e3_pendentes_app.py:2`, `:16`, `:24`, `:118`)

- Infra segura para novos selos: reproduzir a extração AST de funções produtivas sem importar Django ou settings. `config/settings/ci.py` cria diretórios temporários fora de `app/` e configura PostgreSQL; portanto sua importação viola as restrições desta volta. (`config/settings/ci.py:6`, `:25`, `:29`, `:33`)

- E1/escritores: o detector existente usa contaminação por ponto fixo para atingir atributos, `setattr`, construtor com alias, dicionários/defaults, uniões e `bulk_update`. Sua varredura inclui `models.py`. Porém não resolve alias de métodos, como `gravar = qs.update`, e declara não alcançar `__dict__`; ampliar para o contrato novo. (`chamados/tests/test_o261e_f2_porta_carimbo.py:109`, `:133`, `:173`, `:192`, `:204`, `:294`)

- Resolução de alias aproveitável: `c17_callers` resolve imports de símbolo/módulo, atribuições locais, escopo lexical e `getattr`. Há contraprovas sobre `models.py` e registro. (`chamados/tests/test_e3_pendentes_app.py:33`, `:64`, `:75`, `:124`, `:149`)

- E9 puro: extrair `_hoje`, `relogio_virou` e `esta_stale`; fornecer timezone com `ZoneInfo('America/Sao_Paulo')` e pergunta `SimpleNamespace`. Casos relevantes matam `<= → <`, `> → >=`, data UTC em lugar da local, lavra ausente e lavra futura. (`chamados/veredito_lavrado.py:102`, `:107`, `:116`, `:213`, `:224`, `:232`)

- E2 existente depende de Django e `TestCase` com banco. Seus casos de referência para bordas são meia-noite local, instante futuro, relógio envenenado e horizonte de decisões. Escrever novos casos puros no arquivo e executar apenas a classe extraída por AST evita inicializar a infraestrutura proibida. (`chamados/tests/test_e2_tarefa_do_dia.py:59`, `:71`, `:640`, `:657`, `:667`, `:714`)

- E17 render real existente depende de banco: a matriz usa views inteiras, `Client`, autenticação e os dois kernels. Não declarar render admin/app certificado com mera inspeção AST. (`chamados/tests/test_e3_app_orm.py:1`, `:12`, `:28`, `:33`, `:38`)

- Conferência: os testes antigos medem apenas timestamp/autor e idempotência; não provam validade para par de dias alterado. O log é explicitamente best effort e pode devolver `None`; não serve como substituto obrigatório do fato. (`chamados/tests/test_data_do_fato_divergente.py:185`, `chamados/tests/test_porta_validacao_pergunta.py:317`, `core/observ.py:98`, `:139`) Falta ato de conferência em `ATOS`. (`chamados/models.py:3846`)

- Padrões existentes adequados à entrega: `Single Writer Principle`, `Architecture Conformance Test`, `Immutable event log + compensating event`; a linha `padrao=` deve refletir o diff realizado. (`docs/GLOSSARIO.md:12`, `:15`, `:25`; `docs/PLANO-PADROES.md:49`, `:53`)

Recomendação para mutantes: executar a função produtiva extraída de fonte com alteração em memória, mostrar a falha e descartar a fonte mutada. Informar explicitamente essa modalidade; os testes de snippets sintéticos provam o detector, mas não substituem os mutantes comportamentais de produção. (`chamados/tests/test_e3_pendentes_app.py:24`, `:149`; equivalência dessa modalidade ao requisito operacional: [nao verificado])

## 09/11 17:07 · sessao 08f5868560fb · FIO-E0-V4-E4-ESCRITORES · leitura · origem=Code

**É (b): leitor a migrar para o fato antes de cessar a escrita legada.** O template consome campos cuja escritora já está no censo; não acrescenta porta de escrita. (`app/templates/chamados/partials/modal_fio.html:169`, `:170`; `app/chamados/services/validacao.py:664`, `:665`; anexo `censo_v2.r:407`)

| Item | Resultado e evidência |
|---|---|
| **1 — Template: instante** | Dentro do ramo de divergência dos dias, `if perg.data_conferida_em` escolhe mostrar “Conferida em”, formatada `d/m/Y H:i`; sem instante, oferece formulário conforme permissão ou informa bloqueio. (`app/templates/chamados/partials/modal_fio.html:162`, `:169`, `:170`, `:171`, `:179`) |
| **1 — Template: autor** | `if perg.data_conferida_por` decide mostrar autoria; `get_full_name\|default:perg.data_conferida_por.username` apresenta nome completo ou username. (`app/templates/chamados/partials/modal_fio.html:170`) |
| **1 — Contexto/apelidos** | O objeto percorre `p → perguntas_por_chamado → ch.perguntas_disputa → perg`; os campos chegam pelo próprio objeto. (`app/chamados/services/fio.py:225`, `:243`, `:253`; `app/templates/chamados/partials/modal_fio.html:157`) |
| **1 — Views/contextos** | Render completo e parcial chamam o mesmo montador e passam `chamados`; o parcial também fornece `pode_conferir_data` pela permissão `gerir_chamados`. A view de conferência delega autor/justificativa ao serviço. (`app/chamados/views.py:889`, `:927`, `:931`, `:1312`; `app/chamados/services/fio.py:446`, `:465`, `:474`) |
| **1 — Python** | Fora da escritora, `_materializar_uma` lê somente `data_conferida_em`: divergência com instante nulo bloqueia materialização. Dentro da escritora, instante não nulo impede repetir a conferência. (`app/chamados/services/materializacao.py:1193`, `:1199`; `app/chamados/services/validacao.py:662`, `:663`) |
| **1 — JS estático** | Não localizei leitor literal desses valores **[nao verificado]**. O formulário encontrado aciona a rota por HTMX, sem transportar os campos de conferência. (`app/templates/chamados/partials/modal_fio.html:172`, `:174`) |
| **1 — Leituras de teste** | Conferem presença/ausência, autoria por `data_conferida_por_id` e preservação do instante no segundo pedido; `em1` é o apelido local. (`app/chamados/tests/test_data_do_fato_divergente.py:191`, `:192`, `:203`, `:211`; `app/chamados/tests/test_porta_validacao_pergunta.py:292`, `:293`, `:306`, `:315`, `:322`, `:327`) |
| **2 — Escritores** | Porta semântica produtiva: `conferir_data_do_fato`, que atribui instante/autor e persiste ambos; `save` é executor. Há escrita direta de preparação nos testes. A FK ainda declara `SET_NULL` quando o usuário é excluído. (`app/chamados/services/validacao.py:664`, `:665`, `:666`; `app/chamados/models.py:3402`, `:3412`, `:2457`; `app/chamados/tests/test_data_do_fato_divergente.py:155`, `:166`) |
| **2 — Ato correspondente** | A porta emite **`LogAuditoria`: ação `conferir_data`, modelo `PerguntaDisputa`, objeto `p.pk`**, com dia anterior, dia válido e justificativa; a trilha recebe usuário e tem `criado_em` próprio. **Nenhuma DP de conferência** consta em `ATOS`. (`app/chamados/services/validacao.py:667`, `:668`, `:669`, `:670`, `:671`, `:672`; `app/core/auditoria.py:22`, `:24`; `app/core/models.py:295`, `:307`; `app/chamados/models.py:3848`) |
| **3 — E4** | **(b), sem aumentar 26 (+27 informado):** migrar a leitura do instante/autoria para o ato correspondente antes de abandonar suas escritas. O candidato factual existente é o log acima; sua gravação admite falha sem impedir o fluxo. Cobertura histórica por pergunta: **[nao verificado]**. Onde não houver registro equivalente, **falta fato**: conferência daquele par de dias, autor, instante e justificativa. (`app/templates/chamados/partials/modal_fio.html:169`, `:170`; `app/chamados/services/validacao.py:664`, `:667`; `app/core/observ.py:98`, `:139`) |

| 4 — Outros leitores de template ausentes do `censo_v2` fornecido | Ocorrências confirmadas |
|---|---|
| Modal: R/V/F, incluindo indiretos | `app/templates/chamados/partials/modal_fio.html:184`, `:216`, `:218`, `:219`, `:227`, `:378`; `aberta`: `:75`, `:325`; `leitura_da_resposta`: `:249`; `todas_respondidas`: `:333`. |
| Card: R/via/estado/F | `app/templates/chamados/partials/_card_atendimento.html:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; via `:186`; estado `:183`; aberta `:10`, `:15`; correção `:190`. |
| Card de pergunta: R/V | `app/templates/chamados/partials/_pergunta_card.html:20`, `:23`, `:24`, `:29`, `:31`, `:32`, `:42`, `:43`. |
| Preview: R | `app/templates/chamados/partials/_lote_preview.html:24`. |
| F por contexto/propriedade | `app/templates/chamados/fio_colaborador.html:164`, `:169`; `app/templates/chamados/detalhe_local.html:6`, `:296`. |
| Correspondência dos indiretos | `aberta → fechada_em`: `app/chamados/models.py:1821`; `estado → V/C/R`: `:3030`, `:3032`, `:3034`, `:3037`; filtro → R: `app/chamados/templatetags/chamados_tags.py:128`; flags → R/F: `app/chamados/services/fio.py:340`, `:341`, `:342`; contexto → F: `app/chamados/services/acoes_chamado.py:336`. |

Exaustividade por reflexão, SQL ou nomes montados dinamicamente: **[nao verificado]**.

## 09/11 17:06 · sessao a086c7213e12 · - · - · origem=Ronald (tmux)

| Template | Leitores adicionais ao `censo_v2` fornecido |
|---|---|
| `app/templates/chamados/partials/modal_fio.html` | R: `:184`, `:216`, `:218`, `:219`; V: `:227`; F: `:378`, e por `disputa_obj.aberta`: `:75`, `:325`; R indireto por `leitura_da_resposta`: `:249`; R indireto por `todas_respondidas`: `:333`. |
| `app/templates/chamados/partials/_card_atendimento.html` | R: `:94`, `:100`, `:108`, `:135`, `:137`, `:142`, `:148`, `:155`, `:176`, `:177`, `:384`, `:385`; via: `:186`; V/C/R por `p.estado`: `:183`; F por `disputa_obj.aberta`: `:10`, `:15`; R/F por `pode_solicitar_correcao`: `:190`. |
| `app/templates/chamados/partials/_pergunta_card.html` | R: `:24`, `:29`, `:31`, `:32`; V: `:20`, `:23`, `:42`; R indireto por `leitura_da_resposta`: `:43`. |
| `app/templates/chamados/partials/_lote_preview.html` | R: `:24`. |
| `app/templates/chamados/fio_colaborador.html` | F por `disputa_obj.aberta`: `:164`, `:169`. |
| `app/templates/chamados/detalhe_local.html` | F por contexto `disputa_aberta`: `:6`, `:296`; seleção por `fechada_em__isnull=True` em `app/chamados/services/acoes_chamado.py:336`. |

| Comprovação dos aliases | Fonte |
|---|---|
| `aberta` lê `fechada_em`. | `app/chamados/models.py:1821` |
| `estado` lê V/C/R. | `app/chamados/models.py:3030`, `:3032`, `:3034`, `:3035`, `:3037` |
| `resposta_display` lê `resposta_colab`. | `app/chamados/models.py:3510`, `:3515`, `:3523`, `:3530`, `:3532` |
| `leitura_da_resposta` lê R por `getattr`. | `app/chamados/templatetags/chamados_tags.py:128` |
| `todas_respondidas` deriva de `all(p.respondida_em)`; `pode_solicitar_correcao` combina isso com `d.aberta`. | `app/chamados/services/fio.py:340`, `:341`, `:342` |

Correção da mensagem anterior: não há ocorrência de `pode_solicitar_correcao` no modal; a ocorrência encontrada é apenas `app/templates/chamados/partials/_card_atendimento.html:190`.

## 09/11 17:06 · sessao d2ea68ef7256 · - · - · origem=Ronald (tmux)

| Caminho | Evidência e uso |
|---|---|
| Transporte para template | `montar_chamados_fio` busca objetos `PerguntaDisputa` como `p`; agrupa em `perguntas_por_chamado`, anexa em `ch.perguntas_disputa`; template percorre como `perg`. Logo, `p → ch.perguntas_disputa → perg` é o apelido relevante; não há cópia separada dos dois campos. (`app/chamados/services/fio.py:225`, `:243`, `:253`; `app/templates/chamados/partials/modal_fio.html:157`) |
| Seleção/preload | Perguntas das disputas abertas; nas fechadas aplica `q_vivas` e depois `pergunta_viva`. `select_related` inclui disputa/célula/destino; prefetch inclui decisões com `validada_original_por`/`ato_por`; não inclui `data_conferida_por`. (`app/chamados/services/fio.py:221`, `:224`, `:225`, `:226`, `:227`, `:231`, `:238`) |
| Contexto completo | `_resp_modal_full` chama `montar_chamados_fio(..., com_explicacao=True)` e passa `'chamados': chamados` para `modal_fio.html`. Não calcula outro valor de conferência. (`app/chamados/views.py:889`, `:927`, `:931`) |
| Contexto parcial | `contexto_parcial_fio` chama o mesmo montador, passa `chamados` e define `pode_conferir_data` pela ação `gerir_chamados`. Consumido no re-render pós-ação de `processar_post_fio` e na reabertura de disputa. (`app/chamados/services/fio.py:446`, `:465`, `:474`, `:496`, `:497`; `app/chamados/views.py:1399`, `:1400`) |
| Leitura do par no modal | Dentro do ramo de divergência entre dias, `if perg.data_conferida_em` decide exibir conferência em vez do formulário/bloqueio. Mostra timestamp `d/m/Y H:i`; `if perg.data_conferida_por` controla a autoria; usa `get_full_name|default:...username`. Sem timestamp, oferece formulário se `pode_conferir_data`, senão informa bloqueio. (`app/templates/chamados/partials/modal_fio.html:162`, `:169`, `:170`, `:171`, `:172`, `:179`) |
| Casca HTTP | `conferir_data_pergunta` valida método/permissão e delega ao serviço com autor e justificativa; não lê os campos. (`app/chamados/views.py:1308`, `:1310`, `:1312`, `:1313`) |
| JS | Busca literal por `data_conferida`/`conferir_data` em `app/**/*.js` não encontrou ocorrência; cobertura dinâmica: **[nao verificado]**. O acionamento concreto está no atributo HTMX do template, `hx-post` para `conferir_data_pergunta`, `hx-swap="none"`. (`app/templates/chamados/partials/modal_fio.html:172`) |

Detalhe potencialmente útil: o contexto **completo** não fornece `pode_conferir_data`; o parcial fornece. Portanto a diferença comprovável é ausência/presença dessa chave nos dois construtores, sem inferir resultado produtivo. (`app/chamados/views.py:927`; `app/chamados/services/fio.py:474`; comportamento produtivo: **[nao verificado]**)

## 09/11 16:29 · sessao f6ee62474778 · FIO-E0-V4-E4-ESCRITORES · leitura · origem=Code

**O pré-desenho precisa incluir a troca dos leitores junto dos escritores.** Nesta cópia, a consulta central do app ainda depende de R/V/C/F; a tarefa ainda depende da via atual e consulta R/V para reconhecer passivo sem decisão. Cortar apenas as escritas deixaria consumidores usando projeções abandonadas. (`app/chamados/services/pergunta_no_app.py:48`, `:57`, `:70`; `app/chamados/services/tarefa_do_dia.py:227`, `:346`)

**Alvo declarado: N=0 portas de escrita de estado derivado da pergunta**, conservando decisões imutáveis, efeitos canônicos e L como cache declarado. Isso é alvo do desenho, ainda não resultado certificado. A própria definição de `DecisaoPergunta` declara transitórias as projeções até a E4. (`app/chamados/models.py:3832`; resultado alcançado: **[nao verificado]**)

Não cabe prometer uma única construção Codex: portas e leitores em `models.py`, migrations e eventuais ajustes em `bin/` são proibidos ao construtor. A quantidade de cópias/voltas necessária é **[nao verificado]**. (`bin/codex_pergunta.sh:108`, `:112`, `:113`, `:115`, `:330`)

Para compactar os endereços abaixo:

- `models.py` significa `app/chamados/models.py`.
- Arquivos de serviços sem prefixo significam `app/chamados/services/<arquivo>`.
- `juizes.py` significa `app/chamados/juizes.py`.
- Os demais caminhos aparecem completos.

Os números produtivos abaixo são exclusivamente os do anexo fornecido; sua reprodução por banco ou arquivo local é **[nao verificado]**. A identidade criptográfica independente da cópia também é **[nao verificado]**.

**A. MATRIZ DAS PORTAS**

R = resposta/texto/instante; V = validação/autor; C = materialização/autor/via; F = fechamento/autor/resolução/motivo; L = veredito/julgamento/expiração. Esses grupos não têm significado uniforme de “aconteceu”: isso está discriminado em B. (`models.py:2444`, `:3023`, `:3092`, `:3366`; `app/chamados/veredito_lavrado.py:213`)

As referências **H-R, H-V, H-C, H-F, H-S, H-L** remetem aos leitores discriminados na tabela H. “DIVERGE” significa dependência produtiva existente que a transição precisa retirar; não afirma uma ocorrência produtiva nova além do anexo. (`models.py:3315`; `pergunta_no_app.py:70`; `materializacao.py:1369`)

| Célula / porta em 05e6aa147 | Classificação e escrita atual | Fato substituto já existente | Leitores atuais / E3 | Resultado esperado |
|---|---|---|---|---|
| **A01 — `models.py:201 _gravar_estado`** | Efeito/decisão: `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`; prioridade/atualização; relavra perguntas. | **Chamado**, transição e trilha do ato. (`models.py:294`) | H-S; fila ainda lê status do chamado. (`painel.py:155`; `tarefa_do_dia.py:363`) | Conservar a porta do efeito chamado; retirar consequências que perpetuem estado derivado da pergunta. Estado canônico do chamado não entra no alvo zero da pergunta. (`models.py:201`; `models.py:3832`) |
| **A02 — `models.py:1852 fechar`** | Efeito no fechamento normal; decisão nas exceções. Grava F e delega C. | Exceção: DP `SILENCIO`. Normal: quitação pelos efeitos; F e trilha registram o fechamento atual. (`models.py:1921`, `:1929`, `:1937`) | H-F/H-C. App ainda particiona por F. **DIVERGE**. (`pergunta_no_app.py:70`) | Fechamento derivado deixa de comandar exigibilidade. Preservar o ato excepcional e a história antes de retirar projeções. (`tarefa_do_dia.py:149`; `models.py:3832`) |
| **A03 — `models.py:2123 reabrir_admin`** | Decisão: limpa F; incrementa contador/agrega motivos. | **Nenhum DP próprio dessa reabertura do continente**; há gesto, contador e trilha. (`models.py:2135`, `:2146`, `:2158`) | H-F. **DIVERGE**: DP `REABERTURA` da resposta não representa automaticamente esse gesto. (`disputa_emissao.py:1612`) | Preservar decisão, autor, limite e motivo; retirar F como cópia de exigibilidade somente com substituição factual do gesto. (`models.py:2123`; `app/docs/LEIS.md:65`) |
| **A04 — `models.py:2160 reabrir_sistema`** | Efeito: limpa F. | Pergunta voltou a viver; nota de sistema existente. (`models.py:2170`, `:2177`) | H-F. **DIVERGE**. (`models.py:1820`; `pergunta_no_app.py:70`) | Exigibilidade vem da tarefa; não depender de “reabrir F” para tornar ação alcançável. (`tarefa_do_dia.py:442`) |
| **A05 — `models.py:3092 carimbar`** | Efeito no carimbo; decisão nos ramos de assinatura. C; completa R/V; pode gravar L. | Batida/Ausência/chamado/lavra conforme ramo. Assinaturas R/V desta porta não ganham DP automaticamente. (`models.py:3119`, `:3123`, `:3140`) | H-R/H-V/H-C/H-L. **DIVERGE**. (`models.py:3023`; `juizes.py:1323`) | C deixa de ser escrito; assinaturas passam pelo fato de decisão; L permanece explicitamente cache. (`models.py:3832`; `app/chamados/veredito_lavrado.py:254`) |
| **A06 — `models.py:3157 carimbar_identidade`** | Efeito; decisão humana depende do chamador. C e recusa opcional. | Retratação de motivo tem DP; duplicata e outras vias não possuem representação factual uniforme equivalente. (`models.py:2053`; `tarefa_do_dia.py:220`) | H-C; **a própria tarefa lê via atual**. **DIVERGE de identidade**. (`tarefa_do_dia.py:227`; `models.py:2743`) | Não abandonar via enquanto dedup, índice e tarefa dependem dela. Factualizar terminalidade antes de substituir esse discriminador. (`models.py:3474`, `:3480`; `tarefa_do_dia.py:216`) |
| **A07 — `models.py:3192 descarimbar`** | Efeito: limpa C; também preparação em memória. | Fato/decisão que invalida carimbo, conforme chamador. A ausência de carimbo não é o fato. (`models.py:3192`; `materializacao.py:639`) | H-C. **DIVERGE**. (`models.py:3315`; `relavra_via.py:181`) | Retirar a escrita da projeção junto dos consumidores; manter a consequência factual e invalidação de L necessária. (`models.py:3832`; `app/chamados/veredito_lavrado.py:213`) |
| **A08 — `models.py:3241 descarimbar_em_lote`** | Efeito: limpa C e grava campos recebidos em `outros`, inclusive R/V. | DP da correção/recusa quando chamada por esse fluxo. (`disputa_emissao.py:1612`) | H-R/H-V/H-C. **DIVERGE**. (`models.py:3253`; `materializacao.py:1369`) | Nenhum campo derivado pode continuar entrando por `outros`; preservar atos e execução em lote. (`models.py:3241`; `models.py:3832`) |
| **A09 — `models.py:3257 encerrar_por_fato_no_espelho`** | Efeito: `via_resolucao=fato_materializado`. | Fato do marco no espelho/lavra. (`models.py:3258`; `juizes.py:645`) | H-C. **DIVERGE** nos consumidores de via. (`explicador.py:68`; `models.py:3182`) | Não escrever via derivada; ler confirmação do efeito pela autoridade. (`tarefa_do_dia.py:140`) |
| **A10 — `validacao.py:127 _carimbar_validacao`** | Decisão: V + DP `VALIDACAO`. | DP no mesmo instante da validação. (`validacao.py:134`, `:139`) | H-V. **DIVERGE**; tarefa já interpreta DP, mas outros consumidores ainda usam V. (`tarefa_do_dia.py:140`; `juizes.py:1323`) | V projetado deixa de ser escrito; o ato permanece. (`models.py:3832`) |
| **A11 — `validacao.py:163 devolver_validacao_recusada`** | Decisão: limpa V, preserva resposta; DP `DEVOLUCAO`. | DP `DEVOLUCAO`. (`validacao.py:205`) | H-V/H-R. **DIVERGE**. Tarefa já lê o ato. (`tarefa_do_dia.py:138`) | Nenhuma limpeza de V projetado; atos anteriores continuam imutáveis. (`models.py:3818`; `models.py:3832`) |
| **A12 — `disputa_emissao.py:1422 responder_pergunta_disputa_core`** | Decisão: R, invalida V/C, pode escrever recusa explicativa. | DP `RESPOSTA`, inclusive texto/escape. (`disputa_emissao.py:1494`, `:1508`) | H-R/H-V/H-C. **DIVERGE**. (`pergunta_no_app.py:70`; `materializacao.py:1370`) | Conteúdo e instante vêm da sequência; eliminar manutenção de R/V/C projetados. (`models.py:3832`) |
| **A13 — `disputa_emissao.py:1559 reabrir_para_correcao`** | Decisão: limpa R/V e opcionalmente C; **não escreve F**. | DP `REABERTURA` ou `RECUSA`, com foto anterior. (`disputa_emissao.py:1612`, `:1625`) | H-R/H-V/H-C. **DIVERGE**. (`models.py:3903`; `models.py:3315`) | Registrar ato seguinte sem apagar resposta anterior; consumidores deixam de depender da limpeza. (`models.py:3811`; `models.py:3832`) |
| **A14 — `disputa_emissao.py:1635 responder_automaticamente`** | Decisão de sistema: R sem sobrescrever texto existente. | DP `RESPOSTA`. (`disputa_emissao.py:1653`) | H-R; a própria guarda usa texto projetado. **DIVERGE**. (`disputa_emissao.py:1644`) | Preservar voz existente e idempotência pela sequência, sem escrever R. (`models.py:3832`) |
| **A15 — `disputa_emissao.py:1658 carimbar_resposta_pela_trilha`** | Decisão reconstruída: `respondida_em`. | Testemunha da resposta + DP `RESPOSTA`. (`disputa_emissao.py:1674`) | H-R; guarda ainda usa projeção. **DIVERGE**. (`disputa_emissao.py:1667`) | Conservar instante testemunhado; não substituí-lo pelo instante do reparo. (`disputa_emissao.py:1658`) |
| **A16 — `relavra_via.py:106 relavrar`** | Efeito: via derivada + L. | Autoridade do efeito e lavra. (`relavra_via.py:95`, `:118`) | H-C/H-L. **DIVERGE** na via. (`relavra_via.py:118`; `explicador.py:68`) | Encolher para manutenção declarada de L; retirar derivação de via sem consumidores sobreviventes. (`app/chamados/veredito_lavrado.py:254`; `app/docs/LEIS.md:137`) |
| **A17 — `materializacao.py:784 _materializar_resposta_hora`** | Efeito: `pendencia_admin`, `recusa_motivo`; plantio/carimbos delegados. | Sucesso: Batida. Pendência técnica: **nenhum DP próprio**. (`materializacao.py:850`, `:859`) | H-P/H-REC, além de R/C. Template também lê pendência. **DIVERGE**. (`explicador.py:85`; `app/templates/chamados/partials/modal_fio.html:221`) | Pendência deixa de ser estado paralelo; apresentar resultado técnico pela autoridade real, preservando trilha do insucesso. Não convertê-lo em recusa humana. (`app/docs/LEIS.md:60`, `:65`; `materializacao.py:1107`) |
| **A18 — `materializacao.py:1083 _carimbar_recusa`** | Efeito técnico: recusa; colisão delega C de identidade. | Colisão/resultado técnico + nota `recusa_materializacao`; **nenhum DP `RECUSA` humano**. (`materializacao.py:1099`, `:1107`) | H-REC/H-C; própria idempotência lê recusa/C. **DIVERGE**. (`materializacao.py:1089`, `:1092`) | Não usar string ou C como prova de decisão; conservar motivo técnico sem criar ato humano inexistente. (`models.py:3849`; `materializacao.py:1107`) |
| **A19 — `validacao.py:533 recusar_pergunta`** | Decisão: recusa, limpeza R/V delegada. | DP `RECUSA`. (`validacao.py:584`, `:593`; `disputa_emissao.py:1612`) | H-REC/H-R/H-V; guarda usa R. **DIVERGE**. (`validacao.py:576`) | Motivo e foto vêm do ato; nenhuma manutenção dos campos derivados. (`models.py:3840`) |
| **A20 — `validacao.py:625 conferir_data_do_fato`** | Decisão: `data_conferida_em/por`. | **Outro fato existente:** `LogAuditoria` com os dois dias e justificativa. Não há ato DP de conferência. (`validacao.py:664`, `:667`; `models.py:3846`) | H-DATA. Materializador exige timestamp; modal exibe instante **e autor**. **DIVERGE**. (`materializacao.py:1193`; `app/templates/chamados/partials/modal_fio.html:170`) | Substituir o sinalizador por leitura inequívoca do ato de conferência; preservar o par de dias, autor e justificativa. (`validacao.py:667`; `app/docs/LEIS.md:65`) |
| **A21 — `models.py:1968 remover_motivo`** | Decisão; motivos/contexto próprios; C delegado. | DP `RETRATACAO`. (`models.py:2053`) | Tarefa lê DP e ainda via; C6 lê via/C. **DIVERGE**. (`tarefa_do_dia.py:227`, `:230`; `juizes.py:1334`) | Entra apesar do “fica” parcial: retirar a dependência da consequência C, preservando retratação e metadados. (`models.py:2053`, `:2058`) |
| **A22 — `models.py:2822 _nascer`** | Nascimento/reuso: identidade, defaults projetados, costura e L. | Identidade factual e decisões já existentes no reuso. (`models.py:2822`, `:2997`) | H-R/H-V/H-C; identidade continua usando via. **DIVERGE** se defaults/reuso mantiverem campos abolidos. (`models.py:2743`, `:3474`) | Entra por obrigação do diff: nascimento e reuso não podem reintroduzir projeções retiradas. Identidade e L permanecem. (`models.py:3832`) |
| **A23 — `models.py:2179 reabrir_por_colab`** | Contador próprio; decisão delegada. | DP da correção + contador factual do pedido. (`models.py:2234`, `:2236`) | Gate lê F; idempotência atual depende de R ser limpo. **DIVERGE**. (`models.py:2201`, `:2221`) | Preservar teto/contador; migrar gate e idempotência para atos, sem repetir incremento na segunda chamada. (`models.py:2223`, `:2236`) |
| **A24 — `models.py:2247 limpar_resolucao_residual`** | Limpeza de `resolucao` em aberta. | Trilha com texto anterior. (`models.py:2259`, `:2266`) | H-F. **DIVERGE** se F/resolução forem retirados. (`models.py:2259`) | Entra por obrigação do diff; depois da conservação histórica, perde trabalho próprio e deve sair com seus consumidores. (`app/docs/LEIS.md:137`) |

As **20 portas centrais de alteração** conservam os endereços apresentados no censo; os seis nascimentos continuam em `models.py:833`, `:1054`, `:2822` e `disputa_emissao.py:474`, `:1024`, `:1299`. Isso não comprova identidade de commits sem metadados Git. (`models.py:201`, `:1852`, `:2123`, `:2160`, `:3092`, `:3157`, `:3192`, `:3241`, `:3257`; `validacao.py:127`, `:163`, `:533`, `:625`; `disputa_emissao.py:1422`, `:1559`, `:1635`, `:1658`; `relavra_via.py:106`; `materializacao.py:784`, `:1083`; identidade criptográfica: **[nao verificado]**) [nao verificado]

Duas referências do censo são **ocorrências de leitura**, não início de função: `cobranca.py:229` fica dentro de `_calados`, definido em `:221`; `fabrica_da_celula.py:128` fica dentro de `motivos_a_perguntar`, definido em `:74`. Não são evidência de deslocamento entre as árvores. (`cobranca.py:221`, `:229`; `fabrica_da_celula.py:74`, `:128`)

As referências antigas do aval a badge em `models.py:1729/1736` não apontam o leitor atual: há lápide em `models.py:1795`; os consumidores atuais chamam a consulta central em `app/api/views.py:325` e `app/api/views_core.py:314`. (`models.py:1729`, `:1736`, `:1795`; `app/api/views.py:325`; `app/api/views_core.py:314`)

**Conta:** o “16” é o inventário inicial de alterações; o censo corrigido conta **16 + 4 alterações adicionais = 20**, e depois **20 + 6 nascimentos = 26**. Não são o mesmo universo. As quatro adicionais são A17–A20; executores `save`/`registrar` não devem ser somados novamente às portas que executam. (`materializacao.py:784`, `:1083`; `validacao.py:533`, `:625`; `models.py:3402`, `:3932`; número histórico do censo anexado: **[nao verificado]**)

**RESULTADO 11, com unidade explícita: 16 → N=0 portas de escrita de estado derivado da pergunta; lista residual: vazia.** No censo corrigido, a comparação correspondente é **20 → 0**, fiscalizando também os nascimentos. Separadamente permanecem o executor factual `DecisaoPergunta.registrar`, as portas dos efeitos e os escritores declarados de L. Não seria comparável anunciar “16→4” somando três famílias de efeitos a um executor de decisões. (`models.py:3832`, `:3932`; `app/chamados/veredito_lavrado.py:240`, `:254`; realização desse alvo: **[nao verificado]**)

**B. CAMPO A CAMPO — ESMERIL-SEMÂNTICA**

| Célula | Significado e divergência comprovável | Onde morde A |
|---|---|---|
| **B01 — R: presença** | `respondida` testa instante não nulo; SQL combina instante/texto; C6 usa texto com `strip`; tarefa usa última DP e trata projeção sem linha como desconhecimento. São critérios distintos. (`models.py:3486`, `:3337`; `juizes.py:1339`; `tarefa_do_dia.py:127`, `:132`) | A05, A08, A12–A15, A22–A23. |
| **B02 — R: conteúdo** | Resposta pode ser hora, data/hora, escolha, identificador ou escape. Intervalo pode plantar pela hora do marco. Não transformar qualquer texto em instante. (`models.py:3508`; `materializacao.py:356`, `:400`, `:978`) | A05, A12–A15, A17. |
| **B03 — V** | V é assinatura, não confirmação de efeito. C6 encerra por V não nulo; tarefa com DP `VALIDACAO` exige L vigente e distingue materializar, rejulgar e encerrar. Ramo de carimbo também preenche V. (`juizes.py:1323`; `tarefa_do_dia.py:140`; `models.py:3123`) | A05, A10–A13. |
| **B04 — C** | `materializada_em` não significa uniformemente “Batida criada”: identidade/colisão também carimbam; identidade pode conservar instante e autor antigos. Nulo pode resultar de limpeza posterior. (`models.py:3157`, `:3171`, `:3192`; `materializacao.py:1099`) | A05–A09, A16–A18. |
| **B05 — via/identidade** | Via mistura resultado derivado com exclusão da identidade do fato. Retratação tem DP datada; duplicata não tem equivalente datado na tarefa atual. Índices e seleção ainda dependem da via. (`tarefa_do_dia.py:216`, `:220`, `:227`; `models.py:3474`, `:3480`) | A06–A08, A16, A21–A22. |
| **B06 — F** | F significa fechamento do continente, não inexistência de tarefa. App manda viva em fechada ao histórico; pedido de correção mantém gate de fechada. Reabertura da resposta não é reabertura do continente. (`pergunta_no_app.py:70`; `models.py:2221`; `disputa_emissao.py:1559`) | A02–A04, A13, A23–A24. |
| **B07 — pendência** | Pendência técnica precede o rótulo: resposta pode parecer validada/resolvida e ainda exigir admin. Explicador e templates dependem disso. (`materializacao.py:850`; `explicador.py:85`; `app/templates/chamados/partials/modal_fio.html:221`; `app/templates/chamados/partials/_card_atendimento.html:183`) | A17; saída dos leitores de A05/A10. |
| **B08 — recusa** | Humana gera DP `RECUSA`; técnica grava campo/nota; advertência de escape acompanha DP `RESPOSTA`. Não reconstruir recusa humana de qualquer string nesse campo. (`validacao.py:593`; `materializacao.py:1103`, `:1107`; `disputa_emissao.py:1494`, `:1508`) | A06, A12–A13, A17–A19. |
| **B09 — conferência** | `data_conferida_em/por` representa autorização humana diante de dois dias divergentes. Materializador usa apenas presença do timestamp; trilha guarda o par/justificativa; modal usa também autor. (`materializacao.py:1193`; `validacao.py:667`; `app/templates/chamados/partials/modal_fio.html:170`) | A20. Validade da conferência após alteração do par de dias: **[nao verificado]**. |
| **B10 — dia versus instante** | `data_evento` é dia do turno no fato e dia civil na conversa. Plantio combina dia/hora e ajusta overnight pelo envelope inclusivo. O instante da Batida não é esse dia. (`models.py:2791`; `juizes.py:296`, `:332`; `materializacao.py:413`) | A17, A20, A22; identidade de A06/A21. |
| **B11 — L e bordas** | `julgado_em` é instante; expiração é dia local. Expira por `<=`; marco posterior invalida por `>`. Ausente/indeterminado não equivale a “não materializado”. C6 ainda usa janela inclusiva e relógio que `--agora` não governa integralmente. (`app/chamados/veredito_lavrado.py:116`, `:224`, `:232`; `juizes.py:575`, `:601`; `app/chamados/management/commands/diff_e3_sombra.py:105`) | A05/A16; medidas C e leitores H-L. Divergência produtiva efetiva nessa borda: **[nao verificado]**. |
| **B12 — chamado** | `VIVOS`, `COBRANCA`, `ENCERRADOS`, `TERMINAIS` e `NAO_FECHADOS` respondem perguntas diferentes. “Tem tarefa” não substitui globalmente esses conjuntos. (`app/chamados/catalogo/motor.py:24`, `:29`, `:30`, `:31`, `:32`) | A01 e nascimentos do chamado; classificação C. |
| **B13 — dois silêncios** | Véu temporário vigora com `silenciado_ate > agora`, final exclusivo. DP `SILENCIO` é encerramento excepcional da pergunta. Não são o mesmo ato. (`models.py:511`, `:1921`; `arquivo.py:154`) | A02; filtros de fila usados por A01 e pela tarefa. |
| **B14 — instante histórico** | Reconstrução usa instante da testemunha; reparação ocorre depois. Ordenação das decisões é por `ato_em, pk`; carga considera atos `<= agora`. (`disputa_emissao.py:1658`; `models.py:3878`; `tarefa_do_dia.py:338`) | A15; backfill C3. |

Logo, **não**, escritores e leitores não supõem todos o mesmo significado dos campos. A retirada precisa conservar essas distinções na leitura factual, sem uma propriedade local voltar a ser segundo juiz. (`app/docs/LEIS.md:60`; evidências B01–B14)

**C. O FIO MORTO E O PASSIVO**

**C1 — contadores e universos**

| Contador | Definição efetiva |
|---|---|
| `fio_morto` | Células acusadoras alcançadas por chamados vivos de `FURO_MODULES

[truncado em 20000 de 59200 caracteres]

## 09/11 16:22 · sessao 1b6b5f41c939 · - · - · origem=Ronald (tmux)

Achado que impede retirar escritores isoladamente: o app ainda lê `R/V/C` por `q_vivas` e C6 e depois particiona por `R/F`; a tarefa lê DP, mas ainda depende de via atual, L, da projeção R/V para detectar passivo e de status do chamado pela fila. Isso é DIVERGE estrutural para qualquer célula de A que deixe esses campos sem manutenção antes da migração dos leitores. (`app/chamados/services/pergunta_no_app.py:48`; `app/chamados/services/pergunta_no_app.py:57`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/tarefa_do_dia.py:227`; `app/chamados/services/tarefa_do_dia.py:346`; `app/chamados/services/tarefa_do_dia.py:351`; `app/chamados/services/tarefa_do_dia.py:365`)

**B — distinções semânticas verificadas**

| ID | Campo e significado | Onde diverge e célula afetada |
|---|---|---|
| B1 | `respondida_em`: instante do ato, reconstruível pela testemunha; `respondida` significa só não nulo. (`app/chamados/services/disputa_emissao.py:1658`; `app/chamados/models.py:3486`) | SQL exige instante + texto não vazio; C6 normaliza texto com `strip`; tarefa usa a última DP RESPOSTA e `texto_vazio`, e passivo sem linha produz desconhecimento. Timestamp sozinho, texto sozinho e ato não são equivalentes. A: responder/reabrir/carimbar/resposta automática/reconstrução. (`app/chamados/models.py:3337`; `app/chamados/juizes.py:1339`; `app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/services/tarefa_do_dia.py:131`) |
| B2 | `resposta_colab`: conteúdo discriminado por `tipo_resposta`, opções e escapes; não representa sempre hora. (`app/chamados/models.py:3508`; `app/chamados/services/materializacao.py:356`; `app/chamados/services/materializacao.py:978`) | Plantio pode usar marco para intervalo; não converter todo texto em instante. Reabertura limpa texto e R/V juntos; limpar só R devolve resposta antiga por leitores de texto. A: responder/reabrir/materializar. (`app/chamados/services/materializacao.py:400`; `app/chamados/services/disputa_emissao.py:1564`; `app/chamados/services/disputa_emissao.py:1590`) |
| B3 | `validada_em/por`: assinatura da validação atual, não prova de efeito. Escritor humano carimba agora e registra DP no mesmo instante. (`app/chamados/services/validacao.py:134`; `app/chamados/services/validacao.py:139`) | C6 mata por V não nulo antes de conferir efeito; tarefa com DP VALIDACAO exige L vigente e distingue materializar/rejulgar/encerrada. Autor do V pode vir do ramo `carimbar(resposta='validar')`, portanto V não prova decisão humana independente. A: validar/devolver/carimbar. (`app/chamados/juizes.py:1323`; `app/chamados/services/tarefa_do_dia.py:140`; `app/chamados/models.py:3123`) |
| B4 | C = `materializada_em/por` e `via_resolucao`: carimbo de processamento, identidade ou encerramento; não é uniformemente Batida criada. (`app/chamados/models.py:3063`; `app/chamados/models.py:3157`) | Help text promete batida criada + chamado fechado, mas carimbo de identidade pode preservar instante/autor antigo; recusa COLISAO carimba sem plantar. A: carimbar/carimbar_identidade/_carimbar_recusa/relavrar. (`app/chamados/models.py:3384`; `app/chamados/models.py:3171`; `app/chamados/services/materializacao.py:1093`; `app/chamados/services/materializacao.py:1099`) |
| B5 | `via_resolucao` também delimita identidade do fato; duplicata/retratada são terminais. (`app/chamados/models.py:3182`; `app/chamados/models.py:3474`; `app/chamados/models.py:3480`) | Não pode simplesmente parar de escrevê-la: consulta SQL, índices parciais e tarefa ainda excluem por ela; DP RETRATACAO existe, mas duplicata não tem ato datado equivalente na tarefa atual. A: carimbar_identidade/remover_motivo/nascimento/reapontar. (`app/chamados/models.py:3341`; `app/chamados/services/tarefa_do_dia.py:220`; `app/chamados/services/tarefa_do_dia.py:227`) |
| B6 | F = fechamento do continente; `aberta` é `fechada_em is None`; fechamento excepcional registra DP SILENCIO por pergunta atingida. (`app/chamados/models.py:1820`; `app/chamados/models.py:1921`; `app/chamados/models.py:1929`) | App põe viva em disputa fechada no histórico; POST continua guardado. Reabertura administrativa/sistema limpa F sem ato DP próprio. Portanto DP atual não substitui integralmente F/contador/gesto do continente. A: fechar/reabrir_admin/reabrir_sistema. (`app/chamados/services/pergunta_no_app.py:70`; `app/chamados/models.py:2146`; `app/chamados/models.py:2170`; `app/chamados/models.py:2221`) |
| B7 | `pendencia_admin` e `recusa_motivo`: pendência técnica pode manter estado aparentemente encerrado; par é escrito junto. (`app/chamados/services/materializacao.py:848`; `app/chamados/services/materializacao.py:850`) | Explicador precisa ler pendência antes da prova do efeito para não esconder ação do admin. Não existe DP específica para essa pendência no universo ATOS atual. A: _materializar_resposta_hora. (`app/chamados/services/explicador.py:82`; `app/chamados/services/explicador.py:85`; `app/chamados/models.py:3846`) |
| B8 | `recusa_motivo` mistura recusa humana, técnica e aviso sobre escape. Humana gera DP RECUSA; técnica escreve campo/followup; escape gera DP RESPOSTA. (`app/chamados/services/validacao.py:584`; `app/chamados/services/validacao.py:593`; `app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1107`; `app/chamados/services/disputa_emissao.py:1494`; `app/chamados/services/disputa_emissao.py:1508`) | Não reconstruir DP RECUSA de qualquer string de recusa: produziria uma decisão humana inexistente. A: recusar/_carimbar_recusa/responder/materializar. (`app/chamados/models.py:3849`; `app/chamados/services/tarefa_do_dia.py:135`) |
| B9 | `data_conferida_em/por`: ato humano que olhou dois dias divergentes e justificou qual vale; não significa simples coincidência das datas. (`app/chamados/services/validacao.py:641`; `app/chamados/services/validacao.py:664`; `app/chamados/services/validacao.py:667`) | Materializador lê somente timestamp como autorização; DP não contém ato de conferência atual, porém LogAuditoria recebe dois dias/justificativa. Fato substituto já existente = outro, trilha de conferência, cuja disponibilidade produtiva concreta não foi medida. A: conferir_data_do_fato. (`app/chamados/services/materializacao.py:1193`; `app/chamados/services/validacao.py:668`; `app/chamados/models.py:3846`) |
| B10 | `data_evento`: dia do turno para fato, dia civil de nascimento para conversa; não é timestamp da Batida. (`app/chamados/models.py:2780`; `app/chamados/models.py:2791`; `app/chamados/models.py:2798`) | Plantio combina dia/hora e ajusta overnight por cadeia/envelope inclusivo; não preencher default com dia civil nem escolher data do chamado em silêncio. A: nascimento/materialização/conferência. (`app/chamados/services/materializacao.py:413`; `app/chamados/juizes.py:296`; `app/chamados/services/materializacao.py:1193`) |
| B11 | L: `julgado_em` é instante; `janela_expira_em` é dia local; `sim/nao/indeterminado/ausente` não são dois estados booleanos. (`app/chamados/veredito_lavrado.py:107`; `app/chamados/veredito_lavrado.py:213`; `app/chamados/veredito_lavrado.py:257`) | Expiração `<= dia local`; marco posterior invalida por `>`; C6 janela é inclusiva e usa `date.today()`. `--agora` não governa C6. A: relavrar/carimbar e leitores L; C: medida em sombra. (`app/chamados/veredito_lavrado.py:116`; `app/chamados/veredito_lavrado.py:232`; `app/chamados/juizes.py:575`; `app/chamados/juizes.py:601`; `app/chamados/management/commands/diff_e3_sombra.py:105`) |
| B12 | `status_local`: VIVOS, COBRANCA, ENCERRADOS, TERMINAIS e NAO_FECHADOS são conjuntos distintos. (`app/chamados/catalogo/motor.py:24`; `app/chamados/catalogo/motor.py:29`; `app/chamados/catalogo/motor.py:30`; `app/chamados/catalogo/motor.py:31`; `app/chamados/catalogo/motor.py:32`) | `registrado` vive mas não cobra; `resolvido` pode ser não fechado. Não trocar qualquer teste de status por “tem tarefa”. Chamado é efeito permitido pelo aval; `status_local` dele não pode ser eliminado globalmente como se fosse só quarta cópia da pergunta. A: _gravar_estado e nascimentos. (`app/chamados/catalogo/motor.py:26`; `app/chamados/models.py:201`; `app/chamados/models.py:833`) |

Nulo não tem significado único transversal: em R/V/C é ausência de carimbo atual (inclusive porque foi limpo), em L pode ser desconhecimento/nunca lavrado, em `data_conferida_em` é ausência da autorização de conferência; não equiparar esses nulos. (`app/chamados/services/disputa_emissao.py:1590`; `app/chamados/models.py:3197`; `app/chamados/veredito_lavrado.py:224`; `app/chamados/services/materializacao.py:1193`)

**H — escritores e leitores, agrupamento para incorporar à tabela única**

Os leitores abaixo são referências produtivas sintáticas conferidas; são endereços de função quando agrupados. ORM, `getattr`, templates e operações por strings também precisam entrar no selo; só AST de `Attribute Load` não demonstra exaustividade e consumidores dinâmicos permanecem [nao verificado]. (`app/chamados/services/explicador.py:85`; `app/chamados/models.py:3337`; `app/chamados/tests/test_o261e_f2_porta_carimbo.py:109`)

| Campo | Escritores semânticos | Leitores produtivos a migrar/conservar |
|---|---|---|
| `resposta_colab/respondida_em` | `app/chamados/services/disputa_emissao.py:1422`, `:1559`, `:1635`, `:1658`; ramo R de `app/chamados/models.py:3092`; defaults/reuso `app/chamados/models.py:2822` | `app/chamados/models.py:1823`, `:1852`, `:3023`, `:3092`, `:3315`, `:3486`, `:3508`, `:3903`; `app/chamados/juizes.py:339`, `:1274`, `:1322`, `:1444`, `:1590`, `:1597`, `:1613`, `:1765`, `:1871`, `:1892`, `:1915`; `app/chamados/services/pergunta_no_app.py:60`; `app/chamados/services/cobranca.py:229`; `app/chamados/services/materializacao.py:244`, `:330`, `:425`, `:462`, `:496`, `:639`, `:784`, `:978`, `:1370`; `app/chamados/services/validacao.py:127`, `:163`, `:234`, `:373`, `:533`, `:845`, `:912`; `app/chamados/services/declaracao_texto.py:306`, `:641`; `app/chamados/services/disputa_emissao.py:1635`, `:1658`, `:1798`; `app/chamados/services/explicador.py:25`, `:68`; `app/chamados/services/fio.py:163`, `:485`, `:1216`, `:1227`; `app/chamados/services/acoes_disputa.py:118`, `:302`; `app/chamados/services/becos.py:25`; `app/chamados/services/historico.py:69`; `app/chamados/reconciliador.py:340`, `:1060`; `app/chamados/views.py:1228`; `app/chamados/views_cobrar.py:145`, `:275`; `app/api/views.py:991`, `:2549`, `:2572`, `:2607`; `app/api/views_mensageria.py:554`; `app/ponto/supra_juiz.py:539`; `app/ponto/services/cartorio.py:1597`; `app/ponto/services/veredito_celula.py:160`; `app/ponto/services/dia_do_colab.py:6`; `app/folha/previa.py:139`; `app/colaboradores/services/ficha.py:99`; diagnóstico `app/chamados/services/vigia_fio.py:542` |
| `validada_em/por` | `app/chamados/services/validacao.py:127`, `:163`; limpeza por `app/chamados/services/disputa_emissao.py:1422`, `:1559`; ramo V `app/chamados/models.py:3092`; defaults `app/chamados/models.py:2822` | `app/chamados/models.py:1823`, `:3023`, `:3092`, `:3315`, `:3903`; `app/chamados/juizes.py:1322`; `app/chamados/services/materializacao.py:639`, `:1371`; `app/chamados/services/validacao.py:127`, `:144`, `:163`, `:373`, `:533`, `:695`; `app/chamados/services/declaracao_texto.py:216`; `app/chamados/services/verbos.py:218`; `app/chamados/services/painel.py:268`; `app/chamados/services/disputa_emissao.py:1422`, `:1798`; `app/chamados/services/acoes_disputa.py:302`; `app/ponto/services/procedencia_batida.py:54`; consultas `app/ponto/services/flip_auto.py:130`, `app/chamados/services/vigia_carimbo.py:98`, `app/chamados/reconciliador.py:630`, `:1049` |
| `materializada_em/por` | `app/chamados/models.py:3092`, `:3157`, `:3192`, `:3241`; atribuição executora em `:3086`; restauração em `:3148` | `app/chamados/models.py:1823`, `:1852`, `:3023`, `:3039`, `:3092`, `:3157`, `:3268`, `:3278`, `:3290`, `:3315`; `app/chamados/juizes.py:282`, `:645`, `:1322`, `:1411`, `:1915`; `app/chamados/services/materializacao.py:425`, `:496`, `:603`, `:639`, `:1083`, `:1372`, `:1419`; `app/chamados/services/validacao.py:163`, `:373`; `app/chamados/services/disputa_emissao.py:1798`; `app/chamados/services/raiox.py:153`; `app/chamados/services/relavra_via.py:187`; `app/chamados/services/painel.py:67`; `app/chamados/reconciliador.py:340`, `:1112`, `:1049`, `:1500`; `app/chamados/views.py:1228`; `app/ponto/services/veredito_celula.py:160`; `app/ponto/services/procedencia_batida.py:54`; `app/chamados/services/fabrica_da_celula.py:148` |
| `via_resolucao` | C acima; adicional `app/chamados/models.py:3257`; `app/chamados/services/relavra_via.py:106` | C acima mais identidade: `app/chamados/models.py:2694`, `:2743`, `:3182`, `:3474`, `:3480`; `app/chamados/services/tarefa_do_dia.py:163`; `app/chamados/services/fio.py:1075`, `:1110`, `:1216`, `:1227`, `:1336`, `:1413`, `:1831`, `:1869`, `:1896`, `:1914`; `app/chamados/services/passivo_validado.py:131`, `:474`; `app/chamados/services/explicador.py:68`; `app/chamados/services/fabrica_da_celula.py:128`; `app/ponto/services/cartorio.py:1610`; `app/ponto/services/flip_auto.py:129` |
| `fechada_em/por/resolucao/motivo_fechamento` | `app/chamados/models.py:1852`, `:2123`, `:2160`; residual `:2247`; defaults em `app/chamados/services/disputa_emissao.py:474`, `:1024`, `:1299` | `app/chamados/models.py:1820`, `:1852`, `:1968`, `:2123`, `:2160`, `:2179`, `:2247`, `:2626`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/juizes.py:2306`; `app/chamados/services/materializacao.py:496`, `:1302`; `app/chamados/services/validacao.py:144`, `:163`; `app/chamados/services/disputa_emissao.py:34`, `:474`, `:942`, `:1024`, `:1299`, `:1798`; `app/chamados/services/becos.py:25`, `:36`; `app/chamados/services/acoes_disputa.py:302`; `app/chamados/services/fio.py:485`; `app/chamados/services/relavra_via.py:187`; `app/chamados/signals.py:19`, `:121`, `:148`; `app/api/views.py:991`, `:2693`; consultas adicionais: `app/colaboradores/painel_cell.py:32`, `app/colaboradores/signals.py:73`, `app/escala/signals.py:157`, `app/api/foto_recebida.py:62`, `app/colaboradores/services/situacional.py:117`, `app/colaboradores/services/drawer.py:70`, `app/chamados/services/fila_por_causa.py:225`, `app/chamados/services/regularizacao_ext.py:59` |
| `pendencia_admin` | `app/chamados/services/materializacao.py:850` | `app/chamados/services/explicador.py:85`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1112` |
| `recusa_motivo` | `app/chamados/services/materializacao.py:850`, `:1102`; `app/chamados/services/validacao.py:584`; `app/chamados/services/disputa_emissao.py:1494`; opcional executor `app/chamados/models.py:3177` | `app/chamados/services/materializacao.py:1092`; `app/chamados/services/validacao.py:144`, `:163`, `:533`; `app/chamados/services/explicador.py:90`; `app/ponto/services/flip_auto.py:132`; leituras por strings `app/chamados/services/acoes_disputa.py:81`, `app/chamados/services/fio.py:518`, `:1351`; auditoria `app/chamados/management/commands/auditar_invariantes_chamados.py:536`, `:1075`, `:1112` |
| `data_conferida_em/por` | `app/chamados/services/validacao.py:664` | em: `app/chamados/services/validacao.py:662`, `app/chamados/services/materializacao.py:1193`; por: consumidor adicional [nao verificado] (`app/chamados/services/validacao.py:665` é escrita) |
| `veredito_lavrado/julgado_em/janela_expira_em` | `app/chamados/veredito_lavrado.py:240`, `:254`; composição de save `app/chamados/models.py:3140`, reuso `:2997`, `app/chamados/services/relavra_via.py:128`, `app/chamados/services/fio.py:1034` | `app/chamados/veredito_lavrado.py:213`, `:279`; `app/chamados/services/tarefa_do_dia.py:351`; `app/chamados/services/raiox.py:34`, `:35`, `:153`; `app/chamados/services/relavra_via.py:106`; snapshot `app/chamados/services/passivo_validado.py:65`; `app/chamados/management/commands/backfill_veredito_lavrado.py:82`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1173` |
| `resolvido_em/respondido_por/via_encerramento` | `app/chamados/models.py:201`; motor inicial pelos nascimentos `:833`, `:1054` | `app/chamados/models.py:663`, `:695`, `:983`, `:1318`, `:1362`; `app/chamados/services/verbos.py:218`; `app/chamados/services/painel.py:50`, `:247`; `app/chamados/services/fila.py:109`; `app/chamados/views_arquivar.py:52`, `:153`; `app/chamados/services/disputa_emissao.py:1779`; `app/chamados/services/competencia_trancada.py:163`, `:235`; `app/chamados/services/adesao.py:44`; `app/ponto/services/furos_sem_cobranca.py:69`; relatórios `app/relatorios/views.py:738`, `:914` |
| `DecisaoPergunta.*` | porta `app/chamados/models.py:3932`; `save` só inserção `:3892`; autores das linhas nos escritores R/V/recusa/reabertura, `:1921` SILENCIO, `:2053` RETRATACAO e `app/chamados/services/materializacao.py:563` ABSORCAO | `app/chamados/services/tarefa_do_dia.py:127`, `:131`, `:140`, `:149`, `:334`; `app/chamados/models.py:3489`, `:3903`; backfill `app/chamados/management/commands/backfill_decisoes_pergunta.py:44`, `:163` |

O inventário completo de `status_local` é muito maior, conforme o censo já colado; o escritor canônico é `_gravar_estado` e os leitores incluem a própria fila que alimenta tarefa, sinais/cascatas, dedup, motor e guardas do chamado. A enumeração global por AST encontra também modelos homônimos, consultas ORM, snapshots e testes em `tests.py`; não transforma esse achado bruto em censo completo tipado sem conferência. (`app/chamados/models.py:201`; `app/chamados/services/painel.py:155`; `app/chamados/services/painel.py:189`; `app/chamados/services/tarefa_do_dia.py:365`; `app/chamados/catalogo/motor.py:125`)

**E — testemunhas existentes e contrato que falta**

1. Imutabilidade/mesmo relógio/recusa/reabertura/SILENCIO/RETRATACAO já têm casos em `app/chamados/tests/test_e1_entradas.py:64`, `:78`, `:94`, `:103`, `:115`, `:121`, `:142`; esses casos não provam que E4 parou de gravar os derivados. Mutante necessário para A/B1–B8: restaurar qualquer atribuição a R/V/C depois de migrar o leitor. (`app/chamados/tests/test_e1_entradas.py:64`; `app/chamados/tests/test_e1_entradas.py:217`)
2. AST com apelido deve cobrir atributos, `setattr`, `update/create`, dict espalhado, alias de modelo e alias de método; há detector exemplar e mutantes concretos, inclusive método novo dentro de models. Sua allowlist atual autoriza C nas portas e precisa virar ausência para campos abolidos, com exceção L declarada. (`app/chamados/tests/test_o261e_f2_porta_carimbo.py:109`; `:226`; `:270`; `:283`; `:296`; `:515`; `:583`)
3. Selo novo de “nenhum leitor produtivo lê campo abolido” precisa morder atributo, `getattr`, ORM, `values`, `order_by`, agregação, strings e template; catálogo de consumidor dinâmico fora disso permanece [nao verificado]. O detector E3 atual resolve imports e apelidos, mas só sua pergunta específica; não é esse selo geral. (`app/chamados/tests/test_e3_pendentes_app.py:34`; `:149`; `app/chamados/services/explicador.py:85`; `app/chamados/services/painel.py:67`)
4. Matriz nova app/admin/tarefa deve pôr projeção deliberadamente contraditória com DP/efeito e exigir saída igual pela regra; mutantes “consulta volta a R/F”, “C6 volta a V”, “tarefa volta a via” mordem B1/B3/B5/B6. Testes atuais preservam os três chamadores C17, e não certificam E4 completa. (`app/chamados/tests/test_e3_pendentes_app.py:144`; `:159`; `app/chamados/tests/test_e3_portas_tarefa.py:113`)
5. Caso B9: conferência divergente libera pelo ato humano; concordante não fabrica ato; duas datas mudadas depois não devem herdar conferência de outro par. Mutantes: nulo vira autorização, ou qualquer conferência antiga autoriza todo par. Os dois primeiros comportamentos estão no código; validade da conferência após mudança do par é risco técnico a medir, não lei nova inventada. (`app/chamados/services/validacao.py:658`; `:662`; `app/chamados/services/materializacao.py:1193`)
6. B10/B11: madrugada, envelope exatamente na borda, L expirando exatamente hoje, lavra futura, marco exatamente igual a

[truncado em 20000 de 21890 caracteres]

## 09/11 16:20 · sessao 34f39634760c · - · - · origem=Ronald (tmux)

**C — definições e passivo**

| Contador | Universo e incremento |
|---|---|
| `fio_morto` | Parte dos chamados `VIVOS` com célula, recorta `FURO_MODULES` e lê células com veredito acusador; tira fio vivo, hoje/futuro e quatro baldes com ação/dono; incrementa por célula restante. `app/chamados/services/vigia_fio.py:335`, `:349`, `:378`, `:418`, `:437`, `:449`. |
| `fio_morto_cobrando` | Subconjunto dos dias acima com algum chamado do vocabulário em `COBRANCA`; mapa em `app/chamados/services/vigia_fio.py:351`, incremento em `:451`. |
| `fio_morto_alcance` | Dia-colaborador que segura folha, nas competências enumeradas por `competencias_a_lavrar`, sem porta, fora de hoje/futuro e dos quatro baldes; universo em `app/chamados/services/vigia_fio.py:580`, `:586`, `:591`; porta em `:638`; exclusões em `:648`, `:654`; incremento em `:695`. Fontes reais: `app/folha/fora_da_folha.py:102`, `app/ponto/services/he_pendente_lavrado.py:209`. |
| `materializada_viva` | Quantidade de perguntas com carimbo elegível a desfazer, disputa aberta e julgadas vivas por C6; separa competência entregue de aberta. Universo em `app/chamados/services/vigia_fio.py:805`, juiz em `:813`, dono em `:816`, contador em `:847`; elegibilidade canônica em `app/chamados/services/relavra_via.py:171`, `:181`. |

A medida anexada permite classificar **377 = 2 + 0 + 17 + 284 + 74**, sem nova sonda. Os números vêm do anexo, sem arquivo local numerado: **[nao verificado]**. A interpretação das classes vem dos seguintes sítios:

| Classe | Número anexado | Significado efetivo e limite da conclusão |
|---|---:|---|
| a | 2 | Chamado vivo fora da fila; detalhes anexados `registrado`. Não equivale automaticamente a silêncio temporário. `app/chamados/services/vigia_fio.py:664`, `:669`. |
| b | 0 | Pergunta viva respondida sem texto. `app/chamados/services/vigia_fio.py:673`. |
| c | 17 | Pergunta viva respondida sob destino terminal. `app/chamados/services/vigia_fio.py:675`. |
| d | 284 | Sem pergunta viva, colaborador aparece na CxR, **dia não aparece** na lista. A classe não prova que o dia tem dono CADASTRO/BATIDA. `app/chamados/services/vigia_fio.py:677`; declaração explícita em `:520`. |
| e | 74 | 50 `pergunta_viva_sob_chamado_terminal` + 24 `sem_pergunta_viva`, conforme anexo **[nao verificado]**; regras em `app/chamados/services/vigia_fio.py:685`, `:687`. |

Os 170 `fio_do_admin`, 276 `decisao_do_dia_pendente`, 16 `aguarda_decisao_de_ausencia`, 2 `escala_nao_descreve_o_dia` e 3 `alcance_de_hoje_pra_frente` não compõem os 377, pelo controle de fluxo de `app/chamados/services/vigia_fio.py:648`, `:654`, `:657`, `:695`; seus valores são do anexo **[nao verificado]**.

Também não se pode somar os 2 `materializada_viva` aos 377: um contador conta perguntas, o outro conta dias, e a interseção não foi fornecida. `app/chamados/services/vigia_fio.py:591`, `:818`, `:851`.

Os cinco dias do universo antigo já vêm divididos em quatro com perguntas da porta sem alvo e um sem pergunta na porta; `fio_morto_com_alvo=0`, conforme anexo **[nao verificado]**. A porta `passivo_validado` não é solução universal: exclui `plantio_fora_do_slot` e `slot_nao_derivavel` em `app/chamados/services/passivo_validado.py:60`, aplicando esse veto em `:330`; o alvo do vigia deriva dessa declaração em `app/chamados/services/vigia_fio.py:152`.

**O que pode cair prospectivamente:** parar carimbos derivados indevidos e completar decisões E1 impede novas combinações incompatíveis; porém o efeito quantitativo de cada porta sobre cada contador é **[nao verificado]**. Há dependência real dos leitores antigos: `censo` ainda chama `fio_do_dia`/C6 em `app/chamados/services/vigia_fio.py:418`, e `censo_alcance` usa `alimentar_portas`/C17 em `:604`, `:638` e `dono_fora_do_fio` em `:654`. Só remover escritores não demonstra queda imediata nesses contadores.

Para separar o passivo por cura real, “**precisa medida na sombra**”: chamar `censo_alcance`, `carimbos_sem_lastro_por_dono`, `tarefa_do_dia` e as portas correspondentes; para os 284, ler a mesma `lista`/`dias_na_lista` carregada por `ListaDoAdmin._carregar`, sem inventar mapa classe→dono. `app/chamados/services/vigia_fio.py:212`, `:216`, `:217`, `:542`, `:789`; `app/chamados/services/tarefa_do_dia.py:442`. O próprio vigia proíbe deduzir dono de classe em `app/chamados/services/vigia_fio.py:99`.

**C3 — backfill único, desenho e limitações das portas existentes**

1. Inventário DRY pela fonte real, agrupado por competência e com todas as linhas sem porta; não truncar o passivo. O censo já conserva todo `sem_porta` em `app/chamados/services/vigia_fio.py:630`; competência real em `app/ponto/services/fechamento.py:1602`. Leis de origem: L-003 `app/docs/LEIS.md:61`, L-121 `:142`, L-124 `:145`.
2. Preservar a projeção legada como decisão antes de retirar o campo que a guarda: produtor canônico `DecisaoPergunta.linhas_da_projecao_sem_linha` em `app/chamados/models.py:3903`, inserção canônica `registrar` em `:3932`. Não fabricar texto de respostas antigas sobrescritas: limite declarado em `app/chamados/management/commands/backfill_decisoes_pergunta.py:12`.
3. **Não afirmar que o `--aplicar` E1 atual resolve essa etapa.** Ele mede as projeções R/V, mas explicitamente não as aplica; só escreve colisões e silêncio faltante. `app/chamados/management/commands/backfill_decisoes_pergunta.py:3`, `:6`, `:25`, `:426`.
4. Exportada fica fora com lista para o `!`, conforme pedido literal do usuário **[nao verificado]**; precedente mecânico real: lista de projeções por competência em `app/chamados/management/commands/backfill_decisoes_pergunta.py:455`, `:471`, `:480`; grupos exportados fora e rc 3 em `:620`. Não usar a permissividade geral de L-113 para alargar esse aval específico: L-004 `app/docs/LEIS.md:62`.
5. Escrever reversão antes de qualquer alteração, incluindo estado das perguntas, disputas, chamados e fatos atingidos. Precedente existente em `app/chamados/management/commands/reabrir_orfas_enterradas.py:123`, `:128`, `:137`; o comando E1 atual não fornece essa reversão em arquivo **[nao verificado]**.
6. DRY e APPLY devem atravessar a mesma porta, com rollback no DRY; molde existente em `app/chamados/management/commands/backfill_decisoes_pergunta.py:578`, `:582`, `:584`. Ordem de travas real em `app/chamados/models.py:2626`; uso no reparo em `app/chamados/management/commands/reabrir_orfas_enterradas.py:153`.
7. Por espécie de ato: decisão faltante por `registrar`; Batida pela materialização que chega a `registrar_batida` em `app/chamados/services/materializacao.py:926`, `:1006`; ausência por `app/chamados/services/materializacao.py:462`; ausência de pergunta pela fábrica `app/chamados/services/fabrica_da_celula.py:139`; carimbo sem lastro pela porta `app/chamados/services/relavra_via.py:187`, hoje chamando `descarimbar` em `:246`. Essas portas precisam da cura E4 antes de reutilização no backfill.
8. Idempotência não vem de `registrar`: ele faz `bulk_create` direto em `app/chamados/models.py:3934`. A seleção canônica deduplica por pergunta+ato+instante em `:3916`, `:3920`, `:3924`; deve ser relida sob trava. O requisito “duas execuções, mesmo estado, uma trilha por ato” ainda exige selo E4 **[nao verificado]**.
9. A porta de desfazer carimbo já evita segunda trilha quando não há ação: relê elegibilidade sob trava em `app/chamados/services/relavra_via.py:230`, `:238`, continua se não há candidato em `:240`, grava trilha só depois da mudança em `:248`.
10. O carimbo único é sobrescrito por `os.replace` em `app/chamados/management/commands/vigiar_fio_morto.py:134`; sozinho não prova três rodadas consecutivas. As três evidências precisam ser retidas pela sessão ou receber contrato próprio de sequência **[nao verificado]**.

**D — alarme**

- `ALARMA_EM_0` atual contém três chaves em `app/chamados/management/commands/vigiar_fio_morto.py:83`; `ESPERADO_DO_CARIMBO` já leva `materializada_viva=0` e `fio_morto_alcance=0` em `:96`. Os esperados não precisam virar 377/2 nem ganhar tolerância.
- **Armadilha concreta:** apenas acrescentar as duas chaves à tupla causa consulta inexistente: `gritou` lê `d[k]` em `:179`, enquanto alcance/mvv só são anexados ao dicionário do carimbo em `:221`. O desenho deve usar a composição dos contadores antes de julgar o alarme. Esse ajuste está dentro de `app/`.
- O texto/comentário “fora do alarme até zerar” está em `:93`; `materializada_viva` também se apresenta como “não alarma” em `:195`. Precisam acompanhar o contrato final.
- O cron já usa `--alarme --alarme-passivo`: `app/config/crons.py:1718`; não é necessário mudar esse arquivo proibido apenas para ativar as novas chaves.
- Com números não zero, o painel **já** acusa FORA DO ESPERADO: `bin/fio_estado.py:125`, `:180`. Ativar o alarme antes do backfill acrescenta `gritou` e rc 2: `app/chamados/management/commands/vigiar_fio_morto.py:204`, `:232`; o carimbo fica gravado antes da saída em `:207`, `:211`.
- Ordem de saída: portas/leitores coerentes → backfill permitido → nova medição → prova das três rodadas → alarme definitivo; essa ordem operacional cumpre o aval fornecido **[nao verificado]**.
- Leitor direto do JSON: `bin/fio_estado.py:98`, `:104`; compara todos os esperados em `:125`, lê chaves/alarme em `:146`, monta aviso condicionado em `:182`. Consumidor da representação: `bin/gerar_estado.py:260`. Outros leitores por acesso dinâmico: **[nao verificado]**.
- `bin/fio_estado.py:182` já deixa de imprimir o aviso quando a chave entra no carimbo; não exige hunk funcional para essa retirada. Docstring velha em `bin/fio_estado.py:50` é hunk de sessão, por estar fora de `app/`.

**Selos que precisam mudar / mutantes**

1. O selo atual exige que alcance **não** esteja no alarme: `app/chamados/tests/test_o261e_f5_alcance.py:459`, `:471`. Inverter para exigir inclusão após a etapa; mutante: retirar a chave.
2. O conjunto de chaves antigas é fixado em `app/chamados/tests/test_o261_vigia_fio.py:753`, `:759`, e exige presença no `censo` simples em `:767`. Adaptar ao mapa composto; mutantes: continuar usando `d[k]`; omitir alcance/mvv da composição.
3. Selo funcional: fio antigo zero com alcance=1 e depois mvv=1 deve alarmar e deixar JSON completo; mutantes: ler só `d`, calcular `gritou` antes da composição, sair antes de `_carimbar`. Arquivo E4 proposto ainda inexistente **[nao verificado]**.
4. Prova do carimbo como fotografia já existe em `app/chamados/tests/test_o261_vigia_fio.py:812`, esperados em `:819`.
5. Idempotência de desfazer carimbo já tem precedente em `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:122`; E4 precisa medir a operação inteira, sem supor que o precedente cobre todo o lote. Mutante: `registrar` incondicional na segunda corrida.
6. Exportada intocada precisa comparar hash de FM/TXT antes/depois e testar exportação surgindo entre DRY e trava; selo E4 ainda **[nao verificado]**. Precedente de lista/recusa atual em `app/chamados/management/commands/backfill_decisoes_pergunta.py:480`, `:620`.

**Proibidos para C/D**

A cura da porta `descarimbar` e das decisões de projeção exige hunk de sessão em `app/chamados/models.py:3192`, `:3903`; a composição/alarme fica com o construtor em `app/chamados/management/commands/vigiar_fio_morto.py:178`; representação/documentação de host fica com a sessão em `bin/fio_estado.py:50`; o cron existente já passa as opções em `app/config/crons.py:1718`. Migração só por este subescopo C/D não é demonstrada; remoção física de campos demanda etapa de sessão **[nao verificado]**.

## 09/11 16:20 · sessao 8e342f692882 · - · - · origem=Ronald (tmux)

Todas as **20 definições de alteração** abaixo estão nas mesmas linhas do censo fornecido. Os seis nascimentos também permanecem em `models.py:833,1054,2822` e `disputa_emissao.py:474,1024,1299`. Identidade criptográfica da cópia: **[nao verificado]**, pois `git rev-parse HEAD` encontrou diretório sem repositório Git.

Abreviações de leitores, todos de produção:

- **LC**: `app/chamados/juizes.py:645,796,1322,1411`; `app/chamados/models.py:3023,3182,3315`; `app/chamados/services/materializacao.py:425,496,1089,1369`; `app/chamados/services/relavra_via.py:118`; `app/chamados/services/raiox.py:51,153`. São leituras do campo C/via; C6 e pré-filtro permanecem ativos.
- **LR**: `app/chamados/models.py:3023,3315,3486,3508,3903`; `app/chamados/juizes.py:1339,1444,1590,1597,1765`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/materializacao.py:829,1369`; `app/chamados/services/validacao.py:576,580,912`; `app/chamados/services/disputa_emissao.py:1644,1667`. Ainda leem R.
- **LV**: `app/chamados/models.py:3023,3315,3903`; `app/chamados/juizes.py:1323`; `app/chamados/services/materializacao.py:1369`; `app/chamados/services/validacao.py:144,373,573,695`; `app/ponto/services/procedencia_batida.py:54`. Ainda leem V.
- **LF**: `app/chamados/models.py:1820,1852,2123,2160,2179,2247,2626`; `app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/disputa_emissao.py:34,474,942,1024,1299`; `app/chamados/signals.py:19,121,148`. Ainda leem F.
- **LS**: `app/chamados/models.py:119,128,201,410,663,695,717,773,800,809,833,983,1318,1362,1602,1619`; `app/chamados/services/painel.py:96,135,158,198`; `app/chamados/reconciliador.py:238,761,961,1201,1441,1529`; `app/chamados/signals.py:54,75,91,179,188,270,337`. Ainda leem estado canônico do chamado.
- **LL**: `app/chamados/veredito_lavrado.py:107,151,213,279`; `app/chamados/services/tarefa_do_dia.py:349`; `app/chamados/services/raiox.py:10,51,153`. L é cache declarado; a tarefa já o lê em `tarefa_do_dia.py:351-352`.

| Célula/porta atual | Campo escrito | Fato disponível para substituir | Leitor / divergência e destino esperado |
|---|---|---|---|
| A01 `app/chamados/models.py:201 _gravar_estado` — efeito/decisão | `status_local,resolvido_em,via_encerramento,respondido_por`, urgência e L delegado | **Chamado**, transição e trilha no mesmo ato (`:294-300`) | LS; permanece porta canônica do efeito chamado. Não pode somar seu estado canônico como se fosse cópia derivada da pergunta. |
| A02 `models.py:1852 fechar` — efeito/exceção decisão | F; C delegado | DP `SILENCIO` nas exceções (`:1921`); fechamento normal só F/trilha (`:1930-1937`) | LF/LC. **DIVERGE**: fechamento normal não possui fato DP equivalente; exceção possui. Separar fato humano de fechamento derivado antes de retirar F. |
| A03 `models.py:2123 reabrir_admin` — decisão | limpa F, contador/motivos | Nenhuma linha DP desta reabertura; autor, contador e F apenas (`:2135-2158`) | LF. **DIVERGE**: não confundir com DP `REABERTURA` da resposta. Metadados/limite permanecem; decisão precisa fato antes de retirar F. |
| A04 `models.py:2160 reabrir_sistema` — efeito | limpa F | Pergunta voltou a viver, com nota no chamado (`:2174-2177`) | LF. **DIVERGE**: consumidores ainda exigem F aberto; retirar projeção apenas junto da troca. |
| A05 `models.py:3092 carimbar` — efeito/assinatura decisão | C, completa R/V, opcional L | Batida/Ausência/chamado/ata conforme ramo; DP apenas se outro chamador já o gravou | LC/LR/LV/LL. **DIVERGE**: a porta completa assinatura em `:3119-3125` sem registrar DP. Remover escrita C; assinatura não pode sumir; L declarado permanece. |
| A06 `models.py:3157 carimbar_identidade` — efeito/decisão delegada | C, `recusa_motivo` opcional | DP retratação quando chamado por `remover_motivo:2053`; outros ramos não têm um fato DP uniforme | LC; a própria tarefa lê via atual (`tarefa_do_dia.py:216-228`). **DIVERGE crítico**: duplicata não tem fato datado (`:220-221`); parar via quebra identidade/dedup, além da pendência. |
| A07 `models.py:3192 descarimbar` — efeito | limpa C | Retração/ausência rejeitada/resposta nova conforme chamador | LC. **DIVERGE**: remover limpeza junto de todos consumidores e do índice que usa via, nunca isoladamente. |
| A08 `models.py:3241 descarimbar_em_lote` — efeito | limpa C e `outros` R/V recebidos | DP da reabertura (`disputa_emissao.py:1612`) quando esse é chamador | LC/LR/LV. **DIVERGE**: filtro terminal por via ainda necessário (`models.py:3253-3255`); efeito deve ler decisões/identidade factual. |
| A09 `models.py:3257 encerrar_por_fato_no_espelho` — efeito | via | Fato do marco já lavrado, descrito em `:3258-3261` | LC; **DIVERGE** para leitores via. Via derivada deixa de ser escrita quando leitores conferirem efeito. |
| A10 `app/chamados/services/validacao.py:127 _carimbar_validacao` — decisão | V | DP `VALIDACAO` no mesmo instante (`:139-141`) | LV; **DIVERGE** nos leitores V. V sai; DP é fato. |
| A11 `validacao.py:163 devolver_validacao_recusada` — decisão | limpa V, preserva R | DP `DEVOLUCAO` (`:205`) | LV/LR; **DIVERGE**. Fatos anteriores preservados, tarefa lê ato em `tarefa_do_dia.py:138-139`. |
| A12 `app/chamados/services/disputa_emissao.py:1422 responder_pergunta_disputa_core` — decisão | R, limpa V/C, recusa explicativa | DP `RESPOSTA` (`:1508`); escape é `ato_texto`, não DP `RECUSA` | LR/LV/LC; **DIVERGE**. R/V/C derivados saem; resposta passa a ser conteúdo do ato vigente. |
| A13 `disputa_emissao.py:1559 reabrir_para_correcao` — decisão | limpa R/V e C opcional | DP `REABERTURA`/`RECUSA` com foto (`:1612-1625`) | LR/LV/LC; **DIVERGE**. Não escreve F hoje; não atribuir a esta porta reabertura da disputa. |
| A14 `disputa_emissao.py:1635 responder_automaticamente` — decisão sistema | R | DP `RESPOSTA` (`:1653-1654`) | LR; **DIVERGE**. Idempotência atual usa texto projetado (`:1644`); precisa consultar ato. |
| A15 `disputa_emissao.py:1658 carimbar_resposta_pela_trilha` — decisão reconstruída | `respondida_em` | DP `RESPOSTA`, instante da testemunha (`:1674-1675`) | LR; **DIVERGE**. Guarda atual R/texto (`:1667`) precisa ler sequência; não trocar instante por agora. |
| A16 `app/chamados/services/relavra_via.py:106 relavrar` — efeito | via + L | Via derivada do fato (`:95-103`), L da autoridade | LC/LL; **DIVERGE** na via. Encolhe para L, ou desaparece quando não tiver trabalho próprio; não deixar segundo juiz morto (L-111). |
| A17 `app/chamados/services/materializacao.py:784 _materializar_resposta_hora` — efeito | `pendencia_admin,recusa_motivo`, efeitos/carimbos delegados | Batida quando sucesso (`:859`); na pendência técnica o fato DP é **nenhum** (`:845-851`) | `app/chamados/services/explicador.py:68`; `materializacao.py:784`; auditorias. **DIVERGE**: mensagem técnica não vira decisão humana. Recalcular resultado técnico pela porta real ou guardar fato do insucesso, sem conservar estado derivado disfarçado. |
| A18 `materializacao.py:1083 _carimbar_recusa` — efeito técnico | `recusa_motivo`, C identidade delegado | Nenhum DP; nota `recusa_materializacao` (`:1107`) e colisão existente | `materializacao.py:1089-1094`, `explicador.py:68`; LC. **DIVERGE**: não tratar colisão como DP `RECUSA` humana. |
| A19 `validacao.py:533 recusar_pergunta` — decisão | recusa, R/V delegado | DP `RECUSA` (`:593-594` → `disputa_emissao.py:1612`) | `validacao.py:573-584`, `explicador.py:68`, LR/LV. **DIVERGE**: idempotência depende R (`:576`), motivo projetado pode sair após leitor do ato. |
| A20 `validacao.py:625 conferir_data_do_fato` — decisão | `data_conferida_em/por` | **Nenhum DP**; LogAuditoria com dias/justificativa (`:667-672`) | `validacao.py:662`; `materializacao.py:1193`. **DIVERGE**: remover sem substituto factual trava materialização; DP `ATOS` não tem conferência (`models.py:3846-3856`). |
| A21 condicional `models.py:1968 remover_motivo` — decisão + metadados próprios | motivos/contexto; C delegado | DP `RETRATACAO` (`:2053-2057`) | `tarefa_do_dia.py:230-232` lê ato, mas `:227` ainda lê via; C6 `juizes.py:1334`. **DIVERGE**: muda no diff de E4 por identidade/terminalidade, portanto deve entrar na matriz mesmo fora das 20 alterações contadas. |

**Nascimento que a E4 obriga a mudar:** `PerguntaDisputa._nascer` (`app/chamados/models.py:2822`) inicializa projeções e faz costura/L no reuso; a remoção física de campos ou a proibição de escrevê-los precisa contemplar essa porta. Não é permitida exclusão por estar rotulada “fica”. Defaults dos campos também exigem revisão no modelo (`app/chamados/models.py:2444`, `:3366`).

**16 versus 26:** a soma reconstruída do censo é **16 primeiras alterações + quatro alterações adicionais =20; + seis nascimentos =26**. `remover_motivo` tem decisão delegada/metadados próprios e não integra os 20 campos derivados escritos diretamente listados. A comparação literal **16 → N** não mede o mesmo universo de **26** (censo fornecido **[nao verificado]** como arquivo local; definições verificadas nos endereços acima).

**N defendível:** alvo **N=0 portas que escrevem estado DERIVADO da pergunta**, lista residual vazia; conservar separadamente o escritor factual `DecisaoPergunta.registrar` (`models.py:3932`), efeitos Batida/Ausência/chamado e cache L declarado. Isso não significa zero escritores do sistema. Não é honesto declarar “16→4” somando três famílias de efeitos com um executor DP: seria troca da unidade de contagem. `DecisaoPergunta` declara explicitamente que as projeções são transitórias até E4 (`models.py:3832-3833`).

**Bloqueio real da cópia:** E3-A centraliza a consulta do app, mas não a desliga dos campos. `consulta_do_app:69` chama `perguntas_do_app`, que usa `q_vivas:49` e C6 `:57`; depois particiona por R/F (`:70`). C6 ainda lê V (`juizes.py:1323`), C/via (`:1334,1360,1366`) e R/texto (`:1339`). A seleção materializadora ainda exige R/V/M (`materializacao.py:1369-1373`). A própria tarefa usa R/V para detectar passivo (`tarefa_do_dia.py:346` → `models.py:3903`) e via para identidade (`tarefa_do_dia.py:227`). Logo, cortar somente escritores deixa leitores de produção dependentes dos campos; **a E4 exige troca coordenada desses leitores**, inclusive modelos proibidos, antes de ser fechada.

## 09/11 16:14 · sessao 756b2c3a17e7 · FIO-E0-V4-E4-ESCRITORES · censo · origem=Code

**O total comparável é 26 portas: 20 de alteração + 6 de nascimento, diferença de +4 sobre o número anunciado no v1.** As quatro adicionais já estavam descritas no v1: gravam pendência, recusa e conferência de data. Esses campos interferem no processamento da pergunta; não são apenas apresentação ou cache. (`app/chamados/services/materializacao.py:851`; `app/chamados/services/materializacao.py:1103`; `app/chamados/services/materializacao.py:1193`; `app/chamados/services/validacao.py:590`; `app/chamados/services/validacao.py:666`)

A conferência foi estática por AST, sem importar a aplicação, executar testes, acessar banco/log ou escrever arquivos. Identidade criptográfica independente da cópia, execução produtiva, despacho inteiramente dinâmico, SQL dinâmico e consumidores por reflexão: **[nao verificado]**. [nao verificado]

Para a soma, cada função com escrita semântica própria conta uma vez, mesmo quando também delega. `PerguntaDisputa.save` e `DecisaoPergunta.registrar` ficam discriminados como executores das portas: somá-los novamente duplicaria as persistências que executam. A sequência E1 possui efetivamente sua porta única de inserção. (`app/chamados/models.py:3402`; `app/chamados/models.py:3836`; `app/chamados/models.py:3932`)

**Delta dos escritores.** Nenhum dos escritores do v1 foi eliminado ou teve seus campos/classificação alterados pelo diff fornecido; as definições das 26 portas permanecem nas linhas apresentadas no CENSO. O acréscimo de quatro ao total corrige a soma do v1; não representa quatro portas criadas pelo diff. (`app/chamados/models.py:201`; `app/chamados/models.py:2822`; `app/chamados/services/disputa_emissao.py:1422`; `app/chamados/services/materializacao.py:784`; `app/chamados/services/materializacao.py:1083`; `app/chamados/services/validacao.py:533`; `app/chamados/services/validacao.py:625`)

Omissões antigas do inventário de escritores, acrescentadas abaixo como **fica**, sem aumentar o N de estado da pergunta:

| Escritor omitido | Escrita própria | Chamadores encontrados |
|---|---|---|
| `app/chamados/models.py:488 rebaixar` | Urgência/original/atualização | T: `app/chamados/tests/test_selo_e4_gates.py:73`; P direto: **[nao verificado]** |
| `app/chamados/models.py:513 escalar_urgencia` | Urgência/atualização | P: `app/ponto/services/triagem_batida.py:292`; `app/ponto/management/commands/processar_alertas_avancados.py:176` |
| `app/chamados/models.py:618 _rebaixar_sem_post_save` | Urgência/original/atualização | P: `app/chamados/signals.py:72` |
| `app/chamados/models.py:573 definir_prazo_sla` | Prazo/atualização | P: `app/chamados/services/cobranca.py:178`; `app/chamados/services/disputa_emissao.py:1715` |
| `app/chamados/models.py:594 marcar_escalonado_dp` | Recibo de escalada/atualização | P: `app/ponto/management/commands/escalonar_chamados_supervisao.py:103` |
| `app/chamados/models.py:2072 anotar_contexto` | Contexto da disputa | P: `app/chamados/views.py:1343`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159` |
| `app/chamados/models.py:2105 renovar_contexto` | Contexto da disputa | P: `app/ponto/management/commands/alertar_disputas_pendentes.py:48` |
| `app/chamados/services/disputa_emissao.py:257 perguntar_entradas_pendentes` | Contexto da disputa | P: `app/chamados/services/disputa_emissao.py:685`; `app/chamados/services/disputa_emissao.py:1267` |

Chamadores de teste dessas omissões, salvo os expressamente apontados: **[nao verificado]**. [nao verificado]

Também faltava a linha de escritor de `DecisaoPergunta.registrar`: preserva os atos E1 por `bulk_create`. Chamadores P incluem fechamento, retratação, absorção, resposta, validação, devolução e reconstrução; há testemunhas T específicas. (`app/chamados/models.py:3932`; `app/chamados/models.py:1921`; `app/chamados/models.py:2053`; `app/chamados/services/materializacao.py:563`; `app/chamados/services/validacao.py:139`; `app/chamados/services/validacao.py:205`; `app/chamados/services/disputa_emissao.py:1508`; `app/chamados/services/disputa_emissao.py:1612`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`; `app/chamados/tests/test_e1_volta2.py:84`)

**Delta dos leitores.** As referências seguintes são as linhas atuais do lado `+` dos respectivos hunks do diff fornecido:

| Leitor do v1 | Mudança efetiva |
|---|---|
| `app/api/views.py:991 api_chamados_v2` | A seleção crua passa a `consulta_do_app`; percorre pendentes + histórico e conserva somente disputas abertas nessa representação. Histórico respondido continua disponível para correção. (`app/api/views.py:1023`; `app/api/views.py:1058`; `app/api/views.py:1059`) |
| `app/chamados/services/aviso_pendencia.py:7 perguntas_pendentes_do_colab` | Deixa de montar seleção própria; retorna a lista central de pendentes. (`app/chamados/services/aviso_pendencia.py:9`) |
| `app/chamados/services/pergunta_no_app.py:60 consulta_do_app` | Passa a chamar `perguntas_do_app(..., em_lote=True)`; preserva a partição por resposta e fechamento. (`app/chamados/services/pergunta_no_app.py:69`; `app/chamados/services/pergunta_no_app.py:70`) |
| `app/chamados/services/tarefa_do_dia.py:396 CargaDaTarefa.tarefa` | Troca `alimentar_portas` por `alimentar_fila`: a carga obtém fila/verbos sem rejulgar C6. (`app/chamados/services/tarefa_do_dia.py:408`; `app/chamados/services/tarefa_do_dia.py:409`) |
| `app/chamados/services/cobranca.py:221 _calados` | Deslocamento de linha decorrente da mudança de `_pendentes`; mantém a leitura de respostas para a seleção dos calados. (`app/chamados/services/cobranca.py:229`) |
| Leitores posteriores em `juizes.py` | Deslocamentos pelas inserções/refatoração; campos e significado dos leitores já listados permanecem, com endereços atuais no CENSO. (`app/chamados/juizes.py:1322`; `app/chamados/juizes.py:1590`; `app/chamados/juizes.py:1765`; `app/chamados/juizes.py:2306`) |

Duas correções do v1 não são mudanças criadas pelo diff: `_estado_da_decisao` era o nome errado para `_pela_decisao`; a ocorrência de `data_conferida_por` na linha 666 é persistência, não consumidor. A definição de `atualizado_em` também não constitui leitor. (`app/chamados/services/tarefa_do_dia.py:115`; `app/chamados/services/validacao.py:665`; `app/chamados/services/validacao.py:666`; `app/chamados/models.py:68`)

Leitores novos no diff ou ausentes do inventário v1, com seus caminhos de chamada principais:

| Leitor | Campos/significado e origem | Chamadores |
|---|---|---|
| `app/chamados/juizes.py:1084 tranca_da_consulta` | Novo no diff; dia pelo juiz, resultado da tranca e memo restrito à consulta; apenas memória | P: `app/chamados/services/pergunta_no_app.py:56`; T indireto: `app/chamados/tests/test_e3_v4_orm.py:108` |
| `app/chamados/juizes.py:1450 alimentar_fila` | Novo no diff; fila/célula/verbos do painel | P: `app/chamados/juizes.py:1495`; `app/chamados/services/tarefa_do_dia.py:409`; T: `app/chamados/tests/test_e3_portas_tarefa.py:119` |
| `app/chamados/services/pergunta_no_app.py:44 perguntas_do_app` | Omitido no v1; R/V/C por `q_vivas` + C6; o diff acrescenta o contexto de consulta | P: `app/chamados/services/pergunta_no_app.py:69`; `app/chamados/juizes.py:1519`; T: `app/chamados/tests/test_n1_perguntas_do_app.py:94` |
| `app/chamados/services/cobranca.py:45 _pendentes` | Omitido no v1; agora lê a mesma partição central | P: `app/chamados/services/cobranca.py:82`; `app/chamados/services/cobranca.py:163`; T: `app/chamados/tests/test_e3_v4_orm.py:31` |
| `app/chamados/services/tarefa_do_dia.py:418 _portas_da_tarefa` | Omitido no v1; ações exigíveis + fila/verbos produzem rota/fila/verbos/fio | P: `app/chamados/services/tarefa_do_dia.py:412`; T indireto: `app/chamados/tests/test_e3_portas_tarefa.py:121` |
| `app/chamados/services/painel.py:50 marcador_do_placar` | Omitido no v1; instantes de resolução/R/V/M invalidam apresentação | P: `app/chamados/services/painel.py:91`; T: `app/chamados/tests/test_selo_cadeia_placar.py:76` |
| `app/chamados/models.py:336 _ancoradas` | Omitido no v1; destino e validação das perguntas ancoradas | P: `app/chamados/models.py:223` |
| `app/chamados/models.py:374 _plano_pergunta_viva` | Omitido no v1; identidade/dia no plano de vivacidade | P: `app/chamados/models.py:419` |
| `app/chamados/models.py:983 _renascer_por_premissa_viva` | Omitido no v1; via anterior do chamado | P: `app/chamados/models.py:922`; `app/chamados/services/resposta_tardia.py:45` |
| `app/chamados/models.py:2626 trava_na_ordem` | Omitido no v1; fechamento/status durante aquisição das travas | P: `app/chamados/models.py:290` |
| `app/chamados/models.py:2694 do_fato`; `app/chamados/models.py:2743 q_dos_fatos` | Omitidos no v1; conversa/via/dia/instância delimitam identidade do fato | P: `app/chamados/models.py:2770`; `app/chamados/services/fabrica_da_celula.py:124` |
| `app/chamados/models.py:3039 fechar_por_admin` | Omitido no v1; via/M para idempotência do encerramento | P: `app/chamados/models.py:1918`; `app/ponto/services/veredito_celula.py:189` |
| `app/chamados/juizes.py:339 _slot_esperado` | Omitido no v1; resposta usada na identificação do slot | P: `app/chamados/juizes.py:732`; T: `app/chamados/tests/test_marco_por_motivo.py:67` |
| `app/chamados/juizes.py:604 _celulas_da_pergunta` | Omitido no v1; dia gravado e células pertinentes | P: `app/chamados/juizes.py:680`; `app/chamados/juizes.py:1392` |
| `app/chamados/juizes.py:2148 resolvedora_valida` | Omitido no v1; discriminador conversa e dia pelo juiz | P: `app/chamados/services/fio.py:907`; T: `app/chamados/tests/test_e1_v15.py:110` |
| `app/chamados/services/fio.py:929 _colisao_ao_datar`; `app/chamados/services/fio.py:1047 rechavear_no_reuso` | Omitidos no v1; dia/identidade no reuso | P: `app/chamados/services/fio.py:895`; `app/chamados/models.py:2949`; T: `app/chamados/tests/test_e1_v15f.py:408` |
| `app/chamados/services/fio.py:1110 violacao_de_saida` | Omitido no v1; dia e contrato de saída da identidade | P: `app/chamados/services/fio.py:1137`; `app/chamados/services/fio.py:1169` |
| `app/chamados/services/fio.py:1547 recusa_conversa` | Omitido no v1; conversa não recebe identidade de fato | P: `app/chamados/models.py:2769`; `app/chamados/services/fio.py:875` |
| `app/chamados/services/fio.py:1869 perguntas_sobre_batida_retratada`; `app/chamados/services/fio.py:1987 fora_do_indice` | Omitidos no v1; conversa/via/dia em diagnósticos de identidade | P: `app/chamados/management/commands/backfill_decisoes_pergunta.py:545`; `app/chamados/management/commands/backfill_decisoes_pergunta.py:486`; T: `app/chamados/tests/test_e1_v15c.py:93`; `app/chamados/tests/test_e1_v6_horizonte.py:138` |
| `app/chamados/management/commands/diff_e3_sombra.py:42 candidata`; `app/chamados/management/commands/diff_e3_sombra.py:47 projecoes`; `app/chamados/management/commands/diff_e3_sombra.py:61 casos_do_par`; `app/chamados/management/commands/diff_e3_sombra.py:108 Command.comparar` | Omitidos no v1; comparação instrumental de C17/tarefa por campos, chamador e caso | P operacional: `app/chamados/management/commands/diff_e3_sombra.py:100`; `app/chamados/management/commands/diff_e3_sombra.py:126`; `app/chamados/management/commands/diff_e3_sombra.py:128`; T: `app/chamados/tests/test_e3_diff_sombra.py:20`; `app/chamados/tests/test_e3_diff_sombra.py:135` |

Testemunhas T dos demais leitores omitidos não indicadas na tabela: **[nao verificado]**. O instrumento está declarado fora do pipeline; isso não demonstra que tenha sido executado. (`app/config/crons.py:771`)

**D1–D15 não caíram.** D2 incorpora a partição pendentes/histórico e a retirada de C6 da alimentação da tarefa; D9 incorpora a limitação explícita de `--agora`; D11 precisa corrigir a atribuição do ramo: `reabrir_por_colab` recusa disputa fechada antes de delegar, enquanto `reabrir_para_correcao` é a porta que limpa respostas sem escrever F. (`app/chamados/services/pergunta_no_app.py:70`; `app/chamados/services/tarefa_do_dia.py:408`; `app/chamados/management/commands/diff_e3_sombra.py:105`; `app/chamados/models.py:2221`; `app/chamados/services/disputa_emissao.py:1559`)

As novas distinções D16–D17 constam integralmente abaixo. Unidade do app e destino de C17 já receberam cortes; não abrem pergunta de lei nova. A troca de C17 continua condicionada ao DIFF, e seus três chamadores diretos permanecem os mesmos. (`app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`; `app/folha/fora_da_folha.py:67`; `app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`)

Leis/cortes checados: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4; LEI-TRANCA-E2-JUIZ; LEI-E3-UNIDADE-E-C17; LEI-C17-UM-JUIZ. **Perguntas de lei: 0.** (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`; `app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`)

Abreviações do inventário: **C** = `materializada_em/por`, `via_resolucao`; **R** = `resposta_colab`, `respondida_em`; **V** = `validada_em/por`; **L** = `veredito_lavrado`, `julgado_em`, `janela_expira_em`; **F** = `fechada_em/por`, `resolucao`, `motivo_fechamento`; **DP** = sequência `DecisaoPergunta`. (`app/chamados/models.py:3115`; `app/chamados/models.py:2444`; `app/chamados/models.py:3366`; `app/chamados/veredito_lavrado.py:250`; `app/chamados/models.py:1929`; `app/chamados/models.py:3828`)

## CENSO

escritores:

- app/chamados/models.py:201 ChamadoColaborador._gravar_estado -- `status_local`, `resolvido_em`, `via_encerramento`, `respondido_por`, urgência/original, atualização; relavra perguntas atingidas -- efeito nos derivados; decisao na transição humana; fica atualização.
- app/chamados/models.py:1852 DisputaSupervisao.fechar -- F; encerramento excepcional delega carimbo das perguntas e registra DP `SILENCIO` -- efeito no fechamento normal; decisao nas exceções.
- app/chamados/models.py:2123 DisputaSupervisao.reabrir_admin -- limpa F, incrementa contador e pode agregar motivos -- decisao; fica contador/metadados.
- app/chamados/models.py:2160 DisputaSupervisao.reabrir_sistema -- limpa F -- efeito.
- app/chamados/models.py:3092 PerguntaDisputa.carimbar -- C; pode completar `respondida_em`, V e L -- efeito no carimbo; decisao nos ramos de assinatura; fica L.
- app/chamados/models.py:3157 PerguntaDisputa.carimbar_identidade -- C, preservação opcional de instante/autor, `recusa_motivo` opcional -- efeito; decisao humana determinada pelo chamador.
- app/chamados/models.py:3192 PerguntaDisputa.descarimbar -- limpa C; `salvar=False` prepara memória -- efeito.
- app/chamados/models.py:3241 PerguntaDisputa.descarimbar_em_lote -- limpa C e grava campos recebidos em `outros` -- efeito.
- app/chamados/models.py:3257 PerguntaDisputa.encerrar_por_fato_no_espelho -- `via_resolucao=fato_materializado` -- efeito.
- app/chamados/services/validacao.py:127 _carimbar_validacao -- V e DP `VALIDACAO` -- decisao.
- app/chamados/services/validacao.py:163 devolver_validacao_recusada -- limpa V, preserva resposta; DP `DEVOLUCAO` -- decisao.
- app/chamados/services/disputa_emissao.py:1422 responder_pergunta_disputa_core -- R, limpa V; pode limpar C e registrar recusa explicativa; DP `RESPOSTA` -- decisao.
- app/chamados/services/disputa_emissao.py:1559 reabrir_para_correcao -- limpa R/V e opcionalmente C; DP com foto anterior `REABERTURA` ou `RECUSA` -- decisao.
- app/chamados/services/disputa_emissao.py:1635 responder_automaticamente -- R sem sobrescrever texto existente; DP `RESPOSTA` -- decisao de sistema.
- app/chamados/services/disputa_emissao.py:1658 carimbar_resposta_pela_trilha -- `respondida_em` no instante da testemunha; DP `RESPOSTA` -- decisao reconstruída.
- app/chamados/services/relavra_via.py:106 relavrar -- via derivada e L no mesmo save -- efeito na via; fica L.
- app/chamados/services/materializacao.py:784 _materializar_resposta_hora -- `pendencia_admin`, `recusa_motivo`; plantio/carimbos delegados -- efeito.
- app/chamados/services/materializacao.py:1083 _carimbar_recusa -- `recusa_motivo`; colisão delega C de identidade -- efeito de recusa técnica.
- app/chamados/services/validacao.py:533 recusar_pergunta -- `recusa_motivo`; delega limpeza R/V e DP `RECUSA` -- decisao.
- app/chamados/services/validacao.py:625 conferir_data_do_fato -- `data_conferida_em/por`, com justificativa e dois dias na trilha -- decisao.
- app/chamados/models.py:833 ChamadoColaborador.abrir -- identidade/contexto e status inicial pelo motor -- fica nascimento.
- app/chamados/models.py:1054 ChamadoColaborador.criar -- identidade/contexto e status inicial pelo motor -- fica nascimento.
- app/chamados/services/disputa_emissao.py:474 _abrir_ou_agregar_disputa -- nascimento aberto, motivos/contexto -- fica.
- app/chamados/services/disputa_emissao.py:1024 criar_disputa_a_partir_de_chamados_orfaos -- nascimento aberto, identidade, motivos/contexto -- fica.
- app/chamados/services/disputa_emissao.py:1299 criar_disputa_ancora_de_chamado -- nascimento aberto, vínculo com chamado, motivos/contexto -- fica.
- app/chamados/models.py:2822 PerguntaDisputa._nascer -- identidade, dia, instância, célula, destino, conteúdo/tipo/opções, defaults R/V/C; costura e L no reuso -- fica.
- app/chamados/veredito_lavrado.py:240 gravar_lavras_em_lote -- L por `bulk_update` -- fica cache.
- app/chamados/veredito_lavrado.py:254 lavrar -- L; `salvar=False` prepara memória -- fica cache.
- app/chamados/services/arquivo.py:141 silenciar -- `silenciado_ate/por/motivo`, atualização -- fica prazo/metadados de apresentação.
- app/chamados/services/arquivo.py:177 levantar -- limpa silêncio; atualização e primeiro `resgatado_em` -- fica.
- app/chamados/services/fio.py:804 reapontar_resolvedora -- destino, célula/dia quando aplicável, L -- fica identidade/cache.
- app/chamados/services/fio.py:970 rechavear_data_evento -- dia/célula e L -- fica identidade/data/cache.
- app/chamados/models.py:1968 DisputaSupervisao.remover_motivo -- motivos/contexto próprios; DP `RETRATACAO` e C delegados -- decisao; fica metadados.
- app/chamados/models.py:1949 DisputaSupervisao.aparar_motivo_respondido -- motivos/contexto; não apaga pergunta -- fica.
- app/chamados/models.py:2179 DisputaSupervisao.reabrir_por_colab -- contador próprio; reabertura R/V delegada -- fica contador; decisao delegada.
- app/chamados/models.py:2247 DisputaSupervisao.limpar_resolucao_residual -- `resolucao=''` em disputa aberta -- fica.
- app/chamados/models.py:488 ChamadoColaborador.rebaixar -- urgência/original, atualização -- fica prioridade.
- app/chamados/models.py:513 ChamadoColaborador.escalar_urgencia -- urgência, atualização -- fica prioridade.
- app/chamados/models.py:618 ChamadoColaborador._rebaixar_sem_post_save -- urgência/original, atualização por update -- fica prioridade.
- app/chamados/models.py:573 ChamadoColaborador.definir_prazo_sla -- prazo SLA, atualização -- fica prazo.
- app/chamados/models.py:594 ChamadoColaborador.marcar_escalonado_dp -- recibo/instante da escalada, atualização -- fica.
- app/chamados/models.py:2072 DisputaSupervisao.anotar_contexto -- contexto -- fica.
- app/chamados/models.py:2105 DisputaSupervisao.renovar_contexto -- contexto -- fica.
- app/chamados/services/disputa_emissao.py:257 perguntar_entradas_pendentes -- contexto da disputa; emissão delegada -- fica na escrita própria.
- app/chamados/services/contexto.py:62 anotar_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/contexto.py:82 renovar_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/contexto.py:108 retirar_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/contexto.py:131 fundir_contexto -- `contexto_json`, atualização -- fica.
- app/chamados/services/vinculos.py:81 apontar_justificativa -- 

[truncado em 20000 de 67950 caracteres]

## 09/11 16:07 · sessao 7e56db703d9c · - · - · origem=Ronald (tmux)

Fecho dos chamadores por AST, sem importar a aplicação:

| Leitor | Campos/significado | Chamadores |
|---|---|---|
| `app/chamados/juizes.py:1084 tranca_da_consulta` | Dia do turno pelo juiz, resultado da tranca e memo restrito à consulta; memória, sem persistência. | P: `app/chamados/services/pergunta_no_app.py:56`. T indireto: `app/chamados/tests/test_e3_v4_orm.py:108`. |
| `app/chamados/juizes.py:1450 alimentar_fila` | Fila, célula e verbos do painel; leitura indireta de status/silêncio/urgência. | P: `app/chamados/juizes.py:1495`; `app/chamados/services/tarefa_do_dia.py:409`. T: `app/chamados/tests/test_e3_portas_tarefa.py:119`. |
| `app/chamados/services/tarefa_do_dia.py:396 CargaDaTarefa.tarefa` | Perguntas, decisões, efeitos, passivo, lavra e tranca carregados; agora alimenta painel sem chamar C6. | P: `app/chamados/services/tarefa_do_dia.py:452`. T: `app/chamados/tests/test_e3_portas_tarefa.py:121`. |
| `app/chamados/services/tarefa_do_dia.py:418 _portas_da_tarefa` | Ações exigíveis, fila e verbos; publica rota/fila/verbos/fio. | P: `app/chamados/services/tarefa_do_dia.py:412`. T indireto: `app/chamados/tests/test_e3_portas_tarefa.py:121`. |
| `app/chamados/management/commands/diff_e3_sombra.py:42 candidata` | Projeta rota/fila/verbos/fio da tarefa. | P operacional: mesmo arquivo `:126`. T: `app/chamados/tests/test_e3_diff_sombra.py:135`. |
| `app/chamados/management/commands/diff_e3_sombra.py:47 projecoes` | Campos consumidos pelos três leitores C17. | P operacional: mesmo arquivo `:118`, `:127`. T: `app/chamados/tests/test_e3_diff_sombra.py:141`. |
| `app/chamados/management/commands/diff_e3_sombra.py:61 casos_do_par` | Fila, perguntas vivas, âncoras e razão da tarefa; classifica casos do instrumento. | P operacional: mesmo arquivo `:128`. T direto: `[nao verificado]`. |
| `app/chamados/management/commands/diff_e3_sombra.py:108 Command.comparar` | Compara C17 e tarefa por colaborador/dia; utiliza helpers anteriores. | P operacional: mesmo arquivo `:99`. T indireto: `app/chamados/tests/test_e3_diff_sombra.py:20`. |

`diff_e3_sombra` é instrumento declarado fora do pipeline; declaração não demonstra execução. (`app/config/crons.py:771`) Execução produtiva de todos os chamadores acima: **[nao verificado]**.

Correções indispensáveis do v1: `_estado_da_decisao` não existe no AST atual; o corpo correspondente é `_pela_decisao`, em `app/chamados/services/tarefa_do_dia.py:115`. O diff fornecido não demonstra renomeação desse corpo: **[nao verificado]** quanto à origem do erro do v1. A referência `data_conferida_por` em `app/chamados/services/validacao.py:666` é atribuição, não leitura; a guarda lê `data_conferida_em` em `:662`. `_resolv` existe em `app/chamados/services/becos.py:21`; `universo_da_linha` existe em `app/chamados/services/fio.py:1906`.

## 09/11 16:07 · sessao 5bf5272c2c2f · - · - · origem=Ronald (tmux)

A conferência AST manteve as 16 portas históricas e os seis nascimentos nas mesmas linhas do v1. Nenhum arquivo desses escritores é alterado pelo diff fornecido. A soma **22 é incompleta para a unidade declarada**: quatro gravadores adicionais de estado já constavam da lista do v1, mas ficaram fora da soma; o total de projeção é **20 alterações + 6 nascimentos = 26, diferença +4**, sem escritor criado pelo diff. (`app/chamados/services/materializacao.py:851`; `app/chamados/services/materializacao.py:1103`; `app/chamados/services/validacao.py:590`; `app/chamados/services/validacao.py:666`)

As quatro portas adicionais afetam ação: pendência do admin muda a explicação antes do encerramento; recusa mantém aplicação pendente ou terminal conforme a causa; conferência de data libera plantio bloqueado por divergência. (`app/chamados/services/explicador.py:84`; `app/chamados/services/materializacao.py:1083`; `app/chamados/services/materializacao.py:1193`)

Inventário das 26 portas:

- `app/chamados/models.py:201` `ChamadoColaborador._gravar_estado` — status/resolução/via/autor/urgência/atualização — efeito|decisao.
- `app/chamados/models.py:1852` `DisputaSupervisao.fechar` — F; delega carimbo excepcional — efeito|decisao.
- `app/chamados/models.py:2123` `DisputaSupervisao.reabrir_admin` — F, contador, motivos — decisao|fica.
- `app/chamados/models.py:2160` `DisputaSupervisao.reabrir_sistema` — F — efeito.
- `app/chamados/models.py:3092` `PerguntaDisputa.carimbar` — C; condicionalmente R/V/L — efeito|decisao|fica.
- `app/chamados/models.py:3157` `PerguntaDisputa.carimbar_identidade` — C, recusa opcional — efeito; decisão vem do chamador.
- `app/chamados/models.py:3192` `PerguntaDisputa.descarimbar` — C nulo — efeito.
- `app/chamados/models.py:3241` `PerguntaDisputa.descarimbar_em_lote` — C nulo, `outros` — efeito.
- `app/chamados/models.py:3257` `PerguntaDisputa.encerrar_por_fato_no_espelho` — via — efeito.
- `app/chamados/services/validacao.py:127` `_carimbar_validacao` — V — decisao.
- `app/chamados/services/validacao.py:163` `devolver_validacao_recusada` — V nulo — decisao.
- `app/chamados/services/disputa_emissao.py:1422` `responder_pergunta_disputa_core` — R, limpa V, recusa e C condicionais — decisao.
- `app/chamados/services/disputa_emissao.py:1559` `reabrir_para_correcao` — limpa R/V, C opcional — decisao.
- `app/chamados/services/disputa_emissao.py:1635` `responder_automaticamente` — R — decisao.
- `app/chamados/services/disputa_emissao.py:1658` `carimbar_resposta_pela_trilha` — instante original da resposta — decisao.
- `app/chamados/services/relavra_via.py:106` `relavrar` — via/L — efeito|fica.
- `app/chamados/services/materializacao.py:784` `_materializar_resposta_hora` — pendência/recusa — efeito.
- `app/chamados/services/materializacao.py:1083` `_carimbar_recusa` — recusa; delega C na colisão — efeito.
- `app/chamados/services/validacao.py:533` `recusar_pergunta` — recusa; delega R/V — decisao.
- `app/chamados/services/validacao.py:625` `conferir_data_do_fato` — data conferida/autor — decisao.
- `app/chamados/models.py:833` `ChamadoColaborador.abrir` — identidade/contexto/status inicial — fica.
- `app/chamados/models.py:1054` `ChamadoColaborador.criar` — identidade/contexto/status inicial — fica.
- `app/chamados/services/disputa_emissao.py:474` `_abrir_ou_agregar_disputa` — nascimento da Disputa — fica.
- `app/chamados/services/disputa_emissao.py:1024` `criar_disputa_a_partir_de_chamados_orfaos` — nascimento da Disputa — fica.
- `app/chamados/services/disputa_emissao.py:1299` `criar_disputa_ancora_de_chamado` — nascimento da Disputa — fica.
- `app/chamados/models.py:2822` `PerguntaDisputa._nascer` — identidade/defaults R/V/C; cache no reuso — fica.

Demais escritores do v1 permanecem:

- `app/chamados/veredito_lavrado.py:240` `gravar_lavras_em_lote` — L — fica/cache.
- `app/chamados/veredito_lavrado.py:254` `lavrar` — L — fica/cache.
- `app/chamados/services/arquivo.py:141` `silenciar` — silêncio/prazo/autoria/motivo/atualização — fica.
- `app/chamados/services/arquivo.py:177` `levantar` — silêncio/resgate/atualização — fica.
- `app/chamados/services/fio.py:804` `reapontar_resolvedora` — destino/célula/dia/L — fica.
- `app/chamados/services/fio.py:970` `rechavear_data_evento` — dia/célula/L — fica.
- `app/chamados/models.py:1968` `DisputaSupervisao.remover_motivo` — próprios motivos/contexto; decisão e carimbo delegados — decisao|fica.
- `app/chamados/models.py:1949` `DisputaSupervisao.aparar_motivo_respondido` — motivos/contexto — fica.
- `app/chamados/models.py:2179` `DisputaSupervisao.reabrir_por_colab` — contador; reabertura delegada — fica|decisao.
- `app/chamados/models.py:2247` `DisputaSupervisao.limpar_resolucao_residual` — texto residual — fica.
- `app/chamados/models.py:3402` `PerguntaDisputa.save` — executor genérico — fica.
- `app/chamados/models.py:563` `ChamadoColaborador.apagar_em_ensaio` — exclusão restrita a ensaio — fica.
- `app/chamados/models.py:349` `_vivas_entre`; `:1468` `_universo_da_ressuscitacao`; `:3086` `_escrever_carimbo`; `:3148` `_desfazer_em_memoria` — memória/simulação, sem persistência própria — fica.

Omissão da lista de escritores do v1: `app/chamados/models.py:3932` `DecisaoPergunta.registrar`, `bulk_create` em `:3934`, campos do ato, autoria, instante, conteúdo, motivo, foto anterior e ligação de absorção — **decisao**. É a persistência canônica da sequência, sem ato novo independente: apresentá-la separadamente das 26 portas da projeção evita somar novamente todas as decisões que já a chamam. Seu executor `DecisaoPergunta.save` fica em `app/chamados/models.py:3892`.

Chamadores produtivos da sequência:

- `app/chamados/models.py:1921` `fechar`: SILENCIO.
- `app/chamados/models.py:2053` `remover_motivo`: RETRATACAO.
- `app/chamados/services/materializacao.py:563`: ABSORCAO.
- `app/chamados/services/validacao.py:139`, `:205`.
- `app/chamados/services/disputa_emissao.py:1508`, `:1612`, `:1653`, `:1674`.
- `app/ponto/services/veredito_celula.py:194`: SILENCIO.
- `app/chamados/management/commands/backfill_decisoes_pergunta.py:324`: reconstrução operacional.
- Testes exemplos: `app/chamados/tests/test_e2_tarefa_do_dia.py:120`, `:757`, `:779`; `app/chamados/tests/test_e1_volta2.py:84`, `:88`.

Omissões antigas de metadados/conteiner, fora do N literal de estado da pergunta:

- `app/chamados/models.py:488` `rebaixar`, save `:498`: urgência/original/atualização.
- `app/chamados/models.py:513` `escalar_urgencia`, save `:553`: urgência/atualização.
- `app/chamados/models.py:618` `_rebaixar_sem_post_save`, update `:636`: urgência/original/atualização.
- `app/chamados/models.py:573` `definir_prazo_sla`, save `:591`: prazo/atualização.
- `app/chamados/models.py:594` `marcar_escalonado_dp`, save `:610`: recibo do push/atualização.
- `app/chamados/models.py:2072` `anotar_contexto`, save `:2102`.
- `app/chamados/models.py:2105` `renovar_contexto`, save `:2120`.
- `app/chamados/services/disputa_emissao.py:257` `perguntar_entradas_pendentes`, save `:296`: contexto da Disputa.
- `app/chamados/services/contexto.py:62`, `:82`, `:108`, `:131`: `anotar_contexto`, `renovar_contexto`, `retirar_contexto`, `fundir_contexto`; contexto_json/atualização.
- `app/chamados/services/vinculos.py:81`, `:93`, `:105`, `:121`, `:139`: justificativa/célula/ticket/responsável.

Chamadores das principais omissões: `_rebaixar_sem_post_save` é chamado por `app/chamados/signals.py:72`; escalada por `app/ponto/services/triagem_batida.py:292`, `:369`, `app/ponto/management/commands/escalonar_chamados_supervisao.py:132`, `app/ponto/management/commands/processar_alertas_avancados.py:176`; prazos por `app/chamados/services/cobranca.py:178` e `app/chamados/services/disputa_emissao.py:1715`; recibo DP por `app/ponto/management/commands/escalonar_chamados_supervisao.py:103`; anotar contexto da Disputa por `app/chamados/views.py:1343` e `app/ponto/management/commands/escalonar_chamados_supervisao.py:159`; renovar por `app/ponto/management/commands/alertar_disputas_pendentes.py:48`; entradas pendentes por `app/chamados/services/disputa_emissao.py:685`, `:1267`. `rebaixar` teve somente chamadores diretos de teste localizados: `app/chamados/tests/test_selo_e4_gates.py:73`, `:84`, `:89`; chamada dinâmica produtiva [nao verificado].

## 09/11 16:05 · sessao 6ed3e3a2c318 · - · - · origem=Ronald (tmux)

D1–D15 permanecem; o diff não resolve nem elimina as distinções de estado já catalogadas. D2, D9 e D11 precisam incorporar os novos leitores e os limites explícitos abaixo. (`app/chamados/models.py:3023`; `app/chamados/models.py:3092`; `app/chamados/juizes.py:1322`; `app/chamados/services/pergunta_no_app.py:60`; `app/chamados/management/commands/diff_e3_sombra.py:105`)

- **D2 ampliada:** pré-filtro SQL, julgamento C6, partição de apresentação e tarefa são operações diferentes. `consulta_do_app` particiona as C6 vivas: pendente exige `respondida_em is None` **e** disputa aberta; demais vivas vão ao histórico. A carga da tarefa agora chama `alimentar_fila`, sem executar novamente C6. (`app/chamados/models.py:3325`; `app/chamados/services/pergunta_no_app.py:69`; `app/chamados/services/tarefa_do_dia.py:407`; `app/chamados/juizes.py:1450`)
- **D9 ampliada:** janela C6 continua inclusiva e usa `date.today()`; expiração da lavra usa dia local do `agora` recebido, com `janela_expira_em <= hoje_local`. O instrumento passou a declarar que `--agora` não controla aquele relógio C6. Divergência efetiva em produção: **[nao verificado]**. (`app/chamados/juizes.py:575`; `app/chamados/juizes.py:601`; `app/chamados/veredito_lavrado.py:104`; `app/chamados/veredito_lavrado.py:116`; `app/chamados/management/commands/diff_e3_sombra.py:105`)
- **D11 ampliada:** disputa fechada continua sem demonstrar ausência de tarefa. A alteração põe pergunta viva de disputa fechada no histórico do app; o teste mantém a aceitação pelo POST. Atenção: `reabrir_por_colab` recusa disputa fechada antes de delegar a correção; a afirmação ampla do v1 sobre correção sem reabrir o continente deve referir-se à porta `reabrir_para_correcao`, não ao wrapper de colaborador. (`app/chamados/services/pergunta_no_app.py:70`; `app/api/tests/test_hx_api_estado.py:54`; `app/chamados/models.py:2221`; `app/chamados/models.py:2234`; `app/chamados/services/disputa_emissao.py:1559`)

Duas decisões adicionais justificadas, omitidas no v1:

- **D16 — “No app” e “pendente no app” têm universos diferentes.** `perguntas_do_app` devolve todas as C6 vivas; `consulta_do_app` extrai pendentes de disputa aberta e mantém as restantes no histórico. O leitor C17 `_no_app` continua usando a coleção completa de `perguntas_do_app`. Isso já tem corte de unidade; não é pergunta de lei nova. (`app/chamados/services/pergunta_no_app.py:44`; `app/chamados/services/pergunta_no_app.py:60`; `app/chamados/juizes.py:1509`; `app/chamados/juizes.py:1519`; `app/docs/PROMPTS.md:2002`)
- **D17 — `rota/fila/verbos/fio` de C17 e tarefa não prometem a mesma coisa.** C17 parte da alcançabilidade pela fila e pelas perguntas C6; a tarefa parte das ações exigíveis e publica somente suas portas. `fio` C17 pode ser `chamado`; o da tarefa também exprime `sistema` e `nao_sei`. O corte já escolheu a resposta da tarefa e condicionou a troca dos três chamadores ao DIFF. (`app/chamados/juizes.py:1532`; `app/chamados/juizes.py:1575`; `app/chamados/services/tarefa_do_dia.py:425`; `app/chamados/services/tarefa_do_dia.py:431`; `app/docs/PROMPTS.md:2007`)

Correção de inventário do v1: **`_estado_da_decisao` não existe nesta cópia**. As referências v1 de passivo e validação pertencem a `_pela_decisao`, definida na linha 115; passivo na 127 e validação na 140. O diff fornecido não renomeia essa função. (`app/chamados/services/tarefa_do_dia.py:115`; `app/chamados/services/tarefa_do_dia.py:127`; `app/chamados/services/tarefa_do_dia.py:140`)

Chamadores produtivos C17 confirmados pelo scanner AST extraído sem importar a aplicação: `vigia_fio.censo_alcance`, `cadastro_realidade.beco_em_lote`, `fora_da_folha._porta`. O instrumento chama `_porta` existente; não cria quarto chamador direto. (`app/chamados/services/vigia_fio.py:635`; `app/escala/services/cadastro_realidade.py:488`; `app/folha/fora_da_folha.py:67`; `app/chamados/management/commands/diff_e3_sombra.py:124`)

**Perguntas de lei: 0.** O v1 não tinha perguntas abertas; unidade do app e destino de C17 já têm respostas literais, inclusive a emenda posterior que condiciona a fachada ao DIFF. (`app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`)

Leis/cortes checados: L-002, L-007, L-051, L-061, L-073, L-080, L-085, L-088, L-110, L-111, L-113, L-117 e L-124; FIO-E0-V4; LEI-TRANCA-E2-JUIZ; LEI-E3-UNIDADE-E-C17; LEI-C17-UM-JUIZ. (`app/docs/LEIS.md:60`; `app/docs/LEIS.md:65`; `app/docs/LEIS.md:92`; `app/docs/LEIS.md:95`; `app/docs/LEIS.md:100`; `app/docs/LEIS.md:107`; `app/docs/LEIS.md:117`; `app/docs/LEIS.md:120`; `app/docs/LEIS.md:136`; `app/docs/LEIS.md:137`; `app/docs/LEIS.md:151`; `app/docs/LEIS.md:138`; `app/docs/LEIS.md:145`; `app/docs/CORTES.md:426`; `app/docs/CORTES.md:427`; `app/docs/PROMPTS.md:2002`; `app/docs/PROMPTS.md:2007`)

## 09/11 15:48 · sessao 43a71806087d · BUG-PAR-DO-INTERVALO-EM-DOIS-ATOS · construtor · origem=Code

Cura escrita para a unidade **pergunta + testemunha-batida**, com A factual, escrita apenas da pergunta, ordem por instante e paridade final. [materializacao.py:72](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/chamados/services/materializacao.py:72)

Foram escritos 13 testes novos, reforçada a fixture `sem_par` e acrescentados M41–M50. O M41 recoloca a cegueira à batida contraparte e aponta para o teste (a). [Testes e mutantes](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA_V2.r:117) [nao verificado]

A entrega contém todos os chamadores, efeitos nos leitores, endereço de cada hunk e **comandos individuais**. [Entrega v2](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA_V2.r:1)

Somente verificações estáticas foram executadas. **RED/GREEN, mortes, concorrência e efeitos em produção: [nao verificado].** C7/fechar permanecem para os hunks da sessão. [Pendências](/home/ronald/wt-codex-bug-par-do-intervalo-em-dois-atos-144706/app/docs/BUG_PAR_ENTREGA_V2.r:110)

## 09/11 15:47 · sessao 6b3547444afa · - · - · origem=Ronald (tmux)

Não encontrei falha concreta no diff que impeça os testes positivos ou inclua a batida no universo de escrita. Revisão estática, execução [nao verificado]. [nao verificado]

Conferi:

- Identidade usa o DNA do dia, alinhamento existente e `batidas_apuraveis`; exige uma candidata: `chamados/services/materializacao.py:113`.
- `perguntas_do_par` separa seleção/escritas da testemunha: `chamados/services/materializacao.py:67`.
- Projeção da testemunha é A e seus IDs não entram em `par_ids`: `chamados/services/materializacao.py:570`, `:634`.
- Ordem compara instantes antes da escrita e instantes reais depois; paridade examina ambas as metades: `chamados/services/materializacao.py:1443`, `:1457`, `:1467`.
- ON distingue A factual da testemunha de A da pergunta; replay de aplicação retorna antes de decisões/escritas: `chamados/services/materializacao.py:1537`, `:1542`.
- Casca aceita uma pergunta, template não cria entrada horária da testemunha: `chamados/services/acoes_disputa.py:23`, `templates/chamados/partials/_par_intervalo.html:37`.
- Testes positivos exigem uma chamada à porta, ordem integral E–S–E–S e preservação da batida existente: `chamados/tests/test_bug_par_unidade.py:428`, `:456`.
- Testes de universo e O330 trabalham somente com a pergunta: `chamados/tests/test_bug_par_unidade.py:622`, `:634`.

Nenhum arquivo editado por este explorer; nenhum runtime executado [nao verificado].

## 09/11 15:46 · sessao 5275290d9dd3 · FIO-E0-V4-E3 · verificador · origem=Code

**PROCEDENTE adicional no TOTAL:** `tarefa_do_dia` ainda pode alcançar escrita indireta: carga → tranca → toque → `get_user_sistema()` → `User.objects.get_or_create`. Com tranca vigente e usuário ausente, a leitura pode criar usuário. Cura: leitura estrita do usuário, com erro se ausente, e teste que proíba criação nesse cenário. (`app/chamados/services/tarefa_do_dia.py:370`, `app/chamados/juizes.py:1173`, `app/core/utils.py:18`)

As leis literais, triagem v3b, delta e entregas citadas não foram localizados na cópia; sua conferência literal e a identificação exata de F1/F2 ficam **[nao verificado]**. Abaixo, caminhos relativos a `app/`.

1. **P1–P5: cura e mordida**

| Item | Classificação e sítio | Teste que morde; passaria sem cura? | [nao verificado]
|---|---|---|
| P1 | **NAO PROCEDE no sítio corrigido.** Aviso e cobrança delegam à mesma lista central. `chamados/services/aviso_pendencia.py:9`, `chamados/services/cobranca.py:48`. | Fechada viva + aberta, e somente fechada: `chamados/tests/test_e3_v4_orm.py:20`, `:37`. **Não:** o filtro anterior incluiria a fechada. |
| P2 | **NAO PROCEDE no sítio corrigido.** Lista nativa usa pendentes + histórico julgados por C6. `api/views.py:1058`. | O teste prova que a morta por tranca permanece em `q_vivas` e exige somente viva + respondida na API. **Não:** o SELECT anterior incluiria a morta; somente pendentes perderia a respondida. `chamados/tests/test_e3_v4_orm.py:42`, `:60`, `:66`. |
| P3 | **NAO PROCEDE pelo `handle` do instrumento.** Lê sistema e fornece seu PK às dependências. `chamados/management/commands/diff_e3_sombra.py:94`, `:98`. | Ausência de sistema e tranca real: **não passariam** reintroduzindo criação ou retirando a semente. `chamados/tests/test_e3_diff_sombra.py:26`, `:41`. |
| P4 | **NAO PROCEDE para exclusões, atribuição simples e `getattr` literal.** Models/registro entram na varredura. `chamados/tests/test_e3_pendentes_app.py:69`, `:75`, `:96`. | **Não:** inclusão e resolução são exigidas em `:119`, `:124`. O contador isolado de `:144` poderia passar com detector incompleto. |
| P5 | **NAO PROCEDE no isolamento/custo corrigidos da consulta.** Memo novo, alimentação única e restauração do contexto. `chamados/juizes.py:1091`, `:1099`, `:1102`. | Uma/cinco perguntas frias e frescor após tranca, toque, retirada do toque e reabertura. **Não:** retirar lote ou reutilizar memo persistente viola essas asserções. `chamados/tests/test_e3_v4_orm.py:85`, `:101`. |

A equivalência desses rótulos com o texto integral do v3b permanece **[nao verificado]**.

2. **UNIDADE: ainda existem leitores externos**

**FILA — o contador literal não é zero:**

- Relatório calcula C6 + ausência de resposta; pauta semanal calcula `q_vivas + sem resposta + disputa aberta`. `chamados/relatorios_disputa.py:18`, `:21`; `chamados/services/cobranca.py:228`.
- Painel/placar separa respondidas e não respondidas fora da consulta. `chamados/services/raiox.py:135`, `:139`, `:140`; consumidor `chamados/services/verbos.py:34`.
- UI web combina `p.viva` e `respondida_em`; o fio também calcula `todas_respondidas`. `templates/chamados/partials/_card_atendimento.html:77`, `:94`, `:155`; `chamados/services/fio.py:340`.

Esses cálculos não foram acrescentados pelo TOTAL fornecido. Qual deles corresponde exatamente a F1/F2, e quais exceções a lei autoriza, fica **[nao verificado]**.

**NAO PROCEDE — kernels e dashboard corrigidos:** ambos os kernels e o dashboard contam perguntas da consulta central. `api/views.py:325`, `api/views_core.py:314`, `core/views.py:253`.

3. **P5: memo, autoridade, equivalência e N+1**

- **NAO PROCEDE — persistência do memo da consulta:** o `ContextVar` é global, mas o conteúdo nasce por chamada e o token é restaurado no `finally`. `chamados/juizes.py:1080`, `:1091`, `:1102`.
- **NAO PROCEDE — predicado duplicado:** C6 chama o avulso; o avulso delega ao lote; toque avulso também delega ao lote. Empresa/competência e trilha são julgadas nesses mesmos corpos. `chamados/juizes.py:1327`, `:951`, `:1022`, `:1145`.
- **FILA — equivalência irrestrita é falsa:** fora da consulta, o avulso conserva `_tranca_memo` no request/colaborador. Pode ler “não trancado”, ocorrer tranca e continuar retornando falso; a consulta seguinte usa memo novo e retorna verdadeiro. Reproduzido em memória. Cura eventual: invalidar o memo ou explicitar seu escopo de validade. `chamados/juizes.py:1064`, `:1068`, `:1024`, `:1091`.
- **O `assertNumQueries` morde N+1 de tranca/toque no cenário exercitado:** exige nove consultas tanto com uma quanto com cinco perguntas, em instâncias frias. Não prova custo constante nos demais caminhos de C6. `chamados/tests/test_e3_v4_orm.py:90`, `:91`, `:97`, `:98`.

4. **P3: escrita transitiva**

**NAO PROCEDE para o comando normal:** a semente impede alcançar o `get_or_create`; todos os aliases recebem a guarda SQL, e há teste que injeta escritor numa dependência. `chamados/management/commands/diff_e3_sombra.py:92`, `:98`; `chamados/juizes.py:1173`; `chamados/tests/test_e3_diff_sombra.py:71`.

**FILA da guarda:** aceita `SELECT … FOR UPDATE`; isso não demonstra caminho concreto de trava/escrita nesta árvore, mas impede considerá-la bloqueio universal de travas. Cura: rejeitar também essa forma e acrescentar caso negativo. `chamados/management/commands/diff_e3_sombra.py:35`.

5. **Classes reincidentes no TOTAL**

- **PROCEDENTE — escritor transitivo na tarefa:** é o achado inicial. O selo de zero escritas exercita cenário sem tranca e, portanto, pode passar apesar desse caminho. `chamados/services/tarefa_do_dia.py:370`, `chamados/juizes.py:1176`, `core/utils.py:18`, `chamados/tests/test_e2_tarefa_do_dia.py:878`.
- **NAO PROCEDE — derivação paralela nos pontos examinados:** posição, passivo, lavra, tranca e verbos são lidos das autoridades existentes. `chamados/services/tarefa_do_dia.py:65`, `:346`, `:350`, `:367`, `:408`.
- **NAO PROCEDE — fachada C17 pousada antecipadamente:** o instrumento compara resultados; não redireciona C17 para tarefa. `chamados/management/commands/diff_e3_sombra.py:124`, `:125`, `:127`.
- **NAO PROCEDE — relógio novo dentro da tarefa/lavra leitora:** recebem instante; decisões/efeitos incluem igualdade, lavra futura é rejeitada e silêncio é estritamente futuro. `chamados/services/tarefa_do_dia.py:339`, `:358`; `chamados/veredito_lavrado.py:224`; `chamados/services/painel.py:190`. O fallback antigo de C6 ainda lê `date.today()`. `chamados/juizes.py:575`.
- **FILA — selo C17 incompleto:** import relativo e alias por desempacotamento escapam do detector; nenhum quarto chamador real foi demonstrado. Cura: resolver `ImportFrom.level` e alvos compostos, com REDs. `chamados/tests/test_e3_pendentes_app.py:46`, `:78`.
- **FILA — ocorrência repetida:** tarefa distingue `int_ida:2`, mas a identidade persistente exige posição derivada do motivo; a tarefa abstém ou pede emissão. Cura futura exige migrar identidade, emissão e leitura juntas. `chamados/services/tarefa_do_dia.py:67`, `:268`; `chamados/models.py:3425`. Exigibilidade nesta etapa **[nao verificado]**.

6. **Escritores → leitores principais, inclusive fora do diff**

| Dado/função | Quem produz/escreve → quem lê |
|---|---|
| `pendentes`, `historico` | Consulta particiona em memória: `chamados/services/pergunta_no_app.py:69` → kernels, dashboard e API: `api/views.py:325`, `:1023`, `:2561`; `api/views_core.py:314`; `core/views.py:253`. |
| `perguntas_pendentes_do_colab` | Delega à consulta → aviso textual. `chamados/services/aviso_pendencia.py:9`, `:13`. Cobrança delega diretamente: `chamados/services/cobranca.py:48`. |
| Saída da tarefa: instâncias, ações, razão, rota/fila/verbos/fio | Produção em memória: `chamados/services/tarefa_do_dia.py:237`, `:283`, `:433` → instrumento/projeções: `chamados/management/commands/diff_e3_sombra.py:125`, `:44`, `:51`. |
| `data_evento`, identidade | Atribuição: `chamados/services/fio.py:800`; persistência do rechaveamento: `:1034`; identidade: `chamados/models.py:3425` → tarefa: `chamados/services/tarefa_do_dia.py:233`, `:324`; C6: `chamados/juizes.py:1386`. |
| Resposta/validação e sequência | Projeção: `chamados/models.py:3121`, `:3124`, `:3141`; sequência: `:3934` → consulta: `chamados/services/pergunta_no_app.py:70`; passivo/decisões: `chamados/services/tarefa_do_dia.py:337`, `:346`. |
| Ata/lâmpadas | Persistência pela porta: `ponto/portas/celula.py:969` → leitor único: `escala/services/leitor_celula.py:130` → tarefa: `chamados/services/tarefa_do_dia.py:84`; C6: `chamados/juizes.py:925`. |
| Lavra e marcos | Escritores: `chamados/veredito_lavrado.py:250`, `:270` → validade: `:224`; tarefa: `chamados/services/tarefa_do_dia.py:350`; leitor legado: `chamados/services/raiox.py:184`. |
| Ausência/cobertura | Estado: `ponto/services/ausencia.py:824`, `:835` → cobertura: `ponto/turnos.py:3064`; tarefa: `chamados/services/tarefa_do_dia.py:211`, `:152`. |
| Status/silêncio, fila e verbos | Status: `chamados/models.py:296`; silêncio: `chamados/services/arquivo.py:166`, `:202` → painel: `chamados/services/painel.py:188`; verbos: `chamados/services/verbos.py:47`, `:51` → alimentação: `chamados/juizes.py:1460`. |
| Tranca/toque e lote C6 | Período: `ponto/services/fechamento.py:1400`, `:1432`; trilha: `chamados/services/cobrar_dia.py:39`, `ponto/services/veredito_celula.py:355`, `chamados/views_arquivar.py:77` → lote: `chamados/juizes.py:1026`, `:1215`; consumidores: `:1327`, `chamados/models.py:1843`, `chamados/reconciliador.py:667`, `chamados/services/disputa_emissao.py:1844`, `chamados/services/tarefa_do_dia.py:370`. |

7. **Merge futuro**

**FILA — acoplamentos previsíveis:** preservar seleção/histórico nos três pontos da API; manter identidade do modelo compatível com a tarefa; preservar a regra compartilhada de posição. Alterações nesses pontos podem mudar comportamento sem conflito textual. Conflito efetivo com o outro main **[nao verificado]**. `api/views.py:325`, `:1058`, `:2561`; `chamados/models.py:3425`; `chamados/services/tarefa_do_dia.py:108`; `chamados/services/declaracao_texto.py:79`, `:134`.

Resultado da suíte inteira e “48/48 mortos”: **[nao verificado]**. [nao verificado]

## 09/11 15:46 · sessao 3efb1b2451e1 · CODEX-2-RAIAS-PORTAO-CPU · leitura · origem=Code

**A matriz fixa quatro divergências procedentes: P1 na representação dos nomes; P2 no julgamento da pasta-pai; P3 no significado declarado de “run”; P4 nos esperados dos mutantes de carga.** O eco de diagnóstico é uma fragilidade adicional do classificador, sem mutante atual demonstrado que a produza. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:287`, `bin/codex_vagas_medida.py:114`, `bin/tests/test_codex_pergunta.sh:720`, `bin/tests/test_codex_pergunta.sh:747`; ocorrência do eco em mutante atual: [não verificado])

Nesta matriz, **E** significa esperado e **A** significa atual. `E=A` indica concordância. O esperado aplica “arquivo em comum”, preserva arquivo exato sem `/`, pasta por componente com `/` e a recusa conservadora do mesmo nome com/sem `/`. (`CLAUDE.md:306`, `CLAUDE.md:307`, `CLAUDE.md:278`, `bin/codex_pergunta.sh:258`)

Os resultados abaixo são leitura do código e dos casos escritos. GREEN atual da árvore e ocorrência em produção permanecem **[não verificado]**.

**Convenções de persistência e retorno usadas em A e B**

| Sigla | Esperado e atual, salvo divergência expressa |
|---|---|
| **P0** | Recusa anterior ao `ini`: nenhum novo par `ini/fim` nem novo `.rc` desta tentativa. Pode haver criação dos arquivos de lock; estado eventualmente publicado antes de uma recusa posterior é removido pelo EXIT. Artefatos antigos no mesmo nome não equivalem a sinais desta tentativa. (`bin/codex_pergunta.sh:228`, `bin/codex_pergunta.sh:236`, `bin/codex_pergunta.sh:380`, `bin/codex_pergunta.sh:392`, `bin/codex_pergunta.sh:428`) |
| **PV** | Recusa de portão/teto/meta na admissão: P0 mais tentativa de append em `vaga2.log`, campos `hora, obra, motivo, load5, fonte`. Falha desse append não impede rc 2. (`bin/codex_pergunta.sh:249`, `bin/codex_pergunta.sh:250`) |
| **P+** | Admitida: `.estado` temporário por rename, com `toca=`; depois `chamadas.log ini`. Após retorno do Codex: `.inventario`; no EXIT, `fim`, `.rc` e remoção do estado. `.id` somente com RC 0 e resposta nova desta chamada. (`bin/codex_pergunta.sh:310`, `bin/codex_pergunta.sh:311`, `bin/codex_pergunta.sh:428`, `bin/codex_pergunta.sh:560`, `bin/codex_pergunta.sh:229`, `bin/codex_pergunta.sh:232`, `bin/codex_pergunta.sh:236`, `bin/codex_pergunta.sh:603`) |
| **P!** | P+ com `.inventario` proibido/falho, `.recusa`, `.rc rc=3`, `fim`; sem novo `.id`. A mensagem manda descartar a WT, mas não a remove automaticamente. (`bin/codex_pergunta.sh:576`, `bin/codex_pergunta.sh:578`, `bin/codex_pergunta.sh:579`, `bin/codex_pergunta.sh:587`, `bin/tests/test_codex_pergunta.sh:169`) |
| **R0** | Admitida e execução/inventário verdes: retorno do CLI, 0 no controle; mensagem `sessao=… rc=0 jsonl=…`. Admissão, sozinha, não garante sucesso final. (`bin/codex_pergunta.sh:551`, `bin/codex_pergunta.sh:608`, `bin/codex_pergunta.sh:609`) |
| **R2** | `construtor: … (rc 2; Codex nao chamado)`. (`bin/codex_pergunta.sh:244`) |
| **RV2** | `VAGA2: … (rc 2; Codex nao chamado)`. (`bin/codex_pergunta.sh:250`) |
| **R3** | `RECUSA rc 3 … wt DESCARTADA`; inventário proibido/falho vence qualquer retorno do CLI. (`bin/codex_pergunta.sh:557`, `bin/codex_pergunta.sh:578`, `bin/codex_pergunta.sh:587`) |

**A. PORTA `--toca`**

As comparações da admissão operam sobre declarações. O fim compara caminhos encontrados no inventário com essas declarações; só processa registros cuja primeira coluna seja `TOCADO`. (`bin/codex_pergunta.sh:270`, `bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:280`)

| Célula / estado do caminho | Argumento: parser/validação | Admissão: outra vaga viva | Inventário: classe, serialização e releitura | Juiz final: fora / comum | Persistido | Retorno | Prova: caso / mutante |
|---|---|---|---|---|---|---|---|
| **A1 Arquivo exato** `app/folha/views.py` | E=A: recebe em `TOCA[]`; sintaxe válida. (`bin/codex_pergunta.sh:71`, `bin/codex_pergunta.sh:323`) | E=A: mesmo arquivo cruza; arquivo distinto não cruza, ressalvada cobertura por pasta. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:266`) | E=A para nome simples: `TOCADO`, TSV cru; leitor recupera o segundo campo. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`) | E=A: igualdade cobre somente esse arquivo; outro arquivo fica fora; cobertura da outra raia recusa. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`) | E=A: P+ se válido; P! se violar escopo. | E=A: R0/R3; conflito declarado na abertura dá RV2. | `c_toca_exato`, `c_toca_fim` / `toca_exato`, `toca_fim`; E1/E3. (`bin/tests/test_codex_pergunta.sh:559`, `bin/tests/test_codex_pergunta.sh:562`, `bin/tests/test_codex_pergunta.sh:732`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A2 Pasta/** `app/folha/` | E=A: `/` final declara pasta. (`bin/codex_pergunta.sh:321`) | E=A: cruza descendentes e pastas ancestrais declaradas. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:266`) | E=A: descendentes modificados são inventariados; pasta nova também ganha registro terminado em `/`. (`bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:217`) | E=A: prefixo com `/` cobre por componente. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`) | E=A: P+/P!. | E=A: R0/R3; sobreposição declarada dá RV2. | `c_toca_exato/controle_pasta` / `toca_exato`; E1. (`bin/tests/test_codex_pergunta.sh:567`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A3 Pasta-pai criada para arquivo nomeado**, sem outra declaração sob o pai | E=A: declara `app/novo/a.py`, sem exigir declaração ampla de `app/novo/`. (`bin/codex_pergunta.sh:323`) | E=A: compara o arquivo declarado com a outra declaração. (`bin/codex_pergunta.sh:273`) | E=A: cria registros `app/novo/` e `app/novo/a.py`; ambos em TSV cru. (`bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:217`) | E=A: pai necessário é aceito pelo meu escopo; arquivo continua exato. (`bin/codex_pergunta.sh:284`) | E=A: P+. | E=A: R0. | `c_toca_exato/controle_pasta_nova`; falta mutante específico da aceitação do pai. E6. (`bin/tests/test_codex_pergunta.sh:569`; mutante específico: [não verificado]) |
| **A4 Arquivos irmãos sob pai novo**: minha `app/novo/a.py`, outra `app/novo/b.py` | E=A: ambas declarações válidas. (`bin/codex_pergunta.sh:323`) | E=A: admite, pois arquivos diferentes não cruzam. (`bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:273`) | E=A: a pasta-pai nova também é `TOCADO`. (`bin/codex_pergunta.sh:196`) | **DIVERGE P2. E:** pai necessário não constitui arquivo comum; aceita. **A:** aceita o pai no meu escopo, mas `cruza(outra, pai)` o considera comum com `b.py`. (`CLAUDE.md:306`, `bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`) | **E: P+. A: P!**, com falso motivo de arquivo comum. (`bin/codex_pergunta.sh:288`, `bin/codex_pergunta.sh:579`) | **E: R0. A: R3.** (`bin/codex_pergunta.sh:587`) | E6: caso irmãos novos / mutante restaura colisão pelo pai. E7: controle outra pasta cobrindo deve recusar. Novos: [não verificado]. Sítio atual: `bin/codex_pergunta.sh:287`. |
| **A5 Pasta irmã / prefixo parecido**: `app/folhas/` × `app/folha/` | E=A: válida. (`bin/codex_pergunta.sh:323`) | E=A: disjuntas. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:266`) | E=A: nomes permanecem distintos no TSV simples. (`bin/codex_pergunta.sh:217`) | E=A: `folha/` não cobre `folhas/`; tocar esta última sob declaração da primeira fica fora. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`) | E=A: P+ quando declarada; P! quando fora. | E=A: R0/R3. | `c_v2_toca/controle`, `c_toca_exato/folha_nao_cobre_folhas` / `toca`, `toca_exato`; E1/E3. (`bin/tests/test_codex_pergunta.sh:530`, `bin/tests/test_codex_pergunta.sh:564`, `bin/tests/test_codex_pergunta.sh:725`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A6 Mesmo nome com e sem `/`** | E=A: ambas grafias válidas, com significados distintos de cobertura. (`bin/codex_pergunta.sh:258`, `bin/codex_pergunta.sh:323`) | E=A: recusa conservadora por igualdade após retirar `/`. (`CLAUDE.md:278`, `bin/codex_pergunta.sh:266`) | E=A: se já recusado, não alcança inventário final. Sem outra viva, inventário mantém arquivo e pasta com suas grafias. (`bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:306`) | E=A: sem `/` não autoriza descendentes; sobreposição com outra declaração continua proibida. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:287`) | E=A: PV na admissão; P+/P! nos cenários sem conflito inicial. | E=A: RV2; ou R0/R3 conforme o toque. | `c_v2_toca/app/folha`, `c_toca_exato/arquivo_nao_e_pasta` / `toca`, `toca_exato`; E1/E3. (`bin/tests/test_codex_pergunta.sh:529`, `bin/tests/test_codex_pergunta.sh:563`, `bin/tests/test_codex_pergunta.sh:725`, `bin/tests/test_codex_pergunta.sh:733`) |
| **A7 Nome com TAB**: arquivo real `views.py<TAB>fora`, declaração exata `views.py` | E=A: argumento contendo TAB é recusado; a declaração simples passa. (`bin/codex_pergunta.sh:323`) | E=A sobre a declaração simples; desconhece o futuro nome com TAB. (`bin/codex_pergunta.sh:270`) | **DIVERGE P1. E:** nome com controle é PROIBIDO, com motivo próprio inequívoco. **A:** imprime nome cru; TAB divide campos, e só `c[1]` vira caminho. (`CLAUDE.md:278`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`, `bin/codex_pergunta.sh:282`) | **E:** recusa o nome real. **A:** compara apenas `views.py`, podendo aprová-lo como arquivo exato. (`bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:284`) | **E: P!. A:** pode ficar P+, com `.inventario` cujo segundo campo truncado aparenta ser permitido. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:560`) | **E: R3. A: R0 possível** no cenário isolado descrito. (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:587`, `bin/codex_pergunta.sh:609`) | E4/TAB / mutante remove proibição de controle. Caso e mutante próprios atuais: [não verificado]. Lista atual: `bin/tests/test_codex_pergunta.sh:617`. |
| **A8 Nome com newline** | E=A: argumento completo recusado; declaração simples passa. (`bin/codex_pergunta.sh:323`) | E=A sobre a declaração simples. (`bin/codex_pergunta.sh:270`) | **DIVERGE P1. E:** PROIBIDO por controle. **A:** nome cru quebra registro em linhas; fragmento inicial é lido como caminho, fragmentos sem classe `TOCADO` são ignorados. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:280`) | **E:** recusa. **A:** prefixo anterior ao newline pode casar com arquivo exato declarado. (`bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:284`) | **E: P!. A:** P+ possível; registros físicos deixam de representar uma entrada por linha. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:560`) | **E: R3. A: R0 possível.** (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:609`) | E4/newline / mutante remove proibição de controle. Novos: [não verificado]. Sítios atuais: `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`. |
| **A9 Nome com CR** | E=A: argumento completo recusado. (`bin/codex_pergunta.sh:323`) | E=A sobre eventual declaração simples. (`bin/codex_pergunta.sh:270`) | **DIVERGE P1. E:** PROIBIDO por controle. **A:** escrita crua e leitura textual com tradução de quebras permitem que CR separe o nome durante a leitura. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:278`) | **E:** recusa. **A:** pode comparar somente o prefixo anterior ao CR. (`bin/codex_pergunta.sh:282`, `bin/codex_pergunta.sh:284`) | **E: P!. A:** P+ possível, sem preservação do nome real no julgamento. (`bin/codex_pergunta.sh:278`, `bin/codex_pergunta.sh:560`) | **E: R3. A: R0 possível.** (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:609`) | E4/CR / mutante remove proibição de controle. Novos: [não verificado]. |
| **A10 Nome com espaço** | E=A: argumento com espaço recusado pela gramática atual restritiva. (`bin/codex_pergunta.sh:323`) | E=A: pode declarar pasta ancestral sem espaço. (`bin/codex_pergunta.sh:264`) | E=A: espaço não separa campos TSV; nome encontrado é preservado. (`bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`) | E=A: sob pasta declarada pode passar; `views.py espaço` não equivale ao arquivo exato `views.py`. (`bin/codex_pergunta.sh:264`, `bin/codex_pergunta.sh:284`) | E=A: P+/P!, conforme cobertura. | E=A: R0/R3; argumento com espaço dá R2. | E5: controles positivo sob pasta e negativo contra arquivo exato / mutante reduz nome ao primeiro trecho separado por espaço. Novos: [não verificado]. |
| **A11 Symlink tocado** | E=A: valida nome, não o tipo futuro. (`bin/codex_pergunta.sh:323`) | E=A: compara declarações. (`bin/codex_pergunta.sh:273`) | E=A: `lstat`; walk não segue diretório symlink; link tocado é PROIBIDO, motivo `symlink`. (`bin/codex_pergunta.sh:188`, `bin/codex_pergunta.sh:194`, `bin/codex_pergunta.sh:215`) | E=A: já proibido pelo inventário; o juiz de escopo não o reautoriza. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P! no fim; se já inventariado no pré-check, P0. | E=A: R3 no fim; R2 no pré-check sujo. (`bin/codex_pergunta.sh:399`, `bin/codex_pergunta.sh:587`) | `c_p_link` / `p_link`; E8. (`bin/tests/test_codex_pergunta.sh:366`, `bin/tests/test_codex_pergunta.sh:663`) |
| **A12 Hardlink** | E=A: argumento não concede exceção ao tipo. (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:216`) | E=A: comparação lexical não substitui inventário. (`bin/codex_pergunta.sh:273`, `bin/codex_pergunta.sh:397`) | E=A: `nlink>1` entra mesmo com conteúdo igual; motivo `hardlink`. (`bin/codex_pergunta.sh:201`, `bin/codex_pergunta.sh:216`) | E=A: pré-check/revalidação recusam; se criado na execução, inventário final proíbe. (`bin/codex_pergunta.sh:399`, `bin/codex_pergunta.sh:530`, `bin/codex_pergunta.sh:560`) | E=A: P0 antes do `ini`; P! no fim. | E=A: R2 antes da execução; R3 no fim. | `hardlink_ini`, `p_hardlink_ident`, `revalida_hardlink`, `p_hardlink` / `inv_nlink`, `p_hardlink`; E8. (`bin/tests/test_codex_pergunta.sh:296`, `bin/tests/test_codex_pergunta.sh:403`, `bin/tests/test_codex_pergunta.sh:406`, `bin/tests/test_codex_pergunta.sh:656`, `bin/tests/test_codex_pergunta.sh:664`) |
| **A13 `..`** | E=A: qualquer ocorrência de `..` é recusada, inclusive dentro de componente. (`bin/codex_pergunta.sh:323`) | E=A: não alcança. (`bin/codex_pergunta.sh:324`) | E=A: não alcança nesta chamada; inventário usa nomes relativos encontrados, não a grafia da operação. (`bin/codex_pergunta.sh:189`, `bin/codex_pergunta.sh:193`) | E=A: não alcança por esse argumento. | E=A: P0. | E=A: R2, `invalido`. | `c_toca_forma` / `toca_forma`; E2. (`bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A14 `//`** | E=A: recusado. (`bin/codex_pergunta.sh:323`) | E=A: não alcança. | E=A: não alcança por esse argumento. | E=A: não alcança. | E=A: P0. | E=A: R2, `invalido`. (`bin/codex_pergunta.sh:324`) | `c_toca_forma/app//x` / `toca_forma`; E2. (`bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A15 Absoluto** | E=A: recusado por não começar com `app/` ou `bin/`. (`bin/codex_pergunta.sh:323`) | E=A: não alcança. | E=A: não alcança por esse argumento. | E=A: não alcança. | E=A: P0. | E=A: R2, `invalido`. (`bin/codex_pergunta.sh:324`) | `c_toca_forma//abs` / `toca_forma`; E2. (`bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A16 Fora de `app/` e `bin/`** | E=A: declaração `ops/` recusada. (`bin/codex_pergunta.sh:323`) | E=A: não alcança por essa declaração. | E=A: se execução com outro argumento criar `FORA.txt` na WT, inventário marca PROIBIDO, `fora de app/ e bin/`. (`bin/codex_pergunta.sh:151`, `bin/codex_pergunta.sh:210`) | E=A: proibição independe do próprio `--toca`. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P0 no argumento; P! no toque encontrado. | E=A: R2/R3. | `c_toca_forma`, `c_p_fora` / `toca_forma`, `p_fora`; E2/E10. (`bin/tests/test_codex_pergunta.sh:352`, `bin/tests/test_codex_pergunta.sh:554`, `bin/tests/test_codex_pergunta.sh:689`, `bin/tests/test_codex_pergunta.sh:731`) |
| **A17 `bin/` comum** | E=A: permitido declarar. (`bin/codex_pergunta.sh:323`) | E=A: mesmas regras de arquivo/pasta comum. (`bin/codex_pergunta.sh:266`) | E=A: TOCADO se não houver outro motivo proibido; declaração `bin/...` concede root adicional `<wt>/bin`. (`bin/codex_pergunta.sh:151`, `bin/codex_pergunta.sh:539`, `bin/codex_pergunta.sh:543`) | E=A: exige cobertura própria e ausência de cobertura conflitante. (`bin/codex_pergunta.sh:284`, `bin/codex_pergunta.sh:287`) | E=A: P+/P!. | E=A: R0/R3. | `c_bin_ok`; E9. Caso atual: `bin/tests/test_codex_pergunta.sh:582`; mutante específico da concessão do root: [não verificado]. |
| **A18 `bin/` protegido** | E=A: declaração pode passar; declarar não revoga proteção. (`bin/codex_pergunta.sh:323`, `bin/codex_pergunta.sh:162`) | E=A: pode admitir se declarações disjuntas. (`bin/codex_pergunta.sh:273`) | E=A: PROIBIDO pela lista protegida, mesmo coberto pelo `--toca`. (`bin/codex_pergunta.sh:157`, `bin/codex_pergunta.sh:162`, `bin/codex_pergunta.sh:210`) | E=A: já proibido; não reautoriza. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P!. | E=A: R3, motivo `protegido (segue do opus)`. | `c_bin_protegido` / `lista_protegida`, `p_protegido`; E9. (`bin/tests/test_codex_pergunta.sh:592`, `bin/tests/test_codex_pergunta.sh:736`, `bin/tests/test_codex_pergunta.sh:737`) |
| **A19 `.claude/`** | E=A: `.claude/` na raiz é argumento inválido; `app/.claude/...` pode passar na sintaxe. (`bin/codex_pergunta.sh:323`) | E=A: eventual declaração válida passa só se disjunta. (`bin/codex_pergunta.sh:273`) | E=A: componente `.claude` é PROIBIDO em qualquer nível. (`bin/codex_pergunta.sh:147`) | E=A: cobertura própria não revoga proibição. (`bin/codex_pergunta.sh:280`, `bin/codex_pergunta.sh:576`) | E=A: P0 ou P!, conforme fase. | E=A: R2/R3. | `c_p_claude` / `p_claude`; E10. (`bin/tests/test_codex_pergunta.sh:348`, `bin/tests/test_codex_pergunta.sh:685`) |

**ESMERIL-SEMÂNTICA:** em **A4/juiz final**, a admissão diz “arquivos disjuntos”, mas o fim transforma a pasta-pai necessária em arquivo comum. Em **A7–A9/inventário e juiz final**, o nome encontrado deixa de ser a unidade comparada: a representação admite delimitadores que o leitor interpreta estruturalmente. (`bin/codex_pergunta.sh:266`, `bin/codex_pergunta.sh:196`, `bin/codex_pergunta.sh:287`, `bin/codex_pergunta.sh:217`, `bin/codex_pergunta.sh:279`, `bin/codex_pergunta.sh:282`)

A linha de symlink refere-se ao **tocado/inventariado**; não demonstra proibição de todo symlink rastreado intacto. Escrita transitória desfeita antes do inventário, ou fora da WT sem vestígio nela, permanece **[não verificado]**. (`bin/codex_pergunta.sh:199`, `bin/codex_pergunta.sh:209`, `bin/codex_pergunta.sh:215`)

**B. VAGA E PORTÕES**

“1ª” abaixo significa nenhuma vaga viva; “2ª”, uma viva. A aplicação dos portões depende da **outra vaga viva**, inclusive quando a vaga disponível é a 1 e a ocupada é a 2. (`bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:389`, `bin/codex_pergunta.sh:390`, `bin/codex_pergunta.sh:391`)

| Célula / situação | Juiz: E × A | Admite/recusa: E × A | Persistido: E × A | Retorno: E × A | Prova: caso / mutante |
|---|---|---|---|---|---|
| **B1 1ª chamada** | E=A: meta-trava, tenta vaga 1, confirma outra livre. (`bin/codex_pergunta.sh:380`, `bin/codex_pergunta.sh:383`, `bin/codex_pergunta.sh:390`) | E=A: dispensa portões da segunda simultânea; publica estado. (`bin/codex_pergunta.sh:391`, `bin/codex_pergunta.sh:392`) | E=A: P+. | E=A: R0 se restante verde. | `c_positivo`, primeira chamada de `c_v2_admite`; E11. (`bin/tests/test_codex_pergunta.sh:315`, `bin/tests/test_codex_pergunta.sh:506`) Controle com portões vermelhos e nenhuma outra viva: [não verificado]. |
| **B2 2ª chamada** | E=A: outra viva exige pausa, estado, carga, janela, desligador e escopo verdes. (`bin/codex_pergunta.sh:293`, `bin/codex_pergunta.sh:391`) | E=A: admite só após todos; publica sob meta. (`bin/codex_pergunta.sh:305`, `bin/codex_pergunta.sh:392`, `bin/codex_pergunta.sh:393`) | E=A: P+ com `vaga=2` no caso em que a primeira ocupa vaga 1. (`bin/

[truncado em 20000 de 60850 caracteres]

## 09/11 15:44 · sessao c2a5482fbf2c · - · - · origem=Ronald (tmux)

- **P3 — NAO PROCEDE no instrumento:** o comando lê o usuário e fornece `sistema_pk` antes das dependências; o contexto restaura o memo ao sair. Assim, o fallback escritor fica inacessível pelo `handle` normal (`app/chamados/management/commands/diff_e3_sombra.py:94`, `app/chamados/management/commands/diff_e3_sombra.py:98`, `app/chamados/juizes.py:1111`, `app/chamados/juizes.py:1173`). Os testes de usuário ausente e de tranca real falhariam sem as respectivas curas (`app/chamados/tests/test_e3_diff_sombra.py:26`, `app/chamados/tests/test_e3_diff_sombra.py:41`). A guarda intercepta todos os aliases e o teste injeta escritor transitivo (`app/chamados/management/commands/diff_e3_sombra.py:92`, `app/chamados/tests/test_e3_diff_sombra.py:71`).

- **PROCEDENTE fora do instrumento — leitura nova pode escrever:** `CargaDaTarefa` chama `dias_trancados`; com tranca e memo sem usuário, chega a `get_user_sistema().pk`, cujo corpo executa `get_or_create`. Portanto, usuário ausente permite INSERT durante `tarefa_do_dia` (`app/chamados/services/tarefa_do_dia.py:367`, `app/chamados/juizes.py:1036`, `app/chamados/juizes.py:1173`, `app/core/utils.py:18`). Cura: obter o usuário por leitura estrita ou exigir sua identidade na borda; acrescentar teste de tranca com usuário ausente, bloqueando qualquer tentativa de criação. O teste atual de zero escritas não cria tranca (`app/chamados/tests/test_e2_tarefa_do_dia.py:878`).

- **P4 — NAO PROCEDE para os defeitos nomeados:** atribuição simples e `getattr` literal são resolvidos; models e registro entram no universo (`app/chamados/tests/test_e3_pendentes_app.py:46`, `app/chamados/tests/test_e3_pendentes_app.py:69`, `app/chamados/tests/test_e3_pendentes_app.py:75`, `app/chamados/tests/test_e3_pendentes_app.py:96`). Os testes de inclusão e resolução falhariam sem cura; o contador isolado poderia passar perdendo o quarto chamador (`app/chamados/tests/test_e3_pendentes_app.py:119`, `app/chamados/tests/test_e3_pendentes_app.py:124`, `app/chamados/tests/test_e3_pendentes_app.py:144`).

- **FILA — alcance residual do selo:** imports relativos ignoram `ImportFrom.level`; atribuição por desempacotamento não registra aliases. Ambos podem esconder um quarto chamador; uso real nesta árvore não demonstrado (`app/chamados/tests/test_e3_pendentes_app.py:46`, `app/chamados/tests/test_e3_pendentes_app.py:78`). Cura sugerida: resolver esses dois formatos e acrescentar respectivos REDs.

- **FILA — guarda SQL:** aceita `SELECT … FOR UPDATE`; nenhum caminho concreto com essa trava foi demonstrado. Não constitui escritor P3 comprovado, mas impede chamar a guarda de bloqueio universal de travas (`app/chamados/management/commands/diff_e3_sombra.py:35`).

_3209 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
