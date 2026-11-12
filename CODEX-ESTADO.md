# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 23:23** · publicado em **11/11/2026 23:26** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 23:23 · sessao 2ecf1a0154a7 · FIO-E0-V4-E4B · construtor · origem=Code

C12 aplicada. A [entrega com tabela, leitores/escritores e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c12.txt:6) registra os 46 sítios migrados. [nao verificado]

Validação local:

- [15 testes novos verdes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:1185) e [106 de regressão verdes](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:730). [nao verificado]
- [55 mutantes: exatamente um subTest vermelho por sítio](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:470).
- [Base: 46 falhas esperadas](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/chamados/tests/e4b_c12_execucao.txt:1162).

ORM D3/D5 preparado, sem execução: **[não verificado]**. Sem commit, conforme a [proibição da obra](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c12.txt:4). Certificação permanece com Code/suite/opus. [nao verificado]

## 11/11 23:01 · sessao e5ede8a32f5b · - · - · origem=Ronald (tmux)

Exploração somente leitura concluída; ORM não executado [não verificado].

- Reutilizar `RecusaS143RuntimeTest._cenario`: cria exportação, fechamento e pergunta viva com assertions reais (`chamados/tests/test_e4b_c6_orm.py:45`).
- D3 deve testar `resolvido → fechado`: matriz permite (`chamados/catalogo/motor.py:102`), guarda humana de viva só roda para origem `VIVOS` (`chamados/models.py:165`), e save terminal dispara S143 (`chamados/signals.py:96`). Resultado ORM: [não verificado].
- Vínculo: tornar categoria `escala_aceite` e chamar `_executar_aceite` com template do vínculo existente; fechamento precede criação (`colaboradores/services/vinculo.py:384`, `colaboradores/services/vinculo.py:393`).
- Desligamento: `_cenario(limbo=True)` mantém disputa mista aberta pela resposta sem veredito (`chamados/models.py:1843`); cascata pula essa disputa e alcança `encerrar` (`colaboradores/services/desligamento.py:139`, `colaboradores/services/desligamento.py:155`).
- Regularização: preparar disputas fechadas sem carimbar a pergunta, verificar C6 viva, então materializar reg confirmada. Guard só barra disputas abertas (`chamados/services/regularizacao_ext.py:58`), mas coleta S143 usa `_ancoradas()` também de disputas fechadas (`chamados/services/disputa_emissao.py:1940`). C6 não consulta `disputa.fechada_em` (`chamados/juizes.py:1330`). Existência desse acervo em produção: [não verificado].
- Cron: chamar `Command()._fechar_chamado(ch, entrada)` com chamado não-furo da fixture; gate permite premissa não-furo (`chamados/models.py:728`), sítio preserva booleano (`ponto/management/commands/detectar_ausencias.py:842`).
- Signal: fechar outra disputa sem perguntas cujo pai seja `ch`, conservando a disputa da viva; receiver reconcilia pai (`chamados/signals.py:28`).
- API: chamado `batida_ausente` no dia aberto, marco E08 aceso na célula e viva em outro dia exportado. Lookup é real (`chamados/juizes.py:2105`), juiz lê célula gravada (`ponto/services/lastro.py:185`).
- Cartório: cobrar marco antigo E07 num dia cuja grade contém E08/S17; manter viva em outro dia exportado. Ramo `marcos_inexistentes → reconciliar` exercita `_julgar_colab_corpo` sem adiar fechamento (`ponto/services/cartorio.py:1523`); gate consulta mesmo juiz (`chamados/models.py:1298`).

Para mutantes ORM, seguir enxerto em memória já existente, substituindo somente função/sítio e preservando globais (`chamados/tests/e4b_c11_verificar.py:78`, `chamados/tests/e4b_c11_verificar.py:90`). Execução e comandos novos dependem dos nomes finais [não verificado].

## 11/11 22:57 · sessao 65a24b6b5e82 · FIO-E0-V4-E4B · sonda · origem=Code

JSON integral fornecido, sem reexecução independente: não localizei `sonda_mig0039.py` na cópia. [não verificado]

```json
{"aplicada_em": "2026-08-23 04:09:15.485039+00:00", "chamados_superado": 107, "mig0039_aplicada": 1, "migrations_chamados_posteriores": 14} [nao verificado]
```

**(a)** O acionamento registrado é `RunPython(backfill, desfazer)`: `backfill` na aplicação efetiva da 0039; `desfazer` na reversão dela, com dependência da 0038. ([0039_a3_via_backfill.py:38](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0039_a3_via_backfill.py:38), [0039_a3_via_backfill.py:39](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0039_a3_via_backfill.py:39))

“Somente podem rodar por migrate/rollback” seria excessivo: testes chamam ambas diretamente. ([test_esmeril2_migracoes_reversas.py:36](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:36), [test_esmeril2_migracoes_reversas.py:39](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:39))

**(b)** No fluxo de tenant novo implementado, **zero**: a tabela nasce por `CreateModel`, sem inserções; as migrations de dados anteriores apenas atualizam registros existentes. ([0001_initial.py:16](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0001_initial.py:16), [0023_rebaixar_urgencia_resolvidos.py:4](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4), [0024_rebaixar_urgencia_resolvidos_regressao.py:5](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5), [0038_a3_registrado_backfill.py:9](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/migrations/0038_a3_registrado_backfill.py:9))

O provisionamento cria o tenant com schema automático antes do bootstrap administrativo. ([models.py:21](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/models.py:21), [services.py:11](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/services.py:11), [services.py:13](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/tenants/services.py:13)) Contagem medida num tenant novo de produção: [não verificado].

**(c)** Não encontrei importação ou chamada dessas funções em código de produção fora de migrations na busca estática da cópia. Ausência em execução real: [não verificado]. O import direto encontrado está em teste; as referências produtivas encontradas à 0039 são comentários. ([test_esmeril2_migracoes_reversas.py:11](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/chamados/tests/test_esmeril2_migracoes_reversas.py:11), [cluster_espurio.py:134](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cluster_espurio.py:134), [cluster_espurio.py:136](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cluster_espurio.py:136))

Atribuir os 107 `superado` à execução da 0039, apenas com esse JSON: [não verificado].

## 11/11 22:57 · sessao 7e1f793c05ad · - · - · origem=Ronald (tmux)

No fluxo de tenant novo implementado, a 0039 encontra **0 `ChamadoColaborador`**: a tabela é criada em `app/chamados/migrations/0001_initial.py:16`, sem inserção de registros; as migrações de dados anteriores apenas alteram objetos existentes (`app/chamados/migrations/0023_rebaixar_urgencia_resolvidos.py:4`, `app/chamados/migrations/0024_rebaixar_urgencia_resolvidos_regressao.py:5`, `app/chamados/migrations/0038_a3_registrado_backfill.py:9`).

O provisionamento cria `Cliente` antes do bootstrap (`app/tenants/services.py:11`, `app/tenants/services.py:13`), com criação automática do schema habilitada (`app/tenants/models.py:14`, `app/tenants/models.py:21`). O bootstrap posterior cria Empresa, Setores e usuário administrador (`app/tenants/bootstrap.py:27`, `app/tenants/bootstrap.py:32`, `app/tenants/bootstrap.py:34`). O teste de scaffolding também contém a asserção `ChamadoColaborador.objects.count() == 0` (`app/tenants/tests.py:13`).

Isso descreve o fluxo da cópia; não executei testes nem medi um tenant novo em produção. O dado fornecido não contém essa contagem de tenant novo. [não verificado] [nao verificado]

## 11/11 22:57 · sessao f6699bf6e06d · - · - · origem=Ronald (tmux)

A 0039 registra `backfill` e `desfazer` como operação direta/reversa de `RunPython`; depende da 0038. `app/chamados/migrations/0039_a3_via_backfill.py:38-39`.

Não cabe afirmar que as funções **somente podem** executar via migrate/rollback: o teste importa a migration e chama ambas diretamente. `app/chamados/tests/test_esmeril2_migracoes_reversas.py:11`, `:36`, `:39`, `:47`, `:55`.

Na busca estática de toda a cópia, incluindo arquivos ocultos/ignorados e excluindo `.git`, não encontrei importação ou chamada dessas funções em código de produção fora migrations; ausência absoluta em execução é [não verificado]. As referências produtivas encontradas são comentários: `app/ponto/services/cluster_espurio.py:134`, `:136`, `:139`; `app/chamados/models.py:798`. A migration seguinte apenas declara a dependência: `app/chamados/migrations/0040_a27_recusa_motivo.py:9`.

Não encontrei arquivo `sonda_mig0039.py` nem ocorrência do hash `273df217b8f8d27b` na cópia; execução independente da sonda e conferência desse hash são [não verificado].

## 11/11 22:53 · sessao 0524c0e7b362 · FIO-E0-V4-E4B · censo · origem=Code

[resposta retida: senha_atribuida -- nao se publica credencial nem caminho de segredo]

## 11/11 22:50 · sessao f576ee34ec90 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

Não reexecutei a sonda; reproduzo integralmente o JSON raspado fornecido, sem validação independente em banco ou logs. [nao verificado]

```json
{
  "celulas_cobertas_sem_ata": 3,
  "celulas_por_tipo_e_ata_tipo_dia": {
    "folga|ausencia": 9,
    "troca_de_plantao|ausencia": 12
  },
  "dias_cobertos_folga_comp11": 24,
  "parcial_por_tipo_e_ata_tipo_dia": {},
  "tipos_inativos_com_ausencias": {
    "troca_de_posto": 0
  }
}
```

**(a)** No recorte solicitado da comp 11, o consolidado fornecido indica **24 células cobertas**: **21 com ata `ausencia`** (9 de `folga` + 12 de `troca_de_plantao`), **0 com ata `folga`**, **0 com outro `tipo_dia`** e **3 sem ata**. O mapa de parciais está vazio: **0 com `cobertura_parcial` no resultado fornecido**. [nao verificado]

**(b)** Nenhum tipo inativo aparece com contagem positiva de ausências; a única entrada fornecida é **`troca_de_posto`: 0**. [nao verificado]

**(c)** **A mudança isolada da grade de `ausencia` para `folga` não provoca relavratura de uma ata já carimbada e com veredito.** O próprio comentário declara que `tipo_dia` da grade não integra a impressão; com impressão igual e `forcar=False`, o código incrementa `pulados` e executa `continue`. [app/ponto/services/cartorio.py:190](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:190), [app/ponto/services/cartorio.py:1395](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:1395)

O mecanismo de atualização por mudança de regra é:

- `REGRA_VERSOES` declara `((1, '2026-09'), (2, '2026-10'), (3, '2026-10'), (4, '2026-10'))`; `versao_para` escolhe a maior versão cujo horizonte alcança a competência — portanto, **4 para a comp 11**. [app/ponto/services/cartorio.py:132](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:132), [app/ponto/services/cartorio.py:248](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:248)
- A versão entra na impressão como `regra:N`. Uma nova versão aplicável muda esse insumo; `forcar=True` também permite atravessar o pulo por impressão igual. [app/ponto/services/cartorio.py:489](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:489), [app/ponto/services/cartorio.py:1399](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:1399)
- Quando há julgamento e `apply_=True`, a ata recebe `tipo_dia` do dia da grade, passa por `lavrar_veredito` e é carimbada com a impressão. [app/ponto/services/cartorio.py:728](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:728), [app/ponto/services/cartorio.py:1836](/home/ronald/codex_ws/dbf76ed34c62dfe145ca3b263ea682ad1eb40e3e/app/ponto/services/cartorio.py:1836)

O JSON não informa a impressão nem a versão gravada nessas 21 células; sua elegibilidade individual para rejulgamento permanece [nao verificado].

## 11/11 22:47 · sessao 041b3efca22e · LASTRO-FAIXA-SEM-DEFAULT · verificador · origem=Code

**REPROVO a c5.** Encontrei três regressões bloqueantes: perda de valores quando o tipo é inativado; parcial que deixa de cobrar o restante do turno; folga aprovada que pode virar `nunca_bateu`. A propriedade “grade == status” também não está garantida para atas antigas já carimbadas. Referências: `app/ponto/services/dia_pago.py:390` **c5**; `app/chamados/juizes.py:438`; `app/ponto/services/cartorio.py:1230`, `:1275`, `:1395`.

Referências sem indicação são da **base**; **c5** indica a linha resultante do patch. Os contraexemplos abaixo são construídos a partir do código; ocorrência e impacto quantitativo em produção: **[nao verificado]**.

**1. Quem escreve, quem lê e o significado dos campos**

| Campo/valor | Quem escreve ou define | Quem lê, inclusive fora do diff |
|---|---|---|
| `faixa` | `_FAIXA_DE_TIPO` define a faixa; `faixa_de` consulta; A2 escolhe a vencedora. `app/ponto/precedencia.py:52`, `:100`, `:421` | Grade nova, status, predicado de folga humana, semana do posto, reavaliação de chamados, diagnóstico do colaborador e carimbo. `app/escala/utils.py:1224` **c5**; `app/ponto/status_dia.py:308`; `app/ponto/precedencia.py:212`; `app/escala/services/semana_do_posto.py:177`; `app/chamados/services/feriado_reavaliar.py:36`; `app/ponto/services/dia_do_colab.py:132`; `app/ponto/services/cartorio.py:1988` |
| `tipo_do_dia` | A2 escreve o código vencedor; c5 distingue folga automática por `folga_da_escala`. `app/ponto/precedencia.py:413`, `:432` **c5** | Tradutor, status, deslogados, faltas de hoje, sem-furo, diagnóstico e carimbo. `app/ponto/services/dia_decidido.py:371` **c5**; `app/ponto/status_dia.py:311`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/folha/services/sem_furo.py:26`; `app/ponto/services/dia_do_colab.py:87`; `app/ponto/services/cartorio.py:1987` |
| `tipo_dia` | Builders/overlays escrevem a classificação da grade; cartório serializa; porta persiste; leitor reconstrói da ata. `app/escala/utils.py:1801`, `:1809`; `app/escala/utils.py:1224` **c5**; `app/ponto/services/cartorio.py:728`; `app/ponto/portas/celula.py:995`; `app/escala/services/leitor_celula.py:397` | Dinheiro, status, cobrança parcial, suprajuiz, ramo sem adesão e espelho. `app/ponto/services/dia_pago.py:397` **c5**; `app/ponto/status_dia.py:189`; `app/chamados/juizes.py:438`; `app/ponto/supra_juiz.py:304`; `app/ponto/services/cartorio.py:1230`; `app/ponto/services/espelho.py:679` |
| `ausencia_tipo` | Grade obtém o código da cobertura para folha; c5 transporta grade → ata → grade lida. `app/escala/utils.py:1188`, `:1189`; `app/ponto/services/cartorio.py:765` **c5**; `app/escala/services/leitor_celula.py:436` **c5** | Novo leitor financeiro consulta o efeito vigente desse código. `app/ponto/services/dia_pago.py:390` **c5** |
| `ausencia_id`, `ausencia_label` | Grade escreve PK/display da cobertura; c5 transporta as chaves pela ata. Falta recebe ID no ramo próprio. `app/escala/utils.py:1189`, `:1216`; `app/escala/utils.py:1226` **c5**; `app/ponto/services/cartorio.py:765` **c5** | O transporte preserva as chaves, mas o link do calendário citado nas notas usa outro objeto: cobertura atual → `dia.ausencia.id/label`. `app/colaboradores/services/calendario.py:485`, `:673`; `app/templates/colaboradores/partials/_calendario_grade.html:97` |
| Mapas `abono/previsto/realizado` por dia | `por_dia_da_grade`, em minutos, indexados por `data`. `app/ponto/services/dia_pago.py:364`, `:367`; implementação nova em `:387` **c5** | Fechamento soma; lavratura copia para `DiaPago`; prontidão nova soma; retrato de exportada e testemunha da porta também consultam. `app/ponto/services/fechamento.py:1062`, `:1212`; `app/ponto/services/dia_pago.py:288`, `:464`; `app/folha/export.py:1098` **c5**; `app/folha/porta_export.py:139` |
| Totais do fechamento | Rubricas horárias vêm de `resultado`; os três totais de minutos vêm dos mapas; previsto/dias/carimbo são gravados no mesmo bloco. `app/ponto/services/fechamento.py:1002`, `:1062`, `:1083` | Prontidão com snapshot, exportação de eventos e conferências de lavratura. `app/folha/export.py:1045`, `:1076`, `:402`; `app/ponto/services/dia_pago.py:550`; `app/folha/porta_export.py:143` |
| `prontidao` | Calculada por `folha.export.prontidao`; retorna percentual e previsto em horas. `app/folha/export.py:1025`, `:1050` | Snapshot lê os totais persistidos; caminho sem snapshot passa a consultar os mapas. `app/folha/export.py:1045`; `app/folha/export.py:1098` **c5** |
| `TipoSemFaixa` | `faixa_de` levanta recusa nomeada para código fora do mapa. Já existe na c3. `app/ponto/precedencia.py:96`, `:100` | Vigia conta; três capturas passam a propagar; cartório ainda captura genericamente. `app/ponto/services/vigia_ausencia.py:163`; `app/ferias/situacao.py:95` **c5**; `app/escala/services/semana_do_posto.py:178` **c5**; `app/ponto/services/espelho.py:651` **c5**; `app/ponto/services/cartorio.py:2035` |
| `tipos_ativos_sem_faixa` | Vigia percorre **todos os tipos ativos**, mesmo sem lançamentos, perguntando a `faixa_de`. `app/ponto/services/vigia_ausencia.py:144`, `:160`, `:168` | API de contadores e revisão da família. `app/api/views_mensageria.py:1416`; `app/ponto/management/commands/vigiar_familia_ausencia.py:23` |

**Não supõem todos o mesmo significado.** A2 escolhe o fato mais forte; cobertura para folha escolhe a ausência aprovada mais recente. Portanto `tipo_dia` pode refletir a faixa de um fato enquanto `ausencia_tipo/id/label` identificam outro lançamento. Essa separação é exercitada pelo próprio teste de sobreposição da c5. `app/ponto/precedencia.py:421`; `app/ponto/turnos.py:2516`, `:2753`; `app/ponto/tests/test_lastro_c5_sem_banco.py:261` **c5**.

Datas de cobertura são inclusivas; o parcial noturno usa minutos desde o início do turno, podendo ultrapassar 1440, e inclui ambas as bordas. Os mapas financeiros mantêm a data recebida da grade; não convertem para o dia civil das batidas. `app/ponto/turnos.py:2540`, `:2638`, `:2641`, `:2647`; `app/ponto/services/dia_pago.py:390`, `:288`.

B2 separa a folga automática na origem da A2, mas `tipo_dia='folga'` continua agrupando folga humana e automática. Os leitores de cobrança que ainda interpretam esse rótulo causam os bloqueios abaixo. `app/ponto/precedencia.py:413` **c5**; `app/escala/utils.py:1224` **c5**; `app/chamados/juizes.py:438`; `app/ponto/services/cartorio.py:1230`.

**2. Dinheiro: preservação limitada e regressão concreta**

**BLOQUEIA — a ata nova perde valores quando o tipo sai do catálogo ativo.**

Contraexemplo: tipo `folga`, efeito ABONA; ausência aprovada; jornada prevista de 480 minutos; batidas pareadas cobrindo 180 minutos, por exemplo 08–11. A ata antiga era `ausencia`; a nova é `folga` com `ausencia_tipo='folga'`. Enquanto o efeito está disponível, ambas produzem abono 300, previsto 480 e realizado 180. Se o tipo for inativado, o catálogo deixa de retorná-lo e a ata nova passa a produzir **três mapas vazios**. `app/escala/utils.py:1224` **c5**; `app/ponto/services/dia_pago.py:390`, `:397` **c5**; `app/ponto/catalogo/ausencias.py:249`, `:293`.

A inativação é permitida pela porta; o signal invalida o cache, e o modelo declara que inativar preserva o histórico. O vigia de faixas não acusa esse caso, pois só conta tipos ativos sem mapa. `app/ponto/services/ausencia.py:1301`; `app/ponto/models.py:607`, `:1235`; `app/ponto/services/vigia_ausencia.py:160`.

A prova executada em memória confirmou os resultados abaixo para P=480/C=180; não exercitou banco ou persistência. As condições executadas correspondem a `app/ponto/services/dia_pago.py:387` da base e `:390` **c5**.

| Efeito disponível | Base, ata antiga `ausencia` | c5, ata nova `folga` |
|---|---:|---:|
| `abona`, `suprime` ou `desconta` | 300 / 480 / 180 | 300 / 480 / 180 |
| `None` | 300 / 480 / 180 | mapas vazios |

Alcance dessa regressão:

- **Com snapshot:** prontidão continua lendo o número gravado até sua invalidação/recálculo. **Sem snapshot:** uma janela composta só desse dia passa de 8 horas previstas/100% para `cego=True`, motivo `sem_escala`. `app/folha/export.py:1045`, `:1078`; `app/folha/export.py:1098`, `:1108` **c5**.
- **Fechamento e DiaPago:** recálculo soma os mapas; lavratura copia seus minutos. `app/ponto/services/fechamento.py:1046` **c5**; `app/ponto/services/dia_pago.py:288`.
- **Exportada:** o helper alterado também é chamado por `retratar_exportada`, que atualiza ou zera esses campos. Portanto “não tocar exportada” não está assegurado apenas por não editar esse chamador. A execução depende da guarda existente. `app/ponto/services/dia_pago.py:451`, `:464`, `:481`, `:495`.
- **Falha do catálogo:** `_do_banco` captura exceções e `vigente` usa a semente; códigos como `folga` e `troca_de_plantao` não estão nessa semente. Isso oferece outro caminho para efeito ausente. Falha real nesse caminho: **[nao verificado]**. `app/ponto/catalogo/ausencias.py:87`, `:253`, `:278`.

**O que preserva:** para dias anteriormente classificados como `ausencia`, com código ainda encontrável e efeito válido, c5 conserva P−C/P/C, inclusive nos SUPRIME/DESCONTA que o pedido mandou manter. Na prontidão sem snapshot, a contribuição antiga P passa a `(P−C)+C`, preservando o percentual nesse universo. `app/ponto/services/dia_pago.py:394` **c5**; `app/folha/export.py:1100`; `app/folha/export.py:1098` **c5**.

Não encontrei mudança direta nas fórmulas das rubricas horárias emitidas pelo TXT: elas continuam lendo os campos horários do fechamento. Isso não elimina a mudança de retenção descrita abaixo. `app/folha/export.py:402`; `app/folha/export.py:1241`.

Valores, batidas, atas e snapshots dos 24 dias reais mencionados no raspado: **[nao verificado]**.

**3. Classes de reprovação**

| Classe | Conclusão |
|---|---|
| Segundo juiz | Não identifiquei nova função que clone A2, cobertura ou previsto: a grade consulta A2 e os consumidores financeiros passam a compartilhar o helper. Permanecem leitores com regra incompatível, detalhados abaixo. Autoridades registradas: `app/core/juizes.py:184`, `:187`, `:190`, `:290`; costura c5: `app/escala/utils.py:1204`; `app/folha/export.py:1098` **c5**. |
| Segundo escritor fora da porta | Não encontrei novo escritor persistente no delta: `ata_do_dia` monta o dicionário; a lavra e a porta existentes persistem. `app/ponto/services/cartorio.py:765` **c5**; `app/ponto/services/cartorio.py:1839`; `app/ponto/portas/celula.py:995`, `:899`. |
| Relógio fora da borda | Não há nova leitura de relógio nos acréscimos do patch. Existe leitura preexistente de `localdate()`/`now()` em `julgar_celula`, mesmo com `agora=` recebido: **fila**. Contraexemplo: replay com `agora` de outro dia usa a data real nas guardas. `app/ponto/services/cartorio.py:1997`, `:2002`, `:2023`, `:2026`. |
| Derivação paralela | A soma financeira fica concentrada no helper; entretanto, leitores de `tipo_dia` mantêm interpretações próprias da cobertura. São os bloqueios parcial/sem adesão abaixo. `app/ponto/services/fechamento.py:1046` **c5**; `app/chamados/juizes.py:438`; `app/ponto/services/cartorio.py:1230`. |
| Campo com dois significados | `folga` passa a representar também ausência aprovada com efeito financeiro e possível cobertura parcial, enquanto leitores ainda o tratam como descanso integral. `app/escala/utils.py:1224` **c5**; `app/chamados/juizes.py:456`; `app/ponto/services/cartorio.py:1230`. |
| Default/except que engole | A recusa de faixa é nominal, mas a aplicação final de precedência ainda a captura genericamente; há também a captura/fallback do catálogo que alcança o novo leitor financeiro. `app/ponto/precedencia.py:105`; `app/ponto/services/cartorio.py:2033`; `app/ponto/catalogo/ausencias.py:253`, `:278`. |
| Ordem de trava | O delta não acrescenta locks/transações; save e evento continuam no mesmo `atomic`. Integração concorrente e ordem efetiva de locks: **[nao verificado]**. `app/ponto/portas/celula.py:899`. |
| Pouso antes do pré-requisito | Pouso executado: **[nao verificado]**. Suite inteira e opus final desta volta: **[nao verificado]**. A ordem exigida é suite verde → opus final → pouso. `CLAUDE.md:377`, `:381`. | [nao verificado]

**BLOQUEIA — parcial passa a calar o restante do turno.**

Contraexemplo: `folga_compensatoria` cadastrada como ABONA e `parcial=True`; ausência aprovada de 120 minutos, período 08–10; turno 08–17; marco das 17 apagado. Esse cadastro é aceito pela porta e pelo juiz de parcialidade. `app/ponto/services/ausencia.py:1301`; `app/ponto/catalogo/ausencias.py:250`; `app/ponto/turnos.py:2600`.

A c5 escreve `tipo_dia='folga'`, mantendo `cobertura_parcial=True`. Porém `ausencia_parcial_cobra_o_resto` exige literalmente `ausencia`; passa a False, e `dia_sem_trabalho` passa a True. O suprajuiz também deixa de reconhecer o dia como dia de cobrança. A prova em memória confirmou a mudança False → True no juiz de dia sem trabalho. `app/escala/utils.py:1224`, `:1234` **c5**; `app/chamados/juizes.py:438`, `:456`; `app/ponto/supra_juiz.py:304`.

**BLOQUEIA — folga integral aprovada pode virar `nunca_bateu` e reter folha.**

Contraexemplo: colaborador ativo, nenhuma batida apurável na vida; célula `trabalha=True`; folga aprovada integral em 15/10/2026, empresa com corte 21; primeiro julgamento ou julgamento forçado. Na base, a grade `ausencia` satisfaz `_coberto_inteiro_nb`; na c5, a grade `folga` deixa de satisfazê-lo. Fora do horizonte de competência 11, `sem_trabalho` também é False, e o ramo força `nunca_bateu`. `app/ponto/services/cartorio.py:1197`, `:1230`, `:1241`, `:1275`; `app/ponto/services/cartorio.py:270`, `:281`.

Esse veredito acusa e retém a folha mesmo sem protestos: muda a elegibilidade de pagamento/exportação, embora os mapas do dia possam continuar iguais. `app/escala/models.py:1767`; `app/folha/export.py:1241`, `:1251`, `:1262`.

**FILA preexistente — recusa pode ser engolida depois da lavra.**

Uma ausência aberta já persistida, de tipo sem faixa, pode ficar fora da cobertura de folha; o overlay retorna sem perguntar A2. A aplicação final de precedência encontra o tipo e sua exceção é capturada após o julgamento. Existência dessa linha em produção: **[nao verificado]**. `app/ponto/turnos.py:2516`; `app/escala/utils.py:1202`; `app/ponto/services/cartorio.py:2026`, `:2033`; `app/ponto/precedencia.py:105`.

Não atribuo esse exemplo à criação normal: o caminho com veto consulta a precedência antes. `app/ponto/services/ausencia.py:377`; `app/ponto/precedencia.py:522`.

**O RED sem banco não cobre esses caminhos reais.**

O harness executa funções e trechos AST, mas substitui ORM, calendário, cadastro e relógio. Em particular: `vigente()` vira dicionário; o teste de ata fabrica a célula diretamente; o fechamento executa só o trecho até `_dias_inc`; a prontidão recebe `previsto_em=None`. `app/ponto/tests/test_lastro_c5_sem_banco.py:7`, `:169`, `:307`, `:354`, `:240` **c5**.

Assim, ele não exercita inativação/cache/signals, guardas da impressão, `julgar_colab` completo, persistência pela porta, eventos, transações, snapshot nem retenção real. Esses caminhos estão fora dos trechos chamados pelos testes citados; suas implementações reais estão em `app/ponto/models.py:1235`; `app/ponto/services/cartorio.py:1246`, `:1395`; `app/ponto/portas/celula.py:899`; `app/folha/export.py:1045`, `:1241`.

**4. Ata antiga lida pelo leitor novo**

As três chaves ausentes **continuam ausentes**. O transporte só copia quando `campo in ata`; não consulta a cobertura atual. `app/escala/services/leitor_celula.py:436` **c5**.

Quem decide é:

- **Classificação:** `ata.tipo_dia`; se ausente/vazio, o leitor usa `cel.trabalha`. `app/escala/services/leitor_celula.py:397`.
- **Dinheiro sem slug:** o ramo legado de `por_dia_da_grade`. Ata `ausencia`, P480/C180, mantém 300/480/180; ata `folga` sem slug continua fora dos mapas. `app/ponto/services/dia_pago.py:397` **c5**.
- **Status já lavrado:** a coluna persistida, quando disponível e fora do andamento do turno. `app/ponto/status_dia.py:287`.

**BLOQUEIA a propriedade B1 integral — ata antiga pode permanecer divergente.** Contraexemplo: folga humana aprovada, ata antiga `ausencia`, status persistido `ausencia`, impressão atual e nenhum fato novo. A grade viva c5 passa a `folga`; a ata/status continuam antigos porque o cartório pula impressão igual. O patch não altera `REGRA_VERSOES`, e `tipo_dia` da grade não integra essa impressão. `app/ponto/services/cartorio.py:132`, `:192`, `:1395`; `app/escala/utils.py:1224` **c5**; `app/ponto/status_dia.py:287`.

Isso preserva o dinheiro legado da ata, mas não prova “grade == status no mesmo dia” em todo caminho. O teste de B1 chama status sem lavra e a função de status da lavra diretamente, sem passar pela guarda real. `app/ponto/tests/test_lastro_c5_sem_banco.py:228`, `:245` **c5**.

**5. B3: efeitos fora da A2**

**FILA — há mudança concreta de classe fora da A2.** Contraexemplo: troca `aguardando_decisao`, dia previsto de trabalho, nenhuma batida, ata com lâmpada apagada. Antes, A2 classificava como folga e `dias_com_furo` não incluía a data; c5 exclui a troca, A2 responde trabalho e o contador passa a incluir a data como furo. `app/ponto/precedencia.py:301` **c5**; `app/folha/services/sem_furo.py:20`, `:26`.

Simultaneamente, a cobertura de cobrança continua incluindo a troca aberta e pode suspender a cobrança antes de consultar a ata. Deslogados e faltas de hoje também a excluem pela cobertura. Isso mantém respostas distintas entre os indicadores. `app/ponto/turnos.py:2518`; `app/chamados/juizes.py:476`; `app/ponto/services/deslogados.py:163`; `app/ponto/services/faltas_de_hoje.py:59`.

A mudança do contador é consequência demonstrável; que ela seja indesejada: **[nao verificado]**. Mudança de valor financeiro causada especificamente por B3: **[nao verificado]**. A cobertura de folha já exige aprovação, independentemente dessa alteração na A2. `app/ponto/turnos.py:2516`.

**6. Leitores que não migraram ou mantêm regra própria**

| Sítio | Resultado |
|---|---|
| `app/chamados/juizes.py:438` e consumidores, inclusive `app/chamados/services/declaracao_texto.py:119` | **Bloqueia:** parcial só é reconhecida sob `ausencia`; contraexemplo 08–10/turno até 17 acima. |
| `app/ponto/supra_juiz.py:304` | **Bloqueia:** mesmo requisito literal para cobrar o restante da parcial. |
| `app/ponto/services/cartorio.py:1230` | **Bloqueia:** cobertura integral aprovada só reconhecida sob `ausencia/falta`; contraexemplo sem adesão na competência 10 acima. |
| `app/escala/utils.py:1210`, `:1213` | **Fila preexistente:** falta continua com ramo próprio por slug. Falta aprovada em folga conserva folga na grade, enquanto A2 admite o fato em faixa de atestado. `app/ponto/precedencia.py:69`, `:406`; `app/ponto/status_dia.py:311`. |
| `app/ponto/services/cartorio.py:2035` | **Fila preexistente:** captura genérica ainda alcança `TipoSemFaixa`; contraexemplo de ausência aberta já persistida acima. |
| `app/colaboradores/services/calendario.py:485`, `:673` | **Fila de evidência:** o link citado nas notas nasce da cobertura atual, não das novas chaves da ata. Transportar ID/label não prova sozinho o clique no calendário. `app/templates/colaboradores/partials/_calendario_grade.html:97`. |

No censo de `tipo_do_dia`, não encontrei outra comparação produtiva direta com `folga` além do tradutor alterado: os demais selecionam trabalho, leem faixa ou transportam o código. Li `app/ponto/services/dia_decidido.py:370`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/folha/services/sem_furo.py:26`; `app/ponto/status_dia.py:308`; `app/ponto/services/dia_do_colab.py:87`; `app/ponto/services/cartorio.py:1987`. Isso não elimina as regressões dos leitores de **`tipo_dia`** listadas acima.

## 11/11 22:43 · sessao 1c91f963fa17 · - · - · origem=Ronald (tmux)

Prova AST executada em memória, rc=0, sem escrita. Ordem: `(abono, previsto, realizado)`, P=480/C=180 (`ponto/services/dia_pago.py:364`, base; `:390-406`, c5).

| Efeito simulado | Base, ata antiga | c5, ata nova | c5, ata antiga |
|---|---|---|---|
| `abona` | 300/480/180 | 300/480/180 | 300/480/180 |
| `None` | 300/480/180 | mapas vazios | 300/480/180 |

Ata antiga: `tipo_dia='ausencia'`, sem slug. Ata nova: `tipo_dia='folga', ausencia_tipo='folga'` (`ponto/services/cartorio.py:728`, base; `:765-767`, c5). Ocorrência produtiva [nao verificado].

## 11/11 22:42 · sessao 1c91f963fa17 · - · - · origem=Ronald (tmux)

**Achado bloqueia — dinheiro depende do cadastro atual, apesar da ata congelada.** A c5 passa a consultar `efeito_vigente(ausencia_tipo)` e exclui uma ata `tipo_dia='folga'` quando esse efeito é `None` (`ponto/services/dia_pago.py:390-399`, c5). O catálogo só carrega tipos ativos e devolve `None` para código ausente (`ponto/catalogo/ausencias.py:249`, `:293-295`).

Contraexemplo de código, **ocorrência em produção [nao verificado]**: escala prevê 480 min; ausência `folga` aprovada; batidas cobrem 180 min. Na base, a ata `ausencia` gera **abono=300, previsto=480, realizado=180** (`ponto/services/dia_pago.py:388-404`, base). Na c5, a nova ata `folga` com slug conserva esses números enquanto o tipo estiver ativo; depois de inativá-lo, os três mapas ficam vazios (`ponto/services/dia_pago.py:390-399`, c5). A inativação é permitida pela porta, invalida o cache e o próprio modelo promete preservar histórico (`ponto/services/ausencia.py:1301-1308`; `ponto/models.py:607-608`, `:1235-1238`).

A mesma regressão pode ocorrer quando `_do_banco()` captura uma falha e `vigente()` usa a semente congelada, que não contém os três tipos acrescentados somente ao cadastro do teste (`ponto/catalogo/ausencias.py:253-254`, `:278`; `ponto/tests/test_lastro_c5_sem_banco.py:166-169`, c5). Falha real de banco nesse caminho [nao verificado].

**Alcance do achado:**

- Prontidão **com snapshot** conserva o número armazenado; **sem snapshot** consulta o novo helper e, numa janela composta só desse dia, muda de previsto 8 h / 100% para `cego=True, motivo='sem_escala'` (`folha/export.py:1045-1080`, base; `:1098-1109`, c5).
- Recalcular fechamento soma os mapas e grava os totais; lavrar copia seus minutos para `DiaPago` (`ponto/services/fechamento.py:1046-1063`, c5; `ponto/services/dia_pago.py:288-292`, base).
- A exportada **também alcança o helper alterado**: `retratar_exportada()` lê-o e atualiza ou zera os três campos; sua guarda restringe apenas as competências declaradas em `GUARDA_COMPETENCIA` (`ponto/services/dia_pago.py:451-464`, `:481-495`, base). Portanto o patch não oferece isolamento geral de exportadas por construção (`ponto/services/dia_pago.py:464`, base).
- A testemunha `_gravado_x_grade()` também usa o helper alterado para distinguir divergências (`folha/porta_export.py:133-145`, base).

**Preservação demonstrável, com limite:** para ausência que antes era `tipo_dia='ausencia'`, cujo código continua encontrável com qualquer dos três efeitos válidos, a c5 preserva **P−C/P/C**, incluindo `feriado_folga`: todos os efeitos fazem `_ausencia=True` (`ponto/services/dia_pago.py:390-406`, c5). Na prontidão antiga, ausência contribuía com P de abono e zero de realizado; na nova contribui com `(P−C)+C=P`, preservando o percentual nesse universo (`folha/export.py:1100-1111`, base; `:1098-1110`, c5). Garantia na frota [nao verificado].

**Ata antiga:** sem `ausencia_tipo/id/label`, o leitor simplesmente não acrescenta essas chaves; não busca ausência atual (`escala/services/leitor_celula.py:436-438`, c5). Se seu `tipo_dia` é `ausencia`, o helper preserva o contrato anterior mesmo sem slug; se é `folga`, continua excluída. Quem decide é o ramo por rótulo de `por_dia_da_grade()` (`ponto/services/dia_pago.py:397-399`, c5).

**Sem mudança direta encontrada no TXT:** o emissor de eventos lê rubricas horárias do fechamento, não os três mapas nem o rótulo; isento retorna lista vazia (`folha/export.py:402-449`, `:417-419`, base). Isso não afasta mudanças de retenção/classificação por outros leitores [nao verificado].

**Selo insuficiente para o achado:** o teste substitui `vigente()` por um dicionário em memória; não exercita consulta real, cache, signal de inativação ou falha do catálogo (`ponto/tests/test_lastro_c5_sem_banco.py:166-169`, c5). Seus testes de prontidão passam `previsto_em=None`, deixando o ramo de snapshot fora dessa prova (`ponto/tests/test_lastro_c5_sem_banco.py:240-244`, c5). A prova AST em memória confirmou o contrato antigo sem slug e o descarte da ata nova com efeito ausente; integração ORM e ocorrência produtiva [nao verificado].

## 11/11 22:42 · sessao cb5fe192ae3f · - · - · origem=Ronald (tmux)

Referências sem sufixo são da base; “c5” indica linha resultante do patch.

**1. Bloqueia: leitor financeiro/retentor não migrou de `ausencia` para `folga`.**

`ponto/services/cartorio.py:1230` define cobertura integral por `tipo_dia in ('ausencia','falta')`. O patch muda folga humana aprovada para `folga` em `escala/utils.py:1224` c5. O cartório continua com a regra anterior.

Contraexemplo: colaborador ativo, admitido antes da janela, nenhuma batida apurável na vida, célula `trabalha=True`, previsto 480, ausência `folga` aprovada inteira em 15/10/2026 numa empresa com corte 21, primeiro julgamento ou julgamento forçado. Na base, `_coberto_inteiro_nb=True`; na c5, False. `sem_trabalho` também é False porque seu horizonte começa na competência 11 (`cartorio.py:270`, `:281-285`, `:1241-1242`). A condição em `cartorio.py:1275-1277` passa a forçar `nunca_bateu`. Esse veredito acusa (`escala/models.py:1767-1768`) e retém a folha mesmo sem protestos (`folha/export.py:1241-1252`). Ocorrência produtiva: [nao verificado].

**2. Bloqueia B1 integral: a cura não alcança automaticamente atas antigas já carimbadas.**

O patch não altera `REGRA_VERSOES` (`ponto/services/cartorio.py:132`). A grade e seu `tipo_dia` não entram na impressão; isso é explicitado em `cartorio.py:190-193`. O julgamento normal pula célula com impressão igual (`cartorio.py:1395-1401`); o ramo sem adesão também (`:1246-1252`).

Contraexemplo: célula antiga de folga humana aprovada, com ata `tipo_dia='ausencia'`, `status_dia='ausencia'`, impressão atual e nenhuma mudança de insumo. Grade viva c5 passa a `folga`; leitor da ata conserva `ausencia` (`escala/services/leitor_celula.py:397-428`); leitor de status prioriza a coluna já lavrada (`ponto/status_dia.py:287-289`). Portanto “grade == status no mesmo dia” só está demonstrado para nova lavra. O teste usa `status_sem_lavra` e chama `status_da_lavra` diretamente (`ponto/tests/test_lastro_c5_sem_banco.py:228-254` c5); não executa a guarda real do cartório. Ocorrência produtiva: [nao verificado].

**3. Ata antiga: ausência das novas chaves é mantida, não preenchida.**

O transporte c5 usa `if campo in ata` (`escala/services/leitor_celula.py:436-438` c5). Logo ausência de `ausencia_tipo`, `ausencia_id`, `ausencia_label` deixa essas chaves ausentes no dia. O leitor não consulta cobertura atual. `tipo_dia` continua vindo da ata ou, se ausente/vazio, de `cel.trabalha` (`leitor_celula.py:397-398`).

Quem decide dinheiro sem slug é `por_dia_da_grade`: seu ramo c5 preserva o contrato `trabalho/ausencia` (`ponto/services/dia_pago.py:397-400` c5). Ata antiga `ausencia`, P480/C180 continua abono300, previsto480, realizado180; ata antiga `folga` sem slug continua fora desses mapas. Não há evidência de ata antiga produzida pela base representando ausência humana com `tipo_dia='folga'`: [nao verificado].

Nova lavra substitui as chaves do cartório; a porta preserva apenas `precedencia` de outra operação (`ponto/portas/celula.py:101-104`, `:991-1006`). Não há segundo escritor persistente introduzido pelo transporte: o cartório monta dicionário (`cartorio.py:720-762` base; acréscimo c5 `:762-767`), e o caminho existente chama `cel.lavrar_veredito` (`cartorio.py:1837-1843` base).

**4. Fila/limitação da recusa: permanece captura genérica que engole `TipoSemFaixa`.**

`julgar_celula` chama `aplicar_precedencia_na_celula` dentro de `except Exception`, registra log e devolve resultado (`ponto/services/cartorio.py:2032-2040` base; c5 `:2038-2046`). Essa função chama A2 (`:1984-1985` base), cujo `faixa_de` levanta a recusa (`ponto/precedencia.py:100-105`).

Contraexemplo lógico: ausência já persistida, aberta, de tipo ativo sem mapa, não sendo `troca_de_plantao`, em dia julgado. Cobertura `para='folha'` só admite aprovada (`ponto/turnos.py:2516-2519`), então `_marcar_ausencias` pode retornar antes de perguntar A2 (`escala/utils.py:1202-1203`); o cartório pode lavrar normalmente e só a aplicação final de precedência encontra o tipo e tem a exceção engolida. Existência desse cadastro/linha em produção: [nao verificado]. A criação normal com `se_existir='veto'` já chama `veto_de_lancamento`, que passa por `faixa_de` (`ponto/services/ausencia.py:377-383`, `ponto/precedencia.py:522`); portanto não afirmar que toda criação normal permite o exemplo.

**5. O selo sem banco não exercita os riscos acima.**

O harness extrai funções e trechos AST, substitui ORM, relógio e módulos (`ponto/tests/test_lastro_c5_sem_banco.py:7-10`, `:86-93`, `:158-211` c5). O teste de ata chama `ata_do_dia`, fabrica `NS(ata=..., trabalha=True, veredito='coberta')` e passa diretamente ao leitor (`:307-326` c5). Não chama `julgar_colab`, `julgar_celula`, `lavrar_veredito`, `carimbar`, `salvar_e_emitir`, transação, evento, signals ou a guarda da impressão. O teste do fechamento compila somente `_dentro` até `_dias_inc` (`:354-373` c5); não alcança persistência nem o `except Exception` real do fechamento (`ponto/services/fechamento.py:1087-1099` base). A prontidão do harness sempre fabrica `previsto_em=None` (`test_lastro_c5_sem_banco.py:237-242` c5); snapshot não é exercitado, embora exista o ramo real em `folha/export.py:1045-1080`.

**6. Travas, relógio e porta.**

Não encontrei novo relógio ou nova trava nos acréscimos de transporte, rótulo e mapas do patch; o caminho existente usa o relógio recebido ao montar grade (`ponto/services/cartorio.py:1161`) e persiste por `carimbar`/porta (`:1839`, `:1850` aproximadamente; linha exata final desse segundo ponto não reconferida: [nao verificado]). O relógio avulso preexistente ignora parcialmente `agora=`: `julgar_celula` lê `localdate()` e `now()` nas guardas (`cartorio.py:2002`, `:2023`) antes de passar `agora` ao lote (`:2026`). Como não foi introduzido pelo patch, classificaria como fila preexistente; seu alcance concreto nesta obra: [nao verificado].

A porta faz save e evento no mesmo atomic (`ponto/portas/celula.py:899-907`). Os testes c5 não provam ordem de locks, atomicidade ou integração de eventos; ordem de trava nova/invertida: [nao verificado]. Suite inteira, opus final e pré-requisitos de pouso executados nesta volta: [nao verificado], conforme as próprias notas fornecidas.

## 11/11 22:42 · sessao 5516f8ea04bc · - · - · origem=Ronald (tmux)

Achado confirmado — **bloqueia: parcial perde cobrança ao virar `folga`**. O cadastro permite `parcial=True` para qualquer tipo pela porta genérica (`app/ponto/services/ausencia.py:1301`); o catálogo entrega essa propriedade (`app/ponto/catalogo/ausencias.py:250`) e o juiz considera parcial qualquer tipo assim cadastrado com `0 < minutos < 480` (`app/ponto/turnos.py:2585`). Contraexemplo sintético: `folga_compensatoria`, ABONA, parcial=True; ausência aprovada de 120 minutos, período 08–10; jornada 08–17, marco 17 apagado. A c5 passa a escrever `tipo_dia='folga'`, conservando `cobertura_parcial=True` (`app/escala/utils.py:1224`, c5). Entretanto, o juiz registrado só reconhece parcial se `tipo_dia=='ausencia'` (`app/chamados/juizes.py:438`); assim `dia_sem_trabalho` passa a True (`app/chamados/juizes.py:456`) e o supra deixa de considerar dia de cobrança (`app/ponto/supra_juiz.py:304`). Ocorrência desse cadastro em produção: [nao verificado].

B3 — **fila: universos continuam diferentes fora da A2**. A c5 exclui troca não aprovada da A2 (`app/ponto/precedencia.py:301`, c5), mas a cobertura de cobrança continua incluindo qualquer não rejeitada (`app/ponto/turnos.py:2518`). Uma troca aberta de dia inteiro, em jornada prevista sem batidas, continua calando `_celula_furada` antes de consultar a ata (`app/chamados/juizes.py:476`). Também continua excluída dos indicadores de falta/deslogados pela cobertura (`app/ponto/services/faltas_de_hoje.py:59`; `app/ponto/services/deslogados.py:163`). A A2 já responde trabalho, enquanto esses leitores suspendem a cobrança. Essa distinção antecede a c5; regressão financeira causada por ela: [nao verificado].

B3 — mudança concreta de classe no contador: com a mesma troca aberta, célula de trabalho e lâmpada apagada, o dia deixa de ser considerado limpo e passa a furo porque `dias_com_furo` consulta a A2 atual e aceita apenas `tipo_do_dia=='trabalho'` (`app/folha/services/sem_furo.py:20`). Portanto, retirar a troca da A2 alcança leitores fora dos testes B3; alteração de valor financeiro por esse contador: [nao verificado].

B2 — não encontrei outro leitor produtivo comparando diretamente `tipo_do_dia=='folga'` além do tradutor alterado; os demais selecionam trabalho, transportam o código ou leem a faixa (`app/ponto/services/dia_decidido.py:370`; `app/ponto/services/deslogados.py:166`; `app/ponto/services/faltas_de_hoje.py:61`; `app/folha/services/sem_furo.py:26`; `app/ponto/services/dia_do_colab.py:87`; `app/ponto/status_dia.py:308`; `app/ponto/services/cartorio.py:1987`). Esse censo não elimina o achado dos leitores de **tipo_dia** acima (`app/chamados/juizes.py:438`; `app/ponto/supra_juiz.py:304`).

Recusa engolida remanescente — `julgar_celula` captura toda exceção de `aplicar_precedencia_na_celula` e apenas registra log, incluindo `TipoSemFaixa` (`app/ponto/services/cartorio.py:2033`). A chamada acontece depois de `julgar_colab`, que pode ter aplicado a lavra (`app/ponto/services/cartorio.py:2026`). As três capturas corrigidas pela c5 não cobrem essa sequência real (`app/ferias/situacao.py:95`, c5; `app/escala/services/semana_do_posto.py:178`, c5; `app/ponto/services/espelho.py:651`, c5).

## 11/11 22:38 · sessao 17dfcca52004 · - · - · origem=Ronald (tmux)

Censo adicional: 41 chamadas a `reconciliar/cancelar/encerrar/encerrar_por_competencia_trancada`; `pautas/views.py:123` é falso positivo de outro domínio. As referências abaixo são linhas do HEAD; o diff c11 fornecido não altera estes arquivos/sites. [não verificado quanto à identidade Git da cópia, que não contém `.git`]

| Sítio | Função | Disparador comprovado |
|---|---|---|
| `chamados/management/commands/auditar_chamados_orfaos.py:67` | `Command.handle` | humano comando, guarda `--fechar` em `:58` |
| `chamados/reconciliador.py:496` | `encerrar_com_retencao` | humano UI, `ponto/services/justificativa.py:87`, `:400` |
| `chamados/reconciliador.py:523` | `_retratar` | cron `config/crons.py:394` → `reconciliador.py:1368`, `:1372`; signal de batida `ponto/signals.py:215`; humano comando `ponto/management/commands/reconciliar_fantasmas.py:280` |
| `chamados/services/adesao.py:115` | `sincronizar_adesao` | cron/comando `escala/management/commands/apurar_furos_diarios.py:17`, `config/crons.py:1512` |
| `chamados/services/adesao.py:150` | `sincronizar_adesao` | mesmo caminho, `fechar=True` em `escala/management/commands/apurar_furos_diarios.py:17` |
| `chamados/services/competencia_trancada.py:184` | `encerrar` | humano comando `chamados/management/commands/encerrar_por_competencia_trancada.py:29`; ato de trancar `ponto/services/fechamento.py:1428`; não cron por desenho `config/crons.py:1276` |
| `chamados/services/feriado_reavaliar.py:47` | `encerrar` | signal `chamados/signals.py:334`; porta da célula `ponto/portas/celula.py:500` |
| `chamados/services/limbo_folgas.py:101` | `aceitar_sugestao` | humano UI `chamados/views.py:433` |
| `chamados/services/limbo_folgas.py:283` | `agrupar_furo` | cron/comando `escala/management/commands/apurar_furos_diarios.py:19`; `ponto/management/commands/detectar_ausencias.py:761` |
| `chamados/services/limbo_folgas.py:286` | `agrupar_furo` | mesmo caminho; segunda tentativa após rollback, `:284` |
| `chamados/services/materializacao.py:582` | `_materializar_ausencia_de_resposta` | confirmação/materialização de resposta; chamada humana `chamados/services/validacao.py:469` [demais gatilhos não verificados] |
| `chamados/services/materializacao.py:771` | `_reconciliar_destino_e_clones._fecha` | humano UI `chamados/services/fio.py:670`, `chamados/services/acoes_disputa.py:139`; humano comando `chamados/management/commands/backfill_fio_morto_e4b.py:147` |
| `chamados/services/regularizacao_ext.py:205` | `materializar_regularizacao_externa` | humano UI `chamados/views_regularizacao.py:217`; API colaborador `api/views.py:3000`, `api/views_core.py:1322`; signal de disputa `chamados/signals.py:133` → `regularizacao_ext.py:224` |
| `chamados/services/silencio_ausencia.py:101` | `reconciliar_por_ausencia` | signal `ponto/signals.py:265`–`:269`; humano comando `ponto/management/commands/reavaliar_ausencias_lancadas.py:54` |
| `chamados/signals.py:42` | `disputa_fechada_fecha_chamado_pai` | signal `post_save` de disputa, `:18` |
| `colaboradores/management/commands/marcar_feriado_do_posto.py:140` | `Command._marcar` | humano comando `config/crons.py:1331` |
| `colaboradores/services/desligamento.py:155` | `executar_desligamento` | humano UI `colaboradores/views.py:1434`; cron `config/crons.py:440` → `colaboradores/management/commands/efetivar_desligamentos_agendados.py:57` |
| `colaboradores/services/pos_vinculo.py:42` | `fechar_adesao_com_batida` | signal `ponto/signals.py:234`; humano UI via `colaboradores/services/vinculo.py:330` → `pos_vinculo.py:88`; humano comando `colaboradores/management/commands/corrigir_escala_retroativa.py:237` |
| `colaboradores/services/vinculo.py:387` | `_executar_aceite` | humano UI `colaboradores/views.py:423`; chat `colaboradores/services/troca_pelo_chat.py:75`; humano comando `ponto/management/commands/censo_fase_12x36.py:423` |
| `escala/management/commands/fechar_avisos_regua_na_lista.py:47` | `Command.handle` | humano comando, `--apply` em `:42` |
| `ponto/management/commands/curar_residuo_bug115.py:151` | `Command._ata_sem_furo` | humano comando `config/crons.py:1458` |
| `ponto/management/commands/detectar_ausencias.py:158` | `Command.handle` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_ausencias.py:824` | `Command._fechar_por_afastamento` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_ausencias.py:834` | `Command._fechar_por_cobertura` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_ausencias.py:845` | `Command._fechar_chamado` | cron `config/crons.py:364` |
| `ponto/management/commands/detectar_intervalo_ausente.py:138` | `Command.handle` | humano comando; cron usa `--alarme`, `config/crons.py:491`, que força dry em `detectar_intervalo_ausente.py:107` |
| `ponto/management/commands/fechar_chamados_vinculo_na_lista.py:57` | `Command.handle` | humano comando, `--apply` em `:52` |
| `ponto/services/ausencia.py:694` | `fechar_chamado_da_ausencia` | signal `ponto/signals.py:266`, `:271` → `ausencia.py:729` |
| `ponto/services/cartorio.py:986` | `julgar_colab` | cron `config/crons.py:616`; outros chamadores [não verificado] |
| `ponto/services/cartorio.py:1511` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1524` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1545` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1623` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cartorio.py:1638` | `_julgar_colab_corpo` | mesmo cartório, `config/crons.py:616` |
| `ponto/services/cluster_espurio.py:124` | `reconciliar_colab` | cartório `ponto/services/cartorio.py:1016`; humano comando `ponto/management/commands/detectar_cluster_espurio.py:46` |
| `ponto/services/cobertura.py:122` | `designar_cobertura` | humano UI `ponto/views.py:3048` |
| `ponto/services/cobertura.py:317` | `declarar_cobertura_passada` | chamador de produção não encontrado no `rg` por nome; disparador [não verificado] |
| `ponto/services/entrada_sincronizada.py:31` | `resolver_cobranca_da_entrada` | API colaborador `api/views.py:803`, `api/views_core.py:798` |
| `ponto/services/lastro.py:400` | `fechar_pelo_marco_colab` | cron consumidor `config/crons.py:394` → `chamados/reconciliador.py:1383` |
| `ponto/services/triagem_batida.py:597` | `_autoresolve_f2_f4` | UI colaborador `ponto/services/triagem_batida.py:753`, `ponto/views.py:349` |
| `ponto/services/veredito_celula.py:202` | `fechar_o_fio_do_dia` | humano UI `ponto/services/veredito_celula.py:346`, `ponto/services/declarar_dia.py:206`, `chamados/services/validacao.py:296` |

Os alvos dos 41 sites são: `reconciliar` → `resolvido`, pelo gate em `chamados/models.py:641`, `:654`; `encerrar` → `fechado`, `models.py:1605`, `:1619`; `cancelar` → decisão `CANCELAR`, `models.py:783`, `:788`; tranca → `fechado`, `models.py:1632`, `:1639`. O catálogo declara `resolvido/fechado/cancelado/superado` terminais e não declara um estado literal `encerrado`, em `chamados/catalogo/motor.py:19`, `:30`, `:31`.

**Provas e limites da classificação:**

- Todos os `reconciliar/encerrar` genéricos acima ficam **sem-prova**: a guarda de pergunta viva ignora competência entregue cujo resultado seja diferente de `False`, em `chamados/models.py:364`, `:365`; a documentação diz explicitamente que exportada fica fora em `:328`. O S143 percorre `_ancoradas()` e chama a reabertura em `chamados/services/disputa_emissao.py:1938`, `:1945`, registrando recusas em `:1946`, `:1950`.
- O site de tranca tem **não quanto à recusa S143**: escreve `via=VIA_COMPETENCIA_TRANCADA` em `chamados/models.py:1639`; o resolver retorna antes do laço de reabertura em `chamados/services/disputa_emissao.py:1899`, `:1900`. Também mede vivas pelas duas relações antes de selecionar chamados em `chamados/services/competencia_trancada.py:139`, `:142`, `:146`.
- Os dois sites de `cancelar` têm **não quanto à recusa S143**: escrevem `cancelado`, que não pertence a `ENCERRADOS` (`chamados/catalogo/motor.py:30`, `:31`); o receiver retorna em `chamados/signals.py:96`. Isso não prova ausência de pergunta viva exportada.
- `chamados/migrations/0039_a3_via_backfill.py:24` escreve `superado`; `:33` escreve `cancelado`. `RunPython` liga ambos à migração em `:39`; `.update()` não emite o `post_save`, conforme `chamados/signals.py:95`. Não excluir essas linhas do inventário fora de testes.
- `smoke_chamados.py:134` é um site dinâmico que pode alcançar terminal; `:147` só reabre, por destinos literais de `:144`; `:163` só tenta destinos de reabertura/mesmo aberto listados em `:158`. O comando recusa banco sem `test` no nome em `:32`–`:35` e força rollback em `:39`–`:44`.

**Leitores comuns / resolver:** os saves de `_gravar_estado` nomeiam `status_local` em `chamados/models.py:229`; S143 filtra encerrado e coluna salva em `chamados/signals.py:96`, `:104`, chama o resolver em `:116`. Outros leitores principais: rebaixa urgência em `signals.py:70`; C6 lê destino encerrado em `chamados/juizes.py:760`; fila/painel em `chamados/services/painel.py:155`, `:217`, `:218`; fio em `chamados/services/fio.py:432`, `:771`. `_retratar` ainda chama o resolver diretamente em `chamados/reconciliador.py:536`, além do signal.

**Obstáculos para trechos mínimos:**

| Grupo | Obstáculo comprovado |
|---|---|
| Todos os automáticos | Trocar por `fechar()` ativa perfil e guards humanos: `chamados/models.py:130`, `:136`, `:148`, `:181`; perde a via do evento original porque `_aplicar` escreve `TRANSICAO_HUMANA`, `:449`, `:450`. |
| Todos os `if reconciliar()` | `transicionar_com_pauta` devolve número de Pautas, inclusive zero após fechar, em `chamados/services/fecho_runtime.py:159`; não pode substituir como retorno booleano. `reconciliar` devolve falso/verdadeiro em `models.py:652`, `:655`. |
| Reconciliar | Preservar premissa, dedup e reata em `chamados/models.py:682`–`:692`; fechamento humano recusa sem reatar em `:165`–`:171`. |
| Desligamento | Atomic externo `colaboradores/services/desligamento.py:103`; disputa fecha antes do chamado em `:139`–`:155`; cron pode não ter user em `:70`–`:75`. |
| Regularização externa | Atomic externo `chamados/services/regularizacao_ext.py:121`; status do lote salvo antes do chamado em `:197`–`:205`; retomada pode vir de signal/on_commit `chamados/signals.py:133`–`:145`. |
| LIMBO agrupamento | Atomic e ordem global `chamados/services/limbo_folgas.py:270`–`:274`; savepoint e segunda tentativa `:277`–`:288`; idempotência inicial `:252`. |
| Materialização | Pergunta em voo e atomic do materializador `chamados/services/materializacao.py:751`–`:754`; retenção de irmãs antes do fechamento `:762`–`:771`. |
| Lastro | Fecho + nota no mesmo atomic `ponto/services/lastro.py:399`–`:402`. |
| Cobertura | Atomic externo `ponto/services/cobertura.py:99`, `:283`; user existe na assinatura `:22`, `:246`. |
| Marcar feriado | Autor pode ser `None` no lookup `colaboradores/management/commands/marcar_feriado_do_posto.py:127`; atomic por chamado em `:139`. |
| Tranca | Via própria com efeito de não resolver, `chamados/models.py:1639`, `chamados/services/disputa_emissao.py:1899`; não migrar ingenuamente para humano. |

A solução mínima que preserve comportamento dos automáticos exige permitir à porta existente envolver a execução do gate original, preservando retorno e via; uma simples troca de chamada por `fechar()` altera guardas e idempotência pelos pontos acima. Isso é proposta de implementação [não verificado], sem necessidade demonstrada de juiz novo.

## 11/11 22:37 · sessao 001149cdb325 · LASTRO-FAIXA-SEM-DEFAULT · construtor · origem=Code

C5 implementada na cópia: B1/B2/B3, transporte pela ata e propagação de `TipoSemFaixa`. Mapa, censo e escritores/leitores estão no [RELATO](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/RELATO.md:5).

Verificação local sem banco: **17 testes passaram; RED na base e nove mutantes falharam por asserções**. [Execuções e comandos](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c5_execucoes.r:1). [nao verificado]

O abono legado de `feriado_folga`/`suprime` foi preservado; quem mudaria de valor está listado no [DIFF financeiro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/RELATO.md:12).

Sem commit, conforme a proibição explícita; HEAD permanece na base. [Registro](/home/ronald/wt-codex-lastro-faixa-sem-default-215253/app/docs/lastro_faixa_c5_execucoes.r:2037). Suíte completa, opus final e valores reais de produção: **[nao verificado]**. [nao verificado]

## 11/11 22:36 · sessao 55ec9f8f4980 · - · - · origem=Ronald (tmux)

Censo humano, referências HEAD → c11 aplicado virtualmente: [nao verificado]

| Sítio c11 | Função | Disparo | Porta | Âncora |
|---|---|---|---|---|
| `chamados/services/acoes_chamado.py:71` (HEAD:70) | `acao_aprovar_excecao` | humano UI, `chamados/views.py:426-427` | sim, chamada direta c11 | sem-prova: o guard só exclui encerrados, HEAD `acoes_chamado.py:25`; não exclui perguntas |
| `chamados/services/acoes_chamado.py:110` (HEAD:108) | `acao_responder_agenda` | humano UI do colaborador, `chamados/views.py:459-461` | sim, chamada direta c11 | sem-prova: categoria/alvo conferidos HEAD `acoes_chamado.py:88-97`, sem exclusão de perguntas |
| `chamados/services/acoes_chamado.py:152` (HEAD:149) | `acao_responder_escala` | humano UI do colaborador, `chamados/views.py:463-464` | sim, chamada direta c11 | sem-prova: categoria/alvo conferidos HEAD `acoes_chamado.py:130-135`, sem exclusão de perguntas |
| `chamados/services/acoes_chamado.py:226` (HEAD:222) | `acao_autorizar_geofence` | humano UI, `chamados/views.py:540-541`, também `chamados/services/fio.py:533-546` | sim, chamada direta c11 | sem-prova: encerrados excluídos HEAD `acoes_chamado.py:176-177`, sem exclusão de perguntas |
| `chamados/services/acoes_chamado.py:282` (HEAD:277) | `acao_recusar_geofence` | humano UI, `chamados/views.py:543-544`, também `chamados/services/fio.py:544-545` | sim, chamada direta c11 | sem-prova: batida validada HEAD `acoes_chamado.py:257-265`, sem exclusão de perguntas |
| `chamados/services/acoes_chamado.py:321` (HEAD:316) | `acao_resposta_livre` | humano UI gestor, HEAD `acoes_chamado.py:313`, `chamados/views.py:547` | sim, chamada direta | sem-prova; alvo dinâmico pode qualquer `STATUS_CHOICES`, HEAD `acoes_chamado.py:287,313-316` |
| `chamados/services/fio.py:722` | `processar_post_fio` | humano UI DP, `chamados/services/fio.py:708-722` | sim, chamada direta | sem-prova; alvo POST dinâmico `fio.py:502,719` |
| `colaboradores/views.py:2069` (HEAD:2068) | `avaliar_solicitacao_beneficio` | humano UI, permissão/POST em `colaboradores/views.py:2048` | sim, chamada direta c11 | sem-prova; só exige `sol.chamado_origem`, HEAD `colaboradores/views.py:2066-2068` |
| `ponto/management/commands/fechar_chamados_meio_periodo.py:109` | `Command.handle` | humano comando `--apply`, `:94-106` | sim, `ChamadoColaborador.fechar` c11 `chamados/models.py:455` | sem-prova; universo por módulo/dia/template `:55-81`, sem exclusão de perguntas |

Todos acima chegam ao S143 síncrono por `post_save`, `chamados/signals.py:90-116`; leitores comuns incluem fila `chamados/services/painel.py:155`, placar `:217-224`, API `api/views.py:1142,1152`, contador `chamados/services/na_fila.py:17`.

Obstáculos já presentes, sem migração adicional porque esses sítios entram na c11:

- Agenda já tem atomic externo HEAD `acoes_chamado.py:102` → c11:103.
- Escala já tem atomic externo HEAD `acoes_chamado.py:140` → c11:142; a recusa chama `reverter_para_snapshot` antes de fechar, HEAD `:145-146` → c11:147-148. Essa operação pode executar a cascata de vínculo antes da porta humana.
- Benefício tem atomic externo `colaboradores/views.py:2065`; o chamado fecha antes dos efeitos de benefício `:2072-2087`.
- Geofence recusa retrata a batida antes da transição HEAD `acoes_chamado.py:269` → c11:273; autorização cria advertência antes de fechar HEAD `:193-209`.
- Meio período escolhe superuser ativo para assinar, `fechar_chamados_meio_periodo.py:102-106`; via humana permanece através de `fechar`.

Outros achados importantes para o censo completo:

| Sítio | Alvo | Observação |
|---|---|---|
| `chamados/migrations/0039_a3_via_backfill.py:24` | `superado` | `apps.get_model`, `:12`; atualização direta não emite S143, prova expressa `chamados/signals.py:90-95` |
| `chamados/migrations/0039_a3_via_backfill.py:33` | `cancelado` | reversão com modelo histórico `:32`; atualização direta não emite S143, `chamados/signals.py:90-95` |
| `chamados/management/commands/smoke_chamados.py:134` | dinâmico, inclui terminais | arquivo fora de tests; guard exige substring `test` no nome do banco, `:32-35`; atomic e rollback `:39-43`; cria chamados `smoke`, `:122-127` |
| `smoke_chamados.py:147` | `aberto`/`em_analise` | não terminal pelo laço `:144`; excluir da tabela terminal |
| `smoke_chamados.py:163` | `aberto` | não terminal pelo laço `:158`; excluir da tabela terminal |

Não reutilizar silenciosamente exclusões do censo existente: `core/censo_escritas.py:66-82` exclui migrations e arquivos `test*`; o pedido específico pode requerer que migrações sejam listadas.

Verificação AST somente em memória encontrou as únicas atribuições a `status_local` fora tests em `chamados/models.py:228,356,371`; as duas últimas são estado temporário em memória, restaurado em `:371`. Não encontrou `setattr(..., 'status_local', ...)`, nem `bulk_update` desse campo, nem alias de método guardado por `f = x.fechar` etc. [não verificado: conclusão depende do pequeno verificador executado, sem artefato de arquivo]

Imports relevantes HEAD: `transicionar_com_pauta` em `acoes_chamado.py:315` e `fio.py:721`; `encerrar` importado em `management/commands/encerrar_por_competencia_trancada.py:20` é serviço, não método de Chamado. [não verificado: completude decorre da varredura AST somente em memória]

## 11/11 22:36 · sessao 98f714f60e4a · - · - · origem=Ronald (tmux)

Referências abaixo são **HEAD físico**, salvo indicação `c11`; o diff acrescenta uma linha em `models.py` após 454, seis linhas em `fecho_runtime.py` após 68 e duas em `disputa_emissao.py` após 1835. [não verificado: contagem do patch fornecido, sem arquivo local]

| Ponto | Evidência |
|---|---|
| Terminais | `resolvido`, `fechado`, `cancelado`, `superado`; `encerrado` não consta em `ESTADOS`. `app/chamados/catalogo/motor.py:19`, `:30`, `:31`. |
| Gravador físico | `_gravar_estado`: atribui `self.status_local = novo` e inclui coluna em `campos` (`models.py:228-229`); salva em `:296`; exige via válida em `:233-245`. |
| Gate humano | `transicionar` passa por perfil (`models.py:131`), motor (`:132`), justificativa (`:136`), disputa viva (`:153`), perguntas vivas (`:166`) e CLT para resolvido (`:181-198`); chama `_aplicar` (`:199`) → `_gravar_estado` com via admin (`:450`). |
| Proteção insuficiente para exportadas | `_vivas_entre` exclui todas as perguntas cuja competência não retorna `False` (`models.py:364-365`), portanto a recusa de viva aberta não prova ausência de viva exportada; `_ancoradas` busca destino ou disputa sem destino (`:344-347`). |
| `.fechar` c11 | Delega à porta pelo import e chamada acrescentados no patch; localização virtual `models.py:454-455`. |
| Reconciliação | `reconciliar` abre atomic sem savepoint (`models.py:650`), decide+gates (`:651`, `:682`, `:685`) e grava via sistema (`:654`; c11 `:655`). |
| Encerrar | Recarrega sob atomic (`models.py:1612-1613`), motor ENCERRAR (`:1614`), gate viva (`:1617`), grava via cascade (`:1619`; c11 `:1620`). |
| Cancelar | Motor CANCELAR (`models.py:783`) → `cancelado` (`motor.py:116`), gate viva (`models.py:786`), grava via cancelado (`:788`; c11 `:789`). |
| Competência trancada | Grava via `competencia_trancada` (`models.py:1639`; c11 `:1640`), não via admin; admite explicitamente pergunta ancorada de outra competência (`:1635-1637`). |

**S143 e provas de “não”.**

- Save que nomeia status acorda S143 (`app/chamados/signals.py:75-87`, `:104-116`); só estados `ENCERRADOS` entram (`:96`), logo cancelamento tem **prova de recusa S143 inalcançável**, conforme a alternativa “OU” aceita pelo usuário. Isso não prova ausência de pergunta viva exportada. `models.py:788`, `motor.py:116`, `signals.py:96`.
- Cadeia `encerrar_por_competencia_trancada` tem **prova de recusa S143 inalcançável**: a via escrita em `models.py:1639` faz o wrapper entrar no corpo (`disputa_emissao.py:1872-1873`), que retorna contagens zero antes da coleta (`:1899-1900`; c11 `:1901-1902`).
- Demais gates encerrados: S143 roda pelo signal, recebe usuário sistema (`signals.py:114-116`) e percorre **todas as `_ancoradas`** (`disputa_emissao.py:1938-1945`; c11 `:1940-1947`).
- Reabertura exige pergunta não respondida e viva (`disputa_emissao.py:1821`), credor existente fora de VIVOS (`:1823-1828`). Recusa considera toda carga relavrada/ressuscitada mais a pergunta atual (`:1829-1833`), com exportada ou limbo (`:1835-1844`; c11 usa `_dia_de_turno_da_pergunta` e acrescenta duas linhas).
- Coleta ocorre somente para `credor_toca_exportada` ou `credor_carrega_limbo` (`disputa_emissao.py:1946-1950`; c11 `:1948-1952`).
- Fora de contexto, `registrar_recusa` é no-op (`fecho_runtime.py:18-20`); contexto atual somente nasce na porta (`:153-158`; c11 `:159-164`).

**Leitores comuns para referenciar em todas as linhas da tabela.**

| Leitor | Local |
|---|---|
| Rebaixamento de urgência | `app/chamados/signals.py:70-72` |
| Resolver S143 | `app/chamados/signals.py:96-116` |
| Reconciliar pai quando filho deixa VIVOS | `app/chamados/signals.py:355-370` |
| Materialização de fato: fallback destino encerrado | `app/chamados/juizes.py:759-760` |
| Juiz vivo: validada/tranca/decisão/resposta/lâmpada | `app/chamados/juizes.py:1330-1416` |
| Universo vivo da apresentação e desenlace | `app/chamados/services/fio.py:389-403` |
| Próximo card do fio | `app/chamados/services/fio.py:431-433` |
| Costura recusa destino terminal só para competência aberta | `app/chamados/services/fio.py:771-787` |
| Crédito de pergunta por destino/pai entre os vivos | `app/chamados/services/raiox.py:56-66` |
| Relavra de perguntas destino após cruzar ENCERRADOS | `app/chamados/models.py:273`, `:303-312` |

**Obstáculos à migração automática sem alterar regra.**

A troca direta `ch.encerrar()`/`ch.reconciliar()` por `transicionar_com_pauta(ch, ..., None)` **não conserva o comportamento existente**:

1. A porta chama obrigatoriamente `chamado.transicionar` (`fecho_runtime.py:150`, `:156`; c11 `:156`, `:162`), que consulta `get_perfil(user)` (`models.py:131`); `get_perfil` acessa `user.is_superuser` (`colaboradores/mixins.py:163`).
2. Acrescenta os guards humanos já relacionados (`models.py:136-198`).
3. Muda via de sistema/cascade/tranca para admin (`motor.py:95-96`, `models.py:450`).
4. Muda no-op automático para exceção ao repetir status (`motor.py:148-149`, `models.py:133-134` contra `models.py:652-655`, `:1615-1620`).
5. `cancelado`/`superado` não entram por transição humana (`motor.py:145-147`).
6. Atomic externo prende as travas até commit; `_gravar_estado` toma ordem C/D/Ch antes do save (`models.py:280-296`), e a porta toma contador DP apenas após a transição (`fecho_runtime.py:159`, `:122-126`; c11 `:165`, `:128-132`). [nao verificado]

Para manter gates e vias automáticos, a menor adaptação é estender **a própria** `transicionar_com_pauta` com callback opcional para executar o gate existente dentro do coletor+atomic; callsites passam `ato=ch.encerrar` ou `ato=ch.reconciliar`. Isso reutiliza os mesmos juízes; o corpo atual sempre chama o gate humano, portanto só trocar os callsites não atende à preservação. `fecho_runtime.py:149-159`, `models.py:641-655`, `:1605-1620`.

Não há método produtivo `.superar` no modelo; foi removido e duplicatas novas usam `cancelar`, conforme lápide `app/chamados/models.py:794-798`. A inexistência global de outro escritor superado ainda depende do censo principal. [não verificado]

## 11/11 22:22 · sessao fba735337724 · - · - · origem=Ronald (tmux)

Execução sem banco já tem molde na cópia: `ponto/tests/test_reverter_exportada_sem_banco.py:1-7` e `ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:1-5`. Ambos usam `unittest` stdlib, fonte real compilado e duplos apenas de I/O. O settings de testes usa PostgreSQL; evite `manage.py test`. (`config/settings_test.py:4`)

Proposta de harness:

- Extraia por AST as funções reais `_marcar_ausencias`, `por_dia_da_grade`, `fatos_do_periodo`, `folga_humana_ou_feriado`, `fatos_do_status`, `status_da_lavra` e `status_dos_dias_lavrados`. (`escala/utils.py:1156`; `ponto/services/dia_pago.py:365`; `ponto/precedencia.py:203`; `ponto/precedencia.py:284`; `ponto/status_dia.py:113`; `ponto/status_dia.py:166`; `ponto/status_dia.py:221`)
- Use QuerySet duplo estrito que efetivamente avalie filtros/exclusões, conforme o molde existente; falso que apenas retorna a lista não prova B3. (`ponto/tests/test_bo0611_05_vinculo_geo_parcial.py:23-58`)
- B1 deve usar a mesma ausência na A2 real, overlay real da grade e status real. O ramo sem lavra aceita carga e fatos já alimentados, evitando qualquer acesso ao banco. (`ponto/status_dia.py:244-247`; `ponto/status_dia.py:300-313`)
- Para comparação financeira, carregue também `por_dia_da_grade` e `prontidao` da base via `git show`, em memória, e compare resultados por tipo, com realizado zero/parcial/excedente. O molde `--base` já faz exatamente essa leitura sem alteração de fonte. (`ponto/tests/test_bo0611_05_leitores_geo.py:150-165`)
- B2 deve comparar ausência humana `'folga'` e dia exclusivamente de escala, chegando pelo produtor real. A colisão está no marcador da escala e no predicado consumidor. (`ponto/precedencia.py:215-216`; `ponto/precedencia.py:410`)
- B3 deve testar `troca_de_plantao` aguardando/aprovada/rejeitada, incluindo controle de outro tipo aguardando para verificar o recorte específico. A origem atualmente exclui apenas rejeitadas. (`ponto/precedencia.py:298-304`)

Mutantes sugeridos, sempre em memória: default de volta em `faixa_de`; atribuição independente `dia['tipo_dia']='ausencia'` na grade; restaurar marcador automático `'folga'`; remover exclusão da troca aberta. O harness pode oferecer `--mutante`, seguindo o existente. (`ponto/precedencia.py:100-105`; `escala/utils.py:1220`; `ponto/tests/test_bo0611_05_leitores_geo.py:154-165`)

Atenção: testes financeiros anteriores constroem dias `ausencia` sem `ausencia_tipo`. Eles devem continuar cobertos no teste de compatibilidade para evitar quebra involuntária. (`ponto/tests/test_bob_b2_abono_feriado_guarda.py:54-69`; `ponto/tests/test_retrato_exportada.py:47`)

Não editei arquivos nem executei testes. [nao verificado por execução] [nao verificado]

## 11/11 22:22 · sessao b8dc92699ec6 · - · - · origem=Ronald (tmux)

Censo B2, somente leitura:

| Sítio | Significado e ajuste |
|---|---|
| `ponto/precedencia.py:410` | Hoje mistura FolgaDia/escala com `Ausencia.tipo='folga'`. Separar na origem: `folga_da_escala` para `previsto=False` sem FolgaDia; preservar `folga` para FolgaDia e ausência humana. |
| `ponto/precedencia.py:216` | Predicado deve excluir `(None, 'folga_da_escala')`, mantendo o teste existente de `fatos.folga_lancada`. Assim `folga` humana passa a responder True. |
| `ponto/services/dia_decidido.py:371` | Única outra comparação produtiva direta encontrada: aceitar `('folga', 'folga_da_escala')`, preservando palavra/veredito FOLGA. |
| `ponto/status_dia.py:311` | Lê o código para efeito somente nas faixas `atestado`/`afastamento`; nova distinção não entra nesse ramo. |
| `ponto/services/deslogados.py:166` | Exclui qualquer tipo diferente de trabalho/None: significado preservado. |
| `ponto/services/faltas_de_hoje.py:61` | Mesmo predicado: significado preservado. |
| `folha/services/sem_furo.py:26` | Seleciona somente trabalho: significado preservado. |
| `ponto/services/cartorio.py:1987` | Copia o código para o carimbo de precedência existente. |
| `ponto/services/dia_do_colab.py:87` | Expõe o código no resultado; passa a distinguir origem automática. |
| `ponto/precedencia.py:450` | Texto da supressão usa faixa/rótulo; preservado. |

Testes antigos que precisam acompanhar a nova origem: expectativa de folga automática em `ponto/tests/test_a2_precedencia.py:152`; fixture automática em `ponto/tests/test_e3_status_lavrado.py:128`. A fixture humana com FolgaDia deve continuar `folga` em `ponto/tests/test_e3_status_lavrado.py:124`.

Capturas: adicionar `except TipoSemFaixa: raise` antes da captura genérica, importando a exceção junto da autoridade, em:

- `escala/services/semana_do_posto.py:175`: outras exceções continuam retornando `trabalho` em `:179`.
- `ferias/situacao.py:92`: outras exceções continuam retornando False em `:96`.
- `ponto/services/espelho.py:646`: outras exceções continuam registrando warning e prosseguindo com mapas vazios em `:650`.

A captura de cartório não foi incluída no pedido c5; permanece a captura genérica em `ponto/services/cartorio.py:2032`. Não executei testes nem fiz alterações.

## 11/11 22:20 · sessao 671e565c4e1a · FIO-E0-V4-E4B · construtor · origem=Code

Implementei as seis bordas, relógio do apply, dia canônico e entrega das recusas posteriores. Arquivos, campos e comandos estão no [recibo c11](docs/FIO-E0-V4-E4B-c11.txt:7).

Execução local sem banco: **106 testes rc=0**, 12 mutantes c11 com assertion esperada; retorno antecipado e sem Pauta também falharam nos dois casos em memória ([registro](chamados/tests/e4b_c11_execucao.txt:49)).

**ORM, queries e estado pós-reconciliador: [nao verificado].** Os comandos estão [preparados para o Code](docs/FIO-E0-V4-E4B-c11.txt:126).

A9 permanece pendente, com [trecho exato para a sessão](docs/FIO-E0-V4-E4B-c11.txt:41). Não editei models nem fiz commit, conforme as proibições ([recibo](docs/FIO-E0-V4-E4B-c11.txt:4)).

## 11/11 21:59 · sessao 0ffd773054d2 · LASTRO-FAIXA-SEM-DEFAULT · censo · origem=Code

**O abono materializado no fechamento vem da grade.** A prontidão usa esse snapshot ou calcula diretamente pela grade. O efeito cadastrado também alimenta o motor, em etapa separada. (`app/ponto/services/fechamento.py:697–701`; `app/ponto/services/fechamento.py:1060–1064`; `app/folha/export.py:1078`; `app/folha/export.py:1095–1106`)

Não reexecutei a sonda de produção; consolidei o JSON fornecido. Todos os valores das duas tabelas abaixo têm como fonte esse dado raspado, sem validação independente: **[nao verificado]**. `—` significa combinação não informada no JSON. **[nao verificado]**

| Tipo | Efeito cadastrado | Linhas aprovadas | Linhas rejeitadas | Linhas aguardando_decisao |
|---|---|---:|---:|---:|
| `folga` | abona | 55 | 2 | — |
| `folga_abonada` | abona | 9 | 1 | — |
| `folga_compensatoria` | abona | 48 | 1 | — |
| `feriado_folga` | suprime | 1 | — | — |
| `troca_de_plantao` | abona | 40 | 5 | 1 |
| **Total: 163 linhas** | — | **153** | **9** | **1** |

No corte 21→20, as janelas são: 08/2026 = 21/07–20/08; 09/2026 = 21/08–20/09; 10/2026 = 21/09–20/10; 11/2026 = 21/10–20/11; 12/2026 = 21/11–20/12. (`app/ponto/janelas.py:10–21`)

Cada célula abaixo contém **DIAS / COLABS**, conforme o agregado recebido. **[nao verificado]**

| Tipo | Status | 08/2026 anterior | 09/2026 anterior | 10/2026 anterior | **11/2026 aberta** | 12/2026 futura |
|---|---|---:|---:|---:|---:|---:|
| `folga` | aprovada | — | 3 / 1 | 40 / 22 | **10 / 7** | 2 / 1 |
| `folga` | rejeitada | — | — | 2 / 2 | — | — |
| `folga_abonada` | aprovada | — | 4 / 2 | 5 / 4 | — | — |
| `folga_abonada` | rejeitada | — | — | 1 / 1 | — | — |
| `folga_compensatoria` | aprovada | 17 / 13 | 29 / 19 | 5 / 4 | — | — |
| `folga_compensatoria` | rejeitada | — | — | 2 / 1 | — | — |
| `feriado_folga` | aprovada | 1 / 1 | — | — | — | — |
| `troca_de_plantao` | aprovada | — | 9 / 9 | 20 / 16 | **14 / 14** | — |
| `troca_de_plantao` | rejeitada | — | 1 / 1 | 5 / 4 | — | — |
| `troca_de_plantao` | aguardando_decisao | — | 1 / 1 | — | — | — |

Em **11/2026**, a soma das células aprovadas é **24 dias**; nas anteriores, **18 em 08/2026, 45 em 09/2026 e 70 em 10/2026**. Total de colaboradores distintos entre tipos ou competências: **[nao verificado]**. Situação financeira das competências anteriores — fechadas, exportadas ou pagas: **[nao verificado]**.

**(1) O que fechamento e prontidão usam para abonar**

| Sítio | Comportamento concreto | Arquivo:linha |
|---|---|---|
| Grade | A cobertura para folha seleciona aprovadas; o ramo não-falta escreve `tipo_dia='ausencia'`, sem separar os cinco tipos pelo efeito cadastrado. | `app/ponto/turnos.py:2516–2517`; `app/escala/utils.py:1188`; `app/escala/utils.py:1210–1223` |
| Mapas por dia | Inclui somente `trabalho` e `ausencia`. Para ausência: **abono = previsto − min(realizado, previsto)**. Não consulta efeito nem `ausencia_tipo`. | `app/ponto/services/dia_pago.py:387–401` |
| Fechamento: abono e previsto gravados | Obtém os mapas da grade e atribui a soma do abono a `minutos_abonados`. O previsto também seleciona somente trabalho/ausência. | `app/ponto/services/fechamento.py:1046–1049`; `app/ponto/services/fechamento.py:1060–1064`; `app/ponto/services/fechamento.py:1083` |
| Prontidão com snapshot | Lê previsto, realizado e abono materializados; soma realizado + abono no numerador. | `app/folha/export.py:1045–1050`; `app/folha/export.py:1076–1079` |
| Prontidão sem snapshot | Lê a grade. Soma previsto de trabalho/ausência, abona o **previsto inteiro** das ausências e conta realizado somente dos dias de trabalho. | `app/folha/export.py:1095–1111` |
| Outra etapa do fechamento | Consulta `_a.efeito`: `ABONA` acrescenta a `datas_justificadas`; efeito diferente de `DESCONTA` acrescenta a `_datas_suprimidas`, ressalvado o desvio das parciais. Esses conjuntos alimentam o motor. | `app/ponto/services/fechamento.py:693–701`; `app/ponto/services/fechamento.py:729–740` |

Portanto, **um dia recebido como `folga` fica fora dos três mapas**, inclusive abono e previsto; o fallback da prontidão também o exclui. Isso vale nesse caminho inclusive para `feriado_folga`: o ramo da grade escreve `ausencia` para não-falta, sem consultar seu efeito `suprime`. (`app/ponto/services/dia_pago.py:387–402`; `app/folha/export.py:1096–1106`; `app/escala/utils.py:1213–1223`)

Minutos e valores financeiros correspondentes às contagens raspadas: **[nao verificado]**.

**(2) `folga_abonada` já recebe tratamento monetário diferente de `folga`?**

**Não encontrei distinção monetária específica entre esses nomes nos caminhos examinados.** Ambos têm faixa A2 `folga`; o ramo da grade os trata como não-falta, e o cálculo monetário por grade consulta `tipo_dia`, sem consultar o slug da ausência. (`app/ponto/precedencia.py:81–82`; `app/escala/utils.py:1213–1223`; `app/ponto/services/dia_pago.py:387–401`)

No caminho cadastral, o motor usa o efeito; a rubrica **8932** seleciona os tipos que abonam e exclui apenas `ferias`, `declaracao` e `licenca_maternidade`, sem exceção para nenhum dos dois. (`app/ponto/services/fechamento.py:697–701`; `app/folha/export.py:193–205`; `app/ponto/catalogo/ausencias.py:290`; `app/ponto/catalogo/ausencias.py:308–310`)

O raspado informa **`abona` para ambos**. Diferenças de minutos, parcialidade, batidas ou pagamento efetivo entre seus registros: **[nao verificado]**.

_4061 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
