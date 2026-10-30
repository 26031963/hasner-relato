# CODEX-ESTADO

> Ultima resposta processada: **29/10/2026 23:58** · publicado em **30/10/2026 00:03** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 29/10 23:58 · sessao 25e3a63159a1 · mariaB · verificador · origem=Code

Encontrei **3 achados**. A análise é estática; execução da suíte, contagem dinâmica de queries e reprodução concorrente: **[nao verificado]**. [nao verificado]

1. **O custo novo na batida não é zero.**  
   **Local:** `app/chamados/services/fabrica_da_celula.py:103` e `app/chamados/services/relavra_via.py:202`.

   **Cenário:** célula continua acusando um marco; existe outra pergunta do dia carimbada, em disputa aberta, inclusive com `admin_decidiu`. A coleta a encaminha; a porta abre `atomic` e faz `SELECT FOR UPDATE` **antes** de excluir identidade. Portanto, com *k* candidatas há *k* releituras adicionais, além do custo transacional; cada cura acrescenta escrita, auditoria e callback. Reutilizar a query do `ja` elimina apenas o custo da coleta. (`fabrica_da_celula.py:98–103`; `relavra_via.py:202–222`; `app/chamados/signals.py:191–202`.)

   O selo existente cobra queries e quantidade de callbacks, mas sua fixture com três perguntas não cria carimbos e não exercita esse ramo. **Classe:** custo no chokepoint; selo sem mordida para o estado acrescentado. (`app/ponto/tests/test_selo_chokepoint_escrita.py:169–218`, `:226–240`.)

2. **A restrição “só disputa aberta” pode ser perdida entre coleta e escrita.**  
   **Local:** `app/chamados/services/relavra_via.py:203` e `:230`.

   **Cenário concorrente concreto:**

   - B calcula que pode fechar a disputa enquanto a célula ainda absolve. (`app/chamados/models.py:1481–1489`.)
   - A rejulga a célula como acusadora e coleta a pergunta enquanto a disputa ainda está aberta. (`app/ponto/services/cartorio.py:754–770`; `fabrica_da_celula.py:98–103`.)
   - B grava o fechamento normal; esse fechamento não carimba identidade. (`models.py:1490–1502`.)
   - A relê e trava somente a pergunta, encontra C6 vivo e **reabre a disputa agora fechada**. (`relavra_via.py:203–231`; `app/chamados/juizes.py:1054–1057`.)

   Isso contraria o recorte declarado pelo próprio diff e pode recolocar a disputa no push diário, que seleciona abertas. **Classe:** ordem de trava/transação; condição de entrada não preservada na escrita. (`fabrica_da_celula.py:134–135`; `app/ponto/management/commands/alertar_disputas_pendentes.py:33–64`.) Reprodução concorrente: **[nao verificado]**.

3. **A cura herda filtros de emissão que excluem carimbos que C6 considera sem lastro.**  
   **Local:** `app/chamados/services/fabrica_da_celula.py:102`.

   **Cenário:** pergunta de saída sem resposta, carimbada por `legado_sem_lastro`, sem plantio nem destino que a sustente; célula `concorde` provisória, com saída pendente. C6 não aceita o veredito provisório como absolvição e pode manter essa pergunta viva pelo último degrau. Porém `motivos_a_perguntar` retorna pelo veredito não acusador **antes de preencher `carimbadas`**: mesmo passando pelo julgamento, o carimbo permanece. (`app/chamados/models.py:2562–2563`; `app/chamados/juizes.py:675–715`, `:1058–1083`; `fabrica_da_celula.py:86–103`.)

   A distinção entre `concorde` provisório e fato encerrado já está selada; a coleta nova não a acompanha. **Classe:** incompletude contra o juiz existente e teto temporal. (`app/chamados/tests/test_o261_pergunta_viva_teto.py:43–60`.) Reprodução desse cenário com carimbo: **[nao verificado]**.

Os caminhos pedidos ficam assim:

| Caminho | Encadeamento |
|---|---|
| Batida | `post_save`/`post_delete` → `_julgar_dias`, em D−1 e D → `julgar_celula` → `julgar_colab` → `perguntar_pela_celula`. (`app/ponto/signals.py:85–112`, `:131–140`; `app/ponto/services/cartorio.py:1560`, `:770`.) |
| Cron diário | `processar_cartorio --apply` → `julgar_colab` → mesma fábrica. (`app/config/crons.py:597`; `app/ponto/management/commands/processar_cartorio.py:59–65`.) |
| Vigia de hora | Chama `julgar_celula(..., teto_de_hoje=True)` e chega à mesma fábrica. (`app/ponto/services/vigia_de_hora.py:130`; `app/ponto/services/cartorio.py:770`.) |
| Lote | Chama `perguntar`, sem passar por `perguntar_pela_celula`; o dedup anterior normalmente exclui a pergunta já existente. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124–129`, `:276–289`.) |

Depois da queda normal, são limpos os três campos e gravada auditoria; não há chamada direta de push ou criação de chamado nessa porta. O `post_save` agenda a verificação de beco, cujo fechamento respeita C6. A reabertura ocorre no ramo de disputa fechada descrito no achado 2. (`app/chamados/services/relavra_via.py:213–231`; `app/chamados/signals.py:191–202`; `app/chamados/services/becos.py:25–33`, `:67–70`.)

As não alcançadas **têm cobrança no painel**, embora não tenham cura automática garantida: `materializada_viva` varre abertas sem exigir célula ou rejulgamento; o comando carimba esperado zero e `fio_estado` publica divergências. Destrancar a competência também pode devolver vida pelo juiz sem rejulgar a célula. (`app/chamados/services/vigia_fio.py:400–405`; `app/chamados/management/commands/vigiar_fio_morto.py:197–203`; `bin/fio_estado.py:69–73`, `:105–108`; `app/chamados/juizes.py:875–907`.)

Quanto ao RED de **cada teste novo**, a previsão estática é: [nao verificado]

| Teste | Sem a mudança correspondente | [nao verificado]
|---|---|
| Cura principal | Vermelho no carimbo preservado. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`.) |
| Morta mantém carimbo | Já verde; controle negativo. (`:102–111`.) | [nao verificado]
| Identidade preservada | Já verde; controle negativo. (`:113–120`.) | [nao verificado]
| Idempotência | Vermelho na exigência de primeira cura; evita idempotência vazia. (`:128`.) |
| Fechada não reabre | Já verde no anterior; não cobre fechamento entre coleta e escrita. (`:135–147`.) | [nao verificado]
| Decisão humana após leitura | Vermelho esperado: a implementação anterior usava a instância velha. (`:149–160`.) |
| Vigia conta e zera | Sem contador: erro de importação; mantendo só o contador novo, falha após julgamento. (`:167–172`.) |
| Vigia ignora identidade/morta | Sem contador: erro de importação; com contador, é controle independente da cura. (`:174–178`.) |
| Teste alterado do carimbo | Cobra as novas chaves, mas suas fixtures não criam materializada viva positiva. (`app/chamados/tests/test_o261_vigia_fio.py:690–710`.) |

Esses testes chamam o helper diretamente e não executam os callbacks `on_commit`; não demonstram o custo da batida carimbada nem os efeitos após commit. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:39`, `:81–83`; `app/ponto/tests/test_selo_chokepoint_escrita.py:169–170`.) RED/verde efetivamente executado: **[nao verificado]**.

## 29/10 23:57 · sessao 811b7882f6ff · - · - · origem=Ronald (tmux)

Achado sustentado por interleaving: **a exclusão de disputa fechada não é revalidada na porta**. A candidata é coletada aberta em `app/chamados/services/fabrica_da_celula.py:98-103`; a releitura trava somente a pergunta em `app/chamados/services/relavra_via.py:203-204`, aceita a disputa agora fechada e a reabre em `:229-231`.

Cenário concreto: B calcula `vivas=[]` ao fechar normalmente quando a célula ainda absolvia (`app/chamados/models.py:1481-1489`); A grava julgamento acusador e coleta a candidata ainda aberta; B grava fechamento (`:1490-1494`); A relê pergunta carimbada e disputa fechada, obtém viva pela célula acusadora (`app/chamados/juizes.py:1054-1057`) e reabre. O fechamento normal não ganha identidade, reservada às exceções (`app/chamados/models.py:1499-1502`). Classe: **ordem de trava/transação; guarda aberta fora da unidade da escrita**. Interleaving deduzido, reprodução concorrente não executada [não verificado].

A reabertura indevida entra no push diário: seleção de todas abertas em `app/ponto/management/commands/alertar_disputas_pendentes.py:33-35`, push ao colaborador em `:48-64`, supervisão em `:103-108`; cron às 08h em `app/config/crons.py:719`.

No caminho normal, limpar o carimbo grava três campos e auditoria (`app/chamados/services/relavra_via.py:213-222`), sem push direto; o `post_save` agenda fechamento de beco (`app/chamados/signals.py:191-202`), que respeita C6 e não fecha pergunta viva (`app/chamados/services/becos.py:25-33`). A cobrança manual já via essa pergunta viva antes da limpeza (`app/chamados/services/aviso_pendencia.py:7-13`).

Há cobrança das não alcançadas: `materializada_viva` inclui abertas com ou sem célula e sem exigir rejulgamento (`app/chamados/services/vigia_fio.py:400-405`); o comando grava contador e esperado (`app/chamados/management/commands/vigiar_fio_morto.py:197-203`), e o ESTADO publica divergência (`bin/fio_estado.py:69-73,105-108`). O lote não oferece recuperação: deduplica perguntas existentes (`app/chamados/management/commands/disparar_perguntas_competencia.py:124-129`) e pula fio que C6 já dá por vivo (`:163-164`). Destrancar também pode mudar C6 sem julgamento da célula (`app/chamados/juizes.py:875-907`).

## 29/10 23:55 · sessao fdfed644648e · - · - · origem=Ronald (tmux)

Caminhos e custo confirmados por leitura estática:

- **Batida:** `post_save`/`post_delete` → `_julgar_dias` → `julgar_celula`, nos dias d−1 e d → `julgar_colab` → `perguntar_pela_celula`. Referências: `app/ponto/signals.py:85`, `app/ponto/signals.py:108`, `app/ponto/signals.py:131`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:770`.
- **Cron diário:** `processar_cartorio --apply` → laço de colaboradores → `julgar_colab` → mesma fábrica. Referências: `app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/management/commands/processar_cartorio.py:65`.
- **Vigia de hora:** roda a cada dez minutos e chama `julgar_celula(..., teto_de_hoje=True)` → mesma fábrica. Referências: `app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`.
- **Lote:** não chama `perguntar_pela_celula`; chama `perguntar`. Seu dedup exclui perguntas já existentes antes disso, portanto não recupera normalmente o carimbo existente. Referências: `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`.

**A afirmação “0 query a mais na batida” só vale sem candidatas.** A coleta reaproveita uma query, mas cada candidata agora passa por `atomic` e um `SELECT FOR UPDATE`, inclusive identidade que será poupada. Cada cura acrescenta `UPDATE`, trilha e um registro de `on_commit`; o callback consulta/fecha becos após o commit. Referências: `app/chamados/services/fabrica_da_celula.py:98`, `app/chamados/services/fabrica_da_celula.py:139`, `app/chamados/services/relavra_via.py:202`, `app/chamados/services/relavra_via.py:216`, `app/chamados/signals.py:191`.

**Lacuna do selo:** o cenário explícito com três perguntas não cria carimbos, portanto não exercita esse ramo novo. O selo cobra queries e quantidade exata de callbacks, mas não inclui cenário explícito de batida com carimbo sem lastro. Referências: `app/ponto/tests/test_selo_chokepoint_escrita.py:226`, `app/ponto/tests/test_selo_chokepoint_escrita.py:237`, `app/ponto/tests/test_selo_chokepoint_escrita.py:207`. Quantificação dinâmica: **[não verificado]**.

**Incompletude possível para avaliar na consolidação:** a coleta fica depois dos retornos por `trabalha`, situação e veredito não acusador; entretanto C6 pode continuar dando viva uma pergunta com resposta divergente, ou sem resposta quando o veredito é provisório e não existe testemunha. Nesse caso a fábrica não oferece a pergunta à porta, embora o vigia possa contá-la. Referências: `app/chamados/services/fabrica_da_celula.py:78`, `app/chamados/services/fabrica_da_celula.py:86`, `app/chamados/juizes.py:1020`, `app/chamados/juizes.py:1063`, `app/chamados/juizes.py:1083`. A pertinência do caso respondido ao escopo literal B exige distinguir a cura geral da porta do pedido “sem resposta”.

## 29/10 23:55 · sessao ffedafeaa28e · - · - · origem=Ronald (tmux)

Análise estática; RED/verde executado: **[não verificado]**. [nao verificado]

| Teste novo | Sem mudança de produção | [nao verificado]
|---|---|
| Cura principal | Vermelho esperado no carimbo preservado: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:98`. |
| Morta mantém carimbo | Já verde: controle negativo; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:102`. |
| Identidade preservada | Já verde: controle negativo; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:113`. |
| Idempotência | Vermelho esperado na pré-condição da primeira cura; não passa por ausência de escrita: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:128`. |
| Fechada permanece fechada | Já verde no anterior, que não encaminhava candidata alguma à porta: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:135`. |
| Instância velha versus decisão humana nova | Vermelho esperado: código anterior sobrescreveria `admin_decidiu`; `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:155`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:159`. |
| Vigia conta e zera | Sem vigia: ImportError; mantendo apenas o contador, vermelho esperado após julgamento: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:168`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:172`. |
| Vigia ignora identidade/morta | Sem vigia: ImportError; mantendo contador, controle independente da cura: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:174`. |

O teste alterado do carimbo cobra a chave nova tanto em `censo` como em `esperado`, mas suas fixtures não criam materializada viva positiva; essa cobertura fica no novo arquivo: `app/chamados/tests/test_o261_vigia_fio.py:690`, `app/chamados/tests/test_o261_vigia_fio.py:701`, `app/chamados/tests/test_o261_vigia_fio.py:707`.

A allowlist nova não constitui RED novo; o contrato existente filtra ofensores pelo conjunto autorizado: `app/chamados/tests/test_contract_leitores_vitalidade.py:35`, `app/chamados/tests/test_contract_leitores_vitalidade.py:299`.

**Limite material dos selos novos:** todos herdam `TestCase`, usam o helper diretamente e não executam os callbacks `on_commit`; portanto não demonstram comportamento após commit, disputa fechando concorrentemente ou custo do caminho real da batida com candidata carimbada. Referências: `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:30`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:39`, `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:81`, `app/chamados/signals.py:191`, `app/chamados/signals.py:202`. O selo preexistente conta consultas e registros de callbacks, exigindo cenário que efetivamente passe por essa cura: `app/ponto/tests/test_selo_chokepoint_escrita.py:169`, `app/ponto/tests/test_selo_chokepoint_escrita.py:207`.

## 29/10 23:44 · sessao 1b05e128f7e5 · o261c · leitura · origem=Code

**Sim: há recusas que decorrem de filtros divergentes da regra de costura.** O caso mais direto é o histórico: ele conserva o mapa de **motivo principal**, enquanto o juiz de motivos atendidos permite que o chamado do dia responda pelos quatro motivos. Já o `False` de c1 diante de pergunta VIVA é deliberado e correto. (`app/chamados/reconciliador.py:1391–1394`; `app/chamados/juizes.py:1589–1600`; `app/chamados/reconciliador.py:398–400`)

Não é possível distribuir os ~140 casos entre essas causas apenas pela árvore; a incidência de cada ramo nessa amostra permanece **[não verificado]**.

**1. Fluxo externo e retornos finais**

`reatar_pergunta_orfa` tem quatro pontos explícitos de `return False`:

| Caminho | Condição | Retorno final |
|---|---|---|
| Derivador não encontrou destino; falta data | Nem `p.data_evento` nem `contexto[p.motivo]['data']` fornecem data | `app/chamados/reconciliador.py:1382–1383` |
| Derivador não encontrou destino; cardinalidade do histórico | `hist >= 2`; ou `hist == 0` e **ambos** c1/c2 recusaram | `app/chamados/reconciliador.py:1395–1416` |
| Histórico único; costura recusada | `reapontar_resolvedora(...hist[0]...)` devolveu `_ok=False` | `app/chamados/reconciliador.py:1433–1436` |
| Candidato vivo; costura recusada | `reapontar_resolvedora(...ch...)` devolveu `_ok=False` | `app/chamados/reconciliador.py:1445–1448` |

Portanto, `None` do derivador **não encerra a tentativa**. Mas histórico ambíguo encerra sem tentar c1/c2; candidato vivo recusado pelo juiz também encerra sem tentar os demais caminhos. (`app/chamados/reconciliador.py:1371–1416`, `:1445–1449`)

Exceções não são convertidas em `False` por essa função: o laço externo as captura e registra separadamente. (`app/chamados/reconciliador.py:1338–1343`)

**2. `_derivar_chamado_destino`: quando retorna `(None, None)`**

A implementação está em `services/disputa_emissao.py`, reexportada por `models.py`. O reatamento passa a data da própria pergunta; o contexto da disputa é fallback. (`app/chamados/models.py:2778`; `app/chamados/reconciliador.py:1369–1370`; `app/chamados/services/disputa_emissao.py:174–179`)

| Filtro | Como elimina o candidato |
|---|---|
| Colaborador e estado | Busca somente chamados do colaborador da disputa com `status_local in VIVOS`; exclui categoria exatamente `disputa_supervisao`. `VIVOS = aberto, em_analise, registrado`. (`app/chamados/services/disputa_emissao.py:181–184`; `app/chamados/catalogo/motor.py:24`) |
| Categoria/subcategoria | Normaliza ambos e exige `motivo in motivos_atendidos(cat, sub)`. Se nenhum rótulo pertence aos mapas, descarta. **Não há filtro direto por `modulo_origem`**, nem consulta à lâmpada nessa classificação. (`app/chamados/services/disputa_emissao.py:186–196`; `app/chamados/juizes.py:1595–1600`) |
| Candidato ocupado | Descarta se existe pergunta apontando para aquele chamado, do **mesmo motivo**, com `materializada_em IS NULL`. Não exige disputa aberta, mesma data ou `pergunta_viva=True`. (`app/chamados/services/disputa_emissao.py:209–212`) |
| Dia diferente | Obtém `dia_do_fato(ch)`; quando as duas datas existem, qualquer diferença textual elimina o candidato. Se alguma data falta, esse filtro não elimina. (`app/chamados/services/disputa_emissao.py:214–218`) |
| Nenhum candidato ou ambiguidade residual | Depois do desempate, exige exatamente um; caso contrário retorna `(None, None)` em **227**. (`app/chamados/services/disputa_emissao.py:219–227`) |

`motivos_atendidos` devolve **os quatro motivos** — `orfao_14h`, `saida_sem_entrada`, `intervalo_saida`, `intervalo_volta` — quando categoria **ou** subcategoria está no mapa. Portanto, no derivador vivo, um chamado de turno pode atender intervalo; não se exige igualdade com seu motivo principal. (`app/chamados/juizes.py:1597–1600`; `app/chamados/models.py:2502–2503`)

O desempate só roda com mais de um candidato. Sem data válida ou sem entradas de turnos abertos, mantém a lista original. Com entradas, seleciona os candidatos cujo `entrada_timestamp` casa com elas; um único vence; vários da mesma entrada e das categorias `turno_aberto`/`turno_aberto_24h` escolhem o estágio mais avançado; outras ambiguidades permanecem. (`app/chamados/services/disputa_emissao.py:110–146`, `:219–220`)

A ocupação por carimbo é uma escolha expressa dessa função, inclusive preservada em testes; sua diferença para `pergunta_viva` **não basta, isoladamente, para declarar defeito**. (`app/chamados/services/disputa_emissao.py:206–212`; `app/chamados/tests/test_selo_mapa_motivos_do_dia.py:122–143`)

**3. Histórico: seleção, bloqueios e diferença para o derivador**

O histórico exige simultaneamente:

- Mesmo colaborador; `status_local in ENCERRADOS`, que contém somente `resolvido` e `fechado`, excluindo `cancelado`/`superado`. (`app/chamados/reconciliador.py:1385–1386`; `app/chamados/catalogo/motor.py:30–31`)
- `contexto_json__data_turno == str(data_ctx)`; exclui categoria exatamente `disputa_supervisao`. (`app/chamados/reconciliador.py:1387–1388`)
- Motivo calculado por `MAPA_SUBCATEGORIA.get(sub) or MAPA_CATEGORIA.get(cat)` **igual** ao motivo da pergunta; a subcategoria tem precedência. (`app/chamados/reconciliador.py:1389–1394`)

Os desfechos são:

| Quantidade | Comportamento |
|---|---|
| Zero | Faz parse da data e tenta c1, depois c2. Se ambos recusarem, `False` em **1416**. (`app/chamados/reconciliador.py:1395–1416`) |
| Dois ou mais | `False` em **1416**, sem c1/c2 e sem desempate pelo turno. (`app/chamados/reconciliador.py:1395`, `:1415–1416`) |
| Um | Tenta costura; recusa dá `False` em **1436**. Costura aceita chama `resolver_perguntas_por_chamado_resolvido` e retorna `True`. (`app/chamados/reconciliador.py:1433–1440`) |

**Intervalos nunca entram nesse histórico:** nenhum valor dos dois mapas é `intervalo_saida` ou `intervalo_volta`; seus valores são apenas `orfao_14h` e `saida_sem_entrada`. (`app/chamados/models.py:2466–2483`; `app/chamados/reconciliador.py:1391–1394`)

**4. c1 — `lavrar_sem_turno_aberto`**

| Guarda | `False` |
|---|---|
| Motivo fora de `MOTIVOS_C1 = ('orfao_14h', 'saida_sem_entrada')` | `app/chamados/reconciliador.py:363–364`; conjunto em `:332` |
| Resposta do colaborador não vazia após `strip()` | `app/chamados/reconciliador.py:365–366` |
| Já materializada | `app/chamados/reconciliador.py:367–368`; excluído pelas premissas apresentadas |
| Existe turno aberto na janela do dia | `app/chamados/reconciliador.py:372–373` |
| `pergunta_viva(p)` é verdadeira | `app/chamados/reconciliador.py:398–400` |

Com dia, `_tem_turno_aberto` usa turnos com entrada e sem saída em `(dia−1, dia)`; há veto quando todos esses turnos têm seu próprio dia posicionalmente completo. Sem dia, usa consulta global de turnos abertos. (`app/chamados/reconciliador.py:96–114`, `:171–178`)

**Para a pergunta VIVA da questão, c1 necessariamente recusa:** ou alguma guarda anterior dispara, ou chega ao `False` de **400**. Isso protege o furo real contra carimbo sem lastro; existe teste que combina ausência de turno aberto, pergunta VIVA, lâmpada apagada e recusa de c1. (`app/chamados/reconciliador.py:374–400`; `app/chamados/tests/test_o261_lavra_le_a_lampada.py:129–146`)

Intervalos recusam na primeira guarda. A justificativa expressa é que ausência de turno aberto não resolve um marco no meio do turno. (`app/chamados/reconciliador.py:329–332`, `:363–364`)

**5. c2 — `reemitir_chamado_do_dia`**

C2 também aceita **somente `orfao_14h` e `saida_sem_entrada`**, porque reutiliza `MOTIVOS_C1`. `intervalo_saida` e `intervalo_volta` retornam `False` imediatamente em **713**, antes de examinar célula, protestos ou existência de chamado. (`app/chamados/reconciliador.py:332`, `:712–713`)

Para os motivos aceitos, a ordem das recusas é:

| Gate | Condição de recusa e linha |
|---|---|
| Data ausente | `not data_ctx` → `False` em **716**. (`app/chamados/reconciliador.py:714–716`) |
| Situação | `colab.situacao != 'ativo'` → **724**. (`app/chamados/reconciliador.py:723–724`) |
| Data inválida | `date.fromisoformat` levanta `ValueError` → **736**. (`app/chamados/reconciliador.py:732–736`) |
| Piso de emissão | Sem chamado vivo encontrado pela chave de c2 e fora de `emissao_no_horizonte` → **744**. (`app/chamados/reconciliador.py:738–744`) |
| Isenção | `cala_para_isento('batida_ausente') and isento_ponto(colab)` → **754**. O módulo pertence aos calados; isenção lê a flag do colaborador. (`app/chamados/reconciliador.py:753–754`; `app/chamados/catalogo/modulos.py:235–236`; `app/core/constants.py:13–18`) |
| Ausência | `ausencia_cobre(..., para='cobranca') is not None` → **757**. Para cobrança, ausência não rejeitada suspende, inclusive pendente. (`app/chamados/reconciliador.py:755–757`; `app/ponto/turnos.py:2475–2483`, `:2518–2523`) |
| Protesto | Sem costura em vivo e sem interseção dos protestos da célula com `PROTESTOS_QUE_COBRAM` → **771**. Célula ausente também cai aqui. (`app/chamados/reconciliador.py:766–771`) |
| Veto da célula | `celula_veta_cobranca` retorna motivo → **776**. (`app/chamados/reconciliador.py:772–776`) |
| Motor | `decidir(ESTADO_NASCENTE, 'EMITIR').permitido == False` → **778**. Na tabela atual essa chamada sempre permite. (`app/chamados/reconciliador.py:777–778`; `app/chamados/catalogo/motor.py:130–132`) |
| Resultado de `abrir` | Chamado devolvido continua fora de `VIVOS` → **806**. (`app/chamados/reconciliador.py:802–806`) |
| Costura | `reapontar_resolvedora` recusa → **822**. (`app/chamados/reconciliador.py:819–822`) |

O piso é **competência `2026-09`**, calculada pela janela da empresa, não pelo mês civil do dia. Não é teto temporal. (`app/ponto/services/cartorio.py:190`, `:232–246`)

Os protestos aceitos são exatamente `FURO_PARCIAL`, `FURO_SEM_COBRANCA`, `FURO_COM_COBRANCA_MORTA`. Estar em `VEREDITOS_ACUSAM` **não implica** ter um deles: a lista de vereditos também contém `indefinida`, `cobrado`, `nunca_bateu`, `cobranca_indevida` e outros. (`app/ponto/services/furos_sem_cobranca.py:35`; `app/escala/models.py:1735–1736`)

O veto da célula recusa `trabalha is False` ou veredito em `nunca_bateu`, `trabalhou`, `justificado`, `coberta`, `cobranca_indevida`. Assim, até alguns vereditos que **acusam** podem legitimamente vetar essa emissão. (`app/ponto/marcos.py:119`, `:155–163`; `app/escala/models.py:1735–1736`)

A exceção `_costura_no_vivo` dispensa **piso e protesto**; não dispensa situação, isenção, ausência, veto da célula ou costura final. Sua busca usa exclusivamente módulo `batida_ausente` e `contexto_json__data_turno`. (`app/chamados/reconciliador.py:738–777`)

Quanto ao **teto temporal**, c2 não chama diretamente `dia_encerrado`: lê os protestos já gravados. O classificador que produz os protestos de furo exige turno encerrado nos respectivos ramos; portanto o teto chega por essa classificação, sem rechecagem explícita em c2. (`app/chamados/reconciliador.py:712–823`; `app/ponto/supra_juiz.py:245–253`, `:280–283`)

Quanto a **“cala”**, o gate direto é `cala_para_isento`. O `_cala_emissao` do cartório é outro predicado — chamado vivo ou fora do piso — e não é chamado por c2; `cartorio.calado()` controla reentrada de julgamento, não devolve veto de c2. (`app/chamados/reconciliador.py:753–754`; `app/ponto/services/cartorio.py:1319–1327`, `:58–68`)

**6. Gates transitivos de `abrir` que terminam no `False` de c2:806**

`abrir` encontra o existente pela chave, tenta renascer apenas se estiver em `ENCERRADOS` e devolve o mesmo objeto, mesmo quando o renascimento recusa. Cancelado/superado não entram nessa tentativa. (`app/chamados/models.py:670–688`; `app/chamados/catalogo/motor.py:30–31`)

O renascimento recusa por:

- Colaborador não ativo, via `cascade`, competência trancada ou recusa de `reabrir`. (`app/chamados/models.py:764–771`)
- Na reabertura: fechamento pelo admin sem fato novo; premissa regularizada; agrupamento sob pai vivo; irmão vivo do turno; transição não permitida. As recusas retornam em **1056, 1062, 1064, 1083 e 1085**. (`app/chamados/models.py:1003–1020`, `:1050–1085`)

A premissa regularizada consulta justificativa aprovada, decisão do admin na célula, dia sem trabalho, lâmpadas dos marcos pedidos, marcos inexistentes e, depois, o coletor do furo. Portanto, **esse gate lê lâmpadas**, não apenas o status do chamado. (`app/chamados/models.py:937–985`)

Competência **exportada** deixou de ser veto; competência **trancada** permanece no renascimento. O juiz da pergunta, por sua vez, permite exceção à tranca quando houve toque posterior do admin; assim, “pergunta VIVA” não elimina universalmente a possibilidade do gate transitivo de tranca. (`app/chamados/models.py:753–769`; `app/chamados/juizes.py:875–907`)

**7. Recusa comum da costura**

Nos três caminhos que chamam `reapontar_resolvedora` — vivo, histórico e c2 — a recusa vem de `resolvedora_valida`; a porta desfaz eventual data acrescentada e não grava o destino. (`app/chamados/services/fio.py:788–801`)

Esse juiz permite ignorância de data/escopo; com datas conhecidas, permite igualdade e diferença de um dia quando o turno cruza meia-noite **ou quando isso é desconhecido**. Fora disso, retorna `(False, motivo)` em **1737**. (`app/chamados/juizes.py:1702–1738`)

**Sobre o defeito pedido, a evidência permite distinguir:**

- **Defeito demonstrável no histórico:** aplica o mapa principal 1:1 onde a regra de atendimento permite os quatro motivos. Pode ignorar chamado encerrado do mesmo dia que atende a pergunta; para intervalos, essa exclusão é absoluta. A ausência de candidato em `hist` não prova ausência legítima de chamado. (`app/chamados/reconciliador.py:1391–1394`; `app/chamados/models.py:2486–2503`; `app/chamados/juizes.py:1597–1599`)
- **Lacuna demonstrável para intervalos:** após falhar o derivador vivo, o histórico nunca os seleciona e c1/c2 recusam pelo motivo. A justificativa escrita para excluir intervalo pertence à **lavra por ausência de turno**, enquanto o chamado do dia atende intervalos e o emissor do cartório trata `FURO_PARCIAL`. Isso impede concluir que esses `False` significam “não há cobrança cabível”. (`app/chamados/reconciliador.py:329–332`, `:712–713`, `:1391–1416`; `app/chamados/juizes.py:1597–1599`; `app/ponto/services/cartorio.py:1381–1396`)
- **Pré-filtros divergentes do juiz:** o derivador elimina qualquer diferença de dia antes de alcançar o juiz que aceita certos casos adjacentes; o histórico busca apenas JSON `data_turno`, enquanto o juiz do dia prioriza célula e chave declarada no catálogo. Podem excluir candidatos admissíveis; ocorrência nesses ~140 **[não verificado]**. (`app/chamados/services/disputa_emissao.py:214–217`; `app/chamados/juizes.py:1730–1738`, `:89–112`; `app/chamados/reconciliador.py:1387`)
- **c1 recusando VIVA é correto:** sua própria regra exige essa recusa para não enterrar furo real. (`app/chamados/reconciliador.py:374–400`)
- **Não há veto pelo status terminal do pai da disputa nesses caminhos.** O derivador filtra o estado dos candidatos; c2 verifica o chamado devolvido por `abrir`. O gate transitivo que lê um pai é `agrupado_em`, e só bloqueia quando esse pai está **vivo** — pai terminal não satisfaz essa guarda. (`app/chamados/services/disputa_emissao.py:181–184`; `app/chamados/reconciliador.py:802–806`; `app/chamados/models.py:581–596`, `:1012–1013`)

## 29/10 23:40 · sessao 936da2887a4a · - · - · origem=Ronald (tmux)

`_derivar_chamado_destino` é a função de `services/disputa_emissao.py`, reexportada por `models.py`; o chamador fornece `p.data_evento`, que precede a data do contexto da disputa. (`app/chamados/models.py:2778`; `app/chamados/reconciliador.py:1369-1370`; `app/chamados/services/disputa_emissao.py:174-179`)

- Candidatos iniciais: colaborador da disputa, `status_local ∈ VIVOS`, excluindo categoria exatamente `disputa_supervisao`; não há filtro direto por `modulo_origem`. `VIVOS` inclui aberto, em análise e registrado. (`app/chamados/services/disputa_emissao.py:181-184`; `app/chamados/catalogo/motor.py:24`)
- Categoria/subcategoria: normaliza os dois rótulos; descarta quando o motivo não pertence a `motivos_atendidos(cat, sub)`. Esse juiz devolve os quatro motivos do dia inteiro quando **qualquer** rótulo pertence aos mapas; módulo e lâmpadas não participam dessa classificação. (`app/chamados/services/disputa_emissao.py:186-196`; `app/chamados/juizes.py:1589-1600`; `app/chamados/models.py:2502-2503`)
- Ocupado: descarta se houver qualquer pergunta com `chamado_destino=ch`, mesmo motivo e `materializada_em IS NULL`; não filtra disputa aberta, dia da pergunta ou `pergunta_viva`. O comentário e os testes declaram intencionalmente o carimbo como régua desse dedup; portanto isso sozinho não prova defeito sob a regra local. (`app/chamados/services/disputa_emissao.py:206-212`; `app/chamados/tests/test_contract_leitores_vitalidade.py:84`; `app/chamados/tests/test_selo_mapa_motivos_do_dia.py:122-143`)
- Dia incompatível: obtém `dia_do_fato(ch)` e descarta quando ambas as datas existem e diferem textualmente. Sem uma das datas, o candidato passa. (`app/chamados/services/disputa_emissao.py:213-218`)
- Ambiguidade: com mais de um, tenta desempatar pelo turno; sem data válida ou entradas abertas devolve candidatos inalterados; casa `entrada_timestamp` com entradas de turnos abertos; dois estágios da mesma entrada escolhem o mais avançado. Sobrando zero ou dois ou mais, retorna `(None, None)` em **227**. (`app/chamados/services/disputa_emissao.py:110-146`; `app/chamados/services/disputa_emissao.py:219-227`)

`None` do derivador **não retorna False diretamente**: entra no histórico. Se faltar data da pergunta e do contexto, retorna False em **1383**. O histórico exige mesmo colaborador, estado em `ENCERRADOS` — somente resolvido/fechado, excluindo cancelado/superado —, `contexto_json__data_turno` exatamente igual a `str(data_ctx)` e categoria diferente de `disputa_supervisao`. (`app/chamados/reconciliador.py:1371-1388`; `app/chamados/catalogo/motor.py:30-31`)

No histórico, o motivo é `MAPA_SUBCATEGORIA.get(sub) or MAPA_CATEGORIA.get(cat)`, com igualdade estrita ao motivo da pergunta; não usa `motivos_atendidos`. Zero históricos tenta c1 e depois c2; nenhum sucesso termina no False **1416**. Dois ou mais históricos também retornam False **1416**, **sem tentar c1/c2**. Exatamente um chama `reapontar_resolvedora`; recusa retorna False **1436**, sucesso chama o resolver e retorna True **1440**. (`app/chamados/reconciliador.py:1389-1416`; `app/chamados/reconciliador.py:1433-1440`)

Com candidato vivo, recusa de `reapontar_resolvedora` retorna False **1448**; não tenta histórico/c1/c2. A porta chama `resolvedora_valida`; recusa não salva destino e desfaz a data que tenha preenchido em memória. (`app/chamados/reconciliador.py:1445-1449`; `app/chamados/services/fio.py:788-801`)

Duas inconsistências verificáveis, com conclusão condicional sobre os 140 casos:

1. **Histórico conserva o mapa de motivo principal.** Seus mapas só produzem `orfao_14h` ou `saida_sem_entrada`, logo `intervalo_saida`/`intervalo_volta` jamais entram em `hist`, mesmo diante do chamado encerrado do dia. A regra já declarada diz que o chamado do turno atende os quatro motivos; há teste explícito de intervalo costurado no turno vivo. Isso demonstra divergência entre o ramo histórico e a regra de motivos atendidos; não prova quantos dos 140 possuem histórico elegível. (`app/chamados/reconciliador.py:1391-1394`; `app/chamados/models.py:2466-2483`; `app/chamados/models.py:2486-2503`; `app/chamados/juizes.py:1597-1599`; `app/chamados/tests/test_selo_mapa_motivos_do_dia.py:60-68`)

2. **Pré-filtro de dia mais estrito que o juiz da costura.** O derivador rejeita qualquer diferença de dia; `resolvedora_valida` autoriza diferença de um dia se o turno cruza meia-noite **ou se não foi possível determinar isso**. Assim, sob essa condição, pode eliminar antes da porta um candidato cuja própria regra da costura permitiria. O histórico também procura exclusivamente JSON `data_turno`, enquanto `dia_do_fato` prioriza a célula e a chave de catálogo; portanto pode deixar de enxergar um encerrado cujo dia oficial coincide. São divergências demonstradas no código; ocorrência concreta nesses 140 não verificada. (`app/chamados/services/disputa_emissao.py:214-217`; `app/chamados/juizes.py:1730-1738`; `app/chamados/reconciliador.py:1385-1388`; `app/chamados/juizes.py:89-112`)

Nenhum desses trechos lê o **estado do pai da disputa** como veto; o derivador filtra o estado dos próprios candidatos e o histórico filtra o estado dos próprios encerrados. (`app/chamados/services/disputa_emissao.py:181-184`; `app/chamados/reconciliador.py:1385-1388`)

## 29/10 23:39 · sessao eeca2e3104d5 · mariaB · verificador · origem=Code

1. **O diff deixa um selo existente vermelho.** O filtro novo em [fabrica_da_celula.py:128](/home/ronald/codex_ws/83bc31eb46db22d481dedfed7b3d9805b1896044/app/chamados/services/fabrica_da_celula.py:128) é detectado pelo scanner de vitalidade, mas `perguntar_pela_celula` não está na allowlist. Executei o scanner em memória: o único ofensor retornado foi essa linha. A asserção existente exige lista vazia. **Classe: pré-requisito de pouso não atendido.** Isso exige registrar corretamente a elegibilidade do escritor; não demonstra, sozinho, um segundo juiz. (`app/chamados/tests/test_contract_leitores_vitalidade.py:161`, `:286`, `:297`)

2. **Há custo síncrono novo na batida, inclusive sem carimbo para curar.** Célula julgada, nenhuma pergunta materializada → o queryset novo faz **um SELECT vazio adicional por chamada**, antes das guardas de emissão. Havendo candidatas, a ausência de `select_related` acrescenta carregamentos por pergunta: célula, disputa, colaborador, empresa e consulta da tranca. **Classe: custo no chokepoint sem certificação correspondente.** (`app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/relavra_via.py:195`, `app/chamados/juizes.py:757`, `:888`, `:896`, `:900`)

   O selo fixa tetos sem folga de 191/241 queries nos cenários que fecham chamados e 134/194 nos que os mantêm vivos; o diff não inclui medição nem justificativa desse acréscimo. Totais reais após a mudança e resultado da execução do selo: **[nao verificado]**. (`app/ponto/tests/test_selo_chokepoint_escrita.py:136`, `:157`, `:163`)

3. **A frota já julgada continua podendo conservar o estado ilegal.** Cenário: dia passado, impressão atual, entrada apagada, saída acesa, pergunta sem resposta carimbada `sem_turno_aberto` e viva → numa execução sem novo julgamento daquela data, o cartório pula pela impressão igual; a data não entra em `cods`, a fábrica não roda e o carimbo permanece. **Classe: cura parcial por alcance; frota sem passagem garantida pela porta.** (`app/ponto/services/cartorio.py:1092`, `:1120`, `:759`, `:769`)

   O lote não resolve esse resíduo: elimina fatos com pergunta existente; o caminho de fio mudo também barra pergunta viva ou já existente. O reatador exige materialização nula. Portanto, esses caminhos não compensam a lacuna. (`app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `:163`, `:220`, `app/chamados/reconciliador.py:1317`)

   Há também mudança do juiz fora do julgamento da célula: pergunta sem `CelulaDia`, em data da janela viva, recebe uma saída sem entrada → `classificar_falta` passa a acusar e o C6 pode considerá-la viva; entretanto, `julgar_celula` retorna imediatamente sem célula. A cura nova nunca chega nela. (`app/escala/servico_jornada.py:452`, `app/chamados/juizes.py:1066`, `app/ponto/services/cartorio.py:1547`)

   **Quem cobraria:** I8 denuncia vivas sem destino; I8bc cobre materializadas com destino inválido. São recortes parciais. O vigia de carimbo examina validadas, enquanto o vigia do fio considera uma pergunta viva como fio existente. Não há nesses caminhos uma cobrança geral desta ilegalidade. (`app/chamados/management/commands/auditar_invariantes_chamados.py:853`, `:890`, `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/vigia_fio.py:155`)

4. **Derrubar o carimbo também reabre disputas históricas e pode duplicar a cobrança.** Pergunta viva carimbada em D1 fechada → a porta limpa materialização/autor/via, audita e chama `reabrir_sistema`, apagando fechamento, resolução e motivo da disputa. O pai recebe nota, mas seu status não é reaberto por esse método. **Classe: efeitos de reabertura sem os pré-requisitos do ciclo.** (`app/chamados/services/relavra_via.py:204`, `:209`, `:220`, `app/chamados/models.py:1725`)

   Dois cenários concretos:

   - **Legado:** pergunta histórica em disputa fechada → julgamento posterior reabre a disputa e devolve-a à cobrança diária. Isso contraria o corte registrado para as vivas históricas em disputas fechadas: aparecer ao admin para validar, sem reabrir e sem push. O aval B registra cura do estado ilegal, mas não explicita revogação desse tratamento histórico. (`app/docs/CORTES.md:326`, `:381`, `app/chamados/services/relavra_via.py:220`)
   - **Outra disputa já aberta:** D1 fechada contém o carimbo; D2 foi aberta posteriormente → curar D1 deixa **duas abertas para o colaborador**, pois a reabertura não consulta D2. O emissor declara expressamente esse resultado como corrupção do invariante. (`app/chamados/models.py:1719`, `app/chamados/services/disputa_emissao.py:437`, `:466`)

   Não há push imediato em `reabrir_sistema`. Porém, o cron posterior percorre todas as abertas, podendo cobrar D1 e D2 separadamente, enviar dois pushes e contar duas pendências como dois colaboradores. Usa ainda a idade original de abertura. Esses são efeitos observáveis além da queda do carimbo. (`app/chamados/models.py:1729`, `app/ponto/management/commands/alertar_disputas_pendentes.py:33`, `:48`, `:52`, `:59`, `:103`)

5. **A varredura pode apagar decisão humana concorrente.** A carrega a pergunta com `sem_turno_aberto`; B executa encerramento administrativo e grava `admin_decidiu`, materialização e autor humano; A continua usando a instância antiga, passa pela guarda de identidade e salva os três campos vazios. A decisão humana desaparece; se A carregar a disputa depois do fechamento, também poderá reabri-la. **Classe: ordem de trava/transação; decisão sobre instância velha.** O queryset novo não trava a pergunta, e a porta escreve sem conferir novamente o estado gravado. É uma fragilidade preexistente da porta agora exposta à varredura automática. (`app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/relavra_via.py:198`, `:207`, `:220`, `app/chamados/models.py:1499`, `:2131`)

Os caminhos e o custo associado à chamada nova são:

| Caminho | Encadeamento |
|---|---|
| Batida | `registrar_batida` → signal → julgamento de D−1 e D → `julgar_colab` → fábrica; paga a consulta nova quando alcança a fábrica. (`app/ponto/registro_batida.py:157`, `app/ponto/signals.py:92`, `:108`, `:131`, `app/ponto/services/cartorio.py:770`) |
| Cron diário | `processar_cartorio --apply` → `julgar_colab` → fábrica. (`app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:65`) |
| Cron de dez minutos | `vigia_de_hora` → `julgar_celula(teto_de_hoje=True)` → mesmo caminho. (`app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`) |
| Lote de perguntas | Chama `perguntar` diretamente; **não passa pela função alterada**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:276`, `:288`) |

Quanto aos quatro testes novos, a análise sem a mudança é: [nao verificado]

| Teste | Resultado esperado sem o diff | [nao verificado]
|---|---|
| Cura da pergunta viva | **Vermelho:** exige materialização nula. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:95`) |
| Morta conserva carimbo | **Verde:** não fazer nada já preserva o estado. (`:108`) | [nao verificado]
| Identidade conserva carimbo | **Verde:** mesmo motivo. (`:116`) | [nao verificado]
| Idempotência | **Verde sem curar:** fotografa a primeira passagem, mas não exige que ela tenha removido o carimbo. (`:122`, `:125`, `:128`) | [nao verificado]

Os dois controles são úteis, mas não são REDs contra a ausência da cura. A bateria chama diretamente a fábrica, com dia passado; não verifica o alcance pelo cartório, os efeitos de reabertura, concorrência, teto temporal ou custo da batida. (`app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:36`, `:81`, `:85`)

Execução Django, reprodução concorrente em banco e os números de sombra citados no docstring: **[nao verificado]**.

## 29/10 23:39 · sessao 754eea13af2e · - · - · origem=Ronald (tmux)

C1 só é chamado quando `_derivar_chamado_destino` não achou destino e `hist` ficou vazio; `False` de C1 prossegue para C2, e só retorna `False` da porta se C2 também falhar (`app/chamados/reconciliador.py:1371`, `:1395`, `:1411-1416`).

| Guarda de C1 | Condição | Retorno |
|---|---|---|
| Escopo | `p.motivo not in MOTIVOS_C1`; lista contém somente `orfao_14h`, `saida_sem_entrada` (`app/chamados/reconciliador.py:332`) | `False` em `app/chamados/reconciliador.py:364`, condição `:363` |
| Resposta | `(p.resposta_colab or '').strip()` preenchido | `False` em `app/chamados/reconciliador.py:366`, condição `:365` |
| Idempotência | `p.materializada_em is not None` | `False` em `app/chamados/reconciliador.py:368`, condição `:367`; excluído pela premissa do universo (`:1321`) |
| Núcleo | `_tem_turno_aberto(colab, dia=dia)` | `False` em `app/chamados/reconciliador.py:373`, condição `:372` |
| Juiz soberano | `pergunta_viva(p)` | `False` em `app/chamados/reconciliador.py:400`, condição `:399` |

`dia` fornecido por `reatar_pergunta_orfa` vem do parse de `data_ctx`; parse inválido produz `None`, e nesse caso C1 substitui por `p.data_evento` (`app/chamados/reconciliador.py:1403-1406`, `:1411`, `:370-371`). Com dia, `_tem_turno_aberto` delega a `turnos_abertos_de`, consultando entrada presente/saída ausente na janela `(dia−1, dia)`; se todos esses turnos têm seu próprio dia posicionalmente completo, o veto devolve lista vazia (`app/chamados/reconciliador.py:171-172`, `:96-99`, `:111-114`). Sem dia, consulta turnos abertos globalmente (`app/chamados/reconciliador.py:173-178`).

Sob a premissa de pergunta **VIVA**, C1 necessariamente recusa: se nenhuma guarda anterior disparar, `pergunta_viva(p)` dispara a última (`app/chamados/reconciliador.py:363-400`). Intervalos recusam já na primeira guarda; o comentário declara que ausência de turno aberto não responde ao marco no meio do turno (`app/chamados/reconciliador.py:329-332`).

**Essa recusa de C1 é legítima conforme a própria regra, não o defeito procurado:** C1 agora lê o juiz da pergunta antes de carimbar, precisamente para não matar pergunta cuja célula acusa (`app/chamados/reconciliador.py:374-400`). O teste monta saída real sem entrada, lâmpada de entrada apagada, célula `furo`, e verifica simultaneamente `_tem_turno_aberto=False`, `pergunta_viva=True`, `lavrar_sem_turno_aberto=False`, sem carimbo nem fechamento da disputa (`app/chamados/tests/test_o261_lavra_le_a_lampada.py:108-125`, `:129-146`). Há o contraponto: pergunta já validada é morta pelo juiz, C1 retorna `True` e carimba a via (`app/chamados/tests/test_o261_lavra_le_a_lampada.py:159-170`).

C1 não consulta o status do chamado pai: o corpo só lê disputa para obter colaborador e, depois de carimbar, verificar se pode fechar a disputa (`app/chamados/reconciliador.py:369`, `:398-416`). O juiz considera célula que acusa suficiente para manter a pergunta viva, após suas precedências de validação/tranca/resposta/identidade (`app/chamados/juizes.py:1005-1057`).

## 29/10 23:38 · sessao 7ded52124fe0 · orcamento-contexto · verificador · origem=Code

O ponto central é que **CLEAR precisa de uma transição própria no Stop**: o caminho atual de `PAREI` julga travas por item e pela existência de outros itens andáveis. Ampliar apenas o reconhecimento de `!` não resolve. (`bin/hook_stop_fila1.py:693`, `:724`, `:661`.)

1. **`PAREI: ! CLEAR` não libera pelo motivo pretendido.** `_SO_BANG` aceita o `!`, mas `_parei_do_em_curso` exige também o ID inteiro do item em curso. Cenário: O261 aberto e topo `PAREI: ! CLEAR | espera Ronald` → continua `siga`. Acrescentar O261 passa esse filtro, mas outro item andável produz “trava so O261”. Consequência: a forma proposta continua bloqueada; esse recado específico pressupõe que o topo também tenha casado O261. ([bin/hook_stop_fila1.py:724](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:724), `bin/hook_stop_fila1.py:73`, `:650`, `:272`, `:780`.)

2. **O caminho genérico também pode liberar CLEAR indefinidamente.** Cenário: topo `PAREI: ! CLEAR O261...`, com todos os demais itens travados. `_relato_parou()` autoriza e `main` libera antes de qualquer outra decisão, sem verificar prazo, sessão ou marco. Consequência: depois de 30 minutos, esse mesmo topo continua liberando se a expiração não tiver precedência explícita sobre o `PAREI` genérico. ([bin/hook_stop_fila1.py:769](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:769), `bin/hook_stop_fila1.py:661`, `:724`.)

3. **TETO pode mascarar o reconhecimento e vencer a expiração.** O código declara `TETO = 5`, mas libera quando `n > TETO`: a sexta tentativa no mesmo HEAD, sem trabalho detectado. `_libera` não limpa o contador. Cenário: CLEAR expirado volta ao fluxo comum com contador já acima do limite → sai silenciosamente pelo TETO, em vez do `siga` prometido. Com suite/push detectado, o teto não cresce nem libera; portanto ele tampouco representa um prazo. ([bin/hook_stop_fila1.py:827](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:827), `bin/hook_stop_fila1.py:37`, `:123`, `:815`, `:820`.)

4. **A retomada aos 30 minutos depende de outra chamada.** `_libera` termina o processo com `sys.exit(0)`. Cenário: CLEAR libera o Stop e Ronald não envia mais nada → essa execução não estará presente aos 30 minutos para devolver `siga`. Consequência: com o mecanismo descrito, a expiração pode ser aplicada **no próximo Stop**; retomada automática exatamente no prazo não está demonstrada. ([bin/hook_stop_fila1.py:133](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:133). Outro mecanismo de reentrada: **[nao verificado]**.)

5. **“Consulta a cada Stop” conflita com as saídas antecipadas de ESPERANDO e PAREI.** Hoje a ordem é prova do RELATO → PAREI → ESPERANDO comprovado → fila/TETO. Cenário: orçamento estoura com produtor vivo declarado em ESPERANDO; consulta colocada depois dessa porta nunca roda. Colocada antes e impondo `siga` incondicionalmente, altera a espera sem turno já selada. Além disso, ESPERANDO libera enquanto há produtor trabalhando: não comprova “nada em voo” nem atendimento do CLEAR. ([bin/hook_stop_fila1.py:763](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:763), `bin/hook_stop_fila1.py:774`; `bin/tests/test_hook_esperando_com_prova.sh:94`, `:106`, `:186`.)

6. **`transcript_path` e a forma de `usage` ainda não estão provados pela árvore.** O hook apenas lê e descarta o stdin. Os selos examinados enviam `{}` ou stdin vazio. Cenário: harness entrega campo diferente, transcript não contém `usage`, ou o leitor seleciona o registro errado → os selos atuais não detectam a medida ausente ou incorreta. Que o Stop real entregue `transcript_path`, onde estejam os três campos e qual seja a janela aplicável: **[nao verificado]**. ([bin/hook_stop_fila1.py:755](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:755), `bin/tests/test_hook_esperando_com_prova.sh:65`; `bin/tests/test_hook_stop_saida_valida.sh:26`.)

7. **Falha da medida pode virar liberação geral.** Cenário: parsing/importação do avaliador lança exceção dentro do `try` principal → o tratamento externo chama `_libera`, inclusive com fila aberta. Consequência: isso não equivale a marcar somente (a) como desconhecido e continuar julgando (b)/(c) e a fila; a falha precisa ser contida na fronteira da medida. ([bin/hook_stop_fila1.py:839](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/hook_stop_fila1.py:839), `bin/hook_stop_fila1.py:758`.)

8. **O ciclo `/clear`/compact não está coberto pelo selo de handoff.** O selo exige `SessionStart` matcher `compact`; procura o comando de impressão em qualquer entrada, sem vinculá-lo ao matcher. Cenário: acrescentar matcher `clear` errado → esse selo pode continuar passando sem provar marco nem retomada pelo handoff. Para compact, o comportamento documentado é imprimir o handoff; isso não demonstra o avanço da geração que deve rearmar o orçamento após compact. A configuração instalada está **[nao verificado]**; `.claude/` é ignorada pelo Git. ([bin/tests/test_handoff_sessao.sh:57](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/tests/test_handoff_sessao.sh:57), `CLAUDE.md:726`; `.gitignore:66`.)

9. **(c) por assunto de commit não tem forma canônica comprovada.** A regra manda escrever “MARCO FECHADO” no **painel**; o fabricante prescreve título `[NOME-DA-FATIA] <o que mudou>`, e o integrador usa a mensagem fornecida. Cenário: marco legítimo fechado com esse título → regex `POUSA`/`MARCO FECHADO` conta zero. Consequência: subcontagem. “Desde o marco” também precisa definir o tratamento dos merges: já existe selo usando `--first-parent` para distinguir pouso da história da raia. Histórico real do `main`: **[nao verificado]**. ([CLAUDE.md:720](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/CLAUDE.md:720), `bin/fabricante_prompt.md:49`; `app/core/integrador_lote.py:481`; `bin/tests/test_lei_akita.sh:107`.)

10. **(c) por deploy também precisa escolher uma testemunha suficiente.** Cenários concretos: [nao verificado]

    - Três deploys antes do próximo Stop deixam apenas o último `deploy.stamp`, sobrescrito → subcontagem. (`bin/deploy.sh:337`, `:341`.)
    - Agendamento termina com `rc=1`, mas grava `.done` → contar existência produz falso positivo; o censo alerta expressamente para isso. (`bin/deploy_agendado.sh:73`, `:79`; `bin/artefatos_de_estado.txt:30`.)
    - Push passa e deploy falha no integrador; o retorno do deploy não é condicionado antes de marcar `FIM`/`NO_AR` → contar finalizações de lote como publicação pode produzir falso positivo. (`app/core/integrador_lote.py:748`, `:750`, `:753`.)

11. **Os selos atuais podem ficar verdes sem provar CLEAR.** O selo de ESPERANDO já apaga o contador entre casos para evitar falso sucesso pelo sexto bloqueio e exige o motivo correto na trilha. Cenário: novo selo verifica apenas stdout vazio → passa pelo TETO ou pela falha aberta. Também é necessário preservar os casos atuais: `PAREI` comum bloqueia quando outro item anda e libera quando os demais estão travados; CLEAR exige sua própria exceção. ([bin/tests/test_hook_esperando_com_prova.sh:63](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/bin/tests/test_hook_esperando_com_prova.sh:63), `bin/tests/test_hook_esperando_com_prova.sh:74`; `bin/tests/test_hook_parei_com_andavel.sh:45`, `:55`.)

## 29/10 23:37 · sessao a913b56dd371 · - · - · origem=Ronald (tmux)

Achados sobre (c), por leitura estática:

1. **Assunto `POUSA`/`MARCO FECHADO` não é a forma estabelecida para commits.** A norma escreve `MARCO FECHADO` no **painel**, após item/cura/lote; o fabricante exige título `[NOME-DA-FATIA] <o que mudou>`, e o integrador usa essa mensagem diretamente. Cenário: marco legítimo com título `[O123] corrige ...` e painel atualizado; a regex retorna zero, subcontando marcos e atrasando o gatilho. (`CLAUDE.md:720-727`; `bin/fabricante_prompt.md:49`; `app/core/integrador_lote.py:466-481`; exemplo de molde em `bin/molde_autocura/msg_commit.molde:1`.)

2. **`deploy.stamp` não oferece histórico contável.** O deploy grava `QUANDO`, `COMMIT`, `TITULO` com sobrescrita e troca do mesmo arquivo; o handoff exibe apenas o último. Cenário: três deploys desde clear, antes do próximo Stop; sobra um carimbo, insuficiente para reconstruir três eventos. (`bin/deploy.sh:335-341`; `bin/handoff_sessao.sh:237-240`.)

3. **Arquivo `.done` não comprova deploy bem sucedido.** O agendador registra qualquer `rc` e copia o resultado para `.done` ao terminar; o próprio censo declara que existência não prova sucesso. Cenário: esteira retorna `rc=1`; contar `.done` como deploy acrescenta marco inexistente e pode disparar (c) falsamente. (`bin/deploy_agendado.sh:73-79`; `bin/artefatos_de_estado.txt:30`.)

4. **Contar execuções de deploy também inclui higiene sem marco novo.** Existe cron diário `deploy.sh --reload-agendado`; o mesmo script termina gravando carimbo de publicação. Cenário: nenhuma obra fechada, apenas reload das 03:30; contador de deploys incrementa mesmo assim. É preciso decidir explicitamente se a unidade passa a ser reload ou marco. (`app/config/crons.py:300-308`; `bin/deploy.sh:48-54`; `bin/deploy.sh:335-342`.)

5. **Contar lotes verdes não garante deploy provado.** `_rodar` devolve `returncode`, mas `_subir` ignora o retorno do deploy e continua marcando fatias `FIM`/`NO_AR`; `_fechar` persiste o registro. Cenário: push passa, deploy falha; contar `logs/lotes.jsonl`/fatias finalizadas como publicação pode produzir falso positivo. (`app/core/integrador_lote.py:242-248`; `app/core/integrador_lote.py:739-758`; `app/core/integrador_lote.py:812-815`.)

Para a opção commits, o recorte também precisa definir tratamento de merges: a casa exige `--no-ff`, e outro selo usa `--first-parent` justamente para não julgar novamente a história da raia. Cenário inferido: contar todos os commits alcançáveis pode contar commits da raia e seu merge como várias unidades; contar por data pode perder commits antigos incorporados depois do clear. (`CLAUDE.md:171-173`; `bin/tests/test_lei_akita.sh:107-109`.)

Histórico real de assuntos no `main` e conteúdo dos logs do host: **[nao verificado]**. Os logs são dados ignorados pelo Git; sua ausência na árvore fornecida não demonstra ausência no host. (`.gitignore:10`; `.gitignore:53-54`.)

## 29/10 23:37 · sessao 5ba6333cbc63 · - · - · origem=Ronald (tmux)

Achados do hook, em leitura:

1. **`! CLEAR` não é forma reconhecida.** `_SO_BANG` aceita qualquer `!`, mas o juiz exige também o ID inteiro do item em curso; logo `PAREI: ! CLEAR | espera Ronald`, com O261 aberto, resulta em `siga: O261...`, mesmo com O261 como único item. Acrescentar O261 passa esse juiz, porém outro item andável impede liberar e produz exatamente “trava so O261”. Portanto o relato desse texto exige que o topo também tenha casado O261; o `! CLEAR` isolado não explica esse ramo. Fontes: `bin/hook_stop_fila1.py:73`, `:693-727`, `:650-663`, `:272-274`, `:779-781`.

2. **O ramo genérico pode liberar CLEAR indefinidamente.** Com topo `PAREI: ! CLEAR O261...` e os demais itens travados, `_relato_parou()` retorna verdadeiro e `main` libera sem consultar tempo, sessão ou marco. Depois de 30 minutos, o mesmo topo continua liberando. Fontes: `bin/hook_stop_fila1.py:661-663`, `:724-727`, `:769-770`.

3. **TETO pode simular sucesso e impedir a retomada.** O limite declarado é 5, mas a condição executada é `n > TETO`: no mesmo HEAD, a sexta tentativa sem trabalho detectado libera. `_libera` não zera o contador; uma implementação da expiração que apenas devolva ao trecho comum pode imediatamente liberar de novo por TETO, em vez de emitir o `siga` exigido. Fontes: `bin/hook_stop_fila1.py:37`, `:123-133`, `:798-836`. O próprio selo de ESPERANDO reconhece e evita a falsa liberação pelo sexto caso apagando o contador entre cenários: `bin/tests/test_hook_esperando_com_prova.sh:63-65`.

4. **A ordem de integração precisa ser explícita.** Hoje a ordem é prova do RELATO → PAREI → ESPERANDO comprovado → fila → TETO. Avaliar orçamento depois de ESPERANDO não consulta “a cada Stop” liberado por esse ramo; retornar antes do checador pula a prova obrigatória do RELATO. Fontes: `bin/hook_stop_fila1.py:763-792`; o selo exige que ESPERANDO válido não pule a prova: `bin/tests/test_hook_esperando_com_prova.sh:186-189`.

5. **ESPERANDO não prova preparação concluída.** Um produtor vivo ou arquivo avançando libera o Stop enquanto o trabalho continua; portanto não serve como evidência de “nada em voo”, nem como atendimento do CLEAR. Fontes: `bin/hook_stop_fila1.py:610-645`, `:774-776`; casos concretos no selo: `bin/tests/test_hook_esperando_com_prova.sh:93-114`.

6. **O teto não oferece prazo de 30 minutos.** Ele mede chamadas por HEAD; com suite/push detectado, não cresce nem se aplica. CLEAR sem ID pode, portanto, continuar recebendo `siga` enquanto esse trabalho existir, independentemente do relógio. Fontes: `bin/hook_stop_fila1.py:386-396`, `:805-827`. Os dois sentidos são exigidos em `bin/tests/test_hook_teto_nao_conta_espera.sh:57-67` e `:69-99`.

7. **Os selos existentes defendem a regra oposta à exceção global de CLEAR.** Exigem bloquear PAREI do item em curso quando há outro andável, liberar quando todos os demais estão travados e rejeitar PAREI de outro item/SONDA. CLEAR precisa de exceção própria sem transformar o `!` genérico em autorização global. Fontes: `bin/tests/test_hook_parei_com_andavel.sh:45-61`; `bin/tests/test_hook_parei_trava_o_item_em_curso.sh:52-75`.

8. **Falha da nova medida não pode escapar para o tratamento geral.** Uma exceção do avaliador dentro do `try` geral libera o Stop, inclusive com fila aberta; isso é diferente de simplesmente ignorar a medida (a) e continuar os demais julgamentos. Fontes: `bin/hook_stop_fila1.py:758`, `:839-840`.

9. **O hook atual descarta o payload inteiro.** `stdin` é lido e ignorado; não há extração de `transcript_path` nesse caminho. O conteúdo real recebido do harness não foi verificado por este explorer. Fonte: `bin/hook_stop_fila1.py:753-757`; payload real: `[nao verificado]`.

Sonda complementar executada apenas em memória, carregando o fonte com `compile/exec` e substituindo leitores por lambdas: reproduziu os quatro resultados de CLEAR descritos nos itens 1–2; nenhum teste shell foi executado nem arquivo escrito. A fundamentação reproduzível desses resultados está nos predicados citados acima. [nao verificado]

## 29/10 23:36 · sessao ab10169c2598 · - · - · origem=Ronald (tmux)

Achados dos selos, sem execução:

1. **O contrato introduz uma exceção global que os selos atuais de `PAREI` não cobrem.** `test_hook_parei_trava_o_item_em_curso.sh:52` exige que `PAREI` de outro item não libere; `:64` exige que o item em curso libere quando o restante está travado. `test_hook_parei_com_andavel.sh:38` usa `PAREI: ! O261`; `:45` exige bloqueio com outro item andável, e `:55` exige liberação somente com o restante travado. Cenário: implementar CLEAR apenas ampliando `_parei_do_em_curso` continua sujeito à trava dos outros itens, porque `_relato_parou` retorna `not _andaveis_fora_do_em_curso(...)` (`bin/hook_stop_fila1.py:661`). Consequência: CLEAR pode continuar bloqueado; se a cura relaxar genericamente esse predicado, quebra a proteção comprovada pelos dois selos.

2. **Silêncio sozinho é prova insuficiente de que CLEAR liberou pela regra correta.** O selo de ESPERANDO remove `hook_stop_fila1.json` em cada caso para impedir uma falsa liberação pelo TETO (`bin/tests/test_hook_esperando_com_prova.sh:63` e `:65`) e exige o motivo `ALLOW ESPERANDO com prova` na trilha (`:74`). Cenário: testar `PAREI ! CLEAR` depois de seis Stops com o mesmo HEAD, verificando somente stdout vazio. Consequência: teste pode passar pelo TETO (`bin/hook_stop_fila1.py:827` e `:836`) ou por falha aberta (`:839`), sem provar reconhecimento de CLEAR.

3. **Os selos existentes não alimentam o novo caminho de medida.** ESPERANDO e TETO enviam `{}` (`bin/tests/test_hook_esperando_com_prova.sh:65`; `bin/tests/test_hook_teto_nao_conta_espera.sh:58`); o selo de saída usa stdin vazio (`bin/tests/test_hook_stop_saida_valida.sh:26` e `:52`). Cenário: avaliador ignora indevidamente `transcript_path`, soma o turno errado ou nunca encontra `usage`. Consequência: esses selos continuam verdes; não provam transporte do payload nem os limiares de (a).

4. **A precedência de ESPERANDO precisa de decisão explícita e selo cruzado.** O selo exige liberação para produtor vivo com fila aberta (`bin/tests/test_hook_esperando_com_prova.sh:40`, `:46`, `:94`) e exige que a prova do RELATO venha antes (`:186`–`:189`). No código, ESPERANDO libera antes de selecionar item e antes de calcular TETO (`bin/hook_stop_fila1.py:774`, `:776`, `:779`, `:815`). Cenário: orçamento estoura com ESPERANDO válido no topo. Consequência: consulta adicionada depois dessa porta não roda; adicionada antes e devolvendo `siga` incondicionalmente, muda a espera sem turno já selada.

5. **O teste de TETO exige que ele continue liberando; CLEAR expirado precisa escapar dessa liberação genérica.** O selo injeta contador 99 e exige silêncio sem trabalho em curso (`bin/tests/test_hook_teto_nao_conta_espera.sh:75`–`:98`). Com suite em curso, exige bloqueio repetido (`:57`–`:65`). Cenário: após 30 minutos o ramo CLEAR expirado cai no fluxo normal com contador acima de cinco. Consequência: `_libera` do TETO vence o `siga` prometido (`bin/hook_stop_fila1.py:827` e `:836`); com suite em curso ocorre o contrário, bloqueia sem prazo por `_esperando` (`:815`–`:827`).

6. **O selo de handoff não prova recuperação por `/clear`.** Ele exige `PreCompact` manual e auto (`bin/tests/test_handoff_sessao.sh:51`–`:56`), mas exige `SessionStart` somente `compact` (`:57`–`:61`); a presença do nome do handoff é procurada em qualquer entrada, sem vincular matcher e comando (`:60`). Cenário: acrescentar matcher clear vazio ou com comando errado. Consequência: selo atual passa e não prova nem escrita do marco nem entrega do handoff à sessão limpa.

7. **Preservar a autoria do topo já é requisito selado.** `test_hook_nao_escreve_parei.sh:13` rejeita escrita direta no RELATO; `:18` rejeita montagem de `PAREI: hook`. Cenário: fazer hook corrigir automaticamente o topo CLEAR expirado ou escrever “clear nao dado, seguiu” nele. Consequência: viola a responsabilidade já testada; o contrato atribui essa escrita ao Code e precisa provar a transição via resposta do Stop, sem torná-la escrita do hook (`bin/tests/test_hook_nao_escreve_parei.sh:4`–`:9`).

Não executar os selos foi necessário para cumprir só leitura: eles criam temporários, processos e arquivos (`bin/tests/test_hook_esperando_com_prova.sh:28`, `:55`, `:62`), e o selo de handoff executa o script contra a raiz viva e exige que tenha gravado `HANDOFF-SESSAO.md` (`bin/tests/test_handoff_sessao.sh:69`–`:72`).

## 29/10 23:36 · sessao 3b3e3d1f6a5e · - · - · origem=Ronald (tmux)

Achados candidatos, todos por leitura:

1. **A cura pode apagar uma decisão humana concorrente.** A consulta nova não trava nem relê a pergunta antes de escrever (`app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/services/relavra_via.py:195`). Cenário: A carrega pergunta viva com `sem_turno_aberto`; B encerra pelo admin, gravando `admin_decidiu` e assinatura humana (`app/chamados/models.py:1499`; `app/chamados/models.py:2131`); A continua com a instância antiga, ultrapassa a guarda e zera esses campos (`app/chamados/services/relavra_via.py:198`; `app/chamados/services/relavra_via.py:204`). Classe: **ordem de trava/transação; decisão por instância velha**. O resultado pode reabrir a disputa que B acabou de encerrar (`app/chamados/services/relavra_via.py:220`).

2. **Reabrir uma disputa histórica pode produzir duas abertas para o mesmo colaborador.** Cenário: D1 fechada contém a pergunta carimbada que voltou a viver; D2 nasceu posteriormente para outro dia. A cura reabre D1 sem consultar D2 (`app/chamados/services/relavra_via.py:220`; `app/chamados/models.py:1719`). O emissor declara explicitamente duas abertas como corrupção do invariante e normalmente agrega na existente (`app/chamados/services/disputa_emissao.py:437`; `app/chamados/services/disputa_emissao.py:466`). Classe: **pouso sem pré-requisito de reabertura**. O alerta diário passa a iterar ambas, podendo enviar duas cobranças ao colaborador e contar duas pendências na supervisão (`app/ponto/management/commands/alertar_disputas_pendentes.py:33`; `app/ponto/management/commands/alertar_disputas_pendentes.py:59`; `app/ponto/management/commands/alertar_disputas_pendentes.py:103`).

3. **Efeitos históricos precisam ser confrontados com o corte de não reabrir em lote.** O corte registrado manda apresentar perguntas vivas de disputas fechadas ao admin, sem reabrir e sem push (`app/docs/CORTES.json:1076`); o registro posterior mantém “só Validar, sem reabrir, sem push”, retirando apenas a exceção de leitura da exportada (`app/docs/RELATO.md:415`). A nova chamada pode reabrir essas disputas pela porta existente (`app/chamados/services/fabrica_da_celula.py:126`; `app/chamados/services/relavra_via.py:220`). O pedido B mais recente pode alterar esse escopo; a autorização exata dos efeitos adicionais é **[nao verificado]**.

Fluxo posterior confirmado:

- Caem apenas materialização/autor/via; resposta e validação permanecem (`app/chamados/services/relavra_via.py:204`).
- Há auditoria e reabertura da disputa fechada (`app/chamados/services/relavra_via.py:209`; `app/chamados/services/relavra_via.py:220`).
- A reabertura limpa resolução/motivo e escreve nota; não reabre o status do pai nem envia push (`app/chamados/models.py:1725`).
- O cron diário pode cobrar e notificar posteriormente, usando a idade original de abertura (`app/ponto/management/commands/alertar_disputas_pendentes.py:48`; `app/ponto/management/commands/alertar_disputas_pendentes.py:52`).
- O pai fechado, isoladamente, não prova questionário inacessível: a lista não filtra status e o fio carrega perguntas de disputas abertas (`app/chamados/services/janela_colab.py:70`; `app/chamados/services/fio.py:181`).
- A pergunta sem destino passa a ser elegível para o reatador após cair o carimbo e reabrir a disputa (`app/chamados/reconciliador.py:1317`).
- Desligamento normal protege perguntas vivas com `admin_decidiu`; não confirmei regressão nesse caso (`app/colaboradores/services/desligamento.py:140`; `app/chamados/models.py:1499`; `app/chamados/services/relavra_via.py:198`).

## 29/10 23:36 · sessao 4ab1f48cb650 · - · - · origem=Ronald (tmux)

Achado procedente: a cura prospectiva não cobre a frota já julgada nem dias sem célula.

- **Classe: meia-correção / frota fora da porta.** Cenário: dia passado com impressão atual, entrada apagada, saída acesa e pergunta `sem_turno_aberto`, sem resposta, materializada e viva. O cartório pula o julgamento pela impressão igual; sem data em `cods`, não chama a fábrica. O carimbo permanece. Referências: `app/ponto/services/cartorio.py:1092`, `app/ponto/services/cartorio.py:1120`, `app/ponto/services/cartorio.py:768`. A nova chamada está em `app/chamados/services/fabrica_da_celula.py:126`.
- O lote também não cobre esse resíduo: exclui fatos com pergunta existente, e o caminho de fio mudo barra pergunta viva ou existente. Referências: `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:182`, `app/chamados/management/commands/disparar_perguntas_competencia.py:220`. O reatador exige `materializada_em IS NULL`: `app/chamados/reconciliador.py:1317`.
- **Outro cenário fora:** pergunta sem `CelulaDia`, em data da janela viva; chega uma saída sem entrada e a lei passa a acusar. O juiz pode devolver viva, mas `julgar_celula` retorna sem célula e nunca alcança a chamada nova. Referências: `app/escala/servico_jornada.py:452`, `app/chamados/juizes.py:1066`, `app/ponto/services/cartorio.py:1547`.

Os medidores existentes são parciais: I8 denuncia perguntas vivas sem destino; I8bc denuncia materializadas com destino inválido. O vigia de carimbo examina perguntas já validadas; o vigia do fio considera esta pergunta viva um fio existente. Referências: `app/chamados/management/commands/auditar_invariantes_chamados.py:853`, `app/chamados/management/commands/auditar_invariantes_chamados.py:890`, `app/chamados/services/vigia_carimbo.py:97`, `app/chamados/services/vigia_fio.py:155`.

Análise estática dos testes novos: [nao verificado]

| Teste | Sem a mudança | [nao verificado]
|---|---|
| Cura da pergunta viva | Falha: exige carimbo removido. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:95` |
| Pergunta morta mantém carimbo | Passa: ausência de ação já preserva o carimbo. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:108` |
| Via de identidade permanece | Passa pelo mesmo motivo. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:116` |
| Idempotência | Passa sem curar: fotografa o estado após a primeira chamada, mas não exige que ela tenha removido o carimbo. `app/chamados/tests/test_o261_julgamento_desfaz_carimbo.py:122` |

Execução dos testes e números de frota citados no docstring: [nao verificado]. [nao verificado]

## 29/10 23:35 · sessao 19d5fcb60622 · - · - · origem=Ronald (tmux)

Achado procedente por inspeção: custo síncrono novo no chokepoint, sem medição/justificativa no selo.

- Estado: batida de dia passado com `CelulaDia` existente que precisa de julgamento, mas nenhuma pergunta materializada. Saída: `perguntar_pela_celula` passa a fazer um `SELECT` vazio adicional antes de qualquer guarda; a iteração do desfazedor força avaliação do queryset. Classe: custo no caminho da batida/lei do selo. Referências: `app/chamados/services/fabrica_da_celula.py:126`, `app/chamados/services/fabrica_da_celula.py:129`, `app/chamados/services/relavra_via.py:195`.
- A consulta não traz `select_related`; cada candidato com célula carrega `p.celula` na chave de memo, e candidatos não validados passam por `p.disputa.colaborador`, empresa e consulta da tranca. Isso acrescenta custo por pergunta materializada, mesmo quando nenhuma será limpa. Referências: `app/chamados/juizes.py:757`, `app/chamados/juizes.py:791`, `app/chamados/juizes.py:888`, `app/chamados/juizes.py:896`, `app/chamados/juizes.py:900`, `app/ponto/services/fechamento.py:1438`.
- O selo declara teto sem folga e fixa 191/241 queries para batidas que fecham chamados e 134/194 para aquelas que deixam chamados vivos. Os respectivos cenários criam célula e medem `registrar_batida`. A nova chamada ocorre depois do julgamento, inclusive se o dia não acusa. Referências: `app/ponto/tests/test_selo_chokepoint_escrita.py:136`, `app/ponto/tests/test_selo_chokepoint_escrita.py:157`, `app/ponto/tests/test_selo_chokepoint_escrita.py:163`, `app/ponto/tests/test_selo_chokepoint_escrita.py:321`, `app/ponto/tests/test_selo_chokepoint_escrita.py:336`, `app/ponto/services/cartorio.py:770`.
- Totais reais após o diff e vermelhos da suite: **[nao verificado]**; nenhum teste com escrita foi executado. [nao verificado]

Mapa das chamadas:

- Batida: `registrar_batida` cria `Batida` → receiver → `_julgar_dias` para d−1 e d → `julgar_celula` → `julgar_colab` → fábrica. Referências: `app/ponto/registro_batida.py:157`, `app/ponto/signals.py:131`, `app/ponto/signals.py:92`, `app/ponto/signals.py:108`, `app/ponto/services/cartorio.py:1560`, `app/ponto/services/cartorio.py:770`.
- Cron diário: `processar_cartorio --apply` → comando → `julgar_colab` → fábrica. Referências: `app/config/crons.py:597`, `app/ponto/management/commands/processar_cartorio.py:65`, `app/ponto/services/cartorio.py:770`.
- Cron a cada dez minutos: `vigia_de_hora --apply` → `rodar` → `julgar_celula(..., teto_de_hoje=True)` → mesmo caminho. Referências: `app/config/crons.py:348`, `app/ponto/services/vigia_de_hora.py:130`.
- Lote `disparar_perguntas_competencia`: **não passa pela função modificada**; chama `perguntar` diretamente, apenas nos alvos que sobreviveram aos filtros, dentro de `atomic` e normalmente com push silenciado. Referências: `app/chamados/management/commands/disparar_perguntas_competencia.py:124`, `app/chamados/management/commands/disparar_perguntas_competencia.py:163`, `app/chamados/management/commands/disparar_perguntas_competencia.py:276`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`, `app/chamados/management/commands/disparar_perguntas_competencia.py:295`.

Cuidado ao afirmar que o caminho só visita células efetivamente rejulgadas: `processar_cartorio` compartilha `out` entre colaboradores, enquanto `out['cods']` e a seleção da fábrica usam somente a data; uma data julgada em colaborador anterior pode fazer a fábrica visitar a célula pulada do seguinte. É comportamento preexistente relevante para o alcance/custo desta chamada nova. Referências: `app/ponto/management/commands/processar_cartorio.py:43`, `app/ponto/management/commands/processar_cartorio.py:59`, `app/ponto/services/cartorio.py:1120`, `app/ponto/services/cartorio.py:759`, `app/ponto/services/cartorio.py:769`.

## 29/10 23:30 · sessao eee5391edadd · remede20 · leitura · origem=Code

São **4 crons na tupla literal**: `disparar_perguntas_competencia`, `fechar_cobranca_com_lastro`, `reavaliar_ausencias_feriado` e `reconciliar_chamados`. `reconciliar_perguntas_orfas` já saiu e virou VIGIA. (`app/config/crons.py:1959`, `app/config/crons.py:1964`, `app/config/crons.py:1975`, `app/config/crons.py:1976`, `app/config/crons.py:1981`)

| (a) Cron/command | (b) Que pergunta JULGA / escreve | (c) Juiz ou EVENTO existente | (d) Obra registrada da qual depende | (e) Falta em código para VIGIA: esperado 0, só alarma | (f) Esforço |
|---|---|---|---|---|---|
| **`disparar_perguntas_competencia`** — command: `app/chamados/management/commands/disparar_perguntas_competencia.py:33`; encadeado ainda com `--comp-corrente --apply`: `app/config/crons.py:1456`. | “Que marco vencido e acusado ainda precisa de pergunta?”, incluindo chamado vivo sem pergunta viva — **fio mudo**. Emite pela fábrica, podendo desfazer carimbo sem lastro, abrir/atualizar disputa e gerar perguntas. (`app/chamados/management/commands/disparar_perguntas_competencia.py:111`, `app/chamados/management/commands/disparar_perguntas_competencia.py:163`, `app/chamados/management/commands/disparar_perguntas_competencia.py:288`; `app/chamados/services/fabrica_da_celula.py:98`) | O julgamento da célula já chama **`perguntar_pela_celula`**, que consulta `motivos_a_perguntar`; o prazo delega a `cartorio.marcos_vencidos`. O próprio command declara que o universo fio mudo ainda não tem evento. (`app/ponto/services/cartorio.py:770`; `app/chamados/services/fabrica_da_celula.py:68`, `app/chamados/services/fabrica_da_celula.py:27`; `app/chamados/management/commands/disparar_perguntas_competencia.py:12`) | **DISPARAR-PERGUNTAS-NO-CONTRATO-20**, depois do grupo (1) do CENSO-DE-LEITORES; a ordem registrada manda tratá-lo pela **C11**. (`app/docs/BACKLOG.md:765`, `app/docs/BACKLOG.md:7`) | Cobrir o **fio mudo no evento**; o modo `--alarme` sem escrita e com `SystemExit(2)` já existe. Depois de comprovar zero, trocar `--apply` por `--alarme` no encadeado e migrar o papel junto da retirada da tupla. (`app/chamados/management/commands/disparar_perguntas_competencia.py:12`, `app/chamados/management/commands/disparar_perguntas_competencia.py:261`; `app/config/crons.py:1456`, `app/config/crons.py:1863`; `app/docs/BACKLOG.md:1218`) | **M — estimativa [não verificado]**: fábrica e alarme já existem; resta cobrir o segundo universo, identificado pelo command. (`app/chamados/management/commands/disparar_perguntas_competencia.py:12`, `app/chamados/management/commands/disparar_perguntas_competencia.py:131`, `app/chamados/management/commands/disparar_perguntas_competencia.py:261`) |
| **`fechar_cobranca_com_lastro`** — command: `app/ponto/management/commands/fechar_cobranca_com_lastro.py:39`; cron ainda com `--apply`: `app/config/crons.py:705`. | “Todos os marcos cobrados neste `batida_ausente` vivo já têm lastro?” Consulta lâmpadas; sem lâmpada, faz casamento próprio por tipo e proximidade temporal. Reconcilia o chamado e anexa ocorrência. (`app/ponto/services/lastro.py:264`, `app/ponto/services/lastro.py:282`, `app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:319`) | **`lastro.marcos_acesos`**, já chamado pelo cartório para fechar no julgamento. Para o casamento sem lâmpada, existem **`_match_marcos` / `marco_ocupado_pela_batida`**. (`app/ponto/services/lastro.py:153`; `app/ponto/services/cartorio.py:1162`; `app/escala/utils.py:551`, `app/escala/utils.py:353`) | **CONTRATO-20-LASTRO-E-ORFAS** e **LASTRO-DEGRAU-2**. A prescrição antiga pelo raio foi superada: deve perguntar ao juiz do par, dentro de **JUIZ-DO-PAR-GRUPO-1**. Há ainda a divergência registrada em **GRADE-DEPENDE-DA-JANELA**. (`app/docs/BACKLOG.md:728`, `app/docs/BACKLOG.md:1472`, `app/docs/BACKLOG.md:763`, `app/docs/BACKLOG.md:793`) | Substituir o casamento próprio pela autoridade indicada; implementar `--alarme` sem fechamento/nota; trocar o `--apply` agendado e o papel após prova de zero. Já existe `contador()` esperado zero, mas o command só oferece `--apply`/`--tolerancia`. (`app/ponto/services/lastro.py:282`, `app/ponto/services/lastro.py:334`; `app/ponto/management/commands/fechar_cobranca_com_lastro.py:43`; `app/config/crons.py:705`; `app/docs/BACKLOG.md:1218`) | **M — estimativa [não verificado]**: fechamento reativo e contador existem, mas falta unificar o casamento e tratar a divergência de janela registrada. (`app/ponto/services/cartorio.py:1162`; `app/ponto/services/lastro.py:334`; `app/docs/BACKLOG.md:793`) |
| **`reavaliar_ausencias_feriado`** — command: `app/ponto/management/commands/reavaliar_ausencias_feriado.py:4`; cron ainda com `--apply`: `app/config/crons.py:589`. | “Há `batida_ausente` vivo em feriado/folga, com fase conhecida e premissa morta?” Com aplicação, reconcilia e grava ocorrência/trilha. (`app/chamados/services/feriado_reavaliar.py:23`, `app/chamados/services/feriado_reavaliar.py:33`, `app/chamados/services/feriado_reavaliar.py:38`, `app/chamados/services/feriado_reavaliar.py:43`) | **`precedencia.fatos_do_dia`**, registrado em `core/juizes.py`; o serviço já o consulta. Eventos existentes: `Feriado.post_save → reavaliar_dia` e regeneração de células `→ reavaliar_dias`. (`app/core/juizes.py:244`; `app/chamados/services/feriado_reavaliar.py:33`; `app/chamados/signals.py:265`; `app/ponto/portas/celula.py:421`) | **CONTRATO-20-E2-QUAL-ATO**: decisão registrada manda evento na porta `marcar_trabalha_em_feriado`, apenas em competência aberta, e rebaixamento com esperado zero e caso que morde. (`app/docs/BACKLOG.md:798`) | Implementar o gancho nessa porta, que hoje salva e retorna; respeitar o recorte de competência aberta. Acrescentar contagem/alarme sem encerramento, trocar o `--apply` agendado e migrar o papel após comprovar zero. (`app/colaboradores/services/vinculo.py:822`; `app/docs/BACKLOG.md:798`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:9`, `app/ponto/management/commands/reavaliar_ausencias_feriado.py:28`; `app/config/crons.py:589`, `app/config/crons.py:1863`) | **M — estimativa [não verificado]**: juiz e serviço são reutilizáveis; faltam gancho com guarda de competência e modo alarme. (`app/chamados/services/feriado_reavaliar.py:23`; `app/colaboradores/services/vinculo.py:822`; `app/docs/BACKLOG.md:798`) |
| **`reconciliar_chamados`** — command: `app/chamados/management/commands/reconciliar_chamados.py:8`; cron `*/15`: `app/config/crons.py:376`. | “A premissa do chamado morreu?” Também retrata perguntas, reata órfãs e religa chamados às células; o command ainda executa esses efeitos. (`app/chamados/reconciliador.py:1264`, `app/chamados/reconciliador.py:1265`, `app/chamados/reconciliador.py:1277`; `app/chamados/management/commands/reconciliar_chamados.py:51`, `app/chamados/management/commands/reconciliar_chamados.py:56`) | **C3: `premissa_morta`**. Eventos já existentes: batida chama reconciliador; emissão reata órfã; regeneração religa célula; **O-OUTBOX já consome absolvição do chamado e encerramento de órfãs pela lâmpada**. (`app/core/juizes.py:229`; `app/ponto/signals.py:207`; `app/chamados/services/disputa_emissao.py:406`; `app/ponto/portas/celula.py:421`; `app/chamados/reconciliador.py:1214`, `app/chamados/reconciliador.py:1217`) | **O-OUTBOX** e **CONTRATO-20-DOIS-EVENTOS**. O consumidor a cada minuto **já está declarado**, portanto não falta criar o outbox. (`app/docs/BACKLOG.md:726`, `app/docs/BACKLOG.md:1427`; `app/config/crons.py:375`) | Converter o command inteiro em leitura: retirar reconciliação, rejunção e dreno mutante; contar os resíduos das partes restantes pelas mesmas autoridades e alarmar acima de zero. Hoje os contadores da parte 3 só imprimem. Depois da prova de zero, migrar papel/tupla. (`app/chamados/management/commands/reconciliar_chamados.py:51`, `app/chamados/management/commands/reconciliar_chamados.py:56`, `app/chamados/management/commands/reconciliar_chamados.py:60`, `app/chamados/management/commands/reconciliar_chamados.py:69`; `app/config/crons.py:1863`; `app/docs/BACKLOG.md:1218`) | **M — estimativa [não verificado]**: eventos e contador parcial existem, mas o command reúne vários efeitos que precisam virar fiscalização sem escrita. (`app/chamados/management/commands/reconciliar_chamados.py:51`, `app/chamados/management/commands/reconciliar_chamados.py:60`, `app/chamados/management/commands/reconciliar_chamados.py:69`) |

**Contadores atualmente zerados em execução: [não verificado].**

## 29/10 23:29 · sessao bbd22f08ce77 · - · - · origem=Ronald (tmux)

**`reconciliar_chamados`** — (a) literal em `app/config/crons.py:1976`, agendado */15 em `app/config/crons.py:376`, command `app/chamados/management/commands/reconciliar_chamados.py:8`; (b) pergunta se a premissa do chamado morreu, retrata chamados e resolve perguntas/justificativa pendente (`app/chamados/reconciliador.py:1277`, `app/chamados/reconciliador.py:505`), também reata perguntas órfãs (`app/chamados/reconciliador.py:1265`) e religa chamados à célula (`app/chamados/management/commands/reconciliar_chamados.py:56`); (c) autoridade existente C3 `premissa_morta` (`app/core/juizes.py:229`), avaliação delegada ao catálogo (`app/chamados/reconciliador.py:256`, `app/chamados/reconciliador.py:301`); eventos existentes: Batida chama reconciliador (`app/ponto/signals.py:206`), emissão reata órfã (`app/chamados/services/disputa_emissao.py:406`), regeneração religa célula (`app/ponto/portas/celula.py:421`), outbox recebe a lavra e consome absolvição/fantasma (`app/ponto/portas/celula.py:634`, `app/chamados/reconciliador.py:1214`); (d) **O-OUTBOX**, registrado em `app/docs/BACKLOG.md:726`, já consta como pousado no histórico em `app/docs/BACKLOG.md:14`, e consumidor minuto já está declarado em `app/config/crons.py:375`; **CONTRATO-20-DOIS-EVENTOS**, `app/docs/BACKLOG.md:1427`; (e) falta transformar o command inteiro em contador sem escrita e com alarme: hoje ele ainda executa reconciliação/rejunção (`app/chamados/management/commands/reconciliar_chamados.py:51`) e drena consumidor mutante (`app/chamados/management/commands/reconciliar_chamados.py:60`); contador esperado zero existe apenas para a parte 3 e só imprime (`app/chamados/management/commands/reconciliar_chamados.py:69`), portanto faltam contadores das partes restantes, alarme e prova de zero antes de sair da tupla (`app/docs/BACKLOG.md:1218`); (f) **M, estimativa [não verificado]**: eventos e contador parcial já existem, mas o command reúne vários efeitos e ainda escreve (`app/chamados/management/commands/reconciliar_chamados.py:51`, `app/chamados/management/commands/reconciliar_chamados.py:60`).

## 29/10 23:28 · sessao 6da8f08d73fc · - · - · origem=Ronald (tmux)

**`fechar_cobranca_com_lastro`** — (a) literal em `app/config/crons.py:1964`; cron continua com `--apply` em `app/config/crons.py:705`; command em `app/ponto/management/commands/fechar_cobranca_com_lastro.py:39`. (b) Julga “todos os marcos cobrados deste `batida_ausente` vivo já têm lastro?”; lê lâmpadas e, sem lâmpada, deriva proximidade por tipo/horário; chama `reconciliar()` e escreve ocorrência de fechamento (`app/ponto/services/lastro.py:196`, `app/ponto/services/lastro.py:264`, `app/ponto/services/lastro.py:279`, `app/ponto/services/lastro.py:317`, `app/ponto/services/lastro.py:319`). (c) **Existente:** `lastro.marcos_acesos(ch, cel)` responde por marco, e o cartório já o chama e fecha no julgamento reativo (`app/ponto/services/lastro.py:153`, `app/ponto/services/cartorio.py:1162`); a batida acorda esse julgamento por signal (`app/ponto/signals.py:85`, `app/ponto/signals.py:93`). Para o degrau sem lâmpada, já existe o alinhador `_match_marcos` e seu leitor `marco_ocupado_pela_batida` (`app/escala/utils.py:551`, `app/escala/utils.py:353`). (d) **CONTRATO-20-LASTRO-E-ORFAS**, condicionado à **LASTRO-DEGRAU-2**: a prescrição antiga por raio foi superada pela leitura do juiz do par; também registrados **JUIZ-DO-PAR-GRUPO-1** e **GRADE-DEPENDE-DA-JANELA** (`app/docs/BACKLOG.md:728`, `app/docs/BACKLOG.md:1471`, `app/docs/BACKLOG.md:1472`, `app/docs/BACKLOG.md:763`, `app/docs/BACKLOG.md:793`). (e) Falta substituir o casamento próprio `abs(timestamp-alvo)<=tol` pela autoridade registrada; implementar `--alarme` sem fechamento/nota, substituir o `--apply` agendado e mover o papel somente após medir zero na sombra e caso positivo que morda (`app/ponto/services/lastro.py:279`, `app/ponto/management/commands/fechar_cobranca_com_lastro.py:43`, `app/config/crons.py:705`, `app/docs/BACKLOG.md:1218`). Já existe contador esperado zero, mas conta candidatos `com_lastro`; para representar quem efetivamente fecharia, existe `reconciliaria()`, que usa o mesmo gate sem escrever (`app/ponto/services/lastro.py:334`, `app/chamados/models.py:447`, `app/chamados/models.py:467`). Zero atual na árvore fornecida: **[não verificado]**. (f) **M — estimativa [não verificado]:** contador e fechamento reativo existem, mas falta unificar o degrau de casamento e resolver a divergência registrada da janela antes do rebaixamento (`app/ponto/services/lastro.py:334`, `app/ponto/services/cartorio.py:1162`, `app/docs/BACKLOG.md:793`).

## 29/10 23:28 · sessao 67e870be9593 · - · - · origem=Ronald (tmux)

- **(a)** `reavaliar_ausencias_feriado`: entrada literal em `app/config/crons.py:1975`; agenda ainda com `--apply` em `app/config/crons.py:589`; comando em `app/ponto/management/commands/reavaliar_ausencias_feriado.py:4`.
- **(b)** Julga “há `batida_ausente` vivo em dia que o juiz considera feriado/folga, com fase conhecida e premissa morta?” (`app/chamados/services/feriado_reavaliar.py:14`, `:23`, `:33`, `:34`, `:36`, `:38`). Com `--apply`, reconcilia cada candidato (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:32`), anexa ocorrência e grava trilha (`app/chamados/services/feriado_reavaliar.py:43`, `:47`, `:49`, `:52`).
- **(c)** Já existe juiz: `ponto.precedencia.fatos_do_dia`, registrado em `app/core/juizes.py:244`, com implementação sem escrita em `app/ponto/precedencia.py:179`; o cron já o consulta via serviço (`app/chamados/services/feriado_reavaliar.py:27`, `:33`). Já existem eventos usando o mesmo serviço: `Feriado.post_save → reavaliar_dia` (`app/chamados/signals.py:265`, `:274`, `:276`) e regeneração de células `→ reavaliar_dias`, condicionada a `recalculo` e dias tocados (`app/ponto/portas/celula.py:421`, `:431`, `:433`). `chamados.juizes.dia_do_fato` fornece o dia do chamado (`app/chamados/services/feriado_reavaliar.py:26`, `:30`); `dia_sem_trabalho` responde pela célula em `app/chamados/juizes.py:386`, mas o serviço do cron efetivamente usa `fatos_do_dia`, não esse predicado (`app/chamados/services/feriado_reavaliar.py:27`, `:33`).
- **(d)** Obra específica: `CONTRATO-20-E2-QUAL-ATO`, cuja decisão manda evento na porta `marcar_trabalha_em_feriado` apenas em competência aberta e rebaixamento a VIGIA, com esperado zero e caso que morde (`app/docs/BACKLOG.md:798`). A árvore ainda o mantém juiz (`app/config/crons.py:1975`); os comentários nomeiam lacunas de alcance do evento atual (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:21`, `:22`; `app/chamados/signals.py:259`, `:261`).
- **(e)** Falta implementar esse evento na porta, que hoje só atribui/salva/retorna (`app/colaboradores/services/vinculo.py:822`–`:826`), respeitando o recorte aberto decidido (`app/docs/BACKLOG.md:798`); trocar a execução mutante por contagem/alarme — hoje só há `--apply`, seleção e encerramento (`app/ponto/management/commands/reavaliar_ausencias_feriado.py:9`–`:11`, `:24`, `:28`–`:33`); trocar `--apply` na agenda (`app/config/crons.py:589`) e retirar da tupla no mesmo pouso que estabelecer papel VIGIA (`app/config/crons.py:1863`–`:1865`, `:1975`). O backlog exige esperado zero e caso que morde, mas o código desse comando ainda não oferece `--alarme` (`app/docs/BACKLOG.md:798`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:9`–`:11`). Medida atual efetivamente zero em produção: **[não verificado]**.
- **(f)** **M — estimativa [não verificado]**: serviço julgador e dois eventos já existem (`app/chamados/services/feriado_reavaliar.py:23`, `:64`; `app/chamados/signals.py:265`; `app/ponto/portas/celula.py:421`), mas faltam o gancho da porta e sua guarda de competência, modo alarme e troca de configuração (`app/colaboradores/services/vinculo.py:822`; `app/docs/BACKLOG.md:798`; `app/ponto/management/commands/reavaliar_ausencias_feriado.py:9`; `app/config/crons.py:589`, `:1975`).

## 29/10 23:27 · sessao 73c5d1d5cfbc · remede20 · sonda · origem=Code

LINHA_DO_PLACAR: **contratos_estruturais: 19/20 verdes** — transcrição do dado fornecido. [nao verificado]  
TOTAL: **20** | VERDES: **19** — transcrição do dado fornecido. [nao verificado]  
ERROS: **0** — transcrição de `ERROS=0` da sonda fornecida. [nao verificado]

- **chamado x um juiz por pergunta:** a nota registra 2 perguntas sem juiz (“disputa fecha”, SLA), 58 sítios respondendo por conta própria (4 dinheiro, 39 cobrança, 15 tela) e condição de permanecer PARCIAL até O168, O169 e LASTRO; “o que falta é juiz”. Teste: `chamados.tests.test_contract_juiz_chamado.JuizChamadoContratoTest`; exceções declaradas: `config.crons.JUIZES_POR_VARREDURA` e `core.juizes.PENDENTES_CHAMADO`. [app/core/contratos_estruturais.py:261](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/app/core/contratos_estruturais.py:261)
- **escala x parametro consumido ou sem efeito:** `teste=None`, `excecoes=()`, `nota=''`, `idempotencia=None`; nenhuma falta descrita, conforme a célula padrão devolvida para chave ausente. [app/core/contratos_estruturais.py:1048](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/app/core/contratos_estruturais.py:1048) [app/core/contratos_estruturais.py:62](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/app/core/contratos_estruturais.py:62)
- **chamado x parametro consumido ou sem efeito:** `teste=None`, `excecoes=()`, `nota=''`, `idempotencia=None`; nenhuma falta descrita, conforme a mesma célula padrão. [app/core/contratos_estruturais.py:1048](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/app/core/contratos_estruturais.py:1048) [app/core/contratos_estruturais.py:62](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/app/core/contratos_estruturais.py:62)

As duas células de parâmetro de escala e chamado ficam fora do denominador por ausência de cadastro; não são dois pontos pendentes dos 20. [app/core/contratos_estruturais.py:7](/home/ronald/codex_ws/fb51ec3bc7a2394296ef35dc6726e68613a26e73/app/core/contratos_estruturais.py:7)

_300 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
