# CODEX-ESTADO

> Ultima resposta processada: **11/11/2026 14:47** · publicado em **11/11/2026 14:52** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 11/11 14:47 · sessao d4ad4297d1f3 · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE** — comportamento errado na tela exige volta, inclusive em classe B: “achado de comportamento errado na tela continua sendo volta” (anexo fornecido `aval_proporcao_por_risco_1111.txt:1`).

- **Entrada concreta:** conversa aberta com data local `11/11/2026` grava `conversa=True, data_evento=2026-11-11`; `abrir` distingue esse caso da pergunta datada de turno ([models.py:2783](app/chamados/models.py:2783), [models.py:2801](app/chamados/models.py:2801)). A emissão sem dia encaminha para `abrir_conversa` ([disputa_emissao.py:431](app/chamados/services/disputa_emissao.py:431)).
- **Resultado, estando na lista pendente:** painel mostra **“Dia: 11/11/2026”** pelo campo cru; juiz responde **`None` — sem dia de turno** ([painel_colaborador.html:198](app/templates/colaboradores/partials/painel_colaborador.html:198), [juizes.py:338](app/chamados/juizes.py:338)).
- **Enquadramento:** a função única de **rótulo** exigida pelo aval não autoriza regra independente de **dia** (anexo fornecido `aval_ui_painel_raspado.txt:8`). “Nenhum leitor tem regra propria de dia” e “Fatia que fura qualquer linha abaixo NAO FECHA” ([CLAUDE.md:21](CLAUDE.md:21), [CLAUDE.md:15](CLAUDE.md:15)). É divergência concreta da apresentação, não achado sobre a forma de testar.

Ocorrência efetiva em produção: **[nao verificado]**.

## 11/11 14:45 · sessao 5928edd1fdd0 · FIO-E0-V4-E4B · verificador · origem=Code

**REPROVADO.** Há um **BLOQUEANTE novo de ordem de trava**: a serialização por competência intercala travas de âncora com a trava compartilhada do contador DP. A suíte inteira também permanece pendente para encerramento da obra. (`app/chamados/services/fecho_runtime.py:115`; `app/pautas/services.py:188`; `app/pautas/services.py:354`; `CLAUDE.md:375`)

**B1 — BLOQUEANTE: deadlock entre âncoras e contador DP.** O laço faz `travar(A) → escrever(A) → travar(B)`. Dentro de `escrever(A)`, `lavrar` atualiza o contador DP; essa trava permanece na transação externa. Ordenar as âncoras não ordena a trava intermediária do contador. (`app/chamados/services/fecho_runtime.py:116`; `app/chamados/services/fecho_runtime.py:122`; `app/pautas/services.py:188`; `app/pautas/services.py:354`; `app/chamados/services/fecho_runtime.py:142`)

Intercalação permitida pelo código, com contador DP existente e Pautas ainda ausentes:

| Passo | Transação T1: competências A,B | Transação T2: competência B |
|---|---|---|
| 1 | Trava A, escreve e segura contador DP. | |
| 2 | | Trava B; tenta escrever e espera contador DP. |
| 3 | Pede B e espera T2. | Continua esperando T1. |

O lote com duas competências é expressamente suportado; portanto o contraexemplo não depende de inventar outro escritor. Reprodução em duas conexões: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:105`; `app/chamados/services/fecho_runtime.py:116`; `app/chamados/tests/test_e4b_c6_memoria.py:119`; `app/chamados/tests/test_e4b_c6_memoria.py:128`)

1. Quanto aos achados da c6:

| Achado anterior | Classificação nesta volta |
|---|---|
| Dia civil da conversa virando competência no novo agrupador | **Curado pela CLASSE nesse leitor.** Toda pergunta passa por `_dia_de_turno_da_pergunta`; `None` sai antes de `janela_atual`, sem data substituta. (`app/chamados/services/fecho_runtime.py:100`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:108`; `app/chamados/juizes.py:336`) |
| Duas Pautas concorrentes para marcador+âncora | **Curado pela CLASSE dos escritores E4B participantes**, pela trava determinística anterior ao `exists`, na mesma transação da criação. A cura introduz B1. **Na porta `escrever`, não achei idempotência:** ela continua fazendo `create`; o modelo não impõe unicidade dessa entrega. (`app/chamados/services/fecho_runtime.py:86`; `app/chamados/services/fecho_runtime.py:115`; `app/chamados/services/fecho_runtime.py:117`; `app/pautas/services.py:180`; `app/pautas/models.py:123`) |
| Motivo limbo truncado pela porta | **Curado pela CLASSE dos formatos produzidos pelo S143.** Reserva os motivos completos e abrevia ambas as listas com identificadores inteiros e contagens. O teste ORM mede antes da porta. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/fecho_runtime.py:64`; `app/chamados/services/fecho_runtime.py:76`; `app/chamados/tests/test_e4b_c6_orm.py:226`) |
| Coleta em memória sobrevivendo ao rollback do S143 | **Curado pela CLASSE das falhas propagadas pelo resolver ou pela saída do savepoint.** O lote só é incorporado depois dessa saída sem erro; o token sempre é restaurado. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`) |
| Reentrega não acrescenta novos IDs à Pauta existente | **Não curado — FILA.** Encontrar Pauta viva continua executando `continue`; a nota S143 conserva o rastro individual. (`app/chamados/services/fecho_runtime.py:118`; `app/chamados/services/fecho_runtime.py:120`; `app/chamados/services/disputa_emissao.py:1855`) |
| Fechamentos futuros fora das duas bordas sem entrega E4B | **Não curado — FILA de alcance.** `fechar()` continua usando transição direta; sem contexto, o coletor não entrega. O teste de passivo continua partindo de chamado já encerrado. (`app/chamados/models.py:454`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/tests/test_e4b_c6_orm.py:162`) |
| Relógio fora da borda | **Não curado — FILA preexistente.** S143 fornece `hoje=None`; C6 pode executar `date.today()`. (`app/chamados/services/disputa_emissao.py:1945`; `app/chamados/juizes.py:577`) |
| Falta de prova concorrente e de falha real de escrita da Pauta | **Não curado — FILA de prova.** O ensaio de trava é serial; o erro de memória ocorre durante a transição. Não achei ensaio de falha real de `escrever` nesses testes. (`app/chamados/tests/test_e4b_c6_orm.py:208`; `app/chamados/tests/test_e4b_c6_orm.py:209`; `app/chamados/tests/test_e4b_c6_memoria.py:113`) |
| Suíte inteira pendente | **Não curado — BLOQUEANTE de encerramento.** Os resultados fornecidos não são a suíte inteira exigida antes da leitura final. (`CLAUDE.md:375`; execução integral **[nao verificado]**) | [nao verificado]

A resposta às duas perguntas especiais é distinta: **não achei `data_evento` cru em `fecho_runtime.py`; ainda existe no caminho completo do S143**. A recusa de exportada lê `q.data_evento`; S128/HX passam o atributo cru para determinar a via, e o gate do chamado também o lê. Portanto a c7 cura o leitor acrescentado na c6, sem concluir a migração integral — **FILA preexistente**. (`app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/disputa_emissao.py:1984`; `app/chamados/services/disputa_emissao.py:2056`; `app/chamados/models.py:364`)

A retirada do retorno antecipado permanece: S128, HX e C7 seguem seus próprios juízes. O chamado pode fechar enquanto a disputa mista permanece aberta; isso é exigido pelo teste, não uma contradição entre estados. (`app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/disputa_emissao.py:1964`; `app/chamados/services/disputa_emissao.py:2007`; `app/chamados/services/disputa_emissao.py:2089`; `app/chamados/tests/test_e4b_c6_orm.py:117`; `app/chamados/tests/test_e4b_c6_orm.py:118`)

2. O mapa de escritores e leitores do delta é:

| Campo/valor | Quem escreve ou produz | Quem lê e com qual significado |
|---|---|---|
| `_RECUSAS` e token do ato | Declara `None`, instala o ato e restaura no `finally`. (`app/chamados/services/fecho_runtime.py:13`; `app/chamados/services/fecho_runtime.py:144`; `app/chamados/services/fecho_runtime.py:148`) | `registrar_recusa` e o contexto de confirmação leem o ato corrente. (`app/chamados/services/fecho_runtime.py:18`; `app/chamados/services/fecho_runtime.py:30`) |
| `ato/lote['chamado']` | Ato recebe `chamado.pk`; lote copia esse valor. (`app/chamados/services/fecho_runtime.py:143`; `app/chamados/services/fecho_runtime.py:34`) | Coletor compara com **PK de chamado**, não de pergunta ou disputa. (`app/chamados/services/fecho_runtime.py:19`) |
| Recusas coletadas | S143 produz motivo e chama o coletor; este indexa por **PK da pergunta**, guardando objeto+motivo. (`app/chamados/services/disputa_emissao.py:1840`; `app/chamados/services/disputa_emissao.py:1860`; `app/chamados/services/disputa_emissao.py:1950`; `app/chamados/services/fecho_runtime.py:20`) | Agrupador lê objeto, empresa, dia e motivo; montador interpreta os formatos do S143. (`app/chamados/services/fecho_runtime.py:106`; `app/chamados/services/fecho_runtime.py:110`; `app/chamados/services/fecho_runtime.py:66`) |
| “Savepoint confirmado” | **Não há campo persistido.** É o merge do lote após saída normal dos contextos. (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:40`) | Adaptador entrega o dicionário confirmado ao agrupador após a transição. Não significa commit da transação externa. (`app/chamados/services/fecho_runtime.py:142`; `app/chamados/services/fecho_runtime.py:149`) |
| `data_evento` / `conversa` | `abrir` fornece turno e `False`; `abrir_conversa` fornece dia civil e `True`; atribuição central em `gravar_dia_do_fato`. (`app/chamados/models.py:2789`; `app/chamados/models.py:2801`; `app/chamados/services/fio.py:801`) | Juiz distingue os significados; agrupador usa esse juiz. Os leitores crus remanescentes estão apontados acima. (`app/chamados/juizes.py:336`; `app/chamados/services/fecho_runtime.py:107`) |
| Empresa e corte | Relação do colaborador e cadastro de empresa; manutenção pela porta cadastral. (`app/colaboradores/models.py:321`; `app/colaboradores/models.py:46`; `app/colaboradores/services/estrutura.py:19`) | Runtime usa a empresa da pergunta; `janela_atual` consulta o corte efetivo. (`app/chamados/services/fecho_runtime.py:110`; `app/ponto/janelas.py:36`; `app/ponto/janelas.py:60`) |
| Competência, marcador, âncora e chave | Fim da janela produz `<empresa>:AAAA-MM`; marcador+tipo+âncora produzem a chave advisory. (`app/chamados/services/fecho_runtime.py:12`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:112`; `app/chamados/services/fecho_runtime.py:86`) | Dedup lê marcador+âncora; porta serializa o ID; chip e payload conservam o par. É competência por corte. Não achei leitor desse percurso reinterpretando-a como mês civil. (`app/chamados/services/fecho_runtime.py:118`; `app/pautas/services.py:184`; `app/pautas/services.py:334`; `app/pautas/views.py:40`) |
| Texto e listas | Montador reserva nomes e distribui os 500 caracteres; porta persiste. (`app/chamados/services/fecho_runtime.py:76`; `app/chamados/services/fecho_runtime.py:79`; `app/pautas/services.py:185`) | Payload DP entrega texto; motivo integral permanece na nota original. (`app/pautas/views.py:40`; `app/chamados/services/disputa_emissao.py:1855`) |
| Destinatário, remetente e autor | Runtime fornece DP/TI/autor central; porta valida e grava. (`app/chamados/services/fecho_runtime.py:122`; `app/chamados/services/fecho_runtime.py:124`; `app/pautas/services.py:157`; `app/pautas/services.py:180`) | Listagem filtra departamento; autoria é trilha. (`app/pautas/services.py:308`; `app/pautas/models.py:105`) |
| `feito_em` / `cancelado_em` | Portas centrais de estado da Pauta. (`app/pautas/services.py:247`; `app/pautas/services.py:294`) | Dedup usa `.vivas()`: feita ou cancelada permite nova entrega. (`app/chamados/services/fecho_runtime.py:119`; `app/pautas/models.py:57`; `app/pautas/models.py:64`) |
| `status_local` / destino / usuário | Bordas transportam; `_gravar_estado` atribui e salva. (`app/chamados/services/acoes_chamado.py:316`; `app/chamados/services/fio.py:722`; `app/chamados/models.py:228`; `app/chamados/models.py:296`) | Adaptador lê VIVOS→ENCERRADOS; signal lê estado salvo e se houve escrita do status. (`app/chamados/services/fecho_runtime.py:139`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`) |

3. Em `signals.py`, quem dispara continua sendo o `post_save` de `ChamadoColaborador`, quando não é carga `raw`, o estado está encerrado e o save escreveu o status. Um `.update()` direto não passa por esse receiver. (`app/chamados/signals.py:90`; `app/chamados/signals.py:96`; `app/chamados/signals.py:104`; `app/chamados/signals.py:95`)

No runtime, a sequência é transação do ato → transação de estado → save/receiver → savepoint S143. A novidade envolve o savepoint com o contexto coletor; Python sai primeiro do `atomic` e depois confirma o lote. Exceção no resolver ou na saída do savepoint descarta esse lote e é capturada fora dos contextos. **Não achei entrega de recusa revertida nesse percurso de falha.** (`app/chamados/services/fecho_runtime.py:142`; `app/chamados/models.py:280`; `app/chamados/models.py:296`; `app/chamados/signals.py:115`; `app/chamados/signals.py:117`; `app/chamados/services/fecho_runtime.py:39`; `app/chamados/services/fecho_runtime.py:40`)

A Pauta é escrita depois do receiver, ainda na transação externa: falha de escrita propaga e desfaz esse ato no banco. Fora das duas bordas, o contexto continua ausente; becos, reconciliador e fantasmas não ganham entrega E4B. (`app/chamados/services/fecho_runtime.py:149`; `app/chamados/services/fecho_runtime.py:19`; `app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`; `app/chamados/reconciliador.py:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`)

4. Das classes já reprovadas, **ordem de trava reaparece como B1**. Relógio fora da borda e campo temporal com dois significados permanecem nos leitores preexistentes, como FILA. **Não achei segundo juiz, fórmula paralela de competência ou persistência de Pauta fora da porta no delta produtivo.** (`app/chamados/services/fecho_runtime.py:117`; `app/pautas/services.py:354`; `app/chamados/juizes.py:577`; `app/chamados/services/disputa_emissao.py:1836`; `app/chamados/services/fecho_runtime.py:107`; `app/chamados/services/fecho_runtime.py:111`; `app/chamados/services/fecho_runtime.py:122`)

5. Nas provas, **não achei falso verde demonstrável nos quatro RED c7**, mas há limites importantes: [nao verificado]

- O fake de memória aceita escrita imediatamente numa lista, e seu `atomic` só controla profundidade. Ele não certifica rollback nem o contador DP — justamente a dependência de B1. (`app/chamados/tests/test_e4b_c6_memoria.py:36`; `app/chamados/tests/test_e4b_c6_memoria.py:62`)
- O teste ORM da trava usa uma conexão, chamadas seriais e segundo chamado/colaborador em memória. Prova `lock→exists`, sem testar a intercalação concorrente nem múltiplas âncoras disputando o contador. (`app/chamados/tests/test_e4b_c6_orm.py:196`; `app/chamados/tests/test_e4b_c6_orm.py:203`; `app/chamados/tests/test_e4b_c6_orm.py:208`)
- O texto não passa só por truncamento: mede antes da porta e compara com o persistido. O rollback exige escrita real revertida, chamado fechado e zero Pauta. (`app/chamados/tests/test_e4b_c6_orm.py:226`; `app/chamados/tests/test_e4b_c6_orm.py:228`; `app/chamados/tests/test_e4b_c6_orm.py:237`; `app/chamados/tests/test_e4b_c6_orm.py:249`)
- Não achei fixture invisível de viva/morta: as precondições consultam C6 e entrega. Já o teste de conversa injeta a recusa e chama diretamente o agrupador; sua prova é desse leitor. (`app/chamados/tests/test_e4b_c6_orm.py:78`; `app/chamados/tests/test_e4b_c6_orm.py:82`; `app/chamados/tests/test_e4b_c6_orm.py:182`)
- O protocolo exige GREEN anterior e assertion-alvo, rejeitando erro e precondição quebrada. Há mordida para as quatro curas, mas **não achei mordida para B1**. (`app/chamados/tests/e4b_c6_verificar.py:187`; `app/chamados/tests/e4b_c6_verificar.py:199`; `app/chamados/tests/e4b_c6_verificar.py:34`)
- O lint fornecido continua vermelho; o import não usado existe. O aviso sobre `falha` não demonstra falso verde: o contexto é consumido na própria iteração. (`app/chamados/tests/test_e4b_c6_memoria.py:7`; `app/chamados/tests/test_e4b_c6_memoria.py:264`; `app/chamados/tests/test_e4b_c6_memoria.py:272`; `c7_orm.log:6`, anexo)

Os GREENs e roteiros fornecidos não eliminam B1 nem substituem a suíte inteira. Execução independente, ocorrência do deadlock em produção e correspondência Git integral ao hash/base: **[nao verificado]**; metadados Git nesta cópia: **não achei**. [nao verificado]

## 11/11 14:44 · sessao af472d58e1b7 · - · - · origem=Ronald (tmux)

Censo de `app/chamados`: **181 coordenadas leitoras de produção** (123 runtime +58 management), **143 de testes** e **2 de migrations**; total **326**. Unidade: `arquivo:linha`, deduplicando várias ocorrências na mesma linha; excluída somente `_dia_de_turno_da_pergunta` em `app/chamados/juizes.py:338`.

Atribuição direta de produção: **1**, `gravar_dia_do_fato`, em `app/chamados/services/fio.py:800`. O nascimento instancia sem `data_evento` e chama esse escritor em `app/chamados/models.py:3014` e `app/chamados/models.py:3016`.

Sete leitores dinâmicos adicionais, todos em `app/chamados/services/fio.py`: `1946` (exclude por chave), `1948` (values), `1949` (order_by), `1950` (dicionário serializado `g[k]`), `1958` (values_list/order_by), `1970` (filter via zip) e `2000` (filter de marco sem dia). As definições dos campos ficam em `app/chamados/services/fio.py:1373` e `app/chamados/services/fio.py:1378`; o universo exclui conversa em `app/chamados/services/fio.py:1432`.

Guardas de conversa comprovadas:

- `juizes.py:80,81` — excluída em `app/chamados/juizes.py:78`.
- `juizes.py:214` — excluída em `app/chamados/juizes.py:213`.
- `juizes.py:2327,2331` — filtro `conversa=False` em `app/chamados/juizes.py:2328`.
- `tarefa_do_dia.py:376,380,486` — filtro `conversa=False` em `app/chamados/services/tarefa_do_dia.py:375`.
- `pergunta_no_app.py:115` — discriminador na própria expressão, `app/chamados/services/pergunta_no_app.py:115`.
- `fio.py:1447` — retorno antecipado em `app/chamados/services/fio.py:1445`.
- `fio.py:1854` — filtro em `app/chamados/services/fio.py:1852`.
- `fio.py:1955` — filtro em `app/chamados/services/fio.py:1956`.

Atenção: `dia_do_turno_da_linha` lê o campo em `app/chamados/services/fio.py:1630` e **deliberadamente devolve o dia civil da conversa** em `app/chamados/services/fio.py:1631`. Esse valor diverge do `None` do juiz em `app/chamados/juizes.py:338`, embora participe do contrato de escrita. `violacao_de_saida` também alcança conversa: leituras em `app/chamados/services/fio.py:1133` e `app/chamados/services/fio.py:1136`, universo explicitamente incluindo conversa em `app/chamados/services/fio.py:1156`.

As listas completas de coordenadas, funções, management, testes e ressalvas foram enviadas nas mensagens de consolidação; nenhuma escrita foi realizada. [nao verificado] [nao verificado]

## 11/11 14:43 · sessao 0fe14d66a330 · - · - · origem=Ronald (tmux)

**BLOQUEANTE — nova inversão entre travas de âncora e contador DP.** O laço adquire uma âncora, escreve a Pauta e só depois adquire a próxima; a escrita chama `lavrar`, cujo `UPDATE` trava o contador do departamento. Tudo permanece no `atomic` externo. (`app/chamados/services/fecho_runtime.py:115`, `app/chamados/services/fecho_runtime.py:117`, `app/chamados/services/fecho_runtime.py:122`, `app/pautas/services.py:188`, `app/pautas/services.py:352`, `app/pautas/services.py:354`, `app/chamados/services/fecho_runtime.py:142`)

Contraexemplo estático: T1 tem competências A,B e segura A+contador DP; T2 tem somente B, segura B e espera contador DP; T1 solicita B e espera T2. Ordenar âncoras não impede esse ciclo, porque a trava compartilhada do contador entra entre elas. O suporte a lote com duas competências está explicitamente exercitado. Reprodução concorrente real: **[nao verificado]**. (`app/chamados/services/fecho_runtime.py:116`, `app/chamados/services/fecho_runtime.py:122`, `app/pautas/services.py:354`, `app/chamados/tests/test_e4b_c6_memoria.py:119`, `app/chamados/tests/test_e4b_c6_memoria.py:128`)

Curas anteriores:

- **Dia civil no agrupador: curado pela classe.** Todo agrupamento passa pelo juiz, `None` sai antes da janela; competência usa o fim da janela por corte existente. Não achei `data_evento` cru em `fecho_runtime.py`. (`app/chamados/services/fecho_runtime.py:100`, `app/chamados/services/fecho_runtime.py:107`, `app/chamados/services/fecho_runtime.py:108`, `app/chamados/services/fecho_runtime.py:111`, `app/ponto/janelas.py:60`)
- **Duas Pautas pela mesma âncora: curado entre escritores E4B participantes**, por advisory determinístico adquirido antes de consultar, mantido na mesma transação da criação. **Não está implementado dentro de `pautas.services.escrever`**: essa porta continua fazendo `create` incondicional e não existe unicidade da entrega no modelo. Não achei outro emissor produtivo usando o marcador E4B. (`app/chamados/services/fecho_runtime.py:86`, `app/chamados/services/fecho_runtime.py:115`, `app/chamados/services/fecho_runtime.py:117`, `app/chamados/services/fecho_runtime.py:118`, `app/pautas/services.py:180`, `app/pautas/models.py:116`, `app/pautas/models.py:123`, `app/chamados/services/fecho_runtime.py:12`)
- **Motivo truncado: curado pela classe dos motivos produzidos.** Reserva nomes, abrevia ambas as listas preservando identificadores inteiros e contagens; produtor S143 fornece exatamente os formatos esperados pelo parser. (`app/chamados/services/fecho_runtime.py:43`, `app/chamados/services/fecho_runtime.py:64`, `app/chamados/services/fecho_runtime.py:76`, `app/chamados/services/disputa_emissao.py:1840`, `app/chamados/services/disputa_emissao.py:1844`)

Mapa de campos:

| Valor | Quem escreve/produz | Quem lê |
|---|---|---|
| ContextVar do ato | Instala ato com PK do chamado e dicionário vazio; restaura token. (`app/chamados/services/fecho_runtime.py:143`, `:144`, `:148`) | Coletor compara **PK de chamado**; não usa PK de pergunta/disputa nessa comparação. (`app/chamados/services/fecho_runtime.py:18`, `:19`) |
| Lote confirmado | Contexto cria lote, instala token e transfere perguntas depois do `yield` com saída normal; sempre restaura token. (`app/chamados/services/fecho_runtime.py:34`, `:35`, `:39`, `:40`) | Agrupador recebe somente dicionário do ato após a transição. (`app/chamados/services/fecho_runtime.py:149`, `:106`) |
| Recusas | Coletor indexa por PK da pergunta, guardando objeto+motivo. (`app/chamados/services/fecho_runtime.py:20`) | Agrupador usa objeto para dia/empresa, transporta PK e motivo; texto interpreta formato produzido pelo S143. (`app/chamados/services/fecho_runtime.py:107`, `:110`, `:113`, `:66`) |
| Dia/conversa | `abrir` fornece turno com `conversa=False`; `abrir_conversa` fornece dia civil com `conversa=True`. (`app/chamados/models.py:2789`, `:2801`) | Novo agrupador usa juiz discriminador. (`app/chamados/services/fecho_runtime.py:107`) |
| Empresa/corte | Cadastro central possui corte; porta cadastral atualiza e salva. (`app/colaboradores/models.py:46`, `app/colaboradores/services/estrutura.py:17`, `:19`, `:20`) | Agrupador usa empresa da pergunta; janela lê corte efetivo. (`app/chamados/services/fecho_runtime.py:110`, `app/ponto/janelas.py:36`, `:60`) |
| Âncora/competência | Fim da janela gera `<empresa>:AAAA-MM`; hash inclui marcador+tipo+âncora. (`app/chamados/services/fecho_runtime.py:111`, `:112`, `:86`) | Dedup usa marcador+âncora; porta serializa `ancora_id` em string; leitores gerais conservam mesmo par. (`app/chamados/services/fecho_runtime.py:118`, `app/pautas/services.py:184`, `:334`) |
| Texto | Montador limita a representação; porta persiste até 500. (`app/chamados/services/fecho_runtime.py:76`, `:79`, `app/pautas/services.py:185`) | Payload DP entrega texto e âncora sem recalcular competência. (`app/pautas/views.py:39`, `:40`, `:41`) |
| Destino/remetente/autor | Runtime fornece DP/TI/autor central; porta valida e persiste. (`app/chamados/services/fecho_runtime.py:122`, `:124`, `app/pautas/services.py:70`, `:157`, `:180`) | Listagem usa departamento; autor fica na trilha. (`app/pautas/services.py:308`, `app/pautas/models.py:105`) |
| Feita/cancelada | Portas centrais atualizam esses campos. (`app/pautas/services.py:247`, `:294`) | Dedup delega a `.vivas()`, que exclui ambas. (`app/chamados/services/fecho_runtime.py:119`, `app/pautas/models.py:57`, `:64`) |
| Status | Adaptador delega; `_gravar_estado` atribui e salva. (`app/chamados/services/fecho_runtime.py:139`, `:146`, `app/chamados/models.py:228`, `:296`) | Adaptador lê conjuntos do motor para instalar coleta. (`app/chamados/services/fecho_runtime.py:137`, `:139`) |

**FILA — alcance/rastreabilidade mantidos:** encontrar Pauta viva executa `continue`, sem acrescentar recusas de outro chamado; concluída/cancelada deixa de deduplicar. A UI recebe âncora de competência mas ainda não tem rota para abri-la; isso é preexistente e não troca competência por mês civil. (`app/chamados/services/fecho_runtime.py:118`, `:120`, `app/pautas/models.py:57`, `:64`, `app/templates/core/_copiloto.html:564`, `:570`, `:572`)

## 11/11 14:43 · sessao 5eb076058285 · FABLE-GABARITO · verificador · origem=Code

(1) **BLOQUEANTE.** Motivo literal: “CLASSE A: o diff grava estado, dinheiro”; B exige “só muda tela ou texto e não grava nada”. O custo aceito é “rótulo ou texto pode regredir sem o selo pegar”. Alterar o valor monetário persistido ultrapassa esse custo; a regra de fila para achados de forma em B não autoriza classificar essa escrita como B. (aval_proporcao_por_risco_1111.txt:1, anexo fornecido)

Entrada concreta: no mesmo arquivo, `Colab.valor_hora` retorna `self.salario / 220`; `Parcela.recalcular` executa `self.valor = self.colab.valor_hora; self.save()`. Mudar apenas `220 → 200`, com salário 2200, muda o valor gravado de 10 para 11. O consumo perde `valor_hora` porque seu receptor é `self.colab`; depois descarta os atributos gerais na comparação. (bin/selo_proporcao.py:258, bin/selo_proporcao.py:483, bin/selo_proporcao.py:524)

A reprodução em memória retornou zero achados nos dois lados; sem achados, a declaração B recebe rc 0. (bin/selo_proporcao.py:557, bin/selo_proporcao.py:676, bin/selo_proporcao.py:680)

## 11/11 14:43 · sessao 03b6d7f3c138 · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — ordem de trava nova permite deadlock.** O laço adquire uma âncora, escreve a Pauta e só então pede a próxima (`app/chamados/services/fecho_runtime.py:116`, `:117`, `:122`). A porta atualiza o mesmo contador DP e conserva essa trava até o commit (`app/pautas/services.py:188`, `:352`, `:354`; `app/chamados/services/fecho_runtime.py:142`). Intercalação permitida: A possui âncoras `[a,b]`, segura `a` e contador DP; B possui `[b,c]`, segura `b` e espera contador DP; A espera `b`. Ordenar cada lote não remove o ciclo. Reprodução em duas conexões: **[nao verificado]**.

- **Dia civil do novo agrupador: curado pela CLASSE.** `fecho_runtime.py` delega ao juiz existente e exclui `None` antes da janela, sem substituição por data civil (`app/chamados/services/fecho_runtime.py:100`, `:107`, `:108`, `:111`; `app/chamados/juizes.py:332`, `:336`).

- **“Nenhum `data_evento` cru no caminho S143”: NÃO. FILA preexistente.** O cálculo da recusa de exportada continua lendo `q.data_evento` (`app/chamados/services/disputa_emissao.py:1835`, `:1836`); S128 e HX passam o atributo cru ao juiz da via (`app/chamados/services/disputa_emissao.py:1984`, `:2056`). O gate do chamado também usa a data crua (`app/chamados/models.py:364`, `:390`). A fila para esses significados já está declarada (`app/chamados/services/fio.py:1398`, `:1404`, `:1406`).

- **Coleta revertida: curada pela CLASSE para exceções propagadas e falhas levantadas ao sair do savepoint.** O `post_save` do chamado exige status encerrado e escrita do status (`app/chamados/signals.py:90`, `:96`, `:104`). O contexto coletor envolve o `atomic`, portanto recebe a saída dele antes de publicar; restaura o ContextVar sempre e faz o merge somente após sucesso (`app/chamados/signals.py:115`; `app/chamados/services/fecho_runtime.py:35`, `:39`, `:40`). O receiver conserva best-effort, capturando exceções depois dos contextos (`app/chamados/signals.py:117`). Falhas de carimbo/save propagam (`app/chamados/models.py:3138`, `:3142`, `:3148`, `:3156`). **Não achei caminho concreto** com rollback silencioso de todo o savepoint S143 e publicação do lote; cobertura desse cenário **[nao verificado]**.

- **Relógio fora da borda: não curado, FILA preexistente.** S143 fornece `hoje=None`, e C6 pode consultar `date.today()` (`app/chamados/services/disputa_emissao.py:1943`, `:1945`; `app/chamados/juizes.py:577`). Há ainda leituras de instante para carimbos, anteriores ao delta (`app/chamados/services/disputa_emissao.py:1984`, `:2041`).

- **Alcance restrito permanece.** Só as duas bordas humanas instalam o adaptador (`app/chamados/services/acoes_chamado.py:315`, `:316`; `app/chamados/services/fio.py:721`, `:722`). Fora de contexto, o coletor não faz nada (`app/chamados/services/fecho_runtime.py:13`, `:19`). `fechar()` continua chamando a transição diretamente (`app/chamados/models.py:452`, `:454`); becos, reconciliador e fantasmas continuam chamando o resolver sem instalar o ato (`app/chamados/services/becos.py:54`; `app/chamados/reconciliador.py:536`, `:1648`; `app/ponto/management/commands/reconciliar_fantasmas.py:281`). O teste de passivo planta o chamado já encerrado por SQL; não prova todos os fechamentos humanos futuros (`app/chamados/tests/test_e4b_c6_orm.py:161`, `:162`, `:165`).

- **Não achei juiz novo ou escritor paralelo no delta produtivo.** Vitalidade, tranca, HX e fechamento de disputa continuam nas autoridades existentes (`app/chamados/services/disputa_emissao.py:1964`, `:1968`, `:2007`, `:2046`, `:2089`); a persistência de Pauta continua exclusivamente pela porta (`app/chamados/services/fecho_runtime.py:122`; `app/pautas/services.py:180`).

## 11/11 14:43 · sessao 68bad0c49046 · - · - · origem=Ronald (tmux)

Censo externo a `app/chamados`, com unidade **arquivo:linha única**:

- **16 leitores de produção**, zero escritor direto.
- **6 leitores em testes**, **36 escritores diretos em testes**, mais **1 dict destinado a create** e **2 fixtures SimpleNamespace**. [nao verificado]
- **Zero colisão com outro modelo encontrada**; única definição de campo em classe: `app/chamados/models.py:2423` (`PerguntaDisputa`).
- Não há `AGENTS.md` na cópia [nao verificado por arquivo:linha; busca `rg --files` sem resultado].

| Local | Função | Uso | Conversa |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2` | ISO em resposta JSON | sim |
| `app/api/views.py:1166` | `api_chamados_v2` | Rótulo de data em JSON; comentário chama de dia do fato em `:1160` | sim |
| `app/api/views.py:2637` | `api_disputa_responder` | Repassa a mensagem de log | não se aplica |
| `app/api/views_mensageria.py:568` | `_item_validar` | ISO em JSON | sim |
| `app/colaboradores/services/calendario.py:473` | `contexto_calendario` | Filtra intervalo; import com alias `_PD` em `:469` | sim |
| `app/colaboradores/services/calendario.py:477` | `contexto_calendario` | Indexa pergunta viva por dia | sim |
| `app/folha/previa.py:65` | `montar_previa` | Filtra competência | sim |
| `app/folha/previa.py:67` | `montar_previa` | Agrupa material por dia; consumido em `:95` | sim |
| `app/ponto/management/commands/mapa_divergencia.py:272` | `Command.handle` | Dois filtros de limite e `.only('data_evento')` | sim |
| `app/ponto/management/commands/mapa_divergencia.py:273` | `Command.handle` | Marca cobertura do furo por data | sim |
| `app/ponto/management/commands/vigia_plantio_overnight.py:75` | `batidas_fora_do_turno_da_pergunta` | Filtra relação por data não nula | sim se conversa alcançar o universo; ocorrência real [nao verificado] |
| `app/ponto/management/commands/vigia_plantio_overnight.py:86` | mesma | Determina janela pela data declarada | idem |
| `app/ponto/management/commands/vigia_plantio_overnight.py:90` | mesma | Compara data declarada ao turno real | idem |
| `app/ponto/services/dia_do_colab.py:65` | `dia_do_colab` | Filtra perguntas do retrato diário | sim |
| `app/ponto/services/veredito_celula.py:180` | `fechar_o_fio_do_dia` | Filtra dia **com `conversa=False`** | não se aplica |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` | Consome JSON: `i.get('data_evento')` → `data_do_caso` | sim |

A relação de batida com pergunta está declarada em `app/ponto/models.py:40`. Os leitores de `vigia_plantio_overnight` não excluem conversa explicitamente (`app/ponto/management/commands/vigia_plantio_overnight.py:73`).

Leitores em testes: [nao verificado]

- `app/escala/tests/test_f3_beco1_cadastro_entrou.py:90`: `_perguntas_no_passado`, filtro `data_evento__lt`.
- `app/escala/tests/test_f3_beco1_cadastro_entrou.py:129`, `:133`, `:135`: `test_RED_caso_107_folga_em_01_09_rejulga_22_e_27_09_e_poupa_a_exportada`, filtros por dia para conferir emissão/preservação.
- `app/ponto/tests/test_bateria_lavratura.py:1194`: `test_O222_o_furo_ja_sai_cobrado_na_PRIMEIRA_passada`, filtro por dia e contagem.
- `app/ponto/tests/test_cobparc_marco_no_periodo.py:386`: `_perguntas_vivas`, filtro por dia.

Escritores em testes, **36 linhas**: [nao verificado]

```text
app/api/tests/test_api_mensageria_validar.py:85
app/api/tests/test_atestado_na_resposta.py:34
app/api/tests/test_falta_na_resposta.py:41
app/colaboradores/tests/test_calendario_ultimo_recurso_le_o_c6.py:36
app/colaboradores/tests/test_desligar_guards.py:102
app/colaboradores/tests/test_desligar_guards.py:108
app/core/tests/test_ia_cobrar.py:94
app/core/tests/test_selo_performance.py:321
app/core/tests/test_selo_placar_lavrado.py:55
app/escala/tests/test_cobranca_orfa_regeneracao.py:342
app/escala/tests/test_f3_beco1_folga_na_lista.py:53
app/escala/tests/test_f3_beco1_folga_na_lista.py:146
app/escala/tests/test_f3_beco1_folga_na_lista.py:188
app/ponto/tests/test_bug114_fechador_de_dois_dias.py:238
app/ponto/tests/test_cobparc_v4_procedentes.py:230
app/ponto/tests/test_cxr_tela_le_o_motor.py:363
app/ponto/tests/test_declarar_dia.py:174
app/ponto/tests/test_fechar3_troca_escala_gate.py:202
app/ponto/tests/test_fechar3_troca_escala_gate.py:252
app/ponto/tests/test_fechar3_troca_escala_gate.py:301
app/ponto/tests/test_fio_resolve_cron.py:67
app/ponto/tests/test_justificativa_encerra_origem.py:312
app/ponto/tests/test_justificativa_encerra_origem.py:354
app/ponto/tests/test_justificativa_encerra_origem.py:515
app/ponto/tests/test_justificativa_encerra_origem.py:556
app/ponto/tests/test_jz1_autoridade_unica.py:36
app/ponto/tests/test_lastro_parte3_fecho_na_pergunta.py:83
app/ponto/tests/test_o261e_f1c4_ressuscita_pergunta_ao_c6.py:60
app/ponto/tests/test_orfas_vigia.py:66
app/ponto/tests/test_procedencia_batida.py:66
app/ponto/tests/test_procedencia_batida.py:79
app/ponto/tests/test_procedencia_batida.py:124
app/ponto/tests/test_reconciliar_fantasmas.py:84
app/ponto/tests/test_selo_chokepoint_escrita.py:271
app/ponto/tests/test_selo_chokepoint_escrita.py:306
app/ponto/tests/test_x1_folga_sem_adesao.py:384
```

`app/api/tests/test_api_mensageria_validar.py:85` é `Store`; sua persistência está em `:86` com `save(update_fields=['data_evento'])`. Os outros 35 são kwargs em `create`/`abrir`.

Escritores auxiliares:

- `app/ponto/tests/test_vassoura_orfas_resposta.py:63`: `VassouraOrfasNaoEnterraRespostaTest._perg`, `dict(data_evento=...)`; consumido por `PerguntaDisputa.objects.create(**kw)` em `:66`.
- `app/escala/tests/test_signal_ancora.py:144`: `RebaseE2LeitoresTest._confere_avulso`, fixture `NS(conversa=True, data_evento=...)`.
- `app/escala/tests/test_signal_ancora.py:381`: `RebaseE2ContratoRelogioTest._confere_backfill`, fixture `NS(data_evento=...)`.

As strings de testes que contêm código mutante/documentação, como `app/escala/tests/test_signal_ancora.py:158`, não são leitura AST executável do atributo e devem ficar fora da allowlist de leitores.

## 11/11 14:43 · sessao a7d7fc29f2f0 · - · - · origem=Ronald (tmux)

Auditoria das provas, sem executar testes nem acessar banco: [nao verificado]

- **Não achei falso verde demonstrável nos quatro RED c7.** Há limites relevantes: o lock ORM é ensaiado serialmente, na mesma conexão, e o segundo chamado/colaborador é `SimpleNamespace`; prova a ordem SQL `lock→exists` e a reentrega, sem demonstrar concorrência entre duas conexões. **FILA de comprovação**, sem concluir que a trava está errada. (`app/chamados/tests/test_e4b_c6_orm.py:187`, `app/chamados/tests/test_e4b_c6_orm.py:196`, `app/chamados/tests/test_e4b_c6_orm.py:203`, `app/chamados/tests/test_e4b_c6_orm.py:208`, `app/chamados/tests/test_e4b_c6_orm.py:210`)

- **O fake de memória aceita a escrita imediatamente:** apenas acrescenta os argumentos à lista; `atomic` somente controla profundidade, sem desfazer objetos ou escritas. Portanto os verdes de memória não certificam a porta nem rollback real. Isso está compensado parcialmente pelas provas ORM, não ocultado por elas. (`app/chamados/tests/test_e4b_c6_memoria.py:36`, `app/chamados/tests/test_e4b_c6_memoria.py:62`, `app/chamados/tests/test_e4b_c6_memoria.py:65`)

- **Texto não passa só pelo truncamento da porta:** o ORM envolve `escrever` com `wraps`, verifica o comprimento do argumento anterior à porta e exige igualdade com o texto persistido. O ensaio de memória exige contagens exatas de perguntas/limbo, além dos dois motivos preservados. (`app/chamados/tests/test_e4b_c6_orm.py:224`, `app/chamados/tests/test_e4b_c6_orm.py:226`, `app/chamados/tests/test_e4b_c6_orm.py:228`, `app/chamados/tests/test_e4b_c6_memoria.py:191`, `app/chamados/tests/test_e4b_c6_memoria.py:199`)

- **Rollback ORM tem mordida material:** o resolver substituto coleta, faz `UPDATE` real e levanta exceção; depois o teste exige chamado fechado, `UPDATE` revertido e nenhuma Pauta. A captura best-effort da exceção não basta para fazê-lo passar. (`app/chamados/tests/test_e4b_c6_orm.py:237`, `app/chamados/tests/test_e4b_c6_orm.py:246`, `app/chamados/tests/test_e4b_c6_orm.py:249`, `app/chamados/tests/test_e4b_c6_orm.py:250`, `app/chamados/tests/test_e4b_c6_orm.py:251`)

- **Saída do savepoint também tem controle**, porém somente em memória: exige que a nova coleta ainda não esteja no pai durante a saída, simula erro nessa saída e preserva lote anterior. O fechamento da variável `falha` é consumido dentro da própria iteração; não achei execução adiada que produzisse o falso verde sugerido pelo B023. (`app/chamados/tests/test_e4b_c6_memoria.py:260`, `app/chamados/tests/test_e4b_c6_memoria.py:264`, `app/chamados/tests/test_e4b_c6_memoria.py:268`, `app/chamados/tests/test_e4b_c6_memoria.py:272`, `app/chamados/tests/test_e4b_c6_memoria.py:275`)

- **Conversa:** o ORM cria a conversa pela porta e certifica `conversa=True` e dia civil preenchido; entretanto injeta o motivo de recusa e chama o agrupador diretamente. Demonstra a cura do agrupador, não uma execução integral de S143 para conversa. A memória acrescenta o caso de pergunta factual sem data. (`app/chamados/tests/test_e4b_c6_orm.py:174`, `app/chamados/tests/test_e4b_c6_orm.py:178`, `app/chamados/tests/test_e4b_c6_orm.py:182`, `app/chamados/tests/test_e4b_c6_memoria.py:165`)

- **Não achei fixture invisível de “viva/morta” no cenário ORM:** as precondições consultam C6 e entrega antes do fechamento; o caso usa corte 24 e datas do mesmo mês civil em competências diferentes. A prova de memória do corte substitui o fake de janela pelo derivador real. (`app/chamados/tests/test_e4b_c6_orm.py:23`, `app/chamados/tests/test_e4b_c6_orm.py:24`, `app/chamados/tests/test_e4b_c6_orm.py:78`, `app/chamados/tests/test_e4b_c6_orm.py:82`, `app/chamados/tests/test_e4b_c6_memoria.py:119`, `app/chamados/tests/test_e4b_c6_memoria.py:124`)

- **Gate de mutantes tem mordida:** exige GREEN anterior; depois exige rc=1, um teste, um failure, nome do caso, `AssertionError` e mensagem-alvo, rejeitando erro/precondição. A instrumentação altera `__code__` do objeto receiver registrado e o restaura em `finally`, com controle explícito no erro. (`app/chamados/tests/e4b_c6_verificar.py:187`, `app/chamados/tests/e4b_c6_verificar.py:199`, `app/chamados/tests/e4b_c6_verificar.py:75`, `app/chamados/tests/e4b_c6_verificar.py:94`, `app/chamados/tests/e4b_c6_verificar.py:117`, `app/chamados/tests/test_e4b_c6_memoria.py:237`)

- **Limites restantes:** não achei nesses testes ensaio de falha real de `pautas.services.escrever`; o erro antigo exercitado ocorre durante `transicionar`. O teste de passivo continua plantando o chamado já encerrado e não cobre todos os fechamentos futuros fora das duas bordas. (`app/chamados/tests/test_e4b_c6_memoria.py:108`, `app/chamados/tests/test_e4b_c6_memoria.py:113`, `app/chamados/tests/test_e4b_c6_orm.py:157`, `app/chamados/tests/test_e4b_c6_orm.py:162`)

- O dado fornecido informa GREEN 398 e 91 e sucesso dos roteiros; não demonstra suíte inteira nem concorrência real em duas conexões. **[nao verificado]** [nao verificado]

## 11/11 14:41 · sessao b077e75a6436 · FABLE-GABARITO · verificador · origem=Code

1. **FILA.** O aval determina literalmente: “achado do Codex sobre a FORMA de testar (como o selo poderia ser enganado) vira linha de fila, nunca volta”. O salto perdido é um escape da detecção; isso não torna B a obra que grava. (`aval_proporcao_por_risco_1111.txt:1`, anexo fornecido, itens 1–2)

   Entrada concreta: no mesmo módulo, `h()` retorna `1`, `g()` retorna `h()`, e `f(obj)` faz `obj.valor = g(); obj.save()`. Alterar somente o retorno de `h()` para `2` passa B: o consumo procura interseção direta com unidades já marcadas como escritoras, sem propagar pelo intermediário. (`bin/selo_proporcao.py:516-529`; `bin/selo_proporcao.py:676-680`)

2. **BLOQUEANTE quanto à atribuição de “custo aceito no aval”.** O custo literal autorizado é “rotulo ou texto pode regredir sem o selo pegar; quem pega e o meu smoke”; B exige que o diff “nao grava nada”. Chamar escrita entre módulos de custo autorizado amplia esse aval. (`aval_proporcao_por_risco_1111.txt:1`, anexo fornecido, itens 1–2 e frase final)

   Entrada concreta: `leitura.py` muda `h(): return 1` para `return 2`; outro módulo mantém `obj.valor = leitura.h(); obj.save()`. Esse escape está expressamente documentado na cópia, que também declara que o aval **só aceitou rótulo/texto**. A redação alegada no contrato e no commit está **[nao verificado]**. (`bin/selo_proporcao.py:33-37`; `bin/selo_proporcao.py:641-646`)

## 11/11 14:40 · sessao 57d93ad8e30c · - · - · origem=Ronald (tmux)

(b) **4 leituras comprovadas em 2 templates**, mais **1 expressão com variável não vinculada**:

| Local | Origem/contexto | Uso | Diverge para conversa |
|---|---|---|---|
| `app/templates/chamados/partials/_card_atendimento.html:87` | `p` no laço `chamado.perguntas_disputa` (`:77`); perguntas carregadas por `montar_chamados_fio`, `app/chamados/services/fio.py:225`, agrupadas `:243`, anexadas `:253`; view `app/chamados/views.py:569`, contexto `:639`, `:640`; includes `app/templates/chamados/meu_atendimento.html:127`, `:144` | Testa presença para mostrar cabeçalho “Estamos falando do dia” (`_card_atendimento.html:89`) | **sim**: conversa possui data civil; juiz retorna `None`, `app/chamados/juizes.py:338` |
| `app/templates/chamados/partials/_card_atendimento.html:90` | Mesmo contexto | Exibe como dia de referência/fato sob aquele cabeçalho | **sim**, `app/chamados/juizes.py:338` |
| `app/templates/chamados/partials/_card_atendimento.html:164` | Mesmo contexto | Exibe dia na confirmação JS de opção “Não trabalhou nesse dia?” (`:160`) | **sim**, se pergunta de conversa alcançar esse ramo; ocorrência concreta em produção **[nao verificado]** |
| `app/templates/chamados/partials/_lote_preview.html:24` | `x.pergunta` de `linhas`; view `app/chamados/views_cobrar.py::lote_preview:361`, `:363`; serviço `app/chamados/services/validacao.py::fila_validar_classificada:921`, `:923` coloca instâncias de pergunta no dicionário | Exibe dia na prévia de validação | **sim**, se conversa alcançar classe A; alcance concreto **[nao verificado]**; juiz `app/chamados/juizes.py:338` |

A expressão `app/templates/chamados/meu_atendimento.html:19` usa `p.data_evento` fora de um laço de perguntas; seu formulário está no laço de `reg`, iniciado `:10`. O contexto de `meu_atendimento` não fornece `p` (`app/chamados/views.py:633–652`). Portanto, **não é leitura comprovada de PerguntaDisputa**; divergência **não se aplica**. Há cinco expressões `.data_evento` executáveis ao todo, mas quatro com origem comprovada.

`app/templates/chamados/partials/modal_fio.html:161` contém somente comentário; não integra leitores.

(c) **2 produtores de chave JSON `data_evento`, em 2 endpoints; 1 chave derivada `data_evento_label`; 1 consumidor explícito encontrado na cópia**:

| Local | Função/rota | Uso | Diverge para conversa |
|---|---|---|---|
| `app/api/views.py:1165` | `api_chamados_v2`, rota `app/api/urls.py:73` | Serializa `p.data_evento` em ISO ou `None`; perguntas pertencem a `perguntas_por_chamado`, `app/api/views.py:1040`, `:1046`, `:1174`; resposta `:1189` | **sim** no valor transmitido, juiz `app/chamados/juizes.py:338`; exibição pelo APK **[nao verificado]** |
| `app/api/views.py:1166` | Mesmo endpoint; chave derivada `data_evento_label` | Formata o mesmo valor como `%d/%m/%Y`; resposta `:1189` | **sim**, juiz `app/chamados/juizes.py:338`; exibição pelo APK **[nao verificado]** |
| `app/api/views_mensageria.py:568` | `_item_validar`; usado por `api_mensageria_validar`, `:546`; rota `app/api/urls.py:12`; resposta `views_mensageria.py:549` | Serializa ISO ou `''`; só repassa | **sim** no valor transmitido, juiz `app/chamados/juizes.py:338` |
| `mensageria/nucleo/ferramentas.py:127` | `fila_de_validacao` (`:98`) | Lê `i.get('data_evento')`, renomeia para `data_do_caso`, repassa ao contexto; recebe payload `:107`, percorre itens `:129` | **sim** no valor repassado; uso final pela IA **[nao verificado]** |

O consumidor HTTP da mensageria chama `/api/mensageria/validar/` em `mensageria/nucleo/core_client.py:50` e devolve JSON integral em `:55`. As outras chamadas à fila em `mensageria/nucleo/ferramentas.py:190` e `:1529` usam classe/referências/contagens, sem ler a chave (respectivamente `:194–205` e `:1532–1543`).

Consumidores JS ou código do APK para essas chaves: **[nao verificado]**. Não apareceu literal `data_evento` nos arquivos JS/TS/JSX/TSX/Vue/Dart/Kotlin/Swift pesquisados **[nao verificado]**.

Observação para contagem AST de (a): `api/views.py:1165`, `:1166` e `api/views_mensageria.py:568` contêm **dois acessos de atributo cada** (teste condicional + formatação), portanto **6 nós AST em 3 posições arquivo:linha**. O consumidor de dicionário é mais uma posição: `mensageria/nucleo/ferramentas.py:127`.

## 11/11 14:37 · sessao 889902a5ad4d · MAPA-20 · verificador · origem=Code

**BLOQUEANTE — a v5 ainda aceita N sem fundamento, X sem prova e declaração de encerramento referente a outra obra incompleta.** O tripwire não corrige essas falhas semânticas. (`bin/mapa20_valida.py:91–107`, `bin/mapa20_valida.py:209–228`; `bin/mapa20_montar.py:123–124`)

1. **Entradas concretas que passam com rc 0**

**BLOQUEANTE — N inventado e X sem prova.** `app/docs/BACKLOG.md:1` contém apenas o título do arquivo. Mesmo assim, uma linha com `1 de 1`, essa fonte e estado `encerrada` passa: o selo verifica caminho permitido e linha não vazia, sem contar PRONTO, identificar a obra ou consultar provas no RELATO. (`app/docs/BACKLOG.md:1`; `bin/mapa20_valida.py:91–107`, `bin/mapa20_valida.py:209–216`)

**BLOQUEANTE — encerramento atribuído a outra obra.** E3 em `1 de 1` pode conter `E4B: encerrada`, enquanto a própria linha E4B permanece em `1 de 2`. A autorização acompanha o índice da linha E3; o selo não identifica a obra mencionada na afirmação. (`bin/mapa20_valida.py:213–214`, `bin/mapa20_valida.py:225–228`)

Este gerador mostra as duas entradas completas, sem criar arquivo. Nas sondas, ambas retornaram **rc 0**; a segunda também conservou `E4B: encerrada` na renderização local de tabela. O leitor e o retorno usados são os de `bin/mapa20_valida.py:246–259`.

```bash
python3 -B - <<'PY'
import subprocess, sys
sys.path.insert(0, "bin")
import mapa20_valida as V

for caso in ("N_SEM_FUNDAMENTO", "OUTRA_OBRA"):
    linhas = [
        "## PRONTO AUSENTE", "", "- nenhuma", "", V.LEGENDA, "",
        "| " + " | ".join(V.CABECALHO) + " |",
        "|" + "---|" * len(V.CABECALHO),
    ]
    for obra in V.OBRAS:
        c = [obra] + ["nao medido"] * 8 + [
            "1 de 2", "app/docs/BACKLOG.md:1", "item 2"
        ]
        if obra == "E3":
            c[9], c[11] = "1 de 1", "nada"
            c[7] = ("encerrada" if caso == "N_SEM_FUNDAMENTO"
                    else "E4B: encerrada")
        linhas.append("| " + " | ".join(c) + " |")
    c = [V.SEM_DONO] + ["nao medido"] * 8 + [
        V.XN_SEM_DONO, "nao medido", "nao medido"
    ]
    linhas.append("| " + " | ".join(c) + " |")
    entrada = "\n".join(linhas) + "\n"
    r = subprocess.run(
        [sys.executable, "-B", "bin/mapa20_valida.py", "/dev/stdin"],
        input=entrada, text=True, capture_output=True
    )
    print(caso, "rc", r.returncode)
PY
```

São contraexemplos do **selo real**. Publicação dessas entradas em produção, com os dados vivos, [não verificado].

2. **Bytes publicados, corridas e retornos** [nao verificado]

| Situação | Resultado da leitura |
|---|---|
| Caminho novo aprovado | Raspa a fotografia, julga o temporário raspado e copia esse mesmo arquivo ao destino. Regravar posteriormente o mapa vivo não altera essa entrada publicada. (`bin/relato.sh:100`, `bin/relato.sh:106–111`, `bin/relato.sh:158`) |
| **BLOQUEANTE — mapa retido** | Regrava a cópia pública anterior após raspagem, **sem passar pelo selo**, e permite commit/push dela. Entrada concreta: mapa vivo inválido e cópia pública contendo `E3 encerrada — 1 de 2`. Esse caminho pode conservar ou republicar o texto, com rc 1 ou 2; não é o contraexemplo de rc 0 acima. (`bin/relato.sh:116–125`, `bin/relato.sh:167–180`) |
| Raspagem nova falha ou não escreve | Falha explícita recebe 2; retorno 0 sem produzir o temporário chega ao selo como arquivo ilegível e também recebe 2. (`bin/relato.sh:106–113`; `bin/mapa20_valida.py:246–250`) |
| Remoção falha deixando o mapa presente | Aborta antes do commit/push com 2. Não retira o que já estava publicado remotamente. (`bin/relato.sh:123–124`, `bin/relato.sh:178–179`) |
| **FILA — fonte fora da fotografia** | O selo lê BACKLOG durante o julgamento; a publicação lê novamente o BACKLOG vivo depois. Inserir uma linha entre essas leituras desloca a referência publicada. A fotografia protege o mapa, não suas fontes. Ocorrência real [não verificado]. (`bin/mapa20_valida.py:100–105`; `bin/relato.sh:155`; `bin/raspar_publico.py:28`) |

**FILA — rc 2 não vence rc 1 em todos os caminhos.**

- `cp` falhando por permissão ou espaço recebe 1, como se o mapa não existisse. (`bin/relato.sh:100–101`)
- `cmp` com erro operacional também recebe 1, como divergência de conteúdo. (`bin/relato.sh:104–105`)
- Raspagem do retido falhando, seguida de remoção bem-sucedida, conserva o rc anterior; pode continuar sendo 1. O teste exige justamente esse resultado. (`bin/relato.sh:119–125`; `bin/tests/test_mapa20_valida.sh:309`)

O valor acumulado chega aos dois finais normais: “nada mudou” e depois do push. Entretanto, as classificações acima já podem estar erradas. (`bin/relato.sh:173`, `bin/relato.sh:180`)

O chamador operacional encontrado na cópia é `hasner-relato.service`, diretamente por `ExecStart`, como `oneshot`. A unit não declara tratamento distinto para 1 e 2 nem ação de recuperação; o timer apenas agenda novas execuções. Configuração efetivamente instalada e reação observada em produção [não verificado]. (`bin/hasner-relato.service:12–21`; `bin/hasner-relato.timer:5–8`)

3. **Cura dos achados do opus**

| Achado anterior | Veredito por classe |
|---|---|
| Invisíveis e homóglifos indicados | **Curado.** Alfabeto fechado recusa essas classes antes do parser. (`bin/mapa20_valida.py:78–79`, `bin/mapa20_valida.py:145–149`) |
| `encerrada` fora da tabela | **Curado quanto às posições citadas**: título, lista, negrito e rodapé entram na varredura global. **Não curado semanticamente**: outra obra pode ser declarada encerrada dentro de uma linha completa, como no contraexemplo. (`bin/mapa20_valida.py:213–228`) |
| `splitlines()` versus GFM | **Curado.** Há divisão somente em LF e rejeição dos demais separadores pelo alfabeto. (`bin/mapa20_valida.py:145–150`) |
| N inventado | **Não curado — BLOQUEANTE.** Mudou a categoria de caminho permitida; ainda aceita título do BACKLOG e comentário do TICKETS. (`bin/mapa20_valida.py:91–107`; `app/docs/BACKLOG.md:1`; `app/docs/TICKETS.md:1`) |
| Dois escritores do mapa vivo | **Cura não demonstrada.** O tripwire compara bytes, não autoria. Remoção do antigo `logs/MAPA-20/montar_mapa.py` e escritores vivos [não verificado]. O próprio código registra sua existência anterior fora do repositório. (`bin/relato.sh:104–105`; `bin/mapa20_montar.py:4–8`; `.gitignore:53`) |
| rc 1 sem consumidor específico | **Não curado — FILA.** A unit chama o script, mas não distingue as duas classes de falha. (`bin/hasner-relato.service:12–21`) |
| `SEM DONO` aceita encerramento | **Curado.** Apesar do `continue`, a varredura global posterior também alcança essa linha. (`bin/mapa20_valida.py:202–205`, `bin/mapa20_valida.py:225–228`) |
| X sem prova no RELATO | **Não curado — BLOQUEANTE.** O ramo numérico compara valores e verifica a referência de N; não lê provas de itens. (`bin/mapa20_valida.py:206–216`) |
| Testemunha LEI-AKITA ligada a `e59c25049` | Cura histórica e validade dessa testemunha [não verificado]. A declaração de escritor único no código não demonstra remoção do escritor anterior. (`bin/mapa20_montar.py:2–8`) |
| Raspagem depois do julgamento | **Curado no caminho novo; não no caminho retido.** (`bin/relato.sh:106–120`, `bin/relato.sh:158`) |
| Isenção de `status/encerrada.sim` | **Curado.** Caminhos não são mais removidos antes da busca; os testes agora exigem rejeição. (`bin/mapa20_valida.py:82–88`; `bin/tests/test_mapa20_valida.sh:145–152`) |
| Ambiguidade de rc 1 | **Não curada — FILA**, pelas falhas de `cp`, `cmp` e raspagem do retido já descritas. (`bin/relato.sh:100–125`) |

4. **Testes e guardas sem mordida** [nao verificado]

**FILA — célula `…` passa no teste pelo motivo errado.** Esse caso fica vermelho pelo alfabeto, antes da verificação de célula vazia. Ele não testemunha a guarda de preenchimento; outros casos de pontuação ainda exercitam essa guarda. (`bin/tests/test_mapa20_valida.sh:66–67`; `bin/mapa20_valida.py:145–149`, `bin/mapa20_valida.py:198–200`)

**FILA — “só escreve a saída pedida” compara apenas caminhos.** `arvore()` não captura conteúdo. Uma regravação de arquivo existente, como acrescentar espaço ao JSON, preservaria a lista de caminhos e escaparia dessa asserção. Mutante não executado [não verificado]. (`bin/tests/test_mapa20_valida.sh:354–355`, `bin/tests/test_mapa20_valida.sh:358–375`)

**BLOQUEANTE — testes verdes legitimam fonte inválida.** O título do BACKLOG é a fonte padrão, e o comentário inicial de TICKETS também é aceito como verde. Portanto, a suíte incorpora o significado errado de “fonte do N”. (`bin/tests/test_mapa20_valida.sh:33`, `bin/tests/test_mapa20_valida.sh:55–60`)

Os **156 casos de conteúdo** reproduzidos por stdin deram os resultados esperados, apesar dos contraexemplos novos. Isso não equivale à execução da suíte shell completa, que cria arquivos. (`bin/tests/test_mapa20_valida.sh:25–189`)

5. **Quem escreve, quem lê e significado dos campos**

| Estado | Escritor identificado | Leitor identificado |
|---|---|---|
| `logs/MAPA-20/mapa.md` | O montador escreve no `argv[0]`; chamada efetiva para o caminho vivo [não verificado]. (`bin/mapa20_montar.py:160–161`) | Fotografia e tripwire. (`bin/relato.sh:100–104`) |
| `montador_dados.json` | A instrução atribui edição à sessão; produtor vivo [não verificado]. A fixture escreve esse JSON. (`bin/mapa20_montar.py:7–8`; `bin/tests/test_mapa20_valida.sh:351`) | `json.loads` do montador. (`bin/mapa20_montar.py:89`) |
| `MAPA-20.md` temporário novo | Raspador. (`bin/raspar_publico.py:40`) | Selo; depois cópia ao destino. (`bin/relato.sh:109`, `bin/relato.sh:158`) |
| `MAPA-20.md` público | Cópia do temporário aprovado ou raspagem/rename do retido. (`bin/relato.sh:119–120`, `bin/relato.sh:158`) | Raspagem de retenção e Git. (`bin/relato.sh:119`, `bin/relato.sh:168–179`) |
| BACKLOG | Gerador preserva o bloco manual e substitui o arquivo; `backlog_estado.py` altera a célula de estado. Autor das listas históricas de PRONTO [não verificado]. (`bin/gerar_backlog.py:103–105`, `bin/gerar_backlog.py:138–140`; `bin/backlog_estado.py:58–59`) | Montador localiza chave; selo abre linha citada; publicador raspa o arquivo. (`bin/mapa20_montar.py:58–61`; `bin/mapa20_valida.py:100–105`; `bin/relato.sh:155`) |
| TICKETS | Escritores de topo e rodapé; há também molde de inserção de linha. Autor das listas históricas de PRONTO [não verificado]. (`bin/tickets_placar.sh:82`; `bin/tickets_rodape.sh:78–79`; `bin/molde_autocura/ticket_rows.py:10–12`) | Selo abre a linha citada. (`bin/mapa20_valida.py:100–105`) |

Todos os campos abaixo vêm do JSON; seu escritor operacional permanece [não verificado]. Os consumidores são:

| Campo | Uso efetivo |
|---|---|
| `cabeca` | Prosa inicial. (`bin/mapa20_montar.py:138`) |
| `censo`, `alias_censo` | Leitura das linhas e resolução de nomes. (`bin/mapa20_montar.py:90–105`) |
| `ovr` | Sobrescrita por índice, inclusive identidade. (`bin/mapa20_montar.py:117–118`) |
| `xn[obra]` | Tripla textual X/N, fonte, falta; expansão de referência BACKLOG, sem derivar N. (`bin/mapa20_montar.py:123–124`) |
| `fonte_ausente`, `falta_ausente` | Células das obras sem `xn`. (`bin/mapa20_montar.py:127`) |
| `rascunho.arquivo`, `alias`, `origem` | Contagem do rascunho e anotação na lista superior. (`bin/mapa20_montar.py:65–84`, `bin/mapa20_montar.py:106`, `bin/mapa20_montar.py:132`) |
| Censo colunas 1–8 | Arquivos, cruzamentos, leis, dependência, movimentos, VISIVEL e voltas, conservados após sobrescritas. (`bin/mapa20_valida.py:53–55`; `bin/mapa20_montar.py:117–120`) |
| Censo coluna 9 | Etapa histórica, enviada à prosa; na tabela final, coluna 9 passa a receber X/N. (`bin/mapa20_montar.py:119–124`) |

**BLOQUEANTE — os consumidores não preservam a mesma identidade.** Esta entrada troca os rótulos após a canonicalização, mas continua buscando `xn` pelo nome anterior:

```json
"ovr": {
  "E3": {"0": "E4B"},
  "E4B": {"0": "E3"}
}
```

Com `xn.E3 = 1 de 1` e `xn.E4B = 1 de 2`, o montador produz E4B com dados e progresso de E3, e vice-versa. A sonda em memória do montador real seguida do selo retornou **rc 0**. (`bin/mapa20_montar.py:116–124`; `bin/mapa20_valida.py:188–214`)

O montador ser o **único escritor vivo** continua [não verificado]. `cmp` comprova igualdade naquela comparação; outro escritor produzindo os mesmos bytes não é identificado. (`bin/relato.sh:104–105`)

6. **Classes reincidentes**

| Classe | Resultado |
|---|---|
| Segundo escritor | Existência atual [não verificado]; unicidade não demonstrada pelo tripwire. (`bin/mapa20_montar.py:4–8`; `bin/relato.sh:104–105`) |
| Derivação paralela de fonte única | **BLOQUEANTE:** N declarado no JSON não é derivado da lista citada; apenas o N do rascunho é contado. (`bin/mapa20_montar.py:74–84`, `bin/mapa20_montar.py:123–124`) |
| Selo sem mordida | **BLOQUEANTE:** não exige prova de X nem fundamento quantitativo de N. (`bin/mapa20_valida.py:91–107`, `bin/mapa20_valida.py:209–216`) |
| Relógio fora da borda | **FILA:** fonte e mapa não compartilham fotografia até a publicação. Ocorrência produtiva [não verificado]. (`bin/mapa20_valida.py:100`; `bin/relato.sh:155`) |
| Campo com significados divergentes | **BLOQUEANTE:** `ovr[0]` muda a identidade exibida sem mudar a chave usada para selecionar progresso. (`bin/mapa20_montar.py:116–124`) |

## 11/11 14:33 · sessao a6c6e22ba20e · - · - · origem=Ronald (tmux)

- **BLOQUEANTE — N sem fonte válida, rc 0.** Sonda real, somente memória: todas as obras com `1 de 1`, fonte `app/docs/BACKLOG.md:1`, falta `encerrada`; demais campos `nao medido`, SEM DONO canônica, legenda e cabeçalho oficiais. Resultado: `rc 0`, zero problemas. A fonte é apenas o título do BACKLOG, não PRONTO numerado (`app/docs/BACKLOG.md:1`). O selo confere caminho permitido e linha não vazia, sem relacionar obra, lista e N (`bin/mapa20_valida.py:91-107`). O teste legitima esse título como fonte (`bin/tests/test_mapa20_valida.sh:34`).

- **BLOQUEANTE — “E4B: encerrada” com E4B em 1 de 2, rc 0.** Entrada completa: E3 em `1 de 1`, coluna “arquivos que toca” com `E4B: encerrada`; E4B em `1 de 2`, falta `item 2`; outras linhas canônicas. Sonda real retornou `rc 0`, zero problemas. A permissão acompanha a linha E3, sem identificar a obra afirmada no texto (`bin/mapa20_valida.py:213-214`, `bin/mapa20_valida.py:225-228`).

- **BLOQUEANTE — X não confrontado com prova.** O ramo numérico confere intervalo, fonte e igualdade X/N; não consulta RELATO nem verifica itens comprovados (`bin/mapa20_valida.py:206-216`). A primeira entrada também demonstra aceitação sem apresentar prova de item algum.

- **FILA — caso sem mordida própria.** O caso `celula …` rejeita U+2026 pelo alfabeto antes da guarda de célula vazia (`bin/tests/test_mapa20_valida.sh:70`, `bin/mapa20_valida.py:146-149`). Mutante em memória que faz `_DADO.search` sempre retornar verdadeiro mantém esse caso vermelho; portanto ele não prova a guarda de `bin/mapa20_valida.py:198-200`.

- **Curados:** invisíveis/homóglifos indicados, pelo alfabeto fechado (`bin/mapa20_valida.py:78-79`); separadores de `splitlines`, pelo alfabeto e divisão somente em LF (`bin/mapa20_valida.py:145-150`); “encerrada” em SEM DONO, pela varredura global posterior ao `continue` (`bin/mapa20_valida.py:202-205`, `bin/mapa20_valida.py:225-228`).

Reproduzi os 156 casos de conteúdo construídos pela parte (a), usando `/dev/stdin`: todos deram o resultado esperado. A suíte permanece verde diante dos dois contraexemplos acima; construção e expectativas estão em `bin/tests/test_mapa20_valida.sh:25-189`.

## 11/11 14:33 · sessao 2a97dd5b67c4 · - · - · origem=Ronald (tmux)

BLOQUEANTE — N continua sem derivação da lista de PRONTO: o N aprovado vem da string `d['xn'][nome]`, copiada em `bin/mapa20_montar.py:123-124`. A contagem em `bin/mapa20_montar.py:74-84` aplica-se apenas ao RASCUNHO. O selo aceita caminho permitido + linha não vazia, sem verificar obra, lista numerada ou quantidade (`bin/mapa20_valida.py:91-107`). `app/docs/BACKLOG.md:1` é título; `app/docs/TICKETS.md:1` é comentário. Ambos são aceitos como fontes verdes por `bin/tests/test_mapa20_valida.sh:31` e `bin/tests/test_mapa20_valida.sh:66`. X não lê provas do RELATO: sua única utilização efetiva é comparação aritmética (`bin/mapa20_valida.py:209-215`).

BLOQUEANTE — `ovr` pode trocar a identidade da obra depois da canonicalização: `bin/mapa20_montar.py:116-118`. O `xn` continua escolhido pelo nome anterior (`bin/mapa20_montar.py:123-124`). Sonda em memória confirmou zero problemas para:

```json
"ovr": {
  "E3": {"0": "E4B"},
  "E4B": {"0": "E3"}
}
```

Com censo E3=`dado E3`, E4B=`dado E4B`, `xn.E3=["1 de 1","app/docs/BACKLOG.md:1","nada"]` e `xn.E4B=["1 de 2","app/docs/BACKLOG.md:1","item 2"]`, o montador produz E4B com dados/progresso de E3 e vice-versa; `problemas()` retorna `[]` (`bin/mapa20_montar.py:110`, `bin/mapa20_montar.py:117`, `bin/mapa20_montar.py:124`; execução somente em memória).

FILA — teste “só escreve saída pedida” compara exclusivamente nomes de arquivos: `arvore()` em `bin/tests/test_mapa20_valida.sh:354-355`, captura em `:358`, comparação em `:374`. Mutante concreto após `bin/mapa20_montar.py:152`:

```python
open(os.path.join(RAIZ, DADOS), 'a', encoding='utf-8').write(' ')
```

Modifica JSON existente, preserva JSON válido, saída e lista de caminhos; essa asserção não morde. Mutante não executado [não verificado].

Rastreio de estado:

| Estado/campo | Escritor | Leitor |
|---|---|---|
| `logs/MAPA-20/mapa.md` | Montador grava o `argv[0]` arbitrário, `bin/mapa20_montar.py:160-161`; chamada real para caminho vivo [não verificado] | Foto em `bin/relato.sh:100`; comparação em `:104` |
| `montador_dados.json` | “Sessão edita” é instrução em `bin/mapa20_montar.py:7-8`; escritor real [não verificado]. Teste escreve em `bin/tests/test_mapa20_valida.sh:351` | JSON em `bin/mapa20_montar.py:89` |
| `cabeca` | JSON acima | `bin/mapa20_montar.py:138` |
| `censo` | Prod [não verificado]; fixture `bin/tests/test_mapa20_valida.sh:341` | `bin/mapa20_montar.py:90`, `:93-103` |
| `alias_censo` | JSON | `bin/mapa20_montar.py:104-105`, `:135` |
| `ovr` | JSON | Índices arbitrários em `bin/mapa20_montar.py:117-118` |
| Censo colunas 0–8 | Censo, sobrescritas JSON | Nome em `bin/mapa20_montar.py:116`; demais conservadas por `:120` |
| Censo coluna 9 | Censo/`ovr["9"]` | Etapa em `bin/mapa20_montar.py:119`; removida em `:120` |
| Saída coluna 9 | `xn[0]` | Montador `bin/mapa20_montar.py:124`; selo `bin/mapa20_valida.py:201` |
| `xn` = XdeN/fonte/falta | JSON | Copiado em `bin/mapa20_montar.py:123-124`; selo `bin/mapa20_valida.py:206-218` |
| `fonte_ausente`, `falta_ausente` | JSON | `bin/mapa20_montar.py:127` |
| `rascunho.arquivo/alias/origem` | Prod [não verificado]; fixture `bin/tests/test_mapa20_valida.sh:342-348` | `bin/mapa20_montar.py:106`, `:132` |
| BACKLOG | Gerador `bin/gerar_backlog.py:138-140`, preservando bloco manual `:103-105`; estado em `bin/backlog_estado.py:58-59` | Localiza chave em `bin/mapa20_montar.py:58-61`; fonte em `bin/mapa20_valida.py:100-105`; publicador `bin/relato.sh:155` |
| TICKETS | Topo `bin/tickets_placar.sh:82`; rodapé `bin/tickets_rodape.sh:78-79`; template de linha `bin/molde_autocura/ticket_rows.py:10-12`; autores históricos das listas PRONTO [não verificado] | Fonte em `bin/mapa20_valida.py:100-105`; placa `bin/tickets_placar.sh:64`; contrato `bin/regua_tickets.sh:78` |
| `MAPA-20.md` público | Raspador `bin/raspar_publico.py:40`; regravação retido `bin/relato.sh:119-120`; cópia julgada `:158` | Selo `bin/relato.sh:109`; Git `:168`, `:178` |

Segundo escritor antigo efetivamente removido: [não verificado]. `logs/` não existe nesta cópia; é ignorado em `.gitignore:53`. O código apenas relata a existência anterior em `bin/mapa20_montar.py:4-5`. O tripwire detecta diferença de bytes (`bin/relato.sh:104-105`), não autoria: outro escritor que produza bytes iguais não é identificado por essa comparação.

Classes presentes: derivação paralela N declarado/N contado (`bin/mapa20_montar.py:80`, `:124`), selo sem prova de X/N (`bin/mapa20_valida.py:91-107`, `:209-215`) e identidade com dois consumidores divergentes por `ovr[0]` (`bin/mapa20_montar.py:118`, `:124`). Segundo escritor real e relógio produtivo [não verificado].

## 11/11 14:32 · sessao 0396d9139208 · - · - · origem=Ronald (tmux)

Achados do publicador, por leitura do código; execução em produção [não verificado]. [nao verificado]

- **BLOQUEANTE — bytes publicados sem julgamento no caminho retido.** O caminho novo raspa, julga e copia o mesmo temporário (`bin/relato.sh:106`, `:109`, `:111`, `:158`). Porém, quando retido, o documento público anterior passa apenas pela raspagem e por `mv`, sem selo (`bin/relato.sh:116`, `:119`, `:120`), e entra no commit/push (`bin/relato.sh:167`, `:168`, `:178`, `:179`). Entrada concreta: mapa vivo inválido e `DEST/MAPA-20.md` contendo `E3 encerrada — 1 de 2`; o segundo permanece/republica com rc 1, sem julgamento. O teste aceita até `MAPA VELHO`, sem tabela (`bin/tests/test_mapa20_valida.sh:275`, `:306`, `:307`).

- **FILA — classificação rc 1/2 ainda ambígua.** Falha operacional de `cp`, como EACCES/ENOSPC, vira rc 1 (`bin/relato.sh:100`, `:101`); erro operacional de `cmp` também vira rc 1 (`bin/relato.sh:104`, `:105`). Raspagem do retido falha, remoção funciona e conserva rc 1 (`bin/relato.sh:119`, `:123`, `:125`, `:180`), embora a declaração prometa rc 2 para falha de raspagem e precedência de 2 (`bin/relato.sh:96`, `:97`). O teste explicitamente legitima esse último caso: `raspa_falha … esperado 1` (`bin/tests/test_mapa20_valida.sh:309`).

- **FILA — rc chega, mas não há tratamento específico no chamador da cópia.** O retorno acumulado aparece tanto sem alteração quanto após push (`bin/relato.sh:173`, `:180`); falha de remoção que deixa o mapa presente aborta antes do push com 2 (`bin/relato.sh:123`, `:124`). A unit chama diretamente o script, como oneshot (`bin/hasner-relato.service:13`, `:15`); não declara diferenciação de 1 versus 2, alerta ou leitor que execute reação (`bin/hasner-relato.service:12`). O timer volta periodicamente (`bin/hasner-relato.timer:5`, `:6`); unit/timer vivos [não verificado].

- **FILA — fontes não compartilham a fotografia do mapa.** O selo abre a fonte citada durante julgamento (`bin/mapa20_valida.py:100`, `:101`); depois o publicador manda raspar novamente o BACKLOG vivo (`bin/relato.sh:155`; leitura em `bin/raspar_publico.py:28`). Entrada temporal concreta: inserir uma linha no BACKLOG entre esses passos muda a linha publicada, preservando a citação anteriormente aprovada. Ocorrência real [não verificado].

- **Curado no caminho novo:** regravar o mapa vivo depois do selo não muda o temporário publicado (`bin/relato.sh:100`, `:109`, `:158`); há teste que regrava a origem após o selo e cobra publicação da fotografia (`bin/tests/test_mapa20_valida.sh:244`, `:245`, `:311`). Raspador que retorna 0 sem escrever deixa o arquivo de julgamento ausente, conduzindo a rc 2 (`bin/relato.sh:109`, `:113`; `bin/tests/test_mapa20_valida.sh:262`, `:263`, `:317`). Raspagem que estraga a célula é julgada e retida (`bin/tests/test_mapa20_valida.sh:268`, `:269`, `:318`).

- **Limite da corrida:** o lock de publicação é comum ao produtor de CODEX-ESTADO no caminho de evento (`bin/relato.sh:27`; `bin/codex_estado.sh:65`, `:66`). Isso não fotografa BACKLOG/fontes e não verifica novamente o destino entre `cp`, `git add` e `git commit` (`bin/relato.sh:158`, `:168`, `:178`); outro escritor concreto do MAPA-20 público [não verificado].

## 11/11 14:30 · sessao c243052975dc · TRIAGEM-OFFLINE-SEM-CAMERA · verificador · origem=Code

**NÃO PROCEDE declarar o caminho inteiro fechado.** A cura limita `/hora/` e corrige a prova do aviso/controle dos mutantes; permanecem esperas anteriores à fila e ausência de uma identidade estável para o reenvio. (`app/static/pwa.js:93`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/static/pwa.js:48`)

Chromium, mortes efetivamente executadas, smoke e efeitos em produção: **[nao verificado]**. Não executei o navegador: seu executor cria arquivos e perfil temporários. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:279`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:286`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:295`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:298`)

1. **Fechamento de cada achado da volta 2, por inspeção.**

Na tabela, o veredito qualifica **o fechamento** do achado.

| Achado da volta 2 | Veredito, cura e teste correspondente | [nao verificado]
|---|---|
| Triagem offline escondia confirmar quando a câmera falhava | **PROCEDE.** Botão mostrado antes da abertura e preservado sem stream; M1 reinjeta a ocultação. (`app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:262`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:550`) |
| Triagem online presa em câmera pendente | **PROCEDE.** Abertura limitada, seguida da liberação sem stream; matriz inclui câmera pendente, M3 retira a liberação e Md retira o teto. (`app/static/pwa.js:159`, `app/static/pwa.js:162`; `app/templates/ponto/triagem/apto.html:164`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:55`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:560`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:626`) |
| Triagem online dependia exclusivamente de `toBlob` | **PROCEDE.** Captura central limitada, aguardada antes do submit; matriz e teste direto cobrem callback mudo/conversão falha. (`app/templates/ponto/triagem/apto.html:177`, `app/templates/ponto/triagem/apto.html:231`; `app/static/pwa.js:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:59`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:516`) |
| Triagem offline enfileirava somente dentro do callback | **PROCEDE.** Captura termina antes da chamada independente ao escritor; M4 reinjeta callback sem teto. (`app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:565`) |
| V2 vetava foto nula | **PROCEDE.** Requisitos avisam e prosseguem; M2 reinjeta o veto. (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:298`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:555`) |
| V2 pendurava com `toBlob` mudo e `toDataURL` lançando | **PROCEDE.** Exceção do fallback é absorvida pelo teto; cenário específico e Md cobrem o contrato. (`app/static/pwa.js:138`, `app/static/pwa.js:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:60`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:626`) |
| Barreiras no template legado | **PROCEDE por eliminação.** Seleção atual usa V2/triagem; M6 permanece apenas como histórico, sem ensaio comportamental próprio. (`app/ponto/views.py:356`, `app/scripts/check_a_lei_batida.py:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:524`) |
| Captura/espera duplicadas nas telas | **PROCEDE para a operação da câmera.** A operação fica no PWA; censo exige lista fechada de produtores. Os textos próprios continuam, abaixo. (`app/static/pwa.js:157`, `app/static/pwa.js:168`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:641`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:661`) |
| Mutante com `await` infinito em requisitos | **PROCEDE quanto à prova prevista.** Ma injeta exatamente a espera; agora passa por controle acoplado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:595`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:545`) |
| Mutante sem aviso | **PROCEDE quanto à prova prevista.** Mb troca o aviso por `false` e exige sua falta na tela. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:600`) |
| `throw` no ramo sem foto | **PROCEDE quanto à prova prevista.** Mc reinjeta a exceção e exige ausência de envio. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:605`) |
| PWA não carregou | **NÃO PROCEDE o fechamento offline.** Ambas as telas dependem do escritor ausente; mostram erro sem gravação alternativa. O teste sem PWA continua somente online. (`app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:308`; `app/templates/ponto/bater_ponto_v2.html:332`, `app/templates/ponto/bater_ponto_v2.html:338`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:465`) |
| `estadoPermissao` ainda participava do fluxo | **PROCEDE a retirada do fluxo examinado.** Requisitos consultam captura e GPS diretamente. (`app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:279`) |
| Transporte incompleto de timestamp/GPS | **NÃO PROCEDE.** Sync continua omitindo timestamp corrigido, gap, precisão e instante GPS. (`app/static/pwa.js:188`, `app/static/pwa.js:191`, `app/static/pwa.js:217`, `app/static/pwa.js:226`) |
| Textos de obrigatoriedade contraditórios | **NÃO PROCEDE.** Permanecem na triagem e na V2; os testes procuram o aviso, sem proibir essas mensagens contraditórias. (`app/templates/ponto/triagem/apto.html:66`, `app/templates/ponto/triagem/apto.html:124`, `app/templates/ponto/triagem/apto.html:194`, `app/templates/ponto/triagem/apto.html:250`; `app/templates/ponto/bater_ponto_v2.html:100`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:338`) |
| B1: `/hora/` pendurada antes do `add` | **PROCEDE o fechamento específico da espera HTTP/corpo.** `_noTeto` envolve ambos; FilaRealTest e Me exercitam a fila pelo escritor real. **Não fecha a espera total**, pois IndexedDB permanece sem teto. (`app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:113`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:501`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:610`) |
| B2: aviso invisível em atributo | **PROCEDE.** V2 acrescenta texto; oráculo usa `innerText`; Mf recoloca somente `title`. (`app/templates/ponto/bater_ponto_v2.html:250`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:621`) |
| B2: mutante morto sem controle verde acoplado | **PROCEDE.** `par` junta controle/mutante, exige controle verde/não vazio e exclui sonda sem veredito das mortes. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:451`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:453`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:455`) |

2. **PROCEDE a existência de caminhos sem teto antes da gravação.**

| Caminho | O que acontece antes da fila |
|---|---|
| Triagem online | Clique desabilita botão → GPS com timer de 8 s → selfie limitada → espera de 400 ms → submit nativo. Esse ramo **não possui fallback para a fila** se o envio falhar ou ficar pendente. (`app/templates/ponto/triagem/apto.html:212`, `app/templates/ponto/triagem/apto.html:218`, `app/templates/ponto/triagem/apto.html:231`, `app/templates/ponto/triagem/apto.html:240`, `app/templates/ponto/triagem/apto.html:241`) |
| Triagem offline | Primeiro clique abre confirmação; segundo clique → captura limitada → escritor da fila. Falha de câmera não impede essa chamada; falha do escritor mostra erro, sem gravação alternativa. (`app/templates/ponto/triagem/apto.html:243`, `app/templates/ponto/triagem/apto.html:252`, `app/templates/ponto/triagem/apto.html:276`, `app/templates/ponto/triagem/apto.html:289`, `app/templates/ponto/triagem/apto.html:308`) |
| V2 offline | Clique → selfie limitada → GPS com tentativas de 10 e 12 s → escritor. Esses timers de GPS são próprios, além do teto de selfie/hora. (`app/templates/ponto/bater_ponto_v2.html:262`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:280`, `app/templates/ponto/bater_ponto_v2.html:281`, `app/templates/ponto/bater_ponto_v2.html:332`) |
| V2 online | Após requisitos, espera `fetch` **sem teto**. Somente rejeição chega ao fallback da fila. HTTP de erro espera `r.json()` também sem teto e, depois, retorna sem enfileirar. (`app/templates/ponto/bater_ponto_v2.html:298`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:314`, `app/templates/ponto/bater_ponto_v2.html:316`, `app/templates/ponto/bater_ponto_v2.html:319`) |
| Escritor PWA | `obterGap` → leitura de config → possível medição limitada → leitura/gravação de config → nova abertura do banco → `add` → `tx.oncomplete`. O teto HTTP não engloba essas operações de armazenamento. (`app/static/pwa.js:104`, `app/static/pwa.js:108`, `app/static/pwa.js:113`, `app/static/pwa.js:182`, `app/static/pwa.js:184`, `app/static/pwa.js:194`) |

**`abrirDB` continua sendo uma promessa potencialmente pendente:** resolve em `onsuccess`, rejeita em `onerror`; não possui `onblocked`, teto ou fechamento das conexões abertas. Se a abertura não chegar a nenhum desses dois eventos, `obterGap` já fica esperando antes de alcançar o teto HTTP. A transação da fila também não possui teto nem tratamento explícito de `onabort`. Ocorrência real dessas condições: **[nao verificado]**. (`app/static/pwa.js:48`, `app/static/pwa.js:58`, `app/static/pwa.js:59`, `app/static/pwa.js:74`, `app/static/pwa.js:113`, `app/static/pwa.js:185`, `app/static/pwa.js:195`)

**POST recebido, resposta perdida:**

- Se o `fetch` permanecer pendente, **não entra na fila**; o botão permanece desabilitado. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:295`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:317`)
- Se acabar rejeitando, **tenta entrar na fila**, com timestamp produzido naquele momento. O POST inicial não leva timestamp nem ID estável do clique. (`app/templates/ponto/bater_ponto_v2.html:102`, `app/templates/ponto/bater_ponto_v2.html:304`, `app/templates/ponto/bater_ponto_v2.html:319`; `app/static/pwa.js:182`, `app/static/pwa.js:189`)
- **Não procede “sempre duplica” nem “nunca duplica”.** As APIs deduplicam por **colaborador + tipo + timestamp efetivo em ±5 minutos**. A porta acrescenta mesmo tipo em ±60 segundos e igualdade exata **colaborador + timestamp**; essa última também é a constraint do modelo. Nenhuma é chave do clique original. (`app/api/views.py:539`, `app/api/views.py:542`, `app/api/views_core.py:526`, `app/api/views_core.py:529`; `app/ponto/registro_batida.py:106`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`; `app/ponto/models.py:106`)

Logo, demora antes da rejeição ou relógio desencontrado pode colocar o novo instante fora dessas janelas; o mesmo evento deixa de ser identificado pelo dedup. Duplicação observada em produção: **[nao verificado]**. (`app/static/pwa.js:183`, `app/static/pwa.js:189`; `app/api/views.py:539`; `app/ponto/registro_batida.py:120`)

3. **NÃO PROCEDE confundir os tetos com o gap; PROCEDE a diferença no instante gravado.**

| Valor | Significado, escritor e leitores |
|---|---|
| `HORA_TETO_MS` | Duração de **2000 ms**, declarada uma vez; `calcularESalvarGap` a passa para `_noTeto`, abrangendo fetch e corpo. (`app/static/pwa.js:88`, `app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:98`) |
| `SELFIE_TETO_MS` | Duração **base de 1500 ms**, declarada uma vez; captura lê diretamente e abertura lê multiplicada por dois. Portanto, não é um único teto absoluto para as duas operações. (`app/static/pwa.js:129`, `app/static/pwa.js:162`, `app/static/pwa.js:177`) |
| `timestamp_gap_ms` / `ntp_gap` | Offset assinado em milissegundos: servidor menos estimativa do relógio local. Não é duração de espera nem horário com fuso. (`app/static/pwa.js:101`, `app/static/pwa.js:102`, `app/static/pwa.js:103`, `app/static/pwa.js:190`; `app/ponto/models.py:76`) |

**No teto não se guarda um novo gap:** o código lê o anterior ou usa zero; também não renova `ntp_calculado_em`. Esse é o mesmo fallback da rejeição imediata de rede. Entretanto, `Date.now()` da batida só é lido **depois** dessa espera: `/hora/` pendurada desloca o instante amostrado pelo tempo esperado, aproximadamente 2 s no modelo dos timers. Não há identidade com o instante do clique nem com a execução que rejeitou imediatamente. (`app/static/pwa.js:98`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:182`, `app/static/pwa.js:183`)

**NÃO PROCEDE atribuir outra unidade/fuso ao servidor nesse caminho:** o sincronizador sequer transmite o gap ou o timestamp corrigido. Usa `b.timestamp` para ordenar e envia `timestamp_dispositivo` bruto. A API usada é `/api/ponto/bater/`; ambas as cascas entregam esse bruto a `hora_do_aparelho`. O endpoint offline antigo retorna 410. (`app/static/pwa.js:211`, `app/static/pwa.js:221`, `app/static/pwa.js:227`; `app/api/views.py:518`, `app/api/views_core.py:505`; `app/ponto/views.py:419`, `app/ponto/views.py:426`)

`hora_do_aparelho` aceita ISO ou epoch segundos/ms e decide entre instante bruto e chegada. Os leitores encontrados de `timestamp_gap_ms` mantêm milissegundos: modelo, tela e alerta de skew do **ping geo**, sem aplicação de fuso ao offset. O problema aqui é **transporte omitido**, não conversão nova de unidade. (`app/api/hora_do_aparelho.py:79`, `app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`; `app/ponto/models.py:76`; `app/templates/ponto/batidas_offline_pendentes.html:79`, `app/templates/ponto/batidas_offline_pendentes.html:80`; `app/api/views.py:2340`, `app/api/views.py:2371`)

4. **PROCEDE a mordida específica dos novos testes; NÃO PROCEDE tratá-los como prova de persistência real.** [nao verificado]

| Prova | Avaliação |
|---|---|
| FilaRealTest | Usa o escritor real e testa hora normal, fetch pendente e corpo pendente. Corrige a substituição que escondia B1. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:214`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:437`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:477`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:501`) |
| Me | `1e9` impede o teto dentro dos 15 s do ensaio; exige ausência de fila diretamente e nas duas telas offline, com controle verde acoplado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:439`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:610`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:616`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:618`) |
| Stub que ignora abort | Não dá passagem indevida: obriga `_noTeto` a resolver independentemente do abort. A promise HTTP original pode continuar pendente sem segurar o chamador limitado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:116`; `app/static/pwa.js:138`, `app/static/pwa.js:140`) |
| Mf | `title` não satisfaz `innerText`. Para câmera rejeitada, o aviso precisa estar presente antes do clique; o toast posterior não salva o mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:190`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:342`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:621`) |
| `_Base.par` | Controle e mutante são processos separados submetidos ao mesmo lote; controle vermelho impede declarar morte, e sonda sem veredito é filtrada. Promessa pendente **com veredito da sonda** continua sendo falha válida. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:298`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:318`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:351`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:455`) |

**Há um caso que o selo aceita sem provar a conclusão:** o fake marca fila dentro do `add`, antes de `tx.oncomplete`; `falhas_da_fila` não exige “fila resolveu”. Um escritor pendurado depois desse `add` satisfaria o oráculo, embora a função real ainda estivesse aguardando conclusão. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:135`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:151`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:215`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:484`; `app/static/pwa.js:194`)

O fake sempre abre com sucesso e não reproduz bloqueio, aborto ou rollback; os timers rodam em tempo virtual. O caso online novo injeta rejeição imediata, sem representar POST pendurado ou servidor que gravou antes da rejeição. Persistência nativa e comportamento em tempo real: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:109`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:160`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:173`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:299`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:506`)

A ressalva do frame “ok” permanece: stream vazio, dimensões falsificadas e `drawImage` real. Se isso falhar, agora o controle vermelho impede uma falsa morte; sucesso efetivo desse controle: **[nao verificado]**. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:80`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:87`, `app/ponto/tests/test_selfie_nao_barra_chromium.py:452`; `app/static/pwa.js:172`)

5. **Escritores, leitores e autoridades.**

Além dos dois tetos descritos no item 3, os contratos relevantes são:

| Campo/valor | Quem escreve → quem lê |
|---|---|
| `/hora/`: `hora`, `data`, `timestamp` | `hora_servidor` produz strings de apresentação e ISO com offset → PWA usa timestamp para epoch-ms; relógios das telas usam hora/data. (`app/core/views.py:14`, `app/core/views.py:20`; `app/static/pwa.js:102`; `app/templates/ponto/bater_ponto_v2.html:169`; `app/templates/ponto/triagem/base_triagem.html:46`) |
| `t0`, `t1`, `medida`, `latencia`, `gap` | Medição PWA captura relógios/corpo e deriva um offset → grava config e devolve ao escritor. (`app/static/pwa.js:94`, `app/static/pwa.js:96`, `app/static/pwa.js:97`, `app/static/pwa.js:101`, `app/static/pwa.js:104`, `app/static/pwa.js:182`) |
| `ntp_gap`, `ntp_calculado_em` | `calcularESalvarGap`, via `salvarConfig`, escreve → `obterGap` lê validade de 30 min; fallback lê gap anterior. (`app/static/pwa.js:67`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:113`) |
| `{stream, aviso}` / `{foto, aviso}` | Operação central e `_respostaSelfie` produzem → triagem/V2 leem controles, foto e aviso. (`app/static/pwa.js:145`, `app/static/pwa.js:163`, `app/static/pwa.js:178`; `app/templates/ponto/triagem/apto.html:155`, `app/templates/ponto/triagem/apto.html:233`; `app/templates/ponto/bater_ponto_v2.html:250`, `app/templates/ponto/bater_ponto_v2.html:275`) |
| `_fotoBlob` / foto anterior | Input V2 e captura escrevem → captura preserva anterior; POST/fila transportam; sync anexa somente se presente. (`app/templates/ponto/bater_ponto_v2.html:255`, `app/templates/ponto/bater_ponto_v2.html:274`, `app/templates/ponto/bater_ponto_v2.html:306`, `app/templates/ponto/bater_ponto_v2.html:332`; `app/static/pwa.js:178`, `app/static/pwa.js:226`) |
| Registro IndexedDB | `registrarBatidaOffline` escreve tipo, horários, gap, coordenadas, precisão, instante GPS e foto → sync lê; horário corrigido apenas ordena, e três metadados não são transportados. (`app/static/pwa.js:187`, `app/static/pwa.js:211`, `app/static/pwa.js:217`, `app/static/pwa.js:226`) |
| `id` da fila | IndexedDB gera por autoincremento → sync usa para apagar localmente; não vira ID enviado ao servidor. (`app/static/pwa.js:54`, `app/static/pwa.js:217`, `app/static/pwa.js:236`) |
| `C.hora`, `C.post`, `C.fim` | Construtor de cenário escreve → fetch fake seleciona resposta/rejeição e timer emite veredito. (`app/ponto/tests/test_se

[truncado em 20000 de 24229 caracteres]

## 11/11 14:29 · sessao 9ba71d54c610 · GABARITO-FABLE-SOMBRA-AJUSTE · verificador · origem=Code

**BLOQUEANTE — alternativa (a), pela leitura literal de que o RED deve evidenciar o defeito pelo selo definido no aval.** O aval exige painel renderizado × API para o mesmo colaborador; a PROPORCAO mantém “RED e GREEN”; a LEI-AKITA 5 determina: “Cura sem teste vermelho evidenciado nao existe”. (`aval_ui_painel_raspado.txt:10–11`; `aval_proporcao_por_risco_1111.txt:1`, item 2; `CLAUDE.md:28–29`). [nao verificado]

- **Não há evidência fornecida do ORM vermelho pelo defeito da obra antes da cura.** A docstring registra cinco falhas **em memória**, mas declara que esse selo não certifica ORM e que o “RED visual/ORM antes da cura NAO [foi] executado nesta copia”. (`app/colaboradores/tests/test_ui_painel_mostra_o_app.py:3–11`).
- O único log ORM mostra quatro falhas por **“fixture invisivel”**, inclusive com mutante. No código, essa asserção está no `setUp`, antes da instalação do mutante e da renderização; o comentário atribui a ela os quatro RED v1. Isso evidencia falha de preparação, não ausência do rótulo/lista. (`v1_orm_falhas.txt:13–31`; `app/colaboradores/tests/test_ui_painel_mostra_o_app_orm.py:58–61`, `:93–105`).
- A L-006 explicita o precedente: “RED que nao se le nao vale como evidencia (LEI-AKITA 5)”, com reconstrução contra o código anterior. (`app/docs/LEIS.md:64`).
- O corpo do commit `984ec6938` ficou **[não verificado]**: `git log` não pôde consultá-lo nesta cópia sem repositório Git. **[não verificado]**

**A alternativa (b) não cabe nessa leitura:** a fila cobre “como o selo poderia ser enganado”; aqui falta comprovar o RED obrigatório do selo contratado. O vermelho em memória não o substitui, conforme a própria docstring. (`aval_proporcao_por_risco_1111.txt:1`, item 2; `app/colaboradores/tests/test_ui_painel_mostra_o_app.py:3–4`).

*Referências aos anexos contam linhas desde a primeira linha do conteúdo fornecido.*

## 11/11 14:25 · sessao 0be98c4a67e3 · - · - · origem=Ronald (tmux)

**Servidor, timestamps e reenvio**

- **PROCEDE: POST online pode ficar indefinidamente antes da fila.** V2 espera `fetch` sem teto; só entra na fila no `catch`. Resposta pendente não executa o `catch`, e `_enviando`/botão permanecem bloqueados. HTTP de erro vai para `r.json()`, também sem teto, e depois retorna sem enfileirar. (`app/templates/ponto/bater_ponto_v2.html:294`, `app/templates/ponto/bater_ponto_v2.html:308`, `app/templates/ponto/bater_ponto_v2.html:314`, `app/templates/ponto/bater_ponto_v2.html:317`)
- **Se POST recebido terminar em rejeição no cliente**, V2 enfileira; se apenas permanecer pendente, não enfileira. A fila calcula novo instante depois de `obterGap`; o POST inicial não carrega instante nem identificador estável do evento: o formulário tem tipo/coordenadas/foto e o envio só acrescenta precisão/foto. Portanto, a mesma tentativa não preserva uma chave de idempotência de ponta a ponta. (`app/templates/ponto/bater_ponto_v2.html:102`, `app/templates/ponto/bater_ponto_v2.html:304`, `app/templates/ponto/bater_ponto_v2.html:319`, `app/static/pwa.js:182`)
- **Não é correto dizer “sempre duplica” ou “nunca duplica”.** O dedup API procura `(colaborador, tipo)` com timestamp em ±5 minutos do instante efetivo e retorna `ja_registrada`; as duas cascas fazem isso. É proximidade temporal, não igualdade por ID do clique. Se a nova hora cair fora da janela — por demora antes de rejeitar ou relógio bruto desencontrado — essa primeira barreira não identifica a mesma tentativa. Incidência real: **[nao verificado]**. (`app/api/views.py:539`, `app/api/views.py:541`, `app/api/views.py:549`, `app/api/views_core.py:526`, `app/api/views_core.py:528`; `app/static/pwa.js:183`, `app/static/pwa.js:189`)
- A porta oferece mais duas barreiras: mesmo colaborador/tipo não retratado em ±60 s; depois igualdade exata `(colaborador, timestamp)`, inclusive outro tipo ou retratada. Colisão de tipo oposto usa 60 ou 300 s, salvo marco próprio; a constraint do modelo é `(colaborador, timestamp)`. Nenhuma dessas chaves é o ID do clique original. (`app/ponto/registro_batida.py:21`, `app/ponto/registro_batida.py:106`, `app/ponto/registro_batida.py:113`, `app/ponto/registro_batida.py:120`, `app/ponto/registro_batida.py:138`, `app/ponto/models.py:106`)

**Gap, unidades e leitores**

- `HORA_TETO_MS` é duração de espera em milissegundos, escrita uma vez em 2000 e lida na chamada `_noTeto` que engloba fetch **e** `json()`. Não é o offset `timestamp_gap_ms`. (`app/static/pwa.js:88`, `app/static/pwa.js:93`, `app/static/pwa.js:97`, `app/static/pwa.js:98`)
- `SELFIE_TETO_MS` é duração base de 1500 ms; abertura lê multiplicada por dois, captura lê diretamente. O mecanismo `_noTeto` resolve timeout/erro, sem depender de o abort encerrar a promise original. (`app/static/pwa.js:129`, `app/static/pwa.js:134`, `app/static/pwa.js:138`, `app/static/pwa.js:140`, `app/static/pwa.js:162`, `app/static/pwa.js:177`)
- No teto da hora **não se grava um novo gap**: cai no `catch` e lê `ntp_gap || 0`. A fórmula corrigida permanece `Date.now()+gap`; assim, não surge conversão nova de unidade/fuso, mas uma espera de até 2 s antes da leitura do instante desloca a hora gravada frente à rejeição imediata sem rede. Não se preserva a hora do clique antes da espera. (`app/static/pwa.js:99`, `app/static/pwa.js:104`, `app/static/pwa.js:108`, `app/static/pwa.js:182`, `app/static/pwa.js:183`)
- Escritor dos configs: `calcularESalvarGap`, via `salvarConfig`, grava `ntp_gap` como diferença de epoch-ms e `ntp_calculado_em` como epoch-ms local. Leitores: `obterGap` decide validade de 30 min e recupera offset; fallback da própria medição recupera o offset. (`app/static/pwa.js:63`, `app/static/pwa.js:101`, `app/static/pwa.js:103`, `app/static/pwa.js:104`, `app/static/pwa.js:105`, `app/static/pwa.js:108`, `app/static/pwa.js:112`)
- **PROCEDE, transporte continua incompleto.** Fila grava timestamp ISO corrigido, timestamp ISO bruto, gap-ms, precisão e instante GPS; sync usa `b.timestamp` só para ordenação e transmite apenas tipo/coordenadas/timestamp bruto/foto/liveness, omitindo corrigido/gap/precisão/instante GPS. (`app/static/pwa.js:188`, `app/static/pwa.js:189`, `app/static/pwa.js:190`, `app/static/pwa.js:191`, `app/static/pwa.js:211`, `app/static/pwa.js:217`, `app/static/pwa.js:226`)
- API de sincronização efetivamente usada é `/api/ponto/bater/`, roteada para `api_bater_ponto`; endpoint antigo retorna 410. As duas cascas passam somente `timestamp_dispositivo` para `hora_do_aparelho` e gravam `ts_efetivo`/`ts_aparelho`, sem aplicar gap da fila. (`app/static/pwa.js:227`, `app/api/urls.py:67`, `app/ponto/views.py:419`, `app/ponto/views.py:426`, `app/api/views.py:518`, `app/api/views.py:666`, `app/api/views.py:678`, `app/api/views_core.py:505`, `app/api/views_core.py:658`, `app/api/views_core.py:670`)
- `hora_do_aparelho` aceita ISO e epoch em segundos/ms, torna naive aware, aceita hora até 48 h no passado e usa chegada para futuro/antiga demais. Isso opera sobre **timestamp bruto**, não pressupõe unidade/fuso de `timestamp_gap_ms`. (`app/api/hora_do_aparelho.py:72`, `app/api/hora_do_aparelho.py:79`, `app/api/hora_do_aparelho.py:85`, `app/api/hora_do_aparelho.py:116`, `app/api/hora_do_aparelho.py:141`, `app/api/hora_do_aparelho.py:153`)
- Modelo define gap como milissegundos; tela mostra `ms` e compara com ±300000; JSON de detalhes entrega `gap_ms` sem conversão. As leituras API encontradas de `timestamp_gap_ms` são de **ping geo**, comparando ±300000 para aviso de skew; não são o kernel de bater/sincronizar e não aplicam fuso ao gap. (`app/ponto/models.py:76`, `app/templates/ponto/batidas_offline_pendentes.html:77`, `app/templates/ponto/batidas_offline_pendentes.html:79`, `app/templates/ponto/batidas_offline_pendentes.html:80`, `app/core/views.py:1022`, `app/api/views.py:2327`, `app/api/views.py:2340`, `app/api/views.py:2371`, `app/api/views_core.py:874`, `app/api/views_core.py:905`)

**Autoridades e derivação**

- Juiz registrado de selfie decide **ausência cobrável**, recebendo origem/canal/foto/duração; PWA = 0 min, APK/desconhecido = 90 min. Isso não é o timeout de câmera nem veto à gravação. As duas APIs consultam esse juiz sob foto ausente e depois chamam a porta de registro; foto é anexada condicionalmente. (`app/core/juizes.py:1192`, `app/core/juizes.py:1195`, `app/ponto/registro_batida.py:72`, `app/ponto/registro_batida.py:75`, `app/ponto/registro_batida.py:80`, `app/api/views.py:449`, `app/api/views.py:461`, `app/api/views.py:666`, `app/api/views.py:781`; `app/api/views_core.py:436`, `app/api/views_core.py:448`, `app/api/views_core.py:658`)
- Juiz GPS registrado considera zero/par parcial sem GPS e é consultado pelo geofence depois da gravação; adquirir coordenadas ou medir frescura no front é outra pergunta. (`app/core/juizes.py:1196`, `app/core/juizes.py:1201`, `app/ponto/services/geofence.py:126`, `app/ponto/services/geofence.py:145`, `app/ponto/services/geofence.py:269`, `app/api/views.py:789`)
- **Derivação paralela já existente:** V2 calcula distância e `dentro = d <= POSTO.raio` para badge próprio apesar do juiz registrado `classificar_posicao`. Esse trecho não é introduzido pelo delta fornecido; não confundir com a cura do timeout. (`app/templates/ponto/bater_ponto_v2.html:199`, `app/templates/ponto/bater_ponto_v2.html:217`, `app/templates/ponto/bater_ponto_v2.html:218`, `app/core/juizes.py:1191`)
- Textos contraditórios continuam: V2 exige verbalmente “Tire a selfie para registrar”, embora `garantirRequisitos` só avise e continue com foto ausente. (`app/templates/ponto/bater_ponto_v2.html:100`, `app/templates/ponto/bater_ponto_v2.html:269`, `app/templates/ponto/bater_ponto_v2.html:276`)

## 11/11 14:25 · sessao f9c39a062fdf · - · - · origem=Ronald (tmux)

- **Textos contraditórios: NÃO fechado.** Triagem mantém “Tire sua selfie para liberar o ponto”, “Selfie obrigatória” e “Selfie obrigatoria — registre o ponto”; V2 mantém “Tire a selfie para registrar”, apesar de a fila aceitar foto nula. (`app/templates/ponto/triagem/apto.html:66`, `:124`, `:194`, `:250`; `app/templates/ponto/bater_ponto_v2.html:100`; `app/static/pwa.js:192`)

- **PWA ausente offline: NÃO fechado.** As duas telas dependem de `JulianiPWA.registrarBatidaOffline`; ausência do objeto cai no erro e exige nova tentativa, sem gravação alternativa. O teste sem PWA continua restrito ao online. (`app/templates/ponto/triagem/apto.html:289`, `:308`; `app/templates/ponto/bater_ponto_v2.html:319`, `:325`, `:332`, `:338`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:465`)

- **Legado eliminado: mantido.** A seleção de template retorna triagem ou V2; o comentário do verificador registra a exclusão de `ponto/bater_ponto.html`. O nome textual permanece no ticket histórico. (`app/ponto/views.py:225`, `:356`; `app/scripts/check_a_lei_batida.py:20`; `app/docs/TICKETS.md:693`)

- **`estadoPermissao`: não participa do fluxo examinado.** `garantirRequisitos` chama diretamente a captura central e as tentativas limitadas de GPS. (`app/templates/ponto/bater_ponto_v2.html:269`, `:274`, `:279`)

- **Teto da hora tem declaração única, significado de duração em milissegundos e um consumidor operacional:** `calcularESalvarGap` passa `HORA_TETO_MS` a `_noTeto`, envolvendo fetch e leitura do corpo. A V2 e a casca de triagem conservam literais independentes de 2000 ms para relógios de apresentação; eles não leem a constante e não calculam o gap persistido. Há parametrização paralela, mas são saídas distintas; não constitui, por si, duplicação do juiz registrado de selfie/GPS. (`app/static/pwa.js:88`, `:93`, `:98`, `:103`; `app/templates/ponto/bater_ponto_v2.html:160`, `:167`, `:169`; `app/templates/ponto/triagem/base_triagem.html:38`, `:41`, `:46`; `app/core/juizes.py:1195`, `:1201`)

- **`SELFIE_TETO_MS` é uma duração base, não um teto absoluto único para toda operação:** há uma declaração de 1500 ms; captura lê diretamente e abertura lê multiplicado por dois. Essa diferença é explícita no código e no oráculo. (`app/static/pwa.js:129`, `:162`, `:177`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:359`)

- **O fallback no teto não grava gap novo.** Sucesso escreve `ntp_gap` e `ntp_calculado_em`; timeout apenas lê o gap anterior ou devolve zero. Sem medição válida anterior, cada nova obtenção continua elegível à consulta, pois o fallback não renova `ntp_calculado_em`. (`app/static/pwa.js:104`, `:105`, `:108`, `:113`)

- **Timestamp não é o instante do clique.** O escritor espera `obterGap()` e somente depois lê `Date.now()` para `timestamp`; lê outro relógio para `timestamp_dispositivo`. Assim, para o mesmo gap, uma `/hora/` pendurada faz o timestamp refletir a espera de aproximadamente dois segundos, enquanto rejeição imediata segue antes. O comentário “caminho de hoje sem rede” descreve o valor do fallback, não identidade temporal das duas execuções. (`app/static/pwa.js:86`, `:98`, `:108`, `:182`, `:183`, `:189`)

- **Os testes novos não verificam esse valor temporal.** `falhas_da_fila` julga chegada, duração até `add` e presença de foto; não compara `timestamp`, `timestamp_dispositivo`, gap nem instante do clique. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:484`, `:490`, `:493`)

- **Autoridade servidor continua distinta:** `selfie_ausente` recebe dados e duração, sem ler relógio; decide cobrança vencida conforme canal, não admissibilidade do clique. `_respostaSelfie` produz foto/stream e aviso para as telas. Não há duplicação desse predicado pelo delta examinado. (`app/ponto/registro_batida.py:67`, `:72`, `:80`, `:86`; `app/static/pwa.js:144`; `app/core/juizes.py:1193`)

- **Aviso da V2 agora usa o mesmo valor produzido pelo PWA:** `_respostaSelfie` escreve `aviso`; `abrirCamera` o lê em `span.textContent`; o ramo de captura também o lê no toast. Foto anterior é escrita pelo input e passada à captura central. (`app/static/pwa.js:145`, `:178`; `app/templates/ponto/bater_ponto_v2.html:250`, `:255`, `:274`, `:276`)

- **Duas cascas incluem PWA**, porém o teste força a casca pelo loader, não demonstra a seleção real de casca em requisição de produção. (`app/templates/base.html:20`; `app/templates/base_app.html:20`; `app/ponto/tests/test_selfie_nao_barra_chromium.py:255`, `:258`)

Execução do navegador, smoke real, hash do arquivo em produção e efeitos em produção: **[nao verificado]**.

## 11/11 14:25 · sessao 7fb9cd9072df · - · - · origem=Ronald (tmux)

**B1 e B2: a nova prova tem mordida para os defeitos específicos, por inspeção; execução e morte efetiva dos mutantes são [nao verificado].** O executor escreve página, script e perfil temporários, portanto não o executei sob a restrição de somente leitura. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:279`, `:286`, `:295`, `:298`)

- **FilaRealTest / Me — PROCEDE a correção da lacuna anterior.** A sonda já não substitui `registrarBatidaOffline`: chama o escritor real e observa o `add` em `batidas_pendentes`; exercita `/hora/` normal, fetch pendurado e corpo pendurado. Me troca o teto por `1e9` e exige ausência de chegada à fila, tanto diretamente quanto nas duas telas offline. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:146`, `:151`, `:214`, `:437`, `:477`, `:501`, `:610`)

- **O stub que ignora abort fortalece a pergunta do teto**, porque `/hora/` permanece pendurada mesmo após `ctl.abort()`: o controle depende da resolução de `_noTeto`, e o mutante perde essa resolução dentro dos 15 segundos do ensaio. O corpo pendurado também está dentro do trabalho limitado. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:113`, `:116`, `:119`, `:439`; `app/static/pwa.js:93`, `:97`, `:98`, `:139`)

- **Mf — PROCEDE a correção do aviso invisível.** O oráculo consulta `body.innerText`, enquanto o mutante substitui o `appendChild` visível por `title`. A câmera rejeitada precisa mostrar o aviso **antes de liberar o clique**; por isso o toast posterior de `garantirRequisitos` não salva o mutante. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:100`, `:190`, `:342`, `:621`; `app/templates/ponto/bater_ponto_v2.html:250`, `:277`)

- **`_Base.par` — PROCEDE a correção do controle desacoplado.** Controle e mutante entram no mesmo `roda`; controle com qualquer falha ou sem cenário reprova; resultados “sonda não devolveu veredito” e “juiz sem veredito” são retirados das mortes. Os chamadores exigem mortes e padrão específico. “Mesmo lote” significa execuções Chromium separadas, cada qual com seu perfil temporário, submetidas ao mesmo executor paralelo. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:450`, `:451`, `:453`, `:455`, `:547`, `:548`, `:279`, `:298`, `:318`)

**Limitações concretas:**

- **“FilaReal” não prova persistência real nem conclusão do escritor.** O fake grava em objeto JS e marca sucesso dentro do `add`; `tx.oncomplete` vem depois. O oráculo verifica chegada, tempo e foto, mas não exige “fila resolveu”, embora o escritor real espere `tx.oncomplete`. Um cenário poderia passar tendo alcançado o fake `add` e ficando pendurado antes da conclusão da promessa. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:135`, `:150`, `:151`, `:215`, `:484`; `app/static/pwa.js:194`)

- **O fake encobre abertura bloqueada e falhas de armazenamento.** Seu `open` sempre chama sucesso após 1 ms; não modela upgrade bloqueado, quota, aborto ou rollback. O `abrirDB` real espera somente `onsuccess`/`onerror`, sem `onblocked` nem teto; `obterGap` já o espera antes da pergunta HTTP. Assim, a declaração ampla “do chamado ao add a espera é limitada” excede a prova e o código. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:160`, `:173`, `:473`; `app/static/pwa.js:48`, `:58`, `:59`, `:74`, `:113`, `:182`)

- **O caso V2 online cobre rejeição imediata, não POST pendurado ou resposta perdida.** O stub só retorna rejeição imediata ou resposta 200 imediata; não há cenário de servidor que recebe e deixa a resposta pendente. O `fetch` real anterior à fila não tem teto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:107`, `:109`, `:111`, `:506`; `app/templates/ponto/bater_ponto_v2.html:308`, `:319`)

- **O relógio do ensaio é virtual e o armazenamento é substituído justamente para acompanhá-lo.** Isso prova os timers JS exercitados nesse modelo; tempo real, suspensão da aba e durabilidade IndexedDB são [nao verificado]. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:126`, `:177`, `:299`)

- **Controle de câmera “ok”: ressalva ainda existente, resultado [nao verificado].** A sonda fornece `MediaStream()` vazio e falsifica dimensões; mantém `drawImage(video)` real. Uma falha nesse desenho poderia impedir alcançar `toBlob`; com `_Base.par`, porém, isso torna o controle vermelho e impede declarar o mutante morto. (`app/ponto/tests/test_selfie_nao_barra_chromium.py:80`, `:87`, `:89`, `:452`; `app/static/pwa.js:172`)

## 11/11 14:10 · sessao a833efdfbdac · FIO-E0-V4-E4B · construtor · origem=Code

Quatro correções implementadas: dia pelo juiz existente, trava por âncora, texto limitado e coleta confirmada após savepoint. [Recibo completo](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c7.txt:20).

Observado em memória: quatro REDs na c6, seis mutantes detectados e 21 testes OK. [Resultados e comandos](/home/ronald/wt-codex-fio-e0-v4-e4b-005824/app/docs/FIO-E0-V4-E4B-c7.txt:115). [nao verificado]

ORM não executado; comandos no recibo. Certificação Code/opus e concorrência real: [nao verificado].

_3916 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
