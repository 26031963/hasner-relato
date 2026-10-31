# CODEX-ESTADO

> Ultima resposta processada: **31/10/2026 03:25** · publicado em **31/10/2026 03:25** (UTC-3)
> So as RESPOSTAS FINAIS do Codex (auxiliar de leitura; nunca escreve). Afirmacao de execucao sem hash ou caminho ao lado sai `[nao verificado]`.

## 31/10 03:25 · sessao c8b173220787 · - · - · origem=Ronald (tmux)

Leitores localizados e diferenças:

- **ESTADO usa uma leitura única:** `bin/gerar_estado.py:260-261` importa e chama `fio_estado.ler` e `linha`; o acesso ao arquivo acontece em `bin/fio_estado.py:64-65`. O CLI JSON e `--linhas` passam pelo mesmo `ler` (`bin/fio_estado.py:125-129`).
- **Não localizei outro leitor operacional do arquivo na cópia**, após buscas incluindo ocultos/ignorados, variantes do nome e construções de caminho; a existência de consumidores externos ou caminhos inteiramente dinâmicos é **[nao verificado]**. Evidência positiva dos consumidores identificados: `bin/gerar_estado.py:260-261`, `bin/fio_estado.py:64-65`.
- **O command possui regra diferente, mas não lê o carimbo:** calcula `d` e `alc` diretamente (`app/chamados/management/commands/vigiar_fio_morto.py:167-168`), calcula `gritou` somente sobre `d` (`:170-171`) e escreve o carimbo posteriormente (`:205-221`). O selo F5 exige explicitamente `fio_morto_alcance` fora de `ALARMA_EM_0` (`app/chamados/tests/test_o261e_f5_alcance.py:454-455`).

Falhas no leitor atual:

- `alcance`, `alcance_cego` e `alcance_universo` não são extraídos; o retorno contém somente campos do censo antigo (`bin/fio_estado.py:69-92`). `fio_morto_alcance` entra apenas indiretamente na comparação genérica de `esperado` (`:72-73`).
- A manchete imprime **`censo.fio_morto`**, com seu esperado antigo (`bin/fio_estado.py:101-104`). O aviso CEGO lê **`censo.cego`**, não `alcance.alcance_cego` (`:91`, `:109-111`).
- Carimbo anterior ao F5 pode produzir `medido=True`, zero antigo e nenhuma divergência: não se exige `alcance`; percorrem-se somente chaves existentes em `esperado` (`bin/fio_estado.py:69-73`, `:82-92`, `:101-104`). As próprias fixtures atuais representam esse formato e chamam zero antigo de VERDE (`bin/tests/test_fio_estado.sh:40-47`, `:56-60`).
- Se faltar `esperado`, ele vira `{}` e nenhuma divergência é calculada (`bin/fio_estado.py:70-73`). Se faltar só `esperado.fio_morto_alcance`, a comparação tampouco cobra esse campo (`:72-73`). Se faltar o contador e seu esperado existir, `None != 0` entra em `fora_do_esperado`, mas a manchete ainda imprime o número antigo (`:72-73`, `:101-108`).
- `medido_em` ausente/inválido não invalida a medição: idade fica `None`, `velho=False` e o número permanece, acompanhado de “idade nao medivel” (`bin/fio_estado.py:74-84`, `:112-117`). A idade é arredondada antes de comparar com 26 h, podendo atrasar ligeiramente a marcação de velho (`:79`, `:84`).
- Arquivo ausente, erro de I/O e JSON inválido caem todos no mesmo retorno `medido=False` e texto **NUNCA MEDIDO**, sem linha ILEGIVEL (`bin/fio_estado.py:63-68`, `:97-100`). Erros de forma/tipo após `json.load` podem escapar: `.get`/`.items` fora do `try`, ou `fromisoformat` com valor não textual, pois o segundo `except` captura apenas `ValueError` (`:69-80`).

Sobre o selo:

- O selo atual verifica zero/N antigo, idade, ausência, limiar variável, caminho e `censo.cego`; não testa alcance, universo vazio, recorte no conteúdo, esperado ausente, JSON inválido nem mutante da manchete nova (`bin/tests/test_fio_estado.sh:36-109`).
- Mesmo hoje, o caso CEGO só exige a palavra na saída; não exige supressão do número (`bin/tests/test_fio_estado.sh:104-107`), e o compositor mantém o número antes do aviso (`bin/fio_estado.py:101-111`).
- Há leitores diretos **de fixtures nos testes Django**, não consumidores alternativos de produção: `app/chamados/tests/test_o261_vigia_fio.py:775`, `:830`; `app/chamados/tests/test_o261e_f5_alcance.py:259`, `:275`, `:449`; `app/chamados/tests/test_o261_resposta_vazia.py:157-159`.

## 31/10 03:25 · sessao 6a1beaaa659b · - · - · origem=Ronald (tmux)

O selo atual aceita precisamente o carimbo sem `alcance` que o contrato quer reprovar: fixtures não têm as chaves F5 e o caso 1 exige resultado “verde” (`bin/tests/test_fio_estado.sh:40-47`, `bin/tests/test_fio_estado.sh:56-60`).

- A manchete continua em `censo.fio_morto`; nenhum campo de `alcance` integra o retorno de `ler` (`bin/fio_estado.py:69-92`, `bin/fio_estado.py:101-104`).
- `esperado` ausente vira `{}`; nenhuma comparação acontece, embora `medido=True` e a manchete possa apresentar `None` (`bin/fio_estado.py:70-73`, `bin/fio_estado.py:82-86`, `bin/fio_estado.py:101-104`).
- `medido_em` ausente/inválido mantém idade desconhecida e `velho=False`; não impede número na manchete (`bin/fio_estado.py:74-84`, `bin/fio_estado.py:101-104`). Carimbo antigo válido recebe advertência, mas conserva a manchete numérica (`bin/fio_estado.py:112-117`).
- `cego` legado só acrescenta uma nota; não muda a manchete nem `fora_do_esperado`, e `alcance_cego` é ignorado (`bin/fio_estado.py:72-73`, `bin/fio_estado.py:91`, `bin/fio_estado.py:109-111`).
- Arquivo ausente, falha de abertura e JSON inválido recebem igualmente “NUNCA MEDIDO”; não há saída “ILEGIVEL” (`bin/fio_estado.py:63-68`, `bin/fio_estado.py:97-100`).
- JSON válido de formato errado pode levantar `AttributeError`; `medido_em` não textual pode levantar `TypeError`, fora das capturas existentes (`bin/fio_estado.py:69-80`). A chamada do gerador não protege esta seção, podendo interromper a publicação antes da escrita final (`bin/gerar_estado.py:260-261`, `bin/gerar_estado.py:364`).

O selo valida principalmente `medido`, `velho` e nomes de divergências; não verifica a manchete nos casos numéricos. Só procura textos para ausência e cegueira; assim, um mutante que devolva a manchete ao contador legado pode passar (`bin/tests/test_fio_estado.sh:51`, `bin/tests/test_fio_estado.sh:56-91`, `bin/tests/test_fio_estado.sh:104-107`). O comentário sobre mutações não corresponde a um mecanismo de mutação implementado nesse script (`bin/gerar_estado.py:259`, `bin/tests/test_fio_estado.sh:19-109`).

Os testes F5 já documentam a diferença essencial: legado zero com alcance um; universo vazio cego; carimbo com ambos os locais do contador novo e esperado zero. Esses testes não exercitam o leitor host (`app/chamados/tests/test_o261e_f5_alcance.py:425-434`, `app/chamados/tests/test_o261e_f5_alcance.py:443-455`).

Não executei os testes, pois criam e apagam arquivos temporários (`bin/tests/test_fio_estado.sh:32-34`, `bin/tests/test_fio_estado.sh:48`, `bin/tests/test_fio_estado.sh:75`). Resultado efetivo de execução: [nao verificado].

## 31/10 03:25 · sessao b569565a3261 · - · - · origem=Ronald (tmux)

Escritor e origem dos campos:

| Campo | Quem produz | Quem persiste |
|---|---|---|
| `censo.fio_morto_alcance` | `censo_alcance()` inicializa em 0 e incrementa para cada dia sem porta após as exclusões de hoje/futuro e dos baldes legais (`app/chamados/services/vigia_fio.py:539`, `:603`, `:609`, `:649`). | `Command.handle()` copia `alc['fio_morto_alcance']` para o bloco `censo` (`app/chamados/management/commands/vigiar_fio_morto.py:216-217`). |
| `alcance.alcance_cego` | `not dias`, portanto universo vazio significa cegueira (`app/chamados/services/vigia_fio.py:553`). | Bloco `alcance: alc` inteiro (`app/chamados/management/commands/vigiar_fio_morto.py:218`). |
| `alcance.alcance_universo` | `len(dias)`; `dias` deduplica `(colaborador, data)` dos dias retidos por empresa visível × competência aberta (`app/chamados/services/vigia_fio.py:534-552`). | Mesmo bloco inteiro (`app/chamados/management/commands/vigiar_fio_morto.py:218`). |
| `esperado` | `ESPERADO` contém antigo `fio_morto=0`; `ESPERADO_DO_CARIMBO` acrescenta `fio_morto_alcance=0` (`app/chamados/management/commands/vigiar_fio_morto.py:87-95`). | Cópia de `ESPERADO_DO_CARIMBO` (`app/chamados/management/commands/vigiar_fio_morto.py:211`). |
| `medido_em` | `agora=timezone.now()` antes de executar os dois censos; não é timestamp de conclusão (`app/chamados/management/commands/vigiar_fio_morto.py:162-168`). | `timezone.localtime(agora).isoformat()` (`app/chamados/management/commands/vigiar_fio_morto.py:207`). |
| `censo.fio_morto` antigo | `censo()` inicializa 0; universo parte de chamado vivo com célula; incrementa `fio_morto` após examinar lâmpada, pergunta, teto temporal e dono fora do fio (`app/chamados/services/vigia_fio.py:317`, `:340`, `:359-417`). | `dict(d, ...)` preserva o contador antigo (`app/chamados/management/commands/vigiar_fio_morto.py:216`). |

Persistência e exceções:

- Caminho padrão do escritor: `settings.VIGIA_FIO_CARIMBO` se configurado; senão `<BASE_DIR>/logs/fio_morto.json` (`app/chamados/management/commands/vigiar_fio_morto.py:98`, `:101-111`). O leitor host fixa `<raiz>/app/logs/fio_morto.json`, sem ler essa configuração (`bin/fio_estado.py:55`, `:62`).
- `_carimbar()` grava `.novo` e faz `os.replace()`, sem capturar erro de escrita (`app/chamados/management/commands/vigiar_fio_morto.py:126-133`). Exceção em censos/consultas anteriores impede atingir a gravação; a ordem está em `:167-189` antes de `:205-221`. Consequentemente, uma rodada falha pode deixar o último carimbo legível, ainda fresco para o leitor; não há registro da falha embutido no carimbo antigo (`app/chamados/management/commands/vigiar_fio_morto.py:167-221`; `bin/fio_estado.py:63-84`).
- `--empresa` não carimba: transforma a opção em lista de empresas, e grava só se `empresas is None`; até opção com IDs sem correspondência gera `[]`, portanto cai no “NAO carimbado” (`app/chamados/management/commands/vigiar_fio_morto.py:152-156`, `:205`, `:223-225`). Recorte persistido por essa implementação não é caso normal; arquivo de fixture/manual poderia não ter marcador de recorte, pois o leitor não valida proveniência (`bin/fio_estado.py:63-92`).
- Universo novo vazio retorna `fio_morto_alcance=0`, `alcance_universo=0`, `alcance_cego=True`; o command copia inclusive esse zero ao censo. Assim, comparar apenas contador × esperado produz igualdade 0=0 sem medição não vazia (`app/chamados/services/vigia_fio.py:538-555`; `app/chamados/management/commands/vigiar_fio_morto.py:211`, `:216-218`). O contrato precisa fazer cegueira/universo prevalecerem sobre essa igualdade.
- Falha da lista Cadastro × Realidade é capturada e registrada: mantém `_dias=None`, `dia_do_admin()` retorna falso e conta dia sem resposta, sem retirar o dia do contador (`app/chamados/services/vigia_fio.py:203-225`). Os dois censos compartilham a mesma lista; contador de falhas é copiado após ambos (`app/chamados/management/commands/vigiar_fio_morto.py:164-169`). Logo essa exceção é uma medição degradada que ainda pode produzir carimbo; não equivale à exceção de leitura do JSON (`app/chamados/services/vigia_fio.py:217-225`; `app/chamados/management/commands/vigiar_fio_morto.py:205-221`).

Consumos internos relevantes:

- O command lê `alc['fio_morto_alcance']` para o censo (`app/chamados/management/commands/vigiar_fio_morto.py:217`); `linhas_alcance()` lê contador, universo e cegueira para saída textual (`app/chamados/services/vigia_fio.py:659-664`).
- O alarme do command continua com `ALARMA_SEMPRE=('dias_futuros_no_universo','cego')` e passivos antigos; não inclui `alcance_cego` nem `fio_morto_alcance` (`app/chamados/management/commands/vigiar_fio_morto.py:81-95`, `:170-171`, `:227-228`). Portanto um painel corrigido pode acusar alcance cego/passivo enquanto o cron não alarma por esses mesmos campos, por desenho explícito atual (`:92-94`).
- O leitor atual não expõe `alcance`, `alcance_cego`, `alcance_universo` nem o novo contador em seu retorno; apenas o detecta incidentalmente se estiver listado em `esperado`, pelo comparador genérico (`bin/fio_estado.py:69-92`). Manchete continua no antigo `fio_morto` (`bin/fio_estado.py:101-104`).
- Não há dado produtivo medido no anexo: o que aconteceu em produção é `[nao verificado]`; a descrição acima refere-se aos caminhos possíveis no código.

## 31/10 03:10 · sessao f6d5809d61e6 · O261-E-F1-CLASSE-4 · verificador · origem=Code

**Reprovo o diff:** há fechamento de fatos distintos por dedup, sobrevivência dependente da sequência de eventos, carimbo indevido pela tranca e teste que não distingue a cura de sua reversão. (`app/ponto/services/cartorio.py:1341`, `app/chamados/models.py:709`, `app/chamados/reconciliador.py:611`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63`)

1. **O gate não garante “um vivo”: há contraexemplos com zero e com dois.**

   **Zero depois, um antes:** considere A antigo, `intervalo_pendente`, cobrando intervalo apagado; B novo, `batida_ausente`, cobrando somente S apagada. Ambos sem perguntas ancoradas, num dia abaixo do piso de emissão, sem toque do admin. O código admite essas cobranças separadas por marcos e bloqueia a reemissão automática abaixo do piso. (`app/chamados/catalogo/modulos.py:105`, `app/chamados/catalogo/modulos.py:111`, `app/chamados/models.py:1234`, `app/ponto/services/cartorio.py:1398`, `app/ponto/services/cartorio.py:1460`)

   | Evento | Antes do diff | Depois do diff |
   |---|---|---|
   | Fechamento da disputa de B dispara Defesa 1 | B fecha apoiado em A. | B recusa porque ele é o C8. |
   | Fechamento da disputa de A dispara Defesa 1 | A sozinho recusa. | A fecha apoiado em B. |
   | Apenas S acende; cartório reconcilia por marco | A continua cobrando intervalo. | B fecha; o intervalo continua apagado, mas ficam zero vivos. |

   A sequência decorre do signal chamando a mesma porta, da troca de `excluir=self` pela proteção do próprio C8 e do fechamento independente quando os marcos do sobrevivente acendem. Abaixo do piso, o cartório não recompõe a cobrança apagada. É um contraexemplo de fluxo; ocorrência em produção: **[não verificado]**. (`app/chamados/signals.py:42`, `app/chamados/models.py:704`, `app/chamados/models.py:709`, `app/chamados/models.py:1236`, `app/ponto/services/cartorio.py:1225`, `app/ponto/services/cartorio.py:1406`)

   **Dois depois, um antes:** A antigo e B novo cobram D; B não tem perguntas; A ancora pergunta viva de D−1, admitida pela costura de jornada noturna. O gate protege B pelo C8 de D. A tenta fechar, mas sua pergunta procura uma resolvedora em D−1, onde não há candidata, e também recusa. Antes, B sem pergunta fechava apoiado em A. (`app/chamados/models.py:369`, `app/chamados/models.py:371`, `app/chamados/models.py:373`, `app/chamados/models.py:709`, `app/chamados/juizes.py:1626`, `app/chamados/juizes.py:1719`)

   A extensão nova não alcança esse caso: o cartório pula A por fio vivo e B por ausência de perguntas. Portanto, proteger o eleito e conseguir transferir as perguntas são pré-requisitos diferentes; o primeiro não garante o segundo. (`app/ponto/services/cartorio.py:1325`, `app/ponto/services/cartorio.py:1326`, `app/chamados/models.py:373`)

2. **`[FIO-DUP-C8]` amplia o fechamento para outro turno e para chamado respondido.**

   O filtro novo de `y` exige apenas outro objeto, estado vivo e módulo em `CALADOS_POR_ISENCAO`; não compara `entrada_timestamp` nem aplica a proteção de resposta usada para `x`. (`app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1341`)

   - **Outro turno do mesmo dia:** passa pela seleção por dia e pelo gate de dedup. A reata também aceita imediatamente datas iguais. Isso contraria a identidade explicitada pelo juiz de turno: dois turnos no mesmo dia são dois fatos. (`app/chamados/juizes.py:1625`, `app/chamados/models.py:709`, `app/chamados/juizes.py:1710`, `app/chamados/juizes.py:1647`)
   - **Chamado com resposta:** `y` pode fechar mesmo que sua própria passagem pelo laço externo fosse poupada pelo texto do colaborador. A pergunta viva pode ser reapontada para `x`; o efeito demonstrável é fechamento e mudança de âncora, sem prova de perda do texto. (`app/ponto/services/cartorio.py:1328`, `app/ponto/services/cartorio.py:1343`, `app/chamados/models.py:416`, `app/chamados/juizes.py:1020`)
   - **Outro módulo:** a extensão alcança `batida_ausente`, `turno_aberto` e `intervalo_pendente`. Diferença de módulo, isoladamente, não demonstra erro: já existe teste autorizando dedup entre `batida_ausente` e `turno_aberto`. (`app/chamados/catalogo/furo_modulos.py:15`, `app/chamados/catalogo/modulos.py:235`, `app/chamados/tests/test_fechar3_retorno_do_gate.py:85`)

3. **Há mudança indireta fora de `CALADOS_POR_ISENCAO`, pela retirada da ordenação.**

   A cobertura agora percorre os mais novos primeiro. Considere A antigo, `entrada_bloqueada_turno_aberto`, categoria `turno_aberto`, datado por contexto/célula e com premissa viva; B novo, `batida_ausente`, cujo único marco já acendeu. Ambos passam pelo filtro de premissa da cobertura. Antes, o helper colocava B por último: A fechava apoiado em B, depois B fechava pelo marco. Agora B fecha primeiro e A perde a cobrança de apoio, permanecendo vivo. (`app/ponto/services/cobertura.py:225`, `app/ponto/services/cobertura.py:309`, `app/chamados/catalogo/premissa.py:48`, `app/chamados/models.py:1197`, `app/chamados/models.py:1236`)

   Portanto, “fora de CALADOS nada muda” vale apenas para a comparação isolada do próprio chamado com a resposta do C8; não vale para o estado que ele encontra depois dos outros fechamentos. Existência desse legado datado em produção: **[não verificado]**. (`app/chamados/models.py:707`, `app/chamados/models.py:709`, `app/ponto/services/cobertura.py:317`)

4. **E2 transforma silêncio pela tranca em absolvição pela célula.**

   Com célula não acusadora, marco pendente e marco perguntado apagado, o pré-teste anterior impedia o carimbo. Agora o C6 pode devolver `False` exclusivamente porque o dia está trancado; E2 grava `materializada_em` e `celula_absolveu`, embora a tranca expressamente não constitua absolvição. (`app/chamados/reconciliador.py:584`, `app/chamados/reconciliador.py:611`, `app/chamados/reconciliador.py:615`, `app/chamados/juizes.py:1010`, `app/chamados/juizes.py:1012`)

   O escritor irmão já distingue essas causas e proíbe carimbar pergunta morta pela leitura da tranca. Destrancar só limpa a via `competencia_trancada`; não limpa `celula_absolveu`. O materializador, por sua vez, exige `materializada_em=NULL`. Assim, o carimbo indevido pode sobreviver à reabertura e excluir uma resposta da materialização. (`app/chamados/services/disputa_emissao.py:1549`, `app/chamados/models.py:2541`, `app/chamados/models.py:2534`, `app/chamados/services/materializacao.py:1286`)

   **Resposta à pergunta 4:** no universo atual selecionado por E2, não identifiquei outro delta funcional normal além da tranca. Validação e materialização prévias estão excluídas; marco aceso já passava pelo pré-teste removido; pendência sem marco aceso e sem tranca continua viva no C6. (`app/chamados/reconciliador.py:584`, `app/chamados/juizes.py:1005`, `app/chamados/juizes.py:1032`, `app/chamados/juizes.py:1068`, `app/chamados/juizes.py:1080`)

5. **Os testes mordem parcialmente; a reversão exata da cura E2 permanece indistinguível.** [nao verificado]

   | Mutante | Resultado dedutível pela leitura |
   |---|---|
   | Restaurar exatamente o pré-teste removido de E2 | Preserva os dois resultados esperados: saída acesa passa; apagada continua sem carimbo. Os testes não distinguem a reversão. (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:26`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63`, `app/chamados/juizes.py:1068`) |
   | Restaurar o teto antigo, sem exceção para marco aceso | Contraria o primeiro teste. Esse é o mutante que o teste distingue. (`app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63`) |
   | Restaurar `excluir=self` no gate | Contraria a sobrevivência do novo e a trilha exigidas no teste do signal. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:249`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:255`) |
   | Remover `[FIO-DUP-C8]` | Contraria fechamento e reaponte exigidos no terceiro teste novo. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:313`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:323`) |
   | Restaurar somente a ordenação, mantendo o gate novo | As expectativas novas não distinguem essa restauração. (`app/chamados/models.py:709`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:251`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:303`) |

   Os testes da extensão usam mesmo módulo, mesmos marcos e perguntas sem resposta. Não cobrem outro turno, resposta escrita nem a sequência temporal de desaparecimento da cobrança. Os testes E2 também omitem `data_evento`, deixando a tranca fora do caso. (`app/ponto/tests/test_fechar3_troca_escala_gate.py:272`, `app/ponto/tests/test_fechar3_troca_escala_gate.py:283`, `app/chamados/tests/test_e2_pergunta_so_ao_c6.py:58`, `app/chamados/juizes.py:887`)

O censo dos chamadores de `.reconciliar()` resulta nos efeitos abaixo; “sem mudança direta” refere-se ao caminho descrito, preservados seus insumos. (`app/chamados/models.py:645`, `app/chamados/models.py:688`, `app/chamados/models.py:699`)

| Leitor/chamador | O que passa a observar |
|---|---|
| `app/chamados/signals.py:42` | Pode receber recusa para o novo antes fechável; a nota Defesa 1 depende do retorno. |
| `app/chamados/services/materializacao.py:539` e `:711` | Uma chamada descarta o retorno; a outra o propaga. O novo gate pode manter o destino vivo. |
| `app/ponto/management/commands/detectar_ausencias.py:158` e `:816` | LIMBO normalmente não chega ao C8; os fechamentos de cobrança condicionam suas notas ao retorno. |
| `app/chamados/services/limbo_folgas.py:101` e `:243` | Pai cadastral normalmente permanece igual; filho agrupado sob pai vivo passa antes do C8. (`app/chamados/models.py:699`) |
| `app/ponto/services/cobertura.py:122` e `:317` | Mudam retorno, recusados/explicados e, pela ordem removida, o apoio disponível aos seguintes. |
| `app/ponto/services/veredito_celula.py:174` e `:183` | Fecha perguntas antes do lote; decisão administrativa na célula regulariza antes do C8. (`app/chamados/models.py:1230`) |
| `app/chamados/services/adesao.py:115` e `:150` | Agrupamento vivo e pai cadastral normalmente evitam a comparação alterada. (`app/chamados/models.py:699`, `app/chamados/models.py:1192`) |
| `app/ponto/services/lastro.py:306` e `:317` | Marco efetivamente satisfeito regulariza antes do C8; pode encerrar o sobrevivente que sustentou outros fechamentos. (`app/chamados/models.py:1236`) |
| `app/ponto/services/triagem_batida.py:580` e `:588` | Mantém proteção de outro turno aberto e seleção por módulo; recusa do novo gate impede os efeitos posteriores. |
| `app/ponto/services/ausencia.py:678` | Pode observar outros apoios vivos após a mudança de sequência; o módulo externo entra na guarda pela categoria `ausencia`. (`app/ponto/services/ausencia.py:653`, `app/chamados/models.py:1197`) |
| `app/chamados/services/feriado_reavaliar.py:47` | Espuriedade reconhecida regulariza antes do C8; nota/trilha continuam condicionadas ao fechamento. (`app/chamados/models.py:1252`) |
| `app/chamados/services/silencio_ausencia.py:93` | Aprovação e pré-filtro permanecem; se chegar à comparação alterada, a recusa impede o fechamento. |
| `app/ponto/services/cluster_espurio.py:80` | Classe emitida fora da guarda de furo: sem mudança direta pelo C8. (`app/ponto/services/cluster_espurio.py:89`, `app/chamados/models.py:1192`) |
| `app/ponto/services/entrada_sincronizada.py:28` | Exige marcos acesos antes da porta; normalmente regulariza antes do C8. (`app/chamados/models.py:1236`) |
| `app/chamados/management/commands/auditar_chamados_orfaos.py:67` | Pode trocar fechamento/OK por recusa; nota depende do retorno. |
| `app/colaboradores/management/commands/marcar_feriado_do_posto.py:126` | Premissa espúria reconhecida passa antes do C8; nota e contador dependem do retorno. (`app/chamados/models.py:1252`) |
| `app/colaboradores/services/pos_vinculo.py:36` | Pais cadastrais normalmente ficam fora da guarda alterada; retorno governa nota/contador. |
| `app/ponto/management/commands/curar_residuo_bug115.py:151` | Continua subordinado ao gate; nova recusa impede nota e contagem de encerramento. |
| `app/ponto/management/commands/detectar_intervalo_ausente.py:128` | Premissa específica morta não garante dia regularizado; a proteção do eleito pode mudar fechamento e contador, inclusive no dry-run. |
| `app/chamados/reconciliador.py:520` | Retenções continuam anteriores ao gate; fechamento, justificativa e resolução das perguntas dependem de sua aprovação. |
| `app/ponto/services/cartorio.py:1225`, `:1259` e `:1341` | Perde a ordenação e ganha tentativas de fechamento de outros vivos, inclusive os poupados pelo laço externo. |

Quanto aos campos e autoridades pedidos:

- **`status_local`:** a escrita continua em `_gravar_estado`; o diff altera decisões e sequência que chegam ao escritor, com efeitos no signal resolvedor e no rejulgamento do cartório. (`app/chamados/models.py:618`, `app/chamados/models.py:222`, `app/chamados/models.py:279`, `app/chamados/signals.py:76`, `app/chamados/signals.py:165`)
- **`_f1_dedup`:** mantém escritor e consumidor. A tentativa protegida deixa de preenchê-lo; a trilha continua posterior à aprovação da pergunta viva. (`app/chamados/models.py:717`, `app/chamados/models.py:645`, `app/chamados/models.py:648`, `app/chamados/models.py:654`)
- **`chamado_destino`:** muda qual sobrevivente recebe a pergunta; a escrita continua pela porta `reapontar_resolvedora`, com `resolvedora_valida` e relavra. (`app/chamados/models.py:371`, `app/chamados/models.py:418`, `app/chamados/services/fio.py:837`, `app/chamados/services/fio.py:845`)
- **`materializada_em`/`via_resolucao`:** E2 ganha o alcance indevido da tranca; o resolvedor por chamado mantém sua exclusão explícita. (`app/chamados/reconciliador.py:615`, `app/chamados/services/disputa_emissao.py:1551`)
- **C6/C8:** suas funções não recebem alteração de corpo neste diff; mudam os argumentos ao C8 e os estados que suas consultas encontram. O C6 responde vitalidade, incluindo silêncio pela tranca, não a causa “célula absolveu”. (`app/chamados/models.py:709`, `app/chamados/juizes.py:1625`, `app/chamados/juizes.py:1010`, `app/chamados/reconciliador.py:617`)
- **Cadastro:** C6 está registrado como `pergunta_viva`; C8 está registrado como `ChamadoColaborador.abrir`, e a documentação confirma essa autoridade. Chamar `cobranca_viva_do_dia` de “C8” não demonstra, por si, conformidade com esse registro. (`app/core/juizes.py:238`, `app/core/juizes.py:240`, `app/docs/JUIZES.md:54`)

Execução das suítes/mutantes, incidência dos contraexemplos em produção e resultado após uma agenda real completa de crons: **[não verificado]**.

## 31/10 03:08 · sessao 51114a07024e · - · - · origem=Ronald (tmux)

Dois contraexemplos condicionais pelo código; ocorrência em produção [não verificado]:

1. **Zero vivos depois, um antes**: A velho `intervalo_pendente` cobra intervalo apagado; B novo `batida_ausente` cobra S apagada; dia abaixo do piso, sem perguntas. Fechar disputa de B: antes B fechava apoiado A; agora B recusa por ser C8. Fechar disputa de A: antes A sozinho recusava; agora A fecha apoiado B. Quando somente S acende, B fecha por premissa regularizada; intervalo continua apagado e nenhuma cobrança renasce abaixo do piso. Referências: `app/chamados/signals.py:42`, `app/chamados/models.py:699`, `app/chamados/models.py:709`, `app/chamados/models.py:1236`, `app/ponto/services/cartorio.py:1398`, `app/ponto/services/cartorio.py:1460`. Precisa explicitar ausência de outros chamados/perguntas e de toque que atravesse piso.

2. **Dois vivos depois, um antes**: A velho e B novo cobram D; B não tem perguntas; A ancora pergunta viva de D−1, costura legítima por turno noturno. Agora B recusa por ser C8; A tenta fechar, mas sua pergunta procura C8 em D−1, sem candidato, e segura A. Antes B fechava apoiado A, restando A. Referências: `app/chamados/models.py:369`, `app/chamados/models.py:375`, `app/chamados/models.py:710`, `app/chamados/juizes.py:1626`, `app/chamados/juizes.py:1719`. Cartório pula A com fio vivo e B sem perguntas: `app/ponto/services/cartorio.py:1325`.

**Módulo externo também pode mudar indiretamente**: retirada da ordenação em cobertura faz B novo com marco já atendido fechar antes de A externo; A perde sua cobrança de apoio. Exemplo externo aceito pelo código: `entrada_bloqueada_turno_aberto`, categoria `turno_aberto`, ligado a D por célula/contexto. Ambos passam o filtro da cobertura. Antes A fechava em B; agora permanece. Referências: `app/ponto/services/cobertura.py:225`, `app/ponto/services/cobertura.py:309`, `app/chamados/catalogo/premissa.py:48`, `app/chamados/models.py:1197`, `app/ponto/views.py:297`. Existência desse legado datado em produção [não verificado].

Mapa completo dos chamadores foi enviado nas mensagens anteriores. `_f1_dedup` mantém escritor e consumidor: preenchimento em `app/chamados/models.py:717`, limpeza/leitura em `app/chamados/models.py:645`, emissão somente após aprovação da pergunta em `app/chamados/models.py:649`. O diff muda quais tentativas chegam ao preenchimento; nenhum escritor adicional de `status_local` foi identificado no diff, que continua chegando a `app/chamados/models.py:227` e `app/chamados/models.py:279`.

## 31/10 03:05 · sessao 4b8ade7fe326 · - · - · origem=Ronald (tmux)

Achado principal: a extensão `[FIO-DUP-C8]` trata todo `y` vivo do lote e em `CALADOS_POR_ISENCAO` como duplicata; não compara `entrada_timestamp`, não exige mesmo módulo nem repete a proteção de `resposta_colab` aplicada a `x`. (app/ponto/services/cartorio.py:1328; app/ponto/services/cartorio.py:1341)

- **Outro turno do mesmo dia:** caminho demonstrável pelo código. `vivos` agrupa pelo dia declarado do chamado, sem chave de turno; C8 escolhe pelo dia e antiguidade, também sem chave de turno. A extensão chama `y.reconciliar()`, cujo gate admite dedup sustentado por `x`. Se houver pergunta viva, `resolvedora_valida` aceita imediatamente quando as datas são iguais; não verifica as entradas distintas. Isso conflita com a identidade registrada em `irmao_vivo_do_turno`: dois turnos no mesmo dia são dois fatos. (app/ponto/services/cartorio.py:85; app/ponto/services/cartorio.py:94; app/chamados/juizes.py:1625; app/chamados/models.py:709; app/chamados/models.py:371; app/chamados/juizes.py:1710; app/chamados/juizes.py:1647)

- **Chamado com resposta:** a proteção de texto só vale para `x`; `y` respondido pode fechar pela extensão. Entretanto, não demonstrar isso como “resposta enterrada”: a pergunta viva pode ser reapontada para `x`, preservando seu texto e sua vitalidade. O efeito comprovável é fechar o chamado respondido e mudar sua âncora, contornando a exclusão explícita do laço externo. (app/ponto/services/cartorio.py:1328; app/ponto/services/cartorio.py:1343; app/chamados/models.py:371; app/chamados/models.py:416; app/chamados/juizes.py:1020)

- **Outro módulo:** alcança `batida_ausente`, `turno_aberto` e `intervalo_pendente`, pela interseção entre a lista do cartório e CALADOS. Só a diferença de módulo não prova fechamento indevido: já há testes autorizando dedup entre `batida_ausente` e `turno_aberto`. (app/chamados/catalogo/furo_modulos.py:15; app/chamados/catalogo/modulos.py:235; app/ponto/services/cartorio.py:1343; app/chamados/tests/test_fechar3_retorno_do_gate.py:85)

**Mordida dos testes, por leitura estática:** [nao verificado]

- Reverter o gate para `excluir=self` contraria a expectativa do novo teste com pergunta do mais novo e signal do E2: ele exige o novo vivo e trilha de fechamento do velho sustentada nele. (app/ponto/tests/test_fechar3_troca_escala_gate.py:242; app/ponto/tests/test_fechar3_troca_escala_gate.py:249; app/ponto/tests/test_fechar3_troca_escala_gate.py:255)
- Remover a extensão deixa o velho com pergunta viva fora da reconciliação FIO-DUP normal, contrariando a expectativa de fechamento/reaponte do terceiro teste. (app/ponto/services/cartorio.py:1326; app/ponto/tests/test_fechar3_troca_escala_gate.py:313; app/ponto/tests/test_fechar3_troca_escala_gate.py:323)
- Os testes novos da extensão usam o mesmo módulo, os mesmos marcos e perguntas sem resposta; não exercitam outro turno, outro módulo nem resposta escrita. Retirar filtros ou ampliar indevidamente a extensão nesses eixos pode continuar verde. (app/ponto/tests/test_fechar3_troca_escala_gate.py:272; app/ponto/tests/test_fechar3_troca_escala_gate.py:283; app/ponto/tests/test_fechar3_troca_escala_gate.py:318)
- Restaurar somente a ordenação pelo helper, mantendo o gate atual, não é distinguido pelas expectativas de resultado: o próprio gate já protege a identidade escolhida pelo C8. (app/chamados/models.py:709; app/ponto/tests/test_fechar3_troca_escala_gate.py:251; app/ponto/tests/test_fechar3_troca_escala_gate.py:303)

Execução dos testes/mutantes e ocorrência desses casos em produção: **[não verificado]**. [nao verificado]

## 31/10 03:05 · sessao 46ca8e8ed8eb · - · - · origem=Ronald (tmux)

**E2 falha ao transformar tranca reversível em `celula_absolveu`.** Para pergunta sem carimbo/validação, célula não acusadora com marco pendente e marco perguntado apagado, o pré-teste removido impedia o carimbo. Agora `dia_da_pergunta_trancado()` faz o C6 devolver `False` antes do teto, e E2 grava `materializada_em` e `VIA_CELULA_ABSOLVEU`. O escritor irmão preserva expressamente a recusa de carimbar por tranca. (app/chamados/reconciliador.py:584, app/chamados/reconciliador.py:611, app/chamados/juizes.py:1010, app/chamados/services/disputa_emissao.py:1549)

O efeito persiste no carimbo: `reabrir_por_competencia()` só limpa `VIA_COMPETENCIA_TRANCADA`; `reabrir_por_resposta()` só limpa `VIAS_SEM_LASTRO`, que não inclui `celula_absolveu`; o materializador exige `materializada_em=NULL`. Assim, uma resposta posteriormente viva pode continuar excluída da seleção de materialização. (app/chamados/models.py:2541, app/chamados/models.py:2534, app/chamados/models.py:2893, app/chamados/services/materializacao.py:1286)

**Os dois testes E2 deixam verde o mutante que desfaz exatamente esta cura.** Reintroduzir `if marcos_pendentes(p.celula) and fato_do_marco_no_espelho(p) is not True: continue` preserva ambos resultados: saída acesa passa pelo pré-teste e C6 mata; saída apagada é preservada por ambos. Eles mordem apenas a guarda mais antiga que retinha qualquer pergunta com marco pendente. Os testes também omitem `data_evento`, portanto não exercitam a tranca. (app/chamados/tests/test_e2_pergunta_so_ao_c6.py:26, app/chamados/tests/test_e2_pergunta_so_ao_c6.py:58, app/chamados/tests/test_e2_pergunta_so_ao_c6.py:63, app/chamados/juizes.py:887, app/chamados/juizes.py:1067)

**Pergunta específica 4:** por inspeção de fluxo, no universo selecionado por E2, não há outro delta normal além da tranca. `validada_em` e `materializada_em` nulos excluem as demais mortes antecipadas do C6; resposta concordante exige marco aceso, que já passava pela guarda antiga; pendente sem marco aceso e sem tranca acaba vivo. (app/chamados/reconciliador.py:584, app/chamados/juizes.py:969, app/chamados/juizes.py:1005, app/chamados/juizes.py:1044, app/chamados/juizes.py:1068)

**Escritores e leitores:**

- `retratar_perguntas_absolvidas_por_celula` muda os carimbos apenas no caso acima; `resolver_perguntas_por_chamado_resolvido` não foi alterado e mantém proteção explícita da tranca no ramo com destino. O fechamento do chamado pode dispará-lo pelo signal. (app/chamados/reconciliador.py:615, app/chamados/services/disputa_emissao.py:1536, app/chamados/services/disputa_emissao.py:1551, app/chamados/signals.py:76)
- `chamado_destino` continua passando por `reapontar_resolvedora`, que consulta `resolvedora_valida`, protege o fio, grava a FK e relavra o veredito. A reata no gate usa essa porta. O diff pode mudar **qual** chamado chega a essa reata ao mudar o sobrevivente, mas não cria escritor adicional da FK. (app/chamados/services/fio.py:829, app/chamados/services/fio.py:837, app/chamados/services/fio.py:845, app/chamados/models.py:413)
- O plano do gate procura sobrevivente por pergunta, ainda com `excluir=self`, e exige `resolvedora_valida`. Este juiz aceita mesmo dia e possui exceção para jornada atravessando meia-noite; não distingue dois turnos no mesmo dia quando as datas coincidem. (app/chamados/models.py:367, app/chamados/models.py:371, app/chamados/juizes.py:1710, app/chamados/juizes.py:1719)
- O C6 não mudou neste diff; mudou sua autoridade sobre a escrita de E2. Seus retornos continuam incluindo “morta pela tranca”, que não significa “célula absolveu”. (app/chamados/juizes.py:1005, app/chamados/reconciliador.py:611)

**Registro:** o C6 registrado é `pergunta_viva`; o C8 registrado é `ChamadoColaborador.abrir`, não `cobranca_viva_do_dia`. A tabela documental confirma “duplicado = abrir”. Portanto a denominação “C8” usada pelo diff para o sobrevivente não coincide com o registro atual. (app/core/juizes.py:238, app/core/juizes.py:240, app/docs/JUIZES.md:54)

Não executei testes ou mutantes; o resultado de execução e a ocorrência desses estados em produção são [não verificado]. [nao verificado]

## 31/10 03:01 · sessao 0b50d4b4a7dc · O261-E-F1-CLASSE-4 · sonda · origem=Code

**(1) tipo_dia_por_trabalha**

| trabalha | ata.tipo_dia | Contagem | Referência |
|---|---|---:|---|
| True | 'trabalho' | 46093 | [nao verificado] |
| False | 'folga' | 22128 | [nao verificado] |
| True | None | 10409 | [nao verificado] |
| False | None | 5382 | [nao verificado] |
| True | 'ausencia' | 2904 | [nao verificado] |
| False | 'ausencia' | 1609 | [nao verificado] |
| True | 'falta' | 375 | [nao verificado] |
| True | 'folga' | 1885 | [nao verificado] |
| None | 'trabalho' | 52007 | [nao verificado] |
| False | 'trabalho' | 154 | [nao verificado] |
| None | 'folga' | 222 | [nao verificado] |
| None | 'ausencia' | 78 | [nao verificado] |
| None | None | 66 | [nao verificado] |

**(2) universo_chamados_vivos_com_celula: 2964** [nao verificado]

**(3) divergem**

| Chave (trabalha, tipo_dia, em CALADOS_POR_ISENCAO, modulo) | Contagem | Chamado de exemplo | Referência |
|---|---:|---:|---|
| (None, 'trabalho', False, 'ancora_limbo') | 1 | 12982 | [nao verificado] |
| (True, 'ausencia', False, 'batida_fora_escala_leve') | 1 | 17793 | [nao verificado] |
| (True, 'ausencia', False, 'colaborador_em_ferias') | 1 | 28767 | [nao verificado] |
| (True, 'ausencia', False, 'desligamento_revisao') | 10 | 35918 | [nao verificado] |
| (True, 'ausencia', False, 'disputa_supervisao') | 3 | 20782 | [nao verificado] |
| (True, 'ausencia', False, 'sem_previsao_escala') | 5 | 35217 | [nao verificado] |
| (True, 'falta', False, 'desligamento_revisao') | 4 | 35915 | [nao verificado] |
| (True, 'falta', False, 'disputa_supervisao') | 1 | 15402 | [nao verificado] |
| (True, 'folga', False, 'desligamento_revisao') | 1 | 16004 | [nao verificado] |
| (True, 'folga', False, 'sem_previsao_escala') | 1 | 32564 | [nao verificado] |
| (True, 'folga', True, 'batida_ausente') | 10 | 34867 | [nao verificado] |

## 31/10 02:37 · sessao 6a09fec44f3d · O261-E-F5C · verificador · origem=Code

**FINAL: BLOQUEIA.** O diff mantém divergência entre painel e juiz, promete uma ação que o fio não garante e acrescenta um teste que falha antes das asserções pretendidas (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326`, `app/folha/fora_da_folha.py:43`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`).

1. **Escritores e leitores**

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `porta['fila']` | Tupla ordenada produzida pelo juiz em `app/chamados/juizes.py:1226`, devolvida em `app/chamados/juizes.py:1227`. A entrada anterior, `fila_cel`, vem de `alimentar_portas`, em `app/chamados/juizes.py:1148`. | `o_que_segura`, em `app/folha/fora_da_folha.py:196`; teste direto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; comparações do dict inteiro em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164` e `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`. |
| `FORA_DA_LISTA_PADRAO` | Declaração em `app/chamados/services/verbos.py:18`. | Painel em `app/chamados/services/painel.py:327`; juiz em `app/chamados/juizes.py:1226`. |
| `GRUPOS['C']` | Texto em `app/folha/fora_da_folha.py:36`. | Tooltip via `retrato_folha.ajuda`, em `app/folha/retrato_folha.py:70`; enumeração das chaves em `app/folha/fora_da_folha.py:139`; testes em `app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_retrato_folha_quadros.py:1276` e `app/folha/tests/test_retrato_folha_quadros.py:1285`. |
| `DESTRAVA['chamado']` | Texto em `app/folha/fora_da_folha.py:43`. | Ferramenta em `app/folha/fora_da_folha.py:197`; drawer em `app/folha/retrato_folha.py:1069`; template exibe `r.valor` em `app/templates/folha/_retrato_o_que_fazer.html:37`. Testes em `app/folha/tests/test_haiku_fora_da_folha.py:137`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119` e `app/folha/tests/test_retrato_folha_drawer.py:226`. |
| `o_que_segura()['dias'][*]['chamado']` | Cópia de `porta['fila']` para lista em `app/folha/fora_da_folha.py:196`, incluída no retorno em `app/folha/fora_da_folha.py:199`. | Asserção do teste novo em `app/folha/tests/test_haiku_fora_da_folha.py:138`. Consumidor externo em produção: **[nao verificado]**. |

O vigia lê `verbos` e `rota`, **não a nova `fila`**. Assim, uma porta com `fila=()` e verbo `historico` continua sendo contada como porta alcançável (`app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`).

As pautas dos discordantes leem indiretamente o juiz por `_fio_do_dia`, comparando apenas com `FIO_SEM_PORTA`; não consomem os números de `chamado` (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:74`, `app/ponto/services/pautas_dos_discordantes.py:76`).

Os dois asserts diretos do dict inteiro foram atualizados. A comparação lote versus individual compara duas respostas atuais; o mock antigo do vigia continua sem `fila`, mas seu consumidor acessa somente `rota/verbos` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`, `app/chamados/tests/test_o261e_f5_alcance.py:307`, `app/chamados/services/vigia_fio.py:598`).

2. **Painel padrão versus `fila`: não são equivalentes — bloqueia**

O filtro final por verbo usa a mesma constante, mas o universo anterior difere: o juiz começa por `qs_na_fila`, que elimina qualquer silêncio futuro; a lista padrão elimina os verbos `arquivado/historico` depois da partição (`app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326`).

Contraexemplo de código: chamado **da célula**, status `aberto`, módulo `gps_ausente`, silêncio futuro:

- O módulo é auditoria e produz verbo `None`; o silêncio só substitui verbos diferentes de `None` (`app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:48`).
- O painel padrão mostra: `aberto` pertence a `COBRANCA`, e `None` não pertence à constante de exclusão (`app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/services/verbos.py:18`).
- O juiz exclui antes da partição, pelo silêncio futuro (`app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1144`).

Para os demais resultados possíveis: `validar/cobrar/decidir` sem silêncio entram nos dois; `arquivado` sai nos dois; `historico` sai da lista e da nova `fila`, embora permaneça em `verbos`; `None` sem silêncio futuro entra nos dois, quando o status é de cobrança (`app/chamados/catalogo/natureza.py:57`, `app/chamados/services/verbos.py:48`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1226`).

No mesmo estado e instante, sem filtros extras/paginação e comparando chamados da célula, `fila` é subconjunto da lista padrão; não há caso inverso nessa composição de filtros. A exclusão de âncoras de outra célula é adicional e intencional (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`). Ocorrência desses contraexemplos em produção: **[nao verificado]**.

3. **Universalidade das frases**

**`GRUPOS['C']`: compatível com a escada, sem bloqueio isolado.** Nas duas rotas, `chamado` significa que os verbos da porta não contêm `cobrar` nem `validar`. Isso não garante visibilidade na lista padrão (`app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:36`).

**`DESTRAVA['chamado']`: não universal — bloqueia.** A escada também devolve `chamado` para `None` e `historico`; portanto não garante a ação `decidir` (`app/chamados/juizes.py:1175`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:51`, `app/folha/fora_da_folha.py:43`).

Contraexemplo: chamado aberto da célula, silêncio vencido, nenhuma pergunta. Ele passa por `qs_na_fila`, vira `historico`, produz `porta_fila`, fio `chamado` e `fila=()`. A frase manda abrir um chamado da fila ou uma pergunta no app, embora nesse caso não haja nenhum desses alvos visíveis nomeados (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:51`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`, `app/folha/fora_da_folha.py:43`).

Na `porta_pergunta`, a existência da pergunta também não obriga o verbo a ser `decidir`: o juiz aceita o verbo da âncora, e a natureza de auditoria pode produzir `None` mesmo com pergunta (`app/chamados/juizes.py:1221`, `app/chamados/juizes.py:1222`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`).

4. **Drawer: igualdade da frase, incoerência da ação — bloqueia**

Para o mesmo fio, ferramenta e drawer usam literalmente o mesmo `DESTRAVA`; essa parte está coerente (`app/folha/fora_da_folha.py:197`, `app/folha/retrato_folha.py:183`, `app/folha/retrato_folha.py:1069`).

Porém o botão de `chamado` continua **“Validar no fio”**, enquanto a nova frase manda **decidir** e afirma ausência de validar. O botão chega ao template com esse rótulo (`app/folha/retrato_folha.py:977`, `app/folha/retrato_folha.py:1024`, `app/templates/folha/_retrato_o_que_fazer.html:62`, `app/folha/fora_da_folha.py:43`).

O drawer também não recebe o novo dado numérico: o retrato grava fio, datas e outros eixos, e a view lê essa linha gravada; não chama `o_que_segura` nesse fluxo (`app/folha/retrato_folha.py:1177`, `app/folha/retrato_folha.py:1186`, `app/colaboradores/views.py:980`, `app/colaboradores/views.py:984`). Paridade temporal entre retrato e ferramenta em produção: **[nao verificado]**.

5. **Checklist e mordida dos testes** [nao verificado]

| Item | Veredito |
|---|---|
| Segundo juiz registrado | Não identificado no diff: a porta já está registrada, a escada permanece nela e a folha lê seu resultado (`app/core/juizes.py:253`, `app/chamados/juizes.py:1175`, `app/folha/fora_da_folha.py:68`). |
| Segundo escritor fora da porta | Não identificado nas alterações produtivas: os novos valores são retornos de leitura, sem nova persistência (`app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:199`). |
| Relógio fora da borda | Nenhuma leitura nova no diff; a cadeia existente lê instantes internos separados em `qs_na_fila` e `particionar_verbos`. Não sustenta promessa de fotografia temporal única (`app/chamados/services/painel.py:188`, `app/chamados/services/verbos.py:35`, `app/chamados/juizes.py:1153`). |
| Derivação paralela | A constante é compartilhada, mas isso não unifica os filtros anteriores; permanece a divergência demonstrada para `None` com silêncio futuro (`app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1226`). **Bloqueia a equivalência.** |
| Ordem de trava | Ordem efetivamente executada nesta obra: **[nao verificado]**. |
| Pouso antes do pré-requisito | Sequência real de suite/verificação/pouso: **[nao verificado]**. A regra exige suite inteira verde antes do opus final (`CLAUDE.md:600`). | [nao verificado]

O teste **`test_MORDE_a_frase...` morde a redação anterior fornecida no diff**: suas substrings proibidas rejeitam “sem pergunta viva” e “chamado N”. Contudo, os dois mocks usam somente `decidir`; não exercitam a porta real, `None`, `historico` ou o botão do drawer. Portanto não demonstram a universalidade da nova frase (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:126`, `app/folha/tests/test_haiku_fora_da_folha.py:133`, `app/folha/tests/test_haiku_fora_da_folha.py:137`).

O teste **`test_MORDE_silenciado...` não oferece RED útil como escrito — bloqueia**: importa `Chamado`, inexistente no módulo; o modelo e o helper usam `ChamadoColaborador`. Isso causa `ImportError` antes dos subtestes, também no pai descrito pelo diff (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`).

Mesmo corrigindo esse import, **2099 continua quebrando na versão curada**: o silêncio futuro elimina o chamado antes da partição; sem pergunta, o juiz retorna `None`, mas o teste indexa `porta['verbos']` (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1223`). O ramo de 2020 poderia distinguir a ausência de `fila` no pai, mas está inacessível pelo erro anterior (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`).

**RED/GREEN observado por suite Django contra `dc3ba57f` e contra esta obra: [nao verificado].**

## 31/10 02:36 · sessao f790b02d836e · - · - · origem=Ronald (tmux)

**Bloqueia: o teste novo de silenciamento não certifica a cura.** [nao verificado]

- `test_MORDE_silenciado_na_celula_nao_e_numero_da_fila` importa `Chamado`, símbolo inexistente no módulo; o modelo é `ChamadoColaborador`. Portanto termina em `ImportError` antes dos subtestes, tanto na cópia quanto no pai reconstruído pelo diff. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, helper correto em `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`.
- Corrigido somente esse import, o caso de 2099 continua errado: `qs_na_fila` exclui o silenciamento futuro antes de alimentar o juiz; sem perguntas, este retorna `None`, e `porta['verbos']` produz `TypeError`. Referências: `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1144`, `app/chamados/juizes.py:1146`, `app/chamados/juizes.py:1153`, `app/chamados/juizes.py:1223`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`.
- O caso de 2020, depois dessa correção, alcança `porta_fila`, `historico`, `fila=()`, `fio=chamado` na cópia; o pai não devolvia `fila`, de modo que a leitura nova encontra `KeyError`, não comparação comportamental com número indevido. Referências: `app/chamados/juizes.py:1225`, `app/chamados/juizes.py:1227`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:149`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; ausência da chave no pai consta do diff anexo `[nao verificado]` quanto ao checkout histórico.
- O teste novo da frase detecta os textos antigos: `sem pergunta viva`, `nao ha pergunta` e `chamado N` violam suas asserções. Porém a primeira falha textual impede chegar aos mocks naquele código antigo; isso demonstra mordida textual, sem demonstrar por si a correção universal da frase nova. Referências: `app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:125`, `app/folha/tests/test_haiku_fora_da_folha.py:126`; textos antigos no diff anexo `[nao verificado]` quanto ao checkout histórico.

**Demais itens do checklist:**

- **Segundo juiz registrado: não identifiquei criação pelo diff.** A autoridade já está registrada, a escada continua no juiz e a folha lê sua saída: `app/core/juizes.py:132`, `app/core/juizes.py:253`, `app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1175`, `app/folha/fora_da_folha.py:55`, `app/folha/fora_da_folha.py:68`.
- **Segundo escritor fora da porta: não identifiquei escrita persistente nova.** As alterações produzem valores de retorno e textos de apresentação: `app/chamados/juizes.py:1227`, `app/folha/fora_da_folha.py:196`, `app/folha/fora_da_folha.py:198`.
- **Relógio: nenhuma leitura nova introduzida pelo diff.** A cadeia herda leituras independentes em `app/chamados/services/painel.py:188` e `app/chamados/services/verbos.py:35`; o queryset da fila é consumido para células e novamente para a partição em `app/chamados/juizes.py:1146` e `app/chamados/juizes.py:1153`. Logo não há instante único recebido pela borda nessa cadeia; não atribuir sua introdução à F5C.
- **Fonte única: a constante é compartilhada, mas isso não prova identidade dos universos.** O juiz chega à constante depois do filtro adicional de `qs_na_fila`: `app/chamados/services/verbos.py:18`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:327`, `app/chamados/juizes.py:1226`.
- **Ordem de trava e pouso antes de pré-requisito: execução desta obra `[nao verificado]`.** A norma exige suite inteira verde antes do opus final (`CLAUDE.md:600`) e impede o Codex de aprovar sozinho o pouso (`CLAUDE.md:599`). A dependência documental é F5B antes de F5, mas a mesma linha traz estado “registrada (depois da F5)”; isso é inconsistência documental, não prova da sequência executada: `app/docs/BACKLOG.md:777`. Sem prova da sequência, não acusar pouso indevido; também não certificá-lo `[nao verificado]`.

Verifiquei o import e os resultados 2020/2099 com AST e execução das funções reais isoladas, em memória, com dependências substituídas. Não rodei Django, suite, banco ou logs; resultados integrados e RED empírico em `dc3ba57f` permanecem `[nao verificado]`.

## 31/10 02:36 · sessao 2c86673a353d · - · - · origem=Ronald (tmux)

Achados para consolidar:

1. **DESTRAVA['chamado'] não é universal — bloqueia.** O juiz devolve `chamado` para qualquer conjunto sem `cobrar` nem `validar`, incluindo `None` e `historico`; não garante `decidir` (`app/chamados/juizes.py:1170`, `app/chamados/juizes.py:1175`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:48`). Porém a frase manda abrir “o chamado da fila ou a pergunta viva no app” **e decidir** (`app/folha/fora_da_folha.py:43`). Contraexemplo de código: chamado da célula, aberto, silêncio vencido, sem pergunta: entra em `qs_na_fila`, recebe `historico`, mantém porta/fio `chamado`, mas devolve `fila=()` (`app/chamados/services/painel.py:186`, `app/chamados/services/verbos.py:51`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1226`). Não há chamado visível nem pergunta no app nesse caso; ocorrência em produção [nao verificado].

2. **GRUPOS['C'] é compatível com a escada, nas duas rotas — não bloqueia isoladamente.** “Tem porta aberta” vale quando o juiz retorna dict; fio `chamado` significa ausência de `cobrar` e `validar`, independentemente da rota (`app/folha/fora_da_folha.py:36`, `app/chamados/juizes.py:1175`, `app/chamados/juizes.py:1223`). Isso descreve os verbos; não garante que alguma porta esteja visível na lista padrão (`app/chamados/juizes.py:1226`).

3. **Drawer: frase igual; ação do link incoerente — bloqueia a alegação de coerência completa.** A ferramenta usa `DESTRAVA[_f]` e o drawer lê o mesmo mapa por `FIO_DO_CARTORIO`; portanto a frase para o mesmo fio é literalmente igual (`app/folha/fora_da_folha.py:197`, `app/folha/retrato_folha.py:183`, `app/folha/retrato_folha.py:1069`). Mas o fio `chamado` oferece **“Validar no fio”**, enquanto a nova frase diz **“decidir”** e afirma que o painel não põe a porta em validar (`app/folha/retrato_folha.py:977`, `app/folha/fora_da_folha.py:43`). Esse link é materializado por `portas_da_linha` e renderizado no template (`app/folha/retrato_folha.py:1015`, `app/folha/retrato_folha.py:1024`, `app/templates/folha/_retrato_o_que_fazer.html:60`).

4. **Drawer não recebe o novo dado `chamado`.** Seu retrato grava `dono` derivado do grupo, datas e contagens; não grava números de chamados (`app/folha/retrato_folha.py:1177`, `app/folha/retrato_folha.py:1186`). A view lê a linha gravada e chama `o_que_fazer`/`portas_da_linha`; não chama `o_que_segura` (`app/colaboradores/views.py:980`, `app/colaboradores/views.py:984`). Logo a igualdade demonstrada é de frase para o mesmo fio, não de dados atuais ou números apresentados (`app/folha/fora_da_folha.py:196`, `app/folha/retrato_folha.py:1069`). Coerência temporal em produção [nao verificado].

Censo de escritores/leitores:

| Valor | Escritor | Leitores |
|---|---|---|
| `GRUPOS['C']` | `app/folha/fora_da_folha.py:36` | `retrato_folha.ajuda`, pelo mapa fio→letra (`app/folha/retrato_folha.py:70`, `app/folha/retrato_folha.py:177`); chaves de grupos em `fora_da_folha` (`app/folha/fora_da_folha.py:139`); testes (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_retrato_folha_quadros.py:1276`, `app/folha/tests/test_retrato_folha_quadros.py:1285`). |
| `DESTRAVA['chamado']` | `app/folha/fora_da_folha.py:43` | `o_que_segura` (`app/folha/fora_da_folha.py:197`), drawer (`app/folha/retrato_folha.py:1069`), template via `retrato_fazer` (`app/templates/folha/_retrato_o_que_fazer.html:33`, `app/templates/folha/_retrato_o_que_fazer.html:37`), testes (`app/folha/tests/test_haiku_fora_da_folha.py:137`, `app/folha/tests/test_haiku_fora_da_folha.py:483`, `app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`, `app/folha/tests/test_retrato_folha_drawer.py:226`). |
| `o_que_segura()['dias'][*]['chamado']` | Cópia de `porta['fila']` para lista (`app/folha/fora_da_folha.py:196`, `app/folha/fora_da_folha.py:199`) | Teste novo (`app/folha/tests/test_haiku_fora_da_folha.py:138`). Consumidor produtivo externo ou em mensageria [nao verificado]. |
| `porta['fila']` | `app/chamados/juizes.py:1226`, `app/chamados/juizes.py:1227` | `o_que_segura` (`app/folha/fora_da_folha.py:196`); testes de dict inteiro (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`) e teste novo (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`). |

Vigia e pautas continuam leitores **indiretos** da porta: o vigia lê `verbos` e `rota`, sem usar `fila` (`app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`); pautas dos discordantes leem `_fio_do_dia` e comparam com `FIO_SEM_PORTA`, sem usar o dado `chamado` (`app/ponto/services/pautas_dos_discordantes.py:59`, `app/ponto/services/pautas_dos_discordantes.py:74`).

O teste novo de frase verifica substrings proibidas e dois mocks com verbo `decidir`; não cobre `None`, `historico` nem a porta real e não chama o drawer (`app/folha/tests/test_haiku_fora_da_folha.py:123`, `app/folha/tests/test_haiku_fora_da_folha.py:126`, `app/folha/tests/test_haiku_fora_da_folha.py:133`, `app/folha/tests/test_haiku_fora_da_folha.py:137`). Ele pode reprovar a frase antiga, mas não prova a universalidade da nova (`app/folha/tests/test_haiku_fora_da_folha.py:124`).

## 31/10 02:35 · sessao 5f07ffeaa5e5 · - · - · origem=Ronald (tmux)

**Bloqueia: o teste novo de silenciamento não chega à verificação pretendida.** Ele importa `Chamado`, mas o modelo é `ChamadoColaborador` e não há definição/alias `Chamado` no módulo; o próprio helper usa o nome correto. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:147`, `app/chamados/models.py:21`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:62`.

**Bloqueia também após corrigir o import:** o caso com silêncio em 2099 exige uma porta com verbo `'arquivado'`, mas `alimentar_portas` consulta `qs_na_fila`, que exclui silêncio futuro. Sem pergunta, o juiz devolve `None`, e o teste tenta `porta['verbos']`. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:150`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156`, `app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1217`, `app/chamados/juizes.py:1223`.

**Bloqueia a promessa de equivalência universal painel/fila:** existe contraexemplo de código para verbo `None`. Chamado DA CÉLULA, status `'aberto'`, módulo `'gps_ausente'`, silêncio futuro:

- Natureza retorna `None`, pois auditoria tem precedência; o silêncio somente converte verbos diferentes de `None`. Referências: `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/services/verbos.py:47`, `app/chamados/services/verbos.py:48`.
- Painel padrão mantém: status está em `COBRANCA` e `None` não está em `FORA_DA_LISTA_PADRAO`. Referências: `app/chamados/catalogo/motor.py:29`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:326`, `app/chamados/services/verbos.py:18`.
- Juiz exclui antes da partição, pelo silêncio futuro, portanto não fornece esse número em `fila`. Referências: `app/chamados/juizes.py:1144`, `app/chamados/services/painel.py:188`, `app/chamados/juizes.py:1226`.
- Existência desse caso em produção: **[nao verificado]**.

Comparação no mesmo instante, sem filtros extras/paginação e restrita aos chamados da célula:

| Verbo/condição | Painel padrão | `fila` do juiz | Fonte |
|---|---:|---:|---|
| `validar`, `cobrar`, `decidir`, sem silêncio | inclui | inclui | `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:326`, `app/chamados/juizes.py:1226` |
| `arquivado` | exclui | exclui antes de formar a porta | `app/chamados/services/verbos.py:18`, `app/chamados/services/painel.py:188`, `app/chamados/services/painel.py:326` |
| `historico` | exclui | exclui da nova `fila`; pode permanecer em `verbos` | `app/chamados/services/painel.py:326`, `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1226` |
| `None`, sem silêncio ou silêncio vencido | inclui se status `COBRANCA` | inclui | `app/chamados/services/painel.py:314`, `app/chamados/services/verbos.py:48`, `app/chamados/juizes.py:1226` |
| `None`, silêncio futuro | **inclui** | **exclui** | referências do contraexemplo acima |

Sob essas condições, a nova `fila` é subconjunto da lista padrão: o juiz primeiro restringe `COBRANCA` sem silêncio futuro e depois aplica a mesma exclusão de verbos. Não há caminho inverso demonstrado no código, de `fila` incluir chamado da célula que o painel padrão exclui. Referências: `app/chamados/services/painel.py:186`, `app/chamados/services/painel.py:314`, `app/chamados/services/painel.py:326`, `app/chamados/juizes.py:1226`.

Censo solicitado:

| Valor | Quem escreve | Quem lê |
|---|---|---|
| `FORA_DA_LISTA_PADRAO` | declaração única `app/chamados/services/verbos.py:18` | painel `app/chamados/services/painel.py:227`, `app/chamados/services/painel.py:327`; juiz `app/chamados/juizes.py:1225`, `app/chamados/juizes.py:1226` |
| `porta['fila']` | juiz monta tupla ordenada `app/chamados/juizes.py:1226` e retorna `app/chamados/juizes.py:1227` | `o_que_segura`: `app/folha/fora_da_folha.py:196`; teste direto `app/chamados/tests/test_o261e_f5b_juiz_porta.py:158`; comparações do dict completo `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182` |
| `alimentacao['fila_cel']` — entrada, distinta da nova saída `fila` | `alimentar_portas`: `app/chamados/juizes.py:1145`, `app/chamados/juizes.py:1148`, `app/chamados/juizes.py:1153` | juiz `app/chamados/juizes.py:1217` |
| `porta['verbos']` | juiz `app/chamados/juizes.py:1218`, `app/chamados/juizes.py:1222`, `app/chamados/juizes.py:1227` | vigia `app/chamados/services/vigia_fio.py:598`; testes `app/chamados/tests/test_o261e_f5b_juiz_porta.py:130`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:156` |
| `porta['rota']` | juiz `app/chamados/juizes.py:1227` | vigia `app/chamados/services/vigia_fio.py:599`, `app/chamados/services/vigia_fio.py:601` |
| `porta['fio']` | escada `app/chamados/juizes.py:1175`, retorno `app/chamados/juizes.py:1228` | folha `app/folha/fora_da_folha.py:68` |

O vigia não lê a nova `fila`; continua contando portas e papéis a partir de `rota` e `verbos`. Assim, porta com `verbos={id:'historico'}` e `fila=()` continua entrando no balde da porta e exibindo papel `'historico'`. Referências: `app/chamados/services/vigia_fio.py:596`, `app/chamados/services/vigia_fio.py:598`, `app/chamados/services/vigia_fio.py:601`.

Os dois asserts diretos de dict completo foram atualizados no diff; a comparação lote versus individual compara os dois resultados atuais. Um mock antigo omite `fila`, mas o vigia somente acessa `rota/verbos`, portanto essa omissão não quebra esse teste. Referências: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:164`, `app/chamados/tests/test_o261e_f5b_juiz_porta.py:182`, `app/chamados/tests/test_o261e_f5_alcance.py:306`, `app/chamados/services/vigia_fio.py:598`.

Nenhuma suite foi executada; resultado observado de execução e RED efetivo contra `dc3ba57f`: **[nao verificado]**.

## 31/10 02:35 · sessao b70c86402761 · - · - · origem=Ronald (tmux)

Numeração dos anexos colados, contando a primeira linha de conteúdo como linha 1. Classificação referente à volta FINAL fornecida; conteúdo e respostas das duas chamadas de contrato: **[nao verificado]**.

| achado | classe | trecho da pergunta | trecho da resposta | linha da triagem (se houver) |
|---|---|---|---|---|
| **A — subshell** | **(a) o Codex não viu o caso específico.** Apontou outros defeitos do parser (`dj_codex_final.resp:16`). | “onde pode deixar passar o que eles mandam negar?” (`dj_codex_final.q:4`); “algum caso passa sem morder” (`dj_codex_final.q:8`). | “`… CODEX_VOLTA=final bash bin/codex_pergunta.sh …` perde papel/volta no parser”. O trecho identifica o invólucro `bash`, sem apontar subshell (`dj_codex_final.resp:16`). | **P16, procedente:** “Cura o parser”; não foi descartado (`dj_triagem_final.txt:19`). |
| **B — literal/regex de B3** | **(b) o Codex viu e a triagem descartou.** (`dj_codex_final.resp:12`; `dj_triagem_final.txt:29`). | “Obra com 3 reprovacoes vira pergunta de desenho, sem 4a volta.” (`dj_codex_final.q:2`). | “Exige meta `verificador`, prompt com `LEITURA FINAL`, `obra <ID>` conforme regex”; “Outra grafia, canal ou verificação via Codex não recebe a mesma guarda.” (`dj_codex_final.resp:12`). | **D6:** “identificacao de reprovacao por regex/LEITURA FINAL: forma declarada do verificador da casa”; “Declarar.” (`dj_triagem_final.txt:29`). |
| **C — comando no RELATO aciona Stop / expõe argv** | **(a) o Codex não viu a interação específica.** O diagnóstico correlato trata do registro, sem apontar esses efeitos (`dj_codex_final.resp:19`). | “trilha de cada negacao no RELATO” (`dj_codex_final.q:2`); “alteram alguma decisao de Stop que ja existia (libera/bloqueia)” (`dj_codex_final.q:7`). | “o registro automático no RELATO contém regra, ferramenta e alvo, sem garantir essa anotação na linha da obra.” Esse trecho não denuncia o comando cru como afirmação de ato nem sua exposição pública (`dj_codex_final.resp:19`). | Nenhum descarte específico de C identificado **[nao verificado]**. P8 trata de garantir o registro; P9, da concorrência entre escritores (`dj_triagem_final.txt:11`; `dj_triagem_final.txt:12`). |

Em B, o Codex identificou a restrição de reconhecimento; o resultado quantitativo “4 de 9” aparece no achado posterior, não nesse diagnóstico (`dj_codex_final.resp:12`; `achados_opus.txt:3`).

## 31/10 02:33 · sessao 50280be317b3 · - · - · origem=Ronald (tmux)

1. **Estado de A, B e C**

**A — a falha de subshell descrita foi corrigida.** Este comando agora é reconhecido; os parênteses separam trechos, e o selo exige a negativa por orçamento nesse exemplo (`bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/tests/test_disjuntor.sh:487`):

```bash
cd /x && (CODEX_OBRA=X CODEX_PAPEL=verificador bin/codex_pergunta.sh --anexo d.diff h q r)
```

Isso não fecha o reconhecimento de Bash: há falsos negativos e falsos positivos novos, detalhados no item 2 (`bin/disjuntor.py:751`, `bin/disjuntor.py:787`, `bin/disjuntor.py:817`).

**B — parcialmente corrigido; ainda existem verificadores finais que desaparecem da contagem.** Caixa e acentos são normalizados, e os exemplos novos com markdown/dois-pontos são cobertos (`bin/disjuntor.py:459`, `bin/disjuntor.py:473`, `bin/tests/test_disjuntor.sh:345`). Entretanto, estes prompts concretos dão `_eh_final=False`:

```text
FINAL revisão da obra OBRA-X
```

```text
Revisão
FINAL da obra **OBRA-X**
```

A expressão só aceita `final` antes de `review`, e não atravessa quebras de linha. Com esses prompts e um `SubagentHandback` contendo `REPROVADO`, `_veredito_e_obra` retorna `(None, None)`; o agente não entra em `b3` **nem em `b3_sem_obra`** (`bin/disjuntor.py:88`, `bin/disjuntor.py:495`, `bin/disjuntor.py:520`).

Há também inconsistências de identificação:

| Prompt | Obra extraída |
|---|---|
| `LEITURA FINAL da obra O261 (C)` | `O261 (C)` |
| `LEITURA FINAL da obra **O261** (C)` | `O261` |
| `LEITURA FINAL da obra obra-x` | nenhuma |

O markdown impede a captura da cauda; no último caso, a busca usa `re.I`, mas a validação posterior usa `fullmatch` sem essa opção. Isso pode repartir reprovações entre chaves ou impedir a atribuição à obra pretendida (`bin/disjuntor.py:83`, `bin/disjuntor.py:473`, `bin/disjuntor.py:481`, `bin/disjuntor.py:532`). A situação atual dos “4 de 9” mencionados no anexo é **[não verificado]**: seus nove prompts não foram fornecidos.

**C — não encontrei reprodução do vazamento original pelo comando cru.** Para o payload abaixo, a guarda nega, mas o comando vira somente hash/tamanho na linha pública; seu texto segue para `ALVO` (`bin/disjuntor.py:135`, `bin/disjuntor.py:178`, `bin/disjuntor.py:184`):

```json
{"tool_name":"Bash","tool_input":{"command":"echo deploy NO AR e FEITO; touch DISJUNTOR_DESLIGADO"}} [nao verificado]
```

A proteção ainda permite texto escolhido nos metadados e linhas que casam `ATOS`, como mostro no item 4 (`bin/disjuntor.py:186`, `bin/relato_afirma_com_prova.py:45`).

2. **Chamada real perdida e mera menção reconhecida**

Uma chamada real que agora escapa:

```bash
bin/codex_pergunta.sh</dev/null --anexo app/docs/LEIS.md h q r
```

Bash admite esse redirecionamento colado ao nome do comando. O novo regex exige espaço ou fim imediatamente depois de `.sh`, portanto `_codex_cmds` retorna `[]`; ficam sem aplicação tanto ANEXO-ENXUTO quanto B4-CODEX. O regex anterior fornecido no diff aceitava a fronteira antes de `<` (`bin/disjuntor.py:751`, `bin/disjuntor.py:841`, `bin/disjuntor.py:845`; comparação anterior: `delta.diff`, hunk de `_INVOC_RE`).

Outra regressão concreta:

```bash
cat <<<EOF
bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r
```

O wrapper é executado depois do `cat`, mas o scanner encontra `<<EOF` a partir do **segundo** `<` do here-string. Sem terminador posterior, pula até o fim e perde a chamada (`bin/disjuntor.py:787`, `bin/disjuntor.py:790`, `bin/disjuntor.py:799`).

Um falso positivo novo:

```bash
f() { bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r; }
```

Isso apenas define `f`. O parser separa `()` e remove `{`, tratando o corpo como chamada imediata. Com esse anexo, produz ANEXO-ENXUTO; sem o anexo, pode produzir B4-CODEX quando o orçamento estiver excedido (`bin/disjuntor.py:753`, `bin/disjuntor.py:754`, `bin/disjuntor.py:817`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`).

3. **Quem escreve e quem lê**

| Valor | Escritor | Leitor |
|---|---|---|
| Estado persistido `b3_sem_obra` | `_veredito_e_obra` sinaliza obra ilegível com `''`; `varrer_b3` coleta o agente e acrescenta seu ID à lista, sem duplicá-lo. `_com_estado` persiste o JSON (`bin/disjuntor.py:497`, `bin/disjuntor.py:520`, `bin/disjuntor.py:527`, `bin/disjuntor.py:242`). | A própria varredura consulta a lista para deduplicar; `_b3_sem_obra` lê e calcula seu tamanho (`bin/disjuntor.py:529`, `bin/disjuntor.py:1058`). |
| Chave persistida `b3` | `varrer_b3` cria registros, acrescenta reprovações e marca disparos; `rearma_b3` escreve `rearmado`, zera reprovações e desmarca o disparo (`bin/disjuntor.py:526`, `bin/disjuntor.py:532`, `bin/disjuntor.py:541`, `bin/disjuntor.py:558`). | Varredura, rearme e `b3_disparados`; os resultados alimentam PreToolUse e Stop (`bin/disjuntor.py:538`, `bin/disjuntor.py:553`, `bin/disjuntor.py:565`, `bin/disjuntor.py:912`, `bin/hook_stop_fila1.py:1022`). |
| Evento `ALVO` | `relato_negacao` prepara o evento; `trilha` faz o append físico (`bin/disjuntor.py:185`, `bin/disjuntor.py:107`). | Leitor explícito encontrado: o teste 26. `ler` percorre o arquivo, mas descarta `ALVO`, pois filtra outros eventos; consumidor funcional nos cinco arquivos examinados: não encontrei (`bin/tests/test_disjuntor.sh:557`, `bin/disjuntor.py:998`, `bin/disjuntor.py:1004`). |
| Linha do RELATO | Construída e validada em `relato_negacao`, delegada a `no_relato`; escrita física por temporário e `os.replace` (`bin/disjuntor.py:187`, `bin/disjuntor.py:194`, `bin/disjuntor.py:201`, `app/core/esteira_vigia.py:879`). | A trava lê `ln` antes da escrita; o escritor lê o RELATO anterior; o checador lê o documento completo. Stop executa esse checador e usa seu retorno para bloquear (`bin/disjuntor.py:194`, `app/core/esteira_vigia.py:860`, `bin/relato_afirma_com_prova.py:88`, `bin/hook_stop_fila1.py:1150`, `bin/hook_stop_fila1.py:1172`). |
| `d['b3_sem_obra']` apresentado por `linha()` | `_b3_sem_obra` calcula; `ler` atribui o resultado ao dicionário de apresentação (`bin/disjuntor.py:1058`, `bin/disjuntor.py:1052`). | `linha` formata o valor; `--linha`, o gerador do ESTADO e o teste 16 consomem esse fluxo (`bin/disjuntor.py:1122`, `bin/disjuntor.py:1130`, `bin/gerar_estado.py:281`, `bin/tests/test_disjuntor.sh:355`). |

A varredura que atualiza o estado é chamada pelo PreToolUse e pelo Stop. `ler` apenas conta o estado já acumulado; não descobre agentes novos (`bin/disjuntor.py:912`, `bin/hook_stop_fila1.py:1022`, `bin/disjuntor.py:1058`).

4. **Texto livre, delimitadores e alcance da trava**

**Texto escolhido pelo payload: sim, limitado. `|` e crase: não encontrei passagem para a linha construída.** `limpa` permite letras/dígitos ASCII, `_` e `-`, até 40 caracteres, nos campos agente, ferramenta e subtipo. Não valida esses campos contra um catálogo; portanto `agent_id="texto|livre\`aqui"` vira `textolivreaqui` (`bin/disjuntor.py:186`, `bin/disjuntor.py:189`).

**Uma linha efetivamente aceita pode casar `ATOS`.** Payload concreto:

```json
{"tool_name":"Bash","agent_id":"FEITO-NADA","tool_input":{"command":"touch DISJUNTOR_DESLIGADO"}}
```

A guarda nega pelo comando. Na linha gerada, `FEITO` casa `ATOS`, mas `NADA` casa `NEGA`, que dispensa **a linha inteira**, independentemente da relação entre as palavras. Assim, `RA.afirmacoes(ln)` aceita a linha (`bin/disjuntor.py:135`, `bin/disjuntor.py:189`, `bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:45`).

A cobertura exata é:

- Rejeita os padrões maiúsculos `APLICADO`, `APLIQUEI`, `APLICAMOS`, `NO AR`, `DEPLOYADO`, `DEPLOIADO` e `FEITO`, com fronteiras de palavra, em título ou linha contendo `**`, sem negação/prova. A linha do disjuntor sempre contém `**` (`bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:42`, `bin/disjuntor.py:187`).
- Não detecta outras caixas, como `feito`, palavras fora dessa lista ou ocorrências sem fronteira de palavra; também aceita a linha inteira se encontrar uma das negações previstas (`bin/relato_afirma_com_prova.py:30`, `bin/relato_afirma_com_prova.py:43`, `bin/relato_afirma_com_prova.py:45`).
- A função geral aceita `PROVA:` na própria linha ou nas seis seguintes. Na chamada nova há somente `ln`; não há vizinhas, e os campos limpos não permitem injetar `PROVA:` (`bin/relato_afirma_com_prova.py:47`, `bin/disjuntor.py:186`, `bin/disjuntor.py:194`).
- A trava nova não aplica a base histórica de exceções; essa filtragem pertence ao `main` do checador executado pelo Stop (`bin/disjuntor.py:194`, `bin/relato_afirma_com_prova.py:94`).

Sem uma negação, o payload `tool_name="FEITO"` com `file_path="/r/DISJUNTOR_DESLIGADO"` é barrado na saída: não acrescenta linha e registra `FALHA relato_negacao`. A negativa da ferramenta já foi emitida (`bin/disjuntor.py:137`, `bin/disjuntor.py:195`, `bin/disjuntor.py:203`, `bin/disjuntor.py:947`).

5. **Força dos casos novos e dos mutantes**

`_passa` exige stdout vazio e nenhuma `FALHA` nova, mas não exige evidência de reconhecimento. Um parser retornando `[]` também satisfaz essa condição (`bin/tests/test_disjuntor.sh:91`, `bin/disjuntor.py:841`).

| Casos | Avaliação |
|---|---|
| B3 14–16 | **14 passa se aquele prompt não for reconhecido.** 15 exige a negativa B3; 16 exige numericamente o contador igual a 1 (`bin/tests/test_disjuntor.sh:349`, `bin/tests/test_disjuntor.sh:351`, `bin/tests/test_disjuntor.sh:356`). |
| Codex 24–32 | 24, 26–28 exigem negativas concretas. **25 passa também quando a chamada desaparece.** 29–32 verificam ausência de negativa; isoladamente, não distinguem exclusão correta de omissão do parser (`bin/tests/test_disjuntor.sh:487`, `bin/tests/test_disjuntor.sh:488`, `bin/tests/test_disjuntor.sh:492`). |
| Trilha 2–3 e 21–29 | 2–3 exigem linhas completas. 21, 23, 25 e 28 verificam ausência. Há contrapartes positivas em 22, 24, 26, 27 e 29, mas 25 aceita qualquer saída do Stop sem o nome do checador (`bin/tests/test_disjuntor.sh:542`, `bin/tests/test_disjuntor.sh:544`, `bin/tests/test_disjuntor.sh:553`, `bin/tests/test_disjuntor.sh:560`). |

**Há uma lacuna específica em C:** depois do comando com `NO AR/FEITO`, os casos 22–26 não exigem aumento do número de linhas no RELATO. Omitir esse registro depois de escrever `ALVO` satisfaria esse bloco: a negativa existe, o texto contaminante não aparece, o checador passa e `ALVO` existe. Isso não comprova a exigência de uma linha segura para aquela negativa (`bin/tests/test_disjuntor.sh:553`, `bin/tests/test_disjuntor.sh:557`).

Quanto aos mutantes:

- **`A-sem-continuacao`:** o caso 25 continua passando por ausência; quem o mata é 26, que exige ANEXO-ENXUTO. Logo, 25 sozinho não demonstra a continuação reconhecida (`bin/tests/test_disjuntor.sh:488`, `bin/tests/test_disjuntor.sh:489`, `bin/tests/test_disjuntor.sh:725`).
- **`C-alvo-cru`:** troca o campo ferramenta pelo alvo. Já cai no caso 2 porque desaparece `Agent general-purpose`; mistura quebra de formato e vazamento, sem isolar as verificações posteriores de privacidade/ATOS (`bin/tests/test_disjuntor.sh:666`, `bin/tests/test_disjuntor.sh:542`, `bin/disjuntor.py:189`).
- **`C-sem-trava-de-saida`:** tem verificação específica: ferramenta `FEITO`, nenhuma linha adicional e presença do `ValueError` esperado (`bin/tests/test_disjuntor.sh:560`, `bin/tests/test_disjuntor.sh:562`, `bin/tests/test_disjuntor.sh:667`).

Não encontrei por inspeção um mutante novo necessariamente sobrevivente; resultado integral dos mutantes **[não verificado]**. A suíte cria temporários, escreve cópias e altera arquivos nessas cópias, incompatível com a regra desta análise (`bin/tests/test_disjuntor.sh:37`, `bin/tests/test_disjuntor.sh:612`, `bin/tests/test_disjuntor.sh:621`).

6. **Exceções, efeitos colaterais e regressões**

**Há uma exceção nova não tratada em `--linha`, condicionada a estado com tipo inválido.** Um JSON válido como:

```json
{"b3_sem_obra":1}
```

passa pela validação de `ler_estado`, que só exige objeto. `_b3_sem_obra` executa `len(1)` e captura apenas `RuntimeError`; o `TypeError` sobe por `ler` e por `main --linha`. O escritor normal produz lista; existência desse estado em produção **[não verificado]** (`bin/disjuntor.py:264`, `bin/disjuntor.py:527`, `bin/disjuntor.py:1058`, `bin/disjuntor.py:1130`). O gerador do ESTADO captura esse erro e apresenta “indisponível” (`bin/gerar_estado.py:282`).

Os demais efeitos identificados:

- **Import e `sys.path`:** cada tentativa de relato acrescenta outra entrada no início de `sys.path`. Importar RA não executa seu `main` nem escreve a base histórica; essa escrita está dentro de `main`, protegido pelo teste de `__name__` (`bin/disjuntor.py:192`, `bin/relato_afirma_com_prova.py:100`, `bin/relato_afirma_com_prova.py:123`).
- **I/O adicional:** `ALVO` é escrito antes da validação. A trilha normaliza espaços e corta o detalhe inteiro em 300 caracteres; portanto não preserva necessariamente o alvo completo, embora hash e tamanho sejam calculados sobre ele (`bin/disjuntor.py:108`, `bin/disjuntor.py:184`, `bin/disjuntor.py:190`).
- **Persistência não garantida:** falha ao escrever `ALVO` é engolida; a linha pública ainda pode afirmar “na trilha privada” (`bin/disjuntor.py:110`, `bin/disjuntor.py:185`, `bin/disjuntor.py:190`).
- **“Privada” não impõe permissão própria:** a abertura pede modo `0644`, sujeito ao umask. Permissões efetivas de produção **[não verificado]** (`bin/disjuntor.py:99`, `bin/orcamento_contexto.py:74`).
- **Exceções do registro são contidas:** falhas comuns de hash/import/validação/escrita entram em `FALHA relato_negacao`; a negativa já emitida permanece (`bin/disjuntor.py:203`, `bin/disjuntor.py:947`).
- **Compatibilidade anterior:** além das regressões Bash do item 2, IDs puramente alfabéticos antes aceitos pelo regex fornecido, como `ABC`, agora não satisfazem `_OBRA_ID`. Uso desses IDs em produção **[não verificado]** (`bin/disjuntor.py:81`, `bin/disjuntor.py:473`; comparação anterior: `delta.diff`, hunk de `OBRA_RE`).

## 31/10 02:30 · sessao 54ab8bc9f7f1 · - · - · origem=Ronald (tmux)

Resultados conferidos por leitura e probes Python `-B` em memória; não executei o selo, que cria temporários e cópias (`bin/tests/test_disjuntor.sh:37-49`, `:612-621`).

1. **B foi ampliado, mas permanece incompleto.** Os prompts novos do selo são reconhecidos (`bin/tests/test_disjuntor.sh:345-351`), porém:
   - `Revisão\nFINAL da obra **OBRA-X**` e `FINAL revisão da obra OBRA-X` dão `_eh_final=False`: a expressão não atravessa quebra de linha e só admite `final` antes de `review`, não antes de `revisão` (`bin/disjuntor.py:88`, `:463-465`). `_veredito_e_obra` retorna `(None,None)` e esses agentes não entram nem em `b3_sem_obra` (`:495-497`, `:520-523`).
   - `LEITURA FINAL da obra obra-x` dá obra `None`: os padrões usam `re.I`, mas o `fullmatch` posterior não usa (`bin/disjuntor.py:84-85`, `:473-474`). Entra no contador sem obra, sem incrementar a obra pretendida (`:497`, `:520-523`).
   - `LEITURA FINAL da obra O261 (C)` produz `O261 (C)`; `LEITURA FINAL da obra **O261** (C)` produz `O261`. A cauda exige whitespace imediatamente depois do ID; markdown separa as chaves (`bin/disjuntor.py:83-85`, `:481-482`).
   - `obra ABC` era aceito pela expressão anterior fornecida no diff; agora exige hífen ou letras+algarismos (`bin/disjuntor.py:81`). Uso desses formatos em produção: [não verificado].

2. **Testes por ausência de sinal.** `_passa` exige stdout vazio e nenhuma `FALHA` nova; não exige comprovação de que a invocação foi reconhecida (`bin/tests/test_disjuntor.sh:91-94`).
   - B3 caso14 passa também se aquele prompt final não for reconhecido; caso15 fornece a contraparte positiva (`:349-351`). Caso16 exige numericamente um agente sem obra (`:353-356`).
   - Codex25 passa também com parser retornando `[]`; confirmei isso aplicando somente o mutante `A-sem-continuacao` em memória (`:488`, `:725`). O mesmo mutante também retorna `[]` para o comando26, mas **26 o mata** porque exige `ANEXO-ENXUTO` (`:489`). Codex29–32 também verificam autorização por ausência; 24,26–28 verificam negativas concretas (`:487-495`).
   - Trilha21,23,25,28 são verificações de ausência (`:544`, `:554`, `:556`, `:561`). Há contrapartes positivas: 2/3 exigem linhas com formato completo;22/27 exigem negativas;24 executa o checador;26 exige ALVO;29 exige a falha específica da trava (`:542-562`). Particularmente25 aceita qualquer saída do Stop sem o nome do checador, inclusive vazio por outra falha (`:556`).

3. **Mutantes novos.**
   - `C-alvo-cru` troca a ferramenta pelo alvo, alterando simultaneamente formato e privacidade (`bin/tests/test_disjuntor.sh:666`; `bin/disjuntor.py:189`). Já cai no caso2, que exige `Agent general-purpose`; não isola as verificações de vazamento/ATOS posteriores (`bin/tests/test_disjuntor.sh:542`, `:544`, `:553-557`).
   - `C-sem-trava-de-saida` tem isolamento melhor: ferramenta `FEITO` deve manter a negativa CHAVE, não acrescentar linha e produzir o `ValueError` específico (`bin/tests/test_disjuntor.sh:560-562`, `:667`).
   - Não encontrei, por inspeção, mutante novo necessariamente sobrevivente; execução integral dos mutantes: [não verificado].

4. **Escritores/leitores B3.**
   - `b3_sem_obra`: produtor lógico `_veredito_e_obra`, que emite `''` (`bin/disjuntor.py:497`); coleta em `varrer_b3` (`:520-521`); escritor da lista `_fn` (`:527-530`); persistência `_com_estado` (`:237-243`); leitura e contagem `_b3_sem_obra` (`:1056-1060`); produtor do campo de apresentação `ler` (`:1052`); leitor final `linha` (`:1122`).
   - `b3`: criação/escrita de reprovados/disparado em `varrer_b3` (`bin/disjuntor.py:526`, `:532-546`); leitura e escrita de rearme em `rearma_b3` (`:553-559`); leitor `b3_disparados` (`:564-565`); consumidores da varredura PreToolUse (`:909-916`) e Stop (`bin/hook_stop_fila1.py:1022-1026`).
   - Chamadores que provocam escrita pela varredura: os mesmos PreToolUse e Stop; `ler` apenas conta o estado já acumulado (`bin/disjuntor.py:912`, `:1052-1058`; `bin/hook_stop_fila1.py:1022`).

5. **Exceção nova concreta.** Com estado lógico `{'b3_sem_obra':7}`, `_b3_sem_obra` levanta `TypeError`, pois executa `len(7)` e só captura `RuntimeError`; confirmei com `ler_estado` substituído em memória (`bin/disjuntor.py:1056-1060`). `--linha` não contém captura ao redor de `ler/linha` (`:1129-1131`). Strings e dicionários são contados silenciosamente por `len`, embora o escritor normal produza lista (`:527-530`, `:1058`). Estado desse tipo em produção: [não verificado].

## 31/10 02:30 · sessao ef602f7d040e · - · - · origem=Ronald (tmux)

Achados de C e perguntas 3/4/6, obtidos só por leitura e probes Python `-B` com escrita substituída por funções em memória:

- **C original, comando cru no RELATO:** não reproduzi. `tool_input.command` vira `alvo`, que entra apenas no hash/tamanho da linha; o texto vai a `trilha('ALVO', ...)` (`bin/disjuntor.py:178-190`). Com `{"tool_name":"Bash","tool_input":{"command":"echo deploy NO AR e FEITO; touch DISJUNTOR_DESLIGADO"}}`, a linha resultante contém `Bash -- alvo sha1:e02b244fde (52 car., na trilha privada)`, sem o comando; a própria negação decorre de `guarda_chave` (`bin/disjuntor.py:135-136`).

- **Ainda há texto escolhido pelo payload:** `limpa` aceita qualquer sequência de letras/dígitos ASCII, `_` e `-`, até 40 caracteres, nos campos `agent_id`, `tool_name` e `subagent_type`; não valida catálogo desses campos (`bin/disjuntor.py:186-190`). Exemplo: `agent_id="texto-livre|`XYZ`"` produz `agente texto-livreXYZ`. `|`, crase, estrelas e quebras de linha não passam pela whitelist nesses campos; alvo cru só gera digest hex e tamanho (`bin/disjuntor.py:184-190`).

- **Uma linha gravada pode casar `ATOS`:** payload concreto:
  ```json
  {"tool_name":"Bash","agent_id":"FEITO-NADA","tool_input":{"command":"touch DISJUNTOR_DESLIGADO"}}
  ```
  A negação ocorre por `bin/disjuntor.py:135-136`; a linha contém `agente FEITO-NADA`. `FEITO` casa `ATOS`, mas `NADA` casa `NEGA` e faz `RA.afirmacoes` ignorar a linha toda (`bin/relato_afirma_com_prova.py:30-49`). Probe confirmou gravação. Outra variante: `tool_name="FEITO", agent_id="NADA", file_path="/r/DISJUNTOR_DESLIGADO"`; o caminho dispara a guarda independentemente do nome da ferramenta (`bin/disjuntor.py:137-138`).

- **Cobertura real da trava:** a linha gerada sempre possui `**`; por isso é “forte”, mesmo quando o ato aparece fora do trecho em negrito. A trava rejeita os sete padrões maiúsculos de `ATOS`, com fronteira de palavra, salvo presença de negação em qualquer lugar da linha. Também aceita prosa sem `**`/título, outras caixas (`feito`), palavras fora de `ATOS`, formas embutidas em identificador (`SEM_EFEITO`), e linhas com `PROVA:` na própria linha ou nas seis seguintes (`bin/relato_afirma_com_prova.py:30-50`). Para a chamada nova `RA.afirmacoes(ln)`, só existe uma linha, sem vizinhas; `PROVA:` não é gerável pelos campos limpos (`bin/disjuntor.py:186-195`).

- **Payload `FEITO` sem negação:** `{"tool_name":"FEITO","tool_input":{"file_path":"/r/DISJUNTOR_DESLIGADO"}}` provoca `ValueError` na trava; não entra no RELATO, mas deixa `ALVO` e `FALHA relato_negacao`. Probe confirmou. A exceção é capturada e a negativa já foi emitida antes de tentar o relato (`bin/disjuntor.py:194-204`, `bin/disjuntor.py:947-950`).

**Escritores/leitores pedidos:**

| Valor | Escritor | Leitor |
|---|---|---|
| Evento `ALVO` | Preparado em `bin/disjuntor.py:185`; append físico em `bin/disjuntor.py:103-109` | Leitor explícito localizado: teste `bin/tests/test_disjuntor.sh:557`. `ler` lê o arquivo, mas filtra e descarta `ALVO`, pois só percorre os eventos de `bin/disjuntor.py:998-1005`; consumidor funcional de `ALVO` nos cinco arquivos: não encontrei. |
| Linha do RELATO | Construída em `bin/disjuntor.py:187-190`, validada em `:194`, delegada em `:199-201`; escritor físico `app/core/esteira_vigia.py:859-884` | RA lê o arquivo em `bin/relato_afirma_com_prova.py:86-96`; Stop invoca o checador em `bin/hook_stop_fila1.py:1148-1155`, bloqueia por retorno não limpo em `:1172-1177`; testes em `bin/tests/test_disjuntor.sh:542-562`. O escritor também lê o RELATO anterior em `app/core/esteira_vigia.py:860` e `:104-107`. |
| `d['b3_sem_obra']` de `linha` | `_b3_sem_obra` calcula em `bin/disjuntor.py:1056-1060`; `ler` atribui no dicionário em `:1052` | `linha` formata em `bin/disjuntor.py:1122`; chamada de apresentação `:1130`; teste `bin/tests/test_disjuntor.sh:355`. |

**Efeitos colaterais e exceções:**

- Novo `sys.path.insert(0, ...)` permanece no processo e acrescenta mais uma entrada a cada `relato_negacao`; probe confirmou crescimento de um elemento por chamada (`bin/disjuntor.py:192-193`). O import de RA não chama `main` nem escreve a base: `main` está protegido por `if __name__ == '__main__'` (`bin/relato_afirma_com_prova.py:123-124`); a escrita da base ocorre só dentro de `main`, em `:100-110`. Eventual criação de bytecode pelo interpretador em execução normal: [não verificado].
- `ALVO` acrescenta uma escrita para cada negativa antes da validação do RELATO (`bin/disjuntor.py:185-195`). A trilha normaliza espaços e limita **todo o detalhe a 300 caracteres**, logo não guarda o alvo inteiro apesar de o SHA1 e o tamanho serem calculados sobre ele (`bin/disjuntor.py:108-109`, `:184-190`).
- A designação “privada” não impõe proteção própria: a trilha usa `_abre` (`bin/disjuntor.py:107`), que delega a `orcamento_contexto.abre` (`:97-100`); esta pede modo `0o644` na criação (`bin/orcamento_contexto.py:73-74`). Permissões/umask efetivos de produção: [não verificado].
- Falha de `trilha('ALVO')` é engolida e não impede a linha de dizer “na trilha privada” (`bin/disjuntor.py:103-111`, `:185-190`). Não há confirmação de persistência nem referência garantida ao texto.
- **Exceção nova não tratada em `--linha`:** estado JSON válido `{"b3_sem_obra":1}` passa pela validação que só exige dicionário (`bin/disjuntor.py:264-266`), mas produz `TypeError: object of type 'int' has no len()` em `_b3_sem_obra`; só `RuntimeError` é capturado (`:1056-1060`). O erro sobe por `ler` e por `main --linha`, que não contém `try` (`:1052`, `:1129-1131`). Probe em memória confirmou.
- As exceções comuns introduzidas no caminho de `relato_negacao` são contidas pelo `except Exception`, gerando falha de registro; a negação da ferramenta continua emitida (`bin/disjuntor.py:203-204`, `:947-952`).

## 31/10 02:30 · sessao 3c700e287ce2 · - · - · origem=Ronald (tmux)

- **A original foi corrigido:** `cd /x && (CODEX_OBRA=X CODEX_PAPEL=verificador bin/codex_pergunta.sh --anexo d.diff h q r)` retorna uma chamada em `_codex_cmds`; parênteses agora separam trechos e o selo exige negativa por orçamento nesse exemplo. `bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/disjuntor.py:817`, `bin/tests/test_disjuntor.sh:487`.

- **Regressão de reconhecimento, com redirecionamento colado:**
  ```bash
  bin/codex_pergunta.sh</dev/null --anexo app/docs/LEIS.md h q r
  ```
  Bash invoca o wrapper com cinco argumentos; `_codex_cmds` retorna `[]`, porque depois de `.sh` agora exige espaço ou fim. Assim não aplica ANEXO-ENXUTO nem B4-CODEX. A regex antiga fornecida aceitava o limite de palavra antes de `<`. `bin/disjuntor.py:751`, `bin/disjuntor.py:841`, `bin/disjuntor.py:845`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`; regex antiga: `delta.diff` do usuário [não verificado em arquivo local].

- **Outra regressão causada pelo novo separador:**
  ```bash
  cat <<<EOF
  bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r
  ```
  Bash executa a chamada após o `cat`; `_codex_cmds` retorna `[]`. O primeiro `<` de `<<<` não entra no ramo heredoc, mas a iteração seguinte reconhece `<<EOF` dentro do here-string; como não encontra terminador, salta ao fim e descarta a chamada real. `bin/disjuntor.py:787`, `bin/disjuntor.py:788`, `bin/disjuntor.py:790`, `bin/disjuntor.py:797`, `bin/disjuntor.py:799`, `bin/disjuntor.py:800`.

- **Falso positivo novo — só define uma função:**
  ```bash
  f() { bin/codex_pergunta.sh --anexo app/docs/LEIS.md h q r; }
  ```
  `_codex_cmds` retorna uma chamada com `volta=meio` e o anexo; `codex_decide` nega com ANEXO-ENXUTO, embora Bash apenas defina `f`. Sem `--anexo`, orçamento excedido produz B4-CODEX. O parser divide `()` e elimina `{`, perdendo o contexto de definição. `bin/disjuntor.py:753`, `bin/disjuntor.py:754`, `bin/disjuntor.py:794`, `bin/disjuntor.py:817`, `bin/disjuntor.py:823`, `bin/disjuntor.py:849`, `bin/disjuntor.py:869`.

- **Falso positivo de heredoc:** em `cat <<EOF`, uma linha contendo tabulação + `EOF` ou `EOF ` não encerra o heredoc de Bash, mas o scanner a aceita como terminador. Uma linha posterior do corpo que mencione o wrapper passa a ser interpretada como chamada. `bin/disjuntor.py:799`.

Os exemplos foram conferidos com importação `python3 -B`, orçamento substituído em memória e uma função Bash também em memória no lugar do wrapper; nenhum arquivo foi criado. A suíte completa não foi executada [não verificado]. [nao verificado]

## 31/10 02:29 · sessao 273b3485f29d · O261-E-F5C · verificador · origem=Code

**Bloqueia:** a nova `fila` ainda pode nomear chamado oculto no painel padrão; as frases negam ações que o juiz não exclui; e o drawer mantém “chamado N” literal. (`app/chamados/services/painel.py:188`, `:329`; `app/folha/fora_da_folha.py:36`, `:43`, `:200`; `app/folha/retrato_folha.py:1069`.)

`git show 366e07d4` falhou: a cópia não contém repositório Git. Portanto, diff exato, conteúdo de `dc3ba57f`, RED executado e sequência dos commits: **[nao verificado]**. Os achados abaixo são do código disponível; ocorrência e quantidade em produção: **[nao verificado]**.

1. **Quem escreve e quem lê**

| Valor | Escritor/origem | Leitores |
|---|---|---|
| `porta['fila']` | `alimentar_portas` monta `fila_cel` usando `qs_na_fila`; o juiz seleciona a célula e escreve a tupla ordenada. (`app/chamados/juizes.py:1144`, `:1148`, `:1216`, `:1224`.) | `o_que_segura` lê e interpola os números na frase. (`app/folha/fora_da_folha.py:199`, `:200`, `:202`.) |
| `GRUPOS['C']` | Literal do módulo. (`app/folha/fora_da_folha.py:36`.) | `retrato_folha.ajuda` lê a prosa pelo mapeamento fio→letra; vira `title` e chega ao template da barra. (`app/folha/retrato_folha.py:71`, `:177`, `:196`, `:825`; `app/templates/core/_barra_gestao.html:67`; inclusão em `app/templates/folha/_retrato_barra.html:122`.) `fora_da_folha` também percorre `GRUPOS`, mas somente suas chaves. (`app/folha/fora_da_folha.py:139`.) |
| `DESTRAVA['chamado']` | Literal do módulo. (`app/folha/fora_da_folha.py:43`.) | `o_que_segura` lê e personaliza; `retrato_folha.o_que_fazer` lê literalmente pelo fio gravado. (`app/folha/fora_da_folha.py:197`, `:200`; `app/folha/retrato_folha.py:1058`, `:1069`.) A view fornece o bloco e o template imprime `r.valor`. (`app/colaboradores/views.py:984`; `app/templates/folha/_retrato_o_que_fazer.html:33`, `:37`.) |

Os testes consumidores são: [nao verificado]

- **Dict inteiro do juiz:** expectativas com `fila` nas duas rotas, e igualdade singular/lote. (`app/chamados/tests/test_o261e_f5b_juiz_porta.py:138`, `:145`, `:163`.)
- **Teste novo:** injeta `fila` por mock e verifica a frase personalizada. (`app/folha/tests/test_haiku_fora_da_folha.py:125`, `:127`, `:133`, `:136`, `:139`.)
- **Prosa de `GRUPOS`:** ausência de substring no teste novo; propagação para tooltips; comparação das chaves. (`app/folha/tests/test_haiku_fora_da_folha.py:122`; `app/folha/tests/test_retrato_folha_quadros.py:154`, `:1276`, `:1285`.)
- **`DESTRAVA` literal:** igualdade do drawer, inclusive `chamado`, e restrição aos valores dos mapas. (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:114`, `:119`; `app/folha/tests/test_haiku_fora_da_folha.py:484`; `app/folha/tests/test_retrato_folha_drawer.py:224`, `:229`.) Os testes de `colab` também consomem o mapa. (`app/folha/tests/test_retrato_folha_drawer.py:187`; `app/colaboradores/tests/test_retrato_no_drawer.py:210`.)

O **vigia** consome datas da folha e `verbos`/`rota` do juiz, sem ler a nova `fila` ou essas frases nesses caminhos. (`app/chamados/services/vigia_fio.py:549`, `:550`, `:596`, `:598`, `:601`.) **Pautas** consome o fio por `_alimentar`/`_fio_do_dia`, também sem essas frases. (`app/ponto/services/pautas_dos_discordantes.py:60`, `:74`, `:76`.)

2. **As frases não são verdadeiras para todo `fio='chamado'` — bloqueia**

A garantia efetiva de `fio_pelos_verbos` é: nenhum valor do mapa é `cobrar` ou `validar`. Qualquer outro valor, inclusive `historico` e `None`, resulta em `chamado`. Isso não garante ausência de pergunta pendente nem de resposta aguardando validação. (`app/chamados/juizes.py:1170`, `:1177`, `:1181`.)

**Contraexemplo permitido pelo código, nas duas rotas:** chamado aberto com `silenciado_ate` vencido e pergunta viva pendente.

- `qs_na_fila` admite silêncio vencido: exclui somente `silenciado_ate > agora`. (`app/chamados/services/painel.py:186`, `:188`.)
- A partição substitui o verbo original por `historico` quando o silêncio venceu. Assim, pode encobrir tanto `cobrar` quanto `validar`. (`app/chamados/services/verbos.py:44`, `:45`, `:48`.)
- O app continua selecionando perguntas vivas, sem filtro de silêncio; pergunta viva sem resposta é `PENDENTE`. (`app/chamados/services/pergunta_no_app.py:39`, `:42`, `:44`; `app/chamados/models.py:2207`, `:2208`.)
- Com o chamado ligado à célula, o juiz devolve `porta_fila`; com somente a pergunta ligada à célula e a âncora fora dela, devolve `porta_pergunta`. Em ambos, `historico` permite `fio='chamado'`. (`app/chamados/juizes.py:1216`, `:1218`, `:1220`, `:1224`, `:1225`.)

Logo, “nenhuma pede resposta do COLAB nem validação” pode ser falsa nas duas rotas. (`app/folha/fora_da_folha.py:36`, `:43`; cadeia acima.)

**Também há chamado nomeado invisível no painel padrão.** Na `porta_fila`, esse chamado histórico permanece em `fila`, recebe número na frase, mas o painel padrão exclui IDs com verbo `historico`. A chave nova separa chamados da célula de âncoras externas; não prova visibilidade. (`app/chamados/juizes.py:1224`; `app/folha/fora_da_folha.py:199`, `:201`; `app/chamados/services/painel.py:322`, `:327`, `:329`.)

3. **Drawer versus `o_que_segura`: incoerente — bloqueia**

`o_que_segura` troca o trecho genérico por números reais de `fila` ou somente pela pergunta no app. O drawer entrega o literal completo, incluindo **“chamado N na fila, ou…”**. (`app/folha/fora_da_folha.py:200`, `:202`; `app/folha/retrato_folha.py:1069`; `app/templates/folha/_retrato_o_que_fazer.html:37`.)

O retrato grava o fio em `dono`, mas sua linha não carrega `fila` nem `rota` para realizar essa escolha. (`app/folha/retrato_folha.py:1177`, `:1186`, `:1188`.) Além disso, o contrato já declara “N literal na tela” um bug de rótulo. (`app/docs/CORTES.md:353`.)

Os testes atuais preservam essa incoerência ao exigir igualdade com o literal, inclusive para `chamado`. (`app/folha/tests/test_retrato_folha_dono_sao_tres.py:119`; `app/folha/tests/test_haiku_fora_da_folha.py:484`.)

4. **Checklist das classes**

| Classe | Veredito |
|---|---|
| Segundo juiz duplicando registrado | **Sem bloqueio identificado nos blocos examinados:** porta registrada, escada no juiz e folha lendo seu fio. (`app/core/juizes.py:253`; `app/chamados/juizes.py:1175`; `app/folha/fora_da_folha.py:68`.) |
| Segundo escritor fora da porta | **Sem bloqueio identificado nesses blocos:** montam mapas, texto e retorno, sem escrita persistente. (`app/chamados/juizes.py:1215`, `:1224`; `app/folha/fora_da_folha.py:191`, `:207`.) |
| Relógio fora da borda | Há leituras transitivas em `qs_na_fila` e `particionar_verbos`; introdução ou alteração pelo commit **[nao verificado]**. (`app/chamados/services/painel.py:188`; `app/chamados/services/verbos.py:32`.) |
| Derivação paralela de fonte única | A alimentação consulta as fontes existentes; o defeito comprovado é tratar pertencimento a `qs_na_fila` como garantia de exibição. **Bloqueia pela conclusão indevida**, descrita no item 2. (`app/chamados/juizes.py:1140`, `:1153`; `app/chamados/services/painel.py:329`.) |
| Selo/RED sem mordida | O teste novo mocka o juiz e verifica apenas `GRUPOS['C']` e a saída de `o_que_segura`; deixa passar o drawer e não exercita silêncio vencido. **Insuficiente como selo dessas garantias.** (`app/folha/tests/test_haiku_fora_da_folha.py:121`, `:125`, `:131`, `:139`.) |
| Mordida sobre `dc3ba57f` | **[nao verificado]**: não há árvore anterior nem resultado de execução disponível. O nome “MORDE” e as asserções não evidenciam o RED exigido. (`app/folha/tests/test_haiku_fora_da_folha.py:114`; `CLAUDE.md:28`.) |
| Ordem de trava / pouso antes do pré-requisito | Corte de nascimento e teste de `registrado` fora da fila existem. (`app/docs/CORTES.md:400`; `app/chamados/tests/test_o261e_f5b_juiz_porta.py:177`, `:184`.) Cumprimento temporal, suíte verde e pouso: **[nao verificado]**; suíte antes do opus é requisito explícito. (`CLAUDE.md:600`.) |

## 31/10 02:28 · sessao 41c42f95852f · - · - · origem=Ronald (tmux)

- **RED sobre `dc3ba57f`: [nao verificado].** O teste contém casos com juiz inteiramente mockado, não uma execução da árvore anterior: `app/folha/tests/test_haiku_fora_da_folha.py:125-139`. A obrigação exige vermelho evidenciado: `CLAUDE.md:28-29`, `CLAUDE.md:348-353`. **Bloqueia certificação do RED**, sem demonstrar sozinho defeito da cura.

- **Cobertura incompleta do novo teste:** verifica `GRUPOS['C']` por substring e a saída personalizada de `o_que_segura`; não verifica `DESTRAVA['chamado']` literal, drawer ou produção real da chave `fila`: `app/folha/tests/test_haiku_fora_da_folha.py:121-139`. Pode passar deixando o drawer incoerente. **Bloqueia se usado como selo dessa coerência.**

- **Juiz duplicado/fonte paralela:** não encontrei nos blocos examinados. O juiz está registrado em `app/core/juizes.py:253`; a escada continua em `app/chamados/juizes.py:1170-1181`; folha lê o fio em `app/folha/fora_da_folha.py:60-68` e a fila em `app/folha/fora_da_folha.py:195-202`. Alimentação consulta a fonte existente em `app/chamados/juizes.py:1140-1154`. **Sem bloqueio identificado nesses blocos.**

- **Segundo escritor:** os blocos de apresentação apenas montam retorno e texto: `app/folha/fora_da_folha.py:191-207`; o juiz monta retorno em `app/chamados/juizes.py:1215-1225`. **Sem escrita persistente identificada nesses blocos.**

- **Relógio:** a dependência `particionar_verbos` lê `timezone.now()` e deriva histórico: `app/chamados/services/verbos.py:32-48`. Sua introdução pelo diff é **[nao verificado]**. **Não atribuir como regressão sem histórico.**

- **Pré-requisito `registrado` fora da fila:** existe teste explícito consultando `COBRANCA` e `qs_na_fila`: `app/chamados/tests/test_o261e_f5b_juiz_porta.py:177-184`. Resultado executado **[nao verificado]**.

- **Ordem de trava/pouso:** nascimento autorizado consta em `app/docs/CORTES.md:400`; suite verde antes do opus é exigida em `CLAUDE.md:600-602`. Execução de RED → cura → GREEN → suite e cumprimento do pouso **[nao verificado]**.

## 31/10 02:28 · sessao 145bbd00c71a · - · - · origem=Ronald (tmux)

Bloqueia: `fila` nova continua podendo nomear chamado invisível ao admin.

- `qs_na_fila` aceita chamado aberto/em análise com `silenciado_ate` passado: `app/chamados/services/painel.py:186`.
- A partição muda qualquer verbo não nulo desse chamado para `historico`: `app/chamados/services/verbos.py:43`.
- `fio_pelos_verbos` devolve `chamado` para `historico`: `app/chamados/juizes.py:1175`.
- `alimentar_portas` inclui esse chamado em `fila_cel`: `app/chamados/juizes.py:1144`.
- O juiz devolve seu número em `fila`: `app/chamados/juizes.py:1224`.
- `o_que_segura` escreve “chamado N na fila”: `app/folha/fora_da_folha.py:199`.
- O painel padrão exclui justamente `historico`: `app/chamados/services/painel.py:322`.

Também bloqueia a afirmação universal das frases: sob uma pergunta viva e pendente, cujo chamado âncora tem silêncio vencido, a rota pode ser `porta_pergunta`, verbo `historico` e fio `chamado`; o app continua mostrando a pergunta, pois não filtra silêncio nem verbo histórico. Logo “nenhuma pede resposta do COLAB” não decorre da garantia do juiz. A pergunta viva sem `respondida_em` é pendente. Fontes: `app/chamados/juizes.py:1216`, `app/chamados/services/pergunta_no_app.py:39`, `app/chamados/models.py:2207`, `app/folha/fora_da_folha.py:36`, `app/folha/fora_da_folha.py:43`.

Outro caso que excede a garantia é chamado de módulo de auditoria gravado aberto: o queryset aceita pelo status, mas a natureza produz verbo `None`; o fio também será `chamado`, embora “decidir” não seja o verbo desse chamado. Fontes: `app/chamados/services/painel.py:187`, `app/chamados/catalogo/natureza.py:18`, `app/chamados/catalogo/natureza.py:44`, `app/chamados/catalogo/natureza.py:63`, `app/chamados/juizes.py:1181`.

Censo da chave `fila`:

| Papel | Local |
|---|---|
| Escritor de produção | `app/chamados/juizes.py:1224` |
| Origem do valor | `app/chamados/juizes.py:1216`, alimentada em `app/chamados/juizes.py:1146` |
| Leitor de produção | `app/folha/fora_da_folha.py:199`; interpolação em `app/folha/fora_da_folha.py:200` |
| Teste compara dict inteiro, porta_fila | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:138` |
| Teste compara dict inteiro, porta_pergunta | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:145` |
| Teste compara retorno inteiro singular/lote | `app/chamados/tests/test_o261e_f5b_juiz_porta.py:163` |
| Teste novo injeta `fila` falsa | `app/folha/tests/test_haiku_fora_da_folha.py:125` |
| Teste novo verifica nomes | `app/folha/tests/test_haiku_fora_da_folha.py:136` |
| Vigia lê somente `verbos`/`rota` | `app/chamados/services/vigia_fio.py:596` |

As combinações acima são contraexemplos permitidos pelo código; existência delas em produção [nao verificado]. O caminho citado nas docstrings como `chamados/painel.py` corresponde na cópia ao serviço `app/chamados/services/painel.py:196`.

_861 resposta(s) mais antiga(s) fora deste arquivo (teto de 20)._
