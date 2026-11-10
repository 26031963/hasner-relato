# CODEX-ESTADO

> Ultima resposta processada: **10/11/2026 00:24** · publicado em **10/11/2026 00:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 10/11 00:24 · sessao 70d18c1fa24a · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · censo · origem=Code

**Há um caminho que explica a perda de retorno por vencimento, mas ele depende de o colaborador continuar sem nenhuma batida apurável na vida.** Nesse ramo, o cartório considera a célula em dia pela impressão igual, sem incluir nela o avanço dos marcos. No ramo com adesão, ocorre o contrário: `nunca_bateu` impede o pulo. A sonda não demonstra a adesão atual de cada colaborador; atribuir esse mecanismo aos 137 itens seria **[nao verificado]**. (`app/ponto/services/cartorio.py:1165-1173`, `:1207-1216`, `:1366-1375`)

Uma precisão sobre o dado: o resumo fornecido registra **139/139 com `trabalha=True`**; os **135** correspondem aos itens sem evento posterior ao dia civil, e outros quatro têm evento posterior. As referências abaixo ao resumo contam as linhas do anexo textual. (`sonda1_resumo.txt:7-9`)

1. **Caminhos que podem voltar à célula e respectivas guardas**

| Caminho | Como deveria chegar ao julgamento/fábrica | O que impede ou limita o retorno |
|---|---|---|
| **Vigia de hora**, a cada dez minutos | Seleciona células de hoje e ontem, calcula vencidos e chama `julgar_celula(..., teto_de_hoje=True)`. (`app/config/crons.py:367`; `app/ponto/services/vigia_de_hora.py:114-131`) | Exige `trabalha=True`; exclui desligado/isento; ontem depende de `_ontem_em_janela`; sem vencidos pula; com batidas suficientes e sem cobrança viva pula. **Não tem guarda própria de “já julgada” ou `nunca_bateu`.** (`app/ponto/services/vigia_de_hora.py:21-23`, `:117-129`) |
| **Cartório chamado pelo vigia — sem adesão** | Entra no ramo `adesao is None`, definido pela ausência de qualquer batida apurável na vida. (`app/ponto/services/cartorio.py:1165-1173`) | `_em_dia_nb = bool(cel.julgada_em and cel.impressao == imp0 and cel.veredito)`; se verdadeiro e sem `forcar`, pula. `imp0` **não recebe teto nem encerramento**: o simples vencimento seguinte não muda esse insumo. (`app/ponto/services/cartorio.py:1207-1216`) |
| **Cartório chamado pelo vigia — com adesão** | Calcula teto de hoje/ontem e inclui a quantidade de vencidos na impressão. (`app/ponto/services/cartorio.py:1324-1344`, `:1359-1365`) | Pula impressão igual somente quando também `veredito != 'nunca_bateu'`. Portanto **`nunca_bateu` não bloqueia esse ramo**. (`app/ponto/services/cartorio.py:1369-1375`) |
| **`processar_cartorio`, madrugada** | Configurado com `--apply`; seleciona todas as células passadas da empresa e chama `julgar_colab`. O SQL não exclui julgadas nem `nunca_bateu`. (`app/config/crons.py:616`; `app/ponto/management/commands/processar_cartorio.py:49-65`) | `data__lt=hoje` exclui o dia corrente; depois valem os dois pulos do cartório acima. O cron configurado não passa `--forcar`. (`app/ponto/management/commands/processar_cartorio.py:50`, `:62`, `:65`; `app/config/crons.py:616`) |
| **Consumidor de `EventoCelula`**, a cada minuto | Para as datas dos eventos, relê as células atuais e chama `perguntar_pela_celula`; **não rejulga previamente a célula**. (`app/config/crons.py:394`; `app/chamados/reconciliador.py:1384-1387`; `app/chamados/services/fabrica_da_celula.py:279-292`) | Só lê eventos não processados e com menos de cinco tentativas. Sem evento pendente, encerra o lote. Sem falha ou adiamento específico de órfãs, marca processado, mesmo havendo marcos futuros cortados pela fábrica. (`app/chamados/reconciliador.py:1209`, `:1249`, `:1343-1345`, `:1375-1378`, `:1393-1401`) |
| **`disparar_perguntas_competencia`**, corrente da manhã | No universo da célula, apenas chama `motivos_que_o_evento_deixou` e conta. (`app/config/crons.py:1521-1523`; `app/chamados/management/commands/disparar_perguntas_competencia.py:109-113`) | **Não fabrica nesse universo**, inclusive com `--apply`: alarma o estoque. Seu segundo universo ainda escreve, mas exige chamado vivo sem pergunta — o “fio mudo”. (`app/chamados/management/commands/disparar_perguntas_competencia.py:141-161`, `:240-251`, `:270-283`) |
| **Eventos de batida, ausência, justificativa e chamado** | Save/delete de batida ou ausência, save de justificativa e nascimento/mudança de status de chamado podem chamar o mesmo juiz. (`app/ponto/signals.py:131-165`; `app/chamados/signals.py:187-216`) | Dependem de ocorrer uma escrita; não são disparados pelo relógio. Há guarda contra reentrada durante julgamento; chamado sem mudança de status retorna. Na porta avulsa, hoje sem `teto_de_hoje` espera o fim do turno. (`app/ponto/signals.py:90-93`; `app/chamados/signals.py:192-193`, `:210-211`; `app/ponto/services/cartorio.py:1993-1998`) |

**Há ainda uma restrição anterior à fábrica direta:** `julgar_colab` só chama `perguntar_pela_celula` para datas presentes em `out['cods']` deste ato. O ramo sem adesão lavra/carimba e retorna sem registrar essa data em `cods`; o registro ocorre no ramo normal. Assim, naquele ramo, a fábrica pode ser alcançada pelo outbox da lavra, mas não por essa chamada direta. (`app/ponto/services/cartorio.py:938-944`, `:997-1008`, `:1253-1279`, `:1437`; `app/ponto/portas/celula.py:892-916`)

Os outros caminhos próximos não fornecem uma revisita geral de todos os marcos de uma célula sem batidas:

- **`detectar_ausencias`**, a cada cinco minutos, tenta abrir chamado; pula se já existe e seu funil veta `nunca_bateu`. Logo, não substitui o retorno marco a marco da fábrica. (`app/config/crons.py:364`; `app/ponto/management/commands/detectar_ausencias.py:424-439`, `:630-633`; `app/ponto/marcos.py:119`, `:155-162`)
- **`reconciliar_grade`** está configurado sem `--apply`; seu emissor também consulta esse veto e só emite chamado, sem pergunta própria. (`app/config/crons.py:460-462`; `app/ponto/management/commands/reconciliar_grade.py:502-504`, `:507-532`)
- **Alertas de turno** exigem entrada; **detector de intervalo** exige entrada e saída acesas, e sua emissão está aposentada. Esses requisitos excluem o caso de nenhuma batida no dia. (`app/ponto/management/commands/processar_alertas_turno.py:92-93`; `app/ponto/management/commands/detectar_intervalo_ausente.py:69-70`, `:149-156`)
- **`emitir_furo_retroativo`** é passe sob ordem, não esse cron periódico, e pula colaborador sem batidas na janela. **Cobrar dia manualmente** chama o juiz com `forcar=True`, mas só para passado. (`app/config/crons.py:1396`; `app/ponto/management/commands/emitir_furo_retroativo.py:53-56`; `app/chamados/services/cobrar_dia.py:28-41`)
- Mudança de vínculo, regeneração/reversão e queda de veto também têm chamadas ao juiz; dependem desses atos, sem agenda de vencimento de marco. (`app/colaboradores/services/pos_vinculo.py:61-72`; `app/ponto/portas/celula.py:422-434`, `:815-817`; `app/escala/regua_defesa.py:687-692`)

2. **Qual guarda importa depois do primeiro julgamento**

Para o caso central, não é `julgada_em` preenchida isoladamente: é a conjunção **carimbo + impressão igual + veredito preenchido**, no ramo sem adesão. O relógio dos marcos não entra na impressão desse ramo; no ramo normal, entra. (`app/ponto/services/cartorio.py:1207-1216`, `:1359-1375`)

Também há uma segunda idempotência na escrita: `carimbar` retorna sem salvar quando impressão, ausência e indicadores permanecem iguais e não houve nova lavra. Mesmo chegar novamente à porta não garante movimentar `julgada_em` ou produzir evento. O outbox normal só acorda por campos lavrados de **veredito/ata**. (`app/ponto/portas/celula.py:1070-1081`, `:892-916`)

O teto da fábrica tem outro papel: **corta motivos**, não agenda retorno. Além dele, a fábrica verifica dia sem trabalho/colaborador ativo, futuro, veredito que acusa, parcial na lista e identidade do fato já perguntado. `nunca_bateu` passa pelo teste de acusação porque pertence a `VEREDITOS_ACUSAM`. (`app/chamados/services/fabrica_da_celula.py:105-150`; `app/escala/models.py:1767-1768`)

3. **O julgamento das 09:00 devolveu quais motivos? Quem deveria voltar?**

**A lista histórica efetivamente devolvida às 09:00 é [nao verificado].** O anexo não contém o retorno daquela execução nem seu DNA/ata/dedup histórico; os exemplos mostram horários de julgamento/evento e ausência atual de perguntas. (`sonda1_resumo.txt:10`, `:12-19`)

Pelo código, uma chamada à fábrica naquele instante só poderia devolver os motivos dos marcos faltantes **já vencidos há 30 minutos**. `vencidos_no_instante` chama `marcos_vencidos`; `motivos_pela_ata` exclui explicitamente cada `(tipo, hora)` fora desse conjunto. Portanto, **sim: os marcos seguintes ainda não vencidos são cortados**. (`app/ponto/services/cartorio.py:828`, `:856-859`; `app/chamados/services/fabrica_da_celula.py:35-49`, `:58-73`)

O mapa é:

| Marco faltante | Motivo |
|---|---|
| Início/E | `saida_sem_entrada` |
| Ida do intervalo/S | `intervalo_saida` |
| Volta do intervalo/E | `intervalo_volta` |
| Fim/S | `orfao_14h` |

Fonte: `app/chamados/services/declaracao_texto.py:42-46`.

O desenho geral diz “EVENTO carimba e julga”, consumidor lê a lâmpada e cron de varredura vigia. Entretanto, a configuração declara expressamente a exceção temporal: **o vigia de hora ocupa o papel `prazo`, e “a célula ainda não agenda o próprio marco”**. O responsável implementado pelo retorno por vencimento é esse vigia, delegando ao cartório; não aparece uma agenda independente de evento “marco venceu”. (`CLAUDE.md:166-170`; `app/config/crons.py:2092-2097`, `:2127-2131`)

`_marco_venceu_apagado` existe, mas apenas compara vencidos entre dois instantes e lê a ata; **não emite evento**. Os emissores do outbox registram lavra, chamado, fechamento de turno, alteração de pergunta, reversão ou semeadura. (`app/ponto/services/cartorio.py:403-423`; `app/ponto/portas/celula.py:794-796`, `:896-905`; `app/chamados/signals.py:194-207`)

4. **Por que 09/11 aparece no estoque observado às 00:12 de 10/11?**

O universo passa a admitir 09/11 quando `hoje` vira 10/11, pois exige `data < hoje`. Ontem só é excluído se `_ontem_em_janela(...)` devolver verdadeiro. Portanto, os 28 itens de 09/11 presentes na sonda **já passaram pelo resultado falso dessa função**; isso demonstra sua presença naquela medição, não o instante exato em que surgiram. (`app/chamados/management/commands/disparar_perguntas_competencia.py:100-113`; `sonda1_resumo.txt:1`, `:5`; instante de surgimento: **[nao verificado]**)

As condições de `_ontem_em_janela` são:

- Sem fim calculável: `False`. (`app/ponto/services/vigia_de_hora.py:48-50`)
- Último marco + 30 minutos anterior à meia-noite de hoje: `False`. (`app/ponto/services/vigia_de_hora.py:51-54`)
- Antes de fim + dois tetos: `True`; depois, depende de o dia ainda não estar encerrado em `agora - dois tetos`. (`app/ponto/services/vigia_de_hora.py:55-58`)

**O vigia de hora não alcança esses mesmos itens naquele instante**, pois usa precisamente a guarda inversa: se ontem e fora dessa janela, pula. No dia seguinte, eles também saem do universo hoje/ontem do vigia. Continuam possíveis o cartório de passado e eventos/atos posteriores, sujeitos às guardas anteriores. (`app/ponto/services/vigia_de_hora.py:21-23`, `:120-121`; `app/ponto/management/commands/processar_cartorio.py:49-65`)

5. **Quem escreve, quem lê e o esmeril semântico**

| Campo | Escrita efetiva |
|---|---|
| `julgada_em` e `impressao` | `carimbar`, nas atribuições e persistência da porta; chamado pelos dois ramos do cartório. (`app/ponto/portas/celula.py:1074-1081`; `app/ponto/services/cartorio.py:1275-1277`, `:1833`) |
| `veredito` | `lavrar_veredito`, pela porta; cartório e decisão administrativa a utilizam. Há também cópia explícita **somente em memória**, para leitura do julgamento em curso. (`app/ponto/portas/celula.py:1044-1049`, `:941-956`; `app/ponto/services/cartorio.py:1253`, `:1813`; `app/ponto/services/veredito_celula.py:351`) |

O comentário que ainda chama `processar_cartorio` de escritor único descreve a organização antiga; a atribuição efetiva está na porta acima. (`app/escala/models.py:1738-1746`; `app/ponto/portas/celula.py:1074-1081`)

**Leitores de `julgada_em` e da impressão comparada:**

| Uso | Leitores |
|---|---|
| Pulo e idempotência | Cartório nos dois ramos; `carimbar`. (`app/ponto/services/cartorio.py:1210-1216`, `:1369-1375`; `app/ponto/portas/celula.py:1070-1073`) |
| Julgamento anterior / parcial na lista | Cartório, fábrica, emissão, reconciliador, cluster, disparador e retroativo. (`app/ponto/services/cartorio.py:954`, `:1718`; `app/chamados/services/fabrica_da_celula.py:117`; `app/chamados/services/disputa_emissao.py:934-938`; `app/chamados/reconciliador.py:878-882`; `app/ponto/services/cluster_espurio.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`) |
| Invalidação de leitura/lavra | Veredito lavrado e fase 12×36. (`app/chamados/veredito_lavrado.py:191-194`; `app/escala/services/fase_12x36.py:188-193`) |
| Auditoria/backfill/diagnóstico | Vigia B5, backfill B5, invariantes e diagnóstico de veredito velho; este último também lê `impressao`. (`app/ponto/management/commands/vigia_b5_ausencia.py:55`; `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1597`; `app/ponto/management/commands/celula_veredito_velho.py:109-110`) |

**Leitores operacionais de `veredito`, agrupados pelo efeito:**

- **Acusar/absolver e manter perguntas/chamados:** fábrica, juízes, modelo da pergunta, reconciliador, emissão, resposta tardia, limbo e silêncio por ausência. (`app/chamados/services/fabrica_da_celula.py:113`; `app/chamados/juizes.py:1321`, `:1372-1387`; `app/chamados/models.py:3356-3361`; `app/chamados/reconciliador.py:637-641`, `:1027-1030`; `app/chamados/services/disputa_emissao.py:769`; `app/chamados/services/resposta_tardia.py:45`; `app/chamados/services/limbo_folgas.py:194`; `app/chamados/services/silencio_ausencia.py:40-41`)
- **Vetar cobrança:** `celula_veta_cobranca`, lido pelos emissores e serviço de furos. (`app/ponto/marcos.py:119`, `:155-162`; `app/ponto/management/commands/detectar_ausencias.py:631-633`; `app/ponto/management/commands/reconciliar_grade.py:502-504`; `app/ponto/services/furos_sem_cobranca.py:98`)
- **Retenção, exposição e ata:** exportação, vigia de pausa, prévia, relatório da frota e leitor da célula. (`app/folha/export.py:1233-1255`; `app/folha/services/vigia_pausa.py:157`; `app/folha/previa.py:94`; `app/relatorios/dia_da_frota.py:51-60`; `app/escala/services/leitor_celula.py:424`)
- **Adesão/cadastro/proposição:** adesão, Cadastro × Realidade, propositor e régua de defesa. (`app/chamados/services/adesao.py:34`; `app/escala/services/cadastro_realidade.py:181`, `:477`, `:618`; `app/escala/services/propositor.py:474`; `app/escala/regua_defesa.py:688`)
- **Proteção da decisão humana:** calendário, serviço de veredito, mudanças de vínculo e portas de regeneração/reversão. (`app/colaboradores/services/calendario.py:411-416`; `app/ponto/services/veredito_celula.py:109`; `app/colaboradores/services/pos_vinculo.py:61`; `app/ponto/portas/celula.py:429`, `:816`)
- **Vigias, pautas e censos:** vigia do fio, furo sem canal, furos sem cobrança/vetados, discordantes, ausência × veredito, diagnóstico e pautas. (`app/chamados/services/vigia_fio.py:379`; `app/chamados/services/furo_sem_canal.py:35`, `:72`; `app/ponto/services/furos_sem_cobranca.py:61`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/pautas_dos_discordantes.py:71`; `app/ponto/services/vigia_ausencia_veredito.py:42`, `:56`; `app/ponto/services/diagnostico_escala.py:177`; `app/pautas/services.py:422`)

**Sim, existem semânticas diferentes para `nunca_bateu`:**

- No ramo sem adesão, qualquer veredito preenchido com impressão igual basta para considerar o julgamento atualizado; no ramo com adesão, `nunca_bateu` exige rejulgamento. (`app/ponto/services/cartorio.py:1210-1216`, `:1369-1375`)
- No veto de emissão, `nunca_bateu` **cala cobrança**; na fábrica, nas perguntas e na retenção ele **continua acusando**, por pertencer a `VEREDITOS_ACUSAM`. (`app/ponto/marcos.py:119`, `:161-162`; `app/escala/models.py:1767-1768`; `app/chamados/services/fabrica_da_celula.py:113`; `app/folha/export.py:1233`)
- O comentário “`nunca_bateu` = fato resolvido” ainda existe imediatamente antes da tupla que o inclui entre os que acusam. É divergência documental explícita, não prova de encerramento uniforme no código. (`app/escala/models.py:1760-1768`)

## 10/11 00:23 · sessao f64053de7b9b · - · - · origem=Ronald (tmux)

O censo aponta três sentidos diferentes para `nunca_bateu`:

- **Ainda acusa** no vocabulário efetivo: está em `CelulaDia.VEREDITOS_ACUSAM` (`app/escala/models.py:1767-1768`).
- **Veta cobrança de batida** no gate compartilhado: está em `VEREDITOS_QUE_VETAM` (`app/ponto/marcos.py:119`); `celula_veta_cobranca` lê `trabalha/veredito` e retorna veto (`app/ponto/marcos.py:155-162`). Os emissores obedecem em `app/ponto/management/commands/detectar_ausencias.py:631-633` e `app/ponto/management/commands/reconciliar_grade.py:502-504`.
- **Julgamento em dia enquanto não existe adesão**: o ramo `adesao is None` calcula impressão sem teto (`app/ponto/services/cartorio.py:1173`, `1207-1209`) e pula quando há `julgada_em`, impressão igual e qualquer veredito (`1210-1216`). Quando já existe adesão, a guarda deliberadamente **exclui** `nunca_bateu` do pulo (`1366-1375`).

Existe comentário contraditório dizendo `nunca_bateu = fato resolvido` em `app/escala/models.py:1761`, mas o conjunto executado abaixo inclui a palavra entre os que acusam (`1767-1768`). O comentário não demonstra execução.

**Escritores efetivos**

| Campo | Escritor e chamadas |
|---|---|
| `julgada_em`, `impressao` | `carimbar`: atribuições em `app/ponto/portas/celula.py:1074-1075`; persistência em `1081`. Chamado pelos dois ramos do cartório: `app/ponto/services/cartorio.py:1275`, `1833`. |
| `veredito` | `lavrar_veredito`: atribuição/persistência em `app/ponto/portas/celula.py:1044-1049`. O método do modelo delega (`app/escala/models.py:1819-1828`). Chamadores: cartório (`app/ponto/services/cartorio.py:900`, `1253`, `1813`) e decisão humana (`app/ponto/services/veredito_celula.py:351`). |
| Cópia de `veredito` em memória | `celula_como_o_julgamento_deixa`, explicitamente sem gravação (`app/ponto/portas/celula.py:941-956`). |

`carimbar` também lê ambos os campos para idempotência e não grava se os insumos e a lavra não mudaram (`app/ponto/portas/celula.py:1070-1073`). Somente mudança de `veredito` ou `ata` acorda o consumidor pelo save; alteração isolada do carimbo não basta (`app/ponto/portas/celula.py:893`, `908-916`).

**Leitores de `julgada_em`**

- Pulo e instantâneo anterior: `app/ponto/services/cartorio.py:954`, `1210`, `1369`.
- Cobertura parcial e fato novo: `app/ponto/services/cartorio.py:400`, `1718`; `app/chamados/services/fabrica_da_celula.py:117`; `app/chamados/services/disputa_emissao.py:934-938`; `app/chamados/reconciliador.py:878-882`; `app/ponto/services/cluster_espurio.py:104`; `app/chamados/management/commands/disparar_perguntas_competencia.py:154`; `app/ponto/management/commands/emitir_furo_retroativo.py:121`.
- Invalidação temporal: `app/chamados/veredito_lavrado.py:191-194`; `app/escala/services/fase_12x36.py:188-193`.
- Universo de auditoria/backfill: `app/ponto/management/commands/vigia_b5_ausencia.py:55`; `app/ponto/management/commands/backfill_b5_insumos.py:61`; `app/chamados/management/commands/auditar_invariantes_chamados.py:1597`.
- Diagnóstico: `app/ponto/management/commands/celula_veredito_velho.py:109`.

**Leitores de `impressao` da célula**

- Pulo: `app/ponto/services/cartorio.py:1210`, `1369`.
- Idempotência: `app/ponto/portas/celula.py:1071`.
- Diagnóstico: `app/ponto/management/commands/celula_veredito_velho.py:110`.

O hash inclui o relógio quando o chamador entrega `teto` (`app/ponto/services/cartorio.py:460-464`); o ramo sem adesão não entrega esse argumento (`1207-1209`).

**Leitores operacionais de `veredito`, por grupo**

- Acusação/absolvição: fábrica (`app/chamados/services/fabrica_da_celula.py:113`), juiz de pergunta viva (`app/chamados/juizes.py:1372-1387`), consulta das perguntas (`app/chamados/models.py:3356-3361`), reconciliador (`app/chamados/reconciliador.py:637-641`, `1027-1030`), emissão (`app/chamados/services/disputa_emissao.py:769`), resposta tardia (`app/chamados/services/resposta_tardia.py:45`), limbo (`app/chamados/services/limbo_folgas.py:194`), silêncio por ausência (`app/chamados/services/silencio_ausencia.py:40-41`). Esses leitores usam o conjunto que inclui `nunca_bateu`; não o absolvem por essa palavra.
- Retenção/exibição: `app/folha/export.py:1233-1255`; `app/folha/services/vigia_pausa.py:157`; `app/folha/previa.py:94`; `app/relatorios/dia_da_frota.py:51-60`; `app/escala/services/leitor_celula.py:424`; `app/pautas/services.py:422`; `app/ponto/services/diagnostico_escala.py:177`.
- Cadastro/adesão: `app/chamados/services/adesao.py:34`; `app/escala/services/cadastro_realidade.py:181`, `477`, `618`; `app/escala/services/propositor.py:474`; `app/escala/regua_defesa.py:688`.
- Decisão humana e proteção: `app/colaboradores/services/calendario.py:411-416`; `app/ponto/services/veredito_celula.py:109`; `app/ponto/portas/celula.py:429`, `816`; `app/colaboradores/services/pos_vinculo.py:61`.
- Vigias: `app/chamados/services/vigia_fio.py:379`; `app/ponto/services/furos_sem_cobranca.py:61`; `app/ponto/services/furos_vetados.py:61`; `app/ponto/services/pautas_dos_discordantes.py:71`; `app/ponto/services/vigia_ausencia_veredito.py:42`.
- O vigia de furo sem canal só seleciona `furo/cobrado`, deixando `nunca_bateu` fora desse censo, sem declará-lo resolvido (`app/chamados/services/furo_sem_canal.py:35`, `72`).
- Auditorias: `app/chamados/management/commands/auditar_invariantes_chamados.py:198`, `377`, `1438`, `1597`, `1714`, `1802`; `app/ponto/management/commands/ausencias_sem_efeito_na_celula.py:58-62`; `app/ponto/management/commands/recusar_ponta_pequena.py:145`; `app/ponto/management/commands/celula_veredito_velho.py:88`, `106`.

Não encontrei outro leitor executável que declare literalmente `nunca_bateu` como “resolvido”; o veto compartilhado e a guarda de impressão do ramo sem adesão são as duas traduções operacionais que impedem avanço apontadas acima (`app/ponto/marcos.py:119`, `161-162`; `app/ponto/services/cartorio.py:1210-1216`).

## 10/11 00:23 · sessao 18185c182d52 · - · - · origem=Ronald (tmux)

Caminhos e guardas encontrados, somente por leitura da cópia:

| Caminho | Comportamento e guarda exata |
|---|---|
| **Vigia de hora** | Cron a cada 10 minutos, `--apply` (`app/config/crons.py:367`). Busca `trabalha=True`, datas hoje e ontem (`app/ponto/services/vigia_de_hora.py:21-23`), exclui desligado/isento (`:117-119`), ontem fora da janela (`:120-121`), sem marco vencido (`:124-126`) e lâmpadas suficientes sem cobrança viva (`:127-129`). Chama `julgar_celula(...teto_de_hoje=True)` (`:131`). **Não exclui diretamente `nunca_bateu` ou `julgada_em` preenchida.** |
| **Cartório, ramo sem adesão** | Adesão significa primeira batida apurável **na vida**, sem filtro de data (`app/ponto/services/cartorio.py:1165-1167`); ramo começa quando `adesao is None` (`:1173`). `imp0` não recebe teto, vencidos ou encerramento (`:1207-1209`); considera em dia se `julgada_em`, impressão igual e qualquer veredito (`:1210`), e pula sem `forcar` (`:1214-1216`). O avanço dos marcos sozinho não altera essa impressão. Esse ramo também não alimenta `out['cods']`; retorna em `:1279`, enquanto a fábrica síncrona exige a data em `out['cods']` (`:997`, `:1007-1008`). |
| **Cartório, ramo com adesão** | Teto entra na impressão (`app/ponto/services/cartorio.py:1359-1365`). A guarda **exclui `nunca_bateu` do pulo** (`:1369`); demais células em dia pulam (`:1373-1375`). Logo, “sem batida no dia” e “sem batida na vida” têm comportamentos diferentes. |
| **Cartório diário** | Agenda 06:27, `--apply`, sem `--forcar` (`app/config/crons.py:616`). Universo passado, sem limite temporal inferior (`app/ponto/management/commands/processar_cartorio.py:49-51`, `:61-65`); limite opcional corta colaboradores (`:56-57`). Esbarra nas mesmas guardas do serviço, incluindo o ramo sem adesão. |
| **Sinais de fatos** | Batida julga d−1 e d (`app/ponto/signals.py:108-112`, `:131-140`); ausência (`:143-152`), justificativa (`:161-165`), criação ou mudança de status de chamado (`app/chamados/signals.py:192-193`, `:208-216`). São acionamentos por escrita de fatos. O juiz não julga hoje antes do fim do turno sem `teto_de_hoje` (`app/ponto/services/cartorio.py:1993-1998`), e mantém as guardas de impressão. |
| **Disparar perguntas da competência** | Primeiro universo **somente conta** (`app/chamados/management/commands/disparar_perguntas_competencia.py:109-113`) e alarma (`:247-251`); não chama fábrica para as células contadas. Segundo universo escreve somente para chamado vivo sem pergunta viva (`:141`, `:148-149`), pelo motivo da ata com teto (`:158-161`), chamando `perguntar` (`:271-283`). |
| **Detectar ausências** | Cron de cinco minutos (`app/config/crons.py:364`), hoje/ontem (`app/ponto/management/commands/detectar_ausencias.py:136-138`). Exige marco vencido e dia ainda não encerrado (`:470-473`); chamado existente causa `continue` (`:424-430`). Chama `_abrir_chamado` (`:439`), cujo funil veta `nunca_bateu` (`:630-633`, `app/ponto/marcos.py:119`, `:155-162`). Pode acordar o cartório indiretamente ao criar/mudar chamado, não a cada marco seguinte. |
| **Alertas de turno** | `processar_alertas_turno` pula sem entrada (`app/ponto/management/commands/processar_alertas_turno.py:92-93`). Avançados pula se `turno_aberto_ha` retorna `None` (`app/ponto/management/commands/processar_alertas_avancados.py:63-65`); juiz retorna `None` sem turno aberto ou sem entrada (`app/ponto/turnos.py:3190-3191`). |
| **Detectar intervalo ausente** | Exige entrada inicial e saída final acesas (`app/ponto/management/commands/detectar_intervalo_ausente.py:69-70`); emissão aposentada, `continue` incondicional (`:149-156`). Cron usa `--alarme` (`app/config/crons.py:491`). |
| **Reconciliar grade** | Cron sem `--apply` (`app/config/crons.py:462`); comando declara medição sem escritor (`app/ponto/management/commands/reconciliar_grade.py:3-11`). Eventual bancada emissora só cria chamado, com veto da célula (`:502-504`); caminho de pergunta eliminado (`:531-532`). |
| **Apurar furos diários** | Lê a ata (`app/escala/services/furos_diarios.py:41-43`) e grava `FuroDiario` (`:107-109`); não rejulga nem chama fábrica. Encadeado após cartório (`app/config/crons.py:1509`). |
| **Vigia de entrada faltante** | Apenas lê e imprime (`app/ponto/management/commands/vigia_entrada_faltante.py:23-53`, `app/ponto/services/vigia_entrada.py:78-85`). |
| **Emitir furo retroativo** | **Sob ordem**, não cron (`app/config/crons.py:1396`). Pula sem batida na janela (`app/ponto/management/commands/emitir_furo_retroativo.py:53-56`), exige `FURO_SEM_COBRANCA` (`:111-112`), pula se há chamado (`:148-150`), e delega ao mesmo funil que veta `nunca_bateu` (`:154`; `app/ponto/marcos.py:119`, `:161-162`). |
| **Portas administrativas/cadastro** | Cobrar dia exige passado e força julgamento (`app/chamados/services/cobrar_dia.py:28-30`, `:41`); pós-vínculo força, exceto humano (`app/colaboradores/services/pos_vinculo.py:61-72`); regeneração e reversão forçam nos dias tocados elegíveis (`app/ponto/portas/celula.py:422-434`, `:815-817`). Queda do veto seleciona somente `veredito='furo'`, deixando `nunca_bateu` fora (`app/escala/regua_defesa.py:687-688`). Não são agenda de vencimento de marco. |

**Evento de marco vencido:** o cadastro declara explicitamente que a célula **ainda não agenda o próprio marco**, e atribui ao vigia o papel `prazo` (`app/config/crons.py:2092-2097`, `:2130-2131`). Isso qualifica a regra geral EVENTO/cron do `CLAUDE.md:168-170`.

**Ontem às 00:12:** `disparar` inclui datas `< hoje` (`app/chamados/management/commands/disparar_perguntas_competencia.py:102`), excluindo ontem **somente** quando `_ontem_em_janela` é verdadeira (`:110-111`). O vigia usa a mesma função e exclui quando falsa (`app/ponto/services/vigia_de_hora.py:120-121`). Portanto, os itens de ontem efetivamente contados pela sonda já não estavam na janela do vigia naquele instante; não é prova de que nasceram precisamente às 00:12 [nao verificado].

A janela de ontem devolve falso sem fim de turno (`app/ponto/services/vigia_de_hora.py:49-50`) ou quando `fim + teto < meia-noite` (`:53-54`); mantém até `fim + 2*teto` (`:55-56`), depois depende de `not dia_encerrado(agora - 2*teto)` (`:58`). O próprio comentário registra que, fora disso, `disparar` apenas conta/alarma e `julgada_em` não serve como corte (`:39-43`).

**Limite da atribuição ao dado:** os horários e o veredito fornecidos não demonstram qual ramo de adesão cada célula executa atualmente; falta batida no dia não demonstra ausência de batida na vida [nao verificado].

## 10/11 00:22 · sessao e0bd137fd9b5 · - · - · origem=Ronald (tmux)

Achados para consolidar, todos somente pela cópia:

1. **Não confundir “sem batida no dia” com o ramo `nunca_bateu`.** A seleção desse ramo pergunta pela primeira batida apurável **da vida**, excluindo retratadas: `app/ponto/services/cartorio.py:1165-1173`. Ter veredito `nunca_bateu` na sonda não demonstra que `adesao is None` ainda vale hoje: [nao verificado].

2. **Cartório, ramo sem adesão:** a impressão `imp0` recebe batidas vazias, cobertura, chamados vazios, DNA, regra, parcial e isenção, **sem `teto` nem `enc`** (`app/ponto/services/cartorio.py:1207-1209`). A guarda é:
   ```python
   _em_dia_nb = bool(cel.julgada_em and cel.impressao == imp0 and cel.veredito)
   if _em_dia_nb and not forcar:
       out['pulados'] += 1
       continue
   ```
   `app/ponto/services/cartorio.py:1210-1216`. Esse ramo retorna em `:1279`, antes da lógica normal de teto em `:1324-1344`. Portanto mero avanço dos marcos não altera esse hash nesse ramo (`:1207-1209`).

3. **Fábrica direta no julgamento:** só chama `perguntar_pela_celula` quando a data está no `out['cods']` **deste ato**, não no acumulado do lote: `app/ponto/services/cartorio.py:938-944`, `:992-1008`. A única atribuição de resultado por data do corpo é no ramo normal em `:1437`; o ramo sem adesão termina em `:1279` sem essa atribuição. Assim o ramo sem adesão pode lavrar/carimbar e emitir evento, mas não passa pela fábrica direta nessa estrutura (`:1253-1279`, `:997-1008`, `:1437`).

4. **Cartório, ramo com adesão:** o pulo faz o contrário do que uma leitura superficial sugeriria: `_em_dia` exige `cel.veredito != 'nunca_bateu'`. Logo, nesse ramo, `nunca_bateu` **não** é pulado por impressão igual (`app/ponto/services/cartorio.py:1366-1375`). O teto por quantidade de marcos vencidos entra no hash para hoje e para ontem alcançado pelo vigia (`:1324-1344`, `:1359-1365`); a função de hash acrescenta `teto:N` quando fornecido (`:460-464`). Comentário declara expressamente que isso evita deixar o marco da tarde esperando o cartório seguinte (`:460-462`).

5. **Cron do cartório:** `processar_cartorio` seleciona todas as células passadas da empresa, sem piso temporal, e chama `julgar_colab`; o filtro SQL **não** elimina julgadas nem `nunca_bateu` (`app/ponto/management/commands/processar_cartorio.py:49-65`). `data__lt=hoje` impede pegar o dia corrente (`:50`, `:62`); depois os pulos são os descritos acima. `--forcar` existe e é repassado (`:29`, `:65`); não inferir seu uso em produção: [nao verificado].

6. **Consumidor:** lê apenas eventos pendentes com `tentativas < 5`, não todas as células (`app/chamados/reconciliador.py:1209`, `:1246-1249`, `:1342-1345`). Para as datas do lote ele relê as células atuais e chama a mesma fábrica, sem rejulgar previamente (`app/chamados/reconciliador.py:1384-1387`; `app/chamados/services/fabrica_da_celula.py:279-292`). A execução trabalha em lotes de 500 e prazo de 45 s, podendo continuar no próximo cron (`app/chamados/reconciliador.py:1211`, `:1315`, `:1343`, `:1406-1407`). Sem falhas nem adiamento de órfãs, marca o evento processado mesmo que a fábrica tenha motivos futuros ainda cortados (`:1375-1387`, `:1393-1401`). O adiamento que preserva pendente depende de `_orf['adiadas'] > 0`, não da existência genérica de marcos futuros (`:1375-1378`, `:1396-1399`). Portanto a fila vazia não programa retorno para o próximo marco (`:1249`, `:1343-1345`, `:1401`).

7. **O que às 09h seria perguntável:** o teto é marco + 30 min, comparado por instante (`app/ponto/services/cartorio.py:828`, `:848-859`). `vencidos_no_instante` obtém esse conjunto (`app/chamados/services/fabrica_da_celula.py:35-49`); sem vencidos retorna nenhum motivo (`:58-61`), e exclui cada faltante cujo `(tipo,hora)` não esteja no conjunto (`:68-70`). O mapa é início/E → `saida_sem_entrada`; fim/S → `orfao_14h`; ida do intervalo/S → `intervalo_saida`; volta/E → `intervalo_volta` (`app/chamados/services/declaracao_texto.py:42-46`). Logo marcos posteriores ainda não vencidos não podiam aparecer naquela chamada (`app/chamados/services/fabrica_da_celula.py:68-73`). **Lista histórica efetivamente devolvida às 09h: [nao verificado]**: o anexo não traz o DNA/ata/dedup e retorno daquela execução. Não afirmar que a primeira pergunta nasceu: a sonda diz que hoje não há pergunta do fato, e o ramo especial nem passa pela fábrica direta como explicado acima.

8. **Não foi encontrado emissor de evento “marco venceu”.** O escritor de `EventoCelula` é `emitir_evento_celula`, que só insere identificadores da célula/colaborador/data (`app/ponto/portas/celula.py:896-905`). O mecanismo normal da lavra só emite quando alterou `veredito` ou `ata` (`:892-916`), e lavra idêntica retorna sem evento (`:1027-1030`). Chamadores declarados: lavra, nascimento de chamado de turno, fechamento de turno aberto, mudança de pergunta que solta chamado e semeadura (`app/escala/models.py:1845-1855`). Há ainda reversão explícita do DNA (`app/ponto/portas/celula.py:794-796`) e volta do chamado a vivos (`app/chamados/signals.py:194-207`). `_marco_venceu_apagado` existe, mas é **leitor puro**, compara vencidos entre dois instantes e lê a ata; não emite (`app/ponto/services/cartorio.py:403-423`). O desenho declarado exige evento julgar e consumidor ler a lâmpada; cron de varredura só vigia (`CLAUDE.md:166-170`). O código tem o relógio do vigia como mecanismo explícito de passagem de marco (`app/ponto/services/cartorio.py:825-828`), não um evento temporal independente nesses emissores (`app/ponto/portas/celula.py:896-916`).

9. **Outros gatilhos reativos:** save/delete de batida (`app/ponto/signals.py:131-140`), save/delete de ausência (`:143-152`), decisão/save de justificativa (`:161-165`), nascimento/mudança de status de chamado (`app/chamados/signals.py:187-216`). Não disparam pelo simples passar do relógio; o helper retorna durante julgamento para evitar recursão (`app/ponto/signals.py:85-93`); chamado sem mudança de status retorna (`app/chamados/signals.py:191-193`). A porta avulsa recusa futuro/inexistente (`app/ponto/services/cartorio.py:1985-1989`) e, no dia corrente sem `teto_de_hoje`, espera fim de turno e exige conseguir calculá-lo (`:1993-1998`). A ocorrência de algum desses fatos para cada item da sonda é [nao verificado].

10. **Guarda semântica relevante:** o próprio comentário do ramo sem adesão diz “carimba e não emite” (`app/ponto/services/cartorio.py:1168-1172`), mas ele hoje lavra ata/veredito e carimba (`:1253-1277`), o que pode emitir outbox via `salvar_e_emitir` (`app/ponto/portas/celula.py:892-916`). Outro comentário diz que `nunca_bateu` mantém rejulgamento “para sempre” quando chegar a adesão (`app/ponto/services/cartorio.py:1243-1249`), mas a guarda efetiva do ramo sem adesão aceita qualquer veredito preenchido como em dia (`:1210-1216`). Essa diferença entre os dois ramos deve ficar explícita na resposta; não generalizar o comportamento de um para os 137 registros apenas pelo veredito.

## 10/11 00:19 · sessao bdaf292c04d9 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · sonda · origem=Code

O modo `--sonda` executa o envelope antes de entregar o dado raspado ao agente. Não fiz nova execução; abaixo reproduzo integralmente a saída fornecida, até `ERROS 0`, sem incluir o anexo `censo1.r`. (`bin/codex_pergunta.sh:307–319,337–344`) Autenticidade e completude da execução original: [nao verificado].

```text
sonda_leitura: sonda1.py [d18d7cf94f65bba4] no cpuset 4-7, lendo saas_hasner como leitor (LEITURA)

agora_local 2026-11-10T00:12:27.212274-03:00
janela 2026-10-21 2026-11-20 piso 2026-10-21
2026-11-10 00:12:29,540 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=266 data=2026-11-09 
2026-11-10 00:12:29,911 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=958 data=2026-11-09 
2026-11-10 00:12:30,492 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=994 data=2026-11-09 
2026-11-10 00:12:31,111 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=833 data=2026-11-09 
2026-11-10 00:12:31,717 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=887 data=2026-11-09 
2026-11-10 00:12:34,485 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=735 data=2026-11-09 
2026-11-10 00:12:35,077 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=89 data=2026-11-09 
2026-11-10 00:12:37,575 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=251 data=2026-11-09 
2026-11-10 00:12:37,731 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=238 data=2026-11-09 
2026-11-10 00:12:37,893 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=107 data=2026-11-09 
2026-11-10 00:12:37,935 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=49 data=2026-11-09 
2026-11-10 00:12:38,680 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=294 data=2026-11-09 
2026-11-10 00:12:39,339 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=556 data=2026-11-09 
2026-11-10 00:12:39,936 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=562 data=2026-11-09 
2026-11-10 00:12:40,954 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=364 data=2026-11-09 
2026-11-10 00:12:41,197 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=118 data=2026-11-09 
2026-11-10 00:12:41,840 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=945 data=2026-11-09 
2026-11-10 00:12:43,430 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=849 data=2026-11-09 
2026-11-10 00:12:43,503 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=301 data=2026-11-09 
2026-11-10 00:12:43,781 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=256 data=2026-11-09 
2026-11-10 00:12:45,555 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1000 data=2026-11-09 
2026-11-10 00:12:46,278 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=190 data=2026-11-09 
2026-11-10 00:12:46,571 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=139 data=2026-11-09 
2026-11-10 00:12:46,618 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=848 data=2026-11-09 
2026-11-10 00:12:46,712 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=575 data=2026-11-09 
2026-11-10 00:12:47,806 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=38 data=2026-11-09 
2026-11-10 00:12:49,023 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=814 data=2026-11-09 
2026-11-10 00:12:49,279 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=850 data=2026-11-09 
2026-11-10 00:12:50,961 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=631 data=2026-11-09 
2026-11-10 00:12:51,168 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=278 data=2026-11-09 
2026-11-10 00:12:51,334 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1001 data=2026-11-09 
2026-11-10 00:12:51,440 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=390 data=2026-11-09 
2026-11-10 00:12:51,553 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=259 data=2026-11-09 
2026-11-10 00:12:51,788 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1006 data=2026-11-09 
2026-11-10 00:12:51,873 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=577 data=2026-11-09 
2026-11-10 00:12:52,191 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=523 data=2026-11-09 
2026-11-10 00:12:53,084 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=493 data=2026-11-09 
2026-11-10 00:12:53,559 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=905 data=2026-11-09 
2026-11-10 00:12:53,766 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=587 data=2026-11-09 
2026-11-10 00:12:54,325 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=1012 data=2026-11-09 
2026-11-10 00:12:54,335 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-21 
2026-11-10 00:12:54,347 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-22 
2026-11-10 00:12:54,360 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-23 
2026-11-10 00:12:54,369 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-27 
2026-11-10 00:12:54,381 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-10-30 
2026-11-10 00:12:54,394 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-03 
2026-11-10 00:12:54,404 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-04 
2026-11-10 00:12:54,417 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-05 
2026-11-10 00:12:54,427 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-06 
2026-11-10 00:12:54,439 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=950 data=2026-11-09 
2026-11-10 00:12:54,451 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-22 
2026-11-10 00:12:54,460 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-24 
2026-11-10 00:12:54,472 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-26 
2026-11-10 00:12:54,484 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-28 
2026-11-10 00:12:54,494 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-10-30 
2026-11-10 00:12:54,506 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-01 
2026-11-10 00:12:54,518 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-03 
2026-11-10 00:12:54,528 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-05 
2026-11-10 00:12:54,544 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-07 
2026-11-10 00:12:54,555 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=969 data=2026-11-09 
2026-11-10 00:12:54,818 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=701 data=2026-11-09 
2026-11-10 00:12:55,130 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=258 data=2026-11-09 
2026-11-10 00:12:56,041 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=968 data=2026-11-09 
2026-11-10 00:12:56,172 WARNING chamados.fabrica sem_celula: ata nao lavrada, abstendo operacao=criar_questionario colaborador_id=726 data=2026-11-09 
N_ITENS 139 CELULAS 48
POR_MOTIVO {'saida_sem_entrada': 45, 'orfao_14h': 48, 'intervalo_saida': 23, 'intervalo_volta': 23}
POR_DIA {'2026-10-21': 1, '2026-10-26': 1, '2026-11-04': 2, '2026-11-05': 35, '2026-11-06': 18, '2026-11-07': 38, '2026-11-08': 16, '2026-11-09': 28}
POR_EMPRESA {2: 134, 3: 5}
A 0 B 0 sem_evento_pos_dia 135 sem_evento_nenhum 0 com_evento_pendente 0
COMBO(A,B,ev_pos>0) {(False, False, False): 135, (False, False, True): 4}
TRABALHA {'True': 139} VEREDITO {'nunca_bateu': 137, 'indefinida': 1, 'cobrado': 1}
PERG_MESMO_DIA_QUALQUER>0 0
ITENS (colab,emp,dia,motivo,trab,A,B,n_ev,n_ev_pos,pend,perg_dia,julgada,ult_ev):
 col1009 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 07:30:07 ultev=11-07 07:30:25
 col1009 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 07:30:07 ultev=11-07 07:30:25
 col1009 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1009 e2 2026-11-09 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1009 e2 2026-11-09 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1009 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 07:30:07 ultev=11-09 07:30:31
 col1005 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:24
 col1005 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:23
 col1005 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-07 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-07 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 09:00:08 ultev=11-07 09:00:23
 col1005 e2 2026-11-08 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-08 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-08 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-08 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 09:00:08 ultev=11-08 09:00:25
 col1005 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1005 e2 2026-11-09 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1005 e2 2026-11-09 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1005 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 09:00:13 ultev=11-09 09:00:34
 col1030 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 15:00:07 ultev=11-09 15:00:24
 col1030 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 15:00:07 ultev=11-09 15:00:24
 col427 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 19:30:09 ultev=11-05 19:30:21
 col427 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col427 e2 2026-11-07 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col427 e2 2026-11-07 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col427 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 19:30:07 ultev=11-07 19:30:22
 col1027 e2 2026-11-04 saida_sem_entrada trab=True A=False B=False ev=2 pos=2 pend=0 perg_dia=0 julg=11-05 16:03:54 ultev=11-05 16:03:54
 col1027 e2 2026-11-04 orfao_14h trab=True A=False B=False ev=2 pos=2 pend=0 perg_dia=0 julg=11-05 16:03:54 ultev=11-05 16:03:54
 col1027 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 06:30:07 ultev=11-06 06:30:17
 col1027 e2 2026-11-06 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 06:30:07 ultev=11-06 06:30:17
 col1027 e2 2026-11-08 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 06:30:07 ultev=11-08 06:30:17
 col1027 e2 2026-11-08 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-08 06:30:07 ultev=11-08 06:30:17
 col268 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 08:00:10 ultev=11-05 08:00:12
 col268 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-07 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-07 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 08:00:07 ultev=11-07 08:00:09
 col268 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col268 e2 2026-11-09 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col268 e2 2026-11-09 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col268 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 08:00:08 ultev=11-09 08:00:12
 col1019 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 06:30:09 ultev=11-05 06:30:21
 col1019 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 06:30:08 ultev=11-07 06:30:17
 col1019 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 06:30:08 ultev=11-07 06:30:17
 col1019 e2 2026-11-09 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 06:30:08 ultev=11-09 06:30:19
 col1019 e2 2026-11-09 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-09 06:30:08 ultev=11-09 06:30:19
 col1016 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 18:30:09 ultev=11-05 18:30:28
 col1016 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 18:30:09 ultev=11-05 18:30:28
 col1016 e2 2026-11-07 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 18:30:08 ultev=11-07 18:30:29
 col1016 e2 2026-11-07 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-07 18:30:08 ultev=11-07 18:30:29
 col534 e2 2026-11-05 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-05 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-05 intervalo_volta trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-05 orfao_14h trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-05 09:00:09 ultev=11-05 09:00:14
 col534 e2 2026-11-06 saida_sem_entrada trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=11-06 09:00:10
 col534 e2 2026-11-06 intervalo_saida trab=True A=False B=False ev=2 pos=0 pend=0 perg_dia=0 julg=11-06 09:00:05 ultev=

[truncado em 20000 de 34482 caracteres]

## 10/11 00:18 · sessao b9ec0c8ed08e · - · - · origem=Ronald (tmux)

Censo na base `7b0dbff79`: 31 chamadas externas de `pautas.services.escrever`, mais `responder` interno (`pautas/services.py:194`); criação única em `pautas/services.py:180`. Portas humanas: cabeça em `pautas/views.py:76`, resposta em `pautas/views.py:113` → `pautas/services.py:194`.

Chamadores completos, por arquivo:

| Arquivo | Linhas de chamada |
|---|---|
| `chamados/management/commands/reabrir_orfas_enterradas.py` | 208 |
| `chamados/services/competencia_trancada.py` | 231 |
| `chamados/services/fabrica_da_celula.py` | 270, 360 |
| `chamados/services/passivo_validado.py` | 229, 233, 266 |
| `chamados/services/resposta_tardia.py` | 62 |
| `colaboradores/management/commands/pauta_aparelhos_compartilhados.py` | 107 |
| `core/management/commands/alarme_arvore.py` | 50 |
| `core/management/commands/alarme_esteira.py` | 54 |
| `core/services/crons_quebrados.py` | 71 |
| `ferias/management/commands/pautas_de_ferias.py` | 167 |
| `folha/management/commands/pre_fechamento.py` | 110 |
| `pautas/management/commands/pauta_dia_em_aberto.py` | 127, 139, 142 |
| `pautas/management/commands/pauta_folga_nao_definida_exportada.py` | 134, 139 |
| `pautas/management/commands/pauta_pago_x_devido_exportada.py` | 207, 211, alias `_ps.escrever` |
| `pautas/semanal.py` | 276 |
| `pautas/views.py` | 76, alias `sv.escrever` |
| `ponto/services/afastado_avisa.py` | 74 |
| `ponto/services/ausencia.py` | 1055, 1245 |
| `ponto/services/autorizacao_he_periodo.py` | 244 |
| `ponto/services/cobertura.py` | 333 |
| `ponto/services/pautas_do_esmeril.py` | 76 |
| `ponto/services/pautas_dos_deslogados.py` | 77 |
| `ponto/services/pautas_dos_discordantes.py` | 177 |

Além das cabeças, escrevem filhas: `pautas/services.py:194`, `chamados/services/passivo_validado.py:233`, `pautas/management/commands/pauta_dia_em_aberto.py:127,142`, `pautas/management/commands/pauta_folga_nao_definida_exportada.py:139`, `pautas/management/commands/pauta_pago_x_devido_exportada.py:211`, `ponto/services/autorizacao_he_periodo.py:244-246`.

AST encontra **26** keywords `texto__startswith`, todos sobre Pauta, em produção. Tabela de famílias (linhas da base):

| Marcador / dono | Escritor | Leitor | Cabeças somente? |
|---|---|---|---|
| `PAUTA_LIMBO_EXPORTADA`, `chamados/services/fabrica_da_celula.py:239` | mesmo:270,275 | mesmo:254 | Sim, `pai__isnull=True` em 253 |
| `PAUTA_SEM_TURNO`, mesmo:296 | mesmo:360,363 | mesmo:305 | Sim, 304 |
| literal `[TRANCA]`, `chamados/services/competencia_trancada.py:218,223` | mesmo:231 | mesmo:218 | **Não** |
| `MARCADOR` `[ORFA-ENTERRADA]`, `chamados/management/commands/reabrir_orfas_enterradas.py:32` | mesmo:208 | mesmo:202 | **Não** |
| `MARCADOR` `[PASSIVO-VALIDADO]`, `chamados/services/passivo_validado.py:44` | mesmo:229,233 | mesmo:164, via `cabeca` em 159 | Sim, 164 |
| `MARCADOR_FORA` `[PASSIVO-VALIDADO-FORA]`, mesmo:48 | mesmo:266 | mesmo:256, via `cabeca` em 254 | Sim, 256 |
| `MARCADOR` `ARVORE VERMELHA`, `core/management/commands/alarme_arvore.py:11` | mesmo:50,53 | mesmo:16 | **Não** |
| `MARCADOR` `ESTEIRA`, `core/management/commands/alarme_esteira.py:10` | mesmo:54,56 | mesmo:15, compõe motivo `chave` | **Não** |
| `MARCADOR` `[CRON QUEBRADO]`, `core/services/crons_quebrados.py:15` | mesmo:71 | mesmo:54, compõe cron/empresa | **Não** |
| `MARCADORES`, três valores `[FERIAS/PA-VENCIDO]`, `[FERIAS/PA-VENCENDO]`, `[FERIAS/CADASTRO]`, `ferias/management/commands/pautas_de_ferias.py:30-34` | mesmo:167, usa 149,159 | mesmo:156 | **Não** |
| `MARCADOR` `[PRE-FECHAMENTO]`, `folha/management/commands/pre_fechamento.py:29` | mesmo:110 | mesmo:92,98, segundo usa data `de_hoje` em 93 | **Não**, ambos |
| `MARCA` `[DIA EM ABERTO]`, `pautas/management/commands/pauta_dia_em_aberto.py:16` | mesmo:139; filhas 127,142 | mesmo:108 | **Não** |
| `MARCA` `[FOLGA NAO DEFINIDA - EXPORTADA]`, `pautas/management/commands/pauta_folga_nao_definida_exportada.py:28` | mesmo:134; filhas 139 | mesmo:126 | Sim, 125 |
| `MARCA` `[PAGO X DEVIDO - EXPORTADA]`, `pautas/management/commands/pauta_pago_x_devido_exportada.py:41` | mesmo:207; filhas 211 | mesmo:243,250, chave nasce em 54 | Cabeça: sim em 242; 250 lê `ja.respostas` **intencionalmente** |
| `[A6/REVISAO-SEMANAL]`, `ponto/services/vigia_ausencia.py:28` | chama semanal em 239 → `pautas/semanal.py:276` | `pautas/semanal.py:199,228` | **Não**, ambos |
| `[SUP/REVISAO-SEMANAL]`, `escala/services/fase_12x36.py:237` | chama semanal em 277-278 → `pautas/semanal.py:276` | `pautas/semanal.py:199,228` | **Não**, ambos |
| `TEXTO[:40]`, `ponto/services/afastado_avisa.py:11` | mesmo:74,78 | mesmo:65 | Sim, 63 |
| local `marcador` `[A4/SEM-DOCUMENTO]`, `ponto/services/ausencia.py:1038` | mesmo:1055,1060 | mesmo:1041 | **Não** |
| local dinâmico `marcador` `[A1-FERIAS/AG#%s]`, mesmo:1228 | mesmo:1245,1250 | mesmo:1231 | **Não** |
| `MARCADOR_PAUTA` `[HE-AUTORIZADA-EM-EXPORTADA]`, `ponto/services/autorizacao_he_periodo.py:124` | mesmo:244 | mesmo:242 | Sim |
| `MARCADOR_A_CONFIRMAR` `[COBERTURA-A-CONFIRMAR]`, `ponto/services/cobertura.py:207` | mesmo:333,337 | mesmo:243 | **Não** |
| `MARCADOR` `[ESMERIL %s]`, `ponto/services/pautas_do_esmeril.py:14` | mesmo:76, usa 49 | mesmo:64, literal `[ESMERIL ` | **Não** |
| `MARCADOR` `[APP-401]`, `ponto/services/pautas_dos_deslogados.py:26` | mesmo:77, usa 48 | mesmo:66 | **Não** |
| `MARCADOR` `[DISCORDANTE]`, `ponto/services/pautas_dos_discordantes.py:37` | mesmo:177, usa 93 | mesmo:166 | **Não** |

`vivas()` e `nao_canceladas()` **não** filtram pai (`pautas/models.py:57-64`). Portanto não há exclusão implícita de respostas nos leitores assinalados “Não”.

Dois escritores do censo não têm leitor `texto__startswith`: `[RESPOSTA-TARDIA]` em `chamados/services/resposta_tardia.py:65` e `APARELHO COMPARTILHADO` em `colaboradores/management/commands/pauta_aparelhos_compartilhados.py:99`.

Semânticas que precisam sobreviver:

- `PAGO` usa marcador + colab + competência (`pautas/management/commands/pauta_pago_x_devido_exportada.py:54`); a cabeça exige espaço depois da chave em 243; o leitor de filhas exige ` > ` em 250. Ainda há leitor Python `p.texto.startswith(MARCA)` na reversão em 277.
- `PASSIVO` e `PASSIVO_FORA` são famílias distintas; o comentário explica que misturá-las falseia idempotência (`chamados/services/passivo_validado.py:245-248`).
- `ESTEIRA` distingue motivo por `'%s %s:'` (`core/management/commands/alarme_esteira.py:15`).
- `CRON` distingue nome/empresa (`core/services/crons_quebrados.py:54`).
- `ESMERIL` usa prefixo aberto `[ESMERIL ` e depois faz parsing da ação por posição (`ponto/services/pautas_do_esmeril.py:64-65`), enquanto a constante é template `[ESMERIL %s]` em 14.
- `DISCORDANTE` tem leitor Python adicional em `ponto/services/pautas_dos_discordantes.py:98`, seguido de parsing em 100-101.
- `afastado` reserva só os primeiros 40 caracteres de uma frase normal, não uma tag (`ponto/services/afastado_avisa.py:65`).
- `A1-FERIAS` é família dinâmica por agendamento (`ponto/services/ausencia.py:1228`).
- `escrever` remove espaços externos antes de persistir (`pautas/services.py:164`); prefixo com espaço inicial humano acaba normalizado na base. Marcador no meio não casa com `startswith` por definição dos lookups existentes.
- Os leitores usam `startswith`, não `istartswith` (26 sítios enumerados); o comportamento efetivo de collation do banco está `[não verificado]`. A normalização de caixa exigida para recusa pode ser aplicada só à guarda humana, conservando leitores. [nao verificado]
- Responder cria filha, mantém âncora e inverte destinatário para usuário autor ou departamento (`pautas/services.py:192-201`). Isso pode afastar respostas humanas dos leitores que exigem departamento, mas não equivale a filtro de pai. Sem alterar leitores sem filtro, a afirmação “toda resposta está fora do juiz” seria falsa.

Legado: é possível enumerar **candidatos** por prefixo + pai nulo, mas usuário não prova origem: `autor_sistema` recua a superuser (`pautas/services.py:89-93`); `Pauta.autor_user` só grava usuário (`pautas/models.py:102-106`). A trilha de criação grava remetente/remetente informado, sem canal humano/sistema (`pautas/services.py:186-188`, `_trilha` em 369-373). `LogAuditoria` oferece IP/ator (`core/models.py:302-307`), que poderia ajudar numa investigação autorizada, sem garantir classificação completa. Quantidade e identidade dos casos em produção: `[não verificado]`.

Comandos usados para o censo:

```bash
git grep -n -E 'pautas.services|texto__startswith|escrever\(' -- '*.py' ':!*/tests/*'
```

Varredura AST somente leitura executada com `PYTHONDONTWRITEBYTECODE=1 python3`, sobre `git ls-files '*.py'`, excluindo testes/migrations; enumerou os 26 keywords e validou os imports/chamadas diretos mais aliases. Nenhum arquivo alterado. [nao verificado]

## 10/11 00:14 · sessao d84e0a927d5d · - · - · origem=Ronald (tmux)

Para os **4 itens residuais** do dado — col1027/2026-11-04/`saida_sem_entrada`, col1027/2026-11-04/`orfao_14h`, col155/2026-10-26/`orfao_14h`, col982/2026-10-21/`orfao_14h` — a contagem é **4 itens, 3 células**; identificação proveniente somente do texto fornecido. [nao verificado]

Sugiro três feições separadoras, sem escolher cura:

1. **Estado e instante do consumo:** `processado_em`, `tentativas`, `erro`, comparados com o vencimento real do marco. `pend=0` não distingue processado de morto: a fila viva exige `processado_em IS NULL` e `tentativas<5`; falhas incrementam tentativas, enquanto processamento sem falha nem adiamento grava `processado_em`. O vencimento é marco do turno +30 minutos, não simplesmente mudança do dia civil. (`app/escala/models.py:1857–1863`; `app/chamados/reconciliador.py:1209,1246–1255,1392–1401`; `app/ponto/services/cartorio.py:828,831–859`.) Os valores necessários não aparecem no dado. [nao verificado]

2. **Elegibilidade no instante da execução:** motivos selecionados pela fábrica no consumo/julgamento, associados à ata, ao veredito, à situação do colaborador e ao teto daquele instante. O consumidor relê a célula atual da data, sem payload histórico; a seleção pode devolver vazio pelas guardas e o evento pode ser marcado processado. Isso separaria “houve execução quando ainda não devido/elegível” de “houve seleção devida que não virou pergunta”. (`app/escala/models.py:1848–1856`; `app/chamados/services/fabrica_da_celula.py:58–66,105–118,198,208–211,288–292`; `app/chamados/reconciliador.py:1387–1401`.) O estado histórico e a versão executada não estão demonstrados. [nao verificado]

3. **Resultado após a seleção:** data efetivamente deduplicada, identidade da linha bloqueadora e resultado/recusa da porta `PerguntaDisputa.abrir`. O pré-dedup usa disputa/motivo/data, enquanto a porta pode recusar destino incompatível ou violação do contrato de saída; a fábrica devolve motivos selecionados e ignora a lista efetivamente criada. Isso separaria “selecionou mas bloqueou/recusou” de “nem chegou à seleção devida”. (`app/chamados/services/disputa_emissao.py:395–424,444–452`; `app/chamados/models.py:2818–2822,2839–2840,2888–2890`; `app/chamados/services/fabrica_da_celula.py:165–167,208–211`.) Nenhuma dessas ocorrências individuais foi fornecida. [nao verificado]

Dois cuidados adicionais:

- O pulo por impressão igual **exclui explicitamente `nunca_bateu`**; portanto não se pode usar esse mecanismo genericamente para explicar o grupo que contém 137 itens com esse veredito agregado. (`app/ponto/services/cartorio.py:1369–1375`.) A associação do veredito a cada linha não foi dada. [nao verificado]
- `criar_ou_atualizar_disputa` só retorna `None` normalmente quando `motivos` está vazio; a fábrica passa `[motivo]`. Disputa existente e motivo repetido continuam chamando o gerador. Esse retorno não constitui explicação normal dos quatro residuais. (`app/chamados/services/disputa_emissao.py:656–682`; `app/chamados/services/fabrica_da_celula.py:161–167`.)

## 10/11 00:13 · sessao ee39037e9fe3 · - · - · origem=Ronald (tmux)

- **A limbo/rótulo:** `limbo_vai_a_pauta` só responde True quando há motivos, `cel.trabalha is None` e competência entregue; o escritor encaminha pauta e retorna sem perguntar. O vigia conta motivos sem pauta nesse ramo. (`app/chamados/services/fabrica_da_celula.py:205–207,214–222,233–236,253–256`)
- Todos os 139 itens fornecidos têm `trab=True`, portanto não satisfazem o ramo de limbo nessa fotografia; referência da fotografia em arquivo ausente: **[nao verificado]**. Isso não mede a outra variante de A, o retrato anterior à escrita da própria rodada. (`app/chamados/services/fabrica_da_celula.py:219–220`; `app/chamados/management/commands/disparar_perguntas_competencia.py:108–113,289–303`)
- **B NULL antes do fallback:** sem destino único, o derivador devolve `(None,None)`; para os motivos que exigem destino, o gerador procura pergunta usando `_data_ev`, e pode dar `continue` antes de `_dia_p` usar `_data_fallback`. (`app/chamados/services/disputa_emissao.py:238–244,410–424`)
- Os intervalos usam `_data_fallback` diretamente no pré-dedup; o mecanismo B específico de NULL não se transfere a eles. (`app/chamados/services/disputa_emissao.py:26,395–400`)
- **B=False não distingue a variante de identidades divergentes:** o pré-dedup filtra disputa/motivo/dia/conversa/vias, enquanto `q_dos_fatos` também exige coluna colaborador e instância do motivo. Comparar essas colunas é uma feição adicional possível para os residuais. (`app/chamados/services/disputa_emissao.py:395–420`; `app/chamados/models.py:2753–2755`)
- A inserção normal atual deriva/valida colaborador e instância. Estoque incoerente nessa amostra: **[nao verificado]**. (`app/chamados/models.py:3425–3447`)
- Outras feições que poderiam separar residuais: catálogo encontrado; texto/data obtidos; destino aceito; contrato de saída cumprido. Há recusas correspondentes no código, mas ocorrência individual no raspado é **[nao verificado]**. (`app/chamados/services/disputa_emissao.py:314–316,344–366,424–430`; `app/chamados/models.py:2818–2822,2839–2840,2888–2890`)
- Não localizei `AGENTS.md`, `sonda1.py` ou `censo1.r` nos nomes pesquisados da cópia; conclusão sobre anexos fora da cópia: **[nao verificado]**.

## 10/11 00:09 · sessao 1522924e199b · REVERTER-DIA-SEM-RECALCULO · construtor · origem=Code

A cura está aplicada: a VOLTA chama uma vez `_o_chao_do_dia_mudou`, depois do `atomic`, com o motivo solicitado. Guardas, escrita, evento e trilha conservaram AST idêntico ao da base. [ponto/portas/celula.py:774](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/portas/celula.py:774), [ponto/portas/celula.py:809](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/portas/celula.py:809). [nao verificado]

Arquivos tocados:

- [ponto/portas/celula.py:809](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/portas/celula.py:809): substituição do julgamento paralelo pelo rabo comum.
- [ponto/tests/test_reverter_regen_recalcula_dinheiro.py:18](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:18): contrato AST, contraexemplo da base guardado e teste ORM 240 → 480 → 240, com `on_commit` imediato. [nao verificado]
- [ponto/tests/test_reverter_regen_sem_banco.py:28](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_reverter_regen_sem_banco.py:28): execução instrumentada dos corpos reais e mutações somente em memória.

Não ajustei mocks vizinhos: o alvo continua sendo `ponto.services.cartorio.julgar_celula`; a execução deles por Django ficou pendente. [ponto/tests/test_porta_celula_dia.py:263](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_porta_celula_dia.py:263).

**RED antes e resultado depois.** Antes da cura, o verificador AST falhou com `VOLTA deve chamar o rabo comum uma vez`; o caso financeiro **instrumentado** falhou com `480 != 240`. Depois, o comando abaixo terminou com `Ran 22 tests … OK`. O teste com `DiaPago` real não foi executado: faltam Django e banco; seu RED/verde ORM permanece **[não verificado]**. [verificador:24](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:24), [caso instrumentado:186](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_reverter_regen_sem_banco.py:186), [caso ORM:128](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_reverter_regen_recalcula_dinheiro.py:128). [nao verificado]

```bash
export PYTHONDONTWRITEBYTECODE=1
python3 -m unittest ponto.tests.test_reverter_regen_sem_banco chamados.tests.test_e4_expand -v
```

Os nove mutantes abaixo terminaram com `rc=1`, por asserções, sem erros de infraestrutura. Cobrem base antiga, omissão/duplicação do rabo, escritores paralelos, posição dentro do `atomic`, desligamento do recálculo, dias vazios e perda do motivo. [ponto/tests/test_reverter_regen_sem_banco.py:28](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/tests/test_reverter_regen_sem_banco.py:28).

```bash
export PYTHONDONTWRITEBYTECODE=1
selo_volta=ponto.tests.test_reverter_regen_sem_banco

REVERTER_MUTANTE=base python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=sem_rabo python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=juiz_paralelo python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=dinheiro_paralelo python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=rabo_no_atomic python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=sem_dinheiro python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=dias_vazios python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=motivo_vazio python3 -m unittest "$selo_volta"
REVERTER_MUTANTE=duas_vezes python3 -m unittest "$selo_volta"
```

**Escritores e leitores.** O alcance financeiro é a competência do colaborador, não somente o dia revertido: `recalcular_por_evento` agenda o fechamento mensal em `on_commit`. [ponto/services/fechamento.py:1809](/home/ronald/wt-codex-reverter-dia-sem-recalculo-235559/app/ponto/services/fechamento.py:1809).

| Campos alcançados | Quem escreve | Quem lê |
|---|---|---|
| `DiaPago`: os 15 `CAMPOS_DIA`, incluindo previstos, realizados e abonados; ajuste de DSR e banco | Inventário em `ponto/services/dia_pago.py:34`; `lavrar` substitui as linhas em `:359` | Todos são agregados em `ponto/services/dia_pago.py:562`; relatório em `relatorios/views.py:487`; espelho/PDF em `ponto/services/espelho.py:406` |
| `DiaPago`: chaves, versão, `versao_motor`, `versao_dna`, `evento_origem`, `detalhe`, `furo_apurado` | `ponto/services/dia_pago.py:335`, `:345` | Chaves/versão em `:855`; detalhe em `:897`; furo em `:589`; régua em `folha/porta_export.py:285`. Leitores de `versao_dna` e `evento_origem`: **[não verificado]** |
| `FechamentoMensal`: horas trabalhadas, noturnas, extras e subdivisões, folga, atraso, saída, falta e intra | Update canônico em `ponto/services/fechamento.py:1160` | Comparação campo a campo em `folha/porta_export.py:451`; rubricas TXT em `folha/export.py:420`; totais em `relatorios/cartao_pela_celula.py:94` |
| Mensal: previstos/realizados/abonados, dias previstos, turnos abertos, inconsistências e banco | `ponto/services/fechamento.py:1175` | `relatorios/cartao_pela_celula.py:111`, `folha/export.py:1037`, `:1068` |
| Mensal: semanas DSR, reflexo DSR, dias incertos | `ponto/services/fechamento.py:1177` | Reflexo segue para a lavratura em `:1207`; leitores individuais das semanas e dias incertos: **[não verificado]** |
| Mensal: motivos, causa e carimbos | `ponto/services/fechamento.py:1186` | `folha/export.py:964`, `folha/retrato_folha.py:1115`, `folha/export.py:1037`, `ponto/services/fechamento.py:1696`, `ponto/views.py:1740` |
| Chamado: `celula` e `atualizado_em`, pela rejunção | `chamados/rejuncao.py:119` → `chamados/services/vinculos.py:93` | Dia do fato em `chamados/juizes.py:98`; marcador do painel em `chamados/services/painel.py:64` |
| Falta espúria: status, resolução, via, urgência e carimbos; nota e trilha | `chamados/services/feriado_reavaliar.py:46` → `chamados/models.py:641`, `:228`; nota em `chamados/services/nota.py:103` | Receivers em `chamados/signals.py:91`, `:188`; painel em `chamados/services/painel.py:64` |
| Perguntas, condicionalmente: materialização/autor/via, resposta/validação quando cabíveis, veredito e carimbos; possível fechamento da disputa | `chamados/services/disputa_emissao.py:1860`, `chamados/models.py:3128`, `:3149`, `:1936` | Juiz em `chamados/juizes.py:662`, `:830`; frescor em `chamados/veredito_lavrado.py:228`; painel em `chamados/services/painel.py:66` |

**ESMERIL-SEMÂNTICA:** não encontrei, nos leitores e chamador examinados, condição executável exigindo que a VOLTA deixe de recalcular; ausência absoluta **[não verificado]**. Há comentários que ainda descrevem a VOLTA como lacuna da reavaliação, em `ponto/management/commands/reavaliar_ausencias_feriado.py:15` e `chamados/signals.py:319`. O chamador operacional delega à porta em `ponto/management/commands/aplicar_col900.py:77`.

Os contraexemplos limitam a promessa do golden:

- **Futuro:** pula julgamento, mas agenda dinheiro e reavaliação; não garante ata restaurada para 240. `ponto/portas/celula.py:427`, `:458`; selo em `ponto/tests/test_reverter_regen_sem_banco.py:204`.
- **Veredito humano:** preserva julgamento/ata humanos e agenda dinheiro; o selo conserva ata 480. `ponto/tests/test_reverter_regen_sem_banco.py:213`.
- **Competência guardada:** 09/2026 exportada continua excluída da escrita financeira. `ponto/services/fechamento.py:36`, `:560`.
- **`apesar_da_lavra`:** abre a guarda da célula, sem desligar a guarda financeira. `ponto/portas/celula.py:779`; `ponto/services/fechamento.py:560`.
- **`aplicar_col900 --reverter`:** sem `--aplicar`, apenas simula; aplicando, usa a porta corrigida. “REVERTIDO” indica sucesso da reposição, sem certificar persistência financeira posterior. `ponto/management/commands/aplicar_col900.py:74`, `:77`; falhas financeiras são capturadas em `ponto/services/fechamento.py:1818`.

**Pendências de execução:** o comando completo abaixo parou com `ModuleNotFoundError: No module named 'django'`. Ficaram sem executar o golden ORM, o `SimpleTestCase` pelo runner Django, `test_porta_celula_dia` e `test_ordem_regen_julga_antes_do_dinheiro`. O mesmo verificador AST rodou pelo selo sem banco. Certificação pelo Code/opus: **[não verificado]**. [nao verificado]

```bash
PYTHONDONTWRITEBYTECODE=1 python3 manage.py test \
  ponto.tests.test_reverter_regen_recalcula_dinheiro \
  ponto.tests.test_reverter_regen_sem_banco \
  ponto.tests.test_porta_celula_dia \
  ponto.tests.test_ordem_regen_julga_antes_do_dinheiro \
  chamados.tests.test_e4_expand \
  --settings=config.settings.ci --verbosity=2
```

Ruff retornou `All checks passed!` nos três arquivos tocados, com `--no-cache`; `git diff --check` também passou. O brief externo não foi localizado nos dois caminhos tentados; conteúdo além do contrato transcrito **[não verificado]**. [nao verificado]

## 10/11 00:03 · sessao c850285aa1d5 · FIO-E0-V4-VIGIA-FABRICA-POR-EVENTO-CRESCE · censo · origem=Code

**O N é um retrato do estoque elegível da janela, recalculado a cada rodada. A mesma pendência pode aparecer ontem e hoje; o crescimento não identifica, sozinho, falha do contador, silêncio do emissor ou falta de gatilho.** (`app/chamados/management/commands/disparar_perguntas_competencia.py:101–113,240–242`)

A amostra fornecida descreve 20 motivos, não os 111; o alarme imprime somente os primeiros 20 alvos. A composição dos outros 91 e a ligação do warning do col969 com os 111 permanecem **[não verificado]**. (`app/chamados/management/commands/disparar_perguntas_competencia.py:247–250`)

**1. Universo, cálculo e significado de “emitida”**

- **Janela:** `--comp-corrente` chama `janela_atual(localdate())` **sem empresa**. Portanto usa o corte padrão 21; para uma rodada com data local 09/11/2026, a janela calculada é 21/10–20/11. O piso do vigia é `min(início, hoje−2 dias)`, preservando a borda da competência anterior. (`app/chamados/management/commands/disparar_perguntas_competencia.py:66–70,100–102`; `app/ponto/janelas.py:24–33,50–63`)
- **Células:** todas as `CelulaDia` existentes entre esse piso e o fim, com `data < hoje`; opcionalmente restringidas por `--colab`. A consulta não filtra empresa, `trabalha` ou situação do colaborador; as guardas posteriores decidem quais motivos restam. Célula inexistente não entra nesse universo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:102–112`)
- **Ontem:** fica fora enquanto `_ontem_em_janela` considerar o turno responsabilidade do vigia de hora. A decisão usa fim do turno, teto e `dia_encerrado`, incluindo o período de turno aberto. (`app/chamados/management/commands/disparar_perguntas_competencia.py:105–111`; `app/ponto/services/vigia_de_hora.py:26–58`)
- **N:** soma um item por motivo devolvido por `motivos_que_o_evento_deixou`; **C:** quantidade distinta de pares `(colaborador, data)` dessa lista. `--limite` limita o fio mudo, não esse N. (`app/chamados/management/commands/disparar_perguntas_competencia.py:112–113,235–242`)
- **“Deixou”:** significa “a leitura atual ainda devolve este motivo devido”, e não “foi constatada uma tentativa anterior fracassada”. A função consulta motivos atuais e, no limbo entregue, pautas existentes; não consulta histórico de tentativas ou `EventoCelula`. (`app/chamados/services/fabrica_da_celula.py:225–236`)
- **“Emitida”, para o dedup:** existe pergunta do fato — motivo, dia do turno, instância do marco, colaborador, `conversa=False` — fora das vias `duplicata` e `motivo_retratado`. Não exige pergunta viva, pendente, entregue por push ou ainda sem decisão; pergunta decidida também pode segurar a identidade. A consulta da fábrica ainda restringe pelo colaborador da disputa. (`app/chamados/services/fabrica_da_celula.py:138–150`; `app/chamados/models.py:2393,2731–2755`)

Assim, **é estoque da janela por rodada, não incidência diária nem soma dos logs anteriores**. Uma pendência volta a contar enquanto continuar no recorte e passar pelas guardas; pode deixar de contar por pergunta existente, pauta, mudança de elegibilidade ou saída da janela. (`app/chamados/management/commands/disparar_perguntas_competencia.py:101–113`; `app/chamados/services/fabrica_da_celula.py:105–150,233–236`)

Há uma ressalva de ordem: `deixou` é calculado **antes** da escrita do fio mudo e não é recalculado depois. A própria rodada pode criar a pergunta pelo fio mudo e ainda imprimir/alarmar o N anterior; existe teste exigindo esse comportamento. (`app/chamados/management/commands/disparar_perguntas_competencia.py:108–113,240–242,289–303`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177–184`)

**2. Quem emite e quais guardas podem calar**

Há dois gatilhos para a mesma fábrica: o cartório chama `perguntar_pela_celula` após o julgamento, com `apply_`, somente para datas presentes no `cods` **daquele ato**; o consumidor chama `perguntar_por_eventos`, que relê as células atuais das datas dos eventos do lote. O consumidor não rejulga essas células nessa função. (`app/ponto/services/cartorio.py:938–958,992–1011`; `app/chamados/reconciliador.py:1384–1387`; `app/chamados/services/fabrica_da_celula.py:279–293`)

Os quatro motivos usam a mesma seleção de lâmpadas; diferem pelo marco e por parte do caminho de deduplicação posterior. (`app/chamados/services/declaracao_texto.py:42–57`; `app/chamados/services/disputa_emissao.py:395–420`)

| Motivo | Marco que o produz | Particularidade posterior |
|---|---|---|
| `saida_sem_entrada` | início, tipo E | Deriva chamado-destino; entra no dedup por `_data_ev`. (`app/chamados/services/declaracao_texto.py:43`; `app/chamados/models.py:3657`; `app/chamados/services/disputa_emissao.py:410–420`) |
| `orfao_14h` | fim, tipo S | Mesmo ramo da entrada. Neste caminho, não há teste próprio de “14 horas”: o motivo vem da lâmpada faltante. (`app/chamados/services/declaracao_texto.py:44`; `app/chamados/services/fabrica_da_celula.py:68–73`; `app/chamados/services/disputa_emissao.py:410–420`) |
| `intervalo_saida` | ida ao intervalo, tipo S | Dedup datado por `_data_fallback`; destino não é requisito desse ramo. (`app/chamados/services/declaracao_texto.py:45`; `app/chamados/services/disputa_emissao.py:26,395–409`) |
| `intervalo_volta` | volta do intervalo, tipo E | Mesmo ramo datado do intervalo de saída. (`app/chamados/services/declaracao_texto.py:46`; `app/chamados/services/disputa_emissao.py:26,395–409`) |

As guardas da seleção são estas:

| Guarda/abstenção | Emissor e vigia |
|---|---|
| **Dia sem trabalho** | Ambos leem `dia_sem_trabalho`: `trabalha=False`, ou tipo da ata preenchido diferente de trabalho. Ausência parcial permite cobrar o resto, salvo `trabalha=False`. (`app/chamados/services/fabrica_da_celula.py:103–106`; `app/chamados/juizes.py:426–441`) |
| **Colaborador** | Ambos exigem situação atual exatamente `ativo`; não é consulta histórica da situação naquele dia. (`app/chamados/services/fabrica_da_celula.py:102–106`) |
| **Futuro e teto** | Ambos excluem futuro e exigem marco vencido por instante: marco do turno +30 minutos. (`app/chamados/services/fabrica_da_celula.py:35–49,107–112`; `app/ponto/services/cartorio.py:828–859`) |
| **Passado sem turno previsto** | Emissor escreve pauta de CADASTRO e retorna. Vigia obtém zero motivos, **sem conferir se essa pauta existe**; esse N não mede tal falta de pauta. (`app/chamados/services/fabrica_da_celula.py:47–61,192–196,233–236`) |
| **Veredito** | Veredito preenchido fora de `VEREDITOS_ACUSAM` cala ambos; vazio não é barrado por esse teste. (`app/chamados/services/fabrica_da_celula.py:113–114`; `app/escala/models.py:1767–1768`) |
| **Parcial na lista do admin** | Ambos chamam `parcial_so_lista`: sem período declarado cala sempre; com período, depende de julgamento anterior e fato novo. **O carimbo passado pelo cartório e o lido pelo vigia podem ser diferentes**, detalhado abaixo. (`app/chamados/services/fabrica_da_celula.py:115–118`; `app/ponto/services/cartorio.py:395–400,954,1008`) |
| **Ata/lâmpadas desconhecidas** | Ambos dependem de `_ata_do_dia`; sem autorização ou com `faltantes=None`, devolvem zero motivos. (`app/chamados/services/fabrica_da_celula.py:64–66`; `app/chamados/services/disputa_emissao.py:884–910`) |
| **Lâmpada acesa, dispensada ou “não sei”** | O coletor só aproveita `missing`; o leitor trata `acesa=None` como não faltante. Isento e fora do vínculo também não produzem os faltantes cobrados. (`app/chamados/services/declaracao_texto.py:103–139`; `app/escala/services/leitor_celula.py:119–145`) |
| **Pergunta já existente/decidida** | Ambos retiram os motivos já ocupados no domínio do fato; não é dedup apenas de perguntas vivas. (`app/chamados/services/fabrica_da_celula.py:138–150`; `app/chamados/models.py:2746–2755`) |
| **Limbo em competência entregue** | Emissor encaminha pauta DP e não pergunta ao colaborador. Vigia conta os motivos ainda sem essa pauta, considerando também pauta feita ou cancelada. Portanto N agrega **pauta faltante** sob o rótulo “pergunta sem emitir”. (`app/chamados/services/fabrica_da_celula.py:205–207,214–256`) |

**Horizonte, chamado vivo e tranca não podem ser transferidos automaticamente para a pergunta.** O piso/horizonte e `_cala_emissao` guardam a emissão do **chamado** no corpo do cartório; a fábrica de perguntas roda depois para as datas julgadas e não consulta esse piso na seleção compartilhada. (`app/ponto/services/cartorio.py:1687–1708,1006–1008`; `app/chamados/services/fabrica_da_celula.py:105–150`)

Antes de chegar à fábrica direta, o julgamento pode sair por futuro/célula inexistente, turno de hoje ainda não terminado sem modo de teto, nenhum marco vencido no modo de teto, impressão igual, ausência do dia na grade ou modo de completar somente status. O vigia não reproduz todos esses caminhos de execução: consulta o estado disponível da célula. (`app/ponto/services/cartorio.py:1971–1998,1324–1378,959–961`; `app/chamados/services/fabrica_da_celula.py:233`)

A comparação semântica tem quatro limites:

- **Dia:** fábrica e pergunta datada usam o dia do turno ancorado em `cel.data`; o teto encadeia a madrugada. Embora o coletor leia também a véspera, a fábrica exige `f[0] == dia`. Conversa usa outra porta e data civil, ficando fora desse dedup. (`app/ponto/services/cartorio.py:831–859`; `app/chamados/services/fabrica_da_celula.py:68–73,165`; `app/chamados/models.py:2776–2802`)
- **Dia em curso:** a fábrica pode emitir hoje após vencimento do marco; o contador exclui hoje inteiro e ontem ainda sob responsabilidade do vigia de hora. São recortes deliberadamente diferentes. (`app/chamados/management/commands/disparar_perguntas_competencia.py:91–111`; `app/chamados/services/fabrica_da_celula.py:107–112`)
- **Parcial:** o cartório passa `julgada_antes`, capturado antes do corpo; vigia e consumidor usam o `julgada_em` persistido. A mesma função pode, portanto, responder diferente por receber outro intervalo de “fato novo”; a parcial elegível no primeiro ato pode ficar excluída depois. (`app/ponto/services/cartorio.py:951–958,1008,395–400`; `app/chamados/services/fabrica_da_celula.py:116–118,233,292`)
- **Emissão confirmada:** `perguntar_pela_celula` devolve os motivos selecionados, não uma confirmação de INSERT; `perguntar` ignora a lista devolvida pelo gerador. Logo `[PERGUNTA]`/`perguntas_no_julgamento` não são prova independente de nascimento. (`app/chamados/services/fabrica_da_celula.py:153–168,208–211`; `app/ponto/services/cartorio.py:1008–1011`)

O warning do col969 significa que o leitor respondeu `sem_celula` — inclusive podendo ser ata sem lâmpadas — e havia escala vigente. `_ata_do_dia` devolve `(False,None)`; **sob a mesma leitura, esse par gera zero motivos no N**. Sua presença entre os 111 é **[não verificado]**. (`app/chamados/services/declaracao_texto.py:93–95,123–127`; `app/chamados/services/disputa_emissao.py:899–909`; `app/chamados/services/fabrica_da_celula.py:64–66`)

**3. Candidatas, sem escolher**

| Candidata | Sítio e mecanismo possível | Caso que a distinguiria |
|---|---|---|
| **A — rótulo divergente** | Limbo entregue sem pauta entra em N, embora o destinatário devido seja o DP, não o colaborador. (`app/chamados/services/fabrica_da_celula.py:205–207,233–256`) | Célula contada com `trabalha=None`, competência entregue e motivo sem pauta: prova que aquele item é falta de pauta. Presença nos 111 **[não verificado]**. |
| **A — retrato anterior à cura** | N permanece o calculado antes do fio mudo, mesmo que ele escreva a pergunta na rodada. (`app/chamados/management/commands/disparar_perguntas_competencia.py:108–113,289–303`) | Pergunta do fato ausente na contagem e presente após o `--apply`, apesar do N/rc2. O teste cobre esse caso; ocorrência nas rodadas fornecidas **[não verificado]**. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:177–184`) |
| **B — dedup anterior ao fallback** | Para entrada/saída, sem destino inequívoco, `_data_ev=None`; o gerador procura pergunta NULL e pode pular **antes** de usar o dia válido do contexto. (`app/chamados/services/disputa_emissao.py:238–244,410–424`) | Célula D elegível, sem pergunta D; disputa com pergunta não-conversa do mesmo motivo, data NULL, fora das vias excluídas; nenhum destino derivável. O vigia conta D e o gerador pode calar. Esse mecanismo específico não é o dos intervalos, que consultam `_data_fallback`. Estoque correspondente **[não verificado]**. (`app/chamados/models.py:2423–2425,2753–2755`; `app/chamados/services/disputa_emissao.py:395–400`) |
| **B — identidades diferentes no pré-dedup** | Gerador consulta disputa/motivo/data sem exigir instância e coluna colaborador; vigia exige essas dimensões. (`app/chamados/services/disputa_emissao.py:395–420`; `app/chamados/models.py:2753–2755`) | Linha de estoque com instância/colaborador incoerente bloqueia o gerador sem satisfazer o vigia. A inserção normal atual deriva/valida essas colunas; existência desse estoque **[não verificado]**. (`app/chamados/models.py:3425–3447`) |
| **C — nenhuma nova tentativa no estado devido** | Impressão igual pula o julgamento e a fábrica direta; consumidor só pergunta datas dos eventos recebidos. (`app/ponto/services/cartorio.py:1373–1375,1006–1008`; `app/chamados/services/fabrica_da_celula.py:288–292`) | Célula ainda elegível, julgamento pulado e nenhum evento vivo dessa data após tornar-se devida. Falta de `EventoCelula` isoladamente não basta: é preciso excluir a tentativa direta. Ocorrência **[não verificado]**. |
| **C — evento consumido antes do vencimento** | Fábrica pode retornar vazio por teto; consumidor marca processado se não houver falha nem adiamento. O adiamento mostrado é o das órfãs, não uma retenção geral por futura pergunta. (`app/chamados/services/fabrica_da_celula.py:58–61`; `app/chamados/reconciliador.py:1375–1401`) | Evento processado antes de vencer o marco, sem evento/rejulgamento posterior da data, seguido de motivo contado. Ocorrência **[não verificado]**. |

Abaixo da seleção também existem recusas por catálogo ausente, texto sem data, pergunta de hora sem dia, destino incompatível e contrato de saída. A fábrica normalmente fornece a data da célula; falha de costura **após** nascimento deixa pergunta órfã, portanto isoladamente não explica ausência da pergunta no N. (`app/chamados/services/disputa_emissao.py:314–316,344–366,424–470`; `app/chamados/services/fabrica_da_celula.py:165`; `app/chamados/models.py:2818–2822,2839–2840,2888–2890`)

**Não é possível atribuir os 111 a A, B ou C com a amostra fornecida, nem afirmar que nenhuma delas ocorreu.** Faltam as situações individuais de célula, pergunta, pauta e evento nos instantes correspondentes. **[não verificado]**

**4. Portas existentes de reparo de estoque**

Existe `consumir_eventos_celula --semear`, mas ele **semeia e retorna**, sem consumir na mesma chamada. Seu universo é: células de chamados vivos de módulos de turno, mais células de perguntas órfãs que `fantasma_pela_lampada` encerraria; exige célula existente e ausência de evento pendente vivo. Não é uma varredura geral das células acusadoras sem pergunta. (`app/chamados/management/commands/consumir_eventos_celula.py:14–15,28–31`; `app/chamados/reconciliador.py:1201–1204,1278–1312`)

O ramo de órfãs da semeadura alcança `orfao_14h` e `saida_sem_entrada`, com dia, sem materialização/validação e em disputa aberta; não inclui os intervalos por esse ramo. (`app/chamados/reconciliador.py:1037–1051,1302–1308`)

O cron declarado chama o consumidor a cada minuto **sem `--semear`**. Uso histórico da semeadura em produção: **[não verificado]**. (`app/config/crons.py:393–394`)

Outra porta existente é `processar_cartorio --empresa X --apply --forcar`: todas as células passadas daquela empresa, sem recorte de competência nem filtro de situação; `--forcar` atravessa o pulo por impressão igual, mantendo as guardas da fábrica. (`app/ponto/management/commands/processar_cartorio.py:25–29,45–65`; `app/ponto/services/cartorio.py:1006–1008`)

O outbox nasce atomicamente com saves de ata/veredito pelas portas correspondentes; o consumidor só considera pendentes com menos de cinco tentativas. Falha mantém pendência e incrementa tentativa; esgotamento sai da fila viva. (`app/ponto/portas/celula.py:892–916`; `app/chamados/reconciliador.py:1207–1209,1246–1255,1392–1401`)

**5. Quem lê e o que supõe**

| Leitor | Semântica efetiva |
|---|---|
| **`placar_code.sh`** | Raspa o último N compatível de `logs/fio_mudo.log`; exige mtime do arquivo desde meia-noite, senão mostra `?`. Declara esperado 0, dono Code, fora do TOTAL. A frescura verificada é do arquivo, não da linha individual raspada. (`bin/placar_code.sh:388–390`) |
| **`ESTADO`** | O gerador pode transportar texto incidentalmente pelas últimas 40 linhas do painel tmux. Esse trecho não interpreta N. Leitor específico do N ou ESTADO publicado contendo-o: **[não verificado]**. (`bin/gerar_estado.py:184–190,367–389`) |
| **`pipeline_placar`** | Lê JSONL de execução, não o N: nome, empresa, início, exit e duração. Exit 2 vira `ALARME` de negócio; somente `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA` entram no conjunto que faz o próprio pipeline alarmar. (`app/core/management/commands/pipeline_placar.py:60–80,84–109,130–135`) |
| **Envelope/corrente** | N positivo pode produzir rc2; a corrente usa `--comp-corrente --apply`. `cron_run` grava o exit e carimba conclusão também em rc2, permitindo continuidade. Rc2 sozinho não distingue falta da célula de alarme do fio mudo. (`app/chamados/management/commands/disparar_perguntas_competencia.py:247–263,302–303`; `app/config/crons.py:1521–1523`; `bin/cron_run.sh:11–25`) |
| **Testes de fábrica/vigia** | Exigem emissão idempotente pelo julgamento; lote conta sem escrever pela célula; limbo cobra; folga/já perguntada não contam; limbo entregue conta falta de pauta; fio mudo continua escritor. (`app/chamados/tests/test_fabrica_por_evento.py:34–81`; `app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98–194`) |
| **Testes de relógio/instrumento** | Exigem hoje fora, entrada posterior, preservação da borda e tratamento do noturno; exigem log persistente para o elo. Essas são expectativas do código de teste; execução nesta cópia **[não verificado]**. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:109–163,374–401`; `app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19–28`) |

## 10/11 00:01 · sessao 7e1e0940e71c · - · - · origem=Ronald (tmux)

Rastreio só leitura, com os principais sítios:

- **Responsável por emitir:** `julgar_colab`, quando `apply_`, chama a fábrica apenas para as células cuja data entrou no `cods` deste ato; células puladas por impressão não passam nessa chamada. O `cods` é isolado do acumulador do lote. `app/ponto/services/cartorio.py:938`, `app/ponto/services/cartorio.py:993`, `app/ponto/services/cartorio.py:1006`.
- **Segundo gatilho:** consumidor do outbox chama `perguntar_por_eventos` depois de rejunção, retratação de perguntas/chamados/premissa, órfãs e lastro; essa função relê as células **atuais** apenas das datas dos eventos daquele colaborador, sem rejulgá-las. `app/chamados/reconciliador.py:1363`, `app/chamados/reconciliador.py:1384`, `app/chamados/services/fabrica_da_celula.py:279`.

**Quatro motivos, uma mesma regra:** início/E → `saida_sem_entrada`; fim/S → `orfao_14h`; ida/S → `intervalo_saida`; volta/E → `intervalo_volta`. Os quatro são família MARCO. Nesse caminho os nomes são códigos de lâmpadas faltantes: não há teste próprio “14 horas” para `orfao_14h`, nem exigência de saída existente para `saida_sem_entrada`. `app/chamados/services/declaracao_texto.py:42`, `app/chamados/catalogo/motivos.py:34`, `app/chamados/catalogo/motivos.py:48`, `app/chamados/services/fabrica_da_celula.py:68`.

**Guardas comuns a escritor e vigia via `motivos_a_perguntar`:**

- `dia_sem_trabalho(cel)` ou situação diferente de `ativo`; dia futuro; veredito preenchido fora de `VEREDITOS_ACUSAM`; `parcial_so_lista`; falta de marco vencido; ata que não autoriza/faltantes desconhecidos; lâmpada que não é faltante; motivo já ocupado no domínio do fato. `app/chamados/services/fabrica_da_celula.py:105`, `app/chamados/services/fabrica_da_celula.py:107`, `app/chamados/services/fabrica_da_celula.py:112`, `app/chamados/services/fabrica_da_celula.py:113`, `app/chamados/services/fabrica_da_celula.py:116`, `app/chamados/services/fabrica_da_celula.py:132`, `app/chamados/services/fabrica_da_celula.py:147`.
- `dia_sem_trabalho` significa `trabalha=False` ou `ata.tipo_dia` preenchido diferente de trabalho; ausência parcial mantém cobrança do resto, salvo `trabalha=False`. `app/chamados/juizes.py:426`, `app/chamados/juizes.py:435`.
- Sem turno previsto no passado: escritor pauta CADASTRO e retorna; vigia também obtém zero motivos porque `vencidos=None` termina em lista vazia, mas este N não mede ausência dessa pauta. `app/chamados/services/fabrica_da_celula.py:35`, `app/chamados/services/fabrica_da_celula.py:58`, `app/chamados/services/fabrica_da_celula.py:192`.
- Limbo (`trabalha=None`) em competência entregue: escritor encaminha PAUTA DP, não pergunta; vigia conta motivos ainda sem pauta, inclusive considerando pautas feitas/canceladas. Logo o rótulo “perguntas não emitidas” agrega também falta de pauta. `app/chamados/services/fabrica_da_celula.py:205`, `app/chamados/services/fabrica_da_celula.py:219`, `app/chamados/services/fabrica_da_celula.py:233`, `app/chamados/services/fabrica_da_celula.py:250`.

**“Emitida” significa identidade existente, não pergunta viva/pendente:** dedup lê pergunta `conversa=False`, data igual à célula, instância do motivo, coluna colaborador; só duplicata/motivo retratado saem do domínio. Pergunta decidida/materializada pode segurar identidade. A consulta externa ainda exige `disputa__colaborador=colab`, e não simplesmente a coluna da pergunta; isso é comum ao vigia e à pré-seleção do escritor. `app/chamados/services/fabrica_da_celula.py:138`, `app/chamados/services/fabrica_da_celula.py:141`, `app/chamados/models.py:2746`, `app/chamados/models.py:2393`.

**Assimetria temporal que deve constar:** o cartório passa julgamento **anterior** capturado antes do corpo; vigia e consumidor usam `cel.julgada_em` persistido. Para parcial com período, esse carimbo participa de `parcial_so_lista` (“já julgada e sem fato novo”). Portanto compartilham função, mas não necessariamente o mesmo insumo temporal; a diferença tende a excluir no vigia/consumidor uma parcial que o primeiro ato podia emitir. `app/ponto/services/cartorio.py:951`, `app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:116`, `app/chamados/services/fabrica_da_celula.py:233`, `app/chamados/services/fabrica_da_celula.py:292`, `app/ponto/services/cartorio.py:395`.

**Horizonte/dia em curso:** piso, chamado vivo, desligamento, afastado e existência de vínculo/marco guardam a emissão do **chamado** no corpo; não são uma guarda direta adicional da fábrica de perguntas, chamada depois para datas julgadas. A fábrica usa sua guarda ativo + motivos da ata. `app/ponto/services/cartorio.py:1687`, `app/ponto/services/cartorio.py:1695`, `app/ponto/services/cartorio.py:1707`, `app/ponto/services/cartorio.py:1722`, `app/ponto/services/cartorio.py:1728`, `app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:105`.

O julgamento pode pular por nenhum marco vencido em hoje/ontem no modo de teto; impressão igual; ausência de dia na grade; modo completar status. Mesmo depois de julgar, veredito/ata podem legitimamente impedir motivos. `app/ponto/services/cartorio.py:1324`, `app/ponto/services/cartorio.py:1329`, `app/ponto/services/cartorio.py:1370`, `app/ponto/services/cartorio.py:1373`, `app/ponto/services/cartorio.py:1376`, `app/ponto/services/cartorio.py:960`.

**Dia civil × turno:** teto usa marcos encadeados do turno ancorado em `cel.data`, +30 minutos; noturno de ontem pode ter saída ainda não vencida hoje. Embora `_marcos_faltantes` leia dia e véspera, fábrica só aceita faltantes com `f[0]==cel.data`. Escritor passa essa data no contexto; geração abre pergunta datada pelo dia do turno, conversa usa outra porta com dia civil. `app/ponto/services/cartorio.py:828`, `app/ponto/services/cartorio.py:831`, `app/ponto/services/cartorio.py:858`, `app/chamados/services/declaracao_texto.py:98`, `app/chamados/services/declaracao_texto.py:135`, `app/chamados/services/fabrica_da_celula.py:71`, `app/chamados/services/fabrica_da_celula.py:165`, `app/chamados/services/disputa_emissao.py:443`, `app/chamados/models.py:2794`.

**Warning 969:** `_ata_do_dia` emite essa luz quando leitor diz `sem_celula` e existe escala vigente; também pode representar ata sem lâmpadas, não só ausência física de célula. Retorna `(False,None)`; vigia e fábrica retornariam zero motivos sob essa leitura. Logo não se pode ligar o warning aos 111 só pelo dado fornecido. `app/chamados/services/declaracao_texto.py:93`, `app/chamados/services/declaracao_texto.py:123`, `app/chamados/services/disputa_emissao.py:899`, `app/chamados/services/fabrica_da_celula.py:64`; relação de produção [não verificado].

**Ponto de cuidado sobre retorno:** `perguntar_pela_celula` devolve `ms` selecionados, não o número confirmado de linhas criadas; `perguntar` chama geração, ignorando sua lista de retorno. A geração pode continuar por ausência de catálogo/data/dedup e a porta pode reusar linha. Portanto contador `perguntas_no_julgamento` não é confirmação independente de INSERT. `app/chamados/services/fabrica_da_celula.py:153`, `app/chamados/services/fabrica_da_celula.py:165`, `app/chamados/services/fabrica_da_celula.py:208`, `app/ponto/services/cartorio.py:1009`, `app/chamados/services/disputa_emissao.py:314`, `app/chamados/services/disputa_emissao.py:360`, `app/chamados/services/disputa_emissao.py:398`, `app/chamados/models.py:2776`.

**Eventos:** apenas gravações dos campos ata/veredito acordam no `salvar_e_emitir`; save e evento são atômicos. Evento também nasce nas demais portas documentadas: chamado de turno nascendo, turno aberto fechando, pergunta alterando campo que solta chamado, semeadura. `app/ponto/portas/celula.py:892`, `app/ponto/portas/celula.py:896`, `app/ponto/portas/celula.py:908`.

Consumidor marca processado inclusive quando fábrica legitimamente retorna vazio; exceção impede marca e aumenta tentativa, teto de órfã adia sem gastar tentativa; pendências com 5 falhas saem dos vivos. Assim uma guarda temporal da fábrica que não provoque `adiar` por órfã pode consumir o evento antes do vencimento, dependendo dos demais gatilhos para voltar àquela data — possibilidade estrutural, ocorrência nos 111 [não verificado]. `app/chamados/reconciliador.py:1207`, `app/chamados/reconciliador.py:1387`, `app/chamados/reconciliador.py:1392`.

**Contraexemplos úteis para distinguir sem escolher:**

- A/rótulo: célula limbo entregue com motivo devido sem pauta: N>0, emissão ao colaborador proibida e falta efetiva é pauta; ou parcial cuja diferença é o carimbo anterior/persistido. Sítios acima; presença na amostra [não verificado].
- B/execução: célula julgada neste ato, `ms` não vazio, sem pergunta/pauta depois, evento disponível/consumido e nenhuma exceção; inspecionar continuidade entre seleção → geração/porta. `app/ponto/services/cartorio.py:1008`, `app/chamados/services/fabrica_da_celula.py:208`, `app/chamados/services/disputa_emissao.py:300`; existência de tal célula [não verificado].
- C/gatilho: célula ainda devida pelo vigia, mas dia pulado por impressão e sem novo evento vivo; ou evento da data consumido quando marco não vencera e data não voltou ao julgamento. Diferente de B porque não houve tentativa emissora naquele estado/instante. `app/ponto/services/cartorio.py:1373`, `app/ponto/services/cartorio.py:1006`, `app/chamados/services/fabrica_da_celula.py:288`, `app/chamados/reconciliador.py:1400`; ocorrência na amostra [não verificado].

## 10/11 00:00 · sessao ad68d6da3c84 · - · - · origem=Ronald (tmux)

Achados para consolidação, todos condicionais quanto à produção:

- Os quatro motivos são de marco: entrada `('inicio','E') → saida_sem_entrada`; saída `('fim','S') → orfao_14h`; intervalo `('int_ida','S') → intervalo_saida` e `('int_volta','E') → intervalo_volta`. As instâncias vêm dessa mesma tabela. `app/chamados/services/declaracao_texto.py:42-57`; família MARCO em `app/chamados/catalogo/motivos.py:34-35,48-49`.

- O warning `sem_celula ... criar_questionario` significa que `_marcos_faltantes` respondeu sem ata ou sem marcos e havia escala vigente; `_ata_do_dia` acende warning e devolve `(False,None)`. Sem escala vigente, devolve `(True,None)`; erro de leitura também devolve `(True,None)`. Todos esses resultados produzem zero motivos no vigia porque `motivos_pela_ata` exige `pode=True` e `faltantes is not None`. Portanto o warning col969/dia07 não demonstra participação nos 111 sob a mesma foto. `app/chamados/services/disputa_emissao.py:884-910`; `app/chamados/services/fabrica_da_celula.py:64-66`. Associação temporal entre warning e contador além do dado fornecido: [não verificado].

- `_marcos_faltantes` considera `missing` verdadeiro na ata; ignora isento, fora do vínculo, dia sem trabalho, lâmpada sem informação e ausência parcial que não deixa resto cobrável. A véspera só contribui saída final; se há faltantes no próprio dia, elimina a véspera. A fábrica ainda exige `f[0] == dia`, portanto os motivos da célula permanecem no dia do turno dela. `app/chamados/services/declaracao_texto.py:103-142`; `app/chamados/services/fabrica_da_celula.py:68-73`.

- A fábrica já passou pela ata antes de chamar `perguntar`: `motivos_a_perguntar → motivos_pela_ata` e `perguntar_pela_celula → perguntar`. `_abrir_ou_agregar_disputa` somente cria/agrega disputa; `criar_ou_atualizar_disputa` gera perguntas em disputa nova e reutilizada. Não há segundo gate de ata dentro de `gerar_perguntas_disputa`. `app/chamados/services/fabrica_da_celula.py:132,198-210`; `app/chamados/services/disputa_emissao.py:474-603,669-682`.

**Divergência estrutural concreta, candidata B, condicional ao estoque:**

1. `saida_sem_entrada` e `orfao_14h` precisam de derivação de chamado-destino; os dois intervalos são datados, mas não pertencem a esse conjunto. `app/chamados/models.py:3657`; `app/chamados/services/disputa_emissao.py:26`.
2. Sem destino inequívoco, `_derivar_chamado_destino` retorna `(None,None)`. `app/chamados/services/disputa_emissao.py:238-244`.
3. Entrada/saída fazem dedup por `data_evento=_data_ev`, agora `None`, antes de escolher `_data_fallback` do dia válido. Uma pergunta anterior não-conversa, fora das vias retratada/duplicata, de mesmo motivo e mesma disputa, com `data_evento=NULL`, provoca `continue`; a célula D segue sem pergunta. O campo permite NULL. `app/chamados/services/disputa_emissao.py:410-424`; `app/chamados/models.py:2423-2425`.
4. O vigia consulta o fato do dia D, com instância correta; a pergunta NULL não satisfaz essa consulta. Intervalos usam diretamente `_data_fallback=D` nesse pré-dedup, logo não compartilham esse exato mecanismo. `app/chamados/models.py:2746-2755`; `app/chamados/services/disputa_emissao.py:395-400`.

**Contraexemplo distinguidor:** célula D elegível, sem pergunta do fato D; disputa aberta contendo pergunta NULL do mesmo motivo; nenhum destino derivável. O vigia conta, o gerador pula. Existência desse conjunto entre os 111: [não verificado].

**Outra divergência condicional:** o pré-dedup do gerador usa disputa/motivo/data, sem exigir `instancia_marco` e coluna `colaborador`, enquanto o vigia exige ambos. Estoque inconsistente poderia fazê-lo contar e o gerador calar; a inserção normal atual deriva/valida essas colunas, portanto não prova ocorrência normal. `app/chamados/services/disputa_emissao.py:395-420`; `app/chamados/models.py:2753-2755,3425-3447`.

Outras guardas abaixo da fábrica:

- Motivo sem pergunta no catálogo: pula; os quatro da amostra possuem pergunta de hora no catálogo. `app/chamados/services/disputa_emissao.py:314-316`; `app/chamados/catalogo/perguntas.py:48-61,124-137`.
- Texto sem data e sem fallback da grade: pula; hora sem dia do turno: pula. A fábrica normal passa `{motivo: {'data': dia.isoformat()}}`, então essas guardas não deveriam bloquear esse caminho sob entradas normais. `app/chamados/services/disputa_emissao.py:344-366,424-430`; `app/chamados/services/fabrica_da_celula.py:165`.
- Porta recusa destino de outro colaborador ou resolvedora de outro turno; contrato de saída desfaz o ato se o dia gravado não corresponde ao recebido pelo ponto único. `app/chamados/models.py:2839-2840,2888-2890,2818-2822`.
- Para MARCO normal, não deriva episódio de batida; o dia pedido da célula é confirmado como dia do turno. `app/chamados/models.py:3458-3462`; `app/chamados/services/fio.py:1630-1640`.
- O dedup do fato inclui qualquer estado; exclui conversa, duplicata e motivo retratado. Pergunta decidida também segura o fato. `app/chamados/models.py:2697-2728,2746-2755`.
- Reatar pergunta órfã ocorre depois do nascimento; falha é registrada e mantém a pergunta órfã. Logo essa falha de costura, isoladamente, não explica falta de pergunta contada por N. `app/chamados/services/disputa_emissao.py:459-470`.

## 10/11 00:00 · sessao 4fa80517892c · - · - · origem=Ronald (tmux)

**4. Portas de reparo**

- Existe `consumir_eventos_celula --semear`; o comando semeia e **retorna sem consumir**. A execução seguinte sem `--semear` consome. (`app/chamados/management/commands/consumir_eventos_celula.py:14-15,28-31`)
- Universo da semeadura: uma célula por `(colaborador, dia)` de **chamado vivo de módulo de turno**, mais as perguntas órfãs que `fantasma_pela_lampada` encerraria; exige célula existente e ausência de pendente vivo. **Não varre genericamente todas as células acusadoras sem pergunta**. (`app/chamados/reconciliador.py:1201-1204,1278-1311`)
- O ramo de órfãs considera perguntas sem materialização, sem validação, disputa aberta, motivo `orfao_14h`/`saida_sem_entrada`, dia presente, submetidas ao predicado; não cobre intervalos por esse ramo. (`app/chamados/reconciliador.py:1037-1051,1302-1308`)
- Chamador operacional encontrado da semeadura: o management command; chamadas explícitas adicionais estão nos testes. O cron declarado chama apenas o consumidor, **a cada minuto, sem `--semear`**. Execução histórica manual em produção: **[não verificado]**. (`app/chamados/management/commands/consumir_eventos_celula.py:28-31`; `app/config/crons.py:393-394`; `app/chamados/tests/test_orfas_pelo_evento.py:203-214`)
- Consumir não amplia para todos os dias do colaborador: a fábrica relê células atuais **das datas dos eventos do lote**. (`app/chamados/services/fabrica_da_celula.py:279-293`)
- Outra porta: `processar_cartorio --empresa X --apply --forcar`; `--forcar` manda rejulgar mesmo com impressão igual. Universo: todas as células passadas da empresa, sem filtro de situação do colaborador; não tem recorte de competência nesse comando. A fábrica continua sujeita às suas guardas. (`app/ponto/management/commands/processar_cartorio.py:25-29,45-65`)
- O teste da semeadura prova semear → consumir → encerrar órfã, idempotência e não semear lâmpada apagada; não prova recuperação universal das perguntas ausentes do contador. (`app/chamados/tests/test_orfas_pelo_evento.py:200-228`)

**5. Leitores**

- `placar_code.sh`: raspa **última linha compatível** de `logs/fio_mudo.log`; se o arquivo não tiver mtime desde meia-noite local, imprime `?`. Supõe número esperado zero, dono Code, **fora do TOTAL**. A frescura é do arquivo, não do timestamp individual da linha raspa­da. (`bin/placar_code.sh:388-390`)
- `ESTADO`: não encontrei leitor estruturado de N no gerador; ele captura as últimas 40 linhas do painel tmux, podendo transportar N incidentalmente se estiverem ali. O gerador publica seus placares estrutural/espelho por outras fontes. Um ESTADO publicado contendo N em produção: **[não verificado]**. (`bin/gerar_estado.py:184-190,367-389`)
- `pipeline_placar`: não lê N nem `fio_mudo.log`; lê `placar.jsonl` com início, nome, empresa, exit e duração. Exit 2 vira **alarme de negócio**, distinto de erro de execução; pode apresentar `ALARME`. Porém o conjunto que faz o próprio pipeline alarmar contém `ERRO`, `NAO_RODOU` e `DEPENDENCIA_QUEBRADA`, não `ALARME`. (`app/core/management/commands/pipeline_placar.py:60-80,84-109,130-135`)
- Relação indireta: N positivo provoca `SystemExit(2)` em `--alarme` ou `--apply`; a corrente usa `--comp-corrente --apply`. `cron_run` grava esse exit no JSONL e **carimba conclusão também para RC2**, permitindo a corrente continuar. (`app/chamados/management/commands/disparar_perguntas_competencia.py:247-263,302-303`; `app/config/crons.py:1521-1523`; `bin/cron_run.sh:11-25`)
- Testes antigos assumem julgamento emite uma vez e lote conta o que ficou; teste de costura exige que `apply_=False` não pergunte. (`app/chamados/tests/test_fabrica_por_evento.py:34-43,60-81`)
- Testes vigentes do vigia exigem: `--apply` não escrever pelo caminho da célula, contar e sair 2; limbo cobrar; folga não contar; já perguntada não contar; limbo exportado com pauta não contar, sem pauta contar; parcial sem período não contar; fio mudo continua escritor. (`app/chamados/tests/test_jv4a_disparar_vigia_da_celula.py:98-149,152-194`)
- Os testes de relógio exigem hoje fora do contador, entrada no dia seguinte; piso inclui fim da competência anterior; noturno de ontem fica fora enquanto é candidato do vigia de hora e entra depois. (`app/chamados/tests/test_pergunta_do_dia_em_curso.py:109-163,374-401`)
- O selo do elo exige log persistente `fio_mudo.log` e `--apply`, sem validar a semântica de N. (`app/chamados/tests/test_jv4a_elo_encadeado_tem_log.py:19-28`)

## 09/11 23:54 · sessao 19c98713e862 · REVERTER-DIA-SEM-RECALCULO · censo · origem=Code

**A assimetria está confirmada no código:** a IDA passa pelo caminho comum que julga e agenda recálculo financeiro; a VOLTA restaura a célula, emite evento e julga, mas termina sem esse recálculo. Portanto, sem outro fato que provoque recálculo, o `DiaPago` pode conservar o previsto da regeneração desfeita. É uma conclusão estática; ocorrência e quantidade em banco real **[não verificado]**. Fontes: `app/ponto/portas/celula.py:354`, `app/ponto/portas/celula.py:458`, `app/ponto/portas/celula.py:785`, `app/ponto/portas/celula.py:817`, `app/ponto/portas/celula.py:822`.

**Há duas ressalvas:** existe cascata indireta **condicional** até recálculo, e a trava genérica da L-092 foi revogada pela L-113; permanece uma guarda específica de **09/2026 exportada**. Fontes: `app/chamados/signals.py:120`, `app/chamados/services/regularizacao_ext.py:218`, `app/docs/LEIS.md:153`, `app/ponto/services/fechamento.py:36`, `app/ponto/services/fechamento.py:560`.

**1. IDA: portas, gravações e pós-escrita**

| Porta | Gravação e transação | Depois de gravar |
|---|---|---|
| `regenerar_celulas_vinculo` | `atomic` local; cria ausentes; reescreve `trabalha`, `escala_geradora`, `dna`, `dna_anterior`, `regenerada_em`, `regeneracoes`; marca cobranças órfãs e registra auditoria. `app/ponto/portas/celula.py:218`, `:258`, `:271`, `:289`, `:331`. | Sai do `atomic` local e chama `_o_chao_do_dia_mudou`. `app/ponto/portas/celula.py:354`. |
| `regenerar_celulas_dia` | Delega à anterior com `[dia,dia]` e `estender_horizonte=False`; mantém recálculo padrão. `app/ponto/portas/celula.py:734`. | Mesmo caminho da regeneração por vínculo. `app/ponto/portas/celula.py:734`. |
| `editar_dia` | Exige autor e motivo; `atomic` com `select_for_update`; grava `trabalha`, DNA, vínculo gerador, `origem='editada'`, `editada_em`, `editada_por`; auditoria obrigatória e marcação de órfãs. `app/ponto/portas/celula.py:559`, `:592`, `:600`, `:609`, `:611`, `:619`, `:625`. | Chama o mesmo `_o_chao_do_dia_mudou`. `app/ponto/portas/celula.py:627`. |
| `trocar_plantao` | Duas chamadas de `editar_dia` dentro de um `atomic` externo; recusa de um lado desfaz o par; trilha obrigatória. `app/ponto/portas/celula.py:652`, `:675`, `:676`, `:685`, `:692`. | Julgamentos acontecem dentro do `atomic` externo; callbacks ficam para seu commit. Inferência de `app/ponto/portas/celula.py:675`, `:434`, `:502` e `app/ponto/services/fechamento.py:1824`. |
| `gerar_celulas_periodo` | Pode criar célula passada ausente; passado existente somente recebe DNA quando nulo. `app/ponto/portas/celula.py:862`, `:870`. | Retorna sem chamar o caminho comum de julgamento/recálculo. O command envolve a geração em `atomic`. `app/ponto/portas/celula.py:887`; `app/ponto/management/commands/gerar_celulas.py:309`. |

O caminho comum `_o_chao_do_dia_mudou` executa, nesta ordem:

1. Com `recalculo=True`, julga dias tocados não futuros, excluindo vereditos humanos, usando `julgar_celula(..., apply_=True, forcar=True)`; falhas são logadas. `app/ponto/portas/celula.py:422`, `:427`, `:434`, `:437`.
2. Deduplica competências por `janela_atual` e chama `recalcular_por_evento` uma vez por competência. A execução financeira é agendada pelo próprio serviço em `on_commit`. `app/ponto/portas/celula.py:445`, `:453`, `:458`; `app/ponto/services/fechamento.py:1824`.
3. Agenda em outro `on_commit` a rejunção de chamados e a reavaliação de faltas espúrias, inclusive com `recalculo=False`. `app/ponto/portas/celula.py:490`, `:498`, `:501`, `:502`.

A emissão normal de `EventoCelula` acompanha a escrita de veredito/ata: `salvar_e_emitir` salva e insere o evento no mesmo `atomic` quando os campos lavrados incluem `veredito` ou `ata`. Não é um signal genérico de DNA. `app/ponto/portas/celula.py:893`, `:908`, `:912`, `:914`, `:916`.

Os principais caminhos de entrada encontrados e suas exceções são:

| Entrada | Comportamento |
|---|---|
| Correção de vínculo | Chama alias `_regen` com autor, motivo e retorno dos dias julgados; depois reconcilia evitando repetir julgamento. `app/colaboradores/services/vinculo.py:311`, `:328`. |
| Signals de vigência e âncora | Chamam regeneração na transação de quem salvou o vínculo. `app/escala/signals.py:34`, `:71`, `:79`, `:117`. |
| Foto de folgas | Chama regeneração, podendo pedir `rejulgar` mesmo sem alteração do DNA; lançamento manual envolve foto e regeneração em `atomic`. `app/escala/folgas.py:264`, `:272`, `:309`. |
| Alteração de template em frota | Usa `recalculo=False` e depois `invalidar_fechamento`; não faz o recálculo financeiro normal. `app/escala/services/cadastro_tipo.py:348`, `:357`. |
| Troca de município | Regenera desde hoje com `recalculo=False` e invalida fechamento; mudança da flag de feriado usa recálculo padrão. `app/colaboradores/services/vinculo.py:903`, `:857`. |
| Correção retroativa | Regenera com motivo e possível abertura da lavra; depois reconcilia. `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230`, `:236`. |
| `regenerar_em_exportada` | Exige staff e motivo; dentro de `atomic`, regenera, chama fechamento explicitamente e executa `retratar_exportada`. `app/ponto/portas/regen_exportada.py:106`, `:110`, `:169`, `:174`, `:179`, `:188`. |
| Escala automática | Aplicação e desfazer regeneram somente o futuro dentro de `atomic`. `app/escala/services/escala_auto_executor.py:96`, `:104`, `:140`, `:143`. |
| COL900 | Apply usa regeneração de dia; ensaio usa a mesma porta em transação posteriormente desfeita. `app/ponto/management/commands/aplicar_col900.py:115`; `app/ponto/management/commands/diff_col900.py:56`, `:69`. |

**2. VOLTA: gravação e grafo de chamadas**

`reverter_regeneracao_dia` recusa célula inexistente, origem diferente de `gerada` ou ausência de `dna_anterior`; também recusa competência lavrada sem `apesar_da_lavra`. `app/ponto/portas/celula.py:776`, `:777`, `:779`.

Dentro do `atomic`, ela:

- Restaura `dna=dna_anterior` e limpa `dna_anterior=None`. `app/ponto/portas/celula.py:785`, `:786`, `:787`.
- Define `trabalha` pela presença não nula de `alvo['marcos']`; restaura `escala_geradora_id` somente quando há `vinculo_id` no alvo. `app/ponto/portas/celula.py:788`, `:789`, `:790`.
- Atualiza `regenerada_em` e incrementa `regeneracoes`; salva os seis campos. `app/ponto/portas/celula.py:792`, `:793`, `:794`.
- Insere `EventoCelula` e tenta auditoria com a ação compartilhada `regenerar_celulas_vinculo`, DNAs antes/depois e `revertida=True`. Falha da trilha não desfaz a reversão. `app/ponto/portas/celula.py:796`, `:799`, `:804`, `:805`.

Depois do `atomic` local, chama `julgar_celula` para dia não futuro sem veredito humano e retorna. Não chama `_o_chao_do_dia_mudou`, `recalcular_por_evento` nem invalidação explícita do fechamento. `app/ponto/portas/celula.py:812`, `:815`, `:817`, `:822`.

O grafo explícito é:

| Aresta | Prova |
|---|---|
| VOLTA → INSERT de outbox | `app/ponto/portas/celula.py:796`, `:904`, `:905`. |
| VOLTA → `julgar_celula` → `julgar_colab` → precedência | `app/ponto/portas/celula.py:817`; `app/ponto/services/cartorio.py:2000`, `:2008`. |
| Cron de minuto → consumidor do outbox | `app/config/crons.py:394`; `app/chamados/management/commands/consumir_eventos_celula.py:26`. |
| Consumidor → rejunção, retratações, premissa, órfãs, lastro e fábrica | `app/chamados/reconciliador.py:1363`, `:1365`, `:1368`, `:1372`, `:1375`, `:1383`, `:1387`. |

**Não seria correto concluir “nunca chama indiretamente”.** Há esta cascata condicional comprovável:

| Aresta condicional | Prova |
|---|---|
| Consumidor → retratar perguntas → fechar disputa, se puder fechar | `app/chamados/reconciliador.py:1365`, `:595`, `:611`, `:612`. |
| Fechar disputa → `save` → receiver que agenda retomada | `app/chamados/models.py:1936`; `app/chamados/signals.py:120`, `:137`, `:144`. |
| Retomada → materializar regularização confirmada ainda não materializada | `app/chamados/models.py:4097`; `app/chamados/services/regularizacao_ext.py:218`, `:224`. |
| Materialização → registrar batida → recálculo por evento | `app/chamados/services/regularizacao_ext.py:70`, `:77`; `app/ponto/registro_batida.py:157`, `:160`, `:178`. |

Essa cascata exige regularização pendente de materialização, nenhuma disputa aberta e criação efetiva de batida; a competência recalculada é a **da batida**, podendo diferir da competência revertida. Portanto não substitui a manutenção financeira da VOLTA. `app/chamados/services/regularizacao_ext.py:51`, `:58`, `:73`, `:218`; `app/ponto/services/fechamento.py:1809`.

O próprio julgamento pode entrar nessa cascata antes do consumidor, quando há adiados pela lâmpada: chama a mesma retratação de perguntas. `app/ponto/services/cartorio.py:962`, `:969`.

Não encontrei receiver direto de `CelulaDia`/`EventoCelula` chamando dinheiro; ausência absoluta de ligação dinâmica ou externa **[não verificado]**. O desenho documentado proíbe esse signal, e o emissor efetivo somente insere o evento. `app/ponto/portas/celula.py:394`, `:904`.

Também não há reparação periódica garantida: o catálogo declara `recalcular_fechamento` sem cron e `lavrar_dias_pagos` como ferramenta por ato. Os comentários “o cron rega” no serviço não demonstram reparação atual. `app/config/crons.py:1130`, `:1025`; `app/ponto/services/fechamento.py:1820`.

**3. `recalcular_por_evento`: cálculo, persistência e exportada**

A função resolve a competência do fato, agenda callback via `on_commit` e chama `recalcular_fechamento_mes(..., colaborador_ids=[pk])`. Falhas de execução ou agendamento viram warning. `app/ponto/services/fechamento.py:1594`, `:1602`, `:1807`, `:1810`, `:1818`, `:1824`.

O fechamento seleciona o universo elegível, restringe o colaborador e carrega células, batidas e ausências da janela; portanto chamar a função não garante que qualquer colaborador arbitrário seja processado. `app/ponto/services/fechamento.py:554`, `:557`, `:605`, `:633`, `:639`.

Ele persiste:

- **`FechamentoMensal`:** trabalhadas, noturnas, extras total e subdivisões, folga trabalhada, atraso, saída antecipada, falta, intra indenizada, turnos abertos, inconsistências, DSR, banco, previstos/realizados/abonados, dias previstos/incertos, motivos/causa do espelho e carimbos. A lista concreta está no `update` canônico. `app/ponto/services/fechamento.py:1160`, `:1162`, `:1170`, `:1175`, `:1181`, `:1186`, `:1189`.
- **`DiaPago` versão `motor`:** rubricas diárias, minutos previstos/realizados/abonados, furo, detalhe e versões; mais linha de ajuste para DSR/banco. Substitui a competência/versão por `delete` seguido de `bulk_create`. `app/ponto/services/fechamento.py:1201`, `:1212`, `:1226`; `app/ponto/services/dia_pago.py:34`, `:43`, `:345`, `:357`, `:359`.
- **Versão `oraculo`:** também é lavrada a partir de 10/2026. `app/ponto/services/fechamento.py:1265`, `:1277`, `:1294`.

O previsto diário vem da grade: lê `minutos_previstos` de cada dia e passa o mapa à lavratura. `app/ponto/services/dia_pago.py:389`, `:393`, `:397`; `app/ponto/services/fechamento.py:1212`.

**Atomicidade:** o recálculo por evento ocorre após o commit do ato original. Não há `atomic` próprio abrangendo todo `recalcular_fechamento_mes`; a falha de `DiaPago` é capturada depois da escrita mensal, permitindo divergência adicional entre as projeções. `app/ponto/services/fechamento.py:448`, `:1144`, `:1160`, `:1238`, `:1824`.

Os callers diretos de produção encontrados são:

| Caller | Fonte |
|---|---|
| Registro de batida normal/conflito revisado | `app/ponto/registro_batida.py:155`, `:160`, `:178`. |
| Decisão de HE | `app/ponto/portas/he.py:202`. |
| Aceite de folga | `app/chamados/services/validacao.py:361`. |
| Validação de resposta | `app/chamados/services/validacao.py:446`. |
| Caminho comum de alteração da célula | `app/ponto/portas/celula.py:458`. |

**Guarda atual:** não existe recusa genérica de exportada nesse escritor; existe warning pela L-113. A guarda efetiva consulta `GUARDA_COMPETENCIA`, hoje contendo `(9, 2026)`, e exclui os colaboradores das empresas exportadas, registrando auditoria. `app/ponto/services/fechamento.py:36`, `:539`, `:542`, `:560`, `:564`, `:569`. A guarda da **célula** continua distinta e pode ser aberta por `apesar_da_lavra`. `app/ponto/portas/celula.py:211`, `:250`, `:779`.

**4. Quem chama a VOLTA; staff e trilha**

O único caller operacional encontrado foi o command `aplicar_col900 --reverter`; não encontrei view chamadora — ausência absoluta **[não verificado]**. A chamada concreta está em `app/ponto/management/commands/aplicar_col900.py:77`.

O command exige motivo de pelo menos 20 caracteres, colaborador e `--aplicar`; sem este último somente imprime simulação. A chamada omite `autor`. `app/ponto/management/commands/aplicar_col900.py:54`, `:56`, `:74`, `:77`.

A função aceita autor/motivo opcionais e não exige staff; não oferece a garantia de auditoria obrigatória da edição humana. A trilha guarda os dois DNAs e `revertida=True`, mas pode falhar sem desfazer o ato. `app/ponto/portas/celula.py:740`, `:799`, `:804`, `:805`. Fora de request, o escritor de auditoria tenta identificar `sistema:<comando>`. `app/core/observ.py:22`, `:117`, `:134`. Controle operacional de acesso ao CLI **[não verificado]**.

**5. Leitores: onde aparece e onde pode ficar escondido**

| Leitor | Fonte efetiva e implicação |
|---|---|
| Espelho, dinheiro por dia | Busca `DiaPago` e expõe linha paga, presença de lavratura e rubricas; pode conservar dinheiro diário antigo. `app/ponto/services/espelho.py:406`, `:410`, `:805`, `:811`, `:813`. |
| PDF, dinheiro por dia | Consome `rubricas` e `pago_h` dessa montagem. `app/relatorios/pdf_espelho.py:837`. |
| App, horas diárias | Prioriza linha paga existente; fallback ao motor quando não há linha. `app/api/views.py:1554`, `:1573`; `app/ponto/services/espelho.py:132`. |
| Relatório de horas | Soma `DiaPago` por período. `app/relatorios/views.py:481`. |
| Totais de espelho/PDF/app | Havendo fechamento, usa `fechamento_lido`, derivado online em modo leitura; pode mostrar total atualizado sem reparar o banco. `app/relatorios/cartao_pela_celula.py:69`, `:73`; `app/ponto/services/fechamento.py:1759`. |
| Totais sem fechamento | Há fallback à soma da lavratura. `app/relatorios/cartao_pela_celula.py:340`, `:342`, `:346`. |
| TXT gerado pela view atual | Deriva fechamentos online e injeta os mesmos objetos na classificação e montagem; não é correto afirmar que necessariamente exportará o mensal antigo. `app/folha/views.py:207`, `:212`, `:268`. |
| Helpers de export sem objetos injetados | Ainda consultam `FechamentoMensal` persistido. `app/folha/export.py:709`, `:775`. |
| Prontidão da lista de fechamento | Aceita previsto persistido quando `previsto_em` está preenchido e o valor não é nulo. A VOLTA não o invalida explicitamente. `app/folha/export.py:1037`; `app/ponto/portas/celula.py:785`, `:822`. |

A grade visual prioriza `ata.minutos_previstos`; se ausente, consulta `minutos_previstos_do_dia`, que usa os marcos da autoridade. Portanto o novo julgamento pode corrigir o previsto mostrado e o total online, enquanto o `DiaPago` permanece antigo. Inferência do fluxo em `app/escala/services/leitor_celula.py:399`, `:405`, `:411`; `app/ponto/services/fechamento.py:1083`; `app/ponto/services/fechamento.py:1759`.

O PDF também consulta diretamente o juiz de minutos previstos, que lê marcos do dia e desconta pausas. Esse é outro caminho capaz de mostrar previsto atualizado sem relavrar `DiaPago`. `app/relatorios/pdf_espelho.py:456`; `app/escala/utils.py:2268`, `:2291`, `:2305`.

**6. Testes existentes e mutante** [nao verificado]

Há teste da **IDA contra persistência real por dia**: parte de ata com 240, regenera para 480 e exige tanto ata quanto `DiaPago.minutos_previstos=480`, simulando `on_commit` imediato. `app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:78`, `:84`, `:88`, `:92`, `:94`.

Os testes encontrados da **VOLTA** cobrem restauração, idempotência, contador/trilha, guarda de exportada e mão humana; o teste de passado mocka `julgar_celula` e confere apenas sua chamada. Não conferem `DiaPago` ou fechamento nesse ciclo. `app/ponto/tests/test_porta_celula_dia.py:180`, `:194`, `:208`, `:226`, `:242`, `:258`, `:272`. O teste AST exige evento dentro do `atomic`. `app/chamados/tests/test_e4_expand.py:284`.

Assim, o mutante **“restaurar DNA, emitir evento e julgar, omitindo recálculo financeiro”** satisfaz as asserções encontradas da VOLTA — corresponde à lacuna atual. Inferência estática dessas asserções e de `app/ponto/portas/celula.py:785`, `:817`, `:822`; passagem executada do mutante/suíte **[não verificado]**.

Não encontrei teste **IDA → VOLTA → conferência do gravado diário restaurado**; ausência absoluta em toda forma possível de teste **[não verificado]**. [nao verificado]

**7. Leis e cortes: resultado da busca solicitada**

| Documento | Correspondências relevantes |
|---|---|
| `LEIS.md` | L-020 nomeia as três portas, incluindo VOLTA, como exceção ao passado imutável. `app/docs/LEIS.md:71`. L-052 descreve a guarda da regeneração. `app/docs/LEIS.md:93`. L-092 preserva a formulação histórica de exportada. `app/docs/LEIS.md:125`. L-113 registra a revogação da recusa financeira e a permanência da trava de regeneração. `app/docs/LEIS.md:153`. O ramo de regeneração fora da vigência sem os campos de denúncia aparece como lacuna. `app/docs/LEIS.md:163`. |
| `CORTES.md` | `PORTA-DO-DINHEIRO-JA-EXISTE`: regeneração deve chamar recálculo por competência; signal em `CelulaDia` recusado; frota com `recalculo=False` declarada como resíduo. `app/docs/CORTES.md:229`. Contrato do outbox: evento no mesmo `atomic`, consumidor de minuto e menção ao custo do `on_commit` financeiro. `app/docs/CORTES.md:351`. |
| `DOSSIES.md` | DNA passado alterado pela regeneração dentro do horizonte. `app/docs/DOSSIES.md:178`. O dossiê de HE ainda afirma que `recalcular_por_evento` recusa exportada; esse trecho histórico conflita com a implementação atual e a L-113. `app/docs/DOSSIES.md:279`, `:282`; `app/ponto/services/fechamento.py:539`; `app/docs/LEIS.md:153`. |

A busca não encontrou `reverter_regeneracao` nem `dna_anterior` em `CORTES.md`/`DOSSIES.md`; resultado negativo da busca estática, completude semântica **[não verificado]**.

## 09/11 23:52 · sessao 87532d78def0 · - · - · origem=Ronald (tmux)

Censo do recálculo, somente leitura por inspeção estática; nenhuma execução funcional provando valores no banco. [não verificado]

**`recalcular_por_evento` e gravações**

- Normaliza `datetime` para data local; calcula a competência pelo intervalo do fato, usando `competencias_do_intervalo`, que consulta `janela_atual(data, empresa)` e usa mês/ano do fim da janela: `app/ponto/services/fechamento.py:1558`, `:1594`, `:1602`; `app/ponto/services/dia_pago.py:713`.
- Agenda `_rodar` via `transaction.on_commit`; callback chama `recalcular_fechamento_mes(mês, ano, colaborador_ids=[pk])`; erro de execução ou agendamento vira warning e não é relançado: `app/ponto/services/fechamento.py:1807`, `:1810`, `:1818`, `:1823`.
- O escritor seleciona o universo da competência, restringe `colaborador_ids`, carrega células em lote e batidas/ausências para a janela: `app/ponto/services/fechamento.py:554`, `:557`, `:605`, `:619`, `:633`, `:639`.
- Recalcula `FechamentoMensal`, cria linha se ausente e persiste aberto com `save`; para outros status usa `update`, preservando status/aprovação: `app/ponto/services/fechamento.py:898`, `:902`, `:1137`, `:1143`, `:1160`.
- Campos persistidos: trabalhadas, noturnas, extras total, extras 50, 50 noturna, 100, 100 feriado, 100 noturna, folga trabalhada, atraso, saída antecipada, falta, intra indenizada, turnos abertos, inconsistências, semanas DSR OK/perdido, reflexo DSR, banco, minutos previstos, dias previstos/incertos, minutos abonados/realizados, motivos/causa do espelho, previsto_em, derivado_em, atualizado_em: lista canônica concreta em `app/ponto/services/fechamento.py:1160`–`:1189`.
- Previsto vem da `grade_do_fechamento`; `por_dia_da_grade` fornece os três mapas diários, usados tanto no fechamento quanto em `DiaPago`: `app/ponto/services/fechamento.py:1013`, `:1046`, `:1060`, `:1083`, `:1212`.
- No modo normal lavra `DiaPago` versão `motor` com períodos, folga, órfãos, faltas, atrasos, créditos, DSR, banco, previsto/realizado/abono e furo; carimbo `evento_origem='recalcular_fechamento_mes'`: `app/ponto/services/fechamento.py:1191`, `:1200`, `:1202`, `:1212`, `:1226`, `:1237`.
- A partir de 10/2026 também lavra versão `oraculo`, com rubricas do calculador e os mesmos mapas de grade/furo: `app/ponto/services/fechamento.py:1265`, `:1277`, `:1285`, `:1294`.
- `lavrar` substitui todas as linhas daquele colaborador/competência/versão: constrói dias, guarda furo/detalhe/versão, sempre cria linha de ajuste para DSR e banco e executa `delete` + `bulk_create`: `app/ponto/services/dia_pago.py:288`, `:335`, `:345`, `:348`, `:357`, `:359`.
- Campos `DiaPago`: 12 rubricas horárias + minutos abonados/previstos/realizados e, no ajuste, reflexo DSR/banco: `app/ponto/services/dia_pago.py:34`, `:43`.
- Falha da lavratura é capturada após a escrita do fechamento; portanto o código admite fechamento atualizado e `DiaPago` sem relavratura se essa etapa falhar: `app/ponto/services/fechamento.py:1144`, `:1160`, `:1199`, `:1238`. Não há `atomic` envolvendo o corpo de `recalcular_fechamento_mes`; a declaração de `dia_pago.py:160` (“dentro da transação”) não é prova suficiente de atomicidade do caminho `on_commit`. `app/ponto/services/fechamento.py:448`, `:1311`, `:1807`.

**Competência exportada: premissa precisa ser corrigida**

- Hoje não há recusa genérica L-092 no escritor: para escopo exportado que escreve, ele apenas dá warning com motivo; `app/ponto/services/fechamento.py:539`–`:547`.
- Existe guarda temporária **específica de 09/2026**, declarada em `GUARDA_COMPETENCIA`: `app/ponto/services/fechamento.py:36`–`:43`.
- Guarda efetiva: quando competência guardada tem empresas exportadas, exclui os colaboradores dessas empresas da chamada e grava auditoria `recalculo_barrado_guarda`; leitura pura não monta `_exportadas`, logo passa: `app/ponto/services/fechamento.py:539`, `:560`–`:575`.
- Empresas exportadas são decididas por `marcos_da_competencia`, com escopo por empresa ou empresas dos colaboradores: `app/ponto/services/fechamento.py:1496`, `:1507`–`:1515`.
- A documentação registra L-113 revogando a recusa L-092 e mantendo versão anterior do TXT/trilha; isso confirma a mudança histórica e evita citar comentários antigos como comportamento atual: `app/docs/LEIS.md:153`. A L-092 original permanece documentada em `app/docs/LEIS.md:125`.

**Todos os callers de produção encontrados de `recalcular_por_evento`**

1. `_fechamento_por_evento`, chamado após criação de batida normal ou conflito revisado: `app/ponto/registro_batida.py:155`, `:160`, `:175`–`:178`.
2. Decisão HE após `_gravar`: `app/ponto/portas/he.py:181`, `:201`–`:202`.
3. Aceite de folga após seu ato: `app/chamados/services/validacao.py:354`–`:361`.
4. Validação de resposta, dentro do fluxo que grava validação e materializa: `app/chamados/services/validacao.py:435`–`:447`.
5. `_o_chao_do_dia_mudou`, após re-julgar dias tocados e deduplicar competências: `app/ponto/portas/celula.py:422`–`:458`.

**VOLTA e caminhos indiretos: não afirmar “nunca chama indiretamente”**

- A VOLTA grava célula e `EventoCelula` no seu atomic, depois chama `julgar_celula`, terminando sem chamada direta de recálculo: `app/ponto/portas/celula.py:785`–`:817`, `:822`.
- Evento é INSERT simples de outbox; nenhum recálculo ocorre no emissor: `app/ponto/portas/celula.py:896`–`:905`.
- Consumidor é configurado para cada minuto: `app/config/crons.py:393`–`:394`; comando chama `consumir_eventos_celula`: `app/chamados/management/commands/consumir_eventos_celula.py:26`–`:31`.
- Consumidor executa rejunção, retratação de perguntas/chamados, premissa morta, órfãs, lastro e fábrica, sem chamada explícita de `recalcular_por_evento`: `app/chamados/reconciliador.py:1363`–`:1387`.
- Alteração de status de chamado dispara re-julgamento pelo signal, sem recálculo explícito: `app/chamados/signals.py:187`–`:216`.
- **Há uma cascata condicional demonstrável até recálculo**:
  `VOLTA` → `EventoCelula` → consumidor → `retratar_perguntas_absolvidas_por_celula` → carimba perguntas e, se disputa puder fechar, `.fechar()` → `post_save` de disputa → retoma regularizações confirmadas → cria batida → `recalcular_por_evento`.
  Fontes de cada aresta: `app/ponto/portas/celula.py:796`; `app/chamados/reconciliador.py:1365`, `:592`–`:614`; `app/chamados/models.py:1932`–`:1936`; `app/chamados/signals.py:120`–`:144`; `app/chamados/models.py:4097`–`:4098`; `app/chamados/services/regularizacao_ext.py:218`–`:224`, `:70`–`:82`; `app/ponto/registro_batida.py:157`–`:160`, `:175`–`:178`.
- Essa cascata exige regularização `confirmada`, ainda não materializada, ausência de outra disputa aberta e criação efetiva de batida; portanto não garante recálculo da competência da VOLTA, pois a regularização pode ser de outro dia/competência: `app/chamados/services/regularizacao_ext.py:51`–`:65`, `:73`, `:218`–`:224`; `app/ponto/services/fechamento.py:1809`.
- O próprio julgamento também pode entrar nessa cascata antes do consumidor: quando há adiados pela lâmpada e `apply_`, `julgar_colab` chama `retratar_perguntas_absolvidas_por_celula`: `app/ponto/services/cartorio.py:962`–`:970`.
- Outro fechamento de disputa no consumidor ocorre por órfãs/lâmpada: `app/chamados/reconciliador.py:1170`–`:1172`, sujeito ao mesmo receiver de retomada em `app/chamados/signals.py:120`–`:144`.

**Cron não é reparador periódico garantido**

- Comentários de `recalcular_por_evento` ainda dizem que “cron da madrugada rega” (`app/ponto/services/fechamento.py:1785`, `:1794`, `:1820`), mas o catálogo atual declara que `recalcular_fechamento` **não tem cron de propósito**, por decisão do DP: `app/config/crons.py:1130`–`:1137`.
- `lavrar_dias_pagos` é ferramenta por ato/backfill, expressamente não rotina: `app/config/crons.py:1025`–`:1033`.

Conclusão para consolidar: falta à VOLTA um recálculo **direto e garantido** equivalente ao rabo da IDA; a afirmação absoluta de ausência de qualquer caminho indireto seria falsa pelo caminho condicionado de regularização externa. `app/ponto/portas/celula.py:445`–`:458`, `:785`–`:822`; `app/chamados/signals.py:120`–`:144`; `app/chamados/services/regularizacao_ext.py:70`–`:82`.

## 09/11 23:52 · sessao 92326474c659 · - · - · origem=Ronald (tmux)

**IDA — portas e pós-escrita**

- `regenerar_celulas_vinculo` faz a escrita em `transaction.atomic`; cria célula em `celula.py:258-262`; reescreve `trabalha`, `escala_geradora`, `dna`, `dna_anterior`, `regenerada_em`, `regeneracoes` em `celula.py:271-280`; marca cobranças órfãs em `celula.py:289`; trilha em `celula.py:331-351`. Fora desse atomic local, chama `_o_chao_do_dia_mudou` em `celula.py:354-356` — todos os caminhos abaixo são relativos a `app/ponto/portas/`.
- O rabo comum julga dias tocados não futuros e sem veredito humano, condicionado a `recalculo=True`, com exceções logadas em `celula.py:422-444`; deduplica competências por `janela_atual` e chama `recalcular_por_evento` em `celula.py:445-462`; agenda rejunção e reavaliação de falta espúria em `transaction.on_commit` em `celula.py:490-502`, mesmo com `recalculo=False`.
- `regenerar_celulas_dia` apenas delega com `[dia,dia]`, `estender_horizonte=False`, mantendo recálculo padrão em `celula.py:734-737`.
- `editar_dia` exige `autor.pk` e motivo em `celula.py:559-562`; guarda lavra em `celula.py:568-583`; usa atomic e `select_for_update` em `celula.py:592-609`; grava `trabalha/dna/escala_geradora/origem='editada'/editada_em/editada_por`; trilha obrigatória desfaz edição se não gravar em `celula.py:610-621`; marca órfãs em `celula.py:625-626`; chama mesmo rabo em `celula.py:627-628`.
- `trocar_plantao` envolve ambas `editar_dia` em atomic externo em `celula.py:652-675`; recusa de um lado desfaz par em `celula.py:676-677`; trilha obrigatória em `celula.py:685-693`. Portanto julgamento roda durante atomic externo, recálculo respeita seu próprio `on_commit`, e os eventos do rabo ficam para o commit externo — inferência dos pontos citados. [nao verificado]
- `gerar_celulas_periodo` pode criar células passadas ausentes em `celula.py:862-867`; passado existente apenas recebe DNA se nulo em `celula.py:870-875`; não tem rabo no retorno em `celula.py:887`. Command envolve gerador em atomic: `app/ponto/management/commands/gerar_celulas.py:309-311`.

**Chamadores IDA e exceções**

- `executar_vinculo` chama alias `_regen` com usuário request, motivo e lista `julgados`: `app/colaboradores/services/vinculo.py:311-321`; depois reconcilia sem julgar duas vezes: `:328-331`. Esse trecho está fora do atomic local do cadastro, iniciado em `:187` e já encerrado antes de `:298`.
- Signals de vigência e âncora chamam regeneração: `app/escala/signals.py:34-73` e `:79-119`; herdam transação de quem salvou o vínculo, sem criar um atomic externo próprio nesses corpos.
- Folgas delegam via `relavrar_celulas_da_foto`, inclusive `rejulgar` para mês que saiu do defeito: `app/escala/folgas.py:264-276`; lançamento manual envolve foto + regeneração no mesmo atomic: `:309-316`.
- Frota de template usa `recalculo=False`: `app/escala/services/cadastro_tipo.py:348-350`; compensa com `invalidar_fechamento`: `:357-359`. Comentário declara recálculo manual pendente: `:339-347`.
- Troca município atua desde hoje com `recalculo=False` e invalidação: `app/colaboradores/services/vinculo.py:903-905`. Flag de feriado usa padrão com recálculo: `:857-860`.
- Correção retroativa chama regeneração com motivo e possível abertura da lavra, depois reconcilia: `app/colaboradores/management/commands/corrigir_escala_retroativa.py:230-238`.
- `regenerar_em_exportada` exige staff e motivo ≥10: `app/ponto/portas/regen_exportada.py:104-114`; envolve regeneração + fechamento + retrato `DiaPago` em atomic: `:169-188`. É porta distinta da VOLTA.
- Escala auto aplica/desfaz somente futuro, em atomic: `app/escala/services/escala_auto_executor.py:96-105` e `:140-144`.
- Apply COL900 chama porta de dia: `app/ponto/management/commands/aplicar_col900.py:115-119`; ensaio usa a mesma porta dentro de mutação transacional desfeita: `app/ponto/management/commands/diff_col900.py:56-75`.

**VOLTA**

- Busca célula e recusa inexistente, humana ou sem anterior: `app/ponto/portas/celula.py:776-778`; guarda de lavra: `:779-782`.
- Atomic restaura `dna=dna_anterior`, limpa anterior, deriva `trabalha` por presença não nula de `marcos`, restaura vínculo quando `vinculo_id` existe, avança relógio e contador; `save` lista os seis campos: `:785-795`.
- Insere `EventoCelula` no mesmo atomic: `:796`; trilha `acao='regenerar_celulas_vinculo'`, DNAs antes/depois e `revertida=True`: `:798-804`; erro de trilha é engolido com log: `:805-808`.
- Depois do atomic local, re-julga dia não futuro e sem veredito humano: `:812-821`; retorna em `:822`. Corpo não chama `_o_chao_do_dia_mudou` nem `recalcular_por_evento`; ausência demonstrável pela leitura integral `:740-822`. Prova do restante do grafo indireto fica com a investigação de outbox/signals do agente principal.
- `julgar_celula` chama `julgar_colab` e aplicação de precedência: `app/ponto/services/cartorio.py:2000-2014`; não contém chamada direta de recálculo nesse corpo `:1971-2014`.

**Quem chama VOLTA e garantias**

- Único caller operacional encontrado na busca textual Python: command `app/ponto/management/commands/aplicar_col900.py:77-78`; nenhum caller de view encontrado [não verificado como garantia absoluta].
- Command exige motivo ≥20 caracteres e colaborador em `:54-58`; sem `--aplicar` apenas imprime simulação em `:74-76`; chamada omite autor em `:77-78`. Não existe cheque staff no caminho command nem na função VOLTA (`app/ponto/portas/celula.py:740-822`); restrição operacional do ambiente/CLI [não verificado].
- Autor e motivo são opcionais na função, `:740`; trilha aceita ator nulo e motivo default `'porta'`, `:799-804`; não há obrigação equivalente à edição humana nem rollback por trilha ausente, `:805-808`.

**Testes e mutante** [nao verificado]

- IDA possui teste real que verifica `ata=480` e `DiaPago.minutos_previstos=480`: `app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:88-95`; simula `on_commit` imediato em `:39-41` e `:84-85`.
- R6 mocka recálculo em `app/ponto/tests/test_r6_dinheiro_acompanha_cadastro.py:62-66`; cobra competências, representante, `False` e no-op em `:77-122`.
- VOLTA testa restauração DNA/trabalha, idempotência, contador/trilha, exportada e mão humana em `app/ponto/tests/test_porta_celula_dia.py:180-255`; fixture original é futura em `:53-67`.
- Teste do passado só mocka juiz e verifica argumentos: `:258-273`; futuro/humano em `:275-282`. Não verifica `DiaPago`/`FechamentoMensal` nesse arquivo (`:1-282`). [nao verificado]
- AST só exige evento no atomic: `app/chamados/tests/test_e4_expand.py:284-293`.
- Não encontrei teste ida→volta verificando gravado por dia [não verificado como ausência absoluta]. O mutante “VOLTA restaura DNA/ata, emite evento, mas deixa DiaPago anterior” corresponde ao corpo atual `app/ponto/portas/celula.py:785-822` e não é detectado pelas asserções citadas; execução de mutante não realizada [não verificado].

## 09/11 23:52 · sessao c141c4b76882 · - · - · origem=Ronald (tmux)

Censo de leitores/testes, somente inspeção estática; não executei testes nem mutantes. [não verificado] [nao verificado]

- **O previsto persistido existe:** `DiaPago.CAMPOS_DIA` inclui `minutos_previstos`; `CAMPOS_GRADE` também. A soma da competência agrega todos os `CAMPOS`, portanto inclui esse previsto (`app/ponto/services/dia_pago.py:34-47`, `:550-563`).
- **Tela do espelho:** lê as linhas de `DiaPago` do período (`app/ponto/services/espelho.py:406-412`) e entrega `dia_pago`, presença da lavratura, `pago_h` e rubricas (`:805-813`). Essas rubricas leem dinheiro persistido, sem usar `minutos_previstos` (`app/ponto/services/dia_pago.py:914-928`). A grade visual vem de `grade_da_celula` (`app/ponto/services/espelho.py:506-520`) e `previsto_canon` nasce dessa grade (`:673-687`, `:771`).
- **PDF, linhas por dia:** usa `dia['rubricas']` e `dia['pago_h']` da mesma montagem do espelho; logo o dinheiro diário pode continuar velho após a volta (`app/relatorios/pdf_espelho.py:837-839`; inferência a partir de `app/ponto/services/espelho.py:805-813`). A presença das colunas considera tanto rubrica lavrada quanto motor (`app/relatorios/pdf_espelho.py:855-859`).
- **App, horas diárias:** busca a linha paga e chama `horas_do_dia`; publica `horas_trabalhadas` e `dia_pago_lavrada` (`app/api/views.py:1554-1573`). `horas_do_dia` prioriza qualquer linha existente, inclusive zero; só consulta o período do motor quando não existe linha (`app/ponto/services/espelho.py:132-136`).
- **Relatório de horas:** soma `DiaPago` por período, em vez de recalcular dinheiro (`app/relatorios/views.py:481-487`); os campos diários disponíveis incluem o previsto (`app/ponto/services/dia_pago.py:34-47`).

**Cuidado central: totais e TXT podem esconder a divergência persistida.**

- Totais de espelho/PDF/app passam por `folha_manda` (`app/ponto/services/espelho.py:1283-1284`, `app/relatorios/pdf_espelho.py:628`, `app/api/views.py:1678-1685`).
- Havendo `FechamentoMensal`, `totais_da_folha` usa apenas sua existência; o número vem de `fechamento_lido` (`app/relatorios/cartao_pela_celula.py:69-76`). Esse wrapper chama o motor com `somente_leitura=True, retornar_objeto=True`, sem persistir (`app/ponto/services/fechamento.py:1759-1761`; ausência de escrita no modo: `:1191-1192`).
- O previsto online soma os minutos da grade e os atribui ao objeto de fechamento (`app/ponto/services/fechamento.py:1046-1049`, `:1083-1086`). A grade prioriza `ata.minutos_previstos`; só deriva pela autoridade quando esse campo está ausente (`app/escala/services/leitor_celula.py:399-412`). Assim a VOLTA que atualiza a ata pode dar previsto correto no resumo online enquanto o `DiaPago` continua velho: inferência desse caminho (`app/relatorios/cartao_pela_celula.py:69-76`, `app/ponto/services/fechamento.py:1048`, `app/escala/services/leitor_celula.py:399`).
- Sem fechamento, `folha_manda` cai explicitamente na soma da lavratura (`app/relatorios/cartao_pela_celula.py:340-346`). Nesse ramo `horas_previstas` vem de `DiaPago.minutos_previstos`, salvo soma zero, que vira `None` e preserva a resposta anterior do motor (`:141-144`, `:168-172`, `:192`, `:205-211`).
- O coletor PDF também calcula previsto diretamente por `minutos_previstos_do_dia` antes de `folha_manda` (`app/relatorios/pdf_espelho.py:456-457`, `:628`); esse juiz lê `esc.marcos_do_dia` e desconta pausas (`app/escala/utils.py:2268-2269`, `:2291`, `:2305-2320`).
- **TXT efetivo hoje:** a view deriva fechamentos online com `somente_leitura=True, retornar_objeto=True`, classifica esses objetos e passa os mesmos à montagem (`app/folha/views.py:207-212`, `:265-268`). Logo não é correto afirmar que o arquivo gerado necessariamente repete os números velhos gravados (inferência desse fluxo). O helper sem injeção ainda lê `FechamentoMensal` persistido (`app/folha/export.py:709-724`), e `classificar_export` sem injeção também (`:775-780`).
- **Prontidão da lista de fechamento:** o serviço da lista chama `prontidao` (`app/ponto/services/fechamento_lista.py:85`). Se `previsto_em` está preenchido e `minutos_previstos` não é nulo, usa esse snapshot (`app/folha/export.py:1037-1043`); caso contrário lê a grade e soma o previsto (`:1087-1090`). Isso constitui outro consumidor potencial do mensal velho, condicionado ao carimbo (`:1037-1043`).

**Testes e mutante:** [nao verificado]

- Há teste forte da **IDA**: simula `on_commit` imediato (`app/ponto/tests/test_ordem_regen_julga_antes_do_dinheiro.py:84-85`), exige ata nova = 480 e `DiaPago.minutos_previstos = 480` (`:88-95`). Também garante nenhuma repetição de julgar/recalcular em ida idempotente (`:97-106`).
- Os testes encontrados da **VOLTA** verificam DNA/trabalha (`app/ponto/tests/test_porta_celula_dia.py:180-192`), idempotência (`:194-206`), trilha (`:208-224`), guarda exportada (`:226-240`), exclusão de mão humana (`:242-251`), julgamento de passado e exclusão de futuro/humano (`:257-282`). Nenhuma dessas asserções lê `DiaPago` ou `FechamentoMensal` (`:180-282`).
- O teste AST da volta exige apenas `emitir_evento_celula(cel)` dentro do `atomic` (`app/chamados/tests/test_e4_expand.py:284-293`).
- Portanto o mutante **“VOLTA restaura DNA, re-julga, emite evento, mas omite recálculo financeiro”** satisfaz os requisitos visíveis desses testes da volta; passagem real da suíte não foi executada. Inferência estática: `app/ponto/tests/test_porta_celula_dia.py:180-282`, `app/chamados/tests/test_e4_expand.py:284-293`; execução [não verificado].
- Existem testes de edição humana/troca lendo o persistido por dia: helper lê `DiaPago.minutos_previstos`, `furo_apurado` e descontos (`app/ponto/tests/test_o10b_editar_dia_troca.py:51-65`); G01/G02 exigem a lavratura limpa após a troca (`:145-167`). Eles não chamam a VOLTA de regeneração (`:145-167`).

## 09/11 23:51 · sessao 5adea6f75cea · FIO-E0-V4-E4A-2 · censo · origem=Code

Nos anexos, `:N` conta linhas desde a primeira linha do conteúdo, excluindo o cabeçalho `== anexo… ==`.

**Quem perguntou:** a sessão perguntou explicitamente por leitores/rc, efeitos da tupla vazia e pré-requisito de pouso; também exigiu RED na base. Esses temas não sustentam **(e) NÃO PERGUNTADO**. (`preverif1.q:11–16`, `preverif1.q:29–32`, `construtor1.q:10`) [nao verificado]

**Quem triou:** a triagem reconheceu “vigia falhou” e a medição explícita pós-pouso como responsabilidade da **SESSÃO, `!`**. Essa linha é encaminhamento, não descarte. O descarte final aparece apenas no resumo “todos fila”; seu motivo individual não foi fornecido. (`triagem_preverif1.txt:4`, `opus_final1.txt:7`; motivo: **[nao verificado]**) [nao verificado]

| Achado | Classe | Trecho literal da .q | Trecho literal da .r | Trecho literal da triagem |
|---|---|---|---|---|
| **1 — zero medido antes do C11** | **(c)** — cronologia concreta fora do alcance; a ausência da prova foi apontada. (`censo1.r:3`, `opus_final1.txt:2`) | “pouso antes do” / “pre-requisito (CORTES.md:305)” (`preverif1.q:30–31`) | “o dado fornecido não comprova uma rodada posterior à implantação com **FIO MUDO 0**.” (`censo1.r:3`) | “2 PROCEDENTE -> SESSAO, `!`” para o selo e a pergunta de escopo. Não documenta a comparação **06:35 × 20:48**; descarte específico dessa cronologia: **[nao verificado]**. (`triagem_preverif1.txt:3`) | [nao verificado]
| **2 — FIO MUDO sem canal operacional de alarme** | **(a), parcial** — falhou a conclusão integrada sobre o canal de alarme; várias peças foram vistas. (`preverif1.r:3`, `preverif1.r:13`, `preverif1.r:19`) | “o rc do comando (0/2) e as linhas de saida (FABRICA-POR-EVENTO, VIGIA FIO MUDO, ALVOS, [BARRADO])”; “Todos supoem o MESMO significado” (`preverif1.q:12–13`) | “CONFORME para rc e dependência”; para FIO MUDO: “Leitor operacional específico: [nao verificado].”; “O pipeline também continua calculando esperados somente de `CRONS`, deixando ENCs fora desse universo”. As peças aparecem, mas a conclusão bloqueante não foi formulada. (`preverif1.r:3`, `preverif1.r:13`, `preverif1.r:19`) | Descarte específico da ausência de canal: **[nao verificado]**. “placar_code.sh publica ‘perguntas’ para unidade mista” trata de outro defeito, não prova descarte deste. (`triagem_preverif2.txt:5`) |
| **3 — tupla vazia produz “vigia falhou”** | **(b)** — visto e assumido pela sessão; descarte final reportado pelo opus, com motivo individual **[nao verificado]**. (`triagem_preverif1.txt:4`, `opus_final1.txt:7`) | “Algum leitor de JUIZES_POR_VARREDURA quebra ou MUDA DE VEREDITO com a tupla vazia” (`preverif1.q:15`) | “Vazio saudável vira ‘vigia falhou’.”; “Correção mínima: emitir representação explícita do universo vazio, distinguindo-o de erro de execução”. (`preverif1.r:25`; aspas internas normalizadas) | “3 PROCEDENTE -> SESSAO, `!` (junto do 2): bin/placar_code.sh:205-209 le universo vazio como ‘vigia falhou’”. **Motivo registrado nesse encaminhamento:** vínculo com o item 2, de escopo da sessão. Motivo posterior para mandar à fila: **[nao verificado]**. (`triagem_preverif1.txt:4`; aspas normalizadas) |
| **4 — prova pós-deploy por nome explícito em PROD** | **(c)** — exigência percebida; execução em PROD fora do alcance. (`preverif1.r:26`, `triagem_preverif1.txt:4`) | A pergunta inclui expressamente “vigia_loop.escritas_por_cron” entre os leitores afetados pela retirada. (`preverif1.q:16`) | “Correção mínima: preservar uma medição explícita do comando durante a verificação da retirada”. (`preverif1.r:26`) | “e o pos-pouso mede escritas_por_cron(['disparar_perguntas_competencia']) com o nome explicito.” A triagem acolheu a exigência; resultado da execução: **[nao verificado]**. (`triagem_preverif1.txt:4`) | [nao verificado]
| **5 — RED dos testes Django invertidos sem evidência na base em log** | **(c)** — execução Django/RED em log fora do alcance declarado; não houve alegação de execução ORM bem-sucedida. (`construtor1.r:43`) | “Mostre o RED (rodando na base) antes da cura.”; “Inverter (nao apagar) os testes que fixam a escrita”. (`construtor1.q:10`, `construtor1.q:12`) | “A bateria Django abaixo foi tentada, mas nenhum módulo iniciou”; “execução ORM e serialização via `call_command` ficaram **[nao verificado]**.” (`construtor1.r:43`) | Triagem específica da ausência desse RED Django: **[nao verificado]**. O item 4 encaminhado à volta 3 trata dos escapes do coletor SQL e de callbacks, não comprova RED dos testes invertidos na base. (`triagem_preverif2.txt:3`) | [nao verificado]

A atribuição de **(a)** ao achado 2 é restrita à conclusão sobre a eficácia do alarme: seria incorreto dizer que o Codex não viu nenhum dos mecanismos. A cópia confirma que o placar raspa `FABRICA-POR-EVENTO`, que o pipeline percorre `CRONS` e que rc 2 é aceito como não-quebra. (`bin/placar_code.sh:388`, `app/core/management/commands/pipeline_placar.py:40`, `app/core/services/crons_quebrados.py:16`)

No achado 1, os horários são evidência fornecida pelo opus raspado, sem confirmação independente. A exigência de medir zero **com C11 no ar** consta da própria cópia. (`opus_final1.txt:2`, `app/docs/RELATO.md:26`)

## 09/11 23:49 · sessao 9e374e71ccea · - · - · origem=Ronald (tmux)

A sessão perguntou os temas dos cinco achados: leitores/rc e lista vazia em `preverif1.q:11-18`, pré-requisito em `preverif1.q:29-32`, e RED na base em `construtor1.q:4-11`. Portanto, “não perguntado” não explica esses temas. As linhas dos anexos aqui contam desde a primeira linha do conteúdo, excluindo `== anexo ... ==`.

| Achado | Classificação sustentada | Trechos literais e limites |
|---|---|---|
| 1 | **(c)** para a cronologia concreta; condição já percebida | `.q`: “pouso antes do pre-requisito (CORTES.md:305)” (`preverif1.q:30-31`). `.r`: “A condição exige medição do FIO MUDO e C11 pousado; o registro exige medição com C11 no ar.” e “apresentar essa evidência antes de liberar pouso” (`preverif1.r`, linha física [nao verificado]). A resposta não tinha prova do horário; não é silêncio sobre o requisito. `triagem_preverif1.txt:3` encaminha o selo à **SESSÃO, `!`**, sem demonstrar descarte da cronologia. | [nao verificado]
| 2 | **Ambiguidade entre (a) da conclusão integrada e fatos já vistos; descarte específico [nao verificado]** | `.q`: “QUEM ESCREVE e QUEM LE [...] rc [...] linhas de saida [...] Todos supoem o MESMO significado” (`preverif1.q:11-14`). `.r`: “Leitor operacional específico: [nao verificado]” para VIGIA FIO; “O pipeline também continua calculando esperados somente de `CRONS`, deixando ENCs fora desse universo” (`preverif1.r`, linhas físicas [nao verificado]). O censo também descreveu o placar raspando somente FABRICA, saúde aceitando 0/2 e pipeline CRONS versus ENC. Logo, dizer que ninguém viu esses mecanismos seria falso; porém não foi formulado o achado integrado L-062. `triagem_preverif2.txt:5` descarta **unidade mista**, não ausência de leitor/alarme. |
| 3 | **(b)** como falha de encaminhamento/fechamento da sessão, com ressalva sobre “descarte” | `.q`: “Algum leitor de JUIZES_POR_VARREDURA quebra [...] com a tupla vazia” (`preverif1.q:15-16`). `.r`: “Vazio saudável vira ‘vigia falhou’” e “Correção mínima: emitir representação explícita do universo vazio” (`preverif1.r`, linha física [nao verificado]). Triagem literal: “3 PROCEDENTE -> SESSAO, `!` [...] le universo vazio como ‘vigia falhou’” (`triagem_preverif1.txt:4`). Isso é reconhecimento e atribuição à sessão, **não descarte literal**. O único resumo final de descarte fornecido é “Descartes da triagem: todos fila.” (`opus_final1.txt:7`); motivo individual do descarte [nao verificado]. |
| 4 | **(c)**: prova em PROD | `.q`: censo pede quem conta a lista e leitores (`censo1.q:9,13`). `.r`: “preservar uma medição explícita do comando durante a verificação da retirada” (`preverif1.r`, linha física [nao verificado]). Triagem: “e o pos-pouso mede escritas_por_cron(['disparar_perguntas_competencia']) com o nome explicito” (`triagem_preverif1.txt:4`). A exigência foi vista e acolhida; execução em PROD não está demonstrada. | [nao verificado]
| 5 | **(c)**: RED Django em log/base | `.q`: “Mostre o RED (rodando na base) antes da cura.” (`construtor1.q:11`, confirmar contagem física [nao verificado]). `.r`: “A bateria Django abaixo foi tentada, mas nenhum módulo iniciou: faltou Django”; “execução ORM [...] [nao verificado]” (`construtor1.r`, linhas físicas [nao verificado]). Isso distingue RED de fonte alegado de RED Django não executado. Triagem específica do RED Django em log [nao verificado]. |

Cuidados na consolidação:

- `triagem_preverif2.txt:7` descarta **frescor pela meia-noite do host**; não comprova descarte da medição de 06:35 anterior ao C11.
- `triagem_preverif2.txt:5` descarta **unidade “perguntas” versus pauta**; não comprova descarte de L-062.
- Não há suporte para **(d) NENHUM viu**: os temas e limitações aparecem nas respostas acima.
- O achado 2 exige critério explícito: os mecanismos foram vistos; a conclusão integrada não foi registrada. Atribuir todo ele a (a) ou todo ele a (b), sem essa distinção, excede os anexos. [nao verificado]

## 09/11 23:48 · sessao a8631007fb8c · O169-FECHADA · censo · origem=Code

A premissa está registrada como **L-125**, e a cura da C20-O169 está presente no serviço. Porém, “nunca fecha com premissa viva” **não vale como garantia global**: existem fechamentos genéricos que alcançam cluster sem consultar sua assinatura. Além disso, o banco atual não distingue causalmente fechamento por idade de fechamento por lastro. (`app/docs/LEIS.md:146`; `app/ponto/services/cluster_espurio.py:53`; `app/chamados/models.py:727`; `app/chamados/models.py:292`)

Censo estático. Execução atual da suíte, execução dos mutantes, ocorrências concretas no banco e consumidores externos ao checkout: **[nao verificado]**. [nao verificado]

**1. Caminhos que encerram ou fazem desaparecer**

O cluster canônico nasce como `modulo_origem='cluster_batida_espuria'`, categoria `batida_espuria`. Ele não pertence ao universo normal de furo; portanto, a guarda genérica de premissa de `reconciliar()` retorna autorização sem consultar `FATO_CLUSTER_ESPURIO`. A guarda de perguntas vivas continua podendo impedir o encerramento. (`app/ponto/services/cluster_espurio.py:11`; `app/ponto/services/cluster_espurio.py:132`; `app/ponto/services/cluster_espurio.py:135`; `app/chamados/models.py:1244`; `app/chamados/catalogo/modulos.py:235`; `app/chamados/models.py:727`; `app/chamados/models.py:685`)

| Caminho | Condição e dependência temporal |
|---|---|
| **Serviço específico** | Seleciona todos os chamados vivos do módulo/colaborador e fecha somente os cuja célula do próprio dia demonstra premissa morta. Não recebe horizonte no seletor de fechamento. Conta apenas retorno verdadeiro de `reconciliar()`. (`app/ponto/services/cluster_espurio.py:53`; `app/ponto/services/cluster_espurio.py:66`; `app/ponto/services/cluster_espurio.py:71`; `app/ponto/services/cluster_espurio.py:76`; `app/ponto/services/cluster_espurio.py:123`) |
| **Evento do cartório** | Com `apply_` e células julgadas, chama o serviço específico. O serviço exige colaborador ativo e escala ativa sem término; esse recorte limita quem é examinado, mas não impõe idade aos chamados examinados. (`app/ponto/services/cartorio.py:992`; `app/ponto/services/cartorio.py:1001`; `app/ponto/services/cartorio.py:1005`; `app/ponto/services/cluster_espurio.py:117`) |
| **Detector manual** | `--apply` somente produz escrita sem `--alarme`; delega ao mesmo serviço. O cron declarado usa `--alarme`, portanto não fecha. (`app/ponto/management/commands/detectar_cluster_espurio.py:34`; `app/ponto/management/commands/detectar_cluster_espurio.py:46`; `app/config/crons.py:598`) |
| **Transição humana: resposta livre e fio** | Gestor informa status válido; chama `transicionar()`, sem condição de idade ou assinatura cluster. Há autorização contextual e guardas de justificativa/disputa/pergunta. (`app/chamados/services/acoes_chamado.py:313`; `app/chamados/services/fio.py:711`; `app/chamados/services/fio.py:714`; `app/chamados/services/fio.py:719`; `app/chamados/models.py:135`; `app/chamados/models.py:148`) |
| **Outras ações HTTP** | Aprovar exceção e autorizar geofence também transicionam para resolvido sem filtro de origem nesses caminhos. Exceção usa `criado_em` para montar a data da batida, não como prescrição. Recusar geofence exige batida vinculada ao contexto; o contexto normal do cluster não fornece essa ligação. (`app/chamados/views.py:426`; `app/chamados/views.py:540`; `app/chamados/services/acoes_chamado.py:36`; `app/chamados/services/acoes_chamado.py:70`; `app/chamados/services/acoes_chamado.py:222`; `app/chamados/services/acoes_chamado.py:257`; `app/chamados/services/acoes_chamado.py:277`; `app/ponto/services/cluster_espurio.py:145`) |
| **Decisão administrativa do dia** | Busca todos os vivos por `data_turno` ou FK da célula e chama `reconciliar()`. A data seleciona o dia decidido; não há teto de idade nem consulta à assinatura cluster nesse fechamento. (`app/ponto/services/veredito_celula.py:148`; `app/ponto/services/veredito_celula.py:155`; `app/ponto/services/veredito_celula.py:201`) |
| **Competência trancada** | Parte dos vivos da empresa, seleciona pela competência e aplica exceções; encerra pela via `competencia_trancada`, sem consultar assinatura cluster. Depende da competência/data, não de `--dias`. (`app/chamados/services/competencia_trancada.py:82`; `app/chamados/services/competencia_trancada.py:88`; `app/chamados/services/competencia_trancada.py:96`; `app/chamados/services/competencia_trancada.py:135`; `app/chamados/services/competencia_trancada.py:184`; `app/chamados/models.py:1639`) |
| **Desligamento** | Todos os `NAO_FECHADOS` do colaborador recebem `encerrar()`, sujeito à guarda de perguntas. A data de demissão determina quando executar, sem idade do chamado. O cron efetiva demissões cuja data chegou. (`app/colaboradores/services/desligamento.py:150`; `app/colaboradores/services/desligamento.py:155`; `app/chamados/models.py:1614`; `app/chamados/models.py:1617`; `app/colaboradores/management/commands/efetivar_desligamentos_agendados.py:37`; `app/config/crons.py:440`) |
| **Signal de disputa fechada** | Disputa existente com `fechada_em` e chamado-pai ainda não encerrado chama `reconciliar()`, sem idade. Cluster como pai nessa FK no banco: **[nao verificado]**. (`app/chamados/signals.py:28`; `app/chamados/signals.py:33`; `app/chamados/signals.py:42`) |
| **Justificativa aprovada** | Origem viva apontada por `chamado_origem` recebe `retratar()`, sem filtro de módulo/idade nesse sítio. Cluster nessa FK: **[nao verificado]**. (`app/ponto/services/justificativa.py:33`; `app/ponto/services/justificativa.py:42`; `app/ponto/services/justificativa.py:46`) |
| **Materialização** | Pode reconciliar destino e clones por FK/IDs, protegendo irmãs respondidas sem consumo; há também fechamento de destino quando ausência já foi decidida. Cluster nessas ligações: **[nao verificado]**. (`app/chamados/services/materializacao.py:581`; `app/chamados/services/materializacao.py:761`; `app/chamados/services/materializacao.py:771`; `app/chamados/services/materializacao.py:773`) |
| **Regularização externa** | Chama `reg.chamado.encerrar()` por FK. O fluxo normal cria origem `regularizacao_externa`; ligação divergente com cluster: **[nao verificado]**. (`app/chamados/services/regularizacao_ext.py:201`; `app/chamados/services/regularizacao_ext.py:205`; `app/chamados/services/regularizacao_ext.py:246`) |
| **`retratar_chamados_lei_cala` manual** | Aceita `--modulo` livre. Com cluster, seleciona vivos cujo dia é anterior a hoje e cuja classificação de falta não acusa falta; `--apply` chama `retratar()`, sem consultar assinatura cluster. Coexistência concreta dessas condições com assinatura viva: **[nao verificado]**. (`app/chamados/management/commands/retratar_chamados_lei_cala.py:24`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:33`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:40`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:45`; `app/chamados/management/commands/retratar_chamados_lei_cala.py:59`) |
| **`marcar_feriado_do_posto` manual** | Parte da fila, seleciona pelo vínculo do dia e feriado alcançado, e reconcilia ao declarar posto fechado. Carrega calendário de hoje−180 até hoje: existe dependência temporal indireta no universo consultado. Não consulta assinatura cluster nesse fechamento. (`app/colaboradores/management/commands/marcar_feriado_do_posto.py:92`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:102`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:108`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:114`; `app/colaboradores/management/commands/marcar_feriado_do_posto.py:140`) |

A tranca é disparada pelo ato de fechamento do período, diretamente ou por subprocesso `encerrar_por_competencia_trancada --apply`; não é um cron por desenho. (`app/ponto/services/fechamento.py:1420`; `app/ponto/services/fechamento.py:1428`; `app/chamados/services/competencia_trancada.py:281`; `app/chamados/services/competencia_trancada.py:290`; `app/config/crons.py:1273`)

Há também caminhos que **retiram da exibição sem encerrar**:

| Caminho | Condição temporal |
|---|---|
| Arquivo individual | POST autorizado; não exige idade, embora o motivo padrão diga “Silenciado por idade (30d)”. (`app/chamados/views_arquivar.py:24`; `app/chamados/views_arquivar.py:35`; `app/chamados/views_arquivar.py:37`) |
| Arquivo em lote | Exige `dias>=1`, seleciona por verbo/área e `criado_em < agora-dias`; silencia. (`app/chamados/views_arquivar.py:102`; `app/chamados/views_arquivar.py:125`; `app/chamados/views_arquivar.py:135`; `app/chamados/views_arquivar.py:140`) |
| `arquivar_prescritos` | Mesmo corte de idade por criação, default 30 dias, sem véu/resgate; motivo “Prescrito por idade”. Cron declarado desligado. (`app/chamados/management/commands/arquivar_prescritos.py:32`; `app/chamados/management/commands/arquivar_prescritos.py:53`; `app/chamados/management/commands/arquivar_prescritos.py:85`; `app/config/crons.py:1450`) |
| Competência encerrada | Vivos sem véu, dia do fato até `--corte`, sem disputa viva; silencia por dez anos. É ato manual. (`app/chamados/management/commands/arquivar_competencia_encerrada.py:100`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:112`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:121`; `app/chamados/management/commands/arquivar_competencia_encerrada.py:274`; `app/config/crons.py:1356`) |
| Sem dia irrecuperável | Escopo DIA, sem dia recuperável, criação anterior à competência corrente; silencia por cinco anos. (`app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:51`; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:58`; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:62`; `app/chamados/management/commands/arquivar_sem_dia_irrecuperavel.py:88`) |
| Isentos | Todos os vivos de isento sem véu, sem corte de idade; signals acionam o serviço. (`app/chamados/services/silencio_isentos.py:24`; `app/chamados/services/silencio_isentos.py:27`; `app/chamados/services/silencio_isentos.py:43`; `app/chamados/signals.py:274`; `app/chamados/signals.py:301`) |

Esses silêncios não escrevem `status_local`. O painel exclui silêncio ainda vigente; a partição de verbos também classifica silêncio vencido como histórico, fora da lista padrão. (`app/chamados/services/arquivo.py:154`; `app/chamados/services/arquivo.py:165`; `app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:18`; `app/chamados/services/verbos.py:48`)

Exclusão física encontrada: reset de **todos** os chamados, condicionado a banco de ensaio e confirmação; a porta recusa produção pela flag/nome do banco. Outros apagadores de cluster em produção: **[nao verificado]**. (`app/ponto/management/commands/zerar_dados_operacionais.py:71`; `app/ponto/management/commands/zerar_dados_operacionais.py:84`; `app/chamados/models.py:563`; `app/chamados/models.py:571`; `app/ponto/registro_batida.py:326`)

Nos caminhos examinados, cartório interno/grade/lastro excluem cluster pelos universos de furo; o reconciliador genérico de premissa não coleta seu fato e, portanto, conserva. (`app/ponto/services/cartorio.py:117`; `app/ponto/services/cartorio.py:1280`; `app/ponto/management/commands/reconciliar_grade.py:207`; `app/ponto/services/lastro.py:387`; `app/chamados/reconciliador.py:262`; `app/chamados/reconciliador.py:301`; `app/chamados/catalogo/premissa.py:127`)

**2. Entrada de `dias`, `ref`, `hoje` e horizonte**

- O comando define `--dias=14`, obtém `ref=timezone.localdate()` e passa ambos a `pendencias()` ou `reconciliar_colab()`. Seu universo de colaboradores vem de escalas ativas sem término e colaboradores ativos, opcionalmente `--colab`. (`app/ponto/management/commands/detectar_cluster_espurio.py:29`; `app/ponto/management/commands/detectar_cluster_espurio.py:35`; `app/ponto/management/commands/detectar_cluster_espurio.py:36`; `app/ponto/management/commands/detectar_cluster_espurio.py:46`; `app/ponto/management/commands/detectar_cluster_espurio.py:50`)
- `dias_com_cluster()` consulta células entre `ref-(dias-1)` e `ref`, inclusivamente. Esse resultado alimenta achados, lista e abertura. (`app/ponto/services/cluster_espurio.py:44`; `app/ponto/services/cluster_espurio.py:46`; `app/ponto/services/cluster_espurio.py:100`; `app/ponto/services/cluster_espurio.py:107`)
- O fechamento chama `_vivos_que_a_premissa_matou(colab)`, sem `dias`, `ref` ou `hoje`; lê todos os vivos e consulta suas próprias datas. Sem dia forte recuperável, o chamado nem entra nos candidatos. (`app/ponto/services/cluster_espurio.py:53`; `app/ponto/services/cluster_espurio.py:66`; `app/ponto/services/cluster_espurio.py:68`; `app/ponto/services/cluster_espurio.py:71`; `app/ponto/services/cluster_espurio.py:108`)
- Assinatura presente conserva; célula/insumos ausentes conservam como `None`; medição sem assinatura permite fechar. O juiz de dia sem trabalho prevalece sobre a ausência de insumos. (`app/ponto/services/cluster_espurio.py:25`; `app/ponto/services/cluster_espurio.py:29`; `app/ponto/services/cluster_espurio.py:33`; `app/ponto/services/cluster_espurio.py:74`; `app/chamados/catalogo/premissa.py:127`)
- `agora`/julgamento anterior participam de `na_lista`, usado na abertura; não entram em `a_fechar`. (`app/ponto/services/cluster_espurio.py:101`; `app/ponto/services/cluster_espurio.py:104`; `app/ponto/services/cluster_espurio.py:107`; `app/ponto/services/cluster_espurio.py:108`)

**Resposta direta:** um dia de 15+ dias com assinatura viva não fecha pelo seletor específico. Pode encerrar por caminhos genéricos, como transição humana, desligamento ou tranca; esses caminhos não impõem uma consulta à assinatura cluster. Ocorrência concreta hoje no banco: **[nao verificado]**. (`app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:76`; `app/chamados/services/acoes_chamado.py:315`; `app/colaboradores/services/desligamento.py:155`; `app/chamados/models.py:1639`)

**3. RED da C20-O169 e mutante**

O teste é `ClusterNaoFechaPorIdadeTest.test_RED_dia_de_15_dias_com_assinatura_NAO_fecha_por_idade`, em `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:74`.

Ele cria chamado para **dia do fato** `hoje−15`, com ata assinada; consulta `pendencias`, executa `reconciliar_colab` e recarrega o estado. Afirma que o chamado não está em `a_fechar` e continua em `VIVOS`. Não envelhece `criado_em`. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:43`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:56`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:65`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:75`)

O mutante **“incluir em `a_fechar` todo chamado cujo dia antecede `hoje-(dias-1)`”** fica vermelho na asserção que exige ausência da lista, mesmo que um gate posterior impeça a escrita. Se fechar pelo serviço, também viola a asserção de sobrevivência. Essa é uma conclusão estática; execução efetiva do mutante: **[nao verificado]**. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:59`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:76`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:77`)

Limites do selo:

- Mutante exclusivo no `Command.handle` não é exercitado por esse teste, que chama o serviço diretamente. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:59`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:60`; `app/ponto/management/commands/detectar_cluster_espurio.py:45`)
- Mutante baseado na idade de `criado_em` não encontra uma criação envelhecida nessa fixture. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:43`; `app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:65`)
- Existe contraprova: dia de 15 dias **sem** assinatura deve fechar, impedindo uma cura que conserve tudo fora do horizonte. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:80`)

O pouso de 05/11 e o RED histórico de três falhas estão documentados; os logs originais não foram confirmados nesta leitura. (`app/docs/BACKLOG.md:1790`; `app/docs/RELATO.md:7243`; logs originais: **[nao verificado]**)

**4. Definição de `fechados_por_idade` pela função real**

**Não existe discriminador causal suficiente no fechamento atual.** `reconciliar()` grava via `sistema`; o núcleo carimba estado/data/via, e `estado_chamado` registra somente status anterior, novo e via. Um mutante que selecione pela idade e chame o mesmo gate produz esses mesmos registros. (`app/chamados/models.py:52`; `app/chamados/models.py:54`; `app/chamados/models.py:60`; `app/chamados/models.py:242`; `app/chamados/models.py:292`; `app/chamados/models.py:654`; `app/chamados/catalogo/motor.py:95`)

O contador atual mede **fechamentos efetivos**, usando o retorno real de `ch.reconciliar()`. A decisão de premissa existe antes dele, mas não é persistida como motivo do fechamento. Logo, esse contador não permite separar idade de lastro retrospectivamente. (`app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:76`; `app/ponto/services/cluster_espurio.py:123`; `app/chamados/models.py:292`)

Também não basta contar encerrados fora do horizonte: o teste exige fechamento legítimo por lastro aos 15 dias. Consultar hoje a assinatura de um encerrado identifica uma inconsistência presente, mas não prova o fato existente no instante do encerramento. (`app/ponto/tests/test_o169_cluster_nao_fecha_por_idade.py:80`; `app/ponto/services/cluster_espurio.py:15`; `app/chamados/models.py:292`)

Há uma cegueira adicional para um vigia baseado somente em `pendencias`: o fechamento examina apenas `VIVOS`, enquanto `_sem_chamado` considera existente um chamado de qualquer estado. Um encerrado indevidamente pode deixar de aparecer tanto em `a_fechar` quanto em `a_abrir`. (`app/ponto/services/cluster_espurio.py:66`; `app/ponto/services/cluster_espurio.py:81`; `app/ponto/services/cluster_espurio.py:107`)

Para que o número represente **causa histórica**, falta registrar no ato a decisão produzida pela autoridade real; imprimir zero apenas porque o seletor atual não usa idade não constitui medição independente contra o mutante. Essa conclusão decorre da diferença entre a decisão calculada e a trilha gravada. (`app/ponto/services/cluster_espurio.py:74`; `app/ponto/services/cluster_espurio.py:76`; `app/chamados/models.py:292`)

| Campo/trilha | Escritores e leitores encontrados |
|---|---|
| `via_encerramento` | Núcleo escreve no terminal e limpa ao voltar a vivo; catálogo significa **quem/como encerrou**, não idade/lastro. (`app/chamados/models.py:245`; `app/chamados/models.py:249`; `app/chamados/catalogo/motor.py:86`) |
| `admin` | Reabertura considera fato novo desde encerramento; vigia de furos reconhece decisão humana. (`app/chamados/models.py:1342`; `app/ponto/services/furos_sem_cobranca.py:78`) |
| `cascade` | Bloqueia renascimento automático e retorno de pai de adesão. (`app/chamados/models.py:1007`; `app/chamados/services/adesao.py:90`) |
| `competencia_trancada` | UI trata como arquivo; serviço usa para reabrir ao destrancar; resolvedor de perguntas evita interpretar como realidade regularizada. (`app/chamados/views_arquivar.py:64`; `app/chamados/views_arquivar.py:165`; `app/chamados/services/competencia_trancada.py:259`; `app/chamados/services/disputa_emissao.py:1788`; `app/chamados/services/disputa_emissao.py:1815`) |
| Via preenchida | I16 exige via em terminal, sem verificar idade versus lastro; comando de reversão fotografa a via. (`app/chamados/management/commands/auditar_invariantes_chamados.py:1069`; `app/chamados/management/commands/reabrir_orfas_enterradas.py:135`) |
| `estado_chamado` | Histórico apresenta; vigia lê transições e compara última trilha ao banco. (`app/chamados/services/historico.py:86`; `app/chamados/services/vigia_loop.py:28`; `app/chamados/services/vigia_loop.py:114`) |

`motivo_fechamento` pertence à **DisputaSupervisao**, não ao chamado. A porta da disputa escreve/limpa; ide

[truncado em 20000 de 25199 caracteres]

_3276 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
