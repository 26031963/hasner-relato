# CODEX-ESTADO

> Ultima resposta processada: **30/10/2026 10:28** · publicado em **30/10/2026 10:31** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 30/10 10:28 · sessao f53634859433 · O261-VETO-A · verificador · origem=Code

**Reprova:** há reaberturas automáticas que contornam o veto, nascimento do universo que passa sem consultar o juiz e contraexemplo à promessa de DIFF zero (`app/ponto/management/commands/reconciliar_grade.py:522`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/models.py:893`; `app/ponto/management/commands/processar_alertas_turno.py:100`).

Impacto numérico na frota, execução da suíte Django e RED anterior ao código: **[nao verificado]**. [nao verificado]

**(b) Caminhos automáticos que escapam e falhas de integração**

1. **A grade desfaz a recusa da própria porta.** `abrir()` descarta o booleano de `_renascer_por_premissa_viva()` e devolve o chamado encerrado; a grade recebe esse objeto e chama `reabrir()` diretamente. A guarda nova contra `None` não alcança esse caso (`app/chamados/models.py:685`; `app/chamados/models.py:688`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:522`).

   **Contraexemplo:** chamado `batida_ausente` resolvido, JSON `data_turno=d`, ligado a célula de `d2`; vínculo saudável em `d`, mas `d2` furado e com régua suspeita. O veto externo da grade passa para `d`; o dedup encontra o chamado; o juiz prioriza a célula e `_renascer` veta `d2`; a grade recebe o encerrado e o reabre sem consultar o veto. As guardas comuns devem permitir a reabertura, por exemplo encerramento pelo sistema, premissa viva e nenhum irmão vivo (`app/ponto/management/commands/reconciliar_grade.py:487`; `app/ponto/management/commands/reconciliar_grade.py:499`; `app/chamados/juizes.py:89`; `app/chamados/models.py:778`; `app/chamados/models.py:1072`).

2. **O callback de resolução também reabre automaticamente por fora.** `resolver_perguntas_por_chamado_resolvido()` pode executar `_pai.reabrir()` sem filtro de módulo e sem passar por `_renascer_por_premissa_viva()`. Há acionamento por receiver de fechamento e pelo reconciliador (`app/chamados/services/disputa_emissao.py:1598`; `app/chamados/services/disputa_emissao.py:1606`; `app/chamados/signals.py:92`; `app/chamados/reconciliador.py:538`).

   **Contraexemplo:** disputa aberta, pai `turno_aberto` resolvido em dia vetado, resposta aguardando consumo reconhecida por `irmas_respondidas_sem_consumo` e guardas comuns permitindo reabrir. Resolver um destino da disputa → callback reabre o pai → chamado vivo em dia vetado. O veto novo não participa desse percurso (`app/chamados/services/disputa_emissao.py:1601`; `app/chamados/services/disputa_emissao.py:1604`; `app/chamados/models.py:1119`).

3. **`entrada_bloqueada_turno_aberto` continua nascendo automaticamente sem veto.** `bater_ponto()` reduz as entradas pendentes a texto e passa apenas `pendentes` e `total`; `_veto_da_regua()` retorna permissivamente quando não encontra dia (`app/ponto/views.py:278`; `app/ponto/views.py:280`; `app/ponto/views.py:291`; `app/chamados/models.py:892`).

   **Contraexemplo:** POST de entrada, turno anterior ≥14h, nenhuma regularização pendente e régua suspeita → contexto sem dia → juiz do veto não é chamado → nasce `entrada_bloqueada_turno_aberto`. O percurso passa fisicamente por `criar`, mas escapa da invariante; o teste novo consagra esse comportamento (`app/ponto/views.py:275`; `app/ponto/views.py:299`; `app/chamados/models.py:893`; `app/chamados/tests/test_o261_veto_na_porta.py:313`).

4. **A premissa usada para garantir DIFF zero é falsa no processador de turno.** O vínculo só é obrigatório quando `tm.dt_fim_previsto` está ausente. Com fim previsto persistido, `ec=None` não interrompia o caminho de emissão (`app/ponto/management/commands/processar_alertas_turno.py:100`; `app/ponto/management/commands/processar_alertas_turno.py:134`).

   **Contraexemplo:** turno materializado aberto, entrada presente, fim previsto preenchido, janela `pos_20`, sem ausência e sem vínculo vigente → antes alcançava o nascimento; depois a porta retorna `(None, False)` por `VETO_DEGENERADA`, interrompendo também alerta e justificativa. Portanto DIFF zero não decorre da construção alegada em CORTES; existência desse estado na frota é **[nao verificado]** (`app/ponto/management/commands/processar_alertas_turno.py:138`; `app/ponto/management/commands/processar_alertas_turno.py:317`; `app/ponto/management/commands/processar_alertas_turno.py:335`; `app/ponto/services/furos_vetados.py:32`; `app/docs/CORTES.md:401`).

   Também muda o nascimento N5 com vínculo existente e suspeito: o avançado chegava a `criar` sem perguntar esse veto e agora recebe `None` (`app/ponto/management/commands/processar_alertas_avancados.py:37`; `app/ponto/management/commands/processar_alertas_avancados.py:121`; `app/ponto/management/commands/processar_alertas_avancados.py:141`).

5. **Recusa do renascimento ainda pode produzir cobrança no marco crítico.** `_abrir_chamado_critico()` devolve o objeto encerrado retornado pelo dedup; `_marco_critico()` interrompe apenas para `None` (`app/chamados/models.py:688`; `app/ponto/management/commands/processar_alertas_turno.py:387`; `app/ponto/management/commands/processar_alertas_turno.py:317`).

   **Contraexemplo:** `turno_aberto` resolvido, premissa viva, dia agora vetado, sem alerta `pos_20` nem justificativa existentes → `_renascer` recusa → dedup devolve o resolvido → marco envia alerta e, se enviado, cria justificativa apontando para o chamado encerrado. O leitor não distingue a nova recusa automática (`app/chamados/models.py:778`; `app/ponto/management/commands/processar_alertas_turno.py:314`; `app/ponto/management/commands/processar_alertas_turno.py:319`; `app/ponto/management/commands/processar_alertas_turno.py:391`).

6. **A exceção humana fica incompleta, e o vigia conta essa exceção como violação.** Desarquivar e reabrir administrativamente passam sem consultar/apresentar o motivo do veto; suas trilhas registram resgate ou justificativa administrativa, sem registrar a autorização sobre aquele veto. O contador admite expressamente adiar a trilha própria ao B (`app/chamados/views_arquivar.py:63`; `app/chamados/views_arquivar.py:75`; `app/chamados/services/acoes_disputa.py:245`; `app/chamados/services/acoes_disputa.py:270`; `app/ponto/services/furos_vetados.py:82`).

   **Contraexemplo:** admin desarquiva chamado encerrado pela tranca, em dia `VETO_REGUA`, com guardas comuns permitindo reabrir → chamado volta à fila sem apresentação/trilha específica do veto → vigia retorna um achado, embora o ato seja permitido pelo aval. O contador filtra todos os vivos, sem distinguir a exceção humana (`app/chamados/views_arquivar.py:68`; `app/chamados/views_arquivar.py:80`; `app/ponto/services/furos_vetados.py:87`; `app/ponto/services/furos_vetados.py:98`; `app/docs/CORTES.md:401`).

**(a) Censo dos leitores cujo comportamento muda**

Leitores diretos de produção de `motivo_do_veto`:

| Leitor | Mudança e contraexemplo |
|---|---|
| `_veto_da_regua` — `app/chamados/models.py:896` | Novo veto dentro das três operações; universo com dia e vínculo suspeito → nascimento/renascimento recusado. |
| `reemitir_chamado_do_dia` — `app/chamados/reconciliador.py:854` | Sem vínculo → agora retorna `False` antes de emitir/costurar; anteriormente `motivo_do_veto(..., None, ...)` não vetava. |
| `furos_vetados_por_regua` — `app/ponto/services/furos_vetados.py:57` | Célula `furo` sem vínculo → passa a entrar nos achados com `vinculo=None` (`app/ponto/services/furos_vetados.py:61`). |
| `julgar_colab._veto_do_dia` — `app/ponto/services/cartorio.py:1013` | Mesmo dia sem vínculo e mesmos demais insumos → impressão passa a conter `veto:VETO_DEGENERADA`, deixa de coincidir e provoca rejulgamento (`app/ponto/services/cartorio.py:329`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`). |
| `furos_sem_cobranca.medir` — `app/ponto/services/furos_sem_cobranca.py:96` | Furo elegível, sem chamado vivo e sem vínculo → passa de canal de cobrança para `veto`; sai do total e `cobrar()` o pula (`app/ponto/services/furos_sem_cobranca.py:107`; `app/ponto/services/furos_sem_cobranca.py:109`; `app/ponto/services/furos_sem_cobranca.py:125`). |
| `chamados_vivos_em_dia_vetado` — `app/ponto/services/furos_vetados.py:98` | Leitor novo; inclui também os atos humanos permitidos, conforme achado 6. |

O cartório e o passivo são impactos comportamentais fora das três cópias de emissão nomeadas na allowlist; alterar o juiz compartilhado os alcança já no pouso A (`app/chamados/tests/test_o261_veto_na_porta.py:46`; `app/ponto/services/cartorio.py:1013`; `app/ponto/services/furos_sem_cobranca.py:96`).

Há ainda **erro novo no contador de vínculos**: dois furos sem vínculo → dois achados com `None` → `vinculos={None}` → publica **1 vínculo**, embora existam zero. O leitor foi tornado compatível com nulos na lista, mas não nessa agregação (`app/ponto/services/furos_vetados.py:61`; `app/ponto/management/commands/furos_vetados_por_regua.py:32`; `app/ponto/management/commands/furos_vetados_por_regua.py:33`).

Os comandos de medir e cobrar passivo herdam a mudança; `desfazer_corroboracao_regua` filtra por PK de vínculo existente, portanto os novos achados `None` não alteram esse filtro (`app/ponto/management/commands/furos_sem_cobranca_viva.py:14`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`).

Leitores de produção das três operações da porta alcançados pela mudança:

| Operação e leitor | Comportamento alterado |
|---|---|
| `abrir`: `app/ponto/management/commands/detectar_ausencias.py:688` | Nova recusa quando o vínculo efetivamente consultado pela porta diverge do recebido pelo emissor; retorna `VETO_NA_PORTA` (`app/ponto/management/commands/detectar_ausencias.py:732`). |
| `abrir`: `app/ponto/management/commands/reconciliar_grade.py:499` | Nascimento pode retornar `None`; renascimento recusado pode ser desfeito, conforme achado 1. |
| `abrir`: `app/chamados/reconciliador.py:862` | Pode deixar de emitir e costurar; trata `None` e chamado ainda encerrado (`app/chamados/reconciliador.py:885`; `app/chamados/reconciliador.py:888`). |
| `abrir`: `app/ponto/management/commands/processar_alertas_turno.py:335` | Muda emissão e efeitos do marco crítico; achados 4 e 5. |
| `abrir`: `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Pode deixar de nascer/reabrir; dry também passa a recusar e apply contabiliza `recusado_veto` (`app/ponto/management/commands/emitir_turno_aberto_retroativo.py:138`; `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:174`). |
| `criar`: `app/ponto/management/commands/processar_alertas_avancados.py:121` | Veto interrompe nascimento N5, push e criação do fio (`app/ponto/management/commands/processar_alertas_avancados.py:141`). |
| `_renascer`: `app/chamados/models.py:686` | Dedup pode preservar o encerramento, mas continua devolvendo o objeto e `False` (`app/chamados/models.py:688`). |
| `_renascer`: `app/chamados/services/resposta_tardia.py:45` | Resposta que reabria pode virar nota tardia; texto livre também gera Pauta DP (`app/chamados/services/resposta_tardia.py:52`; `app/chamados/services/resposta_tardia.py:58`). |

`bater_ponto()` também usa `criar` no universo, mas continua passando por falta de dia, conforme achado 3. Os dois serviços de abertura com módulo variável não oferecem esses cinco módulos pelo catálogo humano atual; **sem achado de quebra concreta dessa UI de abertura manual** (`app/ponto/views.py:291`; `app/chamados/services/abertura.py:47`; `app/chamados/services/abertura.py:172`; `app/chamados/catalogo/modulos.py:105`; `app/chamados/catalogo/modulos.py:182`; `app/chamados/catalogo/modulos.py:283`).

**(c) Selo AST: deixa passar mutantes plausíveis**

Os seguintes corpos passam sem registrar a escrita correspondente; a resolução de aliases é superficial, `save` não é examinado e `status_local` é procurado somente dentro da chamada (`app/chamados/tests/test_o261_veto_na_porta.py:98`; `app/chamados/tests/test_o261_veto_na_porta.py:137`; `app/chamados/tests/test_o261_veto_na_porta.py:165`; `app/chamados/tests/test_o261_veto_na_porta.py:170`).

| Mutante | Entrada → resultado |
|---|---|
| Alias transitivo | `from chamados.models import ChamadoColaborador as Ch; CC = Ch` e, numa função, `CC.objects.create(...)` → nascimento fora da porta não encontrado (`app/chamados/tests/test_o261_veto_na_porta.py:100`). |
| Import relativo de módulo | `from . import models as m` e `m.ChamadoColaborador.objects.create(...)` → raiz `m` desconhecida; escrita não encontrada (`app/chamados/tests/test_o261_veto_na_porta.py:92`; `app/chamados/tests/test_o261_veto_na_porta.py:168`). |
| Construtor + `save` | `ch = ChamadoColaborador(modulo_origem="turno_aberto", status_local="aberto", colaborador=c); ch.save()` → nascimento sem veto não encontrado (`app/chamados/tests/test_o261_veto_na_porta.py:28`; `app/chamados/tests/test_o261_veto_na_porta.py:170`). |
| `bulk_create` por manager | `manager = ChamadoColaborador.objects; manager.bulk_create([...])` → raiz desconhecida; escrita não encontrada (`app/chamados/tests/test_o261_veto_na_porta.py:168`). |
| `update(**kwargs)` | `valores = {"status_local": "aberto"}; ChamadoColaborador.objects.filter(pk=pk).update(**valores)` → reabertura não encontrada, pois o literal está fora da chamada (`app/chamados/tests/test_o261_veto_na_porta.py:143`). |

O selo complementar de status também não cobre construtor seguido de `save` nem esse `update(**valores)`; suas expressões procuram atribuição por atributo ou `update` com argumento literal (`app/chamados/tests/test_contract_status_nucleo.py:7`; `app/chamados/tests/test_contract_status_nucleo.py:8`).

A cópia externa do veto também pode esconder o pedido à porta: `abrir = ChamadoColaborador.abrir; if not motivo_do_veto(...): abrir(...)` não preenche `pede_porta`, porque a chamada é por nome. E os reabridores automáticos existentes por instância não são reconhecidos como escritores pelo selo (`app/chamados/tests/test_o261_veto_na_porta.py:163`; `app/chamados/tests/test_o261_veto_na_porta.py:166`; `app/chamados/tests/test_o261_veto_na_porta.py:170`).

**Sem achado** para import relativo direto `from .models import ChamadoColaborador as Ch`, alias simples de importação e `ChamadoColaborador.objects.bulk_create(...)` direto: esses formatos são reconhecidos (`app/chamados/tests/test_o261_veto_na_porta.py:92`; `app/chamados/tests/test_o261_veto_na_porta.py:170`).

**(d) Vacuidade dos testes da porta** [nao verificado]

**Sem achado de vacuidade por guarda anterior** nos casos negativos apresentados. `abrir` e renascimento têm controles positivos; no caso `criar`, a fixture não cria célula, e a guarda de absolvição retorna `None` sem célula — ela não explica sozinha o nascimento recusado (`app/chamados/tests/test_o261_veto_na_porta.py:255`; `app/chamados/tests/test_o261_veto_na_porta.py:279`; `app/chamados/tests/test_o261_veto_na_porta.py:295`; `app/chamados/tests/test_o261_veto_na_porta.py:338`; `app/chamados/models.py:865`).

Há, contudo, **lacuna de integração de `ec=None`**: o teste chama apenas `motivo_do_veto`; os demais casos mantêm vínculo. Mutante da porta que retornasse permissivamente quando `vinculo_do_emissor()` devolvesse `None` continuaria passando nesses casos, violando VETO-EC-NONE (`app/chamados/tests/test_o261_veto_na_porta.py:266`; `app/chamados/tests/test_o261_veto_na_porta.py:302`; `app/chamados/models.py:896`).

Também falta prova dos cinco módulos: os pedidos positivos do universo exercitam `turno_aberto`, `turno_aberto_24h` e `entrada_bloqueada_turno_aberto`; excluir `batida_ausente` e `intervalo_pendente` do universo não é distinguido por esses casos (`app/chamados/tests/test_o261_veto_na_porta.py:269`; `app/chamados/tests/test_o261_veto_na_porta.py:298`; `app/chamados/tests/test_o261_veto_na_porta.py:324`; `app/ponto/services/furos_vetados.py:71`).

Nas demais classes solicitadas: **sem achado de segundo juiz registrado**, relógio novo fora da borda ou inversão demonstrada de trava; as três cópias externas estão expressamente permitidas para B. Há consulta adicional de vínculo por tentativa com universo e dia, mas violação de teto de request/latência é **[nao verificado]** (`app/core/juizes.py:248`; `app/chamados/tests/test_o261_veto_na_porta.py:46`; `app/chamados/models.py:669`; `app/chamados/models.py:896`; `app/escala/servico_jornada.py:46`; `app/escala/servico_jornada.py:58`).

## 30/10 10:26 · sessao d69d66c93b62 · - · - · origem=Ronald (tmux)

1. **Selo AST incompleto.** `varrer_fonte` aceita os seguintes mutantes sem registrar escrita, confirmados por execução em memória; as causas estão na resolução superficial de aliases e na exigência de raiz conhecida (`app/chamados/tests/test_o261_veto_na_porta.py:98`, `:165`, `:170`):

   ```python
   from chamados.models import ChamadoColaborador as Ch
   CC = Ch
   def emite(c):
       CC.objects.create(colaborador=c, modulo_origem="turno_aberto")
   ```

   ```python
   from . import models as m
   def emite(c):
       m.ChamadoColaborador.objects.create(
           colaborador=c, modulo_origem="turno_aberto")
   ```

   ```python
   from chamados.models import ChamadoColaborador
   def emite(c):
       ch = ChamadoColaborador(
           colaborador=c, modulo_origem="turno_aberto",
           status_local="aberto")
       ch.save()
   ```

   ```python
   def emite(c):
       manager = ChamadoColaborador.objects
       manager.bulk_create([
           ChamadoColaborador(colaborador=c, modulo_origem="turno_aberto")
       ])
   ```

   Todos podem criar chamado sem consultar a porta; `save` nem pertence às operações examinadas, e o selo complementar também não reconhece argumento de construtor seguido de `save` (`app/chamados/tests/test_o261_veto_na_porta.py:28`, `:170`; `app/chamados/tests/test_contract_status_nucleo.py:7`, `:8`, `:23`).

2. **Reabertura por `update(**kwargs)` passa.** Entrada abaixo → `varrer_fonte` devolve `([], [])`, pois procura `status_local` apenas dentro da chamada; o dicionário está fora dela (`app/chamados/tests/test_o261_veto_na_porta.py:137`):

   ```python
   def vivifica(pk):
       valores = {"status_local": "aberto"}
       ChamadoColaborador.objects.filter(pk=pk).update(**valores)
   ```

   O selo complementar também não detecta esse formato (`app/chamados/tests/test_contract_status_nucleo.py:8`).

3. **Cópia do veto escondida por alias do método passa.** O selo exige chamada por atributo com raiz reconhecida; chamada por nome não preenche `pede_porta` (`app/chamados/tests/test_o261_veto_na_porta.py:163`):

   ```python
   def emite(c, ec, dia):
       abrir = ChamadoColaborador.abrir
       if not motivo_do_veto(c, ec, dia):
           abrir(colaborador=c, modulo_origem="turno_aberto")
   ```

4. **Import relativo direto e `bulk_create` direto: sem achado.** `from .models import ChamadoColaborador as Ch; Ch.objects.create(...)` e `ChamadoColaborador.objects.bulk_create(...)` foram detectados; os ramos correspondentes existem no selo (`app/chamados/tests/test_o261_veto_na_porta.py:92`, `:170`).

5. **Vacuidade dos testes negativos da porta: sem achado demonstrado.** O nascimento por `abrir` e o renascimento têm controles positivos (`app/chamados/tests/test_o261_veto_na_porta.py:279`, `:284`, `:320`, `:338`). Para `criar`, a fixture não cria célula e a guarda anterior retorna `None` sem célula (`app/chamados/tests/test_o261_veto_na_porta.py:255`; `app/chamados/models.py:865`). Extraindo os métodos em memória com dublês, o mesmo pedido de `turno_aberto_24h` executou `INSERT` sem o trecho novo e retornou `None` pela trilha do veto com ele; a execução Django completa permanece [nao verificado].

6. **Há lacuna de integração em `ec=None`, distinta de vacuidade.** O teste chama somente `motivo_do_veto`, enquanto os demais casos mantêm vínculo na fixture; um mutante que devolvesse `None` dentro de `_veto_da_regua` quando o vínculo faltasse não seria distinguido por esses casos (`app/chamados/tests/test_o261_veto_na_porta.py:266`, `:302`; `app/chamados/models.py:896`).

## 30/10 10:26 · sessao b8c5a39ce1e6 · - · - · origem=Ronald (tmux)

Censo dos leitores de produção alcançados pelo novo veto:

| Leitor | Efeito |
|---|---|
| `app/ponto/management/commands/detectar_ausencias.py:688` | `abrir` pode devolver `None`; guarda nova retorna `VETO_NA_PORTA` em `:732`. Com vínculo idêntico e cadastro estável, a cópia anterior já vetava; diferença surge se a autoridade consultada pela porta divergir do `ec` recebido (`:729`). |
| `app/ponto/management/commands/reconciliar_grade.py:499` | Nascimento recusado retorna `VETO_NA_PORTA` (`:519`), mas reabertura recusada pode ser desfeita pelo `reabrir` direto (`:521`). |
| `app/chamados/reconciliador.py:862` | Pode deixar de emitir/costurar; `None` agora retorna `False` (`:885`), encerrado já era recusado (`:888`). |
| `app/ponto/management/commands/processar_alertas_turno.py:335` | Pode parar de emitir `turno_aberto`; `_marco_critico` também deixa de registrar alerta, push e justificativa quando recebe `None` (`:316`). |
| `app/ponto/management/commands/emitir_turno_aberto_retroativo.py:151` | Pode parar de emitir/reabrir `turno_aberto_24h`; passa a contar `recusado_veto` (`:171`). |
| `app/ponto/management/commands/processar_alertas_avancados.py:121` | Pode parar de criar `turno_aberto_24h`; ramo `None` já interrompia push/fio (`:141`). |
| `app/chamados/models.py:686` | Dedup de encerrado pode preservar o encerramento: ignora o booleano de `_renascer_por_premissa_viva` e devolve o existente (`:688`). |
| `app/chamados/services/resposta_tardia.py:45` | Resposta que anteriormente reabria passa a nota tardia; texto livre também abre Pauta DP (`:52`, `:58`). A view passa a retornar `estado=resposta_tardia` (`app/chamados/views.py:1053`). |

`app/ponto/views.py:291` também chama `criar` para módulo do universo, mas seu contexto não contém dia (`:280`); portanto continua passando pelo retorno permissivo de `app/chamados/models.py:893`. Os dois serviços de abertura com módulo variável estão em `app/chamados/services/abertura.py:47` e `:172`; os cinco módulos do universo não estão declarados como abertura humana no catálogo (`app/chamados/catalogo/modulos.py:105`, `:182`). Não sustento quebra concreta da UI de abertura manual nesses serviços.

Achados:

1. **Reabertura automática contorna a recusa da porta na grade.** `app/ponto/management/commands/reconciliar_grade.py:521` chama `ch.reabrir()` depois de `abrir` devolver um existente encerrado; o booleano da recusa automática foi descartado em `app/chamados/models.py:686`. Contraexemplo sem corrida: chamado `batida_ausente` com JSON `data_turno=d`, porém `celula_id` de `d2`; vínculo saudável em `d`, dia `d2` ainda furado e vetado. A grade passa seu veto para `d`; o dedup encontra o chamado; o juiz do dia prioriza a célula (`app/chamados/juizes.py:89`), então `_renascer` veta `d2` (`app/chamados/models.py:778`). A grade recebe o encerrado e o reabre diretamente (`app/ponto/management/commands/reconciliar_grade.py:522`). Ocorrência desse dado em produção: [nao verificado].

2. **Há outro reabridor automático sem veto.** O callback de fechamento pode reabrir `_d.chamado` sem filtro de módulo e sem `_renascer_por_premissa_viva` (`app/chamados/services/disputa_emissao.py:1598`, `:1606`); esse callback roda por receiver (`app/chamados/signals.py:76`, `:92`). Contraexemplo: disputa aberta com pai `turno_aberto` encerrado em dia vetado, contendo resposta que `irmas_respondidas_sem_consumo` reconhece; fechamento de um destino da disputa → `_pai.reabrir()` → pai vivo, desde que as guardas comuns permitam (`app/chamados/services/disputa_emissao.py:1604`; `app/chamados/models.py:1119`). O veto novo não participa dessa operação (`app/chamados/models.py:1092`).

3. **Nascimento automático do quinto módulo continua sem veto.** Entrada com turno anterior ≥14h cria `entrada_bloqueada_turno_aberto` com somente `pendentes` e `total` (`app/ponto/views.py:275`, `:280`, `:291`). Contraexemplo: colaborador com régua suspeita ou sem vínculo e contexto de bloqueio → `dia_do_fato(fortes=True)` não encontra dia → `_veto_da_regua` retorna `None` → chamado nasce (`app/chamados/models.py:892`). O código passa fisicamente pela porta, mas escapa da invariante declarada; os detalhes datados das entradas são reduzidos a resumo textual antes da chamada (`app/ponto/views.py:278`).

4. **“Sem vínculo já calava” não garante DIFF zero no processador de turno.** O comando só exige vínculo quando `tm.dt_fim_previsto is None` (`app/ponto/management/commands/processar_alertas_turno.py:100`). Contraexemplo: turno materializado aberto com `dt_fim_previsto` preenchido, colaborador sem vínculo vigente, janela `pos_20`, sem ausência cobrindo → código anterior alcança `abrir`; novo código veta `ec=None` (`app/ponto/management/commands/processar_alertas_turno.py:138`, `:335`; `app/chamados/models.py:896`; `app/ponto/services/furos_vetados.py:32`). Existência desse estado na frota: [nao verificado].

Classificação dos demais `reabrir` diretos:

- `app/chamados/services/adesao.py:92`: automático, porém pai `sem_previsao_escala`, fora do universo (`:75`).
- `app/ponto/management/commands/detectar_ausencias.py:301`: automático, porém `ancora_limbo`, fora do universo (`:273`).
- `app/chamados/management/commands/reabrir_orfas_enterradas.py:161`: comando humano de uma vez, com `--apply`, dentro da exceção humana; não deve ser classificado como cron automático (`:1`, `:17`, `:48`).
- `app/chamados/services/competencia_trancada.py:258`: reabertura de período pelo DP, explicitamente humana (`:259`).
- `app/chamados/views_arquivar.py:68` e `app/chamados/services/acoes_disputa.py:266`: atos humanos; passam, mas seus ramos não consultam nem apresentam o motivo do veto (`app/chamados/views_arquivar.py:63`; `app/chamados/services/acoes_disputa.py:245`).

Não encontrei novo `None` causando exceção em leitor concreto do universo: os três emissores de batida ganharam guardas, o avançado já tinha guarda e o marco crítico já testa `None` (`app/ponto/management/commands/detectar_ausencias.py:732`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/chamados/reconciliador.py:885`; `app/ponto/management/commands/processar_alertas_avancados.py:141`; `app/ponto/management/commands/processar_alertas_turno.py:317`).

## 30/10 10:25 · sessao be62f58a56fd · - · - · origem=Ronald (tmux)

Censo de leitores de produção de `motivo_do_veto`:

- `ChamadoColaborador._veto_da_regua`: nova consulta da porta; nasce/reabre automaticamente apenas sem veto, salvo ausência de dia. `app/chamados/models.py:888`, `app/chamados/models.py:893`, `app/chamados/models.py:896`.
- `reemitir_chamado_do_dia`: `ec=None` passa de ausência de veto para retorno `False` antes de `abrir`, quando não costura num vivo. `app/chamados/reconciliador.py:852`, `app/chamados/reconciliador.py:854`, `app/chamados/reconciliador.py:855`.
- `furos_vetados_por_regua`: dias `furo` sem vínculo entram na lista, com vínculo e corroborada nulos. `app/ponto/services/furos_vetados.py:53`, `app/ponto/services/furos_vetados.py:57`, `app/ponto/services/furos_vetados.py:61`.
- `julgar_colab._veto_do_dia`: muda impressão dos dias sem vínculo e provoca rejulgamento, mesmo sem mudança dos outros insumos. `app/ponto/services/cartorio.py:1013`, `app/ponto/services/cartorio.py:1083`, `app/ponto/services/cartorio.py:1092`.
- `furos_sem_cobranca.medir`: muda classificação para `veto`, retirando do total; `cobrar` pula o item. `app/ponto/services/furos_sem_cobranca.py:96`, `app/ponto/services/furos_sem_cobranca.py:109`, `app/ponto/services/furos_sem_cobranca.py:125`.
- Contador novo: consulta todos os vivos com dia, incluindo atos humanos. `app/ponto/services/furos_vetados.py:87`, `app/ponto/services/furos_vetados.py:98`.

Achados:

1. **Censo comportamental incompleto; “DIFF zero por construção” não decorre do código.** Entrada: célula já julgada, `ec=None`, demais insumos iguais → o hash antes omitia veto; depois acrescenta `veto:VETO_DEGENERADA`, invalida o pulo e rejulga. Extraí e executei apenas `impressao_insumos` em memória: hashes diferentes. Também célula ativa elegível ao passivo, sem vivo e sem vínculo → antes `pergunta_ao_colab`/`supervisao_sem_push`; depois `veto`, total diminui e cobrança é pulada. Os números reais são [nao verificado]. `app/ponto/services/furos_vetados.py:32`; `app/ponto/services/cartorio.py:329`; `app/ponto/services/cartorio.py:1083`; `app/ponto/services/cartorio.py:1092`; `app/ponto/services/furos_sem_cobranca.py:96`; `app/ponto/services/furos_sem_cobranca.py:107`; `app/ponto/services/furos_sem_cobranca.py:109`; `app/ponto/services/furos_sem_cobranca.py:125`; `app/docs/CORTES.md:401`.

2. **Contador de vínculos passa a inventar um vínculo.** Entrada: quaisquer dois furos sem vínculo → achados têm `vinculo=None`; comando faz `{None}` e publica `furos_vetados_por_regua_vinculos=1`, embora existam zero vínculos. É impacto novo num leitor não ajustado. `app/ponto/services/furos_vetados.py:61`; `app/ponto/management/commands/furos_vetados_por_regua.py:32`; `app/ponto/management/commands/furos_vetados_por_regua.py:33`.

3. **Reabertura automática da grade contorna a recusa.** Entrada: chamado resolvido com premissa viva; `ec` fornecido ao emissor saudável; vínculo retornado pela porta suspeito → veto externo passa, `abrir` tenta renascer, recusa e devolve o encerrado; grade chama `reabrir()` diretamente. O `if ch is None` novo não alcança esse caso. Diferença de vínculos pode ser entrada explícita ou mudança entre carga e emissão; ocorrência em produção [nao verificado]. `app/ponto/management/commands/reconciliar_grade.py:480`; `app/ponto/management/commands/reconciliar_grade.py:487`; `app/ponto/management/commands/reconciliar_grade.py:491`; `app/ponto/management/commands/reconciliar_grade.py:499`; `app/chamados/models.py:685`; `app/chamados/models.py:688`; `app/chamados/models.py:778`; `app/ponto/management/commands/reconciliar_grade.py:519`; `app/ponto/management/commands/reconciliar_grade.py:521`.

4. **Vigia não distingue violação automática de ato humano autorizado.** Entrada: chamado encerrado em dia vetado, reaberto pelo admin → passa, mas contador sobe para 1. A própria docstring admite adiamento da trilha humana ao B; o aval exige trilha com motivo e apresentação do veto. O teste exercita somente `reabrir()` sem verificar essa condição. `app/ponto/services/furos_vetados.py:82`; `app/ponto/services/furos_vetados.py:87`; `app/ponto/services/furos_vetados.py:98`; `app/chamados/tests/test_o261_veto_na_porta.py:335`; `app/chamados/tests/test_o261_veto_na_porta.py:336`; `app/docs/CORTES.md:401`.

Leitores transitivos: os comandos `furos_sem_cobranca_viva` e `cobrar_furos_sem_cobranca` herdam a mudança do passivo; `desfazer_corroboracao_regua` filtra os achados por PK existente, portanto os novos `None` não alteram seu resultado. `app/ponto/management/commands/furos_sem_cobranca_viva.py:14`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:19`; `app/ponto/management/commands/cobrar_furos_sem_cobranca.py:30`; `app/escala/management/commands/desfazer_corroboracao_regua.py:38`.

Segundo juiz registrado: **sem achado** além das três cópias expressamente autorizadas no lote B; `regua_degenerada` é subpredicado e `vetar_cobranca_se_degenerada` conserva o efeito de cadastro. Relógio novo fora da borda: **sem achado**. Inversão de trava: **sem achado demonstrado**; vínculo é carregado por SELECT comum após a trava do colaborador. Custo: há nova consulta de vínculo por tentativa no universo com dia; impacto de latência/teto [nao verificado]. `app/core/juizes.py:248`; `app/chamados/tests/test_o261_veto_na_porta.py:46`; `app/escala/regua_defesa.py:16`; `app/escala/regua_defesa.py:45`; `app/escala/regua_defesa.py:51`; `app/chamados/models.py:669`; `app/chamados/models.py:731`; `app/escala/servico_jornada.py:46`; `app/escala/servico_jornada.py:58`.

## 30/10 10:22 · sessao 07fb1c737e04 · FIO-MORTO-DONO-NA-LISTA · verificador · origem=Code

- [P2] O teste do universo não protege o cruzamento com o recorte: prepara somente uma empresa visível e depois a oculta (`app/chamados/tests/test_o261_vigia_fio.py:606`, `app/chamados/tests/test_o261_vigia_fio.py:612`); `_censo()` recorta pela mesma empresa (`app/chamados/tests/test_o261_vigia_fio.py:112`). Nesse cenário, retirar a interseção preserva os resultados esperados (`app/chamados/services/vigia_fio.py:257`). Falta uma segunda empresa visível fora do recorte para detectar essa regressão (`app/chamados/tests/test_o261_vigia_fio.py:611`).

## 30/10 10:22 · sessao ffef86d5af39 · - · - · origem=Ronald (tmux)

Achado de cobertura: `app/chamados/tests/test_o261_vigia_fio.py:606-616` não cria empresa visível fora do recorte; `_censo()` restringe sempre à própria fixture (`app/chamados/tests/test_o261_vigia_fio.py:112`). Assim, remover o cruzamento em `app/chamados/services/vigia_fio.py:257-258` pode preservar ambos os resultados esperados. Acrescentar uma segunda empresa visível fora do recorte faria esse caso morder essa regressão.

Os testes têm sinal positivo: universo exige `{self.emp.pk}` (`app/chamados/tests/test_o261_vigia_fio.py:611`); falha exige uma chamada e três dias preservados (`app/chamados/tests/test_o261_vigia_fio.py:592-598`). Execução: [não verificado].

## 30/10 10:22 · sessao 77d199c1f9eb · - · - · origem=Ronald (tmux)

SEM ACHADO.

## 30/10 10:19 · sessao 11eb35b50725 · FIO-MORTO-DONO-NA-LISTA · verificador · origem=Code

**Não aprovaria o diff ainda:** há divergências de universo e contrato entre o censo e a tela, além de repetição da lista quando ela falha (`app/chamados/services/vigia_fio.py:253`; `app/escala/views.py:490`; `app/escala/services/cadastro_realidade.py:588`).

1. **P1 — A frota pode retirar do alarme um dia invisível na tela.** No cron, `empresas=None` chega à lista, significando todas; a tela passa apenas `empresas_visiveis()`, que exclui inativas e empresas de diagnóstico. Portanto, uma C9 dessas empresas pode virar `fio_do_admin` sem estar na lista apresentada ao admin. Ocorrência em produção: **[nao verificado]**. (`app/chamados/management/commands/vigiar_fio_morto.py:148`; `app/chamados/services/vigia_fio.py:253`; `app/escala/services/cadastro_realidade.py:504`; `app/escala/views.py:490`; `app/colaboradores/queries.py:85`.)

2. **P2 — `--empresa` passa objetos onde a lista compara IDs.** O comando monta uma lista de objetos `Empresa`; o vigia a repassa diretamente. A filtragem final da lista compara `empresa_id` inteiro com esses objetos e descarta as linhas. Assim, um dia transferível na frota pode continuar morto no recorte. O próprio helper dos testes passa `[self.emp]`, mas o mock da lista esconde essa incompatibilidade. (`app/chamados/management/commands/vigiar_fio_morto.py:152`; `app/chamados/services/vigia_fio.py:253`; `app/escala/services/cadastro_realidade.py:484`; `app/escala/services/cadastro_realidade.py:588`; `app/chamados/tests/test_o261_vigia_fio.py:112`; `app/chamados/tests/test_o261_vigia_fio.py:560`.)

3. **P2 — Falha da lista multiplica trabalho por dia candidato.** O cache só recebe o resultado depois do sucesso; uma exceção deixa `_na_lista` vazio e o próximo dia executa a lista inteira novamente. Na contraprova sintética em memória, três candidatos produziram três chamadas, três registros e `sem_resposta=3`. O teste novo usa uma única célula, portanto não detecta essa repetição. (`app/chamados/services/vigia_fio.py:251`; `app/chamados/services/vigia_fio.py:263`; `app/chamados/tests/test_o261_vigia_fio.py:584`.)

4. **P2 — O relógio continua fora da borda.** O comando captura `hoje`, chama o censo sem `agora`, o censo consulta `timezone.now()` e o carimbo consulta outro instante. Uma rodada atravessando meia-noite pode combinar referências temporais diferentes. O helper fixa apenas `hoje`, embora declare não ler relógio real. (`app/chamados/management/commands/vigiar_fio_morto.py:158`; `app/chamados/management/commands/vigiar_fio_morto.py:195`; `app/chamados/services/vigia_fio.py:182`; `app/chamados/tests/test_o261_vigia_fio.py:104`.)

5. **P2 — O ESTADO não mostra o passivo transferido.** O carimbo preserva o censo inteiro, mas `fio_estado.ler()` descarta `fio_do_admin`; sua frase publica morto/cobrando/futuro. Pode aparecer “fio morto: 0” sem informar quantos dias passaram ao admin. Não há quebra de parsing demonstrada; há perda de informação na apresentação. (`app/chamados/management/commands/vigiar_fio_morto.py:204`; `bin/fio_estado.py:82`; `bin/fio_estado.py:101`; `bin/gerar_estado.py:261`.)

Sobre **“o dia está na lista”**, o helper não verifica o que a tela efetivamente imprime: aceita qualquer código com `data` e aplica `str(...)[:10]`; o HTML imprime `detalhe` ou ocorrências. Uma assinatura sintética A2 com data oculta é aceita; `25/10/2026` não é normalizado para a chave ISO usada pelo vigia. Entretanto, o produtor atual da C9 escreve ISO e repete a data no detalhe; outro produtor real com esse defeito é **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:667`; `app/templates/escala/cadastro_x_realidade.html:28`; `app/chamados/services/vigia_fio.py:254`; `app/ponto/services/teto_na_lista.py:79`; `app/escala/services/cadastro_realidade.py:314`.)

No sentido inverso, C4/C5 nomeiam até três dias no detalhe, sem campo `data`, e são ignoradas pelo helper. A implementação reconhece atualmente os dias estruturados da C9; elegibilidade de casos C4/C5 pelo dono em produção é **[nao verificado]**. A C9 não recebe novo recorte por `hoje`: lê a última lavra disponível, como a tela. (`app/escala/services/cadastro_realidade.py:142`; `app/escala/services/cadastro_realidade.py:373`; `app/escala/services/cadastro_realidade.py:667`; `app/escala/services/cadastro_realidade.py:306`; `app/ponto/services/teto_na_lista.py:120`.)

O **dono do novo ramo permanece delegado**, sem segunda derivação identificada: `InsumosDoDono.dono()` termina no juiz existente, e somente BATIDA/CADASTRO entram no contador. A ausência posterior na lista devolve o candidato ao caminho de `fio_morto`. (`app/chamados/services/vigia_fio.py:254`; `app/chamados/services/vigia_fio.py:262`; `app/chamados/services/vigia_fio.py:324`; `app/ponto/management/commands/e6_oraculo.py:146`.)

O **custo novo inclui cálculo**: a lista completa executa seus leitores; por colaborador/janela, `InsumosDoDono` lê batidas, monta o oráculo e carrega turnos/cadastro. Se precisar de CXR, pede o espelho, que pode executar o motor, inclusive num intervalo até `hoje`. O cache é local à rodada; `limite` limita amostras de IDs. O cron tem teto de 900 segundos, mas a anotação de 0,9 segundo é anterior à mudança. Custo efetivo atual: **[nao verificado]**. (`app/escala/services/cadastro_realidade.py:513`; `app/chamados/services/vigia_fio.py:242`; `app/ponto/management/commands/e6_oraculo.py:118`; `app/ponto/management/commands/e6_oraculo.py:136`; `app/ponto/services/espelho.py:1231`; `app/ponto/services/espelho.py:1183`; `app/config/crons.py:1650`.)

A exceção externa **mantém o dia no alarme**, mas falhas internas da lista, inclusive C9, retornam lista parcial sem incrementar `fio_do_admin_sem_resposta`. Os testes substituem a lista e usam um `_Ins` que ignora janela, `hoje` e `agora`; a propriedade de soma inclui a chave nova, porém não constrói explicitamente um caso positivo nela. Isso deixa sem cobertura justamente universo, argumentos, custo e falha parcial. (`app/chamados/services/vigia_fio.py:263`; `app/escala/services/cadastro_realidade.py:561`; `app/chamados/tests/test_o261_vigia_fio.py:553`; `app/chamados/tests/test_o261_vigia_fio.py:560`; `app/chamados/tests/test_o261_vigia_fio.py:628`.)

Os demais leitores examinados não demonstram quebra pela chave: alarmes usam campos existentes; `materializar_passivo_validado` incorpora contadores dinamicamente. `fora_da_folha` não lê o censo e, nas mesmas entradas, o dia transferido continua tendo chamado vivo: fica como `chamado`, grupo C, com orientação genérica de resolver o chamado. (`app/chamados/management/commands/vigiar_fio_morto.py:80`; `app/chamados/management/commands/materializar_passivo_validado.py:208`; `app/chamados/services/vigia_fio.py:187`; `app/folha/fora_da_folha.py:53`; `app/folha/fora_da_folha.py:71`; `app/folha/fora_da_folha.py:41`.)

Execução da suíte, RED histórico, impacto nominal em produção e identidade Git `f0d319ba..HEAD`: **[nao verificado]**.

## 30/10 10:17 · sessao 34d0a9b6d129 · - · - · origem=Ronald (tmux)

Achados da lista/universo:

1. **P1 — Frota pode suprimir dia que a tela não mostra.** `censo()` com `empresas=None` chama `lista(..., empresas=None)`, cuja definição admite todas; a tela sempre usa IDs de `empresas_visiveis()`, que exclui empresas inativas e CNPJ de diagnóstico. Portanto o mesmo serviço é usado, mas com universos diferentes: uma C9 dessas empresas pode gerar `fio_do_admin` sem aparecer na tela. Existência desses casos em produção: **[nao verificado]**. Fontes: `app/chamados/services/vigia_fio.py:253`, `app/escala/services/cadastro_realidade.py:502-504`, `app/escala/services/cadastro_realidade.py:588-589`, `app/escala/views.py:490`, `app/colaboradores/queries.py:78-87`.

2. **P1/P2 — `--empresa` quebra a equivalência com a frota.** O comando passa lista de objetos `Empresa`; o novo caminho repassa essa lista para `lista()`, cuja filtragem final compara o inteiro `v['empresa_id']` aos objetos. Toda linha desse recorte é descartada, de modo que `fio_do_admin` fica zero no recorte mesmo quando o mesmo dia poderia sair na frota. Fontes: `app/chamados/management/commands/vigiar_fio_morto.py:148-159`, `app/chamados/services/vigia_fio.py:253`, `app/escala/services/cadastro_realidade.py:484`, `app/escala/services/cadastro_realidade.py:588-589`.

3. **Contrato de dia mais amplo que o da tela.** `dias_na_lista()` aceita qualquer código com `data` e só corta `str(data)[:10]`; HTML e PDF imprimem `detalhe`/`ocorrencias`, não `data`. Uma assinatura que tenha `data` sem nomear o dia no detalhe pode tirar um dia do alarme que a tela não mostra como dia. Isso é demonstrável pelo contrato do helper, porém produtor atual desse formato fora da C9: **[nao verificado]**. A C9 atual é consistente: sua lavra escreve ISO e seu detalhe repete a data. Fontes: `app/escala/services/cadastro_realidade.py:663-668`, `app/templates/escala/cadastro_x_realidade.html:28`, `app/escala/services/cadastro_realidade.py:709-711`, `app/ponto/services/teto_na_lista.py:79`, `app/escala/services/cadastro_realidade.py:312-317`.

4. **Sentido oposto: dias nomeados em C4/C5 não são reconhecidos.** As duas assinaturas mostram até três datas em `detalhe`, sem campo `data`; o helper ignora todas. Assim, hoje ele reconhece efetivamente apenas dias C9, não todos os dias nominalmente apresentados na lista. Caso concreto elegível BATIDA/CADASTRO em produção: **[nao verificado]**. Fontes: `app/escala/services/cadastro_realidade.py:142-143`, `app/escala/services/cadastro_realidade.py:373-376`, `app/escala/services/cadastro_realidade.py:667-668`.

5. **A lista não aplica nova janela à C9.** Ela lê `ultimo_lavrado()` sem `hoje`, sem recortar os itens e sem verificar frescor; a janela de três competências existe no produtor da lavra, não no leitor. Isso é a mesma política da tela, mas chamar `lista(hoje=...)` não garante C9 reconstruída naquele `hoje`. Fontes: `app/escala/services/cadastro_realidade.py:306-318`, `app/ponto/services/teto_na_lista.py:25-36`, `app/ponto/services/teto_na_lista.py:120-130`.

6. **Relógio fora da borda também dentro da lista.** `_jornada_que_mente()` lê `date.today()` e usa competência com 21 fixo; `_afastado_sem_registro(..., hoje)` chama `afastados_hoje()` sem repassar `dia`, que lê `timezone.localdate()`. Portanto repassar `hoje` a `lista()` não fixa toda sua leitura temporal. Esses ramos não produzem `data` e por isso atualmente não alimentam `dias_na_lista`; são custo/inconsistência temporal da chamada completa. Fontes: `app/escala/services/cadastro_realidade.py:186-189`, `app/escala/services/cadastro_realidade.py:350`, `app/colaboradores/services/situacao_exibida.py:37-46`, `app/escala/services/cadastro_realidade.py:667-668`.

7. **Dono permanece delegado.** O novo caminho chama `InsumosDoDono(...).dono(data)`; esse método chama `dono_da_divergencia`, com o motor apenas quando necessário. Não identifiquei juiz paralelo de dono nesse trecho. Fontes: `app/chamados/services/vigia_fio.py:256-262`, `app/ponto/management/commands/e6_oraculo.py:136-155`.

O hash Git da cópia não foi validado: o diretório não contém repositório Git acessível; comparação histórica `f0d319ba..HEAD`: **[nao verificado]**.

## 30/10 10:17 · sessao ab8241ccf0ab · - · - · origem=Ronald (tmux)

Confirmo e retiro o achado “SEM FIO/D”: todo dia transferido tem chamado vivo no universo do censo; a folha lê o mesmo `VIVOS` e retorna `chamado`, grupo C (`app/chamados/services/vigia_fio.py:187`, `app/chamados/services/vigia_fio.py:229`, `app/folha/fora_da_folha.py:53`, `app/folha/fora_da_folha.py:71`, `app/folha/fora_da_folha.py:83`).

A diferença restante é de apresentação: a folha recomenda “resolver o chamado N”, sem apontar Cadastro x Realidade; isso não comprova quebra e merece apenas observação (`app/folha/fora_da_folha.py:41`, `app/folha/fora_da_folha.py:204`, `app/folha/fora_da_folha.py:209`).

## 30/10 10:17 · sessao 1dc07376d59c · - · - · origem=Ronald (tmux)

Achados no eixo juiz/custo/testes: [nao verificado]

- **P2 — lista completa é repetida em cada dia se falhar.** `_na_lista.append(...)` só grava depois de `lista()` e `dias_na_lista()` retornarem; qualquer exceção deixa `_na_lista` vazio, e o próximo candidato tenta a lista inteira novamente. A falha continua contando por célula e registrando traceback, apesar do comentário “UMA vez por censo”. Cachear também a falha evitaria multiplicar custo e logs. `app/chamados/services/vigia_fio.py:251-265`.

- **P2 — novo relógio fora da borda.** O comando captura `hoje` e chama `censo` sem `agora`; a nova linha de `censo` consulta `timezone.now()` dentro do serviço. Assim, em virada de dia, `hoje` e `agora` podem pertencer a dias diferentes. O comando ainda captura outro instante para `medido_em`. `app/chamados/management/commands/vigiar_fio_morto.py:158-159`, `app/chamados/services/vigia_fio.py:181-182`, `app/chamados/management/commands/vigiar_fio_morto.py:195`.

- **P2 — selo novo não alcança a costura real nem os argumentos.** O mock da lista já entrega C9 com data ISO; `_Ins.__init__(c, *a, **k)` descarta janela, `hoje` e `agora`. Portanto, argumentos errados, universo incompatível e relógio incorreto podem conservar o teste verde. O teste de exceção usa só uma célula e também não detecta repetição da lista em falha. `app/chamados/tests/test_o261_vigia_fio.py:548-561`, `app/chamados/tests/test_o261_vigia_fio.py:568-588`.

- **P2 — selo descrito como sem relógio agora lê o relógio.** `_censo()` fixa apenas `hoje`, embora sua docstring diga “nenhum caminho deste selo le o relogio real”; o novo default `agora` consulta o relógio a cada chamada. `app/chamados/tests/test_o261_vigia_fio.py:104-112`, `app/chamados/services/vigia_fio.py:182`.

O dono continua vindo do juiz existente: o vigia chama `InsumosDoDono.dono`, que termina em `dono_da_divergencia`; não encontrei segunda derivação do dono nesse trecho. `app/chamados/services/vigia_fio.py:257-262`, `app/ponto/management/commands/e6_oraculo.py:146-155`.

O custo novo não é somente leitura barata de lavra: por `(colaborador, início da janela)`, o construtor lê batidas e monta o oráculo; `dono()` carrega turnos/cadastro e, quando a resposta sem CXR seria ESTRUTURA, pode pedir espelho e executar apuração/motor. Os caches são locais ao censo; `limite` só limita listas de IDs. `app/chamados/services/vigia_fio.py:171-172`, `app/chamados/services/vigia_fio.py:242-261`, `app/ponto/management/commands/e6_oraculo.py:118-155`, `app/ponto/services/espelho.py:1229-1231`, `app/ponto/services/espelho.py:1166-1184`.

O cron tem teto de 900 segundos; a medição citada de 0,9 segundo/3 queries é histórica e anterior à costura nova. Duração, número de consultas e quantidade efetiva de motores por rodada em produção: **[nao verificado]**. `app/config/crons.py:1650-1656`.

A exceção externa preserva o dia no alarme e registra a falha, portanto não absolve silenciosamente aquele candidato. Contudo, falhas internas da lista, inclusive C9, são engolidas pela própria `lista()` e retornam uma lista parcial; nesses casos o novo `fio_do_admin_sem_resposta` não percebe a falha. `app/chamados/services/vigia_fio.py:263-266`, `app/escala/services/cadastro_realidade.py:561-567`.

## 30/10 10:17 · sessao ab8241ccf0ab · - · - · origem=Ronald (tmux)

Achados e compatibilidade dos leitores:

- **P2 — ESTADO oculta a transferência para o admin.** O carimbo guarda o `censo` inteiro, portanto preserva `fio_do_admin` e eventual `fio_do_admin_sem_resposta` (`app/chamados/management/commands/vigiar_fio_morto.py:204`). O leitor devolve uma seleção fixa sem essas chaves e a frase só mostra morto/cobrando/futuro; o ESTADO usa essa frase diretamente (`bin/fio_estado.py:82`, `bin/fio_estado.py:101`, `bin/gerar_estado.py:260`). Consequência inferida: transferir todos os dias ao admin permite mostrar “fio morto: 0” sem mostrar o passivo transferido; ocorrência em produção [nao verificado].

- **P2 — ferramenta da folha continua dizendo “SEM FIO” para casos que o novo censo pode chamar de fio do admin.** Ela possui classificação própria baseada apenas em pergunta viva e chamado vivo, sem consultar presença na lista ou dono L-099 (`app/folha/fora_da_folha.py:59`). Sem ambos, devolve `nada`, classe D e o texto “ninguém está esperando ninguém” (`app/folha/fora_da_folha.py:73`, `app/folha/fora_da_folha.py:81`, `app/folha/fora_da_folha.py:42`). Essa classificação alimenta tanto `fora_da_folha()` quanto `o_que_segura()` (`app/folha/fora_da_folha.py:154`, `app/folha/fora_da_folha.py:204`). A divergência é possível pelo código; existência de caso concreto no dado fornecido [nao verificado].

- **Relógio fora da borda:** o command captura `hoje` e chama `censo()` sem fornecer `agora`; o novo censo lê `timezone.now()` por conta própria (`app/chamados/management/commands/vigiar_fio_morto.py:158`, `app/chamados/services/vigia_fio.py:182`). O carimbo lê ainda outro instante em `timezone.localtime()` (`app/chamados/management/commands/vigiar_fio_morto.py:195`). Portanto o comentário “UMA leitura de relógio na rodada” não descreve a execução (`app/chamados/management/commands/vigiar_fio_morto.py:154`). Recomendo capturar `agora` na borda, derivar `hoje` dele e usá-lo no censo/carimbo — recomendação baseada nesses pontos.

- **Command compatível com a nova chave:** os alarmes continuam lendo chaves existentes; `fio_do_admin` fica fora de `ESPERADO`, `ALARMA_SEMPRE` e `ALARMA_EM_0`, coerente com o novo bucket não alarmar (`app/chamados/management/commands/vigiar_fio_morto.py:80`, `app/chamados/management/commands/vigiar_fio_morto.py:86`, `app/chamados/management/commands/vigiar_fio_morto.py:160`, `app/chamados/services/vigia_fio.py:314`). O cron passa `--alarme --alarme-passivo` e tem teto de 900 segundos (`app/config/crons.py:1654`).

- **Outro consumidor real é compatível:** `materializar_passivo_validado._juizes()` chama o censo e inclui dinamicamente todo contador `int/bool`, portanto acolhe as novas chaves sem alteração de schema (`app/chamados/management/commands/materializar_passivo_validado.py:201`, `app/chamados/management/commands/materializar_passivo_validado.py:208`).

- **Selo do ESTADO não cobre o novo bucket:** o carimbo sintético não contém `fio_do_admin`, nem `fio_do_admin_sem_resposta`; os casos verificam esperado, idade, ausência, caminho e cegueira (`bin/tests/test_fio_estado.sh:40`, `bin/tests/test_fio_estado.sh:56`, `bin/tests/test_fio_estado.sh:104`). O teste do censo atualiza a soma com o novo bucket, mas sua mistura não constrói explicitamente um dia que entre nele (`app/chamados/tests/test_o261_vigia_fio.py:628`, `app/chamados/tests/test_o261_vigia_fio.py:645`).

Execução de testes e efeitos reais em produção [nao verificado]. [nao verificado]

## 30/10 10:00 · sessao bd9f616654d5 · O261-NOTURNO · censo · origem=Code

O campo recebe **três referências diferentes de dia**: dia da célula/entrada, dia do marco de saída e dia civil da tentativa. O helper persiste o valor recebido, sem converter entre essas referências. (`app/chamados/services/fabrica_da_celula.py:122`; `app/chamados/detector_anomalias.py:43-49`; `app/ponto/services/triagem_batida.py:625-656`; `app/chamados/services/disputa_emissao.py:491-498`)

**Escritores de produção de `contexto[motivo]['data']`:**

| Escritor / montagem | Qual dia grava | Arquivo:linha |
|---|---|---|
| `detector_anomalias.detectar_anomalias`, motivo `orfao_14h` | **MARCO: saída esperada** da entrada pendente selecionada. Grava ISO ou `None` quando não obtém a saída esperada. | `app/chamados/detector_anomalias.py:35-49` |
| `fabrica_da_celula.perguntar`, para cada motivo recebido | Literalmente `dia.isoformat()`. No julgamento, o argumento é **DIA DA CÉLULA**, `cel.data`. | `app/chamados/services/fabrica_da_celula.py:118-124,143-149` |
| Command `disparar_perguntas_competencia` → mesma fábrica | **DIA DA CÉLULA** nos dois caminhos: `c.data` nos alvos da célula; `ch.celula.data` no fio mudo. Ambos chegam a `perguntar(colab, dia, [motivo])`. | `app/chamados/management/commands/disparar_perguntas_competencia.py:111-123,165-168,225-226,244,276-289` |
| `disputa_emissao.perguntar_entradas_pendentes` | **DIA DA ENTRADA**: extrai `entradas_pendentes[*]['data']`, classifica esse dia e escreve em `contexto[_motivo]['data']`. A lista original usa a data local da entrada do turno aberto. Salva o contexto somente se `criadas` ficou não vazia. | `app/ponto/utils_turno_orfao.py:31-42`; `app/chamados/services/disputa_emissao.py:255-276` |
| `api/views.py::api_bater_ponto` | **DIA CIVIL LOCAL DA TENTATIVA/RECEPÇÃO**: `agora_local.date()`, derivado de `timezone.now()`. Escreve sob **`saida_sem_entrada`**, embora acrescente **`orfao_14h`** à lista de motivos. | `app/api/views.py:527-528,652-658,777-779` |
| `api/views_core.py::api_bater_ponto` | Mesma referência: **DIA CIVIL LOCAL DA TENTATIVA/RECEPÇÃO**, com a mesma diferença entre motivo acrescentado e chave escrita. | `app/api/views_core.py:519-520,649-655,777-779` |
| `triagem_batida.ramo_registrar_es` | **DIA CIVIL LOCAL DA TENTATIVA**, calculado de `timezone.now()`. Aqui motivo e chave são ambos `saida_sem_entrada`. | `app/ponto/services/triagem_batida.py:625-626,648-660` |

No detector, `escala/utils.py::data_saida_esperada` ancora os marcos no dia local da **entrada**, calcula o último marco encadeado e retorna `fim.date()`. O encadeamento avança dias enquanto o próximo marco for `<=` ao anterior; por isso a saída noturna pode ser **D+1**. (`app/escala/utils.py:891-909,920-931`)

`criar_ou_atualizar_disputa` é o **persistidor dos contextos recebidos**: passa o mesmo contexto para nascimento e merge. No nascimento, `_abrir_ou_agregar_disputa` grava `contexto_nascimento`; na disputa aberta, faz `merged.update(v)`, de modo que uma nova `data` substitui a anterior. Acrescenta `qtd` e `ultima_em`, mas não conserva as duas datas no campo `data`. (`app/chamados/services/disputa_emissao.py:484-501,523-527,598-603`)

Os caminhos citados abaixo **não persistem `contexto[motivo]['data']`**, mas participam da escolha da data da pergunta:

| Caminho | O que efetivamente faz / qual dia usa | Arquivo:linha |
|---|---|---|
| `_data_furo_da_grade` | Retorna o **dia local de `entrada_aberta_em`**, ou de `ultima_batida.timestamp`, depois de confirmar trabalho nesse mesmo dia na grade. Não calcula saída esperada. | `app/chamados/services/disputa_emissao.py:51-69` |
| Fallback da grade em `gerar_perguntas_disputa` | Escreve `data` somente na cópia local `_ctx`, usada no texto; coloca o dia em `_data_fallback`. Não atribui essa cópia a `disputa.contexto`. | `app/chamados/services/disputa_emissao.py:318-331` |
| `_complementar_marcos_faltantes` | Cria perguntas diretamente. Usa o **dia da célula** devolvido por `_marcos_faltantes`, exigindo `dia == data_evento` recebido. | `app/chamados/services/disputa_emissao.py:783-815`; `app/chamados/services/declaracao_texto.py:80-89,104-123` |
| `declaracao_texto`: declaração do colaborador e aplicação pelo admin | Cria/reutiliza pergunta com o **dia da célula do marco casado**. A véspera pode fornecer somente o marco de fim, conservando o dia da célula da véspera. A disputa nasce apenas com `origem`, sem merge de contexto. | `app/chamados/services/declaracao_texto.py:104-123,131-139,292-323,563-583,605-612`; `app/chamados/services/disputa_emissao.py:563-566` |

`criar_disputa_a_partir_de_chamados_orfaos` também não grava esse campo: o contexto de nascimento contém somente origem e IDs dos chamados. Seus caminhos escolhem `data_evento` diretamente: (`app/chamados/services/disputa_emissao.py:927-931`)

- **Intervalo:** usa literalmente `chamado.contexto_json['marcos_faltantes'][*]['data']`. (`app/chamados/services/disputa_emissao.py:947-958`)
- **Pela ata:** obtém o dia por `data_do_chamado` e chama o complemento, que aceita somente faltantes daquele mesmo dia. (`app/chamados/services/disputa_emissao.py:969-979,806-813`)
- **Com batida de origem:** classifica pelo dia local da entrada; depois usa `resolver_data_evento(...) or data_entrada`. Para `orfao_14h`, o resolvedor retorna o **MARCO de saída esperada**, podendo ser D+1. Aqui ele usa a implementação de `escala/servico_jornada.py`. (`app/chamados/services/disputa_emissao.py:1002-1018`; `app/escala/servico_jornada.py:199-208,482-490`)
- **Sem origem:** usa `data_evento_do_contexto`, cuja prioridade é `data_turno`, `data_batida`, `entrada_timestamp`, data BR em `pendentes`, depois `horario_previsto`. Portanto, esse ramo preserva o dia encontrado nessa hierarquia. (`app/chamados/services/disputa_emissao.py:1030-1047`; `app/chamados/data_evento.py:16-39`)
- **Complementação em disputa existente:** usa o dia do contexto do chamado ou, faltando esse, a `data_evento` da pergunta existente. (`app/chamados/services/disputa_emissao.py:874-885`)

Nos demais commands relacionados:

- `reconciliar_grade._disputa` passa somente `{'origem': 'grade'}`; `marcar_foto_ausente_retro` passa `batida_em` e outros metadados, sem chave `data`. (`app/ponto/management/commands/reconciliar_grade.py:415-419`; `app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`)
- `perguntar_chamados_sem_pergunta`, `detectar_ausencias`, `processar_alertas_turno` e `processar_alertas_avancados` delegam à fábrica de órfãos descrita acima. (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20-29`; `app/chamados/services/pergunta_no_app.py:148-159`; `app/ponto/management/commands/detectar_ausencias.py:782-791`; `app/ponto/management/commands/processar_alertas_turno.py:375-384`; `app/ponto/management/commands/processar_alertas_avancados.py:149-158`)
- Há montagem adicional em **fixture de smoke**: data fixa `2026-05-15`, colaborador `_Smoke`, origem `smoke`. (`app/chamados/management/commands/smoke_chamados.py:77-79,90-101`)
- Os chamadores dos métodos genéricos de contexto da disputa escrevem `supervisao_omissa_escalado`, `incidente_operacional` e `ultimo_alerta_diario`. (`app/chamados/views.py:1343-1344`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159-162`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`)

**Quem lê para definir a pergunta e a célula:**

1. `texto_pergunta_com_data` lê `contexto[motivo]['data']` para interpolar o texto. Se falta data necessária, o gerador tenta `_data_furo_da_grade`. (`app/chamados/catalogo/query.py:58-66`; `app/chamados/services/disputa_emissao.py:317-329`)
2. Em `disputa_emissao.py:343-347`, se `_data_fallback` ainda está vazio, lê o campo e converte seus primeiros dez caracteres em `date`. Depois tenta o dia local de `ultima_batida.timestamp`. (`app/chamados/services/disputa_emissao.py:343-365`)
3. `_derivar_chamado_destino` recebe essa data com precedência sobre o contexto, compara-a por igualdade com `dia_do_fato(ch)` e devolve a data resolvida. A abertura recebe `_data_ev`, ou `_data_fallback` quando aquele é `None`. (`app/chamados/services/disputa_emissao.py:174-179,213-237,389-396`)
4. [`PerguntaDisputa.abrir`](/home/ronald/codex_ws/f0d319ba07066648413b6d346d794e846e613cf4/app/chamados/models.py:2028) recebe `data_evento`; não deriva essa data do contexto. Busca `CelulaDia` por **colaborador e `data=data_evento`**. Assim, D busca célula D; D+1 busca célula D+1, se existente. Sem data recebida, trata como conversa, usa `localdate()` e não executa esse ramo de célula/dedup. (`app/chamados/models.py:2054-2061,2085-2086`)

**Efeito dos dias diferentes no dedup:**

- O dedup local usa igualdade literal de data: intervalo consulta `(disputa, motivo, _data_fallback)`; motivos com destino consultam `(disputa, motivo, _data_ev)`. Se o derivador não acha destino, `_data_ev` é `None`, embora a abertura ainda possa receber `_data_fallback`. (`app/chamados/services/disputa_emissao.py:221-237,369-396`)
- No caminho de órfãos, `_ja_perguntou` também recorta motivo e data exatos dentro da disputa, depois consulta se há fio vivo. (`app/chamados/services/disputa_emissao.py:1202-1209`)
- O gate final amplia o dedup para **`(colaborador, motivo, data_evento)` entre disputas**, conservando a igualdade literal do dia. Para a mesma identidade, devolve a última pergunta existente e pode reapontar destino/célula. (`app/chamados/models.py:2022-2025,2058-2084`)
- **D e D+1 não são fundidos pelo dedup**, mesmo quando correspondem ao mesmo turno. Cada data constitui identidade distinta e busca sua própria célula; o nascimento continua sujeito às guardas de cada caminho. A data nova no contexto também não altera a `data_evento` da pergunta anterior. (`app/chamados/services/disputa_emissao.py:369-396,491-501`; `app/chamados/models.py:2022-2025,2058-2086`)
- A validação do destino pode aceitar D/D+1 como mesma jornada quando há cruzamento de meia-noite ou quando ele é indeterminado; essa validação não modifica as datas nem unifica as identidades do dedup. (`app/chamados/models.py:2044-2051`; `app/chamados/juizes.py:1721-1738`)

## 30/10 09:59 · sessao 017c27885e0f · FOLGA-TRABALHADA-RESOLVE · leitura · origem=Code

O caminho atual bloqueia folga em dois pontos: a oferta exige `eh_trabalho`, e o serviço recusa `tipo_dia != 'trabalho'`. Além disso, a exclusividade de `flip_tipo` não se confirma: a regularização externa também altera `Batida.tipo` diretamente. (`app/colaboradores/services/calendario.py:196–197`; `app/ponto/services/veredito_celula.py:221–222`; `app/chamados/services/regularizacao_ext.py:153–155`)

**1. Oferta do “Resolver dia” e chegada de `trabalhou`**

- `resolver_dia_oferecido` exige: `pode_ultimo_recurso`, `eh_trabalho`, dia passado e status em `falta`, `irregular`, `aberto`, `sem_certidao`. `pode_ultimo_recurso` exige dia anterior ao início da competência atual e nenhuma pergunta viva com resposta. (`app/colaboradores/services/calendario.py:188–197`, `569–579`, `874–875`)
- `contexto_calendario` chama esse juiz e grava `dia['resolver_dia_oferecido']`. A porta curta `resolver_dia_oferecido_na_data` monta o calendário de um dia e lê o mesmo campo. (`app/colaboradores/services/calendario.py:200–208`, `902–905`)
- A view `calendario_ponto` chama `contexto_calendario`, libera `pode_veredito` conforme a permissão `regularizar_batida` e renderiza `calendario_partial.html`; este inclui a grade. A grade inclui o popover somente com `pode_veredito and dia.resolver_dia_oferecido`. (`app/colaboradores/views.py:1935–1962`; `app/templates/colaboradores/calendario_partial.html:62`; `app/templates/colaboradores/partials/_calendario_grade.html:125–126`)
- O predicado decide a presença do botão; as opções são fixas no template. Hoje `trabalhou` aparece como **“Trabalhou no horário da escala”**. O formulário envia POST com `colab`, `data` ISO, `volta` e `veredito=trabalhou`. (`app/templates/ponto/partials/_veredito_dia.html:17–25`)
- Rota efetiva: `/ponto/celula/veredito/` → `veredito_celula_view`. A view exige POST e `regularizar_batida`; aceita `datas` ou `data`, valida as datas e o veredito, depois chama `veredito_celula(colaborador=colab, data=data, veredito=veredito, autor=request.user, tipo_ausencia=… or None, request=request)`. (`app/config/urls.py:29`; `app/ponto/urls.py:58`; `app/ponto/views.py:3302–3331`)

**2. Ramo `trabalhou`, janela, julgamento e final comum**

Nesta revisão, `:220` calcula a grade; a recusa está em `:221–222`. A janela é **`[data 00:00 local, data+2 dias 00:00)`**, ordenada por `timestamp`. Essa consulta não filtra retratadas nem intocáveis. (`app/ponto/services/veredito_celula.py:212–222`)

Para dia de trabalho, o ramo segue assim:

1. Se já perfeito, registra a ação “o dia ja estava completo, nada a corrigir”. Caso contrário, exige marcos, monta a cadeia absoluta e calcula candidatos a flip por `decidir_tipo_estrito`, pulando origens intocáveis e aprovação humana. (`app/ponto/services/veredito_celula.py:223–245`)
2. Simula combinações de flips e preenchimentos por template ou vizinho. Só escolhe combinação aprovada por `dia_perfeito`; sem vencedora, recusa como dia ambíguo. (`app/ponto/services/veredito_celula.py:246–289`)
3. Executa os flips por `flip_tipo(... autor=autor, motivo='veredito_celula trabalhou', acao_log='veredito_flip')`; materializa faltantes por `registrar_batida`, origem `manual`, autoridade humana e aprovação do autor. (`app/ponto/services/veredito_celula.py:290–307`)
4. Chama `recompute_turnos(colaborador, data−1, data+1)`. (`app/ponto/services/veredito_celula.py:308`)

O **final comum existente** é:

- Erros sem nenhuma ação provocam rollback e retorno. Com ações e erros, o código ainda segue para fechamento/lavra e termina com `ok=False`. (`app/ponto/services/veredito_celula.py:321–337`)
- `fechar_o_fio_do_dia` busca perguntas pelo colaborador/data e `q_vivas()`, chamando `fechar_por_admin(autor)`. Essa porta grava via `ADMIN_DECIDIU`, `materializada_em` e `materializada_por`, sem plantar batida. (`app/ponto/services/veredito_celula.py:174–178`; `app/chamados/models.py:2117–2135`)
- Os chamados vivos são reunidos tanto por `contexto_json.data_turno` quanto pelo vínculo com a célula. São ordenados por `sobrevivente_por_ultimo` e passam individualmente por `reconciliar()`; somente quando este retorna verdadeiro recebem ocorrência `dia_decidido_pelo_admin`. Portanto, o fechamento não é incondicional. (`app/ponto/services/veredito_celula.py:148–157`, `181–187`; `app/chamados/models.py:434–445`, `474–525`)
- **Depois do fechamento do fio**, havendo célula, chama `cel.lavrar_veredito(via='admin', veredito=veredito, ator=autor)`. O método delega à porta da célula, que escreve veredito/via/instante, preserva protestos, salva com evento de outbox e grava trilha antes/depois. A mesma lavra repetida retorna sem salvar. (`app/ponto/services/veredito_celula.py:324–330`; `app/escala/models.py:1775–1783`; `app/ponto/portas/celula.py:634–642`, `682–702`)
- O despachante ainda chama `registrar_log` com veredito, ações e erros. Após sucesso, a view chama `reconciliar_apos_vinculo(... motivo='resolver_dia')`. (`app/ponto/services/veredito_celula.py:334–337`; `app/ponto/views.py:3339–3341`)

`grade_dia` delega a `grade_espelho_janela(... data, data, dias=1, agora=agora)`. Na autoridade da escala, célula existente devolve seu `trabalha`; no builder por turno, `False` impede a montagem dos marcos de trabalho, e o dia sem essa montagem sai como `tipo_dia='folga'`, previsto zero e células `None`. Batidas não consumidas entram como órfãs; o overlay de `FolgaDia` limpa células e órfãs. (`app/ponto/services/simular_dia.py:5–8`; `app/escala/models.py:1066–1073`; `app/escala/utils.py:1541–1549`, `1743–1744`, `1755–1776`, `1114–1120`)

**Não existe “folga perfeita” nesse predicado:** `dia_perfeito` retorna falso para qualquer `tipo_dia != 'trabalho'`, inclusive com E,S,E,S. Para trabalho, exige ausência de órfãs, de `missing` e de divergência entre `tipo_real` e tipo do marco. (`app/ponto/services/simular_dia.py:11–23`)

**3. Intocáveis e censo dos escritores de tipo**

No veredito, `ORIGENS_INTOCAVEIS = ('manual', 'disputa_s84_retro')`; também é pulada qualquer batida com `aprovada_por_id`. O automático inclui retratadas no tratamento de “intocável”. (`app/ponto/services/veredito_celula.py:19`, `239–245`; `app/ponto/services/flip_auto.py:287–299`)

Essa proteção pertence aos **chamadores**: `flip_tipo` não verifica origem nem `aprovada_por_id`. Suas recusas são retratação, ausência de autor/request, competência bloqueada e tipo diferente de E/S. `anotar` admite o campo `tipo` e o salva sem guarda de origem. (`app/ponto/services/flip_batida.py:15–33`; `app/ponto/registro_batida.py:203–205`, `235–254`)

| Escritor encontrado | Escrita |
|---|---|
| `registrar_batida` | Define `tipo` na criação. (`app/ponto/registro_batida.py:152–159`) |
| `flip_tipo` → `anotar` | Calcula o oposto e persiste por `setattr`/`save`. (`app/ponto/services/flip_batida.py:25–33`; `app/ponto/registro_batida.py:252–254`) |
| Regularização externa, ação `corrigir` | Escolhe E/S, atribui `alvo.tipo` e salva diretamente. `alvo` vem de `item.batida_alvo`, FK para `Batida`. (`app/chamados/services/regularizacao_ext.py:127`, `142–155`; `app/chamados/models.py:2730–2732`) |
| `semear`, banco de ensaio | Cria batidas sintéticas em lote, com guarda de banco teste/sombra. (`app/ponto/registro_batida.py:316–337`) |

Logo, **não é possível provar `flip_tipo` como único escritor de alteração de tipo neste código**: há o contraexemplo concreto da regularização externa. (`app/chamados/services/regularizacao_ext.py:153–155`)

Não foram encontrados outros `update`, `bulk_update` ou SQL de alteração de `Batida.tipo` na busca estática; exaustividade diante de execução dinâmica ou escritores externos: **[não verificado]**.

**4. Recompute, lote e idempotência**

Cada flip bem-sucedido faz um recompute explícito. Além disso, `anotar.save()` dispara `post_save`, cujo receiver faz outro recompute; os signals são importados em `PontoConfig.ready`. Para N flips, são **N chamadas explícitas + N pelo signal**, e o ramo `trabalhou` acrescenta seu recompute final: **2N+1 nesses sítios**, sem contar outros efeitos. (`app/ponto/services/flip_batida.py:30–39`; `app/ponto/registro_batida.py:254`; `app/ponto/signals.py:19–36`; `app/ponto/apps.py:7–8`; `app/ponto/services/veredito_celula.py:308`)

O trecho chamado `HX-FLIP-LOTE` expõe o conjunto de flips; o comando continua percorrendo batidas e chamando `flip_tipo` individualmente. `com_flips` apenas simula em memória com `SimpleNamespace`. Não localizei API persistente de flip em lote pelo mesmo escritor. (`app/ponto/services/flip_auto.py:315–320`; `app/ponto/management/commands/flip_automatico.py:62–68`; `app/ponto/services/simular_dia.py:26–32`; ausência global: **[não verificado]**)

`flip_tipo` é inversão, sem parâmetro de tipo desejado. Chamado em todas as batidas de E,S,E,S, produz S,E,S,E; repetido, volta a E,S,E,S, com nova escrita/trilha/recompute. `anotar` é idempotente para valores iguais, mas o flip sempre pede o oposto. Já o chamador atual só enfileira quando o tipo calculado difere do gravado. (`app/ponto/services/flip_batida.py:10`, `25–39`; `app/ponto/registro_batida.py:239–254`; `app/ponto/services/veredito_celula.py:243–245`)

**5. Corte de 26/10**

O nome `PAUSA-BATIDA-NA-FOLGA-SEM-TIPO` não foi localizado em `app/docs/CORTES.md` nem em `app/docs/LEIS.md`. **[não verificado nesses dois documentos]**

O registro encontrado está em `app/docs/PROMPTS.md:1483`:

> “26/10 11:0x | **aval: PAUSA-BATIDA-NA-FOLGA-SEM-TIPO = nem (a) nem (b)** -- sem marco o tipo e o gravado (`decidir_tipo_estrito`), E,E,S,S = dono BATIDA, lista do admin; os 92 pares em dia de trabalho viram linha da fila 1 com o censo, sem cura antes. Estado: BACKLOG `R1-FLIP-NAO-VIROU`.”

A reprodução em `app/docs/RELATO.md:577` diz:

> “**RESPONDIDA 26/10 11:0x: PAUSA-BATIDA-NA-FOLGA-SEM-TIPO = nem (a) nem (b)** -- folga sem marco = BATIDA, lista do admin; pousada.”

O decisor confirma a conservação do gravado quando não há marcos. (`app/ponto/turnos.py:2361–2368`)

**6. PRONTO: #35892, col502, 10/10**

Célula, batidas, vínculos do chamado e competência desse caso: **[não verificado]**.

As verificações sustentadas pelo código são:

- Célula `trabalha=False` caracteriza a folga na autoridade da escala; ausência de marcos deve ser conferida separadamente, pois `_marcos_do_dia` pode recorrer ao template vigente. (`app/escala/models.py:1066–1073`; `app/ponto/services/veredito_celula.py:29–44`)
- Elegibilidade: excluir retratadas e respeitar origens `manual`/`disputa_s84_retro` e aprovação humana. Cada flip efetivo exige tipo E/S e autor/request. (`app/ponto/services/flip_batida.py:15–28`; `app/ponto/services/veredito_celula.py:19`, `241–242`)
- Competência “aberta”, para o flip, significa que o fechamento da competência de cada batida, calculada pela data local e corte da empresa, não esteja `aprovado` nem `exportado`. (`app/ponto/janelas.py:75–89`)
- **PAR aceita / ÍMPAR recusa e alternância cronológica ainda não são regras desse ramo.** Hoje a folga é recusada antes de flip e lavra; ter quantidade par e competência aberta não basta para esse dia passar. (`app/ponto/services/veredito_celula.py:220–245`, `271–308`; `app/ponto/services/simular_dia.py:11–13`)

## 30/10 09:58 · sessao 0c23c8bc638a · - · - · origem=Ronald (tmux)

Censo dos caminhos fora do núcleo:

| Caminho | Escrita de `contexto[motivo]['data']` / dia usado |
|---|---|
| `api/views.py` | `contexto_anomalia['saida_sem_entrada']['data'] = agora_local.date().isoformat()`; `agora_local` vem do relógio atual. É **dia civil local da tentativa/recepção**, sem usar `data_turno` nem `data_saida_esperada`. O motivo adicionado à lista é `orfao_14h`, mas a chave escrita é `saida_sem_entrada`. (`app/api/views.py:527-528`, `:652-658`). |
| `api/views_core.py` | Mesma escrita e semântica, com a mesma diferença entre motivo `orfao_14h` e chave `saida_sem_entrada`. (`app/api/views_core.py:519-520`, `:649-655`). |
| Persistência dessas APIs | Ambas passam o contexto montado a `criar_ou_atualizar_disputa`. (`app/api/views.py:777-779`; `app/api/views_core.py:777-779`). |
| PWA `triagem_batida.ramo_registrar_es` | Motivo e chave são `saida_sem_entrada`; grava `agora_local.date()`, calculado de `timezone.now()`: **dia civil local da tentativa**, não uma data extraída da célula/turno. (`app/ponto/services/triagem_batida.py:625-626`, `:648-660`). |
| `disparar_perguntas_competencia` | Delega a `fabrica_da_celula.perguntar`; alvos normais usam `c.data` de `CelulaDia`, e fio mudo usa `ch.celula.data`: **dia da célula**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:76`, `:104-123`, `:155-167`, `:277-289`). |

`declaracao_texto.py` **não escreve esse campo**: os dois caminhos usam `disputa_para_declaracao`, cujo nascimento leva somente `{'origem': origem}` e cujo `merge_contexto=None`; passam o dia diretamente a `PerguntaDisputa.abrir(data_evento=...)`. (`app/chamados/services/declaracao_texto.py:307-323`, `:605-612`; `app/chamados/services/disputa_emissao.py:563-566`).

O dia da declaração é **o dia da célula lida**, inclusive quando a candidata é a célula da véspera: `_marcos_faltantes` percorre `(d, vespera)`, aceita somente posição `fim` da véspera e devolve `data_dia`; o casamento retorna essa tupla e o escritor usa seu `dia`. Não incrementa o dia para o marco final noturno. (`app/chamados/services/declaracao_texto.py:80-89`, `:104-123`, `:131-139`, `:292-323`). No caminho de texto aplicado, a proposta preserva o mesmo dia do casamento e o passa à abertura. (`app/chamados/services/declaracao_texto.py:563-583`, `:600-612`).

Caminhos citados que **não montam `motivo.data`**:

- API `entrada_sem_saida` monta somente `entrada_aberta_em` e `hora_tentativa`; API `foto_ausente` monta somente `tipo_batida`. (`app/api/views.py:483`, `:672-675`; `app/api/views_core.py:475`, `:669-672`).
- `marcar_foto_ausente_retro` monta `foto_ausente` com `tipo_batida`, `batida_id`, `batida_em` e `_por`, sem `data`. (`app/ponto/management/commands/marcar_foto_ausente_retro.py:96-104`).
- `reconciliar_grade._disputa` nasce com `{'origem': 'grade'}`; não passa `merge_contexto`. (`app/ponto/management/commands/reconciliar_grade.py:415-419`).
- `perguntar_chamados_sem_pergunta` delega ao serviço `pergunta_no_app.perguntar`, que chama `criar_disputa_a_partir_de_chamados_orfaos`; não monta datas próprias. (`app/chamados/management/commands/perguntar_chamados_sem_pergunta.py:20-29`; `app/chamados/services/pergunta_no_app.py:148-159`).
- Os comandos `detectar_ausencias`, `processar_alertas_turno` e `processar_alertas_avancados` também delegam à fábrica de órfãos. (`app/ponto/management/commands/detectar_ausencias.py:782-791`; `app/ponto/management/commands/processar_alertas_turno.py:375-384`; `app/ponto/management/commands/processar_alertas_avancados.py:149-158`).
- Chamadores de `DisputaSupervisao.anotar_contexto/renovar_contexto` escrevem `supervisao_omissa_escalado`, `incidente_operacional` e `ultimo_alerta_diario`, não `motivo.data`. (`app/chamados/views.py:1343-1344`; `app/ponto/management/commands/escalonar_chamados_supervisao.py:159-162`; `app/ponto/management/commands/alertar_disputas_pendentes.py:48`).

O command `smoke_chamados` tem uma montagem adicional, mas é **fixture de smoke**: fixa `DATA_OK = "2026-05-15"` e envia `{motivo: {"data": DATA_OK}}` quando o texto contém `{data}`, usando colaborador `_Smoke` e origem `smoke`. Não representa dia calculado de turno real. (`app/chamados/management/commands/smoke_chamados.py:77-79`, `:90-101`).

## 30/10 09:58 · sessao d9c50c254f0a · - · - · origem=Ronald (tmux)

Itens 3/4 consolidados:

- **Intocáveis:** `ORIGENS_INTOCAVEIS = ('manual', 'disputa_s84_retro')`; o ramo `trabalhou` também preserva qualquer batida com `aprovada_por_id`. A fila automática soma `retratada_em` ao filtro. (`app/ponto/services/veredito_celula.py:19`, `app/ponto/services/veredito_celula.py:241`, `app/ponto/services/flip_auto.py:287`)
- **Essa proteção pertence aos chamadores:** `flip_tipo` recusa retratada, falta de autor/request, competência fechada e tipo diferente de E/S, mas não verifica origem protegida nem aprovação humana. (`app/ponto/services/flip_batida.py:15`, `app/ponto/services/flip_batida.py:19`, `app/ponto/services/flip_batida.py:21`, `app/ponto/services/flip_batida.py:25`)

**Censo de escritores de `Batida.tipo`:**

| Categoria | Escrita comprovada |
|---|---|
| Criação normal | `registrar_batida`: `objects.create(... tipo=tipo ...)`, nos dois caminhos. (`app/ponto/registro_batida.py:152`, `app/ponto/registro_batida.py:157`) |
| Alteração pelo flip | `flip_tipo` chama `anotar(... {'tipo': novo})`; `anotar` admite `tipo`, aplica `setattr` e salva. (`app/ponto/services/flip_batida.py:30`, `app/ponto/registro_batida.py:203`, `app/ponto/registro_batida.py:252`) |
| **Outro escritor ativo** | `materializar_regularizacao_externa`, ramo `corrigir`: escolhe E/S, atribui `alvo.tipo` e faz `alvo.save(update_fields=['tipo','timestamp'])`. `alvo` vem de `item.batida_alvo`, FK para `ponto.Batida`. (`app/chamados/services/regularizacao_ext.py:127`, `app/chamados/services/regularizacao_ext.py:142`, `app/chamados/services/regularizacao_ext.py:153`, `app/chamados/models.py:2730`) |
| Ensaio | `semear`: `Batida.objects.bulk_create([Batida(**d)...])`, protegido por `exigir_banco_de_ensaio`. (`app/ponto/registro_batida.py:331`, `app/ponto/registro_batida.py:316`) |
| Legado desabilitado | AEJ antigo fazia `get_or_create(tipo=tipo_batida)`; a view atual retorna HTTP 410. (`app/ponto/legacy/aej_importer.py.disabled:140`, `app/ponto/views.py:54`, `app/ponto/views.py:65`) |
| Apenas memória | `com_flips` e candidatas do veredito usam `SimpleNamespace`. (`app/ponto/services/simular_dia.py:26`, `app/ponto/services/veredito_celula.py:269`) |
| Django admin | `BatidaAdmin` usa `SoLeitura`, que recusa inclusão, alteração e exclusão. (`app/ponto/admin.py:11`, `app/core/admin.py:59`) |

Portanto, **o snapshot não prova exclusividade de `flip_tipo`: contém outro escritor ativo de tipo**. (`app/chamados/services/regularizacao_ext.py:153`)

O censo AST existente não denuncia esse caso: sua saída atribui `alvo.save()` a entidade desconhecida, e o gate ignora entidade `None`; o próprio censo documenta esse balde. (`app/core/censo_escritas.py:43`, `app/ponto/tests/test_chokepoint_batida_gate.py:30`) A execução feita foi `python3 -B` com `runpy` e leitura AST; resultado: Batida atribuída somente aos sítios de criação/save/delete/semeadura da porta, enquanto `regularizacao_ext.py:155` aparece como desconhecida. [não verificado: saída de ferramenta sem artefato persistido]

Não encontrei `bulk_anotar`, outro `anotar` passando `tipo`, ou `.update`/`.bulk_update`/SQL ativo de `Batida.tipo`; exaustividade contra escrita dinâmica ou executores externos não é demonstrável por essas buscas. [não verificado]

- **Recomputes:** cada flip bem-sucedido chama um recompute explicitamente, e o `save` de `anotar` dispara outro pelo receiver `post_save`; `PontoConfig.ready` importa esses signals. No percurso normal, N flips produzem N chamadas explícitas + N pelo signal; `trabalhou` ainda chama um recompute final: **2N+1 nesses sítios**, sem contar outros efeitos disparados pelos signals. (`app/ponto/services/flip_batida.py:39`, `app/ponto/registro_batida.py:254`, `app/ponto/signals.py:26`, `app/ponto/signals.py:34`, `app/ponto/apps.py:7`, `app/ponto/services/veredito_celula.py:308`)
- **Lote:** `HX-FLIP-LOTE` expõe o conjunto de flips; a gravação percorre esse conjunto chamando `flip_tipo` por batida. Não constitui escritor com recompute agregado. (`app/ponto/services/flip_auto.py:315`, `app/ponto/management/commands/flip_automatico.py:62`)
- **Idempotência:** `flip_tipo` é inversão, não atribuição de tipo desejado: E,S,E,S → S,E,S,E; uma segunda passagem → E,S,E,S, com novas escritas/trilhas/recomputes. `anotar` evita escrita quando o valor pedido já existe, mas o flip sempre pede o oposto. (`app/ponto/services/flip_batida.py:25`, `app/ponto/services/flip_batida.py:30`, `app/ponto/registro_batida.py:239`, `app/ponto/registro_batida.py:248`)

## 30/10 09:58 · sessao a825f0b32663 · O261-NOTURNO · sonda · origem=Code

Transcrição do dado fornecido; os números não foram conferidos em produção. `[nao verificado]`

| Motivo | Campos após o motivo em `DEV` | Valor | Verificação |
|---|---|---:|---|
| intervalo_saida | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 510 | [nao verificado] |
| intervalo_saida | cel_trabalha=False\|marco_da_vespera_cai_hoje=True | 114 | [nao verificado] |
| intervalo_saida | cel_trabalha=False\|marco_da_vespera_cai_hoje=True\|viva | 1 | [nao verificado] |
| intervalo_saida | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 6 | [nao verificado] |
| intervalo_saida | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 6961 | [nao verificado] |
| intervalo_saida | cel_trabalha=True\|marco_da_vespera_cai_hoje=True | 142 | [nao verificado] |
| intervalo_saida | cel_trabalha=True\|marco_da_vespera_cai_hoje=True\|viva | 59 | [nao verificado] |
| intervalo_saida | sem_celula | 286 | [nao verificado] |
| intervalo_volta | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 506 | [nao verificado] |
| intervalo_volta | cel_trabalha=False\|marco_da_vespera_cai_hoje=True | 126 | [nao verificado] |
| intervalo_volta | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 5 | [nao verificado] |
| intervalo_volta | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 7398 | [nao verificado] |
| intervalo_volta | cel_trabalha=True\|marco_da_vespera_cai_hoje=True | 309 | [nao verificado] |
| intervalo_volta | cel_trabalha=True\|marco_da_vespera_cai_hoje=True\|viva | 67 | [nao verificado] |
| intervalo_volta | sem_celula | 279 | [nao verificado] |
| orfao_14h | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 1013 | [nao verificado] |
| orfao_14h | cel_trabalha=False\|marco_da_vespera_cai_hoje=True | 404 | [nao verificado] |
| orfao_14h | cel_trabalha=False\|marco_da_vespera_cai_hoje=True\|viva | 5 | [nao verificado] |
| orfao_14h | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 20 | [nao verificado] |
| orfao_14h | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 11917 | [nao verificado] |
| orfao_14h | cel_trabalha=True\|marco_da_vespera_cai_hoje=True | 483 | [nao verificado] |
| orfao_14h | cel_trabalha=True\|marco_da_vespera_cai_hoje=True\|viva | 89 | [nao verificado] |
| orfao_14h | sem_celula | 1167 | [nao verificado] |
| saida_sem_entrada | cel_trabalha=False\|marco_da_vespera_cai_hoje=False | 962 | [nao verificado] |
| saida_sem_entrada | cel_trabalha=None\|marco_da_vespera_cai_hoje=False | 5 | [nao verificado] |
| saida_sem_entrada | cel_trabalha=True\|marco_da_vespera_cai_hoje=False | 8816 | [nao verificado] |
| saida_sem_entrada | sem_celula | 639 | [nao verificado] |
| ERROS | — | 0 | [nao verificado] |

O mapeamento do motivo para o marco está confirmado na cópia:

| Motivo | Tipo | Marco | Fonte |
|---|---|---|---|
| intervalo_saida | S | hii | [juizes.py:826](app/chamados/juizes.py#L826) |
| intervalo_volta | E | hfi | [juizes.py:827](app/chamados/juizes.py#L827) |
| orfao_14h | S | hf | [juizes.py:825](app/chamados/juizes.py#L825) |
| saida_sem_entrada | E | hi | [juizes.py:824](app/chamados/juizes.py#L824) |

**O critério descrito é compatível com `montar_lampadas`:** tomando `D = data_evento` e a célula da véspera como ancorada em `D−1`, o marco dessa célula cai em `D` quando o turno cruza a meia-noite e a hora do marco é estritamente menor que `hi`. A função aplica exatamente esse avanço de um dia e grava o resultado em `instante`. [gerar_celulas.py:55](app/ponto/management/commands/gerar_celulas.py#L55), [gerar_celulas.py:67](app/ponto/management/commands/gerar_celulas.py#L67), [gerar_celulas.py:69](app/ponto/management/commands/gerar_celulas.py#L69)

Isso também significa que `hi` não avança por essa regra; se `hf == hi`, esse marco tampouco avança, pois a comparação é `<`. [gerar_celulas.py:67](app/ponto/management/commands/gerar_celulas.py#L67)

A implementação efetiva da sonda, a seleção da célula da véspera e a afirmação de que o detector gravou o dia da saída nesses registros permanecem **[nao verificado]**: o resultado raspado não mostra essas operações.

## 30/10 09:58 · sessao 19b8586268d3 · - · - · origem=Ronald (tmux)

Censo dos caminhos delegados, somente leitura:

- **`criar_ou_atualizar_disputa` não calcula o dia**: repassa `contexto` como contexto de nascimento e de merge. No nascimento, o núcleo grava o objeto recebido integralmente; na atualização, `merged.update(v)` troca o valor antigo de `data` pelo recebido, preservando outras chaves e atualizando contadores. Portanto, o dia depende do chamador. `app/chamados/services/disputa_emissao.py:598-603`, `:484-501`, `:523-526`.
- **`perguntar_entradas_pendentes` escreve `contexto[motivo]['data']`**: extrai cada `entradas_pendentes[*]['data']`, classifica esse dia e injeta exatamente `dia.isoformat()`. Atribui a `disputa.contexto`; salva somente se a lista `criadas` ficou não vazia. `app/chamados/services/disputa_emissao.py:255-276`.
- **`_data_furo_da_grade` não escreve o campo**: usa o dia local de `entrada_sem_saida.entrada_aberta_em`, ou de `ultima_batida.timestamp`, confirma que esse mesmo dia é trabalho na grade e retorna seu ISO. Não calcula a saída esperada nem avança para D+1. `app/chamados/services/disputa_emissao.py:51-69`.
- **Fallback de `gerar_perguntas_disputa` não persiste o campo**: escreve `data` somente na cópia local `_ctx`, usada para montar o texto; coloca o resultado em `_data_fallback`, mas não atribui `_ctx` a `disputa.contexto`. `app/chamados/services/disputa_emissao.py:318-331`.
- **`_complementar_marcos_faltantes` não escreve `contexto[motivo]['data']`**: recebe `data_evento`, lê os faltantes e só aceita tuplas com `dia == data_evento`; passa esse `dia` diretamente à abertura da pergunta. `app/chamados/services/disputa_emissao.py:783-815`, `:770-777`. Os dias das tuplas são as chaves dos dias da grade/célula, inclusive o dia da célula da véspera para seu marco de fim; não são calculados do horário do marco. `app/chamados/services/declaracao_texto.py:80-89`, `:104-123`.
- **`criar_disputa_a_partir_de_chamados_orfaos` também não grava esse campo**: seu contexto de nascimento contém somente origem e IDs. `app/chamados/services/disputa_emissao.py:927-931`. Cria perguntas diretamente com:
  - Intervalo: `marcos_faltantes[*]['data']` do contexto do chamado. `:947-958`.
  - Caminho da ata: dia retornado por `data_do_chamado`, passado ao complemento. `:969-979`.
  - Grupo com batida de origem: dia local da entrada para classificar; depois `resolver_data_evento(...) or data_entrada` para a pergunta. `:1002-1018`. Para `orfao_14h`, esse resolvedor retorna **dia do marco de saída esperada**, mediante `servico_jornada.data_saida_esperada`, que retorna `fim_turno(...).date()`. `app/escala/servico_jornada.py:482-490`, `:199-208`. O fim é o último marco encadeado do turno. `app/escala/servico_jornada.py:89-92`.
  - Sem origem: dia de `data_evento_do_contexto(ch.contexto_json)` diretamente. `app/chamados/services/disputa_emissao.py:1030-1049`. A hierarquia é `data_turno`, `data_batida`, `entrada_timestamp`, data BR em `pendentes`, `horario_previsto`. `app/chamados/data_evento.py:16-39`.
  - Repasse a disputa já existente: usa o dia do contexto do chamado, ou `PerguntaDisputa.data_evento` anterior. `app/chamados/services/disputa_emissao.py:874-885`.

Leitura e célula:

- `texto_pergunta_com_data` lê `contexto[motivo]['data']` para interpolar o texto. `app/chamados/catalogo/query.py:58-66`.
- `gerar_perguntas_disputa` prioriza `_data_fallback` obtida da grade; na ausência, lê literalmente `contexto[motivo]['data']` e converte os primeiros dez caracteres em `date`. Depois tenta o dia local de `ultima_batida.timestamp`. `app/chamados/services/disputa_emissao.py:322-329`, `:343-365`.
- `_derivar_chamado_destino` recebe essa data com precedência sobre o contexto; compara com `dia_do_fato(ch)` por igualdade e devolve a data da pergunta/contexto, ou a do chamado. `app/chamados/services/disputa_emissao.py:174-179`, `:213-237`. A abertura recebe `_data_ev`, ou `_data_fallback` quando aquele é `None`. `:389-396`.
- `PerguntaDisputa.abrir` não lê o contexto: recebe `data_evento` e procura `CelulaDia` por **colaborador e igualdade `data=data_evento`**. Portanto, D seleciona a célula D e D+1 seleciona a célula D+1, se existente. `app/chamados/models.py:2028`, `:2058-2061`, `:2085-2086`.

Dedup quando os escritores fornecem dias distintos:

- O dedup local usa igualdade literal `(disputa, motivo, data_evento)`: intervalo compara com `_data_fallback`; motivos com destino comparam com `_data_ev`. `app/chamados/services/disputa_emissao.py:369-388`. No caminho de órfãos, `_ja_perguntou` também filtra motivo/data exatos dentro da disputa, depois consulta a condição de fio vivo. `:1202-1209`.
- O gate final amplia a identidade para **`(colaborador, motivo, data_evento)` entre quaisquer disputas**, também por igualdade literal da data. Reaponta célula/destino e retorna a última pergunta existente desse mesmo dia. `app/chamados/models.py:2022-2025`, `:2058-2084`.
- Assim, **D e D+1 são identidades distintas para o dedup**: não há conversão para um dia comum do turno nesses filtros; se os caminhos alcançarem a criação e passarem pelas guardas, podem produzir perguntas distintas, cada qual ligada à célula de sua própria `data_evento`. `app/chamados/services/disputa_emissao.py:369-396`; `app/chamados/models.py:2022-2025`, `:2058-2086`.
- A validação de chamado-destino pode aceitar D versus D+1 como mesmo turno se o turno cruza a meia-noite, ou se essa condição é indeterminada; isso valida o elo, sem modificar a data nem a identidade do dedup. `app/chamados/models.py:2044-2051`; `app/chamados/juizes.py:1721-1738`.

## 30/10 09:58 · sessao 21bfa79ae7b2 · - · - · origem=Ronald (tmux)

| Escritor/caminho | Dia gravado em `contexto[motivo]['data']` | Evidência |
|---|---|---|
| `detectar_anomalias`: `orfao_14h` | **MARCO de saída esperada** da entrada pendente escolhida; pode ser D+1; grava `None` se não consegue obter saída. Seleciona primeira pendência com ≥14 h, senão primeira pendência. | `app/chamados/detector_anomalias.py:35-49` |
| `fabrica_da_celula.perguntar` | Grava literalmente o argumento `dia.isoformat()` para **cada motivo**. | `app/chamados/services/fabrica_da_celula.py:118-124` |
| Julgamento → `perguntar_pela_celula` → `perguntar` | **DIA DA CÉLULA**: fornece `cel.data`. | `app/chamados/services/fabrica_da_celula.py:143-149` |
| Command `disparar_perguntas_competencia`: universo da célula | **DIA DA CÉLULA**: monta `(c.colaborador, c.data, motivo)` e chama o mesmo `perguntar`. | `app/chamados/management/commands/disparar_perguntas_competencia.py:111-123`, `:276-289` |
| Mesmo command: universo “fio mudo” | **DIA DA CÉLULA DO CHAMADO**: `dia = ch.celula.data`, monta `(colab, dia, motivo)` e chama o mesmo `perguntar`. | `app/chamados/management/commands/disparar_perguntas_competencia.py:165-168`, `:225-226`, `:244`, `:276-289` |
| `criar_ou_atualizar_disputa` | **Não escolhe nem converte o dia**: entrega o contexto recebido ao nascimento e ao merge. | `app/chamados/services/disputa_emissao.py:598-603` |
| `_abrir_ou_agregar_disputa`: nascimento | Persiste integralmente `contexto_nascimento`. | `app/chamados/services/disputa_emissao.py:523-527` |
| `_abrir_ou_agregar_disputa`: disputa aberta | Para o mesmo motivo, copia contexto anterior e faz `merged.update(v)`: a `data` recebida **substitui** a anterior quando presente; agrega `qtd` e `ultima_em` e salva. | `app/chamados/services/disputa_emissao.py:479-501` |

O dia do detector é calculado assim: `data_saida_esperada` usa o dia local da **entrada**, consulta `ec.marcos_do_dia(d)`, calcula o fim encadeado e retorna **`fim.date()`**; se vínculo/template/marcos necessários faltarem, retorna `None`. O encadeamento acrescenta um dia enquanto o próximo marco for `<=` ao anterior — daí a saída noturna poder pertencer a D+1. (`app/escala/utils.py:920-931`, `:907-909`, `:891-903`.)

No detector, os demais contextos montados não incluem chave literal `data`: colisão escreve IDs/contagem; ordem inválida escreve ordens; posto desativado escreve ID/nome; sem posto escreve `{}`. O ramo férias passa `info` se for dict ou sua representação textual; a função atualmente chamada retorna `ChamadoColaborador`, não dict. (`app/chamados/detector_anomalias.py:63-66`, `:94-109`, `:113-117`; `app/ponto/utils.py:100-101`, `:103`, `:137`.)

`detectar_batida_fora_escala` produz `data_batida`, não a chave `data`; o contexto de desvio nem inclui campo de data. (`app/chamados/detector_anomalias.py:170-175`, `:215-222`, `:229-236`.)

`models.py` apenas reexporta `criar_ou_atualizar_disputa` e os outros emissores desse módulo; não há uma segunda implementação desse helper ali. (`app/chamados/models.py:2776-2778`.)

## 30/10 09:57 · sessao f5efc31e6b22 · - · - · origem=Ronald (tmux)

- **Janela atual do ramo `trabalhou`:** `j0` é `data` à meia-noite no fuso corrente; `j1=j0+2 dias`; lê `Batida` de `timestamp>=j0` e `<j1`, por `timestamp`. A consulta não exclui retratadas nem origens protegidas; o descarte das intocáveis só aparece na seleção posterior de candidatos a flip. (`app/ponto/services/veredito_celula.py:212-218`, `app/ponto/services/veredito_celula.py:239-245`)

- **Juiz compartilhado:** `grade_dia` chama `grade_espelho_janela(colaborador, batidas, data, data, dias=1, agora=agora)` e devolve os marcos e o primeiro dia. Essa função monta a grade e aplica o clip de piso/hoje; o serviço captura um único `timezone.now()` para o ato. (`app/ponto/services/simular_dia.py:5-8`, `app/escala/utils.py:2263-2279`, `app/ponto/services/veredito_celula.py:200`)

- **Folga e perfeição:** a recusa efetiva nesta revisão fica em **221–222**; 220 apenas calcula `marcos0,dd0`. Se `tipo_dia!='trabalho'`, devolve “a escala nao preve trabalho nesse dia (...) -- nada a materializar”. Independentemente disso, `dia_perfeito` retorna `False` para qualquer tipo diferente de `trabalho`, ou com órfãs; depois exige ausência de `missing` e compatibilidade de `tipo_real` com o marco. Portanto **nenhuma folga é perfeita segundo esse predicado atual**, mesmo E,S,E,S. (`app/ponto/services/veredito_celula.py:220-228`, `app/ponto/services/simular_dia.py:11-23`)

- **Como a grade preserva folga:** `CelulaDia.trabalha=False` significa folga; `EscalaColaborador.eh_dia_trabalho` devolve diretamente `cel.trabalha` quando há célula. No builder por turno, pula o dia quando essa autoridade devolve `False`; dia sem `cel_por_data` desce como `tipo_dia='folga'`, previsto 0 e células `None`. Batidas não consumidas entram como órfãs do dia; o overlay de `FolgaDia` mantém `tipo_dia='folga'` e limpa células/órfãs. (`app/escala/models.py:1654-1656`, `app/escala/models.py:1066-1073`, `app/escala/utils.py:1541-1549`, `app/escala/utils.py:1722-1745`, `app/escala/utils.py:1755-1777`, `app/escala/utils.py:1101-1120`)
  
  A escolha do builder depende de `GRADE_POR_TURNO`; o settings assume `True` por padrão, mas lê variável de ambiente. O valor efetivo de execução aqui é [não verificado]. (`app/escala/utils.py:2024-2030`, `app/config/settings/base.py:165`)

- **O final comum já existente:** após a lógica técnica, há `recompute_turnos(colaborador,data-1,data+1)`; erros sem ações causam rollback/retorno; passa por `fechar_o_fio_do_dia`; se existe célula, chama `cel.lavrar_veredito(via='admin',veredito=veredito,ator=autor)`; grava o `registrar_log` do despachante; retorna `ok=not erros`. (`app/ponto/services/veredito_celula.py:308`, `app/ponto/services/veredito_celula.py:321-337`)

  A lavra da célula preserva protestos no caminho admin; mesma entrada não salva nem emite evento; mudança escreve `veredito`, `veredito_via`, `veredito_em`, `protestos`, chama `salvar_e_emitir` e registra `evento('veredito_celula',...)` com ator e antes/depois. Ela não escreve `trabalha`. (`app/ponto/portas/celula.py:682-703`)

- **Competência aberta:** `flip_tipo` consulta `competencia_fechada` antes da inversão e recusa sua mensagem; o guard calcula a competência pela data local da **batida**, com o corte da empresa, e barra fechamento `aprovado` ou `exportado`. Ausência de fechamento ou outros status devolvem `''`; “aberta” tecnicamente significa não estar nos dois status bloqueadores. (`app/ponto/services/flip_batida.py:21-28`, `app/ponto/janelas.py:75-89`)

- **Caso #35892 / col502 / 10/10:** dados reais, quantidade/origens/tipos de batidas, célula e competência estão [não verificado]. No código atual, uma célula de folga ordinária que chega à grade como `folga` será recusada antes de flip/lavra; paridade par, por si, ainda não é um caminho implementado nesse ramo. (`app/ponto/services/veredito_celula.py:220-228`, `app/ponto/services/simular_dia.py:11-23`)

  Para a leitura de pré-requisitos do desenho pedido: comprovar a célula `trabalha=False`; comprovar as batidas elegíveis e sua quantidade/paridade [não verificado para esse dia]; para cada batida efetivamente flipada, o escritor exige não retratada, autor/request, tipo E/S e competência não bloqueada. A proteção de origem/humano pertence ao chamador do veredito, não a `flip_tipo`. (`app/escala/models.py:1654-1656`, `app/ponto/services/flip_batida.py:10-28`, `app/ponto/services/veredito_celula.py:239-245`)

_444 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
